---
title: "Az első Azure-adatbázis kialakítása a PostgreSQL Azure-portál használatával |} Microsoft Docs"
description: "Ez az oktatóanyag bemutatja hogyan tooDesign az első Azure adatbázis a PostgreSQL hello Azure-portál használatával."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: tutorial, mvc
ms.topic: tutorial
ms.date: 05/10/2017
ms.openlocfilehash: fde7e9d1ae2bad4291d18bebd3356f4f8a2ac86a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="design-your-first-azure-database-for-postgresql-using-hello-azure-portal"></a><span data-ttu-id="ca9f6-103">Az első Azure-adatbázis kialakítása a PostgreSQL hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="ca9f6-103">Design your first Azure Database for PostgreSQL using hello Azure portal</span></span>

<span data-ttu-id="ca9f6-104">Azure PostgreSQL-adatbázishoz egy felügyelt szolgáltatás, amely lehetővé teszi toorun, kezeléséhez, és magas rendelkezésre állású PostgreSQL-adatbázisok hello felhőben méretezése.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-104">Azure Database for PostgreSQL is a managed service that enables you toorun, manage, and scale highly available PostgreSQL databases in hello cloud.</span></span> <span data-ttu-id="ca9f6-105">Hello Azure-portált használja, egyszerűen kezelheti a kiszolgálót, és egy adatbázis tervezése.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-105">Using hello Azure portal, you can easily manage your server and design a database.</span></span>

<span data-ttu-id="ca9f6-106">Ebben az oktatóanyagban használhatja az Azure portál toolearn hello hogyan számára:</span><span class="sxs-lookup"><span data-stu-id="ca9f6-106">In this tutorial, you use hello Azure portal toolearn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="ca9f6-107">Azure-adatbázis létrehozása PostgreSQL-hez</span><span class="sxs-lookup"><span data-stu-id="ca9f6-107">Create an Azure Database for PostgreSQL</span></span>
> * <span data-ttu-id="ca9f6-108">Hello kiszolgáló tűzfal konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ca9f6-108">Configure hello server firewall</span></span>
> * <span data-ttu-id="ca9f6-109">Használjon [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) segédprogram toocreate adatbázis</span><span class="sxs-lookup"><span data-stu-id="ca9f6-109">Use [**psql**](https://www.postgresql.org/docs/9.6/static/app-psql.html) utility toocreate a database</span></span>
> * <span data-ttu-id="ca9f6-110">Mintaadatok betöltése</span><span class="sxs-lookup"><span data-stu-id="ca9f6-110">Load sample data</span></span>
> * <span data-ttu-id="ca9f6-111">Adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="ca9f6-111">Query data</span></span>
> * <span data-ttu-id="ca9f6-112">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="ca9f6-112">Update data</span></span>
> * <span data-ttu-id="ca9f6-113">Adatok visszaállítása</span><span class="sxs-lookup"><span data-stu-id="ca9f6-113">Restore data</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ca9f6-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ca9f6-114">Prerequisites</span></span>
<span data-ttu-id="ca9f6-115">Ha nem rendelkezik Azure-előfizetéssel, első lépésként mindössze néhány perc alatt létrehozhat egy [ingyenes](https://azure.microsoft.com/free/) fiókot.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-115">If you don't have an Azure subscription, create a [free](https://azure.microsoft.com/free/) account before you begin.</span></span>

## <a name="log-in-toohello-azure-portal"></a><span data-ttu-id="ca9f6-116">Jelentkezzen be toohello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="ca9f6-116">Log in toohello Azure portal</span></span>
<span data-ttu-id="ca9f6-117">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ca9f6-117">Log in toohello [Azure portal](https://portal.azure.com).</span></span>

## <a name="create-an-azure-database-for-postgresql"></a><span data-ttu-id="ca9f6-118">Azure-adatbázis létrehozása PostgreSQL-hez</span><span class="sxs-lookup"><span data-stu-id="ca9f6-118">Create an Azure Database for PostgreSQL</span></span>

<span data-ttu-id="ca9f6-119">Az Azure-adatbázis PostgreSQL-kiszolgálóhoz [számítási és tárolási erőforrások](./concepts-compute-unit-and-storage.md) egy meghatározott készletével együtt jön létre.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-119">An Azure Database for PostgreSQL server is created with a defined set of [compute and storage resources](./concepts-compute-unit-and-storage.md).</span></span> <span data-ttu-id="ca9f6-120">hello server belül létrejön egy [Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ca9f6-120">hello server is created within an [Azure resource group](../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="ca9f6-121">Kövesse ezeket a lépéseket toocreate egy PostgreSQL-kiszolgálóhoz tartozó Azure-adatbázis:</span><span class="sxs-lookup"><span data-stu-id="ca9f6-121">Follow these steps toocreate an Azure Database for PostgreSQL server:</span></span>
1.  <span data-ttu-id="ca9f6-122">Kattintson a hello **+ új** hello bal felső sarkában hello Azure-portálon található gombra.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-122">Click hello **+ New**  button found on hello upper left-hand corner of hello Azure portal.</span></span>
2.  <span data-ttu-id="ca9f6-123">Válassza ki **adatbázisok** a hello **új** lapon, és válassza ki **PostgreSQL az Azure-adatbázis** a hello **adatbázisok** lap.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-123">Select **Databases** from hello **New** page, and select **Azure Database for PostgreSQL** from hello **Databases** page.</span></span>
 <span data-ttu-id="ca9f6-124">![Azure-adatbázishoz tartozó PostgreSQL - hello adatbázis létrehozása](./media/tutorial-design-database-using-azure-portal/1-create-database.png)</span><span class="sxs-lookup"><span data-stu-id="ca9f6-124">![Azure Database for PostgreSQL - Create hello database](./media/tutorial-design-database-using-azure-portal/1-create-database.png)</span></span>

3.  <span data-ttu-id="ca9f6-125">Hello új kiszolgáló részletei űrlap kitöltése a következő információ, hello kép megelőző hello szerint:</span><span class="sxs-lookup"><span data-stu-id="ca9f6-125">Fill out hello new server details form with hello following information, as shown on hello preceding image:</span></span>
    - <span data-ttu-id="ca9f6-126">Kiszolgáló neve: **mypgserver-20170401** (a kiszolgáló nevét leképezhető tooDNS nevét és így szükséges toobe globálisan egyedi)</span><span class="sxs-lookup"><span data-stu-id="ca9f6-126">Server name: **mypgserver-20170401** (name of a server maps tooDNS name and is thus required toobe globally unique)</span></span> 
    - <span data-ttu-id="ca9f6-127">Előfizetés: Ha több előfizetéssel rendelkezik, válassza ki a megfelelő előfizetés hello hello erőforrás létezik-e vagy a lesz számlázva.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-127">Subscription: If you have multiple subscriptions, choose hello appropriate subscription in which hello resource exists or is billed for.</span></span>
    - <span data-ttu-id="ca9f6-128">Erőforráscsoport: **myresourcegroup**</span><span class="sxs-lookup"><span data-stu-id="ca9f6-128">Resource group: **myresourcegroup**</span></span>
    - <span data-ttu-id="ca9f6-129">Az Ön által választott kiszolgálói rendszergazdai bejelentkezési név és jelszó</span><span class="sxs-lookup"><span data-stu-id="ca9f6-129">Server admin login and password of your choice</span></span>
    - <span data-ttu-id="ca9f6-130">Hely</span><span class="sxs-lookup"><span data-stu-id="ca9f6-130">Location</span></span>
    - <span data-ttu-id="ca9f6-131">PostgreSQL-verzió</span><span class="sxs-lookup"><span data-stu-id="ca9f6-131">PostgreSQL Version</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="ca9f6-132">hello kiszolgáló-rendszergazdai bejelentkezés és a jelszót, amely az itt megadott a szükséges toolog toohello Server és az adatbázisok a gyors üzembe helyezési későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-132">hello server admin login and password that you specify here are required toolog in toohello server and its databases later in this quick start.</span></span> <span data-ttu-id="ca9f6-133">Jegyezze meg vagy jegyezze fel ezt az információt későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-133">Remember or record this information for later use.</span></span>

4.  <span data-ttu-id="ca9f6-134">Kattintson a **tarifacsomag** toospecify hello és teljesítményszintet szolgáltatásszint az új adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-134">Click **Pricing tier** toospecify hello service tier and performance level for your new database.</span></span> <span data-ttu-id="ca9f6-135">Ehhez a rövid útmutatóhoz válassza az **Alapszintű** szolgáltatásszintet, **50 Számítási egységet**, és a szolgáltatási keretbe foglalt **50 GB** tárterületet.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-135">For this quick start, select **Basic** Tier, **50 Compute Units** and **50 GB** of included storage.</span></span>
 <span data-ttu-id="ca9f6-136">![Azure-adatbázis PostgreSQL - kivételezési hello szolgáltatási rétegben](./media/tutorial-design-database-using-azure-portal/2-service-tier.png)</span><span class="sxs-lookup"><span data-stu-id="ca9f6-136">![Azure Database for PostgreSQL - pick hello service tier](./media/tutorial-design-database-using-azure-portal/2-service-tier.png)</span></span>
5.  <span data-ttu-id="ca9f6-137">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-137">Click **Ok**.</span></span>
6.  <span data-ttu-id="ca9f6-138">Kattintson a **létrehozása** tooprovision hello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-138">Click **Create** tooprovision hello server.</span></span> <span data-ttu-id="ca9f6-139">Az üzembe helyezés eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-139">Provisioning takes a few minutes.</span></span>

  > [!TIP]
  > <span data-ttu-id="ca9f6-140">Ellenőrizze a hello **PIN-kód toodashboard** beállítás tooallow könnyű nyomon a központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-140">Check hello **Pin toodashboard** option tooallow easy tracking of your deployments.</span></span>

7.  <span data-ttu-id="ca9f6-141">Hello eszköztáron kattintson **értesítések** toomonitor hello telepítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-141">On hello toolbar, click **Notifications** toomonitor hello deployment process.</span></span>
 <span data-ttu-id="ca9f6-142">![Azure-adatbázis PostgreSQL-hez - Értesítések megtekintése](./media/tutorial-design-database-using-azure-portal/3-notifications.png)</span><span class="sxs-lookup"><span data-stu-id="ca9f6-142">![Azure Database for PostgreSQL - See notifications](./media/tutorial-design-database-using-azure-portal/3-notifications.png)</span></span>
   
  <span data-ttu-id="ca9f6-143">Alapértelmezés szerint a **postgres** adatbázis a kiszolgáló alatt jön létre.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-143">By default, **postgres** database gets created under your server.</span></span> <span data-ttu-id="ca9f6-144">Hello [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) egy alapértelmezett adatbázis való használatra szolgál, a felhasználók, segédprogramok és harmadik féltől származó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-144">hello [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) database is a default database meant for use by users, utilities, and third-party applications.</span></span> 

## <a name="configure-a-server-level-firewall-rule"></a><span data-ttu-id="ca9f6-145">Kiszolgálószintű tűzfalszabály konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ca9f6-145">Configure a server-level firewall rule</span></span>

<span data-ttu-id="ca9f6-146">hello Azure adatbázis PostgreSQL szolgáltatás tűzfal hello kiszolgáló-szintjén hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-146">hello Azure Database for PostgreSQL service creates a firewall at hello server-level.</span></span> <span data-ttu-id="ca9f6-147">Ez a tűzfal megakadályozza, hogy a külső alkalmazások és eszközök toohello kiszolgáló és a hello kiszolgálón lévő összes adatbázis csatlakozzon, kivéve, ha egy tűzfalszabály tooopen hello tűzfal adott IP-címekhez.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-147">This firewall prevents external applications and tools from connecting toohello server and any databases on hello server unless a firewall rule is created tooopen hello firewall for specific IP addresses.</span></span> 

1.  <span data-ttu-id="ca9f6-148">Hello központi telepítés befejezése után kattintson **összes erőforrás** hello bal oldali menüből és hello nevet írja be a **mypgserver-20170401** toosearch az újonnan létrehozott kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-148">After hello deployment completes, click **All Resources** from hello left-hand menu and type in hello name **mypgserver-20170401** toosearch for your newly created server.</span></span> <span data-ttu-id="ca9f6-149">Kattintson a felsorolt hello keresési eredmény hello kiszolgálónevet.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-149">Click hello server name listed in hello search result.</span></span> <span data-ttu-id="ca9f6-150">Hello **áttekintése** lapon, a kiszolgáló megnyílik, és további konfigurációs lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-150">hello **Overview** page for your server opens and provides options for further configuration.</span></span>
 
 ![<span data-ttu-id="ca9f6-151">Azure-adatbázis PostgreSQL-hez - Kiszolgáló keresés</span><span class="sxs-lookup"><span data-stu-id="ca9f6-151">Azure Database for PostgreSQL - Search for server</span></span> ](./media/tutorial-design-database-using-azure-portal/4-locate.png)

2.  <span data-ttu-id="ca9f6-152">Hello server panelen válassza ki a **kapcsolatbiztonsági**.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-152">In hello server blade, select **Connection Security**.</span></span> 
3.  <span data-ttu-id="ca9f6-153">Hello szövegmező alatt kattintson **szabály neve,** , és adja hozzá egy új tűzfal szabály toowhitelist hello IP-címtartománya kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-153">Click in hello text box under **Rule Name,** and add a new firewall rule toowhitelist hello IP range for connectivity.</span></span> <span data-ttu-id="ca9f6-154">Ebben az oktatóanyagban most engedélyezése minden IP-címet írja be a **szabály neve = AllowAllIps**, **kezdő IP-= 0.0.0.0** és **záró IP-Címnél = 255.255.255.255** , majd **mentése** .</span><span class="sxs-lookup"><span data-stu-id="ca9f6-154">For this tutorial, let's allow all IPs by typing in **Rule Name = AllowAllIps**, **Start IP = 0.0.0.0** and **End IP = 255.255.255.255** and then click **Save**.</span></span> <span data-ttu-id="ca9f6-155">Beállíthat egy tűzfalszabályt, amely hozzá van rendelve egy IP tartomány toobe képes tooconnect a hálózatról.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-155">You can set a firewall rule that covers an IP range toobe able tooconnect from your network.</span></span>
 
 ![Azure-adatbázis PostgreSQL-hez - Tűzfalszabály létrehozása](./media/tutorial-design-database-using-azure-portal/5-firewall-2.png)

4.  <span data-ttu-id="ca9f6-157">Kattintson a **mentése** majd hello **X** tooclose hello **kapcsolatok biztonsági** lap.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-157">Click **Save** and then click hello **X** tooclose hello **Connections Security** page.</span></span>

  > [!NOTE]
  > <span data-ttu-id="ca9f6-158">Azure PostgreSQL-kiszolgáló az 5432-es porton keresztül kommunikál.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-158">Azure PostgreSQL server communicates over port 5432.</span></span> <span data-ttu-id="ca9f6-159">Ha a vállalati hálózatból származó tooconnect, a hálózati tűzfal előfordulhat, hogy nem engedélyezett a port 5432 kimenő forgalmát.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-159">If you are trying tooconnect from within a corporate network, outbound traffic over port 5432 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="ca9f6-160">Ha igen, csak akkor tudja tooconnect tooyour Azure SQL adatbázis-kiszolgáló kivéve, ha az IT-részleg 5432 portot nyit meg.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-160">If so, you will not be able tooconnect tooyour Azure SQL Database server unless your IT department opens port 5432.</span></span>
  >


## <a name="get-hello-connection-information"></a><span data-ttu-id="ca9f6-161">Hello kapcsolatadatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="ca9f6-161">Get hello connection information</span></span>

<span data-ttu-id="ca9f6-162">A PostgreSQL-kiszolgálóhoz tartozó Azure-adatbázis létrehozásakor hello alapértelmezett **postgres** adatbázis is végrehajtásakor létrejön.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-162">When we created our Azure Database for PostgreSQL server, hello default **postgres** database also gets created.</span></span> <span data-ttu-id="ca9f6-163">tooconnect tooyour adatbázis-kiszolgáló tooprovide állomás információkat és a hozzáférési hitelesítő adatok szükségesek.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-163">tooconnect tooyour database server, you need tooprovide host information and access credentials.</span></span>

1. <span data-ttu-id="ca9f6-164">A hello Azure-portálon a bal oldali menüből, kattintson az **összes erőforrás** , és keresse meg az imént létrehozott hello kiszolgáló **mypgserver-20170401**.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-164">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you just created **mypgserver-20170401**.</span></span>

  ![<span data-ttu-id="ca9f6-165">Azure-adatbázis PostgreSQL-hez - Kiszolgáló keresés</span><span class="sxs-lookup"><span data-stu-id="ca9f6-165">Azure Database for PostgreSQL - Search for server</span></span> ](./media/tutorial-design-database-using-azure-portal/4-locate.png)

3. <span data-ttu-id="ca9f6-166">Hello kiszolgáló nevére kattint **mypgserver-20170401**.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-166">Click hello server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="ca9f6-167">Jelölje be hello server **áttekintése** lap.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-167">Select hello server's **Overview** page.</span></span> <span data-ttu-id="ca9f6-168">Jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-168">Make a note of hello **Server name** and **Server admin login name**.</span></span>

 ![Azure-adatbázis PostgreSQL-hez - Kiszolgáló-rendszergazdai bejelentkezés](./media/tutorial-design-database-using-azure-portal/6-server-name.png)


## <a name="connect-toopostgresql-database-using-psql-in-cloud-shell"></a><span data-ttu-id="ca9f6-170">Csatlakozás tooPostgreSQL adatbázis psql felhő rendszerhéj használatával</span><span class="sxs-lookup"><span data-stu-id="ca9f6-170">Connect tooPostgreSQL database using psql in Cloud Shell</span></span>

<span data-ttu-id="ca9f6-171">Most már a hello psql parancssori segédprogram tooconnect toohello Azure adatbázis PostgreSQL-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-171">Let's now use hello psql command-line utility tooconnect toohello Azure Database for PostgreSQL server.</span></span> 
1. <span data-ttu-id="ca9f6-172">Indítsa el a hello Azure Cloud rendszerhéj hello terminál ikonra a felső navigációs panelen hello keresztül.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-172">Launch hello Azure Cloud Shell via hello terminal icon on hello top navigation pane.</span></span>

   ![Azure-adatbázis PostgreSQL-hez - Azure Cloud Shell terminálikon](./media/tutorial-design-database-using-azure-portal/7-cloud-shell.png)

2. <span data-ttu-id="ca9f6-174">hello Azure Cloud rendszerhéj megnyitása a böngészőben, amely lehetővé teszi, tootype bash parancsok.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-174">hello Azure Cloud Shell opens in your browser, enabling you tootype bash commands.</span></span>

   ![Azure-adatbázis PostgreSQL-hez - Azure Shell Bash parancssor](./media/tutorial-design-database-using-azure-portal/8-bash.png)

3. <span data-ttu-id="ca9f6-176">A parancssorból hello felhő rendszerhéj tooyour Azure adatbázis hello psql parancsokkal PostgreSQL-kiszolgáló csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-176">At hello Cloud Shell prompt, connect tooyour Azure Database for PostgreSQL server using hello psql commands.</span></span> <span data-ttu-id="ca9f6-177">hello következő formátuma használt tooconnect tooan Azure adatbázis hello PostgreSQL kiszolgáló [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) segédprogram:</span><span class="sxs-lookup"><span data-stu-id="ca9f6-177">hello following format is used tooconnect tooan Azure Database for PostgreSQL server with hello [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) utility:</span></span>
   ```bash
   psql --host=<myserver> --port=<port> --username=<server admin login> --dbname=<database name>
   ```

   <span data-ttu-id="ca9f6-178">Például a következő parancs hello csatlakozik toohello alapértelmezett adatbázis **postgres** a PostgreSQL kiszolgálón **mypgserver-20170401.postgres.database.azure.com** hozzáférési hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-178">For example, hello following command connects toohello default database called **postgres** on your PostgreSQL server **mypgserver-20170401.postgres.database.azure.com** using access credentials.</span></span> <span data-ttu-id="ca9f6-179">Adja meg a kiszolgáló-rendszergazdai jelszavát, amikor a rendszer kéri.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-179">Enter your server admin password when prompted.</span></span>

   ```bash
   psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
   ```

## <a name="create-a-new-database"></a><span data-ttu-id="ca9f6-180">Új adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="ca9f6-180">Create a New Database</span></span>
<span data-ttu-id="ca9f6-181">Ha már csatlakoztatott toohello kiszolgáló, egy üres adatbázis létrehozásához hello a parancssorból.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-181">Once you're connected toohello server, create a blank database at hello prompt.</span></span>
```bash
CREATE DATABASE mypgsqldb;
```

<span data-ttu-id="ca9f6-182">Hello parancssorba hajtható végre a következő parancs tooswitch kapcsolat található, újonnan létrehozott toohello adatbázis hello **mypgsqldb**.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-182">At hello prompt, execute hello following command tooswitch connection toohello newly created database **mypgsqldb**.</span></span>
```bash
\c mypgsqldb
```
## <a name="create-tables-in-hello-database"></a><span data-ttu-id="ca9f6-183">Táblázatok létrehozása hello adatbázis</span><span class="sxs-lookup"><span data-stu-id="ca9f6-183">Create tables in hello database</span></span>
<span data-ttu-id="ca9f6-184">Most, hogy tudja, hogyan tooconnect toohello PostgreSQL az Azure-adatbázis, azt is ismerteti hogyan toocomplete néhány alapvető műveleteket.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-184">Now that you know how tooconnect toohello Azure Database for PostgreSQL, we can go over how toocomplete some basic tasks.</span></span>

<span data-ttu-id="ca9f6-185">Először hogy hozzon létre egy táblát, és töltse be adatokkal.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-185">First, we can create a table and load it with some data.</span></span> <span data-ttu-id="ca9f6-186">Hozzon létre egy táblát, amely nyomon követi a leltáradatokat.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-186">Let's create a table that tracks inventory information.</span></span>
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

<span data-ttu-id="ca9f6-187">Újonnan létrehozott tábla tabvles hello listája most írja be a hello látható:</span><span class="sxs-lookup"><span data-stu-id="ca9f6-187">You can see hello newly created table in hello list of tabvles now by typing:</span></span>
```sql
\dt
```

## <a name="load-data-into-hello-tables"></a><span data-ttu-id="ca9f6-188">Adatok betöltése az hello táblák</span><span class="sxs-lookup"><span data-stu-id="ca9f6-188">Load data into hello tables</span></span>
<span data-ttu-id="ca9f6-189">Most, hogy a tábla, azt beilleszthet néhány adat azt.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-189">Now that we have a table, we can insert some data into it.</span></span> <span data-ttu-id="ca9f6-190">Hello nyissa meg a parancssort időszakban futtassa a következő lekérdezés tooinsert hello néhány sornyi adat</span><span class="sxs-lookup"><span data-stu-id="ca9f6-190">At hello open command prompt window, run hello following query tooinsert some rows of data</span></span>
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

<span data-ttu-id="ca9f6-191">Lehetősége van a korábban létrehozott hello táblába mintaadatok most két sorát.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-191">You have now two rows of sample data into hello table you created earlier.</span></span>

## <a name="query-and-update-hello-data-in-hello-tables"></a><span data-ttu-id="ca9f6-192">Hello táblázatok lekérdezési és frissítési hello adatait</span><span class="sxs-lookup"><span data-stu-id="ca9f6-192">Query and update hello data in hello tables</span></span>
<span data-ttu-id="ca9f6-193">Hajtható végre a következő lekérdezés tooretrieve információ hello adatbázis táblából hello.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-193">Execute hello following query tooretrieve information from hello database table.</span></span> 
```sql
SELECT * FROM inventory;
```

<span data-ttu-id="ca9f6-194">Hello hello-táblázatok adatait is frissítheti</span><span class="sxs-lookup"><span data-stu-id="ca9f6-194">You can also update hello data in hello tables</span></span>
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

<span data-ttu-id="ca9f6-195">az adatok hello sor ennek megfelelően frissül.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-195">hello row gets updated accordingly when you retrieve data.</span></span>
```sql
SELECT * FROM inventory;
```

## <a name="restore-data-tooa-previous-point-in-time"></a><span data-ttu-id="ca9f6-196">Adatpont tooa előző időpont visszaállítása</span><span class="sxs-lookup"><span data-stu-id="ca9f6-196">Restore data tooa previous point in time</span></span>
<span data-ttu-id="ca9f6-197">Képzelje el ezt a táblázatot véletlenül törölt.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-197">Imagine you have accidentally deleted this table.</span></span> <span data-ttu-id="ca9f6-198">Ebben a helyzetben valami nem egyszerűen állíthat helyre.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-198">This situation is something you cannot easily recover from.</span></span> <span data-ttu-id="ca9f6-199">Azure PostgreSQL-adatbázishoz lehetővé teszi (a legutóbbi too7 nap (alapszintű) és 35 napon (általános) hello) toogo hátsó tooany-időpontban, és állítsa vissza a időpontban tooa új kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-199">Azure Database for PostgreSQL allows you toogo back tooany point-in-time (in hello last up too7 days (Basic) and 35 days (Standard)) and restore this point-in-time tooa new server.</span></span> <span data-ttu-id="ca9f6-200">Az új kiszolgáló toorecover használhatja a törölt adatokat.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-200">You can use this new server toorecover your deleted data.</span></span> <span data-ttu-id="ca9f6-201">hello lépéseket visszaállítási hello minta kiszolgáló tooa pont követően hello tábla hozzá lett adva.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-201">hello following steps restore hello sample server tooa point before hello table was added.</span></span>

1.  <span data-ttu-id="ca9f6-202">A hello Azure-adatbázis a kiszolgáló PostgreSQL-lap, kattintson **visszaállítása** hello eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-202">On hello Azure Database for PostgreSQL page for your server, click **Restore** on hello toolbar.</span></span> <span data-ttu-id="ca9f6-203">Hello **visszaállítása** lap megnyitásakor.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-203">hello **Restore** page opens.</span></span>
  <span data-ttu-id="ca9f6-204">![Azure portál – visszaállítási űrlap beállítások](./media/tutorial-design-database-using-azure-portal/9-azure-portal-restore.png)</span><span class="sxs-lookup"><span data-stu-id="ca9f6-204">![Azure portal - Restore form options](./media/tutorial-design-database-using-azure-portal/9-azure-portal-restore.png)</span></span>
2.  <span data-ttu-id="ca9f6-205">Töltse ki a hello **visszaállítása** hello szükséges információt a képernyőn:</span><span class="sxs-lookup"><span data-stu-id="ca9f6-205">Fill out hello **Restore** form with hello required information:</span></span>

  ![Azure portál – visszaállítási űrlap beállítások](./media/tutorial-design-database-using-azure-portal/10-azure-portal-restore.png)
  - <span data-ttu-id="ca9f6-207">**Visszaállítási pont**: egy pont időponthoz kötött, amely hello server módosítása előtt válasszon</span><span class="sxs-lookup"><span data-stu-id="ca9f6-207">**Restore point**: Select a point-in-time that occurs before hello server was changed</span></span>
  - <span data-ttu-id="ca9f6-208">**Célkiszolgáló**: Adjon meg egy új kiszolgálónevet azt szeretné, hogy toorestore</span><span class="sxs-lookup"><span data-stu-id="ca9f6-208">**Target server**: Provide a new server name you want toorestore to</span></span>
  - <span data-ttu-id="ca9f6-209">**Hely**: hello régió nem választhat, alapértelmezés szerint ugyanaz, mint a forráskiszolgálón hello</span><span class="sxs-lookup"><span data-stu-id="ca9f6-209">**Location**: You cannot select hello region, by default it is same as hello source server</span></span>
  - <span data-ttu-id="ca9f6-210">**A tarifacsomag**: Ez az érték nem módosítható, ha egy kiszolgáló visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-210">**Pricing tier**: You cannot change this value when restoring a server.</span></span> <span data-ttu-id="ca9f6-211">Ugyanaz, mint a forráskiszolgálón hello.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-211">It is same as hello source server.</span></span> 
3.  <span data-ttu-id="ca9f6-212">Kattintson a **OK** toorestore hello server túl[tooa pont időponthoz kötött visszaállítás](./howto-restore-server-portal.md) előtt hello táblák törölve lett.</span><span class="sxs-lookup"><span data-stu-id="ca9f6-212">Click **OK** toorestore hello server too[restore tooa point-in-time](./howto-restore-server-portal.md) before hello tables was deleted.</span></span> <span data-ttu-id="ca9f6-213">A kiszolgáló tooa különböző pont visszaállítása időben ismétlődő új kiszolgáló létrehozása hello eredeti kiszolgálóként hello pont időben ad meg, feltéve, hogy a hello megőrzési időtartamon belül a [szolgáltatásréteg](./concepts-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="ca9f6-213">Restoring a server tooa different point in time creates a duplicate new server as hello original server as of hello point in time you specify, provided that it is within hello retention period for your [service tier](./concepts-service-tiers.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ca9f6-214">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ca9f6-214">Next Steps</span></span>
<span data-ttu-id="ca9f6-215">Ebben az oktatóanyagban megtanulta, hogyan toouse hello Azure-portál és az egyéb segédprogramokat, ha:</span><span class="sxs-lookup"><span data-stu-id="ca9f6-215">In this tutorial, you learned how toouse hello Azure portal and other utilities to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="ca9f6-216">Azure-adatbázis létrehozása PostgreSQL-hez</span><span class="sxs-lookup"><span data-stu-id="ca9f6-216">Create an Azure Database for PostgreSQL</span></span>
> * <span data-ttu-id="ca9f6-217">Hello kiszolgáló tűzfal konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ca9f6-217">Configure hello server firewall</span></span>
> * <span data-ttu-id="ca9f6-218">Használjon [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) segédprogram toocreate adatbázis</span><span class="sxs-lookup"><span data-stu-id="ca9f6-218">Use [**psql**](https://www.postgresql.org/docs/9.6/static/app-psql.html) utility toocreate a database</span></span>
> * <span data-ttu-id="ca9f6-219">Mintaadatok betöltése</span><span class="sxs-lookup"><span data-stu-id="ca9f6-219">Load sample data</span></span>
> * <span data-ttu-id="ca9f6-220">Adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="ca9f6-220">Query data</span></span>
> * <span data-ttu-id="ca9f6-221">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="ca9f6-221">Update data</span></span>
> * <span data-ttu-id="ca9f6-222">Adatok visszaállítása</span><span class="sxs-lookup"><span data-stu-id="ca9f6-222">Restore data</span></span>

<span data-ttu-id="ca9f6-223">A következő megtudhatja, hogyan toouse Azure CLI toodo hasonló feladatokat, tekintse át az oktatóanyag: [az első Azure-adatbázis kialakítása a PostgreSQL Azure parancssori felület használatával](tutorial-design-database-using-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="ca9f6-223">Next, learn how toouse Azure CLI toodo similar tasks, review this tutorial: [Design your first Azure Database for PostgreSQL using Azure CLI](tutorial-design-database-using-azure-cli.md)</span></span>
