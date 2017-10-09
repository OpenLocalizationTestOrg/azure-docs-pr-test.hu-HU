---
title: az Azure diagnostics (.NET), a Cloud Serviceshez aaaHow toouse |} Microsoft Docs
description: "Az Azure diagnostics toogather használatával adatokat az Azure-ból a felhőalapú szolgáltatások hibakeresési információ méri a teljesítményt, figyelés, forgalom elemzése és több."
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: 
ms.assetid: 89623a0e-4e78-4b67-a446-7d19a35a44be
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/22/2017
ms.author: robb
ms.openlocfilehash: 1525eac1e85955d8f05aa21a9805e0a80d0e4bca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a><span data-ttu-id="0a10e-103">Azure Cloud Services Azure Diagnostics engedélyezése</span><span class="sxs-lookup"><span data-stu-id="0a10e-103">Enabling Azure Diagnostics in Azure Cloud Services</span></span>
<span data-ttu-id="0a10e-104">Lásd: [Azure Diagnostics áttekintése](../azure-diagnostics.md) az Azure Diagnostics háttérként.</span><span class="sxs-lookup"><span data-stu-id="0a10e-104">See [Azure Diagnostics Overview](../azure-diagnostics.md) for a background on Azure Diagnostics.</span></span>

## <a name="how-tooenable-diagnostics-in-a-worker-role"></a><span data-ttu-id="0a10e-105">Hogyan tooEnable diagnosztika a feldolgozói szerepkörök</span><span class="sxs-lookup"><span data-stu-id="0a10e-105">How tooEnable Diagnostics in a Worker Role</span></span>
<span data-ttu-id="0a10e-106">A forgatókönyv leírja, hogyan tooimplement egy Azure feldolgozói szerepkör, amely bocsát ki a telemetriai adatok használatával hello .NET EventSource osztály.</span><span class="sxs-lookup"><span data-stu-id="0a10e-106">This walkthrough describes how tooimplement an Azure worker role that emits telemetry data using hello .NET EventSource class.</span></span> <span data-ttu-id="0a10e-107">Az Azure Diagnostics használt toocollect hello telemetriai adatokat, és tárolja az Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="0a10e-107">Azure Diagnostics is used toocollect hello telemetry data and store it in an Azure storage account.</span></span> <span data-ttu-id="0a10e-108">A feldolgozói szerepkör létrehozásakor a Visual Studio automatikusan engedélyezi a diagnosztika 1.0-s .NET 2.4 és korábbi Azure SDK-k hello megoldás részeként.</span><span class="sxs-lookup"><span data-stu-id="0a10e-108">When creating a worker role, Visual Studio automatically enables Diagnostics 1.0 as part of hello solution in Azure SDKs for .NET 2.4 and earlier.</span></span> <span data-ttu-id="0a10e-109">hello következő útmutatások mutatják be hello folyamat hello feldolgozói szerepkör, diagnosztika 1.0 letiltása az hello megoldás és a diagnosztika 1.2-es vagy 1.3 tooyour feldolgozói szerepkör létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="0a10e-109">hello following instructions describe hello process for creating hello worker role, disabling Diagnostics 1.0 from hello solution, and deploying Diagnostics 1.2 or 1.3 tooyour worker role.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="0a10e-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0a10e-110">Prerequisites</span></span>
<span data-ttu-id="0a10e-111">Ez a cikk azt feltételezi, hogy az Azure-előfizetéssel rendelkezik, és a Visual Studio hello Azure SDK-t használ.</span><span class="sxs-lookup"><span data-stu-id="0a10e-111">This article assumes you have an Azure subscription and are using Visual Studio with hello Azure SDK.</span></span> <span data-ttu-id="0a10e-112">Ha nem rendelkezik Azure-előfizetéssel, a hello regisztrálhat [ingyenes][Free Trial].</span><span class="sxs-lookup"><span data-stu-id="0a10e-112">If you do not have an Azure subscription, you can sign up for hello [Free Trial][Free Trial].</span></span> <span data-ttu-id="0a10e-113">Győződjön meg arról, hogy túl[telepítse és konfigurálja az Azure Powershellt 0.8.7 verzió vagy újabb][Install and configure Azure PowerShell version 0.8.7 or later].</span><span class="sxs-lookup"><span data-stu-id="0a10e-113">Make sure too[Install and configure Azure PowerShell version 0.8.7 or later][Install and configure Azure PowerShell version 0.8.7 or later].</span></span>

### <a name="step-1-create-a-worker-role"></a><span data-ttu-id="0a10e-114">1. lépés: A feldolgozói szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="0a10e-114">Step 1: Create a Worker Role</span></span>
1. <span data-ttu-id="0a10e-115">Indítsa el a **Visual Studiót**.</span><span class="sxs-lookup"><span data-stu-id="0a10e-115">Launch **Visual Studio**.</span></span>
2. <span data-ttu-id="0a10e-116">Hozzon létre egy **Azure Cloud Service** projektet a hello **felhő** sablon, amelynek célpontja a .NET-keretrendszer 4.5.</span><span class="sxs-lookup"><span data-stu-id="0a10e-116">Create an **Azure Cloud Service** project from hello **Cloud** template that targets .NET Framework 4.5.</span></span>  <span data-ttu-id="0a10e-117">Hello projekt "WadExample" nevet, és kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="0a10e-117">Name hello project "WadExample" and click Ok.</span></span>
3. <span data-ttu-id="0a10e-118">Válassza ki **feldolgozói szerepkör** kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="0a10e-118">Select **Worker Role** and click Ok.</span></span> <span data-ttu-id="0a10e-119">hello projekt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0a10e-119">hello project will be created.</span></span>
4. <span data-ttu-id="0a10e-120">A **Megoldáskezelőben**, kattintson duplán a hello **WorkerRole1** tulajdonságok fájlt.</span><span class="sxs-lookup"><span data-stu-id="0a10e-120">In **Solution Explorer**, double-click hello **WorkerRole1** properties file.</span></span>
5. <span data-ttu-id="0a10e-121">A hello **konfigurációs** lap, un-ellenőrzés **engedélyezése diagnosztikai** toodisable diagnosztika 1.0 (az Azure SDK 2.4-es és korábbi).</span><span class="sxs-lookup"><span data-stu-id="0a10e-121">In hello **Configuration** tab, un-check **Enable Diagnostics** toodisable Diagnostics 1.0 (Azure SDK 2.4 and earlier).</span></span>
6. <span data-ttu-id="0a10e-122">A megoldás tooverify létrehozása, amely nincs hiba van.</span><span class="sxs-lookup"><span data-stu-id="0a10e-122">Build your solution tooverify that you have no errors.</span></span>

### <a name="step-2-instrument-your-code"></a><span data-ttu-id="0a10e-123">2. lépés: Állíthatnak be a kódot</span><span class="sxs-lookup"><span data-stu-id="0a10e-123">Step 2: Instrument your code</span></span>
<span data-ttu-id="0a10e-124">Cserélje le a következő kód hello WorkerRole.cs hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="0a10e-124">Replace hello contents of WorkerRole.cs with hello following code.</span></span> <span data-ttu-id="0a10e-125">Öröklődés forrása: hello osztály SampleEventSourceWriter, hello [EventSource osztály][EventSource Class], négy naplózási módszerek megvalósítja: **SendEnums**, **MessageMethod** , **SetOther** és **HighFreq**.</span><span class="sxs-lookup"><span data-stu-id="0a10e-125">hello class SampleEventSourceWriter, inherited from hello [EventSource Class][EventSource Class], implements four logging methods: **SendEnums**, **MessageMethod**, **SetOther** and **HighFreq**.</span></span> <span data-ttu-id="0a10e-126">első paraméter toohello hello **WriteEvent** metódus hello megfelelő esemény hello Azonosítóját határozza meg.</span><span class="sxs-lookup"><span data-stu-id="0a10e-126">hello first parameter toohello **WriteEvent** method defines hello ID for hello respective event.</span></span> <span data-ttu-id="0a10e-127">hello Run metódus valósítja meg, hogy egyes hello hello megvalósított naplózási módszereket hív meg végtelen hurkot **SampleEventSourceWriter** 10 másodpercenként osztályban.</span><span class="sxs-lookup"><span data-stu-id="0a10e-127">hello Run method implements an infinite loop that calls each of hello logging methods implemented in hello **SampleEventSourceWriter** class every 10 seconds.</span></span>

```csharp
using Microsoft.WindowsAzure.ServiceRuntime;
using System;
using System.Diagnostics;
using System.Diagnostics.Tracing;
using System.Net;
using System.Threading;

namespace WorkerRole1
{
    sealed class SampleEventSourceWriter : EventSource
    {
        public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
        public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums tooint for efficient logging.
        public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
        public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
        public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }

    }

    enum MyColor
    {
        Red,
        Blue,
        Green
    }

    [Flags]
    enum MyFlags
    {
        Flag1 = 1,
        Flag2 = 2,
        Flag3 = 4
    }

    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
            // This is a sample worker implementation. Replace with your logic.
            Trace.TraceInformation("WorkerRole1 entry point called");

            int value = 0;

            while (true)
            {
                Thread.Sleep(10000);
                Trace.TraceInformation("Working");

                // Emit several events every time we go through hello loop
                for (int i = 0; i < 6; i++)
                {
                    SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
                }

                for (int i = 0; i < 3; i++)
                {
                    SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                    SampleEventSourceWriter.Log.SetOther(true, 123456789);
                }

                if (value == int.MaxValue) value = 0;
                SampleEventSourceWriter.Log.HighFreq(value++);
            }
        }

        public override bool OnStart()
        {
            // Set hello maximum number of concurrent connections
            ServicePointManager.DefaultConnectionLimit = 12;

            // For information on handling configuration changes
            // see hello MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.

            return base.OnStart();
        }
    }
}
```


### <a name="step-3-deploy-your-worker-role"></a><span data-ttu-id="0a10e-128">3. lépés: A feldolgozói szerepkör telepítése</span><span class="sxs-lookup"><span data-stu-id="0a10e-128">Step 3: Deploy your Worker Role</span></span>

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

1. <span data-ttu-id="0a10e-129">A feldolgozói szerepkör tooAzure, a Visual Studio telepítése hello kiválasztásával **WadExample** projektre a Solution Explorer hello majd **közzététel** a hello **Build** menü.</span><span class="sxs-lookup"><span data-stu-id="0a10e-129">Deploy your worker role tooAzure from within Visual Studio by selecting hello **WadExample** project in hello Solution Explorer then **Publish** from hello **Build** menu.</span></span>
2. <span data-ttu-id="0a10e-130">Válassza ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="0a10e-130">Choose your subscription.</span></span>
3. <span data-ttu-id="0a10e-131">A hello **Microsoft Azure közzétételi beállítási** párbeszédablakban válassza **új létrehozása...** .</span><span class="sxs-lookup"><span data-stu-id="0a10e-131">In hello **Microsoft Azure Publish Settings** dialog, select **Create New…**.</span></span>
4. <span data-ttu-id="0a10e-132">A hello **felhőalapú szolgáltatás létrehozása és a Tárfiók** párbeszédpanelen adja meg egy **neve** (például "WadExample"), és válasszon régiót vagy affinitáscsoportot.</span><span class="sxs-lookup"><span data-stu-id="0a10e-132">In hello **Create Cloud Service and Storage Account** dialog, enter a **Name** (for example, "WadExample") and select a region or affinity group.</span></span>
5. <span data-ttu-id="0a10e-133">Set hello **környezet** túl**átmeneti**.</span><span class="sxs-lookup"><span data-stu-id="0a10e-133">Set hello **Environment** too**Staging**.</span></span>
6. <span data-ttu-id="0a10e-134">Módosíthatja bármely más **beállítások** regisztrációja, mivel a megfelelő, és kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="0a10e-134">Modify any other **Settings** as appropriate and click **Publish**.</span></span>
7. <span data-ttu-id="0a10e-135">Központi telepítés befejezése után ellenőrizze az Azure-portál, amely a felhőszolgáltatás hello egy **futtató** állapotát.</span><span class="sxs-lookup"><span data-stu-id="0a10e-135">After deployment has completed, verify in hello Azure portal that your cloud service is in a **Running** state.</span></span>

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-hello-extension"></a><span data-ttu-id="0a10e-136">4. lépés: A diagnosztika konfigurációs fájl létrehozása és hello-kiterjesztés telepítése</span><span class="sxs-lookup"><span data-stu-id="0a10e-136">Step 4: Create your Diagnostics configuration file and install hello extension</span></span>
1. <span data-ttu-id="0a10e-137">Töltse le a hello nyilvános konfigurációs fájl sémadefiníciót a következő hello a következő PowerShell-parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="0a10e-137">Download hello public configuration file schema definition by executing hello following PowerShell command:</span></span>

    ```powershell
    (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'
    ```
2. <span data-ttu-id="0a10e-138">Adja hozzá az XML-fájl tooyour **WorkerRole1** projekt, kattintson a jobb gombbal a hello **WorkerRole1** projektre, és válassza ki **Hozzáadás** -> **új elem...**</span><span class="sxs-lookup"><span data-stu-id="0a10e-138">Add an XML file tooyour **WorkerRole1** project by right-clicking on hello **WorkerRole1** project and select **Add** -> **New Item…**</span></span><span data-ttu-id="0a10e-139"> -> **Visual C# elemek** -> **adatok** -> **XML-fájl**.</span><span class="sxs-lookup"><span data-stu-id="0a10e-139"> -> **Visual C# items** -> **Data** -> **XML File**.</span></span> <span data-ttu-id="0a10e-140">Nevezze el "WadExample.xml" hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="0a10e-140">Name hello file "WadExample.xml".</span></span>

   ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)
3. <span data-ttu-id="0a10e-142">Hello WadConfig.xsd társítandó hello konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="0a10e-142">Associate hello WadConfig.xsd with hello configuration file.</span></span> <span data-ttu-id="0a10e-143">Ellenőrizze, hogy hello WadExample.xml szerkesztő ablakban hello aktív ablak.</span><span class="sxs-lookup"><span data-stu-id="0a10e-143">Make sure hello WadExample.xml editor window is hello active window.</span></span> <span data-ttu-id="0a10e-144">Nyomja le az **F4** tooopen hello **tulajdonságok** ablak.</span><span class="sxs-lookup"><span data-stu-id="0a10e-144">Press **F4** tooopen hello **Properties** window.</span></span> <span data-ttu-id="0a10e-145">Kattintson a hello **sémák** hello tulajdonság **tulajdonságok** ablak.</span><span class="sxs-lookup"><span data-stu-id="0a10e-145">Click hello **Schemas** property in hello **Properties** window.</span></span> <span data-ttu-id="0a10e-146">Kattintson a hello **...**</span><span class="sxs-lookup"><span data-stu-id="0a10e-146">Click hello **…**</span></span> <span data-ttu-id="0a10e-147">a hello **sémák** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="0a10e-147">in hello **Schemas** property.</span></span> <span data-ttu-id="0a10e-148">Kattintson a hello **hozzáadása...**</span><span class="sxs-lookup"><span data-stu-id="0a10e-148">Click hello **Add…**</span></span> <span data-ttu-id="0a10e-149">gombra, és keresse meg a toohello helyet, ahová mentette hello XSD-fájlt, és válassza hello fájl WadConfig.xsd.</span><span class="sxs-lookup"><span data-stu-id="0a10e-149">button and navigate toohello location where you saved hello XSD file and select hello file WadConfig.xsd.</span></span> <span data-ttu-id="0a10e-150">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="0a10e-150">Click **OK**.</span></span>

4. <span data-ttu-id="0a10e-151">Cserélje le a hello hello WadExample.xml konfigurációs fájl tartalmát, a következő XML hello és hello fájl.</span><span class="sxs-lookup"><span data-stu-id="0a10e-151">Replace hello contents of hello WadExample.xml configuration file with hello following XML and save hello file.</span></span> <span data-ttu-id="0a10e-152">A konfigurációs fájl határozza meg néhány teljesítmény-számlálói toocollect: egy CPU-felhasználás, egy, a memória-felhasználás.</span><span class="sxs-lookup"><span data-stu-id="0a10e-152">This configuration file defines a couple performance counters toocollect: one for CPU utilization and one for memory utilization.</span></span> <span data-ttu-id="0a10e-153">Hello konfigurációs majd hello négy események toohello módszerek hello SampleEventSourceWriter osztály a megfelelő határozza meg.</span><span class="sxs-lookup"><span data-stu-id="0a10e-153">Then hello configuration defines hello four events corresponding toohello methods in hello SampleEventSourceWriter class.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <WadCfg>
    <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
      <PerformanceCounters scheduledTransferPeriod="PT1M">
        <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />
        <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT1M" unit="bytes"/>
      </PerformanceCounters>
      <EtwProviders>
        <EtwEventSourceProviderConfiguration provider="SampleEventSourceWriter" scheduledTransferPeriod="PT5M">
          <Event id="1" eventDestination="EnumsTable"/>
          <Event id="2" eventDestination="MessageTable"/>
          <Event id="3" eventDestination="SetOtherTable"/>
          <Event id="4" eventDestination="HighFreqTable"/>
          <DefaultEvents eventDestination="DefaultTable" />
        </EtwEventSourceProviderConfiguration>
      </EtwProviders>
    </DiagnosticMonitorConfiguration>
  </WadCfg>
</PublicConfig>
```

### <a name="step-5-install-diagnostics-on-your-worker-role"></a><span data-ttu-id="0a10e-154">5. lépés: Telepítse a feldolgozói szerepkör diagnosztika</span><span class="sxs-lookup"><span data-stu-id="0a10e-154">Step 5: Install Diagnostics on your Worker Role</span></span>
<span data-ttu-id="0a10e-155">hello diagnosztika webes vagy feldolgozói szerepkör kezelése a PowerShell-parancsmagok vannak: Set-AzureServiceDiagnosticsExtension, a Get-AzureServiceDiagnosticsExtension és a Remove-AzureServiceDiagnosticsExtension.</span><span class="sxs-lookup"><span data-stu-id="0a10e-155">hello PowerShell cmdlets for managing Diagnostics on a web or worker role are: Set-AzureServiceDiagnosticsExtension, Get-AzureServiceDiagnosticsExtension, and Remove-AzureServiceDiagnosticsExtension.</span></span>

1. <span data-ttu-id="0a10e-156">Nyissa meg az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0a10e-156">Open Azure PowerShell.</span></span>
2. <span data-ttu-id="0a10e-157">A feldolgozói szerepkör hello parancsfájl tooinstall diagnosztika hajt végre (cserélje le *StorageAccountKey* a hello tárfiók kulcsa a wadexample tárfiók és *config_path* hello elérési úttal rendelkező toohello *WadExample.xml* fájl):</span><span class="sxs-lookup"><span data-stu-id="0a10e-157">Execute hello script tooinstall Diagnostics on your worker role (replace *StorageAccountKey* with hello storage account key for your wadexample storage account and *config_path* with hello path toohello *WadExample.xml* file):</span></span>

```powershell
$storage_name = "wadexample"
$key = "<StorageAccountKey>"
$config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
$service_name="wadexample"
$storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a><span data-ttu-id="0a10e-158">6. lépés: Nézze meg a telemetriai adatok</span><span class="sxs-lookup"><span data-stu-id="0a10e-158">Step 6: Look at your telemetry data</span></span>
<span data-ttu-id="0a10e-159">A Visual Studio hello **Server Explorer**, keresse meg a toohello wadexample tárfiók.</span><span class="sxs-lookup"><span data-stu-id="0a10e-159">In hello Visual Studio **Server Explorer**, navigate toohello wadexample storage account.</span></span> <span data-ttu-id="0a10e-160">Miután hello felhőszolgáltatás körülbelül öt (5) percig futott, megtekintheti az hello táblák **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** és **WADSetOtherTable**.</span><span class="sxs-lookup"><span data-stu-id="0a10e-160">After hello cloud service has been running about five (5) minutes, you should see hello tables **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** and **WADSetOtherTable**.</span></span> <span data-ttu-id="0a10e-161">Kattintson duplán a hello táblák tooview hello telemetriai gyűjtött.</span><span class="sxs-lookup"><span data-stu-id="0a10e-161">Double-click one of hello tables tooview hello telemetry that has been collected.</span></span>

![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)

## <a name="configuration-file-schema"></a><span data-ttu-id="0a10e-163">Konfigurációs fájl séma</span><span class="sxs-lookup"><span data-stu-id="0a10e-163">Configuration File Schema</span></span>
<span data-ttu-id="0a10e-164">hello diagnosztika konfigurációs fájl használt tooinitialize diagnosztikai konfigurációs beállítások hello diagnosztikai ügynök indulásakor értékeket határozza meg.</span><span class="sxs-lookup"><span data-stu-id="0a10e-164">hello Diagnostics configuration file defines values that are used tooinitialize diagnostic configuration settings when hello diagnostics agent starts.</span></span> <span data-ttu-id="0a10e-165">Lásd: hello [legújabb sémareferenciája](https://msdn.microsoft.com/library/azure/mt634524.aspx) az érvényes értékek és a példákat.</span><span class="sxs-lookup"><span data-stu-id="0a10e-165">See hello [latest schema reference](https://msdn.microsoft.com/library/azure/mt634524.aspx) for valid values and examples.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="0a10e-166">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="0a10e-166">Troubleshooting</span></span>
<span data-ttu-id="0a10e-167">Ha gondja támad, lásd: [hibaelhárítási Azure Diagnostics](../azure-diagnostics-troubleshooting.md) segítséget a gyakori problémák megoldásához.</span><span class="sxs-lookup"><span data-stu-id="0a10e-167">If you have trouble, see [Troubleshooting Azure Diagnostics](../azure-diagnostics-troubleshooting.md) for help with common problems.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0a10e-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0a10e-168">Next Steps</span></span>
<span data-ttu-id="0a10e-169">[A kapcsolódó Azure virtuális gép diagnosztikai cikkek megtekintéséhez](../monitoring-and-diagnostics/azure-diagnostics.md#cloud-services-using-azure-diagnostics) toochange hello adatokat gyűjti, kapcsolatos problémák elhárítása vagy kapcsolatos további diagnosztikai általában.</span><span class="sxs-lookup"><span data-stu-id="0a10e-169">[See a list of related Azure virtual-machine diagnostic articles](../monitoring-and-diagnostics/azure-diagnostics.md#cloud-services-using-azure-diagnostics) toochange hello data you are collecting, troubleshoot problems or learn more about diagnostics in general.</span></span>

[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Free Trial]: http://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
