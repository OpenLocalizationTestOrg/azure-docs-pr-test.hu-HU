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
# <a name="pass-a-json-object-tooan-azure-automation-runbook"></a><span data-ttu-id="c3bce-104">Adjon át egy JSON-objektum tooan Azure Automation-forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="c3bce-104">Pass a JSON object tooan Azure Automation runbook</span></span>

<span data-ttu-id="c3bce-105">Ez lehet a hasznos toostore adat, amelyet az toopass tooa runbook egy JSON-fájlban.</span><span class="sxs-lookup"><span data-stu-id="c3bce-105">It can be useful toostore data that you want toopass tooa runbook in a JSON file.</span></span>
<span data-ttu-id="c3bce-106">Például létrehozhat egy JSON-fájl, amely tartalmazza az összes hello paraméterek toopass tooa runbook szeretné.</span><span class="sxs-lookup"><span data-stu-id="c3bce-106">For example, you might create a JSON file that contains all of hello parameters you want toopass tooa runbook.</span></span>
<span data-ttu-id="c3bce-107">toodo, hogy tooconvert hello JSON tooa karakterlánc és alakítsa át a tartalmak toohello runbook előtt hello karakterlánc tooa PowerShell-objektum.</span><span class="sxs-lookup"><span data-stu-id="c3bce-107">toodo this, you have tooconvert hello JSON tooa string and then convert hello string tooa PowerShell object before passing its contents toohello runbook.</span></span>

<span data-ttu-id="c3bce-108">Ebben a példában, mi létrehozunk egy PowerShell-parancsfájlt, amely behívja [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) toostart hello JSON toohello runbook tartalmának hello átadásakor PowerShell-forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="c3bce-108">In this example, we'll create a PowerShell script that calls [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) toostart a PowerShell runbook, passing hello contents of hello JSON toohello runbook.</span></span>
<span data-ttu-id="c3bce-109">hello PowerShell runbook elindul egy Azure virtuális Gépen, hello paraméterek hello lett átadva a JSON-NÁ. a virtuális gép hello lekérése.</span><span class="sxs-lookup"><span data-stu-id="c3bce-109">hello PowerShell runbook starts an Azure VM, getting hello parameters for hello VM from hello JSON that was passed in.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c3bce-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c3bce-110">Prerequisites</span></span>
<span data-ttu-id="c3bce-111">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="c3bce-111">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="c3bce-112">Egy Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="c3bce-112">Azure subscription.</span></span> <span data-ttu-id="c3bce-113">Ha még nem rendelkezik fiókkal, [aktiválhatja MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), illetve <a href="/pricing/free-account/" target="_blank">[regisztrálhat egy ingyenes fiókot](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="c3bce-113">If you don't have one yet, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or <a href="/pricing/free-account/" target="_blank">[sign up for a free account](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="c3bce-114">[Automation-fiók](automation-sec-configure-azure-runas-account.md) toohold hello runbook és tooAzure erőforrások hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="c3bce-114">[Automation account](automation-sec-configure-azure-runas-account.md) toohold hello runbook and authenticate tooAzure resources.</span></span>  <span data-ttu-id="c3bce-115">Ezt a fiókot kell toostart engedéllyel rendelkezik, és állítsa le a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="c3bce-115">This account must have permission toostart and stop hello virtual machine.</span></span>
* <span data-ttu-id="c3bce-116">Egy Azure virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="c3bce-116">An Azure virtual machine.</span></span> <span data-ttu-id="c3bce-117">Ezt a gépet leállítjuk és elindítjuk, tehát ne olyan virtuális gépet használjon, amely élesben működik.</span><span class="sxs-lookup"><span data-stu-id="c3bce-117">We stop and start this machine so it should not be a production VM.</span></span>
* <span data-ttu-id="c3bce-118">Az Azure Powershell telepítve a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c3bce-118">Azure Powershell installed on a local machine.</span></span> <span data-ttu-id="c3bce-119">Lásd: [telepítse és konfigurálja az Azure Powershellt](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) információ tooget Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c3bce-119">See [Install and configure Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) for information about how tooget Azure PowerShell.</span></span>

## <a name="create-hello-json-file"></a><span data-ttu-id="c3bce-120">Hello JSON-fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="c3bce-120">Create hello JSON file</span></span>

<span data-ttu-id="c3bce-121">Típus hello következő tesztelése a fájlt, és mentse a fájt `test.json` valahol a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c3bce-121">Type hello following test in a text file, and save it as `test.json` somewhere on your local computer.</span></span>

```json
{
   "VmName" : "TestVM",
   "ResourceGroup" : "AzureAutomationTest"
}
```

## <a name="create-hello-runbook"></a><span data-ttu-id="c3bce-122">Hello runbook létrehozása</span><span class="sxs-lookup"><span data-stu-id="c3bce-122">Create hello runbook</span></span>

<span data-ttu-id="c3bce-123">Hozzon létre egy új "Test-Json" az Azure Automationben nevű PowerShell-forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="c3bce-123">Create a new PowerShell runbook named "Test-Json" in Azure Automation.</span></span>
<span data-ttu-id="c3bce-124">Hogyan toocreate egy új PowerShell-forgatókönyv: toolearn [az első PowerShell runbook](automation-first-runbook-textual-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="c3bce-124">toolearn how toocreate a new PowerShell runbook, see [My first PowerShell runbook](automation-first-runbook-textual-powershell.md).</span></span>

<span data-ttu-id="c3bce-125">tooaccept hello JSON-adatokat, hello runbook objektum szükséges bemeneti paraméterként.</span><span class="sxs-lookup"><span data-stu-id="c3bce-125">tooaccept hello JSON data, hello runbook must take an object as an input parameter.</span></span>

<span data-ttu-id="c3bce-126">hello runbook használhatja a hello JSON meghatározott hello tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="c3bce-126">hello runbook can then use hello properties defined in hello JSON.</span></span>

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

 <span data-ttu-id="c3bce-127">Mentse, és tegye közzé ezt a runbookot az Automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="c3bce-127">Save and publish this runbook in your Automation account.</span></span>

## <a name="call-hello-runbook-from-powershell"></a><span data-ttu-id="c3bce-128">Hívás hello runbook PowerShell</span><span class="sxs-lookup"><span data-stu-id="c3bce-128">Call hello runbook from PowerShell</span></span>

<span data-ttu-id="c3bce-129">Most hívása hello runbook a helyi gép Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="c3bce-129">Now you can call hello runbook from your local machine by using Azure PowerShell.</span></span>
<span data-ttu-id="c3bce-130">Futtassa a következő PowerShell-parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="c3bce-130">Run hello following PowerShell commands:</span></span>

1. <span data-ttu-id="c3bce-131">Jelentkezzen be tooAzure:</span><span class="sxs-lookup"><span data-stu-id="c3bce-131">Log in tooAzure:</span></span>
   ```powershell
   Login-AzureRmAccount
   ```
    <span data-ttu-id="c3bce-132">Meg vannak felszólító tooenter Azure hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="c3bce-132">You are prompted tooenter your Azure credentials.</span></span>
1. <span data-ttu-id="c3bce-133">Első hello hello JSON-fájl tartalmát, és konvertálja tooa karakterlánc:</span><span class="sxs-lookup"><span data-stu-id="c3bce-133">Get hello contents of hello JSON file and convert it tooa string:</span></span>
    ```powershell
    $json =  (Get-content -path 'JsonPath\test.json' -Raw) | Out-string
    ```
    <span data-ttu-id="c3bce-134">`JsonPath`hello elérési utat, ahová mentette hello JSON-fájl van.</span><span class="sxs-lookup"><span data-stu-id="c3bce-134">`JsonPath` is hello path where you saved hello JSON file.</span></span>
1. <span data-ttu-id="c3bce-135">Átalakítás hello karakterlánc tartalmát `$json` tooa PowerShell objektum:</span><span class="sxs-lookup"><span data-stu-id="c3bce-135">Convert hello string contents of `$json` tooa PowerShell object:</span></span>
   ```powershell
   $JsonParams = @{"json"=$json}
   ```
1. <span data-ttu-id="c3bce-136">Létrehozni egy kivonattáblát hello paramétereinek `Start-AzureRmAutomstionRunbook`:</span><span class="sxs-lookup"><span data-stu-id="c3bce-136">Create a hashtable for hello parameters for `Start-AzureRmAutomstionRunbook`:</span></span>
   ```powershell
   $RBParams = @{
        AutomationAccountName = 'AATest'
        ResourceGroupName = 'RGTest'
        Name = 'Test-Json'
        Parameters = $JsonParams
   }
   ```
   <span data-ttu-id="c3bce-137">Figyelje meg, hogy hello értékét állítja `Parameters` toohello PowerShell-objektum, amely a JSON-fájl hello hello értékeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c3bce-137">Notice that you are setting hello value of `Parameters` toohello PowerShell object that contains hello values from hello JSON file.</span></span> 
1. <span data-ttu-id="c3bce-138">Hello runbook elindítása</span><span class="sxs-lookup"><span data-stu-id="c3bce-138">Start hello runbook</span></span>
   ```powershell
   $job = Start-AzureRmAutomationRunbook @RBParams
   ```

<span data-ttu-id="c3bce-139">hello runbook hello JSON-fájl toostart egy virtuális gép hello értékeit használja.</span><span class="sxs-lookup"><span data-stu-id="c3bce-139">hello runbook uses hello values from hello JSON file toostart a VM.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c3bce-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c3bce-140">Next steps</span></span>

* <span data-ttu-id="c3bce-141">További információ a szöveges szerkesztőt, PowerShell és a PowerShell-munkafolyamati forgatókönyvek Szerkesztés toolearn lásd [szöveges az Azure Automation runbookjai szerkesztése](automation-edit-textual-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="c3bce-141">toolearn more about editing PowerShell and PowerShell Workflow runbooks with a textual editor, see [Editing textual runbooks in Azure Automation](automation-edit-textual-runbook.md)</span></span> 
* <span data-ttu-id="c3bce-142">További információ az létrehozása és importálása a runbookok, toolearn lásd: [létrehozása vagy egy Azure Automation forgatókönyv importálása](automation-creating-importing-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="c3bce-142">toolearn more about creating and importing runbooks, see [Creating or importing a runbook in Azure Automation](automation-creating-importing-runbook.md)</span></span>


