---
title: "aaaSecure az Azure SQL Database adatbázishoz |} Microsoft Docs"
description: "További információk a módszerek és szolgáltatások toosecure az Azure SQL-adatbázis."
services: sql-database
documentationcenter: 
author: DRediske
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 06/28/2017
ms.author: daredis
ms.openlocfilehash: 1450d633d6f65faf1b8a2dc0dc7dfe996fb0719d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="secure-your-azure-sql-database"></a><span data-ttu-id="45979-103">Az Azure SQL-adatbázis védelme</span><span class="sxs-lookup"><span data-stu-id="45979-103">Secure your Azure SQL Database</span></span>

<span data-ttu-id="45979-104">SQL-adatbázis adatait korlátozza a hozzáférést tooyour adatbázis tűzfalszabályokat igénylő felhasználók tooprove azok identitáskezelési és engedélyezési toodata szerepköralapú tagságát és engedélyeket, valamint a hitelesítési mechanizmusok használatával biztonságossá tételére. a sorszintű biztonsági és dinamikus adatmaszkolási.</span><span class="sxs-lookup"><span data-stu-id="45979-104">SQL Database secures your data by limiting access tooyour database using firewall rules, authentication mechanisms requiring users tooprove their identity, and authorization toodata through role-based memberships and permissions, as well as through row-level security and dynamic data masking.</span></span>

<span data-ttu-id="45979-105">Az adatbázis rosszindulatú felhasználók vagy néhány egyszerű lépésben jogosulatlan hozzáférés elleni védelmének hello növelhető.</span><span class="sxs-lookup"><span data-stu-id="45979-105">You can improve hello protection of your database against malicious users or unauthorized access with just a few simple steps.</span></span> <span data-ttu-id="45979-106">Ebben az oktatóanyagban elsajátíthatja, hogy:</span><span class="sxs-lookup"><span data-stu-id="45979-106">In this tutorial you learn to:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="45979-107">A kiszolgáló hello Azure-portálon a kiszolgálószintű tűzfal-szabályok beállítása</span><span class="sxs-lookup"><span data-stu-id="45979-107">Set up server-level firewall rules for your server in hello Azure portal</span></span>
> * <span data-ttu-id="45979-108">Az adatbázisban az SSMS használatával vonatkozó adatbázis-szintű tűzfalszabályok beállítása</span><span class="sxs-lookup"><span data-stu-id="45979-108">Set up database-level firewall rules for your database using SSMS</span></span>
> * <span data-ttu-id="45979-109">Csatlakozás a biztonságos kapcsolati karakterláncok használatával tooyour adatbázis</span><span class="sxs-lookup"><span data-stu-id="45979-109">Connect tooyour database using a secure connection string</span></span>
> * <span data-ttu-id="45979-110">Felhasználói hozzáférés kezelése</span><span class="sxs-lookup"><span data-stu-id="45979-110">Manage user access</span></span>
> * <span data-ttu-id="45979-111">A titkosított adatok védelme</span><span class="sxs-lookup"><span data-stu-id="45979-111">Protect your data with encryption</span></span>
> * <span data-ttu-id="45979-112">SQL-adatbázis naplózásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="45979-112">Enable SQL Database auditing</span></span>
> * <span data-ttu-id="45979-113">SQL-adatbázis fenyegetésészlelés engedélyezéséhez</span><span class="sxs-lookup"><span data-stu-id="45979-113">Enable SQL Database threat detection</span></span>

<span data-ttu-id="45979-114">Ha nem rendelkezik Azure-előfizetéssel, [ingyenes fiók létrehozását](https://azure.microsoft.com/free/) megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="45979-114">If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="45979-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="45979-115">Prerequisites</span></span>

<span data-ttu-id="45979-116">toocomplete ezen oktatóanyag, győződjön meg arról, hogy rendelkezik a következő hello:</span><span class="sxs-lookup"><span data-stu-id="45979-116">toocomplete this tutorial, make sure you have hello following:</span></span>

- <span data-ttu-id="45979-117">Telepített hello legújabb verziójának [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).</span><span class="sxs-lookup"><span data-stu-id="45979-117">Installed hello newest version of [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).</span></span> 
- <span data-ttu-id="45979-118">Telepített Microsoft Excel</span><span class="sxs-lookup"><span data-stu-id="45979-118">Installed Microsoft Excel</span></span>
- <span data-ttu-id="45979-119">Létrehozott egy Azure SQL server és adatbázis - lásd [hello Azure-portálon az Azure SQL-adatbázis létrehozása](sql-database-get-started-portal.md), [hello Azure parancssori felület használatával egyetlen Azure SQL-adatbázis létrehozása](sql-database-get-started-cli.md), és [hozzon létre egy Azure SQL adatbázis-PowerShell-lel](sql-database-get-started-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="45979-119">Created an Azure SQL server and database - See [Create an Azure SQL database in hello Azure portal](sql-database-get-started-portal.md), [Create a single Azure SQL database using hello Azure CLI](sql-database-get-started-cli.md), and [Create a single Azure SQL database using PowerShell](sql-database-get-started-powershell.md).</span></span> 

## <a name="log-in-toohello-azure-portal"></a><span data-ttu-id="45979-120">Jelentkezzen be toohello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="45979-120">Log in toohello Azure portal</span></span>

<span data-ttu-id="45979-121">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="45979-121">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>

## <a name="create-a-server-level-firewall-rule-in-hello-azure-portal"></a><span data-ttu-id="45979-122">Hozzon létre egy kiszolgálószintű tűzfalszabályt hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="45979-122">Create a server-level firewall rule in hello Azure portal</span></span>

<span data-ttu-id="45979-123">SQL-adatbázisok védelmének tűzfal az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="45979-123">SQL databases are protected by a firewall in Azure.</span></span> <span data-ttu-id="45979-124">Alapértelmezés szerint minden kapcsolatok toohello server és hello adatbázisok hello-kiszolgálón belül kivételével az egyéb Azure-szolgáltatásokhoz érkező kapcsolatokat utasítja el.</span><span class="sxs-lookup"><span data-stu-id="45979-124">By default, all connections toohello server and hello databases inside hello server are rejected except for connections from other Azure services.</span></span> <span data-ttu-id="45979-125">További információkért lásd: [Azure SQL Database kiszolgáló- és adatbázis tűzfalszabályok](sql-database-firewall-configure.md).</span><span class="sxs-lookup"><span data-stu-id="45979-125">For more information, see [Azure SQL Database server-level and database-level firewall rules](sql-database-firewall-configure.md).</span></span>

<span data-ttu-id="45979-126">Ez a legbiztonságosabb konfiguráció hello tooset "Hozzáférés engedélyezése tooAzure szolgáltatások" tooOFF.</span><span class="sxs-lookup"><span data-stu-id="45979-126">hello most secure configuration is tooset 'Allow access tooAzure services' tooOFF.</span></span> <span data-ttu-id="45979-127">Ha tooconnect toohello adatbázis egy Azure virtuális vagy felhőalapú szolgáltatásból kell, hogy hozzon létre egy [foglalt IP-cím](../virtual-network/virtual-networks-reserved-public-ip.md) , és csak hello szolgáltatás számára fenntartott IP-cím elérést hello tűzfalon keresztül teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="45979-127">If you need tooconnect toohello database from an Azure VM or cloud service, you should create a [Reserved IP](../virtual-network/virtual-networks-reserved-public-ip.md) and allow only hello reserved IP address access through hello firewall.</span></span> 

<span data-ttu-id="45979-128">Kövesse az alábbi lépéseket toocreate egy [SQL-adatbázis kiszolgálószintű tűzfalszabály](sql-database-firewall-configure.md) egy adott IP-címről a kiszolgáló tooallow kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="45979-128">Follow these steps toocreate a [SQL Database server-level firewall rule](sql-database-firewall-configure.md) for your server tooallow connections from a specific IP address.</span></span> 

> [!NOTE]
> <span data-ttu-id="45979-129">Ha létrehozott egy mintaadatbázis az Azure-ban egy előző oktatóanyagok hello vagy quickstarts és egy számítógépen hello az oktatóanyag végrehajtásával azonos IP-címet, hogy az informatikai rendelkezett, amikor ezen oktatóanyag segítségével, telefonon, ezt a lépést kihagyhatja, hogy már létrehozott egy kiszolgálószintű tűzfalszabályt.</span><span class="sxs-lookup"><span data-stu-id="45979-129">If you have created a sample database in Azure using one of hello previous tutorials or quickstarts and are performing this tutorial on a computer with hello same IP address that it had when you walked through those tutorials, you can skip this step as you will have already created a server-level firewall rule.</span></span>
>

1. <span data-ttu-id="45979-130">Kattintson a **SQL-adatbázisok** adatbázisból hello bal menüre, majd hello szeretné tooconfigure hello tűzfalszabályt a hello **SQL-adatbázisok** lap.</span><span class="sxs-lookup"><span data-stu-id="45979-130">Click **SQL databases** from hello left-hand menu and click hello database you would like tooconfigure hello firewall rule for on hello **SQL databases** page.</span></span> <span data-ttu-id="45979-131">hello áttekintő lapjára jut a adatbázis megnyílik, teljes mértékben hello megjelenítő minősített kiszolgáló neve (például **mynewserver-20170313.database.windows.net**) és további konfigurációs lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="45979-131">hello overview page for your database opens, showing you hello fully qualified server name (such as **mynewserver-20170313.database.windows.net**) and provides options for further configuration.</span></span>

      ![kiszolgálói tűzfalszabály](./media/sql-database-security-tutorial/server-firewall-rule.png) 

2. <span data-ttu-id="45979-133">Kattintson a **kiszolgáló tűzfalának beállítása** hello eszköztár hello előző ábrának megfelelően.</span><span class="sxs-lookup"><span data-stu-id="45979-133">Click **Set server firewall** on hello toolbar as shown in hello previous image.</span></span> <span data-ttu-id="45979-134">Hello **tűzfalbeállítások** hello SQL Database-kiszolgálóhoz tartozó lapon nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="45979-134">hello **Firewall settings** page for hello SQL Database server opens.</span></span> 

3. <span data-ttu-id="45979-135">Kattintson a **ügyfél IP-cím hozzáadása** az eszköztár tooadd hello nyilvános IP-címe hello számítógéphez csatlakoztatott toohello portál hello vagy hello tűzfalszabály manuálisan adja meg, majd kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="45979-135">Click **Add client IP** on hello toolbar tooadd hello public IP address of hello computer connected toohello portal with or enter hello firewall rule manually and then click **Save**.</span></span>

      ![kiszolgálótűzfal-szabály beállítása](./media/sql-database-security-tutorial/server-firewall-rule-set.png) 

4. <span data-ttu-id="45979-137">Kattintson a **OK** majd hello **X** tooclose hello **tűzfalbeállítások** lap.</span><span class="sxs-lookup"><span data-stu-id="45979-137">Click **OK** and then click hello **X** tooclose hello **Firewall settings** page.</span></span>

<span data-ttu-id="45979-138">Csatlakoztathatja a hello server tooany adatbázis a megadott hello IP-cím vagy IP-címtartomány.</span><span class="sxs-lookup"><span data-stu-id="45979-138">You can now connect tooany database in hello server with hello specified IP address or IP address range.</span></span>

> [!NOTE]
> <span data-ttu-id="45979-139">Az SQL Database az 1433-as porton kommunikál.</span><span class="sxs-lookup"><span data-stu-id="45979-139">SQL Database communicates over port 1433.</span></span> <span data-ttu-id="45979-140">Ha a vállalati hálózatból származó tooconnect, a hálózati tűzfal előfordulhat, hogy nem engedélyezett a 1433-as port kimenő forgalmát.</span><span class="sxs-lookup"><span data-stu-id="45979-140">If you are trying tooconnect from within a corporate network, outbound traffic over port 1433 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="45979-141">Ha igen, csak akkor tudja tooconnect tooyour Azure SQL adatbázis-kiszolgáló kivéve, ha az IT-részleg megnyitja az 1433-as port.</span><span class="sxs-lookup"><span data-stu-id="45979-141">If so, you will not be able tooconnect tooyour Azure SQL Database server unless your IT department opens port 1433.</span></span>
>

## <a name="create-a-database-level-firewall-rule-using-ssms"></a><span data-ttu-id="45979-142">Hozzon létre egy adatbázis-szintű tűzfalszabályt SSMS használatával</span><span class="sxs-lookup"><span data-stu-id="45979-142">Create a database-level firewall rule using SSMS</span></span>

<span data-ttu-id="45979-143">Adatbázis-szintű tűzfalszabályok lehetővé teszik a különböző tűzfalbeállítások toocreate azonos logikai kiszolgáló és toocreate tűzfalszabályokat, amelyek hordozható – azaz követik az hello adatbázis során hello belül a különböző adatbázisokhoz egy [feladatátvételi ](sql-database-geo-replication-overview.md) hello SQL-kiszolgálón tárolt helyett.</span><span class="sxs-lookup"><span data-stu-id="45979-143">Database-level firewall rules enable you toocreate different firewall settings for different databases within hello same logical server and toocreate firewall rules that are portable - meaning that they follow hello database during a [failover](sql-database-geo-replication-overview.md) rather than being stored on hello SQL server.</span></span> <span data-ttu-id="45979-144">Adatbázis-szintű tűzfalszabályok csak akkor lehet Transact-SQL-utasítások segítségével konfigurált csak hello első kiszolgálószintű tűzfalszabály konfigurálása után.</span><span class="sxs-lookup"><span data-stu-id="45979-144">Database-level firewall rules can only be configured by using Transact-SQL statements and only after you have configured hello first server-level firewall rule.</span></span> <span data-ttu-id="45979-145">További információkért lásd: [Azure SQL Database kiszolgáló- és adatbázis tűzfalszabályok](sql-database-firewall-configure.md).</span><span class="sxs-lookup"><span data-stu-id="45979-145">For more information, see [Azure SQL Database server-level and database-level firewall rules](sql-database-firewall-configure.md).</span></span>

<span data-ttu-id="45979-146">A következő ezeket a lépéseket toocreate egy adatbázis-specifikus tűzfalszabályt.</span><span class="sxs-lookup"><span data-stu-id="45979-146">Follows these steps toocreate a database-specific firewall rule.</span></span>

1. <span data-ttu-id="45979-147">Csatlakozás tooyour adatbázis, például a használatával [SQL Server Management Studio](./sql-database-connect-query-ssms.md).</span><span class="sxs-lookup"><span data-stu-id="45979-147">Connect tooyour database, for example using [SQL Server Management Studio](./sql-database-connect-query-ssms.md).</span></span>

2. <span data-ttu-id="45979-148">Az Object Explorerben kattintson a jobb gombbal a hello adatbázis tooadd tűzfal vonatkozó szabály, és kattintson a kívánt **új lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="45979-148">In Object Explorer, right-click on hello database you want tooadd a firewall rule for and click **New Query**.</span></span> <span data-ttu-id="45979-149">Üres lekérdezés megnyílik egy ablak, amely csatlakoztatott tooyour adatbázis.</span><span class="sxs-lookup"><span data-stu-id="45979-149">A blank query window opens that is connected tooyour database.</span></span>

3. <span data-ttu-id="45979-150">Hello lekérdezési ablakban hello IP cím tooyour nyilvános IP-cím módosítása, és majd hajtsa végre a lekérdezés a következő hello:</span><span class="sxs-lookup"><span data-stu-id="45979-150">In hello query window, modify hello IP address tooyour public IP address and then execute hello following query:</span></span>

    ```sql
    EXECUTE sp_set_database_firewall_rule N'Example DB Rule','0.0.0.4','0.0.0.4';
    ```

4. <span data-ttu-id="45979-151">Hello eszköztáron kattintson **Execute** toocreate hello tűzfalszabály.</span><span class="sxs-lookup"><span data-stu-id="45979-151">On hello toolbar, click **Execute** toocreate hello firewall rule.</span></span>

## <a name="view-how-tooconnect-an-application-tooyour-database-using-a-secure-connection-string"></a><span data-ttu-id="45979-152">Hogyan tooconnect egy alkalmazás tooyour adatbázis használata a biztonságos kapcsolati karakterláncok megjelenítése</span><span class="sxs-lookup"><span data-stu-id="45979-152">View how tooconnect an application tooyour database using a secure connection string</span></span>

<span data-ttu-id="45979-153">tooensure egy ügyfélalkalmazást és az SQL-adatbázis közötti biztonságos, titkosított kapcsolatot, hello kapcsolati karakterlánc toobe konfigurálva van:</span><span class="sxs-lookup"><span data-stu-id="45979-153">tooensure a secure, encrypted connection between a client application and SQL Database, hello connection string has toobe configured to:</span></span>

- <span data-ttu-id="45979-154">Egy titkosított kapcsolati kérelmek és</span><span class="sxs-lookup"><span data-stu-id="45979-154">Request an encrypted connection, and</span></span>
- <span data-ttu-id="45979-155">toonot megbízhatósági hello kiszolgálói tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="45979-155">toonot trust hello server certificate.</span></span> 

<span data-ttu-id="45979-156">Ez kapcsolatot hoz létre Transport Layer Security (TLS) használatával, és csökkenti-átjárójának hello kockázatát.</span><span class="sxs-lookup"><span data-stu-id="45979-156">This establishes a connection using Transport Layer Security (TLS) and reduces hello risk of man-in-the-middle attacks.</span></span> <span data-ttu-id="45979-157">Szerezhetők be megfelelően konfigurált kapcsolati karakterláncokat az SQL-adatbázis, a támogatott ügyfél illesztőprogramok hello Azure-portálon az ADO.net ezen a képernyőfelvételen látható módon.</span><span class="sxs-lookup"><span data-stu-id="45979-157">You can obtain correctly configured connection strings for your SQL Database for supported client drivers from hello Azure portal as shown for ADO.net in this screenshot.</span></span>

1. <span data-ttu-id="45979-158">Válassza ki **SQL-adatbázisok** hello bal oldali menüben kattintson a hello adatbázis **SQL-adatbázisok** lap.</span><span class="sxs-lookup"><span data-stu-id="45979-158">Select **SQL databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span>

2. <span data-ttu-id="45979-159">A hello **áttekintése** az adatbázis lapján kattintson **adatbázis-kapcsolati karakterláncok megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="45979-159">On hello **Overview** page for your database, click **Show database connection strings**.</span></span>

3. <span data-ttu-id="45979-160">Felülvizsgálati hello teljes **ADO.NET** kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="45979-160">Review hello complete **ADO.NET** connection string.</span></span>

    ![ADO.NET kapcsolati karakterlánc](./media/sql-database-security-tutorial/adonet-connection-string.png)

## <a name="creating-database-users"></a><span data-ttu-id="45979-162">Adatbázis-felhasználók létrehozásával</span><span class="sxs-lookup"><span data-stu-id="45979-162">Creating database users</span></span>

<span data-ttu-id="45979-163">Mielőtt létrehozna azokat a felhasználókat, először közül kell választania az Azure SQL Database által támogatott két hitelesítési típusok egyikét:</span><span class="sxs-lookup"><span data-stu-id="45979-163">Before creating any users, you must first choose from one of two authentication types supported by Azure SQL Database:</span></span> 

<span data-ttu-id="45979-164">**SQL-hitelesítés**, felhasználónevet és jelszót használó a bejelentkezések során, és a felhasználók, akik érvényes csak a hello logikai kiszolgáló az adott adatbázis környezetében.</span><span class="sxs-lookup"><span data-stu-id="45979-164">**SQL Authentication**, which uses username and password for logins and users that are valid only in hello context of a specific database within a logical server.</span></span> 

<span data-ttu-id="45979-165">**Az Azure Active Directory-hitelesítéssel**, amely Azure Active Directory által kezelt identitások használja.</span><span class="sxs-lookup"><span data-stu-id="45979-165">**Azure Active Directory Authentication**, which uses identities managed by Azure Active Directory.</span></span> 

<span data-ttu-id="45979-166">Ha azt szeretné, hogy toouse [Azure Active Directory](./sql-database-aad-authentication.md) tooauthenticate SQL-adatbázis, a feltöltött Azure Active Directory léteznie kell a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="45979-166">If you want toouse [Azure Active Directory](./sql-database-aad-authentication.md) tooauthenticate against SQL Database, a populated Azure Active Directory must exist before you can proceed.</span></span>

<span data-ttu-id="45979-167">Kövesse ezeket a lépéseket toocreate SQL-hitelesítést használó felhasználó:</span><span class="sxs-lookup"><span data-stu-id="45979-167">Follow these steps toocreate a user using SQL Authentication:</span></span>

1. <span data-ttu-id="45979-168">Csatlakozás tooyour adatbázis, például a használatával [SQL Server Management Studio](./sql-database-connect-query-ssms.md) server rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="45979-168">Connect tooyour database, for example using [SQL Server Management Studio](./sql-database-connect-query-ssms.md) using your server admin credentials.</span></span>

2. <span data-ttu-id="45979-169">Az Object Explorerben kattintson a jobb gombbal a hello adatbázis, a kívánt tooadd egy új felhasználót, majd kattintson az **új lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="45979-169">In Object Explorer, right-click on hello database you want tooadd a new user on and click **New Query**.</span></span> <span data-ttu-id="45979-170">Üres lekérdezés megnyílik egy ablak, amely csatlakoztatott toohello kiválasztott adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="45979-170">A blank query window opens that is connected toohello selected database.</span></span>

3. <span data-ttu-id="45979-171">Adja meg a következő lekérdezés hello hello lekérdezési ablakban:</span><span class="sxs-lookup"><span data-stu-id="45979-171">In hello query window, enter hello following query:</span></span>

    ```sql
    CREATE USER ApplicationUser WITH PASSWORD = 'YourStrongPassword1';
    ```

4. <span data-ttu-id="45979-172">Hello eszköztáron kattintson **Execute** toocreate hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="45979-172">On hello toolbar, click **Execute** toocreate hello user.</span></span>

5. <span data-ttu-id="45979-173">Alapértelmezés szerint a hello felhasználói kapcsolódhatnak toohello adatbázis, de nincsenek engedélyek tooread vagy írási adatok.</span><span class="sxs-lookup"><span data-stu-id="45979-173">By default, hello user can connect toohello database, but has no permissions tooread or write data.</span></span> <span data-ttu-id="45979-174">toogrant ezen engedélyek toohello az újonnan létrehozott felhasználó, a következő két parancsot egy új lekérdezési ablak hello végrehajtása</span><span class="sxs-lookup"><span data-stu-id="45979-174">toogrant these permissions toohello newly created user, execute hello following two commands in a new query window</span></span>

    ```sql
    ALTER ROLE db_datareader ADD MEMBER ApplicationUser;
    ALTER ROLE db_datawriter ADD MEMBER ApplicationUser;
    ```

<span data-ttu-id="45979-175">A nem rendszergazdai fiókok hello adatbázis szintű tooconnect tooyour adatbázis-kivéve, ha szüksége tooexecute rendszergazdai feladatokat, például új felhasználók létrehozása ajánlott eljárás toocreate.</span><span class="sxs-lookup"><span data-stu-id="45979-175">It is best practice toocreate these non-administrator accounts at hello database level tooconnect tooyour database unless you need tooexecute administrator tasks like creating new users.</span></span> <span data-ttu-id="45979-176">Tekintse át a hello [Azure Active Directory-oktatóanyag](./sql-database-aad-authentication-configure.md) hogyan tooauthenticate Azure Active Directory használatával.</span><span class="sxs-lookup"><span data-stu-id="45979-176">Please review hello [Azure Active Directory tutorial](./sql-database-aad-authentication-configure.md) on how tooauthenticate using Azure Active Directory.</span></span>


## <a name="protect-your-data-with-encryption"></a><span data-ttu-id="45979-177">A titkosított adatok védelme</span><span class="sxs-lookup"><span data-stu-id="45979-177">Protect your data with encryption</span></span>

<span data-ttu-id="45979-178">Az Azure SQL Database átlátható adattitkosítás (TDE) automatikusan titkosítja az adatok aktívan, anélkül, hogy a módosítások toohello elérése során hello titkosított adatbázis-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="45979-178">Azure SQL Database transparent data encryption (TDE) automatically encrypts your data at rest, without requiring any changes toohello application accessing hello encrypted database.</span></span> <span data-ttu-id="45979-179">Az újonnan létrehozott adatbázisok esetén TDE alapértelmezés szerint van bekapcsolva.</span><span class="sxs-lookup"><span data-stu-id="45979-179">For newly created databases, TDE is on by default.</span></span> <span data-ttu-id="45979-180">az adatbázis vagy tooverify, amely TDE, a TDE tooenable kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="45979-180">tooenable TDE for your database or tooverify that TDE is on, follow these steps:</span></span>

1. <span data-ttu-id="45979-181">Válassza ki **SQL-adatbázisok** hello bal oldali menüben kattintson a hello adatbázis **SQL-adatbázisok** lap.</span><span class="sxs-lookup"><span data-stu-id="45979-181">Select **SQL databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 

2. <span data-ttu-id="45979-182">Kattintson a **átlátható adattitkosítás** tooopen hello konfigurálása lapon TDE.</span><span class="sxs-lookup"><span data-stu-id="45979-182">Click on **Transparent data encryption** tooopen hello configuration page for TDE.</span></span>

    ![Transzparens adattitkosítás](./media/sql-database-security-tutorial/transparent-data-encryption-enabled.png)

3. <span data-ttu-id="45979-184">Ha szükséges, állítsa be **adattitkosítás** tooON kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="45979-184">If necessary, set **Data encryption** tooON and click **Save**.</span></span>

<span data-ttu-id="45979-185">hello titkosítási folyamat elindul hello háttérben.</span><span class="sxs-lookup"><span data-stu-id="45979-185">hello encryption process starts in hello background.</span></span> <span data-ttu-id="45979-186">Kísérheti hello segítségével csatlakozó tooSQL adatbázis [SQL Server Management Studio](./sql-database-connect-query-ssms.md) hello encryption_state oszlopa hello lekérdezésével `sys.dm_database_encryption_keys` nézet.</span><span class="sxs-lookup"><span data-stu-id="45979-186">You can monitor hello progress by connecting tooSQL Database using [SQL Server Management Studio](./sql-database-connect-query-ssms.md) by querying hello encryption_state column of hello `sys.dm_database_encryption_keys` view.</span></span>

## <a name="enable-sql-database-auditing-if-necessary"></a><span data-ttu-id="45979-187">SQL-adatbázis naplózásának engedélyezése, ha szükséges</span><span class="sxs-lookup"><span data-stu-id="45979-187">Enable SQL Database auditing, if necessary</span></span>

<span data-ttu-id="45979-188">Az Azure SQL Database Auditing követi az adatbázisban, és beírja őket az Azure Storage-fiókban tooan naplót.</span><span class="sxs-lookup"><span data-stu-id="45979-188">Azure SQL Database Auditing tracks database events and writes them tooan audit log in your Azure Storage account.</span></span> <span data-ttu-id="45979-189">Naplózás segít törvényi megfelelőség fenntartásában, ismerje meg adatbázis-tevékenység, és betekintést az eltérések és rendellenességek feltárását, jelezheti a potenciális biztonsági problémát.</span><span class="sxs-lookup"><span data-stu-id="45979-189">Auditing can help you maintain regulatory compliance, understand database activity, and gain insight into discrepancies and anomalies that could indicate potential security violations.</span></span> <span data-ttu-id="45979-190">Kövesse ezeket a lépéseket toocreate az SQL-adatbázis alapértelmezett naplózási házirend:</span><span class="sxs-lookup"><span data-stu-id="45979-190">Follow these steps toocreate a default auditing policy for your SQL database:</span></span>

1. <span data-ttu-id="45979-191">Válassza ki **SQL-adatbázisok** hello bal oldali menüben kattintson a hello adatbázis **SQL-adatbázisok** lap.</span><span class="sxs-lookup"><span data-stu-id="45979-191">Select **SQL databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 

2. <span data-ttu-id="45979-192">Hello-beállítások panelen válassza ki **naplózás és Fenyegetésészlelés**.</span><span class="sxs-lookup"><span data-stu-id="45979-192">In hello Settings blade, select **Auditing & Threat Detection**.</span></span> <span data-ttu-id="45979-193">Figyelje meg, hogy kiszolgálószintű naplózás letiltott állapotában és, hogy egy **beállításainak megtekintéséhez** hivatkozás, amely lehetővé teszi a tooview hello kiszolgáló naplózási beállításainak és módosítása az ebben a környezetben.</span><span class="sxs-lookup"><span data-stu-id="45979-193">Notice that sever-level auditing is diabled and that there is a **View server settings** link that allows you tooview or modify hello server auditing settings from this context.</span></span>

    ![Naplózás panel](./media/sql-database-security-tutorial/auditing-get-started-settings.png)

3. <span data-ttu-id="45979-195">Ha jobban szeret tooenable egy naplózási típust (vagy a hely?) különbözik a hello kiszolgálószinten megadott hello kapcsolja **ON** naplózás esetén hello válassza **Blob** naplózás típusát.</span><span class="sxs-lookup"><span data-stu-id="45979-195">If you prefer tooenable an Audit type (or location?) different from hello one specified at hello server level, turn **ON** Auditing, and choose hello **Blob** Auditing Type.</span></span> <span data-ttu-id="45979-196">Ha server Blobnaplózási funkció engedélyezve van, hello konfigurált adatbázis-ellenőrzéshez és hello server Blob naplózási jelen.</span><span class="sxs-lookup"><span data-stu-id="45979-196">If server Blob auditing is enabled, hello database configured audit will exist side by side with hello server Blob audit.</span></span>

    ![A naplózás bekapcsolása](./media/sql-database-security-tutorial/auditing-get-started-turn-on.png)

4. <span data-ttu-id="45979-198">Válassza ki **tárolási részletek** tooopen hello naplózási naplók tárolás panel.</span><span class="sxs-lookup"><span data-stu-id="45979-198">Select **Storage Details** tooopen hello Audit Logs Storage Blade.</span></span> <span data-ttu-id="45979-199">Jelölje be hello Azure storage-fiók naplók menteni és hello megőrzési időtartam, mely hello után törlődik a régi naplókat, majd kattintson **OK** hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="45979-199">Select hello Azure storage account where logs will be saved, and hello retention period, after which hello old logs will be deleted, then click **OK** at hello bottom.</span></span> 

   > [!TIP]
   > <span data-ttu-id="45979-200">Használja az azonos hello összes naplózott adatbázisok tooget hello hatékonyabb működtetését hello naplózás tárfiók jelentések sablonok.</span><span class="sxs-lookup"><span data-stu-id="45979-200">Use hello same storage account for all audited databases tooget hello most out of hello auditing reports templates.</span></span>
   > 

5. <span data-ttu-id="45979-201">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="45979-201">Click **Save**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="45979-202">Ha azt szeretné, hogy toocustomize hello naplózott események, ehhez PowerShell vagy a REST API - lásd: hello [automatizálási (PowerShell / REST-API)](sql-database-auditing.md#subheading-7) című szakaszban talál további információt.</span><span class="sxs-lookup"><span data-stu-id="45979-202">If you want toocustomize hello audited events, you can do this via PowerShell or REST API - see hello [Automation (PowerShell / REST API)](sql-database-auditing.md#subheading-7) section for more details.</span></span>
>

## <a name="enable-sql-database-threat-detection"></a><span data-ttu-id="45979-203">SQL-adatbázis fenyegetésészlelés engedélyezéséhez</span><span class="sxs-lookup"><span data-stu-id="45979-203">Enable SQL Database threat detection</span></span>

<span data-ttu-id="45979-204">A Fenyegetésészlelés biztonsági, amely lehetővé teszi, hogy az ügyfelek toodetect és toopotential fenyegetések válaszol, a rendellenes tevékenységek adja meg a biztonsági riasztások előforduló új réteget biztosít.</span><span class="sxs-lookup"><span data-stu-id="45979-204">Threat Detection provides a new layer of security, which enables customers toodetect and respond toopotential threats as they occur by providing security alerts on anomalous activities.</span></span> <span data-ttu-id="45979-205">Felhasználók felfedezheti hello gyanús események SQL Database Auditing toodetermine használatával, ha egy kísérlet tooaccess, a biztonsági szabályok megsértésére eredménye, vagy hello adatbázisban kihasználására.</span><span class="sxs-lookup"><span data-stu-id="45979-205">Users can explore hello suspicious events using SQL Database Auditing toodetermine if they result from an attempt tooaccess, breach or exploit data in hello database.</span></span> <span data-ttu-id="45979-206">A Fenyegetésészlelés teszi egyszerű tooaddress potenciális fenyegetések toohello adatbázis hello kell toobe egy biztonsági szakértő nélkül, vagy speciális biztonsági rendszerek figyelése kezelése.</span><span class="sxs-lookup"><span data-stu-id="45979-206">Threat Detection makes it simple tooaddress potential threats toohello database without hello need toobe a security expert or manage advanced security monitoring systems.</span></span>
<span data-ttu-id="45979-207">A Fenyegetésészlelés például bizonyos adatbázist érintő rendellenes tevékenységeket utaló esetleges SQL injektálási kísérletek begyűjtésére észleli.</span><span class="sxs-lookup"><span data-stu-id="45979-207">For example, Threat Detection detects certain anomalous database activities indicating potential SQL injection attempts.</span></span> <span data-ttu-id="45979-208">SQL-injektálás az egyik hello közös webes alkalmazás biztonsági problémái hello Internet, adatvezérelt használt tooattack alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="45979-208">SQL injection is one of hello common Web application security issues on hello Internet, used tooattack data-driven applications.</span></span> <span data-ttu-id="45979-209">A támadók előnyeit alkalmazás biztonsági rések tooinject rosszindulatú SQL-utasítások mezőkbe alkalmazás bejegyzést, megsértése vagy hello adatbázis adatainak módosítása.</span><span class="sxs-lookup"><span data-stu-id="45979-209">Attackers take advantage of application vulnerabilities tooinject malicious SQL statements into application entry fields, for breaching or modifying data in hello database.</span></span>

1. <span data-ttu-id="45979-210">Keresse meg a toohello konfigurációs panelen hello toomonitor kívánt SQL-adatbázis található.</span><span class="sxs-lookup"><span data-stu-id="45979-210">Navigate toohello configuration blade of hello SQL database you want toomonitor.</span></span> <span data-ttu-id="45979-211">Hello-beállítások panelen válassza ki **naplózás és Fenyegetésészlelés**.</span><span class="sxs-lookup"><span data-stu-id="45979-211">In hello Settings blade, select **Auditing & Threat Detection**.</span></span>

    ![Navigációs ablaktábla](./media/sql-database-security-tutorial/auditing-get-started-settings.png)
2. <span data-ttu-id="45979-213">A hello **naplózás és Fenyegetésészlelés** konfigurációs panelen kapcsolja **ON** naplózás, amely megjeleníti hello fenyegetés észlelési beállítások.</span><span class="sxs-lookup"><span data-stu-id="45979-213">In hello **Auditing & Threat Detection** configuration blade turn **ON** auditing, which will display hello threat detection settings.</span></span>

3. <span data-ttu-id="45979-214">Kapcsolja be **ON** veszélyforrások detektálása.</span><span class="sxs-lookup"><span data-stu-id="45979-214">Turn **ON** threat detection.</span></span>

4. <span data-ttu-id="45979-215">Konfigurálhatja az e-mailek adatbázist érintő rendellenes tevékenységeket észlelése a biztonsági riasztásokat fogadó hello listáját.</span><span class="sxs-lookup"><span data-stu-id="45979-215">Configure hello list of emails that will receive security alerts upon detection of anomalous database activities.</span></span>

5. <span data-ttu-id="45979-216">Kattintson a **mentése** a hello **naplózási & Threat detection** panel toosave hello új vagy frissített naplózás és a fenyegetések szabályzat.</span><span class="sxs-lookup"><span data-stu-id="45979-216">Click **Save** in hello **Auditing & Threat detection** blade toosave hello new or updated auditing and threat detection policy.</span></span>

    ![Navigációs ablaktábla](./media/sql-database-security-tutorial/td-turn-on-threat-detection.png)

    <span data-ttu-id="45979-218">Adatbázist érintő rendellenes tevékenységeket a rendszer észleli, ha adatbázist érintő rendellenes tevékenységeket a észlelésekor e-mailben értesítést fog kapni.</span><span class="sxs-lookup"><span data-stu-id="45979-218">If anomalous database activities are detected, you will receive an email notification upon detection of anomalous database activities.</span></span> <span data-ttu-id="45979-219">hello e-mail hello gyanús biztonsági esemény hello rendellenes tevékenységek, az adatbázis neve, a kiszolgáló nevét és hello esemény időpontja hello jellege beleértve tájékoztatást fogunk adni.</span><span class="sxs-lookup"><span data-stu-id="45979-219">hello email will provide information on hello suspicious security event including hello nature of hello anomalous activities, database name, server name and hello event time.</span></span> <span data-ttu-id="45979-220">Emellett az információt nyújt a lehetséges okok és ajánlott műveletek tooinvestigate és hello potenciális fenyegetést toohello adatbázis mérsékelni.</span><span class="sxs-lookup"><span data-stu-id="45979-220">In addition, it will provide information on possible causes and recommended actions tooinvestigate and mitigate hello potential threat toohello database.</span></span> <span data-ttu-id="45979-221">hello milyen toodo végig kell hogy következő lépések bejárása ilyen egy e-maileket kapni:</span><span class="sxs-lookup"><span data-stu-id="45979-221">hello next steps walk you through what toodo should you receive such an email:</span></span>

    ![Fenyegetések észlelése e-mail](./media/sql-database-threat-detection-get-started/4_td_email.png)

6. <span data-ttu-id="45979-223">Hello e-mailben, kattintson a hello **Azure SQL-naplózás napló** hivatkozás, amely hello Azure-portálon elindul, és hello vonatkozó naplózási bejegyzések hello gyanús esemény hello időpontja körül megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="45979-223">In hello email, click on hello **Azure SQL Auditing Log** link, which will launch hello Azure portal and show hello relevant auditing records around hello time of hello suspicious event.</span></span>

    ![Auditálási rekordok](./media/sql-database-threat-detection-get-started/5_td_audit_records.png)

7. <span data-ttu-id="45979-225">Kattintson a hello naplózási bejegyzések tooview további részleteket a hello adatbázis gyanús tevékenységek, például az SQL-utasítás hiba okát, és az ügyfél IP.</span><span class="sxs-lookup"><span data-stu-id="45979-225">Click on hello audit records tooview more details on hello suspicious database activities such as SQL statement, failure reason and client IP.</span></span>

    ![Rekord részletei](./media/sql-database-security-tutorial/6_td_audit_record_details.png)

8. <span data-ttu-id="45979-227">Hello rekordok naplózása paneljén kattintson **Megnyitás az Excelben** előre konfigurált tooopen excel-sablon tooimport és futtatási mélyebb elemzésének hello audit napló hello gyanús esemény hello időpontja körül.</span><span class="sxs-lookup"><span data-stu-id="45979-227">In hello Auditing Records blade, click  **Open in Excel** tooopen a pre-configured excel template tooimport and run deeper analysis of hello audit log around hello time of hello suspicious event.</span></span>

    > [!NOTE]
    > <span data-ttu-id="45979-228">Az Excel 2010 vagy újabb, a Power Query és hello **gyors összevonás** beállításra akkor szükség.</span><span class="sxs-lookup"><span data-stu-id="45979-228">In Excel 2010 or later, Power Query and hello **Fast Combine** setting is required.</span></span>

    ![Nyitott rekordokat az Excel programban](./media/sql-database-threat-detection-get-started/7_td_audit_records_open_excel.png)

9. <span data-ttu-id="45979-230">tooconfigure hello **gyors összevonás** beállítás - hello **POWER QUERY** menüszalag lapon jelölje be **beállítások** toodisplay hello beállítások párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="45979-230">tooconfigure hello **Fast Combine** setting - In hello **POWER QUERY** ribbon tab, select **Options** toodisplay hello Options dialog.</span></span> <span data-ttu-id="45979-231">Jelölje ki hello adatvédelmi szakaszt, és válassza ki a hello második lehetőség - "Hello adatvédelmi szintek figyelmen kívül, és a teljesítmény lehetséges javítása":</span><span class="sxs-lookup"><span data-stu-id="45979-231">Select hello Privacy section and choose hello second option - 'Ignore hello Privacy Levels and potentially improve performance':</span></span>

    ![Gyors Excel egyesítése](./media/sql-database-threat-detection-get-started/8_td_excel_fast_combine.png)

10. <span data-ttu-id="45979-233">tooload SQL naplókat, győződjön meg arról, hogy hello paraméterek hello beállítások lapon helyesen van beállítva és majd hello "Data" menüszalagon válassza hello "Az összes frissítés" gombra.</span><span class="sxs-lookup"><span data-stu-id="45979-233">tooload SQL audit logs, ensure that hello parameters in hello settings tab are set correctly and then select hello 'Data' ribbon and click hello 'Refresh All' button.</span></span>

    ![Excel-paraméterek](./media/sql-database-threat-detection-get-started/9_td_excel_parameters.png)

11. <span data-ttu-id="45979-235">hello eredményei jelennek meg hello **SQL naplók** lap, amely mélyebb elemzése toorun hello rendellenes tevékenységet észlelt, és csökkenteni hello hatását hello biztonsági esemény az alkalmazás lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="45979-235">hello results appear in hello **SQL Audit Logs** sheet which enables you toorun deeper analysis of hello anomalous activities that were detected, and mitigate hello impact of hello security event in your application.</span></span>


## <a name="next-steps"></a><span data-ttu-id="45979-236">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="45979-236">Next steps</span></span>
<span data-ttu-id="45979-237">Az adatbázis rosszindulatú felhasználók vagy néhány egyszerű lépésben jogosulatlan hozzáférés elleni védelmének hello növelhető.</span><span class="sxs-lookup"><span data-stu-id="45979-237">You can improve hello protection of your database against malicious users or unauthorized access with just a few simple steps.</span></span> <span data-ttu-id="45979-238">Ebben az oktatóanyagban elsajátíthatja, hogy:</span><span class="sxs-lookup"><span data-stu-id="45979-238">In this tutorial you learn to:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="45979-239">A kiszolgáló és vagy az adatbázis vonatkozó tűzfalszabályok beállítása</span><span class="sxs-lookup"><span data-stu-id="45979-239">Set up firewall rules for your sever and or database</span></span>
> * <span data-ttu-id="45979-240">Csatlakozás a biztonságos kapcsolati karakterláncok használatával tooyour adatbázis</span><span class="sxs-lookup"><span data-stu-id="45979-240">Connect tooyour database using a secure connection string</span></span>
> * <span data-ttu-id="45979-241">Felhasználói hozzáférés kezelése</span><span class="sxs-lookup"><span data-stu-id="45979-241">Manage user access</span></span>
> * <span data-ttu-id="45979-242">A titkosított adatok védelme</span><span class="sxs-lookup"><span data-stu-id="45979-242">Protect your data with encryption</span></span>
> * <span data-ttu-id="45979-243">SQL-adatbázis naplózásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="45979-243">Enable SQL Database auditing</span></span>
> * <span data-ttu-id="45979-244">SQL-adatbázis fenyegetésészlelés engedélyezéséhez</span><span class="sxs-lookup"><span data-stu-id="45979-244">Enable SQL Database threat detection</span></span>

> [!div class="nextstepaction"]
>[<span data-ttu-id="45979-245">Az SQL Database teljesítményének növelése</span><span class="sxs-lookup"><span data-stu-id="45979-245">Improve SQL Database performance</span></span>](sql-database-performance-tutorial.md)

