---
title: a JSON aaaPass objektum tooan Azure Automation-runbook |} Microsoft Docs
description: "Hogyan toopass paraméterek tooa runbook JSON-objektumként"
services: automation
documentationcenter: dev-center-name
author: eslesar
manager: carmonm
keywords: PowerShell, a runbook, json, az azure automation
ms.service: automation
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: TBD
ms.date: 06/15/2017
ms.author: eslesar
ms.openlocfilehash: 8229a16015d549927ead5496c70e9fb391d35498
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="pass-a-json-object-tooan-azure-automation-runbook"></a>Adjon át egy JSON-objektum tooan Azure Automation-forgatókönyv

Ez lehet a hasznos toostore adat, amelyet az toopass tooa runbook egy JSON-fájlban.
Például létrehozhat egy JSON-fájl, amely tartalmazza az összes hello paraméterek toopass tooa runbook szeretné.
toodo, hogy tooconvert hello JSON tooa karakterlánc és alakítsa át a tartalmak toohello runbook előtt hello karakterlánc tooa PowerShell-objektum.

Ebben a példában, mi létrehozunk egy PowerShell-parancsfájlt, amely behívja [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) toostart hello JSON toohello runbook tartalmának hello átadásakor PowerShell-forgatókönyv.
hello PowerShell runbook elindul egy Azure virtuális Gépen, hello paraméterek hello lett átadva a JSON-NÁ. a virtuális gép hello lekérése.

## <a name="prerequisites"></a>Előfeltételek
toocomplete ebben az oktatóanyagban a következő hello szüksége:

* Egy Azure-előfizetés. Ha még nem rendelkezik fiókkal, [aktiválhatja MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), illetve <a href="/pricing/free-account/" target="_blank">[regisztrálhat egy ingyenes fiókot](https://azure.microsoft.com/free/).
* [Automation-fiók](automation-sec-configure-azure-runas-account.md) toohold hello runbook és tooAzure erőforrások hitelesítéséhez.  Ezt a fiókot kell toostart engedéllyel rendelkezik, és állítsa le a virtuális gép hello.
* Egy Azure virtuális gép. Ezt a gépet leállítjuk és elindítjuk, tehát ne olyan virtuális gépet használjon, amely élesben működik.
* Az Azure Powershell telepítve a helyi számítógépen. Lásd: [telepítse és konfigurálja az Azure Powershellt](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) információ tooget Azure PowerShell.

## <a name="create-hello-json-file"></a>Hello JSON-fájl létrehozása

Típus hello következő tesztelése a fájlt, és mentse a fájt `test.json` valahol a helyi számítógépen.

```json
{
   "VmName" : "TestVM",
   "ResourceGroup" : "AzureAutomationTest"
}
```

## <a name="create-hello-runbook"></a>Hello runbook létrehozása

Hozzon létre egy új "Test-Json" az Azure Automationben nevű PowerShell-forgatókönyv.
Hogyan toocreate egy új PowerShell-forgatókönyv: toolearn [az első PowerShell runbook](automation-first-runbook-textual-powershell.md).

tooaccept hello JSON-adatokat, hello runbook objektum szükséges bemeneti paraméterként.

hello runbook használhatja a hello JSON meghatározott hello tulajdonság.

```powershell
Param(
     [parameter(Mandatory=$true)]
     [object]$json
)

# Connect tooAzure account   
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

# Convert object tooactual JSON
$json = $json | ConvertFrom-Json

# Use hello values from hello JSON object as hello parameters for your command
Start-AzureRmVM -Name $json.VMName -ResourceGroupName $json.ResourceGroup
 ```

 Mentse, és tegye közzé ezt a runbookot az Automation-fiókban.

## <a name="call-hello-runbook-from-powershell"></a>Hívás hello runbook PowerShell

Most hívása hello runbook a helyi gép Azure PowerShell használatával.
Futtassa a következő PowerShell-parancsok hello:

1. Jelentkezzen be tooAzure:
   ```powershell
   Login-AzureRmAccount
   ```
    Meg vannak felszólító tooenter Azure hitelesítő adatait.
1. Első hello hello JSON-fájl tartalmát, és konvertálja tooa karakterlánc:
    ```powershell
    $json =  (Get-content -path 'JsonPath\test.json' -Raw) | Out-string
    ```
    `JsonPath`hello elérési utat, ahová mentette hello JSON-fájl van.
1. Átalakítás hello karakterlánc tartalmát `$json` tooa PowerShell objektum:
   ```powershell
   $JsonParams = @{"json"=$json}
   ```
1. Létrehozni egy kivonattáblát hello paramétereinek `Start-AzureRmAutomstionRunbook`:
   ```powershell
   $RBParams = @{
        AutomationAccountName = 'AATest'
        ResourceGroupName = 'RGTest'
        Name = 'Test-Json'
        Parameters = $JsonParams
   }
   ```
   Figyelje meg, hogy hello értékét állítja `Parameters` toohello PowerShell-objektum, amely a JSON-fájl hello hello értékeket tartalmaz. 
1. Hello runbook elindítása
   ```powershell
   $job = Start-AzureRmAutomationRunbook @RBParams
   ```

hello runbook hello JSON-fájl toostart egy virtuális gép hello értékeit használja.

## <a name="next-steps"></a>Következő lépések

* További információ a szöveges szerkesztőt, PowerShell és a PowerShell-munkafolyamati forgatókönyvek Szerkesztés toolearn lásd [szöveges az Azure Automation runbookjai szerkesztése](automation-edit-textual-runbook.md) 
* További információ az létrehozása és importálása a runbookok, toolearn lásd: [létrehozása vagy egy Azure Automation forgatókönyv importálása](automation-creating-importing-runbook.md)


