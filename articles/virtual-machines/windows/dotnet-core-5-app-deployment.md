---
title: "Alkalmazás központi telepítése virtuálisgép-bővítmények aaaAutomating |} Microsoft Docs"
description: "Azure virtuális gép DotNet fő oktatóanyag"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 79c91304-6c1b-4354-a185-fecc129b139d
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6d52537fbd4e935f19d3864def11484f519f8598
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-deployment-with-azure-resource-manager-templates-for-windows-vms"></a><span data-ttu-id="c040f-103">Alkalmazás központi telepítése Azure Resource Manager-sablonok Windows virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="c040f-103">Application deployment with Azure Resource Manager templates for Windows VMs</span></span>

<span data-ttu-id="c040f-104">Miután az összes Azure infrastrukturális követelmények azonosította, és a központi telepítési sablont lefordítva, hello tényleges alkalmazástelepítés címzett toobe van szüksége.</span><span class="sxs-lookup"><span data-stu-id="c040f-104">Once all Azure infrastructural requirements have been identified and translated into a deployment template, hello actual application deployment needs toobe addressed.</span></span> <span data-ttu-id="c040f-105">Itt az alkalmazás telepítése tooinstalling hello tényleges bináris alkalmazásfájlokat alakzatot Azure-erőforrások hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="c040f-105">Application deployment here is referring tooinstalling hello actual application binaries onto Azure resources.</span></span> <span data-ttu-id="c040f-106">Hello zeneáruház minta .net Core és az IIS kell toobe telepítette és konfigurálta a virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="c040f-106">For hello Music Store sample, .Net Core and IIS need toobe installed and configured on each virtual machine.</span></span> <span data-ttu-id="c040f-107">hello zene tároló bináris toobe hello virtuális gép telepítve kell, és hello zene tároló adatbázis előre létrehozott.</span><span class="sxs-lookup"><span data-stu-id="c040f-107">hello Music Store binaries need toobe installed onto hello virtual machine, and hello Music Store database pre-created.</span></span>

<span data-ttu-id="c040f-108">Ez a dokumentum részletesen, hogyan virtuálisgép-bővítmények az alkalmazás központi telepítése és konfigurálása tooAzure virtuális gépek automatizálható.</span><span class="sxs-lookup"><span data-stu-id="c040f-108">This document details how Virtual Machine extensions can automate application deployment and configuration tooAzure virtual machines.</span></span> <span data-ttu-id="c040f-109">Minden függőségeket és külön konfigurációt vannak kiemelve.</span><span class="sxs-lookup"><span data-stu-id="c040f-109">All dependencies and unique configurations are highlighted.</span></span> <span data-ttu-id="c040f-110">Hello legjobb élmény érdekében előtelepítése hello megoldás tooyour Azure-előfizetés és a munka hello Azure Resource Manager-sablon mellett egy példánya.</span><span class="sxs-lookup"><span data-stu-id="c040f-110">For hello best experience, pre-deploy an instance of hello solution tooyour Azure subscription and work along with hello Azure Resource Manager template.</span></span> <span data-ttu-id="c040f-111">hello teljes sablon – itt található [zene tároló központi telepítést a Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="c040f-111">hello complete template can be found here – [Music Store Deployment on Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

## <a name="configuration-script"></a><span data-ttu-id="c040f-112">Konfigurációs parancsfájl</span><span class="sxs-lookup"><span data-stu-id="c040f-112">Configuration script</span></span>
<span data-ttu-id="c040f-113">Virtuálisgép-bővítmények olyan speciális programok, szemben a virtuális gépek tooprovide konfigurációs automation hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="c040f-113">Virtual Machine extensions are specialized programs that execute against virtual machines tooprovide configuration automation.</span></span> <span data-ttu-id="c040f-114">Számos adott célra, például víruskereső, a naplózási konfiguráció és a Docker konfigurációs bővítmények érhetők el.</span><span class="sxs-lookup"><span data-stu-id="c040f-114">Extensions are available for many specific purposes such as anti-virus, logging configuration, and Docker configuration.</span></span> <span data-ttu-id="c040f-115">Egyéni parancsprogramok futtatására szolgáló bővítmény hello használt toorun létrehozott parancsfájlok a virtuális gépek ellen lehet.</span><span class="sxs-lookup"><span data-stu-id="c040f-115">hello Custom Script extension can be used toorun any script against a virtual machine.</span></span> <span data-ttu-id="c040f-116">Hello zeneáruház példával toohello egyéni parancsfájl kiterjesztése tooconfigure hello Windows virtuális gépek és hello zene áruház-alkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="c040f-116">With hello Music Store sample, it is up toohello custom script extension tooconfigure hello Windows virtual machines and install hello Music Store application.</span></span>

<span data-ttu-id="c040f-117">Előtt, és részletesen leírja, hogyan virtuálisgép-bővítmények bejelentették az Azure Resource Manager-sablonok, ellenőrizze a futtatott parancsfájl hello.</span><span class="sxs-lookup"><span data-stu-id="c040f-117">Before detailing how virtual machine extensions are declared in an Azure Resource Manager template, examine hello script that is run.</span></span> <span data-ttu-id="c040f-118">Ez a parancsfájl hello Windows virtuális gép toohost hello zene áruházból származó alkalmazás konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="c040f-118">This script configures hello Windows virtual machine toohost hello Music Store application.</span></span> <span data-ttu-id="c040f-119">Futtatásakor hello parancsfájl telepíti-e minden szükséges szoftvert, a verziókövetésből hello zene áruház-alkalmazás telepítése és hello adatbázis előkészítése.</span><span class="sxs-lookup"><span data-stu-id="c040f-119">When run, hello script installs all needed software, install hello Music store application from source control, and prepare hello database.</span></span> 

> <span data-ttu-id="c040f-120">Ez a minta bemutatási célokra van.</span><span class="sxs-lookup"><span data-stu-id="c040f-120">This sample is for demonstration purposes.</span></span>

```PowerShell
<#
    .SYNOPSIS
        Downloads and configures .Net Core Music Store application sample across IIS and Azure SQL DB.
#>

Param (
    [string]$user,
    [string]$password,
    [string]$sqlserver
)

# Firewall
netsh advfirewall firewall add rule name="http" dir=in action=allow protocol=TCP localport=80

# Folders
New-Item -ItemType Directory c:\temp
New-Item -ItemType Directory c:\music

# Install iis
Install-WindowsFeature web-server -IncludeManagementTools

# Install dot.net core sdk
Invoke-WebRequest http://go.microsoft.com/fwlink/?LinkID=615460 -outfile c:\temp\vc_redistx64.exe
Start-Process c:\temp\vc_redistx64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkID=809122 -outfile c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe
Start-Process c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkId=817246 -outfile c:\temp\DotNetCore.WindowsHosting.exe
Start-Process c:\temp\DotNetCore.WindowsHosting.exe -ArgumentList '/quiet' -Wait

# Download music app
Invoke-WebRequest  https://github.com/Microsoft/dotnet-core-sample-templates/raw/master/dotnet-core-music-windows/music-app/music-store-azure-demo-pub.zip -OutFile c:\temp\musicstore.zip
Expand-Archive C:\temp\musicstore.zip c:\music

# Azure SQL connection sting in environment variable
[Environment]::SetEnvironmentVariable("Data:DefaultConnection:ConnectionString", "Server=$sqlserver;Database=MusicStore;Integrated Security=False;User Id=$user;Password=$password;MultipleActiveResultSets=True;Connect Timeout=30",[EnvironmentVariableTarget]::Machine)

# Pre-create database
$env:Data:DefaultConnection:ConnectionString = "Server=$sqlserver;Database=MusicStore;Integrated Security=False;User Id=$user;Password=$password;MultipleActiveResultSets=True;Connect Timeout=30"
Start-Process 'C:\Program Files\dotnet\dotnet.exe' -ArgumentList 'c:\music\MusicStore.dll'

# Configure iis
Remove-WebSite -Name "Default Web Site"
Set-ItemProperty IIS:\AppPools\DefaultAppPool\ managedRuntimeVersion ""
New-Website -Name "MusicStore" -Port 80 -PhysicalPath C:\music\ -ApplicationPool DefaultAppPool
& iisreset
```

## <a name="vm-script-extension"></a><span data-ttu-id="c040f-121">Virtuális gép parancsprogramok futtatására szolgáló bővítmény</span><span class="sxs-lookup"><span data-stu-id="c040f-121">VM Script Extension</span></span>
<span data-ttu-id="c040f-122">Virtuálisgép-bővítmények futtathatja a virtuális gépek elleni build időpontban hello Azure Resource Manager sablon hello bővítmény erőforrás-ot.</span><span class="sxs-lookup"><span data-stu-id="c040f-122">VM Extensions can be run against a virtual machine at build time by including hello extension resource in hello Azure Resource Manager template.</span></span> <span data-ttu-id="c040f-123">hello bővítmény hello Visual Studio erőforrás hozzáadása varázsló segítségével, vagy érvényes JSON beszúrása hello sablon adhatók hozzá.</span><span class="sxs-lookup"><span data-stu-id="c040f-123">hello extension can be added with hello Visual Studio Add Resource wizard, or by inserting valid JSON into hello template.</span></span> <span data-ttu-id="c040f-124">hello parancsprogramok futtatására szolgáló bővítmény erőforrás van beágyazva hello virtuálisgép-erőforrás; Ez a példa a következő hello látható.</span><span class="sxs-lookup"><span data-stu-id="c040f-124">hello Script Extension resource is nested inside hello Virtual Machine resource; this can be seen in hello following example.</span></span>

<span data-ttu-id="c040f-125">Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [VM parancsprogramok futtatására szolgáló bővítmény](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L339).</span><span class="sxs-lookup"><span data-stu-id="c040f-125">Follow this link toosee hello JSON sample within hello Resource Manager template – [VM Script Extension](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L339).</span></span> 

<span data-ttu-id="c040f-126">Az alábbi hello megfigyelheti hello parancsfájlok JSON-NÁ. a Githubon tárolja.</span><span class="sxs-lookup"><span data-stu-id="c040f-126">Notice in hello below JSON that hello script is stored in GitHub.</span></span> <span data-ttu-id="c040f-127">Ezt a parancsfájlt is tárolható az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="c040f-127">This script could also be stored in Azure Blob storage.</span></span> <span data-ttu-id="c040f-128">Azure Resource Manager-sablonok engedélyezése is hello parancsfájl végrehajtásának karakterlánc toobe megtervezni, hogy sablon paraméterek értékek parancsfájl végrehajtására használhatók paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="c040f-128">Also, Azure Resource Manager templates allow hello script execution string toobe constructed such that template parameters values can be used as parameters for script execution.</span></span> <span data-ttu-id="c040f-129">Ebben az esetben megadva adatai hello sablonok telepítésekor, és ezeket az értékeket felhasználható hello parancsfájl végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="c040f-129">In this case data is provided when deploying hello templates, and these values can then be used when executing hello script.</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
  }
}
```

<span data-ttu-id="c040f-130">Fent említett, akkor is lehetséges toostore az egyéni parancsfájlok, az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="c040f-130">As mentioned above, it is also possible toostore your custom scripts in Azure Blob storage.</span></span> <span data-ttu-id="c040f-131">Két lehetőség hello parancsfájl erőforrások tárolásához a blob Storage tárolóban; hello tároló, a parancsfájl nyilvános, és hajtsa végre az azonos megközelíti a fenti hello vagy is kezelhetők a titkos blob Storage tárolóban, amelyhez tooprovide hello storageAccountName és storageAccountKey toohello CustomScriptExtension erőforrás-definícióban.</span><span class="sxs-lookup"><span data-stu-id="c040f-131">There are two options for storing hello script resources in blob storage; either make hello container/script public and follow hello same approach as above, or it can also be kept in private blob storage which requires you tooprovide hello storageAccountName and storageAccountKey toohello CustomScriptExtension resource definition.</span></span>

<span data-ttu-id="c040f-132">A hello az alábbi példa azt megtettünk mindent egy lépés további.</span><span class="sxs-lookup"><span data-stu-id="c040f-132">In hello example below we have gone a step further.</span></span> <span data-ttu-id="c040f-133">Is lehetséges tooprovide hello tárfiók neve vagy a kulcs paraméter vagy változó üzembe helyezése során, Resource Manager-sablonok nyújtja hello `listKeys` függvény, amely úgy szerezheti be a tárfiók hello kulcs programozott módon, és szúrja be a toohello sablon, a központi telepítéskor.</span><span class="sxs-lookup"><span data-stu-id="c040f-133">While it is possible tooprovide hello storage account name and key as a parameter or variable during deployment, Resource Manager templates provide hello `listKeys` function that can obtain hello storage account key programmatically and insert it in toohello template for you at deployment time.</span></span>

<span data-ttu-id="c040f-134">CustomScriptExtension erőforrás-definíciót az alábbi, az egyéni parancsfájl már feltöltött tooan hello például az Azure storage-fiók neve `mystorageaccount9999` amelyek létezik egy másik erőforráscsoportban nevű `mysa999rgname`.</span><span class="sxs-lookup"><span data-stu-id="c040f-134">In hello example CustomScriptExtension resource definition below, our custom script has already been uploaded tooan Azure storage account called `mystorageaccount9999` which exists in another Resource Group called `mysa999rgname`.</span></span> <span data-ttu-id="c040f-135">Ehhez az erőforráshoz tartalmazó sablon telepítésekor hello `listKeys` függvény programozott módon jut hozzá a hello tárfiók kulcsa hello tárfiók `mystorageaccount9999` a hello erőforráscsoport `mysa999rgname` , és beilleszti az USA toohello sablont.</span><span class="sxs-lookup"><span data-stu-id="c040f-135">When we deploy a template containing this resource, hello `listKeys` function programmatically obtains hello storage account key for hello storage account `mystorageaccount9999` in hello Resource Group `mysa999rgname` and inserts it in toohello template for us.</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.7",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://mystorageaccount9999.blob.core.windows.net/container/configure-music-app.ps1"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]",
      "storageAccountName": "mystorageaccount9999",
      "storageAccountKey": "[listKeys(resourceId('mysa999rgname','Microsoft.Storage/storageAccounts', mystorageaccount9999), '2015-06-15').key1]"
    }
  }
}
```

<span data-ttu-id="c040f-136">hello fő előnye, hogy ezt a módszert használja, hogy nem követeli meg toochange a sablont, vagy hello eseményben az üzembe helyezéshez megadott paraméterek hello tárolási fiók kulcs módosítása.</span><span class="sxs-lookup"><span data-stu-id="c040f-136">hello main benefit of this approach is that it does not require you toochange your template or deployment parameters in hello event of hello storage account key changing.</span></span>

<span data-ttu-id="c040f-137">További információ az egyéni parancsprogramok futtatására szolgáló bővítmény hello használatával, lásd: [egyéni parancsfájl-kiterjesztés Resource Manager-sablonok](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c040f-137">For more information on using hello custom script extension, see [Custom script extensions with Resource Manager templates](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="next-step"></a><span data-ttu-id="c040f-138">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="c040f-138">Next Step</span></span>
<hr>

[<span data-ttu-id="c040f-139">Megismerkedhet a több Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="c040f-139">Explore More Azure Resource Manager Templates</span></span>](https://github.com/Azure/azure-quickstart-templates)

