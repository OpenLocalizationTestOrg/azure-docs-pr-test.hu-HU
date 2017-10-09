---
title: "Tartományok közötti identitáskezeléshez rendszer aaaUsing automatikusan létesítsen felhasználók és csoportok az Azure Active Directory tooapplications |} Microsoft Docs"
description: "Az Azure Active Directory-felhasználók és csoportok tooany alkalmazás vagy identitás tároló hello felületen meghatározott hello SCIM protokoll-meghatározása a webszolgáltatás által az fronted automatikusan telepíthetik"
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: femila
editor: 
ms.assetid: 4d86f3dc-e2d3-4bde-81a3-4a0e092551c0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.custom: aaddev;it-pro;oldportal
ms.openlocfilehash: 43045c97e68d0d22db598dcb5ec23481c4e97718
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-system-for-cross-domain-identity-management-tooautomatically-provision-users-and-groups-from-azure-active-directory-tooapplications"></a>Tartományok közötti Identity Management tooautomatically rendelkezés felhasználókhoz és csoportokhoz az Azure Active Directory tooapplications használatával

## <a name="overview"></a>Áttekintés
Azure Active Directory (Azure AD) automatikusan telepíthetik a felhasználók és csoportok tooany alkalmazás vagy identitás tároló, amely a webszolgáltatás által meghatározott hello hello felülettel fronted van [rendszer tartományok közötti Identity Management (SCIM) 2.0 protokoll specifikációja](https://tools.ietf.org/html/draft-ietf-scim-api-19). Az Azure Active Directory küldési kérelmek toocreate, módosítása vagy törlése a kijelölt felhasználók és csoportok toohello webes szolgáltatás. hello webszolgáltatás majd is ezeket a kérelmeket, lefordítása hello cél identitás tárolására műveleteket. 

> [!IMPORTANT]
> A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon. 



![][0]
*1. ábra: Azure Active Directory tooan identitás tárolására a webszolgáltatáson keresztül a kiépítés*

Ez a funkció hello "állapotba hozása a saját alkalmazás" lehetőséget az Azure AD tooenable egyszeri bejelentkezést és automatikus kiépítést alkalmazásokat, amelyek adja meg, vagy amelyek fronted SCIM webszolgáltatás által a felhasználói együtt is használható.

Számos két alkalmazási helyzetei SCIM használata az Azure Active Directoryban.

* **Felhasználók és csoportok tooapplications SCIM támogató kiépítés** alkalmazásokat, amelyek támogatják az SCIM 2.0 és OAuth tulajdonosi jogkivonatok használnak, a hitelesítés az Azure AD konfigurálása nélkül működik.
* **Saját kiépítési megoldás az alkalmazások, amelyek támogatják a többi API-alapú üzembe helyezés** nem SCIM alkalmazások, létrehozhat egy SCIM végpont tootranslate hello Azure AD SCIM végpont, valamint esetleges API hello alkalmazás között a felhasználók átadása. toohelp egy SCIM végpont fejleszt, nyújtunk a közös nyelvi infrastruktúra (CLI) szalagtárak mintakódok, amelyek bemutatják, hogyan toodo adjon meg egy SCIM végpontot, és SCIM üzenetek fordítása együtt.  

## <a name="provisioning-users-and-groups-tooapplications-that-support-scim"></a>Felhasználók és csoportok tooapplications SCIM támogató kiépítése
Az Azure AD lehet konfigurált tooautomatically rendelkezés hozzárendelt felhasználók és csoportok tooapplications, amelyek megvalósítják a [tartományok közötti identitáskezeléshez 2 (SCIM) rendszer](https://tools.ietf.org/html/draft-ietf-scim-api-19) webes szolgáltatás, és fogadja el az OAuth tulajdonosi jogkivonatokat a hitelesítéshez . Belül hello SCIM 2.0 specifikációját alkalmazások követelménynek kell megfelelnie:

* Támogatja az felhasználói és/vagy csoportok szerint hello SCIM protokoll 3.3 szakasza létrehozását.  
* Támogatja a felhasználók és/vagy csoportok szerint hello SCIM protokoll 3.5.2 szakasza a patch kéréseknek módosítása.  
* Támogatja az adott hello SCIM protokoll 3.4.1 szakasza ismert erőforrás beolvasása.  
* Támogatja a felhasználók és/vagy csoportok szerint hello SCIM protokoll 3.4.2 szakasza lekérdezése.  Alapértelmezés szerint externalId rendszer megkérdezi a felhasználókat és csoportokat a rendszer megkérdezi a displayName által.  
* Felhasználói azonosító és a kezelő szakaszában 3.4.2 hello SCIM protokoll szerinti lekérdezése támogatja.  
* Azonosító és a tag hello SCIM protokoll 3.4.2 szakaszában szereplő csoportok lekérdezése támogatja.  
* Hello SCIM protokoll szerinti szakasz 2.1 engedélyezését OAuth tulajdonosi jogkivonatokat fogad el.

Kérje meg az alkalmazás-szolgáltatót, és ezeket a követelményeket is kompatibilisek állapotkimutatások az alkalmazás szolgáltató dokumentációját.

### <a name="getting-started"></a>Bevezetés
Ebben a cikkben leírt hello SCIM profil támogató alkalmazások lehet csatlakoztatott tooAzure Active Directory hello Azure AD application gallery a "nem galéria alkalmazás" hello funkciójával. Csatlakoztatott, az Azure AD futtatása után 20 percenként ahol hello alkalmazás SCIM végpontja lekéri a szinkronizálási folyamat hozzárendelt felhasználókat és csoportokat, és hoz létre, vagy módosítja őket szerint toohello hozzárendelés részletei.

**SCIM támogató alkalmazás tooconnect:**

1. Jelentkezzen be a túl[hello Azure-portálon](https://portal.azure.com). 
2. Keresse meg a túl ** Azure Active Directory > Vállalati alkalmazások, és válassza ki **új alkalmazás > minden > nem-gyűjtemény alkalmazás**.
3. Adja meg az alkalmazás nevét, és kattintson a **Hozzáadás** ikon toocreate app objektum.
    
  ![][1]
  *2. ábra: Az Azure AD application gallery*
    
4. Az eredményül kapott üdvözlő képernyőt, válassza ki a hello **kiépítési** lapon hello bal oldali oszlopban.
5. A hello **kiépítési üzemmódban** menü **automatikus**.
    
  ![][2]
  *3. ábra: Konfigurálása kiosztás a hello Azure-portálon*
    
6. A hello **bérlői URL-cím** mezőbe írja be a hello hello alkalmazás SCIM végpont URL-CÍMÉT. Példa: https://api.contoso.com/scim/v2/
7. Ha hello SCIM végpont megköveteli az OAuth tulajdonosi jogkivonat nem az Azure AD egy kiállítótól érkező, akkor a Másolás hello szükséges OAuth tulajdonosi jogkivonattal történő választható hello **titkos Token** mező. Ez a mező üresen marad, ha az Azure AD az Azure ad-minden egyes kérelemmel kiadott OAuth tulajdonosi jogkivonat tartalmazza. Alkalmazások, az Azure AD használja az identitásszolgáltató azt is ellenőrzi az Azure AD-jogkivonatot ki.
8. Kattintson a hello **kapcsolat tesztelése** gomb toohave Azure Active Directory kísérlet tooconnect toohello SCIM végpont. Ha hello kísérlet sikertelen, hiba információk jelennek meg.  
9. Ha hello kísérletek tooconnect toohello alkalmazás sikeres legyen, majd kattintson **mentése** toosave hello rendszergazdai hitelesítő adataival.
10. A hello **hozzárendelések** szakaszban, a két választható csoport attribútum-leképezésekhez: egy felhasználói objektum, egy, a csoport objektumainak. Minden egyes egy tooreview hello attribútumokat szinkronizált Azure Active Directory tooyour app választja ki. kiválasztott attribútumok hello **egyező** tulajdonságainak használt toomatch hello felhasználókat és csoportokat a frissítési műveletek az alkalmazásban. Válassza ki a hello Mentés gombra toocommit módosításokat.

    >[!NOTE]
    >Igény szerint letilthatja hello "csoportok" leképezési letiltásával objektumok szinkronizálása. 

11. A **beállítások**, hello **hatókör** mező határozza meg, hogy mely felhasználók és az or csoportok szinkronizálva. Válassza a "Sync csak hozzárendelt felhasználók és csoportok" (ajánlott) csak szinkronizálva lesz a hello rendelt felhasználók és csoportok **felhasználók és csoportok** fülre.
12. A konfiguráció befejezése után módosítsa a hello **kiépítési állapot** túl**a**.
13. Kattintson a **mentése** toostart hello Azure AD létesítési szolgáltatás. 
14. Ha csak szinkronizálás hozzárendelve felhasználók és csoportok (ajánlott), lehet, hogy tooselect hello **felhasználók és csoportok** lapra, és rendeljen hello felhasználók és/vagy csoportok toosync kívánja.

Miután hello kezdeti szinkronizálás elindult, használhatja a hello **naplók** lapon toomonitor folyamatban, amely mutatja az alkalmazás a szolgáltatás kiépítését hello által végzett összes műveletet. Hogyan tooread hello Azure AD-kiépítés naplózza a további információkért lásd: [automatikus felhasználói fiók kiépítése jelentések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).

>[!NOTE]
>hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe. 


## <a name="building-your-own-provisioning-solution-for-any-application"></a>Bármely alkalmazás a saját kiépítési megoldás létrehozása
Hozzon létre egy SCIM webszolgáltatás-bővítmény is eltárolni, amely az Azure Active Directoryval, engedélyezheti a egyetlen bejelentkezést és automatikus felhasználólétesítés szinte bármilyen alkalmazáshoz, amely a kiépítés API REST vagy SOAP felhasználó biztosít.

Itt látható, hogyan működik:

1. Az Azure AD biztosít a közös nyelvi infrastruktúra szalagtár nevű [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/). Rendszerintegrátorok és a fejlesztők a szalagtár toocreate használja, és csatlakozni tudnak az Azure AD tooany alkalmazás identitás tárolására SCIM-alapú webes szolgáltatásvégpont telepítése.
2. Hozzárendelések hello web service toomap hello szabványos felhasználói séma toohello felhasználói séma és a protokoll hello alkalmazáshoz szükséges valósíthatók meg.
3. hello végponti URL-cím regisztrálva van az Azure AD egy egyéni alkalmazást a hello alkalmazáskatalógusában részeként.
4. Felhasználók és csoportok vannak hozzárendelve toothis alkalmazás az Azure ad-ben. Hozzárendelés, hogy a várólista szinkronizált toobe toohello cél alkalmazásba kerülnek. hello szinkronizálási folyamat hello várólista kezelése 20 percenként fut.

### <a name="code-samples"></a>Kódminták
toomake ez feldolgozása könnyebb, [Kódminták](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) feltéve, hogy hozzon létre egy SCIM webszolgáltatási végpontot, és mutassa be, az Automatikus kiépítés. Egy minta megtartja sorait a CSV-felhasználók és csoportok közti szolgáltatóra van.  más hello hello Amazon Web Services identitás és hozzáférés-kezelés szolgáltatás működik szolgáltatóra van.  

**Előfeltételek**

* A Visual Studio 2013 vagy újabb verzió
* [Azure SDK for .NET](https://azure.microsoft.com/downloads/)
* Windows-számítógép, amely támogatja az ASP.NET-keretrendszer 4.5-ös toobe hello használt hello SCIM végpont. Ezen a számítógépen a hello felhőből elérhetőnek kell lennie.
* [Prémium szintű Azure AD egy próba- vagy licencelt verziójával egy Azure-előfizetés](https://azure.microsoft.com/services/active-directory/)
* hello Amazon AWS igényel könyvtárakat a hello [AWS eszközkészlet a Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html). További információkért lásd: hello információs fájl hello minta.

### <a name="getting-started"></a>Első lépések
hello egy SCIM kiépítés elfogadó kérelmeket az Azure AD legegyszerűbb módja tooimplement toobuild, telepítse a hello kódminta, amely hello kiosztott felhasználók tooa vesszővel tagolt (CSV) fájl.

**a minta SCIM végpont toocreate:**

1. Hello kód a minta-csomag letöltése [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)
2. Bontsa ki a hello csomagot, és helyezze el a Windows-számítógép C:\AzureAD-BYOA-Provisioning-Samples\ például egy helyen.
3. Ebben a mappában nyissa meg a Visual Studio hello FileProvisioningAgent megoldás.
4. Válassza ki **eszközök > Kódtárcsomag-kezelő > Csomagkezelő konzol**, és hajtsa végre a következő parancsok hello FileProvisioningAgent projekt tooresolve hello megoldás hivatkozásainak hello:
  ```` 
   Install-Package Microsoft.SystemForCrossDomainIdentityManagement
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   Install-Package Microsoft.Owin.Diagnostics
   Install-Package Microsoft.Owin.Host.SystemWeb
  ````
5. Hello FileProvisioningAgent projekt felépítéséhez.
6. Indítsa el a (rendszergazdaként) a Windows hello parancssori alkalmazás, és a hello **cd** parancs toochange hello directory tooyour **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** mappa.
7. Futtassa a következő parancsot, a < ip-cím > lecserélését hello IP-cím vagy tartománynév kiszolgálónevét hello Windows gép hello:
  ````   
   FileAgnt.exe http://<ip-address>:9000 TargetFile.csv
  ````
8. A Windows **Windows-beállítások > hálózat és Internet beállítások**, jelölje be hello **Windows tűzfal > Speciális beállítások**, és hozzon létre egy **bejövő szabály** , lehetővé teszi a bejövő hozzáférést tooport 9000.
9. Ha Windows-számítógép hello útválasztó mögött, hello útválasztó igényeinek konfigurált toobe tooperform hálózati hozzáférési fordítási a portjára 9000 között, amely elérhetővé toohello internet és port 9000 hello Windows számítógépen. Ez az Azure AD toobe képes tooaccess szükséges ehhez a végponthoz hello felhőben.

**tooregister hello minta SCIM végpont Azure AD-ben:**

1. Jelentkezzen be a túl[hello Azure-portálon](https://portal.azure.com). 
2. Keresse meg a túl ** Azure Active Directory > Vállalati alkalmazások, és válassza ki **új alkalmazás > minden > nem-gyűjtemény alkalmazás**.
3. Adja meg az alkalmazás nevét, és kattintson a **Hozzáadás** ikon toocreate app objektum. létrehozott hello alkalmazásobjektum tervezett toorepresent hello cél app volna egyszeri bejelentkezés, és nem csak hello SCIM végpont végrehajtási tooand kiépítés.
4. Az eredményül kapott üdvözlő képernyőt, válassza ki a hello **kiépítési** lapon hello bal oldali oszlopban.
5. A hello **kiépítési üzemmódban** menü **automatikus**.
    
  ![][2]
  *4. ábra: Konfigurálása kiosztás a hello Azure-portálon*
    
6. A hello **bérlői URL-cím** mezőbe írja be a hello internet közzétett URL-cím és port a SCIM végpont. Ez lenne valamit, például http://testmachine.contoso.com:9000 vagy http://<ip-address>:9000/, ahol a < ip-cím > hello internet van közzétéve az IP cím.  
7. Ha hello SCIM végpont megköveteli az OAuth tulajdonosi jogkivonat nem az Azure AD egy kiállítótól érkező, akkor a Másolás hello szükséges OAuth tulajdonosi jogkivonattal történő választható hello **titkos Token** mező. Ez a mező üresen marad, ha az Azure AD tartalmazza az Azure ad-minden egyes kérelemmel kiadott OAuth tulajdonosi jogkivonat. Alkalmazások, az Azure AD használja az identitásszolgáltató azt is ellenőrzi az Azure AD-jogkivonatot ki.
8. Kattintson a hello **kapcsolat tesztelése** gomb toohave Azure Active Directory kísérlet tooconnect toohello SCIM végpont. Ha hello kísérlet sikertelen, hiba információk jelennek meg.  
9. Ha hello kísérletek tooconnect toohello alkalmazás sikeres legyen, majd kattintson **mentése** toosave hello rendszergazdai hitelesítő adataival.
10. A hello **hozzárendelések** szakaszban, a két választható csoport attribútum-leképezésekhez: egy felhasználói objektum, egy, a csoport objektumainak. Minden egyes egy tooreview hello attribútumokat szinkronizált Azure Active Directory tooyour app választja ki. kiválasztott attribútumok hello **egyező** tulajdonságainak használt toomatch hello felhasználókat és csoportokat a frissítési műveletek az alkalmazásban. Válassza ki a hello Mentés gombra toocommit módosításokat.
11. A **beállítások**, hello **hatókör** mező határozza meg, hogy mely felhasználók és az or csoportok szinkronizálva. Válassza a "Sync csak hozzárendelt felhasználók és csoportok" (ajánlott) csak szinkronizálva lesz a hello rendelt felhasználók és csoportok **felhasználók és csoportok** fülre.
12. A konfiguráció befejezése után módosítsa a hello **kiépítési állapot** túl**a**.
13. Kattintson a **mentése** toostart hello Azure AD létesítési szolgáltatás. 
14. Ha csak szinkronizálás hozzárendelve felhasználók és csoportok (ajánlott), lehet, hogy tooselect hello **felhasználók és csoportok** lapra, és rendeljen hello felhasználók és/vagy csoportok toosync kívánja.

Miután hello kezdeti szinkronizálás elindult, használhatja a hello **naplók** lapon toomonitor folyamatban, amely mutatja az alkalmazás a szolgáltatás kiépítését hello által végzett összes műveletet. Hogyan tooread hello Azure AD-kiépítés naplózza a további információkért lásd: [automatikus felhasználói fiók kiépítése jelentések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).

utolsó lépés hello ellenőrzése során hello minta tooopen hello TargetFile.csv fájlt a számítógépre a Windows hello \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug mappában. Létesítésének folyamatát kell használnia hello futtatása után ez a fájl hello részletek az összes társított és kiosztása a felhasználók és csoportok jeleníti meg.

### <a name="development-libraries"></a>Fejlesztő függvénytárak
toodevelop saját webes szolgáltatás, amely megfelel a toohello SCIM specifikációját, először megismerje a megadott Microsoft toohelp felgyorsítani hello platform fejlesztési folyamata a következő könyvtárak hello: 

1. Közös nyelvi infrastruktúra (CLI) szalagtárak alapján, hogy az infrastrukturális, például a C# nyelv felkínált való használatra. A tárak egyik [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), deklarál illesztőfelület, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, hello a következő ábrán látható: A fejlesztői hello könyvtárak segítségével egy osztály, amely lehet hivatkozni, általános szolgáltatóként felülettel volna megvalósításához. hello szalagtárak hello fejlesztői toodeploy egy webszolgáltatás, amely megfelel a toohello SCIM meghatározás engedélyezése. hello webszolgáltatás Internet Information Services, vagy bármilyen végrehajtható közös nyelvi infrastruktúra szerelvény vagy lehet üzemeltetni. Kérelem hívások toohello szolgáltató, módszerek által hello fejlesztői toooperate néhány identitás tárolójában lévő volna programozhatók van lefordítva.
  
  ![][3]
  
2. [Express route kezelők](http://expressjs.com/guide/routing.html) érhetők el elemzés node.js kérelem objektumokból felé indított hívások (által meghatározott hello SCIM specification), tooa node.js webes szolgáltatás.   

### <a name="building-a-custom-scim-endpoint"></a>Egy egyéni SCIM végpont létrehozása
Hello CLI könyvtárak segítségével a fejlesztők a tárak segítségével tárolhatja, bármilyen végrehajtható közös nyelvi infrastruktúra szerelvényen belül, vagy az Internet Information Services belül a szolgáltatások. Itt látható mintakód egy végrehajtható szerelvényben, a következő címen: hello http://localhost:9000 szolgáltatás üzemeltetéséhez: 

    private static void Main(string[] arguments)
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IProvider and 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
      new DevelopersMonitor();
    Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
      new DevelopersProvider(arguments[1]);
    Microsoft.SystemForCrossDomainIdentityManagement.Service webService = null;
    try
    {
        webService = new WebService(monitor, provider);
        webService.Start("http://localhost:9000");

        Console.ReadKey(true);
    }
    finally
    {
        if (webService != null)
        {
            webService.Dispose();
            webService = null;
        }
    }
    }

    public class WebService : Microsoft.SystemForCrossDomainIdentityManagement.Service
    {
    private Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor;
    private Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider;

    public WebService(
      Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitoringBehavior, 
      Microsoft.SystemForCrossDomainIdentityManagement.IProvider providerBehavior)
    {
        this.monitor = monitoringBehavior;
        this.provider = providerBehavior;
    }

    public override IMonitor MonitoringBehavior
    {
        get
        {
            return this.monitor;
        }

        set
        {
            this.monitor = value;
        }
    }

    public override IProvider ProviderBehavior
    {
        get
        {
            return this.provider;
        }

        set
        {
            this.provider = value;
        }
    }
    }

Ez a szolgáltatás egy HTTP címet és a kiszolgáló hitelesítési tanúsítvánnyal kell rendelkeznie, mely hello legfelső szintű hitelesítésszolgáltató hello következő: 

* CNNIC
* Comodo
* CyberTrust
* DigiCert
* GeoTrust
* GlobalSign
* Go Daddy
* A VeriSign szolgáltatótól
* WoSign

Kiszolgálói hitelesítési tanúsítvány lehet a Windows hello hálózati rendszerhéj-segédprogrammal történő gazdagépen kötött tooa port: 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  

Itt hello megadott hello certhash argumentum érték hello tanúsítvány ujjlenyomata hello hello appid argumentumban megadott hello érték pedig tetszőleges globálisan egyedi azonosító.  

az Internet Information Services toohost hello szolgáltatást, egy fejlesztő lenne build CLA könyvtár kódszerelvényből indítási nevű hello alapértelmezett névtér hello szerelvény osztállyal.  Íme egy példa ilyen osztály: 

    public class Startup
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor and  
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter starter;

    public Startup()
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
          new DevelopersMonitor();
        Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
          new DevelopersProvider();
        this.starter = 
          new Microsoft.SystemForCrossDomainIdentityManagement.WebApplicationStarter(
            provider, 
            monitor);
    }

    public void Configuration(
      Owin.IAppBuilder builder) // Defined in in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

### <a name="handling-endpoint-authentication"></a>Kezelési végpont hitelesítés
Az Azure Active Directory kérések tartalmazzák az OAuth 2.0 tulajdonosi jogkivonatot.   A fogadó hello szolgáltatáskérés hitelesítenie kell, hogy az Azure Active Directory nevében várt hello Azure Active Directory-bérlőt az Azure Active Directory Graph webszolgáltatás hozzáférés toohello hello kibocsátó.  Hello jogkivonat hello kibocsátó azonosít egy iss jogcímet, például "iss": "https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".  Ebben a példában, alapszintű címéből hello hello jogcím értékét, mivel a kibocsátó hello https://sts.windows.net, azonosítja az Azure Active Directory, pedig hello relatív címet szegmens, cbb1a5ac-f33b-45fa-9bf5-f37db0fed422 egyedi azonosítójának hello Azure Active Directory-bérlő nevében mely hello token lett kiállítva.  Ha hello token ki hello Azure Active Directory Graph webes szolgáltatás, majd hello azonosítója, hogy a szolgáltatás elérésével 00000002-0000-0000-c000-000000000000 kell kell hello token és hello értékének jogcímek.  

SCIM szolgáltatás létrehozása a Microsoft által biztosított hello CLA könyvtárak segítségével a fejlesztők hitelesítheti a kérelmeket az Azure Active Directory hello Microsoft.Owin.Security.ActiveDirectory csomag használata a következő lépések végrehajtásával: 

1. A szolgáltató megvalósíthatja hello Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior tulajdonság azt egy hello szolgáltatás indulásakor nevű metódus toobe vissza: 

  ````
    public override Action\<Owin.IAppBuilder, System.Web.Http.HttpConfiguration.HttpConfiguration\> StartupBehavior
    {
      get
      {
        return this.OnServiceStartup;
      }
    }

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder,  // Defined in Owin.dll.  
      System.Web.Http.HttpConfiguration configuration)  // Defined in System.Web.Http.dll.  
    {
    }
  ````

2. A következő kód toothat metódus toohave hello bármely kérelem tooany hitelesítve, szem előtt a megadott tenantot, az Azure AD Graph webszolgáltatás hozzáférés toohello nevében Azure Active Directory által kiadott tokennek hello szolgáltatás végpontok hozzáadása: 

  ````
    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder IAppBuilder applicationBuilder, 
      System.Web.Http.HttpConfiguration HttpConfiguration configuration)
    {
      // IFilter is defined in System.Web.Http.dll.  
      System.Web.Http.Filters.IFilter authorizationFilter = 
        new System.Web.Http.AuthorizeAttribute(); // Defined in System.Web.Http.dll.configuration.Filters.Add(authorizationFilter);

      // SystemIdentityModel.Tokens.TokenValidationParameters is defined in    
      // System.IdentityModel.Token.Jwt.dll.
      SystemIdentityModel.Tokens.TokenValidationParameters tokenValidationParameters =     
        new TokenValidationParameters()
        {
          ValidAudience = "00000002-0000-0000-c000-000000000000"
        };

      // WindowsAzureActiveDirectoryBearerAuthenticationOptions is defined in 
      // Microsoft.Owin.Security.ActiveDirectory.dll
      Microsoft.Owin.Security.ActiveDirectory.
      WindowsAzureActiveDirectoryBearerAuthenticationOptions authenticationOptions =
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions()    {
        TokenValidationParameters = tokenValidationParameters,
        Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute hello appropriate tenant’s 
                                                      // identifier for this one.  
      };

      applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
    }
  ````


## <a name="user-and-group-schema"></a>Felhasználó- és séma
Az Azure Active Directory kétféle típusú erőforrások tooSCIM webszolgáltatások építhető ki.  Ilyen típusú erőforrások a felhasználók és csoportok.  

Felhasználói erőforrások hello sémaazonosítót, szerepel a protokoll-meghatározása urn: ietf:params:scim:schemas:extension:enterprise:2.0:User azonosítja: http://tools.ietf.org/html/draft-ietf-scim-core-schema.  hello alapértelmezett leképezését hello Azure Active Directory toohello attribútumok urn: ietf:params:scim:schemas:extension:enterprise:2.0:User erőforrások a felhasználók lejjebb tekinthetők meg a tábla 1.  

Erőforrások hello sémaazonosítót, azonosítva http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  2. táblázat, alább látható hello alapértelmezett leképezését hello http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group erőforrások az Azure Active Directory toohello attribútumok csoportok.  

### <a name="table-1-default-user-attribute-mapping"></a>1. táblázat: Alapértelmezett felhasználói címtárattribútum-leképezésben
| Az Azure Active Directory-felhasználó | urn: ietf:params:scim:schemas:extension:enterprise:2.0:User |
| --- | --- |
| IsSoftDeleted |Aktív |
| displayName |displayName |
| Telefax-TelephoneNumber |.value phoneNumbers [típus eq "fax"] |
| givenName |name.givenName |
| Beosztás |Cím |
| mail |e-mailek [típus eq "munkahelyi"] .value |
| mailNickname |externalId |
| Manager |Manager |
| Mobileszköz |.value phoneNumbers [típus eq "mobileszköz"] |
| Objektumazonosító |id |
| Irányítószám |[típus eq "munkahelyi"] címek .postalCode |
| proxy-címek |[Írja be az "egyéb" eq] e-maileket. Érték |
| fizikai-kézbesítés-OfficeName |[Írja be az "egyéb" eq] címek. Formázott |
| StreetAddress |[típus eq "munkahelyi"] címek .streetAddress |
| Vezetéknév |name.familyName |
| Telefonszám |.value phoneNumbers [típus eq "munkahelyi"] |
| felhasználó-egyszerű név |Felhasználónév |

### <a name="table-2-default-group-attribute-mapping"></a>2. táblázat: Alapértelmezett attribútum leképezése
| Azure Active Directory-csoportok | http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group |
| --- | --- |
| displayName |externalId |
| mail |e-mailek [típus eq "munkahelyi"] .value |
| mailNickname |displayName |
| Tagok |Tagok |
| Objektumazonosító |id |
| proxyAddresses |[Írja be az "egyéb" eq] e-maileket. Érték |

## <a name="user-provisioning-and-de-provisioning"></a>Felhasználói üzembe helyezést és megszüntetést
a következő ábra azt mutatja be, hogy Azure Active Directory tooa SCIM szolgáltatás toomanage hello életciklusát a felhasználó elküldi egy másik identitás tárolására köszönőüzenetei hello. hello ábrán is látható, hogyan hello CLI szalagtárak készletével megvalósított SCIM szolgáltatás által biztosított Microsoft rendszerbeli kiépítésének e szolgáltatások lefordítása hívások toohello módszerek a szolgáltató ezeket a kérelmeket.  

![][4]
*5. ábra: A felhasználók átadása, és megszüntetést feladatütemezési*

1. Az Azure Active Directory lekérdezésekkel hello szolgáltatást az Azure AD-ben hello mailNickname attribútum értékét a felhasználó megfelelő externalId attribútum értéke. Ebben a példában, amelynek jyoung egy olyan felhasználó, az Azure Active Directoryban egy mailNickname mintát például Hypertext Transfer Protocol (HTTP) kérelmet hello lekérdezési kifejezése: 
  ````
    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...
  ````
  Ha hello szolgáltatás SCIM szolgáltatások végrehajtásához a Microsoft által biztosított hello közös nyelvi infrastruktúra könyvtárak segítségével lett létrehozva, majd hello kérelem lefordítását egy hívás toohello hello szolgáltatás szolgáltató lekérdezési metódust.  Ez a metódus aláírása hello: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]> Query(
      Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
      string correlationIdentifier);
  ````
  Íme hello Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters felület hello definíciója: 
  ````
    public interface IQueryParameters: 
      Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
        System.Collections.Generic.IReadOnlyCollection <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
        { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
      system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
      { get; }
      System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
      { get; }
      string SchemaIdentifier 
      { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IFilter
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IFilter AdditionalFilter 
          { get; set; }
        string AttributePath 
          { get; } 
        Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator FilterOperator 
          { get; }
        string ComparisonValue 
          { get; }
    }

    public enum Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator
    {
        Equals
    }
  ````
  A következő példa egy felhasználó egy adott értékre hello externalId attribútum lekérdezés hello toohello lekérdezés módszer átadott hello argumentumok értékek a következők: 
  * a paraméterek. AlternateFilters.Count: 1
  * a paraméterek. AlternateFilters.ElementAt(0). AttributePath: "externalId"
  * a paraméterek. AlternateFilters.ElementAt(0). ÖsszehasonlítóOperátor: ComparisonOperator.Equals
  * a paraméterek. AlternateFilter.ElementAt(0). ComparisonValue: "jyoung"
  * correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin. Kérelemazonosító"] 

2. Ha hello válasz tooa lekérdezés toohello webes szolgáltatás, amely megfelel a felhasználó hello mailNickname attribútum értéke externalId attribútumértékkel rendelkező felhasználó nem ad vissza azokat a felhasználókat, Azure Active Directory kéri, hogy hello szolgáltatás kiépítése a felhasználó megfelelő toohello egy Azure Active Directoryban.  Íme egy példa a kérelem: 
  ````
    POST https://.../scim/Users HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas":
      [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0User"],
      "externalId":"jyoung",
      "userName":"jyoung",
      "active":true,
      "addresses":null,
      "displayName":"Joy Young",
      "emails": [
        {
          "type":"work",
          "value":"jyoung@Contoso.com",
          "primary":true}],
      "meta": {
        "resourceType":"User"},
       "name":{
        "familyName":"Young",
        "givenName":"Joy"},
      "phoneNumbers":null,
      "preferredLanguage":null,
      "title":null,
      "department":null,
      "manager":null}
  ````
  hello közös nyelvi infrastruktúra szalagtárak SCIM szolgáltatások végrehajtásához a Microsoft által biztosított lefordítja a kérésre egy hívás toohello Create metódussal hello szolgáltatás szolgáltató be.  Create metódussal hello az aláírása: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);
  ````
  A kérelem tooprovision egy felhasználó hello hello erőforrás argumentum értéke hello Microsoft.SystemForCrossDomainIdentityManagement példánya. Hello Microsoft.SystemForCrossDomainIdentityManagement.Schemas könyvtárban meghatározott Core2EnterpriseUser osztály.  Hello kérelem tooprovision hello felhasználó sikeres, majd hello hello metódus megvalósítása esetén várható tooreturn hello Microsoft.SystemForCrossDomainIdentityManagement példánya. Core2EnterpriseUser osztályra, hello hello azonosítója tulajdonság értékének beállítása toohello hello újonnan kiosztott felhasználó egyedi azonosítója.  

3. a felhasználó által egy SCIM, úgy, hogy a felhasználó aktuális állapotának hello kér hello szolgáltatás például kérelmet tartalmazó Azure Active Directory folytatódik fronted identitás tárolóban tooexist ismert tooupdate: 
  ````
    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  Hello közös nyelvi infrastruktúra szalagtárak SCIM szolgáltatások végrehajtásához a Microsoft által biztosított használatával készített szolgáltatásban hello kérelem lefordítását egy hívás toohello hello szolgáltatás szolgáltatója lekérése metódusában.  Hello lekérése metódus aláírása hello itt található: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource and 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
    // are defined in Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> 
       Retrieve(
         Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
           parameters, 
           string correlationIdentifier);

    public interface 
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
        IRetrievalParameters
        {
          Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
            ResourceIdentifier 
              { get; }
    }
    public interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier
    {
        string Identifier 
          { get; set; }
        string Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier 
          { get; set; }
    }
  ````
  Egy kérelem tooretrieve hello aktuális állapotát egy felhasználó hello példában hello tulajdonságok hello paraméterek argumentumnak hello értékeként megadott hello objektum hello értékei a következők: 
  
  * Azonosító: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

4. Ha egy hivatkozás attribútum toobe frissítve, majd a Azure Active Directory lekérdezések hello szolgáltatás toodetermine attól hello hello hivatkozási attribútum hello identitás tárolójában aktuális értéke fronted által hello szolgáltatást már megegyezik-e hello adott attribútum az Azure Active Directoryban. A felhasználók számára hello attribútum, mely hello a jelenlegi érték a ily módon le kell kérdezni hello manager attribútum. Íme egy példa egy kérelem toodetermine e hello kezelő egy adott felhasználó objektum attribútuma van a megadott érték: 
  ````
    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...
  ````
  érték hello hello attribútumok lekérdezési paraméter azonosítója, jelzi, hogy ha létezik olyan felhasználói objektum, amely megfelel a megadott hello szűrő lekérdezési paraméter értékeként hello hello kifejezést, majd hello szolgáltatás urn: ietf:params:scim:schemas a várt toorespond: Core: 2.0:User vagy urn: ietf:params:scim:schemas:extension:enterprise:2.0:User erőforrás, beleértve az adott erőforrás id attribútum csak hello értékével.  hello értékének hello **azonosító** toohello kérelmező ismert attribútum. Hello szűrő lekérdezésparaméter; hello érték szerepel. hello kérdezi azt célja ténylegesen toorequest egy erőforrást, hogy létezik-e ilyen objektum jelezhetik hello szűrőkifejezés felel meg a minimális megjelenítése.   

  Ha hello szolgáltatás SCIM szolgáltatások végrehajtásához a Microsoft által biztosított hello közös nyelvi infrastruktúra könyvtárak segítségével lett létrehozva, majd hello kérelem lefordítását egy hívás toohello hello szolgáltatás szolgáltató lekérdezési metódust. hello paraméterek argumentumnak hello értékeként megadott hello objektum tulajdonságainak hello hello értékének a következők: 
  
  * a paraméterek. AlternateFilters.Count: 2. régiója
  * a paraméterek. AlternateFilters.ElementAt(x). AttributePath: "id"
  * a paraméterek. AlternateFilters.ElementAt(x). ÖsszehasonlítóOperátor: ComparisonOperator.Equals
  * a paraméterek. AlternateFilter.ElementAt(x). ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * a paraméterek. AlternateFilters.ElementAt(y). AttributePath: "manager"
  * a paraméterek. AlternateFilters.ElementAt(y). ÖsszehasonlítóOperátor: ComparisonOperator.Equals
  * a paraméterek. AlternateFilter.ElementAt(y). ComparisonValue: "2819c223-7f76-453a-919d-413861904646"
  * a paraméterek. RequestedAttributePaths.ElementAt(0): "id"
  * a paraméterek. SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

  Itt hello index x hello értéke lehet 0 és hello index y hello értéke lehet 1, vagy x hello értéke lehet 1 és hello y érték 0, attól függően, hogy hello hello szűrő lekérdezési paraméter hello kifejezések sorrendjét.   

5. Íme egy példa egy kérelem az Azure Active Directory tooan SCIM szolgáltatás tooupdate egy felhasználó: 
  ````
    PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas": 
      [
        "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
      "Operations":
      [
        {
          "op":"Add",
          "path":"manager",
          "value":
            [
              {
                "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
                "value":"2819c223-7f76-453a-919d-413861904646"}]}]}
  ````
  hello Microsoft közös nyelvi infrastruktúra-könyvtárakban SCIM szolgáltatások végrehajtási lefordítja hello kérelem be egy hívás toohello frissítési módszer hello szolgáltatás szolgáltató. Hello aláírása hello frissítési módszer a következő: 
  ````
    // System.Threading.Tasks.Tasks and 
    // System.Collections.Generic.IReadOnlyCollection<T>
    // are defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IPatch, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationName, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IPath and 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationValue 
    // are all defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

    System.Threading.Tasks.Task Update(
      Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, 
      string correlationIdentifier);

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch
    {
    Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase 
      PatchRequest 
        { get; set; }
    Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
      ResourceIdentifier 
        { get; set; }        
    }

    public class PatchRequest2: 
      Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase
    {
    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation> 
        Operations
        { get;}

    public void AddOperation(
      Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation operation);
    }

    public class PatchOperation
    {
    public Microsoft.SystemForCrossDomainIdentityManagement.OperationName 
      Name
      { get; set; }

    public Microsoft.SystemForCrossDomainIdentityManagement.IPath 
      Path
      { get; set; }

    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.OperationValue> Value
      { get; }

    public void AddValue(
      Microsoft.SystemForCrossDomainIdentityManagement.OperationValue value);
    }

    public enum OperationName
    {
      Add,
      Remove,
      Replace
    }

    public interface IPath
    {
      string AttributePath { get; }
      System.Collections.Generic.IReadOnlyCollection<IFilter> SubAttributes { get; }
      Microsoft.SystemForCrossDomainIdentityManagement.IPath ValuePath { get; }
    }

    public class OperationValue
    {
      public string Reference
      { get; set; }

      public string Value
      { get; set; }
    }
  ````
    A kérelem egy felhasználó tooupdate hello példában hello hello javítás argumentumnak hello értékeként megadott objektumnak a tulajdonságok értékeit: 
  
  * ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"
  * (Mint PatchRequest2 PatchRequest). Operations.Count: 1
  * (Mint PatchRequest2 PatchRequest). Operations.ElementAt(0). OperationName: OperationName.Add
  * (Mint PatchRequest2 PatchRequest). Operations.ElementAt(0). Path.AttributePath: "manager"
  * (Mint PatchRequest2 PatchRequest). Operations.ElementAt(0). Value.Count: 1
  * (Mint PatchRequest2 PatchRequest). Operations.ElementAt(0). Value.ElementAt(0). Hivatkozás: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646
  * (Mint PatchRequest2 PatchRequest). Operations.ElementAt(0). Value.ElementAt(0). Érték: 2819c223-7f76-453a-919d-413861904646

6. toode-provision a felhasználó identitása áruházban fronted SCIM szolgáltatása, az Azure AD egy kérést küld, mint: 
  ````
    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  Ha hello szolgáltatás SCIM szolgáltatások végrehajtásához a Microsoft által biztosított hello közös nyelvi infrastruktúra könyvtárak segítségével lett létrehozva, majd hello kérelem lefordítását egy hívás toohello hello szolgáltatás szolgáltató Delete metódust.   Ez a módszer az aláírása: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
  ````
  hello hello resourceIdentifier argumentumnak hello értékeként megadott objektumnak a tulajdonságok értékeit az hello példa egy kérelem toode-rendelkezés a felhasználó: 
  
  * ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

## <a name="group-provisioning-and-de-provisioning"></a>Csoport üzembe helyezést és megszüntetést
a következő ábra azt mutatja be, hogy Azure AcD tooa SCIM szolgáltatás toomanage hello életciklus csoport elküldi egy másik identitás tárolására köszönőüzenetei hello.  Az üzenetek háromféleképpen toousers vonatkozó köszönőüzenetei különböznek: 

* egy csoport erőforrás sémája hello http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group azonosítja.  
* Tooretrieve csoportok határozzák meg, hogy hello tagok attribútum kérelmek bármely válasz toohello kérelemben megadott erőforrás kizárását toobe érték.  
* Kérelmek toodetermine, hogy rendelkezik-e a hivatkozási attribútum egy adott értékre hello tagok attribútum vonatkozó kérelmek.  

![][5]
*6. ábra: Az üzembe helyezést és megszüntetést feladatütemezési csoport*

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [Felhasználói kiépítési/megszüntetés tooSaaS alkalmazások automatizálása](active-directory-saas-app-provisioning.md)
* [A felhasználók átadása attribútum-leképezésekhez testreszabása](active-directory-saas-customizing-attribute-mappings.md)
* [Attribútum-leképezésekhez kifejezések írása](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Helyezése Hatókörszűrőkkel felhasználói történő üzembe helyezéséhez](active-directory-saas-scoping-filters.md)
* [Alkalmazás-kiépítési értesítések](active-directory-saas-account-provisioning-notifications.md)
* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz](active-directory-saas-tutorial-list.md)

<!--Image references-->
[0]: ./media/active-directory-scim-provisioning/scim-figure-1.PNG
[1]: ./media/active-directory-scim-provisioning/scim-figure-2a.PNG
[2]: ./media/active-directory-scim-provisioning/scim-figure-2b.PNG
[3]: ./media/active-directory-scim-provisioning/scim-figure-3.PNG
[4]: ./media/active-directory-scim-provisioning/scim-figure-4.PNG
[5]: ./media/active-directory-scim-provisioning/scim-figure-5.PNG
