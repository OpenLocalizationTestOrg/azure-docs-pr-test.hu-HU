---
title: "Az Azure AD Connect: Áteresztő hitelesítés - frissítési előzetes hitelesítés ügynökök |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooupgrade az Azure Active Directory (Azure AD) áteresztő hitelesítés konfigurációját."
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
ms.date: 08/04/2017
ms.author: billmath
ms.openlocfilehash: 1695326b182278944e9f1584da72b18862b6ef27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-upgrade-preview-authentication-agents"></a>Az Azure Active Directory áteresztő hitelesítés: Frissítési előzetes hitelesítés ügynökök

## <a name="overview"></a>Áttekintés

Ez a cikk az előzetes Azure AD átmenő hitelesítést használó ügyfelek számára. A Microsoft nemrég frissített (és rebranded) hello hitelesítési ügynök szoftver. Too_manually_ frissítési előzetes hitelesítés ügynökök telepítése a helyszíni kiszolgálókon van szüksége. A kézi frissítés egy egyszeri művelet. Az összes jövőbeni tooAuthentication ügynökök frissítései automatikus. hello okok tooupgrade a következők:

- Hitelesítési ügynökök hello előzetes verziói nem kapják meg semmilyen további biztonsági vagy hibajavításokat tartalmaz.
-   Hitelesítési ügynökök hello előzetes verziói nem telepíthető a további kiszolgálók, a magas rendelkezésre állású.

## <a name="check-versions-of-your-authentication-agents"></a>Ellenőrizze a hitelesítési ügynökök verziói

### <a name="step-1-check-where-your-authentication-agents-are-installed"></a>1. lépés: Ellenőrizze, amelyen telepítve van-e a hitelesítés ügynök

Hajtsa végre ezeket a lépéseket toocheck, amelyen telepítve van-e a hitelesítés ügynök:

1. Jelentkezzen be toohello [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com) hello globális rendszergazdai hitelesítő adataival, a bérlő számára.
2. Válassza ki **Azure Active Directory** hello bal oldali navigációs.
3. Válassza ki **az Azure AD Connect**. 
4. Válassza ki **áteresztő hitelesítés**. Ezen a panelen hello kiszolgálók, amelyen telepítve van-e a hitelesítés ügynök sorolja fel.

![Az Azure Active Directory felügyeleti központ – áteresztő hitelesítés panel](./media/active-directory-aadconnect-pass-through-authentication/pta8.png)

### <a name="step-2-check-hello-versions-of-your-authentication-agents"></a>2. lépés: Ellenőrizze a hitelesítési ügynökök hello verziói

a hitelesítési ügynökök, az egyes kiszolgálókon hello megelőző lépésben azonosított toocheck hello verziói kövesse az alábbi utasításokat:

1. Nyissa meg túl**Vezérlőpult -> Programok -> Programok és szolgáltatások** hello helyszíni kiszolgálón.
2. Ha a bejegyzést "**Microsoft Azure AD Connect hitelesítési ügynök**", nem kell tootake bármely művelet ezen a kiszolgálón.
3. Ha a bejegyzést "**Microsoft Azure AD alkalmazásproxy-összekötő**", verzió 1.5.132.0 vagy korábbi verziójú toomanually kell frissítési ezen a kiszolgálón.

![Hitelesítési ügynök előzetes verzióját](./media/active-directory-aadconnect-pass-through-authentication/pta6.png)

## <a name="best-practices-toofollow-before-starting-hello-upgrade"></a>Gyakorlati tanácsok toofollow hello frissítés megkezdése előtt

A frissítés előtt ellenőrizze, hogy a következő helyen elemek hello:

1. **Csak felhőalapú globális rendszergazdai fiók létrehozása**: nem a meglévő anélkül, hogy egy kizárólag felhőalapú globális rendszergazdai fiók toouse vészhelyzeti olyan esetekben, ahol az áteresztő hitelesítés ügynökök nem megfelelően működnek-e. További tudnivalók [csak felhőalapú globális rendszergazdai fiókot hozzá](../active-directory-users-create-azure-portal.md). Ez a lépés elvégzése alapvető fontosságú, és biztosítja, hogy Ön nem ártatlan tévedéssel a bérlő.
2.  **Magas rendelkezésre állásának biztosításához**: Ha korábban nem sikerült befejezni, telepítse a második önálló hitelesítési ügynök tooprovide magas rendelkezésre állás a bejelentkezési kérelmek, ezeket [utasításokat](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability).

## <a name="upgrading-hello-authentication-agent-on-your-azure-ad-connect-server"></a>Hello hitelesítési ügynök a Azure AD Connect-kiszolgáló frissítése

Hello hitelesítési ügynök a hello frissítése előtt kell frissítenie az Azure AD Connect ugyanarra a kiszolgálóra. Az elsődleges és az Azure AD Connect-kiszolgálók átmeneti kövesse az alábbi lépéseket:

1. **Az Azure AD Connect frissítése**: folytassa a [cikk](./active-directory-aadconnect-upgrade-previous-version.md) és toohello legújabb az Azure AD Connect verzióra frissíteni.
2. **Távolítsa el a hitelesítési ügynök hello hello előzetes verzióját**: letöltése [a PowerShell parancsfájl](https://aka.ms/rmpreviewagent) , és futtassa rendszergazdaként hello kiszolgálón.
3. **Hello hello hitelesítési ügynök legújabb verziójának letöltése (1.5.193.0 verzió vagy újabb)**: toohello bejelentkezés [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com) a bérlő globális rendszergazdai hitelesítő adataival. Válassza ki **Azure Active Directory -> az Azure AD Connect -> áteresztő hitelesítés -> Letöltés ügynök**. Fogadja el a szolgáltatási feltételek hello és hello hello hitelesítési ügynök legújabb verziójának letöltéséhez.
4. **Hello hello hitelesítési ügynök legújabb verziójának telepítéséhez**: 3. lépésben letöltött végrehajtható futtató hello. Adja meg a bérlő globális rendszergazda hitelesítő adatait, amikor a rendszer kéri.
5. **Győződjön meg arról, hogy hello legújabb verziója telepítve van**: látható előtt, lépjen túl**Vezérlőpult -> Programok -> Programok és szolgáltatások** , és ellenőrizze, hogy van-e bejegyzés "**Microsoft Azure AD Connect Hitelesítési ügynök**".

>[!NOTE]
>Ha bejelöli a hello áteresztő hitelesítés paneljét hello [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com) hello fentebbi lépések elvégzése után két hitelesítési ügynök bejegyzések kiszolgálónként - megjelenítő hello egy bejegyzés megjelenik Hitelesítési ügynököt **aktív** és egyéb hello **inaktív**. Ez az _várt_. Hello **inaktív** néhány nap múlva automatikusan eltávolítja a bejegyzést.

## <a name="upgrading-hello-authentication-agent-on-other-servers"></a>Hello hitelesítési ügynök a többi kiszolgáló frissítése

Hajtsa végre ezeket a lépéseket tooupgrade hitelesítési ügynökök más kiszolgálókon (amelyen nincs telepítve az Azure AD Connect):

1. **Távolítsa el a hitelesítési ügynök hello hello előzetes verzióját**: letöltése [a PowerShell parancsfájl](https://aka.ms/rmpreviewagent) , és futtassa rendszergazdaként hello kiszolgálón.
2. **Hello hello hitelesítési ügynök legújabb verziójának letöltése (1.5.193.0 verzió vagy újabb)**: toohello bejelentkezés [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com) a bérlő globális rendszergazdai hitelesítő adataival. Válassza ki **Azure Active Directory -> az Azure AD Connect -> áteresztő hitelesítés -> Letöltés ügynök**. Fogadja el a szolgáltatási feltételek hello és hello legújabb verzió letöltéséhez.
3. **Hello hello hitelesítési ügynök legújabb verziójának telepítéséhez**: 2. lépésben letöltött végrehajtható futtató hello. Adja meg a bérlő globális rendszergazda hitelesítő adatait, amikor a rendszer kéri.
4. **Győződjön meg arról, hogy hello legújabb verziója telepítve van**: látható előtt, lépjen túl**Vezérlőpult -> Programok -> Programok és szolgáltatások** , és ellenőrizze, hogy van-e nevezett bejegyzés **Microsoft Azure AD Connect Hitelesítési ügynök**.

>[!NOTE]
>Ha bejelöli a hello áteresztő hitelesítés paneljét hello [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com) hello fentebbi lépések elvégzése után két hitelesítési ügynök bejegyzések kiszolgálónként - megjelenítő hello egy bejegyzés megjelenik Hitelesítési ügynököt **aktív** és egyéb hello **inaktív**. Ez az _várt_. Hello **inaktív** néhány nap múlva automatikusan eltávolítja a bejegyzést.

## <a name="next-steps"></a>Következő lépések
- [**Hibaelhárítás** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -megtudhatja, hogyan tooresolve közös hello szolgáltatást érintő problémákat.
