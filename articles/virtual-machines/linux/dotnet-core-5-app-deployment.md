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
# <a name="application-deployment-with-azure-resource-manager-templates-for-linux-vms"></a><span data-ttu-id="fbe7b-103">Alkalmazás központi telepítése Azure Resource Manager-sablonok Linux virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="fbe7b-103">Application deployment with Azure Resource Manager templates for Linux VMs</span></span>

<span data-ttu-id="fbe7b-104">Miután az összes Azure infrastrukturális követelmények azonosította, és a központi telepítési sablont lefordítva, a tényleges alkalmazástelepítés kell figyelembe venni.</span><span class="sxs-lookup"><span data-stu-id="fbe7b-104">Once all Azure infrastructural requirements have been identified and translated into a deployment template, the actual application deployment needs to be addressed.</span></span> <span data-ttu-id="fbe7b-105">Itt az alkalmazás telepítése hivatkozik a tényleges bináris alkalmazásfájlokat alakzatot Azure-erőforrások telepítése.</span><span class="sxs-lookup"><span data-stu-id="fbe7b-105">Application deployment here is referring to installing the actual application binaries onto Azure resources.</span></span> <span data-ttu-id="fbe7b-106">A zeneáruház minta, a .net Core NGINX és felügyelő kell telepíthetők és konfigurálhatók a virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="fbe7b-106">For the Music Store sample, .Net Core, NGINX, and Supervisor need to be installed and configured on each virtual machine.</span></span> <span data-ttu-id="fbe7b-107">Bináris a zene telepítve kell lennie települ a virtuális gépre, és a zeneáruház adatbázist előre létre.</span><span class="sxs-lookup"><span data-stu-id="fbe7b-107">The Music Store binaries need to be installed onto the virtual machine, and the Music Store database pre-created.</span></span>

<span data-ttu-id="fbe7b-108">Ez a dokumentum részletesen, hogyan automatizálható virtuálisgép-bővítmények alkalmazás központi telepítése és konfigurálása az Azure virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="fbe7b-108">This document details how Virtual Machine extensions can automate application deployment and configuration to Azure virtual machines.</span></span> <span data-ttu-id="fbe7b-109">Minden függőségeket és külön konfigurációt vannak kiemelve.</span><span class="sxs-lookup"><span data-stu-id="fbe7b-109">All dependencies and unique configurations are highlighted.</span></span> <span data-ttu-id="fbe7b-110">A legjobb használhatóság érdekében előtelepítése a megoldást az Azure-előfizetés és a munkahelyi együtt az Azure Resource Manager-sablon egy példányát.</span><span class="sxs-lookup"><span data-stu-id="fbe7b-110">For the best experience, pre-deploy an instance of the solution to your Azure subscription and work along with the Azure Resource Manager template.</span></span> <span data-ttu-id="fbe7b-111">A teljes sablon – itt található [zene tároló központi telepítését, az Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span><span class="sxs-lookup"><span data-stu-id="fbe7b-111">The complete template can be found here – [Music Store Deployment on Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span></span>

## <a name="configuration-script"></a><span data-ttu-id="fbe7b-112">Konfigurációs parancsfájl</span><span class="sxs-lookup"><span data-stu-id="fbe7b-112">Configuration script</span></span>
<span data-ttu-id="fbe7b-113">Virtuálisgép-bővítmények olyan speciális programok, konfigurációs automation biztosításához a virtuális gépek elleni hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="fbe7b-113">Virtual Machine extensions are specialized programs that execute against virtual machines to provide configuration automation.</span></span> <span data-ttu-id="fbe7b-114">Számos adott célra, például víruskereső, a naplózási konfiguráció és a Docker konfigurációs bővítmények érhetők el.</span><span class="sxs-lookup"><span data-stu-id="fbe7b-114">Extensions are available for many specific purposes such as anti-virus, logging configuration, and Docker configuration.</span></span> <span data-ttu-id="fbe7b-115">Egy egyéni parancsprogramok futtatására szolgáló bővítmény segítségével futtassa a parancsfájlt egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="fbe7b-115">A custom script extension can be used to run any script against a virtual machine.</span></span> <span data-ttu-id="fbe7b-116">A zeneáruház minta esetén az egyéni parancsprogramok futtatására szolgáló bővítmény a Ubuntu virtuális gépek konfigurálásáról és telepítéséről zene áruházból származó alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fbe7b-116">With the Music Store sample, it is up to the custom script extension to configure the Ubuntu virtual machines and install the Music Store application.</span></span> 

<span data-ttu-id="fbe7b-117">Előtt, és részletesen leírja, hogyan vannak deklarálva virtuálisgép-bővítmények az Azure Resource Manager-sablon, vizsgálja meg a futtatni kívánt parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="fbe7b-117">Before detailing how virtual machine extensions are declared in an Azure Resource Manager template, examine the script that is run.</span></span> <span data-ttu-id="fbe7b-118">Ezt a parancsfájlt a Ubuntu virtuális gép zene áruházból származó alkalmazás futtatására konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="fbe7b-118">This script configures the Ubuntu virtual machine to host the Music Store application.</span></span> <span data-ttu-id="fbe7b-119">Futtatásakor, a parancsfájl az összes telepíti a szükséges szoftver, a verziókövetésből zene áruházból származó alkalmazás telepítése, és az adatbázis előkészítése.</span><span class="sxs-lookup"><span data-stu-id="fbe7b-119">When run, the script installs all needed software, install the Music store application from source control, and prepare the database.</span></span> 

<span data-ttu-id="fbe7b-120">További információt a .net üzemeltető Linux alkalmazás központi telepítéséről [Linux éles környezetben a közzététel](https://docs.asp.net/en/latest/publishing/linuxproduction.html).</span><span class="sxs-lookup"><span data-stu-id="fbe7b-120">To learn more about hosting a .Net Core application on Linux, see [Publish to a Linux production environment](https://docs.asp.net/en/latest/publishing/linuxproduction.html).</span></span>

> <span data-ttu-id="fbe7b-121">Ez a minta bemutatási célokra van.</span><span class="sxs-lookup"><span data-stu-id="fbe7b-121">This sample is for demonstration purposes.</span></span>
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

## <a name="vm-script-extension"></a><span data-ttu-id="fbe7b-122">Virtuális gép parancsprogramok futtatására szolgáló bővítmény</span><span class="sxs-lookup"><span data-stu-id="fbe7b-122">VM Script Extension</span></span>
<span data-ttu-id="fbe7b-123">Virtuálisgép-bővítmények futtathatja a virtuális gépek elleni build időpontban a bővítmény erőforrás belefoglalja az Azure Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="fbe7b-123">VM Extensions can be run against a virtual machine at build time by including the extension resource in the Azure Resource Manager template.</span></span> <span data-ttu-id="fbe7b-124">A bővítmény a Visual Studio erőforrás hozzáadása varázsló segítségével, vagy érvényes JSON beszúrása a sablon lehet hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="fbe7b-124">The extension can be added with the Visual Studio Add Resource wizard, or by inserting valid JSON into the template.</span></span> <span data-ttu-id="fbe7b-125">A parancsprogramok futtatására szolgáló bővítmény erőforrás van beágyazva a virtuálisgép-erőforrás; Ez az alábbi példában látható.</span><span class="sxs-lookup"><span data-stu-id="fbe7b-125">The Script Extension resource is nested inside the Virtual Machine resource; this can be seen in the following example.</span></span>

<span data-ttu-id="fbe7b-126">Hajtsa végre az erre a hivatkozásra kattintva tekintse meg a JSON-mintát belül a Resource Manager-sablon – [VM parancsprogramok futtatására szolgáló bővítmény](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L359).</span><span class="sxs-lookup"><span data-stu-id="fbe7b-126">Follow this link to see the JSON sample within the Resource Manager template – [VM Script Extension](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L359).</span></span> 

<span data-ttu-id="fbe7b-127">Láthatja az alábbi JSON, hogy a parancsfájl a Githubon található.</span><span class="sxs-lookup"><span data-stu-id="fbe7b-127">Notice in the below JSON that the script is stored in GitHub.</span></span> <span data-ttu-id="fbe7b-128">Ezt a parancsfájlt is tárolható az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="fbe7b-128">This script could also be stored in Azure Blob storage.</span></span> <span data-ttu-id="fbe7b-129">Azure Resource Manager-sablonok is, a parancsfájl végrehajtási karakterláncot megtervezni, hogy sablon paraméterek értékek parancsfájl végrehajtására használhatók paraméterekként engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="fbe7b-129">Also, Azure Resource Manager templates allow the script execution string to constructed such that template parameters values can be used as parameters for script execution.</span></span> <span data-ttu-id="fbe7b-130">Ebben az esetben a adat rendelkezésre áll-e a sablonok telepítésekor, és ezeket az értékeket ezután felhasználhatók, a parancsfájl végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="fbe7b-130">In this case data is provided when deploying the templates, and these values can then be used when executing the script.</span></span>

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

<span data-ttu-id="fbe7b-131">Az egyéni parancsprogramok futtatására szolgáló bővítmény használatával kapcsolatos további információkért lásd: [egyéni parancsfájl-kiterjesztés Resource Manager-sablonok](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="fbe7b-131">For more information on using the custom script extension, see [Custom script extensions with Resource Manager templates](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="next-step"></a><span data-ttu-id="fbe7b-132">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="fbe7b-132">Next Step</span></span>
<hr>

[<span data-ttu-id="fbe7b-133">Megismerkedhet a több Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="fbe7b-133">Explore More Azure Resource Manager Templates</span></span>](https://github.com/Azure/azure-quickstart-templates)

