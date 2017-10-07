---
title: "Active Directory-Identity Protection - aaaAzure hogyan toounblock felhasználók |} Microsoft Docs"
description: "Megtudhatja, hogyan feloldása az Azure Active Directory Identity Protection-házirend által blokkolt felhasználóknak."
services: active-directory
keywords: "az Azure active directory azonosító adatok védelmét, felhasználó tiltásának feloldása"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: a953d425-a3ef-41f8-a55d-0202c3f250a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: cdda2808822888f76aa75cf46478738c94df51a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection---how-toounblock-users"></a>Az Azure Active Directory Identity Protection - hogyan toounblock felhasználók
Az Azure Active Directory azonosító adatok védelmét beállíthatja a házirendek tooblock felhasználók hello konfiguráltságát feltételek teljesülnek. Általában egy letiltott felhasználó névjegyek súgó ügyfélszolgálati toobecome feloldva. A témakörök ismerteti hello toounblock egy letiltott felhasználó végezheti el.

## <a name="determine-hello-reason-for-blocking"></a>Blokkolja hello okának meghatározása
Egy első lépés toounblock egy felhasználó, mint toodetermine hello típusú házirendet, amely hello felhasználó van tiltva, mert a következő lépéseket a attól függően, hogy szüksége van.
Az Azure Active Directory azonosító adatok védelmét a felhasználó vagy blokkolhatja a bejelentkezési kockázat házirendnek vagy egy felhasználó kockázat házirendnek.

Kaphat házirendet, amely a hello címsor hello párbeszédpanelen bemutatott toohello felhasználó egy bejelentkezési kísérlet során a felhasználó letiltotta hello típusát:

| Szabályzat | Felhasználó párbeszédpanel |
| --- | --- |
| Bejelentkezési kockázata |![Letiltott bejelentkezés](./media/active-directory-identityprotection-unblock-howto/02.png) |
| Felhasználói kockázata |![Letiltott fiók](./media/active-directory-identityprotection-unblock-howto/104.png) |

A felhasználó által blokkolt:

* Gyanús bejelentkezés más néven egy bejelentkezési kockázat házirendnek
* Egy felhasználó kockázat házirendnek is olyan veszélyben fiók

## <a name="unblocking-suspicious-sign-ins"></a>Feloldásáról gyanús bejelentkezések
toounblock gyanús bejelentkezés, a következő beállítások hello rendelkezik:

1. **Bejelentkezés ismerős helyről vagy eszköz** -gyakori oka a letiltott gyanús bejelentkezések ismeretlen helyekről vagy eszközök bejelentkezési kísérlet. A felhasználók gyorsan megállapíthatja, hogy ez-e hello OK letiltására, amit próbált toosign az egy ismert helyre vagy egy eszközt a.
2. **Zárja ki a házirend** – Ha úgy gondolja, hogy hello aktuális beállításait, a bejelentkezési házirend olyan felhasználók számára problémákat okoz, kizárhatja hello felhasználók belőle. Lásd: [Azure Active Directory Identity Protection](active-directory-identityprotection.md) további részleteket.
3. **Tiltsa le a házirend** – Ha úgy gondolja, hogy a házirend-konfigurációt a felhasználók számára problémákat okoz, letilthatja hello házirend. Lásd: [Azure Active Directory Identity Protection](active-directory-identityprotection.md) további részleteket.

## <a name="unblocking-accounts-at-risk"></a>Veszélyben feloldásáról fiókok
egy olyan veszélyben fiók toounblock, alábbi beállítások hello van:

1. **Jelszó-átállítási** -visszaállíthatja hello felhasználó jelszavát. Lásd: [manuális biztonságos jelszó-átállítási](active-directory-identityprotection.md#manual-secure-password-reset) további részleteket.
2. **Minden kockázati események elvetése** -hello felhasználói kockázat házirendnek blokkolja a felhasználó, ha elérte a konfigurált hello felhasználói kockázati szintjét blokkolja a hozzáférést. Csökkentheti a felhasználó kockázati események jelentett manuálisan bezárásával kockázati szintjét. További részletekért lásd: [kockázati események manuálisan bezárása](active-directory-identityprotection.md#closing-risk-events-manually).
3. **Zárja ki a házirend** – Ha úgy gondolja, hogy hello aktuális beállításait, a bejelentkezési házirend olyan felhasználók számára problémákat okoz, kizárhatja hello felhasználók belőle. Lásd: [Azure Active Directory Identity Protection](active-directory-identityprotection.md) további részleteket.
4. **Tiltsa le a házirend** – Ha úgy gondolja, hogy a házirend-konfigurációt a felhasználók számára problémákat okoz, letilthatja hello házirend. Lásd: [Azure Active Directory Identity Protection](active-directory-identityprotection.md) további részleteket.

## <a name="next-steps"></a>Következő lépések
 További információk az Azure AD Identity Protection tooknow szeretne? Tekintse meg [Azure Active Directory Identity Protection](active-directory-identityprotection.md).
