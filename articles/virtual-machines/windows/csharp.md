---
title: "Létrehozását és kezelését egy Azure virtuális gépen a C# használatával |} Microsoft Docs"
description: "C# és az Azure Resource Manager segítségével telepítheti a virtuális gép és annak támogató erőforrásokat."
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
ms.openlocfilehash: 5d9021c2f65b70e36d5ea82992c9fb9d2d6d394a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-manage-windows-vms-in-azure-using-c"></a><span data-ttu-id="fadd9-103">Létrehozása és kezelése Windows-alapú virtuális gépek az Azure-ban C#</span><span class="sxs-lookup"><span data-stu-id="fadd9-103">Create and manage Windows VMs in Azure using C#</span></span> #

<span data-ttu-id="fadd9-104">Egy [Azure virtuális gép](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) több támogató Azure-erőforrások kell.</span><span class="sxs-lookup"><span data-stu-id="fadd9-104">An [Azure Virtual Machine](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) needs several supporting Azure resources.</span></span> <span data-ttu-id="fadd9-105">Ez a cikk ismerteti a létrehozása, kezelése és C# használatával Virtuálisgép-erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="fadd9-105">This article covers creating, managing, and deleting VM resources using C#.</span></span> <span data-ttu-id="fadd9-106">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="fadd9-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fadd9-107">Visual Studio-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="fadd9-107">Create a Visual Studio project</span></span>
> * <span data-ttu-id="fadd9-108">A csomag telepítése</span><span class="sxs-lookup"><span data-stu-id="fadd9-108">Install the package</span></span>
> * <span data-ttu-id="fadd9-109">Hitelesítő adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="fadd9-109">Create credentials</span></span>
> * <span data-ttu-id="fadd9-110">Erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="fadd9-110">Create resources</span></span>
> * <span data-ttu-id="fadd9-111">Felügyeleti feladatok végrehajtása</span><span class="sxs-lookup"><span data-stu-id="fadd9-111">Perform management tasks</span></span>
> * <span data-ttu-id="fadd9-112">Erőforrások törlése</span><span class="sxs-lookup"><span data-stu-id="fadd9-112">Delete resources</span></span>
> * <span data-ttu-id="fadd9-113">Az alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="fadd9-113">Run the application</span></span>

<span data-ttu-id="fadd9-114">A lépések elvégzéséhez körülbelül 20 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="fadd9-114">It takes about 20 minutes to do these steps.</span></span>

## <a name="create-a-visual-studio-project"></a><span data-ttu-id="fadd9-115">Visual Studio-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="fadd9-115">Create a Visual Studio project</span></span>

1. <span data-ttu-id="fadd9-116">Ha még nem tette meg, telepítse [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="fadd9-116">If you haven't already, install [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).</span></span> <span data-ttu-id="fadd9-117">Válassza ki **.NET asztali fejlesztési** a munkaterhelések lapot, és kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="fadd9-117">Select **.NET desktop development** on the Workloads page, and then click **Install**.</span></span> <span data-ttu-id="fadd9-118">Az összegzés láthatja, hogy **.NET-keretrendszer 4-4.6 Fejlesztőeszközök** automatikusan ki van jelölve meg.</span><span class="sxs-lookup"><span data-stu-id="fadd9-118">In the summary, you can see that **.NET Framework 4 - 4.6 development tools** is automatically selected for you.</span></span> <span data-ttu-id="fadd9-119">Ha már telepítette a Visual Studio, a .NET alkalmazások és szolgáltatások segítségével a Visual Studio indítója adhat meg.</span><span class="sxs-lookup"><span data-stu-id="fadd9-119">If you have already installed Visual Studio, you can add the .NET workload using the Visual Studio Launcher.</span></span>
2. <span data-ttu-id="fadd9-120">A Visual Studióban kattintson **fájl** > **új** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="fadd9-120">In Visual Studio, click **File** > **New** > **Project**.</span></span>
3. <span data-ttu-id="fadd9-121">A **sablonok** > **Visual C#**, jelölje be **Konzolalkalmazás (.NET-keretrendszer)**, adja meg *myDotnetProject* a projekt nevét, válassza ki a projekt helyét, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="fadd9-121">In **Templates** > **Visual C#**, select **Console App (.NET Framework)**, enter *myDotnetProject* for the name of the project, select the location of the project, and then click **OK**.</span></span>

## <a name="install-the-package"></a><span data-ttu-id="fadd9-122">A csomag telepítése</span><span class="sxs-lookup"><span data-stu-id="fadd9-122">Install the package</span></span>

<span data-ttu-id="fadd9-123">NuGet-csomagok használata a lépések végrehajtása szükséges kódtárak telepítése a legegyszerűbb.</span><span class="sxs-lookup"><span data-stu-id="fadd9-123">NuGet packages are the easiest way to install the libraries that you need to finish these steps.</span></span> <span data-ttu-id="fadd9-124">Ahhoz, hogy a Visual Studio szükséges könyvtárak, hajtsa végre ezeket a lépéseket:</span><span class="sxs-lookup"><span data-stu-id="fadd9-124">To get the libraries that you need in Visual Studio, do these steps:</span></span>

1. <span data-ttu-id="fadd9-125">Kattintson a **eszközök** > **Nuget-Csomagkezelő**, és kattintson a **Csomagkezelő konzol**.</span><span class="sxs-lookup"><span data-stu-id="fadd9-125">Click **Tools** > **Nuget Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="fadd9-126">Ezt a parancsot a konzolon:</span><span class="sxs-lookup"><span data-stu-id="fadd9-126">Type this command in the console:</span></span>

    ```
    Install-Package Microsoft.Azure.Management.Fluent
    ```

## <a name="create-credentials"></a><span data-ttu-id="fadd9-127">Hitelesítő adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="fadd9-127">Create credentials</span></span>

<span data-ttu-id="fadd9-128">Ez a lépés megkezdése előtt győződjön meg arról, hogy rendelkezik-e a hozzáférést egy [Active Directory szolgáltatás egyszerű](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fadd9-128">Before you start this step, make sure that you have access to an [Active Directory service principal](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="fadd9-129">Is rögzíteni kell az alkalmazás Azonosítóját, a hitelesítési kulcs és a bérlő azonosítója, amelyekre szüksége van egy későbbi lépésben.</span><span class="sxs-lookup"><span data-stu-id="fadd9-129">You should also record the application ID, the authentication key, and the tenant ID that you need in a later step.</span></span>

### <a name="create-the-authorization-file"></a><span data-ttu-id="fadd9-130">Az engedélyezési fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="fadd9-130">Create the authorization file</span></span>

1. <span data-ttu-id="fadd9-131">A Megoldáskezelőben kattintson a jobb gombbal *myDotnetProject* > **Hozzáadás** > **új elem**, majd válassza ki **szövegfájl** a *Visual C# elemek*.</span><span class="sxs-lookup"><span data-stu-id="fadd9-131">In Solution Explorer, right-click *myDotnetProject* > **Add** > **New Item**, and then select **Text File** in *Visual C# Items*.</span></span> <span data-ttu-id="fadd9-132">A fájl neve *azureauth.properties*, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="fadd9-132">Name the file *azureauth.properties*, and then click **Add**.</span></span>
2. <span data-ttu-id="fadd9-133">Adja hozzá a engedélyezési tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="fadd9-133">Add these authorization properties:</span></span>

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

    <span data-ttu-id="fadd9-134">Cserélje le  **&lt;előfizetés-azonosító&gt;**  az előfizetés-azonosítóval rendelkező  **&lt;alkalmazásazonosító&gt;**  való a Active Directory-azonosítót,  **&lt;hitelesítési kulcs&gt;**  az alkalmazás kulccsal és  **&lt;bérlőazonosító&gt;**  a bérlő azonosítója.</span><span class="sxs-lookup"><span data-stu-id="fadd9-134">Replace **&lt;subscription-id&gt;** with your subscription identifier, **&lt;application-id&gt;** with the Active Directory application identifier, **&lt;authentication-key&gt;** with the application key, and **&lt;tenant-id&gt;** with the tenant identifier.</span></span>

3. <span data-ttu-id="fadd9-135">Mentse a azureauth.properties fájlt.</span><span class="sxs-lookup"><span data-stu-id="fadd9-135">Save the azureauth.properties file.</span></span> 
4. <span data-ttu-id="fadd9-136">A létrehozott engedélyezési fájl teljes elérési útja AZURE_AUTH_LOCATION nevű Windows környezeti változó értéke.</span><span class="sxs-lookup"><span data-stu-id="fadd9-136">Set an environment variable in Windows named AZURE_AUTH_LOCATION with the full path to authorization file that you created.</span></span> <span data-ttu-id="fadd9-137">Például a következő PowerShell-parancs használható:</span><span class="sxs-lookup"><span data-stu-id="fadd9-137">For example, the following PowerShell command can be used:</span></span>

    ```
    [Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\Visual Studio 2017\Projects\myDotnetProject\myDotnetProject\azureauth.properties", "User")
    ```

### <a name="create-the-management-client"></a><span data-ttu-id="fadd9-138">A felügyeleti ügyfél létrehozása</span><span class="sxs-lookup"><span data-stu-id="fadd9-138">Create the management client</span></span>

1. <span data-ttu-id="fadd9-139">Nyissa meg a Program.cs fájlra a projekthez létrehozott, és adja hozzá ezeket a fájl felső utasítást, hogy a meglévő utasítás használatával:</span><span class="sxs-lookup"><span data-stu-id="fadd9-139">Open the Program.cs file for the project that you created, and then add these using statements to the existing statements at top of the file:</span></span>

    ```
    using Microsoft.Azure.Management.Compute.Fluent;
    using Microsoft.Azure.Management.Compute.Fluent.Models;
    using Microsoft.Azure.Management.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
    ```

2. <span data-ttu-id="fadd9-140">Hozzon létre a felügyeleti ügyfél, vegye fel ezt a kódot a fő metódus:</span><span class="sxs-lookup"><span data-stu-id="fadd9-140">To create the management client, add this code to the Main method:</span></span>

    ```
    var credentials = SdkContext.AzureCredentialsFactory
        .FromFile(Environment.GetEnvironmentVariable("AZURE_AUTH_LOCATION"));

    var azure = Azure
        .Configure()
        .WithLogLevel(HttpLoggingDelegatingHandler.Level.Basic)
        .Authenticate(credentials)
        .WithDefaultSubscription();
    ```

## <a name="create-resources"></a><span data-ttu-id="fadd9-141">Erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="fadd9-141">Create resources</span></span>

### <a name="create-the-resource-group"></a><span data-ttu-id="fadd9-142">Az erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="fadd9-142">Create the resource group</span></span>

<span data-ttu-id="fadd9-143">Minden erőforrás tartalmaznia kell egy [erőforráscsoport](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fadd9-143">All resources must be contained in a [Resource group](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="fadd9-144">Adja meg az alkalmazás az értékét, és az erőforráscsoport létrehozásához adja hozzá ezt a kódot a fő metódus:</span><span class="sxs-lookup"><span data-stu-id="fadd9-144">To specify values for the application and create the resource group, add this code to the Main method:</span></span>

```
var groupName = "myResourceGroup";
var vmName = "myVM";
var location = Region.USWest;
    
Console.WriteLine("Creating resource group...");
var resourceGroup = azure.ResourceGroups.Define(groupName)
    .WithRegion(location)
    .Create();
```

### <a name="create-the-availability-set"></a><span data-ttu-id="fadd9-145">A rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="fadd9-145">Create the availability set</span></span>

<span data-ttu-id="fadd9-146">[Rendelkezésre állási készletek](tutorial-availability-sets.md) megkönnyíti, hogy a virtuális gépeket, amelyet az alkalmazás karbantartása.</span><span class="sxs-lookup"><span data-stu-id="fadd9-146">[Availability sets](tutorial-availability-sets.md) make it easier for you to maintain the virtual machines used by your application.</span></span>

<span data-ttu-id="fadd9-147">A rendelkezésre állási csoport létrehozása, vegye fel ezt a kódot a fő metódus:</span><span class="sxs-lookup"><span data-stu-id="fadd9-147">To create the availability set, add this code to the Main method:</span></span>

```
Console.WriteLine("Creating availability set...");
var availabilitySet = azure.AvailabilitySets.Define("myAVSet")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithSku(AvailabilitySetSkuTypes.Managed)
    .Create();
```

### <a name="create-the-public-ip-address"></a><span data-ttu-id="fadd9-148">A nyilvános IP-cím létrehozása</span><span class="sxs-lookup"><span data-stu-id="fadd9-148">Create the public IP address</span></span>

<span data-ttu-id="fadd9-149">A [nyilvános IP-cím](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) kommunikálni a virtuális gép van szükség.</span><span class="sxs-lookup"><span data-stu-id="fadd9-149">A [Public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) is needed to communicate with the virtual machine.</span></span>

<span data-ttu-id="fadd9-150">A nyilvános IP-cím a virtuális gép létrehozása, vegye fel ezt a kódot a fő metódus:</span><span class="sxs-lookup"><span data-stu-id="fadd9-150">To create the public IP address for the virtual machine, add this code to the Main method:</span></span>
   
```
Console.WriteLine("Creating public IP address...");
var publicIPAddress = azure.PublicIPAddresses.Define("myPublicIP")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithDynamicIP()
    .Create();
```

### <a name="create-the-virtual-network"></a><span data-ttu-id="fadd9-151">A virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="fadd9-151">Create the virtual network</span></span>

<span data-ttu-id="fadd9-152">Az alhálózat szerepelnie kell egy virtuális gépet egy [virtuális hálózati](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fadd9-152">A virtual machine must be in a subnet of a [Virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

<span data-ttu-id="fadd9-153">Hozzon létre egy alhálózatot és egy virtuális hálózatot, vegye fel ezt a kódot a fő metódus:</span><span class="sxs-lookup"><span data-stu-id="fadd9-153">To create a subnet and a virtual network, add this code to the Main method:</span></span>

```
Console.WriteLine("Creating virtual network...");
var network = azure.Networks.Define("myVNet")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithAddressSpace("10.0.0.0/16")
    .WithSubnet("mySubnet", "10.0.0.0/24")
    .Create();
```

### <a name="create-the-network-interface"></a><span data-ttu-id="fadd9-154">A hálózati illesztő létrehozása</span><span class="sxs-lookup"><span data-stu-id="fadd9-154">Create the network interface</span></span>

<span data-ttu-id="fadd9-155">A virtuális gépek kell a hálózati adaptert a virtuális hálózaton való kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="fadd9-155">A virtual machine needs a network interface to communicate on the virtual network.</span></span>

<span data-ttu-id="fadd9-156">Hozzon létre egy hálózati adapter, vegye fel ezt a kódot a fő metódus:</span><span class="sxs-lookup"><span data-stu-id="fadd9-156">To create a network interface, add this code to the Main method:</span></span>

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

### <a name="create-the-virtual-machine"></a><span data-ttu-id="fadd9-157">A virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="fadd9-157">Create the virtual machine</span></span>

<span data-ttu-id="fadd9-158">Most, hogy létrehozta a támogató erőforrásokat, létrehozhat egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="fadd9-158">Now that you created all the supporting resources, you can create a virtual machine.</span></span>

<span data-ttu-id="fadd9-159">A virtuális gép létrehozásához, vegye fel ezt a kódot a fő metódus:</span><span class="sxs-lookup"><span data-stu-id="fadd9-159">To create the virtual machine, add this code to the Main method:</span></span>

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
> <span data-ttu-id="fadd9-160">Ebben az oktatóanyagban létrehoz egy virtuális gépet, a Windows Server operációs rendszer verziója.</span><span class="sxs-lookup"><span data-stu-id="fadd9-160">This tutorial creates a virtual machine running a version of the Windows Server operating system.</span></span> <span data-ttu-id="fadd9-161">Más rendszerképek kiválasztásáról kapcsolatos további információkért lásd: [keresse meg és válassza ki azokat a Windows PowerShell és az Azure CLI Azure virtuális gép lemezképeket](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="fadd9-161">To learn more about selecting other images, see [Navigate and select Azure virtual machine images with Windows PowerShell and the Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
>

<span data-ttu-id="fadd9-162">Ha szeretné használni a meglévő lemez Piactéri rendszerkép helyett, használja ezt a kódot:</span><span class="sxs-lookup"><span data-stu-id="fadd9-162">If you want to use an existing disk instead of a marketplace image, use this code:</span></span>

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

## <a name="perform-management-tasks"></a><span data-ttu-id="fadd9-163">Felügyeleti feladatok végrehajtása</span><span class="sxs-lookup"><span data-stu-id="fadd9-163">Perform management tasks</span></span>

<span data-ttu-id="fadd9-164">A virtuális gépek életciklusa folyamán érdemes lehet indítása, leállítása vagy törlése a virtuális gépek például a felügyeleti feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="fadd9-164">During the lifecycle of a virtual machine, you may want to run management tasks such as starting, stopping, or deleting a virtual machine.</span></span> <span data-ttu-id="fadd9-165">Emellett érdemes bonyolult vagy ismétlődő feladatok automatizálásához kódot létrehozni.</span><span class="sxs-lookup"><span data-stu-id="fadd9-165">Additionally, you may want to create code to automate repetitive or complex tasks.</span></span>

<span data-ttu-id="fadd9-166">Amikor kell tennie semmit a virtuális gép, egy példányát kell:</span><span class="sxs-lookup"><span data-stu-id="fadd9-166">When you need to do anything with the VM, you need to get an instance of it:</span></span>

```
var vm = azure.VirtualMachines.GetByResourceGroup(groupName, vmName);
```

### <a name="get-information-about-the-vm"></a><span data-ttu-id="fadd9-167">A virtuális gép adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="fadd9-167">Get information about the VM</span></span>

<span data-ttu-id="fadd9-168">Ahhoz, hogy a virtuális gép adatait, a fő metódus adja hozzá ezt a kódot:</span><span class="sxs-lookup"><span data-stu-id="fadd9-168">To get information about the virtual machine, add this code to the Main method:</span></span>

```
Console.WriteLine("Getting information about the virtual machine...");
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
Console.WriteLine("Press enter to continue...");
Console.ReadLine();
```

### <a name="stop-the-vm"></a><span data-ttu-id="fadd9-169">A virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="fadd9-169">Stop the VM</span></span>

<span data-ttu-id="fadd9-170">Állítsa le a virtuális gépet és a beállítások megőrzése, de továbbra is azt számlázni, vagy állítsa le a virtuális gépet, és azt felszabadítani.</span><span class="sxs-lookup"><span data-stu-id="fadd9-170">You can stop a virtual machine and keep all its settings, but continue to be charged for it, or you can stop a virtual machine and deallocate it.</span></span> <span data-ttu-id="fadd9-171">Ha egy virtuális gép fel van szabadítva, vele társított összes erőforrás is felszabadított és számlázási végpontjainak.</span><span class="sxs-lookup"><span data-stu-id="fadd9-171">When a virtual machine is deallocated, all resources associated with it are also deallocated and billing ends for it.</span></span>

<span data-ttu-id="fadd9-172">A virtuális gép leállítása nélkül felszabadítása azt, vegye fel ezt a kódot a fő metódus:</span><span class="sxs-lookup"><span data-stu-id="fadd9-172">To stop the virtual machine without deallocating it, add this code to the Main method:</span></span>

```
Console.WriteLine("Stopping vm...");
vm.PowerOff();
Console.WriteLine("Press enter to continue...");
Console.ReadLine();
```

<span data-ttu-id="fadd9-173">Ha a virtuális gép felszabadítása, módosítsa ezt a kódot kikapcsolt hívása:</span><span class="sxs-lookup"><span data-stu-id="fadd9-173">If you want to deallocate the virtual machine, change the PowerOff call to this code:</span></span>

```
vm.Deallocate();
```

### <a name="start-the-vm"></a><span data-ttu-id="fadd9-174">Indítsa el a virtuális Gépet</span><span class="sxs-lookup"><span data-stu-id="fadd9-174">Start the VM</span></span>

<span data-ttu-id="fadd9-175">Indítsa el a virtuális gépet, vegye fel ezt a kódot a fő metódus:</span><span class="sxs-lookup"><span data-stu-id="fadd9-175">To start the virtual machine, add this code to the Main method:</span></span>

```
Console.WriteLine("Starting vm...");
vm.Start();
Console.WriteLine("Press enter to continue...");
Console.ReadLine();
```

### <a name="resize-the-vm"></a><span data-ttu-id="fadd9-176">A virtuális gép átméretezésével</span><span class="sxs-lookup"><span data-stu-id="fadd9-176">Resize the VM</span></span>

<span data-ttu-id="fadd9-177">Telepítési sok szempontját figyelembe kell venni, amikor eldönti, a virtuális gép méretét.</span><span class="sxs-lookup"><span data-stu-id="fadd9-177">Many aspects of deployment should be considered when deciding on a size for your virtual machine.</span></span> <span data-ttu-id="fadd9-178">További információkért lásd: [Virtuálisgép-méretek](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="fadd9-178">For more information, see [VM sizes](sizes.md).</span></span>  

<span data-ttu-id="fadd9-179">Ha módosítani szeretné a virtuális gép méretét, adja hozzá ezt a kódot a fő metódus:</span><span class="sxs-lookup"><span data-stu-id="fadd9-179">To change size of the virtual machine, add this code to the Main method:</span></span>

```
Console.WriteLine("Resizing vm...");
vm.Update()
    .WithSize(VirtualMachineSizeTypes.StandardDS2) 
    .Apply();
Console.WriteLine("Press enter to continue...");
Console.ReadLine();
```

### <a name="add-a-data-disk-to-the-vm"></a><span data-ttu-id="fadd9-180">Adatlemez hozzáadása a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="fadd9-180">Add a data disk to the VM</span></span>

<span data-ttu-id="fadd9-181">Adatlemez hozzáadása a virtuális gépet, adja hozzá ezt a kódot a fő metódus, amely 2 GB-nál, 0 és a gyorsítótárazási ReadWrite a logikai egység han adatlemez hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="fadd9-181">To add a data disk to the virtual machine, add this code to the Main method to add a data disk that is 2 GB in size, han a LUN of 0 and a caching type of ReadWrite:</span></span>

```
Console.WriteLine("Adding data disk to vm...");
vm.Update()
    .WithNewDataDisk(2, 0, CachingTypes.ReadWrite) 
    .Apply();
Console.WriteLine("Press enter to delete resources...");
Console.ReadLine();
```

## <a name="delete-resources"></a><span data-ttu-id="fadd9-182">Erőforrások törlése</span><span class="sxs-lookup"><span data-stu-id="fadd9-182">Delete resources</span></span>

<span data-ttu-id="fadd9-183">Mivel az Azure-ban használt erőforrásokhoz van szó, ajánlott mindig törli az erőforrást, amely már nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="fadd9-183">Because you are charged for resources used in Azure, it is always good practice to delete resources that are no longer needed.</span></span> <span data-ttu-id="fadd9-184">Ha törölni szeretné a virtuális gépek és a támogató erőforrásokat, meg kell nyitnia csak törölje a csoportot.</span><span class="sxs-lookup"><span data-stu-id="fadd9-184">If you want to delete the virtual machines and all the supporting resources, all you have to do is delete the resource group.</span></span>

<span data-ttu-id="fadd9-185">Törölje a csoportot, vegye fel ezt a kódot a fő metódus:</span><span class="sxs-lookup"><span data-stu-id="fadd9-185">To delete the resource group, add this code to the Main method:</span></span>

```
azure.ResourceGroups.DeleteByName(groupName);
```

## <a name="run-the-application"></a><span data-ttu-id="fadd9-186">Az alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="fadd9-186">Run the application</span></span>

<span data-ttu-id="fadd9-187">Öt perc a konzol alkalmazás teljesen futtatásához indítás kell vennie a befejezéshez.</span><span class="sxs-lookup"><span data-stu-id="fadd9-187">It should take about five minutes for this console application to run completely from start to finish.</span></span> 

1. <span data-ttu-id="fadd9-188">Futtassa a konzolalkalmazást, kattintson a **Start**.</span><span class="sxs-lookup"><span data-stu-id="fadd9-188">To run the console application, click **Start**.</span></span>

2. <span data-ttu-id="fadd9-189">Ahhoz, hogy nyomja le az ENTER **Enter** erőforrások törlése elindításához eltarthat néhány percig az Azure-portálon az erőforrások létrehozásának ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="fadd9-189">Before you press **Enter** to start deleting resources, you could take a few minutes to verify the creation of the resources in the Azure portal.</span></span> <span data-ttu-id="fadd9-190">Kattintson a telepítés állapota a telepítéssel kapcsolatos információk megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="fadd9-190">Click the deployment status to see information about the deployment.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fadd9-191">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fadd9-191">Next steps</span></span>
* <span data-ttu-id="fadd9-192">Sablon használatával hozhat létre egy virtuális gép található információk segítségével előnyeit [központi telepítése egy Azure virtuális gépen a C# és a Resource Manager-sablon használatával](csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="fadd9-192">Take advantage of using a template to create a virtual machine by using the information in [Deploy an Azure Virtual Machine using C# and a Resource Manager template](csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="fadd9-193">További információ a [Azure-könyvtárakban .NET](https://docs.microsoft.com/dotnet/azure/?view=azure-dotnet).</span><span class="sxs-lookup"><span data-stu-id="fadd9-193">Learn more about using the [Azure libraries for .NET](https://docs.microsoft.com/dotnet/azure/?view=azure-dotnet).</span></span>

