---
title: "A virtuális gép telepítése a Trend Micro részletes biztonsági |} Microsoft Docs"
description: "A cikkből megtudhatja, hogyan kell telepíteni, és a Trend Micro biztonságának konfigurálása a klasszikus telepítési modellt az Azure-ban létrehozott egy virtuális gépen."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: e991b635-f1e2-483f-b7ca-9d53e7c22e2a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: iainfou
ms.openlocfilehash: 911b8f12472dcbda3e6bfeb8c97bf1d04a63e1dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-install-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a><span data-ttu-id="edb39-103">A Trend Micro Deep Security szolgáltatásként való telepítése és konfigurálása windowsos virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="edb39-103">How to install and configure Trend Micro Deep Security as a Service on a Windows VM</span></span>
> [!IMPORTANT]
> <span data-ttu-id="edb39-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="edb39-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="edb39-105">Ez a cikk a klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="edb39-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="edb39-106">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="edb39-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>

<span data-ttu-id="edb39-107">Ez a cikk bemutatja, hogyan telepítse és konfigurálja a Trend Micro mély biztonsági egy új vagy meglévő virtuális gépen (VM) Windows Server rendszert futtató szolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="edb39-107">This article shows you how to install and configure Trend Micro Deep Security as a Service on a new or existing virtual machine (VM) running Windows Server.</span></span> <span data-ttu-id="edb39-108">Szolgáltatásként mély biztonsági magában foglalja a kártevők elleni védelem, a tűzfal, egy behatolás elleni védelmi rendszerének és integritásának ellenőrzésére.</span><span class="sxs-lookup"><span data-stu-id="edb39-108">Deep Security as a Service includes anti-malware protection, a firewall, an intrusion prevention system, and integrity monitoring.</span></span>

<span data-ttu-id="edb39-109">Az ügyfél biztonsági bővítményként keresztül a Virtuálisgép-ügynök telepítve van.</span><span class="sxs-lookup"><span data-stu-id="edb39-109">The client is installed as a security extension via the VM Agent.</span></span> <span data-ttu-id="edb39-110">Új virtuális gépen, az mély biztonsági ügynök telepítése, mert a Virtuálisgép-ügynök automatikusan hozza létre az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="edb39-110">On a new virtual machine, you install the Deep Security Agent, as the VM Agent is created automatically by the Azure portal.</span></span>

<span data-ttu-id="edb39-111">A klasszikus portál, az Azure parancssori felület vagy a PowerShell használatával létrehozni meglévő virtuális lehet, hogy nincs egy Virtuálisgép-ügynök.</span><span class="sxs-lookup"><span data-stu-id="edb39-111">An existing VM created using the classic portal, the Azure CLI, or PowerShell might not have a VM agent.</span></span> <span data-ttu-id="edb39-112">Egy meglévő virtuális gépen, amelyen a Virtuálisgép-ügynök nem rendelkezik akkor kell letöltheti és telepítheti azt.</span><span class="sxs-lookup"><span data-stu-id="edb39-112">For an existing virtual machine that doesn't have the VM Agent, you need to download and install it first.</span></span> <span data-ttu-id="edb39-113">Ez a cikk mindkét eseteire vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="edb39-113">This article covers both situations.</span></span>

<span data-ttu-id="edb39-114">Ha egy helyszíni megoldás a Trend Micro aktuális előfizetést, használhatja az Azure virtuális gépek védelme érdekében.</span><span class="sxs-lookup"><span data-stu-id="edb39-114">If you have a current subscription from Trend Micro for an on-premises solution, you can use it to help protect your Azure virtual machines.</span></span> <span data-ttu-id="edb39-115">Ha nem az ügyfél még, regisztrálhat egy próba-előfizetésre.</span><span class="sxs-lookup"><span data-stu-id="edb39-115">If you're not a customer yet, you can sign up for a trial subscription.</span></span> <span data-ttu-id="edb39-116">Információ a megoldásról további információkért lásd a következő Trend Micro blogbejegyzésben [Microsoft Azure virtuális gép ügynök bővítmény a mély biztonsági](http://go.microsoft.com/fwlink/p/?LinkId=403945).</span><span class="sxs-lookup"><span data-stu-id="edb39-116">For more information about this solution, see the Trend Micro blog post [Microsoft Azure VM Agent Extension For Deep Security](http://go.microsoft.com/fwlink/p/?LinkId=403945).</span></span>

## <a name="install-the-deep-security-agent-on-a-new-vm"></a><span data-ttu-id="edb39-117">Az átfogó biztonsági ügynök telepítése egy új virtuális gép</span><span class="sxs-lookup"><span data-stu-id="edb39-117">Install the Deep Security Agent on a new VM</span></span>

<span data-ttu-id="edb39-118">A [Azure-portálon](http://portal.azure.com) lehetővé teszi, hogy a Trend Micro biztonsági bővítményének telepítése, a lemezkép használatakor az **piactér** a virtuális gép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="edb39-118">The [Azure portal](http://portal.azure.com) lets you install the Trend Micro security extension when you use an image from the **Marketplace** to create the virtual machine.</span></span> <span data-ttu-id="edb39-119">Ha egyetlen virtuális gép hoz létre, a portál használatával egyszerű módja védelem hozzáadása a Trend Micro.</span><span class="sxs-lookup"><span data-stu-id="edb39-119">If you're creating a single virtual machine, using the portal is an easy way to add protection from Trend Micro.</span></span>

<span data-ttu-id="edb39-120">Az egyik bejegyzésének használatával a **piactér** megnyit egy varázslót, amely segítséget nyújt a virtuális gép beállítása.</span><span class="sxs-lookup"><span data-stu-id="edb39-120">Using an entry from the **Marketplace** opens a wizard that helps you set up the virtual machine.</span></span> <span data-ttu-id="edb39-121">Használja a **beállítások** panelen, a harmadik panelen, a varázsló a Trend Micro biztonsági bővítmény telepítésére.</span><span class="sxs-lookup"><span data-stu-id="edb39-121">You use the **Settings** blade, the third panel of the wizard, to install the Trend Micro security extension.</span></span>  <span data-ttu-id="edb39-122">Általános útmutatás: [hozzon létre egy olyan virtuális géphez a Windows Azure-portálon](tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="edb39-122">For general instructions, see [Create a virtual machine running Windows in the Azure portal](tutorial.md).</span></span>

<span data-ttu-id="edb39-123">Amikor elér a **beállítások** varázsló panelen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="edb39-123">When you get to the **Settings** blade of the wizard, do the following steps:</span></span>

1. <span data-ttu-id="edb39-124">Kattintson a **bővítmények**, majd kattintson a **felvenni a bővítményt** a következő oldalon.</span><span class="sxs-lookup"><span data-stu-id="edb39-124">Click **Extensions**, then click **Add extension** in the next pane.</span></span>

   ![A bővítmény felvételének megkezdése][1]

2. <span data-ttu-id="edb39-126">Válassza ki **mély biztonsági ügynök** a a **új erőforrás** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="edb39-126">Select **Deep Security Agent** in the **New resource** pane.</span></span> <span data-ttu-id="edb39-127">A részletes biztonsági ügynök ablaktábláján kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="edb39-127">In the Deep Security Agent pane, click **Create**.</span></span>

   ![A részletes biztonsági megbízott azonosítása][2]

3. <span data-ttu-id="edb39-129">Adja meg a **bérlői azonosító** és **bérlői aktiválási jelszó** a bővítmény.</span><span class="sxs-lookup"><span data-stu-id="edb39-129">Enter the **Tenant Identifier** and **Tenant Activation Password** for the extension.</span></span> <span data-ttu-id="edb39-130">Másik lehetőségként megadhat egy **biztonsági házirend-azonosító**.</span><span class="sxs-lookup"><span data-stu-id="edb39-130">Optionally, you can enter a **Security Policy Identifier**.</span></span> <span data-ttu-id="edb39-131">Kattintson a **OK** ügyfél.</span><span class="sxs-lookup"><span data-stu-id="edb39-131">Then, click **OK** to add the client.</span></span>

   ![Adja meg a bővítmény részletei][3]

## <a name="install-the-deep-security-agent-on-an-existing-vm"></a><span data-ttu-id="edb39-133">A részletes biztonsági ügynök telepíthető egy meglévő virtuális Gépen</span><span class="sxs-lookup"><span data-stu-id="edb39-133">Install the Deep Security Agent on an existing VM</span></span>
<span data-ttu-id="edb39-134">Az ügynök telepítése egy meglévő virtuális Gépre, az alábbi elemek szükségesek:</span><span class="sxs-lookup"><span data-stu-id="edb39-134">To install the agent on an existing VM, you need the following items:</span></span>

* <span data-ttu-id="edb39-135">Az Azure PowerShell modul, a 0.8.2 verzió vagy újabb, a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="edb39-135">The Azure PowerShell module, version 0.8.2 or newer, installed on your local computer.</span></span> <span data-ttu-id="edb39-136">Ellenőrizheti, hogy az Azure PowerShell használatával telepített verzióját az **Get-Module azure |} táblázat formázása verzió** parancsot.</span><span class="sxs-lookup"><span data-stu-id="edb39-136">You can check the version of Azure PowerShell that you have installed by using the **Get-Module azure | format-table version** command.</span></span> <span data-ttu-id="edb39-137">Útmutatás és egy hivatkozást a legújabb verzióra: [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="edb39-137">For instructions and a link to the latest version, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="edb39-138">Jelentkezzen be az Azure-előfizetés használata `Add-AzureAccount`.</span><span class="sxs-lookup"><span data-stu-id="edb39-138">Log in to your Azure subscription using `Add-AzureAccount`.</span></span>
* <span data-ttu-id="edb39-139">A Virtuálisgép-ügynök telepítve legyen a cél virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="edb39-139">The VM Agent installed on the target virtual machine.</span></span>

<span data-ttu-id="edb39-140">Először is győződjön meg arról, hogy a Virtuálisgép-ügynök telepítve van.</span><span class="sxs-lookup"><span data-stu-id="edb39-140">First, verify that the VM Agent is already installed.</span></span> <span data-ttu-id="edb39-141">Adja meg a felhőszolgáltatás neve és a virtuális gép nevét, és futtassa a következő parancsokat egy rendszergazda szintű Azure PowerShell parancssorban.</span><span class="sxs-lookup"><span data-stu-id="edb39-141">Fill in the cloud service name and virtual machine name, and then run the following commands at an administrator-level Azure PowerShell command prompt.</span></span> <span data-ttu-id="edb39-142">Cserélje le a mindent, ami az ajánlatokat, beleértve a < és > karakter.</span><span class="sxs-lookup"><span data-stu-id="edb39-142">Replace everything within the quotes, including the < and > characters.</span></span>

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

<span data-ttu-id="edb39-143">Ha nem ismeri a felhőszolgáltatás és a virtuális gép nevét, futtassa **Get-AzureVM** megjelenítheti, hogy az összes virtuális gép az aktuális előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="edb39-143">If you don't know the cloud service and virtual machine name, run **Get-AzureVM** to display that information for all the virtual machines in your current subscription.</span></span>

<span data-ttu-id="edb39-144">Ha a **write-host** parancs beolvasása **igaz**, a Virtuálisgép-ügynök telepítve van.</span><span class="sxs-lookup"><span data-stu-id="edb39-144">If the **write-host** command returns **True**, the VM Agent is installed.</span></span> <span data-ttu-id="edb39-145">Ha a visszaadott érték **hamis**, tekintse meg az utasításokat és az Azure blogbejegyzésben letöltési mutató hivatkozást [ügynök és Virtuálisgép-bővítmények – 2. rész](http://go.microsoft.com/fwlink/p/?LinkId=403947).</span><span class="sxs-lookup"><span data-stu-id="edb39-145">If it returns **False**, see the instructions and a link to the download in the Azure blog post [VM Agent and Extensions - Part 2](http://go.microsoft.com/fwlink/p/?LinkId=403947).</span></span>

<span data-ttu-id="edb39-146">Ha a Virtuálisgép-ügynök telepítve van, futtassa az alábbi parancsokat.</span><span class="sxs-lookup"><span data-stu-id="edb39-146">If the VM Agent is installed, run these commands.</span></span>

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a><span data-ttu-id="edb39-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="edb39-147">Next steps</span></span>
<span data-ttu-id="edb39-148">Az ügynök arra, hogy elindítsa a telepítés néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="edb39-148">It takes a few minutes for the agent to start running when it is installed.</span></span> <span data-ttu-id="edb39-149">Ezt követően részletes biztonsági aktiválása a virtuális gépen, így kezelhető egy átfogó biztonsági Manager kell.</span><span class="sxs-lookup"><span data-stu-id="edb39-149">After that, you need to activate Deep Security on the virtual machine so it can be managed by a Deep Security Manager.</span></span> <span data-ttu-id="edb39-150">Tekintse meg a további utasításokat a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="edb39-150">See the following articles for additional instructions:</span></span>

* <span data-ttu-id="edb39-151">Információ a megoldásról a trend a cikk [Instant-On Cloud Security a Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)</span><span class="sxs-lookup"><span data-stu-id="edb39-151">Trend's article about this solution, [Instant-On Cloud Security for Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)</span></span>
* <span data-ttu-id="edb39-152">A [Windows PowerShell-mintaparancsfájl](http://go.microsoft.com/fwlink/?LinkId=404100) a virtuális gép konfigurálásához</span><span class="sxs-lookup"><span data-stu-id="edb39-152">A [sample Windows PowerShell script](http://go.microsoft.com/fwlink/?LinkId=404100) to configure the virtual machine</span></span>
* <span data-ttu-id="edb39-153">[Útmutatás](http://go.microsoft.com/fwlink/?LinkId=404099) a minta</span><span class="sxs-lookup"><span data-stu-id="edb39-153">[Instructions](http://go.microsoft.com/fwlink/?LinkId=404099) for the sample</span></span>

## <a name="additional-resources"></a><span data-ttu-id="edb39-154">További források</span><span class="sxs-lookup"><span data-stu-id="edb39-154">Additional resources</span></span>
<span data-ttu-id="edb39-155">[Bejelentkezés a Windows Server rendszerű virtuális géphez]</span><span class="sxs-lookup"><span data-stu-id="edb39-155">[How to log on to a virtual machine running Windows Server]</span></span>

<span data-ttu-id="edb39-156">[Az Azure Virtuálisgép-bővítmények és szolgáltatások]</span><span class="sxs-lookup"><span data-stu-id="edb39-156">[Azure VM Extensions and features]</span></span>

<!-- Image references -->
[1]: ./media/install-trend/new_vm_Blade3.png
[2]: ./media/install-trend/find_SecurityAgent.png
[3]: ./media/install-trend/SecurityAgentDetails.png

<!-- Link references -->
<span data-ttu-id="edb39-157">[Bejelentkezés a Windows Server rendszerű virtuális géphez]:connect-logon.md</span><span class="sxs-lookup"><span data-stu-id="edb39-157">[How to log on to a virtual machine running Windows Server]:connect-logon.md</span></span>
<span data-ttu-id="edb39-158">[Az Azure Virtuálisgép-bővítmények és szolgáltatások]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="edb39-158">[Azure VM Extensions and features]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409</span></span>
