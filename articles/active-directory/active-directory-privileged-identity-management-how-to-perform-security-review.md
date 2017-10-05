---
title: "Hogyan hajthat végre egy áttekintése |} Microsoft Docs"
description: "Útmutató az Azure Privileged Identity Management alkalmazással felülvizsgálat végrehajtása."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 49ee2feb-7d2e-4acf-82c1-40ff23062862
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: a98ed60221eeba1d9c92df846aeae2deafb8ae60
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-perform-an-access-review-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="b5935-103">Az Azure AD Privileged Identity Management egy hozzáférési felülvizsgálat végrehajtása</span><span class="sxs-lookup"><span data-stu-id="b5935-103">How to perform an access review in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="b5935-104">Az Azure Active Directory (AD) Privileged Identity Management egyszerűbbé teszi a hogyan kezelhetik a vállalatok számára az erőforrások az Azure AD és más Microsoft online szolgáltatások, például az Office 365-öt vagy a Microsoft Intune privilegizált hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="b5935-104">Azure Active Directory (AD) Privileged Identity Management simplifies how enterprises manage privileged access to resources in Azure AD and other Microsoft online services like Office 365 or Microsoft Intune.</span></span>  

<span data-ttu-id="b5935-105">Ha egy rendszergazdai szerepkör van rendelve, a szervezete kiemelt szerepkörű rendszergazda megkérheti, hogy rendszeresen győződjön meg arról, hogy továbbra is szerepkörre van szüksége, hogy a feladat.</span><span class="sxs-lookup"><span data-stu-id="b5935-105">If you are assigned to an administrative role, your organization's privileged role administrator may ask you to regularly confirm that you still need that role for your job.</span></span> <span data-ttu-id="b5935-106">Előfordulhat, hogy kap egy e-mailt, amely tartalmaz egy hivatkozást, vagy elvégezheti a rögtön a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b5935-106">You might get an email that includes a link, or you can go straight to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="b5935-107">Kövesse a cikkben egy önálló tekintse át a hozzárendelt szerepkörök végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="b5935-107">Follow the steps in this article to perform a self-review of your assigned roles.</span></span>

<span data-ttu-id="b5935-108">Ha egy kiemelt szerepkörű rendszergazda hozzáférést értékelést iránt érdeklődik, ezzel további adatokat, [egy hozzáférési felülvizsgálat indítása](active-directory-privileged-identity-management-how-to-start-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="b5935-108">If you're a privileged role administrator interested in access reviews, get more details at [How to start an access review](active-directory-privileged-identity-management-how-to-start-security-review.md).</span></span>

## <a name="add-the-privileged-identity-management-application"></a><span data-ttu-id="b5935-109">A Privileged Identity Management alkalmazás felvétele</span><span class="sxs-lookup"><span data-stu-id="b5935-109">Add the Privileged Identity Management application</span></span>
<span data-ttu-id="b5935-110">Az Azure AD Privileged Identity Management (PIM) alkalmazást is használhatja a [Azure-portálon](https://portal.azure.com/) a felülvizsgálat végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="b5935-110">You can use the Azure AD Privileged Identity Management (PIM) application in the [Azure portal](https://portal.azure.com/) to perform your review.</span></span>  <span data-ttu-id="b5935-111">Ha az Azure AD Privileged Identity Management alkalmazás nem rendelkezik a portálon, kövesse az alábbi lépéseket a kezdéshez.</span><span class="sxs-lookup"><span data-stu-id="b5935-111">If you don't have the Azure AD Privileged Identity Management application on your portal, follow these steps to get started.</span></span>

1. <span data-ttu-id="b5935-112">Jelentkezzen be az [Azure Portalra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b5935-112">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="b5935-113">Jelölje be az Azure-portálon, és válassza ki a könyvtárat, ahol meg fog jobb felső sarokban a felhasználónevére működik.</span><span class="sxs-lookup"><span data-stu-id="b5935-113">Select your username in the upper right-hand corner of the Azure portal, and select the directory where you will you be operating.</span></span>
3. <span data-ttu-id="b5935-114">Válassza a **További szolgáltatások** lehetőséget, és a Szűrők szövegmezővel keresse meg az **Azure AD Privileged Identity Management** alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b5935-114">Select **More services** and use the Filter textbox to search for **Azure AD Privileged Identity Management**.</span></span>
4. <span data-ttu-id="b5935-115">Jelölje be a **Rögzítés az irányítópulton** jelölőnégyzetet, majd kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b5935-115">Check **Pin to dashboard** and then click **Create**.</span></span> <span data-ttu-id="b5935-116">Megnyílik a Privileged Identity Management alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b5935-116">The Privileged Identity Management application will open.</span></span>

## <a name="approve-or-deny-access"></a><span data-ttu-id="b5935-117">Hagyja jóvá vagy nem engedélyezik a hozzáférést</span><span class="sxs-lookup"><span data-stu-id="b5935-117">Approve or deny access</span></span>
<span data-ttu-id="b5935-118">Hagyja jóvá vagy nem engedélyezik a hozzáférést, akkor még csak szólítja fel a felülvizsgáló e továbbra is használhatja ezt a szerepkört vagy nem.</span><span class="sxs-lookup"><span data-stu-id="b5935-118">When you approve or deny access, you're just telling the reviewer whether you still use this role or not.</span></span> <span data-ttu-id="b5935-119">Válasszon **jóváhagyás** Ha azt szeretné, hogy a szerepkör belül vagy **Megtagadás** Ha már nincs szüksége a hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="b5935-119">Choose **Approve** if you want to stay in the role, or **Deny** if you don't need the access anymore.</span></span> <span data-ttu-id="b5935-120">Az állapota nem azonnal, megváltoztatni, amíg meg nem a felülvizsgáló vonatkozik az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="b5935-120">Your status won't change right away, until the reviewer applies the results.</span></span>
<span data-ttu-id="b5935-121">Kövesse az alábbi lépéseket található, majd fejezze be a áttekintése:</span><span class="sxs-lookup"><span data-stu-id="b5935-121">Follow these steps to find and complete the access review:</span></span>

1. <span data-ttu-id="b5935-122">Válassza ki a PIM alkalmazást **felülvizsgálati emelt szintű hozzáférés**.</span><span class="sxs-lookup"><span data-stu-id="b5935-122">In the PIM application, select **Review privileged access**.</span></span> <span data-ttu-id="b5935-123">Ha minden függőben lévő hozzáférés felülvizsgálatra, azok megjelennek az Azure AD hozzáférési értékelést panel.</span><span class="sxs-lookup"><span data-stu-id="b5935-123">If you have any pending access reviews, they appear in the Azure AD Access reviews blade.</span></span>
2. <span data-ttu-id="b5935-124">Válassza ki a végrehajtani kívánt áttekintése.</span><span class="sxs-lookup"><span data-stu-id="b5935-124">Select the review you want to complete.</span></span>
3. <span data-ttu-id="b5935-125">Kivéve, ha a felülvizsgálati hozott létre, akkor jelennek meg az egyetlen felhasználó a felülvizsgálat alatt.</span><span class="sxs-lookup"><span data-stu-id="b5935-125">Unless you created the review, you appear as the only user in the review.</span></span> <span data-ttu-id="b5935-126">Válassza ki a pipa jelre a neve mellett.</span><span class="sxs-lookup"><span data-stu-id="b5935-126">Select the check mark next to your name.</span></span>
4. <span data-ttu-id="b5935-127">Válasszon **jóváhagyása** vagy **megtagadása**.</span><span class="sxs-lookup"><span data-stu-id="b5935-127">Choose either **Approve** or **Deny**.</span></span> <span data-ttu-id="b5935-128">Szükség lehet a döntés okát közé tartozik a **okot** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="b5935-128">You may need to include a reason for your decision in the **Provide a reason** text box.</span></span>  
5. <span data-ttu-id="b5935-129">Zárja be a **tekintse át az Azure AD-szerepkörök** panelen.</span><span class="sxs-lookup"><span data-stu-id="b5935-129">Close the **Review Azure AD roles** blade.</span></span>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="b5935-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b5935-130">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
