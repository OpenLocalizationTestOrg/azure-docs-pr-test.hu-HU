---
title: "az Azure portál Azure Scheduler használatába aaaGet |} Microsoft Docs"
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
ms.openlocfilehash: 58255c0ad19da65932f8b1d36cb8fef1ff6e651b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-scheduler-in-azure-portal"></a><span data-ttu-id="a0c42-103">Ismerkedés az Azure portál Azure Scheduler szolgáltatásával</span><span class="sxs-lookup"><span data-stu-id="a0c42-103">Get started with Azure Scheduler in Azure portal</span></span>
<span data-ttu-id="a0c42-104">Azure Scheduler könnyen toocreate ütemezett feladatok is.</span><span class="sxs-lookup"><span data-stu-id="a0c42-104">It's easy toocreate scheduled jobs in Azure Scheduler.</span></span> <span data-ttu-id="a0c42-105">Ebben az oktatóanyagban megtudhatja, hogyan toocreate egy feladatot.</span><span class="sxs-lookup"><span data-stu-id="a0c42-105">In this tutorial, you'll learn how toocreate a job.</span></span> <span data-ttu-id="a0c42-106">Megismerkedhet a Scheduler megfigyelési és felügyeleti képességeivel is.</span><span class="sxs-lookup"><span data-stu-id="a0c42-106">You'll also learn Scheduler's monitoring and management capabilities.</span></span>

## <a name="create-a-job"></a><span data-ttu-id="a0c42-107">Feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="a0c42-107">Create a job</span></span>
1. <span data-ttu-id="a0c42-108">Jelentkezzen be a túl[Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="a0c42-108">Sign in too[Azure portal](https://portal.azure.com/).</span></span>  
2. <span data-ttu-id="a0c42-109">Kattintson a **+ új** > típus *Feladatütemező* hello keresőmezőbe > válassza **Feladatütemező** eredmények > kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="a0c42-109">Click **+New** > type *Scheduler* in hello search box >  select **Scheduler** in results > click **Create**.</span></span>
   
    ![][marketplace-create]
3. <span data-ttu-id="a0c42-110">Hozzunk létre egy olyan feladatot, amely egy GET kéréssel egyszerűen rákeres a http://www.microsoft.com/ webhelyre.</span><span class="sxs-lookup"><span data-stu-id="a0c42-110">Let’s create a job that simply hits http://www.microsoft.com/ with a GET request.</span></span> <span data-ttu-id="a0c42-111">A hello **ütemezési feladat** képernyőn, írja be a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="a0c42-111">In hello **Scheduler Job** screen, enter hello following information:</span></span>
   
   1. <span data-ttu-id="a0c42-112">**Név:**`getmicrosoft`</span><span class="sxs-lookup"><span data-stu-id="a0c42-112">**Name:** `getmicrosoft`</span></span>  
   2. <span data-ttu-id="a0c42-113">**Előfizetés:** Az Ön Azure-előfizetése</span><span class="sxs-lookup"><span data-stu-id="a0c42-113">**Subscription:** Your Azure subscription</span></span>   
   3. <span data-ttu-id="a0c42-114">**Feladatgyűjtemény:** Válasszon ki egy létező feladatgyűjteményt, vagy kattintson az **Új létrehozása** lehetőségre, és adjon meg egy nevet.</span><span class="sxs-lookup"><span data-stu-id="a0c42-114">**Job Collection:** Select an existing job collection, or click **Create New** > enter a name.</span></span>
4. <span data-ttu-id="a0c42-115">A következő **műveleti beállítások**, adja meg a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="a0c42-115">Next, in **Action Settings**, define hello following values:</span></span>
   
   1. <span data-ttu-id="a0c42-116">**Művelettípus:**` HTTP`</span><span class="sxs-lookup"><span data-stu-id="a0c42-116">**Action Type:** ` HTTP`</span></span>  
   2. <span data-ttu-id="a0c42-117">**Módszer:**`GET`</span><span class="sxs-lookup"><span data-stu-id="a0c42-117">**Method:** `GET`</span></span>  
   3. <span data-ttu-id="a0c42-118">**URL-cím:**` http://www.microsoft.com`</span><span class="sxs-lookup"><span data-stu-id="a0c42-118">**URL:** ` http://www.microsoft.com`</span></span>  
      
      ![][action-settings]
5. <span data-ttu-id="a0c42-119">Végül határozzon meg egy ütemezést.</span><span class="sxs-lookup"><span data-stu-id="a0c42-119">Finally, let's define a schedule.</span></span> <span data-ttu-id="a0c42-120">hello feladat egyszeri feladat sikerült kell definiálni, de most az ismétlődési ütemezés szerint válasszon:</span><span class="sxs-lookup"><span data-stu-id="a0c42-120">hello job could be defined as a one-time job, but let’s pick a recurrence schedule:</span></span>
   
   1. <span data-ttu-id="a0c42-121">**Ismétlődés**: `Recurring`</span><span class="sxs-lookup"><span data-stu-id="a0c42-121">**Recurrence**: `Recurring`</span></span>
   2. <span data-ttu-id="a0c42-122">**Indítás**: A mai dátum</span><span class="sxs-lookup"><span data-stu-id="a0c42-122">**Start**: Today's date</span></span>
   3. <span data-ttu-id="a0c42-123">**Ismétlődés**: `12 Hours`</span><span class="sxs-lookup"><span data-stu-id="a0c42-123">**Recur every**: `12 Hours`</span></span>
   4. <span data-ttu-id="a0c42-124">**Befejeződés**: A mai naptól számított két nap múlva</span><span class="sxs-lookup"><span data-stu-id="a0c42-124">**End by**: Two days from today's date</span></span>  
      
      ![][recurrence-schedule]
6. <span data-ttu-id="a0c42-125">Kattintson a  **Create** (Létrehozás) gombra</span><span class="sxs-lookup"><span data-stu-id="a0c42-125">Click **Create**</span></span>

## <a name="manage-and-monitor-jobs"></a><span data-ttu-id="a0c42-126">Feladatok kezelése és figyelése</span><span class="sxs-lookup"><span data-stu-id="a0c42-126">Manage and monitor jobs</span></span>
<span data-ttu-id="a0c42-127">Ha egy feladat jön létre, hello fő Azure irányítópult jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a0c42-127">Once a job is created, it appears in hello main Azure dashboard.</span></span> <span data-ttu-id="a0c42-128">Kattintson a hello feladat és egy új ablakban nyílik meg a következő lapokon hello:</span><span class="sxs-lookup"><span data-stu-id="a0c42-128">Click hello job and a new window opens with hello following tabs:</span></span>

1. <span data-ttu-id="a0c42-129">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="a0c42-129">Properties</span></span>  
2. <span data-ttu-id="a0c42-130">Művelet beállításai</span><span class="sxs-lookup"><span data-stu-id="a0c42-130">Action Settings</span></span>  
3. <span data-ttu-id="a0c42-131">Ütemezés</span><span class="sxs-lookup"><span data-stu-id="a0c42-131">Schedule</span></span>  
4. <span data-ttu-id="a0c42-132">Előzmények</span><span class="sxs-lookup"><span data-stu-id="a0c42-132">History</span></span>
5. <span data-ttu-id="a0c42-133">Felhasználók</span><span class="sxs-lookup"><span data-stu-id="a0c42-133">Users</span></span>
   
   ![][job-overview]

### <a name="properties"></a><span data-ttu-id="a0c42-134">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="a0c42-134">Properties</span></span>
<span data-ttu-id="a0c42-135">A csak olvasható tulajdonságok hello felügyeleti metaadatok hello Feladatütemező feladat ismertetik.</span><span class="sxs-lookup"><span data-stu-id="a0c42-135">These read-only properties describe hello management metadata for hello Scheduler job.</span></span>

   ![][job-properties]

### <a name="action-settings"></a><span data-ttu-id="a0c42-136">Művelet beállításai</span><span class="sxs-lookup"><span data-stu-id="a0c42-136">Action settings</span></span>
<span data-ttu-id="a0c42-137">Egy feladat hello kattintva **feladatok** képernyő segítségével beállíthatja, hogy a feladat tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="a0c42-137">Clicking on a job in hello **Jobs** screen allows you tooconfigure that job.</span></span> <span data-ttu-id="a0c42-138">Ez lehetővé teszi a Speciális beállítások konfigurálása, ha nem adja meg azokat a hello gyors létrehozása varázsló.</span><span class="sxs-lookup"><span data-stu-id="a0c42-138">This lets you configure advanced settings, if you didn't configure them in hello quick-create wizard.</span></span>

<span data-ttu-id="a0c42-139">Az összes művelet esetében hello újrapróbálkozási házirendje és hello hiba művelet módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="a0c42-139">For all action types, you may change hello retry policy and hello error action.</span></span>

<span data-ttu-id="a0c42-140">A HTTP és HTTPS Feladatművelet típusú módosíthatja hello metódus tooany engedélyezett HTTP-műveletet.</span><span class="sxs-lookup"><span data-stu-id="a0c42-140">For HTTP and HTTPS job action types, you may change hello method tooany allowed HTTP verb.</span></span> <span data-ttu-id="a0c42-141">Előfordulhat, hogy is hozzáadhat, törlése, vagy módosítsa a hello fejlécek és alapszintű hitelesítési adatokat.</span><span class="sxs-lookup"><span data-stu-id="a0c42-141">You may also add, delete, or change hello headers and basic authentication information.</span></span>

<span data-ttu-id="a0c42-142">A tároló várólista művelet típusú hello tárfiók, üzenetsornév, SAS-jogkivonat és body módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="a0c42-142">For storage queue action types, you may change hello storage account, queue name, SAS token, and body.</span></span>

<span data-ttu-id="a0c42-143">A service bus művelet típusú hello névtér, témakör/várólista elérési útja, hitelesítési beállítások, átviteli típust, üzenet- és az üzenet törzse módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="a0c42-143">For service bus action types, you may change hello namespace, topic/queue path, authentication settings, transport type, message properties, and message body.</span></span>

   ![][job-action-settings]

### <a name="schedule"></a><span data-ttu-id="a0c42-144">Ütemezés</span><span class="sxs-lookup"><span data-stu-id="a0c42-144">Schedule</span></span>
<span data-ttu-id="a0c42-145">Ez lehetővé teszi a hello ütemezést konfigurálja újra, ha azt szeretné, hogy toochange hello ütemezés hello létrehozott gyors létrehozása varázsló.</span><span class="sxs-lookup"><span data-stu-id="a0c42-145">This lets you reconfigure hello schedule, if you'd like toochange hello schedule you created in hello quick-create wizard.</span></span>

<span data-ttu-id="a0c42-146">Ez az egy lehetőség toobuild [ütemezése összetett és speciális ismétlődési a feladatban](scheduler-advanced-complexity.md)</span><span class="sxs-lookup"><span data-stu-id="a0c42-146">This is an opportunity toobuild [complex schedules and advanced recurrence in your job](scheduler-advanced-complexity.md)</span></span>

<span data-ttu-id="a0c42-147">Módosíthatja a hello kezdő dátum és idő, illetve ismétlődő ütemezésekhez és hello lejárati dátuma és ideje (ha hello feladat ismétlődő.)</span><span class="sxs-lookup"><span data-stu-id="a0c42-147">You may change hello start date and time, recurrence schedule, and hello end date and time (if hello job is recurring.)</span></span>

   ![][job-schedule]

### <a name="history"></a><span data-ttu-id="a0c42-148">Előzmények</span><span class="sxs-lookup"><span data-stu-id="a0c42-148">History</span></span>
<span data-ttu-id="a0c42-149">Hello **előzmények** lap megjeleníti a minden feladat végrehajtása a kiválasztott metrikák hello rendszer hello kijelölt feladat.</span><span class="sxs-lookup"><span data-stu-id="a0c42-149">hello **History** tab displays selected metrics for every job execution in hello system for hello selected job.</span></span> <span data-ttu-id="a0c42-150">A metrikák a Feladatütemező hello állapotának valós idejű értékeit tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="a0c42-150">These metrics provide real-time values regarding hello health of your Scheduler:</span></span>

1. <span data-ttu-id="a0c42-151">status</span><span class="sxs-lookup"><span data-stu-id="a0c42-151">Status</span></span>  
2. <span data-ttu-id="a0c42-152">Részletek</span><span class="sxs-lookup"><span data-stu-id="a0c42-152">Details</span></span>  
3. <span data-ttu-id="a0c42-153">Újrapróbálkozási kísérletek</span><span class="sxs-lookup"><span data-stu-id="a0c42-153">Retry attempts</span></span>
4. <span data-ttu-id="a0c42-154">Előfordulás: 1., 2., 3. stb.</span><span class="sxs-lookup"><span data-stu-id="a0c42-154">Occurrence: 1st, 2nd, 3rd, etc.</span></span>
5. <span data-ttu-id="a0c42-155">A futtatás kezdési időpontja</span><span class="sxs-lookup"><span data-stu-id="a0c42-155">Start time of execution</span></span>  
6. <span data-ttu-id="a0c42-156">A futtatás befejezési időpontja</span><span class="sxs-lookup"><span data-stu-id="a0c42-156">End time of execution</span></span>
   
   ![][job-history]

<span data-ttu-id="a0c42-157">Rákattinthat a futtatási tooview a **előzmények részletek**, beleértve a hello teljes válasz minden végrehajtásra.</span><span class="sxs-lookup"><span data-stu-id="a0c42-157">You can click on a run tooview its **History Details**, including hello whole response for every execution.</span></span> <span data-ttu-id="a0c42-158">Ezen a párbeszédpanelen is lehetővé teszi toocopy hello válasz toohello vágólapra.</span><span class="sxs-lookup"><span data-stu-id="a0c42-158">This dialog box also allows you toocopy hello response toohello clipboard.</span></span>

   ![][job-history-details]

### <a name="users"></a><span data-ttu-id="a0c42-159">Felhasználók</span><span class="sxs-lookup"><span data-stu-id="a0c42-159">Users</span></span>
<span data-ttu-id="a0c42-160">Az Azure Szerepköralapú hozzáférés-vezérlés (RBAC) részletesen beállítható hozzáférés-vezérlést biztosít az Azure Scheduler szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="a0c42-160">Azure Role-Based Access Control (RBAC) enables fine-grained access management for Azure Scheduler.</span></span> <span data-ttu-id="a0c42-161">Hogyan toouse hello felhasználók lapon tekintse meg a túl toolearn[átruházásához hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md)</span><span class="sxs-lookup"><span data-stu-id="a0c42-161">toolearn how toouse hello Users tab, refer too[Azure Role-Based Access Control](../active-directory/role-based-access-control-configure.md)</span></span>

## <a name="see-also"></a><span data-ttu-id="a0c42-162">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="a0c42-162">See also</span></span>
 [<span data-ttu-id="a0c42-163">A Scheduler ismertetése</span><span class="sxs-lookup"><span data-stu-id="a0c42-163">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="a0c42-164">A Scheduler alapfogalmai, terminológiája és entitáshierarchiája</span><span class="sxs-lookup"><span data-stu-id="a0c42-164">Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="a0c42-165">Csomagok és számlázás az Azure Schedulerben</span><span class="sxs-lookup"><span data-stu-id="a0c42-165">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="a0c42-166">Hogyan ütemezi a toobuild összetett és speciális ismétlődési és Azure-ütemező</span><span class="sxs-lookup"><span data-stu-id="a0c42-166">How toobuild complex schedules and advanced recurrence with Azure Scheduler</span></span>](scheduler-advanced-complexity.md)

 [<span data-ttu-id="a0c42-167">A Scheduler REST API-jának leírása</span><span class="sxs-lookup"><span data-stu-id="a0c42-167">Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="a0c42-168">A Scheduler PowerShell-parancsmagjainak leírása</span><span class="sxs-lookup"><span data-stu-id="a0c42-168">Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="a0c42-169">Scheduler – magas fokú rendelkezésre állás és megbízhatóság</span><span class="sxs-lookup"><span data-stu-id="a0c42-169">Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="a0c42-170">Scheduler – korlátozások, alapértékek és hibakódok</span><span class="sxs-lookup"><span data-stu-id="a0c42-170">Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

 [<span data-ttu-id="a0c42-171">Kimenő hitelesítés a Schedulerben</span><span class="sxs-lookup"><span data-stu-id="a0c42-171">Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

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
