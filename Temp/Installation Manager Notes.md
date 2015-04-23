[wasadmin@localhost IM]$ ./userinstc -h
help, -help, -h, -?  (all, <command>)
    Print short info about available commands and exit.
install  <packageId(_Version)(,featureN,featureM,...)>...
    Install packages or specific features.
version, -version
    Print the version of this application and exit.
-acceptLicense
    Indicate acceptance of the license agreement.
-accessRights, -aR  <access rights>
    Define the user as an admin, a nonAdmin or a group.  The default value is admin.  This setting ignores the system status.
-consoleMode, -c
    Run Installation Manager in console mode.
-dataLocation, -dL  <data-location>
    Specify a directory to hold internal Installation Manager data.
-log, -l  <log file>
    Create a log file from the program script execution.
-nl  <nl>
    Specify desired language to be used.
-passwordKey, -pK  (<passwordKey>)
    Provides password encryption key in UI or silent mode.
-preferences  <key>=<value>(,<key2>=<value2>...)
    Specify a preference value or a comma-delimited list of preference values to be used.
-record  <recordedFile>
    Response file that is recorded.
-showProgress, -sP
    Show progress.
-showVerboseProgress, -sVP
    Show verbose progress.
-silent, -s
    Run Installation Manager in silent mode.
-skipInstall, -sI  <data-location>
    Skip the actual product installation during Installation Manager operation.


./userinstc -acceptLicense -consoleMode -dataLocation /var/ibm/InstallationManager -preferences com.ibm.cic.common.core.preferences.preserveDownloadedArtifacts=false -sP

-preferences
-preferences



IBM Installation Manager

The folders of Installation Manager:
* Binary Installation Fol

userinstc -c -dataLocation /opt/IBM/InstallationManager/IMData
