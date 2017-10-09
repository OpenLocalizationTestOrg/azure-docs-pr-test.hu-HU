---
title: egy Azure IoT Edge-modul a C# aaaCreate |} Microsoft Docs
description: "Ez az oktatóanyag bővíthető, hogy egy BLA konverter modul használatával végzett toowrite hello legújabb Azure IoT peremhálózati NuGet-csomagok, Visual Studio Code és C#."
services: iot-hub
author: jeffreyCline
manager: timlt
keywords: "Azure, iot, oktatóanyag, a modul, nuget, vscode, c Sharp, peremhálózati"
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: jcline
ms.openlocfilehash: b104609c05d1613e21acc7d7bed547f311179151
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-iot-edge-module-with-cx23"></a>Egy Azure IoT Edge-modul létrehozása a C & #x23;

Ez az oktatóanyag bővíthető hogyan toocreate a modul a `Azure IoT Edge` használatával `Visual Studio Code` és `C#`.

Az oktatóanyag azt bízná környezet beállításról, és hogyan toowrite egy [Gedélyezése](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) legújabb konverter modul használatával végzett hello `Azure IoT Edge NuGet` csomagok. 

>[!NOTE]
Ez az oktatóanyag használ hello `.NET Core SDK`, amely támogatja a platformfüggetlen. hello következő oktatóanyag íródott hello `Windows 10` operációs rendszer. Ez az oktatóanyag hello parancsainak függően eltérő lehet a `development environment`. 

## <a name="prerequisites"></a>Előfeltételek

Ez a szakasz azt beállításról a környezetet az `Azure IoT Edge` modul fejlesztési. Ez a kategória tooboth **64 bites Windows** és **64 bites Linux (8 Ubuntu/Debian)** operációs rendszerek.

a következő szoftver hello szükség:

- [Git-ügyfél](https://git-scm.com/downloads)
- [.NET Core SDK](https://www.microsoft.com/net/core#windowscmd)
- [Visual Studio Code](https://code.visualstudio.com/)

Ez a minta a tooclone hello tárház nem kell, azonban az összes hello példakód tárgyalt ebben az oktatóanyagban található a következő tárházat hello:

- `git clone https://github.com/Azure-Samples/iot-edge-samples.git`.
- `cd iot-edge-samples/dotnetcore/simulated_ble`

## <a name="getting-started"></a>Bevezetés

1. Telepítés `.NET Core SDK`.
2. Telepítés `Visual Studio Code` és hello `C# extension` a Visual Studio Code piactér hello.

Ezek megtekintéséhez [gyors videó oktatóanyag](https://channel9.msdn.com/Blogs/dotnet/Get-started-VSCode-Csharp-NET-Core-Windows) kapcsolatos hogyan tooget használatának `Visual Studio Code` és hello `.NET Core SDK`.

## <a name="creating-hello-azure-iot-edge-converter-module"></a>Hello Azure IoT peremhálózati konverter modul létrehozása

1. Egy új inicializálása `.NET Core` C# hordozhatóosztálytár-projektjének:
    - Nyisson meg egy parancssort (`Windows + R` -> `cmd` -> `enter`).
    - Keresse meg a toocreate hello helyének toohello mappa `C#` projekt.
    - Típus **dotnet új classlib -o IoTEdgeConverterModule -f netstandard1.3**. 
    - Ez a parancs létrehoz egy üres osztályt `Class1.cs` a projekteket tartalmazó könyvtárban található.
2. Keresse meg a toohello mappát, ahol imént létrehozott hello hordozhatóosztálytár-projektjének beírásával **cd IoTEdgeConverterModule**.
3. Nyissa meg hello projektre a `Visual Studio Code` beírásával **kód.**.
4. Miután hello projekt meg van nyitva, a `Visual Studio Code`, kattintson a hello **IoTEdgeConverterModule.csproj** tooopen hello fájl látható hello kép a következő módon:

    ![A Visual Studio Code Szerkesztés ablak](media/iot-hub-iot-edge-create-module/vscode-edit-csproj.png)

5. Hello beszúrása `XML` megjelennek a következő kódrészletet közötti hello záró hello blob `PropertyGroup` címkét, és a záró hello `Project` címkét; hat kép megelőző hello. sor, és hello fájl mentése billentyűkombináció lenyomásával `Ctrl`  +  `S`.

   ```xml
     <ItemGroup>
       <PackageReference Include="Microsoft.Azure.Devices.Gateway.Module.NetStandard" Version="1.0.5" />
       <PackageReference Include="System.Threading.Thread" Version="4.3.0" />
       <PackageReference Include="Newtonsoft.Json" Version="10.0.2" />
     </ItemGroup> 
   ```

6. Miután menti hello `.csproj` fájl, `Visual Studio Code` kell kérni a egy `unresolved dependencies` párbeszédpanelen a következő kép hello látható módon: 

    ![A Visual Studio Code visszaállítási függőségek párbeszédpanel](media/iot-hub-iot-edge-create-module/vscode-restore.png)

    a) kattintson `Restore` összes hello sorszámra hello projektek toorestore `.csproj` hello például `PackageReferences` jelentek meg. 

    b) `Visual Studio Code` automatikusan létrehozza a hello `project.assets.json` fájlt a projektben `obj` mappát. Ezt a fájlt a projekt függőségek toomake későbbi visszaállítások gyorsabb információkat tartalmaz.
 
    >[!NOTE]
    `.NET Core Tools`jelenleg MSBuild-alapú. Ez azt jelenti a `.csproj` projekt fájlt hoz létre ahelyett, hogy egy `project.json`.

    - Ha `Visual Studio Code` nem kéri a felhasználót, hogy így megfelelő, azt megteheti manuálisan. Nyissa meg hello `Visual Studio Code` által lenyomásával hello integrált terminálablakot `Ctrl`  +  `backtick` a kulcsok vagy hello menük `View`  ->  `Integrated Terminal`.
    - A hello `Integrated Terminal` időszak típusa **dotnet visszaállítási**.
    
7. Nevezze át a hello `Class1.cs` fájl túl`BleConverterModule.cs`. 

    a) toorename hello fájl először kattintson hello fájlra, majd nyomja le az hello `F2` kulcs.
    
    b) az új név hello típus **BleConverterModule**, a következő kép hello látható módon:

    ![A Visual Studio Code osztály átnevezése](media/iot-hub-iot-edge-create-module/vscode-rename.png)

8. Cserélje le a meglévő kód hello hello `BleConverterModule.cs` másolás és beillesztés hello a következő kódrészletet a fájlt a `BleConverterModule.cs` fájlt.

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Globalization;
   using System.Linq;
   using System.Text;
   using System.Threading;
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Gateway;
   using Newtonsoft.Json;
   
   namespace IoTEdgeConverterModule
   {
       public class BleConverterModule : IGatewayModule, IGatewayModuleStart
       {
           private Broker broker;
           private string configuration;
           private int messageCount;

           public void Create(Broker broker, byte[] configuration)
           {
               this.broker = broker;
               this.configuration = System.Text.Encoding.UTF8.GetString(configuration);
           }
   
           public void Start()
           {
           }

           public void Destroy()
           {
           }

           public void Receive(Message received_message)
           {
               string recMsg = Encoding.UTF8.GetString(received_message.Content, 0, received_message.Content.Length);
               BleData receivedData = JsonConvert.DeserializeObject<BleData>(recMsg);

               float temperature = float.Parse(receivedData.Temperature, CultureInfo.InvariantCulture.NumberFormat); 
               Dictionary<string, string> receivedProperties = received_message.Properties;
            
               Dictionary<string, string> properties = new Dictionary<string, string>();
               properties.Add("source", receivedProperties["source"]);
               properties.Add("macAddress", receivedProperties["macAddress"]);
               properties.Add("temperatureAlert", temperature > 30 ? "true" : "false");
   
               String content = String.Format("{0} \"deviceId\": \"Intel NUC Gateway\", \"messageId\": {1}, \"temperature\": {2} {3}", "{", ++this.messageCount, temperature, "}");
               Message messageToPublish = new Message(content, properties);
   
               this.broker.Publish(messageToPublish);
           }
       }
   }
   ```

9. Hello fájl mentése billentyűkombináció lenyomásával `Ctrl`  +  `S`.

10. Hozzon létre egy új fájlt nevű `Untitled-1` által lenyomásával hello `Ctrl`  +  `N` kulcsok hello kép a következő látható:

    ![A Visual Studio Code új fájl](media/iot-hub-iot-edge-create-module/vscode-new-file.png)

11. toodeserialize hello `JSON` szimulált hello szervezettől kapott objektum `BLE` eszköz, a kód hello be a következő másolási hello `Untitled-1` fájl kód szerkesztő ablakban. 

   ```csharp
   using System;
   using Newtonsoft.Json;

   namespace IoTEdgeConverterModule
   {
       public class BleData
       {
           [JsonProperty(PropertyName = "temperature")]
           public string Temperature { get; set; }
       }
   }
   ```

12. Hello fájl mentése másként `BleData.cs` billentyűkombináció lenyomásával `Ctrl`  +  `Shift`  +  `S` kulcsok.
    - Hello a Mentés másként párbeszédpanel hello `Save as Type` legördülő menüben válassza `C# (*.cs;*.csx)` hello kép a következő látható:

    ![A Visual Studio Code Mentés másként párbeszédpanel](media/iot-hub-iot-edge-create-module/vscode-save-as.png)

13. Hozzon létre egy új fájlt nevű `Untitled-1` által lenyomásával hello `Ctrl`  +  `N` kulcsok.

14. Másolja és illessze be a következő kódrészletet a hello hello `Untitled-1` fájlt. Ez az osztály egy `Azure IoT Edge` modult, amely toooutput hello adatokat az használjuk a `BleConverterModule`.

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Linq;
   using System.Text;
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Gateway;
   using Newtonsoft.Json;
   
   namespace PrinterModule
   {
       public class DotNetPrinterModule : IGatewayModule
       {
           private string configuration;
           public void Create(Broker broker, byte[] configuration)
           {
               this.configuration = System.Text.Encoding.UTF8.GetString(configuration);
           }
   
           public void Destroy()
           {
           }
   
           public void Receive(Message received_message)
           {
               string recMsg = System.Text.Encoding.UTF8.GetString(received_message.Content, 0, received_message.Content.Length);
               Dictionary<string, string> receivedProperties = received_message.Properties;
               
               BleConverterData receivedData = JsonConvert.DeserializeObject<BleConverterData>(recMsg);
   
               Console.WriteLine();
               Console.WriteLine("Module           : [DotNetPrinterModule]");
               Console.WriteLine("received_message : {0}", recMsg);
   
               if(received_message.Properties["source"] == "bleTelemetry")
               {
                   Console.WriteLine("Source           : {0}", receivedProperties["source"]);
                   Console.WriteLine("MAC address      : {0}", receivedProperties["macAddress"]);
                   Console.WriteLine("Temperature Alert: {0}", receivedProperties["temperatureAlert"]);
                   Console.WriteLine("Deserialized Obj : [BleConverterData]");
                   Console.WriteLine("  DeviceId       : {0}", receivedData.DeviceId);
                   Console.WriteLine("  MessageId      : {0}", receivedData.MessageId);
                   Console.WriteLine("  Temperature    : {0}", receivedData.Temperature);
               }
   
               Console.WriteLine();
           }
       }
   }
   ```

15. Hello fájl mentése másként `DotNetPrinterModule.cs` billentyűkombináció lenyomásával `Ctrl`  +  `Shift`  +  `S`.
    - Hello a Mentés másként párbeszédpanel hello `Save as Type` legördülő menüben válassza `C# (*.cs;*.csx)`.

16. Hozzon létre egy új fájlt nevű `Untitled-1` által lenyomásával hello `Ctrl`  +  `N` kulcsok.

17. toodeserialize hello `JSON` hello szervezettől kapott objektum `BleConverterModule`, másolás és beillesztés hello következő kódrészletet a hello `Untitled-1` fájlt. 

   ```csharp
   using System;
   using Newtonsoft.Json;

   namespace PrinterModule
   {
       public class BleConverterData
       {
           [JsonProperty(PropertyName = "deviceId")]
           public string DeviceId { get; set; }
   
           [JsonProperty(PropertyName = "messageId")]
           public string MessageId { get; set; }
   
           [JsonProperty(PropertyName = "temperature")]
           public string Temperature { get; set; }
       }
   }
   ```

18. Hello fájl mentése másként `BleConverterData.cs` billentyűkombináció lenyomásával `Ctrl`  +  `Shift`  +  `S`.
    - Hello a Mentés másként párbeszédpanel hello `Save as Type` legördülő menüben válassza `C# (*.cs;*.csx)`.

19. Hozzon létre egy új fájlt nevű `Untitled-1` által lenyomásával hello `Ctrl`  +  `N` kulcsok.

20. Másolja és illessze be a következő kódrészletet a hello hello `Untitled-1` fájlt.

   ```json
   {
       "loaders": [
           {
               "type": "dotnetcore",
               "name": "dotnetcore",
               "configuration": {
                   "binding.path": "dotnetcore.dll",
                   "binding.coreclrpath": "C:\\Program Files\\dotnet\\shared\\Microsoft.NETCore.App\\1.1.1\\coreclr.dll",
                   "binding.trustedplatformassemblieslocation": "C:\\Program Files\\dotnet\\shared\\Microsoft.NETCore.App\\1.1.1\\"
               }
           }
       ],
          "modules": [
           {
               "name": "simulated_device",
               "loader": {
                   "name": "native",
                   "entrypoint": {
                       "module.path": "simulated_device.dll"
                   }
               },
               "args": {
                   "macAddress": "01:02:03:03:02:01",
                   "messagePeriod": 500
               }
           },
           {
               "name": "ble_converter_module",
               "loader": {
                   "name": "dotnetcore",
                   "entrypoint": {
                       "assembly.name": "IoTEdgeConverterModule",
                       "entry.type": "IoTEdgeConverterModule.BleConverterModule"
                   }
               },
               "args": ""
           },
           {
               "name": "printer_module",
               "loader": {
                   "name": "dotnetcore",
                   "entrypoint": {
                       "assembly.name": "IoTEdgeConverterModule",
                       "entry.type": "PrinterModule.DotNetPrinterModule"
                   }
               },
               "args": ""
           }
       ],
       "links": [
           {
               "source": "simulated_device", "sink": "ble_converter_module"
           },
           {
               "source": "ble_converter_module", "sink": "printer_module"
           }
   ]
   }
   ```

21. Hello fájl mentése másként `gw-config.json` billentyűkombináció lenyomásával `Ctrl`  +  `Shift`  +  `S`.
    - Hello a Mentés másként párbeszédpanel hello `Save as Type` legördülő menüben válassza `JSON (*.json;*.bowerrc;*.jshintrc;*.jscsrc;*.eslintrc;*.babelrc;*webmanifest)`.

22. hello konfigurációs fájl toohello másolását tooenable kimeneti könyvtár, frissítés hello `IoTEdgeConverterModule.csproj` a következő XML-blobot hello:

   ```xml
     <ItemGroup>
       <None Update="gw-config.json" CopyToOutputDirectory="PreserveNewest" />
     </ItemGroup>
   ```
    
   - frissített hello `IoTEdgeConverterModule.csproj` kell hello kinézetét következő lemezképet:

    ![A Visual Studio Code .csproj fájl frissítése](media/iot-hub-iot-edge-create-module/vscode-update-csproj.png)

23. Hozzon létre egy új fájlt nevű `Untitled-1` által lenyomásával hello `Ctrl`  +  `N` kulcsok.

24. Másolja és illessze be a következő kódrészletet a hello hello `Untitled-1` fájlt.

   ```powershell
   Copy-Item -Path $env:userprofile\.nuget\packages\microsoft.azure.devices.gateway.native.windows.x64\1.1.3\runtimes\win-x64\native\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.runtime.serialization.formatters\4.3.0\lib\netstandard1.4\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.runtime.serialization.primitives\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\newtonsoft.json\10.0.2\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.componentmodel.typeconverter\4.3.0\lib\netstandard1.5\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.collections.nongeneric\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.collections.specialized\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   ```

25. Hello fájl mentése másként `binplace.ps1` billentyűkombináció lenyomásával `Ctrl`  +  `Shift`  +  `S`.
    - Hello a Mentés másként párbeszédpanel hello `Save as Type` legördülő menüben válassza `PowerShell (*.ps1;*.psm1;*.psd1;*.pssc;*.psrc)`.

26. Hello projekt felépítéséhez által lenyomásával hello `Ctrl`  +  `Shift`  +  `B` kulcsok. Első alkalommal hello-projektjét, amely hello építésekor `Visual Studio Code` jelenít meg hello `No build task defined.` párbeszédpanelen a következő kép hello látható módon:

    ![A Visual Studio Code build a feladat párbeszédpanelen](media/iot-hub-iot-edge-create-module/vscode-build-task.png)

    a) kattintson hello `Configure Build Task` gombra.

    b) a hello `Select a Task Runner` párbeszédpanel legördülő menüre. Válassza ki `.NET Core` hello kép a következő látható: 

    ![A Visual Studio Code, válassza ki a feladat párbeszédpanelen](media/iot-hub-iot-edge-create-module/vscode-build-task-runner.png)

    c) kattintva hello `.NET Core` elemet hoz létre hello `tasks.json` fájlt a `.vscode` directory és a megnyíló hello hello fájlban `code editor` ablak. Nincs nincs szükség toomodify ezt a fájlt, a Bezárás hello fülre.

27.  Nyitott hello `Visual Studio Code` által lenyomásával hello integrált terminálablakot `Ctrl`  +  `backtick` a kulcsok vagy hello menük `View`  ->  `Integrated Terminal` és típusa **.\binplace.ps1**be hello `PowerShell` parancssort. Ez a parancs másolja át az összes a függőségek toohello kimeneti könyvtár.

28. Keresse meg a toohello projektek kimeneti könyvtár a hello `Integrated Terminal` ablakban írja be a **cd.\bin\Debug\netstandard1.3**.

29. Hello mintaprojektet futtatásához írja be **. \gw.exe gw-config.json** történő hello `Integrated Terminal` ablak kérdés. 
    - Ha követte hello lépéseit az oktatóanyag szorosan, most futhat hello `Azure IoT Edge BLE Data Converter Module` mintaprojektet hello kép a következő látható:
    
        ![Visual Studio Code-beli szimulált eszköz – példa](media/iot-hub-iot-edge-create-module/vscode-run.png)
    
    - Ha azt szeretné, hogy tooterminate hello alkalmazás, nyomja le az hello `<Enter>` kulcs.

>[!IMPORTANT]
Nem ajánlott toouse `Ctrl`  +  `C` tooterminate hello `IoT Edge` átjáró alkalmazás (Ez azt jelenti, hogy **gw.exe**). Ez a művelet lehet, hogy hello folyamat tooterminate rendellenesen oka.

