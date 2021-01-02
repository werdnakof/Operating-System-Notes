---


---

<p>##Outage / Activity Details<br>
Detailed activity procedures can be added at the step level - just provide a general overview here.<br>
This activity is to dial up the following weblab:<br>
<a href="https://weblab.amazon.com/#%7B%7BWEBLAB_NAME%7D">https://weblab.amazon.com/#{{WEBLAB_NAME}</a>}<br>
in {{DIALUP_REALMS}}</p>
<p>####What is the purpose of this activity or change?<br>
The MCM is written in preparation for an emergency dialing down for Order Status Home Cards to only beta customers during Christmas peak.</p>
<p>####What will be required to execute this change?<br>
Dialing down this weblab so that the Production domain has {{TARGET_TREATMENT_CODE}} set to 100%.</p>
<p>####What is the expected end state of the system after this change?<br>
AlexaShoppingOrderStatusHomeScreenService should only consume order, shipments and display settings related events for beta customers, and only publish home cards to AlexaMMHomePublishingService for beta customers subsequently.</p>
<p>####What assumptions, if any, are being made about the state of the system at the time of this change?<br>
The code for the new experiences is deployed to prod, and the weblab is dialed up in prod.</p>
<p>See Slapshot events contracts which are gated by weblab:<br>
<a href="https://slapshot.integ.amazon.com/slapshot-dashboard.html#/slap/ASMM/AlexaShoppingOrderStatusHomeScreen/ASNSSettingsUpdate">https://slapshot.integ.amazon.com/slapshot-dashboard.html#/slap/ASMM/AlexaShoppingOrderStatusHomeScreen/ASNSSettingsUpdate</a><br>
<a href="https://slapshot.integ.amazon.com/slapshot-dashboard.html#/slap/ASMM/AlexaShoppingOrderStatusHomeScreen/ShipTrackEventProcessor">https://slapshot.integ.amazon.com/slapshot-dashboard.html#/slap/ASMM/AlexaShoppingOrderStatusHomeScreen/ShipTrackEventProcessor</a><br>
<a href="https://slapshot.integ.amazon.com/slapshot-dashboard.html#/slap/ASMM/AlexaShoppingOrderStatusHomeScreen/OrderStatusOwenFilter">https://slapshot.integ.amazon.com/slapshot-dashboard.html#/slap/ASMM/AlexaShoppingOrderStatusHomeScreen/OrderStatusOwenFilter</a></p>
<p>##Impact / Risk Assessment<br>
####Why is it necessary? What is the impact of not making this change?<br>
Anchor team is concerned with a large amount of new devices coming online during Christmas period, they may get a burst of traffic that would exceed their estimated TPS. They have requests that in the event they start to see some customer impact, Alexa Shopping Multimodal Experiences team reduces AlexaOrderStatusHomeScreenService’s traffic sent to them as we are not yet publishing the card to customers. Dialing down our Production weblab would reduce their load without affecting our Beta customers.</p>
<p>####Why is this the correct time/day to complete the CM?<br>
There is minimal impact, as the feature behind this weblab has been tested in prod via overrides.<br>
Time period has been chosen to cover the entire christmas period.</p>
<p>####Are there any related, prerequisite changes upon which this CM hinges?<br>
No</p>
<p>####Will this CM be in any way intrusive, and if so, how will you know? What teams, services or functionality will be impacted?<br>
Intrusive only to Anchor’s AlexaMMHomePublishingService</p>
<p>####Have you confirmed that this will not impact “Where’s My Stuff” or other features that share dependencies?<br>
They are on the approvers list.</p>
<p>####Is this change modifying a tier-1 service or website? Which one?<br>
No services being modified are tier-1.</p>
<p>####How has this change been tested to verify it’s safe for production?<br>
Manually tested in beta, gamma and in prod using weblab overrides.<br>
Manually tested in beta, gamma and in prod using weblab overrides. Our Beta customers have their Order Status events already being sent to the system.</p>
<p>##Affected Services<br>
AlexaMMHomePublishingService</p>
<p>####Does this change involve other dependencies? Are they aware of this CM and on the cc-list or approvals for it?<br>
Yes. An email will be sent out to all depenedency owners to notify them of the possibility of our<br>
AlexaMMHomePublishingService</p>
<p>####Provide links to your dependencies dashboard(s)</p>
<ul>
<li>Home Screen Order Status feature dashboard including AWS stack: <a href="https://w.amazon.com/bin/view/Alexa_Shopping/MultiModalExperiences/Order_Status_Home_Screen/Dashboard">https://w.amazon.com/bin/view/Alexa_Shopping/MultiModalExperiences/Order_Status_Home_Screen/Dashboard</a></li>
<li>AlexaMMHomePublishingService: <a href="https://w.amazon.com/index.php/Digital/Alexa/Anchor/AlexaMMHomePublishingService/Dashboard/NA/Prod">https://w.amazon.com/index.php/Digital/Alexa/Anchor/AlexaMMHomePublishingService/Dashboard/NA/Prod</a></li>
<li>Alexa Shopping MultiModal Experiences: <a href="https://w.amazon.com/bin/view/Alexa_Shopping/MultiModalExperiences/Order_Status_Home_Screen/Dashboard/">https://w.amazon.com/bin/view/Alexa_Shopping/MultiModalExperiences/Order_Status_Home_Screen/Dashboard/</a></li>
</ul>
<p>##Worst Case Scenario<br>
####What could happen if everything goes wrong with this change?<br>
&lt;WHAT OTHER SIDE EFFECTS ARE THERE?&gt;<br>
If we discover an unintended side effect of dialling up the changes, we can potentially impact Anchor’s service’s ability to provide home cards for Alexa customers.</p>
<p>####How does this CM attempt to mitigate this risk?<br>
The CM only includes a minimal set of changes, and it only involves weblab dialup.<br>
&lt;Any other risks that can be mitigated?&gt;</p>
<p>##Rollback Procedure<br>
####Detailed rollback procedures can be added at the step level - just provide a general overview here.<br>
Dial back down the weblabs so that #{{ORIGINAL_TREATMENT_CODE}} is in 100% for prod.</p>
<p>####Are there any cases that weblab dial down will cause system to fail?<br>
&lt;Is your code already dealing with weblab switches, if dial up then dial down can possibly case system to fail, rethink your code before actually proceeding with this CM&gt;</p>
<p>###What conditions would indicate a need to rollback? Provide graphs with rollback thresholds marked, or other clear indicators to the technicians when a rollback is required</p>
<p>Monitoring below metrics and alarms:</p>
<ul>
<li>Home Screen Order Status feature dashboard including AWS stack: <a href="https://w.amazon.com/bin/view/Alexa_Shopping/MultiModalExperiences/Order_Status_Home_Screen/Dashboard">https://w.amazon.com/bin/view/Alexa_Shopping/MultiModalExperiences/Order_Status_Home_Screen/Dashboard</a></li>
<li>Alexa Shopping Multi Model Experiences TT queue: <a href="https://tiny.amazon.com/1e634h9ha/tcorpamazissu">https://tiny.amazon.com/1e634h9ha/tcorpamazissu</a></li>
</ul>
<p>If any alarms are triggered at Sev 2, 2.5 we will roll back. &lt;CONSIDER IF THERE ARE ANY SEV 3s THAT SHOULD BE ATTACHED TO THE MCM AND ALSO TRIGGER A ROLLBACK&gt;<br>
&lt;ENSURE ALL ALARMS FOR DEPENDENCIES THAT ARE IMPORTANT ARE EITHER ATTACHED TO THE MCM OR CLONED TO OUR CTI, List specific monitors for this weblab dial up and add them below&gt;</p>
<p>####In the event of problems, what will you do to return your system to a known good state?<br>
Dial back down the weblab.<br>
&lt;ARE THERE ANY OTHER STEPS REQUIRED?&gt;</p>
<blockquote>
<p>Written with <a href="https://stackedit.io/">StackEdit</a>.</p>
</blockquote>

