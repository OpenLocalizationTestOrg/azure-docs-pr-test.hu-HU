---
title: "Egyéni parancsfájl kiterjesztése a Windows aaaAzure |} Microsoft Docs"
description: "Windows virtuális gép konfigurációs feladatok automatizálásához hello egyéni parancsprogramok futtatására szolgáló bővítmény használatával"
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f4181fee-7a9d-4a1c-b517-52956f5b7fa1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/16/2017
ms.author: nepeters
ms.openlocfilehash: 97e065242e9fed116ee20b074f4e302a0cd10585
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="custom-script-extension-for-windows"></a>A Windows egyéni parancsprogramok futtatására szolgáló bővítmény

Egyéni parancsprogramok futtatására szolgáló bővítmény hello és hajtanak végre a parancsfájlok az Azure virtuális gépeken. A bővítmény akkor hasznos, ha a feladás egy vagy több központi telepítés konfigurálása, a szoftver telepítése vagy a más beállításokat / kezelési feladatot. Parancsfájlok le: az Azure storage vagy a Githubon, vagy megadott toohello az Azure portálon, a bővítmény futási időt. Egyéni parancsprogramok futtatására szolgáló bővítmény hello integrálódik az Azure Resource Manager-sablonok, és is futtathat a hello Azure CLI, PowerShell, Azure-portálon vagy hello Azure virtuális gép REST API használatával.

Ez a dokumentum részletesen, hogyan toouse hello egyéni parancsprogramok futtatására szolgáló bővítmény használatával hello a Azure PowerShell-modult, Azure Resource Manager-sablonok és a Windows rendszer hibaelhárítási részleteket.

## <a name="prerequisites"></a>Előfeltételek

### <a name="operating-system"></a>Operációs rendszer

Egyéni parancsprogramok futtatására szolgáló bővítmény hello a Windows is futtathatók a Windows Server 2008 R2, 2012, 2012 R2 és 2016 kiadását.

### <a name="script-location"></a>Parancsfájl helyét

hello parancsfájl kell toobe Azure Blob Storage tárolóban, vagy egy érvényes URL-cím elérhető bármely más helyen tárolja.

### <a name="internet-connectivity"></a>Internetkapcsolat

hello egyéni parancsfájl kiterjesztése Windows rendszerhez szükséges, hogy hello a cél virtuális gép csatlakoztatott toohello internet. 

## <a name="extension-schema"></a>A séma kiterjesztése

hello következő JSON jeleníti meg az egyéni parancsprogramok futtatására szolgáló bővítmény hello hello séma. egy parancsfájl helyét (Azure Storage vagy más helyre érvényes URL-CÍMMEL rendelkező), valamint egy parancs tooexecute hello bővítmény kell rendelkeznie. Ha használja az Azure Storage hello parancsfájl forrásaként, az Azure storage fiók név és fiókkulcs kulcsot meg kell adni. Legyen bizalmas adatokat a rendszer ezeket az elemeket, és hello bővítmények védett beállítás konfigurációjában megadott. Azure virtuális gépekre vonatkozó beállításával bővítmény védett adatok titkosítva, és csak visszafejteni hello cél virtuális gépen.

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
        "typeHandlerVersion": "1.9",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "fileUris": [
                "script location"
            ]
        },
        "protectedSettings": {
            "commandToExecute": "myExecutionCommand",
            "storageAccountName": "myStorageAccountName",
            "storageAccountKey": "myStorageAccountKey"
        }
    }
}
```

### <a name="property-values"></a>A tulajdonság értékek

| Név | Érték / – példa |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Közzétevő | Microsoft.Compute |
| type | Bővítmények |
| typeHandlerVersion | 1.9 |
| fileUris (például) | https://RAW.githubusercontent.com/Microsoft/DotNet-Core-sample-Templates/Master/DotNet-Core-Music-Windows/scripts/Configure-Music-App.ps1 |
| commandToExecute (például) | PowerShell - ExecutionPolicy Unrestricted - fájl konfigurálása zene-app.ps1 |
| storageAccountName (például) | examplestorageacct |
| storageAccountKey (például) | TmJK/1N3AbAZ3q / + hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg == |

**Megjegyzés:** -e a tulajdonságnevek megkülönböztetik a kis-és nagybetűket. Hello-neveket használja, mint fent tooavoid telepítési problémák esetére.

## <a name="template-deployment"></a>Sablonalapú telepítés

Az Azure Virtuálisgép-bővítmények az Azure Resource Manager-sablonok is telepíthető. hello JSON-séma hello előző szakaszban ismertetett az Azure Resource Manager sablon toorun hello egyéni parancsprogramok futtatására szolgáló bővítmény egy Azure Resource Manager sablon telepítése során használható. Amely tartalmazza az egyéni parancsprogramok futtatására szolgáló bővítmény itt található hello mintasablon [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="powershell-deployment"></a>PowerShell telepítése

Hello `Set-AzureRmVMCustomScriptExtension` parancs lehet használt tooadd hello egyéni parancsfájl kiterjesztése tooan meglévő virtuális gépet. További információkért lásd: [Set-AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).
```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName myResourceGroup `
    -VMName myVM `
    -Location myLocation `
    -FileUri myURL `
    -Run 'myScript.ps1' `
    -Name DemoScriptExtension
```

## <a name="troubleshoot-and-support"></a>Hibaelhárítás és támogatás

### <a name="troubleshoot"></a>Hibaelhárítás

A bővítmény központi telepítések hello állapotával kapcsolatos információkat lehet adatokat beolvasni az Azure-portálon hello és hello Azure PowerShell modul használatával. toosee hello telepítési állapota egy adott virtuális Gépet, futtassa a következő parancs hello-bővítmények.

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

A kimeneti bővítmény végrehajtási van naplózott toofiles hello alatt található a következő könyvtár hello cél virtuális gépen.
```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

a megadott hello fájlokat tölti be hello directory következő hello cél virtuális gépen.
```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads\<n>
```
Ha `<n>` decimális egész szám, amely hello bővítmény végrehajtások közötti módosíthatja.  Hello `1.*` értéke megegyezik a tényleges, aktuális hello `typeHandlerVersion` hello bővítmény értékét.  Hello tényleges directory lehet például `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.  

Hello végrehajtásakor `commandToExecute` parancs hello bővítmény fogja beállítani a könyvtár (például `...\Downloads\2`), az aktuális munkakönyvtárban hello. Ez lehetővé teszi, hogy relatív elérési utak toolocate hello fájlok hello használata letöltött hello `fileURIs` tulajdonság. Lásd az alábbi táblázatban hello példák.

Hello abszolút letöltési mappa elérési útját idővel változhatnak, mert-e a fájl relatív parancsfájl elérési utak, a hello jobb tooopt `commandToExecute` karakterlánc, amikor csak lehetséges. Példa:
```json
    "commandToExecute": "powershell.exe . . . -File './scripts/myscript.ps1'"
```

Útvonal-információinak után hello első URI-szegmens őrzi meg a fájlok letöltött hello `fileUris` tulajdonságlistát.  Ahogy az az alábbi táblázat hello, letöltött fájlok letöltési alkönyvtárak tooreflect hello szerkezete hello rendszer társítsa `fileUris` értékeket.  

#### <a name="examples-of-downloaded-files"></a>A letöltött fájlok példák

| URI-azonosító található fileUris | A letöltött helyzetéhez viszonyítva | Abszolút helye letöltött * |
| ---- | ------- |:--- |
| `https://someAcct.blob.core.windows.net/aContainer/scripts/myscript.ps1` | `./scripts/myscript.ps1` |`C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\scripts\myscript.ps1`  |
| `https://someAcct.blob.core.windows.net/aContainer/topLevel.ps1` | `./topLevel.ps1` | `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\topLevel.ps1` |

\*Mint a fenti hello könyvtár abszolút elérési utak változik hello VM, de nem belül egyetlen végrehajtásának hello CustomScript bővítménnyel hello élettartamuk során.

### <a name="support"></a>Támogatás

Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure szakértői hello [MSDN Azure és a Stack Overflow fórumok] hello (https://azure.microsoft.com/en-us/support/forums/). Másik lehetőségként is fájl az Azure támogatási incidens. Nyissa meg toohello [az Azure támogatási webhelyén](https://azure.microsoft.com/en-us/support/options/) válassza ki a Get-támogatást. Támogatja az Azure használatával kapcsolatos információkért olvassa el a hello [Microsoft Azure-támogatás – gyakori kérdések](https://azure.microsoft.com/en-us/support/faq/).
