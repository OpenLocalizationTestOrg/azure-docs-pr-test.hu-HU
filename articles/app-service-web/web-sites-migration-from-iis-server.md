---
title: "aaaMigrate egy vállalati webes alkalmazás tooAzure App Service"
description: "Bemutatja, hogyan toouse Web Apps alkalmazások áttelepítése Segéd tooquickly át a meglévő IIS webhelyek tooAzure App Service Web Apps"
services: app-service
documentationcenter: 
author: cephalin
writer: cephalin
manager: erikre
editor: 
ms.assetid: 2e846fc0-37cc-42e6-ac57-ff442ef16e85
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/01/2016
ms.author: cephalin
ms.openlocfilehash: 7d66c5b799f0eefe85cbd9ba596ee0a05167f295
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-an-enterprise-web-app-tooazure-app-service"></a>Telepítse át egy vállalati webes alkalmazás tooAzure App Service
Egyszerűen áttelepítheti a meglévő webhely, az Internet Information Service (IIS) 6 vagy újabb rendszerű túl[App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). 

> [!IMPORTANT]
> Windows Server 2003 megszűnik a 14. 2015. július támogatás érhető el. Ha jelenleg üzemeltet a webhelyeknek az IIS-kiszolgálón, amely a Windows Server 2003, Web Apps módja a alacsony kockázat, alacsony költségű és kis segíthet a webhelyek online, és a Web Apps alkalmazások áttelepítése Segéd tookeep hello áttelepítési folyamat automatizálásához meg. 
> 
> 

[Web Apps alkalmazások áttelepítése Segéd](https://www.movemetothecloud.net/) is elemezheti az IIS-kiszolgáló telepítése, mely helyek azonosítása is áttelepített tooApp szolgáltatás kell, jelölje ki a elemeket, amelyek nem telepíthetők át, vagy hello platformon nem támogatott, és telepítse át a webhelyek és társított adatbázisokban tooAzure.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="elements-verified-during-compatibility-analysis"></a>Kompatibilitási elemzési során ellenőrzött elemei
Áttelepítési Segéd hello hoz létre a készültségi jelentés tooidentify esetleges problémát jelent, vagy a problémák elhárítását, amelyek megakadályozhatják a sikeres áttelepítéshez a helyi IIS tooAzure App Service Web Apps okait. Néhány hello kulcsfontosságú tételekről toobe tisztában vannak:

* Port kötések – Web Apps csak támogatja a 80-as Port a HTTP és a 443-as portot a HTTPS-forgalmat. Másik portkonfigurációját figyelmen kívül hagyja, és a forgalom irányított too80 vagy 443-as lesz. 
* Hitelesítés – a Web Apps támogatja a névtelen hitelesítés alapértelmezés szerint és a az űrlapos hitelesítés Amennyiben egy olyan alkalmazás használja. Windows-hitelesítés csak integrálása az Azure Active Directory és az AD FS által használható. Hitelesítés – például alapszintű hitelesítés – egyéb kulcstárolást jelenleg nem támogatottak. 
* Globális szerelvény-gyorsítótárban (GAC) – hello webalkalmazások nem támogatja a GAC-ban. Ha az alkalmazás szerelvények hivatkozik, amely általában telepít toohello GAC-ba, szüksége lesz a toodeploy toohello alkalmazás bin mappájában a webalkalmazásokban. 
* IIS5 Kompatibilitási módban – ez nem támogatott a webalkalmazásokban. 
* Alkalmazáskészletek – a webalkalmazásokban, minden hely és annak gyermek alkalmazások futtatása hello ugyanabban az alkalmazáskészletben. Ha a webhely használatával több alkalmazáskészletek több alárendelt alkalmazások, egyesítheti őket egy alkalmazáskészletet tooa azokat a beállításokat, vagy minden egyes alkalmazás tooa külön webalkalmazás át.
* COM-összetevőket – a Web Apps hello platformon hello COM-összetevők regisztrálását teszi lehetővé. Ha a webhelyek vagy alkalmazások bármely COM-összetevőt használja, meg kell írja át őket felügyelt kódban, és azok hello webhely vagy alkalmazás.
* ISAPI-bővítmények – Web Apps ISAPI-bővítmények hello használatát is támogatja. Toodo hello következőkre lesz szüksége:
  
  * hello dll-EK és a webes alkalmazás központi telepítése 
  * hello dll-fájlok használatával regisztrálja [Web.config](http://www.iis.net/configreference/system.webserver/isapifilters)
  * helyezze egy applicationHost.xdt fájlt hello webhely gyökeréhez leírt "Arbitrart ISAPI-bővítmények toobe így betöltése" hello tartalmú [című szakaszát](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples) 
    
  
    
    További példákat toouse XML-dokumentum átalakítások a webhelyet, lásd: [átalakítás a Microsoft Azure webhely](http://blogs.msdn.com/b/waws/archive/2014/06/17/transform-your-microsoft-azure-web-site.aspx).
* Más összetevők, például a SharePoint, a front page server Extensions csomag (FrontPage Server Extensions), FTP, SSL-tanúsítványok nem lesznek áttelepítve.

## <a name="how-toouse-hello-web-apps-migration-assistant"></a>Hogyan toouse hello Web Apps alkalmazások áttelepítése Segéd
Ez a szakasz lépésről-lépésre egy példa tootoomigrate néhány webhelyek által használt SQL Server-adatbázis és egy Windows Server 2003 R2 (az IIS 6.0) a helyi számítógépen futó:

1. Hello IIS server vagy az ügyfélszámítógép keresse meg túl[https://www.movemetothecloud.net/](https://www.movemetothecloud.net/) 
   
   ![](./media/web-sites-migration-from-iis-server/migration-tool-homepage.png)
2. Telepítse a Web Apps alkalmazások áttelepítése Segéd hello kattintva **dedikált IIS-kiszolgálót** gombra. További beállítások lesznek a jövőben közelében hello beállítások. 
3. Kattintson a hello **Telepítőeszközének** gombra kattint, a számítógépre a Web Apps alkalmazások áttelepítése Segéd tooinstall.
   
   ![](./media/web-sites-migration-from-iis-server/install-page.png)
   
   > [!NOTE]
   > Is **töltse le a kapcsolat nélküli telepítés** toodownload egy ZIP-fájlja telepítése a kiszolgálókon nem csatlakoztatott toohello internet. Másik lehetőségként kattinthat **feltöltése a meglévő áttelepítési készültségi jelentés**, ez egy speciális beállítás toowork meglévő áttelepítési készültségi jelentést, amely a korábban létrehozott (később ismertetése).
   > 
   > 
4. A hello **az alkalmazás telepítési** kattintson **telepítése** tooinstall a számítógépen. Például a Web Deploy DacFX és IIS-t, a hozzájuk tartozó függőségek azt is telepíti, szükség esetén. 
   
   ![](./media/web-sites-migration-from-iis-server/install-progress.png)
   
   A telepítést követően Web Apps alkalmazások áttelepítése segéd automatikusan elindul.
5. Válasszon **webhelyek és -adatbázisok át egy távoli kiszolgáló tooAzure**. Adja meg hello távoli kiszolgáló hello rendszergazdai hitelesítő adatokat, majd kattintson **Folytatás**. 
   
   ![](./media/web-sites-migration-from-iis-server/migrate-from-remote.png)
   
   Természetesen toomigrate választhat hello helyi kiszolgáló. hello távoli beállítás akkor hasznos, ha azt szeretné, hogy toomigrate webhelyeket az éles az IIS-kiszolgálón.
   
   Ezen a ponton vizsgálja a hello áttelepítési eszköz hello az IIS-kiszolgáló konfigurációját, például webhelyek, alkalmazások, alkalmazáskészletek és függőségei tooidentify jelölt webhelyeket az áttelepítéshez. 
6. az alábbi hello képernyőfelvételen látható három webhelyek – **alapértelmezett webhely**, **TimeTracker**, és **CommerceNet4**. Az összes van, hogy szeretnénk toomigrate társított adatbázis. Válassza ki az összes hello helyek kívánja tooassess, majd kattintson a **következő**.
   
   ![](./media/web-sites-migration-from-iis-server/select-migration-candidates.png)
7. Kattintson a **feltöltése** tooupload hello készültségi jelentést. Ha **mentse helyileg a fájlt**, később újra futtathatja hello áttelepítési eszköz és a feltöltés hello készültségi jelentés mentése, ahogy azt korábban említettük.
   
   ![](./media/web-sites-migration-from-iis-server/upload-readiness-report.png)
   
   Hello készültségi jelentés feltöltése után Azure-ban készültségének elemzése és akkor hello eredményeit jeleníti meg. Hello assessment részleteiből tájékozódjon azokról az minden webhelyre, és győződjön meg arról, ismer, vagy foglalkoztak kapcsolatos összes problémát, mielőtt továbblép. 
   
   ![](./media/web-sites-migration-from-iis-server/readiness-assessment.png)
8. Kattintson a **megkezdéséhez áttelepítési** toostart hello áttelepítési. Most lesz átirányított tooAzure toolog a fiókjába. Fontos, hogy jelentkezzen be egy aktív Azure-előfizetéssel rendelkező fiókkal. Ha Ön nem rendelkezik Azure-fiókra, majd regisztrálhat egy ingyenes próbaverzióra [Itt](https://azure.microsoft.com/pricing/free-trial/?WT.srch=1&WT.mc_ID=SEM_). 
9. Válassza ki a hello bérlői fiókot, Azure-előfizetés és az áttelepített az Azure web apps és az adatbázisok régió toouse, és kattintson a **Start áttelepítési**. Hello webhelyek toomigrate később is kiválaszthatja.
   
   ![](./media/web-sites-migration-from-iis-server/choose-tenant-account.png)
10. A következő képernyőn hello segítségével módosíthatja toohello alapértelmezett áttelepítési beállításokat, például:
    
    * egy meglévő Azure SQL Database szolgáltatást használna, vagy hozzon létre egy új Azure SQL Database, és a hitelesítő adatok beállítása
    * Válassza ki a hello webhelyek toomigrate
    * hello Azure web Apps alkalmazások és a csatolt SQL-adatbázis nevének megadása
    * globális beállítások hello és webhelyszintű beállítások testreszabása
    
    hello Képernyőkép az alábbi áttelepítési hello alapértelmezett beállításait a kijelölt összes hello webhely jeleníti meg.
    
    ![](./media/web-sites-migration-from-iis-server/migration-settings.png)
    
    > [!NOTE]
    > Hello **Azure Active Directory engedélyezése** jelölőnégyzetet egyéni beállítások integrálja hello Azure web app az [Azure Active Directory](../active-directory/active-directory-whatis.md) (hello **alapértelmezett címtárat**). A szinkronizálás a helyszíni Active Directoryból az Azure Active Directory további információkért lásd: [címtár-integráció](http://msdn.microsoft.com/library/jj573653).
    > 
    > 
11. Ha módosítja az összes szükséges hello, kattintson **létrehozása** toostart hello áttelepítési folyamat. hello áttelepítési eszköz hello Azure SQL Database és az Azure-webalkalmazás létrehozása, és tegye közzé a hello webhelytartalmat és adatbázisokat. hello áttelepítési folyamat jól látható hello áttelepítési eszköz, és megjelenik egy összegző képernyő hello end, mely részleteket hello helyek át, hogy sikeresek voltak-e, hivatkozások toohello újonnan létrehozott Azure-webalkalmazásokban. 
    
    Ha a hiba akkor fordul elő, az áttelepítés során hello az áttelepítési eszköz lesz egyértelműen jelzi hello hibákhoz és visszaállítási hello módosításokat. Fogja tudni toosend hello esetleges hibajelentésben való megjelenítéshez közvetlen toohello mérnöki csapat hello kattintva **Hibajelentés küldése** hello rögzített hiba hívásveremmel rendelkező gombra, és az üzenet törzse felépítéséhez. 
    
    ![](./media/web-sites-migration-from-iis-server/migration-error-report.png)
    
    Ha áttelepítése sikeres, a hiba nélkül is kattinthat hello **visszajelzés** gomb tooprovide olyan visszajelzést közvetlenül. 
12. Kattintson a hello hivatkozások toohello Azure web apps, és győződjön meg arról, hogy sikerült-e hello áttelepítési.
13. Kezelheti az hello az Azure App Service web Apps alkalmazások áttelepítése. toodo e, jelentkezzen be a hello [Azure Portal](https://portal.azure.com).
14. Hello Azure portálon, a hello webalkalmazások panel toosee az áttelepített webhelyek megnyitását (megjelennek az helyeként webalkalmazások), majd kattintson az egyik őket toostart kezelése hello web app alkalmazásban például a folyamatos közzététele, biztonsági mentések, automatikus skálázás, és használati figyelés konfigurálása vagy teljesítmény.
    
    ![](./media/web-sites-migration-from-iis-server/TimeTrackerMigrated.png)

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="whats-changed"></a>A változások
* Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

