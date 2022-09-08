---
title: "複数エンドポイントの横断的活用"
weight: 4
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

# 複数エンドポイントの横断的活用

SPARQLでは、1つのエンドポイントだけでなく、外部の複数のエンドポイントに対してクエリを実行することができる[**federatedクエリ**](https://www.w3.org/TR/2013/REC-sparql11-federated-query-20130321/)という仕組みがあります。

federatedクエリを利用したクエリを紹介します。

## SPARQLクエリエディタ {#query-editor}

{{< yasgui id="federated" />}}

-----

## クエリ集 {#queries}

### 作品の多言語のタイトルを取得する {#multilingual-titles}
https://mediaarts-db.bunka.go.jp/id/C413599

[P7886(メディア芸術データベース識別子)](https://www.wikidata.org/wiki/Property:P7886)

{{< yasgui-query yasgui-id="federated" title="作品の多言語のタイトルを取得する" endpoint="https://mediag.bunka.go.jp/sparql" >}}
PREFIX rdfs:   <http://www.w3.org/2000/01/rdf-schema#>
PREFIX schema: <https://schema.org/>
PREFIX ma:     <https://mediaarts-db.bunka.go.jp/data/property#>
PREFIX wdt:    <http://www.wikidata.org/prop/direct/>

SELECT
  ?MADBLabel ?label (LANG(?label) AS ?lang) ?wikidataEntity
WHERE {
  <https://mediaarts-db.bunka.go.jp/id/C413599>
    schema:identifier ?MADBID ;
    rdfs:label ?MADBLabel .
  # Wikidataへクエリ
  SERVICE <https://query.wikidata.org/sparql> {
    # P7886: メディア芸術データベース識別子
    ?wikidataEntity wdt:P7886 ?MADBID ;
                    rdfs:label ?label .
  }
}
{{< / yasgui-query >}}


### メディア芸術データベースの責任主体の法人番号を取得する {#agent-houjin-bangou}
Wikidataとメディア芸術データベースを連携したfederatedクエリ

法人番号からさらに[gBizINFOのSPARQLエンドポイント](https://info.gbiz.go.jp/hojin/SparqlQueryEditor)などとも繋げられそうですね。

{{< yasgui-query yasgui-id="federated" title="メディア芸術データベースの責任主体の法人番号を取得する" endpoint="https://mediag.bunka.go.jp/sparql" >}}
PREFIX schema: <https://schema.org/>
PREFIX class:  <https://mediaarts-db.bunka.go.jp/data/class#>
PREFIX ma:     <https://mediaarts-db.bunka.go.jp/data/property#>
PREFIX wdt:    <http://www.wikidata.org/prop/direct/>

SELECT
  ?agent ?name ?hojinBangou
WHERE {
  ?agent a class:Agent;
      schema:name ?name ;
      ma:wikidata ?wikidataPage .
  # WikidataのリソースURIに変換
  BIND (URI(REPLACE(?wikidataPage, "https://www.wikidata.org/wiki/", "http://www.wikidata.org/entity/")) AS ?wikidataEntity)

  # Wikidata
  SERVICE <https://query.wikidata.org/sparql> {
    # P3225: 法人番号
    ?wikidataEntity wdt:P3225 ?hojinBangou ;
  }
}
LIMIT 100
{{< / yasgui-query >}}


### 「日本ゲーム大賞」を受賞したゲームを取得する {#japan-game-awards}
DBpedia JapaneseとWikidataとメディア芸術データベースを連携したfederatedクエリ

{{< yasgui-query yasgui-id="federated" title="「日本ゲーム大賞」を受賞したゲームを取得する" endpoint="https://mediag.bunka.go.jp/sparql" >}}
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX schema: <https://schema.org/>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX class:  <https://mediaarts-db.bunka.go.jp/data/class#>
PREFIX ma:     <https://mediaarts-db.bunka.go.jp/data/property#>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX hint: <http://aws.amazon.com/neptune/vocab/v01/QueryHints#>

SELECT
  ?MADBID ?genre ?label
WHERE {
  hint:Query hint:joinOrder "Ordered" .
  # DBpedia Japanese
  SERVICE <https://ja.dbpedia.org/sparql> {
    SELECT DISTINCT
      (URI(REPLACE(STR(?wikidataEntity), "http://wikidata.dbpedia.org/resource/", "http://www.wikidata.org/entity/"))
        AS ?wikidataEntity)
    {
      # 下位カテゴリを含める
      <http://ja.dbpedia.org/resource/Category:日本ゲーム大賞受賞ソフト> ^skos:broader* ?category .
      ?dbpediaEntity dcterms:subject ?category ;
                     ^owl:sameAs ?wikidataEntity .
      FILTER(STRSTARTS(STR(?wikidataEntity), "http://wikidata.dbpedia.org/resource/"))
    }
  }
  # Wikidata
  SERVICE <https://query.wikidata.org/sparql> {
    # P7886: メディア芸術データベース識別子
    ?wikidataEntity wdt:P7886 ?MADBID .
  }
  # メディア芸術データベース
  ?MADBResource schema:identifier ?MADBID ;
            schema:genre ?genre ;
            rdfs:label ?label .
}
LIMIT 100
{{< / yasgui-query >}}

{{< hint info >}}
federatedクエリの実行順序によっては、正しく結果が得られないことがあります。

RDFストアによっては、クエリオプティマイザに実行順序を指示する方法が用意されており、メディア芸術データベースのSPARQLクエリサービスが使用しているAmazon Neptuneでは `hint:Query hint:joinOrder "Ordered" .` というパターンを記述します。

参照: [Amazon Neptune: SPARQL クエリヒント](https://docs.aws.amazon.com/ja_jp/neptune/latest/userguide/sparql-query-hints.html)
{{< /hint >}}


### 「タイムトラベルを題材とした作品」を取得する
DBpedia JapaneseとWikidataとメディア芸術データベースを連携したfederatedクエリ

{{< yasgui-query yasgui-id="federated" title="「タイムトラベルを題材とした作品」を取得する" endpoint="https://mediag.bunka.go.jp/sparql" >}}
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX schema: <https://schema.org/>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX class:  <https://mediaarts-db.bunka.go.jp/data/class#>
PREFIX ma:     <https://mediaarts-db.bunka.go.jp/data/property#>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX hint: <http://aws.amazon.com/neptune/vocab/v01/QueryHints#>

SELECT
  ?MADBID ?genre ?label
WHERE {
  hint:Query hint:joinOrder "Ordered" .
  # DBpedia Japanese
  SERVICE <https://ja.dbpedia.org/sparql> {
    SELECT DISTINCT (URI(REPLACE(STR(?wikidataEntity), "http://wikidata.dbpedia.org/resource/", "http://www.wikidata.org/entity/"))
        AS ?wikidataEntity) {
      <http://ja.dbpedia.org/resource/Category:タイムトラベルを題材とした作品> ^skos:broader* ?category .
      ?dbpediaEntity dcterms:subject ?category;
                     ^owl:sameAs ?wikidataEntity .
      FILTER(STRSTARTS(STR(?wikidataEntity), "http://wikidata.dbpedia.org/resource/"))
    }
  }
  # Wikidata
  SERVICE <https://query.wikidata.org/sparql> {
    ?wikidataEntity wdt:P7886 ?MADBID .
  }
  # メディア芸術データベース
  ?MADBResource schema:identifier ?MADBID ;
            schema:genre ?genre ;
            rdfs:label ?label .
}
LIMIT 200
{{< / yasgui-query >}}

Wikipediaのカテゴリは上記のようなクエリで汎用的に使うことができます。

他にも以下のようなカテゴリを使うことで、いわゆる「聖地」による作品のキュレーションとして活用することができます。

- [`http://ja.dbpedia.org/resource/Category:湘南を舞台とした作品`](http://ja.dbpedia.org/resource/Category:湘南を舞台とした作品)
  - [Wikipedia: Category:湘南を舞台とした作品](https://ja.wikipedia.org/wiki/Category:%E6%B9%98%E5%8D%97%E3%82%92%E8%88%9E%E5%8F%B0%E3%81%A8%E3%81%97%E3%81%9F%E4%BD%9C%E5%93%81)
- [`http://ja.dbpedia.org/resource/Category:京都府を舞台とした作品`](http://ja.dbpedia.org/resource/Category:京都府を舞台とした作品)
  - [Wikipedia: 
Category:京都府を舞台とした作品](https://ja.wikipedia.org/wiki/Category:%E4%BA%AC%E9%83%BD%E5%BA%9C%E3%82%92%E8%88%9E%E5%8F%B0%E3%81%A8%E3%81%97%E3%81%9F%E4%BD%9C%E5%93%81)