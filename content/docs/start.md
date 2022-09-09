---
title: "はじめに"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

# はじめに

本ハンズオンでは、LOD(Linked Open Data)として公開されている文化・芸術に関する情報に対して、
クエリ言語**SPARQL**を用いた活用方法をご紹介します。

実際の公開SPARQLエンドポイントを利用して、データを取得する簡単なクエリから、複数のデータベースを横断するような応用的なクエリまでを実践します。

## 事前準備
WebブラウザがあればOKです。

SPARQLをはじめて扱う方は、導入として以下の資料などを参照していただくのがオススメです。

- [Linked Open Data 勉強会2020 資料 - SPARQLの簡単な使い方](https://github.com/KnowledgeGraphJapan/LOD-ws-2020#linked-open-data-%E5%8B%89%E5%BC%B7%E4%BC%9A2020%E3%81%AE%E8%B3%87%E6%96%99)
- [ナレッジグラフ推論チャレンジ2021「技術勉強会」 - ナレッジグラフ（RDF）の基礎/ナレッジグラフ（RDF）用クエリ言語SPARQLの基礎](https://github.com/KnowledgeGraphJapan/KGRC-ws-2021/tree/main/Section2#%E3%83%8A%E3%83%AC%E3%83%83%E3%82%B8%E3%82%B0%E3%83%A9%E3%83%95rdf%E3%81%AE%E5%9F%BA%E7%A4%8E%E3%83%8A%E3%83%AC%E3%83%83%E3%82%B8%E3%82%B0%E3%83%A9%E3%83%95rdf%E7%94%A8%E3%82%AF%E3%82%A8%E3%83%AA%E8%A8%80%E8%AA%9Esparql%E3%81%AE%E5%9F%BA%E7%A4%8E)


## クエリの仕様
本ハンズオンでは、基本的にSPARQL 1.1に準拠したクエリを扱います。

- [SPARQL 1.1 Query Language](https://www.w3.org/TR/2013/REC-sparql11-query-20130321/)

ただし、一部のSPARQLエンドポイントにおいては、特定のRDFストアの実装に依存した機能を利用することがあります。