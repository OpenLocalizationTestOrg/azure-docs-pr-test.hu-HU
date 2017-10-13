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
# <a name="compiling-configurations-in-azure-automation-dsc"></a>Azure Automation DSC-konfigurációja fordítása

Az Azure Automation szolgáltatásban kétféle módon kívánt állapot konfigurációs szolgáltatása (DSC) konfigurációk állíthat össze: az Azure portálon, és a Windows PowerShell használatával. Az alábbi táblázat segítségével meghatározhatja, hogy az egyes jellemzők alapján módszer használatával:

### <a name="azure-portal"></a>Azure Portal

* Legegyszerűbb módja az interaktív felhasználói felülettel
* Egyszerű paraméter értékének megadására, képernyő
* Könnyen nyomon követheti a feladat állapota
* Az Azure bejelentkezési hitelesített hozzáférés

### <a name="windows-powershell"></a>Windows PowerShell

* A Windows PowerShell-parancsmagokkal a parancssorból hívható
* Automatizált megoldást több lépést tartalmazhat
* Adja meg az egyszerű és összetett paraméterértékek
* Feladat-állapotok nyomon követésére
* PowerShell-parancsmagok támogatásához szükséges ügyfél
* Pass ConfigurationData
* A hitelesítő adatokat használó konfigurációk összeállítása

Miután eldöntötte, hogy egy fordítási metódusra, követheti a vonatkozó fordítása el az alábbi eljárások.

## <a name="compiling-a-dsc-configuration-with-the-azure-portal"></a>Az Azure-portálon a DSC-konfiguráció fordítása

1. Az Automation-fiók kattintson **a DSC-konfigurációk**.
2. Kattintson a konfigurációját, és nyissa meg a panelt.
3. Kattintson a **fordítási**.
4. Ha a konfigurációs paraméterek nélkül, kérni fogja annak ellenőrzéséhez, hogy kívánja-e, hogy. Ha a konfigurációs paraméterek, a **fordítási konfigurációs** panel nyílik meg, megadhatja a paraméterértékek. Tekintse meg a [ **alapvető paraméterek** ](#basic-parameters) alatt további tájékoztatást talál a Paraméterek szakaszban.
5. A **fordítási feladat** panel meg van nyitva, így nyomon követheti a fordítási feladat állapotát, és a csomópont-konfigurációt (MOF konfigurációs dokumentumok) az Azure Automation DSC-lekéréses kiszolgáló elhelyezését okozta.

## <a name="compiling-a-dsc-configuration-with-windows-powershell"></a>A Windows PowerShell DSC-konfiguráció fordítása

Használhat [ `Start-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/start-azurermautomationdsccompilationjob) fordítása a Windows PowerShell indításához. Az alábbi példakód elindít egy nevű DSC-konfiguráció összeállításának **SampleConfig**.

```powershell
Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"
```

`Start-AzureRmAutomationDscCompilationJob`egy fordítási feladat objektumot állapotának nyomon követésére használható adja vissza. Ezután használhatja a fordítási feladat objektum [ `Get-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjob) fordítási feladat állapotának megállapításához és [ `Get-AzureRmAutomationDscCompilationJobOutput` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjoboutput) az adatfolyamok (kimenet) megtekintéséhez. Az alábbi példakód elindít összeállítása a **SampleConfig** konfigurációs, megvárja, amíg befejeződött, és megjeleníti az adatfolyamokat.

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
A paraméterdeklarációhoz a DSC-konfigurációkkal, például a különböző és tulajdonságait, azonos Azure Automation-runbook működik. Lásd: [runbook elindítása az Azure Automationben](automation-starting-a-runbook.md) további információt a runbook-paraméterek.

A következő példában két paramétereket **FeatureName** és **IsPresent**annak megállapításához, a tulajdonságok értékeit az **ParametersExample.sample** csomópont konfiguráció, a fordítás során jönnek létre.

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

Az Azure Automation DSC-portálon, vagy az Azure PowerShell alapvető paramétert használó DSC-konfigurációk állíthat össze:

### <a name="portal"></a>Portál

A portálon írhatja paraméterértékek kattintás után **fordítási**.

![helyettesítő szöveg](./media/automation-dsc-compile/DSC_compiling_1.png)

### <a name="powershell"></a>PowerShell

PowerShell-paraméterek a szükséges egy [hashtable](http://technet.microsoft.com/library/hh847780.aspx) ahol a kulcs megegyezik a paraméter nevével, és értéke egyenlő a paraméter értéke.

```powershell
$Parameters = @{
    "FeatureName" = "Web-Server"
    "IsPresent" = $False
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ParametersExample" -Parameters $Parameters
```

PSCredentials átadása paraméterként kapcsolatos információkért lásd: <a href="#credential-assets"> **hitelesítő eszközök** </a> alatt.

## <a name="configurationdata"></a>ConfigurationData
**ConfigurationData** lehetővé teszi, hogy a környezet konkrét beállításra a PowerShell DSC szerkezeti konfigurációja különítheti el. Lásd: [mappától "Mi" a "Where" a PowerShell DSC](http://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) kapcsolatos további **ConfigurationData**.

> [!NOTE]
> Használhat **ConfigurationData** Azure Automation DSC Azure PowerShell használatával, de nem az Azure-portálon fordítása során.

Használja az alábbi példa a DSC-konfiguráció **ConfigurationData** keresztül a **$ConfigurationData** és **$AllNodes** kulcsszavakat. Biztosítani kell a [ **xWebAdministration** modul](https://www.powershellgallery.com/packages/xWebAdministration/) ehhez a példához:

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

A DSC-konfiguráció fent PowerShell állíthat össze. Az alábbi PowerShell ad hozzá két csomópont-konfigurációt az Azure Automation DSC-lekéréses kiszolgáló: **ConfigurationDataSample.MyVM1** és **ConfigurationDataSample.MyVM3**:

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

Eszköz hivatkozások megegyeznek az Azure Automation DSC-konfiguráció és a runbookok. További információt a következő témakörben talál:

* [Tanúsítványok](automation-certificates.md)
* [Kapcsolatok](automation-connections.md)
* [Hitelesítő adatok](automation-credentials.md)
* [Változók](automation-variables.md)

### <a name="credential-assets"></a>Hitelesítő eszközök

Azure Automation DSC-konfigurációja is hivatkozni lehessen hitelesítő eszközök használata során **Get-AzureRmAutomationCredential**, hitelesítő eszközök is adhatók át a keresztül paraméterek, ha szükséges. Ha egy konfigurációs paramétert **PSCredential** kell egy Azure Automation szolgáltatásbeli hitelesítőadat-eszköz karakterlánc nevét átadni adott paraméter értéke, nem pedig egy PSCredential objektumot, majd írja be. A háttérben az Azure Automation szolgáltatásbeli hitelesítőadat-eszköz ezen a néven lesz lekérje és kapott a konfiguráció.

Hitelesítő adatok megőrzésével csomópont-konfigurációt (MOF konfigurációs dokumentumok) biztonságos kell titkosítani a hitelesítő adatokat, a csomópont konfigurációs MOF-fájlban. Azure Automation szolgáltatásbeli ebben a lépésben egy tovább tart, és titkosítja a teljes MOF-fájlt. Azonban jelenleg pedig kell utasítani fogja a PowerShell DSC nem probléma, a csomópont konfigurációs MOF létrehozása során gyártandó egyszerű szöveges hitelesítő adatokat, mert a PowerShell DSC nem ismert, hogy Azure Automation fog kell titkosítása a teljes MOF-fájlt a generációját után keresztül egy fordítási feladat.

Megadható, hogy a PowerShell DSC, amelyek a hitelesítő adatokat egyszerű szöveges a létrehozott csomópont-konfiguráció MOF-ot a gyártandó kapcsolatát használatával [ **ConfigurationData**](#configurationdata). Át kell `PSDscAllowPlainTextPassword = $true` keresztül **ConfigurationData** minden csomópont blokk neve, amely megjelenik a DSC-konfiguráció és a hitelesítő adatokat használ.

A következő példa bemutatja a DSC-konfiguráció által használt Automation szolgáltatásbeli hitelesítőadat-eszköz.

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

A DSC-konfiguráció fent PowerShell állíthat össze. Az alábbi PowerShell ad hozzá két csomópont-konfigurációt az Azure Automation DSC-lekéréses kiszolgáló: **CredentialSample.MyVM1** és **CredentialSample.MyVM2**.

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
Egy aláírt csomópont-konfiguráció ellenőrzése helyileg felügyelt csomóponton a DSC-ügynök, győződjön meg arról, hogy a konfiguráció alkalmazták a csomópont egy hitelesített forrásból származik-e.

> [!NOTE]
> Használja az importálást aláírt beállításokat az Azure Automation-fiók rendszerbe, de Azure Automation jelenleg nem támogatja a fordítása aláírt konfigurációkat.

> [!NOTE]
> A csomópont konfigurációs fájl nem engedélyezi az Azure Automation importáláshoz 1 MB-nál nagyobb lehet.

Megismerheti https://msdn.microsoft.com/en-us/powershell/wmf/5.1/dsc-improvements#how-to-sign-configuration-and-module csomópont konfigurációját aláírásához.

### <a name="importing-a-node-configuration-in-the-azure-portal"></a>Az Azure-portálon a csomópont-konfiguráció importálása

1. Az Automation-fiók kattintson **DSC-csomópontkonfigurációk**.

    ![A DSC-csomópontkonfigurációk](./media/automation-dsc-compile/node-config.png)
2. Az a **DSC-csomópontkonfigurációk** panelen kattintson a **hozzáadása egy NodeConfiguration**.
3. Az a **importálási** panelen kattintson a Tovább gombra a mappa ikonra a **csomópont konfigurációs fájl** szövegmező a csomópont konfigurációs fájl (MOF) a helyi számítógépen.

    ![Helyi fájl kiválasztása](./media/automation-dsc-compile/import-browse.png)
4. Írjon be egy nevet a **Konfigurációnévvel** szövegmező. Ezt a nevet meg kell egyeznie a nevét, amelyből a csomópont-konfiguráció lett lefordítva, a konfiguráció.
5. Kattintson az **OK** gombra.

### <a name="importing-a-node-configuration-with-powershell"></a>A PowerShell-lel egy csomópont-konfiguráció importálása

Használhatja a [Import-AzureRmAutomationDscNodeConfiguration](/powershell/module/azurerm.automation/import-azurermautomationdscnodeconfiguration) parancsmagot, hogy a csomópont-konfiguráció importálása az automation-fiók.

```powershell
Import-AzureRmAutomationDscNodeConfiguration -AutomationAccountName "MyAutomationAccount" -ResourceGroupName "MyResourceGroup" -ConfigurationName "MyNodeConfiguration" -Path "C:\MyConfigurations\TestVM1.mof"
```



