The focus of this release is to support federation of multiple Bitnobi servers. A Bitnobi user logged into one Bitnobi server (Originating Bitnobi) can create a workflow that can merge data from one or more Remote Bitnobi servers with access controlled through policies.

[[images/bitnobi-federation-overview1.png]]

To use the Federation feature:

1. The administrator of the Originating Bitnobi must configure it with a service id and password from each Remote Bitnobi that it needs to access.
2. The data owner on each Bitnobi server controls access to a result set through policies. The access policy of a datasource is evaluated by the Bitnobi that holds the datasource.
3. During workflow processing, all data remains on the same Bitnobi as the data source. For example if a workflow is created on Bitnobi_A to use an External Datasource from Bitnobi_B, then when the workflow executes all intermediate data and resultset stays on Bitnobi_B. 
   * One exception is preview data. During workflow editing a small number of rows of data are copied via Bitnobi into the browser to provide visual verification to the user. 
   * The other exception is when datasources on two different servers need to be joined. In this case we introduce a new workflow canvas element "Data Mover" to move intermediate data from an remote Bitnobi to the Originating Bitnobi.

### Terminology
* Originating Bitnobi: the bitnobi server that the data consumer is logged into and is using to compose the workflow. The workflow definition and status during execution is stored on the originating Bitnobi. If the workflow contains a datasource in the originating Bitnobi, then a workflow segment may be executed here.
* Remote Bitnobi: if the workflow contains an ExternalDS datasource, then that segment of the workflow is executed on the Remote Bitnobi.
* DataMover element: new canvas element that moves intermediate data from the current Bitnobi to the specified destination. Workflow segment execution terminates at the DataMover element and the destination Bitnobi is notified that data is available for workflow execution.

