
1. Kattintson a hello **alkalmazásszolgáltatások** gombra, válassza ki a Mobile Apps háttér, válassza ki **gyors üzembe helyezés**, majd válassza ki az ügyfél-platform (iOS, Android, Xamarin, Cordova).

    ![Azure Portal a kiemelt Mobile Apps Quickstarttal][quickstart]

2. Ha az adatbázis-kapcsolat nincs konfigurálva, hozzon létre egyet hello következő tevékenységek végrehajtásával:

    ![Azure Mobile Apps Connect toodatabase-portálon][connect]

    a. Hozzon létre egy új SQL-adatbázist és -kiszolgálót.

    ![Azure Portal Mobile Appsszel – új adatbázis és kiszolgáló létrehozása][server]

    b. Várjon, amíg hello adatkapcsolat sikeresen létrehozva.

    ![Az Azure Portal értesítése az adatkapcsolat sikeres létrejöttéről][notification]

    c. Az adatkapcsolatnak sikeresnek kell lennie.

    ![Az Azure Portal értesítése: „Már van adatkapcsolata”][already-connection]

3. A **2. Tábla API létrehozása** elemnél a **Háttérrendszer nyelveként** válassza a Node.js-t. 
 
4. Fogadja el a hello visszaigazolás, és válassza ki **TodoItem tábla létrehozása**.  
    Ez a művelet létrehoz egy új teendőtáblát az adatbázisban. 

    >[!IMPORTANT]
    > Egy meglévő háttér tooNode.js váltás felülírja az összes tartalmát. a .NET háttérből helyett, lásd: toocreate [használható az SDK hello .NET háttér-kiszolgáló a Mobile Apps][instructions].

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png
[connect]: ./media/app-service-mobile-configure-new-backend/connect-to-bd.png
[notification]: ./media/app-service-mobile-configure-new-backend/notification-data-connection-create.png
[server]: ./media/app-service-mobile-configure-new-backend/create-new-server.png
[already-connection]: ./media/app-service-mobile-configure-new-backend/already-connection.png

<!-- URLs -->
[instructions]: ../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app
