---
title: "a személyes eszköz tooyour szervezet aaaJoin |} Microsoft Docs"
description: "Azt ismerteti, hogy a felhasználók regisztrálhatják-e a személyes Windows 10-es eszközök tootheir vállalati hálózaton, és a BYOD-forgatókönyvhöz a központi telepítés lépéseit ismerteti."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 9f3d38f5-1cfd-43d4-97da-4fed1255a1ff
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 979e2461dd9ad0438aa402a0a3223cc84c4c625c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="join-a-personal-device-tooyour-organization"></a><span data-ttu-id="bd2ee-103">Csatlakozás a személyes eszköz tooyour szervezet</span><span class="sxs-lookup"><span data-stu-id="bd2ee-103">Join a personal device tooyour organization</span></span>
## <a name="toojoin-a-windows-10-device-tooyour-organization"></a><span data-ttu-id="bd2ee-104">a Windows 10 toojoin eszköz tooyour szervezet</span><span class="sxs-lookup"><span data-stu-id="bd2ee-104">toojoin a Windows 10 device tooyour organization</span></span>
1. <span data-ttu-id="bd2ee-105">A hello **Start** menüjében válassza **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="bd2ee-105">From hello **Start** menu, select **Settings**.</span></span>
2. <span data-ttu-id="bd2ee-106">Válassza ki **fiókok**, és kattintson a **fiókja**.</span><span class="sxs-lookup"><span data-stu-id="bd2ee-106">Select **Accounts**, and then click **Your account**.</span></span>
3. <span data-ttu-id="bd2ee-107">Kattintson a **adja hozzá a munkahelyi vagy iskolai fiókjával**, majd írja be a szervezeti fiókjával.</span><span class="sxs-lookup"><span data-stu-id="bd2ee-107">Click **Add Work or School account**, and then type in your organizational account.</span></span>
4. <span data-ttu-id="bd2ee-108">Hello bejelentkezési oldal a szervezet számára, írja be a felhasználónevét és jelszavát, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="bd2ee-108">On hello sign-in page for your organization, enter your user name and password, and then click **OK**.</span></span>
5. <span data-ttu-id="bd2ee-109">Rendszer bekéri a multi-factor authentication kihívást.</span><span class="sxs-lookup"><span data-stu-id="bd2ee-109">You will be prompted for a multi-factor authentication challenge.</span></span> <span data-ttu-id="bd2ee-110">(A a kérdés lehet egy informatikai rendszergazda konfigurálni.)</span><span class="sxs-lookup"><span data-stu-id="bd2ee-110">(This challenge is configurable by an IT administrator.)</span></span>
6. <span data-ttu-id="bd2ee-111">Azure Active Directory (Azure AD) ellenőrzi, hogy hello eszköz igényli-e a mobileszköz-kezelési beléptetés.</span><span class="sxs-lookup"><span data-stu-id="bd2ee-111">Azure Active Directory (Azure AD) checks whether hello device requires mobile device management enrollment.</span></span>
7. <span data-ttu-id="bd2ee-112">A Windows hello eszköz regisztrálja hello munkahely címtárában az Azure AD, és regisztrálja a mobileszköz-kezelés, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="bd2ee-112">Windows registers hello device in hello organization’s directory in Azure AD and enrolls it in mobile device management, if appropriate.</span></span>
8. <span data-ttu-id="bd2ee-113">Ha egy felügyelt felhasználók Windows viszi toohello asztali hello automatikus bejelentkezés révén.</span><span class="sxs-lookup"><span data-stu-id="bd2ee-113">If you are a managed user, Windows takes you toohello desktop through hello automatic sign-in.</span></span>
9. <span data-ttu-id="bd2ee-114">Ha egy összevont felhasználók fogja venni a Windows-bejelentkezés tooa képernyő tooenter a hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="bd2ee-114">If you are a federated user, you will be taken tooa Windows sign-in screen tooenter your credentials.</span></span>

## <a name="additional-information"></a><span data-ttu-id="bd2ee-115">További információ</span><span class="sxs-lookup"><span data-stu-id="bd2ee-115">Additional information</span></span>
* [<span data-ttu-id="bd2ee-116">Windows 10 Enterprise hello: módon toouse eszközök munkára</span><span class="sxs-lookup"><span data-stu-id="bd2ee-116">Windows 10 for hello enterprise: Ways toouse devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="bd2ee-117">Felhőalapú képességek tooWindows 10 eszközöknek az Azure Active Directory csatlakozási kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="bd2ee-117">Extending cloud capabilities tooWindows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="bd2ee-118">Microsoft Passporton keresztül jelszó nélkül identitások hitelesítése</span><span class="sxs-lookup"><span data-stu-id="bd2ee-118">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)
* [<span data-ttu-id="bd2ee-119">További információk az Azure AD Join használati forgatókönyveiről</span><span class="sxs-lookup"><span data-stu-id="bd2ee-119">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="bd2ee-120">Csatlakozás a tartományhoz csatlakozó eszközök tooAzure AD-hez Windows 10-es környezetben</span><span class="sxs-lookup"><span data-stu-id="bd2ee-120">Connect domain-joined devices tooAzure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="bd2ee-121">Az Azure AD Join beállítása</span><span class="sxs-lookup"><span data-stu-id="bd2ee-121">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

