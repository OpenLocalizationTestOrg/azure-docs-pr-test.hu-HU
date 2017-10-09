---
title: "az Azure Diagnostics felhőalapú szolgáltatások alkalmazásokban aaaTrace hello folyamata |} Microsoft Docs"
description: "Adja hozzá a nyomkövetés üzenetek tooan Azure alkalmazás toohelp hibakeresés, méri a teljesítményt, figyelés, forgalom elemzése és több."
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: 
ms.assetid: 09934772-cc07-4fd2-ba88-b224ca192f8e
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/20/2016
ms.author: robb
ms.openlocfilehash: d2ed7b5997ae1d298115b4ce593bb5051a9a0c75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="trace-hello-flow-of-a-cloud-services-application-with-azure-diagnostics"></a><span data-ttu-id="21df2-103">Az Azure Diagnostics Felhőszolgáltatások alkalmazás nyomkövetési hello folyamata</span><span class="sxs-lookup"><span data-stu-id="21df2-103">Trace hello flow of a Cloud Services application with Azure Diagnostics</span></span>
<span data-ttu-id="21df2-104">Nyomkövetés módja, toomonitor hello végrehajtásra az alkalmazás a futtatása.</span><span class="sxs-lookup"><span data-stu-id="21df2-104">Tracing is a way for you toomonitor hello execution of your application while it is running.</span></span> <span data-ttu-id="21df2-105">Használhatja a hello [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx), és [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) osztályok toorecord hibákkal kapcsolatos információkat és alkalmazás végrehajtási naplók, szövegfájlok vagy más eszközök későbbi elemzés céljából.</span><span class="sxs-lookup"><span data-stu-id="21df2-105">You can use hello [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx), and [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) classes toorecord information about errors and application execution in logs, text files, or other devices for later analysis.</span></span> <span data-ttu-id="21df2-106">További információ a nyomkövetési: [nyomkövetés és tagolása alkalmazások](https://msdn.microsoft.com/library/zs6s4h68.aspx).</span><span class="sxs-lookup"><span data-stu-id="21df2-106">For more information about tracing, see [Tracing and Instrumenting Applications](https://msdn.microsoft.com/library/zs6s4h68.aspx).</span></span>

## <a name="use-trace-statements-and-trace-switches"></a><span data-ttu-id="21df2-107">Nyomkövetési utasítások és nyomkövetési kapcsolók használata</span><span class="sxs-lookup"><span data-stu-id="21df2-107">Use trace statements and trace switches</span></span>
<span data-ttu-id="21df2-108">Hello hozzáadásával a Felhőszolgáltatások alkalmazás nyomkövetési megvalósítása [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) toohello alkalmazás konfigurációja, és hogy a tooSystem.Diagnostics.Trace vagy a System.Diagnostics.Debug meghívja a alkalmazás kódja.</span><span class="sxs-lookup"><span data-stu-id="21df2-108">Implement tracing in your Cloud Services application by adding hello [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) toohello application configuration and making calls tooSystem.Diagnostics.Trace or System.Diagnostics.Debug in your application code.</span></span> <span data-ttu-id="21df2-109">Hello konfigurációs fájlokat használjon *app.config* feldolgozói szerepköröket és hello *web.config* webes szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="21df2-109">Use hello configuration file *app.config* for worker roles and hello *web.config* for web roles.</span></span> <span data-ttu-id="21df2-110">Amikor létrehoz egy új üzemeltetett szolgáltatást, a Visual Studio-sablonnal, Azure Diagnostics automatikusan fel lesz véve toohello projektet, és a hello DiagnosticMonitorTraceListener toohello megfelelő konfigurációs fájl felvételekor hello szerepkörök ad hozzá.</span><span class="sxs-lookup"><span data-stu-id="21df2-110">When you create a new hosted service using a Visual Studio template, Azure Diagnostics is automatically added toohello project and hello DiagnosticMonitorTraceListener is added toohello appropriate configuration file for hello roles that you add.</span></span>

<span data-ttu-id="21df2-111">Információ a nyomkövetési utasítások elhelyezéséhez: [hogyan: hozzáadása nyomkövetési utasítások tooApplication kód](https://msdn.microsoft.com/library/zd83saa2.aspx).</span><span class="sxs-lookup"><span data-stu-id="21df2-111">For information on placing trace statements, see [How to: Add Trace Statements tooApplication Code](https://msdn.microsoft.com/library/zd83saa2.aspx).</span></span>

<span data-ttu-id="21df2-112">Ha kialakít [nyomkövetési kapcsolók](https://msdn.microsoft.com/library/3at424ac.aspx) nyomkövetés következik be, és hogyan kiterjedt szabályozhatja a kódban.</span><span class="sxs-lookup"><span data-stu-id="21df2-112">By placing [Trace Switches](https://msdn.microsoft.com/library/3at424ac.aspx) in your code, you can control whether tracing occurs and how extensive it is.</span></span> <span data-ttu-id="21df2-113">Ez lehetővé teszi az alkalmazás az éles környezetben hello állapotának figyelése.</span><span class="sxs-lookup"><span data-stu-id="21df2-113">This lets you monitor hello status of your application in a production environment.</span></span> <span data-ttu-id="21df2-114">Ez különösen fontos a több számítógépen futnak több összetevőt használ üzleti alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="21df2-114">This is especially important in a business application that uses multiple components running on multiple computers.</span></span> <span data-ttu-id="21df2-115">További információkért lásd: [hogyan: nyomkövetési kapcsolók konfigurálása](https://msdn.microsoft.com/library/t06xyy08.aspx).</span><span class="sxs-lookup"><span data-stu-id="21df2-115">For more information, see [How to: Configure Trace Switches](https://msdn.microsoft.com/library/t06xyy08.aspx).</span></span>

## <a name="configure-hello-trace-listener-in-an-azure-application"></a><span data-ttu-id="21df2-116">Nyomkövetés-figyelő hello konfigurálása az Azure alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="21df2-116">Configure hello trace listener in an Azure application</span></span>
<span data-ttu-id="21df2-117">Nyomkövetési, a hibakeresési és TraceSource, ehhez "figyelői" toocollect és küldött üzenetek rekord hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="21df2-117">Trace, Debug and TraceSource, require you set up "listeners" toocollect and record hello messages that are sent.</span></span> <span data-ttu-id="21df2-118">Figyelők gyűjtése, tárolására és nyomkövetés üzenetek.</span><span class="sxs-lookup"><span data-stu-id="21df2-118">Listeners collect, store, and route tracing messages.</span></span> <span data-ttu-id="21df2-119">Azok a közvetlen hello nyomkövetési kimeneti tooan megfelelő cél, például a napló, ablakot vagy szövegfájl.</span><span class="sxs-lookup"><span data-stu-id="21df2-119">They direct hello tracing output tooan appropriate target, such as a log, window, or text file.</span></span> <span data-ttu-id="21df2-120">Az Azure Diagnostics használ hello [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) osztály.</span><span class="sxs-lookup"><span data-stu-id="21df2-120">Azure Diagnostics uses hello [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) class.</span></span>

<span data-ttu-id="21df2-121">A következő eljárás hello befejezése előtt inicializálni kell hello Azure diagnosztikai figyelő.</span><span class="sxs-lookup"><span data-stu-id="21df2-121">Before you complete hello following procedure, you must initialize hello Azure diagnostic monitor.</span></span> <span data-ttu-id="21df2-122">toodo a, lásd: [diagnosztika engedélyezése a Microsoft Azure-ban](cloud-services-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="21df2-122">toodo this, see [Enabling Diagnostics in Microsoft Azure](cloud-services-dotnet-diagnostics.md).</span></span>

<span data-ttu-id="21df2-123">Vegye figyelembe, hogy a Visual Studio által biztosított hello sablonok használatakor hello konfigurációs hello figyelő kerül automatikusan meg.</span><span class="sxs-lookup"><span data-stu-id="21df2-123">Note that if you use hello templates that are provided by Visual Studio, hello configuration of hello listener is added automatically for you.</span></span>

### <a name="add-a-trace-listener"></a><span data-ttu-id="21df2-124">Adja hozzá a nyomkövetés-figyelő</span><span class="sxs-lookup"><span data-stu-id="21df2-124">Add a trace listener</span></span>
1. <span data-ttu-id="21df2-125">Nyissa meg a szerepkör az hello web.config vagy az app.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="21df2-125">Open hello web.config or app.config file for your role.</span></span>
2. <span data-ttu-id="21df2-126">Adja hozzá a következő kód toohello fájl hello.</span><span class="sxs-lookup"><span data-stu-id="21df2-126">Add hello following code toohello file.</span></span> <span data-ttu-id="21df2-127">Módosítsa a hello attribútum toouse hello verzió verziószámát hivatkozott hello szerelvény.</span><span class="sxs-lookup"><span data-stu-id="21df2-127">Change hello Version attribute toouse hello version number of hello assembly you are referencing.</span></span> <span data-ttu-id="21df2-128">hello szerelvény verziója nem feltétlenül módosíthatja az egyes Azure SDK kiadások kivéve, ha nincsenek frissítések tooit.</span><span class="sxs-lookup"><span data-stu-id="21df2-128">hello assembly version does not necessarily change with each Azure SDK release unless there are updates tooit.</span></span>
   
    ```
    <system.diagnostics>
        <trace>
            <listeners>
                <add type="Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener,
                  Microsoft.WindowsAzure.Diagnostics,
                  Version=2.8.0.0,
                  Culture=neutral,
                  PublicKeyToken=31bf3856ad364e35"
                  name="AzureDiagnostics">
                    <filter type="" />
                </add>
            </listeners>
        </trace>
    </system.diagnostics>
    ```
   > [!IMPORTANT]
   > <span data-ttu-id="21df2-129">Győződjön meg arról, hogy a projekt hivatkozás toohello Microsoft.WindowsAzure.Diagnostics szerelvény.</span><span class="sxs-lookup"><span data-stu-id="21df2-129">Make sure you have a project reference toohello Microsoft.WindowsAzure.Diagnostics assembly.</span></span> <span data-ttu-id="21df2-130">Frissítés hello verziószáma hello xml hello toomatch hello verziója újabb Microsoft.WindowsAzure.Diagnostics szerelvény hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="21df2-130">Update hello version number in hello xml above toomatch hello version of hello referenced Microsoft.WindowsAzure.Diagnostics assembly.</span></span>
   > 
   > 
3. <span data-ttu-id="21df2-131">Mentse a hello konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="21df2-131">Save hello config file.</span></span>

<span data-ttu-id="21df2-132">Figyelők kapcsolatos további információkért lásd: [nyomkövetésfigyelők](https://msdn.microsoft.com/library/4y5y10s7.aspx).</span><span class="sxs-lookup"><span data-stu-id="21df2-132">For more information about listeners, see [Trace Listeners](https://msdn.microsoft.com/library/4y5y10s7.aspx).</span></span>

<span data-ttu-id="21df2-133">Hello lépéseket tooadd hello figyelő befejezése után a nyomkövetési utasítások tooyour kód is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="21df2-133">After you complete hello steps tooadd hello listener, you can add trace statements tooyour code.</span></span>

### <a name="tooadd-trace-statement-tooyour-code"></a><span data-ttu-id="21df2-134">tooadd nyomkövetési utasítás tooyour kód</span><span class="sxs-lookup"><span data-stu-id="21df2-134">tooadd trace statement tooyour code</span></span>
1. <span data-ttu-id="21df2-135">Az alkalmazás a forrásfájl megnyitásakor.</span><span class="sxs-lookup"><span data-stu-id="21df2-135">Open a source file for your application.</span></span> <span data-ttu-id="21df2-136">Például hello <RoleName>hello feldolgozói szerepkör és a webes szerepkör .cs fájlban.</span><span class="sxs-lookup"><span data-stu-id="21df2-136">For example, hello <RoleName>.cs file for hello worker role or web role.</span></span>
2. <span data-ttu-id="21df2-137">Adja hozzá a következő hello utasítás használatával, ha már nincs hozzáadva:</span><span class="sxs-lookup"><span data-stu-id="21df2-137">Add hello following using statement if it has not already been added:</span></span>
    ```
        using System.Diagnostics;
    ```
3. <span data-ttu-id="21df2-138">Nyomkövetési utasítások, ahol az alkalmazás állapota hello toocapture információt szeretne hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="21df2-138">Add Trace statements where you want toocapture information about hello state of your application.</span></span> <span data-ttu-id="21df2-139">Számos különféle módszereket tooformat hello kimenete hello nyomkövetési utasítás használható.</span><span class="sxs-lookup"><span data-stu-id="21df2-139">You can use a variety of methods tooformat hello output of hello Trace statement.</span></span> <span data-ttu-id="21df2-140">További információkért lásd: [hogyan: hozzáadása nyomkövetési utasítások tooApplication kód](https://msdn.microsoft.com/library/zd83saa2.aspx).</span><span class="sxs-lookup"><span data-stu-id="21df2-140">For more information, see [How to: Add Trace Statements tooApplication Code](https://msdn.microsoft.com/library/zd83saa2.aspx).</span></span>
4. <span data-ttu-id="21df2-141">Mentse a hello forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="21df2-141">Save hello source file.</span></span>

