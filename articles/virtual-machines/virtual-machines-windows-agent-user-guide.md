---
title: "Virtuálisgép-ügynök – áttekintés aaaAzure |} Microsoft Docs"
description: "Az Azure virtuális gép ügynök – áttekintés"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 0a1f212e-053e-4a39-9910-8d622959f594
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/28/2017
ms.author: nepeters
ms.openlocfilehash: 178766925673419cd661dbb460b8427bbfaf54e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machine-agent-overview"></a><span data-ttu-id="dd32f-103">Az Azure virtuális gép ügynökének áttekintése</span><span class="sxs-lookup"><span data-stu-id="dd32f-103">Azure Virtual Machine Agent overview</span></span>

<span data-ttu-id="dd32f-104">Microsoft Azure virtuális gép ügynökének (Virtuálisgép-ügynök) hello egy biztonságos, egyszerű folyamat hello Azure Fabric Controller interakcióba VM kezeli.</span><span class="sxs-lookup"><span data-stu-id="dd32f-104">hello Microsoft Azure Virtual Machine Agent (VM Agent) is a secured, lightweight process that manages VM interaction with hello Azure Fabric Controller.</span></span> <span data-ttu-id="dd32f-105">hello Virtuálisgép-ügynök engedélyezése és a végrehajtása az Azure virtuálisgép-bővítmények az elsődleges szerepkör tartozik.</span><span class="sxs-lookup"><span data-stu-id="dd32f-105">hello VM Agent has a primary role in enabling and executing Azure virtual machine extensions.</span></span> <span data-ttu-id="dd32f-106">Virtuálisgép-bővítmények, amely lehetővé teszi a virtuális gépek, például a telepítése és beállítása a szoftver központi telepítési konfigurációs utáni.</span><span class="sxs-lookup"><span data-stu-id="dd32f-106">VM Extensions enabling post deployment configuration of virtual machines, such as installing and configuring software.</span></span> <span data-ttu-id="dd32f-107">Virtuálisgép-bővítmények is engedélyezheti a helyreállítási szolgáltatás például a virtuális gépek hello a rendszergazdai jelszó alaphelyzetbe állításával.</span><span class="sxs-lookup"><span data-stu-id="dd32f-107">Virtual machine extensions also enable recovery features such as resetting hello administrative password of a virtual machine.</span></span> <span data-ttu-id="dd32f-108">Nélkül hello Azure Virtuálisgép-ügynök a virtuálisgép-bővítmények nem lehet futtatni.</span><span class="sxs-lookup"><span data-stu-id="dd32f-108">Without hello Azure VM Agent, virtual machine extensions cannot be run.</span></span>

<span data-ttu-id="dd32f-109">Ez a dokumentum a telepítés, észlelési és hello Azure virtuális gép ügynök eltávolításának részletes.</span><span class="sxs-lookup"><span data-stu-id="dd32f-109">This document details installation, detection, and removal of hello Azure Virtual Machine Agent.</span></span>

## <a name="install-hello-vm-agent"></a><span data-ttu-id="dd32f-110">Hello Virtuálisgép-ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="dd32f-110">Install hello VM Agent</span></span>

### <a name="azure-gallery-image"></a><span data-ttu-id="dd32f-111">Azure katalógusában kép</span><span class="sxs-lookup"><span data-stu-id="dd32f-111">Azure gallery image</span></span>

<span data-ttu-id="dd32f-112">hello Azure Virtuálisgép-ügynök telepítve van a Windows virtuális gépek Azure-katalógus lemezképből telepített alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="dd32f-112">hello Azure VM Agent is installed by default on any Windows virtual machine deployed from an Azure Gallery image.</span></span> <span data-ttu-id="dd32f-113">Hello portál, a PowerShell, a parancssori felületen vagy a Azure Resource Manager-sablonok az Azure katalógusában lemezképének telepítésekor hello Azure Virtuálisgép-ügynök is van telepítve.</span><span class="sxs-lookup"><span data-stu-id="dd32f-113">When deploying an Azure gallery image from hello Portal, PowerShell, Command Line Interface, or an Azure Resource Manager template, hello Azure VM Agent is also be installed.</span></span> 

### <a name="manual-installation"></a><span data-ttu-id="dd32f-114">Manuális telepítés</span><span class="sxs-lookup"><span data-stu-id="dd32f-114">Manual installation</span></span>

<span data-ttu-id="dd32f-115">hello Windows Virtuálisgép-ügynök manuálisan telepíthető a Windows installer-csomag.</span><span class="sxs-lookup"><span data-stu-id="dd32f-115">hello Windows VM agent can be manually installed using a Windows installer package.</span></span> <span data-ttu-id="dd32f-116">Manuális telepítés akkor lehet szükség, ha egy egyéni virtuálisgép-lemezkép létrehozása, amely telepíti az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="dd32f-116">Manual installation may be necessary when creating a custom virtual machine image that will be deployed in Azure.</span></span> <span data-ttu-id="dd32f-117">toomanually telepítés hello Virtuálisgép-ügynök a Windows hello Virtuálisgép-ügynök telepítő letölteni erről a helyről [Windows Azure VM ügynök letöltése](http://go.microsoft.com/fwlink/?LinkID=394789).</span><span class="sxs-lookup"><span data-stu-id="dd32f-117">toomanually install hello Windows VM Agent, download hello VM Agent installer from this location [Windows Azure VM Agent Download](http://go.microsoft.com/fwlink/?LinkID=394789).</span></span> 

<span data-ttu-id="dd32f-118">Virtuálisgép-ügynök hello hello windows installer fájlra duplán kattintva is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="dd32f-118">hello VM Agent can be installed by double-clicking hello windows installer file.</span></span> <span data-ttu-id="dd32f-119">Az automatikus vagy felügyelet nélküli telepítése hello Virtuálisgép-ügynök futtassa le a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="dd32f-119">For an automated or unattended installation of hello VM agent, run hello following command.</span></span>

```cmd
msiexec.exe /i WindowsAzureVmAgent.2.7.1198.778.rd_art_stable.160617-1120.fre /quiet
```

## <a name="detect-hello-vm-agent"></a><span data-ttu-id="dd32f-120">Virtuálisgép-ügynök hello észlelése</span><span class="sxs-lookup"><span data-stu-id="dd32f-120">Detect hello VM Agent</span></span>

### <a name="powershell"></a><span data-ttu-id="dd32f-121">PowerShell</span><span class="sxs-lookup"><span data-stu-id="dd32f-121">PowerShell</span></span>

<span data-ttu-id="dd32f-122">hello Azure Resource Manager PowerShell modul használt tooretrieve információk kapcsolatos Azure virtuális gépek is lehet.</span><span class="sxs-lookup"><span data-stu-id="dd32f-122">hello Azure Resource Manager PowerShell module can be used tooretrieve information about Azure Virtual Machines.</span></span> <span data-ttu-id="dd32f-123">Futó `Get-AzureRmVM` meglehetősen bit, többek között az üzembe helyezési állapota hello Azure Virtuálisgép-ügynök a hello adja vissza.</span><span class="sxs-lookup"><span data-stu-id="dd32f-123">Running `Get-AzureRmVM` returns quite a bit of information including hello provisioning state for hello Azure VM Agent.</span></span>

```PowerShell
Get-AzureRmVM
```

<span data-ttu-id="dd32f-124">hello az alábbiakban látható hello egy részhalmazát `Get-AzureRmVM` kimeneti.</span><span class="sxs-lookup"><span data-stu-id="dd32f-124">hello following is just a subset of hello `Get-AzureRmVM` output.</span></span> <span data-ttu-id="dd32f-125">Értesítés hello `ProvisionVMAgent` tulajdonság ágyazott `OSProfile`, ez a tulajdonság lehet használt toodetermine, ha hello Virtuálisgép-ügynök üzembe helyezett toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="dd32f-125">Notice hello `ProvisionVMAgent` property nested inside `OSProfile`, this property can be used toodetermine if hello VM agent has been deployed toohello virtual machine.</span></span>

```PowerShell
OSProfile                  :
  ComputerName             : myVM
  AdminUsername            : muUserName
  WindowsConfiguration     :
    ProvisionVMAgent       : True
    EnableAutomaticUpdates : True
```

<span data-ttu-id="dd32f-126">a következő parancsfájl hello lehet használt tooreturn virtuális gépek neveit és a Virtuálisgép-ügynök hello hello állapotának tömör listáját.</span><span class="sxs-lookup"><span data-stu-id="dd32f-126">hello following script can be used tooreturn a concise list of virtual machine names and hello state of hello VM Agent.</span></span>

```PowerShell
$vms = Get-AzureRmVM

foreach ($vm in $vms) {
    $agent = $vm | Select -ExpandProperty OSProfile | Select -ExpandProperty Windowsconfiguration | Select ProvisionVMAgent
    Write-Host $vm.Name $agent.ProvisionVMAgent
}
```

### <a name="manual-detection"></a><span data-ttu-id="dd32f-127">Manuális észlelés</span><span class="sxs-lookup"><span data-stu-id="dd32f-127">Manual Detection</span></span>

<span data-ttu-id="dd32f-128">Amikor bejelentkezik a Windows Azure VM tooa, a Feladatkezelő futó folyamatok használt tooexamine is lehet.</span><span class="sxs-lookup"><span data-stu-id="dd32f-128">When logged in tooa Windows Azure VM, task manager can be used tooexamine running processes.</span></span> <span data-ttu-id="dd32f-129">toocheck a hello Azure Virtuálisgép-ügynök, nyissa meg a Feladatkezelőt > hello részletek fülre, és keresse meg a folyamat nevét `WindowsAzureGuestAgent.exe`.</span><span class="sxs-lookup"><span data-stu-id="dd32f-129">toocheck for hello Azure VM Agent, open Task Manager > click hello details tab, and look for a process name `WindowsAzureGuestAgent.exe`.</span></span> <span data-ttu-id="dd32f-130">hello folyamat jelzi, hogy hello Virtuálisgép-ügynök telepítve van.</span><span class="sxs-lookup"><span data-stu-id="dd32f-130">hello presence of this process indicates that hello VM agent is installed.</span></span>

## <a name="upgrade-hello-vm-agent"></a><span data-ttu-id="dd32f-131">Hello Virtuálisgép-ügynök frissítése</span><span class="sxs-lookup"><span data-stu-id="dd32f-131">Upgrade hello VM Agent</span></span>

<span data-ttu-id="dd32f-132">hello Azure virtuális gép Windows ügynök automatikusan frissítve.</span><span class="sxs-lookup"><span data-stu-id="dd32f-132">hello Azure VM Agent for Windows is automatically upgraded.</span></span> <span data-ttu-id="dd32f-133">Új virtuális gépekre telepített tooAzure vannak, akkor kapnak hello legújabb Virtuálisgép-ügynök.</span><span class="sxs-lookup"><span data-stu-id="dd32f-133">As new virtual machines are deployed tooAzure, they receive hello latest VM agent.</span></span> <span data-ttu-id="dd32f-134">Egyéni Virtuálisgép-rendszerképek kell manuálisan frissített tooinclude hello új Virtuálisgép-ügynök.</span><span class="sxs-lookup"><span data-stu-id="dd32f-134">Custom VM images should be manually updated tooinclude hello new VM agent.</span></span>
