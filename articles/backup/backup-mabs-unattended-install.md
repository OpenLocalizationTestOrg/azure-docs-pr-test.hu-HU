---
title: "Azure Backup Server v2 csendes telepítése |} Microsoft Docs"
description: "PowerShell parancsfájl segítségével csendes telepítéséhez az Azure Backup Server v2. Az ilyen típusú telepítés felügyelet nélküli telepítés néven is ismert."
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
ms.openlocfilehash: 91778a67f9ef523aa87b7918197e0d0ded0f5702
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="run-an-unattended-installation-of-azure-backup-server-v2"></a><span data-ttu-id="77866-104">Az Azure Backup Server v2 felügyelet nélküli telepítés futtatása</span><span class="sxs-lookup"><span data-stu-id="77866-104">Run an unattended installation of Azure Backup Server v2</span></span>

<span data-ttu-id="77866-105">Útmutató az Azure Backup Server v2 felügyelet nélküli telepítés futtatása.</span><span class="sxs-lookup"><span data-stu-id="77866-105">Learn how to run an unattended installation of Azure Backup Server v2.</span></span> 

<span data-ttu-id="77866-106">Ezeket a lépéseket ne alkalmazza az Azure Backup Server v1 telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="77866-106">These steps do not apply if you are installing Azure Backup Server v1.</span></span>

## <a name="install-backup-server-v2"></a><span data-ttu-id="77866-107">Telepítse a helykiszolgáló biztonsági mentése v2</span><span class="sxs-lookup"><span data-stu-id="77866-107">Install Backup Server v2</span></span>

1. <span data-ttu-id="77866-108">A kiszolgálón, amelyen az Azure Backup Server v2 hozzon létre egy szövegfájlt.</span><span class="sxs-lookup"><span data-stu-id="77866-108">On the server that hosts Azure Backup Server v2, create a text file.</span></span> <span data-ttu-id="77866-109">(A fájlt létrehozhatja a Jegyzettömbben vagy más szövegszerkesztőben.) Mentse a fájlt MABSSetup.ini.</span><span class="sxs-lookup"><span data-stu-id="77866-109">(You can create the file in Notepad or in another text editor.) Save the file as MABSSetup.ini.</span></span> 

2. <span data-ttu-id="77866-110">Illessze be a következő kódot a MABSSetup.ini fájlban.</span><span class="sxs-lookup"><span data-stu-id="77866-110">Paste the following code in the MABSSetup.ini file.</span></span> <span data-ttu-id="77866-111">Cserélje le a szöveget a zárójelben (\< \>) a környezet értékeivel.</span><span class="sxs-lookup"><span data-stu-id="77866-111">Replace the text inside the brackets (\< \>) with values from your environment.</span></span> <span data-ttu-id="77866-112">A következő egy példa áll:</span><span class="sxs-lookup"><span data-stu-id="77866-112">The following text is an example:</span></span>

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

3. <span data-ttu-id="77866-113">Mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="77866-113">Save the file.</span></span> <span data-ttu-id="77866-114">Ezután, egy rendszergazda jogú parancssort a telepítési kiszolgálón, írja be ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="77866-114">Then, at an elevated command prompt on the installation server, enter this command:</span></span>

  ```
  start /wait <cdlayout path>/Setup.exe /i  /f <.ini file path>/setup.ini /L <log path>/setup.log
  ```

<span data-ttu-id="77866-115">Ezeknek a jelzőknek a telepítéshez használható:</span><span class="sxs-lookup"><span data-stu-id="77866-115">You can use these flags for the installation:</span></span></br><span data-ttu-id="77866-116">
**/f**: .ini-fájl elérési útja</span><span class="sxs-lookup"><span data-stu-id="77866-116">
**/f**: .ini file path</span></span></br><span data-ttu-id="77866-117">
**/ l**: napló elérési útja</span><span class="sxs-lookup"><span data-stu-id="77866-117">
**/l**: Log path</span></span></br><span data-ttu-id="77866-118">
**/i**: telepítési útvonala</span><span class="sxs-lookup"><span data-stu-id="77866-118">
**/i**: Installation path</span></span></br><span data-ttu-id="77866-119">
**/x**: távolítsa el az elérési út</span><span class="sxs-lookup"><span data-stu-id="77866-119">
**/x**: Uninstall path</span></span></br>

## <a name="next-steps"></a><span data-ttu-id="77866-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="77866-120">Next steps</span></span>
<span data-ttu-id="77866-121">Biztonsági kiszolgáló telepítése után megtudhatja, hogyan készíti elő a kiszolgálót, vagy indítsa el a védelmet a munkaterhelés.</span><span class="sxs-lookup"><span data-stu-id="77866-121">After you install Backup Server, learn how to prepare your server, or begin protecting a workload.</span></span>

- [<span data-ttu-id="77866-122">A kiszolgálói biztonsági mentési feladatok előkészítése</span><span class="sxs-lookup"><span data-stu-id="77866-122">Prepare Backup Server workloads</span></span>](backup-azure-microsoft-azure-backup.md)
- [<span data-ttu-id="77866-123">Készítsen biztonsági másolatot a VMware server Backup Server használatával</span><span class="sxs-lookup"><span data-stu-id="77866-123">Use Backup Server to back up a VMware server</span></span>](backup-azure-backup-server-vmware.md)
- [<span data-ttu-id="77866-124">Készítsen biztonsági másolatot az SQL Server biztonsági másolat-kiszolgáló használatával</span><span class="sxs-lookup"><span data-stu-id="77866-124">Use Backup Server to back up SQL Server</span></span>](backup-azure-sql-mabs.md)
- [<span data-ttu-id="77866-125">Modern biztonsági másolatokat tároló hozzáadása a helykiszolgáló biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="77866-125">Add Modern Backup Storage to Backup Server</span></span>](backup-mabs-add-storage.md)
