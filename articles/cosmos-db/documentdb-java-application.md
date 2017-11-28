---
title: "Azure Cosmos DB használatával aaaJava alkalmazásfejlesztési oktatóanyag |} Microsoft Docs"
description: "Ez a Java webalkalmazásokra vonatkozó oktatóanyag bemutatja, hogyan toouse hello Azure Cosmos DB és DocumentDB API toostore hello és elérni az adatokat az Azure websitesban tárolt Java-alkalmazás."
keywords: "Alkalmazásfejlesztés, adatbázis-oktatóanyag, java-alkalmazás, java-webalkalmazás oktatóanyag, documentdb, azure, Microsoft Azure"
services: cosmos-db
documentationcenter: java
author: dennyglee
manager: jhubbard
editor: mimig
ms.assetid: 0867a4a2-4bf5-4898-a1f4-44e3868f8725
ms.service: cosmos-db
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 08/22/2017
ms.author: denlee
ms.openlocfilehash: e073de23beb0037ee1e37b48a69e8fe7cdc3fc1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-java-web-application-using-azure-cosmos-db-and-hello-documentdb-api"></a><span data-ttu-id="ed3e6-104">Azure Cosmos DB és hello DocumentDB API használatával Java-webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ed3e6-104">Build a Java web application using Azure Cosmos DB and hello DocumentDB API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ed3e6-105">.NET</span><span class="sxs-lookup"><span data-stu-id="ed3e6-105">.NET</span></span>](documentdb-dotnet-application.md)
> * [<span data-ttu-id="ed3e6-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="ed3e6-106">Node.js</span></span>](documentdb-nodejs-application.md)
> * [<span data-ttu-id="ed3e6-107">Java</span><span class="sxs-lookup"><span data-stu-id="ed3e6-107">Java</span></span>](documentdb-java-application.md)
> * [<span data-ttu-id="ed3e6-108">Python</span><span class="sxs-lookup"><span data-stu-id="ed3e6-108">Python</span></span>](documentdb-python-application.md)
> 
> 

<span data-ttu-id="ed3e6-109">Ez a Java webalkalmazásokra vonatkozó oktatóanyag bemutatja, hogyan toouse hello [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) szolgáltatás toostore és hozzáférés az Azure App Service Web Apps tárolt Java-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-109">This Java web application tutorial shows you how toouse hello [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service toostore and access data from a Java application hosted on Azure App Service Web Apps.</span></span> <span data-ttu-id="ed3e6-110">A témakörben érintett témák köre:</span><span class="sxs-lookup"><span data-stu-id="ed3e6-110">In this topic, you will learn:</span></span>

* <span data-ttu-id="ed3e6-111">Hogyan toobuild egy alapszintű JavaServer lapok (JSP) alkalmazást az eclipse-ben.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-111">How toobuild a basic JavaServer Pages (JSP) application in Eclipse.</span></span>
* <span data-ttu-id="ed3e6-112">Hogyan toowork hello Azure Cosmos DB a szolgáltatás használatával hello [Azure Cosmos DB Java SDK](https://github.com/Azure/azure-documentdb-java).</span><span class="sxs-lookup"><span data-stu-id="ed3e6-112">How toowork with hello Azure Cosmos DB service using hello [Azure Cosmos DB Java SDK](https://github.com/Azure/azure-documentdb-java).</span></span>

<span data-ttu-id="ed3e6-113">A Java vonatkozó oktatóanyag bemutatja, hogyan toocreate webalapú Feladatkezelő alkalmazást, amely lehetővé teszi, hogy toocreate, lekérése és be van jelölve feladatok el azokat, ahogy az a következő kép hello.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-113">This Java application tutorial shows you how toocreate a web-based task-management application that enables you toocreate, retrieve, and mark tasks as complete, as shown in hello following image.</span></span> <span data-ttu-id="ed3e6-114">Egyes hello feladatokat a teendőlista hello Azure Cosmos DB JSON-dokumentumokként tárolja.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-114">Each of hello tasks in hello ToDo list are stored as JSON documents in Azure Cosmos DB.</span></span>

![Saját teendőlista Java-alkalmazása](./media/documentdb-java-application/image1.png)

> [!TIP]
> <span data-ttu-id="ed3e6-116">Ez az alkalmazásfejlesztési oktatóanyag feltételezi, hogy rendelkezik korábbi tapasztalattal a Java használatát illetően.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-116">This application development tutorial assumes that you have prior experience using Java.</span></span> <span data-ttu-id="ed3e6-117">Ha új tooJava vagy hello [előfeltételt jelentő eszközöket](#Prerequisites), érdemes letöltenie a teljes hello [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) projektet a Githubról, majd lefordítani azt [hello hello végén ez utasításokat a cikk](#GetProject).</span><span class="sxs-lookup"><span data-stu-id="ed3e6-117">If you are new tooJava or hello [prerequisite tools](#Prerequisites), we recommend downloading hello complete [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) project from GitHub and building it using [hello instructions at hello end of this article](#GetProject).</span></span> <span data-ttu-id="ed3e6-118">Ha felépítette, hello cikk toogain insight hello kódja hello projekt környezetében hello tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-118">Once you have it built, you can review hello article toogain insight on hello code in hello context of hello project.</span></span>  
> 
> 

## <span data-ttu-id="ed3e6-119"><a id="Prerequisites"></a>A jelen Java-webalkalmazásokra vonatkozó oktatóanyag előfeltételei</span><span class="sxs-lookup"><span data-stu-id="ed3e6-119"><a id="Prerequisites"></a>Prerequisites for this Java web application tutorial</span></span>
<span data-ttu-id="ed3e6-120">Az alkalmazásfejlesztési oktatóanyag elkezdéséhez hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="ed3e6-120">Before you begin this application development tutorial, you must have hello following:</span></span>

* <span data-ttu-id="ed3e6-121">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-121">An active Azure account.</span></span> <span data-ttu-id="ed3e6-122">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-122">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="ed3e6-123">További részletekért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/)</span><span class="sxs-lookup"><span data-stu-id="ed3e6-123">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/)</span></span>

    <span data-ttu-id="ed3e6-124">VAGY</span><span class="sxs-lookup"><span data-stu-id="ed3e6-124">OR</span></span>

    <span data-ttu-id="ed3e6-125">Egy helyi telepítését teszi hello [Azure Cosmos DB emulátor](local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="ed3e6-125">A local installation of hello [Azure Cosmos DB Emulator](local-emulator.md).</span></span>
* <span data-ttu-id="ed3e6-126">[Java fejlesztői készlet (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="ed3e6-126">[Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span>
* [<span data-ttu-id="ed3e6-127">Eclipse IDE for Java EE Developers.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-127">Eclipse IDE for Java EE Developers.</span></span>](http://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunasr1)
* [<span data-ttu-id="ed3e6-128">Egy Azure Java-futtatókörnyezettel (pl. Tomcat vagy Jetty) engedélyezve van a webhelyen.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-128">An Azure Web Site with a Java runtime environment (e.g. Tomcat or Jetty) enabled.</span></span>](../app-service-web/web-sites-java-get-started.md)

<span data-ttu-id="ed3e6-129">Telepítése hello az eszközök először, coreservlets.com webhelyen hello telepítési folyamat hello gyors üzembe helyezés szakaszában a [oktatóanyag: TomCat7 telepítése és használata a Eclipse](http://www.coreservlets.com/Apache-Tomcat-Tutorial/tomcat-7-with-eclipse.html) cikk.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-129">If you're installing these tools for hello first time, coreservlets.com provides a walk-through of hello installation process in hello Quick Start section of their [Tutorial: Installing TomCat7 and Using it with Eclipse](http://www.coreservlets.com/Apache-Tomcat-Tutorial/tomcat-7-with-eclipse.html) article.</span></span>

## <span data-ttu-id="ed3e6-130"><a id="CreateDB"></a>1. lépés: Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="ed3e6-130"><a id="CreateDB"></a>Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="ed3e6-131">Először hozzon létre egy Azure Cosmos DB-fiókot.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-131">Let's start by creating an Azure Cosmos DB account.</span></span> <span data-ttu-id="ed3e6-132">Ha már rendelkezik fiókkal, vagy használatakor hello Azure Cosmos DB emulátor ehhez az oktatóanyaghoz, ugorjon túl[2. lépés: hello Java JSP-alkalmazás létrehozása](#CreateJSP).</span><span class="sxs-lookup"><span data-stu-id="ed3e6-132">If you already have an account or if you are using hello Azure Cosmos DB Emulator for this tutorial, you can skip too[Step 2: Create hello Java JSP application](#CreateJSP).</span></span>

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

## <span data-ttu-id="ed3e6-133"><a id="CreateJSP"></a>2. lépés: Hello Java JSP-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ed3e6-133"><a id="CreateJSP"></a>Step 2: Create hello Java JSP application</span></span>
<span data-ttu-id="ed3e6-134">toocreate hello JSP-alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="ed3e6-134">toocreate hello JSP application:</span></span>

1. <span data-ttu-id="ed3e6-135">Először is hozzon létre egy Java-projektet.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-135">First, we’ll start off by creating a Java project.</span></span> <span data-ttu-id="ed3e6-136">Indítsa el az Eclipse-t, kattintson a **File** (Fájl), **New** (Új), majd a **Dynamic Web Projekt** (Dinamikus webes projekt) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-136">Start Eclipse, then click **File**, click **New**, and then click **Dynamic Web Project**.</span></span> <span data-ttu-id="ed3e6-137">Ha nem lát **dinamikus webes projekt** szerepel a listában az elérhető projektek, a következő hello: kattintson a **fájl**, kattintson a **új**, kattintson a **projekt**..., bontsa ki a **webes**, kattintson a **dinamikus webes projekt**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-137">If you don’t see **Dynamic Web Project** listed as an available project, do hello following: click **File**, click **New**, click **Project**…, expand **Web**, click **Dynamic Web Project**, and click **Next**.</span></span>
   
    ![JSP Java-alkalmazások fejlesztése](./media/documentdb-java-application/image10.png)
2. <span data-ttu-id="ed3e6-139">Adja meg a projekt nevét a hello **projektnevet** mezőbe, és a hello **Target Runtime** legördülő menüben válassza ki (pl. Apache Tomcat v7.0) értéket nem kötelező, és kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-139">Enter a project name in hello **Project name** box, and in hello **Target Runtime** drop-down menu, optionally select a value (e.g. Apache Tomcat v7.0), and then click **Finish**.</span></span> <span data-ttu-id="ed3e6-140">A tervezett futásidőt kiválasztása lehetővé teszi, hogy Ön toorun a projekten helyileg eclipse-ben.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-140">Selecting a target runtime enables you toorun your project locally through Eclipse.</span></span>
3. <span data-ttu-id="ed3e6-141">Az eclipse-ben hello Project Explorer nézet, bontsa ki a projektet.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-141">In Eclipse, in hello Project Explorer view, expand your project.</span></span> <span data-ttu-id="ed3e6-142">Kattintson a jobb gombbal a **WebContent** (Webes tartalom), majd a **New** (Új) elemre, és végül a **JSP File** (JSP-fájl) elemre.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-142">Right-click **WebContent**, click **New**, and then click **JSP File**.</span></span>
4. <span data-ttu-id="ed3e6-143">A hello **új JSP-fájl** párbeszédpanelen nevű hello fájl **index.jsp**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-143">In hello **New JSP File** dialog box, name hello file **index.jsp**.</span></span> <span data-ttu-id="ed3e6-144">Hello fölérendelt mappája, tartsa **WebContent**, ahogy a következő ábra hello, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-144">Keep hello parent folder as **WebContent**, as shown in hello following illustration, and then click **Next**.</span></span>
   
    ![Új JSP-fájl létrehozása – Java-alkalmazásokra vonatkozó oktatóanyag](./media/documentdb-java-application/image11.png)
5. <span data-ttu-id="ed3e6-146">A hello **JSP-sablon kiválasztása** párbeszédpanelen hello céllal történik, az oktatóanyag válassza **új JSP-fájl (html)**, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-146">In hello **Select JSP Template** dialog box, for hello purpose of this tutorial select **New JSP File (html)**, and then click **Finish**.</span></span>
6. <span data-ttu-id="ed3e6-147">Hello index.jsp fájl megnyitásakor az eclipse-ben, vegye fel a szöveges toodisplay **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="ed3e6-147">When hello index.jsp file opens in Eclipse, add text toodisplay **Hello World!**</span></span> <span data-ttu-id="ed3e6-148">hello meglévő belül <body> elemet.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-148">within hello existing <body> element.</span></span> <span data-ttu-id="ed3e6-149">A frissített <body> tartalom a következő kód hello hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="ed3e6-149">Your updated <body> content should look like hello following code:</span></span>
   
        <body>
            <% out.println("Hello World!"); %>
        </body>
7. <span data-ttu-id="ed3e6-150">Mentse a hello index.jsp fájlt.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-150">Save hello index.jsp file.</span></span>
8. <span data-ttu-id="ed3e6-151">Ha megadta a tervezett futásidőt a 2. lépésben, **projekt** , majd **futtatása** toorun a JSP-alkalmazás helyileg:</span><span class="sxs-lookup"><span data-stu-id="ed3e6-151">If you set a target runtime in step 2, you can click **Project** and then **Run** toorun your JSP application locally:</span></span>
   
    ![Hello World – Java-alkalmazásokra vonatkozó oktatóanyag](./media/documentdb-java-application/image12.png)

## <span data-ttu-id="ed3e6-153"><a id="InstallSDK"></a>3. lépés: Hello DocumentDB Java SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="ed3e6-153"><a id="InstallSDK"></a>Step 3: Install hello DocumentDB Java SDK</span></span>
<span data-ttu-id="ed3e6-154">hello legegyszerűbb módja toopull hello DocumentDB Java SDK-t és annak függőségeit van keresztül [Apache Maven](http://maven.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="ed3e6-154">hello easiest way toopull in hello DocumentDB Java SDK and its dependencies is through [Apache Maven](http://maven.apache.org/).</span></span>

<span data-ttu-id="ed3e6-155">toodo, szüksége lesz tooconvert a projekt tooa maven project hello lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="ed3e6-155">toodo this, you will need tooconvert your project tooa maven project by completing hello following steps:</span></span>

1. <span data-ttu-id="ed3e6-156">Kattintson a jobb gombbal a projektre a Project Explorer hello parancsra **konfigurálása**, kattintson a **tooMaven projekt átalakítása**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-156">Right-click your project in hello Project Explorer, click **Configure**, click **Convert tooMaven Project**.</span></span>
2. <span data-ttu-id="ed3e6-157">A hello **új POM létrehozása** ablakot, fogadja el hello alapértelmezett beállításokat, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-157">In hello **Create new POM** window, accept hello defaults and click **Finish**.</span></span>
3. <span data-ttu-id="ed3e6-158">A **Project Explorer**, nyissa meg hello pom.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-158">In **Project Explorer**, open hello pom.xml file.</span></span>
4. <span data-ttu-id="ed3e6-159">A hello **függőségek** lap hello **függőségek** ablaktáblán kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-159">On hello **Dependencies** tab, in hello **Dependencies** pane, click **Add**.</span></span>
5. <span data-ttu-id="ed3e6-160">A hello **függőség kiválasztása** ablakban, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="ed3e6-160">In hello **Select Dependency** window, do hello following:</span></span>
   
   * <span data-ttu-id="ed3e6-161">A hello **csoportazonosító** mezőbe írja be a következőt: com.microsoft.azure.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-161">In hello **Group Id** box, enter com.microsoft.azure.</span></span>
   * <span data-ttu-id="ed3e6-162">A hello **összetevő-azonosító** mezőbe írja be az azure-documentdb.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-162">In hello **Artifact Id** box, enter azure-documentdb.</span></span>
   * <span data-ttu-id="ed3e6-163">A hello **verzió** 1.5.1 adja meg.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-163">In hello **Version** box, enter 1.5.1.</span></span>
     
   ![A DocumentDB Java Application SDK telepítése](./media/documentdb-java-application/image13.png)
     
   * <span data-ttu-id="ed3e6-165">Vagy hello függőség XML hozzáadása csoporthoz és összetevő-azonosító közvetlenül toohello pom.xml fájlhoz egy szövegszerkesztő:</span><span class="sxs-lookup"><span data-stu-id="ed3e6-165">Or add hello dependency XML for Group Id and Artifact Id directly toohello pom.xml via a text editor:</span></span>
     
        <span data-ttu-id="ed3e6-166"><dependency><groupId>következőt: com.microsoft.azure</groupId> <artifactId>azure-documentdb</artifactId> <version>1.9.1</version></dependency></span><span class="sxs-lookup"><span data-stu-id="ed3e6-166"><dependency> <groupId>com.microsoft.azure</groupId> <artifactId>azure-documentdb</artifactId> <version>1.9.1</version> </dependency></span></span>
6. <span data-ttu-id="ed3e6-167">Kattintson a **OK** és után a Maven feltelepíti a DocumentDB Java SDK hello.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-167">Click **OK** and Maven will install hello DocumentDB Java SDK.</span></span>
7. <span data-ttu-id="ed3e6-168">Mentse a hello pom.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-168">Save hello pom.xml file.</span></span>

## <span data-ttu-id="ed3e6-169"><a id="UseService"></a>4. lépés: Hello Azure Cosmos DB szolgáltatás használata Java-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="ed3e6-169"><a id="UseService"></a>Step 4: Using hello Azure Cosmos DB service in a Java application</span></span>
1. <span data-ttu-id="ed3e6-170">Először is határozza meg hello TodoItem objektum TodoItem.java:</span><span class="sxs-lookup"><span data-stu-id="ed3e6-170">First, let's define hello TodoItem object in TodoItem.java:</span></span>
   
        @Data
        @Builder
        public class TodoItem {
            private String category;
            private boolean complete;
            private String id;
            private String name;
        }
   
    <span data-ttu-id="ed3e6-171">A projekt használatával hoztuk [Project Lombok](http://projectlombok.org/) toogenerate hello konstruktor, beolvasóinak, Setter elemek és a felépítőt.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-171">In this project, we are using [Project Lombok](http://projectlombok.org/) toogenerate hello constructor, getters, setters, and a builder.</span></span> <span data-ttu-id="ed3e6-172">Azt is megteheti, ezzel a kóddal manuálisan írási, vagy rendelkezik hello IDE hozható létre.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-172">Alternatively, you can write this code manually or have hello IDE generate it.</span></span>
2. <span data-ttu-id="ed3e6-173">tooinvoke hello Azure Cosmos DB szolgáltatás, létre kell hozni egy új **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-173">tooinvoke hello Azure Cosmos DB service, you must instantiate a new **DocumentClient**.</span></span> <span data-ttu-id="ed3e6-174">Ez általában legjobb tooreuse hello **DocumentClient** - ahelyett, hogy minden későbbi kérés esetén új ügyfelet létre.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-174">In general, it is best tooreuse hello **DocumentClient** - rather than construct a new client for each subsequent request.</span></span> <span data-ttu-id="ed3e6-175">Hello ügyfél használatával hello ügyfél újrafelhasználásához egy **DocumentClientFactory**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-175">We can reuse hello client by wrapping hello client in a **DocumentClientFactory**.</span></span> <span data-ttu-id="ed3e6-176">DocumentClientFactory.java, toopaste hello URI és PRIMARY KEY érték tooyour vágólapra mentett kell [1. lépés](#CreateDB).</span><span class="sxs-lookup"><span data-stu-id="ed3e6-176">In DocumentClientFactory.java, you need toopaste hello URI and PRIMARY KEY value you saved tooyour clipboard in [step 1](#CreateDB).</span></span> <span data-ttu-id="ed3e6-177">Cserélje ki a [YOUR\_ENDPOINT\_HERE] részt az URI-re, a [YOUR\_KEY\_HERE] részt pedig az elsődleges kulcsra.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-177">Replace [YOUR\_ENDPOINT\_HERE] with your URI and replace [YOUR\_KEY\_HERE] with your PRIMARY KEY.</span></span>
   
        private static final String HOST = "[YOUR_ENDPOINT_HERE]";
        private static final String MASTER_KEY = "[YOUR_KEY_HERE]";
   
        private static DocumentClient documentClient = new DocumentClient(HOST, MASTER_KEY,
                        ConnectionPolicy.GetDefault(), ConsistencyLevel.Session);
   
        public static DocumentClient getDocumentClient() {
            return documentClient;
        }
3. <span data-ttu-id="ed3e6-178">Most hozzon létre egy Data Access objektum (DAO) tooabstract a ToDo elemeket tooAzure Cosmos DB megőrzése.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-178">Now let's create a Data Access Object (DAO) tooabstract persisting our ToDo items tooAzure Cosmos DB.</span></span>
   
    <span data-ttu-id="ed3e6-179">A rendezés toosave ToDo elemeket tooa gyűjtemény, hello ügyfélnek kell tooknow melyik adatbázis és gyűjtemény toopersist túl (által hivatkozott erre az önhivatkozások).</span><span class="sxs-lookup"><span data-stu-id="ed3e6-179">In order toosave ToDo items tooa collection, hello client needs tooknow which database and collection toopersist too(as referenced by self-links).</span></span> <span data-ttu-id="ed3e6-180">Ez általában legjobb toocache hello adatbázist és a gyűjteményt, ha lehetséges tooavoid fölösleges üzenetváltás toohello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-180">In general, it is best toocache hello database and collection when possible tooavoid additional round-trips toohello database.</span></span>
   
    <span data-ttu-id="ed3e6-181">hello alábbi kód bemutatja, hogyan tooretrieve az adatbázist és a gyűjteményt, ha létezik, vagy hozzon létre egy újat, ha még nem létezik:</span><span class="sxs-lookup"><span data-stu-id="ed3e6-181">hello following code illustrates how tooretrieve our database and collection, if it exists, or create a new one if it doesn't exist:</span></span>
   
        public class DocDbDao implements TodoDao {
            // hello name of our database.
            private static final String DATABASE_ID = "TodoDB";
   
            // hello name of our collection.
            private static final String COLLECTION_ID = "TodoCollection";
   
            // hello Azure Cosmos DB Client
            private static DocumentClient documentClient = DocumentClientFactory
                    .getDocumentClient();
   
            // Cache for hello database object, so we don't have tooquery for it to
            // retrieve self links.
            private static Database databaseCache;
   
            // Cache for hello collection object, so we don't have tooquery for it to
            // retrieve self links.
            private static DocumentCollection collectionCache;
   
            private Database getTodoDatabase() {
                if (databaseCache == null) {
                    // Get hello database if it exists
                    List<Database> databaseList = documentClient
                            .queryDatabases(
                                    "SELECT * FROM root r WHERE r.id='" + DATABASE_ID
                                            + "'", null).getQueryIterable().toList();
   
                    if (databaseList.size() > 0) {
                        // Cache hello database object so we won't have tooquery for it
                        // later tooretrieve hello selfLink.
                        databaseCache = databaseList.get(0);
                    } else {
                        // Create hello database if it doesn't exist.
                        try {
                            Database databaseDefinition = new Database();
                            databaseDefinition.setId(DATABASE_ID);
   
                            databaseCache = documentClient.createDatabase(
                                    databaseDefinition, null).getResource();
                        } catch (DocumentClientException e) {
                            // TODO: Something has gone terribly wrong - hello app wasn't
                            // able tooquery or create hello collection.
                            // Verify your connection, endpoint, and key.
                            e.printStackTrace();
                        }
                    }
                }
   
                return databaseCache;
            }
   
            private DocumentCollection getTodoCollection() {
                if (collectionCache == null) {
                    // Get hello collection if it exists.
                    List<DocumentCollection> collectionList = documentClient
                            .queryCollections(
                                    getTodoDatabase().getSelfLink(),
                                    "SELECT * FROM root r WHERE r.id='" + COLLECTION_ID
                                            + "'", null).getQueryIterable().toList();
   
                    if (collectionList.size() > 0) {
                        // Cache hello collection object so we won't have tooquery for it
                        // later tooretrieve hello selfLink.
                        collectionCache = collectionList.get(0);
                    } else {
                        // Create hello collection if it doesn't exist.
                        try {
                            DocumentCollection collectionDefinition = new DocumentCollection();
                            collectionDefinition.setId(COLLECTION_ID);
   
                            collectionCache = documentClient.createCollection(
                                    getTodoDatabase().getSelfLink(),
                                    collectionDefinition, null).getResource();
                        } catch (DocumentClientException e) {
                            // TODO: Something has gone terribly wrong - hello app wasn't
                            // able tooquery or create hello collection.
                            // Verify your connection, endpoint, and key.
                            e.printStackTrace();
                        }
                    }
                }
   
                return collectionCache;
            }
        }
4. <span data-ttu-id="ed3e6-182">következő lépés hello toowrite van néhány kódot toopersist hello TodoItems toohello gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-182">hello next step is toowrite some code toopersist hello TodoItems in toohello collection.</span></span> <span data-ttu-id="ed3e6-183">A jelen példában használjuk [Gson](https://code.google.com/p/google-gson/) tooserialize és deszerializálni a TodoItem teendő Pojo objektumait (POJOs) tooJSON dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-183">In this example, we will use [Gson](https://code.google.com/p/google-gson/) tooserialize and de-serialize TodoItem Plain Old Java Objects (POJOs) tooJSON documents.</span></span>
   
        // We'll use Gson for POJO <=> JSON serialization for this example.
        private static Gson gson = new Gson();
   
        @Override
        public TodoItem createTodoItem(TodoItem todoItem) {
            // Serialize hello TodoItem as a JSON Document.
            Document todoItemDocument = new Document(gson.toJson(todoItem));
   
            // Annotate hello document as a TodoItem for retrieval (so that we can
            // store multiple entity types in hello collection).
            todoItemDocument.set("entityType", "todoItem");
   
            try {
                // Persist hello document using hello DocumentClient.
                todoItemDocument = documentClient.createDocument(
                        getTodoCollection().getSelfLink(), todoItemDocument, null,
                        false).getResource();
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return null;
            }
   
            return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
        }
5. <span data-ttu-id="ed3e6-184">Csakúgy, mint az Azure Cosmos DB-adatbázisok és -gyűjtemények esetén, a dokumentumok önhivatkozásokkal is hivatkoznak magukra.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-184">Like Azure Cosmos DB databases and collections, documents are also referenced by self-links.</span></span> <span data-ttu-id="ed3e6-185">hello következő segítő függvény segítségével velünk lekérése dokumentumok egy másik attribútum (pl. "id"), hanem önhivatkozást:</span><span class="sxs-lookup"><span data-stu-id="ed3e6-185">hello following helper function lets us retrieve documents by another attribute (e.g. "id") rather than self-link:</span></span>
   
        private Document getDocumentById(String id) {
            // Retrieve hello document using hello DocumentClient.
            List<Document> documentList = documentClient
                    .queryDocuments(getTodoCollection().getSelfLink(),
                            "SELECT * FROM root r WHERE r.id='" + id + "'", null)
                    .getQueryIterable().toList();
   
            if (documentList.size() > 0) {
                return documentList.get(0);
            } else {
                return null;
            }
        }
6. <span data-ttu-id="ed3e6-186">A Microsoft hello segédmetódus használja az 5. lépés tooretrieve egy teendő JSON-dokumentum-azonosító szerint, és tooa pojo-vá deszerializálni:</span><span class="sxs-lookup"><span data-stu-id="ed3e6-186">We can use hello helper method in step 5 tooretrieve a TodoItem JSON document by id and then deserialize it tooa POJO:</span></span>
   
        @Override
        public TodoItem readTodoItem(String id) {
            // Retrieve hello document by id using our helper method.
            Document todoItemDocument = getDocumentById(id);
   
            if (todoItemDocument != null) {
                // De-serialize hello document in tooa TodoItem.
                return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
            } else {
                return null;
            }
        }
7. <span data-ttu-id="ed3e6-187">Hello DocumentClient tooget azt is használható, egy gyűjtemény vagy a DocumentDB SQL használatával TodoItems listáját:</span><span class="sxs-lookup"><span data-stu-id="ed3e6-187">We can also use hello DocumentClient tooget a collection or list of TodoItems using DocumentDB SQL:</span></span>
   
        @Override
        public List<TodoItem> readTodoItems() {
            List<TodoItem> todoItems = new ArrayList<TodoItem>();
   
            // Retrieve hello TodoItem documents
            List<Document> documentList = documentClient
                    .queryDocuments(getTodoCollection().getSelfLink(),
                            "SELECT * FROM root r WHERE r.entityType = 'todoItem'",
                            null).getQueryIterable().toList();
   
            // De-serialize hello documents in tooTodoItems.
            for (Document todoItemDocument : documentList) {
                todoItems.add(gson.fromJson(todoItemDocument.toString(),
                        TodoItem.class));
            }
   
            return todoItems;
        }
8. <span data-ttu-id="ed3e6-188">Nincsenek számos módon tooupdate hello DocumentClient rendelkező dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-188">There are many ways tooupdate a document with hello DocumentClient.</span></span> <span data-ttu-id="ed3e6-189">A teendőlista alkalmazásában szeretnénk tudni tootoggle toobe e beállíthatnánk.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-189">In our Todo list application, we want toobe able tootoggle whether a TodoItem is complete.</span></span> <span data-ttu-id="ed3e6-190">Ez hello hello dokumentum "kész" attribútumának frissítésével érhető el:</span><span class="sxs-lookup"><span data-stu-id="ed3e6-190">This can be achieved by updating hello "complete" attribute within hello document:</span></span>
   
        @Override
        public TodoItem updateTodoItem(String id, boolean isComplete) {
            // Retrieve hello document from hello database
            Document todoItemDocument = getDocumentById(id);
   
            // You can update hello document as a JSON document directly.
            // For more complex operations - you could de-serialize hello document in
            // tooa POJO, update hello POJO, and then re-serialize hello POJO back in to
            // a document.
            todoItemDocument.set("complete", isComplete);
   
            try {
                // Persist/replace hello updated document.
                todoItemDocument = documentClient.replaceDocument(todoItemDocument,
                        null).getResource();
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return null;
            }
   
            return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
        }
9. <span data-ttu-id="ed3e6-191">Végül azt szeretnénk hello képességét toodelete Beállíthatnánk listájából.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-191">Finally, we want hello ability toodelete a TodoItem from our list.</span></span> <span data-ttu-id="ed3e6-192">toodo, korábban megírt segédmetódus hello használhatjuk tooretrieve hello önhivatkozást, és ezután adja meg az ügyfél toodelete hello azt:</span><span class="sxs-lookup"><span data-stu-id="ed3e6-192">toodo this, we can use hello helper method we wrote earlier tooretrieve hello self-link and then tell hello client toodelete it:</span></span>
   
        @Override
        public boolean deleteTodoItem(String id) {
            // Azure Cosmos DB refers toodocuments by self link rather than id.
   
            // Query for hello document tooretrieve hello self link.
            Document todoItemDocument = getDocumentById(id);
   
            try {
                // Delete hello document by self link.
                documentClient.deleteDocument(todoItemDocument.getSelfLink(), null);
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return false;
            }
   
            return true;
        }

## <span data-ttu-id="ed3e6-193"><a id="Wire"></a>5. lépés: A Java-alkalmazásfejlesztési projekt hello hello részeinek együtt huzalozási</span><span class="sxs-lookup"><span data-stu-id="ed3e6-193"><a id="Wire"></a>Step 5: Wiring hello rest of hello of Java application development project together</span></span>
<span data-ttu-id="ed3e6-194">Most, hogy befejeztük hello visszatöltött bits - összes marad toobuild gyors felhasználói felületet, és hozzá kell fűznie azt tooour DAO fel.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-194">Now that we've finished hello fun bits - all that's left is toobuild a quick user interface and wire it up tooour DAO.</span></span>

1. <span data-ttu-id="ed3e6-195">Első lépésként kezdjük egy tartományvezérlő toocall DAO felépítése:</span><span class="sxs-lookup"><span data-stu-id="ed3e6-195">First, let's start with building a controller toocall our DAO:</span></span>
   
        public class TodoItemController {
            public static TodoItemController getInstance() {
                if (todoItemController == null) {
                    todoItemController = new TodoItemController(TodoDaoFactory.getDao());
                }
                return todoItemController;
            }
   
            private static TodoItemController todoItemController;
   
            private final TodoDao todoDao;
   
            TodoItemController(TodoDao todoDao) {
                this.todoDao = todoDao;
            }
   
            public TodoItem createTodoItem(@NonNull String name,
                    @NonNull String category, boolean isComplete) {
                TodoItem todoItem = TodoItem.builder().name(name).category(category)
                        .complete(isComplete).build();
                return todoDao.createTodoItem(todoItem);
            }
   
            public boolean deleteTodoItem(@NonNull String id) {
                return todoDao.deleteTodoItem(id);
            }
   
            public TodoItem getTodoItemById(@NonNull String id) {
                return todoDao.readTodoItem(id);
            }
   
            public List<TodoItem> getTodoItems() {
                return todoDao.readTodoItems();
            }
   
            public TodoItem updateTodoItem(@NonNull String id, boolean isComplete) {
                return todoDao.updateTodoItem(id, isComplete);
            }
        }
   
    <span data-ttu-id="ed3e6-196">Egy összetettebb alkalmazásban hello vezérlő bonyolult üzleti logikát is hello DAO mellett.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-196">In a more complex application, hello controller may house complicated business logic on top of hello DAO.</span></span>
2. <span data-ttu-id="ed3e6-197">Ezután létrehozunk egy servlet tooroute HTTP kérelmeket toohello tartományvezérlő:</span><span class="sxs-lookup"><span data-stu-id="ed3e6-197">Next, we'll create a servlet tooroute HTTP requests toohello controller:</span></span>
   
        public class TodoServlet extends HttpServlet {
            // API Keys
            public static final String API_METHOD = "method";
   
            // API Methods
            public static final String CREATE_TODO_ITEM = "createTodoItem";
            public static final String GET_TODO_ITEMS = "getTodoItems";
            public static final String UPDATE_TODO_ITEM = "updateTodoItem";
   
            // API Parameters
            public static final String TODO_ITEM_ID = "todoItemId";
            public static final String TODO_ITEM_NAME = "todoItemName";
            public static final String TODO_ITEM_CATEGORY = "todoItemCategory";
            public static final String TODO_ITEM_COMPLETE = "todoItemComplete";
   
            public static final String MESSAGE_ERROR_INVALID_METHOD = "{'error': 'Invalid method'}";
   
            private static final long serialVersionUID = 1L;
            private static final Gson gson = new Gson();
   
            @Override
            protected void doGet(HttpServletRequest request,
                    HttpServletResponse response) throws ServletException, IOException {
   
                String apiResponse = MESSAGE_ERROR_INVALID_METHOD;
   
                TodoItemController todoItemController = TodoItemController
                        .getInstance();
   
                String id = request.getParameter(TODO_ITEM_ID);
                String name = request.getParameter(TODO_ITEM_NAME);
                String category = request.getParameter(TODO_ITEM_CATEGORY);
                boolean isComplete = StringUtils.equalsIgnoreCase("true",
                        request.getParameter(TODO_ITEM_COMPLETE)) ? true : false;
   
                switch (request.getParameter(API_METHOD)) {
                case CREATE_TODO_ITEM:
                    apiResponse = gson.toJson(todoItemController.createTodoItem(name,
                            category, isComplete));
                    break;
                case GET_TODO_ITEMS:
                    apiResponse = gson.toJson(todoItemController.getTodoItems());
                    break;
                case UPDATE_TODO_ITEM:
                    apiResponse = gson.toJson(todoItemController.updateTodoItem(id,
                            isComplete));
                    break;
                default:
                    break;
                }
   
                response.getWriter().println(apiResponse);
            }
   
            @Override
            protected void doPost(HttpServletRequest request,
                    HttpServletResponse response) throws ServletException, IOException {
                doGet(request, response);
            }
        }
3. <span data-ttu-id="ed3e6-198">Szükség lesz egy webes felhasználói felület toodisplay toohello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-198">We'll need a web user interface toodisplay toohello user.</span></span> <span data-ttu-id="ed3e6-199">Ideje újra lefuttatni hello index.jsp korábban létrehozott:</span><span class="sxs-lookup"><span data-stu-id="ed3e6-199">Let's re-write hello index.jsp we created earlier:</span></span>
    ```html
        <html>
        <head>
          <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
          <meta http-equiv="X-UA-Compatible" content="IE=edge;" />
          <title>Azure Cosmos DB Java Sample</title>
   
          <!-- Bootstrap -->
          <link href="//ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css" rel="stylesheet">
   
          <style>
            /* Add padding toobody for fixed nav bar */
            body {
              padding-top: 50px;
            }
          </style>
        </head>
        <body>
          <!-- Nav Bar -->
          <div class="navbar navbar-inverse navbar-fixed-top" role="navigation">
            <div class="container">
              <div class="navbar-header">
                <a class="navbar-brand" href="#">My Tasks</a>
              </div>
            </div>
          </div>
   
          <!-- Body -->
          <div class="container">
            <h1>My ToDo List</h1>
   
            <hr/>
   
            <!-- hello ToDo List -->
            <div class = "todoList">
              <table class="table table-bordered table-striped" id="todoItems">
                <thead>
                  <tr>
                    <th>Name</th>
                    <th>Category</th>
                    <th>Complete</th>
                  </tr>
                </thead>
                <tbody>
                </tbody>
              </table>
   
              <!-- Update Button -->
              <div class="todoUpdatePanel">
                <form class="form-horizontal" role="form">
                  <button type="button" class="btn btn-primary">Update Tasks</button>
                </form>
              </div>
   
            </div>
   
            <hr/>
   
            <!-- Item Input Form -->
            <div class="todoForm">
              <form class="form-horizontal" role="form">
                <div class="form-group">
                  <label for="inputItemName" class="col-sm-2">Task Name</label>
                  <div class="col-sm-10">
                    <input type="text" class="form-control" id="inputItemName" placeholder="Enter name">
                  </div>
                </div>
   
                <div class="form-group">
                  <label for="inputItemCategory" class="col-sm-2">Task Category</label>
                  <div class="col-sm-10">
                    <input type="text" class="form-control" id="inputItemCategory" placeholder="Enter category">
                  </div>
                </div>
   
                <button type="button" class="btn btn-primary">Add Task</button>
              </form>
            </div>
   
          </div>
   
          <!-- Placed at hello end of hello document so hello pages load faster -->
          <script src="//ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js"></script>
          <script src="//ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js"></script>
          <script src="assets/todo.js"></script>
        </body>
        </html>
    ```
4. <span data-ttu-id="ed3e6-200">És végezetül ügyféloldali JavaScript tootie hello webes felhasználói felület egyes írási és a servlet együtt hello:</span><span class="sxs-lookup"><span data-stu-id="ed3e6-200">And finally, write some client-side JavaScript tootie hello web user interface and hello servlet together:</span></span>
   
        var todoApp = {
          /*
           * API methods toocall Java backend.
           */
          apiEndpoint: "api",
   
          createTodoItem: function(name, category, isComplete) {
            $.post(todoApp.apiEndpoint, {
                "method": "createTodoItem",
                "todoItemName": name,
                "todoItemCategory": category,
                "todoItemComplete": isComplete
              },
              function(data) {
                var todoItem = data;
                todoApp.addTodoItemToTable(todoItem.id, todoItem.name, todoItem.category, todoItem.complete);
              },
              "json");
          },
   
          getTodoItems: function() {
            $.post(todoApp.apiEndpoint, {
                "method": "getTodoItems"
              },
              function(data) {
                var todoItemArr = data;
                $.each(todoItemArr, function(index, value) {
                  todoApp.addTodoItemToTable(value.id, value.name, value.category, value.complete);
                });
              },
              "json");
          },
   
          updateTodoItem: function(id, isComplete) {
            $.post(todoApp.apiEndpoint, {
                "method": "updateTodoItem",
                "todoItemId": id,
                "todoItemComplete": isComplete
              },
              function(data) {},
              "json");
          },
   
          /*
           * UI Methods
           */
          addTodoItemToTable: function(id, name, category, isComplete) {
            var rowColor = isComplete ? "active" : "warning";
   
            todoApp.ui_table().append($("<tr>")
              .append($("<td>").text(name))
              .append($("<td>").text(category))
              .append($("<td>")
                .append($("<input>")
                  .attr("type", "checkbox")
                  .attr("id", id)
                  .attr("checked", isComplete)
                  .attr("class", "isComplete")
                ))
              .addClass(rowColor)
            );
          },
   
          /*
           * UI Bindings
           */
          bindCreateButton: function() {
            todoApp.ui_createButton().click(function() {
              todoApp.createTodoItem(todoApp.ui_createNameInput().val(), todoApp.ui_createCategoryInput().val(), false);
              todoApp.ui_createNameInput().val("");
              todoApp.ui_createCategoryInput().val("");
            });
          },
   
          bindUpdateButton: function() {
            todoApp.ui_updateButton().click(function() {
              // Disable button temporarily.
              var myButton = $(this);
              var originalText = myButton.text();
              $(this).text("Updating...");
              $(this).prop("disabled", true);
   
              // Call api tooupdate todo items.
              $.each(todoApp.ui_updateId(), function(index, value) {
                todoApp.updateTodoItem(value.name, value.value);
                $(value).remove();
              });
   
              // Re-enable button.
              setTimeout(function() {
                myButton.prop("disabled", false);
                myButton.text(originalText);
              }, 500);
            });
          },
   
          bindUpdateCheckboxes: function() {
            todoApp.ui_table().on("click", ".isComplete", function(event) {
              var checkboxElement = $(event.currentTarget);
              var rowElement = $(event.currentTarget).parents('tr');
              var id = checkboxElement.attr('id');
              var isComplete = checkboxElement.is(':checked');
   
              // Toggle table row color
              if (isComplete) {
                rowElement.addClass("active");
                rowElement.removeClass("warning");
              } else {
                rowElement.removeClass("active");
                rowElement.addClass("warning");
              }
   
              // Update hidden inputs for update panel.
              todoApp.ui_updateForm().children("input[name='" + id + "']").remove();
   
              todoApp.ui_updateForm().append($("<input>")
                .attr("type", "hidden")
                .attr("class", "updateComplete")
                .attr("name", id)
                .attr("value", isComplete));
   
            });
          },
   
          /*
           * UI Elements
           */
          ui_createNameInput: function() {
            return $(".todoForm #inputItemName");
          },
   
          ui_createCategoryInput: function() {
            return $(".todoForm #inputItemCategory");
          },
   
          ui_createButton: function() {
            return $(".todoForm button");
          },
   
          ui_table: function() {
            return $(".todoList table tbody");
          },
   
          ui_updateButton: function() {
            return $(".todoUpdatePanel button");
          },
   
          ui_updateForm: function() {
            return $(".todoUpdatePanel form");
          },
   
          ui_updateId: function() {
            return $(".todoUpdatePanel .updateComplete");
          },
   
          /*
           * Install hello TodoApp
           */
          install: function() {
            todoApp.bindCreateButton();
            todoApp.bindUpdateButton();
            todoApp.bindUpdateCheckboxes();
   
            todoApp.getTodoItems();
          }
        };
   
        $(document).ready(function() {
          todoApp.install();
        });
5. <span data-ttu-id="ed3e6-201">Nagyszerű!</span><span class="sxs-lookup"><span data-stu-id="ed3e6-201">Awesome!</span></span> <span data-ttu-id="ed3e6-202">Most, hogy marad az összes alkalmazás, amely tootest hello.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-202">Now all that's left is tootest hello application.</span></span> <span data-ttu-id="ed3e6-203">Hello alkalmazás helyileg történő futtatása, és adja hozzá az egyes Todo elemeket kitöltése hello elemek nevét és kategóriáját, majd kattintson **feladat hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-203">Run hello application locally, and add some Todo items by filling in hello item name and category and clicking **Add Task**.</span></span>
6. <span data-ttu-id="ed3e6-204">Hello elem jelenik meg, miután-e már azt való átváltással hello jelölőnégyzetet, majd frissítheti **feladatok frissítése**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-204">Once hello item appears, you can update whether it's complete by toggling hello checkbox and clicking **Update Tasks**.</span></span>

## <span data-ttu-id="ed3e6-205"><a id="Deploy"></a>6. lépés: A Java-alkalmazás tooAzure webhelyek központi telepítése</span><span class="sxs-lookup"><span data-stu-id="ed3e6-205"><a id="Deploy"></a>Step 6: Deploy your Java application tooAzure Web Sites</span></span>
<span data-ttu-id="ed3e6-206">Az Azure-webhelyek teszi a Java-alkalmazások telepítését más dolga, mint exportálni az alkalmazást WAR-fájlként, majd feltölteni azt a verziókövetési rendszerrel (pl. Git) vagy FTP segítségével.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-206">Azure Web Sites makes deploying Java applications as simple as exporting your application as a WAR file and either uploading it via source control (e.g. Git) or FTP.</span></span>

1. <span data-ttu-id="ed3e6-207">tooexport az alkalmazás WAR-fájlként kattintson a jobb gombbal a projektre a **Project Explorer**, kattintson a **exportálása**, és kattintson a **WAR-fájlt**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-207">tooexport your application as a WAR file, right-click on your project in **Project Explorer**, click **Export**, and then click **WAR File**.</span></span>
2. <span data-ttu-id="ed3e6-208">A hello **WAR exportálása** ablakban, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="ed3e6-208">In hello **WAR Export** window, do hello following:</span></span>
   
   * <span data-ttu-id="ed3e6-209">Hello webes projekt mezőben adja meg azure-documentdb-java-sample.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-209">In hello Web project box, enter azure-documentdb-java-sample.</span></span>
   * <span data-ttu-id="ed3e6-210">Hello cél mezőben válassza ki a cél toosave hello WAR-fájlt.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-210">In hello Destination box, choose a destination toosave hello WAR file.</span></span>
   * <span data-ttu-id="ed3e6-211">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-211">Click **Finish**.</span></span>
3. <span data-ttu-id="ed3e6-212">Most, hogy a WAR-fájl jár, egyszerűen feltöltheti azt tooyour Azure webhelyére **webapps** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-212">Now that you have a WAR file in hand, you can simply upload it tooyour Azure Web Site's **webapps** directory.</span></span> <span data-ttu-id="ed3e6-213">Hello fájl feltöltésével kapcsolatos útmutatásért lásd: [hozzáadása a Java-alkalmazás tooAzure App Service Web Apps](../app-service-web/web-sites-java-add-app.md).</span><span class="sxs-lookup"><span data-stu-id="ed3e6-213">For instructions on uploading hello file, see [Add a Java application tooAzure App Service Web Apps](../app-service-web/web-sites-java-add-app.md).</span></span>
   
    <span data-ttu-id="ed3e6-214">Ha hello WAR-fájl feltöltése toohello webapps könyvtárba, hello futtatókörnyezet észleli majd annak hozzáadását, és automatikusan betölti azt.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-214">Once hello WAR file is uploaded toohello webapps directory, hello runtime environment will detect that you've added it and will automatically load it.</span></span>
4. <span data-ttu-id="ed3e6-215">tooview a kész termék, keresse meg a toohttp://YOUR\_hely\_NAME.azurewebsites.net/azure-java-sample/ és a feladatok hozzáadását start!</span><span class="sxs-lookup"><span data-stu-id="ed3e6-215">tooview your finished product, navigate toohttp://YOUR\_SITE\_NAME.azurewebsites.net/azure-java-sample/ and start adding your tasks!</span></span>

## <span data-ttu-id="ed3e6-216"><a id="GetProject"></a>Hello projekt beszerzése a Githubról</span><span class="sxs-lookup"><span data-stu-id="ed3e6-216"><a id="GetProject"></a>Get hello project from GitHub</span></span>
<span data-ttu-id="ed3e6-217">Összes hello minta ebben az oktatóanyagban szereplő hello [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) projekt a Githubon.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-217">All hello samples in this tutorial are included in hello [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) project on GitHub.</span></span> <span data-ttu-id="ed3e6-218">tooimport hello teendők projekt eclipse-ben történő ellenőrizze, hogy hello szoftver- és erőforrás hello [Előfeltételek](#Prerequisites) területen, majd a következő hello:</span><span class="sxs-lookup"><span data-stu-id="ed3e6-218">tooimport hello todo project into Eclipse, ensure you have hello software and resources listed in hello [Prerequisites](#Prerequisites) section, then do hello following:</span></span>

1. <span data-ttu-id="ed3e6-219">Telepítse a [Project Lombok](http://projectlombok.org/) nevű projektet.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-219">Install [Project Lombok](http://projectlombok.org/).</span></span> <span data-ttu-id="ed3e6-220">Lombok használt toogenerate konstruktorainak, beolvasóinak, Setter hello projektben.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-220">Lombok is used toogenerate constructors, getters, setters in hello project.</span></span> <span data-ttu-id="ed3e6-221">Hello lombok.jar fájl letöltését követően kattintson rá duplán tooinstall, vagy telepítse a hello parancssorból.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-221">Once you have downloaded hello lombok.jar file, double-click it tooinstall it or install it from hello command line.</span></span>
2. <span data-ttu-id="ed3e6-222">Ha az Eclipse meg nyitva, zárja be, és indítsa újra a tooload Lombok.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-222">If Eclipse is open, close it and restart it tooload Lombok.</span></span>
3. <span data-ttu-id="ed3e6-223">Az eclipse-ben, a hello **fájl** menüben kattintson a **importálási**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-223">In Eclipse, on hello **File** menu, click **Import**.</span></span>
4. <span data-ttu-id="ed3e6-224">A hello **importálási** ablakban kattintson **Git**, kattintson a **Projects from Git**, és kattintson a **tovább**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-224">In hello **Import** window, click **Git**, click **Projects from Git**, and then click **Next**.</span></span>
5. <span data-ttu-id="ed3e6-225">A hello **tárház forrásának kijelölése** kattintson **Klónozás URI**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-225">On hello **Select Repository Source** screen, click **Clone URI**.</span></span>
6. <span data-ttu-id="ed3e6-226">A hello **forrás Git-tárház** képernyő hello **URI** mezőbe, írja be a https://github.com/Azure-Samples/java-todo-app.git, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-226">On hello **Source Git Repository** screen, in hello **URI** box, enter https://github.com/Azure-Samples/java-todo-app.git, and then click **Next**.</span></span>
7. <span data-ttu-id="ed3e6-227">A hello **ág kiválasztása** képernyőn **fő** van kiválasztva, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-227">On hello **Branch Selection** screen, ensure that **master** is selected, and then click **Next**.</span></span>
8. <span data-ttu-id="ed3e6-228">A hello **helyi cél** kattintson **Tallózás** tooselect egy mappát, amelyben hello tárház másolhatók, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-228">On hello **Local Destination** screen, click **Browse** tooselect a folder where hello repository can be copied, and then click **Next**.</span></span>
9. <span data-ttu-id="ed3e6-229">A hello **válassza ki a projektek importálásához varázsló toouse** képernyőn **létező projektek importálása** van kiválasztva, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-229">On hello **Select a wizard toouse for importing projects** screen, ensure that **Import existing projects** is selected, and then click **Next**.</span></span>
10. <span data-ttu-id="ed3e6-230">A hello **Import Projects** képernyő, visszavonása hello **DocumentDB** projektre, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-230">On hello **Import Projects** screen, unselect hello **DocumentDB** project, and then click **Finish**.</span></span> <span data-ttu-id="ed3e6-231">hello DocumentDB-projekt tartalmazza hello Azure Cosmos DB Java SDK, amelyre a adunk hozzá a függőség beállításához helyette.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-231">hello DocumentDB project contains hello Azure Cosmos DB Java SDK, which we will add as a dependency instead.</span></span>
11. <span data-ttu-id="ed3e6-232">A **Project Explorer**tooazure-documentdb-java-sample\src\com.microsoft.azure.documentdb.sample.dao\DocumentClientFactory.java váltson, és cserélje le hello HOST és MASTER_KEY értékeket hello URI és PRIMARY KEY a Azure Cosmos DB-fiókot, majd mentés hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-232">In **Project Explorer**, navigate tooazure-documentdb-java-sample\src\com.microsoft.azure.documentdb.sample.dao\DocumentClientFactory.java and replace hello HOST and MASTER_KEY values with hello URI and PRIMARY KEY for your Azure Cosmos DB account, and then save hello file.</span></span> <span data-ttu-id="ed3e6-233">További információ: [1. lépés Egy Azure Cosmos DB adatbázisfiók létrehozása](#CreateDB).</span><span class="sxs-lookup"><span data-stu-id="ed3e6-233">For more information, see [Step 1. Create an Azure Cosmos DB database account](#CreateDB).</span></span>
12. <span data-ttu-id="ed3e6-234">A **Project Explorer**, kattintson jobb gombbal a hello **azure-documentdb-java-sample**, kattintson **Build elérési**, és kattintson a **konfigurálása Build elérési**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-234">In **Project Explorer**, right click hello **azure-documentdb-java-sample**, click **Build Path**, and then click **Configure Build Path**.</span></span>
13. <span data-ttu-id="ed3e6-235">A hello **Java Build elérési** hello jobb oldali ablaktáblában képernyőn válassza hello **szalagtárak** fülre, majd **külső JARs hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-235">On hello **Java Build Path** screen, in hello right pane, select hello **Libraries** tab, and then click **Add External JARs**.</span></span> <span data-ttu-id="ed3e6-236">Keresse meg a toohello hello lombok.jar fájl helyét, és kattintson a **nyitott**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-236">Navigate toohello location of hello lombok.jar file, and click **Open**, and then click **OK**.</span></span>
14. <span data-ttu-id="ed3e6-237">Használjon 12. lépés tooopen hello **tulajdonságok** ablak újra, majd a hello bal oldali ablaktáblán kattintson **megcélzott futtatókörnyezetek**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-237">Use step 12 tooopen hello **Properties** window again, and then in hello left pane click **Targeted Runtimes**.</span></span>
15. <span data-ttu-id="ed3e6-238">A hello **megcélzott futtatókörnyezetek** kattintson **új**, jelölje be **Apache Tomcat v7.0**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-238">On hello **Targeted Runtimes** screen, click **New**, select **Apache Tomcat v7.0**, and then click **OK**.</span></span>
16. <span data-ttu-id="ed3e6-239">Használjon 12. lépés tooopen hello **tulajdonságok** ablak újra, majd a hello bal oldali ablaktáblán kattintson **Project Facets**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-239">Use step 12 tooopen hello **Properties** window again, and then in hello left pane click **Project Facets**.</span></span>
17. <span data-ttu-id="ed3e6-240">A hello **Project Facets** képernyőn válassza ki **dinamikus webmodul** és **Java**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-240">On hello **Project Facets** screen, select **Dynamic Web Module** and **Java**, and then click **OK**.</span></span>
18. <span data-ttu-id="ed3e6-241">A hello **kiszolgálók** üdvözlő képernyőt hello alján lapon kattintson a jobb gombbal **Tomcat v7.0 Server localhost:** majd **hozzáadása és eltávolítása a**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-241">On hello **Servers** tab at hello bottom of hello screen, right-click **Tomcat v7.0 Server at localhost** and then click **Add and Remove**.</span></span>
19. <span data-ttu-id="ed3e6-242">A hello **hozzáadása és eltávolítása a** ablakban helyezze át **azure-documentdb-java-sample** toohello **beállított** gombra, majd **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-242">On hello **Add and Remove** window, move **azure-documentdb-java-sample** toohello **Configured** box, and then click **Finish**.</span></span>
20. <span data-ttu-id="ed3e6-243">A hello **kiszolgálók** lapon kattintson a jobb gombbal **Tomcat v7.0 Server localhost:**, és kattintson a **indítsa újra a**.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-243">In hello **Servers** tab, right-click **Tomcat v7.0 Server at localhost**, and then click **Restart**.</span></span>
21. <span data-ttu-id="ed3e6-244">Egy böngészőben nyissa meg a toohttp://localhost:8080 / azure-documentdb-java-sample / és tooyour feladatlista felvételének megkezdése.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-244">In a browser, navigate toohttp://localhost:8080/azure-documentdb-java-sample/ and start adding tooyour task list.</span></span> <span data-ttu-id="ed3e6-245">Figyelje meg, hogy ha módosította az alapértelmezett értékeket, módosítsa a 8080-as toohello értéket választotta.</span><span class="sxs-lookup"><span data-stu-id="ed3e6-245">Note that if you changed your default port values, change 8080 toohello value you selected.</span></span>
22. <span data-ttu-id="ed3e6-246">toodeploy a projekt tooan Azure-webhelyre, lásd: [6. lépés. Az alkalmazás tooAzure webhelyek telepítése](#Deploy).</span><span class="sxs-lookup"><span data-stu-id="ed3e6-246">toodeploy your project tooan Azure web site, see [Step 6. Deploy your application tooAzure Web Sites](#Deploy).</span></span>

[1]: media/documentdb-java-application/keys.png
