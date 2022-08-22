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
|id | Yes | Integer| |			
|state|Yes|String|Possible values: initial/ offered/ accepted/ rejected/assigned/ executed/ invalid/canceled| 
|stateReason|No|String||
|offeredById|Yes|Timestamp||
|acceptBeforeTime|No|Timestamp||
|acceptBeforeInterval|No|Integer|Seconds before startAfterTime|
|assignmentBeforeInterval|No|Integer|Seconds before startAfterTime|
|assignmentBeforeTime|No|Timestamp||
|creationTime|Yes|Timestamp||
|endAfterTime|No|Timestamp||
|endBeforeTime|No|Timestamp|
|numSecondsPerInterval|No|Integer|Default:900|
|startAfterTime|No|Timestamp|If not explicated: current time|
|startBeforeTime|No|Timestamp|If not explicated: current time|
|totalEnergyConstraint|No|List of parameters|
|subTotalEnergyConstraint|No|List of parameters|
|powerFactorConstraint|No|List of parameters|
|totalCostConstraint|No|List of parameters|
|flexOfferProfileConstraints|Yes|Array|Null value or empty list means flexibility removal|
|flexOfferSchedule|Yes|List of parameters|Only in the response to an offer|
|defaultSchedule|No|List of parameters|
|locationId|No|String||
|functionTest|No|String||
|currency|No|String||
|flexOfferType|No|String||
|assignment|No|String||
|accountId|No|String||
|unit|No|String||
|multiplier|No|String||


|flexOfferProfileConstraints array element|Mandatory|Type|Comment|
|---------| ----|-------|---|
|tariffConstraint|No|List of parameters||
|energyConstraintList|Yes|Array||	
|minDuration|No|Integer||	
|maxDuration|No|Integer||	

|totalEnergy/subTotalEnergy/totalCost/powerFactor/Constraint parameters|Mandatory|Type|Comment|
|---------| ----|-------|---|
|lower|Yes|Float|	
|upper|Yes|Float|	

|energyConstraintList Array element|Mandatory|Type|Comment|
|---------| ----|-------|---|
|lowerBound|Yes|Float|	
|upperBound|Yes|Float|	
|phases|||||

|flexOfferTariffConstraint parameters|Mandatory|Type|Comment|
|---------| ----|-------|---|
|tariffSlices|Yes|Array|	
|StartTime|No|Timestamp|	

|tariffSlices array element|Mandatory|Type|Comment|
|---------| ----|-------|---|
|duration|No|Integer||
|tarriffConstraint|Yes|List of parameters||	

|tariffConstraint parameters|Mandatory|Type|Comment|
|---------| ----|-------|---|
|minTariff|Yes|Float||	
|maxTariff|Yes|Float||	

|flexOfferSchedule parameters|Mandatory|Type|Comment|
|---------| ----|-------|---|
|startTime|Yes|Timestamp|
|scheduleId|No|Integer|
|scheduleId|No|Integer|
|scheduleSlices|Yes|Array|

|scheduleSlices array element|Mandatory|Type|Comment|
|---------| ----|-------|---|
|duration|No|Integer|
|energyAmount|Yes|Float|
|tariff|No|Float|

|defaultSchedule|Mandatory|Type|Comment|
|---------| ----|-------|---|
|startTime|Yes|Float|
|scheduleSlices|Yes|Array|

### Constraints
Several constraints can be included in the Flex Offer message: 
|Constraint|Mandatory|Type|Use|Scope|
|-------|-------|----|-------|---|
|Start time constraint|Yes|Range: startAfterTime, startBeforeTime|Defines the time range within which the offer can be activated|Whole offer|
|Energy amount constraint|Yes|Range: energyConstraintList[i].lowerBound, energyConstraintList[i].upperBound OR List of floats|For every discrete interval of an active device operation, energy amount flexibility is characterized by a range|Time slice|
|Total energy constraint|No|Range: [totalEnergyConstraint.lower, totalEnergyConstraint.upper]|Bounds the total energy amount requested or offered within the full active operation of a flexible resource|Whole offer|
|Sub Total energy constraint|No|Range: [subTotalEnergyConstraint.lower, subTotalEnergyConstraint.upper]|Bounds the total available energy amount requested or offered within the full active operation of a flexible resource|Whole offer|
|Dependent energy amount constraint|No|2D energy flexibility polygon|Minimum and maximum energy amounts at a discrete interval t depending on the total energy consumed at the intervals 1..t-1. This constraint should be used for the most advanced forms of flexible resources (e.g., heat-pumps), where the flexibility changes over time and is dependent on an internal system state (e.g., temperature).|Time slice|
|Acceptance time constraint|No|Time value: acceptanceBeforeTime|Sets the deadline on when a flex-offer receiving party (e.g., BRP) should acknowledge successful acceptance or rejection of the flex-offer. A flex-offer rejection may occur if, e. g., flex-offer constraints or other metadata (e.g., prices) are invalid or inappropriate (e.g., quantities are too small, prices are too high).|Whole offer|
|Assignment time constraint|No|Absolute timestamp (assignmentBeforeTime) OR relative duration (assignmentBeforeDurationSeconds)|Sets the deadlines on when flex-offer schedule update (assignment) is allowed to be sent by the flex-offer receiving party (BRP) to a flex-offer issuing party (flexible resource).|Whole offer|

### Json representation example  

<img width="500" alt="Example FO2" src="https://user-images.githubusercontent.com/48982460/177325083-38401b22-20ac-4074-bccb-6d585ba58fdf.png">



 
## xEMS-FOA exchange protocol

This protocol is not part of FlexOffer. However, an optional library of API/adapters will be made available open-source to facilitate the adoption of FO. Reference installations will also be described.

