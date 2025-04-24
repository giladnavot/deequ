---
title: The ColumnCondition class
---
This document will cover the class <SwmToken path="src/main/scala/com/amazon/deequ/checks/ColumnCondition.scala" pos="21:7:7" line-data="private[checks] object ColumnCondition {">`ColumnCondition`</SwmToken> in the <SwmPath>[src/…/checks/ColumnCondition.scala](src/main/scala/com/amazon/deequ/checks/ColumnCondition.scala)</SwmPath> file. We will discuss:

1. What <SwmToken path="src/main/scala/com/amazon/deequ/checks/ColumnCondition.scala" pos="21:7:7" line-data="private[checks] object ColumnCondition {">`ColumnCondition`</SwmToken> is.
2. The variables and functions defined in <SwmToken path="src/main/scala/com/amazon/deequ/checks/ColumnCondition.scala" pos="21:7:7" line-data="private[checks] object ColumnCondition {">`ColumnCondition`</SwmToken>.

# What is <SwmToken path="src/main/scala/com/amazon/deequ/checks/ColumnCondition.scala" pos="21:7:7" line-data="private[checks] object ColumnCondition {">`ColumnCondition`</SwmToken>

The <SwmToken path="src/main/scala/com/amazon/deequ/checks/ColumnCondition.scala" pos="21:7:7" line-data="private[checks] object ColumnCondition {">`ColumnCondition`</SwmToken> class is a utility object in the <SwmPath>[src/…/checks/ColumnCondition.scala](src/main/scala/com/amazon/deequ/checks/ColumnCondition.scala)</SwmPath> file. It is used to define conditions on columns in a dataset to check for data quality. Specifically, it provides methods to check if each column in a sequence is not null or if any column in a sequence is not null.

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/ColumnCondition.scala" line="23">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/checks/ColumnCondition.scala" pos="23:3:3" line-data="  def isEachNotNull(cols: Seq[String]): String = {">`isEachNotNull`</SwmToken> is used to check if each column in a given sequence of columns is not null. It maps over the sequence of columns, applies the <SwmToken path="src/main/scala/com/amazon/deequ/checks/ColumnCondition.scala" pos="25:9:9" line-data="      .map(col(_).isNotNull)">`isNotNull`</SwmToken> function to each column, and then reduces the results using the logical AND operator.

```scala
  def isEachNotNull(cols: Seq[String]): String = {
    cols
      .map(col(_).isNotNull)
      .reduce(_ and _)
      .toString()
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/ColumnCondition.scala" line="30">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/checks/ColumnCondition.scala" pos="30:3:3" line-data="  def isAnyNotNull(cols: Seq[String]): String = {">`isAnyNotNull`</SwmToken> is used to check if any column in a given sequence of columns is not null. It maps over the sequence of columns, applies the <SwmToken path="src/main/scala/com/amazon/deequ/checks/ColumnCondition.scala" pos="32:9:9" line-data="      .map(col(_).isNotNull)">`isNotNull`</SwmToken> function to each column, and then reduces the results using the logical OR operator.

```scala
  def isAnyNotNull(cols: Seq[String]): String = {
    cols
      .map(col(_).isNotNull)
      .reduce(_ or _)
      .toString()
  }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBZGVlcXUlM0ElM0Fhd3NsYWJz" repo-name="deequ"><sup>Powered by [Swimm](/)</sup></SwmMeta>
