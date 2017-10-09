
<!--
includes/sql-database-create-new-server-firewall-portal.md

Latest Freshness check:  2016-11-28 , rickbyh.

As of circa 2016-04-11, hello following topics might include this include:
articles/sql-database/sql-database-get-started.md
articles/sql-database/sql-database-configure-firewall-settings
articles/sql-data-warehouse-get-started-provision.md

-->
### <a name="create-a-server-level-firewall-rule-in-hello-azure-portal"></a><span data-ttu-id="c3e85-101">Hozzon létre egy kiszolgálószintű tűzfalszabályt hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c3e85-101">Create a server-level firewall rule in hello Azure portal</span></span>

1. <span data-ttu-id="c3e85-102">Kattintson a hello SQL server panelen, a beállítások **tűzfal** tooopen hello tűzfal panel hello SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c3e85-102">On hello SQL server blade, under Settings, click **Firewall** tooopen hello Firewall blade for hello SQL server.</span></span>

    <!-- ![sql server firewall](../articles/sql-database/media/sql-database-get-started/sql-server-firewall.png) -->

2. <span data-ttu-id="c3e85-103">Tekintse át a hello ügyfél IP-cím jelenik meg, és ellenőrizze, hogy ez az IP-cím a hello internetkapcsolat, és egy böngészőt, a (kérje meg, "Mi az az IP-cím).</span><span class="sxs-lookup"><span data-stu-id="c3e85-103">Review hello client IP address displayed and validate that this is your IP address on hello Internet using a browser of your choice (ask "what is my IP address).</span></span> <span data-ttu-id="c3e85-104">Időnként előfordulhat, hogy az IP-címek nem egyeznek.</span><span class="sxs-lookup"><span data-stu-id="c3e85-104">Occasionally they do not match for a various reasons.</span></span>

    <!-- ![your IP address](../articles/sql-database/media/sql-database-get-started/your-ip-address.png) -->

3. <span data-ttu-id="c3e85-105">Feltételezve, hogy hello IP-címek egyeznek, kattintson a **ügyfél IP-cím hozzáadása** hello eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="c3e85-105">Assuming that hello IP addresses match, click **Add client IP** on hello toolbar.</span></span>

    ![ügyfél IP-címének hozzáadása](../articles/sql-data-warehouse/media/sql-data-warehouse-get-started-provision/add-client-ip.png)

    > [!NOTE]
    > <span data-ttu-id="c3e85-107">SQL-adatbázis tűzfala hello hello server tooa egyetlen IP-címet vagy egy teljes címtartományt nyithatja meg.</span><span class="sxs-lookup"><span data-stu-id="c3e85-107">You can open hello SQL Database firewall on hello server tooa single IP address or an entire range of addresses.</span></span> <span data-ttu-id="c3e85-108">Nyitó hello tűzfala lehetővé teszi, hogy az SQL-rendszergazdák és felhasználók toologin tooany adatbázis hello server toowhich rendelkeznek érvényes hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="c3e85-108">Opening hello firewall enables SQL administrators and users toologin tooany database on hello server toowhich they have valid credentials.</span></span>
    >

4. <span data-ttu-id="c3e85-109">Kattintson a **mentése** hello eszköztár toosave a kiszolgálószintű tűzfalszabály, majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="c3e85-109">Click **Save** on hello toolbar toosave this server-level firewall rule and then click **OK**.</span></span>

    ![ügyfél IP-címének hozzáadása](../articles/sql-database/media/sql-database-get-started-portal/server-firewall-rule.png)

> [!Tip]
> <span data-ttu-id="c3e85-111">Az oktatóanyagot lásd a [Kiszolgáló, kiszolgálószintű tűzfalszabály, mintaadatbázis és adatbázisszintű tűzfalszabály létrehozása, és csatlakozás az SQL Server Management Studióhoz](../articles/sql-database/sql-database-get-started.md) című témakörben.</span><span class="sxs-lookup"><span data-stu-id="c3e85-111">For a tutorial, see [SQL Database tutorial: Create a server, a server-level firewall rule, a sample database, a database-level firewall rule and connect with SQL Server](../articles/sql-database/sql-database-get-started.md).</span></span>    
>
