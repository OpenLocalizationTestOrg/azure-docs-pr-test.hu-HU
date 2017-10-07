---
title: "aaaStreaming naplók és a konzol"
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
ms.openlocfilehash: bb4b8ce5358da12041e164dfff8f43790dd67924
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="streaming-logs-and-hello-console"></a>Folyamatos átviteli naplók és hello konzol
## <a name="streaming-logs"></a>Folyamatos átviteli naplók
Hello **Azure-portálon** biztosít egy integrált adatfolyam naplófájl-megjelenítő, amely lehetővé teszi a nyomkövetési események megtekintéséhez a **App Service** valós idejű alkalmazások.  

Ez a funkció beállítása néhány egyszerű lépést is igényel:

* Nyomok írja be a kódot
* Alkalmazás engedélyezése **diagnosztikai naplók** az alkalmazásra vonatkozóan
* Nézet hello adatfolyam a hello beépített **folyamatos átviteli naplók** hello felhasználói felülete **Azure-portálon**.

### <a name="how-toowrite-traces-in-your-code"></a>Hogyan toowrite követi a kódban
A kód a nyomkövetési adatokat írni az egyszerű.  C# nyelven lehető legkönnyebben írása hello a következő kódot:

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

hello nyomkövetési osztály él hello System.Diagnostics névtérben.

A node.js-alkalmazás az ezzel a kóddal írhat tooachieve hello ugyanazt az eredményt:

`````````````````````````
console.log("My trace statement").
`````````````````````````

### <a name="how-tooenable-and-view-hello-streaming-logs"></a>Hogyan tooenable és nézet hello folyamatos átviteli naplók
![][BrowseSitesScreenshot]Diagnosztika alapon / alkalmazás engedélyezve vannak. Első lépésként toohello hely tooenable szeretné ezt a beállítást, a böngészési.  

![][DiagnosticsLogs]A beállítások menü, görgessen lefelé toohello **figyelés** szakaszt, és kattintson a **(1) diagnosztikai naplók**. Majd **(2) engedélyezése** **Alkalmazásnaplózást (fájlrendszer)** vagy **Alkalmazásnaplózást (blob)** hello **szint** beállítást módosíthatja hello nyomkövetések toocapture súlyossági szintje. Ha csak próbált tooget ismeri a hello szolgáltatást, állítsa be az hello szintje túl**részletes** tooensure a nyomkövetési utasítások összegyűjtött vannak.

Kattintson a **mentése** hello felső hello panelt, és most készen áll a tooview naplókat.

> [!NOTE]
> hello magasabb hello **súlyossági szint** további erőforrások felhasznált toolog és további nyomkövetések előállítása hello hello. Győződjön meg arról, hogy **súlyossági szint** konfigurált toohello megfelelő részletességi éles vagy nagy forgalmú webhely van. 
> 
> 

![][StreamingLogsScreenshot]tooview hello **a folyamatos átviteli naplók** belül hello Azure-portálon kattintson az **(1) naplófolyamot** is a hello **figyelés** hello beállítások menü részét. Ha az alkalmazás nyomkövetési utasítások aktívan ír, akkor a hello kell látni őket **(2) streaming naplózza a felhasználói felület** közel valós időben.

## <a name="console"></a>Konzol
Hello **Azure-portálon** hozzáférés tooyour Konzolalkalmazás biztosít. Az alkalmazás fájlrendszer vizsgálatát, és powershell/cmd parancsfájlok futtatása. Ugyanazokkal az engedélyekkel a futó alkalmazások kódú állítható be, ha a konzol parancsok végrehajtása hello kötik. Hozzáférés tooprotected címtárak vagy parancsfájlok futtatását, emelt szintű jogosultságokat igénylő le van tiltva.  

![][ConsoleScreenshot]Beállítások menüjében, görgessen lefelé túl**Fejlesztőeszközök** szakaszt, és kattintson a **(1) konzol** és hello **(2) konzol** felhasználói felületének megnyitása toohello jobbra.

hello ismernie tooget **konzol**, próbálja alapvető parancsok, például:

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
