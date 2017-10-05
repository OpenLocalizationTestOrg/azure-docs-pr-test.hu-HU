---
title: "Az Azure Resource Manager-sablonok Linux számítási erőforrásokat üzembe helyezi |} Microsoft Docs"
description: "Azure virtuális gép DotNet fő oktatóanyag"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 1c4d419e-ba0e-45e4-a9dd-7ee9975a86f9
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c3f9f98079e0c89d1231f9c3e62e82c33ad18236
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="application-architecture-with-azure-resource-manager-templates-for-linux-vms"></a><span data-ttu-id="aec48-103">Linux virtuális gépek Azure Resource Manager-sablonok alkalmazásarchitektúra</span><span class="sxs-lookup"><span data-stu-id="aec48-103">Application architecture with Azure Resource Manager templates for Linux VMs</span></span>

<span data-ttu-id="aec48-104">Az Azure Resource Manager deployment fejlesztésekor számítási követelmények kell Azure-erőforrások és szolgáltatások kell hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="aec48-104">When developing an Azure Resource Manager deployment, compute requirements need to be mapped to Azure resources and services.</span></span> <span data-ttu-id="aec48-105">Ha egy alkalmazás több http-végpontokról, egy adatbázist és egy szolgáltatás gyorsítótárazás adatokat tartalmaz, az Azure-erőforrások üzemeltető minden ezeket az összetevőket kell rationalized lehet.</span><span class="sxs-lookup"><span data-stu-id="aec48-105">If an application consists of several http endpoints, a database, and a data caching service, the Azure resources that host each of these components needs to be rationalized.</span></span> <span data-ttu-id="aec48-106">Például a zeneáruház mintaalkalmazás virtuális gépen futtatott webalkalmazás, és egy SQL-adatbázis, amely az Azure SQL-adatbázis található.</span><span class="sxs-lookup"><span data-stu-id="aec48-106">For instance, the sample Music Store application includes a web application that is hosted on a virtual machine, and a SQL database, which is hosted in Azure SQL database.</span></span> 

<span data-ttu-id="aec48-107">Ez a dokumentum részletesen, hogyan állíthatók be a minta Azure Resource Manager-sablon a zeneáruház számítási erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="aec48-107">This document details how the Music Store compute resources are configured in the sample Azure Resource Manager template.</span></span> <span data-ttu-id="aec48-108">Minden függőségeket és külön konfigurációt vannak kiemelve.</span><span class="sxs-lookup"><span data-stu-id="aec48-108">All dependencies and unique configurations are highlighted.</span></span> <span data-ttu-id="aec48-109">A legjobb használhatóság érdekében előtelepítése a megoldást az Azure-előfizetés és a munkahelyi együtt az Azure Resource Manager-sablon egy példányát.</span><span class="sxs-lookup"><span data-stu-id="aec48-109">For the best experience, pre-deploy an instance of the solution to your Azure subscription and work along with the Azure Resource Manager template.</span></span> <span data-ttu-id="aec48-110">A teljes sablon – itt található [zene tároló központi telepítését, az Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span><span class="sxs-lookup"><span data-stu-id="aec48-110">The complete template can be found here – [Music Store Deployment on Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span></span> 

## <a name="virtual-machine"></a><span data-ttu-id="aec48-111">Virtuális gép</span><span class="sxs-lookup"><span data-stu-id="aec48-111">Virtual Machine</span></span>
<span data-ttu-id="aec48-112">Zene áruházból származó alkalmazás egy webalkalmazást, ahol az ügyfelek és vásárolhat zene tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="aec48-112">The Music Store application includes a web application where customers can browse and purchase music.</span></span> <span data-ttu-id="aec48-113">Miközben Számos Azure szolgáltatást, amely a webes alkalmazás, például egy virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="aec48-113">While there are several Azure services that can host web applications, for this example, a Virtual Machine is used.</span></span> <span data-ttu-id="aec48-114">A minta zeneáruház sablont használ, a virtuális gép telepítve van, egy webkiszolgáló telepítése és a zeneáruház webhely telepítése és konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="aec48-114">Using the sample Music Store template, a virtual machine is deployed, a web server install, and the Music Store website installed and configured.</span></span> <span data-ttu-id="aec48-115">Ez a cikk az csak a virtuális gép központi telepítés részleteit.</span><span class="sxs-lookup"><span data-stu-id="aec48-115">For the sake of this article, only the virtual machine deployment is detailed.</span></span> <span data-ttu-id="aec48-116">A webalkalmazás-kiszolgáló és az alkalmazás konfigurációja egy újabb cikkben részleteit.</span><span class="sxs-lookup"><span data-stu-id="aec48-116">The configuration of the web server and the application is detailed in a later article.</span></span>

<span data-ttu-id="aec48-117">A virtuális gépek felveheti egy sablont a Visual Studio új erőforrás hozzáadása varázsló használatával, vagy érvényes JSON beszúrása a központi telepítési sablont.</span><span class="sxs-lookup"><span data-stu-id="aec48-117">A virtual machine can be added to a template using the Visual Studio Add New Resource wizard, or by inserting valid JSON into the deployment template.</span></span> <span data-ttu-id="aec48-118">Virtuális gépek telepítésekor számos kapcsolódó erőforrások is szükséges.</span><span class="sxs-lookup"><span data-stu-id="aec48-118">When deploying a virtual machine, several related resources are also needed.</span></span> <span data-ttu-id="aec48-119">A sablon létrehozása a Visual Studio használatával, ha ezeket az erőforrásokat jön létre.</span><span class="sxs-lookup"><span data-stu-id="aec48-119">If using Visual Studio to create the template, these resources are created for you.</span></span> <span data-ttu-id="aec48-120">Ha manuálisan hoz létre, a sablon, ezeket az erőforrásokat kell szúrja be, és konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="aec48-120">If manually constructing the template, these resources need to be inserted and configured.</span></span>

<span data-ttu-id="aec48-121">Hajtsa végre az erre a hivatkozásra kattintva tekintse meg a JSON-mintát belül a Resource Manager-sablon – [virtuális gép JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L295).</span><span class="sxs-lookup"><span data-stu-id="aec48-121">Follow this link to see the JSON sample within the Resource Manager template – [Virtual Machine JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L295).</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[concat(variables('vmName'),copyindex())]",
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "virtualMachineLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "tags": {
    "displayName": "virtual-machine"
  },
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('vhdStorageName'))]",
    "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]",
    "nicLoop"
  ],
  "properties": {
    "availabilitySet": {
      "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
    },
      ........<truncated>  
    }
```

<span data-ttu-id="aec48-122">Amennyiben telepített, a virtuális gép tulajdonságai láthatók az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="aec48-122">Once deployed, the virtual machine properties can be seen in the Azure portal.</span></span>

![Virtuális gép](./media/dotnet-core-2-architecture/vm.png)

## <a name="storage-account"></a><span data-ttu-id="aec48-124">Tárfiók</span><span class="sxs-lookup"><span data-stu-id="aec48-124">Storage Account</span></span>
<span data-ttu-id="aec48-125">Storage-fiókok rendelkezik számos tárolási lehetőségeket és képességeket.</span><span class="sxs-lookup"><span data-stu-id="aec48-125">Storage accounts have many storage options and capabilities.</span></span> <span data-ttu-id="aec48-126">A környezet az Azure virtuális gépek a tárfiók tárolja a virtuális merevlemezek a virtuális gép és bármelyik adatlemeznek.</span><span class="sxs-lookup"><span data-stu-id="aec48-126">For the context of Azure Virtual machines, a storage account holds the virtual hard drives of the virtual machine and any additional data disks.</span></span> <span data-ttu-id="aec48-127">A zeneáruház minta magában foglalja a központi telepítésben az egyes virtuális gépek virtuális merevlemez tárolásához egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="aec48-127">The Music Store sample includes one storage account to hold the virtual hard drive of each virtual machine in the deployment.</span></span> 

<span data-ttu-id="aec48-128">Hajtsa végre az erre a hivatkozásra kattintva tekintse meg a JSON-mintát belül a Resource Manager-sablon – [Tárfiók](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L109).</span><span class="sxs-lookup"><span data-stu-id="aec48-128">Follow this link to see the JSON sample within the Resource Manager template – [Storage Account](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L109).</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[variables('vhdStorageName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "storage-account"
  },
  "properties": {
    "accountType": "[variables('vhdStorageType')]"
  }
}
```

<span data-ttu-id="aec48-129">A storage-fiók társítása egy virtuális géppel a virtuális gép Resource Manager sablon deklarációjában belül.</span><span class="sxs-lookup"><span data-stu-id="aec48-129">A storage account is associate with a virtual machine inside the Resource Manager template declaration of the virtual machine.</span></span> 

<span data-ttu-id="aec48-130">Hajtsa végre az erre a hivatkozásra kattintva tekintse meg a JSON-mintát belül a Resource Manager-sablon – [virtuális gép és a Storage-fiókhoz társításának](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L341).</span><span class="sxs-lookup"><span data-stu-id="aec48-130">Follow this link to see the JSON sample within the Resource Manager template – [Virtual Machine and Storage Account association](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L341).</span></span>

```json
"osDisk": {
  "name": "osdisk",
  "vhd": {
    "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/',variables('vhdStorageName')), '2015-06-15').primaryEndpoints.blob,'vhds/osdisk', copyindex(), '.vhd')]"
  },
  "caching": "ReadWrite",
  "createOption": "FromImage"
}
```

<span data-ttu-id="aec48-131">A központi telepítést követően a tárfiók tekintheti meg az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="aec48-131">After deployment, the storage account can be viewed in the Azure portal.</span></span>

![Tárfiók](./media/dotnet-core-2-architecture/storacct.png)

<span data-ttu-id="aec48-133">Kattintson a fiók blob tároló, a virtuális merevlemez-meghajtóról fájl sablonnal telepített virtuális gépek láthatók.</span><span class="sxs-lookup"><span data-stu-id="aec48-133">Clicking into the storage account blob container, the virtual hard drive file for each virtual machine deployed with the template can be seen.</span></span>

![Virtuális merevlemezek](./media/dotnet-core-2-architecture/vhd.png)

<span data-ttu-id="aec48-135">További információ az Azure Storage: [Azure Storage-dokumentációt](https://azure.microsoft.com/documentation/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="aec48-135">For more information on Azure Storage, see [Azure Storage documentation](https://azure.microsoft.com/documentation/services/storage/).</span></span>

## <a name="virtual-network"></a><span data-ttu-id="aec48-136">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="aec48-136">Virtual Network</span></span>
<span data-ttu-id="aec48-137">Ha egy virtuális géphez szükséges például képes kommunikálni más virtuális gépek és az Azure-erőforrások belső hálózat, a egy Azure virtuális hálózatra szükség.</span><span class="sxs-lookup"><span data-stu-id="aec48-137">If a virtual machine requires internal networking such as the ability to communicate with other virtual machines and Azure resources, an Azure Virtual Network is required.</span></span>  <span data-ttu-id="aec48-138">Virtuális hálózat tegye a virtuális gép elérhetővé az interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="aec48-138">A virtual network does not make the virtual machine accessible over the internet.</span></span> <span data-ttu-id="aec48-139">Nyilvános egy nyilvános IP-címet, amely ezen részében részletes szükséges.</span><span class="sxs-lookup"><span data-stu-id="aec48-139">Public connectivity requires a public IP address, which is detailed later in this series.</span></span>

<span data-ttu-id="aec48-140">Hajtsa végre az erre a hivatkozásra kattintva tekintse meg a JSON-mintát belül a Resource Manager-sablon – [virtuális hálózat és alhálózat](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L136).</span><span class="sxs-lookup"><span data-stu-id="aec48-140">Follow this link to see the JSON sample within the Resource Manager template – [Virtual Network and Subnets](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L136).</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/virtualNetworks",
  "name": "[variables('virtualNetworkName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Network/networkSecurityGroups/', variables('networkSecurityGroup'))]"
  ],
  "tags": {
    "displayName": "virtual-network"
  },
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "10.0.0.0/24"
      ]
    },
    "subnets": [
      {
        "name": "[variables('subnetName')]",
        "properties": {
          "addressPrefix": "10.0.0.0/24",
          "networkSecurityGroup": {
            "id": "[resourceId('Microsoft.Network/networkSecurityGroups', variables('networkSecurityGroup'))]"
          }
        }
      }
    ]
  }
}
```

<span data-ttu-id="aec48-141">Azure-portálról a virtuális hálózati illusztráción láthatóhoz hasonló következő.</span><span class="sxs-lookup"><span data-stu-id="aec48-141">From the Azure portal, the virtual network looks like the following image.</span></span> <span data-ttu-id="aec48-142">Figyelje meg, hogy a virtuális hálózat összes virtuális gépet a sablon használatával telepített vannak csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="aec48-142">Notice that all virtual machines deployed with the template are attached to the virtual network.</span></span>

![Virtual Network](./media/dotnet-core-2-architecture/vnet.png)

## <a name="network-interface"></a><span data-ttu-id="aec48-144">Hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="aec48-144">Network Interface</span></span>
 <span data-ttu-id="aec48-145">A hálózati adaptert egy virtuális hálózathoz, pontosabban, hogy definiálva van a virtuális hálózati alhálózat csatlakoztat egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="aec48-145">A network interface connects a virtual machine to a virtual network, more specifically to a subnet that has been defined in the virtual network.</span></span> 

 <span data-ttu-id="aec48-146">Hajtsa végre az erre a hivatkozásra kattintva tekintse meg a JSON-mintát belül a Resource Manager-sablon – [hálózati illesztő](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L166).</span><span class="sxs-lookup"><span data-stu-id="aec48-146">Follow this link to see the JSON sample within the Resource Manager template – [Network Interface](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L166).</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/networkInterfaces",
  "name": "[concat(variables('networkInterfaceNamePrefix'), copyindex())]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "network-interface"
  },
  "copy": {
    "name": "nicLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
    "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIpAddressName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'), '/inboundNatRules/', 'SSH-VM', copyIndex())]"
  ],
  "properties": {
    "ipConfigurations": [
      {
        "name": "ipconfig1",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "subnet": {
            "id": "[variables('subnetRef')]"
          },
          "loadBalancerBackendAddressPools": [
            {
              "id": "[variables('lbPoolID')]"
            }
          ],
          "loadBalancerInboundNatRules": [
            {
              "id": "[concat(variables('lbID'),'/inboundNatRules/SSH-VM', copyIndex())]"
            }
          ]
        }
      }
    ]
  }
}
```

<span data-ttu-id="aec48-147">Minden egyes virtuálisgép-erőforrást tartalmaz egy hálózati profilt.</span><span class="sxs-lookup"><span data-stu-id="aec48-147">Each virtual machine resource includes a network profile.</span></span> <span data-ttu-id="aec48-148">A hálózati illesztő a virtuális gépet a profil hozzá rendelve.</span><span class="sxs-lookup"><span data-stu-id="aec48-148">The network interface is associated with the virtual machine in this profile.</span></span>  

<span data-ttu-id="aec48-149">Hajtsa végre az erre a hivatkozásra kattintva tekintse meg a JSON-mintát belül a Resource Manager-sablon – [virtuális gép hálózati profil](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L350).</span><span class="sxs-lookup"><span data-stu-id="aec48-149">Follow this link to see the JSON sample within the Resource Manager template – [Virtual Machine Network Profile](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L350).</span></span>

```json
"networkProfile": {
  "networkInterfaces": [
    {
      "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('networkInterfaceNamePrefix'), copyindex()))]"
    }
  ]
}
```

<span data-ttu-id="aec48-150">Azure-portálról a hálózati illesztő illusztráción láthatóhoz hasonló következő.</span><span class="sxs-lookup"><span data-stu-id="aec48-150">From the Azure portal, the network interface looks like the following image.</span></span> <span data-ttu-id="aec48-151">A belső IP-cím és a virtuális gép társítása a hálózati illesztő erőforráson tekinthet meg.</span><span class="sxs-lookup"><span data-stu-id="aec48-151">The internal IP address and the virtual machine association can be seen on the network interface resource.</span></span>

![Hálózati adapter](./media/dotnet-core-2-architecture/nic.png)

<span data-ttu-id="aec48-153">Azure virtuális hálózatokon lévő további információkért lásd: [Azure Virtual Network dokumentáció](https://azure.microsoft.com/documentation/services/virtual-network/).</span><span class="sxs-lookup"><span data-stu-id="aec48-153">For more information on Azure Virtual Networks, see [Azure Virtual Network documentation](https://azure.microsoft.com/documentation/services/virtual-network/).</span></span>

## <a name="azure-sql-database"></a><span data-ttu-id="aec48-154">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="aec48-154">Azure SQL Database</span></span>
<span data-ttu-id="aec48-155">Mellett a zeneáruház webhelyet üzemeltető virtuális gép egy Azure SQL-adatbázis központi telepítése a zene tároló adatbázis.</span><span class="sxs-lookup"><span data-stu-id="aec48-155">In addition to a virtual machine hosting the Music Store website, an Azure SQL Database is deployed to host the Music Store database.</span></span> <span data-ttu-id="aec48-156">Az Azure SQL Database használata itt előnye, hogy nincs szükség a második virtuális gépek csoportja tartalmazza, és a szolgáltatás beépített méretezés és a rendelkezésre állási.</span><span class="sxs-lookup"><span data-stu-id="aec48-156">The advantage of using Azure SQL Database here is that a second set of virtual machines is not required, and scale and availability is built into the service.</span></span>

<span data-ttu-id="aec48-157">Azure SQL-adatbázis a Visual Studio új erőforrás hozzáadása varázsló, vagy érvényes JSON beszúrása egy sablon használatával adhatók meg.</span><span class="sxs-lookup"><span data-stu-id="aec48-157">An Azure SQL database can be added using the Visual Studio Add New Resource wizard, or by inserting valid JSON into a template.</span></span> <span data-ttu-id="aec48-158">Az SQL Server-erőforrást tartalmaz egy felhasználónevet és jelszót, amely az SQL-példányon rendszergazdai jogokkal engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="aec48-158">The SQL Server resource includes a user name and password that is granted administrative rights on the SQL instance.</span></span> <span data-ttu-id="aec48-159">Emellett egy SQL-tűzfal erőforrás kerül.</span><span class="sxs-lookup"><span data-stu-id="aec48-159">Also, a SQL firewall resource is added.</span></span> <span data-ttu-id="aec48-160">Alapértelmezés szerint az Azure-ban üzemeltetett alkalmazások képesek csatlakozni az SQL-példányhoz.</span><span class="sxs-lookup"><span data-stu-id="aec48-160">By default, applications hosted in Azure are able to connect with the SQL instance.</span></span> <span data-ttu-id="aec48-161">Ahhoz, hogy külső alkalmazás ilyen egy SQL Server Management studio segítségével csatlakozzon az SQL-példány, a tűzfal kell megadni.</span><span class="sxs-lookup"><span data-stu-id="aec48-161">To allow external application such a SQL Server Management studio to connect to the SQL instance, the firewall needs to be configured.</span></span> <span data-ttu-id="aec48-162">Az a zeneáruház bemutató rendben az alapértelmezett konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="aec48-162">For the sake of the Music Store demo, the default configuration is fine.</span></span> 

<span data-ttu-id="aec48-163">Hajtsa végre az erre a hivatkozásra kattintva tekintse meg a JSON-mintát belül a Resource Manager-sablon – [Azure SQL Database](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L401).</span><span class="sxs-lookup"><span data-stu-id="aec48-163">Follow this link to see the JSON sample within the Resource Manager template – [Azure SQL DB](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L401).</span></span>

```json
{
  "apiVersion": "2014-04-01-preview",
  "type": "Microsoft.Sql/servers",
  "name": "[variables('musicStoreSqlName')]",
  "location": "[resourceGroup().location]",

  "dependsOn": [],
  "tags": {
    "displayName": "sql-music-store"
  },
  "properties": {
    "administratorLogin": "[parameters('adminUsername')]",
    "administratorLoginPassword": "[parameters('sqlAdminPassword')]"
  },
  "resources": [
    {
      "apiVersion": "2014-04-01-preview",
      "type": "firewallrules",
      "name": "firewall-allow-azure",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Sql/servers/', variables('musicStoreSqlName'))]"
      ],
      "properties": {
        "startIpAddress": "0.0.0.0",
        "endIpAddress": "0.0.0.0"
      }
    }
  ]
}
```

<span data-ttu-id="aec48-164">Az SQL server és adatbázis MusicStore, mint az Azure portálon nézetét.</span><span class="sxs-lookup"><span data-stu-id="aec48-164">A view of the SQL server and MusicStore database as seen in the Azure portal.</span></span>

![SQL Server](./media/dotnet-core-2-architecture/sql.png)

<span data-ttu-id="aec48-166">Az Azure SQL Database telepítésével kapcsolatos további információkért lásd: [dokumentáció az Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/).</span><span class="sxs-lookup"><span data-stu-id="aec48-166">For more information on deploying Azure SQL Database, see [Azure SQL Database documentation](https://azure.microsoft.com/documentation/services/sql-database/).</span></span>

## <a name="next-step"></a><span data-ttu-id="aec48-167">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="aec48-167">Next step</span></span>
<hr>

[<span data-ttu-id="aec48-168">2. lépés - hozzáférés és biztonság az Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="aec48-168">Step 2 - Access and Security in Azure Resource Manager Templates</span></span>](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

