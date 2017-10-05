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
# <a name="streaming-logs-and-the-console"></a><span data-ttu-id="c5b71-103">Folyamatos átviteli naplók és a konzol</span><span class="sxs-lookup"><span data-stu-id="c5b71-103">Streaming Logs and the Console</span></span>
## <a name="streaming-logs"></a><span data-ttu-id="c5b71-104">Folyamatos átviteli naplók</span><span class="sxs-lookup"><span data-stu-id="c5b71-104">Streaming Logs</span></span>
<span data-ttu-id="c5b71-105">A **Azure-portálon** biztosít egy integrált adatfolyam naplófájl-megjelenítő, amely lehetővé teszi a nyomkövetési események megtekintéséhez a **App Service** valós idejű alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="c5b71-105">The **Azure portal** provides an integrated streaming log viewer that lets you view tracing events from your **App Service** apps in real time.</span></span>  

<span data-ttu-id="c5b71-106">Ez a funkció beállítása néhány egyszerű lépést is igényel:</span><span class="sxs-lookup"><span data-stu-id="c5b71-106">Setting up this feature requires a few simple steps:</span></span>

* <span data-ttu-id="c5b71-107">Nyomok írja be a kódot</span><span class="sxs-lookup"><span data-stu-id="c5b71-107">Write traces in your code</span></span>
* <span data-ttu-id="c5b71-108">Alkalmazás engedélyezése **diagnosztikai naplók** az alkalmazásra vonatkozóan</span><span class="sxs-lookup"><span data-stu-id="c5b71-108">Enable Application **Diagnostic Logs** for your app</span></span>
* <span data-ttu-id="c5b71-109">Az adatfolyam elérését a beépített megtekintése **folyamatos átviteli naplók** felhasználói felülete a **Azure-portálon**.</span><span class="sxs-lookup"><span data-stu-id="c5b71-109">View the stream from the built-in **Streaming Logs** UI in the **Azure portal**.</span></span>

### <a name="how-to-write-traces-in-your-code"></a><span data-ttu-id="c5b71-110">Nyomok írásával a kódban</span><span class="sxs-lookup"><span data-stu-id="c5b71-110">How to write traces in your code</span></span>
<span data-ttu-id="c5b71-111">A kód a nyomkövetési adatokat írni az egyszerű.</span><span class="sxs-lookup"><span data-stu-id="c5b71-111">Writing traces in your code is easy.</span></span>  <span data-ttu-id="c5b71-112">C# nyelven egyszerű módon, a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="c5b71-112">In C# it's as easy as writing the following code:</span></span>

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

<span data-ttu-id="c5b71-113">A nyomkövetési osztály él, az System.Diagnostics névtérben.</span><span class="sxs-lookup"><span data-stu-id="c5b71-113">The Trace class lives in the System.Diagnostics namespace.</span></span>

<span data-ttu-id="c5b71-114">A node.js-alkalmazás írhat a kódot, hogy az azonos eredmények elérése:</span><span class="sxs-lookup"><span data-stu-id="c5b71-114">In a node.js app you can write this code to achieve the same result:</span></span>

`````````````````````````
console.log("My trace statement").
`````````````````````````

### <a name="how-to-enable-and-view-the-streaming-logs"></a><span data-ttu-id="c5b71-115">Naplók engedélyezése és megtekintése a adatfolyamként való küldése</span><span class="sxs-lookup"><span data-stu-id="c5b71-115">How to enable and view the streaming logs</span></span>
<span data-ttu-id="c5b71-116">![][BrowseSitesScreenshot]Diagnosztika alapon / alkalmazás engedélyezve vannak.</span><span class="sxs-lookup"><span data-stu-id="c5b71-116">![][BrowseSitesScreenshot] Diagnostics are enabled on a per app basis.</span></span> <span data-ttu-id="c5b71-117">Indítsa el a Ez a funkció engedélyezése a kívánt hely tallózással.</span><span class="sxs-lookup"><span data-stu-id="c5b71-117">Start by browsing to the site you would like to enable this feature on.</span></span>  

<span data-ttu-id="c5b71-118">![][DiagnosticsLogs]A beállítások menü, görgessen le a **figyelés** szakaszt, és kattintson a **(1) diagnosztikai naplók**.</span><span class="sxs-lookup"><span data-stu-id="c5b71-118">![][DiagnosticsLogs] From settings menu, scroll down to the **Monitoring** section and click on **(1) Diagnostic Logs**.</span></span> <span data-ttu-id="c5b71-119">Majd **(2) engedélyezése** **Alkalmazásnaplózást (fájlrendszer)** vagy **Alkalmazásnaplózást (blob)** a **szint** beállítás lehetővé teszi a nyomkövetési rögzítéséhez súlyosságának módosítása.</span><span class="sxs-lookup"><span data-stu-id="c5b71-119">Then **(2) enable** **Application Logging (Filesystem)** or **Application Logging (blob)** The **Level** option lets you change the severity level of traces to capture.</span></span> <span data-ttu-id="c5b71-120">Ha csak próbál Ismerkedjen meg a szolgáltatás, a szint beállítása **részletes** annak érdekében, hogy a nyomkövetési utasítások összegyűjtött vannak.</span><span class="sxs-lookup"><span data-stu-id="c5b71-120">If you're just trying to get familiar with the feature, set the level to **Verbose** to ensure all of your trace statements are collected.</span></span>

<span data-ttu-id="c5b71-121">Kattintson a **mentése** felső részén a panelt, és készen áll a naplók megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="c5b71-121">Click **SAVE** at the top of the blade and you're ready to view logs.</span></span>

> [!NOTE]
> <span data-ttu-id="c5b71-122">A magasabb a **súlyossági szint** a több erőforrást felhasználják való bejelentkezéshez és a további nyomkövetés.</span><span class="sxs-lookup"><span data-stu-id="c5b71-122">The higher the **severity level** the more resources are consumed to log and the more traces are produced.</span></span> <span data-ttu-id="c5b71-123">Győződjön meg arról, hogy **súlyossági szint** éles vagy nagy forgalmú webhely helyes részletességi van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="c5b71-123">Make sure **severity level** is configured to the correct verbosity for a production or high traffic site.</span></span> 
> 
> 

<span data-ttu-id="c5b71-124">![][StreamingLogsScreenshot]Megtekintéséhez a **a folyamatos átviteli naplók** az Azure portálon, kattintson az **(1) naplófolyamot** is a **figyelés** a beállítások menü részét.</span><span class="sxs-lookup"><span data-stu-id="c5b71-124">![][StreamingLogsScreenshot] To view the **streaming logs** from within the Azure portal, click on **(1) Log Stream** also in the **Monitoring** section of the settings menu.</span></span> <span data-ttu-id="c5b71-125">Ha az alkalmazás nyomkövetési utasítások aktívan ír, akkor azok a a **(2) streaming naplózza a felhasználói felület** közel valós időben.</span><span class="sxs-lookup"><span data-stu-id="c5b71-125">If your app is actively writing trace statements, then you should see them in the **(2) streaming logs UI** in near real time.</span></span>

## <a name="console"></a><span data-ttu-id="c5b71-126">Konzol</span><span class="sxs-lookup"><span data-stu-id="c5b71-126">Console</span></span>
<span data-ttu-id="c5b71-127">A **Azure-portálon** konzolt az alkalmazásához való hozzáférést biztosít.</span><span class="sxs-lookup"><span data-stu-id="c5b71-127">The **Azure portal** provides console access to your app.</span></span> <span data-ttu-id="c5b71-128">Az alkalmazás fájlrendszer vizsgálatát, és powershell/cmd parancsfájlok futtatása.</span><span class="sxs-lookup"><span data-stu-id="c5b71-128">You can explore your app's file system and run powershell/cmd scripts.</span></span> <span data-ttu-id="c5b71-129">Ugyanazokkal az engedélyekkel állítja be az alkalmazás kódfuttatásra konzol parancsok végrehajtásakor kötik.</span><span class="sxs-lookup"><span data-stu-id="c5b71-129">You are bound by the same permissions set as your running app code when executing console commands.</span></span> <span data-ttu-id="c5b71-130">A hozzáférést a védett könyvtárak, vagy a parancsfájlokat emelt szintű jogosultságokat igénylő futtasson le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="c5b71-130">Access to protected directories or running scripts that require elevated permissions is blocked.</span></span>  

<span data-ttu-id="c5b71-131">![][ConsoleScreenshot]Beállítások menüjében, görgessen le a **Fejlesztőeszközök** szakaszt, és kattintson a **(1) konzol** és a **(2) konzol** felhasználói felület jobb nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="c5b71-131">![][ConsoleScreenshot] From settings menu, scroll down to **Development Tools** section and click on **(1) Console** and the **(2) console** UI opens to the right.</span></span>

<span data-ttu-id="c5b71-132">Megismerheti a **konzol**, próbálja alapvető parancsok, például:</span><span class="sxs-lookup"><span data-stu-id="c5b71-132">To get familiar with the **console**, try basic commands like:</span></span>

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
