Oracle Role vs Privilege
* System privilege and object privileges

https://docs.oracle.com/cd/E11882_01/server.112/e10897/users_secure.htm#ADMQS007
http://docs.oracle.com/cd/E11882_01/network.112/e36292/authorization.htm#DBSEG004

* A user owner of schema has default privileges 'SELECT, 
* 'RESOURCE' role: Enables a user to create, modify, and delete certain types of schema objects in the schema associated with that user.  It does not grant 'CREATE VIEW' privilege.  It has to be granted explicitly.
* 'CREATE INDEX' To create an index in your own schema, one of the following conditions must be true:
    a. The table or cluster to be indexed must be in your own schema.  If a user is able to create table then he will be to create index since the table object exists.
    b. You must have the INDEX object privilege on the table to be indexed.  'GRANT INDEX ON SCHEMA.TABLENAME TO ...'
    c. You must have the CREATE ANY INDEX system privilege.
Reference: http://docs.oracle.com/cd/B19306_01/server.102/b14200/statements_5010.htm#i2084975
* TRUNCATE and 'GRANT DROP ANY TABLE'
* security domain?? security domain when using 'create view' that excluded role??
* When executing 'CREATE VIEW' the privileges through roles are not available.  http://dba.stackexchange.com/questions/4137/when-creating-views-why-do-users-need-direct-object-permissions-if-they-already
