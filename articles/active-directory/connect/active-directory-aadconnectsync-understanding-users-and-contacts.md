---
title: "Azure AD Connect szinkronizálása: felhasználók, csoportok és névjegyek |} Microsoft Docs"
description: "Felhasználók, csoportok és az Azure AD Connect szinkronizálási partnerek ismerteti."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
ms.assetid: 8d204647-213a-4519-bd62-49563c421602
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi;andkjell
ms.openlocfilehash: 7f4bc51630653bfe341bfcb5c11699020053585a
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-connect-sync-understanding-users-groups-and-contacts"></a>Azure AD Connect szinkronizálása: felhasználók, csoportok és névjegyek
Miért több Active Directory-erdő kellene lennie, és nincsenek számos különböző központi telepítési topológiák számos különféle oka van. Közös modellek például egy egyesülés & az beszerzése után egy fiók-erőforrások telepítése és a globális Címlista sync'ed erdők. De akkor is, ha nincsenek tiszta modellek, a hibrid modellek is. Az alapértelmezett konfiguráció a Azure AD Connect szinkronizálási szolgáltatás nem feltételezi azt egy meghatározott modellre, de attól függően, hogy hogyan felhasználók egyeztetéséről volt jelölve a telepítési útmutatóban, különböző viselkedés tapasztalható.

Ebben a témakörben azt végighaladnia az alapértelmezett konfiguráció bizonyos topológia működését. Azt a konfigurációs végighaladnia, és a szinkronizálási szabályok szerkesztő segítségével tekintse meg a konfigurációt.

A konfigurációs azt feltételezi, hogy néhány általános szabályok vonatkoznak:
* Sorrendet, amelyben a forrás aktív könyvtárak fogunk importálni, függetlenül a végeredménynek mindig azonosnak kell lennie.
* Aktív fiók mindig is hozzájárul a bejelentkezési adatait, beleértve a **userPrincipalName** és **sourceAnchor**.
* Letiltott fiók is hozzájárul a userPrincipalName és sourceAnchor, kivéve ha a hivatkozott postafiókkal, ha nincs aktív fiók található.
* Hivatkozott postafiókkal rendelkező fiók userPrincipalName és sourceAnchor soha nem használható. Feltételezzük, hogy aktív-fiók később találhatók.
* Egy partner objektuma előfordulhat, hogy üzembe az Azure AD-ügyfélként vagy egy olyan felhasználó nevében. Nem igazán ismeri az összes adatforrás Active Directory-erdők feldolgozásáig.

## <a name="groups"></a>Csoportok
Fontos tisztában lennie az Active Directoryból az Azure AD-csoportok szinkronizálásakor szempontok:

* Az Azure AD Connect címtár-szinkronizálás beépített biztonsági csoportok nem tartalmazza.

* Az Azure AD Connect nem támogatja a szinkronizálás [elsődleges csoporttagságok](https://technet.microsoft.com/library/cc771489(v=ws.11).aspx) az Azure ad Szolgáltatásba.

* Az Azure AD Connect nem támogatja a szinkronizálás [dinamikus terjesztési csoporttagságok](https://technet.microsoft.com/library/bb123722(v=exchg.160).aspx) az Azure ad Szolgáltatásba.

* Az Azure AD egy levelezési csoport, egy Active Directory csoport szinkronizálásához:

    * Ha a csoport *proxyAddress* attribútum értéke üres, a *mail* attribútum értékűnek kell lennie.

    * Ha a csoport *proxyAddress* attribútum értéke nem lehet üres, legalább egy SMTP proxy címe értéket kell tartalmaznia. Néhány példa:
    
      * Az Active Directory-csoportot, amelynek proxyAddress attribútum értéke *{"X500:/0=contoso.com/ou=users/cn=testgroup"}* levelezési Azure AD-ben nem lesz. Nem rendelkezik egy SMTP-cím.
      
      * Az Active Directory-csoportot, amelynek proxyAddress attribútum értéke van *{"X500:/0=contoso.com/ou=users/cn=testgroup","SMTP:johndoe@contoso.com"}* lesz levelezési Azure AD-ben.
      
      * Az Active Directory-csoportot, amelynek proxyAddress attribútum értéke van *{"X500:/0=contoso.com/ou=users/cn=testgroup", "smtp:johndoe@contoso.com"}* lesz levelezési Azure AD-ben.

## <a name="contacts"></a>Kapcsolatok
Egy felhasználó egy másik erdőben képviselő névjegyek, akkor az általános egy egyesülés & az beszerzése után hol a GALSync megoldás az adatközponthíd-képzés két vagy több Exchange-erdők. A partner objektuma mindig a metaverzumba, a levél attribútum használatával történő csatlakoztatását a kapcsolódási térbe a. Ha már van egy ügyfél vagy felhasználói objektum ezzel az e-mail címmel, az objektumok kapcsolódnak egymáshoz. Ez a szabály úgy van konfigurálva **a az AD-csatlakozás forduljon**. Szerepel továbbá egy nevű szabályt **a az AD-ügyfél közös** egy Attribútumfolyam, hogy a metaverzum-attribútum a **sourceObjectType** , az állandó **forduljon**. Ez a szabály nagyon alacsony elsőbbséget, ha bármely felhasználói objektum a azonos metaverzum-objektum, majd a szabály **a az AD-felhasználó közös** is hozzájárul a felhasználó érték ennél az attribútumnál. Ez a szabály ezt az attribútumot kell az ügyfél, ha a felhasználó nem lett csatlakoztatva van és érték a felhasználó Ha talált legalább egy felhasználót.

Az Azure AD, a kimenő forgalomra vonatkozó szabály objektum történő üzembe helyezéséhez **az AAD-csatlakozás kérje ki** kapcsolattartási-objektumot hoz létre, ha a metaverzum-attribútum **sourceObjectType** értékre van állítva **forduljon**. Ha ez az attribútum értéke **felhasználói**, majd a szabály **ki az aad-be – felhasználói csatlakozás** felhasználói objektumot hoz létre.
Lehetséges, hogy az objektum állapotban van forduljon felhasználó, amikor több aktív adatforrás-könyvtár vannak importálva és szinkronizálva.

Például a GALSync topológia azt található kapcsolattartási objektumok mindenki számára a második erdő amikor fogunk importálni az első erdőt. Ez lesz az AAD-összekötő új kapcsolattartási objektumok átmenetivé tétele. Amikor azt később importálja, és szinkronizálja a másik erdő, azt a valódi felhasználók megtalálja, és csatlakoztassa azokat a meglévő metaverzum-objektum. Rendszer majd törli az aad-ben a kapcsolattartási objektumot és inkább hozzon létre egy új felhasználói objektum.

Ha egy topológia, ahol a felhasználók jelentésekként jelennek meg a névjegyek, mindenképpen jelölje ki a telepítési útmutató a levél attribútum a felhasználók kereséséhez. Ha egy másik lehetőséget választja, akkor egy rendelés függő konfigurációban fog működni. Kapcsolattartási objektumok mindig csatlakozni fog a levél attribútum, de a felhasználói objektumok csatlakozni a mail attribútum csak fog, ha ezt a beállítást választotta, a telepítési útmutatóban. Akkor sikerült majd végül a metaverzumban azonos mail attribútummal két különböző objektum Ha a partner objektuma importálása előtt a user objektum. Az Azure AD-exportálás során hibát fog jelezni. Ez a viselkedés úgy lett kialakítva, és azt jelzi, hibás adatot vagy az, hogy a topológia megfelelően nem azonosított a telepítés során.

## <a name="disabled-accounts"></a>Letiltott fiókok
Letiltott fiókok is szinkronizálva az Azure ad Szolgáltatásba. Letiltott fiókok használata gyakori az Exchange, például konferenciaterem erőforrások képviseli. A kivétel: a hivatkozott postafiókkal; rendelkező felhasználók mint korábban már említettük ezek soha nem kiépíti az Azure AD-fiókkal.

Feltételezve, hogy ha letiltott felhasználói fiók található, akkor lesz nem találtunk aktív egy másik fiókot, és az objektum ki van építve, az Azure AD a userPrincipalName és a sourceAnchor található. Abban az esetben, ha egy másik aktív fiók, akkor csatlakozik ugyanahhoz a metaverzum-objektumhoz, majd a userPrincipalName és a sourceAnchor használható.

## <a name="changing-sourceanchor"></a>SourceAnchor módosítása
Ha egy objektum exportálva az Azure AD és a sourceAnchor többé módosítása nem engedélyezett. Ha az objektum le lett exportálva a metaverzum-attribútum **cloudSourceAnchor** van beállítva a **sourceAnchor** értékét fogadja el az Azure AD. Ha **sourceAnchor** módosul, és nem felel meg **cloudSourceAnchor**, a szabály **ki az aad-be – felhasználói csatlakozás** a hiba kivételhibát **sourceAnchor attribútum megváltozott**. Ebben az esetben a konfiguráció vagy javítani kell, az azonos sourceAnchor megtalálható a metaverzumban újra előtt az objektum újra kell szinkronizálni.

## <a name="additional-resources"></a>További források
* [Az Azure AD Connect-szinkronizálás: Szinkronizálási beállítások testreszabása](active-directory-aadconnectsync-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)

