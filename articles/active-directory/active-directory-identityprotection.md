---
title: Active Directory-Identity Protection aaaAzure |} Microsoft Docs
description: "Ismerje meg, az Azure AD Identity Protection miként toolimit hello azon képessége, egy támadó tooexploit egy sérült biztonságú identitás vagy az eszköz és a toosecure identitás vagy egy eszköz, amely korábban gyanús vagy ismert toobe sérült."
services: active-directory
keywords: "az Azure active directory azonosító adatok védelmét, a cloud app discovery, alkalmazások, biztonság, kockázat, kockázati szint, biztonsági rés, biztonsági házirend kezelése"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: ecca4f3cdb65585687cf44a80024f26c7cab22ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection"></a>Azure Active Directory Identity Protection

Az Azure Active Directory azonosító adatok védelmét lehetővé teszi az Azure AD Premium P2 hello kiadás, amely lehetővé teszi:

- A szervezet identitásait érintő lehetséges biztonsági rések észlelése

- Automatikus válaszok toodetected gyanús műveleteket kapcsolódó tooyour szervezet identitásait konfigurálása  

- Vizsgálja meg a gyanús incidensek, és tegye meg a megfelelő lépéseket tooresolve őket   


## <a name="getting-started"></a>Bevezetés

A Microsoft felhőalapú Identitások a több mint egy évtizedben biztonságossá tételére. Azure Active Directory azonosító adatok védelmét, a környezetben, segítségével hello azonos védelmi rendszerek Microsoft toosecure identitások használja.

hello többsége megszegése érvénybe biztonsági sor, amikor a támadók hozzáférést tooan környezet nyerhet ellophatják a felhasználó identitását. Hello évek támadók egyre hatékony, a harmadik féltől származó megszegése követését és kifinomult adathalász támadások használatával lettek. Amint egy támadó hozzáfér tooeven alacsony szintű jogosultságokkal rendelkező felhasználói fiókok, is viszonylag egyszerűen toogain tooimportant vállalati erőforrások eléréséhez oldalirányú mozgás révén azokat.

Ennek következtében kell:

- A jogosultsági szint függetlenül minden identitások védelme

- Proaktív a sérült biztonságú identitások megakadályozása visszaélt folyamatban

Sérült biztonságú identitások felderítésére nem egyszerű feladat. Az Azure Active Directory adaptív gépi tanulási algoritmusok és heurisztikus toodetect rendellenességek észlelését és gyanús incidenseket, amelyek jelzik, potenciálisan sérült identitások szolgáltatást. Ezeket az adatokat használva Identity Protection létrehozza a jelentéseket és riasztásokat, amelyek lehetővé teszik a tooevaluate hello problémákat észlelt, és megfelelő megoldás vagy elvégzett szervizelési műveleteket.

Az Azure Active Directory azonosító adatok védelmét a rendszer több mint egy figyelési és jelentéskészítési eszköz. tooprotect a szervezet identitásait, kockázati-alapú, toodetected problémákat automatikusan válaszol, amikor a rendszer elérte a megadott kockázati szint házirendeket konfigurálhat. Ezek a házirendek továbbá tooother feltételes hozzáférés-szabályozási Azure Active Directory és az EMS biztosítja, vagy automatikusan letilthatja vagy kezdeményezheti, többek között a jelszó alaphelyzetbe állítását és a többtényezős hitelesítés kényszerítése adaptív elvégzett szervizelési műveleteket.


#### <a name="identity-protection-capabilities"></a>Identitás-megelőzési képességek

**Biztonsági rések és kockázatos fiókok észlelése:**  

* Egyéni javaslatok tooimprove nyújtó általános biztonsági állapotát a biztonsági rések kiemelésével
* Bejelentkezési kockázati szintek kiszámítása
* Kockázati szintek felhasználói kiszámítása


**Vizsgáló kockázati események:**

* Értesítésküldési kockázati események
* Megfelelő és környezetfüggő információk alapján kockázati események kivizsgálása
* A munkafolyamatok alapvető tootrack vizsgálatok biztosítása
* Így könnyen elérhetők tooremediation műveletek, például a jelszó alaphelyzetbe állítása

**Kockázati-alapú feltételes hozzáférési házirendek:**

* Házirend toomitigate kockázatos bejelentkezések bejelentkezések blokkolja, vagy a multi-factor authentication kihívások szükségessé tételével.
* Házirend tooblock vagy biztonságos kockázatos felhasználói fiókok
* Házirend toorequire felhasználók tooregister a többtényezős hitelesítés



## <a name="identity-protection-roles"></a>Identity Protection szerepkörök

tooload egyenleg hello felügyeleti tevékenységek Identity Protection implementálásának körül, több szerepköröket rendelhet. Az Azure AD Identity Protection 3 directory szerepkörök támogatja:

| Szerepkör                         | Teheti meg                          | Nem hajtható végre
| :--                          | ---                                |  ---   |
| Globális rendszergazda         | Teljes hozzáférés tooIdentity védelmi, bevezetni, Identity Protection| |
| Biztonsági rendszergazda       | Teljes hozzáférés tooIdentity védelme | Identity Protection előkészítésére, állítsa vissza egy felhasználó jelszavát |
| Biztonsági olvasó              | Csak kész tooIdentity védelme | A bevezetni, Identity Protection, remidiate felhasználók, házirendeket konfigurálhat, jelszavak alaphelyzetbe állítása |




További részletekért lásd: [rendszergazdai szerepkörök hozzárendelése az Azure Active Directoryban](active-directory-assign-admin-roles-azure-portal.md)



## <a name="detection"></a>Észlelés

### <a name="vulnerabilities"></a>Biztonsági rések

Az Azure Active Directory azonosító adatok védelmét a konfigurációs és a biztonsági réseket, amelyek hatással lehetnek a felhasználói identitások észleli. További részletekért lásd: [Azure Active Directory Identity Protection által észlelt biztonsági rések](active-directory-identityprotection-vulnerabilities.md).

### <a name="risk-events"></a>Kockázati események

Az Azure Active Directory adaptív gépi tanulási a algoritmusok és heurisztikus toodetect gyanús műveleteket tooyour kapcsolódó felhasználói identitások használja. hello rendszer létrehoz egy rekordot észlelt gyanús műveleteket. Ezeket a rekordokat kockázati események is ismertek.  
További részletek: [Az Azure Active Directory kockázati eseményei](active-directory-identity-protection-risk-events.md).


## <a name="investigation"></a>Vizsgálat
A használatában Identity Protection keresztül általában hello Identity Protection-irányítópult kezdődik.

![Szervizelés](./media/active-directory-identityprotection/1000.png "szervizelés")

hello irányítópult a következő hozzáférési jogosultságot a következőhöz:

* Például a jelentések **felhasználók meg van jelölve, a kockázat**, **kockázati események** és **biztonsági réseket**
* Beállítások, például hello konfigurálása a **biztonsági házirendek**, **értesítések** és **a multi-factor authentication regisztráció**

Általában le a vizsgálat, amely hello tevékenységek, a naplókat, és olyan releváns információkat kapcsolódó tooa kockázati esemény toodecide szervizelési vagy enyhítési lépések szükségesek, hogy véleményezési hello eljárás, és milyen hello identitás volt a kiindulási pont sérült, és megérteni, hogyan hello sérült azonosító lett megadva.

Ön nagy terhelést jelent a vizsgálat tevékenységek toohello [értesítések](active-directory-identityprotection-notifications.md) Azure Active Directory Protection küld egy e-mailt.

hello alább láthatja a további részletek és kapcsolódó tooan vizsgálat hello lépéseket.  


## <a name="risky-sign-ins"></a>Kockázatos bejelentkezések

Az Azure Active Directory észleli [eseménytípusok kockázati](active-directory-reporting-risk-events.md#risk-event-types) valós idejű és offline állapotban. Minden kockázati esemény észlelhető a bejelentkezés felhasználói a(z) hozzájárul tooa kockázatos bejelentkezés nevű logikai fogalom. Egy kockázatos bejelentkezés egy bejelentkezési kísérlet után előfordulhat, hogy nem elvégzett hello jogos tulajdonos egy felhasználói fiók mutatója.


### <a name="sign-in-risk-level"></a>Bejelentkezési kockázati szint

A bejelentkezési kockázati szintje feltüntetése (magas, közepes vagy alacsony) hello valószínűsége, hogy egy bejelentkezési kísérlet után nem lett végrehajtva hello jogos tulajdonos egy felhasználói fiók.

### <a name="mitigating-sign-in-risk-events"></a>Bejelentkezési kockázati események orvoslása

A megoldás egy művelet toolimit hello azon képessége, egy támadó tooexploit egy sérült biztonságú identitás vagy az eszköz hello identitás vagy az eszköz tooa biztonságos állapot visszaállítása nélkül. A megoldás nem oldja meg a társított hello identitás vagy az eszköz korábbi bejelentkezési kockázati eseményekről.

kockázatos toomitigate bejelentkezéseket automatikusan, konfigurálhatja a bejelentkezési kockázat biztonsági policicies. Ezek a házirendek célszerű hello kockázati szintjét hello felhasználói vagy hello bejelentkezési tooblock kockázatos bejelentkezéseket vagy hello felhasználói tooperform többtényezős hitelesítést. Ezek a műveletek előfordulhat, hogy egy támadó a kihasználhassa egy ellopott identitás toocause kárt, és előfordulhat, hogy adjon bizonyos idő toosecure hello identitás.

### <a name="sign-in-risk-security-policy"></a>Bejelentkezési kockázat biztonsági házirend
A kockázat bejelentkezési házirend egy feltételes hozzáférési szabályzatot, amely hello kockázati tooa adott bejelentkezés és azok mérséklési előre meghatározott feltételt és a szabályok alapján alkalmazza.

![Bejelentkezési kockázat házirendnek](./media/active-directory-identityprotection/1014.png "bejelentkezési kockázat házirendnek")

Az Azure AD Identity Protection segítségével felügyelheti a hello megoldás a kockázatos bejelentkezések azáltal, hogy:

* Állítsa be a felhasználók és csoportok hello házirend vonatkozik hello:

    ![Bejelentkezési kockázat házirendnek](./media/active-directory-identityprotection/1015.png "bejelentkezési kockázat házirendnek")
* Hello bejelentkezési kockázati szint küszöbértéke (alacsony, közepes vagy magas), amely elindítja a hello házirend beállítása:

    ![Bejelentkezési kockázat házirendnek](./media/active-directory-identityprotection/1016.png "bejelentkezési kockázat házirendnek")
* Set hello vezérlők toobe lépnek érvénybe, ha elindítja a hello házirend:  

    ![Bejelentkezési kockázat házirendnek](./media/active-directory-identityprotection/1017.png "bejelentkezési kockázat házirendnek")
* A házirend kapcsoló hello állapotát:

    ![Többtényezős hitelesítés regisztrációs](./media/active-directory-identityprotection/403.png "MFA-regisztráció")
* Ellenőrizze, és értékelje hello hatással bír a változtatás aktiválás előtt:

    ![Bejelentkezési kockázat házirendnek](./media/active-directory-identityprotection/1018.png "bejelentkezési kockázat házirendnek")

#### <a name="what-you-need-tooknow"></a>Mit kell tooknow
Konfigurálhatja a bejelentkezési kockázat biztonsági házirend toorequire multi-factor Authentication hitelesítést:

![Bejelentkezési kockázat házirendnek](./media/active-directory-identityprotection/1017.png "bejelentkezési kockázat házirendnek")

Azonban biztonsági okok miatt ez a beállítás csak akkor működik a felhasználók a multi-factor authentication már regisztrált. Ha egy felhasználóhoz, aki még nincs regisztrálva a többtényezős hitelesítés hello feltétel toorequire a multi-factor authentication teljesül, hello felhasználó le van tiltva.

Ajánlott eljárásként Ha azt szeretné, hogy a multi-factor authentication toorequire a kockázatos bejelentkezések, akkor a következőket:

1. Hello engedélyezése [a multi-factor authentication regisztráció házirend](#multi-factor-authentication-registration-policy) hello érintett felhasználók.
2. Hello érintett felhasználók toologin egy kockázatos munkamenet tooperform a multi-factor Authentication regisztráció megkövetelése

A lépések végrehajtása biztosítja, hogy a multi-factor authentication a kockázatos bejelentkezéshez szükséges.

#### <a name="best-practices"></a>Ajánlott eljárások
Válassza a **magas** küszöbérték hello száma szabályzat elindul, és minimálisra csökkenti a hello hatás toousers csökkenti.  

Azonban nem tartalmazza **alacsony** és **Közepes** bejelentkezések meg van jelölve, a kockázat hello házirend, amely nem blokkolhatják a támadó a sérült biztonságú identitás kihasználhassa.

Ha a beállítás hello házirend,

* Felhasználók, akik nem / nem lehet a többtényezős hitelesítés kihagyása
* Kizárhat felhasználókat azokon a területeken, ahol hello házirend engedélyezése nincs gyakorlati (például nincs hozzáférési toohelpdesk)
* Kizárt felhasználók valószínűleg toogenerate nagy mennyiségű hamis-figyelmeztetéséket (fejlesztők, adatbiztonsági elemzők)
* Használja a **magas** küszöbérték alatt kezdeti házirend bevezetési, vagy ha a felhasználók számára kihívást kell minimálisra csökkenthető.
* Használja a **alacsony** küszöbértéket, ha a szervezet nagyobb biztonságot igényel. Válassza a **alacsony** küszöbérték vezet be további felhasználói bejelentkezési kihívás, de a biztonság fokozása érdekében.

javasolt az alapértelmezett, a legtöbb szervezet tooconfigure szabály hello egy **Közepes** küszöbérték toostrike használhatóság és a biztonság egyensúlyára.

hello bejelentkezési kockázat házirendnek van:

* Alkalmazott tooall böngésző forgalom és a modern hitelesítést használó bejelentkezéseket.
* Nem alkalmazott tooapplications helyen összevont hello IDP, például az AD FS hello WS-Trust végpont letiltása a régebbi biztonsági protokollok használatával.

Hello **kockázati események** hello Identity Protection konzolon lap felsorolja az összes eseményt:

* Ez a házirend alkalmazott
* Tekintse át a hello tevékenység, és megtudhatja, hogy hello művelet megfelelő volt-e vagy sem

Hello áttekintését kapcsolódó felhasználói élményt, lásd:

* [Kockázatos bejelentkezési helyreállítási](active-directory-identityprotection-flows.md#risky-sign-in-recovery)
* [Kockázatos bejelentkezés letiltva](active-directory-identityprotection-flows.md#risky-sign-in-blocked)  
* [Az Azure AD Identity Protection bejelentkezési élmény](active-directory-identityprotection-flows.md)  

**tooopen hello kapcsolódó konfigurációs párbeszédpanel**:

- A hello **Azure AD Identity Protection** paneljén, hello **konfigurálása** területén kattintson **bejelentkezési kockázat házirendnek**.

    ![Felhasználói ridk házirend](./media/active-directory-identityprotection/1014.png "felhasználói ridk házirend")



## <a name="users-flagged-for-risk"></a>Kockázatosként megjelölt felhasználók

Összes aktív [kockázati események](active-directory-identity-protection-risk-events.md) , amely az Azure Active Directory által észlelt, a felhasználó hozzájárul tooa felhasználói kockázat nevű logikai fogalom. A kockázat megjelölt felhasználó egy egy felhasználói fiókot, amely hatékonyságát.

![Kockázatosként megjelölt felhasználók](./media/active-directory-identityprotection/1200.png)


### <a name="user-risk-level"></a>Felhasználói kockázati szint

Egy felhasználó kockázati szint feltüntetése (magas, közepes vagy alacsony) hello valószínűsége, hogy sérült hello felhasználó identitása. Ez alapján van kiszámítva hello felhasználói kockázati események társított a felhasználó identitását.

hello a kockázati események állapota vagy **aktív** vagy **lezárva**. Csak a megfelelő események kockázatát **aktív** toohello felhasználói kockázati szint számítási közre.

hello felhasználói kockázati szint számítja ki a következő bemenet hello:

* Hello felhasználói érintő aktív kockázati események
* Ezek az események kockázati szint
* E került sor esetleg elvégzett szervizelési műveleteket

![Felhasználói kockázatok](./media/active-directory-identityprotection/1031.png "felhasználói kockázatok")

Hello felhasználói kockázati szintek toocreate feltételes hozzáférési házirendjeivel, amely megakadályozhatja a kockázatos felhasználók jelentkezik be, vagy őket toosecurely módosíthatják jelszavukat.

### <a name="closing-risk-events-manually"></a>Kockázati események lezárása manuálisan

A legtöbb esetben elvégzett szervizelési műveleteket, mint például egy biztonságos jelszó-átállítási tooautomatically Bezárás kockázati események lépnek meg. Azonban ez nem mindig lehet lehetséges.  
Ez helyzet, például hello, ha:

* A felhasználó Active kockázati események törölve lett
* A vizsgálat azt jelzi, hogy a jelentett kockázat esemény hello hiteles felhasználó által lett-e végre.

Mivel a kockázati eseményekről, amelyek **aktív** toohello felhasználói kockázat számítási közre, előfordulhat, hogy a kockázati szintjét alacsonyabb kockázati események manuálisan bezárásával toomanually.  
Vizsgálat hello során választhat tootake ezen műveletek toochange hello állapotát a kockázati események bármelyikét:

![Műveletek](./media/active-directory-identityprotection/34.png "műveletek")

* **Hárítsa el** – Ha után vizsgálja meg a kockázati események, egy megfelelő szervizelési művelet kívül Identity Protection tartott, és úgy véli, hogy hello kockázat esemény tekintendő bezárul, be van jelölve hello esemény megoldva. Megoldott események hello kockázat esemény állapot tooClosed állítja be, és hello kockázat esemény már nem járul toouser kockázata.
* **Jelölje meg a vakriasztások** – néhány esetben is vizsgálja meg a kockázati események és helytelenül lett megjelölve, egy kockázatos felderítése. Csökkentheti az ilyen előfordulások száma hello hello kockázat esemény vakriasztások megjelölésével. Ez segít hello gépi tanulási a algoritmusok tooimprove hello besorolás jövőbeli hello hasonló események. hello vakriasztások események állapota túl**lezárva** és már nem hozzá toouser kockázata.
* **Figyelmen kívül hagyása** – Ha bármilyen szervizelési művelet nem tett, de szeretné hello kockázat esemény toobe eltávolítása hello aktív listájáról, jelölheti meg a kockázati események figyelmen kívül hagyása és hello eseményállapot be lesz zárva. Figyelmen kívül hagyott események nem járulnak toouser kockázata. Ezt a beállítást csak ritka esetekben használható.
* **Aktiválja újra** -kockázati események manuálisan lezárt (kiválasztásával **megoldásához**, **vakriasztás**, vagy **figyelmen kívül hagyása**) is újra kell aktiválni, hello beállítása eseményállapot biztonsági túl**aktív**. Újraaktivált kockázati események toohello felhasználói kockázati szint számítási függ. Nem lehet újraaktiválni a kockázati eseményekről zárva a szervizelésen keresztül (például egy biztonságos jelszó-átállítási).

**tooopen hello kapcsolódó konfigurációs párbeszédpanel**:

1. A hello **Azure AD Identity Protection** panelen, a **vizsgálat**, kattintson a **kockázati események**.

    ![Manuális jelszó-átállítási](./media/active-directory-identityprotection/1002.png "manuális jelszó alaphelyzetbe állítása")
2. A hello **kockázati események** listában, kattintson a kockázata.

    ![Manuális jelszó-átállítási](./media/active-directory-identityprotection/1003.png "manuális jelszó alaphelyzetbe állítása")
3. Hello kockázat paneljén kattintson jobb gombbal a felhasználó.

    ![Manuális jelszó-átállítási](./media/active-directory-identityprotection/1004.png "manuális jelszó alaphelyzetbe állítása")

### <a name="closing-all-risk-events-for-a-user-manually"></a>Minden kockázati események a felhasználó manuálisan bezárása
Helyett manuálisan bezárása külön-külön kockázati események egy felhasználó, Azure Active Directory azonosító adatok védelmét is biztosít a metódus tooclose összes esemény egy felhasználó egy kattintással.

![Műveletek](./media/active-directory-identityprotection/2222.png "műveletek")

Amikor rákattint **hagyja figyelmen kívül az összes esemény**, az összes esemény be van zárva, és hatással hello felhasználó már nem veszélyben.

### <a name="remediating-user-risk-events"></a>Felmérésével felhasználói kockázati események

A szervizelés egy művelet toosecure identitás vagy egy eszköz, amely korábban gyanús vagy toobe ismert sérült. A javítási művelet visszaállítja a hello identitás vagy az eszköz tooa biztonságos állapotát, és oldja fel az előző kockázati események társított hello identitás vagy az eszköz.

tooremediate felhasználói kockázati eseményekről, a következőket teheti:

* Hajtsa végre kézzel egy biztonságos jelszó alaphelyzetbe állítása tooremediate felhasználói kockázati események
* Konfigurálja a felhasználói kockázat biztonsági házirend toomitigate vagy automatikusan kijavítani a felhasználó kockázati események
* Hello fertőzött eszköz visszaállítása  

#### <a name="manual-secure-password-reset"></a>Manuális biztonságos jelszó alaphelyzetbe állítása
A biztonságos jelszó alaphelyzetbe állítása egy hatékony szervizelése sok kockázati eseményekről, és végrehajtásakor, automatikusan a kockázati eseményekről bezárja, és újraszámítja hello felhasználói kockázati szintjét. Hello Identity Protection-irányítópult tooinitiate egy kockázatos felhasználó új jelszót is használhatja.

hello kapcsolódó párbeszédpanel biztosít két különböző módszereket tooreset jelszót:

**Jelszó-átállítási** – Itt adhatja meg **hello felhasználói tooreset a jelszó szükséges** tooallow hello felhasználói tooself-helyreállítás, ha hello a felhasználó a multi-factor authentication van regisztrálva. Hello felhasználói tovább bejelentkezés során a hello felhasználó multi-factor Authentication hitelesítést sikeresen ellenőrző kérdés szükséges toosolve és majd, a kényszerített toochange hello jelszó lesz. Ez a lehetőség nem érhető el, ha hello felhasználói fiók már nem regisztrált multi-factor Authentication hitelesítést.

**Ideiglenes jelszó** – Itt adhatja meg **létrehozni egy ideiglenes jelszót** tooimmediately érvénytelenné válnak a meglévő hello jelszó, és hozzon létre egy hello felhasználó új ideiglenes jelszót. Hello új ideiglenes jelszó tooan másodlagos e-mail címére hello felhasználó vagy felhasználói toohello manager küldése. Mivel hello jelszó ideiglenes, hello felhasználó fog felszólító toochange hello jelszó bejelentkezés után.

![Házirend](./media/active-directory-identityprotection/1005.png "házirend")

**tooopen hello kapcsolódó konfigurációs párbeszédpanel**:

1. A hello **Azure AD Identity Protection** panelen kattintson a **felhasználók meg van jelölve, a kockázat**.

    ![Manuális jelszó-átállítási](./media/active-directory-identityprotection/1006.png "manuális jelszó alaphelyzetbe állítása")
2. A felhasználók hello listáról válassza ki a felhasználó legalább egy kockázati események.

    ![Manuális jelszó-átállítási](./media/active-directory-identityprotection/1007.png "manuális jelszó alaphelyzetbe állítása")
3. Hello felhasználói paneljén kattintson **jelszó-átállítási**.

    ![Manuális jelszó-átállítási](./media/active-directory-identityprotection/1008.png "manuális jelszó alaphelyzetbe állítása")

### <a name="user-risk-security-policy"></a>Felhasználói kockázat biztonsági házirend
A felhasználó kockázati biztonsági házirend egy feltételes hozzáférési szabályzatot, amely kiértékeli a hello kockázati szint tooa adott felhasználó és előre meghatározott feltételt és a szabályok alapján a szervizelés és a megoldás műveleteket hajt végre.

![Felhasználói ridk házirend](./media/active-directory-identityprotection/1009.png "felhasználói ridk házirend")

Az Azure AD Identity Protection segítségével felügyelheti a hello megoldás, a javítási azáltal, hogy kockázat megjelölt felhasználók:

* Állítsa be a felhasználók és csoportok hello házirend vonatkozik hello:

    ![Felhasználói ridk házirend](./media/active-directory-identityprotection/1010.png "felhasználói ridk házirend")
* Hello felhasználói kockázati szint küszöbértéke (alacsony, közepes vagy magas), amely elindítja a hello házirend beállítása:

    ![Felhasználói ridk házirend](./media/active-directory-identityprotection/1011.png "felhasználói ridk házirend")
* Set hello vezérlők toobe lépnek érvénybe, ha elindítja a hello házirend:

    ![Felhasználói ridk házirend](./media/active-directory-identityprotection/1012.png "felhasználói ridk házirend")
* A házirend kapcsoló hello állapotát:

    ![Felhasználói ridk házirend](./media/active-directory-identityprotection/403.png "MFA-regisztráció")
* Ellenőrizze, és értékelje hello hatással bír a változtatás aktiválás előtt:

    ![Felhasználói ridk házirend](./media/active-directory-identityprotection/1013.png "felhasználói ridk házirend")

Válassza a **magas** küszöbérték hello száma szabályzat elindul, és minimálisra csökkenti a hello hatás toousers csökkenti.
Azonban nem tartalmazza **alacsony** és **Közepes** hello házirendet, amely előfordulhat, hogy nem biztonságos identitások vagy eszközöket, amelyek fenyegeti megjelölt felhasználók korábban gyanús vagy sérült toobe ismert.

Ha a beállítás hello házirend,

* Kizárt felhasználók valószínűleg toogenerate nagy mennyiségű hamis-figyelmeztetéséket (fejlesztők, adatbiztonsági elemzők)
* Kizárhat felhasználókat azokon a területeken, ahol hello házirend engedélyezése nincs gyakorlati (például nincs hozzáférési toohelpdesk)
* Használja a **magas** küszöbérték alatt kezdeti házirend bevezetési, vagy ha a felhasználók számára kihívást kell minimálisra csökkenthető.
* Használja a **alacsony** küszöbértéket, ha a szervezet nagyobb biztonságot igényel. Válassza a **alacsony** küszöbérték vezet be további felhasználói bejelentkezési kihívás, de a biztonság fokozása érdekében.

javasolt az alapértelmezett, a legtöbb szervezet tooconfigure szabály hello egy **Közepes** küszöbérték toostrike használhatóság és a biztonság egyensúlyára.

Hello áttekintését kapcsolódó felhasználói élményt, lásd:

* [Sérült a fiók a helyreállítási folyamat](active-directory-identityprotection-flows.md#compromised-account-recovery).  
* [Sérült a fiók blokkolva van folyamata](active-directory-identityprotection-flows.md#compromised-account-blocked).  

**tooopen hello kapcsolódó konfigurációs párbeszédpanel**:

- A hello **Azure AD Identity Protection** paneljén, hello **konfigurálása** területén kattintson **felhasználói kockázat házirendnek**.

    ![Felhasználói ridk házirend](./media/active-directory-identityprotection/1009.png "felhasználói ridk házirend")

### <a name="mitigating-user-risk-events"></a>Felhasználói kockázati események orvoslása
A rendszergazdák állíthatja be egy felhasználó kockázat biztonsági házirend tooblock felhasználók bejelentkezés után attól függően, hogy hello kockázati szintjét.

Blokkolja a bejelentkezés:

* Megakadályozza, hogy a hello hello érintett felhasználó új felhasználói kockázati események előállítását
* Lehetővé teszi, hogy a rendszergazdák toomanually hello felhasználói identitás érintő hello kockázati eseményekről megoldásáról, majd visszaállítja azt tooa biztonsági állapota



## <a name="multi-factor-authentication-registration-policy"></a>A multi-factor authentication regisztráció házirend
Az Azure többtényezős hitelesítés a ellenőrzésére, akik csupán felhasználónévvel és jelszóval hello használatát igénylő módszer. Biztonsági toouser bejelentkezéseket és tranzakciókat második réteget biztosít.  
Azt javasoljuk, hogy az Azure többtényezős hitelesítés a felhasználói bejelentkezések szükséges el, mert azt:

* Erős hitelesítés, könnyen ellenőrzési lehetőségek széles kézbesíti
* A szervezet tooprotect előkészítésében kulcsfontosságú szerepet játszik, és a fiók biztonság sérüléseinek helyreállítása

![Felhasználói ridk házirend](./media/active-directory-identityprotection/1019.png "felhasználói ridk házirend")

További részletekért lásd: [Mi az Azure multi-factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)

Az Azure AD Identity Protection segítségével felügyelheti a hello kiépítése a multi-factor authentication regisztráció úgy konfigurálja a házirendet, amely lehetővé teszi:

* Állítsa be a felhasználók és csoportok hello házirend vonatkozik hello:

    ![Többtényezős hitelesítési szabályzat](./media/active-directory-identityprotection/1020.png "többtényezős hitelesítési szabályzat")
* Set hello vezérlők toobe lépnek érvénybe, ha elindítja a hello házirend::  

    ![Többtényezős hitelesítési szabályzat](./media/active-directory-identityprotection/1021.png "többtényezős hitelesítési szabályzat")
* A házirend kapcsoló hello állapotát:

    ![Többtényezős hitelesítési szabályzat](./media/active-directory-identityprotection/403.png "többtényezős hitelesítési szabályzat")
* Hello aktuális regisztrációs állapotának megtekintése:

    ![Többtényezős hitelesítési szabályzat](./media/active-directory-identityprotection/1022.png "többtényezős hitelesítési szabályzat")

Hello áttekintését kapcsolódó felhasználói élményt, lásd:

* [A multi-factor authentication regisztrációs folyamat](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).  
* [Bejelentkezés során lép az Azure AD Identity Protection](active-directory-identityprotection-flows.md).  

**tooopen hello kapcsolódó konfigurációs párbeszédpanel**:

- A hello **Azure AD Identity Protection** paneljén, hello **konfigurálása** kattintson **a multi-factor authentication regisztráció**.

    ![Többtényezős hitelesítési szabályzat](./media/active-directory-identityprotection/1019.png "többtényezős hitelesítési szabályzat")

## <a name="next-steps"></a>Következő lépések
* [9. csatornán: Az Azure AD és az Identity: Identity Protection előzetes kiadásának](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

* [Az Azure Active Directory-identitás védelem engedélyezése](active-directory-identityprotection-enable.md)

* [Azure Active Directory Identity Protection által észlelt biztonsági rések](active-directory-identityprotection-vulnerabilities.md)

* [Az Azure Active Directory kockázati események](active-directory-identity-protection-risk-events.md)

* [Az Azure Active Directory Identity Protection-értesítések](active-directory-identityprotection-notifications.md)

* [Az Azure Active Directory Identity Protection-forgatókönyv](active-directory-identityprotection-playbook.md)

* [Az Azure Active Directory-Identity Protection-szószedet](active-directory-identityprotection-glossary.md)

* [Az Azure AD Identity Protection bejelentkezési élmény](active-directory-identityprotection-flows.md)

* [Az Azure Active Directory Identity Protection - hogyan toounblock felhasználók](active-directory-identityprotection-unblock-howto.md)

* [Ismerkedés az Azure Active Directory azonosító adatok védelmét és a Microsoft Graph](active-directory-identityprotection-graph-getting-started.md)
