---
title: Histogram Metric Class Definitions
---
# Introduction

This document will walk you through the Histogram Metric class definitions in the file <SwmPath>[src/…/metrics/HistogramMetric.scala](src/main/scala/com/amazon/deequ/metrics/HistogramMetric.scala)</SwmPath>.

The Histogram Metric is used to represent the distribution of values in a column of a dataset. It provides methods to access distribution values and to flatten the histogram into a sequence of metrics.

We will cover:

1. The structure of the <SwmToken path="src/main/scala/com/amazon/deequ/metrics/HistogramMetric.scala" pos="23:4:4" line-data="case class Distribution(values: Map[String, DistributionValue], numberOfBins: Long) {">`Distribution`</SwmToken> class.
2. The <SwmToken path="src/main/scala/com/amazon/deequ/metrics/HistogramMetric.scala" pos="29:3:3" line-data="  def argmax: String = {">`argmax`</SwmToken> method in the <SwmToken path="src/main/scala/com/amazon/deequ/metrics/HistogramMetric.scala" pos="23:4:4" line-data="case class Distribution(values: Map[String, DistributionValue], numberOfBins: Long) {">`Distribution`</SwmToken> class.
3. The <SwmToken path="src/main/scala/com/amazon/deequ/metrics/HistogramMetric.scala" pos="37:4:4" line-data="case class HistogramMetric(column: String, value: Try[Distribution]) extends Metric[Distribution] {">`HistogramMetric`</SwmToken> class and its <SwmToken path="src/main/scala/com/amazon/deequ/metrics/HistogramMetric.scala" pos="42:3:3" line-data="  def flatten(): Seq[DoubleMetric] = {">`flatten`</SwmToken> method.

# Distribution class structure

<SwmSnippet path="/src/main/scala/com/amazon/deequ/metrics/HistogramMetric.scala" line="17">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/metrics/HistogramMetric.scala" pos="23:4:4" line-data="case class Distribution(values: Map[String, DistributionValue], numberOfBins: Long) {">`Distribution`</SwmToken> class is a case class that holds the distribution values and the number of bins. It provides a method to access distribution values by key.

```scala
package com.amazon.deequ.metrics

import scala.util.{Failure, Success, Try}

case class DistributionValue(absolute: Long, ratio: Double)

case class Distribution(values: Map[String, DistributionValue], numberOfBins: Long) {
```

---

</SwmSnippet>

# Argmax method in Distribution class

<SwmSnippet path="/src/main/scala/com/amazon/deequ/metrics/HistogramMetric.scala" line="29">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/metrics/HistogramMetric.scala" pos="29:3:3" line-data="  def argmax: String = {">`argmax`</SwmToken> method returns the key with the highest absolute value in the distribution. This is useful for identifying the most frequent value in the distribution.

```scala
  def argmax: String = {
    val (distributionKey, _) = values.toSeq
      .maxBy { case (_, distributionValue) => distributionValue.absolute }

    distributionKey
  }
}
```

---

</SwmSnippet>

# <SwmToken path="src/main/scala/com/amazon/deequ/metrics/HistogramMetric.scala" pos="37:4:4" line-data="case class HistogramMetric(column: String, value: Try[Distribution]) extends Metric[Distribution] {">`HistogramMetric`</SwmToken> class and flatten method

<SwmSnippet path="/src/main/scala/com/amazon/deequ/metrics/HistogramMetric.scala" line="37">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/metrics/HistogramMetric.scala" pos="37:4:4" line-data="case class HistogramMetric(column: String, value: Try[Distribution]) extends Metric[Distribution] {">`HistogramMetric`</SwmToken> class represents a histogram metric for a specific column. It extends the <SwmToken path="src/main/scala/com/amazon/deequ/metrics/HistogramMetric.scala" pos="37:23:23" line-data="case class HistogramMetric(column: String, value: Try[Distribution]) extends Metric[Distribution] {">`Metric`</SwmToken> trait and provides a method to flatten the histogram into a sequence of <SwmToken path="src/main/scala/com/amazon/deequ/metrics/HistogramMetric.scala" pos="42:10:10" line-data="  def flatten(): Seq[DoubleMetric] = {">`DoubleMetric`</SwmToken> instances. This is useful for converting the histogram into a format that can be easily processed and analyzed.

```scala
case class HistogramMetric(column: String, value: Try[Distribution]) extends Metric[Distribution] {
  val entity: Entity.Value = Entity.Column
  val instance: String = column
  val name = "Histogram"

  def flatten(): Seq[DoubleMetric] = {
    value
      .map { distribution =>
        val numberOfBins = Seq(DoubleMetric(entity, s"$name.bins", instance,
          Success(distribution.numberOfBins.toDouble)))
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/metrics/HistogramMetric.scala" line="48">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/metrics/HistogramMetric.scala" pos="42:3:3" line-data="  def flatten(): Seq[DoubleMetric] = {">`flatten`</SwmToken> method maps the distribution values to a sequence of <SwmToken path="src/main/scala/com/amazon/deequ/metrics/HistogramMetric.scala" pos="50:1:1" line-data="            DoubleMetric(entity, s&quot;$name.abs.$key&quot;, instance, Success(distValue.absolute)) ::">`DoubleMetric`</SwmToken> instances, including both absolute and ratio values for each key.

```scala
        val details = distribution.values
          .flatMap { case (key, distValue) =>
            DoubleMetric(entity, s"$name.abs.$key", instance, Success(distValue.absolute)) ::
              DoubleMetric(entity, s"$name.ratio.$key", instance, Success(distValue.ratio)) :: Nil
          }
        numberOfBins ++ details
      }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/metrics/HistogramMetric.scala" line="55">

---

It also handles exceptions by returning a <SwmToken path="src/main/scala/com/amazon/deequ/metrics/HistogramMetric.scala" pos="56:12:12" line-data="        case e: Exception =&gt; Seq(DoubleMetric(entity, s&quot;$name.bins&quot;, instance, Failure(e)))">`DoubleMetric`</SwmToken> instance with the failure information.

```scala
      .recover {
        case e: Exception => Seq(DoubleMetric(entity, s"$name.bins", instance, Failure(e)))
      }
      .get
  }

}
```

---

</SwmSnippet>

This concludes the walkthrough of the main ideas in the Histogram Metric class definitions.

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBZGVlcXUlM0ElM0Fhd3NsYWJz" repo-name="deequ"><sup>Powered by [Swimm](/)</sup></SwmMeta>
