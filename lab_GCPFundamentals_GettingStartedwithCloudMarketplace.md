# Overview

In this lab, you use Cloud Marketplace to quickly and easily deploy a LAMP stack on a Compute Engine instance. 
The Bitnami LAMP Stack provides a complete web development environment for Linux that can be launched in one click.

**Component	Role**

L Linux	Operating system
A Apache HTTP Server	Web server
M MySQL	Relational database
P PHP	Web application framework
  phpMyAdmin	PHP administration tool


# Objectives

In this lab, we will launch a solution using Cloud Marketplace.


## Task 1: Sign in to the Google Cloud Platform (GCP) Console

Qwiklabs Ressource 
############################################################
Username : student-03-01a683bd5530@qwiklabs.net
Password : PQ6RfGKg8t
GCP Project ID : qwiklabs-gcp-03-59b44798d503
Region : us-central1
Zone : us-central1-a
############################################################


## Task 2: Use Cloud Marketplace to deploy a LAMP stack

1_In the GCP Console, on the Navigation menu, click Marketplace.

2_In the search bar, type LAMP

3_In the search results, click LAMP Certified by Bitnami.
If you choose another LAMP stack, such as the Google Click to Deploy offering, the lab instructions will not work as expected.

4_On the LAMP page, click Launch.
If this is your first time using Compute Engine, the Compute Engine API must be initialized before you can continue.

5_For Zone, select the deployment zone that Qwiklabs assigned to you.
Leave the remaining settings as their defaults.
If you are prompted to accept the GCP Marketplace Terms of Service, do so.

6_Click Deploy.

If a Welcome to Deployment Manager message appears, click Close to dismiss it.
The status of the deployment appears in the console window: lampstack-1 is being deployed. When the deployment of the infrastructure is complete, the status changes to lampstack-1 has been deployed.
After the software is installed, a summary of the details for the instance, including the site address, is displayed.

## Task 3: Verify your deployment

1_When the deployment is complete, click the Site address link in the right pane.
Alternatively, you can click Visit the site in the Get started with LAMP Certified by Bitnami section of the page. A new browser tab displays a congratulations message. This page confirms that, as part of the LAMP stack, the Apache HTTP Server is running.

2_Close the congratulations browser tab.

3_On the GCP Console, under Get started with LAMP Certified by Bitnami, click SSH.
In a new window, a secure login shell session on your virtual machine appears.

4_In the just-created SSH window, to change the current working directory to /opt/bitnami, execute the following command:
> cd /opt/bitnami

5_To copy the phpinfo.php script from the installation directory to a publicly accessible location under the web server document root, execute the following command:
> sudo sh -c 'echo "<?php phpinfo(); ?>" > apache2/htdocs/phpinfo.php'

6To close the SSH window, execute the following command:
> exit

7_Open a new browser tab.
8_Type the following URL, and replace SITE_ADDRESS with the URL in the Site address field in the right pane of the lampstack page. 
SITE_ADDRESS = http://34.123.35.35/
> http://SITE_ADDRESS/phpinfo.php

A summary of the PHP configuration of your server is displayed.
