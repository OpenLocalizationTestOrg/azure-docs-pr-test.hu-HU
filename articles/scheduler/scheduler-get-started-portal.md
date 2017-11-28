---
title: "Ismerkedés az Azure Portal Azure Scheduler szolgáltatásával | Microsoft Docs"
description: "Ismerkedés az Azure portál Azure Scheduler szolgáltatásával"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: e69542ec-d10f-4f17-9b7a-2ee441ee7d68
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/10/2016
ms.author: deli
ms.openlocfilehash: 3861ee121ed1c4d086ea81640e84d924d7d17ea1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-scheduler-in-azure-portal"></a><span data-ttu-id="d7cb0-103">Ismerkedés az Azure portál Azure Scheduler szolgáltatásával</span><span class="sxs-lookup"><span data-stu-id="d7cb0-103">Get started with Azure Scheduler in Azure portal</span></span>
<span data-ttu-id="d7cb0-104">Az Azure Scheduler szolgáltatásban egyszerűen hozhat létre ütemezett feladatokat.</span><span class="sxs-lookup"><span data-stu-id="d7cb0-104">It's easy to create scheduled jobs in Azure Scheduler.</span></span> <span data-ttu-id="d7cb0-105">Ezen oktatóanyag segítségével elsajátíthatja egy feladat létrehozásának műveletét.</span><span class="sxs-lookup"><span data-stu-id="d7cb0-105">In this tutorial, you'll learn how to create a job.</span></span> <span data-ttu-id="d7cb0-106">Megismerkedhet a Scheduler megfigyelési és felügyeleti képességeivel is.</span><span class="sxs-lookup"><span data-stu-id="d7cb0-106">You'll also learn Scheduler's monitoring and management capabilities.</span></span>

## <a name="create-a-job"></a><span data-ttu-id="d7cb0-107">Feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="d7cb0-107">Create a job</span></span>
1. <span data-ttu-id="d7cb0-108">Jelentkezzen be az [Azure portálra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d7cb0-108">Sign in to [Azure portal](https://portal.azure.com/).</span></span>  
2. <span data-ttu-id="d7cb0-109">Kattintson az **+Új** lehetőségre, írja be a *Scheduler* kifejezést a keresőmezőbe, majd válassza ki a **Scheduler** elemet az eredmények között, végül pedig kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d7cb0-109">Click **+New** > type *Scheduler* in the search box >  select **Scheduler** in results > click **Create**.</span></span>
   
    ![][marketplace-create]
3. <span data-ttu-id="d7cb0-110">Hozzunk létre egy olyan feladatot, amely egy GET kéréssel egyszerűen rákeres a http://www.microsoft.com/ webhelyre.</span><span class="sxs-lookup"><span data-stu-id="d7cb0-110">Let’s create a job that simply hits http://www.microsoft.com/ with a GET request.</span></span> <span data-ttu-id="d7cb0-111">A **Scheduler-feladat** képernyőn adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="d7cb0-111">In the **Scheduler Job** screen, enter the following information:</span></span>
   
   1. <span data-ttu-id="d7cb0-112">**Név:** `getmicrosoft`</span><span class="sxs-lookup"><span data-stu-id="d7cb0-112">**Name:** `getmicrosoft`</span></span>  
   2. <span data-ttu-id="d7cb0-113">**Előfizetés:** Az Ön Azure-előfizetése</span><span class="sxs-lookup"><span data-stu-id="d7cb0-113">**Subscription:** Your Azure subscription</span></span>   
   3. <span data-ttu-id="d7cb0-114">**Feladatgyűjtemény:** Válasszon ki egy létező feladatgyűjteményt, vagy kattintson az **Új létrehozása** lehetőségre, és adjon meg egy nevet.</span><span class="sxs-lookup"><span data-stu-id="d7cb0-114">**Job Collection:** Select an existing job collection, or click **Create New** > enter a name.</span></span>
4. <span data-ttu-id="d7cb0-115">Ezt követően határozza meg a következő értékeket a **Művelet beállításai** területen:</span><span class="sxs-lookup"><span data-stu-id="d7cb0-115">Next, in **Action Settings**, define the following values:</span></span>
   
   1. <span data-ttu-id="d7cb0-116">**Művelettípus:** ` HTTP`</span><span class="sxs-lookup"><span data-stu-id="d7cb0-116">**Action Type:** ` HTTP`</span></span>  
   2. <span data-ttu-id="d7cb0-117">**Módszer:** `GET`</span><span class="sxs-lookup"><span data-stu-id="d7cb0-117">**Method:** `GET`</span></span>  
   3. <span data-ttu-id="d7cb0-118">**URL-cím:** ` http://www.microsoft.com`</span><span class="sxs-lookup"><span data-stu-id="d7cb0-118">**URL:** ` http://www.microsoft.com`</span></span>  
      
      ![][action-settings]
5. <span data-ttu-id="d7cb0-119">Végül határozzon meg egy ütemezést.</span><span class="sxs-lookup"><span data-stu-id="d7cb0-119">Finally, let's define a schedule.</span></span> <span data-ttu-id="d7cb0-120">A feladat egyszeriként is megadható, most azonban vegyünk fel egy ismétlődési ütemezést:</span><span class="sxs-lookup"><span data-stu-id="d7cb0-120">The job could be defined as a one-time job, but let’s pick a recurrence schedule:</span></span>
   
   1. <span data-ttu-id="d7cb0-121">**Ismétlődés**: `Recurring`</span><span class="sxs-lookup"><span data-stu-id="d7cb0-121">**Recurrence**: `Recurring`</span></span>
   2. <span data-ttu-id="d7cb0-122">**Indítás**: A mai dátum</span><span class="sxs-lookup"><span data-stu-id="d7cb0-122">**Start**: Today's date</span></span>
   3. <span data-ttu-id="d7cb0-123">**Ismétlődés**: `12 Hours`</span><span class="sxs-lookup"><span data-stu-id="d7cb0-123">**Recur every**: `12 Hours`</span></span>
   4. <span data-ttu-id="d7cb0-124">**Befejeződés**: A mai naptól számított két nap múlva</span><span class="sxs-lookup"><span data-stu-id="d7cb0-124">**End by**: Two days from today's date</span></span>  
      
      ![][recurrence-schedule]
6. <span data-ttu-id="d7cb0-125">Kattintson a  **Create** (Létrehozás) gombra</span><span class="sxs-lookup"><span data-stu-id="d7cb0-125">Click **Create**</span></span>

## <a name="manage-and-monitor-jobs"></a><span data-ttu-id="d7cb0-126">Feladatok kezelése és figyelése</span><span class="sxs-lookup"><span data-stu-id="d7cb0-126">Manage and monitor jobs</span></span>
<span data-ttu-id="d7cb0-127">A létrehozott feladatok megjelennek az Azure fő irányítópultján.</span><span class="sxs-lookup"><span data-stu-id="d7cb0-127">Once a job is created, it appears in the main Azure dashboard.</span></span> <span data-ttu-id="d7cb0-128">A feladatra kattintva egy új ablak nyílik meg a következő lapokkal:</span><span class="sxs-lookup"><span data-stu-id="d7cb0-128">Click the job and a new window opens with the following tabs:</span></span>

1. <span data-ttu-id="d7cb0-129">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="d7cb0-129">Properties</span></span>  
2. <span data-ttu-id="d7cb0-130">Művelet beállításai</span><span class="sxs-lookup"><span data-stu-id="d7cb0-130">Action Settings</span></span>  
3. <span data-ttu-id="d7cb0-131">Ütemezés</span><span class="sxs-lookup"><span data-stu-id="d7cb0-131">Schedule</span></span>  
4. <span data-ttu-id="d7cb0-132">Előzmények</span><span class="sxs-lookup"><span data-stu-id="d7cb0-132">History</span></span>
5. <span data-ttu-id="d7cb0-133">Felhasználók</span><span class="sxs-lookup"><span data-stu-id="d7cb0-133">Users</span></span>
   
   ![][job-overview]

### <a name="properties"></a><span data-ttu-id="d7cb0-134">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="d7cb0-134">Properties</span></span>
<span data-ttu-id="d7cb0-135">A Scheduler-feladathoz tartozó metaadatok kezelését ezen írásvédett tulajdonságok írják le.</span><span class="sxs-lookup"><span data-stu-id="d7cb0-135">These read-only properties describe the management metadata for the Scheduler job.</span></span>

   ![][job-properties]

### <a name="action-settings"></a><span data-ttu-id="d7cb0-136">Művelet beállításai</span><span class="sxs-lookup"><span data-stu-id="d7cb0-136">Action settings</span></span>
<span data-ttu-id="d7cb0-137">Az adott feladat konfigurálásához kattintson rá a **Feladatok** képernyőn.</span><span class="sxs-lookup"><span data-stu-id="d7cb0-137">Clicking on a job in the **Jobs** screen allows you to configure that job.</span></span> <span data-ttu-id="d7cb0-138">Ha a gyorslétrehozási varázslóban ezt nem tette meg, itt lehetősége van speciális beállítások konfigurálására.</span><span class="sxs-lookup"><span data-stu-id="d7cb0-138">This lets you configure advanced settings, if you didn't configure them in the quick-create wizard.</span></span>

<span data-ttu-id="d7cb0-139">Az újrapróbálkozási házirend és a hibakezelési művelet módosítására az összes művelettípus esetében lehetőség van.</span><span class="sxs-lookup"><span data-stu-id="d7cb0-139">For all action types, you may change the retry policy and the error action.</span></span>

<span data-ttu-id="d7cb0-140">A HTTP- és a HTTPS-feladatok művelettípusai esetében a módszer bármely engedélyezett HTTP-parancsra módosítható.</span><span class="sxs-lookup"><span data-stu-id="d7cb0-140">For HTTP and HTTPS job action types, you may change the method to any allowed HTTP verb.</span></span> <span data-ttu-id="d7cb0-141">Lehetőség van továbbá a fejlécek és az egyszerű hitelesítési adatok hozzáadására, törlésére vagy módosítására is.</span><span class="sxs-lookup"><span data-stu-id="d7cb0-141">You may also add, delete, or change the headers and basic authentication information.</span></span>

<span data-ttu-id="d7cb0-142">Tárolásisor-művelettípusok esetében módosítható a tárfiók, az üzenetsor neve, az SAS-token és a törzs.</span><span class="sxs-lookup"><span data-stu-id="d7cb0-142">For storage queue action types, you may change the storage account, queue name, SAS token, and body.</span></span>

<span data-ttu-id="d7cb0-143">Service Bus művelettípusok esetében módosítható a névtér, a témakör/üzenetsor elérési útvonala, a hitelesítési beállítások, az átvitel típusa, az üzenettulajdonságok és az üzenettörzs.</span><span class="sxs-lookup"><span data-stu-id="d7cb0-143">For service bus action types, you may change the namespace, topic/queue path, authentication settings, transport type, message properties, and message body.</span></span>

   ![][job-action-settings]

### <a name="schedule"></a><span data-ttu-id="d7cb0-144">Ütemezés</span><span class="sxs-lookup"><span data-stu-id="d7cb0-144">Schedule</span></span>
<span data-ttu-id="d7cb0-145">Ha módosítani kívánja a gyorslétrehozási varázslóban létrehozott ütemezést, itt újrakonfigurálhatja azt.</span><span class="sxs-lookup"><span data-stu-id="d7cb0-145">This lets you reconfigure the schedule, if you'd like to change the schedule you created in the quick-create wizard.</span></span>

<span data-ttu-id="d7cb0-146">Ez lehetőséget ad [komplex és speciális, ismétlődő ütemezések létrehozására a feladathoz](scheduler-advanced-complexity.md).</span><span class="sxs-lookup"><span data-stu-id="d7cb0-146">This is an opportunity to build [complex schedules and advanced recurrence in your job](scheduler-advanced-complexity.md)</span></span>

<span data-ttu-id="d7cb0-147">Módosítható a kezdési dátum és időpont, az ismétlődési ütemezés, valamint a befejezési dátum és időpont (amennyiben a feladat ismétlődő jellegű).</span><span class="sxs-lookup"><span data-stu-id="d7cb0-147">You may change the start date and time, recurrence schedule, and the end date and time (if the job is recurring.)</span></span>

   ![][job-schedule]

### <a name="history"></a><span data-ttu-id="d7cb0-148">Előzmények</span><span class="sxs-lookup"><span data-stu-id="d7cb0-148">History</span></span>
<span data-ttu-id="d7cb0-149">Az adott feladathoz a rendszerben elérhető valamennyi feladat-végrehajtási mód kiválasztott adatai megjelennek az **Előzmények** lapon.</span><span class="sxs-lookup"><span data-stu-id="d7cb0-149">The **History** tab displays selected metrics for every job execution in the system for the selected job.</span></span> <span data-ttu-id="d7cb0-150">Ezen adatok segítségével valós időben értékelhető a Scheduler szolgáltatás állapota:</span><span class="sxs-lookup"><span data-stu-id="d7cb0-150">These metrics provide real-time values regarding the health of your Scheduler:</span></span>

1. <span data-ttu-id="d7cb0-151">status</span><span class="sxs-lookup"><span data-stu-id="d7cb0-151">Status</span></span>  
2. <span data-ttu-id="d7cb0-152">Részletek</span><span class="sxs-lookup"><span data-stu-id="d7cb0-152">Details</span></span>  
3. <span data-ttu-id="d7cb0-153">Újrapróbálkozási kísérletek</span><span class="sxs-lookup"><span data-stu-id="d7cb0-153">Retry attempts</span></span>
4. <span data-ttu-id="d7cb0-154">Előfordulás: 1., 2., 3. stb.</span><span class="sxs-lookup"><span data-stu-id="d7cb0-154">Occurrence: 1st, 2nd, 3rd, etc.</span></span>
5. <span data-ttu-id="d7cb0-155">A futtatás kezdési időpontja</span><span class="sxs-lookup"><span data-stu-id="d7cb0-155">Start time of execution</span></span>  
6. <span data-ttu-id="d7cb0-156">A futtatás befejezési időpontja</span><span class="sxs-lookup"><span data-stu-id="d7cb0-156">End time of execution</span></span>
   
   ![][job-history]

<span data-ttu-id="d7cb0-157">Az adott futtatásra kattintva megtekintheti annak **Előzményadatait**, beleértve a teljes választ minden végrehajtás esetében.</span><span class="sxs-lookup"><span data-stu-id="d7cb0-157">You can click on a run to view its **History Details**, including the whole response for every execution.</span></span> <span data-ttu-id="d7cb0-158">Ezen párbeszédpanelről lehetőség van a válasz vágólapra másolására is.</span><span class="sxs-lookup"><span data-stu-id="d7cb0-158">This dialog box also allows you to copy the response to the clipboard.</span></span>

   ![][job-history-details]

### <a name="users"></a><span data-ttu-id="d7cb0-159">Felhasználók</span><span class="sxs-lookup"><span data-stu-id="d7cb0-159">Users</span></span>
<span data-ttu-id="d7cb0-160">Az Azure Szerepköralapú hozzáférés-vezérlés (RBAC) részletesen beállítható hozzáférés-vezérlést biztosít az Azure Scheduler szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="d7cb0-160">Azure Role-Based Access Control (RBAC) enables fine-grained access management for Azure Scheduler.</span></span> <span data-ttu-id="d7cb0-161">A Felhasználók lap használatának elsajátításához lásd: [Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md)</span><span class="sxs-lookup"><span data-stu-id="d7cb0-161">To learn how to use the Users tab, refer to [Azure Role-Based Access Control](../active-directory/role-based-access-control-configure.md)</span></span>

## <a name="see-also"></a><span data-ttu-id="d7cb0-162">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="d7cb0-162">See also</span></span>
 [<span data-ttu-id="d7cb0-163">A Scheduler ismertetése</span><span class="sxs-lookup"><span data-stu-id="d7cb0-163">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="d7cb0-164">A Scheduler alapfogalmai, terminológiája és entitáshierarchiája</span><span class="sxs-lookup"><span data-stu-id="d7cb0-164">Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="d7cb0-165">Csomagok és számlázás az Azure Schedulerben</span><span class="sxs-lookup"><span data-stu-id="d7cb0-165">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="d7cb0-166">Komplex és speciális, ismétlődő ütemezések létrehozása az Azure Scheduler használatával</span><span class="sxs-lookup"><span data-stu-id="d7cb0-166">How to build complex schedules and advanced recurrence with Azure Scheduler</span></span>](scheduler-advanced-complexity.md)

 [<span data-ttu-id="d7cb0-167">A Scheduler REST API-jának leírása</span><span class="sxs-lookup"><span data-stu-id="d7cb0-167">Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="d7cb0-168">A Scheduler PowerShell-parancsmagjainak leírása</span><span class="sxs-lookup"><span data-stu-id="d7cb0-168">Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="d7cb0-169">Scheduler – magas fokú rendelkezésre állás és megbízhatóság</span><span class="sxs-lookup"><span data-stu-id="d7cb0-169">Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="d7cb0-170">Scheduler – korlátozások, alapértékek és hibakódok</span><span class="sxs-lookup"><span data-stu-id="d7cb0-170">Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

 [<span data-ttu-id="d7cb0-171">Kimenő hitelesítés a Schedulerben</span><span class="sxs-lookup"><span data-stu-id="d7cb0-171">Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

[marketplace-create]: ./media/scheduler-get-started-portal/scheduler-v2-portal-marketplace-create.png
[action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-action-settings.png
[recurrence-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-recurrence-schedule.png
[job-properties]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-properties.png
[job-overview]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-overview-1.png
[job-action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-action-settings.png
[job-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-schedule.png
[job-history]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history.png
[job-history-details]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history-details.png


[1]: ./media/scheduler-get-started-portal/scheduler-get-started-portal001.png
[2]: ./media/scheduler-get-started-portal/scheduler-get-started-portal002.png
[3]: ./media/scheduler-get-started-portal/scheduler-get-started-portal003.png
[4]: ./media/scheduler-get-started-portal/scheduler-get-started-portal004.png
[5]: ./media/scheduler-get-started-portal/scheduler-get-started-portal005.png
[6]: ./media/scheduler-get-started-portal/scheduler-get-started-portal006.png
[7]: ./media/scheduler-get-started-portal/scheduler-get-started-portal007.png
[8]: ./media/scheduler-get-started-portal/scheduler-get-started-portal008.png
[9]: ./media/scheduler-get-started-portal/scheduler-get-started-portal009.png
[10]: ./media/scheduler-get-started-portal/scheduler-get-started-portal010.png
[11]: ./media/scheduler-get-started-portal/scheduler-get-started-portal011.png
[12]: ./media/scheduler-get-started-portal/scheduler-get-started-portal012.png
[13]: ./media/scheduler-get-started-portal/scheduler-get-started-portal013.png
[14]: ./media/scheduler-get-started-portal/scheduler-get-started-portal014.png
[15]: ./media/scheduler-get-started-portal/scheduler-get-started-portal015.png
