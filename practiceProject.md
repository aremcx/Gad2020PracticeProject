## Google Cloud Fundamentals: Getting Started with Cloud Storage and Cloud SQL
    #Objectives
        ##~In this lab, you learn how to perform the following tasks:

   ~Create a Cloud Storage bucket and place an image into it.

   ~Create a Cloud SQL instance and configure it.

   ~Connect to the Cloud SQL instance from a web server.

   ~Use the image in the Cloud Storage bucket on a web page.

   ~Task 1: Sign in to the Google Cloud




~~Task 2: Deploy a web server VM instance
    --step 1
            gcloud beta compute --project=qwiklabs-gcp-02-2b12045889b9 instances create bloghost --zone=us-central1-a --machine-type=e2-medium --subnet=default --network-tier=PREMIUM --metadata=startup-script=apt-get\ update$'\n'apt-get\ install\ apache2\ php\ php-mysql\ -y$'\n'service\ apache2\ restart --maintenance-policy=MIGRATE --service-account=19611624287-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --tags=http-server --image=debian-9-stretch-v20200902 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=bloghost --reservation-affinity=any

gcloud compute --project=qwiklabs-gcp-02-2b12045889b9 firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server
            
            
~~Task 3: Create a Cloud Storage bucket using the gsutil command line
    step 1. "On the Google Cloud Platform menu, click Activate Cloud Shell, If a dialog box appears, click Start Cloud Shell.
        step 2. convenience, enter your chosen location into an environment variable called LOCATION. Enter one of these commands:
            'export LOCATION=US'
      step 3. In Cloud Shell, the DEVSHELL_PROJECT_ID environment variable contains your project ID. Enter this command to make a bucket named after your project ID:
               'gsutil mb -l $us-central1 gs://qwiklabs-gcp-02-2b12045889b9'
        step 4. a banner image from a publicly accessible Cloud Storage location:
               'gsutil cp gs://cloud-training/gcpfci/my-excellent-blog.png my-excellent-blog.png'

        step 5. Copy the banner image to your newly created Cloud Storage bucket:
                gsutil cp my-excellent-blog.png gs://qwiklabs-gcp-02-2b12045889b9/my-excellent-blog.png'
                
    step 6. Modify the Access Control List of the object you just created so that it is readable by everyone:
            'gsutil acl ch -u allUsers:R gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png'
            
    
    
    
Task 4: Create the Cloud SQL instance
In the GCP Console
     step 1. 
         `gcloud sql instances create blog-db --database-version=MYSQL_5_7 --tier=db-n1-standard-1 --region=us-central1 --root-password=password123` 
                   
Task 5: Configure an application in a Compute Engine instance to use Cloud SQL
 Step 1. click Compute Engine > VM instances.
 Step 2. In the VM instances list, click SSH in the row for your VM instance bloghost.
                'gcloud compute -vm --instances list`
                
 Step 3. In your ssh session on bloghost, change your working directory to the document root of the web server:
       'cd /var/www/html'
Step 4,5. Use the nano text editor to edit a file called index.php:
            'sudo nano index.php'
            - using nano editor paste the following codes:
            '''''''<html>
<head><title>Welcome to my excellent blog</title></head>
<body>
<h1>Welcome to my excellent blog</h1>
<?php
 $dbserver = "34.121.44.121";
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
</body></html>''''''''
`````In a later step, you will insert your Cloud SQL instance's IP address and your database password into this file. For now, leave the file unmodified.`````

Step 6,7. Ctrl+O, and then press Enter to save your edited file and Press Ctrl+X to exit the nano text editor.

Step 8. Restart the web server:
        'sudo service apache2 restart'

Step 9. Open a new web browser tab and paste into the address bar your bloghost VM instance's external IP address followed by /index.php. The URL will look like this: 
        '35.192.208.2/index.php'

 note :Be sure to use the external IP address of your VM instance followed by /index.php. Do not use the VM instance's internal IP address. 
 --When you load the page, you will see that its content includes an error message beginning with the words:
 ----Database connection failed: .....
 --This message occurs because you have not yet configured PHP's connection to your Cloud SQL instance.
 
 Step 10. Return to your ssh session on bloghost. Use the nano text editor to edit index.php again.
       'sudo nano index.php'
       
 Step 11. the nano text editor, replace CLOUDSQLIP with the Cloud SQL instance Public IP address that you noted above. Leave the quotation marks around the value in place.
 Step 12. In the nano text editor, replace DBPASSWORD with the Cloud SQL database password that you defined above. Leave the quotation marks around the value in place.
 Ctrl + O and Ctrl+X to save and exit from nano editor.
 
 
Step 13. Restart the web server:
           'sudo service apache2 restart'
           
Step 14. Return to the web browser tab in which you opened your bloghost VM instance's external IP address. When you load the page, the following message appears:
````Database connection succeeded.`````



Task 6: Configure an application in a Compute Engine instance to use a Cloud Storage object

Step 1. In the GCP Console, click Storage > Browser.

Step 2. Click on the bucket that is named after your GCP project.
Step 3. n this bucket, there is an object called my-excellent-blog.png. Copy the URL behind the link icon that appears in that object's Public access column, or behind the words "Public link" if shown.
confirm your Access Control list with the 'gsutil acl ch' if the command was successful.
step 4. Return to your ssh session on your bloghost VM instance.
step 5,6. Enter this command to set your working directory to the document root of the web server:
      'cd /var/www/html'
      then run this code 'sudo nano index.php'
step 7. Use the arrow keys to move the cursor to the line that contains the h1 element. Press Enter to open up a new, blank screen line, and then paste the URL you copied earlier into the line.

step 8,9. Paste this HTML markup immediately before the URL:
        '<img src='Place a closing single quotation mark and a closing angle bracket at the end of the URL:'>
step 10. The resulting line will look like this:
        '''''<img src='https://storage.googleapis.com/qwiklabs-gcp-0005e186fa559a09/my-excellent-blog.png'>'''''
Step 11. Ctrl+O, and then press Enter to save your edited file.
Press Ctrl+X to exit the nano text editor.

step 12. Restart the web server:
        'sudo service apache2 restart'

        Return to the web browser tab in which you opened your bloghost VM instance's external IP address. When you load the page, its content now includes a banner image.





 

