---
title: The DataTypeHistogram class
---
This document will cover the class <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/DataType.scala" pos="63:15:15" line-data="  def fromBytes(bytes: Array[Byte]): DataTypeHistogram = {">`DataTypeHistogram`</SwmToken> in the file <SwmPath>[src/…/analyzers/DataType.scala](src/main/scala/com/amazon/deequ/analyzers/DataType.scala)</SwmPath>. We will cover:

1. What is <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/DataType.scala" pos="63:15:15" line-data="  def fromBytes(bytes: Array[Byte]): DataTypeHistogram = {">`DataTypeHistogram`</SwmToken>
2. Variables and functions

# What is <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/DataType.scala" pos="63:15:15" line-data="  def fromBytes(bytes: Array[Byte]): DataTypeHistogram = {">`DataTypeHistogram`</SwmToken>

The <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/DataType.scala" pos="63:15:15" line-data="  def fromBytes(bytes: Array[Byte]): DataTypeHistogram = {">`DataTypeHistogram`</SwmToken> class is used to represent the distribution of different data types within a dataset. It is part of the Deequ library, which is used for defining unit tests for data to measure data quality in large datasets using Apache Spark. The <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/DataType.scala" pos="63:15:15" line-data="  def fromBytes(bytes: Array[Byte]): DataTypeHistogram = {">`DataTypeHistogram`</SwmToken> class helps in capturing the counts of various data types such as nulls, fractional numbers, integral numbers, booleans, and strings.

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/DataType.scala" line="56">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/DataType.scala" pos="56:3:3" line-data="  val SIZE_IN_BYTES = 40">`SIZE_IN_BYTES`</SwmToken> is used to define the size of the byte array representation of the <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/DataType.scala" pos="63:15:15" line-data="  def fromBytes(bytes: Array[Byte]): DataTypeHistogram = {">`DataTypeHistogram`</SwmToken>.

```scala
  val SIZE_IN_BYTES = 40
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/DataType.scala" line="57">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/DataType.scala" pos="57:8:8" line-data="  private[deequ] val NULL_POS = 0">`NULL_POS`</SwmToken> is used to define the position of the null count in the byte array.

```scala
  private[deequ] val NULL_POS = 0
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/DataType.scala" line="58">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/DataType.scala" pos="58:8:8" line-data="  private[deequ] val FRACTIONAL_POS = 1">`FRACTIONAL_POS`</SwmToken> is used to define the position of the fractional count in the byte array.

```scala
  private[deequ] val FRACTIONAL_POS = 1
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/DataType.scala" line="59">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/DataType.scala" pos="59:8:8" line-data="  private[deequ] val INTEGRAL_POS = 2">`INTEGRAL_POS`</SwmToken> is used to define the position of the integral count in the byte array.

```scala
  private[deequ] val INTEGRAL_POS = 2
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/DataType.scala" line="60">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/DataType.scala" pos="60:8:8" line-data="  private[deequ] val BOOLEAN_POS = 3">`BOOLEAN_POS`</SwmToken> is used to define the position of the boolean count in the byte array.

```scala
  private[deequ] val BOOLEAN_POS = 3
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/DataType.scala" line="61">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/DataType.scala" pos="61:8:8" line-data="  private[deequ] val STRING_POS = 4">`STRING_POS`</SwmToken> is used to define the position of the string count in the byte array.

```scala
  private[deequ] val STRING_POS = 4
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/DataType.scala" line="63">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/DataType.scala" pos="63:3:3" line-data="  def fromBytes(bytes: Array[Byte]): DataTypeHistogram = {">`fromBytes`</SwmToken> is used to create a <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/DataType.scala" pos="63:15:15" line-data="  def fromBytes(bytes: Array[Byte]): DataTypeHistogram = {">`DataTypeHistogram`</SwmToken> instance from a byte array.

```scala
  def fromBytes(bytes: Array[Byte]): DataTypeHistogram = {
    require(bytes.length == SIZE_IN_BYTES)
    val buffer = ByteBuffer.wrap(bytes).asLongBuffer().asReadOnlyBuffer()
    val numNull = buffer.get(NULL_POS)
    val numFractional = buffer.get(FRACTIONAL_POS)
    val numIntegral = buffer.get(INTEGRAL_POS)
    val numBoolean = buffer.get(BOOLEAN_POS)
    val numString = buffer.get(STRING_POS)

    DataTypeHistogram(numNull, numFractional, numIntegral, numBoolean, numString)
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/DataType.scala" line="75">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/DataType.scala" pos="75:3:3" line-data="  def toBytes(">`toBytes`</SwmToken> is used to convert a <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/DataType.scala" pos="63:15:15" line-data="  def fromBytes(bytes: Array[Byte]): DataTypeHistogram = {">`DataTypeHistogram`</SwmToken> instance to a byte array.

```scala
  def toBytes(
      numNull: Long,
      numFractional: Long,
      numIntegral: Long,
      numBoolean: Long,
      numString: Long)
    : Array[Byte] = {

    val out = ByteBuffer.allocate(SIZE_IN_BYTES)
    val outB = out.asLongBuffer()

    outB.put(numNull)
    outB.put(numFractional)
    outB.put(numIntegral)
    outB.put(numBoolean)
    outB.put(numString)

    // TODO avoid allocation
    val bytes = new Array[Byte](out.remaining)
    out.get(bytes)
    bytes
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/DataType.scala" line="98">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/DataType.scala" pos="98:3:3" line-data="  def toDistribution(hist: DataTypeHistogram): Distribution = {">`toDistribution`</SwmToken> is used to convert a <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/DataType.scala" pos="98:8:8" line-data="  def toDistribution(hist: DataTypeHistogram): Distribution = {">`DataTypeHistogram`</SwmToken> instance to a <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/DataType.scala" pos="98:12:12" line-data="  def toDistribution(hist: DataTypeHistogram): Distribution = {">`Distribution`</SwmToken>.

```scala
  def toDistribution(hist: DataTypeHistogram): Distribution = {
    val totalObservations =
      hist.numNull + hist.numString + hist.numBoolean + hist.numIntegral + hist.numFractional

    Distribution(Map(
      DataTypeInstances.Unknown.toString ->
        DistributionValue(hist.numNull, hist.numNull.toDouble / totalObservations),
      DataTypeInstances.Fractional.toString ->
        DistributionValue(hist.numFractional, hist.numFractional.toDouble / totalObservations),
      DataTypeInstances.Integral.toString ->
        DistributionValue(hist.numIntegral, hist.numIntegral.toDouble / totalObservations),
      DataTypeInstances.Boolean.toString ->
        DistributionValue(hist.numBoolean, hist.numBoolean.toDouble / totalObservations),
      DataTypeInstances.String.toString ->
        DistributionValue(hist.numString, hist.numString.toDouble / totalObservations)),
      numberOfBins = 5)
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/DataType.scala" line="116">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/DataType.scala" pos="116:3:3" line-data="  def determineType(dist: Distribution): DataTypeInstances.Value = {">`determineType`</SwmToken> is used to determine the predominant data type from a <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/DataType.scala" pos="116:8:8" line-data="  def determineType(dist: Distribution): DataTypeInstances.Value = {">`Distribution`</SwmToken>.

```scala
  def determineType(dist: Distribution): DataTypeInstances.Value = {

    import DataTypeInstances._

    // If all are unknown, we can't decide
    if (ratioOf(Unknown, dist) == 1.0) {
      Unknown
    } else {
      // If we saw string values or a mix of boolean and numbers, we decide for String
      if (ratioOf(String, dist) > 0.0 ||
        (ratioOf(Boolean, dist) > 0.0 &&
          (ratioOf(Integral, dist) > 0.0 || ratioOf(Fractional, dist) > 0.0))) {
        String
      } else {
        // If we have boolean (but no numbers, because we checked for that), we go with boolean
        if (ratioOf(Boolean, dist) > 0.0) {
          Boolean
        } else {
          // If we have seen one fractional, we go with that type
          if (ratioOf(Fractional, dist) > 0.0) {
            Fractional
          } else {
            Integral
          }
        }
      }
    }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/DataType.scala" line="145">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/DataType.scala" pos="145:8:8" line-data="  private[this] def ratioOf(key: DataTypeInstances.Value, distribution: Distribution): Double = {">`ratioOf`</SwmToken> is a helper function used to calculate the ratio of a specific data type in a <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/DataType.scala" pos="145:21:21" line-data="  private[this] def ratioOf(key: DataTypeInstances.Value, distribution: Distribution): Double = {">`Distribution`</SwmToken>.

```scala
  private[this] def ratioOf(key: DataTypeInstances.Value, distribution: Distribution): Double = {
    distribution.values
      .getOrElse(key.toString, DistributionValue(0L, 0.0))
      .ratio
  }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBZGVlcXUlM0ElM0Fhd3NsYWJz" repo-name="deequ"><sup>Powered by [Swimm](/)</sup></SwmMeta>
