wa-admin - a tool for managing webalizer Apache vhosts
======================================================

This file just contains some basic instructions to get wa-admin running on your box.

I know it's a bit messy without any documentation or man pages, but that's the result of
just a few hours work, trying to get my logs being added to webalizer in a simple way :-)

So, to install wa-admin, just follow these steps:

Requirements:

* math/gnuplot
* www/webalizer

Get the latest wa-admin copy from the Git repository:
-----------------------------------------------------
  
	$ git clone git://github.com/dnaeon/wa-admin.git
  
Create the needed directories:
------------------------------

	$ sudo mkdir -p /usr/local/www/wa-admin
	$ sudo mkdir -p /usr/local/etc/wa-admin

Copy the template files and configuration
-----------------------------------------

	$ sudo cp wa-admin/wa-admin.css /usr/local/www/wa-admin/
	$ sudo cp wa-admin/*.html /usr/local/etc/wa-admin/
	$ sudo cp wa-admin/webalizer.conf.tpl /usr/local/etc/wa-admin/
  
Edit `/usr/local/etc/wa-admin/webalizer.conf.tpl`
-------------------------------------------------

This is a just a template file, which is an actual webalizer.conf file.

Just edit it and add/remove what you need or what you don't.

One thing to keep in mind is to leave the following options as they are,
since they are being used by the wa-admin tool to prepare the template for the vhost:

* LogFile
* HostName
* OutputDir
* DNSCache
  
If you do not want DNS reverse lookups, just comment the corresponding DNS lines from
the template file.

Add the hosts with their log files
----------------------------------

To add new log files to be analyzed by webalizer, just do the following:

	$ sudo touch /usr/local/etc/wa-admin/wa-admin.vhosts
  
Please, note that the above is needed to be run only once - the first time 
you install wa-admin.

Now to add a new log file, just do the following:

	$ sudo wa-admin add <hostname> <path-to-log-file>

Just repeat the above command for all hosts and log files, until ready.

Modify Apache
-------------

You will need to modify Apache configuration, so that it
 finds the wa-admin DocumentRoot, which currently defaults to `/usr/local/www/wa-admin`

Run `wa-admin` from cron
------------------------

To run wa-admin from cron, simply put the following line to your /etc/crontab

	5 *   *   *   *       root   /path/to/wa-admin run-cron

This will run wa-admin every hour and 5 minutes.

When ready execute wa-admin run
-------------------------------

	$ sudo wa-admin run

This will go through all added to wa-admin vhosts and create the graphs

It might take some time, until it finishes, if you are running with the DNS
resolver settings for webalizer. 

Check your graphs! :)
---------------------

Now open up a browser and go to your wa-admin Apache vhost.
