---
title: "Felhasználók hozzárendelése egyéni tartományhoz az Azure Active Directoryban |} Microsoft Docs"
description: "Hogyan tölthető fel a felhasználói fiókok Azure Active Directoryban egyéni tartományt."
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
ms.openlocfilehash: 39cb54a6637088c35c6aef864a804c24803f48ba
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="assign-users-to-a-custom-domain"></a><span data-ttu-id="cba59-103">Felhasználók hozzárendelése egyéni tartományhoz</span><span class="sxs-lookup"><span data-stu-id="cba59-103">Assign users to a custom domain</span></span>
<span data-ttu-id="cba59-104">Miután hozzáadta az egyéni tartomány az Azure Active Directoryhoz, hozzá kell adnia a felhasználói fiókokat a tartományhoz, hogy elkezdheti hitelesítése őket.</span><span class="sxs-lookup"><span data-stu-id="cba59-104">After you have added your custom domain to Azure Active Directory, you must add the user accounts for this domain so that you can begin authenticating them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cba59-105">A Microsoft javasolja, hogy az Azure Portalon található [Azure AD felügyeleti központból](https://aad.portal.azure.com) kezelje az Azure AD-t az ebben a cikkben javasolt klasszikus Azure portál helyett.</span><span class="sxs-lookup"><span data-stu-id="cba59-105">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span> <span data-ttu-id="cba59-106">Az Azure AD felügyeleti központban a tartománynevek kezelése című történő [az Azure Active Directoryban egyéni tartománynevek kezelése](active-directory-domains-manage-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="cba59-106">For how to manage your domain names in the Azure AD admin center, see [Managing custom domain names in your Azure Active Directory](active-directory-domains-manage-azure-portal.md).</span></span>

## <a name="users-synced-from-a-on-premises-directory"></a><span data-ttu-id="cba59-107">Az egy helyszíni Directoryból szinkronizált felhasználók</span><span class="sxs-lookup"><span data-stu-id="cba59-107">Users synced from a on-premises directory</span></span>
<span data-ttu-id="cba59-108">Ha már meg van kapcsolat között a helyszíni Active Directory és az Azure Active Directory, a szinkronizálás feltöltheti a fiókokat.</span><span class="sxs-lookup"><span data-stu-id="cba59-108">If you have already set up a connection between your on-premises Active Directory and Azure Active Directory, synchronization can populate the accounts.</span></span> <span data-ttu-id="cba59-109">Az Azure Active Directory szinkronizálása a helyszíni Active Directoryban további információkért lásd: [a helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="cba59-109">For more information on how to synchronize Azure Active Directory with your on-premises Active Directory, see [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

## <a name="users-added-and-managed-in-the-cloud"></a><span data-ttu-id="cba59-110">Felhasználók hozzáadása és kezelése a felhőben</span><span class="sxs-lookup"><span data-stu-id="cba59-110">Users added and managed in the cloud</span></span>
<span data-ttu-id="cba59-111">A tartomány egy olyan meglévő felhasználói fiók módosítása:</span><span class="sxs-lookup"><span data-stu-id="cba59-111">To change the domain for an existing user account:</span></span>

1. <span data-ttu-id="cba59-112">Nyissa meg a klasszikus Azure portálra egy olyan fiókkal, amely globális rendszergazda vagy a felhasználó rendszergazda segítségét.</span><span class="sxs-lookup"><span data-stu-id="cba59-112">Open the Azure classic portal using an account that is a global admin or a user admin.</span></span>
2. <span data-ttu-id="cba59-113">Nyissa meg a címtárat.</span><span class="sxs-lookup"><span data-stu-id="cba59-113">Open your directory.</span></span>
3. <span data-ttu-id="cba59-114">Válassza a **Felhasználók** lapot.</span><span class="sxs-lookup"><span data-stu-id="cba59-114">Select the **Users** tab.</span></span>
4. <span data-ttu-id="cba59-115">Válassza ki a felhasználót a listáról.</span><span class="sxs-lookup"><span data-stu-id="cba59-115">Select the user from the list.</span></span>
5. <span data-ttu-id="cba59-116">A felhasználó a tartományt, majd válassza ki **mentése**.</span><span class="sxs-lookup"><span data-stu-id="cba59-116">Change the domain for the user, and then select **Save**.</span></span>

<span data-ttu-id="cba59-117">Ez is végezhető használatával [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) vagy a [Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).</span><span class="sxs-lookup"><span data-stu-id="cba59-117">This can also be done using [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) or the [Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).</span></span>

## <a name="select-a-custom-domain-when-creating-a-new-user"></a><span data-ttu-id="cba59-118">Jelöljön ki egy egyéni tartományt, amikor új felhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="cba59-118">Select a custom domain when creating a new user</span></span>
1. <span data-ttu-id="cba59-119">Nyissa meg a klasszikus Azure portálra egy olyan fiókkal, amely globális rendszergazda vagy a felhasználó rendszergazda segítségét.</span><span class="sxs-lookup"><span data-stu-id="cba59-119">Open the Azure classic portal using an account that is a global admin or a user admin.</span></span>
2. <span data-ttu-id="cba59-120">Nyissa meg a címtárat.</span><span class="sxs-lookup"><span data-stu-id="cba59-120">Open your directory.</span></span>
3. <span data-ttu-id="cba59-121">Válassza a **Felhasználók** lapot.</span><span class="sxs-lookup"><span data-stu-id="cba59-121">Select the **Users** tab.</span></span>
4. <span data-ttu-id="cba59-122">A parancssávon válassza **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="cba59-122">In the command bar, select **Add**.</span></span>
5. <span data-ttu-id="cba59-123">Amikor a felhasználó nevét, a tartomány listából válassza ki az egyéni tartomány.</span><span class="sxs-lookup"><span data-stu-id="cba59-123">When you add the user name, choose the custom domain from the domain list.</span></span>
6. <span data-ttu-id="cba59-124">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="cba59-124">Select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cba59-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cba59-125">Next steps</span></span>
* [<span data-ttu-id="cba59-126">Egyéni tartománynevek használatával egyszerűbbé teheti a felhasználói bejelentkezési élmény</span><span class="sxs-lookup"><span data-stu-id="cba59-126">Using custom domain names to simplify the sign-in experience for your users</span></span>](active-directory-add-domain.md)
* [<span data-ttu-id="cba59-127">Egyéni tartománynevek kezelése</span><span class="sxs-lookup"><span data-stu-id="cba59-127">Manage custom domain names</span></span>](active-directory-add-manage-domain-names.md)
* [<span data-ttu-id="cba59-128">Ismerkedés az Azure AD tartománykezelési fogalmaival</span><span class="sxs-lookup"><span data-stu-id="cba59-128">Learn about domain management concepts in Azure AD</span></span>](active-directory-add-domain-concepts.md)

