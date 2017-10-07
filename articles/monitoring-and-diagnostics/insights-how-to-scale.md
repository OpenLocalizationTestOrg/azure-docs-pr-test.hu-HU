---
title: "aaaScale példány száma manuálisan vagy automatikusan skálázva Azure portálon |} Microsoft Docs"
description: "Megtudhatja, hogyan tooscale az Azure szolgáltatások."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 2397596a-071f-4d49-8893-bec5f735bd7b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: ancav
ms.openlocfilehash: 8cb78f18416bd3caecce52702a8d630aa434d776
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-instance-count-manually-or-automatically"></a>Méretezhető példányszám manuális vagy automatikus
A hello [Azure Portal](https://portal.azure.com/), manuálisan állíthatja be hello példányok száma a szolgáltatás, vagy igény szerint automatikus méretezése toohave paramétert adhat meg. Ez az általában hivatkozott tooas *horizontális felskálázás* vagy *méretezése*.

Előtt példányok száma alapján skálázás, vegye figyelembe, hogy skálázás érintett **tarifacsomag** hozzáadása tooinstance számot. Különböző tarifacsomagjainak lehet eltérő számú maggal és a memória, és így rendelkeznek jobb teljesítményt nyújt hello példányok azonos számú (amely *vertikális felskálázás* vagy *csökkentheti*). Ez a cikk foglalkozik kifejezetten *méretezése* és *kimenő*.

Hello portálon méretezheti, és használhatja hello [REST API](https://msdn.microsoft.com/library/azure/dn931953.aspx) vagy [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) tooadjust manuális vagy automatikus skálázása.

> [!NOTE]
> Ez a cikk ismerteti, hogyan toocreate az automatikus skálázási beállítás hello portálon, a [http://portal.azure.com](http://portal.azure.com). Ezen a portálon létrehozott automatikus skálázási beállítások értéke nem lehet szerkeszteni, hello klasszikus portál ([http://manage.windowsazure.com](http://manage.windowsazure.com)).
> 
> 

## <a name="scaling-manually"></a>Manuális skálázás
1. A hello [Azure Portal](https://portal.azure.com/), kattintson **Tallózás**, majd keresse meg például a kívánt tooscale, toohello erőforrás egy **App Service-csomag**.
2. Kattintson a **beállítások > kibővítési (App Service-csomag).**
3. Hello hello tetején **méretezési** panelen láthatja, hogy az automatikus skálázási műveletek hello szolgáltatás előzményeit.
   
    ![Skála panel](./media/insights-how-to-scale/Insights_ScaleBladeDayZero.png)
   
   > [!NOTE]
   > Csak az automatikus skálázás által végrehajtott műveletek jelennek meg ezen a diagramon. Ha manuálisan módosítja hello példányok száma, hello módosítása nem jelenik meg ezen a diagramon.
   > 
   > 
4. Manuálisan módosíthatja hello szám **példányok** a csúszkát.
5. Hello kattintson **mentése** parancsot, és akkor lesz méretezhető szinte azonnal toothat-példányok száma.

## <a name="scaling-based-on-a-pre-set-metric"></a>Egy előre meghatározott metrika skálázás alapján
Ha azt szeretné, hogy hello több példányban tooautomatically állítsa be a metrika alapján, válassza ki hello hello kívánt metrika **szerint** legördülő menüből. Például egy **App Service-csomag** szerint méretezheti **processzor**.

1. Amikor kiválaszt egy metrika egy csúszkát kap, és/vagy, közötti tooscale kívánt szöveg mezőkbe tooenter hello példányok száma:
   
    ![Processzor méretezési panelről](./media/insights-how-to-scale/Insights_ScaleBladeCPU.png)
   
    Automatikus skálázási soha nem kerül a szolgáltatás alatt vagy felett hello határok beállított, függetlenül a terhelését.
2. Ezután válassza az hello cél időtartam hello mérőszám. Például, ha úgy döntött, hogy **processzor**, adhatja meg a cél hello átlagos CPU összes hello példányok a szolgáltatásban. A bővített kapacitású történik, ha a hello átlagos CPU meghaladja hello adhat meg, ehhez hasonlóan a skála történjen, amikor hello átlagos CPU minimális hello alá süllyed.
3. Kattintson a hello **mentése** parancsot. Ellenőrzi, hogy az automatikus skálázás minden néhány perc toomake arra, hogy a hello példány tartomány és a cél a mérőszám. A szolgáltatás további forgalom érkezik, amikor elérhetővé válik több példány bármi nélkül.

## <a name="scale-based-on-other-metrics"></a>A skála más metrikákkal alapján
Eltérő hello készletek hello megjelenő alapján metrikák méretezheti **bővítse a** legördülő menüből, és képes még kibővítési álló összetett készlettel rendelkezik, a méretezés szabályokban.

### <a name="adding-or-changing-a-rule"></a>Hozzáadása vagy módosítása egy szabály
1. Válassza ki a hello **ütemezés és a teljesítmény szabályok** a hello **szerint** legördülő: ![teljesítményszabályok](./media/insights-how-to-scale/Insights_PerformanceRules.png)
2. Ha korábban már volt az automatikus skálázás, a megjelennek volt hello pontos szabályok nézetét.
3. egy másik metrika kattintson hello alapján tooscale **szabály hozzáadása** sor. Is kattint a létező sorok toochange hello hello metrika az korábban is rendelkezett toohello metrika azt szeretné, hogy tooscale által.
   ![Szabály hozzáadása](./media/insights-how-to-scale/Insights_AddRule.png)
4. Most mely metrika kívánt által tooscale tooselect van szüksége. Ha egy metrika van választva néhány dolgot tooconsider:
   
   * Hello *erőforrás* hello metrika származik. Általában ez fogja kell hello ugyanaz, mint a hello erőforrás van folyamatban. Azonban ha tooscale hello mélysége tároló várólista által, hello erőforrás hello várólista tooscale által használni kívánt.
   * Hello *metrika neve* magát.
   * Hello *összesítési idő* hello metrika. Ez a hogyan hello adatok egyesítése keresztül hello *időtartam*.
5. A metrika kiválasztása után dönthet hello metrika és hello operátor hello küszöbértékét. Tegyük fel például, **nagyobb, mint** **80 %**.
6. Ezután válassza ki a hello műveletet, amelyet az tootake. Van néhány más típusú műveleteket:
   
   * Növelheti vagy csökkentheti – ezzel hozzáadása vagy eltávolítása hello **érték** adhat meg a példányok száma
   * Növeléséhez vagy csökkentéséhez százalék - egy százalékkal hello példányszám Ezzel módosítja. Például sikerült helyezett 25 hello **érték** mező, és ha jelenleg 8 példányok, akkor adható hozzá a 2.
   * Növeléséhez vagy csökkentéséhez túl – hello példányok száma toohello állítja **érték** adhat meg.
7. Végül választható ritkán le – ez a szabály hello előző skálázási művelet tooscale újra után várjon mennyi ideig.
8. A szabály konfigurálása után kattintson a **OK**.
9. Miután konfigurálta az összes kívánt hello szabály, lehet, hogy toohit hello **mentése** parancsot.

### <a name="scaling-with-multiple-steps"></a>Méretezést, több lépés
a fenti példákban hello olyan közérthető alapszintű. Azonban ha toobe szigorúbb kapcsolatos felfelé skálázással (vagy le), még akkor is hozzáadhat több skálázási szabályainak hello azonos metrikát. Például a processzor két skálázási szabályok adhatók meg:

1. A horizontális felskálázáshoz 1 példány 60 % feletti processzor esetén
2. A horizontális felskálázáshoz 3 példányok 85 % feletti processzor esetén

![Több skálázási szabályok](./media/insights-how-to-scale/Insights_MultipleScaleRules.png)

A további szabálynak Ha a terhelés meghaladja a 85 % tartozó skálázási műveletek előtt elérhetővé válik egy helyett két további példányt.

## <a name="scale-based-on-a-schedule"></a>A skála a ütemezés szerint
Alapértelmezés szerint a skálázási szabály létrehozásakor mindig alkalmazzák. Láthatja, hogy amikor rákattint az hello-profil fejléc:

![Profil](./media/insights-how-to-scale/Insights_Profile.png)

Azonban érdemes lehet toohave szigorúbb méretezésének során hello nap vagy hét hello, mint hello hétvégi. A szolgáltatás teljesen ki munkaidőn is leállíthatja.

1. toodo, hello-profil van, jelölje ki **ismétlődési** helyett **mindig,** hello alkalommal fordult elő az, hogy szeretné-e hello-profil tooapply válassza.
2. Például egy profilt, amely során hello hét hello toohave **nap** legördülő menüből, törölje a jelet **szombat** és **vasárnap**.
3. a profilt, amely során hello nappali, beállíthatja hello toohave **kezdési időpont** toohello időpontot toostart a kívánt.
   
    ![Alapértelmezett ismétlődési](./media/insights-how-to-scale/Insights_ProfileRecurrence.png)
4. Kattintson az **OK** gombra.
5. Ezt követően kell megjeleníteni kívánt tooapply máskor tooadd hello-profil. Kattintson a hello **profil hozzáadása** sor.
    ![Munkába](./media/insights-how-to-scale/Insights_ProfileOffWork.png)
6. Az új, második, a profil neve, például hívhatja azt **munkahelyi kikapcsolva**.
7. Válassza ki **ismétlődési** újra, és válassza ki a kívánt ebben az időszakban hello példányok száma tartomány.
8. Mivel hello alapértelmezett profilként, majd kattintson hello **nap** szeretné a profil tooapply a hello **kezdési időpont** hello nap alatt.
   
   > [!NOTE]
   > Automatikus skálázás használandó hello nyári időszámításra vonatkozó szabályai attól **időzóna** választja. Azonban hello UTC eltolás hello alap időzóna-eltolódás, nem hello nyári megtakarítások UTC eltolás megjeleníti a nyári időszámítás során.
   > 
   > 
9. Kattintson az **OK** gombra.
10. Most, szüksége lesz tooadd függetlenül szabályokat, hogy tooapply szeretné, hogy a második profil során. Kattintson a **szabály hozzáadása**, és majd ugyanaz a szabály során hello alapértelmezett profil van hello lehetett összeállítani.
    
    ![A szabály toooff munkahelyi hozzáadása](./media/insights-how-to-scale/Insights_RuleOffWork.png)
11. Kibővítési és a skála mindkét szabály meg arról, hogy toocreate, vagy során hello profil hello példányszám lesz csak méretének növelése (vagy csökkentése).
12. Végezetül kattintson **mentése**.

## <a name="next-steps"></a>Következő lépések
* [Szolgáltatás metrikát](insights-how-to-customize-monitoring.md) toomake meg arról, hogy a szolgáltatás megfelelően üzemel és rugalmas.
* [Figyelés engedélyezésekor és diagnosztikai](insights-how-to-use-diagnostics.md) toocollect részletes nagyon gyakori metrikákat a szolgáltatásban.
* [Riasztási értesítéseket kaphat](insights-receive-alert-notifications.md), ha működési események történnek vagy a mérőszámok átlépnek egy küszöbértéket.
* [Az alkalmazások teljesítményének figyelésére](../application-insights/app-insights-azure-web-apps.md) Ha azt szeretné, toounderstand pontosan hogyan a kód hajt végre hello felhőben.
* [Események és tevékenységnapló](insights-debugging-with-events.md) toolearn minden, ami következett be a szolgáltatásban.
* [Figyelése rendelkezésre állásának és bármilyen weblap válaszképességének](../application-insights/app-insights-monitor-web-app-availability.md) az Application insights szolgáltatással, így azt is megtudhatja, ha az oldala nem működik.

