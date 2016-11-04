#HDP Password Change
Security is a major concern in a shared data environment. Ensuring that only users who should have access do have access can be a daunting task, especially when an administrator of the environment leaves. Contained herein is a list of the administrator passwords that should be changed and instructions for changing them.

##Changing Database Passwords
Various database engines provide different methods for changing a user's password. Here are examples of how to change passwords in each of these enviornments.

###Oracle Database
In Oracle, a user's password can be chaged from the sqlplus command line:
```
sqlplus / as sysdba
SQL > alter user <username> identified by <password>;
```

###MySQL Database
MySQL grants permissions to users based on the source of the connection as well as the username. Typical sources can be:
- `%` - All hosts
- `localhost` - The localhost (127.0.0.1) network address
- `hostname` - A specific hostname

Identify the users who are defined in the database:
```
mysql> use mysql;
mysql> select user, host from user where user = '<username>';
```

For each of the entries present for a particular user, issue a change password command:
```
mysql> set password for 'username'@'%' = PASSWORD('new_password')';
mysql> set password for 'username'@'localhost' = PASSWORD('new_password')';
mysql> set password for 'username'@'hostname' = PASSWORD('new_password')';
```

###PostgreSQL Database
To change the password for a user in PostgreSQL, connect to the database as either the user or an administrator:
```
psql -U username -d database
database=> \password username
Enter new password:
Enter it again:
```

##Changing HDP Passwords
Within the HDP ecosystem, there area  number of services that utilize database, encryption, or administrator passwords.

###Ambari admin user:
The typical recommendation is to disable the local admin acount in Ambari if integrated with LDAP/AD for user login. However, if the admin user is enabled, the password should be changed regularly. To change the admin user's password, login to Ambari as an administrator. Navigate to `User Drop Down -> Manage Ambari -> Users -> admin -> Change Password`

![Image](ambari_user_dropdown.png?raw=true)

![Image](ambari_chagne_pw.png?raw=true)

###Ambari database user:
Ambari stores its data in a relational database. This databae can be one of several types. To change the user's password, first change it in the database where 

