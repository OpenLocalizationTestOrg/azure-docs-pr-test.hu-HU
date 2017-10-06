---
title: "aaaAssign felhasználók tooa egyéni tartományt az Azure Active Directoryban |} Microsoft Docs"
description: "Hogyan toopopulate a felhasználói fiókok Azure Active Directoryban egyéni tartományt."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 717b5a7c-7bc3-4ab1-98b5-4740b53338fe
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 23c338a361a90fddf42d4df90db94c9774305886
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="assign-users-tooa-custom-domain"></a><span data-ttu-id="bf7a0-103">Rendelje hozzá a felhasználók tooa egyéni tartományt</span><span class="sxs-lookup"><span data-stu-id="bf7a0-103">Assign users tooa custom domain</span></span>
<span data-ttu-id="bf7a0-104">Miután hozzáadta az egyéni tartomány tooAzure Active Directory, hozzá kell adnia a hello felhasználói fiókokat a tartományhoz, így el lehet kezdeni az hitelesítése őket.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-104">After you have added your custom domain tooAzure Active Directory, you must add hello user accounts for this domain so that you can begin authenticating them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bf7a0-105">A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-105">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span> <span data-ttu-id="bf7a0-106">Hogyan toomanage a tartománynevek hello Azure AD felügyeleti központban, lásd: [az Azure Active Directoryban egyéni tartománynevek kezelése](active-directory-domains-manage-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="bf7a0-106">For how toomanage your domain names in hello Azure AD admin center, see [Managing custom domain names in your Azure Active Directory](active-directory-domains-manage-azure-portal.md).</span></span>

## <a name="users-synced-from-a-on-premises-directory"></a><span data-ttu-id="bf7a0-107">Az egy helyszíni Directoryból szinkronizált felhasználók</span><span class="sxs-lookup"><span data-stu-id="bf7a0-107">Users synced from a on-premises directory</span></span>
<span data-ttu-id="bf7a0-108">Ha már meg van kapcsolat között a helyszíni Active Directory és az Azure Active Directory, a szinkronizálás feltöltheti hello fiókok.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-108">If you have already set up a connection between your on-premises Active Directory and Azure Active Directory, synchronization can populate hello accounts.</span></span> <span data-ttu-id="bf7a0-109">További információk hogyan toosynchronize az Azure Active Directory a helyszíni Active Directoryban: [a helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="bf7a0-109">For more information on how toosynchronize Azure Active Directory with your on-premises Active Directory, see [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

## <a name="users-added-and-managed-in-hello-cloud"></a><span data-ttu-id="bf7a0-110">Felhasználók hozzáadása és kezelése hello felhőben</span><span class="sxs-lookup"><span data-stu-id="bf7a0-110">Users added and managed in hello cloud</span></span>
<span data-ttu-id="bf7a0-111">meglévő felhasználói fiók toochange hello tartományt:</span><span class="sxs-lookup"><span data-stu-id="bf7a0-111">toochange hello domain for an existing user account:</span></span>

1. <span data-ttu-id="bf7a0-112">Nyissa meg hello klasszikus Azure portál egy olyan fiókkal, amely globális rendszergazda vagy a felhasználó rendszergazda segítségét.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-112">Open hello Azure classic portal using an account that is a global admin or a user admin.</span></span>
2. <span data-ttu-id="bf7a0-113">Nyissa meg a címtárat.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-113">Open your directory.</span></span>
3. <span data-ttu-id="bf7a0-114">Jelölje be hello **felhasználók** fülre.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-114">Select hello **Users** tab.</span></span>
4. <span data-ttu-id="bf7a0-115">Válassza ki a hello felhasználói hello listából.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-115">Select hello user from hello list.</span></span>
5. <span data-ttu-id="bf7a0-116">Hello felhasználó hello tartományt, majd válassza ki **mentése**.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-116">Change hello domain for hello user, and then select **Save**.</span></span>

<span data-ttu-id="bf7a0-117">Ez is végezhető használatával [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) vagy hello [Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).</span><span class="sxs-lookup"><span data-stu-id="bf7a0-117">This can also be done using [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) or hello [Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).</span></span>

## <a name="select-a-custom-domain-when-creating-a-new-user"></a><span data-ttu-id="bf7a0-118">Jelöljön ki egy egyéni tartományt, amikor új felhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="bf7a0-118">Select a custom domain when creating a new user</span></span>
1. <span data-ttu-id="bf7a0-119">Nyissa meg hello klasszikus Azure portál egy olyan fiókkal, amely globális rendszergazda vagy a felhasználó rendszergazda segítségét.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-119">Open hello Azure classic portal using an account that is a global admin or a user admin.</span></span>
2. <span data-ttu-id="bf7a0-120">Nyissa meg a címtárat.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-120">Open your directory.</span></span>
3. <span data-ttu-id="bf7a0-121">Jelölje be hello **felhasználók** fülre.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-121">Select hello **Users** tab.</span></span>
4. <span data-ttu-id="bf7a0-122">Hello parancssávon válassza **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-122">In hello command bar, select **Add**.</span></span>
5. <span data-ttu-id="bf7a0-123">Hello felhasználónév hozzáadásakor hello tartomány listából válassza ki az egyéni tartomány hello.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-123">When you add hello user name, choose hello custom domain from hello domain list.</span></span>
6. <span data-ttu-id="bf7a0-124">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-124">Select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bf7a0-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bf7a0-125">Next steps</span></span>
* [<span data-ttu-id="bf7a0-126">Az egyéni tartomány nevét toosimplify hello bejelentkezés során tapasztal élmény segítségével a felhasználók számára</span><span class="sxs-lookup"><span data-stu-id="bf7a0-126">Using custom domain names toosimplify hello sign-in experience for your users</span></span>](active-directory-add-domain.md)
* [<span data-ttu-id="bf7a0-127">Egyéni tartománynevek kezelése</span><span class="sxs-lookup"><span data-stu-id="bf7a0-127">Manage custom domain names</span></span>](active-directory-add-manage-domain-names.md)
* [<span data-ttu-id="bf7a0-128">Ismerkedés az Azure AD tartománykezelési fogalmaival</span><span class="sxs-lookup"><span data-stu-id="bf7a0-128">Learn about domain management concepts in Azure AD</span></span>](active-directory-add-domain-concepts.md)

