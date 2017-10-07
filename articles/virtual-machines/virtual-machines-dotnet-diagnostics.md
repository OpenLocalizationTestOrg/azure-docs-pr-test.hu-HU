---
title: "a virtuális gépek Azure diagnostics aaaHow toouse |} Microsoft Docs"
description: "Használja az Azure diagnostics toogather adatokat az Azure virtuális gépek hibakeresés, méri a teljesítményt, figyelés, forgalom elemzése és több."
services: virtual-machines
documentationcenter: .net
author: davidmu1
manager: 
editor: 
ms.assetid: dfaabc7a-23e7-4af0-8369-f504d2915b3d
ms.service: virtual-machines
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/16/2016
ms.author: davidmu
ms.openlocfilehash: 54cdfd30d7bbbb71af449826e90234faf5ecdf44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enabling-diagnostics-in-azure-virtual-machines"></a><span data-ttu-id="1e09c-103">Az Azure virtuális gépeken diagnosztika engedélyezése</span><span class="sxs-lookup"><span data-stu-id="1e09c-103">Enabling Diagnostics in Azure Virtual Machines</span></span>
<span data-ttu-id="1e09c-104">Lásd: [Azure Diagnostics áttekintése](../monitoring-and-diagnostics/azure-diagnostics.md) az Azure Diagnostics háttérként.</span><span class="sxs-lookup"><span data-stu-id="1e09c-104">See [Azure Diagnostics Overview](../monitoring-and-diagnostics/azure-diagnostics.md) for a background on Azure Diagnostics.</span></span>

## <a name="how-tooenable-diagnostics-in-a-virtual-machine"></a><span data-ttu-id="1e09c-105">Hogyan tooEnable diagnosztika a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="1e09c-105">How tooEnable Diagnostics in a Virtual Machine</span></span>
<span data-ttu-id="1e09c-106">Ez a lépésein végighaladva ismerteti, hogyan tooremotely telepítése fejlesztési számítógépről diagnosztika tooan Azure virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="1e09c-106">This walk through describes how tooremotely install Diagnostics tooan Azure virtual machine from a development computer.</span></span> <span data-ttu-id="1e09c-107">Azt is megtudhatja, hogyan tooimplement egy alkalmazás, amely az Azure virtuális gép fut, és bocsát ki a telemetriai adatok használatával hello .NET [EventSource osztály][EventSource Class].</span><span class="sxs-lookup"><span data-stu-id="1e09c-107">You also learn how tooimplement an application that runs on that Azure virtual machine and emits telemetry data using hello .NET [EventSource Class][EventSource Class].</span></span> <span data-ttu-id="1e09c-108">Az Azure Diagnostics használt toocollect hello telemetriai, és tárolja az Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="1e09c-108">Azure Diagnostics is used toocollect hello telemetry and store it in an Azure storage account.</span></span>

### <a name="pre-requisites"></a><span data-ttu-id="1e09c-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1e09c-109">Pre-requisites</span></span>
<span data-ttu-id="1e09c-110">Ez a lépésein végighaladva azt feltételezi, hogy az Azure-előfizetéssel rendelkezik, és a Visual Studio 2017 hello Azure SDK-t használ.</span><span class="sxs-lookup"><span data-stu-id="1e09c-110">This walk through assumes you have an Azure subscription and are using Visual Studio 2017 with hello Azure SDK.</span></span> <span data-ttu-id="1e09c-111">Ha nem rendelkezik Azure-előfizetéssel, a hello regisztrálhat [ingyenes][Free Trial].</span><span class="sxs-lookup"><span data-stu-id="1e09c-111">If you do not have an Azure subscription, you can sign up for hello [Free Trial][Free Trial].</span></span> <span data-ttu-id="1e09c-112">Győződjön meg arról, hogy túl[telepítse és konfigurálja az Azure Powershellt 0.8.7 verzió vagy újabb][Install and configure Azure PowerShell version 0.8.7 or later].</span><span class="sxs-lookup"><span data-stu-id="1e09c-112">Make sure too[Install and configure Azure PowerShell version 0.8.7 or later][Install and configure Azure PowerShell version 0.8.7 or later].</span></span>

### <a name="step-1-create-a-virtual-machine"></a><span data-ttu-id="1e09c-113">1. lépés: Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="1e09c-113">Step 1: Create a Virtual Machine</span></span>
1. <span data-ttu-id="1e09c-114">A fejlesztési számítógépen indítsa el a Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="1e09c-114">On your development computer, launch Visual Studio 2017.</span></span>
2. <span data-ttu-id="1e09c-115">A Visual Studio hello **Server Explorer** bontsa ki a **Azure**, kattintson a jobb gombbal **virtuális gépek** válassza **virtuális gép létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="1e09c-115">In hello Visual Studio **Server Explorer** expand **Azure**, right-click **Virtual Machines** then select **Create Virtual Machine**.</span></span>
3. <span data-ttu-id="1e09c-116">Válassza ki az Azure-előfizetéshez hello **válasszon egy előfizetést** párbeszédpanelt majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="1e09c-116">Select your Azure subscription in hello **Choose a Subscription** dialog and click **Next**.</span></span>
4. <span data-ttu-id="1e09c-117">Válassza ki **Windows Server 2012 R2 Datacenter, június 2017** a hello **válassza ki a virtuálisgép-lemezkép** párbeszédpanel, kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="1e09c-117">Select **Windows Server 2012 R2 Datacenter, June 2017** in hello **Select a Virtual Machine Image** dialog and click **Next**.</span></span>
5. <span data-ttu-id="1e09c-118">A hello **virtuális gép alapbeállítások**, hello virtuális gép neve túl beállítása "wadexample".</span><span class="sxs-lookup"><span data-stu-id="1e09c-118">In hello **Virtual Machine Basic Settings**, set hello virtual machine name too"wadexample".</span></span> <span data-ttu-id="1e09c-119">Állítsa be a rendszergazda felhasználónevét és jelszavát, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="1e09c-119">Set your Administrator user name and password and click **Next**.</span></span>
6. <span data-ttu-id="1e09c-120">A hello **felhőalapú szolgáltatás beállításainak** párbeszédpanel "wadexampleVM" nevű új felhőalapú szolgáltatás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1e09c-120">In hello **Cloud Service Settings** dialog create a new cloud service named "wadexampleVM".</span></span> <span data-ttu-id="1e09c-121">Hozzon létre egy új tárfiókot "wadexample" nevű, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="1e09c-121">Create a new Storage account named "wadexample" and click **Next**.</span></span>
7. <span data-ttu-id="1e09c-122">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="1e09c-122">Click **Create**.</span></span>

### <a name="step-2-create-your-application"></a><span data-ttu-id="1e09c-123">2. lépés: Az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1e09c-123">Step 2: Create your Application</span></span>
1. <span data-ttu-id="1e09c-124">A fejlesztési számítógépen indítsa el a Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="1e09c-124">On your development computer, launch Visual Studio 2017.</span></span>
2. <span data-ttu-id="1e09c-125">Hozzon létre egy új Visual C# Konzolalkalmazást, amelynek célpontja a .NET-keretrendszer 4.5.</span><span class="sxs-lookup"><span data-stu-id="1e09c-125">Create a new Visual C# Console Application that targets .NET Framework 4.5.</span></span> <span data-ttu-id="1e09c-126">Hello projekt neve "WadExampleVM".</span><span class="sxs-lookup"><span data-stu-id="1e09c-126">Name hello project "WadExampleVM".</span></span>

   ![CloudServices_diag_new_project](./media/virtual-machines-dotnet-diagnostics/NewProject.png)
3. <span data-ttu-id="1e09c-128">Cserélje le a Program.cs tartalmát hello hello a következő kódot.</span><span class="sxs-lookup"><span data-stu-id="1e09c-128">Replace hello contents of Program.cs with hello following code.</span></span> <span data-ttu-id="1e09c-129">osztály hello **SampleEventSourceWriter** négy naplózási módszerek megvalósítja: **SendEnums**, **MessageMethod**, **SetOther** és  **HighFreq**.</span><span class="sxs-lookup"><span data-stu-id="1e09c-129">hello class **SampleEventSourceWriter** implements four logging methods: **SendEnums**, **MessageMethod**, **SetOther** and **HighFreq**.</span></span> <span data-ttu-id="1e09c-130">hello első paraméter toohello WriteEvent metódus hello megfelelő esemény hello Azonosítóját határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1e09c-130">hello first parameter toohello WriteEvent method defines hello ID for hello respective event.</span></span> <span data-ttu-id="1e09c-131">hello Run metódus valósítja meg, hogy egyes hello hello megvalósított naplózási módszereket hív meg végtelen hurkot **SampleEventSourceWriter** 10 másodpercenként osztályban.</span><span class="sxs-lookup"><span data-stu-id="1e09c-131">hello Run method implements an infinite loop that calls each of hello logging methods implemented in hello **SampleEventSourceWriter** class every 10 seconds.</span></span>

    ```csharp
     using System;
     using System.Diagnostics;
     using System.Diagnostics.Tracing;
     using System.Threading;

     namespace WadExampleVM
     {
       sealed class SampleEventSourceWriter : EventSource {
         public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
         public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); } // Cast enums tooint for efficient logging.
         public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
         public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
         public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }
       }

       enum MyColor {
         Red,
         Blue,
         Green
       }

       [Flags]
       enum MyFlags {
         Flag1 = 1,
         Flag2 = 2,
         Flag3 = 4
       }

       class Program
       {
         static void Main(string[] args) {
         Trace.TraceInformation("My application entry point called");

         int value = 0;

         while (true) {
             Thread.Sleep(10000);
             Trace.TraceInformation("Working");

             // Emit several events every time we go through hello loop
             for (int i = 0; i < 6; i++) {
                 SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
             }

             for (int i = 0; i < 3; i++) {
                 SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                 SampleEventSourceWriter.Log.SetOther(true, 123456789);
             }

             if (value == int.MaxValue) value = 0;
             SampleEventSourceWriter.Log.HighFreq(value++);
         }

        }
      }
     }
     ```
4. <span data-ttu-id="1e09c-132">Mentse hello fájlt, és válassza ki **megoldás fordítása** a hello **Build** menü toobuild a kódot.</span><span class="sxs-lookup"><span data-stu-id="1e09c-132">Save hello file and select **Build Solution** from hello **Build** menu toobuild your code.</span></span>

### <a name="step-3-deploy-your-application"></a><span data-ttu-id="1e09c-133">3. lépés: Az alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="1e09c-133">Step 3: Deploy your Application</span></span>
1. <span data-ttu-id="1e09c-134">Kattintson a jobb gombbal a hello **WadExampleVM** a projekt **Megoldáskezelőben** válassza **mappa megnyitása a Fájlkezelőben**.</span><span class="sxs-lookup"><span data-stu-id="1e09c-134">Right-click on hello **WadExampleVM** project in **Solution Explorer** and choose **Open Folder in File Explorer**.</span></span>
2. <span data-ttu-id="1e09c-135">Keresse meg a toohello *bin\Debug* mappa és az összes hello másolja a fájlokat (WadExampleVM.*)</span><span class="sxs-lookup"><span data-stu-id="1e09c-135">Navigate toohello *bin\Debug* folder and copy all hello files (WadExampleVM.*)</span></span>
3. <span data-ttu-id="1e09c-136">A **Server Explorer** kattintson a jobb gombbal a hello virtuális gépen, és válassza a **csatlakozzon a távoli asztali kapcsolattal**.</span><span class="sxs-lookup"><span data-stu-id="1e09c-136">In **Server Explorer** right-click on hello virtual machine and choose **Connect using Remote Desktop**.</span></span>
4. <span data-ttu-id="1e09c-137">Miután csatlakozott a virtuális gép toohello WadExampleVM nevű mappa létrehozásához, és illessze be az alkalmazásfájlokat hello mappába.</span><span class="sxs-lookup"><span data-stu-id="1e09c-137">Once connected toohello VM create a folder named WadExampleVM and paste your application files into hello folder.</span></span>
5. <span data-ttu-id="1e09c-138">Indítsa el a hello alkalmazás WadExampleVM.exe.</span><span class="sxs-lookup"><span data-stu-id="1e09c-138">Launch hello application WadExampleVM.exe.</span></span> <span data-ttu-id="1e09c-139">Meg kell jelennie egy üres konzol ablakot.</span><span class="sxs-lookup"><span data-stu-id="1e09c-139">You should see a blank console window.</span></span>

### <a name="step-4-create-your-diagnostics-configuration-and-install-hello-extension"></a><span data-ttu-id="1e09c-140">4. lépés: A diagnosztika-konfiguráció létrehozása és hello bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="1e09c-140">Step 4: Create your Diagnostics configuration and install hello Extension</span></span>
1. <span data-ttu-id="1e09c-141">Töltse le a hello nyilvános konfigurációs fájl schema definíció tooyour fejlesztési számítógépen a következő hello a következő PowerShell-parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1e09c-141">Download hello public configuration file schema definition tooyour development computer by executing hello following PowerShell command:</span></span>

     <span data-ttu-id="1e09c-142">(Get-AzureServiceAvailableExtension - bővítménynév "PaaSDiagnostics" - ProviderNamespace "Microsoft.Azure.Diagnostics"). PublicConfigurationSchema |} Out-File-kódolás utf8 - fájl elérési útja "WadConfig.xsd"</span><span class="sxs-lookup"><span data-stu-id="1e09c-142">(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'</span></span>
2. <span data-ttu-id="1e09c-143">Nyisson meg egy új XML-fájl a Visual Studióban, akár egy nyitott projekt projekt már nyitva van, vagy a Visual Studio-példány.</span><span class="sxs-lookup"><span data-stu-id="1e09c-143">Open a new XML file in Visual Studio, either in a project you already have open or in a Visual Studio instance with no open projects.</span></span> <span data-ttu-id="1e09c-144">A Visual Studio válassza **Hozzáadás** -> **új elem...**</span><span class="sxs-lookup"><span data-stu-id="1e09c-144">In Visual Studio, select **Add** -> **New Item…**</span></span><span data-ttu-id="1e09c-145"> -> **Visual C# elemek** -> **adatok** -> **XML-fájl**.</span><span class="sxs-lookup"><span data-stu-id="1e09c-145"> -> **Visual C# items** -> **Data** -> **XML File**.</span></span> <span data-ttu-id="1e09c-146">Nevű hello fájl "WadExample.xml"</span><span class="sxs-lookup"><span data-stu-id="1e09c-146">Name hello file "WadExample.xml"</span></span>
3. <span data-ttu-id="1e09c-147">Hello WadConfig.xsd társítandó hello konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="1e09c-147">Associate hello WadConfig.xsd with hello configuration file.</span></span> <span data-ttu-id="1e09c-148">Ellenőrizze, hogy hello WadExample.xml szerkesztő ablakban hello aktív ablak.</span><span class="sxs-lookup"><span data-stu-id="1e09c-148">Make sure hello WadExample.xml editor window is hello active window.</span></span> <span data-ttu-id="1e09c-149">Nyomja le az **F4** tooopen hello **tulajdonságok** ablak.</span><span class="sxs-lookup"><span data-stu-id="1e09c-149">Press **F4** tooopen hello **Properties** window.</span></span> <span data-ttu-id="1e09c-150">Kattintson a hello **sémák** hello tulajdonság **tulajdonságok** ablak.</span><span class="sxs-lookup"><span data-stu-id="1e09c-150">Click on hello **Schemas** property in hello **Properties** window.</span></span> <span data-ttu-id="1e09c-151">Kattintson a hello **...**</span><span class="sxs-lookup"><span data-stu-id="1e09c-151">Click hello **…**</span></span> <span data-ttu-id="1e09c-152">a hello **sémák** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="1e09c-152">in hello **Schemas** property.</span></span> <span data-ttu-id="1e09c-153">Kattintson a hello **hozzáadása...**</span><span class="sxs-lookup"><span data-stu-id="1e09c-153">Click hello **Add…**</span></span> <span data-ttu-id="1e09c-154">gombra, és keresse meg a toohello helyet, ahová mentette hello XSD-fájlt, és válassza hello fájl WadConfig.xsd.</span><span class="sxs-lookup"><span data-stu-id="1e09c-154">button and navigate toohello location where you saved hello XSD file and select hello file WadConfig.xsd.</span></span> <span data-ttu-id="1e09c-155">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="1e09c-155">Click **OK**.</span></span>
4. <span data-ttu-id="1e09c-156">Cserélje le a hello hello WadExample.xml konfigurációs fájl tartalmát, a következő XML hello és hello fájl.</span><span class="sxs-lookup"><span data-stu-id="1e09c-156">Replace hello contents of hello WadExample.xml configuration file with hello following XML and save hello file.</span></span> <span data-ttu-id="1e09c-157">A konfigurációs fájl határozza meg néhány teljesítmény-számlálói toocollect: egy CPU-felhasználás, egy, a memória-felhasználás.</span><span class="sxs-lookup"><span data-stu-id="1e09c-157">This configuration file defines a couple performance counters toocollect: one for CPU utilization and one for memory utilization.</span></span> <span data-ttu-id="1e09c-158">Hello konfigurációs majd hello négy események toohello módszerek hello SampleEventSourceWriter osztály a megfelelő határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1e09c-158">Then hello configuration defines hello four events corresponding toohello methods in hello SampleEventSourceWriter class.</span></span>

```
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

### <a name="step-5-remotely-install-diagnostics-on-your-azure-virtual-machine"></a><span data-ttu-id="1e09c-159">5. lépés: Távoli telepítését diagnosztika az Azure virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="1e09c-159">Step 5: Remotely install Diagnostics on your Azure Virtual Machine</span></span>
<span data-ttu-id="1e09c-160">hello diagnosztika a virtuális gép kezelésére szolgáló PowerShell-parancsmagok vannak: Set-AzureVMDiagnosticsExtension, a Get-AzureVMDiagnosticsExtension és a Remove-AzureVMDiagnosticsExtension.</span><span class="sxs-lookup"><span data-stu-id="1e09c-160">hello PowerShell cmdlets for managing Diagnostics on a VM are: Set-AzureVMDiagnosticsExtension, Get-AzureVMDiagnosticsExtension, and Remove-AzureVMDiagnosticsExtension.</span></span>

1. <span data-ttu-id="1e09c-161">A fejlesztői számítógépen nyissa meg az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1e09c-161">On your developer computer, open Azure PowerShell.</span></span>
2. <span data-ttu-id="1e09c-162">Hello parancsfájl tooremotely telepítés diagnosztika a virtuális gép hajt végre (csere `<user>` directory felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="1e09c-162">Execute hello script tooremotely install Diagnostics on your VM (Replace `<user>` with your user directory name.</span></span> <span data-ttu-id="1e09c-163">Cserélje le `<StorageAccountKey>` a hello tárfiók kulcsa a wadexamplevm tárfiók):</span><span class="sxs-lookup"><span data-stu-id="1e09c-163">Replace `<StorageAccountKey>` with hello storage account key for your wadexamplevm storage account):</span></span>
```
     $storage_name = "wadexamplevm"
     $key = "<StorageAccountKey>"
     $config_path="c:\users\<user>\documents\visual studio 2017\Projects\WadExampleVM\WadExampleVM\WadExample.xml"
     $service_name="wadexamplevm"
     $vm_name="WadExample"
     $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
     $VM1 = Get-AzureVM -ServiceName $service_name -Name $vm_name
     $VM2 = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $config_path -Version "1.*" -VM $VM1 -StorageContext $storageContext
     $VM3 = Update-AzureVM -ServiceName $service_name -Name $vm_name -VM $VM2.VM
```
### <a name="step-6-look-at-your-telemetry-data"></a><span data-ttu-id="1e09c-164">6. lépés: Nézze meg a telemetriai adatok</span><span class="sxs-lookup"><span data-stu-id="1e09c-164">Step 6: Look at your telemetry data</span></span>
<span data-ttu-id="1e09c-165">A Visual Studio hello **Server Explorer** toohello wadexample tárfiók lépjen.</span><span class="sxs-lookup"><span data-stu-id="1e09c-165">In hello Visual Studio **Server Explorer** navigate toohello wadexample storage account.</span></span> <span data-ttu-id="1e09c-166">Virtuális gép fut körülbelül 5 percig hello megtekintheti az hello táblák **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**,  **WADPerformanceCountersTable** és **WADSetOtherTable**.</span><span class="sxs-lookup"><span data-stu-id="1e09c-166">After hello VM has been running about 5 minutes you should see hello tables **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** and **WADSetOtherTable**.</span></span> <span data-ttu-id="1e09c-167">Kattintson duplán a hello táblák tooview hello telemetriai gyűjtött egyikét.</span><span class="sxs-lookup"><span data-stu-id="1e09c-167">Double-click on one of hello tables tooview hello telemetry that has been collected.</span></span>

![CloudServices_diag_wadexamplevm_tables](./media/virtual-machines-dotnet-diagnostics/WadExampleVMTables.png)

## <a name="configuration-file-schema"></a><span data-ttu-id="1e09c-169">Konfigurációs fájl séma</span><span class="sxs-lookup"><span data-stu-id="1e09c-169">Configuration file schema</span></span>
<span data-ttu-id="1e09c-170">hello diagnosztika konfigurációs fájl használt tooinitialize diagnosztikai konfigurációs beállítások hello diagnosztikai ügynök indulásakor értékeket határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1e09c-170">hello Diagnostics configuration file defines values that are used tooinitialize diagnostic configuration settings when hello diagnostics agent starts.</span></span> <span data-ttu-id="1e09c-171">Lásd: hello [legújabb sémareferenciája](https://msdn.microsoft.com/library/azure/mt634524.aspx) az érvényes értékek és a példákat.</span><span class="sxs-lookup"><span data-stu-id="1e09c-171">See hello [latest schema reference](https://msdn.microsoft.com/library/azure/mt634524.aspx) for valid values and examples.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="1e09c-172">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="1e09c-172">Troubleshooting</span></span>
<span data-ttu-id="1e09c-173">Lásd: [hibaelhárítási Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics-troubleshooting.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="1e09c-173">See [Troubleshooting Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics-troubleshooting.md) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1e09c-174">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1e09c-174">Next steps</span></span>
<span data-ttu-id="1e09c-175">[A virtuális gép listáját lásd: kapcsolatos Azure Diagnostics cikkek](../monitoring-and-diagnostics/azure-diagnostics.md#virtual-machines-using-azure-diagnostics) toochange hello adatokat gyűjti, kapcsolatos problémák elhárítása vagy kapcsolatos további diagnosztikai általában.</span><span class="sxs-lookup"><span data-stu-id="1e09c-175">[See a list of virtual machine related Azure Diagnostics articles](../monitoring-and-diagnostics/azure-diagnostics.md#virtual-machines-using-azure-diagnostics) toochange hello data you are collecting, troubleshoot problems or learn more about diagnostics in general.</span></span>

[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Free Trial]: http://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
