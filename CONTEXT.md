# Context Information
This section provides information on how the CSuite Context was created.  Once the context is loaded, settings can be modified at File -> Session Properties.  CSuite should be the only context in the context section

## Include in Context
  * http://demo-local.fcsuite.com/erp.*
  * Defines what urls are in scope, currently looking at everything in the /erp* directory

## Exclude in Context
  *  Currently Empty
  *  At one point had */logout included to ensure the scan wasn't logging the user out.  However, this is not needed
 
## Structure
  *  Left as default
  
## Technology
  * many of these tests do not apply to CSuite, and do not apply to a local instance.  
  * Current subset is:
    *  Technology - DB - Postgres
    *  OS - Linux
    *  WS - Apache
    
## Authentication
This is an important section as it deals with how ZAP logs in/out of csuite
![Authentication](/screenshots/Authentication.png?raw=true "Authentication")
  
  * Login Form Target URL:  http://demo-local.fcsuite.com/erp.  The url where the user logs in
  * Login Request POST Data:  login={%username%}&passwd={%password%}.  Tells ZAP to post the username/password as these key/val pairs
  * Username Parameter: login.  Used by zap for username replacement
  * Password Parameter: passwd.  Used by zap for password replacement
  * Regex patterns:
    *  \Q<a href="/erp/logout">Logout</a>\E - when zap sees this in the source, it considers the application "logged in"
    *  \Q<a href="?s_mobile=1" id="desktop_link">Mobile Site</a>\E - when zap sees this in the source ,it considers the application "logged out"
  
## Users
This is where you define who is logging in the system during the scan.  In this case I used the user "luke"

## Remaining Sections
The remaining sections were left as is
