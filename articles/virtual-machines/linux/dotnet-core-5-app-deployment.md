---
title: "Virtuálisgép-bővítmények az alkalmazás központi telepítésének automatizálása |} Microsoft Docs"
description: "Azure virtuális gép DotNet fő oktatóanyag"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 9fc8b1ba-60f5-410b-8190-9f1ff885e50e
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2f972fef75aa8e13af7dab908c2b0e2ec28f1324
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="application-deployment-with-azure-resource-manager-templates-for-linux-vms"></a>Alkalmazás központi telepítése Azure Resource Manager-sablonok Linux virtuális gépekhez

Miután az összes Azure infrastrukturális követelmények azonosította, és a központi telepítési sablont lefordítva, a tényleges alkalmazástelepítés kell figyelembe venni. Itt az alkalmazás telepítése hivatkozik a tényleges bináris alkalmazásfájlokat alakzatot Azure-erőforrások telepítése. A zeneáruház minta, a .net Core NGINX és felügyelő kell telepíthetők és konfigurálhatók a virtuális gépeken. Bináris a zene telepítve kell lennie települ a virtuális gépre, és a zeneáruház adatbázist előre létre.

Ez a dokumentum részletesen, hogyan automatizálható virtuálisgép-bővítmények alkalmazás központi telepítése és konfigurálása az Azure virtuális gépekhez. Minden függőségeket és külön konfigurációt vannak kiemelve. A legjobb használhatóság érdekében előtelepítése a megoldást az Azure-előfizetés és a munkahelyi együtt az Azure Resource Manager-sablon egy példányát. A teljes sablon – itt található [zene tároló központi telepítését, az Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="configuration-script"></a>Konfigurációs parancsfájl
Virtuálisgép-bővítmények olyan speciális programok, konfigurációs automation biztosításához a virtuális gépek elleni hajtható végre. Számos adott célra, például víruskereső, a naplózási konfiguráció és a Docker konfigurációs bővítmények érhetők el. Egy egyéni parancsprogramok futtatására szolgáló bővítmény segítségével futtassa a parancsfájlt egy virtuális gépet. A zeneáruház minta esetén az egyéni parancsprogramok futtatására szolgáló bővítmény a Ubuntu virtuális gépek konfigurálásáról és telepítéséről zene áruházból származó alkalmazás. 

Előtt, és részletesen leírja, hogyan vannak deklarálva virtuálisgép-bővítmények az Azure Resource Manager-sablon, vizsgálja meg a futtatni kívánt parancsfájl. Ezt a parancsfájlt a Ubuntu virtuális gép zene áruházból származó alkalmazás futtatására konfigurálható. Futtatásakor, a parancsfájl az összes telepíti a szükséges szoftver, a verziókövetésből zene áruházból származó alkalmazás telepítése, és az adatbázis előkészítése. 

További információt a .net üzemeltető Linux alkalmazás központi telepítéséről [Linux éles környezetben a közzététel](https://docs.asp.net/en/latest/publishing/linuxproduction.html).

> Ez a minta bemutatási célokra van.
> 
> 

```bash
#!/bin/bash

# install dotnet core
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
sudo apt-get update
sudo apt-get install -y dotnet-dev-1.0.0-preview2-003121

# download application
sudo wget https://raw.github.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/music-store-azure-demo-pub.tar /
sudo mkdir /opt/music
sudo tar -xf music-store-azure-demo-pub.tar -C /opt/music

# install nginx, update config file
sudo apt-get install -y nginx
sudo service nginx start
sudo touch /etc/nginx/sites-available/default
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/nginx-config/default -O /etc/nginx/sites-available/default
sudo cp /opt/music/nginx-config/default /etc/nginx/sites-available/
sudo nginx -s reload

# update and secure music config file
sed -i "s/<replaceserver>/$1/g" /opt/music/config.json
sed -i "s/<replaceuser>/$2/g" /opt/music/config.json
sed -i "s/<replacepass>/$3/g" /opt/music/config.json
sudo chown $2 /opt/music/config.json
sudo chmod 0400 /opt/music/config.json

# config supervisor
sudo apt-get install -y supervisor
sudo touch /etc/supervisor/conf.d/music.conf
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/supervisor/music.conf -O /etc/supervisor/conf.d/music.conf
sudo service supervisor stop
sudo service supervisor start

# pre-create music store database
/usr/bin/dotnet /opt/music/MusicStore.dll &
```

## <a name="vm-script-extension"></a>Virtuális gép parancsprogramok futtatására szolgáló bővítmény
Virtuálisgép-bővítmények futtathatja a virtuális gépek elleni build időpontban a bővítmény erőforrás belefoglalja az Azure Resource Manager-sablon. A bővítmény a Visual Studio erőforrás hozzáadása varázsló segítségével, vagy érvényes JSON beszúrása a sablon lehet hozzáadni. A parancsprogramok futtatására szolgáló bővítmény erőforrás van beágyazva a virtuálisgép-erőforrás; Ez az alábbi példában látható.

Hajtsa végre az erre a hivatkozásra kattintva tekintse meg a JSON-mintát belül a Resource Manager-sablon – [VM parancsprogramok futtatására szolgáló bővítmény](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L359). 

Láthatja az alábbi JSON, hogy a parancsfájl a Githubon található. Ezt a parancsfájlt is tárolható az Azure Blob Storage tárolóban. Azure Resource Manager-sablonok is, a parancsfájl végrehajtási karakterláncot megtervezni, hogy sablon paraméterek értékek parancsfájl végrehajtására használhatók paraméterekként engedélyezése. Ebben az esetben a adat rendelkezésre áll-e a sablonok telepítésekor, és ezeket az értékeket ezután felhasználhatók, a parancsfájl végrehajtása közben.

```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

Az egyéni parancsprogramok futtatására szolgáló bővítmény használatával kapcsolatos további információkért lásd: [egyéni parancsfájl-kiterjesztés Resource Manager-sablonok](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="next-step"></a>Következő lépés
<hr>

[Megismerkedhet a több Azure Resource Manager-sablonok](https://github.com/Azure/azure-quickstart-templates)

