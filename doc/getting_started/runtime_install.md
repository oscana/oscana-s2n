# 互換ライブラリをインストールする

本書ではNablarchのブランクプロジェクトに互換ライブラリをインストールして、リライトツールによって変換されたアプリケーションの動作環境を構築する手順を案内します。

#### ファイル修正箇所の記述ルール

設定ファイルの修正方法は以下の記述ルールに従って表示しています。

```diff
+ このように、行頭に＋(プラス)記号が付与されている行が追記すべき内容です。実際のファイル修正時には、行頭の+記号は不要です。
- このように、行頭に－(マイナス)記号が付与されている行が削除すべき内容です。
```

ファイルの例は実際のファイルと異なる場合があります。その場合、追記個所の前後関係を参考に修正してください。



## 1.Nablarchブランクプロジェクトのインストール

Nablarchのブランクプロジェクトを作成します。
下記リンク先を参照し、起動確認(`mvn waitt:run`コマンドにより画面を表示させる)まで実施してください。

[ブランクプロジェクトの作成](https://nablarch.github.io/docs/LATEST/doc/application_framework/application_framework/blank_project/setup_blankProject/setup_Web.html#firststepgeneratewebblankproject)

※本書では`generateWebProject.bat`の引数を以下のように指定した例として記載しています。
```
generateWebProject.bat 5u18 com.example myapp-web 0.1.0 com.example
```

このコマンドにより作成された`myapp-web`はNablarchのブランクプロジェクトのホームディレクトリとなります。

## 2.pom.xmlの修正

 互換ライブラリはMaven Centralに登録されているので、Nablarch ブランクプロジェクトの`pom.xml`に依存関係を追記することでMavenによる自動ダウンロードの対象に含めることができます。

- 修正対象：`myapp-web/pom.xml`
  - project要素直下のdependencies要素内の末尾に互換ライブラリのdependency要素群を追記してください。

```diff
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  ...
  <dependencies>
     ...
     <!-- Nablarchのテスティングフレームワークを実行するために必要なライブラリ -->
     <dependency>
       <groupId>com.nablarch.framework</groupId>
       <artifactId>nablarch-fw-batch</artifactId>
       <scope>test</scope>
     </dependency>
     <dependency>
       <groupId>com.nablarch.framework</groupId>
       <artifactId>nablarch-fw-messaging</artifactId>
       <scope>test</scope>
     </dependency>
     <dependency>
       <groupId>com.nablarch.framework</groupId>
       <artifactId>nablarch-fw-messaging-http</artifactId>
       <scope>test</scope>
     </dependency>
+    <!-- 互換ライブラリ-->
+    <dependency>
+      <groupId>com.github.oscana</groupId>
+      <artifactId>oscana-s2n-runtime-struts</artifactId>
+      <version>1.0.0</version>
+    </dependency>
+    <dependency>
+      <groupId>javax.inject</groupId>
+      <artifactId>javax.inject</artifactId>
+      <version>1</version>
+    </dependency>
+    <dependency>
+      <groupId>commons-validator</groupId>
+      <artifactId>commons-validator</artifactId>
+      <version>1.2.0</version>
+    </dependency>
+    <dependency>
+      <groupId>javax.servlet</groupId>
+      <artifactId>javax.servlet-api</artifactId>
+      <version>3.1.0</version>
+      <scope>provided</scope>
+    </dependency>
+    <dependency>
+      <groupId>com.nablarch.framework</groupId>
+      <artifactId>nablarch-fw-scoped-dicontainer</artifactId>
+      <version>1.1.0</version>
+    </dependency>
   </dependencies>

```



## 3.設定ファイルの追加と修正

互換ライブラリをNablarchに認識させるために、設定ファイルを追加・修正します。

### 3.1 設定ファイルの追加

以下のファイルを取得元リンクより取得し、配置先で指定したディレクトリに配置してください。

|設定ファイル名  | 配置先 | 取得元 |
|--|--|--|
|S2N_Component.xml  |myapp-web/src/main/resources/   |[S2N_Component.xml](https://github.com/oscana/oscana-s2n-runtime-struts/blob/master/default-config/S2N_Component.xml) |
|interceptors.xml | myapp-web/src/main/resources/ | [interceptors.xml ](https://github.com/oscana/oscana-s2n-runtime-struts/blob/master/default-config/interceptors.xml) |
|SampleAction.java | myapp-web/src/main/java/com/example/ | [SampleAction.java ](https://github.com/oscana/oscana-s2n-runtime-struts/blob/master/default-config/SampleAction.java)|
|DIConfig.java | myapp-web/src/main/java/ | [DIConfig.java ](https://github.com/oscana/oscana-s2n-runtime-struts/blob/master/default-config/DIConfig.java)|
|nablarch.fw.dicontainer.annotation.auto.TraversalConfig | myapp-web/src/main/resources/META-INF/services/ | [nablarch.fw.dicontainer.annotation.auto.TraversalConfig ](https://github.com/oscana/oscana-s2n-runtime-struts/blob/master/default-config/nablarch.fw.dicontainer.annotation.auto.TraversalConfig)|



### 3.2 設定ファイルの修正

以下の5ファイルを修正してください。

#### ■web-boot.xmlへの追記

- 修正対象: `myapp-web/src/main/resources/web-boot.xml`

```diff
 <?xml version="1.0" encoding="UTF-8"?>
 <component-configuration
        xmlns="http://tis.co.jp/nablarch/component-configuration"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://tis.co.jp/nablarch/component-configuration https://nablarch.github.io/schema/component-configuration.xsd">
+  <!-- 互換ライブラリ用のインターセプター設定読み込み -->
+  <import file="interceptors.xml" />
   <!-- プロダクション用の設定読み込み -->
   <import file="web-component-configuration.xml"/>
+   <!-- 互換ライブラリ用の設定読み込み -->
+   <import file="S2N_Component.xml"/>
 </component-configuration>
```

#### ■web-component-configuration.xmlへの追記

- 修正対象:`myapp-web/src/main/resources/web-component-configuration.xml`


```diff
      </list>
    </property>
  </component>

+ <component name="nablarchWebContextHandler" class="nablarch.fw.dicontainer.nablarch.NablarchWebContextHandler"/>
+ <component name="annotationAutoContainerProvider" class="nablarch.fw.dicontainer.nablarch.AnnotationAutoContainerProvider">
+   <property name="requestContextSupplier" ref="nablarchWebContextHandler"/>
+   <property name="sessionContextSupplier" ref="nablarchWebContextHandler"/>
+ </component>

  <!-- 初期化が必要なコンポーネント -->
  <component name="initializer"
             class="nablarch.core.repository.initialization.BasicApplicationInitializer">
    <property name="initializeList">
      <list>
        <!-- web/code.xml:コードのローダ
        <component-ref name="codeLoader" />-->
        <!-- web/code.xml:コードのキャッシュ
        <component-ref name="codeCache" />-->
        <!-- web/session-store.xml:DBストア
        <component-ref name="dbStore" />-->
        <!-- パッケージマッピング-->
        <component-ref name="packageMapping"/>
+       <!-- NablarchDI -->
+       <component-ref name="annotationAutoContainerProvider"/>

      </list>
    </property>
  </component>
```

#### ■handler_dev.xmlへの追記

 - 修正対象:`myapp-web/src/env/dev/resources/handler_dev.xml`

```diff
    <!-- ハンドラキュー構成 -->
  <component name="webFrontController"
             class="nablarch.fw.web.servlet.WebFrontController">
    <property name="handlerQueue">
      <list>
        <component class="nablarch.fw.web.handler.HttpCharacterEncodingHandler"/>
        <component class="nablarch.fw.handler.GlobalErrorHandler"/>
        <component class="nablarch.common.handler.threadcontext.ThreadContextClearHandler"/>
+       <component-ref name="nablarchWebContextHandler"/>
+       <component class="oscana.s2n.handler.HttpResourceHolderHandler"/>
        <component class="nablarch.fw.web.handler.HttpResponseHandler"/>
        <component class="nablarch.fw.web.handler.SecureHandler" />
        <component-ref name="multipartHandler"/>
        <component-ref name="sessionStoreHandler" />
        <!-- ホットデプロイハンドラ -->
        <!--　TODO:開発環境で本ハンドラを使用する場合、各PJでホットデプロイ対象となるパッケージを指定すること
        <component class="nablarch.fw.hotdeploy.HotDeployHandler">
          <property name="targetPackages">
            <list>
              <value>com.nablarch.example.app.web.action</value>
              <value>com.nablarch.example.app.web.form</value>
              <value>com.nablarch.example.app.web.core.validation.validator</value>
              <value>com.nablarch.example.app.web.util</value>
            </list>
          </property>
        </component>
        -->
        <component-ref name="threadContextHandler"/>
        <component class="nablarch.common.web.handler.HttpAccessLogHandler"/>
        <component class="nablarch.fw.web.handler.NormalizationHandler" />
        <component class="nablarch.fw.web.handler.ForwardingHandler"/>
        <component-ref name="httpErrorHandler" />
        <component-ref name="nablarchTagHandler"/>
        <component-ref name="dbConnectionManagementHandler"/>
        <component-ref name="transactionManagementHandler"/>
+       <component class="oscana.s2n.handler.HttpResourceHolderUpdateHandler"/>
        <component-ref name="packageMapping"/>
      </list>
    </property>
  </component>
</component-configuration>

```

#### ■envの下のweb-boot.xmlへの追記

- 修正対象:`myapp-web/src/env/dev/resources/web-boot.xml`

```diff

 <?xml version="1.0" encoding="UTF-8"?>
 <component-configuration
        xmlns="http://tis.co.jp/nablarch/component-configuration"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://tis.co.jp/nablarch/component-configuration https://nablarch.github.io/schema/component-configuration.xsd">
+  <!-- 互換ライブラリ用のインターセプター設定読み込み -->
+  <import file="interceptors.xml" />
   <!-- プロダクション用の設定読み込み -->
   <import file="web-component-configuration.xml"/>
   <!-- 単体試験用の設定 -->
   <import file="override_dev.xml"/>
+  <!-- インターセプタの実行順定義 -->
+  <import file="S2N_Component.xml"/>
 </component-configuration>

```


#### ■messages.propertiesへの追記

- 修正対象:`myapp-web/src/main/resources/messages.properties`

以下からダウンロードした`oscana_messages.txt`に含まれる要素を`messages.properties`に追加してください。

[oscana_messages.txt](https://github.com/oscana/oscana-s2n-runtime-struts/blob/master/default-config/oscana_messages.txt)

また、以下の属性がすでに`messages.properties`にある場合は削除してください。

```
nablarch.core.validation.ee.Required.message
nablarch.core.validation.ee.Length.min.message
nablarch.core.validation.ee.Length.max.message
nablarch.core.validation.ee.Length.fixed.message
nablarch.core.validation.ee.Length.min.max.message
```

以下、修正例
```diff
 errors.doubleSubmission=画面遷移が不正です。

 ## bean validation message
-nablarch.core.validation.ee.Required.message=入力してください。
 nablarch.core.validation.ee.Domain.message=形式が正しくありません。
 nablarch.common.code.validator.ee.CodeValue.message=不正な値が指定されました。
 nablarch.core.validation.ee.DecimalRange.min.message={min}以上の数値を入力してください。
 nablarch.core.validation.ee.DecimalRange.max.message={max}以下の数値を入力してください。
 nablarch.core.validation.ee.DecimalRange.min.max.message={min}以上{max}以下の数値を入力してください。
 nablarch.core.validation.ee.Digits.integer.message=数値を入力してください。
 nablarch.core.validation.ee.Digits.fraction.message=数値を入力してください。
 nablarch.core.validation.ee.Digits.message=数値を入力してください。
-nablarch.core.validation.ee.Length.min.message={min}文字以上の値を入力してください。
-nablarch.core.validation.ee.Length.max.message={max}文字以下の値を入力してください。
-nablarch.core.validation.ee.Length.fixed.message={min}文字の値を入力してください。
-nablarch.core.validation.ee.Length.min.max.message={min}文字以上{max}文字以下の値を入力してください。
 nablarch.core.validation.ee.NumberRange.min.message={min}以上の数値を入力してください。
 nablarch.core.validation.ee.NumberRange.max.message={max}以下の数値を入力してください。
 nablarch.core.validation.ee.NumberRange.min.max.message={min}以上{max}以下の数値を入力してください。
 nablarch.core.validation.ee.Size.min.message=要素数を{min}以上にしてください。
 nablarch.core.validation.ee.Size.max.message=要素数を{max}以下にしてください。
 nablarch.core.validation.ee.Size.min.max.message=要素数を{min}以上{max}以下にしてください。
 nablarch.core.validation.ee.SystemChar.message=不正な文字種の値が指定されました。

+## oscana validation message
+nablarch.core.validation.ee.Required.message={fieldName}は必須です。
+nablarch.core.validation.ee.Length.min.message={fieldName}は{min}文字以上の値を入力してください。
+nablarch.core.validation.ee.Length.max.message={fieldName}は{max}文字以下の値を入力してください。
+nablarch.core.validation.ee.Length.fixed.message={fieldName}は{min}文字の値を入力してください。
+nablarch.core.validation.ee.Length.min.max.message={fieldName}は{min}文字以上{max}文字以下の値を入力してください。
+oscana.s2n.validation.DecimalRange.message={fieldName}は{min}から{max}の間で入力してください。
+oscana.s2n.validation.ByteLength.min.message={fieldName}は{min}バイト以上の値を入力してください。
+oscana.s2n.validation.ByteLength.max.message={fieldName}は{max}バイト以下の値を入力してください。
+oscana.s2n.validation.ByteLength.fixed.message={fieldName}は{min}バイトの値を入力してください。
+oscana.s2n.validation.ByteLength.min.max.message={fieldName}は{min}バイト以上{max}バイト以下の値を入力してください。
+oscana.s2n.validation.ParseInt.message={fieldName}はInt型として不正です。
+oscana.s2n.validation.ParseDouble.message={fieldName}はDouble型として不正です。
+oscana.s2n.validation.ParseFloat.message={fieldName}はFloat型として不正です。
+oscana.s2n.validation.ParseLong.message={fieldName}はLong型として不正です。
+oscana.s2n.validation.ParseShort.message={fieldName}はShort型として不正です。
+oscana.s2n.validation.ParseDate.message={fieldName}はDate型として不正です。
+oscana.s2n.validation.Length.min.message={fieldName}は{min}文字以上の値を入力してください。
+oscana.s2n.validation.Length.max.message={fieldName}は{max}文字以下の値を入力してください。
+oscana.s2n.validation.Length.min.max.message={fieldName}は{min}文字以上{max}文字以下の値を入力してください。
+oscana.s2n.validation.Length.fixed.message={fieldName}は{min}文字の値を入力してください。
+oscana.s2n.validation.ParseByte.message={fieldName}はByte型として不正です。
+oscana.s2n.validation.Pattern.message={fieldName}は入力形式が不正です。入力形式={regexp}。
+oscana.s2n.validation.CreditCardNumber.message={fieldName}はクレジットカードとして不正です。
+oscana.s2n.validation.Email.message={fieldName}はメールとして不正です。
+oscana.s2n.validation.Range.message={fieldName}は{min}以上{max}以下の数値を入力してください。
+oscana.s2n.validation.URL.message={fieldName}はURLとして不正です。
```



## 4.動作確認

Nablarchのブランクプロジェクトのホームディレクトリ上で、以下のコマンドを順に実行し、いずれも `BUILD SUCCESS`となればインストールは成功しています。

```
mvn clean
mvn compile
mvn waitt:run
```
