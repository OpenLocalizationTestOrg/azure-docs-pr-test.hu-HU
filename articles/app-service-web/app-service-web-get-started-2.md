---
title: "aaaAdd funkció tooyour első webalkalmazás |} Microsoft Docs"
description: "Néhány perc alatt menő funkciókat tooyour első webalkalmazás hozzáadása."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 542671c2-22f0-4f20-8b4b-fa477264c492
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2016
ms.author: cephalin
ms.openlocfilehash: 46c9b118c2c188508cb0a369c691a79073b7d7b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-functionality-tooyour-first-web-app"></a>Funkció tooyour első webalkalmazás hozzáadása
A [központi telepítése az első web app tooAzure öt perc múlva](app-service-web-get-started-dotnet.md), egy minta webalkalmazást a telepített [Azure App Service](../app-service/app-service-value-prop-what-is.md). Ebben a cikkben gyorsan néhány remek funkciókat tooyour telepített webalkalmazás fogja hozzáadni. Néhány perc alatt:

* hitelesítést kényszeríthet ki felhasználói számára,
* automatikusan skálázhatja az alkalmazást,
* hello az alkalmazás teljesítményével kapcsolatos riasztásokat fogadhat.

Telepített hello előző cikkben melyik példaalkalmazást, függetlenül követésével hello oktatóanyag.

hello három tevékenység csupán néhány példája a számos hasznos funkciónak, amelyeket a webalkalmazás az App Service helyezésekor hello ezen oktatóanyag is. Számos hello szolgáltatást találhatók hello **szabad** réteg (ez utóbbi érték az első webalkalmazás futó tartalomról), és a szolgáltatások, amelyek magasabb tarifacsomagokhoz próbakreditjeit tootry is használhatja. Benne, hogy a webalkalmazás mindaddig az **szabad** réteg, hacsak Ön kifejezetten nem módosítja azt másik tarifacsomagra tooa.

> [!NOTE]
> hello Azure parancssori felület segítségével létrehozott webalkalmazás fut **szabad** szint, amely csak lehetővé teszi, hogy az erőforrás-kvóták egy virtuális gép példány. Az **Ingyenes** szint tartalmával kapcsolatos további információ: [App Service limits](../azure-subscription-service-limits.md#app-service-limits) (Az App Service korlátozásai).
> 
> 

## <a name="authenticate-your-users"></a>A felhasználók hitelesítése
Most adjuk meg, milyen egyszerűen tooadd hitelesítési tooyour app (további információk: [App Service hitelesítési/engedélyezési](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/)).

1. Hello portálpanelén az alkalmazás imént megnyitott, majd kattintson **beállítások** > **hitelesítési / engedélyezési**.  
    ![Hitelesítés – beállítások panelje](./media/app-service-web-get-started/aad-login-settings.png)
2. Kattintson a **a** tooturn a hitelesítés.  
3. A **Hitelesítésszolgáltatók** részen kattintson az **Azure Active Directory** lehetőségre.  
    ![Hitelesítés – az Azure AD kiválasztása](./media/app-service-web-get-started/aad-login-config.png)
4. A hello **Azure Active Directory beállításai** panelen kattintson **Express**, majd kattintson a **OK**. hello alapértelmezett beállítások hozzon létre egy új Azure AD alkalmazás az alapértelmezett címtárban.  
    ![Hitelesítés – expressz konfiguráció](./media/app-service-web-get-started/aad-login-express.png)
5. Kattintson a **Save** (Mentés) gombra.  
    ![Hitelesítés – konfiguráció mentése](./media/app-service-web-get-started/aad-login-save.png)
   
    Ha hello módosítása sikeres, megjelenik a hello értesítési Csengő egy barátságos üzenet zöld színűre.
6. Az alkalmazás portálpaneljéhez hello, kattintson hello **URL-cím** hivatkozásra (vagy **Tallózás** hello menüsávon). hello hivatkozás egy HTTP-cím.  
    ![Hitelesítés – tooURL tallózása](./media/app-service-web-get-started/aad-login-browse-click.png)  
    De hello alkalmazás új lapon nyílik meg, miután hello URL-cím mezőbe átirányítások többször, és az alkalmazás egy HTTPS-címmel rendelkező befejezése. Most azt láthatja, hogy Ön már be van jelentkezve tooyour Azure-előfizetéssel, és a rendszer automatikusan hitelesítette Önt hello alkalmazásban.  
    ![Hitelesítés – bejelentkezve](./media/app-service-web-get-started/aad-login-browse-http-postclick.png)  
    Ha most nyisson meg egy nem hitelesített munkamenetet egy másik böngészőben, megjelenik egy bejelentkezési képernyőt toohello Navigálás, azonos URL-CÍMMEL.  
    <!-- ![Authenticate - login page](./media/app-service-web-get-started/aad-login-browse.png)  -->
    Ha semmit nem tett az Azure Active Directory-val, akkor előfordulhat, hogy az alapértelmezett címtárban nincsenek Azure AD felhasználók. Ebben az esetben valószínűleg hello egyetlen fiók van hello az Azure előfizetéssel rendelkező Microsoft-fiókkal. Amely rendelkezik miért meg automatikusan a rendszer a toohello alkalmazás hello ugyanaz a böngészőből.
    A bejelentkezési lapon, valamint, hogy ugyanazt a Microsoft fiók toolog is használhatja.

Gratulálunk, akik a webes alkalmazás összes forgalom tooyour hitelesítést.

Talán észrevette a hello **hitelesítési / engedélyezési** panelen sokkal több, mint például elvégezhető:

* Közösségi bejelentkezés engedélyezése
* Többször bejelentkezési beállítás engedélyezése
* Hello alapértelmezett viselkedés módosítása első meglátogatásakor tooyour alkalmazás

App Service biztosít, így nem Önnek kell tooprovide hello hitelesítési logikát saját igényeihez néhány gyakori hitelesítési hello kulcsrakész megoldást.
További információ: [App Service Authentication/Authorization](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/) (App Service hitelesítés/engedélyezés).

## <a name="scale-your-app-automatically-based-on-demand"></a>Alkalmazás igény szerinti automatikus skálázása
Tovább, most automatikus skálázási az alkalmazást, hogy automatikusan módosítsa a kapacitás toorespond toouser igény szerinti (további információk: [vertikális felskálázás az Azure-ban az alkalmazás](web-sites-scale.md) és [méretezése példányszám manuális vagy automatikus](../monitoring-and-diagnostics/insights-how-to-scale.md)).

Röviden: a webalkalmazás két módon skálázható:

* [Vertikális felskálázás](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Nagyobb processzorteljesítmény, több memória, lemezterület és extra funkciók, például dedikált virtuális gépek, egyedi tartományok és tanúsítványok, előkészítési pontok, automatikus skálázás és még sok más. Vertikális felskálázás az alkalmazáshoz tartozó App Service-csomag tarifacsomag hello módosításával.
* [Horizontális felskálázás](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): hello az alkalmazást futtató Virtuálisgép-példányok számának növelése.
  Ki lehet terjeszteni tooas több mint 50 példányt, attól függően, hogy a tarifacsomagot.

Most pedig végre lássuk hozzá az automatikus skálázás beállításához.

1. Először is vertikálisan skálázzunk fel tooenable automatikus skálázást. Kattintson az alkalmazás portálpaneljéhez hello, **beállítások** > **vertikális Felskálázás (App Service-csomag)**.  
    ![Vertikális felskálázás – beállítások panelje](./media/app-service-web-get-started/scale-up-settings.png)
2. Görgessen és válassza hello **S1 Standard** réteg, hello (a képernyőfelvételen látható körben) automatikus skálázást támogató legalacsonyabb szint, majd kattintson a **válasszon**.  
    ![Vertikális felskálázás – szint kiválasztása](./media/app-service-web-get-started/scale-up-select.png)
   
    A vertikális felskálázás kész.
   
   > [!IMPORTANT]
   > Ez a szint elhasználja az ingyenes próbakreditjeit. Ha egy fizetési használat szerinti fizetéses fiókkal rendelkezik, felmerült díjak tooyour fiók.
   > 
   > 
3. Most pedig konfiguráljuk az automatikus skálázást. Kattintson az alkalmazás portálpaneljéhez hello, **beállítások** > **horizontális Felskálázás (App Service-csomag)**.  
    ![Horizontális felskálázás -–beállítások panelje](./media/app-service-web-get-started/scale-out-settings.png)
4. Változás **szerint** túl**processzor**. hello legördülő lista alatti hello csúszkák ennek megfelelően frissülnek. Ezután határozzon meg egy **1** és **2** közötti **példányszám** tartományt és egy **40** és **80**. közötti **céltartományt**. Ehhez írja be a hello jelölőnégyzetéből, vagy hello csúszkák mozgatásával.  
    ![Horizontális felskálázás – automatikus skálázás konfigurálása](./media/app-service-web-get-started/scale-out-configure.png)
   
    Ezen konfiguráció alapján az alkalmazás 80%-os processzor-kihasználtság felett horizontálisan skálázódik fel, 40%-os processzor-kihasználtság alatt pedig horizontálisan skálázódik le automatikusan.
5. Kattintson a **mentése** hello menüsávon.

Gratulálunk, az alkalmazás mostantól automatikusan skálázódik.

Talán észrevette a hello **skálázási beállításokat** panelen sokkal több, mint például elvégezhető:

* Manuálisan méretezhető tooa konkrét példányok száma
* Skálázás más teljesítménymutatók, például memória-kihasználtság (%) vagy lemezvárólista alapján
* Teljesítményszabály aktiválásakor a skálázási viselkedés testreszabása
* Ütemezett automatikus skálázás
* Automatikus skálázási viselkedés beállítása jövőbeli eseményekhez

Az alkalmazás vertikális felskálázásával kapcsolatban a [Scale up your app in Azure](web-sites-scale.md) (Alkalmazás vertikális felskálázása az Azure-ban) című témakörben tekinthet meg további információt. A horizontális felskálázással kapcsolatos további információ: [Scale instance count manually or automatically](../monitoring-and-diagnostics/insights-how-to-scale.md) (Példányszám manuális vagy automatikus skálázása).

## <a name="receive-alerts-for-your-app"></a>Az alkalmazással kapcsolatos riasztások fogadása
Most, hogy az alkalmazás automatikus skálázást, mi történik, ha eléri a hello maximális példányszámot (2) és a Processzor kihasználtsága a kívánt szint (80 %) felett van?
Riasztást állíthat be (további információk: [riasztási értesítéseket](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)), ez a helyzet, így tovább skálázásokat ki az alkalmazás, például tooinform. Állítsunk is be gyorsan egy riasztást ehhez a forgatókönyvhöz.

1. Kattintson az alkalmazás portálpaneljéhez hello, **eszközök** > **riasztások**.  
    ![Riasztások – beállítások panelje](./media/app-service-web-get-started/alert-settings.png)
2. Kattintson a **Riasztás hozzáadása** gombra. Ezt követően a hello **erőforrás** mezőben, válassza hello kifejezéssel végződő erőforrást **(serverfarms)**. Ez az Ön App Service-csomagja.  
    ![Riasztások – riasztások hozzáadása az App Service-csomaghoz](./media/app-service-web-get-started/alert-add.png)
3. Adja meg a **nevet**: `CPU Maxed`, a **metrikát**: **CPU Percentage** (Processzorhasználat (%)) és a **határértéket**: `90`, majd válassza ki az **E-mail küldése a tulajdonosoknak, közreműködőknek és olvasóknak** lehetőséget, és kattintson az **OK** gombra.   
    ![Riasztások – riasztás konfigurálása](./media/app-service-web-get-started/alert-configure.png)
   
    Azure végzett a létrehozással hello riasztást, amikor megjelenik a hello **riasztások** panelen.  
    ![Riasztások – záró nézet](./media/app-service-web-get-started/alert-done.png)

Gratulálunk, mostantól riasztásokat fog kapni.

Ez a riasztási beállítás ötpercenként ellenőrzi a processzor kihasználtságát. Ha ez a szám 90% fölé megy, akkor Ön, illetve minden engedélyezett felhasználó egy e-mail figyelmeztetést fog kapni. toosee bárki, aki jogosult tooreceive hello riasztások, nyissa meg az alkalmazás portálpaneljéhez toohello biztonsági másolatot, majd kattintson a hello **hozzáférés** gombra.  
![A riasztás fogadására jogosultak megtekintése](./media/app-service-web-get-started/alert-rbac.png)

Látni fogja, hogy **előfizetés rendszergazdái** még hello **tulajdonos** hello alkalmazás. Ez a csoport tartalmazhat, ha Ön hello fiókadminisztrátor az Azure előfizetés (például próbaverziós előfizetés). Az Azure szerepköralapú hozzáférés-vezérlésével kapcsolatos további információk: [Azure Role-Based Access Control](../active-directory/role-based-access-control-configure.md) (Azure szerepköralapú hozzáférés-vezérlés).

> [!NOTE]
> A riasztási szabályok az Azure szolgáltatásai. További információ: [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) (Riasztások fogadása).
> 
> 

## <a name="next-steps"></a>Következő lépések
A módszer tooconfigure hello a riasztást, talán észrevette hello az eszközök széles választékának **eszközök** panelen. Itt háríthatóak el a problémákat, figyelemmel kísérheti a teljesítményt, sebezhetőségeket kereshet, kezelheti az erőforrásokat, hello virtuális gép konzoljával kommunikál, és hasznos bővítményeket adhat hozzá. A Microsoft meghívhatjuk, az egyes eszközök toodiscover hello egyszerű, mégis hatékony eszközök a kezében lévő tooclick.

Megtudhatja, hogyan toodo további az üzembe helyezett alkalmazással. Az alábbiak csak a lista kis részét képezik:

* [Buy and configure a custom domain name](custom-dns-web-site-buydomains-web-app.md) (Egyedi tartománynév vásárlása és konfigurálása) – Vásároljon vonzó tartományt webalkalmazása számára a *.azurewebsites.net tartomány helyett. Esetleg használja már meglévő tartományai valamelyikét.
* [Átmeneti környezet beállítása](web-sites-staged-publishing.md) – az alkalmazás tooa átmeneti URL-cím Mielőtt hozzálátna az éles környezetben telepítheti. Magabiztosan frissítheti élő webalkalmazását. Összetett, több telepítési ponttal rendelkező DevOps-megoldásokat állíthat be.
* [Folyamatos üzembe helyezés beállítása](app-service-continuous-deployment.md) – Az alkalmazástelepítést a verziókövetési rendszerbe integrálhatja. Minden egyes véglegesítés után üzembe helyezheti az alkalmazását az Azure-ban.
* [Access on-premises resources](web-sites-hybrid-connection-get-started.md) (Helyszíni erőforrások elérése) – Meglévő, helyszíni adatbázisokhoz vagy CRM-rendszerekhez férhet hozzá.
* [Back up your app](web-sites-backup.md) (Az alkalmazás biztonsági mentése) – Biztonsági mentést készíthet webalkalmazásáról és visszaállíthatja azt. Készüljön fel a váratlan meghibásodásokra és hárítsa el azokat.
* [Diagnosztikai naplók engedélyezése](web-sites-enable-diagnostic-log.md) -olvasási hello IIS naplózza az Azure vagy az alkalmazáskövetéseket. Streamben, letöltve vagy az[Application Insights-ba](../application-insights/app-insights-overview.md) portolva is olvashatja őket a teljes körű elemzés érdekében.
* [Az alkalmazás a biztonsági réseket vizsgálata](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) -
  ellenőrizheti a webalkalmazás által biztosított szolgáltatás segítségével modern fenyegetésekkel szembeni [Tinfoil Security](https://www.tinfoilsecurity.com/).
* [Run background jobs](../azure-functions/functions-overview.md) (Háttérfeladatok futtatása) – Futtathat például adatfeldolgozási vagy jelentéskészítési feladatokat.
* [Ismerje meg az App Service működését](../app-service/app-service-how-works-readme.md)

