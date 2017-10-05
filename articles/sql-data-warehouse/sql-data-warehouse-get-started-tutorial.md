---
title: "Azure SQL Data Warehouse – Első lépéseket ismertető oktatóanyag | Microsoft Docs"
description: "Ez az oktatóanyag az Azure SQL Data Warehouse üzembe helyezését és adatokkal való feltöltését mutatja be. Emellett megismerkedhet a méretezés, a felfüggesztetés és a finomhangolás alapjaival is."
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
ms.openlocfilehash: 95e14824ba3b705bb909ec983652dd3305b98805
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-sql-data-warehouse"></a><span data-ttu-id="9ec4d-104">Bevezetés az SQL Data Warehouse használatába</span><span class="sxs-lookup"><span data-stu-id="9ec4d-104">Get started with SQL Data Warehouse</span></span>

<span data-ttu-id="9ec4d-105">Ez az oktatóanyag az Azure SQL Data Warehouse üzembe helyezését és adatokkal való feltöltését mutatja be.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-105">This tutorial shows how to provision and load data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="9ec4d-106">Emellett megismerkedhet a méretezés, a felfüggesztetés és a finomhangolás alapjaival is.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-106">You’ll also learn the basics about scaling, pausing, and tuning.</span></span> <span data-ttu-id="9ec4d-107">Az oktatóanyag elvégzése után készen áll majd az adattárház lekérdezésére és vizsgálatára.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-107">When you’re finished, you’ll be ready to query and explore your data warehouse.</span></span>

<span data-ttu-id="9ec4d-108">**Az oktatóanyag áttekintésének becsült ideje:** Ez egy példakódot is tartalmazó átfogó oktatóanyag, amelynek az elvégzése kb. 30 percet vesz igénybe, ha az előfeltételeket már teljesítette.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-108">**Estimated time to complete:** This is an end-to-end tutorial with example code that takes about 30 minutes to complete once you have met the prerequisites.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="9ec4d-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9ec4d-109">Prerequisites</span></span>

<span data-ttu-id="9ec4d-110">Ez az oktatóanyag azt feltételezi, hogy már ismeri az SQL Data Warehouse-zal kapcsolatos alapvető fogalmakat.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-110">The tutorial assumes you are familiar with SQL Data Warehouse basic concepts.</span></span> <span data-ttu-id="9ec4d-111">A bevezetésért lásd: [Mi az az SQL Data Warehouse?](sql-data-warehouse-overview-what-is.md)</span><span class="sxs-lookup"><span data-stu-id="9ec4d-111">If you need an introduction, see [What is SQL Data Warehouse?](sql-data-warehouse-overview-what-is.md)</span></span> 

### <a name="sign-up-for-microsoft-azure"></a><span data-ttu-id="9ec4d-112">Regisztráció a Microsoft Azure-ban</span><span class="sxs-lookup"><span data-stu-id="9ec4d-112">Sign up for Microsoft Azure</span></span>
<span data-ttu-id="9ec4d-113">Ha még nem rendelkezik Microsoft Azure-fiókkal, először regisztrálnia kell egyet a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-113">If you don't already have a Microsoft Azure account, you need to sign up for one to use this service.</span></span> <span data-ttu-id="9ec4d-114">Ha már rendelkezik fiókkal, ezt a lépést kihagyhatja.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-114">If you already have an account, you may skip this step.</span></span> 

1. <span data-ttu-id="9ec4d-115">Nyissa meg a fiókoldalakat: [https://azure.microsoft.com/account/](https://azure.microsoft.com/account/)</span><span class="sxs-lookup"><span data-stu-id="9ec4d-115">Navigate to the account pages [https://azure.microsoft.com/account/](https://azure.microsoft.com/account/)</span></span>
2. <span data-ttu-id="9ec4d-116">Hozzon létre egy ingyenes Azure-fiókot, vagy vásároljon egy fiókot.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-116">Create a free Azure account, or purchase an account.</span></span>
3. <span data-ttu-id="9ec4d-117">Kövesse az utasításokat</span><span class="sxs-lookup"><span data-stu-id="9ec4d-117">Follow the instructions</span></span>

### <a name="install-appropriate-sql-client-drivers-and-tools"></a><span data-ttu-id="9ec4d-118">A megfelelő SQL-ügyfélillesztők és -ügyféleszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="9ec4d-118">Install appropriate SQL client drivers and tools</span></span>

<span data-ttu-id="9ec4d-119">A legtöbb SQL-ügyféleszköz képes csatlakozni az SQL Data Warehouse-hoz a JDBC, az ODBC vagy az ADO.NET használatával.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-119">Most SQL client tools can connect to SQL Data Warehouse by using JDBC, ODBC, or ADO.NET.</span></span> <span data-ttu-id="9ec4d-120">Az SQL Data Warehouse által támogatott T-SQL-szolgáltatások széles köre miatt lehetséges, hogy egyes ügyfélalkalmazások nem teljes mértékben kompatibilisek az SQL Data Warehouse-zal.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-120">Due to the large number of T-SQL features that SQL Data Warehouse supports, some client applications are not fully compatible with SQL Data Warehouse.</span></span>

<span data-ttu-id="9ec4d-121">Ha Windows operációs rendszert használ, javasoljuk a [Visual Studio] vagy az [SQL Server Management Studio] használatát.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-121">If you are running a Windows operating system, we recommend using either [Visual Studio] or [SQL Server Management Studio].</span></span>

[!INCLUDE [Create a new logical server](../../includes/sql-data-warehouse-create-logical-server.md)] 

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="create-a-sql-data-warehouse"></a><span data-ttu-id="9ec4d-122">SQL Data Warehouse létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ec4d-122">Create a SQL Data Warehouse</span></span>

<span data-ttu-id="9ec4d-123">Az SQL Data Warehouse egy nagymértékben párhuzamos feldolgozáshoz kialakított speciális típusú adatbázis.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-123">A SQL Data Warehouse is a special type of database that is designed for massively parallel processing.</span></span> <span data-ttu-id="9ec4d-124">Az adatbázis több csomópontra van elosztva, és párhuzamosan dolgozza fel a lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-124">The database is distributed across multiple nodes and processes queries in parallel.</span></span> <span data-ttu-id="9ec4d-125">Az összes csomópont tevékenységét az SQL Data Warehouse vezérlő csomópontja vezényli.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-125">SQL Data Warehouse has a control node that orchestrates the activities of all the nodes.</span></span> <span data-ttu-id="9ec4d-126">Maguk a csomópontok SQL Database használatával kezelik az adatokat.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-126">The nodes themselves use SQL Database to manage your data.</span></span>  

> [!NOTE]
> <span data-ttu-id="9ec4d-127">Egy SQL Data Warehouse létrehozása egy új számlázható szolgáltatás létrejöttét eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-127">Creating a SQL Data Warehouse might result in a new billable service.</span></span>  <span data-ttu-id="9ec4d-128">További információ: [SQL Data Warehouse díjszabása](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span><span class="sxs-lookup"><span data-stu-id="9ec4d-128">For more information, see [SQL Data Warehouse pricing](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span></span>
>

### <a name="create-a-data-warehouse"></a><span data-ttu-id="9ec4d-129">Adattárház létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ec4d-129">Create a data warehouse</span></span>

1. <span data-ttu-id="9ec4d-130">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9ec4d-130">Sign into the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="9ec4d-131">Kattintson a **New** > **Databases** > **SQL Data Warehouse** (Új > Adatbázisok > SQL Data Warehouse) elemre.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-131">Click **New** > **Databases** > **SQL Data Warehouse**.</span></span>

    <span data-ttu-id="9ec4d-132">![NewBlade](../../includes/media/sql-data-warehouse-create-dw/blade-click-new.png) ![SelectDW](../../includes/media/sql-data-warehouse-create-dw/blade-select-dw.png)</span><span class="sxs-lookup"><span data-stu-id="9ec4d-132">![NewBlade](../../includes/media/sql-data-warehouse-create-dw/blade-click-new.png) ![SelectDW](../../includes/media/sql-data-warehouse-create-dw/blade-select-dw.png)</span></span>

3. <span data-ttu-id="9ec4d-133">Adja meg az üzembe helyezés részleteit</span><span class="sxs-lookup"><span data-stu-id="9ec4d-133">Fill out deployment details</span></span>

    <span data-ttu-id="9ec4d-134">**Database name** (Adatbázis neve): Tetszés szerint bármit megadhat.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-134">**Database Name**: Pick anything you'd like.</span></span> <span data-ttu-id="9ec4d-135">Ha több adattárházzal is rendelkezik, akkor javasoljuk, hogy a nevek tartalmazzák a régiót, a környezetet vagy hasonló részleteket (például: *mydw-westus-1-test*).</span><span class="sxs-lookup"><span data-stu-id="9ec4d-135">If you have multiple data warehouses, we recommend your names include details such as the region, environment, for example *mydw-westus-1-test*.</span></span>

    <span data-ttu-id="9ec4d-136">**Subscription** (Előfizetés): Az Ön Azure-előfizetése</span><span class="sxs-lookup"><span data-stu-id="9ec4d-136">**Subscription**: Your Azure subscription</span></span>

    <span data-ttu-id="9ec4d-137">**Erőforráscsoport**: Hozzon létre egy erőforráscsoportot, vagy használjon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-137">**Resource Group**: Create a resource group or use an existing resource group.</span></span>
    > [!NOTE]
    > <span data-ttu-id="9ec4d-138">Az erőforráscsoportok hasznosak az erőforrások adminisztrációja, például a hozzáférés-vezérlés körének meghatározása és a sablonalapú üzembe helyezés során.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-138">Resource groups are useful for resource administration such as scoping access control and templated deployment.</span></span> <span data-ttu-id="9ec4d-139">Tudjon meg többet az Azure erőforráscsoportjairól és az ajánlott eljárásokról [itt](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="9ec4d-139">Read more about Azure resource groups and best practices [here](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)</span></span>

    <span data-ttu-id="9ec4d-140">**Forrás**: Üres adatbázis</span><span class="sxs-lookup"><span data-stu-id="9ec4d-140">**Source**: Blank Database</span></span>

    <span data-ttu-id="9ec4d-141">**Kiszolgáló**: Válassza ki az [Előfeltételek] szakaszban létrehozott kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-141">**Server**: Select the server you created in [Prerequisites].</span></span>

    <span data-ttu-id="9ec4d-142">**Rendezés**: Hagyja meg az alapértelmezett SQL_Latin1_General_CP1_CI_AS beállítást.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-142">**Collation**: Leave the default collation SQL_Latin1_General_CP1_CI_AS.</span></span>

    <span data-ttu-id="9ec4d-143">**Teljesítmény kiválasztása**: Azt javasoljuk, hogy kezdje a standard 400DWU beállítással.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-143">**Select performance**: We recommend starting with the standard 400DWU.</span></span>

4. <span data-ttu-id="9ec4d-144">Válassza a **Rögzítés az irányítópulton** ![Rögzítés az irányítópulton](./media/sql-data-warehouse-get-started-tutorial/pin-to-dashboard.png) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-144">Choose **Pin to dashboard** ![Pin To Dashboard](./media/sql-data-warehouse-get-started-tutorial/pin-to-dashboard.png)</span></span>

5. <span data-ttu-id="9ec4d-145">Dőljön hátra, és várjon, amíg az adattárház üzembe helyezése megtörténik!</span><span class="sxs-lookup"><span data-stu-id="9ec4d-145">Sit back and wait for your data warehouse to deploy!</span></span> <span data-ttu-id="9ec4d-146">Ez a folyamat szokványos esetben több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-146">It's normal for this process to take several minutes.</span></span> <span data-ttu-id="9ec4d-147">A portál értesíti, amint az adattárház használatra kész.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-147">The portal notifies you when your data warehouse is ready to use.</span></span> 

## <a name="connect-to-sql-data-warehouse"></a><span data-ttu-id="9ec4d-148">Connect to SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="9ec4d-148">Connect to SQL Data Warehouse</span></span>

<span data-ttu-id="9ec4d-149">Az oktatóanyagban az SQL Server Management Studio (SSMS) segítségével csatlakozunk az adattárházhoz.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-149">This tutorial uses SQL Server Management Studio (SSMS) to connect to the data warehouse.</span></span> <span data-ttu-id="9ec4d-150">A következő támogatott összekötőkön keresztül is csatlakozhat az SQL Data Warehouse-hoz: ADO.NET, JDBC, ODBC és PHP.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-150">You can connect to SQL Data Warehouse through these supported connectors: ADO.NET, JDBC, ODBC, and PHP.</span></span> <span data-ttu-id="9ec4d-151">Ne feledje, hogy a Microsoft által nem támogatott eszközök esetében a funkcionalitás korlátozott lehet.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-151">Remember, functionality might be limited for tools that are not supported by Microsoft.</span></span>


### <a name="get-connection-information"></a><span data-ttu-id="9ec4d-152">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="9ec4d-152">Get connection information</span></span>

<span data-ttu-id="9ec4d-153">Az adattárházhoz való kapcsolódáshoz az [Előfeltételek] szakaszban létrehozott logikai SQL-kiszolgálón keresztül kell csatlakoznia.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-153">To connect to your data warehouse, you need to connect through the logical SQL server you created in [Prerequisites].</span></span>

1. <span data-ttu-id="9ec4d-154">Válassza ki az adattárházat az irányítópulton vagy keresse meg az erőforrások között.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-154">Select your data warehouse from the dashboard or search for it in your resources.</span></span>

    ![SQL Data Warehouse irányítópult](./media/sql-data-warehouse-get-started-tutorial/sql-dw-dashboard.png)

2. <span data-ttu-id="9ec4d-156">Keresse meg a logikai SQL-kiszolgáló teljes nevét.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-156">Find the full name for the logical SQL server.</span></span>

    ![Kiszolgálónév kiválasztása](./media/sql-data-warehouse-get-started-tutorial/select-server.png)

3. <span data-ttu-id="9ec4d-158">Nyissa meg az SSMS-t, és az Object Explorer használatával csatlakozzon ehhez a kiszolgálóhoz az [Előfeltételek] szakaszban létrehozott kiszolgálói rendszergazdai hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-158">Open SSMS and use object explorer to connect to this server using the server admin credentials you created in [Prerequisites]</span></span>

    ![Csatlakozás SSMS segítségével](./media/sql-data-warehouse-get-started-tutorial/ssms-connect.png)

<span data-ttu-id="9ec4d-160">Ha minden jól megy, mostanra kapcsolódnia kell a logikai SQL-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-160">If all goes correctly, you should now be connected to your logical SQL server.</span></span> <span data-ttu-id="9ec4d-161">Miután kiszolgálói rendszergazdaként jelentkezett be, a kiszolgáló által futtatott bármelyik adatbázishoz kapcsolódhat, beleértve a master adatbázist.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-161">Since you logged in as the server admin, you can connect to any database hosted by the server, including the master database.</span></span> 

<span data-ttu-id="9ec4d-162">Csak egyetlen kiszolgálói rendszergazdai fiók létezik, és az összes felhasználó közül ez rendelkezik a legtöbb jogosultsággal.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-162">There is only one server admin account and it has the most privileges of any user.</span></span> <span data-ttu-id="9ec4d-163">Legyen óvatos, és csak kevesekkel tudassa a rendszergazdai jelszót a szervezetben.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-163">Be careful not to allow too many people in your organization to know the admin password.</span></span> 

<span data-ttu-id="9ec4d-164">Emellett rendelkezhet egy Azure Active Directory-rendszergazdai fiókkal is.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-164">You can also have an Azure active directory admin account.</span></span> <span data-ttu-id="9ec4d-165">Ennek részleteit itt nem ismertetjük.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-165">We don't provide the details here.</span></span> <span data-ttu-id="9ec4d-166">Ha többet szeretne megtudni az Azure Active Directory-alapú hitelesítéssel kapcsolatban, lásd: [Azure AD-hitelesítés](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication).</span><span class="sxs-lookup"><span data-stu-id="9ec4d-166">If you want to learn more about using Azure Active Directory authentication, see [Azure AD authentication](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication).</span></span>

<span data-ttu-id="9ec4d-167">Ezután a további bejelentkezések és felhasználók létrehozásával ismerkedünk meg.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-167">Next, we explore creating additional logins and users.</span></span>


## <a name="create-a-database-user"></a><span data-ttu-id="9ec4d-168">Adatbázis-felhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ec4d-168">Create a database user</span></span>

<span data-ttu-id="9ec4d-169">Ebben a lépésben egy felhasználói fiókot hozhat létre az adattárház eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-169">In this step, you create a user account to access your data warehouse.</span></span> <span data-ttu-id="9ec4d-170">Megmutatjuk azt is, hogyan engedélyezheti a felhasználó számára a nagy mennyiségű memória- és processzor-erőforrást igénylő lekérdezések futtatását.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-170">We also show you how to give that user the ability to run queries with a large amount of memory and CPU resources.</span></span>

### <a name="notes-about-resource-classes-for-allocating-resources-to-queries"></a><span data-ttu-id="9ec4d-171">Az erőforrásosztályokkal kapcsolatos megjegyzések az erőforrások lekérdezésekhez való kiosztásához</span><span class="sxs-lookup"><span data-stu-id="9ec4d-171">Notes about resource classes for allocating resources to queries</span></span>

- <span data-ttu-id="9ec4d-172">Az adatok biztonsága érdekében a kiszolgálói rendszergazdát ne használja a lekérdezések éles adatbázisokon való futtatásához.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-172">To keep your data safe, don't use the server admin to run queries on your production databases.</span></span> <span data-ttu-id="9ec4d-173">Az összes közül ez a felhasználó rendelkezik a legtöbb jogosultsággal, így ha ezzel hajt végre műveleteket a felhasználói adatokon, veszélyeztetheti az adatokat.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-173">It has the most privileges of any user and using it to perform operations on user data puts your data at risk.</span></span> <span data-ttu-id="9ec4d-174">Továbbá, mivel a kiszolgálói rendszergazda felügyeleti tevékenységeket hivatott végezni, a működéséhez kis mennyiségű memória- és processzor-erőforrást használ csak.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-174">Also, since the server admin is meant to perform management operations, it runs operations with only a small allocation of memory and CPU resources.</span></span> 

- <span data-ttu-id="9ec4d-175">Az SQL Data Warehouse előre meghatározott adatbázis-szerepköröket, úgynevezett erőforrásosztályokat alkalmaz a különböző mennyiségű memória, processzor-erőforrás és egyidejű hely lefoglalásához az egyes felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-175">SQL Data Warehouse uses pre-defined database roles, called resource classes, to allocate different amounts of memory, CPU resources, and concurrency slots to users.</span></span> <span data-ttu-id="9ec4d-176">Az egyes felhasználók kicsi, közepes, nagy vagy extra nagy erőforrásosztályokba tartozhatnak.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-176">Each user can belong to a small, medium, large, or extra-large resource class.</span></span> <span data-ttu-id="9ec4d-177">Az adott falhasználó erőforrásosztálya határozza meg, hogy a felhasználó milyen erőforrásokkal rendelkezik a lekérdezések és betöltési műveletek futtatásához.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-177">The user's resource class determines the resources the user has to run queries and load operations.</span></span>

- <span data-ttu-id="9ec4d-178">Az optimális adattömörítéshez szükség lehet arra, hogy nagy vagy nagyon nagy erőforrást foglaljon le a felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-178">For optimal data compression, the user may need to load with large or extra large resource allocations.</span></span> <span data-ttu-id="9ec4d-179">További információkat az erőforrásosztályokról [itt](./sql-data-warehouse-develop-concurrency.md#resource-classes) talál.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-179">Read more about resource classes [here](./sql-data-warehouse-develop-concurrency.md#resource-classes):</span></span>

### <a name="create-an-account-that-can-control-a-database"></a><span data-ttu-id="9ec4d-180">Az adatbázisok vezérlésére alkalmas fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ec4d-180">Create an account that can control a database</span></span>

<span data-ttu-id="9ec4d-181">Mivel jelenleg kiszolgálói rendszergazdaként van bejelentkezve, rendelkezik megfelelő jogosultsággal a bejelentkezések és a felhasználók létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-181">Since you are currently logged in as the server admin you have permissions to create logins and users.</span></span>

1. <span data-ttu-id="9ec4d-182">Az SSMS vagy más lekérdezésügyfél használatával nyisson egy új lekérdezést a **masteren**.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-182">Using SSMS or another query client, open a new query for **master**.</span></span>

    ![Új lekérdezés a Master adatbázison](./media/sql-data-warehouse-get-started-tutorial/query-on-server.png)

    ![Új lekérdezés a Master1 adatbázison](./media/sql-data-warehouse-get-started-tutorial/query-on-master.png)

2. <span data-ttu-id="9ec4d-185">Futtassa a következő T-SQL parancsot a lekérdezésablakban, és hozzon létre egy bejelentkezést MedRCLogin és egy felhasználót LoadingUser néven.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-185">In the query window, run this T-SQL command to create a login named MedRCLogin and user named LoadingUser.</span></span> <span data-ttu-id="9ec4d-186">Ez a bejelentkezés képes kapcsolódni a logikai SQL-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-186">This login can connect to the logical SQL server.</span></span>

    ```sql
    CREATE LOGIN MedRCLogin WITH PASSWORD = 'a123reallySTRONGpassword!';
    CREATE USER LoadingUser FOR LOGIN MedRCLogin;
    ```

3. <span data-ttu-id="9ec4d-187">Most az *SQL Data Warehouse-adatbázis* lekérdezéséhez hozzon létre egy adatbázis-felhasználót azon bejelentkezés alapján, amelyet az adatbázishoz való hozzáféréshez és az azon való tevékenységek elvégzéséhez hozott létre.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-187">Now querying the *SQL Data Warehouse database*, create a database user based on the login you created to access and perform operations on the database.</span></span>

    ```sql
    CREATE USER LoadingUser FOR LOGIN MedRCLogin;
    ```

4. <span data-ttu-id="9ec4d-188">Adjon az adatbázis-felhasználónak felügyeleti jogosultságot az NYT nevű adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-188">Give the database user control permissions to the database called NYT.</span></span> 

    ```sql
    GRANT CONTROL ON DATABASE::[NYT] to LoadingUser;
    ```
    > [!NOTE]
    > <span data-ttu-id="9ec4d-189">Ha az adatbázis nevében található kötőjel, mindenképp foglalja a nevet szögletes zárójelek közé.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-189">If your database name has hyphens in it, be sure to wrap it in brackets!</span></span> 
    >

### <a name="give-the-user-medium-resource-allocations"></a><span data-ttu-id="9ec4d-190">Közepes erőforrás-mennyiség lefoglalása a felhasználó számára</span><span class="sxs-lookup"><span data-stu-id="9ec4d-190">Give the user medium resource allocations</span></span>

1. <span data-ttu-id="9ec4d-191">A következő T-SQL parancs futtatásával tegye a felhasználót a mediumrc nevű közepes erőforrásosztály tagjává.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-191">Run this T-SQL command to make it a member of the medium resource class, which is called mediumrc.</span></span> 

    ```sql
    EXEC sp_addrolemember 'mediumrc', 'LoadingUser';
    ```
    > [!NOTE]
    > <span data-ttu-id="9ec4d-192">Az egyidejűségre és az erőforrásosztályokra vonatkozó további információkért kattintson [ide](sql-data-warehouse-develop-concurrency.md#resource-classes).</span><span class="sxs-lookup"><span data-stu-id="9ec4d-192">Click [here](sql-data-warehouse-develop-concurrency.md#resource-classes) to learn more about concurrency and resource classes!</span></span> 
    >

2. <span data-ttu-id="9ec4d-193">Csatlakozás a logikai kiszolgálóhoz az új hitelesítő adatokkal</span><span class="sxs-lookup"><span data-stu-id="9ec4d-193">Connect to the logical server with the new credentials</span></span>

    ![Bejelentkezés az új bejelentkezési adatokkal](./media/sql-data-warehouse-get-started-tutorial/new-login.png)


## <a name="load-data-from-azure-blob-storage"></a><span data-ttu-id="9ec4d-195">Adatok betöltése az Azure Blob Storage-ből</span><span class="sxs-lookup"><span data-stu-id="9ec4d-195">Load data from Azure blob storage</span></span>

<span data-ttu-id="9ec4d-196">Most már készen áll az adatok betöltésére az adattárházba.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-196">You are now ready to load data into your data warehouse.</span></span> <span data-ttu-id="9ec4d-197">Ez a lépés bemutatja, hogyan töltheti be a New York-i taxik adatait egy nyilvános Azure tárolóblobból.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-197">This step shows you how to load New York City taxi cab data from a public Azure storage blob.</span></span> 

- <span data-ttu-id="9ec4d-198">Az adatok az SQL Data Warehouse-ba való betöltésének gyakori módja, ha először áthelyezi az adatokat az Azure Blob Storage-be, majd eztán tölti be azokat az adattárházba.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-198">A common way to load data into SQL Data Warehouse is to first move the data to Azure blob storage, and then load it into your data warehouse.</span></span> <span data-ttu-id="9ec4d-199">Hogy könnyebben átlássa a betöltés folyamatát, a New York-i taxik adatait már eleve egy nyilvános Azure tárolóblobban tároljuk.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-199">To make it easier to understand how to load, we have New York taxi cab data already hosted in a public Azure storage blob.</span></span> 

- <span data-ttu-id="9ec4d-200">Ha később szeretné megismerni az adatok Azure Blob Storage-be való áthelyezésének vagy a forrásból közvetlenül az SQL Data Warehouse-ba való betöltésének a módját, olvassa el a [betöltés áttekintését](sql-data-warehouse-overview-load.md).</span><span class="sxs-lookup"><span data-stu-id="9ec4d-200">For future reference, to learn how to get your data to Azure blob storage or to load it directly from your source into SQL Data Warehouse, see the [loading overview](sql-data-warehouse-overview-load.md).</span></span>


### <a name="define-external-data"></a><span data-ttu-id="9ec4d-201">Külső adatok meghatározása</span><span class="sxs-lookup"><span data-stu-id="9ec4d-201">Define external data</span></span>

1. <span data-ttu-id="9ec4d-202">Hozzon létre egy főkulcsot.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-202">Create a master key.</span></span> <span data-ttu-id="9ec4d-203">Adatbázisonként csak egyszer kell főkulcsot létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-203">You only need to create a master key once per database.</span></span> 

    ```sql
    CREATE MASTER KEY;
    ```

2. <span data-ttu-id="9ec4d-204">Határozza meg a helyet az Azure blobban, ahol a taxik adatai találhatók.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-204">Define the location of the Azure blob that contains the taxi cab data.</span></span>  

    ```sql
    CREATE EXTERNAL DATA SOURCE NYTPublic
    WITH
    (
        TYPE = Hadoop,
        LOCATION = 'wasbs://2013@nytpublic.blob.core.windows.net/'
    );
    ```

3. <span data-ttu-id="9ec4d-205">Határozza meg a külső fájlformátumokat.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-205">Define the external file formats</span></span>

    <span data-ttu-id="9ec4d-206">A ```CREATE EXTERNAL FILE FORMAT``` parancs használatával adhatja meg a külső adatokat tartalmazó fájlok formátumát.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-206">The ```CREATE EXTERNAL FILE FORMAT``` command is used to specify the format of files that contain the external data.</span></span> <span data-ttu-id="9ec4d-207">Ezek egy vagy több karakter, az úgynevezett elválasztó karakterek használatával vannak elválasztva.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-207">They contain text separated by one or more characters called delimiters.</span></span> <span data-ttu-id="9ec4d-208">Bemutatási célokból a taxik adatait itt tömörítetlen adatokként és GZIP formátumban tömörített adatokként is tároljuk.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-208">For demonstration purposes, the taxi cab data is stored both as uncompressed data and as gzip compressed data.</span></span>

    <span data-ttu-id="9ec4d-209">A következő T-SQL parancsok futtatásával határozhatja meg a két különböző, a tömörítetlen és a tömörített formátumot.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-209">Run these T-SQL commands to define two different formats: uncompressed and compressed.</span></span>

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

4.  <span data-ttu-id="9ec4d-210">Hozzon létre egy sémát a külső fájlformátum számára.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-210">Create a schema for your external file format.</span></span> 

    ```sql
    CREATE SCHEMA ext;
    ```
5. <span data-ttu-id="9ec4d-211">Hozza létre a külső táblákat.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-211">Create the external tables.</span></span> <span data-ttu-id="9ec4d-212">Ezek a táblák az Azure Blob Storage-ben tárolt adatokra hivatkoznak.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-212">These tables reference data stored in Azure blob storage.</span></span> <span data-ttu-id="9ec4d-213">A következő T-SQL parancsok futtatásával hozzon létre több külső táblát, amelyek mind a külső adatforrásban korábban meghatározott Azure-blobra mutatnak.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-213">Run the following T-SQL commands to create several external tables that all point to the Azure blob we defined previously in our external data source.</span></span>

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

### <a name="import-the-data-from-azure-blob-storage"></a><span data-ttu-id="9ec4d-214">Importálja az adatokat az Azure Blob Storage-ből.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-214">Import the data from Azure blob storage.</span></span>

<span data-ttu-id="9ec4d-215">Az SQL Data Warehouse támogat egy CREATE TABLE AS SELECT (CTAS) nevű kulcsutasítást.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-215">SQL Data Warehouse supports a key statement called CREATE TABLE AS SELECT (CTAS).</span></span> <span data-ttu-id="9ec4d-216">Ez az utasítás létrehoz egy új táblát egy kiválasztási utasítás eredményei alapján.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-216">This statement creates a new table based on the results of a select statement.</span></span> <span data-ttu-id="9ec4d-217">Az új tábla oszlopai és adattípusai megegyeznek a kiválasztási utasítás eredményeivel.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-217">The new table has the same columns and data types as the results of the select statement.</span></span>  <span data-ttu-id="9ec4d-218">Ez egy elegáns módja az adatok betöltésének az Azure Blob Storage-ből az SQL Data Warehouse-ba.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-218">This is an elegant way to import data from Azure blob storage into SQL Data Warehouse.</span></span>

1. <span data-ttu-id="9ec4d-219">Az adatok importálásához futtassa ezt a szkriptet.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-219">Run this script to import your data.</span></span>

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

2. <span data-ttu-id="9ec4d-220">A betöltés közben megtekintheti az adatokat.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-220">View your data as it loads.</span></span>

   <span data-ttu-id="9ec4d-221">Több GB-nyi adatot tölt be és tömörít nagy teljesítményű fürtözött oszlopcentrikus indexekbe.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-221">You’re loading several GBs of data and compressing it into highly performant clustered columnstore indexes.</span></span> <span data-ttu-id="9ec4d-222">Futtassa az alábbi lekérdezést, amely dinamikus felügyeleti nézetekkel (DMV-k) jeleníti meg a töltés állapotát.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-222">Run the following query that uses a dynamic management views (DMVs) to show the status of the load.</span></span> <span data-ttu-id="9ec4d-223">A lekérdezés elindítása után igyon egy kávét, vagy szerezzen valami rágcsálnivalót, amíg az SQL Data Warehouse keményen dolgozik.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-223">After starting the query, grab a coffee and a snack while SQL Data Warehouse does some heavy lifting.</span></span>
    
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

3. <span data-ttu-id="9ec4d-224">Tekintse meg az összes rendszerlekérdezést.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-224">View all system queries.</span></span>

    ```sql
    SELECT * FROM sys.dm_pdw_exec_requests;
    ```

4. <span data-ttu-id="9ec4d-225">Láthatja, ahogy adatai szépen betöltődnek az Azure SQL Data Warehouse-ba.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-225">Enjoy seeing your data nicely loaded into your Azure SQL Data Warehouse.</span></span>

    ![Betöltött adatok megjelenítése](./media/sql-data-warehouse-get-started-tutorial/see-data-loaded.png)


## <a name="improve-query-performance"></a><span data-ttu-id="9ec4d-227">Jobb lekérdezési teljesítmény</span><span class="sxs-lookup"><span data-stu-id="9ec4d-227">Improve query performance</span></span>

<span data-ttu-id="9ec4d-228">Számos mód létezik a lekérdezési teljesítmény javítására és a kiemelkedően gyors teljesítmény elérésére, amelyre az SQL Data Warehouse-t tervezték.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-228">There are several ways to improve query performance and to achieve the high-speed performance that SQL Data Warehouse is designed to provide.</span></span>  

### <a name="see-the-effect-of-scaling-on-query-performance"></a><span data-ttu-id="9ec4d-229">A lekérdezésiteljesítmény-méretezés hatásának megtekintése</span><span class="sxs-lookup"><span data-stu-id="9ec4d-229">See the effect of scaling on query performance</span></span> 

<span data-ttu-id="9ec4d-230">Az egyik módja a lekérdezési teljesítmény javításának az, ha méretezi az erőforrásokat az adattárház DWU szolgáltatási szintjének módosításával.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-230">One way to improve query performance is to scale resources by changing the DWU service level for your data warehouse.</span></span> <span data-ttu-id="9ec4d-231">Minden szolgáltatási szintnek nagyobb a költsége, de bármikor visszaválthat vagy szüneteltetheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-231">Each service level costs more, but you can scale back or pause resources at any time.</span></span> 

<span data-ttu-id="9ec4d-232">Ebben a lépésben összehasonlítja a teljesítményt két különböző DWU-beállításnál.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-232">In this step, you compare performance at two different DWU settings.</span></span>

<span data-ttu-id="9ec4d-233">Először csökkentse le a DWU-k számát 100-ra, hogy láthassuk, hogyan teljesít egyetlen számítási csomópont önállóan.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-233">First, let's scale the sizing down to 100 DWU so we can get an idea of how one compute node might perform on its own.</span></span>

1. <span data-ttu-id="9ec4d-234">Lépjen a portálra, és válassza ki az SQL Data Warehouse-t.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-234">Go to the portal and select your SQL Data Warehouse.</span></span>

2. <span data-ttu-id="9ec4d-235">Válassza ki a méretet az SQL Data Warehouse panelen.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-235">Select scale in the SQL Data Warehouse blade.</span></span> 

    ![DW méretezése a portálról](./media/sql-data-warehouse-get-started-tutorial/scale-dw.png)

3. <span data-ttu-id="9ec4d-237">Csökkentse a teljesítményt a sávon 100 DWU-ra, és nyomja le a Mentés gombot.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-237">Scale down the performance bar to 100 DWU and hit save.</span></span>

    ![Méretezés és mentés](./media/sql-data-warehouse-get-started-tutorial/scale-and-save.png)

4. <span data-ttu-id="9ec4d-239">Várjon, amíg a méretezési művelet befejeződik.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-239">Wait for your scale operation to finish.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9ec4d-240">A méretezés módosítása közben nem futhatnak lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-240">Queries cannot run while changing the scale.</span></span> <span data-ttu-id="9ec4d-241">A méretezés az épp futó lekérdezéseket **megszakítja**.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-241">Scaling **kills** your currently running queries.</span></span> <span data-ttu-id="9ec4d-242">A művelet befejezése után újraindíthatja őket.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-242">You can restart them when the operation is finished.</span></span>
    >
    
5. <span data-ttu-id="9ec4d-243">Végezzen egy vizsgálati műveletet az utazási adatokon, és válassza az első egymillió bejegyzést minden oszlopban.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-243">Do a scan operation on the trip data, selecting the top million entries for all the columns.</span></span> <span data-ttu-id="9ec4d-244">Ha szeretne gyorsabban továbblépni, választhat kevesebb sort is.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-244">If you're eager to move on quickly, feel free to select fewer rows.</span></span> <span data-ttu-id="9ec4d-245">Jegyezze fel, hogy mennyi időbe telik ennek a műveletnek az elvégzése.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-245">Take note of the time it takes to run this operation.</span></span>

    ```sql
    SELECT TOP(1000000) * FROM dbo.[Trip]
    ```
6. <span data-ttu-id="9ec4d-246">Méretezze az adattárházat vissza 400 DWU-ra.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-246">Scale your data warehouse back to 400 DWU.</span></span> <span data-ttu-id="9ec4d-247">Ne feledje, hogy minden 100 DWU egy újabb számítási csomópontot ad az Azure SQL Data Warehouse-hoz.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-247">Remember, each 100 DWU is adding another compute node to your Azure SQL Data Warehouse.</span></span>

7. <span data-ttu-id="9ec4d-248">Futtassa újra a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-248">Run the query again!</span></span> <span data-ttu-id="9ec4d-249">Jelentős eltérést kell tapasztalnia.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-249">You should notice a significant difference.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="9ec4d-250">Mivel a lekérdezés számos adatot ad vissza, az SSMS-t futtató gép sávszélességének rendelkezésre állása teljesítménybeli szűk keresztmetszetet eredményezhet.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-250">Because the query returns a lot of data, the bandwidth availability of the machine running SSMS may be a performance bottleneck.</span></span> <span data-ttu-id="9ec4d-251">Emiatt lehetséges, hogy semmilyen teljesítménybeli javulást nem fog tapasztalni.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-251">This can result in you not seeing any performance improvements!</span></span>

> [!NOTE]
> <span data-ttu-id="9ec4d-252">Ennek az az oka, hogy az SQL Data Warehouse nagymértékben párhuzamos feldolgozást használ.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-252">Since SQL Data Warehouse uses massively parallel processing.</span></span> <span data-ttu-id="9ec4d-253">Olyan lekérdezésekkel tapasztalható meg az Azure SQL Data Warehouse igazi ereje, amelyek több millió soron hajtanak végre elemzési funkciókat.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-253">Queries that scan or perform analytic functions on millions of rows experience the true power of Azure SQL Data Warehouse.</span></span>
>

### <a name="see-the-effect-of-statistics-on-query-performance"></a><span data-ttu-id="9ec4d-254">A statisztika hatásának lekérdezések teljesítményére gyakorolt hatása</span><span class="sxs-lookup"><span data-stu-id="9ec4d-254">See the effect of statistics on query performance</span></span>

1. <span data-ttu-id="9ec4d-255">Futtasson egy lekérdezést, amely összekapcsolja a Date (Dátum) táblát a Trip (Utazás) táblával.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-255">Run a query that joins the Date table with the Trip table</span></span>

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

    <span data-ttu-id="9ec4d-256">Ez a lekérdezés eltart egy darabig, mert az SQL Data Warehouse-nak mozgatnia kell az adatokat az összekapcsolás végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-256">This query takes a while because SQL Data Warehouse has to shuffle data before it can perform the join.</span></span> <span data-ttu-id="9ec4d-257">Nem kell mozgatni az adatokat az összekapcsolásokhoz, ha úgy lettek megtervezve, hogy az elosztással megegyező módon kapcsolják össze az adatokat.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-257">Joins do not have to shuffle data if they are designed to join data in the same way it is distributed.</span></span> <span data-ttu-id="9ec4d-258">Ez egy mélyebb téma.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-258">That's a deeper subject.</span></span> 

2. <span data-ttu-id="9ec4d-259">A statisztika sokat számít.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-259">Statistics make a difference.</span></span> 
3. <span data-ttu-id="9ec4d-260">Ezt az utasítást futtatva létrehozhat statisztikát az összekapcsolási oszlopokhoz.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-260">Run this statement to create statistics on the join columns.</span></span>

    ```sql
    CREATE STATISTICS [dbo.Date DateID stats] ON dbo.Date (DateID);
    CREATE STATISTICS [dbo.Trip DateID stats] ON dbo.Trip (DateID);
    ```

    > [!NOTE]
    > <span data-ttu-id="9ec4d-261">Az SQL DW nem kezeli automatikusan a statisztikákat Ön helyett.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-261">SQL DW does not automatically manage statistics for you.</span></span> <span data-ttu-id="9ec4d-262">A statisztikák fontosak a lekérdezések teljesítménye szempontjából, ezért határozottan javasoljuk, hogy hozzon létre statisztikákat, és frissítse azokat.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-262">Statistics are important for query performance and it is highly recommended you create and update statistics.</span></span>
    > 
    > <span data-ttu-id="9ec4d-263">**A legnagyobb előnnyel az jár, ha az összekapcsolások részét képező, a WHERE záradékban használt és a GROUP BY elemben megtalálható oszlopok statisztikáit készíti el.**</span><span class="sxs-lookup"><span data-stu-id="9ec4d-263">**You gain the most benefit by having statistics on columns involved in joins, columns used in the WHERE clause and columns found in GROUP BY.**</span></span>
    >

3. <span data-ttu-id="9ec4d-264">Futtassa újra az Előfeltételek szakaszban szereplő lekérdezést, és figyelje meg a teljesítménybeli különbséget.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-264">Run the query from Prerequisites again and observe any performance differences.</span></span> <span data-ttu-id="9ec4d-265">Bár a lekérdezési teljesítmény változása nem olyan drámai, mint a felskálázás esetében, gyorsulás figyelhető meg.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-265">While the differences in query performance will not be as drastic as scaling up, you should notice a  speed-up.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="9ec4d-266">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9ec4d-266">Next steps</span></span>

<span data-ttu-id="9ec4d-267">Készen áll a lekérdezésre és vizsgálódásra.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-267">You're now ready to query and explore.</span></span> <span data-ttu-id="9ec4d-268">Tekintse meg gyakorlati tanácsainkat és tippjeinket.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-268">Check out our best practices or tips.</span></span>

<span data-ttu-id="9ec4d-269">Ha a mai napra befejezte a vizsgálódást, szüneteltesse a példány működését.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-269">If you're done exploring for the day, make sure to pause your instance!</span></span> <span data-ttu-id="9ec4d-270">Üzemi környezetben hatalmas megtakarításokat érhet el, ha üzleti igényei szerint szünetelteti és méretezi a működést.</span><span class="sxs-lookup"><span data-stu-id="9ec4d-270">In production, you can experience enormous savings by pausing and scaling to meet your business needs.</span></span>

![Szünet](./media/sql-data-warehouse-get-started-tutorial/pause.png)

## <a name="useful-readings"></a><span data-ttu-id="9ec4d-272">Hasznos olvasmányok</span><span class="sxs-lookup"><span data-stu-id="9ec4d-272">Useful readings</span></span>

<span data-ttu-id="9ec4d-273">[Egyidejűség és a számítási feladatok kezelése][]</span><span class="sxs-lookup"><span data-stu-id="9ec4d-273">[Concurrency and Workload Management][]</span></span>

<span data-ttu-id="9ec4d-274">[Ajánlott eljárások az Azure SQL Data Warehouse-hoz][]</span><span class="sxs-lookup"><span data-stu-id="9ec4d-274">[Best practices for Azure SQL Data Warehouse][]</span></span>

<span data-ttu-id="9ec4d-275">[Lekérdezések figyelése][]</span><span class="sxs-lookup"><span data-stu-id="9ec4d-275">[Query Monitoring][]</span></span>

<span data-ttu-id="9ec4d-276">[A 10 leghasznosabb ajánlott eljárás nagyméretű relációs adattárházak létrehozásához][]</span><span class="sxs-lookup"><span data-stu-id="9ec4d-276">[Top 10 Best Practices for Building a Large Scale Relational Data Warehouse][]</span></span>

<span data-ttu-id="9ec4d-277">[Adatok áttelepítése az Azure SQL Data Warehouse-ba][]</span><span class="sxs-lookup"><span data-stu-id="9ec4d-277">[Migrating Data to Azure SQL Data Warehouse][]</span></span>

[Egyidejűség és a számítási feladatok kezelése]: sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example
[Ajánlott eljárások az Azure SQL Data Warehouse-hoz]: sql-data-warehouse-best-practices.md#hash-distribute-large-tables
[Lekérdezések figyelése]: sql-data-warehouse-manage-monitor.md
[A 10 leghasznosabb ajánlott eljárás nagyméretű relációs adattárházak létrehozásához]: https://blogs.msdn.microsoft.com/sqlcat/2013/09/16/top-10-best-practices-for-building-a-large-scale-relational-data-warehouse/
[Adatok áttelepítése az Azure SQL Data Warehouse-ba]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/



[!INCLUDE [Additional Resources](../../includes/sql-data-warehouse-article-footer.md)]

<!-- Internal Links -->
[Előfeltételek]: sql-data-warehouse-get-started-tutorial.md#prerequisites

<!--Other Web references-->
[Visual Studio]: https://www.visualstudio.com/
[SQL Server Management Studio]: https://msdn.microsoft.com/en-us/library/mt238290.aspx
