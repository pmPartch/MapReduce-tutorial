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

## Connecting with cloud VM
These instructions are for Windows users (I'll add details for Mac/Linux ASAP).
* To access the VM from windows, you will need an SSH client such as the free PuTTY. Download it from http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
* 
