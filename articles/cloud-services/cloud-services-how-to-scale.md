---
title: "Automatikus méretezése a felhőszolgáltatást a klasszikus portálon |} Microsoft Docs"
description: "(klasszikus) Megtudhatja, hogyan használja a klasszikus portált automatikus skálázási szabályok felhő szerepkör-szolgáltatás webes vagy feldolgozói szerepkör konfigurálása az Azure-ban."
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
ms.openlocfilehash: 90d55bbac6e113d6add848ace67cf0749e26342b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-configure-auto-scaling-for-a-cloud-service-in-the-classic-portal"></a><span data-ttu-id="2a03e-103">Az automatikus skálázás egy felhőalapú szolgáltatás, a klasszikus portál konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2a03e-103">How to configure auto scaling for a Cloud Service in the classic portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2a03e-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2a03e-104">Azure portal</span></span>](cloud-services-how-to-scale-portal.md)
> * [<span data-ttu-id="2a03e-105">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="2a03e-105">Azure classic portal</span></span>](cloud-services-how-to-scale.md)

<span data-ttu-id="2a03e-106">A klasszikus Azure portálon méretezési oldalán konfigurálhatja a webes vagy feldolgozói szerepkör automatikus méretezési beállításait.</span><span class="sxs-lookup"><span data-stu-id="2a03e-106">On the Scale page of the Azure classic portal, you can configure automatic scale settings for your web role or worker role.</span></span> <span data-ttu-id="2a03e-107">Beállíthatja azt is megteheti, manuális skálázás automatikus skálázás szabályalapú helyett.</span><span class="sxs-lookup"><span data-stu-id="2a03e-107">Alternatively, you can configure manual scaling instead of rules-based automatic scaling.</span></span>

> [!NOTE]
> <span data-ttu-id="2a03e-108">Ez a cikk a felhőalapú szolgáltatás webes és feldolgozói szerepkörök összpontosít.</span><span class="sxs-lookup"><span data-stu-id="2a03e-108">This article focuses on Cloud Service web and worker roles.</span></span> <span data-ttu-id="2a03e-109">A virtuális gép (klasszikus) közvetlenül létrehozásakor tárolja egy felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="2a03e-109">When you create a virtual machine (classic) directly, it is hosted in a cloud service.</span></span> <span data-ttu-id="2a03e-110">Ezen információk némelyikét a virtuális gépek az ilyen típusú vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="2a03e-110">Some of this information applies to these types of virtual machines.</span></span> <span data-ttu-id="2a03e-111">Skálázás egy rendelkezésre állási csoportot a virtuális gépek csak leáll őket be- és kikapcsolását a skálázási szabályok konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="2a03e-111">Scaling an availability set of virtual machines is just shutting them on and off based on the scale rules you configure.</span></span> <span data-ttu-id="2a03e-112">További információ a virtuális gépek és a rendelkezésre állási csoportok: [a virtuális gépek rendelkezésre állásának kezelése](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="2a03e-112">For more information about Virtual Machines and availability sets, see [Manage the Availability of Virtual Machines](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

<span data-ttu-id="2a03e-113">Az alkalmazás skálázás konfigurálása előtt gondolja át a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="2a03e-113">You should consider the following information before you configure scaling for your application:</span></span>

* <span data-ttu-id="2a03e-114">Skálázás alapvető használati van hatással.</span><span class="sxs-lookup"><span data-stu-id="2a03e-114">Scaling is affected by core usage.</span></span>

    <span data-ttu-id="2a03e-115">Nagyobb szerepkörpéldányokat több magot használja.</span><span class="sxs-lookup"><span data-stu-id="2a03e-115">Larger role instances use more cores.</span></span> <span data-ttu-id="2a03e-116">Csak a vonatkozó felső korlátját: belül az előfizetéshez tartozó méretezheti.</span><span class="sxs-lookup"><span data-stu-id="2a03e-116">You can scale an application only within the limit of cores for your subscription.</span></span> <span data-ttu-id="2a03e-117">Tegyük fel, hogy előfizetéséhez maximális hossza 20 magok.</span><span class="sxs-lookup"><span data-stu-id="2a03e-117">For example, say your subscription has a limit of 20 cores.</span></span> <span data-ttu-id="2a03e-118">Ha egy alkalmazás két közepes méretű felhőszolgáltatások (4 mag összesen) futtatja, csak legfeljebb más felhőalapú szolgáltatás központi az előfizetésben a fennmaradó 16 maggal által.</span><span class="sxs-lookup"><span data-stu-id="2a03e-118">If you run an application with two medium-sized cloud services (a total of 4 cores), you can only scale up other cloud service deployments in your subscription by the remaining 16 cores.</span></span> <span data-ttu-id="2a03e-119">Méretek kapcsolatos további információkért lásd: [Felhőszolgáltatások méretével](cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="2a03e-119">For more information about sizes, see [Cloud Service Sizes](cloud-services-sizes-specs.md).</span></span>

* <span data-ttu-id="2a03e-120">Létrehoz egy sort kell, és társíthatja egy szerepkör előtt az alkalmazást egy állapotüzenet küszöbértéke a alapján méretezheti.</span><span class="sxs-lookup"><span data-stu-id="2a03e-120">You must create a queue and associate it with a role before you can scale an application based on a message threshold.</span></span> <span data-ttu-id="2a03e-121">További információkért lásd: [használata a Queue Storage szolgáltatás](../storage/queues/storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="2a03e-121">For more information, see [How to use the Queue Storage Service](../storage/queues/storage-dotnet-how-to-use-queues.md).</span></span>

* <span data-ttu-id="2a03e-122">A felhőalapú szolgáltatáshoz kapcsolódó erőforrások méretezheti.</span><span class="sxs-lookup"><span data-stu-id="2a03e-122">You can scale resources that are linked to your cloud service.</span></span> <span data-ttu-id="2a03e-123">Erőforrások csatolása kapcsolatos további információkért lásd: [hogyan: erőforrás összekapcsolása egy felhőalapú szolgáltatás](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="2a03e-123">For more information about linking resources, see [How to: Link a resource to a cloud service](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).</span></span>

* <span data-ttu-id="2a03e-124">Ahhoz, hogy az alkalmazás magas rendelkezésre állású, akkor győződjön meg arról, hogy két vagy több szerepkör osztályt telepítették.</span><span class="sxs-lookup"><span data-stu-id="2a03e-124">To enable high availability of your application, you should ensure that it is deployed with two or more role instances.</span></span> <span data-ttu-id="2a03e-125">További információkért lásd: [szolgáltatói szerződések](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="2a03e-125">For more information, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>

## <a name="schedule-scaling"></a><span data-ttu-id="2a03e-126">Ütemezés skálázás</span><span class="sxs-lookup"><span data-stu-id="2a03e-126">Schedule scaling</span></span>
<span data-ttu-id="2a03e-127">Alapértelmezés szerint az összes szerepkör ne hajtsa végre egy adott ütemezés.</span><span class="sxs-lookup"><span data-stu-id="2a03e-127">By default, all roles do not follow a specific schedule.</span></span> <span data-ttu-id="2a03e-128">Ezért szereplő beállítások megváltozott vonatkoznak a minden alkalommal, és minden nap a év során.</span><span class="sxs-lookup"><span data-stu-id="2a03e-128">Therefore, any settings changed apply to all times and all days throughout the year.</span></span> <span data-ttu-id="2a03e-129">Ha azt szeretné, beállíthatja manuális vagy automatikus skálázás az alábbi mód:</span><span class="sxs-lookup"><span data-stu-id="2a03e-129">If you want, you can setup manual or automatic scaling for one of the following modes:</span></span>

* <span data-ttu-id="2a03e-130">Hétköznapokon</span><span class="sxs-lookup"><span data-stu-id="2a03e-130">Weekdays</span></span>
* <span data-ttu-id="2a03e-131">Hétvégéket</span><span class="sxs-lookup"><span data-stu-id="2a03e-131">Weekends</span></span>
* <span data-ttu-id="2a03e-132">Hét éjszakák</span><span class="sxs-lookup"><span data-stu-id="2a03e-132">Week nights</span></span>
* <span data-ttu-id="2a03e-133">Hét reggelente</span><span class="sxs-lookup"><span data-stu-id="2a03e-133">Week mornings</span></span>
* <span data-ttu-id="2a03e-134">Konkrét dátumokat</span><span class="sxs-lookup"><span data-stu-id="2a03e-134">Specific dates</span></span>
* <span data-ttu-id="2a03e-135">Adott dátumtartományok</span><span class="sxs-lookup"><span data-stu-id="2a03e-135">Specific date ranges</span></span>

<span data-ttu-id="2a03e-136">Az ütemezési beállítás konfigurálva van a [a klasszikus Azure portálon](https://manage.windowsazure.com/) meg a</span><span class="sxs-lookup"><span data-stu-id="2a03e-136">The schedule setting is configured in the [Azure classic portal](https://manage.windowsazure.com/) on the</span></span>  
<span data-ttu-id="2a03e-137">**A felhőalapú szolgáltatások** > **\[a felhőalapú szolgáltatás\]** > **méretezési** > **\[üzemi és átmeneti\]**  lap.</span><span class="sxs-lookup"><span data-stu-id="2a03e-137">**Cloud Services** > **\[Your cloud service\]** > **Scale** > **\[Production or Staging\]** page.</span></span>

<span data-ttu-id="2a03e-138">Kattintson a **ütemezés beállítása** gomb az egyes szerepkörökhöz módosítani kívánja.</span><span class="sxs-lookup"><span data-stu-id="2a03e-138">Click the **set up schedule times** button for each role you want to change.</span></span>

![A felhőalapú szolgáltatás ütemezés alapján automatikus skálázás][scale_schedules]

## <a name="manual-scale"></a><span data-ttu-id="2a03e-140">Manuális méretezési</span><span class="sxs-lookup"><span data-stu-id="2a03e-140">Manual scale</span></span>
<span data-ttu-id="2a03e-141">Az a **méretezési** lapon manuálisan megnövelhető, vagy egy felhőalapú szolgáltatásban futó példányok számának csökkentéséhez.</span><span class="sxs-lookup"><span data-stu-id="2a03e-141">On the **Scale** page, you can manually increase or decrease the number of running instances in a cloud service.</span></span> <span data-ttu-id="2a03e-142">Ez a beállítás minden létrehozott ütemezést, vagy minden alkalommal, ha nem hozott létre ütemezés.</span><span class="sxs-lookup"><span data-stu-id="2a03e-142">This setting is configured for each schedule you've created or to all time if you have not created a schedule.</span></span>

1. <span data-ttu-id="2a03e-143">A a [a klasszikus Azure portálon](https://manage.windowsazure.com/), kattintson a **Felhőszolgáltatások**, majd kattintson a nevére, a felhőalapú szolgáltatás, az irányítópult megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="2a03e-143">In the [Azure classic portal](https://manage.windowsazure.com/), click **Cloud Services**, and then click the name of the cloud service to open the dashboard.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="2a03e-144">Ha nem látja a felhőalapú szolgáltatás, előfordulhat, hogy módosítani szeretné a **éles** való **átmeneti** vagy fordítva.</span><span class="sxs-lookup"><span data-stu-id="2a03e-144">If you don't see your cloud service, you may need to change from **Production** to **Staging** or vice versa.</span></span>

2. <span data-ttu-id="2a03e-145">Kattintson a **méretezési**.</span><span class="sxs-lookup"><span data-stu-id="2a03e-145">Click **Scale**.</span></span>
3. <span data-ttu-id="2a03e-146">Válassza ki a kívánt méretezési beállításainak módosítása ütemezést.</span><span class="sxs-lookup"><span data-stu-id="2a03e-146">Select the schedule you want to change scaling options for.</span></span> <span data-ttu-id="2a03e-147">*Nem ütemezett időpontokban* az alapértelmezett beállítás, ha még nem definiált ütemezések.</span><span class="sxs-lookup"><span data-stu-id="2a03e-147">*No scheduled times* is the default if you have no schedules defined.</span></span>
4. <span data-ttu-id="2a03e-148">Keresés a **a metrika a skála** válassza ki azt **NONE**.</span><span class="sxs-lookup"><span data-stu-id="2a03e-148">Find the **Scale by metric** section and select **NONE**.</span></span> <span data-ttu-id="2a03e-149">Ez a beállítás az alapértelmezés az összes szerepkör.</span><span class="sxs-lookup"><span data-stu-id="2a03e-149">This setting is the default for all roles.</span></span>
5. <span data-ttu-id="2a03e-150">A felhőalapú szolgáltatás minden szerepkörhöz használandó példányok számának módosítása a csúszkán.</span><span class="sxs-lookup"><span data-stu-id="2a03e-150">Each role in the cloud service has a slider for changing the number of instances to use.</span></span>
   
    ![Manuálisan méretezhető, a felhő szerepkör-szolgáltatás][manual_scale]
   
    <span data-ttu-id="2a03e-152">Ha több példány van szüksége, előfordulhat, hogy módosítani szeretné a [a felhő virtuálisgép-méret](cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="2a03e-152">If you need more instances, you may need to change the [cloud service virtual machine size](cloud-services-sizes-specs.md).</span></span>
6. <span data-ttu-id="2a03e-153">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="2a03e-153">Click **Save**.</span></span>  
   <span data-ttu-id="2a03e-154">Szerepkörpéldányokat hozzáadásakor vagy eltávolításakor a kiválasztott beállítások alapján.</span><span class="sxs-lookup"><span data-stu-id="2a03e-154">Role instances are added or removed based on your selections.</span></span>

> [!TIP]
> <span data-ttu-id="2a03e-155">Amikor látja ![][tip_icon] az egér mozgatásával és kaphat arról, hogy milyen egy adott beállítás.</span><span class="sxs-lookup"><span data-stu-id="2a03e-155">Whenever you see ![][tip_icon] move your mouse to it and you can get help about what a specific setting does.</span></span>

## <a name="automatic-scale---cpu"></a><span data-ttu-id="2a03e-156">Automatikus méretezési - Processzor</span><span class="sxs-lookup"><span data-stu-id="2a03e-156">Automatic scale - CPU</span></span>
<span data-ttu-id="2a03e-157">Ebben a módban arányosan, ha a CPU-használat átlagos aránya a megadott küszöbérték alatti vagy feletti kerül.</span><span class="sxs-lookup"><span data-stu-id="2a03e-157">This mode scales if the average percentage of CPU usage goes above or below specified thresholds.</span></span> <span data-ttu-id="2a03e-158">Szerepkörpéldányokat létrehozott vagy törölt, ha ez történik.</span><span class="sxs-lookup"><span data-stu-id="2a03e-158">Role instances are created or deleted when this happens.</span></span>

1. <span data-ttu-id="2a03e-159">A a [a klasszikus Azure portálon](https://manage.windowsazure.com/), kattintson a **Felhőszolgáltatások**, majd kattintson a nevére, a felhőalapú szolgáltatás, az irányítópult megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="2a03e-159">In the [Azure classic portal](https://manage.windowsazure.com/), click **Cloud Services**, and then click the name of the cloud service to open the dashboard.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="2a03e-160">Ha nem látja a felhőalapú szolgáltatás, előfordulhat, hogy módosítani szeretné a **éles** való **átmeneti** vagy fordítva.</span><span class="sxs-lookup"><span data-stu-id="2a03e-160">If you don't see your cloud service, you may need to change from **Production** to **Staging** or vice versa.</span></span>

2. <span data-ttu-id="2a03e-161">Kattintson a **méretezési**.</span><span class="sxs-lookup"><span data-stu-id="2a03e-161">Click **Scale**.</span></span>
3. <span data-ttu-id="2a03e-162">Válassza ki a kívánt méretezési beállításainak módosítása ütemezést.</span><span class="sxs-lookup"><span data-stu-id="2a03e-162">Select the schedule you want to change scaling options for.</span></span> <span data-ttu-id="2a03e-163">*Nem ütemezett időpontokban* az alapértelmezett beállítás, ha még nem definiált ütemezések.</span><span class="sxs-lookup"><span data-stu-id="2a03e-163">*No scheduled times* is the default if you have no schedules defined.</span></span>
4. <span data-ttu-id="2a03e-164">Keresés a **a metrika a skála** válassza ki azt **CPU**.</span><span class="sxs-lookup"><span data-stu-id="2a03e-164">Find the **Scale by metric** section and select **CPU**.</span></span>
5. <span data-ttu-id="2a03e-165">Most egy minimális és maximális számos szerepkörök példányok, a cél CPU-használat (való terjedő skálán be), és felfelé és lefelé méretezési szerint hány példányban is konfigurálhat.</span><span class="sxs-lookup"><span data-stu-id="2a03e-165">Now you can configure a minimum and maximum range of roles instances, the target CPU usage (to trigger a scale up), and how many instances to scale up and down by.</span></span>

![Bővítse a cpu-terhelés a felhőalapú szolgáltatás szerepkör][cpu_scale]

> [!TIP]
> <span data-ttu-id="2a03e-167">Amikor látja ![][tip_icon] az egér mozgatásával és kaphat arról, hogy milyen egy adott beállítás.</span><span class="sxs-lookup"><span data-stu-id="2a03e-167">Whenever you see ![][tip_icon] move your mouse to it and you can get help about what a specific setting does.</span></span>

## <a name="automatic-scale---queue"></a><span data-ttu-id="2a03e-168">Automatikus méretezési - várólista</span><span class="sxs-lookup"><span data-stu-id="2a03e-168">Automatic scale - Queue</span></span>
<span data-ttu-id="2a03e-169">Ebben a módban automatikusan méretezi, ha a várólistán lévő üzenetek száma a megadott küszöbérték alatti vagy feletti kerül.</span><span class="sxs-lookup"><span data-stu-id="2a03e-169">This mode automatically scales if the number of messages in a queue goes above or below a specified threshold.</span></span> <span data-ttu-id="2a03e-170">Szerepkörpéldányokat létrehozott vagy törölt, ha ez történik.</span><span class="sxs-lookup"><span data-stu-id="2a03e-170">Role instances are created or deleted when this happens.</span></span>

1. <span data-ttu-id="2a03e-171">A a [a klasszikus Azure portálon](https://manage.windowsazure.com/), kattintson a **Felhőszolgáltatások**, majd kattintson a nevére, a felhőalapú szolgáltatás, az irányítópult megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="2a03e-171">In the [Azure classic portal](https://manage.windowsazure.com/), click **Cloud Services**, and then click the name of the cloud service to open the dashboard.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="2a03e-172">Ha nem látja a felhőalapú szolgáltatás, előfordulhat, hogy módosítani szeretné a **éles** való **átmeneti** vagy fordítva.</span><span class="sxs-lookup"><span data-stu-id="2a03e-172">If you don't see your cloud service, you may need to change from **Production** to **Staging** or vice versa.</span></span>

2. <span data-ttu-id="2a03e-173">Kattintson a **méretezési**.</span><span class="sxs-lookup"><span data-stu-id="2a03e-173">Click **Scale**.</span></span>
3. <span data-ttu-id="2a03e-174">Keresés a **a metrika a skála** válassza ki azt **VÁRÓLISTA**.</span><span class="sxs-lookup"><span data-stu-id="2a03e-174">Find the **Scale by metric** section and select **QUEUE**.</span></span>
4. <span data-ttu-id="2a03e-175">Most egy minimális és maximális tartományt szerepkörök példányok, a várólista és minden példányt, és hány példányban felfelé és lefelé méretezési által feldolgozandó üzenetek száma is konfigurálhat.</span><span class="sxs-lookup"><span data-stu-id="2a03e-175">Now you can configure a minimum and maximum range of roles instances, the queue and number of queue messages to process for each instance, and how many instances to scale up and down by.</span></span>

![Bővítse a felhőalapú szolgáltatás szerepkör üzenet-várólista][queue_scale]

> [!TIP]
> <span data-ttu-id="2a03e-177">Amikor látja ![][tip_icon] az egér mozgatásával és kaphat arról, hogy milyen egy adott beállítás.</span><span class="sxs-lookup"><span data-stu-id="2a03e-177">Whenever you see ![][tip_icon] move your mouse to it and you can get help about what a specific setting does.</span></span>

## <a name="scale-linked-resources"></a><span data-ttu-id="2a03e-178">Méretezhető kapcsolt erőforrásokban</span><span class="sxs-lookup"><span data-stu-id="2a03e-178">Scale linked resources</span></span>
<span data-ttu-id="2a03e-179">Gyakran, ha egy szerepkör méretezni, célszerű az alkalmazás által is használt adatbázis méretezése.</span><span class="sxs-lookup"><span data-stu-id="2a03e-179">Often when you scale a role, it's beneficial to scale the database that the application is using also.</span></span> <span data-ttu-id="2a03e-180">Ha az adatbázis csatolása a felhőalapú szolgáltatás, a megfelelő hivatkozásra kattintva hozzáférhet a skálázási beállításokat az adott erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="2a03e-180">If you link the database to the cloud service, you can access the scaling settings for that resource by clicking the appropriate link.</span></span>

1. <span data-ttu-id="2a03e-181">A a [a klasszikus Azure portálon](https://manage.windowsazure.com/), kattintson a **Felhőszolgáltatások**, majd kattintson a nevére, a felhőalapú szolgáltatás, az irányítópult megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="2a03e-181">In the [Azure classic portal](https://manage.windowsazure.com/), click **Cloud Services**, and then click the name of the cloud service to open the dashboard.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="2a03e-182">Ha nem látja a felhőalapú szolgáltatás, előfordulhat, hogy módosítani szeretné a **éles** való **átmeneti** vagy fordítva.</span><span class="sxs-lookup"><span data-stu-id="2a03e-182">If you don't see your cloud service, you may need to change from **Production** to **Staging** or vice versa.</span></span>

2. <span data-ttu-id="2a03e-183">Kattintson a **méretezési**.</span><span class="sxs-lookup"><span data-stu-id="2a03e-183">Click **Scale**.</span></span>
3. <span data-ttu-id="2a03e-184">Keresés a **kapcsolódó erőforrások** szakaszt, és kattintson **az adatbázis kezelésére**.</span><span class="sxs-lookup"><span data-stu-id="2a03e-184">Find the **linked resources** section and click **Manage scale for this database**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="2a03e-185">Ha nem látja a **kapcsolódó erőforrások** szakaszban, valószínűleg nem rendelkezik társított erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="2a03e-185">If you don't see a **linked resources** section, you probably do not have any linked resources.</span></span>

![][linked_resource]

[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png
