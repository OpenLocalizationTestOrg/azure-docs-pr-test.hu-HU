---
title: "Cloud Services Azure diagnostics (.NET) használata |} Microsoft Docs"
description: "Az Azure diagnostics használatával az adatok gyűjtését a hibakeresést, méri a teljesítményt, figyelés, forgalom elemzése és további Azure Felhőszolgáltatások."
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
ms.openlocfilehash: 333d2f26ce043a167fb84858c8327cb39e868ffa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a><span data-ttu-id="9b21a-103">Azure Cloud Services Azure Diagnostics engedélyezése</span><span class="sxs-lookup"><span data-stu-id="9b21a-103">Enabling Azure Diagnostics in Azure Cloud Services</span></span>
<span data-ttu-id="9b21a-104">Lásd: [Azure Diagnostics áttekintése](../azure-diagnostics.md) az Azure Diagnostics háttérként.</span><span class="sxs-lookup"><span data-stu-id="9b21a-104">See [Azure Diagnostics Overview](../azure-diagnostics.md) for a background on Azure Diagnostics.</span></span>

## <a name="how-to-enable-diagnostics-in-a-worker-role"></a><span data-ttu-id="9b21a-105">Diagnosztika a feldolgozói szerepkörök engedélyezése</span><span class="sxs-lookup"><span data-stu-id="9b21a-105">How to Enable Diagnostics in a Worker Role</span></span>
<span data-ttu-id="9b21a-106">Ez a bemutató ismerteti, hogyan megvalósításához egy Azure feldolgozói szerepkör, amely bocsát ki a .NET EventSource osztály használatával telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="9b21a-106">This walkthrough describes how to implement an Azure worker role that emits telemetry data using the .NET EventSource class.</span></span> <span data-ttu-id="9b21a-107">Az Azure Diagnostics szolgál a telemetriai adatokat gyűjt, és tárolja az Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="9b21a-107">Azure Diagnostics is used to collect the telemetry data and store it in an Azure storage account.</span></span> <span data-ttu-id="9b21a-108">A feldolgozói szerepkör létrehozásakor a Visual Studio automatikusan engedélyezi a diagnosztika 1.0-s .NET 2.4 és korábbi Azure SDK-k a megoldás részeként.</span><span class="sxs-lookup"><span data-stu-id="9b21a-108">When creating a worker role, Visual Studio automatically enables Diagnostics 1.0 as part of the solution in Azure SDKs for .NET 2.4 and earlier.</span></span> <span data-ttu-id="9b21a-109">A következő útmutatások mutatják be, a feldolgozói szerepkör diagnosztika 1.0 letiltása a megoldást, és a központi telepítés diagnosztika 1.2-es vagy 1.3 a feldolgozói szerepkör lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9b21a-109">The following instructions describe the process for creating the worker role, disabling Diagnostics 1.0 from the solution, and deploying Diagnostics 1.2 or 1.3 to your worker role.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="9b21a-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9b21a-110">Prerequisites</span></span>
<span data-ttu-id="9b21a-111">Ez a cikk azt feltételezi, hogy az Azure-előfizetéssel rendelkezik, és a Visual Studio Azure SDK-t használ.</span><span class="sxs-lookup"><span data-stu-id="9b21a-111">This article assumes you have an Azure subscription and are using Visual Studio with the Azure SDK.</span></span> <span data-ttu-id="9b21a-112">Ha még nem rendelkezik Azure-előfizetéssel, regisztrálhat az a [ingyenes][Free Trial].</span><span class="sxs-lookup"><span data-stu-id="9b21a-112">If you do not have an Azure subscription, you can sign up for the [Free Trial][Free Trial].</span></span> <span data-ttu-id="9b21a-113">Ügyeljen arra, hogy [telepítse és konfigurálja az Azure Powershellt 0.8.7 verzió vagy újabb][Install and configure Azure PowerShell version 0.8.7 or later].</span><span class="sxs-lookup"><span data-stu-id="9b21a-113">Make sure to [Install and configure Azure PowerShell version 0.8.7 or later][Install and configure Azure PowerShell version 0.8.7 or later].</span></span>

### <a name="step-1-create-a-worker-role"></a><span data-ttu-id="9b21a-114">1. lépés: A feldolgozói szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="9b21a-114">Step 1: Create a Worker Role</span></span>
1. <span data-ttu-id="9b21a-115">Indítsa el **Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="9b21a-115">Launch **Visual Studio**.</span></span>
2. <span data-ttu-id="9b21a-116">Hozzon létre egy **Azure Cloud Service** -projektet a **felhő** sablon, amelynek célpontja a .NET-keretrendszer 4.5.</span><span class="sxs-lookup"><span data-stu-id="9b21a-116">Create an **Azure Cloud Service** project from the **Cloud** template that targets .NET Framework 4.5.</span></span>  <span data-ttu-id="9b21a-117">A projekt "WadExample" nevet, és kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="9b21a-117">Name the project "WadExample" and click Ok.</span></span>
3. <span data-ttu-id="9b21a-118">Válassza ki **feldolgozói szerepkör** kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="9b21a-118">Select **Worker Role** and click Ok.</span></span> <span data-ttu-id="9b21a-119">A projekt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="9b21a-119">The project will be created.</span></span>
4. <span data-ttu-id="9b21a-120">A **Megoldáskezelőben**, kattintson duplán a **WorkerRole1** tulajdonságok fájlt.</span><span class="sxs-lookup"><span data-stu-id="9b21a-120">In **Solution Explorer**, double-click the **WorkerRole1** properties file.</span></span>
5. <span data-ttu-id="9b21a-121">A a **konfigurációs** lap, un-ellenőrzés **engedélyezése diagnosztikai** a diagnosztika 1.0 (az Azure SDK 2.4-es és korábbi) le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="9b21a-121">In the **Configuration** tab, un-check **Enable Diagnostics** to disable Diagnostics 1.0 (Azure SDK 2.4 and earlier).</span></span>
6. <span data-ttu-id="9b21a-122">Ellenőrizze, hogy rendelkezik-e hibák a megoldás felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="9b21a-122">Build your solution to verify that you have no errors.</span></span>

### <a name="step-2-instrument-your-code"></a><span data-ttu-id="9b21a-123">2. lépés: Állíthatnak be a kódot</span><span class="sxs-lookup"><span data-stu-id="9b21a-123">Step 2: Instrument your code</span></span>
<span data-ttu-id="9b21a-124">Cserélje le a WorkerRole.cs tartalmát az alábbira.</span><span class="sxs-lookup"><span data-stu-id="9b21a-124">Replace the contents of WorkerRole.cs with the following code.</span></span> <span data-ttu-id="9b21a-125">Az osztály SampleEventSourceWriter, öröklődik a [EventSource osztály][EventSource Class], négy naplózási módszerek megvalósítja: **SendEnums**, **MessageMethod** , **SetOther** és **HighFreq**.</span><span class="sxs-lookup"><span data-stu-id="9b21a-125">The class SampleEventSourceWriter, inherited from the [EventSource Class][EventSource Class], implements four logging methods: **SendEnums**, **MessageMethod**, **SetOther** and **HighFreq**.</span></span> <span data-ttu-id="9b21a-126">Az első paraméterben a **WriteEvent** módszer határozza meg a megfelelő Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="9b21a-126">The first parameter to the **WriteEvent** method defines the ID for the respective event.</span></span> <span data-ttu-id="9b21a-127">A Run metódus valósítja meg, amely behívja a naplózási módszer valósult meg végtelen hurkot a **SampleEventSourceWriter** 10 másodpercenként osztályban.</span><span class="sxs-lookup"><span data-stu-id="9b21a-127">The Run method implements an infinite loop that calls each of the logging methods implemented in the **SampleEventSourceWriter** class every 10 seconds.</span></span>

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
        public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums to int for efficient logging.
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

                // Emit several events every time we go through the loop
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
            // Set the maximum number of concurrent connections
            ServicePointManager.DefaultConnectionLimit = 12;

            // For information on handling configuration changes
            // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.

            return base.OnStart();
        }
    }
}
```


### <a name="step-3-deploy-your-worker-role"></a><span data-ttu-id="9b21a-128">3. lépés: A feldolgozói szerepkör telepítése</span><span class="sxs-lookup"><span data-stu-id="9b21a-128">Step 3: Deploy your Worker Role</span></span>

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

1. <span data-ttu-id="9b21a-129">A feldolgozói szerepkör telepítése a Visual Studio Azure kiválasztásával a **WadExample** projektben a Megoldáskezelőre majd **közzététel** a a **Build** menü.</span><span class="sxs-lookup"><span data-stu-id="9b21a-129">Deploy your worker role to Azure from within Visual Studio by selecting the **WadExample** project in the Solution Explorer then **Publish** from the **Build** menu.</span></span>
2. <span data-ttu-id="9b21a-130">Válassza ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="9b21a-130">Choose your subscription.</span></span>
3. <span data-ttu-id="9b21a-131">Az a **Microsoft Azure közzétételi beállítási** párbeszédablakban válassza **új létrehozása...** .</span><span class="sxs-lookup"><span data-stu-id="9b21a-131">In the **Microsoft Azure Publish Settings** dialog, select **Create New…**.</span></span>
4. <span data-ttu-id="9b21a-132">Az a **felhőalapú szolgáltatás létrehozása és a Tárfiók** párbeszédpanelen adja meg egy **neve** (például "WadExample"), és válasszon régiót vagy affinitáscsoportot.</span><span class="sxs-lookup"><span data-stu-id="9b21a-132">In the **Create Cloud Service and Storage Account** dialog, enter a **Name** (for example, "WadExample") and select a region or affinity group.</span></span>
5. <span data-ttu-id="9b21a-133">Állítsa be a **környezet** való **átmeneti**.</span><span class="sxs-lookup"><span data-stu-id="9b21a-133">Set the **Environment** to **Staging**.</span></span>
6. <span data-ttu-id="9b21a-134">Módosíthatja bármely más **beállítások** regisztrációja, mivel a megfelelő, és kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="9b21a-134">Modify any other **Settings** as appropriate and click **Publish**.</span></span>
7. <span data-ttu-id="9b21a-135">Miután a telepítés befejeződése után ellenőrizze az Azure portálon, amely a felhőszolgáltatás egy **futtató** állapota.</span><span class="sxs-lookup"><span data-stu-id="9b21a-135">After deployment has completed, verify in the Azure portal that your cloud service is in a **Running** state.</span></span>

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-the-extension"></a><span data-ttu-id="9b21a-136">4. lépés: A diagnosztika konfigurációs fájl létrehozása és a-kiterjesztés telepítése</span><span class="sxs-lookup"><span data-stu-id="9b21a-136">Step 4: Create your Diagnostics configuration file and install the extension</span></span>
1. <span data-ttu-id="9b21a-137">Töltse le a nyilvános konfigurációs fájl sémadefiníciót hajtja végre a következő PowerShell-parancsot:</span><span class="sxs-lookup"><span data-stu-id="9b21a-137">Download the public configuration file schema definition by executing the following PowerShell command:</span></span>

    ```powershell
    (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'
    ```
2. <span data-ttu-id="9b21a-138">Az XML-fájl hozzáadása a **WorkerRole1** kattintson a jobb gombbal a projekt a **WorkerRole1** projektre, és válassza ki **Hozzáadás** -> **új elem...**</span><span class="sxs-lookup"><span data-stu-id="9b21a-138">Add an XML file to your **WorkerRole1** project by right-clicking on the **WorkerRole1** project and select **Add** -> **New Item…**</span></span><span data-ttu-id="9b21a-139"> -> **Visual C# elemek** -> **adatok** -> **XML-fájl**.</span><span class="sxs-lookup"><span data-stu-id="9b21a-139"> -> **Visual C# items** -> **Data** -> **XML File**.</span></span> <span data-ttu-id="9b21a-140">A fájl "WadExample.xml" nevet.</span><span class="sxs-lookup"><span data-stu-id="9b21a-140">Name the file "WadExample.xml".</span></span>

   ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)
3. <span data-ttu-id="9b21a-142">A WadConfig.xsd társítani a konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="9b21a-142">Associate the WadConfig.xsd with the configuration file.</span></span> <span data-ttu-id="9b21a-143">Ellenőrizze, hogy a WadExample.xml-szerkesztő ablakban az aktív ablak.</span><span class="sxs-lookup"><span data-stu-id="9b21a-143">Make sure the WadExample.xml editor window is the active window.</span></span> <span data-ttu-id="9b21a-144">Nyomja le az **F4** megnyitásához a **tulajdonságok** ablak.</span><span class="sxs-lookup"><span data-stu-id="9b21a-144">Press **F4** to open the **Properties** window.</span></span> <span data-ttu-id="9b21a-145">Kattintson a **sémák** tulajdonságot a **tulajdonságok** ablak.</span><span class="sxs-lookup"><span data-stu-id="9b21a-145">Click the **Schemas** property in the **Properties** window.</span></span> <span data-ttu-id="9b21a-146">Kattintson a **...**</span><span class="sxs-lookup"><span data-stu-id="9b21a-146">Click the **…**</span></span> <span data-ttu-id="9b21a-147">az a **sémák** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="9b21a-147">in the **Schemas** property.</span></span> <span data-ttu-id="9b21a-148">Kattintson a **Hozzáadás...**</span><span class="sxs-lookup"><span data-stu-id="9b21a-148">Click the **Add…**</span></span> <span data-ttu-id="9b21a-149">gombra, és keresse meg a helyet, ahová mentette az XSD-fájlt, és válassza ki a fájlt WadConfig.xsd.</span><span class="sxs-lookup"><span data-stu-id="9b21a-149">button and navigate to the location where you saved the XSD file and select the file WadConfig.xsd.</span></span> <span data-ttu-id="9b21a-150">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="9b21a-150">Click **OK**.</span></span>

4. <span data-ttu-id="9b21a-151">Cserélje le a WadExample.xml konfigurációs fájl tartalmát az alábbi XML-fájl, és mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="9b21a-151">Replace the contents of the WadExample.xml configuration file with the following XML and save the file.</span></span> <span data-ttu-id="9b21a-152">A konfigurációs fájl határozza meg néhány gyűjtendő kérelemteljesítmény-számlálókat: egy CPU-felhasználás, egy, a memória-felhasználás.</span><span class="sxs-lookup"><span data-stu-id="9b21a-152">This configuration file defines a couple performance counters to collect: one for CPU utilization and one for memory utilization.</span></span> <span data-ttu-id="9b21a-153">A konfigurációs majd SampleEventSourceWriter osztályban módszerekhez megfelelő négy események határozza meg.</span><span class="sxs-lookup"><span data-stu-id="9b21a-153">Then the configuration defines the four events corresponding to the methods in the SampleEventSourceWriter class.</span></span>

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

### <a name="step-5-install-diagnostics-on-your-worker-role"></a><span data-ttu-id="9b21a-154">5. lépés: Telepítse a feldolgozói szerepkör diagnosztika</span><span class="sxs-lookup"><span data-stu-id="9b21a-154">Step 5: Install Diagnostics on your Worker Role</span></span>
<span data-ttu-id="9b21a-155">A diagnosztika webes vagy feldolgozói szerepkör kezelése a PowerShell-parancsmagok érhetők: Set-AzureServiceDiagnosticsExtension, a Get-AzureServiceDiagnosticsExtension és a Remove-AzureServiceDiagnosticsExtension.</span><span class="sxs-lookup"><span data-stu-id="9b21a-155">The PowerShell cmdlets for managing Diagnostics on a web or worker role are: Set-AzureServiceDiagnosticsExtension, Get-AzureServiceDiagnosticsExtension, and Remove-AzureServiceDiagnosticsExtension.</span></span>

1. <span data-ttu-id="9b21a-156">Nyissa meg az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9b21a-156">Open Azure PowerShell.</span></span>
2. <span data-ttu-id="9b21a-157">Diagnosztika a feldolgozói szerepkör telepítéséhez futtassa a parancsfájlt (csere *StorageAccountKey* wadexample tárfiók a tárolási fiók kulccsal és *config_path* az elérési útját a  *WadExample.xml* fájl):</span><span class="sxs-lookup"><span data-stu-id="9b21a-157">Execute the script to install Diagnostics on your worker role (replace *StorageAccountKey* with the storage account key for your wadexample storage account and *config_path* with the path to the *WadExample.xml* file):</span></span>

```powershell
$storage_name = "wadexample"
$key = "<StorageAccountKey>"
$config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
$service_name="wadexample"
$storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a><span data-ttu-id="9b21a-158">6. lépés: Nézze meg a telemetriai adatok</span><span class="sxs-lookup"><span data-stu-id="9b21a-158">Step 6: Look at your telemetry data</span></span>
<span data-ttu-id="9b21a-159">A Visual Studio **Server Explorer**, lépjen a wadexample tárfiókhoz.</span><span class="sxs-lookup"><span data-stu-id="9b21a-159">In the Visual Studio **Server Explorer**, navigate to the wadexample storage account.</span></span> <span data-ttu-id="9b21a-160">Miután a felhőalapú szolgáltatás körülbelül öt (5) percig futott, megjelenik a táblák **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** és **WADSetOtherTable**.</span><span class="sxs-lookup"><span data-stu-id="9b21a-160">After the cloud service has been running about five (5) minutes, you should see the tables **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** and **WADSetOtherTable**.</span></span> <span data-ttu-id="9b21a-161">Kattintson duplán a táblák gyűjtött telemetriai adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="9b21a-161">Double-click one of the tables to view the telemetry that has been collected.</span></span>

![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)

## <a name="configuration-file-schema"></a><span data-ttu-id="9b21a-163">Konfigurációs fájl séma</span><span class="sxs-lookup"><span data-stu-id="9b21a-163">Configuration File Schema</span></span>
<span data-ttu-id="9b21a-164">A diagnosztika konfigurációs fájl határozza meg a diagnosztikai konfigurációs beállítások inicializálása a diagnosztikai ügynök indulásakor használt értékek.</span><span class="sxs-lookup"><span data-stu-id="9b21a-164">The Diagnostics configuration file defines values that are used to initialize diagnostic configuration settings when the diagnostics agent starts.</span></span> <span data-ttu-id="9b21a-165">Tekintse meg a [legújabb sémareferenciája](https://msdn.microsoft.com/library/azure/mt634524.aspx) az érvényes értékek és a példákat.</span><span class="sxs-lookup"><span data-stu-id="9b21a-165">See the [latest schema reference](https://msdn.microsoft.com/library/azure/mt634524.aspx) for valid values and examples.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="9b21a-166">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="9b21a-166">Troubleshooting</span></span>
<span data-ttu-id="9b21a-167">Ha gondja támad, lásd: [hibaelhárítási Azure Diagnostics](../azure-diagnostics-troubleshooting.md) segítséget a gyakori problémák megoldásához.</span><span class="sxs-lookup"><span data-stu-id="9b21a-167">If you have trouble, see [Troubleshooting Azure Diagnostics](../azure-diagnostics-troubleshooting.md) for help with common problems.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9b21a-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9b21a-168">Next Steps</span></span>
<span data-ttu-id="9b21a-169">[A kapcsolódó Azure virtuális gép diagnosztikai cikkek megtekintéséhez](../monitoring-and-diagnostics/azure-diagnostics.md#cloud-services-using-azure-diagnostics) kapcsolatos problémák elhárítása a gyűjtött adatok módosításához vagy kapcsolatos további diagnosztikai általában.</span><span class="sxs-lookup"><span data-stu-id="9b21a-169">[See a list of related Azure virtual-machine diagnostic articles](../monitoring-and-diagnostics/azure-diagnostics.md#cloud-services-using-azure-diagnostics) to change the data you are collecting, troubleshoot problems or learn more about diagnostics in general.</span></span>

[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Free Trial]: http://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
