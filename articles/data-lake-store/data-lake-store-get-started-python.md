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
# <a name="get-started-with-azure-data-lake-store-using-python"></a>Az Azure Data Lake Store használatának első lépései a Python használatával

> [!div class="op_single_selector"]
> * [Portál](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Ismerje meg, hogyan toouse hello Python SDK az Azure és az Azure Data Lake Store tooperform alapvető műveleteket, mint mappák létrehozása, le- és feltöltése az adatfájlok stb. További információk a Data Lake-ről: [Azure Data Lake Store](data-lake-store-overview.md).

## <a name="prerequisites"></a>Előfeltételek

* **Python**. A Pythont [innen](https://www.python.org/downloads/) töltheti le. Ez a cikk a Python 3.5.2-es verzióját használja.

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).

* **Egy Azure Active Directory-alkalmazás létrehozása**. Hello Azure AD alkalmazás tooauthenticate hello Data Lake Store-alkalmazás használhatja az Azure ad-val. Nincsenek különböző szempontok tooauthenticate az Azure ad-vel, amelyek **végfelhasználói hitelesítési** vagy **szolgáltatások közötti hitelesítési**. További információt és útmutatást tooauthenticate, lásd: [végfelhasználói hitelesítési](data-lake-store-end-user-authenticate-using-active-directory.md) vagy [szolgáltatások közötti hitelesítési](data-lake-store-authenticate-using-active-directory.md).

## <a name="install-hello-modules"></a>Hello modulok telepítése

a Data Lake Store pythonos környezetekben toowork, tooinstall három modulok kell.

* Hello `azure-mgmt-resource` modul. Ez további Azure-modulokat tartalmaz az Active Directoryhoz és más eszközökhöz.
* Hello `azure-mgmt-datalake-store` modul. Ez magában foglalja a hello Azure Data Lake Store fiókkezelési műveletekhez. További információkat erről a modulról [az Azure Data Lake Store kezelési moduljához készült referenciaanyagban](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html) talál.
* Hello `azure-datalake-store` modul. Ez magában foglalja a hello Azure Data Lake Store fájlrendszer-műveletekhez. További információkat erről a modulról [az Azure Data Lake Store fájlrendszermoduljához készült referenciaanyagban](http://azure-datalake-store.readthedocs.io/en/latest/) talál.

A következő parancsok tooinstall hello modulok hello használata.

```
pip install azure-mgmt-resource
pip install azure-mgmt-datalake-store
pip install azure-datalake-store
```

## <a name="create-a-new-python-application"></a>Új Python-alkalmazás létrehozása

1. Az Ön által választott IDE hello hozzon létre egy új Python-alkalmazás, például **mysample.py**.

2. A következő sorokat tooimport szükséges hello modulok hello hozzáadása

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

3. Mentse a módosításokat toomysample.py.

## <a name="authentication"></a>Authentication

Ez a szakasz a döntésről bővebben hello különböző módokon tooauthenticate az Azure ad-val. elérhető hello lehetőségek a következők:

* Végfelhasználói hitelesítés
* Szolgáltatások közötti hitelesítés
* Multi-Factor Authentication

Ezeket a hitelesítési módokat kell használnia a fiókkezelési és a fájlrendszerkezelési modulokban egyaránt.

### <a name="end-user-authentication-for-account-management"></a>Végfelhasználói hitelesítés fiókkezeléshez

A tooauthenticate használja az Azure ad-val fiók felügyeleti műveleteket (Létrehozás/törlés Data Lake Store-fiókba, stb.). Az Azure AD-felhasználók számára meg kell adni egy felhasználónevet és egy jelszót. Vegye figyelembe, hogy hello a felhasználó nem a multi-factor authentication kell beállítani.

    user = input('Enter hello user tooauthenticate with that has permission toosubscription: ')
    password = getpass.getpass()

    credentials = UserPassCredentials(user, password)

### <a name="end-user-authentication-for-filesystem-operations"></a>Végfelhasználói hitelesítés fájlrendszerműveletekhez

A tooauthenticate használja az Azure ad-val fájlrendszer-műveleteket (létrehozni a mappát, a feltöltött fájlt, stb.). Egy meglévő **natív Azure AD-ügyfélalkalmazással** használja. azokat a hitelesítő adatokat hello Azure AD-felhasználó nem a multi-factor authentication kell beállítani.

    tenant_id = 'FILL-IN-HERE'
    client_id = 'FILL-IN-HERE'
    user = input('Enter hello user tooauthenticate with that has permission toosubscription: ')
    password = getpass.getpass()

    token = lib.auth(tenant_id, user, password, client_id)

### <a name="service-to-service-authentication-with-client-secret-for-account-management"></a>Szolgáltatások közötti, titkos ügyfélkulccsal történő hitelesítés a fiókkezeléshez

A tooauthenticate használja az Azure ad-val fiók felügyeleti műveleteket (Létrehozás/törlés Data Lake Store-fiókba, stb.). következő részlet hello lehet használt tooauthenticate az alkalmazás nem interaktív, egy alkalmazás / szolgáltatás egyszerű hello ügyfélkulcs használja. Ezt meglévő „webes” Azure AD-alkalmazással használhatja.

    credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')

### <a name="service-to-service-authentication-with-client-secret-for-filesystem-operations"></a>Szolgáltatások közötti, titkos ügyfélkulccsal történő hitelesítés a fájlrendszerműveletekhez

A tooauthenticate használja az Azure ad-val fájlrendszer-műveleteket (létrehozni a mappát, a feltöltött fájlt, stb.). következő részlet hello lehet használt tooauthenticate az alkalmazás nem interaktív, egy alkalmazás / szolgáltatás egyszerű hello ügyfélkulcs használja. Ezt meglévő „webes” Azure AD-alkalmazással használhatja.

    token = lib.auth(tenant_id = 'FILL-IN-HERE', client_secret = 'FILL-IN-HERE', client_id = 'FILL-IN-HERE')

### <a name="multi-factor-authentication-for-account-management"></a>Többtényezős hitelesítés fiókkezeléshez

A tooauthenticate használja az Azure ad-val fiók felügyeleti műveleteket (Létrehozás/törlés Data Lake Store-fiókba, stb.). hello következő kódrészlettel lehet használt tooauthenticate a többtényezős hitelesítést használó alkalmazások. Ezt meglévő „webes” Azure AD-alkalmazással használhatja.

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

### <a name="multi-factor-authentication-for-filesystem-management"></a>Többtényezős hitelesítés fájlrendszerkezeléshez

A tooauthenticate használja az Azure ad-val fájlrendszer-műveleteket (létrehozni a mappát, a feltöltött fájlt, stb.). hello következő kódrészlettel lehet használt tooauthenticate a többtényezős hitelesítést használó alkalmazások. Ezt meglévő „webes” Azure AD-alkalmazással használhatja.

    token = lib.auth(tenant_id='FILL-IN-HERE')

## <a name="create-an-azure-resource-group"></a>Azure-erőforráscsoport létrehozása

A következő kód részlet toocreate Azure-erőforráscsoport hello használata:

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

## <a name="create-clients-and-data-lake-store-account"></a>Ügyfelek és Data Lake Store-fiókok létrehozása

először a következő kódrészletet hello hello Data Lake Store-fiók ügyfél hoz létre. Hello ügyfél objektum toocreate egy Data Lake Store-fiókot használ. Végezetül hello részlet objektumot hoz létre filesystem ügyfél.

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

## <a name="list-hello-data-lake-store-accounts"></a>Hello Data Lake Store-fiókok listázása

    ## List hello existing Data Lake Store accounts
    result_list_response = adlsAcctClient.account.list()
    result_list = list(result_list_response)
    for items in result_list:
        print(items)

## <a name="create-a-directory"></a>Könyvtár létrehozása

    ## Create a directory
    adlsFileSystemClient.mkdir('/mysampledirectory')

## <a name="upload-a-file"></a>Fájl feltöltése


    ## Upload a file
    multithread.ADLUploader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)


## <a name="download-a-file"></a>Fájl letöltése

    ## Download a file
    multithread.ADLDownloader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt.out', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)

## <a name="delete-a-directory"></a>Könyvtár törlése

    ## Delete a directory
    adlsFileSystemClient.rm('/mysampledirectory', recursive=True)

## <a name="see-also"></a>Lásd még:

- [Biztonságos adattárolás a Data Lake Store-ban](data-lake-store-secure-data.md)
- [Az Azure Data Lake Analytics használata a Data Lake Store-ral](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Az Azure HDInsight használata a Data Lake Store-ral](data-lake-store-hdinsight-hadoop-use-portal.md)
- [A Data Lake Store .NET SDK dokumentációja](https://msdn.microsoft.com/library/mt581387.aspx)
- [A Data Lake Store REST dokumentációja](https://msdn.microsoft.com/library/mt693424.aspx)
