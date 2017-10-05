---
title: "HDInsight-fürtök létrehozásához a Data Lake Store alapértelmezett tárolóként PowerShell használatával |} Microsoft Docs"
description: "Azure PowerShell használatával hozzon létre, és az Azure Data Lake Store a HDInsight-fürtök használata"
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
ms.openlocfilehash: 77eb83b80312eca401e6f60d57ed6a5668ea442e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-hdinsight-clusters-with-data-lake-store-as-default-storage-by-using-powershell"></a><span data-ttu-id="5d42d-103">HDInsight-fürtök létrehozásához a Data Lake Store alapértelmezett tárolóként PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="5d42d-103">Create HDInsight clusters with Data Lake Store as default storage by using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5d42d-104">Az Azure Portal használata</span><span class="sxs-lookup"><span data-stu-id="5d42d-104">Use the Azure portal</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [<span data-ttu-id="5d42d-105">A PowerShell (az alapértelmezett tároló)</span><span class="sxs-lookup"><span data-stu-id="5d42d-105">Use PowerShell (for default storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [<span data-ttu-id="5d42d-106">Használja a Powershellt (tárhely)</span><span class="sxs-lookup"><span data-stu-id="5d42d-106">Use PowerShell (for additional storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [<span data-ttu-id="5d42d-107">Erőforrás-kezelő használata</span><span class="sxs-lookup"><span data-stu-id="5d42d-107">Use Resource Manager</span></span>](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

<span data-ttu-id="5d42d-108">Megtudhatja, hogyan használhatja az Azure Powershellt Azure HDInsight-fürtök konfigurálása az Azure Data Lake Store, alapértelmezett tárolóként.</span><span class="sxs-lookup"><span data-stu-id="5d42d-108">Learn how to use Azure PowerShell to configure Azure HDInsight clusters with Azure Data Lake Store, as default storage.</span></span> <span data-ttu-id="5d42d-109">A HDInsight-fürtök létrehozása a Data Lake Store további tárolóként útmutatásért lásd: [HDInsight-fürtök létrehozása a Data Lake Store további tárolóként](data-lake-store-hdinsight-hadoop-use-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="5d42d-109">For instructions on creating an HDInsight cluster with Data Lake Store as additional storage, see [Create an HDInsight cluster with Data Lake Store as additional storage](data-lake-store-hdinsight-hadoop-use-powershell.md).</span></span>

<span data-ttu-id="5d42d-110">Az alábbiakban a HDInsight a Data Lake Store használatára vonatkozó szempontokat:</span><span class="sxs-lookup"><span data-stu-id="5d42d-110">Here are some important considerations for using HDInsight with Data Lake Store:</span></span>

* <span data-ttu-id="5d42d-111">A HDInsight-fürtök létrehozása a Data Lake Store alapértelmezett tárolóként hozzáférési beállítás HDInsight 3.5-ös és 3.6 verzió érhető el.</span><span class="sxs-lookup"><span data-stu-id="5d42d-111">The option to create HDInsight clusters with access to Data Lake Store as default storage is available for HDInsight version 3.5 and 3.6.</span></span>

* <span data-ttu-id="5d42d-112">Létrehozhat HDInsight-fürtök hozzáféréssel rendelkező Data Lake Store, alapértelmezett tárolási *nem érhető el* a HDInsight prémium fürtök.</span><span class="sxs-lookup"><span data-stu-id="5d42d-112">The option to create HDInsight clusters with access to Data Lake Store as default storage is *not available* for HDInsight Premium clusters.</span></span>

<span data-ttu-id="5d42d-113">A PowerShell használatával a Data Lake Store működéséhez HDInsight konfigurálásához kövesse az utasításokat a következő öt szakaszokban.</span><span class="sxs-lookup"><span data-stu-id="5d42d-113">To configure HDInsight to work with Data Lake Store by using PowerShell, follow the instructions in the next five sections.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5d42d-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5d42d-114">Prerequisites</span></span>
<span data-ttu-id="5d42d-115">Ez az oktatóanyag megkezdése előtt győződjön meg arról, hogy teljesülnek-e az alábbi követelményeknek:</span><span class="sxs-lookup"><span data-stu-id="5d42d-115">Before you begin this tutorial, make sure that you meet the following requirements:</span></span>

* <span data-ttu-id="5d42d-116">**Azure-előfizetés**: Ugrás [beolvasása az Azure ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5d42d-116">**An Azure subscription**: Go to [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="5d42d-117">**Az Azure PowerShell 1.0-ás vagy újabb**: lásd: [telepítése és konfigurálása a PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5d42d-117">**Azure PowerShell 1.0 or greater**: See [How to install and configure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="5d42d-118">**A Windows Software Development Kit (SDK)**: Windows SDK telepítéséhez, [letölti és a Windows 10-eszközök](https://dev.windows.com/en-us/downloads).</span><span class="sxs-lookup"><span data-stu-id="5d42d-118">**Windows Software Development Kit (SDK)**: To install Windows SDK, go to [Downloads and tools for Windows 10](https://dev.windows.com/en-us/downloads).</span></span> <span data-ttu-id="5d42d-119">Az SDK segítségével hozzon létre egy biztonsági tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="5d42d-119">The SDK is used to create a security certificate.</span></span>
* <span data-ttu-id="5d42d-120">**Az Azure Active Directory szolgáltatás egyszerű**: Ez az oktatóanyag ismerteti, hogyan lehet egy egyszerű szolgáltatás létrehozása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5d42d-120">**Azure Active Directory service principal**: This tutorial describes how to create a service principal in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="5d42d-121">Azonban szeretne létrehozni egy egyszerű szolgáltatást, akkor az Azure AD rendszergazdai jogokkal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="5d42d-121">However, to create a service principal, you must be an Azure AD administrator.</span></span> <span data-ttu-id="5d42d-122">Ha Ön rendszergazda, hagyja ki ezt az előfeltételt, és az oktatóanyag folytatásához.</span><span class="sxs-lookup"><span data-stu-id="5d42d-122">If you are an administrator, you can skip this prerequisite and proceed with the tutorial.</span></span>

    >[!NOTE]
    ><span data-ttu-id="5d42d-123">Létrehozhat egy szolgáltatás egyszerű csak akkor, ha az Azure AD-rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="5d42d-123">You can create a service principal only if you are an Azure AD administrator.</span></span> <span data-ttu-id="5d42d-124">Az Azure AD rendszergazdának létre kell hoznia egy szolgáltatás egyszerű HDInsight-fürtök létrehozása a Data Lake Store előtt.</span><span class="sxs-lookup"><span data-stu-id="5d42d-124">Your Azure AD administrator must create a service principal before you can create an HDInsight cluster with Data Lake Store.</span></span> <span data-ttu-id="5d42d-125">A szolgáltatás egyszerű a tanúsítvánnyal kell létrehozni a [hozzon létre egy egyszerű tanúsítvány](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span><span class="sxs-lookup"><span data-stu-id="5d42d-125">The service principal must be created with a certificate, as described in [Create a service principal with certificate](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span></span>
    >

## <a name="create-a-data-lake-store-account"></a><span data-ttu-id="5d42d-126">Data Lake Store-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="5d42d-126">Create a Data Lake Store account</span></span>
<span data-ttu-id="5d42d-127">A Data Lake Store-fiók létrehozásához tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="5d42d-127">To create a Data Lake Store account, do the following:</span></span>

1. <span data-ttu-id="5d42d-128">Az asztalon nyisson meg egy PowerShell-ablakot, és adja meg az alábbi részletek.</span><span class="sxs-lookup"><span data-stu-id="5d42d-128">From your desktop, open a PowerShell window, and then enter the snippets below.</span></span> <span data-ttu-id="5d42d-129">Amikor bejelentkeznek, jelentkezzen be az előfizetés rendszergazdáihoz vagy tulajdonosok egyikeként kéri.</span><span class="sxs-lookup"><span data-stu-id="5d42d-129">When you are prompted to sign in, sign in as one of the subscription administrators or owners.</span></span> 

        # Sign in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    > [!NOTE]
    > <span data-ttu-id="5d42d-130">Ha a Data Lake Store erőforrás-szolgáltató regisztrálása és hasonló hibaüzenetet kap `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid`, az előfizetés nem feltétlenül szerepel az engedélyezési listán a Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="5d42d-130">If you register the Data Lake Store resource provider and receive an error similar to `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid`, your subscription might not be whitelisted for Data Lake Store.</span></span> <span data-ttu-id="5d42d-131">Ahhoz, hogy az Azure-előfizetéshez a Data Lake Store nyilvános előzetes verzióhoz, kövesse az utasításokat a [Ismerkedés az Azure Data Lake Store az Azure portál használatával](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5d42d-131">To enable your Azure subscription for the Data Lake Store public preview, follow the instructions in [Get started with Azure Data Lake Store by using the Azure portal](data-lake-store-get-started-portal.md).</span></span>
    >

2. <span data-ttu-id="5d42d-132">Data Lake Store-fiók tartozik egy Azure-erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="5d42d-132">A Data Lake Store account is associated with an Azure resource group.</span></span> <span data-ttu-id="5d42d-133">Először hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="5d42d-133">Start by creating a resource group.</span></span>

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    <span data-ttu-id="5d42d-134">Ez hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="5d42d-134">You should see an output like this:</span></span>

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. <span data-ttu-id="5d42d-135">Data Lake Store-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5d42d-135">Create a Data Lake Store account.</span></span> <span data-ttu-id="5d42d-136">A megadott fiók neve csak kisbetűket és számokat kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="5d42d-136">The account name you specify must contain only lowercase letters and numbers.</span></span>

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    <span data-ttu-id="5d42d-137">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="5d42d-137">You should see an output like the following:</span></span>

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

4. <span data-ttu-id="5d42d-138">A Data Lake Store alapértelmezett tárolási szükség van, hogy adjon meg egy legfelső szintű elérési utat, amelyhez a fürt fájlokat másolni a fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="5d42d-138">Using Data Lake Store as default storage requires you to specify a root path to which the cluster-specific files are copied during cluster creation.</span></span> <span data-ttu-id="5d42d-139">Egy legfelső szintű elérési útját, amely létrehozásához **/fürtök/hdiadlcluster** a kódrészletet, a következő parancsmag használatával:</span><span class="sxs-lookup"><span data-stu-id="5d42d-139">To create a root path, which is **/clusters/hdiadlcluster** in the  snippet, use the following cmdlets:</span></span>

        $myrootdir = "/"
        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/clusters/hdiadlcluster


## <a name="set-up-authentication-for-role-based-access-to-data-lake-store"></a><span data-ttu-id="5d42d-140">A szerepköralapú hozzáférés-Data Lake Store-hitelesítés beállítása</span><span class="sxs-lookup"><span data-stu-id="5d42d-140">Set up authentication for role-based access to Data Lake Store</span></span>
<span data-ttu-id="5d42d-141">Minden Azure-előfizetés nem tartozik az Azure AD entitás.</span><span class="sxs-lookup"><span data-stu-id="5d42d-141">Every Azure subscription is associated with an Azure AD entity.</span></span> <span data-ttu-id="5d42d-142">Felhasználók és a szolgáltatások, amelyek az előfizetéshez kapcsolódó erőforrásokat elérni az Azure-portálon vagy az Azure Resource Manager API-t először hitelesítenie kell magát az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5d42d-142">Users and services that access subscription resources by using the Azure portal or the Azure Resource Manager API must first authenticate with Azure AD.</span></span> <span data-ttu-id="5d42d-143">Hozzáférés az Azure-előfizetések és-szolgáltatások egy Azure-erőforrás a megfelelő szerepkört hozzárendelésével.</span><span class="sxs-lookup"><span data-stu-id="5d42d-143">Access is granted to Azure subscriptions and services by assigning them the appropriate role on an Azure resource.</span></span> <span data-ttu-id="5d42d-144">Szolgáltatások esetén egy egyszerű szolgáltatás azonosítja a szolgáltatást az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="5d42d-144">For services, a service principal identifies the service in Azure AD.</span></span>

<span data-ttu-id="5d42d-145">Ez a szakasz bemutatja, hogyan alkalmazásszolgáltatás, például a HDInsight hozzáférést az Azure-erőforrás (a Data Lake Store-fiók korábban létrehozott) megadását.</span><span class="sxs-lookup"><span data-stu-id="5d42d-145">This section illustrates how to grant an application service, such as HDInsight, access to an Azure resource (the Data Lake Store account that you created earlier).</span></span> <span data-ttu-id="5d42d-146">Ehhez az alkalmazás és a PowerShell hozzá hozzárendelése szerepkörök egyszerű szolgáltatás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5d42d-146">You do so by creating a service principal for the application and assigning roles to it via PowerShell.</span></span>

<span data-ttu-id="5d42d-147">Active Directory-hitelesítés az Azure Data Lake beállításához hajtsa végre a következő két szakasz a feladatok.</span><span class="sxs-lookup"><span data-stu-id="5d42d-147">To set up Active Directory authentication for Azure Data Lake, perform the tasks in the following two sections.</span></span>

### <a name="create-a-self-signed-certificate"></a><span data-ttu-id="5d42d-148">Önaláírt tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="5d42d-148">Create a self-signed certificate</span></span>
<span data-ttu-id="5d42d-149">Győződjön meg arról, hogy [Windows SDK](https://dev.windows.com/en-us/downloads) ebben a szakaszban a lépések végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="5d42d-149">Make sure you have [Windows SDK](https://dev.windows.com/en-us/downloads) installed before proceeding with the steps in this section.</span></span> <span data-ttu-id="5d42d-150">Kell is létrehozott egy könyvtárat, például a *C:\mycertdir*, ahol a tanúsítvány létrehozását.</span><span class="sxs-lookup"><span data-stu-id="5d42d-150">You must have also created a directory, such as *C:\mycertdir*, where you create the certificate.</span></span>

1. <span data-ttu-id="5d42d-151">A PowerShell ablakban nyissa meg a helyet, amelyre telepítette a Windows SDK (általában *C:\Program Files (x86) \Windows Kits\10\bin\x86*), és használja a [MakeCert] [ makecert] segédprogram egy önaláírt tanúsítványt és a titkos kulcs létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5d42d-151">From the PowerShell window, go to the location where you installed Windows SDK (typically, *C:\Program Files (x86)\Windows Kits\10\bin\x86*) and use the [MakeCert][makecert] utility to create a self-signed certificate and a private key.</span></span> <span data-ttu-id="5d42d-152">Az alábbi parancsokat használja:</span><span class="sxs-lookup"><span data-stu-id="5d42d-152">Use the following commands:</span></span>

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    <span data-ttu-id="5d42d-153">A rendszer bekéri a titkos kulcsok jelszavának megadása.</span><span class="sxs-lookup"><span data-stu-id="5d42d-153">You will be prompted to enter the private key password.</span></span> <span data-ttu-id="5d42d-154">Miután a parancs végrehajtása sikeres, megtekintheti az **CertFile.cer** és **mykey.pvk** megadott tanúsítvány könyvtárban található.</span><span class="sxs-lookup"><span data-stu-id="5d42d-154">After the command is successfully executed, you should see **CertFile.cer** and **mykey.pvk** in the certificate directory that you specified.</span></span>
2. <span data-ttu-id="5d42d-155">Használja a [Pvk2Pfx] [ pvk2pfx] segédprogram a MakeCert létrehozott .pvk és .cer fájl átalakítása egy .pfx fájlba.</span><span class="sxs-lookup"><span data-stu-id="5d42d-155">Use the [Pvk2Pfx][pvk2pfx] utility to convert the .pvk and .cer files that MakeCert created to a .pfx file.</span></span> <span data-ttu-id="5d42d-156">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="5d42d-156">Run the following command:</span></span>

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    <span data-ttu-id="5d42d-157">Amikor a rendszer kéri, adja meg a korábban megadott titkos kulcs jelszava.</span><span class="sxs-lookup"><span data-stu-id="5d42d-157">When you are prompted, enter the private key password that you specified earlier.</span></span> <span data-ttu-id="5d42d-158">A megadott érték a **-po** paramétere a jelszavát, amelyet a .pfx fájl van társítva.</span><span class="sxs-lookup"><span data-stu-id="5d42d-158">The value you specify for the **-po** parameter is the password that's associated with the .pfx file.</span></span> <span data-ttu-id="5d42d-159">A parancs sikeres befejezése után, emellett meg kell jelennie egy **CertFile.pfx** megadott tanúsítvány könyvtárban található.</span><span class="sxs-lookup"><span data-stu-id="5d42d-159">After the command has been completed successfully, you should also see a **CertFile.pfx** in the certificate directory that you specified.</span></span>

### <a name="create-an-azure-ad-and-a-service-principal"></a><span data-ttu-id="5d42d-160">Hozzon létre egy Azure AD és az egyszerű szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="5d42d-160">Create an Azure AD and a service principal</span></span>
<span data-ttu-id="5d42d-161">Ebben a szakaszban egy egyszerű szolgáltatást az Azure AD-alkalmazás létrehozása, szerepkör hozzárendelése az egyszerű szolgáltatás, és hitelesítse magát a szolgáltatás egyszerű, adja meg a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="5d42d-161">In this section, you create a service principal for an Azure AD application, assign a role to the service principal, and authenticate as the service principal by providing a certificate.</span></span> <span data-ttu-id="5d42d-162">Alkalmazás létrehozása az Azure ad-ben, a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="5d42d-162">To create an application in Azure AD, run the following commands:</span></span>

1. <span data-ttu-id="5d42d-163">A PowerShell-konzolablakot illessze be a következő parancsmagokat.</span><span class="sxs-lookup"><span data-stu-id="5d42d-163">Paste the following cmdlets in the PowerShell console window.</span></span> <span data-ttu-id="5d42d-164">Győződjön meg arról, hogy a értéket állít be a **- DisplayName** tulajdonság értéke egyedi.</span><span class="sxs-lookup"><span data-stu-id="5d42d-164">Make sure that the value you specify for the **-DisplayName** property is unique.</span></span> <span data-ttu-id="5d42d-165">Az értékek **- kezdőlap** és **- IdentiferUris** helyőrző értékeket, és nem ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="5d42d-165">The values for **-HomePage** and **-IdentiferUris** are placeholder values and are not verified.</span></span>

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host –Prompt "Enter the password" # This is the password you specified for the .pfx file

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
2. <span data-ttu-id="5d42d-166">Hozzon létre egy egyszerű szolgáltatás által az alkalmazás azonosítójával.</span><span class="sxs-lookup"><span data-stu-id="5d42d-166">Create a service principal by using the application ID.</span></span>

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. <span data-ttu-id="5d42d-167">Adjon a szolgáltatás egyszerű hozzáférést a Data Lake Store legfelső szintű és a legfelső szintű található, amely korábban meghatározott összes mappa.</span><span class="sxs-lookup"><span data-stu-id="5d42d-167">Grant the service principal access to the Data Lake Store root and all the folders in the root path that you specified earlier.</span></span> <span data-ttu-id="5d42d-168">A következő parancsmagokat használja:</span><span class="sxs-lookup"><span data-stu-id="5d42d-168">Use the following cmdlets:</span></span>

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters/hdiadlcluster -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-the-default-storage"></a><span data-ttu-id="5d42d-169">Egy HDInsight Linux-fürt létrehozása a Data Lake Store az alapértelmezett tároló</span><span class="sxs-lookup"><span data-stu-id="5d42d-169">Create an HDInsight Linux cluster with Data Lake Store as the default storage</span></span>

<span data-ttu-id="5d42d-170">Ebben a szakaszban egy HDInsight Hadoop Linux fürt létrehozása a Data Lake Store az alapértelmezett tárolóként.</span><span class="sxs-lookup"><span data-stu-id="5d42d-170">In this section, you create an HDInsight Hadoop Linux cluster with Data Lake Store as the default storage.</span></span> <span data-ttu-id="5d42d-171">Ebben a kiadásban a HDInsight-fürt és a Data Lake Store ugyanazon a helyen kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5d42d-171">For this release, the HDInsight cluster and Data Lake Store must be in the same location.</span></span>

1. <span data-ttu-id="5d42d-172">A bérlő előfizetés-azonosító lekérése, és a későbbi használat céljából tárolja.</span><span class="sxs-lookup"><span data-stu-id="5d42d-172">Retrieve the subscription tenant ID, and store it to use later.</span></span>

        $tenantID = (Get-AzureRmContext).Tenant.TenantId

2. <span data-ttu-id="5d42d-173">A HDInsight-fürt létrehozása a következő parancsmag használatával:</span><span class="sxs-lookup"><span data-stu-id="5d42d-173">Create the HDInsight cluster by using the following cmdlets:</span></span>

        # Set these variables

        $location = "East US 2"
        $storageAccountName = $dataLakeStoreName                       # Data Lake Store account name
        $storageRootPath = "<Storage root path you specified earlier>" # E.g. /clusters/hdiadlcluster
        $clusterName = "<unique cluster name>"
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
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

    <span data-ttu-id="5d42d-174">A parancsmag sikeres befejezése után, akkor a fürt részleteket felsoroló kimenetnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="5d42d-174">After the cmdlet has been successfully completed, you should see an output that lists the cluster details.</span></span>

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-data-lake-store"></a><span data-ttu-id="5d42d-175">A HDInsight-fürt használata a Data Lake Store teszt feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="5d42d-175">Run test jobs on the HDInsight cluster to use Data Lake Store</span></span>
<span data-ttu-id="5d42d-176">Miután konfigurálta a HDInsight-fürtöt, a teszt feladatokat, és győződjön meg arról, hogy hozzáférhessen Data Lake Store is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="5d42d-176">After you have configured an HDInsight cluster, you can run test jobs on it to ensure that it can access Data Lake Store.</span></span> <span data-ttu-id="5d42d-177">Ehhez az szükséges, hozzon létre egy táblát, amely a rendelkezésre álló a Data Lake Store: mintaadatok használ egy minta Hive feladat futtatása  *<cluster root>/example/data/sample.log*.</span><span class="sxs-lookup"><span data-stu-id="5d42d-177">To do so, run a sample Hive job to create a table that uses the sample data that's already available in Data Lake Store at *<cluster root>/example/data/sample.log*.</span></span>

<span data-ttu-id="5d42d-178">Ebben a szakaszban a Secure Shell (SSH) kapcsolat a HDInsight Linux fürthöz létrehozott, és egy minta Hive-lekérdezést futtassa.</span><span class="sxs-lookup"><span data-stu-id="5d42d-178">In this section, you make a Secure Shell (SSH) connection into the HDInsight Linux cluster that you created, and then you run a sample Hive query.</span></span>

* <span data-ttu-id="5d42d-179">Ha egy Windows ügyfél használ a fürthöz az SSH-kapcsolat létrehozásához, lásd: [SSH használata a HDInsight Windows Linux-alapú Hadooppal](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="5d42d-179">If you are using a Windows client to make an SSH connection into the cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>
* <span data-ttu-id="5d42d-180">Ha egy Linux-ügyfél segítségével ellenőrizze a fürthöz az SSH-kapcsolat, lásd: [SSH használata a HDInsight Linux Linux-alapú Hadooppal](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="5d42d-180">If you are using a Linux client to make an SSH connection into the cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

1. <span data-ttu-id="5d42d-181">Miután kiválasztotta a kapcsolatot, a következő parancs használatával indítsa el a Hive parancssori felület (CLI):</span><span class="sxs-lookup"><span data-stu-id="5d42d-181">After you have made the connection, start the Hive command-line interface (CLI) by using the following command:</span></span>

        hive
2. <span data-ttu-id="5d42d-182">Adja meg az alábbi állításokat annak nevű új tábla létrehozása a parancssori felület használatával **járművekről gyűjtött** által megadott mintaadatokat használja a Data Lake Store-ban:</span><span class="sxs-lookup"><span data-stu-id="5d42d-182">Use the CLI to enter the following statements to create a new table named **vehicles** by using the sample data in Data Lake Store:</span></span>

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'adl:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    <span data-ttu-id="5d42d-183">Az SSH-konzolon megtekintheti a lekérdezés eredményében.</span><span class="sxs-lookup"><span data-stu-id="5d42d-183">You should see the query output on the SSH console.</span></span>

    >[!NOTE]
    ><span data-ttu-id="5d42d-184">A mintaadatok az előző CREATE TABLE parancsban szereplő elérési útja `adl:///example/data/`, ahol `adl:///` fürt gyökere.</span><span class="sxs-lookup"><span data-stu-id="5d42d-184">The path to the sample data in the preceding CREATE TABLE command is `adl:///example/data/`, where `adl:///` is the cluster root.</span></span> <span data-ttu-id="5d42d-185">A példa a fürt legfelső szintű megadott ebben az oktatóanyagban a parancs a következő `adl://hdiadlstore.azuredatalakestore.net/clusters/hdiadlcluster`.</span><span class="sxs-lookup"><span data-stu-id="5d42d-185">Following the example of the cluster root that's specified in this tutorial, the command is `adl://hdiadlstore.azuredatalakestore.net/clusters/hdiadlcluster`.</span></span> <span data-ttu-id="5d42d-186">A rövidebb alternatív használja, vagy adja meg a fürt legfelső szintű teljes elérési útja.</span><span class="sxs-lookup"><span data-stu-id="5d42d-186">You can either use the shorter alternative or provide the complete path to the cluster root.</span></span>
    >

## <a name="access-data-lake-store-by-using-hdfs-commands"></a><span data-ttu-id="5d42d-187">Hozzáférés Data Lake Store HDFS parancs használatával</span><span class="sxs-lookup"><span data-stu-id="5d42d-187">Access Data Lake Store by using HDFS commands</span></span>
<span data-ttu-id="5d42d-188">Miután konfigurálta a HDInsight-fürt Data Lake Store használatára, használhatja a Hadoop elosztott fájlrendszerrel (HDFS) felületparancsokat-tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="5d42d-188">After you have configured the HDInsight cluster to use Data Lake Store, you can use Hadoop Distributed File System (HDFS) shell commands to access the store.</span></span>

<span data-ttu-id="5d42d-189">Ebben a szakaszban a HDInsight Linux fürthöz létrehozott egy SSH-kapcsolatot, és a HDFS parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="5d42d-189">In this section, you make an SSH connection into the HDInsight Linux cluster that you created, and then you run the HDFS commands.</span></span>

* <span data-ttu-id="5d42d-190">Ha egy Windows ügyfél használ a fürthöz az SSH-kapcsolat létrehozásához, lásd: [SSH használata a HDInsight Windows Linux-alapú Hadooppal](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="5d42d-190">If you are using a Windows client to make an SSH connection into the cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>
* <span data-ttu-id="5d42d-191">Ha egy Linux-ügyfél segítségével ellenőrizze a fürthöz az SSH-kapcsolat, lásd: [SSH használata a HDInsight Linux Linux-alapú Hadooppal](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="5d42d-191">If you are using a Linux client to make an SSH connection into the cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="5d42d-192">Miután végzett a kapcsolatot, a Data Lake Store található fájlok listázása a következő paranccsal HDFS fájl rendszer.</span><span class="sxs-lookup"><span data-stu-id="5d42d-192">After you've made the connection, list the files in Data Lake Store by using the following HDFS file system command.</span></span>

    hdfs dfs -ls adl:///

<span data-ttu-id="5d42d-193">Használhatja a `hdfs dfs -put` parancs egyes fájlok feltöltése a Data Lake Store-ba, és ezután `hdfs dfs -ls` ellenőrzése, hogy a fájlok sikeresen feltöltve.</span><span class="sxs-lookup"><span data-stu-id="5d42d-193">You can also use the `hdfs dfs -put` command to upload some files to Data Lake Store, and then use `hdfs dfs -ls` to verify whether the files were successfully uploaded.</span></span>

## <a name="see-also"></a><span data-ttu-id="5d42d-194">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="5d42d-194">See also</span></span>
* [<span data-ttu-id="5d42d-195">Azure-portálon: Data Lake Store használatára HDInsight-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="5d42d-195">Azure portal: Create an HDInsight cluster to use Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
