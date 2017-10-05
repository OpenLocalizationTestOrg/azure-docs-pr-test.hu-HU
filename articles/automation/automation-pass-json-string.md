---
title: "Egy JSON-objektum átadása egy Azure Automation-runbook |} Microsoft Docs"
description: "Egy runbook JSON-objektumként paraméterek továbbítása"
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
ms.openlocfilehash: eac0e95a46731b9d396ea0590e629d61ca6a7d70
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="pass-a-json-object-to-an-azure-automation-runbook"></a><span data-ttu-id="8c644-104">Egy JSON-objektum átadása egy Azure Automation-runbook</span><span class="sxs-lookup"><span data-stu-id="8c644-104">Pass a JSON object to an Azure Automation runbook</span></span>

<span data-ttu-id="8c644-105">Lehet hasznos, ha szeretné, hogy egy runbook egy JSON-fájlban használni kívánt adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="8c644-105">It can be useful to store data that you want to pass to a runbook in a JSON file.</span></span>
<span data-ttu-id="8c644-106">Például előfordulhat, hogy hozzon létre egy JSON-fájl, amely tartalmazza az összes runbook átadni kívánt paramétert.</span><span class="sxs-lookup"><span data-stu-id="8c644-106">For example, you might create a JSON file that contains all of the parameters you want to pass to a runbook.</span></span>
<span data-ttu-id="8c644-107">Ehhez az szükséges, akkor a JSON alakítható át karakterlánccá, majd a karakterlánc át kell alakítania egy PowerShell-objektum előtt annak tartalmát a runbookhoz.</span><span class="sxs-lookup"><span data-stu-id="8c644-107">To do this, you have to convert the JSON to a string and then convert the string to a PowerShell object before passing its contents to the runbook.</span></span>

<span data-ttu-id="8c644-108">Ebben a példában, mi létrehozunk egy PowerShell-parancsfájlt, amely behívja [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) PowerShell runbook, a tartalmát a JSON átadja a runbook indítása.</span><span class="sxs-lookup"><span data-stu-id="8c644-108">In this example, we'll create a PowerShell script that calls [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) to start a PowerShell runbook, passing the contents of the JSON to the runbook.</span></span>
<span data-ttu-id="8c644-109">A PowerShell-forgatókönyv egy Azure virtuális Gépen, a paraméterek lekérése lett átadva a JSON a virtuális gép elindul.</span><span class="sxs-lookup"><span data-stu-id="8c644-109">The PowerShell runbook starts an Azure VM, getting the parameters for the VM from the JSON that was passed in.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8c644-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8c644-110">Prerequisites</span></span>
<span data-ttu-id="8c644-111">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="8c644-111">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="8c644-112">Egy Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="8c644-112">Azure subscription.</span></span> <span data-ttu-id="8c644-113">Ha még nem rendelkezik fiókkal, [aktiválhatja MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), illetve <a href="/pricing/free-account/" target="_blank">[regisztrálhat egy ingyenes fiókot](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="8c644-113">If you don't have one yet, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or <a href="/pricing/free-account/" target="_blank">[sign up for a free account](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="8c644-114">[Automation-fiók](automation-sec-configure-azure-runas-account.md) a forgatókönyv tárolásához és az Azure erőforrásokban való hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="8c644-114">[Automation account](automation-sec-configure-azure-runas-account.md) to hold the runbook and authenticate to Azure resources.</span></span>  <span data-ttu-id="8c644-115">Ennek a fióknak jogosultsággal kell rendelkeznie a virtuális gép elindításához és leállításához.</span><span class="sxs-lookup"><span data-stu-id="8c644-115">This account must have permission to start and stop the virtual machine.</span></span>
* <span data-ttu-id="8c644-116">Egy Azure virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="8c644-116">An Azure virtual machine.</span></span> <span data-ttu-id="8c644-117">Ezt a gépet leállítjuk és elindítjuk, tehát ne olyan virtuális gépet használjon, amely élesben működik.</span><span class="sxs-lookup"><span data-stu-id="8c644-117">We stop and start this machine so it should not be a production VM.</span></span>
* <span data-ttu-id="8c644-118">Az Azure Powershell telepítve a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8c644-118">Azure Powershell installed on a local machine.</span></span> <span data-ttu-id="8c644-119">Lásd: [telepítse és konfigurálja az Azure Powershellt](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) hogyan Azure PowerShell kérhet információt.</span><span class="sxs-lookup"><span data-stu-id="8c644-119">See [Install and configure Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) for information about how to get Azure PowerShell.</span></span>

## <a name="create-the-json-file"></a><span data-ttu-id="8c644-120">A JSON-fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="8c644-120">Create the JSON file</span></span>

<span data-ttu-id="8c644-121">Írja be a következő teszt a fájlt, és mentse a fájt `test.json` valahol a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8c644-121">Type the following test in a text file, and save it as `test.json` somewhere on your local computer.</span></span>

```json
{
   "VmName" : "TestVM",
   "ResourceGroup" : "AzureAutomationTest"
}
```

## <a name="create-the-runbook"></a><span data-ttu-id="8c644-122">Hozza létre a runbookot</span><span class="sxs-lookup"><span data-stu-id="8c644-122">Create the runbook</span></span>

<span data-ttu-id="8c644-123">Hozzon létre egy új "Test-Json" az Azure Automationben nevű PowerShell-forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="8c644-123">Create a new PowerShell runbook named "Test-Json" in Azure Automation.</span></span>
<span data-ttu-id="8c644-124">Megtudhatja, hogyan hozzon létre egy új PowerShell runbookot, lásd: [az első PowerShell runbook](automation-first-runbook-textual-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="8c644-124">To learn how to create a new PowerShell runbook, see [My first PowerShell runbook](automation-first-runbook-textual-powershell.md).</span></span>

<span data-ttu-id="8c644-125">A JSON-adatok fogadására, a runbook egy objektum szükséges bemeneti paraméterként.</span><span class="sxs-lookup"><span data-stu-id="8c644-125">To accept the JSON data, the runbook must take an object as an input parameter.</span></span>

<span data-ttu-id="8c644-126">A runbook ezután használhatja a JSON-ban meghatározott tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="8c644-126">The runbook can then use the properties defined in the JSON.</span></span>

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

 <span data-ttu-id="8c644-127">Mentse, és tegye közzé ezt a runbookot az Automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="8c644-127">Save and publish this runbook in your Automation account.</span></span>

## <a name="call-the-runbook-from-powershell"></a><span data-ttu-id="8c644-128">A runbook hívja a Powershellből</span><span class="sxs-lookup"><span data-stu-id="8c644-128">Call the runbook from PowerShell</span></span>

<span data-ttu-id="8c644-129">Most hívása a runbook a helyi gép Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="8c644-129">Now you can call the runbook from your local machine by using Azure PowerShell.</span></span>
<span data-ttu-id="8c644-130">Futtassa a következő PowerShell-parancsokat:</span><span class="sxs-lookup"><span data-stu-id="8c644-130">Run the following PowerShell commands:</span></span>

1. <span data-ttu-id="8c644-131">Jelentkezzen be az Azure:</span><span class="sxs-lookup"><span data-stu-id="8c644-131">Log in to Azure:</span></span>
   ```powershell
   Login-AzureRmAccount
   ```
    <span data-ttu-id="8c644-132">Az Azure hitelesítő adatait kéri.</span><span class="sxs-lookup"><span data-stu-id="8c644-132">You are prompted to enter your Azure credentials.</span></span>
1. <span data-ttu-id="8c644-133">A JSON-fájl tartalmának beolvasása és konvertálható karakterláncra:</span><span class="sxs-lookup"><span data-stu-id="8c644-133">Get the contents of the JSON file and convert it to a string:</span></span>
    ```powershell
    $json =  (Get-content -path 'JsonPath\test.json' -Raw) | Out-string
    ```
    <span data-ttu-id="8c644-134">`JsonPath`az elérési utat, ahová mentette a JSON-fájl van.</span><span class="sxs-lookup"><span data-stu-id="8c644-134">`JsonPath` is the path where you saved the JSON file.</span></span>
1. <span data-ttu-id="8c644-135">Karakterlánc tartalmát `$json` PowerShell objektumhoz:</span><span class="sxs-lookup"><span data-stu-id="8c644-135">Convert the string contents of `$json` to a PowerShell object:</span></span>
   ```powershell
   $JsonParams = @{"json"=$json}
   ```
1. <span data-ttu-id="8c644-136">Létrehozni egy kivonattáblát a paramétereinek `Start-AzureRmAutomstionRunbook`:</span><span class="sxs-lookup"><span data-stu-id="8c644-136">Create a hashtable for the parameters for `Start-AzureRmAutomstionRunbook`:</span></span>
   ```powershell
   $RBParams = @{
        AutomationAccountName = 'AATest'
        ResourceGroupName = 'RGTest'
        Name = 'Test-Json'
        Parameters = $JsonParams
   }
   ```
   <span data-ttu-id="8c644-137">Figyelje meg, az értéke határozza meg `Parameters` és a PowerShell-objektum, amely tartalmazza a JSON-fájl értékeinek.</span><span class="sxs-lookup"><span data-stu-id="8c644-137">Notice that you are setting the value of `Parameters` to the PowerShell object that contains the values from the JSON file.</span></span> 
1. <span data-ttu-id="8c644-138">Elindítja a runbookot</span><span class="sxs-lookup"><span data-stu-id="8c644-138">Start the runbook</span></span>
   ```powershell
   $job = Start-AzureRmAutomationRunbook @RBParams
   ```

<span data-ttu-id="8c644-139">A runbook értékeket használja a JSON-fájlt a virtuális gép elindításához.</span><span class="sxs-lookup"><span data-stu-id="8c644-139">The runbook uses the values from the JSON file to start a VM.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8c644-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8c644-140">Next steps</span></span>

* <span data-ttu-id="8c644-141">Szöveges szerkesztővel PowerShell és a PowerShell-munkafolyamati forgatókönyvek szerkesztésével kapcsolatos további tudnivalókért lásd: [szöveges az Azure Automation runbookjai szerkesztése](automation-edit-textual-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="8c644-141">To learn more about editing PowerShell and PowerShell Workflow runbooks with a textual editor, see [Editing textual runbooks in Azure Automation](automation-edit-textual-runbook.md)</span></span> 
* <span data-ttu-id="8c644-142">További információ létrehozása és importálása a runbookok további tudnivalókért lásd: [létrehozása vagy egy Azure Automation forgatókönyv importálása](automation-creating-importing-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="8c644-142">To learn more about creating and importing runbooks, see [Creating or importing a runbook in Azure Automation](automation-creating-importing-runbook.md)</span></span>


