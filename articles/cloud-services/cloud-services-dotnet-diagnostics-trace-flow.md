---
title: "A folyamat a egy felhőalapú szolgáltatások alkalmazást az Azure diagnosztikai nyomkövetési |} Microsoft Docs"
description: "Nyomkövetési üzenet hozzáadása érdekében a hibakeresés méri a teljesítményt, figyelés, forgalom elemzése és további Azure-alkalmazásfejlesztő."
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
ms.openlocfilehash: 35b4a4270846c54a1ca760e803ef7adba60cf03b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="trace-the-flow-of-a-cloud-services-application-with-azure-diagnostics"></a><span data-ttu-id="de508-103">A folyamatot az Azure Diagnostics Felhőszolgáltatások alkalmazás nyomkövetési</span><span class="sxs-lookup"><span data-stu-id="de508-103">Trace the flow of a Cloud Services application with Azure Diagnostics</span></span>
<span data-ttu-id="de508-104">Nyomkövetés módja a figyelheti a végrehajtása az alkalmazás futtatása során.</span><span class="sxs-lookup"><span data-stu-id="de508-104">Tracing is a way for you to monitor the execution of your application while it is running.</span></span> <span data-ttu-id="de508-105">Használhatja a [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx), és [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) hibák adatainak rögzítésére osztályok és alkalmazás végrehajtási naplók, szövegfájlok vagy más eszközök későbbi elemzés céljából.</span><span class="sxs-lookup"><span data-stu-id="de508-105">You can use the [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx), and [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) classes to record information about errors and application execution in logs, text files, or other devices for later analysis.</span></span> <span data-ttu-id="de508-106">További információ a nyomkövetési: [nyomkövetés és tagolása alkalmazások](https://msdn.microsoft.com/library/zs6s4h68.aspx).</span><span class="sxs-lookup"><span data-stu-id="de508-106">For more information about tracing, see [Tracing and Instrumenting Applications](https://msdn.microsoft.com/library/zs6s4h68.aspx).</span></span>

## <a name="use-trace-statements-and-trace-switches"></a><span data-ttu-id="de508-107">Nyomkövetési utasítások és nyomkövetési kapcsolók használata</span><span class="sxs-lookup"><span data-stu-id="de508-107">Use trace statements and trace switches</span></span>
<span data-ttu-id="de508-108">Adja hozzá a Felhőszolgáltatások alkalmazás nyomkövetési megvalósítása a [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) az alkalmazás konfigurációját, és a System.Diagnostics.Trace vagy System.Diagnostics.Debug a hívások a alkalmazás kódja.</span><span class="sxs-lookup"><span data-stu-id="de508-108">Implement tracing in your Cloud Services application by adding the [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) to the application configuration and making calls to System.Diagnostics.Trace or System.Diagnostics.Debug in your application code.</span></span> <span data-ttu-id="de508-109">A konfigurációs fájl *app.config* feldolgozói szerepkörök és a *web.config* webes szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="de508-109">Use the configuration file *app.config* for worker roles and the *web.config* for web roles.</span></span> <span data-ttu-id="de508-110">Amikor létrehoz egy új üzemeltetett szolgáltatást, a Visual Studio-sablonnal, Azure Diagnostics automatikusan hozzáadódik a projektet, és a DiagnosticMonitorTraceListener hozzáadódik a megfelelő konfigurációs fájlt ad hozzá a szerepkörök számára.</span><span class="sxs-lookup"><span data-stu-id="de508-110">When you create a new hosted service using a Visual Studio template, Azure Diagnostics is automatically added to the project and the DiagnosticMonitorTraceListener is added to the appropriate configuration file for the roles that you add.</span></span>

<span data-ttu-id="de508-111">Információ a nyomkövetési utasítások elhelyezéséhez: [hogyan: nyomkövetési utasítások adja hozzá az alkalmazás kódjában](https://msdn.microsoft.com/library/zd83saa2.aspx).</span><span class="sxs-lookup"><span data-stu-id="de508-111">For information on placing trace statements, see [How to: Add Trace Statements to Application Code](https://msdn.microsoft.com/library/zd83saa2.aspx).</span></span>

<span data-ttu-id="de508-112">Ha kialakít [nyomkövetési kapcsolók](https://msdn.microsoft.com/library/3at424ac.aspx) nyomkövetés következik be, és hogyan kiterjedt szabályozhatja a kódban.</span><span class="sxs-lookup"><span data-stu-id="de508-112">By placing [Trace Switches](https://msdn.microsoft.com/library/3at424ac.aspx) in your code, you can control whether tracing occurs and how extensive it is.</span></span> <span data-ttu-id="de508-113">Ez lehetővé teszi az éles környezetben az alkalmazás állapotának figyelése.</span><span class="sxs-lookup"><span data-stu-id="de508-113">This lets you monitor the status of your application in a production environment.</span></span> <span data-ttu-id="de508-114">Ez különösen fontos a több számítógépen futnak több összetevőt használ üzleti alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="de508-114">This is especially important in a business application that uses multiple components running on multiple computers.</span></span> <span data-ttu-id="de508-115">További információkért lásd: [hogyan: nyomkövetési kapcsolók konfigurálása](https://msdn.microsoft.com/library/t06xyy08.aspx).</span><span class="sxs-lookup"><span data-stu-id="de508-115">For more information, see [How to: Configure Trace Switches](https://msdn.microsoft.com/library/t06xyy08.aspx).</span></span>

## <a name="configure-the-trace-listener-in-an-azure-application"></a><span data-ttu-id="de508-116">A nyomkövetés-figyelő konfigurálása az Azure alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="de508-116">Configure the trace listener in an Azure application</span></span>
<span data-ttu-id="de508-117">Nyomkövetési, a hibakeresési és TraceSource, ehhez állítsa be "figyelői" összegyűjtésére, és jegyezze fel a küldött üzenetek.</span><span class="sxs-lookup"><span data-stu-id="de508-117">Trace, Debug and TraceSource, require you set up "listeners" to collect and record the messages that are sent.</span></span> <span data-ttu-id="de508-118">Figyelők gyűjtése, tárolására és nyomkövetés üzenetek.</span><span class="sxs-lookup"><span data-stu-id="de508-118">Listeners collect, store, and route tracing messages.</span></span> <span data-ttu-id="de508-119">A nyomkövetési kimeneti egy megfelelő célhoz, például a napló, ablakot vagy szövegfájl közvetlen azokat.</span><span class="sxs-lookup"><span data-stu-id="de508-119">They direct the tracing output to an appropriate target, such as a log, window, or text file.</span></span> <span data-ttu-id="de508-120">Az Azure Diagnostics használja a [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) osztály.</span><span class="sxs-lookup"><span data-stu-id="de508-120">Azure Diagnostics uses the [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) class.</span></span>

<span data-ttu-id="de508-121">A következő eljárás végrehajtása előtt inicializálni kell az Azure diagnosztikai figyelő.</span><span class="sxs-lookup"><span data-stu-id="de508-121">Before you complete the following procedure, you must initialize the Azure diagnostic monitor.</span></span> <span data-ttu-id="de508-122">Ehhez tekintse meg a [diagnosztika engedélyezése a Microsoft Azure-ban](cloud-services-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="de508-122">To do this, see [Enabling Diagnostics in Microsoft Azure](cloud-services-dotnet-diagnostics.md).</span></span>

<span data-ttu-id="de508-123">Vegye figyelembe, hogy ha a Visual Studio által biztosított sablonokat használ, a figyelő a konfigurációs kerül automatikusan meg.</span><span class="sxs-lookup"><span data-stu-id="de508-123">Note that if you use the templates that are provided by Visual Studio, the configuration of the listener is added automatically for you.</span></span>

### <a name="add-a-trace-listener"></a><span data-ttu-id="de508-124">Adja hozzá a nyomkövetés-figyelő</span><span class="sxs-lookup"><span data-stu-id="de508-124">Add a trace listener</span></span>
1. <span data-ttu-id="de508-125">Nyissa meg az adott szerepkörhöz a web.config vagy az app.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="de508-125">Open the web.config or app.config file for your role.</span></span>
2. <span data-ttu-id="de508-126">Adja hozzá a következő kódot a fájlt.</span><span class="sxs-lookup"><span data-stu-id="de508-126">Add the following code to the file.</span></span> <span data-ttu-id="de508-127">Módosítja annak verzióattribútumát, a hivatkozott szerelvény verziószáma használatára.</span><span class="sxs-lookup"><span data-stu-id="de508-127">Change the Version attribute to use the version number of the assembly you are referencing.</span></span> <span data-ttu-id="de508-128">A szerelvény verziója nem feltétlenül módosíthatja az egyes Azure SDK kiadások kivéve, ha frissítések érkeztek azt.</span><span class="sxs-lookup"><span data-stu-id="de508-128">The assembly version does not necessarily change with each Azure SDK release unless there are updates to it.</span></span>
   
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
   > <span data-ttu-id="de508-129">Győződjön meg arról, hogy a projektbe egy hivatkozást a Microsoft.WindowsAzure.Diagnostics szerelvényre.</span><span class="sxs-lookup"><span data-stu-id="de508-129">Make sure you have a project reference to the Microsoft.WindowsAzure.Diagnostics assembly.</span></span> <span data-ttu-id="de508-130">A verziószám, a fenti XML-kódban a hivatkozott Microsoft.WindowsAzure.Diagnostics szerelvény verziójának megfelelő frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="de508-130">Update the version number in the xml above to match the version of the referenced Microsoft.WindowsAzure.Diagnostics assembly.</span></span>
   > 
   > 
3. <span data-ttu-id="de508-131">Mentse a konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="de508-131">Save the config file.</span></span>

<span data-ttu-id="de508-132">Figyelők kapcsolatos további információkért lásd: [nyomkövetésfigyelők](https://msdn.microsoft.com/library/4y5y10s7.aspx).</span><span class="sxs-lookup"><span data-stu-id="de508-132">For more information about listeners, see [Trace Listeners](https://msdn.microsoft.com/library/4y5y10s7.aspx).</span></span>

<span data-ttu-id="de508-133">Miután elvégezte a figyelő felvételének lépéseiről, adhat hozzá nyomkövetési utasítások a kódot.</span><span class="sxs-lookup"><span data-stu-id="de508-133">After you complete the steps to add the listener, you can add trace statements to your code.</span></span>

### <a name="to-add-trace-statement-to-your-code"></a><span data-ttu-id="de508-134">A kód nyomkövetési utasítás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="de508-134">To add trace statement to your code</span></span>
1. <span data-ttu-id="de508-135">Az alkalmazás a forrásfájl megnyitásakor.</span><span class="sxs-lookup"><span data-stu-id="de508-135">Open a source file for your application.</span></span> <span data-ttu-id="de508-136">Például a <RoleName>.cs fájlt a feldolgozói szerepkör, illetve a webes szerepkör.</span><span class="sxs-lookup"><span data-stu-id="de508-136">For example, the <RoleName>.cs file for the worker role or web role.</span></span>
2. <span data-ttu-id="de508-137">Adja hozzá a következő using utasítást, ha már nincs hozzáadva:</span><span class="sxs-lookup"><span data-stu-id="de508-137">Add the following using statement if it has not already been added:</span></span>
    ```
        using System.Diagnostics;
    ```
3. <span data-ttu-id="de508-138">Adja hozzá a nyomkövetési utasítások hol kívánja rögzíteni az alkalmazás állapotára vonatkozó adatokat.</span><span class="sxs-lookup"><span data-stu-id="de508-138">Add Trace statements where you want to capture information about the state of your application.</span></span> <span data-ttu-id="de508-139">Számos módszer segítségével formázza a nyomkövetési utasítás kimenete.</span><span class="sxs-lookup"><span data-stu-id="de508-139">You can use a variety of methods to format the output of the Trace statement.</span></span> <span data-ttu-id="de508-140">További információkért lásd: [hogyan: nyomkövetési utasítások adja hozzá az alkalmazás kódjában](https://msdn.microsoft.com/library/zd83saa2.aspx).</span><span class="sxs-lookup"><span data-stu-id="de508-140">For more information, see [How to: Add Trace Statements to Application Code](https://msdn.microsoft.com/library/zd83saa2.aspx).</span></span>
4. <span data-ttu-id="de508-141">A forrás-fájl mentése.</span><span class="sxs-lookup"><span data-stu-id="de508-141">Save the source file.</span></span>

