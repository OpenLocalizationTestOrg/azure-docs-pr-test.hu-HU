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
# <a name="run-an-unattended-installation-of-azure-backup-server-v2"></a><span data-ttu-id="82107-104">Az Azure Backup Server v2 felügyelet nélküli telepítés futtatása</span><span class="sxs-lookup"><span data-stu-id="82107-104">Run an unattended installation of Azure Backup Server v2</span></span>

<span data-ttu-id="82107-105">Megtudhatja, hogyan toorun Azure Backup Server v2 felügyelet nélküli telepítés.</span><span class="sxs-lookup"><span data-stu-id="82107-105">Learn how toorun an unattended installation of Azure Backup Server v2.</span></span> 

<span data-ttu-id="82107-106">Ezeket a lépéseket ne alkalmazza az Azure Backup Server v1 telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="82107-106">These steps do not apply if you are installing Azure Backup Server v1.</span></span>

## <a name="install-backup-server-v2"></a><span data-ttu-id="82107-107">Telepítse a helykiszolgáló biztonsági mentése v2</span><span class="sxs-lookup"><span data-stu-id="82107-107">Install Backup Server v2</span></span>

1. <span data-ttu-id="82107-108">Azure Backup Server v2 üzemeltető kiszolgálón hello hozzon létre egy szövegfájlt.</span><span class="sxs-lookup"><span data-stu-id="82107-108">On hello server that hosts Azure Backup Server v2, create a text file.</span></span> <span data-ttu-id="82107-109">(Hello fájlt létrehozhatja a Jegyzettömbben vagy egy másik szövegben szerkesztő.) MABSSetup.ini hello fájlt menteni.</span><span class="sxs-lookup"><span data-stu-id="82107-109">(You can create hello file in Notepad or in another text editor.) Save hello file as MABSSetup.ini.</span></span> 

2. <span data-ttu-id="82107-110">Illessze be a következő hello MABSSetup.ini fájl hello.</span><span class="sxs-lookup"><span data-stu-id="82107-110">Paste hello following code in hello MABSSetup.ini file.</span></span> <span data-ttu-id="82107-111">Cserélje le a szöveget hello hello zárójelek közé (\< \>) a környezet értékeivel.</span><span class="sxs-lookup"><span data-stu-id="82107-111">Replace hello text inside hello brackets (\< \>) with values from your environment.</span></span> <span data-ttu-id="82107-112">a következő szöveg hello példája:</span><span class="sxs-lookup"><span data-stu-id="82107-112">hello following text is an example:</span></span>

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

3. <span data-ttu-id="82107-113">Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="82107-113">Save hello file.</span></span> <span data-ttu-id="82107-114">Ezután egy rendszergazda jogú parancssorba a hello telepítési kiszolgáló, írja be ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="82107-114">Then, at an elevated command prompt on hello installation server, enter this command:</span></span>

  ```
  start /wait <cdlayout path>/Setup.exe /i  /f <.ini file path>/setup.ini /L <log path>/setup.log
  ```

<span data-ttu-id="82107-115">Ezek a jelölők hello telepítéséhez használhatja:</span><span class="sxs-lookup"><span data-stu-id="82107-115">You can use these flags for hello installation:</span></span></br><span data-ttu-id="82107-116">
**/f**: .ini-fájl elérési útja</span><span class="sxs-lookup"><span data-stu-id="82107-116">
**/f**: .ini file path</span></span></br><span data-ttu-id="82107-117">
**/ l**: napló elérési útja</span><span class="sxs-lookup"><span data-stu-id="82107-117">
**/l**: Log path</span></span></br><span data-ttu-id="82107-118">
**/i**: telepítési útvonala</span><span class="sxs-lookup"><span data-stu-id="82107-118">
**/i**: Installation path</span></span></br><span data-ttu-id="82107-119">
**/x**: távolítsa el az elérési út</span><span class="sxs-lookup"><span data-stu-id="82107-119">
**/x**: Uninstall path</span></span></br>

## <a name="next-steps"></a><span data-ttu-id="82107-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="82107-120">Next steps</span></span>
<span data-ttu-id="82107-121">Biztonsági kiszolgáló telepítése után megtudhatja, hogyan tooprepare kiszolgálóját, vagy indítsa el a védelmet a munkaterhelés.</span><span class="sxs-lookup"><span data-stu-id="82107-121">After you install Backup Server, learn how tooprepare your server, or begin protecting a workload.</span></span>

- [<span data-ttu-id="82107-122">A kiszolgálói biztonsági mentési feladatok előkészítése</span><span class="sxs-lookup"><span data-stu-id="82107-122">Prepare Backup Server workloads</span></span>](backup-azure-microsoft-azure-backup.md)
- [<span data-ttu-id="82107-123">Használja a VMware Server biztonsági másolat Server tooback</span><span class="sxs-lookup"><span data-stu-id="82107-123">Use Backup Server tooback up a VMware server</span></span>](backup-azure-backup-server-vmware.md)
- [<span data-ttu-id="82107-124">SQL Server biztonsági másolat Server tooback használata</span><span class="sxs-lookup"><span data-stu-id="82107-124">Use Backup Server tooback up SQL Server</span></span>](backup-azure-sql-mabs.md)
- [<span data-ttu-id="82107-125">Modern Backup-tárhelyre tooBackup kiszolgáló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="82107-125">Add Modern Backup Storage tooBackup Server</span></span>](backup-mabs-add-storage.md)
