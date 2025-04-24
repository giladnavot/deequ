---
title: The DQDLExecutor class
---
This document will cover the class <SwmToken path="src/main/scala/com/amazon/deequ/dqdl/execution/DQDLExecutor.scala" pos="26:2:2" line-data="object DQDLExecutor {">`DQDLExecutor`</SwmToken> in detail. We will explain:

1. What <SwmToken path="src/main/scala/com/amazon/deequ/dqdl/execution/DQDLExecutor.scala" pos="26:2:2" line-data="object DQDLExecutor {">`DQDLExecutor`</SwmToken> is.
2. The variables and functions defined in <SwmToken path="src/main/scala/com/amazon/deequ/dqdl/execution/DQDLExecutor.scala" pos="26:2:2" line-data="object DQDLExecutor {">`DQDLExecutor`</SwmToken>.

# What is <SwmToken path="src/main/scala/com/amazon/deequ/dqdl/execution/DQDLExecutor.scala" pos="26:2:2" line-data="object DQDLExecutor {">`DQDLExecutor`</SwmToken>

The <SwmToken path="src/main/scala/com/amazon/deequ/dqdl/execution/DQDLExecutor.scala" pos="26:2:2" line-data="object DQDLExecutor {">`DQDLExecutor`</SwmToken> class is a part of the <SwmToken path="src/main/scala/com/amazon/deequ/dqdl/execution/DQDLExecutor.scala" pos="17:6:6" line-data="package com.amazon.deequ.dqdl.execution">`deequ`</SwmToken> library, located in <SwmPath>[src/…/execution/DQDLExecutor.scala](src/main/scala/com/amazon/deequ/dqdl/execution/DQDLExecutor.scala)</SwmPath>. It is used to execute a sequence of Deequ checks on a Spark <SwmToken path="src/main/scala/com/amazon/deequ/dqdl/execution/DQDLExecutor.scala" pos="27:8:8" line-data="  def executeRules(df: DataFrame, checks: Seq[Check]): VerificationResult = {">`DataFrame`</SwmToken>. This class provides a method to run data quality checks defined by the user on a given <SwmToken path="src/main/scala/com/amazon/deequ/dqdl/execution/DQDLExecutor.scala" pos="27:8:8" line-data="  def executeRules(df: DataFrame, checks: Seq[Check]): VerificationResult = {">`DataFrame`</SwmToken>, ensuring that the data meets specified quality standards.

<SwmSnippet path="/src/main/scala/com/amazon/deequ/dqdl/execution/DQDLExecutor.scala" line="27">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/dqdl/execution/DQDLExecutor.scala" pos="27:3:3" line-data="  def executeRules(df: DataFrame, checks: Seq[Check]): VerificationResult = {">`executeRules`</SwmToken> is the primary function in the <SwmToken path="src/main/scala/com/amazon/deequ/dqdl/execution/DQDLExecutor.scala" pos="26:2:2" line-data="object DQDLExecutor {">`DQDLExecutor`</SwmToken> class. It takes a <SwmToken path="src/main/scala/com/amazon/deequ/dqdl/execution/DQDLExecutor.scala" pos="27:8:8" line-data="  def executeRules(df: DataFrame, checks: Seq[Check]): VerificationResult = {">`DataFrame`</SwmToken> and a sequence of checks as input and returns a <SwmToken path="src/main/scala/com/amazon/deequ/dqdl/execution/DQDLExecutor.scala" pos="27:21:21" line-data="  def executeRules(df: DataFrame, checks: Seq[Check]): VerificationResult = {">`VerificationResult`</SwmToken>. This function uses the <SwmToken path="src/main/scala/com/amazon/deequ/dqdl/execution/DQDLExecutor.scala" pos="28:1:1" line-data="    VerificationSuite()">`VerificationSuite`</SwmToken> to run the checks on the <SwmToken path="src/main/scala/com/amazon/deequ/dqdl/execution/DQDLExecutor.scala" pos="27:8:8" line-data="  def executeRules(df: DataFrame, checks: Seq[Check]): VerificationResult = {">`DataFrame`</SwmToken>.

```scala
  def executeRules(df: DataFrame, checks: Seq[Check]): VerificationResult = {
    VerificationSuite()
      .onData(df)
      .addChecks(checks)
      .run()
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/VerificationSuite.scala" line="49">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/dqdl/execution/DQDLExecutor.scala" pos="27:3:3" line-data="  def executeRules(df: DataFrame, checks: Seq[Check]): VerificationResult = {">`executeRules`</SwmToken> function calls the <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="49:3:3" line-data="  def onData(data: DataFrame): VerificationRunBuilder = {">`onData`</SwmToken> method of <SwmToken path="src/main/scala/com/amazon/deequ/dqdl/execution/DQDLExecutor.scala" pos="28:1:1" line-data="    VerificationSuite()">`VerificationSuite`</SwmToken> to specify the <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="49:8:8" line-data="  def onData(data: DataFrame): VerificationRunBuilder = {">`DataFrame`</SwmToken> on which the checks should be executed.

```scala
  def onData(data: DataFrame): VerificationRunBuilder = {
    new VerificationRunBuilder(data)
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/VerificationRunBuilder.scala" line="86">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/dqdl/execution/DQDLExecutor.scala" pos="27:3:3" line-data="  def executeRules(df: DataFrame, checks: Seq[Check]): VerificationResult = {">`executeRules`</SwmToken> function then calls the <SwmToken path="src/main/scala/com/amazon/deequ/VerificationRunBuilder.scala" pos="86:3:3" line-data="  def addChecks(checks: Seq[Check]): this.type = {">`addChecks`</SwmToken> method to add the sequence of checks to be executed during the run.

```scala
  def addChecks(checks: Seq[Check]): this.type = {
    this.checks ++= checks
    this
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/VerificationSuite.scala" line="67">

---

Finally, the <SwmToken path="src/main/scala/com/amazon/deequ/dqdl/execution/DQDLExecutor.scala" pos="27:3:3" line-data="  def executeRules(df: DataFrame, checks: Seq[Check]): VerificationResult = {">`executeRules`</SwmToken> function calls the <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="67:3:3" line-data="  def run(">`run`</SwmToken> method to execute all the checks and return the verification result.

```scala
  def run(
      data: DataFrame,
      checks: Seq[Check],
      requiredAnalysis: Analysis = Analysis(),
      aggregateWith: Option[StateLoader] = None,
      saveStatesWith: Option[StatePersister] = None,
      metricsRepository: Option[MetricsRepository] = None,
      saveOrAppendResultsWithKey: Option[ResultKey] = None)
    : VerificationResult = {
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBZGVlcXUlM0ElM0Fhd3NsYWJz" repo-name="deequ"><sup>Powered by [Swimm](/)</sup></SwmMeta>
