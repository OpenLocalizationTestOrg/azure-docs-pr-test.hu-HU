---
title: "aaaGet lépések az Azure MFA hello felhőben |} Microsoft Docs"
description: "Ez a hello Microsoft Azure multi-factor Authentication hitelesítés lap, amely leírja, hogyan tooget el az Azure MFA felhőben hello."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 6b2e6549-1a26-4666-9c4a-cbe5d64c4e66
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/24/2017
ms.author: kgremban
ms.openlocfilehash: a4aaf44bf08d96f2baad155072fdd6e0e727ce8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-multi-factor-authentication-in-hello-cloud"></a>Ismerkedés az Azure multi-factor Authentication hello felhőben
Ez a cikk végigvezeti a tooget indítási módjának hello felhőalapú Azure multi-factor Authentication használatát.

> [!NOTE]
> hello következő dokumentációja bemutatja, hogyan tooenable felhasználók hello **klasszikus Azure portál**. Ha a keresett információk tooset be Azure multi-factor Authentication a felhasználók Office 365, lásd: [az Office 365 többtényezős hitelesítés beállítása.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)

![A felhő hello MFA](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## <a name="prerequisite"></a>Előfeltétel
[Regisztráljon egy Azure-előfizetés](https://azure.microsoft.com/pricing/free-trial/) -még nem rendelkezik Azure-előfizetéssel, ha egy toosign felfelé kell. Ha csak most kezdi és az Azure MFA-t használja, használhat próbaelőfizetést

## <a name="enable-azure-multi-factor-authentication"></a>Az Azure Multi-Factor Authentication engedélyezése
Mindaddig, amíg a felhasználók rendelkeznek licenccel, amely tartalmazza az Azure multi-factor Authentication, nincs szükség, hogy kell-e az Azure MFA toodo tooturn. Felhasználói alapon, egyenként kezdeményezheti a kétlépéses ellenőrzés igénylését. hello olyan licencet, amely engedélyezi az Azure MFA Használatát a következők:
- Azure Multi-Factor Authentication
- Prémium szintű Azure Active Directory
- Enterprise Mobility + Security

Ha még nincs fiókja, ezek három licencet, vagy nem rendelkezik elegendő licencek toocover minden felhasználó, közül túl ok. Csak akkor toodo egy további lépést és [a többtényezős hitelesítésszolgáltató létrehozása](multi-factor-authentication-get-started-auth-provider.md) a címtárban.

## <a name="turn-on-two-step-verification-for-users"></a>A kétlépéses ellenőrzés bekapcsolása a felhasználók számára

A felsorolt hello eljárásokkal [hogyan toorequire kétlépéses ellenőrzés egy felhasználó vagy csoport](multi-factor-authentication-get-started-user-states.md) toostart az Azure MFA használatával. Kiválaszthatja a összes bejelentkezések tooenforce kétlépéses ellenőrzést, vagy csak akkor, ha akkor számít, tooyou hozhat létre feltételes hozzáférési házirendek toorequire kétlépéses ellenőrzést.

## <a name="next-steps"></a>Következő lépések
Most, hogy állított be Azure multi-factor Authentication hello felhőben, konfigurálása, és állítsa be a központi telepítés. A részletekről lásd: [Az Azure Multi-Factor Authentication konfigurálása](multi-factor-authentication-whats-next.md).

