---
title: "aaaTroubleshoot egy webalkalmazást az Azure App Service szolgáltatásban a Visual Studio használatával"
description: "Ismerje meg, hogyan tootroubleshoot Azure-webalkalmazás távoli hibakeresés használatával, nyomon követését, és a naplózási eszközök beépített tooVisual Studio 2013."
services: app-service
documentationcenter: .net
author: tdykstra
manager: erikre
editor: 
ms.assetid: def8e481-7803-4371-aa55-64025d116c97
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/29/2016
ms.author: rachelap
ms.openlocfilehash: 8e3a8a58293f2ebcdc131fbf2534f8ff99b26730
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-web-app-in-azure-app-service-using-visual-studio"></a>Az Azure App Service szolgáltatásban a Visual Studio használatával webes alkalmazás hibaelhárítása
## <a name="overview"></a>Áttekintés
Ez az oktatóanyag bemutatja, hogyan a toouse Visual Studio eszközök, amelyek segítenek a webes alkalmazás hibakeresése [App Service](http://go.microsoft.com/fwlink/?LinkId=529714), való futtatásával [hibakeresési mód](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) távolról vagy alkalmazásnaplók és a webkiszolgáló naplóinak megtekintésével.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

Az oktatóanyagból a következőket sajátíthatja el:

* Az Azure web app felügyeleti funkciókról a Visual Studio érhetők el.
* Hogyan toouse Visual Studio távoli toomake gyors módosításokat megtekintheti a távoli webes alkalmazás.
* Hogyan toorun hibakeresési mód távolról projekt közben fut, az Azure-ban egy webalkalmazást, valamint az webjobs-feladat.
* Hogyan toocreate alkalmazás nyomkövetési naplóit, és megtekintheti azokat, amíg a hello alkalmazás hozza létre őket.
* Hogyan tooview webkiszolgáló naplóinak, beleértve a részletes hibaüzeneteket, és a sikertelen kérelmek nyomkövetése.
* Hogyan toosend diagnosztikai tooan Azure Storage-fiók naplózza, és megtekintheti, azokat.

Ha a Visual Studio Ultimate, használhatja [IntelliTrace](http://msdn.microsoft.com/library/vstudio/dd264915.aspx) a hibakereséshez. IntelliTrace ebben az oktatóanyagban nem jelez.

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag együttműködve hello fejlesztési környezet, a webes projekt és az Azure-webalkalmazásban a beállított [Ismerkedés az Azure és az ASP.NET][GetStarted]. Hello WebJobs szakaszokat, szüksége lesz a létrehozott alkalmazás hello [Ismerkedés az Azure WebJobs SDK hello][GetStartedWJ].

Ebben az oktatóanyagban minták C# MVC webalkalmazás, de a hibaelhárítási eljárásokkal hello hello kód vannak hello ugyanabban a Visual Basic és a Web Forms-alkalmazásokhoz.

hello oktatóanyag feltételezi a Visual Studio 2015-öt vagy 2013-at használ. Visual Studio 2013 használata, hello WebJobs funkciókhoz [Update 4](http://go.microsoft.com/fwlink/?LinkID=510314) vagy újabb.

hello folyamatos átviteli naplók csak akkor működik a .NET-keretrendszer 4-es vagy újabb alkalmazások funkció.

## <a name="sitemanagement"></a>Webes alkalmazás konfigurálása és kezelése
A Visual Studio biztosít hozzáférést tooa részhalmazát hello web app felügyeleti funkciókat és hello konfigurációs beállításai [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715). Ebben a szakaszban láthatja, mi érhető el a **Server Explorer**. toosee hello legújabb Azure integrációs szolgáltatások kipróbálására **Cloud Explorer** is. A hello nyithatja meg mindkét windows **nézet** menü.

1. Ha még nem jelentkezett a Visual Studio tooAzure, kattintson a hello **tooAzure csatlakozás** gombra **Server Explorer**.

    A másik lehetőség az tooinstall egy felügyeleti tanúsítványt, hogy lehetővé teszi, hogy tooyour fiók eléréséhez. Ha tooinstall válasszon egy tanúsítványt, kattintson a jobb gombbal hello **Azure** csomópontja **Server Explorer**, és kattintson a **kezelése és a szűrő előfizetések** hello helyi menü. A hello **Azure-előfizetések kezelése** párbeszédpanelen kattintson hello **tanúsítványok** fülre, majd **importálása**. Kövesse az utasításokat toodownload hello és importálhatja az előfizetés-fájl (más néven a *.publishsettings* fájl) Azure-fiókját.

   > [!NOTE]
   > A előfizetés-fájl letöltésére, ha tooa mappát (például a hello Letöltések mappába), a forrás kód címtárak kívül mentheti, és törölje azt hello importálás befejezése után. Egy rosszindulatú felhasználó kap hozzáférést toohello előfizetési fájl szerkesztése, létrehozása és törlése az Azure-szolgáltatások.
   >
   >

    A Visual Studio tooAzure erőforrások csatlakoztatása kapcsolatos további információkért lásd: [fiókok kezelése, előfizetések és rendszergazdai szerepkörök](http://go.microsoft.com/fwlink/?LinkId=324796#BKMK_AccountVCert).
2. A **Server Explorer**, bontsa ki a **Azure** csomópontot **App Service**.
3. Bontsa ki a létrehozott webalkalmazás hello tartalmazó erőforráscsoportot hello [Ismerkedés az Azure és az ASP.NET][GetStarted], majd kattintson a jobb gombbal a hello web app csomópont, és kattintson a **beállításainakmegjelenítése**.

    ![A Server Explorer beállításainak megjelenítése](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewsettings.png)

    Hello **Azure Web Apps** lap jelenik meg, és láthatja, hogy nincs hello web app felügyeleti és konfigurációs műveleteket a Visual Studio elérhető.

    ![Az Azure Web App ablak](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-configtab.png)

    Ebben az oktatóanyagban fogja használni hello naplózás és nyomkövetés legördülő listákat. Is alkalmazhatja távoli hibakeresés, de egy másik módszer tooenable fogja használni azt.

    Ebben az ablakban Alkalmazásbeállítások és kapcsolati karakterláncok mezőkbe hello kapcsolatos információkért lásd: [Azure Web Apps alkalmazások: hogyan alkalmazás karakterláncok és a kapcsolati karakterláncok munkahelyi](http://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

    Ha azt szeretné, hogy egy webes alkalmazás feladatot, amely nem hajtható végre ebben az ablakban tooperform, kattintson a **nyissa meg a felügyeleti portál** tooopen egy böngésző ablak toohello Azure-portálon.

## <a name="remoteview"></a>Web Server Explorer alkalmazás fájlok elérése
Általában központi telepítése a webes projekthez hello `customErrors` túl megjelölésre kerülnek a Web.config fájl set hello`On` vagy `RemoteOnly`, ami azt jelenti, nem kap egy hasznos hibaüzenet, ha valami hibás működés vagy művelet. Sok hiba esetén összes le azokat, a következő hello hasonló oldal.

**Kiszolgálóhiba a "/" alkalmazás:**

![Unhelpful hibaüzenetet megjelenítő lap](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror.png)

**Hiba történt:**

![Unhelpful hibaüzenetet megjelenítő lap](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror1.png)

**hello webhely a hello lap nem jeleníthető meg**

![Unhelpful hibaüzenetet megjelenítő lap](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror2.png)

Gyakran hello legegyszerűbb módja toofind hello hello hiba oka tooenable részletes hibaüzeneteket, amelyek képernyőképeket megelőző hello első hello ismerteti, hogyan toodo. Amely a hello módosítják a Web.config fájl telepítésére van szükség. Módosíthatja a hello *Web.config* hello projektet, és helyezze üzembe újra hello projekt fájlt, vagy hozzon létre egy [Web.config transzformálása](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) és hibakeresési buildet telepítsen, de gyorsabban: a **megoldás Explorer** közvetlenül megtekintése és szerkesztése a fájlok hello távoli web app alkalmazásban hello segítségével *távoli nézet* szolgáltatás.

1. A **Server Explorer**, bontsa ki a **Azure**, bontsa ki a **App Service**, és bontsa ki a webalkalmazás található hello erőforráscsoportot, majd bontsa ki a webalkalmazás hello csomópont.

    Megjelenik a csomópontokra, amelyeket hozzáférés toohello web app tartalomfájlokat, és a naplófájlokat.
2. Bontsa ki a hello **fájlok** csomópont, és kattintson duplán a hello *Web.config* fájlt.

    ![Nyissa meg a Web.config fájlban](./media/web-sites-dotnet-troubleshoot-visual-studio/webconfig.png)

    A Visual Studio hello Web.config fájl megnyitása hello távoli web app és [távoli] következő toohello fájlnév hello címsorában látható.
3. Adja hozzá a következő sor toohello hello `system.web` elem:

    `<customErrors mode="Off"></customErrors>`

    ![Web.config szerkesztése](./media/web-sites-dotnet-troubleshoot-visual-studio/webconfigedit.png)
4. Hello unhelpful hibaüzenet látható hello böngésző frissítése, és most egy részletes hibaüzenetet, például a következő példa hello elérhetővé:

    ![Részletes hibaüzenet](./media/web-sites-dotnet-troubleshoot-visual-studio/detailederror.png)

    (vörösen láthatók túl hello sor hozzáadásával hello hiba látható létrejött*Views\Home\Index.cshtml*.)

Szerkesztési hello Web.config fájl melyik hello lehetősége tooread és a Szerkesztés fájlok az Azure-webalkalmazásban könnyebb legyen a hibaelhárítás forgatókönyvek csak egy példában látható.

## <a name="remotedebug"></a>Távoli hibakeresési webalkalmazások
Hello részletes hibaüzenet nem elegendő információt tartalmaznak, és nem lehet újra létrehozni a helyi hello hiba, ha a másik módja tootroubleshoot távolról toorun hibakeresési módban. Állítson be töréspontokat, kezelheti közvetlenül a memória, kód teljesítéséhez és még a hello kód elérési útja módosítható.

Távoli hibakeresés nem működik a Visual Studio Express kiadásait.

Ez a szakasz bemutatja, hogyan hello segítségével távolról toodebug projekt, akkor hozzon létre [Ismerkedés az Azure és az ASP.NET][GetStarted].

1. A nyitott hello webes projekt [Ismerkedés az Azure és az ASP.NET][GetStarted].
2. Nyissa meg *Controllers\HomeController.cs*.
3. Törölje a hello `About()` helyette kód metódus és a Beszúrás hello következő.

        public ActionResult About()
        {
            string currentTime = DateTime.Now.ToLongTimeString();
            ViewBag.Message = "hello current time is " + currentTime;
            return View();
        }
4. [Állítson be egy töréspontot](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) a hello `ViewBag.Message` sor.
5. A **Megoldáskezelőben**, kattintson a jobb gombbal a hello projektet, és kattintson **közzététel**.
6. A hello **profil** legördülő listában, jelölje be hello azonos profilját, amikor a használt [Ismerkedés az Azure és az ASP.NET][GetStarted].
7. Hello kattintson **beállítások** lapon, és módosítsa **konfigurációs** túl**Debug**, és kattintson a **közzététel**.

    ![Tegye közzé ezt a hibakeresési mód](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-publishdebug.png)
8. A telepítés utáni befejeződik, és a böngésző megnyitja toohello Bezárás hello böngészőben a webalkalmazás Azure URL-CÍMÉT.
9. A **Server Explorer**, kattintson a jobb gombbal a webalkalmazást, és kattintson a **csatolása hibakereső**.

    ![Hibakereső csatlakoztatása](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-attachdebugger.png)

    hello böngésző automatikusan megnyitja az Azure-beli tooyour kezdőlapján. Előfordulhat, hogy van toowait 20 másodperc, vagy úgy során Azure beállítja hello kiszolgálót a hibakereséshez. Ez a késés csak akkor zajlik le hello első alkalommal a webes alkalmazás hibakeresési módban futtatja. Indításakor nincs újra hibakeresés következő 48 órán belül hello későbbi alkalommal nem fogja késést.

    **Megjegyzés:** problémája bármely hello hibakereső indítása, ha próbálja toodo azt használatával **Cloud Explorer** helyett **Server Explorer**.
10. Kattintson a **kapcsolatos** hello menüben.

     A Visual Studio leáll a hello töréspont, és hello kódot nem a helyi számítógépen fut, az Azure-ban.
11. Hello rámutat `currentTime` változó toosee hello idő értéknek megfelelően.

     ![Azure-beli hibakeresési módban nézet változó](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugviewinwa.png)

     hello látja ideje hello Azure-kiszolgáló ideje, ami esetleg egy másik időzónába, mint a helyi számítógépen.
12. Adjon meg új értéket hello `currentTime` változó, például az "Azure-ban már futó".
13. Nyomja le az F5 toocontinue futtatása.

     Azure-beli oldalról hello hello új értéket, akkor való hello currentTime változó megadott jeleníti meg.

     ![Új értékkel rendelkező oldalról](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugchangeinwa.png)

## <a name="remotedebugwj"></a>Távoli hibakeresési webjobs-feladatok
Ez a szakasz bemutatja, hogyan toodebug távolról a hello projektet és a webes alkalmazás használatával hoz létre a [Ismerkedés az Azure WebJobs SDK hello](websites-dotnet-webjobs-sdk.md).

Itt látható hello funkciók csak a Visual Studio 2013 Update 4 vagy újabb érhetők el.

Távoli hibakeresés csak működik együtt a folyamatos webjobs-feladatok. Ütemezett és igény szerinti webjobs-feladatok nem támogatják a hibakeresést.

1. A nyitott hello webes projekt [Ismerkedés az Azure WebJobs SDK hello][GetStartedWJ].
2. Hello ContosoAdsWebJob projektben nyissa meg *Functions.cs*.
3. [Állítson be egy töréspontot](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) a hello következő utasításnak elsőként hello `GnerateThumbnail` metódust.

    ![Set töréspont](./media/web-sites-dotnet-troubleshoot-visual-studio/wjbreakpoint.png)
4. A **Megoldáskezelőben**, kattintson a jobb gombbal a hello webes projekt (nem hello webjobs-feladat projekt), és kattintson a **közzététel**.
5. A hello **profil** legördülő listában, jelölje be hello azonos profilját, amikor a használt [Ismerkedés az Azure WebJobs SDK hello](websites-dotnet-webjobs-sdk.md).
6. Hello kattintson **beállítások** lapon, és módosítsa **konfigurációs** túl**Debug**, és kattintson a **közzététel**.

    A Visual Studio telepíti a hello webes és a webjobs-feladat projektek, és a böngésző megnyitja toohello a webalkalmazás Azure URL-CÍMÉT.
7. A **Server Explorer** bontsa ki a **Azure > az App Service > az erőforráscsoport > a webalkalmazás > WebJobs > Continuous**, és kattintson a jobb gombbal **ContosoAdsWebJob**.
8. Kattintson a **Hibakereső csatlakoztatása**.

    ![Hibakereső csatlakoztatása](./media/web-sites-dotnet-troubleshoot-visual-studio/wjattach.png)

    hello böngésző automatikusan megnyitja az Azure-beli tooyour kezdőlapján. Előfordulhat, hogy van toowait 20 másodperc, vagy úgy során Azure beállítja hello kiszolgálót a hibakereséshez. Ez a késés csak akkor zajlik le hello első alkalommal a webes alkalmazás hibakeresési módban futtatja. hello hello hibakereső van csatolása nem akkor késleltetés, ha így tesz, 48 órán belül.
9. Hello böngésző megnyitott toohello Contoso Ads kezdőlapját hozzon létre egy új ad.

    A várólista üzenet toobe hozott létre, amely hello webjobs-feladat észlelnie és a feldolgozott létrehozása az ad okoz. Amikor hello WebJobs SDK hello függvény tooprocess hello üzenetsor-üzenetet, meghívja, hello kód elért a töréspont megjelenését.
10. Amikor hello hibakereső megsérti a töréspont megjelenését, vizsgálja meg, és változók értékeinek módosítása hello futása hello felhő. Hello a következő ábra hello hibakereső hello blobInfo objektum toohello GenerateThumbnail metódus átadott hello tartalmának mutatja.

     ![a hibakereső blobInfo objektum](./media/web-sites-dotnet-troubleshoot-visual-studio/blobinfo.png)
11. Nyomja le az F5 toocontinue futtatása.

     hello GenerateThumbnail metódus végzett a létrehozással hello miniatűr.
12. Hello böngészőben frissítési hello Index lapra, és tekintse meg hello miniatűr.
13. Visual Studio nyomja le a SHIFT + F5 toostop hibakeresést.
14. A **Server Explorer**, kattintson a jobb gombbal a hello ContosoAdsWebJob csomópont, és kattintson a **irányítópult nézet**.
15. Jelentkezzen be Azure hitelesítő adataival, majd kattintson a webjobs-feladat a hello webjobs-feladat neve toogo toohello oldala.

     ![Kattintson a ContosoAdsWebJob](./media/web-sites-dotnet-troubleshoot-visual-studio/clickcaw.png)

     hello irányítópult jeleníti meg, hogy hello nemrég végrehajtott GenerateThumbnail függvény.

     (amikor legközelebb hello **irányítópult nézet**, nem rendelkezik a toosign és hello böngésző vonatkozik közvetlenül toohello lap a webjobs-feladat.)
16. Kattintson a hello függvény neve toosee adatait hello függvény végrehajtása.

     ![Függvény részletei](./media/web-sites-dotnet-troubleshoot-visual-studio/funcdetails.png)

Ha a függvény [naplók megírt](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs), sikerült kattintson **ToggleOutput** toosee őket.

## <a name="notes-about-remote-debugging"></a>Távoli hibakereséssel kapcsolatos megjegyzések
* Éles hibakeresési módban nem ajánlott. Az éles webes alkalmazás nem horizontálisan toomultiple kiszolgálópéldányok, ha a hibakeresés megakadályozza hello webkiszolgáló válaszol tooother kérések. Ha több web server-példány, a véletlenszerű példánya, és kaphat toohello hibakereső csatolása rendelkezik nincs módja tooensure későbbi böngészőben kérelmek toothat példány kerül. Is általában nem kell telepíteni a hibakeresési buildet tooproduction, és fordító optimalizálásokat kiadás buildek esetében előfordulhat, hogy teszi lehetetlen tooshow, mi történik soronként a forráskód. A termelési problémák elhárításához, a legjobb erőforrás alkalmazás nyomkövetés és web server-naplók.
* Hosszú leáll, amikor távoli töréspontok elkerülése hibakeresést. Azure értékként kezelje-e a folyamat, amely le van állítva, több mint egy nem válaszoló folyamatként néhány percig, és leállítja azt.
* Hibakeresése, közben hello kiszolgáló küld adatokat tooVisual Studio, amelyek hatással lehetnek a sávszélesség-költségek. Sávszélesség díjszabás kapcsolatos információkért lásd: [Azure díjszabása](https://azure.microsoft.com/pricing/calculator/).
* Győződjön meg arról, hogy hello `debug` hello attribútumának `compilation` hello elemének *Web.config* fájl tootrue van beállítva. Alapértelmezés szerint tootrue van állítva egy hibakeresési buildet konfigurációs közzétételekor.

        <system.web>
          <compilation debug="true" targetFramework="4.5" />
          <httpRuntime targetFramework="4.5" />
        </system.web>
* Ha talál meg, hogy hello hibakereső nem lépjen be a kódot, amelyet az toodebug, lehetséges, hogy toochange hello csak saját kód beállítást.  További információkért lásd: [almodell tooJust saját kód korlátozása](http://msdn.microsoft.com/library/vstudio/y740d9d3.aspx#BKMK_Restrict_stepping_to_Just_My_Code).
* Egy időzítő indításakor hello kiszolgálón hello távoli hibakeresési szolgáltatás engedélyezése és 48 óra elteltével hello szolgáltatás automatikusan ki van kapcsolva. Ezt a határt 48 óra biztonsági és teljesítménynövelő okokból kell elvégezni. Könnyen bekapcsolása hello szolgáltatás vissza annyi alkalommal. Azt javasoljuk, hogy le van tiltva, akkor nem aktívan hibakeresése esetén hagyja.
* Manuálisan csatolhat hello hibakereső tooany folyamatot, nem csak az hello web app folyamat (w3wp.exe). Hogyan toouse hibakeresési mód a Visual Studio kapcsolatos további információkért lásd: [a Visual Studio hibakeresési](http://msdn.microsoft.com/library/vstudio/sc65sadd.aspx).

## <a name="logsoverview"></a>Diagnosztikai naplók – áttekintés
Az ASP.NET-alkalmazások az Azure-webalkalmazásban futó hozhat létre a következő naplók típusú hello:

* **Alkalmazás nyomkövetési naplók**<br/>
  hello alkalmazás hoz létre a naplók hello módszerek meghívásával [System.Diagnostics.Trace](http://msdn.microsoft.com/library/system.diagnostics.trace.aspx) osztály.
* **Webkiszolgáló naplóinak**<br/>
  hello webkiszolgáló hoz létre a naplóbejegyzés minden HTTP-kérelem toohello webes alkalmazást.
* **Részletes hibanaplókat üzenet**<br/>
  hello webkiszolgáló HTML-lapot hoz létre a sikertelen HTTP-kérelmek (amelyek állapotkód: 400 vagy nagyobb eredményező) további információkra.
* **Sikertelen kérelmek nyomkövetési naplóit**<br/>
  a sikertelen HTTP-kérelmekre részletes nyomkövetési adatok XML-fájl hoz létre a hello webkiszolgáló. hello webkiszolgáló is kínál az XSL fájlt tooformat hello XML a böngészőben.

Naplózás hatással van a webes alkalmazás teljesítménye, így Azure lehetővé teszi az képességét tooenable hello, vagy tiltsa le a napló típusonkénti, igény szerint. Alkalmazásnaplók megadhatja, hogy csak egy bizonyos súlyossági szint fölé naplók kell írni. Amikor hoz létre egy új webalkalmazást, alapértelmezés szerint az összes naplózás le van tiltva.

Írja a naplókat toofiles egy *naplófájlok* mappa hello fájlrendszer a webalkalmazást és FTP-n keresztül érhető el. Webkiszolgáló naplóinak és az alkalmazásnaplók is írhatók tooan Azure Storage-fiók. A storage-fiók, mint lehetséges hello fájlrendszer-naplók nagyobb mennyiségű őrizheti meg. Hello fájlrendszer használatakor Ön korlátozott tooa legfeljebb 100 megabájt naplókat. (Csak a rövid távú megőrzési fájl rendszer naplóit is. Azure törli a régi naplófájl fájlok toomake újaknak hello határérték elérése után.)  

## <a name="apptracelogs"></a>Hozzon létre, és az alkalmazás nyomkövetési naplók megtekintése
Ebben a szakaszban el kell végeznie hello a következő feladatokat:

* Adja hozzá a nyomkövetési utasítások toohello webes projekt létrehozott [Ismerkedés az Azure és az ASP.NET][GetStarted].
* Hello naplók megtekintéséhez a hello projekt helyi futtatásakor.
* Hello naplók megtekintéséhez, mert azokat az Azure-ban futó hello alkalmazás által generált.

Hogyan toocreate alkalmazás naplózza a webjobs-feladatok kapcsolatos információkért lásd: [hogyan Azure üzenetsor-kezelési tárolási használatával toowork hello WebJobs SDK - hogyan toowrite naplózza az](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs). hello naplók megtekintése, és azzal kapcsolatban, hogyan vannak tárolva az Azure-ban az alábbi utasításokat is alkalmazni webjobs-feladatok által létrehozott tooapplication naplókat.

### <a name="add-tracing-statements-toohello-application"></a>Nyomkövetési utasítások toohello alkalmazás hozzáadása
1. Nyissa meg *Controllers\HomeController.cs*, és cserélje le a hello `Index`, `About`, és `Contact` hello a következő módszereket rendelés tooadd kódot `Trace` utasítások és egy `using` utasítás a `System.Diagnostics`:

        public ActionResult Index()
        {
            Trace.WriteLine("Entering Index method");
            ViewBag.Message = "Modify this template toojump-start your ASP.NET MVC application.";
            Trace.TraceInformation("Displaying hello Index page at " + DateTime.Now.ToLongTimeString());
            Trace.WriteLine("Leaving Index method");
            return View();
        }

        public ActionResult About()
        {
            Trace.WriteLine("Entering About method");
            ViewBag.Message = "Your app description page.";
            Trace.TraceWarning("Transient error on hello About page at " + DateTime.Now.ToShortTimeString());
            Trace.WriteLine("Leaving About method");
            return View();
        }

        public ActionResult Contact()
        {
            Trace.WriteLine("Entering Contact method");
            ViewBag.Message = "Your contact page.";
            Trace.TraceError("Fatal error on hello Contact page at " + DateTime.Now.ToLongTimeString());
            Trace.WriteLine("Leaving Contact method");
            return View();
        }        
2. Adja hozzá a `using System.Diagnostics;` utasítás toohello felső hello fájl.

### <a name="view-hello-tracing-output-locally"></a>Nézet hello nyomkövetési kimeneti helyileg
1. Nyomja le az F5 toorun hello alkalmazás hibakeresési módban.

    hello alapértelmezett nyomkövetés-figyelő írja az összes nyomkövetési kimeneti toohello **kimeneti** együtt más hibakeresési kimeneti ablakban. hello alábbi ábrán látható hello hello kimenete toohello hozzáadásának nyomkövetési utasítások `Index` metódust.

    ![Nyomkövetés hibakeresési ablakban](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugtracing.png)

    hello lépések bemutatják, hogyan tooview nyomkövetési kimenetet weblapot hibakeresési módban fordítása nélkül.
2. Nyissa meg a hello alkalmazás Web.config fájljában (hello egy hello projekt mappában található), és adja hozzá a `<system.diagnostics>` elem csak hello Bezárás előtt hello fájl hello végén `</configuration>` elem:

          <system.diagnostics>
            <trace>
              <listeners>
                <add name="WebPageTraceListener"
                    type="System.Web.WebPageTraceListener,
                    System.Web,
                    Version=4.0.0.0,
                    Culture=neutral,
                    PublicKeyToken=b03f5f7f11d50a3a" />
              </listeners>
            </trace>
          </system.diagnostics>

    Hello `WebPageTraceListener` lehetővé megtekintését nyomkövetési kimeneti tallózással túl`/trace.axd`.
3. Adja hozzá a <a href="http://msdn.microsoft.com/library/vstudio/6915t83k(v=vs.100).aspx">nyomkövetési elem</a> alatt `<system.web>` hello Web.config fájlban, például a következő példa hello:

        <trace enabled="true" writeToDiagnosticsTrace="true" mostRecent="true" pageOutput="false" />
4. Nyomja le a CTRL + F5 toorun hello alkalmazás.
5. Hello hello böngészőablak címsorában, adja hozzá *trace.axd* toohello URL-címet, majd nyomja le az Enter (hello URL-címet fog hasonló toohttp://localhost:53370/trace.axd).
6. A hello **alkalmazás nyomkövetési** kattintson **részleteinek megtekintése** hello első sorban (nem hello BrowserLink sor).

    ![Trace.axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd1.png)

    Hello **kérelem részleteinek** lap jelenik meg, és a hello **nyomkövetési adatok** hello nyomkövetési utasítást, hogy hozzáadta a toohello hello kimenet jelenik meg a szakasz `Index` metódust.

    ![Trace.axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd2.png)

    Alapértelmezés szerint `trace.axd` csak helyileg nem érhető el. Ha toomake akkor érhető el a távoli webes alkalmazás, hozzáadhatja `localOnly="false"` toohello `trace` hello elemének *Web.config* fájl, ahogy az alábbi példa hello:

        <trace enabled="true" writeToDiagnosticsTrace="true" localOnly="false" mostRecent="true" pageOutput="false" />

    Azonban engedélyezése `trace.axd` egy éles környezetben a webes alkalmazás általában nem ajánlott biztonsági okokból, és a következő szakaszok hello egy egyszerűbb módon tooread nyomkövetési naplók az Azure web app alkalmazásban látható.

### <a name="view-hello-tracing-output-in-azure"></a>Az Azure-ban hello nyomkövetési kimeneti megtekintése
1. A **Megoldáskezelőben**, kattintson a jobb gombbal a hello webes projektet, és kattintson a **közzététel**.
2. A hello **webhely közzététele** párbeszédpanel, kattintson a **közzététel**.

    Után a Visual Studio közzéteszi a frissítést, a böngésző ablak tooyour kezdőlap nyílik meg (feltéve, hogy ezt nem kapcsolhatja **URL-címre** a hello **kapcsolat** lap).
3. A **Server Explorer**, kattintson a jobb gombbal a webalkalmazást, és válassza ki **folyamatos átviteli naplók megtekintése**.

    ![Folyamatos átviteli naplók megtekintése a helyi menü](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewlogsmenu.png)

    Hello **kimeneti** ablak mutatja, hogy a program csatlakoztatott toohello napló-adatfolyam-szolgáltatásra, és hozzáad egy értesítési sor minden perce, amely a napló toodisplay nélkül kerül.

    ![Folyamatos átviteli naplók megtekintése a helyi menü](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-nologsyet.png)
4. Az alkalmazás kezdőlapját mutatja hello böngészőablakban, kattintson a **forduljon**.

    Néhány másodperc hello hello hiba szintű nyomkövetési kimenetét belül toohello hozzáadott `Contact` metódus hello megjelenik **kimeneti** ablak.

    ![Hiba történt a nyomkövetési kimeneti ablakban](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-errortrace.png)

    A Visual Studio van a hiba szintű nyomkövetések csak jelennek meg, mert a szolgáltatás figyelése hello napló engedélyezésekor ez hello alapértelmezett beállítás. Amikor létrehoz egy új Azure-webalkalmazásban, az összes naplózási alapértelmezésben le van tiltva, ahogy azt korábban a hello-beállítások lap megnyitása:

    ![Kijelentkezés alkalmazás](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-apploggingoff.png)

    Azonban ha kijelölt **folyamatos átviteli naplók megtekintése**, Visual Studio automatikusan **alkalmazás Logging(File System)** túl**hiba**, ami azt jelenti, hogy hiba szintű naplók jelentett beolvasása. A rendezés toosee a nyomkövetési naplók, módosíthatja ezt a beállítást túl**részletes**. Amikor kiválaszt egy súlyossági szintje alacsonyabb, mint a hiba, magasabb súlyossági szintek összes naplójának is jelenti. Ezért részletes kiválasztásakor is látni információ, figyelmeztetés és hibanaplókat.  

1. A **Server Explorer**, kattintson a jobb gombbal a hello webalkalmazást, és kattintson a **nézetbeállítások** úgy, ahogy korábban.
2. Változás **Alkalmazásnaplózást (fájlrendszer)** túl**részletes**, és kattintson a **mentése**.

    ![A beállítás a nyomkövetési szint tooVerbose](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-applogverbose.png)
3. Most megjelenítő hello böngészőablakban a **forduljon** lapján kattintson **Home**, majd kattintson a **kapcsolatos**, és kattintson a **forduljon**.

    Néhány másodpercen belül hello **kimeneti** ablakban láthatók a nyomkövetési kimenetet.

    ![Részletes nyomkövetés](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-verbosetraces.png)

    Ebben a szakaszban engedélyezve van, és le van tiltva a naplózás Azure webalkalmazás-beállítások használatával. Is engedélyezése és letiltása nyomkövetésfigyelők hello Web.config fájl módosításával. Azonban hello Web.config fájl módosítása hatására hello app tartomány toorecycle, miközben keresztül hello web app konfigurációs naplózás engedélyezése nem végez, amely. Ha hello probléma egy hosszú ideig tooreproduce, illetve nem időszakos, hello alkalmazástartományban újrahasznosítási előfordulhat, hogy "javítás" és kényszerítése toowait csak akkor történik, újra. Az Azure diagnostics engedélyezése nem ehhez hibainformációk azonnal rögzítés megkezdéséhez.

### <a name="output-window-features"></a>A kimeneti ablakban szolgáltatások
Hello **Azure naplók** hello lapján **kimeneti** ablak több gombok és szövegmező rendelkezik:

![Naplófájlok lap gombok](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-icons.png)

Ezek hajtsa végre a következő funkciók hello:

* Törölje a jelet hello **kimeneti** ablak.
* Engedélyezi vagy letiltja a sortörés.
* Indítsa el, vagy állítsa le a naplók figyelése.
* Adja meg, hogy toomonitor naplózza.
* Naplók letöltéséhez.
* A keresési karakterláncot vagy reguláris kifejezést alapján szűrni.
* Bezárás hello **kimeneti** ablak.

Ha megad egy keresési karakterláncot vagy reguláris kifejezést, a Visual Studio szűrők hello ügyfél naplózási információkat. Ez azt jelenti, hogy hello feltételeket adhatja meg, miután hello jelenik meg hello **kimeneti** ablakban és szűrési feltételeket módosíthatja anélkül, hogy tooregenerate hello naplókat.

## <a name="webserverlogs"></a>Web server naplók megtekintése
Webkiszolgáló naplóinak jegyezze fel az összes HTTP-tevékenység hello webalkalmazás. A sorrend toosee őket a hello **kimeneti** tooenable azokat hello webalkalmazás, és közölje a Visual Studio, amelyet az toomonitor rendelkezik ablak őket.

1. A hello **Azure Web App konfigurációs** lapon, a megnyitott **Server Explorer**, Web Server-naplózás túl módosítása**a**, és kattintson a **mentése**.

    ![Web server naplózásának engedélyezése](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-webserverloggingon.png)
2. A hello **kimeneti** ablakban kattintson hello **adja meg, mely Azure naplók toomonitor** gombra.

    ![Adja meg, mely Azure naplók toomonitor](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-specifylogs.png)
3. A hello **Azure naplózási beállítások** párbeszédpanelen jelölje ki **webalkalmazás-naplói**, és kattintson a **OK**.

    ![Webkiszolgáló naplóinak figyelése](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorwslogson.png)
4. Hello webalkalmazás mutatja hello böngészőablakban, kattintson a **Home**, majd kattintson a **kapcsolatos**, és kattintson a **forduljon**.

    hello alkalmazásnaplók általában az első, hello webkiszolgáló naplóinak követ. Előfordulhat, hogy toowait igénybe az hello tooappear naplózza.

    ![Webkiszolgáló naplózza a kimeneti ablakban](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-wslogs.png)

Alapértelmezés szerint a webkiszolgáló naplóinak engedélyezése a Visual Studio használatával Azure ír hello naplók toohello fájlrendszer. Alternatív megoldásként használhatja hello Azure portál, amely a webalkalmazás-naplói toospecify kell megírni tooa blob tároló a storage-fiók.

Ha hello portál tooenable webkiszolgáló naplózás tooan Azure storage-fiók, majd tiltsa le a naplózást a Visual Studio alkalmazásban, ha újra engedélyezi a Visual Studio, a tárolási fiók beállításainak visszaállítása-naplózás.

## <a name="detailederrorlogs"></a>Részletes hibanaplókat üzenet megtekintése
Részletes hibanaplókat néhány további információval szolgálnak a válasz hibakódok (400 vagy újabb) eredményező HTTP-kérelmekre. A sorrend toosee őket a hello **kimeneti** ablakban rendelkezik tooenable azokat hello webalkalmazás, és közölje a Visual Studio, amelyet az toomonitor őket.

1. A hello **Azure Web App konfigurációs** lapon, a megnyitott **Server Explorer**, módosítsa **részletes hibaüzenetek** túl**a**, és Kattintson a **mentése**.

    ![A részletes hibaüzenetek engedélyezése](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailedlogson.png)
2. A hello **kimeneti** ablakban kattintson hello **adja meg, mely Azure naplók toomonitor** gombra.
3. A hello **Azure naplózási beállítások** párbeszédpanel, kattintson a **összes napló**, és kattintson a **OK**.

    ![Összes napló figyelése](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorall.png)
4. Hello hello böngészőablak címsorában, adja hozzá egy extra karakter toohello URL-cím toocause 404 hibaüzenetet (például `http://localhost:53370/Home/Contactx`), és nyomja le az ENTER billentyűt.

    Néhány másodpercen belül megjelenik hello részletes hibanapló hello Visual Studio **kimeneti** ablak.

    ![A kimeneti ablakban részletes hibanapló](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorlog.png)

    CTRL + hello hivatkozás toosee hello napló kimeneti formázott böngészőben kattintson:

    ![Részletes hibanapló böngészőablakban](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorloginbrowser.png)

## <a name="downloadlogs"></a>Fájl rendszer naplók letöltése
A naplók a hello megfigyeléséhez **kimeneti** ablak is letölthető a *.zip* fájlt.

1. A hello **kimeneti** ablak, kattintson a **folyamatos átviteli naplók letöltése**.

    ![Naplófájlok lap gombok](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-downloadicon.png)

    Fájlkezelő megnyitása tooyour *letölti* hello mappa a kijelölt fájl letöltése.

    ![A letöltött fájl](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-downloadedfile.png)
2. Bontsa ki a hello *.zip* fájlt, és tekintse meg a következő mappaszerkezet hello:

    ![A letöltött fájl](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-logfilefolders.png)

   * Alkalmazás nyomkövetési naplók vannak *.txt* hello fájlok *LogFiles\Application* mappa.
   * Nincsenek a webkiszolgáló naplói a *.log* hello fájlok *LogFiles\http\RawLogs* mappa. Használhatja például egy eszközt [napló elemző](http://www.microsoft.com/download/details.aspx?displaylang=en&id=24659) tooview és kezelheti ezeket a fájlokat.
   * Üzenet részletes hibanaplókat a rendszer *.html* hello fájlok *LogFiles\DetailedErrors* mappa.

     (hello *központi telepítések* mappa közzététele; verziókezelő által létrehozott fájlok van nincs semmi kapcsolódó tooVisual Studio közzététel. Hello *Git* mappa van nyomkövetések kapcsolódó toosource vezérlő közzétételi és hello naplófájl adatfolyam-szolgáltatás.)  

## <a name="storagelogs"></a>Tárolási naplók megtekintése
Alkalmazás nyomkövetési naplóit is elküldheti tooan Azure-tárfiókot, és megtekintheti azokat a Visual Studio. toodo lesz hozzon létre egy tárfiókot, engedélyezése a tárolási naplófájljai hello klasszikus portál, és megtekintheti hello **naplók** hello lapján **Azure Web Apps** ablak.

Naplók tooany vagy az összes három hely küldheti el:

* hello fájlrendszer.
* Storage-fiók táblákat.
* Tárolási fiók blobokat.

Megadhat egy másik súlyossági szint minden célhelyéhez.

Táblázatokkal naplók online könnyen tooview részleteit, és támogatják a streaming; naplók a következő táblákban lekérdezni, és tekintse meg az új naplókat, azok alatt létrehozásakor. Blobok révén könnyen toodownload naplók a fájlok és tooanalyze őket HDInsight használatával, mert a HDInsight tudja, hogyan toowork az blob storage szolgáltatással. További információkért lásd: **Hadoop és a MapReduce** a [adatok tárolási beállításai (épület valós felhőalapú alkalmazásokat az Azure-ral)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options).

Már van naplók set tooverbose fájlrendszerszintű; hello következő lépések végigvezetik információk szintű naplók toogo toostorage fiók táblák beállítását. Információs szint azt jelenti, hogy a hívó által létrehozott összes napló `Trace.TraceInformation`, `Trace.TraceWarning`, és `Trace.TraceError` jelenik meg, de nem a hívó által létrehozott naplók `Trace.WriteLine`.

Storage-fiókok kínálnak, több helyre és a naplók hosszabb ideig tart megőrzési toohello fájlrendszer képest. Nyomkövetési naplók toostorage küldő alkalmazás egy másik előnye, hogy néhány további információt az egyes naplókon, hogy fájl rendszernaplókat nem kap.

1. Kattintson a jobb gombbal **tárolási** hello Azure csomópont, és kattintson a **Storage-fiók létrehozása**.

![Storage-fiók létrehozása](./media/web-sites-dotnet-troubleshoot-visual-studio/createstor.png)

1. A hello **Storage-fiók létrehozása** párbeszédpanelen adja meg hello storage-fiók nevét.

    hello nevének meg kell egyedinek kell lennie (egyéb Azure storage-fiók hello rendelkezhet ugyanazzal a névvel). Ha hello név már használatban van egy alkalommal toochange kaphat azt.

    a tárfiók lesz URL-cím tooaccess hello *{name}*. core.windows.net.
2. Set hello **régiót vagy Affinitáscsoportot** legördülő lista toohello régió legközelebbi tooyou.

    A beállítással megadható, hogy melyik Azure-adatközpontban fogja futtatni a tárfiók. Ebben az oktatóanyagban a választott nem teszi a érzékelhető különbség, de a termelési webalkalmazás keresi a webkiszolgáló és a tárolási fiók toobe a hello ugyanabban a régióban toominimize késést és az adatok kilépési díjakat. a régió eléréséhez a webalkalmazás a rendelés toominimize késés lehetséges toohello böngészők legközelebb hello web app (amely hoz létre, később) kell futtatni.
3. Set hello **replikációs** legördülő lista túl**helyileg redundáns**.
   
    A georeplikáció engedélyezve van a tárfiók, hello tárolt tartalom esetén meg kell replikált tooa másodlagos adatközpontba tooenable feladatátvételi toothat helyévé hello elsődleges helyen jelentős katasztrófa következik. A georeplikáció további költségeket vonhat maga után. A vizsgálati és fejlesztői fiókok esetében általában nem érdemes toopay a georeplikációért. További információ: [Tárfiók létrehozása, kezelése vagy törlése](../storage/common/storage-create-storage-account.md).
4. Kattintson a **Create** (Létrehozás) gombra.

    ![Új tárfiók](./media/web-sites-dotnet-troubleshoot-visual-studio/newstorage.png)    
5. A Visual Studio hello **Azure Web Apps** ablakban hello kattintson **naplók** fülre, majd **naplózás konfigurálása a kezelési portál**.

    <!-- todo:screenshot of new portal if hello VS page link goes toonew portal -->
    ![Naplózás konfigurálása](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-configlogging.png)

    Ekkor megnyílik a hello **konfigurálása** hello klasszikus portál webalkalmazása fülre.
6. Hello klasszikus portál **konfigurálása** lapon görgessen lefelé toohello alkalmazás diagnosztika szakaszban, és módosítsa **Alkalmazásnaplózást (Table Storage)** túl**a**.
7. Változás **naplózási szint** túl**információk**.
8. Kattintson a **kezelése a Table Storage**.

    ![Kattintson a kezelés TableStorage](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-stgsettingsmgmtportal.png)

    A hello **kezelése a table storage az application diagnostics** mezőben is választhat, a tárfiók Ha több van. Hozzon létre egy új táblát, vagy használjon egy meglévőt.

    ![A table storage kezelése](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-choosestorageacct.png)
9. A hello **kezelése a table storage az application diagnostics** mezőben kattintson hello pipa tooclose hello mezőre.
10. Hello klasszikus portál **konfigurálása** lapra, majd **mentése**.
11. Hello alkalmazás webalkalmazás megjelenítő hello böngészőablakban, kattintson **Home**, majd kattintson a **kapcsolatos**, és kattintson a **forduljon**.

     Ezek a weblapok böngészés által előállított hello naplóinformációk toohello tárfiók lesz írva.
12. A hello **naplók** hello lapján **Azure Web Apps** ablak a Visual Studióban kattintson **frissítése** alatt **diagnosztikai összegzés**.

     ![Kattintson a frissítés](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-refreshstorage.png)

     Hello **diagnosztikai összegzés** szakasz bemutatja naplók hello az elmúlt 15 perc alapértelmezés szerint. Hello időszak toosee további naplók módosíthatja.

     (Ha a "tábla nem található" hibaüzenet jelenik meg, győződjön meg arról, hogy a toohello által felügyelt oldalak felhívásainak hello, miután engedélyezte a nyomkövetés tallózott **Alkalmazásnaplózást (tárolás)** , miután rákattintott **mentése**.)

     ![A tárolási naplófájljai](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-storagelogs.png)

     Figyelje meg, hogy ez a megtekintése, lásd: **Folyamatazonosító** és **Szálazonosító** minden napló, amely hello fájl rendszernaplókban nem kap. További mezők hello az Azure storage tábla közvetlenül megtekintésével tekintheti meg.
13. Kattintson a **megtekintése összes alkalmazásnaplók**.

     hello nyomkövetési napló táblázatának hello az Azure storage tábla viewer jelenik meg.

     (Ha a "sorozat nem tartalmaz elemeket" hibaüzenet jelenik meg, nyissa meg a **Server Explorer**, bontsa ki a tárfiók alapján hello hello csomópontot **Azure** csomópontját, és kattintson rá jobb gombbal **táblák**kattintson **frissítése**.)

     ![Tárolási bejelentkezik tábla megtekintése](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-tracelogtableview.png)

     Ebben a nézetben látható a kiegészítő mezők nem jelenik meg semmilyen más nézetekhez. Ez a nézet is lehetővé teszi toofilter naplói különleges lekérdezés-szerkesztő felhasználói felület használatával hozhat létre lekérdezést. További információkért tekintse meg a tábla erőforrások használata - szűrés az entitások [tárolási erőforrások keresse meg a Server Explorer](http://msdn.microsoft.com/library/ff683677.aspx).
14. toolook egy egysoros, hello részleteit, kattintson duplán a hello sorokat.

     ![A Server Explorer nyomkövetési tábla](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-tracetablerow.png)

## <a name="failedrequestlogs"></a>Sikertelen kérelmek nyomkövetési naplók megtekintése
Sikertelen kérelmek nyomkövetési naplóit hasznosak, ha toounderstand hello részleteit hogyan IIS kezeli a HTTP-kérelem, az URL-címet írja át vagy hitelesítési problémák hasonló forgatókönyvek esetén van szükség.

Azure web apps használata hello ugyanaz a sikertelen kérelmek nyomkövetési funkció, melyet az IIS 7.0 és újabb verziói. Hozzáférés toohello IIS konfiguráló beállítások terjesztésére mely hibák naplózásra, azonban nincs. Ha engedélyezi a sikertelen kérelmek nyomon követése, a rendszer rögzíti az összes hiba.

Visual Studio használatával engedélyezheti a sikertelen kérelmek nyomon követése, de nem lehet megtekinteni azokat a Visual Studióban. Ezek a naplók olyan XML-fájlok. csak a naplózási szolgáltatás streaming hello figyeli fontosságúnak ítélt olvasható egyszerű szöveges módban fájlok: *.txt*, *.html*, és *.log* fájlokat.

Sikertelen kérelmek nyomkövetési naplóit közvetlenül az FTP-n keresztül, vagy helyileg az FTP-eszköz toodownload használata után webböngészőben megtekintheti őket tooyour helyi számítógép. Ebben a szakaszban lesz meg egy böngészőt közvetlenül.

1. A hello **konfigurációs** hello lapján **Azure Web Apps** ablak, amely a megnyitott **Server Explorer**, módosítsa **sikertelen kérelmek nyomkövetése**túl**a**, és kattintson a **mentése**.

    ![A sikertelen kérelmek nyomkövetésének engedélyezése](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-failedrequeston.png)
2. Hello címsorába hello webalkalmazás megjelenítő böngészőablak hello extra karakter toohello URL-címének hozzáadása, és kattintson a Enter toocause 404 hibaüzenetet.

    Ennek hatására a sikertelen kérelmek nyomkövetési napló toobe létrehozott, és hello következő lépések bemutatják, hogyan tooview vagy letöltési hello napló.
3. A Visual Studio, a hello **konfigurációs** hello lapján **Azure Web Apps** ablak, kattintson a **nyissa meg a felügyeleti portál**.
4. A hello [Azure Portal](https://portal.azure.com) **beállítások** a webalkalmazás panelen kattintson a **üzembe helyezési hitelesítő adatok**, majd adja meg egy új felhasználónevet és jelszót.

    ![Új FTP-felhasználónév és jelszó](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-enterftpcredentials.png)

    ** Bejelentkezéskor, hogy toouse hello teljes felhasználónevét hello web app nevét tartalmazó előtaggal tooit. Például ha hello hely be egy felhasználónevet "myid" és "myexample", hogy jelentkezzen be "myexample\myid".
5. Egy új böngészőablakban nyissa meg toohello mezőben látható URL **FTP-állomásnév** vagy **FTPS állomásnév** a hello **webalkalmazás** a webalkalmazás panelen.
6. Jelentkezzen be, amelyet korábban (beleértve a hello web app name előtag hello felhasználónévvel) hozott létre hello FTP hitelesítő adatok használatával.

    hello böngésző hello webalkalmazás gyökérmappájában hello jeleníti meg.
7. Nyissa meg hello *naplófájlok* mappát.

    ![Nyissa meg a naplófájlok mappa](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-logfilesfolder.png)
8. Nyissa meg a hello W3SVC plusz egy numerikus érték nevű mappát.

    ![Nyissa meg a W3SVC mappa](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-w3svcfolder.png)

    hello mappa XML-fájlokat, miután engedélyezte a sikertelen kérelmek nyomkövetése naplózott hibákat, és az, hogy a böngésző használható-e tooformat hello XML XSL-fájl tartalmazza.

    ![W3SVC mappa](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-w3svcfoldercontents.png)
9. Kattintson a sikertelen kérelmek hello toosee nyomkövetési adatok a kívánt hello XML-fájl.

    hello alábbi ábrán egy minta hiba hello nyomkövetési adatait részét.

    ![Sikertelen kérelmek követésének böngészőben](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-failedrequestinbrowser.png)

## <a name="nextsteps"></a>Következő lépések
Megtudhatta, hogyan Visual Studio segítségével könnyen tooview Azure-webalkalmazás által létrehozott naplók. hello következő szakaszokban hivatkozások toomore erőforrások a kapcsolódó témaköröket:

* Az Azure web app hibaelhárítása
* A Visual Studio hibakereső
* Távoli hibakeresés az Azure-ban
* Nyomkövetés az ASP.NET alkalmazásokban.
* Webkiszolgáló naplóinak elemzésekor
* Elemzése sikertelen kérelmek nyomkövetési naplóit.
* Hibakeresési Cloud Services csomag

### <a name="azure-web-app-troubleshooting"></a>Az Azure web app hibaelhárítása
Az Azure App Service web apps hibaelhárítással kapcsolatos további információkért tekintse meg a következő erőforrások hello:

* [Hogyan toomonitor webalkalmazások](/manage/services/web-sites/how-to-monitor-websites/)
* [Vizsgálja meg az Azure Web Apps a Visual Studio 2013 memóriavesztés](http://blogs.msdn.com/b/visualstudioalm/archive/2013/12/20/investigating-memory-leaks-in-azure-web-sites-with-visual-studio-2013.aspx). Visual Studio szolgáltatások elemzése a Microsoft ALM blogbejegyzésben felügyelt memóriaproblémák léptek fel.
* [Az Azure web apps online eszközök kell tudni](https://azure.microsoft.com/blog/2014/03/28/windows-azure-websites-online-tools-you-should-know-about-2/). Amit Apple blogbejegyzést.

Egy adott hibaelhárítási kérdés kapcsolatban elindítani egy szálat a következő fórumok hello egyikében:

* [ASP.NET-webhely hello Azure fórummal hello](http://forums.asp.net/1247.aspx/1?Azure+and+ASP+NET).
* [az MSDN Webhelyén Azure fórumra hello](http://social.msdn.microsoft.com/Forums/windowsazure/).
* [StackOverflow.com](http://www.stackoverflow.com).

### <a name="debugging-in-visual-studio"></a>A Visual Studio hibakereső
Hogyan toouse hibakeresési mód a Visual Studio kapcsolatos további információkért lásd: hello [a Visual Studio hibakeresési](http://msdn.microsoft.com/library/vstudio/sc65sadd.aspx) MSDN-témakörben és [hibakeresési tippeket a Visual Studio 2010](http://weblogs.asp.net/scottgu/archive/2010/08/18/debugging-tips-with-visual-studio-2010.aspx).

### <a name="remote-debugging-in-azure"></a>Távoli hibakeresés az Azure-ban
Az Azure-webalkalmazásokban és webjobs-feladatok távoli hibakereséssel kapcsolatos további információkért tekintse meg a következő erőforrások hello:

* [Bevezetés tooRemote hibakeresés Azure App Service Web Apps](https://azure.microsoft.com/blog/2014/05/06/introduction-to-remote-debugging-on-azure-web-sites/).
* [Bevezetés tooRemote belül távoli hibakeresés 2 - hibakeresés Azure App Service Web Apps. rész](https://azure.microsoft.com/blog/2014/05/07/introduction-to-remote-debugging-azure-web-sites-part-2-inside-remote-debugging/)
* [Bevezetés tooRemote Azure App Service Web Apps részre 3 - többpéldányos környezet és a GIT-hibakeresés](https://azure.microsoft.com/blog/2014/05/08/introduction-to-remote-debugging-on-azure-web-sites-part-3-multi-instance-environment-and-git/)
* [WebJobs-hibakeresés (videó)](https://www.youtube.com/watch?v=ncQm9q5ZFZs&list=UU_SjTh-ZltPmTYzAybypB-g&index=1)

Ha a webes alkalmazás használ egy Azure webes API-t vagy a Mobile Services háttér- és által látható toodebug kell [.NET-háttérrendszer hibakeresés a Visual Studio](http://blogs.msdn.com/b/azuremobile/archive/2014/03/14/debugging-net-backend-in-visual-studio.aspx).

### <a name="tracing-in-aspnet-applications"></a>Nyomkövetés az ASP.NET alkalmazásokban.
Nem érhető el a hello Internet nyomkövetés alapos, naprakész állapotban bevezetés tooASP.NET van. a Web Forms mert MVC még létezik, és nem kiegészítése, amely az újabb blog visszaküldés, amelyek arra utalnak a konkrét problémák írt régi bevezető anyagok hello legjobb elvégezhető van megismerésében. Néhány legalkalmasabb helyek toostart a következő erőforrások hello:

* [Figyelés és Telemetriai (valós felhőalapú alkalmazások összeállítása az Azure-ral)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).<br>
  E-könyv fejezet az ajánlások a nyomkövetés az Azure felhőalapú alkalmazásokhoz.
* [Az ASP.NET nyomkövetési](http://msdn.microsoft.com/library/ms972204.aspx)<br/>
  Régi, de továbbra is egy alapszintű bevezetést toohello tulajdonos helyes erőforrás.
* [Nyomkövetésfigyelők](http://msdn.microsoft.com/library/4y5y10s7.aspx)<br/>
  Információ a nyomkövetésfigyelők, de nem említse meg a hello [WebPageTraceListener](http://msdn.microsoft.com/library/system.web.webpagetracelistener.aspx).
* [Forgatókönyv: Az ASP.NET nyomkövetési integrálása System.Diagnostics nyomkövetése](http://msdn.microsoft.com/library/b0ectfxd.aspx)<br/>
  Ez túl régi, de további információkat tartalmaz hello bevezető cikkben nem foglalkozunk.
* [Az ASP.NET MVC Razor nézetekben nyomkövetése](http://blogs.msdn.com/b/webdev/archive/2013/07/16/tracing-in-asp-net-mvc-razor-views.aspx)<br/>
  Nyomkövetés Razor nézetekben, mellett hello post azt is ismerteti, hogyan toocreate hiba szűrés rendelés toolog MVC alkalmazás összes kezeletlen kivételek. Hogyan nem kezelt összes toolog információt webes űrlapalkalmazás, kivételek hello Global.asax példáját lásd: [hibakezelők teljes példa](http://msdn.microsoft.com/library/bb397417.aspx) az MSDN Webhelyén. MVC, vagy a Web Forms keretrendszerre Ha szeretné, hogy bizonyos kivételek toolog, de hello alapértelmezett keretrendszer kezelése hatását, hogy meg catch és ismételt kiváltási, mint például a következő hello:

        try
        {
           // Your code that might cause an exception toobe thrown.
        }
        catch (Exception ex)
        {
            Trace.TraceError("Exception: " + ex.ToString());
            throw;
        }
* [Adatfolyam-diagnosztikai nyomkövetési naplózás a hello Azure parancssori felülettel (plusz futó!)](http://www.hanselman.com/blog/StreamingDiagnosticsTraceLoggingFromTheAzureCommandLinePlusGlimpse.aspx)<br/>
  Hogyan toouse hello parancssori toodo milyen az oktatóanyag azt mutatja be hogyan toodo a Visual Studióban. [Futó](http://www.hanselman.com/blog/IfYoureNotUsingGlimpseWithASPNETForDebuggingAndProfilingYoureMissingOut.aspx) ASP.NET-alkalmazások hibakereséshez eszköz.
* [Webalkalmazások naplózás és diagnosztika - használata a David Ebbo](/documentation/videos/azure-web-site-logging-and-diagnostics/) és [a folyamatos átviteli naplók, a webalkalmazások - David Ebbo](/documentation/videos/log-streaming-with-azure-web-sites/)<br>
  Videók Scott Hanselman és David Ebbo.

Hiba a naplózás, egy alternatív toowriting saját nyomkövetés kódja: egy nyílt forráskódú toouse naplózási keretrendszert, mint [ELMAH](http://nuget.org/packages/elmah/). További információkért lásd: [Scott Hanselman kapcsolatos blogbejegyzéseket ELMAH](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx).

Emellett vegye figyelembe, hogy toouse ASP.NET nincs, vagy ha azt szeretné, hogy a tooget streaming System.Diagnostics nyomkövetés naplózza az Azure-ból. hello adatfolyam-naplózási szolgáltatás Azure-webalkalmazásban továbbítandó bármely *.txt*, *.html*, vagy *.log* megkeresi az hello fájl *naplófájlok* mappát. Ezért létrehozhatja a saját naplózási rendszer toohello fájlrendszer hello webalkalmazás írja, és a fájl a rendszer automatikusan továbbítja adatfolyamként, majd le. Mindössze meg kell toodo fájlokat hozza létre a hello írási alkalmazáskód *d:\home\logfiles* mappa.

### <a name="analyzing-web-server-logs"></a>Webkiszolgáló naplóinak elemzésekor
Webkiszolgáló naplóinak elemzésével kapcsolatos további információkért tekintse meg a következő erőforrások hello:

* [LogParser](http://www.microsoft.com/download/details.aspx?id=24659)<br/>
  Egy eszköz a adatok megtekintése a webkiszolgáló naplóinak (*.log* fájlok).
* [IIS teljesítményproblémák vagy LogParser használó alkalmazás-hibák elhárítása](http://www.iis.net/learn/troubleshoot/performance-issues/troubleshooting-iis-performance-issues-or-application-errors-using-logparser)<br/>
  Egy bevezető toohello napló elemző eszköz használható tooanalyze webkiszolgáló naplózza.
* [Blogbejegyzéseket által Robert McMurray LogParser használatával](http://blogs.msdn.com/b/robert_mcmurray/archive/tags/logparser/)<br/>
* [hello az IIS 7.0, IIS 7.5 és IIS 8.0-s HTTP-állapotkód:](http://support.microsoft.com/kb/943891)

### <a name="analyzing-failed-request-tracing-logs"></a>Elemzése sikertelen kérelmek nyomkövetési naplóit.
hello Microsoft TechNet webhely tartalmaz egy [használatával sikertelen kérelmek nyomkövetése](http://www.iis.net/learn/troubleshoot/using-failed-request-tracing) szakaszban, amelyek segíthetnek a ismertetése hogyan toouse ezek a naplók. Azonban ez a dokumentáció összpontosít főként sikertelen kérelmek nyomkövetése konfigurálása az IIS-ben, amely az Azure Web Apps nem hajtható végre.

[GetStarted]: app-service-web-get-started-dotnet.md
[GetStartedWJ]: websites-dotnet-webjobs-sdk.md
