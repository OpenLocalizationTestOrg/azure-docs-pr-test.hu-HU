---
title: "aaaHow tooperform egy áttekintése |} Microsoft Docs"
description: "Ismerje meg, hogyan tooperform a áttekintés a hello Azure Privileged Identity Management alkalmazás."
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
ms.openlocfilehash: 301a5e9f97b68fedfbf4954e0bd7dadb7f0fc510
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooperform-an-access-review-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="8dac1-103">Hogyan tooperform hozzáférés tekintse át az Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="8dac1-103">How tooperform an access review in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="8dac1-104">Az Azure Active Directory (AD) Privileged Identity Management egyszerűbbé teszi a hogyan kezelhetik a vállalatok számára az Azure AD privileged access tooresources és más Microsoft online szolgáltatások, például az Office 365-öt vagy a Microsoft Intune.</span><span class="sxs-lookup"><span data-stu-id="8dac1-104">Azure Active Directory (AD) Privileged Identity Management simplifies how enterprises manage privileged access tooresources in Azure AD and other Microsoft online services like Office 365 or Microsoft Intune.</span></span>  

<span data-ttu-id="8dac1-105">Ha tooan rendszergazdai szerepkör van hozzárendelve, a szervezet kiemelt szerepkörű rendszergazda megkérheti, tooregularly győződjön meg arról, hogy továbbra is szerepkörre van szüksége, hogy a feladat.</span><span class="sxs-lookup"><span data-stu-id="8dac1-105">If you are assigned tooan administrative role, your organization's privileged role administrator may ask you tooregularly confirm that you still need that role for your job.</span></span> <span data-ttu-id="8dac1-106">Előfordulhat, hogy kap egy e-mailt, amely tartalmaz egy hivatkozást, vagy elvégezheti a egyenes toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8dac1-106">You might get an email that includes a link, or you can go straight toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="8dac1-107">Kövesse a cikk tooperform egy önálló tekintse át a hozzárendelt szerepkörök hello lépéseit.</span><span class="sxs-lookup"><span data-stu-id="8dac1-107">Follow hello steps in this article tooperform a self-review of your assigned roles.</span></span>

<span data-ttu-id="8dac1-108">Ha egy kiemelt szerepkörű rendszergazda hozzáférést értékelést iránt érdeklődik, ezzel további adatokat, [hogyan toostart hozzáférés tekintse át](active-directory-privileged-identity-management-how-to-start-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="8dac1-108">If you're a privileged role administrator interested in access reviews, get more details at [How toostart an access review](active-directory-privileged-identity-management-how-to-start-security-review.md).</span></span>

## <a name="add-hello-privileged-identity-management-application"></a><span data-ttu-id="8dac1-109">Hello Privileged Identity Management alkalmazás felvétele</span><span class="sxs-lookup"><span data-stu-id="8dac1-109">Add hello Privileged Identity Management application</span></span>
<span data-ttu-id="8dac1-110">Hello hello Azure AD Privileged Identity Management (PIM) alkalmazás használható [Azure-portálon](https://portal.azure.com/) tooperform felülvizsgálandó.</span><span class="sxs-lookup"><span data-stu-id="8dac1-110">You can use hello Azure AD Privileged Identity Management (PIM) application in hello [Azure portal](https://portal.azure.com/) tooperform your review.</span></span>  <span data-ttu-id="8dac1-111">Hello Azure AD Privileged Identity Management alkalmazás nem rendelkezik a portálon, hajtsa végre az ezen lépések tooget elindult.</span><span class="sxs-lookup"><span data-stu-id="8dac1-111">If you don't have hello Azure AD Privileged Identity Management application on your portal, follow these steps tooget started.</span></span>

1. <span data-ttu-id="8dac1-112">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="8dac1-112">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="8dac1-113">Válassza ki a felhasználónév hello jobb felső sarokban a hello Azure-portálon, és ahol lesz, jelölje be hello directory működik.</span><span class="sxs-lookup"><span data-stu-id="8dac1-113">Select your username in hello upper right-hand corner of hello Azure portal, and select hello directory where you will you be operating.</span></span>
3. <span data-ttu-id="8dac1-114">Válassza ki **további szolgáltatások** és hello szűrő szövegmező toosearch a **Azure AD Privileged Identity Management**.</span><span class="sxs-lookup"><span data-stu-id="8dac1-114">Select **More services** and use hello Filter textbox toosearch for **Azure AD Privileged Identity Management**.</span></span>
4. <span data-ttu-id="8dac1-115">Ellenőrizze **PIN-kód toodashboard** majd **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="8dac1-115">Check **Pin toodashboard** and then click **Create**.</span></span> <span data-ttu-id="8dac1-116">Ekkor megnyílik a Privileged Identity Management alkalmazás hello.</span><span class="sxs-lookup"><span data-stu-id="8dac1-116">hello Privileged Identity Management application will open.</span></span>

## <a name="approve-or-deny-access"></a><span data-ttu-id="8dac1-117">Hagyja jóvá vagy nem engedélyezik a hozzáférést</span><span class="sxs-lookup"><span data-stu-id="8dac1-117">Approve or deny access</span></span>
<span data-ttu-id="8dac1-118">Hagyja jóvá vagy nem engedélyezik a hozzáférést, akkor még csak szólítja fel hello felülvizsgáló e továbbra is használhatja ezt a szerepkört vagy nem.</span><span class="sxs-lookup"><span data-stu-id="8dac1-118">When you approve or deny access, you're just telling hello reviewer whether you still use this role or not.</span></span> <span data-ttu-id="8dac1-119">Válasszon **jóváhagyás** Ha azt szeretné, hogy toostay hello szerepkörben vagy **Megtagadás** Ha azt nem kell hello hozzáférés többé.</span><span class="sxs-lookup"><span data-stu-id="8dac1-119">Choose **Approve** if you want toostay in hello role, or **Deny** if you don't need hello access anymore.</span></span> <span data-ttu-id="8dac1-120">Az állapota nem azonnal, megváltoztatni, amíg meg nem hello felülvizsgáló hello eredmények vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="8dac1-120">Your status won't change right away, until hello reviewer applies hello results.</span></span>
<span data-ttu-id="8dac1-121">Kövesse az alábbi lépéseket toofind, és végezze el a hello áttekintése:</span><span class="sxs-lookup"><span data-stu-id="8dac1-121">Follow these steps toofind and complete hello access review:</span></span>

1. <span data-ttu-id="8dac1-122">Hello PIM alkalmazást, válassza ki **felülvizsgálati emelt szintű hozzáférés**.</span><span class="sxs-lookup"><span data-stu-id="8dac1-122">In hello PIM application, select **Review privileged access**.</span></span> <span data-ttu-id="8dac1-123">Ha minden függőben lévő hozzáférés felülvizsgálatra, azok megjelennek hello Azure AD hozzáférési panel ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="8dac1-123">If you have any pending access reviews, they appear in hello Azure AD Access reviews blade.</span></span>
2. <span data-ttu-id="8dac1-124">Válassza ki a kívánt toocomplete hello áttekintése.</span><span class="sxs-lookup"><span data-stu-id="8dac1-124">Select hello review you want toocomplete.</span></span>
3. <span data-ttu-id="8dac1-125">Hello felülvizsgálati hozott létre, kivéve, hello csak felhasználói hello felülvizsgálat alatt jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="8dac1-125">Unless you created hello review, you appear as hello only user in hello review.</span></span> <span data-ttu-id="8dac1-126">Válassza ki a hello pipa következő tooyour nevét.</span><span class="sxs-lookup"><span data-stu-id="8dac1-126">Select hello check mark next tooyour name.</span></span>
4. <span data-ttu-id="8dac1-127">Válasszon **jóváhagyása** vagy **megtagadása**.</span><span class="sxs-lookup"><span data-stu-id="8dac1-127">Choose either **Approve** or **Deny**.</span></span> <span data-ttu-id="8dac1-128">Szükség lehet a hello döntés okát tooinclude **okot** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="8dac1-128">You may need tooinclude a reason for your decision in hello **Provide a reason** text box.</span></span>  
5. <span data-ttu-id="8dac1-129">Bezárás hello **tekintse át az Azure AD-szerepkörök** panelen.</span><span class="sxs-lookup"><span data-stu-id="8dac1-129">Close hello **Review Azure AD roles** blade.</span></span>

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="8dac1-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8dac1-130">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
