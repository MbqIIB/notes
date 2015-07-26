#### Installation Manager Folders
* Installation Directory
* Agent Data Location:
	* contains information about installed packages. This directory is required to update, modify, roll back, or uninstall packages. Stored information includes the state and history of operations
	* default location:
		* Unix/Linux:
			* Admin: `/var/ibm/InstallationManager`
			* Non-Admin: `/user_home_directory/var/ibm/InstallationManager`
	* to change from default provide `-dL` or `-dataLocation` directives followed by path to a folder to the IM installation executable (GUI or non-GUI)
	* this location is stored as property `cic.appDataLocation` in installation manager `config.ini`
	* this property can also be viewed from GUI: `Help -> About Installation Manager -> Installation Details`
	* this location cannot be changed after installation
	* [Reference: Agent data location](http://www-01.ibm.com/support/knowledgecenter/SSDV2W_1.8.0/com.ibm.silentinstall12.doc/topics/r_app_data_loc.html)
* Registry File
	* stores information about the installed version and the installation location
	* location of the file:
		* Unix/Linux:
			* Admin: `/etc/.ibm/registry/InstallationManager.dat`
			* Non-admin: `user_home_directory/etc/.ibm/registry/InstallationManager.dat`
	* The following are the significant properties:
		* `location` : location of IM installation
		* `appDataLocation`: agent data location
	* [Reference](http://www-01.ibm.com/support/knowledgecenter/SSDV2W_1.8.0/com.ibm.silentinstall12.doc/topics/r_app_data_loc.html)

#### config.ini
* contains configuration parameters for IM
* located in `IM Installation Directory/eclipse/configuration/config.ini`

#### [Package groups and the shared resources directory](http://www-01.ibm.com/support/knowledgecenter/SSDV2W_1.8.0/com.ibm.cic.agent.ui.doc/topics/c_install_location.html)

#### Logging
* logs are written under `Agent DataLocation/logs`.
* The logs files are xml files with timestamp as name.
* In the same folder `log.properties` file has logging level configuration.
* by default `log.properties` does not exist. A sample file exist for this property file in the same folder `sample.properties`
* to enable `DEBUG` level: just copy `sample.properties` as `log.properties` and start IM

#### Troubleshooting
* IM by default tries to obtain lock on repository files.  If the files are already locked, the IM might hang waiting to acquire lock.  To resolve this, tell IM not to lock files by specifying `cic.repo.locking=false` in `config.ini` file.
	* [Reference](http://www-01.ibm.com/support/docview.wss?uid=swg21628575)






------

*IM is a java application and comes with its own jre.  There are executable commands that act as interface into this java application.


* imcl (Installation Manager Command Line)

http://www-01.ibm.com/support/docview.wss?uid=swg21438774

http://www-01.ibm.com/support/docview.wss?uid=swg21438776

http://www-01.ibm.com/support/docview.wss?uid=swg21672676

http://www-01.ibm.com/support/docview.wss?uid=swg21692626

http://www-01.ibm.com/support/docview.wss?uid=swg21330190

http://www-01.ibm.com/support/docview.wss?uid=swg21631478

http://www-01.ibm.com/support/docview.wss?uid=swg21443380

http://www-01.ibm.com/support/docview.wss?uid=swg21307121

http://www-01.ibm.com/support/docview.wss?uid=swg21665878

http://www-01.ibm.com/support/knowledgecenter/SSDV2W_1.7.3/com.ibm.silentinstall12.doc/topics/r_app_data_loc.html?lang=en

http://www-01.ibm.com/support/docview.wss?uid=swg21425908

http://www-01.ibm.com/support/knowledgecenter/SSDV2W_1.8.2/com.ibm.cic.commandline.doc/topics/r_imcl_installer.html

http://www-01.ibm.com/support/knowledgecenter/SSDV2W_1.8.2/com.ibm.cic.commandline.doc/topics/r_tools_imcl.html



./imcl -silent -acceptLicense -showProgress -input /tmp/WASInstall.xml -log /tmp/WASInstall.log


./IBMIM -record /tmp/aaa.xml -skipInstall /tmp/myregistry


http://www-01.ibm.com/support/knowledgecenter/SSSH27_7.1.1/com.ibm.rational.clearcase.cc_ms_install.doc/topics/c_selecting_location.htm

Package installation directory
Package group directory
shared resource directory


Changing package group name
http://www-01.ibm.com/support/docview.wss?uid=swg21298079

IBM software product compatibility report
http://www-969.ibm.com/software/reports/compatibility/clarity/index.jsp
