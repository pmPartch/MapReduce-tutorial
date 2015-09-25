# MapReduce-tutorial
This is a modified tutorial for Hortonworks AMI on Amazon Web Services for a very simple demo of MapReduce word count.
## AWS setup for HortonWorks sandbox
* sign up for a free tier AWS account (you will need to supply a credit card number) at http://aws.amazon.com/
* navigate to the AWS Management console
* click on the EC2 link (upper left corner)
* change your region to US-East (N. Virginia) (this region setting is in the upper right, next to your name)
* click on *AMIs* (uder Images category on left pane)
* Select *Public Images* from drop list. In search edit box type *ami-e7d9038c*
* after you select this ami, press the Launch button
* Select instance type *c3-xlarge* (this is not part of the free tier, by the way) and press Next: Configure Instance Details
* Leave defaults and press Next: Add Storage
* Leave defaults and press Next: Tag Instance
* Leave defaults and press Next: Configure Security Group
* Now for some work. You will leave the drault SSH item, but you probably should change the *Anywhere* to *My IP*
  * press add Rule and add Custom TCP Rule for port 8080 and My IP
  * press add Rule and add Custom TCP Rule for port 8888 and My IP
  * press add Rule and add Custom TCP Rule for port 8000 and My IP
  * press add Rule and add HTTP for default port and My IP
  * press add Rule and add HTTPS for default port and My IP
* Now presws Review and Launch
* You should see a warning that your configuration is not under free usage tier (due to machine size). Don't leave it running longer than you need to!!!
* Press Launch button
* You will be asked for a key pair. This configuration does not make use of key pairs so select proceed without a key pair and check the check box and press Launch Intances.
* You can press the View Instances button and wait for the status checks to read 2/2 (there is a refresh button on upper right of page). This takes a few minutes. While we wait...this instance is backed by EBS (so when you shutdown it will keep any data you save on storage). Note that if, instead of shut down, you terminate then the data will be lost.
* once the status checks have changed to 2/2 you can now logon (see next steps)...but before you go, you will need to copy the public IP address of the instance. So go back to the EC2 page (if you ever get lost, just press the oragne cube in the upper left of the page and this will get you back to the management console). From the EC2 page, click on the Instances under the Instances category. Select your running instance and in the lower pane should be your public IP address. Copy this since you will need it more than a few time in the instructions below.

__WARNING__ remember that as long as your instance is running, it is costing you. So alwasy shut it down when you are done with playing around...don't leave it running (say, overnight). There are email alerts and auto shutdown proceedures that you can put into place, but I have not the time to describe them now. 

__NOTE__ how to shut down the instance when you are done (this does not terminate the instance, but meanly powers it down). There are a couple of ways to do this. One from the terminal (just type poweroff and press enter) and the other from the AWS management console where you navigate to the EC2/instances, right mouse click on your instance and select Instance State/Stop.

## Connecting with cloud VM with the teminal
These instructions are for Windows users (I'll add details for Mac/Linux ASAP).
* To access the VM from windows, you will need an SSH client such as the free PuTTY. Download it from http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
* Run PuTTY and enter the instance public IP into the Host Name edit box. Leave the port 22. Press Open button and you will be shown the terminal for login. Your login name is root and password is hadoop (you will probably be shown a warning dialog, press Yes to shut it down)
*NOTE*: you will need this terminal to compile and run the WordCount MapReduce program.

## Connecting with the cloud VM with the browser
The TCP ports that I had you open in the instance creation steps will allow you to access the instance using your browser. This will allow you to upload and download files (as well as other interesting tasks).
* In your browser, type this into your address bar: http://XX.YY.WW.ZZ:8888/ where XX.YY.WW.ZZ is your public IP address of your instance. This should show you the default web page of your instance (as well as links to tutorials and other info). There is also a link to an admin site on the instance (the link with port 8080). This requires a login with user admin and password admin. From this admin site you can access both the local file system and the HDFS file sytem for upload or download. Click on this link now.
* On the admin site, you will find in the upper right of the page an Admin button and just left of it a little box like button. Hover your mouse over this box like icon and see you have access to the HSFS file system and the instance local file system.
* You can also get another view of the instance by typeing into the browser address bar: http://XX.YY.WW.ZZ:8000/ where XX.YY.WW.ZZ is your public IP address of your instance. This gives you access to Hue.

## The Big Data Assignment on Hortonworks
I've needed to re-write the assignment a bit to get it to work on this other VM. I've verified that the output of this set of steps will make the grader happy. Please be patient, I'm writing this up very quickly and might make a bit of a mistake that I'll correct tonight.

