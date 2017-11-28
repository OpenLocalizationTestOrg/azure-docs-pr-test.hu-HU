---
title: "Linux virtuális gépek Azure-ban kérdések aaaFrequently |} Microsoft Docs"
description: "Itt választ toosome hello hello Resource Manager modellt létrehozott Linux virtuális gépek kapcsolatos gyakori kérdéseket."
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
ms.openlocfilehash: 0afd08123dddc408851065c46deedc3146dbec20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-question-about-linux-virtual-machines"></a><span data-ttu-id="afe48-103">Linux virtuális gépek kapcsolatos gyakran ismételt kérdések</span><span class="sxs-lookup"><span data-stu-id="afe48-103">Frequently asked question about Linux Virtual Machines</span></span>
<span data-ttu-id="afe48-104">Ez a cikk foglalkozik az Azure-ban hello Resource Manager üzembe helyezési modellben létrehozott Linux virtuális gépek kapcsolatos gyakori kérdésekre.</span><span class="sxs-lookup"><span data-stu-id="afe48-104">This article addresses some common questions about Linux virtual machines created in Azure using hello Resource Manager deployment model.</span></span> <span data-ttu-id="afe48-105">Ez a témakör hello Windows verziója: [gyakran feltett kérdés kapcsolatban a Windows virtuális gépek](../windows/faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="afe48-105">For hello Windows version of this topic, see [Frequently asked question about Windows Virtual Machines](../windows/faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>

## <a name="what-can-i-run-on-an-azure-vm"></a><span data-ttu-id="afe48-106">Mit futtathatok egy Azure-beli virtuális gépen?</span><span class="sxs-lookup"><span data-stu-id="afe48-106">What can I run on an Azure VM?</span></span>
<span data-ttu-id="afe48-107">Minden előfizető kiszolgálószoftvereket futtathat az Azure-beli virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="afe48-107">All subscribers can run server software on an Azure virtual machine.</span></span> <span data-ttu-id="afe48-108">További információkért lásd: [Azure-Endorsed Terjesztéseket Linux](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="afe48-108">For more information, see [Linux on Azure-Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a><span data-ttu-id="afe48-109">Mennyi tárhelyet használhatok egy virtuális gép esetén?</span><span class="sxs-lookup"><span data-stu-id="afe48-109">How much storage can I use with a virtual machine?</span></span>
<span data-ttu-id="afe48-110">Minden egyes adatlemez mentése too1 TB lehet.</span><span class="sxs-lookup"><span data-stu-id="afe48-110">Each data disk can be up too1 TB.</span></span> <span data-ttu-id="afe48-111">adatlemezek használhatja hello száma hello hello virtuális gép méretétől függ.</span><span class="sxs-lookup"><span data-stu-id="afe48-111">hello number of data disks you can use depends on hello size of hello virtual machine.</span></span> <span data-ttu-id="afe48-112">Részletek: [Virtuális gépek méretei](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="afe48-112">For details, see [Sizes for Virtual Machines](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="afe48-113">Azure-tárfiók tárolási biztosít hello operációsrendszer-lemez és adatlemezeket.</span><span class="sxs-lookup"><span data-stu-id="afe48-113">An Azure storage account provides storage for hello operating system disk and any data disks.</span></span> <span data-ttu-id="afe48-114">Minden lemez egy lapblobként tárolt .vhd-fájl.</span><span class="sxs-lookup"><span data-stu-id="afe48-114">Each disk is a .vhd file stored as a page blob.</span></span> <span data-ttu-id="afe48-115">A díjszabás részleteiért lásd [a Storage szolgáltatás díjszabását](https://azure.microsoft.com/pricing/details/storage/).</span><span class="sxs-lookup"><span data-stu-id="afe48-115">For pricing details, see [Storage Pricing Details](https://azure.microsoft.com/pricing/details/storage/).</span></span>

## <a name="how-can-i-access-my-virtual-machine"></a><span data-ttu-id="afe48-116">Hogyan lehet fér hozzá a virtuális géphez?</span><span class="sxs-lookup"><span data-stu-id="afe48-116">How can I access my virtual machine?</span></span>
<span data-ttu-id="afe48-117">A távoli kapcsolat toolog toohello virtuális gépen, Secure Shell (SSH) használatával hoz létre.</span><span class="sxs-lookup"><span data-stu-id="afe48-117">Establish a remote connection toolog on toohello virtual machine, using Secure Shell (SSH).</span></span> <span data-ttu-id="afe48-118">Hello utasításokat meg, hogyan tooconnect [Windows](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy [a Linux és Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="afe48-118">See hello instructions on how tooconnect [from Windows](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [from Linux and Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="afe48-119">Alapértelmezés szerint az SSH legfeljebb 10 párhuzamos kapcsolatot tesz lehetővé.</span><span class="sxs-lookup"><span data-stu-id="afe48-119">By default, SSH allows a maximum of 10 concurrent connections.</span></span> <span data-ttu-id="afe48-120">Ezt a számot növelheti hello konfigurációs fájl szerkesztésével.</span><span class="sxs-lookup"><span data-stu-id="afe48-120">You can increase this number by editing hello configuration file.</span></span>

<span data-ttu-id="afe48-121">Ha problémákat tapasztal, tekintse meg [hibaelhárítása Secure Shell (SSH) kapcsolatok](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="afe48-121">If you’re having problems, check out [Troubleshoot Secure Shell (SSH) connections](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="can-i-use-hello-temporary-disk-devsdb1-toostore-data"></a><span data-ttu-id="afe48-122">Hello mennyiségű ideiglenes lemezes (/ dev/sdb1) toostore adatok használata</span><span class="sxs-lookup"><span data-stu-id="afe48-122">Can I use hello temporary disk (/dev/sdb1) toostore data?</span></span>
<span data-ttu-id="afe48-123">Ne használjon hello ideiglenes (/ dev/sdb1) toostore lemezadatokat.</span><span class="sxs-lookup"><span data-stu-id="afe48-123">Don't use hello temporary disk (/dev/sdb1) toostore data.</span></span> <span data-ttu-id="afe48-124">Éppen csak átmeneti tárolására.</span><span class="sxs-lookup"><span data-stu-id="afe48-124">It is only there for temporary storage.</span></span> <span data-ttu-id="afe48-125">Veszhetnek el, amelyek nem állíthatók vissza adatokat.</span><span class="sxs-lookup"><span data-stu-id="afe48-125">You risk losing data that can’t be recovered.</span></span>

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a><span data-ttu-id="afe48-126">Másolja vagy egy meglévő Azure virtuális gép klónozása?</span><span class="sxs-lookup"><span data-stu-id="afe48-126">Can I copy or clone an existing Azure VM?</span></span>
<span data-ttu-id="afe48-127">Igen.</span><span class="sxs-lookup"><span data-stu-id="afe48-127">Yes.</span></span> <span data-ttu-id="afe48-128">Útmutatásért lásd: [hogyan egy-egy példányát a Linux virtuális gépek toocreate hello Resource Manager üzembe helyezési modellben](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="afe48-128">For instructions, see [How toocreate a copy of a Linux virtual machine in hello Resource Manager deployment model](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a><span data-ttu-id="afe48-129">Miért nem jelenik meg Kanada központi és Kanada keleti régiók Azure Resource Manageren keresztül?</span><span class="sxs-lookup"><span data-stu-id="afe48-129">Why am I not seeing Canada Central and Canada East regions through Azure Resource Manager?</span></span>
<span data-ttu-id="afe48-130">hello két új régió Kanada központi és Kanada keleti nem automatikusan regisztrálva van a virtuális gép létrehozásához a meglévő Azure-előfizetések.</span><span class="sxs-lookup"><span data-stu-id="afe48-130">hello two new regions of Canada Central and Canada East are not automatically registered for virtual machine creation for existing Azure subscriptions.</span></span> <span data-ttu-id="afe48-131">Ez a regisztráció automatikusan történik, amikor egy virtuális gép telepítve van a hello Azure portál tooany más régióban, Azure Resource Manager használatával.</span><span class="sxs-lookup"><span data-stu-id="afe48-131">This registration is done automatically when a virtual machine is deployed through hello Azure portal tooany other region using Azure Resource Manager.</span></span> <span data-ttu-id="afe48-132">A virtuális gépek után van telepített tooany más Azure-régió, hello új régiók elérhetőknek kell lenniük a következő virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="afe48-132">After a virtual machine is deployed tooany other Azure region, hello new regions should be available for subsequent virtual machines.</span></span>

## <a name="can-i-add-a-nic-toomy-vm-after-its-created"></a><span data-ttu-id="afe48-133">Vehetek fel egy hálózati adapter toomy virtuális gép létrehozása után?</span><span class="sxs-lookup"><span data-stu-id="afe48-133">Can I add a NIC toomy VM after it's created?</span></span>
<span data-ttu-id="afe48-134">Igen, ez a lehetőség most.</span><span class="sxs-lookup"><span data-stu-id="afe48-134">Yes, this is now possible.</span></span> <span data-ttu-id="afe48-135">hello virtuális gép első igények toobe felszabadított leállt.</span><span class="sxs-lookup"><span data-stu-id="afe48-135">hello VM first needs toobe stopped deallocated.</span></span> <span data-ttu-id="afe48-136">Ezt követően adja hozzá, vagy távolítsa el a hálózati adapter (kivéve, ha utolsó hálózati adapterén hello VM hello).</span><span class="sxs-lookup"><span data-stu-id="afe48-136">Then you can add or remove a NIC (unless it's hello last NIC on hello VM).</span></span> 

## <a name="are-there-any-computer-name-requirements"></a><span data-ttu-id="afe48-137">Vannak-e a számítógép neve követelmények?</span><span class="sxs-lookup"><span data-stu-id="afe48-137">Are there any computer name requirements?</span></span>
<span data-ttu-id="afe48-138">Igen.</span><span class="sxs-lookup"><span data-stu-id="afe48-138">Yes.</span></span> <span data-ttu-id="afe48-139">hello számítógép neve legfeljebb 64 karakter hosszúságú lehet.</span><span class="sxs-lookup"><span data-stu-id="afe48-139">hello computer name can be a maximum of 64 characters in length.</span></span> <span data-ttu-id="afe48-140">Lásd: [elnevezési konvenciókat szabályokat és korlátozásokat](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) további információt az erőforrások elnevezési körül.</span><span class="sxs-lookup"><span data-stu-id="afe48-140">See [Naming conventions rules and restrictions](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for more information around naming your resources.</span></span>

## <a name="are-there-any-resource-group-name-requirements"></a><span data-ttu-id="afe48-141">Vannak-e valamely erőforrás csoport vonatkozó követelményeknek?</span><span class="sxs-lookup"><span data-stu-id="afe48-141">Are there any resource group name requirements?</span></span>
<span data-ttu-id="afe48-142">Igen.</span><span class="sxs-lookup"><span data-stu-id="afe48-142">Yes.</span></span> <span data-ttu-id="afe48-143">hello az erőforráscsoport neve legfeljebb 90 karakter hosszúságú lehet.</span><span class="sxs-lookup"><span data-stu-id="afe48-143">hello resource group name can be a maximum of 90 characters in length.</span></span> <span data-ttu-id="afe48-144">Lásd: [elnevezési konvenciókat szabályokat és korlátozásokat](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) erőforráscsoportok további információt.</span><span class="sxs-lookup"><span data-stu-id="afe48-144">See [Naming conventions rules and restrictions](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for more information about resource groups.</span></span>

## <a name="what-are-hello-username-requirements-when-creating-a-vm"></a><span data-ttu-id="afe48-145">Virtuális gép létrehozásakor Mik hello username követelmények?</span><span class="sxs-lookup"><span data-stu-id="afe48-145">What are hello username requirements when creating a VM?</span></span>
<span data-ttu-id="afe48-146">Felhasználónevek 1 – 64 karakter hosszúságúnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="afe48-146">Usernames must be 1 - 64 characters in length.</span></span>

<span data-ttu-id="afe48-147">a következő felhasználónevek hello nem engedélyezettek:</span><span class="sxs-lookup"><span data-stu-id="afe48-147">hello following usernames are not allowed:</span></span>

<table>
    <tr>
        <td style="text-align:center"><span data-ttu-id="afe48-148">Rendszergazda</span><span class="sxs-lookup"><span data-stu-id="afe48-148">administrator</span></span> </td><td style="text-align:center"> <span data-ttu-id="afe48-149">Rendszergazda</span><span class="sxs-lookup"><span data-stu-id="afe48-149">admin</span></span> </td><td style="text-align:center"> <span data-ttu-id="afe48-150">Felhasználó</span><span class="sxs-lookup"><span data-stu-id="afe48-150">user</span></span> </td><td style="text-align:center"> <span data-ttu-id="afe48-151">Felhasználó1</span><span class="sxs-lookup"><span data-stu-id="afe48-151">user1</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="afe48-152">Teszt</span><span class="sxs-lookup"><span data-stu-id="afe48-152">test</span></span> </td><td style="text-align:center"> <span data-ttu-id="afe48-153">Felhasználó2</span><span class="sxs-lookup"><span data-stu-id="afe48-153">user2</span></span> </td><td style="text-align:center"> <span data-ttu-id="afe48-154">Test1</span><span class="sxs-lookup"><span data-stu-id="afe48-154">test1</span></span> </td><td style="text-align:center"> <span data-ttu-id="afe48-155">Felhasználó3</span><span class="sxs-lookup"><span data-stu-id="afe48-155">user3</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="afe48-156">rendszergazda1</span><span class="sxs-lookup"><span data-stu-id="afe48-156">admin1</span></span> </td><td style="text-align:center"> <span data-ttu-id="afe48-157">1</span><span class="sxs-lookup"><span data-stu-id="afe48-157">1</span></span> </td><td style="text-align:center"> <span data-ttu-id="afe48-158">123</span><span class="sxs-lookup"><span data-stu-id="afe48-158">123</span></span> </td><td style="text-align:center"> <span data-ttu-id="afe48-159">egy</span><span class="sxs-lookup"><span data-stu-id="afe48-159">a</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="afe48-160">actuser</span><span class="sxs-lookup"><span data-stu-id="afe48-160">actuser</span></span>  </td><td style="text-align:center"> <span data-ttu-id="afe48-161">ADM</span><span class="sxs-lookup"><span data-stu-id="afe48-161">adm</span></span> </td><td style="text-align:center"> <span data-ttu-id="afe48-162">admin2</span><span class="sxs-lookup"><span data-stu-id="afe48-162">admin2</span></span> </td><td style="text-align:center"> <span data-ttu-id="afe48-163">ASPNET</span><span class="sxs-lookup"><span data-stu-id="afe48-163">aspnet</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="afe48-164">biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="afe48-164">backup</span></span> </td><td style="text-align:center"> <span data-ttu-id="afe48-165">Konzol</span><span class="sxs-lookup"><span data-stu-id="afe48-165">console</span></span> </td><td style="text-align:center"> <span data-ttu-id="afe48-166">David</span><span class="sxs-lookup"><span data-stu-id="afe48-166">david</span></span> </td><td style="text-align:center"> <span data-ttu-id="afe48-167">Vendég</span><span class="sxs-lookup"><span data-stu-id="afe48-167">guest</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="afe48-168">John</span><span class="sxs-lookup"><span data-stu-id="afe48-168">john</span></span> </td><td style="text-align:center"> <span data-ttu-id="afe48-169">Tulajdonos</span><span class="sxs-lookup"><span data-stu-id="afe48-169">owner</span></span> </td><td style="text-align:center"> <span data-ttu-id="afe48-170">legfelső szintű</span><span class="sxs-lookup"><span data-stu-id="afe48-170">root</span></span> </td><td style="text-align:center"> <span data-ttu-id="afe48-171">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="afe48-171">server</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="afe48-172">SQL</span><span class="sxs-lookup"><span data-stu-id="afe48-172">sql</span></span> </td><td style="text-align:center"> <span data-ttu-id="afe48-173">Támogatás</span><span class="sxs-lookup"><span data-stu-id="afe48-173">support</span></span> </td><td style="text-align:center"> <span data-ttu-id="afe48-174">support_388945a0</span><span class="sxs-lookup"><span data-stu-id="afe48-174">support_388945a0</span></span> </td><td style="text-align:center"> <span data-ttu-id="afe48-175">sys</span><span class="sxs-lookup"><span data-stu-id="afe48-175">sys</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="afe48-176">Teszt2</span><span class="sxs-lookup"><span data-stu-id="afe48-176">test2</span></span> </td><td style="text-align:center"> <span data-ttu-id="afe48-177">Teszt3</span><span class="sxs-lookup"><span data-stu-id="afe48-177">test3</span></span> </td><td style="text-align:center"> <span data-ttu-id="afe48-178">Felhasználó4</span><span class="sxs-lookup"><span data-stu-id="afe48-178">user4</span></span> </td><td style="text-align:center"> <span data-ttu-id="afe48-179">user5</span><span class="sxs-lookup"><span data-stu-id="afe48-179">user5</span></span></td>
    </tr>
</table>


## <a name="what-are-hello-password-requirements-when-creating-a-vm"></a><span data-ttu-id="afe48-180">Virtuális gép létrehozásakor Mik hello jelszavakra vonatkozó követelmények?</span><span class="sxs-lookup"><span data-stu-id="afe48-180">What are hello password requirements when creating a VM?</span></span>
<span data-ttu-id="afe48-181">Jelszavak 6 – 72 karakter hosszúságúnak kell és felel meg a 3, 4 összetettségi követelményeknek hello kívül:</span><span class="sxs-lookup"><span data-stu-id="afe48-181">Passwords must be 6 - 72 characters in length and meet 3 out of hello following 4 complexity requirements:</span></span>

* <span data-ttu-id="afe48-182">Alacsonyabb karaktereket</span><span class="sxs-lookup"><span data-stu-id="afe48-182">Have lower characters</span></span>
* <span data-ttu-id="afe48-183">Felső karaktereket</span><span class="sxs-lookup"><span data-stu-id="afe48-183">Have upper characters</span></span>
* <span data-ttu-id="afe48-184">Rendelkezik egy számjegy</span><span class="sxs-lookup"><span data-stu-id="afe48-184">Have a digit</span></span>
* <span data-ttu-id="afe48-185">Rendelkezik egy speciális karaktert (reguláris kifejezéssel egyező [\W_])</span><span class="sxs-lookup"><span data-stu-id="afe48-185">Have a special character (Regex match [\W_])</span></span>

<span data-ttu-id="afe48-186">a következő jelszavak hello nem engedélyezettek:</span><span class="sxs-lookup"><span data-stu-id="afe48-186">hello following passwords are not allowed:</span></span>

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center"><span data-ttu-id="afe48-187">P@$$w0rd</span><span class="sxs-lookup"><span data-stu-id="afe48-187">P@$$w0rd</span></span></td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center"><span data-ttu-id="afe48-188">Pa$ $word</span><span class="sxs-lookup"><span data-stu-id="afe48-188">Pa$$word</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center"><span data-ttu-id="afe48-189">Jelszót!</span><span class="sxs-lookup"><span data-stu-id="afe48-189">Password!</span></span></td>
        <td style="text-align:center"><span data-ttu-id="afe48-190">Jelszó1</span><span class="sxs-lookup"><span data-stu-id="afe48-190">Password1</span></span></td>
        <td style="text-align:center"><span data-ttu-id="afe48-191">Password22</span><span class="sxs-lookup"><span data-stu-id="afe48-191">Password22</span></span></td>
        <td style="text-align:center"><span data-ttu-id="afe48-192">ILOVEYOU!</span><span class="sxs-lookup"><span data-stu-id="afe48-192">iloveyou!</span></span></td>
    </tr>
</table>
