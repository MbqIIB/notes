Start Database:
===============
[oracle@oradb ~]$ sqlplus / as sysdba

SQL> startup

Start listener:
===============
[oracle@oradb ~]$ lsnrctl start

Start enterprise manager:
=========================
[oracle@oradb ~]$ emctl start dbconsole


To shutdown database:
=====================
SQL> shutdown immediate
