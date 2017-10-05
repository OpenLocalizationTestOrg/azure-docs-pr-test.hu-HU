---
title: "Alkalmazások megjelenésének a hozzáférési panel |} Microsoft Docs"
description: "Ezért az alkalmazás jelenik-e meg a hozzáférési panelen hibaelhárítása"
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
ms.reviewr: japere
ms.openlocfilehash: f8ccf2cf66b49940bc7f2b9f4764020efc04838e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-applications-appear-on-the-access-panel"></a><span data-ttu-id="5d370-103">Alkalmazások megjelenésének a hozzáférési panel</span><span class="sxs-lookup"><span data-stu-id="5d370-103">How applications appear on the access panel</span></span>

<span data-ttu-id="5d370-104">A hozzáférési Panel egy webes portál, amely lehetővé teszi a felhasználó munkahelyi vagy iskolai fiókkal az Azure Active Directory (Azure AD) a megtekintése, és indítsa el a felhőalapú alkalmazások, hogy az Azure AD-rendszergazda engedélyezte őket hozzáférés annak.</span><span class="sxs-lookup"><span data-stu-id="5d370-104">The Access Panel is a web-based portal which enables a user with a work or school account in Azure Active Directory (Azure AD) to view and start cloud-based applications that the Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="5d370-105">Ezeket az alkalmazásokat úgy vannak konfigurálva, az Azure AD portálon a felhasználó nevében.</span><span class="sxs-lookup"><span data-stu-id="5d370-105">These applications are configured on behalf of the user in the Azure AD portal.</span></span> <span data-ttu-id="5d370-106">A rendszergazda közvetlenül hozhat létre az alkalmazás a felhasználó vagy csoport számára a felhasználó, ami azt eredményezi, az alkalmazás a felhasználó hozzáférési Panel szereplő.</span><span class="sxs-lookup"><span data-stu-id="5d370-106">The admin can provision the application to the user directly or to a group a user is part of resulting in the application appearing on the user’s Access Panel.</span></span>

## <a name="general-issues-to-check-first"></a><span data-ttu-id="5d370-107">Először ellenőrizze a általános problémák</span><span class="sxs-lookup"><span data-stu-id="5d370-107">General issues to check first</span></span>

-   <span data-ttu-id="5d370-108">Ha egy alkalmazás imént eltávolították a felhasználót vagy csoportot, a felhasználó tagja, próbáljon meg bejelentkezni és újra a felhasználó hozzáférési panelre néhány perc múlva megjelenítéséhez, ha a rendszer eltávolítja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5d370-108">If an application was just removed from a user or group the user is a member of, try to sign in and out again into the user’s Access Panel after a few minutes to see if the application is removed.</span></span>

-   <span data-ttu-id="5d370-109">Ha a licenc csak felhasználó vagy csoport, a felhasználó nem tagja ennek eltarthat egy ideig, attól függően, hogy méretét és összetettségét, el kell végezni a változásokat a csoport el lett távolítva.</span><span class="sxs-lookup"><span data-stu-id="5d370-109">If a license was just removed from a user or group the user is a member of this may take a long time, depending on the size and complexity of the group for changes to be made.</span></span> <span data-ttu-id="5d370-110">Lehetővé teszi további idő előtt jelentkezik be a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="5d370-110">Allow for extra time before signing into the Access Panel.</span></span>

## <a name="problems-related-to-assigning-applications-to-users"></a><span data-ttu-id="5d370-111">Felhasználók hozzárendelése kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="5d370-111">Problems related to assigning applications to users</span></span>

<span data-ttu-id="5d370-112">Egy felhasználó lehet annak, hogy egy alkalmazás a hozzáférési Panel a mert azok volt korábban hozzárendelt.</span><span class="sxs-lookup"><span data-stu-id="5d370-112">A user may be seeing an application on their Access Panel because they had been previously assigned to it.</span></span> <span data-ttu-id="5d370-113">Az alábbiakban néhány ellenőrizze módjai:</span><span class="sxs-lookup"><span data-stu-id="5d370-113">Below are some ways to check:</span></span>

-   [<span data-ttu-id="5d370-114">Ellenőrizze, hogy ha a felhasználó hozzá van rendelve az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="5d370-114">Check if a user is assigned to the application</span></span>](#check-if-a-user-is-assigned-to-the-application)

-   [<span data-ttu-id="5d370-115">Annak ellenőrzése, hogy a felhasználó a licenc, az alkalmazással kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="5d370-115">Check if a user is under a license related to the application</span></span>](#check-if-a-user-is-under-a-license-related-to-the-application)


### <a name="check-if-a-user-is-assigned-to-the-application"></a><span data-ttu-id="5d370-116">Ellenőrizze, hogy ha a felhasználó hozzá van rendelve az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="5d370-116">Check if a user is assigned to the application</span></span>

<span data-ttu-id="5d370-117">Ellenőrizze, hogy ha a felhasználó hozzá van rendelve az alkalmazást, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="5d370-117">To check if a user is assigned to the application, follow the steps below:</span></span>

1.  <span data-ttu-id="5d370-118">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="5d370-118">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5d370-119">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="5d370-119">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5d370-120">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="5d370-120">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5d370-121">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="5d370-121">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="5d370-122">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="5d370-122">click **All Applications** to view a list of all your applications.</span></span>

6.  <span data-ttu-id="5d370-123">**Keresési** a szóban forgó alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="5d370-123">**Search** for the name of the application in question.</span></span>

7.  <span data-ttu-id="5d370-124">Kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="5d370-124">click **Users and groups**.</span></span>

8.  <span data-ttu-id="5d370-125">Ellenőrizze, hogy ha a felhasználó hozzá van rendelve az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="5d370-125">Check to see if your user is assigned to the application.</span></span>

  * <span data-ttu-id="5d370-126">Ha a felhasználó eltávolítja az alkalmazásból **sorára kattintson** a felhasználó, és válassza ki **törlése**.</span><span class="sxs-lookup"><span data-stu-id="5d370-126">If you want to remove the user from the application, **click the row** of the user and select **delete**.</span></span>

### <a name="check-if-a-user-is-under-a-license-related-to-the-application"></a><span data-ttu-id="5d370-127">Annak ellenőrzése, hogy a felhasználó a licenc, az alkalmazással kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="5d370-127">Check if a user is under a license related to the application</span></span>

<span data-ttu-id="5d370-128">A felhasználó licenc-hozzárendeléseket ellenőrzéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="5d370-128">To check a user’s assigned licenses, follow the steps below:</span></span>

1.  <span data-ttu-id="5d370-129">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="5d370-129">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5d370-130">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="5d370-130">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5d370-131">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="5d370-131">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5d370-132">Kattintson a **felhasználók és csoportok** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="5d370-132">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="5d370-133">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="5d370-133">click **All users**.</span></span>

6.  <span data-ttu-id="5d370-134">**Keresési** érdekli, felhasználó és **sorára kattintson** kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="5d370-134">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="5d370-135">Kattintson a **licencek** megtekintéséhez, amely licencek, a felhasználó jelenleg hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="5d370-135">click **Licenses** to see which licenses the user currently has assigned.</span></span>

   * <span data-ttu-id="5d370-136">Ha a felhasználó hozzá van rendelve egy Office licenc a engedélyezése első fél Office-alkalmazásokat jelennek meg a felhasználó hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="5d370-136">If the user is assigned to an Office license this enable First Party Office applications to appear on the user’s Access Panel.</span></span>

## <a name="problems-related-to-assigning-applications-to-groups"></a><span data-ttu-id="5d370-137">Csoportok alkalmazásoké kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="5d370-137">Problems related to assigning applications to groups</span></span>

<span data-ttu-id="5d370-138">Egy felhasználó lehet annak, hogy egy alkalmazás a hozzáférési Panel a mert, amelyet az alkalmazáshoz rendelt csoportba.</span><span class="sxs-lookup"><span data-stu-id="5d370-138">A user may be seeing an application on their Access Panel because they are part of a group that has been assigned the application.</span></span> <span data-ttu-id="5d370-139">Az alábbiakban néhány ellenőrizze módjai:</span><span class="sxs-lookup"><span data-stu-id="5d370-139">Below are some ways to check:</span></span>

-   [<span data-ttu-id="5d370-140">A felhasználói csoporttagság ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="5d370-140">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

-   [<span data-ttu-id="5d370-141">Annak ellenőrzése, hogy egy felhasználó egy licencet rendelt csoport tagja</span><span class="sxs-lookup"><span data-stu-id="5d370-141">Check if a user is a member of a group assigned to a license</span></span>](#check-if-a-user-is-a-member-of-a-group-assigned-to-a-license)

### <a name="check-a-users-group-memberships"></a><span data-ttu-id="5d370-142">A felhasználói csoporttagság ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="5d370-142">Check a user’s group memberships</span></span>

<span data-ttu-id="5d370-143">Ellenőrizze a csoport tagságát, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="5d370-143">To check a group’s membership, follow the steps below:</span></span>

1.  <span data-ttu-id="5d370-144">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="5d370-144">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5d370-145">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="5d370-145">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5d370-146">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="5d370-146">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5d370-147">Kattintson a **felhasználók és csoportok** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="5d370-147">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="5d370-148">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="5d370-148">click **All users**.</span></span>

6.  <span data-ttu-id="5d370-149">**Keresési** érdekli, felhasználó és **sorára kattintson** kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="5d370-149">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="5d370-150">Kattintson a **csoportok.**</span><span class="sxs-lookup"><span data-stu-id="5d370-150">click **Groups.**</span></span>

8.  <span data-ttu-id="5d370-151">Ellenőrizze, hogy a felhasználók egy csoportjának az alkalmazás része.</span><span class="sxs-lookup"><span data-stu-id="5d370-151">Check to see if your user is part of a Group assigned to the application.</span></span>

   * <span data-ttu-id="5d370-152">Ha a felhasználó a csoportból eltávolítani kívánt **sorára kattintson** a csoport- és select tulajdonosával.</span><span class="sxs-lookup"><span data-stu-id="5d370-152">If you want to remove the user from the group, **click the row** of the group and select delete.</span></span>

### <a name="check-if-a-user-is-a-member-of-a-group-assigned-to-a-license"></a><span data-ttu-id="5d370-153">Annak ellenőrzése, hogy egy felhasználó egy licencet rendelt csoport tagja</span><span class="sxs-lookup"><span data-stu-id="5d370-153">Check if a user is a member of a group assigned to a license</span></span>

1.  <span data-ttu-id="5d370-154">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="5d370-154">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5d370-155">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="5d370-155">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5d370-156">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="5d370-156">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5d370-157">Kattintson a **felhasználók és csoportok** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="5d370-157">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="5d370-158">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="5d370-158">click **All users**.</span></span>

6.  <span data-ttu-id="5d370-159">**Keresési** érdekli, felhasználó és **sorára kattintson** kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="5d370-159">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="5d370-160">Kattintson a **csoportok.**</span><span class="sxs-lookup"><span data-stu-id="5d370-160">click **Groups.**</span></span>

8.  <span data-ttu-id="5d370-161">egy adott csoport sorára kattintson.</span><span class="sxs-lookup"><span data-stu-id="5d370-161">click the row of a specific group.</span></span>

9.  <span data-ttu-id="5d370-162">Kattintson a **licencek** megtekintéséhez, amely licencek, a csoport van rendelve.</span><span class="sxs-lookup"><span data-stu-id="5d370-162">click **Licenses** to see which licenses the group has assigned to it.</span></span>

  * <span data-ttu-id="5d370-163">Ha a csoport egy Office-licencet, ezzel lehetővé teheti a bizonyos első fél Office alkalmazások jelennek meg a felhasználó hozzáférési Panel van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="5d370-163">If the group is assigned to an Office license this may enable certain First Party Office applications to appear on the user’s Access Panel.</span></span>


## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a><span data-ttu-id="5d370-164">Ha ezek a hibaelhárítási lépéseket nem a hárítsa el a problémát</span><span class="sxs-lookup"><span data-stu-id="5d370-164">If these troubleshooting steps do not the resolve the issue</span></span>

<span data-ttu-id="5d370-165">támogatási jegy megnyitása a következő információkat, ha rendelkezésre áll:</span><span class="sxs-lookup"><span data-stu-id="5d370-165">open a support ticket with the following information if available:</span></span>

-   <span data-ttu-id="5d370-166">Megfelelési hiba azonosítója</span><span class="sxs-lookup"><span data-stu-id="5d370-166">Correlation error ID</span></span>

-   <span data-ttu-id="5d370-167">Egyszerű felhasználónév (felhasználó e-mail címe)</span><span class="sxs-lookup"><span data-stu-id="5d370-167">UPN (user email address)</span></span>

-   <span data-ttu-id="5d370-168">Bérlőazonosító</span><span class="sxs-lookup"><span data-stu-id="5d370-168">Tenant ID</span></span>

-   <span data-ttu-id="5d370-169">Böngésző típusa</span><span class="sxs-lookup"><span data-stu-id="5d370-169">Browser type</span></span>

-   <span data-ttu-id="5d370-170">Időzóna és idő/időkeretre során hiba történik.</span><span class="sxs-lookup"><span data-stu-id="5d370-170">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="5d370-171">Fiddler nyomkövetések</span><span class="sxs-lookup"><span data-stu-id="5d370-171">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="5d370-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5d370-172">Next steps</span></span>
[<span data-ttu-id="5d370-173">Alkalmazások kezelése az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="5d370-173">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
