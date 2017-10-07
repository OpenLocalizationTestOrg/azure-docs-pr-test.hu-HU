---
title: "Azure AD Connect szinkronizálása: Understanding hello alapértelmezett konfiguráció |} Microsoft Docs"
description: "Ez a cikk ismerteti az Azure AD Connect szinkronizálási szolgáltatás hello alapértelmezett konfigurációt."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: ed876f22-6892-4b9d-acbe-6a2d112f1cd1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 1cf95759af913cad8a1393839d3a836434e7c9bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-understanding-hello-default-configuration"></a>Azure AD Connect szinkronizálása: Understanding hello alapértelmezett konfigurációja
Ez a cikk ismerteti a hello out-of-box konfigurációs szabályok. Ez a dokumentumok hello szabályok és hogyan befolyásolja az ezeket a szabályokat a hello konfigurációs. Azt is bemutatja, hogyan hello alapértelmezett beállítása az Azure AD Connect szinkronizálási szolgáltatás. hello célja, hogy hello olvasó együttműködik a hello konfigurációs modell nevű deklaratív kiépítés, egy valós példában alakulását. Ez a cikk feltételezi, hogy már telepített, és állítsa be az Azure AD Connect sync hello telepítővarázslóval.

hello konfigurációs modell, olvassa el a toounderstand hello részleteit [ismertetése deklaratív kiépítés](active-directory-aadconnectsync-understanding-declarative-provisioning.md).

## <a name="out-of-box-rules-from-on-premises-tooazure-ad"></a>A helyszíni tooAzure AD Out-of-box szabályok
hello következő kifejezések található hello out-of-box konfigurációban.

### <a name="user-out-of-box-rules"></a>Felhasználói out-of-box szabályok
Ezek a szabályok is vonatkoznak toohello iNetOrgPerson objektumtípus.

Olyan felhasználói objektum meg kell felelnie a következő szinkronizált toobe hello:

* Rendelkeznie kell egy sourceAnchor.
* Hello után az objektum az Azure ad-ben létrejött, majd a sourceAnchor nem módosítható. Ha hello értéke megváltozott a helyszíni, hello objektum nem szinkronizálása amíg hello sourceAnchor hátsó tooits előző érték nem módosítják.
* Kell rendelkeznie hello accountEnabled (userAccountControl) attribútum feltöltve. A helyszíni Active Directory, az ezt az attribútumot mindig jelen és ki van töltve.

a következő felhasználói objektumok hello vannak **nem** tooAzure AD szinkronizálva:

* `IsPresent([isCriticalSystemObject])`. Sok out-of-box objektumok az Active Directoryban, például hello beépített rendszergazdai fiók érdekében nincsenek szinkronizálva.
* `IsPresent([sAMAccountName]) = False`. Győződjön meg arról, nincs sAMAccountName attribútummal rendelkező felhasználói objektumok nincsenek szinkronizálva. Ebben az esetben csak gyakorlatilag történne NT4 verzióról frissített tartományban.
* `Left([sAMAccountName], 4) = "AAD_"`, `Left([sAMAccountName], 5) = "MSOL_"`. Ne szinkronizáljon az Azure AD Connect szinkronizálási szolgáltatás és a korábbi verziói által használt szolgáltatásfiók hello.
* Csatlakoztatás nem működik az Exchange Online Exchange-fiókok nem szinkronizálja.
  * `[sAMAccountName] = "SUPPORT_388945a0"`
  * `Left([mailNickname], 14) = "SystemMailbox{"`
  * `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`
  * `(Left([sAMAccountName], 4) = "CAS_" && (InStr([sAMAccountName], "}")> 0))`
* Csatlakoztatás nem működik az Exchange Online-objektumok nem szinkronizálja.
  `CBool(IIF(IsPresent([msExchRecipientTypeDetails]),BitAnd([msExchRecipientTypeDetails],&H21C07000) > 0,NULL))`  
  A következő objektumok hello bitmaszk (& H21C07000) lenne szűrheti:
  * Levelezési nyilvános mappa
  * Rendszer kísérő postaláda
  * Postaláda-adatbázis postaláda (rendszer postaláda)
  * Az univerzális biztonsági csoport (egy felhasználó nem tudnák alkalmazni, de nincs örökölt összetevők miatt)
  * Nem-univerzális csoport (egy felhasználó nem tudnák alkalmazni, de nincs örökölt összetevők miatt)
  * Postaláda-terv
  * Felderítési postaláda
* `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Minden replikációs áldozata objektumok nem szinkronizálja.

a következő attribútum szabályok hello vonatkoznak:

* `sourceAnchor <- IIF([msExchRecipientTypeDetails]=2,NULL,..)`. hello sourceAnchor attribútum nem része volt, a csatolt postaládából. Feltételezzük, hogy ha a hivatkozott postafiókkal talált hello tényleges fiók egy tartományhoz később.
* Exchange kapcsolódó attribútumok csak vannak szinkronizálva, ha hello attribútum **mailNickName** értéke.
* Ha több erdő, majd attribútumok során sorrend hello:
  1. Toosign a kapcsolódó attribútumok (a példa userPrincipalName) közzé hello erdőből engedélyezett fiókkal.
  2. Az Exchange-postaládával hello erdőből közzé attribútumokat, amelyek az Exchange globális Címlista (globális címlista) található.
  3. Ha nincs postaláda található, majd ezek az attribútumok származhatnak bármely erdőben.
  4. Exchange kapcsolódó attribútumok (nem látható a globális Címlista hello műszaki attribútumok) közzé hello erdőből ahol `mailNickname ISNOTNULL`.
  5. Ha több erdő, amely akkor elégíti ki a szabályokat, majd hello létrehozása (dátum/idő) hello összekötők (erdők) sorrendje használt toodetermine melyik erdő hozzájárul hello attribútumok.

### <a name="contact-out-of-box-rules"></a>Lépjen kapcsolatba az out-of-box szabályok
Egy partner objektuma meg kell felelnie a következő szinkronizált toobe hello:

* hello forduljon levelezési kell lennie. A következő szabályok hello igazolható:
  * `IsPresent([proxyAddresses]) = True)`. hello proxyAddresses attribútum ki kell tölteni.
  * Egy elsődleges e-mail címét vagy hello proxyAddresses vagy hello levél attribútum találhatók. hello jelenléte egy @ van használt tooverify, hogy hello tartalma egy e-mail címet. A két szabályokat kiértékelt tooTrue kell lennie.
    * `(Contains([proxyAddresses], "SMTP:") > 0) && (InStr(Item([proxyAddresses], Contains([proxyAddresses], "SMTP:")), "@") > 0))`. Van-e bejegyzés "SMTP:", és ha nincs, egy @ található hello karakterláncban?
    * `(IsPresent([mail]) = True && (InStr([mail], "@") > 0)`. Hello feltöltve levél attribútum, és ha igen, képes egy @ található hello karakterláncban?

a következő kapcsolattartási objektumok hello vannak **nem** tooAzure AD szinkronizálva:

* `IsPresent([isCriticalSystemObject])`. Győződjön meg arról, lássa el kritikus jelzéssel kapcsolattartási objektumok szinkronizálja a rendszer. Nem lehet bármely, az alapértelmezett beállításokkal.
* `((InStr([displayName], "(MSOL)") > 0) && (CBool([msExchHideFromAddressLists])))`.
* `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`. Ezek az objektumok az Exchange Online nem működik.
* `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Minden replikációs áldozata objektumok nem szinkronizálja.

### <a name="group-out-of-box-rules"></a>Csoport out-of-box szabályok
Egy objektum meg kell felelnie a következő szinkronizált toobe hello:

* 50 000-nél kevesebb tagot kell rendelkeznie. Ez a szám nem hello a helyi csoportnak a tagjai hello száma.
  * Ha több tagja van, mielőtt megkezdődik az hello először, hello csoport nincs szinkronizálva.
  * Ha a tagok száma hello a méretének növelése, ha az eredetileg hoztak létre, majd ha 50 000 tagok leállítja szinkronizálása, addig, amíg újra 50 000-nél alacsonyabb hello tagsági száma nem éri.
  * Megjegyzés: hello 50 000 tagsági száma is kényszeríti ki az Azure AD. Még nem tud toosynchronize csoportok több taggal rendelkező akkor is, ha módosítja vagy szabály eltávolításához.
* Ha hello csoport egy **terjesztési csoport**, majd mail engedélyezve kell lennie. Lásd: [out-of-box szabályok forduljon](#contact-out-of-box-rules) esetében ez a szabály érvényesül.

a következő objektumok hello vannak **nem** tooAzure AD szinkronizálva:

* `IsPresent([isCriticalSystemObject])`. Sok out-of-box objektumok az Active Directoryban, például hello beépített Rendszergazdák csoport érdekében nincsenek szinkronizálva.
* `[sAMAccountName] = "MSOL_AD_Sync_RichCoexistence"`. A DirSync által használt örökölt csoport.
* `BitAnd([msExchRecipientTypeDetails],&amp;H40000000)`. Szerepkör-csoport.
* `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Minden replikációs áldozata objektumok nem szinkronizálja.

### <a name="foreignsecurityprincipal-out-of-box-rules"></a>ForeignSecurityPrincipal out-of-box szabályok
FSP túl "any" tartományhoz csatlakoztatott (\*) hello metaverse objektumban. A valóságban ezt az összekapcsolást csak akkor fordul elő, a felhasználók és biztonsági csoportok. Ez a konfiguráció biztosítja a erdők közötti tagságát feloldani és képviselt megfelelő Azure AD-ben.

### <a name="computer-out-of-box-rules"></a>Számítógép out-of-box szabályok
Számítógép-objektumot meg kell felelnie a következő szinkronizált toobe hello:

* `userCertificate ISNOTNULL`. Csak Windows 10-es számítógépeken feltöltése ehhez az attribútumhoz. Ez az attribútum egy értéket az összes számítógép-objektumok szinkronizálva.

## <a name="understanding-hello-out-of-box-rules-scenario"></a>Hello out-of-box szabályokat forgatókönyvben ismertetése
Ebben a példában egy fiókerdő (A) a telepítésben, egy Erőforráserdő (R) és egy Azure AD-címtár használunk.

![Forgatókönyv leírása a kép](./media/active-directory-aadconnectsync-understanding-default-configuration/scenario.png)

Ebben a konfigurációban feltételezzük hello fiók erdő engedélyezett fióknak és a hivatkozott postafiókkal hello erőforrás erdő letiltott fiók.

Hello alapértelmezett konfigurációval célunk:

* Toosign a kapcsolódó attribútumok hello engedélyezett fiókkal hello erdőből szinkronizálva.
* Itt található: hello globális Címlista (globális címlista) attribútumok hello postaládával hello erdőből szinkronizálva. Ha nincs postaláda található, más erdőkben szolgál.
* Ha talál egy hivatkozott postafiókkal, hello csatolt engedélyezett fiókot kell található hello objektum exportált toobe tooAzure AD.

### <a name="synchronization-rule-editor"></a>Szinkronizálási szabály szerkesztő
hello konfigurációs tekinthetők meg és szinkronizálási szabályok Editor (SRE) hello eszközzel megváltozott, és egy helyi tooit hello start menüben található.

![Szinkronizálási szabályok szerkesztő ikon](./media/active-directory-aadconnectsync-understanding-default-configuration/sre.png)

hello SRE resource kit eszköz, az Azure AD Connect szinkronizálási szolgáltatás telepítve van. toobe képes toostart, hello ADSyncAdmins csoport tagjának kell lennie. Ha kezdődik, megjelenik az alábbihoz hasonló:

![Bejövő szinkronizálási szabály](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

Az ezen az ablaktáblán tekintse meg a konfigurációs létrehozott összes szinkronizálási szabályait. Hello táblázat minden sora egy szinkronizálási szabály. toohello szabálytípusok, két különböző felsorolt hello a bal oldali: bejövő és kimenő. Bejövő és kimenő hello metaverse hello nézetéből. A hello főként folyamatos toofocus áll bejövő szabályok a áttekintésében. hello tényleges szinkronizálási szabályok listáját az Active Directory észlelte hello séma függ. A fenti hello kép (fabrikamonline.com) erdő nem rendelkezik a szolgáltatások, például Exchange és a Lync és szinkronizálási szabályait nem hello fiók ezen szolgáltatások jött létre. Hello Erőforráserdő (res.fabrikamonline.com) a szinkronizálási szabályok keresése a szolgáltatások. hello szabályok hello tartalma különböző észlelt hello verziójától függően. Például az Exchange 2013-as központi telepítés nincsenek további attribútumfolyamok, mint az Exchange 2010/2007 konfigurálva.

### <a name="synchronization-rule"></a>Szinkronizálási szabály
A szinkronizálási szabály attribútumok kifelé áramló, ha egy feltétel teljesül-e a konfigurációs objektum. Egyúttal az objektumok a kapcsolódási térbe Mitől hello metaverse néven kapcsolódó tooan objektum használt toodescribe **illesztési** vagy **megfelelő**. hello szinkronizálási szabályok arról, hogyan kapcsolódnak-e más tooeach sorrend értékűnek lennie. A szinkronizálási szabály alacsonyabb numerikus értéke magasabb prioritással rendelkezik, és attribútum folyamata ütköznek, magasabb sorrend wins hello ütközésének feloldása.

Tegyük fel, nézze meg hello szinkronizálási szabály **a az AD-felhasználó AccountEnabled**. Ez a sor hello SRE jel, és válassza ki **szerkesztése**.

Mivel ez a szabály az out-of-box szabály, akkor a rendszer figyelmeztetést hello szabály megnyitásakor. Nem szabad módosítani a [tooout beépített szabályok módosítása](active-directory-aadconnectsync-best-practices-changing-default-configuration.md), így a rendszer kéri, hogy Mik azok a céljaira. Ebben az esetben csak szeretné tooview hello szabály. Válassza ki **nem**.

![Szinkronizálási szabályok figyelmeztetés](./media/active-directory-aadconnectsync-understanding-default-configuration/warningeditrule.png)

A szinkronizálási szabály rendelkezik négy konfigurációs szakasz: hatókörére szűrő illesztési szabályok és átalakítások ismertetését.

#### <a name="description"></a>Leírás
hello első a témakör alapvető információkat, például a nevét és leírását.

![Leírás szinkronban szabály szerkesztő lap ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruledescription.png)

Adatokat kaphat arról, hogy mely Ez a szabály vonatkozik, amelyek hello objektumtípusra csatlakoztatott rendszer csatlakoztatott rendszer vonatkozik, és hello metaverzum-objektum típusaként. metaverzum-objektum típusaként hello esetén mindig függetlenül személy hello Forrásobjektum-típust adott felhasználó, iNetOrgPerson, vagy lépjen kapcsolatba. metaverzum-objektum típusaként hello soha nem kell módosítani, így azt a program létrehoz egy általános típus. hello kapcsolattípus tooJoin, StickyJoin vagy rendelkezés állítható be. Ez a beállítás hello csatlakozás szabályok szakasz együtt működik, és később vonatkozik.

Láthatja, hogy a szinkronizálási szabály használja a jelszó-szinkronizálást. Ha egy felhasználó a szinkronizálási szabály hatóköre, hello jelszó szinkronizálása a helyszíni toocloud (feltéve, hogy engedélyezte a jelszó-szinkronizálási szolgáltatás hello).

#### <a name="scoping-filter"></a>Hatókör-beállítási szűrője
hatókör-beállítási szűrője szakasz hello használt tooconfigure esetén egy szinkronizálási szabály vonatkozik. Hello nézi szinkronizálási szabály hello nevét jelzi, hogy csak az engedélyezett felhasználók kell alkalmazni, mivel a hello-hatókör konfigurálva lett-e Igen hello AD attribútum **userAccountControl** kell nem rendelkezik hello bitje 2. Ha hello szinkronizálási motor AD egy felhasználó talál, a szinkronizálás érvényes szabály **userAccountControl** toohello decimális 512 (engedélyezett normál felhasználói) van beállítva. Nem alkalmazható hello szabály amikor hello felhasználó rendelkezik-e **userAccountControl** too514 beállítása (letiltott normál felhasználói).

![Szinkronizálási szabály szerkesztőben tartalmazó lap ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilter.png)

hello tartalmazó szűrő csoportok és záradékokat, amelyek egymásba ágyazható rendelkezik. A szinkronizálási szabály tooapply csoportban lévő összes záradékok teljesülniük kell. Ha több csoport vannak definiálva, majd legalább egy csoportot kell teljesülnie hello szabály tooapply. Ez azt jelenti, hogy logikai vagy csoportok és a logikai között történik, és ki lesz értékelve a csoporton belül. Ezt a konfigurációt egy példa található hello kimenő szinkronizálási szabály **tooAAD – csoport csatlakozás kimenő**. Több szinkronizálási szűrő csoportok, például egy, a biztonsági csoportok (`securityEnabled EQUAL True`) és egy terjesztési csoportok (`securityEnabled EQUAL False`).

![Szinkronizálási szabály szerkesztőben tartalmazó lap ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilterout.png)

Ez a szabály szerepel a használt toodefine, amely csoportok kiosztott tooAzure AD kell lennie. Terjesztési csoportok az Azure ad-vel szinkronizált engedélyezése levelezési toobe kell lennie, de a biztonsági csoportok egy e-mailt nincs szükség.

#### <a name="join-rules"></a>Csatlakozás a szabályok
hello harmadik szakasz használt tooconfigure milyen objektumokat a kapcsolódási térbe hello áll a hello metaverse tooobjects. hello szabály ellenőrizte korábban nem rendelkezik a csatlakozás szabályok beállításra, helyette a folyamatos toolook áll **a az AD-felhasználó csatlakozás**.

![Szinkronizálási szabály szerkesztő csatlakozott szabályai lap ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulejoinrules.png)

hello illesztési szabály hello tartalma hello telepítővarázsló kiválasztott lehetőségnek megfelelő hello függ. Egy bejövő forgalomra vonatkozó szabály hello értékelési kezdődik-e az objektumok hello adatforrás kapcsolódási térbe és hello illesztési szabályok szereplő valamennyi csoport sorrendben történik. Ha egy adatforrás-objektum van kiértékelt toomatch pontosan egy objektum a hello metaverse hello illesztési szabályok egyikének használatával, hello objektumok kapcsolódnak-e. Ha az összes szabály értékelése, és nincs egyezés, hello leírását tartalmazó lapon a kapcsolódás típusa hello szolgál. Ha ez a konfiguráció túl**rendelkezés**, majd egy új objektum hello célkiszolgálón, hello metaverse jön létre. egy új objektum toohello metaverse túl van más néven tooprovision**projekt** egy objektum toohello metaverse.

hello illesztési szabályok csak egyszer kell kiértékelni. Ha egy összekötő terület objektum és a metaverzum-objektum kapcsolódik, akkor azok maradniuk illesztett hello hello szinkronizálási szabály hatóköre továbbra is eleget.

Szinkronizálási szabályok kiértékelésekor meghatározott illesztési szabályok csak egy szinkronizálási szabály hatókörében kell lennie. Ha több szinkronizálási szabályait illesztési szabályok egy adott objektum található, hiba történt. Emiatt hello érdemes csak egy szinkronizálási szabály illesztési meghatározni, ha több szinkronizálási szabályok szerepelnek az objektum hatóköre toohave. Hello out-of-box konfigurációban az Azure AD Connect szinkronizálási szolgáltatás, ezek a szabályok hello neve megtekintésével található és található rendelkező hello word **csatlakozás** hello végén lévő hello nevét. A megadott illesztési szabályok nélkül szinkronizálási szabály hello attribútumfolyamok érvényes, ha egy másik szinkronizálási szabály hello objektumok összekapcsolódva, vagy kiépített hello cél új objektumot.

A fenti hello kép tekinti meg, ha adott hello szabály próbál toojoin láthatja **objectSID** rendelkező **msExchMasterAccountSid** (Exchange) és **msRTCSIP-OriginatorSid** () Lync), vagyis felel meg az elvártnak a fiók-erőforrás erdő topológiájában. Megkeresi hello ugyanaz a szabály az összes erdőben. hello feltételezi, hogy minden erdőben vagy egy fiókot, vagy az erőforrás erdő lehet. Ez a konfiguráció is működik, ha rendelkezik, az adott erdő live, és nincs csatlakoztatva toobe felhasználói.

#### <a name="transformations"></a>Átalakítás
hello transzformációs szakaszából határozza meg az összes attribútumfolyamok toohello célobjektum alkalmazhassa hello objektumok tartományhoz csatlakoztatott és hello hatókör szűrő teljesül-e. Ha visszalép toohello **a az AD-felhasználó AccountEnabled** szinkronizálási szabály található a következő átalakítások hello:

![Átalakítások szinkronban szabály szerkesztő lap ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruletransformations.png)

tooput ezt a konfigurációt, fiók-erőforrás-erdő telepítés, a környezetben várt toofind hello fiók erdő engedélyezett fióknak és a letiltott fiók hello erőforrás erdő Exchange és a Lync beállításokkal. Szinkronizálási szabály nézi hello a bejelentkezéshez szükséges hello attribútumokat tartalmaz, és ezek az attribútumok kell haladjanak hello erdő ahol van egy engedélyezett fiók. Ezek attribútumfolyamok alkotják, több szinkronizálási szabály.

Átalakítás is rendelkezik különböző: állandó, közvetlen és kifejezés.

* Az adatáramlás mindig forgalomáramlás szoftveresen kötött érték. A fenti hello esetében, mindig egy hello értéket **igaz** hello metaverzum-attribútum neve a **accountEnabled**.
* A közvetlen áramlását mindig forgalomáramlás hello forrás toohello target attribútummal, hello attribútumának értéke hello-van.
* hello harmadik adatfolyam típusú kifejezést, és lehetővé teszi a speciális konfigurációk.

hello kifejezés nyelvi VBA (Visual Basic for Applications), ezért a Microsoft Office élménye rendelkező személyek vagy VBScript felismeri hello formátumban. Attribútumok szögletes zárójelbe [attributeName] parancsfájlblokkjában találhatók. Attribútumnevek és függvény nevek kis-és nagybetűket, de hello szinkronizálási szabályok szerkesztő hello kifejezések kiértékeli, és adja meg a figyelmeztetést, ha a hello kifejezés érvénytelen. Összes kifejezés szerint van megadva, a beágyazott függvények külön sorba. tooshow hello power hello konfigurációs nyelv, ez pwdLastSet, de további megjegyzésekkel beszúrni hello folyamata:

```
// If-then-else
IIF(
// (hello evaluation for IIF) Is hello attribute pwdLastSet present in AD?
IsPresent([pwdLastSet]),
// (hello True part of IIF) If it is, then from right tooleft, convert hello AD time format tooa .Net datetime, change it toohello time format used by Azure AD, and finally convert it tooa string.
CStr(FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")),
// (hello False part of IIF) Nothing toocontribute
NULL
)
```

Lásd: [ismertetése deklaratív kiépítés kifejezések](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) hello kifejezés nyelvi attribútumfolyamokhoz olvashat.

### <a name="precedence"></a>Sorrend
Most nézett néhány egyes szinkronizálási szabályait, de hello szabályok hello konfigurációs működnek együtt. Bizonyos esetekben egy attribútumérték része volt a több szinkronizálási szabályok toohello azonos target attribútuma. Attribútumsorrend ebben az esetben, mely attribútum wins használt toodetermine. Tegyük fel nézze meg a hello attribútum sourceAnchor. Ez az attribútum egy fontos attribútum toobe képes toosign tooAzure AD a rendszer. Az Attribútumfolyam találhat meg két különböző szinkronizálási szabályok ezt az attribútumot **a az AD-felhasználó AccountEnabled** és **a az AD-felhasználó közös**. Lejáró tooSynchronization szabály elsőbbséget hello sourceAnchor attribútum van hozzájárult engedélyezett fiókkal hello erdőből először több objektumok illesztett toohello metaverzum-objektum esetén. Ha nincsenek engedélyezett fiókok, majd hello szinkronizálási motor által használt általános szinkronizálási szabály hello **a az AD-felhasználó közös**. Ez a konfiguráció biztosítja, hogy még a fiók, továbbra is a sourceAnchor.

![Bejövő szinkronizálási szabály](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

Szinkronizálási szabályok hello precedenciáját hello telepítővarázsló csoportok van megadva. A csoportban található összes szabály hello rendelkezik ugyanazzal a névvel, de csatlakoztatva csatlakoztatott toodifferent könyvtárak. hello telepítési varázsló lehetővé teszi a hello szabály **a az AD-felhasználó csatlakozás** legmagasabb élvez és azt megismétli over összes AD könyvtárakban csatlakoztatva. Azt a szabályok egy előre meghatározott sorrendben következő csoportját hello majd folytatja. Egy csoporton belüli hello szabályok hozzáadása történik meg a hello rendelés hello hello varázsló hozzáadott összekötők. Ha egy másik összekötő hello varázsló használatával ad hozzá, hello szinkronizálási szabályok rendezi újra, és hello új összekötő szabályok utolsó egészül ki minden egyes csoport.

### <a name="putting-it-all-together"></a>A teljes kép
Most már tudjuk elegendő szinkronizálási szabályok toobe képes toounderstand hello konfigurációs működése hello kapcsolatos különböző szinkronizálási szabályait. Ha célszerű figyelni felhasználói és hello attribútumokat, amelyek toohello metaverse hozzájárult, hello szabályok alkalmazása sorrendben következő hello:

| Név | Megjegyzés |
|:--- |:--- |
| Az ad-felhasználó illesztési |Összekötő metaverzum-es összekötő terület objektumok szabály. |
| Az ad-– UserAccount engedélyezve |A bejelentkezési tooAzure AD és az Office 365 szükséges attribútum. Azt szeretnénk, ha ezek az attribútumok engedélyezve hello fiókból. |
| Az ad-felhasználó közös az Exchange-ből |A globális címlista hello található attribútum. Feltételezzük, hogy hello az adatminőségi a legcélszerűbb hello erdőben, ahol találtunk, amelyeknek hello felhasználó postaládájához. |
| Az ad-felhasználó közös |A globális címlista hello található attribútum. Abban az esetben, ha a postaláda nem található, a többi illesztett objektum járulhat hello attribútum értéke. |
| Az AD-felhasználó Exchange-ből |Ha a rendszer észlelte az Exchange csak létezik. Összes infrastruktúra Exchange attribútum azt zajlik. |
| Az ad-felhasználó Lync |Ha a rendszer észlelte a Lync csak létezik. Összes infrastruktúra Lync attribútum azt zajlik. |

## <a name="next-steps"></a>Következő lépések
* További információk a hello konfigurációs modell [ismertetése deklaratív kiépítés](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* További információk a hello kifejezés nyelvi [ismertetése deklaratív kiépítés kifejezések](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).
* Továbbra is, hogyan működik hello out-of-box konfiguráció olvasása [felhasználók és névjegyek](active-directory-aadconnectsync-understanding-users-and-contacts.md)
* Tekintse meg, hogyan toomake egy gyakorlati módosítani, használja a deklaratív kiépítés [hogyan toomake egy módosítás toohello alapértelmezett konfiguráció](active-directory-aadconnectsync-change-the-configuration.md).

**Áttekintő témakör**

* [Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása](active-directory-aadconnectsync-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)

