CellOnlyDb : BPM_DB_CELL_ALIAS, CELLDBPS
CommonDb : BPM_DB_CMN_ALIAS, CMNDBPS
ProcessServerDb : BPM_DB_PS_ALIAS, BPMDBPS
PerformanceDb : BPM_DB_PDW_ALIAS, PDWDBPS
BPCDb : BPM_DB_BPC_ALIAS, BPCDBPS
BPCArchiveDb : BPM_DB_BPARC_ALIAS, BPARCDBPS
MessagingDb : BPM_DB_ME_ALIAS, MEDBPS
BusinessSpaceDb : BPM_DB_BS_ALIAS, BSDBPS

-create -sqlfiles C:\temp\config\Advanced-PS-ThreeClusters-Oracle.properties -outputDir C:\temp\config

bpm.de.db.1.dbCapabilities=Messaging,BusinessSpace,CommonDB,BPC
bpm.de.db.2.dbCapabilities=ProcessServer,EmbeddedECM
bpm.de.db.3.dbCapabilities=PDW
bpm.de.db.4.dbCapabilities=CellScopedDB
