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


## クエリエディタ
{{< yasgui id="madb-lod" endpoint="https://mediag.bunka.go.jp/sparql"
default-query=`PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX schema: <https://schema.org/>
PREFIX class:    <https://mediaarts-db.bunka.go.jp/data/class#>
PREFIX ma:       <https://mediaarts-db.bunka.go.jp/data/property#>
SELECT * WHERE {
  ?sub ?pred ?obj .
} LIMIT 10`
>}}

-----

### 公開年毎にアニメテレビレギュラーシリーズ数を集計する
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