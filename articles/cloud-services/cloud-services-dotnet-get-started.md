---
title: "az Azure felhőalapú szolgáltatásairól és ASP.NET lépései aaaGet |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate egy többrétegű alkalmazást ASP.NET MVC és az Azure használatával. hello alkalmazás fut egy felhőszolgáltatás, webes és feldolgozói szerepkörben. Entity Framework, SQL Database és Azure Storage üzenetsorokat és blobokat használ."
services: cloud-services, storage
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: d7aa440d-af4a-4f80-b804-cc46178df4f9
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/15/2017
ms.author: adegeo
ms.openlocfilehash: 86271c29b79fad3f01f8ea0e88fd00c7aefc970c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-cloud-services-and-aspnet"></a>Ismerkedés az Azure Cloud Services szolgáltatással és az ASP.NET keretrendszerrel

## <a name="overview"></a>Áttekintés
Ez az oktatóanyag bemutatja, hogyan toocreate előtér-, az ASP.NET mvc többrétegű .NET-alkalmazásokat, és telepítse azt tooan [Azure-felhőszolgáltatásban](cloud-services-choose-me.md). alkalmazás által használt hello [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279), hello [Azure Blob szolgáltatás](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), és hello [Azure Queue szolgáltatás](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern). Is [hello Visual Studio-projekt letöltése](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4) a hello MSDN Kódgalériából.

hello oktatóanyag bemutatja, hogyan toobuild, és futtassa hello alkalmazás helyileg, hogyan toodeploy azt tooAzure, és futtassa a hello felhőalapú, és hogyan toobuild az alapoktól. Indítsa el az alapoktól a felépítést, és ezután teszt hello és telepítés lépéseit, ha szeretné.

## <a name="contoso-ads-application"></a>Contoso Ads alkalmazás
hello alkalmazás egy hirdetőtábla. A felhasználók szöveg megadásával és egy kép feltöltésével hoznak létre hirdetéseket. Láthatják a hirdetések miniatűr képekkel ellátott listáját, és amikor kiválasztanak egy ad toosee hello részletek láthatják hello teljes méretű kép.

![Hirdetéslista](./media/cloud-services-dotnet-get-started/list.png)

hello alkalmazás használ hello [várólista-központú](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) toooff-betöltési hello processzorigényes munkáján miniatűrök tooa háttér-folyamat létrehozása.

## <a name="alternative-architecture-websites-and-webjobs"></a>Alternatív architektúra: Websites és WebJobs
Ez az oktatóanyag bemutatja, hogyan toorun előtér- és háttér-egy Azure cloud service. A másik lehetőség az előtér-toorun hello egy [Azure-webhelyen](/services/web-sites/) és hello [WebJobs](http://go.microsoft.com/fwlink/?LinkId=390226) szolgáltatást (jelenleg előzetes verzió) hello háttér-a. Webjobs-feladatok alkalmazó oktatóanyagot, lásd: [Ismerkedés az Azure WebJobs SDK hello](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md). Hogyan toochoose hello szolgáltatás, amely legjobban információt adott esetben fér el, a következő témakörben: [Azure Websites, a Cloud Services és a virtuális gépek összevetése](../app-service-web/choose-web-site-cloud-service-vm.md).

## <a name="what-youll-learn"></a>Ismertetett témák
* Hogyan tooenable Azure fejlesztési telepítésével, a gép hello Azure SDK-t.
* Hogyan toocreate a Visual Studio cloud service-projekt egy ASP.NET MVC webes és feldolgozói szerepkörök.
* Hogyan tootest hello felhőszolgáltatás-projekt helyi, hello Azure storage emulator használatával.
* Hogyan toopublish hello felhő projekt tooan Azure felhőalapú szolgáltatás, és tesztelése egy Azure storage-fiók használatával.
* Hogyan tooupload fájlok, illetve hello Azure Blob szolgáltatásban tárolja őket.
* Hogyan toouse hello Azure Queue szolgáltatás rétegek közötti kommunikációhoz.

## <a name="prerequisites"></a>Előfeltételek
hello oktatóanyag feltételezi, hogy tudomásul veszi [alapfogalmaival Azure cloud services](cloud-services-choose-me.md) például *webes szerepkör* és *feldolgozói szerepkör* terminológiát.  Azt is feltételezi, hogy tudja, hogyan rendelkező toowork [ASP.NET MVC](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) vagy [Web Forms](http://www.asp.net/web-forms/tutorials/aspnet-45/getting-started-with-aspnet-45-web-forms/introduction-and-overview) Visual studióban. hello mintaalkalmazás MVC-t használja, de a legtöbb hello oktatóanyag is vonatkoznak tooWeb űrlapok.

Hello alkalmazás helyileg Azure-előfizetés nélkül is futtathatja, de egy toodeploy hello alkalmazás toohello felhő lesz szüksége. Ha nincs fiókja, [aktiválhatja az MSDN előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A55E3C668), vagy [regisztrálhat egy ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A55E3C668).

hello oktatóanyagban szereplő utasítások vagy a következő termékek hello használata:

* Visual Studio 2013
* Visual Studio 2015
* Visual Studio 2017

Ha ezek egyikét nincs, a Visual Studio is települ automatikusan hello Azure SDK telepítésekor.

## <a name="application-architecture"></a>Alkalmazásarchitektúra
hello alkalmazást használó Entity Framework Code First toocreate hello táblák és -hozzáférési hello adatok SQL-adatbázisban tárolja a hirdetéseket. Az egyes hirdetések hello adatbázis tárolók két URL-címet, egy a teljes méretű kép, a másik hello miniatűr hello.

![Hirdetés tábla](./media/cloud-services-dotnet-get-started/adtable.png)

Amikor egy felhasználó feltölt egy képet, hello előtér-futó webes szerepkör tárolja hello lemezképet egy [Azure blob](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), és hello ad adatokat toohello blob URL-CÍMMEL rendelkező hello adatbázisban tárolja. At hello azonos időben, egy üzenet tooan Azure üzenetsor-kezelési ír. A feldolgozói szerepkör rendszeres időközönként futó háttér-folyamat kérdezze le az új üzenetek hello várólista. Egy új üzenet jelenik meg, amikor hello feldolgozói szerepkör létrehozza a kép miniatűrjét, és a frissítések hello Miniatűr URL-adatbázis mező az adott Active Directory. hello alábbi ábra bemutatja hogyan működnek együtt hello hello alkalmazás részei.

![Contoso Ads architektúra](./media/cloud-services-dotnet-get-started/apparchitecture.png)

[!INCLUDE [install-sdk](../../includes/install-sdk-2017-2015-2013.md)]

## <a name="download-and-run-hello-completed-solution"></a>Befejeződött hello megoldás letöltése és futtatása
1. Töltse le és csomagolja ki a hello [kész megoldást](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4).
2. Indítsa el a Visual Studiót.
3. A hello **fájl** menüben válassza **nyissa meg a projekt**, és keresse meg a letöltött hello megoldás toowhere, majd nyissa meg a megoldásfájlt hello.
4. Nyomja le a CTRL + SHIFT + B toobuild hello megoldás.

    Alapértelmezés szerint a Visual Studio automatikusan visszaállítja hello NuGet-csomag tartalmát, amely nem szerepel a hello *.zip* fájlt. Hello csomagok nem állnak vissza, ha a telepítést, manuálisan is toohello **NuGet-csomagok kezelése megoldáshoz** párbeszédpanel megnyitásához, majd kattintson a hello **visszaállítása** hello felső jobb oldali gomb.
5. A **Megoldáskezelőben**, győződjön meg arról, hogy **ContosoAdsCloudService** hello indítási projekt legyen kijelölve.
6. Visual Studio 2015-öt használ, vagy a magasabb módosítani a hello SQL Server kapcsolati karakterlánc hello alkalmazásban *Web.config* fájl hello ContosoAdsWeb projekt és hello *ServiceConfiguration.Local.cscfg* hello ContosoAdsCloudService projekt fájlt. Minden esetben módosítsa "(localdb) \v11.0" túl "(localdb) \MSSQLLocalDB".
7. Nyomja le a CTRL + F5 toorun hello alkalmazás.

    Amikor helyileg futtat egy felhőszolgáltatás-projekt, a Visual Studio automatikusan meghívja-e a hello Azure *compute emulator* és az Azure *storage emulator*. hello számítási emulátor használ, a számítógép erőforrások toosimulate hello webes és feldolgozói szerepkörök környezeteit. hello storage emulator használ egy [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) adatbázis-toosimulate Azure felhőalapú tárolást.

    hello egy felhőszolgáltatás-projekt első futtatásakor szükséges egy percet hello emulátorok toostart fel. Ha az emulátorok elindulását, hello alapértelmezett böngésző megnyitja toohello alkalmazás kezdőlapját.

    ![Contoso Ads architektúra](./media/cloud-services-dotnet-get-started/home.png)
8. Kattintson a **Create an Ad** (Hirdetés létrehozása) gombra.
9. Adjon meg néhány tesztadatot, és válassza ki a *.jpg* tooupload lemezképet, és kattintson a **létrehozása**.

    ![Lap létrehozása](./media/cloud-services-dotnet-get-started/create.png)

    hello app toohello Index lapra kerül, de hello új hirdetés miniatűrje nem jelenik meg, mert a feldolgozása még nem történt meg.
10. Várjon egy kicsit, majd frissítse a hello Index lap toosee hello miniatűr.

     ![Index lap](./media/cloud-services-dotnet-get-started/list.png)
11. Kattintson a **részletek** a ad toosee hello teljes méretű kép.

     ![Részletek lap](./media/cloud-services-dotnet-get-started/details.png)

Ön már futott hello alkalmazás teljes egészében a helyi számítógépen, a felhő nélküli kapcsolat toohello. hello storage emulator hello várólista tárolja, és egy SQL Server Express LocalDB adatbázisban, és hello alkalmazás Blobadatok egy másik LocalDB adatbázisban tárolja az hello ad adatokat. Entity Framework Code First automatikusan létrehozott hello ad adatbázis hello hello webalkalmazás próbált tooaccess először azt.

A következő szakasz hello hello megoldás toouse Azure felhőbeli erőforrások üzenetsorok, blobok és hello adatbázis hello felhőben futtatott kell majd konfigurálnia. Ha helyileg toocontinue toorun szeretett volna, de felhőalapú tárolási és adatbázis erőforrásainak használatához, megteheti, hogy. Csak egy függetlenül attól, hogy a kapcsolati karakterláncok, amelyek láthatja beállítása hogyan toodo.

## <a name="deploy-hello-application-tooazure"></a>Hello alkalmazás tooAzure telepítése
El kell végeznie a következő lépéseket toorun hello alkalmazás hello felhőben hello:

* Hozzon létre egy Azure-felhőszolgáltatást.
* Hozzon létre egy Azure SQL-adatbázist.
* Hozzon létre egy Azure-tárfiókot.
* Konfigurálása hello megoldás toouse az Azure SQL database az Azure-ban futtatott.
* Hello megoldás toouse az Azure storage fiók beállítása az Azure-ban futtatott.
* Hello projekt tooyour Azure felhőalapú szolgáltatás üzembe helyezése.

### <a name="create-an-azure-cloud-service"></a>Azure-felhőszolgáltatás létrehozása
Azure-felhőszolgáltatás hello környezet hello alkalmazás futni fog.

1. A böngészőben nyissa meg a hello [Azure-portálon](https://portal.azure.com).
2. Kattintson az **Új > Számítás > Felhőszolgáltatás** elemre.

3. Hello DNS nevét beviteli mezőbe írja be egy URL-előtagját: hello felhőalapú szolgáltatás.

    Az URL-cím egyedi toobe rendelkezik.  Lesz jelenik meg hibaüzenet, ha úgy dönt, hello előtag már használatban van.
4. Adjon meg egy új erőforráscsoportot hello szolgáltatást. Kattintson a **hozzon létre új** és adjon meg egy nevet hello erőforrás csoport beviteli mezőbe, például a CS_contososadsRG.

5. Hello régió, ahol szeretné toodeploy hello alkalmazás kiválasztása.

    Ez a mező határozza meg, hogy a felhőszolgáltatása melyik adatközpontban fog üzemelni. Termelési alkalmazások esetében válassza hello régió legközelebbi tooyour ügyfelek. A jelen oktatóanyag esetében válassza ki a hello régió legközelebbi tooyou.
5. Kattintson a **Create** (Létrehozás) gombra.

    A kép a következő hello egy felhőalapú szolgáltatás URL-cím CSvccontosoads.cloudapp.net hello hozza létre.

    ![Új felhőszolgáltatás](./media/cloud-services-dotnet-get-started/newcs.png)

### <a name="create-an-azure-sql-database"></a>Azure SQL-adatbázis létrehozása
Hello alkalmazás futtatásakor hello felhőben, felhőalapú adatbázist fog használni.

1. A hello [Azure-portálon](https://portal.azure.com), kattintson a **új > adatbázisok > SQL-adatbázis**.
2. A hello **adatbázisnév** adja meg a *contosoads*.
3. A hello **erőforráscsoport**, kattintson a **meglévő** és select hello erőforráscsoport hello felhőalapú szolgáltatás használt.
4. Hello lemezképet, a következő kattintson **kiszolgáló - kötelező beállítások konfigurálása** és **hozzon létre egy új kiszolgálót**.

    ![Bújtatási toodatabase kiszolgáló](./media/cloud-services-dotnet-get-started/newdb.png)

    Azt is megteheti Ha az előfizetés már rendelkezik egy kiszolgálóra, kiválaszthatja, hogy a kiszolgáló hello legördülő listából.
5. A hello **kiszolgálónév** adja meg a *csvccontosodbserver*.

6. Adjon meg egy rendszergazdai **Bejelentkezési nevet** és **Jelszót**.

    Ha az **Új kiszolgáló létrehozása** elemet választotta, nem meglévő nevet és jelszót kell megadnia. Adta meg egy új nevet és jelszót, amely akkor most toouse később hello adatbázis elérésekor. Ha egy korábban létrehozott server, kéri hello jelszó toohello rendszergazdai felhasználói fiókhoz már létrehozott.
7. Válasszon azonos hello **hely** hello felhőszolgáltatás választott.

    Ha a hello felhőalapú szolgáltatás és az adatbázis van különböző adatközpontokban (különböző régiókban), a késés mértéke megnő, és hello adatközponton kívül használt sávszélességért fizetnie kell. Az adatközponton belül használt sávszélesség ingyenes.
8. Ellenőrizze **engedélyezése az azure szolgáltatások tooaccess server**.
9. Kattintson a **válasszon** hello új kiszolgálóhoz.

    ![Új SQL Database-kiszolgáló](./media/cloud-services-dotnet-get-started/newdbserver.png)
10. Kattintson a **Létrehozás** gombra.

### <a name="create-an-azure-storage-account"></a>Azure-tárfiók létrehozása
Azure-tárfiók forrásokat biztosít hello felhőben üzenetsor és a blob adatainak tárolásához.

Egy valós alkalmazás esetében általában külön fiókot hozna létre az alkalmazás adatai és a naplózási adatok, illetve a tesztadatok és a termelési adatok számára is. Ebben az oktatóanyagban csak egy fiókot fog használni.

1. A hello [Azure-portálon](https://portal.azure.com), kattintson a **új > tárolás > tárfiók - blob, a fájl, a tábla, a várólista**.
2. A hello **neve** mezőbe írjon be egy URL-előtagot.

    A hello mező alatt látható előtag plus hello szöveg hello egyedi URL-cím tooyour tárfiók lesz. Ha valaki más hello előtag már használatban van, akkor kell toochoose különböző előtag.
3. Set hello **telepítési modell** túl*klasszikus*.

4. Set hello **replikációs** legördülő lista túl**helyileg redundáns tárolás**.

    A georeplikáció engedélyezve van a tárfiók, hello tárolt tartalom esetén meg kell replikált tooa másodlagos adatközpontba tooenable feladatátvételi hello elsődleges helyen jelentős katasztrófa esetén. A georeplikáció további költségeket vonhat maga után. A vizsgálati és fejlesztői fiókok esetében általában nem érdemes toopay a georeplikációért. További információ: [Tárfiók létrehozása, kezelése vagy törlése](../storage/common/storage-create-storage-account.md).

5. A hello **erőforráscsoport**, kattintson a **meglévő** és select hello erőforráscsoport hello felhőalapú szolgáltatás használt.
6. Set hello **hely** legördülő lista toohello ugyanabban a régióban hello felhőszolgáltatás számára is választott.

    Ha a hello felhőalapú szolgáltatás és a tárolási fiók különböző adatközpontokban van (különböző régiókban), a késés mértéke megnő, és hello adatközponton kívül használt sávszélességért fizetnie kell. Az adatközponton belül használt sávszélesség ingyenes.

    Azure-affinitáscsoportok egy erőforrást egy adatközpontban, ami csökkentheti a késés mechanizmus toominimize hello távolságát adja meg. A jelen oktatóanyag nem használ affinitáscsoportokat. További információkért lásd: [hogyan tooCreate kapcsolatot csoportosítani az Azure-ban](http://msdn.microsoft.com/library/jj156209.aspx).
7. Kattintson a **Create** (Létrehozás) gombra.

    ![Új tárfiók](./media/cloud-services-dotnet-get-started/newstorage.png)

    Hello kép, a tárfiók létrehozása hello URL-CÍMMEL rendelkező `csvccontosoads.core.windows.net`.

### <a name="configure-hello-solution-toouse-your-azure-sql-database-when-it-runs-in-azure"></a>Konfigurálja a az Azure SQL-adatbázis hello megoldás toouse futáskor az Azure-ban
webes projekt hello és hello feldolgozói szerepkör projekt minden a saját adatbázis-kapcsolati karakterláncot, és minden egyes kell toopoint toohello Azure SQL database az Azure-ban hello alkalmazás futtatásakor.

Fogja használni a [Web.config transzformálása](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) hello webes szerepkör és a felhő környezet szolgáltatásbeállításra hello feldolgozói szerepkör esetében.

> [!NOTE]
> A jelen szakaszban és a következő szakaszban hello tárolt hitelesítő adatokat projektfájlokban. [Ne tároljon bizalmas adatokat nyilvános forráskódú adattárakban.](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets)
>
>

1. Hello ContosoAdsWeb projektben nyissa meg a hello *Web.Release.config* hello alkalmazás átalakítófájlt *Web.config* fájljához, törölje a hello Megjegyzésblokk, amely tartalmaz egy `<connectionStrings>` elem és a Beillesztés a következő kód helyette hello.

    ```xml
    <connectionStrings>
        <add name="ContosoAdsContext" connectionString="{connectionstring}"
        providerName="System.Data.SqlClient" xdt:Transform="SetAttributes" xdt:Locator="Match(name)"/>
    </connectionStrings>
    ```

    Hagyja nyitva a szerkesztéshez hello fájlt.
2. A hello [Azure-portálon](https://portal.azure.com), kattintson a **SQL-adatbázisok** hello bal oldali ablaktáblában kattintson ehhez az oktatóanyaghoz létrehozott hello adatbázisra, majd **kapcsolati karakterláncok megjelenítése**.

    ![Kapcsolati karakterláncok megjelenítése](./media/cloud-services-dotnet-get-started/showcs.png)

    hello portál megjeleníti a kapcsolati karakterláncokat helyőrzővel helyettesített hello jelszó.

    ![Kapcsolati karakterláncok](./media/cloud-services-dotnet-get-started/connstrings.png)
3. A hello *Web.Release.config* fájl átalakítása, akkor törölje `{connectionstring}` és illessze be a hely hello hello Azure portálról származó ADO.NET kapcsolati karakterláncot.
4. Kapcsolat-karakterláncban hello hello beillesztett *Web.Release.config* fájl átalakítása, cserélje le `{your_password_here}` hello új SQL-adatbázis létrehozott hello jelszóval.
5. Hello fájl mentéséhez.  
6. Válassza ki, és másolja a hello kapcsolati karakterláncot (nélkül hello idézőjelek közé foglalt) hello feldolgozói szerepkör projekt konfigurálásának lépéseit a következő hello használható.
7. A **Megoldáskezelőben**, a **szerepkörök** hello felhőszolgáltatás-projekt, kattintson a jobb egérgombbal **ContosoAdsWorker** majd **tulajdonságok**.

    ![Szerepkör tulajdonságai](./media/cloud-services-dotnet-get-started/rolepropertiesworker.png)
8. Kattintson a hello **beállítások** fülre.
9. Változás **szolgáltatáskonfiguráció** túl**felhő**.
10. Jelölje be hello **érték** hello mezőt `ContosoAdsDbConnectionString` beállításával, és illessze be az hello hello az oktatóanyag előző szakaszából másolt hello kapcsolati karakterláncot.

     ![A feldolgozói szerepkör adatbázis-kapcsolati karakterlánca](./media/cloud-services-dotnet-get-started/workerdbcs.png)
11. Mentse a módosításokat.  

### <a name="configure-hello-solution-toouse-your-azure-storage-account-when-it-runs-in-azure"></a>Konfigurálja a Azure-tárfiókot hello megoldás toouse futáskor az Azure-ban
Az Azure storage kapcsolati karakterláncainak hello webes szerepkör projekt és hello feldolgozói szerepkör projekt hello felhőszolgáltatás-projekt környezeti beállításokban vannak tárolva. Minden olyan projekthez nincs az alkalmazás futásakor, hello helyileg használt beállítások toobe külön készletét, és mikor fusson a hello felhőben. Hello felhőkörnyezet beállításait a webes és feldolgozói szerepkör projektek frissíteni fogja.

1. A **Megoldáskezelőben**, kattintson a jobb gombbal **ContosoAdsWeb** alatt **szerepkörök** a hello **ContosoAdsCloudService** projektre, és kattintson a **Tulajdonságok**.

    ![Szerepkör tulajdonságai](./media/cloud-services-dotnet-get-started/roleproperties.png)
2. Kattintson a hello **beállítások** fülre. A hello **szolgáltatáskonfiguráció** legördülő listán válassza ki **felhő**.

    ![Felhő konfigurálása](./media/cloud-services-dotnet-get-started/sccloud.png)
3. Jelölje be hello **StorageConnectionString** bejegyzést, és megjelenik egy három pontot (**...** ) hello jobb oldali végén hello gombra. Kattintson a hello három pont gombra tooopen hello **tárolási fiók kapcsolati karakterlánc létrehozása** párbeszédpanel megnyitásához.

    ![A Kapcsolati karakterlánc létrehozása mező megnyitása](./media/cloud-services-dotnet-get-started/opencscreate.png)
4. A hello **tárolási kapcsolati karakterlánc létrehozása** párbeszédpanel, kattintson a **az előfizetés**, válassza ki a korábban létrehozott hello tárfiók, és kattintson a **OK**. Ha még nincs bejelentkezve, a rendszer az Azure-fiókja hitelesítő adatait kéri.

    ![Tárfiók kapcsolati karakterláncának létrehozása](./media/cloud-services-dotnet-get-started/createstoragecs.png)
5. Mentse a módosításokat.
6. Kövesse hello ugyanazt az eljárást, a hello `StorageConnectionString` kapcsolati karakterlánc tooset hello `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` kapcsolati karakterláncot.

    Ez a kapcsolati karakterlánc naplózásra használható.
7. Kövesse hello ugyanazt az eljárást, a hello **ContosoAdsWeb** szerepkör tooset mindkét kapcsolati karakterláncának hello **ContosoAdsWorker** szerepkör. Ne feledje tooset **szolgáltatáskonfiguráció** túl**felhő**.

Visual Studio felhasználói felületén hello segítségével konfigurált hello beállítások következő fájlok hello ContosoAdsCloudService projektben hello vannak tárolva:

* *ServiceDefinition.csdef* -hello beállításneveket határozza meg.
* *ServiceConfiguration.Cloud.cscfg* – értékeket biztosít hello felhőben hello alkalmazás futtatásakor.
* *ServiceConfiguration.Local.cscfg* – értékeket biztosít amikor hello alkalmazás helyi futtatásához.

Hello ServiceDefinition.csdef például a következő definíciókat hello tartalmazza:

```xml
<ConfigurationSettings>
    <Setting name="StorageConnectionString" />
    <Setting name="ContosoAdsDbConnectionString" />
</ConfigurationSettings>
```

És hello *ServiceConfiguration.Cloud.cscfg* fájl ezeket a beállításokat, a Visual Studio megadott hello értékeket tartalmaz.

```xml
<Role name="ContosoAdsWorker">
    <Instances count="1" />
    <ConfigurationSettings>
        <Setting name="StorageConnectionString" value="{yourconnectionstring}" />
        <Setting name="ContosoAdsDbConnectionString" value="{yourconnectionstring}" />
        <!-- other settings not shown -->

    </ConfigurationSettings>
    <!-- other settings not shown -->

</Role>
```

Hello `<Instances>` beállítással a virtuális gépek futó Azure hello feldolgozói szerepkör kódját hello száma. Hello [további lépések](#next-steps) szakasz tartalmaz hivatkozásokat toomore információkat a felhőalapú szolgáltatás, kiterjesztése

### <a name="deploy-hello-project-tooazure"></a>Hello projekt tooAzure telepítése
1. A **Megoldáskezelőben**, kattintson a jobb gombbal hello **ContosoAdsCloudService** felhőprojektre, és válassza ki **közzététel**.

   ![Közzététel menü](./media/cloud-services-dotnet-get-started/pubmenu.png)
2. A hello **jelentkezzen be a** hello lépését **Azure-alkalmazás közzététele** varázsló, kattintson a **következő**.

    ![Bejelentkezés lépés](./media/cloud-services-dotnet-get-started/pubsignin.png)
3. A hello **beállítások** lépés hello varázsló, kattintson a **következő**.

    ![Beállítások lépés](./media/cloud-services-dotnet-get-started/pubsettings.png)

    alapértelmezett beállítások a hello hello **speciális** lapon ez az oktatóanyag rendben. További információ a Speciális lap hello: [Azure alkalmazás közzététele varázsló](http://msdn.microsoft.com/library/hh535756.aspx).
4. A hello **összegzés** lépésre, a **közzététel**.

    ![Összegzés lépés](./media/cloud-services-dotnet-get-started/pubsummary.png)

   Hello **Azure tevékenységnapló** ablak a Visual Studióban.
5. Hello jobbra mutató nyíl ikon tooexpand hello központi telepítés a Részletek gombra.

    hello telepítési too5 perc vagy több toocomplete is eltarthat.

    ![Azure tevékenységnapló ablak](./media/cloud-services-dotnet-get-started/waal.png)
6. Ha hello központi telepítési állapot befejeződött, kattintson a hello **webes alkalmazás URL-címhez** toostart hello alkalmazás.
7. Most tesztelheti hello app létrehozásával, megtekintésével és szerkesztésével hirdetések, hello alkalmazás helyileg futtatta hasonló módon.

> [!NOTE]
> Amikor végzett a tesztelési, törölje vagy állítsa le a hello felhőalapú szolgáltatás. Akkor is, ha nem használ hello felhőalapú szolgáltatás, keletkezhetnek költségek, mivel a virtuálisgép-erőforrások vannak lefoglalva számára. Ha hagyja, hogy továbbra is fusson, bárki, aki rátalál az URL-címére, létrehozhat és megtekinthet hirdetéseket. A hello [Azure-portálon](https://portal.azure.com), toohello lépjen **áttekintése** a felhőszolgáltatás fülre, majd hello **törlése** hello oldal hello tetején gombra. Ha csak tootemporarily megakadályozni, hogy mások hozzáférni hello webhelyhez, kattintson a **leállítása** helyette. Ebben az esetben díjakat továbbra is tooaccrue. Egy hasonló művelet toodelete hello SQL-adatbázis és a tárolási fiók is hajtsa végre, amikor már nincs szüksége.
>
>

## <a name="create-hello-application-from-scratch"></a>Teljesen új hello alkalmazás létrehozása
Ha még nem töltötte le [befejeződött hello alkalmazás](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4), most tegye. Ön fájlokat fog átmásolni hello letöltött projektet hello új projektbe.

A lépéseket követve hello hello Contoso Ads alkalmazás létrehozása foglal magában:

* Hozzon létre egy Visual Studio felhőszolgáltatás-megoldást.
* Frissítse és adja hozzá a NuGet-csomagokat.
* Állítsa be a projekt hivatkozásait.
* Konfigurálja a kapcsolati karakterláncokat.
* Adja hozzá a kódfájlokat.

Hello megoldás létrehozása után áttekinti hello kód, mely az egyedi toocloud service projektek és Azure blobokat és üzenetsorokat.

### <a name="create-a-cloud-service-visual-studio-solution"></a>Visual Studio felhőszolgáltatás-megoldás létrehozása
1. A Visual Studio felületén válassza **új projekt** a hello **fájl** menü.
2. Hello hello bal oldali ablaktáblájában **új projekt** párbeszédpanelen bontsa ki **Visual C#** válassza **felhő** sablonokat, és válassza a hello **Azure Cloud Service** sablont.
3. Hello projektet és a megoldás ContosoAdsCloudService nevet, és kattintson **OK**.

    ![Új projekt](./media/cloud-services-dotnet-get-started/newproject.png)
4. A hello **új Azure Cloud Service** párbeszédpanelen adjon hozzá egy webes és feldolgozói szerepkörök. Hello webes szerepkörnek adja a ContosoAdsWeb, és a hello feldolgozói szerepkör ContosoAdsWorker nevet. (Használ hello ceruza ikonra hello jobb oldali toochange hello alapértelmezett nevének hello szerepkörök.)

    ![Új Cloud Service-projekt](./media/cloud-services-dotnet-get-started/newcsproj.png)
5. Amikor megjelenik a hello **új ASP.NET projekt** párbeszédpanel hello webes szerepkör, válassza ki a hello MVC-sablont, és kattintson **hitelesítés módosítása**.

    ![Hitelesítés módosítása](./media/cloud-services-dotnet-get-started/chgauth.png)
6. A hello **hitelesítés módosítása** párbeszédpanelen válassza ki **nem hitelesítési**, és kattintson a **OK**.

    ![Nincs hitelesítés](./media/cloud-services-dotnet-get-started/noauth.png)
7. A hello **új ASP.NET projekt** párbeszédpanel, kattintson a **OK**.
8. A **Megoldáskezelőben**, kattintson a jobb gombbal a hello megoldás (nem egy hello projektek), és válassza a **Hozzáadás – új projekt**.
9. A hello **új** párbeszédpanelen válassza ki **Windows** alatt **Visual C#** a hello bal oldali ablaktáblán, és kattintson a hello **Class Library** sablon.  
10. Név hello projekt *ContosoAdsCommon*, és kattintson a **OK**.

    Tooreference hello Entity Framework hello és az adatmodell a webes és feldolgozói szerepkör projekt van szüksége. Alternatív megoldásként sikerült hello webes szerepkör projekt hello EF-hez kapcsolódó osztályokat definiálja, és hello feldolgozói szerepkör projekt hivatkozik erre a projektre. De hello alternatív módszert használja, a feldolgozói szerepkör projekt kellene egy tooweb referenciaszerelvények, amely nem igényel.

### <a name="update-and-add-nuget-packages"></a>NuGet-csomagok frissítése és hozzáadása
1. Nyissa meg hello **NuGet-csomagok kezelése** hello megoldás párbeszédpanel.
2. Hello ablak hello tetején válassza **frissítések**.
3. Keresse meg hello *windowsazure.Storage kifejezésre* csomagot, majd ha hello listában, jelölje ki, majd válassza ki a hello webes és feldolgozói projektek tooupdate legyen, és kattintson **frissítés**.

    hello storage ügyféloldali kódtár gyakrabban Visual Studio projektsablonjai, így gyakran tapasztalhatja, hogy hello verziót egy újonnan létrehozott projektben igények toobe frissítése a frissül.
4. Hello ablak hello tetején válassza **Tallózás**.
5. Hello található *EntityFramework* NuGet csomagot, majd telepítse mind a három projektben.
6. Hello található *Microsoft.WindowsAzure.ConfigurationManager* NuGet csomagot, majd telepítse hello feldolgozói szerepkör projektben.

### <a name="set-project-references"></a>A projekt hivatkozásainak beállítása
1. Hello ContosoAdsWeb projektben állítson be egy hivatkozást toohello ContosoAdsCommon projektre. Kattintson a jobb gombbal a ContosoAdsWeb projektben hello, és kattintson **hivatkozások** - **hivatkozások hozzáadása**. A hello **hivatkozáskezelő** párbeszédpanelen jelölje ki **megoldás – projektek** hello bal oldali ablaktáblában jelöljön ki **ContosoAdsCommon**, és kattintson a **OK**.
2. Hello ContosoAdsWorker projektben állítson be egy hivatkozást toohello ContosAdsCommon projektre.

    ContosoAdsCommon tartalmazza hello Entity Framework adatok adatmodellt és a környezeti osztályt, amely mindkét hello előtér- és használják.
3. Hello ContosoAdsWorker projektben állítson be egy hivatkozást túl`System.Drawing`.

    Ez a szerelvény hello háttér-tooconvert képek toothumbnails használják.

### <a name="configure-connection-strings"></a>Csatlakozási karakterláncok konfigurálása
Ebben a szakaszban Azure Storage- és SQL-kapcsolati sztringeket fog konfigurálni helyi tesztelés céljából. hello korábbi telepítési utasításai hello az oktatóanyag azt ismertetik, hogyan hello kapcsolat tooset karakterláncok a hello alkalmazás futtatásakor hello felhőben.

1. Hello ContosoAdsWeb projektre, nyissa meg hello alkalmazás Web.config fájljában és insert hello következő `connectionStrings` elem után hello `configSections` elemet.

    ```xml
    <connectionStrings>
        <add name="ContosoAdsContext" connectionString="Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
    </connectionStrings>
    ```

    A Visual Studio 2015 vagy újabb használata esetén cserélje le a „v11.0” elemet az „MSSQLLocalDB” elemre.
2. Mentse a módosításokat.
3. Hello ContosoAdsCloudService projektben kattintson a jobb gombbal a ContosoAdsWeb **szerepkörök**, és kattintson a **tulajdonságok**.

    ![Szerepkör tulajdonságai](./media/cloud-services-dotnet-get-started/roleproperties.png)
4. A hello **Conosoadsweb [szerepkör]** tulajdonságai ablakban maradva kattintson hello **beállítások** fülre, majd **beállítás hozzáadása**.

    Hagyja **szolgáltatáskonfiguráció** túl beállítása**összes konfiguráció**.
5. Adjon hozzá egy *StorageConnectionString* névvel ellátott beállítást. Állítsa be **típus** túl*ConnectionString*, és állítsa be **érték** túl*UseDevelopmentStorage = true*.

    ![Új kapcsolati karakterlánc](./media/cloud-services-dotnet-get-started/scall.png)
6. Mentse a módosításokat.
7. Hajtsa végre hello azonos eljárás tooadd egy tárolási kapcsolat karakterláncát a hello Contosoadsweb szerepkör tulajdonságaihoz.
8. Még tart a hello **ContosoAdsWorker [szerepkör]** tulajdonságai ablakban maradva adja hozzá egy másik kapcsolati karakterláncot:

   * Név: ContosoAdsDbConnectionString
   * Típus: Karakterlánc
   * Érték: Illessze be hello azonos hello webes szerepkör projekt esetében használt kapcsolati karakterlánc. (a Visual Studio 2013 a hello a következő példa. Ne feledje toochange hello adatforrás Ha ezt a példát, és a Visual Studio 2015-ös vagy újabb rendszer használata esetén.)

       ```
       Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;
       ```

### <a name="add-code-files"></a>Kódfájlok hozzáadása
Ebben a szakaszban, átmásolja kódfájlok hello letöltött megoldásból hello új megoldás. hello következő szakaszok bemutatják és ismertetik a kód legfontosabb részeit.

tooadd fájlok tooa projekt vagy egy mappát, kattintson a jobb gombbal hello projekt vagy mappa és kattintson **Hozzáadás** - **meglévő cikk**. Válassza ki a hello kívánt fájlokat, és kattintson a **Hozzáadás**. Ha a rendszer rákérdez, hogy kívánja-e tooreplace meglévő fájlokat, kattintson a **Igen**.

1. Hello ContosoAdsCommon projektben törölje a hello *Class1.cs* fájlt, és a hely hello *Ad.cs* és *ContosoAdscontext.cs* hello fájlok letöltése.
2. Hello ContosoAdsWeb projektben adja hozzá a következő fájlok hello letöltött projektből hello.

   * *Global.asax.cs*.  
   * A hello *Views\Shared* mappa:  *\_Layout.cshtml*.
   * A hello *Views\Home* mappa: *Index.cshtml*.
   * A hello *tartományvezérlők* mappa: *AdController.cs*.
   * A hello *Views\Ad* mappában (először hozza létre hello mappa): öt *.cshtml* fájlokat.
3. Hello ContosoAdsWorker projektben adja hozzá *WorkerRole.cs* hello a letöltött projektet.

Most már létrehozhatja és hello alkalmazás futtatásához hello az oktatóanyag korábbi utasításai szerint, és hello alkalmazás fogja használni a helyi adatbázis és a storage emulator erőforrásait.

hello alábbi szakaszok ismertetik a hello kód a kapcsolódó tooworking hello Azure-környezetéhez, blobokat és üzenetsorokat. Ez az oktatóanyag nem tartalmazza az hogyan toocreate MVC-vezérlők és nézetek használatával állványok, toowrite Entity Framework-kód, amelyek működése a SQL Server-adatbázisok, vagy az ASP.NET 4.5 aszinkron programozás alapjait hello. Ezek a témakörök kapcsolatos információkért lásd: a következő erőforrások hello:

* [Bevezetés az MVC 5 használatába](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)
* [Bevezetés az EF 6 és az MVC 5 használatába](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc)
* [Bevezetés tooasynchronous programozásba a .NET 4.5](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async).

### <a name="contosoadscommon---adcs"></a>ContosoAdsCommon – Ad.cs
hello Ad.cs fájl megad, egy felsorolást a kategóriákhoz és egy POCO entitásosztályt a hirdetés információihoz.

```csharp
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
```

### <a name="contosoadscommon---contosoadscontextcs"></a>ContosoAdsCommon – ContosoAdsContext.cs
hello ContosoAdsContext osztály megadja, hogy hello Ad osztály egy DbSet gyűjteményben, amely Entity Framework SQL-adatbázisban tárolja történjen.

```csharp
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
```

hello osztály két konstruktorral rendelkezik. először hello hello webes projekt használja valamelyiket, és annak hello hello Web.config fájlban tárolt kapcsolati karakterlánc nevét adja meg. hello második konstruktor lehetővé teszi toopass hello tényleges kapcsolati karakterlánc hello feldolgozói szerepkör projekt által használt, mivel nem rendelkezik a Web.config fájlt. A korábban Ha a kapcsolati karakterlánc tárolásának, és láthatja, hogyan később hello kód keresi hello kapcsolati karakterlánc elindítja hello DbContext osztályt.

### <a name="contosoadsweb---globalasaxcs"></a>ContosoAdsWeb – Global.asax.cs
Hello meghívott kód `Application_Start` hoz létre egy *képek* blobtárolót és egy *képek* üzenetsort, amennyiben még nem léteznek. Ez biztosítja, hogy valahányszor új tárfiókot használ, vagy indításakor hello storage emulator használatával új számítógépre, hello szükséges blobtároló és üzenetsor automatikusan létrejön.

kód lekérdezi hozzáférés toohello tárfiók hello származó hello hello tárolási kapcsolati karakterlánc használatával *.cscfg* fájlt.

```csharp
var storageAccount = CloudStorageAccount.Parse
    (RoleEnvironment.GetConfigurationSettingValue("StorageConnectionString"));
```

Ezután egy hivatkozás toohello *képek* blobtárolóhoz, létrehozza a hello tárolót, ha még nem létezik, és beállítja a hozzáférési engedélyeket hello új tárolóra. Alapértelmezés szerint új tárolók csak rendelkező ügyfeleknek engedélyezik a tárfiók hitelesítő adatok tooaccess blobokat. hello webhely kell hello toobe nyilvános blobok, úgy, hogy az URL-címekkel, hogy pont toohello kép blobok képeket jeleníthessen meg.

```csharp
var blobClient = storageAccount.CreateCloudBlobClient();
var imagesBlobContainer = blobClient.GetContainerReference("images");
if (imagesBlobContainer.CreateIfNotExists())
{
    imagesBlobContainer.SetPermissions(
        new BlobContainerPermissions
        {
            PublicAccess =BlobContainerPublicAccessType.Blob
        });
}
```

Hasonló kód jogosultságot kap a hivatkozás toohello *képek* várólistára, és létrehoz egy új üzenetsort. Ebben az esetben nincs szükség az engedélyek módosítására.

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
var imagesQueue = queueClient.GetQueueReference("images");
imagesQueue.CreateIfNotExists();
```

### <a name="contosoadsweb---layoutcshtml"></a>ContosoAdsWeb – \_Layout.cshtml
Hello *_Layout.cshtml* fájl hello alkalmazásnév beállítja az hello fejlécében és láblécében, és létrehoz egy "Ads" menübejegyzést.

### <a name="contosoadsweb---viewshomeindexcshtml"></a>ContosoAdsWeb – Views\Home\Index.cshtml
Hello *Views\Home\Index.cshtml* fájl kategóriahivatkozásokat jelenít meg hello kezdőlapján. hello hivatkozások átadják hello egész értéket hello `Category` felsorolás egy lekérdezési karakterlánc változó toohello Ads indexlapnak a.

```razor
<li>@Html.ActionLink("Cars", "Index", "Ad", new { category = (int)Category.Cars }, null)</li>
<li>@Html.ActionLink("Real estate", "Index", "Ad", new { category = (int)Category.RealEstate }, null)</li>
<li>@Html.ActionLink("Free stuff", "Index", "Ad", new { category = (int)Category.FreeStuff }, null)</li>
<li>@Html.ActionLink("All", "Index", "Ad", null, null)</li>
```

### <a name="contosoadsweb---adcontrollercs"></a>ContosoAdsWeb – AdController.cs
A hello *AdController.cs* fájl, hello konstruktor hívások hello `InitializeStorage` metódus toocreate Azure Storage ügyféloldali kódtár objektumok, amelyek az API-k használata a blobokat és üzenetsorokat.

Ezután hello kód jogosultságot kap a hivatkozás toohello *képek* blobtárolóra, ahogy azt korábban a *Global.asax.cs*. Mindeközben beállít egy webalkalmazásokhoz használható alapértelmezett [újrapróbálkozási házirendet](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling). hello alapértelmezett exponenciális leállítási újrapróbálkozási házirend le több mint egy perc egy átmeneti hiba miatti ismételt próbálkozás hello webalkalmazás. Itt megadott hello újrapróbálkozási házirend minden próbálkozás mentése toothree próbálkozás után három másodperc alatt vár.

```csharp
var blobClient = storageAccount.CreateCloudBlobClient();
blobClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
imagesBlobContainer = blobClient.GetContainerReference("images");
```

Hasonló kód jogosultságot kap a hivatkozás toohello *képek* várólista.

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
queueClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
imagesQueue = queueClient.GetQueueReference("images");
```

A legtöbb hello vezérlőkód jellemzően egy DbContext osztályt használó Entity Framework-adatmodell. Egy kivétel: hello HttpPost `Create` metódus, amely feltölt egy fájlt, és menti a blob Storage tárolóban. hello biztosít egy [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx) toohello metódus objektum.

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<ActionResult> Create(
    [Bind(Include = "Title,Price,Description,Category,Phone")] Ad ad,
    HttpPostedFileBase imageFile)
```

Egy fájl tooupload hello felhasználói választásakor hello kód feltölt hello fájlt, egy blobba menti, és frissíti a hirdetés adatbázisrekordot hello toohello blob URL-CÍMMEL.

```csharp
if (imageFile != null && imageFile.ContentLength != 0)
{
    blob = await UploadAndSaveBlobAsync(imageFile);
    ad.ImageURL = blob.Uri.ToString();
}
```

hello kódot, amely hello feltöltés van hello `UploadAndSaveBlobAsync` metódust. Létrehoz egy GUID nevet hello blob, a feltöltéseket és az eredményobjektumokat hello fájl, és egy hivatkozási mentve toohello blob adja vissza.

```csharp
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
```

Miután a hello HttpPost `Create` metódus feltölt egy blobot és frissítések hello adatbázist, létrehoz egy várólista üzenet tooinform, hogy egy kép készen áll a átalakítás tooa miniatűr háttér-folyamat.

```csharp
string queueMessageString = ad.AdId.ToString();
var queueMessage = new CloudQueueMessage(queueMessageString);
await queue.AddMessageAsync(queueMessage);
```

hello HttpPost kódját hello `Edit` módszer a hasonló, azzal a különbséggel, hogy ha hello felhasználó kiválaszt egy új képfájlt minden létező blobot törölni kell.

```csharp
if (imageFile != null && imageFile.ContentLength != 0)
{
    await DeleteAdBlobsAsync(ad);
    imageBlob = await UploadAndSaveBlobAsync(imageFile);
    ad.ImageURL = imageBlob.Uri.ToString();
}
```

hello következő példa bemutatja, blobok törlése, ha töröl egy ad hello kódját.

```csharp
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
```

### <a name="contosoadsweb---viewsadindexcshtml-and-detailscshtml"></a>ContosoAdsWeb – Views\Ad\Index.cshtml és Details.cshtml
Hello *Index.cshtml* fájlt tartalmazó miniatűrt jelenít meg hello hirdetés többi adatát.

```razor
<img src="@Html.Raw(item.ThumbnailURL)" />
```

Hello *Details.cshtml* fájl hello teljes méretű képet jeleníti meg.

```razor
<img src="@Html.Raw(Model.ImageURL)" />
```

### <a name="contosoadsweb---viewsadcreatecshtml-and-editcshtml"></a>ContosoAdsWeb – Views\Ad\Create.cshtml és Edit.cshtml
Hello *Create.cshtml* és *Edit.cshtml* fájlok megadják az űrlap kódolását, hogy a lehetővé teszi, hogy a tartományvezérlő tooget hello hello `HttpPostedFileBase` objektum.

```razor
@using (Html.BeginForm("Create", "Ad", FormMethod.Post, new { enctype = "multipart/form-data" }))
```

Egy `<input>` elem jelzi hello böngésző tooprovide egy Fájlkiválasztási párbeszédpanelt.

```razor
<input type="file" name="imageFile" accept="image/*" class="form-control fileupload" />
```

### <a name="contosoadsworker---workerrolecs---onstart-method"></a>ContosoAdsWorker – WorkerRole.cs – OnStart metódus
hello Azure feldolgozói szerepkör környezet meghívja a hello `OnStart` metódus a hello `WorkerRole` indulásakor hello feldolgozói szerepkör, és meghívja hello `Run` metódusban, amikor hello `OnStart` metódus befejeződik.

Hello `OnStart` metódus hello adatbázis-kapcsolati karakterlánc lekérése hello *.cscfg* fájlt, és átadja toohello Entity Framework DbContext osztálynak. hello SQLClient szolgáltató alapértelmezés szerint van használatban, így hello szolgáltató nincs megadva toobe.

```csharp
var dbConnString = CloudConfigurationManager.GetSetting("ContosoAdsDbConnectionString");
db = new ContosoAdsContext(dbConnString);
```

Ezt követően hello metódus lekérdezi a hivatkozás toohello tárfiók, és hello blobtároló és üzenetsor létrehozása, ha azok még nem léteznek. hello tartozó kód hasonló toowhat hello webes szerepkör már látott `Application_Start` metódust.

### <a name="contosoadsworker---workerrolecs---run-method"></a>ContosoAdsWorker – WorkerRole.cs – Run metódus
Hello `Run` metódus lehívásra kerül, ha hello `OnStart` metódus inicializálási feladatának befejezése után. hello metódus elindít egy végtelen ciklust, amely új üzeneteit figyeli, és feldolgozza őket, amikor azok beérkeznek.

```csharp
public override void Run()
{
    CloudQueueMessage msg = null;

    while (true)
    {
        try
        {
            msg = this.imagesQueue.GetMessage();
            if (msg != null)
            {
                ProcessQueueMessage(msg);
            }
            else
            {
                System.Threading.Thread.Sleep(1000);
            }
        }
        catch (StorageException e)
        {
            if (msg != null && msg.DequeueCount > 5)
            {
                this.imagesQueue.DeleteMessage(msg);
            }
            System.Threading.Thread.Sleep(5000);
        }
    }
}
```

Hello ciklus egyes ismétlései után található nincs üzenet, ha hello program alvó állapotban marad, a második. Ez megakadályozza, hogy a hello feldolgozói szerepkör túl sok CPU idő- és tárolási tranzakciós költségeket halmozzon. hello Microsoft Ügyféltanácsadói csapatának van egy története egy fejlesztőről, aki elfelejtette tooinclude, tooproduction telepíti, és elment nyaralni. Mire vissza, a figyelmetlensége többe hello szabadsága-nál.

A várólista üzenet tartalma hello néha hibát okoz a feldolgozása. Ez a lehetőség egy *az elhalt üzenet*, és ha csak naplózott egy hibát és hello hurok újraindul, akkor feldolgozásával a végtelenségig próbálkozhat tooprocess üzenetet.  Ezért hello catch blokk tartalmaz egy if utasítást, amely ellenőrzi a toosee hányszor hello app megpróbálta tooprocess hello aktuális üzenetet, és ha több mint 5-ször volt, üdvözlőüzenetére törli a várólistáról hello.

`ProcessQueueMessage`akkor lesz meghívva, ha az üzenetsorban található üzenet.

```csharp
private void ProcessQueueMessage(CloudQueueMessage msg)
{
    var adId = int.Parse(msg.AsString);
    Ad ad = db.Ads.Find(adId);
    if (ad == null)
    {
        throw new Exception(String.Format("AdId {0} not found, can't create thumbnail", adId.ToString()));
    }

    CloudBlockBlob inputBlob = this.imagesBlobContainer.GetBlockBlobReference(ad.ImageURL);

    string thumbnailName = Path.GetFileNameWithoutExtension(inputBlob.Name) + "thumb.jpg";
    CloudBlockBlob outputBlob = this.imagesBlobContainer.GetBlockBlobReference(thumbnailName);

    using (Stream input = inputBlob.OpenRead())
    using (Stream output = outputBlob.OpenWrite())
    {
        ConvertImageToThumbnailJPG(input, output);
        outputBlob.Properties.ContentType = "image/jpeg";
    }

    ad.ThumbnailURL = outputBlob.Uri.ToString();
    db.SaveChanges();

    this.imagesQueue.DeleteMessage(msg);
}
```

Ez a kód beolvassa hello adatbázis tooget hello kép URL-címe, hello kép tooa miniatűr alakít, hello miniatűrt egy blobba menti, frissíti hello adatbázist hello miniatűr blob URL- és hello üzenetsor törli.

> [!NOTE]
> hello kód hello `ConvertImageToThumbnailJPG` metódus hello System.Drawing névtérben-osztályokat használja az egyszerűség érdekében. A névtérben lévő osztályok hello azonban a Windows-űrlapokkal való használatra lettek tervezve. Windows- vagy ASP.NET-szolgáltatásban való használatuk nem támogatott. További információk a képfeldolgozási beállításokról: [Dynamic Image Generation](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) (Dinamikus képek létrehozása) és [Deep Inside Image Resizing](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na) (A képek átméretezésének részletei).
>
>

## <a name="troubleshooting"></a>Hibaelhárítás
Abban az esetben, ha valami nem működik, ebben az oktatóanyagban hello utasítások követése, az alábbiakban néhány gyakori hibák, és hogyan tooresolve őket.

### <a name="serviceruntimeroleenvironmentexception"></a>ServiceRuntime.RoleEnvironmentException
Hello `RoleEnvironment` objektum Azure által biztosított, az alkalmazás az Azure-ban való futtatásakor, vagy helyileg a hello Azure compute emulator használatával futtatásakor.  Ha ez a hiba a helyi futtatás során, akkor győződjön meg arról, hogy hello ContosoAdsCloudService projektet állította hello indítási projektként. Hello projekt toorun hello Azure compute emulator használatával állít be.

Hello alkalmazás által használt hello Azure roleenvironment-et hello dolog tooget hello kapcsolati karakterlánc-értékek hello tárolt *.cscfg* fájlokat, ezért ez a kivétel egy másik oka egy hiányzó kapcsolati karakterlánc. Győződjön meg arról, hogy létrehozta hello StorageConnectionString beállítást felhő és a helyi konfigurációiban hello ContosoAdsWeb projektben, és mindkét kapcsolati karakterláncot mindkét konfigurációjában létrehozta hello ContosoAdsWorker projektben. Ha így tesz egy **található összes** keresési StorageConnectionString hello teljes megoldásban, a kell megjelennie 9 alkalommal 6 fájlban.

### <a name="cannot-override-tooport-xxx-new-port-below-minimum-allowed-value-8080-for-protocol-http"></a>Tooport xxx metódus nem bírálhatja felül. Az új port a HTTP protokoll esetében megengedett legalacsonyabb, 8080 érték alatt van
Próbálja meg módosítani a hello hello webes projekt által használt portszámot. Kattintson a jobb gombbal a ContosoAdsWeb projektben hello, és kattintson **tulajdonságok**. Kattintson a hello **webes** lapra, és módosítsa a portszámot hello hello **projekt URL-címe** beállítást.

Alternatív hello probléma megoldására tekintse meg a következő szakasz hello.

### <a name="other-errors-when-running-locally"></a>A helyi futtatás során felmerülő egyéb hibák
Alapértelmezett új cloud service projektek hello Azure compute emulator express toosimulate hello Azure környezetben használják. Ez hello teljes compute emulator egyszerűsített verziója, és bizonyos feltételek hello a teljes emulátor fognak működni, amikor hello express verzió nem.  

toochange hello projekt toouse hello teljes emulátor, kattintson a jobb gombbal a ContosoAdsCloudService projekt hello, és kattintson a **tulajdonságok**. A hello **tulajdonságok** ablakban kattintson a hello **webes** fülre, majd hello **teljes emulátor használata** választógombot.

Rendelés toorun hello alkalmazás hello teljes emulátorral tooopen Visual Studio rendszergazdai jogosultságokkal rendelkezik.

## <a name="next-steps"></a>Következő lépések
hello Contoso Ads alkalmazás kialakítása szándékosan egyszerű-bevezető oktatóanyag. Például az nem implementálja a következőt [függőségi beszúrást](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) vagy hello [adattárát és egységét működik minták](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), nem [használ felületet a naplózáshoz](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), nemhasznál[ EF Code First áttelepítést](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage adatok modell módosítások vagy [EF-kapcsolati rugalmasságot](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage átmeneti hálózati hibák, és így tovább.

Az alábbiakban néhány felhőszolgáltatás-alkalmazásokra, amelyek több valós kódolási gyakorlatot alkalmaz, az összetett kevésbé összetett toomore felsorolt bemutatása:

* [PhluffyFotos](http://code.msdn.microsoft.com/PhluffyFotos-Sample-7ecffd31). Hasonlít a tooContoso Ads elvéhez, de több funkciót és több valós kódolási gyakorlatot alkalmaz.
* [Azure Cloud Service Multi-Tier Application with Tables, Queues, and Blobs](http://code.msdn.microsoft.com/windowsazure/Windows-Azure-Multi-Tier-eadceb36) (Többrétegű Azure-felhőszolgáltatás táblákkal, üzenetsorokkal és blobokkal). Ismerteti az Azure Storage-táblákat, valamint a blobokat és üzenetsorokat. A .NET-alapú hello Azure SDK-t egy régebbi verziójú, néhány módosítás toowork hello aktuális verziójával van szükség.
* [Cloud Service Fundamentals in Microsoft Azure](http://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649) (A Felhőszolgáltatás alapjai a Microsoft Azure-ban). Gyakorlati tanácsok hello Microsoft Patterns és gyakorlatok csoportja által előállított széles skáláját bemutató, átfogó példák.

Hello felhő fejlesztésével kapcsolatos általános információkért lásd: [épület valós Felhőalkalmazások Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction).

A videó tooAzure tárolási az ajánlott eljárások és minták című [Microsoft Azure Storage –, ajánlott eljárások és minták](http://channel9.msdn.com/Events/Build/2014/3-628).

További információkért tekintse meg a következő erőforrások hello:

* [Azure Cloud Services – 1. rész: Bevezetés](http://justazure.com/microsoft-azure-cloud-services-part-1-introduction/)
* [Toomanage Cloud Services](cloud-services-how-to-manage.md)
* [Azure Storage](/documentation/services/storage/)
* [Hogyan toochoose felhő szolgáltató](https://azure.microsoft.com/overview/choosing-a-cloud-service-provider/)
