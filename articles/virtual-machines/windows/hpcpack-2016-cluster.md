---
title: "Csomag 2016 aaaHPC fürtön, az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan toodeploy egy HPC Pack 2016 fürtön, az Azure-ban"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3dde6a68-e4a6-4054-8b67-d6a90fdc5e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 12/15/2016
ms.author: danlep
ms.openlocfilehash: 9e21ec70c822a783474b96da77ce925940279abf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-hpc-pack-2016-cluster-in-azure"></a>HPC Pack 2016-fürt üzembe helyezése az Azure-ban

Ez a cikk toodeploy hello kövesse a [Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) fürt az Azure virtuális gépeken. HPC Pack Microsoft a Microsoft Azure és a Windows Server technológiáira szabad HPC megoldása és a HPC széles tartomány munkaterhelések támogatja.

Hello valamelyikével [Azure Resource Manager-sablonok](https://github.com/MsHpcPack/HPCPack2016) toodeploy hello HPC Pack 2016 fürt. Fürt-topológia központi fürtcsomópontok különböző számú, és vagy a Linux több lehetősége van, vagy a Windows számítási csomópontjain.

## <a name="prerequisites"></a>Előfeltételek

### <a name="pfx-certificate"></a>PFX-tanúsítvány

A Microsoft HPC Pack 2016 fürt igényel a személyes információcseréhez kapcsolódó (PFX) tanúsítvány toosecure hello kommunikáció hello HPC-csomópont között. hello tanúsítvány hello követelményeknek kell megfelelniük:

* Alkalmas a kulcscserére titkos kulccsal kell rendelkeznie
* Kulcshasználat a digitális aláírás és kulcstitkosítás tartalmaz
* Kibővített kulcshasználat ügyfél-hitelesítéshez és a kiszolgáló hitelesítése tartalmazza

Ha még nem rendelkezik olyan tanúsítvány, amely megfelel ezeknek a követelményeknek, hello tanúsítványt kérhet egy hitelesítésszolgáltatótól. Másik lehetőségként a következő parancsok toogenerate hello önaláírt tanúsítvány hello operációs rendszer, amelyre hello parancsot futtatja, és hello PFX formátumban tanúsítvány exportálása a titkos kulcsot tartalmazó alapján hello is használhatja.

* **A Windows 10 vagy Windows Server 2016**, futtassa a beépített hello **New-SelfSignedCertificate** PowerShell-parancsmagot az alábbiak szerint:

  ```PowerShell
  New-SelfSignedCertificate -Subject "CN=HPC Pack 2016 Communication" -KeySpec KeyExchange -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2") -CertStoreLocation cert:\CurrentUser\My -KeyExportPolicy Exportable -NotAfter (Get-Date).AddYears(5)
  ```
* **Windows 10 vagy Windows Server 2016-nál korábbi operációs rendszerek**, töltse le a hello [önaláírt tanúsítvány generátor](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) a Microsoft Script Center hello. Bontsa ki a tartalmát, és futtassa a következő parancsokat a PowerShell-parancssorba hello:

    ```PowerShell 
    Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
  
    New-SelfSignedCertificateEx -Subject "CN=HPC Pack 2016 Communication" -KeySpec Exchange -KeyUsage "DigitalSignature,KeyEncipherment" -EnhancedKeyUsage "Server Authentication","Client Authentication" -StoreLocation CurrentUser -Exportable -NotAfter (Get-Date).AddYears(5)
    ```

### <a name="upload-certificate-tooan-azure-key-vault"></a>Az Azure key vault tanúsítvány tooan feltöltése

Hello HPC-fürt telepítés megkezdése előtt töltse fel a hello tanúsítvány tooan [az Azure key vault](../../key-vault/index.md) , a következő hello központi telepítése során való használatra információ titkos és rekord hello: **tároló neve**,  **Tároló erőforráscsoport**, **tanúsítvány URL-címe**, és **tanúsítvány ujjlenyomata**.

A következő egy minta PowerShell parancsfájl tooupload hello tanúsítványt. Egy tanúsítvány tooan az Azure key vault feltöltésével kapcsolatos további információkért lásd: [Ismerkedés az Azure Key Vault](../../key-vault/key-vault-get-started.md).

```powershell
#Give hello following values
$VaultName = "mytestvault"
$SecretName = "hpcpfxcert"
$VaultRG = "myresourcegroup"
$location = "westus"
$PfxFile = "c:\Temp\mytest.pfx"
$Password = "yourpfxkeyprotectionpassword"
#Validate hello pfx file
try {
    $pfxCert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList $PfxFile, $Password
}
catch [System.Management.Automation.MethodInvocationException]
{
    throw $_.Exception.InnerException
}
$thumbprint = $pfxCert.Thumbprint
$pfxCert.Dispose()
# Create and encode hello JSON object
$pfxContentBytes = Get-Content $PfxFile -Encoding Byte
$pfxContentEncoded = [System.Convert]::ToBase64String($pfxContentBytes)
$jsonObject = @"
{
"data": "$pfxContentEncoded",
"dataType": "pfx",
"password": "$Password"
}
"@
$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)
#Create an Azure key vault and upload hello certificate as a secret
$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
$rg = Get-AzureRmResourceGroup -Name $VaultRG -Location $location -ErrorAction SilentlyContinue
if($null -eq $rg)
{
    $rg = New-AzureRmResourceGroup -Name $VaultRG -Location $location
}
$hpcKeyVault = New-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $VaultRG -Location $location -EnabledForDeployment -EnabledForTemplateDeployment
$hpcSecret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secret
"hello following Information will be used in hello deployment template"
"Vault Name             :   $VaultName"
"Vault Resource Group   :   $VaultRG"
"Certificate URL        :   $($hpcSecret.Id)"
"Certificate Thumbprint :   $thumbprint"

```


## <a name="supported-topologies"></a>Támogatott topológiák

Válasszon egyet a hello [Azure Resource Manager-sablonok](https://github.com/MsHpcPack/HPCPack2016) toodeploy hello HPC Pack 2016 fürt. Az alábbiakban a magas szintű architektúra három támogatott fürt topológiája. Magas rendelkezésre állású topológiák több központi fürtcsomópontok közé tartozik.

1. Magas rendelkezésre állású fürt az Active Directory-tartomány

    ![Az Active Directory-tartománynak a magas rendelkezésre ÁLLÁSÚ fürt](./media/hpcpack-2016-cluster/haad.png)



2. Magas rendelkezésre állású fürt nélkül Active Directory-tartomány

    ![Magas rendelkezésre ÁLLÁSÚ fürt nélkül AD-tartomány](./media/hpcpack-2016-cluster/hanoad.png)

3. Csak egy átjárócsomóponttal-fürtöt

   ![Egy átjárócsomóponttal-fürtöt](./media/hpcpack-2016-cluster/singlehn.png)


## <a name="deploy-a-cluster"></a>Fürt központi telepítése

toocreate hello fürt, válasszon egy sablont, majd kattintson az **tooAzure telepítése**. A hello [Azure-portálon](https://portal.azure.com), adja meg a hello sablon paramétereinek a lépéseket követve hello leírtak szerint. Minden sablon létrehozza a HPC-fürtinfrastruktúra hello szükséges összes Azure-erőforrások. Erőforrások közé tartoznak, egy Azure virtuális hálózatra, nyilvános IP-cím, terheléselosztót (csak egy magas rendelkezésre állású fürt), hálózati adapterek, rendelkezésre állási csoportok, storage-fiókok, és virtuális gépek.

### <a name="step-1-select-hello-subscription-location-and-resource-group"></a>1. lépés: Válassza ki, hello előfizetés, a hely és az erőforráscsoport

Hello **előfizetés** és hello **hely** meg kell egyeznie a PFX-tanúsítvány feltöltött amikor megadott (lásd). Azt javasoljuk, hogy hozzon létre egy **erőforráscsoport** hello központi telepítéshez.

### <a name="step-2-specify-hello-parameter-settings"></a>2. lépés: Hello paraméter beállításainak megadása

Adja meg, vagy módosítsa a hello sablon paraméter értékét. Kattintson a hello ikon következő tooeach paraméter súgójában talál. Is láthatja, hogy hello útmutatást [elérhető Virtuálisgép-méretek](sizes.md).

Adja meg a következő paraméterek hello hello előfeltételei rögzített hello értékeket: **tároló neve**, **tároló erőforráscsoport**, **tanúsítvány URL-címe**, és **Tanúsítvány ujjlenyomata**.

### <a name="step-3-review-legal-terms-and-create"></a>3. lépés Tekintse át a jogi feltételeket és létrehozása
Kattintson a **tekintse át a jogi feltételeket** tooreview hello feltételeket. Ha elfogadja, kattintson a **beszerzési**, és kattintson a **létrehozása** toostart hello központi telepítés.

## <a name="connect-toohello-cluster"></a>Csatlakoztassa toohello fürtöt
1. Hello HPC Pack fürt telepítése után nyissa meg toohello [Azure-portálon](https://portal.azure.com). Kattintson a **erőforráscsoportok**, és a keresés hello erőforráscsoport mely hello fürt tették elérhetővé telepítésre. Hello átjárócsomópont virtuális gépek található.

    ![Központi fürtcsomópontok hello portálon](./media/hpcpack-2016-cluster/clusterhns.png)

2. Kattintson egy átjárócsomópont (egy magas rendelkezésre állású fürt kattintson valamelyik hello átjárócsomópontokkal). A **Essentials**, hello nyilvános IP-cím vagy teljes DNS-nevét hello fürt találja.

    ![Fürt kapcsolatbeállításai](./media/hpcpack-2016-cluster/clusterconnect.png)

3. Kattintson a **Connect** toolog a tooany hello központi csomópontok távoli asztal használata a megadott rendszergazdai felhasználónév. Hello fürt telepítette az Active Directory-tartományban, hello felhasználónév akkor hello űrlap <privateDomainName> \<adminUsername > (például hpc.local\hpcadmin).

## <a name="next-steps"></a>Következő lépések
* Küldje el a feladatok tooyour fürt. Lásd: [küldje el az Azure HPC Pack fürtöt feladatok tooHPC](hpcpack-cluster-submit-jobs.md) és [kezelése az Azure-ban az Azure Active Directory HPC Pack 2016 fürtöt](hpcpack-cluster-active-directory.md).

