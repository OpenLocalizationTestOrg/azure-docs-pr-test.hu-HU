---
title: "Azure Automation-fiók konfigurációja aaaValidate |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooconfirm hello konfigurálása az Automation-fiók helyesen beállítva."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: magoedte
ms.openlocfilehash: 3a990dcc6661cf67c4b62592ce03d55a3791053a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="test-azure-automation-run-as-account-authentication"></a><span data-ttu-id="59246-103">Azure Automation futtató fiók hitelesítésének tesztelése</span><span class="sxs-lookup"><span data-stu-id="59246-103">Test Azure Automation Run As account authentication</span></span>
<span data-ttu-id="59246-104">Automation-fiók sikeres létrehozását követően hajthat végre egy egyszerű tesztelési tooconfirm tud toosuccessfully hitelesítéséhez az Azure Resource Manager vagy az Azure klasszikus üzembe helyezési használják az újonnan létrehozott vagy frissített Automation futtató fiókot.</span><span class="sxs-lookup"><span data-stu-id="59246-104">After an Automation account is successfully created, you can perform a simple test tooconfirm you are able toosuccessfully authenticate in Azure Resource Manager or Azure classic deployment using your newly created or updated Automation Run As account.</span></span>    

## <a name="automation-run-as-authentication"></a><span data-ttu-id="59246-105">Hitelesítés Automation futtató fiókkal</span><span class="sxs-lookup"><span data-stu-id="59246-105">Automation Run As authentication</span></span>
<span data-ttu-id="59246-106">Túl használja az alábbi hello mintakód[létrehozhat egy PowerShell](automation-creating-importing-runbook.md) tooverify hitelesítéssel hello fiókként, az egyéni runbookok tooauthenticate is futnak, és erőforrás-kezelő erőforrások az Automation-fiók kezelése.</span><span class="sxs-lookup"><span data-stu-id="59246-106">Use hello sample code below too[create a PowerShell runbook](automation-creating-importing-runbook.md) tooverify authentication using hello Run As account and also in your custom runbooks tooauthenticate and manage Resource Manager resources with your Automation account.</span></span>   

    $connectionName = "AzureRunAsConnection"
    try
    {
        # Get hello connection "AzureRunAsConnection "
        $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

        "Logging in tooAzure..."
        Add-AzureRmAccount `
           -ServicePrincipal `
           -TenantId $servicePrincipalConnection.TenantId `
           -ApplicationId $servicePrincipalConnection.ApplicationId `
           -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
    }
    catch {
       if (!$servicePrincipalConnection)
       {
          $ErrorMessage = "Connection $connectionName not found."
          throw $ErrorMessage
      } else{
          Write-Error -Message $_.Exception
          throw $_.Exception
      }
    }

    #Get all ARM resources from all resource groups
    $ResourceGroups = Get-AzureRmResourceGroup 

    foreach ($ResourceGroup in $ResourceGroups)
    {    
       Write-Output ("Showing resources in resource group " + $ResourceGroup.ResourceGroupName)
       $Resources = Find-AzureRmResource -ResourceGroupNameContains $ResourceGroup.ResourceGroupName | Select ResourceName, ResourceType
       ForEach ($Resource in $Resources)
       {
          Write-Output ($Resource.ResourceName + " of type " +  $Resource.ResourceType)
       }
       Write-Output ("")
    } 

<span data-ttu-id="59246-107">Figyelje meg a hitelesítéséhez szeretne használni, a parancsmag hello hello runbook - **Add-AzureRmAccount**, használ hello *ServicePrincipalCertificate* paraméterhalmaz.</span><span class="sxs-lookup"><span data-stu-id="59246-107">Notice hello cmdlet used for authenticating in hello runbook - **Add-AzureRmAccount**, uses hello *ServicePrincipalCertificate* parameter set.</span></span>  <span data-ttu-id="59246-108">Ez a szolgáltatásnév segítségével, és nem hitelesítő adatokkal végzi el a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="59246-108">It authenticates by using service principal certificate, not credentials.</span></span>  

<span data-ttu-id="59246-109">Amikor Ön [hello runbook futtatásához](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) toovalidate a Futtatás mint fiók egy [runbook-feladat](automation-runbook-execution.md) van létrehozása hello feladat panelen jelenik meg, és hello feladatállapot hello jelenik meg **feladat összegzése**csempére.</span><span class="sxs-lookup"><span data-stu-id="59246-109">When you [run hello runbook](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) toovalidate your Run As account, a [runbook job](automation-runbook-execution.md) is created, hello Job blade is displayed, and hello job status displayed in hello **Job Summary** tile.</span></span> <span data-ttu-id="59246-110">hello feladat állapota elindul *várakozik* azt jelzi, hogy a forgatókönyv-feldolgozók hello felhő toobecome érhető el a vár.</span><span class="sxs-lookup"><span data-stu-id="59246-110">hello job status will start as *Queued* indicating that it is waiting for a runbook worker in hello cloud toobecome available.</span></span> <span data-ttu-id="59246-111">Ezután ez az objektum túl*indítása* dolgozó jogcímek hello feladatot, amikor, majd *futtató* amikor hello runbook ténylegesen elindul.</span><span class="sxs-lookup"><span data-stu-id="59246-111">It will then move too*Starting* when a worker claims hello job, and then *Running* when hello runbook actually starts running.</span></span>  <span data-ttu-id="59246-112">Hello runbook-feladat befejezése után kell állapota látható **befejezve**.</span><span class="sxs-lookup"><span data-stu-id="59246-112">When hello runbook job completes, we should see a status of **Completed**.</span></span>

<span data-ttu-id="59246-113">toosee hello hello runbook részletes eredményét, kattintson a hello **kimeneti** csempére.</span><span class="sxs-lookup"><span data-stu-id="59246-113">toosee hello detailed results of hello runbook, click on hello **Output** tile.</span></span>  <span data-ttu-id="59246-114">A hello **kimeneti** panelen megtekintheti az sikeresen hitelesítette és az előfizetésében szereplő összes erőforráscsoport összes erőforrás listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="59246-114">On hello **Output** blade, you should see it has successfully authenticated and returns a list of all resources in all resource groups in your subscription.</span></span>  

<span data-ttu-id="59246-115">Ne feledje azonban tooremove hello kódblokkot hello Megjegyzés kezdve `#Get all ARM resources from all resource groups` amikor hello kód felhasználhatja a runbookok.</span><span class="sxs-lookup"><span data-stu-id="59246-115">Just remember tooremove hello block of code starting with hello comment `#Get all ARM resources from all resource groups` when you reuse hello code for your runbooks.</span></span>

## <a name="classic-run-as-authentication"></a><span data-ttu-id="59246-116">Hitelesítés klasszikus futtató fiókkal</span><span class="sxs-lookup"><span data-stu-id="59246-116">Classic Run As authentication</span></span>
<span data-ttu-id="59246-117">Túl használja az alábbi hello mintakód[létrehozhat egy PowerShell](automation-creating-importing-runbook.md) tooverify használatával hello klasszikus fiókként, az egyéni runbookok tooauthenticate is futnak, és kezelheti az erőforrásokat hello klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="59246-117">Use hello sample code below too[create a PowerShell runbook](automation-creating-importing-runbook.md) tooverify authentication using hello Classic Run As account and also in your custom runbooks tooauthenticate and manage resources in hello classic deployment model.</span></span>  

    $ConnectionAssetName = "AzureClassicRunAsConnection"
    # Get hello connection
    $connection = Get-AutomationConnection -Name $connectionAssetName        

    # Authenticate tooAzure with certificate
    Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
    if ($Conn -eq $null)
    {
       throw "Could not retrieve connection asset: $ConnectionAssetName. Assure that this asset exists in hello Automation account."
    }

    $CertificateAssetName = $Conn.CertificateAssetName
    Write-Verbose "Getting hello certificate: $CertificateAssetName" -Verbose
    $AzureCert = Get-AutomationCertificate -Name $CertificateAssetName
    if ($AzureCert -eq $null)
    {
       throw "Could not retrieve certificate asset: $CertificateAssetName. Assure that this asset exists in hello Automation account."
    }

    Write-Verbose "Authenticating tooAzure with certificate." -Verbose
    Set-AzureSubscription -SubscriptionName $Conn.SubscriptionName -SubscriptionId $Conn.SubscriptionID -Certificate $AzureCert
    Select-AzureSubscription -SubscriptionId $Conn.SubscriptionID
    
    #Get all VMs in hello subscription and return list with name of each
    Get-AzureVM | ft Name

<span data-ttu-id="59246-118">Amikor Ön [hello runbook futtatásához](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) toovalidate a Futtatás mint fiók egy [runbook-feladat](automation-runbook-execution.md) van létrehozása hello feladat panelen jelenik meg, és hello feladatállapot hello jelenik meg **feladat összegzése**csempére.</span><span class="sxs-lookup"><span data-stu-id="59246-118">When you [run hello runbook](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) toovalidate your Run As account, a [runbook job](automation-runbook-execution.md) is created, hello Job blade is displayed, and hello job status displayed in hello **Job Summary** tile.</span></span> <span data-ttu-id="59246-119">hello feladat állapota elindul *várakozik* azt jelzi, hogy a forgatókönyv-feldolgozók hello felhő toobecome érhető el a vár.</span><span class="sxs-lookup"><span data-stu-id="59246-119">hello job status will start as *Queued* indicating that it is waiting for a runbook worker in hello cloud toobecome available.</span></span> <span data-ttu-id="59246-120">Ezután ez az objektum túl*indítása* dolgozó jogcímek hello feladatot, amikor, majd *futtató* amikor hello runbook ténylegesen elindul.</span><span class="sxs-lookup"><span data-stu-id="59246-120">It will then move too*Starting* when a worker claims hello job, and then *Running* when hello runbook actually starts running.</span></span>  <span data-ttu-id="59246-121">Hello runbook-feladat befejezése után kell állapota látható **befejezve**.</span><span class="sxs-lookup"><span data-stu-id="59246-121">When hello runbook job completes, we should see a status of **Completed**.</span></span>

<span data-ttu-id="59246-122">toosee hello hello runbook részletes eredményét, kattintson a hello **kimeneti** csempére.</span><span class="sxs-lookup"><span data-stu-id="59246-122">toosee hello detailed results of hello runbook, click on hello **Output** tile.</span></span>  <span data-ttu-id="59246-123">A hello **kimeneti** panelen megtekintheti az sikeresen hitelesítette és által az előfizetés üzembe helyezett VMName minden Azure virtuális gépek listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="59246-123">On hello **Output** blade, you should see it has successfully authenticated and returns a list of all Azure VMs by VMName that are deployed in your subscription.</span></span>  

<span data-ttu-id="59246-124">Ne feledje azonban tooremove hello parancsmag **Get-AzureVM** amikor hello kód felhasználhatja a runbookok.</span><span class="sxs-lookup"><span data-stu-id="59246-124">Just remember tooremove hello cmdlet **Get-AzureVM** when you reuse hello code for your runbooks.</span></span>

## <a name="next-steps"></a><span data-ttu-id="59246-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="59246-125">Next steps</span></span>
* <span data-ttu-id="59246-126">a PowerShell-forgatókönyvek, használatába tooget lásd [az első PowerShell runbook](automation-first-runbook-textual-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="59246-126">tooget started with PowerShell runbooks, see [My first PowerShell runbook](automation-first-runbook-textual-powershell.md).</span></span>
* <span data-ttu-id="59246-127">toolearn grafikus szerzői kapcsolatos további információkért lásd: [grafikus készítése az Azure Automationben](automation-graphical-authoring-intro.md).</span><span class="sxs-lookup"><span data-stu-id="59246-127">toolearn more about Graphical Authoring, see [Graphical authoring in Azure Automation](automation-graphical-authoring-intro.md).</span></span>
