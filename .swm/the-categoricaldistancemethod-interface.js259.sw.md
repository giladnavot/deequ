---
title: The CategoricalDistanceMethod interface
---
This document will cover the interface <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="49:22:22" line-data="    case class LInfinityMethod(alpha: Option[Double] = None) extends CategoricalDistanceMethod">`CategoricalDistanceMethod`</SwmToken> in the <SwmPath>[src/…/analyzers/Distance.scala](src/main/scala/com/amazon/deequ/analyzers/Distance.scala)</SwmPath> file. We will cover:

1. What is <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="49:22:22" line-data="    case class LInfinityMethod(alpha: Option[Double] = None) extends CategoricalDistanceMethod">`CategoricalDistanceMethod`</SwmToken>
2. Variables and functions

# What is <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="49:22:22" line-data="    case class LInfinityMethod(alpha: Option[Double] = None) extends CategoricalDistanceMethod">`CategoricalDistanceMethod`</SwmToken>

The <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="49:22:22" line-data="    case class LInfinityMethod(alpha: Option[Double] = None) extends CategoricalDistanceMethod">`CategoricalDistanceMethod`</SwmToken> is an interface defined in the <SwmPath>[src/…/analyzers/Distance.scala](src/main/scala/com/amazon/deequ/analyzers/Distance.scala)</SwmPath> file. It is used to represent different methods for calculating the distance between categorical profiles. This interface allows for the implementation of various distance calculation methods, such as the <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="55:23:25" line-data="    /** Calculate distance of numerical profiles based on KLL Sketches and L-Infinity Distance */">`L-Infinity`</SwmToken> method and the <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="27:3:5" line-data="    // Chi-square constants">`Chi-square`</SwmToken> method.

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/Distance.scala" line="49">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="49:5:5" line-data="    case class LInfinityMethod(alpha: Option[Double] = None) extends CategoricalDistanceMethod">`LInfinityMethod`</SwmToken> case class is an implementation of the <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="49:22:22" line-data="    case class LInfinityMethod(alpha: Option[Double] = None) extends CategoricalDistanceMethod">`CategoricalDistanceMethod`</SwmToken> interface. It is used to calculate the distance between categorical profiles using the <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="55:23:25" line-data="    /** Calculate distance of numerical profiles based on KLL Sketches and L-Infinity Distance */">`L-Infinity`</SwmToken> method. This method takes an optional <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="49:7:7" line-data="    case class LInfinityMethod(alpha: Option[Double] = None) extends CategoricalDistanceMethod">`alpha`</SwmToken> parameter.

```scala
    case class LInfinityMethod(alpha: Option[Double] = None) extends CategoricalDistanceMethod
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/Distance.scala" line="50">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="50:5:5" line-data="    case class ChisquareMethod(absThresholdYates: Integer = defaultAbsThresholdYates,">`ChisquareMethod`</SwmToken> case class is another implementation of the <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="53:3:3" line-data="      extends CategoricalDistanceMethod">`CategoricalDistanceMethod`</SwmToken> interface. It is used to calculate the distance between categorical profiles using the <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="27:3:5" line-data="    // Chi-square constants">`Chi-square`</SwmToken> method. This method takes three parameters: <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="50:7:7" line-data="    case class ChisquareMethod(absThresholdYates: Integer = defaultAbsThresholdYates,">`absThresholdYates`</SwmToken>, <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="51:1:1" line-data="                               percThresholdYates: Double = defaultPercThresholdYates,">`percThresholdYates`</SwmToken>, and <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="52:1:1" line-data="                               absThresholdCochran: Integer = defaultAbsThresholdCochran)">`absThresholdCochran`</SwmToken>.

```scala
    case class ChisquareMethod(absThresholdYates: Integer = defaultAbsThresholdYates,
                               percThresholdYates: Double = defaultPercThresholdYates,
                               absThresholdCochran: Integer = defaultAbsThresholdCochran)
      extends CategoricalDistanceMethod
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/Distance.scala" line="96">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="96:3:3" line-data="  def categoricalDistance(sample1: scala.collection.mutable.Map[String, Long],">`categoricalDistance`</SwmToken> function calculates the distance between two categorical profiles based on the specified distance method. It takes two samples, a boolean flag <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="98:1:1" line-data="                          correctForLowNumberOfSamples: Boolean = false,">`correctForLowNumberOfSamples`</SwmToken>, and a <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="99:4:4" line-data="                          method: CategoricalDistanceMethod = LInfinityMethod()): Double = {">`CategoricalDistanceMethod`</SwmToken> as parameters. Depending on the method provided, it calls either <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="101:10:10" line-data="      case LInfinityMethod(alpha) =&gt; categoricalLInfinityDistance(sample1, sample2, correctForLowNumberOfSamples, alpha)">`categoricalLInfinityDistance`</SwmToken> or <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="103:3:3" line-data="        =&gt; categoricalChiSquareTest(">`categoricalChiSquareTest`</SwmToken>.

```scala
  def categoricalDistance(sample1: scala.collection.mutable.Map[String, Long],
                          sample2: scala.collection.mutable.Map[String, Long],
                          correctForLowNumberOfSamples: Boolean = false,
                          method: CategoricalDistanceMethod = LInfinityMethod()): Double = {
    method match {
      case LInfinityMethod(alpha) => categoricalLInfinityDistance(sample1, sample2, correctForLowNumberOfSamples, alpha)
      case ChisquareMethod(absThresholdYates, percThresholdYates, absThresholdCochran)
        => categoricalChiSquareTest(
            sample1,
            sample2,
            correctForLowNumberOfSamples,
            absThresholdYates,
            percThresholdYates,
            absThresholdCochran )
    }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/Distance.scala" line="271">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="271:8:8" line-data="  private[this] def categoricalLInfinityDistance(sample1: scala.collection.mutable.Map[String, Long],">`categoricalLInfinityDistance`</SwmToken> function calculates the distance between two categorical profiles using the <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="55:23:25" line-data="    /** Calculate distance of numerical profiles based on KLL Sketches and L-Infinity Distance */">`L-Infinity`</SwmToken> method. It takes two samples, a boolean flag <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="273:1:1" line-data="                                                 correctForLowNumberOfSamples: Boolean = false,">`correctForLowNumberOfSamples`</SwmToken>, and an optional <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="274:1:1" line-data="                                                 alpha: Option[Double]): Double = {">`alpha`</SwmToken> parameter. It computes the <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="55:23:25" line-data="    /** Calculate distance of numerical profiles based on KLL Sketches and L-Infinity Distance */">`L-Infinity`</SwmToken> distance and selects the appropriate metrics based on the sample sizes.

```scala
  private[this] def categoricalLInfinityDistance(sample1: scala.collection.mutable.Map[String, Long],
                                                 sample2: scala.collection.mutable.Map[String, Long],
                                                 correctForLowNumberOfSamples: Boolean = false,
                                                 alpha: Option[Double]): Double = {
    var n = 0.0
    var m = 0.0
    sample1.keySet.foreach { key =>
      n += sample1(key)
    }
    sample2.keySet.foreach { key =>
      m += sample2(key)
    }
    val combinedKeys = sample1.keySet.union(sample2.keySet)
    var linfSimple = 0.0

    combinedKeys.foreach { key =>
      val cdf1 = sample1.getOrElse(key, 0L) / n
      val cdf2 = sample2.getOrElse(key, 0L) / m
      val cdfDiff = Math.abs(cdf1 - cdf2)
      linfSimple = Math.max(linfSimple, cdfDiff)
    }
    selectMetrics(linfSimple, n, m, correctForLowNumberOfSamples, alpha)
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/Distance.scala" line="136">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="136:8:8" line-data="  private[this] def categoricalChiSquareTest(sample: scala.collection.mutable.Map[String, Long],">`categoricalChiSquareTest`</SwmToken> function calculates the distance between two categorical profiles using the <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="27:3:5" line-data="    // Chi-square constants">`Chi-square`</SwmToken> test. It takes two samples, a boolean flag <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="138:1:1" line-data="                                             correctForLowNumberOfSamples: Boolean = false,">`correctForLowNumberOfSamples`</SwmToken>, and three threshold parameters. It normalizes the expected input, regroups categories if necessary, and runs the <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="27:3:5" line-data="    // Chi-square constants">`Chi-square`</SwmToken> test to return either the statistics or <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/Distance.scala" pos="161:19:21" line-data="      // run chi-square test and return statistics or p-value">`p-value`</SwmToken>.

```scala
  private[this] def categoricalChiSquareTest(sample: scala.collection.mutable.Map[String, Long],
                                             expected: scala.collection.mutable.Map[String, Long],
                                             correctForLowNumberOfSamples: Boolean = false,
                                             absThresholdYates : Integer = defaultAbsThresholdYates,
                                             percThresholdYates : Double = defaultPercThresholdYates,
                                             absThresholdCochran : Integer = defaultAbsThresholdCochran): Double = {

    val sampleSum: Double = sample.filter(e => expected.contains(e._1)).values.sum
    val expectedSum: Double = expected.values.sum

    // Normalize the expected input, normalization is required to conduct the chi-square test
    // While normalization is already included in the mllib chi-square test,
    // we perform normalization manually to execute proper regrouping
    // https://spark.apache.org/docs/3.1.3/api/scala/org/apache/spark/mllib/stat/Statistics$.html#chiSqTest
    val expectedNorm: scala.collection.mutable.Map[String, Double] =
      expected.map(e => (e._1, e._2 / expectedSum * sampleSum))

    // Call the function that regroups categories if necessary depending on thresholds
    val (regroupedSample, regroupedExpected) = regroupCategories(
      sample.map(e => (e._1, e._2.toDouble)), expectedNorm, absThresholdYates, percThresholdYates, absThresholdCochran)

    // If less than 2 categories remain we cannot conduct the test
    if (regroupedExpected.keySet.size < chisquareMinDimension) {
      Double.NaN
    } else {
      // run chi-square test and return statistics or p-value
      val result = chiSquareTest(regroupedSample, regroupedExpected)
      if (correctForLowNumberOfSamples) {
        result.statistic
      } else {
        result.pValue
      }
    }
  }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBZGVlcXUlM0ElM0Fhd3NsYWJz" repo-name="deequ"><sup>Powered by [Swimm](/)</sup></SwmMeta>
