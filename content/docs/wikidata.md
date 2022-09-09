---
title: "Wikidata"
weight: 3
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

# Wikidata

https://www.wikidata.org/

- [Wikidata Query Service](https://query.wikidata.org/)
- [ウィキデータ:SPARQLチュートリアル](https://www.wikidata.org/wiki/Wikidata:SPARQL_tutorial/ja)
- [Wikidata:SPARQLクエリサービス/クエリ/例](https://www.wikidata.org/wiki/Wikidata:SPARQL_query_service/queries/examples/ja)
- データモデル
    - [Wikibase/DataModel/Primer](https://www.mediawiki.org/wiki/Wikibase/DataModel/Primer)
    - [Wikibase/Indexing/RDF Dump Format](https://www.mediawiki.org/wiki/Wikibase/Indexing/RDF_Dump_Format)


## SPARQLクエリエディタ {#query-editor}

{{< yasgui id="wikidata" endpoint="https://query.wikidata.org/sparql" >}}
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>

SELECT * WHERE {
  ?sub ?pred ?obj .
} LIMIT 10
{{< / yasgui >}}


## 文化・芸術系に関係のあるクラス
クラスについて: [Help:基本構成プロパティ](https://www.wikidata.org/wiki/Help:Basic_membership_properties/ja)

- [Q838948 (芸術作品)](https://www.wikidata.org/entity/Q838948)
- [Q7725634 (文学作品)](https://www.wikidata.org/entity/Q7725634)
- [Q747381 (ライトノベル)](https://www.wikidata.org/entity/Q747381)
- [Q104213567 (ライトノベルシリーズ)](https://www.wikidata.org/entity/Q104213567)
- [Q8274 (日本の漫画)](https://www.wikidata.org/entity/Q8274)
- [Q21198342 (日本の連載漫画)](https://www.wikidata.org/entity/Q21198342)
- [Q21202185 (日本の読み切り漫画)](https://www.wikidata.org/entity/Q21198342)
- [Q4502142 (視覚芸術作品)](https://www.wikidata.org/entity/Q4502142)
- [Q11424 (映画)](https://www.wikidata.org/entity/Q11424)
- [Q1107 (日本のアニメ)](https://www.wikidata.org/entity/Q1107)
- [Q581714 (アニメシリーズ)](https://www.wikidata.org/entity/Q581714)
- [Q63952888 (テレビアニメシリーズ)](https://www.wikidata.org/entity/Q63952888)
- [Q11086742 (テレビアニメ)](https://www.wikidata.org/entity/Q11086742)
- [Q202866 (アニメーション映画)](https://www.wikidata.org/entity/Q202866)
- [Q220898 (OVA)](https://www.wikidata.org/entity/Q220898)
- [Q7889 (コンピュータゲーム)](https://www.wikidata.org/entity/Q7889)

...


### 「芸術作品」の下位クラスを探す {#work-of-art-classes}
{{< yasgui-query yasgui-id="wikidata" title="「芸術作品」の下位クラスを探す" >}}
PREFIX bd: <http://www.bigdata.com/rdf#>
PREFIX wikibase: <http://wikiba.se/ontology#>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>

SELECT
  ?entity ?entityLabel
WHERE {
  # "芸術作品" の下位クラス
  ?entity wdt:P279* wd:Q838948 .
  FILTER EXISTS { ?s wdt:P31 ?entity . }
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja,en". }
}
LIMIT 1000
{{< / yasgui-query >}}


### 「日本のアニメおよび漫画」の一部分・下位クラスを探す {#japanese-anime-manga-classes}
[Q10901350　(日本のアニメおよび漫画)](https://www.wikidata.org/wiki/Q10901350)

{{< yasgui-query yasgui-id="wikidata" title="「日本のアニメおよび漫画」の一部分・下位クラスを探す" >}}
PREFIX bd: <http://www.bigdata.com/rdf#>
PREFIX wikibase: <http://wikiba.se/ontology#>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>

SELECT
  ?entity ?entityLabel
WHERE {
  # "日本のアニメおよび漫画" の下位クラス
  # "日本のアニメおよび漫画" の一部分であるクラス
  # "日本のアニメおよび漫画" の一部分であるクラスの下位クラス
  ?entity wdt:P279*/wdt:P361* wd:Q10901350 .
  FILTER EXISTS { ?s wdt:P31 ?entity . }
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja,en". }
}
{{< / yasgui-query >}}


## 文化・芸術系に関係のあるプロパティ
- [Wikidata:List of properties/art](https://www.wikidata.org/wiki/Wikidata:List_of_properties/art)
- [美術関連プロパティ (Q27918607)](https://www.wikidata.org/wiki/Q27918607)


### メディア芸術データベースとのリンク
- [P7886 (メディア芸術データベース識別子)](https://www.wikidata.org/wiki/Property:P7886)

#### 使用例
{{< yasgui-query yasgui-id="wikidata" title="メディア芸術データベース識別子の使用例" >}}
PREFIX bd: <http://www.bigdata.com/rdf#>
PREFIX wikibase: <http://wikiba.se/ontology#>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>

SELECT ?item ?itemLabel ?value
{
  ?item wdt:P7886 ?value .
  SERVICE wikibase:label { bd:serviceParam wikibase:language "ja,en"  }
}
LIMIT 1000
{{< / yasgui-query >}}

#### クラスごとの使用数
{{< yasgui-query yasgui-id="wikidata" title="メディア芸術データベース識別子のクラスごとの使用数" >}}
PREFIX bd: <http://www.bigdata.com/rdf#>
PREFIX wikibase: <http://wikiba.se/ontology#>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>

#top 50 for P31 and P279 of items using P7886
SELECT ?class ?classLabel ?count ?use_as_Label
{  {  SELECT ?class (COUNT(*) AS ?count) (wd:P31 as ?use_as_)
    {  ?a  wdt:P7886  ?p  ; wdt:P31  ?class}
        GROUP BY ?class ORDER BY DESC(?count) LIMIT 50
    }
    UNION
  {  SELECT ?class (COUNT(*) AS ?count) (wd:P279 as ?use_as_)
    {  ?a  wdt:P7886  ?p  ; wdt:P279  ?class}
        GROUP BY ?class ORDER BY DESC(?count) LIMIT 50
    }
    SERVICE wikibase:label { bd:serviceParam wikibase:language "ja,en" }
}
ORDER BY DESC(?count) ?class
{{< / yasgui-query >}}


## クエリ例
### 2020年に公開された邦画 {#japanese-movies-released-2020}
- [P31 (分類, instance of)](https://www.wikidata.org/wiki/Property:P31)
- [P495 (本国)](https://www.wikidata.org/wiki/Property:P495)
- [P577 (出版日)](https://www.wikidata.org/wiki/Property:P577)

{{< yasgui-query yasgui-id="wikidata" title="2020年に公開された邦画" >}}
PREFIX bd: <http://www.bigdata.com/rdf#>
PREFIX wikibase: <http://wikiba.se/ontology#>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>

SELECT DISTINCT ?item ?itemLabel
WHERE {
  ?item wdt:P31 wd:Q11424 ; # 映画
        wdt:P495 wd:Q17 ; # 日本
        wdt:P577 ?pubdate .
  FILTER((?pubdate >= "2020-01-01T00:00:00Z"^^xsd:dateTime) && (?pubdate <= "2020-12-31T00:00:00Z"^^xsd:dateTime))
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja,en". }
}
{{< / yasgui-query >}}

`wd:Q11424` を `wd:Q7889 (コンピュータゲーム)` など、別のクラスに変えて試してみる。


### 映画とその物語の場所を地図上に {#japanese-movies-narrative-location}
- [P840 (物語の舞台)](https://www.wikidata.org/wiki/Property:P840)
- [P625 (位置座標)](https://www.wikidata.org/wiki/Property:P625)

```sparql
#defaultView:Map
SELECT ?movie ?movieLabel ?narrative_location ?narrative_locationLabel ?coordinates
WHERE {
   ?movie wdt:P840 ?narrative_location ;
          wdt:P495 wd:Q17 ; # 日本
          wdt:P31 wd:Q11424 . # 映画
   ?narrative_location wdt:P625 ?coordinates .
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja,en". }
}
```

{{< button href="https://w.wiki/5g7e" >}}Wikidata Query Serviceで実行{{< / button >}}

Ref. [Wikidata:SPARQLクエリサービス/クエリ/例#映画とその物語の場所を地図上に](https://www.wikidata.org/wiki/Wikidata:SPARQL_query_service/queries/examples/ja#%E6%98%A0%E7%94%BB%E3%81%A8%E3%81%9D%E3%81%AE%E7%89%A9%E8%AA%9E%E3%81%AE%E5%A0%B4%E6%89%80%E3%82%92%E5%9C%B0%E5%9B%B3%E4%B8%8A%E3%81%AB)より


### SFライトノベル {#love-comedy-lightnovel}
- [P31 (分類, instance of)](https://www.wikidata.org/wiki/Property:P31)
- [P279 (上位クラス)](https://www.wikidata.org/wiki/Property:P279)
- [P136 (ジャンル)](https://www.wikidata.org/wiki/Property:P136)
- [Q747381 (ライトノベル)](https://www.wikidata.org/wiki/Q747381)
- [Q9326077 (スペキュレイティブ・フィクション )](https://www.wikidata.org/wiki/Q9326077)

{{< yasgui-query yasgui-id="wikidata" title="SFライトノベル" >}}
PREFIX bd: <http://www.bigdata.com/rdf#>
PREFIX wikibase: <http://wikiba.se/ontology#>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>

SELECT DISTINCT ?item ?itemLabel
WHERE {
  ?item wdt:P31/wdt:P279* wd:Q747381 ; # ライトノベル
        wdt:P136/wdt:P279* wd:Q9326077 . # スペキュレイティブ・フィクション 
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja,en". }
}
{{< / yasgui-query >}}
