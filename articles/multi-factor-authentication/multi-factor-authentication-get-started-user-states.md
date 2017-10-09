---
title: "Azure multi-factor Authentication felhasználói állapotok aaaMicrosoft"
description: "További tudnivalók az Azure MFA felhasználói állapotok."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 0b9fde23-2d36-45b3-950d-f88624a68fbd
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: cf5b977b09d09330b7b3bc668abd79e602d62015
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorequire-two-step-verification-for-a-user-or-group"></a>Hogyan toorequire kétlépéses ellenőrzés egy felhasználó vagy csoport

Kétféleképpen megkövetelő a kétlépéses ellenőrzést. hello első lehetőség van tooenable egyes felhasználók, az Azure multi-factor Authentication (MFA). Amikor a felhasználók külön-külön engedélyezve vannak, azokat mindig elvégzik a kétlépéses ellenőrzést (néhány kivétellel, például amikor bejelentkeznek a megbízható IP-címekről, vagy ha hello megjegyzett eszközök szolgáltatás engedélyezve van). hello második lehetőség tooset fel egy feltételes hozzáférési szabályzatot, amely szükséges a kétlépéses ellenőrzést, bizonyos feltételek esetén.

>[!TIP] 
>Válasszon egyet ezek módszerek toorequire kétlépéses ellenőrzést, mindkettő nem. A felhasználó engedélyezése az Azure MFA felülbírálja a feltételes hozzáférési házirendekben.

## <a name="which-option-is-right-for-you"></a>Melyik lehetőség az Ön számára legmegfelelőbb

**Azure többtényezős hitelesítés engedélyezése felhasználói állapotok módosításával** hello hagyományos megközelítés megkövetelő a kétlépéses ellenőrzést. Mindkét hello felhőben Azure MFA és az Azure MFA kiszolgáló működik. Minden hello, amely engedélyezi a felhasználóknál hello ugyanazt a felhasználói élményt, amely tooperform kétlépéses ellenőrzés minden alkalommal, amikor bejelentkeznek az. A feltételes hozzáférési szabályzatok, amelyek befolyásolhatják, hogy a felhasználó engedélyezése egy felhasználó felülbírálja. 

**Azure MFA engedélyezésével a feltételes hozzáférési házirenddel** egy olyan rugalmasabb megközelítés megkövetelő a kétlépéses ellenőrzést. Ugyanis csak működik az Azure MFA felhőben hello azonban, és a feltételes hozzáférés egy [az Azure Active Directory szolgáltatás fizetős](https://www.microsoft.com/cloud-platform/azure-active-directory-features). Toogroups, valamint az egyes felhasználókra vonatkozó feltételes hozzáférési házirendeket is létrehozhat. Magas kockázatú csoportok adható meg több korlátozás mint alacsony kockázat csoportok, vagy a kétlépéses ellenőrzés csak magas kockázatú felhőalkalmazások szükséges, és kihagyja a alacsony kockázat néhányat a meglévők közül. 

Első bejelentkezéskor a hello követelmények bekapcsolása után mindkét lehetőség kérni az Azure multi-factor Authentication hello felhasználók tooregister. Mindkét lehetőség is dolgozhat konfigurálható hello [Azure multi-factor Authentication beállításait](multi-factor-authentication-whats-next.md)

## <a name="enable-azure-mfa-by-changing-user-status"></a>Az Azure MFA engedélyezése a felhasználó állapotának módosítása

Azure multi-factor Authentication felhasználói fiókok rendelkeznek a következő három jól elkülöníthető állapottal hello:

| status | Leírás | Érintett böngészőn kívüli alkalmazások |
|:---:|:---:|:---:|
| Letiltva |Új felhasználó alapértelmezett állapota hello nincs regisztrálva az Azure multi-factor Authentication (MFA). |Nem |
| Engedélyezve |hello felhasználó előfizetett a Azure MFA, de nincs regisztrálva. Amikor legközelebb bejelentkeznek a fogja felszólító tooregister hello. |Nem.  Mindaddig toowork hello regisztrációs folyamat befejezéséig. |
| Kényszerítve |hello felhasználó előfizetett, és az Azure MFA hello regisztrációs folyamat befejeződött. |Igen.  Az alkalmazásokhoz alkalmazásjelszókat. |

A felhasználói állapot tükrözi, hogy egy rendszergazda regisztrálta őket az Azure MFA, és hogy azok befejeződött hello regisztrációs folyamat során.

Minden felhasználó indulnak *le van tiltva*. Amikor regisztrál az Azure MFA, azok állapotváltozások felhasználók *engedélyezett*. Ha az engedélyezett felhasználók jelentkezzen be, és végezze el a regisztrációs folyamat hello, az állapota megváltozik. túl*kényszerített*.  

### <a name="view-hello-status-for-a-user"></a>Egy felhasználó hello állapotának megtekintése

Használja a következő lépések tooaccess hello lapján, ahol megtekintheti és kezelheti a felhasználói állapotok hello:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) rendszergazdaként.
2. Nyissa meg túl**Azure Active Directory** > **felhasználók és csoportok** > **minden felhasználó**.
3. Válassza ki **a multi-factor Authentication**.
   ![Válassza ki a multi-factor Authentication](./media/multi-factor-authentication-get-started-user-states/selectmfa.png)
4. Hello felhasználói állapotok jeleníti meg, egy új lapon nyílik meg.
   ![a multi-factor authentication felhasználói állapota – képernyőkép](./media/multi-factor-authentication-get-started-user-states/userstate1.png)

### <a name="change-hello-status-for-a-user"></a>Egy felhasználó hello állapotának módosítása

1. Hello lépéseket tooget toohello a multi-factor authentication a felhasználók lapon megelőző használja.
2. Keresés hello felhasználói tooenable szeretné az Azure MFA számára. Előfordulhat, hogy toochange hello nézet hello tetején. 
   ![Keresse meg azt a felhasználót – képernyőkép](./media/multi-factor-authentication-get-started-cloud/enable1.png)
3. Ellenőrizze a hello mezőben következő tootheir nevét.
4. A jobb oldali Gyorsműveletek a hello válassza **engedélyezése** vagy **letiltása**.
   ![Engedélyezi a kiválasztott felhasználó – képernyőkép](./media/multi-factor-authentication-get-started-cloud/user1.png)

   >[!TIP]
   >*Engedélyezett* automatikusan átváltani túl*kényszerített* amikor regisztrálják az Azure MFA számára. Hello felhasználói állapot tooenforced manuálisan nem szabad módosítani. 

5. Erősítse meg választását a hello előugró ablakban. 

Miután engedélyezte a felhasználókat, e-mailben értesítse őket. Mondja el neki, hogy azok kell majd adnia tooregister hello következő bejelentkezéskor. Is ha a szervezet a böngészőn kívüli alkalmazásokat, amelyek nem támogatják a modern hitelesítést használ, akkor kell toocreate alkalmazásjelszókat. A hivatkozás tooour is használható [Azure MFA végfelhasználói útmutató](./end-user/multi-factor-authentication-end-user.md) toohelp azokat a kezdéshez.

### <a name="use-powershell"></a>A PowerShell használata
toochange hello felhasználói állapot használatával [Azure AD PowerShell](/powershell/azure/overview), módosítsa `$st.State`. Három lehetséges állapota van:

* Engedélyezve
* Kényszerítve
* Letiltva  

Nem helyezhető át a felhasználók közvetlenül toohello *kényszerített* állapotát. A nem a webböngésző-alapú alkalmazások nem fog tovább működni, mert hello felhasználó rendelkezik keresztül MFA regisztrációs állapotba, és nem kapott egy [alkalmazásjelszót](multi-factor-authentication-whats-next.md#app-passwords). 

Jó választás PowerShell-lel akkor, ha a felhasználók toobulk van szüksége. Hozzon létre egy PowerShell-parancsfájlt, amely végighalad a felhasználók listában, és lehetővé teszik, hogy:

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Például:

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }

## <a name="enable-azure-mfa-with-a-conditional-access-policy"></a>Feltételes hozzáférési házirenddel Azure többtényezős hitelesítés engedélyezése

Feltételes hozzáférés lehetővé teszi az fizetős az Azure Active Directory, számos konfigurációs beállításokkal. Egyirányú toocreate házirend bízná ezeket a lépéseket. További információkért olvassa el [feltételes hozzáférés az Azure Active Directoryban](../active-directory/active-directory-conditional-access-azure-portal.md).

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) rendszergazdaként.
2. Nyissa meg túl**Azure Active Directory** > **feltételes hozzáférés**.
3. Válassza ki **új házirend**.
4. A **hozzárendelések**, jelölje be **felhasználók és csoportok**. Használjon hello **Include** és **kizárása** lapokon toospecify mely felhasználók és csoportok hello házirend fogja kezelni.
5. A **hozzárendelések**, jelölje be **felhőalapú alkalmazásokba**. Válassza ki a tooinclude **összes felhőalapú alkalmazások**.
6. A **hozzáférés-szabályozási**, jelölje be **Grant**. Válasszon **többtényezős hitelesítést**.
7. Kapcsolja be **házirend engedélyezése** túl**a** majd **mentése**.

hello hello feltételes hozzáférési házirendben egyéb beállítások lehetővé teszik toospecify pontosan, amikor a kétlépéses ellenőrzést kell. Például egy házirendet, amely szerint az teheti: alvállalkozói megpróbálják tooaccess a nem megbízható hálózatokhoz, amelyek nincsenek tartományhoz csatlakozó eszközök beszerzési alkalmazásából, szükséges a kétlépéses ellenőrzést. 

## <a name="next-steps"></a>Következő lépések

- Tippek hello [ajánlott eljárások a feltételes hozzáférés](../active-directory/active-directory-conditional-access-best-practices.md)

- A multi-factor Authentication szolgáltatás beállításainak kezelése [a felhasználók és az eszközeik](multi-factor-authentication-manage-users-and-devices.md)