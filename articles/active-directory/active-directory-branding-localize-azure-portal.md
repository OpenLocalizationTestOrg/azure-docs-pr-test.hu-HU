---
title: "Nyelvspecifikus vállalati arculat megjelenítése a bejelentkezési oldal az Azure Active Directory |} Microsoft Docs"
description: "Útmutató: a nyelvi adott vállalati arculat képek és szövegek egy Azure bejelentkezési oldalra"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a0310d6a-aaa7-4ea0-991d-6d3135b4382a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: e1fe8d855386ceec39edbc985538cdf32d78a13b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-language-specific-company-branding-to-your-sign-in-page-in-the-azure-active-directory"></a><span data-ttu-id="dadef-103">Nyelvspecifikus vállalati arculat megjelenítése a bejelentkezési oldal az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="dadef-103">Add language-specific company branding to your sign-in page in the Azure Active Directory</span></span>
<span data-ttu-id="dadef-104">A félreértések elkerülése végett számos vállalat igyekszik egységes megjelenést adni az általa kezelt összes webhelynek és szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="dadef-104">To avoid confusion, many companies want to apply a consistent look and feel across all the websites and services they manage.</span></span> <span data-ttu-id="dadef-105">Az Azure Active Directory ezt a képességet biztosít azáltal, hogy a vállalat emblémájának elhelyezésével és egyéni színsémák alkalmazásával a bejelentkezési oldal testreszabása.</span><span class="sxs-lookup"><span data-stu-id="dadef-105">Azure Active Directory provides this capability by allowing you to customize the appearance of the sign-in page with your company logo and custom color schemes.</span></span> <span data-ttu-id="dadef-106">A bejelentkezési oldal az Office 365 vagy más webes alkalmazásokat az identitás-szolgáltatóként az Azure AD-t használó bejelentkezéskor megjelenő lap.</span><span class="sxs-lookup"><span data-stu-id="dadef-106">The sign-in page is the page that appears when you sign in to Office 365 or other web-based applications that are using Azure AD as your identity provider.</span></span> <span data-ttu-id="dadef-107">Ön a szolgáltatóosztályokkal ezen a lapon adja meg a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="dadef-107">You interact with this page to enter your credentials.</span></span>

## <a name="customizing-the-sign-in-page-for-another-language"></a><span data-ttu-id="dadef-108">A bejelentkezési oldal más nyelven történő testreszabása</span><span class="sxs-lookup"><span data-stu-id="dadef-108">Customizing the sign-in page for another language</span></span>
<span data-ttu-id="dadef-109">Hozzáadhat nyelvspecifikus elemek az egyéni bejelentkezési oldal csak akkor, ha már létrehozott egy egyéni bejelentkezési oldala a [vállalati arculat megjelenítése a bejelentkezési oldal](active-directory-branding-custom-signon-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="dadef-109">You can add language-specific elements to your custom sign-in page only if you have already created a custom sign-in page as described in [Add company branding to your sign-in page](active-directory-branding-custom-signon-azure-portal.md).</span></span> <span data-ttu-id="dadef-110">Egy bejelentkezési oldal Címtáranként egy testreszabható elemek egy alapértelmezett készletét konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="dadef-110">You can configure one sign-in page per directory with a default set of customizable elements.</span></span> <span data-ttu-id="dadef-111">Miután konfigurálta a elemei alapértelmezett készletét, konfigurálhatja a különböző területi beállításokhoz további verziókat.</span><span class="sxs-lookup"><span data-stu-id="dadef-111">After you’ve configured the default set of page elements, you can configure more versions for different locales.</span></span> <span data-ttu-id="dadef-112">A különböző elemek szabadon kombinálhatók.</span><span class="sxs-lookup"><span data-stu-id="dadef-112">You can also mix and match various elements.</span></span> <span data-ttu-id="dadef-113">Például hogy a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="dadef-113">For example, you could:</span></span>

* <span data-ttu-id="dadef-114">Hozzon létre egy alapértelmezett **bejelentkezési lap képe** , minden országban használható, majd hozzon létre különböző verziókat angol és francia.</span><span class="sxs-lookup"><span data-stu-id="dadef-114">Create a default **Sign-in page image** that works for all cultures, then create specific versions for English and French.</span></span> <span data-ttu-id="dadef-115">Ha úgy állítja be a böngészőbe egy két nyelv valamelyikére, a nyelvspecifikus kép jelenik meg, míg minden más nyelv az alapértelmezett ábra jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="dadef-115">When you set your browsers to one of these two languages, the language-specific image appears, while the default illustration appears for all other languages.</span></span>
* <span data-ttu-id="dadef-116">A vállalat eltérő verziójú (például japán vagy héber) emblémáit is beállíthatja.</span><span class="sxs-lookup"><span data-stu-id="dadef-116">Configure different logos for your organization (for example, Japanese or Hebrew versions).</span></span>

<span data-ttu-id="dadef-117">Azt javasoljuk, hogy ne túl sok nyelvi változat alacsony karbantartási és teljesítmény okokból.</span><span class="sxs-lookup"><span data-stu-id="dadef-117">We recommend that you keep the number of language variations low, for maintenance and performance reasons.</span></span>

<span data-ttu-id="dadef-118">**Vállalti arculatot szeretne adni a könyvtárhoz:**</span><span class="sxs-lookup"><span data-stu-id="dadef-118">**To add company branding to your directory:**</span></span>

1. <span data-ttu-id="dadef-119">Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="dadef-119">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="dadef-120">Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** a szövegmezőbe, majd válassza ki azt a **Enter**.</span><span class="sxs-lookup"><span data-stu-id="dadef-120">Select **More services**, enter **Users and groups** in the text box, and then select **Enter**.</span></span>

   ![Nyitó felhasználók kezelése](./media/active-directory-branding-localize-azure-portal/user-management.png)
3. <span data-ttu-id="dadef-122">Az a **felhasználók és csoportok** panelen válassza **vállalati arculat**.</span><span class="sxs-lookup"><span data-stu-id="dadef-122">On the **Users and groups** blade, select **Company branding**.</span></span>
4. <span data-ttu-id="dadef-123">Az a **felhasználók és csoportok – a vállalati arculat** panelen válassza a **nyelv hozzáadása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="dadef-123">On the **Users and groups - Company branding** blade, select the **Add language** command.</span></span>

    ![Nyelvspecifikus márkajelzési elemek hozzáadása](./media/active-directory-branding-localize-azure-portal/add-language.png)
5. <span data-ttu-id="dadef-125">Módosítsa az elemeket, amelyeket testre szeretne szabni.</span><span class="sxs-lookup"><span data-stu-id="dadef-125">Modify the elements you want to customize.</span></span> <span data-ttu-id="dadef-126">Ezen elemek egyike sem kötelező.</span><span class="sxs-lookup"><span data-stu-id="dadef-126">All elements are optional.</span></span>
6. <span data-ttu-id="dadef-127">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="dadef-127">Click **Save**.</span></span>

<span data-ttu-id="dadef-128">A bejelentkezési oldal megjelenik márkajelzési végrehajtott módosítások egy órát is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="dadef-128">It can take up to an hour for any changes you made to the sign-in page branding to appear.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dadef-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dadef-129">Next steps</span></span>
[<span data-ttu-id="dadef-130">Vállalati arculat megjelenítése a bejelentkezési oldal</span><span class="sxs-lookup"><span data-stu-id="dadef-130">Add company branding to your sign-in page</span></span>](active-directory-branding-custom-signon-azure-portal.md)
