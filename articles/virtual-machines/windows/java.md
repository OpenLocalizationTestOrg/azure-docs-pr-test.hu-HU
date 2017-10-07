---
title: "aaaCreate és kezelése az Azure virtuális gép használata Java |} Microsoft Docs"
description: "Java és az Azure Resource Manager toodeploy használja a virtuális gép és annak támogató erőforrásokat."
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
ms.openlocfilehash: 31ac8d59f92ecff887e64906940933dd6fd50815
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-windows-vms-in-azure-using-java"></a><span data-ttu-id="34cbf-103">Létrehozása és kezelése Windows-alapú virtuális gépek az Azure-ban Java</span><span class="sxs-lookup"><span data-stu-id="34cbf-103">Create and manage Windows VMs in Azure using Java</span></span>

<span data-ttu-id="34cbf-104">Egy [Azure virtuális gép](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) több támogató Azure-erőforrások kell.</span><span class="sxs-lookup"><span data-stu-id="34cbf-104">An [Azure Virtual Machine](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) needs several supporting Azure resources.</span></span> <span data-ttu-id="34cbf-105">Ez a cikk ismerteti a létrehozása, kezelése és a Java Virtuálisgép-erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="34cbf-105">This article covers creating, managing, and deleting VM resources using Java.</span></span> <span data-ttu-id="34cbf-106">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="34cbf-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="34cbf-107">Maven-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="34cbf-107">Create a Maven project</span></span>
> * <span data-ttu-id="34cbf-108">Adja hozzá a függőségek</span><span class="sxs-lookup"><span data-stu-id="34cbf-108">Add dependencies</span></span>
> * <span data-ttu-id="34cbf-109">Hitelesítő adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="34cbf-109">Create credentials</span></span>
> * <span data-ttu-id="34cbf-110">Erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="34cbf-110">Create resources</span></span>
> * <span data-ttu-id="34cbf-111">Felügyeleti feladatok végrehajtása</span><span class="sxs-lookup"><span data-stu-id="34cbf-111">Perform management tasks</span></span>
> * <span data-ttu-id="34cbf-112">Erőforrások törlése</span><span class="sxs-lookup"><span data-stu-id="34cbf-112">Delete resources</span></span>
> * <span data-ttu-id="34cbf-113">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="34cbf-113">Run hello application</span></span>

<span data-ttu-id="34cbf-114">Körülbelül 20 percet toodo szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="34cbf-114">It takes about 20 minutes toodo these steps.</span></span>

## <a name="create-a-maven-project"></a><span data-ttu-id="34cbf-115">Maven-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="34cbf-115">Create a Maven project</span></span>

1. <span data-ttu-id="34cbf-116">Ha még nem tette meg, telepítse [Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="34cbf-116">If you haven't already done so, install [Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span>
2. <span data-ttu-id="34cbf-117">Telepítés [Maven](http://maven.apache.org/download.cgi).</span><span class="sxs-lookup"><span data-stu-id="34cbf-117">Install [Maven](http://maven.apache.org/download.cgi).</span></span>
3. <span data-ttu-id="34cbf-118">Hozzon létre egy új mappát és hello projektet:</span><span class="sxs-lookup"><span data-stu-id="34cbf-118">Create a new folder and hello project:</span></span>
    
    ```
    mkdir java-azure-test
    cd java-azure-test
    
    mvn archetype:generate -DgroupId=com.fabrikam -DartifactId=testAzureApp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

## <a name="add-dependencies"></a><span data-ttu-id="34cbf-119">Adja hozzá a függőségek</span><span class="sxs-lookup"><span data-stu-id="34cbf-119">Add dependencies</span></span>

1. <span data-ttu-id="34cbf-120">A hello `testAzureApp` mappa, nyissa meg hello `pom.xml` fájlt, és hello build konfigurációját túl&lt;projekt&gt; tooenable hello épület az alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="34cbf-120">Under hello `testAzureApp` folder, open hello `pom.xml` file and add hello build configuration too&lt;project&gt; tooenable hello building of your application:</span></span>

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

2. <span data-ttu-id="34cbf-121">Adja hozzá a szükséges tooaccess hello Azure Java SDK hello függőségek.</span><span class="sxs-lookup"><span data-stu-id="34cbf-121">Add hello dependencies that are needed tooaccess hello Azure Java SDK.</span></span>

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

3. <span data-ttu-id="34cbf-122">Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="34cbf-122">Save hello file.</span></span>

## <a name="create-credentials"></a><span data-ttu-id="34cbf-123">Hitelesítő adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="34cbf-123">Create credentials</span></span>

<span data-ttu-id="34cbf-124">Ez a lépés megkezdése előtt győződjön meg arról, hogy rendelkezik-e hozzáférési tooan [Active Directory szolgáltatás egyszerű](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="34cbf-124">Before you start this step, make sure that you have access tooan [Active Directory service principal](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="34cbf-125">Is rögzíteni kell hello Alkalmazásazonosító, hello hitelesítési kulcs és hello Bérlőazonosító, amelyekre szüksége van egy későbbi lépésben.</span><span class="sxs-lookup"><span data-stu-id="34cbf-125">You should also record hello application ID, hello authentication key, and hello tenant ID that you need in a later step.</span></span>

### <a name="create-hello-authorization-file"></a><span data-ttu-id="34cbf-126">Hello engedélyezési fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="34cbf-126">Create hello authorization file</span></span>

1. <span data-ttu-id="34cbf-127">Hozzon létre egy fájlt `azureauth.properties` , és adja hozzá ezeket a tulajdonságokat tooit:</span><span class="sxs-lookup"><span data-stu-id="34cbf-127">Create a file named `azureauth.properties` and add these properties tooit:</span></span>

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

    <span data-ttu-id="34cbf-128">Cserélje le  **&lt;előfizetés-azonosító&gt;**  az előfizetés-azonosítóval rendelkező  **&lt;alkalmazásazonosító&gt;**  a hello Active Directory-alkalmazás azonosítóval  **&lt;hitelesítési kulcs&gt;**  hello alkalmazás kulccsal, és  **&lt;bérlőazonosító&gt;**  hello bérlővel azonosítója.</span><span class="sxs-lookup"><span data-stu-id="34cbf-128">Replace **&lt;subscription-id&gt;** with your subscription identifier, **&lt;application-id&gt;** with hello Active Directory application identifier, **&lt;authentication-key&gt;** with hello application key, and **&lt;tenant-id&gt;** with hello tenant identifier.</span></span>

2. <span data-ttu-id="34cbf-129">Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="34cbf-129">Save hello file.</span></span>
3. <span data-ttu-id="34cbf-130">A rendszerhéj AZURE_AUTH_LOCATION hello teljes elérési útja toohello hitelesítési fájl nevű környezeti változó értéke.</span><span class="sxs-lookup"><span data-stu-id="34cbf-130">Set an environment variable named AZURE_AUTH_LOCATION in your shell with hello full path toohello authentication file.</span></span>

### <a name="create-hello-management-client"></a><span data-ttu-id="34cbf-131">Hello felügyeleti ügyfél létrehozása</span><span class="sxs-lookup"><span data-stu-id="34cbf-131">Create hello management client</span></span>

1. <span data-ttu-id="34cbf-132">Nyissa meg hello `App.java` a fájl `src\main\java\com\fabrikam` , és győződjön meg arról, hogy a csomag utasítás hello felső:</span><span class="sxs-lookup"><span data-stu-id="34cbf-132">Open hello `App.java` file under `src\main\java\com\fabrikam` and make sure this package statement is at hello top:</span></span>

    ```java
    package com.fabrikam.testAzureApp;
    ```

2. <span data-ttu-id="34cbf-133">Hello csomag utasítás, alatt adja hozzá ezek kimutatások importálása:</span><span class="sxs-lookup"><span data-stu-id="34cbf-133">Under hello package statement, add these import statements:</span></span>
   
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

2. <span data-ttu-id="34cbf-134">toocreate hello Active Directorybeli hitelesítő adatokat, hogy kell-e toomake kérelmekről, a kód toohello fő hello App osztály metódus hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="34cbf-134">toocreate hello Active Directory credentials that you need toomake requests, add this code toohello main method of hello App class:</span></span>
   
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

## <a name="create-resources"></a><span data-ttu-id="34cbf-135">Erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="34cbf-135">Create resources</span></span>

### <a name="create-hello-resource-group"></a><span data-ttu-id="34cbf-136">Hello erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="34cbf-136">Create hello resource group</span></span>

<span data-ttu-id="34cbf-137">Minden erőforrás tartalmaznia kell egy [erőforráscsoport](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="34cbf-137">All resources must be contained in a [Resource group](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="34cbf-138">toospecify értékei alkalmazás hello és hello erőforráscsoport létrehozása, adja hozzá a kódot toohello try blokk hello fő metódus:</span><span class="sxs-lookup"><span data-stu-id="34cbf-138">toospecify values for hello application and create hello resource group, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("Creating resource group...");
ResourceGroup resourceGroup = azure.resourceGroups()
    .define("myResourceGroup")
    .withRegion(Region.US_EAST)
    .create();
```

### <a name="create-hello-availability-set"></a><span data-ttu-id="34cbf-139">Hello rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="34cbf-139">Create hello availability set</span></span>

<span data-ttu-id="34cbf-140">[Rendelkezésre állási készletek](tutorial-availability-sets.md) könnyebben meg az alkalmazás által használt toomaintain hello virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="34cbf-140">[Availability sets](tutorial-availability-sets.md) make it easier for you toomaintain hello virtual machines used by your application.</span></span>

<span data-ttu-id="34cbf-141">toocreate hello rendelkezésre állási megadásához adja hozzá a kódot toohello try blokk hello fő metódus:</span><span class="sxs-lookup"><span data-stu-id="34cbf-141">toocreate hello availability set, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("Creating availability set...");
AvailabilitySet availabilitySet = azure.availabilitySets()
    .define("myAvailabilitySet")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withSku(AvailabilitySetSkuTypes.MANAGED)
    .create();
```
### <a name="create-hello-public-ip-address"></a><span data-ttu-id="34cbf-142">Hello nyilvános IP-cím létrehozása</span><span class="sxs-lookup"><span data-stu-id="34cbf-142">Create hello public IP address</span></span>

<span data-ttu-id="34cbf-143">A [nyilvános IP-cím](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) szükséges toocommunicate hello virtuális gép van.</span><span class="sxs-lookup"><span data-stu-id="34cbf-143">A [Public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) is needed toocommunicate with hello virtual machine.</span></span>

<span data-ttu-id="34cbf-144">toocreate hello nyilvános IP-cím hello virtuális géphez, a kód toohello try blokk hello fő metódus adja hozzá:</span><span class="sxs-lookup"><span data-stu-id="34cbf-144">toocreate hello public IP address for hello virtual machine, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("Creating public IP address...");
PublicIPAddress publicIPAddress = azure.publicIPAddresses()
    .define("myPublicIP")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withDynamicIP()
    .create();
```

### <a name="create-hello-virtual-network"></a><span data-ttu-id="34cbf-145">Hello virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="34cbf-145">Create hello virtual network</span></span>

<span data-ttu-id="34cbf-146">Az alhálózat szerepelnie kell egy virtuális gépet egy [virtuális hálózati](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="34cbf-146">A virtual machine must be in a subnet of a [Virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

<span data-ttu-id="34cbf-147">egy alhálózat toocreate és egy virtuális hálózatot, adja hozzá a kódot toohello try blokk hello fő metódus:</span><span class="sxs-lookup"><span data-stu-id="34cbf-147">toocreate a subnet and a virtual network, add this code toohello try block in hello main method:</span></span>

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

### <a name="create-hello-network-interface"></a><span data-ttu-id="34cbf-148">Hello hálózati illesztő létrehozása</span><span class="sxs-lookup"><span data-stu-id="34cbf-148">Create hello network interface</span></span>

<span data-ttu-id="34cbf-149">A virtuális gép egy hálózati illesztő toocommunicate hello virtuális hálózaton kell.</span><span class="sxs-lookup"><span data-stu-id="34cbf-149">A virtual machine needs a network interface toocommunicate on hello virtual network.</span></span>

<span data-ttu-id="34cbf-150">egy adott hálózati csatoló toocreate a kód toohello try blokk hello fő metódus adja hozzá:</span><span class="sxs-lookup"><span data-stu-id="34cbf-150">toocreate a network interface, add this code toohello try block in hello main method:</span></span>

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

### <a name="create-hello-virtual-machine"></a><span data-ttu-id="34cbf-151">Hello virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="34cbf-151">Create hello virtual machine</span></span>

<span data-ttu-id="34cbf-152">Most, hogy a létrehozott összes hello erőforrások támogatása, létrehozhat egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="34cbf-152">Now that you created all hello supporting resources, you can create a virtual machine.</span></span>

<span data-ttu-id="34cbf-153">toocreate hello virtuális gépet, adja hozzá a kódot toohello try blokk hello fő metódus:</span><span class="sxs-lookup"><span data-stu-id="34cbf-153">toocreate hello virtual machine, add this code toohello try block in hello main method:</span></span>

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
System.out.println("Press enter tooget information about hello VM...");
input.nextLine();
```

> [!NOTE]
> <span data-ttu-id="34cbf-154">Ebben az oktatóanyagban létrehoz egy virtuális gépet, hello Windows Server operációs rendszer verziója.</span><span class="sxs-lookup"><span data-stu-id="34cbf-154">This tutorial creates a virtual machine running a version of hello Windows Server operating system.</span></span> <span data-ttu-id="34cbf-155">További információ az egyéb rendszerképek kiválasztásáról toolearn lásd: [keresse meg és válassza ki azokat a Windows PowerShell és az Azure parancssori felület hello Azure virtuális gép lemezképeket](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="34cbf-155">toolearn more about selecting other images, see [Navigate and select Azure virtual machine images with Windows PowerShell and hello Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
>

<span data-ttu-id="34cbf-156">Ha azt szeretné, hogy a meglévő lemez Piactéri rendszerkép helyett toouse, használja ezt a kódot:</span><span class="sxs-lookup"><span data-stu-id="34cbf-156">If you want toouse an existing disk instead of a marketplace image, use this code:</span></span> 

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

## <a name="perform-management-tasks"></a><span data-ttu-id="34cbf-157">Felügyeleti feladatok végrehajtása</span><span class="sxs-lookup"><span data-stu-id="34cbf-157">Perform management tasks</span></span>

<span data-ttu-id="34cbf-158">A virtuális gépek hello életciklusa során szükség lehet a toorun felügyeleti feladatokhoz, mint az indítása, leállítása vagy egy virtuális gép törlése.</span><span class="sxs-lookup"><span data-stu-id="34cbf-158">During hello lifecycle of a virtual machine, you may want toorun management tasks such as starting, stopping, or deleting a virtual machine.</span></span> <span data-ttu-id="34cbf-159">Emellett érdemes lehet toocreate kód tooautomate ismétlődő vagy összetett feladatokat.</span><span class="sxs-lookup"><span data-stu-id="34cbf-159">Additionally, you may want toocreate code tooautomate repetitive or complex tasks.</span></span>

<span data-ttu-id="34cbf-160">Kell toodo minden hello VM, úgy kell tooget egy példányát.</span><span class="sxs-lookup"><span data-stu-id="34cbf-160">When you need toodo anything with hello VM, you need tooget an instance of it.</span></span> <span data-ttu-id="34cbf-161">A kód toohello try blokk hello fő metódus hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="34cbf-161">Add this code toohello try block of hello main method:</span></span>

```java
VirtualMachine vm = azure.virtualMachines().getByResourceGroup("myResourceGroup", "myVM");
```

### <a name="get-information-about-hello-vm"></a><span data-ttu-id="34cbf-162">Hello virtuális gép adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="34cbf-162">Get information about hello VM</span></span>

<span data-ttu-id="34cbf-163">hello virtuális gép, tooget adatait a kód toohello try blokk hello fő metódus adja hozzá:</span><span class="sxs-lookup"><span data-stu-id="34cbf-163">tooget information about hello virtual machine, add this code toohello try block in hello main method:</span></span>

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
System.out.println("Press enter toocontinue...");
input.nextLine();   
```

### <a name="stop-hello-vm"></a><span data-ttu-id="34cbf-164">Hello VM leállítása</span><span class="sxs-lookup"><span data-stu-id="34cbf-164">Stop hello VM</span></span>

<span data-ttu-id="34cbf-165">Állítsa le a virtuális gépet és a beállítások megtartásához, de továbbra is toobe felszámított, vagy állítsa le a virtuális gépet, és azt felszabadítani.</span><span class="sxs-lookup"><span data-stu-id="34cbf-165">You can stop a virtual machine and keep all its settings, but continue toobe charged for it, or you can stop a virtual machine and deallocate it.</span></span> <span data-ttu-id="34cbf-166">Ha egy virtuális gép fel van szabadítva, vele társított összes erőforrás is felszabadított és számlázási végpontjainak.</span><span class="sxs-lookup"><span data-stu-id="34cbf-166">When a virtual machine is deallocated, all resources associated with it are also deallocated and billing ends for it.</span></span>

<span data-ttu-id="34cbf-167">toostop hello virtuális gép felszabadítása, nélkül adja hozzá a kódot toohello try blokk hello fő metódus:</span><span class="sxs-lookup"><span data-stu-id="34cbf-167">toostop hello virtual machine without deallocating it, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("Stopping vm...");
vm.powerOff();
System.out.println("Press enter toocontinue...");
input.nextLine();
```

<span data-ttu-id="34cbf-168">Ha azt szeretné, hogy toodeallocate hello virtuális gép, módosítsa a hello kikapcsolt hívás toothis kódot:</span><span class="sxs-lookup"><span data-stu-id="34cbf-168">If you want toodeallocate hello virtual machine, change hello PowerOff call toothis code:</span></span>

```java
vm.deallocate();
```

### <a name="start-hello-vm"></a><span data-ttu-id="34cbf-169">Indítsa el a virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="34cbf-169">Start hello VM</span></span>

<span data-ttu-id="34cbf-170">toostart hello virtuális gépet, adja hozzá a kódot toohello try blokk hello fő metódus:</span><span class="sxs-lookup"><span data-stu-id="34cbf-170">toostart hello virtual machine, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("Starting vm...");
vm.start();
System.out.println("Press enter toocontinue...");
input.nextLine();
```

### <a name="resize-hello-vm"></a><span data-ttu-id="34cbf-171">Automatikus oszlopszélesség hello méretű VM</span><span class="sxs-lookup"><span data-stu-id="34cbf-171">Resize hello VM</span></span>

<span data-ttu-id="34cbf-172">Telepítési sok szempontját figyelembe kell venni, amikor eldönti, a virtuális gép méretét.</span><span class="sxs-lookup"><span data-stu-id="34cbf-172">Many aspects of deployment should be considered when deciding on a size for your virtual machine.</span></span> <span data-ttu-id="34cbf-173">További információkért lásd: [Virtuálisgép-méretek](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="34cbf-173">For more information, see [VM sizes](sizes.md).</span></span>  

<span data-ttu-id="34cbf-174">hello virtuális gép, toochange mérete a kód toohello try blokk hello fő metódus adja hozzá:</span><span class="sxs-lookup"><span data-stu-id="34cbf-174">toochange size of hello virtual machine, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("Resizing vm...");
vm.update()
    .withSize(VirtualMachineSizeTypes.STANDARD_DS2)
    .apply();
System.out.println("Press enter toocontinue...");
input.nextLine();
```

### <a name="add-a-data-disk-toohello-vm"></a><span data-ttu-id="34cbf-175">Adja hozzá a adatok lemez toohello méretű VM</span><span class="sxs-lookup"><span data-stu-id="34cbf-175">Add a data disk toohello VM</span></span>

<span data-ttu-id="34cbf-176">tooadd adatok lemez toohello virtuális gép számára pedig 2 GB-nál, 0 és ReadWrite gyorsítótárazási típusú logikai egység van, adja hozzá a kódot toohello try blokk hello fő metódus:</span><span class="sxs-lookup"><span data-stu-id="34cbf-176">tooadd a data disk toohello virtual machine that is 2 GB in size, has a LUN of 0, and a caching type of ReadWrite, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("Adding data disk...");
vm.update()
    .withNewDataDisk(2, 0, CachingTypes.READ_WRITE)
    .apply();
System.out.println("Press enter toodelete resources...");
input.nextLine();
```

## <a name="delete-resources"></a><span data-ttu-id="34cbf-177">Erőforrások törlése</span><span class="sxs-lookup"><span data-stu-id="34cbf-177">Delete resources</span></span>

<span data-ttu-id="34cbf-178">Mivel az Azure-ban használt erőforrásokhoz van szó, még mindig célszerű toodelete erőforrásokat, amelyek már nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="34cbf-178">Because you are charged for resources used in Azure, it is always good practice toodelete resources that are no longer needed.</span></span> <span data-ttu-id="34cbf-179">Ha azt szeretné, hogy toodelete hello virtuális gépek és erőforrások támogató összes hello, minden toodo van hello erőforrás csoport törlése.</span><span class="sxs-lookup"><span data-stu-id="34cbf-179">If you want toodelete hello virtual machines and all hello supporting resources, all you have toodo is delete hello resource group.</span></span>

1. <span data-ttu-id="34cbf-180">toodelete hello erőforrás csoportjában adja hozzá a kódot toohello try blokk hello fő metódus:</span><span class="sxs-lookup"><span data-stu-id="34cbf-180">toodelete hello resource group, add this code toohello try block in hello main method:</span></span>
   
```java
System.out.println("Deleting resources...");
azure.resourceGroups().deleteByName("myResourceGroup");
```

2. <span data-ttu-id="34cbf-181">Hello App.java fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="34cbf-181">Save hello App.java file.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="34cbf-182">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="34cbf-182">Run hello application</span></span>

<span data-ttu-id="34cbf-183">Akkor kell a konzol alkalmazás toorun teljesen a start toofinish körülbelül öt percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="34cbf-183">It should take about five minutes for this console application toorun completely from start toofinish.</span></span>

1. <span data-ttu-id="34cbf-184">toorun hello alkalmazást, használja ezt a Maven-parancsot:</span><span class="sxs-lookup"><span data-stu-id="34cbf-184">toorun hello application, use this Maven command:</span></span>

    ```
    mvn compile exec:java
    ```

2. <span data-ttu-id="34cbf-185">Ahhoz, hogy nyomja le az ENTER **Enter** toostart törlése erőforrásokat, készíthet néhány perc múlva hello erőforrások tooverify hello létrehozását a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="34cbf-185">Before you press **Enter** toostart deleting resources, you could take a few minutes tooverify hello creation of hello resources in hello Azure portal.</span></span> <span data-ttu-id="34cbf-186">Kattintson a hello telepítési toosee telepítésére vonatkozó állapotadatok hello.</span><span class="sxs-lookup"><span data-stu-id="34cbf-186">Click hello deployment status toosee information about hello deployment.</span></span>


## <a name="next-steps"></a><span data-ttu-id="34cbf-187">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="34cbf-187">Next steps</span></span>
* <span data-ttu-id="34cbf-188">További információ hello [Azure Java-könyvtárakban](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-overview).</span><span class="sxs-lookup"><span data-stu-id="34cbf-188">Learn more about using hello [Azure libraries for Java](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-overview).</span></span>

