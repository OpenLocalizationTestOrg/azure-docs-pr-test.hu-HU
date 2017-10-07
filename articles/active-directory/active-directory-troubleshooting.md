---
title: "Hibaelhárítás: \"Active Directory\" elem nem található vagy nem érhető el |} Microsoft Docs"
description: "Milyen toodo, amikor az Active Directory menüpont hello Azure felügyeleti portál nem jelenik meg."
services: active-directory
documentationcenter: na
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 3383020d-6397-43ea-b7aa-c6a9d6a1e3df
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: bryanla
ms.openlocfilehash: d7355a4e39141f0b09272dc5615c309b23c8c70f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-active-directory-item-is-missing-or-not-available"></a>Hibaelhárítás: "Active Directory" elem nem található vagy nem érhető el
Hello utasítások az Azure Active Directory-szolgáltatások és szolgáltatások használatával számos kezdődhet "nyissa meg az Azure felügyeleti portálon toohello és **Active Directory**." Mi a teendő ha hello Active Directory kiterjesztés vagy a menü elem nem jelenik meg, vagy ha meg van jelölve, de **nem érhető el**? Ez a témakör a tervezett toohelp. Ismerteti, hogyan hello feltételeket, amelyek alapján **Active Directory** nem jelenik meg vagy nem érhető el, és elmagyarázza, hogyan tooproceed.

## <a name="active-directory-is-missing"></a>Az Active Directory hiányzik.
Általában egy **Active Directory** hello bal oldali navigációs menü elem jelenik meg. Azure Active Directory eljárások hello utasítások feltételezik, hogy ezt az elemet a nézetben.

![Képernyőfelvétel: az Azure Active Directory](./media/active-directory-troubleshooting/typical-view.png)

hello bal oldali navigációs menü hello Active Directory elem jelenik meg, ha hello a következő feltételek bármelyike teljesül. Ellenkező esetben hello elem nem jelenik meg.

* (korábbi nevén Windows Live ID) Microsoft-fiókkal bejelentkezve hello aktuális felhasználó.
  
    VAGY
* hello Azure-bérlőhöz tartozik egy könyvtárat, valamint hello jelenlegi fiókot directory rendszergazda.
  
    VAGY
* Azure-bérlőhöz hello legalább egy Azure AD-hozzáférés-vezérlés (ACS) névtérrel. További információkért lásd: [hozzáférés-vezérlési Namespace](https://msdn.microsoft.com/library/azure/gg185908.aspx).
  
    VAGY
* Azure-bérlőhöz hello van legalább egy Azure multi-factor Authentication-szolgáltató. További információkért lásd: [Administering Azure többtényezős hitelesítési szolgáltatók](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

toocreate egy hozzáférés-vezérlés névtér vagy egy többtényezős hitelesítési szolgáltató kattintson **+ új** > **alkalmazásszolgáltatások** > **Active Directory**.

tooget rendszergazdai jogosultságokkal tooa directory rendelkezik egy rendszergazda rendelje hozzá a rendszergazda szerepkör tooyour fiók. További információkért lásd: [rendszergazdai szerepkörök hozzárendelése](active-directory-assign-admin-roles.md).

## <a name="active-directory-is-not-available"></a>Active Directory nem érhető el.
Amikor rákattint **+ új** > **alkalmazásszolgáltatások**, egy **Active Directory** elem jelenik meg. Pontosabban a hello Active Directory elem jelenik meg, amikor hello Active Directory-szolgáltatások Directory, hozzáférés-vezérlés és többtényezős hitelesítésszolgáltató, például bármelyike elérhető toohello aktuális felhasználó.

Azonban hello oldal betöltése, amíg hello elem nem aktív, és van megjelölve **nem érhető el**. Ez egy ideiglenes állapot. Várjon néhány másodpercet, ha hello elem elérhetővé válik. Hello késleltetés meghosszabbítják, ha frissíti hello weblap gyakran megszünteti hello probléma.

![Képernyőfelvétel: az Active Directory nem érhető el.](./media/active-directory-troubleshooting/not-available.png)

