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
# <a name="enabling-diagnostics-in-azure-virtual-machines"></a>Az Azure virtuális gépeken diagnosztika engedélyezése
Lásd: [Azure Diagnostics áttekintése](../monitoring-and-diagnostics/azure-diagnostics.md) az Azure Diagnostics háttérként.

## <a name="how-tooenable-diagnostics-in-a-virtual-machine"></a>Hogyan tooEnable diagnosztika a virtuális gép
Ez a lépésein végighaladva ismerteti, hogyan tooremotely telepítése fejlesztési számítógépről diagnosztika tooan Azure virtuális gép. Azt is megtudhatja, hogyan tooimplement egy alkalmazás, amely az Azure virtuális gép fut, és bocsát ki a telemetriai adatok használatával hello .NET [EventSource osztály][EventSource Class]. Az Azure Diagnostics használt toocollect hello telemetriai, és tárolja az Azure storage-fiók.

### <a name="pre-requisites"></a>Előfeltételek
Ez a lépésein végighaladva azt feltételezi, hogy az Azure-előfizetéssel rendelkezik, és a Visual Studio 2017 hello Azure SDK-t használ. Ha nem rendelkezik Azure-előfizetéssel, a hello regisztrálhat [ingyenes][Free Trial]. Győződjön meg arról, hogy túl[telepítse és konfigurálja az Azure Powershellt 0.8.7 verzió vagy újabb][Install and configure Azure PowerShell version 0.8.7 or later].

### <a name="step-1-create-a-virtual-machine"></a>1. lépés: Virtuális gép létrehozása
1. A fejlesztési számítógépen indítsa el a Visual Studio 2017.
2. A Visual Studio hello **Server Explorer** bontsa ki a **Azure**, kattintson a jobb gombbal **virtuális gépek** válassza **virtuális gép létrehozása**.
3. Válassza ki az Azure-előfizetéshez hello **válasszon egy előfizetést** párbeszédpanelt majd **következő**.
4. Válassza ki **Windows Server 2012 R2 Datacenter, június 2017** a hello **válassza ki a virtuálisgép-lemezkép** párbeszédpanel, kattintson **következő**.
5. A hello **virtuális gép alapbeállítások**, hello virtuális gép neve túl beállítása "wadexample". Állítsa be a rendszergazda felhasználónevét és jelszavát, és kattintson a **következő**.
6. A hello **felhőalapú szolgáltatás beállításainak** párbeszédpanel "wadexampleVM" nevű új felhőalapú szolgáltatás létrehozása. Hozzon létre egy új tárfiókot "wadexample" nevű, és kattintson a **következő**.
7. Kattintson a **Create** (Létrehozás) gombra.

### <a name="step-2-create-your-application"></a>2. lépés: Az alkalmazás létrehozása
1. A fejlesztési számítógépen indítsa el a Visual Studio 2017.
2. Hozzon létre egy új Visual C# Konzolalkalmazást, amelynek célpontja a .NET-keretrendszer 4.5. Hello projekt neve "WadExampleVM".

   ![CloudServices_diag_new_project](./media/virtual-machines-dotnet-diagnostics/NewProject.png)
3. Cserélje le a Program.cs tartalmát hello hello a következő kódot. osztály hello **SampleEventSourceWriter** négy naplózási módszerek megvalósítja: **SendEnums**, **MessageMethod**, **SetOther** és  **HighFreq**. hello első paraméter toohello WriteEvent metódus hello megfelelő esemény hello Azonosítóját határozza meg. hello Run metódus valósítja meg, hogy egyes hello hello megvalósított naplózási módszereket hív meg végtelen hurkot **SampleEventSourceWriter** 10 másodpercenként osztályban.

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
4. Mentse hello fájlt, és válassza ki **megoldás fordítása** a hello **Build** menü toobuild a kódot.

### <a name="step-3-deploy-your-application"></a>3. lépés: Az alkalmazás központi telepítése
1. Kattintson a jobb gombbal a hello **WadExampleVM** a projekt **Megoldáskezelőben** válassza **mappa megnyitása a Fájlkezelőben**.
2. Keresse meg a toohello *bin\Debug* mappa és az összes hello másolja a fájlokat (WadExampleVM.*)
3. A **Server Explorer** kattintson a jobb gombbal a hello virtuális gépen, és válassza a **csatlakozzon a távoli asztali kapcsolattal**.
4. Miután csatlakozott a virtuális gép toohello WadExampleVM nevű mappa létrehozásához, és illessze be az alkalmazásfájlokat hello mappába.
5. Indítsa el a hello alkalmazás WadExampleVM.exe. Meg kell jelennie egy üres konzol ablakot.

### <a name="step-4-create-your-diagnostics-configuration-and-install-hello-extension"></a>4. lépés: A diagnosztika-konfiguráció létrehozása és hello bővítmény telepítése
1. Töltse le a hello nyilvános konfigurációs fájl schema definíció tooyour fejlesztési számítógépen a következő hello a következő PowerShell-parancs futtatásával:

     (Get-AzureServiceAvailableExtension - bővítménynév "PaaSDiagnostics" - ProviderNamespace "Microsoft.Azure.Diagnostics"). PublicConfigurationSchema |} Out-File-kódolás utf8 - fájl elérési útja "WadConfig.xsd"
2. Nyisson meg egy új XML-fájl a Visual Studióban, akár egy nyitott projekt projekt már nyitva van, vagy a Visual Studio-példány. A Visual Studio válassza **Hozzáadás** -> **új elem...** -> **Visual C# elemek** -> **adatok** -> **XML-fájl**. Nevű hello fájl "WadExample.xml"
3. Hello WadConfig.xsd társítandó hello konfigurációs fájlt. Ellenőrizze, hogy hello WadExample.xml szerkesztő ablakban hello aktív ablak. Nyomja le az **F4** tooopen hello **tulajdonságok** ablak. Kattintson a hello **sémák** hello tulajdonság **tulajdonságok** ablak. Kattintson a hello **...** a hello **sémák** tulajdonság. Kattintson a hello **hozzáadása...** gombra, és keresse meg a toohello helyet, ahová mentette hello XSD-fájlt, és válassza hello fájl WadConfig.xsd. Kattintson az **OK** gombra.
4. Cserélje le a hello hello WadExample.xml konfigurációs fájl tartalmát, a következő XML hello és hello fájl. A konfigurációs fájl határozza meg néhány teljesítmény-számlálói toocollect: egy CPU-felhasználás, egy, a memória-felhasználás. Hello konfigurációs majd hello négy események toohello módszerek hello SampleEventSourceWriter osztály a megfelelő határozza meg.

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

### <a name="step-5-remotely-install-diagnostics-on-your-azure-virtual-machine"></a>5. lépés: Távoli telepítését diagnosztika az Azure virtuális gépen
hello diagnosztika a virtuális gép kezelésére szolgáló PowerShell-parancsmagok vannak: Set-AzureVMDiagnosticsExtension, a Get-AzureVMDiagnosticsExtension és a Remove-AzureVMDiagnosticsExtension.

1. A fejlesztői számítógépen nyissa meg az Azure PowerShell.
2. Hello parancsfájl tooremotely telepítés diagnosztika a virtuális gép hajt végre (csere `<user>` directory felhasználónevet. Cserélje le `<StorageAccountKey>` a hello tárfiók kulcsa a wadexamplevm tárfiók):
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
### <a name="step-6-look-at-your-telemetry-data"></a>6. lépés: Nézze meg a telemetriai adatok
A Visual Studio hello **Server Explorer** toohello wadexample tárfiók lépjen. Virtuális gép fut körülbelül 5 percig hello megtekintheti az hello táblák **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**,  **WADPerformanceCountersTable** és **WADSetOtherTable**. Kattintson duplán a hello táblák tooview hello telemetriai gyűjtött egyikét.

![CloudServices_diag_wadexamplevm_tables](./media/virtual-machines-dotnet-diagnostics/WadExampleVMTables.png)

## <a name="configuration-file-schema"></a>Konfigurációs fájl séma
hello diagnosztika konfigurációs fájl használt tooinitialize diagnosztikai konfigurációs beállítások hello diagnosztikai ügynök indulásakor értékeket határozza meg. Lásd: hello [legújabb sémareferenciája](https://msdn.microsoft.com/library/azure/mt634524.aspx) az érvényes értékek és a példákat.

## <a name="troubleshooting"></a>Hibaelhárítás
Lásd: [hibaelhárítási Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics-troubleshooting.md) további információt.

## <a name="next-steps"></a>Következő lépések
[A virtuális gép listáját lásd: kapcsolatos Azure Diagnostics cikkek](../monitoring-and-diagnostics/azure-diagnostics.md#virtual-machines-using-azure-diagnostics) toochange hello adatokat gyűjti, kapcsolatos problémák elhárítása vagy kapcsolatos további diagnosztikai általában.

[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Free Trial]: http://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
