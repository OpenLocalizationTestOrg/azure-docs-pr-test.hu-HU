---
title: "Symantec Endpoint Protection szolgáltatás a Windows Azure-ban aaaInstall |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall és hello Symantec Endpoint Protection biztonsági kiterjesztés konfigurálása egy új vagy meglévő Azure virtuális Géphez létre hello klasszikus telepítési modellt."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 19dcebc7-da6b-4510-907b-d64088e81fa2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: iainfou
ms.openlocfilehash: cb6083366185c26c9f43ff94d835cd90725de5b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a><span data-ttu-id="1f139-103">Hogyan tooinstall és a Symantec Endpoint Protection konfigurálása a Windows virtuális gép</span><span class="sxs-lookup"><span data-stu-id="1f139-103">How tooinstall and configure Symantec Endpoint Protection on a Windows VM</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="1f139-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="1f139-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="1f139-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="1f139-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="1f139-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="1f139-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="1f139-107">Ez a cikk bemutatja, hogyan tooinstall és egy meglévő virtuális gépen (VM) Windows Server rendszert futtató hello Symantec Endpoint Protection-ügyfél konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="1f139-107">This article shows you how tooinstall and configure hello Symantec Endpoint Protection client on an existing virtual machine (VM) running Windows Server.</span></span> <span data-ttu-id="1f139-108">A teljes ügyfél szolgáltatásokat, például vírus- és kémprogram-védelmi-, tűzfal, és behatolás megelőzési tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="1f139-108">This full client includes services such as virus and spyware protection, firewall, and intrusion prevention.</span></span> <span data-ttu-id="1f139-109">hello ügyfél biztonsági bővítményként segítségével hello Virtuálisgép-ügynök telepítve van.</span><span class="sxs-lookup"><span data-stu-id="1f139-109">hello client is installed as a security extension by using hello VM Agent.</span></span>

<span data-ttu-id="1f139-110">Ha egy helyszíni megoldás a Symantec egy meglévő előfizetéshez, használhatja tooprotect az Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="1f139-110">If you have an existing subscription from Symantec for an on-premises solution, you can use it tooprotect your Azure virtual machines.</span></span> <span data-ttu-id="1f139-111">Ha nem az ügyfél még, regisztrálhat egy próba-előfizetésre.</span><span class="sxs-lookup"><span data-stu-id="1f139-111">If you're not a customer yet, you can sign up for a trial subscription.</span></span> <span data-ttu-id="1f139-112">Információ a megoldásról további információkért lásd: [Symantec Endpoint Protection a Microsoft Azure platformon][Symantec].</span><span class="sxs-lookup"><span data-stu-id="1f139-112">For more information about this solution, see [Symantec Endpoint Protection on Microsoft's Azure platform][Symantec].</span></span> <span data-ttu-id="1f139-113">Ennek a lapnak a hivatkozások toolicensing információi és útmutatója hello ügyfél telepítése, ha Ön már egy Symantec-ügyfél is van.</span><span class="sxs-lookup"><span data-stu-id="1f139-113">This page also has links toolicensing information and instructions for installing hello client if you're already a Symantec customer.</span></span>

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a><span data-ttu-id="1f139-114">Symantec Endpoint Protection telepítése egy meglévő virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="1f139-114">Install Symantec Endpoint Protection on an existing VM</span></span>
<span data-ttu-id="1f139-115">Mielőtt elkezdené, hello következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="1f139-115">Before you begin, you need hello following:</span></span>

* <span data-ttu-id="1f139-116">hello Azure PowerShell modul, a 0.8.2 verzió vagy újabb verzióját, a munkahelyi számítógép.</span><span class="sxs-lookup"><span data-stu-id="1f139-116">hello Azure PowerShell module, version 0.8.2 or later, on your work computer.</span></span> <span data-ttu-id="1f139-117">Ellenőrizheti a hello Azure PowerShell hello telepített verziójának **Get-Module azure |} táblázat formázása verzió** parancsot.</span><span class="sxs-lookup"><span data-stu-id="1f139-117">You can check hello version of Azure PowerShell that you have installed with hello **Get-Module azure | format-table version** command.</span></span> <span data-ttu-id="1f139-118">Útmutatás és egy hivatkozásra toohello legújabb verzió: [hogyan tooInstall és konfigurálása az Azure PowerShell][PS].</span><span class="sxs-lookup"><span data-stu-id="1f139-118">For instructions and a link toohello latest version, see [How tooInstall and Configure Azure PowerShell][PS].</span></span> <span data-ttu-id="1f139-119">Jelentkezzen be tooyour Azure-előfizetés `Add-AzureAccount`.</span><span class="sxs-lookup"><span data-stu-id="1f139-119">Log in tooyour Azure subscription using `Add-AzureAccount`.</span></span>
* <span data-ttu-id="1f139-120">hello futó Virtuálisgép-ügynök hello Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="1f139-120">hello VM Agent running on hello Azure Virtual Machine.</span></span>

<span data-ttu-id="1f139-121">Először győződjön meg arról, hogy hello Virtuálisgép-ügynök már telepítve van a hello virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="1f139-121">First, verify that hello VM Agent is already installed on hello virtual machine.</span></span> <span data-ttu-id="1f139-122">Adja meg hello felhőszolgáltatás neve és a virtuális gép nevét, és futtassa a parancsokat egy rendszergazda szintű Azure PowerShell parancssorba a következő hello.</span><span class="sxs-lookup"><span data-stu-id="1f139-122">Fill in hello cloud service name and virtual machine name, and then run hello following commands at an administrator-level Azure PowerShell command prompt.</span></span> <span data-ttu-id="1f139-123">Cserélje le mindent, ami hello idézőjelek között, beleértve a hello < és > karakter.</span><span class="sxs-lookup"><span data-stu-id="1f139-123">Replace everything within hello quotes, including hello < and > characters.</span></span>

> [!TIP]
> <span data-ttu-id="1f139-124">Ha nem tudja hello felhőszolgáltatás és a virtuális gép nevét, futtassa **Get-AzureVM** toolist hello nevek az összes virtuális gép az aktuális előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="1f139-124">If you don't know hello cloud service and virtual machine names, run **Get-AzureVM** toolist hello names for all virtual machines in your current subscription.</span></span>

```powershell
$CSName = "<cloud service name>"
$VMName = "<virtual machine name>"
$vm = Get-AzureVM -ServiceName $CSName -Name $VMName
write-host $vm.VM.ProvisionGuestAgent
```

<span data-ttu-id="1f139-125">Ha hello **write-host** parancs megjeleníti **igaz**, hello Virtuálisgép-ügynök telepítve van.</span><span class="sxs-lookup"><span data-stu-id="1f139-125">If hello **write-host** command displays **True**, hello VM Agent is installed.</span></span> <span data-ttu-id="1f139-126">Ha megjeleníti **hamis**, hello utasításokat, és egy hivatkozásra toohello, töltse le a hello Azure blogbejegyzés [ügynök és Virtuálisgép-bővítmények – 2. rész][Agent].</span><span class="sxs-lookup"><span data-stu-id="1f139-126">If it displays **False**, see hello instructions and a link toohello download in hello Azure blog post [VM Agent and Extensions - Part 2][Agent].</span></span>

<span data-ttu-id="1f139-127">Ha hello Virtuálisgép-ügynök telepítve van, ezek parancsok tooinstall hello Symantec Endpoint Protection-ügynök fut.</span><span class="sxs-lookup"><span data-stu-id="1f139-127">If hello VM Agent is installed, run these commands tooinstall hello Symantec Endpoint Protection agent.</span></span>

```powershell
$Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection \
    -VM $vm | Update-AzureVM
```

<span data-ttu-id="1f139-128">tooverify, amelyek Symantec biztonsági bővítmény hello telepítve van és naprakész:</span><span class="sxs-lookup"><span data-stu-id="1f139-128">tooverify that hello Symantec security extension has been installed and is up-to-date:</span></span>

1. <span data-ttu-id="1f139-129">Jelentkezzen be a virtuális gép toohello.</span><span class="sxs-lookup"><span data-stu-id="1f139-129">Log on toohello virtual machine.</span></span> <span data-ttu-id="1f139-130">Útmutatásért lásd: [hogyan tooLog a virtuális gép futó Windows Server tooa][Logon].</span><span class="sxs-lookup"><span data-stu-id="1f139-130">For instructions, see [How tooLog on tooa Virtual Machine Running Windows Server][Logon].</span></span>
2. <span data-ttu-id="1f139-131">A Windows Server 2008 R2, kattintson a **Start > Symantec Endpoint Protection**.</span><span class="sxs-lookup"><span data-stu-id="1f139-131">For Windows Server 2008 R2, click **Start > Symantec Endpoint Protection**.</span></span> <span data-ttu-id="1f139-132">Írja be a Windows Server 2012 vagy Windows Server 2012 R2-ben hello kezdőképernyőről **Symantec**, és kattintson a **Symantec Endpoint Protection**.</span><span class="sxs-lookup"><span data-stu-id="1f139-132">For Windows Server 2012 or Windows Server 2012 R2, from hello start screen, type **Symantec**, and then click **Symantec Endpoint Protection**.</span></span>
3. <span data-ttu-id="1f139-133">A hello **állapot** hello lapján **állapot-Symantec Endpoint Protection** ablak, alkalmazza a frissítéseket, vagy szükség esetén indítsa újra.</span><span class="sxs-lookup"><span data-stu-id="1f139-133">From hello **Status** tab of hello **Status-Symantec Endpoint Protection** window, apply updates or restart if needed.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1f139-134">További források</span><span class="sxs-lookup"><span data-stu-id="1f139-134">Additional resources</span></span>
<span data-ttu-id="1f139-135">[Hogyan tooLog a virtuális gép futó Windows Server tooa][Logon]</span><span class="sxs-lookup"><span data-stu-id="1f139-135">[How tooLog on tooa Virtual Machine Running Windows Server][Logon]</span></span>

<span data-ttu-id="1f139-136">[Az Azure Virtuálisgép-bővítmények és szolgáltatások][Ext]</span><span class="sxs-lookup"><span data-stu-id="1f139-136">[Azure VM Extensions and Features][Ext]</span></span>

<!--Link references-->
[Symantec]: http://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Create]:tutorial.md

[PS]: /powershell/azureps-cmdlets-docs

[Agent]: http://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]:connect-logon.md

[Ext]: http://go.microsoft.com/fwlink/p/?linkid=390493
