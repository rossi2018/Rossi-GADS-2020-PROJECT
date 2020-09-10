#  Getting Started with App Engine
## Objectives:
- Initialize App Engine.
- Preview an App Engine application running locally in Cloud Shell.
- Deploy an App Engine application, so that others can reach it.
- Disable an App Engine application, when you no longer want it to be visible.



### Task 1: Initialize App Engine

1. Initialize your App Engine app with your project and choose its region:
code($):gcloud app create --project=$DEVSHELL_PROJECT_ID

When prompted, select the region where you want your App Engine application located.


2. Clone the source code repository for a sample application in the hello_world directory:
code($):git clone https://github.com/GoogleCloudPlatform/python-docs-samples

3. Navigate to the source directory:
code($):cd python-docs-samples/appengine/standard_python3/hello_world


### Task 2: Run Hello World application locally

In this task, you run the Hello World application in a local, virtual environment in Cloud Shell.

Ensure that you are at the Cloud Shell command prompt.

1. Execute the following command to download and update the packages list:
code($):sudo apt-get update

2. Set up a virtual environment in which you will run your application. Python virtual environments are used to isolate package installations from the system:
code($):sudo apt-get install virtualenv

If prompted [Y/n], press Y and then Enter.Then run this code
code($):virtualenv -p python3 venv

3. Activate the virtual environment.
code($)source venv/bin/activate

4. Navigate to your project directory and install dependencies.
code($):pip install  -r requirements.txt

5. Run the application:
code($):python main.py

Ignore the warning if any.

6. In Cloud Shell, click Web preview (Web Preview) > Preview on port 8080 to preview the application.


7. To end the test, return to Cloud Shell and press Ctrl+C to abort the deployed service


8. Using the Cloud Console, verify that the app is not deployed. In the Cloud Console, on the Navigation menu (Navigation menu), click App Engine > Dashboard.


### Task 3: Deploy and run Hello World on App Engine

1. Navigate to the source directory:
code($):cd ~/python-docs-samples/appengine/standard_python3/hello_world

2. Deploy your Hello World application:
code($):gcloud app deploy

If prompted "Do you want to continue (Y/n)?", press Y and then Enter. This app deploy command uses the app.yaml file to identify project configuration.

3. Launch your browser
code($):gcloud app browse
Copy and paste the URL into a new browser window.

### Task 4: Disable the application
1. In the Cloud Console, on the Navigation menu (Navigation menu), click App Engine > Settings.

2. Click Disable application.

3. Read the dialog message. Enter the App ID and click DISABLE.

If you refresh the browser window you used to view to the application site, you'll get a 404 error.
