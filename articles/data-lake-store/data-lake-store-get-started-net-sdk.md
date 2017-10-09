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
# <a name="get-started-with-azure-data-lake-store-using-net-sdk"></a><span data-ttu-id="bb1e3-103">Az Azure Data Lake Store használatának első lépései a .NET SDK-val</span><span class="sxs-lookup"><span data-stu-id="bb1e3-103">Get started with Azure Data Lake Store using .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="bb1e3-104">Portál</span><span class="sxs-lookup"><span data-stu-id="bb1e3-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="bb1e3-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="bb1e3-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="bb1e3-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="bb1e3-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="bb1e3-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="bb1e3-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="bb1e3-108">REST API</span><span class="sxs-lookup"><span data-stu-id="bb1e3-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="bb1e3-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="bb1e3-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="bb1e3-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="bb1e3-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="bb1e3-111">Python</span><span class="sxs-lookup"><span data-stu-id="bb1e3-111">Python</span></span>](data-lake-store-get-started-python.md)
>
>

<span data-ttu-id="bb1e3-112">Megtudhatja, hogyan toouse hello [Azure Data Lake Store .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet) tooperform alapvető műveleteket, mint mappák létrehozása, le- és feltöltése az adatfájlok stb. További információk a Data Lake-ről: [Azure Data Lake Store](data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="bb1e3-112">Learn how toouse hello [Azure Data Lake Store .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet) tooperform basic operations such as create folders, upload and download data files, etc. For more information about Data Lake, see [Azure Data Lake Store](data-lake-store-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bb1e3-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bb1e3-113">Prerequisites</span></span>
* <span data-ttu-id="bb1e3-114">**Visual Studio 2013, 2015 vagy 2017**</span><span class="sxs-lookup"><span data-stu-id="bb1e3-114">**Visual Studio 2013, 2015, or 2017**.</span></span> <span data-ttu-id="bb1e3-115">az alábbi utasítások hello használja a Visual Studio 2015 Update 2.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-115">hello instructions below use Visual Studio 2015 Update 2.</span></span>

* <span data-ttu-id="bb1e3-116">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-116">**An Azure subscription**.</span></span> <span data-ttu-id="bb1e3-117">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bb1e3-117">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="bb1e3-118">**Azure Data Lake Store-fiók**.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-118">**Azure Data Lake Store account**.</span></span> <span data-ttu-id="bb1e3-119">Útmutatást toocreate egy olyan fiók, lásd: [Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="bb1e3-119">For instructions on how toocreate an account, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>

* <span data-ttu-id="bb1e3-120">**Egy Azure Active Directory-alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-120">**Create an Azure Active Directory Application**.</span></span> <span data-ttu-id="bb1e3-121">Hello Azure AD alkalmazás tooauthenticate hello Data Lake Store-alkalmazás használhatja az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-121">You use hello Azure AD application tooauthenticate hello Data Lake Store application with Azure AD.</span></span> <span data-ttu-id="bb1e3-122">Nincsenek különböző szempontok tooauthenticate az Azure ad-vel, amelyek **végfelhasználói hitelesítési** vagy **szolgáltatások közötti hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-122">There are different approaches tooauthenticate with Azure AD, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="bb1e3-123">További információt és útmutatást tooauthenticate, lásd: [végfelhasználói hitelesítési](data-lake-store-end-user-authenticate-using-active-directory.md) vagy [szolgáltatások közötti hitelesítési](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="bb1e3-123">For instructions and more information on how tooauthenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>

## <a name="create-a-net-application"></a><span data-ttu-id="bb1e3-124">.NET-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="bb1e3-124">Create a .NET application</span></span>
1. <span data-ttu-id="bb1e3-125">Nyissa meg a Visual Studiót, és hozzon létre egy konzolalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-125">Open Visual Studio and create a console application.</span></span>
2. <span data-ttu-id="bb1e3-126">A hello **fájl** menüben kattintson a **új**, és kattintson a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-126">From hello **File** menu, click **New**, and then click **Project**.</span></span>
3. <span data-ttu-id="bb1e3-127">A **új projekt**, írja be vagy válassza ki a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="bb1e3-127">From **New Project**, type or select hello following values:</span></span>

   | <span data-ttu-id="bb1e3-128">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="bb1e3-128">Property</span></span> | <span data-ttu-id="bb1e3-129">Érték</span><span class="sxs-lookup"><span data-stu-id="bb1e3-129">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="bb1e3-130">Kategória</span><span class="sxs-lookup"><span data-stu-id="bb1e3-130">Category</span></span> |<span data-ttu-id="bb1e3-131">Sablonok/Visual C#/Windows</span><span class="sxs-lookup"><span data-stu-id="bb1e3-131">Templates/Visual C#/Windows</span></span> |
   | <span data-ttu-id="bb1e3-132">Sablon</span><span class="sxs-lookup"><span data-stu-id="bb1e3-132">Template</span></span> |<span data-ttu-id="bb1e3-133">Konzolalkalmazás</span><span class="sxs-lookup"><span data-stu-id="bb1e3-133">Console Application</span></span> |
   | <span data-ttu-id="bb1e3-134">Név</span><span class="sxs-lookup"><span data-stu-id="bb1e3-134">Name</span></span> |<span data-ttu-id="bb1e3-135">CreateADLApplication</span><span class="sxs-lookup"><span data-stu-id="bb1e3-135">CreateADLApplication</span></span> |
4. <span data-ttu-id="bb1e3-136">Kattintson a **OK** toocreate hello projekt.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-136">Click **OK** toocreate hello project.</span></span>
5. <span data-ttu-id="bb1e3-137">Hello Nuget-csomagok tooyour projekt hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-137">Add hello Nuget packages tooyour project.</span></span>

   1. <span data-ttu-id="bb1e3-138">Kattintson a jobb gombbal a hello projekt nevére a Megoldáskezelőben hello, és kattintson a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-138">Right-click hello project name in hello Solution Explorer and click **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="bb1e3-139">A hello **Nuget-Csomagkezelő** lapra, győződjön meg arról, hogy **csomag forrása** értéke túl**nuget.org** , és hogy **közé tartoznak az előzetes** jelölőnégyzetet ki van választva.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-139">In hello **Nuget Package Manager** tab, make sure that **Package source** is set too**nuget.org** and that **Include prerelease** check box is selected.</span></span>
   3. <span data-ttu-id="bb1e3-140">Keresse meg, és telepítse a következő NuGet-csomagok hello:</span><span class="sxs-lookup"><span data-stu-id="bb1e3-140">Search for and install hello following NuGet packages:</span></span>

      * <span data-ttu-id="bb1e3-141">`Microsoft.Azure.Management.DataLake.Store` – Ez az oktatóanyag a 2.1.3-as előzetes verziót használja.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-141">`Microsoft.Azure.Management.DataLake.Store` - This tutorial uses v2.1.3-preview.</span></span>
      * <span data-ttu-id="bb1e3-142">`Microsoft.Rest.ClientRuntime.Azure.Authentication` – Ez az oktatóanyag a 2.2.12-es verziót használja.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-142">`Microsoft.Rest.ClientRuntime.Azure.Authentication` - This tutorial uses v2.2.12.</span></span>

        <span data-ttu-id="bb1e3-143">![NuGet-forrás hozzáadása](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Új Azure Data Lake-fiók létrehozása")</span><span class="sxs-lookup"><span data-stu-id="bb1e3-143">![Add a Nuget source](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Create a new Azure Data Lake account")</span></span>
   4. <span data-ttu-id="bb1e3-144">Bezárás hello **Nuget Package Manager**.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-144">Close hello **Nuget Package Manager**.</span></span>
6. <span data-ttu-id="bb1e3-145">Nyissa meg **Program.cs**, törölje hello meglévő kódot, és az a következő utasítások tooadd hivatkozások toonamespaces hello.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-145">Open **Program.cs**, delete hello existing code, and then include hello following statements tooadd references toonamespaces.</span></span>

        using System;
        using System.IO;
        using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates
        using System.Threading;

        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest.Azure.Authentication;

7. <span data-ttu-id="bb1e3-146">Alább látható módon hello változót deklarál, és adjon meg hello értékek Data Lake Store nevét és hello az erőforráscsoport neve, amely már létezik.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-146">Declare hello variables as shown below, and provide hello values for Data Lake Store name and hello resource group name that already exist.</span></span> <span data-ttu-id="bb1e3-147">Ellenőrizze azt is, hello helyi elérési útja és neve itt léteznie kell a hello számítógépen.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-147">Also, make sure hello local path and file name you provide here must exist on hello computer.</span></span> <span data-ttu-id="bb1e3-148">Adja hozzá a következő kódrészletet hello névtér-deklarációk után hello.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-148">Add hello following code snippet after hello namespace declarations.</span></span>

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

<span data-ttu-id="bb1e3-149">A szakaszok hello cikk hátralévő hello láthatja, hogyan toouse hello elérhető .NET módszerek tooperform műveletek, például a hitelesítést, a fájl feltöltése stb.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-149">In hello remaining sections of hello article, you can see how toouse hello available .NET methods tooperform operations such as authentication, file upload, etc.</span></span>

## <a name="authentication"></a><span data-ttu-id="bb1e3-150">Authentication</span><span class="sxs-lookup"><span data-stu-id="bb1e3-150">Authentication</span></span>

### <a name="if-you-are-using-end-user-authentication-recommended-for-this-tutorial"></a><span data-ttu-id="bb1e3-151">Végfelhasználói hitelesítés használata esetén (ehhez az oktatóanyaghoz ajánlott)</span><span class="sxs-lookup"><span data-stu-id="bb1e3-151">If you are using end-user authentication (recommended for this tutorial)</span></span>

<span data-ttu-id="bb1e3-152">Ezzel egy meglévő Azure AD natív alkalmazás tooauthenticate az alkalmazás **interaktív**, amely azt jelenti, hogy fogja kéri tooenter Azure hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-152">Use this with an existing Azure AD native application tooauthenticate your application **interactively**, which means you will be prompted tooenter your Azure credentials.</span></span>

<span data-ttu-id="bb1e3-153">Könnyű használatra hello részlet az alábbi ügyfél-Azonosítóját és átirányítási URI-t, hogy működni fog-e bármely Azure-előfizetéshez tartozó alapértelmezett értékeket használja.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-153">For ease of use, hello snippet below uses default values for client ID and redirect URI that will work with any Azure subscription.</span></span> <span data-ttu-id="bb1e3-154">Ez az oktatóanyag gyorsabban befejeződjenek toohelp, azt javasoljuk ezt a módszert használja.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-154">toohelp you complete this tutorial faster, we recommend you use this approach.</span></span> <span data-ttu-id="bb1e3-155">Az alábbi hello részlet csak adja meg hello érték a bérlő azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-155">In hello snippet below, just provide hello value for your tenant ID.</span></span> <span data-ttu-id="bb1e3-156">Hello utasításokat követve hozzáférhet [hozzon létre egy Active Directory-alkalmazást](data-lake-store-end-user-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="bb1e3-156">You can retrieve it using hello instructions provided at [Create an Active Directory Application](data-lake-store-end-user-authenticate-using-active-directory.md).</span></span>

    // User login via interactive popup
    // Use hello client ID of an existing AAD Web application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var tenant_id = "<AAD_tenant_id>"; // Replace this string with hello user's Azure Active Directory tenant ID
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(tenant_id, activeDirectoryClientSettings).Result;

<span data-ttu-id="bb1e3-157">Néhány dolgot tooknow a fenti részlet kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="bb1e3-157">A couple of things tooknow about this snippet above:</span></span>

* <span data-ttu-id="bb1e3-158">toohelp hello oktatóanyag gyorsabban befejeződjenek, ez kódrészletet használja az Azure AD tartományi és az ügyfél-azonosító alapértelmezés szerint az összes Azure-előfizetések érhető el.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-158">toohelp you complete hello tutorial faster, this snippet uses an an Azure AD domain and client ID that is available by default for all Azure subscriptions.</span></span> <span data-ttu-id="bb1e3-159">Így **a kódrészletet változtatás nélkül használhatja az alkalmazásában**.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-159">So, you can **use this snippet as-is in your application**.</span></span>
* <span data-ttu-id="bb1e3-160">Azonban ha toouse a saját Azure AD-tartomány és az alkalmazás ügyfél-azonosító, létre kell hoznia egy natív Azure AD-alkalmazást és majd használata hello Azure AD bérlői azonosító, ügyfél-azonosító, és hello alkalmazás átirányítási URI létrehozott.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-160">However, if you do want toouse your own Azure AD domain and application client ID, you must create an Azure AD native application and then use hello Azure AD tenant ID, client ID, and redirect URI for hello application you created.</span></span> <span data-ttu-id="bb1e3-161">Útmutatásért tekintse meg az [Active Directory-alkalmazás a Data Lake Store-ral, végfelhasználói hitelesítéshez való létrehozásával](data-lake-store-end-user-authenticate-using-active-directory.md) kapcsolatos témakört.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-161">See [Create an Active Directory Application for end-user authentication with Data Lake Store](data-lake-store-end-user-authenticate-using-active-directory.md) for instructions.</span></span>

### <a name="if-you-are-using-service-to-service-authentication-with-client-secret"></a><span data-ttu-id="bb1e3-162">Szolgáltatások közötti, titkos ügyfélkulccsal történő hitelesítés használata esetén</span><span class="sxs-lookup"><span data-stu-id="bb1e3-162">If you are using service-to-service authentication with client secret</span></span>
<span data-ttu-id="bb1e3-163">következő részlet hello lehet használt tooauthenticate az alkalmazás **nem interaktív**, hello ügyfél titkos kulcs használata / egy alkalmazás / szolgáltatás elsődleges kulcsát.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-163">hello following snippet can be used tooauthenticate your application **non-interactively**, using hello client secret / key for an application / service principal.</span></span> <span data-ttu-id="bb1e3-164">Ez a megoldás meglévő „webes” Azure AD-alkalmazásokhoz használható.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-164">Use this with an existing Azure AD "Web App" Application.</span></span> <span data-ttu-id="bb1e3-165">Hogyan toocreate hello Azure AD-webalkalmazás, és hogyan tooretrieve hello ügyfél-azonosító és a szükséges hello részlet az alábbi, a titkos ügyfélkódot: kapcsolatos utasításokat [hozzon létre egy Active Directory-alkalmazást a szolgáltatások közötti hitelesítéshez adatok Lake Store](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="bb1e3-165">For instructions on how toocreate hello Azure AD web application and how tooretrieve hello client ID and client secret required in hello snippet below, see [Create an Active Directory Application for service-to-service authentication with Data Lake Store](data-lake-store-authenticate-using-active-directory.md).</span></span>

    // Service principal / appplication authentication with client secret / key
    // Use hello client ID of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());

    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = await ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential);

### <a name="if-you-are-using-service-to-service-authentication-with-certificate"></a><span data-ttu-id="bb1e3-166">Szolgáltatások közötti, tanúsítvánnyal történő hitelesítés használata esetén</span><span class="sxs-lookup"><span data-stu-id="bb1e3-166">If you are using service-to-service authentication with certificate</span></span>

<span data-ttu-id="bb1e3-167">Mivel egy harmadik beállítás, hello részlet követően lehet használt tooauthenticate az alkalmazás **nem interaktív**, hello tanúsítványt használ az Azure Active Directory-alkalmazás / szolgáltatás egyszerű.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-167">As a third option, hello following snippet can be used tooauthenticate your application **non-interactively**, using hello certificate for an Azure Active Directory application / service principal.</span></span> <span data-ttu-id="bb1e3-168">Ez [tanúsítványokkal rendelkező meglévő Azure AD-alkalmazásokhoz](../azure-resource-manager/resource-group-authenticate-service-principal.md) használható.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-168">Use this with an existing [Azure AD Application with certificates](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span></span>

    // Service principal / application authentication with certificate
    // Use hello client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());

    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = await ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate);

## <a name="create-client-objects"></a><span data-ttu-id="bb1e3-169">Ügyfélobjektumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="bb1e3-169">Create client objects</span></span>
<span data-ttu-id="bb1e3-170">hello következő kódrészlettel hello Data Lake Store-fiók és a fájlrendszer ügyfél-objektumokat hoz létre, használt tooissue kéri toohello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-170">hello following snippet creates hello Data Lake Store account and filesystem client objects, which are used tooissue requests toohello service.</span></span>

    // Create client objects and set hello subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds) { SubscriptionId = _subId };
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a><span data-ttu-id="bb1e3-171">Az összes Data Lake Store-fiók listázása egy előfizetésen belül</span><span class="sxs-lookup"><span data-stu-id="bb1e3-171">List all Data Lake Store accounts within a subscription</span></span>
<span data-ttu-id="bb1e3-172">hello következő kódrészlettel listája egy adott Azure-előfizetésen belül minden Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-172">hello following snippet lists all Data Lake Store accounts within a given Azure subscription.</span></span>

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

## <a name="create-a-directory"></a><span data-ttu-id="bb1e3-173">Könyvtár létrehozása</span><span class="sxs-lookup"><span data-stu-id="bb1e3-173">Create a directory</span></span>
<span data-ttu-id="bb1e3-174">a következő kódrészletet mutat be hello egy `CreateDirectory` toocreate egy könyvtár, egy Data Lake Store-fiókban használt módszer.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-174">hello following snippet shows a `CreateDirectory` method that you can use toocreate a directory within a Data Lake Store account.</span></span>

    // Create a directory
    public static async Task CreateDirectory(string path)
    {
        await _adlsFileSystemClient.FileSystem.MkdirsAsync(_adlsAccountName, path);
    }

## <a name="upload-a-file"></a><span data-ttu-id="bb1e3-175">Fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="bb1e3-175">Upload a file</span></span>
<span data-ttu-id="bb1e3-176">a következő kódrészletet mutat be hello egy `UploadFile` módszer használható tooupload fájlok tooa Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-176">hello following snippet shows an `UploadFile` method that you can use tooupload files tooa Data Lake Store account.</span></span>

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        _adlsFileSystemClient.FileSystem.UploadFile(_adlsAccountName, srcFilePath, destFilePath, overwrite:force);
    }

<span data-ttu-id="bb1e3-177">hello SDK támogatja a rekurzív feltöltése és letöltéséhez között egy helyi fájl elérési útját és egy Data Lake Store-fájl elérési útját.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-177">hello SDK supports recursive upload and download between a local file path and a Data Lake Store file path.</span></span>    

## <a name="get-file-or-directory-info"></a><span data-ttu-id="bb1e3-178">Fájl vagy könyvtár adatainak lekérése</span><span class="sxs-lookup"><span data-stu-id="bb1e3-178">Get file or directory info</span></span>
<span data-ttu-id="bb1e3-179">a következő kódrészletet mutat be hello egy `GetItemInfo` használható egy elérhető fájlok vagy könyvtárak tooretrieve információk a Data Lake Store metódust.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-179">hello following snippet shows a `GetItemInfo` method that you can use tooretrieve information about a file or directory available in Data Lake Store.</span></span>

    // Get file or directory info
    public static async Task<FileStatusProperties> GetItemInfo(string path)
    {
        return await _adlsFileSystemClient.FileSystem.GetFileStatusAsync(_adlsAccountName, path).FileStatus;
    }

## <a name="list-file-or-directories"></a><span data-ttu-id="bb1e3-180">Fájlok vagy könyvtárak listázása</span><span class="sxs-lookup"><span data-stu-id="bb1e3-180">List file or directories</span></span>
<span data-ttu-id="bb1e3-181">a következő kódrészletet mutat be hello egy `ListItem` egy Data Lake Store-fiókban is toolist hello fájlok és mappák használt módszert.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-181">hello following snippet shows a `ListItem` method that can use toolist hello file and directories in a Data Lake Store account.</span></span>

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## <a name="concatenate-files"></a><span data-ttu-id="bb1e3-182">Fájlok összefűzése</span><span class="sxs-lookup"><span data-stu-id="bb1e3-182">Concatenate files</span></span>
<span data-ttu-id="bb1e3-183">a következő kódrészletet mutat be hello egy `ConcatenateFiles` tooconcatenate fájlok használt módszer.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-183">hello following snippet shows a `ConcatenateFiles` method that you use tooconcatenate files.</span></span>

    // Concatenate files
    public static Task ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        await _adlsFileSystemClient.FileSystem.ConcatAsync(_adlsAccountName, destFilePath, srcFilePaths);
    }

## <a name="append-tooa-file"></a><span data-ttu-id="bb1e3-184">Tooa fájl hozzáfűzése</span><span class="sxs-lookup"><span data-stu-id="bb1e3-184">Append tooa file</span></span>
<span data-ttu-id="bb1e3-185">a következő kódrészletet mutat be hello egy `AppendToFile` módszere hozzáfűzése egy Data Lake Store-fiókban tárolt adatok tooa fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-185">hello following snippet shows a `AppendToFile` method that you use append data tooa file already stored in a Data Lake Store account.</span></span>

    // Append toofile
    public static async Task AppendToFile(string path, string content)
    {
        using (var stream = new MemoryStream(Encoding.UTF8.GetBytes(content)))
        {
            await _adlsFileSystemClient.FileSystem.AppendAsync(_adlsAccountName, path, stream);
        }
    }

## <a name="download-a-file"></a><span data-ttu-id="bb1e3-186">Fájl letöltése</span><span class="sxs-lookup"><span data-stu-id="bb1e3-186">Download a file</span></span>
<span data-ttu-id="bb1e3-187">a következő kódrészletet mutat be hello egy `DownloadFile` , hogy egy fájl Data Lake Store-fiók toodownload használt módszer.</span><span class="sxs-lookup"><span data-stu-id="bb1e3-187">hello following snippet shows a `DownloadFile` method that you use toodownload a file from a Data Lake Store account.</span></span>

    // Download file
    public static void DownloadFile(string srcFilePath, string destFilePath)
    {
         _adlsFileSystemClient.FileSystem.DownloadFile(_adlsAccountName, srcFilePath, destFilePath);
    }

## <a name="next-steps"></a><span data-ttu-id="bb1e3-188">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bb1e3-188">Next steps</span></span>
* [<span data-ttu-id="bb1e3-189">Biztonságos adattárolás a Data Lake Store-ban</span><span class="sxs-lookup"><span data-stu-id="bb1e3-189">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="bb1e3-190">Az Azure Data Lake Analytics használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="bb1e3-190">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="bb1e3-191">Az Azure HDInsight használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="bb1e3-191">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
* [<span data-ttu-id="bb1e3-192">A Data Lake Store .NET SDK dokumentációja</span><span class="sxs-lookup"><span data-stu-id="bb1e3-192">Data Lake Store .NET SDK Reference</span></span>](https://docs.microsoft.com/dotnet/api/?view=azuremgmtdatalakestore-2.1.0-preview&term=DataLake.Store)
* [<span data-ttu-id="bb1e3-193">A Data Lake Store REST dokumentációja</span><span class="sxs-lookup"><span data-stu-id="bb1e3-193">Data Lake Store REST Reference</span></span>](https://msdn.microsoft.com/library/mt693424.aspx)
