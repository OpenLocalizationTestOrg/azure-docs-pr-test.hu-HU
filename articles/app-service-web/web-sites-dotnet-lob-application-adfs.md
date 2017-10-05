---
title: "Üzleti Azure-alkalmazás létrehozása az AD FS-hitelesítéshez |} Microsoft Docs"
description: "Megtudhatja, hogyan üzleti alkalmazás létrehozása az Azure App Service-ben, amely hitelesíti a helyszíni STS. Ez az oktatóanyag a helyszíni STS-ként AD FS célozza."
services: app-service\web
documentationcenter: .net
author: cephalin
manager: erikre
editor: 
ms.assetid: 0fa9f7a1-37bd-4d11-845f-aeff6fc9e4ca
ms.service: app-service-web
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 08/31/2016
ms.author: cephalin
ms.openlocfilehash: f9a8984400378d154a504af8a41609900128d052
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-line-of-business-azure-app-with-ad-fs-authentication"></a>Üzleti Azure-alkalmazás létrehozása az AD FS-hitelesítéshez
A cikkből megtudhatja, hogyan hozhat létre egy ASP.NET MVC üzleti alkalmazást a [Azure App Service](../app-service/app-service-value-prop-what-is.md) használatával egy helyszíni [Active Directory összevonási szolgáltatások](http://technet.microsoft.com/library/hh831502.aspx) az identitás-szolgáltatóként. Ebben a forgatókönyvben is működik, amikor az Azure App Service-üzleti alkalmazások létrehozásához, de a szervezet megköveteli a címtáradatok helyszínen tárolni.

> [!NOTE]
> A különböző vállalati hitelesítési és engedélyezési beállítások az Azure App Service-áttekintését lásd: [hitelesítés a helyszíni Active Directoryval, az Azure alkalmazásban](web-sites-authentication-authorization.md).
> 
> 

<a name="bkmk_build"></a>

## <a name="what-you-will-build"></a>Mit fog létrehozni
Egy egyszerű ASP.NET-alkalmazások az Azure App Service Web Apps a következő funkciók fog létrehozni:

* Hitelesíti a felhasználókat az AD FS ellen
* Használja a `[Authorize]` engedélyezése a felhasználóknak a különféle műveletek
* A Visual Studio hibakeresési, mind az App Service Web Apps történő közzététel statikus konfigurációját (csak egyszer konfigurálnia, hibakeresését és bármikor közzététele)  

<a name="bkmk_need"></a>

## <a name="what-you-need"></a>Mi szükséges
[!INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:

* Egy helyszíni AD FS üzembe helyezése (ebben az oktatóanyagban használt tesztkörnyezet végpont útmutatót lásd: [tesztlabor: az AD FS az Azure virtuális gép (csak tesztelési) önálló STS](https://blogs.msdn.microsoft.com/cephalin/2014/12/21/test-lab-standalone-sts-with-ad-fs-in-azure-vm-for-test-only/))
* Függő létrehozásához szükséges engedélyek fél AD FS felügyelet bizalmi kapcsolatok
* A Visual Studio 2013 Update 4 vagy újabb verzió
* [Az Azure SDK 2.8.1-es verziójának](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) vagy újabb verzió

<a name="bkmk_sample"></a>

## <a name="use-sample-application-for-line-of-business-template"></a>Mintaalkalmazást használ üzleti sablon
Ebben az oktatóanyagban a mintaalkalmazáshoz [WebApp-WSFederation-DotNet)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet), az Azure Active Directory csapata hozza létre. Mivel az AD FS támogatja a WS-Federation, használhatja azt sablonként üzleti alkalmazások létrehozása a könnyű kezelés. A következő jellemzőkkel rendelkezik:

* Használja a [WS-Federation](http://msdn.microsoft.com/library/bb498017.aspx) való hitelesítéshez szükséges egy helyszíni AD FS üzembe helyezése
* Be- és kijelentkezési funkció
* Használja a [Microsoft.Owin](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana) (a Windows Identity Foundation), és nem olyan a jövőbeli az ASP.NET és jóval egyszerűbb, hitelesítési és engedélyezési mint WIF beállítása

<a name="bkmk_setup"></a>

## <a name="set-up-the-sample-application"></a>A mintaalkalmazás beállítása
1. Klónozza, vagy töltse le a minta megoldást a következő [WebApp-WSFederation-DotNet](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) a helyi könyvtárba.
   
   > [!NOTE]
   > Található utasítások segítségével: [README.md](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet/blob/master/README.md) megtudhatja, hogyan állíthat be az alkalmazás az Azure Active Directoryban. Azonban ebben az oktatóanyagban, állítsa be az AD FS-sel, ezért a lépések az itt helyette.
   > 
   > 
2. Nyissa meg a megoldást, és nyissa meg a Controllers\AccountController.cs a **Megoldáskezelőben**.
   
   Látni fogja, hogy a kód egy hitelesítési kérdés hitelesíteni a felhasználót, WS-Federation használatával egyszerűen ad ki. Minden hitelesítési App_Start\Startup.Auth.cs van konfigurálva.
3. Nyissa meg a App_Start\Startup.Auth.cs. Az a `ConfigureAuth` metódus, vegye figyelembe a sor:
   
       app.UseWsFederationAuthentication(
           new WsFederationAuthenticationOptions
           {
               Wtrealm = realm,
               MetadataAddress = metadata                                      
           });
   
   OWIN világában a részlet valójában az operációs rendszer minimális kell konfigurálnia a WS-Federation hitelesítési. Már jóval egyszerűbb, több elegáns, mint a WIF, ahol Web.config van beszúrta XML hely számos országában dolgoznak. Információ szüksége a függő entitás (RP) azonosítóját és az AD FS szolgáltatást metaadatait tartalmazó fájl URL-CÍMÉT. Íme egy példa:
   
   * Függő Entitás azonosítója:`https://contoso.com/MyLOBApp`
   * Metaadatok címe:`http://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`
4. App_Start\Startup.Auth.cs módosítsa a következő statikus karakterlánc-definíciók:  
   
   <pre class="prettyprint">
   private static string realm = ConfigurationManager.AppSettings["ida:<mark>RPIdentifier</mark>"];
   <mark><del>private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];</del></mark>
   <mark><del>private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];</del></mark>
   <mark><del>private static string metadata = string.Format("{0}/{1}/federationmetadata/2007-06/federationmetadata.xml", aadInstance, tenant);</del></mark>
   <mark>private static string metadata = string.Format("https://{0}/federationmetadata/2007-06/federationmetadata.xml", ConfigurationManager.AppSettings["ida:ADFS"]);</mark>
   
   <mark><del>string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);</del></mark>
   </pre>
5. Most végezze el a megfelelő módosításokat a Web.config fájlban. Nyissa meg a Web.config fájl, és módosítsa a következő beállításokkal:  
   
   <pre class="prettyprint">
   &lt;appSettings&gt;
   &lt;add key="webpages:Version" value="3.0.0.0" /&gt;
   &lt;add key="webpages:Enabled" value="false" /&gt;
   &lt;add key="ClientValidationEnabled" value="true" /&gt;
   &lt;add key="UnobtrusiveJavaScriptEnabled" value="true" /&gt;
   <mark><del>&lt;add key="ida:Wtrealm" value="[Enter the App ID URI of WebApp-WSFederation-DotNet https://contoso.onmicrosoft.com/WebApp-WSFederation-DotNet]" /&gt;</del></mark>
   <mark><del>&lt;add key="ida:AADInstance" value="https://login.microsoftonline.com" /&gt;</del></mark>
   <mark><del>&lt;add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" /&gt;</del></mark>
   <mark>&lt;add key="ida:RPIdentifier" value="[Enter the relying party identifier as configured in AD FS, e.g. https://localhost:44320/]" /&gt;</mark>
   <mark>&lt;add key="ida:ADFS" value="[Enter the FQDN of AD FS service, e.g. adfs.contoso.com]" /&gt;</mark>
   
   &lt;/appSettings&gt;
   </pre>
   
   Töltse ki a megfelelő környezet alapján értékek.
6. Győződjön meg arról, hogy nincsenek hibák az alkalmazás létrehozása.

Ennyi az egész. Most már a mintaalkalmazáshoz az AD FS működik készen áll. Továbbra is szeretné egy függő Entitás megbízhatósági kapcsolat beállítása az AD FS újabb alkalmazáshoz.

<a name="bkmk_deploy"></a>

## <a name="deploy-the-sample-application-to-azure-app-service-web-apps"></a>Az Azure App Service Web Apps a mintaalkalmazás telepítése
Itt akkor tegye közzé az alkalmazást egy webes alkalmazás az App Service Web Apps a hibakeresési környezetben adatainak megőrzése mellett. Vegye figyelembe, hogy az alkalmazás közzététele előtt azt egy függő Entitás megbízhatósági kapcsolattal rendelkezik azzal az AD FS-ben, a hitelesítés továbbra sem működik még fog. Azonban ha így tesz, akkor most lehet a webes alkalmazás URL-CÍMÉT, melyekkel később konfigurálhatja a függő Entitás megbízhatóságát.

1. Kattintson jobb gombbal a projektre, és válassza ki **közzététel**.
   
    ![](./media/web-sites-dotnet-lob-application-adfs/01-publish-website.png)
2. Válassza ki **Microsoft Azure App Service**.
3. Ha még nem jelentkezett be az Azure-ba, kattintson a **bejelentkezés** és az Azure-előfizetéshez a Microsoft-fiók használatával jelentkezzen be.
4. Miután bejelentkezett, kattintson a **új** egy webalkalmazás létrehozása.
5. Töltse ki az összes kötelező mezőt. Csatlakozzon a helyszíni adatok később, a webalkalmazás-adatbázis létrehozása nem kívánja.
   
    ![](./media/web-sites-dotnet-lob-application-adfs/02-create-website.png)
6. Kattintson a **Create** (Létrehozás) gombra. A webalkalmazás létrehozása után a webhely közzététele párbeszédpanelen van megnyitva.
7. A **URL-címre**, módosítsa **http** való **https**. Másolja a teljes URL-címet egy szövegszerkesztőben későbbi használatra. Kattintson a **közzététel**.
   
    ![](./media/web-sites-dotnet-lob-application-adfs/03-destination-url.png)
8. A Visual Studióban nyissa meg a **Web.Release.config** a projektben. Helyezze be a következő XML a `<configuration>` címkét, és cserélje le a kulcs értékét a közzététel webes alkalmazás URL-CÍMÉT.  
   
   <pre class="prettyprint">
   &lt;appSettings&gt;
   &lt;add key="ida:RPIdentifier" value="<mark>[e.g. https://mylobapp.azurewebsites.net/]</mark>" xdt:Transform="SetAttributes" xdt:Locator="Match(key)" /&gt;
   &lt;/appSettings&gt;</pre>

Amikor elkészült, hogy a projekt, a Visual Studio hibakeresési környezetben, és egy a közzétett webalkalmazás az Azure-ban beállított két RP azonosítót. Az egyes a két környezet között, az AD FS fog beállítani egy függő Entitás megbízhatósági. Hibakeresési, a beállítások a Web.config fájlban szolgálja, hogy a **Debug** az AD FS konfigurációs munka. Ha közzé van téve (alapértelmezés szerint a **kiadás** konfigurációs közzé van téve), átalakított Web.config tölt fel, amely magában foglalja a Web.Release.config app beállítás változásait.

Ha azt szeretné, hogy a közzétett webalkalmazás az Azure szolgáltatásban csatlakoztatni a hibakereső (vagyis fel kell töltenie a kód a közzétett webes alkalmazás hibakeresési szimbólumok), a hibakeresési konfiguráció Azure hibakereséshez, de a saját egyéni Web.config transzformálása (pl. Web.AzureDebug.config), amely az alkalmazás beállításai Web.Release.config másolat hozhat létre. Ez lehetővé teszi, hogy statikus karbantartása a különböző környezetek között.

<a name="bkmk_rptrusts"></a>

## <a name="configure-relying-party-trusts-in-ad-fs-management"></a>Megbízható függő entitások AD FS felügyelet konfigurálása
Most kell egy függő Entitás megbízhatóságának AD FS felügyeleti konfigurálása előtt használja a mintaalkalmazást, és az AD FS ténylegesen hitelesítéséhez. Szüksége lesz a két külön RP bizalmi kapcsolatok, a hibakeresés környezethez és az egyikben a közzétett webalkalmazás beállítása.

> [!NOTE]
> Győződjön meg arról, hogy mindkét a környezetek esetében ismételje meg az alábbi lépéseket.
> 
> 

1. Az AD FS kiszolgálón jelentkezzen be az AD FS felügyeleti jogosultságokkal rendelkező hitelesítő adatokat.
2. Nyissa meg az AD FS felügyeleti konzolt. Kattintson a jobb gombbal **AD FS\Trusted Relationships\Relying entitások Megbízhatóságai** válassza **függő entitás megbízhatóságának hozzáadása**.
   
   ![](./media/web-sites-dotnet-lob-application-adfs/1-add-rptrust.png)
3. Az a **adatforrás kiválasztása** lapon, hogy melyik **függő entitással kapcsolatos adatok manuális megadása**. 
   
   ![](./media/web-sites-dotnet-lob-application-adfs/2-enter-rp-manually.png)
4. Az a **megjelenítendő név megadása** lapon adjon meg egy megjelenítési nevet az alkalmazáshoz, és kattintson **következő**.
5. Az a **válasszon protokoll** kattintson **következő**.
6. Az a **tanúsítvány konfigurálása** kattintson **következő**.
   
   > [!NOTE]
   > Mivel kell használni HTTPS már, titkosított jogkivonatok nem kötelező. Ha valóban szeretné titkosítani az ezen az oldalon az AD FS jogkivonatokat, jogkivonat-visszafejtési logikát is hozzá kell adnia a kódot. További információkért lásd: [manuálisan titkosítja az OWIN WS-Federation köztes konfigurálása és elfogadása jogkivonatok](http://chris.59north.com/post/2014/08/21/Manually-configuring-OWIN-WS-Federation-middleware-and-accepting-encrypted-tokens.aspx).
   > 
   > 
7. Helyezze át a következő lépés az alakzatot, meg kell információ a Visual Studio projektből. A projekt tulajdonságait, jegyezze fel a **SSL URL-cím** az alkalmazás. 
   
   ![](./media/web-sites-dotnet-lob-application-adfs/3-ssl-url.png)
8. AD FS felügyelet, a biztonsági másolatot a **URL konfigurálása** oldalán a **függő entitás megbízhatóságának hozzáadása varázsló**, jelölje be **a WS-Federation passzív protokoll támogatásának engedélyezése** és az SSL URL-cím típusú-e a Visual Studio-projektet az előző lépésben feljegyzett. Ezután kattintson a **Tovább** gombra.
   
   ![](./media/web-sites-dotnet-lob-application-adfs/4-configure-url.png)
   
   > [!NOTE]
   > URL-CÍMÉT határozza meg, hova küldje az ügyfél a sikeres hitelesítést követően. A hibakeresési környezetben kell <code>https://localhost:&lt;port&gt;/</code>. A közzétett webes alkalmazás esetén a webes alkalmazás URL-CÍMÉT kell lennie.
   > 
   > 
9. Az a **azonosítók konfigurálása** lapon ellenőrizze, hogy a projekt SSL URL-cím már szerepel-e, majd kattintson **következő**. Kattintson a **következő** egészen az alapértelmezett beállításokat a varázsló befejezéséhez.
   
   > [!NOTE]
   > A App_Start\Startup.Auth.cs a Visual Studio-projekt, ez az azonosító értékének elleni egyezik <code>WsFederationAuthenticationOptions.Wtrealm</code> összevont hitelesítés során. Alapértelmezés szerint az előző lépésben az alkalmazás URL-címe meg van adva egy függő Entitás azonosítója.
   > 
   > 
10. Most már befejezte a projekt a RP-alkalmazások konfigurálása az AD FS-ben. Ezután állítsa be az alkalmazás a jogcímek, az alkalmazás által igényelt küldésére. A **Jogcímszabályok szerkesztése** párbeszédpanelen alapértelmezés szerint megnyílik az Ön a varázsló végén, és azonnal megkezdje. Pedig konfiguráljuk legalább a következő jogcímeket (zárójelben sémák):
    
    * Az ASP.NET által használt hydrate neve (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) - `User.Identity.Name`.
    * Egyszerű felhasználónév (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn) – a szervezethez tartozó felhasználók egyedi azonosítására szolgál.
    * A csoporttagságokat, szerepkörök (http://schemas.microsoft.com/ws/2008/06/identity/claims/role) - használható `[Authorize(Roles="role1, role2,...")]` decoration tartományvezérlők/műveletek engedélyezéséhez. A valóságban ez a módszer nem lehet a legtöbb performant a szerepkör-hitelesítéshez. Ha az AD-felhasználók biztonsági csoport több száz tartozik, több száz szerepkör jogcím szerepel a SAML-jogkivonat válnak. Egy másik módszert-hoz feltételes attól függően, hogy a felhasználó tagságát szerepkör egyetlen jogcímet küld ki, egy adott csoport. Azonban szavatoljuk az egyszerű ehhez az oktatóanyaghoz.
    * Azonosító (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier) Name,-hamisítás érvényesítéshez használható. Hamisítás érvényesítéssel használható legyen kapcsolatos további információkért lásd: a **üzleti funkciókkal** szakasza [üzleti Azure-alkalmazás létrehozása az Azure Active Directory hitelesítési](web-sites-dotnet-lob-application-azure-ad.md#bkmk_crud).
    
    > [!NOTE]
    > A jogcímtípusok, konfigurálnia kell az alkalmazás az alkalmazás igényeinek határozza meg. Azure Active Directory-alkalmazások (azaz a függő Entitás megbízhatósági kapcsolatok) által támogatott jogcímek listája, például: [támogatott jogkivonat és jogcímtípusok](http://msdn.microsoft.com/library/azure/dn195587.aspx).
    > 
    > 
11. Kattintson a Jogcímszabályok szerkesztése párbeszédpanel **szabály hozzáadása**.
12. Konfigurálja a nevét, az egyszerű felhasználónév és a szerepkör jogcím, ahogy a képernyőkép, majd kattintson az **Befejezés**.
    
    ![](./media/web-sites-dotnet-lob-application-adfs/5-ldap-claims.png)
    
    Ezután hozzon létre egy átmeneti nevet azonosító jogcím bemutatott lépések [azonosítókat a SAML helyességi feltételek](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).
13. Kattintson a **szabály hozzáadása** újra.
14. Válassza ki **jogcímek küldése egyéni szabály segítségével** kattintson **következő**.
15. Illessze be a következő nyelvének azokat a **egyéni szabály** mezőbe a szabály neve **munkamenet azonosítója** kattintson **Befejezés**.  
    
    <pre class="prettyprint">
    c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"] &amp;&amp;
    c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationinstant"]
     => add(
         store = "_OpaqueIdStore",
         types = ("<mark>http://contoso.com/internal/sessionid</mark>"),
         query = "{0};{1};{2};{3};{4}",
         param = "useEntropy",
         param = c1.Value,
         param = c1.OriginalIssuer,
         param = "",
         param = c2.Value);
    </pre>
    
    Az egyéni szabály ezen a képernyőfelvételen látható hasonlóan kell kinéznie:
    
    ![](./media/web-sites-dotnet-lob-application-adfs/6-per-session-identifier.png)
16. Kattintson a **szabály hozzáadása** újra.
17. Válassza ki **bejövő jogcím átalakítása** kattintson **következő**.
18. A szabály konfigurálása (a létrehozott egyéni szabály jogcímtípus használatával) a képernyőfelvételen látható módon, és kattintson a **Befejezés**.
    
    ![](./media/web-sites-dotnet-lob-application-adfs/7-transient-name-id.png)
    
    Az átmeneti Névazonosító jogcímet lépéseinek részletes információkért lásd: [azonosítókat a SAML helyességi feltételek](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).
19. Kattintson a **alkalmaz** a a **Jogcímszabályok szerkesztése** párbeszédpanel. Mostantól az alábbi képernyőfelvételen hasonlóan kell kinéznie:
    
    ![](./media/web-sites-dotnet-lob-application-adfs/8-all-claim-rules.png)
    
    > [!NOTE]
    > Ebben az esetben győződjön meg arról, hogy a hibakeresési környezetben és a közzétett webes alkalmazás ismételje meg ezeket a lépéseket.
    > 
    > 

<a name="bkmk_test"></a>

## <a name="test-federated-authentication-for-your-application"></a>Az alkalmazás összevont hitelesítés tesztelése
Készen áll az alkalmazás hitelesítési logikát szemben az AD FS tesztelése. Az AD FS laborkörnyezetben, amelyhez tartozik egy tesztcsoportot, az Active Directory (AD) tesztfelhasználó van.

![](./media/web-sites-dotnet-lob-application-adfs/10-test-user-and-group.png)

A hibakereső hitelesítés teszteléséhez mindössze annyit kell tennie most típus `F5`. Ha szeretné a közzétett webes alkalmazás alapuló hitelesítésének tesztelése, keresse meg az URL-címet.

A webalkalmazás betöltése után kattintson **bejelentkezés**. Most szerezheti be bejelentkezési vagy a bejelentkezési oldal AD FS-ben az AD FS által választott hitelesítési módszertől függően szolgálja ki. Ez mit jelenik meg az Internet Explorer 11.

![](./media/web-sites-dotnet-lob-application-adfs/9-test-debugging.png)

Az AD-tartomány az AD FS központi telepítés a felhasználó jelentkezik, ha meg kell jelennie a kezdőlapon újra **Hello, <User Name>!** a sarokban. Ez mit jelenik meg.

![](./media/web-sites-dotnet-lob-application-adfs/11-test-debugging-success.png)

Az eddigi sikeresen a következőképpen:

* Az alkalmazás sikeresen elérte az AD FS és a megfelelő függő Entitás azonosítója az AD FS adatbázisában található
* Az AD-felhasználó és az alkalmazás kezdőlapja biztonsági átirányítási AD FS sikeresen hitelesítette.
* AD FS Szolgáltatásban sikeresen elküldte a jogcím nevét (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) az alkalmazás, ahogy azt a tényt, hogy a felhasználó neve sarokban jelenik meg. 

Ha a jogcím nevét hiányzik, akkor amint láthatta **Hello,!**. Ha megnézzük Views\Shared\_LoginPartial.cshtml, találja, hogy használja `User.Identity.Name` megjelenítése a felhasználónév. Ahogy korábban említettük, ha a név jogcímet a hitelesített felhasználó érhető el a SAML-jogkivonat ASP.NET hydrates Ez a tulajdonság azt. A jogcímek az AD FS által küldött megtekintéséhez be töréspont Controllers\HomeController.cs, Index művelet metódus. A felhasználó hitelesítése után vizsgálja meg a `System.Security.Claims.Current.Claims` gyűjtemény.

![](./media/web-sites-dotnet-lob-application-adfs/12-test-debugging-all-claims.png) 

<a name="bkmk_authorize"></a>

## <a name="authorize-users-for-specific-controllers-or-actions"></a>Adott vezérlőket vagy a műveletek a felhasználók hitelesítéséhez
Csoporttagságok szerepkör jogcímekként bevont a függő Entitás szintű bizalmi kapcsolatainak konfigurációja, mivel most már használhatja azokat közvetlenül a `[Authorize(Roles="...")]` decoration tartományvezérlőket és a műveletek. A létrehozás-olvasás-Módosítás-Törlés (CRUD) mintával üzleti alkalmazásokban minden művelet érhető el az egyes szerepkörök engedélyezhető. Most akkor lesz csak próbálja ki ezt a funkciót a meglévő otthoni vezérlő.

1. Nyissa meg a Controllers\HomeController.cs.
2. Adja a `About` és `Contact` hasonló az alábbi kód, használja-e biztonsági műveletmetódusokhoz csoporttagságok a hitelesített felhasználó rendelkezik-e.  
   
    <pre class="prettyprint">
    <mark>[Authorize(Roles="Test Group")]</mark>
    public ActionResult About()
    {
        ViewBag.Message = "Your application description page.";
   
        return View();
    }
   
    <mark>[Authorize(Roles="Domain Admins")]</mark>
    public ActionResult Contact()
    {
        ViewBag.Message = "Your contact page.";
   
        return View();
    }
    </pre>
   
    Óta hozzáadott **tesztelése felhasználói** való **Tesztcsoport** a az AD FS laborkörnyezetben engedélyezési tesztelése a Tesztcsoport fogok használni `About`. A `Contact`, a negatív gyorsítás esetében is tesztelni fogja **Tartománygazdák**található amely **tesztelése felhasználói** nem tartozik.
3. Indítsa el a hibakereső beírásával `F5` jelentkezzen be, majd kattintson a **kapcsolatos**. Ön most kell megtekintése a `~/About/Index` lapon sikeres legyen, ha a hitelesített felhasználó jogosult-e az adott művelethez.
4. Most kattintson **forduljon**, ami abban az esetben, ha nem engedélyezhetik **tesztfelhasználó** művelethez. Azonban a böngésző átirányítja az AD FS, végül megjelenítheti az ezt az üzenetet:
   
    ![](./media/web-sites-dotnet-lob-application-adfs/13-authorize-adfs-error.png)
   
    Az AD FS-kiszolgálón az eseménynaplóban hibára vizsgálja meg, a következő kivétel üzenet jelenik meg:  
   
    <pre class="prettyprint">
    Microsoft.IdentityServer.Web.InvalidRequestException: MSIS7042: <mark>The same client browser session has made '6' requests in the last '11' seconds.</mark> Contact your administrator for details.
       at Microsoft.IdentityServer.Web.Protocols.PassiveProtocolHandler.UpdateLoopDetectionCookie(WrappedHttpListenerContext context)
       at Microsoft.IdentityServer.Web.Protocols.WSFederation.WSFederationProtocolHandler.SendSignInResponse(WSFederationContext context, MSISSignInResponse response)
       at Microsoft.IdentityServer.Web.PassiveProtocolListener.ProcessProtocolRequest(ProtocolContext protocolContext, PassiveProtocolHandler protocolHandler)
       at Microsoft.IdentityServer.Web.PassiveProtocolListener.OnGetContext(WrappedHttpListenerContext context)
    </pre>
   
    Ez a hiba oka, hogy alapértelmezés szerint MVC esetén ad vissza a 401-es nem engedélyezett a felhasználói szerepkörök nem engedélyezettek. Ezzel elindítja az identitásszolgáltató (AD FS) újrahitelesítés kérést. Mivel a felhasználó már hitelesítve van, az AD FS visszatér ugyanazon az oldalon, majd állít ki egy másik 401-es, amely egy átirányítási jöjjön létre. AuthorizeAttribute meg fog felülbírálása `HandleUnauthorizedRequest` egyszerű logikával valami megjelenítendő helyett az átirányítási hurok Folytatás legjobb módszer.
5. A projekt AuthorizeAttribute.cs nevű fájl létrehozása, és az alábbi kód beillesztése.
   
        using System;
        using System.Web.Mvc;
        using System.Web.Routing;
   
        namespace WebApp_WSFederation_DotNet
        {
            [AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, Inherited = true, AllowMultiple = true)]
            public class AuthorizeAttribute : System.Web.Mvc.AuthorizeAttribute
            {
                protected override void HandleUnauthorizedRequest(AuthorizationContext filterContext)
                {
                    if (filterContext.HttpContext.Request.IsAuthenticated)
                    {
                        filterContext.Result = new System.Web.Mvc.HttpStatusCodeResult((int)System.Net.HttpStatusCode.Forbidden);
                    }
                    else
                    {
                        base.HandleUnauthorizedRequest(filterContext);
                    }
                }
            }
        }
   
    A felülbírálás kódot küld egy HTTP 403 (tiltott) helyett HTTP 401-es (nem engedélyezett) hitelesített, de nem engedélyezett esetekben.
6. Futtassa újra a hibakereső `F5`. Kattintson a **forduljon** most egy még informatívabbá (ámbár unattractive) hibaüzenetet jelenít meg:
   
    ![](./media/web-sites-dotnet-lob-application-adfs/14-unauthorized-forbidden.png)
7. Tegye közzé az alkalmazást újra az Azure App Service Web Apps, és az élő alkalmazás működésének teszteléséhez.

<a name="bkmk_data"></a>

## <a name="connect-to-on-premises-data"></a>Csatlakozás helyszíni adatokhoz
Egy, hogy valósítja meg az üzleti alkalmazás Azure Active Directory helyett AD FS-sel történő kívánt oka szervezeti adatok helyi tartásával megfelelőségi problémákat. Ez azt is jelenti, hogy a webalkalmazás az Azure-ban helyszíni adatbázisokat kell hozzáférni, mert a használata nem engedélyezett [SQL-adatbázis](/services/sql-database/) , az adatok rétegként a webes alkalmazásokhoz.

Az Azure App Service Web Apps férnek hozzá a helyszíni adatbázisok esetén két módszer támogatja: [hibrid kapcsolatok](../biztalk-services/integration-hybrid-connection-overview.md) és [virtuális hálózatok](web-sites-integrate-with-vnet.md). További információkért lásd: [használó hálózatok integráció és az Azure App Service Web Apps hibrid kapcsolatok](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/).

<a name="bkmk_resources"></a>

## <a name="further-resources"></a>További erőforrások
* [A helyszíni Active Directoryval az Azure alkalmazás hitelesítéséhez](web-sites-authentication-authorization.md)
* [Üzleti Azure-alkalmazás létrehozása az Azure Active Directory-hitelesítés](web-sites-dotnet-lob-application-azure-ad.md)
* [A Visual Studio 2013 ASP.NET használni a helyszíni szervezeti hitelesítési lehetőséget (AD FS)](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/)
* [VS2013 webes projektet át WIF Katana](http://www.cloudidentity.com/blog/2014/09/15/MIGRATE-A-VS2013-WEB-PROJECT-FROM-WIF-TO-KATANA/)
* [Az Active Directory összevonási szolgáltatások áttekintése](http://technet.microsoft.com/library/hh831502.aspx)
* [WS-Federation 1.1 szabvány](http://download.boulder.ibm.com/ibmdl/pub/software/dw/specs/ws-fed/WS-Federation-V1-1B.pdf?S_TACT=105AGX04&S_CMP=LP)

