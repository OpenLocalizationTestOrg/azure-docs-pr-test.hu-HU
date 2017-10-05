---
title: "Létrehozása és kezelése az Azure virtuális gép Java használatával |} Microsoft Docs"
description: "Java és az Azure Resource Manager segítségével telepítheti a virtuális gép és annak támogató erőforrásokat."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: davidmu
ms.openlocfilehash: b9e739a07c5863577285fb3a221b372b385c6762
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-manage-windows-vms-in-azure-using-java"></a><span data-ttu-id="a324a-103">Létrehozása és kezelése Windows-alapú virtuális gépek az Azure-ban Java</span><span class="sxs-lookup"><span data-stu-id="a324a-103">Create and manage Windows VMs in Azure using Java</span></span>

<span data-ttu-id="a324a-104">Egy [Azure virtuális gép](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) több támogató Azure-erőforrások kell.</span><span class="sxs-lookup"><span data-stu-id="a324a-104">An [Azure Virtual Machine](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) needs several supporting Azure resources.</span></span> <span data-ttu-id="a324a-105">Ez a cikk ismerteti a létrehozása, kezelése és a Java Virtuálisgép-erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="a324a-105">This article covers creating, managing, and deleting VM resources using Java.</span></span> <span data-ttu-id="a324a-106">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="a324a-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a324a-107">Maven-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="a324a-107">Create a Maven project</span></span>
> * <span data-ttu-id="a324a-108">Adja hozzá a függőségek</span><span class="sxs-lookup"><span data-stu-id="a324a-108">Add dependencies</span></span>
> * <span data-ttu-id="a324a-109">Hitelesítő adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="a324a-109">Create credentials</span></span>
> * <span data-ttu-id="a324a-110">Erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="a324a-110">Create resources</span></span>
> * <span data-ttu-id="a324a-111">Felügyeleti feladatok végrehajtása</span><span class="sxs-lookup"><span data-stu-id="a324a-111">Perform management tasks</span></span>
> * <span data-ttu-id="a324a-112">Erőforrások törlése</span><span class="sxs-lookup"><span data-stu-id="a324a-112">Delete resources</span></span>
> * <span data-ttu-id="a324a-113">Az alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="a324a-113">Run the application</span></span>

<span data-ttu-id="a324a-114">A lépések elvégzéséhez körülbelül 20 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="a324a-114">It takes about 20 minutes to do these steps.</span></span>

## <a name="create-a-maven-project"></a><span data-ttu-id="a324a-115">Maven-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="a324a-115">Create a Maven project</span></span>

1. <span data-ttu-id="a324a-116">Ha még nem tette meg, telepítse [Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="a324a-116">If you haven't already done so, install [Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span>
2. <span data-ttu-id="a324a-117">Telepítés [Maven](http://maven.apache.org/download.cgi).</span><span class="sxs-lookup"><span data-stu-id="a324a-117">Install [Maven](http://maven.apache.org/download.cgi).</span></span>
3. <span data-ttu-id="a324a-118">Hozzon létre egy új mappát és a projekthez:</span><span class="sxs-lookup"><span data-stu-id="a324a-118">Create a new folder and the project:</span></span>
    
    ```
    mkdir java-azure-test
    cd java-azure-test
    
    mvn archetype:generate -DgroupId=com.fabrikam -DartifactId=testAzureApp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

## <a name="add-dependencies"></a><span data-ttu-id="a324a-119">Adja hozzá a függőségek</span><span class="sxs-lookup"><span data-stu-id="a324a-119">Add dependencies</span></span>

1. <span data-ttu-id="a324a-120">Az a `testAzureApp` mappa, nyissa meg a `pom.xml` fájlt, és a build konfigurációját, és &lt;projekt&gt; ahhoz, hogy az alkalmazás összeállítása:</span><span class="sxs-lookup"><span data-stu-id="a324a-120">Under the `testAzureApp` folder, open the `pom.xml` file and add the build configuration to &lt;project&gt; to enable the building of your application:</span></span>

    ```xml
    <build>
      <plugins>
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <configuration>
                <mainClass>com.fabrikam.testAzureApp.App</mainClass>
            </configuration>
        </plugin>
      </plugins>
    </build>
    ```

2. <span data-ttu-id="a324a-121">Adja a függőségeket, az Azure Java SDK eléréséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="a324a-121">Add the dependencies that are needed to access the Azure Java SDK.</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure</artifactId>
      <version>1.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-mgmt-compute</artifactId>
      <version>1.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-mgmt-resources</artifactId>
      <version>1.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-mgmt-network</artifactId>
      <version>1.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.squareup.okio</groupId>
      <artifactId>okio</artifactId>
      <version>1.13.0</version>
    </dependency>
    <dependency> 
      <groupId>com.nimbusds</groupId>
      <artifactId>nimbus-jose-jwt</artifactId>
      <version>3.6</version>
    </dependency>
    <dependency>
      <groupId>net.minidev</groupId>
      <artifactId>json-smart</artifactId>
      <version>1.0.6.3</version>
    </dependency>
    <dependency>
      <groupId>javax.mail</groupId>
      <artifactId>mail</artifactId>
      <version>1.4.5</version>
    </dependency>
    ```

3. <span data-ttu-id="a324a-122">Mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="a324a-122">Save the file.</span></span>

## <a name="create-credentials"></a><span data-ttu-id="a324a-123">Hitelesítő adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="a324a-123">Create credentials</span></span>

<span data-ttu-id="a324a-124">Ez a lépés megkezdése előtt győződjön meg arról, hogy rendelkezik-e a hozzáférést egy [Active Directory szolgáltatás egyszerű](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a324a-124">Before you start this step, make sure that you have access to an [Active Directory service principal](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="a324a-125">Is rögzíteni kell az alkalmazás Azonosítóját, a hitelesítési kulcs és a bérlő azonosítója, amelyekre szüksége van egy későbbi lépésben.</span><span class="sxs-lookup"><span data-stu-id="a324a-125">You should also record the application ID, the authentication key, and the tenant ID that you need in a later step.</span></span>

### <a name="create-the-authorization-file"></a><span data-ttu-id="a324a-126">Az engedélyezési fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="a324a-126">Create the authorization file</span></span>

1. <span data-ttu-id="a324a-127">Hozzon létre egy fájlt `azureauth.properties` , és ezek a tulajdonságok felvétele:</span><span class="sxs-lookup"><span data-stu-id="a324a-127">Create a file named `azureauth.properties` and add these properties to it:</span></span>

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

    <span data-ttu-id="a324a-128">Cserélje le  **&lt;előfizetés-azonosító&gt;**  az előfizetés-azonosítóval rendelkező  **&lt;alkalmazásazonosító&gt;**  való a Active Directory-azonosítót,  **&lt;hitelesítési kulcs&gt;**  az alkalmazás kulccsal és  **&lt;bérlőazonosító&gt;**  a bérlő azonosítója.</span><span class="sxs-lookup"><span data-stu-id="a324a-128">Replace **&lt;subscription-id&gt;** with your subscription identifier, **&lt;application-id&gt;** with the Active Directory application identifier, **&lt;authentication-key&gt;** with the application key, and **&lt;tenant-id&gt;** with the tenant identifier.</span></span>

2. <span data-ttu-id="a324a-129">Mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="a324a-129">Save the file.</span></span>
3. <span data-ttu-id="a324a-130">A fájl teljes elérési útját a hitelesítés a rendszerhéj AZURE_AUTH_LOCATION nevű környezeti változó értéke.</span><span class="sxs-lookup"><span data-stu-id="a324a-130">Set an environment variable named AZURE_AUTH_LOCATION in your shell with the full path to the authentication file.</span></span>

### <a name="create-the-management-client"></a><span data-ttu-id="a324a-131">A felügyeleti ügyfél létrehozása</span><span class="sxs-lookup"><span data-stu-id="a324a-131">Create the management client</span></span>

1. <span data-ttu-id="a324a-132">Nyissa meg a `App.java` a fájl `src\main\java\com\fabrikam` , és győződjön meg arról, hogy a csomag utasítás szerepel a szabálylista:</span><span class="sxs-lookup"><span data-stu-id="a324a-132">Open the `App.java` file under `src\main\java\com\fabrikam` and make sure this package statement is at the top:</span></span>

    ```java
    package com.fabrikam.testAzureApp;
    ```

2. <span data-ttu-id="a324a-133">A csomag utasítás alatt adja hozzá ezek kimutatások importálása:</span><span class="sxs-lookup"><span data-stu-id="a324a-133">Under the package statement, add these import statements:</span></span>
   
    ```java
    import com.microsoft.azure.management.Azure;
    import com.microsoft.azure.management.compute.AvailabilitySet;
    import com.microsoft.azure.management.compute.AvailabilitySetSkuTypes;
    import com.microsoft.azure.management.compute.CachingTypes;
    import com.microsoft.azure.management.compute.InstanceViewStatus;
    import com.microsoft.azure.management.compute.DiskInstanceView;
    import com.microsoft.azure.management.compute.VirtualMachine;
    import com.microsoft.azure.management.compute.VirtualMachineSizeTypes;
    import com.microsoft.azure.management.network.PublicIPAddress;
    import com.microsoft.azure.management.network.Network;
    import com.microsoft.azure.management.network.NetworkInterface;
    import com.microsoft.azure.management.resources.ResourceGroup;
    import com.microsoft.azure.management.resources.fluentcore.arm.Region;
    import com.microsoft.azure.management.resources.fluentcore.model.Creatable;
    import com.microsoft.rest.LogLevel;
    import java.io.File;
    import java.util.Scanner;
    ```

2. <span data-ttu-id="a324a-134">Az Active Directory hitelesítő adatait hozhatják létre, meg kell győződnie kérelmeket, adja hozzá ezt a kódot a fő metódus App osztály:</span><span class="sxs-lookup"><span data-stu-id="a324a-134">To create the Active Directory credentials that you need to make requests, add this code to the main method of the App class:</span></span>
   
    ```java
    try {    
        final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
        Azure azure = Azure.configure()
            .withLogLevel(LogLevel.BASIC)
            .authenticate(credFile)
            .withDefaultSubscription();
    } catch (Exception e) {
        System.out.println(e.getMessage());
        e.printStackTrace();
    }

    ```

## <a name="create-resources"></a><span data-ttu-id="a324a-135">Erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="a324a-135">Create resources</span></span>

### <a name="create-the-resource-group"></a><span data-ttu-id="a324a-136">Az erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="a324a-136">Create the resource group</span></span>

<span data-ttu-id="a324a-137">Minden erőforrás tartalmaznia kell egy [erőforráscsoport](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a324a-137">All resources must be contained in a [Resource group](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="a324a-138">Adja meg az alkalmazás az értékét, és az erőforráscsoport létrehozása, vegye fel ezt a kódot a try blokk fő metódus:</span><span class="sxs-lookup"><span data-stu-id="a324a-138">To specify values for the application and create the resource group, add this code to the try block in the main method:</span></span>

```java
System.out.println("Creating resource group...");
ResourceGroup resourceGroup = azure.resourceGroups()
    .define("myResourceGroup")
    .withRegion(Region.US_EAST)
    .create();
```

### <a name="create-the-availability-set"></a><span data-ttu-id="a324a-139">A rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="a324a-139">Create the availability set</span></span>

<span data-ttu-id="a324a-140">[Rendelkezésre állási készletek](tutorial-availability-sets.md) megkönnyíti, hogy a virtuális gépeket, amelyet az alkalmazás karbantartása.</span><span class="sxs-lookup"><span data-stu-id="a324a-140">[Availability sets](tutorial-availability-sets.md) make it easier for you to maintain the virtual machines used by your application.</span></span>

<span data-ttu-id="a324a-141">A rendelkezésre állási csoport létrehozása, vegye fel ezt a kódot a try blokk a fő metódusban:</span><span class="sxs-lookup"><span data-stu-id="a324a-141">To create the availability set, add this code to the try block in the main method:</span></span>

```java
System.out.println("Creating availability set...");
AvailabilitySet availabilitySet = azure.availabilitySets()
    .define("myAvailabilitySet")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withSku(AvailabilitySetSkuTypes.MANAGED)
    .create();
```
### <a name="create-the-public-ip-address"></a><span data-ttu-id="a324a-142">A nyilvános IP-cím létrehozása</span><span class="sxs-lookup"><span data-stu-id="a324a-142">Create the public IP address</span></span>

<span data-ttu-id="a324a-143">A [nyilvános IP-cím](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) kommunikálni a virtuális gép van szükség.</span><span class="sxs-lookup"><span data-stu-id="a324a-143">A [Public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) is needed to communicate with the virtual machine.</span></span>

<span data-ttu-id="a324a-144">A nyilvános IP-cím a virtuális gép létrehozása, vegye fel ezt a kódot a try blokk a fő metódusban:</span><span class="sxs-lookup"><span data-stu-id="a324a-144">To create the public IP address for the virtual machine, add this code to the try block in the main method:</span></span>

```java
System.out.println("Creating public IP address...");
PublicIPAddress publicIPAddress = azure.publicIPAddresses()
    .define("myPublicIP")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withDynamicIP()
    .create();
```

### <a name="create-the-virtual-network"></a><span data-ttu-id="a324a-145">A virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="a324a-145">Create the virtual network</span></span>

<span data-ttu-id="a324a-146">Az alhálózat szerepelnie kell egy virtuális gépet egy [virtuális hálózati](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a324a-146">A virtual machine must be in a subnet of a [Virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

<span data-ttu-id="a324a-147">Hozzon létre egy alhálózatot és egy virtuális hálózatot, vegye fel ezt a kódot a try blokk a fő metódusban:</span><span class="sxs-lookup"><span data-stu-id="a324a-147">To create a subnet and a virtual network, add this code to the try block in the main method:</span></span>

```java
System.out.println("Creating virtual network...");
Network network = azure.networks()
    .define("myVN")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withAddressSpace("10.0.0.0/16")
    .withSubnet("mySubnet","10.0.0.0/24")
    .create();
```

### <a name="create-the-network-interface"></a><span data-ttu-id="a324a-148">A hálózati illesztő létrehozása</span><span class="sxs-lookup"><span data-stu-id="a324a-148">Create the network interface</span></span>

<span data-ttu-id="a324a-149">A virtuális gépek kell a hálózati adaptert a virtuális hálózaton való kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="a324a-149">A virtual machine needs a network interface to communicate on the virtual network.</span></span>

<span data-ttu-id="a324a-150">Hozzon létre egy hálózati adapter, vegye fel ezt a kódot a try blokk a fő metódusban:</span><span class="sxs-lookup"><span data-stu-id="a324a-150">To create a network interface, add this code to the try block in the main method:</span></span>

```java
System.out.println("Creating network interface...");
NetworkInterface networkInterface = azure.networkInterfaces()
    .define("myNIC")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withExistingPrimaryNetwork(network)
    .withSubnet("mySubnet")
    .withPrimaryPrivateIPAddressDynamic()
    .withExistingPrimaryPublicIPAddress(publicIPAddress)
    .create();
```

### <a name="create-the-virtual-machine"></a><span data-ttu-id="a324a-151">A virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="a324a-151">Create the virtual machine</span></span>

<span data-ttu-id="a324a-152">Most, hogy létrehozta a támogató erőforrásokat, létrehozhat egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="a324a-152">Now that you created all the supporting resources, you can create a virtual machine.</span></span>

<span data-ttu-id="a324a-153">A virtuális gép létrehozásához, vegye fel ezt a kódot a try blokk a fő metódusban:</span><span class="sxs-lookup"><span data-stu-id="a324a-153">To create the virtual machine, add this code to the try block in the main method:</span></span>

```java
System.out.println("Creating virtual machine...");
VirtualMachine virtualMachine = azure.virtualMachines()
    .define("myVM")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withExistingPrimaryNetworkInterface(networkInterface)
    .withLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .withAdminUsername("azureuser")
    .withAdminPassword("Azure12345678")
    .withComputerName("myVM")
    .withExistingAvailabilitySet(availabilitySet)
    .withSize("Standard_DS1")
    .create();
Scanner input = new Scanner(System.in);
System.out.println("Press enter to get information about the VM...");
input.nextLine();
```

> [!NOTE]
> <span data-ttu-id="a324a-154">Ebben az oktatóanyagban létrehoz egy virtuális gépet, a Windows Server operációs rendszer verziója.</span><span class="sxs-lookup"><span data-stu-id="a324a-154">This tutorial creates a virtual machine running a version of the Windows Server operating system.</span></span> <span data-ttu-id="a324a-155">Más rendszerképek kiválasztásáról kapcsolatos további információkért lásd: [keresse meg és válassza ki azokat a Windows PowerShell és az Azure CLI Azure virtuális gép lemezképeket](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a324a-155">To learn more about selecting other images, see [Navigate and select Azure virtual machine images with Windows PowerShell and the Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
>

<span data-ttu-id="a324a-156">Ha szeretné használni a meglévő lemez Piactéri rendszerkép helyett, használja ezt a kódot:</span><span class="sxs-lookup"><span data-stu-id="a324a-156">If you want to use an existing disk instead of a marketplace image, use this code:</span></span> 

```java
ManagedDisk managedDisk = azure.disks.define("myosdisk") 
    .withRegion(Region.US_EAST) 
    .withExistingResourceGroup("myResourceGroup") 
    .withWindowsFromVhd("https://mystorage.blob.core.windows.net/vhds/myosdisk.vhd") 
    .withSizeInGB(128) 
    .withSku(DiskSkuTypes.PremiumLRS) 
    .create(); 

azure.virtualMachines.define("myVM") 
    .withRegion(Region.US_EAST) 
    .withExistingResourceGroup("myResourceGroup") 
    .withExistingPrimaryNetworkInterface(networkInterface) 
    .withSpecializedOSDisk(managedDisk, OperatingSystemTypes.Windows) 
    .withExistingAvailabilitySet(availabilitySet) 
    .withSize(VirtualMachineSizeTypes.StandardDS1) 
    .create(); 
``` 

## <a name="perform-management-tasks"></a><span data-ttu-id="a324a-157">Felügyeleti feladatok végrehajtása</span><span class="sxs-lookup"><span data-stu-id="a324a-157">Perform management tasks</span></span>

<span data-ttu-id="a324a-158">A virtuális gépek életciklusa folyamán érdemes lehet indítása, leállítása vagy törlése a virtuális gépek például a felügyeleti feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="a324a-158">During the lifecycle of a virtual machine, you may want to run management tasks such as starting, stopping, or deleting a virtual machine.</span></span> <span data-ttu-id="a324a-159">Emellett érdemes bonyolult vagy ismétlődő feladatok automatizálásához kódot létrehozni.</span><span class="sxs-lookup"><span data-stu-id="a324a-159">Additionally, you may want to create code to automate repetitive or complex tasks.</span></span>

<span data-ttu-id="a324a-160">Ha semmit a a virtuális Géphez van szüksége, kell egy példányát.</span><span class="sxs-lookup"><span data-stu-id="a324a-160">When you need to do anything with the VM, you need to get an instance of it.</span></span> <span data-ttu-id="a324a-161">Ez a kód hozzáadása a try blokk a fő metódus:</span><span class="sxs-lookup"><span data-stu-id="a324a-161">Add this code to the try block of the main method:</span></span>

```java
VirtualMachine vm = azure.virtualMachines().getByResourceGroup("myResourceGroup", "myVM");
```

### <a name="get-information-about-the-vm"></a><span data-ttu-id="a324a-162">A virtuális gép adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="a324a-162">Get information about the VM</span></span>

<span data-ttu-id="a324a-163">A virtuális géppel kapcsolatos információk beszerzéséhez vegye fel ezt a kódot a try blokk fő metódus:</span><span class="sxs-lookup"><span data-stu-id="a324a-163">To get information about the virtual machine, add this code to the try block in the main method:</span></span>

```java
System.out.println("hardwareProfile");
System.out.println("    vmSize: " + vm.size());
System.out.println("storageProfile");
System.out.println("  imageReference");
System.out.println("    publisher: " + vm.storageProfile().imageReference().publisher());
System.out.println("    offer: " + vm.storageProfile().imageReference().offer());
System.out.println("    sku: " + vm.storageProfile().imageReference().sku());
System.out.println("    version: " + vm.storageProfile().imageReference().version());
System.out.println("  osDisk");
System.out.println("    osType: " + vm.storageProfile().osDisk().osType());
System.out.println("    name: " + vm.storageProfile().osDisk().name());
System.out.println("    createOption: " + vm.storageProfile().osDisk().createOption());
System.out.println("    caching: " + vm.storageProfile().osDisk().caching());
System.out.println("osProfile");
System.out.println("    computerName: " + vm.osProfile().computerName());
System.out.println("    adminUserName: " + vm.osProfile().adminUsername());
System.out.println("    provisionVMAgent: " + vm.osProfile().windowsConfiguration().provisionVMAgent());
System.out.println("    enableAutomaticUpdates: " + vm.osProfile().windowsConfiguration().enableAutomaticUpdates());
System.out.println("networkProfile");
System.out.println("    networkInterface: " + vm.primaryNetworkInterfaceId());
System.out.println("vmAgent");
System.out.println("  vmAgentVersion: " + vm.instanceView().vmAgent().vmAgentVersion());
System.out.println("    statuses");
for(InstanceViewStatus status : vm.instanceView().vmAgent().statuses()) {
    System.out.println("    code: " + status.code());
    System.out.println("    displayStatus: " + status.displayStatus());
    System.out.println("    message: " + status.message());
    System.out.println("    time: " + status.time());
}
System.out.println("disks");
for(DiskInstanceView disk : vm.instanceView().disks()) {
    System.out.println("  name: " + disk.name());
    System.out.println("  statuses");
    for(InstanceViewStatus status : disk.statuses()) {
        System.out.println("    code: " + status.code());
        System.out.println("    displayStatus: " + status.displayStatus());
        System.out.println("    time: " + status.time());
    }
}
System.out.println("VM general status");
System.out.println("  provisioningStatus: " + vm.provisioningState());
System.out.println("  id: " + vm.id());
System.out.println("  name: " + vm.name());
System.out.println("  type: " + vm.type());
System.out.println("VM instance status");
for(InstanceViewStatus status : vm.instanceView().statuses()) {
    System.out.println("  code: " + status.code());
    System.out.println("  displayStatus: " + status.displayStatus());
}
System.out.println("Press enter to continue...");
input.nextLine();   
```

### <a name="stop-the-vm"></a><span data-ttu-id="a324a-164">A virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="a324a-164">Stop the VM</span></span>

<span data-ttu-id="a324a-165">Állítsa le a virtuális gépet és a beállítások megőrzése, de továbbra is azt számlázni, vagy állítsa le a virtuális gépet, és azt felszabadítani.</span><span class="sxs-lookup"><span data-stu-id="a324a-165">You can stop a virtual machine and keep all its settings, but continue to be charged for it, or you can stop a virtual machine and deallocate it.</span></span> <span data-ttu-id="a324a-166">Ha egy virtuális gép fel van szabadítva, vele társított összes erőforrás is felszabadított és számlázási végpontjainak.</span><span class="sxs-lookup"><span data-stu-id="a324a-166">When a virtual machine is deallocated, all resources associated with it are also deallocated and billing ends for it.</span></span>

<span data-ttu-id="a324a-167">A virtuális gép leállítása nélkül felszabadítása azt, vegye fel ezt a kódot a try blokk a fő metódusban:</span><span class="sxs-lookup"><span data-stu-id="a324a-167">To stop the virtual machine without deallocating it, add this code to the try block in the main method:</span></span>

```java
System.out.println("Stopping vm...");
vm.powerOff();
System.out.println("Press enter to continue...");
input.nextLine();
```

<span data-ttu-id="a324a-168">Ha a virtuális gép felszabadítása, módosítsa ezt a kódot kikapcsolt hívása:</span><span class="sxs-lookup"><span data-stu-id="a324a-168">If you want to deallocate the virtual machine, change the PowerOff call to this code:</span></span>

```java
vm.deallocate();
```

### <a name="start-the-vm"></a><span data-ttu-id="a324a-169">Indítsa el a virtuális Gépet</span><span class="sxs-lookup"><span data-stu-id="a324a-169">Start the VM</span></span>

<span data-ttu-id="a324a-170">Indítsa el a virtuális gépet, vegye fel ezt a kódot a try blokk a fő metódusban:</span><span class="sxs-lookup"><span data-stu-id="a324a-170">To start the virtual machine, add this code to the try block in the main method:</span></span>

```java
System.out.println("Starting vm...");
vm.start();
System.out.println("Press enter to continue...");
input.nextLine();
```

### <a name="resize-the-vm"></a><span data-ttu-id="a324a-171">A virtuális gép átméretezésével</span><span class="sxs-lookup"><span data-stu-id="a324a-171">Resize the VM</span></span>

<span data-ttu-id="a324a-172">Telepítési sok szempontját figyelembe kell venni, amikor eldönti, a virtuális gép méretét.</span><span class="sxs-lookup"><span data-stu-id="a324a-172">Many aspects of deployment should be considered when deciding on a size for your virtual machine.</span></span> <span data-ttu-id="a324a-173">További információkért lásd: [Virtuálisgép-méretek](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="a324a-173">For more information, see [VM sizes](sizes.md).</span></span>  

<span data-ttu-id="a324a-174">A virtuális gép Oldalméret módosítása oly módon, vegye fel ezt a kódot a try blokk a fő metódusban:</span><span class="sxs-lookup"><span data-stu-id="a324a-174">To change size of the virtual machine, add this code to the try block in the main method:</span></span>

```java
System.out.println("Resizing vm...");
vm.update()
    .withSize(VirtualMachineSizeTypes.STANDARD_DS2)
    .apply();
System.out.println("Press enter to continue...");
input.nextLine();
```

### <a name="add-a-data-disk-to-the-vm"></a><span data-ttu-id="a324a-175">Adatlemez hozzáadása a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="a324a-175">Add a data disk to the VM</span></span>

<span data-ttu-id="a324a-176">Adatlemez hozzáadása a virtuális gépet, amely pedig 2 GB-nál, 0 és ReadWrite gyorsítótárazási típusú logikai egység tartozik, vegye fel ezt a kódot a try blokk a fő metódusban:</span><span class="sxs-lookup"><span data-stu-id="a324a-176">To add a data disk to the virtual machine that is 2 GB in size, has a LUN of 0, and a caching type of ReadWrite, add this code to the try block in the main method:</span></span>

```java
System.out.println("Adding data disk...");
vm.update()
    .withNewDataDisk(2, 0, CachingTypes.READ_WRITE)
    .apply();
System.out.println("Press enter to delete resources...");
input.nextLine();
```

## <a name="delete-resources"></a><span data-ttu-id="a324a-177">Erőforrások törlése</span><span class="sxs-lookup"><span data-stu-id="a324a-177">Delete resources</span></span>

<span data-ttu-id="a324a-178">Mivel az Azure-ban használt erőforrásokhoz van szó, ajánlott mindig törli az erőforrást, amely már nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="a324a-178">Because you are charged for resources used in Azure, it is always good practice to delete resources that are no longer needed.</span></span> <span data-ttu-id="a324a-179">Ha törölni szeretné a virtuális gépek és a támogató erőforrásokat, meg kell nyitnia csak törölje a csoportot.</span><span class="sxs-lookup"><span data-stu-id="a324a-179">If you want to delete the virtual machines and all the supporting resources, all you have to do is delete the resource group.</span></span>

1. <span data-ttu-id="a324a-180">Törölje a csoportot, vegye fel ezt a kódot a try blokk a fő metódusban:</span><span class="sxs-lookup"><span data-stu-id="a324a-180">To delete the resource group, add this code to the try block in the main method:</span></span>
   
```java
System.out.println("Deleting resources...");
azure.resourceGroups().deleteByName("myResourceGroup");
```

2. <span data-ttu-id="a324a-181">Mentse a App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="a324a-181">Save the App.java file.</span></span>

## <a name="run-the-application"></a><span data-ttu-id="a324a-182">Az alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="a324a-182">Run the application</span></span>

<span data-ttu-id="a324a-183">Öt perc a konzol alkalmazás teljesen futtatásához indítás kell vennie a befejezéshez.</span><span class="sxs-lookup"><span data-stu-id="a324a-183">It should take about five minutes for this console application to run completely from start to finish.</span></span>

1. <span data-ttu-id="a324a-184">Az alkalmazás futtatásához használja a Maven-parancsot:</span><span class="sxs-lookup"><span data-stu-id="a324a-184">To run the application, use this Maven command:</span></span>

    ```
    mvn compile exec:java
    ```

2. <span data-ttu-id="a324a-185">Ahhoz, hogy nyomja le az ENTER **Enter** erőforrások törlése elindításához eltarthat néhány percig az Azure-portálon az erőforrások létrehozásának ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="a324a-185">Before you press **Enter** to start deleting resources, you could take a few minutes to verify the creation of the resources in the Azure portal.</span></span> <span data-ttu-id="a324a-186">Kattintson a telepítés állapota a telepítéssel kapcsolatos információk megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a324a-186">Click the deployment status to see information about the deployment.</span></span>


## <a name="next-steps"></a><span data-ttu-id="a324a-187">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a324a-187">Next steps</span></span>
* <span data-ttu-id="a324a-188">További információ a [Azure Java-könyvtárakban](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-overview).</span><span class="sxs-lookup"><span data-stu-id="a324a-188">Learn more about using the [Azure libraries for Java](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-overview).</span></span>

