# Translation Code : From Console to 100% Command-line Instructions.

# Overview
```
In this lab, you create a Cloud Storage bucket and place an image in it.
You'll also configure an application running in Compute Engine to use a database managed by Cloud SQL. 
For this lab, you will configure a web server with PHP, a web development environment that is the basis for popular blogging software. 
Outside this lab, you will use analogous techniques to configure these packages.

You also configure the web server to reference the image in the Cloud Storage bucket.
```

# Objectives

In this lab, you learn how to perform the following tasks:

1. Create a Cloud Storage bucket and place an image into it.

2. Create a Cloud SQL instance and configure it.

3. Connect to the Cloud SQL instance from a web server.

4. Use the image in the Cloud Storage bucket on a web page.


# Translation Code:

## Steps

1. Create a Cloud Storage bucket and place an image into it:

  1. Create VM Instance:
```
gcloud beta compute --project=qwiklabs-gcp-01-f79f97e8bece instances create bloghost --zone=us-central1-a --machine-type=n1-standard-1 --subnet=default 
 --metadata=startup-script=apt-get\ update$'\n'apt-get\ install\ apache2\ php\ php-mysql\ -y$'\n'service\ apache2\ restart --maintenance-policy=MIGRATE 
 --tags=http-server --image=debian-10-buster-v20200902 
 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard 
 --boot-disk-device-name=bloghost --no-shielded-secure-boot --no-shielded-vtpm 
 --no-shielded-integrity-monitoring --reservation-affinity=any

gcloud compute --project=qwiklabs-gcp-01-f79f97e8bece firewall-rules create default-allow-http --direction=INGRESS 
--priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server

```

  2. Copye Bloghost Internal & External IP@
    ```gcloud compute instance list```
    And save it in note for later use.
  
  3. Create a Cloud Storage bucket using the gsutil command line:
    
    - On the Google Cloud Platform menu, click Activate Cloud Shell Activate Cloud Shell. If a dialog box appears, click Start Cloud Shell.
    
    - For convenience, enter your chosen location into an environment variable called LOCATION. Enter one of these commands:
      ```export LOCATION=US or export LOCATION=EU (choosed this one) or export LOCATION=ASIA```
    
    - In Cloud Shell, the DEVSHELL_PROJECT_ID environment variable contains your project ID. Enter this command to make a bucket named after your project ID:
        ```gsutil mb -l $LOCATION gs://$DEVSHELL_PROJECT_ID```
    - Retrieve a banner image from a publicly accessible Cloud Storage location:
        ```gsutil cp gs://cloud-training/gcpfci/my-excellent-blog.png my-excellent-blog.png```
        
     - Copy the banner image to your newly created Cloud Storage bucket:
        ```gsutil cp my-excellent-blog.png gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png```
     
     - Modify the Access Control List of the object you just created so that it is readable by everyone:
        ```gsutil acl ch -u allUsers:R gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png``` DONE.

2. Create a Cloud SQL instance and configure it.
```
  1. Create SQL insctance : __blog-db__
  gcloud sql instances create blog-db --tier=db-n1-standard-2 --region=us-central1
  
  __Result__ : 
  Creating Cloud SQL instance...done.
  Created [https://sqladmin.googleapis.com/sql/v1beta4/projects/qwiklabs-gcp-01-f79f97e8bece/instances/blog-db].
  NAME    | DATABASE_VERSION  |LOCATION       |TIER              |PRIMARY_ADDRESS  |PRIVATE_ADDRESS  |STATUS
  blog-db | MYSQL_5_7         |us-central1-a  |db-n1-standard-2  |34.71.197.161    |-                |RUNNABLE
  --------------------------------------------------------------------------------------------------------------
  
  2. Note the Attributed IP@ for the instance
    Primary@ : 34.71.197.161

  3. Configure & Set a Password:
  gcloud sql users set-password root --host=% --instance blog-db --password bloghostayoub
  DONE
  4. Create user : 
    
  gcloud sql users create blogdbuser --instance blog-db --password bloghostayoub
  DONE
  
  5. Assign the IP@
  
  gcloud sql instances patch blog-db --assign-ip
  DONE 
  to gonfirm : 
  gcloud sql instances describe blog-db
  __Results__ IP@ : 34.71.197.161
  6. Ctreate network: 
  gcloud compute instances bloghost  --network-interface network= web front end, subnet=32 ,address= 34.70.208.116
  
  
```

3. Configure an application in a Compute Engine instance to use Cloud SQL

- In your ssh session on bloghost, change your working directory to the document root of the web server:
  cd /var/www/html
- Use the nano text editor to edit a file called index.php:
  sudo nano index.php
- Paste the content below into the file:

```<html>
<head><title>Welcome to my excellent blog</title></head>
<body>
<h1>Welcome to my excellent blog</h1>
<?php
 $dbserver = "CLOUDSQLIP";
$dbuser = "blogdbuser";
$dbpassword = "DBPASSWORD";
// In a production blog, we would not store the MySQL
// password in the document root. Instead, we would store it in a
// configuration file elsewhere on the web server VM instance.

$conn = new mysqli($dbserver, $dbuser, $dbpassword);

if (mysqli_connect_error()) {
        echo ("Database connection failed: " . mysqli_connect_error());
} else {
        echo ("Database connection succeeded.");
}
?>
</body></html>```

- Ctrl+O , enter , Ctrl+X

- Restart the web server:
  sudo service apache2 restart

- Open a new web browser tab and paste into the address bar your bloghost VM instance's external IP address followed by /index.php. The URL will look like this:
  35.192.208.2/index.php

- When you load the page, you will see that its content includes an error message beginning with the words:

   > Database connection failed: ...
This message occurs because you have not yet configured PHP's connection to your Cloud SQL instance.

- Return to your ssh session on bloghost. Use the nano text editor to edit index.php again.
  sudo nano index.php

- In the nano text editor, replace CLOUDSQLIP with the Cloud SQL instance Public IP address that you noted above. Leave the quotation marks around the value in place.

- In the nano text editor, replace DBPASSWORD with the Cloud SQL database password that you defined above. Leave the quotation marks around the value in place.

- Press Ctrl+O, and then press Enter to save. Then press Ctrl+X to exit the nano text editor

- Restart the web server:
  sudo service apache2 restart

- Return to the web browser tab in which you opened your bloghost VM instance's external IP address. When you load the page, the following message appears:
   > Database connection succeeded....
   
4. Use the image in the Cloud Storage bucket on a web page :
  
  1. Retrieve the link of the bucket in Storage & Copy it:
    gsutil ls -r gs://qwiklabs-gcp-01-f79f97e8bece/my-excellent-blog.png
    
  2. Return to your ssh session on your bloghost VM instance.
    gcloud compute ssh bloghost
    
  3. Enter this command to set your working directory to the document root of the web server:
    cd /var/www/html

  4. Use the nano text editor to edit index.php:
    sudo nano index.php

  5. Use the arrow keys to move the cursor to the line that contains the h1 element. 
      Press Enter to open up a new, blank screen line, and then paste the URL you copied earlier into the line.
  6. Paste this HTML markup immediately before the URL:
    ```<img src='https://storage.googleapis.com/qwiklabs-gcp-01-f79f97e8bece/my-excellent-blog.png'>
    
      The effect of these steps is to place the line containing <img src='...'> immediately before the line containing <h1>...</h1>
      
      Do not copy the URL shown here. Instead, copy the URL shown by the Storage browser in your own Cloud Platform project.
    ```
  7. Ctrl+O , Enter, Ctrl+X
  8. Restart the web server:
    ```sudo service apache2 restart```
    
    
    
  ```Return to the web browser tab in which you opened your bloghost VM instance's external IP address. 
    When you load the page, its content now includes a banner image.```
    
    
    __CONGRATULATION__
      
    
  




 
  
