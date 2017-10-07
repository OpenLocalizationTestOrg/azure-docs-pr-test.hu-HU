---
title: "aaaConnect tooon helyszíni SQL Server rendszert egy webalkalmazást az Azure App Service hibrid kapcsolatok használata"
description: "A Microsoft Azure-webalkalmazás létrehozása, és csatlakoztassa tooan a helyszíni SQL Server-adatbázis"
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
ms.openlocfilehash: 2e8f8f7e0b9733cfb0433697615faba4358c6023
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooon-premises-sql-server-from-a-web-app-in-azure-app-service-using-hybrid-connections"></a>Kapcsolódó tooon helyszíni SQL Server egy webalkalmazást az Azure App Service hibrid kapcsolatok használata
Hibrid kapcsolatok kapcsolódhatnak [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) statikus TCP-portot használó webalkalmazások tooon helyszíni erőforrások. Támogatott erőforrások közé tartoznak a Microsoft SQL Server, a MySQL, a HTTP webes API-k, a App Service és a legtöbb egyéni webszolgáltatások.

Ebből az oktatóanyagból megtudhatja, hogyan toocreate egy App Service webalkalmazás a hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715), hello web app tooyour helyi helyszíni SQL Server adatbázis hello új hibrid kapcsolat funkció használata csatlakozáshoz, hozzon létre egy egyszerű ASP.NET az alkalmazás hello hibrid kapcsolat használatát, és hello alkalmazás toohello App Service-webalkalmazások telepítése. hello végrehajtani a webalkalmazás Azure felhasználói hitelesítő adatokat, amely a helyszíni tagsági adatbázisban tárolja. hello oktatóanyag feltételezi, hogy nincs korábbi tapasztalata az Azure vagy az ASP.NET használatával kapcsolatban.

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> hello webalkalmazások hello hibrid kapcsolatok szolgáltatás része csak a hello [Azure Portal](https://portal.azure.com). BizTalk szolgáltatások, a kapcsolat toocreate lásd [hibrid kapcsolatok](http://go.microsoft.com/fwlink/p/?LinkID=397274).  
> 
> 

## <a name="prerequisites"></a>Előfeltételek
toocomplete ebben az oktatóanyagban szüksége lesz a következő termékek hello. Az összes szabad verziók, elérhetők nekiláthat teljes mértékben az Azure ingyenes.

* **Azure-előfizetés** - ingyenes előfizetésre, lásd: [Azure ingyenes próbaverzió](/pricing/free-trial/).
* **A Visual Studio 2013** -toodownload egy ingyenes próbaverzióját Visual Studio 2013, lásd: [Visual Studio letöltések](http://www.visualstudio.com/downloads/download-visual-studio-vs). Telepítse a folytatás előtt.
* **A Microsoft .NET Framework 3.5 Service Pack 1** – Ha az operációs rendszer Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7, vagy Windows Server 2008 R2, engedélyezheti a Vezérlőpult > Programok és szolgáltatások > Windows-szolgáltatások be- és kikapcsolása. Ellenkező esetben letöltheti a hello [Microsoft Download Center](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22).
* **SQL Server 2014 Express eszközökkel** -Microsoft SQL Server Express ingyenesen letölthető: hello [Microsoft Web Platform adatbázissal kapcsolatos beállításainak lapja](http://www.microsoft.com/web/platform/database.aspx). Válassza ki a hello **Express** (nem LocalDB) verziója. Hello **eszközökkel Express** tartalmazza az ebben az oktatóanyagban használandó SQL Server Management Studio.
* **SQL Server Management Studio Express** – Ez az SQL Server 2014 Express hello tartalmazza a fent említett eszközök letöltése, de ha tooinstall van szüksége, külön-külön, letöltheti és telepítse azt hello [SQL Server Express letöltési oldalát](http://www.microsoft.com/web/platform/database.aspx).

hello oktatóanyag feltételezi, hogy rendelkezik-e az Azure-előfizetésre, hogy a Visual Studio 2013 telepítve van, és, hogy telepítve vagy engedélyezve van a .NET-keretrendszer 3.5. hello oktatóanyag bemutatja, hogyan a konfigurációkat, amelyek hello Azure hibrid kapcsolatok jól működik az SQL Server 2014 Express tooinstall funkció (egy alapértelmezett példány statikus TCP-port). Hello oktatóanyag megkezdése előtt letöltése SQL Server 2014 Express eszközökkel hello fent említett, ha nincs telepítve az SQL Server.

### <a name="notes"></a>Megjegyzések
toouse egy helyi SQL Server vagy SQL Server Express adatbázist egy hibrid kapcsolat, az TCP/IP toobe statikus port engedélyezve van szüksége. SQL Server alapértelmezett példány statikus 1433-as port használja, mivel elnevezett példányok azonban nem.

hello számítógép, amelyen hello helyszíni Hybrid Connection Manager ügynököt telepíti:

* Kimenő kapcsolódás tooAzure kell rendelkeznie a keresztül:

| Port | miért |
| --- | --- |
| 80 |**Szükséges** leellenőrizni a tanúsítvány és választhatóan adatkapcsolattal HTTP-porthoz. |
| 443 |**Nem kötelező** az adatkapcsolat. Kimenő kapcsolódás too443 nem érhető el, ha a 80-as TCP-port használatos. |
| 5671 és 9352 |**Ajánlott** , de az adatkapcsolat nem kötelező. Megjegyzés: Ebben a módban általában nagyobb átviteli teljesítményt eredményez. Kimenő kapcsolódás toothese portok nem érhető el, ha a 443-as TCP-port használatos. |

* Képes tooreach hello kell *állomásnév*:*portszám* a helyi erőforrás.

Ez a cikk hello lépések azt feltételezik, hogy hello helyszíni hibrid kapcsolat ügynököt futtató számítógépről hello hello böngészőt használ.

Ha már van telepítve a konfigurációban, és olyan környezetben, a fent leírt hello feltételeknek megfelelő SQL Server, kihagyhatja azokat, amelyek, és kezdje [egy SQL Server-adatbázis létrehozása a helyszínen](#CreateSQLDB).

<a name="InstallSQL"></a>

## <a name="a-install-sql-server-express-enable-tcpip-and-create-a-sql-server-database-on-premises"></a>A. Telepítse az SQL Server Expresst, engedélyezze a TCP/IP protokollt, és a helyszíni SQL Server-adatbázis létrehozása
Ez a szakasz bemutatja, hogyan tooinstall SQL Server Express, engedélyezze a TCP/IP protokollt, és, hogy a webes alkalmazás működni fog-e hello Azure Portal-adatbázis létrehozása.

### <a name="install-sql-server-express"></a>SQL Server Express telepítése
1. hello futtassa az SQL Server Express tooinstall **SQLEXPRWT_x64_ENU.exe** vagy **SQLEXPR_x86_ENU.exe** letöltött fájlban. hello SQL Server telepítési központjának varázsló jelenik meg.
   
    ![SQL-kiszolgáló telepítése][SQLServerInstall]
2. Válasszon **új SQL Server önálló telepítése vagy szolgáltatások tooan meglévő telepítési hozzáadása**. Útmutatás alapján hello, elfogadása hello alapértelmezett választási lehetőségek és beállítások, amíg elér toohello **példány konfigurációja** lap.
3. A hello **példány konfigurációja** lapon, válassza ki **alapértelmezett példány**.
   
    ![Válassza ki az alapértelmezett példány][ChooseDefaultInstance]
   
    A statikus 1433-as port az SQL Server-ügyfelektől érkező kéréseket figyeli az SQL Server alapértelmezett példányát hello alapértelmezés szerint ez milyen hello szolgáltatáshoz szükséges a hibrid kapcsolatok. Elnevezett példányok használata a dinamikus portok és UDP, amely nem támogatja a hibrid kapcsolatok.
4. Fogadja el a hello hello alapértelmezett **kiszolgálókonfiguráció** lap.
5. A hello **adatbázismotor beállítása** lap **hitelesítési mód**, válassza ki **vegyes üzemmódú (SQL Server-hitelesítést és a Windows-hitelesítés)**, és adja meg a jelszó.
   
    ![Válassza ki a kevert üzemmód][ChooseMixedMode]
   
    Ebben az oktatóanyagban fog használni az SQL Server-hitelesítést. Lehet, hogy tooremember hello jelszót ad meg, mert később szüksége lesz az.
6. Hello részeinek hello varázsló toocomplete hello telepítési lépéseit.

### <a name="enable-tcpip"></a>Engedélyezze a TCP/IP protokollt
TCP/IP tooenable, szüksége lesz a lett telepítve az SQL Server Express telepítése során az SQL Server Configuration Manager. Hello kövesse [engedélyezése TCP/IP hálózati protokollt az SQL Server](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) folytatása előtt.

<a name="CreateSQLDB"></a>

### <a name="create-a-sql-server-database-on-premises"></a>A helyszíni SQL Server-adatbázis létrehozása
A Visual Studio webalkalmazás az Azure-ban elérhető tagsági adatbázis szükséges. Ehhez szükséges, hogy egy SQL Server vagy SQL Server Express adatbázist (nem hello LocalDB adatbázisban, amely az MVC-sablont használ hello alapértelmezés szerint), így hello tagsági adatbázisokból mellett hoz létre.

1. Az SQL Server Management Studio eszközben csatlakozzon a toohello éppen most telepítette az SQL Server. (Ha hello **tooServer csatlakozás** párbeszédpanelen nem jelenik meg automatikusan, keresse meg a túl**Object Explorer** hello bal oldali ablaktáblában kattintson **Connect**, és kattintson a **Adatbázis-motor**.) ![Csatlakozás tooServer][SSMSConnectToServer]
   
    A **kiszolgálótípus**, válassza a **adatbázismotor**. A **kiszolgálónév**, használhat **localhost** vagy hello számítógép által használt hello nevét. Válasszon **SQL Server-hitelesítés**, majd jelentkezzen be hello sa felhasználónév és a korábban létrehozott hello jelszót.
2. Kattintson jobb gombbal az új adatbázis SQL Server Management Studio használatával toocreate **adatbázisok** az Object Explorer, és kattintson a **új adatbázis**.
   
    ![Új adatbázis létrehozása][SSMScreateNewDB]
3. A hello **új adatbázis** párbeszédpanelen adja meg MembershipDB hello adatbázis nevét, és kattintson a **OK**.
   
    ![Adja meg az adatbázis neve][SSMSprovideDBname]
   
    Vegye figyelembe, hogy nem végzi el a módosításokat toohello adatbázis ezen a ponton. hello csoporttagsági információkat megkapja később automatikusan a webes alkalmazás futtatásakor.
4. Az Object Explorerben, ha kibontja **adatbázisok**, látni fogja, hogy hello tagsági az adatbázis létrehozását.
   
    ![MembershipDB létrehozása][SSMSMembershipDBCreated]

<a name="CreateSite"></a>

## <a name="b-create-a-web-app-in-hello-azure-portal"></a>B. A webalkalmazás létrehozása az Azure portál hello
> [!NOTE]
> Ha már létrehozott egy webalkalmazást az Azure portálon megjeleníteni kívánt toouse ebben az oktatóanyagban hello, akkor kihagyhatja azokat, amelyek túl[egy hibrid kapcsolat és a BizTalk szolgáltatás létrehozása](#CreateHC) és továbbra is onnan.
> 
> 

1. A hello [Azure Portal](https://portal.azure.com), kattintson a **új** > **Web + mobil** > **webalkalmazás**.
   
    ![Új gomb][New]
2. A webalkalmazás konfigurálása, és kattintson a **létrehozása**.
   
    ![Webhely neve][WebsiteCreationBlade]
3. Néhány másodpercen belül hello webalkalmazás jön létre, és a webalkalmazás panelen jelenik meg. hello panel függőleges görgethető irányítópultot, amellyel kezelheti a webes alkalmazás.
   
    ![Futtató webhely][WebSiteRunningBlade]
   
    élő tooverify hello webalkalmazás, rákattinthat hello **Tallózás** ikon toodisplay hello alapértelmezett oldalt.

Ezt követően egy hibrid kapcsolat és a BizTalk szolgáltatás hello webalkalmazás hoz létre.

<a name="CreateHC"></a>

## <a name="c-create-a-hybrid-connection-and-a-biztalk-service"></a>C. A hibrid kapcsolat és a BizTalk szolgáltatás létrehozása
1. Vissza a hello Portal, nyissa meg toosettings és **hálózati** > **hibrid kapcsolati végpontok konfigurálása**.
   
    ![Hibrid kapcsolatok][CreateHCHCIcon]
2. Hello hibrid kapcsolatok paneljén kattintson **Hozzáadás** > **új hibrid kapcsolat**.
3. A hello **hibrid kapcsolat létrehozása** panel:
   
   * A **neve**, hello kapcsolat nevét adja meg.
   * A **állomásnév**, az SQL Server számítógép hello számítógép nevének megadását.
   * A **Port**, adja meg az alapértelmezett 1433-as (hello az SQL Server).
   * Kattintson a **BizTalk szolgáltatás** > **új BizTalk szolgáltatás** , és írja be a hello BizTalk szolgáltatás nevét.
     
     ![A hibrid kapcsolat létrehozása][TwinCreateHCBlades]
4. Kattintson a **OK** kétszer.
   
    Hello folyamat befejeződésekor hello **értesítések** terület villogjon, egy zöld **sikeres** és hello **a hibrid kapcsolat** panelen megjelenik a hello új hibrid kapcsolat az hello állapotának **nincs csatlakoztatva**.
   
    ![Egy hibrid kapcsolat létrehozása][CreateHCOneConnectionCreated]

Ezen a ponton befejeződött hello felhőalapú hibrid kapcsolat infrastruktúra fontos része. Ezután létrehozhat egy helyszíni megfelelő adat.

<a name="InstallHCM"></a>

## <a name="d-install-hello-on-premises-hybrid-connection-manager-toocomplete-hello-connection"></a>D. Hello helyszíni Hybrid Connection Manager toocomplete hello kapcsolat telepítése
[!INCLUDE [app-service-hybrid-connections-manager-install](../../includes/app-service-hybrid-connections-manager-install.md)]

Most, hogy hello hibrid kapcsolat infrastruktúra befejeződött, létrehozhat egy webes alkalmazás, amely használja ezt a szolgáltatást.

<a name="CreateASPNET"></a>

## <a name="e-create-a-basic-aspnet-web-project-edit-hello-database-connection-string-and-run-hello-project-locally"></a>E. Hozzon létre egy egyszerű ASP.NET webes projekt hello adatbázis-kapcsolati karakterlánc szerkesztése és hello projekt helyi futtatása
### <a name="create-a-basic-aspnet-project"></a>Egy egyszerű ASP.NET-projekt létrehozása
1. A Visual Studio, a hello **fájl** menüben hozzon létre egy új projektet:
   
    ![Új Visual Studio-projekt][HCVSNewProject]
2. A hello **sablonok** hello szakasza **új projekt** párbeszédablakban válassza **webes** , és válassza **ASP.NET Web Application**, és kattintson a  **OK**.
   
    ![ASP.NET webalkalmazás kiválasztása][HCVSChooseASPNET]
3. A hello **új ASP.NET projekt** párbeszédpanelen válasszon **MVC**, és kattintson a **OK**.
   
    ![MVC kiválasztása][HCVSChooseMVC]
4. Hello projekt létrehozásakor hello alkalmazás információs lap jelenik meg. Ne futtassa a még hello webes projektet.
   
    ![Információs oldal][HCVSReadmePage]

### <a name="edit-hello-database-connection-string-for-hello-application"></a>Adatbázis-kapcsolati karakterlánc hello a hello alkalmazás szerkesztése
Ebben a lépésben szerkeszteni hello kapcsolati karakterlánc, amely közli az alkalmazás hol toofind a helyi SQL Server Express adatbázist. hello kapcsolati karakterlánca hello alkalmazás Web.config fájl, amely hello alkalmazás konfigurációs információit tartalmazza.

> [!NOTE]
> az alkalmazás által használt hello adatbázis az SQL Server Express, és nem hello egy Visual Studio alapértelmezett LocalDB létrehozott tooensure, fontos, hogy a projekt futtatása előtt fejezze be ezt a lépést.
> 
> 

1. A Megoldáskezelőben kattintson duplán a hello Web.config fájlt.
   
    ![Web.config][HCVSChooseWebConfig]
2. Hello szerkesztése **connectionStrings** szakasz toopoint toohello SQL Server-adatbázisban a helyi gépén, a következő példa hello hello szintaxist a következő:
   
    ![Kapcsolati karakterlánc][HCVSConnectionString]
   
    Hello kapcsolati karakterlánc szerkesztésekor tartsa szem előtt tartva hello következő:
   
   * Ha megnevezett példány helyett egy alapértelmezett példány (például YourServer\SQLEXPRESS) tooa kapcsolódik, konfigurálnia kell az SQL-kiszolgáló toouse statikus port. A statikus használt portok konfigurálásáról további információkért lásd: [hogyan tooconfigure SQL Server toolisten egy adott portot](http://support.microsoft.com/kb/823938). Alapértelmezés szerint a elnevezett példányok UDP és dinamikus portok, amely nem támogatja a hibrid kapcsolatok használatára.
   * Javasoljuk, hogy megadja a hello port hello kapcsolati karakterlánc (hello példa látható alapértelmezés szerint 1433), így biztosíthatja, hogy a helyi SQL Server TCP engedélyezve van, és hello megfelelő portot használ.
   * Ne felejtse el toouse SQL Server-hitelesítés tooconnect hello Felhasználóazonosító és jelszó megadásával a kapcsolati karakterláncban.
3. Kattintson a **mentése** Visual Studio toosave hello Web.config fájlban.

### <a name="run-hello-project-locally-and-register-a-new-user"></a>Futtassa helyileg a hello projektet, és regisztrálni egy új felhasználót
1. Futtassa az új webes projekt helyi csoport hibakeresési hello Tallózás gombra kattintva. Ez a példa az Internet Explorerben.
   
    ![Futtassa a projekt][HCVSRunProject]
2. Az hello jobb felső sarkában hello alapértelmezett weblapot, majd válassza **regisztrálása** tooregister egy új fiókot:
   
    ![Egy új fiók regisztrálása][HCVSRegisterLocally]
3. Adjon meg egy felhasználónevet és jelszót:
   
    ![Adja meg a felhasználónevet és jelszót][HCVSCreateNewAccount]
   
    Ez automatikusan létrehoz egy adatbázis SQL-kiszolgálón a helyi tároló hello csoporttagsági információkat az alkalmazás. Hello tábla (**dbo. AspNetUsers**) tartás webes alkalmazás felhasználói hitelesítő adatok, például hello azokat az előbb hozzáadott. Ez a táblázat hello oktatóanyag későbbi részében jelenik meg.
4. Zárja be a böngészőablakot hello hello alapértelmezett weblap. Ezzel leállítja a hello alkalmazást a Visual Studio.

Most már készen áll a hello következő lépésre, amely toopublish hello alkalmazás tooAzure és tesztelik azt.

<a name="PubNTest"></a>

## <a name="f-publish-hello-web-application-tooazure-and-test-it"></a>F. Hello webes alkalmazás tooAzure közzététele és tesztelik azt
Most, lesz az alkalmazás tooyour App Service webalkalmazás közzététele, és korábban konfigurált hello hibrid kapcsolat Mitől toosee tesztelése alatt használt tooconnect a webes alkalmazás toohello adatbázis a helyi számítógépen.

### <a name="publish-hello-web-application"></a>Hello webes alkalmazás közzététele
1. A közzétételi profil hello App Service webalkalmazás az Azure portál hello töltheti le. A webalkalmazás hello paneljén kattintson **Get közzétételi profil**, majd mentse a hello fájl tooyour számítógép.
   
    ![Közzétételi profil letöltése][PortalDownloadPublishProfile]
   
    A következő importálni ezt a fájlt a Visual Studio web alkalmazásba.
2. A Visual Studióban, kattintson a jobb gombbal a hello projekt nevére a Megoldáskezelőben, és válassza ki **közzététel**.
   
    ![Válassza ki közzététele][HCVSRightClickProjectSelectPublish]
3. A hello **webhely közzététele** párbeszédpanel, hello **profil** lapra, majd **importálási**.
   
    ![Importálás][HCVSPublishWebDialogImport]
4. Tallózás tooyour letöltött közzétételi profil, válassza ki azt, és kattintson **OK**.
   
    ![Keresse meg a tooprofile][HCVSBrowseToImportPubProfile]
5. A közzétételi információkat importálja, és megjeleníti a hello **kapcsolat** hello párbeszédpanel lapján.
   
    ![Kattintson a közzététel][HCVSClickPublish]
   
    Kattintson a **Publish** (Közzététel) gombra.
   
    Közzététel befejezése után a böngésző elindul, és a már ismerős ASP.NET-alkalmazás – megjelenítése, azzal a különbséggel, hogy mostantól az Azure-felhőbe hello élő!

A következő fogja használni az élő webes alkalmazás toosee a hibrid kapcsolat működés közben.

### <a name="test-hello-completed-web-application-on-azure"></a>Teszt hello Azure webalkalmazás befejeződött
1. Hello felső sarkában található weblap az Azure-on, válassza ki **jelentkezzen be**.
   
    ![A teszt-napló][HCTestLogIn]
2. Az App Service webalkalmazás már csatlakoztatott tooyour webes alkalmazás tagsági adatbázisokból a helyi számítógépen. tooverify e, jelentkezzen be a korábbi adatbázis-hitelesítő adatokkal, hogy a megadott hello helyi hello.
   
    ![Hello üdvözlőlap][HCTestHelloContoso]
3. toofurther az új hibrid kapcsolat tesztelése, jelentkezzen ki az Azure-webalkalmazás, és regisztrálja egy másik felhasználóként. Adjon meg egy új felhasználónevet és jelszót, és kattintson a **regisztrálása**.
   
    ![Teszt regisztrálja egy másik felhasználó][HCTestRegisterRelecloud]
4. tooverify, hogy hello új felhasználói hitelesítő adatok a hibrid kapcsolaton keresztül a helyi adatbázisban tárolja a helyi számítógépen nyissa meg az SQL Management Studio. Az Object Explorerben bontsa ki a hello **MembershipDB** adatbázisából, és végül **táblák**. Kattintson a jobb gombbal hello **dbo. AspNetUsers** tagsági tábla, és válassza a **legfelső 1000 sor kiválasztása** tooview hello eredmények.
   
    ![Hello eredmények megtekintése][HCTestSSMSTree]
5. A helyi csoporttagság most táblázat mindkét fiókok – hello helyileg létre, és egy, az Azure-felhőbe hello létrehozott hello. hello felhőben létre hello Azure hibrid kapcsolat funkción keresztül tooyour a helyi adatbázis mentése megtörtént.
   
    ![A helyszíni adatbázisban regisztrált felhasználók][HCTestShowMemberDb]

Most már létrehozta és telepítve az ASP.NET webalkalmazások által használt egy hibrid kapcsolat egy webalkalmazást az Azure-felhőbe hello és a helyszíni SQL Server-adatbázis között. Gratulálunk!

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
