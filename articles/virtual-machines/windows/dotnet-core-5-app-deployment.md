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
# <a name="application-deployment-with-azure-resource-manager-templates-for-windows-vms"></a>Alkalmazás központi telepítése Azure Resource Manager-sablonok Windows virtuális gépek

Miután az összes Azure infrastrukturális követelmények azonosította, és a központi telepítési sablont lefordítva, hello tényleges alkalmazástelepítés címzett toobe van szüksége. Itt az alkalmazás telepítése tooinstalling hello tényleges bináris alkalmazásfájlokat alakzatot Azure-erőforrások hivatkozik. Hello zeneáruház minta .net Core és az IIS kell toobe telepítette és konfigurálta a virtuális gépeken. hello zene tároló bináris toobe hello virtuális gép telepítve kell, és hello zene tároló adatbázis előre létrehozott.

Ez a dokumentum részletesen, hogyan virtuálisgép-bővítmények az alkalmazás központi telepítése és konfigurálása tooAzure virtuális gépek automatizálható. Minden függőségeket és külön konfigurációt vannak kiemelve. Hello legjobb élmény érdekében előtelepítése hello megoldás tooyour Azure-előfizetés és a munka hello Azure Resource Manager-sablon mellett egy példánya. hello teljes sablon – itt található [zene tároló központi telepítést a Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="configuration-script"></a>Konfigurációs parancsfájl
Virtuálisgép-bővítmények olyan speciális programok, szemben a virtuális gépek tooprovide konfigurációs automation hajtható végre. Számos adott célra, például víruskereső, a naplózási konfiguráció és a Docker konfigurációs bővítmények érhetők el. Egyéni parancsprogramok futtatására szolgáló bővítmény hello használt toorun létrehozott parancsfájlok a virtuális gépek ellen lehet. Hello zeneáruház példával toohello egyéni parancsfájl kiterjesztése tooconfigure hello Windows virtuális gépek és hello zene áruház-alkalmazás telepítése.

Előtt, és részletesen leírja, hogyan virtuálisgép-bővítmények bejelentették az Azure Resource Manager-sablonok, ellenőrizze a futtatott parancsfájl hello. Ez a parancsfájl hello Windows virtuális gép toohost hello zene áruházból származó alkalmazás konfigurálja. Futtatásakor hello parancsfájl telepíti-e minden szükséges szoftvert, a verziókövetésből hello zene áruház-alkalmazás telepítése és hello adatbázis előkészítése. 

> Ez a minta bemutatási célokra van.

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

## <a name="vm-script-extension"></a>Virtuális gép parancsprogramok futtatására szolgáló bővítmény
Virtuálisgép-bővítmények futtathatja a virtuális gépek elleni build időpontban hello Azure Resource Manager sablon hello bővítmény erőforrás-ot. hello bővítmény hello Visual Studio erőforrás hozzáadása varázsló segítségével, vagy érvényes JSON beszúrása hello sablon adhatók hozzá. hello parancsprogramok futtatására szolgáló bővítmény erőforrás van beágyazva hello virtuálisgép-erőforrás; Ez a példa a következő hello látható.

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [VM parancsprogramok futtatására szolgáló bővítmény](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L339). 

Az alábbi hello megfigyelheti hello parancsfájlok JSON-NÁ. a Githubon tárolja. Ezt a parancsfájlt is tárolható az Azure Blob Storage tárolóban. Azure Resource Manager-sablonok engedélyezése is hello parancsfájl végrehajtásának karakterlánc toobe megtervezni, hogy sablon paraméterek értékek parancsfájl végrehajtására használhatók paraméterekként. Ebben az esetben megadva adatai hello sablonok telepítésekor, és ezeket az értékeket felhasználható hello parancsfájl végrehajtása közben.

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

Fent említett, akkor is lehetséges toostore az egyéni parancsfájlok, az Azure Blob Storage tárolóban. Két lehetőség hello parancsfájl erőforrások tárolásához a blob Storage tárolóban; hello tároló, a parancsfájl nyilvános, és hajtsa végre az azonos megközelíti a fenti hello vagy is kezelhetők a titkos blob Storage tárolóban, amelyhez tooprovide hello storageAccountName és storageAccountKey toohello CustomScriptExtension erőforrás-definícióban.

A hello az alábbi példa azt megtettünk mindent egy lépés további. Is lehetséges tooprovide hello tárfiók neve vagy a kulcs paraméter vagy változó üzembe helyezése során, Resource Manager-sablonok nyújtja hello `listKeys` függvény, amely úgy szerezheti be a tárfiók hello kulcs programozott módon, és szúrja be a toohello sablon, a központi telepítéskor.

CustomScriptExtension erőforrás-definíciót az alábbi, az egyéni parancsfájl már feltöltött tooan hello például az Azure storage-fiók neve `mystorageaccount9999` amelyek létezik egy másik erőforráscsoportban nevű `mysa999rgname`. Ehhez az erőforráshoz tartalmazó sablon telepítésekor hello `listKeys` függvény programozott módon jut hozzá a hello tárfiók kulcsa hello tárfiók `mystorageaccount9999` a hello erőforráscsoport `mysa999rgname` , és beilleszti az USA toohello sablont.

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

hello fő előnye, hogy ezt a módszert használja, hogy nem követeli meg toochange a sablont, vagy hello eseményben az üzembe helyezéshez megadott paraméterek hello tárolási fiók kulcs módosítása.

További információ az egyéni parancsprogramok futtatására szolgáló bővítmény hello használatával, lásd: [egyéni parancsfájl-kiterjesztés Resource Manager-sablonok](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="next-step"></a>Következő lépés
<hr>

[Megismerkedhet a több Azure Resource Manager-sablonok](https://github.com/Azure/azure-quickstart-templates)

