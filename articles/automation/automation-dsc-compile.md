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
# <a name="compiling-configurations-in-azure-automation-dsc"></a>Azure Automation DSC-konfigurációja fordítása

Az Azure Automation szolgáltatásban kétféle módon kívánt állapot konfigurációs szolgáltatása (DSC) konfigurációk állíthat össze: hello Azure-portálon, és a Windows PowerShell használatával. hello következő táblázat segítségével meghatározhatja, hogy mikor toouse módszer egyes hello jellemzők alapján:

### <a name="azure-portal"></a>Azure Portal

* Legegyszerűbb módja az interaktív felhasználói felülettel
* Űrlap tooprovide egyszerű paraméterértékek
* Könnyen nyomon követheti a feladat állapota
* Az Azure bejelentkezési hitelesített hozzáférés

### <a name="windows-powershell"></a>Windows PowerShell

* A Windows PowerShell-parancsmagokkal a parancssorból hívható
* Automatizált megoldást több lépést tartalmazhat
* Adja meg az egyszerű és összetett paraméterértékek
* Feladat-állapotok nyomon követésére
* Ügyfél szükséges toosupport PowerShell-parancsmagok
* Pass ConfigurationData
* A hitelesítő adatokat használó konfigurációk összeállítása

Miután eldöntötte, hogy egy fordítási metódusra, követésével hello vonatkozó eljárások toostart fordítás alatt.

## <a name="compiling-a-dsc-configuration-with-hello-azure-portal"></a>A DSC-konfiguráció hello Azure-portálon fordítása

1. Az Automation-fiók kattintson **a DSC-konfigurációk**.
2. Egy konfigurációs tooopen kattintson a panel.
3. Kattintson a **fordítási**.
4. Ha hello konfigurációs nincs paraméterekkel rendelkezik, akkor fogja rákérdezéses tooconfirm toocompile kíván-e azt. Ha hello konfigurációs paraméterek, hello **fordítási konfigurációs** panel nyílik meg, megadhatja a paraméterértékek. Lásd: hello [ **alapvető paraméterek** ](#basic-parameters) alatt további tájékoztatást talál a Paraméterek szakaszban.
5. Hello **fordítási feladat** panel már meg van nyitva, hogy hello fordítási feladat állapotát nyomon követheti, és hello csomópont-konfigurációt (MOF konfigurációs dokumentumok) okozta toobe hello Azure Automation DSC lekéréses kiszolgáló helyezve.

## <a name="compiling-a-dsc-configuration-with-windows-powershell"></a>A Windows PowerShell DSC-konfiguráció fordítása

Használhat [ `Start-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/start-azurermautomationdsccompilationjob) toostart fordítása a Windows PowerShell használatával. a következő példakód hello kezdődik nevű DSC-konfiguráció összeállításának **SampleConfig**.

```powershell
Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"
```

`Start-AzureRmAutomationDscCompilationJob`értéket ad vissza a fordítási feladat-objektum használható tootrack annak állapotát. Ezután használhatja a fordítási feladat objektum [ `Get-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjob) toodetermine hello állapotának hello fordítási feladat, és [ `Get-AzureRmAutomationDscCompilationJobOutput` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjoboutput) tooview az adatfolyamok (kimenet). a következő példakód hello kezdődik hello összeállításának **SampleConfig** konfigurációs, megvárja, amíg befejeződött, és megjeleníti az adatfolyamokat.

```powershell
$CompilationJob = Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"

while($CompilationJob.EndTime –eq $null -and $CompilationJob.Exception –eq $null)
{
    $CompilationJob = $CompilationJob | Get-AzureRmAutomationDscCompilationJob
    Start-Sleep -Seconds 3
}

$CompilationJob | Get-AzureRmAutomationDscCompilationJobOutput –Stream Any
```

## <a name="basic-parameters"></a>Alapvető paraméterek
A DSC-konfigurációkkal, például a különböző és tulajdonságait, works paraméterdeklarációhoz hello azonos hasonlóan az Azure Automation-forgatókönyv. Lásd: [runbook elindítása az Azure Automationben](automation-starting-a-runbook.md) runbook paraméterekkel kapcsolatos további toolearn.

hello következő példában két paramétereket **FeatureName** és **IsPresent**, toodetermine hello értékek hello tulajdonságokat **ParametersExample.sample** csomópont konfiguráció, a fordítás során jönnek létre.

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

A DSC-konfigurációk hello Azure Automation DSC-portálon, vagy az Azure PowerShell alapvető paramétert használó állíthat össze:

### <a name="portal"></a>Portál

Hello portálon adhatja meg a paraméterértékek kattintás után **fordítási**.

![helyettesítő szöveg](./media/automation-dsc-compile/DSC_compiling_1.png)

### <a name="powershell"></a>PowerShell

PowerShell-paraméterek a szükséges egy [hashtable](http://technet.microsoft.com/library/hh847780.aspx) ahol hello kulcs hello paraméter neve megegyezik, és hello érték hello paraméter értékét.

```powershell
$Parameters = @{
    "FeatureName" = "Web-Server"
    "IsPresent" = $False
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ParametersExample" -Parameters $Parameters
```

PSCredentials átadása paraméterként kapcsolatos információkért lásd: <a href="#credential-assets"> **hitelesítő eszközök** </a> alatt.

## <a name="configurationdata"></a>ConfigurationData
**ConfigurationData** teszi lehetővé a környezet konkrét beállításra a PowerShell DSC tooseparate strukturális konfigurációja. Lásd: [mappától "Mi" a "Where" a PowerShell DSC](http://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) kapcsolatos további toolearn **ConfigurationData**.

> [!NOTE]
> Használhat **ConfigurationData** Azure Automation DSC Azure PowerShell használatával, de nem hello Azure-portálon fordítása során.

hello alábbi példa a DSC-konfiguráció használ **ConfigurationData** keresztül hello **$ConfigurationData** és **$AllNodes** kulcsszavakat. Meg kell hello [ **xWebAdministration** modul](https://www.powershellgallery.com/packages/xWebAdministration/) ehhez a példához:

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

Hello DSC-konfiguráció fent PowerShell állíthat össze. PowerShell alatt hello két csomópont konfigurációk toohello Azure Automation DSC lekéréses kiszolgáló hozzáadása: **ConfigurationDataSample.MyVM1** és **ConfigurationDataSample.MyVM3**:

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

## <a name="assets"></a>Objektumok

Eszköz hivatkozások vannak hello ugyanazt az Azure Automation DSC-konfiguráció és a runbookok. Hello alábbiakat a további információkért lásd:

* [Tanúsítványok](automation-certificates.md)
* [Kapcsolatok](automation-connections.md)
* [Hitelesítő adatok](automation-credentials.md)
* [Változók](automation-variables.md)

### <a name="credential-assets"></a>Hitelesítő eszközök

Azure Automation DSC-konfigurációja is hivatkozni lehessen hitelesítő eszközök használata során **Get-AzureRmAutomationCredential**, hitelesítő eszközök is adhatók át a keresztül paraméterek, ha szükséges. Ha egy konfigurációs paramétert **PSCredential** toopass hello karakterlánc nevét egy Azure Automation szolgáltatásbeli hitelesítőadat-eszköz van szüksége adott paraméter értéke, nem pedig egy PSCredential objektumot, majd írja be. Hello háttérben hello Azure Automation szolgáltatásbeli hitelesítőadat-eszköz ezen a néven lesz lekérje és toohello konfigurációs átadott.

Hitelesítő adatok megőrzésével csomópont-konfigurációt (MOF konfigurációs dokumentumok) biztonságos szükséges hello csomópont konfigurációs MOF-fájlnak a hello hitelesítő adatok titkosításához. Azure Automation szolgáltatásbeli ebben a lépésben egy tovább tart, és titkosítja a hello teljes MOF-fájlt. Azonban jelenleg pedig kell utasítani fogja a PowerShell DSC nem hitelesítő adatok toobe outputted egyszerű szöveges csomópont konfigurációs MOF létrehozásakor, a probléma, mert a PowerShell DSC nem ismert, hogy Azure Automation fog kell Titkosított hello teljes MOF-fájlt után a egy fordítási feladat keresztül létrehozása.

Úgy írhatja elő PowerShell DSC róla egy értesítést a outputted hello jön létre a csomópont-konfiguráció MOF-ot az egyszerű szöveges hitelesítő adatokat toobe használatával [ **ConfigurationData**](#configurationdata). Át kell `PSDscAllowPlainTextPassword = $true` keresztül **ConfigurationData** minden csomópont blokk neve, amely megjelenik az hello DSC-konfiguráció és a hitelesítő adatokat használ.

hello következő példa bemutatja a DSC-konfiguráció által használt Automation szolgáltatásbeli hitelesítőadat-eszköz.

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

Hello DSC-konfiguráció fent PowerShell állíthat össze. PowerShell alatt hello két csomópont konfigurációk toohello Azure Automation DSC lekéréses kiszolgáló hozzáadása: **CredentialSample.MyVM1** és **CredentialSample.MyVM2**.

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

## <a name="importing-node-configurations"></a>Csomópont-konfigurációk importálása

Csomópont configuratons (MOF-ot), amely rendelkezik Azure-on kívüli fordított is importálhat. Egy ennek előnye, hogy a csomópont confiturations írhatók alá.
Egy aláírt csomópont-konfiguráció ellenőrzése helyileg felügyelt csomóponton hello DSC-ügynök, győződjön meg arról, hogy hello konfigurálása alkalmazott toohello csomópont alatt egy hitelesített forrásból származnak.

> [!NOTE]
> Használja az importálást aláírt beállításokat az Azure Automation-fiók rendszerbe, de Azure Automation jelenleg nem támogatja a fordítása aláírt konfigurációkat.

> [!NOTE]
> Lehet, hogy a csomópont konfigurációs fájl nem nagyobb, mint 1 MB tooallow az Azure Automation importált toobe.

Azt is megtudhatja, hogyan https://msdn.microsoft.com/en-us/powershell/wmf/5.1/dsc-improvements#how-to-sign-configuration-and-module toosign csomópont konfigurációját.

### <a name="importing-a-node-configuration-in-hello-azure-portal"></a>A csomópont-konfiguráció hello Azure-portálon a importálása

1. Az Automation-fiók kattintson **DSC-csomópontkonfigurációk**.

    ![A DSC-csomópontkonfigurációk](./media/automation-dsc-compile/node-config.png)
2. A hello **DSC-csomópontkonfigurációk** panelen kattintson a **hozzáadása egy NodeConfiguration**.
3. A hello **importálása** panelen kattintson hello mappa ikon következő toohello **csomópont konfigurációs fájl** szövegmező toobrowse a csomópont konfigurációs fájl (MOF) a helyi számítógépen.

    ![Helyi fájl kiválasztása](./media/automation-dsc-compile/import-browse.png)
4. Adjon meg egy nevet a hello **Konfigurációnévvel** szövegmező. Ezt a nevet meg kell egyeznie hello csomópont-konfiguráció lefordított hello konfigurációs hello nevét.
5. Kattintson az **OK** gombra.

### <a name="importing-a-node-configuration-with-powershell"></a>A PowerShell-lel egy csomópont-konfiguráció importálása

Használhatja a hello [Import-AzureRmAutomationDscNodeConfiguration](/powershell/module/azurerm.automation/import-azurermautomationdscnodeconfiguration) parancsmag tooimport egy csomópont-konfiguráció az automation-fiók be.

```powershell
Import-AzureRmAutomationDscNodeConfiguration -AutomationAccountName "MyAutomationAccount" -ResourceGroupName "MyResourceGroup" -ConfigurationName "MyNodeConfiguration" -Path "C:\MyConfigurations\TestVM1.mof"
```



