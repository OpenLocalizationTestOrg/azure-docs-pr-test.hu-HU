---
title: "ajánlott eljárások aaaSecurity MFA |} Microsoft Docs"
description: "Ez a dokumentum gyakorlati tanácsokat körül az Azure MFA használata az Azure-fiókra"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 3be7d968-96bb-4320-8701-869fd04a2595
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 7f18c2592764878b842d81783b321a05f29ee3d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="security-best-practices-for-using-azure-multi-factor-authentication-with-azure-ad-accounts"></a>Ajánlott biztonsági eljárások az Azure multi-factor Authentication használata az Azure AD-fiókok

Kétlépéses ellenőrzés az előnyben részesített hello választás a legtöbb szervezet számára, így tooenhance a hitelesítési folyamat. Az Azure multi-factor Authentication (MFA) megkönnyíti a vállalatok számára a biztonsági és megfelelőségi követelményeknek ugyanakkor biztosítható egy egyszerű bejelentkezési élmény a felhasználók számára. Ez a cikk az Azure MFA hello elfogadását tervezése során érdemes tippek ismerteti.

## <a name="deploy-azure-mfa-in-hello-cloud"></a>Az Azure MFA felhőben hello telepítése

Nincsenek két módon tooenable Azure MFA a felhasználók számára.

* Az egyes felhasználók (vagy az Azure MFA, Azure AD Premium vagy Enterprise Mobility + Security) licencek vásárlása
* A többtényezős hitelesítésszolgáltató és a fizetési felhasználói vagy a hitelesítés létrehozása

### <a name="licenses"></a>Licencek
![AZ EMS](./media/multi-factor-authentication-security-best-practices/ems.png)

Prémium szintű Azure AD vagy nagyvállalati mobilitási + biztonsági licenccel rendelkezik, ha már rendelkezik Azure MFA. A szervezet nem kell semmi további tooextend hello kétlépéses ellenőrzés funkció tooall felhasználók. Csak akkor kell egy licencet tooa felhasználói tooassign, és majd többtényezős hitelesítés bekapcsolása.

Többtényezős hitelesítés beállítása, vegye figyelembe a következő tippek hello:

* Ne hozzon létre egy hitelesítés többtényezős hitelesítésszolgáltató. Ha így tesz, akkor sikerült végül licenccel már rendelkező felhasználóktól a hitelesítési kérelmek fizet.
* Ha a felhasználók számára nem elegendő licenccel rendelkezik, létrehozhat egy felhasználói többtényezős hitelesítésszolgáltató toocover hello a szervezet többi része. 
* Az Azure AD Connect csak akkor szükséges, ha a helyszíni Active Directory-környezet az Azure AD-címtár szinkronizál. Ha az Azure AD-címtárhoz, amely nincs szinkronizálva az Active Directory helyszíni példányát használja, nem kell az Azure AD Connect.

### <a name="multi-factor-auth-provider"></a>Többtényezős hitelesítésszolgáltató
![Többtényezős hitelesítésszolgáltató](./media/multi-factor-authentication-security-best-practices/authprovider.png)

Ha még nem rendelkezik licenccel, amely tartalmazza az Azure MFA, majd létrehozhat egy Többtényezős hitelesítési szolgáltató. 

Hello hitelesítésszolgáltató létrehozásakor meg kell tooselect egy könyvtárat, és vegye figyelembe a következő adatok hello:

* Nem kell egy Azure Active directory toocreate a többtényezős hitelesítésszolgáltató, de egy további funkciókat elérhetővé. a következő funkciók hello engedélyezve vannak, ha a hitelesítésszolgáltató hello társítása Azure AD-címtár:  
  * A felhasználók kiterjesztése a kétlépéses ellenőrzés tooall  
  * A globális rendszergazdák további szolgáltatásokat, például hello felügyeleti portál, az egyéni hónap és a jelentések kínálnak.
* Ha Azure AD-címtárral szinkronizálja a helyszíni Active Directory környezetben, akkor a DirSync és AAD Sync. Ha az Azure AD-címtárhoz, amely nincs szinkronizálva az Active Directory helyszíni példányát használja, nem kell a DirSync és AAD Sync.
* Válassza ki, amely a legjobban megfelel az üzleti hello fogyasztás modelljét. Hello használati modell kiválasztása után nem módosítható. hello két modellek a következők:
  * Hitelesítésenként: elszámolás meg minden egyes ellenőrzésre. Ezt a modellt használja, ha azt szeretné, hogy a kétlépéses ellenőrzést, bárki, aki hozzáfér egy bizonyos alkalmazás, bizonyos felhasználók részére nem.
  * Engedélyezett felhasználónként: minden olyan felhasználóhoz, az Azure MFA számára engedélyezi a díja meg. Ezt a modellt használja, ha egyes felhasználók az Azure AD Premium vagy a nagyvállalati mobilitási csomag licenceket, és néhány nélkül.

### <a name="supportability"></a>Támogatási lehetőségek
Mivel a legtöbb felhasználó csak jelszavak tooauthenticate kezelői toousing, fontos, hogy a vállalat felhívja a tooall felhasználókkal szemben ezt a folyamatot. A tájékoztatási hello valószínűsége, hogy felhasználók hívja a segélyszolgálatot a kisebb problémák kapcsolódó tooMFA csökkentheti. Van azonban néhány olyan forgatókönyvet, ahol ideiglenesen letilthatja az MFA szükség. A következő irányelveket toounderstand hogyan használja hello toohandle azokra:

* A technikai támogatási szolgálathoz személyzet toohandle forgatókönyvek, ahol hello felhasználói nem tud bejelentkezni, mert egy értesítést vagy a telefonhívás nem fogad hello mobilalkalmazás vagy a telefon betanításához. A technikai támogatási szolgálathoz is [engedélyezése az egyszeri Mellőzés](multi-factor-authentication-whats-next.md#one-time-bypass) tooallow egy felhasználó tooauthenticate "kihagyásával" kétlépéses ellenőrzés egy alkalommal. hello Mellőzés átmeneti, és a megadott számú másodperc után lejár.
* Vegye figyelembe a hello [megbízható IP-címek funkció](multi-factor-authentication-whats-next.md#trusted-ips) az Azure MFA olyan módon toominimize kétlépéses ellenőrzést. Ezzel a szolgáltatással kezelt vagy összevont bérlő rendszergazdák jogosultak a kétlépéses ellenőrzést, a felhasználók számára, amely a helyi intranet hello vállalati jelentkezik be. Azure AD Premium, a nagyvállalati mobilitási csomag vagy az Azure multi-factor Authentication licenccel rendelkezik Azure AD-bérlő hello funkciók érhetők el.

## <a name="best-practices-for-an-on-premises-deployment"></a>Egy a helyszíni központi telepítésének ajánlott eljárásai
Ha a vállalat úgy döntött, tooleverage saját infrastruktúra tooenable többtényezős Hitelesítést, akkor szüksége toodeploy helyszíni az Azure multi-factor Authentication kiszolgáló. a következő diagram hello hello MFA kiszolgáló-összetevői láthatók:

![Multi-factor Authentication kiszolgáló-összetevők alapértelmezett: konzol, a szinkronizálási motor, a felügyeleti portálon, a felhőalapú szolgáltatás](./media/multi-factor-authentication-security-best-practices/server.png) \*alapértelmezés szerint nincs telepítve \** telepített, de alapértelmezés szerint nincs engedélyezve

Az Azure multi-factor Authentication kiszolgáló biztonságossá teheti a felhőalapú erőforrások és a helyszíni erőforrások összevonási használatával. Kell rendelkeznie az AD FS és azt az Azure AD-bérlő összevont.
A multi-factor Authentication kiszolgáló beállításakor vegye figyelembe a következő adatok hello:

* Ha biztosítása, hogy az Azure AD-erőforrások Active Directory összevonási szolgáltatások (AD FS), majd hello első ellenőrzési lépés történik a helyszíni Active Directory összevonási szolgáltatások használatával. hello második lépésben szerint hello jogcím érvényesítenie a helyszínen történik.
* Nincs tooinstall hello Azure multi-factor Authentication kiszolgáló az AD FS összevonási kiszolgáló. Azonban az AD FS többtényezős hitelesítés Adapter hello telepíteni kell a Windows Server 2012 R2 AD FS-t futtató. Hello kiszolgáló egy másik számítógépre telepítse, mindaddig, amíg támogatott verziója, és hello AD FS-adaptert külön-külön telepíteni az AD FS összevonási kiszolgálón. 
* hello multi-factor Authentication AD FS-Adapter telepítése varázsló PhoneFactor-Adminisztrátorok meghívta az Active Directory biztonsági csoportot hoz létre, és hozzáadja a az AD FS szolgáltatási fiók toothis csoport. Ellenőrizze, hogy hello PhoneFactor-Adminisztrátorok csoport lett létrehozva, a tartományvezérlő, és adott hello AD FS-szolgáltatásfiók tagja ennek a csoportnak. Ha szükséges, a tartományvezérlő hozzáadása manuálisan hello AD FS szolgáltatás fiók toohello PhoneFactor-Adminisztrátorok csoporthoz.

### <a name="user-portal"></a>Felhasználói portál
hello felhasználói portál lehetővé teszi, hogy a önkiszolgálói képességeit, és a teljes felhasználói felügyeleti képességeket biztosít. Az Internet Information Server (IIS) webhely fusson. A következő irányelveket tooconfigure hello ezt az összetevőt használja:

* Használja az IIS 6 vagy újabb
* Telepítse és regisztrálja az ASP.NET v2.0.507207
* Győződjön meg arról, hogy a kiszolgáló telepíthető a szegélyhálózatban

### <a name="app-passwords"></a>Alkalmazásjelszók
Ha a szervezet az egyszeri bejelentkezés az Azure AD össze van vonva, és az Azure MFA használatával toobe kívánja, majd vegye figyelembe a következő adatok hello:

* hello alkalmazásjelszót ellenőrizte-e az Azure AD, és ezért nincs hatással az összevonási. Összevonási csak akkor használatos, alkalmazásjelszók beállítása során.
* Az összevont felhasználókra (SSO) jelszavak hello szervezeti azonosító tárolja. Ha hello felhasználó elhagyja a hello vállalati, adott információ kapcsolatazonosítója tooflow tooorganizational DirSync-kel. Fiók letiltása/törlése toothree óra toosync, ami késlelteti az alkalmazásjelszók letiltása/törlése az Azure AD is tarthat.
* Az alkalmazásjelszó nem tartja be a helyszíni ügyfél hozzáférés-vezérlési beállításait.
* Alkalmazásjelszók nem helyszíni hitelesítési naplózási/naplózási képesség érhető el.
* Bizonyos speciális architekturális tervek előfordulhat, hogy a szervezeti felhasználónévvel és a jelszavak és a alkalmazásjelszókat, ha segítségével kétlépéses hitelesítéssel rendelkező ügyfelek, attól függően, hogy hol hitelesítéshez. A hitelesítést, a helyszíni infrastruktúra-ügyfelek esetében használja a szervezeti felhasználónévvel és jelszóval. A hitelesítést, az Azure AD-ügyfelek esetében használja a hello alkalmazásjelszót.
* Alapértelmezés szerint a felhasználók nem hozhatják létre alkalmazásjelszókat. Ha a felhasználók toocreate alkalmazásjelszók tooallow van szüksége, válassza ki a hello **engedélyezése a felhasználók toocreate app jelszavak toosign nem böngészőben megjelenő alkalmazásokba** lehetőséget.

## <a name="additional-considerations"></a>További szempontok
További szempontokat használja ezt a listát, és útmutatást az egyes összetevők, amely a helyszíni telepített:

- Az Azure Multi-Factor Authentication telepítése az [Active Directory összevonási szolgáltatással](multi-factor-authentication-get-started-adfs.md).
- Beállítása és konfigurálása az Azure MFA kiszolgáló hello [RADIUS-hitelesítés](multi-factor-authentication-get-started-server-radius.md).
- Beállítása és konfigurálása az Azure MFA kiszolgáló hello [IIS-hitelesítés](multi-factor-authentication-get-started-server-iis.md).
- Beállítása és konfigurálása az Azure MFA kiszolgáló hello [Windows-hitelesítés](multi-factor-authentication-get-started-server-windows.md).
- Beállítása és konfigurálása az Azure MFA kiszolgáló hello [LDAP-hitelesítés](multi-factor-authentication-get-started-server-ldap.md).
- Beállítása és konfigurálása az Azure MFA kiszolgáló hello [távoli asztali átjáró és az Azure multi-factor Authentication kiszolgáló RADIUS használata](multi-factor-authentication-get-started-server-rdg.md).
- Állítson be és a szinkronizálás konfigurálása hello Azure MFA kiszolgáló között, és [Windows Server Active Directory](multi-factor-authentication-get-started-server-dirint.md).
- [Hello Azure multi-factor Authentication kiszolgáló mobilalkalmazás webszolgáltatásának telepítése](multi-factor-authentication-get-started-server-webservice.md).
- [VPN-konfiguráció Azure multi-factor Authentication speciális](multi-factor-authentication-advanced-vpn-configurations.md) Cisco ASA, a Citrix Netscaler és a Juniper/Pulse Secure VPN készülékek LDAP vagy a RADIUS használatával.

## <a name="next-steps"></a>Következő lépések
Ez a cikk az Azure MFA emel ki, néhány ajánlott eljárás, amíg nincsenek más erőforrások, amelyek az MFA telepítésének tervezése során is használhatók. hello listája az alábbi tartalmaz néhány főbb cikkeket, amelyek segítséget nyújthatnak a folyamat során:

* [Azure multi-factor Authentication jelentései](multi-factor-authentication-manage-reports.md)
* [hello kétlépéses ellenőrzés regisztrációs élmény](multi-factor-authentication-end-user-first-time.md)
* [Az Azure többtényezős hitelesítés – gyakori kérdések](multi-factor-authentication-faq.md)

