---
title: The ColumnProfilerRunner class
---
This document will cover the class <SwmToken path="src/main/scala/com/amazon/deequ/profiles/ColumnProfilerRunner.scala" pos="112:8:8" line-data="  def apply(): ColumnProfilerRunner = {">`ColumnProfilerRunner`</SwmToken> in detail. We will discuss:

1. What is <SwmToken path="src/main/scala/com/amazon/deequ/profiles/ColumnProfilerRunner.scala" pos="112:8:8" line-data="  def apply(): ColumnProfilerRunner = {">`ColumnProfilerRunner`</SwmToken>
2. Variables and functions in <SwmToken path="src/main/scala/com/amazon/deequ/profiles/ColumnProfilerRunner.scala" pos="112:8:8" line-data="  def apply(): ColumnProfilerRunner = {">`ColumnProfilerRunner`</SwmToken>

# What is <SwmToken path="src/main/scala/com/amazon/deequ/profiles/ColumnProfilerRunner.scala" pos="112:8:8" line-data="  def apply(): ColumnProfilerRunner = {">`ColumnProfilerRunner`</SwmToken>

The <SwmToken path="src/main/scala/com/amazon/deequ/profiles/ColumnProfilerRunner.scala" pos="112:8:8" line-data="  def apply(): ColumnProfilerRunner = {">`ColumnProfilerRunner`</SwmToken> class, located in <SwmPath>[src/…/profiles/ColumnProfilerRunner.scala](src/main/scala/com/amazon/deequ/profiles/ColumnProfilerRunner.scala)</SwmPath>, is used for profiling columns in a dataset. It helps in analyzing and understanding the data by generating profiles for each column, which includes statistics and other relevant information.

<SwmSnippet path="/src/main/scala/com/amazon/deequ/profiles/ColumnProfilerRunner.scala" line="39">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/profiles/ColumnProfilerRunner.scala" pos="39:3:3" line-data="  def onData(data: DataFrame): ColumnProfilerRunBuilder = {">`onData`</SwmToken> function is used to set the data that needs to be profiled. It takes a <SwmToken path="src/main/scala/com/amazon/deequ/profiles/ColumnProfilerRunner.scala" pos="39:8:8" line-data="  def onData(data: DataFrame): ColumnProfilerRunBuilder = {">`DataFrame`</SwmToken> as input and returns a <SwmToken path="src/main/scala/com/amazon/deequ/profiles/ColumnProfilerRunner.scala" pos="39:12:12" line-data="  def onData(data: DataFrame): ColumnProfilerRunBuilder = {">`ColumnProfilerRunBuilder`</SwmToken> instance.

```scala
  def onData(data: DataFrame): ColumnProfilerRunBuilder = {
    new ColumnProfilerRunBuilder(data)
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/profiles/ColumnProfilerRunner.scala" line="43">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/profiles/ColumnProfilerRunner.scala" pos="43:8:8" line-data="  private[profiles] def run(">`run`</SwmToken> function is a private method that performs the actual profiling of the data. It takes several parameters including the data to be profiled, options for restricting columns, and various configuration options. It returns a <SwmToken path="src/main/scala/com/amazon/deequ/profiles/ColumnProfilerRunner.scala" pos="54:3:3" line-data="    : ColumnProfiles = {">`ColumnProfiles`</SwmToken> object containing the profiles of the columns.

```scala
  private[profiles] def run(
      data: DataFrame,
      restrictToColumns: Option[Seq[String]],
      lowCardinalityHistogramThreshold: Int,
      printStatusUpdates: Boolean,
      cacheInputs: Boolean,
      fileOutputOptions: ColumnProfilerRunBuilderFileOutputOptions,
      metricsRepositoryOptions: ColumnProfilerRunBuilderMetricsRepositoryOptions,
      kllProfiling: Boolean,
      kllParameters: Option[KLLParameters],
      predefinedTypes: Map[String, DataTypeInstances.Value])
    : ColumnProfiles = {

    if (cacheInputs) {
      data.cache()
    }

    val columnProfiles = ColumnProfiler
      .profile(
        data,
        restrictToColumns,
        printStatusUpdates,
        lowCardinalityHistogramThreshold,
        metricsRepositoryOptions.metricsRepository,
        metricsRepositoryOptions.reuseExistingResultsKey,
        metricsRepositoryOptions.failIfResultsForReusingMissing,
        metricsRepositoryOptions.saveOrAppendResultsKey,
        kllProfiling,
        kllParameters,
        predefinedTypes
      )

    saveColumnProfilesJsonToFileSystemIfNecessary(
      fileOutputOptions,
      printStatusUpdates,
      columnProfiles
    )

    if (cacheInputs) {
      data.unpersist()
    }

    columnProfiles
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/profiles/ColumnProfilerRunner.scala" line="88">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/profiles/ColumnProfilerRunner.scala" pos="88:8:8" line-data="  private[this] def saveColumnProfilesJsonToFileSystemIfNecessary(">`saveColumnProfilesJsonToFileSystemIfNecessary`</SwmToken> function is a private method that saves the generated column profiles to the file system if required. It takes options for file output, a flag for printing status updates, and the column profiles to be saved.

```scala
  private[this] def saveColumnProfilesJsonToFileSystemIfNecessary(
      fileOutputOptions: ColumnProfilerRunBuilderFileOutputOptions,
      printStatusUpdates: Boolean,
      columnProfiles: ColumnProfiles)
    : Unit = {

    fileOutputOptions.session.foreach { session =>
      fileOutputOptions.saveColumnProfilesJsonToPath.foreach { profilesOutput =>
        if (printStatusUpdates) {
          println(s"### WRITING COLUMN PROFILES TO $profilesOutput")
        }

        DfsUtils.writeToTextFileOnDfs(session, profilesOutput,
          overwrite = fileOutputOptions.overwriteResults) { writer =>
            writer.append(ColumnProfiles.toJson(columnProfiles.profiles.values.toSeq).toString)
            writer.newLine()
          }
        }
    }
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/profiles/ColumnProfilerRunner.scala" line="112">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/profiles/ColumnProfilerRunner.scala" pos="112:3:3" line-data="  def apply(): ColumnProfilerRunner = {">`apply`</SwmToken> function is a companion object method that creates a new instance of <SwmToken path="src/main/scala/com/amazon/deequ/profiles/ColumnProfilerRunner.scala" pos="112:8:8" line-data="  def apply(): ColumnProfilerRunner = {">`ColumnProfilerRunner`</SwmToken>.

```scala
  def apply(): ColumnProfilerRunner = {
    new ColumnProfilerRunner()
  }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBZGVlcXUlM0ElM0Fhd3NsYWJz" repo-name="deequ"><sup>Powered by [Swimm](/)</sup></SwmMeta>
