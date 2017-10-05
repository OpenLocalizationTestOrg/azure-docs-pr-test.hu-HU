---
title: "Azure Automation DSC-konfigurációja fordítása |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan Azure Automation kívánt állapot konfigurációs szolgáltatása (DSC) konfigurációi összeállításához."
services: automation
documentationcenter: na
author: eslesar
manager: carmonm
ms.assetid: 49f20b31-4fa5-4712-b1c7-8f4409f1aecc
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 02/07/2017
ms.author: magoedte; eslesar
ms.openlocfilehash: 1aadd604e676659475f00760af3b0bdfb13a4792
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="compiling-configurations-in-azure-automation-dsc"></a><span data-ttu-id="63355-103">Azure Automation DSC-konfigurációja fordítása</span><span class="sxs-lookup"><span data-stu-id="63355-103">Compiling configurations in Azure Automation DSC</span></span>

<span data-ttu-id="63355-104">Az Azure Automation szolgáltatásban kétféle módon kívánt állapot konfigurációs szolgáltatása (DSC) konfigurációk állíthat össze: az Azure portálon, és a Windows PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="63355-104">You can compile Desired State Configuration (DSC) configurations in two ways with Azure Automation: in the Azure portal, and with Windows PowerShell.</span></span> <span data-ttu-id="63355-105">Az alábbi táblázat segítségével meghatározhatja, hogy az egyes jellemzők alapján módszer használatával:</span><span class="sxs-lookup"><span data-stu-id="63355-105">The following table will help you determine when to use which method based on the characteristics of each:</span></span>

### <a name="azure-portal"></a><span data-ttu-id="63355-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="63355-106">Azure portal</span></span>

* <span data-ttu-id="63355-107">Legegyszerűbb módja az interaktív felhasználói felülettel</span><span class="sxs-lookup"><span data-stu-id="63355-107">Simplest method with interactive user interface</span></span>
* <span data-ttu-id="63355-108">Egyszerű paraméter értékének megadására, képernyő</span><span class="sxs-lookup"><span data-stu-id="63355-108">Form to provide simple parameter values</span></span>
* <span data-ttu-id="63355-109">Könnyen nyomon követheti a feladat állapota</span><span class="sxs-lookup"><span data-stu-id="63355-109">Easily track job state</span></span>
* <span data-ttu-id="63355-110">Az Azure bejelentkezési hitelesített hozzáférés</span><span class="sxs-lookup"><span data-stu-id="63355-110">Access authenticated with Azure logon</span></span>

### <a name="windows-powershell"></a><span data-ttu-id="63355-111">Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="63355-111">Windows PowerShell</span></span>

* <span data-ttu-id="63355-112">A Windows PowerShell-parancsmagokkal a parancssorból hívható</span><span class="sxs-lookup"><span data-stu-id="63355-112">Call from command line with Windows PowerShell cmdlets</span></span>
* <span data-ttu-id="63355-113">Automatizált megoldást több lépést tartalmazhat</span><span class="sxs-lookup"><span data-stu-id="63355-113">Can be included in automated solution with multiple steps</span></span>
* <span data-ttu-id="63355-114">Adja meg az egyszerű és összetett paraméterértékek</span><span class="sxs-lookup"><span data-stu-id="63355-114">Provide simple and complex parameter values</span></span>
* <span data-ttu-id="63355-115">Feladat-állapotok nyomon követésére</span><span class="sxs-lookup"><span data-stu-id="63355-115">Track job state</span></span>
* <span data-ttu-id="63355-116">PowerShell-parancsmagok támogatásához szükséges ügyfél</span><span class="sxs-lookup"><span data-stu-id="63355-116">Client required to support PowerShell cmdlets</span></span>
* <span data-ttu-id="63355-117">Pass ConfigurationData</span><span class="sxs-lookup"><span data-stu-id="63355-117">Pass ConfigurationData</span></span>
* <span data-ttu-id="63355-118">A hitelesítő adatokat használó konfigurációk összeállítása</span><span class="sxs-lookup"><span data-stu-id="63355-118">Compile configurations that use credentials</span></span>

<span data-ttu-id="63355-119">Miután eldöntötte, hogy egy fordítási metódusra, követheti a vonatkozó fordítása el az alábbi eljárások.</span><span class="sxs-lookup"><span data-stu-id="63355-119">Once you have decided on a compilation method, you can follow the respective procedures below to start compiling.</span></span>

## <a name="compiling-a-dsc-configuration-with-the-azure-portal"></a><span data-ttu-id="63355-120">Az Azure-portálon a DSC-konfiguráció fordítása</span><span class="sxs-lookup"><span data-stu-id="63355-120">Compiling a DSC Configuration with the Azure portal</span></span>

1. <span data-ttu-id="63355-121">Az Automation-fiók kattintson **a DSC-konfigurációk**.</span><span class="sxs-lookup"><span data-stu-id="63355-121">From your Automation account, click **DSC Configurations**.</span></span>
2. <span data-ttu-id="63355-122">Kattintson a konfigurációját, és nyissa meg a panelt.</span><span class="sxs-lookup"><span data-stu-id="63355-122">Click a configuration to open its blade.</span></span>
3. <span data-ttu-id="63355-123">Kattintson a **fordítási**.</span><span class="sxs-lookup"><span data-stu-id="63355-123">Click **Compile**.</span></span>
4. <span data-ttu-id="63355-124">Ha a konfigurációs paraméterek nélkül, kérni fogja annak ellenőrzéséhez, hogy kívánja-e, hogy.</span><span class="sxs-lookup"><span data-stu-id="63355-124">If the configuration has no parameters, you will be prompted to confirm whether you want to compile it.</span></span> <span data-ttu-id="63355-125">Ha a konfigurációs paraméterek, a **fordítási konfigurációs** panel nyílik meg, megadhatja a paraméterértékek.</span><span class="sxs-lookup"><span data-stu-id="63355-125">If the configuration has parameters, the **Compile Configuration** blade will open so you can provide parameter values.</span></span> <span data-ttu-id="63355-126">Tekintse meg a [ **alapvető paraméterek** ](#basic-parameters) alatt további tájékoztatást talál a Paraméterek szakaszban.</span><span class="sxs-lookup"><span data-stu-id="63355-126">See the [**Basic Parameters**](#basic-parameters) section below for further details on parameters.</span></span>
5. <span data-ttu-id="63355-127">A **fordítási feladat** panel meg van nyitva, így nyomon követheti a fordítási feladat állapotát, és a csomópont-konfigurációt (MOF konfigurációs dokumentumok) az Azure Automation DSC-lekéréses kiszolgáló elhelyezését okozta.</span><span class="sxs-lookup"><span data-stu-id="63355-127">The **Compilation Job** blade is opened so that you can track the compilation job's status, and the node configurations (MOF configuration documents) it caused to be placed on the Azure Automation DSC Pull Server.</span></span>

## <a name="compiling-a-dsc-configuration-with-windows-powershell"></a><span data-ttu-id="63355-128">A Windows PowerShell DSC-konfiguráció fordítása</span><span class="sxs-lookup"><span data-stu-id="63355-128">Compiling a DSC Configuration with Windows PowerShell</span></span>

<span data-ttu-id="63355-129">Használhat [ `Start-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/start-azurermautomationdsccompilationjob) fordítása a Windows PowerShell indításához.</span><span class="sxs-lookup"><span data-stu-id="63355-129">You can use [`Start-AzureRmAutomationDscCompilationJob`](/powershell/module/azurerm.automation/start-azurermautomationdsccompilationjob) to start compiling with Windows PowerShell.</span></span> <span data-ttu-id="63355-130">Az alábbi példakód elindít egy nevű DSC-konfiguráció összeállításának **SampleConfig**.</span><span class="sxs-lookup"><span data-stu-id="63355-130">The following sample code starts compilation of a DSC configuration called **SampleConfig**.</span></span>

```powershell
Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"
```

<span data-ttu-id="63355-131">`Start-AzureRmAutomationDscCompilationJob`egy fordítási feladat objektumot állapotának nyomon követésére használható adja vissza.</span><span class="sxs-lookup"><span data-stu-id="63355-131">`Start-AzureRmAutomationDscCompilationJob` returns a compilation job object that you can use to track its status.</span></span> <span data-ttu-id="63355-132">Ezután használhatja a fordítási feladat objektum [ `Get-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjob) fordítási feladat állapotának megállapításához és [ `Get-AzureRmAutomationDscCompilationJobOutput` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjoboutput) az adatfolyamok (kimenet) megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="63355-132">You can then use this compilation job object with [`Get-AzureRmAutomationDscCompilationJob`](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjob) to determine the status of the compilation job, and [`Get-AzureRmAutomationDscCompilationJobOutput`](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjoboutput) to view its streams (output).</span></span> <span data-ttu-id="63355-133">Az alábbi példakód elindít összeállítása a **SampleConfig** konfigurációs, megvárja, amíg befejeződött, és megjeleníti az adatfolyamokat.</span><span class="sxs-lookup"><span data-stu-id="63355-133">The following sample code starts compilation of the **SampleConfig** configuration, waits until it has completed, and then displays its streams.</span></span>

```powershell
$CompilationJob = Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"

while($CompilationJob.EndTime –eq $null -and $CompilationJob.Exception –eq $null)
{
    $CompilationJob = $CompilationJob | Get-AzureRmAutomationDscCompilationJob
    Start-Sleep -Seconds 3
}

$CompilationJob | Get-AzureRmAutomationDscCompilationJobOutput –Stream Any
```

## <a name="basic-parameters"></a><span data-ttu-id="63355-134">Alapvető paraméterek</span><span class="sxs-lookup"><span data-stu-id="63355-134">Basic Parameters</span></span>
<span data-ttu-id="63355-135">A paraméterdeklarációhoz a DSC-konfigurációkkal, például a különböző és tulajdonságait, azonos Azure Automation-runbook működik.</span><span class="sxs-lookup"><span data-stu-id="63355-135">Parameter declaration in DSC configurations, including parameter types and properties, works the same as in Azure Automation runbooks.</span></span> <span data-ttu-id="63355-136">Lásd: [runbook elindítása az Azure Automationben](automation-starting-a-runbook.md) további információt a runbook-paraméterek.</span><span class="sxs-lookup"><span data-stu-id="63355-136">See [Starting a runbook in Azure Automation](automation-starting-a-runbook.md) to learn more about runbook parameters.</span></span>

<span data-ttu-id="63355-137">A következő példában két paramétereket **FeatureName** és **IsPresent**annak megállapításához, a tulajdonságok értékeit az **ParametersExample.sample** csomópont konfiguráció, a fordítás során jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="63355-137">The following example uses two parameters called **FeatureName** and **IsPresent**, to determine the values of properties in the **ParametersExample.sample** node configuration, generated during compilation.</span></span>

```powershell
Configuration ParametersExample
{
    param(
        [Parameter(Mandatory=$true)]

        [string] $FeatureName,

        [Parameter(Mandatory=$true)]
        [boolean] $IsPresent
    )

    $EnsureString = "Present"
    if($IsPresent -eq $false)
    {
        $EnsureString = "Absent"
    }

    Node "sample"
    {
        WindowsFeature ($FeatureName + "Feature")
        {
            Ensure = $EnsureString
            Name = $FeatureName
        }
    }
}
```

<span data-ttu-id="63355-138">Az Azure Automation DSC-portálon, vagy az Azure PowerShell alapvető paramétert használó DSC-konfigurációk állíthat össze:</span><span class="sxs-lookup"><span data-stu-id="63355-138">You can compile DSC Configurations that use basic parameters in the Azure Automation DSC portal, or with Azure PowerShell:</span></span>

### <a name="portal"></a><span data-ttu-id="63355-139">Portál</span><span class="sxs-lookup"><span data-stu-id="63355-139">Portal</span></span>

<span data-ttu-id="63355-140">A portálon írhatja paraméterértékek kattintás után **fordítási**.</span><span class="sxs-lookup"><span data-stu-id="63355-140">In the portal, you can enter parameter values after clicking **Compile**.</span></span>

![helyettesítő szöveg](./media/automation-dsc-compile/DSC_compiling_1.png)

### <a name="powershell"></a><span data-ttu-id="63355-142">PowerShell</span><span class="sxs-lookup"><span data-stu-id="63355-142">PowerShell</span></span>

<span data-ttu-id="63355-143">PowerShell-paraméterek a szükséges egy [hashtable](http://technet.microsoft.com/library/hh847780.aspx) ahol a kulcs megegyezik a paraméter nevével, és értéke egyenlő a paraméter értéke.</span><span class="sxs-lookup"><span data-stu-id="63355-143">PowerShell requires parameters in a [hashtable](http://technet.microsoft.com/library/hh847780.aspx) where the key matches the parameter name, and the value equals the parameter value.</span></span>

```powershell
$Parameters = @{
    "FeatureName" = "Web-Server"
    "IsPresent" = $False
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ParametersExample" -Parameters $Parameters
```

<span data-ttu-id="63355-144">PSCredentials átadása paraméterként kapcsolatos információkért lásd: <a href="#credential-assets"> **hitelesítő eszközök** </a> alatt.</span><span class="sxs-lookup"><span data-stu-id="63355-144">For information about passing PSCredentials as parameters, see <a href="#credential-assets">**Credential Assets**</a> below.</span></span>

## <a name="configurationdata"></a><span data-ttu-id="63355-145">ConfigurationData</span><span class="sxs-lookup"><span data-stu-id="63355-145">ConfigurationData</span></span>
<span data-ttu-id="63355-146">**ConfigurationData** lehetővé teszi, hogy a környezet konkrét beállításra a PowerShell DSC szerkezeti konfigurációja különítheti el.</span><span class="sxs-lookup"><span data-stu-id="63355-146">**ConfigurationData** allows you to separate structural configuration from any environment specific configuration while using PowerShell DSC.</span></span> <span data-ttu-id="63355-147">Lásd: [mappától "Mi" a "Where" a PowerShell DSC](http://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) kapcsolatos további **ConfigurationData**.</span><span class="sxs-lookup"><span data-stu-id="63355-147">See [Separating "What" from "Where" in PowerShell DSC](http://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) to learn more about **ConfigurationData**.</span></span>

> [!NOTE]
> <span data-ttu-id="63355-148">Használhat **ConfigurationData** Azure Automation DSC Azure PowerShell használatával, de nem az Azure-portálon fordítása során.</span><span class="sxs-lookup"><span data-stu-id="63355-148">You can use **ConfigurationData** when compiling in Azure Automation DSC using Azure PowerShell, but not in the Azure portal.</span></span>

<span data-ttu-id="63355-149">Használja az alábbi példa a DSC-konfiguráció **ConfigurationData** keresztül a **$ConfigurationData** és **$AllNodes** kulcsszavakat.</span><span class="sxs-lookup"><span data-stu-id="63355-149">The following example DSC configuration uses **ConfigurationData** via the **$ConfigurationData** and **$AllNodes** keywords.</span></span> <span data-ttu-id="63355-150">Biztosítani kell a [ **xWebAdministration** modul](https://www.powershellgallery.com/packages/xWebAdministration/) ehhez a példához:</span><span class="sxs-lookup"><span data-stu-id="63355-150">You'll also need the [**xWebAdministration** module](https://www.powershellgallery.com/packages/xWebAdministration/) for this example:</span></span>

```powershell
Configuration ConfigurationDataSample
{
    Import-DscResource -ModuleName xWebAdministration -Name MSFT_xWebsite

    Write-Verbose $ConfigurationData.NonNodeData.SomeMessage

    Node $AllNodes.Where{$_.Role -eq "WebServer"}.NodeName
    {
        xWebsite Site
        {
            Name = $Node.SiteName
            PhysicalPath = $Node.SiteContents
            Ensure   = "Present"
        }
    }
}
```

<span data-ttu-id="63355-151">A DSC-konfiguráció fent PowerShell állíthat össze.</span><span class="sxs-lookup"><span data-stu-id="63355-151">You can compile the DSC configuration above with PowerShell.</span></span> <span data-ttu-id="63355-152">Az alábbi PowerShell ad hozzá két csomópont-konfigurációt az Azure Automation DSC-lekéréses kiszolgáló: **ConfigurationDataSample.MyVM1** és **ConfigurationDataSample.MyVM3**:</span><span class="sxs-lookup"><span data-stu-id="63355-152">The below PowerShell adds two node configurations to the Azure Automation DSC Pull Server: **ConfigurationDataSample.MyVM1** and **ConfigurationDataSample.MyVM3**:</span></span>

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = "MyVM1"
            Role = "WebServer"
        },
        @{
            NodeName = "MyVM2"
            Role = "SQLServer"
        },
        @{
            NodeName = "MyVM3"
            Role = "WebServer"
        }
    )

    NonNodeData = @{
        SomeMessage = "I love Azure Automation DSC!"
    }
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ConfigurationDataSample" -ConfigurationData $ConfigData
```

## <a name="assets"></a><span data-ttu-id="63355-153">Objektumok</span><span class="sxs-lookup"><span data-stu-id="63355-153">Assets</span></span>

<span data-ttu-id="63355-154">Eszköz hivatkozások megegyeznek az Azure Automation DSC-konfiguráció és a runbookok.</span><span class="sxs-lookup"><span data-stu-id="63355-154">Asset references are the same in Azure Automation DSC configurations and runbooks.</span></span> <span data-ttu-id="63355-155">További információt a következő témakörben talál:</span><span class="sxs-lookup"><span data-stu-id="63355-155">See the following for more information:</span></span>

* [<span data-ttu-id="63355-156">Tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="63355-156">Certificates</span></span>](automation-certificates.md)
* [<span data-ttu-id="63355-157">Kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="63355-157">Connections</span></span>](automation-connections.md)
* [<span data-ttu-id="63355-158">Hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="63355-158">Credentials</span></span>](automation-credentials.md)
* [<span data-ttu-id="63355-159">Változók</span><span class="sxs-lookup"><span data-stu-id="63355-159">Variables</span></span>](automation-variables.md)

### <a name="credential-assets"></a><span data-ttu-id="63355-160">Hitelesítő eszközök</span><span class="sxs-lookup"><span data-stu-id="63355-160">Credential Assets</span></span>

<span data-ttu-id="63355-161">Azure Automation DSC-konfigurációja is hivatkozni lehessen hitelesítő eszközök használata során **Get-AzureRmAutomationCredential**, hitelesítő eszközök is adhatók át a keresztül paraméterek, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="63355-161">While DSC configurations in Azure Automation can reference credential assets using **Get-AzureRmAutomationCredential**, credential assets can also be passed in via parameters, if desired.</span></span> <span data-ttu-id="63355-162">Ha egy konfigurációs paramétert **PSCredential** kell egy Azure Automation szolgáltatásbeli hitelesítőadat-eszköz karakterlánc nevét átadni adott paraméter értéke, nem pedig egy PSCredential objektumot, majd írja be.</span><span class="sxs-lookup"><span data-stu-id="63355-162">If a configuration takes a parameter of **PSCredential** type, then you need to pass the string name of an Azure Automation credential asset as that parameter’s value, rather than a PSCredential object.</span></span> <span data-ttu-id="63355-163">A háttérben az Azure Automation szolgáltatásbeli hitelesítőadat-eszköz ezen a néven lesz lekérje és kapott a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="63355-163">Behind the scenes, the Azure Automation credential asset with that name will be retrieved and passed to the configuration.</span></span>

<span data-ttu-id="63355-164">Hitelesítő adatok megőrzésével csomópont-konfigurációt (MOF konfigurációs dokumentumok) biztonságos kell titkosítani a hitelesítő adatokat, a csomópont konfigurációs MOF-fájlban.</span><span class="sxs-lookup"><span data-stu-id="63355-164">Keeping credentials secure in node configurations (MOF configuration documents) requires encrypting the credentials in the node configuration MOF file.</span></span> <span data-ttu-id="63355-165">Azure Automation szolgáltatásbeli ebben a lépésben egy tovább tart, és titkosítja a teljes MOF-fájlt.</span><span class="sxs-lookup"><span data-stu-id="63355-165">Azure Automation takes this one step further and encrypts the entire MOF file.</span></span> <span data-ttu-id="63355-166">Azonban jelenleg pedig kell utasítani fogja a PowerShell DSC nem probléma, a csomópont konfigurációs MOF létrehozása során gyártandó egyszerű szöveges hitelesítő adatokat, mert a PowerShell DSC nem ismert, hogy Azure Automation fog kell titkosítása a teljes MOF-fájlt a generációját után keresztül egy fordítási feladat.</span><span class="sxs-lookup"><span data-stu-id="63355-166">However, currently you must tell PowerShell DSC it is okay for credentials to be outputted in plain text during node configuration MOF generation, because PowerShell DSC doesn’t know that Azure Automation will be encrypting the entire MOF file after its generation via a compilation job.</span></span>

<span data-ttu-id="63355-167">Megadható, hogy a PowerShell DSC, amelyek a hitelesítő adatokat egyszerű szöveges a létrehozott csomópont-konfiguráció MOF-ot a gyártandó kapcsolatát használatával [ **ConfigurationData**](#configurationdata).</span><span class="sxs-lookup"><span data-stu-id="63355-167">You can tell PowerShell DSC that it is okay for credentials to be outputted in plain text in the generated node configuration MOFs using [**ConfigurationData**](#configurationdata).</span></span> <span data-ttu-id="63355-168">Át kell `PSDscAllowPlainTextPassword = $true` keresztül **ConfigurationData** minden csomópont blokk neve, amely megjelenik a DSC-konfiguráció és a hitelesítő adatokat használ.</span><span class="sxs-lookup"><span data-stu-id="63355-168">You should pass `PSDscAllowPlainTextPassword = $true` via **ConfigurationData** for each node block’s name that appears in the DSC configuration and uses credentials.</span></span>

<span data-ttu-id="63355-169">A következő példa bemutatja a DSC-konfiguráció által használt Automation szolgáltatásbeli hitelesítőadat-eszköz.</span><span class="sxs-lookup"><span data-stu-id="63355-169">The following example shows a DSC configuration that uses an Automation credential asset.</span></span>

```powershell
Configuration CredentialSample
{
    $Cred = Get-AzureRmAutomationCredential -ResourceGroupName "ResourceGroup01" -AutomationAccountName "AutomationAcct" -Name "SomeCredentialAsset"

    Node $AllNodes.NodeName
    {
        File ExampleFile
        {
            SourcePath = "\\Server\share\path\file.ext"
            DestinationPath = "C:\destinationPath"
            Credential = $Cred
        }
    }
}
```

<span data-ttu-id="63355-170">A DSC-konfiguráció fent PowerShell állíthat össze.</span><span class="sxs-lookup"><span data-stu-id="63355-170">You can compile the DSC configuration above with PowerShell.</span></span> <span data-ttu-id="63355-171">Az alábbi PowerShell ad hozzá két csomópont-konfigurációt az Azure Automation DSC-lekéréses kiszolgáló: **CredentialSample.MyVM1** és **CredentialSample.MyVM2**.</span><span class="sxs-lookup"><span data-stu-id="63355-171">The below PowerShell adds two node configurations to the Azure Automation DSC Pull Server:  **CredentialSample.MyVM1** and **CredentialSample.MyVM2**.</span></span>

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = "*"
            PSDscAllowPlainTextPassword = $True
        },
        @{
            NodeName = "MyVM1"
        },
        @{
            NodeName = "MyVM2"
        }
    )
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "CredentialSample" -ConfigurationData $ConfigData
```

## <a name="importing-node-configurations"></a><span data-ttu-id="63355-172">Csomópont-konfigurációk importálása</span><span class="sxs-lookup"><span data-stu-id="63355-172">Importing node configurations</span></span>

<span data-ttu-id="63355-173">Csomópont configuratons (MOF-ot), amely rendelkezik Azure-on kívüli fordított is importálhat.</span><span class="sxs-lookup"><span data-stu-id="63355-173">You can also import node configuratons (MOFs) that you have compiled outside of Azure.</span></span> <span data-ttu-id="63355-174">Egy ennek előnye, hogy a csomópont confiturations írhatók alá.</span><span class="sxs-lookup"><span data-stu-id="63355-174">One advantage of this is that node confiturations can be signed.</span></span>
<span data-ttu-id="63355-175">Egy aláírt csomópont-konfiguráció ellenőrzése helyileg felügyelt csomóponton a DSC-ügynök, győződjön meg arról, hogy a konfiguráció alkalmazták a csomópont egy hitelesített forrásból származik-e.</span><span class="sxs-lookup"><span data-stu-id="63355-175">A signed node configuration is verified locally on a managed node by the DSC agent, ensuring that the configuration being applied to the node comes from an authorized source.</span></span>

> [!NOTE]
> <span data-ttu-id="63355-176">Használja az importálást aláírt beállításokat az Azure Automation-fiók rendszerbe, de Azure Automation jelenleg nem támogatja a fordítása aláírt konfigurációkat.</span><span class="sxs-lookup"><span data-stu-id="63355-176">You can use import signed configurations into your Azure Automation account, but Azure Automation does not currently support compiling signed configurations.</span></span>

> [!NOTE]
> <span data-ttu-id="63355-177">A csomópont konfigurációs fájl nem engedélyezi az Azure Automation importáláshoz 1 MB-nál nagyobb lehet.</span><span class="sxs-lookup"><span data-stu-id="63355-177">A node configuration file must be no larger than 1 MB to allow it to be imported into Azure Automation.</span></span>

<span data-ttu-id="63355-178">Megismerheti https://msdn.microsoft.com/en-us/powershell/wmf/5.1/dsc-improvements#how-to-sign-configuration-and-module csomópont konfigurációját aláírásához.</span><span class="sxs-lookup"><span data-stu-id="63355-178">You can learn how to sign node configurations at https://msdn.microsoft.com/en-us/powershell/wmf/5.1/dsc-improvements#how-to-sign-configuration-and-module.</span></span>

### <a name="importing-a-node-configuration-in-the-azure-portal"></a><span data-ttu-id="63355-179">Az Azure-portálon a csomópont-konfiguráció importálása</span><span class="sxs-lookup"><span data-stu-id="63355-179">Importing a node configuration in the Azure portal</span></span>

1. <span data-ttu-id="63355-180">Az Automation-fiók kattintson **DSC-csomópontkonfigurációk**.</span><span class="sxs-lookup"><span data-stu-id="63355-180">From your Automation account, click **DSC node configurations**.</span></span>

    ![A DSC-csomópontkonfigurációk](./media/automation-dsc-compile/node-config.png)
2. <span data-ttu-id="63355-182">Az a **DSC-csomópontkonfigurációk** panelen kattintson a **hozzáadása egy NodeConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="63355-182">In the **DSC node configurations** blade, click **Add a NodeConfiguration**.</span></span>
3. <span data-ttu-id="63355-183">Az a **importálási** panelen kattintson a Tovább gombra a mappa ikonra a **csomópont konfigurációs fájl** szövegmező a csomópont konfigurációs fájl (MOF) a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="63355-183">In the **Import** blade, click the folder icon next to the **Node Configuration File** textbox to browse for a node configuration file (MOF) on your local computer.</span></span>

    ![Helyi fájl kiválasztása](./media/automation-dsc-compile/import-browse.png)
4. <span data-ttu-id="63355-185">Írjon be egy nevet a **Konfigurációnévvel** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="63355-185">Enter a name in the **Configuration Name** textbox.</span></span> <span data-ttu-id="63355-186">Ezt a nevet meg kell egyeznie a nevét, amelyből a csomópont-konfiguráció lett lefordítva, a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="63355-186">This name must match the name of the configuration from which the node configuration was compiled.</span></span>
5. <span data-ttu-id="63355-187">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="63355-187">Click **OK**.</span></span>

### <a name="importing-a-node-configuration-with-powershell"></a><span data-ttu-id="63355-188">A PowerShell-lel egy csomópont-konfiguráció importálása</span><span class="sxs-lookup"><span data-stu-id="63355-188">Importing a node configuration with PowerShell</span></span>

<span data-ttu-id="63355-189">Használhatja a [Import-AzureRmAutomationDscNodeConfiguration](/powershell/module/azurerm.automation/import-azurermautomationdscnodeconfiguration) parancsmagot, hogy a csomópont-konfiguráció importálása az automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="63355-189">You can use the [Import-AzureRmAutomationDscNodeConfiguration](/powershell/module/azurerm.automation/import-azurermautomationdscnodeconfiguration) cmdlet to import a node configuration into your automation account.</span></span>

```powershell
Import-AzureRmAutomationDscNodeConfiguration -AutomationAccountName "MyAutomationAccount" -ResourceGroupName "MyResourceGroup" -ConfigurationName "MyNodeConfiguration" -Path "C:\MyConfigurations\TestVM1.mof"
```



