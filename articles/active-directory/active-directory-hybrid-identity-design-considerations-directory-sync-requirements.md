---
title: "aaaAzure Active Directory hibrid identitáskezelési elrendezésével kapcsolatos szempontok - határozza meg a címtár-szinkronizálás követelményeinek |} Microsoft Docs"
description: "Azonosítsa, hogy milyen követelmények között senki hello szinkronizálásához van szükség a helyszíni és felhőalapú hello vállalati =."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: 593eaa71-17eb-4c16-8c98-43cc62987e65
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 6646e3792c65f37c3d62eecdb6c6f3bd257f04f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="determine-directory-synchronization-requirements"></a>Címtár-szinkronizálás követelmények meghatározása
Szinkronizálás legyen biztosítható a felhasználók a helyszíni identitás alapján hello felhőbeli identitás. Függetlenül attól, a hitelesítés és az összevont hitelesítés szinkronizált fiókkal fogja használata, hello felhasználók továbbra is kell toohave hello felhőbeli identitás.  Ezzel az identitással toobe karbantartani, és rendszeresen frissíteni kell.  hello frissítések számos formája, a cím módosítások toopassword módosítások.  

Első lépésként hello szervezetek helyszíni identitáskezelési megoldás és a felhasználói követelmények értékelése. Ez a kiértékelés fontos toodefine hello műszaki követelményeitől hogyan felhasználói identitások létrejön és karbantartása az hello felhő.  A szervezetek többsége, az Active Directory a helyszíni és hello helyszíni címtárat, hogy a felhasználók által lesznek szinkronizálva lesznek a, azonban néhány esetben ez nem lesz hello eset.  

Győződjön meg arról, hogy tooanswer hello a következő kérdéseket:

* Rendelkezik egy AD-erdő, több vagy nincs?
  
  * Hány Azure AD-címtártól fog meg kell szinkronizálja a névjegyadatokat?
    
    1. Használ szűrés?
    2. Van több Azure AD Connect-kiszolgálók tervezett?
* Jelenleg rendelkezik a szinkronizálás a helyszíni eszköze?
  
  * Ha igen, ha a felhasználók egy virtuális könyvtárat és-integráció identitások használ a felhasználók?
* Van bármilyen más címtár a helyszínen, amelyet az toosynchronize (pl. LDAP-címtárhoz, személyzeti adatbázis stb)?
  * Lesz a módon bármely GALSync toobe?
  * Mi az a hello aktuális állapotának UPN-EK a szervezetében? 
  * Van egy másik címtárat, hogy a felhasználók hitelesítése?
  * A vállalat használ a Microsoft Exchange?
    * Ezek fogja, hogy az exchange hibrid telepítés?

Most, hogy segítse az szinkronizálási követelményekről, kell eszköztől hello helyes-e egy toomeet toodetermine ezeket a követelményeket.  A Microsoft biztosít több tooaccomplish címtár-integrációs eszközök és a szinkronizálás.  Lásd: hello [hibrid Identity directory integration eszközök összehasonlító táblázatában](active-directory-hybrid-identity-design-considerations-tools-comparison.md) további információt. 

Most, hogy a szinkronizálásának követelményeiről és a rendszer ehhez a vállalat hello eszköz, tooevaluate hello az alkalmazásoknak, amelyek a címtárszolgáltatások kell. Ez a kiértékelés fontos toodefine hello műszaki követelményeiben toointegrate ezen alkalmazások toohello felhő. Győződjön meg arról, hogy tooanswer hello a következő kérdéseket:

* Ezek az alkalmazások nem áthelyezett toohello felhőhöz, és hello címtár?
* Vannak-e különleges attribútumok, ezek az alkalmazások használhassa őket sikeresen szinkronizált toobe toohello felhő igénylő?
* Ezeket az alkalmazásokat kell újra a felhőalapú hitelesítés tootake előnyeit írt toobe?
* Ezek az alkalmazások továbbra is toolive helyszíni során a felhasználók férhetnek hozzá, azok hello felhőalapú identitás használatával?

Meg kell toodetermine hello biztonsági követelményeket és korlátokat a címtár-szinkronizálás is. Ez a kiértékelés fontos tooget hello követelményeket rendelés toocreate szükség lesz, és karbantartása a felhasználók identitásainak hello felhőben. Győződjön meg arról, hogy tooanswer hello a következő kérdéseket:

* Amelyben hello szinkronizáló kiszolgáló található?
* Lesz tartományhoz csatlakoztatott?
* Hello server helyezkednek el a korlátozott jogú hálózatban, például egy Szegélyhálózaton tűzfal mögött található?
  * Fogja tudni tooopen szükséges hello tűzfal portok toosupport szinkronizálás?
* A vész-helyreállítási terv hello szinkronizálási kiszolgáló van?
* Azt szeretné, hogy toosynch az összes erdőben hello megfelelő jogosultságokkal rendelkező fiókkal van?
  * Ha a vállalat nem tudja hello válasz a kérdésre, tekintse át a hello szakasz "A jelszó-szinkronizálás engedélyeinek" hello cikkben [telepítés hello Azure Active Directory szinkronizáló szolgáltatás](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) és döntse el, ha már rendelkezik egy Ezekkel a jogosultságokkal rendelkező fiókot, vagy ha szüksége van-e egy toocreate.
* Ha rendelkezik mutli-erdő szinkronizálási az hello szinkronizáló kiszolgáló képes tooget tooeach erdő?

> [!NOTE]
> Győződjön meg arról, hogy tootake megjegyzések minden válaszról és hello válasz hello logikája ismertetése. [Incidensválasz-követelmények meghatározása](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) hello lehetőségeit ismerteti. Ezen kérdések mely leginkább megfelelő lehetőséget az üzleti megválaszolása szükséges.
> 
> 

## <a name="next-steps"></a>Következő lépések
[A multi-factor authentication követelmények meghatározása](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="see-also"></a>Lásd még:
[Kialakítási szempontok áttekintése](active-directory-hybrid-identity-design-considerations-overview.md)

