---
title: "PowerShell: A Data Lake Store bővítmény tárolóként az Azure HDInsight fürt |} Microsoft Docs"
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 164ada5a-222e-4be2-bd32-e51dbe993bc0
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/08/2017
ms.author: nitinme
ms.openlocfilehash: 7a7069adab5742a9dae2833c13a1db57337a41a0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-powershell-to-create-an-hdinsight-cluster-with-data-lake-store-as-additional-storage"></a><span data-ttu-id="7039f-102">Azure PowerShell használata a HDInsight-fürtök létrehozása a Data Lake Store (a további tárhely)</span><span class="sxs-lookup"><span data-stu-id="7039f-102">Use Azure PowerShell to create an HDInsight cluster with Data Lake Store (as additional storage)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7039f-103">A Portal használata</span><span class="sxs-lookup"><span data-stu-id="7039f-103">Using Portal</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [<span data-ttu-id="7039f-104">PowerShell használatával (az alapértelmezett tároló)</span><span class="sxs-lookup"><span data-stu-id="7039f-104">Using PowerShell (for default storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [<span data-ttu-id="7039f-105">(Tárhely) a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="7039f-105">Using PowerShell (for additional storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [<span data-ttu-id="7039f-106">Erőforrás-kezelő használatával</span><span class="sxs-lookup"><span data-stu-id="7039f-106">Using Resource Manager</span></span>](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

<span data-ttu-id="7039f-107">Azure PowerShell használata a HDInsight-fürtök konfigurálása az Azure Data Lake Store **további tárolóként**.</span><span class="sxs-lookup"><span data-stu-id="7039f-107">Learn how to use Azure PowerShell to configure an HDInsight cluster with Azure Data Lake Store, **as additional storage**.</span></span> <span data-ttu-id="7039f-108">A HDInsight-fürtök létrehozása az Azure Data Lake Store alapértelmezett tárolóként útmutatásért lásd: [HDInsight-fürtök létrehozása a Data Lake Store alapértelmezett tárolóként](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md).</span><span class="sxs-lookup"><span data-stu-id="7039f-108">For instructions on how to create an HDInsight cluster with Azure Data Lake Store as default storage, see [Create an HDInsight cluster with Data Lake Store as default storage](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md).</span></span>

> [!NOTE]
> <span data-ttu-id="7039f-109">Ha a Data Lake Store-t a HDInsight-fürt kiegészítő tárolójaként tervezi használni, akkor kifejezetten ezt a megoldást javasoljuk, amikor létrehozza a fürtöt az ebben a cikkben leírtaknak megfelelően.</span><span class="sxs-lookup"><span data-stu-id="7039f-109">If you are going to use Azure Data Lake Store as additional storage for HDInsight cluster, we strongly recommend that you do this while you create the cluster as described in this article.</span></span> <span data-ttu-id="7039f-110">Az Azure Data Lake Store-t kiegészítő tárolóként hozzáadni egy meglévő HDInsight-fürthöz bonyolult folyamat, és sok hibalehetőséggel jár.</span><span class="sxs-lookup"><span data-stu-id="7039f-110">Adding Azure Data Lake Store as additional storage to an existing HDInsight cluster is a complicated process and prone to errors.</span></span>
>

<span data-ttu-id="7039f-111">Támogatott fürttípusok Data Lake Store egy alapértelmezett tároló vagy a további tárhely fiókként használható.</span><span class="sxs-lookup"><span data-stu-id="7039f-111">For supported cluster types, Data Lake Store can be used as a default storage or additional storage account.</span></span> <span data-ttu-id="7039f-112">Ha a Data Lake Store további tárterületet, az alapértelmezett tárfiókot, a fürt továbbra is Azure Storage Blobs (WASB), és a fürt kapcsolatos (például naplói, stb.) továbbra is írja a fájlt, alapértelmezett tárolására, amíg a Data Lake Store-fiók tárolhatja az adatokat, hogy fel szeretné dolgoztatni.</span><span class="sxs-lookup"><span data-stu-id="7039f-112">When Data Lake Store is used as additional storage, the default storage account for the clusters will still be Azure Storage Blobs (WASB) and the cluster-related files (such as logs, etc.) are still written to the default storage, while the data that you want to process can be stored in a Data Lake Store account.</span></span> <span data-ttu-id="7039f-113">További tárhely fiókként használatával a Data Lake Store nem befolyásolja a teljesítményt vagy olvasására vagy írására a tárhelyet a fürtből történő alkalmazásának képességét.</span><span class="sxs-lookup"><span data-stu-id="7039f-113">Using Data Lake Store as an additional storage account does not impact performance or the ability to read/write to the storage from the cluster.</span></span>

## <a name="using-data-lake-store-for-hdinsight-cluster-storage"></a><span data-ttu-id="7039f-114">Data Lake Store használata a HDInsight-fürt tárolására</span><span class="sxs-lookup"><span data-stu-id="7039f-114">Using Data Lake Store for HDInsight cluster storage</span></span>

<span data-ttu-id="7039f-115">Az alábbiakban a HDInsight a Data Lake Store használatára vonatkozó szempontokat:</span><span class="sxs-lookup"><span data-stu-id="7039f-115">Here are some important considerations for using HDInsight with Data Lake Store:</span></span>

* <span data-ttu-id="7039f-116">A HDInsight-fürtök létrehozása a Data Lake Store elérését, további tárhely áll rendelkezésre a HDInsight-verziókról 3.2-es, 3.4, 3.5-ös és 3.6 lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="7039f-116">Option to create HDInsight clusters with access to Data Lake Store as additional storage is available for HDInsight versions 3.2, 3.4, 3.5, and 3.6.</span></span>

<span data-ttu-id="7039f-117">HDInsight a Data Lake Store működéséhez konfigurálása PowerShell használatával az alábbi lépésekből:</span><span class="sxs-lookup"><span data-stu-id="7039f-117">Configuring HDInsight to work with Data Lake Store using PowerShell involves the following steps:</span></span>

* <span data-ttu-id="7039f-118">Hozzon létre egy Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="7039f-118">Create an Azure Data Lake Store</span></span>
* <span data-ttu-id="7039f-119">A szerepköralapú hozzáférés-Data Lake Store-hitelesítés beállítása</span><span class="sxs-lookup"><span data-stu-id="7039f-119">Set up authentication for role-based access to Data Lake Store</span></span>
* <span data-ttu-id="7039f-120">HDInsight-fürt létrehozása a Data Lake Store-hitelesítéssel</span><span class="sxs-lookup"><span data-stu-id="7039f-120">Create HDInsight cluster with authentication to Data Lake Store</span></span>
* <span data-ttu-id="7039f-121">Egy tesztelési feladat futtatása a fürtön</span><span class="sxs-lookup"><span data-stu-id="7039f-121">Run a test job on the cluster</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7039f-122">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7039f-122">Prerequisites</span></span>
<span data-ttu-id="7039f-123">Az oktatóanyag elkezdéséhez az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="7039f-123">Before you begin this tutorial, you must have the following:</span></span>

* <span data-ttu-id="7039f-124">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="7039f-124">**An Azure subscription**.</span></span> <span data-ttu-id="7039f-125">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7039f-125">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="7039f-126">Az **Azure PowerShell 1.0-s vagy újabb verziója**.</span><span class="sxs-lookup"><span data-stu-id="7039f-126">**Azure PowerShell 1.0 or greater**.</span></span> <span data-ttu-id="7039f-127">Lásd: [How to install and configure Azure PowerShell](/powershell/azure/overview) (Az Azure PowerShell telepítése és konfigurálása).</span><span class="sxs-lookup"><span data-stu-id="7039f-127">See [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="7039f-128">**Windows SDK**.</span><span class="sxs-lookup"><span data-stu-id="7039f-128">**Windows SDK**.</span></span> <span data-ttu-id="7039f-129">A későbbiekben telepítheti az [Itt](https://dev.windows.com/en-us/downloads).</span><span class="sxs-lookup"><span data-stu-id="7039f-129">You can install it from [here](https://dev.windows.com/en-us/downloads).</span></span> <span data-ttu-id="7039f-130">Ezzel a biztonsági tanúsítvány létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="7039f-130">You use this to create a security certificate.</span></span>
* <span data-ttu-id="7039f-131">**Az Azure Active Directory szolgáltatás egyszerű**.</span><span class="sxs-lookup"><span data-stu-id="7039f-131">**Azure Active Directory Service Principal**.</span></span> <span data-ttu-id="7039f-132">Ez az oktatóanyag lépéseit ad útmutatást az egyszerű szolgáltatás létrehozása az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="7039f-132">Steps in this tutorial provide instructions on how to create a service principal in Azure AD.</span></span> <span data-ttu-id="7039f-133">Azonban az Azure AD a rendszergazda létrehozhat egy egyszerű szolgáltatást kell lennie.</span><span class="sxs-lookup"><span data-stu-id="7039f-133">However, you must be an Azure AD administrator to be able to create a service principal.</span></span> <span data-ttu-id="7039f-134">Ha az Azure AD-rendszergazdaként, hagyja ki ezt az előfeltételt, és az oktatóanyag folytatásához.</span><span class="sxs-lookup"><span data-stu-id="7039f-134">If you are an Azure AD administrator, you can skip this prerequisite and proceed with the tutorial.</span></span>

    <span data-ttu-id="7039f-135">**Ha nem az Azure AD-rendszergazda**, nem fog tudni egy egyszerű szolgáltatásnév létrehozásához szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="7039f-135">**If you are not an Azure AD administrator**, you will not be able to perform the steps required to create a service principal.</span></span> <span data-ttu-id="7039f-136">Ebben az esetben az Azure AD-rendszergazda először létre kell hoznia egy egyszerű szolgáltatást a Data Lake Store egy HDInsight-fürt létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="7039f-136">In such a case, your Azure AD administrator must first create a service principal before you can create an HDInsight cluster with Data Lake Store.</span></span> <span data-ttu-id="7039f-137">Emellett az egyszerű szolgáltatás segítségével kell létrehozni egy tanúsítványt, részben ismertetett módon [hozzon létre egy egyszerű tanúsítvány](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span><span class="sxs-lookup"><span data-stu-id="7039f-137">Also, the service principal must be created using a certificate, as described at [Create a service principal with certificate](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span></span>

## <a name="create-an-azure-data-lake-store"></a><span data-ttu-id="7039f-138">Hozzon létre egy Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="7039f-138">Create an Azure Data Lake Store</span></span>
<span data-ttu-id="7039f-139">Kövesse az alábbi lépéseket egy Data Lake Store létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="7039f-139">Follow these steps to create a Data Lake Store.</span></span>

1. <span data-ttu-id="7039f-140">Az asztalon nyisson meg egy új Azure PowerShell-ablakot, és adja meg a következő kódrészletet.</span><span class="sxs-lookup"><span data-stu-id="7039f-140">From your desktop, open a new Azure PowerShell window, and enter the following snippet.</span></span> <span data-ttu-id="7039f-141">Amikor a rendszer kéri-e jelentkezni, győződjön meg arról jelentkezik be a rendelkezésre álló az előfizetés rendszergazdája vagy tulajdonosa:</span><span class="sxs-lookup"><span data-stu-id="7039f-141">When prompted to log in, make sure you log in as one of the subscription administrator/owner:</span></span>

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

   > [!NOTE]
   > <span data-ttu-id="7039f-142">Ha a hibaüzenet hasonló `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid` a Data Lake Store erőforrás-szolgáltató regisztrálása, esetén lehetséges, hogy az előfizetés nem szerepel az Azure Data Lake Store az engedélyezési listán.</span><span class="sxs-lookup"><span data-stu-id="7039f-142">If you receive an error similar to `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid` when registering the Data Lake Store resource provider, it is possible that your subscription is not whitelisted for Azure Data Lake Store.</span></span> <span data-ttu-id="7039f-143">Győződjön meg arról, hogy az Azure-előfizetéshez a Data Lake Store nyilvános előzetes verziójához engedélyezze a következő [utasításokat](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7039f-143">Make sure you enable your Azure subscription for Data Lake Store public preview by following these [instructions](data-lake-store-get-started-portal.md).</span></span>
   >
   >
2. <span data-ttu-id="7039f-144">Az Azure Data Lake Store-fiókok egy Azure-erőforráscsoporthoz vannak társítva.</span><span class="sxs-lookup"><span data-stu-id="7039f-144">An Azure Data Lake Store account is associated with an Azure Resource Group.</span></span> <span data-ttu-id="7039f-145">Először hozzon létre egy Azure-erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="7039f-145">Start by creating an Azure Resource Group.</span></span>

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    <span data-ttu-id="7039f-146">Ez hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="7039f-146">You should see an output like this:</span></span>

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. <span data-ttu-id="7039f-147">Hozzon létre egy Azure Data Lake Store-fiókot.</span><span class="sxs-lookup"><span data-stu-id="7039f-147">Create an Azure Data Lake Store account.</span></span> <span data-ttu-id="7039f-148">A megadott fiók neve csak kisbetűket és számokat tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="7039f-148">The account name you specify must only contain lowercase letters and numbers.</span></span>

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    <span data-ttu-id="7039f-149">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="7039f-149">You should see an output like the following:</span></span>

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

5. <span data-ttu-id="7039f-150">Azure Data Lake tölthet fel néhány adatot.</span><span class="sxs-lookup"><span data-stu-id="7039f-150">Upload some sample data to Azure Data Lake.</span></span> <span data-ttu-id="7039f-151">Használjuk Ez a cikk későbbi részében annak ellenőrzéséhez, hogy az adatok érhető el a HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="7039f-151">We'll use this later in this article to verify that the data is accessible from an HDInsight cluster.</span></span> <span data-ttu-id="7039f-152">Ha feltölthető mintaadatokra van szüksége, használhatja az [Azure Data Lake Git-tárában](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData) található **Ambulance Data** mappát.</span><span class="sxs-lookup"><span data-stu-id="7039f-152">If you are looking for some sample data to upload, you can get the **Ambulance Data** folder from the [Azure Data Lake Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span></span>

        $myrootdir = "/"
        Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\<path to data>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## <a name="set-up-authentication-for-role-based-access-to-data-lake-store"></a><span data-ttu-id="7039f-153">A szerepköralapú hozzáférés-Data Lake Store-hitelesítés beállítása</span><span class="sxs-lookup"><span data-stu-id="7039f-153">Set up authentication for role-based access to Data Lake Store</span></span>
<span data-ttu-id="7039f-154">Egy Azure Active Directory minden Azure-előfizetés tartozik.</span><span class="sxs-lookup"><span data-stu-id="7039f-154">Every Azure subscription is associated with an Azure Active Directory.</span></span> <span data-ttu-id="7039f-155">Felhasználók és a szolgáltatások, az előfizetés, a klasszikus Azure portálon vagy az Azure Resource Manager API-t használó erőforrásokat elérő először hitelesítenie kell magát, hogy Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="7039f-155">Users and services that access resources of the subscription using the Azure Classic Portal or Azure Resource Manager API must first authenticate with that Azure Active Directory.</span></span> <span data-ttu-id="7039f-156">Hozzáférés az Azure-előfizetések és-szolgáltatások egy Azure-erőforrás a megfelelő szerepkört hozzárendelésével.</span><span class="sxs-lookup"><span data-stu-id="7039f-156">Access is granted to Azure subscriptions and services by assigning them the appropriate role on an Azure resource.</span></span>  <span data-ttu-id="7039f-157">Szolgáltatások esetén egy egyszerű szolgáltatást a szolgáltatás az Azure Active Directory (AAD) a azonosítja.</span><span class="sxs-lookup"><span data-stu-id="7039f-157">For services, a service principal identifies the service in the Azure Active Directory (AAD).</span></span> <span data-ttu-id="7039f-158">Ez a szakasz bemutatja az alkalmazásszolgáltatás, mint például a HDInsight, egy Azure-erőforrás (a korábban létrehozott Azure Data Lake Store fióknak) való hozzáférés engedélyezése az alkalmazás egyszerű szolgáltatás létrehozása és hozzárendelése a szerepkörök, amelyek Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7039f-158">This section illustrates how to grant an application service, like HDInsight, access to an Azure resource (the Azure Data Lake Store account you created earlier) by creating a service principal for the application and assigning roles to that via Azure PowerShell.</span></span>

<span data-ttu-id="7039f-159">Active Directory-hitelesítés az Azure Data Lake beállításához a következő feladatokat kell elvégeznie.</span><span class="sxs-lookup"><span data-stu-id="7039f-159">To set up Active Directory authentication for Azure Data Lake, you must perform the following tasks.</span></span>

* <span data-ttu-id="7039f-160">Önaláírt tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="7039f-160">Create a self-signed certificate</span></span>
* <span data-ttu-id="7039f-161">Létrehoz egy alkalmazást az Azure Active Directory és az egyszerű szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="7039f-161">Create an application in Azure Active Directory and a Service Principal</span></span>

### <a name="create-a-self-signed-certificate"></a><span data-ttu-id="7039f-162">Önaláírt tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="7039f-162">Create a self-signed certificate</span></span>
<span data-ttu-id="7039f-163">Győződjön meg arról, hogy [Windows SDK](https://dev.windows.com/en-us/downloads) ebben a szakaszban a lépések végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="7039f-163">Make sure you have [Windows SDK](https://dev.windows.com/en-us/downloads) installed before proceeding with the steps in this section.</span></span> <span data-ttu-id="7039f-164">Kell is létrehozott egy könyvtárat, például a **C:\mycertdir**, ahol a tanúsítvány jön létre.</span><span class="sxs-lookup"><span data-stu-id="7039f-164">You must have also created a directory, such as **C:\mycertdir**, where the certificate will be created.</span></span>

1. <span data-ttu-id="7039f-165">A PowerShell ablakban keresse meg a helyet, amelyre telepítette a Windows SDK (általában `C:\Program Files (x86)\Windows Kits\10\bin\x86` , és használja a [MakeCert] [ makecert] segédprogram egy önaláírt tanúsítványt és a titkos kulcs létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7039f-165">From the PowerShell window, navigate to the location where you installed Windows SDK (typically, `C:\Program Files (x86)\Windows Kits\10\bin\x86` and use the [MakeCert][makecert] utility to create a self-signed certificate and a private key.</span></span> <span data-ttu-id="7039f-166">Az alábbi parancsokkal.</span><span class="sxs-lookup"><span data-stu-id="7039f-166">Use the following commands.</span></span>

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    <span data-ttu-id="7039f-167">A rendszer bekéri a titkos kulcsok jelszavának megadása.</span><span class="sxs-lookup"><span data-stu-id="7039f-167">You will be prompted to enter the private key password.</span></span> <span data-ttu-id="7039f-168">Miután a parancs sikeres végrehajtása során, megtekintheti az egy **CertFile.cer** és **mykey.pvk** a tanúsítvány megadott könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="7039f-168">After the command successfully executes, you should see a **CertFile.cer** and **mykey.pvk** in the certificate directory you specified.</span></span>
2. <span data-ttu-id="7039f-169">Használja a [Pvk2Pfx] [ pvk2pfx] segédprogram a MakeCert létrehozott .pvk és .cer fájl átalakítása egy .pfx fájlba.</span><span class="sxs-lookup"><span data-stu-id="7039f-169">Use the [Pvk2Pfx][pvk2pfx] utility to convert the .pvk and .cer files that MakeCert created to a .pfx file.</span></span> <span data-ttu-id="7039f-170">A következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="7039f-170">Run the following command.</span></span>

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    <span data-ttu-id="7039f-171">Amikor a rendszer kéri a korábban meghatározott titkos kulcsok jelszavának megadása.</span><span class="sxs-lookup"><span data-stu-id="7039f-171">When prompted enter the private key password you specified earlier.</span></span> <span data-ttu-id="7039f-172">A megadott érték a **-po** paramétere a jelszót a .pfx fájl társított.</span><span class="sxs-lookup"><span data-stu-id="7039f-172">The value you specify for the **-po** parameter is the password that is associated with the .pfx file.</span></span> <span data-ttu-id="7039f-173">Ha a parancs sikeresen befejeződött, emellett meg kell jelennie egy CertFile.pfx a tanúsítvány megadott könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="7039f-173">After the command successfully completes, you should also see a CertFile.pfx in the certificate directory you specified.</span></span>

### <a name="create-an-azure-active-directory-and-a-service-principal"></a><span data-ttu-id="7039f-174">Egy Azure Active Directory és az egyszerű szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="7039f-174">Create an Azure Active Directory and a service principal</span></span>
<span data-ttu-id="7039f-175">Ebben a szakaszban hajtsa végre a lépéseket egy egyszerű szolgáltatásnév létrehozása az Azure Active Directory-alkalmazás, a szerepkör hozzárendelése az egyszerű szolgáltatásnév és hitelesítse magát a szolgáltatás egyszerű, adja meg a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="7039f-175">In this section, you perform the steps to create a service principal for an Azure Active Directory application, assign a role to the service principal, and authenticate as the service principal by providing a certificate.</span></span> <span data-ttu-id="7039f-176">A következő parancsokat az alkalmazás létrehozása az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="7039f-176">Run the following commands to create an application in Azure Active Directory.</span></span>

1. <span data-ttu-id="7039f-177">A PowerShell-konzolablakot illessze be a következő parancsmagokat.</span><span class="sxs-lookup"><span data-stu-id="7039f-177">Paste the following cmdlets in the PowerShell console window.</span></span> <span data-ttu-id="7039f-178">Győződjön meg arról, hogy a megadott érték a **- DisplayName** tulajdonság értéke egyedi.</span><span class="sxs-lookup"><span data-stu-id="7039f-178">Make sure the value you specify for the **-DisplayName** property is unique.</span></span> <span data-ttu-id="7039f-179">Az is, az értékek **- kezdőlap** és **- IdentiferUris** helyőrző értékeket, és nem ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="7039f-179">Also, the values for **-HomePage** and **-IdentiferUris** are placeholder values and are not verified.</span></span>

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
2. <span data-ttu-id="7039f-180">Hozzon létre egy egyszerű szolgáltatásnév az alkalmazás azonosítójával.</span><span class="sxs-lookup"><span data-stu-id="7039f-180">Create a service principal using the application ID.</span></span>

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. <span data-ttu-id="7039f-181">A Data Lake Store-mappa és a fájlt, amely akkor érik el a HDInsight-fürt a szolgáltatás egyszerű hozzáférést engedélyez.</span><span class="sxs-lookup"><span data-stu-id="7039f-181">Grant the service principal access to the Data Lake Store folder and the file that you will access from the HDInsight cluster.</span></span> <span data-ttu-id="7039f-182">Az alábbi részlet biztosít hozzáférést a Data Lake Store-fiók (másolásakor a mintaadatfájlokat), a legfelső szintű és magát a fájlt.</span><span class="sxs-lookup"><span data-stu-id="7039f-182">The snippet below provides access to the root of the Data Lake Store account (where you copied the sample data file), and the file itself.</span></span>

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /vehicle1_09142014.csv -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-additional-storage"></a><span data-ttu-id="7039f-183">Egy HDInsight Linux-fürt létrehozása a Data Lake Store további tárhely</span><span class="sxs-lookup"><span data-stu-id="7039f-183">Create an HDInsight Linux cluster with Data Lake Store as additional storage</span></span>

<span data-ttu-id="7039f-184">Ebben a részben azt egy HDInsight Hadoop Linux fürt létrehozása a Data Lake Store további tárolóként.</span><span class="sxs-lookup"><span data-stu-id="7039f-184">In this section, we create an HDInsight Hadoop Linux cluster with Data Lake Store as additional storage.</span></span> <span data-ttu-id="7039f-185">Ebben a kiadásban a HDInsight-fürt és a Data Lake Store ugyanazon a helyen kell lennie.</span><span class="sxs-lookup"><span data-stu-id="7039f-185">For this release, the HDInsight cluster and the Data Lake Store must be in the same location.</span></span>

1. <span data-ttu-id="7039f-186">Indítsa el az előfizetés-azonosító. bérlő beolvasása</span><span class="sxs-lookup"><span data-stu-id="7039f-186">Start with retrieving the subscription tenant ID.</span></span> <span data-ttu-id="7039f-187">Később szüksége lesz, amely.</span><span class="sxs-lookup"><span data-stu-id="7039f-187">You will need that later.</span></span>

        $tenantID = (Get-AzureRmContext).Tenant.TenantId
2. <span data-ttu-id="7039f-188">Ebben a kiadásban a Hadoop fürtök a Data Lake Store csak használható további tárterületként a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="7039f-188">For this release, for a Hadoop cluster, Data Lake Store can only be used as an additional storage for the cluster.</span></span> <span data-ttu-id="7039f-189">Az alapértelmezett tároló is az Azure storage blobs szolgáltatásban (WASB).</span><span class="sxs-lookup"><span data-stu-id="7039f-189">The default storage will still be the Azure storage blobs (WASB).</span></span> <span data-ttu-id="7039f-190">Igen először létrehozunk a tárfiók és a tároló a fürt szükséges.</span><span class="sxs-lookup"><span data-stu-id="7039f-190">So, we'll first create the storage account and storage containers required for the cluster.</span></span>

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAcccountName>"   # Provide a Storage account name

        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = (Get-AzureRmStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $containerName -Context $destContext
3. <span data-ttu-id="7039f-191">A HDInsight-fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7039f-191">Create the HDInsight cluster.</span></span> <span data-ttu-id="7039f-192">A következő parancsmagokat használja.</span><span class="sxs-lookup"><span data-stu-id="7039f-192">Use the following cmdlets.</span></span>

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have the same name for the cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $sshCredentials = Get-Credential

        New-AzureRmHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.4" -OSType Linux -SshCredential $sshCredentials -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    <span data-ttu-id="7039f-193">A parancsmag sikeres befejezése után, a fürt részleteket felsoroló kimenetnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="7039f-193">After the cmdlet successfully completes, you should see an output listing the cluster details.</span></span>


## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-the-data-lake-store"></a><span data-ttu-id="7039f-194">A HDInsight-fürt használata a Data Lake Store tesztet-feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="7039f-194">Run test jobs on the HDInsight cluster to use the Data Lake Store</span></span>
<span data-ttu-id="7039f-195">Miután konfigurálta a HDInsight-fürtöt, a teszt feladatok ellenőrzéséhez, hogy a HDInsight-fürt hozzáférhet-e a Data Lake Store a fürtön is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="7039f-195">After you have configured an HDInsight cluster, you can run test jobs on the cluster to test that the HDInsight cluster can access Data Lake Store.</span></span> <span data-ttu-id="7039f-196">Ehhez az szükséges, azt egy minta Hive táblát hoz létre a Data Lake Store korábban feltöltött megadott mintaadatokat használja feladat elindul.</span><span class="sxs-lookup"><span data-stu-id="7039f-196">To do so, we will run a sample Hive job that creates a table using the sample data that you uploaded earlier to your Data Lake Store.</span></span>

<span data-ttu-id="7039f-197">Ebben a szakaszban fogja SSH a HDInsight Linux fürthöz létrehozott, és futtassa a minta Hive-lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="7039f-197">In this section you will SSH into the HDInsight Linux cluster you created and run the a sample Hive query.</span></span>

* <span data-ttu-id="7039f-198">Ha a fürthöz SSH Windows ügyfél használ, tekintse meg [SSH használata a HDInsight Windows Linux-alapú Hadooppal](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="7039f-198">If you are using a Windows client to SSH into the cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>
* <span data-ttu-id="7039f-199">Ha a fürthöz az SSH Linux-ügyfél használ, tekintse meg [SSH használata a HDInsight Linux Linux-alapú Hadooppal](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="7039f-199">If you are using a Linux client to SSH into the cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>

1. <span data-ttu-id="7039f-200">A csatlakozás után a következő parancs használatával indítsa el a Hive CLI:</span><span class="sxs-lookup"><span data-stu-id="7039f-200">Once connected, start the Hive CLI by using the following command:</span></span>

        hive
2. <span data-ttu-id="7039f-201">A parancssori felület használatával adja meg az alábbi állításokat annak nevű új tábla létrehozása **járművekről gyűjtött** által megadott mintaadatokat használja a Data Lake Store-ban:</span><span class="sxs-lookup"><span data-stu-id="7039f-201">Using the CLI, enter the following statements to create a new table named **vehicles** by using the sample data in the Data Lake Store:</span></span>

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
        SELECT * FROM vehicles LIMIT 10;

    <span data-ttu-id="7039f-202">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="7039f-202">You should see an output similar to the following:</span></span>

        1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
        1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
        1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
        1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
        1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
        1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
        1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
        1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
        1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
        1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1

## <a name="access-data-lake-store-using-hdfs-commands"></a><span data-ttu-id="7039f-203">Hozzáférés Data Lake Store HDFS parancs használatával</span><span class="sxs-lookup"><span data-stu-id="7039f-203">Access Data Lake Store using HDFS commands</span></span>
<span data-ttu-id="7039f-204">Miután konfigurálta a Data Lake Store használata a HDInsight-fürthöz, a HDFS felületparancsokat használhatja az áruház eléréséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="7039f-204">Once you have configured the HDInsight cluster to use Data Lake Store, you can use the HDFS shell commands to access the store.</span></span>

<span data-ttu-id="7039f-205">Az itt SSH a HDInsight Linux fürthöz létrehozott és a HDFS parancs futtatása lesz.</span><span class="sxs-lookup"><span data-stu-id="7039f-205">In this section you will SSH into the HDInsight Linux cluster you created and run the HDFS commands.</span></span>

* <span data-ttu-id="7039f-206">Ha a fürthöz SSH Windows ügyfél használ, tekintse meg [SSH használata a HDInsight Windows Linux-alapú Hadooppal](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="7039f-206">If you are using a Windows client to SSH into the cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>
* <span data-ttu-id="7039f-207">Ha a fürthöz az SSH Linux-ügyfél használ, tekintse meg [SSH használata a HDInsight Linux Linux-alapú Hadooppal](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="7039f-207">If you are using a Linux client to SSH into the cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>

<span data-ttu-id="7039f-208">A csatlakozás után a következő HDFS filesystem parancs segítségével a Data Lake Store található fájlok listázása.</span><span class="sxs-lookup"><span data-stu-id="7039f-208">Once connected, use the following HDFS filesystem command to list the files in the Data Lake Store.</span></span>

    hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

<span data-ttu-id="7039f-209">Megjelenik a korábban a Data Lake Store-bA feltöltött fájl.</span><span class="sxs-lookup"><span data-stu-id="7039f-209">This should list the file that you uploaded earlier to the Data Lake Store.</span></span>

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

<span data-ttu-id="7039f-210">Használhatja a `hdfs dfs -put` parancs egyes fájlok feltöltése a Data Lake Store, és ezután `hdfs dfs -ls` ellenőrzése, hogy a fájlok sikeresen feltöltve.</span><span class="sxs-lookup"><span data-stu-id="7039f-210">You can also use the `hdfs dfs -put` command to upload some files to the Data Lake Store, and then use `hdfs dfs -ls` to verify whether the files were successfully uploaded.</span></span>

## <a name="see-also"></a><span data-ttu-id="7039f-211">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="7039f-211">See Also</span></span>
* [<span data-ttu-id="7039f-212">Portál: Data Lake Store használatára HDInsight-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="7039f-212">Portal: Create an HDInsight cluster to use Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
