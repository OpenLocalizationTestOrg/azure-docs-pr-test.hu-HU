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
# <a name="test-azure-automation-run-as-account-authentication"></a>Azure Automation futtató fiók hitelesítésének tesztelése
Automation-fiók sikeres létrehozását követően hajthat végre egy egyszerű tesztelési tooconfirm tud toosuccessfully hitelesítéséhez az Azure Resource Manager vagy az Azure klasszikus üzembe helyezési használják az újonnan létrehozott vagy frissített Automation futtató fiókot.    

## <a name="automation-run-as-authentication"></a>Hitelesítés Automation futtató fiókkal
Túl használja az alábbi hello mintakód[létrehozhat egy PowerShell](automation-creating-importing-runbook.md) tooverify hitelesítéssel hello fiókként, az egyéni runbookok tooauthenticate is futnak, és erőforrás-kezelő erőforrások az Automation-fiók kezelése.   

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

Figyelje meg a hitelesítéséhez szeretne használni, a parancsmag hello hello runbook - **Add-AzureRmAccount**, használ hello *ServicePrincipalCertificate* paraméterhalmaz.  Ez a szolgáltatásnév segítségével, és nem hitelesítő adatokkal végzi el a hitelesítést.  

Amikor Ön [hello runbook futtatásához](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) toovalidate a Futtatás mint fiók egy [runbook-feladat](automation-runbook-execution.md) van létrehozása hello feladat panelen jelenik meg, és hello feladatállapot hello jelenik meg **feladat összegzése**csempére. hello feladat állapota elindul *várakozik* azt jelzi, hogy a forgatókönyv-feldolgozók hello felhő toobecome érhető el a vár. Ezután ez az objektum túl*indítása* dolgozó jogcímek hello feladatot, amikor, majd *futtató* amikor hello runbook ténylegesen elindul.  Hello runbook-feladat befejezése után kell állapota látható **befejezve**.

toosee hello hello runbook részletes eredményét, kattintson a hello **kimeneti** csempére.  A hello **kimeneti** panelen megtekintheti az sikeresen hitelesítette és az előfizetésében szereplő összes erőforráscsoport összes erőforrás listáját adja vissza.  

Ne feledje azonban tooremove hello kódblokkot hello Megjegyzés kezdve `#Get all ARM resources from all resource groups` amikor hello kód felhasználhatja a runbookok.

## <a name="classic-run-as-authentication"></a>Hitelesítés klasszikus futtató fiókkal
Túl használja az alábbi hello mintakód[létrehozhat egy PowerShell](automation-creating-importing-runbook.md) tooverify használatával hello klasszikus fiókként, az egyéni runbookok tooauthenticate is futnak, és kezelheti az erőforrásokat hello klasszikus üzembe helyezési modellben.  

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

Amikor Ön [hello runbook futtatásához](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) toovalidate a Futtatás mint fiók egy [runbook-feladat](automation-runbook-execution.md) van létrehozása hello feladat panelen jelenik meg, és hello feladatállapot hello jelenik meg **feladat összegzése**csempére. hello feladat állapota elindul *várakozik* azt jelzi, hogy a forgatókönyv-feldolgozók hello felhő toobecome érhető el a vár. Ezután ez az objektum túl*indítása* dolgozó jogcímek hello feladatot, amikor, majd *futtató* amikor hello runbook ténylegesen elindul.  Hello runbook-feladat befejezése után kell állapota látható **befejezve**.

toosee hello hello runbook részletes eredményét, kattintson a hello **kimeneti** csempére.  A hello **kimeneti** panelen megtekintheti az sikeresen hitelesítette és által az előfizetés üzembe helyezett VMName minden Azure virtuális gépek listáját adja vissza.  

Ne feledje azonban tooremove hello parancsmag **Get-AzureVM** amikor hello kód felhasználhatja a runbookok.

## <a name="next-steps"></a>Következő lépések
* a PowerShell-forgatókönyvek, használatába tooget lásd [az első PowerShell runbook](automation-first-runbook-textual-powershell.md).
* toolearn grafikus szerzői kapcsolatos további információkért lásd: [grafikus készítése az Azure Automationben](automation-graphical-authoring-intro.md).
