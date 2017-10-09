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
# <a name="setting-up-winrm-access-for-virtual-machines-in-azure-resource-manager"></a>A Rendszerfelügyeleti webszolgáltatások hozzáférés beállítása az Azure Resource Manager virtuális gépekhez
## <a name="winrm-in-azure-service-management-vs-azure-resource-manager"></a>A Rendszerfelügyeleti webszolgáltatások az Azure szolgáltatásfelügyelet vs Azure Resource Manager

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

* Hello Azure Resource Manager áttekintését lásd: a [cikk](../../azure-resource-manager/resource-group-overview.md)
* Azure Service Management és az Azure Resource Manager közötti különbségek miatt, lásd: a [cikk](../../resource-manager-deployment-model.md)

hello WinRM konfiguráció hello két verem közötti fő különbség hogyan hello tanúsítvány lekérdezi telepítve hello virtuális gép. Hello Azure Resource Manager-készletben hello tanúsítványok hello Key Vault erőforrás-szolgáltató által kezelt erőforrások van modellezve. Ezért hello felhasználói tooprovide saját tanúsítványt kell, és töltse fel tooa Key Vault virtuális gépen használatához.

Az alábbiakban a Rendszerfelügyeleti webszolgáltatások kapcsolattal rendelkező virtuális gépet tootake tooset kell hello lépéseket

1. Kulcstartó létrehozása
2. Önaláírt tanúsítvány létrehozása
3. Az önaláírt tanúsítvány tooKey tároló feltöltése
4. A Key Vault hello önaláírt tanúsítványt hello URL-cím lekérése
5. Az önaláírt tanúsítványok URL-CÍMÉRE hivatkozik egy virtuális gép létrehozása közben

## <a name="step-1-create-a-key-vault"></a>1. lépés:, Hozzon létre egy kulcstartót
Használhatja a hello parancs toocreate hello Key Vault alatt

```
New-AzureRmKeyVault -VaultName "<vault-name>" -ResourceGroupName "<rg-name>" -Location "<vault-location>" -EnabledForDeployment -EnabledForTemplateDeployment
```

## <a name="step-2-create-a-self-signed-certificate"></a>2. lépés:, Hozzon létre egy önaláírt tanúsítványt
Létrehozhat egy önaláírt tanúsítványt, a PowerShell-parancsfájl használatával

```
$certificateName = "somename"

$thumbprint = (New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation Cert:\CurrentUser\My -KeySpec KeyExchange).Thumbprint

$cert = (Get-ChildItem -Path cert:\CurrentUser\My\$thumbprint)

$password = Read-Host -Prompt "Please enter hello certificate password." -AsSecureString

Export-PfxCertificate -Cert $cert -FilePath ".\$certificateName.pfx" -Password $password
```

## <a name="step-3-upload-your-self-signed-certificate-toohello-key-vault"></a>3. lépés: Töltse fel az önaláírt tanúsítvány toohello Key Vault
Az 1. lépésben feltöltése hello tanúsítvány toohello kulcstároló létrehozása előtt a Microsoft.Compute erőforrás-szolgáltató megértenek formátum hello be kell tooconverted. hello alábbi PowerShell-parancsfájl lehetővé teszi, így tesz, amely

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

## <a name="step-4-get-hello-url-for-your-self-signed-certificate-in-hello-key-vault"></a>4. lépés: Hello URL-cím beszerzése a Key Vault hello önaláírt tanúsítvány
hello Microsoft.Compute erőforrás-szolgáltató URL-cím toohello titkos kulcs hello Key Vault belül kell hello virtuális gép kiépítése során. Ez lehetővé teszi, hogy a hello Microsoft.Compute erőforrás szolgáltató toodownload hello titkos kulcsot, és hozzon létre hello egyenértékű tanúsítvány hello virtuális gép.

> [!NOTE]
> hello titkos hello URL-címe tooinclude hello verziója is kell. Egy példa URL-CÍMÉT a következőképpen néz https://contosovault.vault.azure.net:443/titkos kulcsok/contososecret/01h9db0df2cd4300a20ence585a6s7ve alatt
> 
> 

#### <a name="templates"></a>Sablonok
Hello hivatkozás toohello URL-CÍMÉT használja az alábbi kód hello hello sablon olvashatók be

    "certificateUrl": "[reference(resourceId(resourceGroup().name, 'Microsoft.KeyVault/vaults/secrets', '<vault-name>', '<secret-name>'), '2015-06-01').secretUriWithVersion]"

#### <a name="powershell"></a>PowerShell
Az URL-cím hello alábbi PowerShell-parancs használatával beszerezheti

    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id

## <a name="step-5-reference-your-self-signed-certificates-url-while-creating-a-vm"></a>5. lépés: Az önaláírt tanúsítványok URL-CÍMÉRE hivatkozik egy virtuális gép létrehozása közben
#### <a name="azure-resource-manager-templates"></a>Az Azure Resource Manager-sablonok
A sablonok használatával virtuális gépek létrehozásakor hello titkok és hello winRM szakaszát, az alábbi hello tanúsítvány lekérdezi hivatkozik:

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

A fenti hello mintasablon itt található [201-vm-winrm-keyvault-windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows)

Ez a sablon forráskódja található [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)

#### <a name="powershell"></a>PowerShell
    $vm = New-AzureRmVMConfig -VMName "<VM name>" -VMSize "<VM Size>"
    $credential = Get-Credential
    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName "<Computer Name>" -Credential $credential -WinRMHttp -WinRMHttps -WinRMCertificateUrl $secretURL
    $sourceVaultId = (Get-AzureRmKeyVault -ResourceGroupName "<Resource Group name>" -VaultName "<Vault Name>").ResourceId
    $CertificateStore = "My"
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore $CertificateStore -CertificateUrl $secretURL

## <a name="step-6-connecting-toohello-vm"></a>6. lépés: Csatlakozás toohello méretű VM
Mielőtt az csatlakozna toohello VM toomake meg arról, hogy a számítógép úgy van konfigurálva a WinRM Távfelügyelet lesz szüksége. Indítsa el a Powershellt rendszergazdaként, majd hajtsa végre az alábbi parancs toomake meg arról, hogy elkészült a beállítással hello.

    Enable-PSRemoting -Force

> [!NOTE]
> Szükség lehet toomake, hello WinRM szolgáltatás fut, ha a fenti hello nem működik. Megteheti, hogy használatával`Get-Service WinRM`
> 
> 

Ha hello telepítő végzett, a kapcsolódás toohello VM hello alábbi parancs használatával

    Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate
