---
title: The AnalyzerDeserializer class
---
This document will cover the class <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala" pos="102:22:22" line-data="      .registerTypeAdapter(classOf[Analyzer[State[_], Metric[_]]], AnalyzerDeserializer)">`AnalyzerDeserializer`</SwmToken> in the file <SwmPath>[src/…/repository/AnalysisResultSerde.scala](src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala)</SwmPath>. We will cover:

1. What is <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala" pos="102:22:22" line-data="      .registerTypeAdapter(classOf[Analyzer[State[_], Metric[_]]], AnalyzerDeserializer)">`AnalyzerDeserializer`</SwmToken>
2. Variables and functions defined in <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala" pos="102:22:22" line-data="      .registerTypeAdapter(classOf[Analyzer[State[_], Metric[_]]], AnalyzerDeserializer)">`AnalyzerDeserializer`</SwmToken>

# What is <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala" pos="102:22:22" line-data="      .registerTypeAdapter(classOf[Analyzer[State[_], Metric[_]]], AnalyzerDeserializer)">`AnalyzerDeserializer`</SwmToken>

The <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala" pos="102:22:22" line-data="      .registerTypeAdapter(classOf[Analyzer[State[_], Metric[_]]], AnalyzerDeserializer)">`AnalyzerDeserializer`</SwmToken> class in <SwmPath>[src/…/repository/AnalysisResultSerde.scala](src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala)</SwmPath> is responsible for deserializing JSON elements into <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala" pos="434:8:8" line-data="    context: JsonDeserializationContext): Analyzer[State[_], Metric[_]] = {">`Analyzer`</SwmToken> objects. It extends <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala" pos="128:11:11" line-data="private[deequ] object ResultKeyDeserializer extends JsonDeserializer[ResultKey] {">`JsonDeserializer`</SwmToken> and provides the necessary logic to convert JSON data into various types of analyzers used in the deequ library.

<SwmSnippet path="/src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala" line="426">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala" pos="426:8:8" line-data="  private[this] def getColumnsAsSeq(context: JsonDeserializationContext,">`getColumnsAsSeq`</SwmToken> is used to deserialize a JSON array of columns into a Scala sequence of strings.

```scala
  private[this] def getColumnsAsSeq(context: JsonDeserializationContext,
    json: JsonObject): Seq[String] = {

    context.deserialize(json.get(COLUMNS_FIELD), new TypeToken[JList[String]]() {}.getType)
      .asInstanceOf[JArrayList[String]].asScala
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala" line="433">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala" pos="433:5:5" line-data="  override def deserialize(jsonElement: JsonElement, t: Type,">`deserialize`</SwmToken> is the main function of the <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala" pos="102:22:22" line-data="      .registerTypeAdapter(classOf[Analyzer[State[_], Metric[_]]], AnalyzerDeserializer)">`AnalyzerDeserializer`</SwmToken> class. It takes a JSON element and converts it into an <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala" pos="434:8:8" line-data="    context: JsonDeserializationContext): Analyzer[State[_], Metric[_]] = {">`Analyzer`</SwmToken> object based on the <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala" pos="573:3:3" line-data="      case analyzerName =&gt;">`analyzerName`</SwmToken> field in the JSON.

```scala
  override def deserialize(jsonElement: JsonElement, t: Type,
    context: JsonDeserializationContext): Analyzer[State[_], Metric[_]] = {

    val json = jsonElement.getAsJsonObject

    val analyzer = json.get(ANALYZER_NAME_FIELD).getAsString match {

      case "Size" =>
        Size(getOptionalWhereParam(json))

      case "Completeness" =>
        Completeness(
          json.get(COLUMN_FIELD).getAsString,
          getOptionalWhereParam(json),
          getOptionalAnalyzerOptions(json))

      case "Compliance" =>
        Compliance(
          json.get("instance").getAsString,
          json.get("predicate").getAsString,
          getOptionalWhereParam(json),
          getColumnsAsSeq(context, json).toList,
          getOptionalAnalyzerOptions(json))

      case "PatternMatch" =>
        PatternMatch(
          json.get(COLUMN_FIELD).getAsString,
          json.get("pattern").getAsString.r,
          getOptionalWhereParam(json),
          getOptionalAnalyzerOptions(json))

      case "Sum" =>
        Sum(
          json.get(COLUMN_FIELD).getAsString,
          getOptionalWhereParam(json))

      case "RatioOfSums" =>
        RatioOfSums(
          json.get("numerator").getAsString,
          json.get("denominator").getAsString,
          getOptionalWhereParam(json))

      case "Mean" =>
        Mean(
          json.get(COLUMN_FIELD).getAsString,
          getOptionalWhereParam(json))

      case "Minimum" =>
        Minimum(
          json.get(COLUMN_FIELD).getAsString,
          getOptionalWhereParam(json),
          getOptionalAnalyzerOptions(json))

      case "Maximum" =>
        Maximum(
          json.get(COLUMN_FIELD).getAsString,
          getOptionalWhereParam(json),
          getOptionalAnalyzerOptions(json))

      case "CountDistinct" =>
        CountDistinct(getColumnsAsSeq(context, json))

      case "Distinctness" =>
        Distinctness(getColumnsAsSeq(context, json))

      case "Entropy" =>
        Entropy(json.get(COLUMN_FIELD).getAsString)

      case "MutualInformation" =>
        MutualInformation(getColumnsAsSeq(context, json))

      case "UniqueValueRatio" =>
        UniqueValueRatio(
          getColumnsAsSeq(context, json),
          analyzerOptions = getOptionalAnalyzerOptions(json))

      case "Uniqueness" =>
        Uniqueness(
          getColumnsAsSeq(context, json),
          analyzerOptions = getOptionalAnalyzerOptions(json))

      case "Histogram" =>
        Histogram(
          json.get(COLUMN_FIELD).getAsString,
          None,
          json.get("maxDetailBins").getAsInt,
          aggregateFunction = createAggregateFunction(
            getOptionalStringParam(json, "aggregateFunction").getOrElse(Histogram.count_function),
            getOptionalStringParam(json, "aggregateColumn").getOrElse("")))

      case "DataType" =>
        DataType(
          json.get(COLUMN_FIELD).getAsString,
          getOptionalWhereParam(json))

      case "ApproxCountDistinct" =>
        ApproxCountDistinct(
          json.get(COLUMN_FIELD).getAsString,
          getOptionalWhereParam(json))

      case "Correlation" =>
        Correlation(
          json.get("firstColumn").getAsString,
          json.get("secondColumn").getAsString,
          getOptionalWhereParam(json))

      case "StandardDeviation" =>
        StandardDeviation(
          json.get(COLUMN_FIELD).getAsString,
          getOptionalWhereParam(json))

      case "ApproxQuantile" =>
        val column = json.get(COLUMN_FIELD).getAsString
        val quantile = json.get("quantile").getAsDouble
        val relativeError = json.get("relativeError").getAsDouble
        ApproxQuantile(column, quantile, relativeError)

      case "ApproxQuantiles" =>
        val column = json.get(COLUMN_FIELD).getAsString
        val quantile = json.get("quantiles").getAsString.split(",").map { _.toDouble }
        val relativeError = json.get("relativeError").getAsDouble
        ApproxQuantiles(column, quantile, relativeError)

      case "ExactQuantile" =>
        val column = json.get(COLUMN_FIELD).getAsString
        val quantile = json.get("quantile").getAsDouble
        ExactQuantile(column, quantile)

      case "MinLength" =>
        MinLength(
          json.get(COLUMN_FIELD).getAsString,
          getOptionalWhereParam(json),
          getOptionalAnalyzerOptions(json))

      case "MaxLength" =>
        MaxLength(
          json.get(COLUMN_FIELD).getAsString,
          getOptionalWhereParam(json),
          getOptionalAnalyzerOptions(json))

      case analyzerName =>
        throw new IllegalArgumentException(s"Unable to deserialize analyzer $analyzerName.")
    }

    analyzer.asInstanceOf[Analyzer[State[_], Metric[_]]]
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala" line="580">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala" pos="580:8:8" line-data="  private[this] def getOptionalWhereParam(jsonObject: JsonObject): Option[String] = {">`getOptionalWhereParam`</SwmToken> retrieves an optional <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala" pos="48:8:8" line-data="  val WHERE_FIELD = &quot;where&quot;">`where`</SwmToken> parameter from the JSON object.

```scala
  private[this] def getOptionalWhereParam(jsonObject: JsonObject): Option[String] = {
    getOptionalStringParam(jsonObject, WHERE_FIELD)
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala" line="584">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala" pos="584:8:8" line-data="  private[this] def getOptionalStringParam(jsonObject: JsonObject, field: String): Option[String] = {">`getOptionalStringParam`</SwmToken> retrieves an optional string parameter from the JSON object based on the provided field name.

```scala
  private[this] def getOptionalStringParam(jsonObject: JsonObject, field: String): Option[String] = {
    if (jsonObject.has(field)) {
      Option(jsonObject.get(field).getAsString)
    } else {
      None
    }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala" line="592">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala" pos="592:8:8" line-data="  private[this] def getOptionalAnalyzerOptions(jsonObject: JsonObject): Option[AnalyzerOptions] = {">`getOptionalAnalyzerOptions`</SwmToken> retrieves optional analyzer options from the JSON object.

```scala
  private[this] def getOptionalAnalyzerOptions(jsonObject: JsonObject): Option[AnalyzerOptions] = {

    if (jsonObject.has("analyzerOptions")) {
      val options = jsonObject.get("analyzerOptions").getAsJsonObject

      val nullBehavior = if (options.has("nullBehavior")) {
        Some(NullBehavior.withName(options.get("nullBehavior").getAsString))
      } else {
        None
      }

      val filteredRowOutcome = if (options.has("filteredRow")) {
        Some(FilteredRowOutcome.withName(options.get("filteredRow").getAsString))
      } else {
        None
      }

      (nullBehavior, filteredRowOutcome) match {
        case (Some(nb), Some(fr)) => Some(AnalyzerOptions(nb, fr))
        case (None, Some(fr)) => Some(AnalyzerOptions(filteredRow = fr))
        case (Some(nb), None) => Some(AnalyzerOptions(nullBehavior = nb))
        case _ => None
      }
    } else {
      None
    }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala" line="620">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/repository/AnalysisResultSerde.scala" pos="620:8:8" line-data="  private[this] def createAggregateFunction(function: String, aggregateColumn: String): AggregateFunction = {">`createAggregateFunction`</SwmToken> creates an aggregate function based on the provided function name and aggregate column.

```scala
  private[this] def createAggregateFunction(function: String, aggregateColumn: String): AggregateFunction = {
    function match {
      case Histogram.count_function => HistogramCount
      case Histogram.sum_function => HistogramSum(aggregateColumn)
      case _ => throw new IllegalArgumentException("Wrong aggregate function name: " + function)
    }
  }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBZGVlcXUlM0ElM0Fhd3NsYWJz" repo-name="deequ"><sup>Powered by [Swimm](/)</sup></SwmMeta>
