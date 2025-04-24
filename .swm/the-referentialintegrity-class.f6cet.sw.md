---
title: The ReferentialIntegrity class
---
This document will cover the class <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="24:2:2" line-data="object ReferentialIntegrity extends ComparisonBase {">`ReferentialIntegrity`</SwmToken> in the codebase. We will explain:

1. What <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="24:2:2" line-data="object ReferentialIntegrity extends ComparisonBase {">`ReferentialIntegrity`</SwmToken> is and its purpose.
2. The variables and functions defined in <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="24:2:2" line-data="object ReferentialIntegrity extends ComparisonBase {">`ReferentialIntegrity`</SwmToken>.

# What is <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="24:2:2" line-data="object ReferentialIntegrity extends ComparisonBase {">`ReferentialIntegrity`</SwmToken>

The <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="24:2:2" line-data="object ReferentialIntegrity extends ComparisonBase {">`ReferentialIntegrity`</SwmToken> class is an object that extends <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="24:6:6" line-data="object ReferentialIntegrity extends ComparisonBase {">`ComparisonBase`</SwmToken>. It is used to check the extent to which a set of columns from one <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="27:23:23" line-data="   * Checks to what extent a set of columns from a DataFrame is a subset of another set of columns">`DataFrame`</SwmToken> is a subset of another set of columns from another <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="27:23:23" line-data="   * Checks to what extent a set of columns from a DataFrame is a subset of another set of columns">`DataFrame`</SwmToken>. This utility is experimental and helps in validating the referential integrity between two datasets.

<SwmSnippet path="/src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" line="26">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="48:3:3" line-data="  def subsetCheck(primary: DataFrame,">`subsetCheck`</SwmToken> checks to what extent a set of columns from a <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="27:23:23" line-data="   * Checks to what extent a set of columns from a DataFrame is a subset of another set of columns">`DataFrame`</SwmToken> is a subset of another set of columns from another <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="27:23:23" line-data="   * Checks to what extent a set of columns from a DataFrame is a subset of another set of columns">`DataFrame`</SwmToken>. It takes the primary dataset, primary columns, reference dataset, reference columns, and an assertion function as parameters. It returns a <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="41:6:6" line-data="   * @return ComparisonResult If validation of parameters fails, we return a &quot;ComparisonFailed&quot;.">`ComparisonResult`</SwmToken> based on the validation of parameters and the calculated referential integrity ratio.

```scala
  /**
   * Checks to what extent a set of columns from a DataFrame is a subset of another set of columns
   * from another DataFrame.
   *
   * This is an experimental utility.
   *
   * @param primary           The primary data set which contains the columns which the customer
   *                          will select to do the Referential Integrity check.
   * @param primaryColumns    The names of the columns selected from the primary data set.
   * @param reference         The reference data set which contains the possible values for the columns
   *                          from the primary dataset.
   * @param referenceColumns  The names of the columns selected from the reference data set, which
   *                          contains those values.
   * @param assertion         A function which accepts the match ratio and returns a Boolean.
   *
   * @return ComparisonResult If validation of parameters fails, we return a "ComparisonFailed".
   *                          If validation succeeds, internally we calculate the referential integrity
   *                          as a ratio, and we run the assertion on that outcome.
   *                          That ends up being a true or false response, which translates to
   *                          ComparisonSucceeded or ComparisonFailed respectively.
   *
   */
  def subsetCheck(primary: DataFrame,
                  primaryColumns: Seq[String],
                  reference: DataFrame,
                  referenceColumns: Seq[String],
                  assertion: Double => Boolean): ComparisonResult = {
    val validatedParameters = validateParameters(primary, primaryColumns, reference, referenceColumns)

    if (validatedParameters.isDefined) {
      validatedParameters.get
    } else {
      val primaryCount = primary.count()
      val primarySparkCols = primary.select(primaryColumns.map(col): _*)
      val referenceSparkCols = reference.select(referenceColumns.map(col): _*)
      val mismatchCount = primarySparkCols.except(referenceSparkCols).count()

      val ratio = if (mismatchCount == 0) 1.0 else (primaryCount - mismatchCount).toDouble / primaryCount

      if (assertion(ratio)) {
        ComparisonSucceeded()
      } else {
        ComparisonFailed(s"Value: $ratio does not meet the constraint requirement.")
      }
    }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" line="73">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="94:3:3" line-data="  def subsetCheckRowLevel(primary: DataFrame,">`subsetCheckRowLevel`</SwmToken> annotates a given <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="88:11:11" line-data="   * @return Either[ComparisonFailed, DataFrame] If validation of parameters fails, we return a &quot;ComparisonFailed&quot;.">`DataFrame`</SwmToken> with a column that contains the outcome of whether a provided set of columns exists in another given <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="88:11:11" line-data="   * @return Either[ComparisonFailed, DataFrame] If validation of parameters fails, we return a &quot;ComparisonFailed&quot;.">`DataFrame`</SwmToken>. It takes the primary dataset, primary columns, reference dataset, reference columns, and an optional outcome column name as parameters. It returns either a <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="88:8:8" line-data="   * @return Either[ComparisonFailed, DataFrame] If validation of parameters fails, we return a &quot;ComparisonFailed&quot;.">`ComparisonFailed`</SwmToken> or an annotated <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="88:11:11" line-data="   * @return Either[ComparisonFailed, DataFrame] If validation of parameters fails, we return a &quot;ComparisonFailed&quot;.">`DataFrame`</SwmToken>.

```scala
  /**
   * Annotates a given data frame with a column that contains the outcome of whether a provided set of columns
   * exist in another given data frame.
   *
   * This is an experimental utility.
   *
   * @param primary           The primary data set which contains the columns which the customer
   *                          will select to do the Referential Integrity check.
   * @param primaryColumns    The names of the columns selected from the primary data set.
   * @param reference         The reference data set which contains the possible values for the columns
   *                          from the primary dataset.
   * @param referenceColumns  The names of the columns selected from the reference data set, which
   *                          contains those values.
   * @param outcomeColumnName Name of the column that will contain the outcome results.
   *
   * @return Either[ComparisonFailed, DataFrame] If validation of parameters fails, we return a "ComparisonFailed".
   *                                             If validation succeeds, we annotate the primary data frame with a
   *                                             column that contains either true or false. That value depends on
   *                                             whether the referential integrity check succeeds for that row.
   *
   */
  def subsetCheckRowLevel(primary: DataFrame,
                          primaryColumns: Seq[String],
                          reference: DataFrame,
                          referenceColumns: Seq[String],
                          outcomeColumnName: Option[String] = None): Either[ComparisonFailed, DataFrame] = {
    val validatedParameters = validateParameters(primary, primaryColumns, reference, referenceColumns)

    if (validatedParameters.isDefined) {
      Left(validatedParameters.get)
    } else {
      // The provided columns can be nested, so we first map the column names to values that we can control
      val updatedRefColNamesMap = referenceColumns.zipWithIndex.map {
        case (col, i) => col -> s"${referenceColumnNamePrefix}_$i"
      }.toMap
      // We then add the new column names to the existing data frame
      val referenceWithUpdatedNames = updatedRefColNamesMap.foldLeft(reference) {
        case (accDf, (refColName, updatedRefColName)) => accDf.withColumn(updatedRefColName, accDf(refColName))
      }
      val updatedReferenceColNames = updatedRefColNamesMap.values.toSeq
      // We select the new column names and ensure that there are no duplicates
      val processedRef = referenceWithUpdatedNames.select(updatedReferenceColNames.map(col): _*).distinct()

      // We join on the provided list of columns from primary with the updated column names from reference
      // It will be a left join so that any rows that fail the referential integrity check will have nulls
      val joinClause = primaryColumns
        .zip(updatedReferenceColNames).map { case (colP, colR) => primary(colP) === processedRef(colR) }
        .reduce((e1, e2) => e1 && e2)

      // We cannot keep the new reference columns in the final data frame
      // Before we drop them, we need to calculate a final true/false outcome
      // If all the new columns that are added are not null, the outcome is true, otherwise it is false.
      val condition = updatedReferenceColNames.foldLeft(lit(true)) {
        case (cond, c) => cond && col(c).isNotNull
      }

      val outcomeColumn = outcomeColumnName.getOrElse(defaultOutcomeColumnName)
      Right(
        primary
          .join(processedRef, joinClause, "left")
          .withColumn(outcomeColumn, when(condition, lit(true)).otherwise(lit(false)))
          .drop(updatedReferenceColNames: _*)
      )
    }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" line="139">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="139:5:5" line-data="  private def validateParameters(primary: DataFrame,">`validateParameters`</SwmToken> validates the parameters provided to the <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="48:3:3" line-data="  def subsetCheck(primary: DataFrame,">`subsetCheck`</SwmToken> and <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="94:3:3" line-data="  def subsetCheckRowLevel(primary: DataFrame,">`subsetCheckRowLevel`</SwmToken> functions. It checks if the primary and reference columns are not empty, if their sizes match, and if the columns exist in the respective datasets. It returns an <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="142:11:14" line-data="                                 referenceColumns: Seq[String]): Option[ComparisonFailed] = {">`Option[ComparisonFailed]`</SwmToken> based on the validation results.

```scala
  private def validateParameters(primary: DataFrame,
                                 primaryColumns: Seq[String],
                                 reference: DataFrame,
                                 referenceColumns: Seq[String]): Option[ComparisonFailed] = {
    if (primaryColumns.isEmpty) {
      Some(ComparisonFailed(s"Empty list provided for columns to check from the primary data frame."))
    } else if (referenceColumns.isEmpty) {
      Some(ComparisonFailed(s"Empty list provided for columns to check from the reference data frame."))
    } else if (primaryColumns.size != referenceColumns.size) {
      Some(ComparisonFailed(s"The number of columns to check from the primary data frame" +
        s" must equal the number of columns to check from the reference data frame."))
    } else {
      val primaryColumnsNotInDataset = primaryColumns.filterNot(c => Try(primary(c)).isSuccess)
      val referenceColumnsNotInDataset = referenceColumns.filterNot(c => Try(reference(c)).isSuccess)

      if (primaryColumnsNotInDataset.nonEmpty) {
        primaryColumnsNotInDataset match {
          case Seq(c) => Some(ComparisonFailed(s"Column $c does not exist in primary data frame."))
          case cols => Some(ComparisonFailed(s"Columns ${cols.mkString(", ")} do not exist in primary data frame."))
        }
      } else if (referenceColumnsNotInDataset.nonEmpty) {
        referenceColumnsNotInDataset match {
          case Seq(c) => Some(ComparisonFailed(s"Column $c does not exist in reference data frame."))
          case cols => Some(ComparisonFailed(s"Columns ${cols.mkString(", ")} do not exist in reference data frame."))
        }
      } else if (primary.head(1).isEmpty) {
        Some(ComparisonFailed(s"Primary data frame contains no data."))
      } else None
    }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" line="53">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="53:3:3" line-data="    val validatedParameters = validateParameters(primary, primaryColumns, reference, referenceColumns)">`validatedParameters`</SwmToken> is used to store the result of the <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="53:7:7" line-data="    val validatedParameters = validateParameters(primary, primaryColumns, reference, referenceColumns)">`validateParameters`</SwmToken> function call. It is used within the <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="48:3:3" line-data="  def subsetCheck(primary: DataFrame,">`subsetCheck`</SwmToken> and <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="94:3:3" line-data="  def subsetCheckRowLevel(primary: DataFrame,">`subsetCheckRowLevel`</SwmToken> functions to determine if the parameters are valid before proceeding with the referential integrity check.

```scala
    val validatedParameters = validateParameters(primary, primaryColumns, reference, referenceColumns)

```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" line="58">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="58:3:3" line-data="      val primaryCount = primary.count()">`primaryCount`</SwmToken> stores the count of rows in the primary <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="27:23:23" line-data="   * Checks to what extent a set of columns from a DataFrame is a subset of another set of columns">`DataFrame`</SwmToken>. It is used within the <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="48:3:3" line-data="  def subsetCheck(primary: DataFrame,">`subsetCheck`</SwmToken> function to calculate the referential integrity ratio.

```scala
      val primaryCount = primary.count()
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" line="109">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="109:3:3" line-data="      val referenceWithUpdatedNames = updatedRefColNamesMap.foldLeft(reference) {">`referenceWithUpdatedNames`</SwmToken> is used to store the reference <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="27:23:23" line-data="   * Checks to what extent a set of columns from a DataFrame is a subset of another set of columns">`DataFrame`</SwmToken> with updated column names. It is used within the <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="94:3:3" line-data="  def subsetCheckRowLevel(primary: DataFrame,">`subsetCheckRowLevel`</SwmToken> function to handle nested columns and ensure there are no duplicates.

```scala
      val referenceWithUpdatedNames = updatedRefColNamesMap.foldLeft(reference) {
        case (accDf, (refColName, updatedRefColName)) => accDf.withColumn(updatedRefColName, accDf(refColName))
      }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" line="114">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="114:3:3" line-data="      val processedRef = referenceWithUpdatedNames.select(updatedReferenceColNames.map(col): _*).distinct()">`processedRef`</SwmToken> stores the reference <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="27:23:23" line-data="   * Checks to what extent a set of columns from a DataFrame is a subset of another set of columns">`DataFrame`</SwmToken> with selected and distinct updated column names. It is used within the <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="94:3:3" line-data="  def subsetCheckRowLevel(primary: DataFrame,">`subsetCheckRowLevel`</SwmToken> function to join with the primary <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="27:23:23" line-data="   * Checks to what extent a set of columns from a DataFrame is a subset of another set of columns">`DataFrame`</SwmToken> for the referential integrity check.

```scala
      val processedRef = referenceWithUpdatedNames.select(updatedReferenceColNames.map(col): _*).distinct()
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" line="125">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="125:3:3" line-data="      val condition = updatedReferenceColNames.foldLeft(lit(true)) {">`condition`</SwmToken> is used to calculate the final <SwmToken path="src/main/scala/com/amazon/deequ/comparison/ReferentialIntegrity.scala" pos="123:24:26" line-data="      // Before we drop them, we need to calculate a final true/false outcome">`true/false`</SwmToken> outcome for the referential integrity check. It checks if all the new columns added are not null, indicating a successful referential integrity check.

```scala
      val condition = updatedReferenceColNames.foldLeft(lit(true)) {
        case (cond, c) => cond && col(c).isNotNull
      }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBZGVlcXUlM0ElM0Fhd3NsYWJz" repo-name="deequ"><sup>Powered by [Swimm](/)</sup></SwmMeta>
