---
layout: default
title: LogDNA 
parent: Logging
grand_parent: Developer Information
---

#  LogDNA queries


 

## Sample Python Script
This [python](Python-script-used-to-query-logDNA_194216567.html) script can be used to query logDNA.

Found this in a [public IBM git repo](https://www.ibm.com/cloud/blog/search-logdna-records-from-the-command-line)

```bash
#!/usr/bin/python3  
# Simple Python script to search LogDNA entries and thereby exporting them  
#  
# Written by Henrik Loeser, IBM, [hloeser@de.ibm.com](mailto:hloeser@de.ibm.com)  
  
import requests, json, sys  
from datetime import timedelta, datetime, tzinfo, timezone  
  
# load the configuration file with region and service key info  
def loadConfig(filename):  
    with open(filename) as data_file:  
        logConfig = json.load(data_file)  
    return logConfig  
  
# perform the actual search  
def getLogs(region, key, numHours, query):  
    url     = "https://api."+region+".logging.cloud.ibm.com/v1/export"  
    params = {  
        "from": datetime.timestamp(datetime.now())-(3600*int(numHours)),  
        "to": datetime.timestamp(datetime.now()),  
        "query": query,  
        "size": "10000"  
        }  
    response  = requests.get( url, auth=(key,''), params=params)  
    # the result are lines of JSON, so store them individually  
    try:  
        result = [json.loads(jline) for jline in response.text.splitlines()]  
    except:  
        print(response)  
        result = response  
    return result  
  
def printHelp(progname):  
    print ("Usage: "+progname+" config [hours] [query]")  
    print ("   config: name of configuration file")  
    print ("   hours:  number of hours back from now")  
    print ("   query:  search term(s) as string")  
  
# Get started by going over the parameters  
if __name__== "__main__":  
    if (len(sys.argv)<2):  
        printHelp(sys.argv[0])  
        exit()  
    else:  
        # we definitely expect a config file  
        logConfig=loadConfig(sys.argv[1])  
        # no query by default  
        query=""  
        # 48 hours by default  
        numHours=48  
    if (len(sys.argv)==3):  
        numHours=sys.argv[2]  
    elif (len(sys.argv)==4):  
        numHours=sys.argv[2]  
        query=sys.argv[3]  
    else:  
        printHelp(sys.argv[0])  
        exit()  
  
    # Obtain the LogDNA logs  
    logs=getLogs(logConfig["region"],logConfig["key"],numHours, query)  
    # Because the result are loglines (JSONL), we need to dump each one individually  
    try:  
        [print (json.dumps(jsonl, indent=2)) for jsonl in logs]  
    except:  
        print('\nUnable to print json\n',logs)  
Jeffreys-MBP:logdna jeffreyjzarend$ 
```



## LogDNA Api Keys 
  


It requires logDNA api keys which can be obtained from Telemetry team


To request api keys:    [Requesting LogDNA Service API Keys](https://confluence.swg.usma.ibm.com:8445/display/TEL/Requesting+LogDNA+Service+API+Keys)

```bash
Jeffreys\-MBP:logdna jeffreyjzarend$ cat [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json

{

  "region": "us\-south",

  "key": "\<key\>"

}

  


Jeffreys\-MBP:logdna jeffreyjzarend$ cat [logConfig.us](http://logConfig.us)\-south\-qz1\.json

{

  "region": "us\-south",

  "key": "\<different key\>"

}

  


Jeffreys\-MBP:logdna jeffreyjzarend$ cat [logConfig.us](http://logConfig.us)\-east\-qz1\.json

{

  "region": "us\-east",

  "key": "\<yet even another differnt key\>"

}

  


Jeffreys\-MBP:logdna jeffreyjzarend$ 


```
 

You need a different key for each region you intend to query.  dal\-sts\-qz2 (dev \& stage) also needs a key.  I have keys for all they prod regions (us\-east, us\-south, jp\-osa, gb\-lon, de\-fra, etc) plus dal\-sts\-qz2

  


  

## Sample logDNA command line query arguments
You pass in query args on the command line

|  |  |  |
| --- | --- | --- |
|  | "hostos" | all log entries generated by hostos |
|  | 'hostosipv4 AND 431662e  AND \-OUTDROP  AND \-NOISE2 AND \-PROTO\=ICMP AND \-SPT\=6443 AND \-DPT\=6443' | iptables rules from bundle 431662e |
|  | "user\= AND command\= \-sysop" | user commands not entered by sysop |
|  | "hostos\_logipv4\_INP AND \-NOISE" | iptable rules with out noise |
|  | "7203\_4c656662\-1c56\-4e94\-9e0c\-d146841d210e app:instance\-spec" | VSI related |
|  | "hostosipv4 AND ed9896f" | iptables rules for ipv4 \& bundle id |
|  | "ed9896f AND ipset" | configuring iptables rules |

Basically, anything you might try in the GUI, you can pass in as a command line query argument


  


  
## Sample command line logDNA execution runs


You run the script, pass in the number of historical hours back to search, and it returns json

"history" output showing various runs  
  
```bash
  515  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 96 "hostos" \> hostos\-example\-1\.json

  518  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 330 'hostosipv4 AND 431662e  AND \-OUTDROP  AND \-NOISE2 AND \-PROTO\=ICMP AND \-SPT\=6443 AND \-DPT\=6443' \> hostos\-example\-2\.json 

  532  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 330 'hostosipv4 AND 431662e  AND \-OUTDROP  AND \-NOISE2 AND \-PROTO\=ICMP AND \-SPT\=6443 AND \-DPT\=6443' \> hostos\-example\-2\.json 

  534  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 330 'hostosipv4 AND \-OUTDROP  AND \-NOISE2 AND \-PROTO\=ICMP AND \-SPT\=6443 AND \-DPT\=6443' \> hostos\-example\-2\.json 

  550  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 'hostosipv4 AND ed9896f'

  551  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 24 ‘ed9896f’.  \| grep config\_ipset

  552  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 24 ‘ed9896f’. 

  553  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 24 ‘ed9896f’

  554  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 ‘ed9896f’

  555  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 "ed9896f"

  556  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 "hostosipv4 AND ed9896f"

  557  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 "ed9896f" \| grep ipset

  558  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 "ed9896f" \| grep ipset \| wc \-l

  559  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 "ed9896f" \| grep config\_ipset \| wc \-l

  560  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 "ed9896f" \| grep config\_ipset 

  561  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 "ed9896f" \| kernel

  563  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 "ed9896f" \| grep kernel

  568  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 "ed9896f"

  569  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 "ed9896f" \| grep kernel

  570  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 "ed9896f" \| grep ed9896f

  571  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 "ed9896f" \| grep ed9896f \| grep \-v uid

  572  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 "ed9896f" \| grep ed9896f \| grep \-v uid \| grep sudo

  573  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 "ed9896f" \| grep ed9896f \| grep \-v uid \| grep \-v sudo

  574  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 "ed9896f" \| grep ed9896f \| grep \-v uid \| grep \-v config

  575  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 "hostos"

  576  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 "hostos" \| grep '\_line'

  577  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 "hostosipv4"

  578  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 "hostosipv4" \| grep '\_line'

  579  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 "hostosipv4" \| grep '\_account'

  580  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 "kern"

  581  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 "kern" \| grep '\_account'

  582  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 "kern"

  583  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 'kern.log'

  584  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 'unreadable'

  585  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 'unreadable' \| grep message

  586  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 'unreadable' \| grep message \| sort \| uniq \-c

  587  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 'kern.log'

  588  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 'r134\-49bbb7fe\-07c5\-412f\-b65f\-6dfed7d297ce'

  595  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 'r134\-49bbb7fe\-07c5\-412f\-b65f\-6dfed7d297ce' \| grep \-i error

  597  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 'r134\-49bbb7fe\-07c5\-412f\-b65f\-6dfed7d297ce' \| grep \-i warn

  598  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 'r134\-49bbb7fe\-07c5\-412f\-b65f\-6dfed7d297ce' \| grep \-i crit

  599  python3 searchLogDNA.py [logConfig.us](http://logConfig.us)\-south\-qz2\-sts.json 44 'r134\-49bbb7fe\-07c5\-412f\-b65f\-6dfed7d297ce' \| grep \-i pending  
  
Jeffreys\-MBP:logdna jeffreyjzarend$ cat 3\.json \| jq \-r '.pod' \| sort \| rev \| cut \-d '\-' \-f 2\- \| rev \| sort \| uniq \| rev \| cut \-d '\-' \-f 2\- \| rev \| sort \| uniq \-c
```




  


Finally, if you wish, you can use jq to parse and extract information from the logdna json


 

