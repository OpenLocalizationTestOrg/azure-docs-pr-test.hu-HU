---
title: "Azure-végpont hozzáférés-vezérlési listák kezelése |} PowerShell |} Klasszikus |} Microsoft Docs"
description: "Megtudhatja, hogyan kezelheti a hozzáférés-vezérlési listákat a PowerShell használatával"
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
ms.openlocfilehash: c3476908447380ccd7e8b9c0f1c2a55ae763cc1e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-endpoint-access-control-lists-using-powershell-in-the-classic-deployment-model"></a><span data-ttu-id="a4f5e-103">A klasszikus üzembe helyezési modellel powershellel végpont hozzáférés-vezérlési listák kezelése</span><span class="sxs-lookup"><span data-stu-id="a4f5e-103">Manage endpoint access control lists using PowerShell in the classic deployment model</span></span>
<span data-ttu-id="a4f5e-104">Hozzon létre, és kezelheti a hálózati hozzáférés-vezérlési listák (ACL) végpontok Azure PowerShell használatával vagy a felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-104">You can create and manage Network Access Control Lists (ACLs) for endpoints by using Azure PowerShell or in the Management Portal.</span></span> <span data-ttu-id="a4f5e-105">Ebben a témakörben található eljárások a hozzáférés-vezérlési lista leggyakoribb feladatokat, amelyek a PowerShell segítségével hajthatja végre.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-105">In this topic, you'll find procedures for ACL common tasks that you can complete using PowerShell.</span></span> <span data-ttu-id="a4f5e-106">A listán szereplő Azure PowerShell parancsmagok lásd [Azure parancsmagokat](http://go.microsoft.com/fwlink/?LinkId=317721).</span><span class="sxs-lookup"><span data-stu-id="a4f5e-106">For the list of Azure PowerShell cmdlets see [Azure Management Cmdlets](http://go.microsoft.com/fwlink/?LinkId=317721).</span></span> <span data-ttu-id="a4f5e-107">Hozzáférés-vezérlési listák kapcsolatos további információkért lásd: [Mi az a hálózati hozzáférés-vezérlési lista (ACL)?](virtual-networks-acl.md).</span><span class="sxs-lookup"><span data-stu-id="a4f5e-107">For more information about ACLs, see [What is a Network Access Control List (ACL)?](virtual-networks-acl.md).</span></span> <span data-ttu-id="a4f5e-108">Ha azt szeretné, hogy az ACL-ek kezelése a felügyeleti portál használatával kapcsolatos tudnivalókat lásd: [hogyan állítsa be végpontok egy virtuális géphez](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a4f5e-108">If you want to manage your ACLs by using the Management Portal, see [How to Set Up Endpoints to a Virtual Machine](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

## <a name="manage-network-acls-by-using-azure-powershell"></a><span data-ttu-id="a4f5e-109">Hálózati hozzáférés-vezérlési listák kezelése az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="a4f5e-109">Manage Network ACLs by using Azure PowerShell</span></span>
<span data-ttu-id="a4f5e-110">Azure PowerShell-parancsmagok segítségével létrehozása, törlése és beállítása (set) hálózati hozzáférés-vezérlési listák (ACL).</span><span class="sxs-lookup"><span data-stu-id="a4f5e-110">You can use Azure PowerShell cmdlets to create, remove, and configure (set) Network Access Control Lists (ACLs).</span></span> <span data-ttu-id="a4f5e-111">Néhány példa a néhány módot is beállíthat egy hozzáférés-vezérlési lista PowerShell-lel azt szerepel.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-111">We've included a few examples of some of the ways you can configure an ACL using PowerShell.</span></span>

<span data-ttu-id="a4f5e-112">A hozzáférés-vezérlési lista PowerShell-parancsmagok teljes listájának lekéréséhez az alábbiak bármelyikét használhatja:</span><span class="sxs-lookup"><span data-stu-id="a4f5e-112">To retrieve a complete list of the ACL PowerShell cmdlets, you can use either of the following:</span></span>

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### <a name="create-a-network-acl-with-rules-that-permit-access-from-a-remote-subnet"></a><span data-ttu-id="a4f5e-113">Hozzon létre egy hálózati hozzáférés-vezérlési lista szabályokat, amelyek lehetővé teszik a hozzáférést egy távoli alhálózatból</span><span class="sxs-lookup"><span data-stu-id="a4f5e-113">Create a Network ACL with rules that permit access from a remote subnet</span></span>
<span data-ttu-id="a4f5e-114">Az alábbi példában látható módon, hogy hozzon létre egy új hozzáférés-vezérlési lista szabályokat tartalmaz egy módja.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-114">The example below illustrates a way to create a new ACL that contains rules.</span></span> <span data-ttu-id="a4f5e-115">A hozzáférés-vezérlési lista alkalmazva lesz a virtuális gép végpontja.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-115">This ACL is then applied to a virtual machine endpoint.</span></span> <span data-ttu-id="a4f5e-116">Az ACL-szabályok a következő példa engedélyezi a hozzáférést a távoli alhálózati.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-116">The ACL rules in the example below will allow access from a remote subnet.</span></span> <span data-ttu-id="a4f5e-117">Hozzon létre egy új hálózati hozzáférés-vezérlési lista engedélyezési szabályainak távoli alhálózati, nyissa meg az Azure PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-117">To create a new Network ACL with permit rules for a remote subnet, open an Azure PowerShell ISE.</span></span> <span data-ttu-id="a4f5e-118">Másolja és illessze be az alábbi, a parancsfájl konfigurálása a saját értékekkel parancsfájlt, és futtassa a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-118">Copy and paste the script below, configuring the script with your own values, and then run the script.</span></span>

1. <span data-ttu-id="a4f5e-119">Hozza létre az új hálózati hozzáférés-vezérlési lista objektumot.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-119">Create the new network ACL object.</span></span>
   
        $acl1 = New-AzureAclConfig
2. <span data-ttu-id="a4f5e-120">Olyan szabály, amely lehetővé teszi a hozzáférést a távoli alhálózati beállítása.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-120">Set a rule that permits access from a remote subnet.</span></span> <span data-ttu-id="a4f5e-121">Az alábbi példában a szabály beállítása *100* (amely elsőbbséget élveznek szabály 200 és újabb) engedélyezi a távoli alhálózati *10.0.0.0/8* a virtuális gép végpontjának eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-121">In the example below, you set rule *100* (which has priority over rule 200 and higher) to allow the remote subnet *10.0.0.0/8* access to the virtual machine endpoint.</span></span> <span data-ttu-id="a4f5e-122">Cserélje le az értékeket a saját konfigurációs követelményeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-122">Replace the values with your own configuration requirements.</span></span> <span data-ttu-id="a4f5e-123">A rövid név, amelyet szeretne hívás Ez a szabály neve "SharePoint ACL-config" cserélni.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-123">The name "SharePoint ACL config" should be replaced with the friendly name that you want to call this rule.</span></span>
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"
3. <span data-ttu-id="a4f5e-124">A további szabályok ismételje meg az értékeket a saját konfigurációs követelményeinek, és cserélje le a következő parancsmag.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-124">For additional rules, repeat the cmdlet, replacing the values with your own configuration requirements.</span></span> <span data-ttu-id="a4f5e-125">Ügyeljen arra, hogy a szabály módosításához rendelést, hogy tükrözze a sorrendet, amelyben a szabályokat alkalmazni kívánt szám.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-125">Be sure to change the rule number Order to reflect the order in which you want the rules to be applied.</span></span> <span data-ttu-id="a4f5e-126">A szabály kevesebb élvez a nagyobb számot.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-126">The lower rule number takes precedence over the higher number.</span></span>
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
4. <span data-ttu-id="a4f5e-127">Következő lépésként hozzon létre egy új végpontot (Hozzáadás), vagy állítsa be a hozzáférés-vezérlési listája egy meglévő végpontot (Set).</span><span class="sxs-lookup"><span data-stu-id="a4f5e-127">Next, you can either create a new endpoint (Add) or set the ACL for an existing endpoint (Set).</span></span> <span data-ttu-id="a4f5e-128">Ebben a példában a rendszer a "web" nevű új virtuális gép végpontjának felvétele és a virtuális gép végpontjának frissítése a hozzáférés-vezérlési lista beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-128">In this example, we will add a new virtual machine endpoint called "web" and update the virtual machine endpoint with the ACL settings.</span></span>
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM
5. <span data-ttu-id="a4f5e-129">A következő parancsmagjait, és futtassa a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-129">Next, combine the cmdlets and run the script.</span></span> <span data-ttu-id="a4f5e-130">Az ebben a példában a kombinált parancsmagok néz ki:</span><span class="sxs-lookup"><span data-stu-id="a4f5e-130">For this example, the combined cmdlets would look like this:</span></span>
   
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

### <a name="remove-a-network-acl-rule-that-permits-access-from-a-remote-subnet"></a><span data-ttu-id="a4f5e-131">Távolítsa el a hálózati hozzáférés-vezérlési lista szabályt, amely lehetővé teszi a hozzáférést egy távoli alhálózatból</span><span class="sxs-lookup"><span data-stu-id="a4f5e-131">Remove a Network ACL rule that permits access from a remote subnet</span></span>
<span data-ttu-id="a4f5e-132">Az alábbi példában látható módon egy hálózati ACL-szabályának eltávolítható.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-132">The example below illustrates a way to remove a network ACL rule.</span></span>  <span data-ttu-id="a4f5e-133">Engedélyezési szabályainak távoli alhálózati hálózati hozzáférés-vezérlési lista szabály eltávolításához nyissa meg az Azure PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-133">To remove a Network ACL rule with permit rules for a remote subnet, open an Azure PowerShell ISE.</span></span> <span data-ttu-id="a4f5e-134">Másolja és illessze be az alábbi, a parancsfájl konfigurálása a saját értékekkel parancsfájlt, és futtassa a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-134">Copy and paste the script below, configuring the script with your own values, and then run the script.</span></span>

1. <span data-ttu-id="a4f5e-135">Első lépés a hálózati hozzáférés-vezérlési lista objektum lekérése a virtuális gép végpontja.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-135">First step is to get the Network ACL object for the virtual machine endpoint.</span></span> <span data-ttu-id="a4f5e-136">Az ACL-szabályának távolítsa el lesz.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-136">You'll then remove the ACL rule.</span></span> <span data-ttu-id="a4f5e-137">Ebben az esetben megszüntetjük azt által szabály azonosítója.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-137">In this case, we are removing it by rule ID.</span></span> <span data-ttu-id="a4f5e-138">A szabály azonosítója 0 a hozzáférés-vezérlési lista csak a művelettel eltávolítja.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-138">This will only remove the rule ID 0 from the ACL.</span></span> <span data-ttu-id="a4f5e-139">Az ACL objektum nem távolítja el a virtuális gép végpontjának a.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-139">It does not remove the ACL object from the virtual machine endpoint.</span></span>
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1
2. <span data-ttu-id="a4f5e-140">Ezt követően kell alkalmazni a hálózati hozzáférés-vezérlési lista objektum a virtuális gép végpontjának, és frissítse a virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-140">Next, you must apply the Network ACL object to the virtual machine endpoint and update the virtual machine.</span></span>
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### <a name="remove-a-network-acl-from-a-virtual-machine-endpoint"></a><span data-ttu-id="a4f5e-141">Távolítsa el a hálózati hozzáférés-vezérlési lista egy virtuális gép végpontja</span><span class="sxs-lookup"><span data-stu-id="a4f5e-141">Remove a Network ACL from a virtual machine endpoint</span></span>
<span data-ttu-id="a4f5e-142">Bizonyos esetekben érdemes a hálózati hozzáférés-vezérlési lista objektumok eltávolítása a virtuális gép végpontja.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-142">In certain scenarios, you might want to remove a Network ACL object from a virtual machine endpoint.</span></span> <span data-ttu-id="a4f5e-143">Ehhez nyissa meg az Azure PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-143">To do that, open an Azure PowerShell ISE.</span></span> <span data-ttu-id="a4f5e-144">Másolja és illessze be az alábbi, a parancsfájl konfigurálása a saját értékekkel parancsfájlt, és futtassa a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="a4f5e-144">Copy and paste the script below, configuring the script with your own values, and then run the script.</span></span>

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## <a name="next-steps"></a><span data-ttu-id="a4f5e-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a4f5e-145">Next steps</span></span>
[<span data-ttu-id="a4f5e-146">Mi az a hálózati hozzáférés-vezérlési lista (ACL)?</span><span class="sxs-lookup"><span data-stu-id="a4f5e-146">What is a Network Access Control List (ACL)?</span></span>](virtual-networks-acl.md)

