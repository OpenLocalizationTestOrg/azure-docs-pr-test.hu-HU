
<!--
includes/sql-database-include-ip-address-22-v12portal.md

Latest Freshness check:  2016-03-21 , daleche.

As of circa 2015-09-04, hello following topics might include this include:
articles/sql-database/sql-database-configure-firewall-settings.md
articles/sql-database/sql-database-connect-query.md


## Server-level firewall rules

### Add a server-level firewall rule through hello new Azure portal
-->


1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/) , http://portal.azure.com/.
2. Kattintson a bal oldali szalagcím hello, **összes TALLÓZÁSA**. Hello **Tallózás** panel jelenik meg.
3. Görgetéssel **SQL Server-kiszolgálók**. Hello **SQL Server-kiszolgálók** panel jelenik meg.
   
    ![Az Azure SQL Database-kiszolgálóhoz az hello portálon található][b21-FindServerInPortal]
4. Kényelmi okokból kattintson hello minimalizálása érdekében a korábbi hello levő **Tallózás** panelen.
5. A hello szűrő szövegmezőbe írja be a kiszolgáló nevének hello elindításához. A sor jelenik meg.
6. Kattintson a kiszolgáló hello sor. A kiszolgáló egy panel jelenik meg.
7. A kiszolgáló paneljén kattintson **beállítások**. Hello **beállítások** panel jelenik meg.
8. Kattintson a **tűzfal**. Hello **tűzfalbeállítások** panel jelenik meg.
   
    ![Kattintson a beállítások > tűzfal][b31-SettingsFirewallNavig]
9. Kattintson a **adja hozzá ügyfél IP**. Adjon meg egy nevet az új szabály hello első szövegmezőbe írja be.
10. Hello alacsony és magas írja be az IP-cím kívánt hello tartomány értékeinek tooenable.
    
    * Hasznos toohave hello alacsony értékre end azok **.0** és magas hello **.255**.
    
    ![Az IP-cím tartomány tooallow hozzáadása][b41-AddRange]
11. Kattintson a **Save** (Mentés) gombra.

<!-- Image references. -->

[b21-FindServerInPortal]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b21-v12portal-findsvr.png

[b31-SettingsFirewallNavig]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b31-v12portal-settingsfirewall.png

[b41-AddRange]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b41-v12portal-addrange.png



<!--
These includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-ip-address-22-v12portal.md
? includes/sql-database-include-ip-address-*.md
-->
