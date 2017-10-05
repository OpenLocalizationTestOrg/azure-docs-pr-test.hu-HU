---
title: "Háttérfeladatok futtatása WebJobs-feladatokkal"
description: "Útmutató az Azure web apps háttér feladatok futtatásához."
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
ms.openlocfilehash: 3f8ae748e3d9c6b4e342536926a52b4e8f37ee51
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="run-background-tasks-with-webjobs"></a><span data-ttu-id="fe81c-103">Háttérfeladatok futtatása WebJobs-feladatokkal</span><span class="sxs-lookup"><span data-stu-id="fe81c-103">Run Background tasks with WebJobs</span></span>
## <a name="overview"></a><span data-ttu-id="fe81c-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="fe81c-104">Overview</span></span>
<span data-ttu-id="fe81c-105">Programok vagy parancsfájlok futtathatók a webjobs-feladatok a [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webalkalmazás háromféleképpen: az igény szerinti folyamatosan, vagy ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="fe81c-105">You can run programs or scripts in WebJobs in your [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) web app in three ways: on demand, continuously, or on a schedule.</span></span> <span data-ttu-id="fe81c-106">Nincs webjobs-feladatok használandó további költség nélkül.</span><span class="sxs-lookup"><span data-stu-id="fe81c-106">There is no additional cost to use WebJobs.</span></span>

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

<span data-ttu-id="fe81c-107">Ez a cikk bemutatja, hogyan telepíthetők a webjobs-feladatok segítségével a [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fe81c-107">This article shows how to deploy WebJobs by using the [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="fe81c-108">A Visual Studio vagy egy folyamatos kézbesítési folyamatának telepítésével kapcsolatos információkért lásd: [Azure WebJobs központi telepítése a Web Apps hogyan](websites-dotnet-deploy-webjobs.md).</span><span class="sxs-lookup"><span data-stu-id="fe81c-108">For information about how to deploy by using Visual Studio or a continuous delivery process, see [How to Deploy Azure WebJobs to Web Apps](websites-dotnet-deploy-webjobs.md).</span></span>

<span data-ttu-id="fe81c-109">Az Azure WebJobs SDK egyszerűbbé teszi a sok WebJobs-programozási feladathoz.</span><span class="sxs-lookup"><span data-stu-id="fe81c-109">The Azure WebJobs SDK simplifies many WebJobs programming tasks.</span></span> <span data-ttu-id="fe81c-110">További információkért lásd: [Mi az a WebJobs SDK](websites-dotnet-webjobs-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="fe81c-110">For more information, see [What is the WebJobs SDK](websites-dotnet-webjobs-sdk.md).</span></span>

 <span data-ttu-id="fe81c-111">Az Azure Functions segítségével más programok és parancsfájlok futtathatók, vagy egy kiszolgáló nélküli környezetben vagy egy App Service-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fe81c-111">Azure Functions provides another way to run programs and scripts from either a serverless environment or from an App Service app.</span></span> <span data-ttu-id="fe81c-112">További információkért lásd: [Azure Functions áttekintése](../azure-functions/functions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fe81c-112">For more information, see [Azure Functions overview](../azure-functions/functions-overview.md).</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <span data-ttu-id="fe81c-113"><a name="acceptablefiles"></a>Elfogadható fájltípusait ismerteti programok és parancsfájlok</span><span class="sxs-lookup"><span data-stu-id="fe81c-113"><a name="acceptablefiles"></a>Acceptable file types for scripts or programs</span></span>
<span data-ttu-id="fe81c-114">A következő típusú elfogadottak:</span><span class="sxs-lookup"><span data-stu-id="fe81c-114">The following file types are accepted:</span></span>

* <span data-ttu-id="fe81c-115">.cmd, .bat, .exe (a windows parancssori eszközzel)</span><span class="sxs-lookup"><span data-stu-id="fe81c-115">.cmd, .bat, .exe (using windows cmd)</span></span>
* <span data-ttu-id="fe81c-116">.ps1 (a powershell használatával)</span><span class="sxs-lookup"><span data-stu-id="fe81c-116">.ps1 (using powershell)</span></span>
* <span data-ttu-id="fe81c-117">.SH (a bash használatával)</span><span class="sxs-lookup"><span data-stu-id="fe81c-117">.sh (using bash)</span></span>
* <span data-ttu-id="fe81c-118">.php (a php használatával)</span><span class="sxs-lookup"><span data-stu-id="fe81c-118">.php (using php)</span></span>
* <span data-ttu-id="fe81c-119">.PY (python használatával)</span><span class="sxs-lookup"><span data-stu-id="fe81c-119">.py (using python)</span></span>
* <span data-ttu-id="fe81c-120">.js (a csomópont használatával)</span><span class="sxs-lookup"><span data-stu-id="fe81c-120">.js (using node)</span></span>
* <span data-ttu-id="fe81c-121">a .JAR fájlt (a java)</span><span class="sxs-lookup"><span data-stu-id="fe81c-121">.jar (using java)</span></span>

## <span data-ttu-id="fe81c-122"><a name="CreateOnDemand"></a>Az on igény szerinti webjobs-feladat létrehozása a portálon</span><span class="sxs-lookup"><span data-stu-id="fe81c-122"><a name="CreateOnDemand"></a>Create an on demand WebJob in the portal</span></span>
1. <span data-ttu-id="fe81c-123">A a **Web App** panelen található a [Azure Portal](https://portal.azure.com), kattintson a **összes beállítás > WebJobs** megjelenítése a **WebJobs** panelen.</span><span class="sxs-lookup"><span data-stu-id="fe81c-123">In the **Web App** blade of the [Azure Portal](https://portal.azure.com), click **All settings > WebJobs** to show the **WebJobs** blade.</span></span>
   
    ![Webjobs-feladat panelen](./media/web-sites-create-web-jobs/wjblade.png)
2. <span data-ttu-id="fe81c-125">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="fe81c-125">Click **Add**.</span></span> <span data-ttu-id="fe81c-126">A **hozzáadása webjobs-feladat** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="fe81c-126">The **Add WebJob** dialog appears.</span></span>
   
    ![Adja hozzá a webjobs-feladat panelen](./media/web-sites-create-web-jobs/addwjblade.png)
3. <span data-ttu-id="fe81c-128">A **neve**, adja meg a webjobs-feladat nevét.</span><span class="sxs-lookup"><span data-stu-id="fe81c-128">Under **Name**, provide a name for the WebJob.</span></span> <span data-ttu-id="fe81c-129">A név betűvel vagy számmal kell kezdődnie, és nem tartalmazhat különleges karaktereket eltérő "-" és "_".</span><span class="sxs-lookup"><span data-stu-id="fe81c-129">The name must start with a letter or a number and cannot contain any special characters other than "-" and "_".</span></span>
4. <span data-ttu-id="fe81c-130">Az a **Futtatás hogyan** válassza **igény szerint futtatható**.</span><span class="sxs-lookup"><span data-stu-id="fe81c-130">In the **How to Run** box, choose **Run on Demand**.</span></span>
5. <span data-ttu-id="fe81c-131">Az a **fájlfeltöltés** mezőben, a mappa ikonra, és keresse meg a parancsfájlt tartalmazó zip fájlt.</span><span class="sxs-lookup"><span data-stu-id="fe81c-131">In the **File Upload** box, click the folder icon and browse to the zip file that contains your script.</span></span> <span data-ttu-id="fe81c-132">A zip-fájl a végrehajtható fájl (.exe .cmd .bat .sh .php .py .js) kell tartalmaznia, valamint minden kiegészítő fájlt, a program vagy parancsfájl futtatásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="fe81c-132">The zip file should contain your executable (.exe .cmd .bat .sh .php .py .js) as well as any supporting files needed to run the program or script.</span></span>
6. <span data-ttu-id="fe81c-133">Ellenőrizze **létrehozása** töltse fel a parancsfájlt a webes alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fe81c-133">Check **Create** to upload the script to your web app.</span></span> 
   
    <span data-ttu-id="fe81c-134">A név megadott a listában megjelenik a webjobs-feladat a **WebJobs** panelen.</span><span class="sxs-lookup"><span data-stu-id="fe81c-134">The name you specified for the WebJob appears in the list on the **WebJobs** blade.</span></span>
7. <span data-ttu-id="fe81c-135">A webjobs-feladat futtatásához kattintson a jobb gombbal a nevét a listában, és kattintson a **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="fe81c-135">To run the WebJob, right-click its name in the list and click **Run**.</span></span>
   
    ![Webjobs-feladat futtatása](./media/web-sites-create-web-jobs/runondemand.png)

## <span data-ttu-id="fe81c-137"><a name="CreateContinuous"></a>Egy folyamatosan futó webjobs-feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="fe81c-137"><a name="CreateContinuous"></a>Create a continuously running WebJob</span></span>
1. <span data-ttu-id="fe81c-138">Folyamatosan futó webjobs-feladat létrehozásához kövesse a lépéseket a webjobs-feladat létrehozása, amely csak egyszer fut, de a a **Futtatás hogyan** válassza **folyamatos**.</span><span class="sxs-lookup"><span data-stu-id="fe81c-138">To create a continuously executing WebJob, follow the same steps for creating a WebJob that runs once, but in the **How to Run** box, choose **Continuous**.</span></span>
2. <span data-ttu-id="fe81c-139">Indítsa el, vagy egy folyamatos webjobs-feladat leállítása, kattintson a jobb gombbal a webjobs-feladat, a listában, és kattintson a **Start** vagy **leállítása**.</span><span class="sxs-lookup"><span data-stu-id="fe81c-139">To start or stop a continuous WebJob, right-click the WebJob in the list and click **Start** or **Stop**.</span></span>

> [!NOTE]
> <span data-ttu-id="fe81c-140">Ha egynél több példány a webalkalmazás fut, folyamatosan futó webjobs-feladat fut az összes olyan saját példányait.</span><span class="sxs-lookup"><span data-stu-id="fe81c-140">If your web app runs on more than one instance, a continuously running WebJob will run on all of your instances.</span></span> <span data-ttu-id="fe81c-141">Igény szerinti és az ütemezett webjobs-feladatok futtatása a Microsoft Azure terheléselosztást kijelölt egyetlen példányán.</span><span class="sxs-lookup"><span data-stu-id="fe81c-141">On-demand and scheduled WebJobs run on a single instance selected for load balancing by Microsoft Azure.</span></span>
> 
> <span data-ttu-id="fe81c-142">A folyamatos webjobs-feladatok futtatásához, megbízható és összes példánya, engedélyezze az Always On * konfigurációs beállítást a webalkalmazás nem lehet leállítani a módban való futáskor az SCM állomás hely le lett üresjárati idő túl hosszú.</span><span class="sxs-lookup"><span data-stu-id="fe81c-142">For Continuous WebJobs to run reliably and on all instances, enable the Always On* configuration setting for the web app otherwise they can stop running when the SCM host site has been idle for too long.</span></span>
> 
> 

## <span data-ttu-id="fe81c-143"><a name="CreateScheduledCRON"></a>Hozzon létre egy ütemezett webjobs-feladat CRON-kifejezés használata</span><span class="sxs-lookup"><span data-stu-id="fe81c-143"><a name="CreateScheduledCRON"></a>Create a scheduled WebJob using a CRON expression</span></span>
<span data-ttu-id="fe81c-144">Ez a módszer a Basic, Standard vagy prémium módban futó webalkalmazások érhető el, és van szükség a **mindig a** beállítást engedélyezni kell az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fe81c-144">This technique is available to Web Apps running in Basic, Standard or Premium mode, and requires the **Always On** setting to be enabled on the app.</span></span>

<span data-ttu-id="fe81c-145">Kapcsolja be az az igény szerinti webjobs-feladat az ütemezett webjobs-feladat, egyszerűen közé tartozik egy `settings.job` fájl a webjobs-feladat zip-fájl gyökerében.</span><span class="sxs-lookup"><span data-stu-id="fe81c-145">To turn an On Demand WebJob into a scheduled WebJob, simply include a `settings.job` file at the root of your WebJob zip file.</span></span> <span data-ttu-id="fe81c-146">A JSON-fájl tartalmaznia kell egy `schedule` tulajdonság egy [CRON-kifejezés](https://en.wikipedia.org/wiki/Cron), az alábbi példában egy.</span><span class="sxs-lookup"><span data-stu-id="fe81c-146">This JSON file should include a `schedule` property with a [CRON expression](https://en.wikipedia.org/wiki/Cron), per example below.</span></span>

<span data-ttu-id="fe81c-147">A CRON-kifejezés áll 6 mezők: `{second} {minute} {hour} {day} {month} {day of the week}`.</span><span class="sxs-lookup"><span data-stu-id="fe81c-147">The CRON expression is composed of 6 fields: `{second} {minute} {hour} {day} {month} {day of the week}`.</span></span>

<span data-ttu-id="fe81c-148">Például elindítható a webjobs-feladat 15 percenként a `settings.job` kellene lennie:</span><span class="sxs-lookup"><span data-stu-id="fe81c-148">For example, to trigger your WebJob every 15 minutes, your `settings.job` would have:</span></span>

```json
{
    "schedule": "0 */15 * * * *"
}
``` 

<span data-ttu-id="fe81c-149">Más CRON-ütemezését példák:</span><span class="sxs-lookup"><span data-stu-id="fe81c-149">Other CRON schedule examples:</span></span>

* <span data-ttu-id="fe81c-150">Minden órában (azaz mindig, amikor a perc száma 0):`0 0 * * * *`</span><span class="sxs-lookup"><span data-stu-id="fe81c-150">Every hour (i.e. whenever the count of minutes is 0): `0 0 * * * *`</span></span> 
* <span data-ttu-id="fe81c-151">Minden órában a Reggel 9 a délután 5 óra:`0 0 9-17 * * *`</span><span class="sxs-lookup"><span data-stu-id="fe81c-151">Every hour from 9 AM to 5 PM: `0 0 9-17 * * *`</span></span> 
* <span data-ttu-id="fe81c-152">A 9:30 AM minden nap:`0 30 9 * * *`</span><span class="sxs-lookup"><span data-stu-id="fe81c-152">At 9:30 AM every day: `0 30 9 * * *`</span></span>
* <span data-ttu-id="fe81c-153">A 9:30 AM hét naponta:`0 30 9 * * 1-5`</span><span class="sxs-lookup"><span data-stu-id="fe81c-153">At 9:30 AM every week day: `0 30 9 * * 1-5`</span></span>

<span data-ttu-id="fe81c-154">**Megjegyzés:**: a Visual Studio eszközből webjobs-feladat telepítésekor ügyeljen arra, hogy jelölje meg a `settings.job` fájl tulajdonságai "Ha újabb másolatát".</span><span class="sxs-lookup"><span data-stu-id="fe81c-154">**Note**: when deploying a WebJob from Visual Studio, make sure to mark your `settings.job` file properties as 'Copy if newer'.</span></span>

## <span data-ttu-id="fe81c-155"><a name="CreateScheduled"></a>Az Azure-ütemező használatával ütemezett webjobs-feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="fe81c-155"><a name="CreateScheduled"></a>Create a scheduled WebJob using the Azure Scheduler</span></span>
<span data-ttu-id="fe81c-156">A következő alternatív módszer lehetővé teszi, az Azure Scheduler használatát.</span><span class="sxs-lookup"><span data-stu-id="fe81c-156">The following alternate technique makes use of the Azure Scheduler.</span></span> <span data-ttu-id="fe81c-157">Ebben az esetben a webjobs-feladat nem rendelkezik az ütemezés közvetlen ismerete.</span><span class="sxs-lookup"><span data-stu-id="fe81c-157">In this case, your WebJob does not have any direct knowledge of the schedule.</span></span> <span data-ttu-id="fe81c-158">Ehelyett az Azure Scheduler lekérdezi úgy, hogy aktiválja a webjobs-feladat ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="fe81c-158">Instead, the Azure Scheduler gets configured to trigger your WebJob on a schedule.</span></span> 

<span data-ttu-id="fe81c-159">Az Azure portálon még nem kapott képes létrehozni az ütemezett webjobs-feladat, de amíg nincs hozzáadva a szolgáltatást is elvégezheti a a [klasszikus portál](http://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="fe81c-159">The Azure Portal doesn't yet have the ability to create a scheduled WebJob, but until that feature is added you can do it by using the [classic portal](http://manage.windowsazure.com).</span></span>

1. <span data-ttu-id="fe81c-160">Az a [klasszikus portál](http://manage.windowsazure.com) nyissa meg a webjobs-feladat a lapon, majd kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="fe81c-160">In the [classic portal](http://manage.windowsazure.com) go to the WebJob page and click **Add**.</span></span>
2. <span data-ttu-id="fe81c-161">Az a **Futtatás hogyan** válassza **ütemezhető**.</span><span class="sxs-lookup"><span data-stu-id="fe81c-161">In the **How to Run** box, choose **Run on a schedule**.</span></span>
   
    ![Új ütemezett feladatot][NewScheduledJob]
3. <span data-ttu-id="fe81c-163">Válassza ki a **Feladatütemező régió** a feladat, majd kattintson a jobb alsó sarkában folytassa a következő képernyőn a párbeszédpanelen látható nyílra.</span><span class="sxs-lookup"><span data-stu-id="fe81c-163">Choose the **Scheduler Region** for your job, and then click the arrow on the bottom right of the dialog to proceed to the next screen.</span></span>
4. <span data-ttu-id="fe81c-164">Az a **feladat létrehozása** párbeszédpanelen válassza ki a **ismétlődési** kívánt: **egyszeri feladat** vagy **ismétlődő feladat**.</span><span class="sxs-lookup"><span data-stu-id="fe81c-164">In the **Create Job** dialog, choose the type of **Recurrence** you want: **One-time job** or **Recurring job**.</span></span>
   
    ![Ismétlődés ütemezése][SchdRecurrence]
5. <span data-ttu-id="fe81c-166">Kiválaszthat egy **indítása** idő: **most** vagy **adott időpontban**.</span><span class="sxs-lookup"><span data-stu-id="fe81c-166">Also choose a **Starting** time: **Now** or **At a specific time**.</span></span>
   
    ![Kezdő időpontjának ütemezése][SchdStart]
6. <span data-ttu-id="fe81c-168">Ha el szeretné indítani a megadott időpontban, válassza ki a kezdési idő értékei alapján **indítása a**.</span><span class="sxs-lookup"><span data-stu-id="fe81c-168">If you want to start at a specific time, choose your starting time values under **Starting On**.</span></span>
   
    ![Egy adott időpontban ütemezés kezdési][SchdStartOn]
7. <span data-ttu-id="fe81c-170">Ha úgy dönt, hogy ismétlődő feladat, hogy a **Ismétlődés minden** beállítással adhatja meg a előfordulási gyakoriságát és a **befejezési a** beállítással adhatja meg a befejezési időpontot.</span><span class="sxs-lookup"><span data-stu-id="fe81c-170">If you chose a recurring job, you have the **Recur Every** option to specify the frequency of occurrence and the **Ending On** option to specify an ending time.</span></span>
   
    ![Ismétlődés ütemezése][SchdRecurEvery]
8. <span data-ttu-id="fe81c-172">Ha úgy dönt, **hét**, kiválaszthatja a **az egy adott ütemezés** és adja meg a napokat a hét, amelyet a feladat futtatásához.</span><span class="sxs-lookup"><span data-stu-id="fe81c-172">If you choose **Weeks**, you can select the **On a Particular Schedule** box and specify the days of the week that you want the job to run.</span></span>
   
    ![A hét napjai ütemezése][SchdWeeksOnParticular]
9. <span data-ttu-id="fe81c-174">Ha úgy dönt, **hónap** válassza ki a **az egy adott ütemezés** mezőben állíthatja be a feladat futtatásának számozott adott **nap** az adott hónapban.</span><span class="sxs-lookup"><span data-stu-id="fe81c-174">If you choose **Months** and select the **On a Particular Schedule** box, you can set the job to run on particular numbered **Days** in the month.</span></span> 
   
    ![A hónap adott dátumok ütemezése][SchdMonthsOnPartDays]
10. <span data-ttu-id="fe81c-176">Ha úgy dönt, **hét napjai**, mely a napot vagy a hét napjai kiválaszthatja az szeretné futtatni a feladatot az adott hónapban.</span><span class="sxs-lookup"><span data-stu-id="fe81c-176">If you choose **Week Days**, you can select which day or days of the week in the month you want the job to run on.</span></span>
    
     ![A hónap adott hét napjai ütemezése][SchdMonthsOnPartWeekDays]
11. <span data-ttu-id="fe81c-178">Végezetül is használhatja a **előfordulások** beállítást adja meg a hónap hányadik hetére (először, a második, harmadik stb.) a feladatot a megadott hét napjai futtatni szeretné.</span><span class="sxs-lookup"><span data-stu-id="fe81c-178">Finally, you can also use the **Occurrences** option to choose which week in the month (first, second, third etc.) you want the job to run on the week days you specified.</span></span>
    
    ![A hónap adott héten adott hét napjai ütemezése][SchdMonthsOnPartWeekDaysOccurences]
12. <span data-ttu-id="fe81c-180">Egy vagy több feladat létrehozását követően a nevük azok állapotát, az ütemezés típusa és egyéb információkat a webjobs-feladatok lapon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="fe81c-180">After you have created one or more jobs, their names will appear on the WebJobs tab with their status, schedule type, and other information.</span></span> <span data-ttu-id="fe81c-181">Az utolsó 30 webjobs-feladatok az előzményadatok megmarad.</span><span class="sxs-lookup"><span data-stu-id="fe81c-181">Historical information for the last 30  WebJobs is maintained.</span></span>
    
    ![Feladatok listája][WebJobsListWithSeveralJobs]

### <span data-ttu-id="fe81c-183"><a name="Scheduler"></a>Ütemezett feladatok és az Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="fe81c-183"><a name="Scheduler"></a>Scheduled jobs and Azure Scheduler</span></span>
<span data-ttu-id="fe81c-184">Ütemezett feladatok Azure Scheduler lapján további konfigurálható a [klasszikus portál](http://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="fe81c-184">Scheduled jobs can be further configured in the Azure Scheduler pages of the [classic portal](http://manage.windowsazure.com).</span></span>

1. <span data-ttu-id="fe81c-185">A webjobs-feladatok lapon kattintson a feladat **ütemezés** navigáljon az Azure Scheduler portállapon mutató hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="fe81c-185">On the WebJobs page, click the job's **schedule** link to navigate to the Azure Scheduler portal page.</span></span> 
   
   ![Azure Schedulerrel mutató hivatkozás][LinkToScheduler]
2. <span data-ttu-id="fe81c-187">A Feladatütemező lapon kattintson a feladatra.</span><span class="sxs-lookup"><span data-stu-id="fe81c-187">On the Scheduler page, click the job.</span></span>
   
    ![A Feladatütemező portál lapján feladat][SchedulerPortal]
3. <span data-ttu-id="fe81c-189">A **Feladatművelet** lap megnyitásakor, ahol további konfigurálhatja a feladatot.</span><span class="sxs-lookup"><span data-stu-id="fe81c-189">The **Job Action** page opens, where you can further configure the job.</span></span> 
   
    ![Feladatművelet PageInScheduler][JobActionPageInScheduler]

## <span data-ttu-id="fe81c-191"><a name="ViewJobHistory"></a>A feladat előzményeinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="fe81c-191"><a name="ViewJobHistory"></a>View the job history</span></span>
1. <span data-ttu-id="fe81c-192">A feladat, többek között a WebJobs SDK-val létrehozott feladatok végrehajtási előzményeinek megtekintéséhez kattintson a alatt a megfelelő hivatkozásra a **naplók** oszlop a webjobs-feladatok panelről.</span><span class="sxs-lookup"><span data-stu-id="fe81c-192">To view the execution history of a job, including jobs created with the WebJobs SDK, click  its corresponding link under the **Logs** column of the WebJobs blade.</span></span> <span data-ttu-id="fe81c-193">(Használhatja a vágólapra ikonra a napló fájl lap URL-CÍMÉT a vágólapra másolni, ha.)</span><span class="sxs-lookup"><span data-stu-id="fe81c-193">(You can use the clipboard icon to copy the URL of the log file page to the clipboard if you wish.)</span></span>
   
    ![Naplók hivatkozás](./media/web-sites-create-web-jobs/wjbladelogslink.png)
2. <span data-ttu-id="fe81c-195">A hivatkozásra kattintva megnyílik a részleteit megjelenítő oldalon a webjobs-feladat.</span><span class="sxs-lookup"><span data-stu-id="fe81c-195">Clicking the link opens the details page for the WebJob.</span></span> <span data-ttu-id="fe81c-196">Ezen a lapon látható a neve a parancs futtatása, a legutóbbi alkalommal lefutott, és a sikeres vagy sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="fe81c-196">This page shows you the name of the command run, the last times it ran, and its success or failure.</span></span> <span data-ttu-id="fe81c-197">A **legutóbbi feladat futtatása**, egy időpontot további részletek megtekintéséhez kattintson.</span><span class="sxs-lookup"><span data-stu-id="fe81c-197">Under **Recent job runs**, click a time to see further details.</span></span>
   
    ![WebJobDetails][WebJobDetails]
3. <span data-ttu-id="fe81c-199">A **webjobs-feladat futtatása részletek** lap jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="fe81c-199">The **WebJob Run Details** page appears.</span></span> <span data-ttu-id="fe81c-200">Kattintson a **váltása kimeneti** a naplók tartalmáról szöveg.</span><span class="sxs-lookup"><span data-stu-id="fe81c-200">Click **Toggle Output** to see the text of the log contents.</span></span> <span data-ttu-id="fe81c-201">A kimeneti naplót szöveg formátumban van.</span><span class="sxs-lookup"><span data-stu-id="fe81c-201">The output log is in text format.</span></span> 
   
    ![Webes feladat futtatása részletei][WebJobRunDetails]
4. <span data-ttu-id="fe81c-203">A kimeneti szöveg. egy külön böngészőablakban megtekintéséhez kattintson a **letöltése** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="fe81c-203">To see the output text in a separate browser window, click the **download** link.</span></span> <span data-ttu-id="fe81c-204">A szöveg betűméretét letöltéséhez kattintson a jobb gombbal a hivatkozásra, és mentse a fájl tartalmát a böngésző beállításait használja.</span><span class="sxs-lookup"><span data-stu-id="fe81c-204">To download the text itself, right-click the link and use your browser options to save the file contents.</span></span>
   
    ![Töltse le a kimenet][DownloadLogOutput]
5. <span data-ttu-id="fe81c-206">A **WebJobs** az oldal tetején a webjobs-feladatok listája az előzmények irányítópult eléréséhez kényelmes megoldást kínál.</span><span class="sxs-lookup"><span data-stu-id="fe81c-206">The **WebJobs** link at the top of the page provides a convenient way to get to a list of WebJobs on the history dashboard.</span></span>
   
    ![Webjobs-feladatok listája csatolása][WebJobsLinkToDashboardList]
   
    ![Webjobs-feladatok előzményeinek irányítópulton listája][WebJobsListInJobsDashboard]
   
    <span data-ttu-id="fe81c-209">Kattintson az alábbi hivatkozások megnyitná a webjobs-feladat részleteit megjelenítő oldalon a kijelölt feladat.</span><span class="sxs-lookup"><span data-stu-id="fe81c-209">Clicking one of these links takes you to the WebJob Details page for the job you selected.</span></span>

## <span data-ttu-id="fe81c-210"><a name="WHPNotes"></a>Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="fe81c-210"><a name="WHPNotes"></a>Notes</span></span>
* <span data-ttu-id="fe81c-211">Webalkalmazások szabad módban is túllépi az időkorlátot, miután 20 perc, ha nincsenek kérelmek nem az scm (telepítés) hely és a webalkalmazás-portál nem nyitható meg az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="fe81c-211">Web apps in Free mode can time out after 20 minutes if there are no requests to the scm (deployment) site and the web app's portal is not open in Azure.</span></span> <span data-ttu-id="fe81c-212">A tényleges helyet kérelmek nem állítja vissza a.</span><span class="sxs-lookup"><span data-stu-id="fe81c-212">Requests to the actual site will not reset this.</span></span>
* <span data-ttu-id="fe81c-213">Folyamatos feladat kód ki kellene írni a végtelen ciklus futtatásához.</span><span class="sxs-lookup"><span data-stu-id="fe81c-213">Code for a continuous job needs to be written to run in an endless loop.</span></span>
* <span data-ttu-id="fe81c-214">Folyamatos feladatok folyamatosan futnak, csak akkor, ha a webalkalmazás működik-e.</span><span class="sxs-lookup"><span data-stu-id="fe81c-214">Continuous jobs run continuously only when the web app is up.</span></span>
* <span data-ttu-id="fe81c-215">Egyszerű és szabványos mód ajánlat az Always On funkció, amellyel engedélyezésekor megakadályozza a webalkalmazások tétlenné.</span><span class="sxs-lookup"><span data-stu-id="fe81c-215">Basic and Standard modes offer the Always On feature which, when enabled, prevents web apps from becoming idle.</span></span>
* <span data-ttu-id="fe81c-216">Folyamatosan futó webjobs-feladatok csak hibakeresési.</span><span class="sxs-lookup"><span data-stu-id="fe81c-216">You can only debug continuously running WebJobs.</span></span> <span data-ttu-id="fe81c-217">Ütemezett vagy igény szerinti WebJobs-hibakeresés nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="fe81c-217">Debugging scheduled or on-demand WebJobs is not supported.</span></span>

## <span data-ttu-id="fe81c-218"><a name="NextSteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fe81c-218"><a name="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="fe81c-219">További információkért lásd: [Azure webjobs-feladatok ajánlott erőforrások][WebJobsRecommendedResources].</span><span class="sxs-lookup"><span data-stu-id="fe81c-219">For more information, see [Azure WebJobs Recommended Resources][WebJobsRecommendedResources].</span></span>

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

