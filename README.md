# Oscana: SAStruts/Struts1からNablarchへのマイグレーションツール

## Oscanaとは

**Oscana** は、EOLを迎えた[SAStruts](http://sastruts.seasar.org/)や[Struts1](https://struts.apache.org/struts1eol-announcement.html)を使用したアプリケーションを、長期的に利用可能なOSSフレームワークである[Nablarch](http://nablarch.github.io/)を使用したアプリケーションにマイグレーションするツールです。<br>

Javaのソースコードだけでなく、JSP、SQLファイルや各種設定ファイルも含めた業務アプリケーションの大部分を自動で変換することにより、マイグレーションにおける開発作業にかかる工数を大幅に削減することができます。<br>

EOLを迎えたSAStruts、Struts1は脆弱性への対応が停止しているため、これらの継続利用はセキュリティ上のリスクを伴う可能性があります。Oscanaは、これらのフレームワークを利用するアプリケーションを、安全でモダンなフレームワークであるNablarchのアプリケーションへと自動変換することで、このリスクを迅速に解消します。<br>

### コンセプト<br>

**Oscana** におけるマイグレーションは、業務アプリケーションのうち、移行元フレームワーク(SAStruts/Struts1)に依存する部位だけを、移行先フレームワーク(Nablarch)に依存する形式にリライトする方式を採用しています。<br>
このアプローチは、既存コードの再利用性を高め、機能の移植漏れや仕様不一致などの不具合が起こりにくく、安全・確実にマイグレーションを完遂させることができます。

<img src="doc/image/oscana_gaiyou.png" width ="800" >

Oscanaが提供するリライトツールは、コード解析・変換技術を活用することで依存個所の大半を自動検出し、変換することができます。<br>
Oscanaによる移行先となるNablarchはミッションクリティカルな基幹系システムでの実績が豊富で、長期間使い続けることができるため、移行後は、EOL起因によるアプリケーションフレームワークのマイグレーションの心配がなくなります。また、NablarchはOSSであるため、ベンダロックインとなることもありません。<br>


### リライトツール

リライトツールは移行元フレームワークに依存するアプリケーションコードを、Nablarch形式に自動変換するツールです。<br>

**[リライトツールの主な変換対象]**
  * Java
    * *Action* - インタフェースやアノテーションをNablarch形式にリライトする。
    * *Form* - バリデーションアノテーションをNablarchのバリデーションアノテーションにリライトする。
    * *Service* - DBアクセス処理をNablarchのUniversalDao形式にリライトする。
    * *Entity* - DBMSとのマッピング情報となるアノテーションをNablarchのUniversalDao形式にリライトする。
  * JSP
    * *JSP* - カスタムタグをNablarchのカスタムタグにリライトする。
  * 設定ファイル
    * *SQLファイル* - S2JDBCの制御構文をNablarch仕様の制御構文にリライトする。
    * *struts-config.xml* - 画面遷移情報をAction内のJavaコードにリライトする。
    * *validation.xml* - バリデーション情報をForm内のバリデーションアノテーションにリライトする。


### 互換ライブラリ

互換ライブラリは移行元フレームワークの機能をNablarch上で再現させるものです。<br>
互換ライブラリをNablarchにプラグインすることで、旧フレームワークに依存したコードがそのまま、もしくは、小さな修正だけで動作するようになります。<br>

**[互換ライブラリの主な内容]**
  * *Java互換API* - 旧フレームワークにあるAPIをNablarchに移植したもの。
  * *イベントハンドラアダプタ* - 旧フレームワークで呼び出されるイベントハンドラをNablarchからも呼び出されるようにしたもの。
  * *バリデータ* - 旧フレームワークにあるバリデーションをNablarchに移植したもの。
  * *SQLコンバータ* - S2JDBCの制御構文をNablarch上で実行するSQLコンバータ。
  * *メッセージフィルタ* - 旧フレームワーク形式のメッセージをNablarchでも使用できるようにするフィルタ。




## はじめてみよう - Getting Started

簡単なアプリケーションを変換し、動かしてみることを通じて、**Oscana**を用いたマイグレーションの考え方を紹介したコンテンツです。<br>

リライトツールを用いてアプリケーションを変換し、お使いのPC上に構築したNablarch上で動作確認するまでの手順を記載しています。<br>

本コンテンツでは、SAStruts,Struts1それぞれのExampleをベースにしたサンプルアプリケーションを変換元として提供しています。これらのサンプルは、変換後の手作業による修正がほぼ不要です。簡単な手順にて一連の開発の流れを体験することができます。

１．[リライトツールをインストールする](https://github.com/oscana/oscana-s2n/blob/master/doc/getting_started/tool_install.md)<br>
２．[互換ライブラリをインストールする](https://github.com/oscana/oscana-s2n/blob/master/doc/getting_started/runtime_install.md)<br>
３．[サンプルアプリケーションを変換する(Struts1)](https://github.com/oscana/oscana-s2n/blob/master/doc/getting_started/sample_change_struts.md)<br>
４．[サンプルアプリケーションを変換する(SAStruts)](https://github.com/oscana/oscana-s2n/blob/master/doc/getting_started/sample_change_sastruts.md)<br>

## 活用ガイド

Oscanaを用いたマイグレーションの進め方をより深く解説したコンテンツです。<br>アプリケーションを変換する方法と、コンパイル結果やソース上の注釈コメントなどから、開発作業が必要な個所を見つける方法を記載しています。

１．[アプリケーションを変換する](https://github.com/oscana/oscana-s2n/blob/master/doc/rebuild_guide/users_guide.md)<br>
２．[開発作業が必要な個所を見つける](https://github.com/oscana/oscana-s2n/blob/master/doc/rebuild_guide/rebuild_guide.md)

## リファレンス

Oscanaによるマイグレーションにより、アプリケーションが使用していた旧フレームワーク(SAStruts, Struts1)の機能はNablarchの類似する機能に置き換えられます。<br>
このコンテンツでは、カスタムタグ、バリデーション、APIの新旧対応表を提供します。

[カスタムタグ変換リファレンス](https://github.com/oscana/oscana-s2n/blob/master/doc/reference/customtag_conversion_reference.pdf)<br>
[バリデーション変換リファレンス](https://github.com/oscana/oscana-s2n/blob/master/doc/reference/validation_conversion_reference.pdf)<br>
[新旧API対応表](https://github.com/oscana/oscana-s2n/blob/master/doc/reference/api_reference.pdf)<br>

## ライセンス

Oscanaのソースコードのライセンスは[Apache License 2.0](https://fintan.jp/wp-content/uploads/2018/06/LICENSE.txt) に準拠しています。<br>
付属のドキュメントのライセンスは[Fintan コンテンツ 使用許諾条項](https://fintan.jp/wp-content/uploads/2020/11/172fbd84c9f1ab241e8771b3f2830405.pdf)に準拠しています。

※ 記載されている会社名、製品名は、各社の登録商標または商標です。