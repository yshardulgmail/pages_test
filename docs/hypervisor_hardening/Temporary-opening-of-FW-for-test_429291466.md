---
layout: default
title: Opening of FW for test 
parent: Hypervisor Hardening
---

## Temporary opening of FW for test


**cr\_temp\_allow\_hostos\_fw.sh**

```bash
#!/usr/bin/env bash

#
# A short script called from GT to temporarily open up the
# hostos FW on a single node for a short period of time.
#
# called from GT with one of 2 command syntax.   The first
# is to enable and the second to cleanup/disable

# ACTION=""          LOCAL_SCRIPT=~/cr_temp_allow_hostos_fw.sh NUM=$(date +%s);  eval  gt -Z <mzoneXYZ> <singleNODE> $(cat $LOCAL_SCRIPT | gzip -cf | base64 -w 0 | gawk '{print "\" sudo -E \"$ACTION\" bash -c \047echo " $1 " | base64 -d | gunzip -cf  > ~/.scrpt$NUM && chmod +x ~/.scrpt$NUM && ~/.scrpt$NUM ; sleep 3 && cat ~/nohup.out\047  \""}') #### ENABLE COMMAND ####

# ACTION="CLEANUP=1" LOCAL_SCRIPT=~/cr_temp_allow_hostos_fw.sh NUM=$(date +%s);  eval  gt -Z <mzoneXYZ> <singleNODE> $(cat $LOCAL_SCRIPT | gzip -cf | base64 -w 0 | gawk '{print "\" sudo -E \"$ACTION\" bash -c \047echo " $1 " | base64 -d | gunzip -cf  > ~/.scrpt$NUM && chmod +x ~/.scrpt$NUM && ~/.scrpt$NUM ; sleep 3 && cat ~/nohup.out\047  \""}') #### CLEANUP COMMAND ####

#  STEPS: 
#         1 open CR to request a TEMP HostOS Firewall opening for ONE 1 node
#         2 open up a CIO ticket to the HostOS team so we can follow up with logDNA monitoring
#         3 place this script in user home on deployer as ~/cr_temp_allow_hostos_fw.sh 
#         4 run 1st command to ENABLE the opening
#         5 when your network test is complete, run CLEANUP comand 
#
#         * note: the TEMP opening will close back down when a 3 hour timer has expired
#

declare DEBUG=true
declare stderr_or_none=$(eval $DEBUG && echo -n '-s')

if [ "$(id -u)" != "0" ]; then
    echo "$$: Please re-run this script as root."
    exit 1
fi

if [  -z "${CLEANUP}" ]; then
    # CLEANUP not set so 3 hours default cleanup specified in secs
    declare -i CLEANUP=$(( 3*60*60 ))
elif [ "${CLEANUP}" -eq 1 ]; then
    eval $DEBUG && echo $$: user said to cleanup args=$#
else
    # user set it up. look at the value must be set to 1 or invalid
    echo "$$: ERROR: CLEANUP is set to an invalid value must be set to 1 for cleanup $CLEANUP"
    exit -1
fi

declare -i rightnow=$(date --date="now" --utc +%s)
declare -i future=$(( rightnow + CLEANUP ))
declare    future_str=$(echo $(date -d @${future} --utc +%m-%d-%Y-%T) | tr ':' '_')

if [ "$#" -eq 1 ]; then
    eval $DEBUG && echo $$: sleeping...
    eval $DEBUG && [ "${CLEANUP}" -ne 1 ] && echo $$: job launched...
    sleep ${CLEANUP}
    eval $DEBUG && [ "${CLEANUP}" -eq 1 ] && echo $$: cleaning up now...
    for chain in INPUT OUTPUT FORWARD; do
        # should be just one line but just in case a dupe gets added...
        comment_key="HOSTOS-${chain}_CR_TEMP_ALLOWANCE"
        # reverse sort is important so the index can roll back without holes in it
        chain_lines=$(iptables --line-numbers -nL HOSTOS-${chain} | grep ${comment_key} | awk '{print $1}' | sort -nr | xargs)
        for line in ${chain_lines}; do
        re='^[0-9]+$'
        if ! [[ ${line} =~ $re ]] ; then
            logger ${stderr_or_none} -t hostos-debug "IPTABLES: not a valid index into chain ${line}"
            exit -1
        fi
        # grab err and out
        cmd="iptables -D HOSTOS-${chain} ${line}"
        tmp=$(eval ${cmd} 2>&1)  || {
            logger ${stderr_or_none} -t hostos-debug "IPTABLES: error running ${cmd} context: ${tmp}"
            exit -1
        }
        logger ${stderr_or_none} -t hostos-debug "$$: IPTABLES: ran ${cmd}"
        done
    done
    # get rid of itself
    rm $0
    eval $DEBUG && echo $$: finished!
else
    if [  ! "${CLEANUP}" -eq 1 ]; then
        # this is not specified by the user, the default cleanup is in play so do the TEMP open
        for chain in INPUT OUTPUT FORWARD; do
            iptables -A HOSTOS-${chain} -j ACCEPT -m comment --comment  HOSTOS-${chain}_CR_TEMP_ALLOWANCE_${future_str}
        done
    else
        eval $DEBUG && echo $$: did not create new rules
    fi
    if eval $DEBUG; then
        #rm -f ~/nohup.out
        nohup $0 run_it${future_str} &> ~/nohup.out &
    else
        nohup $0 run_it${future_str} &> /dev/null &
    fi
fi

exit 0


```
