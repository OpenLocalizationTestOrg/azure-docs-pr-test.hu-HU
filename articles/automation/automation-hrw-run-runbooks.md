---
title: "Azure Automation hibrid forgatókönyv-feldolgozó aaaRun runbookok |} Microsoft Docs"
description: "Ez a cikk tájékoztatást ad azokról a helyi adatközpontban, illetve a felhőbeli szolgáltató hello hibrid forgatókönyv-feldolgozó szerepkörrel rendelkező gépeken futó runbookok."
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
ms.openlocfilehash: 51961e02603e5690edd11e577594ad2ddea489a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a><span data-ttu-id="4b84f-103">A hibrid forgatókönyv-feldolgozók a futó runbookot</span><span class="sxs-lookup"><span data-stu-id="4b84f-103">Running runbooks on a Hybrid Runbook Worker</span></span> 
<span data-ttu-id="4b84f-104">Nincs különbség az Azure Automation, valamint azokat, amelyeket a hibrid forgatókönyv-feldolgozók futtatnak futó runbookokat hello szerkezete.</span><span class="sxs-lookup"><span data-stu-id="4b84f-104">There is no difference in hello structure of runbooks that run in Azure Automation and those that run on a Hybrid Runbook Worker.</span></span> <span data-ttu-id="4b84f-105">Az összes használt Runbookok valószínűleg jelentős különbség azonban mivel a hibrid forgatókönyv-feldolgozók általában célzó runbookok hello helyi számítógépen saját magát vagy hello helyi környezetben, ahol központilag telepítették, miközben a runbookok erőforrásokon erőforrások kezelése az Azure Automationben általában hello Azure-felhőbe az erőforrások kezelése.</span><span class="sxs-lookup"><span data-stu-id="4b84f-105">Runbooks that you use with each most likely differ significantly though since runbooks targeting a Hybrid Runbook Worker typically manage resources on hello local computer itself or against resources in hello local environment where it is deployed, while runbooks in Azure Automation typically manage resources in hello Azure cloud.</span></span>

<span data-ttu-id="4b84f-106">Runbook szerkesztése a hibrid forgatókönyv-feldolgozó Azure Automation, de előfordulhat, hogy nehézségek tootest hello runbook hello szerkesztőben jelszómódosítás.</span><span class="sxs-lookup"><span data-stu-id="4b84f-106">You can edit a runbook for Hybrid Runbook Worker in Azure Automation, but you may have difficulties if you try tootest hello runbook in hello editor.</span></span>  <span data-ttu-id="4b84f-107">hello PowerShell-modulok hello helyi erőforrások nincs telepítve az Azure Automation környezetben ebben az esetben elérhető, a hello teszt sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="4b84f-107">hello PowerShell modules that access hello local resources may not be installed in your Azure Automation environment in which case, hello test would fail.</span></span>  <span data-ttu-id="4b84f-108">Ha telepíti a hello szükséges modulokat, majd hello runbook fog futni, de nem lesz képes tooaccess helyi erőforrások teljes vizsgálat.</span><span class="sxs-lookup"><span data-stu-id="4b84f-108">If you do install hello required modules, then hello runbook will run, but it will not be able tooaccess local resources for a complete test.</span></span>

## <a name="starting-a-runbook-on-hybrid-runbook-worker"></a><span data-ttu-id="4b84f-109">Runbook indítása a hibrid forgatókönyv-feldolgozó</span><span class="sxs-lookup"><span data-stu-id="4b84f-109">Starting a runbook on Hybrid Runbook Worker</span></span>
<span data-ttu-id="4b84f-110">[Runbook elindítása az Azure Automationben](automation-starting-a-runbook.md) runbook indítása másik módszerét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="4b84f-110">[Starting a Runbook in Azure Automation](automation-starting-a-runbook.md) describes different methods for starting a runbook.</span></span>  <span data-ttu-id="4b84f-111">Hibrid forgatókönyv-feldolgozó hozzáadása egy **RunOn** hello nevét a hibrid forgatókönyv-feldolgozó csoport megadására lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="4b84f-111">Hybrid Runbook Worker adds a **RunOn** option where you can specify hello name of a Hybrid Runbook Worker Group.</span></span>  <span data-ttu-id="4b84f-112">Ha kiválaszt egy csoportot, majd hello runbook lekérése és hello munkavállalók csoport által futtatott.</span><span class="sxs-lookup"><span data-stu-id="4b84f-112">If a group is specified, then hello runbook is retrieved and run by of hello workers in that group.</span></span>  <span data-ttu-id="4b84f-113">Ha ez a beállítás nincs megadva, majd azt legyen futtatva az Azure Automationben normál.</span><span class="sxs-lookup"><span data-stu-id="4b84f-113">If this option is not specified, then it is run in Azure Automation as normal.</span></span>

<span data-ttu-id="4b84f-114">Amikor elindít egy runbookot a hello Azure-portálon, lehetősége lesz egy **futtassa a** beállítás, ahol kiválaszthatja **Azure** vagy **Hibridfeldolgozó**.</span><span class="sxs-lookup"><span data-stu-id="4b84f-114">When you start a runbook in hello Azure portal, you are presented with a **Run on** option where you can select **Azure** or **Hybrid Worker**.</span></span>  <span data-ttu-id="4b84f-115">Ha **Hibridfeldolgozó**, jelölje be hello csoportot a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="4b84f-115">If you select **Hybrid Worker**, then you can select hello group from a dropdown.</span></span>

<span data-ttu-id="4b84f-116">Használjon hello **RunOn** paraméter.</span><span class="sxs-lookup"><span data-stu-id="4b84f-116">Use hello **RunOn** parameter.</span></span>  <span data-ttu-id="4b84f-117">A következő parancs toostart a hibrid forgatókönyv-feldolgozó csoport nevű Windows PowerShell használatával MyHybridGroup a Test-Runbook nevű runbook hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="4b84f-117">You can use hello following command toostart a runbook named Test-Runbook on a Hybrid Runbook Worker Group named MyHybridGroup using Windows PowerShell.</span></span>

    Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"

> [!NOTE]
> <span data-ttu-id="4b84f-118">Hello **RunOn** paraméter meg lett adva toohello **Start-AzureAutomationRunbook** verziójában 0.9.1 Microsoft Azure PowerShell parancsmag.</span><span class="sxs-lookup"><span data-stu-id="4b84f-118">hello **RunOn** parameter was added toohello **Start-AzureAutomationRunbook** cmdlet in version 0.9.1 of Microsoft Azure PowerShell.</span></span>  <span data-ttu-id="4b84f-119">Meg kell [hello legújabb verzió letöltéséhez](https://azure.microsoft.com/downloads/) Ha egy korábbi egy telepítve van.</span><span class="sxs-lookup"><span data-stu-id="4b84f-119">You should [download hello latest version](https://azure.microsoft.com/downloads/) if you have an earlier one installed.</span></span>  <span data-ttu-id="4b84f-120">Csak akkor kell tooinstall ahol indításakor hello runbook Windows powershellből munkaállomáson jelen verziójában.</span><span class="sxs-lookup"><span data-stu-id="4b84f-120">You only need tooinstall this version on a workstation where you are starting hello runbook from Windows PowerShell.</span></span>  <span data-ttu-id="4b84f-121">Nem kell tooinstall hello munkavégző számítógépen, kivéve, ha azt tervezi, hogy toostart runbookok erről a számítógépről.</span><span class="sxs-lookup"><span data-stu-id="4b84f-121">You do not need tooinstall it on hello worker computer unless you intend toostart runbooks from that computer.</span></span>  <span data-ttu-id="4b84f-122">Nem lehet jelenleg elindít egy runbookot egy másik runbookból a hibrid forgatókönyv-feldolgozók a mivel ehhez hello Azure Powershell toobe az Automation-fiók telepített legújabb verziójára van szükség.</span><span class="sxs-lookup"><span data-stu-id="4b84f-122">You cannot currently start a runbook on a Hybrid Runbook Worker from another runbook since this would require hello latest version of Azure Powershell toobe installed in your Automation account.</span></span>  <span data-ttu-id="4b84f-123">hello legújabb verziója automatikusan frissíti az Azure Automationben, és automatikusan leküldött hamarosan le toohello munkavállalók.</span><span class="sxs-lookup"><span data-stu-id="4b84f-123">hello latest version is automatically updated in Azure Automation and automatically pushed down toohello workers soon.</span></span>
>
>

## <a name="runbook-permissions"></a><span data-ttu-id="4b84f-124">Runbookokra vonatkozó engedélyek</span><span class="sxs-lookup"><span data-stu-id="4b84f-124">Runbook permissions</span></span>
<span data-ttu-id="4b84f-125">A hibrid forgatókönyv-feldolgozók futó Runbookok nem használható hello azonos tooAzure erőforrások hitelesítéséhez, mert érik el az Azure-on kívüli erőforrások runbookok általában használt módszer.</span><span class="sxs-lookup"><span data-stu-id="4b84f-125">Runbooks running on a Hybrid Runbook Worker cannot use hello same method that is typically used for runbooks authenticating tooAzure resources, since they are accessing resources outside of Azure.</span></span>  <span data-ttu-id="4b84f-126">hello runbook vagy biztosíthat a saját hitelesítési toolocal erőforrásokat, vagy megadhatja, hogy a RunAs fiók tooprovide az összes runbook felhasználói környezetet.</span><span class="sxs-lookup"><span data-stu-id="4b84f-126">hello runbook can either provide its own authentication toolocal resources, or you can specify a RunAs account tooprovide a user context for all runbooks.</span></span>

### <a name="runbook-authentication"></a><span data-ttu-id="4b84f-127">Runbook-hitelesítés</span><span class="sxs-lookup"><span data-stu-id="4b84f-127">Runbook authentication</span></span>
<span data-ttu-id="4b84f-128">Alapértelmezés szerint runbookokat hello hello hello a helyi számítógépen helyi rendszerfiók környezetében fog futni, meg kell adniuk a saját hitelesítési tooresources, akik hozzáférhetnek a azokat.</span><span class="sxs-lookup"><span data-stu-id="4b84f-128">By default, runbooks will run in hello context of hello local System account on hello on-premises computer, so they must provide their own authentication tooresources that they will access.</span></span>  

<span data-ttu-id="4b84f-129">Használható [hitelesítő adat](http://msdn.microsoft.com/library/dn940015.aspx) és [tanúsítvány](http://msdn.microsoft.com/library/dn940013.aspx) eszközök a runbookban a parancsmagokat, amelyek lehetővé teszik toospecify hitelesítő adatokat, toodifferent erőforrások hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="4b84f-129">You can use [Credential](http://msdn.microsoft.com/library/dn940015.aspx) and [Certificate](http://msdn.microsoft.com/library/dn940013.aspx) assets in your runbook with cmdlets that allow you toospecify credentials so you can authenticate toodifferent resources.</span></span>  <span data-ttu-id="4b84f-130">hello következő példa bemutatja, hogy újraindítja a számítógépet a runbook egy részét.</span><span class="sxs-lookup"><span data-stu-id="4b84f-130">hello following example shows a portion of a runbook that restarts a computer.</span></span>  <span data-ttu-id="4b84f-131">Hitelesítő adatok lekéri a hitelesítő adat eszköz és hello nevének egy változóeszköz hello számítógép, és ezután hello Restart-Computer parancsmag ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="4b84f-131">It retrieves credentials from a credential asset and hello name of hello computer from a variable asset and then uses these values with hello Restart-Computer cmdlet.</span></span>

    $Cred = Get-AzureRmAutomationCredential -ResourceGroupName "ResourceGroup01" -Name "MyCredential"
    $Computer = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" -Name  "ComputerName"

    Restart-Computer -ComputerName $Computer -Credential $Cred

<span data-ttu-id="4b84f-132">Kihasználhatja [InlineScript](automation-powershell-workflow.md#inlinescript), amely lehetővé teszi toorun kódblokkokat, egy másik számítógépen hello által megadott hitelesítő adatokkal [PSCredential általános paraméter](http://technet.microsoft.com/library/jj129719.aspx).</span><span class="sxs-lookup"><span data-stu-id="4b84f-132">You can also leverage [InlineScript](automation-powershell-workflow.md#inlinescript), which  allows you toorun blocks of code on another computer with credentials specified by hello [PSCredential common parameter](http://technet.microsoft.com/library/jj129719.aspx).</span></span>

### <a name="runas-account"></a><span data-ttu-id="4b84f-133">Futtató fiók</span><span class="sxs-lookup"><span data-stu-id="4b84f-133">RunAs account</span></span>
<span data-ttu-id="4b84f-134">Ahelyett, hogy a runbookok biztosít a saját hitelesítési toolocal erőforrásokat, megadhat egy **RunAs** fiókot használja a hibrid feldolgozócsoport.</span><span class="sxs-lookup"><span data-stu-id="4b84f-134">Instead of having runbooks provide their own authentication toolocal resources, you can specify a **RunAs** account for a Hybrid worker group.</span></span>  <span data-ttu-id="4b84f-135">Megadhat egy [hitelesítőadat-eszköz](automation-credentials.md) , amely rendelkezik, toolocal erőforrások eléréséhez, és minden runbook futtatásához ezeket a hitelesítő adatokat a hibrid forgatókönyv-feldolgozók hello csoport használatakor.</span><span class="sxs-lookup"><span data-stu-id="4b84f-135">You specify a [credential asset](automation-credentials.md) that has access toolocal resources, and all runbooks run under these credentials when running on a Hybrid Runbook Worker in hello group.</span></span>  

<span data-ttu-id="4b84f-136">hello felhasználónév hello hitelesítési kell hello a következő formátumok egyikében:</span><span class="sxs-lookup"><span data-stu-id="4b84f-136">hello user name for hello credential must be in one of hello following formats:</span></span>

* <span data-ttu-id="4b84f-137">tartomány\felhasználónév</span><span class="sxs-lookup"><span data-stu-id="4b84f-137">domain\username</span></span>
* username@domain
* <span data-ttu-id="4b84f-138">felhasználónév (a fiókok helyi toohello a helyi számítógép)</span><span class="sxs-lookup"><span data-stu-id="4b84f-138">username (for accounts local toohello on-premises computer)</span></span>

<span data-ttu-id="4b84f-139">A következő eljárás toospecify egy futtató fiókhoz a(z) egy hibrid feldolgozócsoport hello használata:</span><span class="sxs-lookup"><span data-stu-id="4b84f-139">Use hello following procedure toospecify a RunAs account for a Hybrid worker group:</span></span>

1. <span data-ttu-id="4b84f-140">Hozzon létre egy [hitelesítőadat-eszköz](automation-credentials.md) a toolocal erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="4b84f-140">Create a [credential asset](automation-credentials.md) with access toolocal resources.</span></span>
2. <span data-ttu-id="4b84f-141">Nyissa meg a hello Automation-fiók hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="4b84f-141">Open hello Automation account in hello Azure portal.</span></span>
3. <span data-ttu-id="4b84f-142">Jelölje be hello **hibrid dolgozó csoportok** csempére, majd válassza ki a hello csoport.</span><span class="sxs-lookup"><span data-stu-id="4b84f-142">Select hello **Hybrid Worker Groups** tile, and then select hello group.</span></span>
4. <span data-ttu-id="4b84f-143">Válassza ki **összes beállítás** , majd **hibrid feldolgozócsoport beállításai**.</span><span class="sxs-lookup"><span data-stu-id="4b84f-143">Select **All settings** and then **Hybrid worker group settings**.</span></span>
5. <span data-ttu-id="4b84f-144">Változás **futtató** a **alapértelmezett** túl**egyéni**.</span><span class="sxs-lookup"><span data-stu-id="4b84f-144">Change **Run As** from **Default** too**Custom**.</span></span>
6. <span data-ttu-id="4b84f-145">Válassza ki a hello hitelesítő adat, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="4b84f-145">Select hello credential and click **Save**.</span></span>

### <a name="automation-run-as-account"></a><span data-ttu-id="4b84f-146">Automatizálási futtató fiók</span><span class="sxs-lookup"><span data-stu-id="4b84f-146">Automation Run As account</span></span>
<span data-ttu-id="4b84f-147">Az Azure-erőforrások telepítéséhez az automatizált felépítési folyamat részeként lehet szükség az access tooon helyi rendszerek toosupport feladat vagy további lépés a központi telepítési sorrendben.</span><span class="sxs-lookup"><span data-stu-id="4b84f-147">As part of your automated build process for deploying resources in Azure, you may require access tooon-premise systems toosupport a task or set of steps in your deployment sequence.</span></span>  <span data-ttu-id="4b84f-148">Azure-ban hello Futtatás mint fiók toosupport hitelesítést, tanúsítványra van szükség tooinstall hello Futtatás mint fiók.</span><span class="sxs-lookup"><span data-stu-id="4b84f-148">toosupport authentication against Azure using hello Run As account, you need tooinstall hello Run As account certificate.</span></span>  

<span data-ttu-id="4b84f-149">a következő PowerShell-forgatókönyv hello *Export-RunAsCertificateToHybridWorker*, hello futtató tanúsítvány exportálja az Azure Automation-fiók és letölti és a helyi számítógép tanúsítványtárolójába hello importálja a Hibridfeldolgozó csatlakoztatott toohello ugyanazt a fiókot.</span><span class="sxs-lookup"><span data-stu-id="4b84f-149">hello following PowerShell runbook, *Export-RunAsCertificateToHybridWorker*, exports hello Run As certificate from your Azure Automation account and downloads and imports it into hello local machine certificate store on a Hybrid worker connected toohello same account.</span></span>  <span data-ttu-id="4b84f-150">Ha a lépés befejeződött, ellenőrzi hello munkavégző sikeresen hitelesítheti tooAzure hello Futtatás mint fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="4b84f-150">Once that step is completed, it verifies hello worker can successfully authenticate tooAzure using hello Run As account.</span></span>

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
    Exports hello Run As certificate from an Azure Automation account tooa hybrid worker in that account. 
  
    .DESCRIPTION  
    This runbook exports hello Run As certificate from an Azure Automation account tooa hybrid worker in that account.
    Run this runbook in hello hybrid worker where you want hello certificate installed.
    This allows hello use of hello AzureRunAsConnection tooauthenticate tooAzure and manage Azure resources from runbooks running in hello hybrid worker.

    .EXAMPLE
    .\Export-RunAsCertificateToHybridWorker

    .NOTES
    AUTHOR: Azure Automation Team 
    LASTEDIT: 2016.10.13
    #>

    [OutputType([string])] 

    # Set hello password used for this certificate
    $Password = "YourStrongPasswordForTheCert"

    # Stop on errors
    $ErrorActionPreference = 'stop'

    # Get hello management certificate that will be used toomake calls into Azure Service Management resources
    $RunAsCert = Get-AutomationCertificate -Name "AzureRunAsCertificate"
       
    # location toostore temporary certificate in hello Automation service host
    $CertPath = Join-Path $env:temp  "AzureRunAsCertificate.pfx"
   
    # Save hello certificate
    $Cert = $RunAsCert.Export("pfx",$Password)
    Set-Content -Value $Cert -Path $CertPath -Force -Encoding Byte | Write-Verbose 

    Write-Output ("Importing certificate into $env:computername local machine root store from " + $CertPath)
    $SecurePassword = ConvertTo-SecureString $Password -AsPlainText -Force
    Import-PfxCertificate -FilePath $CertPath -CertStoreLocation Cert:\LocalMachine\My -Password $SecurePassword -Exportable | Write-Verbose

    # Test that authentication tooAzure Resource Manager is working
    $RunAsConnection = Get-AutomationConnection -Name "AzureRunAsConnection" 
    
    Add-AzureRmAccount `
      -ServicePrincipal `
      -TenantId $RunAsConnection.TenantId `
      -ApplicationId $RunAsConnection.ApplicationId `
      -CertificateThumbprint $RunAsConnection.CertificateThumbprint | Write-Verbose

    Set-AzureRmContext -SubscriptionId $RunAsConnection.SubscriptionID | Write-Verbose

    # List automation accounts tooconfirm Azure Resource Manager calls are working
    Get-AzureRmAutomationAccount | Select AutomationAccountName

<span data-ttu-id="4b84f-151">Mentés hello *Export-RunAsCertificateToHybridWorker* runbook tooyour számítógép egy `.ps1` bővítmény.</span><span class="sxs-lookup"><span data-stu-id="4b84f-151">Save hello *Export-RunAsCertificateToHybridWorker* runbook tooyour computer with a `.ps1` extension.</span></span>  <span data-ttu-id="4b84f-152">Importálja a fájlt az Automation-fiók és hello forgatókönyv hello hello változó értékének módosítása szerkesztése `$Password` a saját jelszavát.</span><span class="sxs-lookup"><span data-stu-id="4b84f-152">Import it into your Automation account and edit hello runbook, changing hello value of hello variable `$Password` with your own password.</span></span>  <span data-ttu-id="4b84f-153">Közzététele, és futtassa a hello runbook célzó hello hibrid feldolgozócsoport futtatásához, és hitelesíteni runbookokat hello Futtatás mint fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="4b84f-153">Publish and then run hello runbook targeting hello Hybrid Worker group that run and authenticate runbooks using hello Run As account.</span></span>  <span data-ttu-id="4b84f-154">hello feladat adatfolyam jelentések hello kísérlet tooimport hello tanúsítvány hello helyi számítógép tárolójában, és attól függően, hogy hány Automation-fiókok vannak definiálva az előfizetés és a sikeres hitelesítés esetén több sort tartalmazó követi.</span><span class="sxs-lookup"><span data-stu-id="4b84f-154">hello job stream reports hello attempt tooimport hello certificate into hello local machine store, and follows with multiple lines depending on how many Automation accounts are defined in your subscription and if authentication is successful.</span></span>  

## <a name="troubleshooting-runbooks-on-hybrid-runbook-worker"></a><span data-ttu-id="4b84f-155">A hibrid forgatókönyv-feldolgozó hibaelhárítási runbookok</span><span class="sxs-lookup"><span data-stu-id="4b84f-155">Troubleshooting runbooks on Hybrid Runbook Worker</span></span>
<span data-ttu-id="4b84f-156">Naplók minden hibridfeldolgozó C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes, helyileg tárolja.</span><span class="sxs-lookup"><span data-stu-id="4b84f-156">Logs are stored locally on each hybrid worker at C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes.</span></span>  <span data-ttu-id="4b84f-157">Hibridfeldolgozó is rögzíti a hibák és események hello Windows eseménynaplóban **alkalmazások és szolgáltatások Logs\Microsoft-SMA\Operational**.</span><span class="sxs-lookup"><span data-stu-id="4b84f-157">Hybrid worker also records errors and events in hello Windows event log under **Application and Services Logs\Microsoft-SMA\Operational**.</span></span>  <span data-ttu-id="4b84f-158">Csoporttal kapcsolatos események toorunbooks hello munkavégző végrehajtása túl írt**alkalmazások és szolgáltatások Logs\Microsoft-Automation\Operational**.</span><span class="sxs-lookup"><span data-stu-id="4b84f-158">Events related toorunbooks executed on hello worker are written too**Application and Services Logs\Microsoft-Automation\Operational**.</span></span>  <span data-ttu-id="4b84f-159">Hello **Microsoft-SMA** napló számos további események kapcsolódó toohello runbook feladat megnyomott toohello munkavégző és hello feldolgozása hello runbook tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="4b84f-159">hello **Microsoft-SMA** log includes many more events related toohello runbook job pushed toohello worker and hello processing of hello runbook.</span></span>  <span data-ttu-id="4b84f-160">Hello közben **Microsoft-automatizálás** Eseménynapló nincs sok eseményt hello hibaelhárításához a runbook végrehajtása védelmével adatokkal, hello runbook-feladat eredményének hello legalább talál.</span><span class="sxs-lookup"><span data-stu-id="4b84f-160">While hello **Microsoft-Automation** event log does not have many events with details assisting with hello troubleshooting of runbook execution, you will at least find hello results of hello runbook job.</span></span>  

<span data-ttu-id="4b84f-161">[Runbook kimenet és üzenetek](automation-runbook-output-and-messages.md) Automation hibrid feldolgozók csak a például a runbook-feladatok hello felhőben futtatható tooAzure küldése.</span><span class="sxs-lookup"><span data-stu-id="4b84f-161">[Runbook output and messages](automation-runbook-output-and-messages.md) are sent tooAzure Automation from hybrid workers just like runbook jobs run in hello cloud.</span></span>  <span data-ttu-id="4b84f-162">Hello részletes is engedélyezheti, és folyamatban lévő adatfolyamok hello megegyezik más runbookokat ugyanúgy.</span><span class="sxs-lookup"><span data-stu-id="4b84f-162">You can also enable hello Verbose and Progress streams hello same way you would for other runbooks.</span></span>  

<span data-ttu-id="4b84f-163">Ha a runbookok nem sikeres befejezését, és hello feladat összegzése állapotot jelez, **felfüggesztett**, tekintse át a hibaelhárítási cikk hello [hibrid forgatókönyv-feldolgozó: A runbook-feladatok leállítja állapottal Felfüggesztve](automation-troubleshooting-hybrid-runbook-worker.md#a-runbook-job-terminates-with-a-status-of-suspended).</span><span class="sxs-lookup"><span data-stu-id="4b84f-163">If your runbooks are not completing successfully and hello job summary shows a status of **Suspended**, please review hello troubleshooting article [Hybrid Runbook Worker: A runbook job terminates with a status of Suspended](automation-troubleshooting-hybrid-runbook-worker.md#a-runbook-job-terminates-with-a-status-of-suspended).</span></span>   

## <a name="next-steps"></a><span data-ttu-id="4b84f-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4b84f-164">Next steps</span></span>
* <span data-ttu-id="4b84f-165">További információ az hello többféle módszerrel is lehet a runbookban használt toostart toolearn lásd [Runbook elindítása az Azure Automationben](automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="4b84f-165">toolearn more about hello different methods that can be used toostart a runbook, see [Starting a Runbook in Azure Automation](automation-starting-a-runbook.md).</span></span>  
* <span data-ttu-id="4b84f-166">Tekintse meg a toounderstand hello különböző eljárásokkal a PowerShell és a PowerShell-munkafolyamati forgatókönyvek az Azure Automationben hello szöveges-szerkesztő segítségével használata a [az Azure Automationben Runbook szerkesztése](automation-edit-textual-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="4b84f-166">toounderstand hello different procedures for working with PowerShell and PowerShell Workflow runbooks in Azure Automation using hello textual editor, see [Editing a Runbook in Azure Automation](automation-edit-textual-runbook.md)</span></span>
