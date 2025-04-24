---
title: The Histogram class
---
This document will cover the class Histogram in the repo. We will cover:

1. What Histogram is
2. Variables and functions in Histogram

# What is Histogram

The Histogram class in <SwmPath>[src/…/analyzers/Histogram.scala](src/main/scala/com/amazon/deequ/analyzers/Histogram.scala)</SwmPath> is used to summarize the values in a column of a <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="62:10:10" line-data="  override def computeStateFrom(data: DataFrame,">`DataFrame`</SwmToken>. It groups the given column's values and calculates either the number of rows with that specific value and the fraction of this value or the sum of values in another column.

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" line="152">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="152:3:3" line-data="  val NullFieldReplacement = &quot;NullValue&quot;">`NullFieldReplacement`</SwmToken> is used to replace null values in the column being analyzed.

```scala
  val NullFieldReplacement = "NullValue"
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" line="153">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="153:3:3" line-data="  val MaximumAllowedDetailBins = 1000">`MaximumAllowedDetailBins`</SwmToken> sets the maximum number of bins for which histogram details are provided.

```scala
  val MaximumAllowedDetailBins = 1000
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" line="154">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="154:3:3" line-data="  val count_function = &quot;count&quot;">`count_function`</SwmToken> is a string representing the count function used in aggregation.

```scala
  val count_function = "count"
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" line="155">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="155:3:3" line-data="  val sum_function = &quot;sum&quot;">`sum_function`</SwmToken> is a string representing the sum function used in aggregation.

```scala
  val sum_function = "sum"
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" line="158">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="158:3:3" line-data="    def query(column: String, data: DataFrame): DataFrame">`query`</SwmToken> in the <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="167:9:9" line-data="  case object Count extends AggregateFunction {">`AggregateFunction`</SwmToken> trait is used to query the <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="158:14:14" line-data="    def query(column: String, data: DataFrame): DataFrame">`DataFrame`</SwmToken> based on the column and perform the aggregation.

```scala
    def query(column: String, data: DataFrame): DataFrame

```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" line="160">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="160:3:3" line-data="    def total(data: DataFrame): Long">`total`</SwmToken> in the <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="167:9:9" line-data="  case object Count extends AggregateFunction {">`AggregateFunction`</SwmToken> trait calculates the total count or sum based on the aggregation function.

```scala
    def total(data: DataFrame): Long
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" line="162">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="162:3:3" line-data="    def aggregateColumn(): Option[String]">`aggregateColumn`</SwmToken> in the <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="167:9:9" line-data="  case object Count extends AggregateFunction {">`AggregateFunction`</SwmToken> trait returns the column used for aggregation, if any.

```scala
    def aggregateColumn(): Option[String]
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" line="164">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="164:3:3" line-data="    def function(): String">`function`</SwmToken> in the <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="167:9:9" line-data="  case object Count extends AggregateFunction {">`AggregateFunction`</SwmToken> trait returns the name of the aggregation function.

```scala
    def function(): String
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" line="167">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="167:5:5" line-data="  case object Count extends AggregateFunction {">`Count`</SwmToken> object extends <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="167:9:9" line-data="  case object Count extends AggregateFunction {">`AggregateFunction`</SwmToken> and implements the <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="168:5:5" line-data="    override def query(column: String, data: DataFrame): DataFrame = {">`query`</SwmToken>, <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="181:5:5" line-data="    override def total(data: DataFrame): Long = {">`total`</SwmToken>, <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="177:5:5" line-data="    override def aggregateColumn(): Option[String] = None">`aggregateColumn`</SwmToken>, and <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="179:5:5" line-data="    override def function(): String = count_function">`function`</SwmToken> methods to perform count aggregation.

```scala
  case object Count extends AggregateFunction {
    override def query(column: String, data: DataFrame): DataFrame = {
      data
        .select(col(column).cast(StringType))
        .na.fill(Histogram.NullFieldReplacement)
        .groupBy(column)
        .count()
        .withColumnRenamed("count", Analyzers.COUNT_COL)
    }

    override def aggregateColumn(): Option[String] = None

    override def function(): String = count_function

    override def total(data: DataFrame): Long = {
      data.count()
    }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" line="186">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="186:5:5" line-data="  case class Sum(aggColumn: String) extends AggregateFunction {">`Sum`</SwmToken> case class extends <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="186:15:15" line-data="  case class Sum(aggColumn: String) extends AggregateFunction {">`AggregateFunction`</SwmToken> and implements the <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="187:5:5" line-data="    override def query(column: String, data: DataFrame): DataFrame = {">`query`</SwmToken>, <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="196:5:5" line-data="    override def total(data: DataFrame): Long = {">`total`</SwmToken>, <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="200:5:5" line-data="    override def aggregateColumn(): Option[String] = {">`aggregateColumn`</SwmToken>, and <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="204:5:5" line-data="    override def function(): String = sum_function">`function`</SwmToken> methods to perform sum aggregation.

```scala
  case class Sum(aggColumn: String) extends AggregateFunction {
    override def query(column: String, data: DataFrame): DataFrame = {
      data
        .select(col(column).cast(StringType), col(aggColumn).cast(LongType))
        .na.fill(Histogram.NullFieldReplacement)
        .groupBy(column)
        .sum(aggColumn)
        .withColumnRenamed("count", Analyzers.COUNT_COL)
    }

    override def total(data: DataFrame): Long = {
      data.groupBy().sum(aggColumn).first().getLong(0)
    }

    override def aggregateColumn(): Option[String] = {
      Some(aggColumn)
    }

    override def function(): String = sum_function
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" line="62">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="62:5:5" line-data="  override def computeStateFrom(data: DataFrame,">`computeStateFrom`</SwmToken> computes the state from the given <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="62:10:10" line-data="  override def computeStateFrom(data: DataFrame,">`DataFrame`</SwmToken>, applying optional filters and binning functions.

```scala
  override def computeStateFrom(data: DataFrame,
                                filterCondition: Option[String] = None): Option[FrequenciesAndNumRows] = {

    // TODO figure out a way to pass this in if its known before hand
    val totalCount = if (computeFrequenciesAsRatio) {
      aggregateFunction.total(data)
    } else {
      1
    }

    val df = data
      .transform(filterOptional(where))
      .transform(binOptional(binningUdf))
    val frequencies = query(df)

    Some(FrequenciesAndNumRows(frequencies, totalCount))
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" line="80">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="80:5:5" line-data="  override def computeMetricFrom(state: Option[FrequenciesAndNumRows]): HistogramMetric = {">`computeMetricFrom`</SwmToken> computes the histogram metric from the given state.

```scala
  override def computeMetricFrom(state: Option[FrequenciesAndNumRows]): HistogramMetric = {

    state match {

      case Some(theState) =>
        val value: Try[Distribution] = Try {

          val countColumnName = theState.frequencies.schema.fields
            .find(field => field.dataType == LongType && field.name != column)
            .map(_.name)
            .getOrElse(throw new IllegalStateException(s"Count column not found in the frequencies DataFrame"))

          val topNRowsDF = theState.frequencies
            .orderBy(col(countColumnName).desc)
            .limit(maxDetailBins)
            .collect()

          val binCount = theState.frequencies.count()

          val columnName = theState.frequencies.columns
            .find(_ == column)
            .getOrElse(throw new IllegalStateException(s"Column $column not found"))

          val histogramDetails = topNRowsDF
            .map { row =>
              val discreteValue = row.getAs[String](columnName)
              val absolute = row.getAs[Long](countColumnName)
              val ratio = absolute.toDouble / theState.numRows
              discreteValue -> DistributionValue(absolute, ratio)
            }
            .toMap

          Distribution(histogramDetails, binCount)
        }

        HistogramMetric(column, value)

      case None =>
        HistogramMetric(column, Failure(Analyzers.emptyStateException(this)))
    }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" line="122">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="122:5:5" line-data="  override def toFailureMetric(exception: Exception): HistogramMetric = {">`toFailureMetric`</SwmToken> converts an exception to a histogram metric failure.

```scala
  override def toFailureMetric(exception: Exception): HistogramMetric = {
    HistogramMetric(column, Failure(MetricCalculationException.wrapIfNecessary(exception)))
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" line="126">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="126:5:5" line-data="  override def preconditions: Seq[StructType =&gt; Unit] = {">`preconditions`</SwmToken> returns a sequence of preconditions that must be met for the histogram analysis.

```scala
  override def preconditions: Seq[StructType => Unit] = {
    PARAM_CHECK :: Preconditions.hasColumn(column) :: Nil
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" line="132">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="132:5:5" line-data="  private def filterOptional(where: Option[String])(data: DataFrame): DataFrame = {">`filterOptional`</SwmToken> applies an optional filter condition to the <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="132:19:19" line-data="  private def filterOptional(where: Option[String])(data: DataFrame): DataFrame = {">`DataFrame`</SwmToken>.

```scala
  private def filterOptional(where: Option[String])(data: DataFrame): DataFrame = {
    where match {
      case Some(condition) => data.filter(condition)
      case _ => data
    }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" line="139">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="139:5:5" line-data="  private def binOptional(binningUdf: Option[UserDefinedFunction])(data: DataFrame): DataFrame = {">`binOptional`</SwmToken> applies an optional binning function to the <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="139:19:19" line-data="  private def binOptional(binningUdf: Option[UserDefinedFunction])(data: DataFrame): DataFrame = {">`DataFrame`</SwmToken>.

```scala
  private def binOptional(binningUdf: Option[UserDefinedFunction])(data: DataFrame): DataFrame = {
    binningUdf match {
      case Some(bin) => data.withColumn(column, bin(col(column)))
      case _ => data
    }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" line="146">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="146:5:5" line-data="  private def query(data: DataFrame): DataFrame = {">`query`</SwmToken> performs the aggregation query on the <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Histogram.scala" pos="146:10:10" line-data="  private def query(data: DataFrame): DataFrame = {">`DataFrame`</SwmToken> using the specified aggregate function.

```scala
  private def query(data: DataFrame): DataFrame = {
    aggregateFunction.query(this.column, data)
  }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBZGVlcXUlM0ElM0Fhd3NsYWJz" repo-name="deequ"><sup>Powered by [Swimm](/)</sup></SwmMeta>
