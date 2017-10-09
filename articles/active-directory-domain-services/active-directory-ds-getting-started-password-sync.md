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
# <a name="enable-password-synchronization-tooazure-active-directory-domain-services"></a><span data-ttu-id="d4600-103">Jelszó-szinkronizálás tooAzure Active Directory tartományi szolgáltatások engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d4600-103">Enable password synchronization tooAzure Active Directory Domain Services</span></span>
<span data-ttu-id="d4600-104">Az előző feladatokban engedélyezte az Active Directory Domain Servicest az Azure Active Directory (Azure AD) bérlő számára.</span><span class="sxs-lookup"><span data-stu-id="d4600-104">In preceding tasks, you enabled Azure Active Directory Domain Services for your Azure Active Directory (Azure AD) tenant.</span></span> <span data-ttu-id="d4600-105">hello következő feladata NT LAN Manager-(NTLM-) és a Kerberos-hitelesítés tooAzure AD tartományi szolgáltatások a hitelesítő kivonatok tooenable szinkronizálás szükséges.</span><span class="sxs-lookup"><span data-stu-id="d4600-105">hello next task is tooenable synchronization of credential hashes required for NT LAN Manager (NTLM) and Kerberos authentication tooAzure AD Domain Services.</span></span> <span data-ttu-id="d4600-106">Miután beállította a hitelesítő adatok szinkronizálása, felhasználók bejelentkezhetnek toohello által kezelt tartomány a vállalati hitelesítő adataikkal.</span><span class="sxs-lookup"><span data-stu-id="d4600-106">After you've set up credential synchronization, users can sign in toohello managed domain with their corporate credentials.</span></span>

<span data-ttu-id="d4600-107">hello lépései eltérőek csak felhőalapú felhasználói fiókok és felhasználói fiókok, amelyek alapján az Azure AD Connect használatával a helyszíni címtárral szinkronizálja.</span><span class="sxs-lookup"><span data-stu-id="d4600-107">hello steps involved are different for cloud-only user accounts vs user accounts that are synchronized from your on-premises directory using Azure AD Connect.</span></span>  <span data-ttu-id="d4600-108">Ha az Azure AD-bérlő rendelkezik a felhő csak a felhasználók és a felhasználó számára a helyszíni Active Directory, tooperform két lépést kell.</span><span class="sxs-lookup"><span data-stu-id="d4600-108">If your Azure AD tenant has a combination of cloud only users and users from your on-premises AD, you need tooperform both steps.</span></span>

<br>

> [!div class="op_single_selector"]
> * <span data-ttu-id="d4600-109">**Csak felhőalapú felhasználói fiókokhoz**: [szinkronizálja a jelszavakat, csak felhőalapú felhasználói fiókok tooyour felügyelt tartomány számára](active-directory-ds-getting-started-password-sync.md)</span><span class="sxs-lookup"><span data-stu-id="d4600-109">**Cloud-only user accounts**: [Synchronize passwords for cloud-only user accounts tooyour managed domain](active-directory-ds-getting-started-password-sync.md)</span></span>
> * <span data-ttu-id="d4600-110">**A helyszíni felhasználói fiókok**: [szinkronizálja a jelszavakat a felhasználói fiókok szinkronizálása a helyszíni AD tooyour felügyelt tartományhoz](active-directory-ds-getting-started-password-sync-synced-tenant.md)</span><span class="sxs-lookup"><span data-stu-id="d4600-110">**On-premises user accounts**: [Synchronize passwords for user accounts synced from your on-premises AD tooyour managed domain](active-directory-ds-getting-started-password-sync-synced-tenant.md)</span></span>
>
>

<br>

## <a name="task-5-enable-password-synchronization-tooyour-managed-domain-for-cloud-only-user-accounts"></a><span data-ttu-id="d4600-111">5. feladat: jelszó szinkronizálási tooyour által kezelt tartomány csak felhőalapú felhasználói fiókokhoz engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d4600-111">Task 5: enable password synchronization tooyour managed domain for cloud-only user accounts</span></span>
<span data-ttu-id="d4600-112">hello tooauthenticate felhasználók felügyelt tartomány, Azure Active Directory tartományi szolgáltatások kell hitelesítőadat-kivonatok formátuma nem megfelelő az NTLM és Kerberos hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="d4600-112">tooauthenticate users on hello managed domain, Azure Active Directory Domain Services needs credential hashes in a format that's suitable for NTLM and Kerberos authentication.</span></span> <span data-ttu-id="d4600-113">Az Azure AD nem hozza létre vagy hello formátum, amely az NTLM vagy Kerberos hitelesítéshez, amíg nem engedélyezte az Azure Active Directory tartományi szolgáltatások a bérlő számára szükséges hitelesítő kivonatokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="d4600-113">Azure AD does not generate or store credential hashes in hello format that's required for NTLM or Kerberos authentication, until you enable Azure Active Directory Domain Services for your tenant.</span></span> <span data-ttu-id="d4600-114">Nyilvánvaló biztonsági okokból az Azure AD nem tiszta szöveges formátumban tárolja a jelszóalapú hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="d4600-114">For obvious security reasons, Azure AD also does not store any password credentials in clear-text form.</span></span> <span data-ttu-id="d4600-115">Az Azure AD ezért nincs szükség a felhasználók meglévő hitelesítő adatok alapján ezek NTLM vagy Kerberos hitelesítő kivonatok készítése módon tooautomatically.</span><span class="sxs-lookup"><span data-stu-id="d4600-115">Therefore, Azure AD does not have a way tooautomatically generate these NTLM or Kerberos credential hashes based on users' existing credentials.</span></span>

> [!NOTE]
> <span data-ttu-id="d4600-116">Ha a szervezet csak felhőalapú felhasználói fiókokhoz, a felhasználók, akik toouse Azure Active Directory tartományi szolgáltatások módosítania kell a jelszavukat.</span><span class="sxs-lookup"><span data-stu-id="d4600-116">If your organization has cloud-only user accounts, users who need toouse Azure Active Directory Domain Services must change their passwords.</span></span> <span data-ttu-id="d4600-117">A felhőalapú felhasználói fiók pedig egy fiókot, amelyet az Azure AD-címtár hello Azure-portálon vagy az Azure AD PowerShell-parancsmagok használatával hozták létre.</span><span class="sxs-lookup"><span data-stu-id="d4600-117">A cloud-only user account is an account that was created in your Azure AD directory using either hello Azure portal or Azure AD PowerShell cmdlets.</span></span> <span data-ttu-id="d4600-118">Az ilyen felhasználói fiókok nem a helyszíni címtárból szinkronizálódnak.</span><span class="sxs-lookup"><span data-stu-id="d4600-118">Such user accounts aren't synchronized from an on-premises directory.</span></span>
>
>

<span data-ttu-id="d4600-119">A jelszómódosítási folyamat eredményeképp hello hitelesítő kivonatok Azure Active Directory tartományi szolgáltatások az Azure AD-ben létrehozott Kerberos és NTLM-hitelesítés toobe szükséges.</span><span class="sxs-lookup"><span data-stu-id="d4600-119">This password change process causes hello credential hashes that are required by Azure Active Directory Domain Services for Kerberos and NTLM authentication toobe generated in Azure AD.</span></span> <span data-ttu-id="d4600-120">Akkor is vagy elévülése hello jelszavak hello lévő összes felhasználó számára bérlői, akik kell toouse Azure Active Directory tartományi szolgáltatások, vagy utasíthatja őket toochange jelszavukat.</span><span class="sxs-lookup"><span data-stu-id="d4600-120">You can either expire hello passwords for all users in hello tenant who need toouse Azure Active Directory Domain Services or instruct them toochange their passwords.</span></span>

### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-user-account"></a><span data-ttu-id="d4600-121">Kerberos és NTLM hitelesítőadat-kivonatok létrehozásának engedélyezése egy kizárólag felhőalapú felhasználói fiókon</span><span class="sxs-lookup"><span data-stu-id="d4600-121">Enable NTLM and Kerberos credential hash generation for a cloud-only user account</span></span>
<span data-ttu-id="d4600-122">Az alábbiakban szüksége tooprovide felhasználók módosíthatják a jelszavukat, hello utasításokat:</span><span class="sxs-lookup"><span data-stu-id="d4600-122">Here are hello instructions you need tooprovide users, so they can change their passwords:</span></span>

1. <span data-ttu-id="d4600-123">Nyissa meg toohello [Azure AD hozzáférési Panel](http://myapps.microsoft.com) lap a szervezet számára.</span><span class="sxs-lookup"><span data-stu-id="d4600-123">Go toohello [Azure AD Access Panel](http://myapps.microsoft.com) page for your organization.</span></span>

    ![Indítsa el a hello Azure AD hozzáférési panel](./media/active-directory-domain-services-getting-started/access-panel.png)

2. <span data-ttu-id="d4600-125">A hello jobb felső sarokban, kattintson a a nevét, és válassza **profil** hello menüből.</span><span class="sxs-lookup"><span data-stu-id="d4600-125">In hello top right corner, click on your name and select **Profile** from hello menu.</span></span>

    ![Profil kiválasztása](./media/active-directory-domain-services-getting-started/select-profile.png)

3. <span data-ttu-id="d4600-127">A hello **profil** lapján kattintson a **jelszó módosítása**.</span><span class="sxs-lookup"><span data-stu-id="d4600-127">On hello **Profile** page, click on **Change password**.</span></span>

    ![Kattintson a „Jelszó módosítása” lehetőségre](./media/active-directory-domain-services-getting-started/user-change-password.png)

   > [!NOTE]
   > <span data-ttu-id="d4600-129">Ha hello **jelszó módosítása** hello hozzáférési Panel ablakban nem jelenik meg a beállítást, győződjön meg arról, hogy van-e beállítva a szervezet [az Azure AD-jelszókezelés](../active-directory/active-directory-passwords-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="d4600-129">If hello **Change password** option is not displayed in hello Access Panel window, ensure that your organization has configured [password management in Azure AD](../active-directory/active-directory-passwords-getting-started.md).</span></span>
   >
   >
4. <span data-ttu-id="d4600-130">A hello **jelszómódosítás** lapon írja be a létező (régi) jelszavát, adjon meg egy új jelszót, és erősítse meg.</span><span class="sxs-lookup"><span data-stu-id="d4600-130">On hello **change password** page, type your existing (old) password, type a new password, and then confirm it.</span></span>

    ![Hozzon létre virtuális hálózatot az Azure AD tartományi szolgáltatásokhoz.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

5. <span data-ttu-id="d4600-132">Kattintson a **Submit** (Küldés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d4600-132">Click **submit**.</span></span>

<span data-ttu-id="d4600-133">Miután megváltoztatta a jelszavát, néhány perc múlva használható az Azure Active Directory tartományi szolgáltatásokban kell hello új jelszót.</span><span class="sxs-lookup"><span data-stu-id="d4600-133">A few minutes after you have changed your password, hello new password is usable in Azure Active Directory Domain Services.</span></span> <span data-ttu-id="d4600-134">Után néhány percet (általában körülbelül 20 perc), bármikor beléphet toocomputers, amelyek összeillesztett toohello által kezelt tartomány hello újonnan módosított jelszó használatával.</span><span class="sxs-lookup"><span data-stu-id="d4600-134">After a few more minutes (typically, about 20 minutes), you can sign in toocomputers that are joined toohello managed domain by using hello newly changed password.</span></span>

## <a name="related-content"></a><span data-ttu-id="d4600-135">Kapcsolódó tartalom</span><span class="sxs-lookup"><span data-stu-id="d4600-135">Related Content</span></span>
* [<span data-ttu-id="d4600-136">Hogyan tooupdate a saját jelszavát</span><span class="sxs-lookup"><span data-stu-id="d4600-136">How tooupdate your own password</span></span>](../active-directory/active-directory-passwords-update-your-own-password.md)
* [<span data-ttu-id="d4600-137">A jelszókezelés első lépései az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d4600-137">Getting started with Password Management in Azure AD</span></span>](../active-directory/active-directory-passwords-getting-started.md)
* [<span data-ttu-id="d4600-138">Jelszó-szinkronizálás engedélyezése tooAzure Active Directory tartományi szolgáltatások egy szinkronizált Azure ad-bérlőben</span><span class="sxs-lookup"><span data-stu-id="d4600-138">Enable password synchronization tooAzure Active Directory Domain Services for a synced Azure AD tenant</span></span>](active-directory-ds-getting-started-password-sync-synced-tenant.md)
* [<span data-ttu-id="d4600-139">Az Azure Active Directory tartományi szolgáltatások által felügyelt tartományok adminisztrációja</span><span class="sxs-lookup"><span data-stu-id="d4600-139">Administer an Azure Active Directory Domain Services-managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
* [<span data-ttu-id="d4600-140">Csatlakozás a Windows virtuális gép tooan Azure Active Directory tartományi szolgáltatások által felügyelt tartományhoz</span><span class="sxs-lookup"><span data-stu-id="d4600-140">Join a Windows virtual machine tooan Azure Active Directory Domain Services-managed domain</span></span>](active-directory-ds-admin-guide-join-windows-vm.md)
* [<span data-ttu-id="d4600-141">Csatlakozás Red Hat Enterprise Linux virtuális gép tooan Azure Active Directory tartományi szolgáltatások által felügyelt tartományhoz</span><span class="sxs-lookup"><span data-stu-id="d4600-141">Join a Red Hat Enterprise Linux virtual machine tooan Azure Active Directory Domain Services-managed domain</span></span>](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
