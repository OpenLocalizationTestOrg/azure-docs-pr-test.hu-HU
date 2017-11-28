---
title: "önkiszolgáló alkalmazás-hozzáférés segítségével aaaProblem |} Microsoft Docs"
description: "Kapcsolódó tooself szolgáltatásalkalmazás hozzáférési problémák elhárítása"
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
ms.reviewer: japere
ms.openlocfilehash: 2487be1df191a4e7fd0bcc0ebbe4ea62fae0fd5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problem-using-self-service-application-access"></a><span data-ttu-id="c3c40-103">Problémát az önkiszolgáló alkalmazás-hozzáférés</span><span class="sxs-lookup"><span data-stu-id="c3c40-103">Problem using self-service application access</span></span>

<span data-ttu-id="c3c40-104">Önkiszolgáló alkalmazás hozzáférése egy remek mód tooallow felhasználók tooself-alkalmazások észlelése, nem kötelezően engedélyezése hello üzleti csoport tooapprove toothose alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="c3c40-104">Self-service application access is a great way tooallow users tooself-discover applications, optionally allow hello business group tooapprove access toothose applications.</span></span> <span data-ttu-id="c3c40-105">Engedélyezheti, hogy hello üzleti csoport toomanage hello hitelesítő adatokat kap toothose jelszó egyszeri bejelentkezést az alkalmazások közvetlenül a hozzáférési panel a felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="c3c40-105">You can allow hello business group toomanage hello credentials assigned toothose users for Password Single-Sign On Applications right from their access panels.</span></span>

<span data-ttu-id="c3c40-106">Mielőtt a felhasználók saját maguk felderíthetők az alkalmazások a hozzáférési panelen, meg kell-e tooenable **önkiszolgáló alkalmazás-hozzáférés** tooany alkalmazások, hogy kívánja-e tooallow felhasználók tooself-felderítését és ő hozzáférést tud adni.</span><span class="sxs-lookup"><span data-stu-id="c3c40-106">Before your users can self-discover applications from their access panel, you need tooenable **Self-service application access** tooany applications that you wish tooallow users tooself-discover and request access to.</span></span>

## <a name="general-issues-toocheck-first"></a><span data-ttu-id="c3c40-107">Általános problémák első toocheck</span><span class="sxs-lookup"><span data-stu-id="c3c40-107">General issues toocheck first</span></span>

-   <span data-ttu-id="c3c40-108">Ellenőrizze, hogy az önkiszolgáló alkalmazás-hozzáférés megfelelően van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="c3c40-108">Make sure self-service application access is configured correctly.</span></span> <span data-ttu-id="c3c40-109">Lásd: hogyan tooconfigure önkiszolgáló alkalmazás hozzáférés-".</span><span class="sxs-lookup"><span data-stu-id="c3c40-109">See “How tooconfigure self-service application access”.</span></span>

-   <span data-ttu-id="c3c40-110">Ellenőrizze, hogy hello felhasználó vagy csoport lett toorequest önkiszolgáló alkalmazás-hozzáférés engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="c3c40-110">Make sure hello user or group has been enabled toorequest self-service application access.</span></span>

-   <span data-ttu-id="c3c40-111">Ellenőrizze, hogy hello felhasználói felkeresett önkiszolgáló alkalmazás-hozzáférési hello a megfelelő helyen.</span><span class="sxs-lookup"><span data-stu-id="c3c40-111">Make sure hello user is visiting hello correct place for self-service application access.</span></span> <span data-ttu-id="c3c40-112">felhasználók navigálhatnak tootheir [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/) hello kattintson **+ Hozzáadás** gomb toofind hello alkalmazások toowhich önkiszolgáló hozzáférés engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="c3c40-112">users can navigate tootheir [Application Access Panel](https://myapps.microsoft.com/) and click hello **+Add** button toofind hello apps toowhich you have enabled self-service access.</span></span>

-   <span data-ttu-id="c3c40-113">Ha önkiszolgáló alkalmazás-hozzáférés csak nemrég lett konfigurálva, próbálja toosign bejövő és kimenő adatforgalma újra hello felhasználói hozzáférési panelre után néhány perc toosee Ha hello önkiszolgáló változásokat jelentek.</span><span class="sxs-lookup"><span data-stu-id="c3c40-113">If self-service application access was just recently configured, try toosign in and out again into hello user’s Access Panel after a few minutes toosee if hello self-service access changes have appeared.</span></span>

## <a name="how-tooconfigure-self-service-application-access"></a><span data-ttu-id="c3c40-114">Hogyan férnek hozzá az tooconfigure önkiszolgáló alkalmazás</span><span class="sxs-lookup"><span data-stu-id="c3c40-114">How tooconfigure self-service application access</span></span>

<span data-ttu-id="c3c40-115">tooenable önkiszolgáló alkalmazás access tooan alkalmazást, kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c3c40-115">tooenable self-service application access tooan application, follow hello steps below:</span></span>

1.  <span data-ttu-id="c3c40-116">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="c3c40-116">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="c3c40-117">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="c3c40-117">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="c3c40-118">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="c3c40-118">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="c3c40-119">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="c3c40-119">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="c3c40-120">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="c3c40-120">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="c3c40-121">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="c3c40-121">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="c3c40-122">Válassza ki a kívánt tooenable önkiszolgáló hozzáférés toofrom hello lista hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c3c40-122">Select hello application you want tooenable Self-service access toofrom hello list.</span></span>

7.  <span data-ttu-id="c3c40-123">Amikor hello alkalmazás betölt, kattintson a **önkiszolgáló** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="c3c40-123">Once hello application loads, click **Self-service** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="c3c40-124">tooenable önkiszolgáló alkalmazás hozzáférése az alkalmazáshoz, kapcsolja be a hello **toorequest access toothis alkalmazás engedélyezése a felhasználók?** túl Váltás**igen.**</span><span class="sxs-lookup"><span data-stu-id="c3c40-124">tooenable Self-service application access for this application, turn hello **Allow users toorequest access toothis application?** toggle too**Yes.**</span></span>

9.  <span data-ttu-id="c3c40-125">A következő tooselect hello csoport toowhich felhasználókat, akik kérnek hozzáférést toothis alkalmazás hozzá kell adni, kattintson a hello választó következő toohello címke **toowhich csoport kell hozzárendelve felhasználókat kell felvenni?** válasszon ki egy csoportot.</span><span class="sxs-lookup"><span data-stu-id="c3c40-125">Next, tooselect hello group toowhich users who request access toothis application should be added, click hello selector next toohello label **toowhich group should assigned users be added?** and select a group.</span></span>

10. <span data-ttu-id="c3c40-126">**Választható lehetőség:** Ha toorequire egy üzleti jóváhagyása előtt a felhasználók hozzáférhetnek, állítsa be a hello **access toothis alkalmazás megadása előtt jóvá kell hagyni?** túl váltása**Igen**.</span><span class="sxs-lookup"><span data-stu-id="c3c40-126">**Optional:** If you wish toorequire a business approval before users are allowed access, set hello **Require approval before granting access toothis application?** toggle too**Yes**.</span></span>

11. <span data-ttu-id="c3c40-127">**Választható lehetőség: az alkalmazások csak a jelszó egyszeri bejelentkezés használatával** Ha tooallow adott üzleti jóváhagyóknak toospecify hello jelszavak jóváhagyott felhasználók toothis kérelmet küldött, állítsa be a hello **jóváhagyóknak tooset engedélyezése felhasználói jelszavak ehhez az alkalmazáshoz?**  túl váltása**Igen**.</span><span class="sxs-lookup"><span data-stu-id="c3c40-127">**Optional: For applications using password single-sign on only,** if you wish tooallow those business approvers toospecify hello passwords that are sent toothis application for approved users, set hello **Allow approvers tooset user’s passwords for this application?** toggle too**Yes**.</span></span>

12. <span data-ttu-id="c3c40-128">**Választható lehetőség:** toospecify hello üzleti jóváhagyóknak tooapprove access toothis alkalmazást, akik kattintson hello választó következő toohello címke **tooapprove access toothis alkalmazás engedélyezett?** tooselect mentése too10 egyedi üzleti jóváhagyóknak.</span><span class="sxs-lookup"><span data-stu-id="c3c40-128">**Optional:** toospecify hello business approvers who are allowed tooapprove access toothis application, click hello selector next toohello label **Who is allowed tooapprove access toothis application?** tooselect up too10 individual business approvers.</span></span>

 >[!NOTE]
 > <span data-ttu-id="c3c40-129">Csoportok használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="c3c40-129">Groups are not supported.</span></span>
 >
 >

13. <span data-ttu-id="c3c40-130">**Választható lehetőség:** **az alkalmazások, amelyeknél a szerepkörök**, ha tooassign jóváhagyott felhasználók önkiszolgáló tooa szerepkör hello választó következő toohello kattintson **toowhich szerepkör felhasználóhoz rendelni ezen alkalmazás?**  tooselect hello szerepkör toowhich ezeket a felhasználókat hozzá kell rendelni.</span><span class="sxs-lookup"><span data-stu-id="c3c40-130">**Optional:** **For applications which expose roles**, if you wish tooassign self-service approved users tooa role, click hello selector next toohello **toowhich role should users be assigned in this application?** tooselect hello role toowhich these users should be assigned.</span></span>

14. <span data-ttu-id="c3c40-131">Kattintson a hello **mentése** hello panel toofinish hello tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="c3c40-131">Click hello **Save** button at hello top of hello blade toofinish.</span></span>

<span data-ttu-id="c3c40-132">Ha befejezte az önkiszolgáló Alkalmazáskonfiguráció, a felhasználók navigálhatnak tootheir [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/) hello kattintson **+ Hozzáadás** gomb toofind hello alkalmazások toowhich engedélyezte Önkiszolgáló hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="c3c40-132">Once you complete Self-service application configuration, users can navigate tootheir [Application Access Panel](https://myapps.microsoft.com/) and click hello **+Add** button toofind hello apps toowhich you have enabled Self-service access.</span></span> <span data-ttu-id="c3c40-133">Üzleti jóváhagyóknak is megjelenik egy értesítés a saját [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="c3c40-133">Business approvers also see a notification in their [Application Access Panel](https://myapps.microsoft.com/).</span></span> <span data-ttu-id="c3c40-134">Engedélyezheti a felhasználói hozzáférés tooan a jóváhagyást igénylő alkalmazás kérelmezésekor értesítő e-mailt.</span><span class="sxs-lookup"><span data-stu-id="c3c40-134">You can enable an email notifying them when a user has requested access tooan application that requires their approval.</span></span> 

<span data-ttu-id="c3c40-135">Ezek a jóváhagyások egyetlen jóváhagyási munkafolyamatok csak, ami azt jelenti, hogy több jóváhagyó ad meg, ha bármely egyetlen jóváhagyó jóváhagyhatja a access toohello alkalmazást támogatja.</span><span class="sxs-lookup"><span data-stu-id="c3c40-135">These approvals support single approval workflows only, meaning that if you specify multiple approvers, any single approver may approve access toohello application.</span></span>

## <a name="if-these-troubleshooting-steps-do-not-resolve-hello-issue"></a><span data-ttu-id="c3c40-136">Ha ezek a hibaelhárítási lépéseket nem oldható meg hello probléma</span><span class="sxs-lookup"><span data-stu-id="c3c40-136">If these troubleshooting steps do not resolve hello issue</span></span> 

<span data-ttu-id="c3c40-137">Nyisson meg egy támogatási jegy hello ha rendelkezésre áll a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="c3c40-137">open a support ticket with hello following information if available:</span></span>

-   <span data-ttu-id="c3c40-138">Megfelelési hiba azonosítója</span><span class="sxs-lookup"><span data-stu-id="c3c40-138">Correlation error ID</span></span>

-   <span data-ttu-id="c3c40-139">Egyszerű felhasználónév (felhasználó e-mail címe)</span><span class="sxs-lookup"><span data-stu-id="c3c40-139">UPN (user email address)</span></span>

-   <span data-ttu-id="c3c40-140">A TenantID</span><span class="sxs-lookup"><span data-stu-id="c3c40-140">TenantID</span></span>

-   <span data-ttu-id="c3c40-141">Böngésző típusa</span><span class="sxs-lookup"><span data-stu-id="c3c40-141">Browser type</span></span>

-   <span data-ttu-id="c3c40-142">Időzóna és idő/időkeretre során hiba történik.</span><span class="sxs-lookup"><span data-stu-id="c3c40-142">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="c3c40-143">Fiddler nyomkövetések</span><span class="sxs-lookup"><span data-stu-id="c3c40-143">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="c3c40-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c3c40-144">Next steps</span></span>
[<span data-ttu-id="c3c40-145">Azure Active Directory beállítása önkiszolgáló csoportkezelés</span><span class="sxs-lookup"><span data-stu-id="c3c40-145">Setting up Azure Active Directory for self-service group management</span></span>](active-directory-accessmanagement-self-service-group-management.md)
