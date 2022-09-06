---
title: "メディア芸術データベース"
weight: 2
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

# メディア芸術データベース(ベータ版)

https://mediaarts-db.bunka.go.jp/


## メディア芸術データベース・ラボ (MADB Lab)
https://mediag.bunka.go.jp/madb_lab/

- [SPARQLクエリサービス](https://mediag.bunka.go.jp/madb_lab/lod/sparql/)
- [データセット (Turtle, JSON-LD)](https://mediag.bunka.go.jp/madb_lab/lod/download/)
- [GitHubリポジトリ](https://github.com/mediaarts-db/dataset)
  - [スキーマ仕様書 Ver. 1.1](https://github.com/mediaarts-db/dataset/blob/397b40d4e7dd35096a8c835f55f6b2406ded2315/doc/MADB%E3%83%A1%E3%82%BF%E3%83%87%E3%83%BC%E3%82%BF%E3%82%B9%E3%82%AD%E3%83%BC%E3%83%9E%E4%BB%95%E6%A7%98%E6%9B%B8.pdf)
- 独自に定義する語彙
  - [クラス](https://mediaarts-db.bunka.go.jp/data/class)
  - [プロパティ](https://mediaarts-db.bunka.go.jp/data/property)


## SPARQLクエリエディタ {#query-editor}
{{< yasgui id="madb-lod" endpoint="https://mediag.bunka.go.jp/sparql" >}}
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX schema: <https://schema.org/>
PREFIX class:    <https://mediaarts-db.bunka.go.jp/data/class#>
PREFIX ma:       <https://mediaarts-db.bunka.go.jp/data/property#>
SELECT * WHERE {
  ?sub ?pred ?obj .
} LIMIT 10
{{< / yasgui >}}

-----

## クエリ集 {#queries}

### 全リソースを種別ごとに集計する {#aggregation-by-genre}
{{< yasgui-query yasgui-id="madb-lod" title="集計" >}}
PREFIX schema: <https://schema.org/>
PREFIX class:  <https://mediaarts-db.bunka.go.jp/data/class#>

SELECT
  ?additionalType ?class ?genre (COUNT(*) AS ?count)
WHERE {
  ?resource a ?class;
  		      schema:additionalType ?additionalType ;
            schema:genre ?genre .
}
GROUP BY ?class ?additionalType ?genre
ORDER BY ?additionalType
{{< / yasgui-query >}}

### マンガ単行本の一覧を取得する {#manga-book}
「マンガ単行本」を表すクラス `https://mediaarts-db.bunka.go.jp/data/class#MangaBook`

{{< yasgui-query yasgui-id="madb-lod" title="マンガ単行本の一覧を取得する" hl_lines="9" >}}
PREFIX rdfs:   <http://www.w3.org/2000/01/rdf-schema#>
PREFIX schema: <https://schema.org/>
PREFIX ma:     <https://mediaarts-db.bunka.go.jp/data/property#>
PREFIX class:  <https://mediaarts-db.bunka.go.jp/data/class#>

SELECT
  ?id ?label ?creator ?datePublished
WHERE {
  ?item a class:MangaBook ;
        schema:identifier ?id ;
        rdfs:label ?label ;
        schema:creator ?creator ;
        schema:datePublished ?datePublished .
}
LIMIT 1000
{{< / yasgui-query >}}

### マンガ単行本とその所蔵館の一覧を取得する {#manga-book-and-provider}
[`https://mediaarts-db.bunka.go.jp/id/M464950`](https://mediaarts-db.bunka.go.jp/id/M464950) を主語とするTurtle形式のRDFデータ

```turtle {hl_lines="23-36"}
<https://mediaarts-db.bunka.go.jp/id/M464950>
        a                      class:MangaBook ;
        rdfs:label             "鬼滅の刃 1" ;
        schema:identifier      "M464950" ;
        schema:additionalType  class:CM ;
        schema:genre           "マンガ単行本" ;
        dcterms:creator        <https://mediaarts-db.bunka.go.jp/id/C53400> ;
        dcterms:publisher      "P4080000000" ;
        ma:jpno                "22740403" ;
        ma:ndc                 "726.1" ;
        ma:note                "【言語】日本語　／　JPN" ;
        schema:alternateName   "残酷" ;
        schema:brand           "ジャンプコミックス" , "ジャンプ コミックス"@ja-Hrkt ;
        schema:creator         "[著]吾峠呼世晴" ;
        schema:datePublished   "2016-06-08" ;
        schema:description     "残酷" ;
        schema:inLanguage      "日本語" ;
        schema:isPartOf        <https://mediaarts-db.bunka.go.jp/id/C361806> ;
        schema:isbn            "9784088807232" ;
        schema:location        "東京" ;
        schema:name            "キメツ ノ ヤイバ"@ja-Hrkt , "鬼滅の刃" ;
        schema:position        "1.0" ;
        schema:provider        [ ma:materialIdentifier     <https://id.ndl.go.jp/bib/027321240> ;
                                 ma:note                   "2016-06" ;
                                 ma:ownerIdentifier        "2" ;
                                 ma:subMaterialIdentifier  "1" ;
                                 schema:name               "国立国会図書館" ;
                                 schema:price              "400円"
                               ] ;
        schema:provider        [ ma:materialIdentifier     "10071400016547" ;
                                 ma:note                   "1刷　／　付:帯・カバー　／　付:「ジャンパラ!」(JUMP PARADISE) Vol.156" ;
                                 ma:ownerIdentifier        "6" ;
                                 ma:subMaterialIdentifier  "1" ;
                                 schema:name               "大阪府立中央図書館国際児童文学館" ;
                                 schema:price              "400円"
                               ] ;
        schema:publisher       "集英社　∥　シュウエイシャ" ;
        schema:size            "18cm　／　18cm　×　12cm" ;
        schema:volumeNumber    "1" .
```

[空白ノードの構文](https://www.w3.org/TR/2013/REC-sparql11-query-20130321/#QSynBlankNodes)を使ってパターンを指定します。

{{< yasgui-query yasgui-id="madb-lod" title="マンガ単行本の一覧を取得する" hl_lines="14-16" >}}
PREFIX rdfs:   <http://www.w3.org/2000/01/rdf-schema#>
PREFIX schema: <https://schema.org/>
PREFIX ma:     <https://mediaarts-db.bunka.go.jp/data/property#>
PREFIX class:  <https://mediaarts-db.bunka.go.jp/data/class#>

SELECT
  ?id ?label ?creator ?datePublished ?providerName
WHERE {
  ?item a class:MangaBook ;
        schema:identifier ?id ;
        rdfs:label ?label ;
        schema:creator ?creator ;
        schema:datePublished ?datePublished ;
        schema:provider [
          schema:name ?providerName
        ] .
}
LIMIT 1000
{{< / yasgui-query >}}

参考: [MADB Lab: データ利活用例その２：マンガの連携機関所蔵リスト](https://mediag.bunka.go.jp/madb_lab/lod/usecase/case2/)

### 公開年毎にアニメテレビレギュラーシリーズ数を集計 {#anime-tv-series}
{{< yasgui-query yasgui-id="madb-lod" title="公開年毎にTVアニメシリーズ数を集計する" >}}
PREFIX schema: <https://schema.org/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX class: <https://mediaarts-db.bunka.go.jp/data/class#>
SELECT ?y (COUNT(DISTINCT *) AS ?cnt)  WHERE {
  ?s a class:AnimationTVRegularSeries ;
     schema:datePublished ?datePublished .
}
GROUP BY (SUBSTR(?datePublished, 1, 4) AS ?y)
ORDER BY ASC(?y)
{{< / yasgui-query >}}


### 登場キャラクター名がタイトルのアニメシリーズ {#anime-character-name-title}
{{< yasgui-query yasgui-id="madb-lod" title="公開年毎にTVアニメシリーズ数を集計する" >}}
PREFIX schema: <https://schema.org/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX class: <https://mediaarts-db.bunka.go.jp/data/class#>
SELECT
	?col ?genre ?colName ?actors
WHERE {
  ?col a ?animeColClasses ;
         schema:name ?colName ;
         schema:genre ?genre ;
         schema:actor ?actors .
  VALUES ?animeColClasses {class:AnimationTVRegularSeries class:AnimationTVSpecialSeries class:AnimationMovieSeries}
  FILTER(LANG(?colName) = "")
  FILTER(REGEX(?actors, CONCAT("【", ?colName ,"】")))
}
{{< / yasgui-query >}}

