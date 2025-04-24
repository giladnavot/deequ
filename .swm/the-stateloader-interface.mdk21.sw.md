---
title: The StateLoader interface
---
This document will cover the interface <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala" pos="37:2:2" line-data="trait StateLoader {">`StateLoader`</SwmToken> in the file <SwmPath>[src/…/analyzers/StateProvider.scala](src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala)</SwmPath>. We will cover:

1. What is <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala" pos="37:2:2" line-data="trait StateLoader {">`StateLoader`</SwmToken>
2. Variables and functions

# What is <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala" pos="37:2:2" line-data="trait StateLoader {">`StateLoader`</SwmToken>

The <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala" pos="37:2:2" line-data="trait StateLoader {">`StateLoader`</SwmToken> interface in <SwmPath>[src/…/analyzers/StateProvider.scala](src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala)</SwmPath> is used to load a stored state for an analyzer. It defines a method to retrieve the state associated with a specific analyzer, which is essential for resuming analysis or reusing previously computed states.

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala" line="38">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala" pos="38:3:3" line-data="  def load[S &lt;: State[_]](analyzer: Analyzer[S, _]): Option[S]">`load`</SwmToken> function is the primary method defined in the <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala" pos="37:2:2" line-data="trait StateLoader {">`StateLoader`</SwmToken> interface. It is used to load a stored state for a given analyzer. The function takes an <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala" pos="38:18:18" line-data="  def load[S &lt;: State[_]](analyzer: Analyzer[S, _]): Option[S]">`Analyzer`</SwmToken> as a parameter and returns an <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/StateProvider.scala" pos="38:28:28" line-data="  def load[S &lt;: State[_]](analyzer: Analyzer[S, _]): Option[S]">`Option`</SwmToken> containing the state if it exists.

```scala
  def load[S <: State[_]](analyzer: Analyzer[S, _]): Option[S]
}
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBZGVlcXUlM0ElM0Fhd3NsYWJz" repo-name="deequ"><sup>Powered by [Swimm](/)</sup></SwmMeta>
