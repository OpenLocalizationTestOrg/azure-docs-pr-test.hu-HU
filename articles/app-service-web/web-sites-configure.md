---
title: "aaaConfigure webalkalmazásokkal az Azure App Service-ben"
description: "Hogyan tooconfigure egy webalkalmazást az Azure App Service szolgáltatások"
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9af8a367-7d39-4399-9941-b80cbc5f39a0
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 8697ab6f21cfeb470e11f0d82c68692d43142fc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-web-apps-in-azure-app-service"></a>Webalkalmazások konfigurálása az Azure App Service-ben
Ez a témakör ismerteti, hogyan web app használatával tooconfigure hello [Azure Portal].

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="application-settings"></a>Alkalmazásbeállítások
1. A hello [Azure Portal], nyissa meg hello webalkalmazás hello panelen.
2. Kattintson a **összes beállítás**.
3. Kattintson a **Alkalmazásbeállítások**.

![Alkalmazásbeállítások][configure01]

Hello **Alkalmazásbeállítások** panel több kategóriák szerint csoportosítva beállításokkal rendelkezik.

### <a name="general-settings"></a>Általános beállítások
**Keretrendszer-verziók**. Állítsa be ezeket a beállításokat, ha az alkalmazás használja, minden ezen keretrendszerek: 

* **.NET-keretrendszer**: Set hello .NET-keretrendszer verziója. 
* **A PHP**: hello PHP verzióját, vagy **OFF** toodisable PHP. 
* **Java**: Select hello Java-verziót vagy **OFF** toodisable Java. Használjon hello **webes tároló** beállítás toochoose Tomcat- és Jetty verziói között.
* **Python**: Select hello Python verzióját, vagy **OFF** toodisable Python.

Technikai okokból hello .NET, PHP és Python beállítások Java engedélyezése az alkalmazás letiltása.

<a name="platform"></a>
**Platform**. Választja ki, hogy a webalkalmazás fut, 32 bites vagy 64 bites környezetben. hello 64 bites környezet Basic vagy Standard módot igényel. Ingyenes, és megosztott mód mindig 32-bit-es környezetben fusson.

**Webes szoftvercsatornák**. Állítsa be **ON** tooenable hello WebSocket protokoll; például, ha a webes alkalmazás használ [ASP.NET SignalR] vagy [socket.io].

<a name="alwayson"></a>
**Always On**. Alapértelmezés szerint a webalkalmazások a memóriából, ha bizonyos ideig inaktív. Ez lehetővé teszi, hogy az erőforrások megőrzése hello rendszer. Basic vagy Standard módban, ahol engedélyezheti **mindig a** tookeep hello app betöltött összes hello idő. Ha az alkalmazás fut a folyamatos webjobs-feladatok webjobs-feladatok futtatása indított CRON-kifejezés használatával, vagy engedélyezze **mindig a**, vagy hello webes feladatok nem futtatható megbízhatóan.

**Felügyelt folyamatkezelési verzió**. Készletek hello IIS [folyamatkezelési mód]. Hagyja meg a beállított tooIntegrated (hello alapértelmezett), kivéve, ha egy örökölt alkalmazás, amely az IIS egy régebbi verziója szükséges.

**Az automatikus felcserélés**. Ha engedélyezte az automatikus felcserélés egy üzembe helyezési tárhelyet, az App Service lesz automatikusan felcserélése hello webalkalmazás éles környezetben amikor egy frissítés toothat helyre. További információkért lásd: [központi telepítése az Azure App Service web Apps toostaging tárhelyek](web-sites-staged-publishing.md).

### <a name="debugging"></a>Hibakeresés
**Távoli hibakeresés**. Távoli hibakeresésének engedélyezése. Ha engedélyezve van, használhatja hello távoli hibakereső a Visual Studio tooconnect közvetlen tooyour webalkalmazás. Távoli hibakeresés továbbra is engedélyezett marad 48 órán keresztül. 

### <a name="app-settings"></a>Alkalmazásbeállítások
Ebben a szakaszban található név/érték párok, akkor web app betölti a kezdőlapon. 

* .NET-alkalmazások esetén ezek a beállítások vannak be a nézetmodellbe, a .NET-konfiguráció `AppSettings` futásidőben, felülírva a meglévő beállításokat. 
* A PHP, Python, Java és csomópont alkalmazások férhetnek hozzá ezeket a beállításokat a környezeti változók futásidőben. Minden egyes alkalmazás-beállítás két környezeti változók jönnek létre; egy megadott hello app beállítás bejegyzést, és egy másik APPSETTING_ előtaggal rendelkező hello nevet. Mindkettő rendelkezik hello ugyanazt az értéket.

### <a name="connection-strings"></a>Kapcsolati karakterláncok
Kapcsolati karakterláncok kapcsolt erőforrásokban. 

.NET-alkalmazások esetén a kapcsolati karakterláncok vannak be a nézetmodellbe, a .NET-konfiguráció `connectionStrings` beállításokat futásidőben, felülírva a meglévő bejegyzéseket, ahol hello kulcs értéke hello csatolt adatbázis neve. 

A PHP, Python, Java és csomópont alkalmazások ezek a beállítások használhatók környezeti változók előtagként hello kapcsolattípus futásidőben. hello környezeti változó előtagok a következők: 

* SQL Server:`SQLCONNSTR_`
* MySQL:`MYSQLCONNSTR_`
* SQL-adatbázis:`SQLAZURECONNSTR_`
* Egyéni:`CUSTOMCONNSTR_`

Például, ha a MySql-kapcsolati karakterlánc lett nevű `connectionstring1`, akkor elérhetőek hello környezeti változó `MYSQLCONNSTR_connectionString1`.

### <a name="default-documents"></a>Alapértelmezett dokumentum
hello alapértelmezett dokumentum egy hello weblap, akkor jelenik meg, a webhely hello gyökér URL-címen.  hello első egyező hello listában fájllal. 

Webalkalmazások használhatja a modulok, hogy útvonal URL-címe alapján. ahelyett, hogy nincs alapértelmezett dokumentum ilyen szolgáltató statikus tartalmat, ebben az esetben nincs.    

### <a name="handler-mappings"></a>Kezelőleképezések
A terület tooadd egyéni parancsfájl processzorok toohandle kérelmek adott fájlkiterjesztésekre használja. 

* **Bővítmény**. hello fájl kiterjesztése toobe kezelni, például *.php vagy handler.fcgi. 
* **Parancsfájl-feldolgozó elérési útja**. hello abszolút elérési útja hello parancsfájl-feldolgozókhoz. Hello parancsprogram-feldolgozó által a kérelmek toofiles hello fájlkiterjesztés megfelelő dolgoz fel. Elérési út használata hello `D:\home\site\wwwroot` toorefer tooyour alkalmazás gyökérkönyvtárában.
* **További argumentumok**. Parancssori argumentumokat használni hello parancsprogram-feldolgozó 

### <a name="virtual-applications-and-directories"></a>Virtuális alkalmazások és könyvtárak
tooconfigure virtuális alkalmazások és könyvtárak, adja meg az egyes virtuális könyvtárakat és az annak megfelelő fizikai elérési út relatív toohello webhely gyökeréhez. Másik lehetőségként választhatja hello **alkalmazás** jelölőnégyzet toomark alkalmazásként egy virtuális könyvtárat.

## <a name="enabling-diagnostic-logs"></a>Diagnosztikai naplók engedélyezése
diagnosztikai naplók tooenable:

1. A webalkalmazás hello paneljén kattintson **összes beállítás**.
2. Kattintson a **diagnosztikai naplók**. 

Diagnosztikai naplók írása egy webalkalmazás, amely támogatja a naplózási beállítások: 

* **Alkalmazásnaplózás**. Írja az alkalmazásnaplók toohello fájlrendszer. A naplózás 12 óra ideig tart. 

**Szint**. Ha az alkalmazásadatok naplózása engedélyezve van, ez a beállítás ennyi hello rögzített (hiba, figyelmeztetés, információ, vagy a részletes) adatokat.

**Webkiszolgáló naplózásának**. Naplók hello W3C bővített naplófájlformátum lesznek mentve. 

**A részletes hibaüzeneteket**. Menti a hiba részleteit a .htm fájl üzenetek. hello mentésre /LogFiles/DetailedErrors alatt. 

**Sikertelen kérelmek követésének**. Naplók kérelmek tooXML fájlokat nem sikerült. hello menti a/naplófájlok/W3SVC*xxx*, ahol a xxx egyedi azonosítója. Ez a mappa tartalmaz egy XSL-fájlt és egy vagy több XML-fájlokat. Győződjön meg arról, hogy toodownload hello XSL-fájl, mert funkciókat biztosít a formázás és szűrés hello XML-fájlok hello tartalmát.

kell tooview hello naplófájlok, FTP hitelesítő adatokat, az alábbiak szerint hozzon létre:

1. A webalkalmazás hello paneljén kattintson **összes beállítás**.
2. Kattintson a **üzembe helyezési hitelesítő adatok**.
3. Adjon meg egy felhasználónevet és jelszót.
4. Kattintson a **Save** (Mentés) gombra.

![Telepítési hitelesítő adatok beállítása][configure03]

Ha a rendszer "app\username" Hello teljes FTP-felhasználó nevét *app* hello a webes alkalmazás neve. hello felhasználónév szerepel hello webalkalmazás panelen a **Essentials**.  

![FTP telepítési hitelesítő adatok][configure02]

## <a name="other-configuration-tasks"></a>Egyéb konfigurációs feladatok
### <a name="ssl"></a>SSL
Basic vagy Standard módban SSL-tanúsítványokat az egyéni tartományt is feltölthet. További információ: [HTTPS engedélyezése az webalkalmazáshoz]. 

tooview a feltöltött tanúsítványok kattintson **összes beállítás** > **egyéni tartományok és SSL**.

### <a name="domain-names"></a>Tartománynevek
A webalkalmazás egyéni tartományneveket adhat hozzá. További információ: [egy webalkalmazást az Azure App Service szolgáltatásban az egyéni tartománynév konfigurálása].

tooview a tartománynevek kattintson **összes beállítás** > **egyéni tartományok és SSL**.

### <a name="deployments"></a>Központi telepítés
* Folyamatos üzembe helyezést beállítani. Lásd: [az Azure App Service Web Apps használatával Git toodeploy](web-sites-deploy.md).
* Üzembe helyezési pontok. Lásd: [tooStaging környezetek telepítése az Azure App Service Web Apps].

Kattintson a tooview az üzembe helyezési **összes beállítás** > **üzembe helyezési**.

### <a name="monitoring"></a>Figyelés
Basic vagy Standard módban hello rendelkezésre állási HTTP vagy HTTPS-végpontok, tesztelheti a toothree földrajzilag elosztott helyeken fel. A figyelési teszt sikertelen, ha HTTP-válaszkód hello (4xx vagy 5xx) hiba vagy hello válasz több mint 30 másodpercet vesz igénybe. A végpont tekinthető érhető el, ha az összes sikeres hello figyelési tesztek hello megadott helyeken. 

További információkért lásd: [Útmutató: webes végpont állapotának figyelése].

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása], ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="next-steps"></a>Következő lépések
* [Egyéni tartománynév konfigurálása az Azure App Service-ben]
* [HTTPS engedélyezése az alkalmazásoknak az Azure App Service-ben]
* [A webalkalmazás skálázása az Azure App Service-ben]
* [Az Azure App Service Web Apps figyelési alapjai]

<!-- URL List -->

[ASP.NET SignalR]: http://www.asp.net/signalr
[Azure Portal]: https://portal.azure.com/
[Egyéni tartománynév konfigurálása az Azure App Service-ben]: ./app-service-web-tutorial-custom-domain.md
[tooStaging környezetek telepítése az Azure App Service Web Apps]: ./web-sites-staged-publishing.md
[HTTPS engedélyezése az alkalmazásoknak az Azure App Service-ben]: ./app-service-web-tutorial-custom-ssl.md
[Útmutató: webes végpont állapotának figyelése]: http://go.microsoft.com/fwLink/?LinkID=279906
[Az Azure App Service Web Apps figyelési alapjai]: ./web-sites-monitor.md
[folyamatkezelési mód]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[A webalkalmazás skálázása az Azure App Service-ben]: ./web-sites-scale.md
[socket.io]: ./web-sites-nodejs-chat-app-socketio.md
[App Service kipróbálása]: https://azure.microsoft.com/try/app-service/

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png
