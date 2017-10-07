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
# <a name="run-background-tasks-with-webjobs"></a>Háttérfeladatok futtatása WebJobs-feladatokkal
## <a name="overview"></a>Áttekintés
Programok vagy parancsfájlok futtathatók a webjobs-feladatok a [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webalkalmazás háromféleképpen: az igény szerinti folyamatosan, vagy ütemezés szerint. Nincs nem jelent többletköltséget toouse webjobs-feladatok.

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

Ez a cikk bemutatja, hogyan toodeploy webjobs-feladatok használatával hello [Azure Portal](https://portal.azure.com). További információ a Visual Studio vagy egy folyamatos kézbesítési folyamatának toodeploy lásd [hogyan tooDeploy Azure webjobs-feladatok tooWeb alkalmazások](websites-dotnet-deploy-webjobs.md).

hello Azure WebJobs SDK számos programozási WebJobs-feladatok egyszerűbbé teszi. További információkért lásd: [hello WebJobs SDK Újdonságok](websites-dotnet-webjobs-sdk.md).

 Az Azure Functions biztosít egy másik módja toorun programokat és parancsfájlokat vagy a kiszolgáló nélküli környezetekben, illetve egy App Service-alkalmazást. További információkért lásd: [Azure Functions áttekintése](../azure-functions/functions-overview.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="acceptablefiles"></a>Elfogadható fájltípusait ismerteti programok és parancsfájlok
a következő fájltípusokat hello elfogadottak:

* .cmd, .bat, .exe (a windows parancssori eszközzel)
* .ps1 (a powershell használatával)
* .SH (a bash használatával)
* .php (a php használatával)
* .PY (python használatával)
* .js (a csomópont használatával)
* a .JAR fájlt (a java)

## <a name="CreateOnDemand"></a>Hozzon létre egy on igény szerinti webjobs-feladat hello portálon
1. A hello **Web App** panelen található hello [Azure Portal](https://portal.azure.com), kattintson a **összes beállítás > webjobs-feladatok** tooshow hello **webjobs-feladatok** panelen.
   
    ![Webjobs-feladat panelen](./media/web-sites-create-web-jobs/wjblade.png)
2. Kattintson az **Add** (Hozzáadás) parancsra. Hello **hozzáadása webjobs-feladat** párbeszédpanel jelenik meg.
   
    ![Adja hozzá a webjobs-feladat panelen](./media/web-sites-create-web-jobs/addwjblade.png)
3. A **neve**, adjon meg egy nevet hello webjobs-feladat. hello nevének betűvel vagy számmal kell kezdődnie, és nem tartalmazhat különleges karaktereket eltérő "-" és "_".
4. A hello **hogyan tooRun** válassza **igény szerint futtatható**.
5. A hello **fájlfeltöltés** mezőben, hello mappa ikonra, és keresse meg a parancsfájlt tartalmazó toohello zip-fájl. hello zip-fájl tartalmaznia kell a végrehajtható fájl (.exe .cmd .bat .sh .php .py .js) és minden kiegészítő fájlt szükséges toorun hello program vagy parancsfájl.
6. Ellenőrizze **létrehozása** tooupload hello parancsfájl tooyour webalkalmazás. 
   
    hello hello webjobs-feladat a megadott néven jelennek meg hello listáján a hello **WebJobs** panelen.
7. toorun hello webjobs-feladat, kattintson a jobb gombbal a nevére hello listán, és kattintson a **futtatása**.
   
    ![Webjobs-feladat futtatása](./media/web-sites-create-web-jobs/runondemand.png)

## <a name="CreateContinuous"></a>Egy folyamatosan futó webjobs-feladat létrehozása
1. toocreate folyamatosan futó webjobs-feladat, hajtsa végre az azonos lépések a webjobs-feladat létrehozása, amely csak egyszer fut, de a hello hello **hogyan tooRun** válassza **folyamatos**.
2. toostart vagy állítsa le a folyamatos webjobs-feladat, kattintson a jobb gombbal hello listán, és kattintson a webjobs-feladat hello **Start** vagy **leállítása**.

> [!NOTE]
> Ha egynél több példány a webalkalmazás fut, folyamatosan futó webjobs-feladat fut az összes olyan saját példányait. Igény szerinti és az ütemezett webjobs-feladatok futtatása a Microsoft Azure terheléselosztást kijelölt egyetlen példányán.
> 
> A folyamatos webjobs-feladatok toorun megbízhatóan és összes példánya lehetővé teszik a mindig bekapcsolva hello * hello webalkalmazás egyéb azok leállíthatja, ha túl sokáig üresjáratban hello SCM állomás hely konfigurációs beállítást.
> 
> 

## <a name="CreateScheduledCRON"></a>Hozzon létre egy ütemezett webjobs-feladat CRON-kifejezés használata
Ezzel a technikával Basic, Standard vagy prémium módban futó elérhető tooWeb alkalmazásokat, és hello igényel **mindig a** toobe hello alkalmazás engedélyezve beállítás.

tooturn egy az igény szerinti webjobs-feladat az ütemezett webjobs-feladat, egyszerűen közé tartozik egy `settings.job` fájlban a következő hello a webjobs-feladat zip-fájl gyökerében. A JSON-fájl tartalmaznia kell egy `schedule` tulajdonság egy [CRON-kifejezés](https://en.wikipedia.org/wiki/Cron), az alábbi példában egy.

hello CRON-kifejezés áll 6 mezők: `{second} {minute} {hour} {day} {month} {day of hello week}`.

Például tootrigger a webjobs-feladat 15 percenként a `settings.job` kellene lennie:

```json
{
    "schedule": "0 */15 * * * *"
}
``` 

Más CRON-ütemezését példák:

* Minden órában (azaz mindig, amikor perc hello száma 0):`0 0 * * * *` 
* A Reggel 9 too5 óránként PM:`0 0 9-17 * * *` 
* A 9:30 AM minden nap:`0 30 9 * * *`
* A 9:30 AM hét naponta:`0 30 9 * * 1-5`

**Megjegyzés:**: a Visual Studio eszközből webjobs-feladat való telepítésekor, győződjön meg arról, hogy toomark a `settings.job` fájl tulajdonságai "Ha újabb másolatát".

## <a name="CreateScheduled"></a>Hello Azure Scheduler használatával ütemezett webjobs-feladat létrehozása
a következő alternatív módszer hello hello Azure Scheduler használ. Ebben az esetben a webjobs-feladat nincs hello ütemezés közvetlen ismerete. Ehelyett hello Azure Scheduler lekérdezi konfigurált tootrigger a webjobs-feladat ütemezés szerint. 

hello Azure portálon még nem kapott hello képességét toocreate ütemezett webjobs-feladat, de amíg nincs hozzáadva a szolgáltatást is elvégezheti a hello [klasszikus portál](http://manage.windowsazure.com).

1. A hello [klasszikus portál](http://manage.windowsazure.com) lépjen toohello webjobs-feladat lapra, és kattintson **Hozzáadás**.
2. A hello **hogyan tooRun** válassza **ütemezhető**.
   
    ![Új ütemezett feladatot][NewScheduledJob]
3. Válassza ki a hello **Feladatütemező régió** a feladat, majd hello hello jobb alsó sarkában hello párbeszédpanel tooproceed toohello következő képernyőn látható nyílra.
4. A hello **feladat létrehozása** párbeszédpanelen válassza ki, hello típusú **ismétlődési** kívánt: **egyszeri feladat** vagy **ismétlődő feladat**.
   
    ![Ismétlődés ütemezése][SchdRecurrence]
5. Kiválaszthat egy **indítása** idő: **most** vagy **adott időpontban**.
   
    ![Kezdő időpontjának ütemezése][SchdStart]
6. Ha toostart adott időpontban, válassza ki a kezdési idő értékei alapján **indítása a**.
   
    ![Egy adott időpontban ütemezés kezdési][SchdStartOn]
7. Ha úgy dönt, hogy ismétlődő feladat, hogy hello **Ismétlődés minden** toospecify hello gyakorisága előfordulási és hello lehetőséget **befejezési a** beállítás toospecify befejezési időpont.
   
    ![Ismétlődés ütemezése][SchdRecurEvery]
8. Ha úgy dönt, **hét**, kiválaszthatja a hello **az egy adott ütemezés** és adja meg a hello napot hello kívánt hello feladat toorun.
   
    ![Hello hét napjai ütemezése][SchdWeeksOnParticular]
9. Ha úgy dönt, **hónap** és select hello **az egy adott ütemezés** mezőben állíthatja be hello feladat toorun számozott adott **nap** hello hónapban. 
   
    ![Hello hónapban az adott dátumok ütemezése][SchdMonthsOnPartDays]
10. Ha úgy dönt, **hét napjai**, mely nap vagy hello hét napjai kiválaszthatja az azt szeretné, hogy a feladat toorun hello hello hónap.
    
     ![A hónap adott hét napjai ütemezése][SchdMonthsOnPartWeekDays]
11. Végezetül is használhatja hello **előfordulások** beállítás toochoose hello hónap hányadik hetére (először, a második, harmadik stb.) hello feladat toorun hello a hét napjai megadott szeretné.
    
    ![A hónap adott héten adott hét napjai ütemezése][SchdMonthsOnPartWeekDaysOccurences]
12. Egy vagy több feladat létrehozását követően a nevük azok állapotát, az ütemezés típusa és egyéb információkat hello webjobs-feladatok lapon jelenik meg. Korábbi hello utolsó 30 webjobs-feladatok a tárolt adatok.
    
    ![Feladatok listája][WebJobsListWithSeveralJobs]

### <a name="Scheduler"></a>Ütemezett feladatok és az Azure Scheduler
Ütemezett feladatok további konfigurálásra hello Azure Scheduler lapján hello [klasszikus portál](http://manage.windowsazure.com).

1. Hello webjobs-feladatok lapon kattintson a hello feladat **ütemezés** hivatkozás toonavigate toohello Azure Scheduler portálon. 
   
   ![Hivatkozás tooAzure Feladatütemező][LinkToScheduler]
2. Hello Feladatütemező lapján kattintson a hello feladat.
   
    ![Feladat hello Feladatütemező portál lapján][SchedulerPortal]
3. Hello **Feladatművelet** lap megnyitásakor, ahol további konfigurálható hello feladat. 
   
    ![Feladatművelet PageInScheduler][JobActionPageInScheduler]

## <a name="ViewJobHistory"></a>Hello-feladat előzményeinek megtekintése
1. tooview hello futtatási előzményei feladat, többek között a WebJobs SDK hello létrehozott feladatokat, kattintson a megfelelő hivatkozásra a hello **naplók** oszlop hello WebJobs panelről. (Használhatja hello vágólapra ikon toocopy hello URL-címet a hello napló fájl lap toohello vágólap Ha.)
   
    ![Naplók hivatkozás](./media/web-sites-create-web-jobs/wjbladelogslink.png)
2. Gombra kattintva hello a hivatkozás megnyitja hello részleteit megjelenítő oldalon hello webjobs-feladat. A lap tartalmazza, akkor hello neve hello parancs futtatása, a legutóbbi alkalommal lefutott, és annak sikerességét vagy sikertelenségét hello. A **legutóbbi feladat futtatása**, kattintson a idő toosee további részleteket.
   
    ![WebJobDetails][WebJobDetails]
3. Hello **webjobs-feladat futtatása részletek** lap jelenik meg. Kattintson a **váltása kimeneti** hello naplók tartalmáról toosee hello szövegét. hello kimeneti naplót szöveg formátumban van. 
   
    ![Webes feladat futtatása részletei][WebJobRunDetails]
4. toosee hello kimeneti szöveg egy külön böngészőablakban kattintson hello **letöltése** hivatkozásra. toodownload hello szöveg magát, kattintson a jobb gombbal a hello hivatkozásra, és használja a böngésző beállításai toosave hello fájl tartalmát.
   
    ![Töltse le a kimenet][DownloadLogOutput]
5. Hello **WebJobs** hivatkozás hello oldal hello tetején a webjobs-feladatok kényelmesen tooget tooa listáját tartalmazza hello előzmények irányítópulton.
   
    ![Hivatkozás tooWebJobs listája][WebJobsLinkToDashboardList]
   
    ![Webjobs-feladatok előzményeinek irányítópulton listája][WebJobsListInJobsDashboard]
   
    Kattintson az alábbi hivatkozások viszi toohello webjobs-feladat részletei lapon kiválasztott hello feladat.

## <a name="WHPNotes"></a>Megjegyzések
* Webalkalmazások szabad módban a túllépi az időkorlátot 20 perc után is, ha nincs kérelmek toohello scm (telepítés) helyen, és hello web app portal nincs megnyitva az Azure-ban. Kérelmek toohello tényleges hely ez nem alaphelyzetbe állnak.
* Folyamatos feladat kódot kell toobe toorun írt ki végtelen ciklus.
* Folyamatos feladatok folyamatosan futnak, csak akkor, ha hello a webalkalmazás működik-e.
* Alapszintű és a szabványos mód ajánlat hello mindig a beállítást, amely, ha engedélyezve van, megakadályozza a webalkalmazások tétlenné.
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

