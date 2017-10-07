---
title: "aaaAzure Active Directory kockázati események |} Microsoft Docs"
description: "Ez a témakör Mik azok a kockázati események részletes áttekintést nyújt."
services: active-directory
keywords: "az Azure active directory azonosító adatok védelmét, biztonság, kockázat, kockázati szint, biztonsági rést, biztonsági házirend"
author: MarkusVi
manager: femila
ms.assetid: fa2c8b51-d43d-4349-8308-97e87665400b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: d771c1f43707744aac728c4f72000d855cbd6e1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-risk-events"></a>Az Azure Active Directory kockázati események

hello többsége megszegése érvénybe biztonsági sor, amikor a támadók hozzáférést tooan környezet nyerhet ellophatják a felhasználó identitását. Sérült biztonságú identitások felderítésére nem egyszerű feladat. Az Azure Active Directory adaptív gépi tanulási a algoritmusok és heurisztikus toodetect gyanús műveleteket kapcsolódó tooyour felhasználói fiókokat használ. Minden észlelt gyanús művelet tárolódik nevű rekord *kockázat esemény*.

Jelenleg Azure Active Directory észlelt kockázati események típusai hat:

- [Kiszivárgott hitelesítő adatokkal rendelkező felhasználók](#leaked-credentials) 
- [Névtelen IP-címekről bejelentkezések](#sign-ins-from-anonymous-ip-addresses) 
- [Lehetetlen odautazás tooatypical helyek](#impossible-travel-to-atypical-locations) 
- [Bejelentkezések ismeretlen helyekről](#sign-in-from-unfamiliar-locations)
- [Fertőzött eszközökről bejelentkezések](#sign-ins-from-infected-devices) 
- [Bejelentkezések gyanús tevékenységeket IP-címekről](#sign-ins-from-ip-addresses-with-suspicious-activity) 


![Kockázati esemény](./media/active-directory-reporting-risk-events/91.png)

Ez a témakör minden, a rendszer milyen kockázati események részletes áttekintést nyújt, és hogyan használhatja őket tooprotect az Azure AD-identitásai.


## <a name="risk-event-types"></a>Kockázati események típusai

hello kockázat esemény típusa tulajdonság gyanús hello a művelethez a kockázat eseményrekord azonosító lett létrehozva.  
A Microsoft hello észlelési folyamat során a folyamatos beruházások értékét előfordulhat, hogy:

- A meglévő kockázati események fejlesztései toohello észlelési pontossága 
- Új kockázat eseménytípusok, a jövőbeli hello hozzáadni

### <a name="leaked-credentials"></a>Kiszivárgott hitelesítő adatok

Amikor internetes bűnözők érvényes jelszavak jogos felhasználók hibát okoz, hello bűnözők gyakran ossza meg ezeket a hitelesítő adatokat. Általában ehhez könyvelés nyilvánosan hello sötét webhelyek és a Beillesztés helyek vagy kereskedelmi vagy hello hitelesítő adatok hello fekete piacon kínál. hello Microsoft szivárgását hitelesítő szolgáltatás szerez be a felhasználónév / jelszó párokat nyilvános és a sötét webhelyek figyelése és a használata:

- Kutatók
- Értve a rendészeti szerveket
- A Microsoft biztonsági csoportok
- Más megbízható források 

Ha szerez be a hello szolgáltatás felhasználónév / jelszó párok, amelyek veti össze AAD-felhasználókat aktuális érvényes hitelesítő adatokat. Amikor a program egyezést talál, az azt jelenti, hogy a jelszó feltörték, és egy *elszivárgott a hitelesítő adatok kockázat esemény* jön létre.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Névtelen IP-címről történő bejelentkezések

A kockázat esemény a névtelen proxy IP-címként azonosított IP-cím sikeresen bejelentkezett felhasználók azonosítja. Ezek proxyk olyan toohide számára az eszköz IP-címet használja, és felhasználhatja a rosszindulatú behatolással szemben.


### <a name="impossible-travel-tooatypical-locations"></a>Lehetetlen odautazás tooatypical helyek

A kockázat esemény két bejelentkezéseket földrajzilag távoli helyről, ahol hello helyek közül legalább egy is lehet nem típusos hello felhasználóhoz, korábbi viselkedés azonosítja. Ezenkívül hello idő közötti hello két bejelentkezések rövidebb, mint hello rendelkezik szükséges idő hello felhasználói tootravel hello első hely toohello a második, a hello jelzi, hogy egy másik felhasználó használja ugyanazt hitelesítő adatokat. 

A gépi tanulási algoritmus figyelmen kívül hagyja a nyilvánvaló "*téves*" hozzájáruló toohello lehetetlen odautazás feltétel, például a virtuális magánhálózatok és rendszeresen hello szervezeten belüli más felhasználók által használt helyek.  hello rendszer kezdeti tanulási időszaka 14 napos, amely során egy új felhasználó bejelentkezési viselkedésének Tanulja meg.

### <a name="sign-in-from-unfamiliar-locations"></a>Bejelentkezés ismeretlen helyekről

A kockázat eseménytípus úgy ítéli meg, bejelentkezési helyek túli (szélesség, IP / hosszúság és a ASN) toodetermine új / ismeretlen helyét. hello rendszer fent említett helyeken, felhasználó által használt kapcsolatos információkat tárolja, és úgy ítéli meg ezeket a "megszokott" helyeket. hello kockázat esemény akkor váltódik ki, ha hello bejelentkezés történik, amely még nem ismeri helyek hello listájának helyről. hello rendszer kezdeti tanulási időszaka a 30 nap során, ami azt nem ez a jelző bármely új helyek ismeretlen helyként. hello rendszer is figyelmen kívül hagyja az ismerős eszközről bejelentkezéseket, és földrajzi helyeken zárja be a tooa ismerős helyét. 

### <a name="sign-ins-from-infected-devices"></a>Bejelentkezések fertőzött eszközökről

A kockázat esemény ismert tooactively botot-kiszolgálóval való kommunikációhoz, kártevővel fertőzött eszközökről származó bejelentkezések azonosítja. Ez határozza meg IP-címek hello felhasználó-eszköz IP-címek, amelyek szerepelnek a botnetes kiszolgálóhoz volt ellen használatával történik. 

### <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Bejelentkezések gyanús tevékenységeket mutató IP-címekkel
A kockázat eseménytípus azonosítja az IP-címek, ahol nagyszámú sikertelen bejelentkezési kísérlet volt, körében tapasztalható több felhasználói fiókot, egy rövid időtartamra vonatkozóan. Ez megegyezik a támadók által használt IP-címek adatforgalmi mintái jellemzően, és a fiókok vagy már vagy sérült toobe kapcsolatos erős mutatója. Ez az a gépi tanulási algoritmus figyelmen kívül hagyja a nyilvánvaló "*hamis-figyelmeztetéséket*", például az IP-címek, amelyek rendszeresen hello szervezeten belüli más felhasználók által használt.  hello rendszer időszaka kezdeti tanulási 14 napos ahol megtanulja a hello bejelentkezési viselkedését egy új felhasználó és az új bérlőhöz.


## <a name="detection-type"></a>Észlelési típusa

hello észlelési type tulajdonság egy (valós idejű vagy kapcsolat nélküli) hello észlelési időkeretre kockázat esemény esetében.  
Jelenleg a legtöbb kockázati események észlelésének offline utó-feldolgozási művelet után hello kockázat esemény történt.

a következő táblázat hello hello időn szükséges egy észlelési típus tooshow kapcsolódó jelentés sorolja fel:

| Észlelési típusa | Jelentéskészítési késés |
| --- | --- |
| Valós idejű | 5 too10 perc |
| Kapcsolat nélküli módban | 2 too4 üzemideje (óra) |


Kapcsolódó hello kockázat különböző Azure Active Directory észlel hello észlelési típusok a következők:

| Kockázati esemény típusa | Észlelési típusa |
| :-- | --- | 
| [Kiszivárgott hitelesítő adatokkal rendelkező felhasználók](#leaked-credentials) | Kapcsolat nélküli módban |
| [Névtelen IP-címekről bejelentkezések](#sign-ins-from-anonymous-ip-addresses) | Valós idejű |
| [Lehetetlen odautazás tooatypical helyek](#impossible-travel-to-atypical-locations) | Kapcsolat nélküli módban |
| [Bejelentkezések ismeretlen helyekről](#sign-in-from-unfamiliar-locations) | Valós idejű |
| [Fertőzött eszközökről bejelentkezések](#sign-ins-from-infected-devices) | Kapcsolat nélküli módban |
| [Bejelentkezések gyanús tevékenységeket IP-címekről](#sign-ins-from-ip-addresses-with-suspicious-activity) | Kapcsolat nélküli módban|


## <a name="risk-level"></a>Kockázati szint

hello kockázati szint a kockázati események tulajdonsága egy kijelző hello súlyossági és hello abban, hogy a kockázati esemény (magas, közepes vagy alacsony). Ez a tulajdonság segít tooprioritize hello műveletek végre kell hajtania. 

hello kockázat esemény súlyossága hello hello jel hello erősségével egy előrejelzőjének az identitás illetéktelen behatolásoknak meghatalmazottként képviseli.  
hello abban, hogy egy mutató a vakriasztások hello lehetőségét. 

Például: 

* **Magas**: magas megbízhatósági és magas súlyosságú kockázat esemény. Ezek az események olyan erős mutatók hello felhasználói identitás feltörték, és egyetlen felhasználói fiókot sem érintett azonnal szervizelni kell.

* **Közepes**: magas súlyosságú, de alacsonyabb abban, hogy kockázat esemény, vagy fordítva. Ezek az események potenciálisan veszélyes, és egyetlen felhasználói fiókot sem érintett szervizelni kell.

* **Alacsony**: alacsony abban, hogy és alacsony súlyosságú kockázat esemény. Ez az esemény egy azonnali beavatkozásra nincs szükség lehet, de más kockázati események kombinálva előfordulhat biztonsága sérül, amely identitás hello jelzi.

![Kockázati szint](./media/active-directory-reporting-risk-events/01.png)

### <a name="leaked-credentials"></a>Kiszivárgott hitelesítő adatok

Kockázati események besorolt hitelesítő adatok szivárgását egy **magas**, mert egy tiszta arra vonatkozóan, hogy hello felhasználónév és jelszó-e a rendelkezésre álló tooan támadó biztosítanak.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Névtelen IP-címről történő bejelentkezések

a kockázat eseménytípus hello kockázati szintje **Közepes** mert névtelen IP-cím nincs egy fiók biztonsági sérülése a jelzi.  
Azt javasoljuk, hogy azonnal forduljon hello felhasználói tooverify Ha melyikét használta éppen a névtelen IP-címeket.


### <a name="impossible-travel-tooatypical-locations"></a>Lehetetlen odautazás tooatypical helyek

Lehetetlen odautazás az általában jól jelzi, hogy egy támadó képes toosuccessfully bejelentkezés volt-e. Hamis-pozitív fordulhat azonban amikor egy felhasználó utazik, egy új eszközt használ, vagy egy hello szervezeten belüli más felhasználók által általában nem használ VPN-kapcsolattal. Hamis – figyelmeztetéséket egy másik forrása alkalmazásokat helytelenül kiszolgáló IP-címek ügyfél IP-címek, amelyek hello látszatát zajlik, ahol az alkalmazás csomagazonosítóját háttér-hello adatközpontból üzemelteti a bejelentkezések (gyakran ezek a Microsoft adatközpontjaiban amely látszatát hello a bejelentkezések lefolyása a Microsoft tulajdonában lévő IP-címek). Eredményeképpen ezek hamis pozitív hello kockázati szintjét a kockázat esemény van **Közepes**.

> [!TIP]
> A kockázat eseménytípus jelentett hamis positves mennyisége hello segítségével csökkenthető [helyek nevű](active-directory-named-locations.md). 

### <a name="sign-in-from-unfamiliar-locations"></a>Bejelentkezés ismeretlen helyekről

Ismeretlen helyek egy erős annak jelzése, hogy egy támadó képes toouse egy ellopott identitást biztosíthat. Hamis – pozitív akkor fordulhat elő, amikor egy felhasználó utazik, próbálja ki az új eszköz vagy egy új VPN-t használja. Hatására a vakriasztások hello kockázati szintjét az esemény típusa van **Közepes**.

### <a name="sign-ins-from-infected-devices"></a>Bejelentkezések fertőzött eszközökről

A kockázat esemény IP-címek, nem a felhasználói eszközök azonosítja. Ha több eszköz egyetlen IP-címnek mögött találhatók, és a csak néhány van egy botot hálózati, a más eszközökről származó bejelentkezések a eseményindító ezt az eseményt szükségtelenül, ez az a kockázat esemény osztályozásához hello OK, **alacsony**.  

Azt javasoljuk, hogy forduljon hello felhasználói, és minden hello felhasználói eszközök. Lehetőség arra is, hogy a felhasználó személyes eszköz fertőzött, vagy a korábbiak, hogy valaki más használta egy fertőzött eszköz hello azonos IP-cím hello felhasználóként. Víruskereső szoftver még nem azonosított, és azt is jelezheti, felhasználói rossz a szokásokat, amelyek oka hello eszköz toobecome fertőzött kártevők által fertőzött eszközök gyakran fertőzött.

További információ tooaddress kártevőszoftver-fertőzések, lásd: hello [Kártevőkezelési központ](http://go.microsoft.com/fwlink/?linkid=335773&clcid=0x409).


### <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Bejelentkezések gyanús tevékenységeket mutató IP-címekkel

Azt javasoljuk, hogy forduljon hello felhasználói tooverify, ha azokat ténylegesen bejelentkezett lett megjelölve, a gyanús IP-címről. esemény hello kockázati szintje "**Közepes**" mert lehet, hogy több eszköz mögött hello azonos IP-címet, amikor csak lehet, hogy hello gyanús tevékenység felelős. 


 
## <a name="next-steps"></a>Következő lépések

Kockázati események védelméhez az Azure AD-identitások hello foundation rendszer. Az Azure AD jelenleg képes észlelni hat kockázati események: 


| Kockázati esemény típusa | Kockázati szint | Észlelési típusa |
| :-- | --- | --- |
| [Kiszivárgott hitelesítő adatokkal rendelkező felhasználók](#leaked-credentials) | Magas | Kapcsolat nélküli módban |
| [Névtelen IP-címekről bejelentkezések](#sign-ins-from-anonymous-ip-addresses) | Közepes | Valós idejű |
| [Lehetetlen odautazás tooatypical helyek](#impossible-travel-to-atypical-locations) | Közepes | Kapcsolat nélküli módban |
| [Bejelentkezések ismeretlen helyekről](#sign-in-from-unfamiliar-locations) | Közepes | Valós idejű |
| [Fertőzött eszközökről bejelentkezések](#sign-ins-from-infected-devices) | Alacsony | Kapcsolat nélküli módban |
| [Bejelentkezések gyanús tevékenységeket IP-címekről](#sign-ins-from-ip-addresses-with-suspicious-activity) | Közepes | Kapcsolat nélküli módban|

Ha megtalálja az hello környezetében észlelt kockázati események?
Nincsenek két helyen, ahol megtekintheti a jelentett kockázat események:

 - **Az Azure AD jelentéskészítési** -kockázati események részei az Azure AD biztonsági jelentések. További részletekért lásd: hello [kockázat biztonsági jelentés felhasználók](active-directory-reporting-security-user-at-risk.md) és hello [kockázatos bejelentkezések biztonsági jelentés](active-directory-reporting-security-risky-sign-ins.md).

 - **Az Azure AD Identity Protection** -kockázati események olyan is részei [Azure Active Directory Identity Protection által](active-directory-identityprotection.md) jelentéskészítési képességek.
    

Amíg hello észlelési kockázati események már a identitások védelmének fontos eleme jelöli, akkor is hello beállítás tooeither manuálisan cím őket, vagy még akkor is megvalósíthatja automatikus válaszok feltételes hozzáférési szabályzatok konfigurálása. További részletekért lásd: a [Azure Active Directory Identity Protection által](active-directory-identityprotection.md).
 
