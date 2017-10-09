---
title: "egy csoport toomanage hozzáférés tooSaaS alkalmazások aaaUsing |} Microsoft Docs"
description: "Az Azure Active Directory prémium vagy alapszintű tooassign toouse csoportok miként férhetnek hozzá a tooSaaS alkalmazások az Azure Active Directoryval integrált."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: ab8dee63-bedc-46ca-8852-234f5c16ae98
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: it-pro;oldportal
ms.openlocfilehash: f45ea4472b3d88e8ea514af3db31b4cc9ea68d58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-a-group-toomanage-access-toosaas-applications"></a><span data-ttu-id="b10a0-103">Egy csoport toomanage tooSaaS alkalmazásokat használatával</span><span class="sxs-lookup"><span data-stu-id="b10a0-103">Using a group toomanage access tooSaaS applications</span></span>
<span data-ttu-id="b10a0-104">Azure Active Directory (Azure AD) a prémium szintű Azure AD vagy az Azure AD alapvető licenccel rendelkező, csoportok tooassign hozzáférés tooa SaaS-alkalmazás, amely integrálva van az Azure AD használatával.</span><span class="sxs-lookup"><span data-stu-id="b10a0-104">Using Azure Active Directory (Azure AD) with an Azure AD Premium or Azure AD Basic license, you can use groups tooassign access tooa SaaS application that's integrated with Azure AD.</span></span> <span data-ttu-id="b10a0-105">Például ha tooassign hozzáférés az hello marketing részlege toouse öt különböző SaaS-alkalmazásokhoz, akkor is hello marketingrészleg hello felhasználókat tartalmazó csoport létrehozása, és majd rendelje hozzá az adott csoport toothese öt SaaS-alkalmazásokhoz, amelyek hello marketingrészleg igényli.</span><span class="sxs-lookup"><span data-stu-id="b10a0-105">For example, if you want tooassign access for hello marketing department toouse five different SaaS applications, you can create a group that contains hello users in hello marketing department, and then assign that group toothese five SaaS applications that are needed by hello marketing department.</span></span> <span data-ttu-id="b10a0-106">Így időt takaríthat marketing osztályon egy helyen hello hello tagsága kezelésével.</span><span class="sxs-lookup"><span data-stu-id="b10a0-106">This way you can save time by managing hello membership of hello marketing department in one place.</span></span> <span data-ttu-id="b10a0-107">Felhasználók majd rendelt toohello alkalmazás hozzáadásuk után hello marketing csoport tagjaként, és a hozzárendeléseik eltávolított hello alkalmazásból marketing csoportnak hello való eltávolításakor.</span><span class="sxs-lookup"><span data-stu-id="b10a0-107">Users then are assigned toohello application when they are added as members of hello marketing group, and have their assignments removed from hello application when they are removed from hello marketing group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b10a0-108">A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="b10a0-108">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span> 

<span data-ttu-id="b10a0-109">Ez a funkció több száz alkalmazásokat, amelyek adhat hozzá az Azure AD Application Gallery hello használható.</span><span class="sxs-lookup"><span data-stu-id="b10a0-109">This capability can be used with hundreds of applications that you can add from within hello Azure AD Application Gallery.</span></span>

<span data-ttu-id="b10a0-110">**egy csoport tooa SaaS-alkalmazás hozzáférését tooassign**</span><span class="sxs-lookup"><span data-stu-id="b10a0-110">**tooassign access for a group tooa SaaS application**</span></span>

1. <span data-ttu-id="b10a0-111">A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), jelölje be **Active Directory** hello bal oldali navigációs sávján hello.</span><span class="sxs-lookup"><span data-stu-id="b10a0-111">In hello [Azure classic portal](https://manage.windowsazure.com), select **Active Directory** on hello navigation bar on hello left hand side.</span></span>
2. <span data-ttu-id="b10a0-112">Jelölje be hello **Directory** fülre, és ezután nyissa meg hello directory tooassign hozzáférést egy csoport tooa SaaS-alkalmazáshoz használni kívánt.</span><span class="sxs-lookup"><span data-stu-id="b10a0-112">Select hello **Directory** tab, and then open hello directory in which you want tooassign access for a group tooa SaaS application.</span></span>
3. <span data-ttu-id="b10a0-113">Jelölje be hello **alkalmazások** fülre. Válasszon ki egy alkalmazást a hello Alkalmazáskatalógusában hozzáadott, és kattintson a hello **felhasználók és csoportok** fülre.</span><span class="sxs-lookup"><span data-stu-id="b10a0-113">Select hello **Applications** tab. Select an application that you added from hello Application Gallery, and then click  hello **Users and Groups** tab.</span></span>
4. <span data-ttu-id="b10a0-114">A hello **felhasználók és csoportok** lap hello **kezdve** mezőbe írja be a hello neve hello csoport toowhich kívánt tooassign hozzáférést, és ezután válasszon hello hello jobb felső van jelölve.</span><span class="sxs-lookup"><span data-stu-id="b10a0-114">On hello **Users and Groups** tab, in hello **Starting with** field, enter hello name of hello group toowhich you want tooassign access, and then select hello check mark in hello upper right.</span></span> <span data-ttu-id="b10a0-115">Csak akkor kell tootype hello első részére a csoport nevét.</span><span class="sxs-lookup"><span data-stu-id="b10a0-115">You only need tootype hello first part of a group's name.</span></span>
5. <span data-ttu-id="b10a0-116">Hello válasszon ki, majd válassza a hello **hozzáférés hozzárendelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b10a0-116">Select hello group, then then select hello **Assign Access** button.</span></span> <span data-ttu-id="b10a0-117">Válassza ki **Igen** amikor hello megerősítő üzenet megjelenik.</span><span class="sxs-lookup"><span data-stu-id="b10a0-117">Select **Yes** when you see hello confirmation message.</span></span> <span data-ttu-id="b10a0-118">A beágyazott csoporttagság nem támogatottak a csoport-alapú hozzárendelés tooapplications most.</span><span class="sxs-lookup"><span data-stu-id="b10a0-118">Nested group memberships are not supported for group-based assignment tooapplications at this time.</span></span>
6. <span data-ttu-id="b10a0-119">Mely felhasználók legyenek társítva toohello alkalmazás, vagy közvetlenül a csoport tagsága is megtekinthető.</span><span class="sxs-lookup"><span data-stu-id="b10a0-119">You can also see which users are assigned toohello application, either directly or by membership in a group.</span></span> <span data-ttu-id="b10a0-120">toodo, a módosítás hello **legördülő lista megjelenítése "csoportból** túl**"Minden felhasználó"**.</span><span class="sxs-lookup"><span data-stu-id="b10a0-120">toodo this, change hello **Show dropdown from 'Groups'** too**'All Users'**.</span></span> <span data-ttu-id="b10a0-121">hello listán látható a felhasználók hello könyvtárban és-e minden felhasználóhoz hozzá van rendelve toohello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b10a0-121">hello list shows users in hello directory and whether or not each user is assigned toohello application.</span></span> <span data-ttu-id="b10a0-122">hello lista is mutatja, hogy hello hozzárendelt felhasználók legyenek társítva közvetlenül toohello alkalmazás (hozzárendelés-típus "Közvetlen" jelenik meg), vagy csoporttagság (hozzárendelés-típus megjelennek az helyeként "Örökölt.") alapján</span><span class="sxs-lookup"><span data-stu-id="b10a0-122">hello list also shows whether hello assigned users are assigned toohello application directly (assignment type shown as 'Direct'), or by virtue of group membership (assignment type shown as 'Inherited.')</span></span>

> [!NOTE]
> <span data-ttu-id="b10a0-123">Hello felhasználók és csoportok lapon csak a prémium szintű Azure AD vagy az Azure AD alapvető engedélyezése után tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="b10a0-123">You can see hello Users and Groups tab only after you have enabled Azure AD Premium or Azure AD Basic.</span></span>
>
>

### <a name="next-steps"></a><span data-ttu-id="b10a0-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b10a0-124">Next steps</span></span>
<span data-ttu-id="b10a0-125">E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="b10a0-125">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="b10a0-126">Hozzáférés tooresources kezelése az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="b10a0-126">Managing access tooresources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="b10a0-127">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="b10a0-127">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="b10a0-128">Azure Active Directory-parancsmagok csoportbeállítások konfigurálásához</span><span class="sxs-lookup"><span data-stu-id="b10a0-128">Azure Active Directory cmdlets for configuring group settings</span></span>](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [<span data-ttu-id="b10a0-129">Mi az az Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b10a0-129">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="b10a0-130">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="b10a0-130">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
