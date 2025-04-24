---
title: The CheckWithLastConstraintFilterable class
---
This document will cover the class <SwmToken path="src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" pos="50:4:4" line-data="    ): CheckWithLastConstraintFilterable = {">`CheckWithLastConstraintFilterable`</SwmToken> in the <SwmToken path="src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" pos="17:6:6" line-data="package com.amazon.deequ.checks">`deequ`</SwmToken> repository. We will discuss:

1. What <SwmToken path="src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" pos="50:4:4" line-data="    ): CheckWithLastConstraintFilterable = {">`CheckWithLastConstraintFilterable`</SwmToken> is and its purpose.
2. The variables and functions defined in <SwmToken path="src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" pos="50:4:4" line-data="    ): CheckWithLastConstraintFilterable = {">`CheckWithLastConstraintFilterable`</SwmToken>.

# What is <SwmToken path="src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" pos="50:4:4" line-data="    ): CheckWithLastConstraintFilterable = {">`CheckWithLastConstraintFilterable`</SwmToken>

<SwmToken path="src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" pos="50:4:4" line-data="    ): CheckWithLastConstraintFilterable = {">`CheckWithLastConstraintFilterable`</SwmToken> is a class in the <SwmToken path="src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" pos="17:6:6" line-data="package com.amazon.deequ.checks">`deequ`</SwmToken> library, defined in <SwmPath>[src/…/checks/CheckWithLastConstraintFilterable.scala](src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala)</SwmPath>. It is used to replace the last configured constraint in a check with a filtered version. This allows for more flexible and dynamic constraint definitions when working with data quality checks.

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" line="23">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" pos="23:1:1" line-data="    level: CheckLevel.Value,">`level`</SwmToken> is used to store the check level, which indicates the severity of the check (e.g., error, warning). It is initialized in the constructor.

```scala
    level: CheckLevel.Value,
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" line="24">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" pos="24:1:1" line-data="    description: String,">`description`</SwmToken> is used to store a description of the check. It provides context and information about what the check is verifying. It is initialized in the constructor.

```scala
    description: String,
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" line="25">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" pos="25:1:1" line-data="    constraints: Seq[Constraint],">`constraints`</SwmToken> is a sequence of <SwmToken path="src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" pos="25:6:6" line-data="    constraints: Seq[Constraint],">`Constraint`</SwmToken> objects that define the specific checks to be performed. It is initialized in the constructor.

```scala
    constraints: Seq[Constraint],
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" line="26">

---

The variable <SwmToken path="src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" pos="26:1:1" line-data="    createReplacement: Option[String] =&gt; Constraint)">`createReplacement`</SwmToken> is a function that takes an optional filter string and returns a <SwmToken path="src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" pos="26:11:11" line-data="    createReplacement: Option[String] =&gt; Constraint)">`Constraint`</SwmToken>. This function is used to create a new constraint with the specified filter applied. It is initialized in the constructor.

```scala
    createReplacement: Option[String] => Constraint)
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" line="29">

---

The function <SwmToken path="src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" pos="35:3:3" line-data="  def where(filter: String): Check = {">`where`</SwmToken> defines a filter to apply before evaluating the previous constraint. It takes a <SwmToken path="src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" pos="32:8:8" line-data="    * @param filter SparkSQL predicate to apply">`SparkSQL`</SwmToken> predicate as a parameter and returns a new <SwmToken path="src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" pos="35:12:12" line-data="  def where(filter: String): Check = {">`Check`</SwmToken> object with the adjusted constraints.

```scala
  /**
    * Defines a filter to apply before evaluating the previous constraint
    *
    * @param filter SparkSQL predicate to apply
    * @return
    */
  def where(filter: String): Check = {

    val adjustedConstraints =
      constraints.take(constraints.size - 1) :+ createReplacement(Option(filter))

    Check(level, description, adjustedConstraints)
  }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" line="45">

---

The <SwmToken path="src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" pos="45:3:3" line-data="  def apply(">`apply`</SwmToken> function is a companion object method that creates an instance of <SwmToken path="src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" pos="50:4:4" line-data="    ): CheckWithLastConstraintFilterable = {">`CheckWithLastConstraintFilterable`</SwmToken>. It takes the check level, description, constraints, and the <SwmToken path="src/main/scala/com/amazon/deequ/checks/CheckWithLastConstraintFilterable.scala" pos="49:1:1" line-data="      createReplacement: Option[String] =&gt; Constraint">`createReplacement`</SwmToken> function as parameters.

```scala
  def apply(
      level: CheckLevel.Value,
      description: String,
      constraints: Seq[Constraint],
      createReplacement: Option[String] => Constraint
    ): CheckWithLastConstraintFilterable = {

    new CheckWithLastConstraintFilterable(level, description, constraints, createReplacement)
  }
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBZGVlcXUlM0ElM0Fhd3NsYWJz" repo-name="deequ"><sup>Powered by [Swimm](/)</sup></SwmMeta>
