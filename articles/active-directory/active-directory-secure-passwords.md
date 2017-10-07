---
title: "aaaAzure AD rétegzett biztonsági jelszó |} Microsoft Docs"
description: "Azt ismerteti, hogyan az Azure AD kikényszeríti a erős jelszavak és védje a felhasználók jelszavát a számítógépes bűnözők"
services: active-directory
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: joflore
ms.openlocfilehash: 10d8b600d9f4c02355b2cd8c5dccf8505aaf210d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="a-multi-tiered-approach-tooazure-ad-password-security"></a>A többrétegű megközelítés tooAzure AD biztonsági jelszó

Ez a cikk ismerteti, amelyek néhány ajánlott eljárás követésével egy olyan felhasználó nevében, vagy egy rendszergazda tooprotect az Azure Active Directory (Azure AD) vagy a Microsoft Account.

 > [!NOTE]
 > Az Azure AD-rendszergazdák alaphelyzetbe állíthatja a felhasználói jelszavakat hello útmutatásának hello cikkben [az Azure Active Directory felhasználó jelszavának alaphelyzetbe állítása hello](active-directory-users-reset-password-azure-portal.md).
 >
 > Felhasználók visszaállíthatják a saját jelszavát hello útmutatásának hello cikkben [elfelejtettem a saját Azure AD-jelszó súgó](active-directory-passwords-update-your-own-password.md).
 >

## <a name="password-requirements"></a>Jelszavakra vonatkozó követelmények

Az Azure AD magában foglalja a következő közös megközelítések toosecuring jelszavak hello:

* Jelszóhosszra vonatkozó követelmények
* Összetettségi követelményeknek
* Normál és időszakos jelszólejárat

Jelszó alaphelyzetbe állítása, az Azure Active Directoryban kapcsolatos információkért lásd: hello témakör [az Azure AD az önkiszolgáló jelszó-változtatási az informatikai szakembereknek szóló hello](active-directory-passwords.md).

## <a name="azure-ad-password-protections"></a>Az Azure AD-jelszó védelmet

Az Azure AD és a Microsoft-Fiókrendszerben használja bizonyítása iparági hello megközelíti tooensure biztonságos védelmét, beleértve a felhasználói és rendszergazdai jelszavakat:

* Dinamikusan letiltott jelszavak
* Intelligens jelszózárolás

Aktuális eredményei alapján jelszó-kezeléssel kapcsolatos további információkért lásd: hello tanulmány [jelszó útmutatást](http://aka.ms/passwordguidance).

### <a name="dynamically-banned-passwords"></a>Dinamikusan letiltott jelszavak

Az Azure AD és a Microsoft Accounts megakadályozhatja a jelszavas védelem által dinamikusan tiltó a gyakran használt jelszavakat. hello Azure ID Identity Protection-csapatnak rendszeresen elemzi tiltott jelszavak listája, meggátolja, hogy a felhasználók kiválasztása a gyakran használt jelszavakat. Ez a szolgáltatás elérhető tooAzure AD és hello Microsoft-fiók szolgáltatás felhasználói.

Jelszavak létrehozása, ha fontos a rendszergazdák tooencourage felhasználók toochoose jelszó kifejezések, amely tartalmazza az betűket, számokat, karakterek vagy szavak egyedi kombinációja. Ez a megközelítés segít toomake felhasználói jelszavak majdnem lehetetlen toobe biztonsága sérült, de megkönnyíthető a felhasználóknak tooremember.

#### <a name="password-breaches"></a>Jelszó megszegése

Microsoft toostay egy lépésben előre számítógépes-bűnözők mindig működik.

az Azure AD Identity Protection-csapatnak hello folyamatosan elemzi általánosan használt jelszavakat. Számítógépes-bűnözők is használhatja hasonló stratégiák tooinform azok támadásokkal, például a épület egy [rainbow table](https://en.wikipedia.org/wiki/Rainbow_table) repedés jelszókivonatait a.

Folyamatosan elemzi a Microsoft [adatszivárgáshoz](https://www.privacyrights.org/data-breaches) toomaintain dinamikusan frissített tiltott jelszó listáját, amely biztosítja, hogy így elkerülhetők a valódi tilos sebezhető jelszavak fenyegetés tooAzure AD felhasználók. Az aktuális biztonsági erőfeszítéseket kapcsolatos további információkért lásd: hello [Microsoft biztonsági az Eszközintelligencia-jelentés](https://www.microsoft.com/security/sir/default.aspx).

### <a name="smart-password-lockout"></a>Intelligens jelszózárolás

Ha az Azure AD a felhasználói jelszó egy potenciális számítógépes-büntetőjogi közben toohack észlel, azt rendelkező intelligens jelszó fiókzárolási hello felhasználói fiókot a zárolása. Az Azure AD úgy van kialakítva, toodetermine hello kockázatának adott bejelentkezési munkameneteket. Hello legfrissebb biztonsági adatok, majd érvénybe lépni fiókzárolási szemantikáját toostop számítógépes fenyegetéseket.

Ha a felhasználó le van tiltva az Azure AD ki, a képernyő következőhöz hasonló toohello egy, a következő:

  ![Kizárva az Azure AD-ből](./media/active-directory-secure-passwords/locked-out-azuread.png)

Más Microsoft-fiók a képernyő következőhöz hasonló toohello egy, a következő:

  ![Kizárva egy Microsoft-fiókból](./media/active-directory-secure-passwords/locked-out-ms-accounts.png)

Jelszó alaphelyzetbe állítása, az Azure Active Directoryban kapcsolatos információkért lásd: hello témakör [az Azure AD az önkiszolgáló jelszó-változtatási az informatikai szakembereknek szóló hello](active-directory-passwords.md).

  >[!NOTE]
  >Ha az Azure AD-rendszergazdaként, érdemes lehet a toouse [Windows Hello](https://www.microsoft.com/windows/windows-hello) tooavoid, hogy a felhasználók regisztrálását hagyományos jelszót létrehozni.
  >

## <a name="next-steps"></a>Következő lépések

* [Hogyan tooupdate a saját jelszavát](active-directory-passwords-update-your-own-password.md)
* [Azure-identitás felügyeleti hello alapjai](fundamentals-identity.md)
* [A jelentés a jelszó-visszaállítási tevékenység](active-directory-passwords-reporting.md)


