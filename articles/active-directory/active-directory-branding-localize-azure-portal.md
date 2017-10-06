---
title: "aaaAdd nyelvspecifikus Védjegyadatok tooyour bejelentkezési oldal az Azure Active Directory hello |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd egy nyelvspecifikus vállalati arculat képek és szöveget az Azure-bejelentkezés tooan lap"
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
ms.openlocfilehash: 1e33c31abc242e8455290beb1f03760be7b9ac42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-language-specific-company-branding-tooyour-sign-in-page-in-hello-azure-active-directory"></a><span data-ttu-id="c4b53-103">Nyelvspecifikus vállalati arculat megjelenítése a tooyour hello Azure Active Directory bejelentkezési lap</span><span class="sxs-lookup"><span data-stu-id="c4b53-103">Add language-specific company branding tooyour sign-in page in hello Azure Active Directory</span></span>
<span data-ttu-id="c4b53-104">tooavoid zavart, számos vállalat szeretné, hogy egy egységes megjelenést és működést tooapply összes hello webhelyek és szolgáltatások kezelnek.</span><span class="sxs-lookup"><span data-stu-id="c4b53-104">tooavoid confusion, many companies want tooapply a consistent look and feel across all hello websites and services they manage.</span></span> <span data-ttu-id="c4b53-105">Az Azure Active Directory toocustomize hello megjelenését a vállalat emblémájának elhelyezésével és egyéni színsémák alkalmazásával a hello bejelentkezési oldal lehetővé ezt a képességet nyújt.</span><span class="sxs-lookup"><span data-stu-id="c4b53-105">Azure Active Directory provides this capability by allowing you toocustomize hello appearance of hello sign-in page with your company logo and custom color schemes.</span></span> <span data-ttu-id="c4b53-106">hello bejelentkezési oldal hello oldal jelenik meg a bejelentkezéskor tooOffice 365 vagy más webes alkalmazásokat, az identitás-szolgáltatóként az Azure AD-t használó.</span><span class="sxs-lookup"><span data-stu-id="c4b53-106">hello sign-in page is hello page that appears when you sign in tooOffice 365 or other web-based applications that are using Azure AD as your identity provider.</span></span> <span data-ttu-id="c4b53-107">Ön a szolgáltatóosztályokkal a lap tooenter a hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="c4b53-107">You interact with this page tooenter your credentials.</span></span>

## <a name="customizing-hello-sign-in-page-for-another-language"></a><span data-ttu-id="c4b53-108">Más nyelven történő hello bejelentkezési oldal testreszabása</span><span class="sxs-lookup"><span data-stu-id="c4b53-108">Customizing hello sign-in page for another language</span></span>
<span data-ttu-id="c4b53-109">Hozzáadhat nyelvspecifikus elemek tooyour egyéni bejelentkezési oldal csak akkor, ha már létrehozott egy egyéni bejelentkezési oldal leírtak [vállalati arculat megjelenítése a bejelentkezési oldal tooyour](active-directory-branding-custom-signon-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c4b53-109">You can add language-specific elements tooyour custom sign-in page only if you have already created a custom sign-in page as described in [Add company branding tooyour sign-in page](active-directory-branding-custom-signon-azure-portal.md).</span></span> <span data-ttu-id="c4b53-110">Egy bejelentkezési oldal Címtáranként egy testreszabható elemek egy alapértelmezett készletét konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="c4b53-110">You can configure one sign-in page per directory with a default set of customizable elements.</span></span> <span data-ttu-id="c4b53-111">Miután konfigurálta a elemei hello alapértelmezett készletét, konfigurálhatja a különböző területi beállításokhoz további verziókat.</span><span class="sxs-lookup"><span data-stu-id="c4b53-111">After you’ve configured hello default set of page elements, you can configure more versions for different locales.</span></span> <span data-ttu-id="c4b53-112">A különböző elemek szabadon kombinálhatók.</span><span class="sxs-lookup"><span data-stu-id="c4b53-112">You can also mix and match various elements.</span></span> <span data-ttu-id="c4b53-113">Például hogy a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="c4b53-113">For example, you could:</span></span>

* <span data-ttu-id="c4b53-114">Hozzon létre egy alapértelmezett **bejelentkezési lap képe** , minden országban használható, majd hozzon létre különböző verziókat angol és francia.</span><span class="sxs-lookup"><span data-stu-id="c4b53-114">Create a default **Sign-in page image** that works for all cultures, then create specific versions for English and French.</span></span> <span data-ttu-id="c4b53-115">Ha úgy állítja be a böngészők tooone két nyelv valamelyikére, hello nyelvspecifikus kép jelenik meg, míg minden más nyelv hello alapértelmezett ábra jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c4b53-115">When you set your browsers tooone of these two languages, hello language-specific image appears, while hello default illustration appears for all other languages.</span></span>
* <span data-ttu-id="c4b53-116">A vállalat eltérő verziójú (például japán vagy héber) emblémáit is beállíthatja.</span><span class="sxs-lookup"><span data-stu-id="c4b53-116">Configure different logos for your organization (for example, Japanese or Hebrew versions).</span></span>

<span data-ttu-id="c4b53-117">Azt javasoljuk, hogy ne hello túl sok nyelvi változat alacsony karbantartási és teljesítmény okokból.</span><span class="sxs-lookup"><span data-stu-id="c4b53-117">We recommend that you keep hello number of language variations low, for maintenance and performance reasons.</span></span>

<span data-ttu-id="c4b53-118">**tooadd vállalati arculat tooyour könyvtár:**</span><span class="sxs-lookup"><span data-stu-id="c4b53-118">**tooadd company branding tooyour directory:**</span></span>

1. <span data-ttu-id="c4b53-119">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="c4b53-119">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="c4b53-120">Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** hello szövegmezőbe, és válassza ki **Enter**.</span><span class="sxs-lookup"><span data-stu-id="c4b53-120">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

   ![Nyitó felhasználók kezelése](./media/active-directory-branding-localize-azure-portal/user-management.png)
3. <span data-ttu-id="c4b53-122">A hello **felhasználók és csoportok** panelen válassza **vállalati arculat**.</span><span class="sxs-lookup"><span data-stu-id="c4b53-122">On hello **Users and groups** blade, select **Company branding**.</span></span>
4. <span data-ttu-id="c4b53-123">A hello **felhasználók és csoportok – a vállalati arculat** panelen, jelölje be hello **nyelv hozzáadása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="c4b53-123">On hello **Users and groups - Company branding** blade, select hello **Add language** command.</span></span>

    ![Nyelvspecifikus márkajelzési elemek hozzáadása](./media/active-directory-branding-localize-azure-portal/add-language.png)
5. <span data-ttu-id="c4b53-125">Módosítsa a kívánt toocustomize hello elemek.</span><span class="sxs-lookup"><span data-stu-id="c4b53-125">Modify hello elements you want toocustomize.</span></span> <span data-ttu-id="c4b53-126">Ezen elemek egyike sem kötelező.</span><span class="sxs-lookup"><span data-stu-id="c4b53-126">All elements are optional.</span></span>
6. <span data-ttu-id="c4b53-127">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="c4b53-127">Click **Save**.</span></span>

<span data-ttu-id="c4b53-128">A módosítások toohello bejelentkezési lapon márkajelzési tooappear is eltarthat tooan óra.</span><span class="sxs-lookup"><span data-stu-id="c4b53-128">It can take up tooan hour for any changes you made toohello sign-in page branding tooappear.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c4b53-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c4b53-129">Next steps</span></span>
[<span data-ttu-id="c4b53-130">Vállalati arculat megjelenítése a tooyour bejelentkezési oldal</span><span class="sxs-lookup"><span data-stu-id="c4b53-130">Add company branding tooyour sign-in page</span></span>](active-directory-branding-custom-signon-azure-portal.md)
