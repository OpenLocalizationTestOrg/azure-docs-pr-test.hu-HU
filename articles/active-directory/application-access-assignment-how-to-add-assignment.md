---
title: "aaaHow tooassign felhasználók és csoportok tooan alkalmazás |} Microsoft Docs"
description: "Felhasználók toohello alkalmazás toogrant hozzáférés hozzárendelése"
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
ms.openlocfilehash: e039a26e4b8f88ad747354859f1071b8f74b6789
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooassign-users-and-groups-tooan-application"></a><span data-ttu-id="a5403-103">Hogyan tooassign felhasználók és csoportok tooan alkalmazás</span><span class="sxs-lookup"><span data-stu-id="a5403-103">How tooassign users and groups tooan application</span></span>

<span data-ttu-id="a5403-104">Mielőtt a felhasználók lehetőségek közül választhat hello alább egy adott alkalmazáshoz, meg kell-e toofirst **rendeljen hozzájuk toohello alkalmazás** toogrant őket elérni:</span><span class="sxs-lookup"><span data-stu-id="a5403-104">Before your users can do any of hello below for a specific application, you need toofirst **assign them toohello application** toogrant them access:</span></span>

-   <span data-ttu-id="a5403-105">Az alkalmazás által elérése **toohello alkalmazás URL-címet közvetlenül a Navigálás** (más néven Szolgáltató által kezdeményezett bejelentkezés).</span><span class="sxs-lookup"><span data-stu-id="a5403-105">Access an application by **navigating toohello application’s URL directly** (also known as SP-initiated sign-on).</span></span>

-   <span data-ttu-id="a5403-106">Az alkalmazás hozzáférhet a hello **felhasználói URL-CÍMEN** olyan alkalmazást **tulajdonságok** (más néven IDP által kezdeményezett bejelentkezés) lap.</span><span class="sxs-lookup"><span data-stu-id="a5403-106">Access an application by using hello **User Access URL** on an application’s **Properties** page (also known as IDP-initiated sign on).</span></span>

-   <span data-ttu-id="a5403-107">Tekintse meg az alkalmazás jelennek meg azok [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/) vagy mobilalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a5403-107">See an application appear on their [Application Access Panel](https://myapps.microsoft.com/) or mobile application.</span></span>

-   <span data-ttu-id="a5403-108">Tekintse meg az alkalmazás jelennek meg azok [Office 365 alkalmazásindító](https://support.office.com/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a).</span><span class="sxs-lookup"><span data-stu-id="a5403-108">See an application appear on their [Office 365 Application Launcher](https://support.office.com/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a).</span></span>

## <a name="methods-tooassign-applications-with-azure-active-directory"></a><span data-ttu-id="a5403-109">Módszerek tooassign alkalmazásokat az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="a5403-109">Methods tooassign applications with Azure Active Directory</span></span> 

<span data-ttu-id="a5403-110">Alkalmazások az Azure Active Directoryval rendelhet 3 módja van:</span><span class="sxs-lookup"><span data-stu-id="a5403-110">There are 3 ways you can assign applications with Azure Active Directory:</span></span>

-   [<span data-ttu-id="a5403-111">Rendelje hozzá a felhasználó közvetlenül tooan alkalmazás-rendszergazdaként</span><span class="sxs-lookup"><span data-stu-id="a5403-111">Assign a user directly tooan application as an administrator</span></span>](#assign-a-user-directly-as-an-administrator)

-   [<span data-ttu-id="a5403-112">Rendelje hozzá egy csoporthoz közvetlenül tooan alkalmazás-rendszergazdaként</span><span class="sxs-lookup"><span data-stu-id="a5403-112">Assign a group directly tooan application as an administrator</span></span>](#assign-a-group-directly-to-an-application-as-an-administrator)

-   [<span data-ttu-id="a5403-113">Lehetővé teszi a önkiszolgáló alkalmazás hozzáférés tooallow felhasználók toofind saját alkalmazások</span><span class="sxs-lookup"><span data-stu-id="a5403-113">Enable self-service application access tooallow users toofind their own applications</span></span>](#enable-self-service-application-access-to-allow-users-to-find-their-own-applications)

## <a name="assign-a-user-directly-as-an-administrator"></a><span data-ttu-id="a5403-114">Rendelje hozzá a felhasználó közvetlenül rendszergazdaként</span><span class="sxs-lookup"><span data-stu-id="a5403-114">Assign a user directly as an administrator</span></span>

<span data-ttu-id="a5403-115">tooassign felhasználók tooan egy vagy több alkalmazás közvetlenül, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a5403-115">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="a5403-116">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="a5403-116">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="a5403-117">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="a5403-117">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="a5403-118">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="a5403-118">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="a5403-119">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a5403-119">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="a5403-120">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="a5403-120">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="a5403-121">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="a5403-121">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="a5403-122">Válassza ki a kívánt felhasználó toofrom hello listáját tooassign hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a5403-122">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="a5403-123">Amikor hello alkalmazás betölt, kattintson a **felhasználók és csoportok** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a5403-123">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="a5403-124">Hello kattintson **Hozzáadás** gomb felett hello **felhasználók és csoportok** lista tooopen hello **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="a5403-124">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="a5403-125">hello kattintson **felhasználók és csoportok** hello a választó **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="a5403-125">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="a5403-126">Hello típusának **teljes név** vagy **e-mail cím** érdekli hello való hozzárendelése hello felhasználó **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="a5403-126">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="a5403-127">Hello rámutat **felhasználói** a hello lista tooreveal egy **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="a5403-127">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="a5403-128">Kattintson a hello jelölőnégyzet következő toohello felhasználó profil fénykép vagy embléma tooadd a felhasználó toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="a5403-128">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="a5403-129">**Választható lehetőség:** Ha túl szeretné**egynél több felhasználó hozzáadása**, egy másik típus **teljes név** vagy **e-mail cím** be hello **Keresés név e-mail cím vagy** keresési mezőbe, majd kattintson a hello jelölőnégyzet tooadd a felhasználó toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="a5403-129">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="a5403-130">Ha elkészült, válassza a felhasználók, kattintson a hello **válasszon** gomb tooadd őket felhasználók és csoportok toobe toohello listája toohello alkalmazás hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="a5403-130">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="a5403-131">**Nem kötelező:** hello kattintson **Szerepkörválasztás** hello a választó **hozzáadása hozzárendelés** panel tooselect szerepkör tooassign toohello felhasználók kijelölt.</span><span class="sxs-lookup"><span data-stu-id="a5403-131">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="a5403-132">Kattintson a hello **hozzárendelése** gomb tooassign hello alkalmazás toohello kijelölt felhasználók.</span><span class="sxs-lookup"><span data-stu-id="a5403-132">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="a5403-133">Rövid időn belül a kijelölt hello felhasználók kell ezeket az alkalmazásokat használó hello hello megoldás leírása szakaszban ismertetett módszerekkel tudja toolaunch.</span><span class="sxs-lookup"><span data-stu-id="a5403-133">After a short period of time, hello users you have selected be able toolaunch these applications using hello methods described in hello solution description section.</span></span>

## <a name="assign-a-group-directly-tooan-application-as-an-administrator"></a><span data-ttu-id="a5403-134">Rendelje hozzá egy csoporthoz közvetlenül tooan alkalmazás-rendszergazdaként</span><span class="sxs-lookup"><span data-stu-id="a5403-134">Assign a group directly tooan application as an administrator</span></span>

<span data-ttu-id="a5403-135">egy vagy több tooassign csoportok közvetlenül, tooan alkalmazás kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a5403-135">tooassign one or more groups tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="a5403-136">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="a5403-136">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="a5403-137">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="a5403-137">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="a5403-138">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="a5403-138">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="a5403-139">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a5403-139">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="a5403-140">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="a5403-140">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="a5403-141">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="a5403-141">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="a5403-142">Válassza ki a kívánt felhasználó toofrom hello listáját tooassign hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a5403-142">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="a5403-143">Amikor hello alkalmazás betölt, kattintson a **felhasználók és csoportok** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a5403-143">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="a5403-144">Hello kattintson **Hozzáadás** gomb felett hello **felhasználók és csoportok** lista tooopen hello **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="a5403-144">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="a5403-145">hello kattintson **felhasználók és csoportok** hello a választó **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="a5403-145">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="a5403-146">Hello típusa **teljes csoportnév** érdekli hello való hozzárendelése hello csoport **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="a5403-146">Type in hello **full group name** of hello group you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="a5403-147">Hello rámutat **csoport** a hello lista tooreveal egy **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="a5403-147">Hover over hello **group** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="a5403-148">Kattintson a profil fénykép vagy embléma tooadd hello jelölőnégyzet következő toohello csoport a felhasználó toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="a5403-148">Click hello checkbox next toohello group’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="a5403-149">**Nem kötelező:** Ha túl szeretné**egynél több csoport hozzáadása**, egy másik típus **teljes csoport neve** hello be **Keresés név vagy e-mail cím alapján** keresőmezőbe hello jelölőnégyzet tooadd kattintson a csoport toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="a5403-149">**Optional:** If you would like too**add more than one group**, type in another **full group name** into hello **Search by name or email address** search box, and click hello checkbox tooadd this group toohello **Selected** list.</span></span>

13. <span data-ttu-id="a5403-150">Ha befejezte a csoportok kiválasztásával, kattintson a hello **válasszon** gomb tooadd őket felhasználók és csoportok toobe toohello listája toohello alkalmazás hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="a5403-150">When you are finished selecting groups, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="a5403-151">**Választható lehetőség:** hello kattintson **Szerepkörválasztás** hello a választó **hozzáadása hozzárendelés** panel tooselect egy szerepkör tooassign toohello csoportokat kijelölt.</span><span class="sxs-lookup"><span data-stu-id="a5403-151">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello groups you have selected.</span></span>

15. <span data-ttu-id="a5403-152">Kattintson a hello **hozzárendelése** gomb tooassign hello alkalmazás toohello kijelölt csoportok.</span><span class="sxs-lookup"><span data-stu-id="a5403-152">Click hello **Assign** button tooassign hello application toohello selected groups.</span></span>

<span data-ttu-id="a5403-153">Rövid időn belül hello felhasználók kijelölt hello csoportok belül kell ezeket az alkalmazásokat használó hello hello megoldás leírása szakaszban ismertetett módszerekkel tudja toolaunch.</span><span class="sxs-lookup"><span data-stu-id="a5403-153">After a short period of time, hello users within hello groups you have selected be able toolaunch these applications using hello methods described in hello solution description section.</span></span> <span data-ttu-id="a5403-154">Ha ezek a dinamikus csoportok, néhány további feldolgozás késés előfordulhat ezek a felhasználók számára csoportok magukba belül szereplő hozzárendelésekben.</span><span class="sxs-lookup"><span data-stu-id="a5403-154">If these are dynamic groups, there may be some additional processing delay in these assignments appearing for users within these assigned groups.</span></span>

## <a name="enable-self-service-application-access-tooallow-users-toofind-their-own-applications"></a><span data-ttu-id="a5403-155">Lehetővé teszi a önkiszolgáló alkalmazás hozzáférés tooallow felhasználók toofind saját alkalmazások</span><span class="sxs-lookup"><span data-stu-id="a5403-155">Enable self-service application access tooallow users toofind their own applications</span></span>

<span data-ttu-id="a5403-156">Önkiszolgáló alkalmazás hozzáférése egy remek mód tooallow felhasználók tooself-alkalmazások észlelése, nem kötelezően engedélyezése hello üzleti csoport tooapprove toothose alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="a5403-156">Self-service application access is a great way tooallow users tooself-discover applications, optionally allow hello business group tooapprove access toothose applications.</span></span> <span data-ttu-id="a5403-157">Engedélyezheti, hogy hello üzleti csoport toomanage hello hitelesítő adatokat kap toothose jelszó egyszeri bejelentkezést az alkalmazások közvetlenül a hozzáférési panel a felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="a5403-157">You can allow hello business group toomanage hello credentials assigned toothose users for Password Single-Sign On Applications right from their access panels.</span></span>

<span data-ttu-id="a5403-158">tooenable önkiszolgáló alkalmazás access tooan alkalmazást, kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a5403-158">tooenable self-service application access tooan application, follow hello steps below:</span></span>

1.  <span data-ttu-id="a5403-159">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="a5403-159">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="a5403-160">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="a5403-160">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="a5403-161">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="a5403-161">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="a5403-162">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a5403-162">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="a5403-163">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="a5403-163">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="a5403-164">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="a5403-164">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="a5403-165">Válassza ki a kívánt tooenable önkiszolgáló hozzáférés toofrom hello lista hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a5403-165">Select hello application you want tooenable Self-service access toofrom hello list.</span></span>

7.  <span data-ttu-id="a5403-166">Amikor hello alkalmazás betölt, kattintson a **önkiszolgáló** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a5403-166">Once hello application loads, click **Self-service** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="a5403-167">tooenable önkiszolgáló alkalmazás hozzáférése az alkalmazáshoz, kapcsolja be a hello **toorequest access toothis alkalmazás engedélyezése a felhasználók?** túl Váltás**igen.**</span><span class="sxs-lookup"><span data-stu-id="a5403-167">tooenable Self-service application access for this application, turn hello **Allow users toorequest access toothis application?** toggle too**Yes.**</span></span>

9.  <span data-ttu-id="a5403-168">A következő tooselect hello csoport toowhich felhasználókat, akik kérnek hozzáférést toothis alkalmazás hozzá kell adni, kattintson a hello választó következő toohello címke **toowhich csoport kell hozzárendelve felhasználókat kell felvenni?** válasszon ki egy csoportot.</span><span class="sxs-lookup"><span data-stu-id="a5403-168">Next, tooselect hello group toowhich users who request access toothis application should be added, click hello selector next toohello label **toowhich group should assigned users be added?** and select a group.</span></span>

10. <span data-ttu-id="a5403-169">**Választható lehetőség:** Ha toorequire egy üzleti jóváhagyása előtt a felhasználók hozzáférhetnek, állítsa be a hello **access toothis alkalmazás megadása előtt jóvá kell hagyni?** túl váltása**Igen**.</span><span class="sxs-lookup"><span data-stu-id="a5403-169">**Optional:** If you wish toorequire a business approval before users are allowed access, set hello **Require approval before granting access toothis application?** toggle too**Yes**.</span></span>

11. <span data-ttu-id="a5403-170">**Választható lehetőség: az alkalmazások csak a jelszó egyszeri bejelentkezés használatával** Ha tooallow adott üzleti jóváhagyóknak toospecify hello jelszavak jóváhagyott felhasználók toothis kérelmet küldött, állítsa be a hello **jóváhagyóknak tooset engedélyezése felhasználói jelszavak ehhez az alkalmazáshoz?**  túl váltása**Igen**.</span><span class="sxs-lookup"><span data-stu-id="a5403-170">**Optional: For applications using password single-sign on only,** if you wish tooallow those business approvers toospecify hello passwords that are sent toothis application for approved users, set hello **Allow approvers tooset user’s passwords for this application?** toggle too**Yes**.</span></span>

12. <span data-ttu-id="a5403-171">**Választható lehetőség:** toospecify hello üzleti jóváhagyóknak tooapprove access toothis alkalmazást, akik kattintson hello választó következő toohello címke **tooapprove access toothis alkalmazás engedélyezett?** tooselect mentése too10 egyedi üzleti jóváhagyóknak.</span><span class="sxs-lookup"><span data-stu-id="a5403-171">**Optional:** toospecify hello business approvers who are allowed tooapprove access toothis application, click hello selector next toohello label **Who is allowed tooapprove access toothis application?** tooselect up too10 individual business approvers.</span></span>

  >[!NOTE]
  ><span data-ttu-id="a5403-172">Csoportok használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="a5403-172">Groups are not supported.</span></span>
  >
  >

13. <span data-ttu-id="a5403-173">**Választható lehetőség:** **az alkalmazások, amelyeknél a szerepkörök**, ha tooassign jóváhagyott felhasználók önkiszolgáló tooa szerepkör hello választó következő toohello kattintson **toowhich szerepkör felhasználóhoz rendelni ezen alkalmazás?**  tooselect hello szerepkör toowhich ezeket a felhasználókat hozzá kell rendelni.</span><span class="sxs-lookup"><span data-stu-id="a5403-173">**Optional:** **For applications which expose roles**, if you wish tooassign self-service approved users tooa role, click hello selector next toohello **toowhich role should users be assigned in this application?** tooselect hello role toowhich these users should be assigned.</span></span>

14. <span data-ttu-id="a5403-174">Kattintson a hello **mentése** hello panel toofinish hello tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="a5403-174">Click hello **Save** button at hello top of hello blade toofinish.</span></span>

<span data-ttu-id="a5403-175">Ha befejezte az önkiszolgáló Alkalmazáskonfiguráció, a felhasználók navigálhatnak tootheir [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/) hello kattintson **+ Hozzáadás** gomb toofind hello alkalmazások toowhich engedélyezte Önkiszolgáló hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="a5403-175">Once you complete Self-service application configuration, users can navigate tootheir [Application Access Panel](https://myapps.microsoft.com/) and click hello **+Add** button toofind hello apps toowhich you have enabled Self-service access.</span></span> <span data-ttu-id="a5403-176">Üzleti jóváhagyóknak is megjelenik egy értesítés a saját [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="a5403-176">Business approvers also see a notification in their [Application Access Panel](https://myapps.microsoft.com/).</span></span> <span data-ttu-id="a5403-177">Engedélyezheti a felhasználói hozzáférés tooan a jóváhagyást igénylő alkalmazás kérelmezésekor értesítő e-mailt.</span><span class="sxs-lookup"><span data-stu-id="a5403-177">You can enable an email notifying them when a user has requested access tooan application that requires their approval.</span></span> 

<span data-ttu-id="a5403-178">Ezek a jóváhagyások támogatja egyetlen jóváhagyási munkafolyamatok csak, ami azt jelenti, hogy több jóváhagyó ad meg, ha bármely egyetlen jóváhagyó jóváhagyó access toohello alkalmazást is.</span><span class="sxs-lookup"><span data-stu-id="a5403-178">These approvals support single approval workflows only, meaning that if you specify multiple approvers, any single approver may approver access toohello application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a5403-179">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a5403-179">Next steps</span></span>
[<span data-ttu-id="a5403-180">Adja meg az egyszeri bejelentkezés tooyour alkalmazások alkalmazásproxyval</span><span class="sxs-lookup"><span data-stu-id="a5403-180">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
