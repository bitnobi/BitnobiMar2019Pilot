# Release Notes
### Bitnobi December 2018 Pilot release


### Overview:
This distribution of Bitnobi software is intended for customer pilot deployments. The expectation is that the customer will use it for about a month for testing & evaluation and then will undeploy it.
This distribution is not intended for extended enterprise use. 

* For deployment instructions, please see the wiki: https://github.com/bitnobi/BitnobiDec2018Pilot/wiki
* For customer support issues, please contact: hassan@bitnobi.com


### Key features
* the focus of this release is to support federation of multiple Bitnobi servers. A workflow running on one Bitnobi server can merge data from one or more external Bitnobi servers with access controlled through policies.
* attribute based policies to control data sharing in a multi-tenant data ecosystem.
* workflows perform a sequence of operations to assemble the data to be shared.
* graphical workflow editor 
* workflow operations extensible through Python to support packages such as Tensorflow
* basic data visualization and charting built in.
* supports data sources from SQL databases (currently MySQL), CSV files and JSON files.
* interfaces to external packages such as Watson Analytics for data visualization, and Jupyter Notebooks for algorithm exploration.
* maintains audit log of user actions and datasource accesses. Audit log is accessible by admin user only. Audit log can  be used as a datasource in a workflow for log analysis.



### Recent changes
* admin UI functionality for creating, editing and testing connections to external Bitnobi servers.
* new ExternalDS (External Data Source), DataMover and Union workflow elements.
* audit log records accesses from external Bitnobi servers along with remote hostname and userid.
* uses Hashicorp Vault for storing remote Bitnobi passwords.


### Known limitations
* the distribution installs everything into a single docker container. 
* designed to have up to 20 users with max of 5 users simultaneously signed in and actively using Bitnobi. 
* has been load tested with 8 users simultaneously running workflows with 100K row data sources. 
* workflows support a maximum data set size of about 1 million rows. 
* has datasource drivers for MySQL relational database, CSV and JSON files.
* Watson Analytics functionality requires the user to have a valid license for the Watson Analytics web site.
* all Bitnobi users share a single Jupyter Notebooks instance.
