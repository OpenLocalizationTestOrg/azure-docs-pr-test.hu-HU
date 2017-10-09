---
title: a Windows Azure-ban a MongoDB aaaInstall |} Microsoft Docs
description: "Ismerje meg, hogyan tooinstall MongoDB egy Windows Server 2012 R2 rendszert futtató Azure virtuális gépen létre hello Resource Manager üzembe helyezési modellben."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 53faf630-8da5-4955-8d0b-6e829bf30cba
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: becd2c607d098e2bc806139e03f2c42f1f01f6f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-mongodb-on-a-windows-vm-in-azure"></a><span data-ttu-id="201cc-103">Telepítse és konfigurálja a Windows Azure-ban mongodb-Protokolltámogatással</span><span class="sxs-lookup"><span data-stu-id="201cc-103">Install and configure MongoDB on a Windows VM in Azure</span></span>
<span data-ttu-id="201cc-104">[MongoDB](http://www.mongodb.org) egy népszerű nyílt forráskódú, nagy teljesítményű NoSQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="201cc-104">[MongoDB](http://www.mongodb.org) is a popular open-source, high-performance NoSQL database.</span></span> <span data-ttu-id="201cc-105">Ez a cikk végigvezeti telepítése és konfigurálása a MongoDB a Windows Server 2012 R2 virtuális gépen (VM) az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="201cc-105">This article guides you through installing and configuring MongoDB on a Windows Server 2012 R2 virtual machine (VM) in Azure.</span></span> <span data-ttu-id="201cc-106">Emellett [a MongoDB telepítése egy Linux virtuális gépre az Azure-ban](../linux/install-mongodb.md).</span><span class="sxs-lookup"><span data-stu-id="201cc-106">You can also [install MongoDB on a Linux VM in Azure](../linux/install-mongodb.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="201cc-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="201cc-107">Prerequisites</span></span>
<span data-ttu-id="201cc-108">Mielőtt telepítése és konfigurálása a MongoDB, meg kell toocreate egy virtuális Gépet és, ideális esetben az adatok lemezre tooit hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="201cc-108">Before you install and configure MongoDB, you need toocreate a VM and, ideally, add a data disk tooit.</span></span> <span data-ttu-id="201cc-109">Tekintse meg a következő cikkek toocreate egy virtuális gép hello, és hozzá adatlemezt:</span><span class="sxs-lookup"><span data-stu-id="201cc-109">See hello following articles toocreate a VM and add a data disk:</span></span>

* <span data-ttu-id="201cc-110">Hozzon létre egy Windows Server virtuális gép az [hello Azure-portálon](quick-create-portal.md) vagy [Azure PowerShell](quick-create-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="201cc-110">Create a Windows Server VM using [hello Azure portal](quick-create-portal.md) or [Azure PowerShell](quick-create-powershell.md).</span></span>
* <span data-ttu-id="201cc-111">Adatok tooa Windows Server virtuális gép használatával csatolása [hello Azure-portálon](attach-managed-disk-portal.md) vagy [Azure PowerShell](attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="201cc-111">Attach a data disk tooa Windows Server VM using [hello Azure portal](attach-managed-disk-portal.md) or [Azure PowerShell](attach-disk-ps.md).</span></span>

<span data-ttu-id="201cc-112">toobegin telepítése és konfigurálása a MongoDB, [jelentkezzen be Windows Server virtuális gép tooyour](connect-logon.md) távoli asztal használatával.</span><span class="sxs-lookup"><span data-stu-id="201cc-112">toobegin installing and configuring MongoDB, [log on tooyour Windows Server VM](connect-logon.md) by using Remote Desktop.</span></span>

## <a name="install-mongodb"></a><span data-ttu-id="201cc-113">A MongoDB telepítése</span><span class="sxs-lookup"><span data-stu-id="201cc-113">Install MongoDB</span></span>
> [!IMPORTANT]
> <span data-ttu-id="201cc-114">Alapértelmezés szerint nem engedélyezettek a MongoDB biztonsági funkciók, például hitelesítés és az IP-cím kötés.</span><span class="sxs-lookup"><span data-stu-id="201cc-114">MongoDB security features, such as authentication and IP address binding, are not enabled by default.</span></span> <span data-ttu-id="201cc-115">Biztonsági funkciókat engedélyezni kell a MongoDB tooa éles környezetben üzembe helyezése előtt.</span><span class="sxs-lookup"><span data-stu-id="201cc-115">Security features should be enabled before deploying MongoDB tooa production environment.</span></span> <span data-ttu-id="201cc-116">További információkért lásd: [MongoDB biztonsági és hitelesítési](http://www.mongodb.org/display/DOCS/Security+and+Authentication).</span><span class="sxs-lookup"><span data-stu-id="201cc-116">For more information, see [MongoDB Security and Authentication](http://www.mongodb.org/display/DOCS/Security+and+Authentication).</span></span>


1. <span data-ttu-id="201cc-117">Távoli asztal használó virtuális gépek tooyour csatlakoztatása után nyissa meg az Internet Explorer hello **Start** hello VM menüjéből.</span><span class="sxs-lookup"><span data-stu-id="201cc-117">After you've connected tooyour VM using Remote Desktop, open Internet Explorer from hello **Start** menu on hello VM.</span></span>
2. <span data-ttu-id="201cc-118">Válassza ki **az ajánlott biztonsági, adatvédelmi és kompatibilitási beállítások** Ha Internet Explorer először, majd kattintson az **OK**.</span><span class="sxs-lookup"><span data-stu-id="201cc-118">Select **Use recommended security, privacy, and compatibility settings** when Internet Explorer first opens, and click **OK**.</span></span>
3. <span data-ttu-id="201cc-119">Internet Explorer fokozott biztonsági beállításai alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="201cc-119">Internet Explorer enhanced security configuration is enabled by default.</span></span> <span data-ttu-id="201cc-120">Adja hozzá az engedélyezett webhelyek hello MongoDB webhely toohello listájához:</span><span class="sxs-lookup"><span data-stu-id="201cc-120">Add hello MongoDB website toohello list of allowed sites:</span></span>
   
   * <span data-ttu-id="201cc-121">Jelölje be hello **eszközök** hello jobb felső sarokban látható ikonra.</span><span class="sxs-lookup"><span data-stu-id="201cc-121">Select hello **Tools** icon in hello upper-right corner.</span></span>
   * <span data-ttu-id="201cc-122">A **Internetbeállítások**, jelölje be hello **biztonsági** lapra, majd válassza ki a hello **megbízható helyek** ikonra.</span><span class="sxs-lookup"><span data-stu-id="201cc-122">In **Internet Options**, select hello **Security** tab, and then select hello **Trusted Sites** icon.</span></span>
   * <span data-ttu-id="201cc-123">Kattintson a hello **helyek** gombra.</span><span class="sxs-lookup"><span data-stu-id="201cc-123">Click hello **Sites** button.</span></span> <span data-ttu-id="201cc-124">Adja hozzá *https://\*. mongodb.org* toohello listáját a megbízható helyek, valamint majd Bezárás hello párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="201cc-124">Add *https://\*.mongodb.org* toohello list of trusted sites, and then close hello dialog box.</span></span>
     
     ![Internet Explorer biztonsági beállításainak konfigurálása](./media/install-mongodb/configure-internet-explorer-security.png)
4. <span data-ttu-id="201cc-126">Keresse meg a toohello [tölti le a MongoDB -](http://www.mongodb.org/downloads) (http://www.mongodb.org/downloads) lap.</span><span class="sxs-lookup"><span data-stu-id="201cc-126">Browse toohello [MongoDB - Downloads](http://www.mongodb.org/downloads) page (http://www.mongodb.org/downloads).</span></span>
5. <span data-ttu-id="201cc-127">Ha szükséges, jelölje be a hello **közösségi Server** edition és jelölje ki hello legújabb aktuális stabil verzióját a Windows Server 2008 R2 64 bites és újabb verzióihoz.</span><span class="sxs-lookup"><span data-stu-id="201cc-127">If needed, select hello **Community Server** edition and then select hello latest current stable release for Windows Server 2008 R2 64-bit and later.</span></span> <span data-ttu-id="201cc-128">toodownload hello installer, kattintson a **letöltési (msi)**.</span><span class="sxs-lookup"><span data-stu-id="201cc-128">toodownload hello installer, click **DOWNLOAD (msi)**.</span></span>
   
    ![Töltse le a MongoDB-telepítő](./media/install-mongodb/download-mongodb.png)
   
    <span data-ttu-id="201cc-130">Hello telepítő futtatásához hello letöltés befejezése után.</span><span class="sxs-lookup"><span data-stu-id="201cc-130">Run hello installer after hello download is complete.</span></span>
6. <span data-ttu-id="201cc-131">Olvassa el és fogadja el a licencszerződést hello.</span><span class="sxs-lookup"><span data-stu-id="201cc-131">Read and accept hello license agreement.</span></span> <span data-ttu-id="201cc-132">Amikor a rendszer kéri, válassza ki a **Complete** telepítése.</span><span class="sxs-lookup"><span data-stu-id="201cc-132">When you're prompted, select **Complete** install.</span></span>
7. <span data-ttu-id="201cc-133">Hello végső képernyőn kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="201cc-133">On hello final screen, click **Install**.</span></span>

## <a name="configure-hello-vm-and-mongodb"></a><span data-ttu-id="201cc-134">Virtuális gép hello és MongoDB konfigurálása</span><span class="sxs-lookup"><span data-stu-id="201cc-134">Configure hello VM and MongoDB</span></span>
1. <span data-ttu-id="201cc-135">hello MongoDB telepítési nem frissíti a hello elérésiút-változókat.</span><span class="sxs-lookup"><span data-stu-id="201cc-135">hello path variables are not updated by hello MongoDB installer.</span></span> <span data-ttu-id="201cc-136">Hello MongoDB nélkül `bin` helyre a path változóban, kell toospecify hello teljes elérési útja a MongoDB végrehajtható fájl használata során.</span><span class="sxs-lookup"><span data-stu-id="201cc-136">Without hello MongoDB `bin` location in your path variable, you need toospecify hello full path each time you use a MongoDB executable.</span></span> <span data-ttu-id="201cc-137">tooadd hello hely tooyour elérésiút-változónak:</span><span class="sxs-lookup"><span data-stu-id="201cc-137">tooadd hello location tooyour path variable:</span></span>
   
   * <span data-ttu-id="201cc-138">Kattintson a jobb gombbal hello **Start** menüben, és válassza a **rendszer**.</span><span class="sxs-lookup"><span data-stu-id="201cc-138">Right-click hello **Start** menu, and select **System**.</span></span>
   * <span data-ttu-id="201cc-139">Kattintson a **Speciális rendszerbeállítások**, és kattintson a **környezeti változók**.</span><span class="sxs-lookup"><span data-stu-id="201cc-139">Click **Advanced system settings**, and then click **Environment Variables**.</span></span>
   * <span data-ttu-id="201cc-140">A **rendszerváltozók**, jelölje be **elérési**, és kattintson a **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="201cc-140">Under **System variables**, select **Path**, and then click **Edit**.</span></span>
     
     ![Az ELÉRÉSIÚT-változókat konfigurálása](./media/install-mongodb/configure-path-variables.png)
     
     <span data-ttu-id="201cc-142">Adja hozzá a hello elérési tooyour MongoDB `bin` mappa.</span><span class="sxs-lookup"><span data-stu-id="201cc-142">Add hello path tooyour MongoDB `bin` folder.</span></span> <span data-ttu-id="201cc-143">MongoDB telepítése általában a *C:\Program Files\MongoDB*.</span><span class="sxs-lookup"><span data-stu-id="201cc-143">MongoDB is typically installed in *C:\Program Files\MongoDB*.</span></span> <span data-ttu-id="201cc-144">Ellenőrizze a virtuális gép hello telepítési útvonalán.</span><span class="sxs-lookup"><span data-stu-id="201cc-144">Verify hello installation path on your VM.</span></span> <span data-ttu-id="201cc-145">hello következő példakóddal felveheti a hello alapértelmezett MongoDB telepítési helye toohello `PATH` változó:</span><span class="sxs-lookup"><span data-stu-id="201cc-145">hello following example adds hello default MongoDB install location toohello `PATH` variable:</span></span>
     
     ```
     ;C:\Program Files\MongoDB\Server\3.2\bin
     ```
     
     > [!NOTE]
     > <span data-ttu-id="201cc-146">Lehet, hogy tooadd hello vezető pontosvessző (`;`), hogy a hely tooyour hozzáadni kívánt tooindicate `PATH` változó.</span><span class="sxs-lookup"><span data-stu-id="201cc-146">Be sure tooadd hello leading semicolon (`;`) tooindicate that you are adding a location tooyour `PATH` variable.</span></span>

2. <span data-ttu-id="201cc-147">Az adatok a lemezen létrehozni a MongoDB adatainak és naplókönyvtárainak könyvtárak.</span><span class="sxs-lookup"><span data-stu-id="201cc-147">Create MongoDB data and log directories on your data disk.</span></span> <span data-ttu-id="201cc-148">A hello **Start** menüjében válassza **parancssor**.</span><span class="sxs-lookup"><span data-stu-id="201cc-148">From hello **Start** menu, select **Command Prompt**.</span></span> <span data-ttu-id="201cc-149">a következő példák hello hello könyvtárak létrehozása F: meghajtón</span><span class="sxs-lookup"><span data-stu-id="201cc-149">hello following examples create hello directories on drive F:</span></span>
   
    ```
    mkdir F:\MongoData
    mkdir F:\MongoLogs
    ```
3. <span data-ttu-id="201cc-150">A MongoDB-példány kezdődnie hello a következő parancsot, hello elérési tooyour adatok beállítása, és ennek megfelelően jelentkezzen könyvtárak:</span><span class="sxs-lookup"><span data-stu-id="201cc-150">Start a MongoDB instance with hello following command, adjusting hello path tooyour data and log directories accordingly:</span></span>
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log
    ```
   
    <span data-ttu-id="201cc-151">Ez hello Adatbázisnapló-fájlok a MongoDB tooallocate néhány percet igénybe vehet, és indítsa el a kapcsolatfigyelést.</span><span class="sxs-lookup"><span data-stu-id="201cc-151">It may take several minutes for MongoDB tooallocate hello journal files and start listening for connections.</span></span> <span data-ttu-id="201cc-152">Összes naplózási üzenetek irányított toohello *F:\MongoLogs\mongolog.log* tárolási `mongod.exe` server elindul, és lefoglalja a Adatbázisnapló-fájlok.</span><span class="sxs-lookup"><span data-stu-id="201cc-152">All log messages are directed toohello *F:\MongoLogs\mongolog.log* file as `mongod.exe` server starts and allocates journal files.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="201cc-153">hello parancssor marad összpontosítanak ezt a feladatot, a MongoDB-példány futtatása közben.</span><span class="sxs-lookup"><span data-stu-id="201cc-153">hello command prompt stays focused on this task while your MongoDB instance is running.</span></span> <span data-ttu-id="201cc-154">Hello parancssori ablak nyitva toocontinue MongoDB futtató hagyja.</span><span class="sxs-lookup"><span data-stu-id="201cc-154">Leave hello command prompt window open toocontinue running MongoDB.</span></span> <span data-ttu-id="201cc-155">Vagy a MongoDB telepítése a következő lépés hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="201cc-155">Or, install MongoDB as service, as detailed in hello next step.</span></span>

4. <span data-ttu-id="201cc-156">Robusztusabb MongoDB élmény érdekében telepítse a hello `mongod.exe` szolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="201cc-156">For a more robust MongoDB experience, install hello `mongod.exe` as a service.</span></span> <span data-ttu-id="201cc-157">Szolgáltatás létrehozása azt jelenti, hogy nem kell tooleave toouse MongoDB minden alkalommal fut egy parancssort.</span><span class="sxs-lookup"><span data-stu-id="201cc-157">Creating a service means you don't need tooleave a command prompt running each time you want toouse MongoDB.</span></span> <span data-ttu-id="201cc-158">Hello szolgáltatás létrehozása az alábbiak szerint elérési tooyour adatainak és naplókönyvtárainak könyvtárak hello beállítása ennek megfelelően:</span><span class="sxs-lookup"><span data-stu-id="201cc-158">Create hello service as follows, adjusting hello path tooyour data and log directories accordingly:</span></span>
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log `
        --logappend  --install
    ```
   
    <span data-ttu-id="201cc-159">hello előző parancs létrehoz egy szolgáltatást MongoDB, nevű "Mongo DB" leírását.</span><span class="sxs-lookup"><span data-stu-id="201cc-159">hello preceding command creates a service named MongoDB, with a description of "Mongo DB".</span></span> <span data-ttu-id="201cc-160">a következő paraméterek hello is meg vannak adva:</span><span class="sxs-lookup"><span data-stu-id="201cc-160">hello following parameters are also specified:</span></span>
   
   * <span data-ttu-id="201cc-161">Hello `--dbpath` beállítás hello adatkönyvtára hello helyét adja meg.</span><span class="sxs-lookup"><span data-stu-id="201cc-161">hello `--dbpath` option specifies hello location of hello data directory.</span></span>
   * <span data-ttu-id="201cc-162">Hello `--logpath` beállítást meg kell használt toospecify egy naplófájl, mert hello futó szolgáltatás nem rendelkezik egy toodisplay parancs kimenetét.</span><span class="sxs-lookup"><span data-stu-id="201cc-162">hello `--logpath` option must be used toospecify a log file, because hello running service does not have a command window toodisplay output.</span></span>
   * <span data-ttu-id="201cc-163">Hello `--logappend` beállítás megadja, hogy hello szolgáltatás újraindítását okozza-e a kimeneti tooappend toohello naplófájlokat.</span><span class="sxs-lookup"><span data-stu-id="201cc-163">hello `--logappend` option specifies that a restart of hello service causes output tooappend toohello existing log file.</span></span>
   
   <span data-ttu-id="201cc-164">toostart hello MongoDB szolgáltatást, futtathatja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="201cc-164">toostart hello MongoDB service, run hello following command:</span></span>
   
    ```
    net start MongoDB
    ```
   
    <span data-ttu-id="201cc-165">Hello MongoDB szolgáltatás létrehozásával kapcsolatos további információkért lásd: [egy Windows-szolgáltatás konfigurálása a mongodb-protokolltámogatással](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).</span><span class="sxs-lookup"><span data-stu-id="201cc-165">For more information about creating hello MongoDB service, see [Configure a Windows Service for MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).</span></span>

## <a name="test-hello-mongodb-instance"></a><span data-ttu-id="201cc-166">Teszt hello MongoDB-példány</span><span class="sxs-lookup"><span data-stu-id="201cc-166">Test hello MongoDB instance</span></span>
<span data-ttu-id="201cc-167">Egy példányban fut, vagy a szolgáltatásként telepített mongodb programot, és most elindíthatja létrehozása és használata az adatbázisokat.</span><span class="sxs-lookup"><span data-stu-id="201cc-167">With MongoDB running as a single instance or installed as a service, you can now start creating and using your databases.</span></span> <span data-ttu-id="201cc-168">toostart hello MongoDB felügyeleti rendszerhéjat, nyisson meg egy másik parancssort a hello **Start** menü, és írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="201cc-168">toostart hello MongoDB administrative shell, open another command prompt window from hello **Start** menu, and enter hello following command:</span></span>

```
mongo  
```

<span data-ttu-id="201cc-169">Hello hello adatbázisokkal listázhatja `db` parancsot.</span><span class="sxs-lookup"><span data-stu-id="201cc-169">You can list hello databases with hello `db` command.</span></span> <span data-ttu-id="201cc-170">Adatok beszúrása az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="201cc-170">Insert some data as follows:</span></span>

```
db.foo.insert( { a : 1 } )
```

<span data-ttu-id="201cc-171">Adatok keresése az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="201cc-171">Search for data as follows:</span></span>

```
db.foo.find()
```

<span data-ttu-id="201cc-172">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="201cc-172">hello output is similar toohello following example:</span></span>

```
{ "_id" : "ObjectId("57f6a86cee873a6232d74842"), "a" : 1 }
```

<span data-ttu-id="201cc-173">Kilépés hello `mongo` konzolon a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="201cc-173">Exit hello `mongo` console as follows:</span></span>

```
exit
```

## <a name="configure-firewall-and-network-security-group-rules"></a><span data-ttu-id="201cc-174">Konfigurálja a tűzfal és a hálózati biztonsági csoportszabályok</span><span class="sxs-lookup"><span data-stu-id="201cc-174">Configure firewall and Network Security Group rules</span></span>
<span data-ttu-id="201cc-175">Most, hogy a MongoDB telepítve és fut, nyissa meg a portot a Windows tűzfal, távolról csatlakozhat a tooMongoDB.</span><span class="sxs-lookup"><span data-stu-id="201cc-175">Now that MongoDB is installed and running, open a port in Windows Firewall so you can remotely connect tooMongoDB.</span></span> <span data-ttu-id="201cc-176">toocreate új bejövő szabály tooallow TCP-port 27017, nyisson meg egy rendszergazda PowerShell-parancssort, és írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="201cc-176">toocreate a new inbound rule tooallow TCP port 27017, open an administrative PowerShell prompt and enter hello following command:</span></span>

```powerahell
New-NetFirewallRule `
    -DisplayName "Allow MongoDB" `
    -Direction Inbound `
    -Protocol TCP `
    -LocalPort 27017 `
    -Action Allow
```

<span data-ttu-id="201cc-177">Hello szabály hello segítségével is létrehozhat **fokozott biztonságú Windows tűzfal** grafikus kezelőeszköz.</span><span class="sxs-lookup"><span data-stu-id="201cc-177">You can also create hello rule by using hello **Windows Firewall with Advanced Security** graphical management tool.</span></span> <span data-ttu-id="201cc-178">Hozzon létre egy új bejövő szabály tooallow TCP-port 27017.</span><span class="sxs-lookup"><span data-stu-id="201cc-178">Create a new inbound rule tooallow TCP port 27017.</span></span>

<span data-ttu-id="201cc-179">Szükség esetén hozzon létre egy hálózati biztonsági csoport szabály tooallow hozzáférés tooMongoDB a hello meglévő Azure virtuális hálózati alhálózat kívül.</span><span class="sxs-lookup"><span data-stu-id="201cc-179">If needed, create a Network Security Group rule tooallow access tooMongoDB from outside of hello existing Azure virtual network subnet.</span></span> <span data-ttu-id="201cc-180">Létrehozhat hello hálózati biztonsági csoportszabályok hello segítségével [Azure-portálon](nsg-quickstart-portal.md) vagy [Azure PowerShell](nsg-quickstart-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="201cc-180">You can create hello Network Security Group rules by using hello [Azure portal](nsg-quickstart-portal.md) or [Azure PowerShell](nsg-quickstart-powershell.md).</span></span> <span data-ttu-id="201cc-181">Hello Windows tűzfal-szabályokat, az TCP port 27017 toohello virtuális hálózati adapter a MongoDB VM engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="201cc-181">As with hello Windows Firewall rules, allow TCP port 27017 toohello virtual network interface of your MongoDB VM.</span></span>

> [!NOTE]
> <span data-ttu-id="201cc-182">TCP-port 27017 hello alapértelmezett portot használják a MongoDB.</span><span class="sxs-lookup"><span data-stu-id="201cc-182">TCP port 27017 is hello default port used by MongoDB.</span></span> <span data-ttu-id="201cc-183">Hello segítségével módosíthatja ezt a portot `--port` paraméter indításakor `mongod.exe` manuálisan, vagy egy szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="201cc-183">You can change this port by using hello `--port` parameter when starting `mongod.exe` manually or from a service.</span></span> <span data-ttu-id="201cc-184">Ha megváltoztatja hello portot, győződjön meg arról, hogy tooupdate hello Windows tűzfal és a hálózati biztonsági csoport szabályok az előző lépésekben hello.</span><span class="sxs-lookup"><span data-stu-id="201cc-184">If you change hello port, make sure tooupdate hello Windows Firewall and Network Security Group rules in hello preceding steps.</span></span>


## <a name="next-steps"></a><span data-ttu-id="201cc-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="201cc-185">Next steps</span></span>
<span data-ttu-id="201cc-186">Ebben az oktatóanyagban, megtudta, hogyan tooinstall és MongoDB konfigurálása a Windows virtuális gépén.</span><span class="sxs-lookup"><span data-stu-id="201cc-186">In this tutorial, you learned how tooinstall and configure MongoDB on your Windows VM.</span></span> <span data-ttu-id="201cc-187">Most már elérheti az MongoDB a Windows virtuális gépre, a következő hello témakörei speciális hello [MongoDB dokumentációt](https://docs.mongodb.com/manual/).</span><span class="sxs-lookup"><span data-stu-id="201cc-187">You can now access MongoDB on your Windows VM, by following hello advanced topics in hello [MongoDB documentation](https://docs.mongodb.com/manual/).</span></span>

