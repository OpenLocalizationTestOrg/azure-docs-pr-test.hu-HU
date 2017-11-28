---
title: "aaaCreate HDInsight-fürtök a Data Lake Store alapértelmezett tárolóként PowerShell használatával |} Microsoft Docs"
description: "Azure PowerShell toocreate igénybe, valamint a HDInsight-fürtök az Azure Data Lake Store"
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 8917af15-8e37-46cf-87ad-4e6d5d67ecdb
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/08/2017
ms.author: nitinme
ms.openlocfilehash: a5c0ad416da6ad9bd07204af2ebb6b7470916085
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-hdinsight-clusters-with-data-lake-store-as-default-storage-by-using-powershell"></a><span data-ttu-id="7fcc0-103">HDInsight-fürtök létrehozásához a Data Lake Store alapértelmezett tárolóként PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="7fcc0-103">Create HDInsight clusters with Data Lake Store as default storage by using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7fcc0-104">Hello Azure portál használata</span><span class="sxs-lookup"><span data-stu-id="7fcc0-104">Use hello Azure portal</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [<span data-ttu-id="7fcc0-105">A PowerShell (az alapértelmezett tároló)</span><span class="sxs-lookup"><span data-stu-id="7fcc0-105">Use PowerShell (for default storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [<span data-ttu-id="7fcc0-106">Használja a Powershellt (tárhely)</span><span class="sxs-lookup"><span data-stu-id="7fcc0-106">Use PowerShell (for additional storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [<span data-ttu-id="7fcc0-107">Erőforrás-kezelő használata</span><span class="sxs-lookup"><span data-stu-id="7fcc0-107">Use Resource Manager</span></span>](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

<span data-ttu-id="7fcc0-108">Ismerje meg, hogyan toouse Azure PowerShell tooconfigure Azure HDInsight-fürtök az Azure Data Lake Store, alapértelmezett tárolóként.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-108">Learn how toouse Azure PowerShell tooconfigure Azure HDInsight clusters with Azure Data Lake Store, as default storage.</span></span> <span data-ttu-id="7fcc0-109">A HDInsight-fürtök létrehozása a Data Lake Store további tárolóként útmutatásért lásd: [HDInsight-fürtök létrehozása a Data Lake Store további tárolóként](data-lake-store-hdinsight-hadoop-use-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="7fcc0-109">For instructions on creating an HDInsight cluster with Data Lake Store as additional storage, see [Create an HDInsight cluster with Data Lake Store as additional storage](data-lake-store-hdinsight-hadoop-use-powershell.md).</span></span>

<span data-ttu-id="7fcc0-110">Az alábbiakban a HDInsight a Data Lake Store használatára vonatkozó szempontokat:</span><span class="sxs-lookup"><span data-stu-id="7fcc0-110">Here are some important considerations for using HDInsight with Data Lake Store:</span></span>

* <span data-ttu-id="7fcc0-111">hello beállítás toocreate a HDInsight-fürtök hozzáféréssel rendelkező tooData Lake Store alapértelmezett tárolóként esetén HDInsight 3.5-ös és 3.6 verzió érhető el.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-111">hello option toocreate HDInsight clusters with access tooData Lake Store as default storage is available for HDInsight version 3.5 and 3.6.</span></span>

* <span data-ttu-id="7fcc0-112">a HDInsight-fürtök hello beállítás toocreate tooData Lake Store férhetnek hozzá, mint a alapértelmezett tároló *nem érhető el* a HDInsight prémium fürtök.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-112">hello option toocreate HDInsight clusters with access tooData Lake Store as default storage is *not available* for HDInsight Premium clusters.</span></span>

<span data-ttu-id="7fcc0-113">tooconfigure HDInsight toowork a Data Lake Store powershellel, kövesse a következő öt szakaszok hello hello utasításait.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-113">tooconfigure HDInsight toowork with Data Lake Store by using PowerShell, follow hello instructions in hello next five sections.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7fcc0-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7fcc0-114">Prerequisites</span></span>
<span data-ttu-id="7fcc0-115">Ez az oktatóanyag megkezdése előtt győződjön meg arról, hogy teljesülnek-e hello követelményeknek:</span><span class="sxs-lookup"><span data-stu-id="7fcc0-115">Before you begin this tutorial, make sure that you meet hello following requirements:</span></span>

* <span data-ttu-id="7fcc0-116">**Azure-előfizetés**: Nyissa meg túl[beolvasása az Azure ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7fcc0-116">**An Azure subscription**: Go too[Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="7fcc0-117">**Az Azure PowerShell 1.0-ás vagy újabb**: lásd: [hogyan tooinstall, és konfigurálja a PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7fcc0-117">**Azure PowerShell 1.0 or greater**: See [How tooinstall and configure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="7fcc0-118">**A Windows Software Development Kit (SDK)**: tooinstall Windows SDK-t, lépjen túl[letölti és a Windows 10-eszközök](https://dev.windows.com/en-us/downloads).</span><span class="sxs-lookup"><span data-stu-id="7fcc0-118">**Windows Software Development Kit (SDK)**: tooinstall Windows SDK, go too[Downloads and tools for Windows 10](https://dev.windows.com/en-us/downloads).</span></span> <span data-ttu-id="7fcc0-119">hello SDK használt toocreate biztonsági tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-119">hello SDK is used toocreate a security certificate.</span></span>
* <span data-ttu-id="7fcc0-120">**Az Azure Active Directory szolgáltatás egyszerű**: Ez az oktatóanyag leírja, hogyan toocreate egy egyszerű szolgáltatást az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7fcc0-120">**Azure Active Directory service principal**: This tutorial describes how toocreate a service principal in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="7fcc0-121">Azonban toocreate egy egyszerű szolgáltatást, kell lennie az Azure AD-rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-121">However, toocreate a service principal, you must be an Azure AD administrator.</span></span> <span data-ttu-id="7fcc0-122">Ha Ön rendszergazda, akkor hagyja ki ezt az előfeltételt, és folytassa a hello oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-122">If you are an administrator, you can skip this prerequisite and proceed with hello tutorial.</span></span>

    >[!NOTE]
    ><span data-ttu-id="7fcc0-123">Létrehozhat egy szolgáltatás egyszerű csak akkor, ha az Azure AD-rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-123">You can create a service principal only if you are an Azure AD administrator.</span></span> <span data-ttu-id="7fcc0-124">Az Azure AD rendszergazdának létre kell hoznia egy szolgáltatás egyszerű HDInsight-fürtök létrehozása a Data Lake Store előtt.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-124">Your Azure AD administrator must create a service principal before you can create an HDInsight cluster with Data Lake Store.</span></span> <span data-ttu-id="7fcc0-125">hello szolgáltatás egyszerű léteznie kell a tanúsítvánnyal leírtak [hozzon létre egy egyszerű tanúsítvány](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span><span class="sxs-lookup"><span data-stu-id="7fcc0-125">hello service principal must be created with a certificate, as described in [Create a service principal with certificate](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span></span>
    >

## <a name="create-a-data-lake-store-account"></a><span data-ttu-id="7fcc0-126">Data Lake Store-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="7fcc0-126">Create a Data Lake Store account</span></span>
<span data-ttu-id="7fcc0-127">Data Lake Store-fiók, toocreate hello a következő:</span><span class="sxs-lookup"><span data-stu-id="7fcc0-127">toocreate a Data Lake Store account, do hello following:</span></span>

1. <span data-ttu-id="7fcc0-128">Az asztalon nyisson meg egy PowerShell-ablakot, és adja meg az alábbi kódtöredékek hello.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-128">From your desktop, open a PowerShell window, and then enter hello snippets below.</span></span> <span data-ttu-id="7fcc0-129">Amikor felszólító toosign a bejelentkezési hello előfizetés rendszergazdák vagy a tulajdonosok egyikeként.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-129">When you are prompted toosign in, sign in as one of hello subscription administrators or owners.</span></span> 

        # Sign in tooyour Azure account
        Login-AzureRmAccount

        # List all hello subscriptions associated tooyour account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    > [!NOTE]
    > <span data-ttu-id="7fcc0-130">Ha hello Data Lake Store erőforrás-szolgáltató regisztrálása és a hibaüzenet hasonló túl`Register-AzureRmResourceProvider : InvalidResourceNamespace: hello resource namespace 'Microsoft.DataLakeStore' is invalid`, az előfizetés nem feltétlenül szerepel az engedélyezési listán a Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-130">If you register hello Data Lake Store resource provider and receive an error similar too`Register-AzureRmResourceProvider : InvalidResourceNamespace: hello resource namespace 'Microsoft.DataLakeStore' is invalid`, your subscription might not be whitelisted for Data Lake Store.</span></span> <span data-ttu-id="7fcc0-131">tooenable hello Data Lake Store nyilvános előzetes Azure-előfizetése hello utasításokat követve [hello Azure-portál használatával Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7fcc0-131">tooenable your Azure subscription for hello Data Lake Store public preview, follow hello instructions in [Get started with Azure Data Lake Store by using hello Azure portal](data-lake-store-get-started-portal.md).</span></span>
    >

2. <span data-ttu-id="7fcc0-132">Data Lake Store-fiók tartozik egy Azure-erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-132">A Data Lake Store account is associated with an Azure resource group.</span></span> <span data-ttu-id="7fcc0-133">Először hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-133">Start by creating a resource group.</span></span>

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    <span data-ttu-id="7fcc0-134">Ez hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="7fcc0-134">You should see an output like this:</span></span>

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. <span data-ttu-id="7fcc0-135">Data Lake Store-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-135">Create a Data Lake Store account.</span></span> <span data-ttu-id="7fcc0-136">hello fiókhoz megadott név csak kisbetűket és számokat tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-136">hello account name you specify must contain only lowercase letters and numbers.</span></span>

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    <span data-ttu-id="7fcc0-137">Hello hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="7fcc0-137">You should see an output like hello following:</span></span>

        ...
        ProvisioningState           : Succeeded
        State                       : Active
        CreationTime                : 5/5/2017 10:53:56 PM
        EncryptionState             : Enabled
        ...
        LastModifiedTime            : 5/5/2017 10:53:56 PM
        Endpoint                    : hdiadlstore.azuredatalakestore.net
        DefaultGroup                :
        Id                          : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp/providers/Microsoft.DataLakeStore/accounts/hdiadlstore
        Name                        : hdiadlstore
        Type                        : Microsoft.DataLakeStore/accounts
        Location                    : East US 2
        Tags                        : {}

4. <span data-ttu-id="7fcc0-138">A Data Lake Store alapértelmezett tárolási szükség van a toospecify, a legfelső szintű elérési toowhich hello fürtre másol fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-138">Using Data Lake Store as default storage requires you toospecify a root path toowhich hello cluster-specific files are copied during cluster creation.</span></span> <span data-ttu-id="7fcc0-139">egy legfelső szintű elérési útját, amely toocreate **/fürtök/hdiadlcluster** hello részlet, használja a következő parancsmagok hello:</span><span class="sxs-lookup"><span data-stu-id="7fcc0-139">toocreate a root path, which is **/clusters/hdiadlcluster** in hello  snippet, use hello following cmdlets:</span></span>

        $myrootdir = "/"
        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/clusters/hdiadlcluster


## <a name="set-up-authentication-for-role-based-access-toodata-lake-store"></a><span data-ttu-id="7fcc0-140">A hitelesítés beállítása, szerepköralapú hozzáférési tooData Lake</span><span class="sxs-lookup"><span data-stu-id="7fcc0-140">Set up authentication for role-based access tooData Lake Store</span></span>
<span data-ttu-id="7fcc0-141">Minden Azure-előfizetés nem tartozik az Azure AD entitás.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-141">Every Azure subscription is associated with an Azure AD entity.</span></span> <span data-ttu-id="7fcc0-142">Felhasználók és a szolgáltatások, hogy az előfizetés-erőforrások eléréséhez a hello Azure-portálon, vagy hello Azure Resource Manager API először hitelesíteniük kell az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-142">Users and services that access subscription resources by using hello Azure portal or hello Azure Resource Manager API must first authenticate with Azure AD.</span></span> <span data-ttu-id="7fcc0-143">Hozzáférés tooAzure előfizetések és a szolgáltatások hello megfelelő szerepkört egy Azure-erőforrás hozzárendelésével.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-143">Access is granted tooAzure subscriptions and services by assigning them hello appropriate role on an Azure resource.</span></span> <span data-ttu-id="7fcc0-144">Szolgáltatások esetén egy egyszerű hello szolgáltatás azonosítja az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-144">For services, a service principal identifies hello service in Azure AD.</span></span>

<span data-ttu-id="7fcc0-145">Ez a szakasz bemutatja, hogyan toogrant egy alkalmazás-szolgáltatáshoz, például a HDInsight hozzáférést tooan Azure-erőforrás (hello korábban létrehozott Data Lake Store-fiókot).</span><span class="sxs-lookup"><span data-stu-id="7fcc0-145">This section illustrates how toogrant an application service, such as HDInsight, access tooan Azure resource (hello Data Lake Store account that you created earlier).</span></span> <span data-ttu-id="7fcc0-146">Ehhez a szolgáltatás egyszerű hello alkalmazáshoz hoz létre és szerepkörök tooit PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-146">You do so by creating a service principal for hello application and assigning roles tooit via PowerShell.</span></span>

<span data-ttu-id="7fcc0-147">az Active Directory-hitelesítés az Azure Data Lake tooset a következő két szakasz hello hello feladatok végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-147">tooset up Active Directory authentication for Azure Data Lake, perform hello tasks in hello following two sections.</span></span>

### <a name="create-a-self-signed-certificate"></a><span data-ttu-id="7fcc0-148">Önaláírt tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="7fcc0-148">Create a self-signed certificate</span></span>
<span data-ttu-id="7fcc0-149">Győződjön meg arról, hogy [Windows SDK](https://dev.windows.com/en-us/downloads) ebben a szakaszban lévő lépéseket hello folytatása előtt.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-149">Make sure you have [Windows SDK](https://dev.windows.com/en-us/downloads) installed before proceeding with hello steps in this section.</span></span> <span data-ttu-id="7fcc0-150">Kell is létrehozott egy könyvtárat, például a *C:\mycertdir*, ahol létre hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-150">You must have also created a directory, such as *C:\mycertdir*, where you create hello certificate.</span></span>

1. <span data-ttu-id="7fcc0-151">Hello PowerShell ablakban nyissa meg toohello helyét Windows SDK-t (általában *C:\Program Files (x86) \Windows Kits\10\bin\x86*), és hello [MakeCert] [ makecert] segédprogram toocreate önaláírt tanúsítvány és titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-151">From hello PowerShell window, go toohello location where you installed Windows SDK (typically, *C:\Program Files (x86)\Windows Kits\10\bin\x86*) and use hello [MakeCert][makecert] utility toocreate a self-signed certificate and a private key.</span></span> <span data-ttu-id="7fcc0-152">A következő parancsok hello használata:</span><span class="sxs-lookup"><span data-stu-id="7fcc0-152">Use hello following commands:</span></span>

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    <span data-ttu-id="7fcc0-153">Rákérdezéses tooenter hello titkos kulcs jelszava lesz.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-153">You will be prompted tooenter hello private key password.</span></span> <span data-ttu-id="7fcc0-154">Miután hello parancs végrehajtása sikeres, megtekintheti az **CertFile.cer** és **mykey.pvk** hello tanúsítvány megadott könyvtárban található.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-154">After hello command is successfully executed, you should see **CertFile.cer** and **mykey.pvk** in hello certificate directory that you specified.</span></span>
2. <span data-ttu-id="7fcc0-155">Használjon hello [Pvk2Pfx] [ pvk2pfx] segédprogram tooconvert hello .pvk és .cer-fájlokat a MakeCert létrehozott tooa .pfx fájlt.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-155">Use hello [Pvk2Pfx][pvk2pfx] utility tooconvert hello .pvk and .cer files that MakeCert created tooa .pfx file.</span></span> <span data-ttu-id="7fcc0-156">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="7fcc0-156">Run hello following command:</span></span>

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    <span data-ttu-id="7fcc0-157">Amikor a rendszer kéri, adja meg a hello titkos kulcs jelszava korábban megadott.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-157">When you are prompted, enter hello private key password that you specified earlier.</span></span> <span data-ttu-id="7fcc0-158">hello számára megadott érték hello **-po** paraméter hello jelszót, amely hello .pfx fájlba van társítva.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-158">hello value you specify for hello **-po** parameter is hello password that's associated with hello .pfx file.</span></span> <span data-ttu-id="7fcc0-159">Miután hello parancs sikeresen befejeződött, emellett meg kell jelennie egy **CertFile.pfx** hello tanúsítvány megadott könyvtárban található.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-159">After hello command has been completed successfully, you should also see a **CertFile.pfx** in hello certificate directory that you specified.</span></span>

### <a name="create-an-azure-ad-and-a-service-principal"></a><span data-ttu-id="7fcc0-160">Hozzon létre egy Azure AD és az egyszerű szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="7fcc0-160">Create an Azure AD and a service principal</span></span>
<span data-ttu-id="7fcc0-161">Ebben a szakaszban egy egyszerű szolgáltatást az Azure AD-alkalmazás létrehozása, hozzárendelése egy szerepkörhöz toohello szolgáltatás egyszerű, és hitelesítse magát hello szolgáltatás egyszerű, adja meg a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-161">In this section, you create a service principal for an Azure AD application, assign a role toohello service principal, and authenticate as hello service principal by providing a certificate.</span></span> <span data-ttu-id="7fcc0-162">Futtassa a következő parancsok hello az Azure AD-alkalmazás toocreate:</span><span class="sxs-lookup"><span data-stu-id="7fcc0-162">toocreate an application in Azure AD, run hello following commands:</span></span>

1. <span data-ttu-id="7fcc0-163">Illessze be a következő parancsmagok a PowerShell-konzolablakot hello hello.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-163">Paste hello following cmdlets in hello PowerShell console window.</span></span> <span data-ttu-id="7fcc0-164">Győződjön meg arról, hogy hello értéket állít be hello **- DisplayName** tulajdonság értéke egyedi.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-164">Make sure that hello value you specify for hello **-DisplayName** property is unique.</span></span> <span data-ttu-id="7fcc0-165">az értékek hello **- kezdőlap** és **- IdentiferUris** helyőrző értékeket, és nem ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-165">hello values for **-HomePage** and **-IdentiferUris** are placeholder values and are not verified.</span></span>

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host –Prompt "Enter hello password" # This is hello password you specified for hello .pfx file

        $certificatePFX = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certificateFilePath, $password)

        $rawCertificateData = $certificatePFX.GetRawCertData()

        $credential = [System.Convert]::ToBase64String($rawCertificateData)

        $application = New-AzureRmADApplication `
            -DisplayName "HDIADL" `
            -HomePage "https://contoso.com" `
            -IdentifierUris "https://mycontoso.com" `
            -CertValue $credential  `
            -StartDate $certificatePFX.NotBefore  `
            -EndDate $certificatePFX.NotAfter

        $applicationId = $application.ApplicationId
2. <span data-ttu-id="7fcc0-166">Hozzon létre egy egyszerű hello azonosítót.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-166">Create a service principal by using hello application ID.</span></span>

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. <span data-ttu-id="7fcc0-167">Adja meg a hello szolgáltatás egyszerű hozzáférés toohello Data Lake Store legfelső szintű és a korábban megadott hello gyökérútvonalát hello mappához.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-167">Grant hello service principal access toohello Data Lake Store root and all hello folders in hello root path that you specified earlier.</span></span> <span data-ttu-id="7fcc0-168">A következő parancsmagok hello használata:</span><span class="sxs-lookup"><span data-stu-id="7fcc0-168">Use hello following cmdlets:</span></span>

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters/hdiadlcluster -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-hello-default-storage"></a><span data-ttu-id="7fcc0-169">Egy HDInsight Linux-fürt létrehozása a Data Lake Store hello alapértelmezett tároló</span><span class="sxs-lookup"><span data-stu-id="7fcc0-169">Create an HDInsight Linux cluster with Data Lake Store as hello default storage</span></span>

<span data-ttu-id="7fcc0-170">Ebben a szakaszban egy HDInsight Hadoop Linux fürt létrehozása a Data Lake Store hello alapértelmezett tárolóként.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-170">In this section, you create an HDInsight Hadoop Linux cluster with Data Lake Store as hello default storage.</span></span> <span data-ttu-id="7fcc0-171">Ebben a kiadásban hello HDInsight-fürtöt, és a Data Lake Store hello kell lennie ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-171">For this release, hello HDInsight cluster and Data Lake Store must be in hello same location.</span></span>

1. <span data-ttu-id="7fcc0-172">Hello előfizetés Bérlőazonosító lekérni, és tárolja toouse később.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-172">Retrieve hello subscription tenant ID, and store it toouse later.</span></span>

        $tenantID = (Get-AzureRmContext).Tenant.TenantId

2. <span data-ttu-id="7fcc0-173">Hello HDInsight-fürt létrehozása a következő parancsmagok hello használatával:</span><span class="sxs-lookup"><span data-stu-id="7fcc0-173">Create hello HDInsight cluster by using hello following cmdlets:</span></span>

        # Set these variables

        $location = "East US 2"
        $storageAccountName = $dataLakeStoreName                       # Data Lake Store account name
        $storageRootPath = "<Storage root path you specified earlier>" # E.g. /clusters/hdiadlcluster
        $clusterName = "<unique cluster name>"
        $clusterNodes = <ClusterSizeInNodes>            # hello number of nodes in hello HDInsight cluster
        $httpCredentials = Get-Credential
        $sshCredentials = Get-Credential

        New-AzureRmHDInsightCluster `
               -ClusterType Hadoop `
               -OSType Linux `
               -ClusterSizeInNodes $clusterNodes `
               -ResourceGroupName $resourceGroupName `
               -ClusterName $clusterName `
               -HttpCredential $httpCredentials `
               -Location $location `
               -DefaultStorageAccountType AzureDataLakeStore `
               -DefaultStorageAccountName "$storageAccountName.azuredatalakestore.net" `
               -DefaultStorageRootPath $storageRootPath `
               -Version "3.6" `
               -SshCredential $sshCredentials `
               -AadTenantId $tenantId `
               -ObjectId $objectId `
               -CertificateFilePath $certificateFilePath `
               -CertificatePassword $password

    <span data-ttu-id="7fcc0-174">Hello parancsmag sikeres befejezése után, akkor a fürt részleteinek hello felsoroló kimenetnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-174">After hello cmdlet has been successfully completed, you should see an output that lists hello cluster details.</span></span>

## <a name="run-test-jobs-on-hello-hdinsight-cluster-toouse-data-lake-store"></a><span data-ttu-id="7fcc0-175">Hello HDInsight fürt toouse Data Lake Store a teszt feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="7fcc0-175">Run test jobs on hello HDInsight cluster toouse Data Lake Store</span></span>
<span data-ttu-id="7fcc0-176">Miután konfigurálta a HDInsight-fürtöt, futtathatja Tesztfeladatok azt, hogy hozzáférhessen Data Lake Store tooensure.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-176">After you have configured an HDInsight cluster, you can run test jobs on it tooensure that it can access Data Lake Store.</span></span> <span data-ttu-id="7fcc0-177">toodo Igen, futtassa a minta Hive feladat toocreate egy tábla már elérhető a Data Lake Store: mintaadatok hello használó * <cluster root>/example/data/sample.log*.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-177">toodo so, run a sample Hive job toocreate a table that uses hello sample data that's already available in Data Lake Store at *<cluster root>/example/data/sample.log*.</span></span>

<span data-ttu-id="7fcc0-178">Ebben a szakaszban a Secure Shell (SSH) kapcsolat hello létrehozott HDInsight Linux-fürt be, és egy minta Hive-lekérdezést futtassa.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-178">In this section, you make a Secure Shell (SSH) connection into hello HDInsight Linux cluster that you created, and then you run a sample Hive query.</span></span>

* <span data-ttu-id="7fcc0-179">Ha egy Windows ügyfél toomake hello fürthöz az SSH-kapcsolat használata esetén lásd: [SSH használata a HDInsight Windows Linux-alapú Hadooppal](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="7fcc0-179">If you are using a Windows client toomake an SSH connection into hello cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>
* <span data-ttu-id="7fcc0-180">Ha a Linux-ügyfél toomake hello fürthöz az SSH-kapcsolat használata esetén lásd: [SSH használata a HDInsight Linux Linux-alapú Hadooppal](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="7fcc0-180">If you are using a Linux client toomake an SSH connection into hello cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

1. <span data-ttu-id="7fcc0-181">Hello kapcsolat el hello a következő parancs használatával indítsa el az hello Hive parancssori felület (CLI):</span><span class="sxs-lookup"><span data-stu-id="7fcc0-181">After you have made hello connection, start hello Hive command-line interface (CLI) by using hello following command:</span></span>

        hive
2. <span data-ttu-id="7fcc0-182">Használjon hello CLI tooenter hello utasítások toocreate nevű új tábla a következő **járművekről gyűjtött** hello mintaadatok használatával a Data Lake Store-ban:</span><span class="sxs-lookup"><span data-stu-id="7fcc0-182">Use hello CLI tooenter hello following statements toocreate a new table named **vehicles** by using hello sample data in Data Lake Store:</span></span>

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'adl:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    <span data-ttu-id="7fcc0-183">A hello SSH konzolon hello lekérdezés kimenetnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-183">You should see hello query output on hello SSH console.</span></span>

    >[!NOTE]
    ><span data-ttu-id="7fcc0-184">hello elérési toohello minta CREATE TABLE parancs megelőző hello adatai `adl:///example/data/`, ahol `adl:///` hello fürt legfelső szintű.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-184">hello path toohello sample data in hello preceding CREATE TABLE command is `adl:///example/data/`, where `adl:///` is hello cluster root.</span></span> <span data-ttu-id="7fcc0-185">Következő hello fürt legfelső szintű ebben az oktatóanyagban megadott hello példában hello parancs: `adl://hdiadlstore.azuredatalakestore.net/clusters/hdiadlcluster`.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-185">Following hello example of hello cluster root that's specified in this tutorial, hello command is `adl://hdiadlstore.azuredatalakestore.net/clusters/hdiadlcluster`.</span></span> <span data-ttu-id="7fcc0-186">Használjon rövidebb alternatív hello, vagy adja meg a hello teljes elérési útja toohello fürt legfelső szintű.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-186">You can either use hello shorter alternative or provide hello complete path toohello cluster root.</span></span>
    >

## <a name="access-data-lake-store-by-using-hdfs-commands"></a><span data-ttu-id="7fcc0-187">Hozzáférés Data Lake Store HDFS parancs használatával</span><span class="sxs-lookup"><span data-stu-id="7fcc0-187">Access Data Lake Store by using HDFS commands</span></span>
<span data-ttu-id="7fcc0-188">Miután konfigurálta a hello HDInsight fürt toouse Data Lake Store, Hadoop elosztott fájlrendszerrel (HDFS) rendszerhéj parancsok tooaccess hello tároló is használhatja.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-188">After you have configured hello HDInsight cluster toouse Data Lake Store, you can use Hadoop Distributed File System (HDFS) shell commands tooaccess hello store.</span></span>

<span data-ttu-id="7fcc0-189">Ebben a szakaszban egy SSH-kapcsolatot hello létrehozott HDInsight Linux-fürt be, és hello HDFS parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-189">In this section, you make an SSH connection into hello HDInsight Linux cluster that you created, and then you run hello HDFS commands.</span></span>

* <span data-ttu-id="7fcc0-190">Ha egy Windows ügyfél toomake hello fürthöz az SSH-kapcsolat használata esetén lásd: [SSH használata a HDInsight Windows Linux-alapú Hadooppal](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="7fcc0-190">If you are using a Windows client toomake an SSH connection into hello cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>
* <span data-ttu-id="7fcc0-191">Ha a Linux-ügyfél toomake hello fürthöz az SSH-kapcsolat használata esetén lásd: [SSH használata a HDInsight Linux Linux-alapú Hadooppal](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="7fcc0-191">If you are using a Linux client toomake an SSH connection into hello cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="7fcc0-192">Hello kapcsolat elkészítése után hello fájlok listázása a Data Lake Store a következő HDFS hello segítségével a rendszer parancs fájl.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-192">After you've made hello connection, list hello files in Data Lake Store by using hello following HDFS file system command.</span></span>

    hdfs dfs -ls adl:///

<span data-ttu-id="7fcc0-193">Is használhatja a hello `hdfs dfs -put` tooupload bizonyos fájlok tooData Lake Store parancsot, és ezután `hdfs dfs -ls` tooverify e hello fájlok sikeresen feltöltve.</span><span class="sxs-lookup"><span data-stu-id="7fcc0-193">You can also use hello `hdfs dfs -put` command tooupload some files tooData Lake Store, and then use `hdfs dfs -ls` tooverify whether hello files were successfully uploaded.</span></span>

## <a name="see-also"></a><span data-ttu-id="7fcc0-194">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="7fcc0-194">See also</span></span>
* [<span data-ttu-id="7fcc0-195">Azure-portálon: hozzon létre egy HDInsight-fürt toouse Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="7fcc0-195">Azure portal: Create an HDInsight cluster toouse Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
