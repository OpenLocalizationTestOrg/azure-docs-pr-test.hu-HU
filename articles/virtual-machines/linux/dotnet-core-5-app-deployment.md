---
title: "Alkalmazás központi telepítése virtuálisgép-bővítmények aaaAutomating |} Microsoft Docs"
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
ms.openlocfilehash: 38a02a4271d6b9ba02a473a51794a7bd90ca3a35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-deployment-with-azure-resource-manager-templates-for-linux-vms"></a>Alkalmazás központi telepítése Azure Resource Manager-sablonok Linux virtuális gépekhez

Miután az összes Azure infrastrukturális követelmények azonosította, és a központi telepítési sablont lefordítva, hello tényleges alkalmazástelepítés címzett toobe van szüksége. Itt az alkalmazás telepítése tooinstalling hello tényleges bináris alkalmazásfájlokat alakzatot Azure-erőforrások hivatkozik. Hello zeneáruház minta .net Core, NGINX és felügyelő kell toobe telepítette és konfigurálta a virtuális gépeken. hello zene tároló bináris toobe hello virtuális gép telepítve kell, és hello zene tároló adatbázis előre létrehozott.

Ez a dokumentum részletesen, hogyan virtuálisgép-bővítmények az alkalmazás központi telepítése és konfigurálása tooAzure virtuális gépek automatizálható. Minden függőségeket és külön konfigurációt vannak kiemelve. Hello legjobb élmény érdekében előtelepítése hello megoldás tooyour Azure-előfizetés és a munka hello Azure Resource Manager-sablon mellett egy példánya. hello teljes sablon – itt található [zene tároló központi telepítését, az Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="configuration-script"></a>Konfigurációs parancsfájl
Virtuálisgép-bővítmények olyan speciális programok, szemben a virtuális gépek tooprovide konfigurációs automation hajtható végre. Számos adott célra, például víruskereső, a naplózási konfiguráció és a Docker konfigurációs bővítmények érhetők el. Egy egyéni parancsprogramok futtatására szolgáló bővítmény használt toorun létrehozott parancsfájlok a virtuális gépek ellen lehet. Hello zeneáruház példával toohello egyéni parancsfájl kiterjesztése tooconfigure hello Ubuntu virtuális gépek és hello zene áruház-alkalmazás telepítése. 

Előtt, és részletesen leírja, hogyan virtuálisgép-bővítmények bejelentették az Azure Resource Manager-sablonok, ellenőrizze a futtatott parancsfájl hello. Ez a parancsfájl hello Ubuntu virtuális gép toohost hello zene áruházból származó alkalmazás konfigurálja. Futtatásakor hello parancsfájl telepíti-e minden szükséges szoftvert, a verziókövetésből hello zene áruház-alkalmazás telepítése és hello adatbázis előkészítése. 

További információ a .net üzemeltető toolearn alapvető alkalmazás Linux, lásd: [közzététel tooa Linux éles környezetben](https://docs.asp.net/en/latest/publishing/linuxproduction.html).

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
Virtuálisgép-bővítmények futtathatja a virtuális gépek elleni build időpontban hello Azure Resource Manager sablon hello bővítmény erőforrás-ot. hello bővítmény hello Visual Studio erőforrás hozzáadása varázsló segítségével, vagy érvényes JSON beszúrása hello sablon adhatók hozzá. hello parancsprogramok futtatására szolgáló bővítmény erőforrás van beágyazva hello virtuálisgép-erőforrás; Ez a példa a következő hello látható.

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [VM parancsprogramok futtatására szolgáló bővítmény](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L359). 

Az alábbi hello megfigyelheti hello parancsfájlok JSON-NÁ. a Githubon tárolja. Ezt a parancsfájlt is tárolható az Azure Blob Storage tárolóban. Azure Resource Manager-sablonok engedélyezése is hello parancsfájl végrehajtásának karakterlánc tooconstructed úgy, hogy a sablon paraméterek értékek parancsfájl végrehajtására használhatók paraméterekként. Ebben az esetben megadva adatai hello sablonok telepítésekor, és ezeket az értékeket felhasználható hello parancsfájl végrehajtása közben.

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

További információ az egyéni parancsprogramok futtatására szolgáló bővítmény hello használatával, lásd: [egyéni parancsfájl-kiterjesztés Resource Manager-sablonok](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="next-step"></a>Következő lépés
<hr>

[Megismerkedhet a több Azure Resource Manager-sablonok](https://github.com/Azure/azure-quickstart-templates)

