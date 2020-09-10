# Lab: Google Cloud Fundamentals: Getting Started with Compute Engine 
## Objectives 
In this lab, you will learn hoe to perform the following tasks:
-	Create a compute Engine virtual machine using the Google Cloud Platform (GCP) console
-	Create a compute Engine virtual machine using the gcloud command line interface.
-	Connect between the two instances 

### Steps:
1.	Create a compute  Engine Virtual machine using the Google cloud platform (GCP) console.
Code($):gcloud compute instances create my-vm-1 --zone=us-central1-a --machine-type=e2-medium --subnet=default --tags=http-server --image=debian-9-stretch-v20200902 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=my-vm-1 
Code($):gcloud compute firewall-rules create allow-http-traffic --direction=INGRESS  --network=default --action=ALLOW --rules=tcp:80 --target-tags=http-server
2.	Create a compute Engine virtual machine using the gcloud command-line interface 
Code($):gcloud config set compute/zone us-central1-b
Code($):gcloud compute instances create "my-vm-2" --machine-type "n1-standard-1" --image-project "debian-cloud" --image "debian-9-stretch-v20190213" --subnet "default"

Connect between the two instances.
1.Use the ping command to confirm that my-vm-2 can reach my-vm-1 over the network:
 Connect to my-vm-2
Code($):gcloud compute ssh my-vm-2 --zone us-central1-b

2. Ping my-vm-1 from my-vm-2
Code($):ping -c 4 my-vm-1

After running the output of the ping command ,exit by typing EXIT and pressing ENTER



Use the ssh command to open a command prompt on my-vm-1 
###steps

1. Code($):gcloud compute ssh my-vm-1 --zone us-central1-a

2. At the command prompt on my-vm-1, install the Nginx web server
Code($): sudo apt-get install nginx-light -y

3. Use the nano text editor to add a custom message to the home page of the web server
Code($):sudo nano /var/www/html/index.nginx-debian.html

4. Use the arrow keys to move the cursor to the line just below the h1 header. Add text like this, and replace YOUR_NAME with your name
Code($):Hi from Rossi

5. Exit the editor and confirm that the web server is serving your new page. 
TO exit the nano text editor .Press Ctrl+o and Enter to save your edited file and press Ctrl+x to exit the nano text editor.

6. At the command prompt on my-vm1, execute this command
Code($):curl http://local host/ 
 Result: The response will be the HTML source of the web server’s home page, including your line of custom text.

7. To exit the command prompt on my-vm1, execute this command
Code($): exit

8. You will return to the command promt on my-vm-2
code($):gcloud compute ssh my-vm-2 --zone us-central1-b

9. To confirm that my-vm-2 can reach the web server on my-vm1, at the command prompt on my-vm-2, execute this command
Code($):curl http://my-vm1/
 Result: The response will be the HTML source of the web server's home page, including your line of custom text

10. Now get the external IP of the my-vm-1 instance form this command
Code($): gcloud compute instances list –zone=us-central1-a

11. Paste the copied IP address of my-vm-1 into a new browser tab and hit enter.
 Result: You will see your web server’s home page, including your custom text.



