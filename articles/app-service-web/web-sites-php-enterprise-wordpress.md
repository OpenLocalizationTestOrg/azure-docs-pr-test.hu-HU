---
title: "az Azure-on aaaEnterprise-osztály WordPress |} Microsoft Docs"
description: "Ismerje meg, hogyan toohost egy vállalati szintű WordPress webhely Azure App Service"
services: app-service\web
documentationcenter: 
author: sunbuild
manager: yochayk
editor: 
ms.assetid: 22d68588-2511-4600-8527-c518fede8978
ms.service: app-service-web
ms.devlang: php
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 10/24/2016
ms.author: sumuth
ms.openlocfilehash: 4347eddb31d622d1189dc5db4d81b0f3745d6e69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enterprise-class-wordpress-on-azure"></a>Vállalati szintű WordPress az Azure-on
Az Azure App Service egy méretezhető, biztonságos és könnyen használható környezetet biztosít az üzletmenet szempontjából kritikus fontosságú, nagy méretű [WordPress] [ wordpress] helyek. Maga a Microsoft hello például nagyvállalati szintű helyek fut [Office] [ officeblog] és [Bing] [ bingblog] blogok. Ez a cikk bemutatja, hogyan toouse hello tooestablish Microsoft Azure App Service Web Apps szolgáltatása, és egy vállalati szintű, a felhő alapú WordPress-webhely, amelyet kezelni tud a látogatók nagy mennyiségű karbantartása.

## <a name="architecture-and-planning"></a>Architektúra és tervezése
Egy alapszintű WordPress telepítése csak két követelményekkel rendelkezik:

* **A MySQL-adatbázis**: ezt a követelményt is elérhetővé [hello Azure piactér a ClearDB][cdbnstore]. Alternatív megoldásként kezelheti a saját MySQL telepítése Azure virtuális gépeken akár [Windows] [ mysqlwindows] vagy [Linux][mysqllinux].

  > [!NOTE]
  > ClearDB MySQL-konfigurációk több biztosít. Minden egyes konfigurációs más-más teljesítménybeli jellemzői. Lásd: hello [Azure Store] [ cdbnstore] ajánlatok által biztosított – hello Azure tárolják vagy közvetlenül hello látható információt [ClearDB webhely](http://www.cleardb.com/pricing.view).
  >
  >
* **A PHP 5.2.4 vagy nagyobb**: jelenleg biztosít az Azure App Service [PHP-verziók 5.4, 5.5 és 5.6][phpwebsite].

  > [!NOTE]
  > Azt javasoljuk, hogy Ön mindig futtatási hello legújabb verzióját a PHP, hogy hello legfrissebb biztonsági javításokat.
  >
  >

### <a name="basic-deployment"></a>Alapszintű üzembe helyezéséhez
Csak hello alapvető követelményeivel használatakor, létrehozhat egy egyszerű megoldást belül egy Azure-régió.

![Egy Azure-webalkalmazás és a MySQL-adatbázis tárolja egy Azure-régió][basic-diagram]

Bár ez lenne létrehozhatja webalkalmazások több példánya hello hely tooscale ki az alkalmazást, minden gazdája hello adatközpontok adott földrajzi régióban található. Ebben a régióban kívül a látogatók válaszidejű hello hely használatakor jelenhet meg. Ha ebben a régióban hello adatközpontok leáll, ezáltal az alkalmazás.

### <a name="multi-region-deployment"></a>Több területi központi telepítés
Azure használatával [Traffic Manager][trafficmanager], méretezhető WordPress-webhely felgyorsítására több földrajzi régióban elhelyezkedő között, és adja meg a látogatói hello URL-CÍMÉRE. A látogatói Traffic Manager, és ezután irányított tooa régió alapján hello terheléselosztás konfigurációs.

![Azure-webalkalmazás, üzemeltetett több régióba, régiók közötti CDBR magas rendelkezésre állású útválasztó tooroute tooMySQL használatával][multi-region-diagram]

Minden régióban hello WordPress-webhely továbbra is szeretné méretezhető webalkalmazások több példánya között, de a skálázás adott tooa régióban. Nagy forgalmú régiók és kis forgalmú régiókból is méretezhető másképp.

tooreplicate és útvonal forgalom toomultiple MySQL-adatbázisok esetén használhatja [ClearDB magas rendelkezésre állású útválasztók (CDBRs)] [ cleardbscale] (lásd a bal oldali) vagy [MySQL fürt szolgáltatónként osztályú Edition (CGE)] [cge].

### <a name="multi-region-deployment-with-media-storage-and-caching"></a>Több területi központi telepítése adathordozó tároló és a gyorsítótár
Hello hely feltöltések vagy állomások médiafájlok fogad el, ha használja az Azure Blob Storage tárolóban. Ha gyorsítótárazás van szüksége, fontolja meg a [Redis gyorsítótár][rediscache], [Memcache felhő](http://azure.microsoft.com/gallery/store/garantiadata/memcached/), [MemCachier](http://azure.microsoft.com/gallery/store/memcachier/memcachier/), vagy az egyik hello más hello a gyorsítótárazási ajánlatok [Azure tárolási](http://azure.microsoft.com/gallery/store/).

![Azure-webalkalmazás, több régióba üzemeltetett MySQL, a gyorsítótár kezelt, a Blob-tároló és a Content Delivery Network CDBR magas rendelkezésre állású útválasztó használata][performance-diagram]

A BLOB storage földrajzilag elosztott alapértelmezés szerint régiók közötti így nem kell tooworry kapcsolatos fájlok replikálása az összes helyen. Továbbá engedélyezheti hello Azure [Content Delivery Network] [ cdn] a Blob-tároló, amely osztja el a fájlok tooend csomópontot, amely szorosabb tooyour látogatóinak.

### <a name="planning"></a>Tervezés
#### <a name="additional-requirements"></a>További követelmények
| toodo ez... | Használandó karakterlánc |
| --- | --- |
| **Töltse fel, vagy nagy fájlok tárolásához** |[WordPress beépülő modul, a Blob storage használatával][storageplugin] |
| **E-mailek küldése** |[SendGrid] [ storesendgrid] és hello [WordPress beépülő modul használatával SendGrid][sendgridplugin] |
| **Egyéni tartománynevek** |[Egyéni tartománynév beállítása az Azure App Service-ben][customdomain] |
| **HTTPS** |[Az Azure App Service webalkalmazás HTTPS engedélyezése][httpscustomdomain] |
| **Éles üzem előtti érvényesítése** |[Átmeneti környezet az Azure App Service web Apps beállítása][staging] <p>A webes alkalmazás a tooproduction átmeneti áthelyezésekor is hello WordPress konfigurációs helyezi át. Ellenőrizze, hogy minden beállítás a termelési alkalmazás frissített toohello követelményeinek előkészített hello app tooproduction áthelyezése előtt.</p> |
| **Figyelés és hibaelhárítás** |[Az Azure App Service web Apps diagnosztikai naplózás engedélyezése] [ log] és [webes alkalmazások figyelése az Azure App Service-ben][monitor] |
| **A hely telepítése** |[Az Azure App Service webalkalmazás üzembe helyezése][deploy] |

#### <a name="availability-and-disaster-recovery"></a>Rendelkezésre állás és vészhelyreállítás
| toodo ez... | Használandó karakterlánc |
| --- | --- |
| **Egyenleg helyek betöltése** vagy **földrajzi-helyek terjesztése** |[Irányíthatja a forgalmat az Azure Traffic Manager][trafficmanager] |
| **Biztonsági mentés és visszaállítás** |[Készítsen biztonsági másolatot egy webalkalmazást az Azure App Service] [ backup] és [visszaállítása egy webalkalmazást az Azure App Service-ben][restore] |

#### <a name="performance"></a>Teljesítmény
Hello felhőben teljesítmény akkor érhető el, elsősorban a gyorsítótárazás és a kibővített keresztül. Azonban hello memória, sávszélesség és webalkalmazások üzemeltetéséhez egyéb attribútumai figyelembe kell venni.

| toodo ez... | Használandó karakterlánc |
| --- | --- |
| **App Service-példány képességekről** |[Díjszabásának részleteit, beleértve az alkalmazás szolgáltatási szinteket képességei][websitepricing] |
| **Gyorsítótár-erőforrások** |[Redis gyorsítótár][rediscache], [Memcache felhő](/gallery/store/garantiadata/memcached/), [MemCachier](/gallery/store/memcachier/memcachier/), vagy az egyik más gyorsítótárazási ajánlatokat a hello hello [Azure tároló](/gallery/store/) |
| **Az alkalmazás skálázása** |[A webalkalmazás skálázása az Azure App Service] [ websitescale] és [ClearDB magas rendelkezésre állás útválasztási][cleardbscale]. Ha toohost választja, és a saját MySQL telepítés felügyeletéhez, érdemes [MySQL-fürt CGE] [ cge] a kibővített. |

#### <a name="migration"></a>Migrálás
Nincsenek két módszer toomigrate egy meglévő WordPress-webhely tooAzure App Service:

* **[WordPress exportálása][export]**: Ez a módszer exportálja a blogban hello tartalmát. Importálhatja hello tartalom tooa új WordPress-webhely Azure App Service hello segítségével [importáló WordPress beépülő modul][import].

  > [!NOTE]
  > Ez a folyamat lehetővé teszi, hogy a tartalmat át, amíg azt nem telepíti át a beépülő modulok, témák vagy más testreszabás is szerepelt. Ezek az összetevők manuálisan újra kell telepíteni.
  >
  >
* **A manuális áttelepítéshez**: [a hely biztonsági mentése] [ wordpressbackup] és [adatbázis][wordpressdbbackup], majd manuálisan állítsa vissza tooa webalkalmazást az Azure-ban App Service és a kapcsolódó MySQL-adatbázis. Ez a módszer nagymértékben testre szabott hasznos toomigrate helyek, mivel ezzel elkerülheti hello tedium manuális telepítésére a beépülő modulok, témák és más testreszabás is szerepelt.

## <a name="step-by-step-instructions"></a>Lépésenkénti utasítások
### <a name="create-a-wordpress-site"></a>A WordPress-webhely létrehozása
1. Használjon hello [Azure piactér] [ cdbnstore] toocreate hello méretű hello meghatározott MySQL-adatbázis [architektúra és tervezési](#planning) hello régió vagy régiókban szakasz Ha a hely üzemelteti.
2. Hello kövesse [WordPress-webalkalmazás létrehozása az Azure App Service] [ createwordpress] toocreate egy WordPress webalkalmazást. Hello webalkalmazás létrehozásakor válassza ki a **meglévő MySQL-adatbázis használata**, majd válassza az 1. lépésben létrehozott hello adatbázishoz.

Ha telepít egy meglévő WordPress-webhely, lásd: [át egy meglévő WordPress-webhely tooAzure](#Migrate-an-existing-WordPress-site-to-Azure) új webalkalmazás létrehozása után.

### <a name="migrate-an-existing-wordpress-site-tooazure"></a>Egy meglévő WordPress-webhely tooAzure áttelepítése
A hello [architektúra és tervezési](#planning) területen egy WordPress-webhely két módon toomigrate vannak:

* **Exportálással és importálni** helyek sok testreszabása, amely nem rendelkezik, vagy csak kívánt toomove hello tartalmat.
* **Használjon biztonsági mentési és visszaállítási** kívánt toomove mindent testreszabási számos rendelkező helyek.

A webhely használja a következő szakaszok toomigrate hello egyikét.

#### <a name="hello-export-and-import-method"></a>hello exportálási és importálási mód
1. Használjon [WordPress exportálása] [ export] tooexport a meglévő helyet.
2. Webalkalmazás létrehozása a hello hello lépések segítségével [egy WordPress-webhely létrehozása](#Create-a-new-WordPress-site) szakasz.
3. Jelentkezzen be a hello tooyour WordPress-webhely [Azure-portálon][mgmtportal], és kattintson a **beépülő modulok** > **új hozzáadása**. Keresse meg és telepítse a hello **WordPress importáló** beépülő modul.
4. Hello importáló WordPress beépülő modul telepítése után kattintson **eszközök** > **importálási**, és kattintson a **WordPress** toouse hello importáló WordPress beépülő modul.
5. A hello **importálási WordPress** kattintson **Choose File**. Keresse meg a meglévő WordPress-webhely exportált hello WXR fájlt, és kattintson a **feltöltendő fájl és az importálás**.
6. Kattintson a **nyújt**. A rendszer kéri, hogy hello importálása sikeres volt-e.
7. Miután elvégezte ezeket a lépéseket, indítsa újra a helyet a **alkalmazásszolgáltatások** hello paneljén [Azure-portálon][mgmtportal].

Hello hely importálása után szükség lehet a következő lépéseket tooenable beállítások, amelyek nincsenek hello importfájl tooperform hello.

| Ha ez... | Ehhez... |
| --- | --- |
| **Idehivatkozások** |A hello WordPress irányítópult hello új hely, kattintson az **beállítások** > **idehivatkozások**, majd frissítse a hello idehivatkozások struktúra. |
| **kép/media hivatkozások** |tooupdate toohello új helyre hivatkozik, használja a hello [bársony kékek frissítése URL-címek beépülő modul][velvet], a Keresés és csere eszközt, vagy manuálisan frissítheti a hello hivatkozásokat az adatbázisban. |
| **Témák** |Nyissa meg túl**megjelenését** > **téma**, majd frissítse a hello Webhelytéma, igény szerint. |
| **Menük** |Ha a téma menük támogatja, hivatkozások tooyour kezdőlap még egyszer hello régi alkönyvtár embedded megjelölésével. Nyissa meg túl**megjelenését** > **menük**, és frissítse azokat. |

#### <a name="hello-backup-and-restore-method"></a>hello biztonsági mentés és visszaállítás metódus
1. Készítsen biztonsági másolatot a meglévő WordPress webhely hello információt használatával [WordPress biztonsági mentések][wordpressbackup].
2. Készítsen biztonsági másolatot a meglévő adatbázis hello információt használatával [az adatbázis biztonsági másolatának][wordpressdbbackup].
3. Hozzon létre egy adatbázist, és hello biztonsági másolat visszaállításával lehetséges.

   1. Vásároljon egy új adatbázist hello [Azure piactér][cdbnstore], vagy a MySQL-adatbázis beállítása a [Windows] [ mysqlwindows] vagy [Linux ] [ mysqllinux] virtuális gépet.
   2. Például a MySQL-ügyfél használata [MySQL munkaterület] [ workbench] tooconnect toohello új adatbázisából, és importálja a WordPress-adatbázist.
   3. Frissítés hello adatbázis toochange hello tartomány bejegyzések tooyour új Azure App Service-tartomány, például mywordpress.azurewebsites.net. Használjon hello [kereshet és cserélhet WordPress adatbázisok parancsfájl] [ searchandreplace] toosafely módosítása az összes példányát.
4. A webalkalmazás létrehozása az Azure-portálon hello, és tegye közzé a hello WordPress biztonsági mentés.

   1. a webes alkalmazás, amely rendelkezik egy adatbázis hello toocreate [Azure-portálon][mgmtportal], kattintson a **új** > **Web + mobil**  >  **Azure piactér** > **webalkalmazások** > **webes alkalmazás + SQL** (vagy **webes alkalmazás + MySQL**) > **Létrehozása**. Az összes szükséges hello beállítások toocreate egy üres webalkalmazás konfigurálása.
   2. A WordPress biztonsági mentése, keresse meg a hello **wp-config.php** fájlt, és nyissa meg valamelyik szerkesztőben. Cserélje le a következő tételek hello információkkal az új MySQL-adatbázis hello:

      * **DB_NAME**: hello adatbázis hello felhasználói nevét.
      * **DB_USER**: hello felhasználói használt név tooaccess hello adatbázis.
      * **DB_PASSWORD**: hello felhasználói jelszavát.

        Ezek a bejegyzések módosítása után mentse, és zárja be a hello **wp-config.php** fájlt.
   3. Használjon hello [webalkalmazás üzembe helyezése az Azure App Service] [ deploy] információk tooenable hello az üzembe helyezési módszer toouse szeretné, és telepítheti a WordPress biztonsági mentési tooyour webalkalmazás az Azure App Service-ben.
5. Hello WordPress-webhely telepítése után el tudja tooaccess hello új hely (a App Service-webalkalmazások) hello segítségével *. azurewebsite.net hello webhely URL-CÍMÉT.

### <a name="configure-your-site"></a>A hely konfigurálása
Hello WordPress-webhely létrehozása vagy áttelepítése után használja a következő információk tooimprove teljesítmény hello, vagy további funkciók engedélyezéséhez.

| toodo ez... | Használandó karakterlánc |
| --- | --- |
| **Az alkalmazásszolgáltatási csomag mód, mérete és engedélyezése skálázás beállítása** |[A webalkalmazás skálázása az Azure App Service][websitescale]. |
| **Állandó adatbázis-kapcsolatok engedélyezése** |WordPress alapértelmezés szerint nem használ állandó adatbázis-kapcsolatok előfordulhat, hogy a kapcsolat toohello adatbázis toobecome halmozódni után több kapcsolatot. tooenable állandó kapcsolat, telepítse a hello [állandó kapcsolat adapter beépülő modul](https://wordpress.org/plugins/persistent-database-connection-updater/installation/). |
| **A teljesítmény javítása** |<ul><li><p><a href="https://azure.microsoft.com/en-us/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/">Tiltsa le a hello ARR cookie-k</a>, ami javíthatja a teljesítményt WordPress webalkalmazások több példánya a futtatásakor.</p></li><li><p>Gyorsítótárazás engedélyezése. Használható <a href="http://msdn.microsoft.com/library/azure/dn690470.aspx">Redis gyorsítótár</a> (előzetes) rendelkező hello <a href="https://wordpress.org/plugins/redis-object-cache/">object cache WordPress beépülő modult Redis</a>, vagy használhat egy hello más gyorsítótárazási ajánlatokat a hello <a href="/gallery/store/">Azure Store</a>.</p></li><li><p>[Ellenőrizze a Wincache gyorsabb WordPress](https://wordpress.org/plugins/w3-total-cache/). A web Apps alapértelmezés szerint engedélyezve van a wincache-bővítmény. Ha WinCache és dinamikus gyorsítótár együtt, kapcsolja ki a WinCache a gyorsítótárban, de hagyja hello felhasználói és munkamenet-gyorsítótár engedélyezve. tooturn ki fájlgyorsítótár rendszerszintű .ini fájlban, állítsa be a következő érték hello:<br/><code>wincache.fcenabled = 0</code></p></li><li><p>[A webalkalmazás skálázása az Azure App Service] [ websitescale] és <a href="http://www.cleardb.com/developers/cdbr/introduction">ClearDB magas rendelkezésre állás útválasztási</a> vagy <a href="http://www.mysql.com/products/cluster/">MySQL-fürt CGE</a>.</p></li></ul> |
| **Használja a blobok tárolására** |<ol><li><p>[Az Azure storage-fiók létrehozása](../storage/common/storage-create-storage-account.md).</p></li><li><p>Ismerje meg, hogyan túl[használata hello a tartalom terjesztési hálózati](../cdn/cdn-create-new-endpoint.md) toogeo-blobok tárolt adatok terjesztése.</p></li><li><p>Telepítse és konfigurálja a hello <a href="https://wordpress.org/plugins/windows-azure-storage/">WordPress beépülő modul az Azure Storage</a>.</p><p>Részletes üzembe helyezési és konfigurációs adatait hello beépülő modult, tekintse meg a hello <a href="http://plugins.svn.wordpress.org/windows-azure-storage/trunk/UserGuide.docx">felhasználói útmutató</a>.</p> </li></ol> |
| **E-mailek engedélyezése** |Engedélyezése <a href="https://azure.microsoft.com/en-us/marketplace/partners/sendgrid/sendgrid-azure/">SendGrid</a> hello Azure Store használatával. Telepítse a hello <a href="http://wordpress.org/plugins/sendgrid-email-delivery-simplified">SendGrid beépülő modul</a> a WordPress alkalmazás. |
| **Egyéni tartománynév konfigurálása** |[Egyéni tartománynév beállítása az Azure App Service][customdomain]. |
| **Egy egyéni tartománynevet HTTPS engedélyezése** |[HTTPS engedélyezése az Azure App Service webalkalmazás][httpscustomdomain]. |
| **Egyenleg vagy földrajzi betöltése – a webhely terjesztése** |[Az Azure Traffic Manager forgalmat][trafficmanager]. Ha egyéni tartományt használ, tekintse meg [egyéni tartománynév beállítása az Azure App Service] [ customdomain] további információ a Traffic Manager toouse egyéni tartománynevekkel. |
| **Az automatikus biztonsági mentés engedélyezése** |[Készítsen biztonsági másolatot egy webalkalmazást az Azure App Service][backup]. |
| **Diagnosztikai naplózás engedélyezése** |[Az Azure App Service web Apps diagnosztikai naplózás engedélyezése][log]. |

## <a name="next-steps"></a>Következő lépések
* [WordPress optimalizálása](http://codex.wordpress.org/WordPress_Optimization)
* [Alakítsa át a WordPress toomultisite az Azure App Service-ben](web-sites-php-convert-wordpress-multisite.md)
* [ClearDB frissítése varázsló az Azure-bA](http://www.cleardb.com/store/azure/upgrade)
* [A webalkalmazás az Azure App Service almappájában üzemeltetési WordPress](http://blogs.msdn.com/b/webapps/archive/2013/02/13/hosting-wordpress-in-a-subfolder-of-your-windows-azure-web-site.aspx)
* [Részletes útmutató: Azure használatával egy WordPress-webhely létrehozása](http://blogs.technet.com/b/blainbar/archive/2013/08/07/article-create-a-wordpress-site-using-windows-azure-read-on.aspx)
* [A gazdagép a meglévő WordPress-bloghoz az Azure-on](http://blogs.msdn.com/b/msgulfcommunity/archive/2013/08/26/migrating-a-self-hosted-wordpress-blog-to-windows-azure.aspx)
* [A WordPress közérthető idehivatkozások engedélyezése](http://www.iis.net/learn/extensions/url-rewrite-module/enabling-pretty-permalinks-in-wordpress)
* [Hogyan toomigrate, és futtassa a WordPress bloghoz az Azure App Service](http://www.kefalidis.me/2012/06/how-to-migrate-and-run-your-wordpress-blog-on-windows-azure-websites/)
* [Hogyan toorun WordPress Azure App Service az ingyenes](http://architects.dzone.com/articles/how-run-wordpress-azure)
* [WordPress Azure kevesebb mint két perc alatt](http://www.sitepoint.com/wordpress-windows-azure-2-minutes-less/)
* [Helyezze át egy WordPress bloghoz tooAzure - 1. rész: egy WordPress-bloghoz létrehozása az Azure-on](http://www.davebost.com/2013/07/10/moving-a-wordpress-blog-to-windows-azure-part-1)
* [Helyezze át egy WordPress bloghoz tooAzure - 2. rész: a tartalom átvitele](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-transferring-your-content)
* [Helyezze át egy WordPress bloghoz tooAzure - 3. rész: az egyéni tartomány beállítása](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-3-setting-up-your-custom-domain)
* [A WordPress bloghoz tooAzure - rész 4 áthelyezése: közérthető idehivatkozások és URL-újraíró szabályok](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-4-pretty-permalinks-and-url-rewrite-rules)
* [A WordPress bloghoz tooAzure - rész 5 áthelyezése: almappa toohello legfelső szintű áthelyezése](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-5-moving-from-a-subfolder-to-the-root)
* [Hogyan tooset fel a WordPress webalkalmazás az Azure-fiókjába](http://www.itexperience.net/2014/01/20/how-to-set-up-a-wordpress-website-in-your-windows-azure-account/)
* [WordPress Azure mentése propping](http://www.johnpapa.net/wordpress-on-azure/)
* [WordPress tippek az Azure-on](http://www.johnpapa.net/azurecleardbmysql/)

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépések tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Nincs bankkártyára szükség, és nincsenek nem jár kötelezettségekkel.
>
>

## <a name="whats-changed"></a>A változások
A webhelyek tooApp szolgáltatás útmutató toohello módosítást, lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714).

<!-- URL List -->

[performance-diagram]: ./media/web-sites-php-enterprise-wordpress/performance-diagram.png
[basic-diagram]: ./media/web-sites-php-enterprise-wordpress/basic-diagram.png
[multi-region-diagram]: ./media/web-sites-php-enterprise-wordpress/multi-region-diagram.png
[wordpress]: http://www.microsoft.com/web/wordpress
[officeblog]: http://blogs.office.com/
[bingblog]: http://blogs.bing.com/
[cdbnstore]: http://www.cleardb.com/store/azure
[storageplugin]: https://wordpress.org/plugins/windows-azure-storage/
[sendgridplugin]: http://wordpress.org/plugins/sendgrid-email-delivery-simplified/
[phpwebsite]: web-sites-php-configure.md
[customdomain]: app-service-web-tutorial-custom-domain.md
[trafficmanager]: ../traffic-manager/traffic-manager-overview.md
[backup]: web-sites-backup.md
[restore]: web-sites-restore.md
[rediscache]: https://azure.microsoft.com/documentation/services/redis-cache/
[managedcache]: http://msdn.microsoft.com/library/azure/dn386122.aspx
[websitescale]: web-sites-scale.md
[managedcachescale]: http://msdn.microsoft.com/library/azure/dn386113.aspx
[cleardbscale]: http://www.cleardb.com/developers/cdbr/introduction
[staging]: web-sites-staged-publishing.md
[monitor]: web-sites-monitor.md
[log]: web-sites-enable-diagnostic-log.md
[httpscustomdomain]: app-service-web-tutorial-custom-ssl.md
[mysqlwindows]:../virtual-machines/windows/classic/mysql-2008r2.md
[mysqllinux]:../virtual-machines/linux/classic/mysql-on-opensuse.md
[cge]: http://www.mysql.com/products/cluster/
[websitepricing]: /pricing/details/app-service/
[export]: http://en.support.wordpress.com/export/
[import]: http://wordpress.org/plugins/wordpress-importer/
[wordpressbackup]: http://wordpress.org/plugins/wordpress-importer/
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[createwordpress]: web-sites-php-web-site-gallery.md
[velvet]: https://wordpress.org/plugins/velvet-blues-update-urls/
[mgmtportal]: https://portal.azure.com/
[wordpressbackup]: http://codex.wordpress.org/WordPress_Backups
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[workbench]: http://www.mysql.com/products/workbench/
[searchandreplace]: http://interconnectit.com/124/search-and-replace-for-wordpress-databases/
[deploy]: web-sites-deploy.md
[posh]: /powershell/azureps-cmdlets-docs
[Azure CLI]:../cli-install-nodejs.md
[storesendgrid]: https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/
[cdn]: ../cdn/cdn-overview.md
