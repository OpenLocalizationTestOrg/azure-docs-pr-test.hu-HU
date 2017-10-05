---
title: "Azure Automation hibrid forgatókönyv-feldolgozót a runbookok futtatásához |} Microsoft Docs"
description: "Ez a cikk tájékoztatást ad azokról a helyi adatközpontban, illetve a hibrid forgatókönyv-feldolgozó szerepkörrel rendelkező felhőszolgáltatóként gépeken futó runbookok."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 06227cda-f3d1-47fe-b3f8-436d2b9d81ee
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/22/2017
ms.author: magoedte
ms.openlocfilehash: 993bc3ea480a329541ca4ae825189cdb5a2b4a8b
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a><span data-ttu-id="863c8-103">A hibrid forgatókönyv-feldolgozók a futó runbookot</span><span class="sxs-lookup"><span data-stu-id="863c8-103">Running runbooks on a Hybrid Runbook Worker</span></span> 
<span data-ttu-id="863c8-104">Nincs különbség a futó Azure Automation és a hibrid forgatókönyv-feldolgozót a futó runbookok struktúrában.</span><span class="sxs-lookup"><span data-stu-id="863c8-104">There is no difference in the structure of runbooks that run in Azure Automation and those that run on a Hybrid Runbook Worker.</span></span> <span data-ttu-id="863c8-105">Az összes használt Runbookok valószínűleg jelentős különbség azonban mivel a hibrid forgatókönyv-feldolgozók általában célzó runbookok kezelheti az erőforrásokat a helyi számítógép vagy a helyi környezetben, ahol központilag telepítették, a runbookok közben erőforrásokon Azure Automation szolgáltatásbeli általában az Azure felhőalapú erőforrások kezelésére.</span><span class="sxs-lookup"><span data-stu-id="863c8-105">Runbooks that you use with each most likely differ significantly though since runbooks targeting a Hybrid Runbook Worker typically manage resources on the local computer itself or against resources in the local environment where it is deployed, while runbooks in Azure Automation typically manage resources in the Azure cloud.</span></span>

<span data-ttu-id="863c8-106">Runbook szerkesztése a hibrid forgatókönyv-feldolgozó Azure Automation, de előfordulhat, hogy nehézségek Ha kísérli meg a runbook tesztelése a szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="863c8-106">You can edit a runbook for Hybrid Runbook Worker in Azure Automation, but you may have difficulties if you try to test the runbook in the editor.</span></span>  <span data-ttu-id="863c8-107">A PowerShell-moduljai a helyi erőforrások nincs telepítve az Azure Automation környezetben ebben az esetben elérhető, a teszt sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="863c8-107">The PowerShell modules that access the local resources may not be installed in your Azure Automation environment in which case, the test would fail.</span></span>  <span data-ttu-id="863c8-108">Ha a szükséges modulokat telepíti, a runbookot a rendszer futtatja, de akkor nem fog tudni hozzáférni a helyi erőforrások teljes vizsgálat.</span><span class="sxs-lookup"><span data-stu-id="863c8-108">If you do install the required modules, then the runbook will run, but it will not be able to access local resources for a complete test.</span></span>

## <a name="starting-a-runbook-on-hybrid-runbook-worker"></a><span data-ttu-id="863c8-109">Runbook indítása a hibrid forgatókönyv-feldolgozó</span><span class="sxs-lookup"><span data-stu-id="863c8-109">Starting a runbook on Hybrid Runbook Worker</span></span>
<span data-ttu-id="863c8-110">[Runbook elindítása az Azure Automationben](automation-starting-a-runbook.md) runbook indítása másik módszerét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="863c8-110">[Starting a Runbook in Azure Automation](automation-starting-a-runbook.md) describes different methods for starting a runbook.</span></span>  <span data-ttu-id="863c8-111">Hibrid forgatókönyv-feldolgozó hozzáadása egy **RunOn** beállítás, amelyen megadhatja a hibrid forgatókönyv-feldolgozó csoport nevét.</span><span class="sxs-lookup"><span data-stu-id="863c8-111">Hybrid Runbook Worker adds a **RunOn** option where you can specify the name of a Hybrid Runbook Worker Group.</span></span>  <span data-ttu-id="863c8-112">Ha kiválaszt egy csoportot, majd a runbook lekérése és csoport munkavállalók futtatásához.</span><span class="sxs-lookup"><span data-stu-id="863c8-112">If a group is specified, then the runbook is retrieved and run by of the workers in that group.</span></span>  <span data-ttu-id="863c8-113">Ha ez a beállítás nincs megadva, majd azt legyen futtatva az Azure Automationben normál.</span><span class="sxs-lookup"><span data-stu-id="863c8-113">If this option is not specified, then it is run in Azure Automation as normal.</span></span>

<span data-ttu-id="863c8-114">Amikor elindít egy forgatókönyvet az Azure portálon, lehetősége lesz a **futtatnak** beállítás, ahol kiválaszthatja **Azure** vagy **Hibridfeldolgozó**.</span><span class="sxs-lookup"><span data-stu-id="863c8-114">When you start a runbook in the Azure portal, you are presented with a **Run on** option where you can select **Azure** or **Hybrid Worker**.</span></span>  <span data-ttu-id="863c8-115">Ha **Hibridfeldolgozó**, jelölje be a csoport a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="863c8-115">If you select **Hybrid Worker**, then you can select the group from a dropdown.</span></span>

<span data-ttu-id="863c8-116">Használja a **RunOn** paraméter.</span><span class="sxs-lookup"><span data-stu-id="863c8-116">Use the **RunOn** parameter.</span></span>  <span data-ttu-id="863c8-117">A következő paranccsal egy hibrid forgatókönyv-feldolgozó csoport nevű Windows PowerShell használatával MyHybridGroup a Test-Runbook nevű runbookot.</span><span class="sxs-lookup"><span data-stu-id="863c8-117">You can use the following command to start a runbook named Test-Runbook on a Hybrid Runbook Worker Group named MyHybridGroup using Windows PowerShell.</span></span>

    Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"

> [!NOTE]
> <span data-ttu-id="863c8-118">A **RunOn** paraméter hozzá lett adva a **Start-AzureAutomationRunbook** verziójában 0.9.1 Microsoft Azure PowerShell parancsmag.</span><span class="sxs-lookup"><span data-stu-id="863c8-118">The **RunOn** parameter was added to the **Start-AzureAutomationRunbook** cmdlet in version 0.9.1 of Microsoft Azure PowerShell.</span></span>  <span data-ttu-id="863c8-119">Meg kell [a legújabb verzió letöltéséhez](https://azure.microsoft.com/downloads/) Ha egy korábbi egy telepítve van.</span><span class="sxs-lookup"><span data-stu-id="863c8-119">You should [download the latest version](https://azure.microsoft.com/downloads/) if you have an earlier one installed.</span></span>  <span data-ttu-id="863c8-120">Csak szeretné ezt a verziót telepíteni egy adott számítógépet a runbook Windows PowerShell munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="863c8-120">You only need to install this version on a workstation where you are starting the runbook from Windows PowerShell.</span></span>  <span data-ttu-id="863c8-121">Nem kell telepíteni a munkavégző számítógépre, ha runbookokat elindítania erről a számítógépről.</span><span class="sxs-lookup"><span data-stu-id="863c8-121">You do not need to install it on the worker computer unless you intend to start runbooks from that computer.</span></span>  <span data-ttu-id="863c8-122">Nem lehet jelenleg elindít egy runbookot egy másik runbookból a hibrid forgatókönyv-feldolgozók a mivel ehhez az Automation-fiók telepíteni az Azure Powershell legújabb verziójára van szükség.</span><span class="sxs-lookup"><span data-stu-id="863c8-122">You cannot currently start a runbook on a Hybrid Runbook Worker from another runbook since this would require the latest version of Azure Powershell to be installed in your Automation account.</span></span>  <span data-ttu-id="863c8-123">A legújabb verzióra automatikusan frissíti az Azure Automationben, és automatikusan leküldve a munkavállalók hamarosan.</span><span class="sxs-lookup"><span data-stu-id="863c8-123">The latest version is automatically updated in Azure Automation and automatically pushed down to the workers soon.</span></span>
>
>

## <a name="runbook-permissions"></a><span data-ttu-id="863c8-124">Runbookokra vonatkozó engedélyek</span><span class="sxs-lookup"><span data-stu-id="863c8-124">Runbook permissions</span></span>
<span data-ttu-id="863c8-125">A hibrid forgatókönyv-feldolgozók futó Runbookok ugyanezt a módszert, amelyet főként a runbookok óta érik el az Azure-on kívüli erőforrások hitelesítéséhez az Azure-erőforrások, nem használható.</span><span class="sxs-lookup"><span data-stu-id="863c8-125">Runbooks running on a Hybrid Runbook Worker cannot use the same method that is typically used for runbooks authenticating to Azure resources, since they are accessing resources outside of Azure.</span></span>  <span data-ttu-id="863c8-126">A runbook vagy adja meg a saját helyi erőforrások hitelesítéséhez, vagy megadhat egy futtató fiókot, felhasználói környezetet biztosít az összes runbook.</span><span class="sxs-lookup"><span data-stu-id="863c8-126">The runbook can either provide its own authentication to local resources, or you can specify a RunAs account to provide a user context for all runbooks.</span></span>

### <a name="runbook-authentication"></a><span data-ttu-id="863c8-127">Runbook-hitelesítés</span><span class="sxs-lookup"><span data-stu-id="863c8-127">Runbook authentication</span></span>
<span data-ttu-id="863c8-128">Alapértelmezés szerint runbookok fog futni a helyi rendszerfiók környezetében a helyi számítógépen, ezért meg kell adniuk a saját hitelesítési, akik hozzáférhetnek a azok erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="863c8-128">By default, runbooks will run in the context of the local System account on the on-premises computer, so they must provide their own authentication to resources that they will access.</span></span>  

<span data-ttu-id="863c8-129">Használhat [hitelesítő adat](http://msdn.microsoft.com/library/dn940015.aspx) és [tanúsítvány](http://msdn.microsoft.com/library/dn940013.aspx) eszközök a runbookban a parancsmagokat, amelyek lehetővé teszik a hitelesítő adatok megadásához, így a különböző erőforrások elvégezheti a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="863c8-129">You can use [Credential](http://msdn.microsoft.com/library/dn940015.aspx) and [Certificate](http://msdn.microsoft.com/library/dn940013.aspx) assets in your runbook with cmdlets that allow you to specify credentials so you can authenticate to different resources.</span></span>  <span data-ttu-id="863c8-130">A következő példa bemutatja, hogy újraindítja a számítógépet a runbook egy részét.</span><span class="sxs-lookup"><span data-stu-id="863c8-130">The following example shows a portion of a runbook that restarts a computer.</span></span>  <span data-ttu-id="863c8-131">Hitelesítő adatok lekéri a hitelesítőadat-eszköz és a változó eszköz a számítógép nevét, és ezután a Restart-Computer parancsmag ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="863c8-131">It retrieves credentials from a credential asset and the name of the computer from a variable asset and then uses these values with the Restart-Computer cmdlet.</span></span>

    $Cred = Get-AzureRmAutomationCredential -ResourceGroupName "ResourceGroup01" -Name "MyCredential"
    $Computer = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" -Name  "ComputerName"

    Restart-Computer -ComputerName $Computer -Credential $Cred

<span data-ttu-id="863c8-132">Kihasználhatja [InlineScript](automation-powershell-workflow.md#inlinescript), amely lehetővé teszi, hogy kódblokkokat, egy másik számítógépen által megadott hitelesítő adatokkal futtathatja a [PSCredential általános paraméter](http://technet.microsoft.com/library/jj129719.aspx).</span><span class="sxs-lookup"><span data-stu-id="863c8-132">You can also leverage [InlineScript](automation-powershell-workflow.md#inlinescript), which  allows you to run blocks of code on another computer with credentials specified by the [PSCredential common parameter](http://technet.microsoft.com/library/jj129719.aspx).</span></span>

### <a name="runas-account"></a><span data-ttu-id="863c8-133">Futtató fiók</span><span class="sxs-lookup"><span data-stu-id="863c8-133">RunAs account</span></span>
<span data-ttu-id="863c8-134">Ahelyett, hogy a runbookok adja meg a saját helyi erőforrások hitelesítéséhez, megadhat egy **RunAs** fiókot használja a hibrid feldolgozócsoport.</span><span class="sxs-lookup"><span data-stu-id="863c8-134">Instead of having runbooks provide their own authentication to local resources, you can specify a **RunAs** account for a Hybrid worker group.</span></span>  <span data-ttu-id="863c8-135">Megadhat egy [hitelesítőadat-eszköz](automation-credentials.md) , amely rendelkezik a helyi erőforrások elérését, és minden runbook futtatásához ezeket a hitelesítő adatokat a hibrid forgatókönyv-feldolgozó csoport használatakor.</span><span class="sxs-lookup"><span data-stu-id="863c8-135">You specify a [credential asset](automation-credentials.md) that has access to local resources, and all runbooks run under these credentials when running on a Hybrid Runbook Worker in the group.</span></span>  

<span data-ttu-id="863c8-136">A felhasználónév a hitelesítő adatokat kell lennie a következő formátumok egyikében:</span><span class="sxs-lookup"><span data-stu-id="863c8-136">The user name for the credential must be in one of the following formats:</span></span>

* <span data-ttu-id="863c8-137">tartomány\felhasználónév</span><span class="sxs-lookup"><span data-stu-id="863c8-137">domain\username</span></span>
* username@domain
* <span data-ttu-id="863c8-138">felhasználónév (fiókok esetében a helyi számítógép helyi)</span><span class="sxs-lookup"><span data-stu-id="863c8-138">username (for accounts local to the on-premises computer)</span></span>

<span data-ttu-id="863c8-139">A következő eljárás használatával adjon meg egy hibrid feldolgozócsoport futtató fiókot:</span><span class="sxs-lookup"><span data-stu-id="863c8-139">Use the following procedure to specify a RunAs account for a Hybrid worker group:</span></span>

1. <span data-ttu-id="863c8-140">Hozzon létre egy [hitelesítőadat-eszköz](automation-credentials.md) helyi erőforrásokhoz való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="863c8-140">Create a [credential asset](automation-credentials.md) with access to local resources.</span></span>
2. <span data-ttu-id="863c8-141">Nyissa meg az Automation-fiók az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="863c8-141">Open the Automation account in the Azure portal.</span></span>
3. <span data-ttu-id="863c8-142">Válassza ki a **hibrid dolgozó csoportok** csempére, majd válassza ki a csoport.</span><span class="sxs-lookup"><span data-stu-id="863c8-142">Select the **Hybrid Worker Groups** tile, and then select the group.</span></span>
4. <span data-ttu-id="863c8-143">Válassza ki **összes beállítás** , majd **hibrid feldolgozócsoport beállításai**.</span><span class="sxs-lookup"><span data-stu-id="863c8-143">Select **All settings** and then **Hybrid worker group settings**.</span></span>
5. <span data-ttu-id="863c8-144">Változás **futtató** a **alapértelmezett** való **egyéni**.</span><span class="sxs-lookup"><span data-stu-id="863c8-144">Change **Run As** from **Default** to **Custom**.</span></span>
6. <span data-ttu-id="863c8-145">Válassza ki a hitelesítő adatokat, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="863c8-145">Select the credential and click **Save**.</span></span>

### <a name="automation-run-as-account"></a><span data-ttu-id="863c8-146">Automatizálási futtató fiók</span><span class="sxs-lookup"><span data-stu-id="863c8-146">Automation Run As account</span></span>
<span data-ttu-id="863c8-147">Az Azure-erőforrások telepítéséhez az automatizált felépítési folyamat részeként szüksége lehet a helyszíni rendszerek támogatásához egy feladat vagy a lépéseket a telepítési sorrendjét elérésére.</span><span class="sxs-lookup"><span data-stu-id="863c8-147">As part of your automated build process for deploying resources in Azure, you may require access to on-premise systems to support a task or set of steps in your deployment sequence.</span></span>  <span data-ttu-id="863c8-148">Azure-ban a Futtatás mint fiók ellen-hitelesítés támogatásához telepítendő a Futtatás mint fiók tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="863c8-148">To support authentication against Azure using the Run As account, you need to install the Run As account certificate.</span></span>  

<span data-ttu-id="863c8-149">A következő PowerShell-forgatókönyv *Export-RunAsCertificateToHybridWorker*, exportálja a Futtatás mint tanúsítvány az Azure Automation-fiók és tölti le, és importálja azokat a helyi számítógép tanúsítványtárolójába a hibrid munkavégző ugyanazzal a fiókkal csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="863c8-149">The following PowerShell runbook, *Export-RunAsCertificateToHybridWorker*, exports the Run As certificate from your Azure Automation account and downloads and imports it into the local machine certificate store on a Hybrid worker connected to the same account.</span></span>  <span data-ttu-id="863c8-150">Ha a lépés befejeződött, ellenőrzi a dolgozó sikeresen hitelesítheti a Futtatás mint fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="863c8-150">Once that step is completed, it verifies the worker can successfully authenticate to Azure using the Run As account.</span></span>

    <#PSScriptInfo
    .VERSION 1.0
    .GUID 3a796b9a-623d-499d-86c8-c249f10a6986
    .AUTHOR Azure Automation Team
    .COMPANYNAME Microsoft
    .COPYRIGHT 
    .TAGS Azure Automation 
    .LICENSEURI 
    .PROJECTURI 
    .ICONURI 
    .EXTERNALMODULEDEPENDENCIES 
    .REQUIREDSCRIPTS 
    .EXTERNALSCRIPTDEPENDENCIES 
    .RELEASENOTES
    #>

    <#  
    .SYNOPSIS  
    Exports the Run As certificate from an Azure Automation account to a hybrid worker in that account. 
  
    .DESCRIPTION  
    This runbook exports the Run As certificate from an Azure Automation account to a hybrid worker in that account.
    Run this runbook in the hybrid worker where you want the certificate installed.
    This allows the use of the AzureRunAsConnection to authenticate to Azure and manage Azure resources from runbooks running in the hybrid worker.

    .EXAMPLE
    .\Export-RunAsCertificateToHybridWorker

    .NOTES
    AUTHOR: Azure Automation Team 
    LASTEDIT: 2016.10.13
    #>

    [OutputType([string])] 

    # Set the password used for this certificate
    $Password = "YourStrongPasswordForTheCert"

    # Stop on errors
    $ErrorActionPreference = 'stop'

    # Get the management certificate that will be used to make calls into Azure Service Management resources
    $RunAsCert = Get-AutomationCertificate -Name "AzureRunAsCertificate"
       
    # location to store temporary certificate in the Automation service host
    $CertPath = Join-Path $env:temp  "AzureRunAsCertificate.pfx"
   
    # Save the certificate
    $Cert = $RunAsCert.Export("pfx",$Password)
    Set-Content -Value $Cert -Path $CertPath -Force -Encoding Byte | Write-Verbose 

    Write-Output ("Importing certificate into $env:computername local machine root store from " + $CertPath)
    $SecurePassword = ConvertTo-SecureString $Password -AsPlainText -Force
    Import-PfxCertificate -FilePath $CertPath -CertStoreLocation Cert:\LocalMachine\My -Password $SecurePassword -Exportable | Write-Verbose

    # Test that authentication to Azure Resource Manager is working
    $RunAsConnection = Get-AutomationConnection -Name "AzureRunAsConnection" 
    
    Add-AzureRmAccount `
      -ServicePrincipal `
      -TenantId $RunAsConnection.TenantId `
      -ApplicationId $RunAsConnection.ApplicationId `
      -CertificateThumbprint $RunAsConnection.CertificateThumbprint | Write-Verbose

    Set-AzureRmContext -SubscriptionId $RunAsConnection.SubscriptionID | Write-Verbose

    # List automation accounts to confirm Azure Resource Manager calls are working
    Get-AzureRmAutomationAccount | Select AutomationAccountName

<span data-ttu-id="863c8-151">Mentse a *Export-RunAsCertificateToHybridWorker* a számítógépet a runbook egy `.ps1` bővítmény.</span><span class="sxs-lookup"><span data-stu-id="863c8-151">Save the *Export-RunAsCertificateToHybridWorker* runbook to your computer with a `.ps1` extension.</span></span>  <span data-ttu-id="863c8-152">Importálja a fájlt az Automation-fiók és szerkeszteni a runbookot, a változó értékének módosítása `$Password` a saját jelszavát.</span><span class="sxs-lookup"><span data-stu-id="863c8-152">Import it into your Automation account and edit the runbook, changing the value of the variable `$Password` with your own password.</span></span>  <span data-ttu-id="863c8-153">Közzététele, és futtassa a célcsoport-kezelési a hibrid feldolgozócsoport futtatásához, és a Futtatás mint fiókkal runbookok hitelesítéséhez a runbook.</span><span class="sxs-lookup"><span data-stu-id="863c8-153">Publish and then run the runbook targeting the Hybrid Worker group that run and authenticate runbooks using the Run As account.</span></span>  <span data-ttu-id="863c8-154">A feladatstream jelentés irányuló kísérlet: importálja a tanúsítványt a helyi számítógép tárolójában, és attól függően, hogy hány Automation-fiókok vannak definiálva az előfizetés és a sikeres hitelesítés esetén több sort tartalmazó követi.</span><span class="sxs-lookup"><span data-stu-id="863c8-154">The job stream reports the attempt to import the certificate into the local machine store, and follows with multiple lines depending on how many Automation accounts are defined in your subscription and if authentication is successful.</span></span>  

## <a name="troubleshooting-runbooks-on-hybrid-runbook-worker"></a><span data-ttu-id="863c8-155">A hibrid forgatókönyv-feldolgozó hibaelhárítási runbookok</span><span class="sxs-lookup"><span data-stu-id="863c8-155">Troubleshooting runbooks on Hybrid Runbook Worker</span></span>
<span data-ttu-id="863c8-156">Naplók minden hibridfeldolgozó C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes, helyileg tárolja.</span><span class="sxs-lookup"><span data-stu-id="863c8-156">Logs are stored locally on each hybrid worker at C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes.</span></span>  <span data-ttu-id="863c8-157">Hibridfeldolgozó hibák és események a rögzíti a Windows eseménynaplóban **alkalmazások és szolgáltatások Logs\Microsoft-SMA\Operational**.</span><span class="sxs-lookup"><span data-stu-id="863c8-157">Hybrid worker also records errors and events in the Windows event log under **Application and Services Logs\Microsoft-SMA\Operational**.</span></span>  <span data-ttu-id="863c8-158">A munkavégző végre runbookokkal kapcsolatos eseményeket jegyez **alkalmazások és szolgáltatások Logs\Microsoft-Automation\Operational**.</span><span class="sxs-lookup"><span data-stu-id="863c8-158">Events related to runbooks executed on the worker are written to **Application and Services Logs\Microsoft-Automation\Operational**.</span></span>  <span data-ttu-id="863c8-159">A **Microsoft-SMA** napló a runbook-feladat a dolgozó és a runbook a feldolgozási leküldött kapcsolódó számos további eseményeket is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="863c8-159">The **Microsoft-SMA** log includes many more events related to the runbook job pushed to the worker and the processing of the runbook.</span></span>  <span data-ttu-id="863c8-160">Amíg a **Microsoft-automatizálás** Eseménynapló nincs sok eseményt a runbook végrehajtása hibaelhárítás védelmével adatokkal, legalább megtalálja a runbook-feladat eredményét.</span><span class="sxs-lookup"><span data-stu-id="863c8-160">While the **Microsoft-Automation** event log does not have many events with details assisting with the troubleshooting of runbook execution, you will at least find the results of the runbook job.</span></span>  

<span data-ttu-id="863c8-161">[Runbook kimenet és üzenetek](automation-runbook-output-and-messages.md) az Azure Automation hibrid küldi munkavállalók hasonlóan a runbook-feladatok futtatása a felhőben.</span><span class="sxs-lookup"><span data-stu-id="863c8-161">[Runbook output and messages](automation-runbook-output-and-messages.md) are sent to Azure Automation from hybrid workers just like runbook jobs run in the cloud.</span></span>  <span data-ttu-id="863c8-162">A részletes és az állapot adatfolyamokat a módon más runbookok is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="863c8-162">You can also enable the Verbose and Progress streams the same way you would for other runbooks.</span></span>  

<span data-ttu-id="863c8-163">Ha a runbookok nem sikeres befejezését és az összefoglaló feladat állapota **felfüggesztett**, tekintse át a hibaelhárítási cikk [hibrid forgatókönyv-feldolgozó: A runbook-feladat leállítása felfüggesztett állapotú ](automation-troubleshooting-hybrid-runbook-worker.md#a-runbook-job-terminates-with-a-status-of-suspended).</span><span class="sxs-lookup"><span data-stu-id="863c8-163">If your runbooks are not completing successfully and the job summary shows a status of **Suspended**, please review the troubleshooting article [Hybrid Runbook Worker: A runbook job terminates with a status of Suspended](automation-troubleshooting-hybrid-runbook-worker.md#a-runbook-job-terminates-with-a-status-of-suspended).</span></span>   

## <a name="next-steps"></a><span data-ttu-id="863c8-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="863c8-164">Next steps</span></span>
* <span data-ttu-id="863c8-165">A runbook-indítási használható különböző módszerekkel kapcsolatos további információkért lásd: [Runbook elindítása az Azure Automationben](automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="863c8-165">To learn more about the different methods that can be used to start a runbook, see [Starting a Runbook in Azure Automation](automation-starting-a-runbook.md).</span></span>  
* <span data-ttu-id="863c8-166">A különböző eljárásokkal a PowerShell és a PowerShell-munkafolyamati forgatókönyvek az Azure Automationben a szöveges szerkesztővel használata a ismertetése: [az Azure Automationben Runbook szerkesztése](automation-edit-textual-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="863c8-166">To understand the different procedures for working with PowerShell and PowerShell Workflow runbooks in Azure Automation using the textual editor, see [Editing a Runbook in Azure Automation](automation-edit-textual-runbook.md)</span></span>