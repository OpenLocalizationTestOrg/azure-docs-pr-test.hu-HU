---
title: "A .NET SDK használata Azure Data Lake Store-alkalmazások fejlesztéséhez | Microsoft Docs"
description: "Data Lake Store-fiók létrehozása és alapszintű műveletek végrehajtása a Data Lake Store-ban az Azure Data Lake Store .NET SDK használatával"
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
ms.openlocfilehash: 70f94a07b0102e3135eaf85e5877e3502762d7e3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-azure-data-lake-store-using-net-sdk"></a><span data-ttu-id="7c92f-103">Az Azure Data Lake Store használatának első lépései a .NET SDK-val</span><span class="sxs-lookup"><span data-stu-id="7c92f-103">Get started with Azure Data Lake Store using .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7c92f-104">Portál</span><span class="sxs-lookup"><span data-stu-id="7c92f-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="7c92f-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7c92f-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="7c92f-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="7c92f-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="7c92f-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="7c92f-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="7c92f-108">REST API</span><span class="sxs-lookup"><span data-stu-id="7c92f-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="7c92f-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="7c92f-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="7c92f-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="7c92f-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="7c92f-111">Python</span><span class="sxs-lookup"><span data-stu-id="7c92f-111">Python</span></span>](data-lake-store-get-started-python.md)
>
>

<span data-ttu-id="7c92f-112">A cikkből megtudhatja, hogyan végezhet el olyan alapvető műveleteket az [Azure Data Lake Store .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet) segítségével, mint például a mappák létrehozása, adatfájlok le- és feltöltése stb. További információk a Data Lake-ről: [Azure Data Lake Store](data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7c92f-112">Learn how to use the [Azure Data Lake Store .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet) to perform basic operations such as create folders, upload and download data files, etc. For more information about Data Lake, see [Azure Data Lake Store](data-lake-store-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7c92f-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7c92f-113">Prerequisites</span></span>
* <span data-ttu-id="7c92f-114">**Visual Studio 2013, 2015 vagy 2017**</span><span class="sxs-lookup"><span data-stu-id="7c92f-114">**Visual Studio 2013, 2015, or 2017**.</span></span> <span data-ttu-id="7c92f-115">Az alábbi utasítások a Visual Studio 2015 Update 2-t használják.</span><span class="sxs-lookup"><span data-stu-id="7c92f-115">The instructions below use Visual Studio 2015 Update 2.</span></span>

* <span data-ttu-id="7c92f-116">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="7c92f-116">**An Azure subscription**.</span></span> <span data-ttu-id="7c92f-117">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7c92f-117">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="7c92f-118">**Azure Data Lake Store-fiók**.</span><span class="sxs-lookup"><span data-stu-id="7c92f-118">**Azure Data Lake Store account**.</span></span> <span data-ttu-id="7c92f-119">A fióklétrehozás módjáról [Az Azure Data Lake Store használatának első lépései](data-lake-store-get-started-portal.md) című cikk nyújt tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="7c92f-119">For instructions on how to create an account, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>

* <span data-ttu-id="7c92f-120">**Egy Azure Active Directory-alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="7c92f-120">**Create an Azure Active Directory Application**.</span></span> <span data-ttu-id="7c92f-121">A Data Lake Store alkalmazás Azure AD-val történő hitelesítéséhez az Azure AD alkalmazást kell használni.</span><span class="sxs-lookup"><span data-stu-id="7c92f-121">You use the Azure AD application to authenticate the Data Lake Store application with Azure AD.</span></span> <span data-ttu-id="7c92f-122">Az Azure AD-val többféle módon is lehet hitelesíteni. Ezek a következők: **végfelhasználói hitelesítés** vagy **szolgáltatások közötti hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="7c92f-122">There are different approaches to authenticate with Azure AD, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="7c92f-123">A hitelesítéssel kapcsolatban a [Végfelhasználói hitelesítés](data-lake-store-end-user-authenticate-using-active-directory.md) vagy a [Szolgáltatások közötti hitelesítés](data-lake-store-authenticate-using-active-directory.md) című témakörben talál útmutatást és további tudnivalókat.</span><span class="sxs-lookup"><span data-stu-id="7c92f-123">For instructions and more information on how to authenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>

## <a name="create-a-net-application"></a><span data-ttu-id="7c92f-124">.NET-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="7c92f-124">Create a .NET application</span></span>
1. <span data-ttu-id="7c92f-125">Nyissa meg a Visual Studiót, és hozzon létre egy konzolalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7c92f-125">Open Visual Studio and create a console application.</span></span>
2. <span data-ttu-id="7c92f-126">Kattintson a **File** (Fájl) menüben a **New** (Új), majd a **Project** (Projekt) elemre.</span><span class="sxs-lookup"><span data-stu-id="7c92f-126">From the **File** menu, click **New**, and then click **Project**.</span></span>
3. <span data-ttu-id="7c92f-127">Az **Új projekt** területen írja be vagy válassza ki az alábbi értékeket:</span><span class="sxs-lookup"><span data-stu-id="7c92f-127">From **New Project**, type or select the following values:</span></span>

   | <span data-ttu-id="7c92f-128">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="7c92f-128">Property</span></span> | <span data-ttu-id="7c92f-129">Érték</span><span class="sxs-lookup"><span data-stu-id="7c92f-129">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="7c92f-130">Kategória</span><span class="sxs-lookup"><span data-stu-id="7c92f-130">Category</span></span> |<span data-ttu-id="7c92f-131">Sablonok/Visual C#/Windows</span><span class="sxs-lookup"><span data-stu-id="7c92f-131">Templates/Visual C#/Windows</span></span> |
   | <span data-ttu-id="7c92f-132">Sablon</span><span class="sxs-lookup"><span data-stu-id="7c92f-132">Template</span></span> |<span data-ttu-id="7c92f-133">Konzolalkalmazás</span><span class="sxs-lookup"><span data-stu-id="7c92f-133">Console Application</span></span> |
   | <span data-ttu-id="7c92f-134">Név</span><span class="sxs-lookup"><span data-stu-id="7c92f-134">Name</span></span> |<span data-ttu-id="7c92f-135">CreateADLApplication</span><span class="sxs-lookup"><span data-stu-id="7c92f-135">CreateADLApplication</span></span> |
4. <span data-ttu-id="7c92f-136">A projekt létrehozásához kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="7c92f-136">Click **OK** to create the project.</span></span>
5. <span data-ttu-id="7c92f-137">Adja hozzá a NuGet-csomagokat a projekthez.</span><span class="sxs-lookup"><span data-stu-id="7c92f-137">Add the Nuget packages to your project.</span></span>

   1. <span data-ttu-id="7c92f-138">Kattintson a jobb gombbal a projekt nevére a Megoldáskezelőben, majd kattintson a **Manage NuGet Packages** (NuGet-csomagok kezelése) elemre.</span><span class="sxs-lookup"><span data-stu-id="7c92f-138">Right-click the project name in the Solution Explorer and click **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7c92f-139">Győződjön meg arról, hogy a **Nuget Package Manager** (NuGet-csomagkezelő) lapon a **Package source** (Csomag forrása) értéke **nuget.org**, és az **Include prerelease** (Előzetes verzió belefoglalása) jelölőnégyzet be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="7c92f-139">In the **Nuget Package Manager** tab, make sure that **Package source** is set to **nuget.org** and that **Include prerelease** check box is selected.</span></span>
   3. <span data-ttu-id="7c92f-140">Keresse meg és telepítse az alábbi NuGet-csomagokat:</span><span class="sxs-lookup"><span data-stu-id="7c92f-140">Search for and install the following NuGet packages:</span></span>

      * <span data-ttu-id="7c92f-141">`Microsoft.Azure.Management.DataLake.Store` – Ez az oktatóanyag a 2.1.3-as előzetes verziót használja.</span><span class="sxs-lookup"><span data-stu-id="7c92f-141">`Microsoft.Azure.Management.DataLake.Store` - This tutorial uses v2.1.3-preview.</span></span>
      * <span data-ttu-id="7c92f-142">`Microsoft.Rest.ClientRuntime.Azure.Authentication` – Ez az oktatóanyag a 2.2.12-es verziót használja.</span><span class="sxs-lookup"><span data-stu-id="7c92f-142">`Microsoft.Rest.ClientRuntime.Azure.Authentication` - This tutorial uses v2.2.12.</span></span>

        <span data-ttu-id="7c92f-143">![NuGet-forrás hozzáadása](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Új Azure Data Lake-fiók létrehozása")</span><span class="sxs-lookup"><span data-stu-id="7c92f-143">![Add a Nuget source](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Create a new Azure Data Lake account")</span></span>
   4. <span data-ttu-id="7c92f-144">Zárja be a **NuGet-csomagkezelőt**.</span><span class="sxs-lookup"><span data-stu-id="7c92f-144">Close the **Nuget Package Manager**.</span></span>
6. <span data-ttu-id="7c92f-145">Nyissa meg a **Program.cs** fájlt, törölje a meglévő kódot, majd illessze be az alábbi utasításokat, hogy hivatkozásokat a névterekre való hivatkozásokat tudjon felvenni.</span><span class="sxs-lookup"><span data-stu-id="7c92f-145">Open **Program.cs**, delete the existing code, and then include the following statements to add references to namespaces.</span></span>

        using System;
        using System.IO;
        using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates
        using System.Threading;

        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest.Azure.Authentication;

7. <span data-ttu-id="7c92f-146">Deklarálja a változókat az alább látható módon, és adja meg a meglévő Data Lake Store és erőforráscsoport nevének értékét.</span><span class="sxs-lookup"><span data-stu-id="7c92f-146">Declare the variables as shown below, and provide the values for Data Lake Store name and the resource group name that already exist.</span></span> <span data-ttu-id="7c92f-147">Továbbá győződjön meg arról, hogy a megadott helyi elérési út és fájlnév létezik a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7c92f-147">Also, make sure the local path and file name you provide here must exist on the computer.</span></span> <span data-ttu-id="7c92f-148">Vegye fel a következő kódrészletet a névtér-deklarációk után.</span><span class="sxs-lookup"><span data-stu-id="7c92f-148">Add the following code snippet after the namespace declarations.</span></span>

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
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with the name of the resource group containing your Data Lake Store account.
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";

                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = Path.Combine(localFolderPath, "file.txt"); // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = Path.Combine(remoteFolderPath, "file.txt");
                }
            }
        }

<span data-ttu-id="7c92f-149">A cikk fennmaradó részéből megtudhatja, hogyan használhatja az elérhető .NET-metódusokat az olyan műveletek elvégzésére, mint a hitelesítés, a fájlok feltöltése stb.</span><span class="sxs-lookup"><span data-stu-id="7c92f-149">In the remaining sections of the article, you can see how to use the available .NET methods to perform operations such as authentication, file upload, etc.</span></span>

## <a name="authentication"></a><span data-ttu-id="7c92f-150">Authentication</span><span class="sxs-lookup"><span data-stu-id="7c92f-150">Authentication</span></span>

### <a name="if-you-are-using-end-user-authentication-recommended-for-this-tutorial"></a><span data-ttu-id="7c92f-151">Végfelhasználói hitelesítés használata esetén (ehhez az oktatóanyaghoz ajánlott)</span><span class="sxs-lookup"><span data-stu-id="7c92f-151">If you are using end-user authentication (recommended for this tutorial)</span></span>

<span data-ttu-id="7c92f-152">Ezt egy meglévő Azure AD natív alkalmazással használva **interaktív** módon hitelesítheti az alkalmazást, vagyis a rendszer az Azure-beli hitelesítő adatok megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="7c92f-152">Use this with an existing Azure AD native application to authenticate your application **interactively**, which means you will be prompted to enter your Azure credentials.</span></span>

<span data-ttu-id="7c92f-153">Az egyszerű használat érdekében az alábbi kódrészletben alapértelmezett értékek vannak megadva az ügyfél-azonosítóhoz és az átirányítási URI-hez, amelyek bármely Azure-előfizetéssel működnek.</span><span class="sxs-lookup"><span data-stu-id="7c92f-153">For ease of use, the snippet below uses default values for client ID and redirect URI that will work with any Azure subscription.</span></span> <span data-ttu-id="7c92f-154">Ha segítségre van szüksége az oktatóanyag gyorsabb teljesítéséhez, ennek a módszernek a használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="7c92f-154">To help you complete this tutorial faster, we recommend you use this approach.</span></span> <span data-ttu-id="7c92f-155">Az alábbi kódrészletben csak adja meg a bérlőazonosító értékét.</span><span class="sxs-lookup"><span data-stu-id="7c92f-155">In the snippet below, just provide the value for your tenant ID.</span></span> <span data-ttu-id="7c92f-156">Ezt az [Active Directory-alkalmazás létrehozásával kapcsolatos](data-lake-store-end-user-authenticate-using-active-directory.md) szakaszban megadott útmutatás szerint kérheti le.</span><span class="sxs-lookup"><span data-stu-id="7c92f-156">You can retrieve it using the instructions provided at [Create an Active Directory Application](data-lake-store-end-user-authenticate-using-active-directory.md).</span></span>

    // User login via interactive popup
    // Use the client ID of an existing AAD Web application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var tenant_id = "<AAD_tenant_id>"; // Replace this string with the user's Azure Active Directory tenant ID
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(tenant_id, activeDirectoryClientSettings).Result;

<span data-ttu-id="7c92f-157">Néhány tudnivaló a fenti kódrészlettel kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="7c92f-157">A couple of things to know about this snippet above:</span></span>

* <span data-ttu-id="7c92f-158">Az oktatóanyag gyorsabb teljesítése érdekében ez a kódrészlet olyan Azure AD-tartományt és ügyfél-azonosítót használ, amely minden Azure-előfizetés számára alapértelmezés szerint elérhető.</span><span class="sxs-lookup"><span data-stu-id="7c92f-158">To help you complete the tutorial faster, this snippet uses an an Azure AD domain and client ID that is available by default for all Azure subscriptions.</span></span> <span data-ttu-id="7c92f-159">Így **a kódrészletet változtatás nélkül használhatja az alkalmazásában**.</span><span class="sxs-lookup"><span data-stu-id="7c92f-159">So, you can **use this snippet as-is in your application**.</span></span>
* <span data-ttu-id="7c92f-160">Ha azonban a saját Azure AD-tartományát és alkalmazásügyfél-azonosítóját szeretné használni, létre kell hoznia egy natív Azure AD-alkalmazást, majd a létrehozott alkalmazáshoz használnia kell az Azure AD-bérlőazonosítót, az ügyfél-azonosítót és az átirányítási URI-t.</span><span class="sxs-lookup"><span data-stu-id="7c92f-160">However, if you do want to use your own Azure AD domain and application client ID, you must create an Azure AD native application and then use the Azure AD tenant ID, client ID, and redirect URI for the application you created.</span></span> <span data-ttu-id="7c92f-161">Útmutatásért tekintse meg az [Active Directory-alkalmazás a Data Lake Store-ral, végfelhasználói hitelesítéshez való létrehozásával](data-lake-store-end-user-authenticate-using-active-directory.md) kapcsolatos témakört.</span><span class="sxs-lookup"><span data-stu-id="7c92f-161">See [Create an Active Directory Application for end-user authentication with Data Lake Store](data-lake-store-end-user-authenticate-using-active-directory.md) for instructions.</span></span>

### <a name="if-you-are-using-service-to-service-authentication-with-client-secret"></a><span data-ttu-id="7c92f-162">Szolgáltatások közötti, titkos ügyfélkulccsal történő hitelesítés használata esetén</span><span class="sxs-lookup"><span data-stu-id="7c92f-162">If you are using service-to-service authentication with client secret</span></span>
<span data-ttu-id="7c92f-163">A következő kódrészlet használható az alkalmazás **nem interaktív hitelesítéséhez** az alkalmazás/egyszerű szolgáltatás titkos ügyfélkódja/kulcsa segítségével.</span><span class="sxs-lookup"><span data-stu-id="7c92f-163">The following snippet can be used to authenticate your application **non-interactively**, using the client secret / key for an application / service principal.</span></span> <span data-ttu-id="7c92f-164">Ez a megoldás meglévő „webes” Azure AD-alkalmazásokhoz használható.</span><span class="sxs-lookup"><span data-stu-id="7c92f-164">Use this with an existing Azure AD "Web App" Application.</span></span> <span data-ttu-id="7c92f-165">Az Azure AD-webalkalmazás létrehozásával és az alábbi kódrészlethez szükséges ügyfél-azonosító és titkos ügyfélkód lekérésével kapcsolatos útmutatásért lásd az [Active Directory-alkalmazás szolgáltatások közötti hitelesítéshez, a Data Lake Store-ral való létrehozásáról](data-lake-store-authenticate-using-active-directory.md) szóló témakört.</span><span class="sxs-lookup"><span data-stu-id="7c92f-165">For instructions on how to create the Azure AD web application and how to retrieve the client ID and client secret required in the snippet below, see [Create an Active Directory Application for service-to-service authentication with Data Lake Store](data-lake-store-authenticate-using-active-directory.md).</span></span>

    // Service principal / appplication authentication with client secret / key
    // Use the client ID of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());

    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = await ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential);

### <a name="if-you-are-using-service-to-service-authentication-with-certificate"></a><span data-ttu-id="7c92f-166">Szolgáltatások közötti, tanúsítvánnyal történő hitelesítés használata esetén</span><span class="sxs-lookup"><span data-stu-id="7c92f-166">If you are using service-to-service authentication with certificate</span></span>

<span data-ttu-id="7c92f-167">Harmadik lehetőségként a következő kódrészlet is használható az alkalmazás **nem interaktív**, az Azure Active Directory-alkalmazás/egyszerű szolgáltatás tanúsítványával történő hitelesítésére.</span><span class="sxs-lookup"><span data-stu-id="7c92f-167">As a third option, the following snippet can be used to authenticate your application **non-interactively**, using the certificate for an Azure Active Directory application / service principal.</span></span> <span data-ttu-id="7c92f-168">Ez [tanúsítványokkal rendelkező meglévő Azure AD-alkalmazásokhoz](../azure-resource-manager/resource-group-authenticate-service-principal.md) használható.</span><span class="sxs-lookup"><span data-stu-id="7c92f-168">Use this with an existing [Azure AD Application with certificates](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span></span>

    // Service principal / application authentication with certificate
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());

    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = await ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate);

## <a name="create-client-objects"></a><span data-ttu-id="7c92f-169">Ügyfélobjektumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="7c92f-169">Create client objects</span></span>
<span data-ttu-id="7c92f-170">A következő kódrészlet létrehozza a Data Lake Store-fiókot és a fájlrendszeri ügyfélobjektumokat. Ezek a szolgáltatásnak küldött kérések kiadására használatosak.</span><span class="sxs-lookup"><span data-stu-id="7c92f-170">The following snippet creates the Data Lake Store account and filesystem client objects, which are used to issue requests to the service.</span></span>

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds) { SubscriptionId = _subId };
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a><span data-ttu-id="7c92f-171">Az összes Data Lake Store-fiók listázása egy előfizetésen belül</span><span class="sxs-lookup"><span data-stu-id="7c92f-171">List all Data Lake Store accounts within a subscription</span></span>
<span data-ttu-id="7c92f-172">A következő kódrészlet listázza az összes Data Lake Store-fiókot egy Azure-előfizetésen belül.</span><span class="sxs-lookup"><span data-stu-id="7c92f-172">The following snippet lists all Data Lake Store accounts within a given Azure subscription.</span></span>

    // List all ADLS accounts within the subscription
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

## <a name="create-a-directory"></a><span data-ttu-id="7c92f-173">Könyvtár létrehozása</span><span class="sxs-lookup"><span data-stu-id="7c92f-173">Create a directory</span></span>
<span data-ttu-id="7c92f-174">Az alábbi részlet egy `CreateDirectory` metódust mutat be, amely egy könyvtár létrehozására használható egy Data Lake Store-fiókon belül.</span><span class="sxs-lookup"><span data-stu-id="7c92f-174">The following snippet shows a `CreateDirectory` method that you can use to create a directory within a Data Lake Store account.</span></span>

    // Create a directory
    public static async Task CreateDirectory(string path)
    {
        await _adlsFileSystemClient.FileSystem.MkdirsAsync(_adlsAccountName, path);
    }

## <a name="upload-a-file"></a><span data-ttu-id="7c92f-175">Fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="7c92f-175">Upload a file</span></span>
<span data-ttu-id="7c92f-176">Az alábbi részlet egy `UploadFile` metódust mutat be, amely fájlok egy Data Lake Store-fiókba való feltöltésére használható.</span><span class="sxs-lookup"><span data-stu-id="7c92f-176">The following snippet shows an `UploadFile` method that you can use to upload files to a Data Lake Store account.</span></span>

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        _adlsFileSystemClient.FileSystem.UploadFile(_adlsAccountName, srcFilePath, destFilePath, overwrite:force);
    }

<span data-ttu-id="7c92f-177">Az SDK támogatja a rekurzív fel- és letöltést egy helyi fájlelérési út és egy Data Lake Store-fájl elérési útja között.</span><span class="sxs-lookup"><span data-stu-id="7c92f-177">The SDK supports recursive upload and download between a local file path and a Data Lake Store file path.</span></span>    

## <a name="get-file-or-directory-info"></a><span data-ttu-id="7c92f-178">Fájl vagy könyvtár adatainak lekérése</span><span class="sxs-lookup"><span data-stu-id="7c92f-178">Get file or directory info</span></span>
<span data-ttu-id="7c92f-179">Az alábbi részlet egy `GetItemInfo` metódust mutat be, amely a Data Lake Store-ban elérhető fájlok vagy könyvtárak adatainak lekérésére használható.</span><span class="sxs-lookup"><span data-stu-id="7c92f-179">The following snippet shows a `GetItemInfo` method that you can use to retrieve information about a file or directory available in Data Lake Store.</span></span>

    // Get file or directory info
    public static async Task<FileStatusProperties> GetItemInfo(string path)
    {
        return await _adlsFileSystemClient.FileSystem.GetFileStatusAsync(_adlsAccountName, path).FileStatus;
    }

## <a name="list-file-or-directories"></a><span data-ttu-id="7c92f-180">Fájlok vagy könyvtárak listázása</span><span class="sxs-lookup"><span data-stu-id="7c92f-180">List file or directories</span></span>
<span data-ttu-id="7c92f-181">Az alábbi részlet egy `ListItem` metódust mutat be, amely egy Data Lake Store-fiókban található fájlok és könyvtárak listázására használható.</span><span class="sxs-lookup"><span data-stu-id="7c92f-181">The following snippet shows a `ListItem` method that can use to list the file and directories in a Data Lake Store account.</span></span>

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## <a name="concatenate-files"></a><span data-ttu-id="7c92f-182">Fájlok összefűzése</span><span class="sxs-lookup"><span data-stu-id="7c92f-182">Concatenate files</span></span>
<span data-ttu-id="7c92f-183">Az alábbi részlet egy `ConcatenateFiles` metódust mutat be, amely fájlok összefűzésére használható.</span><span class="sxs-lookup"><span data-stu-id="7c92f-183">The following snippet shows a `ConcatenateFiles` method that you use to concatenate files.</span></span>

    // Concatenate files
    public static Task ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        await _adlsFileSystemClient.FileSystem.ConcatAsync(_adlsAccountName, destFilePath, srcFilePaths);
    }

## <a name="append-to-a-file"></a><span data-ttu-id="7c92f-184">Hozzáfűzés fájlhoz</span><span class="sxs-lookup"><span data-stu-id="7c92f-184">Append to a file</span></span>
<span data-ttu-id="7c92f-185">Az alábbi részlet egy `AppendToFile` metódust mutat be, amely az adatok egy Data Lake Store-fiókban tárolt fájlhoz történő hozzáfűzésére használható.</span><span class="sxs-lookup"><span data-stu-id="7c92f-185">The following snippet shows a `AppendToFile` method that you use append data to a file already stored in a Data Lake Store account.</span></span>

    // Append to file
    public static async Task AppendToFile(string path, string content)
    {
        using (var stream = new MemoryStream(Encoding.UTF8.GetBytes(content)))
        {
            await _adlsFileSystemClient.FileSystem.AppendAsync(_adlsAccountName, path, stream);
        }
    }

## <a name="download-a-file"></a><span data-ttu-id="7c92f-186">Fájl letöltése</span><span class="sxs-lookup"><span data-stu-id="7c92f-186">Download a file</span></span>
<span data-ttu-id="7c92f-187">Az alábbi részlet egy `DownloadFile` metódust mutat be, amely egy fájl Data Lake Store-fiókból való letöltésére használható.</span><span class="sxs-lookup"><span data-stu-id="7c92f-187">The following snippet shows a `DownloadFile` method that you use to download a file from a Data Lake Store account.</span></span>

    // Download file
    public static void DownloadFile(string srcFilePath, string destFilePath)
    {
         _adlsFileSystemClient.FileSystem.DownloadFile(_adlsAccountName, srcFilePath, destFilePath);
    }

## <a name="next-steps"></a><span data-ttu-id="7c92f-188">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7c92f-188">Next steps</span></span>
* [<span data-ttu-id="7c92f-189">Biztonságos adattárolás a Data Lake Store-ban</span><span class="sxs-lookup"><span data-stu-id="7c92f-189">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="7c92f-190">Az Azure Data Lake Analytics használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="7c92f-190">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="7c92f-191">Az Azure HDInsight használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="7c92f-191">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
* [<span data-ttu-id="7c92f-192">A Data Lake Store .NET SDK dokumentációja</span><span class="sxs-lookup"><span data-stu-id="7c92f-192">Data Lake Store .NET SDK Reference</span></span>](https://docs.microsoft.com/dotnet/api/?view=azuremgmtdatalakestore-2.1.0-preview&term=DataLake.Store)
* [<span data-ttu-id="7c92f-193">A Data Lake Store REST dokumentációja</span><span class="sxs-lookup"><span data-stu-id="7c92f-193">Data Lake Store REST Reference</span></span>](https://msdn.microsoft.com/library/mt693424.aspx)
