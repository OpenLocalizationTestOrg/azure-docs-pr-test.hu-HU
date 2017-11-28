---
title: "aaaManage Azure-végpont hozzáférés szabályozásához listák |} PowerShell |} Klasszikus |} Microsoft Docs"
description: "Megtudhatja, hogyan toomanage hozzáférés-vezérlési listákat a PowerShell használatával"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: c84e40af-f351-4572-b3f0-d572d46bafe7
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: a7ca241ea108a266085bfb689b742d781e58da1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-endpoint-access-control-lists-using-powershell-in-hello-classic-deployment-model"></a><span data-ttu-id="6e71d-103">Kezelése a PowerShell használatával hello klasszikus üzembe helyezési modellel végpont hozzáférés-vezérlési listák</span><span class="sxs-lookup"><span data-stu-id="6e71d-103">Manage endpoint access control lists using PowerShell in hello classic deployment model</span></span>
<span data-ttu-id="6e71d-104">Hozzon létre, és kezelheti a hálózati hozzáférés-vezérlési listák (ACL) végpontok Azure PowerShell használatával vagy a kezelési portál hello.</span><span class="sxs-lookup"><span data-stu-id="6e71d-104">You can create and manage Network Access Control Lists (ACLs) for endpoints by using Azure PowerShell or in hello Management Portal.</span></span> <span data-ttu-id="6e71d-105">Ebben a témakörben található eljárások a hozzáférés-vezérlési lista leggyakoribb feladatokat, amelyek a PowerShell segítségével hajthatja végre.</span><span class="sxs-lookup"><span data-stu-id="6e71d-105">In this topic, you'll find procedures for ACL common tasks that you can complete using PowerShell.</span></span> <span data-ttu-id="6e71d-106">Hello listája Azure PowerShell parancsmagok: [Azure parancsmagokat](http://go.microsoft.com/fwlink/?LinkId=317721).</span><span class="sxs-lookup"><span data-stu-id="6e71d-106">For hello list of Azure PowerShell cmdlets see [Azure Management Cmdlets](http://go.microsoft.com/fwlink/?LinkId=317721).</span></span> <span data-ttu-id="6e71d-107">Hozzáférés-vezérlési listák kapcsolatos további információkért lásd: [Mi az a hálózati hozzáférés-vezérlési lista (ACL)?](virtual-networks-acl.md).</span><span class="sxs-lookup"><span data-stu-id="6e71d-107">For more information about ACLs, see [What is a Network Access Control List (ACL)?](virtual-networks-acl.md).</span></span> <span data-ttu-id="6e71d-108">Ha azt szeretné toomanage a hozzáférés-vezérlési listák hello felügyeleti portál használatával, lásd: [hogyan tooSet be végpontok tooa virtuális gép](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6e71d-108">If you want toomanage your ACLs by using hello Management Portal, see [How tooSet Up Endpoints tooa Virtual Machine](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

## <a name="manage-network-acls-by-using-azure-powershell"></a><span data-ttu-id="6e71d-109">Hálózati hozzáférés-vezérlési listák kezelése az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="6e71d-109">Manage Network ACLs by using Azure PowerShell</span></span>
<span data-ttu-id="6e71d-110">Használhatja az Azure PowerShell-parancsmagok toocreate, távolítsa el, és beállítása (set) hálózati hozzáférés-vezérlési listák (ACL).</span><span class="sxs-lookup"><span data-stu-id="6e71d-110">You can use Azure PowerShell cmdlets toocreate, remove, and configure (set) Network Access Control Lists (ACLs).</span></span> <span data-ttu-id="6e71d-111">Néhány példa a néhány hello módon egy hozzáférés-vezérlési lista PowerShell használatával is konfigurálhatja azt szerepel.</span><span class="sxs-lookup"><span data-stu-id="6e71d-111">We've included a few examples of some of hello ways you can configure an ACL using PowerShell.</span></span>

<span data-ttu-id="6e71d-112">tooretrieve hello ACL PowerShell-parancsmagok teljes listája, hello következő választhat:</span><span class="sxs-lookup"><span data-stu-id="6e71d-112">tooretrieve a complete list of hello ACL PowerShell cmdlets, you can use either of hello following:</span></span>

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### <a name="create-a-network-acl-with-rules-that-permit-access-from-a-remote-subnet"></a><span data-ttu-id="6e71d-113">Hozzon létre egy hálózati hozzáférés-vezérlési lista szabályokat, amelyek lehetővé teszik a hozzáférést egy távoli alhálózatból</span><span class="sxs-lookup"><span data-stu-id="6e71d-113">Create a Network ACL with rules that permit access from a remote subnet</span></span>
<span data-ttu-id="6e71d-114">hello az alábbi példa bemutatja, egy módon toocreate egy új hozzáférés-vezérlési lista szabályokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6e71d-114">hello example below illustrates a way toocreate a new ACL that contains rules.</span></span> <span data-ttu-id="6e71d-115">A rendszer ezután alkalmazza a hozzáférés-vezérlési lista tooa virtuális gép végpontja.</span><span class="sxs-lookup"><span data-stu-id="6e71d-115">This ACL is then applied tooa virtual machine endpoint.</span></span> <span data-ttu-id="6e71d-116">hello ACL-szabályok az alábbi példa hello lehetővé teszi a hozzáférést a távoli alhálózati.</span><span class="sxs-lookup"><span data-stu-id="6e71d-116">hello ACL rules in hello example below will allow access from a remote subnet.</span></span> <span data-ttu-id="6e71d-117">az Azure PowerShell ISE toocreate egy új hálózati hozzáférés-vezérlési lista az engedélyezési szabályainak távoli alhálózati, nyissa meg.</span><span class="sxs-lookup"><span data-stu-id="6e71d-117">toocreate a new Network ACL with permit rules for a remote subnet, open an Azure PowerShell ISE.</span></span> <span data-ttu-id="6e71d-118">Másolja és illessze be az alábbi parancsfájl hello konfigurálása a saját értékekkel hello parancsfájlt, majd futtassa az hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="6e71d-118">Copy and paste hello script below, configuring hello script with your own values, and then run hello script.</span></span>

1. <span data-ttu-id="6e71d-119">Hozzon létre új hálózati hozzáférés-vezérlési lista hello objektum.</span><span class="sxs-lookup"><span data-stu-id="6e71d-119">Create hello new network ACL object.</span></span>
   
        $acl1 = New-AzureAclConfig
2. <span data-ttu-id="6e71d-120">Olyan szabály, amely lehetővé teszi a hozzáférést a távoli alhálózati beállítása.</span><span class="sxs-lookup"><span data-stu-id="6e71d-120">Set a rule that permits access from a remote subnet.</span></span> <span data-ttu-id="6e71d-121">Hello az alábbi példában a szabály beállítása *100* (amely elsőbbséget élveznek szabály 200 és újabb) tooallow hello távoli alhálózati *10.0.0.0/8* toohello virtuális gép végpontjának eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="6e71d-121">In hello example below, you set rule *100* (which has priority over rule 200 and higher) tooallow hello remote subnet *10.0.0.0/8* access toohello virtual machine endpoint.</span></span> <span data-ttu-id="6e71d-122">Cserélje le a saját konfigurációs követelmények hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="6e71d-122">Replace hello values with your own configuration requirements.</span></span> <span data-ttu-id="6e71d-123">hello neve "SharePoint ACL-config" hello rövid nevet, amelyet toocall Ez a szabály cserélni.</span><span class="sxs-lookup"><span data-stu-id="6e71d-123">hello name "SharePoint ACL config" should be replaced with hello friendly name that you want toocall this rule.</span></span>
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"
3. <span data-ttu-id="6e71d-124">Ismételje meg a további szabályok hello parancsmag hello értékeket a saját konfigurációs követelményeinek, és cserélje le.</span><span class="sxs-lookup"><span data-stu-id="6e71d-124">For additional rules, repeat hello cmdlet, replacing hello values with your own configuration requirements.</span></span> <span data-ttu-id="6e71d-125">Lehet, hogy toochange hello szabály számú tooreflect hello rendelés hello szabályok toobe alkalmazni kívánt.</span><span class="sxs-lookup"><span data-stu-id="6e71d-125">Be sure toochange hello rule number Order tooreflect hello order in which you want hello rules toobe applied.</span></span> <span data-ttu-id="6e71d-126">hello szabály kevesebb élvez hello nagyobb számot.</span><span class="sxs-lookup"><span data-stu-id="6e71d-126">hello lower rule number takes precedence over hello higher number.</span></span>
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
4. <span data-ttu-id="6e71d-127">Ezután hozzon létre egy új végpontot (Hozzáadás), vagy állítsa be a hozzáférés-vezérlési lista hello meglévő a végpont (Set).</span><span class="sxs-lookup"><span data-stu-id="6e71d-127">Next, you can either create a new endpoint (Add) or set hello ACL for an existing endpoint (Set).</span></span> <span data-ttu-id="6e71d-128">Ebben a példában adunk hozzá egy új virtuális gép végpontjának neve "web" és a frissítés hello virtuális gép végpontjának hello az ACL beállításokról.</span><span class="sxs-lookup"><span data-stu-id="6e71d-128">In this example, we will add a new virtual machine endpoint called "web" and update hello virtual machine endpoint with hello ACL settings.</span></span>
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM
5. <span data-ttu-id="6e71d-129">A következő hello parancsmagjait, és hello parancsprogrammal.</span><span class="sxs-lookup"><span data-stu-id="6e71d-129">Next, combine hello cmdlets and run hello script.</span></span> <span data-ttu-id="6e71d-130">Ehhez a példához hello kombinált parancsmagok néz ki:</span><span class="sxs-lookup"><span data-stu-id="6e71d-130">For this example, hello combined cmdlets would look like this:</span></span>
   
        $acl1 = New-AzureAclConfig
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "Sharepoint ACL config"
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        |Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        |Update-AzureVM

### <a name="remove-a-network-acl-rule-that-permits-access-from-a-remote-subnet"></a><span data-ttu-id="6e71d-131">Távolítsa el a hálózati hozzáférés-vezérlési lista szabályt, amely lehetővé teszi a hozzáférést egy távoli alhálózatból</span><span class="sxs-lookup"><span data-stu-id="6e71d-131">Remove a Network ACL rule that permits access from a remote subnet</span></span>
<span data-ttu-id="6e71d-132">az alábbi példa hello egy módon tooremove hálózati hozzáférés-vezérlési lista szabályt mutatja be.</span><span class="sxs-lookup"><span data-stu-id="6e71d-132">hello example below illustrates a way tooremove a network ACL rule.</span></span>  <span data-ttu-id="6e71d-133">a távoli alhálózati, nyissa meg az Azure PowerShell ISE szabályainak tooremove egy hálózati hozzáférés-vezérlési lista szabály engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6e71d-133">tooremove a Network ACL rule with permit rules for a remote subnet, open an Azure PowerShell ISE.</span></span> <span data-ttu-id="6e71d-134">Másolja és illessze be az alábbi parancsfájl hello konfigurálása a saját értékekkel hello parancsfájlt, majd futtassa az hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="6e71d-134">Copy and paste hello script below, configuring hello script with your own values, and then run hello script.</span></span>

1. <span data-ttu-id="6e71d-135">Első lépés egy olyan tooget hello hálózati hozzáférés-vezérlési lista objektum hello virtuális gép végpontja.</span><span class="sxs-lookup"><span data-stu-id="6e71d-135">First step is tooget hello Network ACL object for hello virtual machine endpoint.</span></span> <span data-ttu-id="6e71d-136">Hello ACL-szabályának távolítsa el lesz.</span><span class="sxs-lookup"><span data-stu-id="6e71d-136">You'll then remove hello ACL rule.</span></span> <span data-ttu-id="6e71d-137">Ebben az esetben megszüntetjük azt által szabály azonosítója.</span><span class="sxs-lookup"><span data-stu-id="6e71d-137">In this case, we are removing it by rule ID.</span></span> <span data-ttu-id="6e71d-138">Hello Szabályazonosító 0 hello hozzáférés-vezérlési lista csak a művelettel eltávolítja.</span><span class="sxs-lookup"><span data-stu-id="6e71d-138">This will only remove hello rule ID 0 from hello ACL.</span></span> <span data-ttu-id="6e71d-139">Hello ACL objektum nem távolítja el a virtuális gép végpontjának hello.</span><span class="sxs-lookup"><span data-stu-id="6e71d-139">It does not remove hello ACL object from hello virtual machine endpoint.</span></span>
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1
2. <span data-ttu-id="6e71d-140">Ezt követően kell alkalmaznia hello hálózati hozzáférés-vezérlési lista objektum toohello virtuális gép végpontjának, és frissíti a hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="6e71d-140">Next, you must apply hello Network ACL object toohello virtual machine endpoint and update hello virtual machine.</span></span>
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### <a name="remove-a-network-acl-from-a-virtual-machine-endpoint"></a><span data-ttu-id="6e71d-141">Távolítsa el a hálózati hozzáférés-vezérlési lista egy virtuális gép végpontja</span><span class="sxs-lookup"><span data-stu-id="6e71d-141">Remove a Network ACL from a virtual machine endpoint</span></span>
<span data-ttu-id="6e71d-142">Bizonyos esetekben érdemes lehet tooremove egy virtuális gép végpontjának a hálózati hozzáférés-vezérlési lista objektumot.</span><span class="sxs-lookup"><span data-stu-id="6e71d-142">In certain scenarios, you might want tooremove a Network ACL object from a virtual machine endpoint.</span></span> <span data-ttu-id="6e71d-143">Ezt követően nyissa meg az Azure PowerShell ISE toodo.</span><span class="sxs-lookup"><span data-stu-id="6e71d-143">toodo that, open an Azure PowerShell ISE.</span></span> <span data-ttu-id="6e71d-144">Másolja és illessze be az alábbi parancsfájl hello konfigurálása a saját értékekkel hello parancsfájlt, majd futtassa az hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="6e71d-144">Copy and paste hello script below, configuring hello script with your own values, and then run hello script.</span></span>

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## <a name="next-steps"></a><span data-ttu-id="6e71d-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6e71d-145">Next steps</span></span>
[<span data-ttu-id="6e71d-146">Mi az a hálózati hozzáférés-vezérlési lista (ACL)?</span><span class="sxs-lookup"><span data-stu-id="6e71d-146">What is a Network Access Control List (ACL)?</span></span>](virtual-networks-acl.md)

