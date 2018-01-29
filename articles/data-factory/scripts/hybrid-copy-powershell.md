---
title: "PowerShell-parancsprogram: adatok másolása az helyszíni az Azure Data Factory használatával |} Microsoft Docs"
description: "A PowerShell parancsfájl másol adatokat egy helyi SQL Server-adatbázis egy másik egy Azure Blob Storage tárolóban."
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
ms.openlocfilehash: 7f062a58482ad72e3dd3844431205502b4c44786
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/01/2017
---
# <a name="use-powershell-to-create-a-data-factory-pipeline-to-copy-data-from-on-premises-to-azure"></a>A helyszíni adatok másolása az Azure data factory folyamatokat létrehozni a PowerShell használatával

A PowerShell-parancsfájlpélda egy folyamatot, amely adatokat másol egy helyi SQL Server-adatbázis egy Azure Blob Storage Azure Data Factory hoz létre.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Előfeltételek

- **Egy SQL Server**. Mint a helyszíni SQL Server adatbázis használata a **forrás** adatok tárolása a minta.
- **Azure Storage-fiók** Azure blob Storage tárolót használja a **cél/fogadó** adatok tárolása a minta. Ha még nem rendelkezik Azure Storage-fiókkal, a létrehozás folyamatáért lásd a [tárfiók létrehozását](../../storage/common/storage-create-storage-account.md#create-a-storage-account) ismertető cikket.
- **Önállóan üzemel integrációs futásidejű**. Az MSI-fájl letöltésére a [letöltőközpontból](https://www.microsoft.com/download/details.aspx?id=39717) , majd futtassa egy önálló üzemeltetett integrációs futásidejű telepítése a számítógépre.  

### <a name="create-sample-database-in-sql-server"></a>Hozzon létre mintaadatbázist az SQL Server
1. A helyszíni SQL Server adatbázisban nevű tábla létrehozása **üres** a következő SQL-parancsfájl használatával: 

   ```sql   
     CREATE TABLE dbo.emp
     (
         ID int IDENTITY(1,1) NOT NULL,
         FirstName varchar(50),
         LastName varchar(50),
         CONSTRAINT PK_emp PRIMARY KEY (ID)
     )
     GO
   ```

2. Szúrja be a mintaadatok a táblázatba:

   ```sql
     INSERT INTO emp VALUES ('John', 'Doe')
     INSERT INTO emp VALUES ('Jane', 'Doe')
   ```

## <a name="sample-script"></a>Mintaparancsfájl

> [!IMPORTANT]
> Ezt a parancsfájlt a merevlemezen a c:\ mappában hoz létre, amelyek meghatározzák a Data Factory entitások (a társított szolgáltatás, a dataset és a feldolgozási sor) JSON-fájlokat.

[!code-powershell[main](../../../powershell_scripts/data-factory/copy-from-onprem-sql-server-to-azure-blob/copy-from-onprem-sql-server-to-azure-blob.ps1 "Copy from on-premises SQL Server -> Azure Blob Storage")]


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
| [Új AzureRmDataFactoryV2LinkedServiceEncryptCredential](/powershell/module/azurerm.datafactoryv2/new-azurermdatafactoryv2linkedserviceencryptedcredential) | A kapcsolódószolgáltatás-felhasználó hitelesítő adatait titkosítja, és létrehoz egy új kapcsolódószolgáltatás-definíció a titkosított hitelesítő adatokat. 
| [Set-AzureRmDataFactoryV2LinkedService](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2linkedservice) | Létrehoz egy csatolt szolgáltatást az adat-előállítóban. A társított szolgáltatás egy adat-előállító egy számítási vagy az adattárban hivatkozásokat tartalmaz. |
| [Set-AzureRmDataFactoryV2Dataset](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2dataset) | A data factory dataset hoz létre. A DataSet adatkészlet egy folyamat egy tevékenység bemeneti jelöli. | 
| [Set-AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactorv2ypipeline) | Az adat-előállítóban hoz létre egy folyamatot. Egy folyamatot, amely bizonyos műveletet hajt végre egy vagy több tevékenységet tartalmaz. Az adatcsatorna a másolási tevékenység adatainak másolása egyik helyről egy Azure Blob Storage tárolóban egy másik helyre. |
| [Invoke-AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Invoke-azurermdatafactoryv2pipelinerun) | A tölcsér futtató hoz létre. Ez azt jelenti futtatja a folyamatot. |
| [Get-AzureRmDataFactoryV2ActivityRun](/powershell/module/azurerm.datafactoryv2/get-azurermdatafactoryv2activityrun) | Lekérdezi a futtatáskor a tevékenység (tevékenységfuttatási) adatait a feldolgozási. 
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |
|||

## <a name="next-steps"></a>Következő lépések

Az Azure PowerShell további információkért lásd: [Azure PowerShell dokumentációs](https://docs.microsoft.com/powershell/).

További Azure Data Factory PowerShell parancsfájl minták megtalálhatók a [Azure Data Factory PowerShell-példák](../samples-powershell.md).