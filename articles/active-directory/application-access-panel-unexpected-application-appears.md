---
title: "aaaHow alkalmazások jelennek meg a hozzáférési panel hello |} Microsoft Docs"
description: "Ezért az alkalmazás jelenik-e meg a hozzáférési Panel hello hibaelhárítása"
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
ms.openlocfilehash: 14ee732c4ed5260cba878e949cf9d90877aee67e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-applications-appear-on-hello-access-panel"></a><span data-ttu-id="f0227-103">Alkalmazások megjelenésének hello hozzáférési panel</span><span class="sxs-lookup"><span data-stu-id="f0227-103">How applications appear on hello access panel</span></span>

<span data-ttu-id="f0227-104">hello hozzáférési Panel egy webes portál, amely lehetővé teszi a felhasználó munkahelyi vagy iskolai fiókkal az Azure Active Directory (Azure AD) tooview és kezdő felhőalapú alkalmazások, hogy hello Azure AD-rendszergazda rendelkezik hozzáféréssel őket.</span><span class="sxs-lookup"><span data-stu-id="f0227-104">hello Access Panel is a web-based portal which enables a user with a work or school account in Azure Active Directory (Azure AD) tooview and start cloud-based applications that hello Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="f0227-105">Ezeket az alkalmazásokat úgy vannak konfigurálva, hello Azure AD portálon hello felhasználó nevében.</span><span class="sxs-lookup"><span data-stu-id="f0227-105">These applications are configured on behalf of hello user in hello Azure AD portal.</span></span> <span data-ttu-id="f0227-106">Üdvözöljük a rendszergazdákat hello toohello Alkalmazásfelhasználó közvetlenül vagy egy felhasználó része hello felhasználói hozzáférési Panel szereplő hello alkalmazás eredményezve tooa csoportot hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="f0227-106">hello admin can provision hello application toohello user directly or tooa group a user is part of resulting in hello application appearing on hello user’s Access Panel.</span></span>

## <a name="general-issues-toocheck-first"></a><span data-ttu-id="f0227-107">Általános problémák első toocheck</span><span class="sxs-lookup"><span data-stu-id="f0227-107">General issues toocheck first</span></span>

-   <span data-ttu-id="f0227-108">Ha az alkalmazás csak el lett távolítva a felhasználó vagy csoport hello felhasználó tagja, próbálja toosign bejövő és kimenő adatforgalma újra hello felhasználói hozzáférési panelre után néhány perc toosee hello alkalmazás eltávolításakor.</span><span class="sxs-lookup"><span data-stu-id="f0227-108">If an application was just removed from a user or group hello user is a member of, try toosign in and out again into hello user’s Access Panel after a few minutes toosee if hello application is removed.</span></span>

-   <span data-ttu-id="f0227-109">A licenc csak el lett távolítva a felhasználó vagy csoport hello felhasználó esetén tagja ennek attól függően, hogy hello méretét és összetettségét hello csoportot a végzett módosítások toobe hosszú ideig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="f0227-109">If a license was just removed from a user or group hello user is a member of this may take a long time, depending on hello size and complexity of hello group for changes toobe made.</span></span> <span data-ttu-id="f0227-110">Lehetővé teszi további alkalommal jelentkezik be a hozzáférési Panel hello előtt.</span><span class="sxs-lookup"><span data-stu-id="f0227-110">Allow for extra time before signing into hello Access Panel.</span></span>

## <a name="problems-related-tooassigning-applications-toousers"></a><span data-ttu-id="f0227-111">Problémák kapcsolódó tooassigning alkalmazások toousers</span><span class="sxs-lookup"><span data-stu-id="f0227-111">Problems related tooassigning applications toousers</span></span>

<span data-ttu-id="f0227-112">Egy felhasználó lehet annak, hogy egy alkalmazás a hozzáférési Panel a mert azok már korábban kiosztott tooit.</span><span class="sxs-lookup"><span data-stu-id="f0227-112">A user may be seeing an application on their Access Panel because they had been previously assigned tooit.</span></span> <span data-ttu-id="f0227-113">Az alábbiakban néhány módon toocheck:</span><span class="sxs-lookup"><span data-stu-id="f0227-113">Below are some ways toocheck:</span></span>

-   [<span data-ttu-id="f0227-114">Ellenőrizze, hogy ha a felhasználó hozzá van rendelve toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="f0227-114">Check if a user is assigned toohello application</span></span>](#check-if-a-user-is-assigned-to-the-application)

-   [<span data-ttu-id="f0227-115">Annak ellenőrzése, hogy egy felhasználó egy licenc kapcsolódó toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="f0227-115">Check if a user is under a license related toohello application</span></span>](#check-if-a-user-is-under-a-license-related-to-the-application)


### <a name="check-if-a-user-is-assigned-toohello-application"></a><span data-ttu-id="f0227-116">Ellenőrizze, hogy ha a felhasználó hozzá van rendelve toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="f0227-116">Check if a user is assigned toohello application</span></span>

<span data-ttu-id="f0227-117">toocheck a felhasználó hozzá van rendelve a toohello alkalmazást, kövesse az hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f0227-117">toocheck if a user is assigned toohello application, follow hello steps below:</span></span>

1.  <span data-ttu-id="f0227-118">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="f0227-118">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="f0227-119">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="f0227-119">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f0227-120">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="f0227-120">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f0227-121">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="f0227-121">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="f0227-122">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="f0227-122">click **All Applications** tooview a list of all your applications.</span></span>

6.  <span data-ttu-id="f0227-123">**Keresési** a szóban forgó hello alkalmazás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="f0227-123">**Search** for hello name of hello application in question.</span></span>

7.  <span data-ttu-id="f0227-124">Kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="f0227-124">click **Users and groups**.</span></span>

8.  <span data-ttu-id="f0227-125">Ellenőrizze a toosee, ha a felhasználó a toohello alkalmazás hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="f0227-125">Check toosee if your user is assigned toohello application.</span></span>

  * <span data-ttu-id="f0227-126">Ha azt szeretné, hogy tooremove hello felhasználói hello alkalmazásból **hello sorban kattintson** hello felhasználó, és válassza a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="f0227-126">If you want tooremove hello user from hello application, **click hello row** of hello user and select **delete**.</span></span>

### <a name="check-if-a-user-is-under-a-license-related-toohello-application"></a><span data-ttu-id="f0227-127">Annak ellenőrzése, hogy egy felhasználó egy licenc kapcsolódó toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="f0227-127">Check if a user is under a license related toohello application</span></span>

<span data-ttu-id="f0227-128">toocheck a felhasználó hozzárendelt licencek, hajtsa végre hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f0227-128">toocheck a user’s assigned licenses, follow hello steps below:</span></span>

1.  <span data-ttu-id="f0227-129">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="f0227-129">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="f0227-130">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="f0227-130">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f0227-131">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="f0227-131">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f0227-132">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="f0227-132">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="f0227-133">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="f0227-133">click **All users**.</span></span>

6.  <span data-ttu-id="f0227-134">**Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="f0227-134">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="f0227-135">Kattintson a **licencek** toosee licencek hello felhasználóhoz van rendelve.</span><span class="sxs-lookup"><span data-stu-id="f0227-135">click **Licenses** toosee which licenses hello user currently has assigned.</span></span>

   * <span data-ttu-id="f0227-136">Hello felhasználó van hozzárendelve, tooan Office-licencet az első fél Office alkalmazások tooappear ezzel engedélyezi a felhasználó hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="f0227-136">If hello user is assigned tooan Office license this enable First Party Office applications tooappear on hello user’s Access Panel.</span></span>

## <a name="problems-related-tooassigning-applications-toogroups"></a><span data-ttu-id="f0227-137">Problémák kapcsolódó tooassigning alkalmazások toogroups</span><span class="sxs-lookup"><span data-stu-id="f0227-137">Problems related tooassigning applications toogroups</span></span>

<span data-ttu-id="f0227-138">Egy felhasználó lehet annak, hogy egy alkalmazás a hozzáférési Panel a mert hello alkalmazás rendelt csoportba.</span><span class="sxs-lookup"><span data-stu-id="f0227-138">A user may be seeing an application on their Access Panel because they are part of a group that has been assigned hello application.</span></span> <span data-ttu-id="f0227-139">Az alábbiakban néhány módon toocheck:</span><span class="sxs-lookup"><span data-stu-id="f0227-139">Below are some ways toocheck:</span></span>

-   [<span data-ttu-id="f0227-140">A felhasználói csoporttagság ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="f0227-140">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

-   [<span data-ttu-id="f0227-141">Ellenőrizze, hogy a felhasználó tagja egy csoportnak tooa licenc hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="f0227-141">Check if a user is a member of a group assigned tooa license</span></span>](#check-if-a-user-is-a-member-of-a-group-assigned-to-a-license)

### <a name="check-a-users-group-memberships"></a><span data-ttu-id="f0227-142">A felhasználói csoporttagság ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="f0227-142">Check a user’s group memberships</span></span>

<span data-ttu-id="f0227-143">toocheck egy csoport tagságát, hajtsa végre hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f0227-143">toocheck a group’s membership, follow hello steps below:</span></span>

1.  <span data-ttu-id="f0227-144">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="f0227-144">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="f0227-145">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="f0227-145">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f0227-146">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="f0227-146">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f0227-147">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="f0227-147">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="f0227-148">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="f0227-148">click **All users**.</span></span>

6.  <span data-ttu-id="f0227-149">**Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="f0227-149">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="f0227-150">Kattintson a **csoportok.**</span><span class="sxs-lookup"><span data-stu-id="f0227-150">click **Groups.**</span></span>

8.  <span data-ttu-id="f0227-151">Toosee ellenőrizze, hogy a felhasználók egy csoportjának toohello alkalmazás részét.</span><span class="sxs-lookup"><span data-stu-id="f0227-151">Check toosee if your user is part of a Group assigned toohello application.</span></span>

   * <span data-ttu-id="f0227-152">Ha azt szeretné, hogy tooremove hello felhasználói hello csoportból **hello sorban kattintson** hello csoport, és válassza a törlés.</span><span class="sxs-lookup"><span data-stu-id="f0227-152">If you want tooremove hello user from hello group, **click hello row** of hello group and select delete.</span></span>

### <a name="check-if-a-user-is-a-member-of-a-group-assigned-tooa-license"></a><span data-ttu-id="f0227-153">Ellenőrizze, hogy a felhasználó tagja egy csoportnak tooa licenc hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="f0227-153">Check if a user is a member of a group assigned tooa license</span></span>

1.  <span data-ttu-id="f0227-154">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="f0227-154">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="f0227-155">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="f0227-155">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f0227-156">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="f0227-156">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f0227-157">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="f0227-157">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="f0227-158">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="f0227-158">click **All users**.</span></span>

6.  <span data-ttu-id="f0227-159">**Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="f0227-159">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="f0227-160">Kattintson a **csoportok.**</span><span class="sxs-lookup"><span data-stu-id="f0227-160">click **Groups.**</span></span>

8.  <span data-ttu-id="f0227-161">Kattintson egy meghatározott csoport hello sorát.</span><span class="sxs-lookup"><span data-stu-id="f0227-161">click hello row of a specific group.</span></span>

9.  <span data-ttu-id="f0227-162">Kattintson a **licencek** melyik licencek hello csoporthoz rendelt tooit toosee.</span><span class="sxs-lookup"><span data-stu-id="f0227-162">click **Licenses** toosee which licenses hello group has assigned tooit.</span></span>

  * <span data-ttu-id="f0227-163">Ha hello csoport hozzárendelt tooan Office-licencet ezzel előfordulhat, hogy engedélyezi bizonyos első fél Office alkalmazások tooappear hello felhasználói hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="f0227-163">If hello group is assigned tooan Office license this may enable certain First Party Office applications tooappear on hello user’s Access Panel.</span></span>


## <a name="if-these-troubleshooting-steps-do-not-hello-resolve-hello-issue"></a><span data-ttu-id="f0227-164">Ha a lépések nem hello hello probléma megoldásához.</span><span class="sxs-lookup"><span data-stu-id="f0227-164">If these troubleshooting steps do not hello resolve hello issue</span></span>

<span data-ttu-id="f0227-165">Nyisson meg egy támogatási jegy hello ha rendelkezésre áll a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="f0227-165">open a support ticket with hello following information if available:</span></span>

-   <span data-ttu-id="f0227-166">Megfelelési hiba azonosítója</span><span class="sxs-lookup"><span data-stu-id="f0227-166">Correlation error ID</span></span>

-   <span data-ttu-id="f0227-167">Egyszerű felhasználónév (felhasználó e-mail címe)</span><span class="sxs-lookup"><span data-stu-id="f0227-167">UPN (user email address)</span></span>

-   <span data-ttu-id="f0227-168">Bérlőazonosító</span><span class="sxs-lookup"><span data-stu-id="f0227-168">Tenant ID</span></span>

-   <span data-ttu-id="f0227-169">Böngésző típusa</span><span class="sxs-lookup"><span data-stu-id="f0227-169">Browser type</span></span>

-   <span data-ttu-id="f0227-170">Időzóna és idő/időkeretre során hiba történik.</span><span class="sxs-lookup"><span data-stu-id="f0227-170">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="f0227-171">Fiddler nyomkövetések</span><span class="sxs-lookup"><span data-stu-id="f0227-171">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="f0227-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f0227-172">Next steps</span></span>
[<span data-ttu-id="f0227-173">Alkalmazások kezelése az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="f0227-173">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
