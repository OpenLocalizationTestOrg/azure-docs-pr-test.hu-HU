---
title: az Azure App Service .NET webjobs-feladat aaaCreate |} Microsoft Docs
description: "Hozzon létre egy többrétegű alkalmazást ASP.NET MVC és az Azure használatával. hello első end fut egy webalkalmazást az Azure App Service és hello a webjobs-feladat befejezési futtatása gépet. hello app Entity Framework, SQL-adatbázis, és az Azure storage várólisták és blobokat használ."
services: app-service
documentationcenter: .net
author: tdykstra
manager: erikre
editor: mollybos
ms.assetid: 99cb9917-483a-45f8-a98d-07d19c68c753
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 6/14/2017
ms.author: glenga
ms.openlocfilehash: d92fc4b81cc0d58f8e900f257e747af56d32b911
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-net-webjob-in-azure-app-service"></a>.NET WebJobs-feladat létrehozása az Azure App Service-ben
Ez az oktatóanyag bemutatja, hogyan toowrite kód egy egyszerű többrétegű ASP.NET MVC 5 alkalmazás esetén hello [WebJobs SDK](websites-dotnet-webjobs-sdk.md).

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

hello célját hello [WebJobs SDK](websites-webjobs-resources.md) közös elvégezhető webjobs-feladat, például képek feldolgozását, a várólista feldolgozása, a RSS összesítési, a fájlkarbantartás, írt toosimplify hello kódot, és e-mailek küldésekor. hello WebJobs SDK az Azure Storage és a Service Bus, a feladatütemezés és a hibák kezelése és sok más szabhatják beépített szolgáltatásai rendelkezik. Emellett rendelkezik bővíthető toobe készült, és nincs olyan [nyílt forráskódú extensions tárháza](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).

hello mintaalkalmazás egy hirdetőtábla. Felhasználók feltöltheti ads lemezképeket, és a háttér folyamat hello képek toothumbnails alakítja. hello ad a lista lapon látható hello miniatűröket, és hello ad részleteit megjelenítő oldalra hello teljes méretű képet jeleníti meg. Íme egy képernyőkép:

![Hirdetéslista](./media/websites-dotnet-webjobs-sdk-get-started/list.png)

A mintaalkalmazás együttműködve [Azure várólisták](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) és [Azure-blobokat](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage). hello az oktatóanyag bemutatja, hogyan a toodeploy hello alkalmazás túl[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) és [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279).

## <a id="prerequisites"></a>Előfeltételek
hello oktatóanyag feltételezi, hogy ismeri hogyan rendelkező toowork [ASP.NET MVC 5](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) Visual studióban.

hello oktatóanyag eredetileg készült Visual Studio 2013, de a Visual Studio újabb verzióival használható. Ha a Visual Studio 2015-öt vagy 2017 használ, jegyezze fel, hogy mielőtt hello alkalmazás helyi futtatásához, meg kell változtatnia hello `Data Source` hello SQL Server LocalDB kapcsolati karakterláncot a hello Web.config és App.config fájlok egy részének `Data Source=(localdb)\v11.0` túl`Data Source=(LocalDb)\MSSQLLocalDB`.

> [!NOTE]
> <a name="note"></a>Ez az oktatóanyag egy Azure-fiók toocomplete kell rendelkeznie:
>
> * Is [nyissa meg az Azure-fiók szabad](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F): kapott kreditek, hogy használhatja ki tootry fizetős Azure-szolgáltatásokat, és még azok lejárta után, megtarthatja hello fiókot, és ingyenes, például a webhelyek szolgáltatásokat használja. A hitelkártya soha nem lesz felszámítva, kivéve, ha explicit módon a beállítások módosításához és kérje meg a toobe számítjuk fel.
> * Is [aktiválása havi Azure-kredit a Visual Studio-előfizetők](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): az előfizetés biztosít Önnek krediteket minden hónapban, fizetős Azure-szolgáltatásokat is használhat.
>
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
>
>

## <a id="learn"></a>Ismertetett témák
hello az oktatóanyag bemutatja, hogyan toodo hello a következő feladatokat:

* Engedélyezze a számítógépen az Azure fejlesztési hello Azure SDK telepítése (csak a Visual Studio 2013 és a felhasználók 2015).
* Hozzon létre egy konzolalkalmazás-projektet, amely automatikusan telepíti az Azure webjobs-feladatot, amikor hello társított webes projekt telepítése.
* A WebJobs SDK háttérrendszer helyi számítógépen hello fejlesztési tesztelése.
* A webjobs-feladatok háttér tooa webalkalmazás alkalmazás közzététele az App Service-ben.
* Fájlok feltöltése, és tárolja őket a hello Azure Blob szolgáltatásban.
* Hello Azure WebJobs SDK toowork használata Azure Storage üzenetsorokat és blobokat.

## <a id="contosoads"></a>Alkalmazás-architektúra
hello mintaalkalmazást használ hello [várólista-központú](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) toooff-betöltési hello processzorigényes munkáján miniatűrök tooa háttér folyamat létrehozása.

hello alkalmazást használó Entity Framework Code First toocreate hello táblák és -hozzáférési hello adatok SQL-adatbázisban tárolja a hirdetéseket. Az egyes hirdetések hello adatbázis két URL-címek tárolja: hello teljes méretű képhez, egyet, hello miniatűr egy.

![Hirdetés tábla](./media/websites-dotnet-webjobs-sdk-get-started/adtable.png)

Amikor egy felhasználó feltölt egy képet, hello webalkalmazás hello-lemezképet tárolja egy [Azure blob](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), és hello ad adatokat toohello blob URL-CÍMMEL rendelkező hello adatbázisban tárolja. At hello azonos időben, egy üzenet tooan Azure üzenetsor-kezelési ír. Az Azure webjobs-feladat futtató háttérkiszolgáló folyamatban hello WebJobs SDK kérdezze le az új üzenetek hello várólista. Egy új üzenet jelenik meg, amikor hello webjobs-feladat létrehoz egy kép miniatűrjét, és a frissítések hello Miniatűr URL-adatbázis mező az adott Active Directory. Íme egy diagram bemutatja, hogyan működnek együtt hello alkalmazás hello részei:

![Contoso Ads architektúra](./media/websites-dotnet-webjobs-sdk-get-started/apparchitecture.png)

[!INCLUDE [install-sdk](../../includes/install-sdk-2017-2015-2013.md)]  
hello oktatóanyagban szereplő utasítások alkalmazása tooAzure SDK for .NET 2.7.1-es vagy újabb.

## <a id="storage"></a>Egy Azure Storage-fiók létrehozása
Azure-tárfiók forrásokat biztosít hello felhőben üzenetsor és a blob adatainak tárolásához. Azt is használják hello WebJobs SDK toostore naplózási adatok hello irányítópult.

Egy valós alkalmazás általában létrehozhat külön az alkalmazásadatok és a naplózási adatok és külön fiókok a Tesztadatok és termelési adatok. Ebben az oktatóanyagban csak egy fiókot fog használni.

1. Nyissa meg hello **Server Explorer** ablak a Visual Studióban.
2. Kattintson a jobb gombbal hello **Azure** csomópontra, majd **csatlakozás Azure-előfizetés tooMicrosoft...** .
   
   ![Csatlakozás tooAzure](./media/websites-dotnet-webjobs-sdk-get-started/connaz.png)

3. Jelentkezzen be Azure hitelesítő adataival.
4. Kattintson a jobb gombbal **tárolási** hello Azure csomópont, és kattintson a **Storage-fiók létrehozása**.
   
   ![Storage-fiók létrehozása](./media/websites-dotnet-webjobs-sdk-get-started/createstor.png)
   
5. A hello **Storage-fiók létrehozása** párbeszédpanelen adja meg hello storage-fiók nevét.

    hello nevének meg kell egyedinek kell lennie (egyéb Azure storage-fiók hello rendelkezhet ugyanazzal a névvel). Ha hello név már használatban van, kap egy alkalommal toochange azt.

    a tárfiók lesz URL-cím tooaccess hello *{name}*. core.windows.net.
6. Set hello **régiót vagy Affinitáscsoportot** legördülő lista toohello régió legközelebbi tooyou.

    A beállítással megadható, hogy melyik Azure-adatközpontban fogja futtatni a tárfiók. Ebben az oktatóanyagban a választott érdekében nem érzékelhető különbség. Azonban éles web app alkalmazásban keresi a webkiszolgáló és a tárolási fiók toobe hello az ugyanabban a régióban toominimize késést és az adatok kilépési díjakat. hello web app (amely hoz létre, később) datacenter kell lennie, mint a lehető toohello böngészők hello webalkalmazás elérése a rendelés toominimize késés bezárásához.
7. Set hello **replikációs** legördülő lista túl**helyileg redundáns**.

    A georeplikáció engedélyezve van a tárfiók, hello tárolt tartalom esetén meg kell replikált tooa másodlagos adatközpontba tooenable feladatátvételi toothat helyévé hello elsődleges helyen jelentős katasztrófa következik. A georeplikáció további költségeket vonhat maga után. A vizsgálati és fejlesztői fiókok esetében általában nem érdemes toopay a georeplikációért. További információ: [Tárfiók létrehozása, kezelése vagy törlése](../storage/common/storage-create-storage-account.md).
8. Kattintson a **Create** (Létrehozás) gombra.

    ![Új tárfiók](./media/websites-dotnet-webjobs-sdk-get-started/newstorage.png)

## <a id="download"></a>Hello alkalmazás letöltése
1. Töltse le és csomagolja ki a hello [kész megoldást](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb).
2. Indítsa el a Visual Studiót.
3. A hello **fájl** menüben válassza **nyissa meg a > Projekt/megoldás**, és keresse meg a letöltött hello megoldás toowhere, majd nyissa meg a megoldásfájlt hello.
4. Nyomja le a CTRL + SHIFT + B toobuild hello megoldás.

    Alapértelmezés szerint a Visual Studio automatikusan visszaállítja hello NuGet-csomag tartalmát, amely nem szerepel a hello *.zip* fájlt. Hello csomagok nem állnak vissza, ha a telepítést, manuálisan is toohello **NuGet-csomagok kezelése megoldáshoz** párbeszédpanel, kattintson a hello **visszaállítása** hello felső jobb oldali gomb.
5. A **Megoldáskezelőben**, győződjön meg arról, hogy **ContosoAdsWeb** hello indítási projekt legyen kijelölve.

## <a id="configurestorage"></a>A tárfiók hello alkalmazás toouse konfigurálása
1. Nyissa meg a hello alkalmazás *Web.config* fájl hello ContosoAdsWeb projektben.

    SQL kapcsolati karakterlánc és az Azure tárolási kapcsolati karakterlánc blobokat és üzenetsorokat való munkához hello-fájl tartalmazza.

    SQL kapcsolati karakterlánc hello mutat tooa [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) adatbázis.

    hello tárolási kapcsolati karakterlánc, amely rendelkezik a tárolási fiók nevét és hívóbetűjét hello helyőrzőit példa. Cseréli le ez egy olyan kapcsolati karakterlánccal, amely rendelkezik hello nevét és a tárfiók kulcsa.  

    ```
    <connectionStrings>
        <add name="ContosoAdsContext" connectionString="Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
        <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
    </connectionStrings>
    ```
    hello tárolási kapcsolati karakterlánc neve AzureWebJobsStorage hello neve hello WebJobs SDK használja, mert alapértelmezés szerint. hello ugyanazzal a névvel itt van használatban, így hello Azure környezetben vannak tooset csak egy kapcsolati karakterláncot.
2. A **Server Explorer**, kattintson a jobb gombbal a tárfiókba hello **tárolási** csomópontra, majd **tulajdonságok**.

    ![Kattintson a Tárfiók tulajdonságai](./media/websites-dotnet-webjobs-sdk-get-started/storppty.png)
3. A hello **tulajdonságok** ablak, kattintson a **Tárfiókkulcsok**, és kattintson a hello három pont.

    ![Tárfiókkulcsok](./media/websites-dotnet-webjobs-sdk-get-started/stor-account-keys.png)
4. Másolás hello **kapcsolati karakterlánc**.

    ![Tároló kulcsait párbeszédpanel](./media/websites-dotnet-webjobs-sdk-get-started/cpak.png)
5. Cserélje le a hello tárolási kapcsolat karakterláncát a hello *Web.config* imént kimásolt kapcsolati karakterláncot hello fájlt. Gondoskodjon róla, hogy minden hello idézőjelek közötti, de nem tartalmazza a beillesztés előtt hello idézőjelek között.
6. Nyissa meg hello *App.config* fájl hello ContosoAdsWebJob projektben.

    Ez a fájl két tárolási kapcsolati karakterlánc, az alkalmazásadatok egyet, majd egy naplózási rendelkezik. Külön tárfiókok használata alkalmazásadatok és a naplózás, és használhatja [több tárfiókot adatok](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs). Ebben az oktatóanyagban egy tárfiókot fogja használni. hello kapcsolati karakterláncok hello tárfiókkulcsok helyőrzőit rendelkezik.

    ```
    <configuration>
        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="ContosoAdsContext" connectionString="Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;"/>
    </connectionStrings>
        <startup>
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
    </startup>
    </configuration>

    ```

    Alapértelmezés szerint hello WebJobs SDK AzureWebJobsStorage és AzureWebJobsDashboard nevű kapcsolati karakterláncok keresi. Alternatív megoldásként is [hello kapcsolati karakterláncot tárolni szeretné, és adja át azt kifejezetten toohello azonban `JobHost` objektum](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#config).
7. Cserélje le mindkét tárolási kapcsolati karakterláncok korábban kimásolt hello kapcsolati karakterlánccal.
8. Mentse a módosításokat.

## <a id="run"></a>Hello alkalmazás helyileg történő futtatása
1. toostart hello webes előtér hello alkalmazás, nyomja le a CTRL + F5 billentyűkombinációt.

    hello alapértelmezett böngésző megnyitja toohello kezdőlapján. (hello webes projekt hajtott végre, mert fut hello kezdőprojektként.)

    ![Contoso Ads kezdőlap](./media/websites-dotnet-webjobs-sdk-get-started/home.png)
2. toostart hello webjobs-feladat háttér hello alkalmazás, kattintson a jobb gombbal a hello ContosoAdsWebJob projektre a **Megoldáskezelőben**, és kattintson a **Debug** > **Start új példányt** .

    Egy alkalmazás konzolablak megnyílik, és jelző hello WebJobs SDK JobHost objektum megkezdődött toorun naplózási üzeneteket jelenít meg.

    ![Megjeleníti az adott hello háttér alkalmazás konzolablak fut.](./media/websites-dotnet-webjobs-sdk-get-started/backendrunning.png)
3. A böngészőben kattintson **hoznak létre hirdetéseket**.
4. Adjon meg néhány tesztadatot, egy kép tooupload válassza ki, majd kattintson a **létrehozása**.

    ![Lap létrehozása](./media/websites-dotnet-webjobs-sdk-get-started/create.png)

    hello app toohello Index lapra kerül, de hello új hirdetés miniatűrje nem jelenik meg, mert a feldolgozása még nem történt meg.

    Eközben a rövid várakozás után naplózási megjelenik egy üzenet hello alkalmazás konzolablakban, hogy a várólista üzenet érkezett, de feldolgozása megtörtént.

    ![Megjeleníti, hogy a várólista-üzenet feldolgozása alkalmazás konzolablak](./media/websites-dotnet-webjobs-sdk-get-started/backendlogs.png)
5. Miután meggyőződött arról, hogy a naplózási üzenetek hello alkalmazás konzolablakban hello, hello Index lap toosee hello miniatűrök frissítése.

    ![Index lap](./media/websites-dotnet-webjobs-sdk-get-started/list.png)
6. Kattintson a **részletek** a ad toosee hello teljes méretű kép.

    ![Részletek lap](./media/websites-dotnet-webjobs-sdk-get-started/details.png)

Ön már futott hello alkalmazás a helyi számítógépen, és egy SQL Server adatbázis található a számítógépen, de az üzenetsorok működik, és a blobok használ hello felhő. A következő szakasz hello fogja futtatni hello alkalmazás hello felhőben felhő adatbázis, valamint a felhő blobokat és üzenetsorokat használ.  

## <a id="runincloud"></a>Hello felhő hello alkalmazást futtat
El kell végeznie a következő lépéseket toorun hello alkalmazás hello felhőben hello:

* TooWeb alkalmazások telepítése. A Visual Studio automatikusan létrehoz egy új webalkalmazást az App Service és az SQL Database-példányt.
* Hello web app toouse Azure SQL adatbázis és a tárolási fiók beállítása.

Miután létrehozta a hirdetések hello felhőben futtatása során, megtekintheti lesz hello WebJobs SDK irányítópult toosee hello gazdag figyelési funkciókat toooffer rendelkezik.

### <a name="deploy-tooweb-apps"></a>TooWeb alkalmazások telepítése

1. Hello böngésző és hello console application ablak bezárása.
2. Hello kövesse hello [közzététele az SQL Database tooAzure](https://docs.microsoft.com/azure/app-service-web/app-service-web-tutorial-dotnet-sqldatabase#publish-to-azure-with-sql-database) szakasz.
3. Hello központi telepítésének lépései befejezése után folytassa a hátralévő feladatok hello ebben a cikkben.

### <a name="configure-hello-web-app-toouse-your-azure-sql-database-and-storage-account"></a>Hello web app toouse Azure SQL adatbázis és a tárolási fiók beállítása
Biztonsági szempontból ajánlott túl[ne tegye a bizalmas adatokat például kapcsolati karakterláncok forráskódú adattárakban tárolt fájlok](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets). Azure tartalmaz egy módja toodo, amely: kapcsolati karakterlánc és más beállításértékek beállítható hello Azure-környezetéhez, és az ASP.NET konfigurációs API-k automatikusan átvételéhez ezeket az értékeket, az Azure-ban hello alkalmazás futtatásakor. Beállíthatja ezeket az értékeket az Azure-ban használatával **Server Explorer**, hello Azure portálon, a Windows PowerShell vagy a platformfüggetlen parancssori felületre hello. További információkért lásd: [hogyan alkalmazás karakterláncok és a kapcsolati karakterláncok munkahelyi](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).

Ebben a szakaszban használhatja **Server Explorer** tooset kapcsolati karakterlánc-értékek az Azure-ban.

1. A **Server Explorer**, kattintson a jobb gombbal a web app alatt **Azure > az App Service > {az erőforráscsoport}**, és kattintson a **nézetbeállítások**.

    Hello **Azure Web Apps** ablak nyílik meg a hello **konfigurációs** fülre.
2. Hello nevének módosítása hello DefaultConnection kapcsolati karakterlánc toohello nevű úgy döntött, hogy mikor konfigurált SQL-adatbázis hello hello [közzététele az SQL Database tooAzure](https://docs.microsoft.com/azure/app-service-web/app-service-web-tutorial-dotnet-sqldatabase#publish-to-azure-with-sql-database) cikk.

    Azure automatikusan létrejön, ez a kapcsolati karakterlánc létrehozott hello webalkalmazáshoz társított adatbázissal, így már hello megfelelő kapcsolati karakterlánc. Csak hello neve toowhat keres a kód módosítása.
3. Adjon hozzá két új kapcsolati karakterláncokat AzureWebJobsStorage és AzureWebJobsDashboard nevű. Hello adatbázis típusának beállítása túl**egyéni**, és a set hello kapcsolati karakterlánc értékét toohello azonos értéket használta korábban a hello *Web.config* és *App.config* fájlokat. (Ügyeljen hello teljes kapcsolati karakterlánc, nem csak a hello elérési kulcsot tartalmaz, és nem jelennek meg a hello idézőjelek között.)

    A kapcsolati karakterláncok hello WebJobs SDK, az alkalmazásadatok egy és egy, a naplózás által használt. Mivel a korábban hello az alkalmazásadatok is használják hello webes előtér-kódját.
4. Kattintson a **Save** (Mentés) gombra.

    ![Kapcsolati karakterláncok az Azure portálon](./media/websites-dotnet-webjobs-sdk-get-started/azconnstr.png)
5. A **Server Explorer**, kattintson a jobb gombbal a hello webalkalmazást, és kattintson a **leállítása**.
6. Miután hello webes alkalmazás leáll, kattintson ismét a jobb gombbal hello webalkalmazás, és kattintson **Start**.

   hello webjobs-feladat automatikusan elindul, amikor a tesz közzé, de az megáll, amikor olyan konfigurációs módosítást hajt végre. toorestart, hello a webalkalmazás újraindítása, vagy indítsa újra a webjobs-feladat hello hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715). A konfiguráció módosítása után általában ajánlott toorestart hello webalkalmazás.
7. Frissítse a hello böngészőablakban, amely rendelkezik hello webes alkalmazás URL-CÍMÉT a böngésző címsorába.

    hello kezdőlap jelenik meg.
8. Hozzon létre egy Active Directory, úgy, ahogy mikor meg [hello alkalmazás helyileg futtatta](https://docs.microsoft.com/azure/app-service-web/websites-dotnet-webjobs-sdk-get-started#a-idrunarun-the-application-locally).

   hello Index lapot a miniatűr először nélkül jeleníti meg.
9. Néhány másodperc múlva frissítse a hello lapot, és hello miniatűr jelenik meg.

   Hello miniatűrje nem jelenik meg, ha lehetséges, hogy toowait egy percet a hello webjobs-feladat toorestart. Után igénybe, ha még nem látja hello miniatűr hello lapon hello webjobs-feladat frissítésekor előfordulhat, hogy nem indult el automatikusan. Ebben az esetben lépjen toohello **alkalmazásszolgáltatások** hello paneljén [Azure-portálon](https://portal.azure.com/), keresse meg a webes alkalmazást, és kattintson **Start**.

### <a name="view-hello-webjobs-sdk-dashboard"></a>Nézet hello WebJobs SDK irányítópult
1. A hello [Azure-portálon](https://portal.azure.com/), jelölje be hello **alkalmazásszolgáltatások panel**, keresse meg a webes alkalmazást, és válassza ki **WebJobs**.
3. Jelölje be hello **naplók** fülre.

    ![Naplófájlok lap](./media/websites-dotnet-webjobs-sdk-get-started/log-tab.png)

    Egy új böngészőlapon toohello WebJobs SDK irányítópult megnyitása. hello irányítópult adott webjobs-feladat fut, és a kód funkciók listáját láthatja, hogy hello indított WebJobs SDK hello jeleníti meg.
4. Kattintson az egyik hello funkciók toosee adatait a végrehajtása.

    ![WebJobs SDK irányítópult](./media/websites-dotnet-webjobs-sdk-get-started/wjdashboardhome.png)

    ![WebJobs SDK irányítópult](./media/websites-dotnet-webjobs-sdk-get-started/wjfunctiondetails.png)

    Hello **ismétlési függvény** ezen az oldalon gomb hatására a WebJobs SDK hello keretrendszer toocall hello függvény újra, és biztosít egy alkalommal toochange hello átadott adatok toohello függvény első.

> [!NOTE]
> Ha végzett tesztelése során, fontolja meg törlését hello webalkalmazás, tárfiókot és az SQL Database-példányt. hello webalkalmazás szabad, de hello SQL tárfiók és adatbázispéldány keletkeznek költségek (ámbár, minimális toohello kis méret miatt). Is ha nem adja meg hello a webalkalmazás fut, bárki, aki rátalál az URL-cím létrehozhat és megtekinthet hirdetéseket. 
>
>

### <a name="delete-your-web-app"></a>A webalkalmazás törlése
Hello portálon lépjen toohello **alkalmazásszolgáltatások** panelen keresse meg és jelölje ki a webalkalmazás, és kattintson a **törlése**. Ha csak tootemporarily megakadályozni, hogy mások hello webalkalmazás-hozzáférése, kattintson a **leállítása** helyette. Ebben az esetben díjakat továbbra is az SQL-adatbázis és a Tárfiók hello tooaccrue.

### <a name="delete-your-storage-account"></a>A tárfiók törlése
toodelete a tárfiók, lásd: [tárfiók törlése](https://docs.microsoft.com/azure/storage/storage-create-storage-account#delete-a-storage-account). 

### <a name="delete-your-database"></a>Az adatbázis törlése
toodelete az SQL-adatbázis, lásd: hello [Azure SQL Database REST API](https://docs.microsoft.com/rest/api/sql/) dokumentációját.

## <a id="create"></a>Teljesen új hello alkalmazás létrehozása
Ebben a szakaszban el kell végeznie hello a következő feladatokat:

* Hozzon létre webes projektet a Visual Studio megoldás.
* Adja hozzá a hello adatok hordozhatóosztálytár-projektjének hello előtér- és a háttérrendszer között megosztott hozzáférési réteg.
* Vegyen fel egy konzolalkalmazás projekt hello háttérkiszolgáló tartalmazó engedélyezve van a webjobs-feladatok telepítés.
* Adja hozzá a NuGet-csomagok.
* Állítsa be a projekt hivatkozásait.
* Másolja át a kódot és konfigurációs fájljainak együttműködik hello oktatóanyag hello előző szakaszban letöltött hello alkalmazásból.
* Tekintse át a hello részei hello kódot, amely együttműködik az Azure-blobokat és üzenetsorokat és WebJobs SDK hello.

### <a name="create-a-visual-studio-solution-with-a-web-project-and-class-library-project"></a>A Visual Studio megoldás hordozhatóosztálytár-projektjének és webes projektet hoz létre
1. A Visual Studio felületén válassza **fájl** > **új** > **projekt**.
2. A hello **új projekt** párbeszédpanelen válasszon **Visual C#** > **webes** > **ASP.NET Web Application (.NET-keretrendszer)**.
3. Név: hello projekt ContosoAdsWeb, hello megoldás ContosoAdsWebJobsSDK name (hello megoldásnév módosítható, ha a hello kívánja menteni mappában, amelyben hello letöltött megoldás), és kattintson a **OK**.

    ![Új projekt](./media/websites-dotnet-webjobs-sdk-get-started/newproject.png)
4. A hello **új ASP.NET-webalkalmazás** párbeszédpanelen válassza hello MVC-sablont, majd **hitelesítés módosítása**.

    ![Hitelesítés módosítása](./media/websites-dotnet-webjobs-sdk-get-started/chgauth.png)
5. A hello **hitelesítés módosítása** párbeszédpanelen válasszon **nem hitelesítési**, és kattintson a **OK**.

    ![Nincs hitelesítés](./media/websites-dotnet-webjobs-sdk-get-started/noauth.png)
6. A hello **új ASP.NET-webalkalmazás** párbeszédpanel, kattintson a **OK**.

    A Visual Studio létrehozza a hello megoldás és hello webes projektet.
7. A **Megoldáskezelőben**, kattintson a jobb gombbal a hello megoldás (nem hello projekt), és válassza a **Hozzáadás** > **új projekt**.
8. A hello **új projekt hozzáadása** párbeszédpanelen válasszon **Visual C#** > **klasszikus Windows asztal** > **Class Library (.NET Keretrendszer)** sablont.  
9. Név hello projekt *ContosoAdsCommon*, és kattintson a **OK**.

    A projektnek hello Entity Framework-környezetre és a hello adatmodell, amely mindkét hello előtér és háttér fogja használni. Alternatív megoldásként sikerült hello webes projekt hello EF-hez kapcsolódó osztályokat definiálja, és hivatkozhat erre a projektre hello webjobs-feladat projektből. De a webjobs-feladat projekt majd kell egy tooweb referenciaszerelvények, amelyre nincs szüksége.

### <a name="add-a-console-application-project-that-has-webjobs-deployment-enabled"></a>Egy konzolalkalmazás-projektet, amelyen engedélyezve van a webjobs-feladatok telepítés hozzáadása
1. Kattintson a jobb gombbal a hello webes projekt (nem hello megoldás vagy hello hordozhatóosztálytár-projektjének), és kattintson **Hozzáadás** > **új Azure webjobs-feladat projekt**.

    ![Új Azure webjobs-feladat Projekt menü kiválasztása](./media/websites-dotnet-webjobs-sdk-get-started/newawjp.png)
2. A hello **adja hozzá a Azure webjobs-feladat** párbeszédpanelen adja meg mindkét hello ContosoAdsWebJob **projekt neve** és hello **webjobs-feladat neve**. Hagyja **webjobs-feladat futtatása mód** túl beállítása**futtatása folyamatosan**.
3. Kattintson az **OK** gombra.

   Visual Studio létrehoz egy konzolalkalmazást, amely konfigurált toodeploy hello webes projekt telepítése, a webjobs-feladat van. toodo, hogy végrehajtott feladatok hello projekt létrehozása után a következő hello:

   * Hozzáadott egy *webjobs-feladat közzététele settings.json* hello webjobs-feladat projekt Tulajdonságok mappában található fájl.
   * Hozzáadott egy *webjobs-list.json* hello webes projekt Tulajdonságok mappában található fájl.
   * Hello Microsoft.Web.WebJobs.Publish NuGet-csomagja telepítve hello webjobs-feladat projektben.

   Változásokkal kapcsolatos további információkért lásd: [hogyan toodeploy webjobs-feladatok Visual Studio használatával](websites-dotnet-deploy-webjobs.md).

### <a name="add-nuget-packages"></a>Adja hozzá a NuGet-csomagok
Új projekt sablon hello webjobs-feladat projekt automatikusan telepíti a hello WebJobs SDK NuGet-csomag [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) és annak függőségeit.

Hello webjobs-feladat projekt automatikusan telepített WebJobs SDK-függőség van hello egyik hello Azure Storage ügyfél könyvtár (SCL). Azonban meg kell tooadd azt toohello webes projekt toowork a blobokat és üzenetsorokat.

1. Nyissa meg hello **NuGet-csomagok kezelése** hello megoldás párbeszédpanel.
2. Hello bal oldali ablaktáblában jelöljön ki **telepített csomag**.
3. Hello található *Azure Storage* csomagot, majd kattintson a **kezelése**.
4. A hello **projekt kijelölése** jelölését, jelölje be hello **ContosoAdsWeb** jelölőnégyzetet, majd kattintson a **OK**.

    Mind a három projekt hello Entity Framework toowork használja az SQL-adatbázis adatokkal.
5. Hello bal oldali ablaktáblában jelöljön ki **Online**.
6. Hello található *EntityFramework* NuGet csomagot, majd telepítse mind a három projektben.

### <a name="set-project-references"></a>A projekt hivatkozásainak beállítása
Az internetes és a webjobs-feladat projektek együttműködve hello SQL-adatbázis, így mindkettő kell egy hivatkozás toohello ContosoAdsCommon projektre.

1. Hello ContosoAdsWeb projektben állítson be egy hivatkozást toohello ContosoAdsCommon projektre. (Kattintson a jobb gombbal a ContosoAdsWeb projektben hello, és kattintson **Hozzáadás** > **hivatkozás**. 
2. A hello **hivatkozáskezelő** párbeszédpanelen jelölje ki **projektek** > **megoldás** > **ContosoAdsCommon**, és kattintson a **OK**.)
   
    hello webjobs-feladat projekt hivatkozásainak van szüksége, lemezképek használata és a kapcsolati karakterláncok használata.

4. Hello ContosoAdsWebJob projektben állítson be egy hivatkozást túl`System.Drawing` és `System.Configuration`.

### <a name="add-code-and-configuration-files"></a>Adja hozzá a kódot és konfigurációs fájlok
Ez az oktatóanyag nem szerepelnek azok hogyan túl[hozzon létre az MVC-vezérlők és nézetek használatával állványok](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started), hogyan túl[Entity Framework-kód, amely az SQL Server-adatbázisok írási](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc), vagy [hello alapjait aszinkron programozás az ASP.NET 4.5 a](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async). Ezért, az marad toodo hello letöltött megoldásból hello új megoldásba történő kód és a konfigurációs fájlok másolása. Miután ezt megtenné, hello következő szakaszok bemutatják és ismertetik hello kód legfontosabb részeit.

tooadd fájlok tooa projekt vagy egy mappát, kattintson a jobb gombbal hello projekt vagy mappa és kattintson **Hozzáadás** > **meglévő cikk**. Jelölje be hello fájlokat kívánja, majd kattintson **Hozzáadás**. Ha a rendszer rákérdez, hogy kívánja-e tooreplace meglévő fájlokat, kattintson a **Igen**.

1. Hello ContosoAdsCommon projektben törölje a hello *Class1.cs* fájlt, és a hely hello következő fájlok hello letöltött projektből.

   * *Ad.cs*
   * *ContosoAdscontext.cs*
   * *BlobInformation.cs*
2. Hello ContosoAdsWeb projektben adja hozzá a következő fájlok hello letöltött projektből hello.

   * *Web.config*
   * *Global.asax.cs*  
   * A hello *tartományvezérlők* mappa: *AdController.cs*
   * A hello *Views\Shared* mappa: *_Layout.cshtml* fájl
   * A hello *Views\Home* mappa: *Index.cshtml*
   * A hello *Views\Ad* mappában (először hozza létre hello mappa): öt *.cshtml* fájlok
3. Hello ContosoAdsWebJob projektben adja hozzá a következő fájlok hello letöltött projektből hello.

   * *App.config* (hello fájl szűrő túl módosítása**minden fájl**)
   * *Program.cs*
   * *Functions.cs*

Most létre, futtatása és hello az oktatóanyag korábbi utasításai szerint hello alkalmazás központi telepítése. Mielőtt ezt megtenné, azonban leállítása hello webjobs-feladat, amely hello első webalkalmazás telepített továbbra is futtatja. Ellenkező esetben a webjobs-feladat fog helyileg létrehozott üzenetek feldolgozásához vagy hello alkalmazás egy új webalkalmazást futtatja, mivel minden használ hello azonos a tárfiók.

## <a id="code"></a>Tekintse át a hello alkalmazás kódja
hello alábbi szakaszok ismertetik a kód hello kapcsolódó tooworking a hello WebJobs SDK és Azure Storage blobokat és üzenetsorokat.

> [!NOTE]
> Hello az adott toohello kód WebJobs SDK, nyissa meg a toohello [Program.cs és Functions.cs](#programcs) szakaszok.
>
>

### <a name="contosoadscommon---adcs"></a>ContosoAdsCommon – Ad.cs
hello Ad.cs fájl megad, egy felsorolást a kategóriákhoz és egy POCO entitásosztályt a hirdetés információihoz.

        public enum Category
        {
            Cars,
            [Display(Name="Real Estate")]
            RealEstate,
            [Display(Name = "Free Stuff")]
            FreeStuff
        }

        public class Ad
        {
            public int AdId { get; set; }

            [StringLength(100)]
            public string Title { get; set; }

            public int Price { get; set; }

            [StringLength(1000)]
            [DataType(DataType.MultilineText)]
            public string Description { get; set; }

            [StringLength(1000)]
            [DisplayName("Full-size Image")]
            public string ImageURL { get; set; }

            [StringLength(1000)]
            [DisplayName("Thumbnail")]
            public string ThumbnailURL { get; set; }

            [DataType(DataType.Date)]
            [DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
            public DateTime PostedDate { get; set; }

            public Category? Category { get; set; }
            [StringLength(12)]
            public string Phone { get; set; }
        }

### <a name="contosoadscommon---contosoadscontextcs"></a>ContosoAdsCommon – ContosoAdsContext.cs
hello ContosoAdsContext osztály megadja szolgál, hogy hello Ad osztály egy DbSet gyűjteményben, amely Entity Framework egy SQL-adatbázisban tárolja.

        public class ContosoAdsContext : DbContext
        {
            public ContosoAdsContext() : base("name=ContosoAdsContext")
            {
            }
            public ContosoAdsContext(string connString)
                : base(connString)
            {
            }
            public System.Data.Entity.DbSet<Ad> Ads { get; set; }
        }

hello osztály két konstruktorral rendelkezik. hello először hello webes projekt használja, és annak hello hello Web.config fájl vagy hello Azure futtatókörnyezetben tárolt kapcsolati karakterlánc nevét adja meg. hello második konstruktor lehetővé teszi toopass hello tényleges kapcsolati karakterláncban. Mivel nem rendelkezik a Web.config fájl, amely van szükség hello webjobs-feladat a projekten. A korábban Ha a kapcsolati karakterlánc tárolásának, és láthatja, hogyan később hello kód keresi hello kapcsolati karakterlánc elindítja hello DbContext osztályt.

### <a name="contosoadscommon---blobinformationcs"></a>ContosoAdsCommon – BlobInformation.cs
Hello `BlobInformation` osztálya: egy kép blob várólista üzenetben használt toostore információt.

        public class BlobInformation
        {
            public Uri BlobUri { get; set; }

            public string BlobName
            {
                get
                {
                    return BlobUri.Segments[BlobUri.Segments.Length - 1];
                }
            }
            public string BlobNameWithoutExtension
            {
                get
                {
                    return Path.GetFileNameWithoutExtension(BlobName);
                }
            }
            public int AdId { get; set; }
        }


### <a name="contosoadsweb---globalasaxcs"></a>ContosoAdsWeb – Global.asax.cs
Hello meghívott kód `Application_Start` hoz létre egy *képek* blobtárolót és egy *képek* üzenetsort, amennyiben még nem léteznek. Ez biztosítja, hogy valahányszor új tárfiókot használatba hello szükséges blobtároló és üzenetsor automatikusan létrejönnek.

kód lekérdezi hozzáférés toohello tárfiók hello származó hello hello tárolási kapcsolati karakterlánc használatával *Web.config* fájl- vagy Azure futtatókörnyezetben.

        var storageAccount = CloudStorageAccount.Parse
            (ConfigurationManager.ConnectionStrings["AzureWebJobsStorage"].ToString());

Ezt követően onnan kapta, hogy a hivatkozás toohello *képek* blobtárolóhoz, létrehozza a hello tárolót, ha még nem létezik, és beállítja a hozzáférési engedélyeket hello új tárolóra. Alapértelmezés szerint az új tárolók csak a tárolási fiók hitelesítő adatait tooaccess BLOB ügyfelek engedélyezése. hello webalkalmazás kell hello toobe nyilvános blobok, úgy, hogy az URL-címekkel, hogy pont toohello kép blobok képeket jeleníthessen meg.

        var blobClient = storageAccount.CreateCloudBlobClient();
        var imagesBlobContainer = blobClient.GetContainerReference("images");
        if (imagesBlobContainer.CreateIfNotExists())
        {
            imagesBlobContainer.SetPermissions(
                new BlobContainerPermissions
                {
                    PublicAccess = BlobContainerPublicAccessType.Blob
                });
        }

Hasonló kód jogosultságot kap a hivatkozás toohello *thumbnailrequest* várólistára, és létrehoz egy új üzenetsort. Ebben az esetben nincs szükség az engedélyek módosítására.

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        var imagesQueue = queueClient.GetQueueReference("thumbnailrequest");
        imagesQueue.CreateIfNotExists();

### <a name="contosoadsweb---layoutcshtml"></a>ContosoAdsWeb – _Layout.cshtml
Hello *_Layout.cshtml* fájl hello alkalmazásnév beállítja az hello fejlécében és láblécében, és létrehoz egy "Ads" menübejegyzést.

### <a name="contosoadsweb---viewshomeindexcshtml"></a>ContosoAdsWeb – Views\Home\Index.cshtml
Hello *Views\Home\Index.cshtml* fájl kategóriahivatkozásokat jelenít meg hello kezdőlapján. hello hivatkozások átadják hello egész értéket hello `Category` felsorolás egy lekérdezési karakterlánc változó toohello Ads indexlapnak a.

        <li>@Html.ActionLink("Cars", "Index", "Ad", new { category = (int)Category.Cars }, null)</li>
        <li>@Html.ActionLink("Real estate", "Index", "Ad", new { category = (int)Category.RealEstate }, null)</li>
        <li>@Html.ActionLink("Free stuff", "Index", "Ad", new { category = (int)Category.FreeStuff }, null)</li>
        <li>@Html.ActionLink("All", "Index", "Ad", null, null)</li>

### <a name="contosoadsweb---adcontrollercs"></a>ContosoAdsWeb – AdController.cs
A hello *AdController.cs* fájl, hello konstruktor hívások hello `InitializeStorage` metódus toocreate Azure Storage ügyféloldali kódtár objektumok, amelyek az API-k használata a blobokat és üzenetsorokat.

Ezt követően hello kód kap-e a hivatkozási toohello *képek* blobtárolóra, ahogy azt korábban a *Global.asax.cs*. Hogy Mindeközben beállít egy alapértelmezett [újrapróbálkozási házirendet](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) megfelelő egy webalkalmazás. hello alapértelmezett exponenciális leállítási újrapróbálkozási házirend le több mint egy perc egy átmeneti hiba miatti ismételt próbálkozás hello webalkalmazás. Itt megadott hello újrapróbálkozási házirend minden próbálkozás mentése toothree próbálkozás után három másodperc alatt vár.

        var blobClient = storageAccount.CreateCloudBlobClient();
        blobClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
        imagesBlobContainer = blobClient.GetContainerReference("images");

Hasonló kód jogosultságot kap a hivatkozás toohello *képek* várólista.

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        queueClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
        imagesQueue = queueClient.GetQueueReference("blobnamerequest");

A legtöbb hello vezérlőkód jellemzően egy DbContext osztályt használó Entity Framework-adatmodell. Egy kivétel: hello HttpPost `Create` metódus, amely feltölt egy fájlt, és menti a blob Storage tárolóban. hello biztosít egy [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx) toohello metódus objektum.

        [HttpPost]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> Create(
            [Bind(Include = "Title,Price,Description,Category,Phone")] Ad ad,
            HttpPostedFileBase imageFile)

Egy fájl tooupload hello felhasználói választásakor hello kód feltölt hello fájlt, egy blobba menti, és frissíti a hirdetés adatbázisrekordot hello toohello blob URL-CÍMMEL.

        if (imageFile != null && imageFile.ContentLength != 0)
        {
            blob = await UploadAndSaveBlobAsync(imageFile);
            ad.ImageURL = blob.Uri.ToString();
        }

hello kódot, amely hello feltöltés van hello `UploadAndSaveBlobAsync` metódust. Létrehoz egy GUID nevet hello blob, a feltöltéseket és az eredményobjektumokat hello fájl, és egy hivatkozási mentve toohello blob adja vissza.

        private async Task<CloudBlockBlob> UploadAndSaveBlobAsync(HttpPostedFileBase imageFile)
        {
            string blobName = Guid.NewGuid().ToString() + Path.GetExtension(imageFile.FileName);
            CloudBlockBlob imageBlob = imagesBlobContainer.GetBlockBlobReference(blobName);
            using (var fileStream = imageFile.InputStream)
            {
                await imageBlob.UploadFromStreamAsync(fileStream);
            }
            return imageBlob;
        }

Miután a hello HttpPost `Create` metódus feltölt egy blobot és frissítések hello adatbázist, létrehoz egy várólista üzenet tooinform hello háttérfolyamatot, hogy a lemezkép átalakítás tooa miniatűr készen áll-e.

        BlobInformation blobInfo = new BlobInformation() { AdId = ad.AdId, BlobUri = new Uri(ad.ImageURL) };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        await thumbnailRequestQueue.AddMessageAsync(queueMessage);

hello HttpPost kódját hello `Edit` metódus hasonló, azzal a különbséggel, hogy hello felhasználó kiválaszt egy új képfájlt, ha bármely a hirdetés létező blobot törölni kell.

        if (imageFile != null && imageFile.ContentLength != 0)
        {
            await DeleteAdBlobsAsync(ad);
            imageBlob = await UploadAndSaveBlobAsync(imageFile);
            ad.ImageURL = imageBlob.Uri.ToString();
        }

Itt található blobok törlése, ha töröl egy ad hello kódot:

        private async Task DeleteAdBlobsAsync(Ad ad)
        {
            if (!string.IsNullOrWhiteSpace(ad.ImageURL))
            {
                Uri blobUri = new Uri(ad.ImageURL);
                await DeleteAdBlobAsync(blobUri);
            }
            if (!string.IsNullOrWhiteSpace(ad.ThumbnailURL))
            {
                Uri blobUri = new Uri(ad.ThumbnailURL);
                await DeleteAdBlobAsync(blobUri);
            }
        }
        private static async Task DeleteAdBlobAsync(Uri blobUri)
        {
            string blobName = blobUri.Segments[blobUri.Segments.Length - 1];
            CloudBlockBlob blobToDelete = imagesBlobContainer.GetBlockBlobReference(blobName);
            await blobToDelete.DeleteAsync();
        }

### <a name="contosoadsweb---viewsadindexcshtml-and-detailscshtml"></a>ContosoAdsWeb – Views\Ad\Index.cshtml és Details.cshtml
Hello *Index.cshtml* fájlt tartalmazó miniatűrt jelenít meg hello hirdetés többi adatát:

        <img  src="@Html.Raw(item.ThumbnailURL)" />

Hello *Details.cshtml* fájl hello teljes méretű képet jeleníti meg:

        <img src="@Html.Raw(Model.ImageURL)" />

### <a name="contosoadsweb---viewsadcreatecshtml-and-editcshtml"></a>ContosoAdsWeb – Views\Ad\Create.cshtml és Edit.cshtml
Hello *Create.cshtml* és *Edit.cshtml* fájlok megadják az űrlap kódolását, hogy a lehetővé teszi, hogy a tartományvezérlő tooget hello hello `HttpPostedFileBase` objektum.

        @using (Html.BeginForm("Create", "Ad", FormMethod.Post, new { enctype = "multipart/form-data" }))

Egy `<input>` elem jelzi hello böngésző tooprovide egy Fájlkiválasztási párbeszédpanelt.

        <input type="file" name="imageFile" accept="image/*" class="form-control fileupload" />

### <a id="programcs"></a>ContosoAdsWebJob - Program.cs
Ha hello webjobs-feladat indítása hello `Main` metódushívások hello WebJobs SDK `JobHost.RunAndBlock` metódus toobegin végrehajtása hello aktuális szálon kiváltott funkciók.

        static void Main(string[] args)
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

### <a id="generatethumbnail"></a>ContosoAdsWebJob - Functions.cs - GenerateThumbnail módszer
hello WebJobs SDK a metódus meghívja a várólista-üzenet fogadásakor. hello módszer miniatűr hoz létre, és visszahelyezi a miniatűr hello hello adatbázis URL-címet.

        public static void GenerateThumbnail(
        [QueueTrigger("thumbnailrequest")] BlobInformation blobInfo,
        [Blob("images/{BlobName}", FileAccess.Read)] Stream input,
        [Blob("images/{BlobNameWithoutExtension}_thumbnail.jpg")] CloudBlockBlob outputBlob)
        {
            using (Stream output = outputBlob.OpenWrite())
            {
                ConvertImageToThumbnailJPG(input, output);
                outputBlob.Properties.ContentType = "image/jpeg";
            }

            // Entity Framework context class is not thread-safe, so it must
            // be instantiated and disposed within hello function.
            using (ContosoAdsContext db = new ContosoAdsContext())
            {
                var id = blobInfo.AdId;
                Ad ad = db.Ads.Find(id);
                if (ad == null)
                {
                    throw new Exception(String.Format("AdId {0} not found, can't create thumbnail", id.ToString()));
                }
                ad.ThumbnailURL = outputBlob.Uri.ToString();
                db.SaveChanges();
            }
        }

* Hello `QueueTrigger` attribútum arra utasítja a WebJobs SDK toocall hello ezt a módszert egy új üzenet hello thumbnailrequest várólista fogadásakor.

        [QueueTrigger("thumbnailrequest")] BlobInformation blobInfo,

    Hello `BlobInformation` üdvözlőüzenetére várólista célja a hello automatikusan deszerializált `blobInfo` paraméter. Hello metódus befejezésekor hello várólista üzenet törlődik. Ha hello módszer nem jár sikerrel, befejezése előtt, a rendszer nem törli a várólista üdvözlőüzenetére; a 10 perces címbérlet lejárta után hello üzenet kiadott toobe újra felvételre és feldolgozni. Ebben a sorozatban a nem határozatlan ideig ismétlődik, ha üzenet mindig kivételt okoz. 5 sikertelen kísérlet tooprocess egy üzenetet, után üdvözlőüzenetére-e az áthelyezett tooa várólista nevű {queuename}-elhalt. kísérletek maximális száma hello lehet konfigurálni.
* két hello `Blob` attribútumok adja meg az objektumok kötött tooblobs közül: egy toohello meglévő lemezkép-blob és egy tooa új miniatűr blob, amely hello hoz létre.

        [Blob("images/{BlobName}", FileAccess.Read)] Stream input,
        [Blob("images/{BlobNameWithoutExtension}_thumbnail.jpg")] CloudBlockBlob outputBlob)

    BLOB nevének származhat hello tulajdonságainak `BlobInformation` hello üzenetsorban található üzenetet kapott objektum (`BlobName` és `BlobNameWithoutExtension`). tooget hello működése érdekében a Storage ügyféloldali kódtára hello, hello használható `CloudBlockBlob` osztály toowork a BLOB. Ha azt szeretné, a toowork írt kódot tooreuse `Stream` objektumok, használhatja a hello `Stream` osztály.

További információ a toowrite működését, amelyek WebJobs SDK attribútumok használata, tekintse meg a következő erőforrások hello:

* [Hogyan toouse Azure várólista hello WebJobs SDK szolgáltatással](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)
* [Hogyan toouse Azure blob-tároló hello WebJobs SDK a](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
* [Hogyan toouse Azure table storage a hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)
* [Hogyan toouse Azure Service Bus a hello WebJobs SDK](websites-dotnet-webjobs-sdk-service-bus.md)

> [!NOTE]
> * A webalkalmazás több virtuális gép fut, ha több WebJobs egyidejűleg fog futni, és egyes esetekben ez eredményezhet hello azonos adatfeldolgozási első többször. Ez nem probléma hello beépített várólista, a blob és a Service Bus-eseményindítók használatakor. hello SDK biztosítja, hogy a funkciók dolgoz fel csak egyszer minden üzenet vagy a blob.
> * További információ tooimplement szabályos leállítást, lásd: [szabályos leállítást](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).
> * hello kód hello `ConvertImageToThumbnailJPG` (nincs ábrázolva) metódus osztályokat alkalmaz hello `System.Drawing` névtér az egyszerűség érdekében. A névtérben lévő osztályok hello azonban a Windows-űrlapokkal való használatra lettek tervezve. Windows- vagy ASP.NET-szolgáltatásban való használatuk nem támogatott. További információk a képfeldolgozási beállításokról: [Dynamic Image Generation](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) (Dinamikus képek létrehozása) és [Deep Inside Image Resizing](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na) (A képek átméretezésének részletei).
>
>

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban megtudhatta, egy egyszerű, a háttérbeli feldolgozás hello WebJobs SDK használó többrétegű alkalmazást. Ez a szakasz néhány ASP.NET Többrétegű alkalmazások és a webjobs-feladatok többet javaslatokat kínál.

### <a name="missing-features"></a>Hiányzó funkciók
hello alkalmazás-bevezető oktatóanyag az egyszerű lett megtartva. Egy valós alkalmazás megvalósítása volna [függőségi beszúrást](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) és hello [adattárát és egységét működik minták](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), használja [egy naplózási felület](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), használata[ EF Code First áttelepítést](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage adatok módosításait a modell, és használja [EF-kapcsolati rugalmasságot](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage átmeneti hálózati hibák.

### <a name="scaling-webjobs"></a>Webjobs-feladatok méretezése
Webjobs-feladatok egy webalkalmazás hello környezetében futnak, és nem méretezhetők legyenek külön-külön. Például ha egy szokásos webes alkalmazás ezen példányát, a háttérben futó folyamat csak egy példánya van, és néhány hello kiszolgáló-erőforrások (CPU, a memória, a stb.) ellenkező esetben lenne elérhető tooserve webes tartalmat használ.

Ha a forgalom függ a nap vagy hét napja, és hello háttérbeli feldolgozás meg kell toodo megvárhatja, alacsony forgalmú időpontban tudta ütemezni a a webjobs-feladatok toorun. Ha továbbra is túl magas, az adott megoldáshoz tartozó hello terhelést, hello háttér et futtathatja egy dedikált külön web app alkalmazásban webjobs-feladat erre a célra. Majd méretezheti a háttér-webalkalmazás egymástól függetlenül az előtérbeli webes alkalmazásból.

További információkért lásd: [skálázás WebJobs](websites-webjobs-resources.md#scale).

### <a name="avoiding-web-app-timeout-shut-downs"></a>Webes alkalmazás időtúllépés leállítási oszlopánál elkerülése
toomake meg arról, hogy a webjobs-feladatok mindig fut, és a webalkalmazás összes példányán fut, el kell tooenable hello [AlwaysOn](http://weblogs.asp.net/scottgu/archive/2014/01/16/windows-azure-staging-publishing-support-for-web-sites-monitoring-improvements-hyper-v-recovery-manager-ga-and-pci-compliance.aspx) szolgáltatás.

### <a name="using-hello-webjobs-sdk-outside-of-webjobs"></a>Hello kívül WebJobs WebJobs SDK használatával
Hello WebJobs SDK használó toorun nem rendelkezik Azure a webjobs-feladat. Helyileg futtathat, és más környezetekben, például egy felhőalapú szolgáltatás feldolgozói szerepkör vagy egy Windows-szolgáltatás is futtathatja. Azonban csak érheti el WebJobs SDK irányítópult hello Azure-webalkalmazás. toouse hello irányítópult tooconnect hello web app toohello storage-fiók beállításával hello AzureWebJobsDashboard kapcsolati karakterlánc hello használata van **konfigurálása** hello klasszikus portál lapján. Ezt követően kaphat a toohello irányítópult hello URL-cím a következő használatával:

https://{webappname}.SCM.azurewebsites.NET/azurejobs/#/Functions

További információkért lásd: [irányítópult első hello WebJobs SDK a helyi fejlesztési](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), azonban vegye figyelembe, hogy látható-e egy régi kapcsolati karakterlánc nevét.

### <a name="more-webjobs-documentation"></a>További WebJobs-dokumentáció
További információkért lásd: [Azure WebJobs-dokumentáció erőforrások](http://go.microsoft.com/fwlink/?LinkId=390226).
