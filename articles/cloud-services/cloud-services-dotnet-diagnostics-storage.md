---
title: "aaaStore és diagnosztikai adatok megtekintése az Azure Storage |} Microsoft Docs"
description: "Az Azure diagnosztikai adatokat az Azure Storage és az megtekintése"
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: tysonn
ms.assetid: 18e0780d-43e7-41e4-b8e9-f1fb9a36eb03
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2016
ms.author: robb
ms.openlocfilehash: dd47a2ef6d6488c80c102c72b2ebf6ca6d2e473f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="store-and-view-diagnostic-data-in-azure-storage"></a>Az Azure Storage diagnosztikai adatok tárolása és nézet
Diagnosztikai adatok nem tárolódik véglegesen, kivéve, ha azt át toohello Microsoft Azure storage emulator vagy tooAzure tárolására. Egyszer, megőrzés is megtekinthető számos elérhető eszköz egyike.

## <a name="specify-a-storage-account"></a>Adja meg a storage-fiók
Megadja, hogy szeretné-e a hello ServiceConfiguration.cscfg fájlban toouse hello tárfiók. hello fiókadatok egy kapcsolati karakterláncot egy konfigurációs beállítás típusúként van definiálva. hello alábbi példa bemutatja hello alapértelmezett kapcsolati karakterlánc létrehozása a Visual Studio új Cloud Service projektek:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

Módosíthatja a kapcsolati karakterlánc tooprovide fiókadatai Azure storage-fiók.

Hello gyűjtött diagnosztikai adatok típusától függően a Azure Diagnostics hello Blob szolgáltatás vagy hello Table szolgáltatás használja. hello következő táblázatban megmaradnak hello adatforrások és a formátuma.

| Adatforrás | Tárolási formátum |
| --- | --- |
| Az Azure naplói |Tábla |
| Az IIS 7.0-naplók |Blob |
| Az Azure Diagnostics infrastruktúra naplók |Tábla |
| Sikertelen kérelmek nyomkövetési naplóit |Blob |
| Windows-Eseménynapló |Tábla |
| Teljesítményszámlálók |Tábla |
| Összeomlási memóriaképek |Blob |
| Egyéni hibanaplókat |Blob |

## <a name="transfer-diagnostic-data"></a>Diagnosztikai adatok átvitele
Az SDK 2.5-ös vagy újabb hello kérelem tootransfer diagnosztikai adatok akkor fordulhat elő, hello konfigurációs fájl segítségével. Diagnosztikai adatok vihetők át a hello konfigurációs rendszeres időközönként.

Az SDK 2.4 és az előző kérhet tootransfer hello diagnosztikai adatok keresztül hello konfigurációs fájlba is, programozott módon. hello programozott módszert is lehetővé teszi toodo igény átvitel.

> [!IMPORTANT]
> Diagnosztikai adatok tooan Azure storage-fiók átvitel során fel Önnek a diagnosztikai adatok által használt hello tárerőforrásokhoz költségek.
> 
> 

## <a name="store-diagnostic-data"></a>Diagnosztikai adatok tárolásához
Naplóadatok tárolt Blob vagy a tábla a következő neveket hello:

**Táblák**

* **WadLogsTable** - kód megadása hello nyomkövetés-figyelő írt naplókat.
* **WADDiagnosticInfrastructureLogsTable** -diagnosztikai figyelő és a konfigurációs módosításokat.
* **WADDirectoriesTable** – könyvtárak adott hello diagnosztikai figyelő által figyelt.  Ez magában foglalja az IIS-napló, az IIS nem sikerült a kérelem naplókat, és egyéni könyvtárak.  hello blob naplófájl helye hello hello tároló mezőben megadott hello név pedig hello BLOB hello RelativePath mezőben.  adott hello Azure virtuális gép verzióját hello AbsolutePath mező hello helyét és hello fájl nevét jelzi.
* **WADPerformanceCountersTable** – teljesítményszámlálók.
* **WADWindowsEventLogsTable** – Windows-eseményt naplózza.

**Blobok**

* **üvegvatta-vezérlő-tároló** – (csak SDK 2.4 és előző) hello Azure diagnostics meghatározza hello XML konfigurációs fájlokat tartalmaz.
* **üvegvatta-az iis-failedreqlogfiles** – IIS sikertelen kérelem naplók adatait tartalmazza.
* **üvegvatta-az iis-naplófájlok** – IIS-napló adatait tartalmazza.
* **"egyéni"** – egy egyéni tároló alapján hello diagnosztikai figyelő által figyelt könyvtárak beállítása.  a blob tároló hello nevét fogja megadni WADDirectoriesTable.

## <a name="tools-tooview-diagnostic-data"></a>Eszközök tooview diagnosztikai adatok
Számos eszközt olyan elérhető tooview hello adatok, miután átvitt toostorage. Példa:

* A Visual Studio - Server Explorer Ha telepítve van a hello Azure eszközök a Microsoft Visual Studio, használhatja hello Azure Storage-csomópont a Server Explorer tooview csak olvasható blob és table adatokat az Azure storage-fiókok. A helyi storage emulator fiókhoz tartozó megjeleníthető adatok, és is a storage-fiókok hozott létre az Azure-bA. További információkért lásd: [böngészés és tárolási erőforrások kezelése a Server Explorer](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).
* [A Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy különálló alkalmazás, amely lehetővé teszi az Azure Storage-adatokkal Windows, OSX és Linux tooeasily használata.
* [Az Azure Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) tartalmazza az Azure Diagnostics Managert, amely lehetővé teszi a tooview, töltse le és kezelése az Azure-on futó hello alkalmazások által gyűjtött hello diagnosztikai adatokat.

## <a name="next-steps"></a>Következő lépések
[Cloud Services-alkalmazásokban az Azure diagnosztikai nyomkövetési hello folyamata](cloud-services-dotnet-diagnostics-trace-flow.md)

