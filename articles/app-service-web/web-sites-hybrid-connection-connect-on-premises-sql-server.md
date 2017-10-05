---
title: "Csatlakoztassa a helyszíni SQL Server egy webalkalmazást az Azure App Service hibrid kapcsolatok használata"
description: "A Microsoft Azure-webalkalmazás létrehozása, és csatlakoztassa a helyszíni SQL Server-adatbázis"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: 2b4e0539-1a0b-4aa1-8a69-b4b053c3b2e5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2016
ms.author: cephalin
ms.openlocfilehash: 12456ef3e2aecfa7a03cca97de2ff6ffd9602357
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-on-premises-sql-server-from-a-web-app-in-azure-app-service-using-hybrid-connections"></a>Csatlakoztassa a helyszíni SQL Server egy webalkalmazást az Azure App Service hibrid kapcsolatok használata
Hibrid kapcsolatok kapcsolódhatnak [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps statikus TCP-port használatára a helyszíni erőforrásokhoz. Támogatott erőforrások közé tartoznak a Microsoft SQL Server, a MySQL, a HTTP webes API-k, a App Service és a legtöbb egyéni webszolgáltatások.

Ebből az oktatóanyagból megtudhatja hogyan egy App Service webalkalmazás létrehozása a [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715), a webes alkalmazás csatlakoztatása az új hibrid kapcsolat funkciójával helyi helyszíni SQL Server-adatbázis, hozzon létre egy egyszerű ASP.NET-alkalmazást, amely a hibrid kapcsolat használatát, és telepítse központilag az alkalmazást az App Service webalkalmazásba. Az elkészült webalkalmazás az Azure felhasználói hitelesítő adatokat, amely a helyszíni tagsági adatbázisban tárolja. Az oktatóanyag azt feltételezi, hogy nincs korábbi tapasztalata az Azure vagy az ASP.NET használatával kapcsolatban.

> [!NOTE]
> Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> A Web Apps része a hibrid kapcsolatok szolgáltatás csak a érhető el a [Azure Portal](https://portal.azure.com). BizTalk szolgáltatások VPN-kapcsolat létrehozásához lásd: [hibrid kapcsolatok](http://go.microsoft.com/fwlink/p/?LinkID=397274).  
> 
> 

## <a name="prerequisites"></a>Előfeltételek
Az oktatóanyag teljesítéséhez szüksége lesz a következő termékek. Az összes szabad verziók, elérhetők nekiláthat teljes mértékben az Azure ingyenes.

* **Azure-előfizetés** - ingyenes előfizetésre, lásd: [Azure ingyenes próbaverzió](/pricing/free-trial/).
* **A Visual Studio 2013** – a Visual Studio 2013 ingyenes próbaverziójának letöltéséhez lásd: [Visual Studio letöltések](http://www.visualstudio.com/downloads/download-visual-studio-vs). Telepítse a folytatás előtt.
* **A Microsoft .NET Framework 3.5 Service Pack 1** – Ha az operációs rendszer Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7, vagy Windows Server 2008 R2, engedélyezheti a Vezérlőpult > Programok és szolgáltatások > Windows-szolgáltatások be- és kikapcsolása. Ellenkező esetben töltheti le a [Microsoft Download Center](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22).
* **SQL Server 2014 Express eszközökkel** -Microsoft SQL Server Express ingyenesen letölthető a [Microsoft Web Platform adatbázissal kapcsolatos beállításainak lapja](http://www.microsoft.com/web/platform/database.aspx). Válassza ki a **Express** (nem LocalDB) verziója. A **eszközökkel Express** tartalmazza az ebben az oktatóanyagban használandó SQL Server Management Studio.
* **SQL Server Management Studio Express** – Ez tartalmazza az SQL Server 2014 Express a fent említett eszközök letöltése, de ha külön telepíteni kell, töltse le és telepítse innen a [SQL Server Express letöltési oldala](http://www.microsoft.com/web/platform/database.aspx).

Az oktatóanyag feltételezi, hogy rendelkezik-e az Azure-előfizetésre, hogy a Visual Studio 2013 telepítve van, és, hogy telepítve vagy engedélyezve van a .NET-keretrendszer 3.5. Az oktatóanyag bemutatja, hogyan telepítse az SQL Server 2014 Express konfiguráció esetén, amely az Azure hibrid kapcsolatok funkció (egy alapértelmezett példány statikus TCP-port) megfelelően működik. Az oktatóanyag megkezdése előtt letöltése SQL Server 2014 Express eszközökkel a fent említett, ha nincs telepítve az SQL Server.

### <a name="notes"></a>Megjegyzések
A helyszíni SQL Server vagy SQL Server Express adatbázis használata hibrid kapcsolaton keresztül, TCP/IP kell engedélyezni a statikus port. SQL Server alapértelmezett példány statikus 1433-as port használja, mivel elnevezett példányok azonban nem.

A számítógép, amelyen a helyszíni Hybrid Connection Manager ügynököt telepíti:

* Rendelkeznie kell Azure kimenő kapcsolódás keresztül:

| Port | miért |
| --- | --- |
| 80 |**Szükséges** leellenőrizni a tanúsítvány és választhatóan adatkapcsolattal HTTP-porthoz. |
| 443 |**Nem kötelező** az adatkapcsolat. Ha a 443-as kimenő kapcsolat nem érhető el, 80-as TCP-portot használja. |
| 5671 és 9352 |**Ajánlott** , de az adatkapcsolat nem kötelező. Megjegyzés: Ebben a módban általában nagyobb átviteli teljesítményt eredményez. Ha nem érhető el kimenő kapcsolódás ezeket a portokat, 443-as TCP-portot használja. |

* Kell tudni érnie a *állomásnév*:*portszám* a helyi erőforrás.

Ebben a cikkben szereplő lépések azt feltételezik, hogy a számítógépről, amely üzemelteti a helyszíni hibrid kapcsolat ügynök böngészőt használ.

Ha már van telepítve a konfigurációban, és egy környezetben, amely megfelel a fenti körülmények között SQL Server, kihagyhatja azokat, amelyek, és kezdje [egy SQL Server-adatbázis létrehozása a helyszínen](#CreateSQLDB).

<a name="InstallSQL"></a>

## <a name="a-install-sql-server-express-enable-tcpip-and-create-a-sql-server-database-on-premises"></a>A. Telepítse az SQL Server Expresst, engedélyezze a TCP/IP protokollt, és a helyszíni SQL Server-adatbázis létrehozása
Ez a szakasz bemutatja, hogyan telepíti az SQL Server Expresst, engedélyezze a TCP/IP protokollt, és hozzon létre egy adatbázist, így a webes alkalmazás működni fog-e az Azure portál.

### <a name="install-sql-server-express"></a>SQL Server Express telepítése
1. SQL Server Express telepítéséhez futtassa a **SQLEXPRWT_x64_ENU.exe** vagy **SQLEXPR_x86_ENU.exe** letöltött fájlban. Az SQL Server telepítési központjának varázsló jelenik meg.
   
    ![SQL-kiszolgáló telepítése][SQLServerInstall]
2. Válasszon **új SQL Server önálló telepítése vagy szolgáltatások hozzáadása egy meglévő telepítéshez**. Kövesse az utasításokat, elfogadja az alapértelmezett választási lehetőségek és beállítások, amíg elér a **példány konfigurációja** lap.
3. Az a **példány konfigurációja** lapon, válassza ki **alapértelmezett példány**.
   
    ![Válassza ki az alapértelmezett példány][ChooseDefaultInstance]
   
    A statikus 1433-as port az SQL Server-ügyfelektől érkező kéréseket figyeli az SQL Server alapértelmezett példányát alapértelmezés szerint ez a hibrid kapcsolatok szolgáltatás szükséges. Elnevezett példányok használata a dinamikus portok és UDP, amely nem támogatja a hibrid kapcsolatok.
4. Fogadja el az alapértelmezett a a **kiszolgálókonfiguráció** lap.
5. Az a **adatbázismotor beállítása** lap **hitelesítési mód**, válassza a **vegyes üzemmódú (SQL Server-hitelesítést és a Windows-hitelesítés)**, és adjon meg egy jelszót.
   
    ![Válassza ki a kevert üzemmód][ChooseMixedMode]
   
    Ebben az oktatóanyagban fog használni az SQL Server-hitelesítést. Ne felejtse el a jelszót ad meg, mert később szüksége lesz az.
6. A továbbiakban a telepítés befejezéséhez a varázsló lépéseit.

### <a name="enable-tcpip"></a>Engedélyezze a TCP/IP protokollt
Engedélyezze a TCP/IP protokollt, szüksége lesz lett telepítve az SQL Server Express telepítése során az SQL Server Configuration Manager. Kövesse a [engedélyezése TCP/IP hálózati protokollt az SQL Server](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) folytatása előtt.

<a name="CreateSQLDB"></a>

### <a name="create-a-sql-server-database-on-premises"></a>A helyszíni SQL Server-adatbázis létrehozása
A Visual Studio webalkalmazás az Azure-ban elérhető tagsági adatbázis szükséges. Ehhez szükséges, hogy egy SQL Server vagy SQL Server Express adatbázist (nem a LocalDB adatbázisban, amely alapértelmezés szerint az MVC-sablont használ), így hoz létre, a tagsági adatbázisokból mellett.

1. A SQL Server Management Studio eszközben csatlakozzon az éppen most telepítette az SQL Server. (Ha a **kapcsolódás a kiszolgálóhoz** párbeszédpanelen nem jelenik meg automatikusan, navigáljon a **Object Explorer** a bal oldali ablaktáblán kattintson a **Connect**, és kattintson a **adatbázismotor**.) ![Csatlakozás kiszolgálóhoz][SSMSConnectToServer]
   
    A **kiszolgálótípus**, válassza a **adatbázismotor**. A **kiszolgálónév**, használhat **localhost** vagy az Ön által használt számítógép nevét. Válasszon **SQL Server-hitelesítés**, majd jelentkezzen be a rendszergazdai felhasználónevet és a korábban létrehozott jelszót.
2. Az SQL Server Management Studio használatával hozzon létre egy új adatbázist, kattintson a jobb gombbal **adatbázisok** az Object Explorer, és kattintson a **új adatbázis**.
   
    ![Új adatbázis létrehozása][SSMScreateNewDB]
3. A a **új adatbázis** párbeszédpanelen adja meg MembershipDB az adatbázis nevét, és kattintson a **OK**.
   
    ![Adja meg az adatbázis neve][SSMSprovideDBname]
   
    Vegye figyelembe, hogy nem módosíthatja az adatbázis ezen a ponton. A tagsági információ bekerül később automatikusan a webes alkalmazás futtatásakor.
4. Az Object Explorerben, ha kibontja **adatbázisok**, látni fogja, hogy a tagsági adatbázisokból létrejött-e.
   
    ![MembershipDB létrehozása][SSMSMembershipDBCreated]

<a name="CreateSite"></a>

## <a name="b-create-a-web-app-in-the-azure-portal"></a>B. Webalkalmazás létrehozása az Azure portálon
> [!NOTE]
> Ha már létrehozott egy webalkalmazást az Azure portálon, hogy a jelen oktatóanyag használni kívánt, ugorjon előre [egy hibrid kapcsolat és a BizTalk szolgáltatás létrehozása](#CreateHC) és továbbra is onnan.
> 
> 

1. Az a [Azure Portal](https://portal.azure.com), kattintson a **új** > **Web + mobil** > **webalkalmazás**.
   
    ![Új gomb][New]
2. A webalkalmazás konfigurálása, és kattintson a **létrehozása**.
   
    ![Webhely neve][WebsiteCreationBlade]
3. Néhány másodpercen belül a web app jön létre, és a webalkalmazás panelen jelenik meg. A panel függőleges görgethető irányítópultot, amellyel kezelheti a webes alkalmazás.
   
    ![Futtató webhely][WebSiteRunningBlade]
   
    Annak ellenőrzéséhez, hogy a webalkalmazás élő, kattintson a **Tallózás** ikonra kattintva jelenítse meg az alapértelmezett lapon.

A következő létrehozhat egy hibrid kapcsolat és a BizTalk szolgáltatás a webalkalmazás.

<a name="CreateHC"></a>

## <a name="c-create-a-hybrid-connection-and-a-biztalk-service"></a>C. A hibrid kapcsolat és a BizTalk szolgáltatás létrehozása
1. Vissza a portálon, válassza a beállítások, és kattintson a **hálózati** > **hibrid kapcsolati végpontok konfigurálása**.
   
    ![Hibrid kapcsolatok][CreateHCHCIcon]
2. A hibrid kapcsolatok paneljén kattintson **Hozzáadás** > **új hibrid kapcsolat**.
3. Az a **hibrid kapcsolat létrehozása** panel:
   
   * A **neve**, adja meg a kapcsolat nevét.
   * A **állomásnév**, az SQL Server számítógép a számítógép nevének megadását.
   * A **Port**, adja meg az alapértelmezett 1433-as (az SQL Server).
   * Kattintson a **BizTalk szolgáltatás** > **új BizTalk szolgáltatás** , és írja be a BizTalk szolgáltatás nevét.
     
     ![A hibrid kapcsolat létrehozása][TwinCreateHCBlades]
4. Kattintson a **OK** kétszer.
   
    A folyamat befejezése után a **értesítések** terület villogjon, egy zöld **sikeres** és a **a hibrid kapcsolat** panelen jelennek meg az új hibrid kapcsolat a a állapotának **nincs csatlakoztatva**.
   
    ![Egy hibrid kapcsolat létrehozása][CreateHCOneConnectionCreated]

Fontos része a hibrid kapcsolat felhőalapú infrastruktúra ezen a ponton befejeződött. Ezután létrehozhat egy helyszíni megfelelő adat.

<a name="InstallHCM"></a>

## <a name="d-install-the-on-premises-hybrid-connection-manager-to-complete-the-connection"></a>D. A kapcsolat a helyszíni hibrid Csatlakozáskezelő telepítése
[!INCLUDE [app-service-hybrid-connections-manager-install](../../includes/app-service-hybrid-connections-manager-install.md)]

Most, hogy a hibrid kapcsolat infrastruktúra befejeződött, létrehozhat egy webes alkalmazás, amely használja ezt a szolgáltatást.

<a name="CreateASPNET"></a>

## <a name="e-create-a-basic-aspnet-web-project-edit-the-database-connection-string-and-run-the-project-locally"></a>E. Hozzon létre egy egyszerű ASP.NET webes projekt, az adatbázis-kapcsolati karakterlánc szerkesztése és a projekt helyi futtatása
### <a name="create-a-basic-aspnet-project"></a>Egy egyszerű ASP.NET-projekt létrehozása
1. A Visual Studio a a **fájl** menüben hozzon létre egy új projektet:
   
    ![Új Visual Studio-projekt][HCVSNewProject]
2. A a **sablonok** szakasza a **új projekt** párbeszédablakban válassza **webes** válassza **ASP.NET Web Application**, és kattintson a **OK**.
   
    ![ASP.NET webalkalmazás kiválasztása][HCVSChooseASPNET]
3. Az a **új ASP.NET projekt** párbeszédpanelen válasszon **MVC**, és kattintson a **OK**.
   
    ![MVC kiválasztása][HCVSChooseMVC]
4. A projekt létrehozása után az alkalmazás információs lap jelenik meg. Nem működnek a webes projekt még.
   
    ![Információs oldal][HCVSReadmePage]

### <a name="edit-the-database-connection-string-for-the-application"></a>Az adatbázis-kapcsolati karakterlánc az alkalmazás szerkesztése
Ebben a lépésben szerkeszteni a kapcsolati karakterlánc, amely közli az alkalmazás hol található a helyi SQL Server Express adatbázisban. A kapcsolati karakterlánc: az alkalmazás Web.config fájl, amely az alkalmazás konfigurációs információit tartalmazza.

> [!NOTE]
> Győződjön meg arról, hogy az alkalmazás használja az adatbázis az SQL Server Express, és nem a Visual Studio alapértelmezett LocalDB létrehozott, fontos, hogy a projekt futtatása előtt fejezze be ezt a lépést.
> 
> 

1. A Megoldáskezelőben kattintson duplán a Web.config fájlt.
   
    ![Web.config][HCVSChooseWebConfig]
2. Szerkessze a **connectionStrings** mutasson az SQL Server-adatbázis, a helyi számítógépen, a szintaxis a következő példában a következő szakaszban:
   
    ![Kapcsolati karakterlánc][HCVSConnectionString]
   
    A kapcsolati karakterlánc szerkesztésekor vegye figyelembe a következőket:
   
   * Ha egy alapértelmezett példány (például YourServer\SQLEXPRESS) helyett egy megnevezett példányt csatlakozik, konfigurálnia kell az SQL Server statikus port használatára. A statikus használt portok konfigurálásáról további információkért lásd: [konfigurálása egy adott portot az SQL Server](http://support.microsoft.com/kb/823938). Alapértelmezés szerint a elnevezett példányok UDP és dinamikus portok, amely nem támogatja a hibrid kapcsolatok használatára.
   * Javasoljuk, hogy megadja a port (a példában látható módon alapértelmezés szerint 1433) a kapcsolati karakterlánc, hogy akkor is ügyeljen arra, hogy a helyi SQL Server TCP engedélyezve van, és a megfelelő portot használ.
   * Fontos, hogy az SQL Server-hitelesítés szeretne csatlakozni, a felhasználói azonosító és jelszó megadásával a kapcsolati karakterláncban.
3. Kattintson a **mentése** a Visual Studio menteni a Web.config fájlt.

### <a name="run-the-project-locally-and-register-a-new-user"></a>A projekt helyi futtatása, és egy új felhasználó regisztrálása
1. Futtassa az új webes projekt helyi csoport hibakeresési a Tallózás gombra kattintva. Ez a példa az Internet Explorerben.
   
    ![Futtassa a projekt][HCVSRunProject]
2. A képernyő jobb felső sarkában az alapértelmezett webes lapra, válassza **regisztrálása** regisztrálni egy új fiókot:
   
    ![Egy új fiók regisztrálása][HCVSRegisterLocally]
3. Adjon meg egy felhasználónevet és jelszót:
   
    ![Adja meg a felhasználónevet és jelszót][HCVSCreateNewAccount]
   
    Ez automatikusan létrehoz egy adatbázis a helyi SQL-kiszolgálón, amely tárolja a csoporttagsági információkat az alkalmazás. A tábla (**dbo. AspNetUsers**) tartás webes alkalmazás felhasználói hitelesítő adatokat, mint a csak a megadott. Ebben a táblázatban láthatja az oktatóanyag későbbi részében.
4. Zárja be a böngészőablakot, az alapértelmezett weblap. Ezzel leállítja a Visual Studio alkalmazás.

Most már készen áll a következő lépéshez, és tegye közzé az alkalmazást az Azure-ba, és tesztelik azt.

<a name="PubNTest"></a>

## <a name="f-publish-the-web-application-to-azure-and-test-it"></a>F. Tegye közzé a webalkalmazást az Azure-ba, és tesztelik azt
Most lesz az alkalmazás az App Service webalkalmazásba közzétételére, és ellenőrizze, hogy kiderüljön, a hibrid kapcsolat korábban konfigurált használatának adatbázishoz való kapcsolódáshoz a webes alkalmazás a helyi gépen.

### <a name="publish-the-web-application"></a>Tegye közzé a webalkalmazást
1. A közzétételi profil az App Service webalkalmazás az Azure portálon töltheti le. A webalkalmazás paneljén kattintson **Get közzétételi profil**, és mentse a fájlt a számítógépre.
   
    ![Közzétételi profil letöltése][PortalDownloadPublishProfile]
   
    A következő importálni ezt a fájlt a Visual Studio web alkalmazásba.
2. A Visual Studióban, kattintson a jobb gombbal a projekt nevére a Megoldáskezelőben, és válassza ki **közzététel**.
   
    ![Válassza ki közzététele][HCVSRightClickProjectSelectPublish]
3. Az a **webhely közzététele** párbeszédpanelen, a a **profil** lapra, majd **importálási**.
   
    ![Importálás][HCVSPublishWebDialogImport]
4. Keresse meg a letöltött közzétételi profilt, válassza ki azt, és kattintson **OK**.
   
    ![Keresse meg a profil][HCVSBrowseToImportPubProfile]
5. A közzétételi információkat importálja, és megjeleníti a **kapcsolat** párbeszédpanel lapján.
   
    ![Kattintson a közzététel][HCVSClickPublish]
   
    Kattintson a **Publish** (Közzététel) gombra.
   
    Közzététel befejezése után a böngésző elindul, és a már ismerős ASP.NET-alkalmazás – megjelenítése, azzal a különbséggel, hogy most már az élő Azure felhőben!

A következő élő webalkalmazásokat használatával fog látni a hibrid kapcsolatot, a művelet.

### <a name="test-the-completed-web-application-on-azure"></a>Az elkészült webalkalmazás az Azure-on tesztelése
1. A felső sarkában található weblap az Azure-on, válassza ki **jelentkezzen be**.
   
    ![A teszt-napló][HCTestLogIn]
2. Az App Service webalkalmazásba most csatlakozik a webes alkalmazás tagsági adatbázisokból a helyi számítógépen. Ennek ellenőrzéséhez jelentkezzen be ugyanazon hitelesítő adatokkal, a helyi adatbázis korábban megadott.
   
    ![Hello üdvözlőlap][HCTestHelloContoso]
3. Az új hibrid kapcsolat további teszteléséhez jelentkezzen ki az Azure-webalkalmazás, és regisztrálja egy másik felhasználóként. Adjon meg egy új felhasználónevet és jelszót, és kattintson a **regisztrálása**.
   
    ![Teszt regisztrálja egy másik felhasználó][HCTestRegisterRelecloud]
4. Győződjön meg arról, hogy az új felhasználói hitelesítő adatok a hibrid kapcsolaton keresztül a helyi adatbázisban tárolja, a helyi számítógépen nyissa meg az SQL Management Studio. Az Object Explorerben bontsa ki a **MembershipDB** adatbázisából, és végül **táblák**. Kattintson a jobb gombbal a **dbo. AspNetUsers** tagsági tábla, és válassza a **legfelső 1000 sor kiválasztása** az eredmények megtekintéséhez.
   
    ![Az eredmények megtekintése][HCTestSSMSTree]
5. A helyi csoporttagság most táblázat fiókot is - helyileg létrehozó, és az Azure felhőben létrehozott. Az egy, a felhőben Azure hibrid kapcsolat funkción keresztül a helyi adatbázisba mentése megtörtént.
   
    ![A helyszíni adatbázisban regisztrált felhasználók][HCTestShowMemberDb]

Most már létrehozta és telepítve az ASP.NET webalkalmazások által használt egy hibrid kapcsolat egy webalkalmazást az Azure felhőben és helyszíni SQL Server-adatbázis között. Gratulálunk!

## <a name="see-also"></a>Lásd még:
[A hibrid kapcsolatok áttekintése](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[Josh a jelölés vezet be a hibrid kapcsolatok (videó a Channel 9)](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[A hibrid kapcsolatok áttekintése](/services/biztalk-services/)

[BizTalk szolgáltatások: Irányítópult, a figyelő, a méretezés, a konfigurálás és a hibrid kapcsolat lapokon](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[Zökkenőmentes alkalmazások hordozhatóságát (videó a Channel 9) egy valós hibrid felhő felépítése](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[Hozzáférés a helyszíni erőforrások hibrid kapcsolatok használata az Azure App Service-ben](web-sites-hybrid-connection-get-started.md)

[ASP.NET identitás áttekintése](http://www.asp.net/identity)

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

<!-- IMAGES -->
[SQLServerInstall]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A01SQLServerInstall.png
[ChooseDefaultInstance]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A02ChooseDefaultInstance.png
[ChooseMixedMode]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A03ChooseMixedMode.png
[SSMSConnectToServer]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A04SSMSConnectToServer.png
[SSMScreateNewDB]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A05SSMScreateNewDBlh.png
[SSMSprovideDBname]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A06SSMSprovideDBname.png
[SSMSMembershipDBCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A07SSMSMembershipDBCreated.png
[New]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B01New.png
[NewWebsite]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B02NewWebsite.png
[WebsiteCreationBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B03WebsiteCreationBlade.png
[WebSiteRunningBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B04WebSiteRunningBlade.png
[Browse]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B05Browse.png
[DefaultWebSitePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B06DefaultWebSitePage.png
[CreateHCHCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C01CreateHCHCIcon.png
[CreateHCAddHC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C02CreateHCAddHC.png
[TwinCreateHCBlades]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C03TwinCreateHCBlades.png
[CreateHCCreateBTS]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C04CreateHCCreateBTS.png
[CreateBTScomplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C05CreateBTScomplete.png
[CreateHCSuccessNotification]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C06CreateHCSuccessNotification.png
[CreateHCOneConnectionCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C07CreateHCOneConnectionCreated.png
[HCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D01HCIcon.png
[NotConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D02NotConnected.png
[NotConnectedBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D03NotConnectedBlade.png
[ClickListenerSetup]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D04ClickListenerSetup.png
[ClickToInstallHCM]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D05ClickToInstallHCM.png
[ApplicationRunWarning]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D06ApplicationRunWarning.png
[UAC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D07UAC.png
[HCMInstalling]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D08HCMInstalling.png
[HCMInstallComplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D09HCMInstallComplete.png
[HCStatusConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D10HCStatusConnected.png
[HCVSNewProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E01HCVSNewProject.png
[HCVSChooseASPNET]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E02HCVSChooseASPNET.png
[HCVSChooseMVC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E03HCVSChooseMVC.png
[HCVSReadmePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E04HCVSReadmePage.png
[HCVSChooseWebConfig]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E05HCVSChooseWebConfig.png
[HCVSConnectionString]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSConnectionString.png
[HCVSRunProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSRunProject.png
[HCVSRegisterLocally]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E07HCVSRegisterLocally.png
[HCVSCreateNewAccount]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E08HCVSCreateNewAccount.png
[PortalDownloadPublishProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F01PortalDownloadPublishProfile.png
[HCVSPublishProfileInDownloadsFolder]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F02HCVSPublishProfileInDownloadsFolder.png
[HCVSRightClickProjectSelectPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F03HCVSRightClickProjectSelectPublish.png
[HCVSPublishWebDialogImport]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F04HCVSPublishWebDialogImport.png
[HCVSBrowseToImportPubProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F05HCVSBrowseToImportPubProfile.png
[HCVSClickPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F06HCVSClickPublish.png
[HCTestLogIn]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F07HCTestLogIn.png
[HCTestHelloContoso]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F08HCTestHelloContoso.png
[HCTestRegisterRelecloud]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F09HCTestRegisterRelecloud.png
[HCTestSSMSTree]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F10HCTestSSMSTree.png
[HCTestShowMemberDb]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F11HCTestShowMemberDb.png
