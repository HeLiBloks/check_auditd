# check_auditd
nagios plugin for monitoring auditd status and logged events

```bash
bash-4.2$ ./check_auditd --failedlogins 3,5 --anomalyevents 1,2 --events 280,300
```
     OK - events=53 users=2 terminals=2 hostnames=1 executables=1 processIDs=11 rules=33 pid=621|
     events=53;280;300; changesinconfiguration=0; changestoaccountsgroupsorroles=0; logins=0; failedlogins=0;3;5; authentications=0; failedauthentications=0; users=2; terminals=2; hostnames=1; executables=1; commands=0; files=0; AVCs=0; MACevents=0; failedsyscalls=0; anomalyevents=0;1;2; responsestoanomalyevents=0; cryptoevents=0; integrityevents=0; virtevents=0; keys=0; processIDs=11; rules=33; pid=621; lost=0; backlog=0;

This plugin uses ausearch, aureport to parse the auditd daemon logs and
auditctl for daemon status.


## nagios service configuration
### service config if using `check_by_ssh`

ausearch has a feature that requires it to be started as a coproc over ssh,
therefore the ampersand after `check_auditd`

    define service {
        service_description     auditd
        check_command           check_by_ssh!/usr/bin/sudo $USER1$/check_auditd -v -a '--failed' &!
        check_interval          10
        register                1
    }

### service config if using `check_nrpe`

    define service {
        service_description     auditd
        check_command           check_nrpe!/usr/bin/sudo $USER1$/check_auditd -v -a '--failed'!
        check_interval          10
        register                1
    }


## sudoers setup
Add following to /etc/sudoers or /etc/sudoers.d/nagios

    nagios ALL=(root:ALL) NOPASSWD:/usr/lib64/nagios/plugins/check_auditd

