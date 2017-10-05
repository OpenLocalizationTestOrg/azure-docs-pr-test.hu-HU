---
title: "Személyes eszköz csatlakoztatása a szervezet |} Microsoft Docs"
description: "Azt ismerteti, hogy a felhasználók regisztrálhatják Windows 10 saját eszközét, hogy a vállalati hálózaton, és a BYOD-forgatókönyvhöz a központi telepítés lépéseit ismerteti."
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
ms.openlocfilehash: 9418365ea18b065551448742b21c8b17a1749fc8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="join-a-personal-device-to-your-organization"></a><span data-ttu-id="c20c7-103">Személyes eszköz csatlakoztatása a szervezet</span><span class="sxs-lookup"><span data-stu-id="c20c7-103">Join a personal device to your organization</span></span>
## <a name="to-join-a-windows-10-device-to-your-organization"></a><span data-ttu-id="c20c7-104">A szervezet egy Windows 10 rendszerű eszköz csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="c20c7-104">To join a Windows 10 device to your organization</span></span>
1. <span data-ttu-id="c20c7-105">Az a **Start** menüjében válassza **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="c20c7-105">From the **Start** menu, select **Settings**.</span></span>
2. <span data-ttu-id="c20c7-106">Válassza ki **fiókok**, és kattintson a **fiókja**.</span><span class="sxs-lookup"><span data-stu-id="c20c7-106">Select **Accounts**, and then click **Your account**.</span></span>
3. <span data-ttu-id="c20c7-107">Kattintson a **adja hozzá a munkahelyi vagy iskolai fiókjával**, majd írja be a szervezeti fiókjával.</span><span class="sxs-lookup"><span data-stu-id="c20c7-107">Click **Add Work or School account**, and then type in your organizational account.</span></span>
4. <span data-ttu-id="c20c7-108">A szervezet a bejelentkezési lapon adja meg a felhasználónevét és jelszavát, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="c20c7-108">On the sign-in page for your organization, enter your user name and password, and then click **OK**.</span></span>
5. <span data-ttu-id="c20c7-109">Rendszer bekéri a multi-factor authentication kihívást.</span><span class="sxs-lookup"><span data-stu-id="c20c7-109">You will be prompted for a multi-factor authentication challenge.</span></span> <span data-ttu-id="c20c7-110">(A a kérdés lehet egy informatikai rendszergazda konfigurálni.)</span><span class="sxs-lookup"><span data-stu-id="c20c7-110">(This challenge is configurable by an IT administrator.)</span></span>
6. <span data-ttu-id="c20c7-111">Azure Active Directory (Azure AD) ellenőrzi, hogy az eszköz igényli-e a mobileszköz-kezelési beléptetés.</span><span class="sxs-lookup"><span data-stu-id="c20c7-111">Azure Active Directory (Azure AD) checks whether the device requires mobile device management enrollment.</span></span>
7. <span data-ttu-id="c20c7-112">A Windows regisztrálja az eszközt a szervezet címtárához az Azure AD-ben, és regisztrálja a mobileszköz-kezelés, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="c20c7-112">Windows registers the device in the organization’s directory in Azure AD and enrolls it in mobile device management, if appropriate.</span></span>
8. <span data-ttu-id="c20c7-113">Ha egy felügyelt felhasználók Windows végigvezeti Önt az asztalhoz az automatikus bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="c20c7-113">If you are a managed user, Windows takes you to the desktop through the automatic sign-in.</span></span>
9. <span data-ttu-id="c20c7-114">Ha egy összevont felhasználó, akkor megnyílik a Windows bejelentkezési képernyő hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="c20c7-114">If you are a federated user, you will be taken to a Windows sign-in screen to enter your credentials.</span></span>

## <a name="additional-information"></a><span data-ttu-id="c20c7-115">További információ</span><span class="sxs-lookup"><span data-stu-id="c20c7-115">Additional information</span></span>
* [<span data-ttu-id="c20c7-116">Vállalati használatú Windows 10: Az eszközök munkahelyi célú használata</span><span class="sxs-lookup"><span data-stu-id="c20c7-116">Windows 10 for the enterprise: Ways to use devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="c20c7-117">A felhőalapú képességek kiterjesztése a Windows 10-eszközökre az Azure Active Directory Joinon keresztül</span><span class="sxs-lookup"><span data-stu-id="c20c7-117">Extending cloud capabilities to Windows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="c20c7-118">Microsoft Passporton keresztül jelszó nélkül identitások hitelesítése</span><span class="sxs-lookup"><span data-stu-id="c20c7-118">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)
* [<span data-ttu-id="c20c7-119">További információk az Azure AD Join használati forgatókönyveiről</span><span class="sxs-lookup"><span data-stu-id="c20c7-119">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="c20c7-120">Tartományhoz csatlakoztatott eszközök csatlakoztatása az Azure AD-hez Windows 10-es környezetben</span><span class="sxs-lookup"><span data-stu-id="c20c7-120">Connect domain-joined devices to Azure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="c20c7-121">Az Azure AD Join beállítása</span><span class="sxs-lookup"><span data-stu-id="c20c7-121">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

