## 開発作業が必要な個所を見つける

リライトツールはStruts1、SAStrutsの代表的な利用方法に対する自動変換をサポートしていますが、リライトツールがサポートしていない利用方法については個別の開発作業が必要となります。

「サンプルアプリケーションを変換する([Struts1](../getting_started/sample_change_struts.md),[SAStruts](../getting_started/sample_change_sastruts.md))」では変換後の手作業による修正がほぼ不要となるように事前整備したサンプルアプリケーションを使用しましたが、実際のアプリケーションを用いる場合は変換後に多くの開発作業が必要となります。<br>

本章ではコンパイル結果やソース上の注釈コメントなどから、開発作業が必要な個所を見つける方法を説明します。

<table border="2">
<tr><td>
アプリケーションはフレームワークの様々な実装内容に依存している可能性があるので、ソースコードの記述内容から開発作業が必要な個所をすべて洗い出すのは困難です。<br>
そのため、開発完了後、新旧のアプリケーションの挙動を比較する新旧比較テストを全ての画面に対して実施することを推奨します。
</td></tr>
</table>


## 1.Javaソースファイル

### 1.1 アノテーションのTODOコメント

未対応のアノテーションを使用している場合、そのアノテーションの直前に以下のような注釈コメントが表示されます。<br>

```
// TODO ツールで変換できません
```
このコメントが出力されているアノテーションは個別対応が必要です。

**出力例：一般的なアノテーション**<br>
未対応のアノテーションはコメントアウトされます。<br>

```Java
// TODO ツールで変換できません
//@ManyToOne
// TODO ツールで変換できません
//@JoinColumn(name = "ACCESS_STATUS", referencedColumnName = "CODE_VALUE")
public CodeMaster getCodeMasterAccessStatus() {
   return codeMasterAccessStatus;
}

```

**出力例：バリデーションアノテーション**<br>
未対応のアノテーションはコメントアウトされます。<br>

```java
// TODO ツールで変換できません
// @Validwhen(test = "(*this* == newPassword)", msg = @Msg(key = "A001"))
public String chkPassword;

```

### 1.2 Javaソース上のTODOコメント

リライトツールによる変換が未対応となっている表現が使用されている場合は，その直前に以下のような注釈コメントが表示されます。

```
// TODO ツールで変換できません : where
```

```
// TODO この組み合わせの挙動は移行元と異なる場合があります。詳細はドキュメントを参照
```

これらのコメントが出力されている処理は個別対応が必要です。

**出力例：変換できなかった場合**<br>
「ツールで変換できません 」の注釈コメントが表示されている場合は、変換できなかったことを意味します。<br>
変換できなかった行はコメントアウトされます。

```java
// TODO ツールで変換できません : where
// List<Tbjtxshubetsukihonnjhoknri> list =  select().where(eq(sakujoflag(),Short.valueOf("0"))).orderBy(asc(jtxDatashubetsu())).getResultList();
```

**出力例：変換できたが移行元と挙動が異なる場合**<br>
「この組み合わせの挙動は移行元と異なる場合があります」の注釈コメントが表示されている場合は、変換できているが移行元の挙動を完全には再現できていないことを意味します。<br>

この注釈コメントが出力された場合は、[新旧API対応表](../reference/api_conversion_reference.pdf)や互換ライブラリのJavadoc([Struts1](https://oscana.github.io/javadoc/oscana-s2n-runtime-struts),[SAStruts](https://oscana.github.io/javadoc/oscana-s2n-runtime-sastruts))を確認してください。<br>
この例の場合、CreateAndCopyにおけるincludes, prefixの併用は挙動が移行元と異なるため、個別対応が必要になることが分かります。

```java
// TODO この組み合わせの挙動は移行元と異なる場合があります。詳細はドキュメントを参照
entity1 = new CreateAndCopy<>(testEntity.getClass(), testMap14).includes("test").prefix("test").execute();

```


### 1.3 旧フレームワークのAPI呼出し

Struts1，SAStrutsのAPIを使用している個所のうち、サポートしているAPIを使用している箇所は、リライトツールにより互換ライブラリのAPIを使用するように変換されますが、サポートされていないAPIを使用している箇所は、リライトツールは変換しません。<br>
Nablarchプロジェクト上にはStruts1，SAStrutsのAPIは存在しないので、変換されなかった個所はコンパイルエラーとなります。<br>

コンパイルエラーとなった箇所は個別対応が必要です。

## 2.JSPファイル

### 2.1. JSP上のTODOコメント

未対応のカスタムタグや属性を使用している場合、そのタグの直前に以下のような注釈コメントが表示されます。<br>

```
<%-- // TODO ツールで変換できません --%>
```

このコメントが出力されているカスタムタグは個別対応が必要です。<br>

**出力例：未対応のカスタムタグを使用している場合**<br>
未対応のカスタムタグはコメントアウトされます。<br>

```jsp
<%-- // TODO ツールで変換できません --%>
<%-- <html:cancel> --%>
```

**出力例：未対応の属性を使用している場合**<br>
未対応の属性は削除されます。（削除された属性は注釈コメント内に移動）<br>

```jsp
<%-- // TODO ツールで変換できません 未サポート属性：unEditableName="doRegist"  unEditableValue="登録"  --%>
<n:button name="doConfirm" value="確認" cssClass="button" uri="doConfirm" >確認</n:button>
```

## 3.設定ファイル
### 3.1 SQLファイル上のTODOコメント

[S2JDBCのSQLファイル](http://s2container.seasar.org/2.4/ja/s2jdbc_tutorial.html#SQL%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB)はS2JDBCの制御構文を/* */のコメントとして記述します。<br>
このうちリライトツールによる変換対象となるのは以下の３種類で、IFについては変換できるものが限られています。

|対象|表記|変換できるもの（制約）|
|--|--|--|
|バインド変数|/\*変数名\*/|なし|
|IF文|/\*IF 条件式 \*/<br>　　～<br>/\*END\*/|以下の３つの条件を満たすIF文のみ変換できる。<br>・条件式が単項式である。<br>・WHERE句内で使用されている。<br>・/\*IF\*/～/\*END\*/で囲われた範囲が１行である|
|リテラル置換|/\*$変数名\*/|なし|

リライトツールによる変換が未対応のものが使用されている場合、その直前に以下のような注釈コメントが表示されます。<br>

```
-- TODO ツールで変換できません
```

このコメントが出力されている制御構文は個別対応が必要です。<br>

**出力例：**<br>

```sql
ORDER BY
   HANNEIYOTEIHI,
-- TODO ツールで変換できません
/*IF null != kid_EQ*/
   USERCODE
/*END*/
```


### 3.2 Struts設定ファイル上のTODOコメント

`struts-config.xml`, `validation.xml`に未対応の要素や属性が使用されている場合は、その直前に以下のような注釈コメントが表示されます。<br>

```
<!--TODO ツールで変換できません  属性：cancellable -->
```

`struts-config.xml`, `validation.xml`の内容はJavaコードにリライトされますが、未対応の要素や属性が使用されている場合、その情報は引き継がれません。<br>
このコメントが出力されている要素や属性は個別対応が必要です。<br>

**出力例：**<br>

```xml
<action forward="/registration.jsp" path="/registration"/>
<!--TODO ツールで変換できません  属性：cancellable -->
<action cancellable="true" input="input" name="registrationForm" path="/registration-submit" scope="request" type="org.apache.struts.webapp.validator.RegistrationAction" validate="true">
```


