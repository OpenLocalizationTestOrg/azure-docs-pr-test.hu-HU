---
title: "aaaAuto felhőalapú szolgáltatásokat méretezni hello klasszikus portál |} Microsoft Docs"
description: "(klasszikus) Ismerje meg, hogyan toouse hello klasszikus portál tooconfigure automatikus skálázási szabályok felhő szerepkör-szolgáltatás webes vagy feldolgozói szerepkör az Azure-ban."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: eb167d70-4eba-42a4-b157-d8d0688abf4b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: ddb5816d4d22192c6d2f51d7508e390779742078
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-auto-scaling-for-a-cloud-service-in-hello-classic-portal"></a><span data-ttu-id="c7ae5-103">Hogyan tooconfigure automatikus skálázás egy felhőalapú szolgáltatás hello klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="c7ae5-103">How tooconfigure auto scaling for a Cloud Service in hello classic portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c7ae5-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c7ae5-104">Azure portal</span></span>](cloud-services-how-to-scale-portal.md)
> * [<span data-ttu-id="c7ae5-105">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="c7ae5-105">Azure classic portal</span></span>](cloud-services-how-to-scale.md)

<span data-ttu-id="c7ae5-106">A klasszikus Azure portálon hello hello méretezési lapján konfigurálhatja a webes szerepkör vagy a feldolgozói szerepkör automatikus méretezési beállításainak.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-106">On hello Scale page of hello Azure classic portal, you can configure automatic scale settings for your web role or worker role.</span></span> <span data-ttu-id="c7ae5-107">Beállíthatja azt is megteheti, manuális skálázás automatikus skálázás szabályalapú helyett.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-107">Alternatively, you can configure manual scaling instead of rules-based automatic scaling.</span></span>

> [!NOTE]
> <span data-ttu-id="c7ae5-108">Ez a cikk a felhőalapú szolgáltatás webes és feldolgozói szerepkörök összpontosít.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-108">This article focuses on Cloud Service web and worker roles.</span></span> <span data-ttu-id="c7ae5-109">A virtuális gép (klasszikus) közvetlenül létrehozásakor tárolja egy felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-109">When you create a virtual machine (classic) directly, it is hosted in a cloud service.</span></span> <span data-ttu-id="c7ae5-110">Az információk érvényes toothese típusú virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-110">Some of this information applies toothese types of virtual machines.</span></span> <span data-ttu-id="c7ae5-111">Skálázás egy rendelkezésre állási csoportot a virtuális gépek csak leáll őket be- és kikapcsolását konfigurálnia hello skálázási szabályok alapján.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-111">Scaling an availability set of virtual machines is just shutting them on and off based on hello scale rules you configure.</span></span> <span data-ttu-id="c7ae5-112">További információ a virtuális gépek és a rendelkezésre állási csoportok: [kezelése hello rendelkezésre állású virtuális gépek](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="c7ae5-112">For more information about Virtual Machines and availability sets, see [Manage hello Availability of Virtual Machines](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

<span data-ttu-id="c7ae5-113">A következő információkat az alkalmazás skálázás konfigurálása előtt hello kell figyelembe vennie:</span><span class="sxs-lookup"><span data-stu-id="c7ae5-113">You should consider hello following information before you configure scaling for your application:</span></span>

* <span data-ttu-id="c7ae5-114">Skálázás alapvető használati van hatással.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-114">Scaling is affected by core usage.</span></span>

    <span data-ttu-id="c7ae5-115">Nagyobb szerepkörpéldányokat több magot használja.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-115">Larger role instances use more cores.</span></span> <span data-ttu-id="c7ae5-116">Az előfizetéshez tartozó méretezhető hello vonatkozó felső korlátját: belül csak egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-116">You can scale an application only within hello limit of cores for your subscription.</span></span> <span data-ttu-id="c7ae5-117">Tegyük fel, hogy előfizetéséhez maximális hossza 20 magok.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-117">For example, say your subscription has a limit of 20 cores.</span></span> <span data-ttu-id="c7ae5-118">Ha egy alkalmazás két közepes méretű felhőszolgáltatások (4 mag összesen) futtatja, csak legfeljebb más felhőalapú szolgáltatás telepítések az előfizetésében hello fennmaradó 16 maggal által.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-118">If you run an application with two medium-sized cloud services (a total of 4 cores), you can only scale up other cloud service deployments in your subscription by hello remaining 16 cores.</span></span> <span data-ttu-id="c7ae5-119">Méretek kapcsolatos további információkért lásd: [Felhőszolgáltatások méretével](cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="c7ae5-119">For more information about sizes, see [Cloud Service Sizes](cloud-services-sizes-specs.md).</span></span>

* <span data-ttu-id="c7ae5-120">Létrehoz egy sort kell, és társíthatja egy szerepkör előtt az alkalmazást egy állapotüzenet küszöbértéke a alapján méretezheti.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-120">You must create a queue and associate it with a role before you can scale an application based on a message threshold.</span></span> <span data-ttu-id="c7ae5-121">További információkért lásd: [hogyan toouse hello Queue Storage szolgáltatás](../storage/queues/storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="c7ae5-121">For more information, see [How toouse hello Queue Storage Service](../storage/queues/storage-dotnet-how-to-use-queues.md).</span></span>

* <span data-ttu-id="c7ae5-122">Csatolt tooyour erőforrásokhoz is méretezhető felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-122">You can scale resources that are linked tooyour cloud service.</span></span> <span data-ttu-id="c7ae5-123">Erőforrások csatolása kapcsolatos további információkért lásd: [hogyan: hivatkozás erőforrás tooa felhőszolgáltatás](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="c7ae5-123">For more information about linking resources, see [How to: Link a resource tooa cloud service](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).</span></span>

* <span data-ttu-id="c7ae5-124">tooenable magas rendelkezésre állás az alkalmazás, akkor győződjön meg arról, hogy két vagy több szerepkör osztályt telepítették.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-124">tooenable high availability of your application, you should ensure that it is deployed with two or more role instances.</span></span> <span data-ttu-id="c7ae5-125">További információkért lásd: [szolgáltatói szerződések](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="c7ae5-125">For more information, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>

## <a name="schedule-scaling"></a><span data-ttu-id="c7ae5-126">Ütemezés skálázás</span><span class="sxs-lookup"><span data-stu-id="c7ae5-126">Schedule scaling</span></span>
<span data-ttu-id="c7ae5-127">Alapértelmezés szerint az összes szerepkör ne hajtsa végre egy adott ütemezés.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-127">By default, all roles do not follow a specific schedule.</span></span> <span data-ttu-id="c7ae5-128">Ezért a beállításokat módosítani alkalmazni tooall ideje és minden nap hello év során.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-128">Therefore, any settings changed apply tooall times and all days throughout hello year.</span></span> <span data-ttu-id="c7ae5-129">Ha azt szeretné, beállíthatja manuális vagy automatikus skálázás hello módok a következő egyike:</span><span class="sxs-lookup"><span data-stu-id="c7ae5-129">If you want, you can setup manual or automatic scaling for one of hello following modes:</span></span>

* <span data-ttu-id="c7ae5-130">Hétköznapokon</span><span class="sxs-lookup"><span data-stu-id="c7ae5-130">Weekdays</span></span>
* <span data-ttu-id="c7ae5-131">Hétvégéket</span><span class="sxs-lookup"><span data-stu-id="c7ae5-131">Weekends</span></span>
* <span data-ttu-id="c7ae5-132">Hét éjszakák</span><span class="sxs-lookup"><span data-stu-id="c7ae5-132">Week nights</span></span>
* <span data-ttu-id="c7ae5-133">Hét reggelente</span><span class="sxs-lookup"><span data-stu-id="c7ae5-133">Week mornings</span></span>
* <span data-ttu-id="c7ae5-134">Konkrét dátumokat</span><span class="sxs-lookup"><span data-stu-id="c7ae5-134">Specific dates</span></span>
* <span data-ttu-id="c7ae5-135">Adott dátumtartományok</span><span class="sxs-lookup"><span data-stu-id="c7ae5-135">Specific date ranges</span></span>

<span data-ttu-id="c7ae5-136">hello ütemezési beállítás konfigurálva van a hello [a klasszikus Azure portálon](https://manage.windowsazure.com/) a hello</span><span class="sxs-lookup"><span data-stu-id="c7ae5-136">hello schedule setting is configured in hello [Azure classic portal](https://manage.windowsazure.com/) on hello</span></span>  
<span data-ttu-id="c7ae5-137">**A felhőalapú szolgáltatások** > **\[a felhőalapú szolgáltatás\]** > **méretezési** > **\[üzemi és átmeneti\]**  lap.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-137">**Cloud Services** > **\[Your cloud service\]** > **Scale** > **\[Production or Staging\]** page.</span></span>

<span data-ttu-id="c7ae5-138">Kattintson a hello **ütemezés beállítása** gomb szerepkörönként toochange keresi.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-138">Click hello **set up schedule times** button for each role you want toochange.</span></span>

![A felhőalapú szolgáltatás ütemezés alapján automatikus skálázás][scale_schedules]

## <a name="manual-scale"></a><span data-ttu-id="c7ae5-140">Manuális méretezési</span><span class="sxs-lookup"><span data-stu-id="c7ae5-140">Manual scale</span></span>
<span data-ttu-id="c7ae5-141">A hello **méretezési** lapon, akkor manuálisan növelhető és csökkenthető futó példány egy felhőalapú szolgáltatás hello száma.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-141">On hello **Scale** page, you can manually increase or decrease hello number of running instances in a cloud service.</span></span> <span data-ttu-id="c7ae5-142">Ez a beállítás minden létrehozott ütemezést vagy tooall idő van konfigurálva, ha nem hozott létre ütemezés.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-142">This setting is configured for each schedule you've created or tooall time if you have not created a schedule.</span></span>

1. <span data-ttu-id="c7ae5-143">A hello [a klasszikus Azure portálon](https://manage.windowsazure.com/), kattintson a **Felhőszolgáltatások**, majd hello cloud service tooopen hello irányítópult hello nevére.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-143">In hello [Azure classic portal](https://manage.windowsazure.com/), click **Cloud Services**, and then click hello name of hello cloud service tooopen hello dashboard.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="c7ae5-144">Ha nem látja a felhőalapú szolgáltatás, szükség lehet a toochange **éles** túl**átmeneti** vagy fordítva.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-144">If you don't see your cloud service, you may need toochange from **Production** too**Staging** or vice versa.</span></span>

2. <span data-ttu-id="c7ae5-145">Kattintson a **méretezési**.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-145">Click **Scale**.</span></span>
3. <span data-ttu-id="c7ae5-146">Válassza ki a kívánt skálázás beállításai toochange hello ütemezés.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-146">Select hello schedule you want toochange scaling options for.</span></span> <span data-ttu-id="c7ae5-147">*Nem ütemezett időpontokban* hello alapértelmezett van, ha még nem definiált ütemezések.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-147">*No scheduled times* is hello default if you have no schedules defined.</span></span>
4. <span data-ttu-id="c7ae5-148">Hello található **a metrika a skála** válassza ki azt **NONE**.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-148">Find hello **Scale by metric** section and select **NONE**.</span></span> <span data-ttu-id="c7ae5-149">Ez a beállítás akkor hello alapértelmezett összes szerepköre tekintetében.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-149">This setting is hello default for all roles.</span></span>
5. <span data-ttu-id="c7ae5-150">Hello felhőalapú szolgáltatás minden szerepkörhöz a csúszkán példányok toouse hello számának módosítása.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-150">Each role in hello cloud service has a slider for changing hello number of instances toouse.</span></span>
   
    ![Manuálisan méretezhető, a felhő szerepkör-szolgáltatás][manual_scale]
   
    <span data-ttu-id="c7ae5-152">Ha több példány van szüksége, szükség lehet a toochange hello [a felhő virtuálisgép-méret](cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="c7ae5-152">If you need more instances, you may need toochange hello [cloud service virtual machine size](cloud-services-sizes-specs.md).</span></span>
6. <span data-ttu-id="c7ae5-153">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-153">Click **Save**.</span></span>  
   <span data-ttu-id="c7ae5-154">Szerepkörpéldányokat hozzáadásakor vagy eltávolításakor a kiválasztott beállítások alapján.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-154">Role instances are added or removed based on your selections.</span></span>

> [!TIP]
> <span data-ttu-id="c7ae5-155">Amikor látja ![][tip_icon] áthelyezése az egér tooit és kaphat arról, hogy milyen egy adott beállítás.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-155">Whenever you see ![][tip_icon] move your mouse tooit and you can get help about what a specific setting does.</span></span>

## <a name="automatic-scale---cpu"></a><span data-ttu-id="c7ae5-156">Automatikus méretezési - Processzor</span><span class="sxs-lookup"><span data-stu-id="c7ae5-156">Automatic scale - CPU</span></span>
<span data-ttu-id="c7ae5-157">Ebben a módban arányosan, ha hello átlagos CPU-használat aránya a megadott küszöbérték alatti vagy feletti kerül.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-157">This mode scales if hello average percentage of CPU usage goes above or below specified thresholds.</span></span> <span data-ttu-id="c7ae5-158">Szerepkörpéldányokat létrehozott vagy törölt, ha ez történik.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-158">Role instances are created or deleted when this happens.</span></span>

1. <span data-ttu-id="c7ae5-159">A hello [a klasszikus Azure portálon](https://manage.windowsazure.com/), kattintson a **Felhőszolgáltatások**, majd hello cloud service tooopen hello irányítópult hello nevére.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-159">In hello [Azure classic portal](https://manage.windowsazure.com/), click **Cloud Services**, and then click hello name of hello cloud service tooopen hello dashboard.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="c7ae5-160">Ha nem látja a felhőalapú szolgáltatás, szükség lehet a toochange **éles** túl**átmeneti** vagy fordítva.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-160">If you don't see your cloud service, you may need toochange from **Production** too**Staging** or vice versa.</span></span>

2. <span data-ttu-id="c7ae5-161">Kattintson a **méretezési**.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-161">Click **Scale**.</span></span>
3. <span data-ttu-id="c7ae5-162">Válassza ki a kívánt skálázás beállításai toochange hello ütemezés.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-162">Select hello schedule you want toochange scaling options for.</span></span> <span data-ttu-id="c7ae5-163">*Nem ütemezett időpontokban* hello alapértelmezett van, ha még nem definiált ütemezések.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-163">*No scheduled times* is hello default if you have no schedules defined.</span></span>
4. <span data-ttu-id="c7ae5-164">Hello található **a metrika a skála** válassza ki azt **CPU**.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-164">Find hello **Scale by metric** section and select **CPU**.</span></span>
5. <span data-ttu-id="c7ae5-165">Most minimális és maximális számos szerepkörök példányai, hello cél CPU-használat (a skálától beállítható tootrigger), és hogy hány példánya tooscale felfelé és lefelé által konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-165">Now you can configure a minimum and maximum range of roles instances, hello target CPU usage (tootrigger a scale up), and how many instances tooscale up and down by.</span></span>

![Bővítse a cpu-terhelés a felhőalapú szolgáltatás szerepkör][cpu_scale]

> [!TIP]
> <span data-ttu-id="c7ae5-167">Amikor látja ![][tip_icon] áthelyezése az egér tooit és kaphat arról, hogy milyen egy adott beállítás.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-167">Whenever you see ![][tip_icon] move your mouse tooit and you can get help about what a specific setting does.</span></span>

## <a name="automatic-scale---queue"></a><span data-ttu-id="c7ae5-168">Automatikus méretezési - várólista</span><span class="sxs-lookup"><span data-stu-id="c7ae5-168">Automatic scale - Queue</span></span>
<span data-ttu-id="c7ae5-169">Ebben a módban automatikusan méretezi, ha a várólistán lévő üzenetek hello száma a megadott küszöbérték alatti vagy feletti kerül.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-169">This mode automatically scales if hello number of messages in a queue goes above or below a specified threshold.</span></span> <span data-ttu-id="c7ae5-170">Szerepkörpéldányokat létrehozott vagy törölt, ha ez történik.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-170">Role instances are created or deleted when this happens.</span></span>

1. <span data-ttu-id="c7ae5-171">A hello [a klasszikus Azure portálon](https://manage.windowsazure.com/), kattintson a **Felhőszolgáltatások**, majd hello cloud service tooopen hello irányítópult hello nevére.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-171">In hello [Azure classic portal](https://manage.windowsazure.com/), click **Cloud Services**, and then click hello name of hello cloud service tooopen hello dashboard.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="c7ae5-172">Ha nem látja a felhőalapú szolgáltatás, szükség lehet a toochange **éles** túl**átmeneti** vagy fordítva.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-172">If you don't see your cloud service, you may need toochange from **Production** too**Staging** or vice versa.</span></span>

2. <span data-ttu-id="c7ae5-173">Kattintson a **méretezési**.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-173">Click **Scale**.</span></span>
3. <span data-ttu-id="c7ae5-174">Hello található **a metrika a skála** válassza ki azt **VÁRÓLISTA**.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-174">Find hello **Scale by metric** section and select **QUEUE**.</span></span>
4. <span data-ttu-id="c7ae5-175">Most szerepkörök példányai, hello várólista és az egyes példányok és által növekszik és csökken hány példányok tooscale várólista-üzenetek tooprocess száma minimális és maximális számos is konfigurálhat.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-175">Now you can configure a minimum and maximum range of roles instances, hello queue and number of queue messages tooprocess for each instance, and how many instances tooscale up and down by.</span></span>

![Bővítse a felhőalapú szolgáltatás szerepkör üzenet-várólista][queue_scale]

> [!TIP]
> <span data-ttu-id="c7ae5-177">Amikor látja ![][tip_icon] áthelyezése az egér tooit és kaphat arról, hogy milyen egy adott beállítás.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-177">Whenever you see ![][tip_icon] move your mouse tooit and you can get help about what a specific setting does.</span></span>

## <a name="scale-linked-resources"></a><span data-ttu-id="c7ae5-178">Méretezhető kapcsolt erőforrásokban</span><span class="sxs-lookup"><span data-stu-id="c7ae5-178">Scale linked resources</span></span>
<span data-ttu-id="c7ae5-179">Gyakran szerepkör méretezéséhez esetén hasznos tooscale hello adatbázis hello alkalmazás által is használt.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-179">Often when you scale a role, it's beneficial tooscale hello database that hello application is using also.</span></span> <span data-ttu-id="c7ae5-180">Ha hello adatbázis toohello felhőalapú szolgáltatás, hello-beállítások az adott erőforrás skálázás hello megfelelő hivatkozásra kattintva érheti el.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-180">If you link hello database toohello cloud service, you can access hello scaling settings for that resource by clicking hello appropriate link.</span></span>

1. <span data-ttu-id="c7ae5-181">A hello [a klasszikus Azure portálon](https://manage.windowsazure.com/), kattintson a **Felhőszolgáltatások**, majd hello cloud service tooopen hello irányítópult hello nevére.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-181">In hello [Azure classic portal](https://manage.windowsazure.com/), click **Cloud Services**, and then click hello name of hello cloud service tooopen hello dashboard.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="c7ae5-182">Ha nem látja a felhőalapú szolgáltatás, szükség lehet a toochange **éles** túl**átmeneti** vagy fordítva.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-182">If you don't see your cloud service, you may need toochange from **Production** too**Staging** or vice versa.</span></span>

2. <span data-ttu-id="c7ae5-183">Kattintson a **méretezési**.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-183">Click **Scale**.</span></span>
3. <span data-ttu-id="c7ae5-184">Hello található **kapcsolódó erőforrások** szakaszt, és kattintson **az adatbázis kezelésére**.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-184">Find hello **linked resources** section and click **Manage scale for this database**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="c7ae5-185">Ha nem látja a **kapcsolódó erőforrások** szakaszban, valószínűleg nem rendelkezik társított erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="c7ae5-185">If you don't see a **linked resources** section, you probably do not have any linked resources.</span></span>

![][linked_resource]

[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png
