---
title: The AnalysisResult class
---
This document will cover the <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" pos="25:4:4" line-data="case class AnalysisResult(">`AnalysisResult`</SwmToken> class. We will cover:

1. What <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" pos="25:4:4" line-data="case class AnalysisResult(">`AnalysisResult`</SwmToken> is
2. Variables and functions defined in <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" pos="25:4:4" line-data="case class AnalysisResult(">`AnalysisResult`</SwmToken>

# What is <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" pos="25:4:4" line-data="case class AnalysisResult(">`AnalysisResult`</SwmToken>

The <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" pos="25:4:4" line-data="case class AnalysisResult(">`AnalysisResult`</SwmToken> class in <SwmPath>[src/…/repository/AnalysisResult.scala](src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala)</SwmPath> is used to encapsulate the results of data analysis. It contains the key identifying the dataset and the context of the analyzers applied to the dataset.

<SwmSnippet path="/src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" line="25">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" pos="26:1:1" line-data="    resultKey: ResultKey,">`resultKey`</SwmToken> is used to store the key that identifies the dataset. It is a part of the <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" pos="25:4:4" line-data="case class AnalysisResult(">`AnalysisResult`</SwmToken> case class.

```scala
case class AnalysisResult(
    resultKey: ResultKey,
    analyzerContext: AnalyzerContext
)
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" line="25">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" pos="27:1:1" line-data="    analyzerContext: AnalyzerContext">`analyzerContext`</SwmToken> is used to store the context of the analyzers applied to the dataset. It is a part of the <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" pos="25:4:4" line-data="case class AnalysisResult(">`AnalysisResult`</SwmToken> case class.

```scala
case class AnalysisResult(
    resultKey: ResultKey,
    analyzerContext: AnalyzerContext
)
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" line="34">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" pos="41:3:3" line-data="  def getSuccessMetricsAsDataFrame(">`getSuccessMetricsAsDataFrame`</SwmToken> is used to convert an <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" pos="35:7:7" line-data="    * Get a AnalysisResult as DataFrame containing the success metrics">`AnalysisResult`</SwmToken> to a <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" pos="35:11:11" line-data="    * Get a AnalysisResult as DataFrame containing the success metrics">`DataFrame`</SwmToken> containing the success metrics. It takes a Spark session, an <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" pos="35:7:7" line-data="    * Get a AnalysisResult as DataFrame containing the success metrics">`AnalysisResult`</SwmToken>, a sequence of analyzers, and a sequence of tags as parameters.

```scala
  /**
    * Get a AnalysisResult as DataFrame containing the success metrics
    *
    * @param analysisResult      The AnalysisResult to convert
    * @param forAnalyzers Only include metrics for these Analyzers in the DataFrame
    * @param withTags            Only include these Tags in the DataFrame
    */
  def getSuccessMetricsAsDataFrame(
      sparkSession: SparkSession,
      analysisResult: AnalysisResult,
      forAnalyzers: Seq[Analyzer[_, Metric[_]]] = Seq.empty,
      withTags: Seq[String] = Seq.empty)
    : DataFrame = {

    var analyzerContextDF = AnalyzerContext
      .successMetricsAsDataFrame(sparkSession, analysisResult.analyzerContext, forAnalyzers)
      .withColumn(DATASET_DATE_FIELD, lit(analysisResult.resultKey.dataSetDate))

    analysisResult.resultKey.tags
      .filterKeys(tagName => withTags.isEmpty || withTags.contains(tagName))
      .map { case (tagName, tagValue) =>
          formatTagColumnNameInDataFrame(tagName, analyzerContextDF) -> tagValue}
      .foreach {
        case (key, value) => analyzerContextDF = analyzerContextDF.withColumn(key, lit(value))
      }

    analyzerContextDF
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" line="63">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" pos="70:3:3" line-data="  def getSuccessMetricsAsJson(">`getSuccessMetricsAsJson`</SwmToken> is used to convert an <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" pos="64:7:7" line-data="    * Get a AnalysisResult as Json containing the success metrics">`AnalysisResult`</SwmToken> to a JSON string containing the success metrics. It takes an <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" pos="64:7:7" line-data="    * Get a AnalysisResult as Json containing the success metrics">`AnalysisResult`</SwmToken>, a sequence of analyzers, and a sequence of tags as parameters.

```scala
  /**
    * Get a AnalysisResult as Json containing the success metrics
    *
    * @param analysisResult      The AnalysisResult to convert
    * @param forAnalyzers Only include metrics for these Analyzers in the DataFrame
    * @param withTags            Only include these Tags in the DataFrame
    */
  def getSuccessMetricsAsJson(
      analysisResult: AnalysisResult,
      forAnalyzers: Seq[Analyzer[_, Metric[_]]] = Seq.empty,
      withTags: Seq[String] = Seq.empty)
    : String = {

    var serializableResult = SimpleResultSerde.deserialize(
        AnalyzerContext.successMetricsAsJson(analysisResult.analyzerContext, forAnalyzers))
      .asInstanceOf[Seq[Map[String, Any]]]

    serializableResult = addColumnToSerializableResult(
      serializableResult, DATASET_DATE_FIELD, analysisResult.resultKey.dataSetDate)

    analysisResult.resultKey.tags
      .filterKeys(tagName => withTags.isEmpty || withTags.contains(tagName))
      .map { case (tagName, tagValue) =>
        (formatTagColumnNameInJson(tagName, serializableResult), tagValue)}
      .foreach { case (key, value) => serializableResult = addColumnToSerializableResult(
        serializableResult, key, value)
      }

    SimpleResultSerde.serialize(serializableResult)
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" line="94">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" pos="94:8:8" line-data="  private[this] def addColumnToSerializableResult(">`addColumnToSerializableResult`</SwmToken> is a private helper function used to add a column to a serializable result. It takes a sequence of maps, a tag name, and a serializable tag value as parameters.

```scala
  private[this] def addColumnToSerializableResult(
      serializableResult: Seq[Map[String, Any]],
      tagName: String,
      serializableTagValue: Any)
    : Seq[Map[String, Any]] = {

    if (serializableResult.headOption.nonEmpty &&
      !serializableResult.head.keySet.contains(tagName)) {

      serializableResult.map {
        map => map + (tagName -> serializableTagValue)
      }
    } else {
      serializableResult
    }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" line="111">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" pos="111:8:8" line-data="  private[this] def formatTagColumnNameInDataFrame(">`formatTagColumnNameInDataFrame`</SwmToken> is a private helper function used to format a tag column name in a <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" pos="113:4:4" line-data="      dataFrame: DataFrame)">`DataFrame`</SwmToken>. It takes a tag name and a <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" pos="113:4:4" line-data="      dataFrame: DataFrame)">`DataFrame`</SwmToken> as parameters.

```scala
  private[this] def formatTagColumnNameInDataFrame(
      tagName : String,
      dataFrame: DataFrame)
    : String = {

    var tagColumnName = tagName.replaceAll("[^A-Za-z0-9_]", "").toLowerCase
    if (dataFrame.columns.contains(tagColumnName)) {
      tagColumnName = tagColumnName + "_2"
    }
    tagColumnName
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" line="123">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResult.scala" pos="123:8:8" line-data="  private[this] def formatTagColumnNameInJson(">`formatTagColumnNameInJson`</SwmToken> is a private helper function used to format a tag column name in a JSON string. It takes a tag name and a sequence of maps as parameters.

```scala
  private[this] def formatTagColumnNameInJson(
      tagName : String,
      serializableResult : Seq[Map[String, Any]])
    : String = {

    var tagColumnName = tagName.replaceAll("[^A-Za-z0-9_]", "").toLowerCase

    if (serializableResult.headOption.nonEmpty) {
      if (serializableResult.head.keySet.contains(tagColumnName)) {
        tagColumnName = tagColumnName + "_2"
      }
    }
    tagColumnName
  }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBZGVlcXUlM0ElM0Fhd3NsYWJz" repo-name="deequ"><sup>Powered by [Swimm](/)</sup></SwmMeta>
