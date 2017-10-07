---
title: "Útmutató az Azure Active Directory feltételes hozzáférési aaaDeveloper |} Microsoft Docs"
description: "Fejlesztői útmutatás és az Azure AD feltételes hozzáférési forgatókönyvek"
services: active-directory
keywords: 
author: danieldobalian
manager: mbaldwin
editor: PatAltimore
ms.author: dadobali
ms.date: 07/19/2017
ms.assetid: 115bdab2-e1fd-4403-ac15-d4195e24ac95
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.openlocfilehash: 589393f5d084d64872b372d895dc889f300592bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="developer-guidance-for-azure-active-directory-conditional-access"></a>A feltételes hozzáférés az Azure Active Directory fejlesztői útmutató

Az Azure Active Directory (AD) kínál számos módon toosecure az alkalmazás és szolgáltatás védelmére.  Ezek a egyedi szolgáltatások egyik feltételes hozzáférés.  Feltételes hozzáférés lehetővé teszi a fejlesztők és a vállalati ügyfelek tooprotect szolgáltatások többféle többek között:

* Multi-Factor Authentication
* Így csak az Intune-ban regisztrált eszközök tooaccess adott szolgáltatások
* Korlátozza a felhasználó helyét és IP-címtartományok

A feltételes hozzáférés hello szolgáltatás összes funkciójáról a további információkért lásd: [feltételes hozzáférés a klasszikus Azure portálon hello](../active-directory-conditional-access.md). 

Ez a cikk azt összpontosítani milyen feltételes hozzáférési azt jelenti, hogy toodevelopers épület alkalmazások az Azure AD.  Ismeretét feltételezi [egyetlen](active-directory-integrating-applications.md) és [több-bérlős](active-directory-devhowto-multi-tenant-overview.md) alkalmazások és [közös hitelesítési minták](active-directory-authentication-scenarios.md).

Igazolnia kell alaposabban hello hatása lehet alkalmazni a feltételes hozzáférési szabályzatokat vezérlő nincs keresztül erőforrások eléréséhez.  Továbbá azt Fedezze fel hello következményei hello a-meghatalmazásos folyamat, a feltételes hozzáférés webes alkalmazások elérése a Microsoft Graph hello, hívja az API-k.

## <a name="how-does-conditional-access-impact-an-app"></a>Milyen hatással van a feltételes hozzáférés az alkalmazások?

### <a name="app-topologies-impacted"></a>Érintett alkalmazás topológiák

Leggyakoribb esetekben a feltételes hozzáférés nem változtatja meg az alkalmazások viselkedését, vagy hello fejlesztőtől származó módosításokat igényel.  Csak bizonyos esetekben egy alkalmazás közvetve vagy csendes kér egy jogkivonatot egy szolgáltatáshoz, egy alkalmazás megköveteli kód módosítása toohandle feltételes hozzáférési "kihívások".  Elképzelhető, hogy más dolga, mint egy interaktív bejelentkezési kérelem végrehajtása. 

Pontosabban hello következő esetekben szükséges kód toohandle feltételes hozzáférés "kihívást": 

* A Microsoft Graph hello hozzáférő alkalmazásokat
* Alkalmazások hello a-meghatalmazásos folyamat végrehajtása
* Az alkalmazások több szolgáltatások/erőforrások elérése
* Egylapos alkalmazások ADAL.js

Feltételes hozzáférési házirendek alkalmazott toohello alkalmazás, de is lehet alkalmazott tooa webes API az alkalmazás fér hozzá. További részletek toolearn, hogyan tooconfigure feltételes hozzáférési házirendet, adja meg a [Azure Active Directory feltételes hozzáférés – első lépések](../active-directory-conditional-access-azuread-connected-apps.md#configure-per-application-access-rules).

Hello forgatókönyvtől függően az ügyfél egy vállalati alkalmazni, és bármikor eltávolíthatja a feltételes hozzáférési szabályzatokat.  Ahhoz, hogy az alkalmazás toocontinue működik-e egy új házirend alkalmazásakor kell tooimplement hello "kérdés" kezelését. hello a következő példák bemutatják, kérdés kezelését. 

### <a name="conditional-access-examples"></a>Feltételes hozzáférési példa

Bizonyos esetekben kód módosítások toohandle feltételes hozzáférést igényelnek, míg mások a vártnak megfelelően van.  Az alábbiakban néhány olyan forgatókönyvet, néhány hello különbség betekintést biztosít a feltételes hozzáférés toodo multi-factor authentication használatával.

* Egy egyszeres-bérlő iOS-alkalmazást fejleszt, és a feltételes hozzáférési házirend alkalmazása.  hello alkalmazás a felhasználó bejelentkezik, és hozzáférést tooan API nem igényelhetnek.  Hello felhasználó jelentkezik be, amikor hello házirend automatikusan megnyílik, és hello felhasználó számára szükséges tooperform többtényezős hitelesítés (MFA). 
* A több-bérlős webes alkalmazás által használt hello Microsoft Graph tooaccess Exchange, más szolgáltatások között készítésekor.  Ez az alkalmazás fogad el egy vállalati ügyfelünk SharePoint Online-on állít be egy házirendet.  Amikor hello webalkalmazás jogkivonat MS Graph, az összes Microsoft Service egy házirend érvényben van-e (kifejezetten szolgáltatások graph keresztül elérhető).  A végfelhasználói többtényezős Hitelesítést a rendszer. Hello esetben hello végfelhasználói érvényes jogkivonatokat van bejelentkezve, a "kérdés" jogcímek toohello webalkalmazás adja vissza.  
* Egy natív alkalmazás, amely a középső réteg tooaccess hello Microsoft Graph használ készítésekor.  Egy vállalati ügyfél használja az alkalmazást hello vállalat egy házirend tooExchange Online vonatkozik.  Ha felhasználó jelentkezik be, a hello natív alkalmazás kéri a hozzáférési toohello középső réteg, és elküldi a hello jogkivonat.  hello középső réteg a-meghatalmazásos folyamat toorequest hozzáférés toohello MS Graph hajt végre.  Ezen a ponton a "kérdés" jogcímek toohello középső réteg áll rendelkezésre. hello középső réteg küldi hello challenge hátsó toohello natív alkalmazást, mert toocomply hello feltételes hozzáférési házirenddel kell.

### <a name="complying-with-a-conditional-access-policy"></a>A feltételes hozzáférési házirend megfelel

Számos különböző alkalmazás topológia esetén a feltételes hozzáférési házirend van kiértékelve, amikor a hello munkamenet.  Mint az alkalmazások és szolgáltatások hello granularitása a feltételes hozzáférési házirend, hello pontot, amellyel meghívták nagyban függ hello forgatókönyvön tooaccomplish próbál.

Amikor az alkalmazás tooaccess feltételes hozzáférési házirenddel szolgáltatásként próbál, akkor a feltételes hozzáférés kihívást merülhetnek fel.  Ez a kérdés hello van kódolva `claims` paraméter, amely rendelkezik az Azure AD választ, vagy a Microsoft Graph hello.  Íme egy példa a challenge paraméter: 

```
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

A fejlesztők a challenge igénybe, és egy új kérelmet tooAzure AD alakzatot fűzi azokat.  Ebben az állapotban átadásakor megadását kéri az hello végfelhasználói tooperform bármely művelet szükséges toocomply hello feltételes hozzáférési házirenddel. A következő forgatókönyvek hello hello hiba, és hogyan tooextract hello paraméter mintaadatokról magyarázatát olvashatja. 

## <a name="scenarios"></a>Forgatókönyvek

### <a name="prerequisites"></a>Előfeltételek

Azure AD feltételes hozzáférés lehetővé teszi a [Azure AD Premium](../active-directory-whatis.md#choose-an-edition).  Hello követelményeinek licenceléssel kapcsolatos részletesebb [licenc nélküli használati jelentés](../active-directory-conditional-access-unlicensed-usage-report.md).  A fejlesztők csatlakozhat hello [Microsoft Developer Network](https://msdn.microsoft.com/dn308572.aspx), mely tartalmazza a nagyvállalati mobilitási csomag, amely magában foglalja a prémium szintű Azure AD ingyenes előfizetés toohello.

### <a name="considerations-for-specific-scenarios"></a>Meghatározott forgatókönyvek szempontjai

információk csak a következő hello alkalmazza a feltételes hozzáférés forgatókönyvekben:

* A Microsoft Graph hello hozzáférő alkalmazásokat
* Alkalmazások hello a-meghatalmazásos folyamat végrehajtása
* Az alkalmazások több szolgáltatások/erőforrások elérése
* Egylapos alkalmazások ADAL.js

A következő részekben hello azt fogja bejutni közös bemutató forgatókönyvek összetettebbek.  működési elv hello core feltételes hozzáférési házirendek kiértékelése időpontban hello hello token kérik hello szolgáltatást egy feltételes hozzáférési házirend alkalmazva, kivéve, ha a Microsoft Graph hello keresztül van használatban.

### <a name="scenario-app-accessing-hello-microsoft-graph"></a>Forgatókönyv: Alkalmazás hello Microsoft Graph elérése

Ebben a forgatókönyvben azt bízná hello esetben ha a webes alkalmazás hozzáférés toohello Microsoft Graph kér. hello feltételes hozzáférési házirend ebben az esetben hozzárendelése sikerült tooSharePoint, Exchange vagy néhány egyéb szolgáltatás, amelyhez a Microsoft Graph hello keresztül feladatként.  Ebben a példában tegyük fel nincs Sharepoint online feltételes hozzáférési házirendet.

![Alkalmazás hello Microsoft Graph folyamatábrán elérése](media/active-directory-conditional-access-developer/app-accessing-microsoft-graph-scenario.png)

hello app először kérelmez engedélyezési toohello Microsoft Graph-szükséges fér hozzá egy alsóbb rétegbeli munkaterhelés feltételes hozzáférés nélkül.  sikeres hello bármely házirend meghívása nélkül, és hello alkalmazást a Microsoft Graph-jogkivonatokat kap.  Ezen a ponton hello alkalmazás használhat hello hozzáférési jogkivonat tulajdonosi kérelemben kért hello végpont. Most, hello alkalmazásnak kell tooaccess a Sharepoint Online végpont Microsoft Graph, például:`https://graph.microsoft.com/v1.0/me/mySite`

hello app már van egy érvényes tokent a Microsoft Graph-hello, így műveleteket hajthat végre hello új kérelem nélkül egy új jogkivonat kiállítása történik. A kérelem sikertelen lesz, és a Microsoft Graph formájában hello egy HTTP 403 Tiltott a kiadott jogcímeket kihívást egy ```WWW-Authenticate``` kérdés.
Íme egy példa a hello válasz: 

```
HTTP 403; Forbidden 
error=insufficient_claims
www-authenticate="Bearer realm="", authorization_uri="https://login.windows.net/common/oauth2/authorize", client_id="<GUID>", error=insufficient_claims, claims={"access_token":{"polids":{"essential":true,"values":["<GUID>"]}}}"
```

hello jogcímek challenge belül van hello ```WWW-Authenticate``` fejlécet, amely lehet elemzett tooextract hello jogcímek paraméter hello következő kérés.  Miután hozzáfűzött toohello új kérelem, az Azure AD tudja tooevaluate hello feltételes hozzáférési házirend, ha most hello felhasználói és hello alkalmazás aláírása nem felelnek meg hello feltételes hozzáférési szabályzat.  Ismétlődő hello kérelem toohello Sharepoint Online végpont sikeres lesz.

Bemutatják, hogyan toohandle hello jogcímek challenge mintakódok, tekintse meg a toohello [.NET asztali kódminta](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) ADAL .NET vagy hello [a nevében-az kódminta](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca) ADAL .NET-keretrendszerhez készült.

### <a name="scenario-app-performing-hello-on-behalf-of-flow"></a>Forgatókönyv: Alkalmazás hello a-meghatalmazásos folyamat végrehajtása

Ebben a forgatókönyvben azt bízná hello esetet, amelyben a natív alkalmazással hívja a webes szolgáltatás/API-t is.  Viszont teszi a szolgáltatás ["a-meghatalmazásos" folyamat hello](active-directory-authentication-scenarios.md#application-types-and-scenarios) toocall egy alsóbb rétegbeli szolgáltatás.  Ebben az esetben azt alkalmazta a feltételes hozzáférési házirend toohello alárendelt szolgáltatás (Web API 2) és a kiszolgáló/démon alkalmazás helyett a natív alkalmazással használ. 

![Alkalmazás végrehajtása hello a nevében-az folyamatábrája](media/active-directory-conditional-access-developer/app-performing-on-behalf-of-scenario.png)

hello kezdeti jogkivonatkérelem webes API 1 nem kéri a felhasználót a multi-factor authentication hello végfelhasználói, webes API 1 nem lehetséges, hogy mindig találati hello alsóbb rétegbeli API.  Webes API 1 toorequest egy token a-nevében-: hello felhasználó megpróbál a Web API 2, miután hello kérelem sikertelen lesz, mivel hello felhasználó nem jelentkezett be többtényezős hitelesítést.

Az Azure AD egy HTTP-válasz érdekes adatokkal tér vissza: 

> [!NOTE]
> Ebben a példában a multi-factor Authentication hitelesítés hibaleírás, de számos különböző `interaction_required` lehetséges kapcsolódó tooconditional hozzáférést.  

```
HTTP 400; Bad Request 
error=interaction_required
error_description=AADSTS50076: Due tooa configuration change made by your administrator, or because you moved tooa new location, you must use multi-factor authentication tooaccess '<Web API 2 App/Client ID>'.
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

A webes API-1, a Microsoft hello hiba catch `error=interaction_required`, és küldje vissza hello `claims` challenge toohello egy asztali alkalmazás.  Ezen a ponton végezhet egy új hello egy asztali alkalmazás `acquireToken()` hívja, és a hozzáfűző hello `claims`challenge extra lekérdezési karakterlánc paraméterként.  Az új kérelemben hello felhasználói toodo többtényezős hitelesítést igényel, és elküldheti-e az új jogkivonat hátsó tooWeb API 1 és a teljes hello a-meghatalmazásos folyamat.

Ebben az esetben ki tootry lásd: a [.NET kódminta](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca).  Azt mutatja be, hogyan toopass jogcímek challenge vissza alkalmazásból webes API 1 toohello natív hello és hoz létre új kérelmet hello ügyfélalkalmazás belül. 

### <a name="scenario-app-accessing-multiple-services"></a>Forgatókönyv: Alkalmazás több szolgáltatás használata

Ebben a forgatókönyvben azt bízná amelyek egyike a feltételes hozzáférési házirend hozzárendelése egy webalkalmazást fér hozzá két szolgáltatások hello eset is.  Attól függően, hogy az alkalmazás logika egy elérési utat, amelyben az alkalmazás nem igényli a hozzáférési tooboth webszolgáltatások előfordulhat, hogy létezik.  Ebben a forgatókönyvben jogkivonatot kérni hello sorrendje fontos szerepet hello végfelhasználói élményt nyújt.

Tegyük fel, A és B webszolgáltatás tudunk és B webszolgáltatás rendelkezik a feltételes hozzáférési házirend is tartozik.  Hello kezdeti interaktív hitelesítési kérelmek mindkét szolgáltatás hozzájárulási igényel, míg hello feltételes hozzáférési házirend nem minden esetben szükséges.  Ha hello app webszolgáltatáshoz B jogkivonat kér, hello házirend meghívták, és A webszolgáltatás kérelmeknél is az alábbiak szerint sikeres lesz.

![Az alkalmazás eléréséhez több-szolgáltatások folyamatábrája](media/active-directory-conditional-access-developer/app-accessing-multiple-services-scenario.png)

Másik lehetőségként Ha hello alkalmazás jogkivonat kezdetben kér a webes szolgáltatás A, hello végfelhasználói nem lép működésbe hello feltételes hozzáférési szabályzat.  Így hello alkalmazás fejlesztőjének toocontrol hello végfelhasználói élményt, és nem kényszerítik ki a hello feltételes hozzáférési házirend toobe minden esetben meghívni. hello legbonyolultabb eset. Ha hello alkalmazás ezt követően kéri jogkivonat webszolgáltatáshoz a b kiszolgálóra. Ezen a ponton hello végfelhasználói kell toocomply hello feltételes hozzáférési házirenddel.  Amikor megpróbál hello app túl`acquireToken`, akkor hozhat létre a következő (mutatja be a következő diagram hello) hiba hello: 

```
HTTP 400; Bad Request
error=interaction_required
error_description=AADSTS50076: Due tooa configuration change made by your administrator, or because you moved tooa new location, you must use multi-factor authentication tooaccess '<Web API App/Client ID>'.
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
``` 

![Az alkalmazás egy új jogkivonatot kérő több szolgáltatás eléréséhez](media/active-directory-conditional-access-developer/app-accessing-multiple-services-new-token.png)

Ha hello alkalmazás hello ADAL-könyvtár, egy hiba tooacquire hello token mindig a rendszer ismét megkísérli interaktív módon.  Az interaktív kérés esetén hello végfelhasználónak hello lehetőség toocomply hello feltételes hozzáférést.  Ez érvényét veszti, ha hello kérelme, mert egy `AcquireTokenSilentAsync` vagy `PromptBehavior.Never` ebben az esetben hello alkalmazásnak kell-e az interaktív tooperform ```AcquireToken``` kérelem toogive hello végfelhasználás hello lehetőség toocomply hello házirendnek. 

### <a name="scenario-single-page-app-spa-using-adaljs"></a>Forgatókönyv: Egy lap App (SPA) ADAL.js használatával

Ebben a forgatókönyvben azt végezze el a hello eset, amikor a egylapos alkalmazások (SPA) van, egy feltételes hozzáférési védett ADAL.js toocall használatával webes API.  Ez egy egyszerű architektúra, de néhány apró kell figyelembe venni, a feltételes hozzáférés körül fejlesztésekor toobe rendelkezik.

ADAL.js, nincsenek tokenek beszerzése néhány függvények: `login()`, `acquireToken(...)`, `acquireTokenPopup(…)`, és `acquireTokenRedirect(…)`. 

* `login()`beolvassa az azonosító tokent egy interaktív bejelentkezési kérelem keresztül, de nem olvasható be a hozzáférési jogkivonatok valamelyik (beleértve a feltételes hozzáférés védett webes API-k) szolgáltatás.  
* `acquireToken(…)`majd kell használt toosilently szerezhet be egy jogkivonatot, azaz a felhasználói felület semmilyen körülmények között nem szerepelnek.  
* `acquireTokenPopup(…)`és `acquireTokenRedirect(…)` mindkét használt toointeractively kérjen mindig bejelentkezési felhasználói felület megjelenítése tehát erőforrás tokent.

Amikor egy alkalmazásnak kell egy hozzáférési jogkivonat toocall egy webes API-t, az megpróbál egy `acquireToken(…)`.  Ha hello token munkamenet lejárt, vagy a feltételes hozzáférési házirenddel toocomply kell, majd hello *acquireToken* sikertelen funkciót, és az alkalmazás által használt hello `acquireTokenPopup()` vagy `acquireTokenRedirect()`.

![Egyetlen lapnak alkalmazást ADAL folyamatábrája](media/active-directory-conditional-access-developer/spa-using-adal-scenario.png)

Bemutatjuk, a feltételes hozzáférési forgatókönyv egy példán keresztül.  hello végfelhasználói ebben az esetben hello helyen lévő és nem rendelkezik a munkamenetben.  Most elvégezni egy `login()` hívja, szerezze be a Azonosítót tokent multi-factor authentication nélkül.  Majd hello felhasználói találatok hello toorequest adatai egy webes API-t igénylő gomb.  hello app toodo megpróbál egy `acquireToken()` hívás azonban sikertelen, mert hello felhasználó nem végezte el a multi-factor authentication még és toocomply hello feltételes hozzáférési házirenddel kell.

Az Azure AD küld vissza a következő HTTP-válasz hello: 

```
HTTP 400; Bad Request 
error=interaction_required
error_description=AADSTS50076: Due tooa configuration change made by your administrator, or because you moved tooa new location, you must use multi-factor authentication tooaccess '<Web API App/Client ID>'.
```

Az alkalmazásnak kell toocatch hello `error=interaction_required`.  hello alkalmazás majd választhat `acquireTokenPopup()` vagy `acquireTokenRedirect()` a hello azonos erőforrás.  hello felhasználó kényszerített toodo multi-factor Authentication hitelesítést. Hello multi-factor Authentication hitelesítést követően a hello felhasználói hello app kiadott új hozzáférési jogkivonat hello a kért erőforrás.

Ebben az esetben ki tootry lásd: a [JS SPA a nevében-az kódminta](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca).  A fenti hello feltételes hozzáférési házirend és a webes API-t regisztrált korábban egy JS SPA toodemonstrate ebben a forgatókönyvben használja. Hogyan tooproperly leíró hello jogcímek ellenőrző kérdés és a szereznie egy hozzáférési jogkivonatot, amely nem használható a Web API mutatja. Másik lehetőségként kivételt hello általános [Angular.js kódminta](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) egy szögben kifejezett SPA útmutatót


## <a name="see-also"></a>Lásd még:

* toolearn hello képességeit, bővebben lásd: [feltételes hozzáférés az Azure AD](../active-directory-conditional-access.md).
* További Azure AD-mintakódok, lásd: [Github-tárház, kódmintákat](https://github.com/azure-samples?utf8=%E2%9C%93&q=active-directory). 
* További információk a hello ADAL SDK és a hozzáférés hello referenciadokumentációt: [könyvtár útmutató](active-directory-authentication-libraries.md).
* További információ a több-bérlős forgatókönyvek toolearn lásd: [hogyan kell hello több-bérlős minta használatával a felhasználók toosign](active-directory-devhowto-multi-tenant-overview.md).
