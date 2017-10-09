---
title: "egyéni parancsfájlok aaaRun Linux virtuális gépek Azure-ban |} Microsoft Docs"
description: "Linux virtuális gép konfigurációs feladatok automatizálásához hello egyéni parancsprogramok futtatására szolgáló bővítmény használatával"
services: virtual-machines-linux
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: cf17ab2b-8d7e-4078-b6df-955c6d5071c2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: nepeters
ms.openlocfilehash: f2c273a5fbd4cd1695aea48fa4bd08e691511e5f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-custom-script-extension-with-linux-virtual-machines"></a><span data-ttu-id="f834e-103">Hello Azure egyéni parancsprogramok futtatására szolgáló bővítmény Linux virtuális gépek használata</span><span class="sxs-lookup"><span data-stu-id="f834e-103">Using hello Azure Custom Script Extension with Linux Virtual Machines</span></span>
<span data-ttu-id="f834e-104">Egyéni parancsprogramok futtatására szolgáló bővítmény hello és hajtanak végre a parancsfájlok az Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="f834e-104">hello Custom Script Extension downloads and executes scripts on Azure virtual machines.</span></span> <span data-ttu-id="f834e-105">A bővítmény akkor hasznos, ha a feladás egy vagy több központi telepítés konfigurálása, a szoftver telepítése vagy a más beállításokat / kezelési feladatot.</span><span class="sxs-lookup"><span data-stu-id="f834e-105">This extension is useful for post deployment configuration, software installation, or any other configuration / management task.</span></span> <span data-ttu-id="f834e-106">Parancsfájlok le: az Azure storage vagy más elérhető internet helyen, vagy megadott toohello bővítmény futási időt.</span><span class="sxs-lookup"><span data-stu-id="f834e-106">Scripts can be downloaded from Azure storage or other accessible internet location, or provided toohello extension run time.</span></span> <span data-ttu-id="f834e-107">Egyéni parancsprogramok futtatására szolgáló bővítmény hello integrálódik az Azure Resource Manager-sablonok, és is futtathat a hello Azure CLI, PowerShell, Azure-portálon vagy hello Azure virtuális gép REST API használatával.</span><span class="sxs-lookup"><span data-stu-id="f834e-107">hello Custom Script extension integrates with Azure Resource Manager templates, and can also be run using hello Azure CLI, PowerShell, Azure portal, or hello Azure Virtual Machine REST API.</span></span>

<span data-ttu-id="f834e-108">Ez a dokumentum részletek hogyan toouse hello az egyéni parancsprogramok futtatására szolgáló bővítmény hello Azure CLI és az Azure Resource Manager-sablon, valamint Linux rendszereken hibaelhárítási részleteket.</span><span class="sxs-lookup"><span data-stu-id="f834e-108">This document details how toouse hello Custom Script Extension from hello Azure CLI, and an Azure Resource Manager template, and also details troubleshooting steps on Linux systems.</span></span>

## <a name="extension-configuration"></a><span data-ttu-id="f834e-109">Bővítmény konfigurációja</span><span class="sxs-lookup"><span data-stu-id="f834e-109">Extension Configuration</span></span>
<span data-ttu-id="f834e-110">hello egyéni parancsprogramok futtatására szolgáló bővítmény konfigurációja határozza meg, többek között a parancsfájl helyét és hello parancs toobe futtatásához.</span><span class="sxs-lookup"><span data-stu-id="f834e-110">hello Custom Script Extension configuration specifies things like script location and hello command toobe run.</span></span> <span data-ttu-id="f834e-111">Ebben a konfigurációban tárolható konfigurációs fájlokban hello parancssorban vagy egy Azure Resource Manager-sablon a megadott.</span><span class="sxs-lookup"><span data-stu-id="f834e-111">This configuration can be stored in configuration files, specified on hello command line, or in an Azure Resource Manager template.</span></span> <span data-ttu-id="f834e-112">Bizalmas adatok tárolhatók védett konfigurációban, amely titkosított, és csak visszafejteni hello virtuális gépen belül.</span><span class="sxs-lookup"><span data-stu-id="f834e-112">Sensitive data can be stored in a protected configuration, which is encrypted and only decrypted inside hello virtual machine.</span></span> <span data-ttu-id="f834e-113">hello védett konfiguráció akkor hasznos, ha hello végrehajtási parancs magában foglalja a titkos kulcsokat például a jelszót.</span><span class="sxs-lookup"><span data-stu-id="f834e-113">hello protected configuration is useful when hello execution command includes secrets such as a password.</span></span>

### <a name="public-configuration"></a><span data-ttu-id="f834e-114">Nyilvános konfigurációja</span><span class="sxs-lookup"><span data-stu-id="f834e-114">Public Configuration</span></span>
<span data-ttu-id="f834e-115">Séma:</span><span class="sxs-lookup"><span data-stu-id="f834e-115">Schema:</span></span>

<span data-ttu-id="f834e-116">**Megjegyzés:** -e a tulajdonságnevek megkülönböztetik a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="f834e-116">**Note** - these property names are case sensitive.</span></span> <span data-ttu-id="f834e-117">Hello-neveket használja az tooavoid telepítésekkel kapcsolatos problémákhoz alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="f834e-117">Use hello names as seen below tooavoid deployment issues.</span></span>

* <span data-ttu-id="f834e-118">**commandToExecute**: (szükség esetén karakterlánc) belépési pont parancsfájl tooexecute hello</span><span class="sxs-lookup"><span data-stu-id="f834e-118">**commandToExecute**: (required, string) hello entry point script tooexecute</span></span>
* <span data-ttu-id="f834e-119">**fileUris**: (nem kötelező, karakterlánc-tömbben) hello URL-címéből fájlok toobe le.</span><span class="sxs-lookup"><span data-stu-id="f834e-119">**fileUris**: (optional, string array) hello URLs for files toobe downloaded.</span></span>
* <span data-ttu-id="f834e-120">**Timestamp típusú** (nem kötelező, egész szám) Ez a mező csak tootrigger egy újrafuttatása hello parancsfájl használatára Ez a mező értékének módosításával.</span><span class="sxs-lookup"><span data-stu-id="f834e-120">**timestamp** (optional, integer) use this field only tootrigger a rerun of hello script by changing value of this field.</span></span>

```json
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a><span data-ttu-id="f834e-121">Védett konfiguráció</span><span class="sxs-lookup"><span data-stu-id="f834e-121">Protected Configuration</span></span>
<span data-ttu-id="f834e-122">Séma:</span><span class="sxs-lookup"><span data-stu-id="f834e-122">Schema:</span></span>

<span data-ttu-id="f834e-123">**Megjegyzés:** -e a tulajdonságnevek megkülönböztetik a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="f834e-123">**Note** - these property names are case sensitive.</span></span> <span data-ttu-id="f834e-124">Hello-neveket használja az tooavoid telepítésekkel kapcsolatos problémákhoz alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="f834e-124">Use hello names as seen below tooavoid deployment issues.</span></span>

* <span data-ttu-id="f834e-125">**commandToExecute**: (nem kötelező, karakterlánc) belépési pont parancsfájl tooexecute hello.</span><span class="sxs-lookup"><span data-stu-id="f834e-125">**commandToExecute**: (optional, string) hello entry point script tooexecute.</span></span> <span data-ttu-id="f834e-126">Ehelyett használja ezt a mezőt, ha a parancs tartalmazza a titkos kulcsok, például jelszavakat.</span><span class="sxs-lookup"><span data-stu-id="f834e-126">Use this field instead if your command contains secrets such as passwords.</span></span>
* <span data-ttu-id="f834e-127">**storageAccountName**: (nem kötelező, karakterlánc) storage-fiók hello nevét.</span><span class="sxs-lookup"><span data-stu-id="f834e-127">**storageAccountName**: (optional, string) hello name of storage account.</span></span> <span data-ttu-id="f834e-128">Ha megadja a tároló hitelesítő adatait, minden fileUris URL-címek az Azure BLOB kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f834e-128">If you specify storage credentials, all fileUris must be URLs for Azure Blobs.</span></span>
* <span data-ttu-id="f834e-129">**storageAccountKey**: (nem kötelező, karakterlánc) hello hozzáférési kulcsot a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="f834e-129">**storageAccountKey**: (optional, string) hello access key of storage account.</span></span>

```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a><span data-ttu-id="f834e-130">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f834e-130">Azure CLI</span></span>
<span data-ttu-id="f834e-131">Hello Azure CLI toorun hello egyéni parancsprogramok futtatására szolgáló bővítmény használata esetén hozzon létre egy konfigurációs fájlban vagy a minimális hello fájl uri és hello végrehajtási parancsfájlt tartalmazó fájlokat.</span><span class="sxs-lookup"><span data-stu-id="f834e-131">When using hello Azure CLI toorun hello Custom Script Extension, create a configuration file or files containing at minimum hello file uri, and hello script execution command.</span></span>

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

<span data-ttu-id="f834e-132">Opcionálisan hello beállításokat adhat meg hello parancsban JSON formátumú karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="f834e-132">Optionally hello settings can be specified in hello command as a JSON formatted string.</span></span> <span data-ttu-id="f834e-133">Ez lehetővé teszi a hello konfigurációs toobe megadott végrehajtása közben, és egy külön konfigurációs fájl nélkül.</span><span class="sxs-lookup"><span data-stu-id="f834e-133">This allows hello configuration toobe specified during execution and without a separate configuration file.</span></span>

```azurecli
az vm extension set '
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://raw.githubusercontent.com/neilpeterson/test-extension/master/test.sh"],"commandToExecute": "./test.sh"}'
```

### <a name="azure-cli-examples"></a><span data-ttu-id="f834e-134">Az Azure CLI példák</span><span class="sxs-lookup"><span data-stu-id="f834e-134">Azure CLI Examples</span></span>

<span data-ttu-id="f834e-135">**1. példa** -parancsfájl nyilvános konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="f834e-135">**Example 1** - Public configuration with script file.</span></span>

```json
{
  "fileUris": ["https://raw.githubusercontent.com/neilpeterson/test-extension/master/test.sh"],
  "commandToExecute": "./test.sh"
}
```

<span data-ttu-id="f834e-136">Az Azure CLI-parancsot:</span><span class="sxs-lookup"><span data-stu-id="f834e-136">Azure CLI command:</span></span>

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

<span data-ttu-id="f834e-137">**2. példa** -nyilvános konfigurációja nem parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="f834e-137">**Example 2** - Public configuration with no script file.</span></span>

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

<span data-ttu-id="f834e-138">Az Azure CLI-parancsot:</span><span class="sxs-lookup"><span data-stu-id="f834e-138">Azure CLI command:</span></span>

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

<span data-ttu-id="f834e-139">**3. példa** – nyilvános konfigurációs fájlban használt toospecify hello parancsfájl URI, és egy védett konfigurációs fájl használt toospecify hello parancs toobe végre.</span><span class="sxs-lookup"><span data-stu-id="f834e-139">**Example 3** - A public configuration file is used toospecify hello script file URI, and a protected configuration file is used toospecify hello command toobe executed.</span></span>

<span data-ttu-id="f834e-140">Nyilvános konfigurációs fájlban:</span><span class="sxs-lookup"><span data-stu-id="f834e-140">Public configuration file:</span></span>

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"]
}
```

<span data-ttu-id="f834e-141">Védett konfigurációs fájlban:</span><span class="sxs-lookup"><span data-stu-id="f834e-141">Protected configuration file:</span></span>  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

<span data-ttu-id="f834e-142">Az Azure CLI-parancsot:</span><span class="sxs-lookup"><span data-stu-id="f834e-142">Azure CLI command:</span></span>

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json --protected-settings ./protected-config.json
```

## <a name="resource-manager-template"></a><span data-ttu-id="f834e-143">Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="f834e-143">Resource Manager Template</span></span>
<span data-ttu-id="f834e-144">hello Azure egyéni parancsprogramok futtatására szolgáló bővítmény futtatható a virtuális gép központi telepítéskor Resource Manager-sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="f834e-144">hello Azure Custom Script Extension can be run at Virtual Machine deployment time using a Resource Manager template.</span></span> <span data-ttu-id="f834e-145">toodo Igen, vegye fel a megfelelően formázott JSON toohello központi telepítési sablont.</span><span class="sxs-lookup"><span data-stu-id="f834e-145">toodo so, add properly formatted JSON toohello deployment template.</span></span>

### <a name="resource-manager-examples"></a><span data-ttu-id="f834e-146">Erőforrás-kezelő példák</span><span class="sxs-lookup"><span data-stu-id="f834e-146">Resource Manager Examples</span></span>
<span data-ttu-id="f834e-147">**1. példa** -nyilvános konfigurációjától.</span><span class="sxs-lookup"><span data-stu-id="f834e-147">**Example 1** - public configuration.</span></span>

```json
{
    "name": "scriptextensiondemo",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-15",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('scriptextensiondemoName'))]"
    ],
    "tags": {
        "displayName": "scriptextensiondemo"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
      "settings": {
        "fileUris": [
          "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
        ],
        "commandToExecute": "sh hello.sh"
      }
    }
}
```

<span data-ttu-id="f834e-148">**2. példa** -végrehajtási parancs védett konfigurációban.</span><span class="sxs-lookup"><span data-stu-id="f834e-148">**Example 2** - execution command in protected configuration.</span></span>

```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
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
        "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
      ]              
    },
    "protectedSettings": {
      "commandToExecute": "sh hello.sh <password>"
    }
  }
}
```

<span data-ttu-id="f834e-149">Lásd: a .net Core zeneáruház hello teljes például - bemutató [zene tároló bemutató](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).</span><span class="sxs-lookup"><span data-stu-id="f834e-149">See hello .Net Core Music Store Demo for a complete example - [Music Store Demo](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="f834e-150">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="f834e-150">Troubleshooting</span></span>
<span data-ttu-id="f834e-151">Egyéni parancsprogramok futtatására szolgáló bővítmény hello futtatásakor a hello parancsfájl jön létre, vagy egy hasonló toohello directory, a következő példa letöltött.</span><span class="sxs-lookup"><span data-stu-id="f834e-151">When hello Custom Script Extension runs, hello script is created or downloaded into a directory similar toohello following example.</span></span> <span data-ttu-id="f834e-152">hello parancs kimenetében is menti a könyvtárba, amely a `stdout` és `stderr` fájlt.</span><span class="sxs-lookup"><span data-stu-id="f834e-152">hello command output is also saved into this directory in `stdout` and `stderr` file.</span></span>

```bash
/var/lib/waagent/custom-script/download/0/
```

<span data-ttu-id="f834e-153">hello Azure parancsprogramok futtatására szolgáló bővítmény hoz létre a napló, amely itt található.</span><span class="sxs-lookup"><span data-stu-id="f834e-153">hello Azure Script Extension produces a log, which can be found here.</span></span>

```bash
/var/log/azure/custom-script/handler.log
```

<span data-ttu-id="f834e-154">Egyéni parancsprogramok futtatására szolgáló bővítmény hello hello végrehajtási állapotát is beolvashatók az Azure CLI hello.</span><span class="sxs-lookup"><span data-stu-id="f834e-154">hello execution state of hello Custom Script Extension can also be retrieved with hello Azure CLI.</span></span>

```azurecli
az vm extension list -g myResourceGroup --vm-name myVM
```

<span data-ttu-id="f834e-155">hello kimenete a következő szöveg hello néz ki:</span><span class="sxs-lookup"><span data-stu-id="f834e-155">hello output looks like hello following text:</span></span>

```azurecli
info:    Executing command vm extension get
+ Looking up hello VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a><span data-ttu-id="f834e-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f834e-156">Next Steps</span></span>
<span data-ttu-id="f834e-157">A többi virtuális gép parancsfájl-kiterjesztés információkért lásd: [Linux Azure parancsprogramok futtatására szolgáló bővítmény áttekintése](extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f834e-157">For information on other VM Script Extensions, see [Azure Script Extension overview for Linux](extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

