---
title: "Azure AD Connect szinkronizálása: felhasználók és névjegyek |} Microsoft Docs"
description: "Felhasználók és az Azure AD Connect szinkronizálási partnerek mutatja be."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 8d204647-213a-4519-bd62-49563c421602
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi;andkjell
ms.openlocfilehash: 4d80648c53a2981eb2626338913ed2282423f183
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-understanding-users-and-contacts"></a>Az Azure AD Connect szinkronizálása: a felhasználók és a kapcsolattartók ismertetése
Miért több Active Directory-erdő kellene lennie, és nincsenek számos különböző központi telepítési topológiák számos különféle oka van. Közös modellek például egy egyesülés & az beszerzése után egy fiók-erőforrások telepítése és a globális Címlista sync'ed erdők. De akkor is, ha nincsenek tiszta modellek, a hibrid modellek is. az Azure AD Connect szinkronizálási szolgáltatás hello alapértelmezett konfigurációja nem feltételezi azt egy meghatározott modellre, de attól függően, hogy hogyan felhasználók egyeztetéséről kiválasztott hello telepítési útmutatóban, különböző viselkedés tapasztalható.

Ebben a témakörben azt végighaladnia hello alapértelmezett konfigurációjának bizonyos topológia működését. A Microsoft hello konfigurációs végighaladnia, és szinkronizálási szabályok szerkesztő hello hello konfigurálás használt toolook.

Hello konfigurációs azt feltételezi, hogy néhány általános szabályok vonatkoznak:

* Függetlenül attól, amely order fogunk importálni hello forrásból aktív könyvtárak hello végeredménynek kell mindig kell hello azonos.
* Aktív fiók mindig is hozzájárul a bejelentkezési adatait, beleértve a **userPrincipalName** és **sourceAnchor**.
* Letiltott fiók is hozzájárul a userPrincipalName és sourceAnchor, kivéve ha a hivatkozott postafiókkal, ha nincs aktív fiók toobe található.
* Hivatkozott postafiókkal rendelkező fiók userPrincipalName és sourceAnchor soha nem használható. Feltételezzük, hogy aktív-fiók később találhatók.
* Egy partner objektuma lehet kiosztott tooAzure AD ügyfélként vagy egy olyan felhasználó nevében. Nem igazán ismeri az összes adatforrás Active Directory-erdők feldolgozásáig.

## <a name="contacts"></a>Kapcsolatok
Egy felhasználó egy másik erdőben képviselő névjegyek, akkor az általános egy egyesülés & az beszerzése után hol a GALSync megoldás az adatközponthíd-képzés két vagy több Exchange-erdők. hello kapcsolattartási objektum mindig csatlakozik, a hello összekötő terület toohello metaverse hello mail attribútum használatával. Ha már létezik a kapcsolattartási vagy hello rendelkező felhasználói objektumot megegyezik az e-mail cím, hello objektumok kapcsolódnak egymáshoz. Ennek a konfigurációja hello szabályban **a az AD-csatlakozás forduljon**. Szerepel továbbá egy nevű szabályt **a az AD-ügyfél közös** egy attribútum folyamat toohello metaverse attribútummal rendelkező **sourceObjectType** , hello állandó **forduljon**. Ez a szabály nagyon alacsony elsőbbséget, ha bármely felhasználói objektum illesztett toohello azonos metaverzum-objektum, majd hello szabály **a az AD-felhasználó közös** hozzájárul hello érték felhasználói toothis attribútum. Ez a szabály a ezt az attribútumot hello értéke forduljon, ha a felhasználó nem lett csatlakoztatva van, és hello érték felhasználói, ha legalább egy felhasználói talált lesz.

Egy objektum tooAzure AD történő üzembe helyezéséhez, a kimenő forgalomra vonatkozó szabály hello **kimenő tooAAD – csatlakoztatás forduljon** kapcsolattartási-objektumot hoz létre, ha hello metaverzum-attribútum **sourceObjectType** értéke túl**forduljon** . Ha ez az attribútum értéke túl**felhasználói**, majd hello szabály **tooAAD – felhasználói csatlakozás kimenő** felhasználói objektumot hoz létre.
Lehetséges, hogy az objektum állapotban van forduljon tooUser Ha több aktív adatforrás-könyvtár vannak importálva és szinkronizálva.

Például a GALSync topológia azt található kapcsolattartási objektumok mindenki számára hello második erdő amikor fogunk importálni hello első erdőt. Ez fogja tesztelése hello AAD-összekötő új kapcsolattartási objektumokat. Azt később importálása, és a második erdő hello szinkronizálni, rendszer hello valódi felhasználók keresése és csatlakoztassa azokat toohello meglévő metaverzum-objektum. Rendszer majd hello kapcsolattartási objektum az aad-ben törlése és inkább hozzon létre egy új felhasználói objektum.

Ha egy topológia, ahol a felhasználók jelentésekként jelennek meg a névjegyek, feltétlenül hello levél attribútum toomatch felhasználók hello telepítési útmutatóban. Ha egy másik lehetőséget választja, akkor egy rendelés függő konfigurációban fog működni. Kapcsolattartási objektumok mindig csatlakozni fog hello levél attribútum, de ha hello telepítési útmutató ezt a beállítást választotta, akkor csak csatlakozik felhasználói objektumok hello mail attribútum. Ön sikerült majd végül a hello hello metaverse két különböző objektumok azonos levél attribútum Ha hello kapcsolattartási objektum előtt hello felhasználói objektum lett importálva. Exportálás tooAzure AD, során hibát fog jelezni. Ez a viselkedés úgy lett kialakítva, és azt jelentené, hogy hibás adat, vagy adott hello topológia nem megfelelően azonosított hello telepítése során.

## <a name="disabled-accounts"></a>Letiltott fiókok
Letiltott fiókok is tooAzure AD szinkronizálása. Letiltott fiókok Exchange, például konferenciaterem közös toorepresent erőforrások. hello kivétel: a hivatkozott postafiókkal; rendelkező felhasználók mint korábban már említettük ezek soha nem kiépíti egy fiók tooAzure AD.

hello feltételezi, hogy ha letiltott felhasználói fiók található, akkor lesz nem találtunk aktív egy másik fiókot, és hello objektum kiosztott tooAzure AD hello userPrincipalName és sourceAnchor található. Esetben egy másik aktív fiók csatlakozni fog a toohello azonos metaverzum-objektum, majd a userPrincipalName és sourceAnchor fog történni.

## <a name="changing-sourceanchor"></a>SourceAnchor módosítása
Ha egy objektum exportálása tooAzure AD, akkor azt már nem engedélyezett toochange hello sourceAnchor. Ha a hello objektum lett exportált hello metaverzum-attribútum **cloudSourceAnchor** hello beállítása **sourceAnchor** értékét fogadja el az Azure AD. Ha **sourceAnchor** módosul, és nem felel meg **cloudSourceAnchor**, hello szabály **tooAAD – felhasználói csatlakozás kimenő** hello hiba kivételhibát **sourceAnchor attribútum megváltozott**. Ebben az esetben a konfiguráció hello vagy javítani kell tehát hello azonos sourceAnchor megtalálható-e újra hello metaverse előtt hello objektum újra kell szinkronizálni.

## <a name="additional-resources"></a>További források
* [Az Azure AD Connect-szinkronizálás: Szinkronizálási beállítások testreszabása](active-directory-aadconnectsync-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)

