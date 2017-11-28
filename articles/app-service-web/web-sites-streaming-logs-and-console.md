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
# <a name="streaming-logs-and-hello-console"></a><span data-ttu-id="64e00-103">Folyamatos átviteli naplók és hello konzol</span><span class="sxs-lookup"><span data-stu-id="64e00-103">Streaming Logs and hello Console</span></span>
## <a name="streaming-logs"></a><span data-ttu-id="64e00-104">Folyamatos átviteli naplók</span><span class="sxs-lookup"><span data-stu-id="64e00-104">Streaming Logs</span></span>
<span data-ttu-id="64e00-105">Hello **Azure-portálon** biztosít egy integrált adatfolyam naplófájl-megjelenítő, amely lehetővé teszi a nyomkövetési események megtekintéséhez a **App Service** valós idejű alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="64e00-105">hello **Azure portal** provides an integrated streaming log viewer that lets you view tracing events from your **App Service** apps in real time.</span></span>  

<span data-ttu-id="64e00-106">Ez a funkció beállítása néhány egyszerű lépést is igényel:</span><span class="sxs-lookup"><span data-stu-id="64e00-106">Setting up this feature requires a few simple steps:</span></span>

* <span data-ttu-id="64e00-107">Nyomok írja be a kódot</span><span class="sxs-lookup"><span data-stu-id="64e00-107">Write traces in your code</span></span>
* <span data-ttu-id="64e00-108">Alkalmazás engedélyezése **diagnosztikai naplók** az alkalmazásra vonatkozóan</span><span class="sxs-lookup"><span data-stu-id="64e00-108">Enable Application **Diagnostic Logs** for your app</span></span>
* <span data-ttu-id="64e00-109">Nézet hello adatfolyam a hello beépített **folyamatos átviteli naplók** hello felhasználói felülete **Azure-portálon**.</span><span class="sxs-lookup"><span data-stu-id="64e00-109">View hello stream from hello built-in **Streaming Logs** UI in hello **Azure portal**.</span></span>

### <a name="how-toowrite-traces-in-your-code"></a><span data-ttu-id="64e00-110">Hogyan toowrite követi a kódban</span><span class="sxs-lookup"><span data-stu-id="64e00-110">How toowrite traces in your code</span></span>
<span data-ttu-id="64e00-111">A kód a nyomkövetési adatokat írni az egyszerű.</span><span class="sxs-lookup"><span data-stu-id="64e00-111">Writing traces in your code is easy.</span></span>  <span data-ttu-id="64e00-112">C# nyelven lehető legkönnyebben írása hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="64e00-112">In C# it's as easy as writing hello following code:</span></span>

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

<span data-ttu-id="64e00-113">hello nyomkövetési osztály él hello System.Diagnostics névtérben.</span><span class="sxs-lookup"><span data-stu-id="64e00-113">hello Trace class lives in hello System.Diagnostics namespace.</span></span>

<span data-ttu-id="64e00-114">A node.js-alkalmazás az ezzel a kóddal írhat tooachieve hello ugyanazt az eredményt:</span><span class="sxs-lookup"><span data-stu-id="64e00-114">In a node.js app you can write this code tooachieve hello same result:</span></span>

`````````````````````````
console.log("My trace statement").
`````````````````````````

### <a name="how-tooenable-and-view-hello-streaming-logs"></a><span data-ttu-id="64e00-115">Hogyan tooenable és nézet hello folyamatos átviteli naplók</span><span class="sxs-lookup"><span data-stu-id="64e00-115">How tooenable and view hello streaming logs</span></span>
<span data-ttu-id="64e00-116">![][BrowseSitesScreenshot]Diagnosztika alapon / alkalmazás engedélyezve vannak.</span><span class="sxs-lookup"><span data-stu-id="64e00-116">![][BrowseSitesScreenshot] Diagnostics are enabled on a per app basis.</span></span> <span data-ttu-id="64e00-117">Első lépésként toohello hely tooenable szeretné ezt a beállítást, a böngészési.</span><span class="sxs-lookup"><span data-stu-id="64e00-117">Start by browsing toohello site you would like tooenable this feature on.</span></span>  

<span data-ttu-id="64e00-118">![][DiagnosticsLogs]A beállítások menü, görgessen lefelé toohello **figyelés** szakaszt, és kattintson a **(1) diagnosztikai naplók**.</span><span class="sxs-lookup"><span data-stu-id="64e00-118">![][DiagnosticsLogs] From settings menu, scroll down toohello **Monitoring** section and click on **(1) Diagnostic Logs**.</span></span> <span data-ttu-id="64e00-119">Majd **(2) engedélyezése** **Alkalmazásnaplózást (fájlrendszer)** vagy **Alkalmazásnaplózást (blob)** hello **szint** beállítást módosíthatja hello nyomkövetések toocapture súlyossági szintje.</span><span class="sxs-lookup"><span data-stu-id="64e00-119">Then **(2) enable** **Application Logging (Filesystem)** or **Application Logging (blob)** hello **Level** option lets you change hello severity level of traces toocapture.</span></span> <span data-ttu-id="64e00-120">Ha csak próbált tooget ismeri a hello szolgáltatást, állítsa be az hello szintje túl**részletes** tooensure a nyomkövetési utasítások összegyűjtött vannak.</span><span class="sxs-lookup"><span data-stu-id="64e00-120">If you're just trying tooget familiar with hello feature, set hello level too**Verbose** tooensure all of your trace statements are collected.</span></span>

<span data-ttu-id="64e00-121">Kattintson a **mentése** hello felső hello panelt, és most készen áll a tooview naplókat.</span><span class="sxs-lookup"><span data-stu-id="64e00-121">Click **SAVE** at hello top of hello blade and you're ready tooview logs.</span></span>

> [!NOTE]
> <span data-ttu-id="64e00-122">hello magasabb hello **súlyossági szint** további erőforrások felhasznált toolog és további nyomkövetések előállítása hello hello.</span><span class="sxs-lookup"><span data-stu-id="64e00-122">hello higher hello **severity level** hello more resources are consumed toolog and hello more traces are produced.</span></span> <span data-ttu-id="64e00-123">Győződjön meg arról, hogy **súlyossági szint** konfigurált toohello megfelelő részletességi éles vagy nagy forgalmú webhely van.</span><span class="sxs-lookup"><span data-stu-id="64e00-123">Make sure **severity level** is configured toohello correct verbosity for a production or high traffic site.</span></span> 
> 
> 

<span data-ttu-id="64e00-124">![][StreamingLogsScreenshot]tooview hello **a folyamatos átviteli naplók** belül hello Azure-portálon kattintson az **(1) naplófolyamot** is a hello **figyelés** hello beállítások menü részét.</span><span class="sxs-lookup"><span data-stu-id="64e00-124">![][StreamingLogsScreenshot] tooview hello **streaming logs** from within hello Azure portal, click on **(1) Log Stream** also in hello **Monitoring** section of hello settings menu.</span></span> <span data-ttu-id="64e00-125">Ha az alkalmazás nyomkövetési utasítások aktívan ír, akkor a hello kell látni őket **(2) streaming naplózza a felhasználói felület** közel valós időben.</span><span class="sxs-lookup"><span data-stu-id="64e00-125">If your app is actively writing trace statements, then you should see them in hello **(2) streaming logs UI** in near real time.</span></span>

## <a name="console"></a><span data-ttu-id="64e00-126">Konzol</span><span class="sxs-lookup"><span data-stu-id="64e00-126">Console</span></span>
<span data-ttu-id="64e00-127">Hello **Azure-portálon** hozzáférés tooyour Konzolalkalmazás biztosít.</span><span class="sxs-lookup"><span data-stu-id="64e00-127">hello **Azure portal** provides console access tooyour app.</span></span> <span data-ttu-id="64e00-128">Az alkalmazás fájlrendszer vizsgálatát, és powershell/cmd parancsfájlok futtatása.</span><span class="sxs-lookup"><span data-stu-id="64e00-128">You can explore your app's file system and run powershell/cmd scripts.</span></span> <span data-ttu-id="64e00-129">Ugyanazokkal az engedélyekkel a futó alkalmazások kódú állítható be, ha a konzol parancsok végrehajtása hello kötik.</span><span class="sxs-lookup"><span data-stu-id="64e00-129">You are bound by hello same permissions set as your running app code when executing console commands.</span></span> <span data-ttu-id="64e00-130">Hozzáférés tooprotected címtárak vagy parancsfájlok futtatását, emelt szintű jogosultságokat igénylő le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="64e00-130">Access tooprotected directories or running scripts that require elevated permissions is blocked.</span></span>  

<span data-ttu-id="64e00-131">![][ConsoleScreenshot]Beállítások menüjében, görgessen lefelé túl**Fejlesztőeszközök** szakaszt, és kattintson a **(1) konzol** és hello **(2) konzol** felhasználói felületének megnyitása toohello jobbra.</span><span class="sxs-lookup"><span data-stu-id="64e00-131">![][ConsoleScreenshot] From settings menu, scroll down too**Development Tools** section and click on **(1) Console** and hello **(2) console** UI opens toohello right.</span></span>

<span data-ttu-id="64e00-132">hello ismernie tooget **konzol**, próbálja alapvető parancsok, például:</span><span class="sxs-lookup"><span data-stu-id="64e00-132">tooget familiar with hello **console**, try basic commands like:</span></span>

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
