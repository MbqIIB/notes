SIBus Database Notes:

* The user used to connect to messaging engine schema needs privilege to truncate.  This privilege is given already in the script that creates message engine tables:
'GRANT DROP ANY TABLE TO __DBUSER__;'
In oracle 'DROP ANY TABLE' allows a user to truncate any table as well.
