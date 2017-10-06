---
title: "Az Identitáskezelés aaaPrivileged - előfizetések Azure |} Microsoft Docs"
description: "Ismerteti a hello előfizetés és a licencelés kezelése és az Azure AD Privileged Identity Management használatát a bérlő követelményei"
services: active-directory
documentationcenter: 
author: barclayn
manager: mbaldwin
editor: mwahl
ms.assetid: 34367721-8b42-4fab-a443-a2e55cdbf33d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: barclayn
ms.custom: pim
ms.openlocfilehash: 2639d13c250a582fdcf0b277c9bab37fdfcabcb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-privileged-identity-management-subscription-requirements"></a>Az Azure Active Directory Privileged Identity Management előfizetés követelményeinek

Az Azure AD Privileged Identity Management érhető el az Azure AD Premium P2 hello edition részeként. További információ a hello P2 és összehasonlítja azt tooPremium P1, a más szolgáltatását: [Azure Active Directory-kiadások](../active-directory-editions.md).

>[!NOTE]
Ha az Azure Active Directory (Azure AD) Privileged Identity Management preview volt, volt nem licenc ellenőrzi a bérlő tootry hello szolgáltatás számára.  Most, hogy az Azure AD Privileged Identity Management elérte az általánosan rendelkezésre álló, egy próbaverziós vagy fizetős előfizetésre hello bérlői toocontinue Privileged Identity Management használatával a 2016. December után jelen kell lennie.
  

## <a name="confirm-your-trial-or-paid-subscription"></a>Erősítse meg a próbaverziós vagy fizetős előfizetésre

Ha nem biztos abban, hogy a szervezet rendelkezik-e a próbaverziójának vagy megvásárolt előfizetést, majd ellenőrizheti hogy van-e a bérlő előfizetés-parancsokkal hello Azure Active Directory modul a Windows PowerShell V1 szerepel. 
1. Nyisson meg egy PowerShell-ablakot.
2. Adja meg `Connect-MsolService` tooauthenticate a bérlő felhasználói.
3. Adja meg `Get-MsolSubscription | ft SkuPartNumber,IsTrial,Status`.

A parancs segítségével lekérdezhető az Ön bérelt szolgáltatásának hello előfizetések listáját. Ha nincsenek sorok visszaadva, akkor szüksége lesz egy Azure AD Premium P2 tooobtain próbaverzió, vásárolhat egy Azure AD Premium P2-előfizetés vagy az EMS E5 előfizetés toouse Azure AD Privileged Identity Management.  próbaverzió tooget és Azure AD Privileged Identity Management használatával start olvasási [Ismerkedés az Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-getting-started.md).

Ha ez a parancs visszaadja a sor mely SkuPartNumber "AAD_PREMIUM_P2" vagy "EMSPREMIUM" és IsTrial értéke "True", az azt jelenti, az Azure AD Premium P2 próbaverziójával hello bérlői szerepel.  Ha nincs engedélyezve a hello előfizetés állapotát, és nem rendelkezik az Azure AD Premium P2- vagy EMS E5 előfizetés beszerzési, majd meg kell vásárolnia az Azure AD Premium P2-előfizetés vagy az EMS E5 előfizetés toocontinue Azure AD Privileged Identity Management használatával.

Az Azure AD Premium P2 keresztül érhető el egy [Microsoft nagyvállalati szerződés](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx), hello [nyissa meg a mennyiségi licencelési programon](https://www.microsoft.com/en-us/licensing/licensing-programs/open-license.aspx), és hello [megoldás szolgáltatók program](https://partner.microsoft.com/en-US/cloud-solution-provider). Office 365 és az Azure-előfizetők vásárolhatja online az Azure AD Premium P2.  További információ a Azure AD Premium árképzési, és hogyan tooorder online helyen találhatók [Azure Active Directory árképzési](https://azure.microsoft.com/en-us/pricing/details/active-directory/).

## <a name="azure-ad-privileged-identity-management-is-not-available-in-tenant"></a>Az Azure AD Privileged Identity Management a bérlő nem érhető el

Az Azure AD Privileged Identity Management már nem lesz elérhető az Ön bérelt szolgáltatásának ha:
- A szervezet Azure AD Privileged Identity Management használt Preview történt és nem vásárol Azure AD Premium P2-előfizetés vagy az EMS E5 előfizetés esetén.
- A szervezet kellett az Azure AD Premium P2 vagy az EMS E5 próbaverzió lejárt.
- A szervezet kellett a megvásárolt előfizetése lejárt.

Ha az Azure AD Premium P2 előfizetés vagy az EMS E5 előfizetés lejár, vagy egy olyan szervezet, amely a képen használta az Azure AD Privileged Identity Management nem kap Azure AD Premium P2- vagy EMS E5 előfizetés:

- Állandó szerepkör-hozzárendelések tooAzure AD szerepkörök nem érinti.
- hello hello Azure-portálon az Azure AD Privileged Identity Management bővítmény, valamint a hello Graph API-parancsmagok és az Azure AD Privileged Identity Management PowerShell felületei fogja már nem kell a felhasználók tooactivate kiemelt szerepköröket, kezelése emelt szintű hozzáférés, vagy hajtsa végre a hozzáférési felülvizsgálatát kiemelt szerepköröket.
- Az Azure AD-szerepkörök jogosult szerepkör hozzárendelése el lesz távolítva, felhasználók már nem lesz képes tooactivate kiemelt szerepköröket.
- Minden folyamatban lévő hozzáférés felülvizsgálatra az Azure AD-szerepkörök véget ér, és az Azure AD Privileged Identity Management konfigurációs beállításait.
- Az Azure AD Privileged Identity Management a szerepkör-hozzárendelési módosítások már nem küld e-maileket.

## <a name="next-steps"></a>Következő lépések

- [Ismerkedés az Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-getting-started.md)
- [Szerepkörök az Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-roles.md)
