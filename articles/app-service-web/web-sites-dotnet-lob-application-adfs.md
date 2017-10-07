---
title: "az AD FS-hitelesítéshez üzleti az Azure alkalmazás aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy üzleti alkalmazást az Azure App Service szolgáltatásban, amely hitelesíti a helyszíni STS. Ez az oktatóanyag az AD FS, a helyszíni STS hello célozza."
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
ms.openlocfilehash: cfd6f14837642fbf58ca5da9dee72db69cd5ab7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-line-of-business-azure-app-with-ad-fs-authentication"></a>Üzleti Azure-alkalmazás létrehozása az AD FS-hitelesítéshez
Ez a cikk bemutatja, hogyan toocreate egy ASP.NET MVC-üzleti alkalmazás [Azure App Service](../app-service/app-service-value-prop-what-is.md) használatával egy helyszíni [Active Directory összevonási szolgáltatások](http://technet.microsoft.com/library/hh831502.aspx) hello identitás-szolgáltatóként. Ebben a forgatókönyvben úgy dolgozhatnak, ha azt szeretné, hogy toocreate-üzletági alkalmazások az Azure App Service szolgáltatásban a szervezet azonban előírja directory adatok toobe helyszínen tárolja.

> [!NOTE]
> Hello különböző vállalati hitelesítési és engedélyezési az Azure App Service-beállítások áttekintéséhez lásd: [hitelesítés a helyszíni Active Directoryval, az Azure alkalmazásban](web-sites-authentication-authorization.md).
> 
> 

<a name="bkmk_build"></a>

## <a name="what-you-will-build"></a>Mit fog létrehozni
A szolgáltatások a következő hello fog létrehozni egy egyszerű ASP.NET-alkalmazás az Azure App Service Web Apps:

* Hitelesíti a felhasználókat az AD FS ellen
* Használja a `[Authorize]` tooauthorize felhasználók a különböző műveletek
* Statikus konfigurálásához a Visual Studio hibakeresési és közzétételi tooApp Service Web Apps (csak egyszer konfigurálnia, hibakeresését és bármikor közzététele)  

<a name="bkmk_need"></a>

## <a name="what-you-need"></a>Mi szükséges
[!INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

Meg kell hello toocomplete követően ez az oktatóanyag:

* Egy helyszíni AD FS üzembe helyezése (ebben az oktatóanyagban használt hello tesztkörnyezet végpont útmutatót lásd: [tesztlabor: az AD FS az Azure virtuális gép (csak tesztelési) önálló STS](https://blogs.msdn.microsoft.com/cephalin/2014/12/21/test-lab-standalone-sts-with-ad-fs-in-azure-vm-for-test-only/))
* Engedélyek toocreate függő entitás megbízhatóságai AD FS felügyelet
* A Visual Studio 2013 Update 4 vagy újabb verzió
* [Az Azure SDK 2.8.1-es verziójának](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) vagy újabb verzió

<a name="bkmk_sample"></a>

## <a name="use-sample-application-for-line-of-business-template"></a>Mintaalkalmazást használ üzleti sablon
Ebben az oktatóanyagban mintaalkalmazás hello [WebApp-WSFederation-DotNet)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet), hello Azure Active Directory csapata hozza létre. Mivel az AD FS támogatja a WS-Federation, használhatja egy sablon toocreate üzleti alkalmazásokként alkalmazásaiba. A következő funkciók hello rendelkezik:

* Használja a [WS-Federation](http://msdn.microsoft.com/library/bb498017.aspx) tooauthenticate egy helyszíni AD FS üzembe helyezése
* Be- és kijelentkezési funkció
* Használja a [Microsoft.Owin](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana) (nem Windows Identity Foundation), amely van hello jövőbeli az ASP.NET és a hitelesítési és engedélyezési feliratkozott jóval egyszerűbb tooset WIF-nál

<a name="bkmk_setup"></a>

## <a name="set-up-hello-sample-application"></a>Hello mintaalkalmazás beállítása
1. Klónozza, vagy töltse le a hello minta megoldást a következő [WebApp-WSFederation-DotNet](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) tooyour helyi könyvtárba.
   
   > [!NOTE]
   > útmutatás hello [README.md](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet/blob/master/README.md) mutatja be tooset mentése hello alkalmazás Azure Active Directoryval. Azonban ebben az oktatóanyagban, állítsa be az AD FS-sel, ezért a lépések hello itt helyette.
   > 
   > 
2. Nyissa meg a hello megoldás, és nyissa meg a hello Controllers\AccountController.cs **Megoldáskezelőben**.
   
   Látni fogja, hogy egyszerűen hello kódot állít ki egy hitelesítési kérdés tooauthenticate hello felhasználó WS-Federation használatával. Minden hitelesítési App_Start\Startup.Auth.cs van konfigurálva.
3. Nyissa meg a App_Start\Startup.Auth.cs. A hello `ConfigureAuth` metódust, Megjegyzés hello sor:
   
       app.UseWsFederationAuthentication(
           new WsFederationAuthenticationOptions
           {
               Wtrealm = realm,
               MetadataAddress = metadata                                      
           });
   
   Hello OWIN világában a részlet valóban operációs rendszer minimálisan szükséges tooconfigure WS-Federation hitelesítési hello. Már jóval egyszerűbb, több elegáns, mint a WIF, ahol Web.config van beszúrta XML hello hely számos országában dolgoznak. csak a szükséges információkat hello megbízó fél (RP) hello azonosítója és hello URL-címe az AD FS szolgáltatást metaadatait tartalmazó fájl. Íme egy példa:
   
   * Függő Entitás azonosítója:`https://contoso.com/MyLOBApp`
   * Metaadatok címe:`http://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`
4. App_Start\Startup.Auth.cs módosítsa a következő statikus karakterlánc-definíciók hello:  
   
   <pre class="prettyprint">
   private static string realm = ConfigurationManager.AppSettings["ida:<mark>RPIdentifier</mark>"];
   <mark><del>private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];</del></mark>
   <mark><del>private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];</del></mark>
   <mark><del>private static string metadata = string.Format("{0}/{1}/federationmetadata/2007-06/federationmetadata.xml", aadInstance, tenant);</del></mark>
   <mark>private static string metadata = string.Format("https://{0}/federationmetadata/2007-06/federationmetadata.xml", ConfigurationManager.AppSettings["ida:ADFS"]);</mark>
   
   <mark><del>string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);</del></mark>
   </pre>
5. Most módosításokat hello megfelelő a Web.config fájlban. Nyissa meg a hello Web.config, és módosítsa a következő beállítások hello:  
   
   <pre class="prettyprint">
   &lt;appSettings&gt;
   &lt;add key="webpages:Version" value="3.0.0.0" /&gt;
   &lt;add key="webpages:Enabled" value="false" /&gt;
   &lt;add key="ClientValidationEnabled" value="true" /&gt;
   &lt;add key="UnobtrusiveJavaScriptEnabled" value="true" /&gt;
   <mark><del>&lt;add key="ida:Wtrealm" value="[Enter hello App ID URI of WebApp-WSFederation-DotNet https://contoso.onmicrosoft.com/WebApp-WSFederation-DotNet]" /&gt;</del></mark>
   <mark><del>&lt;add key="ida:AADInstance" value="https://login.microsoftonline.com" /&gt;</del></mark>
   <mark><del>&lt;add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" /&gt;</del></mark>
   <mark>&lt;add key="ida:RPIdentifier" value="[Enter hello relying party identifier as configured in AD FS, e.g. https://localhost:44320/]" /&gt;</mark>
   <mark>&lt;add key="ida:ADFS" value="[Enter hello FQDN of AD FS service, e.g. adfs.contoso.com]" /&gt;</mark>
   
   &lt;/appSettings&gt;
   </pre>
   
   Töltse ki a megfelelő környezet alapján hello kulcsértékei.
6. Build hello alkalmazás toomake meg arról, hogy nincsenek hibák.

Ennyi az egész. Hello mintaalkalmazást most már készen áll a toowork az AD FS áll. Szüksége van egy függő Entitás megbízhatósági kapcsolatban áll az alkalmazás később az AD FS tooconfigure.

<a name="bkmk_deploy"></a>

## <a name="deploy-hello-sample-application-tooazure-app-service-web-apps"></a>Hello minta alkalmazás tooAzure App Service Web Apps telepítése
Itt közzéteszi hello alkalmazás tooa webalkalmazást az App Service Web Apps hello hibakeresési környezetben adatainak megőrzése mellett. Vegye figyelembe, hogy fog toopublish hello alkalmazás rendelkezik egy függő Entitás megbízhatóság az AD FS-sel, így a hitelesítés továbbra sem működik még előtt. Azonban ha így tesz, akkor most lehet hello webes URL-címet, hogy tooconfigure hello RP megbízhatósági később használhatja.

1. Kattintson jobb gombbal a projektre, és válassza ki **közzététel**.
   
    ![](./media/web-sites-dotnet-lob-application-adfs/01-publish-website.png)
2. Válassza ki **Microsoft Azure App Service**.
3. Ha még nem jelentkezett tooAzure, kattintson a **bejelentkezés** és az Azure-előfizetés toosign a hello Microsoft-fiókot használja.
4. Miután bejelentkezett, kattintson a **új** toocreate egy webalkalmazást.
5. Töltse ki az összes kötelező mezőt. Tooconnect tooon helyszíni adatokhoz készül később, ezért ne hozzon létre egy adatbázist a webalkalmazás.
   
    ![](./media/web-sites-dotnet-lob-application-adfs/02-create-website.png)
6. Kattintson a **Create** (Létrehozás) gombra. Hello webalkalmazás létrehozása után hello webhely közzététele párbeszédpanelen van megnyitva.
7. A **URL-címre**, módosítsa **http** túl**https**. Másolja a hello teljes URL-cím tooa szövegszerkesztőben későbbi használatra. Kattintson a **közzététel**.
   
    ![](./media/web-sites-dotnet-lob-application-adfs/03-destination-url.png)
8. A Visual Studióban nyissa meg a **Web.Release.config** a projektben. Helyezze be a hello XML hello be a következő `<configuration>` címkét, és hello kulcsérték cserélje le a közzétételi webes alkalmazás URL-CÍMÉT.  
   
   <pre class="prettyprint">
   &lt;appSettings&gt;
   &lt;add key="ida:RPIdentifier" value="<mark>[e.g. https://mylobapp.azurewebsites.net/]</mark>" xdt:Transform="SetAttributes" xdt:Locator="Match(key)" /&gt;
   &lt;/appSettings&gt;</pre>

Amikor elkészült, a projekt egy Visual Studio, a hibakeresési környezetben a konfigurált két RP azonosítók rendelkezik, és egy hello a közzétett webalkalmazás az Azure-ban. Az egyes hello két környezetet az AD FS fog beállítani egy függő Entitás megbízhatósági. Hibakeresési, hello Alkalmazásbeállítások a Web.config fájlban használt toomake a **Debug** az AD FS konfigurációs munka. Ha közzé van téve (alapértelmezés szerint hello **kiadás** konfigurációs van közzétéve), átalakított Web.config tölt fel, amely magában foglalja a hello app beállítás változása Web.Release.config.

Ha a kívánt tooattach hello a közzétett webes alkalmazás Azure toohello hibakereső (vagyis fel kell töltenie a kód hello a közzétett webes alkalmazás hibakeresési szimbólumok), akkor is klónozhatja hello hibakeresése az Azure-hibakeresés konfigurációs, de a saját egyéni Web.config átalakítás) pl. Web.AzureDebug.config) használó Web.Release.config hello alkalmazás beállításait. Ez lehetővé teszi toomaintain statikus hello különböző környezetek között.

<a name="bkmk_rptrusts"></a>

## <a name="configure-relying-party-trusts-in-ad-fs-management"></a>Megbízható függő entitások AD FS felügyelet konfigurálása
A továbbiakban létre kell egy függő Entitás megbízhatóságának AD FS felügyeleti tooconfigure előtt használja a mintaalkalmazást, és az AD FS ténylegesen hitelesítéséhez. Szüksége lesz a tooset két külön RP bizalmi kapcsolatok mentése, egy, a hibakeresés környezethez és egy közzétett webalkalmazáshoz.

> [!NOTE]
> Győződjön meg arról, hogy mind a környezetek lépések hello ismételje.
> 
> 

1. Az AD FS-kiszolgálón jelentkezzen be a hitelesítő adatokat, amelyek felügyeleti jogok tooAD FS.
2. Nyissa meg az AD FS felügyeleti konzolt. Kattintson a jobb gombbal **AD FS\Trusted Relationships\Relying entitások Megbízhatóságai** válassza **függő entitás megbízhatóságának hozzáadása**.
   
   ![](./media/web-sites-dotnet-lob-application-adfs/1-add-rptrust.png)
3. A hello **adatforrás kiválasztása** lapon, hogy melyik **hello függő entitással kapcsolatos adatok manuális megadása**. 
   
   ![](./media/web-sites-dotnet-lob-application-adfs/2-enter-rp-manually.png)
4. A hello **megjelenítendő név megadása** lapon adjon meg egy megjelenítési nevet hello alkalmazáshoz, és kattintson **következő**.
5. A hello **válasszon protokoll** kattintson **következő**.
6. A hello **tanúsítvány konfigurálása** kattintson **következő**.
   
   > [!NOTE]
   > Mivel kell használni HTTPS már, titkosított jogkivonatok nem kötelező. Ha valóban tooencrypt jogkivonatok az AD FS ezen a lapon, jogkivonat-visszafejtési logikát is hozzá kell adnia a kódot. További információkért lásd: [manuálisan titkosítja az OWIN WS-Federation köztes konfigurálása és elfogadása jogkivonatok](http://chris.59north.com/post/2014/08/21/Manually-configuring-OWIN-WS-Federation-middleware-and-accepting-encrypted-tokens.aspx).
   > 
   > 
7. Mielőtt alakzatot hello következő lépésre viszi, a Visual Studio-projektet a kell információ. Hello projekt tulajdonságai, vegye figyelembe a hello **SSL URL-cím** hello alkalmazás. 
   
   ![](./media/web-sites-dotnet-lob-application-adfs/3-ssl-url.png)
8. Vissza az AD FS-kezelés hello **URL konfigurálása** hello oldalán **függő entitás megbízhatóságának hozzáadása varázsló**, jelölje be **hello WS-Federation passzív protokoll támogatásának engedélyezése a** és Írja be a hello SSL URL-címet, a Visual Studio-projekt, hogy hello előző lépésben feljegyzett. Ezután kattintson a **Tovább** gombra.
   
   ![](./media/web-sites-dotnet-lob-application-adfs/4-configure-url.png)
   
   > [!NOTE]
   > URL-CÍMÉT határozza meg, ahol toosend hello ügyfél hitelesítést követően sikeres lesz. Hello hibakeresés környezethez kell <code>https://localhost:&lt;port&gt;/</code>. Hello közzétett webalkalmazás hello webes alkalmazás URL-CÍMÉT kell lennie.
   > 
   > 
9. A hello **azonosítók konfigurálása** lapon ellenőrizze, hogy a projekt SSL URL-cím már szerepel-e, majd kattintson **következő**. Kattintson a **következő** összes hello módon toohello vége hello varázsló az alapértelmezett beállításokat.
   
   > [!NOTE]
   > Ez az azonosító App_Start\Startup.Auth.cs a Visual Studio-projekt, a elleni hello értéke egyezik <code>WsFederationAuthenticationOptions.Wtrealm</code> összevont hitelesítés során. Alapértelmezés szerint hello előző lépésben hello alkalmazás URL-címe meg van adva egy függő Entitás azonosítója.
   > 
   > 
10. A projekthez RP alkalmazás hello beállítása az AD FS most után. Ezután állítsa be az alkalmazás toosend hello az alkalmazás által igényelt jogcímeket. Hello **Jogcímszabályok szerkesztése** párbeszédpanelen alapértelmezés szerint megnyílik az Ön hello varázsló hello végén, és azonnal megkezdje. Pedig konfiguráljuk legalább hello követően a jogcímeket (zárójelben sémák):
    
    * ASP.NET toohydrate által használt név (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) - `User.Identity.Name`.
    * Felhasználó egyszerű felhasználónév (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn) - használt toouniquely hello szervezethez tartozó felhasználók azonosításához.
    * A csoporttagságokat, szerepkörök (http://schemas.microsoft.com/ws/2008/06/identity/claims/role) - használható `[Authorize(Roles="role1, role2,...")]` decoration tooauthorize tartományvezérlők/művelet. A valóságban ez a megközelítés lehet, hogy nem kell hello legtöbb performant a szerepkör-hitelesítéshez. Ha az AD-felhasználók biztonsági csoportjainak toohundreds tartozik, több száz szerepkör jogcím szerepel hello SAML-jogkivonat válnak. Egy másik módszert toosend egyetlen szerepkör jogcím feltételesen attól függően, hogy egy adott csoport hello felhasználó tagja. Azonban szavatoljuk az egyszerű ehhez az oktatóanyaghoz.
    * Azonosító (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier) Name,-hamisítás érvényesítéshez használható. A toomake azt működése hamisítás érvényesítéssel további információkért lásd: hello **üzleti funkciókkal** szakasza [üzleti Azure-alkalmazás létrehozása az Azure Active Directory-hitelesítés ](web-sites-dotnet-lob-application-azure-ad.md#bkmk_crud).
    
    > [!NOTE]
    > hello jogcím típusokat, tooconfigure szükséges az alkalmazás határozza meg az alkalmazás igényeinek. Azure Active Directory-alkalmazások (azaz a függő Entitás megbízhatósági kapcsolatok) által támogatott jogcímek hello listája, például: [támogatott jogkivonat és jogcímtípusok](http://msdn.microsoft.com/library/azure/dn195587.aspx).
    > 
    > 
11. Hello Jogcímszabályok szerkesztése párbeszédpanelen kattintson a **szabály hozzáadása**.
12. Hello nevét, az egyszerű felhasználónév és a szerepkör jogcím konfigurálása hello képernyőfelvételen látható módon, és kattintson a **Befejezés**.
    
    ![](./media/web-sites-dotnet-lob-application-adfs/5-ldap-claims.png)
    
    Ezután hozzon létre egy átmeneti nevet azonosító jogcím használatával hello lépéseket mutatja be [azonosítókat a SAML helyességi feltételek](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).
13. Kattintson a **szabály hozzáadása** újra.
14. Válassza ki **jogcímek küldése egyéni szabály segítségével** kattintson **következő**.
15. Beillesztés hello következő nyelvi szabály be hello **egyéni szabály** mezőbe, a név hello szabály **munkamenet azonosítója** kattintson **Befejezés**.  
    
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
18. Hello szabály konfigurálása (használatával létrehozott egyéni szabály hello hello jogcímtípus) hello képernyőfelvételen látható módon, és kattintson a **Befejezés**.
    
    ![](./media/web-sites-dotnet-lob-application-adfs/7-transient-name-id.png)
    
    A hello lépésekre hello átmeneti Névazonosító jogcímet részletes információkért lásd: [azonosítókat a SAML helyességi feltételek](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).
19. Kattintson a **alkalmaz** a hello **Jogcímszabályok szerkesztése** párbeszédpanel. Mostantól a következő képernyőkép hello hasonlóan kell kinéznie:
    
    ![](./media/web-sites-dotnet-lob-application-adfs/8-all-claim-rules.png)
    
    > [!NOTE]
    > Ebben az esetben győződjön meg arról, hogy a hibakeresési környezetben és a közzétett webes alkalmazás ismételje meg ezeket a lépéseket.
    > 
    > 

<a name="bkmk_test"></a>

## <a name="test-federated-authentication-for-your-application"></a>Az alkalmazás összevont hitelesítés tesztelése
Akkor van kész tootest az alkalmazás hitelesítési logikát szemben az AD FS. Az AD FS laborkörnyezetben tooa teszt csoportot az Active Directory (AD) tartozó tesztfelhasználó van.

![](./media/web-sites-dotnet-lob-application-adfs/10-test-user-and-group.png)

hello hibakeresőben, most szüksége toodo tootest hitelesítési típus `F5`. Ha azt szeretné, hogy a közzétett webalkalmazás hello tootest hitelesítési, keresse meg a toohello URL-CÍMÉT.

Hello webalkalmazás betöltése után kattintson **bejelentkezés**. Most vagy egy bejelentkezési párbeszédpanel vagy hello bejelentkezési oldalt az AD FS által megadott hello hitelesítési módszertől függően az AD FS által kiszolgált szerezheti be. Ez mit jelenik meg az Internet Explorer 11.

![](./media/web-sites-dotnet-lob-application-adfs/9-test-debugging.png)

Hello AD-tartomány hello AD FS központi telepítés a felhasználó jelentkezik, ha meg kell jelennie hello kezdőlap újra **Hello, <User Name>!** hello sarokban. Ez mit jelenik meg.

![](./media/web-sites-dotnet-lob-application-adfs/11-test-debugging-success.png)

Az eddigi sikeresen hello a következő módon:

* Az alkalmazás sikeresen elérte az AD FS és a megfelelő függő Entitás azonosítója hello AD FS adatbázisában található
* AD FS sikeresen hitelesítette egy AD-felhasználó és az átirányítási biztonsági toohello alkalmazás kezdőlap
* Az AD FS elküldése sikerült hello neveként (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) tooyour alkalmazás, jogcím-hello tény jelöli, hogy hello a felhasználó neve hello sarokban jelenik meg. 

Ha hello név jogcímet hiányzik, akkor amint láthatta **Hello,!**. Ha megnézzük Views\Shared\_LoginPartial.cshtml, találja, hogy használja `User.Identity.Name` toodisplay hello felhasználónevet. Ahogy korábban említettük, ha a felhasználó SAML-jogkivonat hello érhető el a hitelesített hello hello neve jogcím ASP.NET hydrates Ez a tulajdonság azt. a jogcímek összes hello toosee indexhez tartozó műveleti módszer küldött Controllers\HomeController.cs, a hello állíthasson be az AD FS által. Hello felhasználó hitelesítése után vizsgálja meg a hello `System.Security.Claims.Current.Claims` gyűjtemény.

![](./media/web-sites-dotnet-lob-application-adfs/12-test-debugging-all-claims.png) 

<a name="bkmk_authorize"></a>

## <a name="authorize-users-for-specific-controllers-or-actions"></a>Adott vezérlőket vagy a műveletek a felhasználók hitelesítéséhez
Csoporttagságok szerepkör jogcímekként bevont a függő Entitás szintű bizalmi kapcsolatainak konfigurációja, mivel most már használhatja azokat közvetlenül a hello `[Authorize(Roles="...")]` decoration tartományvezérlőket és a műveletek. Hello létrehozás-olvasás-Módosítás-Törlés (CRUD) mintával üzleti alkalmazásokban engedélyezheti egyes szerepkörök tooaccess egyes műveletek során. Most meg fog csak próbálja ki ezt a funkciót hello meglévő otthoni vezérlő.

1. Nyissa meg a Controllers\HomeController.cs.
2. Adja a hello `About` és `Contact` művelet módszerek hasonló toohello a következő kódot, biztonsági csoporttagság, amely a hitelesített felhasználó rendelkezik-e használatával.  
   
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
   
    Mivel a hozzáadott **tesztfelhasználó** túl**Tesztcsoport** a az AD FS laborkörnyezetben, a teszt csoportot tootest engedélyezési fogok használni `About`. A `Contact`, hello negatív gyorsítás esetében is tesztelni fogja **Tartománygazdák**, toowhich **tesztelése felhasználói** nem tartozik.
3. Indítsa el a hello hibakereső beírásával `F5` jelentkezzen be, majd kattintson a **kapcsolatos**. Meg kell most tekintjük hello `~/About/Index` lapon sikeres legyen, ha a hitelesített felhasználó jogosult-e az adott művelethez.
4. Most kattintson **forduljon**, ami abban az esetben, ha nem engedélyezhetik **tesztfelhasználó** hello a művelethez. Hello böngésző azonban átirányított tooAD FS, ami végül jeleníti meg ezt az üzenetet:
   
    ![](./media/web-sites-dotnet-lob-application-adfs/13-authorize-adfs-error.png)
   
    Ez a hiba az eseménynaplóban az AD FS-kiszolgáló hello vizsgálja meg, a következő kivétel üzenet jelenik meg:  
   
    <pre class="prettyprint">
    Microsoft.IdentityServer.Web.InvalidRequestException: MSIS7042: <mark>hello same client browser session has made '6' requests in hello last '11' seconds.</mark> Contact your administrator for details.
       at Microsoft.IdentityServer.Web.Protocols.PassiveProtocolHandler.UpdateLoopDetectionCookie(WrappedHttpListenerContext context)
       at Microsoft.IdentityServer.Web.Protocols.WSFederation.WSFederationProtocolHandler.SendSignInResponse(WSFederationContext context, MSISSignInResponse response)
       at Microsoft.IdentityServer.Web.PassiveProtocolListener.ProcessProtocolRequest(ProtocolContext protocolContext, PassiveProtocolHandler protocolHandler)
       at Microsoft.IdentityServer.Web.PassiveProtocolListener.OnGetContext(WrappedHttpListenerContext context)
    </pre>
   
    hello Ez a hiba oka, hogy alapértelmezés szerint MVC esetén ad vissza a 401-es nem engedélyezett a felhasználói szerepkörök nem engedélyezettek. Ezzel elindítja a újrahitelesítés kérelem tooyour identitásszolgáltató (AD FS). Hello felhasználó már hitelesítve van, mivel az AD FS adja vissza toohello ugyanazon az oldalon, amely majd kiadja a másik 401, egy átirányítási jöjjön létre. AuthorizeAttribute meg fog felülbírálása `HandleUnauthorizedRequest` egyszerű logikai tooshow metódust használ, amelyet érdemes folyamatos hello átirányítási hurok helyett.
5. Hozzon létre egy fájlt AuthorizeAttribute.cs, és a Beillesztés hello kód bele a következő nevű hello projektben.
   
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
   
    hello bírálja felül a kód elküldi egy HTTP 403 (tiltott) helyett HTTP 401-es (nem engedélyezett) azokban az esetekben, hitelesített, de nem engedélyezett.
6. Futtassa újra hello hibakereső `F5`. Kattintson a **forduljon** most egy még informatívabbá (ámbár unattractive) hibaüzenetet jelenít meg:
   
    ![](./media/web-sites-dotnet-lob-application-adfs/14-unauthorized-forbidden.png)
7. Tegye közzé újra hello alkalmazás tooAzure App Service Web Apps, és tesztelje a hello élő alkalmazás hello jellemzőit.

<a name="bkmk_data"></a>

## <a name="connect-tooon-premises-data"></a>Csatlakozás tooon helyszíni adatokhoz
Miért érdemes tooimplement az üzleti alkalmazás az Azure Active Directory helyett AD FS fogja szervezeti adatok helyi tartásával megfelelőségi problémák. Ez azt is jelenti, hogy a webalkalmazás az Azure-ban helyszíni adatbázisokat kell hozzáférni, mert nem szerepelhet toouse [SQL-adatbázis](/services/sql-database/) , hello adatok rétegként a webes alkalmazásokhoz.

Az Azure App Service Web Apps férnek hozzá a helyszíni adatbázisok esetén két módszer támogatja: [hibrid kapcsolatok](../biztalk-services/integration-hybrid-connection-overview.md) és [virtuális hálózatok](web-sites-integrate-with-vnet.md). További információkért lásd: [használó hálózatok integráció és az Azure App Service Web Apps hibrid kapcsolatok](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/).

<a name="bkmk_resources"></a>

## <a name="further-resources"></a>További erőforrások
* [A helyszíni Active Directoryval az Azure alkalmazás hitelesítéséhez](web-sites-authentication-authorization.md)
* [Üzleti Azure-alkalmazás létrehozása az Azure Active Directory-hitelesítés](web-sites-dotnet-lob-application-azure-ad.md)
* [A helyszíni szervezeti hitelesítés lehetőséget (AD FS) az ASP.NET hello használja a Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/)
* [A webes projektet a WIF VS2013 tooKatana áttelepítése](http://www.cloudidentity.com/blog/2014/09/15/MIGRATE-A-VS2013-WEB-PROJECT-FROM-WIF-TO-KATANA/)
* [Az Active Directory összevonási szolgáltatások áttekintése](http://technet.microsoft.com/library/hh831502.aspx)
* [WS-Federation 1.1 szabvány](http://download.boulder.ibm.com/ibmdl/pub/software/dw/specs/ws-fed/WS-Federation-V1-1B.pdf?S_TACT=105AGX04&S_CMP=LP)

