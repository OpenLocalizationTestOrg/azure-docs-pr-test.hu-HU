---
title: "Hitelesítés és az Azure MFA kiszolgáló aaaRADIUS |} Microsoft Docs"
description: "Ez a hello Azure multi-factor authentication oldal, amely segíti a RADIUS-hitelesítés és az Azure multi-factor Authentication kiszolgáló üzembe helyezése."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: f4ba0fb2-2be9-477e-9bea-04c7340c8bce
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/26/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: H1Hack27Feb2017, it-pro
ms.openlocfilehash: dac061b83f2657c67192a7aa9c5de63ffeffaaa8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-radius-authentication-with-azure-multi-factor-authentication-server"></a>RADIUS-hitelesítés integrálása az Azure Multi-Factor Authentication-kiszolgálóval
Azure MFA kiszolgáló tooenable szakasza hello RADIUS-hitelesítés használatára, és konfigurálja a RADIUS-hitelesítés. RADIUS, egy szabványos protokoll tooaccept hitelesítési kérelmek és tooprocess ezeket a kérelmeket. hello Azure multi-factor Authentication kiszolgáló RADIUS-kiszolgálóként működik. Szúrja be a RADIUS-ügyfél (VPN-készülék) és a hitelesítési célpont, legyen az Active Directory (AD), az LDAP-címtárhoz vagy egy másik RADIUS kiszolgáló tooadd Azure multi-factor Authentication között. Azure multi-factor Authentication (MFA) toofunction konfigurálnia kell hello Azure MFA kiszolgáló így képes legyen kommunikálni mind a hello ügyfélkiszolgálókkal, mind a hello hitelesítési célpont. hello Azure MFA kiszolgáló fogadja a RADIUS-ügyfelek kérelmeit, ellenőrzi a célon hello hitelesítés hitelesítő adatait, Azure multi-factor Authentication ad, és elküldi egy válasz hátsó toohello RADIUS-ügyfél. hello hitelesítési kérelem csak akkor sikeres, ha hello elsődleges hitelesítési és hello Azure multi-factor Authentication hitelesítés is sikeres.

> [!NOTE]
> hello MFA kiszolgáló a PAP (jelszó-hitelesítési protokoll) és MSCHAPv2 csak támogatja (a Microsoft kérdés-kézfogás típusú hitelesítési protokoll) RADIUS protokollal, ha a RADIUS-kiszolgálóként.  Más protokoll, EAP (extensible authentication protocol), például is használható, ha hello multi-factor Authentication kiszolgáló RADIUS proxyként tooanother RADIUS-kiszolgálóként, amely támogatja az adott protokollhoz működik.
>
> Ebben a konfigurációban egyirányú SMS és az OATH jogkivonatok óta hello multi-factor Authentication kiszolgáló nem indítható el egy sikeres RADIUS kérdés-válasz alternatív protokollok használatával nem használhatók.

![Radius-hitelesítés](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## <a name="add-a-radius-client"></a>RADIUS-ügyfél hozzáadása
tooconfigure RADIUS-hitelesítés, a telepítés hello Azure multi-factor Authentication kiszolgáló a Windows server. Ha egy Active Directory-környezetbe, hello server illesztett toohello tartomány hello hálózaton belül kell lennie. A következő eljárás tooconfigure hello Azure multi-factor Authentication kiszolgáló hello használata:

1. Hello Azure multi-factor Authentication kiszolgáló kattintson hello RADIUS-hitelesítés ikonra a bal oldali menü hello.
2. Ellenőrizze a hello **RADIUS-hitelesítés engedélyezése** jelölőnégyzetet.
3. Hello ügyfelek lapon módosítsa a hello hitelesítési és nyilvántartási portokat lehetőséget, ha hello Azure MFA RADIUS szolgáltatás toolisten kell a RADIUS-kéréseket nem szabványos porton.
4. Kattintson az **Add** (Hozzáadás) parancsra.
5. Adja meg a hello IP-cím hello készülék/Server toohello Azure multi-factor Authentication kiszolgáló, az alkalmazás neve (nem kötelező) és egy közös titkot hitelesíti magát.

  hello alkalmazásnév Azure multi-factor Authentication-jelentésekben jelenik meg, és előfordulhat, hogy SMS vagy mobilalkalmazás hitelesítési üzenet jelenjen meg.

  hello megosztott titkos igények toobe hello, azonos az mindkét hello Azure multi-factor Authentication kiszolgáló és a készülék/kiszolgáló.

6. Ellenőrizze a hello **multi-factor Authentication felhasználói egyeztetés megkövetelése** mezőben, ha az összes felhasználót törölték, vagy importálja a hello kiszolgáló és a tulajdonos toomulti kétfaktoros hitelesítést. Ha a felhasználók jelentős számú nem még importálva lettek hello kiszolgálón és/vagy a kétlépéses ellenőrzés alól, hagyja bejelölve hello mezőt.
7. Ellenőrizze a hello **engedélyezése tartalék OATH token** mezőben, ha szeretné, hogy toouse OATH PIN-kódok mobileszközös ellenőrzés alkalmazásokból, a tartalék toohello sávon kívüli telefonhívás, SMS- vagy leküldéses értesítési.
8. Kattintson az **OK** gombra.

Ismételje meg a 4 – 8 tooadd annyi további RADIUS-ügyfelek csak szeretne.

## <a name="configure-your-radius-client"></a>RADIUS-ügyfél konfigurálása

1. Kattintson a hello **cél** fülre.
2. Ha hello Azure MFA kiszolgáló telepítve van egy tartományhoz csatlakoztatott kiszolgálóra végzi az Active Directory környezetben, válassza ki a Windows-tartományhoz.
3. Ha a felhasználókat egy LDAP-címtár alapján kell hitelesíteni, válassza az **LDAP-kötést**.

  toouse LDAP-kötés, hello címtár-integráció ikonra, és hello-beállítások lapon hello LDAP-konfiguráció szerkesztése, így hello Server köthető tooyour directory. LDAP konfigurálására vonatkozó útmutatásokat hello található [LDAP-Proxy konfigurációs útmutató](multi-factor-authentication-get-started-server-ldap.md).

4. Ha a felhasználókat egy másik RADIUS-kiszolgálón kell hitelesíteni, válassza a RADIUS-kiszolgáló(k) lehetőséget.
5. Kattintson a **Hozzáadás** tooconfigure hello server toowhich hello Azure MFA kiszolgáló lesz proxy hello RADIUS-kérelmeket.
6. A hello RADIUS-kiszolgáló hozzáadása párbeszédpanelen adja meg a hello IP-címe hello RADIUS-kiszolgáló és egy közös titkot.

  hello megosztott titkos igények toobe hello, ugyanazokat a mindkét hello Azure multi-factor Authentication kiszolgáló és a RADIUS-kiszolgáló. Ha különböző portok hello RADIUS-kiszolgáló által használt, módosítsa a hello hitelesítési és nyilvántartási portot.

7. Kattintson az **OK** gombra.
8. Adja hozzá hello Azure MFA kiszolgáló RADIUS-ügyfél a hello más RADIUS-kiszolgáló így tooit hello Azure MFA kiszolgáló által küldött kérések tud feldolgozni. Ugyanazt a közös titkos kulcs hello Azure multi-factor Authentication kiszolgálón konfigurált hello használata.

Ismételje meg ezeket a lépéseket tooadd több RADIUS-kiszolgálók, és konfigurálja a mely hello Azure MFA kiszolgáló meg kell hívnia az azokat a hello hello sorrendben **fel** és **le** gombokat.

Ezzel befejezte a hello Azure multi-factor Authentication kiszolgáló konfigurációja. hello kiszolgáló most a RADIUS-hozzáférési kérelmeket hello konfigurált ügyfelek figyeli hello konfigurált portokon.   

## <a name="radius-client-configuration"></a>A RADIUS-ügyfél konfigurálása
tooconfigure hello RADIUS-ügyfél, használja a hello irányelveket:

* Konfigurálja a készülék/kiszolgáló tooauthenticate keresztül RADIUS toohello Azure multi-factor Authentication kiszolgáló IP-cím, más hello RADIUS-kiszolgálóként fog működni.
* Ugyanazt a közös titkos kulcsot, korábban konfigurált hello használata.
* Hello RADIUS időtúllépés too30-60 másodperc konfigurálja, hogy ne legyen idő toovalidate hello felhasználói hitelesítő adatok, hajtsa végre a kétlépéses ellenőrzést, a választ kapnak és majd válaszolnia toohello RADIUS hozzáférési kérelem.
