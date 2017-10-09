---
title: "az OMS szolgáltatáshoz aaaConfigure adatforrások |} Microsoft Docs"
description: "Adatforrások határozza meg, hogy Naplóelemzési gyűjti az ügynökök és egyéb kapcsolódó források hello adatokat.  Ez a cikk ismerteti, hogyan használja a Naplóelemzési az adatforrások hello fogalmát, azt ismerteti, hogyan hello részleteit tooconfigure őket, és a hello különböző forrásokból elérhető összegzését tartalmazza."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 67710115-c861-40f8-a377-57c7fa6909b4
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/23/2017
ms.author: bwren
ms.openlocfilehash: ebe8d29a2442a654b98004f624181ff406868e2c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-sources-in-log-analytics"></a>A Naplóelemzési adatforrások
Naplóelemzési hello csatlakoztatott források szakaszát az OMS-munkaterület gyűjti az adatokat, és tárolja a OMS-tárházban.  minden gyűjtött adatok hello hello konfigurált adatforrások határozzák meg.  Hello a rekordkészlet OMS-tárház tárol adatokat.  Az egyes adatforrások egy adott típusú rekordot hoz létre a saját tulajdonságkészletbe minden típus.

![Jelentkezzen az Analytics-adatok gyűjtése](./media/log-analytics-data-sources/overview.png)

Az adatforrások azok is adatokat gyűjteni a csatlakoztatott források és rekordok létrehozása a hello OMS-tárházban OMS-megoldások eltérő.  Megoldások adhat hozzá tooyour munkaterület hello megoldások gyűjteménye, és általában biztosít az OMS-portálon hello további elemzésére szolgáló eszközöket.  

## <a name="summary-of-data-sources"></a>Adatforrások áttekintése
hello adatforrások, amelyek jelenleg a Naplóelemzési hello a következő táblázatban láthatók.  Mindegyik rendelkezik-e a hivatkozás tooa külön cikk részletek megadása, hogy az adatforrás.

| Adatforrás | Esemény típusa | Leírás |
|:--- |:--- |:--- |
| [Egyéni naplók](log-analytics-data-sources-custom-logs.md) |\<Naplónév\>_CL |Windows vagy Linux ügynökök naplófájl-információkat tartalmazó szövegfájlok. |
| [Windows-Eseménynapló](log-analytics-data-sources-windows-events.md) |Esemény |Hello Eseménynapló gyűjtött eseményeket a Windows rendszerű számítógépeken. |
| [Windows-teljesítményszámlálók](log-analytics-data-sources-performance-counters.md) |A Teljesítményfigyelő |Windows rendszerű számítógépek gyűjtött teljesítményszámlálók. |
| [Linux-teljesítményszámlálók](log-analytics-data-sources-performance-counters.md) |A Teljesítményfigyelő |A Linux rendszerű számítógépekről gyűjtött teljesítményszámlálók. |
| [IIS-naplók](log-analytics-data-sources-iis-logs.md) |W3CIISLog |Az Internet Information Services W3C formátumban naplózza. |
| [Syslog](log-analytics-data-sources-syslog.md) |Rendszernapló |Syslog-események Windows vagy Linux rendszerű számítógépeken. |

## <a name="configuring-data-sources"></a>Adatforrások konfigurálása
Hello adatforrást konfigurálnia **adatok** Naplóelemzési menüjében **beállítások**.  Bármely konfiguráció az OMS-munkaterület tooall csatlakoztatott források kerülnek.  Ez a konfiguráció jelenleg nem összes ügynököt kizárni.

![Windows-események konfigurálása](./media/log-analytics-data-sources/configure-events.png)

1. Hello OMS-konzolon kattintson a hello **beállítások** csempe vagy hello **beállítások** üdvözlő képernyőt hello tetején gombra.
2. Válassza ki **adatok**.
3. Kattintson a hello adatok forrás tooconfigure.
4. Hello hivatkozás toohello hello részletes táblázat felett található minden adatforrás dokumentációját kövesse a konfigurációt.

> [!NOTE]
> Naplóelemzési adatforrások hello Azure-portál jelenleg nem lehet konfigurálni.

## <a name="data-collection"></a>Adatgyűjtés
Adatok forrás konfigurációkat, amelyek közvetlenül csatlakoztatott tooLog Analytics néhány percen belül tooagents érkeznek.  hello megadott adatok gyűjtése hello ügynök, és közvetlenül tooLog időszakok adott tooeach adatforrásban Analytics kézbesíteni.  A részletekről az egyes adatforrások hello dokumentációjában talál.

A System Center Operations Manager (SCOM) ügynökök a csatlakoztatott felügyeleti csoport adatok forrás konfigurációk lefordítani a felügyeleti csomagok, és alapértelmezés szerint 5 percenként toohello felügyeleti csoport kézbesíteni.  hello ügynök hello felügyeleti csomag más tölti le, és gyűjti hello megadott adatok. Attól függően, hogy hello adatok forrás hello adatokat kell vagy elküldi tooa felügyeleti kiszolgálóra, amely hello adatok toohello Naplóelemzési továbbítja, vagy hello ügynök hello felügyeleti kiszolgálón keresztül küldi hello adatok tooLog elemzés. Tekintse meg a túl[az gyűjtemény adatait az OMS-szolgáltatásaival és megoldásaival](log-analytics-add-solutions.md#data-collection-details) részleteiről.  Áttekintheti az SCOM és OMS összekötő és hello gyakoriságának módosítása, hogy a konfigurálás a rendszer [integráció konfigurálása a System Center Operations Manager](log-analytics-om-agents.md).

Ha hello ügynök nem tooconnect tooLog Analytics vagy az Operations Manager, továbbra is azt fog továbbítani, amikor kapcsolatot létesít az toocollect adatokat.  Ha hello adatok mennyisége eléri hello ügyfél hello gyorsítótár maximális méretét, vagy ha hello ügynök nem tudja tooestablish 24 órán belül egy kapcsolat adat elveszhet.

## <a name="log-analytics-records"></a>Log Analytics-rekordok
A Naplóelemzési által gyűjtött összes adat hello OMS-tárház rekordok tárolja.  Különböző forrásokból gyűjtött rögzíti a saját tulajdonságkészletbe lesz, és azonosítható, ha azok **típus** tulajdonság.  Minden adatforrás hello dokumentációjában és a megoldás részletes tekintse meg a különböző rekordtípusú.

## <a name="next-steps"></a>Következő lépések
* További tudnivalók [megoldások](log-analytics-add-solutions.md) , amely funkció tooLog Analytics hozzáadása és is gyűjthet adatokat történő hello OMS tárházba.
* További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) tooanalyze hello adatokat gyűjteni az adatforrások és megoldásokat.  
* Konfigurálása [riasztások](log-analytics-alerts.md) tooproactively tájékoztatást adnak a kritikus fontosságú adatok összegyűjtése az adatforrások és megoldásokat.
