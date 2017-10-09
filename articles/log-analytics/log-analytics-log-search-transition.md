---
title: "aaaAzure Log Analytics lekérdezési nyelv Adatlap |} Microsoft Docs"
description: "Ez a cikk segítséget nyújt tér át toohello új lekérdezési nyelv a Naplóelemzési, ha már ismeri a hello örökölt nyelv."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/23/2017
ms.author: bwren
ms.openlocfilehash: 8b4ee3d0b5e1ec8a9f95a09e0ad9835615ad1342
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="transitioning-tooazure-log-analytics-new-query-language"></a>Váltás tooAzure Naplóelemzési új lekérdezési nyelv

> [!NOTE]
> További információk az új Naplóelemzési lekérdezési nyelv, és az beszerzése hello hello eljárás tooupgrade frissítés: a munkaterület érheti el a [Azure Naplóelemzés munkaterület toonew napló keresési](log-analytics-log-search-upgrade.md).

Ez a cikk segítséget nyújt tér át toohello új lekérdezési nyelv a Naplóelemzési, ha már ismeri a hello örökölt nyelv.

## <a name="language-converter"></a>Nyelvi konverter

Ha ismeri a hello örökölt Log Analytics lekérdezési nyelv, hello toocreate hello ugyanabban a lekérdezésben hello új nyelven legkönnyebben toouse hello nyelvi konverter hello napló keresése portál telepítve van, ha a munkaterületet alakítja át.  Hello konverter használata éppolyan egyszerű, mintha egy korábbi lekérdezés hello felső szövegmezőben, írja be, majd kattintson **átalakítása**.  Kattintson a hello keresési gomb toorun hello lekérdezés vagy a másolás és illessze be toouse azt máshol.

![Nyelvi konverter](media/log-analytics-log-search-upgrade/language-converter.png)


## <a name="cheat-sheet"></a>Adatlap

hello következő táblázat általános lekérdezések számos összehasonlítása tooequivalent parancsok hello új és az örökölt lekérdező nyelve az Azure Naplóelemzés között.

| Leírás | Örökölt | új |
|:--|:--|:--|
| Minden olyan táblát keresése      | error | keressen az "error" (nem kis-és nagybetűket) |
| Válassza ki az adatok táblázatból | Típus = esemény |  Esemény |
|                        | Típus = esemény &#124; Válassza ki a forrás, az eseménynaplóban, eseményazonosító | Esemény &#124; a projekt forrás, az eseménynaplóban, eseményazonosító |
|                        | Típus = esemény &#124; az első 100 | Esemény &#124; 100 igénybe |
| Karakterláncok összehasonlításának      | Típus esemény Computer=srv01.contoso.com =   | Esemény &#124; Ha számítógép == "srv01.contoso.com" |
|                        | Típus esemény Computer=contains("contoso") = | Esemény &#124; Ha a számítógépen található a "contoso" (nem kis-és nagybetűket)<br>Esemény &#124; Ha számítógép contains_cs "Contoso" (kis-és nagybetűket) |
|                        | Típus = esemény számítógép = RegEx ("@contoso@")  | Esemény &#124; Ha a számítógép megegyezik regex ". *contoso*" |
| Dátum összehasonlítása        | Típus esemény TimeGenerated = > most-1DAYS | Esemény &#124; Ha TimeGenerated > ago(1d) |
|                        | Típus esemény TimeGenerated = > 2017-05-01 TimeGenerated < 2017-05-31 | Esemény &#124; Ha TimeGenerated között (datetime(2017-05-01)... datetime(2017-05-31)) |
| Logikai összehasonlítása     | Típus = szívverés IsGatewayInstalled = false  | Szívverés | Ha IsGatewayInstalled == false |
| Rendezés                   | Típus = esemény &#124; Számítógép asc, az eseménynaplóban desc, EventLevelName asc rendezése | Esemény \| Rendezze a számítógép asc, az eseménynaplóban desc, EventLevelName asc |
| Különböző               | Típus = esemény &#124; a deduplikáció számítógép \| Jelölje be a számítógép | Esemény &#124; számítógép, az eseménynaplóban összefoglalója |
| Oszlopok kiterjesztése         | Típus = telj CounterName = "kihasználtsága (%)" &#124; BŐVÍTÉSE if(map(CounterValue,0,50,0,1),"HIGH","LOW"), kihasználtsága | A Teljesítményfigyelő &#124; Ha CounterName == "kihasználtsága (%)" \| Kihasználtság kiterjesztése = iff ("Alacsony" a "Felső" > 50. ellenértéknek) |
| Összesítés            | Típus = esemény &#124; mérték count() számítógépenként darabszámként | Esemény &#124; összesíteni a Count = count() számítógépenként |
|                                | Típus = telj ObjectName processzor CounterName = = "kihasználtsága (%)" &#124; mérték avg(CounterValue) által számítógép időköz 5 perc | A Teljesítményfigyelő &#124; Ha ObjectName == "Processzor" és a CounterName == "kihasználtsága (%)" &#124; összefoglalója avg(CounterValue) számítógépenként bin (TimeGenerated, azaz 5 perc) |
| Összesítő és a korlátja | Típus = esemény &#124; mérték count() számítógép &#124; első 10 | Esemény &#124; AggregatedValue összefoglalója = count() számítógép &#124; 10 korlátozása |
| a UNION                  | Típus = esemény vagy típus = Syslog | Syslog esemény, Unió |
| Csatlakozás                   | Típus = NetworkMonitoring &#124; Csatlakozás a belső AgentIP (típus = szívverés) ComputerIP | NetworkMonitoring &#124; Csatlakozás típusú belső = (keresési típus == "Szívverés") a $left. AgentIP == $right.ComputerIP |



## <a name="next-steps"></a>Következő lépések
- Tekintse meg a [útmutató a lekérdezések írásáról](https://go.microsoft.com/fwlink/?linkid=856078) hello új lekérdezési nyelv használatával.
- Tekintse meg a toohello [Query Language Reference](https://go.microsoft.com/fwlink/?linkid=856079) minden parancs, a kezelők és a hello új query Language funkciók leírását.  
