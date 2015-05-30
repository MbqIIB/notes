1) make sure that the hostname in the config file is added to hosts file or DNS and test it
2) BPMConfig.sh -create -de aaa.properties
3) run the DB scripts
4) start deployment manager
5) Fix JDBC and datasource issues and point to the Oracle thin client jar
6) in deployment manager profile bin directory: bootstrapProcessServerData.sh -clusterName cluster_name
7) in deployment environment page, click 'configuration done' in deffered configuration
8) start node of all nodes
9) start deployment environment and make sure there are no errors in logs
10) in deployment environment, click on health center and make that all status are ok.
