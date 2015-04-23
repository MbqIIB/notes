Disaster recovery mode allows you to create a secure backup to recover the configuration of an appliance.
The secure backup contains private data like certificates, keys and user data.  The data in the backup is
decrypted and cannot be seen.

* DP has memory (volatile), flash memory (non-volatile memory) and auxiliary disk storage.  The file system
sits on the flash memory and when the auxiliary storage is configured it is mounted as two folders under 'local' and 'logstore'.  The name of the mounted folders is the name you give the auxiliary disk array.


Q/When a configuration object is changed, does it take effect immediatly or when the config is saved?
Running configuration vs Saved configuration

* The domain config is stored:
 a) default domain: 'config/autoconfig.cfg'
 b) other domains : 'config/domainname.cfg' e.g. domain name: Dev -> 'config/Dev.cfg'
 
* The folders of any domain are visible from the default domain.  e.g.: the local folder of 'Dev' domain will be visible from default domain under 'local/Dev' and so the rest of the folders.
