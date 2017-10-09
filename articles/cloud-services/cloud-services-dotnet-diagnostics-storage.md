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
# <a name="store-and-view-diagnostic-data-in-azure-storage"></a><span data-ttu-id="f1e20-103">Az Azure Storage diagnosztikai adatok tárolása és nézet</span><span class="sxs-lookup"><span data-stu-id="f1e20-103">Store and view diagnostic data in Azure Storage</span></span>
<span data-ttu-id="f1e20-104">Diagnosztikai adatok nem tárolódik véglegesen, kivéve, ha azt át toohello Microsoft Azure storage emulator vagy tooAzure tárolására.</span><span class="sxs-lookup"><span data-stu-id="f1e20-104">Diagnostic data is not permanently stored unless you transfer it toohello Microsoft Azure storage emulator or tooAzure storage.</span></span> <span data-ttu-id="f1e20-105">Egyszer, megőrzés is megtekinthető számos elérhető eszköz egyike.</span><span class="sxs-lookup"><span data-stu-id="f1e20-105">Once in storage, it can be viewed with one of several available tools.</span></span>

## <a name="specify-a-storage-account"></a><span data-ttu-id="f1e20-106">Adja meg a storage-fiók</span><span class="sxs-lookup"><span data-stu-id="f1e20-106">Specify a storage account</span></span>
<span data-ttu-id="f1e20-107">Megadja, hogy szeretné-e a hello ServiceConfiguration.cscfg fájlban toouse hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="f1e20-107">You specify hello storage account that you want toouse in hello ServiceConfiguration.cscfg file.</span></span> <span data-ttu-id="f1e20-108">hello fiókadatok egy kapcsolati karakterláncot egy konfigurációs beállítás típusúként van definiálva.</span><span class="sxs-lookup"><span data-stu-id="f1e20-108">hello account information is defined as a connection string in a configuration setting.</span></span> <span data-ttu-id="f1e20-109">hello alábbi példa bemutatja hello alapértelmezett kapcsolati karakterlánc létrehozása a Visual Studio új Cloud Service projektek:</span><span class="sxs-lookup"><span data-stu-id="f1e20-109">hello following example shows hello default connection string created for a new Cloud Service project in  Visual Studio:</span></span>

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

<span data-ttu-id="f1e20-110">Módosíthatja a kapcsolati karakterlánc tooprovide fiókadatai Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="f1e20-110">You can change this connection string tooprovide account information for an Azure storage account.</span></span>

<span data-ttu-id="f1e20-111">Hello gyűjtött diagnosztikai adatok típusától függően a Azure Diagnostics hello Blob szolgáltatás vagy hello Table szolgáltatás használja.</span><span class="sxs-lookup"><span data-stu-id="f1e20-111">Depending on hello type of diagnostic data that is being collected, Azure Diagnostics uses either hello Blob service or hello Table service.</span></span> <span data-ttu-id="f1e20-112">hello következő táblázatban megmaradnak hello adatforrások és a formátuma.</span><span class="sxs-lookup"><span data-stu-id="f1e20-112">hello following table shows hello data sources that are persisted and their format.</span></span>

| <span data-ttu-id="f1e20-113">Adatforrás</span><span class="sxs-lookup"><span data-stu-id="f1e20-113">Data source</span></span> | <span data-ttu-id="f1e20-114">Tárolási formátum</span><span class="sxs-lookup"><span data-stu-id="f1e20-114">Storage format</span></span> |
| --- | --- |
| <span data-ttu-id="f1e20-115">Az Azure naplói</span><span class="sxs-lookup"><span data-stu-id="f1e20-115">Azure logs</span></span> |<span data-ttu-id="f1e20-116">Tábla</span><span class="sxs-lookup"><span data-stu-id="f1e20-116">Table</span></span> |
| <span data-ttu-id="f1e20-117">Az IIS 7.0-naplók</span><span class="sxs-lookup"><span data-stu-id="f1e20-117">IIS 7.0 logs</span></span> |<span data-ttu-id="f1e20-118">Blob</span><span class="sxs-lookup"><span data-stu-id="f1e20-118">Blob</span></span> |
| <span data-ttu-id="f1e20-119">Az Azure Diagnostics infrastruktúra naplók</span><span class="sxs-lookup"><span data-stu-id="f1e20-119">Azure Diagnostics infrastructure logs</span></span> |<span data-ttu-id="f1e20-120">Tábla</span><span class="sxs-lookup"><span data-stu-id="f1e20-120">Table</span></span> |
| <span data-ttu-id="f1e20-121">Sikertelen kérelmek nyomkövetési naplóit</span><span class="sxs-lookup"><span data-stu-id="f1e20-121">Failed Request Trace logs</span></span> |<span data-ttu-id="f1e20-122">Blob</span><span class="sxs-lookup"><span data-stu-id="f1e20-122">Blob</span></span> |
| <span data-ttu-id="f1e20-123">Windows-Eseménynapló</span><span class="sxs-lookup"><span data-stu-id="f1e20-123">Windows Event logs</span></span> |<span data-ttu-id="f1e20-124">Tábla</span><span class="sxs-lookup"><span data-stu-id="f1e20-124">Table</span></span> |
| <span data-ttu-id="f1e20-125">Teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="f1e20-125">Performance counters</span></span> |<span data-ttu-id="f1e20-126">Tábla</span><span class="sxs-lookup"><span data-stu-id="f1e20-126">Table</span></span> |
| <span data-ttu-id="f1e20-127">Összeomlási memóriaképek</span><span class="sxs-lookup"><span data-stu-id="f1e20-127">Crash dumps</span></span> |<span data-ttu-id="f1e20-128">Blob</span><span class="sxs-lookup"><span data-stu-id="f1e20-128">Blob</span></span> |
| <span data-ttu-id="f1e20-129">Egyéni hibanaplókat</span><span class="sxs-lookup"><span data-stu-id="f1e20-129">Custom error logs</span></span> |<span data-ttu-id="f1e20-130">Blob</span><span class="sxs-lookup"><span data-stu-id="f1e20-130">Blob</span></span> |

## <a name="transfer-diagnostic-data"></a><span data-ttu-id="f1e20-131">Diagnosztikai adatok átvitele</span><span class="sxs-lookup"><span data-stu-id="f1e20-131">Transfer diagnostic data</span></span>
<span data-ttu-id="f1e20-132">Az SDK 2.5-ös vagy újabb hello kérelem tootransfer diagnosztikai adatok akkor fordulhat elő, hello konfigurációs fájl segítségével.</span><span class="sxs-lookup"><span data-stu-id="f1e20-132">For SDK 2.5 and later, hello request tootransfer diagnostic data can occur through hello configuration file.</span></span> <span data-ttu-id="f1e20-133">Diagnosztikai adatok vihetők át a hello konfigurációs rendszeres időközönként.</span><span class="sxs-lookup"><span data-stu-id="f1e20-133">You can transfer diagnostic data at scheduled intervals as specified in hello configuration.</span></span>

<span data-ttu-id="f1e20-134">Az SDK 2.4 és az előző kérhet tootransfer hello diagnosztikai adatok keresztül hello konfigurációs fájlba is, programozott módon.</span><span class="sxs-lookup"><span data-stu-id="f1e20-134">For SDK 2.4 and previous you can request tootransfer hello diagnostic data through hello configuration file as well as programmatically.</span></span> <span data-ttu-id="f1e20-135">hello programozott módszert is lehetővé teszi toodo igény átvitel.</span><span class="sxs-lookup"><span data-stu-id="f1e20-135">hello programmatic approach also allows you toodo on-demand transfers.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f1e20-136">Diagnosztikai adatok tooan Azure storage-fiók átvitel során fel Önnek a diagnosztikai adatok által használt hello tárerőforrásokhoz költségek.</span><span class="sxs-lookup"><span data-stu-id="f1e20-136">When you transfer diagnostic data tooan Azure storage account, you incur costs for hello storage resources that your diagnostic data uses.</span></span>
> 
> 

## <a name="store-diagnostic-data"></a><span data-ttu-id="f1e20-137">Diagnosztikai adatok tárolásához</span><span class="sxs-lookup"><span data-stu-id="f1e20-137">Store diagnostic data</span></span>
<span data-ttu-id="f1e20-138">Naplóadatok tárolt Blob vagy a tábla a következő neveket hello:</span><span class="sxs-lookup"><span data-stu-id="f1e20-138">Log data is stored in either Blob or Table storage with hello following names:</span></span>

<span data-ttu-id="f1e20-139">**Táblák**</span><span class="sxs-lookup"><span data-stu-id="f1e20-139">**Tables**</span></span>

* <span data-ttu-id="f1e20-140">**WadLogsTable** - kód megadása hello nyomkövetés-figyelő írt naplókat.</span><span class="sxs-lookup"><span data-stu-id="f1e20-140">**WadLogsTable** - Logs written in code using hello trace listener.</span></span>
* <span data-ttu-id="f1e20-141">**WADDiagnosticInfrastructureLogsTable** -diagnosztikai figyelő és a konfigurációs módosításokat.</span><span class="sxs-lookup"><span data-stu-id="f1e20-141">**WADDiagnosticInfrastructureLogsTable** - Diagnostic monitor and configuration changes.</span></span>
* <span data-ttu-id="f1e20-142">**WADDirectoriesTable** – könyvtárak adott hello diagnosztikai figyelő által figyelt.</span><span class="sxs-lookup"><span data-stu-id="f1e20-142">**WADDirectoriesTable** – Directories that hello diagnostic monitor is monitoring.</span></span>  <span data-ttu-id="f1e20-143">Ez magában foglalja az IIS-napló, az IIS nem sikerült a kérelem naplókat, és egyéni könyvtárak.</span><span class="sxs-lookup"><span data-stu-id="f1e20-143">This includes IIS logs, IIS failed request logs, and custom directories.</span></span>  <span data-ttu-id="f1e20-144">hello blob naplófájl helye hello hello tároló mezőben megadott hello név pedig hello BLOB hello RelativePath mezőben.</span><span class="sxs-lookup"><span data-stu-id="f1e20-144">hello location of hello blob log file is specified in hello Container field and hello name of hello blob is in hello RelativePath field.</span></span>  <span data-ttu-id="f1e20-145">adott hello Azure virtuális gép verzióját hello AbsolutePath mező hello helyét és hello fájl nevét jelzi.</span><span class="sxs-lookup"><span data-stu-id="f1e20-145">hello AbsolutePath field indicates hello location and name of hello file as it existed on hello Azure virtual machine.</span></span>
* <span data-ttu-id="f1e20-146">**WADPerformanceCountersTable** – teljesítményszámlálók.</span><span class="sxs-lookup"><span data-stu-id="f1e20-146">**WADPerformanceCountersTable** – Performance counters.</span></span>
* <span data-ttu-id="f1e20-147">**WADWindowsEventLogsTable** – Windows-eseményt naplózza.</span><span class="sxs-lookup"><span data-stu-id="f1e20-147">**WADWindowsEventLogsTable** – Windows Event logs.</span></span>

<span data-ttu-id="f1e20-148">**Blobok**</span><span class="sxs-lookup"><span data-stu-id="f1e20-148">**Blobs**</span></span>

* <span data-ttu-id="f1e20-149">**üvegvatta-vezérlő-tároló** – (csak SDK 2.4 és előző) hello Azure diagnostics meghatározza hello XML konfigurációs fájlokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f1e20-149">**wad-control-container** – (Only for SDK 2.4 and previous) Contains hello XML configuration files that controls hello Azure diagnostics .</span></span>
* <span data-ttu-id="f1e20-150">**üvegvatta-az iis-failedreqlogfiles** – IIS sikertelen kérelem naplók adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f1e20-150">**wad-iis-failedreqlogfiles** – Contains information from IIS Failed Request logs.</span></span>
* <span data-ttu-id="f1e20-151">**üvegvatta-az iis-naplófájlok** – IIS-napló adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f1e20-151">**wad-iis-logfiles** – Contains information about IIS logs.</span></span>
* <span data-ttu-id="f1e20-152">**"egyéni"** – egy egyéni tároló alapján hello diagnosztikai figyelő által figyelt könyvtárak beállítása.</span><span class="sxs-lookup"><span data-stu-id="f1e20-152">**"custom"** – A custom container based on configuring directories that are monitored by hello diagnostic monitor.</span></span>  <span data-ttu-id="f1e20-153">a blob tároló hello nevét fogja megadni WADDirectoriesTable.</span><span class="sxs-lookup"><span data-stu-id="f1e20-153">hello name of this blob container will be specified in WADDirectoriesTable.</span></span>

## <a name="tools-tooview-diagnostic-data"></a><span data-ttu-id="f1e20-154">Eszközök tooview diagnosztikai adatok</span><span class="sxs-lookup"><span data-stu-id="f1e20-154">Tools tooview diagnostic data</span></span>
<span data-ttu-id="f1e20-155">Számos eszközt olyan elérhető tooview hello adatok, miután átvitt toostorage.</span><span class="sxs-lookup"><span data-stu-id="f1e20-155">Several tools are available tooview hello data after it is transferred toostorage.</span></span> <span data-ttu-id="f1e20-156">Példa:</span><span class="sxs-lookup"><span data-stu-id="f1e20-156">For example:</span></span>

* <span data-ttu-id="f1e20-157">A Visual Studio - Server Explorer Ha telepítve van a hello Azure eszközök a Microsoft Visual Studio, használhatja hello Azure Storage-csomópont a Server Explorer tooview csak olvasható blob és table adatokat az Azure storage-fiókok.</span><span class="sxs-lookup"><span data-stu-id="f1e20-157">Server Explorer in Visual Studio - If you have installed hello Azure Tools for Microsoft Visual Studio, you can use hello Azure Storage node in Server Explorer tooview read-only blob and table data from your Azure storage accounts.</span></span> <span data-ttu-id="f1e20-158">A helyi storage emulator fiókhoz tartozó megjeleníthető adatok, és is a storage-fiókok hozott létre az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="f1e20-158">You can display data from your local storage emulator account and also from storage accounts you have created for Azure.</span></span> <span data-ttu-id="f1e20-159">További információkért lásd: [böngészés és tárolási erőforrások kezelése a Server Explorer](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).</span><span class="sxs-lookup"><span data-stu-id="f1e20-159">For more information, see [Browsing and Managing Storage Resources with Server Explorer](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).</span></span>
* <span data-ttu-id="f1e20-160">[A Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy különálló alkalmazás, amely lehetővé teszi az Azure Storage-adatokkal Windows, OSX és Linux tooeasily használata.</span><span class="sxs-lookup"><span data-stu-id="f1e20-160">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a standalone app that enables you tooeasily work with Azure Storage data on Windows, OSX, and Linux.</span></span>
* <span data-ttu-id="f1e20-161">[Az Azure Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) tartalmazza az Azure Diagnostics Managert, amely lehetővé teszi a tooview, töltse le és kezelése az Azure-on futó hello alkalmazások által gyűjtött hello diagnosztikai adatokat.</span><span class="sxs-lookup"><span data-stu-id="f1e20-161">[Azure Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) includes Azure Diagnostics Manager which allows you tooview, download and manage hello diagnostics data collected by hello applications running on Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f1e20-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f1e20-162">Next Steps</span></span>
[<span data-ttu-id="f1e20-163">Cloud Services-alkalmazásokban az Azure diagnosztikai nyomkövetési hello folyamata</span><span class="sxs-lookup"><span data-stu-id="f1e20-163">Trace hello flow in a Cloud Services application with Azure Diagnostics</span></span>](cloud-services-dotnet-diagnostics-trace-flow.md)

