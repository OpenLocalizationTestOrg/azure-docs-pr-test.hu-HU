---
title: "aaaGetting lépések az Azure multi-factor Authentication kiszolgáló |} Microsoft Docs"
description: "Ez a hello Azure többtényezős hitelesítés lap, amely leírja, hogyan tooget el az Azure MFA kiszolgáló."
services: multi-factor-authentication
keywords: "hitelesítési kiszolgáló, azure multi factor authentication alkalmazásaktiválási oldal, hitelesítési kiszolgáló letöltése"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: e94120e4-ed77-44b8-84e4-1c5f7e186a6b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2017
ms.author: joflore
ms.reviewer: alexwe
ms.custom: it-pro
ms.openlocfilehash: 92a6a586eb96375e92a9455ad64e67221001db81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-hello-azure-multi-factor-authentication-server"></a>Ismerkedés az Azure multi-factor Authentication kiszolgáló hello

<center>![Helyszíni MFA](./media/multi-factor-authentication-get-started-server/server2.png)</center>

Most, hogy toouse a helyi multi-factor Authentication kiszolgáló azt észlelte, adjuk hozzá. Ezen a lapon egy új telepítés hello kiszolgáló és a helyszíni Active Directory beállítása magában foglalja. Ha már hello MFA kiszolgáló telepítve van, és tooupgrade olyan eszközökre, [toohello frissítés legújabb Azure multi-factor Authentication kiszolgáló](multi-factor-authentication-server-upgrade.md). Ha csak hello webszolgáltatás telepítésével kapcsolatos információkat keres, tekintse meg [Deploying hello Azure multi-factor Authentication kiszolgáló mobilalkalmazás webszolgáltatásának](multi-factor-authentication-get-started-server-webservice.md).

## <a name="plan-your-deployment"></a>Az üzembe helyezés megtervezése

Hello Azure multi-factor Authentication kiszolgáló letöltése előtt gondolja át a terhelés és a magas rendelkezésre állás biztosításához van. Ezen információk toodecide használja, hogyan és hol toodeploy.

Jó megoldás tooauthenticate rendszeresen várt hello memóriamennyiség szüksége van hello felhasználók száma.

| Felhasználók | RAM |
| ----- | --- |
| 1–10 000 | 4 GB |
| 10 001–50 000 | 8 GB |
| 50 001–100 000 | 12 GB |
| 100 000–200 001 | 16 GB |
| 200 001+ | 32 GB |

Meg kell tooset több kiszolgáló magas rendelkezésre állásra vagy terheléselosztás? Számos módon tooset be az Azure MFA kiszolgáló a konfigurációt. Az első Azure MFA kiszolgáló telepítésekor hello fő válik. Bármely további kiszolgálók lesz alárendelt, és a felhasználók és a konfiguráció automatikus szinkronizálás hello master. Ezután, egy elsődleges kiszolgáló konfigurálása, és a működésre hello rest rendelkezik, a biztonsági másolatból, vagy minden hello kiszolgálók közötti terheléselosztás állíthat be.

A fő Azure MFA kiszolgáló offline állapotba kerül, ha hello alárendelt kiszolgálók továbbra is kétlépéses ellenőrzés kérelmek feldolgozásához is. Azonban nem adhat hozzá új és meglévő felhasználók nem tudja frissíteni a beállításait, amíg hello fő újra online állapotba kerül, vagy egy alárendelt lekérése előléptetve.

### <a name="prepare-your-environment"></a>A környezet előkészítése

Győződjön meg arról, amelyen az Azure multi-factor Authentication kiszolgáló hello megfelel a követelményeknek hello:

| Az Azure Multi-Factor Authentication-kiszolgáló követelményei | Leírás |
|:--- |:--- |
| Hardver |<li>200 MB merevlemez-terület</li><li>x32-es vagy x64-es verzió futtatására képes processzor</li><li>Legalább 1 GB RAM</li> |
| Szoftver |<li>Windows Server 2008 vagy újabb, ha az hello állomás operációs rendszer</li><li>Windows 7-es vagy nagyobb, ha hello gazdagép egy ügyfél OS</li><li>Microsoft .NET-keretrendszer 4.0</li><li>IIS 7.0-s vagy újabb, ha telepíti a felhasználói portál vagy a web service SDK hello</li> |

### <a name="azure-mfa-server-components"></a>Az Azure MFA-kiszolgáló összetevői

Az Azure MFA-kiszolgáló három webösszetevőt tartalmaz:

* Webszolgáltatási SDK - lehetővé teszi a kommunikációt hello más összetevők és hello Azure MFA kiszolgáló telepítve van
* Felhasználói portál –, amely lehetővé teszi, hogy a felhasználók tooenroll Azure multi-factor Authentication (MFA) és a fiókok karbantartásához IIS-webhelyet.
* Mobil webszolgáltatás - lehetővé teszi, hogy a kétlépéses ellenőrzéshez például hello Microsoft Authenticator alkalmazás használatával.

Mindhárom összetevő hello telepíthető ugyanarra a kiszolgálóra, ha hello kiszolgáló internetre irányuló. Ha összeállításának hello összetevők, hello webszolgáltatási SDK telepítve van-e hello Azure MFA alkalmazáskiszolgálón, és hello felhasználói portál és a mobilalkalmazás webszolgáltatás egy internetre irányuló kiszolgálón van telepítve.

### <a name="azure-multi-factor-authentication-server-firewall-requirements"></a>Az Azure Multi-Factor Authentication-kiszolgáló tűzfalkövetelményei

Minden multi-factor Authentication kiszolgáló a 443-as port kimenő toohello a következő címeket tud toocommunicate kell lennie:

* https://pfd.phonefactor.net
* https://pfd2.phonefactor.net
* https://css.phonefactor.net

Ha kimenő tűzfalak korlátozza a 443-as porton, nyissa meg a következő IP-címtartományok hello:

| IP-alhálózat | Hálózati maszk | IP-címtartomány |
|:---: |:---: |:---: |
| 134.170.116.0/25 |255.255.255.128 |134.170.116.1 – 134.170.116.126 |
| 134.170.165.0/25 |255.255.255.128 |134.170.165.1 – 134.170.165.126 |
| 70.37.154.128/25 |255.255.255.128 |70.37.154.129 – 70.37.154.254 |

Ha a nem használt hello Eseménymegerősítési szolgáltatást, és a felhasználók a nem használt eszközökön mobilalkalmazások tooverify hello a vállalati hálózaton, csak kell hello tartomány a következő:

| IP-alhálózat | Hálózati maszk | IP-címtartomány |
|:---: |:---: |:---: |
| 134.170.116.72/29 |255.255.255.248 |134.170.116.72 – 134.170.116.79 |
| 134.170.165.72/29 |255.255.255.248 |134.170.165.72 – 134.170.165.79 |
| 70.37.154.200/29 |255.255.255.248 |70.37.154.201 – 70.37.154.206 |

## <a name="download-hello-azure-multi-factor-authentication-server"></a>Hello Azure multi-factor Authentication kiszolgáló letöltése

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) rendszergazdaként.
2. Hello bal oldalon válassza ki a **Active Directory**
3. Kattintson a **Felhasználók és csoportok** elemre
4. Kattintson a **Minden felhasználó** elemre
5. Kattintson a **Többtényezős hitelesítés** elemre
6. A **Többtényezős hitelesítés** szakaszban válassza a **Szolgáltatásbeállítások** elemet

   ![Szolgáltatásbeállítások oldal](./media/multi-factor-authentication-get-started-server/servicesettings.png)

6. A hello services beállítások oldalán üdvözlő képernyőt hello alján kattintson **Ugrás toohello portal**. Megnyílik egy új lap.
7. Kattintson a **Letöltések** elemre.
8. Kattintson a hello **letöltése** hivatkozásra, és mentse hello telepítő.

   ![Az MFA-kiszolgáló letöltése](./media/multi-factor-authentication-get-started-server/download4.png)

9. Ezen a lapon tartsa nyitva, tooit után futó hello telepítő hivatkozik.

## <a name="install-and-configure-hello-azure-multi-factor-authentication-server"></a>Telepítse és konfigurálja a hello Azure multi-factor Authentication kiszolgáló

Most, hogy a letöltött hello kiszolgáló telepítéséhez, és konfigurálásához. Győződjön meg arról, hogy telepíti azt hello kiszolgáló megfelel-e hello tervezési szakaszban felsorolt követelményeknek.

1. Kattintson duplán a hello végrehajtható.
2. A telepítési mappa kijelölése hello képernyőn ellenőrizze, hogy megfelelő-e a hello mappában, és kattintson a **következő**.
3. Hello telepítés befejeződése után kattintson **Befejezés**.  hello konfigurációs varázsló elindul.
4. Hello konfigurációs varázsló üdvözlőképernyőn jelölje **használatával Skip hello Hitelesítéskonfiguráló varázsló** kattintson **következő**.  hello varázsló bezárása után, és hello kiszolgáló indul.

   ![Felhő](./media/multi-factor-authentication-get-started-server/skip2.png)

5. Vissza a Microsoft hello kiszolgálót a letöltött hello lapon kattintson hello **aktiválási hitelesítő adatok generálása** gombra. Másolja ezt az információt hello Azure MFA kiszolgáló hello mezőkben megadott, és kattintson a **aktiválás**.

## <a name="send-users-an-email"></a>E-mail küldése a felhasználóknak

a bevezetési tooease engedélyezi az MFA kiszolgáló toocommunicate a felhasználóival. Multi-factor Authentication kiszolgáló egy e-mailek tooinform elküldheti őket, hogy azok regisztrálva vannak a kétlépéses ellenőrzéshez.

hello e-mailt küldünk hogyan konfigurálja a felhasználók a kétlépéses ellenőrzéshez segítségével határozható meg. Például ha tudja tooimport telefonszámok hello vállalati könyvtárból, hello e-mail tartalmaznia kell hello alapértelmezett telefonszámokat, hogy a felhasználók tudják, milyen tooexpect. Ha nem importálja telefonszámokat, vagy a felhasználók toouse hello mobilalkalmazás fog, küldje el az e-mailek, toocomplete vezeti őket a fiók regisztráció. Vegye fel a hivatkozás toohello Azure multi-factor Authentication felhasználói portál hello e-mailek.

hello e-mailek tartalma hello hello módszer annak ellenőrzése, hogy be van állítva (telefonhívás, SMS vagy mobilalkalmazás) hello felhasználói is függ.  Például ha hello felhasználó PIN-kód szükséges toouse hitelesítéshez, hello e-mail meghatározza, milyen a kezdeti PIN-KÓDJUKBAN van beállítva.  Felhasználók vannak a szükséges toochange PIN-kódjukat az első hitelesítés során.

### <a name="configure-email-and-email-templates"></a>E-mailek és e-mail-sablonok konfigurálása

Kattintson a hello e-mail ikonra a bal oldali tooset hello hello-beállítások ezen e-mailek küldésére. Ezen a lapon be hello SMTP-adatokat a levelezési kiszolgáló, majd küldje el e-mailek hello ellenőrzésével **küldése e-mailt küld toousers** jelölőnégyzetet.

![MFA-kiszolgáló – E-mail-konfiguráció](./media/multi-factor-authentication-get-started-server/email1.png)

Hello E-mail tartalma lapon láthatja, amelyek a rendelkezésre álló toochoose hello e-mail sablonok. Attól függően, hogy hogyan konfigurálta a felhasználók tooperform kétlépéses ellenőrzést válassza ki a leginkább megfelelő hello sablont.

![MFA-kiszolgáló – E-mail-sablonok](./media/multi-factor-authentication-get-started-server/email2.png)

## <a name="import-users-from-active-directory"></a>Felhasználók importálása az Active Directoryból

Most már telepítve van a hello kiszolgálón érdemes tooadd felhasználók. Kiválaszthatja a toocreate őket manuálisan, az Active Directoryból importál felhasználókat vagy az automatikus szinkronizálás konfigurálása az Active Directoryban.

### <a name="manual-import-from-active-directory"></a>Manuális importálás az Active Directoryból

1. Válassza ki az Azure MFA kiszolgáló hello hello bal oldali **felhasználók**.
2. Hello alján válassza **Active Directoryból való importálás**.
3. Most vagy kereshet felhasználónként vagy keresési hello Active directory szervezeti egységek felhasználóival őket.  Ebben az esetben hello felhasználók szervezeti egység adtuk meg.
4. Jelölje ki a megfelelő hello senki hello majd **importálási**.  Megjelenik egy előugró ablak, amely a művelet sikerességéről értesít.  Bezárás hello importálás ablak.

   ![MFA-kiszolgáló – Felhasználók importálása](./media/multi-factor-authentication-get-started-server/import2.png)

### <a name="automated-synchronization-with-active-directory"></a>Automatikus szinkronizálás az Active Directoryval

1. Válassza ki az Azure MFA kiszolgáló hello hello bal oldali **címtár-integráció**.
2. Keresse meg a toohello **szinkronizálási** fülre.
3. Hello alján válassza **hozzáadása**
4. A hello **szinkronizálási elemek hozzáadása** meg hello tartomány, szervezeti egység kiválasztása **vagy** biztonsági csoport, beállítások, módszer alapbeállításai és nyelve alapértelmezés szerint ez a szinkronizálás a feladatot, és kattintson a **Hozzáadása**.
5. Hello jelölőnégyzetet **Active Directory-szinkronizálás engedélyezése** , és válassza a **szinkronizálás időköze** egy perc és 24 óra között.

## <a name="how-hello-azure-multi-factor-authentication-server-handles-user-data"></a>Hogyan kezeli a hello Azure multi-factor Authentication kiszolgáló a felhasználói adatokat

Hello multi-factor Authentication (MFA) helyszíni Server használata esetén a felhasználói adatok hello a helyszíni kiszolgálók találhatók. Nem állandó felhasználói adatok hello felhő tárolja. Hello felhasználó a kétlépéses ellenőrzést hajt végre, amikor hello MFA kiszolgáló küld adatokat toohello Azure MFA felhőalapú szolgáltatás tooperform hello ellenőrzése. Ha ezeket a hitelesítési kérelmeket küld toohello felhőalapú szolgáltatás, hello következő mezők küldése a hello kérelem és a naplókat, hogy elérhetők a hello az ügyfél-hitelesítési/használati jelentésekben. Néhány hello mező nem kötelező, ezért engedélyezve van vagy le van tiltva, a multi-factor Authentication kiszolgáló hello belül. hello kommunikációt a multi-factor Authentication kiszolgáló toohello MFA felhőalapú szolgáltatás hello SSL/TLS használja a 443-as kimenő porton keresztül. Ezek a mezők a következők:

* Egyedi azonosító – felhasználónév vagy belső MFA-kiszolgálói azonosító
* Utónév és vezetéknév (nem kötelező)
* E-mail-cím (nem kötelező)
* Telefonszám – hanghívások vagy SMS-hitelesítés esetén
* Eszköztoken – mobilalkalmazásos hitelesítés esetén
* Hitelesítési módszer
* Hitelesítés eredménye
* MFA-kiszolgáló neve
* MFA-kiszolgáló IP-címe
* Ügyfél IP-címe – ha elérhető

Ezenkívül toohello fenti mezők, hello ellenőrzés eredményét (sikeres vagy megtagadását) és bármely elutasítások okát is tárolt hello hitelesítési adatokkal és hello hitelesítési/használati jelentések keresztül elérhető.

## <a name="back-up-and-restore-azure-mfa-server"></a>Az Azure MFA-kiszolgáló biztonsági mentése és visszaállítása

Annak biztosítása, hogy a helyes biztonsági másolatot a rendszer egy fontos lépés tootake.

tooback Azure MFA kiszolgáló, győződjön meg arról, hogy rendelkezik-e hello másolatát **C:\Program Files\Multi-Factor Authentication Server\Data** mappa, beleértve a hello **PhoneFactor.pfdata** fájlt. 

A visszaállítási esetben van szükséges teljes hello a következő lépéseket:

1. Telepítse újra az Azure MFA-kiszolgálót egy új kiszolgálón.
2. Aktiválása hello új Azure MFA kiszolgáló.
3. Állítsa le hello **MultiFactorAuth** szolgáltatás.
4. Hello felülírása **PhoneFactor.pfdata** a biztonsági másolat hello.
5. Indítsa el a hello **MultiFactorAuth** szolgáltatás.

hello új kiszolgáló megfelelően most működik, és a hello eredeti konfigurációs és a felhasználói adatokat.

## <a name="next-steps"></a>Következő lépések

- Beállítása és konfigurálása hello [felhasználói portál](multi-factor-authentication-get-started-portal.md) önkiszolgáló felhasználó számára.
- Beállítása és konfigurálása az Azure MFA kiszolgáló hello [Active Directory összevonási szolgáltatás](multi-factor-authentication-get-started-adfs.md), [RADIUS-hitelesítés](multi-factor-authentication-get-started-server-radius.md), vagy [LDAP-hitelesítés](multi-factor-authentication-get-started-server-ldap.md).
- [Távoli asztali átjáró és RADIUS-t használó Azure Multi-Factor Authentication-kiszolgáló](multi-factor-authentication-get-started-server-rdg.md) telepítése és konfigurálása.
- [Hello Azure multi-factor Authentication kiszolgáló mobilalkalmazás webszolgáltatásának telepítése](multi-factor-authentication-get-started-server-webservice.md).
- [Speciális, az Azure Multi-Factor Authenticationre és külső VPN-ekre vonatkozó forgatókönyvek](multi-factor-authentication-advanced-vpn-configurations.md).
