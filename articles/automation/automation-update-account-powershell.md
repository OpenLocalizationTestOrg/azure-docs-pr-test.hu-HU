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
# <a name="update-automation-run-as-account-using-powershell"></a><span data-ttu-id="fcbaa-103">Automation futtató fiók frissítése a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="fcbaa-103">Update Automation Run As account using PowerShell</span></span>
<span data-ttu-id="fcbaa-104">Akkor használhatja PowerShell tooupdate a meglévő Automation-fiókot:</span><span class="sxs-lookup"><span data-stu-id="fcbaa-104">You can use PowerShell tooupdate your existing Automation account if:</span></span>

* <span data-ttu-id="fcbaa-105">Automation-fiók létrehozása, de elutasítása toocreate hello futtató fiókot.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-105">You create an Automation account but decline toocreate hello Run As account.</span></span>
* <span data-ttu-id="fcbaa-106">Már használja egy automatizálási fiókot toomanage Resource Managerhez tartozó erőforrások és tooupdate hello tooinclude hello futtató fiókok kívánt runbook-hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-106">You already use an Automation account toomanage Resource Manager resources and you want tooupdate hello account tooinclude hello Run As account for runbook authentication.</span></span>
* <span data-ttu-id="fcbaa-107">Az automatizálási fiók toomanage hagyományos erőforrások már használja, és azt szeretné, hogy tooupdate azt toouse hello klasszikus futtató fiókot új fiók létrehozása és a forgatókönyve és eszköze tooit áttelepítése helyett.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-107">You already use an Automation account toomanage classic resources and you want tooupdate it toouse hello Classic Run As account instead of creating a new account and migrating your runbooks and assets tooit.</span></span>   
* <span data-ttu-id="fcbaa-108">Érdemes toocreate egy olyan futtató és a klasszikus futtató fiókot a vállalati hitelesítésszolgáltató (CA) által kiadott tanúsítvánnyal.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-108">You want toocreate a Run As and a Classic Run As account by using a certificate issued by your enterprise certification authority (CA).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fcbaa-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fcbaa-109">Prerequisites</span></span>

* <span data-ttu-id="fcbaa-110">hello parancsfájl csak a Windows 10 és Windows Server 2016 futtatható Azure Resource Manager modulok 2.01 és újabb verziók.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-110">hello script can be run only on Windows 10 and Windows Server 2016 with Azure Resource Manager modules 2.01 and later.</span></span> <span data-ttu-id="fcbaa-111">A korábbi Windows-verziók esetében nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-111">It is not supported on earlier versions of Windows.</span></span>
* <span data-ttu-id="fcbaa-112">Az Azure PowerShell 1.0-s és újabb verziói.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-112">Azure PowerShell 1.0 and later.</span></span> <span data-ttu-id="fcbaa-113">További információk hello PowerShell 1.0-s kiadásról: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fcbaa-113">For information about hello PowerShell 1.0 release, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="fcbaa-114">Automation-fiók, hello hello értékként hivatkozott *– AutomationAccountName* és *- ApplicationDisplayName* paramétereket a PowerShell-parancsfájl a következő hello.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-114">An Automation account, which is referenced as hello value for hello *–AutomationAccountName* and *-ApplicationDisplayName* parameters in hello following PowerShell script.</span></span>

<span data-ttu-id="fcbaa-115">tooget hello értékei *SubscriptionID*, *ResourceGroup*, és *AutomationAccountName*, amely hello parancsfájlok kötelező paraméterek tartoznak, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="fcbaa-115">tooget hello values for *SubscriptionID*, *ResourceGroup*, and *AutomationAccountName*, which are required parameters for hello scripts, do hello following:</span></span>
1. <span data-ttu-id="fcbaa-116">A hello Azure-portálon, válassza ki az Automation-fiókban lévő hello **Automation-fiók** panelt, és válassza **összes beállítás**.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-116">In hello Azure portal, select your Automation account on hello **Automation account** blade, and then select **All settings**.</span></span> 
2. <span data-ttu-id="fcbaa-117">A hello **összes beállítás** panel alatt **Fiókbeállítások**, jelölje be **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-117">On hello **All settings** blade, under **Account Settings**, select **Properties**.</span></span> 
3. <span data-ttu-id="fcbaa-118">Vegye figyelembe a hello hello értékek **tulajdonságok** panelen.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-118">Note hello values on hello **Properties** blade.</span></span>

![hello Automation-fiók "Tulajdonságok" panelen](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

## <a name="create-run-as-account-powershell-script"></a><span data-ttu-id="fcbaa-120">Futtató fiókhoz használható PowerShell-parancsprogram létrehozása</span><span class="sxs-lookup"><span data-stu-id="fcbaa-120">Create Run As Account PowerShell script</span></span>
<span data-ttu-id="fcbaa-121">A PowerShell-parancsfájl a következő konfigurációk hello támogatását tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="fcbaa-121">This PowerShell script includes support for hello following configurations:</span></span>

* <span data-ttu-id="fcbaa-122">Futtató fiók létrehozása önaláírt tanúsítvány használatával.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-122">Create a Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="fcbaa-123">Futtató fiók és klasszikus futtató fiók létrehozása önaláírt tanúsítvány használatával.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-123">Create a Run As account and a Classic Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="fcbaa-124">Futtató fiók és klasszikus futtató fiók létrehozása vállalati tanúsítvány használatával.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-124">Create a Run As account and a Classic Run As account by using an enterprise certificate.</span></span>
* <span data-ttu-id="fcbaa-125">Hozzon létre egy futtató fiókot és egy klasszikus futtató fiókot egy önaláírt tanúsítványt a hello Azure Government felhő.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-125">Create a Run As account and a Classic Run As account by using a self-signed certificate in hello Azure Government cloud.</span></span>

<span data-ttu-id="fcbaa-126">Attól függően, hogy hello konfigurációs beállítást választja a hello parancsfájl a következő elemek hello hoz létre.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-126">Depending on hello configuration option you select, hello script creates hello following items.</span></span>

<span data-ttu-id="fcbaa-127">**Futtató fiókok esetén:**</span><span class="sxs-lookup"><span data-stu-id="fcbaa-127">**For Run As accounts:**</span></span>

* <span data-ttu-id="fcbaa-128">Létrehoz egy Azure AD alkalmazás toobe exportálni vagy hello önaláírt vagy vállalati tanúsítvány nyilvános kulcsát, az Azure AD-hoz létre egy egyszerű szolgáltatásfiók hello alkalmazáshoz, és rendel hello közreműködő szerepkört az aktuális hello fiókhoz előfizetés.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-128">Creates an Azure AD application toobe exported with either hello self-signed or enterprise certificate public key, creates a service principal account for hello application in Azure AD, and assigns hello Contributor role for hello account in your current subscription.</span></span> <span data-ttu-id="fcbaa-129">Ez a beállítás tooOwner vagy bármilyen más szerepkör módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-129">You can change this setting tooOwner or any other role.</span></span> <span data-ttu-id="fcbaa-130">További információk: [Szerepköralapú hozzáférés-vezérlés az Azure Automationben](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="fcbaa-130">For more information, see [Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="fcbaa-131">Létrehoz egy nevű Automation szolgáltatásbeli tanúsítványeszköz *AzureRunAsCertificate* a hello megadva az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-131">Creates an Automation certificate asset named *AzureRunAsCertificate* in hello specified Automation account.</span></span> <span data-ttu-id="fcbaa-132">hello tanúsítványeszköz hello tanúsítvány titkos kulcsa hello Azure AD-alkalmazás által használt tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-132">hello certificate asset holds hello certificate private key that's used by hello Azure AD application.</span></span>
* <span data-ttu-id="fcbaa-133">Létrehoz egy nevű Automation szolgáltatásbeli kapcsolateszköz *AzureRunAsConnection* a hello megadva az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-133">Creates an Automation connection asset named *AzureRunAsConnection* in hello specified Automation account.</span></span> <span data-ttu-id="fcbaa-134">hello kapcsolódási eszköz rendelkezik hello applicationId, tenantId, előfizetés-azonosító és tanúsítvány-ujjlenyomatot.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-134">hello connection asset holds hello applicationId, tenantId, subscriptionId, and certificate thumbprint.</span></span>

<span data-ttu-id="fcbaa-135">**Klasszikus futtatófiókok esetében:**</span><span class="sxs-lookup"><span data-stu-id="fcbaa-135">**For Classic Run As accounts:**</span></span>

* <span data-ttu-id="fcbaa-136">Létrehoz egy nevű Automation szolgáltatásbeli tanúsítványeszköz *AzureClassicRunAsCertificate* a hello megadva az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-136">Creates an Automation certificate asset named *AzureClassicRunAsCertificate* in hello specified Automation account.</span></span> <span data-ttu-id="fcbaa-137">hello tanúsítványeszköz hello tanúsítvány titkos kulcsa hello felügyeleti tanúsítvány által használt tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-137">hello certificate asset holds hello certificate private key used by hello management certificate.</span></span>
* <span data-ttu-id="fcbaa-138">Létrehoz egy nevű Automation szolgáltatásbeli kapcsolateszköz *AzureClassicRunAsConnection* a hello megadva az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-138">Creates an Automation connection asset named *AzureClassicRunAsConnection* in hello specified Automation account.</span></span> <span data-ttu-id="fcbaa-139">hello kapcsolódási eszköz hello előfizetés nevét, a subscriptionId és a tanúsítvány eszköz neve tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-139">hello connection asset holds hello subscription name, subscriptionId, and certificate asset name.</span></span>

>[!NOTE]
> <span data-ttu-id="fcbaa-140">Ha a beállítást vagy a klasszikus futtató fiók létrehozásához, hello parancsfájl végrehajtása után, feltöltés hello nyilvános tanúsítványtároló (.cer kiterjesztésű) toohello felügyeleti hello előfizetés adott hello Automation-fiók hozták létre.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-140">If you select either option for creating a Classic Run As account, after hello script is executed, upload hello public certificate (.cer file name extension) toohello management store for hello subscription that hello Automation account was created in.</span></span>
> 

1. <span data-ttu-id="fcbaa-141">Mentse a parancsfájlt a számítógépen a következő hello.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-141">Save hello following script on your computer.</span></span> <span data-ttu-id="fcbaa-142">Ebben a példában mentse hello Fájlnév *New-RunAsAccount.ps1*.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-142">In this example, save it with hello filename *New-RunAsAccount.ps1*.</span></span>

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

2. <span data-ttu-id="fcbaa-143">A számítógépen indítása **Windows PowerShell** a hello **Start** képernyő emelt szintű felhasználói jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-143">On your computer, start **Windows PowerShell** from hello **Start** screen with elevated user rights.</span></span>
3. <span data-ttu-id="fcbaa-144">A hello emelt szintű parancssori rendszerhéj, az 1. lépésben létrehozott hello parancsfájl tartalmazó lépjen toohello mappát.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-144">From hello elevated command-line shell, go toohello folder that contains hello script you created in step 1.</span></span>  
4. <span data-ttu-id="fcbaa-145">Hajtsa végre a hello parancsfájlt hello paraméter értékének használatával hello konfiguráció szükséges.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-145">Execute hello script by using hello parameter values for hello configuration you require.</span></span>

    <span data-ttu-id="fcbaa-146">**Futtató fiók létrehozása önaláírt tanúsítvány használatával**</span><span class="sxs-lookup"><span data-stu-id="fcbaa-146">**Create a Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    <span data-ttu-id="fcbaa-147">**Futtató fiók és klasszikus futtató fiók létrehozása önaláírt tanúsítvány használatával**</span><span class="sxs-lookup"><span data-stu-id="fcbaa-147">**Create a Run As account and a Classic Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    <span data-ttu-id="fcbaa-148">**Futtató fiók és klasszikus futtató fiók létrehozása vállalati tanúsítvány használatával**</span><span class="sxs-lookup"><span data-stu-id="fcbaa-148">**Create a Run As account and a Classic Run As account by using an enterprise certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    <span data-ttu-id="fcbaa-149">**Hozzon létre egy futtató fiókot és egy klasszikus Futtatás mint fiók hello Azure Government felhő önaláírt tanúsítvány használatával**</span><span class="sxs-lookup"><span data-stu-id="fcbaa-149">**Create a Run As account and a Classic Run As account by using a self-signed certificate in hello Azure Government cloud**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > <span data-ttu-id="fcbaa-150">Hello parancsfájl által végrehajtott, miután az Azure-ral felszólító tooauthenticate fogja.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-150">After hello script has executed, you will be prompted tooauthenticate with Azure.</span></span> <span data-ttu-id="fcbaa-151">Olyan fiókkal jelentkezzen be, amely hello előfizetés Rendszergazdák szerepkör tagja, és hello előfizetés társadminisztrátoraként.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-151">Sign in with an account that is a member of hello subscription administrators role and co-administrator of hello subscription.</span></span>
    >
    >

<span data-ttu-id="fcbaa-152">Miután hello parancsfájl végrehajtása sikeres, vegye figyelembe a hello következőket:</span><span class="sxs-lookup"><span data-stu-id="fcbaa-152">After hello script has executed successfully, note hello following:</span></span>
* <span data-ttu-id="fcbaa-153">Nyilvános önaláírt tanúsítvány (.cer-fájl) klasszikus futtató fiókot hozta létre, ha hello parancsfájl hoz létre, és a számítógép hello felhasználói profil toohello ideiglenes fájlok mappába menti *%USERPROFILE%\AppData\Local\Temp*, használt tooexecute hello PowerShell-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-153">If you created a Classic Run As account with a self-signed public certificate (.cer file), hello script creates and saves it toohello temporary files folder on your computer under hello user profile *%USERPROFILE%\AppData\Local\Temp*, which you used tooexecute hello PowerShell session.</span></span>
* <span data-ttu-id="fcbaa-154">Ha egy (.cer formátumú) vállalati tanúsítvánnyal rendelkező klasszikus futtató fiókot hozott létre, használja ezt a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="fcbaa-154">If you created a Classic Run As account with an enterprise public certificate (.cer file), use this certificate.</span></span> <span data-ttu-id="fcbaa-155">Hello utasításokat követve [feltöltése a felügyeleti API tanúsítvány toohello a klasszikus Azure portálon](../azure-api-management-certs.md), és ezután ellenőrizze a klasszikus üzembe helyezési erőforrások hello hitelesítőadat-konfiguráció hello segítségével [példakód az Azure klasszikus telepítési erőforrásai tooauthenticate](automation-verify-runas-authentication.md#classic-run-as-authentication).</span><span class="sxs-lookup"><span data-stu-id="fcbaa-155">Follow hello instructions for [uploading a management API certificate toohello Azure classic portal](../azure-api-management-certs.md), and then validate hello credential configuration with classic deployment resources by using hello [sample code tooauthenticate with Azure Classic Deployment Resources](automation-verify-runas-authentication.md#classic-run-as-authentication).</span></span> 
* <span data-ttu-id="fcbaa-156">Ha elvégezte *nem* klasszikus futtató fiók létrehozása, erőforrás-kezelő erőforrások a hitelesítést és hello hitelesítőadat-konfiguráció érvényesítése hello segítségével [példakód azonosítja a felügyeleti szolgáltatás erőforrások](automation-verify-runas-authentication.md#automation-run-as-authentication).</span><span class="sxs-lookup"><span data-stu-id="fcbaa-156">If you did *not* create a Classic Run As account, authenticate with Resource Manager resources and validate hello credential configuration by using hello [sample code for authenticating with Service Management resources](automation-verify-runas-authentication.md#automation-run-as-authentication).</span></span>

## <a name="next-steps"></a><span data-ttu-id="fcbaa-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fcbaa-157">Next steps</span></span>
* <span data-ttu-id="fcbaa-158">Szolgáltatásnevekről kapcsolatos további információkért tekintse meg túl[alkalmazás és szolgáltatás egyszerű objektumok](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="fcbaa-158">For more information about Service Principals, refer too[Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md).</span></span>
* <span data-ttu-id="fcbaa-159">Tanúsítványok és az Azure-szolgáltatásokkal kapcsolatos további információkért tekintse meg túl[tanúsítványok áttekintése Azure-szolgáltatásokhoz](../cloud-services/cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="fcbaa-159">For more information about certificates and Azure services, refer too[Certificates overview for Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).</span></span>
