---
title: "Egy Azure virtuális Gépen a Rendszerfelügyeleti webszolgáltatások hozzáférési beállítása |} Microsoft Docs"
description: "A telepítő a Rendszerfelügyeleti webszolgáltatások hozzáférési használatra a Resource Manager üzembe helyezési modellel létrehozott Azure virtuális gép a."
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 9718e85b-d360-4621-90b8-0b0b84a21208
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/16/2016
ms.author: kasing
ms.openlocfilehash: 2d6533462400bc1d93d0d3b0227769784e2658a9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="setting-up-winrm-access-for-virtual-machines-in-azure-resource-manager"></a><span data-ttu-id="e2a06-103">A Rendszerfelügyeleti webszolgáltatások hozzáférés beállítása az Azure Resource Manager virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="e2a06-103">Setting up WinRM access for Virtual Machines in Azure Resource Manager</span></span>
## <a name="winrm-in-azure-service-management-vs-azure-resource-manager"></a><span data-ttu-id="e2a06-104">A Rendszerfelügyeleti webszolgáltatások az Azure szolgáltatásfelügyelet vs Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="e2a06-104">WinRM in Azure Service Management vs Azure Resource Manager</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

* <span data-ttu-id="e2a06-105">Az Azure Resource Manager áttekintéséért lásd: a [cikk](../../azure-resource-manager/resource-group-overview.md)</span><span class="sxs-lookup"><span data-stu-id="e2a06-105">For an overview of the Azure Resource Manager, please see this [article](../../azure-resource-manager/resource-group-overview.md)</span></span>
* <span data-ttu-id="e2a06-106">Azure Service Management és az Azure Resource Manager közötti különbségek miatt, lásd: a [cikk](../../resource-manager-deployment-model.md)</span><span class="sxs-lookup"><span data-stu-id="e2a06-106">For differences between Azure Service Management and Azure Resource Manager, please see this [article](../../resource-manager-deployment-model.md)</span></span>

<span data-ttu-id="e2a06-107">A kulcs WinRM konfiguráció a két verem közötti különbség hogyan telepíti a tanúsítványt a virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="e2a06-107">The key difference in setting up WinRM configuration between the two stacks is how the certificate gets installed on the VM.</span></span> <span data-ttu-id="e2a06-108">Az Azure Resource Manager-készletben a tanúsítványok a Key Vault erőforrás-szolgáltató által kezelt erőforrások van modellezve.</span><span class="sxs-lookup"><span data-stu-id="e2a06-108">In the Azure Resource Manager stack, the certificates are modeled as resources managed by the Key Vault Resource Provider.</span></span> <span data-ttu-id="e2a06-109">Ezért a felhasználó adja meg a saját tanúsítványt, és töltse fel azt a kulcstároló virtuális gépen használatához szükséges.</span><span class="sxs-lookup"><span data-stu-id="e2a06-109">Therefore, the user needs to provide their own certificate and upload it to a Key Vault before using it in a VM.</span></span>

<span data-ttu-id="e2a06-110">Az alábbiakban a lépéseket kell tennie egy virtuális gép beállítása a Rendszerfelügyeleti webszolgáltatások kapcsolatban</span><span class="sxs-lookup"><span data-stu-id="e2a06-110">Here are the steps you need to take to set up a VM with WinRM connectivity</span></span>

1. <span data-ttu-id="e2a06-111">Kulcstartó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e2a06-111">Create a Key Vault</span></span>
2. <span data-ttu-id="e2a06-112">Önaláírt tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="e2a06-112">Create a self-signed certificate</span></span>
3. <span data-ttu-id="e2a06-113">Key Vault a önaláírt tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="e2a06-113">Upload your self-signed certificate to Key Vault</span></span>
4. <span data-ttu-id="e2a06-114">URL beolvasása a Key Vault az önaláírt tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="e2a06-114">Get the URL for your self-signed certificate in the Key Vault</span></span>
5. <span data-ttu-id="e2a06-115">Az önaláírt tanúsítványok URL-CÍMÉRE hivatkozik egy virtuális gép létrehozása közben</span><span class="sxs-lookup"><span data-stu-id="e2a06-115">Reference your self-signed certificates URL while creating a VM</span></span>

## <a name="step-1-create-a-key-vault"></a><span data-ttu-id="e2a06-116">1. lépés:, Hozzon létre egy kulcstartót</span><span class="sxs-lookup"><span data-stu-id="e2a06-116">Step 1: Create a Key Vault</span></span>
<span data-ttu-id="e2a06-117">Használhatja az alábbi parancsot a kulcstároló létrehozásához</span><span class="sxs-lookup"><span data-stu-id="e2a06-117">You can use the below command to create the Key Vault</span></span>

```
New-AzureRmKeyVault -VaultName "<vault-name>" -ResourceGroupName "<rg-name>" -Location "<vault-location>" -EnabledForDeployment -EnabledForTemplateDeployment
```

## <a name="step-2-create-a-self-signed-certificate"></a><span data-ttu-id="e2a06-118">2. lépés:, Hozzon létre egy önaláírt tanúsítványt</span><span class="sxs-lookup"><span data-stu-id="e2a06-118">Step 2: Create a self-signed certificate</span></span>
<span data-ttu-id="e2a06-119">Létrehozhat egy önaláírt tanúsítványt, a PowerShell-parancsfájl használatával</span><span class="sxs-lookup"><span data-stu-id="e2a06-119">You can create a self-signed certificate using this PowerShell script</span></span>

```
$certificateName = "somename"

$thumbprint = (New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation Cert:\CurrentUser\My -KeySpec KeyExchange).Thumbprint

$cert = (Get-ChildItem -Path cert:\CurrentUser\My\$thumbprint)

$password = Read-Host -Prompt "Please enter the certificate password." -AsSecureString

Export-PfxCertificate -Cert $cert -FilePath ".\$certificateName.pfx" -Password $password
```

## <a name="step-3-upload-your-self-signed-certificate-to-the-key-vault"></a><span data-ttu-id="e2a06-120">3. lépés: A Key Vault a önaláírt tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="e2a06-120">Step 3: Upload your self-signed certificate to the Key Vault</span></span>
<span data-ttu-id="e2a06-121">A tanúsítvány feltöltése a Key Vault az 1. lépésben létrehozott, előtt kell a Microsoft.Compute erőforrás-szolgáltató megértenek olyan formátumra alakítja át.</span><span class="sxs-lookup"><span data-stu-id="e2a06-121">Before uploading the certificate to the Key Vault created in step 1, it needs to converted into a format the Microsoft.Compute resource provider will understand.</span></span> <span data-ttu-id="e2a06-122">Az alábbi PowerShell parancsfájl lehetővé teheti meg, amely</span><span class="sxs-lookup"><span data-stu-id="e2a06-122">The below PowerShell script will allow you do that</span></span>

```
$fileName = "<Path to the .pfx file>"
$fileContentBytes = Get-Content $fileName -Encoding Byte
$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

$jsonObject = @"
{
  "data": "$filecontentencoded",
  "dataType" :"pfx",
  "password": "<password>"
}
"@

$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText –Force
Set-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>" -SecretValue $secret
```

## <a name="step-4-get-the-url-for-your-self-signed-certificate-in-the-key-vault"></a><span data-ttu-id="e2a06-123">4. lépés: Az URL-cím beszerzése a Key Vault az önaláírt tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="e2a06-123">Step 4: Get the URL for your self-signed certificate in the Key Vault</span></span>
<span data-ttu-id="e2a06-124">A Microsoft.Compute erőforrás-szolgáltató a titkos kulcsot belül a Key Vault URL-CÍMÉT kell a virtuális gép kiépítése során.</span><span class="sxs-lookup"><span data-stu-id="e2a06-124">The Microsoft.Compute resource provider needs a URL to the secret inside the Key Vault while provisioning the VM.</span></span> <span data-ttu-id="e2a06-125">Ez lehetővé teszi a Microsoft.Compute erőforrás-szolgáltató töltse le a titkos kulcsot, és hozzon létre a megfelelő tanúsítványt a virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="e2a06-125">This enables the Microsoft.Compute resource provider to download the secret and create the equivalent certificate on the VM.</span></span>

> [!NOTE]
> <span data-ttu-id="e2a06-126">Az URL-CÍMÉT a titkos kulcsot kell tartalmaznia, valamint a verzió.</span><span class="sxs-lookup"><span data-stu-id="e2a06-126">The URL of the secret needs to include the version as well.</span></span> <span data-ttu-id="e2a06-127">Egy példa URL-CÍMÉT a következőképpen néz https://contosovault.vault.azure.net:443/titkos kulcsok/contososecret/01h9db0df2cd4300a20ence585a6s7ve alatt</span><span class="sxs-lookup"><span data-stu-id="e2a06-127">An example URL looks like below https://contosovault.vault.azure.net:443/secrets/contososecret/01h9db0df2cd4300a20ence585a6s7ve</span></span>
> 
> 

#### <a name="templates"></a><span data-ttu-id="e2a06-128">Sablonok</span><span class="sxs-lookup"><span data-stu-id="e2a06-128">Templates</span></span>
<span data-ttu-id="e2a06-129">A hivatkozásra kattintva az URL-címet a sablon használatával kaphat az alábbi kód</span><span class="sxs-lookup"><span data-stu-id="e2a06-129">You can get the link to the URL in the template using the below code</span></span>

    "certificateUrl": "[reference(resourceId(resourceGroup().name, 'Microsoft.KeyVault/vaults/secrets', '<vault-name>', '<secret-name>'), '2015-06-01').secretUriWithVersion]"

#### <a name="powershell"></a><span data-ttu-id="e2a06-130">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e2a06-130">PowerShell</span></span>
<span data-ttu-id="e2a06-131">Az URL-cím segítségével is ki az alábbi PowerShell-parancs</span><span class="sxs-lookup"><span data-stu-id="e2a06-131">You can get this URL using the below PowerShell command</span></span>

    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id

## <a name="step-5-reference-your-self-signed-certificates-url-while-creating-a-vm"></a><span data-ttu-id="e2a06-132">5. lépés: Az önaláírt tanúsítványok URL-CÍMÉRE hivatkozik egy virtuális gép létrehozása közben</span><span class="sxs-lookup"><span data-stu-id="e2a06-132">Step 5: Reference your self-signed certificates URL while creating a VM</span></span>
#### <a name="azure-resource-manager-templates"></a><span data-ttu-id="e2a06-133">Az Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="e2a06-133">Azure Resource Manager Templates</span></span>
<span data-ttu-id="e2a06-134">A sablonok használatával virtuális gépek létrehozásakor, a titkos kulcsok és a Rendszerfelügyeleti webszolgáltatások szakaszát, az alábbi a tanúsítvány lekérdezi hivatkozik:</span><span class="sxs-lookup"><span data-stu-id="e2a06-134">While creating a VM through templates, the certificate gets referenced in the secrets section and the winRM section as below:</span></span>

    "osProfile": {
          ...
          "secrets": [
            {
              "sourceVault": {
                "id": "<resource id of the Key Vault containing the secret>"
              },
              "vaultCertificates": [
                {
                  "certificateUrl": "<URL for the certificate you got in Step 4>",
                  "certificateStore": "<Name of the certificate store on the VM>"
                }
              ]
            }
          ],
          "windowsConfiguration": {
            ...
            "winRM": {
              "listeners": [
                {
                  "protocol": "http"
                },
                {
                  "protocol": "https",
                  "certificateUrl": "<URL for the certificate you got in Step 4>"
                }
              ]
            },
            ...
          }
        },

<span data-ttu-id="e2a06-135">A fenti példa sablonját itt található [201-vm-winrm-keyvault-windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows)</span><span class="sxs-lookup"><span data-stu-id="e2a06-135">A sample template for the above can be found here at [201-vm-winrm-keyvault-windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows)</span></span>

<span data-ttu-id="e2a06-136">Ez a sablon forráskódja található [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)</span><span class="sxs-lookup"><span data-stu-id="e2a06-136">Source code for this template can be found on [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)</span></span>

#### <a name="powershell"></a><span data-ttu-id="e2a06-137">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e2a06-137">PowerShell</span></span>
    $vm = New-AzureRmVMConfig -VMName "<VM name>" -VMSize "<VM Size>"
    $credential = Get-Credential
    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName "<Computer Name>" -Credential $credential -WinRMHttp -WinRMHttps -WinRMCertificateUrl $secretURL
    $sourceVaultId = (Get-AzureRmKeyVault -ResourceGroupName "<Resource Group name>" -VaultName "<Vault Name>").ResourceId
    $CertificateStore = "My"
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore $CertificateStore -CertificateUrl $secretURL

## <a name="step-6-connecting-to-the-vm"></a><span data-ttu-id="e2a06-138">6. lépés: Csatlakozzon a virtuális Géphez</span><span class="sxs-lookup"><span data-stu-id="e2a06-138">Step 6: Connecting to the VM</span></span>
<span data-ttu-id="e2a06-139">Mielőtt az csatlakozna a virtuális gépre lesz szüksége győződjön meg arról, hogy a gép a WinRM Távfelügyelet van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="e2a06-139">Before you can connect to the VM you'll need to make sure your machine is configured for WinRM remote management.</span></span> <span data-ttu-id="e2a06-140">Indítsa el a Powershellt rendszergazdaként, majd hajtsa végre az alábbi parancs futtatásával ellenőrizze, hogy elkészült a beállítással.</span><span class="sxs-lookup"><span data-stu-id="e2a06-140">Start PowerShell as an administrator and execute the below command to make sure you're set up.</span></span>

    Enable-PSRemoting -Force

> [!NOTE]
> <span data-ttu-id="e2a06-141">Szükség lehet győződjön meg arról, hogy a WinRM szolgáltatás fut, ha a fenti nem működik.</span><span class="sxs-lookup"><span data-stu-id="e2a06-141">You might need to make sure the WinRM service is running if the above does not work.</span></span> <span data-ttu-id="e2a06-142">Megteheti, hogy használatával`Get-Service WinRM`</span><span class="sxs-lookup"><span data-stu-id="e2a06-142">You can do that using `Get-Service WinRM`</span></span>
> 
> 

<span data-ttu-id="e2a06-143">Ha a telepítő végzett, csatlakozhat a virtuális gép használja az alábbi parancs</span><span class="sxs-lookup"><span data-stu-id="e2a06-143">Once the setup is done, you can connect to the VM using the below command</span></span>

    Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate
