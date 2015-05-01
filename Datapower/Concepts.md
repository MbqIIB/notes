Disaster recovery mode allows you to create a secure backup to recover the configuration of an appliance.
The secure backup contains private data like certificates, keys and user data.  The data in the backup is
decrypted and cannot be seen.

* DP has memory (volatile), flash memory (non-volatile memory) and auxiliary disk storage.  The file system
sits on the flash memory and when the auxiliary storage is configured it is mounted as two folders under 'local' and 'logstore'.  The name of the mounted folders is the name you give the auxiliary disk array.

* Datapower configuration is object based.  Everything is an object with properties.  Objects are scoped to domains.  Some objects are only avaiable from 'default' domain.
 * The domain config is stored:
 a) default domain: 'config/autoconfig.cfg'
 b) other domains : 'config/domainname.cfg' e.g. domain name: Dev -> 'config/Dev.cfg'
 * Although objects are individual entities that can be viewed and managed independently. Objects can reference other objects by name and thus building a heirarchy of objects. One object can be referenced by multiple other objects `two services referencing the same policy`.
 * Any object can be exported and imported indvidually.  The exported configuration object is represented in XML. The saved object config in the device is in key-value pair.
 * Any object has status that can be viewed.  There are three statuses:
  *  config status: can be `saved`, `New`, ...etc
  *  op-state: runtime state can be `up` or `down`
  *  admin-state: can be `enabled` or `disabled`.  This is the intentional state changed by admin
* Datapower has two configurations `Running Configuration` and `Saved Configuration`.  When a change is done to the configuration `e.g. an object is created, modified or deleted`, the change takes effect immediately because it changes the `Running Configuration`.  If the device or domain is restarted, then the configuration that is not saved is lost and the saved configuration is reloaded.
* Domain Restart does two things `Reference: section 3.4.3  Restarting and resetting domains or redbook 'DataPower SOA Appliance Administration, Deployment, and Best Practices'`:
 * Does not wait for in-flight transactions and terminates them.  If you want to wait for in-flight transactions, then quiesce the domain before restarting.
 * Reloads the saved configuration and any unsaved configuration is lost.
 


 
* The folders of any domain are visible from the default domain.  e.g.: the local folder of 'Dev' domain will be visible from default domain under 'local/Dev' and so the rest of the folders.
