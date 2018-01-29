---
title: "PowerShell-parancsprogram: adatok Növekményesen betöltése az Azure Data Factory használatával |} Microsoft Docs"
description: "A PowerShell-parancsfájl bemutatja, hogyan használható az Azure Data Factory adatok másolása Növekményesen egy Azure SQL Database az Azure Blob Storage..."
services: data-factory
author: spelluru
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2017
ms.author: spelluru
ms.openlocfilehash: 36a8c8c4ed83a56dfd0f487ec4bcf030e3890ef6
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/01/2017
---
# <a name="powershell-script---incrementally-load-data-by-using-azure-data-factory"></a>PowerShell parancsfájl - adatok betöltése Növekményesen az Azure Data Factory használatával
A PowerShell-parancsfájlpélda betölti csak új vagy frissített rekordokat egy forrás adattárból e fogadó adattárba után a forrásból származó adatokat a fogadó kezdeti teljes másolata.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

Lásd: [oktatóanyag: növekményes másolatot](../tutorial-incremental-copy-powershell.md#prerequisites) Ez a minta futtatásához előfeltételeinek. 

## <a name="sample-script"></a>Mintaparancsfájl

> [!IMPORTANT]
> Ezt a parancsfájlt a merevlemezen a c:\ mappában hoz létre, amelyek meghatározzák a Data Factory entitások (a társított szolgáltatás, a dataset és a feldolgozási sor) JSON-fájlokat.

[!code-powershell[main](../../../powershell_scripts/data-factory/incremental-copy-from-azure-sql-to-blob/incremental-copy-from-azure-sql-to-blob.ps1 "Incremental copy from Azure SQL Database to Azure Blob Storage")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása

A minta-parancsprogram futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot és a vele társított összes erőforrás:

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourceGroupName
```
Az erőforráscsoport az adat-előállítóban eltávolításához futtassa a következő parancsot: 

```powershell
Remove-AzureRmDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

Ezt a parancsfájlt az alábbi parancsokat használja: 

| Parancs | Megjegyzések |
|---|---|
| [Új-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [Set-AzureRmDataFactoryV2](/powershell/module/azurerm.datafactoryv2/set-azurermdatafactoryv2) | Adat-előállító létrehozása |
| [Set-AzureRmDataFactoryV2LinkedService](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2linkedservice) | Létrehoz egy csatolt szolgáltatást az adat-előállítóban. A társított szolgáltatás egy adat-előállító egy számítási vagy az adattárban hivatkozásokat tartalmaz. |
| [Set-AzureRmDataFactoryV2Dataset](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2dataset) | A data factory dataset hoz létre. A DataSet adatkészlet egy folyamat egy tevékenység bemeneti jelöli. | 
| [Set-AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactorv2ypipeline) | Az adat-előállítóban hoz létre egy folyamatot. Egy folyamatot, amely bizonyos műveletet hajt végre egy vagy több tevékenységet tartalmaz. Az adatcsatorna a másolási tevékenység adatainak másolása egyik helyről egy Azure Blob Storage tárolóban egy másik helyre. |
| [Invoke-AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Invoke-azurermdatafactoryv2pipelinerun) | A tölcsér futtató hoz létre. Ez azt jelenti futtatja a folyamatot. |
| [Get-AzureRmDataFactoryV2ActivityRun](/powershell/module/azurerm.datafactoryv2/get-azurermdatafactoryv2activityrun) | Lekérdezi a futtatáskor a tevékenység (tevékenységfuttatási) adatait a feldolgozási. 
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |
|||

## <a name="next-steps"></a>Következő lépések

Az Azure PowerShell további információkért lásd: [Azure PowerShell dokumentációs](https://docs.microsoft.com/powershell/).

További Azure Data Factory PowerShell parancsfájl minták megtalálhatók a [Azure Data Factory PowerShell-parancsfájlok](../samples-powershell.md).