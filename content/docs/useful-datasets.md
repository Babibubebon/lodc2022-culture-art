---
title: "SPARQLエンドポイントリスト"
weight: 5
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

# SPARQLエンドポイントリスト

- [ジャパンサーチ](https://jpsearch.go.jp/)
    - SPARQLエンドポイント: https://jpsearch.go.jp/rdf/sparql/
- [Cultural Japan](https://cultural.jp/)
    - SPARQLエンドポイント: http://ld.cultural.jp/sparql/
- [国立公文書館デジタルアーカイブ](https://www.digital.archives.go.jp/)
    - SPARQLエンドポイント: https://www.digital.archives.go.jp/sparql
- [ヨコハマ・アート・ナビ](https://artnavi.yokohama/)
    - SPARQLエンドポイント: http://data.yafjp.org/sparql
- [Europeana](https://www.europeana.eu/)
    - SPARQLエンドポイント: http://sparql.europeana.eu/
- [RCGSコレクション](https://collection.rcgs.jp/)
    - SPARQLエンドポイント: https://linkeddata.rcgs.jp
- [バーチャルYouTuber LOD](https://mdlab.slis.tsukuba.ac.jp/lodc2018/vtuber/)
    - SPARQLエンドポイント: http://mdlab.slis.tsukuba.ac.jp/lodc2018/vtuber/query
    - [LODチャレンジ2018 テーマ賞 カルチャーＬＯＤ賞](https://2019.lodc.jp/archives/2018/awardCeremony.html)
- [im@sparql](https://sparql.crssnky.xyz/imas/): アイドルマスター（アイマス）
    - SPARQLエンドポイント: https://sparql.crssnky.xyz/spql/imas/query
    - [LODチャレンジ2018 データセット部門 優秀賞](https://2019.lodc.jp/archives/2018/awardCeremony.html)
- [PrismDB](https://prismdb.takanakahiko.me/): プリティーシリーズ
    - SPARQLエンドポイント: https://prismdb.takanakahiko.me/sparql
- [Lemonade](https://lemonade.assaultlily.com/): アサルトリリィ
    - SPARQLエンドポイント: https://luciadb.assaultlily.com/sparql/query
    - [LODチャレンジ2021 テーマ賞 カルチャーLOD賞](https://2021.lodc.jp/awardSymposium2021Report.html)
    - LuciaDB: https://github.com/Assault-Lily/LuciaDB
    - 2022.07.24 アサルトリリィ関連情報を取り扱うファンサイト「Lemonade」が公認化！  
      https://www.assaultlily.com/news/1405.html/

-----

{{< yasgui id="datasets" />}}

-----

## クエリ例
### ジャパンサーチ: 「手塚治虫」の著作を取得する {#jpsearch-tezuka-osamu}
`https://jpsearch.go.jp/rdf/sparql/`
{{< yasgui-query yasgui-id="datasets" title="「手塚治虫」の著作を取得する" endpoint="https://jpsearch.go.jp/rdf/sparql/" >}}
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs:   <http://www.w3.org/2000/01/rdf-schema#>
PREFIX schema: <http://schema.org/>
PREFIX jps: <https://jpsearch.go.jp/term/property#>

SELECT ?s ?label ?datePublished ?providerLabel
WHERE {
  ?s schema:creator/owl:sameAs*
       <http://id.ndl.go.jp/auth/entity/00083890> ;
       #<https://mediaarts-db.bunka.go.jp/id/C48012> ;
       #<http://ja.dbpedia.org/resource/手塚治虫> ;
     rdfs:label ?label ;
     schema:datePublished ?datePublished ;
     jps:sourceInfo [
        schema:provider/rdfs:label ?providerLabel ;
     ] .
}
{{< / yasgui-query >}}


### ジャパンサーチ: パブリックドメイン(CC0)な作品の画像を取得する {#jpsearch-public-domain-image}
`https://jpsearch.go.jp/rdf/sparql/`
{{< yasgui-query yasgui-id="datasets" title="パブリックドメイン(CC0)な作品の画像を取得する" endpoint="https://jpsearch.go.jp/rdf/sparql/" >}}
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs:   <http://www.w3.org/2000/01/rdf-schema#>
PREFIX schema: <http://schema.org/>
PREFIX jps: <https://jpsearch.go.jp/term/property#>

SELECT ?class ?label ?creatorLabel ?media WHERE {
  ?s a ?class ;
     schema:creator/rdfs:label ?creatorLabel ; 
     rdfs:label ?label ;
     jps:accessInfo [
       schema:license <http://creativecommons.org/publicdomain/zero/1.0/> ;
       schema:associatedMedia ?media ;
     ] .
}
{{< / yasgui-query >}}


### LuciaDB: レギオンとその所属リリィの数を集計する {#luciadb-legion-list}
`https://luciadb.assaultlily.com/sparql/query`
{{< yasgui-query yasgui-id="datasets" title="レギオンとその所属リリィの数を集計する" endpoint="https://luciadb.assaultlily.com/sparql/query" >}}
PREFIX lilyrdf: <https://luciadb.assaultlily.com/rdf/RDFs/detail/>
PREFIX lily: <https://luciadb.assaultlily.com/rdf/IRIs/lily_schema.ttl#>
PREFIX schema: <http://schema.org/>

SELECT
    ?s ?name (COUNT(DISTINCT ?member) AS ?members)
WHERE {
  ?s a lily:Legion ;
     schema:name ?name ;
     schema:member ?member .
  FILTER (LANG(?name) = "ja")
}
GROUP BY ?s ?name
{{< / yasgui-query >}}