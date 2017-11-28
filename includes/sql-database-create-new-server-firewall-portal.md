
<!--
includes/sql-database-create-new-server-firewall-portal.md

Latest Freshness check:  2016-11-28 , rickbyh.

As of circa 2016-04-11, the following topics might include this include:
articles/sql-database/sql-database-get-started.md
articles/sql-database/sql-database-configure-firewall-settings
articles/sql-data-warehouse-get-started-provision.md

-->
### <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a><span data-ttu-id="7fb55-101">Kiszolgálószintű tűzfalszabály létrehozása az Azure Portalon</span><span class="sxs-lookup"><span data-stu-id="7fb55-101">Create a server-level firewall rule in the Azure portal</span></span>

1. <span data-ttu-id="7fb55-102">Az SQL Server tűzfalához tartozó panel megnyitásához kattintson a Beállítások terület alatt található **Tűzfal** elemre az SQL Server paneljén.</span><span class="sxs-lookup"><span data-stu-id="7fb55-102">On the SQL server blade, under Settings, click **Firewall** to open the Firewall blade for the SQL server.</span></span>

    <!-- ![sql server firewall](../articles/sql-database/media/sql-database-get-started/sql-server-firewall.png) -->

2. <span data-ttu-id="7fb55-103">Ellenőrizze a megjelenő ügyfél IP-címet, és egy internetes böngészőben győződjön meg róla, hogy valóban ez az Ön által használt IP-cím (vannak olyan weboldalak, amelyek megadják ezt az információt).</span><span class="sxs-lookup"><span data-stu-id="7fb55-103">Review the client IP address displayed and validate that this is your IP address on the Internet using a browser of your choice (ask "what is my IP address).</span></span> <span data-ttu-id="7fb55-104">Időnként előfordulhat, hogy az IP-címek nem egyeznek.</span><span class="sxs-lookup"><span data-stu-id="7fb55-104">Occasionally they do not match for a various reasons.</span></span>

    <!-- ![your IP address](../articles/sql-database/media/sql-database-get-started/your-ip-address.png) -->

3. <span data-ttu-id="7fb55-105">Amennyiben az IP-címek egyeznek, kattintson az **Ügyfél IP-címének hozzáadása** elemre az eszköztárban.</span><span class="sxs-lookup"><span data-stu-id="7fb55-105">Assuming that the IP addresses match, click **Add client IP** on the toolbar.</span></span>

    ![ügyfél IP-címének hozzáadása](../articles/sql-data-warehouse/media/sql-data-warehouse-get-started-provision/add-client-ip.png)

    > [!NOTE]
    > <span data-ttu-id="7fb55-107">Az SQL Database-tűzfalat a kiszolgálón egyetlen IP-cím vagy egy teljes IP-címtartomány előtt is megnyithatja.</span><span class="sxs-lookup"><span data-stu-id="7fb55-107">You can open the SQL Database firewall on the server to a single IP address or an entire range of addresses.</span></span> <span data-ttu-id="7fb55-108">A tűzfal megnyitását követően az SQL-rendszergazdák és -felhasználók a kiszolgáló bármely olyan adatbázisába bejelentkezhetnek, amelyhez érvényes hitelesítő adatokkal rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="7fb55-108">Opening the firewall enables SQL administrators and users to login to any database on the server to which they have valid credentials.</span></span>
    >

4. <span data-ttu-id="7fb55-109">A kiszolgálószintű tűzfalszabály mentéséhez kattintson a **Mentés** gombra az eszköztárban, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="7fb55-109">Click **Save** on the toolbar to save this server-level firewall rule and then click **OK**.</span></span>

    ![ügyfél IP-címének hozzáadása](../articles/sql-database/media/sql-database-get-started-portal/server-firewall-rule.png)

> [!Tip]
> <span data-ttu-id="7fb55-111">Az oktatóanyagot lásd a [Kiszolgáló, kiszolgálószintű tűzfalszabály, mintaadatbázis és adatbázisszintű tűzfalszabály létrehozása, és csatlakozás az SQL Server Management Studióhoz](../articles/sql-database/sql-database-get-started.md) című témakörben.</span><span class="sxs-lookup"><span data-stu-id="7fb55-111">For a tutorial, see [SQL Database tutorial: Create a server, a server-level firewall rule, a sample database, a database-level firewall rule and connect with SQL Server](../articles/sql-database/sql-database-get-started.md).</span></span>    
>
