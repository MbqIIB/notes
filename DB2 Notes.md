### Common commands

###### list databases:
* `db2 list db directory`

###### drop databse:
1. `db2 connect to 'dbname'`
2. `db2 quiesce db immediate force connections`
3. `db2 unquiesce db`
4. `db2 connect reset`
5. `db2 drop db 'dbname'`
