# Network Discovery - Scan-ta Clause

Discover how to scan network ports and uncover what is hidden behind them.

## Scenario

Christmas preparations are delayed - HopSec has breached our QA environment and locked us out! Without it, the TBFC projects can't be tested, and our entire SOC-mas pipeline is frozen. To make things worse, the server is slowly transforming into a twisted EAST-mas node.

Can you uncover HopSec's trail, find a way back into tbfc-devqa01, and restore the server before the bunny's takeover is complete? For this task, you'll need to check every place to hide, every opened port that bunnies left unprotected. Good luck!

Three bad bunnies with three hidden easter eggs occupy the breached server in a Christmas theme.

## Learning Objectives

Learn the basics of network service discovery with Nmap
Learn core network protocols and concepts along the way
Apply your knowledge to find a way back into the server


---

After losing access to the QA server, the objective was to regain entry by identifying and exploiting exposed network services. Using the known target IP, a structured enumeration approach was applied, starting with port scanning and progressing to service interaction and on-host enumeration.

An initial Nmap TCP scan revealed open SSH (22) and HTTP (80) services, with the web server visibly defaced. Expanding the scan to all TCP ports uncovered additional services, including an FTP server running on a non-standard port and a custom TBFC maintenance service. Anonymous FTP access allowed retrieval of the first key, while interaction with the custom service via Netcat exposed the second key.

To locate the final key, a UDP scan was performed, revealing an open DNS service. A targeted DNS TXT query using dig successfully extracted the third key. These keys were combined to access the web application’s hidden admin console.

Once authenticated, on-host enumeration replaced external scanning. Listing listening services with ss revealed internal-only services, including a MySQL database running on its default port. Local database access allowed enumeration of tables and extraction of the final flag.

This task demonstrated the importance of full port coverage (TCP and UDP), interacting with non-standard services, and transitioning from external enumeration to internal host-based discovery after initial access.
