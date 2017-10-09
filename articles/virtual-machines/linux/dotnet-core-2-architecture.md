---
title: "Linux számítási erőforrások az Azure Resource Manager-sablonok aaaDeploying |} Microsoft Docs"
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
ms.openlocfilehash: 0bc26805860fed47923d46fc84f357060f68a951
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-architecture-with-azure-resource-manager-templates-for-linux-vms"></a><span data-ttu-id="f3207-103">Linux virtuális gépek Azure Resource Manager-sablonok alkalmazásarchitektúra</span><span class="sxs-lookup"><span data-stu-id="f3207-103">Application architecture with Azure Resource Manager templates for Linux VMs</span></span>

<span data-ttu-id="f3207-104">Az Azure Resource Manager deployment fejlesztésekor számítási követelményeinek leképezése toobe tooAzure erőforrásokat és szolgáltatásokat kell.</span><span class="sxs-lookup"><span data-stu-id="f3207-104">When developing an Azure Resource Manager deployment, compute requirements need toobe mapped tooAzure resources and services.</span></span> <span data-ttu-id="f3207-105">Ha egy alkalmazás több http-végpontokról, egy adatbázist és egy szolgáltatás gyorsítótárazás adatokat tartalmaz, hello Azure-erőforrások, amely ezeket az összetevőket tárolni kell toobe rationalized.</span><span class="sxs-lookup"><span data-stu-id="f3207-105">If an application consists of several http endpoints, a database, and a data caching service, hello Azure resources that host each of these components needs toobe rationalized.</span></span> <span data-ttu-id="f3207-106">Például hello zeneáruház mintaalkalmazás virtuális gépen futtatott webalkalmazás, és egy SQL-adatbázis, amely az Azure SQL-adatbázis található.</span><span class="sxs-lookup"><span data-stu-id="f3207-106">For instance, hello sample Music Store application includes a web application that is hosted on a virtual machine, and a SQL database, which is hosted in Azure SQL database.</span></span> 

<span data-ttu-id="f3207-107">Ez a dokumentum részletesen, hogyan hello minta Azure Resource Manager-sablon hello zeneáruház számítási erőforrások vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="f3207-107">This document details how hello Music Store compute resources are configured in hello sample Azure Resource Manager template.</span></span> <span data-ttu-id="f3207-108">Minden függőségeket és külön konfigurációt vannak kiemelve.</span><span class="sxs-lookup"><span data-stu-id="f3207-108">All dependencies and unique configurations are highlighted.</span></span> <span data-ttu-id="f3207-109">Hello legjobb élmény érdekében előtelepítése hello megoldás tooyour Azure-előfizetés és a munka hello Azure Resource Manager-sablon mellett egy példánya.</span><span class="sxs-lookup"><span data-stu-id="f3207-109">For hello best experience, pre-deploy an instance of hello solution tooyour Azure subscription and work along with hello Azure Resource Manager template.</span></span> <span data-ttu-id="f3207-110">hello teljes sablon – itt található [zene tároló központi telepítését, az Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span><span class="sxs-lookup"><span data-stu-id="f3207-110">hello complete template can be found here – [Music Store Deployment on Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span></span> 

## <a name="virtual-machine"></a><span data-ttu-id="f3207-111">Virtuális gép</span><span class="sxs-lookup"><span data-stu-id="f3207-111">Virtual Machine</span></span>
<span data-ttu-id="f3207-112">hello zene áruház-alkalmazás egy webalkalmazást, ahol az ügyfelek és vásárolhat zene tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f3207-112">hello Music Store application includes a web application where customers can browse and purchase music.</span></span> <span data-ttu-id="f3207-113">Miközben Számos Azure szolgáltatást, amely a webes alkalmazás, például egy virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="f3207-113">While there are several Azure services that can host web applications, for this example, a Virtual Machine is used.</span></span> <span data-ttu-id="f3207-114">Hello zeneáruház mintasablon használatával, a virtuális gép telepítve van, egy webkiszolgáló telepítése és hello zeneáruház webhely telepítése és konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="f3207-114">Using hello sample Music Store template, a virtual machine is deployed, a web server install, and hello Music Store website installed and configured.</span></span> <span data-ttu-id="f3207-115">Ez a cikk hello szakét csak a hello virtuális gépek telepítése során részleteit.</span><span class="sxs-lookup"><span data-stu-id="f3207-115">For hello sake of this article, only hello virtual machine deployment is detailed.</span></span> <span data-ttu-id="f3207-116">hello konfigurálása hello webkiszolgáló és hello alkalmazás egy újabb cikkben részleteit.</span><span class="sxs-lookup"><span data-stu-id="f3207-116">hello configuration of hello web server and hello application is detailed in a later article.</span></span>

<span data-ttu-id="f3207-117">A virtuális gép tooa sablont hello Visual Studio új erőforrás hozzáadása varázsló, vagy érvényes JSON beszúrása hello központi telepítési sablon használatával lehet hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="f3207-117">A virtual machine can be added tooa template using hello Visual Studio Add New Resource wizard, or by inserting valid JSON into hello deployment template.</span></span> <span data-ttu-id="f3207-118">Virtuális gépek telepítésekor számos kapcsolódó erőforrások is szükséges.</span><span class="sxs-lookup"><span data-stu-id="f3207-118">When deploying a virtual machine, several related resources are also needed.</span></span> <span data-ttu-id="f3207-119">Ha a Visual Studio toocreate hello sablonnal, ezeket az erőforrásokat jön létre.</span><span class="sxs-lookup"><span data-stu-id="f3207-119">If using Visual Studio toocreate hello template, these resources are created for you.</span></span> <span data-ttu-id="f3207-120">Ha manuálisan hoz létre, hello sablon, ezeket az erőforrásokat kell toobe szúrja be, és konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="f3207-120">If manually constructing hello template, these resources need toobe inserted and configured.</span></span>

<span data-ttu-id="f3207-121">Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [virtuális gép JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L295).</span><span class="sxs-lookup"><span data-stu-id="f3207-121">Follow this link toosee hello JSON sample within hello Resource Manager template – [Virtual Machine JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L295).</span></span>

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

<span data-ttu-id="f3207-122">Amennyiben a telepített, hello Azure-portálon látható a hello virtuálisgép-tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="f3207-122">Once deployed, hello virtual machine properties can be seen in hello Azure portal.</span></span>

![Virtuális gép](./media/dotnet-core-2-architecture/vm.png)

## <a name="storage-account"></a><span data-ttu-id="f3207-124">Tárfiók</span><span class="sxs-lookup"><span data-stu-id="f3207-124">Storage Account</span></span>
<span data-ttu-id="f3207-125">Storage-fiókok rendelkezik számos tárolási lehetőségeket és képességeket.</span><span class="sxs-lookup"><span data-stu-id="f3207-125">Storage accounts have many storage options and capabilities.</span></span> <span data-ttu-id="f3207-126">Hello környezet Azure virtuális gépek a tárfiók tárolja a hello virtuális merevlemezek hello virtuális gép és bármelyik adatlemeznek.</span><span class="sxs-lookup"><span data-stu-id="f3207-126">For hello context of Azure Virtual machines, a storage account holds hello virtual hard drives of hello virtual machine and any additional data disks.</span></span> <span data-ttu-id="f3207-127">hello zeneáruház minta egy tárolási fiók toohold hello virtuális merevlemez-meghajtóról minden virtuális gép hello telepítési tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f3207-127">hello Music Store sample includes one storage account toohold hello virtual hard drive of each virtual machine in hello deployment.</span></span> 

<span data-ttu-id="f3207-128">Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [Tárfiók](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L109).</span><span class="sxs-lookup"><span data-stu-id="f3207-128">Follow this link toosee hello JSON sample within hello Resource Manager template – [Storage Account](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L109).</span></span>

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

<span data-ttu-id="f3207-129">A storage-fiók társítása a virtuális gépekkel belül hello Resource Manager sablon deklaráció hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="f3207-129">A storage account is associate with a virtual machine inside hello Resource Manager template declaration of hello virtual machine.</span></span> 

<span data-ttu-id="f3207-130">Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [virtuális gép és a Storage-fiókhoz társításának](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L341).</span><span class="sxs-lookup"><span data-stu-id="f3207-130">Follow this link toosee hello JSON sample within hello Resource Manager template – [Virtual Machine and Storage Account association](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L341).</span></span>

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

<span data-ttu-id="f3207-131">A központi telepítést követően hello tárfiók tekinthető hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="f3207-131">After deployment, hello storage account can be viewed in hello Azure portal.</span></span>

![Tárfiók](./media/dotnet-core-2-architecture/storacct.png)

<span data-ttu-id="f3207-133">Kattintson hello fiók tárolóra, hello virtuális merevlemezfájlra hello sablonnal telepített virtuális gépek láthatók.</span><span class="sxs-lookup"><span data-stu-id="f3207-133">Clicking into hello storage account blob container, hello virtual hard drive file for each virtual machine deployed with hello template can be seen.</span></span>

![Virtuális merevlemezek](./media/dotnet-core-2-architecture/vhd.png)

<span data-ttu-id="f3207-135">További információ az Azure Storage: [Azure Storage-dokumentációt](https://azure.microsoft.com/documentation/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="f3207-135">For more information on Azure Storage, see [Azure Storage documentation](https://azure.microsoft.com/documentation/services/storage/).</span></span>

## <a name="virtual-network"></a><span data-ttu-id="f3207-136">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="f3207-136">Virtual Network</span></span>
<span data-ttu-id="f3207-137">Ha egy virtuális géphez szükséges például hello képességét toocommunicate más virtuális gépek és az Azure-erőforrások belső hálózat, a egy Azure virtuális hálózatra szükség.</span><span class="sxs-lookup"><span data-stu-id="f3207-137">If a virtual machine requires internal networking such as hello ability toocommunicate with other virtual machines and Azure resources, an Azure Virtual Network is required.</span></span>  <span data-ttu-id="f3207-138">Virtuális hálózat nem tesz hello virtuális gép keresztül elérhető hello internet.</span><span class="sxs-lookup"><span data-stu-id="f3207-138">A virtual network does not make hello virtual machine accessible over hello internet.</span></span> <span data-ttu-id="f3207-139">Nyilvános egy nyilvános IP-címet, amely ezen részében részletes szükséges.</span><span class="sxs-lookup"><span data-stu-id="f3207-139">Public connectivity requires a public IP address, which is detailed later in this series.</span></span>

<span data-ttu-id="f3207-140">Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [virtuális hálózat és alhálózat](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L136).</span><span class="sxs-lookup"><span data-stu-id="f3207-140">Follow this link toosee hello JSON sample within hello Resource Manager template – [Virtual Network and Subnets](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L136).</span></span>

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

<span data-ttu-id="f3207-141">Hello Azure-portálon, a virtuális hálózati hello hello kép a következő tűnik.</span><span class="sxs-lookup"><span data-stu-id="f3207-141">From hello Azure portal, hello virtual network looks like hello following image.</span></span> <span data-ttu-id="f3207-142">Figyelje meg, hogy a hello sablonnal telepített összes virtuális gép virtuális hálózathoz csatolt toohello.</span><span class="sxs-lookup"><span data-stu-id="f3207-142">Notice that all virtual machines deployed with hello template are attached toohello virtual network.</span></span>

![Virtual Network](./media/dotnet-core-2-architecture/vnet.png)

## <a name="network-interface"></a><span data-ttu-id="f3207-144">Hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="f3207-144">Network Interface</span></span>
 <span data-ttu-id="f3207-145">Egy adott hálózati csatoló kapcsolódik a virtuális gép tooa virtuális hálózatra, pontosabban tooa-alhálózatot, amely a virtuális hálózati hello definiálva van.</span><span class="sxs-lookup"><span data-stu-id="f3207-145">A network interface connects a virtual machine tooa virtual network, more specifically tooa subnet that has been defined in hello virtual network.</span></span> 

 <span data-ttu-id="f3207-146">Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [hálózati illesztő](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L166).</span><span class="sxs-lookup"><span data-stu-id="f3207-146">Follow this link toosee hello JSON sample within hello Resource Manager template – [Network Interface](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L166).</span></span>

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

<span data-ttu-id="f3207-147">Minden egyes virtuálisgép-erőforrást tartalmaz egy hálózati profilt.</span><span class="sxs-lookup"><span data-stu-id="f3207-147">Each virtual machine resource includes a network profile.</span></span> <span data-ttu-id="f3207-148">hello hálózati illesztő hello virtuális géppel a profil hozzá rendelve.</span><span class="sxs-lookup"><span data-stu-id="f3207-148">hello network interface is associated with hello virtual machine in this profile.</span></span>  

<span data-ttu-id="f3207-149">Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [virtuális gép hálózati profil](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L350).</span><span class="sxs-lookup"><span data-stu-id="f3207-149">Follow this link toosee hello JSON sample within hello Resource Manager template – [Virtual Machine Network Profile](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L350).</span></span>

```json
"networkProfile": {
  "networkInterfaces": [
    {
      "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('networkInterfaceNamePrefix'), copyindex()))]"
    }
  ]
}
```

<span data-ttu-id="f3207-150">Hello Azure-portálon, a hello hálózati adapter a következő kép hello tűnik.</span><span class="sxs-lookup"><span data-stu-id="f3207-150">From hello Azure portal, hello network interface looks like hello following image.</span></span> <span data-ttu-id="f3207-151">hello belső IP-cím és a virtuális gép társítása hello hello hálózati illesztő erőforráson tekinthet meg.</span><span class="sxs-lookup"><span data-stu-id="f3207-151">hello internal IP address and hello virtual machine association can be seen on hello network interface resource.</span></span>

![Hálózati adapter](./media/dotnet-core-2-architecture/nic.png)

<span data-ttu-id="f3207-153">Azure virtuális hálózatokon lévő további információkért lásd: [Azure Virtual Network dokumentáció](https://azure.microsoft.com/documentation/services/virtual-network/).</span><span class="sxs-lookup"><span data-stu-id="f3207-153">For more information on Azure Virtual Networks, see [Azure Virtual Network documentation](https://azure.microsoft.com/documentation/services/virtual-network/).</span></span>

## <a name="azure-sql-database"></a><span data-ttu-id="f3207-154">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="f3207-154">Azure SQL Database</span></span>
<span data-ttu-id="f3207-155">Ezenkívül tooa virtuálisgép üzemeltetési hello zeneáruház webhely, az Azure SQL Database egy telepített toohost hello zene tároló adatbázis.</span><span class="sxs-lookup"><span data-stu-id="f3207-155">In addition tooa virtual machine hosting hello Music Store website, an Azure SQL Database is deployed toohost hello Music Store database.</span></span> <span data-ttu-id="f3207-156">hello Azure SQL Database használata itt előnye, hogy a virtuális gépek egy második együttesét nincs szükség, és a méretezés és a rendelkezésre állási hello szolgáltatás be van építve.</span><span class="sxs-lookup"><span data-stu-id="f3207-156">hello advantage of using Azure SQL Database here is that a second set of virtual machines is not required, and scale and availability is built into hello service.</span></span>

<span data-ttu-id="f3207-157">Azure SQL-adatbázis hello Visual Studio új erőforrás hozzáadása varázsló, vagy érvényes JSON beszúrása egy sablon használatával adhatók meg.</span><span class="sxs-lookup"><span data-stu-id="f3207-157">An Azure SQL database can be added using hello Visual Studio Add New Resource wizard, or by inserting valid JSON into a template.</span></span> <span data-ttu-id="f3207-158">hello SQL Server-erőforrást tartalmaz egy felhasználónevet és jelszót, amely rendszergazdai jogosultságokkal az SQL-példányában hello engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="f3207-158">hello SQL Server resource includes a user name and password that is granted administrative rights on hello SQL instance.</span></span> <span data-ttu-id="f3207-159">Emellett egy SQL-tűzfal erőforrás kerül.</span><span class="sxs-lookup"><span data-stu-id="f3207-159">Also, a SQL firewall resource is added.</span></span> <span data-ttu-id="f3207-160">Alapértelmezés szerint az Azure-ban üzemeltetett alkalmazások értékek képes tooconnect hello SQL-példánnyal.</span><span class="sxs-lookup"><span data-stu-id="f3207-160">By default, applications hosted in Azure are able tooconnect with hello SQL instance.</span></span> <span data-ttu-id="f3207-161">tooallow külső alkalmazás ilyen egy SQL Server Management studio tooconnect toohello SQL-példányhoz, hello tűzfal toobe konfigurálni kell.</span><span class="sxs-lookup"><span data-stu-id="f3207-161">tooallow external application such a SQL Server Management studio tooconnect toohello SQL instance, hello firewall needs toobe configured.</span></span> <span data-ttu-id="f3207-162">A hello érthetősége hello zeneáruház bemutató hello alapértelmezett konfiguráció rendben.</span><span class="sxs-lookup"><span data-stu-id="f3207-162">For hello sake of hello Music Store demo, hello default configuration is fine.</span></span> 

<span data-ttu-id="f3207-163">Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [Azure SQL Database](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L401).</span><span class="sxs-lookup"><span data-stu-id="f3207-163">Follow this link toosee hello JSON sample within hello Resource Manager template – [Azure SQL DB](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L401).</span></span>

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

<span data-ttu-id="f3207-164">Egy nézet hello SQL server és MusicStore adatbázis hello Azure-portálon látható módon.</span><span class="sxs-lookup"><span data-stu-id="f3207-164">A view of hello SQL server and MusicStore database as seen in hello Azure portal.</span></span>

![SQL Server](./media/dotnet-core-2-architecture/sql.png)

<span data-ttu-id="f3207-166">Az Azure SQL Database telepítésével kapcsolatos további információkért lásd: [dokumentáció az Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/).</span><span class="sxs-lookup"><span data-stu-id="f3207-166">For more information on deploying Azure SQL Database, see [Azure SQL Database documentation](https://azure.microsoft.com/documentation/services/sql-database/).</span></span>

## <a name="next-step"></a><span data-ttu-id="f3207-167">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="f3207-167">Next step</span></span>
<hr>

[<span data-ttu-id="f3207-168">2. lépés - hozzáférés és biztonság az Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="f3207-168">Step 2 - Access and Security in Azure Resource Manager Templates</span></span>](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

