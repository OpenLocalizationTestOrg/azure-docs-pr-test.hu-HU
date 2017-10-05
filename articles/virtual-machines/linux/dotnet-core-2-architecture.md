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
# <a name="application-architecture-with-azure-resource-manager-templates-for-linux-vms"></a>Linux virtuális gépek Azure Resource Manager-sablonok alkalmazásarchitektúra

Az Azure Resource Manager deployment fejlesztésekor számítási követelmények kell Azure-erőforrások és szolgáltatások kell hozzárendelni. Ha egy alkalmazás több http-végpontokról, egy adatbázist és egy szolgáltatás gyorsítótárazás adatokat tartalmaz, az Azure-erőforrások üzemeltető minden ezeket az összetevőket kell rationalized lehet. Például a zeneáruház mintaalkalmazás virtuális gépen futtatott webalkalmazás, és egy SQL-adatbázis, amely az Azure SQL-adatbázis található. 

Ez a dokumentum részletesen, hogyan állíthatók be a minta Azure Resource Manager-sablon a zeneáruház számítási erőforrásokat. Minden függőségeket és külön konfigurációt vannak kiemelve. A legjobb használhatóság érdekében előtelepítése a megoldást az Azure-előfizetés és a munkahelyi együtt az Azure Resource Manager-sablon egy példányát. A teljes sablon – itt található [zene tároló központi telepítését, az Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux). 

## <a name="virtual-machine"></a>Virtuális gép
Zene áruházból származó alkalmazás egy webalkalmazást, ahol az ügyfelek és vásárolhat zene tartalmazza. Miközben Számos Azure szolgáltatást, amely a webes alkalmazás, például egy virtuális gép használja. A minta zeneáruház sablont használ, a virtuális gép telepítve van, egy webkiszolgáló telepítése és a zeneáruház webhely telepítése és konfigurálása. Ez a cikk az csak a virtuális gép központi telepítés részleteit. A webalkalmazás-kiszolgáló és az alkalmazás konfigurációja egy újabb cikkben részleteit.

A virtuális gépek felveheti egy sablont a Visual Studio új erőforrás hozzáadása varázsló használatával, vagy érvényes JSON beszúrása a központi telepítési sablont. Virtuális gépek telepítésekor számos kapcsolódó erőforrások is szükséges. A sablon létrehozása a Visual Studio használatával, ha ezeket az erőforrásokat jön létre. Ha manuálisan hoz létre, a sablon, ezeket az erőforrásokat kell szúrja be, és konfigurálva.

Hajtsa végre az erre a hivatkozásra kattintva tekintse meg a JSON-mintát belül a Resource Manager-sablon – [virtuális gép JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L295).

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

Amennyiben telepített, a virtuális gép tulajdonságai láthatók az Azure-portálon.

![Virtuális gép](./media/dotnet-core-2-architecture/vm.png)

## <a name="storage-account"></a>Tárfiók
Storage-fiókok rendelkezik számos tárolási lehetőségeket és képességeket. A környezet az Azure virtuális gépek a tárfiók tárolja a virtuális merevlemezek a virtuális gép és bármelyik adatlemeznek. A zeneáruház minta magában foglalja a központi telepítésben az egyes virtuális gépek virtuális merevlemez tárolásához egy tárfiókot. 

Hajtsa végre az erre a hivatkozásra kattintva tekintse meg a JSON-mintát belül a Resource Manager-sablon – [Tárfiók](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L109).

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

A storage-fiók társítása egy virtuális géppel a virtuális gép Resource Manager sablon deklarációjában belül. 

Hajtsa végre az erre a hivatkozásra kattintva tekintse meg a JSON-mintát belül a Resource Manager-sablon – [virtuális gép és a Storage-fiókhoz társításának](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L341).

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

A központi telepítést követően a tárfiók tekintheti meg az Azure portálon.

![Tárfiók](./media/dotnet-core-2-architecture/storacct.png)

Kattintson a fiók blob tároló, a virtuális merevlemez-meghajtóról fájl sablonnal telepített virtuális gépek láthatók.

![Virtuális merevlemezek](./media/dotnet-core-2-architecture/vhd.png)

További információ az Azure Storage: [Azure Storage-dokumentációt](https://azure.microsoft.com/documentation/services/storage/).

## <a name="virtual-network"></a>Virtual Network
Ha egy virtuális géphez szükséges például képes kommunikálni más virtuális gépek és az Azure-erőforrások belső hálózat, a egy Azure virtuális hálózatra szükség.  Virtuális hálózat tegye a virtuális gép elérhetővé az interneten keresztül. Nyilvános egy nyilvános IP-címet, amely ezen részében részletes szükséges.

Hajtsa végre az erre a hivatkozásra kattintva tekintse meg a JSON-mintát belül a Resource Manager-sablon – [virtuális hálózat és alhálózat](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L136).

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

Azure-portálról a virtuális hálózati illusztráción láthatóhoz hasonló következő. Figyelje meg, hogy a virtuális hálózat összes virtuális gépet a sablon használatával telepített vannak csatlakoztatva.

![Virtual Network](./media/dotnet-core-2-architecture/vnet.png)

## <a name="network-interface"></a>Hálózati adapter
 A hálózati adaptert egy virtuális hálózathoz, pontosabban, hogy definiálva van a virtuális hálózati alhálózat csatlakoztat egy virtuális gépet. 

 Hajtsa végre az erre a hivatkozásra kattintva tekintse meg a JSON-mintát belül a Resource Manager-sablon – [hálózati illesztő](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L166).

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

Minden egyes virtuálisgép-erőforrást tartalmaz egy hálózati profilt. A hálózati illesztő a virtuális gépet a profil hozzá rendelve.  

Hajtsa végre az erre a hivatkozásra kattintva tekintse meg a JSON-mintát belül a Resource Manager-sablon – [virtuális gép hálózati profil](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L350).

```json
"networkProfile": {
  "networkInterfaces": [
    {
      "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('networkInterfaceNamePrefix'), copyindex()))]"
    }
  ]
}
```

Azure-portálról a hálózati illesztő illusztráción láthatóhoz hasonló következő. A belső IP-cím és a virtuális gép társítása a hálózati illesztő erőforráson tekinthet meg.

![Hálózati adapter](./media/dotnet-core-2-architecture/nic.png)

Azure virtuális hálózatokon lévő további információkért lásd: [Azure Virtual Network dokumentáció](https://azure.microsoft.com/documentation/services/virtual-network/).

## <a name="azure-sql-database"></a>Azure SQL Database
Mellett a zeneáruház webhelyet üzemeltető virtuális gép egy Azure SQL-adatbázis központi telepítése a zene tároló adatbázis. Az Azure SQL Database használata itt előnye, hogy nincs szükség a második virtuális gépek csoportja tartalmazza, és a szolgáltatás beépített méretezés és a rendelkezésre állási.

Azure SQL-adatbázis a Visual Studio új erőforrás hozzáadása varázsló, vagy érvényes JSON beszúrása egy sablon használatával adhatók meg. Az SQL Server-erőforrást tartalmaz egy felhasználónevet és jelszót, amely az SQL-példányon rendszergazdai jogokkal engedélyezett. Emellett egy SQL-tűzfal erőforrás kerül. Alapértelmezés szerint az Azure-ban üzemeltetett alkalmazások képesek csatlakozni az SQL-példányhoz. Ahhoz, hogy külső alkalmazás ilyen egy SQL Server Management studio segítségével csatlakozzon az SQL-példány, a tűzfal kell megadni. Az a zeneáruház bemutató rendben az alapértelmezett konfigurációt. 

Hajtsa végre az erre a hivatkozásra kattintva tekintse meg a JSON-mintát belül a Resource Manager-sablon – [Azure SQL Database](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L401).

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

Az SQL server és adatbázis MusicStore, mint az Azure portálon nézetét.

![SQL Server](./media/dotnet-core-2-architecture/sql.png)

Az Azure SQL Database telepítésével kapcsolatos további információkért lásd: [dokumentáció az Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/).

## <a name="next-step"></a>Következő lépés
<hr>

[2. lépés - hozzáférés és biztonság az Azure Resource Manager-sablonok](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

