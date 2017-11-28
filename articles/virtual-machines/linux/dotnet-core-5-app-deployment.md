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
# <a name="application-deployment-with-azure-resource-manager-templates-for-linux-vms"></a><span data-ttu-id="2c24d-103">Alkalmazás központi telepítése Azure Resource Manager-sablonok Linux virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="2c24d-103">Application deployment with Azure Resource Manager templates for Linux VMs</span></span>

<span data-ttu-id="2c24d-104">Miután az összes Azure infrastrukturális követelmények azonosította, és a központi telepítési sablont lefordítva, hello tényleges alkalmazástelepítés címzett toobe van szüksége.</span><span class="sxs-lookup"><span data-stu-id="2c24d-104">Once all Azure infrastructural requirements have been identified and translated into a deployment template, hello actual application deployment needs toobe addressed.</span></span> <span data-ttu-id="2c24d-105">Itt az alkalmazás telepítése tooinstalling hello tényleges bináris alkalmazásfájlokat alakzatot Azure-erőforrások hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="2c24d-105">Application deployment here is referring tooinstalling hello actual application binaries onto Azure resources.</span></span> <span data-ttu-id="2c24d-106">Hello zeneáruház minta .net Core, NGINX és felügyelő kell toobe telepítette és konfigurálta a virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="2c24d-106">For hello Music Store sample, .Net Core, NGINX, and Supervisor need toobe installed and configured on each virtual machine.</span></span> <span data-ttu-id="2c24d-107">hello zene tároló bináris toobe hello virtuális gép telepítve kell, és hello zene tároló adatbázis előre létrehozott.</span><span class="sxs-lookup"><span data-stu-id="2c24d-107">hello Music Store binaries need toobe installed onto hello virtual machine, and hello Music Store database pre-created.</span></span>

<span data-ttu-id="2c24d-108">Ez a dokumentum részletesen, hogyan virtuálisgép-bővítmények az alkalmazás központi telepítése és konfigurálása tooAzure virtuális gépek automatizálható.</span><span class="sxs-lookup"><span data-stu-id="2c24d-108">This document details how Virtual Machine extensions can automate application deployment and configuration tooAzure virtual machines.</span></span> <span data-ttu-id="2c24d-109">Minden függőségeket és külön konfigurációt vannak kiemelve.</span><span class="sxs-lookup"><span data-stu-id="2c24d-109">All dependencies and unique configurations are highlighted.</span></span> <span data-ttu-id="2c24d-110">Hello legjobb élmény érdekében előtelepítése hello megoldás tooyour Azure-előfizetés és a munka hello Azure Resource Manager-sablon mellett egy példánya.</span><span class="sxs-lookup"><span data-stu-id="2c24d-110">For hello best experience, pre-deploy an instance of hello solution tooyour Azure subscription and work along with hello Azure Resource Manager template.</span></span> <span data-ttu-id="2c24d-111">hello teljes sablon – itt található [zene tároló központi telepítését, az Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span><span class="sxs-lookup"><span data-stu-id="2c24d-111">hello complete template can be found here – [Music Store Deployment on Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span></span>

## <a name="configuration-script"></a><span data-ttu-id="2c24d-112">Konfigurációs parancsfájl</span><span class="sxs-lookup"><span data-stu-id="2c24d-112">Configuration script</span></span>
<span data-ttu-id="2c24d-113">Virtuálisgép-bővítmények olyan speciális programok, szemben a virtuális gépek tooprovide konfigurációs automation hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="2c24d-113">Virtual Machine extensions are specialized programs that execute against virtual machines tooprovide configuration automation.</span></span> <span data-ttu-id="2c24d-114">Számos adott célra, például víruskereső, a naplózási konfiguráció és a Docker konfigurációs bővítmények érhetők el.</span><span class="sxs-lookup"><span data-stu-id="2c24d-114">Extensions are available for many specific purposes such as anti-virus, logging configuration, and Docker configuration.</span></span> <span data-ttu-id="2c24d-115">Egy egyéni parancsprogramok futtatására szolgáló bővítmény használt toorun létrehozott parancsfájlok a virtuális gépek ellen lehet.</span><span class="sxs-lookup"><span data-stu-id="2c24d-115">A custom script extension can be used toorun any script against a virtual machine.</span></span> <span data-ttu-id="2c24d-116">Hello zeneáruház példával toohello egyéni parancsfájl kiterjesztése tooconfigure hello Ubuntu virtuális gépek és hello zene áruház-alkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="2c24d-116">With hello Music Store sample, it is up toohello custom script extension tooconfigure hello Ubuntu virtual machines and install hello Music Store application.</span></span> 

<span data-ttu-id="2c24d-117">Előtt, és részletesen leírja, hogyan virtuálisgép-bővítmények bejelentették az Azure Resource Manager-sablonok, ellenőrizze a futtatott parancsfájl hello.</span><span class="sxs-lookup"><span data-stu-id="2c24d-117">Before detailing how virtual machine extensions are declared in an Azure Resource Manager template, examine hello script that is run.</span></span> <span data-ttu-id="2c24d-118">Ez a parancsfájl hello Ubuntu virtuális gép toohost hello zene áruházból származó alkalmazás konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="2c24d-118">This script configures hello Ubuntu virtual machine toohost hello Music Store application.</span></span> <span data-ttu-id="2c24d-119">Futtatásakor hello parancsfájl telepíti-e minden szükséges szoftvert, a verziókövetésből hello zene áruház-alkalmazás telepítése és hello adatbázis előkészítése.</span><span class="sxs-lookup"><span data-stu-id="2c24d-119">When run, hello script installs all needed software, install hello Music store application from source control, and prepare hello database.</span></span> 

<span data-ttu-id="2c24d-120">További információ a .net üzemeltető toolearn alapvető alkalmazás Linux, lásd: [közzététel tooa Linux éles környezetben](https://docs.asp.net/en/latest/publishing/linuxproduction.html).</span><span class="sxs-lookup"><span data-stu-id="2c24d-120">toolearn more about hosting a .Net Core application on Linux, see [Publish tooa Linux production environment](https://docs.asp.net/en/latest/publishing/linuxproduction.html).</span></span>

> <span data-ttu-id="2c24d-121">Ez a minta bemutatási célokra van.</span><span class="sxs-lookup"><span data-stu-id="2c24d-121">This sample is for demonstration purposes.</span></span>
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

## <a name="vm-script-extension"></a><span data-ttu-id="2c24d-122">Virtuális gép parancsprogramok futtatására szolgáló bővítmény</span><span class="sxs-lookup"><span data-stu-id="2c24d-122">VM Script Extension</span></span>
<span data-ttu-id="2c24d-123">Virtuálisgép-bővítmények futtathatja a virtuális gépek elleni build időpontban hello Azure Resource Manager sablon hello bővítmény erőforrás-ot.</span><span class="sxs-lookup"><span data-stu-id="2c24d-123">VM Extensions can be run against a virtual machine at build time by including hello extension resource in hello Azure Resource Manager template.</span></span> <span data-ttu-id="2c24d-124">hello bővítmény hello Visual Studio erőforrás hozzáadása varázsló segítségével, vagy érvényes JSON beszúrása hello sablon adhatók hozzá.</span><span class="sxs-lookup"><span data-stu-id="2c24d-124">hello extension can be added with hello Visual Studio Add Resource wizard, or by inserting valid JSON into hello template.</span></span> <span data-ttu-id="2c24d-125">hello parancsprogramok futtatására szolgáló bővítmény erőforrás van beágyazva hello virtuálisgép-erőforrás; Ez a példa a következő hello látható.</span><span class="sxs-lookup"><span data-stu-id="2c24d-125">hello Script Extension resource is nested inside hello Virtual Machine resource; this can be seen in hello following example.</span></span>

<span data-ttu-id="2c24d-126">Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [VM parancsprogramok futtatására szolgáló bővítmény](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L359).</span><span class="sxs-lookup"><span data-stu-id="2c24d-126">Follow this link toosee hello JSON sample within hello Resource Manager template – [VM Script Extension](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L359).</span></span> 

<span data-ttu-id="2c24d-127">Az alábbi hello megfigyelheti hello parancsfájlok JSON-NÁ. a Githubon tárolja.</span><span class="sxs-lookup"><span data-stu-id="2c24d-127">Notice in hello below JSON that hello script is stored in GitHub.</span></span> <span data-ttu-id="2c24d-128">Ezt a parancsfájlt is tárolható az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="2c24d-128">This script could also be stored in Azure Blob storage.</span></span> <span data-ttu-id="2c24d-129">Azure Resource Manager-sablonok engedélyezése is hello parancsfájl végrehajtásának karakterlánc tooconstructed úgy, hogy a sablon paraméterek értékek parancsfájl végrehajtására használhatók paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="2c24d-129">Also, Azure Resource Manager templates allow hello script execution string tooconstructed such that template parameters values can be used as parameters for script execution.</span></span> <span data-ttu-id="2c24d-130">Ebben az esetben megadva adatai hello sablonok telepítésekor, és ezeket az értékeket felhasználható hello parancsfájl végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="2c24d-130">In this case data is provided when deploying hello templates, and these values can then be used when executing hello script.</span></span>

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

<span data-ttu-id="2c24d-131">További információ az egyéni parancsprogramok futtatására szolgáló bővítmény hello használatával, lásd: [egyéni parancsfájl-kiterjesztés Resource Manager-sablonok](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2c24d-131">For more information on using hello custom script extension, see [Custom script extensions with Resource Manager templates](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="next-step"></a><span data-ttu-id="2c24d-132">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="2c24d-132">Next Step</span></span>
<hr>

[<span data-ttu-id="2c24d-133">Megismerkedhet a több Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="2c24d-133">Explore More Azure Resource Manager Templates</span></span>](https://github.com/Azure/azure-quickstart-templates)

