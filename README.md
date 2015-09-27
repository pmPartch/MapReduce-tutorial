# MapReduce-tutorial
This is a modified tutorial for Hortonworks AMI on Amazon Web Services for a very simple demo of MapReduce word count.
## AWS setup for HortonWorks sandbox
* sign up for a free tier AWS account (you will need to supply a credit card number) at http://aws.amazon.com/
* navigate to the AWS Management console
* click on the EC2 link (upper left corner)
* change your region to US-East (N. Virginia) (this region setting is in the upper right of the web page, next to your name). The display name of 'N. Virginia' should be visible next to your name.
* click on *AMIs* (uder Images category on left pane)
* Select *Public Images* from drop list (this drop list says either Owned by me, Public images, or Private images). In search edit box type *ami-e7d9038c* and press enter key.
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
* You will be asked for a key pair. This configuration does not make use of key pairs so change the first drop down to  select Proceed without a key pair and check the check box and press Launch Intances.
* You can press the View Instances button and wait for the status checks to read 2/2 (there is a refresh button on upper right of page. Its the icon with the two circled arrows). This takes a few minutes. While we wait some info...this instance is backed by EBS (so when you shutdown it will keep any data you save on storage). Note that if, instead of shut down, you terminate then the data will be lost.
* once the status checks have changed to 2/2 you can now logon (see next steps)...but before you go, you will need to copy the public IP address of the instance. So go back to the EC2 page (if you ever get lost, just press the orange cube in the upper left of the page and this will get you back to the management console main page). From the EC2 page, click on the Instances under the Instances category. Select your running instance and in the lower pane should be your public IP address. Copy this since you will need it more than a few time in the instructions below.

__WARNING__ remember that as long as your instance is running, it is costing you. So always shut it down when you are done with playing around...don't leave it running (say, overnight). There are email alerts and auto shutdown proceedures that you can put into place, but I have not the time to describe them now. 

__NOTE__ how to shut down the instance when you are done (this does not terminate the instance, but meanly powers it down). There are a couple of ways to do this. One from the terminal (just type poweroff and press enter) and the other from the AWS management console where you navigate to the EC2/instances, right mouse click on your instance and select Instance State/Stop.

## Connecting with cloud VM with the teminal
These instructions are for Windows users (I'll add details for Mac/Linux ASAP).
* To access the VM from windows, you will need an SSH client such as the free PuTTY. Download it from http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
* Run PuTTY and enter the instance public IP into the Host Name edit box. Leave the port 22. Press Open button and you will be shown the terminal for login. Your login name is root and password is hadoop (you will probably be shown a warning dialog, press Yes to shut it down)
*NOTE*: you will need this terminal to compile and run the WordCount MapReduce program so leave it open

## Connecting with the cloud VM with the browser
The TCP ports that I had you open in the instance creation steps will allow you to access the instance using your browser. This will allow you to upload and download files (as well as other interesting tasks).
* In your browser, type this into your address bar: http://XX.YY.WW.ZZ:8888/ where XX.YY.WW.ZZ is your public IP address of your instance. This should show you the default web page of your instance (as well as links to tutorials and other info). There is also a link to an admin site on the instance (the link with port 8080). This requires a login with user admin and password admin. From this admin site you can access both the local file system and the HDFS file sytem for upload or download. Click on this link now.
* On the admin site, you will find in the upper right of the page an Admin button and just left of it a little box like button. Hover your mouse over this box like icon and see you have access to the HSFS file system (HDFS Files) and the instance local file system (Local files). Note that for the Local files, your terminal login places you in the folder named root. Also note that when you show the Local files from the browser that it takes a few seconds to display all available folders so be patient.
* You can also get another view of the instance by typeing into the browser address bar: http://XX.YY.WW.ZZ:8000/ where XX.YY.WW.ZZ is your public IP address of your instance. This gives you access to Hue.

## The Big Data Assignment on Hortonworks
I've needed to re-write the assignment a bit to get it to work on this other VM. I've verified that the output of this set of steps will make the grader happy. The step numbers below are one-to-one match to the assignment steps

1. Open the terminal (this is done on Windows using PuTTY as details in my steps above)
1. once you login (root with password hadoop) stay right where you are. Dont change directories (really, there is no need)
1. you will need the source file for the WordCount.java for this (and following steps). You can download this directly from the terminal by typeing the following: wget https://raw.githubusercontent.com/pmPartch/MapReduce-tutorial/master/WordCount.java
  1. you will need to compile. So follow these steps
  2. assuming you are in your login default directory (if not sure, type cd). Type the following commands in the termial and press enter for each line.
  3. mkdir build
  4. export HADOOP_CLASSPATH=$JAVA_HOME/lib/tools.jar
  5. hadoop com.sun.tools.javac.Main WordCount.java -d build
  6. jar -cvf WordCount.jar -C build/ ./
  7. test the result (as in the assignment and note change in case) by typeing: hadoop jar WordCount.jar WordCount
1. same as original instruction except for case change to 'WordCount'
2. create two files in your current directory (no need to with them elsewhere):
  *  echo "Hello world in HDFS" > testfile1
  *  echo "Hadoop word count example in HDFS" > testfile2
1. create a directory for the input to MapReduce on HDFS by typing (_either_ of the folloing will work)
  * hdfs dfs -mkdir -p testmr/input
  * hadoop fs -mkdir -p testmr/input
1. copy these two files to HDFS by typing (either of the next lines will work)
  * hdfs dfs -put testfile* testmr/input
  * hadoop fs -put testfile* testmr/input
1. run the MapReduce program on the input files: hadoop jar WordCount.jar WordCount testmr/input testmr/output
2. if successful, you can list the output files: hdfs dfs -ls testmr/output
3. check the output: hdfs dfs -cat testmr/output/part*
4. put the output in a location. Now we will do this using the browser interface, since we need to get this part* file off the HDFS and onto your local machine. So follow these steps:
  1. type this into your address bar: http://XX.YY.WW.ZZ:8888/ where XX.YY.WW.ZZ is your public IP. Your user/pswd is admin/admin
  2. in the upper right corner of the page is a Admin button and to the left of this is a litte box-like icon. Hover your mouse over this box icon and select HDFS files.
  3. Navigate to the user/root folder and you should see a folder named testmr, open it. Under this folder should be an output folder that contains a file named part-r-00000. down load this file to your local computer (there is a download link on the right side of the page on the same line as the file.
  4. use this file to upload to your coursea submission page.
