
<!--
includes/sql-database-create-new-server-firewall-portal.md

Latest Freshness check:  2016-11-28 , rickbyh.

As of circa 2016-04-11, hello following topics might include this include:
articles/sql-database/sql-database-get-started.md
articles/sql-database/sql-database-configure-firewall-settings
articles/sql-data-warehouse-get-started-provision.md

-->
### <a name="create-a-server-level-firewall-rule-in-hello-azure-portal"></a>Hozzon létre egy kiszolgálószintű tűzfalszabályt hello Azure-portálon

1. Kattintson a hello SQL server panelen, a beállítások **tűzfal** tooopen hello tűzfal panel hello SQL Server.

    <!-- ![sql server firewall](../articles/sql-database/media/sql-database-get-started/sql-server-firewall.png) -->

2. Tekintse át a hello ügyfél IP-cím jelenik meg, és ellenőrizze, hogy ez az IP-cím a hello internetkapcsolat, és egy böngészőt, a (kérje meg, "Mi az az IP-cím). Időnként előfordulhat, hogy az IP-címek nem egyeznek.

    <!-- ![your IP address](../articles/sql-database/media/sql-database-get-started/your-ip-address.png) -->

3. Feltételezve, hogy hello IP-címek egyeznek, kattintson a **ügyfél IP-cím hozzáadása** hello eszköztáron.

    ![ügyfél IP-címének hozzáadása](../articles/sql-data-warehouse/media/sql-data-warehouse-get-started-provision/add-client-ip.png)

    > [!NOTE]
    > SQL-adatbázis tűzfala hello hello server tooa egyetlen IP-címet vagy egy teljes címtartományt nyithatja meg. Nyitó hello tűzfala lehetővé teszi, hogy az SQL-rendszergazdák és felhasználók toologin tooany adatbázis hello server toowhich rendelkeznek érvényes hitelesítő adatokat.
    >

4. Kattintson a **mentése** hello eszköztár toosave a kiszolgálószintű tűzfalszabály, majd **OK**.

    ![ügyfél IP-címének hozzáadása](../articles/sql-database/media/sql-database-get-started-portal/server-firewall-rule.png)

> [!Tip]
> Az oktatóanyagot lásd a [Kiszolgáló, kiszolgálószintű tűzfalszabály, mintaadatbázis és adatbázisszintű tűzfalszabály létrehozása, és csatlakozás az SQL Server Management Studióhoz](../articles/sql-database/sql-database-get-started.md) című témakörben.    
>
