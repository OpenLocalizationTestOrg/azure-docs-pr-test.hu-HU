---
title: "Az Azure Active Directoryban a bejelentkezési oldal testreszabása |} Microsoft Docs"
description: "Útmutató: a vállalati arculat megjelenítése a Azure bejelentkezési oldal"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 27590c018ea55e9793246c7a4cab10f934ea502b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-company-branding-to-your-sign-in-page-in-the-azure-active-directory"></a><span data-ttu-id="b4887-103">Vállalati arculat megjelenítése a bejelentkezési oldal az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="b4887-103">Add company branding to your sign-in page in the Azure Active Directory</span></span>
<span data-ttu-id="b4887-104">A félreértések elkerülése végett számos vállalat igyekszik egységes megjelenést adni az általa kezelt összes webhelynek és szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="b4887-104">To avoid confusion, many companies want to apply a consistent look and feel across all the websites and services they manage.</span></span> <span data-ttu-id="b4887-105">Az Azure Active Directory ezt a képességet biztosít azáltal, hogy a vállalat emblémájának elhelyezésével és egyéni színsémák alkalmazásával a bejelentkezési oldal testreszabása.</span><span class="sxs-lookup"><span data-stu-id="b4887-105">Azure Active Directory provides this capability by allowing you to customize the appearance of the sign-in page with your company logo and custom color schemes.</span></span> <span data-ttu-id="b4887-106">A bejelentkezési oldal az Office 365 vagy más webes alkalmazásokat az identitás-szolgáltatóként az Azure AD-t használó bejelentkezéskor megjelenő lap.</span><span class="sxs-lookup"><span data-stu-id="b4887-106">The sign-in page is the page that appears when you sign in to Office 365 or other web-based applications that are using Azure AD as your identity provider.</span></span> <span data-ttu-id="b4887-107">Ön a szolgáltatóosztályokkal ezen a lapon adja meg a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="b4887-107">You interact with this page to enter your credentials.</span></span>

<span data-ttu-id="b4887-108">Ha meg kívánja jeleníteni vállalatának arculatát, színeit és egyéb testreszabható elemeit ezen az oldalon, az alábbi ábrákon megfigyelheti a különbséget a két megközelítés között.</span><span class="sxs-lookup"><span data-stu-id="b4887-108">If you want to show your company brand, colors and other customizable elements on this page, see the following images to understand the difference between the two experiences.</span></span>

<span data-ttu-id="b4887-109">Az alábbi képernyőfelvételen szereplő példán az Office 365 bejelentkezési oldala látható egy asztali számítógépen, a testreszabást **megelőzően**:</span><span class="sxs-lookup"><span data-stu-id="b4887-109">The following screenshot shows and example for the Office 365 sign-in page on a desktop computer **before** a customization:</span></span>

![Az Office 365 bejelentkezési oldala a testreszabást megelőzően](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-before-customization.png)

<span data-ttu-id="b4887-111">Az alábbi képernyőfelvételen szereplő példán az Office 365 bejelentkezési oldala látható egy asztali számítógépen, a testreszabást **követően**:</span><span class="sxs-lookup"><span data-stu-id="b4887-111">The following screenshot shows and example for the Office 365 sign-in page on a desktop computer **after** a customization:</span></span>

![Az Office 365 bejelentkezési oldala a testreszabást követően](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-after-customization.png)

## <a name="customizing-the-sign-in-page"></a><span data-ttu-id="b4887-113">A bejelentkezési oldal testreszabása</span><span class="sxs-lookup"><span data-stu-id="b4887-113">Customizing the sign-in page</span></span>
<span data-ttu-id="b4887-114">A felhasználók jellemzően akkor használják a bejelentkezési oldalt, ha böngészőalapú hozzáférésre van szükségük a szervezetük által előfizetett felhőalkalmazásaikhoz és szolgáltatásaikhoz.</span><span class="sxs-lookup"><span data-stu-id="b4887-114">Typically, if you need browser-based access to your cloud apps and services that your organization subscribes to, you use the sign-in page.</span></span>

<span data-ttu-id="b4887-115">A bejelentkezési oldalon alkalmazott módosítások megjelenése akár egy órát is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="b4887-115">If you have applied changes to your sign-in page, it can take up to an hour for the changes to appear.</span></span>

<span data-ttu-id="b4887-116">A vállalati arculattal ellátott bejelentkezési oldal csak akkor jelenik meg, ha bérlőspecifikus URL-címmel (például https://outlook.com/**contoso**.com vagy https://mail.**contoso**.com) rendelkező szolgáltatást keres fel.</span><span class="sxs-lookup"><span data-stu-id="b4887-116">A branded sign-in page only appears when you visit a service with a tenant-specific URL such as https://outlook.com/**contoso**.com, or https://mail.**contoso**.com.</span></span>

<span data-ttu-id="b4887-117">Nem bérlőspecifikus URL-címmel (például https://mail.office365.com) ellátott szolgáltatás felkeresésekor nem vállalati arculattal rendelkező bejelentkezési oldal jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b4887-117">When you visit a service with non-tenant specific URLs (e.g.: https://mail.office365.com), a non-branded sign-in page appears.</span></span> <span data-ttu-id="b4887-118">Ebben az esetben a vállalati arculat a felhasználói azonosító megadása és a felhasználói csempe kiválasztása után jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b4887-118">in this case, your branding appears once you have entered your user ID or you have selected a user tile.</span></span>

> [!NOTE]
> * <span data-ttu-id="b4887-119">A tartománynév szerepelnie kell "Aktív" a **tartományok** része az Azure portál, amely a vállalati arculatot konfigurálta.</span><span class="sxs-lookup"><span data-stu-id="b4887-119">Your domain name must appear as “Active" in the **Domains** portion of the Azure portal in which you have configured branding.</span></span> <span data-ttu-id="b4887-120">További információkért lásd: [egyéni tartományneveket adhat hozzá](active-directory-domains-add-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b4887-120">For more information, see [Add custom domain names](active-directory-domains-add-azure-portal.md).</span></span>
> * <span data-ttu-id="b4887-121">A bejelentkezési oldal vállalati arculata a Microsoft ügyfél-bejelentkezési oldalán nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b4887-121">Sign-in page branding doesn’t carry over to the consumer sign in page of Microsoft.</span></span> <span data-ttu-id="b4887-122">Ha Microsoft-fiókkal jelentkezik be, láthatja az Azure AD által nyújtott felhasználói csempék vállalati arculattal ellátott listáját, de a szervezet branding nem vonatkozik a Microsoft-fiókja bejelentkezési oldalára.</span><span class="sxs-lookup"><span data-stu-id="b4887-122">If you sign in with a Microsoft account, you may see a branded list of user tiles rendered by Azure AD, but the branding of your organization does not apply to the Microsoft account sign-in page.</span></span>
>
>

<span data-ttu-id="b4887-123">A bejelentkezési oldalon a **Bejelentkezve szeretnék maradni** jelölőnégyzet lehetővé teszi a felhasználó számára, hogy a böngészője bezárása és újbóli megnyitása után is bejelentkezve maradjon.</span><span class="sxs-lookup"><span data-stu-id="b4887-123">On your sign-in page, the **Keep me signed in** checkbox allows a user to remain signed in when they close and re-open their browser.</span></span>

   ![Bejelentkezve szeretnék maradni](./media/active-directory-branding-custom-signon-azure-portal/01.png)

<span data-ttu-id="b4887-125">Ez nincs hatással a munkamenet élettartamára.</span><span class="sxs-lookup"><span data-stu-id="b4887-125">It does not effect session lifetime.</span></span> <span data-ttu-id="b4887-126">Az Azure Active Directory bejelentkezési oldalán elrejtheti a jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="b4887-126">You can hide the checkbox on the Azure Active Directory sign-in page.</span></span>
<span data-ttu-id="b4887-127">A jelölőnégyzet megjelenítését a beállításától függ **bejelentkezve szeretnék maradni tiltva**.</span><span class="sxs-lookup"><span data-stu-id="b4887-127">Whether the checkbox is displayed depends on the setting of **Keep me signed in disabled**.</span></span>

   ![Bejelentkezve szeretnék maradni](./media/active-directory-branding-custom-signon-azure-portal/02.png)

<span data-ttu-id="b4887-129">A jelölőnégyzet elrejtéséhez e beállítás konfigurálásával **Igen**.</span><span class="sxs-lookup"><span data-stu-id="b4887-129">To hide the checkbox, configure this setting to **Yes**.</span></span>

> [!NOTE]
> <span data-ttu-id="b4887-130">A SharePoint Online és az Office 2010 egyes funkciói attól függenek, hogy a felhasználók be tudják-e jelölni ezt a jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="b4887-130">Some features of SharePoint Online and Office 2010 depend on users being able to check this box.</span></span> <span data-ttu-id="b4887-131">Ha ezt a beállítást rejtett értékre konfigurálja, a felhasználóinak további és váratlan bejelentkezési felszólítások jelenhetnek meg.</span><span class="sxs-lookup"><span data-stu-id="b4887-131">If you configure this setting to hidden, your users may see additional and unexpected prompts to sign-in.</span></span>
>
>

<span data-ttu-id="b4887-132">**Vállalti arculatot szeretne adni a könyvtárhoz:**</span><span class="sxs-lookup"><span data-stu-id="b4887-132">**To add company branding to your directory:**</span></span>

1. <span data-ttu-id="b4887-133">Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="b4887-133">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="b4887-134">Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** a szövegmezőbe, majd válassza ki azt a **Enter**.</span><span class="sxs-lookup"><span data-stu-id="b4887-134">Select **More services**, enter **Users and groups** in the text box, and then select **Enter**.</span></span>

   ![Nyitó felhasználók kezelése](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)
3. <span data-ttu-id="b4887-136">Az a **felhasználók és csoportok** panelen válassza **vállalati arculat**.</span><span class="sxs-lookup"><span data-stu-id="b4887-136">On the **Users and groups** blade, select **Company branding**.</span></span>
4. <span data-ttu-id="b4887-137">Az a **felhasználók és csoportok – a vállalati arculat** panelen válassza a **szerkesztése** parancsot.</span><span class="sxs-lookup"><span data-stu-id="b4887-137">On the **Users and groups - Company branding** blade, select the **Edit** command.</span></span>

    ![Egyéni branding szerkesztése](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)
5. <span data-ttu-id="b4887-139">Módosítsa az elemeket, amelyeket testre szeretne szabni.</span><span class="sxs-lookup"><span data-stu-id="b4887-139">Modify the elements you want to customize.</span></span> <span data-ttu-id="b4887-140">Ezen elemek egyike sem kötelező.</span><span class="sxs-lookup"><span data-stu-id="b4887-140">All elements are optional.</span></span>
6. <span data-ttu-id="b4887-141">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="b4887-141">Click **Save**.</span></span>

<span data-ttu-id="b4887-142">A bejelentkezési oldal megjelenik márkajelzési végrehajtott módosítások egy órát is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="b4887-142">It can take up to an hour for any changes you made to the sign-in page branding to appear.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b4887-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b4887-143">Next steps</span></span>
[<span data-ttu-id="b4887-144">Nyelvspecifikus vállalati arculat megjelenítése</span><span class="sxs-lookup"><span data-stu-id="b4887-144">Add language-specific company branding</span></span>](active-directory-branding-localize-azure-portal.md)
