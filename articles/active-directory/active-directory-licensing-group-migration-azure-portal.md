---
title: "az egyes licenccel rendelkező felhasználók tooa csoport az Azure Active Directoryban aaaHow toomigrate |} Microsoft Docs"
description: "Hogyan tooswitch az egyes felhasználói licencek toogroup alapú licencelése az Azure Active Directoryval"
services: active-directory
keywords: "Az Azure AD-licencelés"
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/05/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 25e5c760b7e632ba71edde10d937fe580aa6ed35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-licensed-users-tooa-group-for-licensing-in-azure-active-directory"></a>Hogyan tooadd licenccel rendelkező felhasználók tooa csoport az Azure Active Directoryban licenckezeléshez

Előfordulhat, hogy meglévő telepített licencek toousers hello szervezetekben via "rendelését"; Ez azt jelenti, hogy használja a PowerShell-parancsfájlok vagy más eszközök tooassign egyedi felhasználói licencet. Ha szeretné toostart Csoportalapú licencelési toomanage licencet használ a szervezetben, szüksége lesz egy áttelepítési terv tooseamlessly cserélje le a már meglévő megoldások Csoportalapú licenceléshez.

hello legfontosabb dolog tookeep szem előtt az, hogy kerülje az olyan helyzet, ahol áttelepítése toogroup alapú licencelési hatására a felhasználók ideiglenesen elvesztése aktuálisan hozzárendelt licencét. A folyamat, amelynek hatására a licencek eltávolítását kell kerülni a felhasználók hozzáférést tooservices és az adatok elvesztése tooremove hello fakadó kockázatokat.

## <a name="recommended-migration-process"></a>Ajánlott áttelepítési folyamat

1. A licenc-hozzárendelést és a felhasználók eltávolításának kezelése automatizálási (például PowerShell) rendelkezik. Fut, hagyja.

2. Hozzon létre egy új licencelési csoportot (vagy döntse el, melyik meglévő csoportok toouse), és győződjön meg arról, hogy minden felhasználót adnak hozzá tagként szükséges.

3. Hello szükséges licencek kiosztása toothose csoportok; a cél kell tooreflect hello azonos licencelési állapotát a meglévő automatizációk (például PowerShell) alkalmazza toothose felhasználók.

4. Győződjön meg arról, hogy a licencek történt-e a felhasználók alkalmazott tooall ezeket a csoportokat. Ehhez hello feldolgozási állapotát az egyes csoportok ellenőrzésével, és a vizsgálati naplók ellenőrzésével.

  - A licenc részletei megtekintésével helyszíni ellenőrzés egyéni felhasználók számára is. Látni fogja, hogy azok rendelkeznek hello azonos licenccel "közvetlenül" és "örökölt" csoportból.

  - Egy PowerShell-parancsfájlt futtathatja túl[győződjön meg arról, hogyan történik az licencek hozzárendelése toousers](active-directory-licensing-group-advanced.md#use-powershell-to-see-who-has-inherited-and-direct-licenses).

  - Hello azonos termék rendel hozzá licencet toohello felhasználói közvetlenül és a csoport révén, amikor hello felhasználó által felhasznált csak egy-egy licencet. Ezért a további licencek nem szükséges tooperform áttelepítési.

5. Győződjön meg arról, hogy nem a licenc-hozzárendeléseivel minden csoport hibás állapotú felhasználók ellenőrzésével sikertelen. További információkért lásd: [azonosítása és licenc problémák megoldásához a csoport](active-directory-licensing-group-problem-resolution-azure-portal.md).

6. Vegye figyelembe a hello eredeti közvetlen hozzárendelések; eltávolítása érdemes lehet toodo először hello fokozatosan, a "hullámok" toomonitor eredménye a felhasználóinak egy részhalmazára.

  Sikerült hagyja hello eredeti közvetlen hozzárendelések a felhasználók, de ha hello felhasználók elhagyják a licenccel rendelkező csoportok továbbra is megőrzik hello eredeti licenc, amely valószínűleg nem szeretné, hogy szeretné.

## <a name="an-example"></a>Példa

Tudunk 1000 felhasználóval rendelkező szervezeteknél. Minden felhasználó Enterprise Mobility + Security (EMS) licencet igényelnek. 200 felhasználó hello pénzügyi részleg, igénylő Office 365 nagyvállalati E3 csomag licenceket. Tudunk futó helyi hozzáadása és eltávolítása a felhasználók licencek, származnak, és nyissa meg egy PowerShell-parancsfájlt. Azt szeretnénk, ha tooreplace hello parancsfájl Csoportalapú licenceléshez, licencek automatikusan kezeli az Azure AD.

Íme, milyen hello áttelepítési folyamat sikerült megjelenését, például:

1. Az Azure portál hozzárendelése hello EMS-licenc toohello használatával hello **minden felhasználó** az Azure AD-csoporthoz. Rendelje hozzá a hello E3 licenc toohello **pénzügyi részleg** csoportot, amely tartalmazza az összes szükséges hello felhasználót.

2. Az egyes csoportokban ellenőrizze, hogy a licenc-hozzárendelést az összes felhasználó számára befejeződött. Nyissa meg toohello panel az egyes csoportokhoz, jelölje be **licencek**, és hello feldolgozási állapotának hello hello tetején **licencek** panelen.

  - Keresse meg az "a legújabb licenc módosításainak alkalmazott tooall felhasználók megtörtént" tooconfirm feldolgozása befejeződött.

  - Keresse meg felül, amely licencek előfordulhat, hogy nem sikerült hozzárendelt felhasználók kapcsolatos értesítést. Nem jelenleg egyes felhasználók licenceinek kívül futtatni? Egyes felhasználók rendelkeznek ütköző licenc, amely megakadályozza, hogy azok csoport licencek öröklődés termékváltozatok?

3. Helyszíni néhány hello közvetlen és a csoport licencek alkalmazza azok rendelkező felhasználók tooverify ellenőrzése. Felhasználóra vonatkozik, paneljén lépjen toohello **licencek**, és vizsgálja meg a licencek hello állapotát.

  - Ez az elvárt hello felhasználói állapot áttelepítése során:

      ![várt érték user állapota](media/active-directory-licensing-group-migration-azure-portal/expected-user-state.png)

  Ez megerősíti, hogy hello felhasználó közvetlen és az örökölt licenccel rendelkezik. Látható, amely mindkét **EMS** és **E3** vannak hozzárendelve.

  - Jelölje ki minden engedélyezett hello szolgáltatások licenc tooshow részleteit. Ez akkor lehet használt toocheck, ha közvetlen hello és csoport licencek engedélyezése pontosan hello hello felhasználó számára azonos service-csomagokról.

      ![Ellenőrizze a service-csomagokról](media/active-directory-licensing-group-migration-azure-portal/check-service-plans.png)

4. Miután meggyőződött arról, hogy közvetlen és a csoport licencek egyenértékűek, megkezdheti a felhasználók közvetlen licencek eltávolítását. Ehhez távolítsa el az egyes felhasználók hello portálon tesztelést, és futtassa az automatizálási parancsfájlokat toohave őket egyszerre eltávolítani. Itt található példa hello hello közvetlen licenccel rendelkező ugyanazon felhasználó eltávolítva hello portálon keresztül. Figyelje meg, hogy hello licenc állapota változatlan marad, de már nem látható közvetlen hozzárendeléseket.

  ![közvetlen licencek eltávolítása](media/active-directory-licensing-group-migration-azure-portal/direct-licenses-removed.png)


## <a name="next-steps"></a>Következő lépések

További információ az egyéb forgatókönyvek licenc felügyeleti csoportok, keresztül toolearn olvasása

* [Az Azure Active Directoryban licencek tooa csoportok átjáróalhálózathoz való hozzárendelése](active-directory-licensing-group-assignment-azure-portal.md)
* [Mi az a csoport-alapú licencelése az Azure Active Directoryban?](active-directory-licensing-whatis-azure-portal.md)
* [Majd azonosítani és megoldani az Azure Active Directory csoport licenc problémák](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Az Azure Active Directory biztonságicsoport-alapú licencelési további helyzeteket is](active-directory-licensing-group-advanced.md)
