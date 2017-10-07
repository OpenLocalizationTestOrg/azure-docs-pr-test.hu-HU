---
title: "az SQL Data Warehouse - aaaAzure első lépések útmutató |} Microsoft Docs"
description: "Ez az oktatóanyag útmutatást ad meg hogyan tooprovision és az adatok betöltése az Azure SQL Data Warehouse. Megismerheti a méretezés, szüneteltetése és hangolása hello alapjairól is."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: barbkess
ms.assetid: 52DFC191-E094-4B04-893F-B64D5828A900
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: quickstart
ms.date: 01/26/2017
ms.author: elbutter;barbkess
ms.openlocfilehash: edd2a21b0fe49ca8e9792c7c512310339a822c55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-sql-data-warehouse"></a><span data-ttu-id="a852c-104">Bevezetés az SQL Data Warehouse használatába</span><span class="sxs-lookup"><span data-stu-id="a852c-104">Get started with SQL Data Warehouse</span></span>

<span data-ttu-id="a852c-105">Ez az oktatóanyag bemutatja, hogyan tooprovision és az adatok betöltése az Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="a852c-105">This tutorial shows how tooprovision and load data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="a852c-106">Megismerheti a méretezés, szüneteltetése és hangolása hello alapjairól is.</span><span class="sxs-lookup"><span data-stu-id="a852c-106">You’ll also learn hello basics about scaling, pausing, and tuning.</span></span> <span data-ttu-id="a852c-107">Amikor végzett, lesz, készen áll a tooquery lenniük, és megismerkedhet az adatraktár.</span><span class="sxs-lookup"><span data-stu-id="a852c-107">When you’re finished, you’ll be ready tooquery and explore your data warehouse.</span></span>

<span data-ttu-id="a852c-108">**Becsült idő toocomplete:** egy végpont oktatóanyag, amely körülbelül 30 percet toocomplete után hello Előfeltételek megfelel példakód azt.</span><span class="sxs-lookup"><span data-stu-id="a852c-108">**Estimated time toocomplete:** This is an end-to-end tutorial with example code that takes about 30 minutes toocomplete once you have met hello prerequisites.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="a852c-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a852c-109">Prerequisites</span></span>

<span data-ttu-id="a852c-110">hello oktatóanyag feltételezi, hogy az SQL Data Warehouse alapvető fogalmak megismerése.</span><span class="sxs-lookup"><span data-stu-id="a852c-110">hello tutorial assumes you are familiar with SQL Data Warehouse basic concepts.</span></span> <span data-ttu-id="a852c-111">A bevezetésért lásd: [Mi az az SQL Data Warehouse?](sql-data-warehouse-overview-what-is.md)</span><span class="sxs-lookup"><span data-stu-id="a852c-111">If you need an introduction, see [What is SQL Data Warehouse?](sql-data-warehouse-overview-what-is.md)</span></span> 

### <a name="sign-up-for-microsoft-azure"></a><span data-ttu-id="a852c-112">Regisztráció a Microsoft Azure-ban</span><span class="sxs-lookup"><span data-stu-id="a852c-112">Sign up for Microsoft Azure</span></span>
<span data-ttu-id="a852c-113">Ha még nem rendelkezik egy Microsoft Azure-fiók, meg kell feliratkozott egy toouse toosign ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="a852c-113">If you don't already have a Microsoft Azure account, you need toosign up for one toouse this service.</span></span> <span data-ttu-id="a852c-114">Ha már rendelkezik fiókkal, ezt a lépést kihagyhatja.</span><span class="sxs-lookup"><span data-stu-id="a852c-114">If you already have an account, you may skip this step.</span></span> 

1. <span data-ttu-id="a852c-115">Keresse meg a fiók lapok toohello [https://azure.microsoft.com/account/](https://azure.microsoft.com/account/)</span><span class="sxs-lookup"><span data-stu-id="a852c-115">Navigate toohello account pages [https://azure.microsoft.com/account/](https://azure.microsoft.com/account/)</span></span>
2. <span data-ttu-id="a852c-116">Hozzon létre egy ingyenes Azure-fiókot, vagy vásároljon egy fiókot.</span><span class="sxs-lookup"><span data-stu-id="a852c-116">Create a free Azure account, or purchase an account.</span></span>
3. <span data-ttu-id="a852c-117">Hello utasítások</span><span class="sxs-lookup"><span data-stu-id="a852c-117">Follow hello instructions</span></span>

### <a name="install-appropriate-sql-client-drivers-and-tools"></a><span data-ttu-id="a852c-118">A megfelelő SQL-ügyfélillesztők és -ügyféleszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="a852c-118">Install appropriate SQL client drivers and tools</span></span>

<span data-ttu-id="a852c-119">A legtöbb SQL-ügyféleszközöket JDBC, ODBC vagy az ADO.NET használatával képes kapcsolódni tooSQL Data warehouse-bA.</span><span class="sxs-lookup"><span data-stu-id="a852c-119">Most SQL client tools can connect tooSQL Data Warehouse by using JDBC, ODBC, or ADO.NET.</span></span> <span data-ttu-id="a852c-120">Lejáró toohello nagy mennyiségű, amely támogatja az SQL Data Warehouse T-SQL funkciókat néhány ügyfélalkalmazások nincsenek teljesen kompatibilis, az SQL Data Warehouse szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="a852c-120">Due toohello large number of T-SQL features that SQL Data Warehouse supports, some client applications are not fully compatible with SQL Data Warehouse.</span></span>

<span data-ttu-id="a852c-121">Ha Windows operációs rendszert használ, javasoljuk a [Visual Studio] vagy az [SQL Server Management Studio] használatát.</span><span class="sxs-lookup"><span data-stu-id="a852c-121">If you are running a Windows operating system, we recommend using either [Visual Studio] or [SQL Server Management Studio].</span></span>

[!INCLUDE [Create a new logical server](../../includes/sql-data-warehouse-create-logical-server.md)] 

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="create-a-sql-data-warehouse"></a><span data-ttu-id="a852c-122">SQL Data Warehouse létrehozása</span><span class="sxs-lookup"><span data-stu-id="a852c-122">Create a SQL Data Warehouse</span></span>

<span data-ttu-id="a852c-123">Az SQL Data Warehouse egy nagymértékben párhuzamos feldolgozáshoz kialakított speciális típusú adatbázis.</span><span class="sxs-lookup"><span data-stu-id="a852c-123">A SQL Data Warehouse is a special type of database that is designed for massively parallel processing.</span></span> <span data-ttu-id="a852c-124">hello adatbázis több csomópont között van elosztva, és feldolgozza a párhuzamos lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="a852c-124">hello database is distributed across multiple nodes and processes queries in parallel.</span></span> <span data-ttu-id="a852c-125">Az SQL Data Warehouse van az összes hello csomópontjának hello tevékenységek vezénylő vezérlő csomópont.</span><span class="sxs-lookup"><span data-stu-id="a852c-125">SQL Data Warehouse has a control node that orchestrates hello activities of all hello nodes.</span></span> <span data-ttu-id="a852c-126">hello csomópontok magukat az adatokat SQL-adatbázis toomanage használni.</span><span class="sxs-lookup"><span data-stu-id="a852c-126">hello nodes themselves use SQL Database toomanage your data.</span></span>  

> [!NOTE]
> <span data-ttu-id="a852c-127">Egy SQL Data Warehouse létrehozása egy új számlázható szolgáltatás létrejöttét eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="a852c-127">Creating a SQL Data Warehouse might result in a new billable service.</span></span>  <span data-ttu-id="a852c-128">További információ: [SQL Data Warehouse díjszabása](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span><span class="sxs-lookup"><span data-stu-id="a852c-128">For more information, see [SQL Data Warehouse pricing](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span></span>
>

### <a name="create-a-data-warehouse"></a><span data-ttu-id="a852c-129">Adattárház létrehozása</span><span class="sxs-lookup"><span data-stu-id="a852c-129">Create a data warehouse</span></span>

1. <span data-ttu-id="a852c-130">Jelentkezzen be a hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a852c-130">Sign into hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="a852c-131">Kattintson a **New** > **Databases** > **SQL Data Warehouse** (Új > Adatbázisok > SQL Data Warehouse) elemre.</span><span class="sxs-lookup"><span data-stu-id="a852c-131">Click **New** > **Databases** > **SQL Data Warehouse**.</span></span>

    <span data-ttu-id="a852c-132">![NewBlade](../../includes/media/sql-data-warehouse-create-dw/blade-click-new.png)![SelectDW](../../includes/media/sql-data-warehouse-create-dw/blade-select-dw.png)</span><span class="sxs-lookup"><span data-stu-id="a852c-132">![NewBlade](../../includes/media/sql-data-warehouse-create-dw/blade-click-new.png) ![SelectDW](../../includes/media/sql-data-warehouse-create-dw/blade-select-dw.png)</span></span>

3. <span data-ttu-id="a852c-133">Adja meg az üzembe helyezés részleteit</span><span class="sxs-lookup"><span data-stu-id="a852c-133">Fill out deployment details</span></span>

    <span data-ttu-id="a852c-134">**Database name** (Adatbázis neve): Tetszés szerint bármit megadhat.</span><span class="sxs-lookup"><span data-stu-id="a852c-134">**Database Name**: Pick anything you'd like.</span></span> <span data-ttu-id="a852c-135">Ha több adatraktárak, célszerű a nevek a következők részletekről, mint a hello régió, például a környezet *mydw-westus-1-teszt*.</span><span class="sxs-lookup"><span data-stu-id="a852c-135">If you have multiple data warehouses, we recommend your names include details such as hello region, environment, for example *mydw-westus-1-test*.</span></span>

    <span data-ttu-id="a852c-136">**Subscription** (Előfizetés): Az Ön Azure-előfizetése</span><span class="sxs-lookup"><span data-stu-id="a852c-136">**Subscription**: Your Azure subscription</span></span>

    <span data-ttu-id="a852c-137">**Erőforráscsoport**: Hozzon létre egy erőforráscsoportot, vagy használjon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="a852c-137">**Resource Group**: Create a resource group or use an existing resource group.</span></span>
    > [!NOTE]
    > <span data-ttu-id="a852c-138">Az erőforráscsoportok hasznosak az erőforrások adminisztrációja, például a hozzáférés-vezérlés körének meghatározása és a sablonalapú üzembe helyezés során.</span><span class="sxs-lookup"><span data-stu-id="a852c-138">Resource groups are useful for resource administration such as scoping access control and templated deployment.</span></span> <span data-ttu-id="a852c-139">Tudjon meg többet az Azure erőforráscsoportjairól és az ajánlott eljárásokról [itt](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="a852c-139">Read more about Azure resource groups and best practices [here](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)</span></span>

    <span data-ttu-id="a852c-140">**Forrás**: Üres adatbázis</span><span class="sxs-lookup"><span data-stu-id="a852c-140">**Source**: Blank Database</span></span>

    <span data-ttu-id="a852c-141">**Kiszolgáló**: Select hello server létrehozott [Előfeltételek].</span><span class="sxs-lookup"><span data-stu-id="a852c-141">**Server**: Select hello server you created in [Prerequisites].</span></span>

    <span data-ttu-id="a852c-142">**Rendezés**: hello alapértelmezett rendezést SQL_Latin1_General_CP1_CI_AS hagyja.</span><span class="sxs-lookup"><span data-stu-id="a852c-142">**Collation**: Leave hello default collation SQL_Latin1_General_CP1_CI_AS.</span></span>

    <span data-ttu-id="a852c-143">**Válassza ki a teljesítmény**: hello szabványos 400DWU kezdődően ajánlott.</span><span class="sxs-lookup"><span data-stu-id="a852c-143">**Select performance**: We recommend starting with hello standard 400DWU.</span></span>

4. <span data-ttu-id="a852c-144">Válasszon **PIN-kód toodashboard** ![PIN-kód tooDashboard](./media/sql-data-warehouse-get-started-tutorial/pin-to-dashboard.png)</span><span class="sxs-lookup"><span data-stu-id="a852c-144">Choose **Pin toodashboard** ![Pin tooDashboard](./media/sql-data-warehouse-get-started-tutorial/pin-to-dashboard.png)</span></span>

5. <span data-ttu-id="a852c-145">Elhelyezkedik vissza, és várja meg a data warehouse toodeploy!</span><span class="sxs-lookup"><span data-stu-id="a852c-145">Sit back and wait for your data warehouse toodeploy!</span></span> <span data-ttu-id="a852c-146">A folyamat tootake normális néhány percig.</span><span class="sxs-lookup"><span data-stu-id="a852c-146">It's normal for this process tootake several minutes.</span></span> <span data-ttu-id="a852c-147">hello portál értesíti, ha az adatraktár készen toouse.</span><span class="sxs-lookup"><span data-stu-id="a852c-147">hello portal notifies you when your data warehouse is ready toouse.</span></span> 

## <a name="connect-toosql-data-warehouse"></a><span data-ttu-id="a852c-148">Csatlakozás az adatraktár tooSQL</span><span class="sxs-lookup"><span data-stu-id="a852c-148">Connect tooSQL Data Warehouse</span></span>

<span data-ttu-id="a852c-149">Ez az oktatóanyag az SQL Server Management Studio (SSMS) tooconnect toohello adatraktár használja.</span><span class="sxs-lookup"><span data-stu-id="a852c-149">This tutorial uses SQL Server Management Studio (SSMS) tooconnect toohello data warehouse.</span></span> <span data-ttu-id="a852c-150">A támogatott összekötők keresztül kapcsolódhatnak a Data Warehouse tooSQL: ADO.NET, JDBC, ODBC és a PHP.</span><span class="sxs-lookup"><span data-stu-id="a852c-150">You can connect tooSQL Data Warehouse through these supported connectors: ADO.NET, JDBC, ODBC, and PHP.</span></span> <span data-ttu-id="a852c-151">Ne feledje, hogy a Microsoft által nem támogatott eszközök esetében a funkcionalitás korlátozott lehet.</span><span class="sxs-lookup"><span data-stu-id="a852c-151">Remember, functionality might be limited for tools that are not supported by Microsoft.</span></span>


### <a name="get-connection-information"></a><span data-ttu-id="a852c-152">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="a852c-152">Get connection information</span></span>

<span data-ttu-id="a852c-153">tooconnect tooyour adatraktár kell hello logikai SQL-kiszolgálón keresztül létrehozott tooconnect [Előfeltételek].</span><span class="sxs-lookup"><span data-stu-id="a852c-153">tooconnect tooyour data warehouse, you need tooconnect through hello logical SQL server you created in [Prerequisites].</span></span>

1. <span data-ttu-id="a852c-154">Válassza ki az adatraktár hello irányítópult vagy keresse meg azt a erőforrásokban.</span><span class="sxs-lookup"><span data-stu-id="a852c-154">Select your data warehouse from hello dashboard or search for it in your resources.</span></span>

    ![SQL Data Warehouse irányítópult](./media/sql-data-warehouse-get-started-tutorial/sql-dw-dashboard.png)

2. <span data-ttu-id="a852c-156">Található hello hello logikai SQL server teljes nevét.</span><span class="sxs-lookup"><span data-stu-id="a852c-156">Find hello full name for hello logical SQL server.</span></span>

    ![Kiszolgálónév kiválasztása](./media/sql-data-warehouse-get-started-tutorial/select-server.png)

3. <span data-ttu-id="a852c-158">Nyissa meg a szolgáltatáshoz az SSMS, object explorer tooconnect toothis kiszolgáló létrehozott hello server rendszergazdai hitelesítő adataival [Előfeltételek]</span><span class="sxs-lookup"><span data-stu-id="a852c-158">Open SSMS and use object explorer tooconnect toothis server using hello server admin credentials you created in [Prerequisites]</span></span>

    ![Csatlakozás SSMS segítségével](./media/sql-data-warehouse-get-started-tutorial/ssms-connect.png)

<span data-ttu-id="a852c-160">Ha minden megfelelően megfelelően, kell csatlakoztatott tooyour logikai SQL-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="a852c-160">If all goes correctly, you should now be connected tooyour logical SQL server.</span></span> <span data-ttu-id="a852c-161">Mivel Ön bejelentkezett, a kiszolgáló rendszergazdája hello, hello kiszolgáló, többek között a master adatbázis hello által üzemeltetett tooany adatbázis is elérheti.</span><span class="sxs-lookup"><span data-stu-id="a852c-161">Since you logged in as hello server admin, you can connect tooany database hosted by hello server, including hello master database.</span></span> 

<span data-ttu-id="a852c-162">Csak egy kiszolgáló-rendszergazdai fiókot, és minden olyan felhasználó, a legtöbb jogosultságával rendelkezik hello.</span><span class="sxs-lookup"><span data-stu-id="a852c-162">There is only one server admin account and it has hello most privileges of any user.</span></span> <span data-ttu-id="a852c-163">Legyen óvatos nem tooallow a szervezet tooknow hello rendszergazdai jelszó túl sokan.</span><span class="sxs-lookup"><span data-stu-id="a852c-163">Be careful not tooallow too many people in your organization tooknow hello admin password.</span></span> 

<span data-ttu-id="a852c-164">Emellett rendelkezhet egy Azure Active Directory-rendszergazdai fiókkal is.</span><span class="sxs-lookup"><span data-stu-id="a852c-164">You can also have an Azure active directory admin account.</span></span> <span data-ttu-id="a852c-165">A Microsoft hello részleteit itt nem ad meg.</span><span class="sxs-lookup"><span data-stu-id="a852c-165">We don't provide hello details here.</span></span> <span data-ttu-id="a852c-166">Ha azt szeretné, hogy Azure Active Directory-hitelesítés használatával kapcsolatos további toolearn, [az Azure AD-alapú hitelesítés](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication).</span><span class="sxs-lookup"><span data-stu-id="a852c-166">If you want toolearn more about using Azure Active Directory authentication, see [Azure AD authentication](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication).</span></span>

<span data-ttu-id="a852c-167">Ezután a további bejelentkezések és felhasználók létrehozásával ismerkedünk meg.</span><span class="sxs-lookup"><span data-stu-id="a852c-167">Next, we explore creating additional logins and users.</span></span>


## <a name="create-a-database-user"></a><span data-ttu-id="a852c-168">Adatbázis-felhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a852c-168">Create a database user</span></span>

<span data-ttu-id="a852c-169">Ebben a lépésben létrehoz egy felhasználói fiók tooaccess az adatraktár.</span><span class="sxs-lookup"><span data-stu-id="a852c-169">In this step, you create a user account tooaccess your data warehouse.</span></span> <span data-ttu-id="a852c-170">Azt is bemutatja, hogyan toogive adott felhasználó hello képességét toorun lekérdezi a nagy mennyiségű memória és CPU-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="a852c-170">We also show you how toogive that user hello ability toorun queries with a large amount of memory and CPU resources.</span></span>

### <a name="notes-about-resource-classes-for-allocating-resources-tooqueries"></a><span data-ttu-id="a852c-171">Erőforrás-osztályok a erőforrások tooqueries lefoglalásával kapcsolatos megjegyzések</span><span class="sxs-lookup"><span data-stu-id="a852c-171">Notes about resource classes for allocating resources tooqueries</span></span>

- <span data-ttu-id="a852c-172">tookeep biztonságos, az adatok az éles adatbázist ne használjon hello server admin toorun lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="a852c-172">tookeep your data safe, don't use hello server admin toorun queries on your production databases.</span></span> <span data-ttu-id="a852c-173">Hello minden olyan felhasználó, a legtöbb jogosultságával rendelkezik, és használja azt tooperform műveleteket a felhasználói adatok helyezi az adatok veszélyben.</span><span class="sxs-lookup"><span data-stu-id="a852c-173">It has hello most privileges of any user and using it tooperform operations on user data puts your data at risk.</span></span> <span data-ttu-id="a852c-174">Is célja, hogy hello kiszolgálói rendszergazda tooperform kezelési műveletek, mert azt fut műveletek csak kis lefoglalása a memória és CPU-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="a852c-174">Also, since hello server admin is meant tooperform management operations, it runs operations with only a small allocation of memory and CPU resources.</span></span> 

- <span data-ttu-id="a852c-175">Az SQL Data Warehouse előre meghatározott adatbázis-szerepkörök, erőforrás-osztályok, a memória, Processzor-erőforrások és feldolgozási üzembe helyezési ponti toousers különböző mennyiségű tooallocate nevű használja.</span><span class="sxs-lookup"><span data-stu-id="a852c-175">SQL Data Warehouse uses pre-defined database roles, called resource classes, tooallocate different amounts of memory, CPU resources, and concurrency slots toousers.</span></span> <span data-ttu-id="a852c-176">Minden felhasználó tooa kis, közepes, nagy méretű vagy extra nagy erőforrásosztály is tartozhatnak.</span><span class="sxs-lookup"><span data-stu-id="a852c-176">Each user can belong tooa small, medium, large, or extra-large resource class.</span></span> <span data-ttu-id="a852c-177">hello felhasználó erőforrásosztály meghatározza, hogy hello erőforrások hello felhasználó rendelkezik-e toorun lekérdezések és műveletek betölteni.</span><span class="sxs-lookup"><span data-stu-id="a852c-177">hello user's resource class determines hello resources hello user has toorun queries and load operations.</span></span>

- <span data-ttu-id="a852c-178">Optimális adattömörítést hello felhasználói kell nagy tooload vagy extra nagy erőforrás-hozzárendelések.</span><span class="sxs-lookup"><span data-stu-id="a852c-178">For optimal data compression, hello user may need tooload with large or extra large resource allocations.</span></span> <span data-ttu-id="a852c-179">További információkat az erőforrásosztályokról [itt](./sql-data-warehouse-develop-concurrency.md#resource-classes) talál.</span><span class="sxs-lookup"><span data-stu-id="a852c-179">Read more about resource classes [here](./sql-data-warehouse-develop-concurrency.md#resource-classes):</span></span>

### <a name="create-an-account-that-can-control-a-database"></a><span data-ttu-id="a852c-180">Az adatbázisok vezérlésére alkalmas fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="a852c-180">Create an account that can control a database</span></span>

<span data-ttu-id="a852c-181">Mivel a kiszolgáló rendszergazdája hello naplózza, hogy engedélyeket toocreate bejelentkezéseket és a felhasználók.</span><span class="sxs-lookup"><span data-stu-id="a852c-181">Since you are currently logged in as hello server admin you have permissions toocreate logins and users.</span></span>

1. <span data-ttu-id="a852c-182">Az SSMS vagy más lekérdezésügyfél használatával nyisson egy új lekérdezést a **masteren**.</span><span class="sxs-lookup"><span data-stu-id="a852c-182">Using SSMS or another query client, open a new query for **master**.</span></span>

    ![Új lekérdezés a Master adatbázison](./media/sql-data-warehouse-get-started-tutorial/query-on-server.png)

    ![Új lekérdezés a Master1 adatbázison](./media/sql-data-warehouse-get-started-tutorial/query-on-master.png)

2. <span data-ttu-id="a852c-185">Hello lekérdezési ablakban futtassa a T-SQL-parancs toocreate MedRCLogin nevű bejelentkezési azonosítót és LoadingUser nevű felhasználót.</span><span class="sxs-lookup"><span data-stu-id="a852c-185">In hello query window, run this T-SQL command toocreate a login named MedRCLogin and user named LoadingUser.</span></span> <span data-ttu-id="a852c-186">Ehhez a bejelentkezéshez toohello logikai SQL server is elérheti.</span><span class="sxs-lookup"><span data-stu-id="a852c-186">This login can connect toohello logical SQL server.</span></span>

    ```sql
    CREATE LOGIN MedRCLogin WITH PASSWORD = 'a123reallySTRONGpassword!';
    CREATE USER LoadingUser FOR LOGIN MedRCLogin;
    ```

3. <span data-ttu-id="a852c-187">Most lekérdezése hello *SQL Data Warehouse-adatbázis*, hozzon létre egy adatbázis-felhasználó alapú tooaccess létrehozott bejelentkezési hello és műveleteket hajtson végre a hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="a852c-187">Now querying hello *SQL Data Warehouse database*, create a database user based on hello login you created tooaccess and perform operations on hello database.</span></span>

    ```sql
    CREATE USER LoadingUser FOR LOGIN MedRCLogin;
    ```

4. <span data-ttu-id="a852c-188">Adjon hello adatbázis felhasználói vezérlő engedélyek toohello adatbázisnak NYT nevezik.</span><span class="sxs-lookup"><span data-stu-id="a852c-188">Give hello database user control permissions toohello database called NYT.</span></span> 

    ```sql
    GRANT CONTROL ON DATABASE::[NYT] tooLoadingUser;
    ```
    > [!NOTE]
    > <span data-ttu-id="a852c-189">Az adatbázisnév kötőjeleket rendelkezik, ha kell, hogy toowrap azt szögletes zárójelbe!</span><span class="sxs-lookup"><span data-stu-id="a852c-189">If your database name has hyphens in it, be sure toowrap it in brackets!</span></span> 
    >

### <a name="give-hello-user-medium-resource-allocations"></a><span data-ttu-id="a852c-190">Adjon hello felhasználói közepes erőforrás-hozzárendelések</span><span class="sxs-lookup"><span data-stu-id="a852c-190">Give hello user medium resource allocations</span></span>

1. <span data-ttu-id="a852c-191">Futtassa a T-SQL-parancs toomake nevezik mediumrc hello közepes erőforrás osztály tagja egy informatikai.</span><span class="sxs-lookup"><span data-stu-id="a852c-191">Run this T-SQL command toomake it a member of hello medium resource class, which is called mediumrc.</span></span> 

    ```sql
    EXEC sp_addrolemember 'mediumrc', 'LoadingUser';
    ```
    > [!NOTE]
    > <span data-ttu-id="a852c-192">Kattintson a [Itt](sql-data-warehouse-develop-concurrency.md#resource-classes) feldolgozási és erőforrás-osztályok kapcsolatos további toolearn!</span><span class="sxs-lookup"><span data-stu-id="a852c-192">Click [here](sql-data-warehouse-develop-concurrency.md#resource-classes) toolearn more about concurrency and resource classes!</span></span> 
    >

2. <span data-ttu-id="a852c-193">Csatlakoztassa a toohello logikai kiszolgálót hello új hitelesítő adatokkal</span><span class="sxs-lookup"><span data-stu-id="a852c-193">Connect toohello logical server with hello new credentials</span></span>

    ![Bejelentkezés az új bejelentkezési adatokkal](./media/sql-data-warehouse-get-started-tutorial/new-login.png)


## <a name="load-data-from-azure-blob-storage"></a><span data-ttu-id="a852c-195">Adatok betöltése az Azure Blob Storage-ből</span><span class="sxs-lookup"><span data-stu-id="a852c-195">Load data from Azure blob storage</span></span>

<span data-ttu-id="a852c-196">Most már áll készen tooload adatok az a data warehouse-bA.</span><span class="sxs-lookup"><span data-stu-id="a852c-196">You are now ready tooload data into your data warehouse.</span></span> <span data-ttu-id="a852c-197">Ez a lépés bemutatja, hogyan tooload New York Város taxi cab adatait egy nyilvános Azure storage blob-e.</span><span class="sxs-lookup"><span data-stu-id="a852c-197">This step shows you how tooload New York City taxi cab data from a public Azure storage blob.</span></span> 

- <span data-ttu-id="a852c-198">Közös úgy tooload adatokat az SQL Data Warehouse toofirst hello adatok tooAzure blob-tároló áthelyezéséhez, és ezután töltse be az adatraktár.</span><span class="sxs-lookup"><span data-stu-id="a852c-198">A common way tooload data into SQL Data Warehouse is toofirst move hello data tooAzure blob storage, and then load it into your data warehouse.</span></span> <span data-ttu-id="a852c-199">toomake azt könnyebb toounderstand hogyan tooload, tudunk Győr taxi cab adatok már található egy nyilvános Azure storage-blobba.</span><span class="sxs-lookup"><span data-stu-id="a852c-199">toomake it easier toounderstand how tooload, we have New York taxi cab data already hosted in a public Azure storage blob.</span></span> 

- <span data-ttu-id="a852c-200">Későbbi használatra toolearn hogyan tooget az adatok tooAzure blob-tároló vagy tooload azt közvetlenül a forráskiszolgálón az SQL Data Warehouse, lásd: hello [betöltést áttekintő](sql-data-warehouse-overview-load.md).</span><span class="sxs-lookup"><span data-stu-id="a852c-200">For future reference, toolearn how tooget your data tooAzure blob storage or tooload it directly from your source into SQL Data Warehouse, see hello [loading overview](sql-data-warehouse-overview-load.md).</span></span>


### <a name="define-external-data"></a><span data-ttu-id="a852c-201">Külső adatok meghatározása</span><span class="sxs-lookup"><span data-stu-id="a852c-201">Define external data</span></span>

1. <span data-ttu-id="a852c-202">Hozzon létre egy főkulcsot.</span><span class="sxs-lookup"><span data-stu-id="a852c-202">Create a master key.</span></span> <span data-ttu-id="a852c-203">Csak egyszer adatbázisonként főkulcs toocreate kell.</span><span class="sxs-lookup"><span data-stu-id="a852c-203">You only need toocreate a master key once per database.</span></span> 

    ```sql
    CREATE MASTER KEY;
    ```

2. <span data-ttu-id="a852c-204">Adja meg a hello Azure blob hello taxi cab-adatokat tartalmazó hello helyét.</span><span class="sxs-lookup"><span data-stu-id="a852c-204">Define hello location of hello Azure blob that contains hello taxi cab data.</span></span>  

    ```sql
    CREATE EXTERNAL DATA SOURCE NYTPublic
    WITH
    (
        TYPE = Hadoop,
        LOCATION = 'wasbs://2013@nytpublic.blob.core.windows.net/'
    );
    ```

3. <span data-ttu-id="a852c-205">Adja meg a hello külső fájlformátum</span><span class="sxs-lookup"><span data-stu-id="a852c-205">Define hello external file formats</span></span>

    <span data-ttu-id="a852c-206">Hello ```CREATE EXTERNAL FILE FORMAT``` parancs használt toospecify hello külső adatokat tartalmazó fájlok formátumban.</span><span class="sxs-lookup"><span data-stu-id="a852c-206">hello ```CREATE EXTERNAL FILE FORMAT``` command is used toospecify the format of files that contain hello external data.</span></span> <span data-ttu-id="a852c-207">Ezek egy vagy több karakter, az úgynevezett elválasztó karakterek használatával vannak elválasztva.</span><span class="sxs-lookup"><span data-stu-id="a852c-207">They contain text separated by one or more characters called delimiters.</span></span> <span data-ttu-id="a852c-208">Bemutatási célokra hello taxi cab-fájl tárolja tömörítetlen adatokhoz és gzip tömörített adatként történjen.</span><span class="sxs-lookup"><span data-stu-id="a852c-208">For demonstration purposes, hello taxi cab data is stored both as uncompressed data and as gzip compressed data.</span></span>

    <span data-ttu-id="a852c-209">T-SQL parancsot futtatva toodefine két különböző formátumokban: tömörítetlenül és tömörített.</span><span class="sxs-lookup"><span data-stu-id="a852c-209">Run these T-SQL commands toodefine two different formats: uncompressed and compressed.</span></span>

    ```sql
    CREATE EXTERNAL FILE FORMAT uncompressedcsv
    WITH (
        FORMAT_TYPE = DELIMITEDTEXT,
        FORMAT_OPTIONS ( 
            FIELD_TERMINATOR = ',',
            STRING_DELIMITER = '',
            DATE_FORMAT = '',
            USE_TYPE_DEFAULT = False
        )
    );

    CREATE EXTERNAL FILE FORMAT compressedcsv
    WITH ( 
        FORMAT_TYPE = DELIMITEDTEXT,
        FORMAT_OPTIONS ( FIELD_TERMINATOR = '|',
            STRING_DELIMITER = '',
        DATE_FORMAT = '',
            USE_TYPE_DEFAULT = False
        ),
        DATA_COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
    );
    ```

4.  <span data-ttu-id="a852c-210">Hozzon létre egy sémát a külső fájlformátum számára.</span><span class="sxs-lookup"><span data-stu-id="a852c-210">Create a schema for your external file format.</span></span> 

    ```sql
    CREATE SCHEMA ext;
    ```
5. <span data-ttu-id="a852c-211">Hello külső táblák létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a852c-211">Create hello external tables.</span></span> <span data-ttu-id="a852c-212">Ezek a táblák az Azure Blob Storage-ben tárolt adatokra hivatkoznak.</span><span class="sxs-lookup"><span data-stu-id="a852c-212">These tables reference data stored in Azure blob storage.</span></span> <span data-ttu-id="a852c-213">Futtassa a következő T-SQL-parancsok toocreate hello több külső táblák, hogy minden pont toohello Azure blob meghatározott korábban a külső adatforrás.</span><span class="sxs-lookup"><span data-stu-id="a852c-213">Run hello following T-SQL commands toocreate several external tables that all point toohello Azure blob we defined previously in our external data source.</span></span>

```sql
    CREATE EXTERNAL TABLE [ext].[Date] 
    (
        [DateID] int NOT NULL,
        [Date] datetime NULL,
        [DateBKey] char(10) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfMonth] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DaySuffix] varchar(4) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeek] char(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeekInMonth] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeekInYear] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfQuarter] varchar(3) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfYear] varchar(3) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfMonth] varchar(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfQuarter] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfYear] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Month] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthOfQuarter] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Quarter] char(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [QuarterName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Year] char(4) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [YearName] char(7) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthYear] char(10) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MMYYYY] char(6) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FirstDayOfMonth] date NULL,
        [LastDayOfMonth] date NULL,
        [FirstDayOfQuarter] date NULL,
        [LastDayOfQuarter] date NULL,
        [FirstDayOfYear] date NULL,
        [LastDayOfYear] date NULL,
        [IsHolidayUSA] bit NULL,
        [IsWeekday] bit NULL,
        [HolidayUSA] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Date',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    );
    
    CREATE EXTERNAL TABLE [ext].[Geography]
    (
        [GeographyID] int NOT NULL,
        [ZipCodeBKey] varchar(10) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [County] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [City] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [State] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Country] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [ZipCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Geography',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0 
    );
        
    
    CREATE EXTERNAL TABLE [ext].[HackneyLicense]
    (
        [HackneyLicenseID] int NOT NULL,
        [HackneyLicenseBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [HackneyLicenseCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'HackneyLicense',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
        
    
    CREATE EXTERNAL TABLE [ext].[Medallion]
    (
        [MedallionID] int NOT NULL,
        [MedallionBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [MedallionCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Medallion',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
        
    CREATE EXTERNAL TABLE [ext].[Time]
    (
        [TimeID] int NOT NULL,
        [TimeBKey] varchar(8) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [HourNumber] tinyint NOT NULL,
        [MinuteNumber] tinyint NOT NULL,
        [SecondNumber] tinyint NOT NULL,
        [TimeInSecond] int NOT NULL,
        [HourlyBucket] varchar(15) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [DayTimeBucketGroupKey] int NOT NULL,
        [DayTimeBucket] varchar(100) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL
    )
    WITH
    (
        LOCATION = 'Time',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
    
    
    CREATE EXTERNAL TABLE [ext].[Trip]
    (
        [DateID] int NOT NULL,
        [MedallionID] int NOT NULL,
        [HackneyLicenseID] int NOT NULL,
        [PickupTimeID] int NOT NULL,
        [DropoffTimeID] int NOT NULL,
        [PickupGeographyID] int NULL,
        [DropoffGeographyID] int NULL,
        [PickupLatitude] float NULL,
        [PickupLongitude] float NULL,
        [PickupLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DropoffLatitude] float NULL,
        [DropoffLongitude] float NULL,
        [DropoffLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [PassengerCount] int NULL,
        [TripDurationSeconds] int NULL,
        [TripDistanceMiles] float NULL,
        [PaymentType] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FareAmount] money NULL,
        [SurchargeAmount] money NULL,
        [TaxAmount] money NULL,
        [TipAmount] money NULL,
        [TollsAmount] money NULL,
        [TotalAmount] money NULL
    )
    WITH
    (
        LOCATION = 'Trip2013',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = compressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
    
    CREATE EXTERNAL TABLE [ext].[Weather]
    (
        [DateID] int NOT NULL,
        [GeographyID] int NOT NULL,
        [PrecipitationInches] float NOT NULL,
        [AvgTemperatureFahrenheit] float NOT NULL
    )
    WITH
    (
        LOCATION = 'Weather2013',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
```

### <a name="import-hello-data-from-azure-blob-storage"></a><span data-ttu-id="a852c-214">Hello adatok importálása az Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="a852c-214">Import hello data from Azure blob storage.</span></span>

<span data-ttu-id="a852c-215">Az SQL Data Warehouse támogat egy CREATE TABLE AS SELECT (CTAS) nevű kulcsutasítást.</span><span class="sxs-lookup"><span data-stu-id="a852c-215">SQL Data Warehouse supports a key statement called CREATE TABLE AS SELECT (CTAS).</span></span> <span data-ttu-id="a852c-216">A jelen nyilatkozat táblázatot hoz létre új hello eredmények select utasítás alapján.</span><span class="sxs-lookup"><span data-stu-id="a852c-216">This statement creates a new table based on hello results of a select statement.</span></span> <span data-ttu-id="a852c-217">hello új táblának azonos oszlopok és adattípusok hello hello hello eredményeit válasszon ki az utasítást.</span><span class="sxs-lookup"><span data-stu-id="a852c-217">hello new table has hello same columns and data types as hello results of hello select statement.</span></span>  <span data-ttu-id="a852c-218">Ez az az Azure blob storage az SQL Data Warehouse egy elegáns módon tooimport adatokat.</span><span class="sxs-lookup"><span data-stu-id="a852c-218">This is an elegant way tooimport data from Azure blob storage into SQL Data Warehouse.</span></span>

1. <span data-ttu-id="a852c-219">Futtassa a parancsfájlt tooimport az adatok.</span><span class="sxs-lookup"><span data-stu-id="a852c-219">Run this script tooimport your data.</span></span>

    ```sql
    CREATE TABLE [dbo].[Date]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Date]
    OPTION (LABEL = 'CTAS : Load [dbo].[Date]')
    ;
    
    CREATE TABLE [dbo].[Geography]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT * FROM [ext].[Geography]
    OPTION (LABEL = 'CTAS : Load [dbo].[Geography]')
    ;
    
    CREATE TABLE [dbo].[HackneyLicense]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[HackneyLicense]
    OPTION (LABEL = 'CTAS : Load [dbo].[HackneyLicense]')
    ;
    
    CREATE TABLE [dbo].[Medallion]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Medallion]
    OPTION (LABEL = 'CTAS : Load [dbo].[Medallion]')
    ;
    
    CREATE TABLE [dbo].[Time]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Time]
    OPTION (LABEL = 'CTAS : Load [dbo].[Time]')
    ;
    
    CREATE TABLE [dbo].[Weather]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Weather]
    OPTION (LABEL = 'CTAS : Load [dbo].[Weather]')
    ;
    
    CREATE TABLE [dbo].[Trip]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Trip]
    OPTION (LABEL = 'CTAS : Load [dbo].[Trip]')
    ;
    ```

2. <span data-ttu-id="a852c-220">A betöltés közben megtekintheti az adatokat.</span><span class="sxs-lookup"><span data-stu-id="a852c-220">View your data as it loads.</span></span>

   <span data-ttu-id="a852c-221">Több GB-nyi adatot tölt be és tömörít nagy teljesítményű fürtözött oszlopcentrikus indexekbe.</span><span class="sxs-lookup"><span data-stu-id="a852c-221">You’re loading several GBs of data and compressing it into highly performant clustered columnstore indexes.</span></span> <span data-ttu-id="a852c-222">Futtassa a következő lekérdezés hello használó hello terhelés a dinamikus felügyeleti nézetek (dinamikus felügyeleti nézetek) tooshow hello állapota.</span><span class="sxs-lookup"><span data-stu-id="a852c-222">Run hello following query that uses a dynamic management views (DMVs) tooshow hello status of hello load.</span></span> <span data-ttu-id="a852c-223">Hello lekérdezés indítás után adása a kávé és egy Rögbi SQL Data Warehouse azonban néhány gyakori emelő.</span><span class="sxs-lookup"><span data-stu-id="a852c-223">After starting hello query, grab a coffee and a snack while SQL Data Warehouse does some heavy lifting.</span></span>
    
    ```sql
    SELECT
        r.command,
        s.request_id,
        r.status,
        count(distinct input_name) as nbr_files,
        sum(s.bytes_processed)/1024/1024/1024 as gb_processed
    FROM 
        sys.dm_pdw_exec_requests r
        INNER JOIN sys.dm_pdw_dms_external_work s
        ON r.request_id = s.request_id
    WHERE
        r.[label] = 'CTAS : Load [dbo].[Date]' OR
        r.[label] = 'CTAS : Load [dbo].[Geography]' OR
        r.[label] = 'CTAS : Load [dbo].[HackneyLicense]' OR
        r.[label] = 'CTAS : Load [dbo].[Medallion]' OR
        r.[label] = 'CTAS : Load [dbo].[Time]' OR
        r.[label] = 'CTAS : Load [dbo].[Weather]' OR
        r.[label] = 'CTAS : Load [dbo].[Trip]'
    GROUP BY
        r.command,
        s.request_id,
        r.status
    ORDER BY
        nbr_files desc, 
        gb_processed desc;
    ```

3. <span data-ttu-id="a852c-224">Tekintse meg az összes rendszerlekérdezést.</span><span class="sxs-lookup"><span data-stu-id="a852c-224">View all system queries.</span></span>

    ```sql
    SELECT * FROM sys.dm_pdw_exec_requests;
    ```

4. <span data-ttu-id="a852c-225">Láthatja, ahogy adatai szépen betöltődnek az Azure SQL Data Warehouse-ba.</span><span class="sxs-lookup"><span data-stu-id="a852c-225">Enjoy seeing your data nicely loaded into your Azure SQL Data Warehouse.</span></span>

    ![Betöltött adatok megjelenítése](./media/sql-data-warehouse-get-started-tutorial/see-data-loaded.png)


## <a name="improve-query-performance"></a><span data-ttu-id="a852c-227">Jobb lekérdezési teljesítmény</span><span class="sxs-lookup"><span data-stu-id="a852c-227">Improve query performance</span></span>

<span data-ttu-id="a852c-228">Számos módon tooimprove lekérdezési teljesítményt, és tooachieve hello nagy sebességű teljesítményéről, amely az SQL Data warehouse tooprovide tervezték.</span><span class="sxs-lookup"><span data-stu-id="a852c-228">There are several ways tooimprove query performance and tooachieve hello high-speed performance that SQL Data Warehouse is designed tooprovide.</span></span>  

### <a name="see-hello-effect-of-scaling-on-query-performance"></a><span data-ttu-id="a852c-229">Tekintse meg a lekérdezési teljesítmény méretezésének hello hatása</span><span class="sxs-lookup"><span data-stu-id="a852c-229">See hello effect of scaling on query performance</span></span> 

<span data-ttu-id="a852c-230">Egyirányú tooimprove lekérdezési teljesítmény tooscale erőforrások hello DWU szolgáltatási szint az adatraktár módosításával.</span><span class="sxs-lookup"><span data-stu-id="a852c-230">One way tooimprove query performance is tooscale resources by changing hello DWU service level for your data warehouse.</span></span> <span data-ttu-id="a852c-231">Minden szolgáltatási szintnek nagyobb a költsége, de bármikor visszaválthat vagy szüneteltetheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="a852c-231">Each service level costs more, but you can scale back or pause resources at any time.</span></span> 

<span data-ttu-id="a852c-232">Ebben a lépésben összehasonlítja a teljesítményt két különböző DWU-beállításnál.</span><span class="sxs-lookup"><span data-stu-id="a852c-232">In this step, you compare performance at two different DWU settings.</span></span>

<span data-ttu-id="a852c-233">Első, vertikálisan skálázzunk hello méretezési DWU, azt is képet kapjon a egy számítási csomópont lehet végre önállóan too100 le.</span><span class="sxs-lookup"><span data-stu-id="a852c-233">First, let's scale hello sizing down too100 DWU so we can get an idea of how one compute node might perform on its own.</span></span>

1. <span data-ttu-id="a852c-234">Nyissa meg toohello portálon, és válassza az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="a852c-234">Go toohello portal and select your SQL Data Warehouse.</span></span>

2. <span data-ttu-id="a852c-235">Válassza ki a skála hello SQL Data Warehouse panelre.</span><span class="sxs-lookup"><span data-stu-id="a852c-235">Select scale in hello SQL Data Warehouse blade.</span></span> 

    ![DW méretezése a portálról](./media/sql-data-warehouse-get-started-tutorial/scale-dw.png)

3. <span data-ttu-id="a852c-237">Eszközterület-too100 DWU hello teljesítmény csökkentheti, és kattintson a mentés.</span><span class="sxs-lookup"><span data-stu-id="a852c-237">Scale down hello performance bar too100 DWU and hit save.</span></span>

    ![Méretezés és mentés](./media/sql-data-warehouse-get-started-tutorial/scale-and-save.png)

4. <span data-ttu-id="a852c-239">Várjon, amíg a méretezési művelet toofinish.</span><span class="sxs-lookup"><span data-stu-id="a852c-239">Wait for your scale operation toofinish.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a852c-240">Lekérdezések hello méretezési módosítása közben nem futtatható.</span><span class="sxs-lookup"><span data-stu-id="a852c-240">Queries cannot run while changing hello scale.</span></span> <span data-ttu-id="a852c-241">A méretezés az épp futó lekérdezéseket **megszakítja**.</span><span class="sxs-lookup"><span data-stu-id="a852c-241">Scaling **kills** your currently running queries.</span></span> <span data-ttu-id="a852c-242">Újraindításukra hello művelet befejezésekor.</span><span class="sxs-lookup"><span data-stu-id="a852c-242">You can restart them when hello operation is finished.</span></span>
    >
    
5. <span data-ttu-id="a852c-243">Hajtsa végre a vizsgálati művelet hello út adatokon, felső millió bejegyzések hello összes hello oszlop kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="a852c-243">Do a scan operation on hello trip data, selecting hello top million entries for all hello columns.</span></span> <span data-ttu-id="a852c-244">Ha a számítógép különösen toomove gyorsan, érzi, hogy szabad tooselect kevesebb sort.</span><span class="sxs-lookup"><span data-stu-id="a852c-244">If you're eager toomove on quickly, feel free tooselect fewer rows.</span></span> <span data-ttu-id="a852c-245">Jegyezze fel a hello időt toorun ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="a852c-245">Take note of hello time it takes toorun this operation.</span></span>

    ```sql
    SELECT TOP(1000000) * FROM dbo.[Trip]
    ```
6. <span data-ttu-id="a852c-246">Az adatraktár méretezhető biztonsági too400 DWU.</span><span class="sxs-lookup"><span data-stu-id="a852c-246">Scale your data warehouse back too400 DWU.</span></span> <span data-ttu-id="a852c-247">Ne feledje, hogy minden 100 DWU egy másik számítási csomópont tooyour Azure SQL Data Warehouse hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="a852c-247">Remember, each 100 DWU is adding another compute node tooyour Azure SQL Data Warehouse.</span></span>

7. <span data-ttu-id="a852c-248">Futtassa újra a hello lekérdezés!</span><span class="sxs-lookup"><span data-stu-id="a852c-248">Run hello query again!</span></span> <span data-ttu-id="a852c-249">Jelentős eltérést kell tapasztalnia.</span><span class="sxs-lookup"><span data-stu-id="a852c-249">You should notice a significant difference.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="a852c-250">Hello lekérdezés nagy mennyiségű adatot ad vissza, mert a hello számítógépen, amelyen SSMS hello sávszélesség rendelkezésre állását a teljesítménybeli szűk keresztmetszetek lehet.</span><span class="sxs-lookup"><span data-stu-id="a852c-250">Because hello query returns a lot of data, hello bandwidth availability of hello machine running SSMS may be a performance bottleneck.</span></span> <span data-ttu-id="a852c-251">Emiatt lehetséges, hogy semmilyen teljesítménybeli javulást nem fog tapasztalni.</span><span class="sxs-lookup"><span data-stu-id="a852c-251">This can result in you not seeing any performance improvements!</span></span>

> [!NOTE]
> <span data-ttu-id="a852c-252">Ennek az az oka, hogy az SQL Data Warehouse nagymértékben párhuzamos feldolgozást használ.</span><span class="sxs-lookup"><span data-stu-id="a852c-252">Since SQL Data Warehouse uses massively parallel processing.</span></span> <span data-ttu-id="a852c-253">Ellenőrzési és analitikai funkciók végrehajtása több millió sort lekérdezések hello igaz hatványra emelésének Azure SQL Data Warehouse tapasztalhat.</span><span class="sxs-lookup"><span data-stu-id="a852c-253">Queries that scan or perform analytic functions on millions of rows experience hello true power of Azure SQL Data Warehouse.</span></span>
>

### <a name="see-hello-effect-of-statistics-on-query-performance"></a><span data-ttu-id="a852c-254">Statisztika hello hatásának tekintse meg a lekérdezési teljesítmény</span><span class="sxs-lookup"><span data-stu-id="a852c-254">See hello effect of statistics on query performance</span></span>

1. <span data-ttu-id="a852c-255">Hogy illesztések hello hello út táblával dátumtáblázat-lekérdezés futtatható</span><span class="sxs-lookup"><span data-stu-id="a852c-255">Run a query that joins hello Date table with hello Trip table</span></span>

    ```sql
    SELECT TOP (1000000) 
        dt.[DayOfWeek],
        tr.[MedallionID],
        tr.[HackneyLicenseID],
        tr.[PickupTimeID],
        tr.[DropoffTimeID],
        tr.[PickupGeographyID],
        tr.[DropoffGeographyID],
        tr.[PickupLatitude],
        tr.[PickupLongitude],
        tr.[PickupLatLong],
        tr.[DropoffLatitude],
        tr.[DropoffLongitude],
        tr.[DropoffLatLong],
        tr.[PassengerCount],
        tr.[TripDurationSeconds],
        tr.[TripDistanceMiles],
        tr.[PaymentType],
        tr.[FareAmount],
        tr.[SurchargeAmount],
        tr.[TaxAmount],
        tr.[TipAmount],
        tr.[TollsAmount],
        tr.[TotalAmount]
    FROM [dbo].[Trip] as tr
        JOIN dbo.[Date] as dt
        ON  tr.DateID = dt.DateID
    ```

    <span data-ttu-id="a852c-256">Ez a lekérdezés eltart egy ideig, mert az SQL Data Warehouse tooshuffle adatok előtt hello illesztési műveleteket hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="a852c-256">This query takes a while because SQL Data Warehouse has tooshuffle data before it can perform hello join.</span></span> <span data-ttu-id="a852c-257">Illesztések ne legyen tooshuffle adatforrás, ha azok hello tervezett toojoin adatok ugyanúgy terjesztése történik.</span><span class="sxs-lookup"><span data-stu-id="a852c-257">Joins do not have tooshuffle data if they are designed toojoin data in hello same way it is distributed.</span></span> <span data-ttu-id="a852c-258">Ez egy mélyebb téma.</span><span class="sxs-lookup"><span data-stu-id="a852c-258">That's a deeper subject.</span></span> 

2. <span data-ttu-id="a852c-259">A statisztika sokat számít.</span><span class="sxs-lookup"><span data-stu-id="a852c-259">Statistics make a difference.</span></span> 
3. <span data-ttu-id="a852c-260">Futtassa a utasítás toocreate statisztika hello illesztési oszlop.</span><span class="sxs-lookup"><span data-stu-id="a852c-260">Run this statement toocreate statistics on hello join columns.</span></span>

    ```sql
    CREATE STATISTICS [dbo.Date DateID stats] ON dbo.Date (DateID);
    CREATE STATISTICS [dbo.Trip DateID stats] ON dbo.Trip (DateID);
    ```

    > [!NOTE]
    > <span data-ttu-id="a852c-261">Az SQL DW nem kezeli automatikusan a statisztikákat Ön helyett.</span><span class="sxs-lookup"><span data-stu-id="a852c-261">SQL DW does not automatically manage statistics for you.</span></span> <span data-ttu-id="a852c-262">A statisztikák fontosak a lekérdezések teljesítménye szempontjából, ezért határozottan javasoljuk, hogy hozzon létre statisztikákat, és frissítse azokat.</span><span class="sxs-lookup"><span data-stu-id="a852c-262">Statistics are important for query performance and it is highly recommended you create and update statistics.</span></span>
    > 
    > <span data-ttu-id="a852c-263">**Ettől kezdve hello legtöbb juttatás azzal, hogy a statisztika oszlopokon érintett illesztésekben, oszlopok használt hello a GROUP BY záradék és az oszlopok találhatók.**</span><span class="sxs-lookup"><span data-stu-id="a852c-263">**You gain hello most benefit by having statistics on columns involved in joins, columns used in hello WHERE clause and columns found in GROUP BY.**</span></span>
    >

3. <span data-ttu-id="a852c-264">Újra az Előfeltételek hello lekérdezés futtatása, és tekintse meg az összes teljesítmény különbséget.</span><span class="sxs-lookup"><span data-stu-id="a852c-264">Run hello query from Prerequisites again and observe any performance differences.</span></span> <span data-ttu-id="a852c-265">A lekérdezések teljesítményét hello különbségek nem lesz, mint a vertikális felskálázásával drasztikus, egy számlázhasson kell észlel.</span><span class="sxs-lookup"><span data-stu-id="a852c-265">While hello differences in query performance will not be as drastic as scaling up, you should notice a  speed-up.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="a852c-266">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a852c-266">Next steps</span></span>

<span data-ttu-id="a852c-267">Most már készen áll a tooquery, és részletesen.</span><span class="sxs-lookup"><span data-stu-id="a852c-267">You're now ready tooquery and explore.</span></span> <span data-ttu-id="a852c-268">Tekintse meg gyakorlati tanácsainkat és tippjeinket.</span><span class="sxs-lookup"><span data-stu-id="a852c-268">Check out our best practices or tips.</span></span>

<span data-ttu-id="a852c-269">Ha végzett feltárása hello nap, győződjön meg arról, hogy toopause példány!</span><span class="sxs-lookup"><span data-stu-id="a852c-269">If you're done exploring for hello day, make sure toopause your instance!</span></span> <span data-ttu-id="a852c-270">Éles, máris elkezdheti felfedezni hatalmas megtakarítások felfüggesztése és a méretezésről toomeet által az üzleti igényeknek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="a852c-270">In production, you can experience enormous savings by pausing and scaling toomeet your business needs.</span></span>

![Szünet](./media/sql-data-warehouse-get-started-tutorial/pause.png)

## <a name="useful-readings"></a><span data-ttu-id="a852c-272">Hasznos olvasmányok</span><span class="sxs-lookup"><span data-stu-id="a852c-272">Useful readings</span></span>

<span data-ttu-id="a852c-273">[Egyidejűség és a számítási feladatok kezelése][]</span><span class="sxs-lookup"><span data-stu-id="a852c-273">[Concurrency and Workload Management][]</span></span>

<span data-ttu-id="a852c-274">[Ajánlott eljárások az Azure SQL Data Warehouse-hoz][]</span><span class="sxs-lookup"><span data-stu-id="a852c-274">[Best practices for Azure SQL Data Warehouse][]</span></span>

<span data-ttu-id="a852c-275">[Lekérdezések figyelése][]</span><span class="sxs-lookup"><span data-stu-id="a852c-275">[Query Monitoring][]</span></span>

<span data-ttu-id="a852c-276">[A 10 leghasznosabb ajánlott eljárás nagyméretű relációs adattárházak létrehozásához][]</span><span class="sxs-lookup"><span data-stu-id="a852c-276">[Top 10 Best Practices for Building a Large Scale Relational Data Warehouse][]</span></span>

<span data-ttu-id="a852c-277">[Áttelepítési adatok tooAzure SQL Data Warehouse][]</span><span class="sxs-lookup"><span data-stu-id="a852c-277">[Migrating Data tooAzure SQL Data Warehouse][]</span></span>

[Egyidejűség és a számítási feladatok kezelése]: sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example
[Ajánlott eljárások az Azure SQL Data Warehouse-hoz]: sql-data-warehouse-best-practices.md#hash-distribute-large-tables
[Lekérdezések figyelése]: sql-data-warehouse-manage-monitor.md
[A 10 leghasznosabb ajánlott eljárás nagyméretű relációs adattárházak létrehozásához]: https://blogs.msdn.microsoft.com/sqlcat/2013/09/16/top-10-best-practices-for-building-a-large-scale-relational-data-warehouse/
[Áttelepítési adatok tooAzure SQL Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/



[!INCLUDE [Additional Resources](../../includes/sql-data-warehouse-article-footer.md)]

<!-- Internal Links -->
[Előfeltételek]: sql-data-warehouse-get-started-tutorial.md#prerequisites

<!--Other Web references-->
[Visual Studio]: https://www.visualstudio.com/
[SQL Server Management Studio]: https://msdn.microsoft.com/en-us/library/mt238290.aspx
