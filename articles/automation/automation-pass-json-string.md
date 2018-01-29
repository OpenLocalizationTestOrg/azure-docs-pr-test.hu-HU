---
title: "Egy JSON-objektum átadása egy Azure Automation-runbook |} Microsoft Docs"
description: "Egy runbook JSON-objektumként paraméterek továbbítása"
services: automation
documentationcenter: dev-center-name
author: georgewallace
manager: carmonm
keywords: PowerShell, a runbook, json, az azure automation
ms.service: automation
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: TBD
ms.date: 06/15/2017
ms.author: gwallace
ms.openlocfilehash: 5390ba34a25713aed84d6e778335e30f27c2b1f8
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/14/2017
---
# <a name="pass-a-json-object-to-an-azure-automation-runbook"></a>Egy JSON-objektum átadása egy Azure Automation-runbook

Lehet hasznos, ha szeretné, hogy egy runbook egy JSON-fájlban használni kívánt adatok tárolásához.
Például előfordulhat, hogy hozzon létre egy JSON-fájl, amely tartalmazza az összes runbook átadni kívánt paramétert.
Ehhez az szükséges, akkor a JSON alakítható át karakterlánccá, majd a karakterlánc át kell alakítania egy PowerShell-objektum előtt annak tartalmát a runbookhoz.

Ebben a példában, mi létrehozunk egy PowerShell-parancsfájlt, amely behívja [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) PowerShell runbook, a tartalmát a JSON átadja a runbook indítása.
A PowerShell-forgatókönyv egy Azure virtuális Gépen, a paraméterek lekérése lett átadva a JSON a virtuális gép elindul.

## <a name="prerequisites"></a>Előfeltételek
Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:

* Egy Azure-előfizetés. Ha még nem rendelkezik fiókkal, [aktiválhatja MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), illetve <a href="/pricing/free-account/" target="_blank">[regisztrálhat egy ingyenes fiókot](https://azure.microsoft.com/free/).
* [Automation-fiók](automation-sec-configure-azure-runas-account.md) a forgatókönyv tárolásához és az Azure erőforrásokban való hitelesítéshez.  Ennek a fióknak jogosultsággal kell rendelkeznie a virtuális gép elindításához és leállításához.
* Egy Azure virtuális gép. Ezt a gépet leállítjuk és elindítjuk, tehát ne olyan virtuális gépet használjon, amely élesben működik.
* Az Azure Powershell telepítve a helyi számítógépen. Lásd: [telepítse és konfigurálja az Azure Powershellt](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) hogyan Azure PowerShell kérhet információt.

## <a name="create-the-json-file"></a>A JSON-fájl létrehozása

Írja be a következő teszt a fájlt, és mentse a fájt `test.json` valahol a helyi számítógépen.

```json
{
   "VmName" : "TestVM",
   "ResourceGroup" : "AzureAutomationTest"
}
```

## <a name="create-the-runbook"></a>Hozza létre a runbookot

Hozzon létre egy új "Test-Json" az Azure Automationben nevű PowerShell-forgatókönyv.
Megtudhatja, hogyan hozzon létre egy új PowerShell runbookot, lásd: [az első PowerShell runbook](automation-first-runbook-textual-powershell.md).

A JSON-adatok fogadására, a runbook egy objektum szükséges bemeneti paraméterként.

A runbook ezután használhatja a JSON-ban meghatározott tulajdonságokat.

```powershell
Param(
     [parameter(Mandatory=$true)]
     [object]$json
)

# Connect to Azure account   
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

# Convert object to actual JSON
$json = $json | ConvertFrom-Json

# Use the values from the JSON object as the parameters for your command
Start-AzureRmVM -Name $json.VMName -ResourceGroupName $json.ResourceGroup
 ```

 Mentse, és tegye közzé ezt a runbookot az Automation-fiókban.

## <a name="call-the-runbook-from-powershell"></a>A runbook hívja a Powershellből

Most hívása a runbook a helyi gép Azure PowerShell használatával.
Futtassa a következő PowerShell-parancsokat:

1. Jelentkezzen be az Azure:
   ```powershell
   Login-AzureRmAccount
   ```
    Az Azure hitelesítő adatait kéri.
1. A JSON-fájl tartalmának beolvasása és konvertálható karakterláncra:
    ```powershell
    $json =  (Get-content -path 'JsonPath\test.json' -Raw) | Out-string
    ```
    `JsonPath`az elérési utat, ahová mentette a JSON-fájl van.
1. Karakterlánc tartalmát `$json` PowerShell objektumhoz:
   ```powershell
   $JsonParams = @{"json"=$json}
   ```
1. Létrehozni egy kivonattáblát a paramétereinek `Start-AzureRmAutomationRunbook`:
   ```powershell
   $RBParams = @{
        AutomationAccountName = 'AATest'
        ResourceGroupName = 'RGTest'
        Name = 'Test-Json'
        Parameters = $JsonParams
   }
   ```
   Figyelje meg, az értéke határozza meg `Parameters` és a PowerShell-objektum, amely tartalmazza a JSON-fájl értékeinek. 
1. Elindítja a runbookot
   ```powershell
   $job = Start-AzureRmAutomationRunbook @RBParams
   ```

A runbook értékeket használja a JSON-fájlt a virtuális gép elindításához.

## <a name="next-steps"></a>Következő lépések

* Szöveges szerkesztővel PowerShell és a PowerShell-munkafolyamati forgatókönyvek szerkesztésével kapcsolatos további tudnivalókért lásd: [szöveges az Azure Automation runbookjai szerkesztése](automation-edit-textual-runbook.md) 
* További információ létrehozása és importálása a runbookok további tudnivalókért lásd: [létrehozása vagy egy Azure Automation forgatókönyv importálása](automation-creating-importing-runbook.md)


