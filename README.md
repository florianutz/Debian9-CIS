Debian 9 CIS STIG
================

[![Build Status](https://travis-ci.com/florianutz/Debian9-CIS.svg?branch=master)](https://travis-ci.com/florianutz/Debian9-CIS)
[![Ansible Role](https://img.shields.io/badge/role-florianutz.Debian9--CIS-blue.svg)](https://galaxy.ansible.com/florianutz/Debian9-CIS/)

Configure Debian 9 machine to be CIS compliant. Level 1 and 2 findings will be corrected by default.

This role **will make changes to the system** that could break things. This is not an auditing tool but rather a remediation tool to be used after an audit has been conducted.

## IMPORTANT INSTALL STEP

If you want to install this via the `ansible-galaxy` command you'll need to run it like this:

`ansible-galaxy install -p roles -r requirements.yml`

With this in the file requirements.yml:

```
- src: https://github.com/florianutz/Debian9-CIS.git
```

Based on [CIS Debian Benchmark v1.0.0 - 21-12-2018 ](https://community.cisecurity.org/collab/public/index.php).

This repository is a migration from Ubuntu 18.04 [Ubuntu1804-CIS](https://github.com/florianutz/Ubuntu1804-CIS). Currently not all tasks have the right number and naming for the official Debian benchmark. 

Requirements
------------

You should carefully read through the tasks to make sure these changes will not break your systems before running this playbook.

Role Variables
--------------
There are many role variables defined in defaults/main.yml. This list shows the most important.

**debian9cis_notauto**: Run CIS checks that we typically do NOT want to automate due to the high probability of breaking the system (Default: false)

**debian9cis_section1**: CIS - General Settings (Section 1) (Default: true)

**debian9cis_section2**: CIS - Services settings (Section 2) (Default: true)

**debian9cis_section3**: CIS - Network settings (Section 3) (Default: true)

**debian9cis_section4**: CIS - Logging and Auditing settings (Section 4) (Default: true)

**debian9cis_section5**: CIS - Access, Authentication and Authorization settings (Section 5) (Default: true)

**debian9cis_section6**: CIS - System Maintenance settings (Section 6) (Default: true)  

##### Disable all selinux functions
`debian9cis_selinux_disable: false`

##### Service variables:
###### These control whether a server should or should not be allowed to continue to run these services

```
debian9cis_avahi_server: false  
debian9cis_cups_server: false  
debian9cis_dhcp_server: false  
debian9cis_ldap_server: false  
debian9cis_telnet_server: false  
debian9cis_nfs_server: false  
debian9cis_rpc_server: false  
debian9cis_ntalk_server: false  
debian9cis_rsyncd_server: false  
debian9cis_tftp_server: false  
debian9cis_rsh_server: false  
debian9cis_nis_server: false  
debian9cis_snmp_server: false  
debian9cis_squid_server: false  
debian9cis_smb_server: false  
debian9cis_dovecot_server: false  
debian9cis_httpd_server: false  
debian9cis_vsftpd_server: false  
debian9cis_named_server: false  
debian9cis_bind: false  
debian9cis_vsftpd: false  
debian9cis_httpd: false  
debian9cis_dovecot: false  
debian9cis_samba: false  
debian9cis_squid: false  
debian9cis_net_snmp: false  
```  

##### Designate server as a Mail server
`debian9cis_is_mail_server: false`


##### System network parameters (host only OR host and router)
`debian9cis_is_router: false`  


##### IPv6 required
`debian9cis_ipv6_required: true`  


##### AIDE
`debian9cis_config_aide: true`

###### AIDE cron settings
```
debian9cis_aide_cron:
  cron_user: root
  cron_file: /etc/crontab
  aide_job: '/usr/sbin/aide --check'
  aide_minute: 0
  aide_hour: 5
  aide_day: '*'
  aide_month: '*'
  aide_weekday: '*'  
```

##### SELinux policy
`debian9cis_selinux_pol: targeted`


##### Set to 'true' if X Windows is needed in your environment
`debian9cis_xwindows_required: no`


##### Client application requirements
```
debian9cis_openldap_clients_required: false
debian9cis_telnet_required: false
debian9cis_talk_required: false  
debian9cis_rsh_required: false
debian9cis_ypbind_required: false
```

##### Time Synchronization
```
debian9cis_time_synchronization: chrony
debian9cis_time_Synchronization: ntp

debian9cis_time_synchronization_servers:
  - uri: "0.pool.ntp.org"
    config: "minpoll 8"
  - uri: "1.pool.ntp.org"
    config: "minpoll 8"
  - uri: "2.pool.ntp.org"
    config: "minpoll 8"
  - uri: "3.pool.ntp.org"
    config: "minpoll 8"
```  

##### 3.4.2 | PATCH | Ensure /etc/hosts.allow is configured
```
debian9cis_host_allow:
  - "10.0.0.0/255.0.0.0"  
  - "172.16.0.0/255.240.0.0"  
  - "192.168.0.0/255.255.0.0"    
```  

```
debian9cis_firewall: firewalld
debian9cis_firewall: iptables
```

##### 5.3.1 | PATCH | Ensure password creation requirements are configured
```
debian9cis_pwquality:
  - key: 'minlen'
    value: '14'
  - key: 'dcredit'
    value: '-1'
  - key: 'ucredit'
    value: '-1'
  - key: 'ocredit'
    value: '-1'
  - key: 'lcredit'
    value: '-1'
```


Dependencies
------------

Ansible >= 2.4 and <= 2.7 (2.8 is not yet supported)

Example Playbook
-------------------------

```
- name: Harden Server
  hosts: servers
  become: yes

  roles:
    - Debian9-CIS
```

To run the tasks in this repository, first create this file one level above the repository
(i.e. the playbook .yml and the directory `Debian9-CIS` should be next to each other),
then review the file `defaults/main.yml` and disable any rule/section you do not wish to execute.

Assuming you named the file `site.yml`, run it with:
```bash
ansible-playbook site.yml
```

Tags
----
Many tags are available for precise control of what is and is not changed.

Some examples of using tags:

```
    # Audit and patch the site
    ansible-playbook site.yml --tags="patch"
```

License
-------

MIT
