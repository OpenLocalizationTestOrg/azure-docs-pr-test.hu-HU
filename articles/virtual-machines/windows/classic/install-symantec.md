---
title: "Symantec Endpoint Protection telepítése a Windows Azure-ban |} Microsoft Docs"
description: "Megtudhatja, hogyan telepítse és konfigurálja a Symantec Endpoint Protection biztonsági bővítményt a klasszikus telepítési modellel létrehozott egy új vagy meglévő Azure virtuális gépen."
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
ms.openlocfilehash: 1603ebc7ee3c29277f30fbb802bdd8205b92d648
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-install-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a><span data-ttu-id="a7d09-103">A Symantec Endpoint Protection telepítése és konfigurálása windowsos virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="a7d09-103">How to install and configure Symantec Endpoint Protection on a Windows VM</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="a7d09-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="a7d09-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="a7d09-105">Ez a cikk a klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="a7d09-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="a7d09-106">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="a7d09-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>

<span data-ttu-id="a7d09-107">Ez a cikk bemutatja, hogyan telepítse és konfigurálja a Symantec Endpoint Protection-ügyfél egy meglévő virtuális gép (VM) Windows Server rendszerű.</span><span class="sxs-lookup"><span data-stu-id="a7d09-107">This article shows you how to install and configure the Symantec Endpoint Protection client on an existing virtual machine (VM) running Windows Server.</span></span> <span data-ttu-id="a7d09-108">A teljes ügyfél szolgáltatásokat, például vírus- és kémprogram-védelmi-, tűzfal, és behatolás megelőzési tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a7d09-108">This full client includes services such as virus and spyware protection, firewall, and intrusion prevention.</span></span> <span data-ttu-id="a7d09-109">Az ügyfél biztonsági bővítményként segítségével a Virtuálisgép-ügynök telepítve van.</span><span class="sxs-lookup"><span data-stu-id="a7d09-109">The client is installed as a security extension by using the VM Agent.</span></span>

<span data-ttu-id="a7d09-110">Ha egy helyszíni megoldás a Symantec egy meglévő előfizetéshez, használhatja az Azure virtuális gépek védelmét.</span><span class="sxs-lookup"><span data-stu-id="a7d09-110">If you have an existing subscription from Symantec for an on-premises solution, you can use it to protect your Azure virtual machines.</span></span> <span data-ttu-id="a7d09-111">Ha nem az ügyfél még, regisztrálhat egy próba-előfizetésre.</span><span class="sxs-lookup"><span data-stu-id="a7d09-111">If you're not a customer yet, you can sign up for a trial subscription.</span></span> <span data-ttu-id="a7d09-112">Információ a megoldásról további információkért lásd: [Symantec Endpoint Protection a Microsoft Azure platformon][Symantec].</span><span class="sxs-lookup"><span data-stu-id="a7d09-112">For more information about this solution, see [Symantec Endpoint Protection on Microsoft's Azure platform][Symantec].</span></span> <span data-ttu-id="a7d09-113">Ezen a lapon olyan licencelési információkat és útmutatást az ügyfél telepítéséhez, ha Ön már egy Symantec-ügyfél mutató hivatkozásokat is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="a7d09-113">This page also has links to licensing information and instructions for installing the client if you're already a Symantec customer.</span></span>

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a><span data-ttu-id="a7d09-114">Symantec Endpoint Protection telepítése egy meglévő virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="a7d09-114">Install Symantec Endpoint Protection on an existing VM</span></span>
<span data-ttu-id="a7d09-115">Mielőtt elkezdené, a következők szükségesek:</span><span class="sxs-lookup"><span data-stu-id="a7d09-115">Before you begin, you need the following:</span></span>

* <span data-ttu-id="a7d09-116">Az Azure PowerShell modul, a 0.8.2 verzió vagy újabb verzióját, a munkahelyi számítógép.</span><span class="sxs-lookup"><span data-stu-id="a7d09-116">The Azure PowerShell module, version 0.8.2 or later, on your work computer.</span></span> <span data-ttu-id="a7d09-117">Ellenőrizheti, hogy az Azure PowerShell, a telepített verzióját az **Get-Module azure |} táblázat formázása verzió** parancsot.</span><span class="sxs-lookup"><span data-stu-id="a7d09-117">You can check the version of Azure PowerShell that you have installed with the **Get-Module azure | format-table version** command.</span></span> <span data-ttu-id="a7d09-118">Útmutatás és egy hivatkozást a legújabb verzióra: [telepítése és konfigurálása az Azure PowerShell][PS].</span><span class="sxs-lookup"><span data-stu-id="a7d09-118">For instructions and a link to the latest version, see [How to Install and Configure Azure PowerShell][PS].</span></span> <span data-ttu-id="a7d09-119">Jelentkezzen be az Azure-előfizetés használata `Add-AzureAccount`.</span><span class="sxs-lookup"><span data-stu-id="a7d09-119">Log in to your Azure subscription using `Add-AzureAccount`.</span></span>
* <span data-ttu-id="a7d09-120">A Virtuálisgép-ügynök az Azure virtuális gépet futtat.</span><span class="sxs-lookup"><span data-stu-id="a7d09-120">The VM Agent running on the Azure Virtual Machine.</span></span>

<span data-ttu-id="a7d09-121">Először is győződjön meg arról, hogy a Virtuálisgép-ügynök már telepítve van a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="a7d09-121">First, verify that the VM Agent is already installed on the virtual machine.</span></span> <span data-ttu-id="a7d09-122">Adja meg a felhőszolgáltatás neve és a virtuális gép nevét, és futtassa a következő parancsokat egy rendszergazda szintű Azure PowerShell parancssorban.</span><span class="sxs-lookup"><span data-stu-id="a7d09-122">Fill in the cloud service name and virtual machine name, and then run the following commands at an administrator-level Azure PowerShell command prompt.</span></span> <span data-ttu-id="a7d09-123">Cserélje le a mindent, ami az ajánlatokat, beleértve a < és > karakter.</span><span class="sxs-lookup"><span data-stu-id="a7d09-123">Replace everything within the quotes, including the < and > characters.</span></span>

> [!TIP]
> <span data-ttu-id="a7d09-124">Ha nem ismeri a felhőszolgáltatás és a virtuális gép nevét, futtassa **Get-AzureVM** listát az összes virtuális gépet a jelenlegi előfizetés nevét.</span><span class="sxs-lookup"><span data-stu-id="a7d09-124">If you don't know the cloud service and virtual machine names, run **Get-AzureVM** to list the names for all virtual machines in your current subscription.</span></span>

```powershell
$CSName = "<cloud service name>"
$VMName = "<virtual machine name>"
$vm = Get-AzureVM -ServiceName $CSName -Name $VMName
write-host $vm.VM.ProvisionGuestAgent
```

<span data-ttu-id="a7d09-125">Ha a **write-host** parancs megjeleníti **igaz**, a Virtuálisgép-ügynök telepítve van.</span><span class="sxs-lookup"><span data-stu-id="a7d09-125">If the **write-host** command displays **True**, the VM Agent is installed.</span></span> <span data-ttu-id="a7d09-126">Ha megjeleníti **hamis**, tekintse meg az utasításokat és az Azure blogbejegyzésben letöltési mutató hivatkozást [ügynök és Virtuálisgép-bővítmények – 2. rész][Agent].</span><span class="sxs-lookup"><span data-stu-id="a7d09-126">If it displays **False**, see the instructions and a link to the download in the Azure blog post [VM Agent and Extensions - Part 2][Agent].</span></span>

<span data-ttu-id="a7d09-127">Ha a Virtuálisgép-ügynök telepítve van, futtassa az alábbi parancsokat a Symantec Endpoint Protection-ügynök telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a7d09-127">If the VM Agent is installed, run these commands to install the Symantec Endpoint Protection agent.</span></span>

```powershell
$Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection \
    -VM $vm | Update-AzureVM
```

<span data-ttu-id="a7d09-128">Ellenőrizheti, hogy a Symantec biztonsági bővítménye telepítve van-e, és naprakész:</span><span class="sxs-lookup"><span data-stu-id="a7d09-128">To verify that the Symantec security extension has been installed and is up-to-date:</span></span>

1. <span data-ttu-id="a7d09-129">Jelentkezzen be a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="a7d09-129">Log on to the virtual machine.</span></span> <span data-ttu-id="a7d09-130">Útmutatásért lásd: [jelentkezzen be a virtuális gép futó Windows Server hogyan][Logon].</span><span class="sxs-lookup"><span data-stu-id="a7d09-130">For instructions, see [How to Log on to a Virtual Machine Running Windows Server][Logon].</span></span>
2. <span data-ttu-id="a7d09-131">A Windows Server 2008 R2, kattintson a **Start > Symantec Endpoint Protection**.</span><span class="sxs-lookup"><span data-stu-id="a7d09-131">For Windows Server 2008 R2, click **Start > Symantec Endpoint Protection**.</span></span> <span data-ttu-id="a7d09-132">Írja be a Windows Server 2012 vagy Windows Server 2012 R2-ben a kezdőképernyőről **Symantec**, és kattintson a **Symantec Endpoint Protection**.</span><span class="sxs-lookup"><span data-stu-id="a7d09-132">For Windows Server 2012 or Windows Server 2012 R2, from the start screen, type **Symantec**, and then click **Symantec Endpoint Protection**.</span></span>
3. <span data-ttu-id="a7d09-133">Az a **állapot** lapján a **állapot-Symantec Endpoint Protection** ablak, alkalmazza a frissítéseket, vagy szükség esetén indítsa újra.</span><span class="sxs-lookup"><span data-stu-id="a7d09-133">From the **Status** tab of the **Status-Symantec Endpoint Protection** window, apply updates or restart if needed.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a7d09-134">További források</span><span class="sxs-lookup"><span data-stu-id="a7d09-134">Additional resources</span></span>
<span data-ttu-id="a7d09-135">[Jelentkezzen be a Windows Server rendszerű virtuális gép hogyan][Logon]</span><span class="sxs-lookup"><span data-stu-id="a7d09-135">[How to Log on to a Virtual Machine Running Windows Server][Logon]</span></span>

<span data-ttu-id="a7d09-136">[Az Azure Virtuálisgép-bővítmények és szolgáltatások][Ext]</span><span class="sxs-lookup"><span data-stu-id="a7d09-136">[Azure VM Extensions and Features][Ext]</span></span>

<!--Link references-->
[Symantec]: http://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Create]:tutorial.md

[PS]: /powershell/azureps-cmdlets-docs

[Agent]: http://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]:connect-logon.md

[Ext]: http://go.microsoft.com/fwlink/p/?linkid=390493
