---
title: "Az Azure AD-áteresztő hitelesítés – gyors üzembe helyezési |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooget el az Azure Active Directory (Azure AD) áteresztő hitelesítés."
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
ms.date: 08/23/2017
ms.author: billmath
ms.openlocfilehash: d6d0f85fe144cf36cc94676f6592d37988b20647
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-quick-start"></a>Az Azure Active Directory átmenő hitelesítést: Gyors üzembe helyezési

## <a name="how-toodeploy-azure-ad-pass-through-authentication"></a>Hogyan toodeploy Azure AD áteresztő hitelesítés

Az Azure Active Directory (Azure AD) áteresztő hitelesítés lehetővé teszi, hogy a felhasználók toosign tooboth a helyszíni és felhőalapú alkalmazások hello ugyanazt a jelszót. Felhasználók akkor jelentkezik be a jelszavuk közvetlenül a helyszíni Active Directory általi érvényesítésével.

>[!IMPORTANT]
>Az Azure AD áteresztő hitelesítés jelenleg előzetes verzió. Használja a szolgáltatás előzetes keresztül, ha győződjön meg arról, hogy hello hitelesítési ügynökök preview verzióinak frissítése hello utasításokat követve [Itt](./active-directory-aadconnect-pass-through-authentication-upgrade-preview-authentication-agents.md).

Toofollow kell ezen utasításokat toodeploy áteresztő hitelesítés:

## <a name="step-1-check-prerequisites"></a>1. lépés: Követelmények ellenőrzésének megismétlése

Győződjön meg arról, hogy hello előfeltételeiről a következő helyen:

### <a name="on-hello-azure-active-directory-admin-center"></a>A hello Azure Active Directory felügyeleti központ

1. Hozzon létre egy kizárólag felhőalapú globális rendszergazdai fiókot az Azure AD-bérlő. Ezzel a módszerrel kezelheti a bérlő konfigurációjának hello kell a helyszíni szolgáltatások sikertelen, vagy már nem érhető el. További tudnivalók [csak felhőalapú globális rendszergazdai fiókot hozzá](../active-directory-users-create-azure-portal.md). Ez a lépés elvégzése, akkor nem get zárolja a bérlő kritikus tooensure.
2. Vegyen fel egy vagy több [egyéni tartomány neve](../active-directory-add-domain.md) tooyour az Azure AD bérlői. A felhasználók jelentkezzen be a tartomány neveinek egyike.

### <a name="in-your-on-premises-environment"></a>A helyszíni környezetben

1. Azonosítsa a Windows Server 2012 R2 vagy újabb mely toorun meg az Azure AD Connect futtató kiszolgálón. Adja hozzá hello server toohello ugyanazt az AD-erdő hello felhasználók, amelyeknek a jelszavánál toobe érvényesíteni kell.
2. Telepítse a hello [az Azure AD Connect legújabb verziójának](https://www.microsoft.com/download/details.aspx?id=47594) előző lépésben azonosított hello kiszolgálón. Ha már rendelkezik az Azure AD Connect fut, győződjön meg arról, hogy hello verziót 1.1.557.0 vagy újabb.
3. További Windows Server 2012 R2 rendszerű kiszolgáló azonosítani vagy későbbi mely önálló hitelesítési ügynök a toorun. hello hitelesítési ügynök verzióját kell toobe 1.5.193.0 vagy újabb. A kiszolgáló nem szükséges tooensure bejelentkezési kérelmek magas rendelkezésre állású. Adja hozzá hello server toohello ugyanazt az AD-erdő hello felhasználók, amelyeknek a jelszavánál toobe érvényesíteni kell.
4. Ha van tűzfal a kiszolgálók és az Azure AD között, a következő elemek tooconfigure hello szüksége:
   - Győződjön meg arról, hogy a hitelesítési ügynökök tehet **kimenő** tooAzure AD kérelmek hello a következő portokon keresztül:
   
   | Portszám | Hogyan használja fel azokat |
   | --- | --- |
   | **80** | Letölti a tanúsítvány-visszavonási listákat (CRL) hello SSL-tanúsítvány érvényesítése közben |
   | **443** | A szolgáltatás minden kimenő kommunikáció |
   
   Ha a tűzfal toooriginating felhasználók szerint szabályokat érvényesíti, nyissa meg ezeket a portokat, a forgalom hálózati szolgáltatásként futó Windows-szolgáltatások.
   - Ha a tűzfal vagy a proxy lehetővé teszi a DNS engedélyezett, az engedélyezett kapcsolatok túl**\*. msappproxy.net** és  **\*. servicebus.windows.net**. Ha nem engedélyezi a hozzáférést túl[Azure DataCenter IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653), amely hetente frissíteni.
   - A hitelesítési ügynökök hozzá kell férniük túl**login.windows.net** és **login.microsoftonline.com** kezdeti regisztrációs, ezért nyissa meg a tűzfal, valamint az URL-címeket a.
   - A tanúsítvány érvényesítési feloldása a következő URL-címek hello: **mscrl.microsoft.com:80**, **crl.microsoft.com:80**, **ocsp.msocsp.com:80** és  **www.microsoft.com:80**. Ezért előfordulhat, hogy már nincs zárolva URL-URL-tanúsítvány érvényesítése más Microsoft-termékekkel használ.

## <a name="step-2-enable-exchange-activesync-support-optional"></a>2. lépés: (Opcionális) az Exchange ActiveSync-támogatás engedélyezése

Hajtsa végre az ezen utasításokat tooenable Exchange ActiveSync-támogatását:

1. Használjon [Exchange PowerShell](https://technet.microsoft.com/library/mt587043(v=exchg.150).aspx) toorun hello a következő parancsot:
```
Get-OrganizationConfig | fl per*
```

2. Ellenőrizze a hello hello értékének `PerTenantSwitchToESTSEnabled` beállítást. Ha hello érték **igaz**, a bérlő helyesen van konfigurálva – ez általában a legtöbb felhasználó hello eset áll. Ha hello érték **hamis**- ben futtassa hello következő parancsot:
```
Set-OrganizationConfig -PerTenantSwitchToESTSEnabled:$true
```

3. Győződjön meg arról, hogy hello hello értékének `PerTenantSwitchToESTSEnabled` beállítás értéke túl**igaz**. Várjon egy órát áthelyezése toohello következő lépés előtt.

Ha probléma merül fel ezzel a lépéssel nyomtatott oldallal, ellenőrizze a [hibaelhárítási útmutatója](active-directory-aadconnect-troubleshoot-pass-through-authentication.md#exchange-activesync-configuration-issues) további információt.

## <a name="step-3-enable-hello-feature"></a>3. lépés: Hello szolgáltatás engedélyezése

Áteresztő hitelesítés használatával engedélyezhető [az Azure AD Connect](active-directory-aadconnect.md).

>[!IMPORTANT]
>Áteresztő hitelesítés hello Azure AD Connect elsődleges vagy egy átmeneti kiszolgálón engedélyezhető. Erősen ajánlott engedélyezni a hello elsődleges kiszolgálóról.

Ha először telepíti az Azure AD Connect a hello, válassza ki azt a hello [egyéni telepítési útvonal](active-directory-aadconnect-get-started-custom.md). A hello **felhasználói bejelentkezés** lapon, válassza ki **áteresztő hitelesítés** , bejelentkezési hello metódus. A művelet sikeresen befejeződött, egy áteresztő hitelesítés ügynök telepítve van hello Azure AD Connect ugyanarra a kiszolgálóra. Ezenkívül hello áteresztő hitelesítés szolgáltatás engedélyezve van a tenant.

![Az Azure AD Connect - felhasználói bejelentkezés](./media/active-directory-aadconnect-sso/sso3.png)

Ha már telepítette az Azure AD Connect (hello segítségével [Expressz telepítés](active-directory-aadconnect-get-started-express.md) vagy hello [egyéni telepítési](active-directory-aadconnect-get-started-custom.md) elérési út) elemre, jelölje be **módosítás felhasználói bejelentkezési oldal** Azure ad-val Csatlakozás, és kattintson a **következő**. Válassza ki **áteresztő hitelesítés** , bejelentkezési hello metódus. A művelet sikeresen befejeződött, egy áteresztő hitelesítés ügynök telepítve van hello ugyanarra a kiszolgálóra az Azure AD Connect és hello szolgáltatás engedélyezve van a bérlő.

![Az Azure AD Connect - módosítás felhasználói bejelentkezés](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

>[!IMPORTANT]
>Átmenő hitelesítést egy olyan bérlői szintű szolgáltatás. Bejelentkezés a felhasználók több bekapcsolását befolyásolja _összes_ hello felügyelt tartományok az Ön bérelt szolgáltatásának. Ha az AD FS tooPass keresztül hitelesítési vált, azt javasoljuk, hogy legalább 12 órában az AD FS infrastruktúra leállítása előtt várja – a várakozási idő tooensure, hogy a felhasználók is tartsa bejelentkezés tooExchange ActiveSync átmenet közben.

## <a name="step-4-test-hello-feature"></a>4. lépés: Hello szolgáltatás tesztelése

Hajtsa végre az ezen utasításokat tooverify megfelelően engedélyezte az áteresztő hitelesítés:

1. Jelentkezzen be toohello [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com) hello globális rendszergazdai hitelesítő adataival, a bérlő számára.
2. Válassza ki **Azure Active Directory** hello bal oldali navigációs.
3. Válassza ki **az Azure AD Connect**.
4. Győződjön meg arról, hogy hello **áteresztő hitelesítés** funkció jelenít meg, mint a **engedélyezve**.
5. Válassza ki **áteresztő hitelesítés**. Ezen a panelen hello kiszolgálók, amelyen telepítve van-e a hitelesítés ügynök sorolja fel.

![Az Azure Active Directory felügyeleti központ bemutatása – az Azure AD Connect panel](./media/active-directory-aadconnect-pass-through-authentication/pta7.png)

![Az Azure Active Directory felügyeleti központ – áteresztő hitelesítés panel](./media/active-directory-aadconnect-pass-through-authentication/pta8.png)

Ebben a szakaszban található összes felügyelt tartomány az Ön bérelt szolgáltatásának felhasználók bejelentkezhetnek áteresztő hitelesítés használatával. Összevont tartományokban lévő felhasználók azonban továbbra is az toosign az Active Directory összevonási szolgáltatások (AD FS) vagy egy másik, amelyet korábban konfigurált összevonás-szolgáltatóként. Az összevont toomanaged át egy tartományhoz, az adott tartomány összes felhasználó indul el automatikusan jelentkezik be, hogy átmenő hitelesítést használ. Csak felhőalapú felhasználó nem érintett hello áteresztő hitelesítés szolgáltatás.

## <a name="step-5-ensure-high-availability"></a>5. lépés: Magas rendelkezésre állásának biztosításához.

Ha azt tervezi, hogy átmenő hitelesítést toodeploy éles környezetben, telepítenie kell egy önálló hitelesítési ügynök. A második hitelesítési ügynök telepítése a kiszolgálón _más_ , mint egy fut az Azure AD Connect hello és hello első hitelesítési ügynök. Ez a beállítás akkor magas rendelkezésre állást biztosít a bejelentkezési kérelmek. Hajtsa végre az ezen utasításokat toodeploy hitelesítési ügynök önálló:

1. **Hello hello hitelesítési ügynök legújabb verziójának letöltése (1.5.193.0 verzió vagy újabb)**: toohello bejelentkezés [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com) a bérlő globális rendszergazdai hitelesítő adataival.
2. Válassza ki **Azure Active Directory** hello bal oldali navigációs.
3. Válassza ki **az Azure AD Connect** , majd **áteresztő hitelesítés**. Válassza ki **letöltési ügynök**.
4. Kattintson a hello **feltételek elfogadásának & Letöltés** gombra.
5. **Hello hello hitelesítési ügynök legújabb verziójának telepítéséhez**: hello lépést megelőző letöltött végrehajtható futtató hello. Adja meg a bérlő globális rendszergazda hitelesítő adatait, amikor a rendszer kéri.

![Az Azure Active Directory felügyeleti központ – hitelesítési ügynök letöltése gomb](./media/active-directory-aadconnect-pass-through-authentication/pta9.png)

![Az Azure Active Directory felügyeleti központ - ügynök letöltése panel](./media/active-directory-aadconnect-pass-through-authentication/pta10.png)

>[!NOTE]
>Emellett letöltheti a hitelesítési ügynök hello [Itt](https://aka.ms/getauthagent). Győződjön meg arról, hogy tekintse át és fogadja el a hello hitelesítési ügynök [szolgáltatási szerződését](https://aka.ms/authagenteula) _előtt_ telepíti azt.

## <a name="next-steps"></a>Következő lépések
- [**Aktuális korlátozások** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) – Ez a funkció jelenleg előzetes verzió. Ismerje meg, melyik forgatókönyvek is támogatottak, és melyek nem.
- [**Műszaki mélyreható** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) – Ez a funkció működésének megismerése.
- [**Gyakran ismételt kérdések** ](active-directory-aadconnect-pass-through-authentication-faq.md) -kérdések toofrequently válaszol.
- [**Hibaelhárítás** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -megtudhatja, hogyan tooresolve közös hello szolgáltatást érintő problémákat.
- [**Az Azure AD zökkenőmentes SSO** ](active-directory-aadconnect-sso.md) -további információ a kiegészítő funkció.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – új funkciókérések tárolásához.
