---
title: "Adatfolyam-Azure diagnosztikai adatokat a gyakran használt adatok elérési útja az Event Hubs használatával |} Microsoft Docs"
description: "Azure Diagnostics konfigurálása az Event Hubs végpontok közötti, beleértve a közös forgatókönyvre vonatkozó útmutatást."
services: event-hubs
documentationcenter: na
author: rboucher
manager: carmonm
editor: 
ms.assetid: edeebaac-1c47-4b43-9687-f28e7e1e446a
ms.service: monitoring-and-diagnostics
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: robb
ms.openlocfilehash: 1c05bd6dc4c4d394aa043b9995de9c184e4f14c6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="streaming-azure-diagnostics-data-in-the-hot-path-by-using-event-hubs"></a><span data-ttu-id="98b46-103">Adatfolyam-Azure diagnosztikai adatokat a gyakran használt adatok elérési útja az Event Hubs használatával</span><span class="sxs-lookup"><span data-stu-id="98b46-103">Streaming Azure Diagnostics data in the hot path by using Event Hubs</span></span>
<span data-ttu-id="98b46-104">Az Azure Diagnostics metrikák és a naplók összegyűjtésére felhőalapú szolgáltatások virtuális gépek (VM) és az eredmények átvitele az Azure Storage rugalmas módszereket biztosítja.</span><span class="sxs-lookup"><span data-stu-id="98b46-104">Azure Diagnostics provides flexible ways to collect metrics and logs from cloud services virtual machines (VMs) and transfer results to Azure Storage.</span></span> <span data-ttu-id="98b46-105">A 2016. március (SDK 2.9) időkereten belül-től kezdődően diagnosztika küldése egyéni adatforrások, működés közbeni elérési adatok átviteléhez az másodpercben használatával [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="98b46-105">Starting in the March 2016 (SDK 2.9) time frame, you can send Diagnostics to custom data sources and transfer hot path data in seconds by using [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/).</span></span>

<span data-ttu-id="98b46-106">Támogatott adattípusok a következők:</span><span class="sxs-lookup"><span data-stu-id="98b46-106">Supported data types include:</span></span>

* <span data-ttu-id="98b46-107">A Windows esemény-nyomkövetés (ETW) eseményei</span><span class="sxs-lookup"><span data-stu-id="98b46-107">Event Tracing for Windows (ETW) events</span></span>
* <span data-ttu-id="98b46-108">Teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="98b46-108">Performance counters</span></span>
* <span data-ttu-id="98b46-109">Windows-Eseménynapló</span><span class="sxs-lookup"><span data-stu-id="98b46-109">Windows event logs</span></span>
* <span data-ttu-id="98b46-110">Alkalmazás-naplók</span><span class="sxs-lookup"><span data-stu-id="98b46-110">Application logs</span></span>
* <span data-ttu-id="98b46-111">Az Azure Diagnostics infrastruktúra naplók</span><span class="sxs-lookup"><span data-stu-id="98b46-111">Azure Diagnostics infrastructure logs</span></span>

<span data-ttu-id="98b46-112">Ez a cikk bemutatja, hogyan Azure Diagnostics konfigurálása az Event Hubs végpontok közötti.</span><span class="sxs-lookup"><span data-stu-id="98b46-112">This article shows you how to configure Azure Diagnostics with Event Hubs from end to end.</span></span> <span data-ttu-id="98b46-113">Útmutató a következő gyakori forgatókönyvek esetén is ismerteti:</span><span class="sxs-lookup"><span data-stu-id="98b46-113">Guidance is also provided for the following common scenarios:</span></span>

* <span data-ttu-id="98b46-114">A naplók és metrikákat, az Event Hubs küldött testreszabása</span><span class="sxs-lookup"><span data-stu-id="98b46-114">How to customize the logs and metrics that get sent to Event Hubs</span></span>
* <span data-ttu-id="98b46-115">Minden környezetben konfigurációk módosítása</span><span class="sxs-lookup"><span data-stu-id="98b46-115">How to change configurations in each environment</span></span>
* <span data-ttu-id="98b46-116">Az Event Hubs adatfolyam adatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="98b46-116">How to view Event Hubs stream data</span></span>
* <span data-ttu-id="98b46-117">A kapcsolat hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="98b46-117">How to troubleshoot the connection</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="98b46-118">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="98b46-118">Prerequisites</span></span>
<span data-ttu-id="98b46-119">Event Hubs receieving Azure diagnosztikai adatait a Cloud Services, a virtuális gépek, a virtuálisgép-méretezési csoportok és a Service Fabric-től kezdve az Azure SDK 2.9 és a megfelelő Azure-eszközök Visual Studio támogatott.</span><span class="sxs-lookup"><span data-stu-id="98b46-119">Event Hubs receieving data from Azure Diagnostics is supported in Cloud Services, VMs, Virtual Machine Scale Sets, and Service Fabric starting in the Azure SDK 2.9 and the corresponding Azure Tools for Visual Studio.</span></span>

* <span data-ttu-id="98b46-120">Az Azure Diagnostics bővítmény 1.6 ([Azure SDK for .NET 2.9 vagy újabb](https://azure.microsoft.com/downloads/) céloz ez alapértelmezés szerint)</span><span class="sxs-lookup"><span data-stu-id="98b46-120">Azure Diagnostics extension 1.6 ([Azure SDK for .NET 2.9 or later](https://azure.microsoft.com/downloads/) targets this by default)</span></span>
* [<span data-ttu-id="98b46-121">A Visual Studio 2013 vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="98b46-121">Visual Studio 2013 or later</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* <span data-ttu-id="98b46-122">Egy alkalmazás használatával az Azure Diagnostics meglévő konfigurációk egy *.wadcfgx* fájl- és a következő módszerek egyikét:</span><span class="sxs-lookup"><span data-stu-id="98b46-122">Existing configurations of Azure Diagnostics in an application by using a *.wadcfgx* file and one of the following methods:</span></span>
  * <span data-ttu-id="98b46-123">A Visual Studio: [diagnosztika konfigurálása az Azure Felhőszolgáltatások és virtuális gépek](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)</span><span class="sxs-lookup"><span data-stu-id="98b46-123">Visual Studio: [Configuring Diagnostics for Azure Cloud Services and Virtual Machines](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)</span></span>
  * <span data-ttu-id="98b46-124">A Windows PowerShell: [a PowerShell használata Azure Cloud Services diagnosztika engedélyezése](../cloud-services/cloud-services-diagnostics-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="98b46-124">Windows PowerShell: [Enable diagnostics in Azure Cloud Services using PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md)</span></span>
* <span data-ttu-id="98b46-125">Esemény hubok névtér kiosztása a cikkenként [Bevezetés az Event Hubs használatába](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)</span><span class="sxs-lookup"><span data-stu-id="98b46-125">Event Hubs namespace provisioned per the article, [Get started with Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)</span></span>

## <a name="connect-azure-diagnostics-to-event-hubs-sink"></a><span data-ttu-id="98b46-126">Csatlakozás Azure Diagnostics Event Hubs fogadó</span><span class="sxs-lookup"><span data-stu-id="98b46-126">Connect Azure Diagnostics to Event Hubs sink</span></span>
<span data-ttu-id="98b46-127">Alapértelmezés szerint Azure Diagnostics mindig küld naplókat, valamint a metrikák egy Azure Storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="98b46-127">By default, Azure Diagnostics always sends logs and metrics to an Azure Storage account.</span></span> <span data-ttu-id="98b46-128">Egy alkalmazás is adatokat küldhet Event hubs egy új hozzáadásával **fogadók esetében** szakaszában a **PublicConfig** / **WadCfg** eleme a *. wadcfgx* fájlt.</span><span class="sxs-lookup"><span data-stu-id="98b46-128">An application may also send data to Event Hubs by adding a new **Sinks** section under the **PublicConfig** / **WadCfg** element of the *.wadcfgx* file.</span></span> <span data-ttu-id="98b46-129">A Visual Studio a *.wadcfgx* fájl található a következő elérési úton: **Felhőszolgáltatás-projekt** > **szerepkörök** > **() RoleName)** > **diagnostics.wadcfgx** fájlt.</span><span class="sxs-lookup"><span data-stu-id="98b46-129">In Visual Studio, the *.wadcfgx* file is stored in the following path: **Cloud Service Project** > **Roles** > **(RoleName)** > **diagnostics.wadcfgx** file.</span></span>

```xml
<SinksConfig>
  <Sink name="HotPath">
    <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" />
  </Sink>
</SinksConfig>
```
```JSON
"SinksConfig": {
    "Sink": [
        {
            "name": "HotPath",
            "EventHub": {
                "Url": "https://diags-mycompany-ns.servicebus.windows.net/diageventhub",
                "SharedAccessKeyName": "SendRule"
            }
        }
    ]
}
```

<span data-ttu-id="98b46-130">Ebben a példában az event hub URL-cím értéke az event hubs teljesen minősített névtere: az Event Hubs névtér + "/" + eseményközpont neveként.</span><span class="sxs-lookup"><span data-stu-id="98b46-130">In this example, the event hub URL is set to the fully qualified namespace of the event hub: Event Hubs namespace  + "/" + event hub name.</span></span>  

<span data-ttu-id="98b46-131">Az event hubs URL-cím jelenik meg a [Azure-portálon](http://go.microsoft.com/fwlink/?LinkID=213885) az Event Hubs irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="98b46-131">The event hub URL is displayed in the [Azure portal](http://go.microsoft.com/fwlink/?LinkID=213885) on the Event Hubs dashboard.</span></span>  

<span data-ttu-id="98b46-132">A **gyűjtése** mindaddig, amíg a ugyanazt az értéket használja a rendszer folyamatosan keresztül a konfigurációs fájl nevét állítható be bármilyen érvényes karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="98b46-132">The **Sink** name can be set to any valid string as long as the same value is used consistently throughout the config file.</span></span>

> [!NOTE]
> <span data-ttu-id="98b46-133">Előfordulhat, további felül, például a *applicationInsights* ebben a szakaszban konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="98b46-133">There may be additional sinks, such as *applicationInsights* configured in this section.</span></span> <span data-ttu-id="98b46-134">Az Azure Diagnostics lehetővé teszi, hogy egy vagy több mosdók kell megadni, ha mindegyik fogadó is deklarálva van a **PrivateConfig** szakasz.</span><span class="sxs-lookup"><span data-stu-id="98b46-134">Azure Diagnostics allows one or more sinks to be defined if each sink is also declared in the **PrivateConfig** section.</span></span>  
>
>

<span data-ttu-id="98b46-135">Az Event Hubs fogadó is kell deklarált és definiált a **PrivateConfig** szakasza a *.wadcfgx* konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="98b46-135">The Event Hubs sink must also be declared and defined in the **PrivateConfig** section of the *.wadcfgx* config file.</span></span>

```XML
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <StorageAccount name="{account name}" key="{account key}" endpoint="{optional storage endpoint}" />
  <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="{base64 encoded key}" />
</PrivateConfig>
```
```JSON
{
    "storageAccountName": "{account name}",
    "storageAccountKey": "{account key}",
    "storageAccountEndPoint": "{optional storage endpoint}",
    "EventHub": {
        "Url": "https://diags-mycompany-ns.servicebus.windows.net/diageventhub",
        "SharedAccessKeyName": "SendRule",
        "SharedAccessKey": "{base64 encoded key}"
    }
}
```

<span data-ttu-id="98b46-136">A `SharedAccessKeyName` értékének meg kell felelnie egy közös hozzáférésű Jogosultságkód (SAS) és egy házirendet, amely definiálva van a **Event Hubs** névtér.</span><span class="sxs-lookup"><span data-stu-id="98b46-136">The `SharedAccessKeyName` value must match a Shared Access Signature (SAS) key and policy that has been defined in the **Event Hubs** namespace.</span></span> <span data-ttu-id="98b46-137">Keresse meg az Event Hubs irányítópult a [Azure-portálon](https://manage.windowsazure.com), kattintson a **konfigurálása** lapot, és hozzon létre egy elnevezett házirendet (például "SendRule"), amely rendelkezik *küldése* engedélyek.</span><span class="sxs-lookup"><span data-stu-id="98b46-137">Browse to the Event Hubs dashboard in the [Azure portal](https://manage.windowsazure.com), click the **Configure** tab, and set up a named policy (for example, "SendRule") that has *Send* permissions.</span></span> <span data-ttu-id="98b46-138">A **StorageAccount** deklarálva van a **PrivateConfig**.</span><span class="sxs-lookup"><span data-stu-id="98b46-138">The **StorageAccount** is also declared in **PrivateConfig**.</span></span> <span data-ttu-id="98b46-139">Nincs szükség itt értékeket módosíthatja, ha működnek.</span><span class="sxs-lookup"><span data-stu-id="98b46-139">There is no need to change values here if they are working.</span></span> <span data-ttu-id="98b46-140">Ebben a példában azt az értéket üresen hagyja, ez az a jele, hogy egy alárendelt eszköz állítja be az értékeket.</span><span class="sxs-lookup"><span data-stu-id="98b46-140">In this example, we leave the values empty, which is a sign that a downstream asset will set the values.</span></span> <span data-ttu-id="98b46-141">Például a *ServiceConfiguration.Cloud.cscfg* környezet konfigurációs fájl beállítja a környezet megfelelő neveket és a kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="98b46-141">For example, the *ServiceConfiguration.Cloud.cscfg* environment configuration file sets the environment-appropriate names and keys.</span></span>  

> [!WARNING]
> <span data-ttu-id="98b46-142">Az Event Hubs SAS-kulcsot a egyszerű szövegként vannak tárolva a *.wadcfgx* fájlt.</span><span class="sxs-lookup"><span data-stu-id="98b46-142">The Event Hubs SAS key is stored in plain text in the *.wadcfgx* file.</span></span> <span data-ttu-id="98b46-143">Gyakran ezt a kulcsot rendszer ellenőrzi, hogy verziókövetési vagy érhető el a build Server eszközként, szükség szerint kell védeni.</span><span class="sxs-lookup"><span data-stu-id="98b46-143">Often, this key is checked in to source code control or is available as an asset in your build server, so you should protect it as appropriate.</span></span> <span data-ttu-id="98b46-144">Azt javasoljuk, hogy egy SAS-kulcsot használ itt a *csak küldése* engedélyeit, hogy egy rosszindulatú felhasználó írhat az event hubs, de nem hallgatni vagy az adatbázis felügyeletét.</span><span class="sxs-lookup"><span data-stu-id="98b46-144">We recommend that you use a SAS key here with *Send only* permissions so that a malicious user can write to the event hub, but not listen to it or manage it.</span></span>
>
>

## <a name="configure-azure-diagnostics-to-send-logs-and-metrics-to-event-hubs"></a><span data-ttu-id="98b46-145">Azure diagnosztikai naplók és a metrikák küldeni az Event Hubs konfigurálása</span><span class="sxs-lookup"><span data-stu-id="98b46-145">Configure Azure Diagnostics to send logs and metrics to Event Hubs</span></span>
<span data-ttu-id="98b46-146">Című szakaszban leírtaknak megfelelően korábban, az összes alapértelmezett és egyéni diagnosztikai adatainak, ez azt jelenti, metrikákat és a naplókat, automatikusan küldése Azure Storage beállított.</span><span class="sxs-lookup"><span data-stu-id="98b46-146">As discussed previously, all default and custom diagnostics data, that is, metrics and logs, is automatically sent to Azure Storage in the configured intervals.</span></span> <span data-ttu-id="98b46-147">Az Event Hubs és a további fogadó adja meg a gyökér vagy a levél csomópont az event hubs küldendő a hierarchiában.</span><span class="sxs-lookup"><span data-stu-id="98b46-147">With Event Hubs and any additional sink, you can specify any root or leaf node in the hierarchy to be sent to the event hub.</span></span> <span data-ttu-id="98b46-148">Ez magában foglalja az ETW-események, a teljesítményszámlálók, a Windows eseménynaplóiban keresse meg és az alkalmazásnaplók.</span><span class="sxs-lookup"><span data-stu-id="98b46-148">This includes ETW events, performance counters, Windows event logs, and application logs.</span></span>   

<span data-ttu-id="98b46-149">Fontos figyelembe venni, hogy hány adatpontok ténylegesen átviszi az Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="98b46-149">It is important to consider how many data points should actually be transferred to Event Hubs.</span></span> <span data-ttu-id="98b46-150">Általában a fejlesztők adatátvitelt kis késleltetésű közbeni elérési útja, amely kell használni, és gyorsan értelmezi.</span><span class="sxs-lookup"><span data-stu-id="98b46-150">Typically, developers transfer low-latency hot-path data that must be consumed and interpreted quickly.</span></span> <span data-ttu-id="98b46-151">Riasztások vagy az automatikus skálázási szabályok figyelő rendszerek példák.</span><span class="sxs-lookup"><span data-stu-id="98b46-151">Systems that monitor alerts or autoscale rules are examples.</span></span> <span data-ttu-id="98b46-152">A fejlesztők is előfordulhat, hogy egy másik elemzési tároló konfigurálása és keressen rá az áruházban – például Azure Stream Analytics, Elasticsearch, egy egyéni figyelési rendszer vagy más kedvenc figyelési rendszer.</span><span class="sxs-lookup"><span data-stu-id="98b46-152">A developer might also configure an alternate analysis store or search store -- for example, Azure Stream Analytics, Elasticsearch, a custom monitoring system, or a favorite monitoring system from others.</span></span>

<span data-ttu-id="98b46-153">Az alábbiakban néhány példa konfigurációkra.</span><span class="sxs-lookup"><span data-stu-id="98b46-153">The following are some example configurations.</span></span>

```xml
<PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
</PerformanceCounters>
```
```JSON
"PerformanceCounters": {
    "scheduledTransferPeriod": "PT1M",
    "sinks": "HotPath",
    "PerformanceCounterConfiguration": [
        {
            "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Memory\\Available MBytes",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
            "sampleRate": "PT3M"
        }
    ]
}
```

<span data-ttu-id="98b46-154">A fenti példában a fogadó és a szülő érvényes **PerformanceCounters** a hierarchiában, ami azt jelenti, hogy minden gyermek csomópont **PerformanceCounters** Event Hubs kapnak.</span><span class="sxs-lookup"><span data-stu-id="98b46-154">In the above example, the sink is applied to the parent **PerformanceCounters** node in the hierarchy, which means all child **PerformanceCounters** will be sent to Event Hubs.</span></span>  

```xml
<PerformanceCounters scheduledTransferPeriod="PT1M">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" sinks="HotPath" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" sinks="HotPath"/>
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" sinks="HotPath"/>
</PerformanceCounters>
```
```JSON
"PerformanceCounters": {
    "scheduledTransferPeriod": "PT1M",
    "PerformanceCounterConfiguration": [
        {
            "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
            "sampleRate": "PT3M",
            "sinks": "HotPath"
        },
        {
            "counterSpecifier": "\\Memory\\Available MBytes",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\ASP.NET\\Requests Rejected",
            "sampleRate": "PT3M",
            "sinks": "HotPath"
        },
        {
            "counterSpecifier": "\\ASP.NET\\Requests Queued",
            "sampleRate": "PT3M",
            "sinks": "HotPath"
        }
    ]
}
```

<span data-ttu-id="98b46-155">Az előző példában a gyűjtő csak három számláló alkalmazzák: **kérései**, **elutasítja kérelmeket**, és **processzoridő százalékos**.</span><span class="sxs-lookup"><span data-stu-id="98b46-155">In the previous example, the sink is applied to only three counters: **Requests Queued**, **Requests Rejected**, and **% Processor time**.</span></span>  

<span data-ttu-id="98b46-156">A következő példa bemutatja, hogyan egy fejlesztő korlátozhatja a fontos metrikák e szolgáltatás állapotát a használt elküldött adatok mennyisége.</span><span class="sxs-lookup"><span data-stu-id="98b46-156">The following example shows how a developer can limit the amount of sent data to be the critical metrics that are used for this service’s health.</span></span>  

```XML
<Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
```
```JSON
"Logs": {
    "scheduledTransferPeriod": "PT1M",
    "scheduledTransferLogLevelFilter": "Error",
    "sinks": "HotPath"
}
```

<span data-ttu-id="98b46-157">Ebben a példában a gyűjtő naplók alkalmazott, és csak a hiba szintű nyomkövetési szűrve van.</span><span class="sxs-lookup"><span data-stu-id="98b46-157">In this example, the sink is applied to logs and is filtered only to error level trace.</span></span>

## <a name="deploy-and-update-a-cloud-services-application-and-diagnostics-config"></a><span data-ttu-id="98b46-158">Központi telepítése és a Cloud Services alkalmazás- és diagnosztikai konfiguráció frissítése</span><span class="sxs-lookup"><span data-stu-id="98b46-158">Deploy and update a Cloud Services application and diagnostics config</span></span>
<span data-ttu-id="98b46-159">A Visual Studio az alkalmazás és az Event Hubs fogadó konfigurációs központi telepítése a legegyszerűbb útvonalat biztosít.</span><span class="sxs-lookup"><span data-stu-id="98b46-159">Visual Studio provides the easiest path to deploy the application and Event Hubs sink configuration.</span></span> <span data-ttu-id="98b46-160">Megtekintheti és szerkesztheti a fájlt, nyissa meg a *.wadcfgx* fájlt a Visual Studio, szerkesztheti és mentheti.</span><span class="sxs-lookup"><span data-stu-id="98b46-160">To view and edit the file, open the *.wadcfgx* file in Visual Studio, edit it, and save it.</span></span> <span data-ttu-id="98b46-161">Az elérési út **Felhőszolgáltatás-projekt** > **szerepkörök** > **(RoleName)** > **diagnostics.wadcfgx**.</span><span class="sxs-lookup"><span data-stu-id="98b46-161">The path is **Cloud Service Project** > **Roles** > **(RoleName)** > **diagnostics.wadcfgx**.</span></span>  

<span data-ttu-id="98b46-162">Ezen a ponton az összes telepítési és telepítési műveletek frissítése a Visual Studio, a Visual Studio Team System, és minden parancsokkal és parancsprogramokkal MSBuild és -felhasználási alapuló a **/t: közzététele** cél közé tartozik a *.wadcfgx* a csomagolási folyamatban.</span><span class="sxs-lookup"><span data-stu-id="98b46-162">At this point, all deployment and deployment update actions in Visual Studio, Visual Studio Team System, and all commands or scripts that are based on MSBuild and use the **/t:publish** target include the *.wadcfgx* in the packaging process.</span></span> <span data-ttu-id="98b46-163">Emellett központi telepítések és frissítések a fájl Azure segítségével telepítheti a megfelelő Azure diagnosztikai ügynök bővítményt a virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="98b46-163">In addition, deployments and updates deploy the file to Azure by using the appropriate Azure Diagnostics agent extension on your VMs.</span></span>

<span data-ttu-id="98b46-164">Az alkalmazás és az Azure diagnosztikai konfiguráció telepítése után az irányítópulton az event hubs tevékenység azonnal látni fogja.</span><span class="sxs-lookup"><span data-stu-id="98b46-164">After you deploy the application and Azure Diagnostics configuration, you will immediately see activity in the dashboard of the event hub.</span></span> <span data-ttu-id="98b46-165">Ez azt jelzi, hogy készen áll áthelyezése a közbeni elérési adatok megtekintése az Ön által választott figyelő ügyfél vagy elemző eszközben.</span><span class="sxs-lookup"><span data-stu-id="98b46-165">This indicates that you're ready to move on to viewing the hot-path data in the listener client or analysis tool of your choice.</span></span>  

<span data-ttu-id="98b46-166">Az alábbi ábrán az Event Hubs irányítópult jeleníti meg, kifogástalan küldése az event hubs némi várakozás után 23 óra kezdő diagnosztikai adatok.</span><span class="sxs-lookup"><span data-stu-id="98b46-166">In the following figure, the Event Hubs dashboard shows healthy sending of diagnostics data to the event hub starting sometime after 11 PM.</span></span> <span data-ttu-id="98b46-167">Ha ez az alkalmazás lett telepítve a frissített *.wadcfgx* fájlt, és a fogadó lett megfelelően konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="98b46-167">That's when the application was deployed with an updated *.wadcfgx* file, and the sink was configured properly.</span></span>

![][0]  

> [!NOTE]
> <span data-ttu-id="98b46-168">Amikor módosításokat az Azure Diagnostics konfigurációs fájljában (.wadcfgx), javasoljuk, hogy leküldéses a frissítések a teljes alkalmazás, valamint a konfigurációs Visual Studio közzététel, vagy a Windows PowerShell-parancsfájl használatával.</span><span class="sxs-lookup"><span data-stu-id="98b46-168">When you make updates to the Azure Diagnostics config file (.wadcfgx), it's recommended that you push the updates to the entire application as well as the configuration by using either Visual Studio publishing, or a Windows PowerShell script.</span></span>  
>
>

## <a name="view-hot-path-data"></a><span data-ttu-id="98b46-169">Közbeni elérési adatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="98b46-169">View hot-path data</span></span>
<span data-ttu-id="98b46-170">Korábban bemutatott, nincsenek sok használati esetek figyelését, és az Event Hubs adatok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="98b46-170">As discussed previously, there are many use cases for listening to and processing Event Hubs data.</span></span>

<span data-ttu-id="98b46-171">Egy egyszerű megoldás, az event hubs figyelését, valamint a kimeneti adatfolyamba nyomtatása egy kis teszt Konzolalkalmazás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="98b46-171">One simple approach is to create a small test console application to listen to the event hub and print the output stream.</span></span> <span data-ttu-id="98b46-172">Az alábbi kód, amely részletesen kifejtett elhelyezheti [Bevezetés az Event Hubs használatába](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)), egy konzolalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="98b46-172">You can place the following code, which is explained in more detail in [Get started with Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)), in a console application.</span></span>  

<span data-ttu-id="98b46-173">Vegye figyelembe, hogy a konzol alkalmazásnak tartalmaznia kell a [esemény processzor állomás NuGet-csomag](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).</span><span class="sxs-lookup"><span data-stu-id="98b46-173">Note that the console application must include the [Event Processor Host NuGet package](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).</span></span>  

<span data-ttu-id="98b46-174">Ne felejtse el lecserélni az értéket a csúcsos zárójelek a **fő** függvény erőforrások értékekkel.</span><span class="sxs-lookup"><span data-stu-id="98b46-174">Remember to replace the values in angle brackets in the **Main** function with values for your resources.</span></span>   

```csharp
//Console application code for EventHub test client
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace EventHubListener
{
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                    Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                        context.Lease.PartitionId, data));

                foreach (var x in eventData.Properties)
                {
                    Console.WriteLine(string.Format("    {0} = {1}", x.Key, x.Value));
                }
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string eventHubConnectionString = "Endpoint= <your connection string>”
            string eventHubName = "<Event hub name>";
            string storageAccountName = "<Storage account name>";
            string storageAccountKey = "<Storage account key>”;
            string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

            string eventProcessorHostName = Guid.NewGuid().ToString();
            EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
            Console.WriteLine("Registering EventProcessor...");
            var options = new EventProcessorOptions();
            options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
            eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

            Console.WriteLine("Receiving. Press enter key to stop worker.");
            Console.ReadLine();
            eventProcessorHost.UnregisterEventProcessorAsync().Wait();
        }
    }
}
```

## <a name="troubleshoot-event-hubs-sinks"></a><span data-ttu-id="98b46-175">Az Event Hubs mosdók hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="98b46-175">Troubleshoot Event Hubs sinks</span></span>
* <span data-ttu-id="98b46-176">Az event hubs nem szerepelnek a bejövő vagy kimenő eseményhez kapcsolódó tevékenység várt módon.</span><span class="sxs-lookup"><span data-stu-id="98b46-176">The event hub does not show incoming or outgoing event activity as expected.</span></span>

    <span data-ttu-id="98b46-177">Ellenőrizze, hogy az eseményközpont sikeresen lett kiépítve.</span><span class="sxs-lookup"><span data-stu-id="98b46-177">Check that your event hub is successfully provisioned.</span></span> <span data-ttu-id="98b46-178">Az összes Kapcsolatinformáció a **PrivateConfig** szakasza *.wadcfgx* az erőforrás értékének egyeznie kell, mint a portálon.</span><span class="sxs-lookup"><span data-stu-id="98b46-178">All connection info in the **PrivateConfig** section of *.wadcfgx* must match the values of your resource as seen in the portal.</span></span> <span data-ttu-id="98b46-179">Győződjön meg arról, hogy rendelkezik-e a biztonsági Társítások házirend lett meghatározva (a példában szereplő "SendRule" jelöli), valamint a portál *küldése* engedélyt.</span><span class="sxs-lookup"><span data-stu-id="98b46-179">Make sure that you have a SAS policy defined ("SendRule" in the example) in the portal and that *Send* permission is granted.</span></span>  
* <span data-ttu-id="98b46-180">Egy adott frissítés után az event hubs eseményközponthoz már nem bejövő vagy kimenő eseményhez kapcsolódó tevékenység jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="98b46-180">After an update, the event hub no longer shows incoming or outgoing event activity.</span></span>

    <span data-ttu-id="98b46-181">Először ellenőrizze, hogy a event hub és konfigurációs adatokat megfelelő, korábban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="98b46-181">First, make sure that the event hub and configuration info is correct as explained previously.</span></span> <span data-ttu-id="98b46-182">Egyes esetekben a **PrivateConfig** az egy központi telepítési alaphelyzetbe áll.</span><span class="sxs-lookup"><span data-stu-id="98b46-182">Sometimes the **PrivateConfig** is reset in a deployment update.</span></span> <span data-ttu-id="98b46-183">A javasolt javítás szükséges összes módosításokat egy *.wadcfgx* a projektet, majd a teljes alkalmazás frissítés leküldéses.</span><span class="sxs-lookup"><span data-stu-id="98b46-183">The recommended fix is to make all changes to *.wadcfgx* in the project and then push a complete application update.</span></span> <span data-ttu-id="98b46-184">Ha ez nem lehetséges, győződjön meg arról, hogy a diagnosztika frissítés leküldéses értesítések teljes **PrivateConfig** , amely tartalmazza az SAS-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="98b46-184">If this is not possible, make sure that the diagnostics update pushes a complete **PrivateConfig** that includes the SAS key.</span></span>  
* <span data-ttu-id="98b46-185">A javaslatok próbáltam, és az event hubs továbbra sem működik.</span><span class="sxs-lookup"><span data-stu-id="98b46-185">I tried the suggestions, and the event hub is still not working.</span></span>

    <span data-ttu-id="98b46-186">Nézzük meg az Azure Storage táblázatban maga Azure diagnosztikai naplók és a hibákat tartalmazó: **WADDiagnosticInfrastructureLogsTable**.</span><span class="sxs-lookup"><span data-stu-id="98b46-186">Try looking in the Azure Storage table that contains logs and errors for Azure Diagnostics itself: **WADDiagnosticInfrastructureLogsTable**.</span></span> <span data-ttu-id="98b46-187">Egy elem egy eszközzel, mint [Azure Tártallózó](http://www.storageexplorer.com) csatlakozni ehhez a tárfiókhoz, ebben a táblázatban megtekintheti, és adja hozzá a lekérdezés az időbélyegzési az elmúlt 24 órában.</span><span class="sxs-lookup"><span data-stu-id="98b46-187">One option is to use a tool such as [Azure Storage Explorer](http://www.storageexplorer.com) to connect to this storage account, view this table, and add a query for TimeStamp in the last 24 hours.</span></span> <span data-ttu-id="98b46-188">Az eszköz segítségével exportálja egy CSV-fájlt, majd nyissa meg például a Microsoft Excel alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="98b46-188">You can use the tool to export a .csv file and open it in an application such as Microsoft Excel.</span></span> <span data-ttu-id="98b46-189">Excel segítségével egyszerűen-kártya karakterláncokat, például keresni **EventHubs**, hogy milyen hibát jelez.</span><span class="sxs-lookup"><span data-stu-id="98b46-189">Excel makes it easy to search for calling-card strings, such as **EventHubs**, to see what error is reported.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="98b46-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="98b46-190">Next steps</span></span>
<span data-ttu-id="98b46-191">• [További információ az Event Hubs](https://azure.microsoft.com/services/event-hubs/)</span><span class="sxs-lookup"><span data-stu-id="98b46-191">•    [Learn more about Event Hubs](https://azure.microsoft.com/services/event-hubs/)</span></span>

## <a name="appendix-complete-azure-diagnostics-configuration-file-wadcfgx-example"></a><span data-ttu-id="98b46-192">A függelék: Végezze el az Azure Diagnostics konfigurációs fájl (.wadcfgx) – Példa</span><span class="sxs-lookup"><span data-stu-id="98b46-192">Appendix: Complete Azure Diagnostics configuration file (.wadcfgx) example</span></span>
```xml
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
        <Directories scheduledTransferPeriod="PT1M">
          <IISLogs containerName="wad-iis-logfiles" />
          <FailedRequestLogs containerName="wad-failedrequestlogs" />
        </Directories>
        <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
        </PerformanceCounters>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*" />
        </WindowsEventLog>
        <CrashDumps>
          <CrashDumpConfiguration processName="WaIISHost.exe" />
          <CrashDumpConfiguration processName="WaWorkerHost.exe" />
          <CrashDumpConfiguration processName="w3wp.exe" />
        </CrashDumps>
        <Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
      </DiagnosticMonitorConfiguration>
      <SinksConfig>
        <Sink name="HotPath">
          <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" />
        </Sink>
        <Sink name="applicationInsights">
          <ApplicationInsights />
          <Channels>
            <Channel logLevel="Error" name="errors" />
          </Channels>
        </Sink>
      </SinksConfig>
    </WadCfg>
    <StorageAccount>ACCOUNT_NAME</StorageAccount>
  </PublicConfig>
  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="{account name}" key="{account key}" endpoint="{storage endpoint}" />
    <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" SharedAccessKey="YOUR_KEY_HERE" />
  </PrivateConfig>
  <IsEnabled>true</IsEnabled>
</DiagnosticsConfiguration>
```

<span data-ttu-id="98b46-193">A kiegészítő *ServiceConfiguration.Cloud.cscfg* az ebben a példában a következőhöz hasonló.</span><span class="sxs-lookup"><span data-stu-id="98b46-193">The complementary *ServiceConfiguration.Cloud.cscfg* for this example looks like the following.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="MyFixItCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="MyFixIt.WorkerRole">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="YOUR_CONNECTION_STRING" />
    </ConfigurationSettings>
  </Role>
</ServiceConfiguration>
```

<span data-ttu-id="98b46-194">A virtuális gépek egyenértékű alapú Json-beállítások a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="98b46-194">Equivalent Json based settings for virtual machines is as follows:</span></span>
```JSON
"settings": {
    "WadCfg": {
        "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": 4096,
            "sinks": "applicationInsights.errors",
            "DiagnosticInfrastructureLogs": {
                "scheduledTransferLogLevelFilter": "Error"
            },
            "Directories": {
                "scheduledTransferPeriod": "PT1M",
                "IISLogs": {
                    "containerName": "wad-iis-logfiles"
                },
                "FailedRequestLogs": {
                    "containerName": "wad-failedrequestlogs"
                }
            },
            "PerformanceCounters": {
                "scheduledTransferPeriod": "PT1M",
                "sinks": "HotPath",
                "PerformanceCounterConfiguration": [
                    {
                        "counterSpecifier": "\\Memory\\Available MBytes",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\Web Service(_Total)\\Bytes Total/Sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Requests/Sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Errors Total/Sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET\\Requests Queued",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET\\Requests Rejected",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                        "sampleRate": "PT3M"
                    }
                ]
            },
            "WindowsEventLog": {
                "scheduledTransferPeriod": "PT1M",
                "DataSource": [
                    {
                        "name": "Application!*"
                    }
                ]
            },
            "Logs": {
                "scheduledTransferPeriod": "PT1M",
                "scheduledTransferLogLevelFilter": "Error",
                "sinks": "HotPath"
            }
        },
        "SinksConfig": {
            "Sink": [
                {
                    "name": "HotPath",
                    "EventHub": {
                        "Url": "https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py",
                        "SharedAccessKeyName": "SendRule"
                    }
                },
                {
                    "name": "applicationInsights",
                    "ApplicationInsights": "",
                    "Channels": {
                        "Channel": [
                            {
                                "logLevel": "Error",
                                "name": "errors"
                            }
                        ]
                    }
                }
            ]
        }
    },
    "StorageAccount": "{account name}"
}


"protectedSettings": {
    "storageAccountName": "{account name}",
    "storageAccountKey": "{account key}",
    "storageAccountEndPoint": "{storage endpoint}",
    "EventHub": {
        "Url": "https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py",
        "SharedAccessKeyName": "SendRule",
        "SharedAccessKey": "YOUR_KEY_HERE"
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="98b46-195">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="98b46-195">Next steps</span></span>
<span data-ttu-id="98b46-196">Az alábbi webhelyeken további információt talál az Event Hubsról:</span><span class="sxs-lookup"><span data-stu-id="98b46-196">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="98b46-197">Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="98b46-197">Event Hubs overview</span></span>](../event-hubs/event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="98b46-198">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="98b46-198">Create an event hub</span></span>](../event-hubs/event-hubs-create.md)
* [<span data-ttu-id="98b46-199">Event Hubs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="98b46-199">Event Hubs FAQ</span></span>](../event-hubs/event-hubs-faq.md)

<!-- Images. -->
[0]: ../event-hubs/media/event-hubs-streaming-azure-diags-data/dashboard.png
