---
title: "Azure Automation DSC-aaaCompiling konfigurációja |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan Azure Automation konfigurációi toocompile kívánt állapot konfigurációs szolgáltatása (DSC)."
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
ms.openlocfilehash: b195311318a2d7431c4d6b29f4b9a5f3a0a0a9a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="compiling-configurations-in-azure-automation-dsc"></a><span data-ttu-id="ade8f-103">Azure Automation DSC-konfigurációja fordítása</span><span class="sxs-lookup"><span data-stu-id="ade8f-103">Compiling configurations in Azure Automation DSC</span></span>

<span data-ttu-id="ade8f-104">Az Azure Automation szolgáltatásban kétféle módon kívánt állapot konfigurációs szolgáltatása (DSC) konfigurációk állíthat össze: hello Azure-portálon, és a Windows PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="ade8f-104">You can compile Desired State Configuration (DSC) configurations in two ways with Azure Automation: in hello Azure portal, and with Windows PowerShell.</span></span> <span data-ttu-id="ade8f-105">hello következő táblázat segítségével meghatározhatja, hogy mikor toouse módszer egyes hello jellemzők alapján:</span><span class="sxs-lookup"><span data-stu-id="ade8f-105">hello following table will help you determine when toouse which method based on hello characteristics of each:</span></span>

### <a name="azure-portal"></a><span data-ttu-id="ade8f-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ade8f-106">Azure portal</span></span>

* <span data-ttu-id="ade8f-107">Legegyszerűbb módja az interaktív felhasználói felülettel</span><span class="sxs-lookup"><span data-stu-id="ade8f-107">Simplest method with interactive user interface</span></span>
* <span data-ttu-id="ade8f-108">Űrlap tooprovide egyszerű paraméterértékek</span><span class="sxs-lookup"><span data-stu-id="ade8f-108">Form tooprovide simple parameter values</span></span>
* <span data-ttu-id="ade8f-109">Könnyen nyomon követheti a feladat állapota</span><span class="sxs-lookup"><span data-stu-id="ade8f-109">Easily track job state</span></span>
* <span data-ttu-id="ade8f-110">Az Azure bejelentkezési hitelesített hozzáférés</span><span class="sxs-lookup"><span data-stu-id="ade8f-110">Access authenticated with Azure logon</span></span>

### <a name="windows-powershell"></a><span data-ttu-id="ade8f-111">Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="ade8f-111">Windows PowerShell</span></span>

* <span data-ttu-id="ade8f-112">A Windows PowerShell-parancsmagokkal a parancssorból hívható</span><span class="sxs-lookup"><span data-stu-id="ade8f-112">Call from command line with Windows PowerShell cmdlets</span></span>
* <span data-ttu-id="ade8f-113">Automatizált megoldást több lépést tartalmazhat</span><span class="sxs-lookup"><span data-stu-id="ade8f-113">Can be included in automated solution with multiple steps</span></span>
* <span data-ttu-id="ade8f-114">Adja meg az egyszerű és összetett paraméterértékek</span><span class="sxs-lookup"><span data-stu-id="ade8f-114">Provide simple and complex parameter values</span></span>
* <span data-ttu-id="ade8f-115">Feladat-állapotok nyomon követésére</span><span class="sxs-lookup"><span data-stu-id="ade8f-115">Track job state</span></span>
* <span data-ttu-id="ade8f-116">Ügyfél szükséges toosupport PowerShell-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="ade8f-116">Client required toosupport PowerShell cmdlets</span></span>
* <span data-ttu-id="ade8f-117">Pass ConfigurationData</span><span class="sxs-lookup"><span data-stu-id="ade8f-117">Pass ConfigurationData</span></span>
* <span data-ttu-id="ade8f-118">A hitelesítő adatokat használó konfigurációk összeállítása</span><span class="sxs-lookup"><span data-stu-id="ade8f-118">Compile configurations that use credentials</span></span>

<span data-ttu-id="ade8f-119">Miután eldöntötte, hogy egy fordítási metódusra, követésével hello vonatkozó eljárások toostart fordítás alatt.</span><span class="sxs-lookup"><span data-stu-id="ade8f-119">Once you have decided on a compilation method, you can follow hello respective procedures below toostart compiling.</span></span>

## <a name="compiling-a-dsc-configuration-with-hello-azure-portal"></a><span data-ttu-id="ade8f-120">A DSC-konfiguráció hello Azure-portálon fordítása</span><span class="sxs-lookup"><span data-stu-id="ade8f-120">Compiling a DSC Configuration with hello Azure portal</span></span>

1. <span data-ttu-id="ade8f-121">Az Automation-fiók kattintson **a DSC-konfigurációk**.</span><span class="sxs-lookup"><span data-stu-id="ade8f-121">From your Automation account, click **DSC Configurations**.</span></span>
2. <span data-ttu-id="ade8f-122">Egy konfigurációs tooopen kattintson a panel.</span><span class="sxs-lookup"><span data-stu-id="ade8f-122">Click a configuration tooopen its blade.</span></span>
3. <span data-ttu-id="ade8f-123">Kattintson a **fordítási**.</span><span class="sxs-lookup"><span data-stu-id="ade8f-123">Click **Compile**.</span></span>
4. <span data-ttu-id="ade8f-124">Ha hello konfigurációs nincs paraméterekkel rendelkezik, akkor fogja rákérdezéses tooconfirm toocompile kíván-e azt.</span><span class="sxs-lookup"><span data-stu-id="ade8f-124">If hello configuration has no parameters, you will be prompted tooconfirm whether you want toocompile it.</span></span> <span data-ttu-id="ade8f-125">Ha hello konfigurációs paraméterek, hello **fordítási konfigurációs** panel nyílik meg, megadhatja a paraméterértékek.</span><span class="sxs-lookup"><span data-stu-id="ade8f-125">If hello configuration has parameters, hello **Compile Configuration** blade will open so you can provide parameter values.</span></span> <span data-ttu-id="ade8f-126">Lásd: hello [ **alapvető paraméterek** ](#basic-parameters) alatt további tájékoztatást talál a Paraméterek szakaszban.</span><span class="sxs-lookup"><span data-stu-id="ade8f-126">See hello [**Basic Parameters**](#basic-parameters) section below for further details on parameters.</span></span>
5. <span data-ttu-id="ade8f-127">Hello **fordítási feladat** panel már meg van nyitva, hogy hello fordítási feladat állapotát nyomon követheti, és hello csomópont-konfigurációt (MOF konfigurációs dokumentumok) okozta toobe hello Azure Automation DSC lekéréses kiszolgáló helyezve.</span><span class="sxs-lookup"><span data-stu-id="ade8f-127">hello **Compilation Job** blade is opened so that you can track hello compilation job's status, and hello node configurations (MOF configuration documents) it caused toobe placed on hello Azure Automation DSC Pull Server.</span></span>

## <a name="compiling-a-dsc-configuration-with-windows-powershell"></a><span data-ttu-id="ade8f-128">A Windows PowerShell DSC-konfiguráció fordítása</span><span class="sxs-lookup"><span data-stu-id="ade8f-128">Compiling a DSC Configuration with Windows PowerShell</span></span>

<span data-ttu-id="ade8f-129">Használhat [ `Start-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/start-azurermautomationdsccompilationjob) toostart fordítása a Windows PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="ade8f-129">You can use [`Start-AzureRmAutomationDscCompilationJob`](/powershell/module/azurerm.automation/start-azurermautomationdsccompilationjob) toostart compiling with Windows PowerShell.</span></span> <span data-ttu-id="ade8f-130">a következő példakód hello kezdődik nevű DSC-konfiguráció összeállításának **SampleConfig**.</span><span class="sxs-lookup"><span data-stu-id="ade8f-130">hello following sample code starts compilation of a DSC configuration called **SampleConfig**.</span></span>

```powershell
Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"
```

<span data-ttu-id="ade8f-131">`Start-AzureRmAutomationDscCompilationJob`értéket ad vissza a fordítási feladat-objektum használható tootrack annak állapotát.</span><span class="sxs-lookup"><span data-stu-id="ade8f-131">`Start-AzureRmAutomationDscCompilationJob` returns a compilation job object that you can use tootrack its status.</span></span> <span data-ttu-id="ade8f-132">Ezután használhatja a fordítási feladat objektum [ `Get-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjob) toodetermine hello állapotának hello fordítási feladat, és [ `Get-AzureRmAutomationDscCompilationJobOutput` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjoboutput) tooview az adatfolyamok (kimenet).</span><span class="sxs-lookup"><span data-stu-id="ade8f-132">You can then use this compilation job object with [`Get-AzureRmAutomationDscCompilationJob`](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjob) toodetermine hello status of hello compilation job, and [`Get-AzureRmAutomationDscCompilationJobOutput`](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjoboutput) tooview its streams (output).</span></span> <span data-ttu-id="ade8f-133">a következő példakód hello kezdődik hello összeállításának **SampleConfig** konfigurációs, megvárja, amíg befejeződött, és megjeleníti az adatfolyamokat.</span><span class="sxs-lookup"><span data-stu-id="ade8f-133">hello following sample code starts compilation of hello **SampleConfig** configuration, waits until it has completed, and then displays its streams.</span></span>

```powershell
$CompilationJob = Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"

while($CompilationJob.EndTime –eq $null -and $CompilationJob.Exception –eq $null)
{
    $CompilationJob = $CompilationJob | Get-AzureRmAutomationDscCompilationJob
    Start-Sleep -Seconds 3
}

$CompilationJob | Get-AzureRmAutomationDscCompilationJobOutput –Stream Any
```

## <a name="basic-parameters"></a><span data-ttu-id="ade8f-134">Alapvető paraméterek</span><span class="sxs-lookup"><span data-stu-id="ade8f-134">Basic Parameters</span></span>
<span data-ttu-id="ade8f-135">A DSC-konfigurációkkal, például a különböző és tulajdonságait, works paraméterdeklarációhoz hello azonos hasonlóan az Azure Automation-forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="ade8f-135">Parameter declaration in DSC configurations, including parameter types and properties, works hello same as in Azure Automation runbooks.</span></span> <span data-ttu-id="ade8f-136">Lásd: [runbook elindítása az Azure Automationben](automation-starting-a-runbook.md) runbook paraméterekkel kapcsolatos további toolearn.</span><span class="sxs-lookup"><span data-stu-id="ade8f-136">See [Starting a runbook in Azure Automation](automation-starting-a-runbook.md) toolearn more about runbook parameters.</span></span>

<span data-ttu-id="ade8f-137">hello következő példában két paramétereket **FeatureName** és **IsPresent**, toodetermine hello értékek hello tulajdonságokat **ParametersExample.sample** csomópont konfiguráció, a fordítás során jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="ade8f-137">hello following example uses two parameters called **FeatureName** and **IsPresent**, toodetermine hello values of properties in hello **ParametersExample.sample** node configuration, generated during compilation.</span></span>

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

<span data-ttu-id="ade8f-138">A DSC-konfigurációk hello Azure Automation DSC-portálon, vagy az Azure PowerShell alapvető paramétert használó állíthat össze:</span><span class="sxs-lookup"><span data-stu-id="ade8f-138">You can compile DSC Configurations that use basic parameters in hello Azure Automation DSC portal, or with Azure PowerShell:</span></span>

### <a name="portal"></a><span data-ttu-id="ade8f-139">Portál</span><span class="sxs-lookup"><span data-stu-id="ade8f-139">Portal</span></span>

<span data-ttu-id="ade8f-140">Hello portálon adhatja meg a paraméterértékek kattintás után **fordítási**.</span><span class="sxs-lookup"><span data-stu-id="ade8f-140">In hello portal, you can enter parameter values after clicking **Compile**.</span></span>

![helyettesítő szöveg](./media/automation-dsc-compile/DSC_compiling_1.png)

### <a name="powershell"></a><span data-ttu-id="ade8f-142">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ade8f-142">PowerShell</span></span>

<span data-ttu-id="ade8f-143">PowerShell-paraméterek a szükséges egy [hashtable](http://technet.microsoft.com/library/hh847780.aspx) ahol hello kulcs hello paraméter neve megegyezik, és hello érték hello paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="ade8f-143">PowerShell requires parameters in a [hashtable](http://technet.microsoft.com/library/hh847780.aspx) where hello key matches hello parameter name, and hello value equals hello parameter value.</span></span>

```powershell
$Parameters = @{
    "FeatureName" = "Web-Server"
    "IsPresent" = $False
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ParametersExample" -Parameters $Parameters
```

<span data-ttu-id="ade8f-144">PSCredentials átadása paraméterként kapcsolatos információkért lásd: <a href="#credential-assets"> **hitelesítő eszközök** </a> alatt.</span><span class="sxs-lookup"><span data-stu-id="ade8f-144">For information about passing PSCredentials as parameters, see <a href="#credential-assets">**Credential Assets**</a> below.</span></span>

## <a name="configurationdata"></a><span data-ttu-id="ade8f-145">ConfigurationData</span><span class="sxs-lookup"><span data-stu-id="ade8f-145">ConfigurationData</span></span>
<span data-ttu-id="ade8f-146">**ConfigurationData** teszi lehetővé a környezet konkrét beállításra a PowerShell DSC tooseparate strukturális konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="ade8f-146">**ConfigurationData** allows you tooseparate structural configuration from any environment specific configuration while using PowerShell DSC.</span></span> <span data-ttu-id="ade8f-147">Lásd: [mappától "Mi" a "Where" a PowerShell DSC](http://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) kapcsolatos további toolearn **ConfigurationData**.</span><span class="sxs-lookup"><span data-stu-id="ade8f-147">See [Separating "What" from "Where" in PowerShell DSC](http://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) toolearn more about **ConfigurationData**.</span></span>

> [!NOTE]
> <span data-ttu-id="ade8f-148">Használhat **ConfigurationData** Azure Automation DSC Azure PowerShell használatával, de nem hello Azure-portálon fordítása során.</span><span class="sxs-lookup"><span data-stu-id="ade8f-148">You can use **ConfigurationData** when compiling in Azure Automation DSC using Azure PowerShell, but not in hello Azure portal.</span></span>

<span data-ttu-id="ade8f-149">hello alábbi példa a DSC-konfiguráció használ **ConfigurationData** keresztül hello **$ConfigurationData** és **$AllNodes** kulcsszavakat.</span><span class="sxs-lookup"><span data-stu-id="ade8f-149">hello following example DSC configuration uses **ConfigurationData** via hello **$ConfigurationData** and **$AllNodes** keywords.</span></span> <span data-ttu-id="ade8f-150">Meg kell hello [ **xWebAdministration** modul](https://www.powershellgallery.com/packages/xWebAdministration/) ehhez a példához:</span><span class="sxs-lookup"><span data-stu-id="ade8f-150">You'll also need hello [**xWebAdministration** module](https://www.powershellgallery.com/packages/xWebAdministration/) for this example:</span></span>

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

<span data-ttu-id="ade8f-151">Hello DSC-konfiguráció fent PowerShell állíthat össze.</span><span class="sxs-lookup"><span data-stu-id="ade8f-151">You can compile hello DSC configuration above with PowerShell.</span></span> <span data-ttu-id="ade8f-152">PowerShell alatt hello két csomópont konfigurációk toohello Azure Automation DSC lekéréses kiszolgáló hozzáadása: **ConfigurationDataSample.MyVM1** és **ConfigurationDataSample.MyVM3**:</span><span class="sxs-lookup"><span data-stu-id="ade8f-152">hello below PowerShell adds two node configurations toohello Azure Automation DSC Pull Server: **ConfigurationDataSample.MyVM1** and **ConfigurationDataSample.MyVM3**:</span></span>

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

## <a name="assets"></a><span data-ttu-id="ade8f-153">Objektumok</span><span class="sxs-lookup"><span data-stu-id="ade8f-153">Assets</span></span>

<span data-ttu-id="ade8f-154">Eszköz hivatkozások vannak hello ugyanazt az Azure Automation DSC-konfiguráció és a runbookok.</span><span class="sxs-lookup"><span data-stu-id="ade8f-154">Asset references are hello same in Azure Automation DSC configurations and runbooks.</span></span> <span data-ttu-id="ade8f-155">Hello alábbiakat a további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="ade8f-155">See hello following for more information:</span></span>

* [<span data-ttu-id="ade8f-156">Tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="ade8f-156">Certificates</span></span>](automation-certificates.md)
* [<span data-ttu-id="ade8f-157">Kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="ade8f-157">Connections</span></span>](automation-connections.md)
* [<span data-ttu-id="ade8f-158">Hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="ade8f-158">Credentials</span></span>](automation-credentials.md)
* [<span data-ttu-id="ade8f-159">Változók</span><span class="sxs-lookup"><span data-stu-id="ade8f-159">Variables</span></span>](automation-variables.md)

### <a name="credential-assets"></a><span data-ttu-id="ade8f-160">Hitelesítő eszközök</span><span class="sxs-lookup"><span data-stu-id="ade8f-160">Credential Assets</span></span>

<span data-ttu-id="ade8f-161">Azure Automation DSC-konfigurációja is hivatkozni lehessen hitelesítő eszközök használata során **Get-AzureRmAutomationCredential**, hitelesítő eszközök is adhatók át a keresztül paraméterek, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="ade8f-161">While DSC configurations in Azure Automation can reference credential assets using **Get-AzureRmAutomationCredential**, credential assets can also be passed in via parameters, if desired.</span></span> <span data-ttu-id="ade8f-162">Ha egy konfigurációs paramétert **PSCredential** toopass hello karakterlánc nevét egy Azure Automation szolgáltatásbeli hitelesítőadat-eszköz van szüksége adott paraméter értéke, nem pedig egy PSCredential objektumot, majd írja be.</span><span class="sxs-lookup"><span data-stu-id="ade8f-162">If a configuration takes a parameter of **PSCredential** type, then you need toopass hello string name of an Azure Automation credential asset as that parameter’s value, rather than a PSCredential object.</span></span> <span data-ttu-id="ade8f-163">Hello háttérben hello Azure Automation szolgáltatásbeli hitelesítőadat-eszköz ezen a néven lesz lekérje és toohello konfigurációs átadott.</span><span class="sxs-lookup"><span data-stu-id="ade8f-163">Behind hello scenes, hello Azure Automation credential asset with that name will be retrieved and passed toohello configuration.</span></span>

<span data-ttu-id="ade8f-164">Hitelesítő adatok megőrzésével csomópont-konfigurációt (MOF konfigurációs dokumentumok) biztonságos szükséges hello csomópont konfigurációs MOF-fájlnak a hello hitelesítő adatok titkosításához.</span><span class="sxs-lookup"><span data-stu-id="ade8f-164">Keeping credentials secure in node configurations (MOF configuration documents) requires encrypting hello credentials in hello node configuration MOF file.</span></span> <span data-ttu-id="ade8f-165">Azure Automation szolgáltatásbeli ebben a lépésben egy tovább tart, és titkosítja a hello teljes MOF-fájlt.</span><span class="sxs-lookup"><span data-stu-id="ade8f-165">Azure Automation takes this one step further and encrypts hello entire MOF file.</span></span> <span data-ttu-id="ade8f-166">Azonban jelenleg pedig kell utasítani fogja a PowerShell DSC nem hitelesítő adatok toobe outputted egyszerű szöveges csomópont konfigurációs MOF létrehozásakor, a probléma, mert a PowerShell DSC nem ismert, hogy Azure Automation fog kell Titkosított hello teljes MOF-fájlt után a egy fordítási feladat keresztül létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ade8f-166">However, currently you must tell PowerShell DSC it is okay for credentials toobe outputted in plain text during node configuration MOF generation, because PowerShell DSC doesn’t know that Azure Automation will be encrypting hello entire MOF file after its generation via a compilation job.</span></span>

<span data-ttu-id="ade8f-167">Úgy írhatja elő PowerShell DSC róla egy értesítést a outputted hello jön létre a csomópont-konfiguráció MOF-ot az egyszerű szöveges hitelesítő adatokat toobe használatával [ **ConfigurationData**](#configurationdata).</span><span class="sxs-lookup"><span data-stu-id="ade8f-167">You can tell PowerShell DSC that it is okay for credentials toobe outputted in plain text in hello generated node configuration MOFs using [**ConfigurationData**](#configurationdata).</span></span> <span data-ttu-id="ade8f-168">Át kell `PSDscAllowPlainTextPassword = $true` keresztül **ConfigurationData** minden csomópont blokk neve, amely megjelenik az hello DSC-konfiguráció és a hitelesítő adatokat használ.</span><span class="sxs-lookup"><span data-stu-id="ade8f-168">You should pass `PSDscAllowPlainTextPassword = $true` via **ConfigurationData** for each node block’s name that appears in hello DSC configuration and uses credentials.</span></span>

<span data-ttu-id="ade8f-169">hello következő példa bemutatja a DSC-konfiguráció által használt Automation szolgáltatásbeli hitelesítőadat-eszköz.</span><span class="sxs-lookup"><span data-stu-id="ade8f-169">hello following example shows a DSC configuration that uses an Automation credential asset.</span></span>

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

<span data-ttu-id="ade8f-170">Hello DSC-konfiguráció fent PowerShell állíthat össze.</span><span class="sxs-lookup"><span data-stu-id="ade8f-170">You can compile hello DSC configuration above with PowerShell.</span></span> <span data-ttu-id="ade8f-171">PowerShell alatt hello két csomópont konfigurációk toohello Azure Automation DSC lekéréses kiszolgáló hozzáadása: **CredentialSample.MyVM1** és **CredentialSample.MyVM2**.</span><span class="sxs-lookup"><span data-stu-id="ade8f-171">hello below PowerShell adds two node configurations toohello Azure Automation DSC Pull Server:  **CredentialSample.MyVM1** and **CredentialSample.MyVM2**.</span></span>

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

## <a name="importing-node-configurations"></a><span data-ttu-id="ade8f-172">Csomópont-konfigurációk importálása</span><span class="sxs-lookup"><span data-stu-id="ade8f-172">Importing node configurations</span></span>

<span data-ttu-id="ade8f-173">Csomópont configuratons (MOF-ot), amely rendelkezik Azure-on kívüli fordított is importálhat.</span><span class="sxs-lookup"><span data-stu-id="ade8f-173">You can also import node configuratons (MOFs) that you have compiled outside of Azure.</span></span> <span data-ttu-id="ade8f-174">Egy ennek előnye, hogy a csomópont confiturations írhatók alá.</span><span class="sxs-lookup"><span data-stu-id="ade8f-174">One advantage of this is that node confiturations can be signed.</span></span>
<span data-ttu-id="ade8f-175">Egy aláírt csomópont-konfiguráció ellenőrzése helyileg felügyelt csomóponton hello DSC-ügynök, győződjön meg arról, hogy hello konfigurálása alkalmazott toohello csomópont alatt egy hitelesített forrásból származnak.</span><span class="sxs-lookup"><span data-stu-id="ade8f-175">A signed node configuration is verified locally on a managed node by hello DSC agent, ensuring that hello configuration being applied toohello node comes from an authorized source.</span></span>

> [!NOTE]
> <span data-ttu-id="ade8f-176">Használja az importálást aláírt beállításokat az Azure Automation-fiók rendszerbe, de Azure Automation jelenleg nem támogatja a fordítása aláírt konfigurációkat.</span><span class="sxs-lookup"><span data-stu-id="ade8f-176">You can use import signed configurations into your Azure Automation account, but Azure Automation does not currently support compiling signed configurations.</span></span>

> [!NOTE]
> <span data-ttu-id="ade8f-177">Lehet, hogy a csomópont konfigurációs fájl nem nagyobb, mint 1 MB tooallow az Azure Automation importált toobe.</span><span class="sxs-lookup"><span data-stu-id="ade8f-177">A node configuration file must be no larger than 1 MB tooallow it toobe imported into Azure Automation.</span></span>

<span data-ttu-id="ade8f-178">Azt is megtudhatja, hogyan https://msdn.microsoft.com/en-us/powershell/wmf/5.1/dsc-improvements#how-to-sign-configuration-and-module toosign csomópont konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="ade8f-178">You can learn how toosign node configurations at https://msdn.microsoft.com/en-us/powershell/wmf/5.1/dsc-improvements#how-to-sign-configuration-and-module.</span></span>

### <a name="importing-a-node-configuration-in-hello-azure-portal"></a><span data-ttu-id="ade8f-179">A csomópont-konfiguráció hello Azure-portálon a importálása</span><span class="sxs-lookup"><span data-stu-id="ade8f-179">Importing a node configuration in hello Azure portal</span></span>

1. <span data-ttu-id="ade8f-180">Az Automation-fiók kattintson **DSC-csomópontkonfigurációk**.</span><span class="sxs-lookup"><span data-stu-id="ade8f-180">From your Automation account, click **DSC node configurations**.</span></span>

    ![A DSC-csomópontkonfigurációk](./media/automation-dsc-compile/node-config.png)
2. <span data-ttu-id="ade8f-182">A hello **DSC-csomópontkonfigurációk** panelen kattintson a **hozzáadása egy NodeConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="ade8f-182">In hello **DSC node configurations** blade, click **Add a NodeConfiguration**.</span></span>
3. <span data-ttu-id="ade8f-183">A hello **importálása** panelen kattintson hello mappa ikon következő toohello **csomópont konfigurációs fájl** szövegmező toobrowse a csomópont konfigurációs fájl (MOF) a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="ade8f-183">In hello **Import** blade, click hello folder icon next toohello **Node Configuration File** textbox toobrowse for a node configuration file (MOF) on your local computer.</span></span>

    ![Helyi fájl kiválasztása](./media/automation-dsc-compile/import-browse.png)
4. <span data-ttu-id="ade8f-185">Adjon meg egy nevet a hello **Konfigurációnévvel** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="ade8f-185">Enter a name in hello **Configuration Name** textbox.</span></span> <span data-ttu-id="ade8f-186">Ezt a nevet meg kell egyeznie hello csomópont-konfiguráció lefordított hello konfigurációs hello nevét.</span><span class="sxs-lookup"><span data-stu-id="ade8f-186">This name must match hello name of hello configuration from which hello node configuration was compiled.</span></span>
5. <span data-ttu-id="ade8f-187">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="ade8f-187">Click **OK**.</span></span>

### <a name="importing-a-node-configuration-with-powershell"></a><span data-ttu-id="ade8f-188">A PowerShell-lel egy csomópont-konfiguráció importálása</span><span class="sxs-lookup"><span data-stu-id="ade8f-188">Importing a node configuration with PowerShell</span></span>

<span data-ttu-id="ade8f-189">Használhatja a hello [Import-AzureRmAutomationDscNodeConfiguration](/powershell/module/azurerm.automation/import-azurermautomationdscnodeconfiguration) parancsmag tooimport egy csomópont-konfiguráció az automation-fiók be.</span><span class="sxs-lookup"><span data-stu-id="ade8f-189">You can use hello [Import-AzureRmAutomationDscNodeConfiguration](/powershell/module/azurerm.automation/import-azurermautomationdscnodeconfiguration) cmdlet tooimport a node configuration into your automation account.</span></span>

```powershell
Import-AzureRmAutomationDscNodeConfiguration -AutomationAccountName "MyAutomationAccount" -ResourceGroupName "MyResourceGroup" -ConfigurationName "MyNodeConfiguration" -Path "C:\MyConfigurations\TestVM1.mof"
```



