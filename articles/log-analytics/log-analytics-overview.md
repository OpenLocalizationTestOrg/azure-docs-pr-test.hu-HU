---
title: "aaaWhat a Naplóelemzési Operations Management Suite (OMS)? | Microsoft Docs"
description: "A Log Analytics az Operations Management Suite (OMS) egy szolgáltatása, amely segít összegyűjteni és elemezni a felhőben és a helyszíni környezetekben található erőforrások által létrehozott működési adatokat.  Ez a cikk rövid áttekintést nyújt az hello Naplóelemzési és összetevői hivatkozások toodetailed tartalmat."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: bd90b460-bacf-4345-ae31-26e155beac0e
ms.service: log-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/12/2017
ms.author: bwren
ms.openlocfilehash: b35da77e3782f7c1fe54cc34142f8cd9f2eb57d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-log-analytics"></a>Mi az a Log Analytics?
A Naplóelemzési rendszer szolgáltatása [Operations Management Suite \(OMS\) ](../operations-management-suite/operations-management-suite-overview.md) , amely figyeli a felhőalapú és helyszíni környezetben toomaintain, azok rendelkezésre állását és teljesítményét.  Összegyűjti az több forrás erőforrások a felhőalapú és helyszíni környezetben és az egyéb felügyeleti eszközök tooprovide elemzés által létrehozott adatok.  Ez a cikk ismerteti egy rövid leírását a hello értéket, hogy a Naplóelemzési áttekintése, hogyan működik, és hivatkozások toomore részletes tartalom, így további átrágniuk is.

## <a name="is-log-analytics-for-you"></a>A Log Analytics az Önnek megfelelő szolgáltatás?
Ha jelenleg nincs megfigyelés üzembe helyezve az Azure-környezetében, érdemes az [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md) szolgáltatással kezdeni, amely az Azure-erőforrások figyelési adatait gyűjti és elemzi.  Naplóelemzési is [adatokat gyűjteni Azure figyelő](log-analytics-azure-storage.md) toocorrelate más adatokkal, és adja meg a további elemzés.

Ha azt szeretné toomonitor a helyszíni környezetben, vagy már meglévő felügyeleti szolgáltatásokat, például Azure figyelő vagy a System Center Operations Manager használatával van, majd Naplóelemzési jelentős értéket adhat hozzá.  Képes egyetlen tárházba gyűjteni a közvetlenül az ügynökökből és ezekből az egyéb eszközökből származó adatokat.  A Log Analytics elemzőeszközei (például naplókeresés, nézetek és megoldások) az összes begyűjtött adattal működnek, így lehetővé teszik a teljes környezet központosított elemzését.


## <a name="using-log-analytics"></a>A Log Analytics használata
A Naplóelemzési hello OMS-portálon vagy az Azure portál, amely futtatását a böngészőben, és adja meg a hozzáférés tooconfiguration beállításokkal, és több eszközök tooanalyze és összegyűjtött adatok rájuk hello keresztül érheti el.  Hello portálról kihasználhatja [keresések jelentkezzen](log-analytics-log-searches.md) , hozhat létre lekérdezéseket tooanalyze gyűjtött adatokat, ha [irányítópultok](log-analytics-dashboards.md) amely testre szabhatja a legértékesebb keresések grafikus nézetekkel és [megoldások](log-analytics-add-solutions.md) cikk nyújt további funkciók és -elemző eszközökkel.

az alábbi képen hello már a hello OMS-portálon mely látható hello irányítópult hello összegzési adatait megjelenítő [megoldások](#add-functionality-with-management-solutions) telepített hello munkaterületen.  Rákattinthat a további bármely csempe toodrill az adott megoldáshoz tartozó hello adatokká.

![OMS-portál](media/log-analytics-overview/portal.png)

A Naplóelemzési tartalmaz egy lekérdezési nyelv tooquickly olvashatók be, és összevonni az adatokat a hello tárházban.  A létrehozott és mentett [napló keresések](log-analytics-log-searches.md) toodirectly adatelemzés hello portálon vagy a napló keresések futtatta-e automatikusan toocreate riasztást hello hello lekérdezési eredmények jelzik, egy fontos állapot.

![Naplókeresés](media/log-analytics-overview/log-search.png)

a teljes környezet hello állapotának gyors grafikus nézetének tooget, adhat hozzá a mentett napló keresések tooyour képi megjelenítések [irányítópult](log-analytics-dashboards.md).   

![Irányítópult](media/log-analytics-overview/dashboard.png)

A Naplóelemzési kívüli rendelés tooanalyze adatokat, exportálhatja hello adatok adattárból hello OMS az eszközök például [Power BI](log-analytics-powerbi.md) vagy Excel.  Kihasználhatja a hello [napló Search API](log-analytics-log-search-api.md) toobuild egyéni megoldások, amelyek a Log Analytics-adatok vagy a más rendszerekkel toointegrate.

## <a name="add-functionality-with-management-solutions"></a>Funkciók hozzáadása felügyeleti megoldásokkal
[Megoldások](log-analytics-add-solutions.md) funkció tooOMS további adatokat és elemzési eszközök tooLog Analytics hozzáadása.  Akkor is határozhatnak meg új rekordtípusokhoz toobe gyűjtött, a napló keresések vagy hello irányítópult hello megoldás által biztosított további felhasználói felület elemzése.  hello például az alábbi képen látható hello [változáskövetési megoldás](log-analytics-change-tracking.md)

![Változáskövetési megoldás](media/log-analytics-overview/change-tracking.png)

A megoldások változatos funkciókkal érhetők el, valamint a további hozzáadott megoldások listája is folyamatosan bővül.  Könnyen böngészhet a rendelkezésre álló megoldások és [tooyour OMS-munkaterület adja hozzá](log-analytics-add-solutions.md) hello megoldások gyűjtemény vagy az Azure piactéren.  Számos megoldást automatikusan telepíti a rendszer, és azonnal elkezdenek működni, míg mások némi konfigurálást igényelnek.

![Megoldástár](media/log-analytics-overview/solution-gallery.png)

## <a name="log-analytics-components"></a>A Log Analytics összetevői
A Naplóelemzési center hello hello OMS tárház hello Azure felhőben lévő.  Adatgyűjtés történő hello tárházba csatlakoztatott adatforrások konfigurálását az adatforrások és hozzáadását megoldások tooyour előfizetés.  Az adatforrások és a megoldások egyes létrehoz különböző rekordtípusokat, saját tulajdonságokat rendelkeznie, de előfordulhat, hogy továbbra is elemezheti együtt lekérdezések toohello tárházban.  Ez lehetővé teszi a különböző típusú adatok azonos eszközök és módszerek toowork különböző forrásokból gyűjtött toouse hello.

![OMS-tárház](media/log-analytics-overview/overview.png)

Csatlakoztatott adatforrások hello számítógépeket és más erőforrásokat, amelyek létrehozzák a Naplóelemzési által gyűjtött adatokat is.  Ilyenek lehetnek például a közvetlenül csatlakozó [Windows](log-analytics-windows-agents.md)- és [Linux](log-analytics-linux-agents.md)-rendszerű számítógépeken telepített, illetve az egy [csatlakoztatott System Center Operations Manager felügyeleti csoportban](log-analytics-om-agents.md) lévő ügynökök.  Az Azure-erőforrásokhoz a Log Analytics az [Azure Monitor és Azure Diagnostics](log-analytics-azure-storage.md) szolgáltatásokból gyűjt adatot.

[Adatforrások](log-analytics-data-sources.md) hello különböző típusú minden csatlakoztatott forrásból származó adatokat.  Ez magában foglalja [események](log-analytics-data-sources-windows-events.md) és [teljesítményadatokat](log-analytics-data-sources-performance-counters.md) a [Windows](log-analytics-data-sources-windows-events.md) és hozzáadását toosources többek között a Linux-ügynökök [IIS-napló](log-analytics-data-sources-iis-logs.md), és [egyéni szöveges naplók](log-analytics-data-sources-custom-logs.md).  Beállíthatja, hogy azt szeretné, hogy toocollect és hello konfigurációs automatikusan kézbesített tooeach csatlakoztatott adatforrás minden adatforrás.

Ha egyéni követelményeknek való megfelelés, akkor használhatja a hello [HTTP adatait gyűjtője API](log-analytics-data-collector-api.md) toohello adattárház toowrite a REST API-ügyfél.

## <a name="log-analytics-architecture"></a>Log Analytics-architektúra
Naplóelemzési hello telepítési követelményeinek minimálisak, mivel hello központi összetevők hello Azure felhőben tárolt.  Ez magában foglalja hello tárház hozzáadása toohello szolgáltatások, amelyek lehetővé teszik toocorrelate és összegyűjtött adatok elemzésére.  hello portal a rendszer minden böngészőből elérhető, így nincs szükség a kliens szoftver is.

[Windows](log-analytics-windows-agents.md)- és [Linux](log-analytics-linux-agents.md)-rendszerű számítógépeken ügynököket kell telepítenie, ám azok a számítógépek, amelyek már egy [csatlakoztatott SCOM-felügyeleti csoport](log-analytics-om-agents.md) tagjai, nem igényelnek további ügynököket.  SCOM-ügynökök továbbra is az felügyeleti kiszolgálóhoz, amely továbbítja az adatokat tooLog Analytics toocommunicate.  Néhány megoldás, ha igényel Naplóelemzési közvetlenül az ügynökök toocommunicate.  minden egyes-megoldása dokumentációjában hello kell megadnia a kommunikációra vonatkozó követelményekről.

[Log Analytics-regisztrációkor](log-analytics-get-started.md) egy OMS-munkaterületet fog létrehozni.  Az eltolásokat tekintheti hello munkaterületen, a saját adattárház, az adatforrások és a megoldások egy egyedi Naplóelemzési környezetben. Előfordulhat, hogy az előfizetés toosupport több munkaterületek létrehozása több környezetekben, például éles és teszteléséhez.

![Log Analytics-architektúra](media/log-analytics-overview/architecture.png)

## <a name="next-steps"></a>Következő lépések
* [Regisztráljon egy ingyenes Log Analytics-fiók](log-analytics-get-started.md) tootest a saját környezetében.
* Másik nézet hello [adatforrások](log-analytics-data-sources.md) elérhető toocollect adatok történő hello OMS tárházba.
* [Hello elérhető megoldások kereséséhez a hello megoldások gyűjtemény](log-analytics-add-solutions.md) tooadd funkció tooLog elemzés.

