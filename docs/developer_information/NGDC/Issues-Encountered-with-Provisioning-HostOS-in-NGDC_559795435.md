---
layout: default
title:  Provisioning Issues in NGDC 
parent: NGDC
grand_parent: Developer Information
---


# Issues Encountered with Provisioning HostOS in NGDC


Various issues came up that affected our ability to provision with FAST\-DS

* **March 4,** fast DS bug:

[https://ibm\-cloudplatform.slack.com/archives/C04AYBXJGAC/p1709579436111619?thread\_ts\=1709579412\.396189\&cid\=C04AYBXJGAC](https://ibm-cloudplatform.slack.com/archives/C04AYBXJGAC/p1709579436111619?thread_ts=1709579412.396189&cid=C04AYBXJGAC)  
  


* **March 13,** rk46 s02 node naming issue (multiple times)

 [https://ibmcloudlab.slack.com/archives/C01L963NZ1D/p1711065556136159?thread\_ts\=1711040562\.091699\&cid\=C01L963NZ1D](https://ibmcloudlab.slack.com/archives/C01L963NZ1D/p1711065556136159?thread_ts=1711040562.091699&cid=C01L963NZ1D)  
  


* **March 15**, rk48 s04 node naming issue IMS record

[https://ibm\-cloudplatform.slack.com/archives/C06MQSERL9F/p1710525107824929?thread\_ts\=1710519063\.058949\&cid\=C06MQSERL9F](https://ibm-cloudplatform.slack.com/archives/C06MQSERL9F/p1710525107824929?thread_ts=1710519063.058949&cid=C06MQSERL9F)

  


* March 20, Ipmi ssue, preventing boot\-up.
* First, [https://ibm\-cloudplatform.slack.com/archives/C04AYBXJGAC/p1710960938501689?thread\_ts\=1710960902\.784109\&cid\=C04AYBXJGAC](https://ibm-cloudplatform.slack.com/archives/C04AYBXJGAC/p1710960938501689?thread_ts=1710960902.784109&cid=C04AYBXJGAC)
* Then, <https://ibmcloudlab.slack.com/archives/C01L963NZ1D/p1710961926417639>
* And finally, [https://ibm\-cloudplatform.slack.com/archives/C068LLL1LCE/p1710968421753529](https://ibm-cloudplatform.slack.com/archives/C068LLL1LCE/p1710968421753529), it just started working.
* **March 25,** Bond interface member/ip changed on rk46 s02, affecting which MAC address is used to pxe\-boot

[https://ibm\-cloudplatform.slack.com/archives/C04AYBXJGAC/p1711398069052369?thread\_ts\=1711121233\.493429\&cid\=C04AYBXJGAC](https://ibm-cloudplatform.slack.com/archives/C04AYBXJGAC/p1711398069052369?thread_ts=1711121233.493429&cid=C04AYBXJGAC)

  


* **April 2,** FAST issue, timeout hostos too early, and other issue with the ape server?

[https://ibm\-cloudplatform.slack.com/archives/C04AYBXJGAC/p1712002457176129?thread\_ts\=1712000801\.067019\&cid\=C04AYBXJGAC](https://ibm-cloudplatform.slack.com/archives/C04AYBXJGAC/p1712002457176129?thread_ts=1712000801.067019&cid=C04AYBXJGAC)

  


* **April 9,** nextgen routing problem with .23 and .24 version on rk23 control nodes
	+ HostOS testing by provisioning each bundle at a time. to find that the issue is with nextgen .23 and .24\.
	+ April 18, rk23 nodes are pre\-elba designed. It needs to be upgraded to have the same configuration as rk24\.

  


* **April 16,** ns3 bundle failed to installed reported to Jose  

	+ April 17 Jose pulled Amit in to help look:  [https://ibmcloudlab.slack.com/archives/C01L963NZ1D/p1713381754049519?thread\_ts\=1713360298\.222389\&cid\=C01L963NZ1D](https://ibmcloudlab.slack.com/archives/C01L963NZ1D/p1713381754049519?thread_ts=1713360298.222389&cid=C01L963NZ1D)
	+ April 17 Asked FAST to look into why docker is failing to pull the ns3 bundle: [https://ibm\-cloudplatform.slack.com/archives/C04AYBXJGAC/p1713384660510459?thread\_ts\=1713381979\.733009\&cid\=C04AYBXJGAC](https://ibm-cloudplatform.slack.com/archives/C04AYBXJGAC/p1713384660510459?thread_ts=1713381979.733009&cid=C04AYBXJGAC)
	+ April 17 Asked FAST to look into a stuck work flow: [https://ibm\-cloudplatform.slack.com/archives/C04AYBXJGAC/p1713396161059259?thread\_ts\=1713393483\.144279\&cid\=C04AYBXJGAC](https://ibm-cloudplatform.slack.com/archives/C04AYBXJGAC/p1713396161059259?thread_ts=1713393483.144279&cid=C04AYBXJGAC)
	+ April 18 retried provisioining and ns3 bundle provisioning error: [https://ibmcloudlab.slack.com/archives/C01L963NZ1D/p1713479408211879?thread\_ts\=1713472141\.127909\&cid\=C01L963NZ1D](https://ibmcloudlab.slack.com/archives/C01L963NZ1D/p1713479408211879?thread_ts=1713472141.127909&cid=C01L963NZ1D)
		- related to hostnaming (see march 15 above)

  


* **April 29,** FAST and NCS issue preventing provisioning on mgmt node
	+ workflow stuck in waiting for ip: [https://ibm\-cloudplatform.slack.com/archives/C04AYBXJGAC/p1714421850495369?thread\_ts\=1714420010\.263269\&cid\=C04AYBXJGAC](https://ibm-cloudplatform.slack.com/archives/C04AYBXJGAC/p1714421850495369?thread_ts=1714420010.263269&cid=C04AYBXJGAC)
	+ resumitted and contacted NCS: [https://ibm\-cloudplatform.slack.com/archives/C06MQSERL9F/p1714427695299579?thread\_ts\=1714423522\.605159\&cid\=C06MQSERL9F](https://ibm-cloudplatform.slack.com/archives/C06MQSERL9F/p1714427695299579?thread_ts=1714423522.605159&cid=C06MQSERL9F)
	+ Shireesh hit similar issue: [https://ibmcloudlab.slack.com/archives/C06633AKE74/p1714427886531899?thread\_ts\=1714409429\.637889\&cid\=C06633AKE74](https://ibmcloudlab.slack.com/archives/C06633AKE74/p1714427886531899?thread_ts=1714409429.637889&cid=C06633AKE74)
	+ there is a [NFP\-398](https://jiracloud.swg.usma.ibm.com:8443/browse/NFP-398) files for intermitten network problem. ping dropped.



 


Document generated by Confluence on Jul 15, 2024 13:04


[Atlassian](https://www.atlassian.com/)


 


