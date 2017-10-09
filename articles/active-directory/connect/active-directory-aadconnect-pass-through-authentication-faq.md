---
title: "Az Azure AD Connect: Áteresztő hitelesítés – gyakori kérdések |} Microsoft Docs"
description: "Válaszok toofrequently kérdések az Azure Active Directory átmenő hitelesítést."
services: active-directory
keywords: "Az Azure AD Connect áteresztő hitelesítés, telepítés Active Directory szükséges összetevőket az Azure AD, SSO, egyszeri bejelentkezést."
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: billmath
ms.openlocfilehash: 550e2599177682f8ea971a05485dd5ac12b9b072
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-frequently-asked-questions"></a>Az Azure Active Directory áteresztő hitelesítés: Gyakran ismételt kérdések

Ebben a cikkben oldjuk Azure Active Directory (Azure AD) áteresztő hitelesítés kapcsolatos gyakori kérdésekre. Vissza az új tartalom ellenőrizni.

>[!IMPORTANT]
>hello áteresztő hitelesítési funkció jelenleg előzetes verzió.

## <a name="which-of-hello-azure-ad-sign-in-methods---pass-through-authentication-password-hash-synchronization-and-active-directory-federation-services-ad-fs---should-i-choose"></a>Amely hello Azure AD bejelentkezési módszerek - áteresztő hitelesítés, Jelszókivonat-szinkronizálást és az Active Directory összevonási szolgáltatások (AD FS) - érdemes kiválasztása?

Ez a helyszíni környezetben és a szervezeti követelményektől függ. Tekintse át a jelen cikk egy [összehasonlítása különböző Azure AD bejelentkezési módszer hello](active-directory-aadconnect-user-signin.md).

## <a name="is-pass-through-authentication-a-free-feature"></a>Az áteresztő hitelesítés szabad szolgáltatás?

Áteresztő hitelesítés szabad funkció, minden Azure AD toouse fizetős verziója nem kell azt. A leválasztást szabad hello szolgáltatás általános elérhetőségével elérésekor.

## <a name="is-pass-through-authentication-available-in-microsoft-cloud-germanyhttpwwwmicrosoftdecloud-deutschland-and-microsoft-azure-government-cloudhttpsazuremicrosoftcomfeaturesgov"></a>A érhető áteresztő hitelesítés [Microsoft Cloud Németország](http://www.microsoft.de/cloud-deutschland) és [Microsoft Azure Government felhő](https://azure.microsoft.com/features/gov/)?

Nem, áteresztő hitelesítés csak akkor az Azure AD példányával hello világszerte.

## <a name="does-conditional-accessactive-directory-conditional-accessmd-work-with-pass-through-authentication"></a>Does [feltételes hozzáférés](../active-directory-conditional-access.md) munkahelyi átmenő hitelesítést?

Igen, az összes feltételes hozzáférés lehetőségekről, beleértve a Azure multi-factor Authentication áteresztő hitelesítés működik.

## <a name="does-pass-through-authentication-support-alternate-id-as-hello-username-instead-of-userprincipalname"></a>Támogatja áteresztő hitelesítés "A másik azonosító" helyett a "userPrincipalName" hello felhasználóneveként?

Igen. Áteresztő hitelesítés támogat `Alternate ID` , amikor konfigurálása az Azure AD Connectben, ahogy felhasználónév hello [Itt](active-directory-aadconnect-get-started-custom.md). Nem minden Office 365-alkalmazások támogatják a `Alternate ID`. Tekintse meg a toohello adott alkalmazás által dokumentációját hello támogatási utasítás.

## <a name="does-password-hash-synchronization-act-as-a-fallback-toopass-through-authentication"></a>Jelszókivonat-szinkronizálást a tartalék tooPass keresztül hitelesítés összekötőként?

Nem, Jelszókivonat-szinkronizálást nem egy általános tartalék tooPass keresztül hitelesítésre. Csak a tartalékként működik [szolgáltatásokat, amelyek jelenleg nem támogatja az áteresztő hitelesítés](active-directory-aadconnect-pass-through-authentication-current-limitations.md#unsupported-scenarios). tooavoid felhasználói sikertelen bejelentkezés, konfigurálnia kell az áteresztő hitelesítés [magas rendelkezésre állású](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability).

## <a name="can-i-install-an-azure-ad-application-proxyactive-directory-application-proxy-get-startedmd-connector-on-hello-same-server-as-a-pass-through-authentication-agent"></a>Telepíthető egy [az Azure AD-alkalmazásproxy](../active-directory-application-proxy-get-started.md) hello-összekötő ugyanarra a kiszolgálóra egy csatlakoztatott hitelesítési ügynök?

Igen, ez a konfiguráció a hello rebranded hello áteresztő hitelesítés ügynök verziója támogatott (1.5.193.0 verzió vagy újabb).

## <a name="what-versions-of-azure-ad-connect-and-pass-through-authentication-agent-do-you-need"></a>Az Azure AD Connect és az áteresztő hitelesítés ügynök mely verzióit van szüksége?

1.1.486.0 verziójára van szükség, vagy később az Azure AD Connect és 1.5.58.0 vagy később hello áteresztő hitelesítés ügynök esetében ez a szolgáltatás toowork. Minden szoftver a Windows Server 2012 R2 vagy újabb kiszolgálókon kell telepíteni.

## <a name="what-happens-if-my-users-password-has-expired-and-they-try-toosign-in-using-pass-through-authentication"></a>Mi történik, ha a felhasználó jelszava lejárt, és próbálja meg az áteresztő hitelesítés használatával toosign?

Ha már konfigurált [jelszóvisszaírás](../active-directory-passwords-update-your-own-password.md) egy adott felhasználó, és ha hello felhasználó jelentkezik be, hogy átmenő hitelesítést használ, módosítsa vagy visszaállíthassák a jelszavukat. hello jelszavak írt vissza tooon a helyi Active Directory-várt módon.

Azonban ha egy adott felhasználó nincs konfigurálva a jelszóvisszaírás, vagy ha hello a felhasználó nem rendelkezik egy érvényes Azure AD-licenccel, hello felhasználó nem tudja frissíteni a jelszavát hello felhőben. Azok a jelszavát nem frissíthető, még akkor is, ha a jelszó lejárt. hello felhasználói inkább ezt az üzenetet látja: "a szervezet nem engedélyezi, hogy tooupdate jelszavát ezen a helyen. Adja a frissítést a szervezete által ajánlott toohello módszer alapján történik, vagy kérje meg a rendszergazdát, ha segítségre van szüksége." hello felhasználói vagy rendszergazdai hello rendelkezik tooreset a jelszavát a helyszíni Active Directoryban.

## <a name="how-does-pass-through-authentication-protect-you-against-brute-force-password-attacks"></a>Hogyan áteresztő hitelesítés elleni védelemre találgatásos jelszó támadások ellen?

Olvasási [Ez a cikk](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) további információt.

## <a name="what-do-pass-through-authentication-agents-communicate-over-ports-80-and-443"></a>Mi tegye áteresztő hitelesítés ügynökök kommunikálni 80-as és 443-as porton keresztül?

- hello hitelesítési ügynökök ellenőrizze a HTTPS-kéréseket a szolgáltatás minden műveletet a 443-as porton keresztül.
- hello hitelesítési ügynökök ellenőrizze a HTTP-kérelmek port 80 toodownload SSL visszavonttanúsítvány-listák keresztül.

     >[!NOTE]
     >A legújabb frissítéseket a Microsoft kevesebb hello hello szolgáltatás szükséges portokat. Ha az Azure AD Connect vagy hello hitelesítési ügynök régebbi verzióját, megnyitva, ezeket a portokat is: 5671, 8080-as, 9090, 9091, 9350, 9352 és 10100-10120.

## <a name="can-hello-pass-through-authentication-agents-communicate-over-an-outbound-web-proxy-server"></a>Egy kimenő proxy webkiszolgálón keresztül kommunikálhat hello áteresztő hitelesítés ügynökök?

Igen. Ha WPAD (Web Proxy Auto-Discovery) engedélyezve van a helyszíni környezetben, a hitelesítési ügynökök automatikusan toolocate kísérlet, és proxy-webkiszolgáló hello hálózati használhatja.

## <a name="can-i-install-two-or-more-pass-through-authentication-agents-on-hello-same-server"></a>Telepíthető két vagy több áteresztő hitelesítés-ügynökök hello ugyanarra a kiszolgálóra?

Nem, csak telepíthető egy csatlakoztatott hitelesítési ügynök egyetlen kiszolgálóra. Áteresztő hitelesítés tooconfigure a magas rendelkezésre állású, hajtsa végre ezen hello utasításokat [cikk](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability) helyette.

## <a name="i-already-use-active-directory-federation-services-ad-fs-for-azure-ad-sign-in-how-do-i-switch-it-toopass-through-authentication"></a>Már az Active Directory összevonási szolgáltatások (AD FS) az Azure AD-bejelentkezés. Átváltása azt tooPass keresztül hitelesítést?

Ha konfigurálta az AD FS legyen a hello Azure AD Connect varázsló segítségével bejelentkezési módszer, hello felhasználói bejelentkezési módszer váltása tooPass keresztül hitelesítést. Ez a módosítás lehetővé teszi, hogy átmenő hitelesítést hello tenant és alakít _összes_ a felügyelt tartományok külső tartományait. Az összes további bejelentkezési kérelmet az Ön bérelt szolgáltatásának áteresztő hitelesítés kezeli. Jelenleg nincs támogatott mód az Azure AD Connect toouse belül különböző tartományokban AD FS és az áteresztő hitelesítés kombinációja.

Ha az AD FS konfigurálása hello bejelentkezési módszer _kívül_ hello Azure AD Connect varázsló hello felhasználói bejelentkezési módszer váltása tooPass keresztül hitelesítést (hello "Nem konfigurálása" lehetőséget). Ez a módosítás lehetővé teszi, hogy átmenő hitelesítést hello tenant. Azonban a külső tartományok toouse AD FS bejelentkezésnél továbbra is. Használjon PowerShell toomanually convert néhány vagy az összes külső tartományok tooManaged területeken. Ezt követően minden bejelentkezési kérelmek felügyelt tartományok (_csak_) áteresztő hitelesítés kezeli.

>[!IMPORTANT]
>Áteresztő hitelesítés nem kezeli az bejelentkezés csak felhőalapú Azure Active Directory-felhasználók.

## <a name="can-i-use-pass-through-authentication-in-a-multi-forest-ad-environment"></a>Használható átmenő hitelesítés AD Többerdős környezetben?

Igen. Többerdős környezetben támogatottak, ha a AD erdők között erdőszintű megbízhatóság, és ha névutótag megfelelően van beállítva.

## <a name="do-pass-through-authentication-agents-provide-load-balancing-capability"></a>Áteresztő hitelesítés ügynökök nyújtanak terheléselosztási funkció?

Nem, több áteresztő hitelesítés ügynökök telepítése biztosítja [magas rendelkezésre állású](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability), de nem terheléselosztás. Egy vagy két hello hitelesítési ügynökök is szükségessé tehet hello tömeges hello bejelentkezési kérelmek kezelése.

Jelszó ellenőrzési kérések, amelyek a hitelesítési ügynökök kell toohandle hello használata egyszerűsített. Ezért maximális és átlagos terhelés a legtöbb ügyfél könnyen képes kezelni két vagy három hitelesítési ügynökök összesen.

Azt javasoljuk, hogy telepítse a hitelesítési ügynökök Bezárás tooyour tartományvezérlők tooimprove bejelentkezési késés.

## <a name="can-i-install-hello-first-pass-through-authentication-agent-on-a-server-other-than-hello-one-that-runs-azure-ad-connect"></a>Telepíthető hello első áteresztő hitelesítés ügynök egy, az Azure AD Connect futtató hello azon?

Nem, ez a forgatókönyv van _nem_ támogatott.

## <a name="how-many-pass-through-authentication-agents-should-i-install"></a>Hány áteresztő hitelesítés ügynököt kell telepíteni?

Azt javasoljuk, hogy:

- Két vagy három hitelesítési ügynökök teljes telepítése. Ez a konfiguráció is elegendő a legtöbb felhasználó.
- A tartományvezérlőn (vagy Bezárás toothem lehetőség szerinti) hitelesítési ügynökök tooimprove bejelentkezési késés telepítése.
- Biztosíthatja, hogy (ha hitelesítési ügynök telepítve van) kiszolgálók hozzá szeretné adni toohello ugyanazt az AD-erdő hello felhasználók, amelyeknek a jelszavánál toobe érvényesíteni kell.

>[!NOTE]
>A rendszer korlátainak 12 hitelesítési ügynökök bérlőnként van.

## <a name="how-can-i-disable-pass-through-authentication"></a>Hogyan letilthatja az áteresztő hitelesítés?

Futtassa újra a hello Azure AD Connect varázsló, és módosítsa a hello felhasználói bejelentkezési módszer áteresztő hitelesítés tooanother metódusból. Ez letiltja az átmenő hitelesítést hello tenant és hello hitelesítési ügynök eltávolítja az alkalmazást hello. Lehetősége van toomanually eltávolítás hello hitelesítési ügynökök más kiszolgálókról.

## <a name="what-happens-when-i-uninstall-a-pass-through-authentication-agent"></a>Mi történik, ha távolítható el egy átmenő hitelesítés ügynök?

Egy csatlakoztatott hitelesítési ügynök olyan kiszolgálóról távolít azt eredményezi, akkor toostop bejelentkezési kérelmek fogadása. Győződjön meg arról, hogy egy másik hitelesítési ügynök fut, ez a művelet megszakítása a felhasználói bejelentkezési a tenant tooavoid elvégzése előtt.

## <a name="next-steps"></a>Következő lépések
- [**Aktuális korlátozások** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) – Ez a funkció jelenleg előzetes verzió. Ismerje meg, melyik forgatókönyvek is támogatottak, és melyek nem.
- [**Gyors üzembe helyezési** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) - létrehozásához, és fut az Azure AD áteresztő hitelesítés.
- [**Műszaki mélyreható** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) – Ez a funkció működésének megismerése.
- [**Hibaelhárítás** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -megtudhatja, hogyan tooresolve közös hello szolgáltatást érintő problémákat.
- [**Az Azure AD zökkenőmentes SSO** ](active-directory-aadconnect-sso.md) -további információ a kiegészítő funkció.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – új funkciókérések tárolásához.
