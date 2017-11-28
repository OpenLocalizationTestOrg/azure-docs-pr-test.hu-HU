---
title: "aaaUse hello Python SDK tooget Azure Data Lake Store használatába |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Python SDK toowork Data Lake Store-fiókok és hello fájlrendszer."
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
ms.openlocfilehash: 7061fdf25ef607608bab618a20ddd3d6fc7af01d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-python"></a><span data-ttu-id="fbd54-103">Az Azure Data Lake Store használatának első lépései a Python használatával</span><span class="sxs-lookup"><span data-stu-id="fbd54-103">Get started with Azure Data Lake Store using Python</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="fbd54-104">Portál</span><span class="sxs-lookup"><span data-stu-id="fbd54-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="fbd54-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="fbd54-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="fbd54-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="fbd54-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="fbd54-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="fbd54-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="fbd54-108">REST API</span><span class="sxs-lookup"><span data-stu-id="fbd54-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="fbd54-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="fbd54-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="fbd54-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="fbd54-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="fbd54-111">Python</span><span class="sxs-lookup"><span data-stu-id="fbd54-111">Python</span></span>](data-lake-store-get-started-python.md)
>
>

<span data-ttu-id="fbd54-112">Ismerje meg, hogyan toouse hello Python SDK az Azure és az Azure Data Lake Store tooperform alapvető műveleteket, mint mappák létrehozása, le- és feltöltése az adatfájlok stb. További információk a Data Lake-ről: [Azure Data Lake Store](data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fbd54-112">Learn how toouse hello Python SDK for Azure and Azure Data Lake Store tooperform basic operations such as create folders, upload and download data files, etc. For more information about Data Lake, see [Azure Data Lake Store](data-lake-store-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fbd54-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fbd54-113">Prerequisites</span></span>

* <span data-ttu-id="fbd54-114">**Python**.</span><span class="sxs-lookup"><span data-stu-id="fbd54-114">**Python**.</span></span> <span data-ttu-id="fbd54-115">A Pythont [innen](https://www.python.org/downloads/) töltheti le.</span><span class="sxs-lookup"><span data-stu-id="fbd54-115">You can download Python from [here](https://www.python.org/downloads/).</span></span> <span data-ttu-id="fbd54-116">Ez a cikk a Python 3.5.2-es verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="fbd54-116">This article uses Python 3.5.2.</span></span>

* <span data-ttu-id="fbd54-117">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="fbd54-117">**An Azure subscription**.</span></span> <span data-ttu-id="fbd54-118">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fbd54-118">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="fbd54-119">**Egy Azure Active Directory-alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="fbd54-119">**Create an Azure Active Directory Application**.</span></span> <span data-ttu-id="fbd54-120">Hello Azure AD alkalmazás tooauthenticate hello Data Lake Store-alkalmazás használhatja az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="fbd54-120">You use hello Azure AD application tooauthenticate hello Data Lake Store application with Azure AD.</span></span> <span data-ttu-id="fbd54-121">Nincsenek különböző szempontok tooauthenticate az Azure ad-vel, amelyek **végfelhasználói hitelesítési** vagy **szolgáltatások közötti hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="fbd54-121">There are different approaches tooauthenticate with Azure AD, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="fbd54-122">További információt és útmutatást tooauthenticate, lásd: [végfelhasználói hitelesítési](data-lake-store-end-user-authenticate-using-active-directory.md) vagy [szolgáltatások közötti hitelesítési](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="fbd54-122">For instructions and more information on how tooauthenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>

## <a name="install-hello-modules"></a><span data-ttu-id="fbd54-123">Hello modulok telepítése</span><span class="sxs-lookup"><span data-stu-id="fbd54-123">Install hello modules</span></span>

<span data-ttu-id="fbd54-124">a Data Lake Store pythonos környezetekben toowork, tooinstall három modulok kell.</span><span class="sxs-lookup"><span data-stu-id="fbd54-124">toowork with Data Lake Store using Python, you need tooinstall three modules.</span></span>

* <span data-ttu-id="fbd54-125">Hello `azure-mgmt-resource` modul.</span><span class="sxs-lookup"><span data-stu-id="fbd54-125">hello `azure-mgmt-resource` module.</span></span> <span data-ttu-id="fbd54-126">Ez további Azure-modulokat tartalmaz az Active Directoryhoz és más eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="fbd54-126">This includes Azure modules for Active Directory, etc..</span></span>
* <span data-ttu-id="fbd54-127">Hello `azure-mgmt-datalake-store` modul.</span><span class="sxs-lookup"><span data-stu-id="fbd54-127">hello `azure-mgmt-datalake-store` module.</span></span> <span data-ttu-id="fbd54-128">Ez magában foglalja a hello Azure Data Lake Store fiókkezelési műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="fbd54-128">This includes hello Azure Data Lake Store account management operations.</span></span> <span data-ttu-id="fbd54-129">További információkat erről a modulról [az Azure Data Lake Store kezelési moduljához készült referenciaanyagban](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html) talál.</span><span class="sxs-lookup"><span data-stu-id="fbd54-129">For more information on this module, see [Azure Data Lake Store Management module reference](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html).</span></span>
* <span data-ttu-id="fbd54-130">Hello `azure-datalake-store` modul.</span><span class="sxs-lookup"><span data-stu-id="fbd54-130">hello `azure-datalake-store` module.</span></span> <span data-ttu-id="fbd54-131">Ez magában foglalja a hello Azure Data Lake Store fájlrendszer-műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="fbd54-131">This includes hello Azure Data Lake Store filesystem operations.</span></span> <span data-ttu-id="fbd54-132">További információkat erről a modulról [az Azure Data Lake Store fájlrendszermoduljához készült referenciaanyagban](http://azure-datalake-store.readthedocs.io/en/latest/) talál.</span><span class="sxs-lookup"><span data-stu-id="fbd54-132">For more information on this module, see [Azure Data Lake Store Filesystem module reference](http://azure-datalake-store.readthedocs.io/en/latest/).</span></span>

<span data-ttu-id="fbd54-133">A következő parancsok tooinstall hello modulok hello használata.</span><span class="sxs-lookup"><span data-stu-id="fbd54-133">Use hello following commands tooinstall hello modules.</span></span>

```
pip install azure-mgmt-resource
pip install azure-mgmt-datalake-store
pip install azure-datalake-store
```

## <a name="create-a-new-python-application"></a><span data-ttu-id="fbd54-134">Új Python-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="fbd54-134">Create a new Python application</span></span>

1. <span data-ttu-id="fbd54-135">Az Ön által választott IDE hello hozzon létre egy új Python-alkalmazás, például **mysample.py**.</span><span class="sxs-lookup"><span data-stu-id="fbd54-135">In hello IDE of your choice create a new Python application, for example, **mysample.py**.</span></span>

2. <span data-ttu-id="fbd54-136">A következő sorokat tooimport szükséges hello modulok hello hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fbd54-136">Add hello following lines tooimport hello required modules</span></span>

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

3. <span data-ttu-id="fbd54-137">Mentse a módosításokat toomysample.py.</span><span class="sxs-lookup"><span data-stu-id="fbd54-137">Save changes toomysample.py.</span></span>

## <a name="authentication"></a><span data-ttu-id="fbd54-138">Authentication</span><span class="sxs-lookup"><span data-stu-id="fbd54-138">Authentication</span></span>

<span data-ttu-id="fbd54-139">Ez a szakasz a döntésről bővebben hello különböző módokon tooauthenticate az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="fbd54-139">In this section, we talk about hello different ways tooauthenticate with Azure AD.</span></span> <span data-ttu-id="fbd54-140">elérhető hello lehetőségek a következők:</span><span class="sxs-lookup"><span data-stu-id="fbd54-140">hello options available are:</span></span>

* <span data-ttu-id="fbd54-141">Végfelhasználói hitelesítés</span><span class="sxs-lookup"><span data-stu-id="fbd54-141">End-user authentication</span></span>
* <span data-ttu-id="fbd54-142">Szolgáltatások közötti hitelesítés</span><span class="sxs-lookup"><span data-stu-id="fbd54-142">Service-to-service authentication</span></span>
* <span data-ttu-id="fbd54-143">Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="fbd54-143">Multi-factor authentication</span></span>

<span data-ttu-id="fbd54-144">Ezeket a hitelesítési módokat kell használnia a fiókkezelési és a fájlrendszerkezelési modulokban egyaránt.</span><span class="sxs-lookup"><span data-stu-id="fbd54-144">You must use these authentication options for both account management and filesystem management modules.</span></span>

### <a name="end-user-authentication-for-account-management"></a><span data-ttu-id="fbd54-145">Végfelhasználói hitelesítés fiókkezeléshez</span><span class="sxs-lookup"><span data-stu-id="fbd54-145">End-user authentication for account management</span></span>

<span data-ttu-id="fbd54-146">A tooauthenticate használja az Azure ad-val fiók felügyeleti műveleteket (Létrehozás/törlés Data Lake Store-fiókba, stb.).</span><span class="sxs-lookup"><span data-stu-id="fbd54-146">Use this tooauthenticate with Azure AD for account management operations (create/delete Data Lake Store account, etc.).</span></span> <span data-ttu-id="fbd54-147">Az Azure AD-felhasználók számára meg kell adni egy felhasználónevet és egy jelszót.</span><span class="sxs-lookup"><span data-stu-id="fbd54-147">You must provide username and password for an Azure AD user.</span></span> <span data-ttu-id="fbd54-148">Vegye figyelembe, hogy hello a felhasználó nem a multi-factor authentication kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="fbd54-148">Note that hello user should not be configured for multi-factor authentication.</span></span>

    user = input('Enter hello user tooauthenticate with that has permission toosubscription: ')
    password = getpass.getpass()

    credentials = UserPassCredentials(user, password)

### <a name="end-user-authentication-for-filesystem-operations"></a><span data-ttu-id="fbd54-149">Végfelhasználói hitelesítés fájlrendszerműveletekhez</span><span class="sxs-lookup"><span data-stu-id="fbd54-149">End-user authentication for filesystem operations</span></span>

<span data-ttu-id="fbd54-150">A tooauthenticate használja az Azure ad-val fájlrendszer-műveleteket (létrehozni a mappát, a feltöltött fájlt, stb.).</span><span class="sxs-lookup"><span data-stu-id="fbd54-150">Use this tooauthenticate with Azure AD for filesystem operations (create folder, upload file, etc.).</span></span> <span data-ttu-id="fbd54-151">Egy meglévő **natív Azure AD-ügyfélalkalmazással** használja.</span><span class="sxs-lookup"><span data-stu-id="fbd54-151">Use this with an existing Azure AD **native client** application.</span></span> <span data-ttu-id="fbd54-152">azokat a hitelesítő adatokat hello Azure AD-felhasználó nem a multi-factor authentication kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="fbd54-152">hello Azure AD user you provide credentials for should not be configured for multi-factor authentication.</span></span>

    tenant_id = 'FILL-IN-HERE'
    client_id = 'FILL-IN-HERE'
    user = input('Enter hello user tooauthenticate with that has permission toosubscription: ')
    password = getpass.getpass()

    token = lib.auth(tenant_id, user, password, client_id)

### <a name="service-to-service-authentication-with-client-secret-for-account-management"></a><span data-ttu-id="fbd54-153">Szolgáltatások közötti, titkos ügyfélkulccsal történő hitelesítés a fiókkezeléshez</span><span class="sxs-lookup"><span data-stu-id="fbd54-153">Service-to-service authentication with client secret for account management</span></span>

<span data-ttu-id="fbd54-154">A tooauthenticate használja az Azure ad-val fiók felügyeleti műveleteket (Létrehozás/törlés Data Lake Store-fiókba, stb.).</span><span class="sxs-lookup"><span data-stu-id="fbd54-154">Use this tooauthenticate with Azure AD for account management operations (create/delete Data Lake Store account, etc.).</span></span> <span data-ttu-id="fbd54-155">következő részlet hello lehet használt tooauthenticate az alkalmazás nem interaktív, egy alkalmazás / szolgáltatás egyszerű hello ügyfélkulcs használja.</span><span class="sxs-lookup"><span data-stu-id="fbd54-155">hello following snippet can be used tooauthenticate your application non-interactively, using hello client secret for an application / service principal.</span></span> <span data-ttu-id="fbd54-156">Ezt meglévő „webes” Azure AD-alkalmazással használhatja.</span><span class="sxs-lookup"><span data-stu-id="fbd54-156">Use this with an existing Azure AD "Web App" application.</span></span>

    credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')

### <a name="service-to-service-authentication-with-client-secret-for-filesystem-operations"></a><span data-ttu-id="fbd54-157">Szolgáltatások közötti, titkos ügyfélkulccsal történő hitelesítés a fájlrendszerműveletekhez</span><span class="sxs-lookup"><span data-stu-id="fbd54-157">Service-to-service authentication with client secret for filesystem operations</span></span>

<span data-ttu-id="fbd54-158">A tooauthenticate használja az Azure ad-val fájlrendszer-műveleteket (létrehozni a mappát, a feltöltött fájlt, stb.).</span><span class="sxs-lookup"><span data-stu-id="fbd54-158">Use this tooauthenticate with Azure AD for filesystem operations (create folder, upload file, etc.).</span></span> <span data-ttu-id="fbd54-159">következő részlet hello lehet használt tooauthenticate az alkalmazás nem interaktív, egy alkalmazás / szolgáltatás egyszerű hello ügyfélkulcs használja.</span><span class="sxs-lookup"><span data-stu-id="fbd54-159">hello following snippet can be used tooauthenticate your application non-interactively, using hello client secret for an application / service principal.</span></span> <span data-ttu-id="fbd54-160">Ezt meglévő „webes” Azure AD-alkalmazással használhatja.</span><span class="sxs-lookup"><span data-stu-id="fbd54-160">Use this with an existing Azure AD "Web App" application.</span></span>

    token = lib.auth(tenant_id = 'FILL-IN-HERE', client_secret = 'FILL-IN-HERE', client_id = 'FILL-IN-HERE')

### <a name="multi-factor-authentication-for-account-management"></a><span data-ttu-id="fbd54-161">Többtényezős hitelesítés fiókkezeléshez</span><span class="sxs-lookup"><span data-stu-id="fbd54-161">Multi-factor authentication for account management</span></span>

<span data-ttu-id="fbd54-162">A tooauthenticate használja az Azure ad-val fiók felügyeleti műveleteket (Létrehozás/törlés Data Lake Store-fiókba, stb.).</span><span class="sxs-lookup"><span data-stu-id="fbd54-162">Use this tooauthenticate with Azure AD for account management operations (create/delete Data Lake Store account, etc.).</span></span> <span data-ttu-id="fbd54-163">hello következő kódrészlettel lehet használt tooauthenticate a többtényezős hitelesítést használó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="fbd54-163">hello following snippet can be used tooauthenticate your application using multi-factor authentication.</span></span> <span data-ttu-id="fbd54-164">Ezt meglévő „webes” Azure AD-alkalmazással használhatja.</span><span class="sxs-lookup"><span data-stu-id="fbd54-164">Use this with an existing Azure AD "Web App" application.</span></span>

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

### <a name="multi-factor-authentication-for-filesystem-management"></a><span data-ttu-id="fbd54-165">Többtényezős hitelesítés fájlrendszerkezeléshez</span><span class="sxs-lookup"><span data-stu-id="fbd54-165">Multi-factor authentication for filesystem management</span></span>

<span data-ttu-id="fbd54-166">A tooauthenticate használja az Azure ad-val fájlrendszer-műveleteket (létrehozni a mappát, a feltöltött fájlt, stb.).</span><span class="sxs-lookup"><span data-stu-id="fbd54-166">Use this tooauthenticate with Azure AD for filesystem operations (create folder, upload file, etc.).</span></span> <span data-ttu-id="fbd54-167">hello következő kódrészlettel lehet használt tooauthenticate a többtényezős hitelesítést használó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="fbd54-167">hello following snippet can be used tooauthenticate your application using multi-factor authentication.</span></span> <span data-ttu-id="fbd54-168">Ezt meglévő „webes” Azure AD-alkalmazással használhatja.</span><span class="sxs-lookup"><span data-stu-id="fbd54-168">Use this with an existing Azure AD "Web App" application.</span></span>

    token = lib.auth(tenant_id='FILL-IN-HERE')

## <a name="create-an-azure-resource-group"></a><span data-ttu-id="fbd54-169">Azure-erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="fbd54-169">Create an Azure Resource Group</span></span>

<span data-ttu-id="fbd54-170">A következő kód részlet toocreate Azure-erőforráscsoport hello használata:</span><span class="sxs-lookup"><span data-stu-id="fbd54-170">Use hello following code snippet toocreate an Azure Resource Group:</span></span>

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

## <a name="create-clients-and-data-lake-store-account"></a><span data-ttu-id="fbd54-171">Ügyfelek és Data Lake Store-fiókok létrehozása</span><span class="sxs-lookup"><span data-stu-id="fbd54-171">Create clients and Data Lake Store account</span></span>

<span data-ttu-id="fbd54-172">először a következő kódrészletet hello hello Data Lake Store-fiók ügyfél hoz létre.</span><span class="sxs-lookup"><span data-stu-id="fbd54-172">hello following snippet first creates hello Data Lake Store account client.</span></span> <span data-ttu-id="fbd54-173">Hello ügyfél objektum toocreate egy Data Lake Store-fiókot használ.</span><span class="sxs-lookup"><span data-stu-id="fbd54-173">It uses hello client object toocreate a Data Lake Store account.</span></span> <span data-ttu-id="fbd54-174">Végezetül hello részlet objektumot hoz létre filesystem ügyfél.</span><span class="sxs-lookup"><span data-stu-id="fbd54-174">Finally, hello snippet creates a filesystem client object.</span></span>

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

## <a name="list-hello-data-lake-store-accounts"></a><span data-ttu-id="fbd54-175">Hello Data Lake Store-fiókok listázása</span><span class="sxs-lookup"><span data-stu-id="fbd54-175">List hello Data Lake Store accounts</span></span>

    ## List hello existing Data Lake Store accounts
    result_list_response = adlsAcctClient.account.list()
    result_list = list(result_list_response)
    for items in result_list:
        print(items)

## <a name="create-a-directory"></a><span data-ttu-id="fbd54-176">Könyvtár létrehozása</span><span class="sxs-lookup"><span data-stu-id="fbd54-176">Create a directory</span></span>

    ## Create a directory
    adlsFileSystemClient.mkdir('/mysampledirectory')

## <a name="upload-a-file"></a><span data-ttu-id="fbd54-177">Fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="fbd54-177">Upload a file</span></span>


    ## Upload a file
    multithread.ADLUploader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)


## <a name="download-a-file"></a><span data-ttu-id="fbd54-178">Fájl letöltése</span><span class="sxs-lookup"><span data-stu-id="fbd54-178">Download a file</span></span>

    ## Download a file
    multithread.ADLDownloader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt.out', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)

## <a name="delete-a-directory"></a><span data-ttu-id="fbd54-179">Könyvtár törlése</span><span class="sxs-lookup"><span data-stu-id="fbd54-179">Delete a directory</span></span>

    ## Delete a directory
    adlsFileSystemClient.rm('/mysampledirectory', recursive=True)

## <a name="see-also"></a><span data-ttu-id="fbd54-180">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="fbd54-180">See also</span></span>

- [<span data-ttu-id="fbd54-181">Biztonságos adattárolás a Data Lake Store-ban</span><span class="sxs-lookup"><span data-stu-id="fbd54-181">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
- [<span data-ttu-id="fbd54-182">Az Azure Data Lake Analytics használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="fbd54-182">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [<span data-ttu-id="fbd54-183">Az Azure HDInsight használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="fbd54-183">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
- [<span data-ttu-id="fbd54-184">A Data Lake Store .NET SDK dokumentációja</span><span class="sxs-lookup"><span data-stu-id="fbd54-184">Data Lake Store .NET SDK Reference</span></span>](https://msdn.microsoft.com/library/mt581387.aspx)
- [<span data-ttu-id="fbd54-185">A Data Lake Store REST dokumentációja</span><span class="sxs-lookup"><span data-stu-id="fbd54-185">Data Lake Store REST Reference</span></span>](https://msdn.microsoft.com/library/mt693424.aspx)
