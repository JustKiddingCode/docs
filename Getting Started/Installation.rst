Installation
============

osTicket comes with its own web-based installer to help guide you through the installation process without the frustration. While the installer provides step by step guide during the installation process, it's important and helpful to have general knowledge about Web servers, PHP and MySQL.

Prerequisites
-------------

To install osTicket, your web server must have PHP 5.6 (or better) and MySQL 5.0 (or better) installed. If you are unsure whether your server meets these requirements, please check with your host or webmaster before proceeding with the installation.

You will need one MySQL database with valid user, password and hostname handy during installation. MySQL user must have FULL privileges on the database. If you are unsure whether you have these details or if the user has sufficient permissions, please consult your host or database admin before proceeding.

**WINDOWS RECOMMENDED LINKS**

#. PHP 5.6 for Windows Server `64-bit <http://windows.php.net/download#php-5.6-nts-VC11-x64>`__ | `32-bit <http://windows.php.net/download#php-5.6-nts-VC11-x86>`__
#. MariaDB 5.5 for Windows Server `64-bit <https://downloads.mariadb.org/interstitial/mariadb-5.5.37/winx64-packages/mariadb-5.5.37-winx64.msi/from/http://mirror.jmu.edu/pub/mariadb>`__ | `32-bit <https://downloads.mariadb.org/interstitial/mariadb-5.5.37/win32-packages/mariadb-5.5.37-win32.msi/from/http://mirror.jmu.edu/pub/mariadb>`__
#. PHP Manager for IIS (makes managing PHP on IIS much easier) `here <http://phpmanager.codeplex.com/>`_

**Using nginx**

While not officially supported, it is possible to run osTicket on nginx.
You will need a configuration like this::
	location ~ /include {
		deny all;
	}

	if ($request_uri ~ "^/api(/[^\?]+)") {
		set $path_info $1;
	}

	location ~ ^/api/(?:tickets|tasks).*$ {
		try_files $uri $uri/ /api/http.php?$query_string;
	}

	if ($request_uri ~ "^/scp/.*\.php(/[^\?]+)") {
		set $path_info $1;
	}

	if ($request_uri ~ "^/.*\.php(/[^\?]+)") {
		set $path_info $1;
	}

	location ~ ^/scp/ajax.php/.*$ {
		try_files $uri $uri/ /scp/ajax.php?$query_string;
	}

	location ~ ^/ajax.php/.*$ {
		try_files $uri $uri/ /ajax.php?$query_string;
	}

	location / {
		try_files $uri $uri/ index.php;
	}

	location ~ \.php$ {
	#	include snippets/fastcgi-php.conf;
		fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
		fastcgi_param  PATH_INFO        $path_info;
		fastcgi_index  index.php;
		fastcgi_pass   unix:/run/php/php7-osticket.sock;
		include fastcgi.conf;
	}


Getting Started
---------------

At this point you should have downloaded `latest version <https://osticket.com/download>`_ of osTicket. Uncompress the files and upload files and directories in :code:`upload` folder to a directory of your choice on your server. For example :code:`/osticket/`, :code:`/helpdesk/` or :code:`/support/` depending on your preference. Basic knowledge of using FTP is a plus at this stage. If you don't know how to use FTP, we would recommend you read the documentation supplied with your FTP client and learn the basics of uploading and setting permissions on files.

osTicket installer needs to be able to write and modify :code:`ost-config.php` found in the include directory. Please follow the instructions given by the installer.

Using Installation Script
-------------------------

Once all of the above steps are complete, you can complete the installation and basic setup in a web browser. You can invoke the installer by simply browsing the osTicket URL e.g :code:`http://www.yourdomain.com/support`. Alternatively you can enter the URL to it into your browser address bar e.g :code:`http://www.yourdomain.com/support/setup/`.

osTicket's installation script will attempt to auto-detect paths and any permission issues. Please follow the instructions to finish up the installation process.

#. If the script spots any configuration errors then it will not allow you to continue until the errors are corrected.
#. If everything checks out, you will be presented with a form to fill in the required information.
#. If any errors occurs, go back and check the data entered.
#. On valid data the script will create and populate the database plus write a configuration file.

Note that the installer performs basic configuration required to get osTicket up and running. Further configuration is required, post-install, to make the system fully functional.

Installing osTicket Using Fantastico In CPanel
----------------------------------------------

osTicket can also be installed on CPanel based web hosting accounts using Fantastico.From your CPanel, click on Fantastico and follow the instructions to install osTicket.

**IMPORTANT:**

#. The Fantastico default installation package (as of 9 Jan 2010) installs osTicket with the default email address of support@system.com. If you install using Fantastico, you MUST immediately change your default email addresses in the main System Preferences and in your Department settings.
#. The Fantastico package for osTicket may not be as up to date as the latest release available on osTicket.com. Please check the osTicket.com website for the most up to date version.

Finishing Up
------------

If the setup script has finished running with no errors, then congratulations osTicket is now installed. You can now log in with the username and password you created during the install process. After verifying that the installation completed correctly - your next step should be to fully configure your new support ticket system for use. But before you get to it please take a second to cleanup.

#. Change permission of include/ost-config.php to remove write access
#. Delete setup directory
#. Enable the system

Once you have done the above, you can proceed with the next step, Post-Install Setup.

**HAVING TROUBLE**

We can help install and configure osTicket to your needs. Please learn more about our `professional installation services <http://www.osticket.com/support/>`_.

Self-Help Troubleshooting
-------------------------

If you can not find any solutions to the problem you are having, you can enable the "Show Errors" flags located in :code:`/bootstrap.php` (or :code:`/main.inc.php` in older versions):

.. code-block:: bash

   # Don't Display Errors
   ini_set('display_errors',0);
   ini_set('display_startup_errors',0);

Change this to:

.. code-block:: bash

   ini_set('display_errors',1);
   ini_set('display_startup_errors',1);

Then errors should be displayed either in your web browser or in your server's :code:`error.log` file.

Moreover, don't forget to check your osTicket Dashboard page and your mail server log.
