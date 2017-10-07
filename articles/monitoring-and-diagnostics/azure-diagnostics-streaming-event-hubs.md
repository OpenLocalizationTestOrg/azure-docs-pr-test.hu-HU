---
title: "aaaStreaming Azure diagnosztikai adatokat hello gyors elérési út az Event Hubs használatával |} Microsoft Docs"
description: "Az Event Hubs konfigurálása Azure Diagnostics végén tooend, beleértve a közös forgatókönyvre vonatkozó útmutatást."
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
ms.openlocfilehash: a2528ddd0688d1c23a8631e769ca016dd79e4159
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="streaming-azure-diagnostics-data-in-hello-hot-path-by-using-event-hubs"></a><span data-ttu-id="cd114-103">Adatfolyam-továbbítási Azure diagnosztikai adatokat hello gyors elérési út az Event Hubs használatával</span><span class="sxs-lookup"><span data-stu-id="cd114-103">Streaming Azure Diagnostics data in hello hot path by using Event Hubs</span></span>
<span data-ttu-id="cd114-104">Az Azure Diagnostics rugalmas lehetőségeket biztosítja a toocollect metrikákat, és naplózza a felhőalapú szolgáltatások virtuális gépek (VM) és átviteli eredmények tooAzure tároló.</span><span class="sxs-lookup"><span data-stu-id="cd114-104">Azure Diagnostics provides flexible ways toocollect metrics and logs from cloud services virtual machines (VMs) and transfer results tooAzure Storage.</span></span> <span data-ttu-id="cd114-105">Hello 2016. március (SDK 2.9) időkereten belül-től kezdődően küldjön diagnosztikai toocustom adatforrások, gyors útvonal adatok átviteléhez az másodpercben használatával [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="cd114-105">Starting in hello March 2016 (SDK 2.9) time frame, you can send Diagnostics toocustom data sources and transfer hot path data in seconds by using [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/).</span></span>

<span data-ttu-id="cd114-106">Támogatott adattípusok a következők:</span><span class="sxs-lookup"><span data-stu-id="cd114-106">Supported data types include:</span></span>

* <span data-ttu-id="cd114-107">A Windows esemény-nyomkövetés (ETW) eseményei</span><span class="sxs-lookup"><span data-stu-id="cd114-107">Event Tracing for Windows (ETW) events</span></span>
* <span data-ttu-id="cd114-108">Teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="cd114-108">Performance counters</span></span>
* <span data-ttu-id="cd114-109">Windows-Eseménynapló</span><span class="sxs-lookup"><span data-stu-id="cd114-109">Windows event logs</span></span>
* <span data-ttu-id="cd114-110">Alkalmazás-naplók</span><span class="sxs-lookup"><span data-stu-id="cd114-110">Application logs</span></span>
* <span data-ttu-id="cd114-111">Az Azure Diagnostics infrastruktúra naplók</span><span class="sxs-lookup"><span data-stu-id="cd114-111">Azure Diagnostics infrastructure logs</span></span>

<span data-ttu-id="cd114-112">Ez a cikk bemutatja, hogyan tooconfigure Azure Diagnostics az Event Hubs az end tooend.</span><span class="sxs-lookup"><span data-stu-id="cd114-112">This article shows you how tooconfigure Azure Diagnostics with Event Hubs from end tooend.</span></span> <span data-ttu-id="cd114-113">Emellett útmutatást a következő gyakori helyzetek hello:</span><span class="sxs-lookup"><span data-stu-id="cd114-113">Guidance is also provided for hello following common scenarios:</span></span>

* <span data-ttu-id="cd114-114">Hogyan naplózza az toocustomize hello és metrikákat, küldött tooEvent hubok</span><span class="sxs-lookup"><span data-stu-id="cd114-114">How toocustomize hello logs and metrics that get sent tooEvent Hubs</span></span>
* <span data-ttu-id="cd114-115">Hogyan toochange beállításai minden környezetben</span><span class="sxs-lookup"><span data-stu-id="cd114-115">How toochange configurations in each environment</span></span>
* <span data-ttu-id="cd114-116">Hogyan tooview Event Hubs adatok folyamatos átviteléhez</span><span class="sxs-lookup"><span data-stu-id="cd114-116">How tooview Event Hubs stream data</span></span>
* <span data-ttu-id="cd114-117">Hogyan tootroubleshoot hello kapcsolat</span><span class="sxs-lookup"><span data-stu-id="cd114-117">How tootroubleshoot hello connection</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="cd114-118">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cd114-118">Prerequisites</span></span>
<span data-ttu-id="cd114-119">Event Hubs receieving adatait Azure Diagnostics Cloud Services, a virtuális gépek, a virtuálisgép-méretezési csoportok és a Service Fabric hello Azure SDK 2.9 és a Visual Studio Azure eszközök megfelelő hello kezdve támogatott.</span><span class="sxs-lookup"><span data-stu-id="cd114-119">Event Hubs receieving data from Azure Diagnostics is supported in Cloud Services, VMs, Virtual Machine Scale Sets, and Service Fabric starting in hello Azure SDK 2.9 and hello corresponding Azure Tools for Visual Studio.</span></span>

* <span data-ttu-id="cd114-120">Az Azure Diagnostics bővítmény 1.6 ([Azure SDK for .NET 2.9 vagy újabb](https://azure.microsoft.com/downloads/) céloz ez alapértelmezés szerint)</span><span class="sxs-lookup"><span data-stu-id="cd114-120">Azure Diagnostics extension 1.6 ([Azure SDK for .NET 2.9 or later](https://azure.microsoft.com/downloads/) targets this by default)</span></span>
* [<span data-ttu-id="cd114-121">A Visual Studio 2013 vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="cd114-121">Visual Studio 2013 or later</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* <span data-ttu-id="cd114-122">Egy alkalmazás használatával az Azure Diagnostics meglévő konfigurációk egy *.wadcfgx* fájl- és hello a következő módszerek egyikét:</span><span class="sxs-lookup"><span data-stu-id="cd114-122">Existing configurations of Azure Diagnostics in an application by using a *.wadcfgx* file and one of hello following methods:</span></span>
  * <span data-ttu-id="cd114-123">A Visual Studio: [diagnosztika konfigurálása az Azure Felhőszolgáltatások és virtuális gépek](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)</span><span class="sxs-lookup"><span data-stu-id="cd114-123">Visual Studio: [Configuring Diagnostics for Azure Cloud Services and Virtual Machines](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)</span></span>
  * <span data-ttu-id="cd114-124">A Windows PowerShell: [a PowerShell használata Azure Cloud Services diagnosztika engedélyezése](../cloud-services/cloud-services-diagnostics-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="cd114-124">Windows PowerShell: [Enable diagnostics in Azure Cloud Services using PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md)</span></span>
* <span data-ttu-id="cd114-125">Esemény hubok névtér hello cikkenként kiépített [Bevezetés az Event Hubs használatába](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)</span><span class="sxs-lookup"><span data-stu-id="cd114-125">Event Hubs namespace provisioned per hello article, [Get started with Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)</span></span>

## <a name="connect-azure-diagnostics-tooevent-hubs-sink"></a><span data-ttu-id="cd114-126">Csatlakozás Azure Diagnostics tooEvent hubok fogadó</span><span class="sxs-lookup"><span data-stu-id="cd114-126">Connect Azure Diagnostics tooEvent Hubs sink</span></span>
<span data-ttu-id="cd114-127">Alapértelmezés szerint Azure Diagnostics mindig a küldi naplók és a metrikák tooan Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="cd114-127">By default, Azure Diagnostics always sends logs and metrics tooan Azure Storage account.</span></span> <span data-ttu-id="cd114-128">Egy alkalmazás egy új hozzáadásával is küldhetünk adatok tooEvent hubok **fogadók esetében** hello szakasza **PublicConfig** / **WadCfg** hello eleme*.wadcfgx* fájlt.</span><span class="sxs-lookup"><span data-stu-id="cd114-128">An application may also send data tooEvent Hubs by adding a new **Sinks** section under hello **PublicConfig** / **WadCfg** element of hello *.wadcfgx* file.</span></span> <span data-ttu-id="cd114-129">A Visual Studio hello *.wadcfgx* fájl elérési útja a következő hello tárolja: **Felhőszolgáltatás-projekt** > **szerepkörök** > **() RoleName)** > **diagnostics.wadcfgx** fájlt.</span><span class="sxs-lookup"><span data-stu-id="cd114-129">In Visual Studio, hello *.wadcfgx* file is stored in hello following path: **Cloud Service Project** > **Roles** > **(RoleName)** > **diagnostics.wadcfgx** file.</span></span>

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

<span data-ttu-id="cd114-130">Ebben a példában hello eseményközpont URL-cím értéke toohello teljesen minősített hello az event hubs névtér: az Event Hubs névtér + "/" + eseményközpont neveként.</span><span class="sxs-lookup"><span data-stu-id="cd114-130">In this example, hello event hub URL is set toohello fully qualified namespace of hello event hub: Event Hubs namespace  + "/" + event hub name.</span></span>  

<span data-ttu-id="cd114-131">URL-cím jelenik meg hello hello eseményközpont [Azure-portálon](http://go.microsoft.com/fwlink/?LinkID=213885) hello Event Hubs irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="cd114-131">hello event hub URL is displayed in hello [Azure portal](http://go.microsoft.com/fwlink/?LinkID=213885) on hello Event Hubs dashboard.</span></span>  

<span data-ttu-id="cd114-132">Hello **gyűjtése** neve tooany érvényes karakterlánc beállítható, mindaddig, amíg hello ugyanazt az értéket használja a rendszer folyamatosan keresztül hello konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="cd114-132">hello **Sink** name can be set tooany valid string as long as hello same value is used consistently throughout hello config file.</span></span>

> [!NOTE]
> <span data-ttu-id="cd114-133">Előfordulhat, további felül, például a *applicationInsights* ebben a szakaszban konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="cd114-133">There may be additional sinks, such as *applicationInsights* configured in this section.</span></span> <span data-ttu-id="cd114-134">Az Azure Diagnostics lehetővé teszi, hogy egy vagy több fogadók esetében meg, ha mindegyik fogadó is deklarálva van hello toobe **PrivateConfig** szakasz.</span><span class="sxs-lookup"><span data-stu-id="cd114-134">Azure Diagnostics allows one or more sinks toobe defined if each sink is also declared in hello **PrivateConfig** section.</span></span>  
>
>

<span data-ttu-id="cd114-135">hello Event Hubs fogadó is kell deklarált és definiált hello **PrivateConfig** hello szakasza *.wadcfgx* konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="cd114-135">hello Event Hubs sink must also be declared and defined in hello **PrivateConfig** section of hello *.wadcfgx* config file.</span></span>

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

<span data-ttu-id="cd114-136">Hello `SharedAccessKeyName` értékének meg kell felelnie egy közös hozzáférésű Jogosultságkód (SAS) és egy házirendet, amely hello definiáltak **Event Hubs** névtér.</span><span class="sxs-lookup"><span data-stu-id="cd114-136">hello `SharedAccessKeyName` value must match a Shared Access Signature (SAS) key and policy that has been defined in hello **Event Hubs** namespace.</span></span> <span data-ttu-id="cd114-137">Keresse meg az Event Hubs irányítópult toohello hello [Azure-portálon](https://manage.windowsazure.com), hello kattintson **konfigurálása** lapot, és hozzon létre egy elnevezett házirendet (például "SendRule"), amely rendelkezik *küldése* engedélyek.</span><span class="sxs-lookup"><span data-stu-id="cd114-137">Browse toohello Event Hubs dashboard in hello [Azure portal](https://manage.windowsazure.com), click hello **Configure** tab, and set up a named policy (for example, "SendRule") that has *Send* permissions.</span></span> <span data-ttu-id="cd114-138">Hello **StorageAccount** deklarálva van a **PrivateConfig**.</span><span class="sxs-lookup"><span data-stu-id="cd114-138">hello **StorageAccount** is also declared in **PrivateConfig**.</span></span> <span data-ttu-id="cd114-139">Nincs szükség az itt toochange értékek Ha működnek.</span><span class="sxs-lookup"><span data-stu-id="cd114-139">There is no need toochange values here if they are working.</span></span> <span data-ttu-id="cd114-140">Ebben a példában a Microsoft hello értékek üresen hagyja, vagyis a jele, hogy egy alárendelt eszköz állítja be hello értékek.</span><span class="sxs-lookup"><span data-stu-id="cd114-140">In this example, we leave hello values empty, which is a sign that a downstream asset will set hello values.</span></span> <span data-ttu-id="cd114-141">Például hello *ServiceConfiguration.Cloud.cscfg* környezet konfigurációs fájl beállítja hello környezet megfelelő neveket és a kulcsok.</span><span class="sxs-lookup"><span data-stu-id="cd114-141">For example, hello *ServiceConfiguration.Cloud.cscfg* environment configuration file sets hello environment-appropriate names and keys.</span></span>  

> [!WARNING]
> <span data-ttu-id="cd114-142">hello Event Hubs SAS-kulcsot a hello egyszerű szövegként vannak tárolva *.wadcfgx* fájlt.</span><span class="sxs-lookup"><span data-stu-id="cd114-142">hello Event Hubs SAS key is stored in plain text in hello *.wadcfgx* file.</span></span> <span data-ttu-id="cd114-143">Gyakran ezt a kulcsot be van jelölve, a toosource ellenőrző vagy a build Server eszközként elérhető, megfelelő módon kell védeni.</span><span class="sxs-lookup"><span data-stu-id="cd114-143">Often, this key is checked in toosource code control or is available as an asset in your build server, so you should protect it as appropriate.</span></span> <span data-ttu-id="cd114-144">Azt javasoljuk, hogy egy SAS-kulcsot használ itt a *csak küldése* engedélyeit, hogy egy rosszindulatú felhasználó írhat toohello eseményközpont, de nem tooit figyelésére vagy az adatbázis felügyeletét.</span><span class="sxs-lookup"><span data-stu-id="cd114-144">We recommend that you use a SAS key here with *Send only* permissions so that a malicious user can write toohello event hub, but not listen tooit or manage it.</span></span>
>
>

## <a name="configure-azure-diagnostics-toosend-logs-and-metrics-tooevent-hubs"></a><span data-ttu-id="cd114-145">Konfigurálja az Azure Diagnostics toosend naplók és a metrikák tooEvent hubok</span><span class="sxs-lookup"><span data-stu-id="cd114-145">Configure Azure Diagnostics toosend logs and metrics tooEvent Hubs</span></span>
<span data-ttu-id="cd114-146">Című szakaszban leírtaknak megfelelően korábban, az összes alapértelmezett és egyéni diagnosztikai adatainak, ez azt jelenti, metrikákat és a naplókat, automatikusan küldött tooAzure tárolási konfigurált hello időközök.</span><span class="sxs-lookup"><span data-stu-id="cd114-146">As discussed previously, all default and custom diagnostics data, that is, metrics and logs, is automatically sent tooAzure Storage in hello configured intervals.</span></span> <span data-ttu-id="cd114-147">Az Event Hubs és a további fogadó a legfelső szintű vagy levél csomópont hello hierarchia toobe toohello eseményközpont küldött adhat meg.</span><span class="sxs-lookup"><span data-stu-id="cd114-147">With Event Hubs and any additional sink, you can specify any root or leaf node in hello hierarchy toobe sent toohello event hub.</span></span> <span data-ttu-id="cd114-148">Ez magában foglalja az ETW-események, a teljesítményszámlálók, a Windows eseménynaplóiban keresse meg és az alkalmazásnaplók.</span><span class="sxs-lookup"><span data-stu-id="cd114-148">This includes ETW events, performance counters, Windows event logs, and application logs.</span></span>   

<span data-ttu-id="cd114-149">Fontos, hány adatpontok ténylegesen kell tooconsider tooEvent hubok át.</span><span class="sxs-lookup"><span data-stu-id="cd114-149">It is important tooconsider how many data points should actually be transferred tooEvent Hubs.</span></span> <span data-ttu-id="cd114-150">Általában a fejlesztők adatátvitelt kis késleltetésű közbeni elérési útja, amely kell használni, és gyorsan értelmezi.</span><span class="sxs-lookup"><span data-stu-id="cd114-150">Typically, developers transfer low-latency hot-path data that must be consumed and interpreted quickly.</span></span> <span data-ttu-id="cd114-151">Riasztások vagy az automatikus skálázási szabályok figyelő rendszerek példák.</span><span class="sxs-lookup"><span data-stu-id="cd114-151">Systems that monitor alerts or autoscale rules are examples.</span></span> <span data-ttu-id="cd114-152">A fejlesztők is előfordulhat, hogy egy másik elemzési tároló konfigurálása és keressen rá az áruházban – például Azure Stream Analytics, Elasticsearch, egy egyéni figyelési rendszer vagy más kedvenc figyelési rendszer.</span><span class="sxs-lookup"><span data-stu-id="cd114-152">A developer might also configure an alternate analysis store or search store -- for example, Azure Stream Analytics, Elasticsearch, a custom monitoring system, or a favorite monitoring system from others.</span></span>

<span data-ttu-id="cd114-153">hello az alábbiakban néhány példa konfigurációkat.</span><span class="sxs-lookup"><span data-stu-id="cd114-153">hello following are some example configurations.</span></span>

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

<span data-ttu-id="cd114-154">A fenti példa hello, hello fogadó esetében alkalmazott toohello szülő **PerformanceCounters** hello hierarchiában, ami azt jelenti, hogy minden gyermek csomópont **PerformanceCounters** tooEvent hubok küldi.</span><span class="sxs-lookup"><span data-stu-id="cd114-154">In hello above example, hello sink is applied toohello parent **PerformanceCounters** node in hello hierarchy, which means all child **PerformanceCounters** will be sent tooEvent Hubs.</span></span>  

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

<span data-ttu-id="cd114-155">Hello előző példában hello fogadó alkalmazott tooonly három számláló áll: **kérései**, **elutasítja kérelmeket**, és **processzoridő százalékos**.</span><span class="sxs-lookup"><span data-stu-id="cd114-155">In hello previous example, hello sink is applied tooonly three counters: **Requests Queued**, **Requests Rejected**, and **% Processor time**.</span></span>  

<span data-ttu-id="cd114-156">hello következő példa bemutatja, hogyan fejlesztők is korlátozását hello elküldött adatok toobe hello kritikus metrikákat, amelyeket a szolgáltatás állapotát.</span><span class="sxs-lookup"><span data-stu-id="cd114-156">hello following example shows how a developer can limit hello amount of sent data toobe hello critical metrics that are used for this service’s health.</span></span>  

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

<span data-ttu-id="cd114-157">Ebben a példában hello fogadó alkalmazott toologs és szűrt csak tooerror szintű nyomkövetési.</span><span class="sxs-lookup"><span data-stu-id="cd114-157">In this example, hello sink is applied toologs and is filtered only tooerror level trace.</span></span>

## <a name="deploy-and-update-a-cloud-services-application-and-diagnostics-config"></a><span data-ttu-id="cd114-158">Központi telepítése és a Cloud Services alkalmazás- és diagnosztikai konfiguráció frissítése</span><span class="sxs-lookup"><span data-stu-id="cd114-158">Deploy and update a Cloud Services application and diagnostics config</span></span>
<span data-ttu-id="cd114-159">A Visual Studio hello legegyszerűbb elérési toodeploy hello alkalmazás és az Event Hubs fogadó konfigurációs biztosít.</span><span class="sxs-lookup"><span data-stu-id="cd114-159">Visual Studio provides hello easiest path toodeploy hello application and Event Hubs sink configuration.</span></span> <span data-ttu-id="cd114-160">tooview és Szerkesztés hello fájlt, nyissa meg hello *.wadcfgx* fájlt a Visual Studio, szerkesztheti és mentheti.</span><span class="sxs-lookup"><span data-stu-id="cd114-160">tooview and edit hello file, open hello *.wadcfgx* file in Visual Studio, edit it, and save it.</span></span> <span data-ttu-id="cd114-161">hello elérési út **Felhőszolgáltatás-projekt** > **szerepkörök** > **(RoleName)** > **diagnostics.wadcfgx**.</span><span class="sxs-lookup"><span data-stu-id="cd114-161">hello path is **Cloud Service Project** > **Roles** > **(RoleName)** > **diagnostics.wadcfgx**.</span></span>  

<span data-ttu-id="cd114-162">Ezen a ponton az összes központi telepítés és frissítés a Visual Studio, a Visual Studio Team System, és minden parancsokkal és parancsprogramokkal MSBuild alapulnak, és hello használó műveletek **/t: közzététele** cél tartalmaznak hello *.wadcfgx*  hello csomagolás folyamat során.</span><span class="sxs-lookup"><span data-stu-id="cd114-162">At this point, all deployment and deployment update actions in Visual Studio, Visual Studio Team System, and all commands or scripts that are based on MSBuild and use hello **/t:publish** target include hello *.wadcfgx* in hello packaging process.</span></span> <span data-ttu-id="cd114-163">Emellett központi telepítések és frissítések központi telepítését hello fájl tooAzure hello megfelelő Azure diagnosztikai ügynök bővítményt a virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="cd114-163">In addition, deployments and updates deploy hello file tooAzure by using hello appropriate Azure Diagnostics agent extension on your VMs.</span></span>

<span data-ttu-id="cd114-164">Hello alkalmazás és az Azure diagnosztikai konfiguráció telepítése után hello irányítópult hello az event hubs tevékenység azonnal látni fogja.</span><span class="sxs-lookup"><span data-stu-id="cd114-164">After you deploy hello application and Azure Diagnostics configuration, you will immediately see activity in hello dashboard of hello event hub.</span></span> <span data-ttu-id="cd114-165">Ez azt jelzi, hogy róla, hogy készen áll a toomove adatokon tooviewing hello közbeni elérési hello figyelő ügyfél vagy elemzésük az Ön által választott eszközben.</span><span class="sxs-lookup"><span data-stu-id="cd114-165">This indicates that you're ready toomove on tooviewing hello hot-path data in hello listener client or analysis tool of your choice.</span></span>  

<span data-ttu-id="cd114-166">A következő ábra hello hello Event Hubs irányítópult jeleníti meg, megfelelő küld diagnosztikai adatok toohello event hub kiindulási némi várakozás után 23 óra.</span><span class="sxs-lookup"><span data-stu-id="cd114-166">In hello following figure, hello Event Hubs dashboard shows healthy sending of diagnostics data toohello event hub starting sometime after 11 PM.</span></span> <span data-ttu-id="cd114-167">Ha ez hello alkalmazás lett telepítve, a frissített *.wadcfgx* fájlt, és hello fogadó lett megfelelően konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="cd114-167">That's when hello application was deployed with an updated *.wadcfgx* file, and hello sink was configured properly.</span></span>

![][0]  

> [!NOTE]
> <span data-ttu-id="cd114-168">Ha frissítések toohello Azure Diagnostics konfigurációs fájljában (.wadcfgx), javasoljuk, hogy leküldéses hello frissítések toohello teljes alkalmazás, valamint a hello konfigurációs Visual Studio közzététel, vagy a Windows PowerShell-parancsfájl használatával.</span><span class="sxs-lookup"><span data-stu-id="cd114-168">When you make updates toohello Azure Diagnostics config file (.wadcfgx), it's recommended that you push hello updates toohello entire application as well as hello configuration by using either Visual Studio publishing, or a Windows PowerShell script.</span></span>  
>
>

## <a name="view-hot-path-data"></a><span data-ttu-id="cd114-169">Közbeni elérési adatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="cd114-169">View hot-path data</span></span>
<span data-ttu-id="cd114-170">Korábban bemutatott, nincsenek az Event Hubs adatfeldolgozás figyelő tooand sok alkalmazási helyzetei.</span><span class="sxs-lookup"><span data-stu-id="cd114-170">As discussed previously, there are many use cases for listening tooand processing Event Hubs data.</span></span>

<span data-ttu-id="cd114-171">Egy egyszerű módszer egy kis tesztelési konzol alkalmazás toolisten toohello eseményközpont toocreate és nyomtatási hello kimeneti adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="cd114-171">One simple approach is toocreate a small test console application toolisten toohello event hub and print hello output stream.</span></span> <span data-ttu-id="cd114-172">A következő kódra, amely részletesen kifejtett hello elhelyezheti [Bevezetés az Event Hubs használatába](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)), egy konzolalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="cd114-172">You can place hello following code, which is explained in more detail in [Get started with Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)), in a console application.</span></span>  

<span data-ttu-id="cd114-173">Vegye figyelembe, hogy hello Konzolalkalmazás tartalmaznia kell hello [esemény processzor állomás NuGet-csomag](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).</span><span class="sxs-lookup"><span data-stu-id="cd114-173">Note that hello console application must include hello [Event Processor Host NuGet package](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).</span></span>  

<span data-ttu-id="cd114-174">Ne feledje tooreplace hello értékek hello a csúcsos zárójelek **fő** függvény erőforrások értékekkel.</span><span class="sxs-lookup"><span data-stu-id="cd114-174">Remember tooreplace hello values in angle brackets in hello **Main** function with values for your resources.</span></span>   

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

            Console.WriteLine("Receiving. Press enter key toostop worker.");
            Console.ReadLine();
            eventProcessorHost.UnregisterEventProcessorAsync().Wait();
        }
    }
}
```

## <a name="troubleshoot-event-hubs-sinks"></a><span data-ttu-id="cd114-175">Az Event Hubs mosdók hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="cd114-175">Troubleshoot Event Hubs sinks</span></span>
* <span data-ttu-id="cd114-176">hello event hubs nem szerepelnek a bejövő vagy kimenő eseményhez kapcsolódó tevékenység várt módon.</span><span class="sxs-lookup"><span data-stu-id="cd114-176">hello event hub does not show incoming or outgoing event activity as expected.</span></span>

    <span data-ttu-id="cd114-177">Ellenőrizze, hogy az eseményközpont sikeresen lett kiépítve.</span><span class="sxs-lookup"><span data-stu-id="cd114-177">Check that your event hub is successfully provisioned.</span></span> <span data-ttu-id="cd114-178">A hello összes Kapcsolatinformáció **PrivateConfig** szakasza *.wadcfgx* az erőforrás hello értékének egyeznie kell hello portal látható módon.</span><span class="sxs-lookup"><span data-stu-id="cd114-178">All connection info in hello **PrivateConfig** section of *.wadcfgx* must match hello values of your resource as seen in hello portal.</span></span> <span data-ttu-id="cd114-179">Győződjön meg arról, hogy rendelkezik-e a biztonsági Társítások házirend lett meghatározva (hello példában "SendRule" jelöli) hello portal, valamint *küldése* engedélyt.</span><span class="sxs-lookup"><span data-stu-id="cd114-179">Make sure that you have a SAS policy defined ("SendRule" in hello example) in hello portal and that *Send* permission is granted.</span></span>  
* <span data-ttu-id="cd114-180">Egy adott frissítés után hello event hubs nem bejövő vagy kimenő eseményhez kapcsolódó tevékenység jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="cd114-180">After an update, hello event hub no longer shows incoming or outgoing event activity.</span></span>

    <span data-ttu-id="cd114-181">Először is győződjön meg arról, hogy hello eseményközpont, és a konfigurációs adatok helyességéről, amint azt korábban.</span><span class="sxs-lookup"><span data-stu-id="cd114-181">First, make sure that hello event hub and configuration info is correct as explained previously.</span></span> <span data-ttu-id="cd114-182">Egyes esetekben hello **PrivateConfig** az egy központi telepítési alaphelyzetbe áll.</span><span class="sxs-lookup"><span data-stu-id="cd114-182">Sometimes hello **PrivateConfig** is reset in a deployment update.</span></span> <span data-ttu-id="cd114-183">hello ajánlott javítás minden változás túl toomake*.wadcfgx* a projekt hello és a teljes alkalmazás frissítés majd leküldéses.</span><span class="sxs-lookup"><span data-stu-id="cd114-183">hello recommended fix is toomake all changes too*.wadcfgx* in hello project and then push a complete application update.</span></span> <span data-ttu-id="cd114-184">Ha ez nem lehetséges, győződjön meg arról, hogy hello diagnosztika update leküldéses értesítések teljes **PrivateConfig** , amely tartalmazza a hello SAS-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="cd114-184">If this is not possible, make sure that hello diagnostics update pushes a complete **PrivateConfig** that includes hello SAS key.</span></span>  
* <span data-ttu-id="cd114-185">Hello javaslatok próbáltam, és hello eseményközpont továbbra sem működik.</span><span class="sxs-lookup"><span data-stu-id="cd114-185">I tried hello suggestions, and hello event hub is still not working.</span></span>

    <span data-ttu-id="cd114-186">Nézzük meg magát Azure diagnosztikai naplók és a hibákat tartalmazó hello Azure Storage tábla: **WADDiagnosticInfrastructureLogsTable**.</span><span class="sxs-lookup"><span data-stu-id="cd114-186">Try looking in hello Azure Storage table that contains logs and errors for Azure Diagnostics itself: **WADDiagnosticInfrastructureLogsTable**.</span></span> <span data-ttu-id="cd114-187">Egy elem toouse olyan eszköz, például [Azure Tártallózó](http://www.storageexplorer.com) tooconnect toothis tárfiókot, ebben a táblázatban megtekintheti, és adja hozzá a lekérdezés az időbélyegzési hello utolsó 24 órában.</span><span class="sxs-lookup"><span data-stu-id="cd114-187">One option is toouse a tool such as [Azure Storage Explorer](http://www.storageexplorer.com) tooconnect toothis storage account, view this table, and add a query for TimeStamp in hello last 24 hours.</span></span> <span data-ttu-id="cd114-188">Hello eszköz tooexport egy CSV-fájlt használ, és nyissa meg a Microsoft Excel alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="cd114-188">You can use hello tool tooexport a .csv file and open it in an application such as Microsoft Excel.</span></span> <span data-ttu-id="cd114-189">Excel segítségével könnyen toosearch-kártya karakterláncok, például a **EventHubs**, toosee milyen hibát jelez.</span><span class="sxs-lookup"><span data-stu-id="cd114-189">Excel makes it easy toosearch for calling-card strings, such as **EventHubs**, toosee what error is reported.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="cd114-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cd114-190">Next steps</span></span>
<span data-ttu-id="cd114-191">• [További információ az Event Hubs](https://azure.microsoft.com/services/event-hubs/)</span><span class="sxs-lookup"><span data-stu-id="cd114-191">•    [Learn more about Event Hubs](https://azure.microsoft.com/services/event-hubs/)</span></span>

## <a name="appendix-complete-azure-diagnostics-configuration-file-wadcfgx-example"></a><span data-ttu-id="cd114-192">A függelék: Végezze el az Azure Diagnostics konfigurációs fájl (.wadcfgx) – Példa</span><span class="sxs-lookup"><span data-stu-id="cd114-192">Appendix: Complete Azure Diagnostics configuration file (.wadcfgx) example</span></span>
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

<span data-ttu-id="cd114-193">kiegészítő hello *ServiceConfiguration.Cloud.cscfg* az ebben a példában a következőhöz hasonló, hello.</span><span class="sxs-lookup"><span data-stu-id="cd114-193">hello complementary *ServiceConfiguration.Cloud.cscfg* for this example looks like hello following.</span></span>

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

<span data-ttu-id="cd114-194">A virtuális gépek egyenértékű alapú Json-beállítások a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="cd114-194">Equivalent Json based settings for virtual machines is as follows:</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="cd114-195">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cd114-195">Next steps</span></span>
<span data-ttu-id="cd114-196">További információ az Event Hubs érhetők el a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="cd114-196">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="cd114-197">Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="cd114-197">Event Hubs overview</span></span>](../event-hubs/event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="cd114-198">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="cd114-198">Create an event hub</span></span>](../event-hubs/event-hubs-create.md)
* [<span data-ttu-id="cd114-199">Event Hubs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="cd114-199">Event Hubs FAQ</span></span>](../event-hubs/event-hubs-faq.md)

<!-- Images. -->
[0]: ../event-hubs/media/event-hubs-streaming-azure-diags-data/dashboard.png
