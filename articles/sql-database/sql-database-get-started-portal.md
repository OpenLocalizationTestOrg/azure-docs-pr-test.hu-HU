---
title: "Azure Portal: SQL-adatbázis létrehozása | Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy SQL Database logikai kiszolgálóhoz kiszolgálószintű tűzfalszabályt és adatbázisok hello Azure-portálon. Azt is megtudhatja, tooquery hello Azure-portál használatával Azure SQL-adatbázis."
keywords: "oktatóanyag az SQL Database használatához, SQL-adatbázis létrehozása"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: portal
ms.devlang: na
ms.topic: hero-article
ms.date: 05/30/2017
ms.author: carlrab
ms.openlocfilehash: d30352d834f2007e0b6b3eabfc3c108c61479b22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-sql-database-in-hello-azure-portal"></a><span data-ttu-id="90182-105">Hozzon létre egy Azure SQL-adatbázis hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="90182-105">Create an Azure SQL database in hello Azure portal</span></span>

<span data-ttu-id="90182-106">A gyors üzembe helyezési útmutató végigvezeti hogyan toocreate egy SQL-adatbázis használati ideje az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="90182-106">This quick start tutorial walks through how toocreate a SQL database in Azure.</span></span> <span data-ttu-id="90182-107">Az Azure SQL-adatbázis egy "adatbázis-a-szolgáltatás" ajánlat, amely lehetővé teszi toorun és a skála magas rendelkezésre állású SQL Server-adatbázisok hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="90182-107">Azure SQL Database is a “Database-as-a-Service” offering that enables you toorun and scale highly available SQL Server databases in hello cloud.</span></span> <span data-ttu-id="90182-108">A gyors üzembe helyezési bemutatja, hogyan tooget el hozzon létre egy SQL-adatbázist hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="90182-108">This quick start shows you how tooget started by creating a SQL database using hello Azure portal.</span></span>

<span data-ttu-id="90182-109">Ha nem rendelkezik Azure-előfizetéssel, első lépésként mindössze néhány perc alatt létrehozhat egy [ingyenes](https://azure.microsoft.com/free/) fiókot.</span><span class="sxs-lookup"><span data-stu-id="90182-109">If you don't have an Azure subscription, create a [free](https://azure.microsoft.com/free/) account before you begin.</span></span>

## <a name="log-in-toohello-azure-portal"></a><span data-ttu-id="90182-110">Jelentkezzen be toohello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="90182-110">Log in toohello Azure portal</span></span>

<span data-ttu-id="90182-111">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="90182-111">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>

## <a name="create-a-sql-database"></a><span data-ttu-id="90182-112">SQL-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="90182-112">Create a SQL database</span></span>

<span data-ttu-id="90182-113">Az Azure SQL-adatbázis [számítási és tárolási erőforrások](sql-database-service-tiers.md) egy meghatározott készletével együtt jön létre.</span><span class="sxs-lookup"><span data-stu-id="90182-113">An Azure SQL database is created with a defined set of [compute and storage resources](sql-database-service-tiers.md).</span></span> <span data-ttu-id="90182-114">hello adatbázist a rendszer létrehoz egy [Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md) és az egy [Azure SQL Database logikai kiszolgáló](sql-database-features.md).</span><span class="sxs-lookup"><span data-stu-id="90182-114">hello database is created within an [Azure resource group](../azure-resource-manager/resource-group-overview.md) and in an [Azure SQL Database logical server](sql-database-features.md).</span></span> 

<span data-ttu-id="90182-115">Kövesse ezeket a lépéseket toocreate hello Adventure Works LT mintaadatokat tartalmazó SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="90182-115">Follow these steps toocreate a SQL database containing hello Adventure Works LT sample data.</span></span> 

1. <span data-ttu-id="90182-116">Kattintson a hello **új** hello bal felső sarkában hello Azure-portálon található gombra.</span><span class="sxs-lookup"><span data-stu-id="90182-116">Click hello **New** button found on hello upper left-hand corner of hello Azure portal.</span></span>

2. <span data-ttu-id="90182-117">Válassza ki **adatbázisok** a hello **új** lapon, és válassza ki **SQL-adatbázis** a hello **adatbázisok** lap.</span><span class="sxs-lookup"><span data-stu-id="90182-117">Select **Databases** from hello **New** page, and select **SQL Database** from hello **Databases** page.</span></span>

   ![adatbázis létrehozása-1](./media/sql-database-get-started-portal/create-database-1.png)

3. <span data-ttu-id="90182-119">Hello SQL-adatbázis űrlap kitöltése a következő információ, hello kép megelőző hello szerint:</span><span class="sxs-lookup"><span data-stu-id="90182-119">Fill out hello SQL Database form with hello following information, as shown on hello preceding image:</span></span>   

   | <span data-ttu-id="90182-120">Beállítás</span><span class="sxs-lookup"><span data-stu-id="90182-120">Setting</span></span>       | <span data-ttu-id="90182-121">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="90182-121">Suggested value</span></span> | <span data-ttu-id="90182-122">Leírás</span><span class="sxs-lookup"><span data-stu-id="90182-122">Description</span></span> | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | <span data-ttu-id="90182-123">**Adatbázis neve**</span><span class="sxs-lookup"><span data-stu-id="90182-123">**Database name**</span></span> | <span data-ttu-id="90182-124">mySampleDatabase</span><span class="sxs-lookup"><span data-stu-id="90182-124">mySampleDatabase</span></span> | <span data-ttu-id="90182-125">Az érvényes adatbázisnevekkel kapcsolatban lásd az [adatbázis-azonosítókat](https://docs.microsoft.com/en-us/sql/relational-databases/databases/database-identifiers) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="90182-125">For valid database names, see [Database Identifiers](https://docs.microsoft.com/en-us/sql/relational-databases/databases/database-identifiers).</span></span> | 
   | <span data-ttu-id="90182-126">**Előfizetés**</span><span class="sxs-lookup"><span data-stu-id="90182-126">**Subscription**</span></span> | <span data-ttu-id="90182-127">Az Ön előfizetése</span><span class="sxs-lookup"><span data-stu-id="90182-127">Your subscription</span></span>  | <span data-ttu-id="90182-128">Az előfizetései részleteivel kapcsolatban lásd az [előfizetéseket](https://account.windowsazure.com/Subscriptions) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="90182-128">For details about your subscriptions, see [Subscriptions](https://account.windowsazure.com/Subscriptions).</span></span> |
   | <span data-ttu-id="90182-129">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="90182-129">**Resource group**</span></span>  | <span data-ttu-id="90182-130">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="90182-130">myResourceGroup</span></span> | <span data-ttu-id="90182-131">Az érvényes erőforráscsoport-nevekkel kapcsolatban lásd az [elnevezési szabályokat és korlátozásokat](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="90182-131">For valid resource group names, see [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span></span> |
   | <span data-ttu-id="90182-132">**Forrás forrása**</span><span class="sxs-lookup"><span data-stu-id="90182-132">**Source source**</span></span> | <span data-ttu-id="90182-133">Minta (AdventureWorksLT)</span><span class="sxs-lookup"><span data-stu-id="90182-133">Sample (AdventureWorksLT)</span></span> | <span data-ttu-id="90182-134">Hello AdventureWorksLT séma- és adatok betöltődnek az új adatbázis</span><span class="sxs-lookup"><span data-stu-id="90182-134">Loads hello AdventureWorksLT schema and data into your new database</span></span> |

   > [!IMPORTANT]
   > <span data-ttu-id="90182-135">Hello mintaadatbázis az űrlapon kell kijelölni, mert a gyors üzembe helyezési hello maradéka használatban van.</span><span class="sxs-lookup"><span data-stu-id="90182-135">You must select hello sample database on this form because it is used in hello remainder of this quick start.</span></span>
   > 

4. <span data-ttu-id="90182-136">A **Server**, kattintson a **kötelező beállítások konfigurálása** és kitöltése hello az SQL server (a logikai kiszolgáló) képernyőn hello során a következő információkat, mint a kép a következő hello:</span><span class="sxs-lookup"><span data-stu-id="90182-136">Under **Server**, click **Configure required settings** and fill out hello SQL server (logical server) form with hello following information, as shown on hello following image:</span></span>   

   | <span data-ttu-id="90182-137">Beállítás</span><span class="sxs-lookup"><span data-stu-id="90182-137">Setting</span></span>       | <span data-ttu-id="90182-138">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="90182-138">Suggested value</span></span> | <span data-ttu-id="90182-139">Leírás</span><span class="sxs-lookup"><span data-stu-id="90182-139">Description</span></span> | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | <span data-ttu-id="90182-140">**Kiszolgálónév**</span><span class="sxs-lookup"><span data-stu-id="90182-140">**Server name**</span></span> | <span data-ttu-id="90182-141">Bármely globálisan egyedi név</span><span class="sxs-lookup"><span data-stu-id="90182-141">Any globally unique name</span></span> | <span data-ttu-id="90182-142">Az érvényes kiszolgálónevekkel kapcsolatban lásd az [elnevezési szabályokat és korlátozásokat](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="90182-142">For valid server names, see [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span></span> | 
   | <span data-ttu-id="90182-143">**Kiszolgálói rendszergazdai bejelentkezés**</span><span class="sxs-lookup"><span data-stu-id="90182-143">**Server admin login**</span></span> | <span data-ttu-id="90182-144">Bármely érvényes név</span><span class="sxs-lookup"><span data-stu-id="90182-144">Any valid name</span></span> | <span data-ttu-id="90182-145">Az érvényes bejelentkezési nevekkel kapcsolatban lásd az [adatbázis-azonosítókat](https://docs.microsoft.com/en-us/sql/relational-databases/databases/database-identifiers) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="90182-145">For valid login names, see [Database Identifiers](https://docs.microsoft.com/en-us/sql/relational-databases/databases/database-identifiers).</span></span> |
   | <span data-ttu-id="90182-146">**Jelszó**</span><span class="sxs-lookup"><span data-stu-id="90182-146">**Password**</span></span> | <span data-ttu-id="90182-147">Bármely érvényes jelszó</span><span class="sxs-lookup"><span data-stu-id="90182-147">Any valid password</span></span> | <span data-ttu-id="90182-148">A jelszó legalább 8 karakterből kell állnia, és a következő kategóriák hello hármat tartalmaznia kell: nagybetűk, kisbetűk, számok, és nem alfanumerikus karakterek és.</span><span class="sxs-lookup"><span data-stu-id="90182-148">Your password must have at least 8 characters and must contain characters from three of hello following categories: upper case characters, lower case characters, numbers, and and non-alphanumeric characters.</span></span> |
   | <span data-ttu-id="90182-149">**Előfizetés**</span><span class="sxs-lookup"><span data-stu-id="90182-149">**Subscription**</span></span> | <span data-ttu-id="90182-150">Az Ön előfizetése</span><span class="sxs-lookup"><span data-stu-id="90182-150">Your subscription</span></span> | <span data-ttu-id="90182-151">Az előfizetései részleteivel kapcsolatban lásd az [előfizetéseket](https://account.windowsazure.com/Subscriptions) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="90182-151">For details about your subscriptions, see [Subscriptions](https://account.windowsazure.com/Subscriptions).</span></span> |
   | <span data-ttu-id="90182-152">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="90182-152">**Resource group**</span></span> | <span data-ttu-id="90182-153">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="90182-153">myResourceGroup</span></span> | <span data-ttu-id="90182-154">Az érvényes erőforráscsoport-nevekkel kapcsolatban lásd az [elnevezési szabályokat és korlátozásokat](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="90182-154">For valid resource group names, see [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span></span> |
   | <span data-ttu-id="90182-155">**Hely**</span><span class="sxs-lookup"><span data-stu-id="90182-155">**Location**</span></span> | <span data-ttu-id="90182-156">Bármely érvényes hely</span><span class="sxs-lookup"><span data-stu-id="90182-156">Any valid location</span></span> | <span data-ttu-id="90182-157">A régiókkal kapcsolatos információkért lásd [az Azure régióit](https://azure.microsoft.com/regions/) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="90182-157">For information about regions, see [Azure Regions](https://azure.microsoft.com/regions/).</span></span> |

   > [!IMPORTANT]
   > <span data-ttu-id="90182-158">hello kiszolgáló-rendszergazdai bejelentkezés és a jelszót, amely az itt megadott a szükséges toolog toohello Server és az adatbázisok a gyors üzembe helyezési későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="90182-158">hello server admin login and password that you specify here are required toolog in toohello server and its databases later in this quick start.</span></span> <span data-ttu-id="90182-159">Jegyezze meg vagy jegyezze fel ezt az információt későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="90182-159">Remember or record this information for later use.</span></span> 
   >  

   ![adatbázis-kiszolgáló létrehozása](./media/sql-database-get-started-portal/create-database-server.png)

5. <span data-ttu-id="90182-161">Hello űrlap befejeződésekor kattintson **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="90182-161">When you have completed hello form, click **Select**.</span></span>

6. <span data-ttu-id="90182-162">Kattintson a **tarifacsomag** toospecify hello és teljesítményszintet szolgáltatásszint az új adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="90182-162">Click **Pricing tier** toospecify hello service tier and performance level for your new database.</span></span> <span data-ttu-id="90182-163">Használjon hello csúszkát tooselect **20 Dtu** és **250** GB tárhelyet.</span><span class="sxs-lookup"><span data-stu-id="90182-163">Use hello slider tooselect **20 DTUs** and **250** GB of storage.</span></span> <span data-ttu-id="90182-164">További információ a DTU-król: [Mi a DTU?](sql-database-what-is-a-dtu.md)</span><span class="sxs-lookup"><span data-stu-id="90182-164">For more information on DTUs, see [What is a DTU?](sql-database-what-is-a-dtu.md).</span></span>

   ![adatbázis létrehozása-s1](./media/sql-database-get-started-portal/create-database-s1.png)

7. <span data-ttu-id="90182-166">Követően kijelölt hello dtu-k, kattintson a **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="90182-166">After selected hello amount of DTUs, click **Apply**.</span></span>  

8. <span data-ttu-id="90182-167">Most, hogy az SQL-adatbázis űrlap hello befejeződött, kattintson a **létrehozása** tooprovision hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="90182-167">Now that you have completed hello SQL Database form, click **Create** tooprovision hello database.</span></span> <span data-ttu-id="90182-168">Az üzembe helyezés eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="90182-168">Provisioning takes a few minutes.</span></span> 

9. <span data-ttu-id="90182-169">Hello eszköztáron kattintson **értesítések** toomonitor hello telepítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="90182-169">On hello toolbar, click **Notifications** toomonitor hello deployment process.</span></span>

   ![értesítés](./media/sql-database-get-started-portal/notification.png)

## <a name="create-a-server-level-firewall-rule"></a><span data-ttu-id="90182-171">Kiszolgálószintű tűzfalszabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="90182-171">Create a server-level firewall rule</span></span>

<span data-ttu-id="90182-172">SQL Database szolgáltatás hello tűzfal hello kiszolgálói szinten-, amely megakadályozza, hogy a külső alkalmazások és eszközök toohello kiszolgáló vagy hello kiszolgálón lévő összes adatbázis csatlakozzon, kivéve, ha egy tűzfalszabály tooopen hello tűzfal adott IP-címekhez hoz létre.</span><span class="sxs-lookup"><span data-stu-id="90182-172">hello SQL Database service creates a firewall at hello server-level that prevents external applications and tools from connecting toohello server or any databases on hello server unless a firewall rule is created tooopen hello firewall for specific IP addresses.</span></span> <span data-ttu-id="90182-173">Kövesse az alábbi lépéseket toocreate egy [SQL-adatbázis kiszolgálószintű tűzfalszabály](sql-database-firewall-configure.md) az ügyfél IP-cím, és engedélyezze a külső kapcsolatot csak az IP-cím hello SQL-adatbázis tűzfalon keresztül.</span><span class="sxs-lookup"><span data-stu-id="90182-173">Follow these steps toocreate a [SQL Database server-level firewall rule](sql-database-firewall-configure.md) for your client's IP address and enable external connectivity through hello SQL Database firewall for your IP address only.</span></span> 

> [!NOTE]
> <span data-ttu-id="90182-174">Az SQL Database az 1433-as porton kommunikál.</span><span class="sxs-lookup"><span data-stu-id="90182-174">SQL Database communicates over port 1433.</span></span> <span data-ttu-id="90182-175">Ha a vállalati hálózatból származó tooconnect, a hálózati tűzfal előfordulhat, hogy nem engedélyezett a 1433-as port kimenő forgalmát.</span><span class="sxs-lookup"><span data-stu-id="90182-175">If you are trying tooconnect from within a corporate network, outbound traffic over port 1433 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="90182-176">Ha igen, kivéve, ha az IT-részleg megnyitja az 1433-as port tooyour Azure SQL adatbázis-kiszolgáló nem lehet csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="90182-176">If so, you cannot connect tooyour Azure SQL Database server unless your IT department opens port 1433.</span></span>
>

1. <span data-ttu-id="90182-177">Hello központi telepítés befejezése után kattintson **SQL-adatbázisok** hello bal oldali menüből, és kattintson a **mySampleDatabase** a hello **SQL-adatbázisok** lap.</span><span class="sxs-lookup"><span data-stu-id="90182-177">After hello deployment completes, click **SQL databases** from hello left-hand menu and then click **mySampleDatabase** on hello **SQL databases** page.</span></span> <span data-ttu-id="90182-178">hello áttekintő lapjára jut a adatbázis megnyílik, teljes mértékben hello megjelenítő minősített kiszolgáló neve (például **mynewserver20170313.database.windows.net**) és további konfigurációs lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="90182-178">hello overview page for your database opens, showing you hello fully qualified server name (such as **mynewserver20170313.database.windows.net**) and provides options for further configuration.</span></span> <span data-ttu-id="90182-179">Későbbi felhasználás céljára másolja ki ezt a teljes kiszolgálónevet.</span><span class="sxs-lookup"><span data-stu-id="90182-179">Copy this fully qualified server name for use later.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="90182-180">A teljesen minősített neve tooconnect tooyour kiszolgálók és a későbbi gyors üzembe helyezések adatbázisainak van szükség.</span><span class="sxs-lookup"><span data-stu-id="90182-180">You need this fully qualified server name tooconnect tooyour server and its databases in subsequent quick starts.</span></span>
   > 

   ![kiszolgáló neve](./media/sql-database-connect-query-dotnet/server-name.png) 

2. <span data-ttu-id="90182-182">Kattintson a **kiszolgáló tűzfalának beállítása** hello eszköztár hello előző ábrának megfelelően.</span><span class="sxs-lookup"><span data-stu-id="90182-182">Click **Set server firewall** on hello toolbar as shown in hello previous image.</span></span> <span data-ttu-id="90182-183">Hello **tűzfalbeállítások** hello SQL Database-kiszolgálóhoz tartozó lapon nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="90182-183">hello **Firewall settings** page for hello SQL Database server opens.</span></span> 

   ![kiszolgálói tűzfalszabály](./media/sql-database-get-started-portal/server-firewall-rule.png) 

3. <span data-ttu-id="90182-185">Kattintson a **ügyfél IP-cím hozzáadása** hello eszköztár tooadd meg az aktuális IP-cím tooa Új tűzfalszabály.</span><span class="sxs-lookup"><span data-stu-id="90182-185">Click **Add client IP** on hello toolbar tooadd your current IP address tooa new firewall rule.</span></span> <span data-ttu-id="90182-186">A tűzfalszabály az 1433-as portot egy egyedi IP-cím vagy egy IP-címtartomány számára nyithatja meg.</span><span class="sxs-lookup"><span data-stu-id="90182-186">A firewall rule can open port 1433 for a single IP address or a range of IP addresses.</span></span>

4. <span data-ttu-id="90182-187">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="90182-187">Click **Save**.</span></span> <span data-ttu-id="90182-188">Az aktuális IP-címek hello logikai kiszolgálón 1433-as port megnyitása egy kiszolgálószintű tűzfalszabályt jön létre.</span><span class="sxs-lookup"><span data-stu-id="90182-188">A server-level firewall rule is created for your current IP address opening port 1433 on hello logical server.</span></span>

   ![kiszolgálótűzfal-szabály beállítása](./media/sql-database-get-started-portal/server-firewall-rule-set.png) 

4. <span data-ttu-id="90182-190">Kattintson a **OK** , majd zárja be a hello **tűzfalbeállítások** lap.</span><span class="sxs-lookup"><span data-stu-id="90182-190">Click **OK** and then close hello **Firewall settings** page.</span></span>

<span data-ttu-id="90182-191">Csatlakoztathatja toohello SQL adatbázis-kiszolgáló és az adatbázisok, SQL Server Management Studio vagy az Ön által választott, a korábban létrehozott hello kiszolgáló rendszergazdai fiókjának használatával IP-címről egy másik eszköz használatával.</span><span class="sxs-lookup"><span data-stu-id="90182-191">You can now connect toohello SQL Database server and its databases using SQL Server Management Studio or another tool of your choice from this IP address using hello server admin account created previously.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="90182-192">Az összes Azure-szolgáltatások alapértelmezés szerint engedélyezve van a hozzáférés hello SQL-adatbázis tűzfalon keresztül.</span><span class="sxs-lookup"><span data-stu-id="90182-192">By default, access through hello SQL Database firewall is enabled for all Azure services.</span></span> <span data-ttu-id="90182-193">Kattintson a **OFF** meg a lap toodisable az összes Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="90182-193">Click **OFF** on this page toodisable for all Azure services.</span></span>
>

## <a name="query-hello-sql-database"></a><span data-ttu-id="90182-194">Hello SQL-adatbázis lekérdezése</span><span class="sxs-lookup"><span data-stu-id="90182-194">Query hello SQL database</span></span>

<span data-ttu-id="90182-195">Most, hogy létrehozta a mintaadatbázis az Azure-ban, most eszközzel hello beépített lekérdezést hello toohello adatbázis és a lekérdezés hello adatokat is elérheti az Azure portál tooconfirm belül.</span><span class="sxs-lookup"><span data-stu-id="90182-195">Now that you have created a sample database in Azure, let’s use hello built-in query tool within hello Azure portal tooconfirm that you can connect toohello database and query hello data.</span></span> 

1. <span data-ttu-id="90182-196">Az adatbázis hello SQL-adatbázis lapján kattintson **eszközök** hello eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="90182-196">On hello SQL Database page for your database, click **Tools** on hello toolbar.</span></span> <span data-ttu-id="90182-197">Hello **eszközök** lap megnyitásakor.</span><span class="sxs-lookup"><span data-stu-id="90182-197">hello **Tools** page opens.</span></span>

   ![eszközök menü](./media/sql-database-get-started-portal/tools-menu.png) 

2. <span data-ttu-id="90182-199">Kattintson a **Lekérdezésszerkesztő (előzetes verzió)**, hello kattintson **feltételek előzetes** jelölőnégyzetet, majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="90182-199">Click **Query editor (preview)**, click hello **Preview terms** checkbox, and then click **OK**.</span></span> <span data-ttu-id="90182-200">hello lekérdezés-szerkesztő lap nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="90182-200">hello Query editor page opens.</span></span>

3. <span data-ttu-id="90182-201">Kattintson a **bejelentkezési** majd, amikor a rendszer kéri, válassza ki **SQL server-hitelesítés** hello kiszolgáló-rendszergazdai bejelentkezés és a korábban létrehozott jelszót adja meg.</span><span class="sxs-lookup"><span data-stu-id="90182-201">Click **Login** and then, when prompted, select **SQL server authentication** and then provide hello server admin login and password that you created earlier.</span></span>

   ![bejelentkezés](./media/sql-database-get-started-portal/login.png) 

4. <span data-ttu-id="90182-203">Kattintson a **OK** a toolog.</span><span class="sxs-lookup"><span data-stu-id="90182-203">Click **OK** toolog in.</span></span>

5. <span data-ttu-id="90182-204">Ön hitelesítése után vannak, írja be a következő lekérdezés hello lekérdezés-szerkesztő ablaktáblán hello.</span><span class="sxs-lookup"><span data-stu-id="90182-204">After you are authenticated, type hello following query in hello query editor pane.</span></span>

   ```sql
   SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

6. <span data-ttu-id="90182-205">Kattintson a **futtatása** majd tekintse át a lekérdezés eredményének hello hello **eredmények** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="90182-205">Click **Run** and then review hello query results in hello **Results** pane.</span></span>

   ![lekérdezésszerkesztő: eredmények](./media/sql-database-get-started-portal/query-editor-results.png)

7. <span data-ttu-id="90182-207">Bezárás hello **Lekérdezésszerkesztő** lap és hello **eszközök** lap.</span><span class="sxs-lookup"><span data-stu-id="90182-207">Close hello **Query editor** page and hello **Tools** page.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="90182-208">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="90182-208">Clean up resources</span></span>

<span data-ttu-id="90182-209">Ha egy másik gyorsindítási/oktatóanyag nem kell ezeket az erőforrásokat (lásd: [további lépések](#next-steps)), törölheti azokat a hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="90182-209">If you don't need these resources for another quickstart/tutorial (see [Next steps](#next-steps)), you can delete them by doing hello following:</span></span>


1. <span data-ttu-id="90182-210">A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** majd **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="90182-210">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click **myResourceGroup**.</span></span> 
2. <span data-ttu-id="90182-211">Az erőforrás csoport lapján kattintson a **törlése**, típus **myResourceGroup** hello szövegmezőbe, és kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="90182-211">On your resource group page, click **Delete**, type **myResourceGroup** in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="90182-212">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="90182-212">Next steps</span></span>

<span data-ttu-id="90182-213">Most, hogy rendelkezik egy adatbázissal, csatlakoztathatja a kedvenc eszközeit, és lekérdezéseket hajthat végre velük.</span><span class="sxs-lookup"><span data-stu-id="90182-213">Now that you have a database, you can connect and query using your favorite tools.</span></span> <span data-ttu-id="90182-214">További információkért válassza ki az eszközt az alábbiak közül:</span><span class="sxs-lookup"><span data-stu-id="90182-214">Learn more by choosing your tool below:</span></span>

- [<span data-ttu-id="90182-215">SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="90182-215">SQL Server Management Studio</span></span>](sql-database-connect-query-ssms.md)
- [<span data-ttu-id="90182-216">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="90182-216">Visual Studio Code</span></span>](sql-database-connect-query-vscode.md)
- [<span data-ttu-id="90182-217">.NET</span><span class="sxs-lookup"><span data-stu-id="90182-217">.NET</span></span>](sql-database-connect-query-dotnet.md)
- [<span data-ttu-id="90182-218">PHP</span><span class="sxs-lookup"><span data-stu-id="90182-218">PHP</span></span>](sql-database-connect-query-php.md)
- [<span data-ttu-id="90182-219">Node.js</span><span class="sxs-lookup"><span data-stu-id="90182-219">Node.js</span></span>](sql-database-connect-query-nodejs.md)
- [<span data-ttu-id="90182-220">Java</span><span class="sxs-lookup"><span data-stu-id="90182-220">Java</span></span>](sql-database-connect-query-java.md)
- [<span data-ttu-id="90182-221">Python</span><span class="sxs-lookup"><span data-stu-id="90182-221">Python</span></span>](sql-database-connect-query-python.md)
- [<span data-ttu-id="90182-222">Ruby</span><span class="sxs-lookup"><span data-stu-id="90182-222">Ruby</span></span>](sql-database-connect-query-ruby.md)
