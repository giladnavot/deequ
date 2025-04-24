---
title: The Check class
---
This document will cover the class <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="98:12:12" line-data="  def addConstraint(constraint: Constraint): Check = {">`Check`</SwmToken> in the codebase. We will explain:

1. What <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="98:12:12" line-data="  def addConstraint(constraint: Constraint): Check = {">`Check`</SwmToken> is and what it is used for.
2. The variables and functions defined in <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="98:12:12" line-data="  def addConstraint(constraint: Constraint): Check = {">`Check`</SwmToken>.

# What is Check

The <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="98:12:12" line-data="  def addConstraint(constraint: Constraint): Check = {">`Check`</SwmToken> class in <SwmPath>[src/…/checks/Check.scala](src/main/scala/com/amazon/deequ/checks/Check.scala)</SwmPath> represents a list of constraints that can be applied to a given <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="481:8:8" line-data="  def doesDatasetMatch(otherDataset: DataFrame,">`DataFrame`</SwmToken>. It is used to define and run data quality checks on datasets. The <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="98:12:12" line-data="  def addConstraint(constraint: Constraint): Check = {">`Check`</SwmToken> class allows users to specify various constraints and assertions on data columns, ensuring that the data meets certain quality standards.

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/Check.scala" line="75">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="75:1:1" line-data="  level: CheckLevel.Value,">`level`</SwmToken> is used to store the assertion level of the check group. It determines the severity of the check, such as Error or Warning.

```scala
  level: CheckLevel.Value,
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/Check.scala" line="76">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="76:1:1" line-data="  description: String,">`description`</SwmToken> is used to store the name or description of the check block. This is generally used to show in the logs.

```scala
  description: String,
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/Check.scala" line="77">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="77:8:8" line-data="  private[deequ] val constraints: Seq[Constraint] = Seq.empty) {">`constraints`</SwmToken> is a sequence of constraints to apply when the check is run. New constraints can be added, and it will return a new <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="98:12:12" line-data="  def addConstraint(constraint: Constraint): Check = {">`Check`</SwmToken> object.

```scala
  private[deequ] val constraints: Seq[Constraint] = Seq.empty) {
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/Check.scala" line="83">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="83:3:3" line-data="  def getRowLevelConstraintColumnNames(): Seq[String] = {">`getRowLevelConstraintColumnNames`</SwmToken> returns the names of the columns where each constraint puts <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="80:23:25" line-data="   * Returns the name of the columns where each Constraint puts row-level results, if any">`row-level`</SwmToken> results, if any.

```scala
  def getRowLevelConstraintColumnNames(): Seq[String] = {
    constraints.flatMap(c => {
      c match {
        case c: RowLevelConstraint => Some(c.getColumnName)
        case _ => None
      }
    })
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/Check.scala" line="98">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="98:3:3" line-data="  def addConstraint(constraint: Constraint): Check = {">`addConstraint`</SwmToken> returns a new <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="98:12:12" line-data="  def addConstraint(constraint: Constraint): Check = {">`Check`</SwmToken> object with the given constraint added to the constraints list.

```scala
  def addConstraint(constraint: Constraint): Check = {
    Check(level, description, constraints :+ constraint)
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/Check.scala" line="103">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="103:8:8" line-data="  private[this] def addFilterableConstraint(">`addFilterableConstraint`</SwmToken> adds a constraint that can subsequently be replaced with a filtered version.

```scala
  private[this] def addFilterableConstraint(
      creationFunc: Option[String] => Constraint)
    : CheckWithLastConstraintFilterable = {

    val constraintWithoutFiltering = creationFunc(None)

    CheckWithLastConstraintFilterable(level, description,
      constraints :+ constraintWithoutFiltering, creationFunc)
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/Check.scala" line="124">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="124:3:3" line-data="  def hasSize(assertion: Long =&gt; Boolean, hint: Option[String] = None)">`hasSize`</SwmToken> creates a constraint that calculates the data frame size and runs the assertion on it.

```scala
  def hasSize(assertion: Long => Boolean, hint: Option[String] = None)
    : CheckWithLastConstraintFilterable = {

    addFilterableConstraint { filter => Constraint.sizeConstraint(assertion, filter, hint) }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/Check.scala" line="130">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="130:3:3" line-data="  def hasColumnCount(assertion: Long =&gt; Boolean, hint: Option[String] = None)">`hasColumnCount`</SwmToken> creates a constraint that asserts on the number of columns in the data frame.

```scala
  def hasColumnCount(assertion: Long => Boolean, hint: Option[String] = None)
  : CheckWithLastConstraintFilterable = {
    addFilterableConstraint {
      filter => Constraint.columnCountConstraint(assertion, hint)
    }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/Check.scala" line="145">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="145:3:3" line-data="  def isComplete(column: String, hint: Option[String] = None,">`isComplete`</SwmToken> creates a constraint that asserts on a column completion.

```scala
  def isComplete(column: String, hint: Option[String] = None,
                 analyzerOptions: Option[AnalyzerOptions] = None): CheckWithLastConstraintFilterable = {
    addFilterableConstraint { filter => completenessConstraint(column, Check.IsOne, filter, hint, analyzerOptions) }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/Check.scala" line="161">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="161:3:3" line-data="  def hasCompleteness(">`hasCompleteness`</SwmToken> creates a constraint that asserts on a column's completeness using a given assertion function.

```scala
  def hasCompleteness(
      column: String,
      assertion: Double => Boolean,
      hint: Option[String] = None,
      analyzerOptions: Option[AnalyzerOptions] = None)
    : CheckWithLastConstraintFilterable = {
    addFilterableConstraint { filter => completenessConstraint(column, assertion, filter, hint, analyzerOptions) }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/Check.scala" line="177">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="177:3:3" line-data="  def areComplete(">`areComplete`</SwmToken> creates a constraint that asserts on the completeness of a combined set of columns.

```scala
  def areComplete(
      columns: Seq[String],
      hint: Option[String] = None)
    : CheckWithLastConstraintFilterable = {
    satisfies(isEachNotNull(columns), "Combined Completeness", Check.IsOne, hint, columns = columns.toList)
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/Check.scala" line="192">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="192:3:3" line-data="  def haveCompleteness(">`haveCompleteness`</SwmToken> creates a constraint that asserts on the completeness of a combined set of columns using a given assertion function.

```scala
  def haveCompleteness(
      columns: Seq[String],
      assertion: Double => Boolean,
      hint: Option[String] = None)
    : CheckWithLastConstraintFilterable = {
    satisfies(isEachNotNull(columns), "Combined Completeness", assertion, hint, columns = columns.toList)
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/Check.scala" line="207">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="207:3:3" line-data="  def areAnyComplete(">`areAnyComplete`</SwmToken> creates a constraint that asserts on the completeness of any column in a combined set of columns.

```scala
  def areAnyComplete(
      columns: Seq[String],
      hint: Option[String] = None)
  : CheckWithLastConstraintFilterable = {
    satisfies(isAnyNotNull(columns), "Any Completeness", Check.IsOne, hint, columns = columns.toList)
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/Check.scala" line="222">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="222:3:3" line-data="  def haveAnyCompleteness(">`haveAnyCompleteness`</SwmToken> creates a constraint that asserts on the completeness of any column in a combined set of columns using a given assertion function.

```scala
  def haveAnyCompleteness(
      columns: Seq[String],
      assertion: Double => Boolean,
      hint: Option[String] = None)
  : CheckWithLastConstraintFilterable = {
    satisfies(isAnyNotNull(columns), "Any Completeness", assertion, hint, columns = columns.toList)
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/Check.scala" line="238">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="238:3:3" line-data="  def isUnique(column: String, hint: Option[String] = None,">`isUnique`</SwmToken> creates a constraint that asserts on a column's uniqueness.

```scala
  def isUnique(column: String, hint: Option[String] = None,
               analyzerOptions: Option[AnalyzerOptions] = None): CheckWithLastConstraintFilterable = {
    addFilterableConstraint { filter =>
      uniquenessConstraint(Seq(column), Check.IsOne, filter, hint, analyzerOptions) }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/Check.scala" line="252">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="252:3:3" line-data="  def areUnique(columns: Seq[String], hint: Option[String] = None,">`areUnique`</SwmToken> creates a constraint that asserts on the uniqueness of a combined set of columns.

```scala
  def areUnique(columns: Seq[String], hint: Option[String] = None,
               analyzerOptions: Option[AnalyzerOptions] = None): CheckWithLastConstraintFilterable = {
    addFilterableConstraint { filter =>
      uniquenessConstraint(columns, Check.IsOne, filter, hint, analyzerOptions) }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/Check.scala" line="266">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="266:3:3" line-data="  def isPrimaryKey(column: String, columns: String*): CheckWithLastConstraintFilterable = {">`isPrimaryKey`</SwmToken> creates a constraint that asserts on a column's primary key characteristics.

```scala
  def isPrimaryKey(column: String, columns: String*): CheckWithLastConstraintFilterable = {
    addFilterableConstraint { filter =>
      uniquenessConstraint(column :: columns.toList, Check.IsOne, filter) }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/Check.scala" line="311">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="311:3:3" line-data="  def hasUniqueness(columns: Seq[String], assertion: Double =&gt; Boolean)">`hasUniqueness`</SwmToken> creates a constraint that asserts on the uniqueness of a single or combined set of key columns.

```scala
  def hasUniqueness(columns: Seq[String], assertion: Double => Boolean)
    : CheckWithLastConstraintFilterable = {
    addFilterableConstraint { filter => uniquenessConstraint(columns, assertion, filter) }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/Check.scala" line="406">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="406:3:3" line-data="  def hasDistinctness(">`hasDistinctness`</SwmToken> creates a constraint on the distinctness in a single or combined set of key columns.

```scala
  def hasDistinctness(
      columns: Seq[String], assertion: Double => Boolean,
      hint: Option[String] = None)
    : CheckWithLastConstraintFilterable = {

    addFilterableConstraint { filter => distinctnessConstraint(columns, assertion, filter, hint) }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/Check.scala" line="424">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="424:3:3" line-data="  def hasUniqueValueRatio(">`hasUniqueValueRatio`</SwmToken> creates a constraint on the unique value ratio in a single or combined set of key columns.

```scala
  def hasUniqueValueRatio(
      columns: Seq[String],
      assertion: Double => Boolean,
      hint: Option[String] = None,
      analyzerOptions: Option[AnalyzerOptions] = None)
    : CheckWithLastConstraintFilterable = {

    addFilterableConstraint { filter =>
      uniqueValueRatioConstraint(columns, assertion, filter, hint, analyzerOptions) }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/Check.scala" line="481">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="481:3:3" line-data="  def doesDatasetMatch(otherDataset: DataFrame,">`doesDatasetMatch`</SwmToken> performs a dataset check between the base <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="481:8:8" line-data="  def doesDatasetMatch(otherDataset: DataFrame,">`DataFrame`</SwmToken> and another <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="481:8:8" line-data="  def doesDatasetMatch(otherDataset: DataFrame,">`DataFrame`</SwmToken> using Deequ's column match framework.

```scala
  def doesDatasetMatch(otherDataset: DataFrame,
                       keyColumnMappings: Map[String, String],
                       assertion: Double => Boolean,
                       matchColumnMappings: Option[Map[String, String]] = None,
                       hint: Option[String] = None): Check = {
    val dataMatchAnalyzer = DatasetMatchAnalyzer(otherDataset, keyColumnMappings, assertion, matchColumnMappings)
    val constraint = AnalysisBasedConstraint[DatasetMatchState, Double, Double](dataMatchAnalyzer, assertion,
      hint = hint)
    addConstraint(constraint)
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/Check.scala" line="503">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="503:3:3" line-data="  def hasNumberOfDistinctValues(">`hasNumberOfDistinctValues`</SwmToken> creates a constraint that asserts on the number of distinct values a column has.

```scala
  def hasNumberOfDistinctValues(
      column: String,
      assertion: Long => Boolean,
      binningUdf: Option[UserDefinedFunction] = None,
      maxBins: Integer = Histogram.MaximumAllowedDetailBins,
      hint: Option[String] = None)
    : CheckWithLastConstraintFilterable = {

    addFilterableConstraint { filter =>
      histogramBinConstraint(column, assertion, binningUdf, maxBins, filter, hint, computeFrequenciesAsRatio = false) }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/Check.scala" line="530">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="530:3:3" line-data="  def hasHistogramValues(">`hasHistogramValues`</SwmToken> creates a constraint that asserts on a column's value distribution.

```scala
  def hasHistogramValues(
      column: String,
      assertion: Distribution => Boolean,
      binningUdf: Option[UserDefinedFunction] = None,
      maxBins: Integer = Histogram.MaximumAllowedDetailBins,
      hint: Option[String] = None)
    : CheckWithLastConstraintFilterable = {

    addFilterableConstraint { filter =>
      histogramConstraint(column, assertion, binningUdf, maxBins, filter, hint) }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/Check.scala" line="555">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/checks/Check.scala" pos="554:3:3" line-data="  def kllSketchSatisfies(">`kllSketchSatisfies`</SwmToken> creates a constraint that asserts on a column's sketch size.

```scala
                          column: String,
                          assertion: BucketDistribution => Boolean,
                          kllParameters: Option[KLLParameters] = None,
                          hint: Option[String] = None)
    : Check = {

    addConstraint(kllConstraint(column, assertion, kllParameters, hint))
  }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBZGVlcXUlM0ElM0Fhd3NsYWJz" repo-name="deequ"><sup>Powered by [Swimm](/)</sup></SwmMeta>
