---
title: "Folyamatos átviteli naplók és a konzol"
description: "Folyamatos átviteli naplók és a konzol áttekintése"
author: btardif
manager: erikre
editor: 
services: app-service\web
documentationcenter: 
ms.assetid: 3e50a287-781b-4c6a-8c53-eec261889d7a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/12/2016
ms.author: byvinyal
ms.openlocfilehash: 120ce6b115820728b9f853e9ff349beb0ef29c9d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="streaming-logs-and-the-console"></a>Folyamatos átviteli naplók és a konzol
## <a name="streaming-logs"></a>Folyamatos átviteli naplók
A **Azure-portálon** biztosít egy integrált adatfolyam naplófájl-megjelenítő, amely lehetővé teszi a nyomkövetési események megtekintéséhez a **App Service** valós idejű alkalmazások.  

Ez a funkció beállítása néhány egyszerű lépést is igényel:

* Nyomok írja be a kódot
* Alkalmazás engedélyezése **diagnosztikai naplók** az alkalmazásra vonatkozóan
* Az adatfolyam elérését a beépített megtekintése **folyamatos átviteli naplók** felhasználói felülete a **Azure-portálon**.

### <a name="how-to-write-traces-in-your-code"></a>Nyomok írásával a kódban
A kód a nyomkövetési adatokat írni az egyszerű.  C# nyelven egyszerű módon, a következő kódot:

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

A nyomkövetési osztály él, az System.Diagnostics névtérben.

A node.js-alkalmazás írhat a kódot, hogy az azonos eredmények elérése:

`````````````````````````
console.log("My trace statement").
`````````````````````````

### <a name="how-to-enable-and-view-the-streaming-logs"></a>Naplók engedélyezése és megtekintése a adatfolyamként való küldése
![][BrowseSitesScreenshot]Diagnosztika alapon / alkalmazás engedélyezve vannak. Indítsa el a Ez a funkció engedélyezése a kívánt hely tallózással.  

![][DiagnosticsLogs]A beállítások menü, görgessen le a **figyelés** szakaszt, és kattintson a **(1) diagnosztikai naplók**. Majd **(2) engedélyezése** **Alkalmazásnaplózást (fájlrendszer)** vagy **Alkalmazásnaplózást (blob)** a **szint** beállítás lehetővé teszi a nyomkövetési rögzítéséhez súlyosságának módosítása. Ha csak próbál Ismerkedjen meg a szolgáltatás, a szint beállítása **részletes** annak érdekében, hogy a nyomkövetési utasítások összegyűjtött vannak.

Kattintson a **mentése** felső részén a panelt, és készen áll a naplók megtekintéséhez.

> [!NOTE]
> A magasabb a **súlyossági szint** a több erőforrást felhasználják való bejelentkezéshez és a további nyomkövetés. Győződjön meg arról, hogy **súlyossági szint** éles vagy nagy forgalmú webhely helyes részletességi van konfigurálva. 
> 
> 

![][StreamingLogsScreenshot]Megtekintéséhez a **a folyamatos átviteli naplók** az Azure portálon, kattintson az **(1) naplófolyamot** is a **figyelés** a beállítások menü részét. Ha az alkalmazás nyomkövetési utasítások aktívan ír, akkor azok a a **(2) streaming naplózza a felhasználói felület** közel valós időben.

## <a name="console"></a>Konzol
A **Azure-portálon** konzolt az alkalmazásához való hozzáférést biztosít. Az alkalmazás fájlrendszer vizsgálatát, és powershell/cmd parancsfájlok futtatása. Ugyanazokkal az engedélyekkel állítja be az alkalmazás kódfuttatásra konzol parancsok végrehajtásakor kötik. A hozzáférést a védett könyvtárak, vagy a parancsfájlokat emelt szintű jogosultságokat igénylő futtasson le van tiltva.  

![][ConsoleScreenshot]Beállítások menüjében, görgessen le a **Fejlesztőeszközök** szakaszt, és kattintson a **(1) konzol** és a **(2) konzol** felhasználói felület jobb nyílik meg.

Megismerheti a **konzol**, próbálja alapvető parancsok, például:

`````````````````````````
dir
`````````````````````````

`````````````````````````
cd
`````````````````````````

<!-- Images. -->
[DiagnosticsLogs]: ./media/web-sites-streaming-logs-and-console/diagnostic-logs.png
[BrowseSitesScreenshot]: ./media/web-sites-streaming-logs-and-console/browse-sites.png
[StreamingLogsScreenshot]: ./media/web-sites-streaming-logs-and-console/streaming-logs.png
[ConsoleScreenshot]: ./media/web-sites-streaming-logs-and-console/console.png
