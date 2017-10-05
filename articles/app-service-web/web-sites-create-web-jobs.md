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
# <a name="run-background-tasks-with-webjobs"></a>Háttérfeladatok futtatása WebJobs-feladatokkal
## <a name="overview"></a>Áttekintés
Programok vagy parancsfájlok futtathatók a webjobs-feladatok a [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webalkalmazás háromféleképpen: az igény szerinti folyamatosan, vagy ütemezés szerint. Nincs webjobs-feladatok használandó további költség nélkül.

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

Ez a cikk bemutatja, hogyan telepíthetők a webjobs-feladatok segítségével a [Azure Portal](https://portal.azure.com). A Visual Studio vagy egy folyamatos kézbesítési folyamatának telepítésével kapcsolatos információkért lásd: [Azure WebJobs központi telepítése a Web Apps hogyan](websites-dotnet-deploy-webjobs.md).

Az Azure WebJobs SDK egyszerűbbé teszi a sok WebJobs-programozási feladathoz. További információkért lásd: [Mi az a WebJobs SDK](websites-dotnet-webjobs-sdk.md).

 Az Azure Functions segítségével más programok és parancsfájlok futtathatók, vagy egy kiszolgáló nélküli környezetben vagy egy App Service-alkalmazást. További információkért lásd: [Azure Functions áttekintése](../azure-functions/functions-overview.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="acceptablefiles"></a>Elfogadható fájltípusait ismerteti programok és parancsfájlok
A következő típusú elfogadottak:

* .cmd, .bat, .exe (a windows parancssori eszközzel)
* .ps1 (a powershell használatával)
* .SH (a bash használatával)
* .php (a php használatával)
* .PY (python használatával)
* .js (a csomópont használatával)
* a .JAR fájlt (a java)

## <a name="CreateOnDemand"></a>Az on igény szerinti webjobs-feladat létrehozása a portálon
1. A a **Web App** panelen található a [Azure Portal](https://portal.azure.com), kattintson a **összes beállítás > WebJobs** megjelenítése a **WebJobs** panelen.
   
    ![Webjobs-feladat panelen](./media/web-sites-create-web-jobs/wjblade.png)
2. Kattintson az **Add** (Hozzáadás) parancsra. A **hozzáadása webjobs-feladat** párbeszédpanel jelenik meg.
   
    ![Adja hozzá a webjobs-feladat panelen](./media/web-sites-create-web-jobs/addwjblade.png)
3. A **neve**, adja meg a webjobs-feladat nevét. A név betűvel vagy számmal kell kezdődnie, és nem tartalmazhat különleges karaktereket eltérő "-" és "_".
4. Az a **Futtatás hogyan** válassza **igény szerint futtatható**.
5. Az a **fájlfeltöltés** mezőben, a mappa ikonra, és keresse meg a parancsfájlt tartalmazó zip fájlt. A zip-fájl a végrehajtható fájl (.exe .cmd .bat .sh .php .py .js) kell tartalmaznia, valamint minden kiegészítő fájlt, a program vagy parancsfájl futtatásához szükséges.
6. Ellenőrizze **létrehozása** töltse fel a parancsfájlt a webes alkalmazás. 
   
    A név megadott a listában megjelenik a webjobs-feladat a **WebJobs** panelen.
7. A webjobs-feladat futtatásához kattintson a jobb gombbal a nevét a listában, és kattintson a **futtatása**.
   
    ![Webjobs-feladat futtatása](./media/web-sites-create-web-jobs/runondemand.png)

## <a name="CreateContinuous"></a>Egy folyamatosan futó webjobs-feladat létrehozása
1. Folyamatosan futó webjobs-feladat létrehozásához kövesse a lépéseket a webjobs-feladat létrehozása, amely csak egyszer fut, de a a **Futtatás hogyan** válassza **folyamatos**.
2. Indítsa el, vagy egy folyamatos webjobs-feladat leállítása, kattintson a jobb gombbal a webjobs-feladat, a listában, és kattintson a **Start** vagy **leállítása**.

> [!NOTE]
> Ha egynél több példány a webalkalmazás fut, folyamatosan futó webjobs-feladat fut az összes olyan saját példányait. Igény szerinti és az ütemezett webjobs-feladatok futtatása a Microsoft Azure terheléselosztást kijelölt egyetlen példányán.
> 
> A folyamatos webjobs-feladatok futtatásához, megbízható és összes példánya, engedélyezze az Always On * konfigurációs beállítást a webalkalmazás nem lehet leállítani a módban való futáskor az SCM állomás hely le lett üresjárati idő túl hosszú.
> 
> 

## <a name="CreateScheduledCRON"></a>Hozzon létre egy ütemezett webjobs-feladat CRON-kifejezés használata
Ez a módszer a Basic, Standard vagy prémium módban futó webalkalmazások érhető el, és van szükség a **mindig a** beállítást engedélyezni kell az alkalmazást.

Kapcsolja be az az igény szerinti webjobs-feladat az ütemezett webjobs-feladat, egyszerűen közé tartozik egy `settings.job` fájl a webjobs-feladat zip-fájl gyökerében. A JSON-fájl tartalmaznia kell egy `schedule` tulajdonság egy [CRON-kifejezés](https://en.wikipedia.org/wiki/Cron), az alábbi példában egy.

A CRON-kifejezés áll 6 mezők: `{second} {minute} {hour} {day} {month} {day of the week}`.

Például elindítható a webjobs-feladat 15 percenként a `settings.job` kellene lennie:

```json
{
    "schedule": "0 */15 * * * *"
}
``` 

Más CRON-ütemezését példák:

* Minden órában (azaz mindig, amikor a perc száma 0):`0 0 * * * *` 
* Minden órában a Reggel 9 a délután 5 óra:`0 0 9-17 * * *` 
* A 9:30 AM minden nap:`0 30 9 * * *`
* A 9:30 AM hét naponta:`0 30 9 * * 1-5`

**Megjegyzés:**: a Visual Studio eszközből webjobs-feladat telepítésekor ügyeljen arra, hogy jelölje meg a `settings.job` fájl tulajdonságai "Ha újabb másolatát".

## <a name="CreateScheduled"></a>Az Azure-ütemező használatával ütemezett webjobs-feladat létrehozása
A következő alternatív módszer lehetővé teszi, az Azure Scheduler használatát. Ebben az esetben a webjobs-feladat nem rendelkezik az ütemezés közvetlen ismerete. Ehelyett az Azure Scheduler lekérdezi úgy, hogy aktiválja a webjobs-feladat ütemezés szerint. 

Az Azure portálon még nem kapott képes létrehozni az ütemezett webjobs-feladat, de amíg nincs hozzáadva a szolgáltatást is elvégezheti a a [klasszikus portál](http://manage.windowsazure.com).

1. Az a [klasszikus portál](http://manage.windowsazure.com) nyissa meg a webjobs-feladat a lapon, majd kattintson a **Hozzáadás**.
2. Az a **Futtatás hogyan** válassza **ütemezhető**.
   
    ![Új ütemezett feladatot][NewScheduledJob]
3. Válassza ki a **Feladatütemező régió** a feladat, majd kattintson a jobb alsó sarkában folytassa a következő képernyőn a párbeszédpanelen látható nyílra.
4. Az a **feladat létrehozása** párbeszédpanelen válassza ki a **ismétlődési** kívánt: **egyszeri feladat** vagy **ismétlődő feladat**.
   
    ![Ismétlődés ütemezése][SchdRecurrence]
5. Kiválaszthat egy **indítása** idő: **most** vagy **adott időpontban**.
   
    ![Kezdő időpontjának ütemezése][SchdStart]
6. Ha el szeretné indítani a megadott időpontban, válassza ki a kezdési idő értékei alapján **indítása a**.
   
    ![Egy adott időpontban ütemezés kezdési][SchdStartOn]
7. Ha úgy dönt, hogy ismétlődő feladat, hogy a **Ismétlődés minden** beállítással adhatja meg a előfordulási gyakoriságát és a **befejezési a** beállítással adhatja meg a befejezési időpontot.
   
    ![Ismétlődés ütemezése][SchdRecurEvery]
8. Ha úgy dönt, **hét**, kiválaszthatja a **az egy adott ütemezés** és adja meg a napokat a hét, amelyet a feladat futtatásához.
   
    ![A hét napjai ütemezése][SchdWeeksOnParticular]
9. Ha úgy dönt, **hónap** válassza ki a **az egy adott ütemezés** mezőben állíthatja be a feladat futtatásának számozott adott **nap** az adott hónapban. 
   
    ![A hónap adott dátumok ütemezése][SchdMonthsOnPartDays]
10. Ha úgy dönt, **hét napjai**, mely a napot vagy a hét napjai kiválaszthatja az szeretné futtatni a feladatot az adott hónapban.
    
     ![A hónap adott hét napjai ütemezése][SchdMonthsOnPartWeekDays]
11. Végezetül is használhatja a **előfordulások** beállítást adja meg a hónap hányadik hetére (először, a második, harmadik stb.) a feladatot a megadott hét napjai futtatni szeretné.
    
    ![A hónap adott héten adott hét napjai ütemezése][SchdMonthsOnPartWeekDaysOccurences]
12. Egy vagy több feladat létrehozását követően a nevük azok állapotát, az ütemezés típusa és egyéb információkat a webjobs-feladatok lapon jelenik meg. Az utolsó 30 webjobs-feladatok az előzményadatok megmarad.
    
    ![Feladatok listája][WebJobsListWithSeveralJobs]

### <a name="Scheduler"></a>Ütemezett feladatok és az Azure Scheduler
Ütemezett feladatok Azure Scheduler lapján további konfigurálható a [klasszikus portál](http://manage.windowsazure.com).

1. A webjobs-feladatok lapon kattintson a feladat **ütemezés** navigáljon az Azure Scheduler portállapon mutató hivatkozást. 
   
   ![Azure Schedulerrel mutató hivatkozás][LinkToScheduler]
2. A Feladatütemező lapon kattintson a feladatra.
   
    ![A Feladatütemező portál lapján feladat][SchedulerPortal]
3. A **Feladatművelet** lap megnyitásakor, ahol további konfigurálhatja a feladatot. 
   
    ![Feladatművelet PageInScheduler][JobActionPageInScheduler]

## <a name="ViewJobHistory"></a>A feladat előzményeinek megtekintése
1. A feladat, többek között a WebJobs SDK-val létrehozott feladatok végrehajtási előzményeinek megtekintéséhez kattintson a alatt a megfelelő hivatkozásra a **naplók** oszlop a webjobs-feladatok panelről. (Használhatja a vágólapra ikonra a napló fájl lap URL-CÍMÉT a vágólapra másolni, ha.)
   
    ![Naplók hivatkozás](./media/web-sites-create-web-jobs/wjbladelogslink.png)
2. A hivatkozásra kattintva megnyílik a részleteit megjelenítő oldalon a webjobs-feladat. Ezen a lapon látható a neve a parancs futtatása, a legutóbbi alkalommal lefutott, és a sikeres vagy sikertelen volt. A **legutóbbi feladat futtatása**, egy időpontot további részletek megtekintéséhez kattintson.
   
    ![WebJobDetails][WebJobDetails]
3. A **webjobs-feladat futtatása részletek** lap jelenik meg. Kattintson a **váltása kimeneti** a naplók tartalmáról szöveg. A kimeneti naplót szöveg formátumban van. 
   
    ![Webes feladat futtatása részletei][WebJobRunDetails]
4. A kimeneti szöveg. egy külön böngészőablakban megtekintéséhez kattintson a **letöltése** hivatkozásra. A szöveg betűméretét letöltéséhez kattintson a jobb gombbal a hivatkozásra, és mentse a fájl tartalmát a böngésző beállításait használja.
   
    ![Töltse le a kimenet][DownloadLogOutput]
5. A **WebJobs** az oldal tetején a webjobs-feladatok listája az előzmények irányítópult eléréséhez kényelmes megoldást kínál.
   
    ![Webjobs-feladatok listája csatolása][WebJobsLinkToDashboardList]
   
    ![Webjobs-feladatok előzményeinek irányítópulton listája][WebJobsListInJobsDashboard]
   
    Kattintson az alábbi hivatkozások megnyitná a webjobs-feladat részleteit megjelenítő oldalon a kijelölt feladat.

## <a name="WHPNotes"></a>Megjegyzések
* Webalkalmazások szabad módban is túllépi az időkorlátot, miután 20 perc, ha nincsenek kérelmek nem az scm (telepítés) hely és a webalkalmazás-portál nem nyitható meg az Azure-ban. A tényleges helyet kérelmek nem állítja vissza a.
* Folyamatos feladat kód ki kellene írni a végtelen ciklus futtatásához.
* Folyamatos feladatok folyamatosan futnak, csak akkor, ha a webalkalmazás működik-e.
* Egyszerű és szabványos mód ajánlat az Always On funkció, amellyel engedélyezésekor megakadályozza a webalkalmazások tétlenné.
* Folyamatosan futó webjobs-feladatok csak hibakeresési. Ütemezett vagy igény szerinti WebJobs-hibakeresés nem támogatott.

## <a name="NextSteps"></a>Következő lépések
További információkért lásd: [Azure webjobs-feladatok ajánlott erőforrások][WebJobsRecommendedResources].

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

