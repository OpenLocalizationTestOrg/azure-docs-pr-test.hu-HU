---
title: a Mobile Services tooan App Service Mobile Apps aaaMigrate
description: "Ismerje meg, hogyan tooeasily át a Mobile Services alkalmazás tooan App Service Mobile Apps"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 07507ea2-690f-4f79-8776-3375e2adeb9e
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile
ms.devlang: na
ms.topic: article
ms.date: 10/03/2016
ms.author: glenga
ms.openlocfilehash: cd2e8d98595703389300b79da9bf51cdcefe7b40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="article-top"></a>Telepítse át a meglévő Azure Mobile Services mobilszolgáltatás tooAzure App Service
A hello [Azure App Service általános rendelkezésre állását], Azure Mobile Services helyek lehetnek könnyen áttelepített helyi tootake előnyeit hello Azure App Service összes funkcióját.  Ez a dokumentum ismerteti a milyen tooexpect, a webhely az App Service Azure Mobile Services tooAzure áttelepítésekor.

## <a name="what-does-migration-do"></a>Mire áttelepítési? tooyour hely
Az Azure Mobile szolgáltatás kapcsolja be a Mobile Service egy [Azure App Service] app hello kód módosítása nélkül.  A Notification Hubs, SQL adatkapcsolat, hitelesítési beállításai, ütemezett feladatokat, és tartománynév változatlanok maradnak.  Az Azure Mobile szolgáltatással mobil ügyfelek általában a toooperate továbbra is.  Áttelepítés a szolgáltatás újraindul, miután átvitt tooAzure App Service.

[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

## <a name="why-migrate"></a>Miért át kell telepítenie a webhely
Microsoft van javasolja, hogy az áttelepített hello funkciók az Azure App Service, beleértve az Azure Mobile Services mobilszolgáltatás tootake mértékben:

* Új üzemeltetési funkciók, például [WebJobs] és [egyéni tartománynevek].
* Kapcsolat tooyour a helyszíni erőforrásokhoz [VNet] továbbá túl[hibrid kapcsolatok].
* Figyelés és hibaelhárítás az új New Relic vagy [Application Insights].
* Beépített DevOps tooling, beleértve a [átmeneti üzembe helyezési ponti]visszaállítási és az éles tesztelése.
* [Automatikus méretezése], terheléselosztás, és [teljesítményfigyelés].

Az Azure App Service hello előnyeit a további információkért lásd: hello [Mobile Services vs. App Service] témakör.

## <a name="before-you-begin"></a>Előkészületek
A webhely fő munka megkezdése előtt készítsen biztonsági másolatot a Mobile Service parancsfájlok és az SQL-adatbázis.

## <a name="migrating-site"></a>A helyek áttelepítése
hello áttelepítési folyamat során minden hely egyetlen Azure-régión belül áttelepíti.

toomigrate webhelyét:

1. Jelentkezzen be toohello [klasszikus Azure portál].
2. Válassza ki a Mobile Service hello régióban toomigrate kívánja.
3. Kattintson a hello **tooApp szolgáltatás áttelepítése** gombra.

   ![hello áttelepítése gomb][0]
4. Hello áttelepítése tooApp Service párbeszédpanelen olvasható.
5. Adja meg a Mobile Service hello nevét található hello mezőbe.  Ha a tartomány neve contoso.azure-mobile.net, majd írja be például *contoso* található hello mezőbe.
6. Hello osztásjelek gombra.

Hello áttelepítési hello tevékenység figyelőben hello állapotának figyelése. A webhely jelenik meg, *áttelepítése* a klasszikus Azure portál hello.

  ![Áttelepítési tevékenység figyelése][1]

A mobilszolgáltatáshoz az áttelepítés alatt álló 3 too15 percig minden áttelepítési is igénybe vehet.  A webhely hello áttelepítés közben elérhető marad.
A webhely újraindítása hello hello áttelepítési folyamat végén.  hello újraindítási folyamatot, amely lehet akár egy néhány másodperc alatt hello helyhez nem érhető el.

## <a name="finalizing-migration"></a>Áttelepítési hello véglegesítése
Tervezze meg a webhely egy mobil ügyfél hello megkötése hello áttelepítési folyamat a tootest.  Gondoskodjon arról, hogy minden közös Ügyfélműveletek módosítások toohello mobil ügyfélalkalmazás nélkül végezheti el.  

### <a name="update-app-service-tier"></a>Válassza ki a megfelelő IP-címek alkalmazásszolgáltatás
Miután áttelepítette a tooAzure App Service díjszabás több beleszólása van.

1. Jelentkezzen be toohello [Azure-portálon].
2. Válassza ki **összes erőforrás** vagy **alkalmazásszolgáltatások** kattintson az áttelepített Mobile Service hello nevére.
3. Alapértelmezés szerint hello-beállítások panel nyílik meg.
4. Kattintson a **App Service-csomag** hello-beállítások menüjében.
5. Kattintson a hello **Tarifacsomagot** csempére.
6. Hello csempe megfelelő tooyour követelményeinek, majd **válasszon**.  Szükség lehet a tooClick **összes** toosee hello rendelkezésre tarifacsomag szükséges.

Kiindulási pontként a következő rétegek hello javasoljuk:

| Mobilszolgáltatás Tarifacsomagot | Az App Service Tarifacsomagot |
|:--- |:--- |
| Ingyenes |F1 – Ingyenes |
| Basic |A K1 Basic |
| Standard |S1 – Standard |

Nincs rugalmasan hello jobb tarifacsomag az alkalmazás kiválasztása.  Tekintse meg a túl[App Service szolgáltatás díjszabása] hello díjszabása az új App Service teljes leírását.

> [!TIP]
> hello App Service Standard csomag tartalmaz hozzáférési toomany funkciókat, amelyeknél felmerülhet toouse, beleértve a [átmeneti üzembe helyezési ponti], automatikus biztonsági mentésekhez, és az automatikus skálázást.  Tekintse meg a hello új képességeit, amíg nincs!
>
>

### <a name="review-migration-scheduler-jobs"></a>Tekintse át a hello át a Feladatütemező szolgáltatás
A Feladatütemező szolgáltatás körülbelül 30 percet az áttelepítés után nem látható lesz.  Ütemezett feladatok toorun hello háttérben továbbra is.
tooview az ütemezett feladatok után azokat újra be nem látható:

1. Jelentkezzen be toohello [Azure-portálon].
2. Válassza ki **Tallózás >**, adja meg **ütemezés** a hello *szűrő* mezőbe, majd válassza ki **Feladatütemező gyűjtemények**.

Ingyenes scheduler feladatok érhető el az áttelepítést követő korlátozott számú van.  Tekintse át a használati és hello [Azure Scheduler tervek].

### <a name="configure-cors"></a>A CORS konfigurálása, ha szükséges
Eltérő eredetű erőforrások megosztása a technika tooallow egy webhely tooaccess egy webes API-t egy másik tartományban.  Ha Azure Mobile Services webhelyekhez használ, akkor szüksége tooconfigure CORS hello áttelepítés részeként.  Azure Mobile Services kizárólag a mobileszközök érnek el, ha majd CORS nem ritka esetekben kivéve konfigurált toobe kell.

Az áttelepített CORS-beállítások állnak rendelkezésre, hello **MS_CrossDomainWhitelist** Alkalmazásbeállítás.  toomigrate a hely toohello App Service CORS létesítmény:

1. Jelentkezzen be toohello [Azure-portálon].
2. Válassza ki **összes erőforrás** vagy **alkalmazásszolgáltatások** kattintson az áttelepített Mobile Service hello nevére.
3. Alapértelmezés szerint hello-beállítások panel nyílik meg.
4. Kattintson a **CORS** hello API menüben.
5. Adjon meg egy engedélyezett eredetet hello mezőben megadott, minden egyes után nyomja le az ENTER billentyűt.
6. Ha az engedélyezett Eredeteket listája helyes-e, kattintson a hello Mentés gombra.

> [!TIP]
> Az Azure App Service használatának előnyei hello egyik, hogy futtathatja a mobilszolgáltatás és a webhely hello ugyanazon a helyen.  További információkért lásd: hello [lépések](#next-steps) szakasz.
>
>

### <a name="download-publish-profile"></a>Egy új közzétételi profil letöltése
hello közzétételi profilt, amely a webhely az App Service tooAzure áttelepítésekor módosul.  Ha azt tervezi, toopublish Visual Studio a helyet, új közzétételi profil kell.  toodownload hello új közzétételi profil:

1. Jelentkezzen be toohello [Azure-portálon].
2. Válassza ki **összes erőforrás** vagy **alkalmazásszolgáltatások** kattintson az áttelepített Mobile Service hello nevére.
3. Kattintson a **Get közzétételi profil**.

hello PublishSettings-fájl az letöltött tooyour számítógép.  Általában nevezik *sitename*. PublishSettings.  Importálás hello közzétételi beállítások a meglévő projektben:

1. Nyissa meg a Visual Studio és az Azure Mobile Services mobilszolgáltatás-projektet.
2. Kattintson a jobb gombbal a projektre a hello **Megoldáskezelőben** válassza **közzététel...**
3. Kattintson a **importálása**
4. Kattintson a **Tallózás** válassza ki a letöltött közzététele beállításfájl.  Kattintson az **OK** gombra
5. Kattintson a **kapcsolat ellenőrzése** tooensure hello közzététele a munkahelyi beállításokat.
6. Kattintson a **közzététel** toopublish a helyen.

## <a name="working-with-your-site"></a>Az áttelepítés utáni webhely használata
Az új App Service hello a munka megkezdéséhez [Azure-portálon] áttelepítés utáni.  hello az alábbiakban néhány Megjegyzés a konkrét műveletek tooperform szerepel hello [klasszikus Azure portál]App Service-megfelelőjükkel együtt,.

### <a name="publishing-your-site"></a>Töltsön le és az áttelepített hely közzététele
A webhely git- vagy ftp segítségével érhető el, és képes különböző különböző mechanizmusokkal, beleértve a WebDeploy, TFS, Mercurial, GitHub vagy FTP újból közzé.  a webhely többi hello hello üzembe helyezési hitelesítő adatok áttelepítése.  Ha nem állított az üzembe helyezési hitelesítő adatok, vagy nem tárolja őket, visszaállíthatja őket:

1. Jelentkezzen be toohello [Azure-portálon].
2. Válassza ki **összes erőforrás** vagy **alkalmazásszolgáltatások** kattintson az áttelepített Mobile Service hello nevére.
3. Alapértelmezés szerint hello-beállítások panel nyílik meg.
4. Kattintson a **üzembe helyezési hitelesítő adatok** a közzététel menü hello.
5. Adja meg a hello új üzembe helyezési hitelesítő adatok a hello mezőkbe, majd kattintson a hello Mentés gombra.

Ezen hitelesítő adatok tooclone hello hely használata a git, vagy állítsa be automatikus telepítések a Githubból, TFS vagy Mercurial.  További információkért lásd: hello [Azure App Service üzembe helyezési dokumentációja].

### <a name="appsettings"></a>Alkalmazásbeállítások
A legtöbb beállítások áttelepített mobilszolgáltatás Alkalmazásbeállítások keresztül érhetők el.  Hello Alkalmazásbeállítások listájának lekérése hello [Azure-portálon].
tooview vagy az alkalmazás beállításainak módosítása:

1. Jelentkezzen be toohello [Azure-portálon].
2. Válassza ki **összes erőforrás** vagy **alkalmazásszolgáltatások** kattintson az áttelepített Mobile Service hello nevére.
3. Alapértelmezés szerint hello-beállítások panel nyílik meg.
4. Kattintson a **Alkalmazásbeállítások** hello általános menüben.
5. Görgessen toohello App beállítások szakaszban, és keresse meg az Alkalmazásbeállítás.
6. Kattintson a hello app tooedit hello beállításérték hello értékét.  Kattintson a **mentése** toosave hello érték.

Több alkalmazás beállításai hello frissítheti ugyanannyi időt vesz igénybe.

> [!TIP]
> Hello a két alkalmazás-beállítások ugyanazt az értéket.  Például előfordulhat, hogy látja *ApplicationKey* és *MS\_ApplicationKey*.  Mindkét alkalmazás beállításai hello frissítése ugyanannyi időt vesz igénybe.
>
>

### <a name="authentication"></a>Hitelesítés
Minden hitelesítési beállítások állnak rendelkezésre, a áttelepített hely Alkalmazásbeállítások.  tooupdate a hitelesítési beállításokat kell módosítania a megfelelő alkalmazás beállításai.  hello következő táblázatban a hitelesítésszolgáltató hello megfelelő alkalmazás beállításait:

| Szolgáltató | Ügyfél-azonosító | Ügyfélkulcs | Egyéb beállítások |
|:--- |:--- |:--- |:--- |
| Microsoft-fiók |**MS\_MicrosoftClientID** |**MS\_MicrosoftClientSecret** |**MS\_MicrosoftPackageSID** |
| Facebook |**MS\_FacebookAppID** |**MS\_FacebookAppSecret** | |
| Twitter |**MS\_TwitterConsumerKey** |**MS\_TwitterConsumerSecret** | |
| Google |**MS\_GoogleClientID** |**MS\_GoogleClientSecret** | |
| Azure AD |**MS\_AadClientID** | |**MS\_AadTenants** |

Megjegyzés: **MS\_AadTenants** bérlői tartományok ("Engedélyezett bérlők" mezők hello hello Mobile Services portálon) vesszővel tagolt listáját tárolja.

> [!WARNING]
> **Ne használjon hello hitelesítési mechanizmusok hello beállítási menüben**
>
> Az Azure App Service biztosít egy külön "no-kód" hitelesítési és engedélyezési rendszer hello *hitelesítési / engedélyezési* beállítások menü és hello (elavult) *Mobile hitelesítési* a beállítás hello-beállítások menüjében.  Ezek a beállítások nem kompatibilisek egy áttelepített Azure Mobile Service.  Is [a hely frissítése](app-service-mobile-net-upgrading-from-mobile-services.md) hello Azure App Service hitelesítés tootake előnyeit.
>
>

### <a name="easytables"></a>Adatok
Hello *adatok* a Mobile Servicesben lap váltotta *könnyen táblák* hello Azure-portálon belül.  tooaccess könnyen táblák:

1. Jelentkezzen be toohello [Azure-portálon].
2. Válassza ki **összes erőforrás** vagy **alkalmazásszolgáltatások** kattintson az áttelepített Mobile Service hello nevére.
3. Alapértelmezés szerint hello-beállítások panel nyílik meg.
4. Kattintson a **könnyen táblák** hello mobil menüben.

Egy tábla hello kattintva vehet fel **Hozzáadás** gombra kattint, vagy nem érhető el a létező táblák gombra kattintva egy tábla nevét.  Ezen a panelen elvégezhető különböző műveletek többek között:

* Tábla engedélyek módosítása
* Hello működési parancsfájlok szerkesztése
* Hello táblaséma kezelése
* Hello tábla törlése
* Hello táblázat tartalmának törlése
* Hello tábla sorait törlése

### <a name="easyapis"></a>API
Hello *API* a Mobile Servicesben lap váltotta *egyszerű API-k* hello Azure-portálon belül.  Egyszerű API-k tooaccess:

1. Jelentkezzen be toohello [Azure-portálon].
2. Válassza ki **összes erőforrás** vagy **alkalmazásszolgáltatások** kattintson az áttelepített Mobile Service hello nevére.
3. Alapértelmezés szerint hello-beállítások panel nyílik meg.
4. Kattintson a **egyszerű API-k** hello mobil menüben.

Az áttelepített API-kat már szereplő hello panelen.  Azt is megteheti az API-k ezen a panelen.  egy adott API toomanage hello API kattintson.
Hello új panelen hello engedélyek igazítása, és hello parancsfájl-hello API szerkesztése.

### <a name="on-demand-jobs"></a>Feladatütemező feladatai
A Feladatütemező összes feladat hello Scheduler Feladatgyűjteményei szakasz keresztül érhetők el.  tooaccess a Feladatütemező feladatok:

1. Jelentkezzen be toohello [Azure-portálon].
2. Válassza ki **Tallózás >**, adja meg **ütemezés** a hello *szűrő* mezőbe, majd válassza ki **Feladatütemező gyűjtemények**.
3. Válassza ki a webhely Feladatgyűjtemény hello.  A neve *sitename*-feladatokat.
4. Kattintson a **beállítások**.
5. Kattintson a **ütemezőjének** felügyelete alatt.

Ütemezett feladatok szerepel a listában megadott áttelepítés előtt hello gyakoriságát.  Igény szerinti feladatok le vannak tiltva.  egy igény szerinti feladat toorun:

1. Jelölje ki a kívánt toorun hello feladatot.
2. Szükség esetén kattintson **engedélyezése** tooenable hello feladat.
3. Kattintson a **beállítások**, majd **ütemezés**.
4. Válassza ki a ismétlődése **egyszer**, majd kattintson a **mentése**

Az igény szerinti feladatok találhatók `App_Data/config/scripts/scheduler post-migration`.  Javasoljuk, hogy az összes igény szerinti feladatok túl átalakítása[WebJobs] vagy [funkciók].  Új Feladatütemező feladatok írási [WebJobs] vagy [funkciók].

### <a name="notification-hubs"></a>A Notification Hubs
A Mobile Services Notification Hubs leküldéses értesítések használ.  a következő beállítások hello használt toolink hello értesítési központ tooyour Mobile Service az áttelepítés után:

| Alkalmazás-beállítás | Leírás |
|:--- |:--- |
| **MS\_PushEntityNamespace** |Értesítési központ Namespace hello |
| **MS\_NotificationHubName** |hello az értesítési központ neve |
| **MS\_NotificationHubConnectionString** |Értesítési központ kapcsolati karakterlánc hello |
| **MS\_NamespaceName** |Egy aliast MS_PushEntityNamespace |

Az értesítési központ kezeli hello [Azure-portálon].  Jegyezze fel a hello értesítési központ nevét (található ez hello App beállításokkal):

1. Jelentkezzen be toohello [Azure-portálon].
2. Válassza ki **Tallózás**> elemre, majd válassza **értesítési központok**
3. Kattintson a hello mobilszolgáltatás társított hello értesítési központ nevét.

> [!NOTE]
> Ha az értesítési központ egy "Mixed" típust, nincs látható.  "Mixed" írja be a notification hubs Notification Hubs és a korábbi Service Bus-funkciók használatára.  [Alakítsa át a vegyes névterek] folytatása előtt.  Hello átalakítás befejeződése után hello megjelenik az értesítési központ [Azure-portálon].
>
>

További információkért tekintse át a hello [Notification Hubs] dokumentációját.

> [!TIP]
> Notification Hubs-felügyeleti funkciókat hello [Azure-portálon] továbbra is a képen vannak.  Hello [klasszikus Azure portál] továbbra is elérhető a Notification Hubs kezeléséhez.
>
>

### <a name="legacy-push"></a>A hagyományos leküldéses beállításai
Ha a mobilszolgáltatáshoz előtt hello bemutatása a Notification Hubs leküldéses konfigurált, az *örökölt leküldéses*.  Ha leküldéses használ, és nem jelenik meg egy értesítési központot, szerepel a konfigurációs, akkor valószínűleg használ *örökölt leküldéses*.  Ez a szolgáltatás az összes többi szolgáltatás áttelepítése.  Javasoljuk azonban, hogy frissítse tooNotification hubok, amint hello áttelepítés befejezése után.

A ideiglenes hello minden hello örökölt leküldéses beállítás (kivétellel hello figyelmet a jelentősebb hello APNS-tanúsítvány) érhetők el az alkalmazás beállításai.  Frissítés hello APNS-tanúsítványt azáltal, hogy a megfelelő fájlt hello hello fájlrendszer.

### <a name="app-settings"></a>Egyéb beállítások
További beállítások a következő hello: áttelepített Mobile szolgáltatás és a rendelkezésre álló *beállítások* > *Alkalmazásbeállítások*:

| Alkalmazás-beállítás | Leírás |
|:--- |:--- |
| **MS\_MobileServiceName** |az alkalmazás hello neve |
| **MS\_MobileServiceDomainSuffix** |hello tartománynév előtagja. Egytényezős Azure-mobile.net |
| **MS\_ApplicationKey** |Az alkalmazás kulcs |
| **MS\_főkulcsos** |Az alkalmazás főkulcs |

hello alkalmazáskulcsot és a főkulcs is azonos toohello alkalmazás kulcsok eredeti Mobile szolgáltatás.  Különösen hello Alkalmazáskulcsot küldi el mobil ügyfelek toovalidate hello mobil API használatát.

### <a name="cliequivalents"></a>Parancssori megfelelője
Már használhatja a hello *mobil azure* toomanage parancsot az Azure Mobile Services-webhelyre.  Számos funkciót hello helyett inkább *azure hely* parancsot.  A következő tábla toofind megfelelője általános jellegű parancsok hello használata:

| *Az Azure Mobile* parancs | Egyenértékű *Azure Site* parancs |
|:--- |:--- |
| mobil helyek |webhelylistájának helye |
| mobil listája |webhely-lista |
| mobil megjelenítése *neve* |hely megjelenítése *neve* |
| mobil újraindítás *neve* |Újraindítás hely *neve* |
| mobil helyezze üzembe újra *neve* |hely telepítési helyezze üzembe újra *commitId* *neve* |
| mobil kulcskészlet *neve* *típus* *érték* |hely appsetting törlése *kulcs* *neve* <br/> hely appsetting hozzáadása *kulcs*=*érték* *neve* |
| Mobile config lista *neve* |appsetting webhelylista *neve* |
| Mobile config beolvasása *neve* *kulcs* |hely appsetting megjelenítése *kulcs* *neve* |
| mobil konfiguráció *neve* *kulcs* |hely appsetting törlése *kulcs* *neve* <br/> hely appsetting hozzáadása *kulcs*=*érték* *neve* |
| mobil tartománylista *neve* |tartomány webhelylista *neve* |
| mobil tartomány hozzáadása *neve* *tartomány* |hely tartomány hozzáadása *tartomány* *neve* |
| mobil tartomány törlése *neve* |webhely tartomány törlése *tartomány* *neve* |
| mobil méretezési megjelenítése *neve* |hely megjelenítése *neve* |
| mobil méretezési módosítása *neve* |skála webhelymódot *mód* *neve* <br /> hely méretezési példányok *példányok* *neve* |
| mobil appsetting lista *neve* |appsetting webhelylista *neve* |
| mobil appsetting hozzáadása *neve* *kulcs* *érték* |hely appsetting hozzáadása *kulcs*=*érték* *neve* |
| mobil appsetting törlése *neve* *kulcs* |hely appsetting törlése *kulcs* *neve* |
| mobil appsetting megjelenítése *neve* *kulcs* |hely appsetting törlése *kulcs* *neve* |

Frissítési beállítások frissítésével hello megfelelő alkalmazás-beállítás hitelesítés vagy a leküldéses értesítés.
Szerkesztheti a fájlokat, és tegye közzé a webhely, ftp vagy git.

### <a name="diagnostics"></a>Diagnosztika és naplózás
Diagnosztikai naplózás általában le van tiltva, az Azure App Service-ben.  diagnosztikai naplózás tooenable:

1. Jelentkezzen be toohello [Azure-portálon].
2. Válassza ki **összes erőforrás** vagy **alkalmazásszolgáltatások** kattintson az áttelepített Mobile Service hello nevére.
3. Alapértelmezés szerint hello-beállítások panel nyílik meg.
4. Válassza ki **diagnosztikai naplók** hello szolgáltatások menüjében.
5. Kattintson a **ON** a naplók a következő hello: **Alkalmazásnaplózást (fájlrendszer)**, **a részletes hibaüzeneteket**, és **sikertelen kérelmek nyomkövetése**
6. Kattintson a **fájlrendszer** Web server naplózás
7. Kattintson a **mentése**

tooview hello naplói:

1. Jelentkezzen be toohello [Azure-portálon].
2. Válassza ki **összes erőforrás** vagy **alkalmazásszolgáltatások** kattintson az áttelepített Mobile Service hello nevére.
3. Kattintson a hello **eszközök** gomb
4. Válassza ki **naplófolyamot** hello OBSERVE menüjében.

Naplók hello ablakban jelennek meg, akkor jönnek létre.  Az üzembe helyezési hitelesítő adatok használatával későbbi elemzési naplókat hello is letöltheti. További információkért lásd: hello [naplózás] dokumentációját.

## <a name="known-issues"></a>Ismert problémák
### <a name="deleting-a-migrated-mobile-app-clone-causes-a-site-outage"></a>Egy hely leállás Mobile App át másolat törlése okoz.
Az Azure PowerShell, majd törölje hello Klónozás használatával áttelepített mobilszolgáltatás a klón hello DNS-bejegyzést a termelési service távolítja el.  A webhely már nem érhető el a hello Internet.  

Megoldás: Ha a tooclone a helyen, ehhez hello portálon keresztül.

### <a name="changing-webconfig-does-not-work"></a>Web.config módosítása nem működik
Ha egy ASP.NET-webhely, megváltozik a toohello `Web.config` fájl nem érvényben.  hello Azure App Service összeállít egy megfelelő `Web.config` fájl indítási toosupport hello Mobile Services futásidőben.  Egyes beállítások (például egyéni fejlécek) felülbírálhatja egy XML-transzformációs fájl használatával.  Hozzon létre egy fájlt a nevű `applicationHost.xdt` -ezt a fájlt kell végződnie hello `D:\home\site` hello Azure szolgáltatás könyvtárába.  Töltse fel a `applicationHost.xdt` fájlt egy egyéni telepítési parancsfájl használatával, vagy közvetlenül a Kudu használatával.  hello következő egy példa a dokumentum bemutatja:

```
<?xml version="1.0" encoding="utf-8"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <httpProtocol>
      <customHeaders>
        <add name="X-Frame-Options" value="DENY" xdt:Transform="Replace" />
        <remove name="X-Powered-By" xdt:Transform="Insert" />
      </customHeaders>
    </httpProtocol>
    <security>
      <requestFiltering removeServerHeader="true" xdt:Transform="SetAttributes(removeServerHeader)" />
    </security>
  </system.webServer>
</configuration>
```

További információkért lásd: hello [XDT átalakítási minták] dokumentációnkat a Githubon.

### <a name="migrated-mobile-services-cannot-be-added-tootraffic-manager"></a>Áttelepített Mobile Services nem vehető fel kezelő tooTraffic
Traffic Manager-profil létrehozásakor egy áttelepített mobilszolgáltatás toohello profil közvetlenül nem választható.  Használja a "külső végpont."  A külső végpont nem vehető Powershellen keresztül.  További információkért lásd: a [Traffic Manager-oktatóanyag](https://azure.microsoft.com/blog/azure-traffic-manager-external-endpoints-and-weighted-round-robin-via-powershell/).

## <a name="next-steps"></a>Következő lépések
Most, hogy az alkalmazás áttelepített tooApp szolgáltatás, nincsenek még több szolgáltatást is használhatja:

* Központi telepítési [átmeneti üzembe helyezési ponti] toostage módosítások tooyour hely lehetővé teszi, és végezze el A / B tesztelés.
* [WebJobs] pótlásáról igény ütemezett feladatokhoz.
* Is [folyamatosan telepítése] létrehozhatja, ha a hely tooGitHub, TFS vagy Mercurial webhelyét.
* Használhat [Application Insights] toomonitor a helyen.
* A webhely szolgálnak és a mobil API a hello ugyanazt a kódot.

### <a name="upgrading-your-site"></a>A Mobile Services webhely tooAzure Mobile Apps SDK frissítése
* Node.js-alapú kiszolgáló-projektekhez hello új [Mobile Apps Node.js SDK] számos új szolgáltatást nyújt. Például mostantól helyi fejlesztési és hibakeresés, bármely fent 0.10 Node.js verziójával, és az összes Express.js köztes testreszabása.
* A. A NET-alapú kiszolgáló-projektek, új hello [Mobile Apps SDK NuGet-csomagok](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) NuGet függőségekkel több beleszólása van.  Ezeket a csomagokat hello új App Service hitelesítés támogatásához, és bármely ASP.NET-projekt állítható össze. Történő frissítéssel kapcsolatos további tudnivalókért lásd: [frissítse a meglévő .NET Mobile Service tooApp szolgáltatás](app-service-mobile-net-upgrading-from-mobile-services.md).

<!-- Images -->
[0]: ./media/app-service-mobile-migrating-from-mobile-services/migrate-to-app-service-button.PNG
[1]: ./media/app-service-mobile-migrating-from-mobile-services/migration-activity-monitor.png
[2]: ./media/app-service-mobile-migrating-from-mobile-services/triggering-job-with-postman.png

<!-- Links -->
[App Service díjszabás]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[Application Insights]: ../application-insights/app-insights-overview.md
[Automatikus méretezése]: ../app-service-web/web-sites-scale.md
[Azure App Service]: ../app-service/app-service-value-prop-what-is.md
[Azure App Service üzembe helyezési dokumentációja]: ../app-service-web/web-sites-deploy.md
[klasszikus Azure portál]: https://manage.windowsazure.com
[Azure-portálon]: https://portal.azure.com
[Azure Region]: https://azure.microsoft.com/en-us/regions/
[Azure Scheduler tervek]: ../scheduler/scheduler-plans-billing.md
[folyamatosan telepítése]: ../app-service-web/app-service-continuous-deployment.md
[Alakítsa át a vegyes névterek]: https://azure.microsoft.com/en-us/blog/updates-from-notification-hubs-independent-nuget-installation-model-pmt-and-more/
[curl]: http://curl.haxx.se/
[egyéni tartománynevek]: ../app-service-web/web-sites-custom-domain-name.md
[Fiddler]: http://www.telerik.com/fiddler
[Azure App Service általános rendelkezésre állását]: https://azure.microsoft.com/blog/announcing-general-availability-of-app-service-mobile-apps/
[hibrid kapcsolatok]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[naplózás]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Mobile Apps Node.js SDK]: https://github.com/azure/azure-mobile-apps-node
[Mobile Services vs. App Service]: app-service-mobile-value-prop-migration-from-mobile-services.md
[Notification Hubs]: ../notification-hubs/notification-hubs-push-notification-overview.md
[teljesítményfigyelés]: ../app-service-web/web-sites-monitor.md
[Postman]: http://www.getpostman.com/
[átmeneti üzembe helyezési ponti]: ../app-service-web/web-sites-staged-publishing.md
[VNet]: ../app-service-web/web-sites-integrate-with-vnet.md
[WebJobs]: ../app-service-web/websites-webjobs-resources.md
[XDT átalakítási minták]: https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples
[funkciók]: ../azure-functions/functions-overview.md
