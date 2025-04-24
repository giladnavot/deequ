---
title: The ColumnProfiles class
---
This document will cover the class <SwmToken path="src/main/scala/com/amazon/deequ/profiles/ColumnProfile.scala" pos="73:4:4" line-data="case class ColumnProfiles(">`ColumnProfiles`</SwmToken> in the <SwmPath>[src/…/profiles/ColumnProfile.scala](src/main/scala/com/amazon/deequ/profiles/ColumnProfile.scala)</SwmPath> file. We will discuss:

1. What <SwmToken path="src/main/scala/com/amazon/deequ/profiles/ColumnProfile.scala" pos="73:4:4" line-data="case class ColumnProfiles(">`ColumnProfiles`</SwmToken> is and its purpose.
2. The variables and functions defined in <SwmToken path="src/main/scala/com/amazon/deequ/profiles/ColumnProfile.scala" pos="73:4:4" line-data="case class ColumnProfiles(">`ColumnProfiles`</SwmToken>.

# What is <SwmToken path="src/main/scala/com/amazon/deequ/profiles/ColumnProfile.scala" pos="73:4:4" line-data="case class ColumnProfiles(">`ColumnProfiles`</SwmToken>

The <SwmToken path="src/main/scala/com/amazon/deequ/profiles/ColumnProfile.scala" pos="73:4:4" line-data="case class ColumnProfiles(">`ColumnProfiles`</SwmToken> class in <SwmPath>[src/…/profiles/ColumnProfile.scala](src/main/scala/com/amazon/deequ/profiles/ColumnProfile.scala)</SwmPath> is used to store profiling results for columns. These results are then utilized by the constraint suggestion engine to suggest constraints based on the data profiles.

<SwmSnippet path="/src/main/scala/com/amazon/deequ/profiles/ColumnProfile.scala" line="73">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/profiles/ColumnProfile.scala" pos="74:1:1" line-data="    profiles: Map[String, ColumnProfile],">`profiles`</SwmToken> is a map that stores the profiling results for each column. The key is the column name, and the value is a <SwmToken path="src/main/scala/com/amazon/deequ/profiles/ColumnProfile.scala" pos="74:9:9" line-data="    profiles: Map[String, ColumnProfile],">`ColumnProfile`</SwmToken> object.

```scala
case class ColumnProfiles(
    profiles: Map[String, ColumnProfile],
    numRecords: Long)
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/profiles/ColumnProfile.scala" line="73">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/profiles/ColumnProfile.scala" pos="75:1:1" line-data="    numRecords: Long)">`numRecords`</SwmToken> stores the number of records in the dataset that was profiled.

```scala
case class ColumnProfiles(
    profiles: Map[String, ColumnProfile],
    numRecords: Long)
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/profiles/ColumnProfile.scala" line="80">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/profiles/ColumnProfile.scala" pos="80:3:3" line-data="  def toJson(columnProfiles: Seq[ColumnProfile]): String = {">`toJson`</SwmToken> converts a sequence of <SwmToken path="src/main/scala/com/amazon/deequ/profiles/ColumnProfile.scala" pos="80:10:10" line-data="  def toJson(columnProfiles: Seq[ColumnProfile]): String = {">`ColumnProfile`</SwmToken> objects into a JSON string. This function is useful for exporting the profiling results in a structured format.

```scala
  def toJson(columnProfiles: Seq[ColumnProfile]): String = {

    val json = new JsonObject()

    val columns = new JsonArray()

    columnProfiles.foreach { case profile =>

      val columnProfileJson = new JsonObject()
      columnProfileJson.addProperty("column", profile.column)
      columnProfileJson.addProperty("dataType", profile.dataType.toString)
      columnProfileJson.addProperty("isDataTypeInferred", profile.isDataTypeInferred.toString)

      if (profile.typeCounts.nonEmpty) {
        val typeCountsJson = new JsonObject()
        profile.typeCounts.foreach { case (typeName, count) =>
          typeCountsJson.addProperty(typeName, count.toString)
        }
      }

      columnProfileJson.addProperty("completeness", profile.completeness)
      columnProfileJson.addProperty("approximateNumDistinctValues",
        profile.approximateNumDistinctValues)

      if (profile.histogram.isDefined) {
        val histogram = profile.histogram.get
        val histogramJson = new JsonArray()

        histogram.values.foreach { case (name, distributionValue) =>
          val histogramEntry = new JsonObject()
          histogramEntry.addProperty("value", name)
          histogramEntry.addProperty("count", distributionValue.absolute)
          histogramEntry.addProperty("ratio", distributionValue.ratio)
          histogramJson.add(histogramEntry)
        }

        columnProfileJson.add("histogram", histogramJson)
      }

      profile match {
        case numericColumnProfile: NumericColumnProfile =>
          numericColumnProfile.mean.foreach { mean =>
            columnProfileJson.addProperty("mean", mean)
          }
          numericColumnProfile.maximum.foreach { maximum =>
            columnProfileJson.addProperty("maximum", maximum)
          }
          numericColumnProfile.minimum.foreach { minimum =>
            columnProfileJson.addProperty("minimum", minimum)
          }
          numericColumnProfile.sum.foreach { sum =>
            columnProfileJson.addProperty("sum", sum)
          }
          numericColumnProfile.stdDev.foreach { stdDev =>
            columnProfileJson.addProperty("stdDev", stdDev)
          }

          // KLL Sketch
          if (numericColumnProfile.kll.isDefined) {
            val kllSketch = numericColumnProfile.kll.get
            val kllSketchJson = new JsonObject()

            val tmp = new JsonArray()
            kllSketch.buckets.foreach{bucket =>
              val entry = new JsonObject()
              entry.addProperty("low_value", bucket.lowValue)
              entry.addProperty("high_value", bucket.highValue)
              entry.addProperty("count", bucket.count)
              tmp.add(entry)
            }

            kllSketchJson.add("buckets", tmp)
            val entry = new JsonObject()
            entry.addProperty("c", kllSketch.parameters(0))
            entry.addProperty("k", kllSketch.parameters(1))
            val store = new JsonObject()
            store.add("parameters", entry)

            val gson = new Gson()
            val dataJson = gson.toJson(kllSketch.data)

            store.addProperty("data", dataJson)

            kllSketchJson.add("sketch", store)
            columnProfileJson.add("kll", kllSketchJson)
          }

          val approxPercentilesJson = new JsonArray()
          numericColumnProfile.approxPercentiles.foreach {
            _.foreach { percentile =>
              approxPercentilesJson.add(new JsonPrimitive(percentile))
            }
          }

          columnProfileJson.add("approxPercentiles", approxPercentilesJson)

        case _ =>
      }

      columns.add(columnProfileJson)
    }

    json.add("columns", columns)

    val gson = new GsonBuilder()
      .setPrettyPrinting()
      .create()

    gson.toJson(json)
  }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBZGVlcXUlM0ElM0Fhd3NsYWJz" repo-name="deequ"><sup>Powered by [Swimm](/)</sup></SwmMeta>
