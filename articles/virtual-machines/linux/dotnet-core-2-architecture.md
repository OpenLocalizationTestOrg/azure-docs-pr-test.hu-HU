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
# <a name="application-architecture-with-azure-resource-manager-templates-for-linux-vms"></a>Linux virtuális gépek Azure Resource Manager-sablonok alkalmazásarchitektúra

Az Azure Resource Manager deployment fejlesztésekor számítási követelményeinek leképezése toobe tooAzure erőforrásokat és szolgáltatásokat kell. Ha egy alkalmazás több http-végpontokról, egy adatbázist és egy szolgáltatás gyorsítótárazás adatokat tartalmaz, hello Azure-erőforrások, amely ezeket az összetevőket tárolni kell toobe rationalized. Például hello zeneáruház mintaalkalmazás virtuális gépen futtatott webalkalmazás, és egy SQL-adatbázis, amely az Azure SQL-adatbázis található. 

Ez a dokumentum részletesen, hogyan hello minta Azure Resource Manager-sablon hello zeneáruház számítási erőforrások vannak konfigurálva. Minden függőségeket és külön konfigurációt vannak kiemelve. Hello legjobb élmény érdekében előtelepítése hello megoldás tooyour Azure-előfizetés és a munka hello Azure Resource Manager-sablon mellett egy példánya. hello teljes sablon – itt található [zene tároló központi telepítését, az Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux). 

## <a name="virtual-machine"></a>Virtuális gép
hello zene áruház-alkalmazás egy webalkalmazást, ahol az ügyfelek és vásárolhat zene tartalmazza. Miközben Számos Azure szolgáltatást, amely a webes alkalmazás, például egy virtuális gép használja. Hello zeneáruház mintasablon használatával, a virtuális gép telepítve van, egy webkiszolgáló telepítése és hello zeneáruház webhely telepítése és konfigurálása. Ez a cikk hello szakét csak a hello virtuális gépek telepítése során részleteit. hello konfigurálása hello webkiszolgáló és hello alkalmazás egy újabb cikkben részleteit.

A virtuális gép tooa sablont hello Visual Studio új erőforrás hozzáadása varázsló, vagy érvényes JSON beszúrása hello központi telepítési sablon használatával lehet hozzáadni. Virtuális gépek telepítésekor számos kapcsolódó erőforrások is szükséges. Ha a Visual Studio toocreate hello sablonnal, ezeket az erőforrásokat jön létre. Ha manuálisan hoz létre, hello sablon, ezeket az erőforrásokat kell toobe szúrja be, és konfigurálva.

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [virtuális gép JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L295).

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

Amennyiben a telepített, hello Azure-portálon látható a hello virtuálisgép-tulajdonságokat.

![Virtuális gép](./media/dotnet-core-2-architecture/vm.png)

## <a name="storage-account"></a>Tárfiók
Storage-fiókok rendelkezik számos tárolási lehetőségeket és képességeket. Hello környezet Azure virtuális gépek a tárfiók tárolja a hello virtuális merevlemezek hello virtuális gép és bármelyik adatlemeznek. hello zeneáruház minta egy tárolási fiók toohold hello virtuális merevlemez-meghajtóról minden virtuális gép hello telepítési tartalmazza. 

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [Tárfiók](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L109).

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

A storage-fiók társítása a virtuális gépekkel belül hello Resource Manager sablon deklaráció hello virtuális gép. 

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [virtuális gép és a Storage-fiókhoz társításának](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L341).

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

A központi telepítést követően hello tárfiók tekinthető hello Azure-portálon.

![Tárfiók](./media/dotnet-core-2-architecture/storacct.png)

Kattintson hello fiók tárolóra, hello virtuális merevlemezfájlra hello sablonnal telepített virtuális gépek láthatók.

![Virtuális merevlemezek](./media/dotnet-core-2-architecture/vhd.png)

További információ az Azure Storage: [Azure Storage-dokumentációt](https://azure.microsoft.com/documentation/services/storage/).

## <a name="virtual-network"></a>Virtual Network
Ha egy virtuális géphez szükséges például hello képességét toocommunicate más virtuális gépek és az Azure-erőforrások belső hálózat, a egy Azure virtuális hálózatra szükség.  Virtuális hálózat nem tesz hello virtuális gép keresztül elérhető hello internet. Nyilvános egy nyilvános IP-címet, amely ezen részében részletes szükséges.

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [virtuális hálózat és alhálózat](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L136).

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

Hello Azure-portálon, a virtuális hálózati hello hello kép a következő tűnik. Figyelje meg, hogy a hello sablonnal telepített összes virtuális gép virtuális hálózathoz csatolt toohello.

![Virtual Network](./media/dotnet-core-2-architecture/vnet.png)

## <a name="network-interface"></a>Hálózati adapter
 Egy adott hálózati csatoló kapcsolódik a virtuális gép tooa virtuális hálózatra, pontosabban tooa-alhálózatot, amely a virtuális hálózati hello definiálva van. 

 Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [hálózati illesztő](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L166).

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

Minden egyes virtuálisgép-erőforrást tartalmaz egy hálózati profilt. hello hálózati illesztő hello virtuális géppel a profil hozzá rendelve.  

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [virtuális gép hálózati profil](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L350).

```json
"networkProfile": {
  "networkInterfaces": [
    {
      "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('networkInterfaceNamePrefix'), copyindex()))]"
    }
  ]
}
```

Hello Azure-portálon, a hello hálózati adapter a következő kép hello tűnik. hello belső IP-cím és a virtuális gép társítása hello hello hálózati illesztő erőforráson tekinthet meg.

![Hálózati adapter](./media/dotnet-core-2-architecture/nic.png)

Azure virtuális hálózatokon lévő további információkért lásd: [Azure Virtual Network dokumentáció](https://azure.microsoft.com/documentation/services/virtual-network/).

## <a name="azure-sql-database"></a>Azure SQL Database
Ezenkívül tooa virtuálisgép üzemeltetési hello zeneáruház webhely, az Azure SQL Database egy telepített toohost hello zene tároló adatbázis. hello Azure SQL Database használata itt előnye, hogy a virtuális gépek egy második együttesét nincs szükség, és a méretezés és a rendelkezésre állási hello szolgáltatás be van építve.

Azure SQL-adatbázis hello Visual Studio új erőforrás hozzáadása varázsló, vagy érvényes JSON beszúrása egy sablon használatával adhatók meg. hello SQL Server-erőforrást tartalmaz egy felhasználónevet és jelszót, amely rendszergazdai jogosultságokkal az SQL-példányában hello engedélyezett. Emellett egy SQL-tűzfal erőforrás kerül. Alapértelmezés szerint az Azure-ban üzemeltetett alkalmazások értékek képes tooconnect hello SQL-példánnyal. tooallow külső alkalmazás ilyen egy SQL Server Management studio tooconnect toohello SQL-példányhoz, hello tűzfal toobe konfigurálni kell. A hello érthetősége hello zeneáruház bemutató hello alapértelmezett konfiguráció rendben. 

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [Azure SQL Database](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L401).

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

Egy nézet hello SQL server és MusicStore adatbázis hello Azure-portálon látható módon.

![SQL Server](./media/dotnet-core-2-architecture/sql.png)

Az Azure SQL Database telepítésével kapcsolatos további információkért lásd: [dokumentáció az Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/).

## <a name="next-step"></a>Következő lépés
<hr>

[2. lépés - hozzáférés és biztonság az Azure Resource Manager-sablonok](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

