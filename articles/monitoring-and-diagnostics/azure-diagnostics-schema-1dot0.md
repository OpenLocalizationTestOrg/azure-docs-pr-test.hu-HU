---
title: "Diagnosztika 1.0 konfigurációs séma aaaAzure |} Microsoft Docs"
description: "CSAK akkor érvényes, ha az Azure SDK 2.4 használ és az alacsonyabb Azure virtuális gépek, a virtuálisgép-méretezési csoportok, a Service Fabric vagy a Cloud Services."
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/15/2017
ms.author: robb
ms.openlocfilehash: bdd2b26217d6ea28f19e651ab429e7e7401ff57b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-diagnostics-10-configuration-schema"></a><span data-ttu-id="f00f1-103">Az Azure Diagnostics 1.0 konfigurációs séma</span><span class="sxs-lookup"><span data-stu-id="f00f1-103">Azure Diagnostics 1.0 Configuration Schema</span></span>
> [!NOTE]
> <span data-ttu-id="f00f1-104">Az Azure Diagnostics hello használt toocollect teljesítményszámlálók és más Azure virtuális gépek, a virtuálisgép-méretezési csoportok, a Service Fabric és a Felhőszolgáltatások statisztikáit.</span><span class="sxs-lookup"><span data-stu-id="f00f1-104">Azure Diagnostics is hello component used toocollect performance counters and other statistics from Azure Virtual Machines, Virtual Machine Scale Sets, Service Fabric, and Cloud Services.</span></span>  <span data-ttu-id="f00f1-105">Ezen a lapon csak fontos, ha ilyen szolgáltatást használ.</span><span class="sxs-lookup"><span data-stu-id="f00f1-105">This page is only relevant if you are using one of these services.</span></span>
>

<span data-ttu-id="f00f1-106">Az Azure Diagnostics más Microsoft-diagnosztika termékekkel, például Azure figyelő, az Application Insights és Naplóelemzési szolgál.</span><span class="sxs-lookup"><span data-stu-id="f00f1-106">Azure Diagnostics is used with other Microsoft diagnostics products like Azure Monitor, Application Insights, and Log Analytics.</span></span>

<span data-ttu-id="f00f1-107">hello Azure Diagnostics konfigurációs fájl használt tooinitialize hello diagnosztikai figyelő értékeket határozza meg.</span><span class="sxs-lookup"><span data-stu-id="f00f1-107">hello Azure Diagnostics configuration file defines values that are used tooinitialize hello Diagnostics Monitor.</span></span> <span data-ttu-id="f00f1-108">A fájl használt tooinitialize diagnosztikai konfigurációs beállítások, amikor a hello diagnosztika indítása.</span><span class="sxs-lookup"><span data-stu-id="f00f1-108">This file is used tooinitialize diagnostic configuration settings when hello diagnostics monitor starts.</span></span>  

 <span data-ttu-id="f00f1-109">Alapértelmezés szerint hello Azure Diagnostics konfigurációs sémafájl telepített toohello `C:\Program Files\Microsoft SDKs\Azure\.NET SDK\<version>\schemas` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="f00f1-109">By default, hello Azure Diagnostics configuration schema file is installed toohello `C:\Program Files\Microsoft SDKs\Azure\.NET SDK\<version>\schemas` directory.</span></span> <span data-ttu-id="f00f1-110">Cserélje le `<version>` hello verziójával telepített hello [Azure SDK](http://www.windowsazure.com/develop/downloads/).</span><span class="sxs-lookup"><span data-stu-id="f00f1-110">Replace `<version>` with hello installed version of hello [Azure SDK](http://www.windowsazure.com/develop/downloads/).</span></span>  

> [!NOTE]
>  <span data-ttu-id="f00f1-111">hello diagnosztika konfigurációs fájl főként a a korábbi hello indítási folyamat gyűjtött diagnosztikai adatok toobe igénylő feladatok indítása.</span><span class="sxs-lookup"><span data-stu-id="f00f1-111">hello diagnostics configuration file is typically used with startup tasks that require diagnostic data toobe collected earlier in hello startup process.</span></span> <span data-ttu-id="f00f1-112">Azure Diagnostics használatával kapcsolatos további információkért lásd: [naplózási adatok gyűjtése Azure Diagnostics használatával](assetId:///83a91c23-5ca2-4fc9-8df3-62036c37a3d7).</span><span class="sxs-lookup"><span data-stu-id="f00f1-112">For more information about using Azure Diagnostics, see [Collect Logging Data by Using Azure Diagnostics](assetId:///83a91c23-5ca2-4fc9-8df3-62036c37a3d7).</span></span>  

## <a name="example-of-hello-diagnostics-configuration-file"></a><span data-ttu-id="f00f1-113">Hello diagnosztika konfigurációs fájl példa</span><span class="sxs-lookup"><span data-stu-id="f00f1-113">Example of hello diagnostics configuration file</span></span>  
 <span data-ttu-id="f00f1-114">a következő példa hello mutatja egy tipikus diagnosztika konfigurációs fájlt:</span><span class="sxs-lookup"><span data-stu-id="f00f1-114">hello following example shows a typical diagnostics configuration file:</span></span>  

```xml  
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"  
      configurationChangePollInterval="PT1M"  
      overallQuotaInMB="4096">  
   <DiagnosticInfrastructureLogs bufferQuotaInMB="1024"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M" />  
   <Logs bufferQuotaInMB="1024"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M" />  

   <Directories bufferQuotaInMB="1024"   
      scheduledTransferPeriod="PT1M">  

      <!-- These three elements specify hello special directories   
           that are set up for hello log types -->  
      <CrashDumps container="wad-crash-dumps" directoryQuotaInMB="256" />  
      <FailedRequestLogs container="wad-frq" directoryQuotaInMB="256" />  
      <IISLogs container="wad-iis" directoryQuotaInMB="256" />  

      <!-- For regular directories hello DataSources element is used -->  
      <DataSources>  
         <DirectoryConfiguration container="wad-panther" directoryQuotaInMB="128">  
            <!-- Absolute specifies an absolute path with optional environment expansion -->  
            <Absolute expandEnvironment="true" path="%SystemRoot%\system32\sysprep\Panther" />  
         </DirectoryConfiguration>  
         <DirectoryConfiguration container="wad-custom" directoryQuotaInMB="128">  
            <!-- LocalResource specifies a path relative tooa local   
                 resource defined in hello service definition -->  
            <LocalResource name="MyLoggingLocalResource" relativePath="logs" />  
         </DirectoryConfiguration>  
      </DataSources>  
   </Directories>  

   <PerformanceCounters bufferQuotaInMB="512" scheduledTransferPeriod="PT1M">  
      <!-- hello counter specifier is in hello same format as hello imperative   
           diagnostics configuration API -->  
      <PerformanceCounterConfiguration   
         counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT5S" />  
   </PerformanceCounters>  

   <WindowsEventLog bufferQuotaInMB="512"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M">  
      <!-- hello event log name is in hello same format as hello imperative   
           diagnostics configuration API -->  
      <DataSource name="System!*" />  
   </WindowsEventLog>  
</DiagnosticMonitorConfiguration>  
```  

## <a name="diagnosticsconfiguration-namespace"></a><span data-ttu-id="f00f1-115">DiagnosticsConfiguration Namespace</span><span class="sxs-lookup"><span data-stu-id="f00f1-115">DiagnosticsConfiguration Namespace</span></span>  
 <span data-ttu-id="f00f1-116">hello XML-névtér hello diagnosztika konfigurációs fájl van:</span><span class="sxs-lookup"><span data-stu-id="f00f1-116">hello XML namespace for hello diagnostics configuration file is:</span></span>  

```  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  
```  

## <a name="schema-elements"></a><span data-ttu-id="f00f1-117">Séma elemei</span><span class="sxs-lookup"><span data-stu-id="f00f1-117">Schema Elements</span></span>  
 <span data-ttu-id="f00f1-118">hello diagnosztika konfigurációs fájl hello a következő elemeket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f00f1-118">hello diagnostics configuration file includes hello following elements.</span></span>


## <a name="diagnosticmonitorconfiguration-element"></a><span data-ttu-id="f00f1-119">DiagnosticMonitorConfiguration elem</span><span class="sxs-lookup"><span data-stu-id="f00f1-119">DiagnosticMonitorConfiguration Element</span></span>  
<span data-ttu-id="f00f1-120">hello legfelső szintű elem hello diagnosztika konfigurációs fájl.</span><span class="sxs-lookup"><span data-stu-id="f00f1-120">hello top-level element of hello diagnostics configuration file.</span></span>  

<span data-ttu-id="f00f1-121">Attribútumok:</span><span class="sxs-lookup"><span data-stu-id="f00f1-121">Attributes:</span></span>

|<span data-ttu-id="f00f1-122">Attribútum</span><span class="sxs-lookup"><span data-stu-id="f00f1-122">Attribute</span></span>  |<span data-ttu-id="f00f1-123">Típus</span><span class="sxs-lookup"><span data-stu-id="f00f1-123">Type</span></span>   |<span data-ttu-id="f00f1-124">Szükséges</span><span class="sxs-lookup"><span data-stu-id="f00f1-124">Required</span></span>| <span data-ttu-id="f00f1-125">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="f00f1-125">Default</span></span> | <span data-ttu-id="f00f1-126">Leírás</span><span class="sxs-lookup"><span data-stu-id="f00f1-126">Description</span></span>|  
|-----------|-------|--------|---------|------------|  
|<span data-ttu-id="f00f1-127">**configurationChangePollInterval**</span><span class="sxs-lookup"><span data-stu-id="f00f1-127">**configurationChangePollInterval**</span></span>|<span data-ttu-id="f00f1-128">Időtartam</span><span class="sxs-lookup"><span data-stu-id="f00f1-128">duration</span></span>|<span data-ttu-id="f00f1-129">Optional</span><span class="sxs-lookup"><span data-stu-id="f00f1-129">Optional</span></span> | <span data-ttu-id="f00f1-130">PT1M</span><span class="sxs-lookup"><span data-stu-id="f00f1-130">PT1M</span></span>| <span data-ttu-id="f00f1-131">Diagnosztikai konfigurációs módosítások, mely hello diagnosztikai figyelő szavazások hello időközét adja meg.</span><span class="sxs-lookup"><span data-stu-id="f00f1-131">Specifies hello interval at which hello diagnostic monitor polls for diagnostic configuration changes.</span></span>|  
|<span data-ttu-id="f00f1-132">**overallQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="f00f1-132">**overallQuotaInMB**</span></span>|<span data-ttu-id="f00f1-133">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="f00f1-133">unsignedInt</span></span>|<span data-ttu-id="f00f1-134">Optional</span><span class="sxs-lookup"><span data-stu-id="f00f1-134">Optional</span></span>| <span data-ttu-id="f00f1-135">4000 MB.</span><span class="sxs-lookup"><span data-stu-id="f00f1-135">4000 MB.</span></span> <span data-ttu-id="f00f1-136">Ha megad egy értéket, nem haladhatja meg ezt a mennyiséget</span><span class="sxs-lookup"><span data-stu-id="f00f1-136">If you provide a value, it must not exceed this amount</span></span> |<span data-ttu-id="f00f1-137">hello teljes mennyisége az összes naplózási pufferek lefoglalt fájl rendszerhez szükséges tárhelyet.</span><span class="sxs-lookup"><span data-stu-id="f00f1-137">hello total amount of file system storage allocated for all logging buffers.</span></span>|  

## <a name="diagnosticinfrastructurelogs-element"></a><span data-ttu-id="f00f1-138">DiagnosticInfrastructureLogs elem</span><span class="sxs-lookup"><span data-stu-id="f00f1-138">DiagnosticInfrastructureLogs Element</span></span>  
<span data-ttu-id="f00f1-139">Definiálja a hello puffer konfigurációt hello tartozó diagnosztikai infrastruktúra által létrehozott hello naplókhoz.</span><span class="sxs-lookup"><span data-stu-id="f00f1-139">Defines hello buffer configuration for hello logs that are generated by hello underlying diagnostics infrastructure.</span></span>

<span data-ttu-id="f00f1-140">Szülőelem: [DiagnosticMonitorConfiguration elem](#DiagnosticMonitorConfiguration).</span><span class="sxs-lookup"><span data-stu-id="f00f1-140">Parent Element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span></span>  

<span data-ttu-id="f00f1-141">Attribútumok:</span><span class="sxs-lookup"><span data-stu-id="f00f1-141">Attributes:</span></span>

|<span data-ttu-id="f00f1-142">Attribútum</span><span class="sxs-lookup"><span data-stu-id="f00f1-142">Attribute</span></span>|<span data-ttu-id="f00f1-143">Típus</span><span class="sxs-lookup"><span data-stu-id="f00f1-143">Type</span></span>|<span data-ttu-id="f00f1-144">Leírás</span><span class="sxs-lookup"><span data-stu-id="f00f1-144">Description</span></span>|  
|---------|----|-----------------|  
|<span data-ttu-id="f00f1-145">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="f00f1-145">**bufferQuotaInMB**</span></span>|<span data-ttu-id="f00f1-146">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="f00f1-146">unsignedInt</span></span>|<span data-ttu-id="f00f1-147">Választható.</span><span class="sxs-lookup"><span data-stu-id="f00f1-147">Optional.</span></span> <span data-ttu-id="f00f1-148">Ennyi hello maximális fájl rendszerhez szükséges tárhelyet, amelyet a hello megadott adatok.</span><span class="sxs-lookup"><span data-stu-id="f00f1-148">Specifies hello maximum amount of file system storage that is available for hello specified data.</span></span><br /><br /> <span data-ttu-id="f00f1-149">hello alapértelmezett érték 0.</span><span class="sxs-lookup"><span data-stu-id="f00f1-149">hello default is 0.</span></span>|  
|<span data-ttu-id="f00f1-150">**scheduledTransferLogLevelFilter**</span><span class="sxs-lookup"><span data-stu-id="f00f1-150">**scheduledTransferLogLevelFilter**</span></span>|<span data-ttu-id="f00f1-151">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f00f1-151">string</span></span>|<span data-ttu-id="f00f1-152">Választható.</span><span class="sxs-lookup"><span data-stu-id="f00f1-152">Optional.</span></span> <span data-ttu-id="f00f1-153">Adja meg, amelyeket a naplóbejegyzések hello minimális súlyossági szintje.</span><span class="sxs-lookup"><span data-stu-id="f00f1-153">Specifies hello minimum severity level for log entries that are transferred.</span></span> <span data-ttu-id="f00f1-154">hello alapértelmezett értéke **meghatározatlan**.</span><span class="sxs-lookup"><span data-stu-id="f00f1-154">hello default value is **Undefined**.</span></span> <span data-ttu-id="f00f1-155">Más lehetséges értékei **részletes**, **információk**, **figyelmeztetés**, **hiba**, és **kritikus**.</span><span class="sxs-lookup"><span data-stu-id="f00f1-155">Other possible values are **Verbose**, **Information**, **Warning**, **Error**, and **Critical**.</span></span>|  
|<span data-ttu-id="f00f1-156">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="f00f1-156">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="f00f1-157">Időtartam</span><span class="sxs-lookup"><span data-stu-id="f00f1-157">duration</span></span>|<span data-ttu-id="f00f1-158">Választható.</span><span class="sxs-lookup"><span data-stu-id="f00f1-158">Optional.</span></span> <span data-ttu-id="f00f1-159">Hello időköze ütemezett átvitelek adatok felfelé toohello legközelebbi perc között.</span><span class="sxs-lookup"><span data-stu-id="f00f1-159">Specifies hello interval between scheduled transfers of data, rounded up toohello nearest minute.</span></span><br /><br /> <span data-ttu-id="f00f1-160">hello alapértelmezés szerint PT0S.</span><span class="sxs-lookup"><span data-stu-id="f00f1-160">hello default is PT0S.</span></span>|  

## <a name="logs-element"></a><span data-ttu-id="f00f1-161">Naplók elem</span><span class="sxs-lookup"><span data-stu-id="f00f1-161">Logs Element</span></span>  
 <span data-ttu-id="f00f1-162">Definiálja a hello puffer konfigurációt az alapszintű Azure-naplók.</span><span class="sxs-lookup"><span data-stu-id="f00f1-162">Defines hello buffer configuration for basic Azure logs.</span></span>

 <span data-ttu-id="f00f1-163">Szülőelem: [DiagnosticMonitorConfiguration elem](#DiagnosticMonitorConfiguration).</span><span class="sxs-lookup"><span data-stu-id="f00f1-163">Parent element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span></span>  

<span data-ttu-id="f00f1-164">Attribútumok:</span><span class="sxs-lookup"><span data-stu-id="f00f1-164">Attributes:</span></span>  

|<span data-ttu-id="f00f1-165">Attribútum</span><span class="sxs-lookup"><span data-stu-id="f00f1-165">Attribute</span></span>|<span data-ttu-id="f00f1-166">Típus</span><span class="sxs-lookup"><span data-stu-id="f00f1-166">Type</span></span>|<span data-ttu-id="f00f1-167">Leírás</span><span class="sxs-lookup"><span data-stu-id="f00f1-167">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="f00f1-168">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="f00f1-168">**bufferQuotaInMB**</span></span>|<span data-ttu-id="f00f1-169">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="f00f1-169">unsignedInt</span></span>|<span data-ttu-id="f00f1-170">Választható.</span><span class="sxs-lookup"><span data-stu-id="f00f1-170">Optional.</span></span> <span data-ttu-id="f00f1-171">Ennyi hello maximális fájl rendszerhez szükséges tárhelyet, amelyet a hello megadott adatok.</span><span class="sxs-lookup"><span data-stu-id="f00f1-171">Specifies hello maximum amount of file system storage that is available for hello specified data.</span></span><br /><br /> <span data-ttu-id="f00f1-172">hello alapértelmezett érték 0.</span><span class="sxs-lookup"><span data-stu-id="f00f1-172">hello default is 0.</span></span>|  
|<span data-ttu-id="f00f1-173">**scheduledTransferLogLevelFilter**</span><span class="sxs-lookup"><span data-stu-id="f00f1-173">**scheduledTransferLogLevelFilter**</span></span>|<span data-ttu-id="f00f1-174">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f00f1-174">string</span></span>|<span data-ttu-id="f00f1-175">Választható.</span><span class="sxs-lookup"><span data-stu-id="f00f1-175">Optional.</span></span> <span data-ttu-id="f00f1-176">Adja meg, amelyeket a naplóbejegyzések hello minimális súlyossági szintje.</span><span class="sxs-lookup"><span data-stu-id="f00f1-176">Specifies hello minimum severity level for log entries that are transferred.</span></span> <span data-ttu-id="f00f1-177">hello alapértelmezett értéke **meghatározatlan**.</span><span class="sxs-lookup"><span data-stu-id="f00f1-177">hello default value is **Undefined**.</span></span> <span data-ttu-id="f00f1-178">Más lehetséges értékei **részletes**, **információk**, **figyelmeztetés**, **hiba**, és **kritikus**.</span><span class="sxs-lookup"><span data-stu-id="f00f1-178">Other possible values are **Verbose**, **Information**, **Warning**, **Error**, and **Critical**.</span></span>|  
|<span data-ttu-id="f00f1-179">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="f00f1-179">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="f00f1-180">Időtartam</span><span class="sxs-lookup"><span data-stu-id="f00f1-180">duration</span></span>|<span data-ttu-id="f00f1-181">Választható.</span><span class="sxs-lookup"><span data-stu-id="f00f1-181">Optional.</span></span> <span data-ttu-id="f00f1-182">Hello időköze ütemezett átvitelek adatok felfelé toohello legközelebbi perc között.</span><span class="sxs-lookup"><span data-stu-id="f00f1-182">Specifies hello interval between scheduled transfers of data, rounded up toohello nearest minute.</span></span><br /><br /> <span data-ttu-id="f00f1-183">hello alapértelmezés szerint PT0S.</span><span class="sxs-lookup"><span data-stu-id="f00f1-183">hello default is PT0S.</span></span>|  

## <a name="directories-element"></a><span data-ttu-id="f00f1-184">Könyvtárak elem</span><span class="sxs-lookup"><span data-stu-id="f00f1-184">Directories Element</span></span>  
<span data-ttu-id="f00f1-185">Hello puffer konfigurációs fájl alapú naplófájlokat, amelyek alapján meghatározhatja az határozza meg.</span><span class="sxs-lookup"><span data-stu-id="f00f1-185">Defines hello buffer configuration for file-based logs that you can define.</span></span>

<span data-ttu-id="f00f1-186">Szülőelem: [DiagnosticMonitorConfiguration elem](#DiagnosticMonitorConfiguration).</span><span class="sxs-lookup"><span data-stu-id="f00f1-186">Parent element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span></span>  


<span data-ttu-id="f00f1-187">Attribútumok:</span><span class="sxs-lookup"><span data-stu-id="f00f1-187">Attributes:</span></span>  

|<span data-ttu-id="f00f1-188">Attribútum</span><span class="sxs-lookup"><span data-stu-id="f00f1-188">Attribute</span></span>|<span data-ttu-id="f00f1-189">Típus</span><span class="sxs-lookup"><span data-stu-id="f00f1-189">Type</span></span>|<span data-ttu-id="f00f1-190">Leírás</span><span class="sxs-lookup"><span data-stu-id="f00f1-190">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="f00f1-191">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="f00f1-191">**bufferQuotaInMB**</span></span>|<span data-ttu-id="f00f1-192">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="f00f1-192">unsignedInt</span></span>|<span data-ttu-id="f00f1-193">Választható.</span><span class="sxs-lookup"><span data-stu-id="f00f1-193">Optional.</span></span> <span data-ttu-id="f00f1-194">Ennyi hello maximális fájl rendszerhez szükséges tárhelyet, amelyet a hello megadott adatok.</span><span class="sxs-lookup"><span data-stu-id="f00f1-194">Specifies hello maximum amount of file system storage that is available for hello specified data.</span></span><br /><br /> <span data-ttu-id="f00f1-195">hello alapértelmezett érték 0.</span><span class="sxs-lookup"><span data-stu-id="f00f1-195">hello default is 0.</span></span>|  
|<span data-ttu-id="f00f1-196">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="f00f1-196">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="f00f1-197">Időtartam</span><span class="sxs-lookup"><span data-stu-id="f00f1-197">duration</span></span>|<span data-ttu-id="f00f1-198">Választható.</span><span class="sxs-lookup"><span data-stu-id="f00f1-198">Optional.</span></span> <span data-ttu-id="f00f1-199">Hello időköze ütemezett átvitelek adatok felfelé toohello legközelebbi perc között.</span><span class="sxs-lookup"><span data-stu-id="f00f1-199">Specifies hello interval between scheduled transfers of data, rounded up toohello nearest minute.</span></span><br /><br /> <span data-ttu-id="f00f1-200">hello alapértelmezés szerint PT0S.</span><span class="sxs-lookup"><span data-stu-id="f00f1-200">hello default is PT0S.</span></span>|  

## <a name="crashdumps-element"></a><span data-ttu-id="f00f1-201">CrashDumps elem</span><span class="sxs-lookup"><span data-stu-id="f00f1-201">CrashDumps Element</span></span>  
 <span data-ttu-id="f00f1-202">Hello összeomlási memóriaképek directory határozza meg.</span><span class="sxs-lookup"><span data-stu-id="f00f1-202">Defines hello crash dumps directory.</span></span>

 <span data-ttu-id="f00f1-203">Szülőelem: [könyvtárak elem](#Directories).</span><span class="sxs-lookup"><span data-stu-id="f00f1-203">Parent Element: [Directories Element](#Directories).</span></span>  

<span data-ttu-id="f00f1-204">Attribútumok:</span><span class="sxs-lookup"><span data-stu-id="f00f1-204">Attributes:</span></span>  

|<span data-ttu-id="f00f1-205">Attribútum</span><span class="sxs-lookup"><span data-stu-id="f00f1-205">Attribute</span></span>|<span data-ttu-id="f00f1-206">Típus</span><span class="sxs-lookup"><span data-stu-id="f00f1-206">Type</span></span>|<span data-ttu-id="f00f1-207">Leírás</span><span class="sxs-lookup"><span data-stu-id="f00f1-207">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="f00f1-208">**tároló**</span><span class="sxs-lookup"><span data-stu-id="f00f1-208">**container**</span></span>|<span data-ttu-id="f00f1-209">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f00f1-209">string</span></span>|<span data-ttu-id="f00f1-210">hol hello hello könyvtár tartalma található toobe hello tároló neve hello át.</span><span class="sxs-lookup"><span data-stu-id="f00f1-210">hello name of hello container where hello contents of hello directory is toobe transferred.</span></span>|  
|<span data-ttu-id="f00f1-211">**directoryQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="f00f1-211">**directoryQuotaInMB**</span></span>|<span data-ttu-id="f00f1-212">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="f00f1-212">unsignedInt</span></span>|<span data-ttu-id="f00f1-213">Választható.</span><span class="sxs-lookup"><span data-stu-id="f00f1-213">Optional.</span></span> <span data-ttu-id="f00f1-214">Hello hello könyvtár maximális méretét megabájtban megadva.</span><span class="sxs-lookup"><span data-stu-id="f00f1-214">Specifies hello maximum size of hello directory in megabytes.</span></span><br /><br /> <span data-ttu-id="f00f1-215">hello alapértelmezett érték 0.</span><span class="sxs-lookup"><span data-stu-id="f00f1-215">hello default is 0.</span></span>|  

## <a name="failedrequestlogs-element"></a><span data-ttu-id="f00f1-216">FailedRequestLogs elem</span><span class="sxs-lookup"><span data-stu-id="f00f1-216">FailedRequestLogs Element</span></span>  
 <span data-ttu-id="f00f1-217">Határozza meg a sikertelen kérelmek naplókönyvtár hello.</span><span class="sxs-lookup"><span data-stu-id="f00f1-217">Defines hello failed request log directory.</span></span>

 <span data-ttu-id="f00f1-218">Szülő elem [könyvtárak elem](#Directories).</span><span class="sxs-lookup"><span data-stu-id="f00f1-218">Parent Element [Directories Element](#Directories).</span></span>  

<span data-ttu-id="f00f1-219">Attribútumok:</span><span class="sxs-lookup"><span data-stu-id="f00f1-219">Attributes:</span></span>  

|<span data-ttu-id="f00f1-220">Attribútum</span><span class="sxs-lookup"><span data-stu-id="f00f1-220">Attribute</span></span>|<span data-ttu-id="f00f1-221">Típus</span><span class="sxs-lookup"><span data-stu-id="f00f1-221">Type</span></span>|<span data-ttu-id="f00f1-222">Leírás</span><span class="sxs-lookup"><span data-stu-id="f00f1-222">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="f00f1-223">**tároló**</span><span class="sxs-lookup"><span data-stu-id="f00f1-223">**container**</span></span>|<span data-ttu-id="f00f1-224">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f00f1-224">string</span></span>|<span data-ttu-id="f00f1-225">hol hello hello könyvtár tartalma található toobe hello tároló neve hello át.</span><span class="sxs-lookup"><span data-stu-id="f00f1-225">hello name of hello container where hello contents of hello directory is toobe transferred.</span></span>|  
|<span data-ttu-id="f00f1-226">**directoryQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="f00f1-226">**directoryQuotaInMB**</span></span>|<span data-ttu-id="f00f1-227">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="f00f1-227">unsignedInt</span></span>|<span data-ttu-id="f00f1-228">Választható.</span><span class="sxs-lookup"><span data-stu-id="f00f1-228">Optional.</span></span> <span data-ttu-id="f00f1-229">Hello hello könyvtár maximális méretét megabájtban megadva.</span><span class="sxs-lookup"><span data-stu-id="f00f1-229">Specifies hello maximum size of hello directory in megabytes.</span></span><br /><br /> <span data-ttu-id="f00f1-230">hello alapértelmezett érték 0.</span><span class="sxs-lookup"><span data-stu-id="f00f1-230">hello default is 0.</span></span>|  

##  <a name="iislogs-element"></a><span data-ttu-id="f00f1-231">IISLogs elem</span><span class="sxs-lookup"><span data-stu-id="f00f1-231">IISLogs Element</span></span>  
 <span data-ttu-id="f00f1-232">Hello IIS naplókönyvtár határozza meg.</span><span class="sxs-lookup"><span data-stu-id="f00f1-232">Defines hello IIS log directory.</span></span>

 <span data-ttu-id="f00f1-233">Szülő elem [könyvtárak elem](#Directories).</span><span class="sxs-lookup"><span data-stu-id="f00f1-233">Parent Element [Directories Element](#Directories).</span></span>  

<span data-ttu-id="f00f1-234">Attribútumok:</span><span class="sxs-lookup"><span data-stu-id="f00f1-234">Attributes:</span></span>  

|<span data-ttu-id="f00f1-235">Attribútum</span><span class="sxs-lookup"><span data-stu-id="f00f1-235">Attribute</span></span>|<span data-ttu-id="f00f1-236">Típus</span><span class="sxs-lookup"><span data-stu-id="f00f1-236">Type</span></span>|<span data-ttu-id="f00f1-237">Leírás</span><span class="sxs-lookup"><span data-stu-id="f00f1-237">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="f00f1-238">**tároló**</span><span class="sxs-lookup"><span data-stu-id="f00f1-238">**container**</span></span>|<span data-ttu-id="f00f1-239">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f00f1-239">string</span></span>|<span data-ttu-id="f00f1-240">hol hello hello könyvtár tartalma található toobe hello tároló neve hello át.</span><span class="sxs-lookup"><span data-stu-id="f00f1-240">hello name of hello container where hello contents of hello directory is toobe transferred.</span></span>|  
|<span data-ttu-id="f00f1-241">**directoryQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="f00f1-241">**directoryQuotaInMB**</span></span>|<span data-ttu-id="f00f1-242">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="f00f1-242">unsignedInt</span></span>|<span data-ttu-id="f00f1-243">Választható.</span><span class="sxs-lookup"><span data-stu-id="f00f1-243">Optional.</span></span> <span data-ttu-id="f00f1-244">Hello hello könyvtár maximális méretét megabájtban megadva.</span><span class="sxs-lookup"><span data-stu-id="f00f1-244">Specifies hello maximum size of hello directory in megabytes.</span></span><br /><br /> <span data-ttu-id="f00f1-245">hello alapértelmezett érték 0.</span><span class="sxs-lookup"><span data-stu-id="f00f1-245">hello default is 0.</span></span>|  

## <a name="datasources-element"></a><span data-ttu-id="f00f1-246">Adatforrások elem</span><span class="sxs-lookup"><span data-stu-id="f00f1-246">DataSources Element</span></span>  
 <span data-ttu-id="f00f1-247">Határozza meg a nulla vagy több további naplók könyvtárak.</span><span class="sxs-lookup"><span data-stu-id="f00f1-247">Defines zero or more additional log directories.</span></span>

 <span data-ttu-id="f00f1-248">Szülőelem: [könyvtárak elem](#Directories).</span><span class="sxs-lookup"><span data-stu-id="f00f1-248">Parent Element: [Directories Element](#Directories).</span></span>

## <a name="directoryconfiguration-element"></a><span data-ttu-id="f00f1-249">DirectoryConfiguration elem</span><span class="sxs-lookup"><span data-stu-id="f00f1-249">DirectoryConfiguration Element</span></span>  
 <span data-ttu-id="f00f1-250">Határozza meg a napló fájlok toomonitor hello könyvtárában.</span><span class="sxs-lookup"><span data-stu-id="f00f1-250">Defines hello directory of log files toomonitor.</span></span>

 <span data-ttu-id="f00f1-251">Szülőelem: [adatforrások elem](#DataSources).</span><span class="sxs-lookup"><span data-stu-id="f00f1-251">Parent Element: [DataSources Element](#DataSources).</span></span>

<span data-ttu-id="f00f1-252">Attribútumok:</span><span class="sxs-lookup"><span data-stu-id="f00f1-252">Attributes:</span></span>

|<span data-ttu-id="f00f1-253">Attribútum</span><span class="sxs-lookup"><span data-stu-id="f00f1-253">Attribute</span></span>|<span data-ttu-id="f00f1-254">Típus</span><span class="sxs-lookup"><span data-stu-id="f00f1-254">Type</span></span>|<span data-ttu-id="f00f1-255">Leírás</span><span class="sxs-lookup"><span data-stu-id="f00f1-255">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="f00f1-256">**tároló**</span><span class="sxs-lookup"><span data-stu-id="f00f1-256">**container**</span></span>|<span data-ttu-id="f00f1-257">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f00f1-257">string</span></span>|<span data-ttu-id="f00f1-258">hol hello hello könyvtár tartalma található toobe hello tároló neve hello át.</span><span class="sxs-lookup"><span data-stu-id="f00f1-258">hello name of hello container where hello contents of hello directory is toobe transferred.</span></span>|  
|<span data-ttu-id="f00f1-259">**directoryQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="f00f1-259">**directoryQuotaInMB**</span></span>|<span data-ttu-id="f00f1-260">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="f00f1-260">unsignedInt</span></span>|<span data-ttu-id="f00f1-261">Választható.</span><span class="sxs-lookup"><span data-stu-id="f00f1-261">Optional.</span></span> <span data-ttu-id="f00f1-262">Hello hello könyvtár maximális méretét megabájtban megadva.</span><span class="sxs-lookup"><span data-stu-id="f00f1-262">Specifies hello maximum size of hello directory in megabytes.</span></span><br /><br /> <span data-ttu-id="f00f1-263">hello alapértelmezett érték 0.</span><span class="sxs-lookup"><span data-stu-id="f00f1-263">hello default is 0.</span></span>|  

## <a name="absolute-element"></a><span data-ttu-id="f00f1-264">Abszolút elem</span><span class="sxs-lookup"><span data-stu-id="f00f1-264">Absolute Element</span></span>  
 <span data-ttu-id="f00f1-265">A nem kötelező környezeti bővítéssel rendelkező hello directory toomonitor abszolút elérési utat határozza meg.</span><span class="sxs-lookup"><span data-stu-id="f00f1-265">Defines an absolute path of hello directory toomonitor with optional environment expansion.</span></span>

 <span data-ttu-id="f00f1-266">Szülőelem: [DirectoryConfiguration elem](#DirectoryConfiguration).</span><span class="sxs-lookup"><span data-stu-id="f00f1-266">Parent Element: [DirectoryConfiguration Element](#DirectoryConfiguration).</span></span>  

<span data-ttu-id="f00f1-267">Attribútumok:</span><span class="sxs-lookup"><span data-stu-id="f00f1-267">Attributes:</span></span>  

|<span data-ttu-id="f00f1-268">Attribútum</span><span class="sxs-lookup"><span data-stu-id="f00f1-268">Attribute</span></span>|<span data-ttu-id="f00f1-269">Típus</span><span class="sxs-lookup"><span data-stu-id="f00f1-269">Type</span></span>|<span data-ttu-id="f00f1-270">Leírás</span><span class="sxs-lookup"><span data-stu-id="f00f1-270">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="f00f1-271">**elérési út**</span><span class="sxs-lookup"><span data-stu-id="f00f1-271">**path**</span></span>|<span data-ttu-id="f00f1-272">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f00f1-272">string</span></span>|<span data-ttu-id="f00f1-273">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="f00f1-273">Required.</span></span> <span data-ttu-id="f00f1-274">hello abszolút elérési út toohello directory toomonitor.</span><span class="sxs-lookup"><span data-stu-id="f00f1-274">hello absolute path toohello directory toomonitor.</span></span>|  
|<span data-ttu-id="f00f1-275">**expandEnvironment**</span><span class="sxs-lookup"><span data-stu-id="f00f1-275">**expandEnvironment**</span></span>|<span data-ttu-id="f00f1-276">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="f00f1-276">boolean</span></span>|<span data-ttu-id="f00f1-277">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="f00f1-277">Required.</span></span> <span data-ttu-id="f00f1-278">Ha túl beállítása**igaz**, környezeti változók hello elérési úton levő figyelő kibontva látható.</span><span class="sxs-lookup"><span data-stu-id="f00f1-278">If set too**true**, environment variables in hello path are expanded.</span></span>|  

## <a name="localresource-element"></a><span data-ttu-id="f00f1-279">LocalResource elem</span><span class="sxs-lookup"><span data-stu-id="f00f1-279">LocalResource Element</span></span>  
 <span data-ttu-id="f00f1-280">Határozza meg az elérési út relatív tooa helyi erőforrás hello szolgáltatás-definícióban meghatározott.</span><span class="sxs-lookup"><span data-stu-id="f00f1-280">Defines a path relative tooa local resource defined in hello service definition.</span></span>

 <span data-ttu-id="f00f1-281">Szülőelem: [DirectoryConfiguration elem](#DirectoryConfiguration).</span><span class="sxs-lookup"><span data-stu-id="f00f1-281">Parent Element: [DirectoryConfiguration Element](#DirectoryConfiguration).</span></span>  

<span data-ttu-id="f00f1-282">Attribútumok:</span><span class="sxs-lookup"><span data-stu-id="f00f1-282">Attributes:</span></span>  

|<span data-ttu-id="f00f1-283">Attribútum</span><span class="sxs-lookup"><span data-stu-id="f00f1-283">Attribute</span></span>|<span data-ttu-id="f00f1-284">Típus</span><span class="sxs-lookup"><span data-stu-id="f00f1-284">Type</span></span>|<span data-ttu-id="f00f1-285">Leírás</span><span class="sxs-lookup"><span data-stu-id="f00f1-285">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="f00f1-286">**név**</span><span class="sxs-lookup"><span data-stu-id="f00f1-286">**name**</span></span>|<span data-ttu-id="f00f1-287">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f00f1-287">string</span></span>|<span data-ttu-id="f00f1-288">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="f00f1-288">Required.</span></span> <span data-ttu-id="f00f1-289">hello helyi erőforrás hello directory toomonitor tartalmazó hello nevét.</span><span class="sxs-lookup"><span data-stu-id="f00f1-289">hello name of hello local resource that contains hello directory toomonitor.</span></span>|  
|<span data-ttu-id="f00f1-290">**relativePath**</span><span class="sxs-lookup"><span data-stu-id="f00f1-290">**relativePath**</span></span>|<span data-ttu-id="f00f1-291">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f00f1-291">string</span></span>|<span data-ttu-id="f00f1-292">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="f00f1-292">Required.</span></span> <span data-ttu-id="f00f1-293">hello elérési útja relatív toohello helyi erőforrás toomonitor.</span><span class="sxs-lookup"><span data-stu-id="f00f1-293">hello path relative toohello local resource toomonitor.</span></span>|  

## <a name="performancecounters-element"></a><span data-ttu-id="f00f1-294">PerformanceCounters elem</span><span class="sxs-lookup"><span data-stu-id="f00f1-294">PerformanceCounters Element</span></span>  
 <span data-ttu-id="f00f1-295">Hello elérési toohello teljesítmény számláló toocollect határozza meg.</span><span class="sxs-lookup"><span data-stu-id="f00f1-295">Defines hello path toohello performance counter toocollect.</span></span>

 <span data-ttu-id="f00f1-296">Szülőelem: [DiagnosticMonitorConfiguration elem](#DiagnosticMonitorConfiguration).</span><span class="sxs-lookup"><span data-stu-id="f00f1-296">Parent Element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span></span>


 <span data-ttu-id="f00f1-297">Attribútumok:</span><span class="sxs-lookup"><span data-stu-id="f00f1-297">Attributes:</span></span>  

|<span data-ttu-id="f00f1-298">Attribútum</span><span class="sxs-lookup"><span data-stu-id="f00f1-298">Attribute</span></span>|<span data-ttu-id="f00f1-299">Típus</span><span class="sxs-lookup"><span data-stu-id="f00f1-299">Type</span></span>|<span data-ttu-id="f00f1-300">Leírás</span><span class="sxs-lookup"><span data-stu-id="f00f1-300">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="f00f1-301">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="f00f1-301">**bufferQuotaInMB**</span></span>|<span data-ttu-id="f00f1-302">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="f00f1-302">unsignedInt</span></span>|<span data-ttu-id="f00f1-303">Választható.</span><span class="sxs-lookup"><span data-stu-id="f00f1-303">Optional.</span></span> <span data-ttu-id="f00f1-304">Ennyi hello maximális fájl rendszerhez szükséges tárhelyet, amelyet a hello megadott adatok.</span><span class="sxs-lookup"><span data-stu-id="f00f1-304">Specifies hello maximum amount of file system storage that is available for hello specified data.</span></span><br /><br /> <span data-ttu-id="f00f1-305">hello alapértelmezett érték 0.</span><span class="sxs-lookup"><span data-stu-id="f00f1-305">hello default is 0.</span></span>|  
|<span data-ttu-id="f00f1-306">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="f00f1-306">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="f00f1-307">Időtartam</span><span class="sxs-lookup"><span data-stu-id="f00f1-307">duration</span></span>|<span data-ttu-id="f00f1-308">Választható.</span><span class="sxs-lookup"><span data-stu-id="f00f1-308">Optional.</span></span> <span data-ttu-id="f00f1-309">Hello időköze ütemezett átvitelek adatok felfelé toohello legközelebbi perc között.</span><span class="sxs-lookup"><span data-stu-id="f00f1-309">Specifies hello interval between scheduled transfers of data, rounded up toohello nearest minute.</span></span><br /><br /> <span data-ttu-id="f00f1-310">hello alapértelmezés szerint PT0S.</span><span class="sxs-lookup"><span data-stu-id="f00f1-310">hello default is PT0S.</span></span>|  

## <a name="performancecounterconfiguration-element"></a><span data-ttu-id="f00f1-311">PerformanceCounterConfiguration elem</span><span class="sxs-lookup"><span data-stu-id="f00f1-311">PerformanceCounterConfiguration Element</span></span>  
 <span data-ttu-id="f00f1-312">Hello teljesítmény számláló toocollect határozza meg.</span><span class="sxs-lookup"><span data-stu-id="f00f1-312">Defines hello performance counter toocollect.</span></span>

 <span data-ttu-id="f00f1-313">Szülőelem: [PerformanceCounters elem](#PerformanceCounters).</span><span class="sxs-lookup"><span data-stu-id="f00f1-313">Parent Element: [PerformanceCounters Element](#PerformanceCounters).</span></span>  

 <span data-ttu-id="f00f1-314">Attribútumok:</span><span class="sxs-lookup"><span data-stu-id="f00f1-314">Attributes:</span></span>  

|<span data-ttu-id="f00f1-315">Attribútum</span><span class="sxs-lookup"><span data-stu-id="f00f1-315">Attribute</span></span>|<span data-ttu-id="f00f1-316">Típus</span><span class="sxs-lookup"><span data-stu-id="f00f1-316">Type</span></span>|<span data-ttu-id="f00f1-317">Leírás</span><span class="sxs-lookup"><span data-stu-id="f00f1-317">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="f00f1-318">**counterSpecifier**</span><span class="sxs-lookup"><span data-stu-id="f00f1-318">**counterSpecifier**</span></span>|<span data-ttu-id="f00f1-319">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f00f1-319">string</span></span>|<span data-ttu-id="f00f1-320">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="f00f1-320">Required.</span></span> <span data-ttu-id="f00f1-321">hello elérési toohello teljesítmény számláló toocollect.</span><span class="sxs-lookup"><span data-stu-id="f00f1-321">hello path toohello performance counter toocollect.</span></span>|  
|<span data-ttu-id="f00f1-322">**sampleRate**</span><span class="sxs-lookup"><span data-stu-id="f00f1-322">**sampleRate**</span></span>|<span data-ttu-id="f00f1-323">Időtartam</span><span class="sxs-lookup"><span data-stu-id="f00f1-323">duration</span></span>|<span data-ttu-id="f00f1-324">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="f00f1-324">Required.</span></span> <span data-ttu-id="f00f1-325">hello sebessége, mely hello teljesítményszámláló kell összegyűjteni.</span><span class="sxs-lookup"><span data-stu-id="f00f1-325">hello rate at which hello performance counter should be collected.</span></span>|  

## <a name="windowseventlog-element"></a><span data-ttu-id="f00f1-326">WindowsEventLog elem</span><span class="sxs-lookup"><span data-stu-id="f00f1-326">WindowsEventLog Element</span></span>  
 <span data-ttu-id="f00f1-327">Hello eseménynaplók toomonitor határozza meg.</span><span class="sxs-lookup"><span data-stu-id="f00f1-327">Defines hello event logs toomonitor.</span></span>

 <span data-ttu-id="f00f1-328">Szülőelem: [DiagnosticMonitorConfiguration elem](#DiagnosticMonitorConfiguration).</span><span class="sxs-lookup"><span data-stu-id="f00f1-328">Parent Element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span></span>

  <span data-ttu-id="f00f1-329">Attribútumok:</span><span class="sxs-lookup"><span data-stu-id="f00f1-329">Attributes:</span></span>

|<span data-ttu-id="f00f1-330">Attribútum</span><span class="sxs-lookup"><span data-stu-id="f00f1-330">Attribute</span></span>|<span data-ttu-id="f00f1-331">Típus</span><span class="sxs-lookup"><span data-stu-id="f00f1-331">Type</span></span>|<span data-ttu-id="f00f1-332">Leírás</span><span class="sxs-lookup"><span data-stu-id="f00f1-332">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="f00f1-333">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="f00f1-333">**bufferQuotaInMB**</span></span>|<span data-ttu-id="f00f1-334">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="f00f1-334">unsignedInt</span></span>|<span data-ttu-id="f00f1-335">Választható.</span><span class="sxs-lookup"><span data-stu-id="f00f1-335">Optional.</span></span> <span data-ttu-id="f00f1-336">Ennyi hello maximális fájl rendszerhez szükséges tárhelyet, amelyet a hello megadott adatok.</span><span class="sxs-lookup"><span data-stu-id="f00f1-336">Specifies hello maximum amount of file system storage that is available for hello specified data.</span></span><br /><br /> <span data-ttu-id="f00f1-337">hello alapértelmezett érték 0.</span><span class="sxs-lookup"><span data-stu-id="f00f1-337">hello default is 0.</span></span>|  
|<span data-ttu-id="f00f1-338">**scheduledTransferLogLevelFilter**</span><span class="sxs-lookup"><span data-stu-id="f00f1-338">**scheduledTransferLogLevelFilter**</span></span>|<span data-ttu-id="f00f1-339">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f00f1-339">string</span></span>|<span data-ttu-id="f00f1-340">Választható.</span><span class="sxs-lookup"><span data-stu-id="f00f1-340">Optional.</span></span> <span data-ttu-id="f00f1-341">Adja meg, amelyeket a naplóbejegyzések hello minimális súlyossági szintje.</span><span class="sxs-lookup"><span data-stu-id="f00f1-341">Specifies hello minimum severity level for log entries that are transferred.</span></span> <span data-ttu-id="f00f1-342">hello alapértelmezett értéke **meghatározatlan**.</span><span class="sxs-lookup"><span data-stu-id="f00f1-342">hello default value is **Undefined**.</span></span> <span data-ttu-id="f00f1-343">Más lehetséges értékei **részletes**, **információk**, **figyelmeztetés**, **hiba**, és **kritikus**.</span><span class="sxs-lookup"><span data-stu-id="f00f1-343">Other possible values are **Verbose**, **Information**, **Warning**, **Error**, and **Critical**.</span></span>|  
|<span data-ttu-id="f00f1-344">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="f00f1-344">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="f00f1-345">Időtartam</span><span class="sxs-lookup"><span data-stu-id="f00f1-345">duration</span></span>|<span data-ttu-id="f00f1-346">Választható.</span><span class="sxs-lookup"><span data-stu-id="f00f1-346">Optional.</span></span> <span data-ttu-id="f00f1-347">Hello időköze ütemezett átvitelek adatok felfelé toohello legközelebbi perc között.</span><span class="sxs-lookup"><span data-stu-id="f00f1-347">Specifies hello interval between scheduled transfers of data, rounded up toohello nearest minute.</span></span><br /><br /> <span data-ttu-id="f00f1-348">hello alapértelmezés szerint PT0S.</span><span class="sxs-lookup"><span data-stu-id="f00f1-348">hello default is PT0S.</span></span>|  

## <a name="datasource-element"></a><span data-ttu-id="f00f1-349">DataSource elem</span><span class="sxs-lookup"><span data-stu-id="f00f1-349">DataSource Element</span></span>  
 <span data-ttu-id="f00f1-350">Hello Eseménynapló toomonitor határozza meg.</span><span class="sxs-lookup"><span data-stu-id="f00f1-350">Defines hello event log toomonitor.</span></span>

 <span data-ttu-id="f00f1-351">Szülőelem: [WindowsEventLog elem](#windowsEventLog).</span><span class="sxs-lookup"><span data-stu-id="f00f1-351">Parent Element: [WindowsEventLog Element](#windowsEventLog).</span></span>  

 <span data-ttu-id="f00f1-352">Attribútumok:</span><span class="sxs-lookup"><span data-stu-id="f00f1-352">Attributes:</span></span>

|<span data-ttu-id="f00f1-353">Attribútum</span><span class="sxs-lookup"><span data-stu-id="f00f1-353">Attribute</span></span>|<span data-ttu-id="f00f1-354">Típus</span><span class="sxs-lookup"><span data-stu-id="f00f1-354">Type</span></span>|<span data-ttu-id="f00f1-355">Leírás</span><span class="sxs-lookup"><span data-stu-id="f00f1-355">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="f00f1-356">**név**</span><span class="sxs-lookup"><span data-stu-id="f00f1-356">**name**</span></span>|<span data-ttu-id="f00f1-357">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f00f1-357">string</span></span>|<span data-ttu-id="f00f1-358">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="f00f1-358">Required.</span></span> <span data-ttu-id="f00f1-359">Hello napló toocollect megadó XPath kifejezés.</span><span class="sxs-lookup"><span data-stu-id="f00f1-359">An XPath expression specifying hello log toocollect.</span></span>|  
