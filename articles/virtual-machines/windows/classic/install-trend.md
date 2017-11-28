---
title: "Trend Micro mély biztonsági a virtuális gép aaaInstall |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooinstall és a Trend Micro biztonságának konfigurálása a hello klasszikus telepítési modellt az Azure-ban létrehozott virtuális gép."
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
ms.openlocfilehash: dc5492db07a37a2296df5da673183a14c6d5b1f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a><span data-ttu-id="9c5d9-103">Hogyan tooinstall és a Trend Micro mély biztonságának konfigurálása a Windows virtuális gép szolgáltatásként</span><span class="sxs-lookup"><span data-stu-id="9c5d9-103">How tooinstall and configure Trend Micro Deep Security as a Service on a Windows VM</span></span>
> [!IMPORTANT]
> <span data-ttu-id="9c5d9-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="9c5d9-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="9c5d9-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="9c5d9-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="9c5d9-107">Ez a cikk bemutatja, hogyan tooinstall és a Trend Micro mély biztonságának konfigurálása egy új vagy meglévő virtuális gépen (VM) Windows Server rendszert futtató szolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-107">This article shows you how tooinstall and configure Trend Micro Deep Security as a Service on a new or existing virtual machine (VM) running Windows Server.</span></span> <span data-ttu-id="9c5d9-108">Szolgáltatásként mély biztonsági magában foglalja a kártevők elleni védelem, a tűzfal, egy behatolás elleni védelmi rendszerének és integritásának ellenőrzésére.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-108">Deep Security as a Service includes anti-malware protection, a firewall, an intrusion prevention system, and integrity monitoring.</span></span>

<span data-ttu-id="9c5d9-109">hello ügyfél biztonsági bővítményként keresztül hello Virtuálisgép-ügynök telepítve van.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-109">hello client is installed as a security extension via hello VM Agent.</span></span> <span data-ttu-id="9c5d9-110">Új virtuális gépen a hello mély biztonsági ügynök hello Virtuálisgép-ügynök hello Azure-portál által automatikusan létrehozott, telepítenie.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-110">On a new virtual machine, you install hello Deep Security Agent, as hello VM Agent is created automatically by hello Azure portal.</span></span>

<span data-ttu-id="9c5d9-111">Hello klasszikus portálra, a hello Azure CLI vagy a PowerShell segítségével létrehozni meglévő virtuális lehet, hogy nincs egy Virtuálisgép-ügynök.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-111">An existing VM created using hello classic portal, hello Azure CLI, or PowerShell might not have a VM agent.</span></span> <span data-ttu-id="9c5d9-112">Egy meglévő virtuális gép nem található a Virtuálisgép-ügynök hello esetén toodownload kell, és azt telepítse először.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-112">For an existing virtual machine that doesn't have hello VM Agent, you need toodownload and install it first.</span></span> <span data-ttu-id="9c5d9-113">Ez a cikk mindkét eseteire vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-113">This article covers both situations.</span></span>

<span data-ttu-id="9c5d9-114">Ha egy helyszíni megoldás a Trend Micro aktuális előfizetést, használhatja toohelp védeni az Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-114">If you have a current subscription from Trend Micro for an on-premises solution, you can use it toohelp protect your Azure virtual machines.</span></span> <span data-ttu-id="9c5d9-115">Ha nem az ügyfél még, regisztrálhat egy próba-előfizetésre.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-115">If you're not a customer yet, you can sign up for a trial subscription.</span></span> <span data-ttu-id="9c5d9-116">Információ a megoldásról további információkért lásd: hello Trend Micro blogbejegyzésben [Microsoft Azure virtuális gép ügynök bővítmény a mély biztonsági](http://go.microsoft.com/fwlink/p/?LinkId=403945).</span><span class="sxs-lookup"><span data-stu-id="9c5d9-116">For more information about this solution, see hello Trend Micro blog post [Microsoft Azure VM Agent Extension For Deep Security](http://go.microsoft.com/fwlink/p/?LinkId=403945).</span></span>

## <a name="install-hello-deep-security-agent-on-a-new-vm"></a><span data-ttu-id="9c5d9-117">Hello mély biztonsági ügynök telepítése egy új virtuális Gépre</span><span class="sxs-lookup"><span data-stu-id="9c5d9-117">Install hello Deep Security Agent on a new VM</span></span>

<span data-ttu-id="9c5d9-118">Hello [Azure-portálon](http://portal.azure.com) lehetővé teszi, hogy hello Trend Micro biztonsági bővítményének telepítése az hello lemezképének használatakor **piactér** toocreate hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-118">hello [Azure portal](http://portal.azure.com) lets you install hello Trend Micro security extension when you use an image from hello **Marketplace** toocreate hello virtual machine.</span></span> <span data-ttu-id="9c5d9-119">Ha egyetlen virtuális gép hoz létre, egy egyszerűen tooadd védelmet a Trend Micro hello portál használatával.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-119">If you're creating a single virtual machine, using hello portal is an easy way tooadd protection from Trend Micro.</span></span>

<span data-ttu-id="9c5d9-120">A hello egyik bejegyzésének használatával **piactér** megnyit egy varázslót, amely segít állítson be hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-120">Using an entry from hello **Marketplace** opens a wizard that helps you set up hello virtual machine.</span></span> <span data-ttu-id="9c5d9-121">Hello használata **beállítások** panelen, a harmadik panel hello hello varázsló tooinstall hello Trend Micro biztonsági bővítmény.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-121">You use hello **Settings** blade, hello third panel of hello wizard, tooinstall hello Trend Micro security extension.</span></span>  <span data-ttu-id="9c5d9-122">Általános útmutatás: [hello Azure-portálon a Windows operációs rendszert futtató virtuális gép létrehozása](tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="9c5d9-122">For general instructions, see [Create a virtual machine running Windows in hello Azure portal](tutorial.md).</span></span>

<span data-ttu-id="9c5d9-123">Amikor elérhetővé toohello **beállítások** hello varázsló panel hello lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="9c5d9-123">When you get toohello **Settings** blade of hello wizard, do hello following steps:</span></span>

1. <span data-ttu-id="9c5d9-124">Kattintson a **bővítmények**, majd kattintson a **felvenni a bővítményt** hello következő ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-124">Click **Extensions**, then click **Add extension** in hello next pane.</span></span>

   ![Hello bővítmény felvételének megkezdése][1]

2. <span data-ttu-id="9c5d9-126">Válassza ki **mély biztonsági ügynök** a hello **új erőforrás** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-126">Select **Deep Security Agent** in hello **New resource** pane.</span></span> <span data-ttu-id="9c5d9-127">Hello mély biztonsági ügynök ablaktábláján kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-127">In hello Deep Security Agent pane, click **Create**.</span></span>

   ![A részletes biztonsági megbízott azonosítása][2]

3. <span data-ttu-id="9c5d9-129">Adja meg a hello **bérlői azonosító** és **bérlői aktiválási jelszó** hello bővítmény.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-129">Enter hello **Tenant Identifier** and **Tenant Activation Password** for hello extension.</span></span> <span data-ttu-id="9c5d9-130">Másik lehetőségként megadhat egy **biztonsági házirend-azonosító**.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-130">Optionally, you can enter a **Security Policy Identifier**.</span></span> <span data-ttu-id="9c5d9-131">Kattintson a **OK** tooadd hello ügyfél.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-131">Then, click **OK** tooadd hello client.</span></span>

   ![Adja meg a bővítmény részletei][3]

## <a name="install-hello-deep-security-agent-on-an-existing-vm"></a><span data-ttu-id="9c5d9-133">A meglévő virtuális hello mély biztonsági ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="9c5d9-133">Install hello Deep Security Agent on an existing VM</span></span>
<span data-ttu-id="9c5d9-134">tooinstall hello ügynök egy meglévő virtuális gépen, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="9c5d9-134">tooinstall hello agent on an existing VM, you need hello following items:</span></span>

* <span data-ttu-id="9c5d9-135">hello Azure PowerShell modul, a 0.8.2 verzió vagy újabb, a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-135">hello Azure PowerShell module, version 0.8.2 or newer, installed on your local computer.</span></span> <span data-ttu-id="9c5d9-136">Ellenőrizheti a hello Azure PowerShell hello segítségével telepített verziójának **Get-Module azure |} táblázat formázása verzió** parancsot.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-136">You can check hello version of Azure PowerShell that you have installed by using hello **Get-Module azure | format-table version** command.</span></span> <span data-ttu-id="9c5d9-137">Útmutatás és egy hivatkozásra toohello legújabb verzió: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9c5d9-137">For instructions and a link toohello latest version, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="9c5d9-138">Jelentkezzen be tooyour Azure-előfizetés `Add-AzureAccount`.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-138">Log in tooyour Azure subscription using `Add-AzureAccount`.</span></span>
* <span data-ttu-id="9c5d9-139">hello hello cél virtuális gépen telepített Virtuálisgép-ügynök.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-139">hello VM Agent installed on hello target virtual machine.</span></span>

<span data-ttu-id="9c5d9-140">Először is győződjön meg arról, hogy hello Virtuálisgép-ügynök már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-140">First, verify that hello VM Agent is already installed.</span></span> <span data-ttu-id="9c5d9-141">Adja meg hello felhőszolgáltatás neve és a virtuális gép nevét, és futtassa a parancsokat egy rendszergazda szintű Azure PowerShell parancssorba a következő hello.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-141">Fill in hello cloud service name and virtual machine name, and then run hello following commands at an administrator-level Azure PowerShell command prompt.</span></span> <span data-ttu-id="9c5d9-142">Cserélje le mindent, ami hello idézőjelek között, beleértve a hello < és > karakter.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-142">Replace everything within hello quotes, including hello < and > characters.</span></span>

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

<span data-ttu-id="9c5d9-143">Ha nem tudja hello felhőszolgáltatás és a virtuális gép nevét, futtassa **Get-AzureVM** toodisplay, hogy az összes információt hello virtuális gépek az aktuális előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-143">If you don't know hello cloud service and virtual machine name, run **Get-AzureVM** toodisplay that information for all hello virtual machines in your current subscription.</span></span>

<span data-ttu-id="9c5d9-144">Ha hello **write-host** parancs beolvasása **igaz**, hello Virtuálisgép-ügynök telepítve van.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-144">If hello **write-host** command returns **True**, hello VM Agent is installed.</span></span> <span data-ttu-id="9c5d9-145">Ha a visszaadott érték **hamis**, hello utasításokat, és egy hivatkozásra toohello, töltse le a hello Azure blogbejegyzés [ügynök és Virtuálisgép-bővítmények – 2. rész](http://go.microsoft.com/fwlink/p/?LinkId=403947).</span><span class="sxs-lookup"><span data-stu-id="9c5d9-145">If it returns **False**, see hello instructions and a link toohello download in hello Azure blog post [VM Agent and Extensions - Part 2](http://go.microsoft.com/fwlink/p/?LinkId=403947).</span></span>

<span data-ttu-id="9c5d9-146">Ha hello Virtuálisgép-ügynök telepítve van, futtassa az alábbi parancsokat.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-146">If hello VM Agent is installed, run these commands.</span></span>

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a><span data-ttu-id="9c5d9-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9c5d9-147">Next steps</span></span>
<span data-ttu-id="9c5d9-148">Hello ügynök toostart fut, a telepítés néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-148">It takes a few minutes for hello agent toostart running when it is installed.</span></span> <span data-ttu-id="9c5d9-149">Ezt követően kell tooactivate mély biztonsági hello virtuális gépen, a fiók kezelhető egy átfogó biztonsági Manager.</span><span class="sxs-lookup"><span data-stu-id="9c5d9-149">After that, you need tooactivate Deep Security on hello virtual machine so it can be managed by a Deep Security Manager.</span></span> <span data-ttu-id="9c5d9-150">Tekintse meg a következő cikkeket a további utasításokat hello:</span><span class="sxs-lookup"><span data-stu-id="9c5d9-150">See hello following articles for additional instructions:</span></span>

* <span data-ttu-id="9c5d9-151">Információ a megoldásról a trend a cikk [Instant-On Cloud Security a Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)</span><span class="sxs-lookup"><span data-stu-id="9c5d9-151">Trend's article about this solution, [Instant-On Cloud Security for Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)</span></span>
* <span data-ttu-id="9c5d9-152">A [Windows PowerShell-mintaparancsfájl](http://go.microsoft.com/fwlink/?LinkId=404100) tooconfigure hello virtuális gép</span><span class="sxs-lookup"><span data-stu-id="9c5d9-152">A [sample Windows PowerShell script](http://go.microsoft.com/fwlink/?LinkId=404100) tooconfigure hello virtual machine</span></span>
* <span data-ttu-id="9c5d9-153">[Útmutatás](http://go.microsoft.com/fwlink/?LinkId=404099) hello minta</span><span class="sxs-lookup"><span data-stu-id="9c5d9-153">[Instructions](http://go.microsoft.com/fwlink/?LinkId=404099) for hello sample</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9c5d9-154">További források</span><span class="sxs-lookup"><span data-stu-id="9c5d9-154">Additional resources</span></span>
<span data-ttu-id="9c5d9-155">[Hogyan toolog Windows Server rendszert futtató tooa virtuális gépen]</span><span class="sxs-lookup"><span data-stu-id="9c5d9-155">[How toolog on tooa virtual machine running Windows Server]</span></span>

<span data-ttu-id="9c5d9-156">[Az Azure Virtuálisgép-bővítmények és szolgáltatások]</span><span class="sxs-lookup"><span data-stu-id="9c5d9-156">[Azure VM Extensions and features]</span></span>

<!-- Image references -->
[1]: ./media/install-trend/new_vm_Blade3.png
[2]: ./media/install-trend/find_SecurityAgent.png
[3]: ./media/install-trend/SecurityAgentDetails.png

<!-- Link references -->
[Hogyan toolog Windows Server rendszert futtató tooa virtuális gépen]:connect-logon.md
[Az Azure Virtuálisgép-bővítmények és szolgáltatások]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409
