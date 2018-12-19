# Smoke test
This set of tests should be run after Bitnobi is deployed to a new server to verify that each Bitnobi service and component is working. This covers:
* signin, signout, user authentication
* visibility of dashboard
* workflow server and execution of workflow
* audit log server
* python code in workflow
* Jupyter Notebook integration
* Watson integration

### 1. Direct browser to Bitnobi server
* verify that https: is being used and http: is denied
* no warnings about unsecure content.

### 2. Signin as admin user, 
* creating an admin user is part of deployment instructions. First user to signup becomes the administrator. 
* verify that admin functions enabled.
* verify that dashboard is visible
* signout and signin

### 3. create a workflow "audit log":  AuditLog -> Result
* verify that preview working correctly, 
* save and run workflow.
* verify that workflow runs to completion.
* open workflow, Apply AuditLog and look at preview to verify that workflow start/end recorded.

### 4. create a workflow "python test": AuditLog -> Python -> Result
* open Python editor, add in sample template
* press "Execute". should see output in console.
* press "Save", should exit back to workflow editor
* press "Apply Python". Preview should show two columns "name" and "id" with 2 rows.
* connect to Result, attach default policy and press "Apply"
* save and run workflow.
* verify that workflow runs to completion.

### 5. create a report "python test", using "python test" result set
* create a bar chart
* select "name" for x axis, "id" for y axis
* should show justin as 1, john as 2.

### 6. create a Jupyter Notebook named "jupyter test"
* set Jupyter directory = "bitnobi"
* set Jypyter file name = "test.csv"
* set Workflow = "audit log test"
* submit
* press "Upload", 
* will get "pop up blocked", click on icon in browser address bard and select "Always allow pop-ups from xxxx", then press "done".
* refresh browser and press "Upload" again.
* this will open a new browser tab. Enter the following token value, then press "Log in":
6f04edc021ed3ffa7a44c9b4253c3f619530c42c96d98c32
* Jupyter notbook screen presented. Navigate to the "bitnobi" directory and verify that "test.csv" is present.
* log out of Jupyter Notebooks and close the "Jupyter Notebook" tab in the browser.

### 7. Upload to Watson Analytics (requires a Watson account & credentials)
* click on "Hello admin user" menu, select "Register Watson"
* enter your Watson credentials
* go to the "Watson Analytics" page
* click "Create New", enter the name "watson test" and workflow name "audit log test".
* press "Submit" and then "Upload"
* this should open a new browser tab with the Watson Analytics login page.
* enter your password and sign in.
* should now see your data uploaded into Watson Analytics

### 8. signout as admin.