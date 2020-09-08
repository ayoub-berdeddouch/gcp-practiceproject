# Translation Code : From Console to 100% Command-line Instructions.


## Overview
In this lab, you will create virtual machines (VMs) and connect to them. You will also create connections between the instances.

## Objectives
In this lab, you will learn how to perform the following tasks:

1. Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console.

2. Create a Compute Engine virtual machine using the gcloud command-line interface.

3. Connect between the two instances.



## Translation Code:

__Steps__:

1.  Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console :

> gcloud beta compute --project=qwiklabs-gcp-02-afda413be700 instances create my-vm-1 --zone=us-central1-a --machine-type=n1-standard-1 --image=debian-9-stretch-v20200902 --image-project=debian-cloud 

> gcloud compute --project=qwiklabs-gcp-02-afda413be700 firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server


2. Create a Compute Engine virtual machine using the gcloud command-line interface :

> gcloud config set compute/zone us-central1-b

> gcloud compute instances create "my-vm-2" \
            --machine-type "n1-standard-1" \
            --image-project "debian-cloud" \
            --image "debian-9-stretch-v20190213" \
            --subnet "default"
            
            
 3. Connect between the two instances :
 
    1. Use the ping command to confirm that my-vm-2 can reach my-vm-1 over the network:
    
    - Connect to my-vm-2: 
    
      > gcloud compute ssh my-vm-2
      
    - Ping my-vm-1 from my-vm-2:
    
      > ping -c 4 my-vm-1
    - Use the ssh command to open a command prompt on my-vm-1: 
      
      > ssh my-vm-1
      
    - At the command prompt on my-vm-1, install the Nginx web server:
      
      > sudo apt-get install nginx-light -y
      
    - Use the nano text editor to add a custom message to the home page of the web server:

      > sudo nano /var/www/html/index.nginx-debian.html
     
    - Use the arrow keys to move the cursor to the line just below the h1 header. Add text like this, and replace YOUR_NAME with your name:

      > Hi from YOUR_NAME
      
    - Exit and Confirm that the web server is serving your new page. At the command prompt on my-vm-1, execute this command:

      > curl http://localhost/
      
        __Result__ : The response will be the HTML source of the web server's home page, including your line of custom text.
        
     - To exit the command prompt on my-vm-1, execute this command:

       > exit
       
     - To confirm that my-vm-2 can reach the web server on my-vm-1, at the command prompt on my-vm-2, execute this command:

       > curl http://my-vm-1/
       
   2. NOW get the External IP@ of my-mv-1 instance from this line command:
        
       > gcloud compute instance list
       
   3. Past the copied IP@ of my-mv-1 into a new tab of your browser and hit Enter and VOILA.
   
   END.
      
      
      
      
        

