---
title: "Azure Automation futtató fiók létrehozása PowerShell használatával | Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan frissítheti Automation-fiókját a PowerShell használatával, hogy futtató fiókokat hozzon létre, ha a portálon a létrehozás során nem végezte el ezt a lépést."
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
ms.openlocfilehash: a41a57ee3815a815c23e9bbe2dd0309e6c61c8f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="update-automation-run-as-account-using-powershell"></a><span data-ttu-id="eeb58-103">Automation futtató fiók frissítése a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="eeb58-103">Update Automation Run As account using PowerShell</span></span>
<span data-ttu-id="eeb58-104">A PowerShell segítségével frissítheti meglévő Automation-fiókját. Erre a következő esetekben lehet szükség:</span><span class="sxs-lookup"><span data-stu-id="eeb58-104">You can use PowerShell to update your existing Automation account if:</span></span>

* <span data-ttu-id="eeb58-105">Létrehozott egy Automation-fiókot, de futtató fiókot nem.</span><span class="sxs-lookup"><span data-stu-id="eeb58-105">You create an Automation account but decline to create the Run As account.</span></span>
* <span data-ttu-id="eeb58-106">Már használ egy, a Resource Manager-erőforrások felügyeletére szolgáló Automation-fiókot, de szeretné frissíteni, hogy runbookos hitelesítést lehetővé tevő futtató fiókot is tartalmazzon.</span><span class="sxs-lookup"><span data-stu-id="eeb58-106">You already use an Automation account to manage Resource Manager resources and you want to update the account to include the Run As account for runbook authentication.</span></span>
* <span data-ttu-id="eeb58-107">Már használ egy, a klasszikus erőforrások felügyeletére szolgáló Automation-fiókot, de szeretné azt frissíteni, és klasszikus futtató fiókként használni, hogy megtakarítsa az új fiók létrehozásához és a runbookok és objektumok áthelyezéséhez szükséges időt.</span><span class="sxs-lookup"><span data-stu-id="eeb58-107">You already use an Automation account to manage classic resources and you want to update it to use the Classic Run As account instead of creating a new account and migrating your runbooks and assets to it.</span></span>   
* <span data-ttu-id="eeb58-108">Egy futtató és egy klasszikus futtató fiókot szeretne létrehozni a vállalati hitelesítésszolgáltató által kibocsátott tanúsítvánnyal.</span><span class="sxs-lookup"><span data-stu-id="eeb58-108">You want to create a Run As and a Classic Run As account by using a certificate issued by your enterprise certification authority (CA).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eeb58-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="eeb58-109">Prerequisites</span></span>

* <span data-ttu-id="eeb58-110">Ez a szkript kizárólag Windows 10 és Azure Resource Manager 2.01-es vagy újabb modulokkal rendelkező Windows Server 2016 rendszeren futtatható.</span><span class="sxs-lookup"><span data-stu-id="eeb58-110">The script can be run only on Windows 10 and Windows Server 2016 with Azure Resource Manager modules 2.01 and later.</span></span> <span data-ttu-id="eeb58-111">A korábbi Windows-verziók esetében nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="eeb58-111">It is not supported on earlier versions of Windows.</span></span>
* <span data-ttu-id="eeb58-112">Az Azure PowerShell 1.0-s és újabb verziói.</span><span class="sxs-lookup"><span data-stu-id="eeb58-112">Azure PowerShell 1.0 and later.</span></span> <span data-ttu-id="eeb58-113">Információk a PowerShell 1.0-s kiadásáról: [Az Azure PowerShell telepítése és konfigurálása](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="eeb58-113">For information about the PowerShell 1.0 release, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="eeb58-114">Olyan Automation-fiók, amelyre az alábbi PowerShell-szkript az *–AutomationAccountName* és az *-ApplicationDisplayName* paraméterek értékeként hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="eeb58-114">An Automation account, which is referenced as the value for the *–AutomationAccountName* and *-ApplicationDisplayName* parameters in the following PowerShell script.</span></span>

<span data-ttu-id="eeb58-115">A szkript végrehajtásához feltétlenül szükséges *SubscriptionID*, *ResourceGroup* és *AutomationAccountName* paraméterek értékének lekéréséhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="eeb58-115">To get the values for *SubscriptionID*, *ResourceGroup*, and *AutomationAccountName*, which are required parameters for the scripts, do the following:</span></span>
1. <span data-ttu-id="eeb58-116">Az Azure Portalon válassza ki Automation-fiókját az **Automation-fiók** panelen, majd válassza a **Minden beállítás** elemet.</span><span class="sxs-lookup"><span data-stu-id="eeb58-116">In the Azure portal, select your Automation account on the **Automation account** blade, and then select **All settings**.</span></span> 
2. <span data-ttu-id="eeb58-117">A **Minden beállítás** panel **Fiókbeállítások** részénél válassza a **Tulajdonságok** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="eeb58-117">On the **All settings** blade, under **Account Settings**, select **Properties**.</span></span> 
3. <span data-ttu-id="eeb58-118">Jegyezze fel a **Tulajdonságok** panelen megjelenő értékeket.</span><span class="sxs-lookup"><span data-stu-id="eeb58-118">Note the values on the **Properties** blade.</span></span>

![Az Automation-fiók „Tulajdonságok” panelje](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

## <a name="create-run-as-account-powershell-script"></a><span data-ttu-id="eeb58-120">Futtató fiókhoz használható PowerShell-parancsprogram létrehozása</span><span class="sxs-lookup"><span data-stu-id="eeb58-120">Create Run As Account PowerShell script</span></span>
<span data-ttu-id="eeb58-121">Ez a PowerShell-szkript a következő konfigurációk támogatását tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="eeb58-121">This PowerShell script includes support for the following configurations:</span></span>

* <span data-ttu-id="eeb58-122">Futtató fiók létrehozása önaláírt tanúsítvány használatával.</span><span class="sxs-lookup"><span data-stu-id="eeb58-122">Create a Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="eeb58-123">Futtató fiók és klasszikus futtató fiók létrehozása önaláírt tanúsítvány használatával.</span><span class="sxs-lookup"><span data-stu-id="eeb58-123">Create a Run As account and a Classic Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="eeb58-124">Futtató fiók és klasszikus futtató fiók létrehozása vállalati tanúsítvány használatával.</span><span class="sxs-lookup"><span data-stu-id="eeb58-124">Create a Run As account and a Classic Run As account by using an enterprise certificate.</span></span>
* <span data-ttu-id="eeb58-125">Futtató fiók és klasszikus futtató fiók létrehozása az Azure Government Cloud egyik önaláírt tanúsítványának használatával.</span><span class="sxs-lookup"><span data-stu-id="eeb58-125">Create a Run As account and a Classic Run As account by using a self-signed certificate in the Azure Government cloud.</span></span>

<span data-ttu-id="eeb58-126">A szkript az alábbi elemeket hozza létre a kiválasztott konfigurációs beállításoktól függően.</span><span class="sxs-lookup"><span data-stu-id="eeb58-126">Depending on the configuration option you select, the script creates the following items.</span></span>

<span data-ttu-id="eeb58-127">**Futtató fiókok esetén:**</span><span class="sxs-lookup"><span data-stu-id="eeb58-127">**For Run As accounts:**</span></span>

* <span data-ttu-id="eeb58-128">Létrehoz egy Azure AD-alkalmazást, amelynek az exportálása az önaláírt vagy vállalati tanúsítvány nyilvános kulcsával történik, továbbá létrehoz egy egyszerű szolgáltatásfiókot az alkalmazáshoz az Azure AD-ben, és hozzárendeli a Közreműködő szerepkört ehhez a fiókhoz a jelenlegi előfizetésében.</span><span class="sxs-lookup"><span data-stu-id="eeb58-128">Creates an Azure AD application to be exported with either the self-signed or enterprise certificate public key, creates a service principal account for the application in Azure AD, and assigns the Contributor role for the account in your current subscription.</span></span> <span data-ttu-id="eeb58-129">Ezt a beállítást bármikor módosíthatja Tulajdonos értékre vagy bármely egyéb szerepkörre.</span><span class="sxs-lookup"><span data-stu-id="eeb58-129">You can change this setting to Owner or any other role.</span></span> <span data-ttu-id="eeb58-130">További információk: [Szerepköralapú hozzáférés-vezérlés az Azure Automationben](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="eeb58-130">For more information, see [Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="eeb58-131">Létrehoz egy *AzureRunAsCertificate* nevű Automation-tanúsítványobjektumot a megadott Automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="eeb58-131">Creates an Automation certificate asset named *AzureRunAsCertificate* in the specified Automation account.</span></span> <span data-ttu-id="eeb58-132">Ez a tanúsítványobjektum tartalmazza az Azure AD-alkalmazás által használt titkos tanúsítványkulcsot.</span><span class="sxs-lookup"><span data-stu-id="eeb58-132">The certificate asset holds the certificate private key that's used by the Azure AD application.</span></span>
* <span data-ttu-id="eeb58-133">Létrehoz egy *AzureRunAsConnection* nevű Automation-kapcsolatobjektumot a megadott Automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="eeb58-133">Creates an Automation connection asset named *AzureRunAsConnection* in the specified Automation account.</span></span> <span data-ttu-id="eeb58-134">Ez a kapcsolatobjektum magában foglalja az alkalmazásazonosítót, a bérlőazonosítót, az előfizetés-azonosítót és a tanúsítvány ujjlenyomatát.</span><span class="sxs-lookup"><span data-stu-id="eeb58-134">The connection asset holds the applicationId, tenantId, subscriptionId, and certificate thumbprint.</span></span>

<span data-ttu-id="eeb58-135">**Klasszikus futtatófiókok esetében:**</span><span class="sxs-lookup"><span data-stu-id="eeb58-135">**For Classic Run As accounts:**</span></span>

* <span data-ttu-id="eeb58-136">Létrehoz egy *AzureClassicRunAsCertificate* nevű Automation-tanúsítványobjektumot a megadott Automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="eeb58-136">Creates an Automation certificate asset named *AzureClassicRunAsCertificate* in the specified Automation account.</span></span> <span data-ttu-id="eeb58-137">Ez a tanúsítványobjektum tartalmazza a felügyeleti tanúsítvány által használt titkos tanúsítványkulcsot.</span><span class="sxs-lookup"><span data-stu-id="eeb58-137">The certificate asset holds the certificate private key used by the management certificate.</span></span>
* <span data-ttu-id="eeb58-138">Létrehoz egy *AzureClassicRunAsConnection* nevű Automation-kapcsolatobjektumot a megadott Automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="eeb58-138">Creates an Automation connection asset named *AzureClassicRunAsConnection* in the specified Automation account.</span></span> <span data-ttu-id="eeb58-139">Ez a kapcsolatobjektum tartalmazza az előfizetés nevét, a subscriptionId paramétert, valamint a tanúsítványobjektum nevét.</span><span class="sxs-lookup"><span data-stu-id="eeb58-139">The connection asset holds the subscription name, subscriptionId, and certificate asset name.</span></span>

>[!NOTE]
> <span data-ttu-id="eeb58-140">Ha klasszikus futtató fiók létrehozását választja, a szkript végrehajtása után töltse fel a nyilvános tanúsítványt (.cer fájlnévkiterjesztéssel) annak az előfizetésnek a felügyeleti tárába, amelyben az Automation-fiókot létrehozták.</span><span class="sxs-lookup"><span data-stu-id="eeb58-140">If you select either option for creating a Classic Run As account, after the script is executed, upload the public certificate (.cer file name extension) to the management store for the subscription that the Automation account was created in.</span></span>
> 

1. <span data-ttu-id="eeb58-141">Mentse el a következő parancsprogramot a számítógépén.</span><span class="sxs-lookup"><span data-stu-id="eeb58-141">Save the following script on your computer.</span></span> <span data-ttu-id="eeb58-142">Ebben a példában mentse a következő fájlnéven: *New-RunAsAccount.ps1*.</span><span class="sxs-lookup"><span data-stu-id="eeb58-142">In this example, save it with the filename *New-RunAsAccount.ps1*.</span></span>

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

        # Sleep here for a few seconds to allow the service principal application to become active (ordinarily takes a few seconds)
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
           Write-Error -Message "Please install the latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
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

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

        # Populate the ConnectionFieldValues
        $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
        $TenantID = $SubscriptionInfo | Select TenantId -First 1
        $Thumbprint = $PfxCert.Thumbprint
        $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

        if ($CreateClassicRunAsAccount) {
            # Create a Run As account by using a service principal
            $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
            $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
            $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
            $UploadMessage = "Please upload the .cer format of #CERT# to the Management store by following the steps below." + [Environment]::NewLine +
                    "Log in to the Microsoft Azure Management portal (https://manage.windowsazure.com) and select Settings -> Management Certificates." + [Environment]::NewLine +
                    "Then click Upload and upload the .cer format of #CERT#"

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

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate the ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.SubscriptionName
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. <span data-ttu-id="eeb58-143">A számítógépén indítsa el a **Windows PowerShell** alkalmazást a **Kezdőlap** képernyőről emelt szintű felhasználói jogokkal.</span><span class="sxs-lookup"><span data-stu-id="eeb58-143">On your computer, start **Windows PowerShell** from the **Start** screen with elevated user rights.</span></span>
3. <span data-ttu-id="eeb58-144">A rendszergazda jogú parancssori felületből lépjen abba a mappába, amely az 1. lépésben létrehozott szkriptet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="eeb58-144">From the elevated command-line shell, go to the folder that contains the script you created in step 1.</span></span>  
4. <span data-ttu-id="eeb58-145">Futtassa a szkriptet a kívánt konfigurációhoz szükséges paraméterértékekkel.</span><span class="sxs-lookup"><span data-stu-id="eeb58-145">Execute the script by using the parameter values for the configuration you require.</span></span>

    <span data-ttu-id="eeb58-146">**Futtató fiók létrehozása önaláírt tanúsítvány használatával**</span><span class="sxs-lookup"><span data-stu-id="eeb58-146">**Create a Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    <span data-ttu-id="eeb58-147">**Futtató fiók és klasszikus futtató fiók létrehozása önaláírt tanúsítvány használatával**</span><span class="sxs-lookup"><span data-stu-id="eeb58-147">**Create a Run As account and a Classic Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    <span data-ttu-id="eeb58-148">**Futtató fiók és klasszikus futtató fiók létrehozása vállalati tanúsítvány használatával**</span><span class="sxs-lookup"><span data-stu-id="eeb58-148">**Create a Run As account and a Classic Run As account by using an enterprise certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    <span data-ttu-id="eeb58-149">**Futtató fiók és klasszikus futtató fiók létrehozása az Azure Government Cloud egyik önaláírt tanúsítványának használatával**</span><span class="sxs-lookup"><span data-stu-id="eeb58-149">**Create a Run As account and a Classic Run As account by using a self-signed certificate in the Azure Government cloud**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > <span data-ttu-id="eeb58-150">A rendszer az Azure-ral történő hitelesítést fogja kérni a szkript futtatása után.</span><span class="sxs-lookup"><span data-stu-id="eeb58-150">After the script has executed, you will be prompted to authenticate with Azure.</span></span> <span data-ttu-id="eeb58-151">Jelentkezzen be egy olyan fiókkal, amely tagja az Előfizetés-adminisztrátorok szerepkörnek, és emellett az előfizetés társadminisztrátorának is számít.</span><span class="sxs-lookup"><span data-stu-id="eeb58-151">Sign in with an account that is a member of the subscription administrators role and co-administrator of the subscription.</span></span>
    >
    >

<span data-ttu-id="eeb58-152">A szkript sikeres futtatása után jegyezze fel a következőket:</span><span class="sxs-lookup"><span data-stu-id="eeb58-152">After the script has executed successfully, note the following:</span></span>
* <span data-ttu-id="eeb58-153">Ha önaláírt nyilvános tanúsítvánnyal (.cer fájl) ellátott klasszikus futtató fiókot hoz létre, a szkript létrehozza és menti a tanúsítványt a számítógépen a PowerShell-munkamenet végrehajtásához használt *%USERPROFILE%\AppData\Local\Temp* felhasználói profilhoz tartozó ideiglenes mappába.</span><span class="sxs-lookup"><span data-stu-id="eeb58-153">If you created a Classic Run As account with a self-signed public certificate (.cer file), the script creates and saves it to the temporary files folder on your computer under the user profile *%USERPROFILE%\AppData\Local\Temp*, which you used to execute the PowerShell session.</span></span>
* <span data-ttu-id="eeb58-154">Ha egy (.cer formátumú) vállalati tanúsítvánnyal rendelkező klasszikus futtató fiókot hozott létre, használja ezt a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="eeb58-154">If you created a Classic Run As account with an enterprise public certificate (.cer file), use this certificate.</span></span> <span data-ttu-id="eeb58-155">Kövesse az utasításokat, amelyek bemutatják [a felügyeleti API-tanúsítványok klasszikus Azure portálra való feltöltését](../azure-api-management-certs.md), majd ellenőrizze a hitelesítő adatok konfigurációját a klasszikus üzembe helyezési erőforrásokkal. Ehhez használja a [klasszikus Azure üzembe helyezési modell erőforrásaival való hitelesítésre szolgáló mintakódot](automation-verify-runas-authentication.md#classic-run-as-authentication).</span><span class="sxs-lookup"><span data-stu-id="eeb58-155">Follow the instructions for [uploading a management API certificate to the Azure classic portal](../azure-api-management-certs.md), and then validate the credential configuration with classic deployment resources by using the [sample code to authenticate with Azure Classic Deployment Resources](automation-verify-runas-authentication.md#classic-run-as-authentication).</span></span> 
* <span data-ttu-id="eeb58-156">Ha *nem* klasszikus futtató fiókot hozott létre, állítsa be a hitelesítést a Resource Manager-erőforrásokkal, valamint ellenőrizze a hitelesítő adatok konfigurációját. Ehhez használja a [Service Management-erőforrásokkal való hitelesítéshez használt mintakódot.](automation-verify-runas-authentication.md#automation-run-as-authentication)</span><span class="sxs-lookup"><span data-stu-id="eeb58-156">If you did *not* create a Classic Run As account, authenticate with Resource Manager resources and validate the credential configuration by using the [sample code for authenticating with Service Management resources](automation-verify-runas-authentication.md#automation-run-as-authentication).</span></span>

## <a name="next-steps"></a><span data-ttu-id="eeb58-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="eeb58-157">Next steps</span></span>
* <span data-ttu-id="eeb58-158">Az Egyszerű szolgáltatásokkal kapcsolatos további információkért lásd: [Alkalmazásobjektumok és egyszerű szolgáltatási objektumok](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="eeb58-158">For more information about Service Principals, refer to [Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md).</span></span>
* <span data-ttu-id="eeb58-159">A tanúsítványokkal és az Azure-szolgáltatásokkal kapcsolatos részletes információkért lásd: [Tanúsítványok áttekintése az Azure Cloud Servicesben](../cloud-services/cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="eeb58-159">For more information about certificates and Azure services, refer to [Certificates overview for Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).</span></span>
