---
title: "Active Directory Identity Protection-szószedet aaaAzure |} Microsoft Docs"
description: "Az Azure Active Directory-Identity Protection-szószedet"
services: active-directory
keywords: "az Azure active directory azonosító adatok védelmét, a cloud app discovery, alkalmazások, biztonság, kockázati, kockázati szint, biztonsági rést, biztonsági házirend, szószedet kezelése"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 833119a5-33d6-4482-adda-fa35218c72c3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: ff2e96d20e2a3f1df24b78e66be5a0c6807e60a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection-glossary"></a>Az Azure Active Directory-Identity Protection-szószedet
### <a name="at-risk-user"></a>Fennáll a veszélye (felhasználó)
A felhasználó egy vagy több aktív kockázati események. 

### <a name="atypical-sign-in-location"></a>Bejelentkezés szokatlan bejelentkezési helye
A bejelentkezés, amely nincs a hello konkrét felhasználó, hasonló felhasználók vagy hello bérlői jellemző földrajzi helyről.

### <a name="azure-ad-identity-protection"></a>Azure AD Identity Protection
A biztonsági modul az Azure Active Directoryban, amely a kockázati eseményekről és egy szervezet identitásait érintő lehetséges biztonsági rések egyesített nézetét biztosítja.

### <a name="conditional-access"></a>Feltételes hozzáférés
A biztonságos hozzáférés tooresources egy házirendet. Feltételes hozzáférési szabályai hello Azure Active Directoryban tárolja, és az Azure AD hozzáférési toohello erőforrás megelőzően értékeli ki.  Például a szabályok a következők történő hozzáférés a felhasználó helye alapján eszköz állapotát, vagy a felhasználó a hitelesítési módszert.

### <a name="credentials"></a>Hitelesítő adatok
Azonosító és az azonosítót a használt toogain toolocal és a hálózati erőforrások eléréséhez igazolása tartalmaz információt. A hitelesítő adatok többek között a felhasználónevek és jelszavak, az intelligens kártyák és a tanúsítványok.

### <a name="event"></a>Esemény
Az Azure Active Directoryban tevékenység egy olyan rekordot.

### <a name="false-positive-risk-event"></a>A vakriasztások (kockázat esemény)
A kockázat eseményállapot manuálisan Identity Protection felhasználó, jelezve, hogy hello kockázat esemény vizsgált lett, és a kockázati események megjelölt helytelenül lett beállítva.

### <a name="identity"></a>Identitás
Személy vagy vállalat hitelesítést, például a jelszóval vagy tanúsítvánnyal feltétel alapján történő ellenőrizni kell.

### <a name="identity-risk-event"></a>Identitás kockázat esemény
Az AAD esemény Identity Protection által rendellenes tevékenységként lett megjelölve, és arra utalhat, hogy identitás feltörték.

### <a name="ignored-risk-event"></a>Figyelmen kívül hagyva (kockázat esemény)
A kockázat eseményállapot Identity Protection felhasználó, jelezve, hogy hello kockázat esemény le van zárva, a szervizelési műveletek nélkül manuálisan állítsa be.

### <a name="impossible-travel-from-atypical-locations"></a>Lehetetlen odautazás bejelentkezés szokatlan helyekről
A kockázat hello ugyanahhoz a felhasználóhoz észlelt, amikor legalább az egyik van helyről a rendellenes bejelentkezési, és ha hello közötti hello bejelentkezések ideje rövidebb, mint a minimális hello idő két bejelentkezést időt vesz igénybe toophysically által elindított esemény haladnak, ezek között helyek.  

### <a name="investigation"></a>Vizsgálat
hello hello tevékenységek, a naplókat, és olyan releváns információkat véleményezési eljárás kapcsolódó tooa kockázat esemény toodecide szervizelési vagy enyhítési lépések szükségesek, hogy, megértéséhez Ha, és hogyan hello identitás biztonsági szempontból sérült, és megismerje hogyan hello sérült biztonságú azonosító lett megadva.

### <a name="leaked-credentials"></a>Kiszivárgott hitelesítő adatok
A kockázat aktuális felhasználói hitelesítő adatok (felhasználónév és jelszó) találhatók által elindított esemény nyilvánosan hello sötét webes a kutatói által közzétett.

### <a name="mitigation"></a>Kezelés
Egy művelet toolimit vagy hello azon képessége, egy támadó tooexploit egy sérült biztonságú identitás vagy az eszköz kiküszöbölése hello identitás vagy az eszköz tooa biztonságos állapot visszaállítása nélkül. A megoldás nem oldja meg a társított hello identitás vagy az eszköz korábbi kockázati eseményekről.

### <a name="multi-factor-authentication"></a>Multi-Factor Authentication
A hitelesítési módszert, amelyhez két vagy több hitelesítési módszert, amelyek magukban foglalhatják valami hello felhasználó rendelkezik-e, ez a tanúsítvány; valami hello felhasználó ismer, például felhasználóneveket, jelszavakat vagy fázis kifejezések; fizikai attribútumait, például egy ujjlenyomat; és személyes attribútumait, például a személyes aláírás.

### <a name="offline-detection"></a>Kapcsolat nélküli észlelése
hello észlelési rendellenességek észlelését és az értékelés hello kockázatát az esemény például a bejelentkezési kísérlet után hello ténynek megfelelő esemény, amely már elvégzett.

### <a name="policy-condition"></a>Házirend feltétel
A biztonsági házirend hello entitások (csoportok, felhasználók, alkalmazások, eszközök, eszközök állapotai, IP-címtartományok, ügyféltípusokat) hello házirendben, illetve tiltani szeretné a azt meghatározó része.

### <a name="policy-rule"></a>Házirend szabályai
egy biztonsági házirendet, amely ismerteti, indítsa el hello házirend, és hello házirend kiváltásakor hello műveleteit hello körülmények hello részét.

### <a name="prevention"></a>Megelőzés
A művelet tooprevent kárt toohello munkahely visszaélés keresztül identitás vagy eszköz gyanús vagy sérült toobe tudja. Egy megelőzési művelet hello eszköz vagy identitás nem biztonságos, és nem oldja meg az előző kockázati eseményekről.

### <a name="privileged-user"></a>Kiemelt (felhasználó)
Egy olyan felhasználó, a kockázat esemény hello idején volt állandó vagy ideiglenes rendszergazdai engedélyek tooone vagy további erőforrás az Azure Active Directoryban, például egy globális rendszergazda számlázási rendszergazda, a szolgáltatás-rendszergazdát, a felhasználó rendszergazda és a jelszó Rendszergazda. 

### <a name="real-time"></a>Valós idejű
Tekintse meg a valós idejű észlelése.

### <a name="real-time-detection"></a>Valós idejű észlelése
hello tegye a rendellenességek észlelését és az esemény például a bejelentkezési kísérlet hello esemény előtt hello kockázatbecslés tooproceed engedélyezett.

### <a name="remediated-risk-event"></a>Kijavítva (kockázat esemény)
Azonosító adatok védelmét, jelezve, hogy hello kockázat esemény lett kijavítva hello szabványos szervizelési művelet használata az ilyen típusú kockázat esemény által automatikusan beállított kockázati esemény állapota. Például az hello felhasználó jelszavát, amikor sok kockázati események, amelyek jelzik, hogy hello korábbi jelszó biztonsági szempontból sérült rendszer automatikusan javítja.

### <a name="remediation"></a>Szervizkiszolgáló
Egy művelet toosecure identitás vagy egy eszköz, amely korábban gyanús vagy toobe ismert biztonsági sérülés esetén. A javítási művelet visszaállítja a hello identitás vagy az eszköz tooa biztonságos állapotát, és oldja fel az előző kockázati események társított hello identitás vagy az eszköz.

### <a name="resolved-risk-event"></a>Megoldott (kockázat esemény)
Állítsa be manuálisan egy Identity Protection felhasználónak, jelezve, hogy hello felhasználó elvégez egy megfelelő szervizelési művelet kívül Identity Protection, és adott hello kockázat eseményt kell kezelni a kockázat eseményállapot zárva.

### <a name="risk-event-status"></a>Kockázati eseményállapot
A kockázati események tulajdonságának, amely jelzi, hogy aktív-e hello esemény, és ha bezárul, bezárása, hello okát.

### <a name="risk-event-type"></a>Kockázati esemény típusa
Hello kategóriáját eseményt, hello esemény toobe tekinthető kockázatos okozó anomáliadetektálási hello típusú kockázatát.

### <a name="risk-level-risk-event"></a>Kockázati szint (kockázat esemény)
Hello kockázat esemény toohelp Identity Protection felhasználók hello súlyossága megjelölése (magas, közepes vagy alacsony) rangsorolhatja hello műveletek tooreduce hello kockázati tootheir szervezet tartanak. 

### <a name="risk-level-sign-in"></a>Kockázati szint (bejelentkezés)
Arra utal, hogy (magas, közepes vagy alacsony) hello valószínűségét, hogy az egy adott bejelentkezés, valaki más próbál toouse hello felhasználói azonosító.

### <a name="risk-level-user-compromise"></a>Kockázati szint (felhasználói sérült biztonság esetén)
Arra utal, hogy (magas, közepes vagy alacsony) hello valószínűségét, hogy identitás megtámadták.

### <a name="risk-level-vulnerability"></a>Kockázati szint (biztonsági rés)
Hello biztonsági rés toohelp Identity Protection felhasználók hello súlyossága megjelölése (magas, közepes vagy alacsony) rangsorolhatja hello műveletek tooreduce hello kockázati tootheir szervezet tartanak.

### <a name="secure-identity"></a>Biztonságos (identitás)
Például a jelszó módosítása vagy a gép, különösen a potenciálisan veszélyeztetett identitás biztonsági szempontból sértetlen tooan állapot toorestore szervizelési művelet végrehajtása.

### <a name="security-policy"></a>Biztonsági házirend
Szabályok és az állapot gyűjteménye. Egy házirend lehet például a felhasználók, csoportok, alkalmazások, eszközök, eszközök, eszköz állapotok, IP-címtartományok és Auth2.0 ügyféltípusokat alkalmazott tooentities. A házirend engedélyezve van, amikor egy entitás hello házirendben erőforrás jogkivonatot ad ki lesznek kiértékelve.

### <a name="sign-in-v"></a>Jelentkezzen be (v)
tooauthenticate tooan identitás, az Azure Active Directoryban.

### <a name="sign-in-n"></a>Bejelentkezés (n)
hello folyamat vagy egy Azure Active Directory és a hello esemény, amely a művelet rögzíti a személyazonossága hitelesítésének művelettel.

### <a name="sign-in-from-anonymous-ip-address"></a>Bejelentkezés a névtelen IP-cím
A kockázat esemény után egy sikeres bejelentkezés névtelen proxy IP-címként azonosított IP-címről következik be.

### <a name="sign-in-from-infected-device"></a>Bejelentkezés a fertőzött eszköz
A kockázat a bejelentkezés más néven toobe egy vagy több feltört eszközök, amelyek aktívan próbált botot kiszolgálóval toocommunicate által használt IP-cím származik által elindított esemény.

### <a name="sign-in-from-ip-address-with-suspicious-activity"></a>Jelentkezzen be a következő IP-gyanús tevékenység
A kockázat az esemény akkor váltódik ki, miután egy sikeres bejelentkezés IP cím nagyszámú sikertelen bejelentkezési kísérlet között több felhasználói fiókot egy rövid időtartamra vonatkozóan.

### <a name="sign-in-from-unfamiliar-location"></a>Bejelentkezés ismeretlen helyről
A kockázat felhasználó sikeresen jelentkezik be egy új helyről (IP, szélesség/hosszúsági és ASN) által elindított esemény.

### <a name="sign-in-risk"></a>Bejelentkezési kockázata
Tekintse meg a kockázati szintjét (bejelentkezés)

### <a name="sign-in-risk-policy"></a>Bejelentkezési kockázat házirendnek
Feltételes hozzáférési szabályzatot, amely hello kockázati tooa adott bejelentkezés és azok mérséklési előre meghatározott feltételt és a szabályok alapján alkalmazza.

### <a name="user-compromise-risk"></a>Felhasználó biztonsági sérülés kockázata
Tekintse meg a kockázati szintjét (felhasználói sérült biztonság esetén)

### <a name="user-risk"></a>Felhasználói kockázata
Tekintse meg a kockázati szintjét (felhasználói sérült biztonság esetén).

### <a name="user-risk-policy"></a>Felhasználói kockázat házirendnek
Feltételes hozzáférési szabályzatot, amely hello bejelentkezés úgy ítéli meg, és azok mérséklési előre meghatározott feltételt és a szabályok alapján alkalmazza.

### <a name="users-flagged-for-risk"></a>Kockázatosként megjelölt felhasználók
Kockázati eseményekről, amelyek aktív vagy szervizelt rendelkező felhasználók

### <a name="vulnerability"></a>A biztonsági rés
Egy konfigurációs vagy az Azure Active Directoryban, így hello directory ki vannak téve tooexploits vagy fenyegetések feltétel.

## <a name="see-also"></a>Lásd még:
* [Az Azure Active Directory azonosító adatok védelmét](active-directory-identityprotection.md)

