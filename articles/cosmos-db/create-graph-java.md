---
title: "egy Azure Cosmos DB graph adatbázist, Java aaaCreate |} Microsoft Docs"
description: "Megadja a Java mintakód tooconnect tooand lekérdezés Diagramadatok használhatja az Azure Cosmos Adatbázisba Gremlin használatával."
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/24/2017
ms.author: denlee
ms.openlocfilehash: 595c0fb108f3dbe8c83674f0c9c4b0cdd3ab4c95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-create-a-graph-database-using-java-and-hello-azure-portal"></a><span data-ttu-id="c91c1-103">Azure Cosmos DB: Hozzon létre egy grafikonon adatbázist Java és hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c91c1-103">Azure Cosmos DB: Create a graph database using Java and hello Azure portal</span></span>

<span data-ttu-id="c91c1-104">Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="c91c1-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="c91c1-105">Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="c91c1-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="c91c1-106">A gyors üzembe helyezés létrehoz egy grafikonon adatbázis használata az Azure portál eszközök hello Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c91c1-106">This quickstart creates a graph database using hello Azure portal tools for Azure Cosmos DB.</span></span> <span data-ttu-id="c91c1-107">A gyors üzembe helyezés azt is bemutatja, hogyan tooquickly hozzon létre egy Java-Konzolalkalmazás használatával egy grafikonon adatbázist hello OSS [Gremlin Java](https://mvnrepository.com/artifact/org.apache.tinkerpop/gremlin-driver) illesztőprogram.</span><span class="sxs-lookup"><span data-stu-id="c91c1-107">This quickstart also shows you how tooquickly create a Java console app using a graph database using hello OSS [Gremlin Java](https://mvnrepository.com/artifact/org.apache.tinkerpop/gremlin-driver) driver.</span></span> <span data-ttu-id="c91c1-108">hello utasításait a gyors üzembe helyezés követhetők bármely operációs rendszeren, amely alkalmas a Java futtatására.</span><span class="sxs-lookup"><span data-stu-id="c91c1-108">hello instructions in this quickstart can be followed on any operating system that is capable of running Java.</span></span> <span data-ttu-id="c91c1-109">A gyors üzembe helyezés familiarizes létrehozásával és módosításával vagy hello felhasználói felületén, vagy programozott módon, amelyik igény szerint a graph-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="c91c1-109">This quickstart familiarizes you with creating and modifying graph resources in either hello UI or programmatically, whichever is your preference.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="c91c1-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c91c1-110">Prerequisites</span></span>

* [<span data-ttu-id="c91c1-111">Java fejlesztői készlet (JDK) 1.7+</span><span class="sxs-lookup"><span data-stu-id="c91c1-111">Java Development Kit (JDK) 1.7+</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * <span data-ttu-id="c91c1-112">Ubuntu, futtassa `apt-get install default-jdk` tooinstall hello JDK.</span><span class="sxs-lookup"><span data-stu-id="c91c1-112">On Ubuntu, run `apt-get install default-jdk` tooinstall hello JDK.</span></span>
    * <span data-ttu-id="c91c1-113">Lehet, hogy tooset hello JAVA_HOME környezeti változó toopoint toohello mappa hello JDK futtató.</span><span class="sxs-lookup"><span data-stu-id="c91c1-113">Be sure tooset hello JAVA_HOME environment variable toopoint toohello folder where hello JDK is installed.</span></span>
* <span data-ttu-id="c91c1-114">[Maven](http://maven.apache.org/download.cgi) bináris archívum [letöltése](http://maven.apache.org/install.html) és [telepítése](http://maven.apache.org/)</span><span class="sxs-lookup"><span data-stu-id="c91c1-114">[Download](http://maven.apache.org/download.cgi) and [install](http://maven.apache.org/install.html) a [Maven](http://maven.apache.org/) binary archive</span></span>
    * <span data-ttu-id="c91c1-115">Ubuntu, futtathatja `apt-get install maven` tooinstall Maven.</span><span class="sxs-lookup"><span data-stu-id="c91c1-115">On Ubuntu, you can run `apt-get install maven` tooinstall Maven.</span></span>
* [<span data-ttu-id="c91c1-116">Git</span><span class="sxs-lookup"><span data-stu-id="c91c1-116">Git</span></span>](https://www.git-scm.com/)
    * <span data-ttu-id="c91c1-117">Ubuntu, futtathatja `sudo apt-get install git` tooinstall Git.</span><span class="sxs-lookup"><span data-stu-id="c91c1-117">On Ubuntu, you can run `sudo apt-get install git` tooinstall Git.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="c91c1-118">Adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="c91c1-118">Create a database account</span></span>

<span data-ttu-id="c91c1-119">Egy grafikonon adatbázist hozhat létre, meg kell toocreate egy Azure Cosmos DB Gremlin (diagramhoz) adatbázis fiókot.</span><span class="sxs-lookup"><span data-stu-id="c91c1-119">Before you can create a graph database, you need toocreate a Gremlin (Graph) database account with Azure Cosmos DB.</span></span>

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a><span data-ttu-id="c91c1-120">Gráf hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c91c1-120">Add a graph</span></span>

<span data-ttu-id="c91c1-121">Hello Data Explorer eszköz már használhatja, ha az Azure portál toocreate egy grafikonon adatbázis hello.</span><span class="sxs-lookup"><span data-stu-id="c91c1-121">You can now use hello Data Explorer tool in hello Azure portal toocreate a graph database.</span></span> 

1. <span data-ttu-id="c91c1-122">Hello hello bal oldali navigációs menü, Azure-portálon kattintson **adatok kezelővel (előzetes verzió)**.</span><span class="sxs-lookup"><span data-stu-id="c91c1-122">In hello Azure portal, in hello left navigation menu, click **Data Explorer (Preview)**.</span></span> 
2. <span data-ttu-id="c91c1-123">A hello **adatok kezelővel (előzetes verzió)** panelen kattintson a **új Graph**, használja a következő információ hello hello lapon töltse ki:</span><span class="sxs-lookup"><span data-stu-id="c91c1-123">In hello **Data Explorer (Preview)** blade, click **New Graph**, then fill in hello page using hello following information:</span></span>

    ![Az Azure-portálon hello adatkezelő](./media/create-graph-java/azure-cosmosdb-data-explorer.png)

    <span data-ttu-id="c91c1-125">Beállítás</span><span class="sxs-lookup"><span data-stu-id="c91c1-125">Setting</span></span>|<span data-ttu-id="c91c1-126">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="c91c1-126">Suggested value</span></span>|<span data-ttu-id="c91c1-127">Leírás</span><span class="sxs-lookup"><span data-stu-id="c91c1-127">Description</span></span>
    ---|---|---
    <span data-ttu-id="c91c1-128">Adatbázis-azonosító</span><span class="sxs-lookup"><span data-stu-id="c91c1-128">Database ID</span></span>|<span data-ttu-id="c91c1-129">sample-database</span><span class="sxs-lookup"><span data-stu-id="c91c1-129">sample-database</span></span>|<span data-ttu-id="c91c1-130">az új adatbázis hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="c91c1-130">hello ID for your new database.</span></span> <span data-ttu-id="c91c1-131">Az adatbázis neve 1–255 karakter hosszúságú lehet, és nem tartalmazhat `/ \ # ?` karaktereket vagy záró szóközt.</span><span class="sxs-lookup"><span data-stu-id="c91c1-131">Database names must be between 1 and 255 characters, and cannot contain `/ \ # ?` or a trailing space.</span></span>
    <span data-ttu-id="c91c1-132">Gráfazonosító</span><span class="sxs-lookup"><span data-stu-id="c91c1-132">Graph ID</span></span>|<span data-ttu-id="c91c1-133">sample-graph</span><span class="sxs-lookup"><span data-stu-id="c91c1-133">sample-graph</span></span>|<span data-ttu-id="c91c1-134">az új diagram hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="c91c1-134">hello ID for your new graph.</span></span> <span data-ttu-id="c91c1-135">Graph nevek rendelkezik hello azonos követelmények karakter, adatbázis-azonosító.</span><span class="sxs-lookup"><span data-stu-id="c91c1-135">Graph names have hello same character requirements as database ids.</span></span>
    <span data-ttu-id="c91c1-136">Tárkapacitás</span><span class="sxs-lookup"><span data-stu-id="c91c1-136">Storage Capacity</span></span>| <span data-ttu-id="c91c1-137">10 GB</span><span class="sxs-lookup"><span data-stu-id="c91c1-137">10 GB</span></span>|<span data-ttu-id="c91c1-138">Hagyja hello alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="c91c1-138">Leave hello default value.</span></span> <span data-ttu-id="c91c1-139">Ez a hello tárolási kapacitás hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="c91c1-139">This is hello storage capacity of hello database.</span></span>
    <span data-ttu-id="c91c1-140">Teljesítmény</span><span class="sxs-lookup"><span data-stu-id="c91c1-140">Throughput</span></span>|<span data-ttu-id="c91c1-141">400 kérelemegység</span><span class="sxs-lookup"><span data-stu-id="c91c1-141">400 RUs</span></span>|<span data-ttu-id="c91c1-142">Hagyja hello alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="c91c1-142">Leave hello default value.</span></span> <span data-ttu-id="c91c1-143">Legfeljebb hello átviteli később Ha azt szeretné, hogy tooreduce késés.</span><span class="sxs-lookup"><span data-stu-id="c91c1-143">You can scale up hello throughput later if you want tooreduce latency.</span></span>
    <span data-ttu-id="c91c1-144">Partíciókulcs</span><span class="sxs-lookup"><span data-stu-id="c91c1-144">Partition key</span></span>|<span data-ttu-id="c91c1-145">Hagyja üresen</span><span class="sxs-lookup"><span data-stu-id="c91c1-145">Leave blank</span></span>|<span data-ttu-id="c91c1-146">Hello célja a gyors üzembe helyezés hagyja üresen hello partíciós kulcs.</span><span class="sxs-lookup"><span data-stu-id="c91c1-146">For hello purpose of this quickstart, leave hello partition key blank.</span></span>

3. <span data-ttu-id="c91c1-147">Amikor hello űrlap ki van töltve, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="c91c1-147">Once hello form is filled out, click **OK**.</span></span>

## <a name="clone-hello-sample-application"></a><span data-ttu-id="c91c1-148">Klónozza a mintaalkalmazást hello</span><span class="sxs-lookup"><span data-stu-id="c91c1-148">Clone hello sample application</span></span>

<span data-ttu-id="c91c1-149">Most tegyük klónozni egy grafikonon alkalmazást a githubból, állítsa be a hello kapcsolati karakterláncot, és futtassa azt.</span><span class="sxs-lookup"><span data-stu-id="c91c1-149">Now let's clone a graph app from github, set hello connection string, and run it.</span></span> <span data-ttu-id="c91c1-150">Ön meg, milyen egyszerűen adatokkal toowork programozott módon.</span><span class="sxs-lookup"><span data-stu-id="c91c1-150">You see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="c91c1-151">Nyisson meg egy git terminálablakot, például a git bash eszközt, és `cd` tooa munkakönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="c91c1-151">Open a git terminal window, such as git bash, and `cd` tooa working directory.</span></span>  

2. <span data-ttu-id="c91c1-152">Futtassa a következő parancs tooclone hello minta tárház hello.</span><span class="sxs-lookup"><span data-stu-id="c91c1-152">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-java-getting-started.git
    ```

## <a name="review-hello-code"></a><span data-ttu-id="c91c1-153">Tekintse át a hello kódot</span><span class="sxs-lookup"><span data-stu-id="c91c1-153">Review hello code</span></span>

<span data-ttu-id="c91c1-154">Most Meggyőződünk arról, mi történik a hello app gyors áttekintése.</span><span class="sxs-lookup"><span data-stu-id="c91c1-154">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="c91c1-155">Nyissa meg hello `Program.java` hello \src\GetStarted mappából fájlt, és ezek a sorok, a kód található.</span><span class="sxs-lookup"><span data-stu-id="c91c1-155">Open hello `Program.java` file from hello \src\GetStarted folder and find these lines of code.</span></span> 

* <span data-ttu-id="c91c1-156">hello Gremlin `Client` inicializálva van a hello konfigurációból `src/remote.yaml`.</span><span class="sxs-lookup"><span data-stu-id="c91c1-156">hello Gremlin `Client` is initialized from hello configuration in `src/remote.yaml`.</span></span>

    ```java
    cluster = Cluster.build(new File("src/remote.yaml")).create();
    ...
    client = cluster.connect();
    ```

* <span data-ttu-id="c91c1-157">Hello Gremlin lépések egy sorozatát végrehajtása `client.submit` metódust.</span><span class="sxs-lookup"><span data-stu-id="c91c1-157">A series of Gremlin steps are executed using hello `client.submit` method.</span></span>

    ```java
    ResultSet results = client.submit(gremlin);

    CompletableFuture<List<Result>> completableFutureResults = results.all();
    List<Result> resultList = completableFutureResults.get();

    for (Result result : resultList) {
        System.out.println(result.toString());
    }
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="c91c1-158">A kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="c91c1-158">Update your connection string</span></span>

1. <span data-ttu-id="c91c1-159">Nyissa meg hello src/remote.yaml fájlt.</span><span class="sxs-lookup"><span data-stu-id="c91c1-159">Open hello src/remote.yaml file.</span></span> 

3. <span data-ttu-id="c91c1-160">Töltse ki a *állomások*, *felhasználónév*, és *jelszó* hello src/remote.yaml fájl értékeit.</span><span class="sxs-lookup"><span data-stu-id="c91c1-160">Fill in your *hosts*, *username*, and *password* values in hello src/remote.yaml file.</span></span> <span data-ttu-id="c91c1-161">hello részeinek hello-beállítások nem kell módosítani toobe.</span><span class="sxs-lookup"><span data-stu-id="c91c1-161">hello rest of hello settings do not need toobe changed.</span></span>

    <span data-ttu-id="c91c1-162">Beállítás</span><span class="sxs-lookup"><span data-stu-id="c91c1-162">Setting</span></span>|<span data-ttu-id="c91c1-163">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="c91c1-163">Suggested value</span></span>|<span data-ttu-id="c91c1-164">Leírás</span><span class="sxs-lookup"><span data-stu-id="c91c1-164">Description</span></span>
    ---|---|---
    <span data-ttu-id="c91c1-165">Hosts</span><span class="sxs-lookup"><span data-stu-id="c91c1-165">Hosts</span></span>|<span data-ttu-id="c91c1-166">[***.graphs.azure.com]</span><span class="sxs-lookup"><span data-stu-id="c91c1-166">[***.graphs.azure.com]</span></span>|<span data-ttu-id="c91c1-167">Tekintse meg a táblázat utáni hello képernyőképet.</span><span class="sxs-lookup"><span data-stu-id="c91c1-167">See hello screenshot following this table.</span></span> <span data-ttu-id="c91c1-168">Ez az érték hello Gremlin URI azonosítóját az Azure-portálon, a hello záró szögletes zárójelben hello hello áttekintése lapon: 443 / eltávolított.</span><span class="sxs-lookup"><span data-stu-id="c91c1-168">This value is hello Gremlin URI value on hello Overview page of hello Azure portal, in square brackets, with hello trailing :443/ removed.</span></span><br><br><span data-ttu-id="c91c1-169">Ez az érték is lekérhetők hello kulcsok lapon hello URI érték használatával https:// eltávolítása, dokumentumok toographs módosítása és eltávolítása a hello záró: 443 /.</span><span class="sxs-lookup"><span data-stu-id="c91c1-169">This value can also be retrieved from hello Keys tab, using hello URI value by removing https://, changing documents toographs, and removing hello trailing :443/.</span></span>
    <span data-ttu-id="c91c1-170">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="c91c1-170">Username</span></span>|<span data-ttu-id="c91c1-171">/dbs/sample-database/colls/sample-graph</span><span class="sxs-lookup"><span data-stu-id="c91c1-171">/dbs/sample-database/colls/sample-graph</span></span>|<span data-ttu-id="c91c1-172">erőforrás hello űrlap hello `/dbs/<db>/colls/<coll>` ahol `<db>` a meglévő adatbázis neve és `<coll>` a meglévő gyűjtemény neve.</span><span class="sxs-lookup"><span data-stu-id="c91c1-172">hello resource of hello form `/dbs/<db>/colls/<coll>` where `<db>` is your existing database name and `<coll>` is your existing collection name.</span></span>
    <span data-ttu-id="c91c1-173">Jelszó</span><span class="sxs-lookup"><span data-stu-id="c91c1-173">Password</span></span>|<span data-ttu-id="c91c1-174">*Az Ön elsődleges főkulcsa*</span><span class="sxs-lookup"><span data-stu-id="c91c1-174">*Your primary master key*</span></span>|<span data-ttu-id="c91c1-175">Tekintse meg a táblázatot követő hello második képernyőképet.</span><span class="sxs-lookup"><span data-stu-id="c91c1-175">See hello second screenshot following this table.</span></span> <span data-ttu-id="c91c1-176">Ezt az értéket az elsődleges kulcs, és kérhetnek le hello kulcsok lapján hello Azure-portálon, hello elsődleges kulcs mezőjében.</span><span class="sxs-lookup"><span data-stu-id="c91c1-176">This value is your primary key, which you can retrieve from hello Keys page of hello Azure portal, in hello Primary Key box.</span></span> <span data-ttu-id="c91c1-177">Másolja a hello Másolás gombra kattintva hello mező hello jobb oldalán a hello értékét.</span><span class="sxs-lookup"><span data-stu-id="c91c1-177">Copy hello value using hello copy button on hello right side of hello box.</span></span>

    <span data-ttu-id="c91c1-178">Hello állomások értékhez, másolja a hello **Gremlin URI** hello értéket **áttekintése** lap.</span><span class="sxs-lookup"><span data-stu-id="c91c1-178">For hello Hosts value, copy hello **Gremlin URI** value from hello **Overview** page.</span></span> <span data-ttu-id="c91c1-179">Ha üres, tudnivalókat hello tábla létrehozása hello Gremlin URI hello kulcsok paneljén megelőző hello hello állomások sorában.</span><span class="sxs-lookup"><span data-stu-id="c91c1-179">If it's empty, see hello instructions in hello Hosts row in hello preceding table about creating hello Gremlin URI from hello Keys blade.</span></span>
<span data-ttu-id="c91c1-180">![Megtekintése és másolása hello Gremlin URI érték hello áttekintése lapon a hello Azure-portálon](./media/create-graph-java/gremlin-uri.png)</span><span class="sxs-lookup"><span data-stu-id="c91c1-180">![View and copy hello Gremlin URI value on hello Overview page in hello Azure portal](./media/create-graph-java/gremlin-uri.png)</span></span>

    <span data-ttu-id="c91c1-181">Hello jelszó értékét, másolja a hello **elsődleges kulcs** a hello **kulcsok** panel: ![megtekintése és másolása az elsődleges kulcs hello Azure-portálon, a kulcsok lap](./media/create-graph-java/keys.png)</span><span class="sxs-lookup"><span data-stu-id="c91c1-181">For hello Password value, copy hello **Primary key** from hello **Keys** blade: ![View and copy your primary key in hello Azure portal, Keys page](./media/create-graph-java/keys.png)</span></span>

## <a name="run-hello-console-app"></a><span data-ttu-id="c91c1-182">Hello konzol alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="c91c1-182">Run hello console app</span></span>

1. <span data-ttu-id="c91c1-183">A hello git terminálablakot `cd` toohello azure-cosmos-db-graph-java-getting-started mappa.</span><span class="sxs-lookup"><span data-stu-id="c91c1-183">In hello git terminal window, `cd` toohello azure-cosmos-db-graph-java-getting-started folder.</span></span>

2. <span data-ttu-id="c91c1-184">Írja be a hello git terminálablakot, `mvn package` tooinstall hello szükséges Java-csomagok.</span><span class="sxs-lookup"><span data-stu-id="c91c1-184">In hello git terminal window, type `mvn package` tooinstall hello required Java packages.</span></span>

3. <span data-ttu-id="c91c1-185">Hello git terminál-ablakban futtassa `mvn exec:java -D exec.mainClass=GetStarted.Program` a hello terminálablakot toostart a Java-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c91c1-185">In hello git terminal window, run `mvn exec:java -D exec.mainClass=GetStarted.Program` in hello terminal window toostart your Java application.</span></span>

<span data-ttu-id="c91c1-186">hello terminálablakot toohello graph hozzáadott hello csúcsban jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="c91c1-186">hello terminal window displays hello vertices being added toohello graph.</span></span> <span data-ttu-id="c91c1-187">Hello program befejeztét követően az internetböngésző kapcsoló hátsó toohello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="c91c1-187">Once hello program completes, switch back toohello Azure portal in your internet browser.</span></span> 

<a id="add-sample-data"></a>
## <a name="review-and-add-sample-data"></a><span data-ttu-id="c91c1-188">Áttekintés és mintaadatok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c91c1-188">Review and add sample data</span></span>

<span data-ttu-id="c91c1-189">Ezután térjen vissza tooData Explorer és hello csúcsban toohello graph hozzá, és adja hozzá a további adatok pontokat.</span><span class="sxs-lookup"><span data-stu-id="c91c1-189">You can now go back tooData Explorer and see hello vertices added toohello graph, and add additional data points.</span></span>

1. <span data-ttu-id="c91c1-190">Az adatok Explorerben bontsa ki a hello **-mintaadatbázist**/**minta-grafikon**, kattintson **Graph**, és kattintson a **Szűrés**.</span><span class="sxs-lookup"><span data-stu-id="c91c1-190">In Data Explorer, expand hello **sample-database**/**sample-graph**, click **Graph**, and then click **Apply Filter**.</span></span> 

   ![Hozzon létre új dokumentumok az adatkezelő a hello Azure-portálon](./media/create-graph-java/azure-cosmosdb-data-explorer-expanded.png)

2. <span data-ttu-id="c91c1-192">A hello **eredmények** listában, figyelje meg, hogy hozzá toohello graph hello új felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="c91c1-192">In hello **Results** list, notice hello new users added toohello graph.</span></span> <span data-ttu-id="c91c1-193">Válassza ki **ben** és figyelje meg, hogy csatlakozott-e általa toorobin.</span><span class="sxs-lookup"><span data-stu-id="c91c1-193">Select **ben** and notice that he's connected toorobin.</span></span> <span data-ttu-id="c91c1-194">Hello graph explorer hello csúcsban helyezhetik, nagyítás növelésére és csökkentésére, és bontsa ki a hello graph explorer felület hello mérete.</span><span class="sxs-lookup"><span data-stu-id="c91c1-194">You can move hello vertices around on hello graph explorer, zoom in and out, and expand hello size of hello graph explorer surface.</span></span> 

   ![Új csúcsban hello Graph Data Explorer a hello Azure-portálon](./media/create-graph-java/azure-cosmosdb-graph-explorer-new.png)

3. <span data-ttu-id="c91c1-196">Adjunk néhány új felhasználók toohello graph hello Data Explorer használatával.</span><span class="sxs-lookup"><span data-stu-id="c91c1-196">Let's add a few new users toohello graph using hello Data Explorer.</span></span> <span data-ttu-id="c91c1-197">Kattintson a hello **új csúcspont** gomb tooadd adatok tooyour grafikon.</span><span class="sxs-lookup"><span data-stu-id="c91c1-197">Click hello **New Vertex** button tooadd data tooyour graph.</span></span>

   ![Hozzon létre új dokumentumok az adatkezelő a hello Azure-portálon](./media/create-graph-java/azure-cosmosdb-data-explorer-new-vertex.png)

4. <span data-ttu-id="c91c1-199">Adjon meg egy címkét a *személy* írja be a következő kulcsok és értékek toocreate hello első csúcspont hello Graph hello.</span><span class="sxs-lookup"><span data-stu-id="c91c1-199">Enter a label of *person* then enter hello following keys and values toocreate hello first vertex in hello graph.</span></span> <span data-ttu-id="c91c1-200">Egyedi tulajdonságokat hozhat létre a gráfban található minden egyes személy számára.</span><span class="sxs-lookup"><span data-stu-id="c91c1-200">Notice that you can create unique properties for each person in your graph.</span></span> <span data-ttu-id="c91c1-201">Csak a hello azonosító kulcsot meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="c91c1-201">Only hello id key is required.</span></span>

    <span data-ttu-id="c91c1-202">kulcs</span><span class="sxs-lookup"><span data-stu-id="c91c1-202">key</span></span>|<span data-ttu-id="c91c1-203">érték</span><span class="sxs-lookup"><span data-stu-id="c91c1-203">value</span></span>|<span data-ttu-id="c91c1-204">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="c91c1-204">Notes</span></span>
    ----|----|----
    <span data-ttu-id="c91c1-205">id</span><span class="sxs-lookup"><span data-stu-id="c91c1-205">id</span></span>|<span data-ttu-id="c91c1-206">ashley</span><span class="sxs-lookup"><span data-stu-id="c91c1-206">ashley</span></span>|<span data-ttu-id="c91c1-207">hello hello csúcspont egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="c91c1-207">hello unique identifier for hello vertex.</span></span> <span data-ttu-id="c91c1-208">Ha nem ad meg azonosítót, a rendszer létrehoz egyet.</span><span class="sxs-lookup"><span data-stu-id="c91c1-208">If you don't specify an id, one is generated for you.</span></span>
    <span data-ttu-id="c91c1-209">gender</span><span class="sxs-lookup"><span data-stu-id="c91c1-209">gender</span></span>|<span data-ttu-id="c91c1-210">female</span><span class="sxs-lookup"><span data-stu-id="c91c1-210">female</span></span>| 
    <span data-ttu-id="c91c1-211">tech</span><span class="sxs-lookup"><span data-stu-id="c91c1-211">tech</span></span> | <span data-ttu-id="c91c1-212">java</span><span class="sxs-lookup"><span data-stu-id="c91c1-212">java</span></span> | 

    > [!NOTE]
    > <span data-ttu-id="c91c1-213">Ebben a rövid útmutatóban egy nem particionált gyűjteményt hozunk létre.</span><span class="sxs-lookup"><span data-stu-id="c91c1-213">In this quickstart we create a non-partitioned collection.</span></span> <span data-ttu-id="c91c1-214">Azonban ha egy particionált gyűjtemény hoz létre a partíciós kulcs megadása hello webhelycsoport létrehozása során, akkor szüksége tooinclude hello partíciós kulcs lesz a kulcs minden új csomópont.</span><span class="sxs-lookup"><span data-stu-id="c91c1-214">However, if you create a partitioned collection by specifying a partition key during hello collection creation, then you need tooinclude hello partition key as a key in each new vertex.</span></span> 

5. <span data-ttu-id="c91c1-215">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="c91c1-215">Click **OK**.</span></span> <span data-ttu-id="c91c1-216">Szükség lehet tooexpand a képernyő toosee **OK** a hello hello képernyő aljára.</span><span class="sxs-lookup"><span data-stu-id="c91c1-216">You may need tooexpand your screen toosee **OK** on hello bottom of hello screen.</span></span>

6. <span data-ttu-id="c91c1-217">Kattintson ismét az **Új csúcspont** lehetőségre, és adjon hozzá még egy új felhasználót.</span><span class="sxs-lookup"><span data-stu-id="c91c1-217">Click **New Vertex** again and add an additional new user.</span></span> <span data-ttu-id="c91c1-218">Adjon meg egy címkét a *személy* majd írja be a hello következő kulcsokat és értékeket:</span><span class="sxs-lookup"><span data-stu-id="c91c1-218">Enter a label of *person* then enter hello following keys and values:</span></span>

    <span data-ttu-id="c91c1-219">kulcs</span><span class="sxs-lookup"><span data-stu-id="c91c1-219">key</span></span>|<span data-ttu-id="c91c1-220">érték</span><span class="sxs-lookup"><span data-stu-id="c91c1-220">value</span></span>|<span data-ttu-id="c91c1-221">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="c91c1-221">Notes</span></span>
    ----|----|----
    <span data-ttu-id="c91c1-222">id</span><span class="sxs-lookup"><span data-stu-id="c91c1-222">id</span></span>|<span data-ttu-id="c91c1-223">rakesh</span><span class="sxs-lookup"><span data-stu-id="c91c1-223">rakesh</span></span>|<span data-ttu-id="c91c1-224">hello hello csúcspont egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="c91c1-224">hello unique identifier for hello vertex.</span></span> <span data-ttu-id="c91c1-225">Ha nem ad meg azonosítót, a rendszer létrehoz egyet.</span><span class="sxs-lookup"><span data-stu-id="c91c1-225">If you don't specify an id, one is generated for you.</span></span>
    <span data-ttu-id="c91c1-226">gender</span><span class="sxs-lookup"><span data-stu-id="c91c1-226">gender</span></span>|<span data-ttu-id="c91c1-227">male</span><span class="sxs-lookup"><span data-stu-id="c91c1-227">male</span></span>| 
    <span data-ttu-id="c91c1-228">school</span><span class="sxs-lookup"><span data-stu-id="c91c1-228">school</span></span>|<span data-ttu-id="c91c1-229">MIT</span><span class="sxs-lookup"><span data-stu-id="c91c1-229">MIT</span></span>| 

7. <span data-ttu-id="c91c1-230">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="c91c1-230">Click **OK**.</span></span> 

8. <span data-ttu-id="c91c1-231">Kattintson a **szűrés** hello alapértelmezett `g.V()` szűrő.</span><span class="sxs-lookup"><span data-stu-id="c91c1-231">Click **Apply Filter** with hello default `g.V()` filter.</span></span> <span data-ttu-id="c91c1-232">Hello felhasználói hello most megjelenítése **eredmények** listája.</span><span class="sxs-lookup"><span data-stu-id="c91c1-232">All of hello users now show in hello **Results** list.</span></span> <span data-ttu-id="c91c1-233">További adatok hozzáadása, használhatja az eredményeket szűrők toolimit.</span><span class="sxs-lookup"><span data-stu-id="c91c1-233">As you add more data, you can use filters toolimit your results.</span></span> <span data-ttu-id="c91c1-234">Alapértelmezés szerint adatkezelő használja `g.V()` tooretrieve egy grafikonon, de minden csúcsban módosíthatja, hogy különböző tooa [graph lekérdezés](tutorial-query-graph.md), például a `g.V().count()`, tooreturn hello Graph JSON formátumban minden hello csúcsban számát.</span><span class="sxs-lookup"><span data-stu-id="c91c1-234">By default, Data Explorer uses `g.V()` tooretrieve all vertices in a graph, but you can change that tooa different [graph query](tutorial-query-graph.md), such as `g.V().count()`, tooreturn a count of all hello vertices in hello graph in JSON format.</span></span>

9. <span data-ttu-id="c91c1-235">Most már összekapcsolhatjuk a rakesh és az ashley elemet.</span><span class="sxs-lookup"><span data-stu-id="c91c1-235">Now we can connect rakesh and ashley.</span></span> <span data-ttu-id="c91c1-236">Győződjön meg arról **Pálfi** a kiválasztott hello **eredmények** listában, majd kattintson a Szerkesztés gomb hello tovább túl**célok** a jobb alsó.</span><span class="sxs-lookup"><span data-stu-id="c91c1-236">Ensure **ashley** in selected in hello **Results** list, then click hello edit button next too**Targets** on lower right side.</span></span> <span data-ttu-id="c91c1-237">Szükség lehet toowiden az ablak toosee hello **tulajdonságok** területen.</span><span class="sxs-lookup"><span data-stu-id="c91c1-237">You may need toowiden your window toosee hello **Properties** area.</span></span>

   ![Hello célja egy grafikonon csúcspont módosítása](./media/create-graph-java/azure-cosmosdb-data-explorer-edit-target.png)

10. <span data-ttu-id="c91c1-239">A hello **cél** mezőbe írja be *rakesh*, és a hello **peremhálózati címke** mezőbe írja be *tudja*, és kattintson a hello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="c91c1-239">In hello **Target** box type *rakesh*, and in hello **Edge label** box type *knows*, and then click hello check box.</span></span>

   ![ashley és rakesh közötti kapcsolat hozzáadása az Adatkezelőben](./media/create-graph-java/azure-cosmosdb-data-explorer-set-target.png)

11. <span data-ttu-id="c91c1-241">Immár **rakesh** a hello eredmények listájában, és ellenőrizze, hogy Pálfi és rakesh csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="c91c1-241">Now select **rakesh** from hello results list and see that ashley and rakesh are connected.</span></span> 

   ![Két összekapcsolt csúcspont az Adatkezelőben](./media/create-graph-java/azure-cosmosdb-graph-explorer.png)

    <span data-ttu-id="c91c1-243">Is használhatja adatkezelő toocreate tárolt eljárások, felhasználó által megadott függvények és eseményindítók tooperform kiszolgálóoldali üzleti logikát is mint méretezési átviteli sebesség.</span><span class="sxs-lookup"><span data-stu-id="c91c1-243">You can also use Data Explorer toocreate stored procedures, UDFs, and triggers tooperform server-side business logic as well as scale throughput.</span></span> <span data-ttu-id="c91c1-244">Adatkezelő mutatja meg az összes hello beépített programozott adatelérési hello API-k érhető el, de hello Azure-portálon található egyszerű a hozzáférés tooyour adatokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="c91c1-244">Data Explorer exposes all of hello built-in programmatic data access available in hello APIs, but provides easy access tooyour data in hello Azure portal.</span></span>



## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="c91c1-245">Tekintse át a szolgáltatásiszint-szerződések a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c91c1-245">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="c91c1-246">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="c91c1-246">Clean up resources</span></span>

<span data-ttu-id="c91c1-247">Toocontinue toouse az alkalmazás nem fog, ha törli az összes erőforrást hozta létre a gyors üzembe helyezés hello az Azure-portálon az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c91c1-247">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span> 

1. <span data-ttu-id="c91c1-248">A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="c91c1-248">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="c91c1-249">Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="c91c1-249">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c91c1-250">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c91c1-250">Next steps</span></span>

<span data-ttu-id="c91c1-251">A gyors üzembe helyezés hogy megtanulta, hogyan toocreate Azure Cosmos DB adatait, hozzon létre egy grafikonon hello adatkezelő használatával, és futtassa az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c91c1-251">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, create a graph using hello Data Explorer, and run an app.</span></span> <span data-ttu-id="c91c1-252">Most már készen áll arra, hogy a Gremlin használatával összetettebb lekérdezéseket hozzon létre és hatékony gráfbejárási logikákat implementáljon.</span><span class="sxs-lookup"><span data-stu-id="c91c1-252">You can now build more complex queries and implement powerful graph traversal logic using Gremlin.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="c91c1-253">Lekérdezés a Gremlin használatával</span><span class="sxs-lookup"><span data-stu-id="c91c1-253">Query using Gremlin</span></span>](tutorial-query-graph.md)

