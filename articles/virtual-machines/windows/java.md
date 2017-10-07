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
# <a name="create-and-manage-windows-vms-in-azure-using-java"></a>Létrehozása és kezelése Windows-alapú virtuális gépek az Azure-ban Java

Egy [Azure virtuális gép](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) több támogató Azure-erőforrások kell. Ez a cikk ismerteti a létrehozása, kezelése és a Java Virtuálisgép-erőforrások törlése. Az alábbiak végrehajtásának módját ismerheti meg:

> [!div class="checklist"]
> * Maven-projekt létrehozása
> * Adja hozzá a függőségek
> * Hitelesítő adatok létrehozása
> * Erőforrások létrehozása
> * Felügyeleti feladatok végrehajtása
> * Erőforrások törlése
> * Hello alkalmazás futtatása

Körülbelül 20 percet toodo szükséges lépéseket.

## <a name="create-a-maven-project"></a>Maven-projekt létrehozása

1. Ha még nem tette meg, telepítse [Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
2. Telepítés [Maven](http://maven.apache.org/download.cgi).
3. Hozzon létre egy új mappát és hello projektet:
    
    ```
    mkdir java-azure-test
    cd java-azure-test
    
    mvn archetype:generate -DgroupId=com.fabrikam -DartifactId=testAzureApp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

## <a name="add-dependencies"></a>Adja hozzá a függőségek

1. A hello `testAzureApp` mappa, nyissa meg hello `pom.xml` fájlt, és hello build konfigurációját túl&lt;projekt&gt; tooenable hello épület az alkalmazás:

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

2. Adja hozzá a szükséges tooaccess hello Azure Java SDK hello függőségek.

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

3. Hello fájl mentéséhez.

## <a name="create-credentials"></a>Hitelesítő adatok létrehozása

Ez a lépés megkezdése előtt győződjön meg arról, hogy rendelkezik-e hozzáférési tooan [Active Directory szolgáltatás egyszerű](../../azure-resource-manager/resource-group-create-service-principal-portal.md). Is rögzíteni kell hello Alkalmazásazonosító, hello hitelesítési kulcs és hello Bérlőazonosító, amelyekre szüksége van egy későbbi lépésben.

### <a name="create-hello-authorization-file"></a>Hello engedélyezési fájl létrehozása

1. Hozzon létre egy fájlt `azureauth.properties` , és adja hozzá ezeket a tulajdonságokat tooit:

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

2. Hello fájl mentéséhez.
3. A rendszerhéj AZURE_AUTH_LOCATION hello teljes elérési útja toohello hitelesítési fájl nevű környezeti változó értéke.

### <a name="create-hello-management-client"></a>Hello felügyeleti ügyfél létrehozása

1. Nyissa meg hello `App.java` a fájl `src\main\java\com\fabrikam` , és győződjön meg arról, hogy a csomag utasítás hello felső:

    ```java
    package com.fabrikam.testAzureApp;
    ```

2. Hello csomag utasítás, alatt adja hozzá ezek kimutatások importálása:
   
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

2. toocreate hello Active Directorybeli hitelesítő adatokat, hogy kell-e toomake kérelmekről, a kód toohello fő hello App osztály metódus hozzáadása:
   
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

## <a name="create-resources"></a>Erőforrások létrehozása

### <a name="create-hello-resource-group"></a>Hello erőforráscsoport létrehozása

Minden erőforrás tartalmaznia kell egy [erőforráscsoport](../../azure-resource-manager/resource-group-overview.md).

toospecify értékei alkalmazás hello és hello erőforráscsoport létrehozása, adja hozzá a kódot toohello try blokk hello fő metódus:

```java
System.out.println("Creating resource group...");
ResourceGroup resourceGroup = azure.resourceGroups()
    .define("myResourceGroup")
    .withRegion(Region.US_EAST)
    .create();
```

### <a name="create-hello-availability-set"></a>Hello rendelkezésre állási csoport létrehozása

[Rendelkezésre állási készletek](tutorial-availability-sets.md) könnyebben meg az alkalmazás által használt toomaintain hello virtuális gépeket.

toocreate hello rendelkezésre állási megadásához adja hozzá a kódot toohello try blokk hello fő metódus:

```java
System.out.println("Creating availability set...");
AvailabilitySet availabilitySet = azure.availabilitySets()
    .define("myAvailabilitySet")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withSku(AvailabilitySetSkuTypes.MANAGED)
    .create();
```
### <a name="create-hello-public-ip-address"></a>Hello nyilvános IP-cím létrehozása

A [nyilvános IP-cím](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) szükséges toocommunicate hello virtuális gép van.

toocreate hello nyilvános IP-cím hello virtuális géphez, a kód toohello try blokk hello fő metódus adja hozzá:

```java
System.out.println("Creating public IP address...");
PublicIPAddress publicIPAddress = azure.publicIPAddresses()
    .define("myPublicIP")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withDynamicIP()
    .create();
```

### <a name="create-hello-virtual-network"></a>Hello virtuális hálózat létrehozása

Az alhálózat szerepelnie kell egy virtuális gépet egy [virtuális hálózati](../../virtual-network/virtual-networks-overview.md).

egy alhálózat toocreate és egy virtuális hálózatot, adja hozzá a kódot toohello try blokk hello fő metódus:

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

### <a name="create-hello-network-interface"></a>Hello hálózati illesztő létrehozása

A virtuális gép egy hálózati illesztő toocommunicate hello virtuális hálózaton kell.

egy adott hálózati csatoló toocreate a kód toohello try blokk hello fő metódus adja hozzá:

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

### <a name="create-hello-virtual-machine"></a>Hello virtuális gép létrehozása

Most, hogy a létrehozott összes hello erőforrások támogatása, létrehozhat egy virtuális gépet.

toocreate hello virtuális gépet, adja hozzá a kódot toohello try blokk hello fő metódus:

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
> Ebben az oktatóanyagban létrehoz egy virtuális gépet, hello Windows Server operációs rendszer verziója. További információ az egyéb rendszerképek kiválasztásáról toolearn lásd: [keresse meg és válassza ki azokat a Windows PowerShell és az Azure parancssori felület hello Azure virtuális gép lemezképeket](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
>

Ha azt szeretné, hogy a meglévő lemez Piactéri rendszerkép helyett toouse, használja ezt a kódot: 

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

## <a name="perform-management-tasks"></a>Felügyeleti feladatok végrehajtása

A virtuális gépek hello életciklusa során szükség lehet a toorun felügyeleti feladatokhoz, mint az indítása, leállítása vagy egy virtuális gép törlése. Emellett érdemes lehet toocreate kód tooautomate ismétlődő vagy összetett feladatokat.

Kell toodo minden hello VM, úgy kell tooget egy példányát. A kód toohello try blokk hello fő metódus hozzáadása:

```java
VirtualMachine vm = azure.virtualMachines().getByResourceGroup("myResourceGroup", "myVM");
```

### <a name="get-information-about-hello-vm"></a>Hello virtuális gép adatainak beolvasása

hello virtuális gép, tooget adatait a kód toohello try blokk hello fő metódus adja hozzá:

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

### <a name="stop-hello-vm"></a>Hello VM leállítása

Állítsa le a virtuális gépet és a beállítások megtartásához, de továbbra is toobe felszámított, vagy állítsa le a virtuális gépet, és azt felszabadítani. Ha egy virtuális gép fel van szabadítva, vele társított összes erőforrás is felszabadított és számlázási végpontjainak.

toostop hello virtuális gép felszabadítása, nélkül adja hozzá a kódot toohello try blokk hello fő metódus:

```java
System.out.println("Stopping vm...");
vm.powerOff();
System.out.println("Press enter toocontinue...");
input.nextLine();
```

Ha azt szeretné, hogy toodeallocate hello virtuális gép, módosítsa a hello kikapcsolt hívás toothis kódot:

```java
vm.deallocate();
```

### <a name="start-hello-vm"></a>Indítsa el a virtuális gép hello

toostart hello virtuális gépet, adja hozzá a kódot toohello try blokk hello fő metódus:

```java
System.out.println("Starting vm...");
vm.start();
System.out.println("Press enter toocontinue...");
input.nextLine();
```

### <a name="resize-hello-vm"></a>Automatikus oszlopszélesség hello méretű VM

Telepítési sok szempontját figyelembe kell venni, amikor eldönti, a virtuális gép méretét. További információkért lásd: [Virtuálisgép-méretek](sizes.md).  

hello virtuális gép, toochange mérete a kód toohello try blokk hello fő metódus adja hozzá:

```java
System.out.println("Resizing vm...");
vm.update()
    .withSize(VirtualMachineSizeTypes.STANDARD_DS2)
    .apply();
System.out.println("Press enter toocontinue...");
input.nextLine();
```

### <a name="add-a-data-disk-toohello-vm"></a>Adja hozzá a adatok lemez toohello méretű VM

tooadd adatok lemez toohello virtuális gép számára pedig 2 GB-nál, 0 és ReadWrite gyorsítótárazási típusú logikai egység van, adja hozzá a kódot toohello try blokk hello fő metódus:

```java
System.out.println("Adding data disk...");
vm.update()
    .withNewDataDisk(2, 0, CachingTypes.READ_WRITE)
    .apply();
System.out.println("Press enter toodelete resources...");
input.nextLine();
```

## <a name="delete-resources"></a>Erőforrások törlése

Mivel az Azure-ban használt erőforrásokhoz van szó, még mindig célszerű toodelete erőforrásokat, amelyek már nem szükséges. Ha azt szeretné, hogy toodelete hello virtuális gépek és erőforrások támogató összes hello, minden toodo van hello erőforrás csoport törlése.

1. toodelete hello erőforrás csoportjában adja hozzá a kódot toohello try blokk hello fő metódus:
   
```java
System.out.println("Deleting resources...");
azure.resourceGroups().deleteByName("myResourceGroup");
```

2. Hello App.java fájl mentéséhez.

## <a name="run-hello-application"></a>Hello alkalmazás futtatása

Akkor kell a konzol alkalmazás toorun teljesen a start toofinish körülbelül öt percet is igénybe vehet.

1. toorun hello alkalmazást, használja ezt a Maven-parancsot:

    ```
    mvn compile exec:java
    ```

2. Ahhoz, hogy nyomja le az ENTER **Enter** toostart törlése erőforrásokat, készíthet néhány perc múlva hello erőforrások tooverify hello létrehozását a hello Azure-portálon. Kattintson a hello telepítési toosee telepítésére vonatkozó állapotadatok hello.


## <a name="next-steps"></a>Következő lépések
* További információ hello [Azure Java-könyvtárakban](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-overview).

