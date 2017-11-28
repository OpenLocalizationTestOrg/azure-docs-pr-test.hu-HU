---
title: "HPC Pack 2016 fürt az Azure-ban |} Microsoft Docs"
description: "Megtudhatja, hogyan HPC Pack 2016-fürt üzembe helyezése az Azure-ban"
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
ms.openlocfilehash: 88d1f4e29f38ba1a6bef57c2da43bee205575eee
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-an-hpc-pack-2016-cluster-in-azure"></a><span data-ttu-id="80a8c-103">HPC Pack 2016-fürt üzembe helyezése az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="80a8c-103">Deploy an HPC Pack 2016 cluster in Azure</span></span>

<span data-ttu-id="80a8c-104">Ez a cikk telepítéséhez kövesse a [Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) fürt az Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="80a8c-104">Follow the steps in this article to deploy a [Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) cluster in Azure virtual machines.</span></span> <span data-ttu-id="80a8c-105">HPC Pack Microsoft a Microsoft Azure és a Windows Server technológiáira szabad HPC megoldása és a HPC széles tartomány munkaterhelések támogatja.</span><span class="sxs-lookup"><span data-stu-id="80a8c-105">HPC Pack is Microsoft's free HPC solution built on Microsoft Azure and Windows Server technologies and supports a wide range of HPC workloads.</span></span>

<span data-ttu-id="80a8c-106">Valamelyikével a [Azure Resource Manager-sablonok](https://github.com/MsHpcPack/HPCPack2016) a HPC Pack 2016 fürt telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="80a8c-106">Use one of the [Azure Resource Manager templates](https://github.com/MsHpcPack/HPCPack2016) to deploy the HPC Pack 2016 cluster.</span></span> <span data-ttu-id="80a8c-107">Fürt-topológia központi fürtcsomópontok különböző számú, és vagy a Linux több lehetősége van, vagy a Windows számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="80a8c-107">You have several choices of cluster topology with different numbers of cluster head nodes, and with either Linux or Windows compute nodes.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="80a8c-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="80a8c-108">Prerequisites</span></span>

### <a name="pfx-certificate"></a><span data-ttu-id="80a8c-109">PFX-tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="80a8c-109">PFX certificate</span></span>

<span data-ttu-id="80a8c-110">A Microsoft HPC Pack 2016 fürt csak a személyes információcseréhez kapcsolódó (PFX) tanúsítványának a HPC-csomópontok közötti kommunikáció védelméhez.</span><span class="sxs-lookup"><span data-stu-id="80a8c-110">A Microsoft HPC Pack 2016 cluster requires a Personal Information Exchange (PFX) certificate to secure the communication between the HPC nodes.</span></span> <span data-ttu-id="80a8c-111">A tanúsítvány a következő követelményeknek kell megfelelniük:</span><span class="sxs-lookup"><span data-stu-id="80a8c-111">The certificate must meet the following requirements:</span></span>

* <span data-ttu-id="80a8c-112">Alkalmas a kulcscserére titkos kulccsal kell rendelkeznie</span><span class="sxs-lookup"><span data-stu-id="80a8c-112">It must have a private key capable of key exchange</span></span>
* <span data-ttu-id="80a8c-113">Kulcshasználat a digitális aláírás és kulcstitkosítás tartalmaz</span><span class="sxs-lookup"><span data-stu-id="80a8c-113">Key usage includes Digital Signature and Key Encipherment</span></span>
* <span data-ttu-id="80a8c-114">Kibővített kulcshasználat ügyfél-hitelesítéshez és a kiszolgáló hitelesítése tartalmazza</span><span class="sxs-lookup"><span data-stu-id="80a8c-114">Enhanced key usage includes Client Authentication and Server Authentication</span></span>

<span data-ttu-id="80a8c-115">Ha még nem rendelkezik olyan tanúsítvány, amely megfelel ezeknek a követelményeknek, a tanúsítványt kérhet egy hitelesítésszolgáltatótól.</span><span class="sxs-lookup"><span data-stu-id="80a8c-115">If you don’t already have a certificate that meets these requirements, you can request the certificate from a certification authority.</span></span> <span data-ttu-id="80a8c-116">Azt is megteheti az alábbi parancsokkal létrehozni az önaláírt tanúsítvány alapján az operációs rendszer, amelyen a parancsot futtatja, és a PFX-formátum tanúsítvány exportálása a titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="80a8c-116">Alternatively, you can use the following commands to generate the self-signed certificate based on the operating system on which you run the command, and export the PFX format certificate with private key.</span></span>

* <span data-ttu-id="80a8c-117">**A Windows 10 vagy Windows Server 2016**, futtassa a beépített **New-SelfSignedCertificate** PowerShell-parancsmagot az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="80a8c-117">**For Windows 10 or Windows Server 2016**, run the built-in **New-SelfSignedCertificate** PowerShell cmdlet as follows:</span></span>

  ```PowerShell
  New-SelfSignedCertificate -Subject "CN=HPC Pack 2016 Communication" -KeySpec KeyExchange -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2") -CertStoreLocation cert:\CurrentUser\My -KeyExportPolicy Exportable -NotAfter (Get-Date).AddYears(5)
  ```
* <span data-ttu-id="80a8c-118">**Windows 10 vagy Windows Server 2016-nál korábbi operációs rendszerek**, töltse le a [önaláírt tanúsítvány generátor](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) a Microsoft Script Center.</span><span class="sxs-lookup"><span data-stu-id="80a8c-118">**For operating systems earlier than Windows 10 or Windows Server 2016**, download the [self-signed certificate generator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) from the Microsoft Script Center.</span></span> <span data-ttu-id="80a8c-119">Bontsa ki a tartalmát, és futtassa az alábbi parancsokat a PowerShell-parancssorba:</span><span class="sxs-lookup"><span data-stu-id="80a8c-119">Extract its contents and run the following commands at a PowerShell prompt:</span></span>

    ```PowerShell 
    Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
  
    New-SelfSignedCertificateEx -Subject "CN=HPC Pack 2016 Communication" -KeySpec Exchange -KeyUsage "DigitalSignature,KeyEncipherment" -EnhancedKeyUsage "Server Authentication","Client Authentication" -StoreLocation CurrentUser -Exportable -NotAfter (Get-Date).AddYears(5)
    ```

### <a name="upload-certificate-to-an-azure-key-vault"></a><span data-ttu-id="80a8c-120">Az egy az Azure key vault-tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="80a8c-120">Upload certificate to an Azure key vault</span></span>

<span data-ttu-id="80a8c-121">A HPC-fürt telepítése előtt töltse fel a tanúsítvány egy [az Azure key vault](../../key-vault/index.md) titkos kulcs és rekord a telepítés során használja a következő információkat: **tároló neve**, **tároló erőforráscsoport**, **tanúsítvány URL-címe**, és **tanúsítvány ujjlenyomata**.</span><span class="sxs-lookup"><span data-stu-id="80a8c-121">Before deploying the HPC cluster, upload the certificate to an [Azure key vault](../../key-vault/index.md) as a secret, and record the following information for use during the deployment: **Vault name**, **Vault resource group**, **Certificate URL**, and **Certificate thumbprint**.</span></span>

<span data-ttu-id="80a8c-122">A PowerShell-parancsfájlpélda a tanúsítvány feltöltése követi.</span><span class="sxs-lookup"><span data-stu-id="80a8c-122">A sample PowerShell script to upload the certificate follows.</span></span> <span data-ttu-id="80a8c-123">További információ a tanúsítvány feltöltése egy az Azure key vault: [Ismerkedés az Azure Key Vault](../../key-vault/key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="80a8c-123">For more information about uploading a certificate to an Azure key vault, see [Get started with Azure Key Vault](../../key-vault/key-vault-get-started.md).</span></span>

```powershell
#Give the following values
$VaultName = "mytestvault"
$SecretName = "hpcpfxcert"
$VaultRG = "myresourcegroup"
$location = "westus"
$PfxFile = "c:\Temp\mytest.pfx"
$Password = "yourpfxkeyprotectionpassword"
#Validate the pfx file
try {
    $pfxCert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList $PfxFile, $Password
}
catch [System.Management.Automation.MethodInvocationException]
{
    throw $_.Exception.InnerException
}
$thumbprint = $pfxCert.Thumbprint
$pfxCert.Dispose()
# Create and encode the JSON object
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
#Create an Azure key vault and upload the certificate as a secret
$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
$rg = Get-AzureRmResourceGroup -Name $VaultRG -Location $location -ErrorAction SilentlyContinue
if($null -eq $rg)
{
    $rg = New-AzureRmResourceGroup -Name $VaultRG -Location $location
}
$hpcKeyVault = New-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $VaultRG -Location $location -EnabledForDeployment -EnabledForTemplateDeployment
$hpcSecret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secret
"The following Information will be used in the deployment template"
"Vault Name             :   $VaultName"
"Vault Resource Group   :   $VaultRG"
"Certificate URL        :   $($hpcSecret.Id)"
"Certificate Thumbprint :   $thumbprint"

```


## <a name="supported-topologies"></a><span data-ttu-id="80a8c-124">Támogatott topológiák</span><span class="sxs-lookup"><span data-stu-id="80a8c-124">Supported topologies</span></span>

<span data-ttu-id="80a8c-125">Válasszon egyet a a [Azure Resource Manager-sablonok](https://github.com/MsHpcPack/HPCPack2016) a HPC Pack 2016 fürt telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="80a8c-125">Choose one of the [Azure Resource Manager templates](https://github.com/MsHpcPack/HPCPack2016) to deploy the HPC Pack 2016 cluster.</span></span> <span data-ttu-id="80a8c-126">Az alábbiakban a magas szintű architektúra három támogatott fürt topológiája.</span><span class="sxs-lookup"><span data-stu-id="80a8c-126">Following are high-level architectures of three supported cluster topologies.</span></span> <span data-ttu-id="80a8c-127">Magas rendelkezésre állású topológiák több központi fürtcsomópontok közé tartozik.</span><span class="sxs-lookup"><span data-stu-id="80a8c-127">High-availability topologies include multiple cluster head nodes.</span></span>

1. <span data-ttu-id="80a8c-128">Magas rendelkezésre állású fürt az Active Directory-tartomány</span><span class="sxs-lookup"><span data-stu-id="80a8c-128">High-availability cluster with Active Directory domain</span></span>

    ![Az Active Directory-tartománynak a magas rendelkezésre ÁLLÁSÚ fürt](./media/hpcpack-2016-cluster/haad.png)



2. <span data-ttu-id="80a8c-130">Magas rendelkezésre állású fürt nélkül Active Directory-tartomány</span><span class="sxs-lookup"><span data-stu-id="80a8c-130">High-availability cluster without Active Directory domain</span></span>

    ![Magas rendelkezésre ÁLLÁSÚ fürt nélkül AD-tartomány](./media/hpcpack-2016-cluster/hanoad.png)

3. <span data-ttu-id="80a8c-132">Csak egy átjárócsomóponttal-fürtöt</span><span class="sxs-lookup"><span data-stu-id="80a8c-132">Cluster with a single head node</span></span>

   ![Egy átjárócsomóponttal-fürtöt](./media/hpcpack-2016-cluster/singlehn.png)


## <a name="deploy-a-cluster"></a><span data-ttu-id="80a8c-134">Fürt központi telepítése</span><span class="sxs-lookup"><span data-stu-id="80a8c-134">Deploy a cluster</span></span>

<span data-ttu-id="80a8c-135">A fürt létrehozásához válassza ki a sablont, és kattintson **az Azure telepítéséhez**.</span><span class="sxs-lookup"><span data-stu-id="80a8c-135">To create the cluster, choose a template and click **Deploy to Azure**.</span></span> <span data-ttu-id="80a8c-136">Az a [Azure-portálon](https://portal.azure.com), adja meg a sablon paramétereinek, a következő lépésben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="80a8c-136">In the [Azure portal](https://portal.azure.com), specify parameters for the template as described in the following steps.</span></span> <span data-ttu-id="80a8c-137">Minden sablon létrehozza a HPC-fürtinfrastruktúra szükséges összes Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="80a8c-137">Each template creates all Azure resources required for the HPC cluster infrastructure.</span></span> <span data-ttu-id="80a8c-138">Erőforrások közé tartoznak, egy Azure virtuális hálózatra, nyilvános IP-cím, terheléselosztót (csak egy magas rendelkezésre állású fürt), hálózati adapterek, rendelkezésre állási csoportok, storage-fiókok, és virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="80a8c-138">Resources include an Azure virtual network, public IP address, load balancer (only for a high-availability cluster), network interfaces, availability sets, storage accounts, and virtual machines.</span></span>

### <a name="step-1-select-the-subscription-location-and-resource-group"></a><span data-ttu-id="80a8c-139">1. lépés: Válassza ki az előfizetést, helyét és erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="80a8c-139">Step 1: Select the subscription, location, and resource group</span></span>

<span data-ttu-id="80a8c-140">A **előfizetés** és a **hely** meg kell egyeznie a PFX-tanúsítvány feltöltött amikor megadott (lásd).</span><span class="sxs-lookup"><span data-stu-id="80a8c-140">The **Subscription** and the **Location** must be same that you specified when you uploaded your PFX certificate (see Prerequisites).</span></span> <span data-ttu-id="80a8c-141">Azt javasoljuk, hogy hozzon létre egy **erőforráscsoport** a központi telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="80a8c-141">We recommend that you create a **Resource group** for the deployment.</span></span>

### <a name="step-2-specify-the-parameter-settings"></a><span data-ttu-id="80a8c-142">2. lépés: A paraméter-beállításainak megadása</span><span class="sxs-lookup"><span data-stu-id="80a8c-142">Step 2: Specify the parameter settings</span></span>

<span data-ttu-id="80a8c-143">Adja meg, vagy módosítsa a sablon paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="80a8c-143">Enter or modify values for the template parameters.</span></span> <span data-ttu-id="80a8c-144">Kattintson a súgó információt minden paraméter melletti ikonra.</span><span class="sxs-lookup"><span data-stu-id="80a8c-144">Click the icon next to each parameter for help information.</span></span> <span data-ttu-id="80a8c-145">Is láthatja, hogy az útmutató a [elérhető Virtuálisgép-méretek](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="80a8c-145">Also see the guidance for [available VM sizes](sizes.md).</span></span>

<span data-ttu-id="80a8c-146">Adja meg az értékeket az előfeltételeket a következő paraméterek rögzített: **tároló neve**, **tároló erőforráscsoport**, **tanúsítvány URL-címe**, és **tanúsítvány ujjlenyomata**.</span><span class="sxs-lookup"><span data-stu-id="80a8c-146">Specify the values you recorded in the Prerequisites for the following parameters: **Vault name**, **Vault resource group**, **Certificate URL**, and **Certificate thumbprint**.</span></span>

### <a name="step-3-review-legal-terms-and-create"></a><span data-ttu-id="80a8c-147">3. lépés</span><span class="sxs-lookup"><span data-stu-id="80a8c-147">Step 3.</span></span> <span data-ttu-id="80a8c-148">Tekintse át a jogi feltételeket és létrehozása</span><span class="sxs-lookup"><span data-stu-id="80a8c-148">Review legal terms and create</span></span>
<span data-ttu-id="80a8c-149">Kattintson a **tekintse át a jogi feltételeket** való olvassa el a feltételeket.</span><span class="sxs-lookup"><span data-stu-id="80a8c-149">Click **Review legal terms** to review the terms.</span></span> <span data-ttu-id="80a8c-150">Ha elfogadja, kattintson a **beszerzési**, és kattintson a **létrehozása** a telepítés elindításához.</span><span class="sxs-lookup"><span data-stu-id="80a8c-150">If you agree, click **Purchase**, and then click **Create** to start the deployment.</span></span>

## <a name="connect-to-the-cluster"></a><span data-ttu-id="80a8c-151">Csatlakozás a fürthöz</span><span class="sxs-lookup"><span data-stu-id="80a8c-151">Connect to the cluster</span></span>
1. <span data-ttu-id="80a8c-152">Lépjen a HPC Pack fürt telepítése után a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="80a8c-152">After the HPC Pack cluster is deployed, go to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="80a8c-153">Kattintson a **erőforráscsoportok**, és keresse meg az erőforráscsoportot, amely a fürt tették elérhetővé telepítésre.</span><span class="sxs-lookup"><span data-stu-id="80a8c-153">Click **Resource groups**, and find the resource group in which the cluster was deployed.</span></span> <span data-ttu-id="80a8c-154">Az átjárócsomópont virtuális gépek található.</span><span class="sxs-lookup"><span data-stu-id="80a8c-154">You can find the head node virtual machines.</span></span>

    ![Központi fürtcsomópontok a portálon](./media/hpcpack-2016-cluster/clusterhns.png)

2. <span data-ttu-id="80a8c-156">Kattintson egy átjárócsomópont (magas rendelkezésre állású fürt, kattintson a head csomópontokon).</span><span class="sxs-lookup"><span data-stu-id="80a8c-156">Click one head node (in a high-availability cluster, click any of the head nodes).</span></span> <span data-ttu-id="80a8c-157">A **Essentials**, megtalálhatja a nyilvános IP-cím vagy a fürt teljes DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="80a8c-157">In **Essentials**, you can find the public IP address or full DNS name of the cluster.</span></span>

    ![Fürt kapcsolatbeállításai](./media/hpcpack-2016-cluster/clusterconnect.png)

3. <span data-ttu-id="80a8c-159">Kattintson a **Connect** jelentkezzen be a távoli asztal használata a megadott rendszergazdai felhasználónév központi csomópontokon.</span><span class="sxs-lookup"><span data-stu-id="80a8c-159">Click **Connect** to log on to any of the head nodes using Remote Desktop with your specified administrator user name.</span></span> <span data-ttu-id="80a8c-160">Ha a fürt, amelyet központilag telepített Active Directory-tartományban, a felhasználónév az űrlap van <privateDomainName> \<adminUsername > (például hpc.local\hpcadmin).</span><span class="sxs-lookup"><span data-stu-id="80a8c-160">If the cluster you deployed is in an Active Directory Domain, the user name is of the form <privateDomainName>\<adminUsername> (for example, hpc.local\hpcadmin).</span></span>

## <a name="next-steps"></a><span data-ttu-id="80a8c-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="80a8c-161">Next steps</span></span>
* <span data-ttu-id="80a8c-162">A fürt feladatok elküldéséhez.</span><span class="sxs-lookup"><span data-stu-id="80a8c-162">Submit jobs to your cluster.</span></span> <span data-ttu-id="80a8c-163">Lásd: [küldés feladatok HPC egy HPC Pack fürtön, az Azure-ban](hpcpack-cluster-submit-jobs.md) és [kezelése az Azure-ban az Azure Active Directory HPC Pack 2016 fürtöt](hpcpack-cluster-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="80a8c-163">See [Submit jobs to HPC an HPC Pack cluster in Azure](hpcpack-cluster-submit-jobs.md) and [Manage an HPC Pack 2016 cluster in Azure using Azure Active Directory](hpcpack-cluster-active-directory.md).</span></span>

