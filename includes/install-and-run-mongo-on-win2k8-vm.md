<span data-ttu-id="5a3eb-101">Kövesse az alábbi lépéseket tooinstall, és futtassa a MongoDB Windows Server rendszerű virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-101">Follow these steps tooinstall and run MongoDB on a virtual machine running Windows Server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5a3eb-102">Alapértelmezés szerint nem engedélyezettek a MongoDB biztonsági funkciók, például hitelesítés és az IP-cím kötés.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-102">MongoDB security features, such as authentication and IP address binding, are not enabled by default.</span></span> <span data-ttu-id="5a3eb-103">Biztonsági funkciókat engedélyezni kell a MongoDB tooa éles környezetben üzembe helyezése előtt.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-103">Security features should be enabled before deploying MongoDB tooa production environment.</span></span>  <span data-ttu-id="5a3eb-104">További információkért lásd: [biztonsági és hitelesítési](http://www.mongodb.org/display/DOCS/Security+and+Authentication).</span><span class="sxs-lookup"><span data-stu-id="5a3eb-104">For more information, see [Security and Authentication](http://www.mongodb.org/display/DOCS/Security+and+Authentication).</span></span>
>
>

1. <span data-ttu-id="5a3eb-105">A távoli asztal toohello virtuális gép csatlakoztatása után nyissa meg az Internet Explorer hello **Start** menü hello virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-105">After you've connected toohello virtual machine using Remote Desktop, open Internet Explorer from hello **Start** menu on hello virtual machine.</span></span>
2. <span data-ttu-id="5a3eb-106">Jelölje be hello **eszközök** hello jobb felső sarkában található gombra.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-106">Select hello **Tools** button in hello upper right corner.</span></span>  <span data-ttu-id="5a3eb-107">A **Internetbeállítások**, jelölje be hello **biztonsági** lapra, majd válassza ki a hello **megbízható helyek** ikonra, végül kattintson a hello **helyek** jelölt gombra.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-107">In **Internet Options**, select hello **Security** tab, and then select hello **Trusted Sites** icon, and finally click hello **Sites** button.</span></span> <span data-ttu-id="5a3eb-108">Adja hozzá *https://\*. mongodb.org* toohello a megbízható helyek listájához.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-108">Add *https://\*.mongodb.org* toohello list of trusted sites.</span></span>
3. <span data-ttu-id="5a3eb-109">Nyissa meg túl[letölti – MongoDB](https://www.mongodb.com/download-center#community).</span><span class="sxs-lookup"><span data-stu-id="5a3eb-109">Go too[Downloads - MongoDB](https://www.mongodb.com/download-center#community).</span></span>
4. <span data-ttu-id="5a3eb-110">Hello található **jelenlegi stabil kiadásban** a **közösségi Server**, jelölje be legújabb hello **64 bites** változatban a Windows hello.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-110">Find hello **Current Stable Release** of **Community Server**, select hello latest **64-bit** version in hello Windows column.</span></span> <span data-ttu-id="5a3eb-111">Töltse le, majd futtassa a hello MSI telepítő.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-111">Download, then run hello MSI installer.</span></span>
5. <span data-ttu-id="5a3eb-112">MongoDB telepítése általában a C:\Program Files\MongoDB.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-112">MongoDB is typically installed in C:\Program Files\MongoDB.</span></span> <span data-ttu-id="5a3eb-113">Keresse meg a környezeti változók hello asztalán, és hello MongoDB bináris fájlok elérési útja toohello ELÉRÉSI változó hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-113">Search for Environment Variables on hello desktop and add hello MongoDB binaries path toohello PATH variable.</span></span> <span data-ttu-id="5a3eb-114">Például előfordulhat hello bináris fájljai a C:\Program Files\MongoDB\Server\3.4\bin a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-114">For example, you might find hello binaries at C:\Program Files\MongoDB\Server\3.4\bin on your machine.</span></span>
6. <span data-ttu-id="5a3eb-115">MongoDB adatainak és naplókönyvtárainak könyvtárak létrehozása a hello adatlemez (például meghajtó **F:**) hello előző lépésekben létrehozott.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-115">Create MongoDB data and log directories in hello data disk (such as drive **F:**) you created in hello preceding steps.</span></span> <span data-ttu-id="5a3eb-116">A **Start**, jelölje be **parancssor** tooopen egy parancssori ablakot.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-116">From **Start**, select **Command Prompt** tooopen a command prompt window.</span></span>  <span data-ttu-id="5a3eb-117">Típus:</span><span class="sxs-lookup"><span data-stu-id="5a3eb-117">Type:</span></span>

        C:\> F:
        F:\> mkdir \MongoData
        F:\> mkdir \MongoLogs
7. <span data-ttu-id="5a3eb-118">toorun hello adatbázis futtatásához:</span><span class="sxs-lookup"><span data-stu-id="5a3eb-118">toorun hello database, run:</span></span>

        F:\> C:
        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log

    <span data-ttu-id="5a3eb-119">Összes naplózási üzenetek irányított toohello *F:\MongoLogs\mongolog.log* mongod.exe server elindul, és Adatbázisnapló-fájlok preallocates fájlt.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-119">All log messages are directed toohello *F:\MongoLogs\mongolog.log* file as mongod.exe server starts and preallocates journal files.</span></span> <span data-ttu-id="5a3eb-120">Ez hello Adatbázisnapló-fájlok a MongoDB toopreallocate néhány percet igénybe vehet, és indítsa el a kapcsolatfigyelést.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-120">It may take several minutes for MongoDB toopreallocate hello journal files and start listening for connections.</span></span> <span data-ttu-id="5a3eb-121">hello parancssor marad összpontosítanak ezt a feladatot, a MongoDB-példány futtatása közben.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-121">hello command prompt stays focused on this task while your MongoDB instance is running.</span></span>
8. <span data-ttu-id="5a3eb-122">toostart hello MongoDB felügyeleti rendszerhéjat, nyisson meg egy másik parancs ablakot a **Start** és hello típus a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="5a3eb-122">toostart hello MongoDB administrative shell, open another command window from **Start** and type hello following commands:</span></span>

        C:\> cd \my_mongo_dir\bin  
        C:\my_mongo_dir\bin> mongo  
        >db  
        test
        > db.foo.insert( { a : 1 } )  
        > db.foo.find()  
        { _id : ..., a : 1 }  
        > show dbs  
        ...  
        > show collections  
        ...  
        > help  

    <span data-ttu-id="5a3eb-123">hello adatbázist hello insert hozta létre.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-123">hello database is created by hello insert.</span></span>
9. <span data-ttu-id="5a3eb-124">Másik lehetőségként telepítése mongod.exe szolgáltatásként:</span><span class="sxs-lookup"><span data-stu-id="5a3eb-124">Alternatively, you can install mongod.exe as a service:</span></span>

        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log --logappend  --install

    <span data-ttu-id="5a3eb-125">A szolgáltatást, MongoDB nevű "Mongo DB" leírását.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-125">A service is installed named MongoDB with a description of "Mongo DB".</span></span> <span data-ttu-id="5a3eb-126">Hello `--logpath` beállítás kell használt toospecify naplófájl, óta hello szolgáltatást futtató nem rendelkezik egy toodisplay parancs kimenetét.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-126">hello `--logpath` option must be used toospecify a log file, since hello running service does not have a command window toodisplay output.</span></span>  <span data-ttu-id="5a3eb-127">Hello `--logappend` beállítás megadja, hogy hello szolgáltatás újraindítását okozza-e a kimeneti tooappend toohello naplófájlokat.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-127">hello `--logappend` option specifies that a restart of hello service causes output tooappend toohello existing log file.</span></span>  <span data-ttu-id="5a3eb-128">Hello `--dbpath` beállítás hello adatkönyvtára hello helyét adja meg.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-128">hello `--dbpath` option specifies hello location of hello data directory.</span></span> <span data-ttu-id="5a3eb-129">A szolgáltatással kapcsolatos további parancssori kapcsolókat lásd: [parancssori kapcsolók szolgáltatással kapcsolatos][MongoWindowsSvcOptions].</span><span class="sxs-lookup"><span data-stu-id="5a3eb-129">For more service-related command-line options, see [Service-related command-line options][MongoWindowsSvcOptions].</span></span>

    <span data-ttu-id="5a3eb-130">toostart hello szolgáltatást, futtassa a parancsot:</span><span class="sxs-lookup"><span data-stu-id="5a3eb-130">toostart hello service, run this command:</span></span>

        C:\> net start MongoDB
10. <span data-ttu-id="5a3eb-131">Most, hogy a MongoDB telepítve van és fut, a Windows tűzfal port tooopen kell tehát távolról csatlakozhat tooMongoDB.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-131">Now that MongoDB is installed and running, you need tooopen a port in Windows Firewall so you can remotely connect tooMongoDB.</span></span>  <span data-ttu-id="5a3eb-132">A hello **Start** menü **felügyeleti eszközök** , majd **fokozott biztonságú Windows tűzfal**.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-132">From hello **Start** menu, select **Administrative Tools** and then **Windows Firewall with Advanced Security**.</span></span>
11. <span data-ttu-id="5a3eb-133">a) hello bal oldali ablaktáblában jelöljön ki **bejövő szabályok**.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-133">a) In hello left pane, select **Inbound Rules**.</span></span>  <span data-ttu-id="5a3eb-134">A hello **műveletek** ablaktáblán a jobb oldali hello válassza **új szabály létrehozása...** .</span><span class="sxs-lookup"><span data-stu-id="5a3eb-134">In hello **Actions** pane on hello right, select **New Rule...**.</span></span>

    ![A Windows tűzfal][Image1]

    <span data-ttu-id="5a3eb-136">b) a hello **új bejövő szabály varázsló**, jelölje be **Port** majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-136">b) In hello **New Inbound Rule Wizard**, select **Port** and then click **Next**.</span></span>

    ![A Windows tűzfal][Image2]

    <span data-ttu-id="5a3eb-138">c) válassza **TCP** , majd **adott helyi portok**.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-138">c) Select **TCP** and then **Specific local ports**.</span></span>  <span data-ttu-id="5a3eb-139">Adjon meg egy portot az "27017" (hello alapértelmezett portot figyel MongoDB), és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-139">Specify a port of "27017" (hello default port MongoDB listens on) and click **Next**.</span></span>

    ![A Windows tűzfal][Image3]

    <span data-ttu-id="5a3eb-141">d) válassza **hello csatlakozás engedélyezése** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-141">d) Select **Allow hello connection** and click **Next**.</span></span>

    ![A Windows tűzfal][Image4]

    <span data-ttu-id="5a3eb-143">e) kattintson **következő** újra.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-143">e) Click **Next** again.</span></span>

    ![A Windows tűzfal][Image5]

    <span data-ttu-id="5a3eb-145">f) adjon meg egy nevet hello szabályhoz, például "MongoPort", és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-145">f) Specify a name for hello rule, such as "MongoPort", and click **Finish**.</span></span>

    ![A Windows tűzfal][Image6]

12. <span data-ttu-id="5a3eb-147">Ha nem konfigurál egy olyan végpont mongodb hello virtuális gép létrehozása után, most is elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-147">If you didn't configure an endpoint for MongoDB when you created hello virtual machine, you can do it now.</span></span> <span data-ttu-id="5a3eb-148">Távolról kell hello tűzfalszabály és hello végpont toobe képes tooconnect tooMongoDB is.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-148">You need both hello firewall rule and hello endpoint toobe able tooconnect tooMongoDB remotely.</span></span>

  <span data-ttu-id="5a3eb-149">Hello Azure-portálon, kattintson **virtuális gépek (klasszikus)**, kattintson az új virtuális gép hello neve, majd **végpontok**.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-149">In hello Azure portal, click **Virtual Machines (classic)**, click hello name of your new virtual machine, and then click **Endpoints**.</span></span>

    ![Végpontok][Image7]

13. <span data-ttu-id="5a3eb-151">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-151">Click **Add**.</span></span>

14. <span data-ttu-id="5a3eb-152">"Mongo" protokoll nevű végpont hozzáadása **TCP**, és mindkét **nyilvános** és **titkos** portok készlet túl "27017".</span><span class="sxs-lookup"><span data-stu-id="5a3eb-152">Add an endpoint with name "Mongo", protocol **TCP**, and both **Public** and **Private** ports set too"27017".</span></span> <span data-ttu-id="5a3eb-153">Ez a port megnyitása lehetővé teszi, hogy a MongoDB toobe érhető el távolról.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-153">Opening this port allows MongoDB toobe accessed remotely.</span></span>

    ![Végpontok][Image9]

> [!NOTE]
> <span data-ttu-id="5a3eb-155">hello port 27017 hello alapértelmezett portot használják a MongoDB.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-155">hello port 27017 is hello default port used by MongoDB.</span></span> <span data-ttu-id="5a3eb-156">Hello megadásával módosíthatja az alapértelmezett port `--port` paraméter hello mongod.exe kiszolgáló indításakor.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-156">You can change this default port by specifying hello `--port` parameter when starting hello mongod.exe server.</span></span> <span data-ttu-id="5a3eb-157">Győződjön meg arról, hogy toogive hello ugyanazt a portszámot a hello tűzfal és az előző utasítások hello "Mongo" végpont hello.</span><span class="sxs-lookup"><span data-stu-id="5a3eb-157">Make sure toogive hello same port number in hello firewall and hello "Mongo" endpoint in hello preceding instructions.</span></span>
>
>

[MongoDownloads]: http://www.mongodb.org/downloads

[MongoWindowsSvcOptions]: http://www.mongodb.org/display/DOCS/Windows+Service


[Image1]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall1.png
[Image2]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall2.png
[Image3]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall3.png
[Image4]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall4.png
[Image5]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall5.png
[Image6]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall6.png
[Image7]: ./media/install-and-run-mongo-on-win2k8-vm/menusendpointadd.png
<!-- Removed 03/08/2017. Not in new portal. -->
<!-- [Image8]: ./media/install-and-run-mongo-on-win2k8-vm/WinVmAddEndpoint2.png
-->
[Image9]: ./media/install-and-run-mongo-on-win2k8-vm/newendpointdetails.png
