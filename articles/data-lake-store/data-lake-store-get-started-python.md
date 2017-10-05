---
title: "Az Azure Data Lake Store használatának első lépései Python SDK használatával | Microsoft Docs"
description: "Ebből a cikkből megtudhatja, hogyan használhatja a Python SDK-t a Data Lake Store-fiókokkal és a fájlrendszerrel végzett munkához."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 75f6de6f-6fd8-48f4-8707-cb27d22d27a6
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 375a603360ac249fc1b08923a94c85652390a3fc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-data-lake-store-using-python"></a><span data-ttu-id="0a371-103">Az Azure Data Lake Store használatának első lépései a Python használatával</span><span class="sxs-lookup"><span data-stu-id="0a371-103">Get started with Azure Data Lake Store using Python</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="0a371-104">Portál</span><span class="sxs-lookup"><span data-stu-id="0a371-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="0a371-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0a371-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="0a371-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="0a371-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="0a371-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="0a371-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="0a371-108">REST API</span><span class="sxs-lookup"><span data-stu-id="0a371-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="0a371-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="0a371-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="0a371-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="0a371-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="0a371-111">Python</span><span class="sxs-lookup"><span data-stu-id="0a371-111">Python</span></span>](data-lake-store-get-started-python.md)
>
>

<span data-ttu-id="0a371-112">A cikkből megtudhatja, hogyan végezhet el olyan alapvető műveleteket a Python SDK for Azure és az Azure Data Lake Store segítségével, mint például mappák létrehozása vagy adatfájlok le- és feltöltése. További információk a Data Lake-ről: [Azure Data Lake Store](data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0a371-112">Learn how to use the Python SDK for Azure and Azure Data Lake Store to perform basic operations such as create folders, upload and download data files, etc. For more information about Data Lake, see [Azure Data Lake Store](data-lake-store-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0a371-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0a371-113">Prerequisites</span></span>

* <span data-ttu-id="0a371-114">**Python**.</span><span class="sxs-lookup"><span data-stu-id="0a371-114">**Python**.</span></span> <span data-ttu-id="0a371-115">A Pythont [innen](https://www.python.org/downloads/) töltheti le.</span><span class="sxs-lookup"><span data-stu-id="0a371-115">You can download Python from [here](https://www.python.org/downloads/).</span></span> <span data-ttu-id="0a371-116">Ez a cikk a Python 3.5.2-es verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="0a371-116">This article uses Python 3.5.2.</span></span>

* <span data-ttu-id="0a371-117">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="0a371-117">**An Azure subscription**.</span></span> <span data-ttu-id="0a371-118">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0a371-118">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="0a371-119">**Egy Azure Active Directory-alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="0a371-119">**Create an Azure Active Directory Application**.</span></span> <span data-ttu-id="0a371-120">A Data Lake Store alkalmazás Azure AD-val történő hitelesítéséhez az Azure AD alkalmazást kell használni.</span><span class="sxs-lookup"><span data-stu-id="0a371-120">You use the Azure AD application to authenticate the Data Lake Store application with Azure AD.</span></span> <span data-ttu-id="0a371-121">Az Azure AD-val többféle módon is lehet hitelesíteni. Ezek a következők: **végfelhasználói hitelesítés** vagy **szolgáltatások közötti hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="0a371-121">There are different approaches to authenticate with Azure AD, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="0a371-122">Útmutatás a hitelesítéshez és további tudnivalók a [Végfelhasználói hitelesítés](data-lake-store-end-user-authenticate-using-active-directory.md) vagy a [Szolgáltatások közötti hitelesítés](data-lake-store-authenticate-using-active-directory.md) című témakörben.</span><span class="sxs-lookup"><span data-stu-id="0a371-122">For instructions and more information on how to authenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>

## <a name="install-the-modules"></a><span data-ttu-id="0a371-123">A modulok telepítése</span><span class="sxs-lookup"><span data-stu-id="0a371-123">Install the modules</span></span>

<span data-ttu-id="0a371-124">A Data Lake Store a Pythonnal való használatához három modult kell telepítenie.</span><span class="sxs-lookup"><span data-stu-id="0a371-124">To work with Data Lake Store using Python, you need to install three modules.</span></span>

* <span data-ttu-id="0a371-125">Az `azure-mgmt-resource` modult.</span><span class="sxs-lookup"><span data-stu-id="0a371-125">The `azure-mgmt-resource` module.</span></span> <span data-ttu-id="0a371-126">Ez további Azure-modulokat tartalmaz az Active Directoryhoz és más eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="0a371-126">This includes Azure modules for Active Directory, etc..</span></span>
* <span data-ttu-id="0a371-127">Az `azure-mgmt-datalake-store` modult.</span><span class="sxs-lookup"><span data-stu-id="0a371-127">The `azure-mgmt-datalake-store` module.</span></span> <span data-ttu-id="0a371-128">Ez az Azure Data Lake Store fiókkezelési műveleteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0a371-128">This includes the Azure Data Lake Store account management operations.</span></span> <span data-ttu-id="0a371-129">További információkat erről a modulról [az Azure Data Lake Store kezelési moduljához készült referenciaanyagban](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html) talál.</span><span class="sxs-lookup"><span data-stu-id="0a371-129">For more information on this module, see [Azure Data Lake Store Management module reference](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html).</span></span>
* <span data-ttu-id="0a371-130">Az `azure-datalake-store` modult.</span><span class="sxs-lookup"><span data-stu-id="0a371-130">The `azure-datalake-store` module.</span></span> <span data-ttu-id="0a371-131">Ez az Azure Data Lake Store fájlrendszer-műveleteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0a371-131">This includes the Azure Data Lake Store filesystem operations.</span></span> <span data-ttu-id="0a371-132">További információkat erről a modulról [az Azure Data Lake Store fájlrendszermoduljához készült referenciaanyagban](http://azure-datalake-store.readthedocs.io/en/latest/) talál.</span><span class="sxs-lookup"><span data-stu-id="0a371-132">For more information on this module, see [Azure Data Lake Store Filesystem module reference](http://azure-datalake-store.readthedocs.io/en/latest/).</span></span>

<span data-ttu-id="0a371-133">A modulok telepítéséhez használja a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="0a371-133">Use the following commands to install the modules.</span></span>

```
pip install azure-mgmt-resource
pip install azure-mgmt-datalake-store
pip install azure-datalake-store
```

## <a name="create-a-new-python-application"></a><span data-ttu-id="0a371-134">Új Python-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0a371-134">Create a new Python application</span></span>

1. <span data-ttu-id="0a371-135">A választott IDE-ben hozzon létre egy új Python-alkalmazást, például **mysample.py** néven.</span><span class="sxs-lookup"><span data-stu-id="0a371-135">In the IDE of your choice create a new Python application, for example, **mysample.py**.</span></span>

2. <span data-ttu-id="0a371-136">Adja hozzá a következő sorokat a szükséges modulok importálásához.</span><span class="sxs-lookup"><span data-stu-id="0a371-136">Add the following lines to import the required modules</span></span>

    ```
    ## Use this only for Azure AD service-to-service authentication
    from azure.common.credentials import ServicePrincipalCredentials

    ## Use this only for Azure AD end-user authentication
    from azure.common.credentials import UserPassCredentials

    ## Use this only for Azure AD multi-factor authentication
    from msrestazure.azure_active_directory import AADTokenCredentials

    ## Required for Azure Data Lake Store account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Azure Data Lake Store filesystem management
    from azure.datalake.store import core, lib, multithread

    # Common Azure imports
    from azure.mgmt.resource.resources import ResourceManagementClient
    from azure.mgmt.resource.resources.models import ResourceGroup

    ## Use these as needed for your application
    import logging, getpass, pprint, uuid, time
    ```

3. <span data-ttu-id="0a371-137">Mentse a mysample.py módosításait.</span><span class="sxs-lookup"><span data-stu-id="0a371-137">Save changes to mysample.py.</span></span>

## <a name="authentication"></a><span data-ttu-id="0a371-138">Hitelesítés</span><span class="sxs-lookup"><span data-stu-id="0a371-138">Authentication</span></span>

<span data-ttu-id="0a371-139">Ebben a szakaszban az Azure AD-hitelesítés különböző módjait tárgyaljuk.</span><span class="sxs-lookup"><span data-stu-id="0a371-139">In this section, we talk about the different ways to authenticate with Azure AD.</span></span> <span data-ttu-id="0a371-140">Az elérhető lehetőségek:</span><span class="sxs-lookup"><span data-stu-id="0a371-140">The options available are:</span></span>

* <span data-ttu-id="0a371-141">Végfelhasználói hitelesítés</span><span class="sxs-lookup"><span data-stu-id="0a371-141">End-user authentication</span></span>
* <span data-ttu-id="0a371-142">Szolgáltatások közötti hitelesítés</span><span class="sxs-lookup"><span data-stu-id="0a371-142">Service-to-service authentication</span></span>
* <span data-ttu-id="0a371-143">Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="0a371-143">Multi-factor authentication</span></span>

<span data-ttu-id="0a371-144">Ezeket a hitelesítési módokat kell használnia a fiókkezelési és a fájlrendszerkezelési modulokban egyaránt.</span><span class="sxs-lookup"><span data-stu-id="0a371-144">You must use these authentication options for both account management and filesystem management modules.</span></span>

### <a name="end-user-authentication-for-account-management"></a><span data-ttu-id="0a371-145">Végfelhasználói hitelesítés fiókkezeléshez</span><span class="sxs-lookup"><span data-stu-id="0a371-145">End-user authentication for account management</span></span>

<span data-ttu-id="0a371-146">Használja ezt az eljárást az Azure AD-val való hitelesítésre a fiókkezelési műveleteknél (Data Lake Store-fiók létrehozása/törlése stb).</span><span class="sxs-lookup"><span data-stu-id="0a371-146">Use this to authenticate with Azure AD for account management operations (create/delete Data Lake Store account, etc.).</span></span> <span data-ttu-id="0a371-147">Az Azure AD-felhasználók számára meg kell adni egy felhasználónevet és egy jelszót.</span><span class="sxs-lookup"><span data-stu-id="0a371-147">You must provide username and password for an Azure AD user.</span></span> <span data-ttu-id="0a371-148">Ügyeljen arra, hogy a felhasználókat ne többtényezős hitelesítéssel konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="0a371-148">Note that the user should not be configured for multi-factor authentication.</span></span>

    user = input('Enter the user to authenticate with that has permission to subscription: ')
    password = getpass.getpass()

    credentials = UserPassCredentials(user, password)

### <a name="end-user-authentication-for-filesystem-operations"></a><span data-ttu-id="0a371-149">Végfelhasználói hitelesítés fájlrendszerműveletekhez</span><span class="sxs-lookup"><span data-stu-id="0a371-149">End-user authentication for filesystem operations</span></span>

<span data-ttu-id="0a371-150">Használja ezt az eljárást az Azure AD-val való hitelesítésre a fájlrendszerműveleteknél (mappa létrehozása, fájl feltöltése stb).</span><span class="sxs-lookup"><span data-stu-id="0a371-150">Use this to authenticate with Azure AD for filesystem operations (create folder, upload file, etc.).</span></span> <span data-ttu-id="0a371-151">Egy meglévő **natív Azure AD-ügyfélalkalmazással** használja.</span><span class="sxs-lookup"><span data-stu-id="0a371-151">Use this with an existing Azure AD **native client** application.</span></span> <span data-ttu-id="0a371-152">Az Azure AD-felhasználót a hitelesítő adatok kiosztása során ne többtényezős hitelesítéssel konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="0a371-152">The Azure AD user you provide credentials for should not be configured for multi-factor authentication.</span></span>

    tenant_id = 'FILL-IN-HERE'
    client_id = 'FILL-IN-HERE'
    user = input('Enter the user to authenticate with that has permission to subscription: ')
    password = getpass.getpass()

    token = lib.auth(tenant_id, user, password, client_id)

### <a name="service-to-service-authentication-with-client-secret-for-account-management"></a><span data-ttu-id="0a371-153">Szolgáltatások közötti, titkos ügyfélkulccsal történő hitelesítés a fiókkezeléshez</span><span class="sxs-lookup"><span data-stu-id="0a371-153">Service-to-service authentication with client secret for account management</span></span>

<span data-ttu-id="0a371-154">Használja ezt az eljárást az Azure AD-val való hitelesítésre a fiókkezelési műveleteknél (Data Lake Store-fiók létrehozása/törlése stb).</span><span class="sxs-lookup"><span data-stu-id="0a371-154">Use this to authenticate with Azure AD for account management operations (create/delete Data Lake Store account, etc.).</span></span> <span data-ttu-id="0a371-155">A következő kódrészlet használható az alkalmazás nem interaktív hitelesítéséhez, az alkalmazás/egyszerű szolgáltatás titkos ügyfélkódjának használatával.</span><span class="sxs-lookup"><span data-stu-id="0a371-155">The following snippet can be used to authenticate your application non-interactively, using the client secret for an application / service principal.</span></span> <span data-ttu-id="0a371-156">Ezt meglévő „webes” Azure AD-alkalmazással használhatja.</span><span class="sxs-lookup"><span data-stu-id="0a371-156">Use this with an existing Azure AD "Web App" application.</span></span>

    credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')

### <a name="service-to-service-authentication-with-client-secret-for-filesystem-operations"></a><span data-ttu-id="0a371-157">Szolgáltatások közötti, titkos ügyfélkulccsal történő hitelesítés a fájlrendszerműveletekhez</span><span class="sxs-lookup"><span data-stu-id="0a371-157">Service-to-service authentication with client secret for filesystem operations</span></span>

<span data-ttu-id="0a371-158">Használja ezt az eljárást az Azure AD-val való hitelesítésre a fájlrendszerműveleteknél (mappa létrehozása, fájl feltöltése stb).</span><span class="sxs-lookup"><span data-stu-id="0a371-158">Use this to authenticate with Azure AD for filesystem operations (create folder, upload file, etc.).</span></span> <span data-ttu-id="0a371-159">A következő kódrészlet használható az alkalmazás nem interaktív hitelesítéséhez, az alkalmazás/egyszerű szolgáltatás titkos ügyfélkódjának használatával.</span><span class="sxs-lookup"><span data-stu-id="0a371-159">The following snippet can be used to authenticate your application non-interactively, using the client secret for an application / service principal.</span></span> <span data-ttu-id="0a371-160">Ezt meglévő „webes” Azure AD-alkalmazással használhatja.</span><span class="sxs-lookup"><span data-stu-id="0a371-160">Use this with an existing Azure AD "Web App" application.</span></span>

    token = lib.auth(tenant_id = 'FILL-IN-HERE', client_secret = 'FILL-IN-HERE', client_id = 'FILL-IN-HERE')

### <a name="multi-factor-authentication-for-account-management"></a><span data-ttu-id="0a371-161">Többtényezős hitelesítés fiókkezeléshez</span><span class="sxs-lookup"><span data-stu-id="0a371-161">Multi-factor authentication for account management</span></span>

<span data-ttu-id="0a371-162">Használja ezt az eljárást az Azure AD-val való hitelesítésre a fiókkezelési műveleteknél (Data Lake Store-fiók létrehozása/törlése stb).</span><span class="sxs-lookup"><span data-stu-id="0a371-162">Use this to authenticate with Azure AD for account management operations (create/delete Data Lake Store account, etc.).</span></span> <span data-ttu-id="0a371-163">A következő kódrészlet használható az alkalmazás többtényezős hitelesítés használatával történő hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="0a371-163">The following snippet can be used to authenticate your application using multi-factor authentication.</span></span> <span data-ttu-id="0a371-164">Ezt meglévő „webes” Azure AD-alkalmazással használhatja.</span><span class="sxs-lookup"><span data-stu-id="0a371-164">Use this with an existing Azure AD "Web App" application.</span></span>

    authority_host_url = "https://login.microsoftonline.com"
    tenant = "FILL-IN-HERE"
    authority_url = authority_host_url + '/' + tenant
    client_id = 'FILL-IN-HERE'
    redirect = 'urn:ietf:wg:oauth:2.0:oob'
    RESOURCE = 'https://management.core.windows.net/'
    
    context = adal.AuthenticationContext(authority_url)
    code = context.acquire_user_code(RESOURCE, client_id)
    print(code['message'])
    mgmt_token = context.acquire_token_with_device_code(RESOURCE, code, client_id)
    credentials = AADTokenCredentials(mgmt_token, client_id)

### <a name="multi-factor-authentication-for-filesystem-management"></a><span data-ttu-id="0a371-165">Többtényezős hitelesítés fájlrendszerkezeléshez</span><span class="sxs-lookup"><span data-stu-id="0a371-165">Multi-factor authentication for filesystem management</span></span>

<span data-ttu-id="0a371-166">Használja ezt az eljárást az Azure AD-val való hitelesítésre a fájlrendszerműveleteknél (mappa létrehozása, fájl feltöltése stb).</span><span class="sxs-lookup"><span data-stu-id="0a371-166">Use this to authenticate with Azure AD for filesystem operations (create folder, upload file, etc.).</span></span> <span data-ttu-id="0a371-167">A következő kódrészlet használható az alkalmazás többtényezős hitelesítés használatával történő hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="0a371-167">The following snippet can be used to authenticate your application using multi-factor authentication.</span></span> <span data-ttu-id="0a371-168">Ezt meglévő „webes” Azure AD-alkalmazással használhatja.</span><span class="sxs-lookup"><span data-stu-id="0a371-168">Use this with an existing Azure AD "Web App" application.</span></span>

    token = lib.auth(tenant_id='FILL-IN-HERE')

## <a name="create-an-azure-resource-group"></a><span data-ttu-id="0a371-169">Azure-erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="0a371-169">Create an Azure Resource Group</span></span>

<span data-ttu-id="0a371-170">Azure-erőforráscsoport létrehozásához használja a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="0a371-170">Use the following code snippet to create an Azure Resource Group:</span></span>

    ## Declare variables
    subscriptionId= 'FILL-IN-HERE'
    resourceGroup = 'FILL-IN-HERE'
    location = 'eastus2'
    
    ## Create management client object
    resourceClient = ResourceManagementClient(
        credentials,
        subscriptionId
    )
    
    ## Create an Azure Resource Group
    resourceClient.resource_groups.create_or_update(
        resourceGroup,
        ResourceGroup(
            location=location
        )
    )

## <a name="create-clients-and-data-lake-store-account"></a><span data-ttu-id="0a371-171">Ügyfelek és Data Lake Store-fiókok létrehozása</span><span class="sxs-lookup"><span data-stu-id="0a371-171">Create clients and Data Lake Store account</span></span>

<span data-ttu-id="0a371-172">Az alábbi kódrészlet először a Data Lake Store-fiókügyfelet hozza létre.</span><span class="sxs-lookup"><span data-stu-id="0a371-172">The following snippet first creates the Data Lake Store account client.</span></span> <span data-ttu-id="0a371-173">Az ügyfélobjektum használatával hoz majd létre egy Data Lake Store-fiókot.</span><span class="sxs-lookup"><span data-stu-id="0a371-173">It uses the client object to create a Data Lake Store account.</span></span> <span data-ttu-id="0a371-174">Végül pedig létrehoz egy fájlrendszerügyfél-objektumot.</span><span class="sxs-lookup"><span data-stu-id="0a371-174">Finally, the snippet creates a filesystem client object.</span></span>

    ## Declare variables
    subscriptionId = 'FILL-IN-HERE'
    adlsAccountName = 'FILL-IN-HERE'

    ## Create management client object
    adlsAcctClient = DataLakeStoreAccountManagementClient(credentials, subscriptionId)

    ## Create a Data Lake Store account
    adlsAcctResult = adlsAcctClient.account.create(
        resourceGroup,
        adlsAccountName,
        DataLakeStoreAccount(
            location=location
        )
    ).wait()

    ## Create a filesystem client object
    adlsFileSystemClient = core.AzureDLFileSystem(token, store_name=adlsAccountName)

## <a name="list-the-data-lake-store-accounts"></a><span data-ttu-id="0a371-175">A Data Lake Store-fiókok kilistázása</span><span class="sxs-lookup"><span data-stu-id="0a371-175">List the Data Lake Store accounts</span></span>

    ## List the existing Data Lake Store accounts
    result_list_response = adlsAcctClient.account.list()
    result_list = list(result_list_response)
    for items in result_list:
        print(items)

## <a name="create-a-directory"></a><span data-ttu-id="0a371-176">Könyvtár létrehozása</span><span class="sxs-lookup"><span data-stu-id="0a371-176">Create a directory</span></span>

    ## Create a directory
    adlsFileSystemClient.mkdir('/mysampledirectory')

## <a name="upload-a-file"></a><span data-ttu-id="0a371-177">Fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="0a371-177">Upload a file</span></span>


    ## Upload a file
    multithread.ADLUploader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)


## <a name="download-a-file"></a><span data-ttu-id="0a371-178">Fájl letöltése</span><span class="sxs-lookup"><span data-stu-id="0a371-178">Download a file</span></span>

    ## Download a file
    multithread.ADLDownloader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt.out', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)

## <a name="delete-a-directory"></a><span data-ttu-id="0a371-179">Könyvtár törlése</span><span class="sxs-lookup"><span data-stu-id="0a371-179">Delete a directory</span></span>

    ## Delete a directory
    adlsFileSystemClient.rm('/mysampledirectory', recursive=True)

## <a name="see-also"></a><span data-ttu-id="0a371-180">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="0a371-180">See also</span></span>

- [<span data-ttu-id="0a371-181">Biztonságos adattárolás a Data Lake Store-ban</span><span class="sxs-lookup"><span data-stu-id="0a371-181">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
- [<span data-ttu-id="0a371-182">Az Azure Data Lake Analytics használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="0a371-182">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [<span data-ttu-id="0a371-183">Az Azure HDInsight használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="0a371-183">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
- [<span data-ttu-id="0a371-184">A Data Lake Store .NET SDK dokumentációja</span><span class="sxs-lookup"><span data-stu-id="0a371-184">Data Lake Store .NET SDK Reference</span></span>](https://msdn.microsoft.com/library/mt581387.aspx)
- [<span data-ttu-id="0a371-185">A Data Lake Store REST dokumentációja</span><span class="sxs-lookup"><span data-stu-id="0a371-185">Data Lake Store REST Reference</span></span>](https://msdn.microsoft.com/library/mt693424.aspx)
