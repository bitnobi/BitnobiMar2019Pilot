The main new feature in this Bitnobi release is support for federation of multiple Bitnobi servers. This allows the user logged into one Bitnobi server (Originating Bitnobi) to create a workflow that accesses data from local as well as one or more remote Bitnobi servers. In particular:

1. The administrator of the Originating Bitnobi must configure it with a service id and password from each Remote Bitnobi that it needs to access.
2. The data owner on each Bitnobi server controls access to a result set through policies. The access policy of a datasource is evaluated by the Bitnobi that holds the datasource.
3. During workflow processing, all data remains on the same Bitnobi as the data source. For example if a workflow is created on Bitnobi_A to use an External Datasource from Bitnobi_B, then when the workflow executes all intermediate data and resultset stays on Bitnobi_B. 
  * One exception is preview data. During workflow editing a small number of rows of data are copied via Bitnobi into the browser to provide visual verification to the user. 
  * The other exception is when datasources on two different servers need to be joined. In this case we introduce a new workflow canvas element "Data Mover" to move intermediate data from an remote Bitnobi to the Originating Bitnobi.


[[images/bitnobi-federation-overview1.png]]

