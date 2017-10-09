---
title: "aaaCollect és elemezheti a Windows eseménynaplóiban keresse meg az OMS szolgáltatáshoz |} Microsoft Docs"
description: "Windows-eseménynaplók hello leggyakoribb által használt adatforrás Naplóelemzési egyikét.  Ez a cikk ismerteti, hogyan tooconfigure gyűjteménye a Windows-eseménynaplók és hello rekordok részleteit hoznak létre a hello OMS-tárházban."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: ee52f564-995b-450f-a6ba-0d7b1dac3f32
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/15/2017
ms.author: bwren
ms.openlocfilehash: c05648af39258443f22fd11e1d751b5ccec8c391
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="windows-event-log-data-sources-in-log-analytics"></a>A Windows Eseménynapló adatforrások Naplóelemzési
Windows-eseménynaplók rendszer egyik leggyakoribb hello [adatforrások](log-analytics-data-sources.md) Windows ügynököt használ, mivel számos alkalmazás írása toohello Windows-Eseménynapló adatainak gyűjtéséhez.  Begyűjtheti események hozzáadását toospecifying a rendszer és az alkalmazások például szabványos naplókból a létrehozott egyéni naplók alkalmazások toomonitor kell.

![Windows-események](media/log-analytics-data-sources-windows-events/overview.png)     

## <a name="configuring-windows-event-logs"></a>Konfigurálása Windows-eseményt naplóz
Konfigurálja a Windows hello eseménynaplók [Naplóelemzés beállításai adatok menüben](log-analytics-data-sources.md#configuring-data-sources).

A Naplóelemzési hello Windows Eseménynapló, amely a megadott hello-beállítások csak eseményeket gyűjti.  Írja be a hello napló hello nevét, majd kattintson az Eseménynapló adhat hozzá  **+** .  Az egyes naplókon csak hello események kijelölt hello súlyosságokkal összegyűjtése.  Ellenőrizze a hello fokozatok hello adott napló, amelyet az toocollect.  További feltételek bármelyikének toofilter események nem biztosítanak.

Az Eseménynapló neve hello megadásakor Naplóelemzési nyújt a közös eseménynaplók neve. Ha azt szeretné, hogy tooadd hello napló hello lista nem jelenik meg, továbbra is hozzáadhatja írja be a hello napló hello teljes nevét. Az Eseménynapló használatával található hello napló hello teljes nevét. Az eseménynaplóban, nyissa meg a hello *tulajdonságok* hello naplót, és másolja hello karakterláncnak a következőről hello lapján *teljes nevét* mező.

![Windows-események konfigurálása](media/log-analytics-data-sources-windows-events/configure.png)

## <a name="data-collection"></a>Adatgyűjtés
A Naplóelemzési gyűjt minden esemény, amely megfelel a kiválasztott súlyossága a figyelt eseménynaplóban hello esemény létrehozását.  hello ügynök helyére azt gyűjti össze az egyes eseménynapló rögzíti.  Hello ügynök egy ideig offline állapotba kerül, ha majd Naplóelemzési gyűjt eseményeket ahol utolsó abbamaradtak, még akkor is, ha olyan eseményeket hozta létre, miközben hello ügynök offline állapotban volt.  Lehetőség van az ezen események toonot gyűjtik be, ha hello Eseménynapló becsomagolja elveszne események felülírás hello ügynök kapcsolat nélküli módban.

>[!NOTE]
>A Naplóelemzési nem gyűjt forrás SQL-kiszolgáló által létrehozott események naplózása *MSSQLSERVER* azonosítójú esemény 18453 kulcsszavak - tartalmazó *klasszikus* vagy *naplózási sikeres* és kulcsszó *0xa0000000000000*.
>

## <a name="windows-event-records-properties"></a>A Windows esemény-rekordok tulajdonságai
Windows eseményrekordok típusa lehet **esemény** és a következő táblázat hello hello jellemzőkkel rendelkezik:

| Tulajdonság | Leírás |
|:--- |:--- |
| Computer |Az összegyűjtött esemény hello hello számítógép nevét. |
| EventCategory |Hello esemény kategóriáját. |
| EventData |Összes esemény adatai nyers formátumban. |
| Eseményazonosító |Hello esemény száma. |
| EventLevel |A numerikus formában hello esemény súlyossága. |
| EventLevelName |A szöveges formában hello esemény súlyossága. |
| Az eseménynaplóban talál |Az összegyűjtött esemény hello hello Eseménynapló neve. |
| ParameterXml |Esemény paraméterértékek XML formátumban. |
| ManagementGroupName |A System Center Operations Manager-ügynökök hello felügyeleti csoport neve.  Más ügynökök ezt az értéket a rendszer AOI-<workspace ID> |
| RenderedDescription |Esemény – leírás paraméterértékekkel |
| Forrás |Hello esemény forrását. |
| SourceSystem |Az ügynök hello esemény típusa történt gyűjtött. <br> OpsManager – Windows-ügynök, vagy közvetlenül kapcsolódjon, vagy az Operations Manager által felügyelt <br> Linux – az összes Linux-ügynökök  <br> AzureStorage – az Azure Diagnostics |
| TimeGenerated |A Windows hello esemény dátuma és időpontja hozták létre. |
| Felhasználónév |Hello eseményt naplózó hello fiók felhasználónevét. |

## <a name="log-searches-with-windows-events"></a>Napló megkeresi a Windows-események
hello következő táblázat példákat különböző Windows-esemény lehívása napló kereséseket.

| Lekérdezés | Leírás |
|:--- |:--- |
| Típus = esemény |Minden Windows-eseményeket. |
| Típus = esemény EventLevelName hiba = |Minden Windows-hiba az eseményeket. |
| Típus = esemény &#124; Mérték count() forrás |Forrás száma a Windows-eseményeket. |
| Típus = esemény EventLevelName = hiba &#124; Mérték count() forrás |Hiba események száma a Windows forrás. |


>[!NOTE]
> Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), majd a fenti lekérdezések hello megváltozna toohello következő.
>
>| Lekérdezés | Leírás |
|:---|:---|
| Esemény |Minden Windows-eseményeket. |
| Esemény &#124; Ha EventLevelName == "error" |Minden Windows-hiba az eseményeket. |
| Esemény &#124; forrás count() összefoglalója |Forrás száma a Windows-eseményeket. |
| Esemény &#124; Ha EventLevelName == "error" &#124; forrás count() összefoglalója |Hiba események száma a Windows forrás. |


## <a name="next-steps"></a>Következő lépések
* Más Naplóelemzési toocollect konfigurálása [adatforrások](log-analytics-data-sources.md) elemzés céljából.
* További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) tooanalyze hello adatokat gyűjteni az adatforrások és megoldásokat.  
* Használjon [egyéni mezők](log-analytics-custom-fields.md) tooparse hello eseményrekordok egyes mezőkbe.
* Konfigurálása [teljesítményszámlálók gyűjteményét](log-analytics-data-sources-performance-counters.md) a Windows-ügynökök.
