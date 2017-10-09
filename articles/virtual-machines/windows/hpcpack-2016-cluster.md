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
# <a name="deploy-an-hpc-pack-2016-cluster-in-azure"></a><span data-ttu-id="d5652-103">HPC Pack 2016-fürt üzembe helyezése az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="d5652-103">Deploy an HPC Pack 2016 cluster in Azure</span></span>

<span data-ttu-id="d5652-104">Ez a cikk toodeploy hello kövesse a [Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) fürt az Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="d5652-104">Follow hello steps in this article toodeploy a [Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) cluster in Azure virtual machines.</span></span> <span data-ttu-id="d5652-105">HPC Pack Microsoft a Microsoft Azure és a Windows Server technológiáira szabad HPC megoldása és a HPC széles tartomány munkaterhelések támogatja.</span><span class="sxs-lookup"><span data-stu-id="d5652-105">HPC Pack is Microsoft's free HPC solution built on Microsoft Azure and Windows Server technologies and supports a wide range of HPC workloads.</span></span>

<span data-ttu-id="d5652-106">Hello valamelyikével [Azure Resource Manager-sablonok](https://github.com/MsHpcPack/HPCPack2016) toodeploy hello HPC Pack 2016 fürt.</span><span class="sxs-lookup"><span data-stu-id="d5652-106">Use one of hello [Azure Resource Manager templates](https://github.com/MsHpcPack/HPCPack2016) toodeploy hello HPC Pack 2016 cluster.</span></span> <span data-ttu-id="d5652-107">Fürt-topológia központi fürtcsomópontok különböző számú, és vagy a Linux több lehetősége van, vagy a Windows számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="d5652-107">You have several choices of cluster topology with different numbers of cluster head nodes, and with either Linux or Windows compute nodes.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d5652-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d5652-108">Prerequisites</span></span>

### <a name="pfx-certificate"></a><span data-ttu-id="d5652-109">PFX-tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="d5652-109">PFX certificate</span></span>

<span data-ttu-id="d5652-110">A Microsoft HPC Pack 2016 fürt igényel a személyes információcseréhez kapcsolódó (PFX) tanúsítvány toosecure hello kommunikáció hello HPC-csomópont között.</span><span class="sxs-lookup"><span data-stu-id="d5652-110">A Microsoft HPC Pack 2016 cluster requires a Personal Information Exchange (PFX) certificate toosecure hello communication between hello HPC nodes.</span></span> <span data-ttu-id="d5652-111">hello tanúsítvány hello követelményeknek kell megfelelniük:</span><span class="sxs-lookup"><span data-stu-id="d5652-111">hello certificate must meet hello following requirements:</span></span>

* <span data-ttu-id="d5652-112">Alkalmas a kulcscserére titkos kulccsal kell rendelkeznie</span><span class="sxs-lookup"><span data-stu-id="d5652-112">It must have a private key capable of key exchange</span></span>
* <span data-ttu-id="d5652-113">Kulcshasználat a digitális aláírás és kulcstitkosítás tartalmaz</span><span class="sxs-lookup"><span data-stu-id="d5652-113">Key usage includes Digital Signature and Key Encipherment</span></span>
* <span data-ttu-id="d5652-114">Kibővített kulcshasználat ügyfél-hitelesítéshez és a kiszolgáló hitelesítése tartalmazza</span><span class="sxs-lookup"><span data-stu-id="d5652-114">Enhanced key usage includes Client Authentication and Server Authentication</span></span>

<span data-ttu-id="d5652-115">Ha még nem rendelkezik olyan tanúsítvány, amely megfelel ezeknek a követelményeknek, hello tanúsítványt kérhet egy hitelesítésszolgáltatótól.</span><span class="sxs-lookup"><span data-stu-id="d5652-115">If you don’t already have a certificate that meets these requirements, you can request hello certificate from a certification authority.</span></span> <span data-ttu-id="d5652-116">Másik lehetőségként a következő parancsok toogenerate hello önaláírt tanúsítvány hello operációs rendszer, amelyre hello parancsot futtatja, és hello PFX formátumban tanúsítvány exportálása a titkos kulcsot tartalmazó alapján hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d5652-116">Alternatively, you can use hello following commands toogenerate hello self-signed certificate based on hello operating system on which you run hello command, and export hello PFX format certificate with private key.</span></span>

* <span data-ttu-id="d5652-117">**A Windows 10 vagy Windows Server 2016**, futtassa a beépített hello **New-SelfSignedCertificate** PowerShell-parancsmagot az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="d5652-117">**For Windows 10 or Windows Server 2016**, run hello built-in **New-SelfSignedCertificate** PowerShell cmdlet as follows:</span></span>

  ```PowerShell
  New-SelfSignedCertificate -Subject "CN=HPC Pack 2016 Communication" -KeySpec KeyExchange -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2") -CertStoreLocation cert:\CurrentUser\My -KeyExportPolicy Exportable -NotAfter (Get-Date).AddYears(5)
  ```
* <span data-ttu-id="d5652-118">**Windows 10 vagy Windows Server 2016-nál korábbi operációs rendszerek**, töltse le a hello [önaláírt tanúsítvány generátor](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) a Microsoft Script Center hello.</span><span class="sxs-lookup"><span data-stu-id="d5652-118">**For operating systems earlier than Windows 10 or Windows Server 2016**, download hello [self-signed certificate generator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) from hello Microsoft Script Center.</span></span> <span data-ttu-id="d5652-119">Bontsa ki a tartalmát, és futtassa a következő parancsokat a PowerShell-parancssorba hello:</span><span class="sxs-lookup"><span data-stu-id="d5652-119">Extract its contents and run hello following commands at a PowerShell prompt:</span></span>

    ```PowerShell 
    Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
  
    New-SelfSignedCertificateEx -Subject "CN=HPC Pack 2016 Communication" -KeySpec Exchange -KeyUsage "DigitalSignature,KeyEncipherment" -EnhancedKeyUsage "Server Authentication","Client Authentication" -StoreLocation CurrentUser -Exportable -NotAfter (Get-Date).AddYears(5)
    ```

### <a name="upload-certificate-tooan-azure-key-vault"></a><span data-ttu-id="d5652-120">Az Azure key vault tanúsítvány tooan feltöltése</span><span class="sxs-lookup"><span data-stu-id="d5652-120">Upload certificate tooan Azure key vault</span></span>

<span data-ttu-id="d5652-121">Hello HPC-fürt telepítés megkezdése előtt töltse fel a hello tanúsítvány tooan [az Azure key vault](../../key-vault/index.md) , a következő hello központi telepítése során való használatra információ titkos és rekord hello: **tároló neve**,  **Tároló erőforráscsoport**, **tanúsítvány URL-címe**, és **tanúsítvány ujjlenyomata**.</span><span class="sxs-lookup"><span data-stu-id="d5652-121">Before deploying hello HPC cluster, upload hello certificate tooan [Azure key vault](../../key-vault/index.md) as a secret, and record hello following information for use during hello deployment: **Vault name**, **Vault resource group**, **Certificate URL**, and **Certificate thumbprint**.</span></span>

<span data-ttu-id="d5652-122">A következő egy minta PowerShell parancsfájl tooupload hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="d5652-122">A sample PowerShell script tooupload hello certificate follows.</span></span> <span data-ttu-id="d5652-123">Egy tanúsítvány tooan az Azure key vault feltöltésével kapcsolatos további információkért lásd: [Ismerkedés az Azure Key Vault](../../key-vault/key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d5652-123">For more information about uploading a certificate tooan Azure key vault, see [Get started with Azure Key Vault](../../key-vault/key-vault-get-started.md).</span></span>

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


## <a name="supported-topologies"></a><span data-ttu-id="d5652-124">Támogatott topológiák</span><span class="sxs-lookup"><span data-stu-id="d5652-124">Supported topologies</span></span>

<span data-ttu-id="d5652-125">Válasszon egyet a hello [Azure Resource Manager-sablonok](https://github.com/MsHpcPack/HPCPack2016) toodeploy hello HPC Pack 2016 fürt.</span><span class="sxs-lookup"><span data-stu-id="d5652-125">Choose one of hello [Azure Resource Manager templates](https://github.com/MsHpcPack/HPCPack2016) toodeploy hello HPC Pack 2016 cluster.</span></span> <span data-ttu-id="d5652-126">Az alábbiakban a magas szintű architektúra három támogatott fürt topológiája.</span><span class="sxs-lookup"><span data-stu-id="d5652-126">Following are high-level architectures of three supported cluster topologies.</span></span> <span data-ttu-id="d5652-127">Magas rendelkezésre állású topológiák több központi fürtcsomópontok közé tartozik.</span><span class="sxs-lookup"><span data-stu-id="d5652-127">High-availability topologies include multiple cluster head nodes.</span></span>

1. <span data-ttu-id="d5652-128">Magas rendelkezésre állású fürt az Active Directory-tartomány</span><span class="sxs-lookup"><span data-stu-id="d5652-128">High-availability cluster with Active Directory domain</span></span>

    ![Az Active Directory-tartománynak a magas rendelkezésre ÁLLÁSÚ fürt](./media/hpcpack-2016-cluster/haad.png)



2. <span data-ttu-id="d5652-130">Magas rendelkezésre állású fürt nélkül Active Directory-tartomány</span><span class="sxs-lookup"><span data-stu-id="d5652-130">High-availability cluster without Active Directory domain</span></span>

    ![Magas rendelkezésre ÁLLÁSÚ fürt nélkül AD-tartomány](./media/hpcpack-2016-cluster/hanoad.png)

3. <span data-ttu-id="d5652-132">Csak egy átjárócsomóponttal-fürtöt</span><span class="sxs-lookup"><span data-stu-id="d5652-132">Cluster with a single head node</span></span>

   ![Egy átjárócsomóponttal-fürtöt](./media/hpcpack-2016-cluster/singlehn.png)


## <a name="deploy-a-cluster"></a><span data-ttu-id="d5652-134">Fürt központi telepítése</span><span class="sxs-lookup"><span data-stu-id="d5652-134">Deploy a cluster</span></span>

<span data-ttu-id="d5652-135">toocreate hello fürt, válasszon egy sablont, majd kattintson az **tooAzure telepítése**.</span><span class="sxs-lookup"><span data-stu-id="d5652-135">toocreate hello cluster, choose a template and click **Deploy tooAzure**.</span></span> <span data-ttu-id="d5652-136">A hello [Azure-portálon](https://portal.azure.com), adja meg a hello sablon paramétereinek a lépéseket követve hello leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="d5652-136">In hello [Azure portal](https://portal.azure.com), specify parameters for hello template as described in hello following steps.</span></span> <span data-ttu-id="d5652-137">Minden sablon létrehozza a HPC-fürtinfrastruktúra hello szükséges összes Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="d5652-137">Each template creates all Azure resources required for hello HPC cluster infrastructure.</span></span> <span data-ttu-id="d5652-138">Erőforrások közé tartoznak, egy Azure virtuális hálózatra, nyilvános IP-cím, terheléselosztót (csak egy magas rendelkezésre állású fürt), hálózati adapterek, rendelkezésre állási csoportok, storage-fiókok, és virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="d5652-138">Resources include an Azure virtual network, public IP address, load balancer (only for a high-availability cluster), network interfaces, availability sets, storage accounts, and virtual machines.</span></span>

### <a name="step-1-select-hello-subscription-location-and-resource-group"></a><span data-ttu-id="d5652-139">1. lépés: Válassza ki, hello előfizetés, a hely és az erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="d5652-139">Step 1: Select hello subscription, location, and resource group</span></span>

<span data-ttu-id="d5652-140">Hello **előfizetés** és hello **hely** meg kell egyeznie a PFX-tanúsítvány feltöltött amikor megadott (lásd).</span><span class="sxs-lookup"><span data-stu-id="d5652-140">hello **Subscription** and hello **Location** must be same that you specified when you uploaded your PFX certificate (see Prerequisites).</span></span> <span data-ttu-id="d5652-141">Azt javasoljuk, hogy hozzon létre egy **erőforráscsoport** hello központi telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="d5652-141">We recommend that you create a **Resource group** for hello deployment.</span></span>

### <a name="step-2-specify-hello-parameter-settings"></a><span data-ttu-id="d5652-142">2. lépés: Hello paraméter beállításainak megadása</span><span class="sxs-lookup"><span data-stu-id="d5652-142">Step 2: Specify hello parameter settings</span></span>

<span data-ttu-id="d5652-143">Adja meg, vagy módosítsa a hello sablon paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="d5652-143">Enter or modify values for hello template parameters.</span></span> <span data-ttu-id="d5652-144">Kattintson a hello ikon következő tooeach paraméter súgójában talál.</span><span class="sxs-lookup"><span data-stu-id="d5652-144">Click hello icon next tooeach parameter for help information.</span></span> <span data-ttu-id="d5652-145">Is láthatja, hogy hello útmutatást [elérhető Virtuálisgép-méretek](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="d5652-145">Also see hello guidance for [available VM sizes](sizes.md).</span></span>

<span data-ttu-id="d5652-146">Adja meg a következő paraméterek hello hello előfeltételei rögzített hello értékeket: **tároló neve**, **tároló erőforráscsoport**, **tanúsítvány URL-címe**, és **Tanúsítvány ujjlenyomata**.</span><span class="sxs-lookup"><span data-stu-id="d5652-146">Specify hello values you recorded in hello Prerequisites for hello following parameters: **Vault name**, **Vault resource group**, **Certificate URL**, and **Certificate thumbprint**.</span></span>

### <a name="step-3-review-legal-terms-and-create"></a><span data-ttu-id="d5652-147">3. lépés</span><span class="sxs-lookup"><span data-stu-id="d5652-147">Step 3.</span></span> <span data-ttu-id="d5652-148">Tekintse át a jogi feltételeket és létrehozása</span><span class="sxs-lookup"><span data-stu-id="d5652-148">Review legal terms and create</span></span>
<span data-ttu-id="d5652-149">Kattintson a **tekintse át a jogi feltételeket** tooreview hello feltételeket.</span><span class="sxs-lookup"><span data-stu-id="d5652-149">Click **Review legal terms** tooreview hello terms.</span></span> <span data-ttu-id="d5652-150">Ha elfogadja, kattintson a **beszerzési**, és kattintson a **létrehozása** toostart hello központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="d5652-150">If you agree, click **Purchase**, and then click **Create** toostart hello deployment.</span></span>

## <a name="connect-toohello-cluster"></a><span data-ttu-id="d5652-151">Csatlakoztassa toohello fürtöt</span><span class="sxs-lookup"><span data-stu-id="d5652-151">Connect toohello cluster</span></span>
1. <span data-ttu-id="d5652-152">Hello HPC Pack fürt telepítése után nyissa meg toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d5652-152">After hello HPC Pack cluster is deployed, go toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="d5652-153">Kattintson a **erőforráscsoportok**, és a keresés hello erőforráscsoport mely hello fürt tették elérhetővé telepítésre.</span><span class="sxs-lookup"><span data-stu-id="d5652-153">Click **Resource groups**, and find hello resource group in which hello cluster was deployed.</span></span> <span data-ttu-id="d5652-154">Hello átjárócsomópont virtuális gépek található.</span><span class="sxs-lookup"><span data-stu-id="d5652-154">You can find hello head node virtual machines.</span></span>

    ![Központi fürtcsomópontok hello portálon](./media/hpcpack-2016-cluster/clusterhns.png)

2. <span data-ttu-id="d5652-156">Kattintson egy átjárócsomópont (egy magas rendelkezésre állású fürt kattintson valamelyik hello átjárócsomópontokkal).</span><span class="sxs-lookup"><span data-stu-id="d5652-156">Click one head node (in a high-availability cluster, click any of hello head nodes).</span></span> <span data-ttu-id="d5652-157">A **Essentials**, hello nyilvános IP-cím vagy teljes DNS-nevét hello fürt találja.</span><span class="sxs-lookup"><span data-stu-id="d5652-157">In **Essentials**, you can find hello public IP address or full DNS name of hello cluster.</span></span>

    ![Fürt kapcsolatbeállításai](./media/hpcpack-2016-cluster/clusterconnect.png)

3. <span data-ttu-id="d5652-159">Kattintson a **Connect** toolog a tooany hello központi csomópontok távoli asztal használata a megadott rendszergazdai felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="d5652-159">Click **Connect** toolog on tooany of hello head nodes using Remote Desktop with your specified administrator user name.</span></span> <span data-ttu-id="d5652-160">Hello fürt telepítette az Active Directory-tartományban, hello felhasználónév akkor hello űrlap <privateDomainName> \<adminUsername > (például hpc.local\hpcadmin).</span><span class="sxs-lookup"><span data-stu-id="d5652-160">If hello cluster you deployed is in an Active Directory Domain, hello user name is of hello form <privateDomainName>\<adminUsername> (for example, hpc.local\hpcadmin).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d5652-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d5652-161">Next steps</span></span>
* <span data-ttu-id="d5652-162">Küldje el a feladatok tooyour fürt.</span><span class="sxs-lookup"><span data-stu-id="d5652-162">Submit jobs tooyour cluster.</span></span> <span data-ttu-id="d5652-163">Lásd: [küldje el az Azure HPC Pack fürtöt feladatok tooHPC](hpcpack-cluster-submit-jobs.md) és [kezelése az Azure-ban az Azure Active Directory HPC Pack 2016 fürtöt](hpcpack-cluster-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="d5652-163">See [Submit jobs tooHPC an HPC Pack cluster in Azure](hpcpack-cluster-submit-jobs.md) and [Manage an HPC Pack 2016 cluster in Azure using Azure Active Directory](hpcpack-cluster-active-directory.md).</span></span>

