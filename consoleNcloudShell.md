########### Console and Cloud Shell

Objectives
In this lab, you learn how to perform the following tasks:

Get access to Google Cloud.

Create a Cloud Storage bucket using the Cloud Console.

Create a Cloud Storage bucket using Cloud Shell.

Become familiar with Cloud Shell features.

Task 1: Create a bucket using the Cloud Console
step 1. In the Cloud Console, on the Navigation menu click Storage > Browser.

step 2. Click Create bucket.

For Name, type a globally unique bucket name, and leave all other values as their defaults.

Click Create.

### Task 2: Access Cloud Shell

In this section, you explore Cloud Shell and some of its features.

You can use the Cloud Shell to manage projects and resources via command line, without having to install the Cloud SDK and other tools on your computer.

Cloud shell provides the following:

**Temporary Compute Engine VM
*Command-line access to the instance via a browser
*5 GB of persistent disk storage ($HOME dir)
*Pre-installed Cloud SDK and other tools
*gcloud: for working with Google Compute Engine and many Google Cloud services
*gsutil: for working with Cloud Storage
*kubectl: for working with Google Container Engine and Kubernetes
*bq: for working with BigQuery
*Language support for Java, Go, Python, Node.js, PHP, and Ruby
*Web preview functionality
*Built-in authorization for access to resources and instances
step 1----In the Google Cloud menu, click Activate Cloud Shell. If prompted, click Continue. Cloud Shell opens at the bottom of the Cloud Console window.
There are three icons to the far right of the Cloud Shell toolbar:

Minimize/Restore: The first one minimize and restores the window, giving you full access to the Cloud Console without closing Cloud Shell.

Open in new window: Having Cloud Shell at the bottom of the Cloud Console is useful when you are issuing individual commands. However, sometimes you will be editing files or want to see the full output of a command. For these uses, this icon pops the Cloud Shell out into a full-sized terminal window.

Close terminal: This icon closes Cloud Shell. Every time you close Cloud Shell, the virtual machine is recycled and all machine context is lost.
step 2. Close Cloud Shell now

### Task 3: Create a bucket using Cloud Shell
step 1. Open Cloud Shell again.

step 2. Use the gsutil command to create another bucket. Replace <BUCKET_NAME> with a globally unique name (you can append a 2 to the globally unique bucket name you used previously):
	'gsutil mb gs://qwiklabs-gcp-01-efb4a8d25c5b'
step 3. In the Cloud Console, on the Navigation menu, click Storage > Browser, or click Refresh if you are already in the Storage Browser. The second bucket should be displayed in the Buckets list.

#### Task 4: Explore more Cloud Shell features.

step 1. Open Cloud Shell.

step 2. Click the three dots (b4af82f98f85f64f.png) icon in the Cloud Shell toolbar to display further options.

step 3. Click Upload file. Upload any file from your local machine to the Cloud Shell VM. This file will be referred to as [MY_FILE].

step 4. In Cloud Shell, type ls to confirm that the file was uploaded.

step 5. Copy the file into one of the buckets you created earlier in the lab. Replace [MY_FILE] with the file you uploaded and [BUCKET_NAME] with one of your bucket names:
			'gsutil cp yersinia.log gs://qwiklabs-gcp-01-efb4a8d25c5b'
Note: If your filename has whitespaces, ensure to place single quotes around the filename. For example, gsutil cp ‘my file.txt' gs://[BUCKET_NAME].
```You have uploaded a file to the Cloud Shell VM and copied it to a bucket.```
step 6. Explore the options available in Cloud Shell by clicking on different icons in the Cloud Shell toolbar.
step 7. Close all the Cloud Shell sessions.

#### Task 5: Create a persistent state in Cloud Shell.
*****Identify available regions
step 1. Open Cloud Shell from the Cloud Console. Note that this allocates a new VM for you.

step 2. To list available regions, execute the following command:
		'gcloud compute regions list'
step 3. Select a region from the list and note the value in any text editor. This region will now be referred to as [YOUR_REGION] in the remainder of the lab.
*****Create and verify an environment variable
step 4. Create an environment variable and replace [YOUR_REGION] with the region you selected in the previous step:
			'INFRACLASS_REGION=us-central1'
step 5. Verify it with echo:
			'echo $INFRACLASS_REGION'
***** Append the environment variable to a file(making directories)
    1. Create a subdirectory for materials used in this class: 
    		'mkdir infraclass'
    2. Create a file called config in the infraclass directory:
		'touch infraclass/config'
    3. Append the value of your Region environment variable to the config file:
		'echo INFRACLASS_REGION=$INFRACLASS_REGION >> ~/infraclass/config'
    4. Create a second environment variable for your Project ID, replacing [YOUR_PROJECT_ID] with your Project ID. You can find the project ID on the Cloud Console Home page.
		'INFRACLASS_PROJECT_ID=qwiklabs-gcp-01-efb4a8d25c5b'
    5. Append the value of your Project ID environment variable to the config file:
		'echo INFRACLASS_PROJECT_ID=$INFRACLASS_PROJECT_ID >> ~/infraclass/config'
    6. Use the source command to set the environment variables, and use the echo command to verify that the project variable was set:
		'source infraclass/config' 
		 'echo $INFRACLASS_PROJECT_ID'
    7. Close and re-open Cloud Shell. Then issue the echo command again:
		'echo $INFRACLASS_PROJECT_ID'

 ***** Modify the bash profile and create persistence
    1. Edit the shell profile with the following command:
			'nano .profile'
    2. Add the following line to the end of the file:
			'source infraclass/config'
    3. Press Ctrl+O, ENTER to save the file, and then press Ctrl+X to exit nano.
    4.  Close and then re-open Cloud Shell to cycle the VM.
    5. Use the echo command to verify that the variable is still set:
			'echo $INFRACLASS_PROJECT_ID'
    ```You should now see the expected value that you set in the config file.```.

 #### Task 6: Review the Google Cloud interface 


