---
title: "aaaHow tooconfigure önkiszolgáló alkalmazás-hozzárendelés |} Microsoft Docs"
description: "Lehetővé teszi a önkiszolgáló alkalmazás hozzáférés tooallow felhasználók toofind saját alkalmazások"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: d25a0146c4c8cebf9c2ae8c516f094a8eccb4570
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-self-service-application-assignment"></a><span data-ttu-id="bff23-103">Hogyan tooconfigure önkiszolgáló alkalmazás-hozzárendelés</span><span class="sxs-lookup"><span data-stu-id="bff23-103">How tooconfigure self-service application assignment</span></span>

<span data-ttu-id="bff23-104">Mielőtt a felhasználók saját maguk felderíthetők az alkalmazások a hozzáférési panelen, meg kell-e tooenable **önkiszolgáló alkalmazás-hozzáférés** tooany alkalmazások, hogy kívánja-e tooallow felhasználók tooself-felderítését és ő hozzáférést tud adni.</span><span class="sxs-lookup"><span data-stu-id="bff23-104">Before your users can self-discover applications from their access panel, you need tooenable **Self-service application access** tooany applications that you wish tooallow users tooself-discover and request access to.</span></span>

<span data-ttu-id="bff23-105">Ez a funkció az Ön kiváló módja toosave időt és pénzt, egy IT-részleg, és erősen ajánlott az Azure Active Directoryban a modern alkalmazások központi telepítésének részeként.</span><span class="sxs-lookup"><span data-stu-id="bff23-105">This feature is a great way for you toosave time and money as an IT group, and is highly recommended as part of a modern applications deployment with Azure Active Directory.</span></span>

<span data-ttu-id="bff23-106">Ez a szolgáltatás használatával:</span><span class="sxs-lookup"><span data-stu-id="bff23-106">Using this feature, you can:</span></span>

-   <span data-ttu-id="bff23-107">Önálló felderítése hello alkalmazások felhasználók [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/) nélkül bothering hello informatikai csoport.</span><span class="sxs-lookup"><span data-stu-id="bff23-107">Let users self-discover applications from hello [Application Access Panel](https://myapps.microsoft.com/) without bothering hello IT group.</span></span>

-   <span data-ttu-id="bff23-108">Adja hozzá ezeket felhasználók tooa előre konfigurált csoportot, tekintse meg, akik hozzáférést kér, megszünteti a hozzáférést, és hozzárendelt toothem hello szerepkörök kezelése.</span><span class="sxs-lookup"><span data-stu-id="bff23-108">Add those users tooa pre-configured group so you can see who has requested access, remove access, and manage hello roles assigned toothem.</span></span>

-   <span data-ttu-id="bff23-109">Opcionálisan egy vállalati jóváhagyó tooapprove alkalmazás hozzáférési kérelmek engedélyezéséhez, IT-részleg hello nem kell.</span><span class="sxs-lookup"><span data-stu-id="bff23-109">Optionally allow a business approver tooapprove application access requests so hello IT group doesn’t have to.</span></span>

-   <span data-ttu-id="bff23-110">Választható lehetőségként konfigurálhatja too10 használhatják, akik jóváhagyhatja access toothis alkalmazás fel.</span><span class="sxs-lookup"><span data-stu-id="bff23-110">Optionally configure up too10 individuals who may approve access toothis application.</span></span>

-   <span data-ttu-id="bff23-111">Opcionálisan lehetővé teszi a vállalati jóváhagyó tooset hello jelszavak azoknak a felhasználóknak használható toosign toohello alkalmazásban, közvetlenül a hello vállalati jóváhagyó [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="bff23-111">Optionally allow a business approver tooset hello passwords those users can use toosign in toohello application, right from hello business approver’s [Application Access Panel](https://myapps.microsoft.com/).</span></span>

-   <span data-ttu-id="bff23-112">Nem kötelező automatikus hozzárendelése közvetlenül hozzárendelt önkiszolgáló felhasználók tooan alkalmazás-szerepkör.</span><span class="sxs-lookup"><span data-stu-id="bff23-112">Optionally automatically assign self-service assigned users tooan application role directly.</span></span>

## <a name="enable-self-service-application-access-tooallow-users-toofind-their-own-applications"></a><span data-ttu-id="bff23-113">Lehetővé teszi a önkiszolgáló alkalmazás hozzáférés tooallow felhasználók toofind saját alkalmazások</span><span class="sxs-lookup"><span data-stu-id="bff23-113">Enable self-service application access tooallow users toofind their own applications</span></span>

<span data-ttu-id="bff23-114">Önkiszolgáló alkalmazás hozzáférése egy remek mód tooallow felhasználók tooself-alkalmazások észlelése, nem kötelezően engedélyezése hello üzleti csoport tooapprove toothose alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="bff23-114">Self-service application access is a great way tooallow users tooself-discover applications, optionally allow hello business group tooapprove access toothose applications.</span></span> <span data-ttu-id="bff23-115">Engedélyezheti, hogy hello üzleti csoport toomanage hello hitelesítő adatokat kap toothose jelszó egyszeri bejelentkezést az alkalmazások közvetlenül a hozzáférési panel a felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="bff23-115">You can allow hello business group toomanage hello credentials assigned toothose users for Password Single-Sign On Applications right from their access panels.</span></span>

<span data-ttu-id="bff23-116">tooenable önkiszolgáló alkalmazás access tooan alkalmazást, kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="bff23-116">tooenable self-service application access tooan application, follow hello steps below:</span></span>

1.  <span data-ttu-id="bff23-117">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="bff23-117">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="bff23-118">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="bff23-118">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="bff23-119">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="bff23-119">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="bff23-120">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="bff23-120">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="bff23-121">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="bff23-121">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="bff23-122">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="bff23-122">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="bff23-123">Válassza ki a kívánt tooenable önkiszolgáló hozzáférés toofrom hello lista hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bff23-123">Select hello application you want tooenable Self-service access toofrom hello list.</span></span>

7.  <span data-ttu-id="bff23-124">Amikor hello alkalmazás betölt, kattintson a **önkiszolgáló** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="bff23-124">Once hello application loads, click **Self-service** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="bff23-125">tooenable önkiszolgáló alkalmazás hozzáférése az alkalmazáshoz, kapcsolja be a hello **toorequest access toothis alkalmazás engedélyezése a felhasználók?** túl Váltás**igen.**</span><span class="sxs-lookup"><span data-stu-id="bff23-125">tooenable Self-service application access for this application, turn hello **Allow users toorequest access toothis application?** toggle too**Yes.**</span></span>

9.  <span data-ttu-id="bff23-126">A következő tooselect hello csoport toowhich felhasználókat, akik kérnek hozzáférést toothis alkalmazás hozzá kell adni, kattintson a hello választó következő toohello címke **toowhich csoport kell hozzárendelve felhasználókat kell felvenni?** válasszon ki egy csoportot.</span><span class="sxs-lookup"><span data-stu-id="bff23-126">Next, tooselect hello group toowhich users who request access toothis application should be added, click hello selector next toohello label **toowhich group should assigned users be added?** and select a group.</span></span>

10. <span data-ttu-id="bff23-127">**Választható lehetőség:** Ha toorequire egy üzleti jóváhagyása előtt a felhasználók hozzáférhetnek, állítsa be a hello **access toothis alkalmazás megadása előtt jóvá kell hagyni?** túl váltása**Igen**.</span><span class="sxs-lookup"><span data-stu-id="bff23-127">**Optional:** If you wish toorequire a business approval before users are allowed access, set hello **Require approval before granting access toothis application?** toggle too**Yes**.</span></span>

11. <span data-ttu-id="bff23-128">**Választható lehetőség: az alkalmazások csak a jelszó egyszeri bejelentkezés használatával** Ha tooallow adott üzleti jóváhagyóknak toospecify hello jelszavak jóváhagyott felhasználók toothis kérelmet küldött, állítsa be a hello **jóváhagyóknak tooset engedélyezése felhasználói jelszavak ehhez az alkalmazáshoz?**  túl váltása**Igen**.</span><span class="sxs-lookup"><span data-stu-id="bff23-128">**Optional: For applications using password single-sign on only,** if you wish tooallow those business approvers toospecify hello passwords that are sent toothis application for approved users, set hello **Allow approvers tooset user’s passwords for this application?** toggle too**Yes**.</span></span>

12. <span data-ttu-id="bff23-129">**Választható lehetőség:** toospecify hello üzleti jóváhagyóknak tooapprove access toothis alkalmazást, akik kattintson hello választó következő toohello címke **tooapprove access toothis alkalmazás engedélyezett?** tooselect mentése too10 egyedi üzleti jóváhagyóknak.</span><span class="sxs-lookup"><span data-stu-id="bff23-129">**Optional:** toospecify hello business approvers who are allowed tooapprove access toothis application, click hello selector next toohello label **Who is allowed tooapprove access toothis application?** tooselect up too10 individual business approvers.</span></span>

   >[!NOTE]
   ><span data-ttu-id="bff23-130">Csoportok használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="bff23-130">Groups are not supported.</span></span>
   >
   >

13. <span data-ttu-id="bff23-131">**Választható lehetőség:** **az alkalmazások, amelyeknél a szerepkörök**, ha tooassign jóváhagyott felhasználók önkiszolgáló tooa szerepkör hello választó következő toohello kattintson **toowhich szerepkör felhasználóhoz rendelni ezen alkalmazás?**  tooselect hello szerepkör toowhich ezeket a felhasználókat hozzá kell rendelni.</span><span class="sxs-lookup"><span data-stu-id="bff23-131">**Optional:** **For applications which expose roles**, if you wish tooassign self-service approved users tooa role, click hello selector next toohello **toowhich role should users be assigned in this application?** tooselect hello role toowhich these users should be assigned.</span></span>

14. <span data-ttu-id="bff23-132">Kattintson a hello **mentése** hello panel toofinish hello tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="bff23-132">Click hello **Save** button at hello top of hello blade toofinish.</span></span>

<span data-ttu-id="bff23-133">Ha befejezte az önkiszolgáló Alkalmazáskonfiguráció, a felhasználók navigálhatnak tootheir [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/) hello kattintson **+ Hozzáadás** gomb toofind hello alkalmazások toowhich engedélyezte Önkiszolgáló hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="bff23-133">Once you complete Self-service application configuration, users can navigate tootheir [Application Access Panel](https://myapps.microsoft.com/) and click hello **+Add** button toofind hello apps toowhich you have enabled Self-service access.</span></span> <span data-ttu-id="bff23-134">Üzleti jóváhagyóknak is megjelenik egy értesítés a saját [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="bff23-134">Business approvers also see a notification in their [Application Access Panel](https://myapps.microsoft.com/).</span></span> <span data-ttu-id="bff23-135">Engedélyezheti a felhasználói hozzáférés tooan a jóváhagyást igénylő alkalmazás kérelmezésekor értesítő e-mailt.</span><span class="sxs-lookup"><span data-stu-id="bff23-135">You can enable an email notifying them when a user has requested access tooan application that requires their approval.</span></span> 

<span data-ttu-id="bff23-136">Ezek a jóváhagyások támogatja egyetlen jóváhagyási munkafolyamatok csak, ami azt jelenti, hogy több jóváhagyó ad meg, ha bármely egyetlen jóváhagyó jóváhagyó access toohello alkalmazást is.</span><span class="sxs-lookup"><span data-stu-id="bff23-136">These approvals support single approval workflows only, meaning that if you specify multiple approvers, any single approver may approver access toohello application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bff23-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bff23-137">Next steps</span></span>
[<span data-ttu-id="bff23-138">Azure Active Directory beállítása önkiszolgáló csoportkezelés</span><span class="sxs-lookup"><span data-stu-id="bff23-138">Setting up Azure Active Directory for self-service group management</span></span>](active-directory-accessmanagement-self-service-group-management.md)
