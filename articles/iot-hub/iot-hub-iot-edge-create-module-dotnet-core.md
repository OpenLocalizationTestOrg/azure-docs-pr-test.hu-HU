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
# <a name="create-an-azure-iot-edge-module-with-cx23"></a><span data-ttu-id="25ea9-104">Egy Azure IoT Edge-modul létrehozása a C & #x23;</span><span class="sxs-lookup"><span data-stu-id="25ea9-104">Create an Azure IoT Edge Module with C&#x23;</span></span>

<span data-ttu-id="25ea9-105">Ez az oktatóanyag bővíthető hogyan toocreate a modul a `Azure IoT Edge` használatával `Visual Studio Code` és `C#`.</span><span class="sxs-lookup"><span data-stu-id="25ea9-105">This tutorial showcases how toocreate a module for `Azure IoT Edge` using `Visual Studio Code` and `C#`.</span></span>

<span data-ttu-id="25ea9-106">Az oktatóanyag azt bízná környezet beállításról, és hogyan toowrite egy [Gedélyezése](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) legújabb konverter modul használatával végzett hello `Azure IoT Edge NuGet` csomagok.</span><span class="sxs-lookup"><span data-stu-id="25ea9-106">In this tutorial, we walk through environment set-up and how toowrite a [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) data converter module using hello latest `Azure IoT Edge NuGet` packages.</span></span> 

>[!NOTE]
<span data-ttu-id="25ea9-107">Ez az oktatóanyag használ hello `.NET Core SDK`, amely támogatja a platformfüggetlen.</span><span class="sxs-lookup"><span data-stu-id="25ea9-107">This tutorial is using hello `.NET Core SDK`, which supports cross-platform compatibility.</span></span> <span data-ttu-id="25ea9-108">hello következő oktatóanyag íródott hello `Windows 10` operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="25ea9-108">hello following tutorial is written using hello `Windows 10` operating system.</span></span> <span data-ttu-id="25ea9-109">Ez az oktatóanyag hello parancsainak függően eltérő lehet a `development environment`.</span><span class="sxs-lookup"><span data-stu-id="25ea9-109">Some of hello commands in this tutorial may be different depending on your `development environment`.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="25ea9-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="25ea9-110">Prerequisites</span></span>

<span data-ttu-id="25ea9-111">Ez a szakasz azt beállításról a környezetet az `Azure IoT Edge` modul fejlesztési.</span><span class="sxs-lookup"><span data-stu-id="25ea9-111">In this section, we set-up your environment for `Azure IoT Edge` module development.</span></span> <span data-ttu-id="25ea9-112">Ez a kategória tooboth **64 bites Windows** és **64 bites Linux (8 Ubuntu/Debian)** operációs rendszerek.</span><span class="sxs-lookup"><span data-stu-id="25ea9-112">It applies tooboth **64-bit Windows** and **64-bit Linux (Ubuntu/Debian 8)** operating systems.</span></span>

<span data-ttu-id="25ea9-113">a következő szoftver hello szükség:</span><span class="sxs-lookup"><span data-stu-id="25ea9-113">hello following software is required:</span></span>

- [<span data-ttu-id="25ea9-114">Git-ügyfél</span><span class="sxs-lookup"><span data-stu-id="25ea9-114">Git Client</span></span>](https://git-scm.com/downloads)
- [<span data-ttu-id="25ea9-115">.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="25ea9-115">.NET Core SDK</span></span>](https://www.microsoft.com/net/core#windowscmd)
- [<span data-ttu-id="25ea9-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="25ea9-116">Visual Studio Code</span></span>](https://code.visualstudio.com/)

<span data-ttu-id="25ea9-117">Ez a minta a tooclone hello tárház nem kell, azonban az összes hello példakód tárgyalt ebben az oktatóanyagban található a következő tárházat hello:</span><span class="sxs-lookup"><span data-stu-id="25ea9-117">You do not need tooclone hello repo for this sample, however all of hello sample code discussed in this tutorial is located in hello following repository:</span></span>

- <span data-ttu-id="25ea9-118">`git clone https://github.com/Azure-Samples/iot-edge-samples.git`.</span><span class="sxs-lookup"><span data-stu-id="25ea9-118">`git clone https://github.com/Azure-Samples/iot-edge-samples.git`.</span></span>
- `cd iot-edge-samples/dotnetcore/simulated_ble`

## <a name="getting-started"></a><span data-ttu-id="25ea9-119">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="25ea9-119">Getting started</span></span>

1. <span data-ttu-id="25ea9-120">Telepítés `.NET Core SDK`.</span><span class="sxs-lookup"><span data-stu-id="25ea9-120">Install `.NET Core SDK`.</span></span>
2. <span data-ttu-id="25ea9-121">Telepítés `Visual Studio Code` és hello `C# extension` a Visual Studio Code piactér hello.</span><span class="sxs-lookup"><span data-stu-id="25ea9-121">Install `Visual Studio Code` and hello `C# extension` from hello Visual Studio Code Marketplace.</span></span>

<span data-ttu-id="25ea9-122">Ezek megtekintéséhez [gyors videó oktatóanyag](https://channel9.msdn.com/Blogs/dotnet/Get-started-VSCode-Csharp-NET-Core-Windows) kapcsolatos hogyan tooget használatának `Visual Studio Code` és hello `.NET Core SDK`.</span><span class="sxs-lookup"><span data-stu-id="25ea9-122">View this [quick video tutorial](https://channel9.msdn.com/Blogs/dotnet/Get-started-VSCode-Csharp-NET-Core-Windows) about how tooget started using `Visual Studio Code` and hello `.NET Core SDK`.</span></span>

## <a name="creating-hello-azure-iot-edge-converter-module"></a><span data-ttu-id="25ea9-123">Hello Azure IoT peremhálózati konverter modul létrehozása</span><span class="sxs-lookup"><span data-stu-id="25ea9-123">Creating hello Azure IoT Edge converter module</span></span>

1. <span data-ttu-id="25ea9-124">Egy új inicializálása `.NET Core` C# hordozhatóosztálytár-projektjének:</span><span class="sxs-lookup"><span data-stu-id="25ea9-124">Initialize a new `.NET Core` class library C# project:</span></span>
    - <span data-ttu-id="25ea9-125">Nyisson meg egy parancssort (`Windows + R` -> `cmd` -> `enter`).</span><span class="sxs-lookup"><span data-stu-id="25ea9-125">Open a command prompt (`Windows + R` -> `cmd` -> `enter`).</span></span>
    - <span data-ttu-id="25ea9-126">Keresse meg a toocreate hello helyének toohello mappa `C#` projekt.</span><span class="sxs-lookup"><span data-stu-id="25ea9-126">Navigate toohello folder where you'd like toocreate hello `C#` project.</span></span>
    - <span data-ttu-id="25ea9-127">Típus **dotnet új classlib -o IoTEdgeConverterModule -f netstandard1.3**.</span><span class="sxs-lookup"><span data-stu-id="25ea9-127">Type **dotnet new classlib -o IoTEdgeConverterModule -f netstandard1.3**.</span></span> 
    - <span data-ttu-id="25ea9-128">Ez a parancs létrehoz egy üres osztályt `Class1.cs` a projekteket tartalmazó könyvtárban található.</span><span class="sxs-lookup"><span data-stu-id="25ea9-128">This command creates an empty class called `Class1.cs` in your projects directory.</span></span>
2. <span data-ttu-id="25ea9-129">Keresse meg a toohello mappát, ahol imént létrehozott hello hordozhatóosztálytár-projektjének beírásával **cd IoTEdgeConverterModule**.</span><span class="sxs-lookup"><span data-stu-id="25ea9-129">Navigate toohello folder where we just created hello class library project by typing **cd IoTEdgeConverterModule**.</span></span>
3. <span data-ttu-id="25ea9-130">Nyissa meg hello projektre a `Visual Studio Code` beírásával **kód.**.</span><span class="sxs-lookup"><span data-stu-id="25ea9-130">Open hello project in `Visual Studio Code` by typing **code .**.</span></span>
4. <span data-ttu-id="25ea9-131">Miután hello projekt meg van nyitva, a `Visual Studio Code`, kattintson a hello **IoTEdgeConverterModule.csproj** tooopen hello fájl látható hello kép a következő módon:</span><span class="sxs-lookup"><span data-stu-id="25ea9-131">Once hello project is opened in `Visual Studio Code`, click on hello **IoTEdgeConverterModule.csproj** tooopen hello file as shown in hello following image:</span></span>

    ![A Visual Studio Code Szerkesztés ablak](media/iot-hub-iot-edge-create-module/vscode-edit-csproj.png)

5. <span data-ttu-id="25ea9-133">Hello beszúrása `XML` megjelennek a következő kódrészletet közötti hello záró hello blob `PropertyGroup` címkét, és a záró hello `Project` címkét; hat kép megelőző hello. sor, és hello fájl mentése billentyűkombináció lenyomásával `Ctrl`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="25ea9-133">Insert hello `XML` blob shown in hello following code snippet between hello closing `PropertyGroup` tag and hello closing `Project` tag; line six in hello preceding image and save hello file by pressing `Ctrl` + `S`.</span></span>

   ```xml
     <ItemGroup>
       <PackageReference Include="Microsoft.Azure.Devices.Gateway.Module.NetStandard" Version="1.0.5" />
       <PackageReference Include="System.Threading.Thread" Version="4.3.0" />
       <PackageReference Include="Newtonsoft.Json" Version="10.0.2" />
     </ItemGroup> 
   ```

6. <span data-ttu-id="25ea9-134">Miután menti hello `.csproj` fájl, `Visual Studio Code` kell kérni a egy `unresolved dependencies` párbeszédpanelen a következő kép hello látható módon:</span><span class="sxs-lookup"><span data-stu-id="25ea9-134">Once you save hello `.csproj` file, `Visual Studio Code` should prompt you with an `unresolved dependencies` dialog as seen in hello following image:</span></span> 

    ![A Visual Studio Code visszaállítási függőségek párbeszédpanel](media/iot-hub-iot-edge-create-module/vscode-restore.png)

    <span data-ttu-id="25ea9-136">a) kattintson `Restore` összes hello sorszámra hello projektek toorestore `.csproj` hello például `PackageReferences` jelentek meg.</span><span class="sxs-lookup"><span data-stu-id="25ea9-136">a) Click `Restore` toorestore all of hello references in hello projects `.csproj` file including hello `PackageReferences` we have added.</span></span> 

    <span data-ttu-id="25ea9-137">b) `Visual Studio Code` automatikusan létrehozza a hello `project.assets.json` fájlt a projektben `obj` mappát.</span><span class="sxs-lookup"><span data-stu-id="25ea9-137">b) `Visual Studio Code` automatically creates hello `project.assets.json` file in your projects `obj` folder.</span></span> <span data-ttu-id="25ea9-138">Ezt a fájlt a projekt függőségek toomake későbbi visszaállítások gyorsabb információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="25ea9-138">This file contains information about your project's dependencies toomake subsequent restores quicker.</span></span>
 
    >[!NOTE]
    <span data-ttu-id="25ea9-139">`.NET Core Tools`jelenleg MSBuild-alapú.</span><span class="sxs-lookup"><span data-stu-id="25ea9-139">`.NET Core Tools` are now MSBuild-based.</span></span> <span data-ttu-id="25ea9-140">Ez azt jelenti a `.csproj` projekt fájlt hoz létre ahelyett, hogy egy `project.json`.</span><span class="sxs-lookup"><span data-stu-id="25ea9-140">Which means a `.csproj` project file is created instead of a `project.json`.</span></span>

    - <span data-ttu-id="25ea9-141">Ha `Visual Studio Code` nem kéri a felhasználót, hogy így megfelelő, azt megteheti manuálisan.</span><span class="sxs-lookup"><span data-stu-id="25ea9-141">If `Visual Studio Code` does not prompt you that is ok, we can do it manually.</span></span> <span data-ttu-id="25ea9-142">Nyissa meg hello `Visual Studio Code` által lenyomásával hello integrált terminálablakot `Ctrl`  +  `backtick` a kulcsok vagy hello menük `View`  ->  `Integrated Terminal`.</span><span class="sxs-lookup"><span data-stu-id="25ea9-142">Open hello `Visual Studio Code` integrated terminal window by pressing hello `Ctrl` + `backtick` keys or using hello menus `View` -> `Integrated Terminal`.</span></span>
    - <span data-ttu-id="25ea9-143">A hello `Integrated Terminal` időszak típusa **dotnet visszaállítási**.</span><span class="sxs-lookup"><span data-stu-id="25ea9-143">In hello `Integrated Terminal` window type **dotnet restore**.</span></span>
    
7. <span data-ttu-id="25ea9-144">Nevezze át a hello `Class1.cs` fájl túl`BleConverterModule.cs`.</span><span class="sxs-lookup"><span data-stu-id="25ea9-144">Rename hello `Class1.cs` file too`BleConverterModule.cs`.</span></span> 

    <span data-ttu-id="25ea9-145">a) toorename hello fájl először kattintson hello fájlra, majd nyomja le az hello `F2` kulcs.</span><span class="sxs-lookup"><span data-stu-id="25ea9-145">a) toorename hello file first click on hello file then press hello `F2` key.</span></span>
    
    <span data-ttu-id="25ea9-146">b) az új név hello típus **BleConverterModule**, a következő kép hello látható módon:</span><span class="sxs-lookup"><span data-stu-id="25ea9-146">b) Type in hello new name **BleConverterModule**, as seen in hello following image:</span></span>

    ![A Visual Studio Code osztály átnevezése](media/iot-hub-iot-edge-create-module/vscode-rename.png)

8. <span data-ttu-id="25ea9-148">Cserélje le a meglévő kód hello hello `BleConverterModule.cs` másolás és beillesztés hello a következő kódrészletet a fájlt a `BleConverterModule.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="25ea9-148">Replace hello existing code in hello `BleConverterModule.cs` file by copying and pasting hello following code snippet into your `BleConverterModule.cs` file.</span></span>

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

9. <span data-ttu-id="25ea9-149">Hello fájl mentése billentyűkombináció lenyomásával `Ctrl`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="25ea9-149">Save hello file by pressing `Ctrl` + `S`.</span></span>

10. <span data-ttu-id="25ea9-150">Hozzon létre egy új fájlt nevű `Untitled-1` által lenyomásával hello `Ctrl`  +  `N` kulcsok hello kép a következő látható:</span><span class="sxs-lookup"><span data-stu-id="25ea9-150">Create a new file called `Untitled-1` by pressing hello `Ctrl` + `N` keys as seen in hello following image:</span></span>

    ![A Visual Studio Code új fájl](media/iot-hub-iot-edge-create-module/vscode-new-file.png)

11. <span data-ttu-id="25ea9-152">toodeserialize hello `JSON` szimulált hello szervezettől kapott objektum `BLE` eszköz, a kód hello be a következő másolási hello `Untitled-1` fájl kód szerkesztő ablakban.</span><span class="sxs-lookup"><span data-stu-id="25ea9-152">toodeserialize hello `JSON` object that we receive from hello simulated `BLE` device, copy hello following code into hello `Untitled-1` file code editor window.</span></span> 

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

12. <span data-ttu-id="25ea9-153">Hello fájl mentése másként `BleData.cs` billentyűkombináció lenyomásával `Ctrl`  +  `Shift`  +  `S` kulcsok.</span><span class="sxs-lookup"><span data-stu-id="25ea9-153">Save hello file as `BleData.cs` by pressing `Ctrl` + `Shift` + `S` keys.</span></span>
    - <span data-ttu-id="25ea9-154">Hello a Mentés másként párbeszédpanel hello `Save as Type` legördülő menüben válassza `C# (*.cs;*.csx)` hello kép a következő látható:</span><span class="sxs-lookup"><span data-stu-id="25ea9-154">On hello save as dialog box, in hello `Save as Type` dropdown menu, select `C# (*.cs;*.csx)` as seen in hello following image:</span></span>

    ![A Visual Studio Code Mentés másként párbeszédpanel](media/iot-hub-iot-edge-create-module/vscode-save-as.png)

13. <span data-ttu-id="25ea9-156">Hozzon létre egy új fájlt nevű `Untitled-1` által lenyomásával hello `Ctrl`  +  `N` kulcsok.</span><span class="sxs-lookup"><span data-stu-id="25ea9-156">Create a new file called `Untitled-1` by pressing hello `Ctrl` + `N` keys.</span></span>

14. <span data-ttu-id="25ea9-157">Másolja és illessze be a következő kódrészletet a hello hello `Untitled-1` fájlt.</span><span class="sxs-lookup"><span data-stu-id="25ea9-157">Copy and paste hello following code snippet into hello `Untitled-1` file.</span></span> <span data-ttu-id="25ea9-158">Ez az osztály egy `Azure IoT Edge` modult, amely toooutput hello adatokat az használjuk a `BleConverterModule`.</span><span class="sxs-lookup"><span data-stu-id="25ea9-158">This class is a `Azure IoT Edge` module, which we use toooutput hello data received from our `BleConverterModule`.</span></span>

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

15. <span data-ttu-id="25ea9-159">Hello fájl mentése másként `DotNetPrinterModule.cs` billentyűkombináció lenyomásával `Ctrl`  +  `Shift`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="25ea9-159">Save hello file as `DotNetPrinterModule.cs` by pressing `Ctrl` + `Shift` + `S`.</span></span>
    - <span data-ttu-id="25ea9-160">Hello a Mentés másként párbeszédpanel hello `Save as Type` legördülő menüben válassza `C# (*.cs;*.csx)`.</span><span class="sxs-lookup"><span data-stu-id="25ea9-160">On hello save as dialog box, in hello `Save as Type` dropdown menu, select `C# (*.cs;*.csx)`.</span></span>

16. <span data-ttu-id="25ea9-161">Hozzon létre egy új fájlt nevű `Untitled-1` által lenyomásával hello `Ctrl`  +  `N` kulcsok.</span><span class="sxs-lookup"><span data-stu-id="25ea9-161">Create a new file called `Untitled-1` by pressing hello `Ctrl` + `N` keys.</span></span>

17. <span data-ttu-id="25ea9-162">toodeserialize hello `JSON` hello szervezettől kapott objektum `BleConverterModule`, másolás és beillesztés hello következő kódrészletet a hello `Untitled-1` fájlt.</span><span class="sxs-lookup"><span data-stu-id="25ea9-162">toodeserialize hello `JSON` object that we receive from hello `BleConverterModule`, Copy and paste hello following code snippet into hello `Untitled-1` file.</span></span> 

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

18. <span data-ttu-id="25ea9-163">Hello fájl mentése másként `BleConverterData.cs` billentyűkombináció lenyomásával `Ctrl`  +  `Shift`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="25ea9-163">Save hello file as `BleConverterData.cs` by pressing `Ctrl` + `Shift` + `S`.</span></span>
    - <span data-ttu-id="25ea9-164">Hello a Mentés másként párbeszédpanel hello `Save as Type` legördülő menüben válassza `C# (*.cs;*.csx)`.</span><span class="sxs-lookup"><span data-stu-id="25ea9-164">On hello save as dialog box, in hello `Save as Type` dropdown menu, select `C# (*.cs;*.csx)`.</span></span>

19. <span data-ttu-id="25ea9-165">Hozzon létre egy új fájlt nevű `Untitled-1` által lenyomásával hello `Ctrl`  +  `N` kulcsok.</span><span class="sxs-lookup"><span data-stu-id="25ea9-165">Create a new file called `Untitled-1` by pressing hello `Ctrl` + `N` keys.</span></span>

20. <span data-ttu-id="25ea9-166">Másolja és illessze be a következő kódrészletet a hello hello `Untitled-1` fájlt.</span><span class="sxs-lookup"><span data-stu-id="25ea9-166">Copy and paste hello following code snippet into hello `Untitled-1` file.</span></span>

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

21. <span data-ttu-id="25ea9-167">Hello fájl mentése másként `gw-config.json` billentyűkombináció lenyomásával `Ctrl`  +  `Shift`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="25ea9-167">Save hello file as `gw-config.json` by pressing `Ctrl` + `Shift` + `S`.</span></span>
    - <span data-ttu-id="25ea9-168">Hello a Mentés másként párbeszédpanel hello `Save as Type` legördülő menüben válassza `JSON (*.json;*.bowerrc;*.jshintrc;*.jscsrc;*.eslintrc;*.babelrc;*webmanifest)`.</span><span class="sxs-lookup"><span data-stu-id="25ea9-168">On hello save as dialog box, in hello `Save as Type` dropdown menu, select `JSON (*.json;*.bowerrc;*.jshintrc;*.jscsrc;*.eslintrc;*.babelrc;*webmanifest)`.</span></span>

22. <span data-ttu-id="25ea9-169">hello konfigurációs fájl toohello másolását tooenable kimeneti könyvtár, frissítés hello `IoTEdgeConverterModule.csproj` a következő XML-blobot hello:</span><span class="sxs-lookup"><span data-stu-id="25ea9-169">tooenable copying of hello configuration file toohello output directory, update hello `IoTEdgeConverterModule.csproj` with hello following XML blob:</span></span>

   ```xml
     <ItemGroup>
       <None Update="gw-config.json" CopyToOutputDirectory="PreserveNewest" />
     </ItemGroup>
   ```
    
   - <span data-ttu-id="25ea9-170">frissített hello `IoTEdgeConverterModule.csproj` kell hello kinézetét következő lemezképet:</span><span class="sxs-lookup"><span data-stu-id="25ea9-170">hello updated `IoTEdgeConverterModule.csproj` should look like hello following image:</span></span>

    ![A Visual Studio Code .csproj fájl frissítése](media/iot-hub-iot-edge-create-module/vscode-update-csproj.png)

23. <span data-ttu-id="25ea9-172">Hozzon létre egy új fájlt nevű `Untitled-1` által lenyomásával hello `Ctrl`  +  `N` kulcsok.</span><span class="sxs-lookup"><span data-stu-id="25ea9-172">Create a new file called `Untitled-1` by pressing hello `Ctrl` + `N` keys.</span></span>

24. <span data-ttu-id="25ea9-173">Másolja és illessze be a következő kódrészletet a hello hello `Untitled-1` fájlt.</span><span class="sxs-lookup"><span data-stu-id="25ea9-173">Copy and paste hello following code snippet into hello `Untitled-1` file.</span></span>

   ```powershell
   Copy-Item -Path $env:userprofile\.nuget\packages\microsoft.azure.devices.gateway.native.windows.x64\1.1.3\runtimes\win-x64\native\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.runtime.serialization.formatters\4.3.0\lib\netstandard1.4\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.runtime.serialization.primitives\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\newtonsoft.json\10.0.2\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.componentmodel.typeconverter\4.3.0\lib\netstandard1.5\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.collections.nongeneric\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.collections.specialized\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   ```

25. <span data-ttu-id="25ea9-174">Hello fájl mentése másként `binplace.ps1` billentyűkombináció lenyomásával `Ctrl`  +  `Shift`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="25ea9-174">Save hello file as `binplace.ps1` by pressing `Ctrl` + `Shift` + `S`.</span></span>
    - <span data-ttu-id="25ea9-175">Hello a Mentés másként párbeszédpanel hello `Save as Type` legördülő menüben válassza `PowerShell (*.ps1;*.psm1;*.psd1;*.pssc;*.psrc)`.</span><span class="sxs-lookup"><span data-stu-id="25ea9-175">On hello save as dialog box, in hello `Save as Type` dropdown menu, select `PowerShell (*.ps1;*.psm1;*.psd1;*.pssc;*.psrc)`.</span></span>

26. <span data-ttu-id="25ea9-176">Hello projekt felépítéséhez által lenyomásával hello `Ctrl`  +  `Shift`  +  `B` kulcsok.</span><span class="sxs-lookup"><span data-stu-id="25ea9-176">Build hello project by pressing hello `Ctrl` + `Shift` + `B` keys.</span></span> <span data-ttu-id="25ea9-177">Első alkalommal hello-projektjét, amely hello építésekor `Visual Studio Code` jelenít meg hello `No build task defined.` párbeszédpanelen a következő kép hello látható módon:</span><span class="sxs-lookup"><span data-stu-id="25ea9-177">When you build hello project for hello first time, `Visual Studio Code` prompts you with hello `No build task defined.` dialog as seen in hello following image:</span></span>

    ![A Visual Studio Code build a feladat párbeszédpanelen](media/iot-hub-iot-edge-create-module/vscode-build-task.png)

    <span data-ttu-id="25ea9-179">a) kattintson hello `Configure Build Task` gombra.</span><span class="sxs-lookup"><span data-stu-id="25ea9-179">a) Click hello `Configure Build Task` button.</span></span>

    <span data-ttu-id="25ea9-180">b) a hello `Select a Task Runner` párbeszédpanel legördülő menüre.</span><span class="sxs-lookup"><span data-stu-id="25ea9-180">b) In hello `Select a Task Runner` dialog dropdown menu.</span></span> <span data-ttu-id="25ea9-181">Válassza ki `.NET Core` hello kép a következő látható:</span><span class="sxs-lookup"><span data-stu-id="25ea9-181">Select `.NET Core` as seen in hello following image:</span></span> 

    ![A Visual Studio Code, válassza ki a feladat párbeszédpanelen](media/iot-hub-iot-edge-create-module/vscode-build-task-runner.png)

    <span data-ttu-id="25ea9-183">c) kattintva hello `.NET Core` elemet hoz létre hello `tasks.json` fájlt a `.vscode` directory és a megnyíló hello hello fájlban `code editor` ablak.</span><span class="sxs-lookup"><span data-stu-id="25ea9-183">c) Clicking hello `.NET Core` item creates hello `tasks.json` file in your `.vscode` directory and opens hello file in hello `code editor` window.</span></span> <span data-ttu-id="25ea9-184">Nincs nincs szükség toomodify ezt a fájlt, a Bezárás hello fülre.</span><span class="sxs-lookup"><span data-stu-id="25ea9-184">There is no need toomodify this file, close hello tab.</span></span>

27.  <span data-ttu-id="25ea9-185">Nyitott hello `Visual Studio Code` által lenyomásával hello integrált terminálablakot `Ctrl`  +  `backtick` a kulcsok vagy hello menük `View`  ->  `Integrated Terminal` és típusa **.\binplace.ps1**be hello `PowerShell` parancssort.</span><span class="sxs-lookup"><span data-stu-id="25ea9-185">Open hello `Visual Studio Code` integrated terminal window by pressing hello `Ctrl` + `backtick` keys or using hello menus `View` -> `Integrated Terminal` and type **.\binplace.ps1** into hello `PowerShell` command prompt.</span></span> <span data-ttu-id="25ea9-186">Ez a parancs másolja át az összes a függőségek toohello kimeneti könyvtár.</span><span class="sxs-lookup"><span data-stu-id="25ea9-186">This command copies all our dependencies toohello output directory.</span></span>

28. <span data-ttu-id="25ea9-187">Keresse meg a toohello projektek kimeneti könyvtár a hello `Integrated Terminal` ablakban írja be a **cd.\bin\Debug\netstandard1.3**.</span><span class="sxs-lookup"><span data-stu-id="25ea9-187">Navigate toohello projects output directory in hello `Integrated Terminal` window by typing **cd .\bin\Debug\netstandard1.3**.</span></span>

29. <span data-ttu-id="25ea9-188">Hello mintaprojektet futtatásához írja be **. \gw.exe gw-config.json** történő hello `Integrated Terminal` ablak kérdés.</span><span class="sxs-lookup"><span data-stu-id="25ea9-188">Run hello sample project by typing **.\gw.exe gw-config.json** into hello `Integrated Terminal` window prompt.</span></span> 
    - <span data-ttu-id="25ea9-189">Ha követte hello lépéseit az oktatóanyag szorosan, most futhat hello `Azure IoT Edge BLE Data Converter Module` mintaprojektet hello kép a következő látható:</span><span class="sxs-lookup"><span data-stu-id="25ea9-189">If you have followed hello steps in this tutorial closely, you should now be running hello `Azure IoT Edge BLE Data Converter Module` sample project as seen in hello following image:</span></span>
    
        ![Visual Studio Code-beli szimulált eszköz – példa](media/iot-hub-iot-edge-create-module/vscode-run.png)
    
    - <span data-ttu-id="25ea9-191">Ha azt szeretné, hogy tooterminate hello alkalmazás, nyomja le az hello `<Enter>` kulcs.</span><span class="sxs-lookup"><span data-stu-id="25ea9-191">If you want tooterminate hello application, press hello `<Enter>` key.</span></span>

>[!IMPORTANT]
<span data-ttu-id="25ea9-192">Nem ajánlott toouse `Ctrl`  +  `C` tooterminate hello `IoT Edge` átjáró alkalmazás (Ez azt jelenti, hogy **gw.exe**).</span><span class="sxs-lookup"><span data-stu-id="25ea9-192">It is not recommended toouse `Ctrl` + `C` tooterminate hello `IoT Edge` gateway application (that is, **gw.exe**).</span></span> <span data-ttu-id="25ea9-193">Ez a művelet lehet, hogy hello folyamat tooterminate rendellenesen oka.</span><span class="sxs-lookup"><span data-stu-id="25ea9-193">As this action may cause hello process tooterminate abnormally.</span></span>

