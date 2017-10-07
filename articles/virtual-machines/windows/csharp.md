---
title: "aaaCreate és kezelése az Azure virtuális gép használata a C# |} Microsoft Docs"
description: "C# és az Azure Resource Manager toodeploy használja a virtuális gép és annak támogató erőforrásokat."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 87524373-5f52-4f4b-94af-50bf7b65c277
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: davidmu
ms.openlocfilehash: 8beeabde731bbaa25e68d2b9c5abbf71acbe377f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-windows-vms-in-azure-using-c"></a>Létrehozása és kezelése Windows-alapú virtuális gépek az Azure-ban C# #

Egy [Azure virtuális gép](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) több támogató Azure-erőforrások kell. Ez a cikk ismerteti a létrehozása, kezelése és C# használatával Virtuálisgép-erőforrások törlése. Az alábbiak végrehajtásának módját ismerheti meg:

> [!div class="checklist"]
> * Visual Studio-projekt létrehozása
> * Hello csomag telepítése
> * Hitelesítő adatok létrehozása
> * Erőforrások létrehozása
> * Felügyeleti feladatok végrehajtása
> * Erőforrások törlése
> * Hello alkalmazás futtatása

Körülbelül 20 percet toodo szükséges lépéseket.

## <a name="create-a-visual-studio-project"></a>Visual Studio-projekt létrehozása

1. Ha még nem tette meg, telepítse [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio). Válassza ki **.NET asztali fejlesztési** hello munkaterhelések lap, és kattintson a **telepítése**. Az összefoglaló hello, láthatja, hogy **.NET-keretrendszer 4-4.6 Fejlesztőeszközök** automatikusan ki van jelölve meg. Ha már telepítette a Visual Studio, a Visual Studio indítója hello segítségével hello .NET munkaterhelés is hozzáadhat.
2. A Visual Studióban kattintson **fájl** > **új** > **projekt**.
3. A **sablonok** > **Visual C#**, jelölje be **Konzolalkalmazás (.NET-keretrendszer)**, adja meg *myDotnetProject* hello neveként hello projekt, hello projekt, jelölje be hello helyét, és kattintson a **OK**.

## <a name="install-hello-package"></a>Hello csomag telepítése

NuGet-csomagok olyan hello legegyszerűbb módja tooinstall hello könyvtárak kell toofinish ezeket a lépéseket. tooget hello szalagtárak, amelyekre szüksége van a Visual Studio, hajtsa végre ezeket a lépéseket:

1. Kattintson a **eszközök** > **Nuget-Csomagkezelő**, és kattintson a **Csomagkezelő konzol**.
2. Hello konzolján futtassa az alábbi parancsot:

    ```
    Install-Package Microsoft.Azure.Management.Fluent
    ```

## <a name="create-credentials"></a>Hitelesítő adatok létrehozása

Ez a lépés megkezdése előtt győződjön meg arról, hogy rendelkezik-e hozzáférési tooan [Active Directory szolgáltatás egyszerű](../../azure-resource-manager/resource-group-create-service-principal-portal.md). Is rögzíteni kell hello Alkalmazásazonosító, hello hitelesítési kulcs és hello Bérlőazonosító, amelyekre szüksége van egy későbbi lépésben.

### <a name="create-hello-authorization-file"></a>Hello engedélyezési fájl létrehozása

1. A Megoldáskezelőben kattintson a jobb gombbal *myDotnetProject* > **Hozzáadás** > **új elem**, majd válassza ki **szövegfájl** a *Visual C# elemek*. Nevű hello fájl *azureauth.properties*, és kattintson a **Hozzáadás**.
2. Adja hozzá a engedélyezési tulajdonságok:

    ```
    subscription=<subscription-id>
    client=<application-id>
    key=<authentication-key>
    tenant=<tenant-id>
    managementURI=https://management.core.windows.net/
    baseURL=https://management.azure.com/
    authURL=https://login.windows.net/
    graphURL=https://graph.windows.net/
    ```

    Cserélje le  **&lt;előfizetés-azonosító&gt;**  az előfizetés-azonosítóval rendelkező  **&lt;alkalmazásazonosító&gt;**  a hello Active Directory-alkalmazás azonosítóval  **&lt;hitelesítési kulcs&gt;**  hello alkalmazás kulccsal, és  **&lt;bérlőazonosító&gt;**  hello bérlővel azonosítója.

3. Hello azureauth.properties fájl mentéséhez. 
4. A Windows hello teljes elérési útja tooauthorization létrehozott fájl AZURE_AUTH_LOCATION nevű környezeti változó értéke. Például hello a következő PowerShell-parancs használható:

    ```
    [Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\Visual Studio 2017\Projects\myDotnetProject\myDotnetProject\azureauth.properties", "User")
    ```

### <a name="create-hello-management-client"></a>Hello felügyeleti ügyfél létrehozása

1. Nyissa meg a Program.cs fájl hello hello projekthez létrehozott, és adja hozzá ezek az utasítások toohello meglévő utasítások segítségével a hello fájl felső:

    ```
    using Microsoft.Azure.Management.Compute.Fluent;
    using Microsoft.Azure.Management.Compute.Fluent.Models;
    using Microsoft.Azure.Management.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
    ```

2. toocreate hello felügyeleti ügyfél, a kód toohello fő metódus hozzáadása:

    ```
    var credentials = SdkContext.AzureCredentialsFactory
        .FromFile(Environment.GetEnvironmentVariable("AZURE_AUTH_LOCATION"));

    var azure = Azure
        .Configure()
        .WithLogLevel(HttpLoggingDelegatingHandler.Level.Basic)
        .Authenticate(credentials)
        .WithDefaultSubscription();
    ```

## <a name="create-resources"></a>Erőforrások létrehozása

### <a name="create-hello-resource-group"></a>Hello erőforráscsoport létrehozása

Minden erőforrás tartalmaznia kell egy [erőforráscsoport](../../azure-resource-manager/resource-group-overview.md).

toospecify értékei alkalmazás hello és hello erőforráscsoport létrehozása, a kód toohello fő metódus hozzáadása:

```
var groupName = "myResourceGroup";
var vmName = "myVM";
var location = Region.USWest;
    
Console.WriteLine("Creating resource group...");
var resourceGroup = azure.ResourceGroups.Define(groupName)
    .WithRegion(location)
    .Create();
```

### <a name="create-hello-availability-set"></a>Hello rendelkezésre állási csoport létrehozása

[Rendelkezésre állási készletek](tutorial-availability-sets.md) könnyebben meg az alkalmazás által használt toomaintain hello virtuális gépeket.

toocreate hello rendelkezésre állási beállítása, a kód toohello fő metódus hozzáadása:

```
Console.WriteLine("Creating availability set...");
var availabilitySet = azure.AvailabilitySets.Define("myAVSet")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithSku(AvailabilitySetSkuTypes.Managed)
    .Create();
```

### <a name="create-hello-public-ip-address"></a>Hello nyilvános IP-cím létrehozása

A [nyilvános IP-cím](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) szükséges toocommunicate hello virtuális gép van.

toocreate hello nyilvános IP-cím hello virtuális géphez, a kód toohello fő metódus hozzáadása:
   
```
Console.WriteLine("Creating public IP address...");
var publicIPAddress = azure.PublicIPAddresses.Define("myPublicIP")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithDynamicIP()
    .Create();
```

### <a name="create-hello-virtual-network"></a>Hello virtuális hálózat létrehozása

Az alhálózat szerepelnie kell egy virtuális gépet egy [virtuális hálózati](../../virtual-network/virtual-networks-overview.md).

toocreate egy alhálózatot és egy virtuális hálózatot, adja hozzá a kódot toohello fő metódus:

```
Console.WriteLine("Creating virtual network...");
var network = azure.Networks.Define("myVNet")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithAddressSpace("10.0.0.0/16")
    .WithSubnet("mySubnet", "10.0.0.0/24")
    .Create();
```

### <a name="create-hello-network-interface"></a>Hello hálózati illesztő létrehozása

A virtuális gép egy hálózati illesztő toocommunicate hello virtuális hálózaton kell.

egy adott hálózati csatoló toocreate a kód toohello fő metódus hozzáadása:

```
Console.WriteLine("Creating network interface...");
var networkInterface = azure.NetworkInterfaces.Define("myNIC")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithExistingPrimaryNetwork(network)
    .WithSubnet("mySubnet")
    .WithPrimaryPrivateIPAddressDynamic()
    .WithExistingPrimaryPublicIPAddress(publicIPAddress)
    .Create();
 ```

### <a name="create-hello-virtual-machine"></a>Hello virtuális gép létrehozása

Most, hogy a létrehozott összes hello erőforrások támogatása, létrehozhat egy virtuális gépet.

toocreate hello virtuális gépet, a kód toohello fő metódus hozzáadása:

```
Console.WriteLine("Creating virtual machine...");
azure.VirtualMachines.Define(vmName)
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithExistingPrimaryNetworkInterface(networkInterface)
    .WithLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .WithAdminUsername("azureuser")
    .WithAdminPassword("Azure12345678")
    .WithComputerName(vmName)
    .WithExistingAvailabilitySet(availabilitySet)
    .WithSize(VirtualMachineSizeTypes.StandardDS1)
    .Create();
```

> [!NOTE]
> Ebben az oktatóanyagban létrehoz egy virtuális gépet, hello Windows Server operációs rendszer verziója. További információ az egyéb rendszerképek kiválasztásáról toolearn lásd: [keresse meg és válassza ki azokat a Windows PowerShell és az Azure parancssori felület hello Azure virtuális gép lemezképeket](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
>

Ha azt szeretné, hogy a meglévő lemez Piactéri rendszerkép helyett toouse, használja ezt a kódot:

```
var managedDisk = azure.Disks.Define("myosdisk")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithWindowsFromVhd("https://mystorage.blob.core.windows.net/vhds/myosdisk.vhd")
    .WithSizeInGB(128)
    .WithSku(DiskSkuTypes.PremiumLRS)
    .Create();

azure.VirtualMachines.Define("myVM")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithExistingPrimaryNetworkInterface(networkInterface)
    .WithSpecializedOSDisk(managedDisk, OperatingSystemTypes.Windows)
    .WithExistingAvailabilitySet(availabilitySet)
    .WithSize(VirtualMachineSizeTypes.StandardDS1)
    .Create();
```

## <a name="perform-management-tasks"></a>Felügyeleti feladatok végrehajtása

A virtuális gépek hello életciklusa során szükség lehet a toorun felügyeleti feladatokhoz, mint az indítása, leállítása vagy egy virtuális gép törlése. Emellett érdemes lehet toocreate kód tooautomate ismétlődő vagy összetett feladatokat.

Ha kell toodo minden virtuális gép hello, tooget egy példányát kell:

```
var vm = azure.VirtualMachines.GetByResourceGroup(groupName, vmName);
```

### <a name="get-information-about-hello-vm"></a>Hello virtuális gép adatainak beolvasása

hello virtuális gép, tooget adatait a kód toohello fő metódus hozzáadása:

```
Console.WriteLine("Getting information about hello virtual machine...");
Console.WriteLine("hardwareProfile");
Console.WriteLine("   vmSize: " + vm.Size);
Console.WriteLine("storageProfile");
Console.WriteLine("  imageReference");
Console.WriteLine("    publisher: " + vm.StorageProfile.ImageReference.Publisher);
Console.WriteLine("    offer: " + vm.StorageProfile.ImageReference.Offer);
Console.WriteLine("    sku: " + vm.StorageProfile.ImageReference.Sku);
Console.WriteLine("    version: " + vm.StorageProfile.ImageReference.Version);
Console.WriteLine("  osDisk");
Console.WriteLine("    osType: " + vm.StorageProfile.OsDisk.OsType);
Console.WriteLine("    name: " + vm.StorageProfile.OsDisk.Name);
Console.WriteLine("    createOption: " + vm.StorageProfile.OsDisk.CreateOption);
Console.WriteLine("    caching: " + vm.StorageProfile.OsDisk.Caching);
Console.WriteLine("osProfile");
Console.WriteLine("  computerName: " + vm.OSProfile.ComputerName);
Console.WriteLine("  adminUsername: " + vm.OSProfile.AdminUsername);
Console.WriteLine("  provisionVMAgent: " + vm.OSProfile.WindowsConfiguration.ProvisionVMAgent.Value);
Console.WriteLine("  enableAutomaticUpdates: " + vm.OSProfile.WindowsConfiguration.EnableAutomaticUpdates.Value);
Console.WriteLine("networkProfile");
foreach (string nicId in vm.NetworkInterfaceIds)
{
    Console.WriteLine("  networkInterface id: " + nicId);
}
Console.WriteLine("vmAgent");
Console.WriteLine("  vmAgentVersion" + vm.InstanceView.VmAgent.VmAgentVersion);
Console.WriteLine("    statuses");
foreach (InstanceViewStatus stat in vm.InstanceView.VmAgent.Statuses)
{
    Console.WriteLine("    code: " + stat.Code);
    Console.WriteLine("    level: " + stat.Level);
    Console.WriteLine("    displayStatus: " + stat.DisplayStatus);
    Console.WriteLine("    message: " + stat.Message);
    Console.WriteLine("    time: " + stat.Time);
}
Console.WriteLine("disks");
foreach (DiskInstanceView disk in vm.InstanceView.Disks)
{
    Console.WriteLine("  name: " + disk.Name);
    Console.WriteLine("  statuses");
    foreach (InstanceViewStatus stat in disk.Statuses)
    {
        Console.WriteLine("    code: " + stat.Code);
        Console.WriteLine("    level: " + stat.Level);
        Console.WriteLine("    displayStatus: " + stat.DisplayStatus);
        Console.WriteLine("    time: " + stat.Time);
    }
}
Console.WriteLine("VM general status");
Console.WriteLine("  provisioningStatus: " + vm.ProvisioningState);
Console.WriteLine("  id: " + vm.Id);
Console.WriteLine("  name: " + vm.Name);
Console.WriteLine("  type: " + vm.Type);
Console.WriteLine("  location: " + vm.Region);
Console.WriteLine("VM instance status");
foreach (InstanceViewStatus stat in vm.InstanceView.Statuses)
{
    Console.WriteLine("  code: " + stat.Code);
    Console.WriteLine("  level: " + stat.Level);
    Console.WriteLine("  displayStatus: " + stat.DisplayStatus);
}
Console.WriteLine("Press enter toocontinue...");
Console.ReadLine();
```

### <a name="stop-hello-vm"></a>Hello VM leállítása

Állítsa le a virtuális gépet és a beállítások megtartásához, de továbbra is toobe felszámított, vagy állítsa le a virtuális gépet, és azt felszabadítani. Ha egy virtuális gép fel van szabadítva, vele társított összes erőforrás is felszabadított és számlázási végpontjainak.

toostop hello virtuális gép felszabadítása, nélkül adja hozzá a kódot toohello fő metódus:

```
Console.WriteLine("Stopping vm...");
vm.PowerOff();
Console.WriteLine("Press enter toocontinue...");
Console.ReadLine();
```

Ha azt szeretné, hogy toodeallocate hello virtuális gép, módosítsa a hello kikapcsolt hívás toothis kódot:

```
vm.Deallocate();
```

### <a name="start-hello-vm"></a>Indítsa el a virtuális gép hello

toostart hello virtuális gépet, a kód toohello fő metódus hozzáadása:

```
Console.WriteLine("Starting vm...");
vm.Start();
Console.WriteLine("Press enter toocontinue...");
Console.ReadLine();
```

### <a name="resize-hello-vm"></a>Automatikus oszlopszélesség hello méretű VM

Telepítési sok szempontját figyelembe kell venni, amikor eldönti, a virtuális gép méretét. További információkért lásd: [Virtuálisgép-méretek](sizes.md).  

hello virtuális gép, toochange mérete a kód toohello fő metódus hozzáadása:

```
Console.WriteLine("Resizing vm...");
vm.Update()
    .WithSize(VirtualMachineSizeTypes.StandardDS2) 
    .Apply();
Console.WriteLine("Press enter toocontinue...");
Console.ReadLine();
```

### <a name="add-a-data-disk-toohello-vm"></a>Adja hozzá a adatok lemez toohello méretű VM

tooadd adatok lemez toohello virtuális gép, a kód toohello fő metódus tooadd, amely 2 GB-nál, 0 és a gyorsítótárazási ReadWrite a logikai egység han adatlemez hozzáadása:

```
Console.WriteLine("Adding data disk toovm...");
vm.Update()
    .WithNewDataDisk(2, 0, CachingTypes.ReadWrite) 
    .Apply();
Console.WriteLine("Press enter toodelete resources...");
Console.ReadLine();
```

## <a name="delete-resources"></a>Erőforrások törlése

Mivel az Azure-ban használt erőforrásokhoz van szó, még mindig célszerű toodelete erőforrásokat, amelyek már nem szükséges. Ha azt szeretné, hogy toodelete hello virtuális gépek és erőforrások támogató összes hello, minden toodo van hello erőforrás csoport törlése.

toodelete hello erőforrás csoportjában adja hozzá a kódot toohello fő metódus:

```
azure.ResourceGroups.DeleteByName(groupName);
```

## <a name="run-hello-application"></a>Hello alkalmazás futtatása

Akkor kell a konzol alkalmazás toorun teljesen a start toofinish körülbelül öt percet is igénybe vehet. 

1. toorun hello konzolalkalmazást, kattintson a **Start**.

2. Ahhoz, hogy nyomja le az ENTER **Enter** toostart törlése erőforrásokat, készíthet néhány perc múlva hello erőforrások tooverify hello létrehozását a hello Azure-portálon. Kattintson a hello telepítési toosee telepítésére vonatkozó állapotadatok hello.

## <a name="next-steps"></a>Következő lépések
* Kihasználhatja a egy sablon toocreate használatával egy virtuális gép hello témakörben található információk alapján [központi telepítése egy Azure virtuális gépen a C# és a Resource Manager-sablon használatával](csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* További információ hello [Azure-könyvtárakban .NET](https://docs.microsoft.com/dotnet/azure/?view=azure-dotnet).

