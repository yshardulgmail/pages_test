---
layout: default
title:  Pod to Pod traffic restriction 
parent: Planning
---


## Pod to pod network traffic restriction (11\.x network)
 

PCE  Extension 1 year : 2024\-05\-02 to 2025\-05\-02

  


* Compute node
	+ All done  
	
		- firewall restriction  in place
* Control nodes \- 3 months efforts ( PCE )
	+ Rules added
	+ we are watching packet drops
	+ 30% percent
* Master nodes \- 2 months  efforts ( PCE )
	+ Under evaluation
* Edge nodes \- 4 months efforts ( PCE )
	+ Under evaluation
* Service nodes \- 2 months efforts ( PCE )   

	+ Under evaluation
* Observability node \- 1 month efforts ( PCE )
	+ Basic ruleset added
	+ 80%  Done

### Plan

  




| Phase | Description | Date | Notes |
| --- | --- | --- | --- |
| **Done** |  |  |  |
| **Phase 1** | Pod to pod network flow data collection |  | Going through logs, watching and understanding all different flows. |
|  | Pre\-int/int | 13 Jul 2022 | Gathered pod traffic for  Pre\-int/int. |
|  | Staging | 27 Jul 2022 | Gathered  staging  logs. |
|  | Prod ( canary ) | 17 Aug 2022 | Gathered SYD pod network data. |
| **Phase 2** | Initial set of rules with logging enable |  | Add rules which will audit the traffic. Will not block it. |
|  | pre\-int/int | 31 Aug 2022 | External traffic identification and classification  to 443 port 1. Amazon 2. Softalyer 3. Canonical Identified pod, Listed pod's communicating as per above classification. |
|  | Staging | 14 Sep 2022 | Forward rules changes logged.verified HOSTOS\-FORWARD traffic with config .85 |
|  | Prod ( canary ) \-SYD | 05 Oct 2022 | Forward rules changes logged to SYD ( hostOS .79\) |
|  |  | 18 Nov 2022 | Config .87 rollout with rule changes and external traffic identification |
| **In Progress** |  |  |  |
| **Phase 3** | Drop traffic \- Compute ONLY |  | Default drop only allow by rule \- Compute node type ONLY |
|  | Dev Zone | 12 Jan 2023 | chicken switch implementation in config to Compute node Drop Done, Rolled out with 27 Oct 2022 3\.5\.92\-20221027T120450Z\_192739a2\.5\.92\-20221027T120431Z\_4aa6900   5\.5\.92\-20221027T120529Z\_27f0184Switch implemented too [https://github.ibm.com/cloudlab/platform\-inventory/pull/10332/files](https://github.ibm.com/cloudlab/platform-inventory/pull/10332/files) |
|  | pre\-int/int | 15 Feb 2023 | .100  Default DROP Compute. |
|  | Staging | 28 Feb 2023 | .100  Default DROP Compute. |
|  | Prod ( canary ) \- SYD | 27 Mar 2023 | .102  Default DROP Compute. * Update Switch \- Platform inventory update * Watch Traffic, Verified nothing important is blocked |
|  | Prod SAO | 05 Apr 2023 | .102  Default DROP Compute.Completed with config 102 on 03 Apr 2023 |
|  | Prod OSA | 12 Apr 2023 | .102  Default DROP Compute. Completed with config 102 on 05 Apr 2023 |
|  | Prod  LON | 19 Apr 2023 | .102  Default DROP Compute.Completed with config 102 on 05 Apr 2023 |
|  | Prod FRA | 26 Apr 2023 | .102  Default DROP Compute.Completed with config 102 on  07 Apr 2023 |
|  | Prod TOR | 03 May 2023 | .102  Default DROP Compute. Completed with config 102 on 11 Apr 2023 |
|  | Prod WDC | 10 May 2023 | .102  Default DROP Compute. Completed with config 102 on 10 Apr 2023 |
|  | Prod DAL | 14 May 2023 | .102  Default DROP Compute. |

  

After Compute node type is completed we will move to other node types (Control, Master, Edge, etc.)

 


