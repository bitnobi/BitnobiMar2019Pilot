Federated Botnobi servers allows a Bitnobi user logged into one Bitnobi server (Originating Bitnobi) to create a workflow that can merge data from one or more Remote Bitnobi servers with access controlled through policies. An example of such a configuration is below:

[[images/bitnobi-federation-configuration-1.png]]

In the diagram above, there are 3 separate organizations (Org1, Org2, Org3) that have each deployed a Bitnobi server. Each organization has its own private databases and needs to protect this data from getting into the hands of unauthorized users.
There are data owners in each organization that use Bitnobi to set data access policies. Each organization has its own Bitnobi administrator. A Bitnobi user logged into Org1 can create a workflow in Bitnobi-Org1 that merges data from Bitnobi servers in Org2 and Org2 as well as local data.

Here are the steps to configure the federation of these servers:

1. The administrator of Bitnobi-Org1 browses to Bitnobi-Org2 and Bitnobi-Org3 and creates new accounts. These will become the service accounts for Bitnobi-Org1 to access Bitnobi-Org2 and Bitnobi-Org3.
2. The administrator of Bitnobi-Org1 contacts the administrators of Bitnobi-Org2 and Bitnobi-Org3 to convert them into service accounts.
3. 