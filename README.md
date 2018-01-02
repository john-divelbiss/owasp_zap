# OWASP ZAP Configuration/Setup

## Download/Install
The following is specific to the csuite environment.  Assumption is this tool will run against a locally hosted instance of csuite.  Setup via: [Developer Setup](https://github.com/john-divelbiss/csuitetools/tree/master/developer_setup)

  * Zed Attack Proxy (ZAP) website:  [Here](https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project) 
  * ZAP can be downloaded:  [Here](https://github.com/zaproxy/zaproxy/wiki/Downloads)

When first running ZAP, you'll be presented with the following dialog:
![Session Persist](/screenshots/persist_sessions.png?raw=true "Sessions")

This option can always be changed later in Options -> Database, however, for the time being I've been leaving it to yes.  But.....

## ZAP Sessions vs Contexts- IMPORTANT!!
ZAP logs everything...EVERYTHING.  During a single session security scan, log files can reach 2-3 GB in size, running multiple scans on the same session will kill your machine.   It's been my experience that when running this against csuite, one security run should be considered one session.  Meaning, everytime a new csuite scan occurs we should create a new session. 

It's important to keep on eye on your session files so they don't get too big and slow things down.  ZAP allowed a 12GB session file be created, which was unusable when finished.

