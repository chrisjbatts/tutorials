***Beginners Guide to Setting Up a Linux Server to Host Websites On. N.B. I’m fed up with heading down the ‘rabbit hole’, trusting that a tutorial will help me out, only to find that it stops part way. To be completely clear, this guide will show you how to setup and manage a server using Linode and a Mac, to the point where you can host multiple Rails sites on a single server and point to them from different domain names. From my small amount of experience in the area of linux, it seems a particularly hard topic for newbies to get into.***

*I’m writing this article after spending a day setting up a virtual private server (VPS) running Ubuntu 14.04 LTS, which hosts all of my small-time personal websites. Before starting the process, I was completely new to Linux. I’d only recently learned Ruby, Rails and Javascript and had previously only dabbled in wordpress sites, hosting with the straightforward setup that one.com provides — it sets up an SQL database and FTP host — you just need to download wordpress, FTP it up, link it to the SQL database, then get building. Just don’t use the pre-installed version one.com provides as they’ve hacked it to only allow certain themes.*

Sign up to Linode. Go through all the usual admin/payment stages.
Add a linode — this is a single virtual private server.
Deploy an image — this installs the operating system on your new VPS. Select ```Ubuntu 14.04 LTS```.
Make the deployment disk size the max (unless you want to add a second instance on the same server).
Swap disk is a small amount of hard drive space allocated to writing RAM to when not in use — leave it on the default setting.
Give a root password. Make it ultra secure and write it down everywhere.
Select the instance then boot the server.
Open up terminal or iterm. Click on Linodes. Look for the IP address of the server. It’ll be in this number format ```000.00.000.00```
Copy it. Go to your terminal window. Type: ```ssh root@000.00.000.00``` — swapping the zeros for your server’s IP of course.
Ignore and pass the warning by typing yes and enter.
Type in that password you made and wrote down earlier.

You’ve now bought, built, and initiated a new server with linux on it and then connected to it. Act smug for a couple of moments.

Move to the root of the file system from the root directory by typing ```cd /```. Type ```ls``` and have a look around. It should look vaguely familiar to OSX.
Type ```sudo useradd -d /home/appsuser -m appsuser``` — this creates a new user called appsuser and makes a home directory for the user in home/appsuser at the same time.
Type ```passwd appsuser```. This command allows you to make a new password for the appsuser user.
Make a secure password. Write it down. Type it in. Press enter. Type it again to confirm it.
Type ```su — appsuser``` (su means switch user).
Type ```pwd```. Get your bearings. You should recognise the path from the command earlier in the setup when creating the second user. This is the default home directory for appsuser.
Type ```su — root```.
Type ```usermod -G sudo appsuser```. usermod stands for modify a user, ```-G``` refers to modifying user groups — in this case the sudo group. ```sudo appsuser``` instructs linux to add appsuser to the sudo group.
Type ```chown appsuser /home/appsuser```. This gives ownership of the ```home/appsuser``` directory to ```appsuser```.
Type ```su — appsuser```.
Type: ```sudo \curl -sSL https://get.rvm.io | bash -s stable —rails```. This’ll fail. Don’t worry.
Read the failure. Follow the instructions in the terminal window and copy and enter the line that looks like this: ```gpg —keyserver hkp://keys.gnupg.net —recv-keys 000000000000000000000000```. This downloads the decryption key for the installation.
Type it again: ```sudo \curl -sSL https://get.rvm.io | bash -s stable —rails```. This’ll install ruby version manager with rails. It isn’t quick.
```sudo apt-get install _____``` is the standard command in Linux for installing a package. If you’ve used homebrew previous on mac, the OSX equivalent is ```brew install ___```.
Type ```sudo apt-get update```. This will update any packages on the server. It is good practise to do this before installing a new package.
Type ```apt-get install nginx```. This’ll install the nginx package which will is a clever little bit of software that routes incoming web traffic to different folders — there are many of these available. Its great for hosting multiple websites on one server.
Type ```cd /``` to get back to the home directory. Type ```ls``` to have another look around.
Type ```cd etc/nginx```. This is the default path to nginx. Type ```ls``` to have a look around. Note sites-available and sites-enabled folders. sites-available is a repository for any sites you may run in the future from the server. Its a quick way to toggle on and off site settings for the future. They won’t work until you copy the setup to sites-enabled though.
Type ```cd sites-available```.
nginx adds a default configuration file in the sites-available folder as a template. Copy it for your first website: ```sudo cp default website1```.
Type ```nano website1```. This opens a simple text editor called nano to amend the setup.
Clean up the file. Use ```ctrl+k``` to remove any lines not needed. The only info needed is: ```server { listen 80; root /home/website1; index index.html index.htm; server_name website1.com; }```.
```cd /home```. ```mkdir website1```. ```cd /etc/nginx/sites-available```.
```cd /etc/nginx```. ```ln -s /etc/nginx/sites-available/website1 sites-enabled``` — this makes a symbolic link from the sites-available content to the sites-enabled content. Having just one copy adheres to the single responsibility principle.
Go to your domain provider. Add an A record with the IP address of the server. This routes any traffic coming into the domain to the IP address of the server (this could take a while to update the records. Don’t be surprised if it doesn’t work for a few hours. Until its updated, access it by putting the server’s IP in your browser. That’ll at least direct you to the nginx landing page for now).
Type ```nginx -s reload```. This will reload the nginx config and make it all work.


**To try next:**

* Set up SSH keys: http://kb.mediatemple.net/questions/1626/Using+SSH+keys+on+your+server
* Install bitbucket. Its almost identical to github but allows free private repositories. Use it to sync content with the server.
* Install Capistrano. It manages the manual labour around administering Rails.
