---
title: "idő a virtuális gépen aaaJust elérni az Azure Security Centerben |} Microsoft Docs"
description: "A dokumentumból megtudhatja, hogyan igény szerint VM hozzáférés az Azure Security Center segítségével szabályozhatja a hozzáférést tooyour Azure virtuális gépek."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: terrylan
ms.openlocfilehash: e6b58dd2c686cb227392b294e85914df5a546016
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-virtual-machine-access-using-just-in-time"></a><span data-ttu-id="14276-103">JIT-virtuális gép hozzáférés kezelése</span><span class="sxs-lookup"><span data-stu-id="14276-103">Manage virtual machine access using just in time</span></span>

<span data-ttu-id="14276-104">Csak az idő a virtuális gép (VM) hozzáférést lehet használt toolock bejövő forgalom tooyour Azure virtuális gépeken, csökkenti a kitettség tooattacks egyszerű a hozzáférés tooconnect tooVMs szükség esetén adja meg.</span><span class="sxs-lookup"><span data-stu-id="14276-104">Just in time virtual machine (VM) access can be used toolock down inbound traffic tooyour Azure VMs, reducing exposure tooattacks while providing easy access tooconnect tooVMs when needed.</span></span>

> [!NOTE]
> <span data-ttu-id="14276-105">hello csak az idő a szolgáltatás jelenleg előzetes és hello a Security Center Standard csomagban érhető el.</span><span class="sxs-lookup"><span data-stu-id="14276-105">hello just in time feature is in preview and available on hello Standard tier of Security Center.</span></span>  <span data-ttu-id="14276-106">Lásd: [árazás](security-center-pricing.md) toolearn bővebben a Security Center által tarifacsomag szükséges.</span><span class="sxs-lookup"><span data-stu-id="14276-106">See [Pricing](security-center-pricing.md) toolearn more about Security Center's pricing tiers.</span></span>
>
>

## <a name="attack-scenario"></a><span data-ttu-id="14276-107">Támadás</span><span class="sxs-lookup"><span data-stu-id="14276-107">Attack scenario</span></span>

<span data-ttu-id="14276-108">Találgatásos támadások gyakran felügyeleti célportjainak, azt jelenti, hogy toogain hozzáférés tooa virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="14276-108">Brute force attacks commonly target management ports as a means toogain access tooa VM.</span></span> <span data-ttu-id="14276-109">Ha sikeres, a támadó ellenőrzése alatt tartja a virtuális gép hello igénybe, és egy foothold létesíteni a környezetbe.</span><span class="sxs-lookup"><span data-stu-id="14276-109">If successful, an attacker can take control over hello VM and establish a foothold into your environment.</span></span>

<span data-ttu-id="14276-110">Egyirányú tooreduce kitettség tooa találgatásos támadás toolimit hello, hogy mennyi ideig legyen egy nyitott port.</span><span class="sxs-lookup"><span data-stu-id="14276-110">One way tooreduce exposure tooa brute force attack is toolimit hello amount of time that a port is open.</span></span> <span data-ttu-id="14276-111">Szolgáltatásfelügyelet portjai nem szükséges toobe nyílt mindig tegye.</span><span class="sxs-lookup"><span data-stu-id="14276-111">Management ports do not need toobe open at all times.</span></span> <span data-ttu-id="14276-112">Szükségük toobe csak akkor nyitva vannak csatlakoztatott toohello VM, például tooperform felügyeleti vagy karbantartási feladatok.</span><span class="sxs-lookup"><span data-stu-id="14276-112">They only need toobe open while you are connected toohello VM, for example tooperform management or maintenance tasks.</span></span> <span data-ttu-id="14276-113">Amikor a JIT engedélyezve van, a Security Center az [hálózati biztonsági csoport](../virtual-network/virtual-networks-nsg.md) (NSG) szabályok, amelyek korlátozzák a hozzáférési toomanagement portok, így nem tudja megcélozni a támadók.</span><span class="sxs-lookup"><span data-stu-id="14276-113">When just in time is enabled, Security Center uses [Network Security Group](../virtual-network/virtual-networks-nsg.md) (NSG) rules, which restrict access toomanagement ports so they cannot be targeted by attackers.</span></span>

![Csak az idő forgatókönyv][1]

## <a name="how-does-just-in-time-access-work"></a><span data-ttu-id="14276-115">Hogyan csak az idő access működik?</span><span class="sxs-lookup"><span data-stu-id="14276-115">How does just in time access work?</span></span>

<span data-ttu-id="14276-116">Ha JIT engedélyezve van, a Security Center zárolja bejövő forgalom tooyour Azure virtuális gépek egy NSG-szabály létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="14276-116">When just in time is enabled, Security Center locks down inbound traffic tooyour Azure VMs by creating an NSG rule.</span></span> <span data-ttu-id="14276-117">Kiválaszthatja hello hello VM toowhich a bejövő forgalom lesz zárolva.</span><span class="sxs-lookup"><span data-stu-id="14276-117">You select hello ports on hello VM toowhich inbound traffic will be locked down.</span></span> <span data-ttu-id="14276-118">Csak az idő-megoldások hello ezeket a portokat szabályozzák.</span><span class="sxs-lookup"><span data-stu-id="14276-118">These ports are controlled by hello just in time solution.</span></span>

<span data-ttu-id="14276-119">Ha a felhasználó hozzáférési tooa VM kér, a Security Center ellenőrzi hello felhasználó [szerepköralapú hozzáférés-vezérlést (RBAC)](../active-directory/role-based-access-control-configure.md) írási hozzáférést biztosítson az Azure-erőforrás hello engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="14276-119">When a user requests access tooa VM, Security Center checks that hello user has [Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-configure.md) permissions that provide write access for hello Azure resource.</span></span> <span data-ttu-id="14276-120">Ha írási engedélyekkel rendelkeznek, a hello kérelem jóváhagyása és a Security Center automatikusan konfigurálja a hálózati biztonsági csoportokkal (NSG-k) tooallow hello bejövő forgalom toohello kezelőportjai megadott hello időt.</span><span class="sxs-lookup"><span data-stu-id="14276-120">If they have write permissions, hello request is approved and Security Center automatically configures hello Network Security Groups (NSGs) tooallow inbound traffic toohello management ports for hello amount of time you specified.</span></span> <span data-ttu-id="14276-121">Hello időszak lejárta után a Security Center visszaállítja hello NSG-k tootheir korábbi állapotába.</span><span class="sxs-lookup"><span data-stu-id="14276-121">After hello time has expired, Security Center restores hello NSGs tootheir previous states.</span></span>

> [!NOTE]
> <span data-ttu-id="14276-122">Biztonsági központ csak a virtuális gép elérhető jelenleg csak virtuális gépek Azure Resource Manager használatával telepített.</span><span class="sxs-lookup"><span data-stu-id="14276-122">Security Center just in time VM access currently supports only VMs deployed through Azure Resource Manager.</span></span> <span data-ttu-id="14276-123">hello klasszikus és Resource Manager üzembe helyezési modellel kapcsolatos információkért tekintse meg toolearn [Azure Resource Manager és klasszikus üzembe helyezési](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="14276-123">toolearn more about hello classic and Resource Manager deployment models see [Azure Resource Manager vs. classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>
>
>

## <a name="using-just-in-time-access"></a><span data-ttu-id="14276-124">Csak az idő access használatával</span><span class="sxs-lookup"><span data-stu-id="14276-124">Using just in time access</span></span>

<span data-ttu-id="14276-125">Hello **közvetlenül a virtuális gép elérhető** hello csempét **Security Center** panel virtuális gépek csak az idő hozzáférési és hello engedélyezett hozzáférési kérelmek száma a hello múlt héten konfigurálva hello számát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="14276-125">hello **Just in time VM access** tile on hello **Security Center** blade shows hello number of VMs configured for just in time access and hello number of approved access requests made in hello last week.</span></span>

<span data-ttu-id="14276-126">Jelölje be hello **közvetlenül a virtuális gép elérhető** csempe és hello **közvetlenül a virtuális gép elérhető** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="14276-126">Select hello **Just in time VM access** tile and hello **Just in time VM access** blade opens.</span></span>

![Virtuális gép hozzáférés JIT csempéje][2]

<span data-ttu-id="14276-128">Hello **közvetlenül a virtuális gép elérhető** panel információt nyújt a virtuális gépek hello állapotát:</span><span class="sxs-lookup"><span data-stu-id="14276-128">hello **Just in time VM access** blade provides information on hello state of your VMs:</span></span>

- <span data-ttu-id="14276-129">**Konfigurált** -virtuális gépek, amelyek csak a virtuális gép elérhető konfigurált toosupport lettek.</span><span class="sxs-lookup"><span data-stu-id="14276-129">**Configured** - VMs that have been configured toosupport just in time VM access.</span></span> <span data-ttu-id="14276-130">hello adatainak hello múlt héten és az egyes virtuális gép hello száma jóváhagyott kéréseket, legutóbbi hozzáférés dátuma és időpontja és utolsó felhasználó tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="14276-130">hello data presented is for hello last week and includes for each VM hello number of approved requests, last access date and time, and last user.</span></span>
- <span data-ttu-id="14276-131">**Ajánlott** -csak a virtuális gép elérhető támogatható, de nincs beállítva a virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="14276-131">**Recommended** - VMs that can support just in time VM access but have not been configured to.</span></span> <span data-ttu-id="14276-132">Javasoljuk, hogy engedélyezze a csak az idő VM hozzáférés-vezérlés a virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="14276-132">We recommend that you enable just in time VM access control for these VMs.</span></span> <span data-ttu-id="14276-133">Lásd: [engedélyezése csak a virtuális gép elérhető](#enable-just-in-time-vm-access).</span><span class="sxs-lookup"><span data-stu-id="14276-133">See [Enable just in time VM access](#enable-just-in-time-vm-access).</span></span>
- <span data-ttu-id="14276-134">**Nincs javaslat** -egy virtuális gép nem ajánlott toobe okozó okai:</span><span class="sxs-lookup"><span data-stu-id="14276-134">**No recommendation** - Reasons that can cause a VM not toobe recommended are:</span></span>
  - <span data-ttu-id="14276-135">Hiányzik az NSG - hello csak az idő-megoldások szükséges egy NSG toobe helyen.</span><span class="sxs-lookup"><span data-stu-id="14276-135">Missing NSG - hello just in time solution requires an NSG toobe in place.</span></span>
  - <span data-ttu-id="14276-136">Klasszikus VM - VM elérhető csak a Security Center jelenleg csak virtuális gépek Azure Resource Manager használatával telepített.</span><span class="sxs-lookup"><span data-stu-id="14276-136">Classic VM - Security Center just in time VM access currently supports only VMs deployed through Azure Resource Manager.</span></span> <span data-ttu-id="14276-137">A klasszikus üzembe helyezési hello csak az idő-megoldás által nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="14276-137">A classic deployment is not supported by hello just in time solution.</span></span>
  - <span data-ttu-id="14276-138">Egyéb - egy virtuális gép esetén ebbe a kategóriába tartozó hello biztonsági házirendje hello előfizetéshez vagy erőforráscsoporthoz hello ki van kapcsolva a megoldás JIT hello, vagy adott VM hello hiányzik egy nyilvános IP-cím, és nem rendelkezik egy NSG.</span><span class="sxs-lookup"><span data-stu-id="14276-138">Other - A VM is in this category if hello just in time solution is turned off in hello security policy of hello subscription or hello resource group, or that hello VM is missing a public IP and doesn't have an NSG in place.</span></span>

## <a name="configuring-a-just-in-time-access-policy"></a><span data-ttu-id="14276-139">Beállítása csak hozzáférési házirendben idő</span><span class="sxs-lookup"><span data-stu-id="14276-139">Configuring a just in time access policy</span></span>

<span data-ttu-id="14276-140">tooselect hello tooenable kívánt virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="14276-140">tooselect hello VMs that you want tooenable:</span></span>

1. <span data-ttu-id="14276-141">A hello **közvetlenül a virtuális gép elérhető** panelen, jelölje be hello **ajánlott** fülre.</span><span class="sxs-lookup"><span data-stu-id="14276-141">On hello **Just in time VM access** blade, select hello **Recommended** tab.</span></span>

  ![Csak az idő hozzáférés engedélyezése][3]

2. <span data-ttu-id="14276-143">A **virtuális gépek**, válassza ki a megjeleníteni kívánt tooenable hello virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="14276-143">Under **VMs**, select hello VMs that you want tooenable.</span></span> <span data-ttu-id="14276-144">A pipa következő tooa VM ez helyezi.</span><span class="sxs-lookup"><span data-stu-id="14276-144">This puts a checkmark next tooa VM.</span></span>
3. <span data-ttu-id="14276-145">Válassza ki **engedélyezése virtuális gépeken JIT**.</span><span class="sxs-lookup"><span data-stu-id="14276-145">Select **Enable JIT on VMs**.</span></span>
4. <span data-ttu-id="14276-146">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="14276-146">Select **Save**.</span></span>

### <a name="default-ports"></a><span data-ttu-id="14276-147">Alapértelmezett port</span><span class="sxs-lookup"><span data-stu-id="14276-147">Default ports</span></span>

<span data-ttu-id="14276-148">A Security Center JIT engedélyezését javasolja hello alapértelmezett portokat tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="14276-148">You can see hello default ports that Security Center recommends enabling just in time.</span></span>

1. <span data-ttu-id="14276-149">A hello **közvetlenül a virtuális gép elérhető** panelen, jelölje be hello **ajánlott** fülre.</span><span class="sxs-lookup"><span data-stu-id="14276-149">On hello **Just in time VM access** blade, select hello **Recommended** tab.</span></span>

  ![Megjeleníti az alapértelmezett portok][6]

2. <span data-ttu-id="14276-151">A **virtuális gépek**, jelöljön ki egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="14276-151">Under **VMs**, select a VM.</span></span> <span data-ttu-id="14276-152">Ez egy pipa következő toohello virtuális Gépet, és megnyílik hello helyezi **JIT VM konfiguráció** panelen.</span><span class="sxs-lookup"><span data-stu-id="14276-152">This puts a checkmark next toohello VM and opens hello **JIT VM access configuration** blade.</span></span> <span data-ttu-id="14276-153">Ezen a panelen hello alapértelmezett portok jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="14276-153">This blade displays hello default ports.</span></span>

### <a name="add-ports"></a><span data-ttu-id="14276-154">Portok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="14276-154">Add ports</span></span>

<span data-ttu-id="14276-155">A hello **JIT VM konfiguráció** panelen is hozzáadhat és kívánja tooenable hello csak az idő-megoldások új port konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="14276-155">From hello **JIT VM access configuration** blade, you can also add and configure a new port on which you want tooenable hello just in time solution.</span></span>

1. <span data-ttu-id="14276-156">A hello **JIT VM konfiguráció** panelen válassza **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="14276-156">On hello **JIT VM access configuration** blade, select **Add**.</span></span> <span data-ttu-id="14276-157">Ekkor megnyílik a hello **Hozzáadás portkonfigurációjának** panelen.</span><span class="sxs-lookup"><span data-stu-id="14276-157">This opens hello **Add port configuration** blade.</span></span>

  ![Port konfigurálása][7]

2. <span data-ttu-id="14276-159">A **Hozzáadás portkonfigurációjának** panelen azonosíthatja a kérelem ideje forrás IP-címek és maximális megengedett hello port protokolltípus,.</span><span class="sxs-lookup"><span data-stu-id="14276-159">On **Add port configuration** blade, you identify hello port, protocol type, allowed source IPs, and maximum request time.</span></span>

  <span data-ttu-id="14276-160">Forrás IP-címek engedélyezett vannak hello IP-címtartományok engedélyezett tooget elérést egy jóváhagyott kérésre.</span><span class="sxs-lookup"><span data-stu-id="14276-160">Allowed source IPs are hello IP ranges allowed tooget access upon an approved request.</span></span>

  <span data-ttu-id="14276-161">Kérések maximális ideje hello maximális időszak, hogy egy adott portot lehet megnyitni.</span><span class="sxs-lookup"><span data-stu-id="14276-161">Maximum request time is hello maximum time window that a specific port can be opened.</span></span>

3. <span data-ttu-id="14276-162">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="14276-162">Select **OK**.</span></span>

## <a name="requesting-access-tooa-vm"></a><span data-ttu-id="14276-163">A kért hozzáférés tooa méretű VM</span><span class="sxs-lookup"><span data-stu-id="14276-163">Requesting access tooa VM</span></span>

<span data-ttu-id="14276-164">toorequest hozzáférés tooa VM:</span><span class="sxs-lookup"><span data-stu-id="14276-164">toorequest access tooa VM:</span></span>

1. <span data-ttu-id="14276-165">A hello **közvetlenül a virtuális gép elérhető** panelen, jelölje be hello **beállított** fülre.</span><span class="sxs-lookup"><span data-stu-id="14276-165">On hello **Just in time VM access** blade, select hello **Configured** tab.</span></span>
2. <span data-ttu-id="14276-166">A **virtuális gépek**, válassza ki a megjeleníteni kívánt tooenable hozzáférés hello virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="14276-166">Under **VMs**, select hello VMs that you want tooenable access.</span></span> <span data-ttu-id="14276-167">A pipa következő tooa VM ez helyezi.</span><span class="sxs-lookup"><span data-stu-id="14276-167">This puts a checkmark next tooa VM.</span></span>
3. <span data-ttu-id="14276-168">Válassza ki **hozzáférés kérése**.</span><span class="sxs-lookup"><span data-stu-id="14276-168">Select **Request access**.</span></span> <span data-ttu-id="14276-169">Ekkor megnyílik a hello **hozzáférés kérése** panelen.</span><span class="sxs-lookup"><span data-stu-id="14276-169">This opens hello **Request access** blade.</span></span>

  ![Kérelem hozzáférés tooa méretű VM][4]

4. <span data-ttu-id="14276-171">A hello **hozzáférés kérése** panelen konfigurálhatja az egyes VM hello portok tooopen együtt hello forrás IP-címe, amely hello port megnyitott hello időkerete tooand mely hello a port meg van nyitva.</span><span class="sxs-lookup"><span data-stu-id="14276-171">On hello **Request access** blade, you configure for each VM hello ports tooopen along with hello source IP that hello port is opened tooand hello time window for which hello port is opened.</span></span> <span data-ttu-id="14276-172">Hello csak a idő házirendben beállított hozzáférés csak toohello portok is igényelhet.</span><span class="sxs-lookup"><span data-stu-id="14276-172">You can request access only toohello ports that are configured in hello just in time policy.</span></span> <span data-ttu-id="14276-173">Minden porthoz tartozik egy maximális engedélyezett idő, csak az idő házirend hello származik.</span><span class="sxs-lookup"><span data-stu-id="14276-173">Each port has a maximum allowed time derived from hello just in time policy.</span></span>
5. <span data-ttu-id="14276-174">Válassza ki **portok megnyitása**.</span><span class="sxs-lookup"><span data-stu-id="14276-174">Select **Open ports**.</span></span>

## <a name="editing-a-just-in-time-access-policy"></a><span data-ttu-id="14276-175">Szerkesztés csak hozzáférési házirendben idő</span><span class="sxs-lookup"><span data-stu-id="14276-175">Editing a just in time access policy</span></span>

<span data-ttu-id="14276-176">Módosíthatja a virtuális gép létezik csak az idő házirend hozzáadásával és konfigurálásával egy új port tooopen ezt a virtuális gépet, vagy bármely más paraméter módosításával kapcsolódó tooan már védett port.</span><span class="sxs-lookup"><span data-stu-id="14276-176">You can change a VM's existing just in time policy by adding and configuring a new port tooopen for that VM, or by changing any other parameter related tooan already protected port.</span></span>

<span data-ttu-id="14276-177">A rendezés tooedit egy meglévő csak az idő házirendet a virtuális gépek hello **beállított** lapon:</span><span class="sxs-lookup"><span data-stu-id="14276-177">In order tooedit an existing just in time policy of a VM, hello **Configured** tab is used:</span></span>

1. <span data-ttu-id="14276-178">A **virtuális gépek**, válassza ki a virtuális gép tooadd egy port tooby, kattintson a három hello pontokból hello soron belüli ezt a virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="14276-178">Under **VMs**, select a VM tooadd a port tooby clicking on hello three dots within hello row for that VM.</span></span> <span data-ttu-id="14276-179">Ekkor megnyílik egy menüben.</span><span class="sxs-lookup"><span data-stu-id="14276-179">This opens a menu.</span></span>
2. <span data-ttu-id="14276-180">Válassza ki **szerkesztése** hello menüben.</span><span class="sxs-lookup"><span data-stu-id="14276-180">Select **Edit** in hello menu.</span></span> <span data-ttu-id="14276-181">Ekkor megnyílik a hello **JIT VM konfiguráció** panelen.</span><span class="sxs-lookup"><span data-stu-id="14276-181">This opens hello **JIT VM access configuration** blade.</span></span>

  ![Házirend szerkesztése][8]

3. <span data-ttu-id="14276-183">A hello **JIT VM konfiguráció** panelen módosíthatja hello meglévő beállítások port már védett gombra kattintva a porton, vagy választhat **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="14276-183">On hello **JIT VM access configuration** blade, you can either edit hello existing settings of an already protected port by clicking on its port, or you can select **Add**.</span></span> <span data-ttu-id="14276-184">Ekkor megnyílik a hello **Hozzáadás portkonfigurációjának** panelen.</span><span class="sxs-lookup"><span data-stu-id="14276-184">This opens hello **Add port configuration** blade.</span></span>

  ![Adjon hozzá egy portot][7]

4. <span data-ttu-id="14276-186">A **Hozzáadás portkonfigurációjának** panel hello port, a protokoll típusát, a forrás IP-címek és a kérelem maximális idő azonosításához.</span><span class="sxs-lookup"><span data-stu-id="14276-186">On **Add port configuration** blade identify hello port, protocol type, allowed source IPs, and maximum request time.</span></span>
5. <span data-ttu-id="14276-187">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="14276-187">Select **OK**.</span></span>
6. <span data-ttu-id="14276-188">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="14276-188">Select **Save**.</span></span>

## <a name="auditing-just-in-time-access-activity"></a><span data-ttu-id="14276-189">Csak az idő-hozzáférési tevékenységet naplózás</span><span class="sxs-lookup"><span data-stu-id="14276-189">Auditing just in time access activity</span></span>

<span data-ttu-id="14276-190">Akkor is kaphat a naplófájl-keresési VM tevékenységeket.</span><span class="sxs-lookup"><span data-stu-id="14276-190">You can gain insights into VM activities using log search.</span></span> <span data-ttu-id="14276-191">tooview naplói:</span><span class="sxs-lookup"><span data-stu-id="14276-191">tooview logs:</span></span>

1. <span data-ttu-id="14276-192">A hello **közvetlenül a virtuális gép elérhető** panelen, jelölje be hello **beállított** fülre.</span><span class="sxs-lookup"><span data-stu-id="14276-192">On hello **Just in time VM access** blade, select hello **Configured** tab.</span></span>
2. <span data-ttu-id="14276-193">A **virtuális gépek**, ezt a virtuális gépet a hello soron belüli hello három pont kattintva válassza ki a Virtuálisgép-tooview adatok kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="14276-193">Under **VMs**, select a VM tooview information about by clicking on hello three dots within hello row for that VM.</span></span> <span data-ttu-id="14276-194">Ekkor megnyílik egy menüben.</span><span class="sxs-lookup"><span data-stu-id="14276-194">This opens a menu.</span></span>
3. <span data-ttu-id="14276-195">Válassza ki **tevékenységnapló** hello menüben.</span><span class="sxs-lookup"><span data-stu-id="14276-195">Select **Activity Log** in hello menu.</span></span> <span data-ttu-id="14276-196">Ekkor megnyílik a hello **tevékenységnapló** panelen.</span><span class="sxs-lookup"><span data-stu-id="14276-196">This opens hello **Activity log** blade.</span></span>

![Válassza ki a tevékenység naplója][9]

<span data-ttu-id="14276-198">Hello **tevékenységnapló** panel ezt a virtuális gépet és idő, dátum és előfizetés korábbi műveletek szűrt nézetét jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="14276-198">hello **Activity log** blade provides a filtered view of previous operations for that VM along with time, date, and subscription.</span></span>

![Tevékenység napló megtekintése][5]

<span data-ttu-id="14276-200">Hello naplóadatok kiválasztásával letöltheti **ide toodownload minden hello elem CSV-ként**.</span><span class="sxs-lookup"><span data-stu-id="14276-200">You can download hello log information by selecting **Click here toodownload all hello items as CSV**.</span></span>

<span data-ttu-id="14276-201">Hello szűrők módosítása, és válassza ki **alkalmaz** toocreate a Keresés és a napló.</span><span class="sxs-lookup"><span data-stu-id="14276-201">Modify hello filters and select **Apply** toocreate a search and log.</span></span>

## <a name="using-just-in-time-vm-access-via-powershell"></a><span data-ttu-id="14276-202">Igény szerint VM hozzáférés PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="14276-202">Using just in time VM access via PowerShell</span></span>

<span data-ttu-id="14276-203">Rendelés toouse hello csak a PowerShell idő megoldás, győződjön meg arról, hogy hello [legújabb](/powershell/azure/install-azurerm-ps) Azure PowerShell-verzió.</span><span class="sxs-lookup"><span data-stu-id="14276-203">In order toouse hello just in time solution via PowerShell, make sure you have hello [latest](/powershell/azure/install-azurerm-ps) Azure PowerShell version.</span></span>
<span data-ttu-id="14276-204">Ha így tesz, meg kell-e tooinstall hello [legújabb](https://www.powershellgallery.com/packages/Azure-Security-Center/0.0.12) az Azure Security Center hello PowerShell gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="14276-204">Once you do, you need tooinstall hello [latest](https://www.powershellgallery.com/packages/Azure-Security-Center/0.0.12) Azure Security Center from hello PowerShell gallery.</span></span>

### <a name="configuring-a-just-in-time-policy-for-a-vm"></a><span data-ttu-id="14276-205">Konfigurálása csak a virtuális gépek ideje házirend</span><span class="sxs-lookup"><span data-stu-id="14276-205">Configuring a just in time policy for a VM</span></span>

<span data-ttu-id="14276-206">tooconfigure csak egy adott VM-idő szabályzatok kell toorun ezt a parancsot a PowerShell-munkamenetben: Set-ASCJITAccessPolicy.</span><span class="sxs-lookup"><span data-stu-id="14276-206">tooconfigure a just in time policy on a specific VM, you need toorun this command in your PowerShell session: Set-ASCJITAccessPolicy.</span></span>
<span data-ttu-id="14276-207">Hajtsa végre a hello parancsmag dokumentáció toolearn további.</span><span class="sxs-lookup"><span data-stu-id="14276-207">Follow hello cmdlet documentation toolearn more.</span></span>

### <a name="requesting-access-tooa-vm"></a><span data-ttu-id="14276-208">A kért hozzáférés tooa méretű VM</span><span class="sxs-lookup"><span data-stu-id="14276-208">Requesting access tooa VM</span></span>

<span data-ttu-id="14276-209">csak az idő-megoldások hello tooaccess egy adott virtuális gép által védett, akkor kell toorun ezt a parancsot a PowerShell-munkamenetben: meghívása ASCJITAccess.</span><span class="sxs-lookup"><span data-stu-id="14276-209">tooaccess a specific VM that is protected with hello just in time solution, you need toorun this command in your PowerShell session: Invoke-ASCJITAccess.</span></span>
<span data-ttu-id="14276-210">Hajtsa végre a hello parancsmag dokumentáció toolearn további.</span><span class="sxs-lookup"><span data-stu-id="14276-210">Follow hello cmdlet documentation toolearn more.</span></span>

## <a name="next-steps"></a><span data-ttu-id="14276-211">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="14276-211">Next steps</span></span>
<span data-ttu-id="14276-212">Ebben a cikkben megtanulta, hogyan igény szerint VM hozzáférés a Security Center segítségével szabályozhatja a hozzáférést tooyour Azure virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="14276-212">In this article, you learned how just in time VM access in Security Center helps you control access tooyour Azure virtual machines.</span></span>

<span data-ttu-id="14276-213">További információ a Security Center toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="14276-213">toolearn more about Security Center, see hello following:</span></span>

- <span data-ttu-id="14276-214">[Biztonsági szabályzatok beállítása](security-center-policies.md) – további hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="14276-214">[Setting security policies](security-center-policies.md) — Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
- <span data-ttu-id="14276-215">[Biztonsági javaslatok kezelése](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.</span><span class="sxs-lookup"><span data-stu-id="14276-215">[Managing security recommendations](security-center-recommendations.md) — Learn how recommendations help you protect your Azure resources.</span></span>
- <span data-ttu-id="14276-216">[Biztonsági állapotfigyelés](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.</span><span class="sxs-lookup"><span data-stu-id="14276-216">[Security health monitoring](security-center-monitoring.md) — Learn how toomonitor hello health of your Azure resources.</span></span>
- <span data-ttu-id="14276-217">[Kezelése és reagálás toosecurity riasztások](security-center-managing-and-responding-alerts.md) – további hogyan toomanage és válaszoljon toosecurity riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="14276-217">[Managing and responding toosecurity alerts](security-center-managing-and-responding-alerts.md) — Learn how toomanage and respond toosecurity alerts.</span></span>
- <span data-ttu-id="14276-218">[Partnermegoldások figyelése](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="14276-218">[Monitoring partner solutions](security-center-partner-solutions.md) — Learn how toomonitor hello health status of your partner solutions.</span></span>
- <span data-ttu-id="14276-219">[Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="14276-219">[Security Center FAQ](security-center-faq.md) — Find frequently asked questions about using hello service.</span></span>
- <span data-ttu-id="14276-220">[Azure Security blog](https://blogs.msdn.microsoft.com/azuresecurity/) – Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.</span><span class="sxs-lookup"><span data-stu-id="14276-220">[Azure Security blog](https://blogs.msdn.microsoft.com/azuresecurity/) — Find blog posts about Azure security and compliance.</span></span>


<!--Image references-->
[1]: ./media/security-center-just-in-time/just-in-time-scenario.png
[2]: ./media/security-center-just-in-time/just-in-time.png
[3]: ./media/security-center-just-in-time/enable-just-in-time-access.png
[4]: ./media/security-center-just-in-time/request-access-to-a-vm.png
[5]: ./media/security-center-just-in-time/activity-log.png
[6]: ./media/security-center-just-in-time/default-ports.png
[7]: ./media/security-center-just-in-time/add-a-port.png
[8]: ./media/security-center-just-in-time/edit-policy.png
[9]: ./media/security-center-just-in-time/select-activity-log.png
