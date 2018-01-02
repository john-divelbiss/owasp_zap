# OWASP ZAP Configuration/Setup

## Download/Install
The following is specific to the csuite environment.  Assumption is this tool will run against a locally hosted instance of csuite.  Setup via: [Developer Setup](https://github.com/john-divelbiss/csuitetools/tree/master/developer_setup)

  * Zed Attack Proxy (ZAP) website:  [Here](https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project) 
  * ZAP can be downloaded:  [Here](https://github.com/zaproxy/zaproxy/wiki/Downloads)

When first running ZAP, you'll be presented with the following dialog:
![Session Persist](/screenshots/persist_sessions.png?raw=true "Sessions")

This option can always be changed later in Options -> Database, however, for the time being I've been leaving it to yes.  But.....

## ZAP Sessions vs Contexts- IMPORTANT!!
### Sessions
ZAP logs everything...EVERYTHING.  During a single session security scan, log files can reach 2-3 GB in size, running multiple scans on the same session will kill your machine.   It's been my experience that when running this against csuite, one security run should be considered one session.  Meaning, everytime a new csuite scan occurs we should create a new session. 

It's important to keep on eye on your session files so they don't get too big and slow things down.  ZAP allowed a 12GB session file be created, which was unusable when finished.

We have created a baseline session that has pre-spidered csuite.  The baseline session populates urls for ZAP to scan, which speeds up the security scan. 

### Contexts
Context contain the details of the site being scanned.  The idea is that contexts deal with detail about a site to attack, and sessions are the details of what/how contexts executed.

The current context file included contains:
  * Details of the site being scanned (demo-local.fcsuite.com)
  * Authentication details for the scanner
  * List of attackes to perform against the list of urls

## Import CSuite Context
We'll be using the context file located:  [CSuite.context](contexts/CSuite.context)

First, we need to remove the Default Context.  It includes default tests/scans that are not applicable to CSuite and will interfer with our scans.   Right Click Default Context -> Delete

Next, import the CSuite context:  File -> Import Context -> CSuite.context 

## Import Baseline Session
The baseline session for csuite contains the list of urls that ZAP previously scanned (spidering CSuite takes several hours).  It's important to note, that this baseline spidering will need to be redone from time to time.  

An alternative would be to not import the baseline session, and perform a spider scan prior to the security scan (more on that later)
