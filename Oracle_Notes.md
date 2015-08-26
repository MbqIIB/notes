* Start Database:
```
[oracle@oradb ~]$ sqlplus / as sysdba

SQL> startup
```
* Start listener:
```
[oracle@oradb ~]$ lsnrctl start
```
* Start enterprise manager:
```
[oracle@oradb ~]$ emctl start dbconsole
```
* To shutdown database:
```
SQL> shutdown immediate
```

* SQLPlus scripts:
  * To combine multiple scripts related to different schemas, you can use the following to switch the schema:
    * `ALTER SESSION SET CURRENT_SCHEMA = SCHEMA_NAME`;

### Concepts

* [Is there a benefit to have more than  tablespace](http://stackoverflow.com/questions/1291317/should-an-oracle-database-have-more-than-one-tablespace-for-data-storage)
