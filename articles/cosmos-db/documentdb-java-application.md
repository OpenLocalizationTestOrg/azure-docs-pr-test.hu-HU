---
title: "Oktatóanyag Java alapú alkalmazásfejlesztéshez Azure Cosmos DB használatával | Microsoft Docs"
description: "A Java webalkalmazásokra vonatkozó oktatóanyag bemutatja tárolására az Azure Cosmos DB és a DocumentDB API használatával és a hozzáférési adatok Azure Websitesban tárolt Java-alkalmazás."
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
ms.openlocfilehash: 292115b5603c6f05a5eab3492d4b3e2096b58ed2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="build-a-java-web-application-using-azure-cosmos-db-and-the-documentdb-api"></a><span data-ttu-id="ae601-104">Azure Cosmos DB és a DocumentDB API-t használó Java-webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae601-104">Build a Java web application using Azure Cosmos DB and the DocumentDB API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ae601-105">.NET</span><span class="sxs-lookup"><span data-stu-id="ae601-105">.NET</span></span>](documentdb-dotnet-application.md)
> * [<span data-ttu-id="ae601-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="ae601-106">Node.js</span></span>](documentdb-nodejs-application.md)
> * [<span data-ttu-id="ae601-107">Java</span><span class="sxs-lookup"><span data-stu-id="ae601-107">Java</span></span>](documentdb-java-application.md)
> * [<span data-ttu-id="ae601-108">Python</span><span class="sxs-lookup"><span data-stu-id="ae601-108">Python</span></span>](documentdb-python-application.md)
> 
> 

<span data-ttu-id="ae601-109">Ez a Java webalkalmazásokra vonatkozó oktatóanyag bemutatja, hogyan használja a [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) szolgáltatás tárolhatja és érheti el az Azure App Service Web Apps tárolt Java-alkalmazás adatait.</span><span class="sxs-lookup"><span data-stu-id="ae601-109">This Java web application tutorial shows you how to use the [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service to store and access data from a Java application hosted on Azure App Service Web Apps.</span></span> <span data-ttu-id="ae601-110">A témakörben érintett témák köre:</span><span class="sxs-lookup"><span data-stu-id="ae601-110">In this topic, you will learn:</span></span>

* <span data-ttu-id="ae601-111">Megtudhatja, hogyan hozható létre olyan alapszintű JavaServer lapok (JSP) alkalmazás az eclipse-ben.</span><span class="sxs-lookup"><span data-stu-id="ae601-111">How to build a basic JavaServer Pages (JSP) application in Eclipse.</span></span>
* <span data-ttu-id="ae601-112">Az Azure Cosmos DB szolgáltatás használata az [Azure Cosmos DB Java SDK-val](https://github.com/Azure/azure-documentdb-java).</span><span class="sxs-lookup"><span data-stu-id="ae601-112">How to work with the Azure Cosmos DB service using the [Azure Cosmos DB Java SDK](https://github.com/Azure/azure-documentdb-java).</span></span>

<span data-ttu-id="ae601-113">Ez a Java-alkalmazásokra vonatkozó oktatóanyag bemutatja, hogyan hozhat létre egy webalapú feladatkezelő alkalmazást, amellyel feladatokat hozhat létre, kérhet le, valamint „kész” jelöléssel láthatja el azokat, ahogyan azt az alábbi illusztráció is mutatja.</span><span class="sxs-lookup"><span data-stu-id="ae601-113">This Java application tutorial shows you how to create a web-based task-management application that enables you to create, retrieve, and mark tasks as complete, as shown in the following image.</span></span> <span data-ttu-id="ae601-114">A rendszer a teendőlistában szereplő összes feladatot JSON-dokumentumként tárolja az Azure Cosmos DB-ben.</span><span class="sxs-lookup"><span data-stu-id="ae601-114">Each of the tasks in the ToDo list are stored as JSON documents in Azure Cosmos DB.</span></span>

![Saját teendőlista Java-alkalmazása](./media/documentdb-java-application/image1.png)

> [!TIP]
> <span data-ttu-id="ae601-116">Ez az alkalmazásfejlesztési oktatóanyag feltételezi, hogy rendelkezik korábbi tapasztalattal a Java használatát illetően.</span><span class="sxs-lookup"><span data-stu-id="ae601-116">This application development tutorial assumes that you have prior experience using Java.</span></span> <span data-ttu-id="ae601-117">Ha még nem ismeri a Javát vagy az [előfeltételt jelentő eszközöket](#Prerequisites), ajánlott letölteni a teljes [teendők](https://github.com/Azure-Samples/documentdb-java-todo-app) projektet a GitHubról, majd lefordítani azt a [jelen cikk végén található utasítások](#GetProject) segítségével.</span><span class="sxs-lookup"><span data-stu-id="ae601-117">If you are new to Java or the [prerequisite tools](#Prerequisites), we recommend downloading the complete [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) project from GitHub and building it using [the instructions at the end of this article](#GetProject).</span></span> <span data-ttu-id="ae601-118">A projekt lefordítása után a cikk áttekintésével betekintést nyerhet a kódba a projekt környezetében.</span><span class="sxs-lookup"><span data-stu-id="ae601-118">Once you have it built, you can review the article to gain insight on the code in the context of the project.</span></span>  
> 
> 

## <span data-ttu-id="ae601-119"><a id="Prerequisites"></a>A jelen Java-webalkalmazásokra vonatkozó oktatóanyag előfeltételei</span><span class="sxs-lookup"><span data-stu-id="ae601-119"><a id="Prerequisites"></a>Prerequisites for this Java web application tutorial</span></span>
<span data-ttu-id="ae601-120">Az alkalmazásfejlesztési oktatóanyag elkezdéséhez az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="ae601-120">Before you begin this application development tutorial, you must have the following:</span></span>

* <span data-ttu-id="ae601-121">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="ae601-121">An active Azure account.</span></span> <span data-ttu-id="ae601-122">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="ae601-122">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="ae601-123">További részletekért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/)</span><span class="sxs-lookup"><span data-stu-id="ae601-123">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/)</span></span>

    <span data-ttu-id="ae601-124">VAGY</span><span class="sxs-lookup"><span data-stu-id="ae601-124">OR</span></span>

    <span data-ttu-id="ae601-125">Az [Azure Cosmos DB Emulator](local-emulator.md) helyi telepítése.</span><span class="sxs-lookup"><span data-stu-id="ae601-125">A local installation of the [Azure Cosmos DB Emulator](local-emulator.md).</span></span>
* <span data-ttu-id="ae601-126">[Java fejlesztői készlet (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="ae601-126">[Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span>
* [<span data-ttu-id="ae601-127">Eclipse IDE for Java EE Developers.</span><span class="sxs-lookup"><span data-stu-id="ae601-127">Eclipse IDE for Java EE Developers.</span></span>](http://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunasr1)
* [<span data-ttu-id="ae601-128">Egy Azure Java-futtatókörnyezettel (pl. Tomcat vagy Jetty) engedélyezve van a webhelyen.</span><span class="sxs-lookup"><span data-stu-id="ae601-128">An Azure Web Site with a Java runtime environment (e.g. Tomcat or Jetty) enabled.</span></span>](../app-service-web/web-sites-java-get-started.md)

<span data-ttu-id="ae601-129">Ha először telepíti ezeket az eszközöket, a coreservlets.com webhelyen megtalálhatja a telepítési folyamat útmutatóját (angol nyelven) az alábbi cikk Quick Start (Gyors üzembe helyezés) szakaszában: [Tutorial: Installing TomCat7 and Using it with Eclipse](http://www.coreservlets.com/Apache-Tomcat-Tutorial/tomcat-7-with-eclipse.html) (Oktatóanyag: A Tomcat7 telepítése és használata az Eclipse-szel).</span><span class="sxs-lookup"><span data-stu-id="ae601-129">If you're installing these tools for the first time, coreservlets.com provides a walk-through of the installation process in the Quick Start section of their [Tutorial: Installing TomCat7 and Using it with Eclipse](http://www.coreservlets.com/Apache-Tomcat-Tutorial/tomcat-7-with-eclipse.html) article.</span></span>

## <span data-ttu-id="ae601-130"><a id="CreateDB"></a>1. lépés: Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae601-130"><a id="CreateDB"></a>Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="ae601-131">Először hozzon létre egy Azure Cosmos DB-fiókot.</span><span class="sxs-lookup"><span data-stu-id="ae601-131">Let's start by creating an Azure Cosmos DB account.</span></span> <span data-ttu-id="ae601-132">Ha már rendelkezik fiókkal vagy az oktatóanyagban az Azure Cosmos DB Emulatort használja, továbbléphet a [2. lépés: Új Java JSP-alkalmazás létrehozása](#CreateJSP) című lépésre.</span><span class="sxs-lookup"><span data-stu-id="ae601-132">If you already have an account or if you are using the Azure Cosmos DB Emulator for this tutorial, you can skip to [Step 2: Create the Java JSP application](#CreateJSP).</span></span>

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

## <span data-ttu-id="ae601-133"><a id="CreateJSP"></a>2. lépés: Új Java JSP-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae601-133"><a id="CreateJSP"></a>Step 2: Create the Java JSP application</span></span>
<span data-ttu-id="ae601-134">JSP-alkalmazás létrehozása:</span><span class="sxs-lookup"><span data-stu-id="ae601-134">To create the JSP application:</span></span>

1. <span data-ttu-id="ae601-135">Először is hozzon létre egy Java-projektet.</span><span class="sxs-lookup"><span data-stu-id="ae601-135">First, we’ll start off by creating a Java project.</span></span> <span data-ttu-id="ae601-136">Indítsa el az Eclipse-t, kattintson a **File** (Fájl), **New** (Új), majd a **Dynamic Web Projekt** (Dinamikus webes projekt) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="ae601-136">Start Eclipse, then click **File**, click **New**, and then click **Dynamic Web Project**.</span></span> <span data-ttu-id="ae601-137">Ha nem jelenik meg a **Dynamic Web Projekt** (Dinamikus webes projekt) lehetőség az elérhető projektek között, tegye a következőt: kattintson a **File** (Fájl), **New** (Új), **Project** (Projekt) lehetőségre, bontsa ki a **Web** elemet, majd kattintson a **Dynamic Web Projekt** (Dinamikus webes projekt) elemre, és végül a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="ae601-137">If you don’t see **Dynamic Web Project** listed as an available project, do the following: click **File**, click **New**, click **Project**…, expand **Web**, click **Dynamic Web Project**, and click **Next**.</span></span>
   
    ![JSP Java-alkalmazások fejlesztése](./media/documentdb-java-application/image10.png)
2. <span data-ttu-id="ae601-139">Adja meg a projekt nevét a **Project name** (Projekt neve) mezőben, majd a **Target Runtime** (Tervezett futásidő) legördülő menüben válasszon ki egy értéket (pl. Apache Tomcat v7.0) (nem kötelező), és kattintson a **Finish** (Befejezés) gombra.</span><span class="sxs-lookup"><span data-stu-id="ae601-139">Enter a project name in the **Project name** box, and in the **Target Runtime** drop-down menu, optionally select a value (e.g. Apache Tomcat v7.0), and then click **Finish**.</span></span> <span data-ttu-id="ae601-140">A tervezett futásidő megadása lehetővé teszi, hogy helyileg, az Eclipse-ben is futtathassa projektjét.</span><span class="sxs-lookup"><span data-stu-id="ae601-140">Selecting a target runtime enables you to run your project locally through Eclipse.</span></span>
3. <span data-ttu-id="ae601-141">Az Eclipse Project Explorer (Projektböngésző) nézetében bontsa ki a projektet.</span><span class="sxs-lookup"><span data-stu-id="ae601-141">In Eclipse, in the Project Explorer view, expand your project.</span></span> <span data-ttu-id="ae601-142">Kattintson a jobb gombbal a **WebContent** (Webes tartalom), majd a **New** (Új) elemre, és végül a **JSP File** (JSP-fájl) elemre.</span><span class="sxs-lookup"><span data-stu-id="ae601-142">Right-click **WebContent**, click **New**, and then click **JSP File**.</span></span>
4. <span data-ttu-id="ae601-143">A **New JSP File** (Új JSP-fájl) párbeszédablakban nevezze el a fájlt az alábbi módon: **index.jsp**.</span><span class="sxs-lookup"><span data-stu-id="ae601-143">In the **New JSP File** dialog box, name the file **index.jsp**.</span></span> <span data-ttu-id="ae601-144">A szülőmappa neve maradjon **WebContent**, ahogy azt az alábbi ábra is mutatja, majd kattintson a **Next** (Tovább) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="ae601-144">Keep the parent folder as **WebContent**, as shown in the following illustration, and then click **Next**.</span></span>
   
    ![Új JSP-fájl létrehozása – Java-alkalmazásokra vonatkozó oktatóanyag](./media/documentdb-java-application/image11.png)
5. <span data-ttu-id="ae601-146">A **Select JSP Template** (JSP-sablon kiválasztása) párbeszédablakban, a jelen oktatóanyag céljából válassza a **New JSP File (html)** (Új JSP-fájl (html)) lehetőséget, majd kattintson a **Finish** (Befejezés) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="ae601-146">In the **Select JSP Template** dialog box, for the purpose of this tutorial select **New JSP File (html)**, and then click **Finish**.</span></span>
6. <span data-ttu-id="ae601-147">Miután megnyílt az index.jsp fájl az Eclipse-ben, adja hozzá a **Hello World!** szöveget</span><span class="sxs-lookup"><span data-stu-id="ae601-147">When the index.jsp file opens in Eclipse, add text to display **Hello World!**</span></span> <span data-ttu-id="ae601-148">a már meglévő <body> elemhez.</span><span class="sxs-lookup"><span data-stu-id="ae601-148">within the existing <body> element.</span></span> <span data-ttu-id="ae601-149">A frissített <body>-tartalomnak az alábbi kódhoz kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="ae601-149">Your updated <body> content should look like the following code:</span></span>
   
        <body>
            <% out.println("Hello World!"); %>
        </body>
7. <span data-ttu-id="ae601-150">Mentse az index.jsp fájlt.</span><span class="sxs-lookup"><span data-stu-id="ae601-150">Save the index.jsp file.</span></span>
8. <span data-ttu-id="ae601-151">Ha megadta a tervezett futásidőt a 2. lépésben, most rákattinthat a **Project** (Projekt), majd a **Run** (Futtatás) elemre a JSP-alkalmazás helyileg történő futtatásához.</span><span class="sxs-lookup"><span data-stu-id="ae601-151">If you set a target runtime in step 2, you can click **Project** and then **Run** to run your JSP application locally:</span></span>
   
    ![Hello World – Java-alkalmazásokra vonatkozó oktatóanyag](./media/documentdb-java-application/image12.png)

## <span data-ttu-id="ae601-153"><a id="InstallSDK"></a>3. lépés: A DocumentDB Java SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="ae601-153"><a id="InstallSDK"></a>Step 3: Install the DocumentDB Java SDK</span></span>
<span data-ttu-id="ae601-154">A DocumentDB Java SDK, valamint annak függőségei a legegyszerűbben az [Apache Maven](http://maven.apache.org/) használatával kérhetők le.</span><span class="sxs-lookup"><span data-stu-id="ae601-154">The easiest way to pull in the DocumentDB Java SDK and its dependencies is through [Apache Maven](http://maven.apache.org/).</span></span>

<span data-ttu-id="ae601-155">Ehhez át kell konvertálnia a projektet Maven-projektté az alábbi lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="ae601-155">To do this, you will need to convert your project to a maven project by completing the following steps:</span></span>

1. <span data-ttu-id="ae601-156">Kattintson a jobb gombbal a projektre a Project Explorer (Projektböngésző) nézetben, kattintson a **Configure** (Konfigurálás), majd a **Convert to Maven Project** (Konvertálás Maven-projekté) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="ae601-156">Right-click your project in the Project Explorer, click **Configure**, click **Convert to Maven Project**.</span></span>
2. <span data-ttu-id="ae601-157">A **Create new POM** (Új POM létrehozása) ablakban fogadja el az alapértelmezett beállításokat, majd kattintson a **Finish** (Befejezés) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="ae601-157">In the **Create new POM** window, accept the defaults and click **Finish**.</span></span>
3. <span data-ttu-id="ae601-158">A **Project Explorer** (Projektböngésző) nézetben nyissa meg a pom.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="ae601-158">In **Project Explorer**, open the pom.xml file.</span></span>
4. <span data-ttu-id="ae601-159">A **Dependencies** (Függőségek) lap **Dependencies** (Függőségek) paneljén kattintson az **Add** (Hozzáadás) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="ae601-159">On the **Dependencies** tab, in the **Dependencies** pane, click **Add**.</span></span>
5. <span data-ttu-id="ae601-160">A **Select Dependency** (Függőség kiválasztása) ablakban tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="ae601-160">In the **Select Dependency** window, do the following:</span></span>
   
   * <span data-ttu-id="ae601-161">Az a **csoportazonosító** mezőbe írja be a következőt: com.microsoft.azure.</span><span class="sxs-lookup"><span data-stu-id="ae601-161">In the **Group Id** box, enter com.microsoft.azure.</span></span>
   * <span data-ttu-id="ae601-162">Az a **összetevő-azonosító** mezőbe írja be az azure-documentdb.</span><span class="sxs-lookup"><span data-stu-id="ae601-162">In the **Artifact Id** box, enter azure-documentdb.</span></span>
   * <span data-ttu-id="ae601-163">Az a **verzió** 1.5.1 adja meg.</span><span class="sxs-lookup"><span data-stu-id="ae601-163">In the **Version** box, enter 1.5.1.</span></span>
     
   ![A DocumentDB Java Application SDK telepítése](./media/documentdb-java-application/image13.png)
     
   * <span data-ttu-id="ae601-165">Vagy adja hozzá a függőség XML a csoport és összetevő-azonosító közvetlenül a pom.xml fájlhoz egy szövegszerkesztő:</span><span class="sxs-lookup"><span data-stu-id="ae601-165">Or add the dependency XML for Group Id and Artifact Id directly to the pom.xml via a text editor:</span></span>
     
        <span data-ttu-id="ae601-166"><dependency><groupId>következőt: com.microsoft.azure</groupId> <artifactId>azure-documentdb</artifactId> <version>1.9.1</version></dependency></span><span class="sxs-lookup"><span data-stu-id="ae601-166"><dependency> <groupId>com.microsoft.azure</groupId> <artifactId>azure-documentdb</artifactId> <version>1.9.1</version> </dependency></span></span>
6. <span data-ttu-id="ae601-167">Kattintson a **OK** és után a Maven feltelepíti a DocumentDB Java SDK-t.</span><span class="sxs-lookup"><span data-stu-id="ae601-167">Click **OK** and Maven will install the DocumentDB Java SDK.</span></span>
7. <span data-ttu-id="ae601-168">Mentse a pom.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="ae601-168">Save the pom.xml file.</span></span>

## <span data-ttu-id="ae601-169"><a id="UseService"></a>4. lépés: Az Azure Cosmos DB szolgáltatás használata Java-alkalmazásokban</span><span class="sxs-lookup"><span data-stu-id="ae601-169"><a id="UseService"></a>Step 4: Using the Azure Cosmos DB service in a Java application</span></span>
1. <span data-ttu-id="ae601-170">Először is határozza meg a TodoItem objektum TodoItem.java:</span><span class="sxs-lookup"><span data-stu-id="ae601-170">First, let's define the TodoItem object in TodoItem.java:</span></span>
   
        @Data
        @Builder
        public class TodoItem {
            private String category;
            private boolean complete;
            private String id;
            private String name;
        }
   
    <span data-ttu-id="ae601-171">Ebben a projektben a [Project Lombok](http://projectlombok.org/) nevű projekt használatával hoztuk létre a konstruktort, a beolvasókat, a beállítókat és a felépítőt.</span><span class="sxs-lookup"><span data-stu-id="ae601-171">In this project, we are using [Project Lombok](http://projectlombok.org/) to generate the constructor, getters, setters, and a builder.</span></span> <span data-ttu-id="ae601-172">Ezt a kódot kézzel is megírhatja, vagy létrehozhatja azt az IDE használatával.</span><span class="sxs-lookup"><span data-stu-id="ae601-172">Alternatively, you can write this code manually or have the IDE generate it.</span></span>
2. <span data-ttu-id="ae601-173">Az Azure Cosmos DB szolgáltatás elindításához példányosítania kell egy új **DocumentClient**-ügyfelet.</span><span class="sxs-lookup"><span data-stu-id="ae601-173">To invoke the Azure Cosmos DB service, you must instantiate a new **DocumentClient**.</span></span> <span data-ttu-id="ae601-174">Általában érdemes újrafelhasználni a **DocumentClient** ügyfelet ahelyett, hogy minden későbbi kérés esetén új ügyfelet hozna létre.</span><span class="sxs-lookup"><span data-stu-id="ae601-174">In general, it is best to reuse the **DocumentClient** - rather than construct a new client for each subsequent request.</span></span> <span data-ttu-id="ae601-175">Az ügyfél újrafelhasználásához burkolja be azt egy **DocumentClientFactory** használatával.</span><span class="sxs-lookup"><span data-stu-id="ae601-175">We can reuse the client by wrapping the client in a **DocumentClientFactory**.</span></span> <span data-ttu-id="ae601-176">A DocumentClientFactory.java, itt kell beilleszteni a vágólapra mentett URI és elsődleges kulcs értéke [1. lépés](#CreateDB).</span><span class="sxs-lookup"><span data-stu-id="ae601-176">In DocumentClientFactory.java, you need to paste the URI and PRIMARY KEY value you saved to your clipboard in [step 1](#CreateDB).</span></span> <span data-ttu-id="ae601-177">Cserélje ki a [YOUR\_ENDPOINT\_HERE] részt az URI-re, a [YOUR\_KEY\_HERE] részt pedig az elsődleges kulcsra.</span><span class="sxs-lookup"><span data-stu-id="ae601-177">Replace [YOUR\_ENDPOINT\_HERE] with your URI and replace [YOUR\_KEY\_HERE] with your PRIMARY KEY.</span></span>
   
        private static final String HOST = "[YOUR_ENDPOINT_HERE]";
        private static final String MASTER_KEY = "[YOUR_KEY_HERE]";
   
        private static DocumentClient documentClient = new DocumentClient(HOST, MASTER_KEY,
                        ConnectionPolicy.GetDefault(), ConsistencyLevel.Session);
   
        public static DocumentClient getDocumentClient() {
            return documentClient;
        }
3. <span data-ttu-id="ae601-178">Ezután hozzon létre egy Data Access Objectet (DAO-t) a teendők az Azure Cosmos DB szolgáltatásba történő kivonatolásához.</span><span class="sxs-lookup"><span data-stu-id="ae601-178">Now let's create a Data Access Object (DAO) to abstract persisting our ToDo items to Azure Cosmos DB.</span></span>
   
    <span data-ttu-id="ae601-179">Ha menteni szeretné a teendőket egy gyűjteménybe, az ügyfélnek tudnia kell, melyik adatbázisban és gyűjteményben kívánja azt megőrizni (ahogy erre az önhivatkozások is hivatkoznak).</span><span class="sxs-lookup"><span data-stu-id="ae601-179">In order to save ToDo items to a collection, the client needs to know which database and collection to persist to (as referenced by self-links).</span></span> <span data-ttu-id="ae601-180">Általában érdemes gyorsítótárazni az adatbázist és a gyűjteményt, amennyiben ez lehetséges, így elkerülhető az adatbázissal való fölösleges üzenetváltás.</span><span class="sxs-lookup"><span data-stu-id="ae601-180">In general, it is best to cache the database and collection when possible to avoid additional round-trips to the database.</span></span>
   
    <span data-ttu-id="ae601-181">Az alábbi kód bemutatja, hogyan kérheti le az adatbázist és a gyűjteményt, ha azok léteznek, és hogyan hozhat létre újat, ha azok még nem léteznek:</span><span class="sxs-lookup"><span data-stu-id="ae601-181">The following code illustrates how to retrieve our database and collection, if it exists, or create a new one if it doesn't exist:</span></span>
   
        public class DocDbDao implements TodoDao {
            // The name of our database.
            private static final String DATABASE_ID = "TodoDB";
   
            // The name of our collection.
            private static final String COLLECTION_ID = "TodoCollection";
   
            // The Azure Cosmos DB Client
            private static DocumentClient documentClient = DocumentClientFactory
                    .getDocumentClient();
   
            // Cache for the database object, so we don't have to query for it to
            // retrieve self links.
            private static Database databaseCache;
   
            // Cache for the collection object, so we don't have to query for it to
            // retrieve self links.
            private static DocumentCollection collectionCache;
   
            private Database getTodoDatabase() {
                if (databaseCache == null) {
                    // Get the database if it exists
                    List<Database> databaseList = documentClient
                            .queryDatabases(
                                    "SELECT * FROM root r WHERE r.id='" + DATABASE_ID
                                            + "'", null).getQueryIterable().toList();
   
                    if (databaseList.size() > 0) {
                        // Cache the database object so we won't have to query for it
                        // later to retrieve the selfLink.
                        databaseCache = databaseList.get(0);
                    } else {
                        // Create the database if it doesn't exist.
                        try {
                            Database databaseDefinition = new Database();
                            databaseDefinition.setId(DATABASE_ID);
   
                            databaseCache = documentClient.createDatabase(
                                    databaseDefinition, null).getResource();
                        } catch (DocumentClientException e) {
                            // TODO: Something has gone terribly wrong - the app wasn't
                            // able to query or create the collection.
                            // Verify your connection, endpoint, and key.
                            e.printStackTrace();
                        }
                    }
                }
   
                return databaseCache;
            }
   
            private DocumentCollection getTodoCollection() {
                if (collectionCache == null) {
                    // Get the collection if it exists.
                    List<DocumentCollection> collectionList = documentClient
                            .queryCollections(
                                    getTodoDatabase().getSelfLink(),
                                    "SELECT * FROM root r WHERE r.id='" + COLLECTION_ID
                                            + "'", null).getQueryIterable().toList();
   
                    if (collectionList.size() > 0) {
                        // Cache the collection object so we won't have to query for it
                        // later to retrieve the selfLink.
                        collectionCache = collectionList.get(0);
                    } else {
                        // Create the collection if it doesn't exist.
                        try {
                            DocumentCollection collectionDefinition = new DocumentCollection();
                            collectionDefinition.setId(COLLECTION_ID);
   
                            collectionCache = documentClient.createCollection(
                                    getTodoDatabase().getSelfLink(),
                                    collectionDefinition, null).getResource();
                        } catch (DocumentClientException e) {
                            // TODO: Something has gone terribly wrong - the app wasn't
                            // able to query or create the collection.
                            // Verify your connection, endpoint, and key.
                            e.printStackTrace();
                        }
                    }
                }
   
                return collectionCache;
            }
        }
4. <span data-ttu-id="ae601-182">A következő lépésben be kell írnia egy kódrészletet a teendők a gyűjteményben való megőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="ae601-182">The next step is to write some code to persist the TodoItems in to the collection.</span></span> <span data-ttu-id="ae601-183">A jelen példában a [Gson](https://code.google.com/p/google-gson/) segítségével szerializáltuk és deszerializáltuk a teendő POJO objektumait JSON-dokumentumokká.</span><span class="sxs-lookup"><span data-stu-id="ae601-183">In this example, we will use [Gson](https://code.google.com/p/google-gson/) to serialize and de-serialize TodoItem Plain Old Java Objects (POJOs) to JSON documents.</span></span>
   
        // We'll use Gson for POJO <=> JSON serialization for this example.
        private static Gson gson = new Gson();
   
        @Override
        public TodoItem createTodoItem(TodoItem todoItem) {
            // Serialize the TodoItem as a JSON Document.
            Document todoItemDocument = new Document(gson.toJson(todoItem));
   
            // Annotate the document as a TodoItem for retrieval (so that we can
            // store multiple entity types in the collection).
            todoItemDocument.set("entityType", "todoItem");
   
            try {
                // Persist the document using the DocumentClient.
                todoItemDocument = documentClient.createDocument(
                        getTodoCollection().getSelfLink(), todoItemDocument, null,
                        false).getResource();
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return null;
            }
   
            return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
        }
5. <span data-ttu-id="ae601-184">Csakúgy, mint az Azure Cosmos DB-adatbázisok és -gyűjtemények esetén, a dokumentumok önhivatkozásokkal is hivatkoznak magukra.</span><span class="sxs-lookup"><span data-stu-id="ae601-184">Like Azure Cosmos DB databases and collections, documents are also referenced by self-links.</span></span> <span data-ttu-id="ae601-185">A következő segédfüggvény lehetővé teszi a dokumentumok az önhivatkozástól eltérő attribútum (pl. „id”) alapján történő lekérését:</span><span class="sxs-lookup"><span data-stu-id="ae601-185">The following helper function lets us retrieve documents by another attribute (e.g. "id") rather than self-link:</span></span>
   
        private Document getDocumentById(String id) {
            // Retrieve the document using the DocumentClient.
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
6. <span data-ttu-id="ae601-186">Az 5. lépésben leírt segédmetódus segítségével lekérheti egy teendő JSON-dokumentumát annak azonosítója alapján, majd deszerializálhatja azt egy POJO-vá.</span><span class="sxs-lookup"><span data-stu-id="ae601-186">We can use the helper method in step 5 to retrieve a TodoItem JSON document by id and then deserialize it to a POJO:</span></span>
   
        @Override
        public TodoItem readTodoItem(String id) {
            // Retrieve the document by id using our helper method.
            Document todoItemDocument = getDocumentById(id);
   
            if (todoItemDocument != null) {
                // De-serialize the document in to a TodoItem.
                return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
            } else {
                return null;
            }
        }
7. <span data-ttu-id="ae601-187">A DocumentClient ügyfél segítségével szintén lekérheti a teendők gyűjteményét vagy listáját a DocumentDB SQL használatával:</span><span class="sxs-lookup"><span data-stu-id="ae601-187">We can also use the DocumentClient to get a collection or list of TodoItems using DocumentDB SQL:</span></span>
   
        @Override
        public List<TodoItem> readTodoItems() {
            List<TodoItem> todoItems = new ArrayList<TodoItem>();
   
            // Retrieve the TodoItem documents
            List<Document> documentList = documentClient
                    .queryDocuments(getTodoCollection().getSelfLink(),
                            "SELECT * FROM root r WHERE r.entityType = 'todoItem'",
                            null).getQueryIterable().toList();
   
            // De-serialize the documents in to TodoItems.
            for (Document todoItemDocument : documentList) {
                todoItems.add(gson.fromJson(todoItemDocument.toString(),
                        TodoItem.class));
            }
   
            return todoItems;
        }
8. <span data-ttu-id="ae601-188">A DocumentClient ügyfél segítségével számos módon frissítheti a dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="ae601-188">There are many ways to update a document with the DocumentClient.</span></span> <span data-ttu-id="ae601-189">A teendőlista alkalmazásában azt szeretnénk, ha beállíthatnánk, hogy elvégeztük-e az egyes teendőket.</span><span class="sxs-lookup"><span data-stu-id="ae601-189">In our Todo list application, we want to be able to toggle whether a TodoItem is complete.</span></span> <span data-ttu-id="ae601-190">Ez a dokumentum „complete” (befejezve) attribútumának frissítésével érhető el:</span><span class="sxs-lookup"><span data-stu-id="ae601-190">This can be achieved by updating the "complete" attribute within the document:</span></span>
   
        @Override
        public TodoItem updateTodoItem(String id, boolean isComplete) {
            // Retrieve the document from the database
            Document todoItemDocument = getDocumentById(id);
   
            // You can update the document as a JSON document directly.
            // For more complex operations - you could de-serialize the document in
            // to a POJO, update the POJO, and then re-serialize the POJO back in to
            // a document.
            todoItemDocument.set("complete", isComplete);
   
            try {
                // Persist/replace the updated document.
                todoItemDocument = documentClient.replaceDocument(todoItemDocument,
                        null).getResource();
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return null;
            }
   
            return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
        }
9. <span data-ttu-id="ae601-191">Végül azt szeretnénk, ha törölhetnénk az egyes teendőket a listából.</span><span class="sxs-lookup"><span data-stu-id="ae601-191">Finally, we want the ability to delete a TodoItem from our list.</span></span> <span data-ttu-id="ae601-192">Ehhez a korábban megírt segédmetódus használatával kérje le az önhivatkozást, majd adja ki a törlés parancsot az ügyfélnek:</span><span class="sxs-lookup"><span data-stu-id="ae601-192">To do this, we can use the helper method we wrote earlier to retrieve the self-link and then tell the client to delete it:</span></span>
   
        @Override
        public boolean deleteTodoItem(String id) {
            // Azure Cosmos DB refers to documents by self link rather than id.
   
            // Query for the document to retrieve the self link.
            Document todoItemDocument = getDocumentById(id);
   
            try {
                // Delete the document by self link.
                documentClient.deleteDocument(todoItemDocument.getSelfLink(), null);
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return false;
            }
   
            return true;
        }

## <span data-ttu-id="ae601-193"><a id="Wire"></a>5. lépés: A Java-alkalmazásfejlesztési projekt fennmaradó részeinek összefűzése</span><span class="sxs-lookup"><span data-stu-id="ae601-193"><a id="Wire"></a>Step 5: Wiring the rest of the of Java application development project together</span></span>
<span data-ttu-id="ae601-194">Most, hogy befejeztük a visszatöltött bits - marad csak a lehet létrehozni egy gyors felhasználói felületet, és hozzá kell fűznie azt a DAO legfeljebb.</span><span class="sxs-lookup"><span data-stu-id="ae601-194">Now that we've finished the fun bits - all that's left is to build a quick user interface and wire it up to our DAO.</span></span>

1. <span data-ttu-id="ae601-195">Először is hozzon létre egy vezérlőt a DAO meghívásához:</span><span class="sxs-lookup"><span data-stu-id="ae601-195">First, let's start with building a controller to call our DAO:</span></span>
   
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
   
    <span data-ttu-id="ae601-196">Egy összetettebb alkalmazásban a vezérlő a DAO mellett bonyolult üzleti logikát is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="ae601-196">In a more complex application, the controller may house complicated business logic on top of the DAO.</span></span>
2. <span data-ttu-id="ae601-197">Ezután hozzon létre egy servletet, amely továbbítja a HTTP-kéréseket a vezérlőhöz:</span><span class="sxs-lookup"><span data-stu-id="ae601-197">Next, we'll create a servlet to route HTTP requests to the controller:</span></span>
   
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
3. <span data-ttu-id="ae601-198">Szükség lesz a felhasználó számára megjelenített webes felhasználói felületet.</span><span class="sxs-lookup"><span data-stu-id="ae601-198">We'll need a web user interface to display to the user.</span></span> <span data-ttu-id="ae601-199">Írja át a korábban létrehozott index.jsp fájlt az alábbi módon:</span><span class="sxs-lookup"><span data-stu-id="ae601-199">Let's re-write the index.jsp we created earlier:</span></span>
    ```html
        <html>
        <head>
          <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
          <meta http-equiv="X-UA-Compatible" content="IE=edge;" />
          <title>Azure Cosmos DB Java Sample</title>
   
          <!-- Bootstrap -->
          <link href="//ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css" rel="stylesheet">
   
          <style>
            /* Add padding to body for fixed nav bar */
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
   
            <!-- The ToDo List -->
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
   
          <!-- Placed at the end of the document so the pages load faster -->
          <script src="//ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js"></script>
          <script src="//ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js"></script>
          <script src="assets/todo.js"></script>
        </body>
        </html>
    ```
4. <span data-ttu-id="ae601-200">És végezetül írjon egy ügyféloldali JavaScript a webes felhasználói felület és a servlet egy egységként kezelhető:</span><span class="sxs-lookup"><span data-stu-id="ae601-200">And finally, write some client-side JavaScript to tie the web user interface and the servlet together:</span></span>
   
        var todoApp = {
          /*
           * API methods to call Java backend.
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
   
              // Call api to update todo items.
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
           * Install the TodoApp
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
5. <span data-ttu-id="ae601-201">Nagyszerű!</span><span class="sxs-lookup"><span data-stu-id="ae601-201">Awesome!</span></span> <span data-ttu-id="ae601-202">Most már csak le kell tesztelni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ae601-202">Now all that's left is to test the application.</span></span> <span data-ttu-id="ae601-203">Futtassa az alkalmazást helyileg, és adjon hozzá néhány teendőt. Ehhez adja meg az elemek nevét és kategóriáját, majd kattintson az **Add Task** (Feladat hozzáadása) elemre.</span><span class="sxs-lookup"><span data-stu-id="ae601-203">Run the application locally, and add some Todo items by filling in the item name and category and clicking **Add Task**.</span></span>
6. <span data-ttu-id="ae601-204">Ha megjelent az elem, a jelölőnégyzet bejelölésével, majd az **Update Tasks** (Feladatok frissítése) gombra való kattintással állíthatja be, hogy elvégezte-e már azt.</span><span class="sxs-lookup"><span data-stu-id="ae601-204">Once the item appears, you can update whether it's complete by toggling the checkbox and clicking **Update Tasks**.</span></span>

## <span data-ttu-id="ae601-205"><a id="Deploy"></a>6. lépés: A Java-alkalmazás az Azure-webhelyek központi telepítése</span><span class="sxs-lookup"><span data-stu-id="ae601-205"><a id="Deploy"></a>Step 6: Deploy your Java application to Azure Web Sites</span></span>
<span data-ttu-id="ae601-206">Az Azure-webhelyek teszi a Java-alkalmazások telepítését más dolga, mint exportálni az alkalmazást WAR-fájlként, majd feltölteni azt a verziókövetési rendszerrel (pl. Git) vagy FTP segítségével.</span><span class="sxs-lookup"><span data-stu-id="ae601-206">Azure Web Sites makes deploying Java applications as simple as exporting your application as a WAR file and either uploading it via source control (e.g. Git) or FTP.</span></span>

1. <span data-ttu-id="ae601-207">Az alkalmazás WAR-fájlként történő exportálásához kattintson a jobb gombbal a projektre a **Project Explorer**, kattintson a **exportálása**, és kattintson a **WAR-fájlt**.</span><span class="sxs-lookup"><span data-stu-id="ae601-207">To export your application as a WAR file, right-click on your project in **Project Explorer**, click **Export**, and then click **WAR File**.</span></span>
2. <span data-ttu-id="ae601-208">A **WAR Export** (WAR-fájl exportálása) ablakban tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="ae601-208">In the **WAR Export** window, do the following:</span></span>
   
   * <span data-ttu-id="ae601-209">A Web project (Webes projekt) mezőben adja meg a következőt: azure-documentdb-java-sample.</span><span class="sxs-lookup"><span data-stu-id="ae601-209">In the Web project box, enter azure-documentdb-java-sample.</span></span>
   * <span data-ttu-id="ae601-210">A Destination (Cél) mezőben válassza ki, hova szeretné menteni a WAR-fájlt.</span><span class="sxs-lookup"><span data-stu-id="ae601-210">In the Destination box, choose a destination to save the WAR file.</span></span>
   * <span data-ttu-id="ae601-211">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="ae601-211">Click **Finish**.</span></span>
3. <span data-ttu-id="ae601-212">Most, hogy a WAR-fájl jár, egyszerűen feltöltheti azt a a Azure webhely **webapps** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="ae601-212">Now that you have a WAR file in hand, you can simply upload it to your Azure Web Site's **webapps** directory.</span></span> <span data-ttu-id="ae601-213">Fájlok feltöltésével kapcsolatos útmutatásért lásd: [hozzáadása a Java-alkalmazások az Azure App Service Web Apps](../app-service-web/web-sites-java-add-app.md).</span><span class="sxs-lookup"><span data-stu-id="ae601-213">For instructions on uploading the file, see [Add a Java application to Azure App Service Web Apps](../app-service-web/web-sites-java-add-app.md).</span></span>
   
    <span data-ttu-id="ae601-214">Miután feltöltötte a WAR-fájlt a webapps könyvtárba, a futtatókörnyezet észleli majd annak hozzáadását, és automatikusan betölti azt.</span><span class="sxs-lookup"><span data-stu-id="ae601-214">Once the WAR file is uploaded to the webapps directory, the runtime environment will detect that you've added it and will automatically load it.</span></span>
4. <span data-ttu-id="ae601-215">A kész termék megtekintéséhez keresse meg http://YOUR\_hely\_NAME.azurewebsites.net/azure-java-sample/ és a feladatok hozzáadását start!</span><span class="sxs-lookup"><span data-stu-id="ae601-215">To view your finished product, navigate to http://YOUR\_SITE\_NAME.azurewebsites.net/azure-java-sample/ and start adding your tasks!</span></span>

## <span data-ttu-id="ae601-216"><a id="GetProject"></a>A projekt beszerzése a GitHubról</span><span class="sxs-lookup"><span data-stu-id="ae601-216"><a id="GetProject"></a>Get the project from GitHub</span></span>
<span data-ttu-id="ae601-217">A jelen oktatóanyag minden példáját megtalálhatja a GitHubról elérhető [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) (teendők) projektben.</span><span class="sxs-lookup"><span data-stu-id="ae601-217">All the samples in this tutorial are included in the [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) project on GitHub.</span></span> <span data-ttu-id="ae601-218">A teendők projekt Eclipse-be történő importálásához győződjön meg arról, hogy rendelkezik az [Előfeltételek](#Prerequisites) szakaszban ismertetett szoftverekkel és erőforrásokkal, majd tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="ae601-218">To import the todo project into Eclipse, ensure you have the software and resources listed in the [Prerequisites](#Prerequisites) section, then do the following:</span></span>

1. <span data-ttu-id="ae601-219">Telepítse a [Project Lombok](http://projectlombok.org/) nevű projektet.</span><span class="sxs-lookup"><span data-stu-id="ae601-219">Install [Project Lombok](http://projectlombok.org/).</span></span> <span data-ttu-id="ae601-220">A projekt konstruktorainak, beolvasóinak, beállítóinak létrehozása a Lombok használatával történik.</span><span class="sxs-lookup"><span data-stu-id="ae601-220">Lombok is used to generate constructors, getters, setters in the project.</span></span> <span data-ttu-id="ae601-221">A lombok.jar fájl letöltése után kattintson rá duplán annak telepítéséhez, vagy telepítse azt a parancssorból.</span><span class="sxs-lookup"><span data-stu-id="ae601-221">Once you have downloaded the lombok.jar file, double-click it to install it or install it from the command line.</span></span>
2. <span data-ttu-id="ae601-222">Ha az Eclipse meg van nyitva, zárja be, és indítsa el újra a Lombok betöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="ae601-222">If Eclipse is open, close it and restart it to load Lombok.</span></span>
3. <span data-ttu-id="ae601-223">Az Eclipse **File** (Fájl) menüjében kattintson az **Import** (Importálás) elemre.</span><span class="sxs-lookup"><span data-stu-id="ae601-223">In Eclipse, on the **File** menu, click **Import**.</span></span>
4. <span data-ttu-id="ae601-224">Az **Import** (Importálás) ablakban kattintson a **Git**, majd a **Projects from Git** (Git-projektek), és végül a **Next** (Tovább) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="ae601-224">In the **Import** window, click **Git**, click **Projects from Git**, and then click **Next**.</span></span>
5. <span data-ttu-id="ae601-225">A **Select Repository Source** (Tárház forrásának kiválasztása) képernyőn kattintson a **Clone URI** (URI klónozása) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="ae601-225">On the **Select Repository Source** screen, click **Clone URI**.</span></span>
6. <span data-ttu-id="ae601-226">Az a **forrás Git-tárház** képernyő a **URI** mezőbe, írja be a https://github.com/Azure-Samples/java-todo-app.git, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="ae601-226">On the **Source Git Repository** screen, in the **URI** box, enter https://github.com/Azure-Samples/java-todo-app.git, and then click **Next**.</span></span>
7. <span data-ttu-id="ae601-227">A **Branch Selection** (Ág kiválasztása) képernyőn válassza a **master** (fő) lehetőséget, majd kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="ae601-227">On the **Branch Selection** screen, ensure that **master** is selected, and then click **Next**.</span></span>
8. <span data-ttu-id="ae601-228">A **Local Destination** (Helyi cél) képernyőn kattintson a **Browse** (Tallózás) lehetőségre, válassza ki a mappát, ahova a tárházat másolni szeretné, majd kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="ae601-228">On the **Local Destination** screen, click **Browse** to select a folder where the repository can be copied, and then click **Next**.</span></span>
9. <span data-ttu-id="ae601-229">A **Select a wizard to use for importing projects** (Varázsló kiválasztása a projektek importálásához) képernyőn győződjön meg arról, hogy az **Import existing projects** (Létező projektek importálása) lehetőség van kiválasztva, majd kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="ae601-229">On the **Select a wizard to use for importing projects** screen, ensure that **Import existing projects** is selected, and then click **Next**.</span></span>
10. <span data-ttu-id="ae601-230">Az **Import Projects** (Projektek importálása) képernyőn törölje a **DocumentDB** projekt jelölését, majd kattintson a **Finish** (Befejezés) gombra.</span><span class="sxs-lookup"><span data-stu-id="ae601-230">On the **Import Projects** screen, unselect the **DocumentDB** project, and then click **Finish**.</span></span> <span data-ttu-id="ae601-231">A DocumentDB-projekt tartalmazza az Azure Cosmos DB Java SDK, amely adunk hozzá a függőség beállításához helyette.</span><span class="sxs-lookup"><span data-stu-id="ae601-231">The DocumentDB project contains the Azure Cosmos DB Java SDK, which we will add as a dependency instead.</span></span>
11. <span data-ttu-id="ae601-232">A **Project Explorer**Azure-documentdb-Java-sample\src\com.microsoft.Azure.documentdb.sample.dao\DocumentClientFactory.Java váltson, és cserélje le a HOST és MASTER_KEY értékeket a URI és PRIMARY KEY a Az Azure Cosmos DB fiókot, és mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="ae601-232">In **Project Explorer**, navigate to azure-documentdb-java-sample\src\com.microsoft.azure.documentdb.sample.dao\DocumentClientFactory.java and replace the HOST and MASTER_KEY values with the URI and PRIMARY KEY for your Azure Cosmos DB account, and then save the file.</span></span> <span data-ttu-id="ae601-233">További információ: [1. lépés Egy Azure Cosmos DB adatbázisfiók létrehozása](#CreateDB).</span><span class="sxs-lookup"><span data-stu-id="ae601-233">For more information, see [Step 1. Create an Azure Cosmos DB database account](#CreateDB).</span></span>
12. <span data-ttu-id="ae601-234">A **Project Explorer** (Projektböngésző) nézetben kattintson a jobb gombbal az **azure-documentdb-java-sample** elemre, majd kattintson a **Build Path** (Fordítás elérési útja), és végül a **Configure Build Path** (Fordítás elérési útjának konfigurálása) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="ae601-234">In **Project Explorer**, right click the **azure-documentdb-java-sample**, click **Build Path**, and then click **Configure Build Path**.</span></span>
13. <span data-ttu-id="ae601-235">A **Java Build Path** (Java-verzió elérési útja) képernyő jobb oldalsó paneljén válassza ki a **Libraries** (Könyvtárak) lapot, majd kattintson az **Add External JARs** (Külső JAR-fájlok hozzáadása) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="ae601-235">On the **Java Build Path** screen, in the right pane, select the **Libraries** tab, and then click **Add External JARs**.</span></span> <span data-ttu-id="ae601-236">Navigáljon a lombok.jar fájl helyére, kattintson az **Open** (Megnyitás), majd az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="ae601-236">Navigate to the location of the lombok.jar file, and click **Open**, and then click **OK**.</span></span>
14. <span data-ttu-id="ae601-237">A 12. lépésben leírtak segítségével nyissa meg ismét a **Properties** (Tulajdonságok) ablakot, majd a bal oldali ablaktáblán kattintson a **Targeted Runtimes** (Tervezett futásidők) elemre.</span><span class="sxs-lookup"><span data-stu-id="ae601-237">Use step 12 to open the **Properties** window again, and then in the left pane click **Targeted Runtimes**.</span></span>
15. <span data-ttu-id="ae601-238">A **Targeted Runtimes** (Tervezett futásidők) képernyőn kattintson a **New** (Új) elemre, válassza ki az **Apache Tomcat v7.0** lehetőséget, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="ae601-238">On the **Targeted Runtimes** screen, click **New**, select **Apache Tomcat v7.0**, and then click **OK**.</span></span>
16. <span data-ttu-id="ae601-239">A 12. lépésben leírtak segítségével nyissa meg ismét a **Properties** (Tulajdonságok) ablakot, majd a bal oldali ablaktáblán kattintson a **Project Facets** (A projekt aspektusai) elemre.</span><span class="sxs-lookup"><span data-stu-id="ae601-239">Use step 12 to open the **Properties** window again, and then in the left pane click **Project Facets**.</span></span>
17. <span data-ttu-id="ae601-240">A **Project Facets** (A projekt aspektusai) képernyőn válassza a **Dynamic Web Module** (Dinamikus webmodul), majd a **Java** lehetőséget, és kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="ae601-240">On the **Project Facets** screen, select **Dynamic Web Module** and **Java**, and then click **OK**.</span></span>
18. <span data-ttu-id="ae601-241">A **Servers** (Kiszolgálók) lapon, a képernyő alján, kattintson a jobb gombbal a **Tomcat v7.0 Server at localhost** (Tomcat 7.0-s verziójú kiszolgáló itt: localhost), majd az **Add and Remove** (Hozzáadás és eltávolítás) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="ae601-241">On the **Servers** tab at the bottom of the screen, right-click **Tomcat v7.0 Server at localhost** and then click **Add and Remove**.</span></span>
19. <span data-ttu-id="ae601-242">Az **Add and Remove** (Hozzáadás és eltávolítás) ablakban helyezze át az **azure-documentdb-java-sample** kiszolgálót a **Configured** (Konfigurált) mezőbe, majd kattintson a **Finish** (Befejezés) gombra.</span><span class="sxs-lookup"><span data-stu-id="ae601-242">On the **Add and Remove** window, move **azure-documentdb-java-sample** to the **Configured** box, and then click **Finish**.</span></span>
20. <span data-ttu-id="ae601-243">Az a **kiszolgálók** lapon kattintson a jobb gombbal **Tomcat v7.0 Server localhost:**, és kattintson a **indítsa újra a**.</span><span class="sxs-lookup"><span data-stu-id="ae601-243">In the **Servers** tab, right-click **Tomcat v7.0 Server at localhost**, and then click **Restart**.</span></span>
21. <span data-ttu-id="ae601-244">Egy böngészőből keresse fel a http://localhost:8080/azure-documentdb-java-sample/ címet, és kezdje el hozzáadni a feladatait a listához.</span><span class="sxs-lookup"><span data-stu-id="ae601-244">In a browser, navigate to http://localhost:8080/azure-documentdb-java-sample/ and start adding to your task list.</span></span> <span data-ttu-id="ae601-245">Ügyeljen arra, hogy ha módosította a portok alapértelmezett értékét, akkor a 8080 értéket módosítsa a választott értékre.</span><span class="sxs-lookup"><span data-stu-id="ae601-245">Note that if you changed your default port values, change 8080 to the value you selected.</span></span>
22. <span data-ttu-id="ae601-246">Projekt telepítése egy Azure-webhelyre: [6. lépés Telepítheti az alkalmazást az Azure-webhelyek](#Deploy).</span><span class="sxs-lookup"><span data-stu-id="ae601-246">To deploy your project to an Azure web site, see [Step 6. Deploy your application to Azure Web Sites](#Deploy).</span></span>

[1]: media/documentdb-java-application/keys.png
