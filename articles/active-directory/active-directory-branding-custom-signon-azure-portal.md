---
title: "a Bejelentkezés lapon található hello Azure Active Directory aaaCustomize |} Microsoft Docs"
description: "Megtudhatja, hogyan tooadd egy vállalati arculat Azure bejelentkezés toohello lap"
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
ms.openlocfilehash: 151521e3b9cbc6a438a589735058fbff78443cf8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-company-branding-tooyour-sign-in-page-in-hello-azure-active-directory"></a><span data-ttu-id="3e569-103">Vállalati arculat megjelenítése a tooyour hello Azure Active Directory bejelentkezési lap</span><span class="sxs-lookup"><span data-stu-id="3e569-103">Add company branding tooyour sign-in page in hello Azure Active Directory</span></span>
<span data-ttu-id="3e569-104">tooavoid zavart, számos vállalat szeretné, hogy egy egységes megjelenést és működést tooapply összes hello webhelyek és szolgáltatások kezelnek.</span><span class="sxs-lookup"><span data-stu-id="3e569-104">tooavoid confusion, many companies want tooapply a consistent look and feel across all hello websites and services they manage.</span></span> <span data-ttu-id="3e569-105">Az Azure Active Directory toocustomize hello megjelenését a vállalat emblémájának elhelyezésével és egyéni színsémák alkalmazásával a hello bejelentkezési oldal lehetővé ezt a képességet nyújt.</span><span class="sxs-lookup"><span data-stu-id="3e569-105">Azure Active Directory provides this capability by allowing you toocustomize hello appearance of hello sign-in page with your company logo and custom color schemes.</span></span> <span data-ttu-id="3e569-106">hello bejelentkezési oldal hello oldal jelenik meg a bejelentkezéskor tooOffice 365 vagy más webes alkalmazásokat, az identitás-szolgáltatóként az Azure AD-t használó.</span><span class="sxs-lookup"><span data-stu-id="3e569-106">hello sign-in page is hello page that appears when you sign in tooOffice 365 or other web-based applications that are using Azure AD as your identity provider.</span></span> <span data-ttu-id="3e569-107">Ön a szolgáltatóosztályokkal a lap tooenter a hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="3e569-107">You interact with this page tooenter your credentials.</span></span>

<span data-ttu-id="3e569-108">Ha azt szeretné, hogy tooshow vállalatának arculatát, színeit és egyéb testreszabható elemeit ezen a lapon lásd a következő lemezképek toounderstand hello különbségének hello két megközelítés hello.</span><span class="sxs-lookup"><span data-stu-id="3e569-108">If you want tooshow your company brand, colors and other customizable elements on this page, see hello following images toounderstand hello difference between hello two experiences.</span></span>

<span data-ttu-id="3e569-109">következő képernyőfelvételen látható és példán hello Office 365 bejelentkezési oldala egy asztali számítógépen hello **előtt** testreszabás:</span><span class="sxs-lookup"><span data-stu-id="3e569-109">hello following screenshot shows and example for hello Office 365 sign-in page on a desktop computer **before** a customization:</span></span>

![Az Office 365 bejelentkezési oldala a testreszabást megelőzően](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-before-customization.png)

<span data-ttu-id="3e569-111">következő képernyőfelvételen látható és példán hello Office 365 bejelentkezési oldala egy asztali számítógépen hello **után** testreszabás:</span><span class="sxs-lookup"><span data-stu-id="3e569-111">hello following screenshot shows and example for hello Office 365 sign-in page on a desktop computer **after** a customization:</span></span>

![Az Office 365 bejelentkezési oldala a testreszabást követően](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-after-customization.png)

## <a name="customizing-hello-sign-in-page"></a><span data-ttu-id="3e569-113">Hello bejelentkezési oldal testreszabása</span><span class="sxs-lookup"><span data-stu-id="3e569-113">Customizing hello sign-in page</span></span>
<span data-ttu-id="3e569-114">Általában ha böngészőalapú hozzáférésre tooyour felhőalapú alkalmazások és szolgáltatások, amelyek a szervezet által előfizetett, van szüksége, használja hello bejelentkezési oldalára.</span><span class="sxs-lookup"><span data-stu-id="3e569-114">Typically, if you need browser-based access tooyour cloud apps and services that your organization subscribes to, you use hello sign-in page.</span></span>

<span data-ttu-id="3e569-115">Módosítások tooyour bejelentkezési oldal alkalmazta, hello módosítások tooappear tooan órát is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="3e569-115">If you have applied changes tooyour sign-in page, it can take up tooan hour for hello changes tooappear.</span></span>

<span data-ttu-id="3e569-116">A vállalati arculattal ellátott bejelentkezési oldal csak akkor jelenik meg, ha bérlőspecifikus URL-címmel (például https://outlook.com/**contoso**.com vagy https://mail.**contoso**.com) rendelkező szolgáltatást keres fel.</span><span class="sxs-lookup"><span data-stu-id="3e569-116">A branded sign-in page only appears when you visit a service with a tenant-specific URL such as https://outlook.com/**contoso**.com, or https://mail.**contoso**.com.</span></span>

<span data-ttu-id="3e569-117">Nem bérlőspecifikus URL-címmel (például https://mail.office365.com) ellátott szolgáltatás felkeresésekor nem vállalati arculattal rendelkező bejelentkezési oldal jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3e569-117">When you visit a service with non-tenant specific URLs (e.g.: https://mail.office365.com), a non-branded sign-in page appears.</span></span> <span data-ttu-id="3e569-118">Ebben az esetben a vállalati arculat a felhasználói azonosító megadása és a felhasználói csempe kiválasztása után jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3e569-118">in this case, your branding appears once you have entered your user ID or you have selected a user tile.</span></span>

> [!NOTE]
> * <span data-ttu-id="3e569-119">A tartománynév szerepelnie kell "Aktív" hello **tartományok** része hello Azure portál, amely a vállalati arculatot konfigurálta.</span><span class="sxs-lookup"><span data-stu-id="3e569-119">Your domain name must appear as “Active" in hello **Domains** portion of hello Azure portal in which you have configured branding.</span></span> <span data-ttu-id="3e569-120">További információkért lásd: [egyéni tartományneveket adhat hozzá](active-directory-domains-add-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3e569-120">For more information, see [Add custom domain names](active-directory-domains-add-azure-portal.md).</span></span>
> * <span data-ttu-id="3e569-121">Bejelentkezési oldal vállalati arculata toohello ügyfél-bejelentkezési Microsoft oldalán nem jelenik.</span><span class="sxs-lookup"><span data-stu-id="3e569-121">Sign-in page branding doesn’t carry over toohello consumer sign in page of Microsoft.</span></span> <span data-ttu-id="3e569-122">Ha Microsoft-fiókkal jelentkezik be, láthatja az Azure AD által nyújtott felhasználói csempék vállalati arculattal ellátott listáját, de hello branding a szervezet nem érvényes toohello Microsoft-fiókja bejelentkezési oldalára.</span><span class="sxs-lookup"><span data-stu-id="3e569-122">If you sign in with a Microsoft account, you may see a branded list of user tiles rendered by Azure AD, but hello branding of your organization does not apply toohello Microsoft account sign-in page.</span></span>
>
>

<span data-ttu-id="3e569-123">A bejelentkezési oldalon hello **bejelentkezve szeretnék maradni** jelölőnégyzet lehetővé teszi, hogy egy felhasználó tooremain bejelentkezett zárja be, és nyissa meg újra a böngészőt.</span><span class="sxs-lookup"><span data-stu-id="3e569-123">On your sign-in page, hello **Keep me signed in** checkbox allows a user tooremain signed in when they close and re-open their browser.</span></span>

   ![Bejelentkezve szeretnék maradni](./media/active-directory-branding-custom-signon-azure-portal/01.png)

<span data-ttu-id="3e569-125">Ez nincs hatással a munkamenet élettartamára.</span><span class="sxs-lookup"><span data-stu-id="3e569-125">It does not effect session lifetime.</span></span> <span data-ttu-id="3e569-126">Hello Azure Active Directory bejelentkezési oldal hello jelölőnégyzet elrejthetők.</span><span class="sxs-lookup"><span data-stu-id="3e569-126">You can hide hello checkbox on hello Azure Active Directory sign-in page.</span></span>
<span data-ttu-id="3e569-127">Hello beállítása hello jelölőnégyzet megjelenítését függ **bejelentkezve szeretnék maradni tiltva**.</span><span class="sxs-lookup"><span data-stu-id="3e569-127">Whether hello checkbox is displayed depends on hello setting of **Keep me signed in disabled**.</span></span>

   ![Bejelentkezve szeretnék maradni](./media/active-directory-branding-custom-signon-azure-portal/02.png)

<span data-ttu-id="3e569-129">toohide hello jelölőnégyzet, konfigurálja ezt a beállítást túl**Igen**.</span><span class="sxs-lookup"><span data-stu-id="3e569-129">toohide hello checkbox, configure this setting too**Yes**.</span></span>

> [!NOTE]
> <span data-ttu-id="3e569-130">Néhány funkciójának Office 2010 és SharePoint Online attól függnek, hogy a felhasználók képesek toocheck ebben a mezőben.</span><span class="sxs-lookup"><span data-stu-id="3e569-130">Some features of SharePoint Online and Office 2010 depend on users being able toocheck this box.</span></span> <span data-ttu-id="3e569-131">Ha ez a beállítás toohidden konfigurál, előfordulhat, hogy jelennek meg a felhasználók további és váratlan toosign a megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="3e569-131">If you configure this setting toohidden, your users may see additional and unexpected prompts toosign-in.</span></span>
>
>

<span data-ttu-id="3e569-132">**tooadd vállalati arculat tooyour könyvtár:**</span><span class="sxs-lookup"><span data-stu-id="3e569-132">**tooadd company branding tooyour directory:**</span></span>

1. <span data-ttu-id="3e569-133">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="3e569-133">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="3e569-134">Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** hello szövegmezőbe, és válassza ki **Enter**.</span><span class="sxs-lookup"><span data-stu-id="3e569-134">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

   ![Nyitó felhasználók kezelése](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)
3. <span data-ttu-id="3e569-136">A hello **felhasználók és csoportok** panelen válassza **vállalati arculat**.</span><span class="sxs-lookup"><span data-stu-id="3e569-136">On hello **Users and groups** blade, select **Company branding**.</span></span>
4. <span data-ttu-id="3e569-137">A hello **felhasználók és csoportok – a vállalati arculat** panelen, jelölje be hello **szerkesztése** parancsot.</span><span class="sxs-lookup"><span data-stu-id="3e569-137">On hello **Users and groups - Company branding** blade, select hello **Edit** command.</span></span>

    ![Egyéni branding szerkesztése](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)
5. <span data-ttu-id="3e569-139">Módosítsa a kívánt toocustomize hello elemek.</span><span class="sxs-lookup"><span data-stu-id="3e569-139">Modify hello elements you want toocustomize.</span></span> <span data-ttu-id="3e569-140">Ezen elemek egyike sem kötelező.</span><span class="sxs-lookup"><span data-stu-id="3e569-140">All elements are optional.</span></span>
6. <span data-ttu-id="3e569-141">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="3e569-141">Click **Save**.</span></span>

<span data-ttu-id="3e569-142">A módosítások toohello bejelentkezési lapon márkajelzési tooappear is eltarthat tooan óra.</span><span class="sxs-lookup"><span data-stu-id="3e569-142">It can take up tooan hour for any changes you made toohello sign-in page branding tooappear.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3e569-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3e569-143">Next steps</span></span>
[<span data-ttu-id="3e569-144">Nyelvspecifikus vállalati arculat megjelenítése</span><span class="sxs-lookup"><span data-stu-id="3e569-144">Add language-specific company branding</span></span>](active-directory-branding-localize-azure-portal.md)
