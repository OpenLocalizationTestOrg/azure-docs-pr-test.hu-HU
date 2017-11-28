---
title: "aaaUse hello Java SDK toodevelop alkalmazások az Azure Data Lake Store |} Microsoft Docs"
description: "Azure Data Lake Store Java SDK toocreate Data Lake Store-fiók és alapszintű műveletek végrehajtása a Data Lake Store hello"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: d10e09db-5232-4e84-bb50-52efc2c21887
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/28/2017
ms.author: nitinme
ms.openlocfilehash: d3bcee449c2a2a4bd2f7b241af46ecc010b6b62e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-java"></a><span data-ttu-id="41d8d-103">Az Azure Data Lake Store használatának első lépései a Java használatával</span><span class="sxs-lookup"><span data-stu-id="41d8d-103">Get started with Azure Data Lake Store using Java</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="41d8d-104">Portál</span><span class="sxs-lookup"><span data-stu-id="41d8d-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="41d8d-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="41d8d-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="41d8d-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="41d8d-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="41d8d-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="41d8d-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="41d8d-108">REST API</span><span class="sxs-lookup"><span data-stu-id="41d8d-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="41d8d-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="41d8d-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="41d8d-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="41d8d-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="41d8d-111">Python</span><span class="sxs-lookup"><span data-stu-id="41d8d-111">Python</span></span>](data-lake-store-get-started-python.md)
>
> 

<span data-ttu-id="41d8d-112">Ismerje meg, hogyan toouse hello Azure Data Lake Store Java SDK tooperform alapvető műveleteket, mint létre mappák, le- és feltöltése az adatfájlok stb. További információk a Data Lake-ről: [Azure Data Lake Store](data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="41d8d-112">Learn how toouse hello Azure Data Lake Store Java SDK tooperform basic operations such as create folders, upload and download data files, etc. For more information about Data Lake, see [Azure Data Lake Store](data-lake-store-overview.md).</span></span>

<span data-ttu-id="41d8d-113">Érheti el az Azure Data Lake Store: hello Java SDK API docs [Azure Data Lake Store Java API docs](https://azure.github.io/azure-data-lake-store-java/javadoc/).</span><span class="sxs-lookup"><span data-stu-id="41d8d-113">You can access hello Java SDK API docs for Azure Data Lake Store at [Azure Data Lake Store Java API docs](https://azure.github.io/azure-data-lake-store-java/javadoc/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="41d8d-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="41d8d-114">Prerequisites</span></span>
* <span data-ttu-id="41d8d-115">Java-fejlesztőkészlet (JDK 7 vagy újabb, Java 1.7 vagy újabb verzió használatával)</span><span class="sxs-lookup"><span data-stu-id="41d8d-115">Java Development Kit (JDK 7 or higher, using Java version 1.7 or higher)</span></span>
* <span data-ttu-id="41d8d-116">Azure Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="41d8d-116">Azure Data Lake Store account.</span></span> <span data-ttu-id="41d8d-117">Hajtsa végre a hello található utasítások segítségével: [Ismerkedés az Azure Data Lake Store használatának hello Azure Portal](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="41d8d-117">Follow hello instructions at [Get started with Azure Data Lake Store using hello Azure Portal](data-lake-store-get-started-portal.md).</span></span>
* <span data-ttu-id="41d8d-118">[Maven](https://maven.apache.org/install.html).</span><span class="sxs-lookup"><span data-stu-id="41d8d-118">[Maven](https://maven.apache.org/install.html).</span></span> <span data-ttu-id="41d8d-119">Ez az oktatóanyag a Mavent használja a build- és projektfüggőségek kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="41d8d-119">This tutorial uses Maven for build and project dependencies.</span></span> <span data-ttu-id="41d8d-120">Bár lehetséges toobuild nélkül Maven vagy a gradle-lel build rendszert használ, akkor ezek rendszerek győződjön sokkal könnyebb toomanage függőségek.</span><span class="sxs-lookup"><span data-stu-id="41d8d-120">Although it is possible toobuild without using a build system like Maven or Gradle, these systems make is much easier toomanage dependencies.</span></span>
* <span data-ttu-id="41d8d-121">(Nem kötelező) [IntelliJ IDEA](https://www.jetbrains.com/idea/download/), [Eclipse](https://www.eclipse.org/downloads/) vagy hasonló integrált fejlesztőkörnyezet.</span><span class="sxs-lookup"><span data-stu-id="41d8d-121">(Optional) And IDE like [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) or [Eclipse](https://www.eclipse.org/downloads/) or similar.</span></span>

## <a name="how-do-i-authenticate-using-azure-active-directory"></a><span data-ttu-id="41d8d-122">Hogyan végezhető el a hitelesítés az Azure Active Directory használatával?</span><span class="sxs-lookup"><span data-stu-id="41d8d-122">How do I authenticate using Azure Active Directory?</span></span>
<span data-ttu-id="41d8d-123">Ez az oktatóanyag egy Azure AD alkalmazás ügyfél titkos tooretrieve az Azure Active Directory-tokent (szolgáltatások közötti hitelesítés) használjuk.</span><span class="sxs-lookup"><span data-stu-id="41d8d-123">In this tutorial we use a Azure AD application client secret tooretrieve an Azure Active Directory token (service-to-service authentication).</span></span> <span data-ttu-id="41d8d-124">A token toocreate egy Data Lake Store ügyfél tooperform műveletek fájlt és a directory-műveletek használjuk.</span><span class="sxs-lookup"><span data-stu-id="41d8d-124">We use this token toocreate an Data Lake Store client object tooperform operations file and directory operations.</span></span> <span data-ttu-id="41d8d-125">Hogyan Azure Data Lake Store használatát tooauthenticate hello ügyfélkulcs konfigurálásáról végezzük hello magas szintű lépéseket:</span><span class="sxs-lookup"><span data-stu-id="41d8d-125">For instructions on how tooauthenticate with Azure Data Lake Store using hello client secret, we perform hello following high-level steps:</span></span>

1. <span data-ttu-id="41d8d-126">Azure AD-webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="41d8d-126">Create an Azure AD web application</span></span>
2. <span data-ttu-id="41d8d-127">Hello ügyfél-azonosító, a titkos ügyfélkulcs és a hello Azure AD-webalkalmazás token-végpont beolvasása.</span><span class="sxs-lookup"><span data-stu-id="41d8d-127">Retrieve hello client ID, client secret, and token endpoint for hello Azure AD web application.</span></span>
3. <span data-ttu-id="41d8d-128">Data Lake Store mappára vagy fájlra, amelyet a Java-alkalmazás létrehozásakor hello tooaccess hello hozzáférés hello Azure AD-webalkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="41d8d-128">Configure access for hello Azure AD web application on hello Data Lake Store file/folder that you want tooaccess from hello Java application you are creating.</span></span>

<span data-ttu-id="41d8d-129">Hogyan tooperform ezeket a lépéseket: kapcsolatos utasításokat [hozzon létre egy Active Directory-alkalmazást](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="41d8d-129">For instructions on how tooperform these steps, see [Create an Active Directory application](data-lake-store-authenticate-using-active-directory.md).</span></span>

<span data-ttu-id="41d8d-130">Az Azure Active Directory nyújt más lehetőségek is tooretrieve jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="41d8d-130">Azure Active Directory provides other options as well tooretrieve a token.</span></span> <span data-ttu-id="41d8d-131">Választhatja ki a számos különböző hitelesítési mechanizmusok toosuit helyzetnek, például egy futó böngészővel, egy asztali alkalmazás elosztott alkalmazás vagy egy kiszolgáló helyszíni alkalmazást vagy egy Azure virtuális alkalmazás gép.</span><span class="sxs-lookup"><span data-stu-id="41d8d-131">You can pick from a number of different authentication mechanisms toosuit your scenario, for example, an application running in a browser, an application distributed as a desktop application, or a server application running on-premises or in an Azure virtual machine.</span></span> <span data-ttu-id="41d8d-132">Továbbá különböző típusú hitelesítő adatok közül választhat, például jelszavak, tanúsítványok, kétfaktoros hitelesítés, stb. Emellett Azure Active Directory lehetővé teszi a toosynchronize a helyszíni Active Directory-felhasználók hello felhővel.</span><span class="sxs-lookup"><span data-stu-id="41d8d-132">You can also pick from different types of credentials like passwords, certificates, 2-factor authentication, etc. In addition, Azure Active Directory allows you toosynchronize your on-premises Active Directory users with hello cloud.</span></span> <span data-ttu-id="41d8d-133">További információt a [Hitelesítési eljárások az Azure Active Directoryhoz](../active-directory/active-directory-authentication-scenarios.md) című témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="41d8d-133">For details, see [Authentication Scenarios for Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md).</span></span> 

## <a name="create-a-java-application"></a><span data-ttu-id="41d8d-134">Java-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="41d8d-134">Create a Java application</span></span>
<span data-ttu-id="41d8d-135">rendelkezésre álló hello kódminta [a Githubon](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) bemutatja, hogyan hello folyamatot, amely fájlok hello tárolóban, összefűzi, letölti a fájlokat, fájlok létrehozása és törlése néhány hello tárolójában.</span><span class="sxs-lookup"><span data-stu-id="41d8d-135">hello code sample available [on GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) walks you through hello process of creating files in hello store, concatenating files, downloading a file, and deleting some files in hello store.</span></span> <span data-ttu-id="41d8d-136">Ez a szakasz hello cikk ismerteti az hello kód legfontosabb részeit hello keresztül.</span><span class="sxs-lookup"><span data-stu-id="41d8d-136">This section of hello article walk you through hello main parts of hello code.</span></span>

1. <span data-ttu-id="41d8d-137">Hozzon létre egy Maven project az [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) hello parancssori vagy az IDE használatával.</span><span class="sxs-lookup"><span data-stu-id="41d8d-137">Create a Maven project using [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) from hello command-line or using an IDE.</span></span> <span data-ttu-id="41d8d-138">Hogyan toocreate Java projekt IntelliJ, lásd: [Itt](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html).</span><span class="sxs-lookup"><span data-stu-id="41d8d-138">For instructions on how toocreate a Java project using IntelliJ, see [here](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html).</span></span> <span data-ttu-id="41d8d-139">Útmutatást a projekt eclipse-ben programmal toocreate lásd [Itt](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm).</span><span class="sxs-lookup"><span data-stu-id="41d8d-139">For instructions on how toocreate a project using Eclipse, see [here](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm).</span></span> 
2. <span data-ttu-id="41d8d-140">Adja hozzá a következő függőségek tooyour Maven hello **pom.xml** fájlt.</span><span class="sxs-lookup"><span data-stu-id="41d8d-140">Add hello following dependencies tooyour Maven **pom.xml** file.</span></span> <span data-ttu-id="41d8d-141">Adja hozzá a következő szövegrészletet közötti hello hello  **\<version >** címke és hello  **\</project >** címke:</span><span class="sxs-lookup"><span data-stu-id="41d8d-141">Add hello following snippet of text between hello **\</version>** tag and hello **\</project>** tag:</span></span>
   
        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.1.5</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>
   
    <span data-ttu-id="41d8d-142">hello első függőség toouse hello Data Lake Store SDK (`azure-data-lake-store-sdk`) adattárból hello maven.</span><span class="sxs-lookup"><span data-stu-id="41d8d-142">hello first dependency is toouse hello Data Lake Store SDK (`azure-data-lake-store-sdk`) from hello maven repository.</span></span> <span data-ttu-id="41d8d-143">második függőségi hello (`slf4j-nop`) van toospecify mely naplózási keretrendszer toouse ehhez az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="41d8d-143">hello second dependency (`slf4j-nop`) is toospecify which logging framework toouse for this application.</span></span> <span data-ttu-id="41d8d-144">hello Data Lake Store SDK használ [slf4j](http://www.slf4j.org/) naplózási homlokzati, amely lehetővé teszi, hogy számos népszerű naplózási keretrendszert, például a log4j, válassza a Java-naplózás, logback, stb., vagy nem.</span><span class="sxs-lookup"><span data-stu-id="41d8d-144">hello Data Lake Store SDK uses [slf4j](http://www.slf4j.org/) logging façade, which lets you choose from a number of popular logging frameworks, like log4j, Java logging, logback, etc., or no logging.</span></span> <span data-ttu-id="41d8d-145">Ehhez a példához letiltjuk a naplózást, ezért használjuk hello **slf4j-nop** kötés.</span><span class="sxs-lookup"><span data-stu-id="41d8d-145">For this example, we will disable logging, hence we use hello **slf4j-nop** binding.</span></span> <span data-ttu-id="41d8d-146">toouse más naplózási beállításokat az alkalmazásban, lásd: [Itt](http://www.slf4j.org/manual.html#projectDep).</span><span class="sxs-lookup"><span data-stu-id="41d8d-146">toouse other logging options in your app, see [here](http://www.slf4j.org/manual.html#projectDep).</span></span>

### <a name="add-hello-application-code"></a><span data-ttu-id="41d8d-147">Hello alkalmazáskód hozzáadása</span><span class="sxs-lookup"><span data-stu-id="41d8d-147">Add hello application code</span></span>
<span data-ttu-id="41d8d-148">Három fő részből áll toohello kódot.</span><span class="sxs-lookup"><span data-stu-id="41d8d-148">There are three main parts toohello code.</span></span>

1. <span data-ttu-id="41d8d-149">Hello Azure Active Directory jogkivonat beszerzése</span><span class="sxs-lookup"><span data-stu-id="41d8d-149">Obtain hello Azure Active Directory token</span></span>
2. <span data-ttu-id="41d8d-150">Hello token toocreate egy Data Lake Store ügyfél használja.</span><span class="sxs-lookup"><span data-stu-id="41d8d-150">Use hello token toocreate a Data Lake Store client.</span></span>
3. <span data-ttu-id="41d8d-151">Hello Data Lake Store Ügyfélműveletek tooperform használja.</span><span class="sxs-lookup"><span data-stu-id="41d8d-151">Use hello Data Lake Store client tooperform operations.</span></span>

#### <a name="step-1-obtain-an-azure-active-directory-token"></a><span data-ttu-id="41d8d-152">1. lépés: Az Azure Active Directory-jogkivonat beszerzése.</span><span class="sxs-lookup"><span data-stu-id="41d8d-152">Step 1: Obtain an Azure Active Directory token.</span></span>
<span data-ttu-id="41d8d-153">Data Lake Store SDK hello biztosít, amelyekkel kezelheti a hello biztonsági jogkivonatokat kényelmes módszert tootalk toohello Data Lake Store-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="41d8d-153">hello Data Lake Store SDK provides convenient methods that let you manage hello security tokens needed tootalk toohello Data Lake Store account.</span></span> <span data-ttu-id="41d8d-154">Azonban hello SDK nem határozza meg, hogy a metódusokról csak használható.</span><span class="sxs-lookup"><span data-stu-id="41d8d-154">However, hello SDK does not mandate that only these methods be used.</span></span> <span data-ttu-id="41d8d-155">Is használhatja, sem egyéb módszerrel lehet beszerezni a token is, például használjanak hello [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java), vagy egyéni kódot.</span><span class="sxs-lookup"><span data-stu-id="41d8d-155">You can use any other means of obtaining token as well, like using hello [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java), or your own custom code.</span></span>

<span data-ttu-id="41d8d-156">Data Lake Store SDK tooobtain token hello Active Directory webalkalmazás korábban létrehozott toouse hello valamelyikével hello alosztályokat `AccessTokenProvider` (használja az alábbi hello példában `ClientCredsTokenProvider`).</span><span class="sxs-lookup"><span data-stu-id="41d8d-156">toouse hello Data Lake Store SDK tooobtain token for hello Active Directory Web application you created earlier, use one of hello subclasses of `AccessTokenProvider` (hello example below uses `ClientCredsTokenProvider`).</span></span> <span data-ttu-id="41d8d-157">hello jogkivonat-szolgáltató gyorsítótárak hello hitelesítő adatok tooobtain hello lexikális elem szerepel a memória, és automatikusan megújítja hello tokent, ha tooexpire szól.</span><span class="sxs-lookup"><span data-stu-id="41d8d-157">hello token provider caches hello creds used tooobtain hello token in memory, and automatically renews hello token if it is about tooexpire.</span></span> <span data-ttu-id="41d8d-158">A saját alosztályokat lehetséges toocreate `AccessTokenProvider` , jogkivonatok akkor kapja meg, amelyet a felhasználói kód, de most hozzuk csak használata hello egy megadott hello SDK.</span><span class="sxs-lookup"><span data-stu-id="41d8d-158">It is possible toocreate your own subclasses of `AccessTokenProvider` so tokens are obtained by your customer code, but for now let's just use hello one provided in hello SDK.</span></span>

<span data-ttu-id="41d8d-159">Cserélje le **FILL Itt** értékekkel hello tényleges hello Azure Active Directory webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="41d8d-159">Replace **FILL-IN-HERE** with hello actual values for hello Azure Active Directory Web application.</span></span>

    private static String clientId = "FILL-IN-HERE";
    private static String authTokenEndpoint = "FILL-IN-HERE";
    private static String clientKey = "FILL-IN-HERE";

    AccessTokenProvider provider = new ClientCredsTokenProvider(authTokenEndpoint, clientId, clientKey);

#### <a name="step-2-create-an-azure-data-lake-store-client-adlstoreclient-object"></a><span data-ttu-id="41d8d-160">2. lépés: Az Azure Data Lake Store-ügyfél (ADLStoreClient) objektumának létrehozása</span><span class="sxs-lookup"><span data-stu-id="41d8d-160">Step 2: Create an Azure Data Lake Store client (ADLStoreClient) object</span></span>
<span data-ttu-id="41d8d-161">Létrehozása egy [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) objektum szükséges toospecify hello Data Lake Store fiók nevét és hello jogkivonat-szolgáltató hello előző lépésben létrehozott.</span><span class="sxs-lookup"><span data-stu-id="41d8d-161">Creating an [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) object requires you toospecify hello Data Lake Store account name and hello token provider you generated in hello last step.</span></span> <span data-ttu-id="41d8d-162">Vegye figyelembe, hogy hello Data Lake Store-fiók nevének kell toobe egy teljesen minősített tartománynevét.</span><span class="sxs-lookup"><span data-stu-id="41d8d-162">Note that hello Data Lake Store account name needs toobe a fully qualified domain name.</span></span> <span data-ttu-id="41d8d-163">A **FILL-IN-HERE** értéket például a következő tartománynévre cserélheti ki: **mydatalakestore.azuredatalakestore.net**.</span><span class="sxs-lookup"><span data-stu-id="41d8d-163">For example, replace **FILL-IN-HERE** with something like **mydatalakestore.azuredatalakestore.net**.</span></span>

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just hello account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, provider);

### <a name="step-3-use-hello-adlstoreclient-tooperform-file-and-directory-operations"></a><span data-ttu-id="41d8d-164">3. lépés: Hello ADLStoreClient tooperform fájlokra és könyvtárakra műveletek használata</span><span class="sxs-lookup"><span data-stu-id="41d8d-164">Step 3: Use hello ADLStoreClient tooperform file and directory operations</span></span>
<span data-ttu-id="41d8d-165">hello kódot az alábbi példa kódtöredékek néhány gyakori műveletek tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="41d8d-165">hello code below contains example snippets of some common operations.</span></span> <span data-ttu-id="41d8d-166">Megnézheti teljes hello [Data Lake Store Java SDK API-dokumentumok](https://azure.github.io/azure-data-lake-store-java/javadoc/) a hello **ADLStoreClient** objektum toosee más műveleteket.</span><span class="sxs-lookup"><span data-stu-id="41d8d-166">You can look at hello full [Data Lake Store Java SDK API docs](https://azure.github.io/azure-data-lake-store-java/javadoc/) of hello **ADLStoreClient** object toosee other operations.</span></span>

<span data-ttu-id="41d8d-167">Vegye figyelmembe, hogy a fájlok olvasása és írása szabványos Java-adatfolyamok használatával történik.</span><span class="sxs-lookup"><span data-stu-id="41d8d-167">Note that files are read from and written into using standard Java streams.</span></span> <span data-ttu-id="41d8d-168">Ez azt jelenti, hogy tud-e réteg hello Java adatfolyamok fölött hello Data Lake Store az adatfolyamokat, a standard Java funkciók (például nyomtató adatfolyamok formázott vagy bármely hello tömörítés és titkosítás adatfolyamokat a további funkciók toobenefit bármelyikét "felső, stb.).</span><span class="sxs-lookup"><span data-stu-id="41d8d-168">This means that you can layer any of hello Java streams on top of hello Data Lake Store streams toobenefit from standard Java functionality (e.g., Print streams for formatted output, or any of hello compression or encryption streams for additional functionality on top, etc.).</span></span>

     // create file and write some content
     String filename = "/a/b/c.txt";
     OutputStream stream = client.createFile(filename, IfExists.OVERWRITE  );
     PrintStream out = new PrintStream(stream);
     for (int i = 1; i <= 10; i++) {
         out.println("This is line #" + i);
         out.format("This is hello same line (%d), but using formatted output. %n", i);
     }
     out.close();
    
    // set file permission
    client.setPermission(filename, "744");

    // append toofile
    stream = client.getAppendStream(filename);
    stream.write(getSampleContent());
    stream.close();

    // Read File
    InputStream in = client.getReadStream(filename);
    byte[] b = new byte[64000];
    while (in.read(b) != -1) {
        System.out.write(b);
    }
    in.close();

    // concatenate hello two files into one
    List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
    client.concatenateFiles("/a/b/f.txt", fileList);

    //rename hello file
    client.rename("/a/b/f.txt", "/a/b/g.txt");

    // list directory contents
    List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
    System.out.println("Directory listing for directory /a/b:");
    for (DirectoryEntry entry : list) {
        printDirectoryInfo(entry);
    }

    // delete directory along with all hello subdirectories and files in it
    client.deleteRecursive("/a");

#### <a name="step-4-build-and-run-hello-application"></a><span data-ttu-id="41d8d-169">4. lépés: Build és hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="41d8d-169">Step 4: Build and run hello application</span></span>
1. <span data-ttu-id="41d8d-170">egy IDE belül a toorun keresse meg, és nyomja le az ENTER hello **futtatása** gombra.</span><span class="sxs-lookup"><span data-stu-id="41d8d-170">toorun from within an IDE, locate and press hello **Run** button.</span></span> <span data-ttu-id="41d8d-171">a Maven használata toorun [exec: exec](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html).</span><span class="sxs-lookup"><span data-stu-id="41d8d-171">toorun from Maven, use [exec:exec](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html).</span></span>
2. <span data-ttu-id="41d8d-172">egy önálló jar, amelyek futtatásához a parancssori hello jar tartalmazza, minden függőségekkel rendelkező hello segítségével tooproduce [Maven szerelvény beépülő modul](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html).</span><span class="sxs-lookup"><span data-stu-id="41d8d-172">tooproduce a standalone jar that you can run from command-line build hello jar with all dependencies included, using hello [Maven assembly plugin](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html).</span></span> <span data-ttu-id="41d8d-173">a hello pom.xml hello [példa forráskód a githubon](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) példa bemutatja, hogyan van toodo ez.</span><span class="sxs-lookup"><span data-stu-id="41d8d-173">hello pom.xml in hello [example source code on github](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) has an example of how toodo this.</span></span>

## <a name="next-steps"></a><span data-ttu-id="41d8d-174">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="41d8d-174">Next steps</span></span>
* [<span data-ttu-id="41d8d-175">Hello Java SDK JavaDoc fel</span><span class="sxs-lookup"><span data-stu-id="41d8d-175">Explore JavaDoc for hello Java SDK</span></span>](https://azure.github.io/azure-data-lake-store-java/javadoc/)
* [<span data-ttu-id="41d8d-176">Biztonságos adattárolás a Data Lake Store-ban</span><span class="sxs-lookup"><span data-stu-id="41d8d-176">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="41d8d-177">Az Azure Data Lake Analytics használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="41d8d-177">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="41d8d-178">Az Azure HDInsight használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="41d8d-178">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)

