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

The Context properties screen will pop up.  At this point, we can just hit ok, as the CSuite.context has the info we need.

## Import Baseline Session
We'll be using the session file located:  [Session File](https://drive.google.com/open?id=1r5bj-bxrQjBALXhJpipIM7w-dsFKNg4D)
  * The session file is too large for github.  The session file at this point just contains information about how to attack CSuite.  

The baseline session for csuite contains the list of urls that ZAP previously scanned (spidering CSuite can take several hours).  It's important to note, that this baseline spidering will need to be redone from time to time.  

An alternative would be to not import the baseline session, and perform a spider scan prior to the security scan (more on that later).  This would take longer to run the security scan, but would ensure that we have the latest list of URLs.

## Snapshot session
This is not a necessary step, but is extremely useful when first starting.  Since ZAP creates giant log files, running multiple scans needs to be done in a new session each time.  Creating a snapshot at this point allows for quickly creating a "scan session".  

File -> Snapshot Session as...(Name of snapshot)

You will continue working with the session imported above (or a new one if not imported).  A common scenario/workflow I would run into:
  * Setup session for ZAP to scan (MYSession)
  * Create a snapshot (MYSession-20171212)
  * Begin a scan (using MYSession)
  * Some point scan would need to be stopped or issues occurred.
  * Close/reset ZAP...however, now my base_line files are huge 
  * Remove the MYSession session files
  * Import the snapshot created earlier
  * Create a new snapshot (MYSession-2-20171212)
  * Begin Scan using (MYSession-20171212)
  * Repeat if needed....

## Perform Active Scan
At this point we are ready to perform a security scan:  Tools -> Active Scan

![Active Scan](/screenshots/Active_Scan.png?raw=true "Active scan")

  * Starting Point - Choose Contexts -> CSuite
  * Policy - Default Policy (as defined in the CSuite.context)
  * Context - CSuite
  * User - luke 
  * Recurse - true
  * Start Scan

You can view the progress of the scan:
![Progress](/screenshots/Progess.png?raw=true "Progress")
