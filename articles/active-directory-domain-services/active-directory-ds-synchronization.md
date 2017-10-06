---
title: "Azure Active Directory tartományi szolgáltatások: A felügyelt tartományok szinkronizálás |} Microsoft Docs"
description: "Az Azure Active Directory tartományi szolgáltatások által felügyelt tartományokhoz szinkronizációja ismertetése"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 57cbf436-fc1d-4bab-b991-7d25b6e987ef
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 9be25b61823a6b031906f3576395782e73831fc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="synchronization-in-an-azure-ad-domain-services-managed-domain"></a>Szinkronizálás az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz
hello következő diagram azt ábrázolja szinkronizálási működéséről az Azure AD tartományi szolgáltatásokban felügyelt tartományok.

![Az Azure AD tartományi szolgáltatásokra szinkronizált topológiákkal](./media/active-directory-domain-services-design-guide/sync-topology.png)

## <a name="synchronization-from-your-on-premises-directory-tooyour-azure-ad-tenant"></a>A helyszíni címtár tooyour az Azure AD-bérlő való szinkronizálásra
Azure AD Connect szinkronizálása használt toosynchronize felhasználói fiókok, csoporttagságot és hitelesítő kivonatok Azure AD tooyour bérlői. Attribútumok a felhasználói fiókok például hello egyszerű felhasználónév és a helyszíni biztonsági azonosító (security Identifier azonosítót) szinkronizálja a rendszer. Ha az Azure AD tartományi szolgáltatásokat használja, az NTLM és Kerberos hitelesítéshez szükséges a hagyományos hitelesítő kivonatokat egyaránt tooyour szinkronizált Azure AD-bérlő.

Késleltetve visszaírt állítja be, ha az Azure AD-címtár változásokat hátsó tooyour a helyszíni Active Directory szinkronizálva. Például, ha módosítja a jelszavát, az Azure AD önkiszolgáló jelszó-módosítási funkciókat használ, hello módosított jelszó frissül a helyszíni Active Directory-tartománynak.

> [!NOTE]
> Mindig a hello legújabb verzióját használja, az Azure AD Connect tooensure van az összes ismert hibák.
>
>

## <a name="synchronization-from-your-azure-ad-tenant-tooyour-managed-domain"></a>Az Azure AD-bérlő tooyour szinkronizálás felügyelt tartományhoz
Felhasználói fiókok, csoporttagságot és hitelesítő kivonatokat vannak szinkronizálva az Azure AD-bérlő tooyour Azure AD tartományi szolgáltatások által kezelt tartomány. A szinkronizálási folyamat be nem automatikus. Tooconfigure, nem kell figyelni, vagy a szinkronizálási folyamat kezeléséhez. : A címtár kezdeti szinkronizálása egyszeri hello befejezése után, általában az az Azure AD toobe végzett módosítások körülbelül 20 percet vesz igénybe megjelennek a felügyelt tartományok. A szinkronizálás időköze toopassword végzett módosítások alkalmazása, vagy tooattributes Azure AD-ben végrehajtott módosításokat.

hello szinkronizálási folyamat akkor is egy-way/egyirányú jellegűek. A felügyelt tartományok írásvédett nagymértékben kivételével minden egyéni szervezeti egységek hoz létre. Ezért módosítások toouser attribútumok, a felhasználói jelszavakat vagy a csoporttagság nem hajtható végre hello felügyelt tartományon belül. Ennek eredményeképpen nincs nincs fordított a felügyelt tartományra hátsó tooyour Azure AD-bérlő a változások szinkronizálása.

## <a name="synchronization-from-a-multi-forest-on-premises-environment"></a>Egy helyszíni Többerdős környezetben való szinkronizálásra
Számos szervezet rendelkezik több fiókerdővel álló elég bonyolult a helyszíni identitás-infrastruktúra. Az Azure AD Connect szinkronizálási felhasználók, csoportok és hitelesítő kivonatokat Többerdős környezetben tooyour az Azure AD bérlőhöz támogatja.

Ezzel szemben az Azure AD-bérlő egy sokkal egyszerűbb, és strukturálatlan névtér. tooenable felhasználók tooreliably alkalmazásokat az Azure AD által védett UPN-ütközések feloldása különböző erdőkben találhatók a felhasználói fiókok között. Az Azure AD tartományi szolgáltatások által kezelt tartomány aránylik hasonlóságot tooyour Azure AD-bérlő bezárásához. Ezért a felügyelt tartományok látható egy egyszerű Szervezetiegység-struktúrája. Összes felhasználók és csoportok hello "AADDC felhasználók" tárolóban, függetlenül attól, hello helyszíni tartományban vagy erdőben, ahol azok lettek szinkronizálva a tárolják. Előfordulhat, hogy konfigurálta a hierarchikus OU helyszíni struktúra. A felügyelt tartományok azonban továbbra is rendelkezik egy egyszerű strukturálatlan Szervezetiegység-struktúrájától.

## <a name="exclusions---what-isnt-synchronized-tooyour-managed-domain"></a>Kizárások - mi nem szinkronizált tooyour által felügyelt tartományokhoz
hello következő objektumok és attribútumok nem lettek tooyour szinkronizált Azure AD-bérlő tooyour által kezelt tartomány:

* **Kizárt attribútumok:** tooexclude bizonyos attribútumok közül választhat szinkronizálása az Azure AD Connect használatával a helyszíni tartományból tooyour az Azure AD bérlői. Ezek az attribútumok kizárt nem érhetők el a kezelt tartományban.
* **Csoportházirendek:** konfigurált a helyszíni tartományi csoport házirendek nincsenek nem szinkronizált tooyour által felügyelt tartományokhoz.
* **SYSVOL-megosztás:** hasonlóan hello SYSVOL megosztás a helyi tartomány hello tartalma nem szinkronizált tooyour által felügyelt tartományokhoz.
* **Számítógép-objektumok:** számítógépek illesztett tooyour helyszíni tartományhoz tartozó számítógép-objektumok nincsenek szinkronizált tooyour által felügyelt tartományokhoz. Ezeket a számítógépeket a felügyelt tartományok bizalmi kapcsolattal rendelkezik, és nem tooyour helyszíni csak tartományhoz tartozik. A felügyelt tartomány látnia számítógép-objektumok csak a felügyelt tartomány rendelkezik explicit módon tartományhoz toohello számítógépek esetében.
* **A felhasználók és csoportok SidHistory attribútumok:** hello elsődleges felhasználó és elsődleges csoport biztonsági azonosítói a helyi tartomány szinkronizált tooyour által felügyelt tartományokhoz. Azonban a felhasználók és csoportok meglévő SidHistory attribútumok nincsenek szinkronizálva a helyszíni tartományi tooyour felügyelt tartomány.
* **Szervezeti egység (OU) struktúrák:** a helyszíni tartományi megadott szervezeti egységek tooyour által kezelt tartomány nem szinkronizálja. A felügyelt tartományok nincsenek két beépített szervezeti egységekhez. Alapértelmezés szerint a felügyelt tartományok rendelkezik egy egyszerű Szervezetiegység-struktúrája. Azonban dönthet túl[hozzon létre egy egyéni szervezeti Egységet a felügyelt tartományok](active-directory-ds-admin-guide-create-ou.md).

## <a name="how-specific-attributes-are-synchronized-tooyour-managed-domain"></a>Hogyan adott attribútumok szinkronizált tooyour által felügyelt tartományokhoz
a következő táblázat hello néhány általános attribútumokkal rendelkeznek, és ismerteti, hogyan szinkronizált tooyour által kezelt tartomány.

| A felügyelt tartományok attribútum | Forrás | Megjegyzések |
|:--- |:--- |:--- |
| EGYSZERŰ FELHASZNÁLÓNÉV |Felhasználó UPN attribútum az Azure AD-bérlőben |felügyelt tooyour tartomány szinkronizálása az Azure AD-bérlő hello UPN-attribútumot. Ezért hello legmegbízhatóbb módja toosign tooyour felügyelt tartomány használja az egyszerű felhasználónév. |
| sAMAccountName |Felhasználó mailNickname az Azure AD-bérlő attribútumnak, vagy automatikusan generált |hello SAMAccountName attribútum hello mailNickname attribútumot az Azure AD-bérlő származik. Ha több felhasználói fiók azonos mailNickname attribútum, hello SAMAccountName az automatikusan létrehozott hello. Hello felhasználó mailNickname vagy UPN előtagja hosszabb 20 karakternél, hello SAMAccountName akkor automatikusan létrehozott toosatisfy hello 20 karakter lehet SAMAccountName attribútumait. |
| Jelszavak |Az Azure AD-bérlő felhasználói jelszó |Az Azure AD-bérlő (más néven kiegészítő hitelesítő adatok) NTLM vagy Kerberos hitelesítéshez szükséges hitelesítő kivonatokat vannak szinkronizálva. Ha az Azure AD-bérlő a szinkronizált bérlők, ezeket a hitelesítő adatokat a helyszíni tartományból forrása. |
| Elsődleges felhasználó/csoport biztonsági azonosítója |Automatikusan létrehozott |a felhasználó vagy csoport fiókok hello elsődleges biztonsági azonosító jön létre automatikusan a kezelt tartományban. Ez az attribútum nem egyezik meg a hello elsődleges felhasználó/csoport SID hello objektum a helyszíni Active Directory-tartománynak. Ez az eltérés az oka, hogy hello által kezelt tartomány névtérrel rendelkező különböző SID mint a helyszíni tartományban. |
| A felhasználók és csoportok SID-előzmények |A helyszíni elsődleges felhasználói és csoportos biztonsági azonosítója |hello SidHistory attribútum a felhasználók és csoportok a felügyelt tartomány toomatch hello megfelelő elsődleges felhasználó vagy csoport SID van beállítva a helyszíni tartományban. A szolgáltatás segítségével ellenőrizze növekedési-és-shift a helyszíni alkalmazások toohello által kezelt tartomány egyszerűbb, mivel nincs szükség toore-ACL-erőforrások. |

> [!NOTE]
> **Jelentkezzen be toohello által kezelt tartomány hello UPN formátumot használja:** hello SAMAccountName attribútum lehet, hogy az egyes felhasználói fiókok a felügyelt tartományok az automatikusan generált. Ha több felhasználó rendelkezik-e hello azonos mailNickname attribútum vagy felhasználók rendelkeznek-e túl hosszú az UPN-előtagok, hello SAMAccountName ezen felhasználók automatikus-, a rendszer. Ezért hello SAMAccountName (például CONTOSO100\joeuser) formátuma nem mindig egy megbízható módot toosign toohello tartományban. Automatikusan létrehozott SAMAccountName a felhasználói UPN előtag eltérhet. Hello UPN formátum használata (például "joeuser@contoso100.com") a toohello toosign felügyelt tartomány megbízhatóan.
>
>

### <a name="attribute-mapping-for-user-accounts"></a>A felhasználói fiókok címtárattribútum-leképezésben
a következő táblázat hello hogyan adott attribútumok mutatja be, a felhasználói objektumok az Azure AD-bérlőben szinkronizált toocorresponding attribútumokat a felügyelt tartományok.

| Az Azure AD-bérlő felhasználói attribútum | A felügyelt tartományok felhasználói attribútum |
|:--- |:--- |
| AccountEnabled |userAccountControl (be / kikapcsolja a hello ACCOUNT_DISABLED bit) |
| city |l csomag |
| Ország |CO |
| Szervezeti egység |Szervezeti egység |
| displayName |displayName |
| facsimileTelephoneNumber |facsimileTelephoneNumber |
| givenName |givenName |
| Beosztás |Cím |
| mail |mail |
| mailNickname |Az msDS-AzureADMailNickname |
| mailNickname |SAMAccountName (néha lehet automatikusan generált) |
| Mobileszköz |Mobileszköz |
| objektumazonosító |Az msDS-AzureADObjectId |
| onPremiseSecurityIdentifier |sidHistory |
| passwordPolicies |userAccountControl (be / kikapcsolja a hello DONT_EXPIRE_PASSWORD bit) |
| physicalDeliveryOfficeName |physicalDeliveryOfficeName |
| Irányítószám |Irányítószám |
| preferredLanguage |preferredLanguage |
| state |St |
| StreetAddress |StreetAddress |
| Vezetéknév |sorozatszám |
| TelephoneNumber |TelephoneNumber |
| UserPrincipalName |UserPrincipalName |

### <a name="attribute-mapping-for-groups"></a>A csoportok címtárattribútum-leképezésben
a következő táblázat hello hogyan adott attribútumok mutatja be, a csoportobjektumokhoz az Azure AD-bérlőben szinkronizált toocorresponding attribútumokat a felügyelt tartományok.

| Az Azure AD-bérlőben attribútum | A felügyelt tartományok csoport attribútum |
|:--- |:--- |
| displayName |displayName |
| displayName |SAMAccountName (néha lehet automatikusan generált) |
| mail |mail |
| mailNickname |Az msDS-AzureADMailNickname |
| objektumazonosító |Az msDS-AzureADObjectId |
| onPremiseSecurityIdentifier |sidHistory |
| SecurityEnabled |GroupType |

## <a name="objects-that-are-not-synchronized-tooyour-azure-ad-tenant-from-your-managed-domain"></a>Objektumok, amelyek nem szinkronizált Azure AD-bérlő tooyour a felügyelt tartomány
Ez a cikk az előző szakaszban leírtak nincs a felügyelt tartományra hátsó tooyour Azure AD-bérlő a Nincs szinkronizálás. Túl dönthet[hozzon létre egy egyéni szervezeti egységet (OU)](active-directory-ds-admin-guide-create-ou.md) a kezelt tartományban. Létrehozhat további, az egyéb szervezeti egységek, felhasználók, csoportok vagy ezeket egyéni szervezeti egységek belül szolgáltatásfiókok. Egyéni szervezeti egységek belül létrehozott hello objektumok egyike szinkronizálva van a háttérben tooyour Azure AD-bérlő. Ezek az objektumok csak a felügyelt tartományon belüli használatra érhetők el. Ezért ezek az objektumok nem láthatók az Azure AD PowerShell-parancsmagokkal, Azure AD Graph API vagy hello Azure AD felügyeleti felhasználói Felületét használja.

## <a name="related-content"></a>Kapcsolódó tartalom
* [Szolgáltatások – Azure AD tartományi szolgáltatások](active-directory-ds-features.md)
* [Központi telepítési forgatókönyvek - Azure AD tartományi szolgáltatások](active-directory-ds-scenarios.md)
* [Azure AD tartományi szolgáltatások hálózati szempontjai](active-directory-ds-networking.md)
* [Ismerkedés az Azure AD tartományi szolgáltatások](active-directory-ds-getting-started.md)
