This set of tests should be run if Bitnobi is deployed to two servers (or VMs) to verify Federation components are working.

### Prerequisites:
* run the "Smoke Test" on each of the servers to verify that Bitnobi services and components are working correctly.


### Overview:
One of the two Bitnobi servers will act as "Originating Bitnobi" and other server as "Remote Bitnobi".

1. Direct a browser to "Remote Bitnobi" and Sign Up a new user with ID "serviceAccount1", then Signout. Record the password used for later reference.
2. Login as administrator to "Remote Bitnobi" and navigate to "Admin > Resources Management > Users". Edit the attributes of user "serviceAccount1" and sets the "organization", "role", and "department" attributes to the value "remote-system-access". Signout.
3. Login as administrator to "Originating Bitnobi" and navigate to "Admin > Network Management". Create a new Bitnobi Server entry for "Remote Bitnobi" populating fields for hostname (or IP address), port number (8000 for default deployment), Description, user "serviceAccount1" and password (assigned during account SignUp) and press "Submit".
4. An entry should appear in the "Bitnobi Servers" page and status should be "online". A status of "offline" indicates that Originating Bitnobi could not connect to Remote Bitnobi. In this case verify that the hostname, port, userid and password are correct and that there is no firewall blocking access. A status of "forbidden" means that the remote connection was established, but the "serviceAccount1" userid does not have the required "remote-system-access" role.
