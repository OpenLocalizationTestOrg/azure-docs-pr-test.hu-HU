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
# <a name="create-and-manage-windows-vms-in-azure-using-c"></a><span data-ttu-id="f3c60-103">Létrehozása és kezelése Windows-alapú virtuális gépek az Azure-ban C#</span><span class="sxs-lookup"><span data-stu-id="f3c60-103">Create and manage Windows VMs in Azure using C#</span></span> #

<span data-ttu-id="f3c60-104">Egy [Azure virtuális gép](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) több támogató Azure-erőforrások kell.</span><span class="sxs-lookup"><span data-stu-id="f3c60-104">An [Azure Virtual Machine](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) needs several supporting Azure resources.</span></span> <span data-ttu-id="f3c60-105">Ez a cikk ismerteti a létrehozása, kezelése és C# használatával Virtuálisgép-erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="f3c60-105">This article covers creating, managing, and deleting VM resources using C#.</span></span> <span data-ttu-id="f3c60-106">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="f3c60-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f3c60-107">Visual Studio-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3c60-107">Create a Visual Studio project</span></span>
> * <span data-ttu-id="f3c60-108">Hello csomag telepítése</span><span class="sxs-lookup"><span data-stu-id="f3c60-108">Install hello package</span></span>
> * <span data-ttu-id="f3c60-109">Hitelesítő adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3c60-109">Create credentials</span></span>
> * <span data-ttu-id="f3c60-110">Erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3c60-110">Create resources</span></span>
> * <span data-ttu-id="f3c60-111">Felügyeleti feladatok végrehajtása</span><span class="sxs-lookup"><span data-stu-id="f3c60-111">Perform management tasks</span></span>
> * <span data-ttu-id="f3c60-112">Erőforrások törlése</span><span class="sxs-lookup"><span data-stu-id="f3c60-112">Delete resources</span></span>
> * <span data-ttu-id="f3c60-113">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="f3c60-113">Run hello application</span></span>

<span data-ttu-id="f3c60-114">Körülbelül 20 percet toodo szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="f3c60-114">It takes about 20 minutes toodo these steps.</span></span>

## <a name="create-a-visual-studio-project"></a><span data-ttu-id="f3c60-115">Visual Studio-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3c60-115">Create a Visual Studio project</span></span>

1. <span data-ttu-id="f3c60-116">Ha még nem tette meg, telepítse [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="f3c60-116">If you haven't already, install [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).</span></span> <span data-ttu-id="f3c60-117">Válassza ki **.NET asztali fejlesztési** hello munkaterhelések lap, és kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="f3c60-117">Select **.NET desktop development** on hello Workloads page, and then click **Install**.</span></span> <span data-ttu-id="f3c60-118">Az összefoglaló hello, láthatja, hogy **.NET-keretrendszer 4-4.6 Fejlesztőeszközök** automatikusan ki van jelölve meg.</span><span class="sxs-lookup"><span data-stu-id="f3c60-118">In hello summary, you can see that **.NET Framework 4 - 4.6 development tools** is automatically selected for you.</span></span> <span data-ttu-id="f3c60-119">Ha már telepítette a Visual Studio, a Visual Studio indítója hello segítségével hello .NET munkaterhelés is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="f3c60-119">If you have already installed Visual Studio, you can add hello .NET workload using hello Visual Studio Launcher.</span></span>
2. <span data-ttu-id="f3c60-120">A Visual Studióban kattintson **fájl** > **új** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="f3c60-120">In Visual Studio, click **File** > **New** > **Project**.</span></span>
3. <span data-ttu-id="f3c60-121">A **sablonok** > **Visual C#**, jelölje be **Konzolalkalmazás (.NET-keretrendszer)**, adja meg *myDotnetProject* hello neveként hello projekt, hello projekt, jelölje be hello helyét, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="f3c60-121">In **Templates** > **Visual C#**, select **Console App (.NET Framework)**, enter *myDotnetProject* for hello name of hello project, select hello location of hello project, and then click **OK**.</span></span>

## <a name="install-hello-package"></a><span data-ttu-id="f3c60-122">Hello csomag telepítése</span><span class="sxs-lookup"><span data-stu-id="f3c60-122">Install hello package</span></span>

<span data-ttu-id="f3c60-123">NuGet-csomagok olyan hello legegyszerűbb módja tooinstall hello könyvtárak kell toofinish ezeket a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="f3c60-123">NuGet packages are hello easiest way tooinstall hello libraries that you need toofinish these steps.</span></span> <span data-ttu-id="f3c60-124">tooget hello szalagtárak, amelyekre szüksége van a Visual Studio, hajtsa végre ezeket a lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f3c60-124">tooget hello libraries that you need in Visual Studio, do these steps:</span></span>

1. <span data-ttu-id="f3c60-125">Kattintson a **eszközök** > **Nuget-Csomagkezelő**, és kattintson a **Csomagkezelő konzol**.</span><span class="sxs-lookup"><span data-stu-id="f3c60-125">Click **Tools** > **Nuget Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="f3c60-126">Hello konzolján futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="f3c60-126">Type this command in hello console:</span></span>

    ```
    Install-Package Microsoft.Azure.Management.Fluent
    ```

## <a name="create-credentials"></a><span data-ttu-id="f3c60-127">Hitelesítő adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3c60-127">Create credentials</span></span>

<span data-ttu-id="f3c60-128">Ez a lépés megkezdése előtt győződjön meg arról, hogy rendelkezik-e hozzáférési tooan [Active Directory szolgáltatás egyszerű](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f3c60-128">Before you start this step, make sure that you have access tooan [Active Directory service principal](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="f3c60-129">Is rögzíteni kell hello Alkalmazásazonosító, hello hitelesítési kulcs és hello Bérlőazonosító, amelyekre szüksége van egy későbbi lépésben.</span><span class="sxs-lookup"><span data-stu-id="f3c60-129">You should also record hello application ID, hello authentication key, and hello tenant ID that you need in a later step.</span></span>

### <a name="create-hello-authorization-file"></a><span data-ttu-id="f3c60-130">Hello engedélyezési fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3c60-130">Create hello authorization file</span></span>

1. <span data-ttu-id="f3c60-131">A Megoldáskezelőben kattintson a jobb gombbal *myDotnetProject* > **Hozzáadás** > **új elem**, majd válassza ki **szövegfájl** a *Visual C# elemek*.</span><span class="sxs-lookup"><span data-stu-id="f3c60-131">In Solution Explorer, right-click *myDotnetProject* > **Add** > **New Item**, and then select **Text File** in *Visual C# Items*.</span></span> <span data-ttu-id="f3c60-132">Nevű hello fájl *azureauth.properties*, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="f3c60-132">Name hello file *azureauth.properties*, and then click **Add**.</span></span>
2. <span data-ttu-id="f3c60-133">Adja hozzá a engedélyezési tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="f3c60-133">Add these authorization properties:</span></span>

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

    <span data-ttu-id="f3c60-134">Cserélje le  **&lt;előfizetés-azonosító&gt;**  az előfizetés-azonosítóval rendelkező  **&lt;alkalmazásazonosító&gt;**  a hello Active Directory-alkalmazás azonosítóval  **&lt;hitelesítési kulcs&gt;**  hello alkalmazás kulccsal, és  **&lt;bérlőazonosító&gt;**  hello bérlővel azonosítója.</span><span class="sxs-lookup"><span data-stu-id="f3c60-134">Replace **&lt;subscription-id&gt;** with your subscription identifier, **&lt;application-id&gt;** with hello Active Directory application identifier, **&lt;authentication-key&gt;** with hello application key, and **&lt;tenant-id&gt;** with hello tenant identifier.</span></span>

3. <span data-ttu-id="f3c60-135">Hello azureauth.properties fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="f3c60-135">Save hello azureauth.properties file.</span></span> 
4. <span data-ttu-id="f3c60-136">A Windows hello teljes elérési útja tooauthorization létrehozott fájl AZURE_AUTH_LOCATION nevű környezeti változó értéke.</span><span class="sxs-lookup"><span data-stu-id="f3c60-136">Set an environment variable in Windows named AZURE_AUTH_LOCATION with hello full path tooauthorization file that you created.</span></span> <span data-ttu-id="f3c60-137">Például hello a következő PowerShell-parancs használható:</span><span class="sxs-lookup"><span data-stu-id="f3c60-137">For example, hello following PowerShell command can be used:</span></span>

    ```
    [Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\Visual Studio 2017\Projects\myDotnetProject\myDotnetProject\azureauth.properties", "User")
    ```

### <a name="create-hello-management-client"></a><span data-ttu-id="f3c60-138">Hello felügyeleti ügyfél létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3c60-138">Create hello management client</span></span>

1. <span data-ttu-id="f3c60-139">Nyissa meg a Program.cs fájl hello hello projekthez létrehozott, és adja hozzá ezek az utasítások toohello meglévő utasítások segítségével a hello fájl felső:</span><span class="sxs-lookup"><span data-stu-id="f3c60-139">Open hello Program.cs file for hello project that you created, and then add these using statements toohello existing statements at top of hello file:</span></span>

    ```
    using Microsoft.Azure.Management.Compute.Fluent;
    using Microsoft.Azure.Management.Compute.Fluent.Models;
    using Microsoft.Azure.Management.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
    ```

2. <span data-ttu-id="f3c60-140">toocreate hello felügyeleti ügyfél, a kód toohello fő metódus hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="f3c60-140">toocreate hello management client, add this code toohello Main method:</span></span>

    ```
    var credentials = SdkContext.AzureCredentialsFactory
        .FromFile(Environment.GetEnvironmentVariable("AZURE_AUTH_LOCATION"));

    var azure = Azure
        .Configure()
        .WithLogLevel(HttpLoggingDelegatingHandler.Level.Basic)
        .Authenticate(credentials)
        .WithDefaultSubscription();
    ```

## <a name="create-resources"></a><span data-ttu-id="f3c60-141">Erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3c60-141">Create resources</span></span>

### <a name="create-hello-resource-group"></a><span data-ttu-id="f3c60-142">Hello erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3c60-142">Create hello resource group</span></span>

<span data-ttu-id="f3c60-143">Minden erőforrás tartalmaznia kell egy [erőforráscsoport](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f3c60-143">All resources must be contained in a [Resource group](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="f3c60-144">toospecify értékei alkalmazás hello és hello erőforráscsoport létrehozása, a kód toohello fő metódus hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="f3c60-144">toospecify values for hello application and create hello resource group, add this code toohello Main method:</span></span>

```
var groupName = "myResourceGroup";
var vmName = "myVM";
var location = Region.USWest;
    
Console.WriteLine("Creating resource group...");
var resourceGroup = azure.ResourceGroups.Define(groupName)
    .WithRegion(location)
    .Create();
```

### <a name="create-hello-availability-set"></a><span data-ttu-id="f3c60-145">Hello rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3c60-145">Create hello availability set</span></span>

<span data-ttu-id="f3c60-146">[Rendelkezésre állási készletek](tutorial-availability-sets.md) könnyebben meg az alkalmazás által használt toomaintain hello virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="f3c60-146">[Availability sets](tutorial-availability-sets.md) make it easier for you toomaintain hello virtual machines used by your application.</span></span>

<span data-ttu-id="f3c60-147">toocreate hello rendelkezésre állási beállítása, a kód toohello fő metódus hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="f3c60-147">toocreate hello availability set, add this code toohello Main method:</span></span>

```
Console.WriteLine("Creating availability set...");
var availabilitySet = azure.AvailabilitySets.Define("myAVSet")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithSku(AvailabilitySetSkuTypes.Managed)
    .Create();
```

### <a name="create-hello-public-ip-address"></a><span data-ttu-id="f3c60-148">Hello nyilvános IP-cím létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3c60-148">Create hello public IP address</span></span>

<span data-ttu-id="f3c60-149">A [nyilvános IP-cím](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) szükséges toocommunicate hello virtuális gép van.</span><span class="sxs-lookup"><span data-stu-id="f3c60-149">A [Public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) is needed toocommunicate with hello virtual machine.</span></span>

<span data-ttu-id="f3c60-150">toocreate hello nyilvános IP-cím hello virtuális géphez, a kód toohello fő metódus hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="f3c60-150">toocreate hello public IP address for hello virtual machine, add this code toohello Main method:</span></span>
   
```
Console.WriteLine("Creating public IP address...");
var publicIPAddress = azure.PublicIPAddresses.Define("myPublicIP")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithDynamicIP()
    .Create();
```

### <a name="create-hello-virtual-network"></a><span data-ttu-id="f3c60-151">Hello virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3c60-151">Create hello virtual network</span></span>

<span data-ttu-id="f3c60-152">Az alhálózat szerepelnie kell egy virtuális gépet egy [virtuális hálózati](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f3c60-152">A virtual machine must be in a subnet of a [Virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

<span data-ttu-id="f3c60-153">toocreate egy alhálózatot és egy virtuális hálózatot, adja hozzá a kódot toohello fő metódus:</span><span class="sxs-lookup"><span data-stu-id="f3c60-153">toocreate a subnet and a virtual network, add this code toohello Main method:</span></span>

```
Console.WriteLine("Creating virtual network...");
var network = azure.Networks.Define("myVNet")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithAddressSpace("10.0.0.0/16")
    .WithSubnet("mySubnet", "10.0.0.0/24")
    .Create();
```

### <a name="create-hello-network-interface"></a><span data-ttu-id="f3c60-154">Hello hálózati illesztő létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3c60-154">Create hello network interface</span></span>

<span data-ttu-id="f3c60-155">A virtuális gép egy hálózati illesztő toocommunicate hello virtuális hálózaton kell.</span><span class="sxs-lookup"><span data-stu-id="f3c60-155">A virtual machine needs a network interface toocommunicate on hello virtual network.</span></span>

<span data-ttu-id="f3c60-156">egy adott hálózati csatoló toocreate a kód toohello fő metódus hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="f3c60-156">toocreate a network interface, add this code toohello Main method:</span></span>

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

### <a name="create-hello-virtual-machine"></a><span data-ttu-id="f3c60-157">Hello virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3c60-157">Create hello virtual machine</span></span>

<span data-ttu-id="f3c60-158">Most, hogy a létrehozott összes hello erőforrások támogatása, létrehozhat egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="f3c60-158">Now that you created all hello supporting resources, you can create a virtual machine.</span></span>

<span data-ttu-id="f3c60-159">toocreate hello virtuális gépet, a kód toohello fő metódus hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="f3c60-159">toocreate hello virtual machine, add this code toohello Main method:</span></span>

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
> <span data-ttu-id="f3c60-160">Ebben az oktatóanyagban létrehoz egy virtuális gépet, hello Windows Server operációs rendszer verziója.</span><span class="sxs-lookup"><span data-stu-id="f3c60-160">This tutorial creates a virtual machine running a version of hello Windows Server operating system.</span></span> <span data-ttu-id="f3c60-161">További információ az egyéb rendszerképek kiválasztásáról toolearn lásd: [keresse meg és válassza ki azokat a Windows PowerShell és az Azure parancssori felület hello Azure virtuális gép lemezképeket](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f3c60-161">toolearn more about selecting other images, see [Navigate and select Azure virtual machine images with Windows PowerShell and hello Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
>

<span data-ttu-id="f3c60-162">Ha azt szeretné, hogy a meglévő lemez Piactéri rendszerkép helyett toouse, használja ezt a kódot:</span><span class="sxs-lookup"><span data-stu-id="f3c60-162">If you want toouse an existing disk instead of a marketplace image, use this code:</span></span>

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

## <a name="perform-management-tasks"></a><span data-ttu-id="f3c60-163">Felügyeleti feladatok végrehajtása</span><span class="sxs-lookup"><span data-stu-id="f3c60-163">Perform management tasks</span></span>

<span data-ttu-id="f3c60-164">A virtuális gépek hello életciklusa során szükség lehet a toorun felügyeleti feladatokhoz, mint az indítása, leállítása vagy egy virtuális gép törlése.</span><span class="sxs-lookup"><span data-stu-id="f3c60-164">During hello lifecycle of a virtual machine, you may want toorun management tasks such as starting, stopping, or deleting a virtual machine.</span></span> <span data-ttu-id="f3c60-165">Emellett érdemes lehet toocreate kód tooautomate ismétlődő vagy összetett feladatokat.</span><span class="sxs-lookup"><span data-stu-id="f3c60-165">Additionally, you may want toocreate code tooautomate repetitive or complex tasks.</span></span>

<span data-ttu-id="f3c60-166">Ha kell toodo minden virtuális gép hello, tooget egy példányát kell:</span><span class="sxs-lookup"><span data-stu-id="f3c60-166">When you need toodo anything with hello VM, you need tooget an instance of it:</span></span>

```
var vm = azure.VirtualMachines.GetByResourceGroup(groupName, vmName);
```

### <a name="get-information-about-hello-vm"></a><span data-ttu-id="f3c60-167">Hello virtuális gép adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="f3c60-167">Get information about hello VM</span></span>

<span data-ttu-id="f3c60-168">hello virtuális gép, tooget adatait a kód toohello fő metódus hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="f3c60-168">tooget information about hello virtual machine, add this code toohello Main method:</span></span>

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

### <a name="stop-hello-vm"></a><span data-ttu-id="f3c60-169">Hello VM leállítása</span><span class="sxs-lookup"><span data-stu-id="f3c60-169">Stop hello VM</span></span>

<span data-ttu-id="f3c60-170">Állítsa le a virtuális gépet és a beállítások megtartásához, de továbbra is toobe felszámított, vagy állítsa le a virtuális gépet, és azt felszabadítani.</span><span class="sxs-lookup"><span data-stu-id="f3c60-170">You can stop a virtual machine and keep all its settings, but continue toobe charged for it, or you can stop a virtual machine and deallocate it.</span></span> <span data-ttu-id="f3c60-171">Ha egy virtuális gép fel van szabadítva, vele társított összes erőforrás is felszabadított és számlázási végpontjainak.</span><span class="sxs-lookup"><span data-stu-id="f3c60-171">When a virtual machine is deallocated, all resources associated with it are also deallocated and billing ends for it.</span></span>

<span data-ttu-id="f3c60-172">toostop hello virtuális gép felszabadítása, nélkül adja hozzá a kódot toohello fő metódus:</span><span class="sxs-lookup"><span data-stu-id="f3c60-172">toostop hello virtual machine without deallocating it, add this code toohello Main method:</span></span>

```
Console.WriteLine("Stopping vm...");
vm.PowerOff();
Console.WriteLine("Press enter toocontinue...");
Console.ReadLine();
```

<span data-ttu-id="f3c60-173">Ha azt szeretné, hogy toodeallocate hello virtuális gép, módosítsa a hello kikapcsolt hívás toothis kódot:</span><span class="sxs-lookup"><span data-stu-id="f3c60-173">If you want toodeallocate hello virtual machine, change hello PowerOff call toothis code:</span></span>

```
vm.Deallocate();
```

### <a name="start-hello-vm"></a><span data-ttu-id="f3c60-174">Indítsa el a virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="f3c60-174">Start hello VM</span></span>

<span data-ttu-id="f3c60-175">toostart hello virtuális gépet, a kód toohello fő metódus hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="f3c60-175">toostart hello virtual machine, add this code toohello Main method:</span></span>

```
Console.WriteLine("Starting vm...");
vm.Start();
Console.WriteLine("Press enter toocontinue...");
Console.ReadLine();
```

### <a name="resize-hello-vm"></a><span data-ttu-id="f3c60-176">Automatikus oszlopszélesség hello méretű VM</span><span class="sxs-lookup"><span data-stu-id="f3c60-176">Resize hello VM</span></span>

<span data-ttu-id="f3c60-177">Telepítési sok szempontját figyelembe kell venni, amikor eldönti, a virtuális gép méretét.</span><span class="sxs-lookup"><span data-stu-id="f3c60-177">Many aspects of deployment should be considered when deciding on a size for your virtual machine.</span></span> <span data-ttu-id="f3c60-178">További információkért lásd: [Virtuálisgép-méretek](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="f3c60-178">For more information, see [VM sizes](sizes.md).</span></span>  

<span data-ttu-id="f3c60-179">hello virtuális gép, toochange mérete a kód toohello fő metódus hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="f3c60-179">toochange size of hello virtual machine, add this code toohello Main method:</span></span>

```
Console.WriteLine("Resizing vm...");
vm.Update()
    .WithSize(VirtualMachineSizeTypes.StandardDS2) 
    .Apply();
Console.WriteLine("Press enter toocontinue...");
Console.ReadLine();
```

### <a name="add-a-data-disk-toohello-vm"></a><span data-ttu-id="f3c60-180">Adja hozzá a adatok lemez toohello méretű VM</span><span class="sxs-lookup"><span data-stu-id="f3c60-180">Add a data disk toohello VM</span></span>

<span data-ttu-id="f3c60-181">tooadd adatok lemez toohello virtuális gép, a kód toohello fő metódus tooadd, amely 2 GB-nál, 0 és a gyorsítótárazási ReadWrite a logikai egység han adatlemez hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="f3c60-181">tooadd a data disk toohello virtual machine, add this code toohello Main method tooadd a data disk that is 2 GB in size, han a LUN of 0 and a caching type of ReadWrite:</span></span>

```
Console.WriteLine("Adding data disk toovm...");
vm.Update()
    .WithNewDataDisk(2, 0, CachingTypes.ReadWrite) 
    .Apply();
Console.WriteLine("Press enter toodelete resources...");
Console.ReadLine();
```

## <a name="delete-resources"></a><span data-ttu-id="f3c60-182">Erőforrások törlése</span><span class="sxs-lookup"><span data-stu-id="f3c60-182">Delete resources</span></span>

<span data-ttu-id="f3c60-183">Mivel az Azure-ban használt erőforrásokhoz van szó, még mindig célszerű toodelete erőforrásokat, amelyek már nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="f3c60-183">Because you are charged for resources used in Azure, it is always good practice toodelete resources that are no longer needed.</span></span> <span data-ttu-id="f3c60-184">Ha azt szeretné, hogy toodelete hello virtuális gépek és erőforrások támogató összes hello, minden toodo van hello erőforrás csoport törlése.</span><span class="sxs-lookup"><span data-stu-id="f3c60-184">If you want toodelete hello virtual machines and all hello supporting resources, all you have toodo is delete hello resource group.</span></span>

<span data-ttu-id="f3c60-185">toodelete hello erőforrás csoportjában adja hozzá a kódot toohello fő metódus:</span><span class="sxs-lookup"><span data-stu-id="f3c60-185">toodelete hello resource group, add this code toohello Main method:</span></span>

```
azure.ResourceGroups.DeleteByName(groupName);
```

## <a name="run-hello-application"></a><span data-ttu-id="f3c60-186">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="f3c60-186">Run hello application</span></span>

<span data-ttu-id="f3c60-187">Akkor kell a konzol alkalmazás toorun teljesen a start toofinish körülbelül öt percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="f3c60-187">It should take about five minutes for this console application toorun completely from start toofinish.</span></span> 

1. <span data-ttu-id="f3c60-188">toorun hello konzolalkalmazást, kattintson a **Start**.</span><span class="sxs-lookup"><span data-stu-id="f3c60-188">toorun hello console application, click **Start**.</span></span>

2. <span data-ttu-id="f3c60-189">Ahhoz, hogy nyomja le az ENTER **Enter** toostart törlése erőforrásokat, készíthet néhány perc múlva hello erőforrások tooverify hello létrehozását a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="f3c60-189">Before you press **Enter** toostart deleting resources, you could take a few minutes tooverify hello creation of hello resources in hello Azure portal.</span></span> <span data-ttu-id="f3c60-190">Kattintson a hello telepítési toosee telepítésére vonatkozó állapotadatok hello.</span><span class="sxs-lookup"><span data-stu-id="f3c60-190">Click hello deployment status toosee information about hello deployment.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f3c60-191">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f3c60-191">Next steps</span></span>
* <span data-ttu-id="f3c60-192">Kihasználhatja a egy sablon toocreate használatával egy virtuális gép hello témakörben található információk alapján [központi telepítése egy Azure virtuális gépen a C# és a Resource Manager-sablon használatával](csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f3c60-192">Take advantage of using a template toocreate a virtual machine by using hello information in [Deploy an Azure Virtual Machine using C# and a Resource Manager template](csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="f3c60-193">További információ hello [Azure-könyvtárakban .NET](https://docs.microsoft.com/dotnet/azure/?view=azure-dotnet).</span><span class="sxs-lookup"><span data-stu-id="f3c60-193">Learn more about using hello [Azure libraries for .NET](https://docs.microsoft.com/dotnet/azure/?view=azure-dotnet).</span></span>

