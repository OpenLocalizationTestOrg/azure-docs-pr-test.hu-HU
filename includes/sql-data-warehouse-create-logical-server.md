### <a name="create-a-new-logical-sql-server-in-hello-azure-portal"></a>Hozzon létre egy új logikai SQL server hello Azure-portálon

1. Kattintson az **Új** gombra, keresse meg a **logikai kiszolgáló** lehetőséget, majd nyomja le az **ENTER** billentyűt.

    ![logikai kiszolgáló keresése](./media/sql-data-warehouse-create-logical-server/search-logical-server.png)
2. Válassza az **SQL Server (logikai kiszolgáló)** elemet. 

    ![logikai kiszolgáló kiválasztása](./media/sql-data-warehouse-create-logical-server/select-logical-server.png)
  
3. Kattintson a **létrehozása** tooopen hello új SQL-kiszolgáló (logikai kiszolgáló) panelen.

   <kbd>![nyissa meg a logikai kiszolgáló panelt](./media/sql-data-warehouse-create-logical-server/open-logical-server-blade.png) </kbd> <kbd> ![logikai kiszolgálójuk paneljéről](./media/sql-data-warehouse-create-logical-server/logical-server-blade.png)</kbd>
  
3. Hello SQL Server (a logikai kiszolgáló) panelen kiszolgáló neve mezőbe adjon meg egy érvényes nevet a hello új logikai kiszolgáló. Zöld pipa jelzi, hogy érvényes nevet adott meg.
    
    ![új kiszolgáló neve](./media/sql-data-warehouse-create-logical-server/new-name-logical-server.png)

    > [!IMPORTANT]
    > hello az új kiszolgáló teljesen minősített név lesz < your_server_name >. database.windows.net.
    >
    
4. Hello kiszolgáló rendszergazdai bejelentkezés szövegmezőben adjon meg egy SQL-hitelesítési bejelentkezési hello az ehhez a kiszolgálóhoz. Ehhez a bejelentkezéshez hello kiszolgáló fő bejelentkezéssel néven ismert. Zöld pipa jelzi, hogy érvényes nevet adott meg.
    
    ![SQL-rendszergazda felhasználóneve](./media/sql-data-warehouse-create-logical-server/sql-admin-login.png)
5. A hello **jelszó** és **jelszó megerősítése** szövegmezőkben hello server egyszerű bejelentkezési fiók adjon meg egy jelszót. Zöld pipa jelzi, hogy érvényes jelszót adott meg.
    
    ![SQL-rendszergazda jelszava](./media/sql-data-warehouse-create-logical-server/sql-admin-password.png)
6. Válasszon egy előfizetést, amelyben engedély toocreate objektummal rendelkezik.

    ![előfizetést](./media/sql-data-warehouse-create-logical-server/subscription.png)
7. Hello erőforrás csoport szövegmezőben, válassza ki a **hozzon létre új** , hello erőforrás csoport szövegmezőben adja meg egy érvényes nevet az hello új erőforráscsoport (is használhat egy meglévő erőforráscsoportot, ha már létrehozott egy szolgáltatást). Zöld pipa jelzi, hogy érvényes nevet adott meg.

    ![új erőforráscsoport](./media/sql-data-warehouse-create-logical-server/new-resource-group.png)

8. A hello **hely** szövegmezőben, válasszon ki egy adattípust center megfelelő tooyour helyre – például "Kelet-Ausztrália".
    
    ![kiszolgáló helye](./media/sql-data-warehouse-create-logical-server/server-location.png)
    
    > [!TIP]
    > hello jelölőnégyzetét **engedélyezése az azure szolgáltatások tooaccess server** nem módosítható ezen a panelen. Ez a beállítás a hello kiszolgáló tűzfal panelen módosíthatja. További információkért lásd a [biztonságos használat első lépéseit](../articles/sql-database/sql-database-manage-servers-portal.md) ismertető témakört.
    >
    
9. Kattintson a **Létrehozás** gombra.

    ![létrehozás gomb](./media/sql-data-warehouse-create-logical-server/create.png)

