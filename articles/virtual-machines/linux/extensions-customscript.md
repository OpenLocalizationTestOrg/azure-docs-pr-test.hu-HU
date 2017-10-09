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
# <a name="using-hello-azure-custom-script-extension-with-linux-virtual-machines"></a>Hello Azure egyéni parancsprogramok futtatására szolgáló bővítmény Linux virtuális gépek használata
Egyéni parancsprogramok futtatására szolgáló bővítmény hello és hajtanak végre a parancsfájlok az Azure virtuális gépeken. A bővítmény akkor hasznos, ha a feladás egy vagy több központi telepítés konfigurálása, a szoftver telepítése vagy a más beállításokat / kezelési feladatot. Parancsfájlok le: az Azure storage vagy más elérhető internet helyen, vagy megadott toohello bővítmény futási időt. Egyéni parancsprogramok futtatására szolgáló bővítmény hello integrálódik az Azure Resource Manager-sablonok, és is futtathat a hello Azure CLI, PowerShell, Azure-portálon vagy hello Azure virtuális gép REST API használatával.

Ez a dokumentum részletek hogyan toouse hello az egyéni parancsprogramok futtatására szolgáló bővítmény hello Azure CLI és az Azure Resource Manager-sablon, valamint Linux rendszereken hibaelhárítási részleteket.

## <a name="extension-configuration"></a>Bővítmény konfigurációja
hello egyéni parancsprogramok futtatására szolgáló bővítmény konfigurációja határozza meg, többek között a parancsfájl helyét és hello parancs toobe futtatásához. Ebben a konfigurációban tárolható konfigurációs fájlokban hello parancssorban vagy egy Azure Resource Manager-sablon a megadott. Bizalmas adatok tárolhatók védett konfigurációban, amely titkosított, és csak visszafejteni hello virtuális gépen belül. hello védett konfiguráció akkor hasznos, ha hello végrehajtási parancs magában foglalja a titkos kulcsokat például a jelszót.

### <a name="public-configuration"></a>Nyilvános konfigurációja
Séma:

**Megjegyzés:** -e a tulajdonságnevek megkülönböztetik a kis-és nagybetűket. Hello-neveket használja az tooavoid telepítésekkel kapcsolatos problémákhoz alább látható módon.

* **commandToExecute**: (szükség esetén karakterlánc) belépési pont parancsfájl tooexecute hello
* **fileUris**: (nem kötelező, karakterlánc-tömbben) hello URL-címéből fájlok toobe le.
* **Timestamp típusú** (nem kötelező, egész szám) Ez a mező csak tootrigger egy újrafuttatása hello parancsfájl használatára Ez a mező értékének módosításával.

```json
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a>Védett konfiguráció
Séma:

**Megjegyzés:** -e a tulajdonságnevek megkülönböztetik a kis-és nagybetűket. Hello-neveket használja az tooavoid telepítésekkel kapcsolatos problémákhoz alább látható módon.

* **commandToExecute**: (nem kötelező, karakterlánc) belépési pont parancsfájl tooexecute hello. Ehelyett használja ezt a mezőt, ha a parancs tartalmazza a titkos kulcsok, például jelszavakat.
* **storageAccountName**: (nem kötelező, karakterlánc) storage-fiók hello nevét. Ha megadja a tároló hitelesítő adatait, minden fileUris URL-címek az Azure BLOB kell lennie.
* **storageAccountKey**: (nem kötelező, karakterlánc) hello hozzáférési kulcsot a tárfiók.

```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a>Azure CLI
Hello Azure CLI toorun hello egyéni parancsprogramok futtatására szolgáló bővítmény használata esetén hozzon létre egy konfigurációs fájlban vagy a minimális hello fájl uri és hello végrehajtási parancsfájlt tartalmazó fájlokat.

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

Opcionálisan hello beállításokat adhat meg hello parancsban JSON formátumú karakterlánc. Ez lehetővé teszi a hello konfigurációs toobe megadott végrehajtása közben, és egy külön konfigurációs fájl nélkül.

```azurecli
az vm extension set '
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://raw.githubusercontent.com/neilpeterson/test-extension/master/test.sh"],"commandToExecute": "./test.sh"}'
```

### <a name="azure-cli-examples"></a>Az Azure CLI példák

**1. példa** -parancsfájl nyilvános konfigurációja.

```json
{
  "fileUris": ["https://raw.githubusercontent.com/neilpeterson/test-extension/master/test.sh"],
  "commandToExecute": "./test.sh"
}
```

Az Azure CLI-parancsot:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

**2. példa** -nyilvános konfigurációja nem parancsfájlt.

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Az Azure CLI-parancsot:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

**3. példa** – nyilvános konfigurációs fájlban használt toospecify hello parancsfájl URI, és egy védett konfigurációs fájl használt toospecify hello parancs toobe végre.

Nyilvános konfigurációs fájlban:

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"]
}
```

Védett konfigurációs fájlban:  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

Az Azure CLI-parancsot:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json --protected-settings ./protected-config.json
```

## <a name="resource-manager-template"></a>Resource Manager-sablon
hello Azure egyéni parancsprogramok futtatására szolgáló bővítmény futtatható a virtuális gép központi telepítéskor Resource Manager-sablon használatával. toodo Igen, vegye fel a megfelelően formázott JSON toohello központi telepítési sablont.

### <a name="resource-manager-examples"></a>Erőforrás-kezelő példák
**1. példa** -nyilvános konfigurációjától.

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

**2. példa** -végrehajtási parancs védett konfigurációban.

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

Lásd: a .net Core zeneáruház hello teljes például - bemutató [zene tároló bemutató](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).

## <a name="troubleshooting"></a>Hibaelhárítás
Egyéni parancsprogramok futtatására szolgáló bővítmény hello futtatásakor a hello parancsfájl jön létre, vagy egy hasonló toohello directory, a következő példa letöltött. hello parancs kimenetében is menti a könyvtárba, amely a `stdout` és `stderr` fájlt.

```bash
/var/lib/waagent/custom-script/download/0/
```

hello Azure parancsprogramok futtatására szolgáló bővítmény hoz létre a napló, amely itt található.

```bash
/var/log/azure/custom-script/handler.log
```

Egyéni parancsprogramok futtatására szolgáló bővítmény hello hello végrehajtási állapotát is beolvashatók az Azure CLI hello.

```azurecli
az vm extension list -g myResourceGroup --vm-name myVM
```

hello kimenete a következő szöveg hello néz ki:

```azurecli
info:    Executing command vm extension get
+ Looking up hello VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Következő lépések
A többi virtuális gép parancsfájl-kiterjesztés információkért lásd: [Linux Azure parancsprogramok futtatására szolgáló bővítmény áttekintése](extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

