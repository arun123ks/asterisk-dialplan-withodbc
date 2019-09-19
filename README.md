# asteriskdialplanwithodbc

How to do db query in asterisk dialplan using odbc .

1. Create Database Connection in odbc.ini

Open file /etc/odbc.ini and do add below lines in that file 

[MySQL-ivrtest]
Description=MySQL connection to 'ivrtest' database
driver=MySQL
server=localhost
database=ivrtest
Port=3306
Socket=/var/lib/mysql/mysql.sock
option=3
Charset=utf8

Note : change database name and server as per your configuration.



2. Configure DSN in res_odbc_custom.conf

Open file /etc/asterisk/res_odbc_custom.conf  and add below lines 

[ivrtest]
enabled=>yes
dsn=>MySQL-ivrtest
pre-connect=>yes
max_connections=>5
username=>ivrusr
password=>ivrtestusr
database=>ivrtest

Note : change database name , username and password as per your configuration 






3. Configure function in func_odbc.conf

Open file /etc/asterisk/func_odbc.conf and add below lines


[CALLERINFO]
prefix=DDI
dsn=ivrtest
readsql=SELECT connect_id FROM customer_account WHERE telnr_customer = '${ARG1}'


[CONNDEST]
prefix=DDI
dsn=ivrtest
readsql=SELECT connect_destination from call_connect_destination where connect_id =  '${ARG1}'

[CONNDESTYPE]
prefix=DDI
dsn=ivrtest
readsql=SELECT connect_destination_notice from call_connect_destination where  connect_destination =  '${ARG1}'
 
[NEWCUST]
prefix=DDI
dsn=ivrtest
readsql=INSERT into customer_account (telnr_customer, connect_id, textinfo_customer) values ('${ARG1}','1','NEW')



4. write dialplan in extension_custom.conf 









