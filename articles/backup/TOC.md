
# Áttekintés
## [Mi az az Azure Backup?](backup-introduction-to-azure-backup.md)

# Bevezetés
## [Azure-beli virtuális gépek biztonsági mentése](backup-azure-vms-first-look-arm.md)
## [Windows Server vagy Windows rendszerű számítógépek biztonsági mentése](backup-try-azure-backup-in-10-mins.md)
## [VMware-kiszolgálók biztonsági mentése](backup-azure-backup-server-vmware.md)

# Útmutató

## Azure Backup Server
### [Azure Backup Server védelmi mátrix](backup-mabs-protection-matrix.md)
### Telepítés vagy frissítés
#### [Azure Backup Server számítási feladatainak előkészítése az Azure Portalon](backup-azure-microsoft-azure-backup.md)
#### [Azure Backup Server számítási feladatainak előkészítése a klasszikus portálon](backup-azure-microsoft-azure-backup-classic.md)
#### [Tárterület hozzáadása az Azure Backup Serverhez](backup-mabs-add-storage.md)
#### [Azure Backup Server frissítése a 2-es verzióra](backup-mabs-upgrade-to-v2.md)
#### [Az Azure Backup Server felügyelet nélküli telepítése](backup-mabs-unattended-install.md)
### Számítási feladatok védelme
#### [VMware-kiszolgáló biztonsági mentése az Azure Backup Server használatával](backup-azure-backup-server-vmware.md)
#### [Az Exchange biztonsági mentése az Azure Backup Server használatával](backup-azure-exchange-mabs.md)
#### [SharePoint-farm biztonsági mentése az Azure Backup Server használatával](backup-azure-backup-sharepoint-mabs.md)
#### [Az SQL biztonsági mentése az Azure Backup Server használatával](backup-azure-sql-mabs.md)
#### [A rendszer állapotának védelme és operációs rendszer nélküli helyreállítás](backup-mabs-system-state-and-bmr.md)
### [Adatok helyreállítása az Azure Backup Serverről](backup-azure-alternate-dpm-server.md)

## Azure-beli virtuális gépek
### A virtuális gép előkészítése
#### [A Resource Managerrel üzembe helyezett virtuális gépek előkészítése](backup-azure-arm-vms-prepare.md)
#### [Linux virtuális gépek alkalmazáskonzisztens biztonsági mentései](backup-azure-linux-app-consistent.md)
#### [Az Azure-beli virtuális gépek előkészítése](backup-azure-vms-prepare.md)
### A környezet megtervezése
#### [Virtuális gépek biztonsági mentési infrastruktúrájának tervezése](backup-azure-vms-introduction.md)
### Virtuális gépek biztonsági mentése
#### [Azure-beli virtuális gépek biztonsági mentése Recovery Services-tárolóba](backup-azure-arm-vms.md)
#### [Titkosított virtuális gépek biztonsági mentése](backup-azure-vms-encryption.md)
#### [Azure-beli virtuális gépek biztonsági mentése](backup-azure-vms.md)
### Virtuális gépek kezelése és monitorozása
#### [Azure-beli virtuális gépek biztonsági másolatainak kezelése az Azure Portalon](backup-azure-manage-vms.md)
#### [Azure-beli virtuális gépek biztonsági másolataival kapcsolatos riasztások figyelése az Azure Portalon](backup-azure-monitor-vms.md)
#### [Azure-beli virtuális gépek biztonsági másolatainak kezelése és figyelése a klasszikus portálon](backup-azure-manage-vms-classic.md)
### Virtuális gépek adatainak visszaállítása
#### [Fájlok helyreállítása Azure-beli virtuális gépek biztonsági másolataiból](backup-azure-restore-files-from-vm.md)
#### [A Resource Managerrel üzembe helyezett virtuális gépek visszaállítása az Azure Portalon](backup-azure-arm-restore-vms.md)
#### [Titkosított virtuális gépek visszaállítása](backup-azure-vms-encryption.md)
#### [Virtuális gépek visszaállítása az Azure-ban](backup-azure-restore-vms.md)
#### [A titkosított virtuális gépekhez tartozó Key Vault-kulcs és titkos kulcs visszaállítása](backup-azure-restore-key-secret.md)

## Azure Backup-jelentések konfigurálása
### [Azure Backup-jelentések konfigurálása](backup-azure-configure-reports.md)
### [Adatmodell az Azure Backup-jelentésekhez](backup-azure-reports-data-model.md)
### [Naplóelemzési adatmodell az Azure Backuphoz](backup-azure-log-analytics-data-model.md)

## Data Protection Manager
### [A DPM számítási feladatainak előkészítése az Azure Portalon](backup-azure-dpm-introduction.md)
### [A DPM számítási feladatainak előkészítése a klasszikus portálon](backup-azure-dpm-introduction-classic.md)
### [A System Center DPM használata Exchange-kiszolgálók biztonsági mentéséhez](backup-azure-backup-exchange-server.md)
### [Adatok helyreállítása egy másik DPM-kiszolgálóra](backup-azure-alternate-dpm-server.md)
### [A DPM használata az SQL Server számítási feladatainak biztonsági mentéséhez](backup-azure-backup-sql.md)
### [A DPM használata SharePoint-farmok biztonsági mentéséhez](backup-azure-backup-sharepoint.md)

## A PowerShell használata
### [Azure-beli virtuális gépek az Azure Portalon](backup-azure-vms-automation.md)
### [Azure-beli virtuális gépek a klasszikus portálon](backup-azure-vms-classic-automation.md)
### [DPM az Azure Portalon](backup-dpm-automation.md)
### [DPM a klasszikus Azure Portalon](backup-dpm-automation-classic.md)
### [Windows Server az Azure Portalon](backup-client-automation.md)
### [Windows Server a klasszikus portálon](backup-client-automation-classic.md)

## Azure SQL Database
### [A biztonsági mentések hosszú távú megőrzésének konfigurációja](../sql-database/sql-database-configure-long-term-retention.md?toc=%2fazure%2fbackup%2ftoc.json)
### [Biztonsági mentések megtekintése a Recovery Services-tárolóban](../sql-database/sql-database-view-backups-in-vault.md?toc=%2fazure%2fbackup%2ftoc.json)
### [Visszaállítás a biztonsági másolat hosszú távú megőrzéséből](../sql-database/sql-database-restore-from-long-term-retention.md?toc=%2fazure%2fbackup%2ftoc.json)
### [Az Azure SQL hosszú távú biztonsági másolatainak törlése](../sql-database/sql-database-long-term-retention-delete.md?toc=%2fazure%2fbackup%2ftoc.json)

## Windows Server
### [Windows Serveren lévő fájlok és mappák biztonsági mentése](backup-configure-vault.md)
### [A Windows Server rendszerállapotának biztonsági mentése](backup-azure-system-state.md)
### [Azure-beli fájlok helyreállítása Windows Serverre](backup-azure-restore-windows-server.md)
### [A Windows Server rendszerállapotának visszaállítása](backup-azure-restore-system-state.md)
### [Recovery Services-tárolók figyelése és kezelése](backup-azure-manage-windows-server.md)
### Biztonsági mentés és visszaállítás a klasszikus portállal
#### [Klasszikus üzemi modellt használó Windows Server](backup-configure-vault-classic.md)
#### [Backup-tárolók kezelése a klasszikus üzemi modellel](backup-azure-manage-windows-server-classic.md)
#### [Fájlok helyreállítása a Windows-kiszolgálóra a klasszikus üzemi modell használatával](backup-azure-restore-windows-server-classic.md)

## Recovery Services-tároló
### [A Recovery Services-tárolók áttekintése](backup-azure-recovery-services-vault-overview.md)
### [Backup tároló frissítése Recovery Services-tárolóra](backup-azure-upgrade-backup-to-recovery-services.md)
### [Recovery Services-tároló törlése](backup-azure-delete-vault.md)

## Hibaelhárítás
### [Azure-beli virtuális gép biztonsági mentésével kapcsolatos problémák az Azure Portalon](backup-azure-vms-troubleshoot.md)
### [Azure-beli virtuális gép biztonsági mentésével kapcsolatos problémák a klasszikus portálon](backup-azure-vms-troubleshoot-classic.md)
### [Azure-beli virtuális gép mentése sikertelen: nem sikerült kommunikálni a virtuálisgép-ügynökkel a pillanatkép-állapot lekérése érdekében – a virtuális gép részfeladat időtúllépést okozott](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md)
### [Az Azure Backup-fájlok és -mappák lassú biztonsági mentése](backup-azure-troubleshoot-slow-backup-performance-issue.md)
### [Az Azure Backup Server hibaelhárítása](backup-azure-mabs-troubleshoot.md)

# Alapelvek

## GYIK
### [Gyakori kérdések a Recovery Services-tárolókról](backup-azure-backup-faq.md)
### [Gyakori kérdések az Azure-beli virtuális gépek biztonsági mentéséről](backup-azure-vm-backup-faq.md)
### [Gyakori kérdések a fájlok és mappák Azure Backup-ügynökkel végzett biztonsági mentéséről](backup-azure-file-folder-backup-faq.md)

## [Szerepköralapú hozzáférés-vezérlés](backup-rbac-rs-vault.md)
## [Biztonsági szolgáltatások hibrid biztonsági mentésekhez](backup-azure-security-feature.md)
## [Offline biztonsági mentés konfigurálása](backup-azure-backup-import-export.md)
## [A szalagtár lecserélése](backup-azure-backup-cloud-as-tape.md)


# Referencia
## [PowerShell](/powershell/module/azurerm.recoveryservices.backup)
## [.NET](/dotnet/api/microsoft.azure.management.recoveryservices.backup)

# Erőforrások
## [Azure-ütemterv](https://azure.microsoft.com/roadmap/)
## [MSDN-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowsazureonlinebackup)
## [Díjszabás](https://azure.microsoft.com/pricing/details/backup/)
## [Díjkalkulátor](https://azure.microsoft.com/pricing/calculator/)
## [Szolgáltatási hírek](https://azure.microsoft.com/updates/?product=backup)
## [Videók](https://azure.microsoft.com/documentation/videos/index/?services=backup)
