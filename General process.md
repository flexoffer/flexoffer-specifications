# Flex-Offer Lifecycle
After Flex-Offer generation at the Prosumer-side, the Flex-Offer is typically sent to a receiving party, potentially, some utility company, BRP, or Aggregator, where is takes part in flexibility negotiation, planning, control, and billing processes.

![FO Lifecycle](https://user-images.githubusercontent.com/48982460/174026894-a59a7307-38eb-4587-b8f6-cffd18affbcc.png)

-	Negotiation process The Flex-Offer can be accepted, e.g., if all of its attributes are valid and offered flexibility is valuable for the receiving party. On the other hand, the Flex-Offer can be rejected, e.g., due to some validation errors, or due to unacceptable price or energy, which then requires updating and resending the Flex-Offer or operating Prosumer processes under the default profile (baseload), e.g. not using the Flex-Offer.
-	Planning process As mentioned earlier, the Flex-Offer can be decomposed into a number of decision variables and constraints, an used in actor-specific optimization and planning process. This results into one or more Flex-Offer schedules, i.e., assignments, which respect all Flex-Offer constraints and can be executed by the Prosumer.
-	Control process Each Flex-Offer schedule (assignment) sent to a Prosumer is executed, starting at the given starting time, such that prescribed energy amounts are consumed or produced at subsequent time intervals.
-	Billing process Prosumer is rewarded by the flex-offer receiving party for its offered flexibility (Flex-Offers).
Typical message exchange between the Prosumer and Flex-Offer receiving party, covering the negotiation and planning processes, is presented below.

![FO message exchange](https://user-images.githubusercontent.com/48982460/174026946-14416c61-b850-40b9-9fbb-5d8c8ed028d2.png)

Note, the presented Flex-Offer concept and its representation were originally developed in the MIRABEL and TOTALFLEX projects. 
