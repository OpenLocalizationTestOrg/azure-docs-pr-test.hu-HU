---
title: "a Windows virtuális gép parancsprogramok futtatására szolgáló bővítmény aaaCustom |} Microsoft Docs"
description: "Azure virtuális gép konfigurációs feladatok automatizálása a Windows virtuális gép távoli hello egyéni parancsfájl kiterjesztése toorun PowerShell-parancsfájlok használatával"
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: ebb7340a-8f61-4d3c-a290-d7bf8de2d0bd
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 01/17/2017
ms.author: nepeters
ms.openlocfilehash: cf7bb895dd211f07fd010dc03b68cd77df1127b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="custom-script-extension-for-windows-using-hello-classic-deployment-model"></a>Egyéni parancsfájl kiterjesztése a Windows hello klasszikus telepítési modell segítségével

> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Ismerje meg, hogyan túl[hello Resource Manager modellt használja a következő lépésekkel](../extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Egyéni parancsprogramok futtatására szolgáló bővítmény hello és hajtanak végre a parancsfájlok az Azure virtuális gépeken. A bővítmény akkor hasznos, ha a feladás egy vagy több központi telepítés konfigurálása, a szoftver telepítése vagy a más beállításokat / kezelési feladatot. Parancsfájlok le: az Azure storage vagy a Githubon, vagy megadott toohello az Azure portálon, a bővítmény futási időt. Egyéni parancsprogramok futtatására szolgáló bővítmény hello integrálódik az Azure Resource Manager-sablonok, és is futtathat a hello Azure CLI, PowerShell, Azure-portálon vagy hello Azure virtuális gép REST API használatával.

Ez a dokumentum részletesen, hogyan toouse hello egyéni parancsprogramok futtatására szolgáló bővítmény használatával hello a Azure PowerShell-modult, Azure Resource Manager-sablonok és a Windows rendszer hibaelhárítási részleteket.

## <a name="prerequisites"></a>Előfeltételek

### <a name="operating-system"></a>Operációs rendszer

Egyéni parancsprogramok futtatására szolgáló bővítmény hello a Windows is futtathatók a Windows Server 2008 R2, 2012, 2012 R2 és 2016 kiadását.

### <a name="script-location"></a>Parancsfájl helyét

hello parancsfájl az Azure storage vagy egy érvényes URL-cím elérhető bármely más helyen tárolt toobe kell.

### <a name="internet-connectivity"></a>Internetkapcsolat

hello egyéni parancsfájl kiterjesztése Windows rendszerhez szükséges, hogy hello a cél virtuális gép csatlakoztatott toohello internet. 

## <a name="extension-schema"></a>A séma kiterjesztése

hello következő JSON jeleníti meg az egyéni parancsprogramok futtatására szolgáló bővítmény hello hello séma. egy parancsfájl helyét (Azure Storage vagy más helyre érvényes URL-CÍMMEL rendelkező), valamint egy parancs tooexecute hello bővítmény kell rendelkeznie. Ha használja az Azure Storage hello parancsfájl forrásaként, az Azure storage fiók név és fiókkulcs kulcsot meg kell adni. Legyen bizalmas adatokat a rendszer ezeket az elemeket, és hello bővítmények védett beállítás konfigurációjában megadott. Azure virtuális gépekre vonatkozó beállításával bővítmény védett adatok titkosítva, és csak visszafejteni hello cél virtuális gépen.

```json
{
    "name": "config-app",
    "type": "Microsoft.ClassicCompute/virtualMachines/extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-01",
    "properties": {
        "publisher": "Microsoft.Compute",
        "extension": "CustomScriptExtension",
        "version": "1.8",
        "parameters": {
            "public": {
                "fileUris": "[myScriptLocation]"
            },
            "private": {
                "commandToExecute": "[myExecutionString]"
            }
        }
    }
}
```

### <a name="property-values"></a>A tulajdonság értékek

| Név | Érték / – példa |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Közzétevő | Microsoft.Compute |
| Bővítmény | CustomScriptExtension |
| typeHandlerVersion | 1.8 |
| fileUris (például) | https://RAW.githubusercontent.com/Microsoft/DotNet-Core-sample-Templates/Master/DotNet-Core-Music-Windows/scripts/Configure-Music-App.ps1 |
| commandToExecute (például) | PowerShell - ExecutionPolicy Unrestricted - fájl konfigurálása zene-app.ps1 |

## <a name="template-deployment"></a>Sablonalapú telepítés

Az Azure Virtuálisgép-bővítmények az Azure Resource Manager-sablonok is telepíthető. hello JSON-séma hello előző szakaszban ismertetett az Azure Resource Manager sablon toorun hello egyéni parancsprogramok futtatására szolgáló bővítmény egy Azure Resource Manager sablon telepítése során használható. Amely tartalmazza az egyéni parancsprogramok futtatására szolgáló bővítmény itt található hello mintasablon [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="powershell-deployment"></a>PowerShell telepítése

Hello `Set-AzureVMCustomScriptExtension` parancs lehet használt tooadd hello egyéni parancsfájl kiterjesztése tooan meglévő virtuális gépet. További információkért lásd: [Set-AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).

```powershell
# create vm object
$vm = Get-AzureVM -Name 2016clas -ServiceName 2016clas1313

# set extension
Set-AzureVMCustomScriptExtension -VM $vm -FileUri myFileUri -Run 'create-file.ps1'

# update vm
$vm | Update-AzureVM
```

## <a name="troubleshoot-and-support"></a>Hibaelhárítás és támogatás

### <a name="troubleshoot"></a>Hibaelhárítás

A bővítmény központi telepítések hello állapotával kapcsolatos információkat lehet adatokat beolvasni az Azure-portálon hello és hello Azure PowerShell modul használatával. toosee hello telepítési állapota egy adott virtuális Gépet, futtassa a következő parancs hello-bővítmények.

```powershell
Get-AzureVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Bővítmény végrehajtási kimeneti velünk naplózott toofiles hello directory következő hello cél virtuális gépen található.

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

hello parancsfájl önmagában directory hello cél virtuális gépen a következő hello be lesznek letöltve.

```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads
```

### <a name="support"></a>Támogatás

Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure szakértői hello hello [MSDN Azure és a Stack Overflow fórumok](https://azure.microsoft.com/en-us/support/forums/). Másik lehetőségként is fájl az Azure támogatási incidens. Nyissa meg toohello [az Azure támogatási webhelyén](https://azure.microsoft.com/en-us/support/options/) válassza ki a Get-támogatást. Támogatja az Azure használatával kapcsolatos információkért olvassa el a hello [Microsoft Azure-támogatás – gyakori kérdések](https://azure.microsoft.com/en-us/support/faq/).
