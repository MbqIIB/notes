WSRR Oracle Database Requirements:

NLS_NCHAR_CHARACTERSET = UTF8
NLS_CHARACTERSET = AL32UTF8

Schema and database names must be less than eight characters 

select value from NLS_DATABASE_PARAMETERS where parameter=’NLS_NCHAR_CHARACTERSET';
