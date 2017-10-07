---
title: "Azure Backup Server v2 aaaSilent telepítése |} Microsoft Docs"
description: "Használjon egy PowerShell-parancsfájl toosilently telepítse az Azure Backup Server v2. Az ilyen típusú telepítés felügyelet nélküli telepítés néven is ismert."
services: backup
documentationcenter: " "
author: markgalioto
manager: carmonm
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/30/2017
ms.author: markgal;masaran
ms.openlocfilehash: 6b94b4a278bfcd5f8c5c363cb811bd8eec984243
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-an-unattended-installation-of-azure-backup-server-v2"></a>Az Azure Backup Server v2 felügyelet nélküli telepítés futtatása

Megtudhatja, hogyan toorun Azure Backup Server v2 felügyelet nélküli telepítés. 

Ezeket a lépéseket ne alkalmazza az Azure Backup Server v1 telepítésekor.

## <a name="install-backup-server-v2"></a>Telepítse a helykiszolgáló biztonsági mentése v2

1. Azure Backup Server v2 üzemeltető kiszolgálón hello hozzon létre egy szövegfájlt. (Hello fájlt létrehozhatja a Jegyzettömbben vagy egy másik szövegben szerkesztő.) MABSSetup.ini hello fájlt menteni. 

2. Illessze be a következő hello MABSSetup.ini fájl hello. Cserélje le a szöveget hello hello zárójelek közé (\< \>) a környezet értékeivel. a következő szöveg hello példája:

  ```
  [OPTIONS]
  UserName=administrator
  CompanyName=<Microsoft Corporation>
  SQLMachineName=localhost
  SQLInstanceName=<SQL instance name>
  SQLMachineUserName=administrator
  SQLMachinePassword=<admin password>
  SQLMachineDomainName=<machine domain>
  ReportingMachineName=localhost
  ReportingInstanceName=<reporting instance name>
  SqlAccountPassword=<admin password>
  ReportingMachineUserName=<username>
  ReportingMachinePassword=<reporting admin password>
  ReportingMachineDomainName=<domain>
  VaultCredentialFilePath=<vault credential full path and complete name>
  SecurityPassphrase=<passphrase>
  PassphraseSaveLocation=<passphrase save location>
  UseExistingSQL=<1/0 use or do not use existing SQL>
  ```

3. Hello fájl mentéséhez. Ezután egy rendszergazda jogú parancssorba a hello telepítési kiszolgáló, írja be ezt a parancsot:

  ```
  start /wait <cdlayout path>/Setup.exe /i  /f <.ini file path>/setup.ini /L <log path>/setup.log
  ```

Ezek a jelölők hello telepítéséhez használhatja:</br>
**/f**: .ini-fájl elérési útja</br>
**/ l**: napló elérési útja</br>
**/i**: telepítési útvonala</br>
**/x**: távolítsa el az elérési út</br>

## <a name="next-steps"></a>Következő lépések
Biztonsági kiszolgáló telepítése után megtudhatja, hogyan tooprepare kiszolgálóját, vagy indítsa el a védelmet a munkaterhelés.

- [A kiszolgálói biztonsági mentési feladatok előkészítése](backup-azure-microsoft-azure-backup.md)
- [Használja a VMware Server biztonsági másolat Server tooback](backup-azure-backup-server-vmware.md)
- [SQL Server biztonsági másolat Server tooback használata](backup-azure-sql-mabs.md)
- [Modern Backup-tárhelyre tooBackup kiszolgáló hozzáadása](backup-mabs-add-storage.md)
