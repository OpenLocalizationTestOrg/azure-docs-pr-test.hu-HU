---
title: "Active Directory Identity Protection-forgatókönyv aaaAzure |} Microsoft Docs"
description: "Ismerje meg, az Azure AD Identity Protection miként toolimit hello azon képessége, egy támadó tooexploit egy sérült biztonságú identitás vagy az eszköz és a toosecure identitás vagy egy eszköz, amely korábban gyanús vagy ismert toobe sérült."
services: active-directory
keywords: "az Azure active directory azonosító adatok védelmét, a cloud app discovery, alkalmazások, biztonság, kockázat, kockázati szint, biztonsági rés, biztonsági házirend kezelése"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 60836abf-f0e9-459d-b344-8e06b8341d25
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 6252bc133fc0c0f84800ee245a04bbf62d4cd25b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection-playbook"></a>Az Azure Active Directory Identity Protection-forgatókönyv
Ez a forgatókönyv segítséget:

* Amelyek kockázati eseményekről és biztonsági rések hello Identity Protection környezetben adatok feltöltése
* Kockázati-alapú feltételes hozzáférés szabályzatainak beállítása, és ezek a házirendek hello hatásának tesztelése

## <a name="simulating-risk-events"></a>Kockázati események szimulálása
Ez a szakasz biztosít lépéseket a következő kockázatok eseménytípusok hello szimulálva:

* Bejelentkezések névtelen IP-címről (egyszerű)
* Bejelentkezések ismeretlen helyekről (közepes)
* Lehetetlen odautazás tooatypical helyek (nehéz)

Más kockázati események nem szimulált, biztonságos módon.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Névtelen IP-címről történő bejelentkezések
A kockázat esemény a névtelen proxy IP-címként azonosított IP-cím sikeresen bejelentkezett felhasználók azonosítja. Ezek proxyk olyan személyek számára, akik toohide az eszköz IP-címet használja, és a rosszindulatú behatolással szemben használható.

**névtelen IP, a bejelentkezés toosimulate hajtsa végre a lépéseket követve hello**:

1. Töltse le a hello [Tor böngésző](https://www.torproject.org/projects/torbrowser.html.en).
2. Hello Tor böngészőben nyissa meg a túl[https://myapps.microsoft.com](https://myapps.microsoft.com).   
3. Azt szeretné, hogy a hello tooappear hello fiók hitelesítő adatainak megadásával hello **névtelen IP-címekről bejelentkezések** jelentés.

hello bejelentkezés fog megjelenni a hello Identity Protection-irányítópulton 5 percen belül. 

### <a name="sign-ins-from-unfamiliar-locations"></a>Ismeretlen helyekről történt bejelentkezések
hello ismeretlen helyek kockázat egy egy valós idejű bejelentkezési értékelési mechanizmus, amely korábbi bejelentkezési helyek figyelembe veszi (szélesség, IP / hosszúság és a ASN) toodetermine új / ismeretlen helyét. hello rendszer tárolja a korábbi IP-címek, szélesség / hosszúság, és a ASN-eket, hogy egy felhasználó és ismerős toobe helyeken. A bejelentkezési helye akkor tekinthető ismeri, ha hello bejelentkezési helye nem egyezik hello meglévő ismerős helyeken.

Az Azure Active Directory azonosító adatok védelmét:  

* kezdeti tanulási időszaka 14 nap során, ami azt nem ez a jelző bármely új helyek ismeretlen helyként.
* figyelmen kívül hagyja az olyan ismerős eszközökkel és földrajzilag Bezárás tooan meglévő ismerős hely helyeken bejelentkezéseket.

Ismeretlen helyek toosimulate, vannak toosign egy helyről, és az eszköz, amely hello fiók még nem jelentkezett be az előtt. 

**a bejelentkezés egy ismeretlen helyről toosimulate hajtsa végre a lépéseket követve hello**:

1. Válasszon legalább egy 14 napos bejelentkezési előzményei rendelkező fiókkal. 
2. Módszerekkel:
   
   a. A VPN-kapcsolattal, miközben lépjen túl[https://myapps.microsoft.com](https://myapps.microsoft.com) hello hitelesítő adataival hello fióknevet, amelyet a toosimulate hello kockázat eseménye.
   
   b. Kérje meg egy másik helyre toosign hello fiók hitelesítő adataival (nem ajánlott) a társult.

hello bejelentkezés fog megjelenni a hello Identity Protection-irányítópulton 5 percen belül.

### <a name="impossible-travel-tooatypical-location"></a>Lehetetlen odautazás tooatypical helye
Hello lehetetlen odautazás feltétel szimulálva nem nehéz, mert hello algoritmust használ a machine learning tooweed például lehetetlen odautazás ismerős eszközökről vagy hello directory belüli más felhasználók által használt virtuális magánhálózatok bejelentkezések hamis-tömkelegére kimenő. Ezen felül hello algoritmus igényel 3 too14 nap bejelentkezési előzményeit hello felhasználó kockázati események létrehozásának megkezdése előtt.

**toosimulate egy lehetetlen odautazás tooatypical hely, hajtsa végre a lépéseket követve hello**:

1. A szabványos böngészőben nyissa meg a túl[https://myapps.microsoft.com](https://myapps.microsoft.com).  
2. Adja meg a hello fiók toogenerate lehetetlen odautazás kockázat esemény hello hitelesítő adatait.
3. Módosítsa a felhasználói ügynök. Módosítsa az Internet Explorer felhasználói ügynök fejlesztői eszközök, vagy módosítsa a felhasználói ügynök Firefox vagy felhasználói ügynök kapcsoló bővítményével Chrome.
4. Az IP-címének módosítása. Az IP-címe a VPN-és a Tor bővítmény használatával, vagy egy új gépet az Azure különböző adatközpont dolgozik módosíthatja.
5. Bejelentkezés túl[https://myapps.microsoft.com](https://myapps.microsoft.com) használatával hello ugyanazokat a hitelesítő adatokat, mielőtt és hello előző bejelentkezés után néhány percen belül.

hello bejelentkezés fog megjelenni hello Identity Protection-irányítópult 2 – 4 órán belül.<br>
Hello összetett gépi tanulási modellek jár, mert az beszerzése kivétele nem mentése esély van.<br> Érdemes lehet tooreplicate ezeket a lépéseket a több Azure AD-fiókok.

## <a name="simulating-vulnerabilities"></a>Biztonsági rések szimulálása
Biztonsági rések egy hibás szereplő is kihasználható az Azure AD környezetben gyengeségei miatt. Jelenleg biztonsági rések 3 típusú illesztett az Azure AD Identity Protection használó egyéb szolgáltatásokat az Azure AD. A biztonsági rések jelenik meg a hello Identity Protection-irányítópulton automatikusan után ezek a funkciók be vannak állítva.

* Az Azure AD [többtényezős hitelesítés?](../multi-factor-authentication/multi-factor-authentication.md)
* Az Azure AD [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).
* Az Azure AD [Privileged Identity Management](active-directory-privileged-identity-management-configure.md). 

## <a name="user-compromise-risk"></a>Felhasználó biztonsági sérülés kockázata
**tootest felhasználó biztonsági sérülés kockázat, hajtsa végre a lépéseket követve hello**:

1. Bejelentkezés túl[https://portal.azure.com](https://portal.azure.com) a bérlő globális rendszergazdai hitelesítő adatokkal.
2. Keresse meg a túl**Identity Protection**. 
3. A fő hello **Azure AD Identity Protection** panelen kattintson a **beállítások**. 
4. A hello **portál beállításait** panelen, a **biztonsági szabályok**, kattintson a **felhasználó biztonsági sérülés kockázati**. 
5. A hello **kockázat bejelentkezés** panelen kapcsolja **engedélyezése a szabály** ki, és kattintson a **mentése** beállítások.
6. Egy adott felhasználói fiók szimulálása egy ismeretlen helyekről vagy névtelen IP-cím kockázat esemény. Ez fogja jogosultságszint-emelés hello felhasználói kockázati szintjét az adott felhasználó túl**Közepes**.
7. Várjon néhány percet, és ellenőrizze, hogy felhasználói szinten, a felhasználó érték **Közepes**.
8. Nyissa meg toohello **portál beállításait** panelen.
9. A hello **felhasználó biztonsági sérülés kockázati** panel alatt **engedélyezése a szabály**, jelölje be **a** . 
10. Válasszon egyet az alábbi beállítások hello:
    
    a. tooblock, jelölje be **Közepes** alatt **blokk bejelentkezés**.
    
    b. tooenforce biztonságos jelszó módosítása, jelölje be **Közepes** alatt **többtényezős hitelesítést**.
11. Kattintson a **Save** (Mentés) gombra.
12. Kockázati-alapú feltételes hozzáférés egy felhasználó használata egy emelt szintű kockázati szintjét a bejelentkezéssel most tesztelheti. Ha hello felhasználói kockázat közepes, attól függően, hogy hello konfigurációs házirend, a bejelentkezés vagy letiltása, vagy toochange kényszerítve vannak a jelszavát. 
    <br><br>
    ![Alkalmazástervezési](./media/active-directory-identityprotection-playbook/201.png "forgatókönyv")
    <br>

## <a name="sign-in-risk"></a>Bejelentkezési kockázata
**tootest egy bejelentkezési kockázat, hajtsa végre a lépéseket követve hello:**

1. Bejelentkezés túl[https://portal.azure.com ](https://portal.azure.com) a bérlő globális rendszergazdai hitelesítő adatokkal.
2. Keresse meg a túl**Identity Protection**.
3. A fő hello **Azure AD Identity Protection** panelen kattintson a **beállítások**. 
4. A hello **portál beállításait** panelen, a **biztonsági szabályok**, kattintson a **kockázat bejelentkezés**.
5. A hello ** kockázat bejelentkezés ** panelen válassza **a** alatt **engedélyezése a szabály**. 
6. Válasszon egyet az alábbi beállítások hello:
   
   a. tooblock, jelölje be **Közepes** alatt **blokk jelentkezzen be**
   
   b. tooenforce biztonságos jelszó módosítása, jelölje be **Közepes** alatt **többtényezős hitelesítést**.
7. tooblock, a blokk válassza közepes jelentkezzen be.
8. tooenforce többtényezős hitelesítést, jelölje be **Közepes** alatt **többtényezős hitelesítést**.
9. Kattintson a **Mentés** gombra.
10. Kockázati-alapú feltételes hozzáférés hello ismeretlen helyek szimulál most tesztelheti, vagy névtelen IP kockázati események, mivel azok is **Közepes** események kockázatát.


![Alkalmazástervezési](./media/active-directory-identityprotection-playbook/200.png "forgatókönyv")


## <a name="see-also"></a>Lásd még:
* [Az Azure Active Directory azonosító adatok védelmét](active-directory-identityprotection.md)

