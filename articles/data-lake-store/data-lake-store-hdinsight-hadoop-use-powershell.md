---
<span data-ttu-id="a3180-101">cím: aaa "PowerShell: Azure HDInsight fürt a Data Lake Store bővítmény tárolóként |} Microsoft Docs"szolgáltatások:-lake-adattár, hdinsight documentationcenter:" Szerző: nitinme manager: jhubbard szerkesztő: cgronlun</span><span class="sxs-lookup"><span data-stu-id="a3180-101">title: aaa"PowerShell: Azure HDInsight cluster with Data Lake Store as add-on storage | Microsoft Docs" services: data-lake-store,hdinsight documentationcenter: '' author: nitinme manager: jhubbard editor: cgronlun</span></span>

<span data-ttu-id="a3180-102">MS.AssetId: 164ada5a-222e-4be2-bd32-e51dbe993bc0 ms.service: lake-adattár ms.devlang: na ms.topic: ms.tgt_pltfrm a következő cikket: na ms.workload: big data ms.date: 06/08/2017 ms.author: nitinme</span><span class="sxs-lookup"><span data-stu-id="a3180-102">ms.assetid: 164ada5a-222e-4be2-bd32-e51dbe993bc0 ms.service: data-lake-store ms.devlang: na ms.topic: article ms.tgt_pltfrm: na ms.workload: big-data ms.date: 06/08/2017 ms.author: nitinme</span></span>

---
# <a name="use-azure-powershell-toocreate-an-hdinsight-cluster-with-data-lake-store-as-additional-storage"></a><span data-ttu-id="a3180-103">Azure PowerShell toocreate HDInsight-fürtöt használja a Data Lake Store (a további tárhely)</span><span class="sxs-lookup"><span data-stu-id="a3180-103">Use Azure PowerShell toocreate an HDInsight cluster with Data Lake Store (as additional storage)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a3180-104">A Portal használata</span><span class="sxs-lookup"><span data-stu-id="a3180-104">Using Portal</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [<span data-ttu-id="a3180-105">PowerShell használatával (az alapértelmezett tároló)</span><span class="sxs-lookup"><span data-stu-id="a3180-105">Using PowerShell (for default storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [<span data-ttu-id="a3180-106">(Tárhely) a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="a3180-106">Using PowerShell (for additional storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [<span data-ttu-id="a3180-107">Erőforrás-kezelő használatával</span><span class="sxs-lookup"><span data-stu-id="a3180-107">Using Resource Manager</span></span>](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

<span data-ttu-id="a3180-108">Ismerje meg, hogyan toouse Azure PowerShell tooconfigure egy HDInsight-fürtöt az Azure Data Lake Store **további tárolóként**.</span><span class="sxs-lookup"><span data-stu-id="a3180-108">Learn how toouse Azure PowerShell tooconfigure an HDInsight cluster with Azure Data Lake Store, **as additional storage**.</span></span> <span data-ttu-id="a3180-109">Hogyan toocreate egy HDInsight-fürtöt az Azure Data Lake Store alapértelmezett tárolóként, lásd: [HDInsight-fürtök létrehozása a Data Lake Store alapértelmezett tárolóként](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md).</span><span class="sxs-lookup"><span data-stu-id="a3180-109">For instructions on how toocreate an HDInsight cluster with Azure Data Lake Store as default storage, see [Create an HDInsight cluster with Data Lake Store as default storage](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md).</span></span>

> [!NOTE]
> <span data-ttu-id="a3180-110">Ha azt tervezi, hogy toouse Azure Data Lake Store további tárolóként HDInsight-fürthöz, javasoljuk, hogy ehhez hello fürt létrehozásakor, a cikkben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="a3180-110">If you are going toouse Azure Data Lake Store as additional storage for HDInsight cluster, we strongly recommend that you do this while you create hello cluster as described in this article.</span></span> <span data-ttu-id="a3180-111">Azure Data Lake Store további tárhely tooan hozzáadása meglévő HDInsight-fürt egy összetett folyamat és hibalehetőségeket rejt magában tooerrors.</span><span class="sxs-lookup"><span data-stu-id="a3180-111">Adding Azure Data Lake Store as additional storage tooan existing HDInsight cluster is a complicated process and prone tooerrors.</span></span>
>

<span data-ttu-id="a3180-112">Támogatott fürttípusok Data Lake Store egy alapértelmezett tároló vagy a további tárhely fiókként használható.</span><span class="sxs-lookup"><span data-stu-id="a3180-112">For supported cluster types, Data Lake Store can be used as a default storage or additional storage account.</span></span> <span data-ttu-id="a3180-113">Ha a Data Lake Store további tárterületet, hello alapértelmezett tárfiók hello fürtök továbbra is Azure Storage Blobs (WASB), és hello fürt kapcsolatos fájlokhoz (például naplói, stb.) továbbra is írt toohello alapértelmezett tárolási hello adatainak közben, amikor szeretné, hogy tooprocess tárolható Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="a3180-113">When Data Lake Store is used as additional storage, hello default storage account for hello clusters will still be Azure Storage Blobs (WASB) and hello cluster-related files (such as logs, etc.) are still written toohello default storage, while hello data that you want tooprocess can be stored in a Data Lake Store account.</span></span> <span data-ttu-id="a3180-114">További tárhely fiókként használatával a Data Lake Store nem befolyásolja a teljesítményt és a hello képességét tooread/írás toohello tárolást hello fürtből.</span><span class="sxs-lookup"><span data-stu-id="a3180-114">Using Data Lake Store as an additional storage account does not impact performance or hello ability tooread/write toohello storage from hello cluster.</span></span>

## <a name="using-data-lake-store-for-hdinsight-cluster-storage"></a><span data-ttu-id="a3180-115">Data Lake Store használata a HDInsight-fürt tárolására</span><span class="sxs-lookup"><span data-stu-id="a3180-115">Using Data Lake Store for HDInsight cluster storage</span></span>

<span data-ttu-id="a3180-116">Az alábbiakban a HDInsight a Data Lake Store használatára vonatkozó szempontokat:</span><span class="sxs-lookup"><span data-stu-id="a3180-116">Here are some important considerations for using HDInsight with Data Lake Store:</span></span>

* <span data-ttu-id="a3180-117">Beállítás toocreate a HDInsight-fürtök hozzáféréssel rendelkező tooData Lake Store további tárolóként verzióihoz áll rendelkezésre HDInsight 3.2-es, 3.4, 3.5-ös és 3.6.</span><span class="sxs-lookup"><span data-stu-id="a3180-117">Option toocreate HDInsight clusters with access tooData Lake Store as additional storage is available for HDInsight versions 3.2, 3.4, 3.5, and 3.6.</span></span>

<span data-ttu-id="a3180-118">HDInsight konfigurálása a PowerShell használatával a Data Lake Store toowork magában foglalja a hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a3180-118">Configuring HDInsight toowork with Data Lake Store using PowerShell involves hello following steps:</span></span>

* <span data-ttu-id="a3180-119">Hozzon létre egy Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="a3180-119">Create an Azure Data Lake Store</span></span>
* <span data-ttu-id="a3180-120">A hitelesítés beállítása, szerepköralapú hozzáférési tooData Lake</span><span class="sxs-lookup"><span data-stu-id="a3180-120">Set up authentication for role-based access tooData Lake Store</span></span>
* <span data-ttu-id="a3180-121">A hitelesítési tooData Lake Store HDInsight-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3180-121">Create HDInsight cluster with authentication tooData Lake Store</span></span>
* <span data-ttu-id="a3180-122">Egy tesztelési feladat futtatása hello fürtön</span><span class="sxs-lookup"><span data-stu-id="a3180-122">Run a test job on hello cluster</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a3180-123">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a3180-123">Prerequisites</span></span>
<span data-ttu-id="a3180-124">Ez az oktatóanyag elkezdéséhez hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="a3180-124">Before you begin this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="a3180-125">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="a3180-125">**An Azure subscription**.</span></span> <span data-ttu-id="a3180-126">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a3180-126">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="a3180-127">Az **Azure PowerShell 1.0-s vagy újabb verziója**.</span><span class="sxs-lookup"><span data-stu-id="a3180-127">**Azure PowerShell 1.0 or greater**.</span></span> <span data-ttu-id="a3180-128">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a3180-128">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="a3180-129">**Windows SDK**.</span><span class="sxs-lookup"><span data-stu-id="a3180-129">**Windows SDK**.</span></span> <span data-ttu-id="a3180-130">A későbbiekben telepítheti az [Itt](https://dev.windows.com/en-us/downloads).</span><span class="sxs-lookup"><span data-stu-id="a3180-130">You can install it from [here](https://dev.windows.com/en-us/downloads).</span></span> <span data-ttu-id="a3180-131">A toocreate olyan biztonsági tanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="a3180-131">You use this toocreate a security certificate.</span></span>
* <span data-ttu-id="a3180-132">**Az Azure Active Directory szolgáltatás egyszerű**.</span><span class="sxs-lookup"><span data-stu-id="a3180-132">**Azure Active Directory Service Principal**.</span></span> <span data-ttu-id="a3180-133">Ez az oktatóanyag lépéseit ad útmutatást toocreate egy egyszerű Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="a3180-133">Steps in this tutorial provide instructions on how toocreate a service principal in Azure AD.</span></span> <span data-ttu-id="a3180-134">Azonban az Azure AD rendszergazdai toobe képes toocreate szolgáltatásnevet kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a3180-134">However, you must be an Azure AD administrator toobe able toocreate a service principal.</span></span> <span data-ttu-id="a3180-135">Ha az Azure AD-rendszergazdaként, hagyja ki ezt az előfeltételt, és hello oktatóanyag folytatásához.</span><span class="sxs-lookup"><span data-stu-id="a3180-135">If you are an Azure AD administrator, you can skip this prerequisite and proceed with hello tutorial.</span></span>

    <span data-ttu-id="a3180-136">**Ha nem az Azure AD-rendszergazda**, nem fogja tudni tooperform hello lépéseket szükséges toocreate egy egyszerű szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="a3180-136">**If you are not an Azure AD administrator**, you will not be able tooperform hello steps required toocreate a service principal.</span></span> <span data-ttu-id="a3180-137">Ebben az esetben az Azure AD-rendszergazda először létre kell hoznia egy egyszerű szolgáltatást a Data Lake Store egy HDInsight-fürt létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="a3180-137">In such a case, your Azure AD administrator must first create a service principal before you can create an HDInsight cluster with Data Lake Store.</span></span> <span data-ttu-id="a3180-138">Emellett hello szolgáltatás egyszerű segítségével kell létrehozni egy tanúsítványt, részben ismertetett módon [hozzon létre egy egyszerű tanúsítvány](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span><span class="sxs-lookup"><span data-stu-id="a3180-138">Also, hello service principal must be created using a certificate, as described at [Create a service principal with certificate](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span></span>

## <a name="create-an-azure-data-lake-store"></a><span data-ttu-id="a3180-139">Hozzon létre egy Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="a3180-139">Create an Azure Data Lake Store</span></span>
<span data-ttu-id="a3180-140">Hajtsa végre az alábbi lépéseket toocreate egy Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a3180-140">Follow these steps toocreate a Data Lake Store.</span></span>

1. <span data-ttu-id="a3180-141">Az asztalon nyisson meg egy új Azure PowerShell-ablakot, és adja meg a következő kódrészletet hello.</span><span class="sxs-lookup"><span data-stu-id="a3180-141">From your desktop, open a new Azure PowerShell window, and enter hello following snippet.</span></span> <span data-ttu-id="a3180-142">Amikor felszólító toolog, győződjön meg arról, hogy jelentkezik be a rendelkezésre álló hello előfizetés rendszergazdája vagy tulajdonosa:</span><span class="sxs-lookup"><span data-stu-id="a3180-142">When prompted toolog in, make sure you log in as one of hello subscription administrator/owner:</span></span>

        # Log in tooyour Azure account
        Login-AzureRmAccount

        # List all hello subscriptions associated tooyour account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

   > [!NOTE]
   > <span data-ttu-id="a3180-143">Ha a hibaüzenet hasonló túl`Register-AzureRmResourceProvider : InvalidResourceNamespace: hello resource namespace 'Microsoft.DataLakeStore' is invalid` hello Data Lake Store erőforrás-szolgáltató regisztrálása, esetén lehetséges, hogy az előfizetés nem szerepel az Azure Data Lake Store az engedélyezési listán.</span><span class="sxs-lookup"><span data-stu-id="a3180-143">If you receive an error similar too`Register-AzureRmResourceProvider : InvalidResourceNamespace: hello resource namespace 'Microsoft.DataLakeStore' is invalid` when registering hello Data Lake Store resource provider, it is possible that your subscription is not whitelisted for Azure Data Lake Store.</span></span> <span data-ttu-id="a3180-144">Győződjön meg arról, hogy az Azure-előfizetéshez a Data Lake Store nyilvános előzetes verziójához engedélyezze a következő [utasításokat](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a3180-144">Make sure you enable your Azure subscription for Data Lake Store public preview by following these [instructions](data-lake-store-get-started-portal.md).</span></span>
   >
   >
2. <span data-ttu-id="a3180-145">Az Azure Data Lake Store-fiókok egy Azure-erőforráscsoporthoz vannak társítva.</span><span class="sxs-lookup"><span data-stu-id="a3180-145">An Azure Data Lake Store account is associated with an Azure Resource Group.</span></span> <span data-ttu-id="a3180-146">Először hozzon létre egy Azure-erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="a3180-146">Start by creating an Azure Resource Group.</span></span>

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    <span data-ttu-id="a3180-147">Ez hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="a3180-147">You should see an output like this:</span></span>

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. <span data-ttu-id="a3180-148">Hozzon létre egy Azure Data Lake Store-fiókot.</span><span class="sxs-lookup"><span data-stu-id="a3180-148">Create an Azure Data Lake Store account.</span></span> <span data-ttu-id="a3180-149">hello fiókhoz megadott neve csak kisbetűket és számokat tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="a3180-149">hello account name you specify must only contain lowercase letters and numbers.</span></span>

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    <span data-ttu-id="a3180-150">Hello hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="a3180-150">You should see an output like hello following:</span></span>

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

5. <span data-ttu-id="a3180-151">Néhány példa adatok tooAzure Data Lake feltöltése.</span><span class="sxs-lookup"><span data-stu-id="a3180-151">Upload some sample data tooAzure Data Lake.</span></span> <span data-ttu-id="a3180-152">Ez a cikk tooverify, hogy hello adatok érhető el a HDInsight-fürtök a későbbi fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="a3180-152">We'll use this later in this article tooverify that hello data is accessible from an HDInsight cluster.</span></span> <span data-ttu-id="a3180-153">Néhány példa adatok tooupload keres, ha kaphat a hello **Ambulance Data** hello mappát [Azure Data Lake Git-tárház](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span><span class="sxs-lookup"><span data-stu-id="a3180-153">If you are looking for some sample data tooupload, you can get hello **Ambulance Data** folder from hello [Azure Data Lake Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span></span>

        $myrootdir = "/"
        Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\<path toodata>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## <a name="set-up-authentication-for-role-based-access-toodata-lake-store"></a><span data-ttu-id="a3180-154">A hitelesítés beállítása, szerepköralapú hozzáférési tooData Lake</span><span class="sxs-lookup"><span data-stu-id="a3180-154">Set up authentication for role-based access tooData Lake Store</span></span>
<span data-ttu-id="a3180-155">Egy Azure Active Directory minden Azure-előfizetés tartozik.</span><span class="sxs-lookup"><span data-stu-id="a3180-155">Every Azure subscription is associated with an Azure Active Directory.</span></span> <span data-ttu-id="a3180-156">Felhasználók és a szolgáltatások hello előfizetés hello klasszikus Azure portálon vagy az Azure Resource Manager API-t használó erőforrásokat elérő először hitelesítenie kell magát, hogy Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="a3180-156">Users and services that access resources of hello subscription using hello Azure Classic Portal or Azure Resource Manager API must first authenticate with that Azure Active Directory.</span></span> <span data-ttu-id="a3180-157">Hozzáférés tooAzure előfizetések és a szolgáltatások hello megfelelő szerepkört egy Azure-erőforrás hozzárendelésével.</span><span class="sxs-lookup"><span data-stu-id="a3180-157">Access is granted tooAzure subscriptions and services by assigning them hello appropriate role on an Azure resource.</span></span>  <span data-ttu-id="a3180-158">Szolgáltatások esetén egy egyszerű szolgáltatásnév hello Azure Active Directory (AAD) hello szolgáltatást azonosítja.</span><span class="sxs-lookup"><span data-stu-id="a3180-158">For services, a service principal identifies hello service in hello Azure Active Directory (AAD).</span></span> <span data-ttu-id="a3180-159">Ez a szakasz bemutatja, hogyan toogrant alkalmazás szolgáltatást, mint például a HDInsight hozzáférést tooan Azure-erőforrás (hello korábban létrehozott Azure Data Lake Store-fiók) hoz létre egy egyszerű hello alkalmazáshoz és szerepkörök toothat Azure keresztül PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a3180-159">This section illustrates how toogrant an application service, like HDInsight, access tooan Azure resource (hello Azure Data Lake Store account you created earlier) by creating a service principal for hello application and assigning roles toothat via Azure PowerShell.</span></span>

<span data-ttu-id="a3180-160">az Active Directory-hitelesítés az Azure Data Lake tooset, végre kell hajtania a következő feladatok hello.</span><span class="sxs-lookup"><span data-stu-id="a3180-160">tooset up Active Directory authentication for Azure Data Lake, you must perform hello following tasks.</span></span>

* <span data-ttu-id="a3180-161">Önaláírt tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3180-161">Create a self-signed certificate</span></span>
* <span data-ttu-id="a3180-162">Létrehoz egy alkalmazást az Azure Active Directory és az egyszerű szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="a3180-162">Create an application in Azure Active Directory and a Service Principal</span></span>

### <a name="create-a-self-signed-certificate"></a><span data-ttu-id="a3180-163">Önaláírt tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3180-163">Create a self-signed certificate</span></span>
<span data-ttu-id="a3180-164">Győződjön meg arról, hogy [Windows SDK](https://dev.windows.com/en-us/downloads) ebben a szakaszban lévő lépéseket hello folytatása előtt.</span><span class="sxs-lookup"><span data-stu-id="a3180-164">Make sure you have [Windows SDK](https://dev.windows.com/en-us/downloads) installed before proceeding with hello steps in this section.</span></span> <span data-ttu-id="a3180-165">Kell is létrehozott egy könyvtárat, például a **C:\mycertdir**, ahol hello tanúsítvány jön létre.</span><span class="sxs-lookup"><span data-stu-id="a3180-165">You must have also created a directory, such as **C:\mycertdir**, where hello certificate will be created.</span></span>

1. <span data-ttu-id="a3180-166">Hello PowerShell ablakban, keresse meg a toohello helyét Windows SDK-t (általában `C:\Program Files (x86)\Windows Kits\10\bin\x86` és hello [MakeCert] [ makecert] segédprogram toocreate egy önaláírt tanúsítványt és a titkos kulcs.</span><span class="sxs-lookup"><span data-stu-id="a3180-166">From hello PowerShell window, navigate toohello location where you installed Windows SDK (typically, `C:\Program Files (x86)\Windows Kits\10\bin\x86` and use hello [MakeCert][makecert] utility toocreate a self-signed certificate and a private key.</span></span> <span data-ttu-id="a3180-167">A következő parancsok hello használata.</span><span class="sxs-lookup"><span data-stu-id="a3180-167">Use hello following commands.</span></span>

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    <span data-ttu-id="a3180-168">Rákérdezéses tooenter hello titkos kulcs jelszava lesz.</span><span class="sxs-lookup"><span data-stu-id="a3180-168">You will be prompted tooenter hello private key password.</span></span> <span data-ttu-id="a3180-169">Miután hello parancs sikeres végrehajtása során, megjelenik egy **CertFile.cer** és **mykey.pvk** megadott hello tanúsítvány könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="a3180-169">After hello command successfully executes, you should see a **CertFile.cer** and **mykey.pvk** in hello certificate directory you specified.</span></span>
2. <span data-ttu-id="a3180-170">Használjon hello [Pvk2Pfx] [ pvk2pfx] segédprogram tooconvert hello .pvk és .cer-fájlokat a MakeCert létrehozott tooa .pfx fájlt.</span><span class="sxs-lookup"><span data-stu-id="a3180-170">Use hello [Pvk2Pfx][pvk2pfx] utility tooconvert hello .pvk and .cer files that MakeCert created tooa .pfx file.</span></span> <span data-ttu-id="a3180-171">Futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="a3180-171">Run hello following command.</span></span>

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    <span data-ttu-id="a3180-172">Amikor a rendszer kéri hello titkos kulcs jelszava korábban meghatározott adja meg.</span><span class="sxs-lookup"><span data-stu-id="a3180-172">When prompted enter hello private key password you specified earlier.</span></span> <span data-ttu-id="a3180-173">hello számára megadott érték hello **-po** paraméter hello jelszavát hello .pfx-fájllal.</span><span class="sxs-lookup"><span data-stu-id="a3180-173">hello value you specify for hello **-po** parameter is hello password that is associated with hello .pfx file.</span></span> <span data-ttu-id="a3180-174">Ha hello parancs sikeresen befejeződött, emellett meg kell jelennie egy CertFile.pfx megadott hello tanúsítvány könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="a3180-174">After hello command successfully completes, you should also see a CertFile.pfx in hello certificate directory you specified.</span></span>

### <a name="create-an-azure-active-directory-and-a-service-principal"></a><span data-ttu-id="a3180-175">Egy Azure Active Directory és az egyszerű szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3180-175">Create an Azure Active Directory and a service principal</span></span>
<span data-ttu-id="a3180-176">Ebben a szakaszban az Azure Active Directory-alkalmazás végrehajtja a hello lépéseket toocreate egy egyszerű szolgáltatást, hozzárendelése egy szerepkörhöz toohello szolgáltatás egyszerű, és hitelesítse magát hello szolgáltatás egyszerű, adja meg a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="a3180-176">In this section, you perform hello steps toocreate a service principal for an Azure Active Directory application, assign a role toohello service principal, and authenticate as hello service principal by providing a certificate.</span></span> <span data-ttu-id="a3180-177">Futtassa a következő parancsok toocreate hello egy alkalmazást az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="a3180-177">Run hello following commands toocreate an application in Azure Active Directory.</span></span>

1. <span data-ttu-id="a3180-178">Illessze be a következő parancsmagok a PowerShell-konzolablakot hello hello.</span><span class="sxs-lookup"><span data-stu-id="a3180-178">Paste hello following cmdlets in hello PowerShell console window.</span></span> <span data-ttu-id="a3180-179">Győződjön meg arról, hogy hello tulajdonság hello **- DisplayName** tulajdonság értéke egyedi.</span><span class="sxs-lookup"><span data-stu-id="a3180-179">Make sure hello value you specify for hello **-DisplayName** property is unique.</span></span> <span data-ttu-id="a3180-180">Emellett hello értékeinek **- kezdőlap** és **- IdentiferUris** helyőrző értékeket, és nem ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="a3180-180">Also, hello values for **-HomePage** and **-IdentiferUris** are placeholder values and are not verified.</span></span>

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
2. <span data-ttu-id="a3180-181">Hozzon létre egy egyszerű szolgáltatást hello alkalmazás azonosítójával.</span><span class="sxs-lookup"><span data-stu-id="a3180-181">Create a service principal using hello application ID.</span></span>

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. <span data-ttu-id="a3180-182">Adja meg a hello szolgáltatás egyszerű hozzáférés toohello Data Lake Store a mappák és hello használni, akkor a hello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="a3180-182">Grant hello service principal access toohello Data Lake Store folder and hello file that you will access from hello HDInsight cluster.</span></span> <span data-ttu-id="a3180-183">az alábbi hello részlet hozzáférés toohello gyökere hello Data Lake Store (másolásakor hello mintaadatfájlokat) fiók, és maga hello biztosít.</span><span class="sxs-lookup"><span data-stu-id="a3180-183">hello snippet below provides access toohello root of hello Data Lake Store account (where you copied hello sample data file), and hello file itself.</span></span>

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /vehicle1_09142014.csv -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-additional-storage"></a><span data-ttu-id="a3180-184">Egy HDInsight Linux-fürt létrehozása a Data Lake Store további tárhely</span><span class="sxs-lookup"><span data-stu-id="a3180-184">Create an HDInsight Linux cluster with Data Lake Store as additional storage</span></span>

<span data-ttu-id="a3180-185">Ebben a részben azt egy HDInsight Hadoop Linux fürt létrehozása a Data Lake Store további tárolóként.</span><span class="sxs-lookup"><span data-stu-id="a3180-185">In this section, we create an HDInsight Hadoop Linux cluster with Data Lake Store as additional storage.</span></span> <span data-ttu-id="a3180-186">Ebben a kiadásban hello HDInsight-fürtöt és hello Data Lake Store kell, hogy a hello ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="a3180-186">For this release, hello HDInsight cluster and hello Data Lake Store must be in hello same location.</span></span>

1. <span data-ttu-id="a3180-187">Indítsa el beolvasása hello előfizetés bérlő-azonosító.</span><span class="sxs-lookup"><span data-stu-id="a3180-187">Start with retrieving hello subscription tenant ID.</span></span> <span data-ttu-id="a3180-188">Később szüksége lesz, amely.</span><span class="sxs-lookup"><span data-stu-id="a3180-188">You will need that later.</span></span>

        $tenantID = (Get-AzureRmContext).Tenant.TenantId
2. <span data-ttu-id="a3180-189">Ebben a kiadásban a Hadoop fürtök a Data Lake Store csak használható további tárterületként hello fürthöz.</span><span class="sxs-lookup"><span data-stu-id="a3180-189">For this release, for a Hadoop cluster, Data Lake Store can only be used as an additional storage for hello cluster.</span></span> <span data-ttu-id="a3180-190">hello alapértelmezett tárolási is hello az Azure storage blobs (WASB).</span><span class="sxs-lookup"><span data-stu-id="a3180-190">hello default storage will still be hello Azure storage blobs (WASB).</span></span> <span data-ttu-id="a3180-191">Igen először létrehozunk hello fiók- és tárolási tárolókban hello fürt szükséges.</span><span class="sxs-lookup"><span data-stu-id="a3180-191">So, we'll first create hello storage account and storage containers required for hello cluster.</span></span>

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAcccountName>"   # Provide a Storage account name

        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = (Get-AzureRmStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $containerName -Context $destContext
3. <span data-ttu-id="a3180-192">Hello HDInsight-fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a3180-192">Create hello HDInsight cluster.</span></span> <span data-ttu-id="a3180-193">A következő parancsmagok hello használata.</span><span class="sxs-lookup"><span data-stu-id="a3180-193">Use hello following cmdlets.</span></span>

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have hello same name for hello cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # hello number of nodes in hello HDInsight cluster
        $httpCredentials = Get-Credential
        $sshCredentials = Get-Credential

        New-AzureRmHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.4" -OSType Linux -SshCredential $sshCredentials -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    <span data-ttu-id="a3180-194">Ha hello parancsmag sikeresen befejeződött, akkor listaelem hello fürt részletei kimenetnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="a3180-194">After hello cmdlet successfully completes, you should see an output listing hello cluster details.</span></span>


## <a name="run-test-jobs-on-hello-hdinsight-cluster-toouse-hello-data-lake-store"></a><span data-ttu-id="a3180-195">Hello HDInsight fürt toouse hello Data Lake Store a teszt feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="a3180-195">Run test jobs on hello HDInsight cluster toouse hello Data Lake Store</span></span>
<span data-ttu-id="a3180-196">Miután konfigurálta a HDInsight-fürtöt, futtathatja Tesztfeladatok hello fürt tootest adott hello HDInsight fürt Data Lake Store férhetnek hozzá.</span><span class="sxs-lookup"><span data-stu-id="a3180-196">After you have configured an HDInsight cluster, you can run test jobs on hello cluster tootest that hello HDInsight cluster can access Data Lake Store.</span></span> <span data-ttu-id="a3180-197">toodo úgy, hogy fog futni egy minta Hive-feladatot, amely táblát hoz létre, hogy a korábbi tooyour Data Lake Store feltöltött hello mintaadatok használatával.</span><span class="sxs-lookup"><span data-stu-id="a3180-197">toodo so, we will run a sample Hive job that creates a table using hello sample data that you uploaded earlier tooyour Data Lake Store.</span></span>

<span data-ttu-id="a3180-198">Ez a szakasz a SSH hello HDInsight Linux cluster során létrehozott és hello minta Hive-lekérdezések futtatása a tartalma.</span><span class="sxs-lookup"><span data-stu-id="a3180-198">In this section you will SSH into hello HDInsight Linux cluster you created and run hello a sample Hive query.</span></span>

* <span data-ttu-id="a3180-199">Ha egy Windows ügyfél tooSSH hello fürtbe használ, tekintse meg [SSH használata a HDInsight Windows Linux-alapú Hadooppal](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="a3180-199">If you are using a Windows client tooSSH into hello cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>
* <span data-ttu-id="a3180-200">Ha a Linux-ügyfél tooSSH hello fürtbe használ, tekintse meg [SSH használata a HDInsight Linux Linux-alapú Hadooppal](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="a3180-200">If you are using a Linux client tooSSH into hello cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>

1. <span data-ttu-id="a3180-201">A csatlakozás után indítsa el a hello Hive CLI hello a következő parancs segítségével:</span><span class="sxs-lookup"><span data-stu-id="a3180-201">Once connected, start hello Hive CLI by using hello following command:</span></span>

        hive
2. <span data-ttu-id="a3180-202">Hello CLI, adja meg a következő utasítások toocreate nevű új tábla hello **járművekről gyűjtött** hello mintaadatok használatával a Data Lake Store hello:</span><span class="sxs-lookup"><span data-stu-id="a3180-202">Using hello CLI, enter hello following statements toocreate a new table named **vehicles** by using hello sample data in hello Data Lake Store:</span></span>

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
        SELECT * FROM vehicles LIMIT 10;

    <span data-ttu-id="a3180-203">Egy kimeneti hasonló toohello következő kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="a3180-203">You should see an output similar toohello following:</span></span>

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

## <a name="access-data-lake-store-using-hdfs-commands"></a><span data-ttu-id="a3180-204">Hozzáférés Data Lake Store HDFS parancs használatával</span><span class="sxs-lookup"><span data-stu-id="a3180-204">Access Data Lake Store using HDFS commands</span></span>
<span data-ttu-id="a3180-205">Miután konfigurálta a hello HDInsight fürt toouse Data Lake Store, hello HDFS rendszerhéj parancsok tooaccess hello tároló is használhatja.</span><span class="sxs-lookup"><span data-stu-id="a3180-205">Once you have configured hello HDInsight cluster toouse Data Lake Store, you can use hello HDFS shell commands tooaccess hello store.</span></span>

<span data-ttu-id="a3180-206">Az itt SSH hello HDInsight Linux cluster során létrehozott és hello HDFS parancs futtatása a lesz.</span><span class="sxs-lookup"><span data-stu-id="a3180-206">In this section you will SSH into hello HDInsight Linux cluster you created and run hello HDFS commands.</span></span>

* <span data-ttu-id="a3180-207">Ha egy Windows ügyfél tooSSH hello fürtbe használ, tekintse meg [SSH használata a HDInsight Windows Linux-alapú Hadooppal](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="a3180-207">If you are using a Windows client tooSSH into hello cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>
* <span data-ttu-id="a3180-208">Ha a Linux-ügyfél tooSSH hello fürtbe használ, tekintse meg [SSH használata a HDInsight Linux Linux-alapú Hadooppal](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="a3180-208">If you are using a Linux client tooSSH into hello cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>

<span data-ttu-id="a3180-209">Miután csatlakozott, használja a következő HDFS hello filesystem parancs toolist fájlok hello Data Lake Store hello.</span><span class="sxs-lookup"><span data-stu-id="a3180-209">Once connected, use hello following HDFS filesystem command toolist hello files in hello Data Lake Store.</span></span>

    hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

<span data-ttu-id="a3180-210">Hello fájlt, hogy a korábbi toohello Data Lake Store feltöltött megjelenik.</span><span class="sxs-lookup"><span data-stu-id="a3180-210">This should list hello file that you uploaded earlier toohello Data Lake Store.</span></span>

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

<span data-ttu-id="a3180-211">Is használhatja a hello `hdfs dfs -put` tooupload bizonyos fájlok toohello Data Lake Store parancsot, és ezután `hdfs dfs -ls` tooverify e hello fájlok sikeresen feltöltve.</span><span class="sxs-lookup"><span data-stu-id="a3180-211">You can also use hello `hdfs dfs -put` command tooupload some files toohello Data Lake Store, and then use `hdfs dfs -ls` tooverify whether hello files were successfully uploaded.</span></span>

## <a name="see-also"></a><span data-ttu-id="a3180-212">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="a3180-212">See Also</span></span>
* [<span data-ttu-id="a3180-213">-Portálon: Létrehozása egy HDInsight-fürt toouse Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="a3180-213">Portal: Create an HDInsight cluster toouse Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
