#Introduction to Containers and Docker v1.6
##Objectives: Run and Distribute Containers With Docker
-Build a Docker image.

-Push a Docker image to Google Cloud Registry.

-Run a Docker container.

### First of, Run the Web Server Manually
steps 
Before starting the, the vm has already been set with the name k8s-workshop-module-1-lab and its in us- central1-f zone
1. First go the the ssh using this command
Code($):gcloud compute ssh k8s-workshop-module-1-lab --zone=us-central1-f

when prompted, press 	ENTER

2. Make sure the instance is fully provisioned. To do this, run the following command and look for the kickstart directory
code($):ls /

3. The source code for this lab is available in the /kickstart folder. Switch to that directory.
Code($):cd /kickstart

4. And list the contents
code($):ls -lh

5. Install the latest version of Python and PIP.
code($):sudo apt-get install -y python3 python3-pip

6. Install Tornado library that is required by the application.
code($):pip3 install tornado

7. Run the Python application in the background.
code($):python3 web-server.py &

8. Ensure that the web server is accessible.
code($):curl http://localhost:8888

9. The response should look like this:
Hostname: k8s-workshop-module-1-lab

10. Terminate the web server.
code($):kill %1

### Package Using Docker
Docker images are described via Dockerfiles. Docker allows the stacking of images. Your Docker image will be built on top of an existing Docker image library/python that has Python pre-installed.

steps 
1. Look at the Dockerfile.
code($):cat Dockerfile

2. Build a Docker image with the web server.
code($):sudo docker build -t py-web-server:v1 .
Be sure to include the '.' at the end of the command. This tells Docker to start looking for the Dockerfile in the current working directory.

3. Run the web server using Docker.
code($):sudo docker run -d -p 8888:8888 --name py-web-server -h my-web-server py-web-server:v1

4. Try accessing the web server again, and then stop the container.
code($):curl http://localhost:8888
code($):sudo docker rm -f py-web-server

### Upload the Image to a Registry
steps
1. Add the signed in user to the Docker group so you can run docker commands without sudo and push the image to the repository as an authenticated user using the Container Registry credential helper.
code($):sudo usermod -aG docker $USER

2. Exit your the SSH session.
code($):exit 

3. Launch a new SSH session. This action is needed so that the group change you just made will take effect.
code($):gcloud compute ssh k8s-workshop-module-1-lab --zone=us-central1-f

when prompted, press 	ENTER

4. In the new SSH session, return to the kickstart directory.
code($):cd /kickstart

5. Store your GCP project name in an environment variable.
code($):export GCP_PROJECT=`gcloud config list core/project --format='value(core.project)'`

6. Rebuild the Docker image with a tag that includes the registry name gcr.io and the project ID as a prefix.
code($):docker build -t "gcr.io/${GCP_PROJECT}/py-web-server:v1" .
Again, be sure to include the '.' at the end of the command. This tells Docker to store the image in the current working directory.

### Make the Image Publicly Accessible
steps
1. Configure Docker to use gcloud as a Container Registry credential helper (you are only required to do this once).
code($):PATH=/usr/lib/google-cloud-sdk/bin:$PATH
code($):gcloud auth configure-docker

when prompted, press 	ENTER

2. Push the image to gcr.io
code($):docker push gcr.io/${GCP_PROJECT}/py-web-server:v1

3. To see the image stored as a bucket (object) in your Google Cloud Storage repository.
code($):gsutil ls

4. Update the permissions on Google Cloud Storage to make your image repository publicly accessible.
code($):gsutil defacl ch -u AllUsers:R gs://artifacts.${GCP_PROJECT}.appspot.com
code($):gsutil acl ch -r -u AllUsers:R gs://artifacts.${GCP_PROJECT}.appspot.com
code($):gsutil acl ch -u AllUsers:R gs://artifacts.${GCP_PROJECT}.appspot.com

### Run the Web Server From Any Machine
steps
1. The Docker image can now be run from any machine that has Docker installed by running the following command.
code($):docker run -d -p 8888:8888 -h my-web-server gcr.io/${GCP_PROJECT}/py-web-server:v1

