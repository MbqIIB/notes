* deadmin user in BPM does not have the privileges that wasadmin user has.  Only three administrative roles are assigned to him: Operator, Deployer, Administrator
For example, deadmin user will not see the links `Users and Groups -> Administrative user roles` and `Users and Groups -> Administrative group roles` because the administrative role `Admin Security Manager` is not assigned to him

* ==**Question**==: is changing authentication alias password of "DeAdminAlias" in "Security -> Business Integration Security" panel changes the password in the "run as" Java EE roles of BPM applications as well?
