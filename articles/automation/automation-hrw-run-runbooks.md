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
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a>A hibrid forgatókönyv-feldolgozók a futó runbookot 
Nincs különbség az Azure Automation, valamint azokat, amelyeket a hibrid forgatókönyv-feldolgozók futtatnak futó runbookokat hello szerkezete. Az összes használt Runbookok valószínűleg jelentős különbség azonban mivel a hibrid forgatókönyv-feldolgozók általában célzó runbookok hello helyi számítógépen saját magát vagy hello helyi környezetben, ahol központilag telepítették, miközben a runbookok erőforrásokon erőforrások kezelése az Azure Automationben általában hello Azure-felhőbe az erőforrások kezelése.

Runbook szerkesztése a hibrid forgatókönyv-feldolgozó Azure Automation, de előfordulhat, hogy nehézségek tootest hello runbook hello szerkesztőben jelszómódosítás.  hello PowerShell-modulok hello helyi erőforrások nincs telepítve az Azure Automation környezetben ebben az esetben elérhető, a hello teszt sikertelen lesz.  Ha telepíti a hello szükséges modulokat, majd hello runbook fog futni, de nem lesz képes tooaccess helyi erőforrások teljes vizsgálat.

## <a name="starting-a-runbook-on-hybrid-runbook-worker"></a>Runbook indítása a hibrid forgatókönyv-feldolgozó
[Runbook elindítása az Azure Automationben](automation-starting-a-runbook.md) runbook indítása másik módszerét ismerteti.  Hibrid forgatókönyv-feldolgozó hozzáadása egy **RunOn** hello nevét a hibrid forgatókönyv-feldolgozó csoport megadására lehetőséget.  Ha kiválaszt egy csoportot, majd hello runbook lekérése és hello munkavállalók csoport által futtatott.  Ha ez a beállítás nincs megadva, majd azt legyen futtatva az Azure Automationben normál.

Amikor elindít egy runbookot a hello Azure-portálon, lehetősége lesz egy **futtassa a** beállítás, ahol kiválaszthatja **Azure** vagy **Hibridfeldolgozó**.  Ha **Hibridfeldolgozó**, jelölje be hello csoportot a legördülő listából.

Használjon hello **RunOn** paraméter.  A következő parancs toostart a hibrid forgatókönyv-feldolgozó csoport nevű Windows PowerShell használatával MyHybridGroup a Test-Runbook nevű runbook hello is használhatja.

    Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"

> [!NOTE]
> Hello **RunOn** paraméter meg lett adva toohello **Start-AzureAutomationRunbook** verziójában 0.9.1 Microsoft Azure PowerShell parancsmag.  Meg kell [hello legújabb verzió letöltéséhez](https://azure.microsoft.com/downloads/) Ha egy korábbi egy telepítve van.  Csak akkor kell tooinstall ahol indításakor hello runbook Windows powershellből munkaállomáson jelen verziójában.  Nem kell tooinstall hello munkavégző számítógépen, kivéve, ha azt tervezi, hogy toostart runbookok erről a számítógépről.  Nem lehet jelenleg elindít egy runbookot egy másik runbookból a hibrid forgatókönyv-feldolgozók a mivel ehhez hello Azure Powershell toobe az Automation-fiók telepített legújabb verziójára van szükség.  hello legújabb verziója automatikusan frissíti az Azure Automationben, és automatikusan leküldött hamarosan le toohello munkavállalók.
>
>

## <a name="runbook-permissions"></a>Runbookokra vonatkozó engedélyek
A hibrid forgatókönyv-feldolgozók futó Runbookok nem használható hello azonos tooAzure erőforrások hitelesítéséhez, mert érik el az Azure-on kívüli erőforrások runbookok általában használt módszer.  hello runbook vagy biztosíthat a saját hitelesítési toolocal erőforrásokat, vagy megadhatja, hogy a RunAs fiók tooprovide az összes runbook felhasználói környezetet.

### <a name="runbook-authentication"></a>Runbook-hitelesítés
Alapértelmezés szerint runbookokat hello hello hello a helyi számítógépen helyi rendszerfiók környezetében fog futni, meg kell adniuk a saját hitelesítési tooresources, akik hozzáférhetnek a azokat.  

Használható [hitelesítő adat](http://msdn.microsoft.com/library/dn940015.aspx) és [tanúsítvány](http://msdn.microsoft.com/library/dn940013.aspx) eszközök a runbookban a parancsmagokat, amelyek lehetővé teszik toospecify hitelesítő adatokat, toodifferent erőforrások hitelesítéséhez.  hello következő példa bemutatja, hogy újraindítja a számítógépet a runbook egy részét.  Hitelesítő adatok lekéri a hitelesítő adat eszköz és hello nevének egy változóeszköz hello számítógép, és ezután hello Restart-Computer parancsmag ezeket az értékeket.

    $Cred = Get-AzureRmAutomationCredential -ResourceGroupName "ResourceGroup01" -Name "MyCredential"
    $Computer = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" -Name  "ComputerName"

    Restart-Computer -ComputerName $Computer -Credential $Cred

Kihasználhatja [InlineScript](automation-powershell-workflow.md#inlinescript), amely lehetővé teszi toorun kódblokkokat, egy másik számítógépen hello által megadott hitelesítő adatokkal [PSCredential általános paraméter](http://technet.microsoft.com/library/jj129719.aspx).

### <a name="runas-account"></a>Futtató fiók
Ahelyett, hogy a runbookok biztosít a saját hitelesítési toolocal erőforrásokat, megadhat egy **RunAs** fiókot használja a hibrid feldolgozócsoport.  Megadhat egy [hitelesítőadat-eszköz](automation-credentials.md) , amely rendelkezik, toolocal erőforrások eléréséhez, és minden runbook futtatásához ezeket a hitelesítő adatokat a hibrid forgatókönyv-feldolgozók hello csoport használatakor.  

hello felhasználónév hello hitelesítési kell hello a következő formátumok egyikében:

* tartomány\felhasználónév
* username@domain
* felhasználónév (a fiókok helyi toohello a helyi számítógép)

A következő eljárás toospecify egy futtató fiókhoz a(z) egy hibrid feldolgozócsoport hello használata:

1. Hozzon létre egy [hitelesítőadat-eszköz](automation-credentials.md) a toolocal erőforrások eléréséhez.
2. Nyissa meg a hello Automation-fiók hello Azure-portálon.
3. Jelölje be hello **hibrid dolgozó csoportok** csempére, majd válassza ki a hello csoport.
4. Válassza ki **összes beállítás** , majd **hibrid feldolgozócsoport beállításai**.
5. Változás **futtató** a **alapértelmezett** túl**egyéni**.
6. Válassza ki a hello hitelesítő adat, és kattintson **mentése**.

### <a name="automation-run-as-account"></a>Automatizálási futtató fiók
Az Azure-erőforrások telepítéséhez az automatizált felépítési folyamat részeként lehet szükség az access tooon helyi rendszerek toosupport feladat vagy további lépés a központi telepítési sorrendben.  Azure-ban hello Futtatás mint fiók toosupport hitelesítést, tanúsítványra van szükség tooinstall hello Futtatás mint fiók.  

a következő PowerShell-forgatókönyv hello *Export-RunAsCertificateToHybridWorker*, hello futtató tanúsítvány exportálja az Azure Automation-fiók és letölti és a helyi számítógép tanúsítványtárolójába hello importálja a Hibridfeldolgozó csatlakoztatott toohello ugyanazt a fiókot.  Ha a lépés befejeződött, ellenőrzi hello munkavégző sikeresen hitelesítheti tooAzure hello Futtatás mint fiók használatával.

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

Mentés hello *Export-RunAsCertificateToHybridWorker* runbook tooyour számítógép egy `.ps1` bővítmény.  Importálja a fájlt az Automation-fiók és hello forgatókönyv hello hello változó értékének módosítása szerkesztése `$Password` a saját jelszavát.  Közzététele, és futtassa a hello runbook célzó hello hibrid feldolgozócsoport futtatásához, és hitelesíteni runbookokat hello Futtatás mint fiók használatával.  hello feladat adatfolyam jelentések hello kísérlet tooimport hello tanúsítvány hello helyi számítógép tárolójában, és attól függően, hogy hány Automation-fiókok vannak definiálva az előfizetés és a sikeres hitelesítés esetén több sort tartalmazó követi.  

## <a name="troubleshooting-runbooks-on-hybrid-runbook-worker"></a>A hibrid forgatókönyv-feldolgozó hibaelhárítási runbookok
Naplók minden hibridfeldolgozó C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes, helyileg tárolja.  Hibridfeldolgozó is rögzíti a hibák és események hello Windows eseménynaplóban **alkalmazások és szolgáltatások Logs\Microsoft-SMA\Operational**.  Csoporttal kapcsolatos események toorunbooks hello munkavégző végrehajtása túl írt**alkalmazások és szolgáltatások Logs\Microsoft-Automation\Operational**.  Hello **Microsoft-SMA** napló számos további események kapcsolódó toohello runbook feladat megnyomott toohello munkavégző és hello feldolgozása hello runbook tartalmazza.  Hello közben **Microsoft-automatizálás** Eseménynapló nincs sok eseményt hello hibaelhárításához a runbook végrehajtása védelmével adatokkal, hello runbook-feladat eredményének hello legalább talál.  

[Runbook kimenet és üzenetek](automation-runbook-output-and-messages.md) Automation hibrid feldolgozók csak a például a runbook-feladatok hello felhőben futtatható tooAzure küldése.  Hello részletes is engedélyezheti, és folyamatban lévő adatfolyamok hello megegyezik más runbookokat ugyanúgy.  

Ha a runbookok nem sikeres befejezését, és hello feladat összegzése állapotot jelez, **felfüggesztett**, tekintse át a hibaelhárítási cikk hello [hibrid forgatókönyv-feldolgozó: A runbook-feladatok leállítja állapottal Felfüggesztve](automation-troubleshooting-hybrid-runbook-worker.md#a-runbook-job-terminates-with-a-status-of-suspended).   

## <a name="next-steps"></a>Következő lépések
* További információ az hello többféle módszerrel is lehet a runbookban használt toostart toolearn lásd [Runbook elindítása az Azure Automationben](automation-starting-a-runbook.md).  
* Tekintse meg a toounderstand hello különböző eljárásokkal a PowerShell és a PowerShell-munkafolyamati forgatókönyvek az Azure Automationben hello szöveges-szerkesztő segítségével használata a [az Azure Automationben Runbook szerkesztése](automation-edit-textual-runbook.md)
