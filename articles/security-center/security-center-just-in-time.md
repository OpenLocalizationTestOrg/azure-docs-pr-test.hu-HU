---
title: "Csak idő virtuális gép férnek hozzá az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan hogyan igény szerint le a virtuális gép elérhető az Azure Security Center segítségével, az az Azure virtuális gépeken való hozzáférést."
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
ms.openlocfilehash: 5bb87488dcfc79ed4baa1dbd81dc4e1174f84e4b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="manage-virtual-machine-access-using-just-in-time"></a><span data-ttu-id="8f872-103">JIT-virtuális gép hozzáférés kezelése</span><span class="sxs-lookup"><span data-stu-id="8f872-103">Manage virtual machine access using just in time</span></span>

<span data-ttu-id="8f872-104">Csak az idő a virtuális gép (VM) hozzáférés segítségével az Azure virtuális gépeken, támadásoknak való kitettség csökkentése során könnyen hozzá lehet férni a virtuális gépekhez, szükség esetén csatlakoztassa a bejövő forgalom zárolását.</span><span class="sxs-lookup"><span data-stu-id="8f872-104">Just in time virtual machine (VM) access can be used to lock down inbound traffic to your Azure VMs, reducing exposure to attacks while providing easy access to connect to VMs when needed.</span></span>

> [!NOTE]
> <span data-ttu-id="8f872-105">A szolgáltatás egyelőre időben csak és érhető el a Security Center Standard csomagra.</span><span class="sxs-lookup"><span data-stu-id="8f872-105">The just in time feature is in preview and available on the Standard tier of Security Center.</span></span>  <span data-ttu-id="8f872-106">Lásd: [árazás](security-center-pricing.md) további bővebben a Security Center által tarifacsomag szükséges.</span><span class="sxs-lookup"><span data-stu-id="8f872-106">See [Pricing](security-center-pricing.md) to learn more about Security Center's pricing tiers.</span></span>
>
>

## <a name="attack-scenario"></a><span data-ttu-id="8f872-107">Támadás</span><span class="sxs-lookup"><span data-stu-id="8f872-107">Attack scenario</span></span>

<span data-ttu-id="8f872-108">Találgatásos támadások gyakran felügyeleti célportjainak jelentkezhessenek be a virtuális gépek eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="8f872-108">Brute force attacks commonly target management ports as a means to gain access to a VM.</span></span> <span data-ttu-id="8f872-109">Ha sikeres, a támadó ellenőrzése alatt tartja a virtuális gép igénybe, és egy foothold létesíteni a környezetbe.</span><span class="sxs-lookup"><span data-stu-id="8f872-109">If successful, an attacker can take control over the VM and establish a foothold into your environment.</span></span>

<span data-ttu-id="8f872-110">Egy találgatásos támadással való kitettség csökkentése érdekében módja az, hogy mennyi ideig legyen egy nyitott port korlátozza.</span><span class="sxs-lookup"><span data-stu-id="8f872-110">One way to reduce exposure to a brute force attack is to limit the amount of time that a port is open.</span></span> <span data-ttu-id="8f872-111">Szolgáltatásfelügyelet portjai nem kell mindig kapcsolódniuk kell megnyitni.</span><span class="sxs-lookup"><span data-stu-id="8f872-111">Management ports do not need to be open at all times.</span></span> <span data-ttu-id="8f872-112">Csak szükségük lehet megnyitva, amikor csatlakoznak a virtuális Gépet, például felügyeleti vagy karbantartási feladatok elvégzésére.</span><span class="sxs-lookup"><span data-stu-id="8f872-112">They only need to be open while you are connected to the VM, for example to perform management or maintenance tasks.</span></span> <span data-ttu-id="8f872-113">Amikor a JIT engedélyezve van, a Security Center az [hálózati biztonsági csoport](../virtual-network/virtual-networks-nsg.md) (NSG) szabályok, amelyek felügyelet portjai való hozzáférés korlátozása, így nem tudja megcélozni a támadók.</span><span class="sxs-lookup"><span data-stu-id="8f872-113">When just in time is enabled, Security Center uses [Network Security Group](../virtual-network/virtual-networks-nsg.md) (NSG) rules, which restrict access to management ports so they cannot be targeted by attackers.</span></span>

![Csak az idő forgatókönyv][1]

## <a name="how-does-just-in-time-access-work"></a><span data-ttu-id="8f872-115">Hogyan csak az idő access működik?</span><span class="sxs-lookup"><span data-stu-id="8f872-115">How does just in time access work?</span></span>

<span data-ttu-id="8f872-116">Ha az igény szerinti hozzáférés engedélyezve van, a Security Center minden, az Azure-beli virtuális gépekre érkező forgalmat zárol egy NSG-szabály létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="8f872-116">When just in time is enabled, Security Center locks down inbound traffic to your Azure VMs by creating an NSG rule.</span></span> <span data-ttu-id="8f872-117">Kiválaszthatja a a virtuális Gépre, amelyre a bejövő forgalom lesz zárolva.</span><span class="sxs-lookup"><span data-stu-id="8f872-117">You select the ports on the VM to which inbound traffic will be locked down.</span></span> <span data-ttu-id="8f872-118">Ezeket a portokat szabályozzák az imént idő megoldásban.</span><span class="sxs-lookup"><span data-stu-id="8f872-118">These ports are controlled by the just in time solution.</span></span>

<span data-ttu-id="8f872-119">Amikor egy felhasználó egy virtuális Géphez való hozzáférést igényel, a Security Center ellenőrzi, hogy a felhasználó rendelkezik-e [szerepköralapú hozzáférés-vezérlést (RBAC)](../active-directory/role-based-access-control-configure.md) az engedélyeket, az írási hozzáférést biztosítson az Azure-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="8f872-119">When a user requests access to a VM, Security Center checks that the user has [Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-configure.md) permissions that provide write access for the Azure resource.</span></span> <span data-ttu-id="8f872-120">Ha, írási jogosultsággal rendelkeznek, a kérelem jóváhagyása és a Security Center automatikusan konfigurálja a hálózati biztonsági csoportok (NSG-k) a bejövő forgalom a felügyeleti portokat ennyi ideig való adott meg.</span><span class="sxs-lookup"><span data-stu-id="8f872-120">If they have write permissions, the request is approved and Security Center automatically configures the Network Security Groups (NSGs) to allow inbound traffic to the management ports for the amount of time you specified.</span></span> <span data-ttu-id="8f872-121">Az időszak lejárta után a Security Center az NSG-ket visszaállítja korábbi állapotába.</span><span class="sxs-lookup"><span data-stu-id="8f872-121">After the time has expired, Security Center restores the NSGs to their previous states.</span></span>

> [!NOTE]
> <span data-ttu-id="8f872-122">Biztonsági központ csak a virtuális gép elérhető jelenleg csak virtuális gépek Azure Resource Manager használatával telepített.</span><span class="sxs-lookup"><span data-stu-id="8f872-122">Security Center just in time VM access currently supports only VMs deployed through Azure Resource Manager.</span></span> <span data-ttu-id="8f872-123">Ismerje meg, a klasszikus és Resource Manager üzembe helyezési modellel kapcsolatos információkért tekintse meg a [Azure Resource Manager és klasszikus üzembe helyezési](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="8f872-123">To learn more about the classic and Resource Manager deployment models see [Azure Resource Manager vs. classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>
>
>

## <a name="using-just-in-time-access"></a><span data-ttu-id="8f872-124">Csak az idő access használatával</span><span class="sxs-lookup"><span data-stu-id="8f872-124">Using just in time access</span></span>

<span data-ttu-id="8f872-125">A **közvetlenül a virtuális gép elérhető** csempét a **Security Center** panel vonatkozó csak időben való hozzáférésre és jóváhagyott hozzáférési kérelmek száma az elmúlt héten konfigurált virtuális gépek számát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="8f872-125">The **Just in time VM access** tile on the **Security Center** blade shows the number of VMs configured for just in time access and the number of approved access requests made in the last week.</span></span>

<span data-ttu-id="8f872-126">Válassza ki a **közvetlenül a virtuális gép elérhető** csempe és a **közvetlenül a virtuális gép elérhető** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="8f872-126">Select the **Just in time VM access** tile and the **Just in time VM access** blade opens.</span></span>

![Virtuális gép hozzáférés JIT csempéje][2]

<span data-ttu-id="8f872-128">A **közvetlenül a virtuális gép elérhető** panel információt nyújt a virtuális gépek állapotát:</span><span class="sxs-lookup"><span data-stu-id="8f872-128">The **Just in time VM access** blade provides information on the state of your VMs:</span></span>

- <span data-ttu-id="8f872-129">**Konfigurált** -virtuális gépek, amelyek csak a virtuális gép elérhető támogatására vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="8f872-129">**Configured** - VMs that have been configured to support just in time VM access.</span></span> <span data-ttu-id="8f872-130">Közölt adatok az elmúlt héten, és az egyes virtuális gépek magában foglalja a jóváhagyott kéréseket, legutóbbi hozzáférés dátuma és időpontja és utolsó felhasználó számát.</span><span class="sxs-lookup"><span data-stu-id="8f872-130">The data presented is for the last week and includes for each VM the number of approved requests, last access date and time, and last user.</span></span>
- <span data-ttu-id="8f872-131">**Ajánlott** -csak a virtuális gép elérhető támogatható, de nincs beállítva a virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="8f872-131">**Recommended** - VMs that can support just in time VM access but have not been configured to.</span></span> <span data-ttu-id="8f872-132">Javasoljuk, hogy engedélyezze a csak az idő VM hozzáférés-vezérlés a virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="8f872-132">We recommend that you enable just in time VM access control for these VMs.</span></span> <span data-ttu-id="8f872-133">Lásd: [engedélyezése csak a virtuális gép elérhető](#enable-just-in-time-vm-access).</span><span class="sxs-lookup"><span data-stu-id="8f872-133">See [Enable just in time VM access](#enable-just-in-time-vm-access).</span></span>
- <span data-ttu-id="8f872-134">**Nincs javaslat** – ennek oka, hogy egy virtuális Gépet nem lehet ajánlott okozhatják:</span><span class="sxs-lookup"><span data-stu-id="8f872-134">**No recommendation** - Reasons that can cause a VM not to be recommended are:</span></span>
  - <span data-ttu-id="8f872-135">Hiányzik az NSG - a csak időbeli megoldáshoz szükségesek egy NSG-t kell.</span><span class="sxs-lookup"><span data-stu-id="8f872-135">Missing NSG - The just in time solution requires an NSG to be in place.</span></span>
  - <span data-ttu-id="8f872-136">Klasszikus VM - VM elérhető csak a Security Center jelenleg csak virtuális gépek Azure Resource Manager használatával telepített.</span><span class="sxs-lookup"><span data-stu-id="8f872-136">Classic VM - Security Center just in time VM access currently supports only VMs deployed through Azure Resource Manager.</span></span> <span data-ttu-id="8f872-137">A klasszikus üzembe helyezési nem támogatja a csak az idő megoldás.</span><span class="sxs-lookup"><span data-stu-id="8f872-137">A classic deployment is not supported by the just in time solution.</span></span>
  - <span data-ttu-id="8f872-138">Ebbe a kategóriába tartozó egyéb - egy virtuális gép van Ha pedig csak a megoldás ki van kapcsolva a biztonsági szabályzatban az előfizetés vagy az erőforráscsoportot, vagy időpontban a virtuális gép hiányzik egy nyilvános IP-cím, és nem rendelkezik egy NSG.</span><span class="sxs-lookup"><span data-stu-id="8f872-138">Other - A VM is in this category if the just in time solution is turned off in the security policy of the subscription or the resource group, or that the VM is missing a public IP and doesn't have an NSG in place.</span></span>

## <a name="configuring-a-just-in-time-access-policy"></a><span data-ttu-id="8f872-139">Beállítása csak hozzáférési házirendben idő</span><span class="sxs-lookup"><span data-stu-id="8f872-139">Configuring a just in time access policy</span></span>

<span data-ttu-id="8f872-140">Az engedélyezni kívánt virtuális gépek kiválasztása:</span><span class="sxs-lookup"><span data-stu-id="8f872-140">To select the VMs that you want to enable:</span></span>

1. <span data-ttu-id="8f872-141">Az a **közvetlenül a virtuális gép elérhető** panelen válassza a **ajánlott** fülre.</span><span class="sxs-lookup"><span data-stu-id="8f872-141">On the **Just in time VM access** blade, select the **Recommended** tab.</span></span>

  ![Csak az idő hozzáférés engedélyezése][3]

2. <span data-ttu-id="8f872-143">A **virtuális gépek**, válassza ki az engedélyezni kívánt virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="8f872-143">Under **VMs**, select the VMs that you want to enable.</span></span> <span data-ttu-id="8f872-144">Ez helyezi a virtuális gépek melletti négyzetet.</span><span class="sxs-lookup"><span data-stu-id="8f872-144">This puts a checkmark next to a VM.</span></span>
3. <span data-ttu-id="8f872-145">Válassza ki **engedélyezése virtuális gépeken JIT**.</span><span class="sxs-lookup"><span data-stu-id="8f872-145">Select **Enable JIT on VMs**.</span></span>
4. <span data-ttu-id="8f872-146">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="8f872-146">Select **Save**.</span></span>

### <a name="default-ports"></a><span data-ttu-id="8f872-147">Alapértelmezett port</span><span class="sxs-lookup"><span data-stu-id="8f872-147">Default ports</span></span>

<span data-ttu-id="8f872-148">Az alapértelmezett portok, amely a Security Center JIT engedélyezését javasolja tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="8f872-148">You can see the default ports that Security Center recommends enabling just in time.</span></span>

1. <span data-ttu-id="8f872-149">Az a **közvetlenül a virtuális gép elérhető** panelen válassza a **ajánlott** fülre.</span><span class="sxs-lookup"><span data-stu-id="8f872-149">On the **Just in time VM access** blade, select the **Recommended** tab.</span></span>

  ![Megjeleníti az alapértelmezett portok][6]

2. <span data-ttu-id="8f872-151">A **virtuális gépek**, jelöljön ki egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="8f872-151">Under **VMs**, select a VM.</span></span> <span data-ttu-id="8f872-152">Ez a virtuális gép mellett be van jelölve, és megnyílik a **JIT VM konfiguráció** panelen.</span><span class="sxs-lookup"><span data-stu-id="8f872-152">This puts a checkmark next to the VM and opens the **JIT VM access configuration** blade.</span></span> <span data-ttu-id="8f872-153">Ezt a panelt jeleníti meg az alapértelmezett portok.</span><span class="sxs-lookup"><span data-stu-id="8f872-153">This blade displays the default ports.</span></span>

### <a name="add-ports"></a><span data-ttu-id="8f872-154">Portok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8f872-154">Add ports</span></span>

<span data-ttu-id="8f872-155">Az a **JIT VM konfiguráció** panelen is hozzáadhat és engedélyezése a csak akkor szeretne új port konfigurálása idő megoldásban.</span><span class="sxs-lookup"><span data-stu-id="8f872-155">From the **JIT VM access configuration** blade, you can also add and configure a new port on which you want to enable the just in time solution.</span></span>

1. <span data-ttu-id="8f872-156">Az a **JIT VM konfiguráció** panelen válassza **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="8f872-156">On the **JIT VM access configuration** blade, select **Add**.</span></span> <span data-ttu-id="8f872-157">Ekkor megnyílik a **Hozzáadás portkonfigurációjának** panelen.</span><span class="sxs-lookup"><span data-stu-id="8f872-157">This opens the **Add port configuration** blade.</span></span>

  ![Port konfigurálása][7]

2. <span data-ttu-id="8f872-159">A **Hozzáadás portkonfigurációjának** panelen azonosította a port, protokolltípus kérelem ideje forrás IP-címek és a maximális engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="8f872-159">On **Add port configuration** blade, you identify the port, protocol type, allowed source IPs, and maximum request time.</span></span>

  <span data-ttu-id="8f872-160">Forrás IP-címek az IP-címtartományok is elérheti az engedélyezett kérésre engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="8f872-160">Allowed source IPs are the IP ranges allowed to get access upon an approved request.</span></span>

  <span data-ttu-id="8f872-161">Kérések maximális ideje a maximális időszak, hogy egy adott portot lehet megnyitni.</span><span class="sxs-lookup"><span data-stu-id="8f872-161">Maximum request time is the maximum time window that a specific port can be opened.</span></span>

3. <span data-ttu-id="8f872-162">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="8f872-162">Select **OK**.</span></span>

## <a name="requesting-access-to-a-vm"></a><span data-ttu-id="8f872-163">A virtuális gépek hozzáférést kérő</span><span class="sxs-lookup"><span data-stu-id="8f872-163">Requesting access to a VM</span></span>

<span data-ttu-id="8f872-164">Kérjen hozzáférést egy virtuális géphez:</span><span class="sxs-lookup"><span data-stu-id="8f872-164">To request access to a VM:</span></span>

1. <span data-ttu-id="8f872-165">Az a **közvetlenül a virtuális gép elérhető** panelen válassza a **beállított** fülre.</span><span class="sxs-lookup"><span data-stu-id="8f872-165">On the **Just in time VM access** blade, select the **Configured** tab.</span></span>
2. <span data-ttu-id="8f872-166">A **virtuális gépek**, válassza ki a virtuális gépeket, amely engedélyezi a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="8f872-166">Under **VMs**, select the VMs that you want to enable access.</span></span> <span data-ttu-id="8f872-167">Ez helyezi a virtuális gépek melletti négyzetet.</span><span class="sxs-lookup"><span data-stu-id="8f872-167">This puts a checkmark next to a VM.</span></span>
3. <span data-ttu-id="8f872-168">Válassza ki **hozzáférés kérése**.</span><span class="sxs-lookup"><span data-stu-id="8f872-168">Select **Request access**.</span></span> <span data-ttu-id="8f872-169">Ekkor megnyílik a **hozzáférés kérése** panelen.</span><span class="sxs-lookup"><span data-stu-id="8f872-169">This opens the **Request access** blade.</span></span>

  ![Kérjen hozzáférést egy virtuális géphez][4]

4. <span data-ttu-id="8f872-171">Az a **hozzáférés kérése** panelen konfigurálnia az egyes virtuális gépek a portok megnyitásához és a forrás IP-címe, amely a port meg van nyitva, és a időszak, amelynek az port meg van nyitva.</span><span class="sxs-lookup"><span data-stu-id="8f872-171">On the **Request access** blade, you configure for each VM the ports to open along with the source IP that the port is opened to and the time window for which the port is opened.</span></span> <span data-ttu-id="8f872-172">Kérhet hozzáférést csak a portok az imént beállított idő házirendben.</span><span class="sxs-lookup"><span data-stu-id="8f872-172">You can request access only to the ports that are configured in the just in time policy.</span></span> <span data-ttu-id="8f872-173">Minden porthoz tartozik egy engedélyezett maximális időtartamot a csak származó idő házirendben.</span><span class="sxs-lookup"><span data-stu-id="8f872-173">Each port has a maximum allowed time derived from the just in time policy.</span></span>
5. <span data-ttu-id="8f872-174">Válassza ki **portok megnyitása**.</span><span class="sxs-lookup"><span data-stu-id="8f872-174">Select **Open ports**.</span></span>

## <a name="editing-a-just-in-time-access-policy"></a><span data-ttu-id="8f872-175">Szerkesztés csak hozzáférési házirendben idő</span><span class="sxs-lookup"><span data-stu-id="8f872-175">Editing a just in time access policy</span></span>

<span data-ttu-id="8f872-176">Módosíthatja a virtuális gép csak az idő a házirend meglévő, hozzáadásával és konfigurálásával, nyissa meg ezt a virtuális gépet az új port, illetve egy már védett port kapcsolódó más paraméter módosításával.</span><span class="sxs-lookup"><span data-stu-id="8f872-176">You can change a VM's existing just in time policy by adding and configuring a new port to open for that VM, or by changing any other parameter related to an already protected port.</span></span>

<span data-ttu-id="8f872-177">Ahhoz, hogy csak az idő házirendet a virtuális gépek meglévő szerkesztésekor a **beállított** lapon:</span><span class="sxs-lookup"><span data-stu-id="8f872-177">In order to edit an existing just in time policy of a VM, the **Configured** tab is used:</span></span>

1. <span data-ttu-id="8f872-178">A **virtuális gépek**, jelöljön ki egy virtuális Gépet, a port hozzáadása a soron belüli három pontra kattintva ezt a virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="8f872-178">Under **VMs**, select a VM to add a port to by clicking on the three dots within the row for that VM.</span></span> <span data-ttu-id="8f872-179">Ekkor megnyílik egy menüben.</span><span class="sxs-lookup"><span data-stu-id="8f872-179">This opens a menu.</span></span>
2. <span data-ttu-id="8f872-180">Válassza ki **szerkesztése** a menüben.</span><span class="sxs-lookup"><span data-stu-id="8f872-180">Select **Edit** in the menu.</span></span> <span data-ttu-id="8f872-181">Ekkor megnyílik a **JIT VM konfiguráció** panelen.</span><span class="sxs-lookup"><span data-stu-id="8f872-181">This opens the **JIT VM access configuration** blade.</span></span>

  ![Házirend szerkesztése][8]

3. <span data-ttu-id="8f872-183">Az a **JIT VM konfiguráció** panelen, vagy szerkesztheti a meglévő beállítások már védett port a porton kattint, vagy választhat **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="8f872-183">On the **JIT VM access configuration** blade, you can either edit the existing settings of an already protected port by clicking on its port, or you can select **Add**.</span></span> <span data-ttu-id="8f872-184">Ekkor megnyílik a **Hozzáadás portkonfigurációjának** panelen.</span><span class="sxs-lookup"><span data-stu-id="8f872-184">This opens the **Add port configuration** blade.</span></span>

  ![Adjon hozzá egy portot][7]

4. <span data-ttu-id="8f872-186">A **Hozzáadás portkonfigurációjának** panel azonosíthatja a port, a protokoll típusát, a forrás IP-címek és a kérelem maximális idő.</span><span class="sxs-lookup"><span data-stu-id="8f872-186">On **Add port configuration** blade identify the port, protocol type, allowed source IPs, and maximum request time.</span></span>
5. <span data-ttu-id="8f872-187">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="8f872-187">Select **OK**.</span></span>
6. <span data-ttu-id="8f872-188">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="8f872-188">Select **Save**.</span></span>

## <a name="auditing-just-in-time-access-activity"></a><span data-ttu-id="8f872-189">Csak az idő-hozzáférési tevékenységet naplózás</span><span class="sxs-lookup"><span data-stu-id="8f872-189">Auditing just in time access activity</span></span>

<span data-ttu-id="8f872-190">Akkor is kaphat a naplófájl-keresési VM tevékenységeket.</span><span class="sxs-lookup"><span data-stu-id="8f872-190">You can gain insights into VM activities using log search.</span></span> <span data-ttu-id="8f872-191">Naplók megtekintése:</span><span class="sxs-lookup"><span data-stu-id="8f872-191">To view logs:</span></span>

1. <span data-ttu-id="8f872-192">Az a **közvetlenül a virtuális gép elérhető** panelen válassza a **beállított** fülre.</span><span class="sxs-lookup"><span data-stu-id="8f872-192">On the **Just in time VM access** blade, select the **Configured** tab.</span></span>
2. <span data-ttu-id="8f872-193">A **virtuális gépek**, válassza ki a virtuális gépek ezt a virtuális gépet a soron belüli három pontra kattintva adatainak megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="8f872-193">Under **VMs**, select a VM to view information about by clicking on the three dots within the row for that VM.</span></span> <span data-ttu-id="8f872-194">Ekkor megnyílik egy menüben.</span><span class="sxs-lookup"><span data-stu-id="8f872-194">This opens a menu.</span></span>
3. <span data-ttu-id="8f872-195">Válassza ki **tevékenységnapló** a menüben.</span><span class="sxs-lookup"><span data-stu-id="8f872-195">Select **Activity Log** in the menu.</span></span> <span data-ttu-id="8f872-196">Ekkor megnyílik a **tevékenységnapló** panelen.</span><span class="sxs-lookup"><span data-stu-id="8f872-196">This opens the **Activity log** blade.</span></span>

![Válassza ki a tevékenység naplója][9]

<span data-ttu-id="8f872-198">A **tevékenységnapló** panel ezt a virtuális gépet és idő, dátum és előfizetés korábbi műveletek szűrt nézetét jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="8f872-198">The **Activity log** blade provides a filtered view of previous operations for that VM along with time, date, and subscription.</span></span>

![Tevékenység napló megtekintése][5]

<span data-ttu-id="8f872-200">A naplózási adatok kiválasztásával letöltheti **Ide kattintva letöltheti elemeinek CSV-ként**.</span><span class="sxs-lookup"><span data-stu-id="8f872-200">You can download the log information by selecting **Click here to download all the items as CSV**.</span></span>

<span data-ttu-id="8f872-201">Módosítsa a szűrőt, és válasszon **alkalmaz** a Keresés és a napló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="8f872-201">Modify the filters and select **Apply** to create a search and log.</span></span>

## <a name="using-just-in-time-vm-access-via-powershell"></a><span data-ttu-id="8f872-202">Igény szerint VM hozzáférés PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="8f872-202">Using just in time VM access via PowerShell</span></span>

<span data-ttu-id="8f872-203">Ahhoz, hogy az csak a PowerShell idő megoldás győződjön meg arról, hogy a [legújabb](/powershell/azure/install-azurerm-ps) Azure PowerShell-verzió.</span><span class="sxs-lookup"><span data-stu-id="8f872-203">In order to use the just in time solution via PowerShell, make sure you have the [latest](/powershell/azure/install-azurerm-ps) Azure PowerShell version.</span></span>
<span data-ttu-id="8f872-204">Ha így tesz, telepítenie kell a [legújabb](https://www.powershellgallery.com/packages/Azure-Security-Center/0.0.12) az Azure Security Center a PowerShell-galériából.</span><span class="sxs-lookup"><span data-stu-id="8f872-204">Once you do, you need to install the [latest](https://www.powershellgallery.com/packages/Azure-Security-Center/0.0.12) Azure Security Center from the PowerShell gallery.</span></span>

### <a name="configuring-a-just-in-time-policy-for-a-vm"></a><span data-ttu-id="8f872-205">Konfigurálása csak a virtuális gépek ideje házirend</span><span class="sxs-lookup"><span data-stu-id="8f872-205">Configuring a just in time policy for a VM</span></span>

<span data-ttu-id="8f872-206">Csak konfigurálása egy adott VM-idő szabályzatok futtassa ezt a parancsot a PowerShell-munkamenetben kell: Set-ASCJITAccessPolicy.</span><span class="sxs-lookup"><span data-stu-id="8f872-206">To configure a just in time policy on a specific VM, you need to run this command in your PowerShell session: Set-ASCJITAccessPolicy.</span></span>
<span data-ttu-id="8f872-207">A parancsmagok dokumentációira további kövesse.</span><span class="sxs-lookup"><span data-stu-id="8f872-207">Follow the cmdlet documentation to learn more.</span></span>

### <a name="requesting-access-to-a-vm"></a><span data-ttu-id="8f872-208">A virtuális gépek hozzáférést kérő</span><span class="sxs-lookup"><span data-stu-id="8f872-208">Requesting access to a VM</span></span>

<span data-ttu-id="8f872-209">Egy adott virtuális Gépet, a csak a védett eléréséhez idő megoldásban ez a parancs futtatásához a PowerShell-munkamenetben szüksége: meghívása ASCJITAccess.</span><span class="sxs-lookup"><span data-stu-id="8f872-209">To access a specific VM that is protected with the just in time solution, you need to run this command in your PowerShell session: Invoke-ASCJITAccess.</span></span>
<span data-ttu-id="8f872-210">A parancsmagok dokumentációira további kövesse.</span><span class="sxs-lookup"><span data-stu-id="8f872-210">Follow the cmdlet documentation to learn more.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8f872-211">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8f872-211">Next steps</span></span>
<span data-ttu-id="8f872-212">Ebben a cikkben megtanulta, hogyan igény szerint a Security Center segítséget nyújt a virtuális gép hozzáférés az Azure virtuális gépeken való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="8f872-212">In this article, you learned how just in time VM access in Security Center helps you control access to your Azure virtual machines.</span></span>

<span data-ttu-id="8f872-213">A Security Centerrel kapcsolatos további információkért olvassa el a következőket:</span><span class="sxs-lookup"><span data-stu-id="8f872-213">To learn more about Security Center, see the following:</span></span>

- <span data-ttu-id="8f872-214">[Biztonsági szabályzatok beállítása](security-center-policies.md) – útmutató az Azure-előfizetések és -erőforráscsoportok biztonsági szabályzatainak konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="8f872-214">[Setting security policies](security-center-policies.md) — Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
- <span data-ttu-id="8f872-215">[Biztonsági javaslatok kezelése](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.</span><span class="sxs-lookup"><span data-stu-id="8f872-215">[Managing security recommendations](security-center-recommendations.md) — Learn how recommendations help you protect your Azure resources.</span></span>
- <span data-ttu-id="8f872-216">[Biztonsági állapotfigyelés](security-center-monitoring.md) – útmutató az Azure-erőforrások állapotának figyelésére.</span><span class="sxs-lookup"><span data-stu-id="8f872-216">[Security health monitoring](security-center-monitoring.md) — Learn how to monitor the health of your Azure resources.</span></span>
- <span data-ttu-id="8f872-217">[Kezelése és válaszadás a biztonsági riasztások](security-center-managing-and-responding-alerts.md) – útmutató kezelése és válaszadás a biztonsági riasztásokra.</span><span class="sxs-lookup"><span data-stu-id="8f872-217">[Managing and responding to security alerts](security-center-managing-and-responding-alerts.md) — Learn how to manage and respond to security alerts.</span></span>
- <span data-ttu-id="8f872-218">[Partnermegoldások figyelése](security-center-partner-solutions.md) – megtudhatja, hogyan figyelheti a partnermegoldások biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="8f872-218">[Monitoring partner solutions](security-center-partner-solutions.md) — Learn how to monitor the health status of your partner solutions.</span></span>
- <span data-ttu-id="8f872-219">[Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések a szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="8f872-219">[Security Center FAQ](security-center-faq.md) — Find frequently asked questions about using the service.</span></span>
- <span data-ttu-id="8f872-220">[Azure Security blog](https://blogs.msdn.microsoft.com/azuresecurity/) – Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.</span><span class="sxs-lookup"><span data-stu-id="8f872-220">[Azure Security blog](https://blogs.msdn.microsoft.com/azuresecurity/) — Find blog posts about Azure security and compliance.</span></span>


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
