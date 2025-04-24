---
title: The DataSynchronization class
---
This document will cover the class <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="57:3:3" line-data=" * DataSynchronization.columnMatch(">`DataSynchronization`</SwmToken> in the file <SwmPath>[src/…/comparison/DataSynchronization.scala](src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala)</SwmPath>. We will cover:

1. What is <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="57:3:3" line-data=" * DataSynchronization.columnMatch(">`DataSynchronization`</SwmToken>
2. Variables and functions

## What is <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="57:3:3" line-data=" * DataSynchronization.columnMatch(">`DataSynchronization`</SwmToken>

The <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="57:3:3" line-data=" * DataSynchronization.columnMatch(">`DataSynchronization`</SwmToken> class is an object that extends <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="79:6:6" line-data="object DataSynchronization extends ComparisonBase {">`ComparisonBase`</SwmToken> and is used to compare two <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="31:7:7" line-data=" * Compare two DataFrames 1 to 1 with specific columns inputted by the customer.">`DataFrames`</SwmToken> based on specific columns provided by the user. It is an experimental utility designed to help users compare datasets and ensure data quality by checking for column matches and validating key columns.

<SwmSnippet path="/src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" line="80">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="94:3:3" line-data="  def columnMatch(ds1: DataFrame,">`columnMatch`</SwmToken> is used to compare two <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="31:7:7" line-data=" * Compare two DataFrames 1 to 1 with specific columns inputted by the customer.">`DataFrames`</SwmToken> based on a map of key columns and an assertion function. It checks if the key columns are valid and if the <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="100:9:11" line-data="      // Get all the non-key columns from DS1 and verify that they are present in DS2">`non-key`</SwmToken> columns match between the two <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="31:7:7" line-data=" * Compare two DataFrames 1 to 1 with specific columns inputted by the customer.">`DataFrames`</SwmToken>.

```scala
  /**
   * This will evaluate to false. The city column will match, but the state column will not.
   *
   * @param ds1                The first data set which the customer will select for comparison.
   * @param ds2                The second data set which the customer will select for comparison.
   * @param colKeyMap          A map of columns to columns used for joining the two datasets.
   *                           The keys in the map are composite key forming columns from the first dataset.
   *                           The values for each key is the equivalent column from the second dataset.
   * @param assertion          A function which accepts the match ratio and returns a Boolean.
   * @return ComparisonResult  An appropriate subtype of ComparisonResult is returned.
   *                           Once all preconditions are met, we calculate the ratio of the rows
   *                           that match and we run the assertion on that outcome.
   *                           The response is then converted to ComparisonResult.
   */
  def columnMatch(ds1: DataFrame,
                  ds2: DataFrame,
                  colKeyMap: Map[String, String],
                  assertion: Double => Boolean): ComparisonResult = {
    val columnErrors = areKeyColumnsValid(ds1, ds2, colKeyMap)
    if (columnErrors.isEmpty) {
      // Get all the non-key columns from DS1 and verify that they are present in DS2
      val colsDS1 = ds1.columns.filterNot(x => colKeyMap.keys.toSeq.contains(x)).sorted
      val nonKeyColsMatch = colsDS1.forall(columnExists(ds2, _))

      if (!nonKeyColsMatch) {
        DatasetMatchFailed("Non key columns in the given data frames do not match.")
      } else {
        val mergedMaps = colKeyMap ++ colsDS1.map(x => x -> x).toMap
        finalAssertion(ds1, ds2, mergedMaps, assertion)
      }
    } else {
      DatasetMatchFailed(columnErrors.get)
    }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" line="115">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="130:3:3" line-data="  def columnMatch(ds1: DataFrame,">`columnMatch`</SwmToken> with additional parameters <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="123:6:6" line-data="   * @param compCols           A map of columns to columns which we will check for equality, post joining.">`compCols`</SwmToken> is used to compare two <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="31:7:7" line-data=" * Compare two DataFrames 1 to 1 with specific columns inputted by the customer.">`DataFrames`</SwmToken> based on both key columns and comparison columns. It ensures that the specified columns exist in both <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="31:7:7" line-data=" * Compare two DataFrames 1 to 1 with specific columns inputted by the customer.">`DataFrames`</SwmToken> and then performs the comparison.

```scala
  /**
   * This will evaluate to false. The city column will match, but the state column will not.
   *
   * @param ds1                The first data set which the customer will select for comparison.
   * @param ds2                The second data set which the customer will select for comparison.
   * @param colKeyMap          A map of columns to columns used for joining the two datasets.
   *                           The keys in the map are composite key forming columns from the first dataset.
   *                           The values for each key is the equivalent column from the second dataset.
   * @param compCols           A map of columns to columns which we will check for equality, post joining.
   * @param assertion          A function which accepts the match ratio and returns a Boolean.
   * @return ComparisonResult  An appropriate subtype of ComparisonResult is returned.
   *                           Once all preconditions are met, we calculate the ratio of the rows
   *                           that match and we run the assertion on that outcome.
   *                           The response is then converted to ComparisonResult.
   */
  def columnMatch(ds1: DataFrame,
                  ds2: DataFrame,
                  colKeyMap: Map[String, String],
                  compCols: Map[String, String],
                  assertion: Double => Boolean): ComparisonResult = {
    val keyColumnErrors = areKeyColumnsValid(ds1, ds2, colKeyMap)
    if (keyColumnErrors.isEmpty) {
      val nonKeyColumns1NotInDataset = compCols.keys.filterNot(columnExists(ds1, _))
      val nonKeyColumns2NotInDataset = compCols.values.filterNot(columnExists(ds2, _))

      if (nonKeyColumns1NotInDataset.nonEmpty) {
        DatasetMatchFailed(s"The following columns were not found in the first dataset: " +
          s"${nonKeyColumns1NotInDataset.mkString(", ")}")
      } else if (nonKeyColumns2NotInDataset.nonEmpty) {
        DatasetMatchFailed(s"The following columns were not found in the second dataset: " +
          s"${nonKeyColumns2NotInDataset.mkString(", ")}")
      } else {
        val mergedMaps = colKeyMap ++ compCols
        finalAssertion(ds1, ds2, mergedMaps, assertion)
      }
    } else {
      DatasetMatchFailed(keyColumnErrors.get)
    }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" line="155">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="155:3:3" line-data="  def columnMatchRowLevel(ds1: DataFrame,">`columnMatchRowLevel`</SwmToken> is used to compare two <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="31:7:7" line-data=" * Compare two DataFrames 1 to 1 with specific columns inputted by the customer.">`DataFrames`</SwmToken> at the row level based on key columns and optional comparison columns. It returns either a <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="160:3:3" line-data="  Either[DatasetMatchFailed, DataFrame] = {">`DatasetMatchFailed`</SwmToken> or a <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="155:8:8" line-data="  def columnMatchRowLevel(ds1: DataFrame,">`DataFrame`</SwmToken> with the comparison results.

```scala
  def columnMatchRowLevel(ds1: DataFrame,
                          ds2: DataFrame,
                          colKeyMap: Map[String, String],
                          optionalCompCols: Option[Map[String, String]] = None,
                          optionalOutcomeColumnName: Option[String] = None):
  Either[DatasetMatchFailed, DataFrame] = {
    val columnErrors = areKeyColumnsValid(ds1, ds2, colKeyMap)
    if (columnErrors.isEmpty) {
      val compColsEither: Either[DatasetMatchFailed, Map[String, String]] = if (optionalCompCols.isDefined) {
        optionalCompCols.get match {
          case compCols if compCols.isEmpty => Left(DatasetMatchFailed("Empty column comparison map provided."))
          case compCols =>
            val ds1CompColsNotInDataset = compCols.keys.filterNot(columnExists(ds1, _))
            val ds2CompColsNotInDataset = compCols.values.filterNot(columnExists(ds2, _))
            if (ds1CompColsNotInDataset.nonEmpty) {
              Left(
                DatasetMatchFailed(s"The following columns were not found in the first dataset: " +
                  s"${ds1CompColsNotInDataset.mkString(", ")}")
              )
            } else if (ds2CompColsNotInDataset.nonEmpty) {
              Left(
                DatasetMatchFailed(s"The following columns were not found in the second dataset: " +
                  s"${ds2CompColsNotInDataset.mkString(", ")}")
              )
            } else {
              Right(compCols)
            }
        }
      } else {
        // Get all the non-key columns from DS1 and verify that they are present in DS2
        val ds1NonKeyCols = ds1.columns.filterNot(x => colKeyMap.keys.toSeq.contains(x)).sorted
        val nonKeyColsMatch = ds1NonKeyCols.forall(columnExists(ds2, _))

        if (!nonKeyColsMatch) {
          Left(DatasetMatchFailed("Non key columns in the given data frames do not match."))
        } else {
          Right(ds1NonKeyCols.map { c => c -> c}.toMap)
        }
      }

      compColsEither.flatMap { compCols =>
        val outcomeColumn = optionalOutcomeColumnName.getOrElse(defaultOutcomeColumnName)
        Try { columnMatchRowLevelInner(ds1, ds2, colKeyMap, compCols, outcomeColumn) } match {
          case Success(df) => Right(df)
          case Failure(ex) =>
            ex.printStackTrace()
            Left(DatasetMatchFailed(s"Comparison failed due to ${ex.getCause.getClass}"))
        }
      }
    } else {
      Left(DatasetMatchFailed(columnErrors.get))
    }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" line="209">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="209:5:5" line-data="  private def areKeyColumnsValid(ds1: DataFrame,">`areKeyColumnsValid`</SwmToken> checks if the key columns provided for comparison exist in both <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="31:7:7" line-data=" * Compare two DataFrames 1 to 1 with specific columns inputted by the customer.">`DataFrames`</SwmToken> and if they form a valid <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="223:23:25" line-data="      // We verify that the key columns provided form a valid primary/composite key.">`primary/composite`</SwmToken> key. It returns an optional error message if the key columns are not valid.

```scala
  private def areKeyColumnsValid(ds1: DataFrame,
                                 ds2: DataFrame,
                                 colKeyMap: Map[String, String]): Option[String] = {
    val ds1Cols = colKeyMap.keys.toSeq
    val ds2Cols = colKeyMap.values.toSeq

    val ds1ColsNotInDataset = ds1Cols.filterNot(columnExists(ds1, _))
    val ds2ColsNotInDataset = ds2Cols.filterNot(columnExists(ds2, _))

    if (ds1ColsNotInDataset.nonEmpty) {
      Some(s"The following key columns were not found in the first dataset: ${ds1ColsNotInDataset.mkString(", ")}")
    } else if (ds2ColsNotInDataset.nonEmpty) {
      Some(s"The following key columns were not found in the second dataset: ${ds2ColsNotInDataset.mkString(", ")}")
    } else {
      // We verify that the key columns provided form a valid primary/composite key.
      // To achieve this, we group the dataframes and compare their count with the original count.
      // If the key columns provided are valid, then the two counts should match.
      val ds1Unique = ds1.groupBy(ds1Cols.map(col): _*).count()
      val ds2Unique = ds2.groupBy(ds2Cols.map(col): _*).count()

      val ds1Count = ds1.count()
      val ds2Count = ds2.count()
      val ds1UniqueCount = ds1Unique.count()
      val ds2UniqueCount = ds2Unique.count()

      if (ds1UniqueCount == ds1Count && ds2UniqueCount == ds2Count) {
        None
      } else {
        val combo1 = ds1Cols.mkString(", ")
        val combo2 = ds2Cols.mkString(", ")
        Some(s"The selected columns are not comparable due to duplicates present in the dataset." +
          s"Comparison keys must be unique, but " +
          s"in Dataframe 1, there are $ds1UniqueCount unique records and $ds1Count rows," +
          s" and " +
          s"in Dataframe 2, there are $ds2UniqueCount unique records and $ds2Count rows, " +
          s"based on the combination of keys {$combo1} in Dataframe 1 and {$combo2} in Dataframe 2")
      }
    }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" line="249">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="249:5:5" line-data="  private def finalAssertion(ds1: DataFrame,">`finalAssertion`</SwmToken> performs the final comparison between the two <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="31:7:7" line-data=" * Compare two DataFrames 1 to 1 with specific columns inputted by the customer.">`DataFrames`</SwmToken> based on the merged key and comparison columns. It calculates the match ratio and applies the assertion function to determine if the comparison is successful.

```scala
  private def finalAssertion(ds1: DataFrame,
                             ds2: DataFrame,
                             mergedMaps: Map[String, String],
                             assertion: Double => Boolean): ComparisonResult = {

    val ds1Count = ds1.count()
    val ds2Count = ds2.count()

    if (ds1Count != ds2Count) {
      DatasetMatchFailed(s"The row counts of the two data frames do not match.")
    } else {
      val joinExpression: Column = mergedMaps
        .map { case (col1, col2) => ds1(col1) === ds2(col2)}
        .reduce((e1, e2) => e1 && e2)

      val joined = ds1.join(ds2, joinExpression, "inner")
      val passedCount = joined.count()
      val totalCount = ds1Count
      val ratio = passedCount.toDouble / totalCount.toDouble

      if (assertion(ratio)) {
        DatasetMatchSucceeded(passedCount, totalCount)
      } else {
        DatasetMatchFailed(s"Data Synchronization Comparison Metric Value: $ratio does not meet the constraint" +
          s"requirement.", Some(passedCount), Some(totalCount))
      }
    }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" line="278">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="278:5:5" line-data="  private def columnMatchRowLevelInner(ds1: DataFrame,">`columnMatchRowLevelInner`</SwmToken> is an internal function used by <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="155:3:3" line-data="  def columnMatchRowLevel(ds1: DataFrame,">`columnMatchRowLevel`</SwmToken> to perform the row-level comparison. It joins the two <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="31:7:7" line-data=" * Compare two DataFrames 1 to 1 with specific columns inputted by the customer.">`DataFrames`</SwmToken> based on the key columns and compares the hash of the <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="100:9:11" line-data="      // Get all the non-key columns from DS1 and verify that they are present in DS2">`non-key`</SwmToken> columns.

```scala
  private def columnMatchRowLevelInner(ds1: DataFrame,
                                       ds2: DataFrame,
                                       colKeyMap: Map[String, String],
                                       compCols: Map[String, String],
                                       outcomeColumnName: String): DataFrame = {
    // We sort in case .keys / .values do not return the elements in the same order
    val ds1KeyCols = colKeyMap.keys.toSeq.sorted
    val ds2KeyCols = colKeyMap.values.toSeq.sorted

    val ds1HashColName = java.util.UUID.randomUUID().toString
    val ds2HashColName = java.util.UUID.randomUUID().toString

    // The hashing allows us to check the equality without having to check cell by cell
    val ds1HashCol = hash(compCols.keys.toSeq.sorted.map(col): _*)
    val ds2HashCol = hash(compCols.values.toSeq.sorted.map(col): _*)

    // We need to update the names of the columns in ds2 so that they do not clash with ds1 when we join
    val ds2KeyColsUpdatedNamesMap = ds2KeyCols.zipWithIndex.map {
      case (col, i) => col -> s"${referenceColumnNamePrefix}_$i"
    }.toMap

    val ds1WithHashCol = ds1.withColumn(ds1HashColName, ds1HashCol)

    val ds2ReductionSeed = ds2
      .withColumn(ds2HashColName, ds2HashCol)
      .select((ds2KeyColsUpdatedNamesMap.keys.toSeq :+ ds2HashColName).map(col): _*)

    val ds2Reduced = ds2KeyColsUpdatedNamesMap.foldLeft(ds2ReductionSeed) {
      case (accumulatedDF, (origCol, updatedCol)) =>
        accumulatedDF.withColumn(updatedCol, col(origCol)).drop(origCol)
    }

    val joinExpression: Column = ds1KeyCols
      .map { ds1KeyCol => ds1KeyCol -> ds2KeyColsUpdatedNamesMap(colKeyMap(ds1KeyCol)) }
      .map { case (ds1Col, ds2ReducedCol) => ds1WithHashCol(ds1Col) === ds2Reduced(ds2ReducedCol) }
      .reduce((e1, e2) => e1 && e2) && ds1WithHashCol(ds1HashColName) === ds2Reduced(ds2HashColName)

    // After joining, we will have:
    //  - All the columns from ds1
    //  - A column containing the hash of the non key columns from ds1
    //  - All the key columns from ds2, with updated column names
    //  - A column containing the hash of the non key columns from ds2
    val joined = ds1WithHashCol.join(ds2Reduced, joinExpression, "left")

    // In order for rows to match,
    // - The key columns from ds2 must be in the joined dataframe, i.e. not null
    // - The hash column of the non key cols from ds2 must be in the joined dataframe, i.e. not null
    val filterExpression = ds2KeyColsUpdatedNamesMap.values
      .map { ds2ReducedCol => col(ds2ReducedCol).isNotNull }
      .reduce((e1, e2) => e1 && e2)

    joined
      .withColumn(outcomeColumnName, when(filterExpression, lit(true)).otherwise(lit(false)))
      .drop(ds1HashColName)
      .drop(ds2HashColName)
      .drop(ds2KeyColsUpdatedNamesMap.values.toSeq: _*)
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" line="336">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="336:5:5" line-data="  private def columnExists(df: DataFrame, col: String) = Try { df(col) }.isSuccess">`columnExists`</SwmToken> checks if a specified column exists in a <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="336:10:10" line-data="  private def columnExists(df: DataFrame, col: String) = Try { df(col) }.isSuccess">`DataFrame`</SwmToken>. It is used by other functions to validate the presence of columns in the <SwmToken path="src/main/scala/com/amazon/deequ/comparison/DataSynchronization.scala" pos="31:7:7" line-data=" * Compare two DataFrames 1 to 1 with specific columns inputted by the customer.">`DataFrames`</SwmToken>.

```scala
  private def columnExists(df: DataFrame, col: String) = Try { df(col) }.isSuccess
}
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBZGVlcXUlM0ElM0Fhd3NsYWJz" repo-name="deequ"><sup>Powered by [Swimm](/)</sup></SwmMeta>
