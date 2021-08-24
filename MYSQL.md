# Install MySQL on Ubuntu 20.04
## Introduction
This article is part of Big Data series article. We use MySQL as RDBMS as part of Big Data tool we use. MySQL is an open-source database management system. It implements the relational model and uses Structured Query Language (better known as SQL) to manage its data.

This tutorial will go over how to install MySQL version 8.0 on an Ubuntu 20.04 server. By completing it, you will have a working relational database that you can use to build your next website or application.

### Prerequisites
To follow this tutorial, you will need:

One Ubuntu 20.04 server with a non-root administrative user and a firewall configured with UFW. To set this up, follow our initial server setup guide for Ubuntu 20.04.

## Installing MySQL
On Ubuntu 20.04, you can install MySQL using the APT package repository. At the time of this writing, the version of MySQL available in the default Ubuntu repository is version 8.0.19.

To install it, update the package index on your server if you’ve not done so recently:

>sudo apt update
 
Then install the mysql-server package:

>sudo apt install mysql-server
 
This will install MySQL, but will not prompt you to set a password or make any other configuration changes. Because this leaves your installation of MySQL insecure, we will address this next.

## Configuring MySQL
For fresh installations of MySQL, you’ll want to run the DBMS’s included security script. This script changes some of the less secure default options for things like remote root logins and sample users.

Run the security script with sudo:

>sudo mysql_secure_installation
 
This will take you through a series of prompts where you can make some changes to your MySQL installation’s security options. The first prompt will ask whether you’d like to set up the Validate Password Plugin, which can be used to test the password strength of new MySQL users before deeming them valid.

If you select to set up the Validate Password Plugin, any MySQL user you create that authenticates with a password will be required to have a password that satisfies the policy you select. The strongest policy level — which you can select by entering 2 — will require passwords to be at least eight characters long and include a mix of uppercase, lowercase, numeric, and special characters:

    Securing the MySQL server deployment.

    Connecting to MySQL using a blank password.

    VALIDATE PASSWORD COMPONENT can be used to test passwords
    and improve security. It checks the strength of password
    and allows the users to set only those passwords which are
    secure enough. Would you like to setup VALIDATE PASSWORD component?

    Press y|Y for Yes, any other key for No: Y

    There are three levels of password validation policy:

    LOW    Length >= 8
    MEDIUM Length >= 8, numeric, mixed case, and special characters
    STRONG Length >= 8, numeric, mixed case, special characters and dictionary                  file

    Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 2
    Please set the password for root here.

    New password:

    Re-enter new password:

    Estimated strength of the password: 100
    Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : Y
    By default, a MySQL installation has an anonymous user,
    allowing anyone to log into MySQL without having to have
    a user account created for them. This is intended only for
    testing, and to make the installation go a bit smoother.
    You should remove them before moving into a production
    environment.

    Remove anonymous users? (Press y|Y for Yes, any other key for No) : Y
    Success.


    Normally, root should only be allowed to connect from
    'localhost'. This ensures that someone cannot guess at
    the root password from the network.

    Disallow root login remotely? (Press y|Y for Yes, any other key for No) :

    ... skipping.
    By default, MySQL comes with a database named 'test' that
    anyone can access. This is also intended only for testing,
    and should be removed before moving into a production
    environment.


    Remove test database and access to it? (Press y|Y for Yes, any other key for No) :

    ... skipping.
    Reloading the privilege tables will ensure that all changes
    made so far will take effect immediately.

    Reload privilege tables now? (Press y|Y for Yes, any other key for No) :

    ... skipping.
    All done!

Once the script completes, your MySQL installation will be secured. You can now move on to creating a dedicated database user with the MySQL client.

## Creating a Dedicated MySQL User and Granting Privileges
Upon installation, MySQL creates a root user account which you can use to manage your database. This user has full privileges over the MySQL server, meaning it has complete control over every database, table, user, and so on. Because of this, it’s best to avoid using this account outside of administrative functions. This step outlines how to use the root MySQL user to create a new user account and grant it privileges.

In Ubuntu systems running MySQL 5.7 (and later versions), the root MySQL user is set to authenticate using the auth_socket plugin by default rather than with a password. This plugin requires that the name of the operating system user that invokes the MySQL client matches the name of the MySQL user specified in the command, so you must invoke mysql with sudo privileges to gain access to the root MySQL user:

>sudo mysql

Note: If you installed MySQL with another tutorial and enabled password authentication for root, you will need to use a different command to access the MySQL shell. The following will run your MySQL client with regular user privileges, and you will only gain administrator privileges within the database by authenticating:

>mysql -u root -p

Once you have access to the MySQL prompt, you can create a new user with a CREATE USER statement. These follow this general syntax:

>CREATE USER 'username'@'host' IDENTIFIED WITH authentication_plugin BY 'password';

The PRIVILEGE value in this example syntax defines what actions the user is allowed to perform on the specified database and table. You can grant multiple privileges to the same user in one command by separating each with a comma. You can also grant a user privileges globally by entering asterisks (*) in place of the database and table names. In SQL, asterisks are special characters used to represent “all” databases or tables.

To illustrate, the following command grants a user global privileges to CREATE, ALTER, and DROP databases, tables, and users, as well as the power to INSERT, UPDATE, and DELETE data from any table on the server. It also grants the user the ability to query data with SELECT, create foreign keys with the REFERENCES keyword, and perform FLUSH operations with the RELOAD privilege. However, you should only grant users the permissions they need, so feel free to adjust your own user’s privileges as necessary.

You can find the full list of available privileges in the official MySQL documentation.

Run this GRANT statement, replacing sammy with your own MySQL user’s name, to grant these privileges to your user:

>GRANT CREATE, ALTER, DROP, INSERT, UPDATE, DELETE, SELECT, REFERENCES, RELOAD on *.* TO 'sammy'@'localhost' WITH GRANT OPTION;
 
Note that this statement also includes WITH GRANT OPTION. This will allow your MySQL user to grant any that it has to other users on the system.

Warning: Some users may want to grant their MySQL user the ALL PRIVILEGES privilege, which will provide them with broad superuser privileges akin to the root user’s privileges, like so:

>GRANT ALL PRIVILEGES ON *.* TO 'sammy'@'localhost' WITH GRANT OPTION;
 
Such broad privileges should not be granted lightly, as anyone with access to this MySQL user will have complete control over every database on the server.

Following this, it’s good practice to run the FLUSH PRIVILEGES command. This will free up any memory that the server cached as a result of the preceding CREATE USER and GRANT statements:

>FLUSH PRIVILEGES;
 
Then you can exit the MySQL client:

>exit
 
In the future, to log in as your new MySQL user, you’d use a command like the following:

>mysql -u sammy -p
 
The -p flag will cause the MySQL client to prompt you for your MySQL user’s password in order to authenticate.

## Troubleshooting

Sometime you the mysql server couldn't be started and when you see the following error:

    Error: Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' (2)

This error occurs due to multiple installations of mysql. Run the command:

    ps -A|grep mysql

Kill the process by using:

    sudo pkill mysql

and then run command:

    ps -A|grep mysqld

Also Kill this process by running:

    sudo pkill mysqld

Now you are fully set just run the following commands:

    service mysql restart
    mysql -u root -p

Have very well working mysql again!