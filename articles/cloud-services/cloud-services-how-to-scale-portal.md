---
title: "aaaAuto a felhőalapú szolgáltatások méretezéséhez hello portálon |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello portál tooconfigure automatikus skálázási szabályok felhő szerepkör-szolgáltatás webes vagy feldolgozói szerepkör az Azure-ban."
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
ms.openlocfilehash: 265f4c8ec5e1ec2f85585df25f18cd0d0c9946a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-auto-scaling-for-a-cloud-service-in-hello-portal"></a><span data-ttu-id="2b96d-103">Hogyan tooconfigure automatikus skálázás egy felhőalapú szolgáltatás hello portálon</span><span class="sxs-lookup"><span data-stu-id="2b96d-103">How tooconfigure auto scaling for a Cloud Service in hello portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2b96d-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2b96d-104">Azure portal</span></span>](cloud-services-how-to-scale-portal.md)
> * [<span data-ttu-id="2b96d-105">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="2b96d-105">Azure classic portal</span></span>](cloud-services-how-to-scale.md)

<span data-ttu-id="2b96d-106">Egy felhőalapú szolgáltatás feldolgozói szerepkör terjedő skálán bejövő vagy kimenő műveletet kiváltó feltételek állíthat be.</span><span class="sxs-lookup"><span data-stu-id="2b96d-106">Conditions can be set for a cloud service worker role that trigger a scale in or out operation.</span></span> <span data-ttu-id="2b96d-107">hello szerepkör hello feltételei hello Processzor, lemez vagy hálózati terheléselosztási hello szerepkör alapulhatnak.</span><span class="sxs-lookup"><span data-stu-id="2b96d-107">hello conditions for hello role can be based on hello CPU, disk, or network load of hello role.</span></span> <span data-ttu-id="2b96d-108">Egy feltétel alapján néhány más Azure-erőforrás az Ön előfizetéséhez rendelve egy üzenet várólista vagy hello metrikája állíthat be.</span><span class="sxs-lookup"><span data-stu-id="2b96d-108">You can also set a condition based on a message queue or hello metric of some other Azure resource associated with your subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="2b96d-109">Ez a cikk a felhőalapú szolgáltatás webes és feldolgozói szerepkörök összpontosít.</span><span class="sxs-lookup"><span data-stu-id="2b96d-109">This article focuses on Cloud Service web and worker roles.</span></span> <span data-ttu-id="2b96d-110">A virtuális gép (klasszikus) közvetlenül létrehozásakor tárolja egy felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="2b96d-110">When you create a virtual machine (classic) directly, it is hosted in a cloud service.</span></span> <span data-ttu-id="2b96d-111">A normál virtuális gépek méretezheti társítja azt egy [rendelkezésre állási csoport](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) és manuálisan kapcsolja be és ki.</span><span class="sxs-lookup"><span data-stu-id="2b96d-111">You can scale a standard virtual machine by associating it with an [availability set](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) and manually turn them on or off.</span></span>

## <a name="considerations"></a><span data-ttu-id="2b96d-112">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="2b96d-112">Considerations</span></span>
<span data-ttu-id="2b96d-113">A következő információkat az alkalmazás skálázás konfigurálása előtt hello kell figyelembe vennie:</span><span class="sxs-lookup"><span data-stu-id="2b96d-113">You should consider hello following information before you configure scaling for your application:</span></span>

* <span data-ttu-id="2b96d-114">Skálázás alapvető használati van hatással.</span><span class="sxs-lookup"><span data-stu-id="2b96d-114">Scaling is affected by core usage.</span></span>

    <span data-ttu-id="2b96d-115">Nagyobb szerepkörpéldányokat több magot használja.</span><span class="sxs-lookup"><span data-stu-id="2b96d-115">Larger role instances use more cores.</span></span> <span data-ttu-id="2b96d-116">Az előfizetéshez tartozó méretezhető hello vonatkozó felső korlátját: belül csak egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2b96d-116">You can scale an application only within hello limit of cores for your subscription.</span></span> <span data-ttu-id="2b96d-117">Tegyük fel, hogy előfizetéséhez maximális hossza 20 magok.</span><span class="sxs-lookup"><span data-stu-id="2b96d-117">For example, say your subscription has a limit of 20 cores.</span></span> <span data-ttu-id="2b96d-118">Ha egy alkalmazás két közepes méretű felhőszolgáltatások (4 mag összesen) futtatja, csak legfeljebb más felhőalapú szolgáltatás telepítések az előfizetésében hello fennmaradó 16 maggal által.</span><span class="sxs-lookup"><span data-stu-id="2b96d-118">If you run an application with two medium-sized cloud services (a total of 4 cores), you can only scale up other cloud service deployments in your subscription by hello remaining 16 cores.</span></span> <span data-ttu-id="2b96d-119">Méretek kapcsolatos további információkért lásd: [Felhőszolgáltatások méretével](cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="2b96d-119">For more information about sizes, see [Cloud Service Sizes](cloud-services-sizes-specs.md).</span></span>

* <span data-ttu-id="2b96d-120">Méretezheti a várólista állapotüzenet küszöbértéke alapján.</span><span class="sxs-lookup"><span data-stu-id="2b96d-120">You can scale based on a queue message threshold.</span></span> <span data-ttu-id="2b96d-121">További információ toouse sorok, lásd: [hogyan toouse hello Queue Storage szolgáltatás](../storage/queues/storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="2b96d-121">For more information about how toouse queues, see [How toouse hello Queue Storage Service](../storage/queues/storage-dotnet-how-to-use-queues.md).</span></span>

* <span data-ttu-id="2b96d-122">További erőforrások, az Ön előfizetéséhez rendelve is méretezheti.</span><span class="sxs-lookup"><span data-stu-id="2b96d-122">You can also scale other resources associated with your subscription.</span></span>

* <span data-ttu-id="2b96d-123">tooenable magas rendelkezésre állás az alkalmazás, akkor győződjön meg arról, hogy két vagy több szerepkör osztályt telepítették.</span><span class="sxs-lookup"><span data-stu-id="2b96d-123">tooenable high availability of your application, you should ensure that it is deployed with two or more role instances.</span></span> <span data-ttu-id="2b96d-124">További információkért lásd: [szolgáltatói szerződések](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="2b96d-124">For more information, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>


## <a name="where-scale-is-located"></a><span data-ttu-id="2b96d-125">Ahol a méretezési</span><span class="sxs-lookup"><span data-stu-id="2b96d-125">Where scale is located</span></span>
<span data-ttu-id="2b96d-126">Miután kiválasztotta a felhőalapú szolgáltatás, rendelkeznie kell hello cloud service panelen látható.</span><span class="sxs-lookup"><span data-stu-id="2b96d-126">After you select your cloud service, you should have hello cloud service blade visible.</span></span>

1. <span data-ttu-id="2b96d-127">Hello cloud service panelen, a hello **szerepkörök és példányok** csempe, jelölje be hello neve hello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="2b96d-127">On hello cloud service blade, on hello **Roles and Instances** tile, select hello name of hello cloud service.</span></span>   
   <span data-ttu-id="2b96d-128">**FONTOS**: Győződjön meg arról, hogy tooclick hello felhő szerepkör-szolgáltatás, nem hello szerepkörpéldányt, amely hello szerepkör alatt.</span><span class="sxs-lookup"><span data-stu-id="2b96d-128">**IMPORTANT**: Make sure tooclick hello cloud service role, not hello role instance that is below hello role.</span></span>

    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)
2. <span data-ttu-id="2b96d-129">Jelölje be hello **méretezési** csempére.</span><span class="sxs-lookup"><span data-stu-id="2b96d-129">Select hello **scale** tile.</span></span>

    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## <a name="automatic-scale"></a><span data-ttu-id="2b96d-130">Automatikus méretezése</span><span class="sxs-lookup"><span data-stu-id="2b96d-130">Automatic scale</span></span>
<span data-ttu-id="2b96d-131">A szerepkörhöz tartozó skálázási beállítások vagy két mód konfigurálható **manuális** vagy **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="2b96d-131">You can configure scale settings for a role with either two modes **manual** or **automatic**.</span></span> <span data-ttu-id="2b96d-132">Manuális módon teheti meg, beállíthatja a hello abszolút példányainak számát.</span><span class="sxs-lookup"><span data-stu-id="2b96d-132">Manual is as you would expect, you set hello absolute count of instances.</span></span> <span data-ttu-id="2b96d-133">Automatikus azonban lehetővé teszi a tooset meghatározó szabályok és hogyan milyen sokkal meg kell méretezni.</span><span class="sxs-lookup"><span data-stu-id="2b96d-133">Automatic however allows you tooset rules that govern how and by how much you should scale.</span></span>

<span data-ttu-id="2b96d-134">Set hello **szerint** beállítás túl**ütemezés és a teljesítmény szabályok**.</span><span class="sxs-lookup"><span data-stu-id="2b96d-134">Set hello **Scale by** option too**schedule and performance rules**.</span></span>

![Cloud services skálázási beállításokat a profil és a szabály](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. <span data-ttu-id="2b96d-136">Egy meglévő profilt.</span><span class="sxs-lookup"><span data-stu-id="2b96d-136">An existing profile.</span></span>
2. <span data-ttu-id="2b96d-137">Vegyen fel egy szülő hello-profil.</span><span class="sxs-lookup"><span data-stu-id="2b96d-137">Add a rule for hello parent profile.</span></span>
3. <span data-ttu-id="2b96d-138">Egy másik profil hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="2b96d-138">Add another profile.</span></span>

<span data-ttu-id="2b96d-139">Válassza ki **profil hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="2b96d-139">Select **Add Profile**.</span></span> <span data-ttu-id="2b96d-140">hello-profil határozza meg, melyik módban toouse kívánt hello bővített: **mindig**, **ismétlődési**, **rögzített dátum**.</span><span class="sxs-lookup"><span data-stu-id="2b96d-140">hello profile determines which mode you want toouse for hello scale: **always**, **recurrence**, **fixed date**.</span></span>

<span data-ttu-id="2b96d-141">Miután konfigurálta a hello-profil és a szabályokat, válassza ki a hello **mentése** hello felső ikonra.</span><span class="sxs-lookup"><span data-stu-id="2b96d-141">After you have configured hello profile and rules, select hello **Save** icon at hello top.</span></span>

#### <a name="profile"></a><span data-ttu-id="2b96d-142">Profil</span><span class="sxs-lookup"><span data-stu-id="2b96d-142">Profile</span></span>
<span data-ttu-id="2b96d-143">hello-profil beállítja a minimális és maximális példányainak hello méretezhető, illetve is a tartomány használata aktív.</span><span class="sxs-lookup"><span data-stu-id="2b96d-143">hello profile sets minimum and maximum instances for hello scale, and also when this scale range is active.</span></span>

* <span data-ttu-id="2b96d-144">**Mindig**</span><span class="sxs-lookup"><span data-stu-id="2b96d-144">**Always**</span></span>

    <span data-ttu-id="2b96d-145">Ez a tartomány elérhető példányok mindig készüljön.</span><span class="sxs-lookup"><span data-stu-id="2b96d-145">Always keep this range of instances available.</span></span>  

    ![A felhőalapú szolgáltatás, amely mindig](./media/cloud-services-how-to-scale-portal/select-always.png)
* <span data-ttu-id="2b96d-147">**Ismétlődés**</span><span class="sxs-lookup"><span data-stu-id="2b96d-147">**Recurrence**</span></span>

    <span data-ttu-id="2b96d-148">Válasszon olyan hello hét tooscale napon.</span><span class="sxs-lookup"><span data-stu-id="2b96d-148">Choose a set of days of hello week tooscale.</span></span>

    ![Cloud service méretezéssel ismétlődési ütemezés](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
* <span data-ttu-id="2b96d-150">**Rögzített dátum**</span><span class="sxs-lookup"><span data-stu-id="2b96d-150">**Fixed Date**</span></span>

    <span data-ttu-id="2b96d-151">Rögzített dátum tartomány tooscale hello szerepkör.</span><span class="sxs-lookup"><span data-stu-id="2b96d-151">A fixed date range tooscale hello role.</span></span>

    ![CLoud service méretezési fix dátummal](./media/cloud-services-how-to-scale-portal/select-fixed.png)

<span data-ttu-id="2b96d-153">Miután konfigurálta a hello-profil, válassza ki a hello **OK** gomb hello aljához hello-profil panelje.</span><span class="sxs-lookup"><span data-stu-id="2b96d-153">After you have configured hello profile, select hello **OK** button at hello bottom of hello profile blade.</span></span>

#### <a name="rule"></a><span data-ttu-id="2b96d-154">Szabály</span><span class="sxs-lookup"><span data-stu-id="2b96d-154">Rule</span></span>
<span data-ttu-id="2b96d-155">Szabályok tooa profilt ad hozzá, és hello méretezési kiváltó feltétel képviseli.</span><span class="sxs-lookup"><span data-stu-id="2b96d-155">Rules are added tooa profile and represent a condition that triggers hello scale.</span></span>

<span data-ttu-id="2b96d-156">hello szabály eseményindító metrika hello felhőszolgáltatás (CPU-használat, lemezre tevékenységi vagy hálózati tevékenységek) alapuló toowhich feltételes értéket adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="2b96d-156">hello rule trigger is based on a metric of hello cloud service (CPU usage, disk activity, or network activity) toowhich you can add a conditional value.</span></span> <span data-ttu-id="2b96d-157">Továbbá akkor is hello eseményindító néhány más Azure-erőforrás az Ön előfizetéséhez rendelve egy üzenet várólista vagy hello metrikája alapján.</span><span class="sxs-lookup"><span data-stu-id="2b96d-157">Additionally you can have hello trigger based on a message queue or hello metric of some other Azure resource associated with your subscription.</span></span>

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

<span data-ttu-id="2b96d-158">Miután konfigurálta a hello szabály, válassza ki a hello **OK** hello hello szabály panel alsó részén gombra.</span><span class="sxs-lookup"><span data-stu-id="2b96d-158">After you have configured hello rule, select hello **OK** button at hello bottom of hello rule blade.</span></span>

## <a name="back-toomanual-scale"></a><span data-ttu-id="2b96d-159">Biztonsági toomanual méretezési</span><span class="sxs-lookup"><span data-stu-id="2b96d-159">Back toomanual scale</span></span>
<span data-ttu-id="2b96d-160">Keresse meg a toohello [beállítások méretezése](#where-scale-is-located) és set hello **bővítse a** beállítás túl**manuálisan megadott példányszám**.</span><span class="sxs-lookup"><span data-stu-id="2b96d-160">Navigate toohello [scale settings](#where-scale-is-located) and set hello **Scale by** option too**an instance count that I enter manually**.</span></span>

![Cloud services skálázási beállításokat a profil és a szabály](./media/cloud-services-how-to-scale-portal/manual-basics.png)

<span data-ttu-id="2b96d-162">Ez a beállítás eltávolítja az automatikus skálázás hello szerepkörből, és állíthat be hello példányszám közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="2b96d-162">This setting removes automated scaling from hello role and then you can set hello instance count directly.</span></span>

1. <span data-ttu-id="2b96d-163">hello méretezési (Manuális vagy automatikus) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2b96d-163">hello scale (manual or automated) option.</span></span>
2. <span data-ttu-id="2b96d-164">Egy szerepkör példány csúszkát tooset hello példányok tooscale számára.</span><span class="sxs-lookup"><span data-stu-id="2b96d-164">A role instance slider tooset hello instances tooscale to.</span></span>
3. <span data-ttu-id="2b96d-165">Hello szerepkör tooscale a példányai.</span><span class="sxs-lookup"><span data-stu-id="2b96d-165">Instances of hello role tooscale to.</span></span>

<span data-ttu-id="2b96d-166">Miután konfigurálta a hello skálázási beállításokat, válassza ki a hello **mentése** hello felső ikon.</span><span class="sxs-lookup"><span data-stu-id="2b96d-166">After you have configured hello scale settings, select hello **Save** icon at hello top.</span></span>
