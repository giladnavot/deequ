---
title: The KLLRunner class
---
This document will cover the class <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/KLLRunner.scala" pos="87:2:2" line-data="object KLLRunner {">`KLLRunner`</SwmToken> in the <SwmPath>[src/…/runners/KLLRunner.scala](src/main/scala/com/amazon/deequ/analyzers/runners/KLLRunner.scala)</SwmPath> file. We will cover:

1. What <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/KLLRunner.scala" pos="87:2:2" line-data="object KLLRunner {">`KLLRunner`</SwmToken> is and what it is used for.
2. The variables and functions defined in <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/KLLRunner.scala" pos="87:2:2" line-data="object KLLRunner {">`KLLRunner`</SwmToken>.

# What is <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/KLLRunner.scala" pos="87:2:2" line-data="object KLLRunner {">`KLLRunner`</SwmToken>

The <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/KLLRunner.scala" pos="87:2:2" line-data="object KLLRunner {">`KLLRunner`</SwmToken> class is an object in the <SwmPath>[src/…/runners/KLLRunner.scala](src/main/scala/com/amazon/deequ/analyzers/runners/KLLRunner.scala)</SwmPath> file. It is used to compute KLL (Karnin, Lang, and Liberty) sketches for data columns in a <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/KLLRunner.scala" pos="90:4:4" line-data="      data: DataFrame,">`DataFrame`</SwmToken>. This is particularly useful for estimating quantiles and other distribution metrics in large datasets.

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/runners/KLLRunner.scala" line="96">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/KLLRunner.scala" pos="96:3:3" line-data="    val kllAnalyzers = analyzers.map { _.asInstanceOf[KLLSketch] }">`kllAnalyzers`</SwmToken> is used to store the analyzers cast to <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/KLLRunner.scala" pos="96:17:17" line-data="    val kllAnalyzers = analyzers.map { _.asInstanceOf[KLLSketch] }">`KLLSketch`</SwmToken> type.

```scala
    val kllAnalyzers = analyzers.map { _.asInstanceOf[KLLSketch] }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/runners/KLLRunner.scala" line="98">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/KLLRunner.scala" pos="98:3:3" line-data="    val columnsAndParameters = kllAnalyzers">`columnsAndParameters`</SwmToken> maps each column to its corresponding KLL parameters.

```scala
    val columnsAndParameters = kllAnalyzers
      .map { analyzer => (analyzer.column, analyzer.kllParameters) }
        .toMap
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/runners/KLLRunner.scala" line="89">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/KLLRunner.scala" pos="89:3:3" line-data="  def computeKLLSketchesInExtraPass(">`computeKLLSketchesInExtraPass`</SwmToken> computes KLL sketches for the specified analyzers in an extra pass over the data.

```scala
  def computeKLLSketchesInExtraPass(
      data: DataFrame,
      analyzers: Seq[Analyzer[State[_], Metric[_]]],
      aggregateWith: Option[StateLoader] = None,
      saveStatesTo: Option[StatePersister] = None)
    : AnalyzerContext = {

    val kllAnalyzers = analyzers.map { _.asInstanceOf[KLLSketch] }

    val columnsAndParameters = kllAnalyzers
      .map { analyzer => (analyzer.column, analyzer.kllParameters) }
        .toMap

    val sketching = sketchPartitions(columnsAndParameters, data.schema)_

    val sketchPerColumn =
      data.rdd
        .mapPartitions(sketching, preservesPartitioning = true)
        .treeReduce { case (columnAndSketchesA, columnAndSketchesB) =>
            columnAndSketchesA.map { case (column, sketch) =>
              sketch.mergeUntyped(columnAndSketchesB(column))
              column -> sketch
            }
        }

    val metricsByAnalyzer = kllAnalyzers.map { analyzer =>
      val kllState = sketchPerColumn(analyzer.column).asKLLState()
      val metric = analyzer.calculateMetric(Some(kllState), aggregateWith, saveStatesTo)

      analyzer -> metric
    }

    AnalyzerContext(metricsByAnalyzer.toMap[Analyzer[_, Metric[_]], Metric[_]])
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/runners/KLLRunner.scala" line="124">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/KLLRunner.scala" pos="124:8:8" line-data="  private[this] def emptySketches(">`emptySketches`</SwmToken> initializes empty sketches for each column based on its data type and KLL parameters.

```scala
  private[this] def emptySketches(
      columnsAndParameters: Map[String, Option[KLLParameters]],
      schema: StructType): Map[String, UntypedQuantileNonSample] = {

    columnsAndParameters.map { case (column, parameters) =>

      val (sketchSize, shrinkingFactor) = parameters match {
        case Some(kllParameters) => (kllParameters.sketchSize, kllParameters.shrinkingFactor)
        case _ => (KLLSketch.DEFAULT_SKETCH_SIZE, KLLSketch.DEFAULT_SHRINKING_FACTOR)
      }

      val sketch: UntypedQuantileNonSample = schema(column).dataType match {
        case DoubleType => new DoubleQuantileNonSample(sketchSize, shrinkingFactor)
        case FloatType => new FloatQuantileNonSample(sketchSize, shrinkingFactor)
        case ByteType => new ByteQuantileNonSample(sketchSize, shrinkingFactor)
        case ShortType => new ShortQuantileNonSample(sketchSize, shrinkingFactor)
        case IntegerType => new IntQuantileNonSample(sketchSize, shrinkingFactor)
        case LongType => new LongQuantileNonSample(sketchSize, shrinkingFactor)
        // TODO at the moment, we will throw exceptions for Decimals
        case _ => throw new IllegalArgumentException(s"Cannot handle ${schema(column).dataType}")
      }

      column -> sketch
    }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/runners/KLLRunner.scala" line="150">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/KLLRunner.scala" pos="150:8:8" line-data="  private[this] def sketchPartitions(">`sketchPartitions`</SwmToken> processes partitions of rows to update the sketches for each column.

```scala
  private[this] def sketchPartitions(
      columnsAndParameters: Map[String, Option[KLLParameters]],
      schema: StructType)(rows: Iterator[Row])
    : Iterator[Map[String, UntypedQuantileNonSample]] = {

    val columnsAndSketches = emptySketches(columnsAndParameters, schema)

    val namesToIndexes = schema.fields
      .map { _.name }
      .zipWithIndex
      .toMap

    // Include the index to avoid a lookup per row
    val indexesAndSketches = columnsAndSketches.map { case (column, sketch) =>
      (namesToIndexes(column), sketch )
    }

    while (rows.hasNext) {
      val row = rows.next()
      indexesAndSketches.foreach { case (index, sketch) =>
        if (!row.isNullAt(index)) {
          sketch.updateUntyped(row.get(index))
        }
      }
    }

    Iterator.single(columnsAndSketches)
  }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBZGVlcXUlM0ElM0Fhd3NsYWJz" repo-name="deequ"><sup>Powered by [Swimm](/)</sup></SwmMeta>
