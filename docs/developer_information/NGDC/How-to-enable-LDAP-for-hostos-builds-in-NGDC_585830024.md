---
layout: default
title: LDAP Enablement in NGDC 
parent: NGDC
grand_parent: Developer Information
---
## LDAP for hostos builds in NGDC

## Audience

Developers and other engineers seeking to enable LDAP on NGDC systems that are deployed with "hostos". 

## Purpose

The purpose of this document is to provide the information needed to enable users to log into a system built with hostos. 

This includes:

* Enabling LDAP to authenticate through the NGDC instance of OpenLDAP
* Controlling access to certain users and groups via entries in `/etc/security/access.conf`.
* Enabling/limiting certain abilities via `sudoers` entries

## Scope

The scope is limited to instruction on how to create `json` entries for the ***undercloud*** and ***role*** definitions in netbox.

## Prerequisites

To complete this, you will need:

* Knowledge of how to correctly create entries in `access.conf`
* Knowledge of how to correctly create `/etc/sudoers` entries
* IP address of the OpenLDAP  server or proxy
* names of users and/or groups that will need to access to the systems. This includes:
	+ functional users/group used for automation
	+ users/group of administrators
	+ any  other users or groups
* sudo information for the above users \- if any.

## Procedure

LDAP is enabled simply by creating an **`mzone_ldap`** section in your undercloud . . .
### Note:If this is a new tenant 
```
Each tenant team should create their own bind user account for openldap in w3. 
The user must be enrolled to the accesshub application: Nextgen NS3 User Management with the role: ngdc-ns3-ldap-auth-preprod-operator. 
Each tenant team is accountable to populate this path /v1/<tenant>/data/regional/bind/creds with the user and password they will use for ldap bind in the required format for the 2nd config run. 
```
This is documented in detail [here](https://pages.github.ibm.com/nextgen-ns3/ns3_documentation/partners/access_management/ns3_accesshub_application/)

## In the netbox **undercloud definition** used for the deployment:

Create a json entry similar to the **`mzone_ldap`** section in the following *example*:

```bash
        "falcon_sensor": {
            "AV": "No",
            "CID": "20709802E00E4B4C9442CE5F8CA3E69D-9C",
            "C_Code": "NGDC_Fabric",
            "Owner": "2J1124897",
            "UT": "public_cloud-UTcode",
            "Updates": "test",
            "proxy": "proxy"
        },
        "mzone_ldap": {
            "ldap_cert": null,
            "ldap_proxies": [
                "198.18.0.42"
            ],
            "ldap_type": "SLAD",
            "ldaps_port": 1636,
            "test_group": "ns3_operator_full_access"
        },
        "nessus": {
            "cse": {
                "group": "NGDC_FABRIC_STG_DAL4_QZ4",
                "host": "nm2-cse.sos.ibm.com",
                "key": "fee10a3a9a6f7f73744922501d2224373878ec2c846d112c80db958c14f78f8a"
            }
        },

```

Notes:

* `"ldap_proxies"` is a list of IP addresses for the LDAP server or proxy you will be using. Currently, only one IP address is supported.
* The `"test_group": "ns3_operator_full_access"` provides an LDAP group you know will exist, for the system to use for a lookup to verify LDAP is correctly configured.

## In the netbox **role definition** used for the deployment:

At the very top of your definition, create a json entry similar to the following *example*:

```bash
{
    "access_control": {
        "access_conf": [
            {
                "lines": [
                    "+ : infraauto : ALL",
                    "+ : infraadmin : ALL",
                    "+ : (ns3_operator_full_access) : ALL"
                ],
                "name": "accessDotConf"
            }
        ],
        "sudoers": [
            {
                "lines": [
                    "infraauto ALL=(ALL:ALL) ALL",
                    "%ns3_operator_full_access ALL=(ALL:ALL) ALL"
                ],
                "name": "sudoers_local"
            }
        ]
    },
```

All entries above need to be exactly as you see, except the `"lines:"` – which are completely up to you. You can have as many or as few as you like.

## Notes:

* IMPORTANT: sections of the above `access_control` definition can be ignored/overridden. Please see  [Overriding access\_control in coud\-init](https://confluence.swg.usma.ibm.com:8445/display/~Rick.Meldrum/How+to+enable+LDAP+for+hostos+builds+in+NGDC#HowtoenableLDAPforhostosbuildsinNGDC-Overridingaccess_controlincoud-init)  below
  * `"sudoers"` entries will go directly into `/etc/sudoers.d/local.` They will be fully validated, and if found to contain errors, they will be commented out.
  * You do not have control over basic/default sudoers entries, including sysop. They are automatically put in place.
  * `access_conf` entries will be put directly into `/etc/security/access.conf` as is. There is minimal validation, so it is important to get these right. If an error is detected, the line will be included but commented out.
  * You do not have control over the following **basic access.conf entries**.  They are automatically added to any entries you specify:

```bash 
`+ : root : ALL`  
`+ : sysop : ALL  
+ : sysgt : ALL`  
`+ : nobody : LOCAL`
`- : ALL : ALL`
```

### Here's an *example* of a real netbox role with `access_control` added:

```bash
{
      "access_control": {
        "access_conf": [
            {
                "lines": [
                    "+ : infraauto : ALL",
                    "+ : infraadmin : ALL",
                    "+ : (ns3_operator_full_access) : ALL"
                ],
                "name": "accessDotConf"
            }
        ],
        "sudoers": [
            {
                "lines": [
                    "infraauto ALL=(ALL:ALL) ALL",
                    "%ns3_operator_full_access ALL=(ALL:ALL) ALL"
                ],
                "name": "sudoers_local"
            }
        ]
    },
    "environment_attributes": {
        "environment_type": "management",
        "hwtype": 2,
        "master_region": "0.0.0.0",
        "proto_boot_net": "fabric_exit",
        "regionid": 10
    },
    "name": "vpc-zonecontrol",
    "network_configuration": {
        "interfaces": [
            {
                "access_configuration": {
                    "name": "US_BOUNDARY_UNDERCLOUD_IPMI_VLAN"
                },
                "name": "mgmt0",
                "prefix": {
                    "tag": null,
                    "vlan": "US_BOUNDARY_UNDERCLOUD_IPMI_VLAN",
                    "vrf": "DEFAULT"
                }
            },
            {
                "access_configuration": {
                    "name": "US_BOUNDARY_VPC_MGMT_UNDERCLOUD_VLAN"
                },
                "name": "eth0",
                "prefix": {
                    "tag": null,
                    "vlan": "US_BOUNDARY_VPC_MGMT_UNDERCLOUD_VLAN",
                    "vrf": "DEFAULT"
                }
            }
        ],
        "loopbacks": [
            {
                "name": "lo0",
                "prefix": {
                    "tag": "zonecontrol_loopback",
                    "vlan": null,
                    "vrf": "US_BOUNDARY_SERVICES_FOR_UNDERCLOUD"
                },
                "tag": "zonecontrol_loopback"
            },
            {
                "name": "lo1",
                "prefix": {
                    "tag": "zonecontrol_loopback",
                    "vlan": null,
                    "vrf": "US_BOUNDARY_INTERNAL"
                },
                "tag": "zonecontrol_loopback"
            },
            {
                "name": "lo2",
                "prefix": {
                    "tag": "tep_loopback",
                    "vlan": null,
                    "vrf": "US_BOUNDARY_SERVICES_FOR_UNDERCLOUD"
                },
                "tag": "tep_loopback"
            },
            {
                "name": "lo3",
                "prefix": {
                    "tag": "tep_loopback",
                    "vlan": null,
                    "vrf": "US_BOUNDARY_SERVICES_FOR_UNDERCLOUD"
                },
                "tag": "tep_loopback"
            }
        ]
    },
    "node_attributes": {
        "arch": "amd64",
        "compute": false,
        "control": true,
        "edge": false,
        "localdisk": false,
        "mgmt": false
    },
    "release_bundles": [
        {
            "name": "hostos-config-release",
            "operation": null,
            "unique_id": 2,
            "version": "6.6.318-20240524T154353Z_0b28756"
        },
        {
            "name": "hostos-config-release",
            "operation": null,
            "unique_id": 3,
            "version": "6.6.318-20240524T154353Z_0b28756"
        }
    ]
}
```


## Overriding access\_control in coud\-init

In order to support the deployment of nonstandard VM's, we support overriding either or both sections of the access\_control definition via the following mechanisms:

### Overriding sudoers

if hostos\-config detects the presence of  /etc/sudoers.d/vm\_users on any system, it will:

* Ignore  /etc/sudoers.d/vm\_users as well as  /etc/sudoers.d/sysop when configuring sudoers.
  * Ignore any sudoers definitions in the netbox role
  * Any files in /etc/sudoers.d other than sysop, sysgt and vm\_users will be removed

### Overriding access\_conf

if hostos\-config detects the presence of **“\# managed by cloud\-init”** in /etc/security/access.conf, it will:.

* Leave /etc/security/access.conf alone, making no changes whatsoever.
  * Ignore any access\_conf definitions in the netbox role.

