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
