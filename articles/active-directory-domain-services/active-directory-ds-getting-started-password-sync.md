---
title: "Active Directory Domain Services: Jelszavak szinkronizálásának engedélyezése | Microsoft Docs"
description: "Első lépések az Azure Active Directory tartományi szolgáltatások használatával"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 5a32a0df-a3ca-4ebe-b980-91f58f8030fc
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/30/2017
ms.author: maheshu
ms.openlocfilehash: 8e073df9de2996f5ad159dda746881c014ea3d66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-password-synchronization-tooazure-active-directory-domain-services"></a>Jelszó-szinkronizálás tooAzure Active Directory tartományi szolgáltatások engedélyezése
Az előző feladatokban engedélyezte az Active Directory Domain Servicest az Azure Active Directory (Azure AD) bérlő számára. hello következő feladata NT LAN Manager-(NTLM-) és a Kerberos-hitelesítés tooAzure AD tartományi szolgáltatások a hitelesítő kivonatok tooenable szinkronizálás szükséges. Miután beállította a hitelesítő adatok szinkronizálása, felhasználók bejelentkezhetnek toohello által kezelt tartomány a vállalati hitelesítő adataikkal.

hello lépései eltérőek csak felhőalapú felhasználói fiókok és felhasználói fiókok, amelyek alapján az Azure AD Connect használatával a helyszíni címtárral szinkronizálja.  Ha az Azure AD-bérlő rendelkezik a felhő csak a felhasználók és a felhasználó számára a helyszíni Active Directory, tooperform két lépést kell.

<br>

> [!div class="op_single_selector"]
> * **Csak felhőalapú felhasználói fiókokhoz**: [szinkronizálja a jelszavakat, csak felhőalapú felhasználói fiókok tooyour felügyelt tartomány számára](active-directory-ds-getting-started-password-sync.md)
> * **A helyszíni felhasználói fiókok**: [szinkronizálja a jelszavakat a felhasználói fiókok szinkronizálása a helyszíni AD tooyour felügyelt tartományhoz](active-directory-ds-getting-started-password-sync-synced-tenant.md)
>
>

<br>

## <a name="task-5-enable-password-synchronization-tooyour-managed-domain-for-cloud-only-user-accounts"></a>5. feladat: jelszó szinkronizálási tooyour által kezelt tartomány csak felhőalapú felhasználói fiókokhoz engedélyezése
hello tooauthenticate felhasználók felügyelt tartomány, Azure Active Directory tartományi szolgáltatások kell hitelesítőadat-kivonatok formátuma nem megfelelő az NTLM és Kerberos hitelesítéshez. Az Azure AD nem hozza létre vagy hello formátum, amely az NTLM vagy Kerberos hitelesítéshez, amíg nem engedélyezte az Azure Active Directory tartományi szolgáltatások a bérlő számára szükséges hitelesítő kivonatokat tárolja. Nyilvánvaló biztonsági okokból az Azure AD nem tiszta szöveges formátumban tárolja a jelszóalapú hitelesítő adatokat. Az Azure AD ezért nincs szükség a felhasználók meglévő hitelesítő adatok alapján ezek NTLM vagy Kerberos hitelesítő kivonatok készítése módon tooautomatically.

> [!NOTE]
> Ha a szervezet csak felhőalapú felhasználói fiókokhoz, a felhasználók, akik toouse Azure Active Directory tartományi szolgáltatások módosítania kell a jelszavukat. A felhőalapú felhasználói fiók pedig egy fiókot, amelyet az Azure AD-címtár hello Azure-portálon vagy az Azure AD PowerShell-parancsmagok használatával hozták létre. Az ilyen felhasználói fiókok nem a helyszíni címtárból szinkronizálódnak.
>
>

A jelszómódosítási folyamat eredményeképp hello hitelesítő kivonatok Azure Active Directory tartományi szolgáltatások az Azure AD-ben létrehozott Kerberos és NTLM-hitelesítés toobe szükséges. Akkor is vagy elévülése hello jelszavak hello lévő összes felhasználó számára bérlői, akik kell toouse Azure Active Directory tartományi szolgáltatások, vagy utasíthatja őket toochange jelszavukat.

### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-user-account"></a>Kerberos és NTLM hitelesítőadat-kivonatok létrehozásának engedélyezése egy kizárólag felhőalapú felhasználói fiókon
Az alábbiakban szüksége tooprovide felhasználók módosíthatják a jelszavukat, hello utasításokat:

1. Nyissa meg toohello [Azure AD hozzáférési Panel](http://myapps.microsoft.com) lap a szervezet számára.

    ![Indítsa el a hello Azure AD hozzáférési panel](./media/active-directory-domain-services-getting-started/access-panel.png)

2. A hello jobb felső sarokban, kattintson a a nevét, és válassza **profil** hello menüből.

    ![Profil kiválasztása](./media/active-directory-domain-services-getting-started/select-profile.png)

3. A hello **profil** lapján kattintson a **jelszó módosítása**.

    ![Kattintson a „Jelszó módosítása” lehetőségre](./media/active-directory-domain-services-getting-started/user-change-password.png)

   > [!NOTE]
   > Ha hello **jelszó módosítása** hello hozzáférési Panel ablakban nem jelenik meg a beállítást, győződjön meg arról, hogy van-e beállítva a szervezet [az Azure AD-jelszókezelés](../active-directory/active-directory-passwords-getting-started.md).
   >
   >
4. A hello **jelszómódosítás** lapon írja be a létező (régi) jelszavát, adjon meg egy új jelszót, és erősítse meg.

    ![Hozzon létre virtuális hálózatot az Azure AD tartományi szolgáltatásokhoz.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

5. Kattintson a **Submit** (Küldés) gombra.

Miután megváltoztatta a jelszavát, néhány perc múlva használható az Azure Active Directory tartományi szolgáltatásokban kell hello új jelszót. Után néhány percet (általában körülbelül 20 perc), bármikor beléphet toocomputers, amelyek összeillesztett toohello által kezelt tartomány hello újonnan módosított jelszó használatával.

## <a name="related-content"></a>Kapcsolódó tartalom
* [Hogyan tooupdate a saját jelszavát](../active-directory/active-directory-passwords-update-your-own-password.md)
* [A jelszókezelés első lépései az Azure AD-ben](../active-directory/active-directory-passwords-getting-started.md)
* [Jelszó-szinkronizálás engedélyezése tooAzure Active Directory tartományi szolgáltatások egy szinkronizált Azure ad-bérlőben](active-directory-ds-getting-started-password-sync-synced-tenant.md)
* [Az Azure Active Directory tartományi szolgáltatások által felügyelt tartományok adminisztrációja](active-directory-ds-admin-guide-administer-domain.md)
* [Csatlakozás a Windows virtuális gép tooan Azure Active Directory tartományi szolgáltatások által felügyelt tartományhoz](active-directory-ds-admin-guide-join-windows-vm.md)
* [Csatlakozás Red Hat Enterprise Linux virtuális gép tooan Azure Active Directory tartományi szolgáltatások által felügyelt tartományhoz](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
