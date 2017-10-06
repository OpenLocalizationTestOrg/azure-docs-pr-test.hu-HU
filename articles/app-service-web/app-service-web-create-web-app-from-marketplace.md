---
title: "a webes alkalmazás hello Azure Piactérről származó aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy új WordPress webalkalmazást az Azure piactér hello segítségével hello Azure portálon."
services: app-service\web
documentationcenter: 
author: sunbuild
manager: erikre
editor: 
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: sunbuild
ms.custom: mvc
ms.openlocfilehash: 5ad1ca2f3f7831d857c3e9b02738b6b34acf3649
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-from-hello-azure-marketplace"></a>Webalkalmazás létrehozása az Azure piactér hello
<!-- Note: This article replaces web-sites-php-web-site-gallery.md -->

[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

hello Azure piactér nyílt forráskódú szoftvereket Közösségek, például: WordPress és Umbraco CMS által fejlesztett népszerű webalkalmazások széles skáláját biztosítja. Ebben az oktatóanyagban elsajátíthatja, hogyan toocreate WordPress alkalmazás az Azure piactérről.
ami létrehoz egy Azure Web App és a MySQL-adatbázist. 

![Példa WordPress webes alkalmazás irányítópult](./media/app-service-web-create-web-app-from-marketplace/wpdashboard2.png)

## <a name="before-you-begin"></a>Előkészületek 

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

## <a name="deploy-from-azure-marketplace"></a>Az Azure piactérről telepítése
Lépések hello toodeploy WordPress alatt az Azure piactérről.

### <a name="sign-in-tooazure"></a>Jelentkezzen be tooAzure
Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).

### <a name="deploy-wordpress-template"></a>WordPress-sablon üzembe helyezése
hello Azure piactér tartalmaz sablonokat erőforrások, a telepítő hello beállítása [WordPress](https://portal.azure.com/#create/WordPress.WordPress) sablon tooget elindult.
   
Írja be a következő hello információk toodeploy hello WordPress alkalmazás és az erőforrásait.

  ![WordPress folyamat létrehozása](./media/app-service-web-create-web-app-from-marketplace/wordpress-portal-create.png)


| Mező         | Ajánlott érték           | Leírás  |
| ------------- |-------------------------|-------------|
| Alkalmazás neve      | mywordpressapp          | Adjon meg egy egyedi alkalmazásnévvel a a **webalkalmazásnév**. Ez a név hello alapértelmezett DNS-neve az alkalmazás részeként használatos `<app_name>.azurewebsites.net`, így kell toobe egyedi az Azure-ban minden alkalmazások között. Később hozzárendelhető egy egyéni tartomány nevét tooyour alkalmazást ki kell alakítani az tooyour felhasználók |
| Előfizetés  | Utólagos, használatalapú fizetés             | Válasszon ki egy **előfizetést**. Ha több előfizetéssel rendelkezik, válasszon hello megfelelő előfizetést. |
| Erőforráscsoport| mywordpressappgroup                 |    Adjon meg egy **erőforráscsoport**. Erőforráscsoport egy olyan logikai tároló, például webalkalmazások, adatbázisok, amelyek telepítése és felügyelete az Azure erőforrások be. Hozzon létre egy erőforráscsoportot, vagy használjon egy meglévőt |
| App Service-csomag | myappplan          | App Service-csomagokról képviselő használt fizikai erőforrások toohost hello gyűjteményét az alkalmazásokat. Jelölje be hello **hely** és hello **tarifacsomag**. Az árakkal kapcsolatos további információkért lásd: [App service tarifacsomag](https://azure.microsoft.com/pricing/details/app-service/) |
| Adatbázis      | mywordpressapp          | Válassza ki a megfelelő adatbázis-szolgáltató hello a MySQL. Webes alkalmazások által támogatott **ClearDB**, **MySQL az Azure-adatbázis** és **MySQL alkalmazásbeli**. További részletekért lásd: [adatbázis konfigurációja](#database-config) az alábbi szakasz. |
| Application Insights | ON vagy OFF          | Ez nem kötelező. [Az Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) a webalkalmazás figyelési szolgáltatásokat nyújt a kattintva **ON**.|

<a name="database-config"></a>

### <a name="database-configuration"></a>Adatbázis-konfiguráció
Hello lépések végrehajtásával az alábbi MySQL adatbázis-szolgáltató a választott.  Ajánlott a hello használni Web App és a MySQL adatbázis ugyanazon a helyen.

#### <a name="cleardb"></a>ClearDB 
[ClearDB](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview) egy külső megoldás egy teljesen integrált MySQL-szolgáltatás az Azure-on. Rendelés toouse ClearDB adatbázisokban, szüksége lesz egy hitelkártya tooyour tooassociate [Azure-fiók](http://account.windowsazure.com/subscriptions). Ha ClearDB adatbázist szolgáltatót választotta, a meglévő adatbázisok toochoose listájának megtekintéséhez, vagy kattintson a **hozzon létre új** gomb toocreate adatbázis.

![ClearDB létrehozása](./media/app-service-web-create-web-app-from-marketplace/mysqldbcreate.png)

#### <a name="azure-database-for-mysql-preview"></a>A MySQL (előzetes verzió) Azure-adatbázis
[Azure MySQL-adatbázis](https://azure.microsoft.com/en-us/services/mysql) egy felügyelt adatbázis szolgáltatást biztosít az alkalmazások fejlesztéséhez és a központi telepítés, amely lehetővé teszi a perc és a skála hello toostand MySQL adatbázis keresnie a megbízható hello felhő. A két szélsőértéket beleértve árképzési modellekkel, például magas rendelkezésre állású, biztonsági és -helyreállítás – a beépített, nem a kívánt összes hello képességek elérhetővé kapcsolódik további költség. Kattintson a **tarifacsomag** egy másik toochoose [tarifacsomag](https://azure.microsoft.com/pricing/details/mysql). toouse egy meglévő adatbázist, vagy meglévő MySQL-kiszolgáló, használjon egy meglévő erőforráscsoportot, mely hello kiszolgáló található. 

![Hello hello webalkalmazáshoz tartozó adatbázis-beállítások konfigurálása](./media/app-service-web-create-web-app-from-marketplace/wordpress-azure-database.PNG)

> [!NOTE]
>  A MySQL (előzetes verzió) és a webes alkalmazás Linux (előzetes verzió) Azure-adatbázis nem érhetők el minden régióban. toolearn kapcsolatos további [MySQL (előzetes verzió) az Azure-adatbázis](https://docs.microsoft.com/en-us/azure/mysql) és [Linux webalkalmazás](./app-service-linux-intro.md) korlátozások. 

#### <a name="mysql-in-app"></a>MySQL alkalmazásbeli
[MySQL alkalmazásbeli](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app) App Service, amely lehetővé teszi a MySql natív módon hello platformon futó szolgáltatása. hello alapfunkciókat hello szolgáltatás hello kiadása támogatja:

- MySQL hello futó ugyanaz a kiszolgálópéldányt és a kiszolgáló üzemeltetési hello webhelyet. Ez növekedhet az alkalmazás teljesítményét.
- Tároló megosztott MySQL, mind a webes alkalmazás fájljai között. Megjegyzés: a szabad és megosztott tervek határaiba a kvótakorlát Ha hello helykóddal alapján hello műveletek, hajtsa végre. Tekintse meg [sablonkérelem](https://azure.microsoft.com/en-us/pricing/details/app-service/plans/) szabad és megosztott tervekhez.
- Ha bekapcsolja a lassú lekérdezések naplózása és a MySQL általános naplózást. Ne feledje, hogy ez hatással lehet a hello helyteljesítmény nem mindig be kell kapcsolni. hello naplózási szolgáltatás segítségével minden alkalmazás problémákat vizsgálja. 

További részletekért tekintse meg a [cikk](https://blogs.msdn.microsoft.com/appserviceteam/2016/08/18/announcing-mysql-in-app-preview-for-web-apps/ )

![MySQL alkalmazásbeli kezelése](./media/app-service-web-create-web-app-from-marketplace/mysqlinappmanage.PNG)

Hello portállapon hello WordPress alkalmazás telepítése közben hello tetején hello harang ikonra kattintva figyelemmel követheti hello folyamatban van.    
![Folyamatjelző](./media/app-service-web-create-web-app-from-marketplace/deploy-success.png)

## <a name="manage-your-new-azure-web-app"></a>Az új Azure-webapp kezelése

Nyissa meg az Azure portál tootake hello webes alkalmazás imént létrehozott egy pillantást toohello.

toodo, jelentkezzen be a túl[https://portal.azure.com](https://portal.azure.com).

Hello bal oldali menüben kattintson **alkalmazásszolgáltatások**, majd kattintson az Azure-webalkalmazásban hello nevére.

![Portálnavigációjával tooAzure webalkalmazás](./media/app-service-web-create-web-app-from-marketplace/nodejs-docs-hello-world-app-service-list.png)


Ekkor a webapp _paneljére_ (vízszintesen megnyíló portáloldalára) jut.

Alapértelmezés szerint a webalkalmazás panelen látható hello **áttekintése** lap. Ezen az oldalon megtekintheti az alkalmazás állapotát. Itt elvégezhet olyan alapszintű felügyeleti feladatokat is, mint a böngészés, leállítás, elindítás, újraindítás és törlés. hello lapok hello hello panel bal oldalán látható hello különböző konfigurációs lapok megnyithatja.

![Az App Service panel az Azure Portalon](./media/app-service-web-create-web-app-from-marketplace/nodejs-docs-hello-world-app-service-detail.png)

A lapok hello panelen megjelenítése hello számos nagy szolgáltatás tooyour webes alkalmazás is hozzáadhat. a következő lista hello hello lehetőségeket néhány következő:

* Egyéni DNS-név leképezése
* Egyéni SSL-tanúsítvány kötése
* Folyamatos üzembe helyezés konfigurálása
* Vertikális felskálázás és kibővítés
* Felhasználói hitelesítés hozzáadása

Fejezze be a hello 5 perces WordPress telepítése varázsló toohave WordPress alkalmazás lépéseivel. Tekintse meg [Wordpress dokumentáció](https://codex.WordPress.org/) toodevelop a webes alkalmazást.

![WordPress telepítése varázsló](./media/app-service-web-create-web-app-from-marketplace/wplanguage.png)

## <a name="configuring-your-app"></a>Az alkalmazás konfigurálása 
Több lépésből áll részt vesz a WordPress alkalmazás kezelése, mielőtt üzemi használatra kész. Kövesse az alábbi lépéseket tooconfigure, és a WordPress alkalmazás kezelése:

| toodo ez... | Használandó karakterlánc |
| --- | --- |
| **Töltse fel, vagy nagy fájlok tárolásához** |[WordPress beépülő modul, a Blob storage használatával](https://wordpress.org/plugins/windows-azure-storage/)|
| **E-mailek küldése** |A vásárlás [SendGrid](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SendGrid.SendGrid?tab=Overview) e-mail-szolgáltatást, és használja a hello [WordPress beépülő modul használatával a SendGrid](https://wordpress.org/plugins/sendgrid-email-delivery-simplified/) tooconfigure azt|
| **Egyéni tartománynevek** |[Egyéni tartománynév konfigurálása az Azure App Service-ben](app-service-web-tutorial-custom-domain.md) |
| **HTTPS** |[Az Azure App Service webalkalmazás HTTPS engedélyezése](app-service-web-tutorial-custom-ssl.md) |
| **Éles üzem előtti érvényesítése** |[Az Azure App Service web Apps átmeneti és fejlesztői környezetek beállítása](web-sites-staged-publishing.md)|
| **Figyelés és hibaelhárítás** |[Az Azure App Service web Apps diagnosztikai naplózás engedélyezése](web-sites-enable-diagnostic-log.md) és [webes alkalmazások figyelése az Azure App Service-ben](app-service-web-tutorial-monitoring.md) |
| **A hely telepítése** |[Az Azure App Service webalkalmazás üzembe helyezése](app-service-deploy-local-git.md) |


## <a name="secure-your-app"></a>Az alkalmazás biztonságos 
Több lépésből áll részt vesz a WordPress alkalmazás kezelése, mielőtt üzemi használatra kész. Kövesse az alábbi lépéseket tooconfigure, és a WordPress alkalmazás kezelése:

| toodo ez... | Használandó karakterlánc |
| --- | --- |
| **Erős felhasználónév és jelszó**|  Jelszó módosítása gyakran. Tegye nem használja a leggyakrabban használt felhasználónevek, például *admin* vagy *wordpress* stb. WordPress felhasználók toouse egyedi felhasználónév és a erős jelszavak kikényszerítése. |
| **Ezek naprakész állapotát** | Tartsa meg a WordPress-core, a témák, a beépülő modulok toodate fel. Hello legújabb PHP futtatókörnyezetben az Azure App Service szolgáltatásban elérhető használata |
| **WordPress biztonsági kulcsok frissítése** | Frissítés [WordPress biztonsági kulcs](https://codex.wordpress.org/Editing_wp-config.php#Security_Keys) tooimprove titkosítási cookie-kban tárolt|

## <a name="improve-performance"></a>A teljesítmény javítása
Hello felhőben teljesítmény akkor érhető el, elsősorban a gyorsítótárazás és a kibővített keresztül. Azonban hello memória, sávszélesség és webalkalmazások üzemeltetéséhez egyéb attribútumai figyelembe kell venni.

| toodo ez... | Használandó karakterlánc |
| --- | --- |
| **App Service-példány képességekről** |[Díjszabásának részleteit, beleértve az alkalmazás szolgáltatási szinteket képességei](https://azure.microsoft.com/en-us/pricing/details/app-service/)|
| **Gyorsítótár-erőforrások** |Használjon [Azure Redis cache-](https://azure.microsoft.com/en-us/services/cache/), vagy az egyik más gyorsítótárazási ajánlatokat a hello hello [Azure tárolási](https://azuremarketplace.microsoft.com) |
| **Az alkalmazás skálázása** |Tooscale kell [hello webalkalmazást az Azure App Service](web-sites-scale.md) és/vagy a MySQL-adatbázis. MySQL alkalmazásbeli nem támogatja a kibővített, illetve emiatt ClearDB vagy az Azure Database MySQL (előzetes verzió). [A MySQL (előzetes verzió) Azure-adatbázis méretezése](https://azure.microsoft.com/en-us/pricing/details/mysql/) vagy használata [ClearDB magas rendelkezésre állás útválasztási](http://w2.cleardb.net/faqs/) tooscale másolatot az adatbázisról |

## <a name="availability-and-disaster-recovery"></a>Rendelkezésre állás és vészhelyreállítás
Magas rendelkezésre állású hello szempontja, hogy katasztrófa utáni helyreállítás toomaintain az üzletmenet folytonossága tartalmazza. Meghibásodások és vészhelyzetek hello felhőben tervezése szükséges toorecognize hello hibák gyorsan. Ezek a megoldások segítségével magas rendelkezésre állási stratégia megvalósításához.

| toodo ez... | Használandó karakterlánc |
| --- | --- |
| **Egyenleg helyek betöltése** vagy **földrajzi-helyek terjesztése** |[Irányíthatja a forgalmat az Azure Traffic Manager](https://azure.microsoft.com/en-us/services/traffic-manager/) |
| **Biztonsági mentés és visszaállítás** |[Készítsen biztonsági másolatot egy webalkalmazást az Azure App Service](web-sites-backup.md) és [visszaállítása egy webalkalmazást az Azure App Service-ben](web-sites-restore.md) |

## <a name="next-steps"></a>Következő lépések
További tudnivalók a különböző funkcióhoz [App Service toodevelop és a skála](/app-service-web/).
