1. Jelentkezzen be hello [Azure Portal].
2. Kattintson az **+ÚJ** > **Web + mobil** > **Mobile App** elemre, majd adjon meg egy nevet a mobilalkalmazás háttérrendszerének.
3. A hello **erőforráscsoport**, válasszon ki egy meglévő erőforráscsoportot, vagy hozzon létre egy újat (a neve megegyezik az alkalmazás használatával hello.) 
   
    Kiválaszthat egy másik App Service-csomagot, vagy újat is létrehozhat. További információk az App Service szolgáltatások terveket, és hogyan toocreate egy új csomagot egy másik tarifacsomagban réteg, majd a kívánt helyre, a következő látható: [Azure App Service-csomagok részletes áttekintése](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
4. A hello **App Service-csomag**, hello alapértelmezett terv (a hello [Standard csomagra](https://azure.microsoft.com/pricing/details/app-service/)) van kiválasztva. Igény szerint kiválaszthatja egy másik csomagot, vagy [hozzon létre egy újat](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). hello App Service-csomag beállításai határozzák meg hello [helyet, szolgáltatásokat, költségeket és számítási erőforrásokat](https://azure.microsoft.com/pricing/details/app-service/) az alkalmazáshoz társított. 
   
    A hello terv kiválasztása után kattintson **létrehozása**. Ez a Mobile Apps-háttéralkalmazás hello hoz létre. 
5. A hello **beállítások** hello új mobil-háttéralkalmazást, paneljén kattintson **gyors üzembe helyezési** > saját ügyfélalkalmazás-platformjára > **adatbázis csatlakoztatása**. 
   
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)
6. A hello **adatkapcsolat hozzáadása** panelen kattintson a **SQL-adatbázis** > **hozzon létre egy új adatbázist**, típus hello adatbázis **neve**, Válasszon egy tarifacsomagot, majd kattintson az **Server**.  Ezt az új adatbázist többször is felhasználhatja. Ha már rendelkezik adatbázissal hello ugyanazon a helyen, választhatja **meglévő adatbázis használata**. egy adatbázis egy másik helyen lévő hello használata toobandwidth költségekkel és nagyobb késleltetéssel járhat miatt nem ajánlott.
   
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)
7. Hello a **új kiszolgáló** panelen írja be egy egyedi kiszolgálónevet hello **kiszolgálónév** mezőbe, adjon meg egy felhasználónevet és jelszót, ellenőrizze **engedélyezése az azure szolgáltatások tooaccess server**, és kattintson **OK**. Ez a hello új adatbázist hoz létre.
8. Vissza a hello **adatkapcsolat hozzáadása** panelen kattintson a **kapcsolati karakterlánc**, írja be az adatbázis hello felhasználónevet és jelszót értékeket, és kattintson **OK**. Várjon néhány percig, hello adatbázis toobe sikeresen telepített, a folytatás előtt.

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/
