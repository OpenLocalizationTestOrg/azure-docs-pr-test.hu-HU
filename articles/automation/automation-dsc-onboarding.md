---
title: "Azure Automation DSC általi kezelésre aaaOnboarding gépek |} Microsoft Docs"
description: "Hogyan toosetup gépek-kezelés az Azure Automation DSC"
services: automation
documentationcenter: dev-center-name
author: eslesar
manager: carmonm
ms.assetid: da13e1f5-2a1c-443b-8e3b-9f0d6f9e4810
ms.service: automation
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: TBD
ms.date: 12/13/2016
ms.author: eslesar
ms.openlocfilehash: ef15801fec2ffea4ba62dcba2fbe9af09268e424
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="onboarding-machines-for-management-by-azure-automation-dsc"></a>Azure Automation DSC általi kezelésre bevezetési gépek

## <a name="why-manage-machines-with-azure-automation-dsc"></a>Miért kezelése az Azure Automation DSC Szolgáltatásban gépek?

Például [PowerShell célállapot-konfiguráció](https://technet.microsoft.com/library/dn249912.aspx), Azure Automation célállapot-konfiguráció egy olyan egyszerű, mégis hatékony, konfigurációs felügyeleti szolgáltatás a DSC-csomópontok (fizikai és virtuális gépek) számára a felhőalapú vagy helyszíni adatközpontban. Lehetővé teszi méretezhetőség több ezer gép között gyors és egyszerű központi, biztonságos helyről. Könnyen előkészítésére gépek is előfordulhatnak, rendelje hozzá őket deklaratív konfigurációk és nézet jelentéseken minden számítógép megadott megfelelőségi szükséges toohello állapota. hello Azure Automation DSC alkalmazáskezelési réteg tooDSC milyen hello Azure Automation felügyeleti réteg van tooPowerShell parancsfájlok. Más szóval hello Azure Automation segít ugyanúgy felügyelhetők PowerShell-parancsfájlok, emellett segítséget nyújt a DSC-konfigurációk kezelése. További információ az Azure Automation DSC használatának előnyei hello toolearn lásd [Azure Automation DSC – áttekintés](automation-dsc-overview.md).

Az Azure Automation DSC használt toomanage gépek számos lehet:

* Az Azure virtuális gépek (klasszikus)
* Azure virtuális gépek
* Amazon Web Services (AWS) virtuális gépek
* A Windows fizikai vagy virtuális gépek a helyszíni vagy felhőben működő Azure/AWS eltérő
* Fizikai vagy virtuális Linux számítógépek, a helyszíni, az Azure-ban, vagy eltérő Azure felhőben

Emellett ha még nem kész toomanage gépkonfiguráció hello felhőből, Azure Automation DSC is használható csak a jelentés-végpontként. Ez lehetővé teszi tooset (leküldéses) szükségeskonfiguráció helyszíni DSC keresztül, és gazdag jelentéskészítési részleteinek megtekintése hello csomópont való megfelelés szükséges állapotát az Azure Automationben.

a következő szakaszok hello helyzeteit vázolják fel, hogyan zajlik bevezetésében minden gép tooAzure Automation DSC típusú.

## <a name="azure-virtual-machines-classic"></a>Az Azure virtuális gépek (klasszikus)

Az Azure Automation DSC Szolgáltatásban könnyen bevezetni az Azure virtuális gépek (klasszikus) konfigurációs Management hello Azure-portálon, vagy a PowerShell használatával is. Hello technikai részletek alatt, és a virtuális gép hello tooremote rendelkező rendszergazda nélkül a hello Azure VM célállapot-konfiguráció bővítmény hello VM regisztrálja az Azure Automation DSC Szolgáltatásban. Mivel hello Azure VM célállapot-konfiguráció bővítmény előrehaladásával lépéseket tootrack aszinkron módon futtatja, és a hibakeresést, igénybe hello [ **hibaelhárítási Azure virtuális gép bevezetési** ](#troubleshooting-azure-virtual-machine-onboarding)az alábbi szakasz.

### <a name="azure-portal"></a>Azure Portal

A hello [Azure-portálon](http://portal.azure.com/), kattintson a **Tallózás** -> **virtuális gépek (klasszikus)**. Válassza ki a Windows virtuális gép kívánt tooonboard hello. Hello virtuális gép irányítópult paneljén kattintson **összes beállítás** -> **bővítmények** -> **Hozzáadás**  ->   **Azure Automation DSC** -> **létrehozása**. Adja meg a hello [PowerShell DSC helyi Configuration Manager értékek](https://msdn.microsoft.com/powershell/dsc/metaconfig4) a használati eset, az Automation-fiók regisztrációs kulcsot és regisztrációs URL-címet, és nem kötelező a csomópont konfigurációs tooassign toohello virtuális gép szükséges.

![](./media/automation-dsc-onboarding/DSC_Onboarding_1.png)

toofind hello regisztrációs URL-cím és a kulcs hello automatizálási fiókot tooonboard hello gép, lásd: hello [ **regisztrációs biztonságos** ](#secure-registration) az alábbi szakasz.

### <a name="powershell"></a>PowerShell

```powershell
# log in tooboth Azure Service Management and Azure Resource Manager
Add-AzureAccount
Add-AzureRmAccount

# fill in correct values for your VM/Automation account here
$VMName = ""
$ServiceName = ""
$AutomationAccountName = ""
$AutomationAccountResourceGroup = ""

# fill in hello name of a Node Configuration in Azure Automation DSC, for this VM tooconform to
$NodeConfigName = ""

# get Azure Automation DSC registration info
$Account = Get-AzureRmAutomationAccount -ResourceGroupName $AutomationAccountResourceGroup -Name $AutomationAccountName
$RegistrationInfo = $Account | Get-AzureRmAutomationRegistrationInfo

# use hello DSC extension tooonboard hello VM for management with Azure Automation DSC
$VM = Get-AzureVM -Name $VMName -ServiceName $ServiceName

$PublicConfiguration = ConvertTo-Json -Depth 8 @{
    SasToken = ""
    ModulesUrl = "https://eus2oaasibizamarketprod1.blob.core.windows.net/automationdscpreview/RegistrationMetaConfigV2.zip"
    ConfigurationFunction = "RegistrationMetaConfigV2.ps1\RegistrationMetaConfigV2"

# update these PowerShell DSC Local Configuration Manager defaults if they do not match your use case.
# See https://technet.microsoft.com/library/dn249922.aspx?f=255&MSPPError=-2147217396 for more details
    Properties = @{
    RegistrationKey = @{
        UserName = 'notused'
        Password = 'PrivateSettingsRef:RegistrationKey'
    }
    RegistrationUrl = $RegistrationInfo.Endpoint
    NodeConfigurationName = $NodeConfigName
    ConfigurationMode = "ApplyAndMonitor"
    ConfigurationModeFrequencyMins = 15
    RefreshFrequencyMins = 30
    RebootNodeIfNeeded = $False
    ActionAfterReboot = "ContinueConfiguration"
    AllowModuleOverwrite = $False
    }
}

$PrivateConfiguration = ConvertTo-Json -Depth 8 @{
    Items = @{
        RegistrationKey = $RegistrationInfo.PrimaryKey
    }
}

$VM = Set-AzureVMExtension `
    -VM $vm `
    -Publisher Microsoft.Powershell `
    -ExtensionName DSC `
    -Version 2.19 `
    -PublicConfiguration $PublicConfiguration `
    -PrivateConfiguration $PrivateConfiguration `
    -ForceUpdate

$VM | Update-AzureVM
```

## <a name="azure-virtual-machines"></a>Azure virtuális gépek

Az Azure Automation DSC lehetővé teszi a konfigurációkezelésre, könnyen bevezetni az Azure virtuális gépek hello Azure-portál, Azure Resource Manager-sablonok, vagy a PowerShell használatával. Hello technikai részletek alatt, és a virtuális gép hello tooremote rendelkező rendszergazda nélkül a hello Azure VM célállapot-konfiguráció bővítmény hello VM regisztrálja az Azure Automation DSC Szolgáltatásban. Mivel hello Azure VM célállapot-konfiguráció bővítmény előrehaladásával lépéseket tootrack aszinkron módon futtatja, és a hibakeresést, igénybe hello [ **hibaelhárítási Azure virtuális gép bevezetési** ](#troubleshooting-azure-virtual-machine-onboarding)az alábbi szakasz.

### <a name="azure-portal"></a>Azure Portal

A hello [Azure-portálon](https://portal.azure.com/), keresse meg a kívánt virtuális gépek tooonboard toohello Azure Automation-fiók. Az automatizálási fiók irányítópultján hello kattintson **DSC-csomópontok** -> **adja hozzá az Azure virtuális gép**.

A **válassza ki a virtuális gépek tooonboard**, válasszon ki egy vagy több Azure virtuális gépek tooonboard.

![](./media/automation-dsc-onboarding/DSC_Onboarding_2.png)

A **regisztrációs adatok konfigurálása**, adja meg a hello [PowerShell DSC helyi Configuration Manager értékek](https://msdn.microsoft.com/powershell/dsc/metaconfig4) a használati eset, és szükség esetén a csomópont konfigurációs tooassign toohello virtuális gép szükséges.

![](./media/automation-dsc-onboarding/DSC_Onboarding_3.png)

### <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sablonok

Azure virtuális gépeken is telepíthető és előkészítve tooAzure Automation DSC Azure Resource Manager-sablonok használatával. Lásd: [konfigurálja a virtuális gépről a DSC-bővítményt és Azure Automation DSC](https://azure.microsoft.com/documentation/templates/dsc-extension-azure-automation-pullserver/) egy példa sablon, hogy egy meglévő virtuális gép tooAzure Automation DSC onboards. toofind hello regisztrációs kulcs és a regisztrációs URL-cím szükséges bemeneti sablonban szereplő, lásd: hello [ **regisztrációs biztonságos** ](#secure-registration) az alábbi szakasz.

### <a name="powershell"></a>PowerShell

Hello [Register-AzureRmAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode) parancsmag használt tooonboard virtuális gépek hello Azure-portálon keresztül is lehet.

## <a name="amazon-web-services-aws-virtual-machines"></a>Amazon Web Services (AWS) virtuális gépek

Azure Automation DSC hello AWS DSC eszközkészlet használatával által konfigurációkezelés könnyen előkészítésére Amazon Web Services virtuális gépek is. Hello eszközkészlet kapcsolatos részletesebb [Itt](https://blogs.msdn.microsoft.com/powershell/2016/04/20/aws-dsc-toolkit/).

## <a name="physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azureaws"></a>A Windows fizikai vagy virtuális gépek a helyszíni vagy felhőben működő Azure/AWS eltérő

A helyi Windows-alapú gépek és a Windows-alapú gépek az-Azure felhők (például az Amazon Web Services) is lehet előkészítve tooAzure Automation DSC, mindaddig, amíg kimenő hozzáférést toohello rendelkeznek internet-néhány egyszerű lépésben:

1. Győződjön meg arról, hogy hello legújabb verziójának [WMF 5](http://aka.ms/wmf5latest) telepítése hello gépeken tooonboard tooAzure Automation DSC szeretné.
2. Hello szakasz útmutatásait követve [ **generálása DSC metaconfigurations** ](#generating-dsc-metaconfigurations) toogenerate alatt hello tartalmazó mappa DSC metaconfigurations szükséges.
3. Hello PowerShell DSC metakonfigurációját toohello gépek tooonboard kívánt távolról alkalmazni. **Ez a parancs fut hello gépnek rendelkeznie kell hello legújabb verziójának [WMF 5](http://aka.ms/wmf5latest) telepített**:

    ```powershell
    Set-DscLocalConfigurationManager -Path C:\Users\joe\Desktop\DscMetaConfigs -ComputerName MyServer1, MyServer2
    ```

4. Hello PowerShell DSC metaconfigurations távolról nem alkalmazható, ha hello metaconfigurations mappába másolása, minden gép tooonboard 2. lépés. Majd meghívják a **Set-DscLocalConfigurationManager** helyileg az egyes gépek tooonboard.
5. Hello Azure-portál vagy parancsmagok, ellenőrizze, hogy a regisztrált hello gépek tooonboard mostantól megjelennek a DSC-csomópontként az Azure Automation-fiók használatával.

## <a name="physicalvirtual-linux-machines-on-premises-in-azure-or-in-a-cloud-other-than-azure"></a>Fizikai vagy virtuális Linux számítógépek, a helyszíni, az Azure-ban, vagy eltérő Azure felhőben

Helyszíni Linux számítógépek, az Azure Linux-gépek és a Linux-gépek-Azure felhőben is lehet előkészítve tooAzure Automation DSC, mindaddig, amíg azok rendelkeznek-e kimenő hozzáférést toohello internet-néhány egyszerű lépésben:

1. Győződjön meg arról, hogy hello legújabb verziójának [PowerShell célállapot-konfiguráció Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) telepítve van hello gépeken tooonboard tooAzure Automation DSC szeretné.
2. Ha hello [PowerShell DSC helyi Configuration Manager alapértelmezett](https://msdn.microsoft.com/powershell/dsc/metaconfig4) felel meg a használati eset, és azt szeretné, hogy a számítógépek tooonboard, például, hogy azok **mindkét** lekérés a és tooAzure Automation DSC jelentést:

   + Minden egyes Linux számítógép tooonboard tooAzure Automation DSC Register.py tooonboard hello alapértelmezés szerint használt érték a PowerShell DSC helyi Configuration Manager használatával kell használni:

     `/opt/microsoft/dsc/Scripts/Register.py <Automation account registration key> <Automation account registration URL>`

   + toofind hello regisztrációs kulcs és a regisztrációs URL-címe az Automation-fiók, lásd: hello [ **regisztrációs biztonságos** ](#secure-registration) az alábbi szakasz.

     Ha hello PowerShell DSC helyi Configuration Manager alapértelmezett **tegye** **nem** felel meg a használati eset, vagy a kívánt tooonboard gépek úgy, hogy csak tooAzure Automation DSC jelentést, hanem kéri le a konfiguráció vagy a PowerShell-modulok, akkor kövesse a lépéseket 3-6. Egyéb esetben folytassa a műveletet közvetlenül toostep 6.

3. Hello hello útmutatásait követve [ **generálása DSC metaconfigurations** ](#generating-dsc-metaconfigurations) szakasz alatt toogenerate szükséges hello DSC metaconfigurations tartalmazó mappához.
4. Hello PowerShell DSC metakonfigurációját toohello gépek tooonboard kívánt távolról vonatkoznak:

    ```powershell
    $SecurePass = ConvertTo-SecureString -String "<root password>" -AsPlainText -Force
    $Cred = New-Object System.Management.Automation.PSCredential "root", $SecurePass
    $Opt = New-CimSessionOption -UseSsl -SkipCACheck -SkipCNCheck -SkipRevocationCheck

    # need a CimSession for each Linux machine tooonboard

    $Session = New-CimSession -Credential $Cred -ComputerName <your Linux machine> -Port 5986 -Authentication basic -SessionOption $Opt

    Set-DscLocalConfigurationManager -CimSession $Session -Path C:\Users\joe\Desktop\DscMetaConfigs
    ```

Ez a parancs fut hello gépnek rendelkeznie kell hello legújabb verziójának [WMF 5](http://aka.ms/wmf5latest) telepítve.

1. Ha nem alkalmazhat hello PowerShell DSC metaconfigurations távolról, minden egyes Linux-gép tooonboard átmásolja a hello metakonfigurációját megfelelő toothat gép hello mappába települ hello Linux gépre 5. lépés. Majd meghívják a `SetDscLocalConfigurationManager.py` helyileg minden egyes Linux gépen szeretné tooonboard tooAzure Automation DSC:

   `/opt/microsoft/dsc/Scripts/SetDscLocalConfigurationManager.py -configurationmof <path toometaconfiguration file>`

2. Hello Azure-portál vagy parancsmagok, ellenőrizze, hogy a regisztrált hello gépek tooonboard mostantól megjelennek a DSC-csomópontként az Azure Automation-fiók használatával.

## <a name="generating-dsc-metaconfigurations"></a>A DSC metaconfigurations létrehozása

toogenerically bevezetésében bármely gépre tooAzure Automation DSC, egy [DSC metakonfigurációját](https://msdn.microsoft.com/en-us/powershell/dsc/metaconfig) lehet, hogy a létre, amikor egy alkalmazott, hello DSC ügynök közli a hello gép toopull származó és/vagy tooAzure Automation DSC jelentést. Az Azure Automation DSC DSC metaconfigurations a PowerShell DSC-konfiguráció, vagy hello Azure Automation PowerShell-parancsmagok használatával hozható létre.

> [!NOTE]
> A DSC metaconfigurations hello szükséges titkok tooonboard egy gép tooan management Automation-fiók tartalmaz. Győződjön meg arról, hogy tooproperly védelme bármely DSC metaconfigurations hoz létre, vagy után törölje őket.

### <a name="using-a-dsc-configuration"></a>A DSC-konfiguráció használata

1. Nyissa meg rendszergazdaként a PowerShell ISE hello a helyi környezet valamelyik számítógépén. hello gépnek rendelkeznie kell hello legújabb verziójának [WMF 5](http://aka.ms/wmf5latest) telepítve.
2. A következő parancsfájl helyi hello másolja. A parancsfájl tartalmazza a PowerShell DSC-konfiguráció metaconfigurations, és a parancs tookick hello metakonfigurációját létrehozása ki lehet létrehozni.

    ```powershell
    # hello DSC configuration that will generate metaconfigurations
    [DscLocalConfigurationManager()]
    Configuration DscMetaConfigs
    {

        param
        (
            [Parameter(Mandatory=$True)]
            [String]$RegistrationUrl,

            [Parameter(Mandatory=$True)]
            [String]$RegistrationKey,

            [Parameter(Mandatory=$True)]
            [String[]]$ComputerName,

            [Int]$RefreshFrequencyMins = 30,

            [Int]$ConfigurationModeFrequencyMins = 15,

            [String]$ConfigurationMode = "ApplyAndMonitor",

            [String]$NodeConfigurationName,

            [Boolean]$RebootNodeIfNeeded= $False,

            [String]$ActionAfterReboot = "ContinueConfiguration",

            [Boolean]$AllowModuleOverwrite = $False,

            [Boolean]$ReportOnly
        )

        if(!$NodeConfigurationName -or $NodeConfigurationName -eq "")
        {
            $ConfigurationNames = $null
        }
        else
        {
            $ConfigurationNames = @($NodeConfigurationName)
        }

        if($ReportOnly)
        {
        $RefreshMode = "PUSH"
        }
        else
        {
        $RefreshMode = "PULL"
        }

        Node $ComputerName
        {

            Settings
            {
                RefreshFrequencyMins = $RefreshFrequencyMins
                RefreshMode = $RefreshMode
                ConfigurationMode = $ConfigurationMode
                AllowModuleOverwrite = $AllowModuleOverwrite
                RebootNodeIfNeeded = $RebootNodeIfNeeded
                ActionAfterReboot = $ActionAfterReboot
                ConfigurationModeFrequencyMins = $ConfigurationModeFrequencyMins
            }

            if(!$ReportOnly)
            {
            ConfigurationRepositoryWeb AzureAutomationDSC
                {
                    ServerUrl = $RegistrationUrl
                    RegistrationKey = $RegistrationKey
                    ConfigurationNames = $ConfigurationNames
                }

                ResourceRepositoryWeb AzureAutomationDSC
                {
                ServerUrl = $RegistrationUrl
                RegistrationKey = $RegistrationKey
                }
            }

            ReportServerWeb AzureAutomationDSC
            {
                ServerUrl = $RegistrationUrl
                RegistrationKey = $RegistrationKey
            }
        }
    }

    # Create hello metaconfigurations
    # TODO: edit hello below as needed for your use case
    $Params = @{
        RegistrationUrl = '<fill me in>';
        RegistrationKey = '<fill me in>';
        ComputerName = @('<some VM tooonboard>', '<some other VM tooonboard>');
        NodeConfigurationName = 'SimpleConfig.webserver';
        RefreshFrequencyMins = 30;
        ConfigurationModeFrequencyMins = 15;
        RebootNodeIfNeeded = $False;
        AllowModuleOverwrite = $False;
        ConfigurationMode = 'ApplyAndMonitor';
        ActionAfterReboot = 'ContinueConfiguration';
        ReportOnly = $False;  # Set too$True toohave machines only report tooAA DSC but not pull from it
    }

    # Use PowerShell splatting toopass parameters toohello DSC configuration being invoked
    # For more info about splatting, run: Get-Help -Name about_Splatting
    DscMetaConfigs @Params
    ```

3. Töltse ki hello regisztrációs kulcsot és az URL-címet az Automation-fiók, valamint a hello gépek tooonboard hello nevét. Más paraméterek opcionálisak. toofind hello regisztrációs kulcs és a regisztrációs URL-címe az Automation-fiók, lásd: hello [ **regisztrációs biztonságos** ](#secure-registration) az alábbi szakasz.
4. Ha szeretné, hogy hello gépek tooreport DSC állapot információk tooAzure Automation DSC, de nem lekérni a konfigurációját vagy a PowerShell-modulok, állítsa be a hello **ReportOnly** paraméter tootrue.
5. Hello parancsprogrammal. Most már rendelkeznie kell egy nevű mappát **DscMetaConfigs** a munkakönyvtárba tartalmazó hello PowerShell DSC metaconfigurations a hello gépek tooonboard (rendszergazdaként):

    ```powershell
    Set-DscLocalConfigurationManager -Path ./DscMetaConfigs
    ```

### <a name="using-hello-azure-automation-cmdlets"></a>Hello Azure Automation parancsmagokkal

Ha hello PowerShell DSC helyi Configuration Manager alapértelmezett felel meg a használati eset, és gépeihez tooonboard úgy, hogy azok mind a lekéréses, és jelentést tooAzure Automation DSC, hello Azure Automation parancsmagokkal hello DSC generálása egy egyszerűsített lehetőséget nyújt a a metaconfigurations szükséges:

1. Hello PowerShell-konzolt vagy a PowerShell ISE rendszergazdaként nyissa meg a helyi környezet valamelyik számítógépén.
2. Csatlakozás tooAzure erőforrás-kezelő használatával **Add-AzureRmAccount**
3. Hello PowerShell DSC metaconfigurations tölthető azt szeretné, hogy a hello Automation tooonboard hello gépek toowhich tooonboard csomópontok kívánt fiókot:

    ```powershell
    # Define hello parameters for Get-AzureRmAutomationDscOnboardingMetaconfig using PowerShell Splatting
    $Params = @{

        ResourceGroupName = 'ContosoResources'; # hello name of hello ARM Resource Group that contains your Azure Automation Account
        AutomationAccountName = 'ContosoAutomation'; # hello name of hello Azure Automation Account where you want a node on-boarded to
        ComputerName = @('web01', 'web02', 'sql01'); # hello names of hello computers that hello meta configuration will be generated for
        OutputFolder = "$env:UserProfile\Desktop\";
    }
    # Use PowerShell splatting toopass parameters toohello Azure Automation cmdlet being invoked
    # For more info about splatting, run: Get-Help -Name about_Splatting
    Get-AzureRmAutomationDscOnboardingMetaconfig @Params
    ```
    
4. Most már rendelkeznie kell egy nevű mappát ***DscMetaConfigs***, tartalmazó hello PowerShell DSC metaconfigurations a hello gépek tooonboard (rendszergazdaként):
    
    ```powershell
    Set-DscLocalConfigurationManager -Path $env:UserProfile\Desktop\DscMetaConfigs
    ```

## <a name="secure-registration"></a>Biztonságos regisztrációs

Gépek biztonságosan előkészítésére tooan Azure Automation-fiók hello WMF 5 DSC regisztrációs protokoll, amely lehetővé teszi egy DSC csomópont tooauthenticate tooa PowerShell DSC V2 lekéréses vagy a Reporting server (az Azure Automation DSC) keresztül is. hello csomópont toohello kiszolgáló regisztrálása a **regisztrációs URL-cím**, hitelesítő használatával egy **regisztrációs kulcs**. Alatt a regisztrációt hello DSC-csomópont és DSC lekérési/jelentéskészítési kiszolgáló egyezteti a csomópont toouse hitelesítési toohello server utáni regisztrálási egyedi tanúsítványt. Ez a folyamat megakadályozza, hogy a előkészítve csomópontok megszemélyesít egy másikra, például ha egy csomópont biztonsága sérül, és rosszindulatúan viselkedik. A regisztrációt követően hello regisztrációs kulcs nem használatos a hitelesítés újra, és hello csomópont törlődik.

A hello hello DSC regisztrációs protokoll szükséges hello adatokat kaphat **kulcsok kezelése** panel hello Azure betekintő portálon. Nyissa meg ezen a panelen hello hello kulcs ikonra kattintva **Essentials** hello Automation-fiók panelen.

![](./media/automation-dsc-onboarding/DSC_Onboarding_4.png)

* Regisztrációs URL-címe: hello URL-cím mező hello kulcsok kezelése panelen.
* Regisztrációs kulcs hello kulcsok kezelése panel hello elsődleges elérési kulcsot vagy másodlagos elérési kulcsot. Vagy a kulcs használható.

A nagyobb biztonság érdekében bármikor helyreállíthatja a hello elsődleges és másodlagos kulcsok az Automation-fiók (a hello **kulcsok kezelése** panel) tooprevent jövőbeli csomópont regisztrációk korábbi kulcsokat.

## <a name="troubleshooting-azure-virtual-machine-onboarding"></a>Hibaelhárítás az Azure virtuális gép bevezetése

Az Azure Automation DSC könnyen előkészítésére Azure Windows virtuális gépek a konfigurációkezelésre lehetővé teszi. A hello technikai hello Azure VM célállapot-konfiguráció bővítmény azonban a virtuális gép az Azure Automation DSC Szolgáltatásban használt tooregister hello. Mivel hello Azure VM célállapot-konfiguráció bővítmény aszinkron módon futtatja, figyelemmel kíséri, a telepítés előrehaladását, és a végrehajtása hibaelhárítási fontos lehet.

> [!NOTE]
> A bevezetési egy Azure Windows virtuális gép tooAzure Automation DSC hello Azure VM célállapot-konfiguráció bővítmény használó eltarthat tooan órában hello csomópont tooshow mentése az Azure Automationben regisztrált bármely metódusát. Ez egy, a virtuális gép hello által hello Azure VM DSC-bővítményt, amely szükséges tooonboard hello VM tooAzure Automation DSC toohello Windows Management Framework 5.0 telepítése miatt.

hello Azure VM célállapot-konfiguráció bővítménye hello Azure-portálon tootroubleshoot vagy nézet hello állapotának toohello keresse meg a virtuális gép előkészítve alatt, majd kattintson -> **összes beállítás** -> **bővítmények**   ->  **DSC**. További részletekért kattintson **az állapot**.

[![](./media/automation-dsc-onboarding/DSC_Onboarding_5.png)](https://technet.microsoft.com/library/dn249912.aspx)

## <a name="certificate-expiration-and-reregistration"></a>Tanúsítványok és ismételt

A gépek DSC-csomópontként az Azure Automation DSC a regisztrálás után számos okból miért esetleg tooreregister hello az adott csomópont jövőbeli:

* A regisztrálás után minden csomóponton automatikusan egyezteti egyedi tanúsítványt a hitelesítéshez, amely egy év után lejár. Jelenleg hello PowerShell DSC regisztrációs protokoll nem automatikusan megújítani a tanúsítványokat, amikor azok hamarosan lejáró, ezért szüksége lesz egy év idő múlva tooreregister hello csomópontok. Előtt újraregisztrálása, ellenőrizze, hogy mindegyik csomópontján fut a Windows Management Framework 5.0 RTM-re. Ha a csomópont-hitelesítési tanúsítvány lejár, és hello csomópontot a rendszer nem újra regisztrálja, hello csomópont lesz az Azure Automation szolgáltatásban nem toocommunicate, és megjelöli "Unresponsive." Ismételt 90 naponta végrehajtott vagy kisebb hello tanúsítvány lejárati ideje, vagy bármikor hello tanúsítvány lejárati ideje után egy új tanúsítványt generált és a használt eredményez.
* toochange bármely [PowerShell DSC helyi Configuration Manager értékek](https://msdn.microsoft.com/powershell/dsc/metaconfig4) , amely beállított hello csomópont, például a ConfigurationMode kezdeti regisztráció során. Jelenleg a DSC-ügynök értékeiről csak módosítható újraregisztrálni keresztül. hello egyetlen kivétel ez alól a csomópont-konfiguráció hozzárendelése toohello csomópont hello – ez módosítható az Azure Automation DSC közvetlenül.

A jelen dokumentumban ismertetett regisztrált hello csomópont kezdetben hello bevezetési módszereket ugyanúgy hello ismételt hajtható végre. Egy Azure Automation DSC csomópontot toounregister előtt újraregisztrálása azt nem kell.

## <a name="related-articles"></a>Kapcsolódó cikkek

* [Azure Automation DSC – áttekintés](automation-dsc-overview.md)
* [Azure Automation DSC-parancsmagokkal](/powershell/module/azurerm.automation/#automation)
* [Azure Automation DSC – díjszabás](https://azure.microsoft.com/pricing/details/automation/)
