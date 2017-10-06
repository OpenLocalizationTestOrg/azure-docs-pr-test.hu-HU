---
title: "Active Directory fejlesztői szószedet aaaAzure |} Microsoft Docs"
description: "A gyakran használt Azure Active Directory fejlesztői fogalmak és szolgáltatások feltételei listáját."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 551512df-46fb-4219-a14b-9c9fc23998ba
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: 32a6a6194b8d01409159a2b60263bb66a87a843c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-developer-glossary"></a>Az Azure Active Directory fejlesztői szószedet
Ez a cikk tartalmaz definíciókat néhány hello Azure Active Directory (AD) fejlesztői alapfogalmakat, amelyek hasznosak, ha az Azure AD alkalmazásfejlesztés megismerése.

## <a name="access-token"></a>hozzáférési jogkivonat
Olyan típusú [biztonsági jogkivonat](#security-token) által kiadott egy [engedélyezési server](#authorization-server), és által használt egy [ügyfélalkalmazás](#client-application) a rendelés tooaccess egy [védett erőforrás-kiszolgáló ](#resource-server). Általában a hello formában egy [JSON webes jogkivonat (JWT)][JWT], hello jogkivonat, úgy toohello ügyfél hello által biztosított hello engedélyezési [erőforrás tulajdonosa](#resource-owner), a kért szint a hozzáférés. hello token tartalmazza az összes alkalmazható [jogcímek](#claim) hamarosan hello tulajdonos, hello ügyfél alkalmazás toouse engedélyezése azt egy formája, amelyet egy adott erőforráshoz történő hozzáféréskor hitelesítő adatot. Ez is szükségtelenné hello hello erőforrás tulajdonosa tooexpose hitelesítő adatok toohello ügyfél.

Hozzáférési jogkivonatok egyes esetekben hivatkozott tooas "Felhasználó + alkalmazás" vagy "Alkalmazás csak", attól függően, hogy hello hitelesítő adatok képviselt. Például amikor egy ügyfél-alkalmazás használja a:

* ["Engedélyezési kód" hitelesítésengedélyezési](#authorization-grant), hello végfelhasználói hello erőforrás tulajdonosának először hitelesíti, delegálásának engedélyezési toohello ügyfél tooaccess hello erőforrás. hello ügyfél hitelesíti ezt követően hello hozzáférési jogkivonat beszerzése. hello token néha lehet kifejezetten a "Felhasználó + alkalmazás" tokenként hivatkozott toomore azt jelenti, hogy mindkét hello felhasználói hello ügyfélalkalmazást és hello alkalmazás engedélyezett.
* ["Ügyfél-hitelesítő adatok" hitelesítésengedélyezési](#authorization-grant), hello ügyfél hello egyetlen hitelesítést biztosít, működik-e nélkül hello erőforrás-tulajdonos hitelesítési/engedélyezési, így egyes esetekben lehet hello token hivatkozott tooas egy "Csak alkalmazás" token.

Lásd: [Azure AD jogkivonat-hivatkozást] [ AAD-Tokens-Claims] további részleteket.

## <a name="application-manifest"></a>Az alkalmazásjegyzék
Hello által biztosított szolgáltatás [Azure-portálon][AZURE-portal], hello identitás alkalmazást, a frissítés hozzá van rendelve egy mechanizmusként JSON-ábrázolását állít elő [ Alkalmazás] [ AAD-Graph-App-Entity] és [szolgáltatásnév] [ AAD-Graph-Sp-Entity] entitásokat. Lásd: [ismertetése hello Azure Active Directory alkalmazásjegyzékének] [ AAD-App-Manifest] további részleteket.

## <a name="application-object"></a>alkalmazásobjektum
Ha Ön regisztrálása vagy frissítéséhez hello lévő alkalmazás [Azure-portálon][AZURE-portal], hello portálon létrehoz frissítések alkalmazásobjektum és a hozzá tartozó [szolgáltatás egyszerű objektum](#service-principal-object), hogy a bérlő számára. hello alkalmazásobjektum *meghatározása* identitás alkalmazást hello globálisan (Ha hozzáfér minden bérlők) keresztül, egy sablon, amelynek a megfelelő szolgáltatás egyszerű objektumok elő biztosító  *származtatott* helyileg történő futtatása (az egy adott bérlő) használható.

Lásd: [alkalmazás és szolgáltatás egyszerű objektumok] [ AAD-App-SP-Objects] további információt.

## <a name="application-registration"></a>alkalmazás regisztrálása
A rendezés tooallow egy alkalmazás toointegrate rendelkező és a delegált identitás és hozzáférés-kezelés funkciók tooAzure AD, szerepelnie kell egy Azure AD-val [bérlői](#tenant). Az alkalmazás az Azure ad-vel való regisztrálásakor ad identitáskezelési konfigurálása során az alkalmazáshoz, lehetővé téve az Azure ad-val toointegrate, és használatát, mint:

* Az egyszeri bejelentkezés az Azure AD Identity Management használatával robusztus felügyeletet és [OpenID Connect] [ OpenIDConnect] protokollmegvalósítás
* Hozzáférés túl közvetítőalapú[védett erőforrások](#resource-server) által [ügyfélalkalmazások](#client-application), az Azure AD OAuth 2.0 keresztül [engedélyezési server](#authorization-server) végrehajtása
* [Hozzájárulás keretrendszer](#consent) ügyfél tooprotected erőforrások eléréséhez, erőforrás tulajdonosa engedélyezési alapján kezeléséhez.

Lásd: [alkalmazások integrálása az Azure Active Directory] [ AAD-Integrating-Apps] további részleteket.

## <a name="authentication"></a>Hitelesítés
hello kihívást egy entitás hello időközönként kezeléséről egy identitás- és hozzáférés-vezérlési biztonsági egyszerű toobe létrehozása a jogos hitelesítő adatokat, a folyamat. Során egy [OAuth2 hitelesítésengedélyezési](#authorization-grant) például hello felek hitelesítéséhez vagy hello szerepét tölti [erőforrás tulajdonosa](#resource-owner) vagy [ügyfélalkalmazás](#client-application)attól függően, hogy hello grant használt.

## <a name="authorization"></a>Engedélyezési
hello act amely során egy hitelesített biztonsági lényeges engedély toodo valamit. Hello Azure AD programozási modell két elsődleges használati esetek szerepelnek:

* Során egy [OAuth2 hitelesítésengedélyezési](#authorization-grant) folyamata: amikor hello [erőforrás tulajdonosa](#resource-owner) engedélyezési toohello biztosít [ügyfélalkalmazás](#client-application), így hello ügyfél tooaccess hello erőforrás tulajdonosa erőforrás.
* Hello ügyfél erőforrások elérése során: hello által megvalósított módon [erőforrás-kiszolgáló](#resource-server), hello segítségével [jogcím](#claim) értékek szerepelnek hello [hozzáférési jogkivonat](#access-token) toomake hozzáférés-vezérlés a döntések alapján őket.

## <a name="authorization-code"></a>Engedélyezési kód
Egy rövid "token" élt tooa megadott [ügyfélalkalmazás](#client-application) által hello [engedélyezési végpont](#authorization-endpoint), hello "engedélyezési kód" folyamat részeként a négy OAuth2 valamelyikét hello [engedélyezési biztosít ](#authorization-grant). hello vissza toohello ügyfélalkalmazás a válasz tooauthentication egy [erőforrás tulajdonosa](#resource-owner), a kért erőforrások jelző hello erőforrás tulajdonosa engedélyezési tooaccess hello adta át. Hello folyamat részeként a később beváltott hello kód egy [hozzáférési jogkivonat](#access-token).

## <a name="authorization-endpoint"></a>engedélyezési végpont
Hello által megvalósított hello végpont közül [engedélyezési server](#authorization-server), toointeract használt hello [erőforrás tulajdonosa](#resource-owner) a rendelés tooprovide egy [hitelesítésengedélyezési](#authorization-grant) során az OAuth2 engedélyezési grant flow. Attól függően, hogy hello engedélyezési biztosítani folyamat használja, a tényleges hello megadott grant változhat, beleértve egy [engedélyezési kód](#authorization-code) vagy [biztonsági jogkivonat](#security-token).

Lásd: hello OAuth2 specification [engedélyezési biztosítani típusok] [ OAuth2-AuthZ-Grant-Types] és [engedélyezési végpont] [ OAuth2-AuthZ-Endpoint] szakaszok és hello [OpenIDConnect specification] [ OpenIDConnect-AuthZ-Endpoint] további részleteket.

## <a name="authorization-grant"></a>hitelesítésengedélyezési
Hello képviselő hitelesítő adatot [erőforrás tulajdonosa](#resource-owner) [engedélyezési](#authorization) tooaccess tooa engedélyezett, a védett erőforrásokhoz [ügyfélalkalmazás](#client-application). Egy ügyfélalkalmazás hello egyikét használhatja [négy adja meg az OAuth2 hitelesítési keretrendszer hello által megadott] [ OAuth2-AuthZ-Grant-Types] tooobtain támogatás, attól függően, hogy a típus vagy ügyfélkövetelmények: "hitelesítésengedélyezési kód", " ügyfél hitelesítő adatai megadják","implicit grant"és"erőforrás tulajdonosa jelszó hitelesítő adatai megadják". hello credential visszaadott toohello ügyfél vagy egy [hozzáférési jogkivonat](#access-token), vagy egy [engedélyezési kód](#authorization-code) (kicserélt később a hozzáférési token), attól függően, hogy a használt hitelesítésengedélyezési hello típusú.

## <a name="authorization-server"></a>engedélyezési kiszolgáló
Hello által definiált [OAuth2 engedélyezési keretrendszer][OAuth2-Role-Def], hello server kiadására hozzáférési jogkivonatok toohello [ügyfél](#client-application) a sikeres hitelesítés után Hello [erőforrás tulajdonosa](#resource-owner) és annak engedélyezéséhez. A [ügyfélalkalmazás](#client-application) hello engedélyezési server futásidőben keresztül kommunikál a [engedélyezési](#authorization-endpoint) és [token](#token-endpoint) végpontok hello definiált OAuth2 megfelelően [engedélyezési biztosít](#authorization-grant).

Alkalmazások integrálása az Azure AD hello esetben az Azure AD megvalósítja hello engedélyezési kiszolgálói szerepkör az Azure AD-alkalmazások és a Microsoft service API-k, például [Microsoft Graph API-k][Microsoft-Graph].

## <a name="claim"></a>Jogcímszabály
A [biztonsági jogkivonat](#security-token) jogcímeket, amely helyességi feltételek egy entitást tartalmaz (például egy [ügyfélalkalmazás](#client-application) vagy [erőforrás tulajdonosa](#resource-owner)) (például hello tooanother entitás [erőforrás-kiszolgáló](#resource-server)). Jogcímek olyan név/érték párok, tény olvasható az hello token tulajdonos továbbítása (például hello rendszerbiztonsági hello hitelesített [engedélyezési server](#authorization-server)). hello jogcímeket egy adott jogkivonat függenek több változók, beleértve a hello típusú jogkivonat, hello típusa használt hitelesítő adat tooauthenticate hello tulajdonos, hello Alkalmazáskonfiguráció, stb.

Lásd: [az Azure AD-jogkivonatok referenciájából] [ AAD-Tokens-Claims] további részleteket.

## <a name="client-application"></a>ügyfélalkalmazás
Hello által definiált [OAuth2 engedélyezési keretrendszer][OAuth2-Role-Def], egy alkalmazás, amelynek eredményeképpen a védett erőforrás kérelmek nevében hello [erőforrás tulajdonosa](#resource-owner). "ügyfél" Hello kifejezés nem feltétlenül jelenti azt bármely adott megvalósítási hardverjellemzők (például, hogy hello alkalmazás végrehajtja a kiszolgálón, asztali vagy más eszközök).  

Egy ügyfélalkalmazás kérelmek [engedélyezési](#authorization) az egy az erőforrás-tulajdonos tooparticipate egy [OAuth2 hitelesítésengedélyezési](#authorization-grant) flow, és előfordulhat, hogy adatelérés API-k/hello erőforrás tulajdonosa nevében. az OAuth2 hitelesítési keretrendszer hello [határozza meg az ügyfelek kétféle][OAuth2-Client-Types], "bizalmas" és "nyilvános", hello ügyfél képes toomaintain hello bizalmas mivoltát a hitelesítő adatok alapján. Alkalmazások is létrehozható egy [webes ügyfél (bizalmas)](#web-client) egy webkiszolgálón, amely fut egy [natív ügyfél (nyilvános)](#native-client) telepítve az eszközön, vagy egy [felhasználói ügynök-alapú ügyfél (nyilvános)](#user-agent-based-client)amely fut egy eszköz böngészőjében.

## <a name="consent"></a>Hozzájárulás
hello folyamatán a [erőforrás tulajdonosa](#resource-owner) engedélyezési tooa biztosítása [ügyfélalkalmazás](#client-application), tooaccess védett specifikus alatt lévő erőforrások [engedélyek](#permissions), hello nevében erőforrás tulajdonosa. Attól függően, hogy hello ügyfél által kért hello engedélyek rendszergazda vagy a felhasználó kér a rendszer hozzájárulási tooallow access tootheir szervezet vagy egyéni adatok kulcsattribútumokkal. Vegye figyelembe a egy [több-bérlős](#multi-tenant-application) forgatókönyv, hello alkalmazás [egyszerű](#service-principal-object) hello bérlői hello küldőnek felhasználó is rögzíti.

## <a name="id-token"></a>Azonosító token
Egy [OpenID Connect] [ OpenIDConnect-ID-Token] [biztonsági jogkivonat](#security-token) által biztosított egy [engedélyezési server](#authorization-server) [engedélyezésivégpont](#authorization-endpoint), amely tartalmazza [jogcímek](#claim) kapcsolódó toohello hitelesítési a végfelhasználók [erőforrás tulajdonosa](#resource-owner). Például a hozzáférési tokent, azonosító-jogkivonatokat is képviselt digitális aláírások [JSON webes jogkivonat (JWT)][JWT]. Olyan hozzáférési jogkivonatot eltérően azonban egy azonosító tokent jogcímek nem használják célra kapcsolódó tooresource hozzáférés és a kifejezetten a hozzáférés-vezérlés.

Lásd: [az Azure AD-jogkivonatok referenciájából] [ AAD-Tokens-Claims] további részleteket.

## <a name="multi-tenant-application"></a>több-bérlős alkalmazás
Egy osztály, amely lehetővé teszi, hogy a bejelentkezési kérelem és [hozzájárulás](#consent) bármely Azure AD-ban kiépített felhasználók [bérlői](#tenant), beleértve a bérlők hello, ahol hello ügyfél regisztrálva van egy eltérő. [Natív ügyfél](#native-client) olyan több-bérlős alapértelmezés szerint mivel [webes ügyfél](#web-client) és [webes erőforrás/API-t](#resource-server) alkalmazások rendelkeznek hello képességét tooselect egy vagy több-bérlős között. Ezzel szemben webalkalmazás egyetlen-bérlő regisztrálva, csak révén kiépítve hello ugyanaz, mint egy hello bérlői felhasználói fiókok bejelentkezések ahol hello alkalmazás van regisztrálva.

Lásd: [hogyan bármely Azure AD felhasználói használatával toosign hello több-bérlős alkalmazásminta] [ AAD-Multi-Tenant-Overview] további részleteket.

## <a name="native-client"></a>natív ügyfél
Olyan típusú [ügyfélalkalmazás](#client-application) telepített natív módon az eszközön. Összes kódot az eszközön végrehajtása, mert minősül tooits meggátoló toostore hitelesítő adatok közvetlenül a Microsoftnak/bizalmasan miatt egy "nyilvános" ügyfél. Lásd: [OAuth2-ügyfél meg kell adnia, és profilok] [ OAuth2-Client-Types] további részleteket.

## <a name="permissions"></a>Engedélyek
A [ügyfélalkalmazás](#client-application) kap hozzáférést tooa [erőforrás-kiszolgáló](#resource-server) is deklarálni kell engedélyekre vonatkozó kéréseit. Kétféle érhetők el:

* "Delegált" engedéllyel, amelyet meg [hatókör-alapú](#scopes) hozzáférés meghatalmazott bejelentkezve hello engedélye [erőforrás tulajdonosa](#resource-owner), mint futásidőben toohello erőforrás jelennek ["szolgáltatáskapcsolódási pont "jogcímek](#claim) hello ügyfél [hozzáférési jogkivonat](#access-token).
* Adja meg, az "Alkalmazás" engedélyek [szerepköralapú](#roles) hello ügyfél alkalmazás hitelesítő adatok/identitásával elérni, mint futásidőben toohello erőforrás jelennek ["szerepkörök" jogcímek](#claim) a hello ügyfél-hozzáférési jogkivonat.

Akkor is során hello surface [hozzájárulás](#consent) folyamat jogosultságot ad az hello rendszergazda vagy az erőforrás tulajdonosa hello lehetőség toogrant/megtagadási hello client access tooresources a bérlőben.

Engedélykéréseket vannak konfigurálva a hello "Alkalmazás" / "Beállítások" lapon hello [Azure-portálon][AZURE-portal], a "Szükséges engedélyek", hello kiválasztásával szükséges a "Delegált engedélyek" és " Alkalmazásengedélyek"(ez utóbbi hello igényel hello globális rendszergazda szerepkör tagjának kell lennie). Mivel egy [nyilvános ügyfél](#client-application) biztonságosan nem tudja kezelni a hitelesítő adatokat, csak kérheti delegált jogosultságokkal sikeresen telepítették, amíg egy [bizalmas ügyfél](#client-application) hello képességét toorequest delegált és alkalmazás is van engedélyek. hello ügyfél [alkalmazásobjektum](#application-object) deklarált engedélyek szükségesek a tárolók hello a [requiredResourceAccess tulajdonság][AAD-Graph-App-Entity].

## <a name="resource-owner"></a>erőforrás tulajdonosa
Hello által definiált [OAuth2 engedélyezési keretrendszer][OAuth2-Role-Def], egy entitás képes tooa hozzáférés biztosítása a védett erőforrás. Ha hello erőforrás tulajdonosa személy, hivatkozott tooas felhasználó. Például ha egy [ügyfélalkalmazás](#client-application) tooaccess szeretne a felhasználó postaládájához hello keresztül [Microsoft Graph API][Microsoft-Graph], hello erőforrás tulajdonosa engedélyt igényel hello postaláda.

## <a name="resource-server"></a>erőforrás-kiszolgáló
Hello által definiált [OAuth2 engedélyezési keretrendszer][OAuth2-Role-Def], egy kiszolgálót, hogy a gazdagépek védett erőforrások, képes fogadása és tooprotected erőforrás válaszol kérései [ügyfél alkalmazások](#client-application) , hogy jelen van egy [hozzáférési jogkivonat](#access-token). Más néven egy védett erőforrás-kiszolgáló, vagy erőforrás-alkalmazáshoz.

Egy erőforrás-kiszolgáló API-k mutatja, és érvényesíti a keresztül tooits védett erőforrások elérését [hatókörök](#scopes) és [szerepkörök](#roles), OAuth 2.0 hitelesítési keretrendszer használatával hello. Például hello Azure AD Graph API-hozzáférés tooAzure AD bérlő adatokat, és Office 365 API-t adja meg például a levelezés és a naptár hozzáférés toodata hello biztosít. Mindkét esetben is elérhetők hello [Microsoft Graph API][Microsoft-Graph].  

Csakúgy, mint egy ügyfélalkalmazást, erőforrás identitás alkalmazást létrejön keresztül [regisztrációs](#application-registration) az Azure AD-bérlő, így a hello alkalmazás és a szolgáltatás egyszerű objektum. Egyes Microsoft által biztosított API-k, például az Azure AD Graph API hello előre regisztrált szolgáltatásnevekről kiépítése során minden bérlők elérhetik.

## <a name="roles"></a>roles
Például [hatókörök](#scopes), szerepkörök teszik lehetővé az egy [erőforrás-kiszolgáló](#resource-server) toogovern hozzáférés tooits védett erőforrásokhoz. Két típusa van: "user" szerepkör szerepköralapú hozzáférés-vezérlés valósítja meg, az access toohello erőforrás, amíg megvalósítja az "alkalmazás" szerepet igénylő felhasználók/csoportok esetében azonos hello [ügyfélalkalmazások](#client-application) , amelyek hozzáférést igényelnek.

Szerepkörök olyan erőforrás-definiált karakterláncok (például "költség jóváhagyó", "Csak Olvasás", "Directory.ReadWrite.All"), a hello felügyelt [Azure-portálon] [ AZURE-portal] keresztül hello erőforrás [alkalmazás manifest](#application-manifest), és hello erőforrás tárolt [appRoles tulajdonság][AAD-Graph-Sp-Entity]. hello Azure-portálon egyben használt tooassign felhasználók túl "user" szerepköröket, és konfigurálja az ügyfél [Alkalmazásengedélyek](#permissions) tooaccess "alkalmazás" szerepet.

Hello alkalmazás szerepkörök az Azure AD Graph API által elérhetővé tett részletes tárgyalását lásd: [Graph API-Engedélyhatókörök][AAD-Graph-Perm-Scopes]. A részletes megvalósítási példa, lásd: [szerepköralapú hozzáférés-vezérlés az Azure AD felhőalapú alkalmazások][Duyshant-Role-Blog].

## <a name="scopes"></a>Hatókörök
Például [szerepkörök](#roles), hatókörök teszik lehetővé az egy [erőforrás-kiszolgáló](#resource-server) toogovern hozzáférés tooits védett erőforrásokhoz. Hatókörök: használt tooimplement [hatókör-alapú] [ OAuth2-Access-Token-Scopes] a hozzáférés-vezérlés, a [ügyfélalkalmazás](#client-application) , amelyek adott delegált hozzáférést toohello erőforrás tulajdonosa.

Hatókörök olyan erőforrás-definiált karakterláncok (például "Mail.Read", "Directory.ReadWrite.All"), a hello felügyelt [Azure-portálon] [ AZURE-portal] keresztül hello erőforrás [alkalmazásjegyzék](#application-manifest), és hello erőforrás tárolt [oauth2Permissions tulajdonság][AAD-Graph-Sp-Entity]. hello Azure-portálon egyben használt tooconfigure ügyfélalkalmazás [delegált engedélyekkel](#permissions) tooaccess hatókör.

Bevált gyakorlat elnevezési szabályokat alkalmaz, toouse "resource.operation.constraint" formátumban. Az Azure AD Graph API által elérhetővé tett hello hatókörök részletes leírását, lásd: [Graph API-Engedélyhatókörök][AAD-Graph-Perm-Scopes]. A hatókörök jelennek meg, ha az Office 365-szolgáltatásokhoz, lásd: [Office 365 API-engedélyek referencia][O365-Perm-Ref].

## <a name="security-token"></a>biztonsági jogkivonat
Jogcímek, például egy OAuth2 token vagy a SAML 2.0 helyességi feltételt tartalmazó aláírt dokumentumot. Az egy oauth2 protokollt használó [hitelesítésengedélyezési](#authorization-grant), egy [hozzáférési jogkivonat](#access-token) (OAuth2) és egy [azonosító Token](http://openid.net/specs/openid-connect-core-1_0.html#IDToken) különböző típusú biztonsági jogkivonatokat, amelyek mindegyikét használják, mint egy [JSON Webalkalmazás-jogkivonat (JWT)][JWT].

## <a name="service-principal-object"></a>szolgáltatás egyszerű objektum
Ha Ön regisztrálása vagy frissítéséhez hello lévő alkalmazás [Azure-portálon][AZURE-portal], hello portálon létrehoz frissítések is egy [alkalmazásobjektum](#application-object) és a megfelelő egyszerű szolgáltatásnév a bérlő objektum. hello alkalmazásobjektum *meghatározása* identitás alkalmazást hello globálisan (között ahol hello társított alkalmazás hozzáférést kapott minden bérlők), és hello sablon, amelynek a megfelelő szolgáltatáshoz egyszerű objektumok vannak *származtatott* helyileg történő futtatása (az egy adott bérlő) használható.

Lásd: [alkalmazás és szolgáltatás egyszerű objektumok] [ AAD-App-SP-Objects] további információt.

## <a name="sign-in"></a>bejelentkezés
hello folyamatán a [ügyfélalkalmazás](#client-application) kapcsolódó állapota, hello célja az beszerzése a felhasználói hitelesítést kezdeményező, és rögzítése egy [biztonsági jogkivonat](#security-token) és hello alkalmazás munkamenet toothat hatókör állapot. Összetevők, például a felhasználói profillal kapcsolatos információk is tartalmazhat, és információt származó jogkivonat jogcímek állapotban.

hello bejelentkezés egy alkalmazás függvény általánosan használt tooimplement egyszeri bejelentkezéses (SSO). Azt is előz "előfizetés" függvény, a végfelhasználói toogain hozzáférés tooan alkalmazás (után első bejelentkezés) hello belépési pontként. hello regisztrációs függvény használt toogather és további állapot adott toohello felhasználói maradnak, és előfordulhat, hogy [felhasználói hozzájárulás](#consent).

## <a name="sign-out"></a>Kijelentkezés
hello nem hitelesítő folyamatán a felhasználó hello felhasználói állapot hello társított leválasztása [ügyfélalkalmazás](#client-application) munkamenet során [-bejelentkezés](#sign-in)

## <a name="tenant"></a>Bérlői
Az Azure AD-címtár egy példány hivatkozott tooas az Azure AD-bérlő. Funkciót, beleértve a különböző tartalmazza:

* a beállításjegyzék szolgáltatás integrált alkalmazások
* hitelesítés a felhasználói fiókoknak és a társított alkalmazások
* REST-végpontok szükséges toosupport különböző protokollok, beleértve az OAuth2 és SAML, beleértve a hello [engedélyezési végpont](#authorization-endpoint), [token-végpont](#token-endpoint) és "általános" végpont által használt hello [ több-bérlős alkalmazásokhoz](#multi-tenant-application).

A bérlő is hozzá rendelve egy Azure AD-val vagy Office 365-előfizetéssel hello előfizetés Identity & Access Management szolgáltatásokat nyújtó hello előfizetés kiépítése során. Lásd: [hogyan tooget egy Azure Active Directory-bérlőazonosítóra] [ AAD-How-To-Tenant] kaphat hozzáférést tooa különböző módokon bérlői hello leírását. Lásd: [kapcsolódnak hogyan Azure-előfizetések az Azure Active Directory] [ AAD-How-Subscriptions-Assoc] előfizetések és az Azure AD-bérlő hello kapcsolatát leírását.

## <a name="token-endpoint"></a>token-végpont
Hello által megvalósított hello végpont közül [engedélyezési server](#authorization-server) toosupport OAuth2 [engedélyezési biztosít](#authorization-grant). Attól függően, hogy hello grant használt tooacquire lehet egy [hozzáférési jogkivonat](#access-token) (és a kapcsolódó "frissítés" token) tooa [ügyfél](#client-application), vagy [azonosító token](#ID-token) hello használata esetén [ OpenID Connect] [ OpenIDConnect] protokoll.

## <a name="user-agent-based-client"></a>Felhasználói ügynök-alapú ügyfél
Olyan típusú [ügyfélalkalmazás](#client-application) , amely kód webkiszolgálókról és hajtanak végre belül egy felhasználói ügynök (például egy webes böngésző), például egy egyetlen oldal alkalmazás (SPA) fogunk. Összes kódot az eszközön végrehajtása, mert minősül tooits meggátoló toostore hitelesítő adatok közvetlenül a Microsoftnak/bizalmasan miatt egy "nyilvános" ügyfél. Lásd: [OAuth2-ügyfél meg kell adnia, és profilok] [ OAuth2-Client-Types] további részleteket.

## <a name="user-principal"></a>egyszerű felhasználónév
Toohello szolgáltatáshoz hasonlóan a szolgáltatás egyszerű objektum értéke használt toorepresent egy alkalmazáspéldányt, a felhasználó elsődleges objektumot egy rendszerbiztonsági tagot, amely képviseli a felhasználót egy másik típusú. Azure AD Graph hello [felhasználói entitás] [ AAD-Graph-User-Entity] meghatározása hello séma egy felhasználói objektum, beleértve a felhasználóval kapcsolatos tulajdonságait, például utónév és Vezetéknév, felhasználó egyszerű felhasználóneve, directory szerepköri tagság, stb. Ez lehetővé teszi a hello felhasználói identitás konfigurációját az Azure AD tooestablish a felhasználó egyszerű futásidőben. hello egyszerű használt toorepresent az egyszeri bejelentkezést, hitelesített felhasználó felvétel [hozzájárulás](#consent) delegálás, így a hozzáférés-vezérlési döntéseket stb.

## <a name="web-client"></a>webes ügyfél
Olyan típusú [ügyfélalkalmazás](#client-application) , amely végrehajtja az összes kód egy webkiszolgálón, és képes toofunction "bizalmas" ügyfélként biztonságosan tárolja a hitelesítő adatok hello kiszolgálón. Lásd: [OAuth2-ügyfél meg kell adnia, és profilok] [ OAuth2-Client-Types] további részleteket.

## <a name="next-steps"></a>Következő lépések
Hello [Azure AD fejlesztői útmutató] [ AAD-Dev-Guide] hello portál toouse minden Azure AD-fejlesztési van kapcsolódó témakörök, valamint áttekintést [alkalmazásintegráció] [ AAD-How-To-Integrate] és hello alapjait [az Azure AD-alapú hitelesítés és a támogatott hitelesítési forgatókönyvek][AAD-Auth-Scenarios].

A következő megjegyzések szakasz tooprovide visszajelzés hello használata, és segítenek pontosítsa, valamint a tartalom, például a kérelmekről új definíciók vagy meglévőket frissítése!

<!--Image references-->

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-Subscriptions-Assoc]: ../active-directory-how-subscriptions-associated-directory.md
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-How-To-Tenant]: active-directory-howto-tenant.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Multi-Tenant-Overview]: active-directory-devhowto-multi-tenant-overview.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AZURE-portal]: https://portal.azure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[Microsoft-Graph]: https://graph.microsoft.io
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Endpoint]: https://tools.ietf.org/html/rfc6749#section-3.1
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-AuthZ-Endpoint]: http://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
