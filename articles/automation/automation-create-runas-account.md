---
title: "Azure Automation futtató fiókok aaaCreate |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooupdate az automatizálási fiókot és futtató fiókok létrehozása a PowerShell-lel, vagy hello portálról."
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
ms.date: 07/27/2017
ms.author: magoedte
ms.openlocfilehash: 94eb54fa0b518056a726d17146c63411e248273b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="update-your-automation-account-authentication-with-run-as-accounts"></a>Automation-fiók hitelesítésének frissítése futtató fiókokkal 
Hello portálról a meglévő Automation-fiók frissítéséhez, vagy a PowerShell segítségével, ha:

* Automation-fiók létrehozása, de elutasítása toocreate hello futtató fiókot.
* Már használja egy automatizálási fiókot toomanage Resource Managerhez tartozó erőforrások és tooupdate hello tooinclude hello futtató fiókok kívánt runbook-hitelesítéshez.
* Az automatizálási fiók toomanage hagyományos erőforrások már használja, és azt szeretné, hogy tooupdate azt toouse hello klasszikus futtató fiókot új fiók létrehozása és a forgatókönyve és eszköze tooit áttelepítése helyett.   
* Érdemes toocreate egy olyan futtató és a klasszikus futtató fiókot a vállalati hitelesítésszolgáltató (CA) által kiadott tanúsítvánnyal.

## <a name="prerequisites"></a>Előfeltételek

* hello parancsfájl futtatása csak Windows 10 és Windows Server 2016 az Azure Resource Manager modulok 3.0.0 és későbbi lehet. A korábbi Windows-verziók esetében nem támogatott.
* Az Azure PowerShell 1.0-s és újabb verziói. További információk hello PowerShell 1.0-s kiadásról: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs).
* Automation-fiók, hello hello értékként hivatkozott *– AutomationAccountName* és *- ApplicationDisplayName* paramétereket a PowerShell-parancsfájl a következő hello.

tooget hello értékei *SubscriptionID*, *ResourceGroup*, és *AutomationAccountName*, amely hello parancsfájl kötelező paraméterek tartoznak, a következő hello:

1. A hello Azure-portálon, válassza ki az Automation-fiókban lévő hello **Automation-fiók** panelt, és válassza **összes beállítás**.  
2. A hello **összes beállítás** panel alatt **Fiókbeállítások**, jelölje be **tulajdonságok**. 
3. Vegye figyelembe a hello hello értékek **tulajdonságok** panelen.<br><br> ![hello Automation-fiók "Tulajdonságok" panelen](media/automation-create-runas-account/automation-account-properties.png)  

### <a name="required-permissions-tooupdate-your-automation-account"></a>Engedélyek tooupdate az Automation-fiók szükséges
tooupdate Automation-fiók, rendelkeznie kell a következő bizonyos jogokat hello és engedélyek szükséges toocomplete ebben a témakörben.   
 
* Az AD-felhasználói fiókot Microsoft.Automation erőforrások kell toobe hozzáadott tooa szerepkör engedélyek egyenértékű toohello közreműködő szerepkörrel rendelkező, a cikkben leírt módon történő [szerepköralapú hozzáférés-vezérlés az Azure Automationben](automation-role-based-access-control.md#contributor-role-permissions).  
* Az Azure AD-bérlő a nem rendszergazda felhasználók is [AD-alkalmazásokat regisztrálni](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions) Ha hello App regisztrációk beállítás értéke túl**Igen**.  Ha hello app regisztrációk beállítás értéke túl**nem**, a művelet végrehajtása hello felhasználói globális rendszergazdának kell lennie az Azure ad-ben. 

Ha nem tagja Active Directory-példányban hello előfizetés toohello globális rendszergazda/járművezető-administrator szerepkör hello előfizetés vannak adása előtt, akkor tooActive Directory vendégként kerülnek. Ebben az esetben a megjelenik a "nem rendelkezik engedélyekkel toocreate..." Figyelmeztetés-hello **Automation-fiók hozzáadása** panelen. Felhasználók, akik toohello globális rendszergazda/járművezető-administrator szerepkör először eltávolíthatja hello előfizetés Active Directory-példányban, és újból hozzáadták toomake hozzáadták azokat a teljes felhasználói az Active Directoryban. tooverify ebben az esetben a hello **Azure Active Directory** hello Azure portálon, válassza a panelen **felhasználók és csoportok**, jelölje be **minden felhasználó** és hello kiválasztása után adott felhasználó, jelölje be **profil**. hello értékének hello **felhasználótípust** attribútum hello felhasználó profil alapján nem értékűnek kell lennie **vendég**.

## <a name="create-run-as-account-from-hello-portal"></a>Futtató fiók létrehozása hello portálról
Ebben a részben hajtható végre a következő lépéseket tooupdate hello az Azure Automation-fiók hello Azure-portálon.  Létrehozhat hello futtató és a klasszikus futtató fiókok külön-külön, és ha a klasszikus Azure portálon hello toomanage erőforrások nem szükséges, létrehozhat hello Azure-beli futtató fiókot csak.  

hello folyamat a következő elemeket az Automation-fiókban hello hoz létre.

**Futtató fiókok esetén:**

* Egy Azure AD-alkalmazást hoz létre egy önaláírt tanúsítványt hoz létre egy egyszerű szolgáltatásfiók hello alkalmazás az Azure AD és hello közreműködő szerepkört az aktuális előfizetésben hello fiókhoz rendeli hozzá. Ez a beállítás tooOwner vagy bármilyen más szerepkör módosíthatja. További információk: [Szerepköralapú hozzáférés-vezérlés az Azure Automationben](automation-role-based-access-control.md).
* Létrehoz egy nevű Automation szolgáltatásbeli tanúsítványeszköz *AzureRunAsCertificate* a hello megadva az Automation-fiók. hello tanúsítványeszköz hello tanúsítvány titkos kulcsa hello Azure AD-alkalmazás által használt tartalmazza.
* Létrehoz egy nevű Automation szolgáltatásbeli kapcsolateszköz *AzureRunAsConnection* a hello megadva az Automation-fiók. hello kapcsolódási eszköz rendelkezik hello applicationId, tenantId, előfizetés-azonosító és tanúsítvány-ujjlenyomatot.

**Klasszikus futtatófiókok esetében:**

* Létrehoz egy nevű Automation szolgáltatásbeli tanúsítványeszköz *AzureClassicRunAsCertificate* a hello megadva az Automation-fiók. hello tanúsítványeszköz hello tanúsítvány titkos kulcsa hello felügyeleti tanúsítvány által használt tartalmazza.
* Létrehoz egy nevű Automation szolgáltatásbeli kapcsolateszköz *AzureClassicRunAsConnection* a hello megadva az Automation-fiók. hello kapcsolódási eszköz hello előfizetés nevét, a subscriptionId és a tanúsítvány eszköz neve tartalmazza.

1. Bejelentkezés toohello egy olyan fiókkal, amely hello előfizetés-Rendszergazdák szerepkör tagja, és hello előfizetés társadminisztrátoraként Azure-portálon.
2. Hello Automation-fiók panelen válassza ki **futtató fiókok** hello szakaszban **Fiókbeállítások**.  
3. Attól függően, hogy melyik fiókra van szüksége, válassza az **Azure-alapú futtató fiók** vagy a **Klasszikus Azure-alapú futtató fiók** lehetőséget.  Vagy hello kijelölése után **adja hozzá Azure-beli futtató** vagy **hozzáadása Azure klasszikus futtató fiók** panel jelenik meg, és hello áttekintő információkat áttekintése után kattintson **létrehozása** a futtató fiók létrehozása tooproceed.  
4. Azure hello Futtatás mint fiókot hoz létre, amíg előrehaladásának hello alatt **értesítések** a hello menüre, és a fejléc megjelenik hello fiók létrehozása folyamatban van megjelölve.  A folyamat eltarthat néhány percig toocomplete.  

## <a name="create-run-as-account-using-powershell-script"></a>Futtató fiók létrehozása PowerShell-szkripttel
A PowerShell-parancsfájl a következő konfigurációk hello támogatását tartalmazza:

* Futtató fiók létrehozása önaláírt tanúsítvány használatával.
* Futtató fiók és klasszikus futtató fiók létrehozása önaláírt tanúsítvány használatával.
* Futtató fiók és klasszikus futtató fiók létrehozása vállalati tanúsítvány használatával.
* Hozzon létre egy futtató fiókot és egy klasszikus futtató fiókot egy önaláírt tanúsítványt a hello Azure Government felhő.

Attól függően, hogy hello konfigurációs beállítást választja a hello parancsfájl a következő elemek hello hoz létre.

**Futtató fiókok esetén:**

* Létrehoz egy Azure AD alkalmazás toobe exportálni vagy hello önaláírt vagy vállalati tanúsítvány nyilvános kulcsát, az Azure AD-hoz létre egy egyszerű szolgáltatásfiók hello alkalmazáshoz, és rendel hello közreműködő szerepkört az aktuális hello fiókhoz előfizetés. Ez a beállítás tooOwner vagy bármilyen más szerepkör módosíthatja. További információk: [Szerepköralapú hozzáférés-vezérlés az Azure Automationben](automation-role-based-access-control.md).
* Létrehoz egy nevű Automation szolgáltatásbeli tanúsítványeszköz *AzureRunAsCertificate* a hello megadva az Automation-fiók. hello tanúsítványeszköz hello tanúsítvány titkos kulcsa hello Azure AD-alkalmazás által használt tartalmazza.
* Létrehoz egy nevű Automation szolgáltatásbeli kapcsolateszköz *AzureRunAsConnection* a hello megadva az Automation-fiók. hello kapcsolódási eszköz rendelkezik hello applicationId, tenantId, előfizetés-azonosító és tanúsítvány-ujjlenyomatot.

**Klasszikus futtatófiókok esetében:**

* Létrehoz egy nevű Automation szolgáltatásbeli tanúsítványeszköz *AzureClassicRunAsCertificate* a hello megadva az Automation-fiók. hello tanúsítványeszköz hello tanúsítvány titkos kulcsa hello felügyeleti tanúsítvány által használt tartalmazza.
* Létrehoz egy nevű Automation szolgáltatásbeli kapcsolateszköz *AzureClassicRunAsConnection* a hello megadva az Automation-fiók. hello kapcsolódási eszköz hello előfizetés nevét, a subscriptionId és a tanúsítvány eszköz neve tartalmazza.

>[!NOTE]
> Ha a beállítást vagy a klasszikus futtató fiók létrehozásához, hello parancsfájl végrehajtása után, feltöltés hello nyilvános tanúsítványtároló (.cer kiterjesztésű) toohello felügyeleti hello előfizetés adott hello Automation-fiók hozták létre.
> 

1. Mentse a parancsfájlt a számítógépen a következő hello. Ebben a példában mentse hello Fájlnév *New-RunAsAccount.ps1*.

        #Requires -RunAsAdministrator
        Param (
        [Parameter(Mandatory=$true)]
        [String] $ResourceGroup,

        [Parameter(Mandatory=$true)]
        [String] $AutomationAccountName,

        [Parameter(Mandatory=$true)]
        [String] $ApplicationDisplayName,

        [Parameter(Mandatory=$true)]
        [String] $SubscriptionId,

        [Parameter(Mandatory=$true)]
        [Boolean] $CreateClassicRunAsAccount,

        [Parameter(Mandatory=$true)]
        [String] $SelfSignedCertPlainPassword,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$EnvironmentName="AzureCloud",

        [Parameter(Mandatory=$false)]
        [int] $SelfSignedCertNoOfMonthsUntilExpired = 12
        )

        function CreateSelfSignedCertificate([string] $keyVaultName, [string] $certificateName, [string] $selfSignedCertPlainPassword,
                                       [string] $certPath, [string] $certPathCer, [string] $selfSignedCertNoOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My `
            -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
            -NotAfter (Get-Date).AddMonths($selfSignedCertNoOfMonthsUntilExpired)

        $CertPassword = ConvertTo-SecureString $selfSignedCertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPathCer -Type CERT | Write-Verbose
        }

        function CreateServicePrincipal([System.Security.Cryptography.X509Certificates.X509Certificate2] $PfxCert, [string] $applicationDisplayName) {  
        $CurrentDate = Get-Date
        $keyValue = [System.Convert]::ToBase64String($PfxCert.GetRawCertData())
        $KeyId = (New-Guid).Guid

        $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
        $KeyCredential.StartDate = $CurrentDate
        $KeyCredential.EndDate = Get-Date $PfxCert.GetExpirationDateString()
        $KeyCredential.EndDate = $KeyCredential.EndDate.AddDays(-1)
        $KeyCredential.KeyId = $KeyId
        $KeyCredential.CertValue  = $keyValue

        # Use key credentials and create an Azure AD application
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $KeyCredential
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id

        # Sleep here for a few seconds tooallow hello service principal application toobecome active (ordinarily takes a few seconds)
        Sleep -s 15
        $NewRole = New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6)
        {
            Sleep -s 10
            New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
            $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
            $Retries++;
        }
            return $Application.ApplicationId.ToString();
        }

        function CreateAutomationCertificateAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $certifcateAssetName,[string] $certPath, [string] $certPlainPassword, [Boolean] $Exportable) {
        $CertPassword = ConvertTo-SecureString $certPlainPassword -AsPlainText -Force   
        Remove-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $certifcateAssetName -ErrorAction SilentlyContinue
        New-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Path $certPath -Name $certifcateAssetName -Password $CertPassword -Exportable:$Exportable  | write-verbose
        }

        function CreateAutomationConnectionAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $connectionAssetName, [string] $connectionTypeName, [System.Collections.Hashtable] $connectionFieldValues ) {
        Remove-AzureRmAutomationConnection -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -Force -ErrorAction SilentlyContinue
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -ConnectionTypeName $connectionTypeName -ConnectionFieldValues $connectionFieldValues
        }

        Import-Module AzureRM.Profile
        Import-Module AzureRM.Resources

        $AzureRMProfileVersion= (Get-Module AzureRM.Profile).Version
        if (!(($AzureRMProfileVersion.Major -ge 3 -and $AzureRMProfileVersion.Minor -ge 0) -or ($AzureRMProfileVersion.Major -gt 3)))
        {
            Write-Error -Message "Please install hello latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
            return
        }

        Login-AzureRmAccount -Environment $EnvironmentName 
        $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId

        # Create a Run As account by using a service principal
        $CertifcateAssetName = "AzureRunAsCertificate"
        $ConnectionAssetName = "AzureRunAsConnection"
        $ConnectionTypeName = "AzureServicePrincipal"

        if ($EnterpriseCertPathForRunAsAccount -and $EnterpriseCertPlainPasswordForRunAsAccount) {
        $PfxCertPathForRunAsAccount = $EnterpriseCertPathForRunAsAccount
        $PfxCertPlainPasswordForRunAsAccount = $EnterpriseCertPlainPasswordForRunAsAccount
        } else {
           $CertificateName = $AutomationAccountName+$CertifcateAssetName
           $PfxCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".pfx")
           $PfxCertPlainPasswordForRunAsAccount = $SelfSignedCertPlainPassword
           $CerCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".cer")
           CreateSelfSignedCertificate $KeyVaultName $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create a service principal
        $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
        $ApplicationId=CreateServicePrincipal $PfxCert $ApplicationDisplayName

        # Create hello Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

        # Populate hello ConnectionFieldValues
        $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
        $TenantID = $SubscriptionInfo | Select TenantId -First 1
        $Thumbprint = $PfxCert.Thumbprint
        $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}

        # Create an Automation connection asset named AzureRunAsConnection in hello Automation account. This connection uses hello service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

        if ($CreateClassicRunAsAccount) {
             # Create a Run As account by using a service principal
             $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
             $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
             $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
             $UploadMessage = "Please upload hello .cer format of #CERT# toohello Management store by following hello steps below." + [Environment]::NewLine +
                     "Log in toohello Microsoft Azure Management portal (https://manage.windowsazure.com) and select Settings -> Management Certificates." + [Environment]::NewLine +
                     "Then click Upload and upload hello .cer format of #CERT#"

              if ($EnterpriseCertPathForClassicRunAsAccount -and $EnterpriseCertPlainPasswordForClassicRunAsAccount ) {
              $PfxCertPathForClassicRunAsAccount = $EnterpriseCertPathForClassicRunAsAccount
              $PfxCertPlainPasswordForClassicRunAsAccount = $EnterpriseCertPlainPasswordForClassicRunAsAccount
              $UploadMessage = $UploadMessage.Replace("#CERT#", $PfxCertPathForClassicRunAsAccount)
        } else {
              $ClassicRunAsAccountCertificateName = $AutomationAccountName+$ClassicRunAsAccountCertifcateAssetName
              $PfxCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".pfx")
              $PfxCertPlainPasswordForClassicRunAsAccount = $SelfSignedCertPlainPassword
              $CerCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".cer")
              $UploadMessage = $UploadMessage.Replace("#CERT#", $CerCertPathForClassicRunAsAccount)
              CreateSelfSignedCertificate $KeyVaultName $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create hello Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate hello ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.Name
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in hello Automation account. This connection uses hello service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. A számítógépen indítása **Windows PowerShell** a hello **Start** képernyő emelt szintű felhasználói jogosultságokkal.
3. A hello emelt szintű parancssori rendszerhéj, az 1. lépésben létrehozott hello parancsfájl tartalmazó lépjen toohello mappát.  
4. Hajtsa végre a hello parancsfájlt hello paraméter értékének használatával hello konfiguráció szükséges.

    **Futtató fiók létrehozása önaláírt tanúsítvány használatával**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    **Futtató fiók és klasszikus futtató fiók létrehozása önaláírt tanúsítvány használatával**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    **Futtató fiók és klasszikus futtató fiók létrehozása vállalati tanúsítvány használatával**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    **Hozzon létre egy futtató fiókot és egy klasszikus Futtatás mint fiók hello Azure Government felhő önaláírt tanúsítvány használatával**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > Hello parancsfájl által végrehajtott, miután az Azure-ral felszólító tooauthenticate fogja. Olyan fiókkal jelentkezzen be, amely hello előfizetés Rendszergazdák szerepkör tagja, és hello előfizetés társadminisztrátoraként.
    >
    >

Miután hello parancsfájl végrehajtása sikeres, vegye figyelembe a hello következőket:
* Nyilvános önaláírt tanúsítvány (.cer-fájl) klasszikus futtató fiókot hozta létre, ha hello parancsfájl hoz létre, és a számítógép hello felhasználói profil toohello ideiglenes fájlok mappába menti *%USERPROFILE%\AppData\Local\Temp*, használt tooexecute hello PowerShell-munkamenetben.
* Ha egy (.cer formátumú) vállalati tanúsítvánnyal rendelkező klasszikus futtató fiókot hozott létre, használja ezt a tanúsítványt. Hello utasításokat követve [feltöltése a felügyeleti API tanúsítvány toohello a klasszikus Azure portálon](../azure-api-management-certs.md), és ezután ellenőrizze a klasszikus üzembe helyezési erőforrások hello hitelesítőadat-konfiguráció hello segítségével [példakód az Azure klasszikus telepítési erőforrásai tooauthenticate](automation-verify-runas-authentication.md#classic-run-as-authentication). 
* Ha elvégezte *nem* klasszikus futtató fiók létrehozása, erőforrás-kezelő erőforrások a hitelesítést és hello hitelesítőadat-konfiguráció érvényesítése hello segítségével [példakód azonosítja a felügyeleti szolgáltatás erőforrások](automation-verify-runas-authentication.md#automation-run-as-authentication).

## <a name="next-steps"></a>Következő lépések
* Szolgáltatásnevekről kapcsolatos további információkért tekintse meg túl[alkalmazás és szolgáltatás egyszerű objektumok](../active-directory/active-directory-application-objects.md).
* Tanúsítványok és az Azure-szolgáltatásokkal kapcsolatos további információkért tekintse meg túl[tanúsítványok áttekintése Azure-szolgáltatásokhoz](../cloud-services/cloud-services-certs-create.md).
