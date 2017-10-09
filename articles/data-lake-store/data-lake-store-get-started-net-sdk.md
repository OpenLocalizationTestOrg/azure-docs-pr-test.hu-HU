---
title: "aaaUse hello .NET SDK toodevelop alkalmazások az Azure Data Lake Store |} Microsoft Docs"
description: "Azure Data Lake Store .NET SDK toocreate Data Lake Store-fiók és alapszintű műveletek végrehajtása a Data Lake Store hello"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ea57d5a9-2929-4473-9d30-08227912aba7
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/09/2017
ms.author: nitinme
ms.openlocfilehash: cb3a1dfb2f6379f728069d66b0ee77ce0f838fe7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-net-sdk"></a>Az Azure Data Lake Store használatának első lépései a .NET SDK-val
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

Megtudhatja, hogyan toouse hello [Azure Data Lake Store .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet) tooperform alapvető műveleteket, mint mappák létrehozása, le- és feltöltése az adatfájlok stb. További információk a Data Lake-ről: [Azure Data Lake Store](data-lake-store-overview.md).

## <a name="prerequisites"></a>Előfeltételek
* **Visual Studio 2013, 2015 vagy 2017** az alábbi utasítások hello használja a Visual Studio 2015 Update 2.

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Data Lake Store-fiók**. Útmutatást toocreate egy olyan fiók, lásd: [Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md)

* **Egy Azure Active Directory-alkalmazás létrehozása**. Hello Azure AD alkalmazás tooauthenticate hello Data Lake Store-alkalmazás használhatja az Azure ad-val. Nincsenek különböző szempontok tooauthenticate az Azure ad-vel, amelyek **végfelhasználói hitelesítési** vagy **szolgáltatások közötti hitelesítési**. További információt és útmutatást tooauthenticate, lásd: [végfelhasználói hitelesítési](data-lake-store-end-user-authenticate-using-active-directory.md) vagy [szolgáltatások közötti hitelesítési](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-a-net-application"></a>.NET-alkalmazás létrehozása
1. Nyissa meg a Visual Studiót, és hozzon létre egy konzolalkalmazást.
2. A hello **fájl** menüben kattintson a **új**, és kattintson a **projekt**.
3. A **új projekt**, írja be vagy válassza ki a következő értékek hello:

   | Tulajdonság | Érték |
   | --- | --- |
   | Kategória |Sablonok/Visual C#/Windows |
   | Sablon |Konzolalkalmazás |
   | Név |CreateADLApplication |
4. Kattintson a **OK** toocreate hello projekt.
5. Hello Nuget-csomagok tooyour projekt hozzáadása.

   1. Kattintson a jobb gombbal a hello projekt nevére a Megoldáskezelőben hello, és kattintson a **NuGet-csomagok kezelése**.
   2. A hello **Nuget-Csomagkezelő** lapra, győződjön meg arról, hogy **csomag forrása** értéke túl**nuget.org** , és hogy **közé tartoznak az előzetes** jelölőnégyzetet ki van választva.
   3. Keresse meg, és telepítse a következő NuGet-csomagok hello:

      * `Microsoft.Azure.Management.DataLake.Store` – Ez az oktatóanyag a 2.1.3-as előzetes verziót használja.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication` – Ez az oktatóanyag a 2.2.12-es verziót használja.

        ![NuGet-forrás hozzáadása](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Új Azure Data Lake-fiók létrehozása")
   4. Bezárás hello **Nuget Package Manager**.
6. Nyissa meg **Program.cs**, törölje hello meglévő kódot, és az a következő utasítások tooadd hivatkozások toonamespaces hello.

        using System;
        using System.IO;
        using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates
        using System.Threading;

        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest.Azure.Authentication;

7. Alább látható módon hello változót deklarál, és adjon meg hello értékek Data Lake Store nevét és hello az erőforráscsoport neve, amely már létezik. Ellenőrizze azt is, hello helyi elérési útja és neve itt léteznie kell a hello számítógépen. Adja hozzá a következő kódrészletet hello névtér-deklarációk után hello.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;

                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with hello name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with hello name of hello resource group containing your Data Lake Store account.
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";

                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = Path.Combine(localFolderPath, "file.txt"); // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = Path.Combine(remoteFolderPath, "file.txt");
                }
            }
        }

A szakaszok hello cikk hátralévő hello láthatja, hogyan toouse hello elérhető .NET módszerek tooperform műveletek, például a hitelesítést, a fájl feltöltése stb.

## <a name="authentication"></a>Authentication

### <a name="if-you-are-using-end-user-authentication-recommended-for-this-tutorial"></a>Végfelhasználói hitelesítés használata esetén (ehhez az oktatóanyaghoz ajánlott)

Ezzel egy meglévő Azure AD natív alkalmazás tooauthenticate az alkalmazás **interaktív**, amely azt jelenti, hogy fogja kéri tooenter Azure hitelesítő adatait.

Könnyű használatra hello részlet az alábbi ügyfél-Azonosítóját és átirányítási URI-t, hogy működni fog-e bármely Azure-előfizetéshez tartozó alapértelmezett értékeket használja. Ez az oktatóanyag gyorsabban befejeződjenek toohelp, azt javasoljuk ezt a módszert használja. Az alábbi hello részlet csak adja meg hello érték a bérlő azonosítóját. Hello utasításokat követve hozzáférhet [hozzon létre egy Active Directory-alkalmazást](data-lake-store-end-user-authenticate-using-active-directory.md).

    // User login via interactive popup
    // Use hello client ID of an existing AAD Web application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var tenant_id = "<AAD_tenant_id>"; // Replace this string with hello user's Azure Active Directory tenant ID
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(tenant_id, activeDirectoryClientSettings).Result;

Néhány dolgot tooknow a fenti részlet kapcsolatban:

* toohelp hello oktatóanyag gyorsabban befejeződjenek, ez kódrészletet használja az Azure AD tartományi és az ügyfél-azonosító alapértelmezés szerint az összes Azure-előfizetések érhető el. Így **a kódrészletet változtatás nélkül használhatja az alkalmazásában**.
* Azonban ha toouse a saját Azure AD-tartomány és az alkalmazás ügyfél-azonosító, létre kell hoznia egy natív Azure AD-alkalmazást és majd használata hello Azure AD bérlői azonosító, ügyfél-azonosító, és hello alkalmazás átirányítási URI létrehozott. Útmutatásért tekintse meg az [Active Directory-alkalmazás a Data Lake Store-ral, végfelhasználói hitelesítéshez való létrehozásával](data-lake-store-end-user-authenticate-using-active-directory.md) kapcsolatos témakört.

### <a name="if-you-are-using-service-to-service-authentication-with-client-secret"></a>Szolgáltatások közötti, titkos ügyfélkulccsal történő hitelesítés használata esetén
következő részlet hello lehet használt tooauthenticate az alkalmazás **nem interaktív**, hello ügyfél titkos kulcs használata / egy alkalmazás / szolgáltatás elsődleges kulcsát. Ez a megoldás meglévő „webes” Azure AD-alkalmazásokhoz használható. Hogyan toocreate hello Azure AD-webalkalmazás, és hogyan tooretrieve hello ügyfél-azonosító és a szükséges hello részlet az alábbi, a titkos ügyfélkódot: kapcsolatos utasításokat [hozzon létre egy Active Directory-alkalmazást a szolgáltatások közötti hitelesítéshez adatok Lake Store](data-lake-store-authenticate-using-active-directory.md).

    // Service principal / appplication authentication with client secret / key
    // Use hello client ID of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());

    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = await ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential);

### <a name="if-you-are-using-service-to-service-authentication-with-certificate"></a>Szolgáltatások közötti, tanúsítvánnyal történő hitelesítés használata esetén

Mivel egy harmadik beállítás, hello részlet követően lehet használt tooauthenticate az alkalmazás **nem interaktív**, hello tanúsítványt használ az Azure Active Directory-alkalmazás / szolgáltatás egyszerű. Ez [tanúsítványokkal rendelkező meglévő Azure AD-alkalmazásokhoz](../azure-resource-manager/resource-group-authenticate-service-principal.md) használható.

    // Service principal / application authentication with certificate
    // Use hello client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());

    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = await ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate);

## <a name="create-client-objects"></a>Ügyfélobjektumok létrehozása
hello következő kódrészlettel hello Data Lake Store-fiók és a fájlrendszer ügyfél-objektumokat hoz létre, használt tooissue kéri toohello szolgáltatást.

    // Create client objects and set hello subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds) { SubscriptionId = _subId };
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a>Az összes Data Lake Store-fiók listázása egy előfizetésen belül
hello következő kódrészlettel listája egy adott Azure-előfizetésen belül minden Data Lake Store-fiók.

    // List all ADLS accounts within hello subscription
    public static async Task<List<DataLakeStoreAccount>> ListAdlStoreAccounts()
    {
        var response = await _adlsClient.Account.ListAsync();
        var accounts = new List<DataLakeStoreAccount>(response);

        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }

        return accounts;
    }

## <a name="create-a-directory"></a>Könyvtár létrehozása
a következő kódrészletet mutat be hello egy `CreateDirectory` toocreate egy könyvtár, egy Data Lake Store-fiókban használt módszer.

    // Create a directory
    public static async Task CreateDirectory(string path)
    {
        await _adlsFileSystemClient.FileSystem.MkdirsAsync(_adlsAccountName, path);
    }

## <a name="upload-a-file"></a>Fájl feltöltése
a következő kódrészletet mutat be hello egy `UploadFile` módszer használható tooupload fájlok tooa Data Lake Store-fiók.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        _adlsFileSystemClient.FileSystem.UploadFile(_adlsAccountName, srcFilePath, destFilePath, overwrite:force);
    }

hello SDK támogatja a rekurzív feltöltése és letöltéséhez között egy helyi fájl elérési útját és egy Data Lake Store-fájl elérési útját.    

## <a name="get-file-or-directory-info"></a>Fájl vagy könyvtár adatainak lekérése
a következő kódrészletet mutat be hello egy `GetItemInfo` használható egy elérhető fájlok vagy könyvtárak tooretrieve információk a Data Lake Store metódust.

    // Get file or directory info
    public static async Task<FileStatusProperties> GetItemInfo(string path)
    {
        return await _adlsFileSystemClient.FileSystem.GetFileStatusAsync(_adlsAccountName, path).FileStatus;
    }

## <a name="list-file-or-directories"></a>Fájlok vagy könyvtárak listázása
a következő kódrészletet mutat be hello egy `ListItem` egy Data Lake Store-fiókban is toolist hello fájlok és mappák használt módszert.

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## <a name="concatenate-files"></a>Fájlok összefűzése
a következő kódrészletet mutat be hello egy `ConcatenateFiles` tooconcatenate fájlok használt módszer.

    // Concatenate files
    public static Task ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        await _adlsFileSystemClient.FileSystem.ConcatAsync(_adlsAccountName, destFilePath, srcFilePaths);
    }

## <a name="append-tooa-file"></a>Tooa fájl hozzáfűzése
a következő kódrészletet mutat be hello egy `AppendToFile` módszere hozzáfűzése egy Data Lake Store-fiókban tárolt adatok tooa fájlhoz.

    // Append toofile
    public static async Task AppendToFile(string path, string content)
    {
        using (var stream = new MemoryStream(Encoding.UTF8.GetBytes(content)))
        {
            await _adlsFileSystemClient.FileSystem.AppendAsync(_adlsAccountName, path, stream);
        }
    }

## <a name="download-a-file"></a>Fájl letöltése
a következő kódrészletet mutat be hello egy `DownloadFile` , hogy egy fájl Data Lake Store-fiók toodownload használt módszer.

    // Download file
    public static void DownloadFile(string srcFilePath, string destFilePath)
    {
         _adlsFileSystemClient.FileSystem.DownloadFile(_adlsAccountName, srcFilePath, destFilePath);
    }

## <a name="next-steps"></a>Következő lépések
* [Biztonságos adattárolás a Data Lake Store-ban](data-lake-store-secure-data.md)
* [Az Azure Data Lake Analytics használata a Data Lake Store-ral](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Az Azure HDInsight használata a Data Lake Store-ral](data-lake-store-hdinsight-hadoop-use-portal.md)
* [A Data Lake Store .NET SDK dokumentációja](https://docs.microsoft.com/dotnet/api/?view=azuremgmtdatalakestore-2.1.0-preview&term=DataLake.Store)
* [A Data Lake Store REST dokumentációja](https://msdn.microsoft.com/library/mt693424.aspx)
