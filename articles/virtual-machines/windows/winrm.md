---
title: "a Rendszerfelügyeleti webszolgáltatások hozzáférés beállítása az Azure virtuális gép aaaSet |} Microsoft Docs"
description: "A telepítő a Rendszerfelügyeleti webszolgáltatások hozzáférési használatra hello Resource Manager üzembe helyezési modellel létrehozott Azure virtuális gép a."
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
ms.openlocfilehash: 23d1d3a3065cbd8e4036be085c6d835cae36caae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-winrm-access-for-virtual-machines-in-azure-resource-manager"></a><span data-ttu-id="69c0d-103">A Rendszerfelügyeleti webszolgáltatások hozzáférés beállítása az Azure Resource Manager virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="69c0d-103">Setting up WinRM access for Virtual Machines in Azure Resource Manager</span></span>
## <a name="winrm-in-azure-service-management-vs-azure-resource-manager"></a><span data-ttu-id="69c0d-104">A Rendszerfelügyeleti webszolgáltatások az Azure szolgáltatásfelügyelet vs Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="69c0d-104">WinRM in Azure Service Management vs Azure Resource Manager</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

* <span data-ttu-id="69c0d-105">Hello Azure Resource Manager áttekintését lásd: a [cikk](../../azure-resource-manager/resource-group-overview.md)</span><span class="sxs-lookup"><span data-stu-id="69c0d-105">For an overview of hello Azure Resource Manager, please see this [article](../../azure-resource-manager/resource-group-overview.md)</span></span>
* <span data-ttu-id="69c0d-106">Azure Service Management és az Azure Resource Manager közötti különbségek miatt, lásd: a [cikk](../../resource-manager-deployment-model.md)</span><span class="sxs-lookup"><span data-stu-id="69c0d-106">For differences between Azure Service Management and Azure Resource Manager, please see this [article](../../resource-manager-deployment-model.md)</span></span>

<span data-ttu-id="69c0d-107">hello WinRM konfiguráció hello két verem közötti fő különbség hogyan hello tanúsítvány lekérdezi telepítve hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="69c0d-107">hello key difference in setting up WinRM configuration between hello two stacks is how hello certificate gets installed on hello VM.</span></span> <span data-ttu-id="69c0d-108">Hello Azure Resource Manager-készletben hello tanúsítványok hello Key Vault erőforrás-szolgáltató által kezelt erőforrások van modellezve.</span><span class="sxs-lookup"><span data-stu-id="69c0d-108">In hello Azure Resource Manager stack, hello certificates are modeled as resources managed by hello Key Vault Resource Provider.</span></span> <span data-ttu-id="69c0d-109">Ezért hello felhasználói tooprovide saját tanúsítványt kell, és töltse fel tooa Key Vault virtuális gépen használatához.</span><span class="sxs-lookup"><span data-stu-id="69c0d-109">Therefore, hello user needs tooprovide their own certificate and upload it tooa Key Vault before using it in a VM.</span></span>

<span data-ttu-id="69c0d-110">Az alábbiakban a Rendszerfelügyeleti webszolgáltatások kapcsolattal rendelkező virtuális gépet tootake tooset kell hello lépéseket</span><span class="sxs-lookup"><span data-stu-id="69c0d-110">Here are hello steps you need tootake tooset up a VM with WinRM connectivity</span></span>

1. <span data-ttu-id="69c0d-111">Kulcstartó létrehozása</span><span class="sxs-lookup"><span data-stu-id="69c0d-111">Create a Key Vault</span></span>
2. <span data-ttu-id="69c0d-112">Önaláírt tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="69c0d-112">Create a self-signed certificate</span></span>
3. <span data-ttu-id="69c0d-113">Az önaláírt tanúsítvány tooKey tároló feltöltése</span><span class="sxs-lookup"><span data-stu-id="69c0d-113">Upload your self-signed certificate tooKey Vault</span></span>
4. <span data-ttu-id="69c0d-114">A Key Vault hello önaláírt tanúsítványt hello URL-cím lekérése</span><span class="sxs-lookup"><span data-stu-id="69c0d-114">Get hello URL for your self-signed certificate in hello Key Vault</span></span>
5. <span data-ttu-id="69c0d-115">Az önaláírt tanúsítványok URL-CÍMÉRE hivatkozik egy virtuális gép létrehozása közben</span><span class="sxs-lookup"><span data-stu-id="69c0d-115">Reference your self-signed certificates URL while creating a VM</span></span>

## <a name="step-1-create-a-key-vault"></a><span data-ttu-id="69c0d-116">1. lépés:, Hozzon létre egy kulcstartót</span><span class="sxs-lookup"><span data-stu-id="69c0d-116">Step 1: Create a Key Vault</span></span>
<span data-ttu-id="69c0d-117">Használhatja a hello parancs toocreate hello Key Vault alatt</span><span class="sxs-lookup"><span data-stu-id="69c0d-117">You can use hello below command toocreate hello Key Vault</span></span>

```
New-AzureRmKeyVault -VaultName "<vault-name>" -ResourceGroupName "<rg-name>" -Location "<vault-location>" -EnabledForDeployment -EnabledForTemplateDeployment
```

## <a name="step-2-create-a-self-signed-certificate"></a><span data-ttu-id="69c0d-118">2. lépés:, Hozzon létre egy önaláírt tanúsítványt</span><span class="sxs-lookup"><span data-stu-id="69c0d-118">Step 2: Create a self-signed certificate</span></span>
<span data-ttu-id="69c0d-119">Létrehozhat egy önaláírt tanúsítványt, a PowerShell-parancsfájl használatával</span><span class="sxs-lookup"><span data-stu-id="69c0d-119">You can create a self-signed certificate using this PowerShell script</span></span>

```
$certificateName = "somename"

$thumbprint = (New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation Cert:\CurrentUser\My -KeySpec KeyExchange).Thumbprint

$cert = (Get-ChildItem -Path cert:\CurrentUser\My\$thumbprint)

$password = Read-Host -Prompt "Please enter hello certificate password." -AsSecureString

Export-PfxCertificate -Cert $cert -FilePath ".\$certificateName.pfx" -Password $password
```

## <a name="step-3-upload-your-self-signed-certificate-toohello-key-vault"></a><span data-ttu-id="69c0d-120">3. lépés: Töltse fel az önaláírt tanúsítvány toohello Key Vault</span><span class="sxs-lookup"><span data-stu-id="69c0d-120">Step 3: Upload your self-signed certificate toohello Key Vault</span></span>
<span data-ttu-id="69c0d-121">Az 1. lépésben feltöltése hello tanúsítvány toohello kulcstároló létrehozása előtt a Microsoft.Compute erőforrás-szolgáltató megértenek formátum hello be kell tooconverted.</span><span class="sxs-lookup"><span data-stu-id="69c0d-121">Before uploading hello certificate toohello Key Vault created in step 1, it needs tooconverted into a format hello Microsoft.Compute resource provider will understand.</span></span> <span data-ttu-id="69c0d-122">hello alábbi PowerShell-parancsfájl lehetővé teszi, így tesz, amely</span><span class="sxs-lookup"><span data-stu-id="69c0d-122">hello below PowerShell script will allow you do that</span></span>

```
$fileName = "<Path toohello .pfx file>"
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

## <a name="step-4-get-hello-url-for-your-self-signed-certificate-in-hello-key-vault"></a><span data-ttu-id="69c0d-123">4. lépés: Hello URL-cím beszerzése a Key Vault hello önaláírt tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="69c0d-123">Step 4: Get hello URL for your self-signed certificate in hello Key Vault</span></span>
<span data-ttu-id="69c0d-124">hello Microsoft.Compute erőforrás-szolgáltató URL-cím toohello titkos kulcs hello Key Vault belül kell hello virtuális gép kiépítése során.</span><span class="sxs-lookup"><span data-stu-id="69c0d-124">hello Microsoft.Compute resource provider needs a URL toohello secret inside hello Key Vault while provisioning hello VM.</span></span> <span data-ttu-id="69c0d-125">Ez lehetővé teszi, hogy a hello Microsoft.Compute erőforrás szolgáltató toodownload hello titkos kulcsot, és hozzon létre hello egyenértékű tanúsítvány hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="69c0d-125">This enables hello Microsoft.Compute resource provider toodownload hello secret and create hello equivalent certificate on hello VM.</span></span>

> [!NOTE]
> <span data-ttu-id="69c0d-126">hello titkos hello URL-címe tooinclude hello verziója is kell.</span><span class="sxs-lookup"><span data-stu-id="69c0d-126">hello URL of hello secret needs tooinclude hello version as well.</span></span> <span data-ttu-id="69c0d-127">Egy példa URL-CÍMÉT a következőképpen néz https://contosovault.vault.azure.net:443/titkos kulcsok/contososecret/01h9db0df2cd4300a20ence585a6s7ve alatt</span><span class="sxs-lookup"><span data-stu-id="69c0d-127">An example URL looks like below https://contosovault.vault.azure.net:443/secrets/contososecret/01h9db0df2cd4300a20ence585a6s7ve</span></span>
> 
> 

#### <a name="templates"></a><span data-ttu-id="69c0d-128">Sablonok</span><span class="sxs-lookup"><span data-stu-id="69c0d-128">Templates</span></span>
<span data-ttu-id="69c0d-129">Hello hivatkozás toohello URL-CÍMÉT használja az alábbi kód hello hello sablon olvashatók be</span><span class="sxs-lookup"><span data-stu-id="69c0d-129">You can get hello link toohello URL in hello template using hello below code</span></span>

    "certificateUrl": "[reference(resourceId(resourceGroup().name, 'Microsoft.KeyVault/vaults/secrets', '<vault-name>', '<secret-name>'), '2015-06-01').secretUriWithVersion]"

#### <a name="powershell"></a><span data-ttu-id="69c0d-130">PowerShell</span><span class="sxs-lookup"><span data-stu-id="69c0d-130">PowerShell</span></span>
<span data-ttu-id="69c0d-131">Az URL-cím hello alábbi PowerShell-parancs használatával beszerezheti</span><span class="sxs-lookup"><span data-stu-id="69c0d-131">You can get this URL using hello below PowerShell command</span></span>

    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id

## <a name="step-5-reference-your-self-signed-certificates-url-while-creating-a-vm"></a><span data-ttu-id="69c0d-132">5. lépés: Az önaláírt tanúsítványok URL-CÍMÉRE hivatkozik egy virtuális gép létrehozása közben</span><span class="sxs-lookup"><span data-stu-id="69c0d-132">Step 5: Reference your self-signed certificates URL while creating a VM</span></span>
#### <a name="azure-resource-manager-templates"></a><span data-ttu-id="69c0d-133">Az Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="69c0d-133">Azure Resource Manager Templates</span></span>
<span data-ttu-id="69c0d-134">A sablonok használatával virtuális gépek létrehozásakor hello titkok és hello winRM szakaszát, az alábbi hello tanúsítvány lekérdezi hivatkozik:</span><span class="sxs-lookup"><span data-stu-id="69c0d-134">While creating a VM through templates, hello certificate gets referenced in hello secrets section and hello winRM section as below:</span></span>

    "osProfile": {
          ...
          "secrets": [
            {
              "sourceVault": {
                "id": "<resource id of hello Key Vault containing hello secret>"
              },
              "vaultCertificates": [
                {
                  "certificateUrl": "<URL for hello certificate you got in Step 4>",
                  "certificateStore": "<Name of hello certificate store on hello VM>"
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
                  "certificateUrl": "<URL for hello certificate you got in Step 4>"
                }
              ]
            },
            ...
          }
        },

<span data-ttu-id="69c0d-135">A fenti hello mintasablon itt található [201-vm-winrm-keyvault-windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows)</span><span class="sxs-lookup"><span data-stu-id="69c0d-135">A sample template for hello above can be found here at [201-vm-winrm-keyvault-windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows)</span></span>

<span data-ttu-id="69c0d-136">Ez a sablon forráskódja található [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)</span><span class="sxs-lookup"><span data-stu-id="69c0d-136">Source code for this template can be found on [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)</span></span>

#### <a name="powershell"></a><span data-ttu-id="69c0d-137">PowerShell</span><span class="sxs-lookup"><span data-stu-id="69c0d-137">PowerShell</span></span>
    $vm = New-AzureRmVMConfig -VMName "<VM name>" -VMSize "<VM Size>"
    $credential = Get-Credential
    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName "<Computer Name>" -Credential $credential -WinRMHttp -WinRMHttps -WinRMCertificateUrl $secretURL
    $sourceVaultId = (Get-AzureRmKeyVault -ResourceGroupName "<Resource Group name>" -VaultName "<Vault Name>").ResourceId
    $CertificateStore = "My"
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore $CertificateStore -CertificateUrl $secretURL

## <a name="step-6-connecting-toohello-vm"></a><span data-ttu-id="69c0d-138">6. lépés: Csatlakozás toohello méretű VM</span><span class="sxs-lookup"><span data-stu-id="69c0d-138">Step 6: Connecting toohello VM</span></span>
<span data-ttu-id="69c0d-139">Mielőtt az csatlakozna toohello VM toomake meg arról, hogy a számítógép úgy van konfigurálva a WinRM Távfelügyelet lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="69c0d-139">Before you can connect toohello VM you'll need toomake sure your machine is configured for WinRM remote management.</span></span> <span data-ttu-id="69c0d-140">Indítsa el a Powershellt rendszergazdaként, majd hajtsa végre az alábbi parancs toomake meg arról, hogy elkészült a beállítással hello.</span><span class="sxs-lookup"><span data-stu-id="69c0d-140">Start PowerShell as an administrator and execute hello below command toomake sure you're set up.</span></span>

    Enable-PSRemoting -Force

> [!NOTE]
> <span data-ttu-id="69c0d-141">Szükség lehet toomake, hello WinRM szolgáltatás fut, ha a fenti hello nem működik.</span><span class="sxs-lookup"><span data-stu-id="69c0d-141">You might need toomake sure hello WinRM service is running if hello above does not work.</span></span> <span data-ttu-id="69c0d-142">Megteheti, hogy használatával`Get-Service WinRM`</span><span class="sxs-lookup"><span data-stu-id="69c0d-142">You can do that using `Get-Service WinRM`</span></span>
> 
> 

<span data-ttu-id="69c0d-143">Ha hello telepítő végzett, a kapcsolódás toohello VM hello alábbi parancs használatával</span><span class="sxs-lookup"><span data-stu-id="69c0d-143">Once hello setup is done, you can connect toohello VM using hello below command</span></span>

    Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate
