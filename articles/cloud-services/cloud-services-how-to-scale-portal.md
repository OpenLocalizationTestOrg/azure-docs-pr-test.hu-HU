---
title: "Automatikus méretezése a felhőszolgáltatást a portálon |} Microsoft Docs"
description: "Megtudhatja, hogyan használhatja a portált egy felhőalapú szolgáltatás webes szerepkör vagy a feldolgozói szerepkör automatikus skálázási szabályok konfigurálása az Azure-ban."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 701d4404-5cc0-454b-999c-feb94c1685c0
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: e9683d4c5779450fd67fa42ab13095c7f201b4cd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-configure-auto-scaling-for-a-cloud-service-in-the-portal"></a><span data-ttu-id="32f1a-103">Az automatikus skálázás egy felhőalapú szolgáltatás, a portál konfigurálása</span><span class="sxs-lookup"><span data-stu-id="32f1a-103">How to configure auto scaling for a Cloud Service in the portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="32f1a-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="32f1a-104">Azure portal</span></span>](cloud-services-how-to-scale-portal.md)
> * [<span data-ttu-id="32f1a-105">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="32f1a-105">Azure classic portal</span></span>](cloud-services-how-to-scale.md)

<span data-ttu-id="32f1a-106">Egy felhőalapú szolgáltatás feldolgozói szerepkör terjedő skálán bejövő vagy kimenő műveletet kiváltó feltételek állíthat be.</span><span class="sxs-lookup"><span data-stu-id="32f1a-106">Conditions can be set for a cloud service worker role that trigger a scale in or out operation.</span></span> <span data-ttu-id="32f1a-107">A feltételek a szerepkör a Processzor, lemez vagy a szerepkör a hálózati terhelést alapulhatnak.</span><span class="sxs-lookup"><span data-stu-id="32f1a-107">The conditions for the role can be based on the CPU, disk, or network load of the role.</span></span> <span data-ttu-id="32f1a-108">Beállíthat egy feltételt, egy üzenet-várólista vagy valamilyen más Azure-erőforrás az Ön előfizetéséhez rendelve metrikája alapján is.</span><span class="sxs-lookup"><span data-stu-id="32f1a-108">You can also set a condition based on a message queue or the metric of some other Azure resource associated with your subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="32f1a-109">Ez a cikk a felhőalapú szolgáltatás webes és feldolgozói szerepkörök összpontosít.</span><span class="sxs-lookup"><span data-stu-id="32f1a-109">This article focuses on Cloud Service web and worker roles.</span></span> <span data-ttu-id="32f1a-110">A virtuális gép (klasszikus) közvetlenül létrehozásakor tárolja egy felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="32f1a-110">When you create a virtual machine (classic) directly, it is hosted in a cloud service.</span></span> <span data-ttu-id="32f1a-111">A normál virtuális gépek méretezheti társítja azt egy [rendelkezésre állási csoport](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) és manuálisan kapcsolja be és ki.</span><span class="sxs-lookup"><span data-stu-id="32f1a-111">You can scale a standard virtual machine by associating it with an [availability set](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) and manually turn them on or off.</span></span>

## <a name="considerations"></a><span data-ttu-id="32f1a-112">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="32f1a-112">Considerations</span></span>
<span data-ttu-id="32f1a-113">Az alkalmazás skálázás konfigurálása előtt gondolja át a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="32f1a-113">You should consider the following information before you configure scaling for your application:</span></span>

* <span data-ttu-id="32f1a-114">Skálázás alapvető használati van hatással.</span><span class="sxs-lookup"><span data-stu-id="32f1a-114">Scaling is affected by core usage.</span></span>

    <span data-ttu-id="32f1a-115">Nagyobb szerepkörpéldányokat több magot használja.</span><span class="sxs-lookup"><span data-stu-id="32f1a-115">Larger role instances use more cores.</span></span> <span data-ttu-id="32f1a-116">Csak a vonatkozó felső korlátját: belül az előfizetéshez tartozó méretezheti.</span><span class="sxs-lookup"><span data-stu-id="32f1a-116">You can scale an application only within the limit of cores for your subscription.</span></span> <span data-ttu-id="32f1a-117">Tegyük fel, hogy előfizetéséhez maximális hossza 20 magok.</span><span class="sxs-lookup"><span data-stu-id="32f1a-117">For example, say your subscription has a limit of 20 cores.</span></span> <span data-ttu-id="32f1a-118">Ha egy alkalmazás két közepes méretű felhőszolgáltatások (4 mag összesen) futtatja, csak legfeljebb más felhőalapú szolgáltatás központi az előfizetésben a fennmaradó 16 maggal által.</span><span class="sxs-lookup"><span data-stu-id="32f1a-118">If you run an application with two medium-sized cloud services (a total of 4 cores), you can only scale up other cloud service deployments in your subscription by the remaining 16 cores.</span></span> <span data-ttu-id="32f1a-119">Méretek kapcsolatos további információkért lásd: [Felhőszolgáltatások méretével](cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="32f1a-119">For more information about sizes, see [Cloud Service Sizes](cloud-services-sizes-specs.md).</span></span>

* <span data-ttu-id="32f1a-120">Méretezheti a várólista állapotüzenet küszöbértéke alapján.</span><span class="sxs-lookup"><span data-stu-id="32f1a-120">You can scale based on a queue message threshold.</span></span> <span data-ttu-id="32f1a-121">Üzenetsorok használatával kapcsolatos további információkért lásd: [használata a Queue Storage szolgáltatás](../storage/queues/storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="32f1a-121">For more information about how to use queues, see [How to use the Queue Storage Service](../storage/queues/storage-dotnet-how-to-use-queues.md).</span></span>

* <span data-ttu-id="32f1a-122">További erőforrások, az Ön előfizetéséhez rendelve is méretezheti.</span><span class="sxs-lookup"><span data-stu-id="32f1a-122">You can also scale other resources associated with your subscription.</span></span>

* <span data-ttu-id="32f1a-123">Ahhoz, hogy az alkalmazás magas rendelkezésre állású, akkor győződjön meg arról, hogy két vagy több szerepkör osztályt telepítették.</span><span class="sxs-lookup"><span data-stu-id="32f1a-123">To enable high availability of your application, you should ensure that it is deployed with two or more role instances.</span></span> <span data-ttu-id="32f1a-124">További információkért lásd: [szolgáltatói szerződések](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="32f1a-124">For more information, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>


## <a name="where-scale-is-located"></a><span data-ttu-id="32f1a-125">Ahol a méretezési</span><span class="sxs-lookup"><span data-stu-id="32f1a-125">Where scale is located</span></span>
<span data-ttu-id="32f1a-126">Miután kiválasztotta a felhőalapú szolgáltatás, rendelkeznie kell a cloud service panelen látható.</span><span class="sxs-lookup"><span data-stu-id="32f1a-126">After you select your cloud service, you should have the cloud service blade visible.</span></span>

1. <span data-ttu-id="32f1a-127">A felhőalapú szolgáltatás panelen a a **szerepkörök és példányok** csempére, válassza ki a felhőalapú szolgáltatás nevét.</span><span class="sxs-lookup"><span data-stu-id="32f1a-127">On the cloud service blade, on the **Roles and Instances** tile, select the name of the cloud service.</span></span>   
   <span data-ttu-id="32f1a-128">**FONTOS**: Ügyeljen arra, hogy a felhő szerepkör-szolgáltatás, nem a példányon, amely alatt a szerepkör kattintson.</span><span class="sxs-lookup"><span data-stu-id="32f1a-128">**IMPORTANT**: Make sure to click the cloud service role, not the role instance that is below the role.</span></span>

    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)
2. <span data-ttu-id="32f1a-129">Válassza ki a **méretezési** csempére.</span><span class="sxs-lookup"><span data-stu-id="32f1a-129">Select the **scale** tile.</span></span>

    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## <a name="automatic-scale"></a><span data-ttu-id="32f1a-130">Automatikus méretezése</span><span class="sxs-lookup"><span data-stu-id="32f1a-130">Automatic scale</span></span>
<span data-ttu-id="32f1a-131">A szerepkörhöz tartozó skálázási beállítások vagy két mód konfigurálható **manuális** vagy **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="32f1a-131">You can configure scale settings for a role with either two modes **manual** or **automatic**.</span></span> <span data-ttu-id="32f1a-132">Manuális módon teheti meg, beállíthatja a példányainak abszolút számát.</span><span class="sxs-lookup"><span data-stu-id="32f1a-132">Manual is as you would expect, you set the absolute count of instances.</span></span> <span data-ttu-id="32f1a-133">Automatikus azonban lehetővé teszi, hogy a készlet meghatározó szabályok és hogyan milyen sokkal meg kell méretezni.</span><span class="sxs-lookup"><span data-stu-id="32f1a-133">Automatic however allows you to set rules that govern how and by how much you should scale.</span></span>

<span data-ttu-id="32f1a-134">Állítsa be a **szerint** lehetőséggel **ütemezés és a teljesítmény szabályok**.</span><span class="sxs-lookup"><span data-stu-id="32f1a-134">Set the **Scale by** option to **schedule and performance rules**.</span></span>

![Cloud services skálázási beállításokat a profil és a szabály](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. <span data-ttu-id="32f1a-136">Egy meglévő profilt.</span><span class="sxs-lookup"><span data-stu-id="32f1a-136">An existing profile.</span></span>
2. <span data-ttu-id="32f1a-137">Vegye fel a szülő profil szabályt.</span><span class="sxs-lookup"><span data-stu-id="32f1a-137">Add a rule for the parent profile.</span></span>
3. <span data-ttu-id="32f1a-138">Egy másik profil hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="32f1a-138">Add another profile.</span></span>

<span data-ttu-id="32f1a-139">Válassza ki **profil hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="32f1a-139">Select **Add Profile**.</span></span> <span data-ttu-id="32f1a-140">A profil határozza meg, melyik skála használni kívánt módot: **mindig**, **ismétlődési**, **rögzített dátum**.</span><span class="sxs-lookup"><span data-stu-id="32f1a-140">The profile determines which mode you want to use for the scale: **always**, **recurrence**, **fixed date**.</span></span>

<span data-ttu-id="32f1a-141">Miután konfigurálta a profil és a szabályokat, válassza ki a **mentése** ikonra a bal felső.</span><span class="sxs-lookup"><span data-stu-id="32f1a-141">After you have configured the profile and rules, select the **Save** icon at the top.</span></span>

#### <a name="profile"></a><span data-ttu-id="32f1a-142">Profil</span><span class="sxs-lookup"><span data-stu-id="32f1a-142">Profile</span></span>
<span data-ttu-id="32f1a-143">A minimális és maximális példányainak a méretezési készlet, és emellett Ha a tartomány active.</span><span class="sxs-lookup"><span data-stu-id="32f1a-143">The profile sets minimum and maximum instances for the scale, and also when this scale range is active.</span></span>

* <span data-ttu-id="32f1a-144">**Mindig**</span><span class="sxs-lookup"><span data-stu-id="32f1a-144">**Always**</span></span>

    <span data-ttu-id="32f1a-145">Ez a tartomány elérhető példányok mindig készüljön.</span><span class="sxs-lookup"><span data-stu-id="32f1a-145">Always keep this range of instances available.</span></span>  

    ![A felhőalapú szolgáltatás, amely mindig](./media/cloud-services-how-to-scale-portal/select-always.png)
* <span data-ttu-id="32f1a-147">**Ismétlődés**</span><span class="sxs-lookup"><span data-stu-id="32f1a-147">**Recurrence**</span></span>

    <span data-ttu-id="32f1a-148">Válasszon olyan napokat a hét méretezése.</span><span class="sxs-lookup"><span data-stu-id="32f1a-148">Choose a set of days of the week to scale.</span></span>

    ![Cloud service méretezéssel ismétlődési ütemezés](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
* <span data-ttu-id="32f1a-150">**Rögzített dátum**</span><span class="sxs-lookup"><span data-stu-id="32f1a-150">**Fixed Date**</span></span>

    <span data-ttu-id="32f1a-151">Rögzített dátumtartomány méretezése a szerepkört.</span><span class="sxs-lookup"><span data-stu-id="32f1a-151">A fixed date range to scale the role.</span></span>

    ![CLoud service méretezési fix dátummal](./media/cloud-services-how-to-scale-portal/select-fixed.png)

<span data-ttu-id="32f1a-153">Miután konfigurálta a profil, válassza ki a **OK** gombra a profil panel alján.</span><span class="sxs-lookup"><span data-stu-id="32f1a-153">After you have configured the profile, select the **OK** button at the bottom of the profile blade.</span></span>

#### <a name="rule"></a><span data-ttu-id="32f1a-154">Szabály</span><span class="sxs-lookup"><span data-stu-id="32f1a-154">Rule</span></span>
<span data-ttu-id="32f1a-155">Szabályok profil adnak, és megfelelnek a skála kiváltó feltétel.</span><span class="sxs-lookup"><span data-stu-id="32f1a-155">Rules are added to a profile and represent a condition that triggers the scale.</span></span>

<span data-ttu-id="32f1a-156">A szabály eseményindító a felhőszolgáltatás (CPU-használat, lemezre tevékenységi vagy hálózati tevékenységek), amelyhez egy feltételes értéket adhat hozzá metrika alapul.</span><span class="sxs-lookup"><span data-stu-id="32f1a-156">The rule trigger is based on a metric of the cloud service (CPU usage, disk activity, or network activity) to which you can add a conditional value.</span></span> <span data-ttu-id="32f1a-157">Az üzenet-várólista vagy valamilyen más Azure-erőforrás az Ön előfizetéséhez rendelve metrikája alapján eseményindító továbbá akkor is.</span><span class="sxs-lookup"><span data-stu-id="32f1a-157">Additionally you can have the trigger based on a message queue or the metric of some other Azure resource associated with your subscription.</span></span>

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

<span data-ttu-id="32f1a-158">Miután konfigurálta a szabályt, válassza ki a **OK** gombra a szabály panel alján.</span><span class="sxs-lookup"><span data-stu-id="32f1a-158">After you have configured the rule, select the **OK** button at the bottom of the rule blade.</span></span>

## <a name="back-to-manual-scale"></a><span data-ttu-id="32f1a-159">Vissza a manuális méretezési</span><span class="sxs-lookup"><span data-stu-id="32f1a-159">Back to manual scale</span></span>
<span data-ttu-id="32f1a-160">Keresse meg a [méretezési beállításainak](#where-scale-is-located) és állítsa be a **szerint** lehetőséggel **manuálisan megadott példányszám**.</span><span class="sxs-lookup"><span data-stu-id="32f1a-160">Navigate to the [scale settings](#where-scale-is-located) and set the **Scale by** option to **an instance count that I enter manually**.</span></span>

![Cloud services skálázási beállításokat a profil és a szabály](./media/cloud-services-how-to-scale-portal/manual-basics.png)

<span data-ttu-id="32f1a-162">Ez a beállítás eltávolítja az automatikus skálázás a szerepkörből, és állíthat be a példányszám közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="32f1a-162">This setting removes automated scaling from the role and then you can set the instance count directly.</span></span>

1. <span data-ttu-id="32f1a-163">A skála (Manuális vagy automatikus) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="32f1a-163">The scale (manual or automated) option.</span></span>
2. <span data-ttu-id="32f1a-164">Egy szerepkör példány csúszkát beállítása a példányok számára.</span><span class="sxs-lookup"><span data-stu-id="32f1a-164">A role instance slider to set the instances to scale to.</span></span>
3. <span data-ttu-id="32f1a-165">Méretezhető, hogy a szerepkör példánya.</span><span class="sxs-lookup"><span data-stu-id="32f1a-165">Instances of the role to scale to.</span></span>

<span data-ttu-id="32f1a-166">Miután konfigurálta a skálázási beállításokat, válassza ki a **mentése** ikonra a bal felső.</span><span class="sxs-lookup"><span data-stu-id="32f1a-166">After you have configured the scale settings, select the **Save** icon at the top.</span></span>
