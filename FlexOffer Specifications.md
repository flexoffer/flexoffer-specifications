# FlexOffer specifications


## FlexOffer message 

This message is the core of the Flex Offer protocol. It is exchanged between the flexibility trader and the flexibility providers / users. A range of optional parameters can be used to give indications on constraints and to be used in the different steps of the flexibility trading process.
Depending on the parameters used, it can therefore be used to 
-	Offer a flexibility bid
-	Accept or refuse a flexibility offer
-	Assign a flexibility
It offers a common representation of all flexibilities, based on time slices and optional constraints.

### Parameters
Here is the list of parameters to be included in the FlexOffer Message

|Parameter|Mandatory | Type | Comment|
|---------|--------- | ---- | -------|
|priority | No | Integer|0 = emergency, 1 = cheapest price, ...; default = 0|			
|state|Yes|String enumerator|enumerator: 0 = not available; 1 = online, system in operation, no flexibility provided; 2 = available for adaptation; 3 = waiting for execution; 4 = in adaptation| 
|expiryTime|No|Timestamp|After this time, the offer is considered as not valid anymore.|
|assignmentBeforeDurationSeconds|No|Integer|Minimum time between the assignment message (reception of REPLY Flex Agent to xEMS with parameter demandSchedule not empty) and start of adaptation (parameter demandSchedule.startTime in REPLY Flex Agent to xEMS).|
|assignmentBeforeTime|No|Timestamp|Last time when the flexibility activation can begin |
|creationTime|Yes|Timestamp||
|durationSeconds|Yes|Integer||
|endAfterTime|Yes|Timestamp||
|endBeforeTime|Yes|Timestamp|
|numSecondsPerInterval|Yes|Integer|
|startAfterTime|No|Timestamp|If not explicated: current time|
|startBeforeTime|No|Timestamp|If not explicated: current time|
|totalEnergyConstraint|No|List of parameters|
|slices|Yes|List of parameters|
|flexOfferSchedule|Yes|List of parameters|
|defaultSchedule|Yes|List of parameters|

|slices|parameters|Mandatory|Type|Comment|
|---------|---------| ----|-------|---|
|durationSeconds|Yes|Integer|
|costPerEnergyUnitLimit|No|Integer||
|energyConstraint|Yes|List of parameters||	

|energyConstraint|parameters|Mandatory|Type|Comment|
|---------|---------| ----|-------|---|
|lower|Yes|Float|	
|upper|Yes|Float|	

|flexOfferSchedule|parameters|Mandatory|Type|Comment|
|---------|---------| ----|-------|---|
|startTime|Yes|Float|
|energyAmounts|Yes|List of Float|

|defaultSchedule|parameters|Mandatory|Type|Comment|
|---------|---------| ----|-------|---|
|startTime|Yes|Float|
|energyAmounts|Yes|List of Float|

### Constraints
Several constraints can be included in the Flex Offer message: 
|Constraint|Mandatory|Type|Use|Scope|
|-------|-------|----|-------|---|
|Start time constraint|Yes|Range: startAfterTime, startBeforeTime|Defines the time range within which the offer can be activated|Whole offer|
|Energy amount constraint|Yes|Range: energyConstraint.lower, energyConstraint.upper OR List of floats|For every discrete interval of an active device operation, energy amount flexibility is characterized by a range|Time slice|
|Total energy constraint|No|Range: [totalEnergyConstraint.lower, totalEnergyConstraint.upper]|Bounds the total energy amount requested or offered within the full active operation of a flexible resource|Whole offer|
|Dependent energy amount constraint|No|2D energy flexibility polygon|Minimum and maximum energy amounts at a discrete interval t depending on the total energy consumed at the intervals 1..t-1. This constraint should be used for the most advanced forms of flexible resources (e.g., heat-pumps), where the flexibility changes over time and is dependent on an internal system state (e.g., temperature).|Time slice|
|Acceptance time constraint|No|Time value: acceptanceBeforeTime|Sets the deadline on when a flex-offer receiving party (e.g., BRP) should acknowledge successful acceptance or rejection of the flex-offer. A flex-offer rejection may occur if, e. g., flex-offer constraints or other metadata (e.g., prices) are invalid or inappropriate (e.g., quantities are too small, prices are too high).|Whole offer|
|Assignment time constraint|No|Absolute timestamp (assignmentBeforeTime) OR relative duration (assignmentBeforeDurationSeconds)|Sets the deadlines on when flex-offer schedule update (assignment) is allowed to be sent by the flex-offer receiving party (BRP) to a flex-offer issuing party (flexible resource).|Whole offer|

Given any (prosumer) load instance, load flexibility modelling should consider (and use) simple Flex-Offer constraints first before considering more advanced ones, following this algorithm:
-	Step 1, Use Flex-Offer energy flexibility first (energyConstraint.lower - energyConstraint.upper). If sufficient flexibility is captured, stop. Otherwise - go to Step 2.
-	Step 2, If the load/process exhibits time flexibility, use time flexibility (startAfterTime - StartEndTime). Use implicit time flexibility if needed (see above). If sufficient flexibility is captured, stop. Otherwise - go to Step 3.
-	Step 3, If the load/process requires a fixed or flexible amount of energy delivered by the end of operation, use so-called total energy constraints (totalEnergyConstraint.lower - totalEnergyConstraint.upper) constraining the total energy of the operation. If sufficient flexibility is captured, stop. Otherwise - go to Step 4.
-	Step 4 If the load exhibits dependencies of energy amounts across time intervals, use the dependency-Flex-Offer (first choice) or polytopic constraints (second choice). If sufficient, stop; otherwise – goto Step 5.
-	Step 5 if the load exhibit discrete alternatives, use the two-mode conditional operation (first choice) or XOR profiles (second choice).

### Json representation example  

<img width="500" alt="Example FO2" src="https://user-images.githubusercontent.com/48982460/177325083-38401b22-20ac-4074-bccb-6d585ba58fdf.png">



 
## xEMS-FOA exchange protocol

This protocol is not part of FlexOffer. However, an optional library of API/adapters will be made available open-source to facilitate the adoption of FO. Reference installations will also be described.

