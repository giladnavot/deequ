---
title: Running Analyzers for Data Grouping
---
This document describes the process of running analyzers for a specific data grouping. This flow is used to measure data quality by executing various analyzers on grouped data, ensuring efficient and accurate computation of metrics.

For instance, if we have a dataset grouped by a specific column, this flow will execute the relevant analyzers on each group, cache the data if necessary, and return the computed metrics.

```mermaid
sequenceDiagram
  participant System
  participant Data
  System->>Data: Partition analyzers
  System->>Data: Cache grouped data
  System->>Data: Execute shareable analyzers
  System->>Data: Compute offsets
  System->>Data: Map results to metrics
  System->>Data: Execute remaining analyzers
  System->>Data: Store states
  System->>Data: Unpersist data
  System->>System: Return combined metrics
```

# Where is this flow used?

This flow is used multiple times in the codebase as represented in the following diagram:

(Note - these are only some of the entry points of this flow)

```mermaid
graph TD;
      subgraph srcmainscalacomamazondeequanalyzersrunners[src/…/analyzers/runners]
33733958ecd12639b20b16b3fc55c1930b2973ba070e3a0b1b3ebcc41bf61666(runGroupingAnalyzers) --> 3b4b73b181b3a3f8c0d7c66219ae6e63fe8b1a3e2b3299553f8604a6954cee33(runAnalyzersForParticularGrouping):::mainFlowStyle
end

subgraph srcmainscalacomamazondeequanalyzersrunners[src/…/analyzers/runners]
e5752a91830d109e628d9caa4f30f131ae702841d2c1770a788cf852bdffc5c0(doAnalysisRun) --> 33733958ecd12639b20b16b3fc55c1930b2973ba070e3a0b1b3ebcc41bf61666(runGroupingAnalyzers)
end

subgraph srcmainscalacomamazondeequanalyzersrunners[src/…/analyzers/runners]
37019a251f21166cf5d6727a9c743dc9a5aac113185efc286bc7d74bb0772dca(doVerificationRun) --> e5752a91830d109e628d9caa4f30f131ae702841d2c1770a788cf852bdffc5c0(doAnalysisRun)
end

subgraph srcmainscalacomamazondeequ[src/…/amazon/deequ]
eef9f0e47bde415224905aecf6ed08b1262ee367ec5de1f7028ce980049b6f17(run) --> 37019a251f21166cf5d6727a9c743dc9a5aac113185efc286bc7d74bb0772dca(doVerificationRun)
end

subgraph srcmainscalacomamazondeequ[src/…/amazon/deequ]
ffd5fcd1f28130a6370cd1351d97516606cc377c083f8513f90d693d8b14d663(executeRules) --> eef9f0e47bde415224905aecf6ed08b1262ee367ec5de1f7028ce980049b6f17(run)
end

subgraph srcmainscalacomamazondeequdqdl[src/…/deequ/dqdl]
850b81f8d8a4b6332a9abada9ebd0b5970a7917501d4e3dcefc618ae7c9f065f(process) --> ffd5fcd1f28130a6370cd1351d97516606cc377c083f8513f90d693d8b14d663(executeRules)
end

subgraph srcmainscalacomamazondeequ[src/…/amazon/deequ]
eef9f0e47bde415224905aecf6ed08b1262ee367ec5de1f7028ce980049b6f17(run) --> 37019a251f21166cf5d6727a9c743dc9a5aac113185efc286bc7d74bb0772dca(doVerificationRun)
end

subgraph srcmainscalacomamazondeequ[src/…/amazon/deequ]
7c4e6de769dd34ac3db30f879ea7421bb9ff12bec1fc36099d4d606af5870da3(run) --> 37019a251f21166cf5d6727a9c743dc9a5aac113185efc286bc7d74bb0772dca(doVerificationRun)
end

subgraph srcmainscalacomamazondeequanalyzersrunners[src/…/analyzers/runners]
bf54f76e76c709167dcdaf968f93d6acc619c0db169415df2c176fb0f68b8128(run) --> e5752a91830d109e628d9caa4f30f131ae702841d2c1770a788cf852bdffc5c0(doAnalysisRun)
end

subgraph srcmainscalacomamazondeequanalyzersrunners[src/…/analyzers/runners]
3e0484be00eeeeea41f90a04cc8001fc899bc96a93dad02363bcdd3a9beb29dc(run) --> e5752a91830d109e628d9caa4f30f131ae702841d2c1770a788cf852bdffc5c0(doAnalysisRun)
end

subgraph srcmainscalacomamazondeequanalyzersrunners[src/…/analyzers/runners]
fb3372d40f78158defe8fc5f069b96863c6b17e74bf6dbe709b7de0b1bf930c0(run) --> e5752a91830d109e628d9caa4f30f131ae702841d2c1770a788cf852bdffc5c0(doAnalysisRun)
end

subgraph srcmainscalacomamazondeequanalyzersrunners[src/…/analyzers/runners]
db8f3a16e1eb168b291289fc7c4d60779d4cdd5bce7eb2cddc136d1d45daea1e(runOnAggregatedStates) --> 3b4b73b181b3a3f8c0d7c66219ae6e63fe8b1a3e2b3299553f8604a6954cee33(runAnalyzersForParticularGrouping):::mainFlowStyle
end


      classDef mainFlowStyle color:#000000,fill:#7CB9F4
classDef rootsStyle color:#000000,fill:#00FFF4
classDef Style1 color:#000000,fill:#00FFAA
classDef Style2 color:#000000,fill:#FFFF00
classDef Style3 color:#000000,fill:#AA7CB9

%% Swimm:
%% graph TD;
%%       subgraph srcmainscalacomamazondeequanalyzersrunners[<SwmPath>[src/…/analyzers/runners/](src/main/scala/com/amazon/deequ/analyzers/runners/)</SwmPath>]
%% 33733958ecd12639b20b16b3fc55c1930b2973ba070e3a0b1b3ebcc41bf61666(<SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="186:1:1" line-data="          runGroupingAnalyzers(data, groupingColumns, filterCondition, analyzersForGrouping,">`runGroupingAnalyzers`</SwmToken>) --> 3b4b73b181b3a3f8c0d7c66219ae6e63fe8b1a3e2b3299553f8604a6954cee33(<SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="298:7:7" line-data="    val results = runAnalyzersForParticularGrouping(frequenciesAndNumRows, analyzers, saveStatesTo,">`runAnalyzersForParticularGrouping`</SwmToken>):::mainFlowStyle
%% end
%% 
%% subgraph srcmainscalacomamazondeequanalyzersrunners[<SwmPath>[src/…/analyzers/runners/](src/main/scala/com/amazon/deequ/analyzers/runners/)</SwmPath>]
%% e5752a91830d109e628d9caa4f30f131ae702841d2c1770a788cf852bdffc5c0(<SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="79:1:1" line-data="    doAnalysisRun(data, analysis.analyzers, aggregateWith, saveStatesWith,">`doAnalysisRun`</SwmToken>) --> 33733958ecd12639b20b16b3fc55c1930b2973ba070e3a0b1b3ebcc41bf61666(<SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="186:1:1" line-data="          runGroupingAnalyzers(data, groupingColumns, filterCondition, analyzersForGrouping,">`runGroupingAnalyzers`</SwmToken>)
%% end
%% 
%% subgraph srcmainscalacomamazondeequanalyzersrunners[<SwmPath>[src/…/analyzers/runners/](src/main/scala/com/amazon/deequ/analyzers/runners/)</SwmPath>]
%% 37019a251f21166cf5d6727a9c743dc9a5aac113185efc286bc7d74bb0772dca(doVerificationRun) --> e5752a91830d109e628d9caa4f30f131ae702841d2c1770a788cf852bdffc5c0(<SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="79:1:1" line-data="    doAnalysisRun(data, analysis.analyzers, aggregateWith, saveStatesWith,">`doAnalysisRun`</SwmToken>)
%% end
%% 
%% subgraph srcmainscalacomamazondeequ[<SwmPath>[src/…/amazon/deequ/](src/main/scala/com/amazon/deequ/)</SwmPath>]
%% eef9f0e47bde415224905aecf6ed08b1262ee367ec5de1f7028ce980049b6f17(run) --> 37019a251f21166cf5d6727a9c743dc9a5aac113185efc286bc7d74bb0772dca(doVerificationRun)
%% end
%% 
%% subgraph srcmainscalacomamazondeequ[<SwmPath>[src/…/amazon/deequ/](src/main/scala/com/amazon/deequ/)</SwmPath>]
%% ffd5fcd1f28130a6370cd1351d97516606cc377c083f8513f90d693d8b14d663(executeRules) --> eef9f0e47bde415224905aecf6ed08b1262ee367ec5de1f7028ce980049b6f17(run)
%% end
%% 
%% subgraph srcmainscalacomamazondeequdqdl[<SwmPath>[src/…/deequ/dqdl/](src/main/scala/com/amazon/deequ/dqdl/)</SwmPath>]
%% 850b81f8d8a4b6332a9abada9ebd0b5970a7917501d4e3dcefc618ae7c9f065f(process) --> ffd5fcd1f28130a6370cd1351d97516606cc377c083f8513f90d693d8b14d663(executeRules)
%% end
%% 
%% subgraph srcmainscalacomamazondeequ[<SwmPath>[src/…/amazon/deequ/](src/main/scala/com/amazon/deequ/)</SwmPath>]
%% eef9f0e47bde415224905aecf6ed08b1262ee367ec5de1f7028ce980049b6f17(run) --> 37019a251f21166cf5d6727a9c743dc9a5aac113185efc286bc7d74bb0772dca(doVerificationRun)
%% end
%% 
%% subgraph srcmainscalacomamazondeequ[<SwmPath>[src/…/amazon/deequ/](src/main/scala/com/amazon/deequ/)</SwmPath>]
%% 7c4e6de769dd34ac3db30f879ea7421bb9ff12bec1fc36099d4d606af5870da3(run) --> 37019a251f21166cf5d6727a9c743dc9a5aac113185efc286bc7d74bb0772dca(doVerificationRun)
%% end
%% 
%% subgraph srcmainscalacomamazondeequanalyzersrunners[<SwmPath>[src/…/analyzers/runners/](src/main/scala/com/amazon/deequ/analyzers/runners/)</SwmPath>]
%% bf54f76e76c709167dcdaf968f93d6acc619c0db169415df2c176fb0f68b8128(run) --> e5752a91830d109e628d9caa4f30f131ae702841d2c1770a788cf852bdffc5c0(<SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="79:1:1" line-data="    doAnalysisRun(data, analysis.analyzers, aggregateWith, saveStatesWith,">`doAnalysisRun`</SwmToken>)
%% end
%% 
%% subgraph srcmainscalacomamazondeequanalyzersrunners[<SwmPath>[src/…/analyzers/runners/](src/main/scala/com/amazon/deequ/analyzers/runners/)</SwmPath>]
%% 3e0484be00eeeeea41f90a04cc8001fc899bc96a93dad02363bcdd3a9beb29dc(run) --> e5752a91830d109e628d9caa4f30f131ae702841d2c1770a788cf852bdffc5c0(<SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="79:1:1" line-data="    doAnalysisRun(data, analysis.analyzers, aggregateWith, saveStatesWith,">`doAnalysisRun`</SwmToken>)
%% end
%% 
%% subgraph srcmainscalacomamazondeequanalyzersrunners[<SwmPath>[src/…/analyzers/runners/](src/main/scala/com/amazon/deequ/analyzers/runners/)</SwmPath>]
%% fb3372d40f78158defe8fc5f069b96863c6b17e74bf6dbe709b7de0b1bf930c0(run) --> e5752a91830d109e628d9caa4f30f131ae702841d2c1770a788cf852bdffc5c0(<SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="79:1:1" line-data="    doAnalysisRun(data, analysis.analyzers, aggregateWith, saveStatesWith,">`doAnalysisRun`</SwmToken>)
%% end
%% 
%% subgraph srcmainscalacomamazondeequanalyzersrunners[<SwmPath>[src/…/analyzers/runners/](src/main/scala/com/amazon/deequ/analyzers/runners/)</SwmPath>]
%% db8f3a16e1eb168b291289fc7c4d60779d4cdd5bce7eb2cddc136d1d45daea1e(<SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="397:3:3" line-data="    def runOnAggregatedStates(">`runOnAggregatedStates`</SwmToken>) --> 3b4b73b181b3a3f8c0d7c66219ae6e63fe8b1a3e2b3299553f8604a6954cee33(<SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="298:7:7" line-data="    val results = runAnalyzersForParticularGrouping(frequenciesAndNumRows, analyzers, saveStatesTo,">`runAnalyzersForParticularGrouping`</SwmToken>):::mainFlowStyle
%% end
%% 
%% 
%%       classDef mainFlowStyle color:#000000,fill:#7CB9F4
%% classDef rootsStyle color:#000000,fill:#00FFF4
%% classDef Style1 color:#000000,fill:#00FFAA
%% classDef Style2 color:#000000,fill:#FFFF00
%% classDef Style3 color:#000000,fill:#AA7CB9
```

Here is a high level diagram of the flow, showing only the most important functions:

```mermaid
graph TD;
      subgraph srcmainscalacomamazondeequanalyzers[src/…/deequ/analyzers]
3b4b73b181b3a3f8c0d7c66219ae6e63fe8b1a3e2b3299553f8604a6954cee33(runAnalyzersForParticularGrouping) --> 1f1c2b4c219a66cd628cf381ec2a237245ea826fdd17ff8fc151d6988e94062b(successOrFailureMetricFrom)
end

subgraph srcmainscalacomamazondeequanalyzers[src/…/deequ/analyzers]
1f1c2b4c219a66cd628cf381ec2a237245ea826fdd17ff8fc151d6988e94062b(successOrFailureMetricFrom) --> 45121cda4cf293170f79ab9e47e537de5b62406e05a3f9ba84769129370e5d3d(metricFromAggregationResult)
end


      classDef mainFlowStyle color:#000000,fill:#7CB9F4
classDef rootsStyle color:#000000,fill:#00FFF4
classDef Style1 color:#000000,fill:#00FFAA
classDef Style2 color:#000000,fill:#FFFF00
classDef Style3 color:#000000,fill:#AA7CB9

%% Swimm:
%% graph TD;
%%       subgraph srcmainscalacomamazondeequanalyzers[<SwmPath>[src/…/deequ/analyzers/](src/main/scala/com/amazon/deequ/analyzers/)</SwmPath>]
%% 3b4b73b181b3a3f8c0d7c66219ae6e63fe8b1a3e2b3299553f8604a6954cee33(<SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="298:7:7" line-data="    val results = runAnalyzersForParticularGrouping(frequenciesAndNumRows, analyzers, saveStatesTo,">`runAnalyzersForParticularGrouping`</SwmToken>) --> 1f1c2b4c219a66cd628cf381ec2a237245ea826fdd17ff8fc151d6988e94062b(<SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="330:1:1" line-data="            successOrFailureMetricFrom(analyzer, results, offset, aggregateWith, saveStatesTo)">`successOrFailureMetricFrom`</SwmToken>)
%% end
%% 
%% subgraph srcmainscalacomamazondeequanalyzers[<SwmPath>[src/…/deequ/analyzers/](src/main/scala/com/amazon/deequ/analyzers/)</SwmPath>]
%% 1f1c2b4c219a66cd628cf381ec2a237245ea826fdd17ff8fc151d6988e94062b(<SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="330:1:1" line-data="            successOrFailureMetricFrom(analyzer, results, offset, aggregateWith, saveStatesTo)">`successOrFailureMetricFrom`</SwmToken>) --> 45121cda4cf293170f79ab9e47e537de5b62406e05a3f9ba84769129370e5d3d(<SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="360:3:3" line-data="      analyzer.metricFromAggregationResult(aggregationResult, offset, aggregateWith, saveStatesTo)">`metricFromAggregationResult`</SwmToken>)
%% end
%% 
%% 
%%       classDef mainFlowStyle color:#000000,fill:#7CB9F4
%% classDef rootsStyle color:#000000,fill:#00FFF4
%% classDef Style1 color:#000000,fill:#00FFAA
%% classDef Style2 color:#000000,fill:#FFFF00
%% classDef Style3 color:#000000,fill:#AA7CB9
```

# <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="298:7:7" line-data="    val results = runAnalyzersForParticularGrouping(frequenciesAndNumRows, analyzers, saveStatesTo,">`runAnalyzersForParticularGrouping`</SwmToken>

```mermaid
graph TD
 subgraph runAnalyzersForParticularGrouping
 runAnalyzersForParticularGrouping:A["Partition analyzers into shareable and others"] --> runAnalyzersForParticularGrouping:B["Cache grouped data if other analyzers are present"]
 runAnalyzersForParticularGrouping:B --> runAnalyzersForParticularGrouping:C["Execute aggregations for shareable analyzers"]
 runAnalyzersForParticularGrouping:C --> runAnalyzersForParticularGrouping:D["Compute offsets for analyzers"]
 runAnalyzersForParticularGrouping:D --> runAnalyzersForParticularGrouping:E["Map results to success or failure metrics"]
 runAnalyzersForParticularGrouping:B --> runAnalyzersForParticularGrouping:F["Execute remaining analyzers on grouped data"]
 runAnalyzersForParticularGrouping:F --> runAnalyzersForParticularGrouping:G["Store states if required"]
 runAnalyzersForParticularGrouping:G --> runAnalyzersForParticularGrouping:H["Unpersist grouped data"]
 runAnalyzersForParticularGrouping:H --> runAnalyzersForParticularGrouping:I["Return combined metrics"]
 end

%% Swimm:
%% graph TD
%%  subgraph <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="298:7:7" line-data="    val results = runAnalyzersForParticularGrouping(frequenciesAndNumRows, analyzers, saveStatesTo,">`runAnalyzersForParticularGrouping`</SwmToken>
%%  <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="298:7:7" line-data="    val results = runAnalyzersForParticularGrouping(frequenciesAndNumRows, analyzers, saveStatesTo,">`runAnalyzersForParticularGrouping`</SwmToken>:A["Partition analyzers into shareable and others"] --> <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="298:7:7" line-data="    val results = runAnalyzersForParticularGrouping(frequenciesAndNumRows, analyzers, saveStatesTo,">`runAnalyzersForParticularGrouping`</SwmToken>:B["Cache grouped data if other analyzers are present"]
%%  <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="298:7:7" line-data="    val results = runAnalyzersForParticularGrouping(frequenciesAndNumRows, analyzers, saveStatesTo,">`runAnalyzersForParticularGrouping`</SwmToken>:B --> <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="298:7:7" line-data="    val results = runAnalyzersForParticularGrouping(frequenciesAndNumRows, analyzers, saveStatesTo,">`runAnalyzersForParticularGrouping`</SwmToken>:C["Execute aggregations for shareable analyzers"]
%%  <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="298:7:7" line-data="    val results = runAnalyzersForParticularGrouping(frequenciesAndNumRows, analyzers, saveStatesTo,">`runAnalyzersForParticularGrouping`</SwmToken>:C --> <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="298:7:7" line-data="    val results = runAnalyzersForParticularGrouping(frequenciesAndNumRows, analyzers, saveStatesTo,">`runAnalyzersForParticularGrouping`</SwmToken>:D["Compute offsets for analyzers"]
%%  <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="298:7:7" line-data="    val results = runAnalyzersForParticularGrouping(frequenciesAndNumRows, analyzers, saveStatesTo,">`runAnalyzersForParticularGrouping`</SwmToken>:D --> <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="298:7:7" line-data="    val results = runAnalyzersForParticularGrouping(frequenciesAndNumRows, analyzers, saveStatesTo,">`runAnalyzersForParticularGrouping`</SwmToken>:E["Map results to success or failure metrics"]
%%  <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="298:7:7" line-data="    val results = runAnalyzersForParticularGrouping(frequenciesAndNumRows, analyzers, saveStatesTo,">`runAnalyzersForParticularGrouping`</SwmToken>:B --> <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="298:7:7" line-data="    val results = runAnalyzersForParticularGrouping(frequenciesAndNumRows, analyzers, saveStatesTo,">`runAnalyzersForParticularGrouping`</SwmToken>:F["Execute remaining analyzers on grouped data"]
%%  <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="298:7:7" line-data="    val results = runAnalyzersForParticularGrouping(frequenciesAndNumRows, analyzers, saveStatesTo,">`runAnalyzersForParticularGrouping`</SwmToken>:F --> <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="298:7:7" line-data="    val results = runAnalyzersForParticularGrouping(frequenciesAndNumRows, analyzers, saveStatesTo,">`runAnalyzersForParticularGrouping`</SwmToken>:G["Store states if required"]
%%  <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="298:7:7" line-data="    val results = runAnalyzersForParticularGrouping(frequenciesAndNumRows, analyzers, saveStatesTo,">`runAnalyzersForParticularGrouping`</SwmToken>:G --> <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="298:7:7" line-data="    val results = runAnalyzersForParticularGrouping(frequenciesAndNumRows, analyzers, saveStatesTo,">`runAnalyzersForParticularGrouping`</SwmToken>:H["Unpersist grouped data"]
%%  <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="298:7:7" line-data="    val results = runAnalyzersForParticularGrouping(frequenciesAndNumRows, analyzers, saveStatesTo,">`runAnalyzersForParticularGrouping`</SwmToken>:H --> <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="298:7:7" line-data="    val results = runAnalyzersForParticularGrouping(frequenciesAndNumRows, analyzers, saveStatesTo,">`runAnalyzersForParticularGrouping`</SwmToken>:I["Return combined metrics"]
%%  end
```

## Identifying Shareable Analyzers

First, the function identifies all shareable analyzers by partitioning the provided analyzers into shareable and <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="341:5:7" line-data="    /* Run non-shareable analyzers separately */">`non-shareable`</SwmToken> categories. Shareable analyzers are those that can benefit from <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="491:5:7" line-data="    * applying scan-sharing where possible */">`scan-sharing`</SwmToken>, which means they can be executed more efficiently by sharing the same data scan.

## Caching Grouped Data

Next, if there are any <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="341:5:7" line-data="    /* Run non-shareable analyzers separately */">`non-shareable`</SwmToken> analyzers, the grouped data is cached to optimize performance for multiple passes. This is controlled via the storage level parameter, ensuring that the data is readily available for subsequent operations.

## Executing Shareable Analyzers

Moving to the execution of shareable analyzers, the function aggregates the necessary data and computes offsets to correctly map the results back to the analyzers. This step ensures that each analyzer can accurately pick its results from the aggregated data.

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" line="532">

---

Then, the function handles any exceptions that occur during the execution of shareable analyzers by mapping the computation results to either success or failure metrics. This ensures that any errors are captured and reported appropriately.

```scala
      } catch {
        case error: Exception =>
          shareableAnalyzers
            .map { analyzer => analyzer -> analyzer.toFailureMetric(error) }
      }
```

---

</SwmSnippet>

## Executing Non-Shareable Analyzers

Next, the function executes the remaining <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="341:5:7" line-data="    /* Run non-shareable analyzers separately */">`non-shareable`</SwmToken> analyzers on the grouped data. Each analyzer computes its metric from the provided data, ensuring that all analyzers, regardless of their shareability, are executed.

## Storing States

Finally, the function optionally stores the states of the analyzers if a state persister is provided. This allows for the persistence of intermediate states, which can be useful for future computations or debugging purposes.

## Unpersisting Data

The function then unpersists the grouped data to free up memory resources, ensuring that the system remains efficient and does not hold onto unnecessary data.

## Returning Results

The function concludes by returning an <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="69:6:6" line-data="    * @return AnalyzerContext holding the requested metrics per analyzer">`AnalyzerContext`</SwmToken> that contains the combined results of both shareable and <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="341:5:7" line-data="    /* Run non-shareable analyzers separately */">`non-shareable`</SwmToken> analyzers. This context provides a comprehensive view of the data quality metrics computed for the specific data grouping.

# Handling the computation of metrics

```mermaid
graph TD
  subgraph successOrFailureMetricFrom
    successOrFailureMetricFrom:A["Attempt to compute metric from aggregation result"] --> successOrFailureMetricFrom:B{"Was computation successful?"}
    successOrFailureMetricFrom:B -- Yes --> successOrFailureMetricFrom:C["Return computed metric"]
    successOrFailureMetricFrom:B -- No --> successOrFailureMetricFrom:D["Return failure metric"]
  end
  subgraph metricFromAggregationResult
    metricFromAggregationResult:A["Produce metric from aggregation result"] --> metricFromAggregationResult:B["Retrieve state from aggregation result"]
    metricFromAggregationResult:B --> metricFromAggregationResult:C["Calculate metric using optional state loaders"]
  end
  successOrFailureMetricFrom:A --> metricFromAggregationResult

%% Swimm:
%% graph TD
%%   subgraph <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="330:1:1" line-data="            successOrFailureMetricFrom(analyzer, results, offset, aggregateWith, saveStatesTo)">`successOrFailureMetricFrom`</SwmToken>
%%     <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="330:1:1" line-data="            successOrFailureMetricFrom(analyzer, results, offset, aggregateWith, saveStatesTo)">`successOrFailureMetricFrom`</SwmToken>:A["Attempt to compute metric from aggregation result"] --> <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="330:1:1" line-data="            successOrFailureMetricFrom(analyzer, results, offset, aggregateWith, saveStatesTo)">`successOrFailureMetricFrom`</SwmToken>:B{"Was computation successful?"}
%%     <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="330:1:1" line-data="            successOrFailureMetricFrom(analyzer, results, offset, aggregateWith, saveStatesTo)">`successOrFailureMetricFrom`</SwmToken>:B -- Yes --> <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="330:1:1" line-data="            successOrFailureMetricFrom(analyzer, results, offset, aggregateWith, saveStatesTo)">`successOrFailureMetricFrom`</SwmToken>:C["Return computed metric"]
%%     <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="330:1:1" line-data="            successOrFailureMetricFrom(analyzer, results, offset, aggregateWith, saveStatesTo)">`successOrFailureMetricFrom`</SwmToken>:B -- No --> <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="330:1:1" line-data="            successOrFailureMetricFrom(analyzer, results, offset, aggregateWith, saveStatesTo)">`successOrFailureMetricFrom`</SwmToken>:D["Return failure metric"]
%%   end
%%   subgraph <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="360:3:3" line-data="      analyzer.metricFromAggregationResult(aggregationResult, offset, aggregateWith, saveStatesTo)">`metricFromAggregationResult`</SwmToken>
%%     <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="360:3:3" line-data="      analyzer.metricFromAggregationResult(aggregationResult, offset, aggregateWith, saveStatesTo)">`metricFromAggregationResult`</SwmToken>:A["Produce metric from aggregation result"] --> <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="360:3:3" line-data="      analyzer.metricFromAggregationResult(aggregationResult, offset, aggregateWith, saveStatesTo)">`metricFromAggregationResult`</SwmToken>:B["Retrieve state from aggregation result"]
%%     <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="360:3:3" line-data="      analyzer.metricFromAggregationResult(aggregationResult, offset, aggregateWith, saveStatesTo)">`metricFromAggregationResult`</SwmToken>:B --> <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="360:3:3" line-data="      analyzer.metricFromAggregationResult(aggregationResult, offset, aggregateWith, saveStatesTo)">`metricFromAggregationResult`</SwmToken>:C["Calculate metric using optional state loaders"]
%%   end
%%   <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="330:1:1" line-data="            successOrFailureMetricFrom(analyzer, results, offset, aggregateWith, saveStatesTo)">`successOrFailureMetricFrom`</SwmToken>:A --> <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="360:3:3" line-data="      analyzer.metricFromAggregationResult(aggregationResult, offset, aggregateWith, saveStatesTo)">`metricFromAggregationResult`</SwmToken>
```

First, the function <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="330:1:1" line-data="            successOrFailureMetricFrom(analyzer, results, offset, aggregateWith, saveStatesTo)">`successOrFailureMetricFrom`</SwmToken> is responsible for computing a metric from an aggregation result. It takes several parameters including an analyzer, the aggregation result, an offset, and optional state loaders and persisters. The primary role of this function is to attempt to compute the metric using the provided analyzer and aggregation result.

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" line="359">

---

Next, within <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="330:1:1" line-data="            successOrFailureMetricFrom(analyzer, results, offset, aggregateWith, saveStatesTo)">`successOrFailureMetricFrom`</SwmToken>, the function <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="360:3:3" line-data="      analyzer.metricFromAggregationResult(aggregationResult, offset, aggregateWith, saveStatesTo)">`metricFromAggregationResult`</SwmToken> is called to produce the metric. This function processes the aggregation result, considering optional state loading and persisting. If an exception occurs during this process, <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="330:1:1" line-data="            successOrFailureMetricFrom(analyzer, results, offset, aggregateWith, saveStatesTo)">`successOrFailureMetricFrom`</SwmToken> catches the exception and converts it into a failure metric using the analyzer's <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="362:12:12" line-data="      case error: Exception =&gt; analyzer.toFailureMetric(error)">`toFailureMetric`</SwmToken> method.

```scala
    try {
      analyzer.metricFromAggregationResult(aggregationResult, offset, aggregateWith, saveStatesTo)
    } catch {
      case error: Exception => analyzer.toFailureMetric(error)
    }
```

---

</SwmSnippet>

<SwmSnippet path="/src/main/scala/com/amazon/deequ/analyzers/Analyzer.scala" line="208">

---

Diving into <SwmToken path="src/main/scala/com/amazon/deequ/analyzers/runners/AnalysisRunner.scala" pos="360:3:3" line-data="      analyzer.metricFromAggregationResult(aggregationResult, offset, aggregateWith, saveStatesTo)">`metricFromAggregationResult`</SwmToken>, this function first derives the state from the aggregation result and offset. It then calculates the metric based on this state, optionally using provided state loaders and persisters. This ensures that the metric computation is flexible and can incorporate additional state information if available.

```scala
    val state = fromAggregationResult(result, offset)

    calculateMetric(state, aggregateWith, saveStatesWith)
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBZGVlcXUlM0ElM0Fhd3NsYWJz" repo-name="deequ"><sup>Powered by [Swimm](/)</sup></SwmMeta>
