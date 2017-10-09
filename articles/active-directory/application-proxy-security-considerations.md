---
title: "az Azure AD-alkalmazásproxy aaaSecurity szempontjai |} Microsoft Docs"
description: "Az Azure AD-alkalmazásproxy használatának biztonsági szempontjait ismerteti"
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: ebd14b9d1fc8f4629c5916e5a910595727d935d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="security-considerations-for-accessing-apps-remotely-with-azure-ad-application-proxy"></a>Biztonsági szempontok az alkalmazások távolról a Azure AD alkalmazásproxy elérése

Ez a cikk azt ismerteti, hogyan nyújt az Azure Active Directory Alkalmazásproxyjával a közzétételéhez, és távolról fér hozzá az alkalmazások biztonságos szolgáltatás.

a következő ábrán látható az Azure AD hello lehetővé teszi, hogy a biztonságos távoli hozzáférés tooyour a helyszíni alkalmazások.

 ![Azure AD alkalmazásproxy segítségével biztonságos távoli hozzáférés ábrája](./media/application-proxy-security-considerations/secure-remote-access.png)

## <a name="security-benefits"></a>Biztonsági szempontból előnyökkel járhat

Az Azure AD-alkalmazásproxy kínál a következő biztonsági szempontból előnyökkel járhat hello:

### <a name="authenticated-access"></a>Hitelesített hozzáférés 

Ha úgy dönt, hogy toouse Azure Active Directory előhitelesítést, majd csak hitelesített kapcsolatokat hozzáférni a hálózathoz.

Az Azure AD-alkalmazásproxy hello Azure AD biztonsági jogkivonat-szolgáltatás (STS) összes hitelesítési támaszkodik.  Előhitelesítés használatakor jellegéből adódóan, blokkolja jelentős számú névtelen támadások, mivel csak hitelesített identitást hello háttér-alkalmazást érheti el.

Ha csatlakoztatott legyen a előhitelesítési módszer, ez a szolgáltatás nem jelenik meg. 

### <a name="conditional-access"></a>Feltételes hozzáférés

Alkalmazása előtt tooyour hálózati létesített kapcsolatok gazdagabb házirend-ellenőrzést.

A [feltételes hozzáférés](active-directory-conditional-access-azuread-connected-apps.md), definiálhat milyen forgalom korlátozásai engedélyezett tooaccess a háttér-alkalmazások. Házirendeket, amelyek korlátozzák a bejelentkezéseket helye, hitelesítési és kockázatú profil alapján is létrehozhat.

Használhatja a feltételes hozzáférés tooconfigure többtényezős hitelesítési házirendek, biztonsági tooyour felhasználói hitelesítések réteget hozzáadása. 

### <a name="traffic-termination"></a>Forgalom megszüntetése

Az összes forgalom hello felhőben megszakad.

Mivel az Azure AD-alkalmazásproxy egy fordított proxy, minden forgalom tooback-a befejezési alkalmazások: hello szolgáltatás megszakad. hello munkamenet is beolvasása újrakezdte csak a hello háttér-kiszolgálófiók, ami azt jelenti, hogy a háttér-kiszolgálók esetén nem kitett toodirect HTTP-forgalmat. Ez a konfiguráció azt jelenti, hogy jobban védve célzott támadásoktól.

### <a name="all-access-is-outbound"></a>Minden hozzáférés meg kimenő 

Nincs szükség a tooopen bejövő kapcsolatok toohello vállalati hálózathoz.

Application Proxy összekötők csak a kimenő kapcsolatok toohello az Azure AD alkalmazásproxy-szolgáltatás, ami azt jelenti, hogy nincs szükség a bejövő kapcsolatok tooopen tűzfalportok használja. Hagyományos proxyk szükséges a szegélyhálózaton (más néven *DMZ*, *demilitarizált zóna*, vagy *végezheti*), és a hozzáférés toounauthenticated kapcsolatok hello hálózati peremhálózaton. Ebben a forgatókönyvben webes alkalmazást érintő számos további beruházásra termékek tooanalyze forgalom tűzfal- és kínálnak hozzáadása védelmi toohello környezet szükséges. A Proxy nincs szükség a szegélyhálózaton, mert az összes kapcsolat kimenő, és egy biztonságos csatornán kerül sor.

Összekötők kapcsolatos további információkért lásd: [megértéséhez Azure AD-alkalmazásproxy összekötők](application-proxy-understand-connectors.md).

### <a name="cloud-scale-analytics-and-machine-learning"></a>A felhőméretű elemzés és a gépi tanulás 

A legmodernebb védelmet beolvasása.

Mivel az Azure Active Directory részét, kihasználhatják a alkalmazásproxy [Azure AD Identity Protection](active-directory-identityprotection.md)a machine learning-alapú eszközintelligencia és a Microsoft Security Response Center hello és milyen adatokat. Együtt azt megelőző a sérült biztonságú fiókjait azonosítják és magas kockázatú bejelentkezések valós idejű védelmet nyújtsanak. Azt is figyelembe számos tényező befolyásolja, például hozzáférés névtelenítését hálózatokon keresztül fertőzött eszközökről és a rendellenes és szokatlan helyekről.

Ezek a jelentések és események számos már elérhető a biztonsági információk és az esemény (SIEM) rendszerek integrációját egy API-n keresztül.

### <a name="remote-access-as-a-service"></a>A távelérés szolgáltatás

Nincs tooworry kapcsolatos karbantartásáért, valamint a helyszíni kiszolgálók javítását.

Továbbra is a nem javított szoftver támadások számos fiókok. Az Azure AD-alkalmazásproxy egy Internet-skálázási szolgáltatás, amely a Microsoft tulajdonában van,, így mindig hello legújabb biztonsági javítások és frissítéseket.

tooimprove hello biztonsági Azure AD alkalmazásproxy által közzétett alkalmazás, az azt blokkolása webes webbejáró robots indexelő és az alkalmazások Archiválás. Minden alkalommal, amikor egy webes webbejáró robot próbálja beolvasni a robot beállításait a közzétett alkalmazások Application Proxy válaszol a robots.txt fájl tartalmazza a `User-agent: * Disallow: /`.

## <a name="under-hello-hood"></a>Hello technikai részletek alapján

Az Azure AD-alkalmazásproxy két részből áll:

* felhőalapú szolgáltatás hello: Ez a szolgáltatás fut az Azure-ban, és ahol hello külső ügyfélfelhasználó kapcsolódnának.
* [hello a helyszíni összekötő](application-proxy-understand-connectors.md): egy a helyszíni összetevő hello összekötő hello Azure AD alkalmazásproxy-szolgáltatás érkező kéréseket figyeli, és kapcsolatok toohello belső alkalmazások kezeli. 

A folyamat hello összekötő és hello alkalmazásproxy-szolgáltatás között létesít során:

* hello összekötő először állítsa be.
* hello összekötő hello alkalmazásproxy-szolgáltatás konfigurációs adatait kéri le.
* Egy felhasználó a közzétett alkalmazás fér hozzá.

>[!NOTE]
>Az összes kommunikáció SSL-en keresztül történik, és mindig származnak: hello összekötő toohello alkalmazásproxy-szolgáltatás. hello szolgáltatás csak akkor beszélünk.

hello összekötő használ az ügyfél tooauthenticate toohello szinte minden hívások alkalmazásproxy-szolgáltatás. hello csak kivétel toothis folyamat lépése hello kezdeti beállítás, ahol hello ügyféltanúsítvány jön létre.

### <a name="installing-hello-connector"></a>Hello-összekötő telepítése

Ha először hello összekötő van beállítva, a következő folyamat eseményeinek hello kerül sor:

1. hello összekötő regisztrációs toohello szolgáltatás hello összekötő hello telepítésének részeként történik. Felhasználók felszólító tooenter vannak az Azure AD rendszergazdai hitelesítő adatait. A jogkivonat a alapján a hitelesítés majd rendelkezésre toohello az Azure AD alkalmazásproxy-szolgáltatás.
2. Alkalmazásproxy-szolgáltatás hello hello token értékeli ki. Azt biztosítja, hogy hello a felhasználó egy vállalati rendszergazda belül, amely a hello token hello bérlői ki. Ha hello felhasználó nem rendszergazda, hello folyamat megszakadt.
3. hello összekötő egy ügyfél tanúsítványkérelmet állít elő, és továbbítja azokat, valamint hello token, toohello alkalmazásproxy-szolgáltatás. hello szolgáltatás pedig hello token ellenőrzi és aláírja hello ügyfél tanúsítványkérelmet.
4. hello összekötő hello ügyféltanúsítvány hello alkalmazásproxy-szolgáltatás jövőbeli kommunikációt használ.
5. hello összekötő hello szolgáltatás használatával az ügyféltanúsítvány hajt végre egy kezdeti lekéréses hello rendszer konfigurációs adatokat, és most már készen áll a tootake kérelmek.

### <a name="updating-hello-configuration-settings"></a>Hello konfigurációs beállításainak frissítése folyamatban

Amikor hello alkalmazásproxy-szolgáltatás frissíti hello konfigurációs beállításait, a következő folyamat eseményeinek hello kerül sor:

1. hello összekötő toohello konfigurációs végpont belül hello alkalmazásproxy-szolgáltatás kapcsolódik az ügyféltanúsítvány használatával.
2. Hello ügyféltanúsítvány érvényesítése után hello alkalmazásproxy-szolgáltatás adja vissza a konfigurációs adatok toohello összekötő (például hello összekötő csoport, amely összekötő hello részének kell lennie).
3. Ha hello jelenlegi tanúsítvány 180 napnál régebbi, hello összekötőt állít elő, egy új tanúsítványkérelmet, és hatékonyan frissül hello ügyféltanúsítvány 180 nap alatt.

### <a name="accessing-published-applications"></a>A közzétett alkalmazások eléréséhez

Amikor a felhasználók hozzáférnek a közzétett alkalmazás, hello következő események kerül sor hello alkalmazásproxy-szolgáltatás és hello alkalmazásproxy-összekötő között:

1. [hello szolgáltatás hitelesíti hello felhasználói hello alkalmazás](#the-service-checks-the-configuration-settings-for-the-app)
2. [hello szolgáltatás kérelmet helyezi hello összekötő várólista](#The-service-places-a-request-in-the-connector-queue)
3. [Összekötő feldolgozza hello kérést hello üzenetsorból](#the-connector-receives-the-request-from-the-queue)
4. [hello összekötő választ vár](#the-connector-waits-for-a-response)
5. [hello szolgáltatás adatfolyamok adatok toohello felhasználó](#the-service-streams-data-to-the-user)

Mi történik, a lépések, kapcsolatos további információkért toolearn tartsa olvasásakor.


#### <a name="1-hello-service-authenticates-hello-user-for-hello-app"></a>1. hello szolgáltatás hitelesíti hello felhasználói hello alkalmazás

Ha a előhitelesítési módszer konfigurált hello app toouse csatlakoztatott, hello ebben a szakaszban kimarad.

Ha hello app toopreauthenticate konfigurálta az Azure ad-vel, felhasználó átirányított toohello az Azure AD STS tooauthenticate, és hello lépések kerül sor:

1. Alkalmazásproxy ellenőrzi a feltételes hozzáférési házirend követelményeinek hello meghatározott alkalmazásokra vonatkozzanak. Ez a lépés biztosítja, hogy a felhasználó hello toohello alkalmazás lett rendelve. Ha szükséges a kétlépéses ellenőrzést, hello hitelesítési feladatütemezési kéri hello egy második hitelesítési módszert.

2. Miután az ellenőrzés sikeres rendelkeznek, hello Azure AD STS állít ki hello alkalmazás aláírt jogkivonat, és átirányítja a felhasználót vissza toohello alkalmazásproxy-szolgáltatás hello.

3. Alkalmazásproxy ellenőrzi, hogy hello token toocorrect hello alkalmazás adta ki. Azt az egyéb-ellenőrzéseket is végez, például győződjön meg arról, hogy hello jogkivonat aláírása Azure ad és, hogy továbbra is érvényes ablak hello belül van-e.

4. Alkalmazásproxy beállítja egy titkosított hitelesítési cookie-k tooindicate hitelesítési toohello alkalmazás történt. hello cookie-k tartalmaz egy lejárati időbélyeg alapuló hello jogkivonatot az Azure AD és más adatok, például hitelesítés hello hello felhasználónév alapul. hello cookie titkosítása és a titkos kulcs ismert csak toohello alkalmazásproxy-szolgáltatás.

5. Application Proxy átirányítások hello felhasználói hátsó toohello eredetileg kért URL-CÍMÉT.

Hello előhitelesítési lépéseket bármely részét nem sikerül, ha hello felhasználói kérelmet a rendszer megtagadja, és hello felhasználó számára megjelenik egy hibaüzenet, amely hello hello probléma forrása.


#### <a name="2-hello-service-places-a-request-in-hello-connector-queue"></a>2. hello szolgáltatás kérelmet helyezi hello összekötő várólista

Összekötők tartsa egy kimenő kapcsolat megnyitása toohello alkalmazásproxy-szolgáltatás. Ha a kérelem érkezik, hello szolgáltatás hello kérelem hello nyitott kapcsolatok hello összekötő toopick be az egyik várólisták.

hello kérelem hello alkalmazások, például hello kérelemfejléc, hello titkosított cookie-ból adatokat szereplő elemeket tartalmaz, akkor hello kérelmet, és hello tétele hello felhasználói kérelem azonosítója. Bár a hello titkosított cookie-ból adatküldést hello kérelemmel, maga hello hitelesítési cookie nincs.

#### <a name="3-hello-connector-processes-hello-request-from-hello-queue"></a>3. hello összekötő folyamatok hello kérelem hello üzenetsorból. 

Hello kérés alapján alkalmazásproxy egyikét hajtja végre a következő műveletek hello:

* Ha hello kérés egyszerű feladat (például nincs belül, és egy RESTful hello törzs adat *beolvasása* kérelem), hello összekötő lehetővé teszi egy kapcsolat toohello belső célerőforrása, és választ vár.

* Ha tartozik-e a hello kérelemhez társított hello törzsében (például egy RESTful *POST* művelet), hello összekötő lehetővé teszi egy kimenő kapcsolat hello ügyfél tanúsítvány toohello proxyval példány használatával. A kapcsolati toorequest hello adatait, és az nyisson meg egy kapcsolati toohello belső erőforrást. Hello-összekötőről származó hello kérelmet kap, miután hello alkalmazásproxy-szolgáltatás hello felhasználói tartalmat elfogadása kezdődik, és toohello adatösszekötő továbbítja. hello összekötő, viszont hello adatok toohello belső erőforrás továbbítja.

#### <a name="4-hello-connector-waits-for-a-response"></a>4. hello összekötő válaszra.

Miután hello kérelmet, és az összes tartalmat toohello továbbításának biztonsági end elkészült, akkor hello összekötő válaszra.

Választ kap, miután hello összekötő lehetővé teszi egy kimenő kapcsolat toohello alkalmazásproxy-szolgáltatás, tooreturn fejléc részletek hello kezdete streaming hello visszatérési adatai.

#### <a name="5-hello-service-streams-data-toohello-user"></a>5. hello szolgáltatás adatfolyamok adatok toohello felhasználó. 

Egyes feldolgozási hello alkalmazás itt fordulhat elő. Ha az alkalmazás a alkalmazásproxy tootranslate fejlécek vagy URL-címek konfigurálta, a feldolgozása történik, ennél a lépésnél szükség szerint.


## <a name="next-steps"></a>Következő lépések

[Hálózati topológia szempontjai az Azure AD-alkalmazásproxy használatával](application-proxy-network-topology-considerations.md)

[Az Azure AD-alkalmazásproxy összekötők ismertetése](application-proxy-understand-connectors.md)
