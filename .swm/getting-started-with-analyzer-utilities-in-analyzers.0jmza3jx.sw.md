---
title: Getting Started with Analyzer Utilities in Analyzers
---
# Overview

Analyzer Utilities are essential components that facilitate the loading and persisting of states for analyzers. These utilities ensure that the states of various analyzers, such as <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala" pos="91:6:6" line-data="      case _: Size =&gt;">`Size`</SwmToken>, <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala" pos="94:7:7" line-data="      case _ : Completeness | _ : Compliance | _ : PatternMatch =&gt;">`Completeness`</SwmToken>, and <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala" pos="97:6:6" line-data="      case _: Sum =&gt;">`Sum`</SwmToken>, can be efficiently managed and retrieved.

# Components

The primary components of Analyzer Utilities include the <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala" pos="37:2:2" line-data="trait StateLoader {">`StateLoader`</SwmToken> and <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala" pos="42:2:2" line-data="trait StatePersister {">`StatePersister`</SwmToken> traits. These traits define the methods for loading and persisting states, respectively.

# Implementations

The <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala" pos="47:4:4" line-data="case class InMemoryStateProvider() extends StateLoader with StatePersister {">`InMemoryStateProvider`</SwmToken> and <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala" pos="32:9:9" line-data="  // as used in HdfsStateProvider.persistDataTypeState and HdfsStateProvider.loadDataTypeState">`HdfsStateProvider`</SwmToken> classes implement the <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala" pos="37:2:2" line-data="trait StateLoader {">`StateLoader`</SwmToken> and <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala" pos="42:2:2" line-data="trait StatePersister {">`StatePersister`</SwmToken> traits to store states either in memory or on a filesystem.

# <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala" pos="47:4:4" line-data="case class InMemoryStateProvider() extends StateLoader with StatePersister {">`InMemoryStateProvider`</SwmToken>

The <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala" pos="47:4:4" line-data="case class InMemoryStateProvider() extends StateLoader with StatePersister {">`InMemoryStateProvider`</SwmToken> uses a concurrent hash map to manage states, providing an efficient in-memory storage solution.

# <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala" pos="32:9:9" line-data="  // as used in HdfsStateProvider.persistDataTypeState and HdfsStateProvider.loadDataTypeState">`HdfsStateProvider`</SwmToken>

The <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala" pos="32:9:9" line-data="  // as used in HdfsStateProvider.persistDataTypeState and HdfsStateProvider.loadDataTypeState">`HdfsStateProvider`</SwmToken> supports storage on local disks, HDFS, and <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala" pos="72:23:23" line-data="/** Store states on a filesystem (supports local disk, HDFS, S3) */">`S3`</SwmToken>, offering a versatile solution for persisting states across different storage systems.

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala" line="36">

---

These utilities ensure that the states of various analyzers can be efficiently managed and retrieved.

```scala
/** Load a stored state for an analyzer */
trait StateLoader {
  def load[S <: State[_]](analyzer: Analyzer[S, _]): Option[S]
}
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBZGVlcXUlM0ElM0Fhd3NsYWJz" repo-name="deequ"><sup>Powered by [Swimm](/)</sup></SwmMeta>
