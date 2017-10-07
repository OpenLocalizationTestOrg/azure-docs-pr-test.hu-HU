---
title: "a webjobs-feladatok aaaRun háttérfeladatok"
description: "Ismerje meg, hogyan toorun háttérfeladatok az Azure-webalkalmazások."
services: app-service
documentationcenter: 
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: af01771e-54eb-4aea-af5f-f883ff39572b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2016
ms.author: glenga
ms.openlocfilehash: 96a3d977a806e7192207f0f4da79dfd248694336
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-background-tasks-with-webjobs"></a><span data-ttu-id="b6549-103">Háttérfeladatok futtatása WebJobs-feladatokkal</span><span class="sxs-lookup"><span data-stu-id="b6549-103">Run Background tasks with WebJobs</span></span>
## <a name="overview"></a><span data-ttu-id="b6549-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b6549-104">Overview</span></span>
<span data-ttu-id="b6549-105">Programok vagy parancsfájlok futtathatók a webjobs-feladatok a [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webalkalmazás háromféleképpen: az igény szerinti folyamatosan, vagy ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="b6549-105">You can run programs or scripts in WebJobs in your [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) web app in three ways: on demand, continuously, or on a schedule.</span></span> <span data-ttu-id="b6549-106">Nincs nem jelent többletköltséget toouse webjobs-feladatok.</span><span class="sxs-lookup"><span data-stu-id="b6549-106">There is no additional cost toouse WebJobs.</span></span>

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

<span data-ttu-id="b6549-107">Ez a cikk bemutatja, hogyan toodeploy webjobs-feladatok használatával hello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b6549-107">This article shows how toodeploy WebJobs by using hello [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="b6549-108">További információ a Visual Studio vagy egy folyamatos kézbesítési folyamatának toodeploy lásd [hogyan tooDeploy Azure webjobs-feladatok tooWeb alkalmazások](websites-dotnet-deploy-webjobs.md).</span><span class="sxs-lookup"><span data-stu-id="b6549-108">For information about how toodeploy by using Visual Studio or a continuous delivery process, see [How tooDeploy Azure WebJobs tooWeb Apps](websites-dotnet-deploy-webjobs.md).</span></span>

<span data-ttu-id="b6549-109">hello Azure WebJobs SDK számos programozási WebJobs-feladatok egyszerűbbé teszi.</span><span class="sxs-lookup"><span data-stu-id="b6549-109">hello Azure WebJobs SDK simplifies many WebJobs programming tasks.</span></span> <span data-ttu-id="b6549-110">További információkért lásd: [hello WebJobs SDK Újdonságok](websites-dotnet-webjobs-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="b6549-110">For more information, see [What is hello WebJobs SDK](websites-dotnet-webjobs-sdk.md).</span></span>

 <span data-ttu-id="b6549-111">Az Azure Functions biztosít egy másik módja toorun programokat és parancsfájlokat vagy a kiszolgáló nélküli környezetekben, illetve egy App Service-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b6549-111">Azure Functions provides another way toorun programs and scripts from either a serverless environment or from an App Service app.</span></span> <span data-ttu-id="b6549-112">További információkért lásd: [Azure Functions áttekintése](../azure-functions/functions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b6549-112">For more information, see [Azure Functions overview](../azure-functions/functions-overview.md).</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <span data-ttu-id="b6549-113"><a name="acceptablefiles"></a>Elfogadható fájltípusait ismerteti programok és parancsfájlok</span><span class="sxs-lookup"><span data-stu-id="b6549-113"><a name="acceptablefiles"></a>Acceptable file types for scripts or programs</span></span>
<span data-ttu-id="b6549-114">a következő fájltípusokat hello elfogadottak:</span><span class="sxs-lookup"><span data-stu-id="b6549-114">hello following file types are accepted:</span></span>

* <span data-ttu-id="b6549-115">.cmd, .bat, .exe (a windows parancssori eszközzel)</span><span class="sxs-lookup"><span data-stu-id="b6549-115">.cmd, .bat, .exe (using windows cmd)</span></span>
* <span data-ttu-id="b6549-116">.ps1 (a powershell használatával)</span><span class="sxs-lookup"><span data-stu-id="b6549-116">.ps1 (using powershell)</span></span>
* <span data-ttu-id="b6549-117">.SH (a bash használatával)</span><span class="sxs-lookup"><span data-stu-id="b6549-117">.sh (using bash)</span></span>
* <span data-ttu-id="b6549-118">.php (a php használatával)</span><span class="sxs-lookup"><span data-stu-id="b6549-118">.php (using php)</span></span>
* <span data-ttu-id="b6549-119">.PY (python használatával)</span><span class="sxs-lookup"><span data-stu-id="b6549-119">.py (using python)</span></span>
* <span data-ttu-id="b6549-120">.js (a csomópont használatával)</span><span class="sxs-lookup"><span data-stu-id="b6549-120">.js (using node)</span></span>
* <span data-ttu-id="b6549-121">a .JAR fájlt (a java)</span><span class="sxs-lookup"><span data-stu-id="b6549-121">.jar (using java)</span></span>

## <span data-ttu-id="b6549-122"><a name="CreateOnDemand"></a>Hozzon létre egy on igény szerinti webjobs-feladat hello portálon</span><span class="sxs-lookup"><span data-stu-id="b6549-122"><a name="CreateOnDemand"></a>Create an on demand WebJob in hello portal</span></span>
1. <span data-ttu-id="b6549-123">A hello **Web App** panelen található hello [Azure Portal](https://portal.azure.com), kattintson a **összes beállítás > webjobs-feladatok** tooshow hello **webjobs-feladatok** panelen.</span><span class="sxs-lookup"><span data-stu-id="b6549-123">In hello **Web App** blade of hello [Azure Portal](https://portal.azure.com), click **All settings > WebJobs** tooshow hello **WebJobs** blade.</span></span>
   
    ![Webjobs-feladat panelen](./media/web-sites-create-web-jobs/wjblade.png)
2. <span data-ttu-id="b6549-125">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="b6549-125">Click **Add**.</span></span> <span data-ttu-id="b6549-126">Hello **hozzáadása webjobs-feladat** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b6549-126">hello **Add WebJob** dialog appears.</span></span>
   
    ![Adja hozzá a webjobs-feladat panelen](./media/web-sites-create-web-jobs/addwjblade.png)
3. <span data-ttu-id="b6549-128">A **neve**, adjon meg egy nevet hello webjobs-feladat.</span><span class="sxs-lookup"><span data-stu-id="b6549-128">Under **Name**, provide a name for hello WebJob.</span></span> <span data-ttu-id="b6549-129">hello nevének betűvel vagy számmal kell kezdődnie, és nem tartalmazhat különleges karaktereket eltérő "-" és "_".</span><span class="sxs-lookup"><span data-stu-id="b6549-129">hello name must start with a letter or a number and cannot contain any special characters other than "-" and "_".</span></span>
4. <span data-ttu-id="b6549-130">A hello **hogyan tooRun** válassza **igény szerint futtatható**.</span><span class="sxs-lookup"><span data-stu-id="b6549-130">In hello **How tooRun** box, choose **Run on Demand**.</span></span>
5. <span data-ttu-id="b6549-131">A hello **fájlfeltöltés** mezőben, hello mappa ikonra, és keresse meg a parancsfájlt tartalmazó toohello zip-fájl.</span><span class="sxs-lookup"><span data-stu-id="b6549-131">In hello **File Upload** box, click hello folder icon and browse toohello zip file that contains your script.</span></span> <span data-ttu-id="b6549-132">hello zip-fájl tartalmaznia kell a végrehajtható fájl (.exe .cmd .bat .sh .php .py .js) és minden kiegészítő fájlt szükséges toorun hello program vagy parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="b6549-132">hello zip file should contain your executable (.exe .cmd .bat .sh .php .py .js) as well as any supporting files needed toorun hello program or script.</span></span>
6. <span data-ttu-id="b6549-133">Ellenőrizze **létrehozása** tooupload hello parancsfájl tooyour webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b6549-133">Check **Create** tooupload hello script tooyour web app.</span></span> 
   
    <span data-ttu-id="b6549-134">hello hello webjobs-feladat a megadott néven jelennek meg hello listáján a hello **WebJobs** panelen.</span><span class="sxs-lookup"><span data-stu-id="b6549-134">hello name you specified for hello WebJob appears in hello list on hello **WebJobs** blade.</span></span>
7. <span data-ttu-id="b6549-135">toorun hello webjobs-feladat, kattintson a jobb gombbal a nevére hello listán, és kattintson a **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="b6549-135">toorun hello WebJob, right-click its name in hello list and click **Run**.</span></span>
   
    ![Webjobs-feladat futtatása](./media/web-sites-create-web-jobs/runondemand.png)

## <span data-ttu-id="b6549-137"><a name="CreateContinuous"></a>Egy folyamatosan futó webjobs-feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6549-137"><a name="CreateContinuous"></a>Create a continuously running WebJob</span></span>
1. <span data-ttu-id="b6549-138">toocreate folyamatosan futó webjobs-feladat, hajtsa végre az azonos lépések a webjobs-feladat létrehozása, amely csak egyszer fut, de a hello hello **hogyan tooRun** válassza **folyamatos**.</span><span class="sxs-lookup"><span data-stu-id="b6549-138">toocreate a continuously executing WebJob, follow hello same steps for creating a WebJob that runs once, but in hello **How tooRun** box, choose **Continuous**.</span></span>
2. <span data-ttu-id="b6549-139">toostart vagy állítsa le a folyamatos webjobs-feladat, kattintson a jobb gombbal hello listán, és kattintson a webjobs-feladat hello **Start** vagy **leállítása**.</span><span class="sxs-lookup"><span data-stu-id="b6549-139">toostart or stop a continuous WebJob, right-click hello WebJob in hello list and click **Start** or **Stop**.</span></span>

> [!NOTE]
> <span data-ttu-id="b6549-140">Ha egynél több példány a webalkalmazás fut, folyamatosan futó webjobs-feladat fut az összes olyan saját példányait.</span><span class="sxs-lookup"><span data-stu-id="b6549-140">If your web app runs on more than one instance, a continuously running WebJob will run on all of your instances.</span></span> <span data-ttu-id="b6549-141">Igény szerinti és az ütemezett webjobs-feladatok futtatása a Microsoft Azure terheléselosztást kijelölt egyetlen példányán.</span><span class="sxs-lookup"><span data-stu-id="b6549-141">On-demand and scheduled WebJobs run on a single instance selected for load balancing by Microsoft Azure.</span></span>
> 
> <span data-ttu-id="b6549-142">A folyamatos webjobs-feladatok toorun megbízhatóan és összes példánya lehetővé teszik a mindig bekapcsolva hello * hello webalkalmazás egyéb azok leállíthatja, ha túl sokáig üresjáratban hello SCM állomás hely konfigurációs beállítást.</span><span class="sxs-lookup"><span data-stu-id="b6549-142">For Continuous WebJobs toorun reliably and on all instances, enable hello Always On* configuration setting for hello web app otherwise they can stop running when hello SCM host site has been idle for too long.</span></span>
> 
> 

## <span data-ttu-id="b6549-143"><a name="CreateScheduledCRON"></a>Hozzon létre egy ütemezett webjobs-feladat CRON-kifejezés használata</span><span class="sxs-lookup"><span data-stu-id="b6549-143"><a name="CreateScheduledCRON"></a>Create a scheduled WebJob using a CRON expression</span></span>
<span data-ttu-id="b6549-144">Ezzel a technikával Basic, Standard vagy prémium módban futó elérhető tooWeb alkalmazásokat, és hello igényel **mindig a** toobe hello alkalmazás engedélyezve beállítás.</span><span class="sxs-lookup"><span data-stu-id="b6549-144">This technique is available tooWeb Apps running in Basic, Standard or Premium mode, and requires hello **Always On** setting toobe enabled on hello app.</span></span>

<span data-ttu-id="b6549-145">tooturn egy az igény szerinti webjobs-feladat az ütemezett webjobs-feladat, egyszerűen közé tartozik egy `settings.job` fájlban a következő hello a webjobs-feladat zip-fájl gyökerében.</span><span class="sxs-lookup"><span data-stu-id="b6549-145">tooturn an On Demand WebJob into a scheduled WebJob, simply include a `settings.job` file at hello root of your WebJob zip file.</span></span> <span data-ttu-id="b6549-146">A JSON-fájl tartalmaznia kell egy `schedule` tulajdonság egy [CRON-kifejezés](https://en.wikipedia.org/wiki/Cron), az alábbi példában egy.</span><span class="sxs-lookup"><span data-stu-id="b6549-146">This JSON file should include a `schedule` property with a [CRON expression](https://en.wikipedia.org/wiki/Cron), per example below.</span></span>

<span data-ttu-id="b6549-147">hello CRON-kifejezés áll 6 mezők: `{second} {minute} {hour} {day} {month} {day of hello week}`.</span><span class="sxs-lookup"><span data-stu-id="b6549-147">hello CRON expression is composed of 6 fields: `{second} {minute} {hour} {day} {month} {day of hello week}`.</span></span>

<span data-ttu-id="b6549-148">Például tootrigger a webjobs-feladat 15 percenként a `settings.job` kellene lennie:</span><span class="sxs-lookup"><span data-stu-id="b6549-148">For example, tootrigger your WebJob every 15 minutes, your `settings.job` would have:</span></span>

```json
{
    "schedule": "0 */15 * * * *"
}
``` 

<span data-ttu-id="b6549-149">Más CRON-ütemezését példák:</span><span class="sxs-lookup"><span data-stu-id="b6549-149">Other CRON schedule examples:</span></span>

* <span data-ttu-id="b6549-150">Minden órában (azaz mindig, amikor perc hello száma 0):`0 0 * * * *`</span><span class="sxs-lookup"><span data-stu-id="b6549-150">Every hour (i.e. whenever hello count of minutes is 0): `0 0 * * * *`</span></span> 
* <span data-ttu-id="b6549-151">A Reggel 9 too5 óránként PM:`0 0 9-17 * * *`</span><span class="sxs-lookup"><span data-stu-id="b6549-151">Every hour from 9 AM too5 PM: `0 0 9-17 * * *`</span></span> 
* <span data-ttu-id="b6549-152">A 9:30 AM minden nap:`0 30 9 * * *`</span><span class="sxs-lookup"><span data-stu-id="b6549-152">At 9:30 AM every day: `0 30 9 * * *`</span></span>
* <span data-ttu-id="b6549-153">A 9:30 AM hét naponta:`0 30 9 * * 1-5`</span><span class="sxs-lookup"><span data-stu-id="b6549-153">At 9:30 AM every week day: `0 30 9 * * 1-5`</span></span>

<span data-ttu-id="b6549-154">**Megjegyzés:**: a Visual Studio eszközből webjobs-feladat való telepítésekor, győződjön meg arról, hogy toomark a `settings.job` fájl tulajdonságai "Ha újabb másolatát".</span><span class="sxs-lookup"><span data-stu-id="b6549-154">**Note**: when deploying a WebJob from Visual Studio, make sure toomark your `settings.job` file properties as 'Copy if newer'.</span></span>

## <span data-ttu-id="b6549-155"><a name="CreateScheduled"></a>Hello Azure Scheduler használatával ütemezett webjobs-feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6549-155"><a name="CreateScheduled"></a>Create a scheduled WebJob using hello Azure Scheduler</span></span>
<span data-ttu-id="b6549-156">a következő alternatív módszer hello hello Azure Scheduler használ.</span><span class="sxs-lookup"><span data-stu-id="b6549-156">hello following alternate technique makes use of hello Azure Scheduler.</span></span> <span data-ttu-id="b6549-157">Ebben az esetben a webjobs-feladat nincs hello ütemezés közvetlen ismerete.</span><span class="sxs-lookup"><span data-stu-id="b6549-157">In this case, your WebJob does not have any direct knowledge of hello schedule.</span></span> <span data-ttu-id="b6549-158">Ehelyett hello Azure Scheduler lekérdezi konfigurált tootrigger a webjobs-feladat ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="b6549-158">Instead, hello Azure Scheduler gets configured tootrigger your WebJob on a schedule.</span></span> 

<span data-ttu-id="b6549-159">hello Azure portálon még nem kapott hello képességét toocreate ütemezett webjobs-feladat, de amíg nincs hozzáadva a szolgáltatást is elvégezheti a hello [klasszikus portál](http://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="b6549-159">hello Azure Portal doesn't yet have hello ability toocreate a scheduled WebJob, but until that feature is added you can do it by using hello [classic portal](http://manage.windowsazure.com).</span></span>

1. <span data-ttu-id="b6549-160">A hello [klasszikus portál](http://manage.windowsazure.com) lépjen toohello webjobs-feladat lapra, és kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="b6549-160">In hello [classic portal](http://manage.windowsazure.com) go toohello WebJob page and click **Add**.</span></span>
2. <span data-ttu-id="b6549-161">A hello **hogyan tooRun** válassza **ütemezhető**.</span><span class="sxs-lookup"><span data-stu-id="b6549-161">In hello **How tooRun** box, choose **Run on a schedule**.</span></span>
   
    ![Új ütemezett feladatot][NewScheduledJob]
3. <span data-ttu-id="b6549-163">Válassza ki a hello **Feladatütemező régió** a feladat, majd hello hello jobb alsó sarkában hello párbeszédpanel tooproceed toohello következő képernyőn látható nyílra.</span><span class="sxs-lookup"><span data-stu-id="b6549-163">Choose hello **Scheduler Region** for your job, and then click hello arrow on hello bottom right of hello dialog tooproceed toohello next screen.</span></span>
4. <span data-ttu-id="b6549-164">A hello **feladat létrehozása** párbeszédpanelen válassza ki, hello típusú **ismétlődési** kívánt: **egyszeri feladat** vagy **ismétlődő feladat**.</span><span class="sxs-lookup"><span data-stu-id="b6549-164">In hello **Create Job** dialog, choose hello type of **Recurrence** you want: **One-time job** or **Recurring job**.</span></span>
   
    ![Ismétlődés ütemezése][SchdRecurrence]
5. <span data-ttu-id="b6549-166">Kiválaszthat egy **indítása** idő: **most** vagy **adott időpontban**.</span><span class="sxs-lookup"><span data-stu-id="b6549-166">Also choose a **Starting** time: **Now** or **At a specific time**.</span></span>
   
    ![Kezdő időpontjának ütemezése][SchdStart]
6. <span data-ttu-id="b6549-168">Ha toostart adott időpontban, válassza ki a kezdési idő értékei alapján **indítása a**.</span><span class="sxs-lookup"><span data-stu-id="b6549-168">If you want toostart at a specific time, choose your starting time values under **Starting On**.</span></span>
   
    ![Egy adott időpontban ütemezés kezdési][SchdStartOn]
7. <span data-ttu-id="b6549-170">Ha úgy dönt, hogy ismétlődő feladat, hogy hello **Ismétlődés minden** toospecify hello gyakorisága előfordulási és hello lehetőséget **befejezési a** beállítás toospecify befejezési időpont.</span><span class="sxs-lookup"><span data-stu-id="b6549-170">If you chose a recurring job, you have hello **Recur Every** option toospecify hello frequency of occurrence and hello **Ending On** option toospecify an ending time.</span></span>
   
    ![Ismétlődés ütemezése][SchdRecurEvery]
8. <span data-ttu-id="b6549-172">Ha úgy dönt, **hét**, kiválaszthatja a hello **az egy adott ütemezés** és adja meg a hello napot hello kívánt hello feladat toorun.</span><span class="sxs-lookup"><span data-stu-id="b6549-172">If you choose **Weeks**, you can select hello **On a Particular Schedule** box and specify hello days of hello week that you want hello job toorun.</span></span>
   
    ![Hello hét napjai ütemezése][SchdWeeksOnParticular]
9. <span data-ttu-id="b6549-174">Ha úgy dönt, **hónap** és select hello **az egy adott ütemezés** mezőben állíthatja be hello feladat toorun számozott adott **nap** hello hónapban.</span><span class="sxs-lookup"><span data-stu-id="b6549-174">If you choose **Months** and select hello **On a Particular Schedule** box, you can set hello job toorun on particular numbered **Days** in hello month.</span></span> 
   
    ![Hello hónapban az adott dátumok ütemezése][SchdMonthsOnPartDays]
10. <span data-ttu-id="b6549-176">Ha úgy dönt, **hét napjai**, mely nap vagy hello hét napjai kiválaszthatja az azt szeretné, hogy a feladat toorun hello hello hónap.</span><span class="sxs-lookup"><span data-stu-id="b6549-176">If you choose **Week Days**, you can select which day or days of hello week in hello month you want hello job toorun on.</span></span>
    
     ![A hónap adott hét napjai ütemezése][SchdMonthsOnPartWeekDays]
11. <span data-ttu-id="b6549-178">Végezetül is használhatja hello **előfordulások** beállítás toochoose hello hónap hányadik hetére (először, a második, harmadik stb.) hello feladat toorun hello a hét napjai megadott szeretné.</span><span class="sxs-lookup"><span data-stu-id="b6549-178">Finally, you can also use hello **Occurrences** option toochoose which week in hello month (first, second, third etc.) you want hello job toorun on hello week days you specified.</span></span>
    
    ![A hónap adott héten adott hét napjai ütemezése][SchdMonthsOnPartWeekDaysOccurences]
12. <span data-ttu-id="b6549-180">Egy vagy több feladat létrehozását követően a nevük azok állapotát, az ütemezés típusa és egyéb információkat hello webjobs-feladatok lapon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b6549-180">After you have created one or more jobs, their names will appear on hello WebJobs tab with their status, schedule type, and other information.</span></span> <span data-ttu-id="b6549-181">Korábbi hello utolsó 30 webjobs-feladatok a tárolt adatok.</span><span class="sxs-lookup"><span data-stu-id="b6549-181">Historical information for hello last 30  WebJobs is maintained.</span></span>
    
    ![Feladatok listája][WebJobsListWithSeveralJobs]

### <span data-ttu-id="b6549-183"><a name="Scheduler"></a>Ütemezett feladatok és az Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="b6549-183"><a name="Scheduler"></a>Scheduled jobs and Azure Scheduler</span></span>
<span data-ttu-id="b6549-184">Ütemezett feladatok további konfigurálásra hello Azure Scheduler lapján hello [klasszikus portál](http://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="b6549-184">Scheduled jobs can be further configured in hello Azure Scheduler pages of hello [classic portal](http://manage.windowsazure.com).</span></span>

1. <span data-ttu-id="b6549-185">Hello webjobs-feladatok lapon kattintson a hello feladat **ütemezés** hivatkozás toonavigate toohello Azure Scheduler portálon.</span><span class="sxs-lookup"><span data-stu-id="b6549-185">On hello WebJobs page, click hello job's **schedule** link toonavigate toohello Azure Scheduler portal page.</span></span> 
   
   ![Hivatkozás tooAzure Feladatütemező][LinkToScheduler]
2. <span data-ttu-id="b6549-187">Hello Feladatütemező lapján kattintson a hello feladat.</span><span class="sxs-lookup"><span data-stu-id="b6549-187">On hello Scheduler page, click hello job.</span></span>
   
    ![Feladat hello Feladatütemező portál lapján][SchedulerPortal]
3. <span data-ttu-id="b6549-189">Hello **Feladatművelet** lap megnyitásakor, ahol további konfigurálható hello feladat.</span><span class="sxs-lookup"><span data-stu-id="b6549-189">hello **Job Action** page opens, where you can further configure hello job.</span></span> 
   
    ![Feladatművelet PageInScheduler][JobActionPageInScheduler]

## <span data-ttu-id="b6549-191"><a name="ViewJobHistory"></a>Hello-feladat előzményeinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="b6549-191"><a name="ViewJobHistory"></a>View hello job history</span></span>
1. <span data-ttu-id="b6549-192">tooview hello futtatási előzményei feladat, többek között a WebJobs SDK hello létrehozott feladatokat, kattintson a megfelelő hivatkozásra a hello **naplók** oszlop hello WebJobs panelről.</span><span class="sxs-lookup"><span data-stu-id="b6549-192">tooview hello execution history of a job, including jobs created with hello WebJobs SDK, click  its corresponding link under hello **Logs** column of hello WebJobs blade.</span></span> <span data-ttu-id="b6549-193">(Használhatja hello vágólapra ikon toocopy hello URL-címet a hello napló fájl lap toohello vágólap Ha.)</span><span class="sxs-lookup"><span data-stu-id="b6549-193">(You can use hello clipboard icon toocopy hello URL of hello log file page toohello clipboard if you wish.)</span></span>
   
    ![Naplók hivatkozás](./media/web-sites-create-web-jobs/wjbladelogslink.png)
2. <span data-ttu-id="b6549-195">Gombra kattintva hello a hivatkozás megnyitja hello részleteit megjelenítő oldalon hello webjobs-feladat.</span><span class="sxs-lookup"><span data-stu-id="b6549-195">Clicking hello link opens hello details page for hello WebJob.</span></span> <span data-ttu-id="b6549-196">A lap tartalmazza, akkor hello neve hello parancs futtatása, a legutóbbi alkalommal lefutott, és annak sikerességét vagy sikertelenségét hello.</span><span class="sxs-lookup"><span data-stu-id="b6549-196">This page shows you hello name of hello command run, hello last times it ran, and its success or failure.</span></span> <span data-ttu-id="b6549-197">A **legutóbbi feladat futtatása**, kattintson a idő toosee további részleteket.</span><span class="sxs-lookup"><span data-stu-id="b6549-197">Under **Recent job runs**, click a time toosee further details.</span></span>
   
    ![WebJobDetails][WebJobDetails]
3. <span data-ttu-id="b6549-199">Hello **webjobs-feladat futtatása részletek** lap jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b6549-199">hello **WebJob Run Details** page appears.</span></span> <span data-ttu-id="b6549-200">Kattintson a **váltása kimeneti** hello naplók tartalmáról toosee hello szövegét.</span><span class="sxs-lookup"><span data-stu-id="b6549-200">Click **Toggle Output** toosee hello text of hello log contents.</span></span> <span data-ttu-id="b6549-201">hello kimeneti naplót szöveg formátumban van.</span><span class="sxs-lookup"><span data-stu-id="b6549-201">hello output log is in text format.</span></span> 
   
    ![Webes feladat futtatása részletei][WebJobRunDetails]
4. <span data-ttu-id="b6549-203">toosee hello kimeneti szöveg egy külön böngészőablakban kattintson hello **letöltése** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="b6549-203">toosee hello output text in a separate browser window, click hello **download** link.</span></span> <span data-ttu-id="b6549-204">toodownload hello szöveg magát, kattintson a jobb gombbal a hello hivatkozásra, és használja a böngésző beállításai toosave hello fájl tartalmát.</span><span class="sxs-lookup"><span data-stu-id="b6549-204">toodownload hello text itself, right-click hello link and use your browser options toosave hello file contents.</span></span>
   
    ![Töltse le a kimenet][DownloadLogOutput]
5. <span data-ttu-id="b6549-206">Hello **WebJobs** hivatkozás hello oldal hello tetején a webjobs-feladatok kényelmesen tooget tooa listáját tartalmazza hello előzmények irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="b6549-206">hello **WebJobs** link at hello top of hello page provides a convenient way tooget tooa list of WebJobs on hello history dashboard.</span></span>
   
    ![Hivatkozás tooWebJobs listája][WebJobsLinkToDashboardList]
   
    ![Webjobs-feladatok előzményeinek irányítópulton listája][WebJobsListInJobsDashboard]
   
    <span data-ttu-id="b6549-209">Kattintson az alábbi hivatkozások viszi toohello webjobs-feladat részletei lapon kiválasztott hello feladat.</span><span class="sxs-lookup"><span data-stu-id="b6549-209">Clicking one of these links takes you toohello WebJob Details page for hello job you selected.</span></span>

## <span data-ttu-id="b6549-210"><a name="WHPNotes"></a>Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="b6549-210"><a name="WHPNotes"></a>Notes</span></span>
* <span data-ttu-id="b6549-211">Webalkalmazások szabad módban a túllépi az időkorlátot 20 perc után is, ha nincs kérelmek toohello scm (telepítés) helyen, és hello web app portal nincs megnyitva az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="b6549-211">Web apps in Free mode can time out after 20 minutes if there are no requests toohello scm (deployment) site and hello web app's portal is not open in Azure.</span></span> <span data-ttu-id="b6549-212">Kérelmek toohello tényleges hely ez nem alaphelyzetbe állnak.</span><span class="sxs-lookup"><span data-stu-id="b6549-212">Requests toohello actual site will not reset this.</span></span>
* <span data-ttu-id="b6549-213">Folyamatos feladat kódot kell toobe toorun írt ki végtelen ciklus.</span><span class="sxs-lookup"><span data-stu-id="b6549-213">Code for a continuous job needs toobe written toorun in an endless loop.</span></span>
* <span data-ttu-id="b6549-214">Folyamatos feladatok folyamatosan futnak, csak akkor, ha hello a webalkalmazás működik-e.</span><span class="sxs-lookup"><span data-stu-id="b6549-214">Continuous jobs run continuously only when hello web app is up.</span></span>
* <span data-ttu-id="b6549-215">Alapszintű és a szabványos mód ajánlat hello mindig a beállítást, amely, ha engedélyezve van, megakadályozza a webalkalmazások tétlenné.</span><span class="sxs-lookup"><span data-stu-id="b6549-215">Basic and Standard modes offer hello Always On feature which, when enabled, prevents web apps from becoming idle.</span></span>
* <span data-ttu-id="b6549-216">Folyamatosan futó webjobs-feladatok csak hibakeresési.</span><span class="sxs-lookup"><span data-stu-id="b6549-216">You can only debug continuously running WebJobs.</span></span> <span data-ttu-id="b6549-217">Ütemezett vagy igény szerinti WebJobs-hibakeresés nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="b6549-217">Debugging scheduled or on-demand WebJobs is not supported.</span></span>

## <span data-ttu-id="b6549-218"><a name="NextSteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b6549-218"><a name="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="b6549-219">További információkért lásd: [Azure webjobs-feladatok ajánlott erőforrások][WebJobsRecommendedResources].</span><span class="sxs-lookup"><span data-stu-id="b6549-219">For more information, see [Azure WebJobs Recommended Resources][WebJobsRecommendedResources].</span></span>

[PSonWebJobs]:http://blogs.msdn.com/b/nicktrog/archive/2014/01/22/running-powershell-web-jobs-on-azure-websites.aspx
[WebJobsRecommendedResources]:http://go.microsoft.com/fwlink/?LinkId=390226

[OnDemandWebJob]: ./media/web-sites-create-web-jobs/01aOnDemandWebJob.png
[WebJobsList]: ./media/web-sites-create-web-jobs/02aWebJobsList.png
[NewContinuousJob]: ./media/web-sites-create-web-jobs/03aNewContinuousJob.png
[NewScheduledJob]: ./media/web-sites-create-web-jobs/04aNewScheduledJob.png
[SchdRecurrence]: ./media/web-sites-create-web-jobs/05SchdRecurrence.png
[SchdStart]: ./media/web-sites-create-web-jobs/06SchdStart.png
[SchdStartOn]: ./media/web-sites-create-web-jobs/07SchdStartOn.png
[SchdRecurEvery]: ./media/web-sites-create-web-jobs/08SchdRecurEvery.png
[SchdWeeksOnParticular]: ./media/web-sites-create-web-jobs/09SchdWeeksOnParticular.png
[SchdMonthsOnPartDays]: ./media/web-sites-create-web-jobs/10SchdMonthsOnPartDays.png
[SchdMonthsOnPartWeekDays]: ./media/web-sites-create-web-jobs/11SchdMonthsOnPartWeekDays.png
[SchdMonthsOnPartWeekDaysOccurences]: ./media/web-sites-create-web-jobs/12SchdMonthsOnPartWeekDaysOccurences.png
[RunOnce]: ./media/web-sites-create-web-jobs/13RunOnce.png
[WebJobsListWithSeveralJobs]: ./media/web-sites-create-web-jobs/13WebJobsListWithSeveralJobs.png
[WebJobLogs]: ./media/web-sites-create-web-jobs/14WebJobLogs.png
[WebJobDetails]: ./media/web-sites-create-web-jobs/15WebJobDetails.png
[WebJobRunDetails]: ./media/web-sites-create-web-jobs/16WebJobRunDetails.png
[DownloadLogOutput]: ./media/web-sites-create-web-jobs/17DownloadLogOutput.png
[WebJobsLinkToDashboardList]: ./media/web-sites-create-web-jobs/18WebJobsLinkToDashboardList.png
[WebJobsListInJobsDashboard]: ./media/web-sites-create-web-jobs/19WebJobsListInJobsDashboard.png
[LinkToScheduler]: ./media/web-sites-create-web-jobs/31LinkToScheduler.png
[SchedulerPortal]: ./media/web-sites-create-web-jobs/32SchedulerPortal.png
[JobActionPageInScheduler]: ./media/web-sites-create-web-jobs/33JobActionPageInScheduler.png

