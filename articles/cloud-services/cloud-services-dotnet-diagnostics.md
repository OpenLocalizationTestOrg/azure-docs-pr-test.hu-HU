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
# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a>Azure Cloud Services Azure Diagnostics engedélyezése
Lásd: [Azure Diagnostics áttekintése](../azure-diagnostics.md) az Azure Diagnostics háttérként.

## <a name="how-tooenable-diagnostics-in-a-worker-role"></a>Hogyan tooEnable diagnosztika a feldolgozói szerepkörök
A forgatókönyv leírja, hogyan tooimplement egy Azure feldolgozói szerepkör, amely bocsát ki a telemetriai adatok használatával hello .NET EventSource osztály. Az Azure Diagnostics használt toocollect hello telemetriai adatokat, és tárolja az Azure storage-fiók. A feldolgozói szerepkör létrehozásakor a Visual Studio automatikusan engedélyezi a diagnosztika 1.0-s .NET 2.4 és korábbi Azure SDK-k hello megoldás részeként. hello következő útmutatások mutatják be hello folyamat hello feldolgozói szerepkör, diagnosztika 1.0 letiltása az hello megoldás és a diagnosztika 1.2-es vagy 1.3 tooyour feldolgozói szerepkör létrehozásához.

### <a name="prerequisites"></a>Előfeltételek
Ez a cikk azt feltételezi, hogy az Azure-előfizetéssel rendelkezik, és a Visual Studio hello Azure SDK-t használ. Ha nem rendelkezik Azure-előfizetéssel, a hello regisztrálhat [ingyenes][Free Trial]. Győződjön meg arról, hogy túl[telepítse és konfigurálja az Azure Powershellt 0.8.7 verzió vagy újabb][Install and configure Azure PowerShell version 0.8.7 or later].

### <a name="step-1-create-a-worker-role"></a>1. lépés: A feldolgozói szerepkör létrehozása
1. Indítsa el a **Visual Studiót**.
2. Hozzon létre egy **Azure Cloud Service** projektet a hello **felhő** sablon, amelynek célpontja a .NET-keretrendszer 4.5.  Hello projekt "WadExample" nevet, és kattintson az OK gombra.
3. Válassza ki **feldolgozói szerepkör** kattintson az OK gombra. hello projekt létrehozása.
4. A **Megoldáskezelőben**, kattintson duplán a hello **WorkerRole1** tulajdonságok fájlt.
5. A hello **konfigurációs** lap, un-ellenőrzés **engedélyezése diagnosztikai** toodisable diagnosztika 1.0 (az Azure SDK 2.4-es és korábbi).
6. A megoldás tooverify létrehozása, amely nincs hiba van.

### <a name="step-2-instrument-your-code"></a>2. lépés: Állíthatnak be a kódot
Cserélje le a következő kód hello WorkerRole.cs hello tartalmát. Öröklődés forrása: hello osztály SampleEventSourceWriter, hello [EventSource osztály][EventSource Class], négy naplózási módszerek megvalósítja: **SendEnums**, **MessageMethod** , **SetOther** és **HighFreq**. első paraméter toohello hello **WriteEvent** metódus hello megfelelő esemény hello Azonosítóját határozza meg. hello Run metódus valósítja meg, hogy egyes hello hello megvalósított naplózási módszereket hív meg végtelen hurkot **SampleEventSourceWriter** 10 másodpercenként osztályban.

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


### <a name="step-3-deploy-your-worker-role"></a>3. lépés: A feldolgozói szerepkör telepítése

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

1. A feldolgozói szerepkör tooAzure, a Visual Studio telepítése hello kiválasztásával **WadExample** projektre a Solution Explorer hello majd **közzététel** a hello **Build** menü.
2. Válassza ki az előfizetését.
3. A hello **Microsoft Azure közzétételi beállítási** párbeszédablakban válassza **új létrehozása...** .
4. A hello **felhőalapú szolgáltatás létrehozása és a Tárfiók** párbeszédpanelen adja meg egy **neve** (például "WadExample"), és válasszon régiót vagy affinitáscsoportot.
5. Set hello **környezet** túl**átmeneti**.
6. Módosíthatja bármely más **beállítások** regisztrációja, mivel a megfelelő, és kattintson a **közzététel**.
7. Központi telepítés befejezése után ellenőrizze az Azure-portál, amely a felhőszolgáltatás hello egy **futtató** állapotát.

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-hello-extension"></a>4. lépés: A diagnosztika konfigurációs fájl létrehozása és hello-kiterjesztés telepítése
1. Töltse le a hello nyilvános konfigurációs fájl sémadefiníciót a következő hello a következő PowerShell-parancs futtatásával:

    ```powershell
    (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'
    ```
2. Adja hozzá az XML-fájl tooyour **WorkerRole1** projekt, kattintson a jobb gombbal a hello **WorkerRole1** projektre, és válassza ki **Hozzáadás** -> **új elem...** -> **Visual C# elemek** -> **adatok** -> **XML-fájl**. Nevezze el "WadExample.xml" hello fájlt.

   ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)
3. Hello WadConfig.xsd társítandó hello konfigurációs fájlt. Ellenőrizze, hogy hello WadExample.xml szerkesztő ablakban hello aktív ablak. Nyomja le az **F4** tooopen hello **tulajdonságok** ablak. Kattintson a hello **sémák** hello tulajdonság **tulajdonságok** ablak. Kattintson a hello **...** a hello **sémák** tulajdonság. Kattintson a hello **hozzáadása...** gombra, és keresse meg a toohello helyet, ahová mentette hello XSD-fájlt, és válassza hello fájl WadConfig.xsd. Kattintson az **OK** gombra.

4. Cserélje le a hello hello WadExample.xml konfigurációs fájl tartalmát, a következő XML hello és hello fájl. A konfigurációs fájl határozza meg néhány teljesítmény-számlálói toocollect: egy CPU-felhasználás, egy, a memória-felhasználás. Hello konfigurációs majd hello négy események toohello módszerek hello SampleEventSourceWriter osztály a megfelelő határozza meg.

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

### <a name="step-5-install-diagnostics-on-your-worker-role"></a>5. lépés: Telepítse a feldolgozói szerepkör diagnosztika
hello diagnosztika webes vagy feldolgozói szerepkör kezelése a PowerShell-parancsmagok vannak: Set-AzureServiceDiagnosticsExtension, a Get-AzureServiceDiagnosticsExtension és a Remove-AzureServiceDiagnosticsExtension.

1. Nyissa meg az Azure PowerShell.
2. A feldolgozói szerepkör hello parancsfájl tooinstall diagnosztika hajt végre (cserélje le *StorageAccountKey* a hello tárfiók kulcsa a wadexample tárfiók és *config_path* hello elérési úttal rendelkező toohello *WadExample.xml* fájl):

```powershell
$storage_name = "wadexample"
$key = "<StorageAccountKey>"
$config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
$service_name="wadexample"
$storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a>6. lépés: Nézze meg a telemetriai adatok
A Visual Studio hello **Server Explorer**, keresse meg a toohello wadexample tárfiók. Miután hello felhőszolgáltatás körülbelül öt (5) percig futott, megtekintheti az hello táblák **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** és **WADSetOtherTable**. Kattintson duplán a hello táblák tooview hello telemetriai gyűjtött.

![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)

## <a name="configuration-file-schema"></a>Konfigurációs fájl séma
hello diagnosztika konfigurációs fájl használt tooinitialize diagnosztikai konfigurációs beállítások hello diagnosztikai ügynök indulásakor értékeket határozza meg. Lásd: hello [legújabb sémareferenciája](https://msdn.microsoft.com/library/azure/mt634524.aspx) az érvényes értékek és a példákat.

## <a name="troubleshooting"></a>Hibaelhárítás
Ha gondja támad, lásd: [hibaelhárítási Azure Diagnostics](../azure-diagnostics-troubleshooting.md) segítséget a gyakori problémák megoldásához.

## <a name="next-steps"></a>Következő lépések
[A kapcsolódó Azure virtuális gép diagnosztikai cikkek megtekintéséhez](../monitoring-and-diagnostics/azure-diagnostics.md#cloud-services-using-azure-diagnostics) toochange hello adatokat gyűjti, kapcsolatos problémák elhárítása vagy kapcsolatos további diagnosztikai általában.

[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Free Trial]: http://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
