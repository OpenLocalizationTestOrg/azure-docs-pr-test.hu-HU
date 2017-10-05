---
title: "Gyakori kérdések a Linux virtuális gépek Azure-ban |} Microsoft Docs"
description: "A Resource Manager modellt létrehozott Linux virtuális gépek kapcsolatos gyakori kérdésekre adott válaszok biztosít."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-management
ms.assetid: 3648e09c-1115-4818-93c6-688d7a54a353
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: cynthn
ms.openlocfilehash: 0e06d21bd0b6ef807f38e41dcd50c9cd715607a3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="frequently-asked-question-about-linux-virtual-machines"></a><span data-ttu-id="da25d-103">Linux virtuális gépek kapcsolatos gyakran ismételt kérdések</span><span class="sxs-lookup"><span data-stu-id="da25d-103">Frequently asked question about Linux Virtual Machines</span></span>
<span data-ttu-id="da25d-104">Ez a cikk foglalkozik az Azure-ban a Resource Manager üzembe helyezési modellel létrehozott Linux virtuális gépek kapcsolatos gyakori kérdésekre.</span><span class="sxs-lookup"><span data-stu-id="da25d-104">This article addresses some common questions about Linux virtual machines created in Azure using the Resource Manager deployment model.</span></span> <span data-ttu-id="da25d-105">Ez a témakör a Windows verziója: [gyakran feltett kérdés kapcsolatban a Windows virtuális gépek](../windows/faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="da25d-105">For the Windows version of this topic, see [Frequently asked question about Windows Virtual Machines](../windows/faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>

## <a name="what-can-i-run-on-an-azure-vm"></a><span data-ttu-id="da25d-106">Mit futtathatok egy Azure-beli virtuális gépen?</span><span class="sxs-lookup"><span data-stu-id="da25d-106">What can I run on an Azure VM?</span></span>
<span data-ttu-id="da25d-107">Minden előfizető kiszolgálószoftvereket futtathat az Azure-beli virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="da25d-107">All subscribers can run server software on an Azure virtual machine.</span></span> <span data-ttu-id="da25d-108">További információkért lásd: [Azure-Endorsed Terjesztéseket Linux](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="da25d-108">For more information, see [Linux on Azure-Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a><span data-ttu-id="da25d-109">Mennyi tárhelyet használhatok egy virtuális gép esetén?</span><span class="sxs-lookup"><span data-stu-id="da25d-109">How much storage can I use with a virtual machine?</span></span>
<span data-ttu-id="da25d-110">Minden adatlemez akár 1 TB méretű is lehet.</span><span class="sxs-lookup"><span data-stu-id="da25d-110">Each data disk can be up to 1 TB.</span></span> <span data-ttu-id="da25d-111">A használható adatlemezek száma a virtuális gép méretétől függ.</span><span class="sxs-lookup"><span data-stu-id="da25d-111">The number of data disks you can use depends on the size of the virtual machine.</span></span> <span data-ttu-id="da25d-112">Részletek: [Virtuális gépek méretei](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="da25d-112">For details, see [Sizes for Virtual Machines](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="da25d-113">Az Azure-tárfiók tárhelyet biztosít az operációsrendszer-lemez és bármely adatlemez számára.</span><span class="sxs-lookup"><span data-stu-id="da25d-113">An Azure storage account provides storage for the operating system disk and any data disks.</span></span> <span data-ttu-id="da25d-114">Minden lemez egy lapblobként tárolt .vhd-fájl.</span><span class="sxs-lookup"><span data-stu-id="da25d-114">Each disk is a .vhd file stored as a page blob.</span></span> <span data-ttu-id="da25d-115">A díjszabás részleteiért lásd [a Storage szolgáltatás díjszabását](https://azure.microsoft.com/pricing/details/storage/).</span><span class="sxs-lookup"><span data-stu-id="da25d-115">For pricing details, see [Storage Pricing Details](https://azure.microsoft.com/pricing/details/storage/).</span></span>

## <a name="how-can-i-access-my-virtual-machine"></a><span data-ttu-id="da25d-116">Hogyan lehet fér hozzá a virtuális géphez?</span><span class="sxs-lookup"><span data-stu-id="da25d-116">How can I access my virtual machine?</span></span>
<span data-ttu-id="da25d-117">Jelentkezzen be a virtuális gépet, a Secure Shell (SSH) használatával történő távoli kapcsolatot létesíteni.</span><span class="sxs-lookup"><span data-stu-id="da25d-117">Establish a remote connection to log on to the virtual machine, using Secure Shell (SSH).</span></span> <span data-ttu-id="da25d-118">Kapcsolódás című [Windows](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy [a Linux és Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="da25d-118">See the instructions on how to connect [from Windows](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [from Linux and Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="da25d-119">Alapértelmezés szerint az SSH legfeljebb 10 párhuzamos kapcsolatot tesz lehetővé.</span><span class="sxs-lookup"><span data-stu-id="da25d-119">By default, SSH allows a maximum of 10 concurrent connections.</span></span> <span data-ttu-id="da25d-120">Ezt a számot a konfigurációs fájl szerkesztésével növelheti.</span><span class="sxs-lookup"><span data-stu-id="da25d-120">You can increase this number by editing the configuration file.</span></span>

<span data-ttu-id="da25d-121">Ha problémákat tapasztal, tekintse meg [hibaelhárítása Secure Shell (SSH) kapcsolatok](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="da25d-121">If you’re having problems, check out [Troubleshoot Secure Shell (SSH) connections](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="can-i-use-the-temporary-disk-devsdb1-to-store-data"></a><span data-ttu-id="da25d-122">Az ideiglenes lemez (/ dev/sdb1) használatával tárolja az adatokat?</span><span class="sxs-lookup"><span data-stu-id="da25d-122">Can I use the temporary disk (/dev/sdb1) to store data?</span></span>
<span data-ttu-id="da25d-123">Ne használjon a mennyiségű ideiglenes lemezes (/ dev/sdb1) adatainak tárolásához.</span><span class="sxs-lookup"><span data-stu-id="da25d-123">Don't use the temporary disk (/dev/sdb1) to store data.</span></span> <span data-ttu-id="da25d-124">Éppen csak átmeneti tárolására.</span><span class="sxs-lookup"><span data-stu-id="da25d-124">It is only there for temporary storage.</span></span> <span data-ttu-id="da25d-125">Veszhetnek el, amelyek nem állíthatók vissza adatokat.</span><span class="sxs-lookup"><span data-stu-id="da25d-125">You risk losing data that can’t be recovered.</span></span>

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a><span data-ttu-id="da25d-126">Másolja vagy egy meglévő Azure virtuális gép klónozása?</span><span class="sxs-lookup"><span data-stu-id="da25d-126">Can I copy or clone an existing Azure VM?</span></span>
<span data-ttu-id="da25d-127">Igen.</span><span class="sxs-lookup"><span data-stu-id="da25d-127">Yes.</span></span> <span data-ttu-id="da25d-128">Útmutatásért lásd: [létrehozása Linux virtuális gép másolatát a Resource Manager üzembe helyezési modellel](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="da25d-128">For instructions, see [How to create a copy of a Linux virtual machine in the Resource Manager deployment model](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a><span data-ttu-id="da25d-129">Miért nem jelenik meg Kanada központi és Kanada keleti régiók Azure Resource Manageren keresztül?</span><span class="sxs-lookup"><span data-stu-id="da25d-129">Why am I not seeing Canada Central and Canada East regions through Azure Resource Manager?</span></span>
<span data-ttu-id="da25d-130">A két új régiók Kanada központi és Kanada keleti nem automatikusan regisztrálva van a virtuális gép létrehozásához a meglévő Azure-előfizetések.</span><span class="sxs-lookup"><span data-stu-id="da25d-130">The two new regions of Canada Central and Canada East are not automatically registered for virtual machine creation for existing Azure subscriptions.</span></span> <span data-ttu-id="da25d-131">Ez a regisztráció automatikusan történik, amikor egy virtuális gép telepítése Azure Resource Manager használatával bármely más régióban az Azure portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="da25d-131">This registration is done automatically when a virtual machine is deployed through the Azure portal to any other region using Azure Resource Manager.</span></span> <span data-ttu-id="da25d-132">Bármely más Azure-régió, hogy a virtuális gép telepítése után az új régiók elérhetőknek kell lenniük a következő virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="da25d-132">After a virtual machine is deployed to any other Azure region, the new regions should be available for subsequent virtual machines.</span></span>

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a><span data-ttu-id="da25d-133">Hozzáadható egy hálózati Adaptert a virtuális gép létrehozása után?</span><span class="sxs-lookup"><span data-stu-id="da25d-133">Can I add a NIC to my VM after it's created?</span></span>
<span data-ttu-id="da25d-134">Igen, ez a lehetőség most.</span><span class="sxs-lookup"><span data-stu-id="da25d-134">Yes, this is now possible.</span></span> <span data-ttu-id="da25d-135">A virtuális gép először meg kell állítani a felszabadított.</span><span class="sxs-lookup"><span data-stu-id="da25d-135">The VM first needs to be stopped deallocated.</span></span> <span data-ttu-id="da25d-136">Majd adja hozzá, vagy távolítsa el a hálózati adapter (kivéve, ha a virtuális gép utolsó hálózati adapter).</span><span class="sxs-lookup"><span data-stu-id="da25d-136">Then you can add or remove a NIC (unless it's the last NIC on the VM).</span></span> 

## <a name="are-there-any-computer-name-requirements"></a><span data-ttu-id="da25d-137">Vannak-e a számítógép neve követelmények?</span><span class="sxs-lookup"><span data-stu-id="da25d-137">Are there any computer name requirements?</span></span>
<span data-ttu-id="da25d-138">Igen.</span><span class="sxs-lookup"><span data-stu-id="da25d-138">Yes.</span></span> <span data-ttu-id="da25d-139">A számítógép neve legfeljebb 64 karakter hosszúságú lehet.</span><span class="sxs-lookup"><span data-stu-id="da25d-139">The computer name can be a maximum of 64 characters in length.</span></span> <span data-ttu-id="da25d-140">Lásd: [elnevezési konvenciókat szabályokat és korlátozásokat](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) további információt az erőforrások elnevezési körül.</span><span class="sxs-lookup"><span data-stu-id="da25d-140">See [Naming conventions rules and restrictions](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for more information around naming your resources.</span></span>

## <a name="are-there-any-resource-group-name-requirements"></a><span data-ttu-id="da25d-141">Vannak-e valamely erőforrás csoport vonatkozó követelményeknek?</span><span class="sxs-lookup"><span data-stu-id="da25d-141">Are there any resource group name requirements?</span></span>
<span data-ttu-id="da25d-142">Igen.</span><span class="sxs-lookup"><span data-stu-id="da25d-142">Yes.</span></span> <span data-ttu-id="da25d-143">Az erőforráscsoport neve legfeljebb 90 karakter hosszúságú lehet.</span><span class="sxs-lookup"><span data-stu-id="da25d-143">The resource group name can be a maximum of 90 characters in length.</span></span> <span data-ttu-id="da25d-144">Lásd: [elnevezési konvenciókat szabályokat és korlátozásokat](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) erőforráscsoportok további információt.</span><span class="sxs-lookup"><span data-stu-id="da25d-144">See [Naming conventions rules and restrictions](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for more information about resource groups.</span></span>

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a><span data-ttu-id="da25d-145">Virtuális gép létrehozásakor Mik a username követelmények?</span><span class="sxs-lookup"><span data-stu-id="da25d-145">What are the username requirements when creating a VM?</span></span>
<span data-ttu-id="da25d-146">Felhasználónevek 1 – 64 karakter hosszúságúnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="da25d-146">Usernames must be 1 - 64 characters in length.</span></span>

<span data-ttu-id="da25d-147">A következő felhasználónevek nem engedélyezettek:</span><span class="sxs-lookup"><span data-stu-id="da25d-147">The following usernames are not allowed:</span></span>

<table>
    <tr>
        <td style="text-align:center"><span data-ttu-id="da25d-148">Rendszergazda</span><span class="sxs-lookup"><span data-stu-id="da25d-148">administrator</span></span> </td><td style="text-align:center"> <span data-ttu-id="da25d-149">Rendszergazda</span><span class="sxs-lookup"><span data-stu-id="da25d-149">admin</span></span> </td><td style="text-align:center"> <span data-ttu-id="da25d-150">Felhasználó</span><span class="sxs-lookup"><span data-stu-id="da25d-150">user</span></span> </td><td style="text-align:center"> <span data-ttu-id="da25d-151">Felhasználó1</span><span class="sxs-lookup"><span data-stu-id="da25d-151">user1</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="da25d-152">Teszt</span><span class="sxs-lookup"><span data-stu-id="da25d-152">test</span></span> </td><td style="text-align:center"> <span data-ttu-id="da25d-153">Felhasználó2</span><span class="sxs-lookup"><span data-stu-id="da25d-153">user2</span></span> </td><td style="text-align:center"> <span data-ttu-id="da25d-154">Test1</span><span class="sxs-lookup"><span data-stu-id="da25d-154">test1</span></span> </td><td style="text-align:center"> <span data-ttu-id="da25d-155">Felhasználó3</span><span class="sxs-lookup"><span data-stu-id="da25d-155">user3</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="da25d-156">rendszergazda1</span><span class="sxs-lookup"><span data-stu-id="da25d-156">admin1</span></span> </td><td style="text-align:center"> <span data-ttu-id="da25d-157">1</span><span class="sxs-lookup"><span data-stu-id="da25d-157">1</span></span> </td><td style="text-align:center"> <span data-ttu-id="da25d-158">123</span><span class="sxs-lookup"><span data-stu-id="da25d-158">123</span></span> </td><td style="text-align:center"> <span data-ttu-id="da25d-159">egy</span><span class="sxs-lookup"><span data-stu-id="da25d-159">a</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="da25d-160">actuser</span><span class="sxs-lookup"><span data-stu-id="da25d-160">actuser</span></span>  </td><td style="text-align:center"> <span data-ttu-id="da25d-161">ADM</span><span class="sxs-lookup"><span data-stu-id="da25d-161">adm</span></span> </td><td style="text-align:center"> <span data-ttu-id="da25d-162">admin2</span><span class="sxs-lookup"><span data-stu-id="da25d-162">admin2</span></span> </td><td style="text-align:center"> <span data-ttu-id="da25d-163">ASPNET</span><span class="sxs-lookup"><span data-stu-id="da25d-163">aspnet</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="da25d-164">biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="da25d-164">backup</span></span> </td><td style="text-align:center"> <span data-ttu-id="da25d-165">Konzol</span><span class="sxs-lookup"><span data-stu-id="da25d-165">console</span></span> </td><td style="text-align:center"> <span data-ttu-id="da25d-166">David</span><span class="sxs-lookup"><span data-stu-id="da25d-166">david</span></span> </td><td style="text-align:center"> <span data-ttu-id="da25d-167">Vendég</span><span class="sxs-lookup"><span data-stu-id="da25d-167">guest</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="da25d-168">John</span><span class="sxs-lookup"><span data-stu-id="da25d-168">john</span></span> </td><td style="text-align:center"> <span data-ttu-id="da25d-169">Tulajdonos</span><span class="sxs-lookup"><span data-stu-id="da25d-169">owner</span></span> </td><td style="text-align:center"> <span data-ttu-id="da25d-170">legfelső szintű</span><span class="sxs-lookup"><span data-stu-id="da25d-170">root</span></span> </td><td style="text-align:center"> <span data-ttu-id="da25d-171">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="da25d-171">server</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="da25d-172">SQL</span><span class="sxs-lookup"><span data-stu-id="da25d-172">sql</span></span> </td><td style="text-align:center"> <span data-ttu-id="da25d-173">Támogatás</span><span class="sxs-lookup"><span data-stu-id="da25d-173">support</span></span> </td><td style="text-align:center"> <span data-ttu-id="da25d-174">support_388945a0</span><span class="sxs-lookup"><span data-stu-id="da25d-174">support_388945a0</span></span> </td><td style="text-align:center"> <span data-ttu-id="da25d-175">sys</span><span class="sxs-lookup"><span data-stu-id="da25d-175">sys</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="da25d-176">Teszt2</span><span class="sxs-lookup"><span data-stu-id="da25d-176">test2</span></span> </td><td style="text-align:center"> <span data-ttu-id="da25d-177">Teszt3</span><span class="sxs-lookup"><span data-stu-id="da25d-177">test3</span></span> </td><td style="text-align:center"> <span data-ttu-id="da25d-178">Felhasználó4</span><span class="sxs-lookup"><span data-stu-id="da25d-178">user4</span></span> </td><td style="text-align:center"> <span data-ttu-id="da25d-179">user5</span><span class="sxs-lookup"><span data-stu-id="da25d-179">user5</span></span></td>
    </tr>
</table>


## <a name="what-are-the-password-requirements-when-creating-a-vm"></a><span data-ttu-id="da25d-180">A jelszavakra vonatkozó követelmények Mik azok a virtuális gép létrehozásakor?</span><span class="sxs-lookup"><span data-stu-id="da25d-180">What are the password requirements when creating a VM?</span></span>
<span data-ttu-id="da25d-181">Jelszavak 6 – 72 karakter hosszúságúnak kell és 3 kívül a következő 4 bonyolultsági megfeleljen:</span><span class="sxs-lookup"><span data-stu-id="da25d-181">Passwords must be 6 - 72 characters in length and meet 3 out of the following 4 complexity requirements:</span></span>

* <span data-ttu-id="da25d-182">Alacsonyabb karaktereket</span><span class="sxs-lookup"><span data-stu-id="da25d-182">Have lower characters</span></span>
* <span data-ttu-id="da25d-183">Felső karaktereket</span><span class="sxs-lookup"><span data-stu-id="da25d-183">Have upper characters</span></span>
* <span data-ttu-id="da25d-184">Rendelkezik egy számjegy</span><span class="sxs-lookup"><span data-stu-id="da25d-184">Have a digit</span></span>
* <span data-ttu-id="da25d-185">Rendelkezik egy speciális karaktert (reguláris kifejezéssel egyező [\W_])</span><span class="sxs-lookup"><span data-stu-id="da25d-185">Have a special character (Regex match [\W_])</span></span>

<span data-ttu-id="da25d-186">A következő jelszavak használata nem megengedett:</span><span class="sxs-lookup"><span data-stu-id="da25d-186">The following passwords are not allowed:</span></span>

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center"><span data-ttu-id="da25d-187">P@$$w0rd</span><span class="sxs-lookup"><span data-stu-id="da25d-187">P@$$w0rd</span></span></td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center"><span data-ttu-id="da25d-188">Pa$ $word</span><span class="sxs-lookup"><span data-stu-id="da25d-188">Pa$$word</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center"><span data-ttu-id="da25d-189">Jelszót!</span><span class="sxs-lookup"><span data-stu-id="da25d-189">Password!</span></span></td>
        <td style="text-align:center"><span data-ttu-id="da25d-190">Jelszó1</span><span class="sxs-lookup"><span data-stu-id="da25d-190">Password1</span></span></td>
        <td style="text-align:center"><span data-ttu-id="da25d-191">Password22</span><span class="sxs-lookup"><span data-stu-id="da25d-191">Password22</span></span></td>
        <td style="text-align:center"><span data-ttu-id="da25d-192">ILOVEYOU!</span><span class="sxs-lookup"><span data-stu-id="da25d-192">iloveyou!</span></span></td>
    </tr>
</table>
