## アプリケーションを変換する


本書では、実際のアプリケーションを変換し、必要に応じてコードを修正して動作させるための手順を説明します。
<table border="2">
<tr><td>
<b>[前提]</b><br>
以下の手順は実施済みと仮定します。<br>
<a href="../getting_started/tool_install.md">１．リライトツールをインストールする</a><br>
<a href="../getting_started/runtime_install.md">２．互換ライブラリをインストールする</a><br>
※今回変換するアプリケーション用に、新たなNablarchブランクプロジェクトを用意してください。
</td></tr>
</table>


## 1.アプリケーション変換手順

実際のアプリケーションを変換し、動作させる手順は以下のとおりです。<br>

| No | 表題 | 実施内容詳細 | 参照先 |
|--|--|--|--|
| 1 | リライトツールへの変換対象の配置 | 参照先の記述に沿って、アプリケーションソースをリライトツールにコピーしてください。| [Struts1からの移行](../getting_started/sample_change_struts.md#21-リライトツールへの変換対象の配置) /[SAStrutsからの移行](../getting_started/sample_change_sastruts.md#21-リライトツールへの変換対象の配置) |
| 2 | リライトツールの設定変更 | 参照先の記述に沿いますが、設定ファイルの記述方法については「[2.1.リライトツールの設定方法](#21リライトツールの設定方法)」を参照してください。 <br>参照先では`sample.properties`を修正する手順を紹介していますが、別のファイルとして作成しても構いません。| [Struts1からの移行](../getting_started/sample_change_struts.md#22-リライトツールの設定変更) /[SAStrutsからの移行](../getting_started/sample_change_sastruts.md#22-リライトツールの設定変更) |
| 3 | リライトツールの実行 | 参照先の記述に沿って、リライトツールを実行してください。<br>前項の作業で設定ファイル名を変更した場合は、コマンドの引数に付与している`sample.properties`は適切な名前に変更してください。 | [Struts1からの移行](../getting_started/sample_change_struts.md#23-リライトツールの実行) /[SAStrutsからの移行](../getting_started/sample_change_sastruts.md#23-リライトツールの実行) |
| 4 | ソースコードの修正 | リライトツールによる変換がサポートされていない箇所は、手作業による修正が必要です。<br>詳細は[「開発作業が必要な個所を見つける」](rebuild_guide.md)を参照してください。<br>この作業はアプリケーションのビルドを実施する前であればどこで実施しても構いません。 | |
| 5 | 変換結果のNablarchプロジェクトへの配置 | 参照先の記述に沿って、変換結果をNablarchプロジェクトにコピーしてください。 | [Struts1からの移行](../getting_started/sample_change_struts.md#31-変換結果のnablarchプロジェクトへの配置) /[SAStrutsからの移行](../getting_started/sample_change_sastruts.md#31-変換結果のnablarchプロジェクトへの配置) |
| 6 | 互換ライブラリを使用するための設定修正 | 参照先の記述に沿って、Nablarchプロジェクトの設定ファイルを修正してください。 | [Struts1からの移行](../getting_started/sample_change_struts.md#32-struts1用互換ライブラリを使用するための設定修正) /[SAStrutsからの移行](../getting_started/sample_change_sastruts.md#32-sastruts用互換ライブラリを使用するための設定修正) |
| 7 | 設定ファイルの修正 |参照先の記述に沿いますが、設定ファイルの記述方法については「[2.2.設定ファイルの修正方法](#22設定ファイルの修正方法)」を参照してください。 | [Struts1からの移行](../getting_started/sample_change_struts.md#33-設定ファイルの修正) /[SAStrutsからの移行](../getting_started/sample_change_sastruts.md#33-設定ファイルの修正) |
| 8 | 画面表示用ファイルの配置 | 参照先の記述に沿って、アプリケーションのhtmlや画像、スクリプト等をNablarchプロジェクトに配置してください。 | [Struts1からの移行](../getting_started/sample_change_struts.md#34-画面表示用ファイルの配置) /[SAStrutsからの移行](../getting_started/sample_change_sastruts.md#34-画面表示用ファイルの配置) |
| 9 | アプリケーションのビルド | 参照先の記述に沿って、アプリケーションをビルドしてください。 | [Struts1からの移行](../getting_started/sample_change_struts.md#35-アプリケーションのビルド) /[SAStrutsからの移行](../getting_started/sample_change_sastruts.md#35-アプリケーションのビルド) |
| 10 | アプリケーションの実行 | 参照先の記述に沿って、アプリケーションを実行してください。 | [Struts1からの移行](../getting_started/sample_change_struts.md#36-アプリケーションの実行) /[SAStrutsからの移行](../getting_started/sample_change_sastruts.md#36-アプリケーションの実行) |




### SQLファイルの変換方法(SAStruts)

リライトツールはSAStrutsで使用するS2JDBCの外部SQLファイルをNablarchのデータベースアクセス機能に準拠した外部SQL形式に変換する機能を提供します。<br>
この機能を使用する場合は「No1.リライトツールへの変換対象の配置」「No5.変換結果のNablarchプロジェクトへの配置」を実施するときに以下を考慮してください。<br>
※サンプルアプリケーションでは使用していません。

<table border="2">
<tr><td>
<b>データベースアクセス機能を使用するための設定について</b><br>
本機能を使用するには、Nablarchのデータベースアクセス機能についての知識が必要です。<br>
Nablarchのデータベースアクセス機能については下記リンク先を参照してください。<br>
<a href="https://nablarch.github.io/docs/LATEST/doc/application_framework/application_framework/libraries/database_management.html">データベースアクセス</a>
</td></tr>
</table>


#### No1.リライトツールへの変換対象の配置

以下の場所にSQLファイルを配置してからリライトツールのjavaコンバータを実行してください。

 - SQLファイルの配置場所:`converter/work/sql/from/`

#### No5.変換結果のNablarchプロジェクトへの配置

リライトツールが生成したSQLファイルはNablarchプロジェクトの所定の位置に配置してください。

- SQLファイルの生成場所:`converter/work/sql/to/`

SQLファイルの配置場所等については[データベースアクセス(JDBCラッパー)](https://nablarch.github.io/docs/LATEST/doc/application_framework/application_framework/libraries/database/database.html)を参照してください。


## 2.実施内容詳細の補足

本章はアプリケーション変換手順に対する特記事項の詳細記述です。

### 2.1.リライトツールの設定方法

リライトツールの設定方法を詳説します。<br>

#### 2.1.1 Struts1，SAStruts共通のパラメータ

これらはStruts1，SAStruts両方に共通するパラメータです。<br>

|項目| 意味 | デフォルト値 |設定方法|
|--|--|--|--|
|projectPath | リライトツールの作業対象パス | work/  | 作業対象パスを変更したい場合はそのパスを指定してください。 |
| basePackage |変換対象アプリケーションのBaseパッケージ  |com.example| 「[2.3.ベースパッケージについて](#23ベースパッケージについて)」を参照してください。 |
| savePathForRoutexml | routes.xmlの出力先 | work/routes.xml  | `routes.xml`の出力先を変更したい場合はそのパスを指定してください。 |
| fileEncoding | ソースファイルの文字コード | UTF-8  | ソースファイルの文字コードを指定してください。ファイルごとに個別の指定はできないので、ソースファイルの文字コードは事前に統一させておく必要があります。 |
| convertMode| 変換元アプリケーションの利用フレームワーク(1...Struts、2...SAStruts)| 2  | 変換元アプリケーションがStruts1であれば1を、SAStrutsであれば2を指定してください。|

#### 2.1.2 Struts1専用のパラメータ

これらはStruts1専用パラメータです。convertMode=2(SAStruts)として実施する場合はすべて空欄に設定してください。

|項目| 意味 | デフォルト値 |設定方法|
|--|--|--|--|
| strutsAnalyze.strutsConfigFile| struts-config.xml（複数指定可） |（なし）| アプリケーションが参照する`struts-config.xml`をprojectPathからの相対パスで指定します。複数モジュール構成における記述方法は下記を参照してください。 |
| strutsAnalyze.validationFile| validation.xml（複数指定可） |（なし）| アプリケーションが参照する`validation.xml`をprojectPathからの相対パスで指定します。複数モジュール構成における記述方法は下記を参照してください。|
| strutsAnalyze.module| struts-config.xmlにmodule付与しているモジュール名（複数指定可） |（なし）| `struts-config.xml`に付与しているモジュール名を指定します。複数モジュール構成における記述方法は下記を参照してください。 |

**[複数モジュール構成時の記述方法]**

Struts1は、1つのアプリケーションを複数のモジュールから構成することが可能です。各モジュールごとに`struts-config.xml`や`validation.xml`を定義することができます。<br>
各モジュールはモジュール名により識別します。<br>
複数モジュール構成のアプリケーションを変換する場合の設定方法は以下のとおりです。

|項目|設定方法|
|--|--|
|strutsAnalyze.strutsConfigFile| `struts-config.xml`をカンマ区切りで列挙してください。|
|strutsAnalyze.validationFile| `validation.xml`をカンマ区切りで列挙してください。|
|strutsAnalyze.module| モジュール名をカンマ区切りで列挙してください。|

それぞれの項目において、列挙する順は関係があります。<br>
対となる`struts-config.xml`、`validation.xml`、モジュール名は同一インデックスとなるように記述してください。<br>
`struts-config.xml`に対する`validation.xml`やモジュール名がない場合はstrutsAnalyze.validationFileやstrutsAnalyze.moduleは空項目として記述してください。

**例：**<br>
モジュールa の設定ファイルが `struts-conf-a.xml`, `validation-a.xml`, モジュールb の設定ファイルが `struts-conf-b.xml`, `validation-b.xml` の場合、以下のように設定します。

```
strutsAnalyze.strutsConfigFile=struts-conf-a.xml,struts-conf-b.xml
strutsAnalyze.validtionFile=validation-a.xml,validation-b.xml
strutsAnalyze.module=a,b
```


### 2.2.設定ファイルの修正方法

Nablarchのブランクプロジェクトの設定方法を詳説します。<br>



#### 2.2.1 web.xml

`web.xml`のfilter-mappingの記述はリライトツールが生成する`routes.xml`のmatch要素を転記してください。

**[routes.xml]**

```
<routes>
   <match path="/validator/registration-submit.do" controller="validator.Registration" action="execute" />
   <match path="/upload /upload-submit.do" controller="upload.Upload" action="execute" />
</routes>
```

　↓　`routes.xml`のmatch要素のpath属性を、`web.xml`のurl-pattern要素として転記してください。

**[web.xml]**

```
<filter-mapping>
  ...
   <url-pattern>/validator/registration-submit.do</url-pattern>
   <url-pattern>/upload/upload-submit.do</url-pattern>
  ...
</filter-mapping>
```

なお、url-patternにはワイルドカードが使用できます。

```
<routes>
   <match path="/add/index" controller="Add" action="index" />
   <match path="/add/submit" controller="Add" action="submit" />
</routes>
```

　↓　上記のように類似したパスはまとめて、以下のように記述することができます。

```
<filter-mapping>
  ...
   <url-pattern>/add/*</url-pattern>
  ...
</filter-mapping>
```


#### 2.2.2 common.config

nablarch.commonProperty.basePackageで指定すべきベースパッケージについては[2.3.ベースパッケージについて](#23ベースパッケージについて)を参照してください。

### 2.3.ベースパッケージについて

ベースパッケージとはActionクラスの共通パッケージのことで、これらを設定する項目は２つあります。<br>
・[リライトツールの設定ファイル](#211-struts1sastruts共通のパラメータ)のbasePackage<br>
・[Nablarchプロジェクトの設定ファイル(common.config)](#222-commonconfig)のnablarch.commonProperty.basePackage<br>

これらの設定方法はStruts1とSAStrutsでは異なるので、それぞれについて説明します。

**[注意事項]**

ベースパッケージはリライト結果に影響します。<br>
ベースパッケージを決めてからリライトツールを実行してください。

#### 2.3.1 Struts1

全てのActionクラスが含まれる適切な階層をベースパッケージとして指定してください。<br>

下記のような構造の場合、org/apache/struts/webapp/であれば全てのActionクラスが含まれます。<br>

```
org/apache/struts/webapp/upload/UploadAction.java
org/apache/struts/webapp/validator/RegistrationAction.java
```

そのため、ベースパッケージは`org.apache.struts.webapp`と設定するのが適切です。<br>
※ベースパッケージはorg.apache.strutsのようにさらに上の階層にしてもかまいません。<br>

Struts1の場合、ベースパッケージの値をそのまま両プロパティの設定値としてください。

|設定項目|設定値|
|--|--|
|basePackage|org.apache.struts.webapp|
|nablarch.commonProperty.basePackage|org.apache.struts.webapp|


#### 2.3.2 SAStruts

SAStrutsのアプリケーションではベースパッケージ名の後に続くactionというパッケージにアクションクラスを格納するルールがあります。<br>

下記のような構造の場合、actionの一つ上位はtutorialとなります。<br>

```
tutorial.action.AddAction.java
tutorial.action.DownloadAction.java
```

そのため、ベースパッケージは`tutorial`となります。<br>

SAStrutsの場合、basePackageはベースパッケージの値をそのまま設定値とし、nablarch.commonProperty.basePackageは予約語の`.action`を連結した文字列を設定値としてください。<br>

|設定項目|設定値|
|--|--|
|basePackage|tutorial|
|nablarch.commonProperty.basePackage|tutorial.action|





