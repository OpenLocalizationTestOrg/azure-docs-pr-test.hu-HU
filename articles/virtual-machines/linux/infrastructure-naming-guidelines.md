---
title: "irányelvek - Linux elnevezési aaaAzure infrastruktúra |} Microsoft Docs"
description: "Ismerje meg hello legfontosabb tervezési és megvalósítási irányelvek elnevezéséhez az Azure infrastruktúra-szolgáltatásokat."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 18ee4fb1-8297-49a1-8d3f-097880be67c7
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 333146e7b2071e43527a5d7dc2ec02ebfb316eb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-infrastructure-naming-guidelines-for-linux-vms"></a><span data-ttu-id="32525-103">Linux virtuális gépek elnevezési irányelvek Azure-infrastruktúra</span><span class="sxs-lookup"><span data-stu-id="32525-103">Azure infrastructure naming guidelines for Linux VMs</span></span> 

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

<span data-ttu-id="32525-104">Ez a cikk foglalkozik a megértése, hogyan tooapproach elnevezési szabályai minden a különböző Azure-erőforrások toobuild a logikai és könnyen azonosítható állíthatja az erőforrások e a környezetben.</span><span class="sxs-lookup"><span data-stu-id="32525-104">This article focuses on understanding how tooapproach naming conventions for all your various Azure resources toobuild a logical and easily identifiable set of resources across your environment.</span></span>

## <a name="implementation-guidelines-for-naming-conventions"></a><span data-ttu-id="32525-105">Implementációs segédlet az elnevezési konvenciók</span><span class="sxs-lookup"><span data-stu-id="32525-105">Implementation guidelines for naming conventions</span></span>
<span data-ttu-id="32525-106">Döntéseket:</span><span class="sxs-lookup"><span data-stu-id="32525-106">Decisions:</span></span>

* <span data-ttu-id="32525-107">Mik az Azure-erőforrások elnevezési szabályai?</span><span class="sxs-lookup"><span data-stu-id="32525-107">What are your naming conventions for Azure resources?</span></span>

<span data-ttu-id="32525-108">Feladatok:</span><span class="sxs-lookup"><span data-stu-id="32525-108">Tasks:</span></span>

* <span data-ttu-id="32525-109">Adja meg a hello elő-/ utótagok toouse az erőforrások toomaintain konzisztencia között.</span><span class="sxs-lookup"><span data-stu-id="32525-109">Define hello affixes toouse across your resources toomaintain consistency.</span></span>
* <span data-ttu-id="32525-110">Adja meg a hello számukra követelmény toobe globálisan egyedi nevek a megadott tárfiók.</span><span class="sxs-lookup"><span data-stu-id="32525-110">Define storage account names given hello requirement for them toobe globally unique.</span></span>
* <span data-ttu-id="32525-111">A dokumentum hello elnevezési egyezmény toobe használt, és ossza ki tooall Felek érintett tooensure konzisztencia központi telepítések egységességét.</span><span class="sxs-lookup"><span data-stu-id="32525-111">Document hello naming convention toobe used and distribute tooall parties involved tooensure consistency across deployments.</span></span>

## <a name="naming-conventions"></a><span data-ttu-id="32525-112">Elnevezési konvenciók</span><span class="sxs-lookup"><span data-stu-id="32525-112">Naming conventions</span></span>
<span data-ttu-id="32525-113">Helyen jó elnevezési semmit az Azure-ban létrehozása előtt kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="32525-113">You should have a good naming convention in place before creating anything in Azure.</span></span> <span data-ttu-id="32525-114">Elnevezési biztosítja, hogy minden hello erőforrások egy előre jelezhető nevét, amely segít a alacsonyabb hello felügyeleti terheket társított erőforrások kezelése.</span><span class="sxs-lookup"><span data-stu-id="32525-114">A naming convention ensures that all hello resources have a predictable name, which helps lower hello administrative burden associated with managing those resources.</span></span>

<span data-ttu-id="32525-115">Akkor érdemes választania toofollow elnevezési konvenciókat meghatározása a teljes szervezet számára, vagy egy adott Azure-előfizetés vagy a fiók egy adott készletét.</span><span class="sxs-lookup"><span data-stu-id="32525-115">You might choose toofollow a specific set of naming conventions defined for your entire organization or for a specific Azure subscription or account.</span></span> <span data-ttu-id="32525-116">Bár a szervezetek tooestablish implicit szabályok egyéni felhasználók számára könnyen az Azure-erőforrások használatakor, toobe képes tooscale szüksége csoportjai működik együtt az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="32525-116">Although it is easy for individuals within organizations tooestablish implicit rules when working with Azure resources, you need toobe able tooscale for teams working together in Azure.</span></span>

<span data-ttu-id="32525-117">Az elnevezési egyezmény előre egyezniük.</span><span class="sxs-lookup"><span data-stu-id="32525-117">Agree on a set of naming conventions up front.</span></span> <span data-ttu-id="32525-118">Nincsenek szabályok elnevezési konvenciókat, amelyek között Kivágás megadó kapcsolatos szempontokat.</span><span class="sxs-lookup"><span data-stu-id="32525-118">There are some considerations regarding naming conventions that cut across that sets of rules.</span></span>

## <a name="affixes"></a><span data-ttu-id="32525-119">Kell</span><span class="sxs-lookup"><span data-stu-id="32525-119">Affixes</span></span>
<span data-ttu-id="32525-120">Toodefine elnevezési tekinti meg, mert egy döntési az e hello utótag:</span><span class="sxs-lookup"><span data-stu-id="32525-120">As you look toodefine a naming convention, one decision is whether hello affix is at:</span></span>

* <span data-ttu-id="32525-121">hello elejére hello neve (előtag)</span><span class="sxs-lookup"><span data-stu-id="32525-121">hello beginning of hello name (prefix)</span></span>
* <span data-ttu-id="32525-122">hello végét hello neve (utótag)</span><span class="sxs-lookup"><span data-stu-id="32525-122">hello end of hello name (suffix)</span></span>

<span data-ttu-id="32525-123">Például az alábbiakban a két lehetséges nevek használatával hello erőforráscsoport `rg` elhelyezi:</span><span class="sxs-lookup"><span data-stu-id="32525-123">For instance, here are two possible names for a Resource Group using hello `rg` affix:</span></span>

* <span data-ttu-id="32525-124">Rg-webalkalmazást (előtag)</span><span class="sxs-lookup"><span data-stu-id="32525-124">Rg-WebApp (prefix)</span></span>
* <span data-ttu-id="32525-125">Webalkalmazás-Rg (utótag)</span><span class="sxs-lookup"><span data-stu-id="32525-125">WebApp-Rg (suffix)</span></span>

<span data-ttu-id="32525-126">Elő-/ utótagok hivatkozhat toodifferent szempontjait ismertető hello adott erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="32525-126">Affixes can refer toodifferent aspects that describe hello particular resources.</span></span> <span data-ttu-id="32525-127">hello következő táblázatban néhány példa látható általában akkor használható.</span><span class="sxs-lookup"><span data-stu-id="32525-127">hello following table shows some examples typically used.</span></span>

| <span data-ttu-id="32525-128">Aspektusa</span><span class="sxs-lookup"><span data-stu-id="32525-128">Aspect</span></span> | <span data-ttu-id="32525-129">Példák</span><span class="sxs-lookup"><span data-stu-id="32525-129">Examples</span></span> | <span data-ttu-id="32525-130">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="32525-130">Notes</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="32525-131">Környezet</span><span class="sxs-lookup"><span data-stu-id="32525-131">Environment</span></span> |<span data-ttu-id="32525-132">fejlesztői, stg, termék</span><span class="sxs-lookup"><span data-stu-id="32525-132">dev, stg, prod</span></span> |<span data-ttu-id="32525-133">Attól függően, hogy hello célját és minden környezet nevét.</span><span class="sxs-lookup"><span data-stu-id="32525-133">Depending on hello purpose and name of each environment.</span></span> |
| <span data-ttu-id="32525-134">Hely</span><span class="sxs-lookup"><span data-stu-id="32525-134">Location</span></span> |<span data-ttu-id="32525-135">usw (USA nyugati régiója), használjon (USA keleti régiója 2)</span><span class="sxs-lookup"><span data-stu-id="32525-135">usw (West US), use (East US 2)</span></span> |<span data-ttu-id="32525-136">Attól függően, hogy hello régió hello datacenter vagy hello terület hello szervezet.</span><span class="sxs-lookup"><span data-stu-id="32525-136">Depending on hello region of hello datacenter or hello region of hello organization.</span></span> |
| <span data-ttu-id="32525-137">Az Azure összetevő, szolgáltatás vagy termék</span><span class="sxs-lookup"><span data-stu-id="32525-137">Azure component, service, or product</span></span> |<span data-ttu-id="32525-138">Az erőforráscsoport, a virtuális hálózat virtuális hálózat rg</span><span class="sxs-lookup"><span data-stu-id="32525-138">Rg for resource group, VNet for virtual network</span></span> |<span data-ttu-id="32525-139">Attól függően, hogy mely hello erőforrás biztosít hello termék támogatja.</span><span class="sxs-lookup"><span data-stu-id="32525-139">Depending on hello product for which hello resource provides support.</span></span> |
| <span data-ttu-id="32525-140">Szerepkör</span><span class="sxs-lookup"><span data-stu-id="32525-140">Role</span></span> |<span data-ttu-id="32525-141">DB, alkalmazást, webes</span><span class="sxs-lookup"><span data-stu-id="32525-141">db, app, web</span></span> |<span data-ttu-id="32525-142">Attól függően, hogy hello szerepkör hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="32525-142">Depending on hello role of hello virtual machine.</span></span> |
| <span data-ttu-id="32525-143">Példány</span><span class="sxs-lookup"><span data-stu-id="32525-143">Instance</span></span> |<span data-ttu-id="32525-144">01, 02, 03, stb.</span><span class="sxs-lookup"><span data-stu-id="32525-144">01, 02, 03, etc.</span></span> |<span data-ttu-id="32525-145">Egynél több példánnyal rendelkező erőforrások.</span><span class="sxs-lookup"><span data-stu-id="32525-145">For resources that have more than one instance.</span></span> <span data-ttu-id="32525-146">Például az elosztott terhelésű webkiszolgálók felhőszolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="32525-146">For example, load balanced web servers in a cloud service.</span></span> |

<span data-ttu-id="32525-147">Amikor az elnevezési konvenciókat létrehozó, győződjön meg arról, hogy azok egyértelműen megállapítják amely kell toouse az egyes erőforrás, és melyik helyen (előtag vs utótag).</span><span class="sxs-lookup"><span data-stu-id="32525-147">When establishing your naming conventions, make sure that they clearly state which affixes toouse for each type of resource, and in which position (prefix vs suffix).</span></span>

## <a name="dates"></a><span data-ttu-id="32525-148">dátum</span><span class="sxs-lookup"><span data-stu-id="32525-148">Dates</span></span>
<span data-ttu-id="32525-149">Ez a legtöbbször fontos toodetermine hello erőforrás neve alapján hello létrehozásának dátuma.</span><span class="sxs-lookup"><span data-stu-id="32525-149">It is often important toodetermine hello date of creation from hello name of a resource.</span></span> <span data-ttu-id="32525-150">Azt javasoljuk, hogy hello ÉÉÉÉHHNN dátumformátum.</span><span class="sxs-lookup"><span data-stu-id="32525-150">We recommend hello YYYYMMDD date format.</span></span> <span data-ttu-id="32525-151">Ezt a formátumot biztosítja, hogy teljes dátum rögzítése, de az is, hogy két erőforrás mappanevek csak hello dátumon eltérőek rendezi a rendszer betűrendben és időrendben hello csak nem.</span><span class="sxs-lookup"><span data-stu-id="32525-151">This format ensures that not only is hello full date is recorded, but also that two resources whose names differ only on hello date are sorted alphabetically and chronologically.</span></span>

## <a name="naming-resources"></a><span data-ttu-id="32525-152">Elnevezési erőforrások</span><span class="sxs-lookup"><span data-stu-id="32525-152">Naming resources</span></span>
<span data-ttu-id="32525-153">Adjon meg minden típusú erőforrás hello elnevezési konvenciót, amely kell rendelkeznie a szabályokat, amelyek meghatározzák, hogyan tooassign nevek tooeach erőforrás, amely jön létre.</span><span class="sxs-lookup"><span data-stu-id="32525-153">Define each type of resource in hello naming convention, which should have rules that define how tooassign names tooeach resource that is created.</span></span> <span data-ttu-id="32525-154">Ezek a szabályok vonatkozzon tooall típusú erőforrások, például:</span><span class="sxs-lookup"><span data-stu-id="32525-154">These rules should apply tooall types of resources, for example:</span></span>

* <span data-ttu-id="32525-155">Előfizetések</span><span class="sxs-lookup"><span data-stu-id="32525-155">Subscriptions</span></span>
* <span data-ttu-id="32525-156">Fiókok</span><span class="sxs-lookup"><span data-stu-id="32525-156">Accounts</span></span>
* <span data-ttu-id="32525-157">Tárfiókok</span><span class="sxs-lookup"><span data-stu-id="32525-157">Storage accounts</span></span>
* <span data-ttu-id="32525-158">Virtuális hálózatok</span><span class="sxs-lookup"><span data-stu-id="32525-158">Virtual networks</span></span>
* <span data-ttu-id="32525-159">Alhálózatok</span><span class="sxs-lookup"><span data-stu-id="32525-159">Subnets</span></span>
* <span data-ttu-id="32525-160">Rendelkezésre állási csoportok</span><span class="sxs-lookup"><span data-stu-id="32525-160">Availability sets</span></span>
* <span data-ttu-id="32525-161">Erőforráscsoportok</span><span class="sxs-lookup"><span data-stu-id="32525-161">Resource groups</span></span>
* <span data-ttu-id="32525-162">Virtual machines (Virtuális gépek)</span><span class="sxs-lookup"><span data-stu-id="32525-162">Virtual machines</span></span>
* <span data-ttu-id="32525-163">Végpontok</span><span class="sxs-lookup"><span data-stu-id="32525-163">Endpoints</span></span>
* <span data-ttu-id="32525-164">Network security groups (Hálózati biztonsági csoportok)</span><span class="sxs-lookup"><span data-stu-id="32525-164">Network security groups</span></span>
* <span data-ttu-id="32525-165">Szerepkörök</span><span class="sxs-lookup"><span data-stu-id="32525-165">Roles</span></span>

<span data-ttu-id="32525-166">tooensure, amelyek neve hello tartalmaz elég információt toodetermine toowhich erőforrás hivatkozik, leíró neveket kell használnia.</span><span class="sxs-lookup"><span data-stu-id="32525-166">tooensure that hello name provides enough information toodetermine toowhich resource it refers, you should use descriptive names.</span></span>

## <a name="computer-names"></a><span data-ttu-id="32525-167">Számítógép neve</span><span class="sxs-lookup"><span data-stu-id="32525-167">Computer names</span></span>
<span data-ttu-id="32525-168">Amikor létrehoz egy virtuális gép (VM), az Azure-nak nevet a virtuális Géphez a mentést too64 karakterek hello erőforrásnév használt.</span><span class="sxs-lookup"><span data-stu-id="32525-168">When you create a virtual machine (VM), Azure requires a VM name of up too64 characters that is used for hello resource name.</span></span> <span data-ttu-id="32525-169">Az Azure által használt hello hello operációs rendszer van telepítve, a virtuális gép hello ugyanazt a nevet.</span><span class="sxs-lookup"><span data-stu-id="32525-169">Azure uses hello same name for hello operating system installed in hello VM.</span></span> <span data-ttu-id="32525-170">Azonban ezeket a neveket előfordulhat, hogy nem minden esetben kell hello azonos.</span><span class="sxs-lookup"><span data-stu-id="32525-170">However, these names might not always be hello same.</span></span>

<span data-ttu-id="32525-171">Ha egy virtuális Gépet, amely már tartalmazza az operációs rendszer .vhd kép fájlból jön létre, hello VM nevét az Azure-ban eltérhet hello a virtuális gép operációs rendszerének számítógép nevét.</span><span class="sxs-lookup"><span data-stu-id="32525-171">If a VM is created from a .vhd image file that already contains an operating system, hello VM name in Azure can differ from hello VM's operating system computer name.</span></span> <span data-ttu-id="32525-172">Ebben az esetben is hozzáadhat egy bizonyos fokú nehezen tooVM felügyeletig, ezért nem javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="32525-172">This situation can add a degree of difficulty tooVM management, which we therefore do not recommend.</span></span> <span data-ttu-id="32525-173">Rendelje hozzá a hello Azure VM erőforrás hello azonos hello számítógép neve, hogy rendelje toohello operációs rendszer, hogy a virtuális gép neve.</span><span class="sxs-lookup"><span data-stu-id="32525-173">Assign hello Azure VM resource hello same name as hello computer name that you assign toohello operating system of that VM.</span></span>

<span data-ttu-id="32525-174">Azt javasoljuk, hogy hello Azure virtuális gép neve ugyanaz, mint az alapul szolgáló operációs rendszer számítógépnév hello van hello.</span><span class="sxs-lookup"><span data-stu-id="32525-174">We recommend that hello Azure VM name is hello same as hello underlying operating system computer name.</span></span>

## <a name="storage-account-names"></a><span data-ttu-id="32525-175">Tárfiókok neve</span><span class="sxs-lookup"><span data-stu-id="32525-175">Storage account names</span></span>
<span data-ttu-id="32525-176">Ez a szakasz nem érvényes túl[Azure felügyelt lemezek](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), hogy ne hozzon létre egy külön tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="32525-176">This section does not apply too[Azure Managed Disks](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), as you do not create a separate storage account.</span></span> <span data-ttu-id="32525-177">Nem felügyelt lemezek storage-fiókok különleges szabályokat nevük rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="32525-177">For unmanaged disks, storage accounts have special rules governing their names.</span></span> <span data-ttu-id="32525-178">Csak kisbetűket és számokat használható.</span><span class="sxs-lookup"><span data-stu-id="32525-178">You can only use lowercase letters and numbers.</span></span> <span data-ttu-id="32525-179">További információkért lásd: [hozzon létre egy tárfiókot](../../storage/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="32525-179">For more information, see [Create a storage account](../../storage/storage-create-storage-account.md#create-a-storage-account).</span></span> <span data-ttu-id="32525-180">Tárfiók hello nevét, a core.windows.net, továbbá egy globálisan érvényes, egyedi DNS-nevet kell lennie.</span><span class="sxs-lookup"><span data-stu-id="32525-180">Additionally, hello storage account name, with core.windows.net, should be a globally valid, unique DNS name.</span></span> <span data-ttu-id="32525-181">Például ha hello tárfiók neve mystorageaccount, hello következő eredményül kapott DNS-nevek egyedinek kell lennie:</span><span class="sxs-lookup"><span data-stu-id="32525-181">For instance, if hello storage account is called mystorageaccount, hello following resulting DNS names should be unique:</span></span>

* <span data-ttu-id="32525-182">mystorageaccount.BLOB.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="32525-182">mystorageaccount.blob.core.windows.net</span></span>
* <span data-ttu-id="32525-183">mystorageaccount.TABLE.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="32525-183">mystorageaccount.table.core.windows.net</span></span>
* <span data-ttu-id="32525-184">mystorageaccount.Queue.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="32525-184">mystorageaccount.queue.core.windows.net</span></span>

## <a name="next-steps"></a><span data-ttu-id="32525-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="32525-185">Next steps</span></span>
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

