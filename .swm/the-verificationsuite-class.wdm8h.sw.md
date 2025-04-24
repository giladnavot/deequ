---
title: The VerificationSuite class
---
This document will cover the class <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="287:8:8" line-data="  def apply(): VerificationSuite = {">`VerificationSuite`</SwmToken> in detail. We will discuss:

1. What is <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="287:8:8" line-data="  def apply(): VerificationSuite = {">`VerificationSuite`</SwmToken>
2. Variables and functions in <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="287:8:8" line-data="  def apply(): VerificationSuite = {">`VerificationSuite`</SwmToken>

# What is <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="287:8:8" line-data="  def apply(): VerificationSuite = {">`VerificationSuite`</SwmToken>

The <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="287:8:8" line-data="  def apply(): VerificationSuite = {">`VerificationSuite`</SwmToken> class in <SwmPath>[src/…/deequ/VerificationSuite.scala](src/main/scala/com/amazon/deequ/VerificationSuite.scala)</SwmPath> is responsible for running checks and required analysis on data and returning the results. It is used to measure data quality by verifying constraints on datasets.

<SwmSnippet path="/src/main/scala/com/amazon/deequ/VerificationSuite.scala" line="287">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="287:3:3" line-data="  def apply(): VerificationSuite = {">`apply`</SwmToken> function is a factory method that creates a new instance of <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="287:8:8" line-data="  def apply(): VerificationSuite = {">`VerificationSuite`</SwmToken>.

```scala
  def apply(): VerificationSuite = {
    new VerificationSuite()
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/VerificationSuite.scala" line="292">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="292:3:3" line-data="  def run(">`run`</SwmToken> function is deprecated and was used to run checks on a dataset and return the verification result. It takes a <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="293:4:4" line-data="      data: DataFrame,">`DataFrame`</SwmToken>, a sequence of <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="294:6:6" line-data="      checks: Seq[Check],">`Check`</SwmToken> objects, and optionally an <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="295:4:4" line-data="      requiredAnalysis: Analysis = Analysis())">`Analysis`</SwmToken> object.

```scala
  def run(
      data: DataFrame,
      checks: Seq[Check],
      requiredAnalysis: Analysis = Analysis())
    : VerificationResult = {

    val analyzers = requiredAnalysis.analyzers ++ checks.flatMap { _.requiredAnalyzers() }
    VerificationSuite().doVerificationRun(data, checks, analyzers)
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/VerificationSuite.scala" line="302">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="302:3:3" line-data="  def runOnAggregatedStates(">`runOnAggregatedStates`</SwmToken> function runs checks on aggregated states and returns the verification result. It takes a schema, a sequence of <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="304:6:6" line-data="      checks: Seq[Check],">`Check`</SwmToken> objects, a sequence of <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="305:6:6" line-data="      stateLoaders: Seq[StateLoader],">`StateLoader`</SwmToken> objects, and optionally an <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="306:4:4" line-data="      requiredAnalysis: Analysis = Analysis(),">`Analysis`</SwmToken> object, a <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="307:6:6" line-data="      saveStatesWith: Option[StatePersister] = None,">`StatePersister`</SwmToken>, a <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="308:6:6" line-data="      metricsRepository: Option[MetricsRepository] = None,">`MetricsRepository`</SwmToken>, and a <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="309:6:6" line-data="      saveOrAppendResultsWithKey: Option[ResultKey] = None                           )">`ResultKey`</SwmToken>.

```scala
  def runOnAggregatedStates(
      schema: StructType,
      checks: Seq[Check],
      stateLoaders: Seq[StateLoader],
      requiredAnalysis: Analysis = Analysis(),
      saveStatesWith: Option[StatePersister] = None,
      metricsRepository: Option[MetricsRepository] = None,
      saveOrAppendResultsWithKey: Option[ResultKey] = None                           )
    : VerificationResult = {

    VerificationSuite().runOnAggregatedStates(schema, checks, stateLoaders, requiredAnalysis,
      saveStatesWith, metricsRepository, saveOrAppendResultsWithKey)
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/VerificationSuite.scala" line="49">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="49:3:3" line-data="  def onData(data: DataFrame): VerificationRunBuilder = {">`onData`</SwmToken> function is the starting point to construct a <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="45:13:13" line-data="    * Starting point to construct a VerificationRun.">`VerificationRun`</SwmToken>. It takes a <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="49:8:8" line-data="  def onData(data: DataFrame): VerificationRunBuilder = {">`DataFrame`</SwmToken> and returns a <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="49:12:12" line-data="  def onData(data: DataFrame): VerificationRunBuilder = {">`VerificationRunBuilder`</SwmToken>.

```scala
  def onData(data: DataFrame): VerificationRunBuilder = {
    new VerificationRunBuilder(data)
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/VerificationSuite.scala" line="107">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="107:8:8" line-data="  private[deequ] def doVerificationRun(">`doVerificationRun`</SwmToken> function runs all check groups and returns the verification result. It includes all the metrics computed during the run and takes various parameters including the data, checks, required analyzers, and options for state loading, state persisting, metrics repository, and file output.

```scala
  private[deequ] def doVerificationRun(
      data: DataFrame,
      checks: Seq[Check],
      requiredAnalyzers: Seq[Analyzer[_, Metric[_]]],
      aggregateWith: Option[StateLoader] = None,
      saveStatesWith: Option[StatePersister] = None,
      metricsRepositoryOptions: VerificationMetricsRepositoryOptions =
        VerificationMetricsRepositoryOptions(),
      fileOutputOptions: VerificationFileOutputOptions =
        VerificationFileOutputOptions())
    : VerificationResult = {

    val analyzers = requiredAnalyzers ++ checks.flatMap { _.requiredAnalyzers() }

    val analysisResults = AnalysisRunner.doAnalysisRun(
      data,
      analyzers.distinct,
      aggregateWith,
      saveStatesWith,
      metricsRepositoryOptions = AnalysisRunnerRepositoryOptions(
        metricsRepositoryOptions.metricsRepository,
        metricsRepositoryOptions.reuseExistingResultsForKey,
        metricsRepositoryOptions.failIfResultsForReusingMissing,
        saveOrAppendResultsWithKey = None))

    val verificationResult = evaluate(checks, analysisResults)

    val analyzerContext = AnalyzerContext(verificationResult.metrics)

    saveOrAppendResultsIfNecessary(
      analyzerContext,
      metricsRepositoryOptions.metricsRepository,
      metricsRepositoryOptions.saveOrAppendResultsWithKey)

    saveJsonOutputsToFilesystemIfNecessary(fileOutputOptions, verificationResult)

    verificationResult
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/VerificationSuite.scala" line="238">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="238:3:3" line-data="  def isCheckApplicableToData(">`isCheckApplicableToData`</SwmToken> function checks whether a check is applicable to some data using the schema of the data. It takes a <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="239:4:4" line-data="      check: Check,">`Check`</SwmToken>, a <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="240:4:4" line-data="      schema: StructType,">`StructType`</SwmToken> schema, and a <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="241:4:4" line-data="      sparkSession: SparkSession)">`SparkSession`</SwmToken>.

```scala
  def isCheckApplicableToData(
      check: Check,
      schema: StructType,
      sparkSession: SparkSession)
    : CheckApplicability = {

    new Applicability(sparkSession).isApplicable(check, schema)
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/VerificationSuite.scala" line="254">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="254:3:3" line-data="  def areAnalyzersApplicableToData(">`areAnalyzersApplicableToData`</SwmToken> function checks whether analyzers are applicable to some data using the schema of the data. It takes a sequence of <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="255:6:6" line-data="      analyzers: Seq[Analyzer[_ &lt;: State[_], Metric[_]]],">`Analyzer`</SwmToken> objects, a <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="256:4:4" line-data="      schema: StructType,">`StructType`</SwmToken> schema, and a <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="257:4:4" line-data="      sparkSession: SparkSession)">`SparkSession`</SwmToken>.

```scala
  def areAnalyzersApplicableToData(
      analyzers: Seq[Analyzer[_ <: State[_], Metric[_]]],
      schema: StructType,
      sparkSession: SparkSession)
    : AnalyzersApplicability = {

    new Applicability(sparkSession).isApplicable(analyzers, schema)
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/VerificationSuite.scala" line="263">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="263:8:8" line-data="  private[this] def evaluate(">`evaluate`</SwmToken> function runs all check groups and returns the verification result. It includes all the metrics computed during the run and takes the checks and analysis context as parameters.

```scala
  private[this] def evaluate(
      checks: Seq[Check],
      analysisContext: AnalyzerContext)
    : VerificationResult = {

    val checkResults = checks
      .map { check => check -> check.evaluate(analysisContext) }
      .toMap

    val verificationStatus = if (checkResults.isEmpty) {
      CheckStatus.Success
    } else {
      checkResults.values
        .map { _.status }
        .max
    }

    VerificationResult(verificationStatus, checkResults, analysisContext.metricMap)
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/VerificationSuite.scala" line="174">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="174:8:8" line-data="  private[this] def saveOrAppendResultsIfNecessary(">`saveOrAppendResultsIfNecessary`</SwmToken> function saves or appends results to the metrics repository if necessary. It takes the resulting analyzer context, an optional metrics repository, and an optional result key.

```scala
  private[this] def saveOrAppendResultsIfNecessary(
      resultingAnalyzerContext: AnalyzerContext,
      metricsRepository: Option[MetricsRepository],
      saveOrAppendResultsWithKey: Option[ResultKey])
    : Unit = {

    metricsRepository.foreach{repository =>
      saveOrAppendResultsWithKey.foreach { key =>

        val currentValueForKey = repository.loadByKey(key)

        // AnalyzerContext entries on the right side of ++ will overwrite the ones on the left
        // if there are two different metric results for the same analyzer
        val valueToSave = currentValueForKey.getOrElse(AnalyzerContext.empty) ++
          resultingAnalyzerContext

        repository.save(saveOrAppendResultsWithKey.get, valueToSave)
      }
    }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/VerificationSuite.scala" line="146">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/VerificationSuite.scala" pos="146:8:8" line-data="  private[this] def saveJsonOutputsToFilesystemIfNecessary(">`saveJsonOutputsToFilesystemIfNecessary`</SwmToken> function saves JSON outputs to the filesystem if necessary. It takes the file output options and the verification result.

```scala
  private[this] def saveJsonOutputsToFilesystemIfNecessary(
    fileOutputOptions: VerificationFileOutputOptions,
    verificationResult: VerificationResult)
  : Unit = {

    fileOutputOptions.sparkSession.foreach { session =>
      fileOutputOptions.saveCheckResultsJsonToPath.foreach { profilesOutput =>

        DfsUtils.writeToTextFileOnDfs(session, profilesOutput,
          overwrite = fileOutputOptions.overwriteOutputFiles) { writer =>
            writer.append(VerificationResult.checkResultsAsJson(verificationResult))
            writer.newLine()
          }
        }
    }

    fileOutputOptions.sparkSession.foreach { session =>
      fileOutputOptions.saveSuccessMetricsJsonToPath.foreach { profilesOutput =>

        DfsUtils.writeToTextFileOnDfs(session, profilesOutput,
          overwrite = fileOutputOptions.overwriteOutputFiles) { writer =>
            writer.append(VerificationResult.successMetricsAsJson(verificationResult))
            writer.newLine()
          }
        }
    }
  }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBZGVlcXUlM0ElM0Fhd3NsYWJz" repo-name="deequ"><sup>Powered by [Swimm](/)</sup></SwmMeta>
