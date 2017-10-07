---
title: "aaaUpgrading Azure Naplóelemzés toonew napló keresési |} Microsoft Docs"
description: "új Naplóelemzési lekérdezési nyelv hello szinte és hello nyilvános előzetes verziójában való részvételhez.  Ez a cikk ismerteti az új nyelvi hello hello előnyei és hogyan tooconvert a munkaterületen."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 7659c9d1467cab79d3a16e73b52b507ed281b002
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-your-azure-log-analytics-workspace-toonew-log-search"></a>Az Azure Naplóelemzés munkaterület toonew napló keresés frissítése

> [!NOTE]
> Frissítési toohello új Log Analytics lekérdezési nyelv a túl idő jelenleg választható jogosultságot ad[hatékonyságának növeléséhez hello új nyelvi](https://go.microsoft.com/fwlink/?linkid=856078).  

hello új Naplóelemzési lekérdező nyelv, és tooupgrade azt tootake munkaterület előnyére van szüksége.  Ez a cikk ismerteti az új nyelvi hello hello előnyei és hogyan tooconvert a munkaterületen.  Ha nem választja tooupgrade, a munkaterület folytatódik, csakúgy, mint az toooperate mindig volt, de azt a rendszer automatikusan konvertál egy későbbi időpontban.  Jelentős mennyiségű időt és értesítést fog kapni, a dátumnak beállításakor.

Ez a cikk részletesen hello új nyelvet és a frissítési folyamat hello.

## <a name="why-hello-new-language"></a>Miért hello új nyelvi?
Tudjuk, hogy bármely átmenet problémás van, és azt nem csupán módosítása hello lekérdezési nyelv a hello visszatöltött azt.  Több oka is, amely ezt a változtatást jelentős értéket tooour Naplóelemzési ügyfelek.

- **Egyszerű, mégis hatékony.** hello új nyelvi könnyebb toounderstand, és a további szerkezetek hasonló tooSQL, például természetes nyelvű, mint a hagyományos nyelvi hello.
- **Teljes kimenetátirányításával nyelv.**  hello új nyelvi szélesebb körű kimenetátirányításával képességekkel rendelkezik mint hello örökölt nyelv.  Gyakorlatilag minden kimeneti lehet védőeszközön tooanother parancs toocreate összetettebb lekérdezések, mint korábban volt.
- **Keresési kötött mező információkinyerés.**  hello új nyelv speciális számított futtatókörnyezeti mezőkben, mint a hagyományos nyelvi hello támogatja.  Kiegészítő mezők összetett számítások használja, és kövesse a további parancsok, beleértve az összekapcsolásokhoz és az aggregációhoz hello számított mezők.
- **Speciális illesztéseket.**  hello új nyelvi speciális illesztések mint hello örökölt nyelvi hello képességét toojoin táblázatot több mezőt is biztosít, belső és külső illesztések használata és a kiterjesztett mezőkre csatlakozás.
- **Dátum és idő funkciók.**  hello új nyelvi hello örökölt nyelvi-nál több speciális dátum/idő funkciók.
- **Intelligens elemzés.**  Új nyelv hello speciális algoritmusok tooevaluate minták adatkészleteket, és hasonlítsa össze az adatok más-más részhalmazához.
- **Speciális Analytics-portálról.**  hello Advanced Analytics portál elemzési funkciókat nyújtja nem érhető el a hello Naplóelemzés portáljáról, beleértve a többsoros szerkesztési lekérdezéseket, a további képi megjelenítések és a speciális diagnosztika.
- **Más alkalmazásokkal konzisztencia.**  Új nyelv és hello Advanced Analytics portál már használhatók az Application Insightsban analytics hello.  A Naplóelemzési bevezetné, lehetővé teszi az konzisztencia Azure-szolgáltatásokhoz.
- **Jobb integráció a Power bi-ban.** Lekérdezések hello új nyelven lehet exportált tooPower BI Desktopban, így használhatja a részletes adatok átalakítása képességei.
- **Sok minden mást.** Tekintse meg a toohello [Azure napló Analytics lekérdezési nyelv](https://docs.loganalytics.io) helyek további információkat és oktatóanyagok hello új nyelvével.


## <a name="when-can-i-upgrade"></a>Ha frissíthető?
hello frissítés lesz állítva az összes Azure-régiók közötti ezért elérhető egyes régiókban többi előtt.  Tudni fogja, a munkaterület Ha elérhető toobe frissítése a lila hello szalagcím lásd: a munkaterület felkérte a tooupgrade hello tetején.

![1. frissítése](media/log-analytics-log-search-upgrade/upgrade-01a.png)

## <a name="what-happens-when-i-upgrade"></a>Mi történik, ha frissítek?
Ha a munkaterületet, minden mentett keresések, a riasztási szabályok és a nézeteket, amelyek az adatforrásnézet-tervezőből hello létrehozott nincs automatikusan átalakított toohello új nyelv.  Megoldások szereplő keresések nem automatikusan történik, de a azok még inkább hello menet közben a megnyitásakor.  Ez a teljes mértékben transzparens tooyou.

## <a name="can-i-go-back-after-i-upgrade"></a>Frissíthetem után is lépjen?
Amikor frissít, a munkaterület egy teljes biztonsági mentés használatban van, amely tartalmazza az összes mentett keresések, a riasztási szabály és a nézetek pillanatképet.  Ez lehetővé teszi, hogy toorestore a régi munkaterület, ha később kell felügyelni.

toorestore örökölt munkaterületén lépjen túl**beállítások** a munkaterületen, majd **összefoglaló frissítése**.  Azt is választhatja hello túl**örökölt munkaterület visszaállítása**.  

![Örökölt visszaállítása](media/log-analytics-log-search-upgrade/restore-legacy-b.png)

## <a name="how-do-i-perform-hello-upgrade"></a>Hogyan végezhető el hello frissítését?
A munkaterület frissítheti, amikor lila hello szalagcím hello portal hello tetején megjelenik.  

1.  Hello frissítési folyamat elindításához lila hello szalagcím, amely szerint kattintva **további és frissítési**.<br>![2. frissítés](media/log-analytics-log-search-upgrade/upgrade-01a.png)<br>
2.  Olvassa végig hello tájékozódhat hello frissítés hello frissítési információkat lapon.<br>![2. frissítés](media/log-analytics-log-search-upgrade/upgrade-03.png)<br>
3.  Kattintson a **frissítés most** toostart hello frissítését.<br>![4. frissítés](media/log-analytics-log-search-upgrade/upgrade-04.png)<br>Egy értesítési mezőben hello jobb felső sarkában található hello állapotát jeleníti meg.<br>![5. frissítése](media/log-analytics-log-search-upgrade/upgrade-05.png)
4.  Készen is van.  Tekintse meg a frissített munkaterület vizsgálni toohello naplófájl-keresési lap toohave.<br>![6 frissítése](media/log-analytics-log-search-upgrade/upgrade-06.png)<br>

Ha hello frissítési toofail okozó hibát észlel, elvégezheti a toohello [vitafóruma](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights) és tegye fel a kérdését vagy [hozzon létre egy támogatási kérést](../azure-supportability/how-to-create-azure-support-request.md) a hello Azure-portálon.

## <a name="how-do-i-learn-hello-new-language"></a>Hogyan ismerje meg az új nyelvi hello?
Mivel több szolgáltatás használják létrehoztunk Önnek egy [külső webhely toohost hello dokumentáció](https://docs.loganalytics.io/) hello új nyelvhez.  Ez magában foglalja, oktatóanyagok, példák és a teljes hivatkozási toohelp toospeed készít. Egy új nyelv hello az oktatóanyag lépéseinek végigvezetheti [Ismerkedés a lekérdezések](https://go.microsoft.com/fwlink/?linkid=856078) és hozzáférési hello nyelvi referencia: [Log Analytics lekérdezési nyelv](https://go.microsoft.com/fwlink/?linkid=856079).  

Ha már ismeri a hello örökölt Log Analytics lekérdezési nyelv azonban, majd hello nyelvi konverter tooyour munkaterület felvett hello frissítés részeként is használhatja.

A hagyományos lekérdezés megírása, és kattintson a **átalakítása** toosee hello lefordított verzióját.  Ezután kattintson a hello Keresés gomb toorun hello keresési vagy másolása és illessze be a konvertált hello lekérdezés toouse valahol máshol, például a riasztási szabályt.

![Nyelvi konverter](media/log-analytics-log-search-upgrade/language-converter.png)


## <a name="next-steps"></a>Következő lépések
- Tekintse meg a [oktatóanyag hello új nyelvével](https://go.microsoft.com/fwlink/?linkid=856078).
- Végezze el a [hello napló keresése portál használata az oktatóanyag](log-analytics-log-search-log-search-portal.md) hello új lekérdezési nyelv.
- Egy bevezető toohello új beolvasása [Advanced Analytics portál](https://go.microsoft.com/fwlink/?linkid=856587).
