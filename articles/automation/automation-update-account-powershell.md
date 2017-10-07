---
title: "Azure Automation futtató fiókot a PowerShell-lel aaaCreate |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooupgrade az Automation-fiók a PowerShell toocreate hello futtató fiókok Ha hello portálról kezdeti létrehozása során nem volt végrehajtani ezt a lépést."
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
ms.date: 04/14/2017
ms.author: magoedte
ms.openlocfilehash: 1049601321d2bc1e5f9d982f622788f8e4e4d797
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="update-automation-run-as-account-using-powershell"></a>Automation futtató fiók frissítése a PowerShell használatával
Akkor használhatja PowerShell tooupdate a meglévő Automation-fiókot:

* Automation-fiók létrehozása, de elutasítása toocreate hello futtató fiókot.
* Már használja egy automatizálási fiókot toomanage Resource Managerhez tartozó erőforrások és tooupdate hello tooinclude hello futtató fiókok kívánt runbook-hitelesítéshez.
* Az automatizálási fiók toomanage hagyományos erőforrások már használja, és azt szeretné, hogy tooupdate azt toouse hello klasszikus futtató fiókot új fiók létrehozása és a forgatókönyve és eszköze tooit áttelepítése helyett.   
* Érdemes toocreate egy olyan futtató és a klasszikus futtató fiókot a vállalati hitelesítésszolgáltató (CA) által kiadott tanúsítvánnyal.

## <a name="prerequisites"></a>Előfeltételek

* hello parancsfájl csak a Windows 10 és Windows Server 2016 futtatható Azure Resource Manager modulok 2.01 és újabb verziók. A korábbi Windows-verziók esetében nem támogatott.
* Az Azure PowerShell 1.0-s és újabb verziói. További információk hello PowerShell 1.0-s kiadásról: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).
* Automation-fiók, hello hello értékként hivatkozott *– AutomationAccountName* és *- ApplicationDisplayName* paramétereket a PowerShell-parancsfájl a következő hello.

tooget hello értékei *SubscriptionID*, *ResourceGroup*, és *AutomationAccountName*, amely hello parancsfájlok kötelező paraméterek tartoznak, a következő hello:
1. A hello Azure-portálon, válassza ki az Automation-fiókban lévő hello **Automation-fiók** panelt, és válassza **összes beállítás**. 
2. A hello **összes beállítás** panel alatt **Fiókbeállítások**, jelölje be **tulajdonságok**. 
3. Vegye figyelembe a hello hello értékek **tulajdonságok** panelen.

![hello Automation-fiók "Tulajdonságok" panelen](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

## <a name="create-run-as-account-powershell-script"></a>Futtató fiókhoz használható PowerShell-parancsprogram létrehozása
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
        $KeyCredential.EndDate= [DateTime]$PfxCert.GetExpirationDateString()
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
        if (!(($AzureRMProfileVersion.Major -ge 2 -and $AzureRMProfileVersion.Minor -ge 1) -or ($AzureRMProfileVersion.Major -gt 2)))
        {
           Write-Error -Message "Please install hello latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
           return
        }

        Login-AzureRmAccount -EnvironmentName $EnvironmentName
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
        $SubscriptionName = $subscription.Subscription.SubscriptionName
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
