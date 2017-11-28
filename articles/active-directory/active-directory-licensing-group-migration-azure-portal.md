---
title: "Hogyan telepítheti át a egyedi licenccel rendelkező felhasználók az Azure Active Directory csoporthoz |} Microsoft Docs"
description: "Váltás az egyes felhasználói licencek Csoportalapú licencelésre az Azure Active Directoryval"
services: active-directory
keywords: "Az Azure AD-licencelés"
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/05/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6b77dd4e9a6d361a05382397e89b575896fdad84
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-add-licensed-users-to-a-group-for-licensing-in-azure-active-directory"></a><span data-ttu-id="0b4d8-104">A csoport az Azure Active Directory licencelésének licenccel rendelkező felhasználók felvétele</span><span class="sxs-lookup"><span data-stu-id="0b4d8-104">How to add licensed users to a group for licensing in Azure Active Directory</span></span>

<span data-ttu-id="0b4d8-105">Előfordulhat, hogy telepíti a felhasználók számára a "közvetlen hozzárendelés"; keresztül a szervezetek meglévő licencek Ez azt jelenti, hogy használatával PowerShell-parancsfájlok vagy más eszközök egyéni felhasználói licencek hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-105">You may have existing licenses deployed to users in the organizations via “direct assignment”; that is, using PowerShell scripts or other tools to assign individual user licenses.</span></span> <span data-ttu-id="0b4d8-106">Ha azt szeretné, indíthatja Csoportalapú licencelési a szervezet-licencek kezelése, szüksége lesz egy áttelepítési terv zökkenőmentesen lecseréli a meglévő megoldásokat Csoportalapú licencelési.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-106">If you would like to start using group-based licensing to manage licenses in your organization, you will need a migration plan to seamlessly replace existing solutions with group-based licensing.</span></span>

<span data-ttu-id="0b4d8-107">A legtöbb fontos szem előtt tartani, hogy kerülje az olyan helyzet, ahol történő Csoportalapú licencelési hatására a felhasználók ideiglenesen elvesztése aktuálisan hozzárendelt licencét.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-107">The most important thing to keep in mind is that you should avoid a situation where migrating to group-based licensing will result in users temporarily losing their currently assigned licenses.</span></span> <span data-ttu-id="0b4d8-108">Távolítsa el a felhasználók, szolgáltatások és az adataik elveszítenék kockázatát kerülni kell a bármely folyamat, amelynek hatására a licencek eltávolítását.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-108">Any process that may result in removal of licenses should be avoided to remove the risk of users losing access to services and their data.</span></span>

## <a name="recommended-migration-process"></a><span data-ttu-id="0b4d8-109">Ajánlott áttelepítési folyamat</span><span class="sxs-lookup"><span data-stu-id="0b4d8-109">Recommended migration process</span></span>

1. <span data-ttu-id="0b4d8-110">A licenc-hozzárendelést és a felhasználók eltávolításának kezelése automatizálási (például PowerShell) rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-110">You have existing automation (for example, PowerShell) managing license assignment and removal for users.</span></span> <span data-ttu-id="0b4d8-111">Fut, hagyja.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-111">Leave it running as is.</span></span>

2. <span data-ttu-id="0b4d8-112">Hozzon létre egy új licencelési csoportot (vagy döntse el, melyik meglévő csoportokat kívánja használni), és győződjön meg arról, hogy minden felhasználót adnak hozzá tagként szükséges.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-112">Create a new licensing group (or decide which existing groups to use) and make sure that all required users are added as members.</span></span>

3. <span data-ttu-id="0b4d8-113">A szükséges licencek kiosztása ezekhez a csoportokhoz; a cél megfelelően a meglévő automatizációk (például PowerShell) azoknak a felhasználóknak érvényes olyan licencelési állapotban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-113">Assign the required licenses to those groups; your goal should be to reflect the same licensing state your existing automation (for example, PowerShell) is applying to those users.</span></span>

4. <span data-ttu-id="0b4d8-114">Győződjön meg arról, hogy ezeket a csoportokat az összes felhasználó licencek alkalmaztak.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-114">Verify that licenses have been applied to all users in those groups.</span></span> <span data-ttu-id="0b4d8-115">Ehhez a feldolgozási állapotát az egyes csoportok ellenőrzésével, és ehhez ellenőrizze a naplókat.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-115">This can be done by checking the processing state on each group and by checking Audit Logs.</span></span>

  - <span data-ttu-id="0b4d8-116">A licenc részletei megtekintésével helyszíni ellenőrzés egyéni felhasználók számára is.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-116">You can spot check individual users by looking at their license details.</span></span> <span data-ttu-id="0b4d8-117">Látni fogja, hogy rendelkeznek azonos licenccel "közvetlenül" és "örökölt" csoportból.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-117">You will see that they have the same licenses assigned “directly” and “inherited” from groups.</span></span>

  - <span data-ttu-id="0b4d8-118">PowerShell parancsfájl futtatása [győződjön meg arról, hogyan licenceket a felhasználókhoz rendelt](active-directory-licensing-group-advanced.md#use-powershell-to-see-who-has-inherited-and-direct-licenses).</span><span class="sxs-lookup"><span data-stu-id="0b4d8-118">You can run a PowerShell script to [verify how licenses are assigned to users](active-directory-licensing-group-advanced.md#use-powershell-to-see-who-has-inherited-and-direct-licenses).</span></span>

  - <span data-ttu-id="0b4d8-119">A azonos termék rendel hozzá licencet a felhasználóhoz, mindkét közvetlenül és a csoport révén, amikor a felhasználó által felhasznált csak egy-egy licencet.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-119">When the same product license is assigned to the user both directly and through a group, only one license is consumed by the user.</span></span> <span data-ttu-id="0b4d8-120">További licencek ezért az áttelepítéshez szükségesek.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-120">Hence no additional licenses are required to perform migration.</span></span>

5. <span data-ttu-id="0b4d8-121">Győződjön meg arról, hogy nem a licenc-hozzárendeléseivel minden csoport hibás állapotú felhasználók ellenőrzésével sikertelen.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-121">Verify that no license assignments failed by checking each group for users in error state.</span></span> <span data-ttu-id="0b4d8-122">További információkért lásd: [azonosítása és licenc problémák megoldásához a csoport](active-directory-licensing-group-problem-resolution-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="0b4d8-122">For more information, see [Identifying and resolving license problems for a group](active-directory-licensing-group-problem-resolution-azure-portal.md).</span></span>

6. <span data-ttu-id="0b4d8-123">Fontolja meg az eredeti közvetlen hozzárendelések; eltávolítását érdemes lehet fokozatosan, ne a "hullámok", a felhasználók egy részhalmazát eredménye először figyelése.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-123">Consider removing the original direct assignments; you may want to do it gradually, in “waves”, to monitor the outcome on a subset of users first.</span></span>

  <span data-ttu-id="0b4d8-124">Felhasználók sikerült meghagyása az eredeti közvetlen hozzárendelések, de amikor a felhasználók elhagyják a licenccsoportok azok továbbra is megőrzik az eredeti licenc, amely valószínűleg nem szeretné, hogy szeretné.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-124">You could leave the original direct assignments on users, but when the users leave their licensed groups they will still retain the original license, which is possibly not want you want.</span></span>

## <a name="an-example"></a><span data-ttu-id="0b4d8-125">Példa</span><span class="sxs-lookup"><span data-stu-id="0b4d8-125">An example</span></span>

<span data-ttu-id="0b4d8-126">Tudunk 1000 felhasználóval rendelkező szervezeteknél.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-126">We have an organization with 1,000 users.</span></span> <span data-ttu-id="0b4d8-127">Minden felhasználó Enterprise Mobility + Security (EMS) licencet igényelnek.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-127">All users require Enterprise Mobility + Security (EMS) licenses.</span></span> <span data-ttu-id="0b4d8-128">200 felhasználó a pénzügyi részleg és Office 365 nagyvállalati E3 csomag licencek igényel.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-128">200 users are in the Finance Department and require Office 365 Enterprise E3 licenses.</span></span> <span data-ttu-id="0b4d8-129">Tudunk futó helyi hozzáadása és eltávolítása a felhasználók licencek, származnak, és nyissa meg egy PowerShell-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-129">We have a PowerShell script running on premises adding and removing licenses from users as they come and go.</span></span> <span data-ttu-id="0b4d8-130">Cserélje le a parancsfájlt, licencek automatikusan kezeli az Azure AD Csoportalapú licencelési szeretnénk.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-130">We want to replace the script with group-based licensing so licenses are managed automatically by Azure AD.</span></span>

<span data-ttu-id="0b4d8-131">Íme néhány nézhet ki az áttelepítési folyamat:</span><span class="sxs-lookup"><span data-stu-id="0b4d8-131">Here is what the migration process could look like:</span></span>

1. <span data-ttu-id="0b4d8-132">Az EMS-licencet az Azure portál használatával rendelje hozzá a **minden felhasználó** az Azure AD-csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-132">Using the Azure portal assign the EMS license to the **All users** group in Azure AD.</span></span> <span data-ttu-id="0b4d8-133">A E3 licenc hozzárendelése a **pénzügyi részleg** csoportot, amely tartalmazza a szükséges felhasználók.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-133">Assign the E3 license to the **Finance department** group that contains all the required users.</span></span>

2. <span data-ttu-id="0b4d8-134">Az egyes csoportokban ellenőrizze, hogy a licenc-hozzárendelést az összes felhasználó számára befejeződött.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-134">For each group, confirm that license assignment has completed for all users.</span></span> <span data-ttu-id="0b4d8-135">Keresse fel az egyes csoportokhoz, jelölje be a panelt **licencek**, és a feldolgozási állapotának tetején a **licencek** panelen.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-135">Go to the blade for each group, select **Licenses**, and check the processing status at the top of the **Licenses** blade.</span></span>

  - <span data-ttu-id="0b4d8-136">Keressen a "Legújabb licenc módosításai frissültek az összes felhasználóra" megerősítéséhez feldolgozása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-136">Look for “Latest license changes have been applied to all users" to confirm processing has completed.</span></span>

  - <span data-ttu-id="0b4d8-137">Keresse meg felül, amely licencek előfordulhat, hogy nem sikerült hozzárendelt felhasználók kapcsolatos értesítést.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-137">Look for a notification on top about any users for whom licenses may have not been successfully assigned.</span></span> <span data-ttu-id="0b4d8-138">Nem jelenleg egyes felhasználók licenceinek kívül futtatni?</span><span class="sxs-lookup"><span data-stu-id="0b4d8-138">Did we run out of licenses for some users?</span></span> <span data-ttu-id="0b4d8-139">Egyes felhasználók rendelkeznek ütköző licenc, amely megakadályozza, hogy azok csoport licencek öröklődés termékváltozatok?</span><span class="sxs-lookup"><span data-stu-id="0b4d8-139">Do some users have conflicting license SKUs that prevent them from inheriting group licenses?</span></span>

3. <span data-ttu-id="0b4d8-140">Helyszíni egyes felhasználókat győződjön meg arról, hogy rendelkezik-e mind a közvetlen és csoportosítási licencek alkalmazott ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-140">Spot check some users to verify that they have both the direct and group licenses applied.</span></span> <span data-ttu-id="0b4d8-141">Nyissa meg a felhasználó, jelölje be a panelt **licencek**, és vizsgálja meg a licenc állapotát.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-141">Go to the blade for a user, select **Licenses**, and examine the state of licenses.</span></span>

  - <span data-ttu-id="0b4d8-142">Ez az a várt felhasználói állapot áttelepítése során:</span><span class="sxs-lookup"><span data-stu-id="0b4d8-142">This is the expected user state during migration:</span></span>

      ![várt érték user állapota](media/active-directory-licensing-group-migration-azure-portal/expected-user-state.png)

  <span data-ttu-id="0b4d8-144">Ez megerősíti, hogy a felhasználó rendelkezik-e közvetlen és az örökölt licenceket.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-144">This confirms that the user has both direct and inherited licenses.</span></span> <span data-ttu-id="0b4d8-145">Látható, amely mindkét **EMS** és **E3** vannak hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-145">We see that both **EMS** and **E3** are assigned.</span></span>

  - <span data-ttu-id="0b4d8-146">Jelölje ki minden egyes licencet az engedélyezett szolgáltatások részleteit.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-146">Select each license to show details about the enabled services.</span></span> <span data-ttu-id="0b4d8-147">Ennek segítségével ellenőrizze, hogy ha a közvetlen és csoportosítási licencek engedélyezése pontosan a azonos service-csomagokról a felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-147">This can be used to check if the direct and group licenses enable exactly the same service plans for the user.</span></span>

      ![Ellenőrizze a service-csomagokról](media/active-directory-licensing-group-migration-azure-portal/check-service-plans.png)

4. <span data-ttu-id="0b4d8-149">Miután meggyőződött arról, hogy közvetlen és a csoport licencek egyenértékűek, megkezdheti a felhasználók közvetlen licencek eltávolítását.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-149">After confirming that both direct and group licenses are equivalent, you can start removing direct licenses from users.</span></span> <span data-ttu-id="0b4d8-150">Ehhez távolítsa el az egyes felhasználók számára a portálon tesztelést, és futtassa az automatizálási parancsfájlokat azok egyszerre eltávolítani.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-150">You can test this by removing them for individual users in the portal and then run automation scripts to have them removed in bulk.</span></span> <span data-ttu-id="0b4d8-151">Íme egy példa a portálon keresztül a közvetlen licencek ugyanazon felhasználónál.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-151">Here is an example of the same user with the direct licenses removed through the portal.</span></span> <span data-ttu-id="0b4d8-152">Figyelje meg, hogy a licenc állapota változatlan marad, de már nem látható közvetlen hozzárendeléseket.</span><span class="sxs-lookup"><span data-stu-id="0b4d8-152">Notice that the license state remains unchanged, but we no longer see direct assignments.</span></span>

  ![közvetlen licencek eltávolítása](media/active-directory-licensing-group-migration-azure-portal/direct-licenses-removed.png)


## <a name="next-steps"></a><span data-ttu-id="0b4d8-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0b4d8-154">Next steps</span></span>

<span data-ttu-id="0b4d8-155">Más esetekben a Licenckezelés csoportokon keresztül kapcsolatos további tudnivalókért olvassa el</span><span class="sxs-lookup"><span data-stu-id="0b4d8-155">To learn more about other scenarios for license management through groups, read</span></span>

* [<span data-ttu-id="0b4d8-156">Licencek hozzárendelése az Azure Active Directory csoport</span><span class="sxs-lookup"><span data-stu-id="0b4d8-156">Assigning licenses to a group in Azure Active Directory</span></span>](active-directory-licensing-group-assignment-azure-portal.md)
* [<span data-ttu-id="0b4d8-157">Mi az a csoport-alapú licencelése az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="0b4d8-157">What is group-based licensing in Azure Active Directory?</span></span>](active-directory-licensing-whatis-azure-portal.md)
* [<span data-ttu-id="0b4d8-158">Majd azonosítani és megoldani az Azure Active Directory csoport licenc problémák</span><span class="sxs-lookup"><span data-stu-id="0b4d8-158">Identifying and resolving license problems for a group in Azure Active Directory</span></span>](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [<span data-ttu-id="0b4d8-159">Az Azure Active Directory biztonságicsoport-alapú licencelési további helyzeteket is</span><span class="sxs-lookup"><span data-stu-id="0b4d8-159">Azure Active Directory group-based licensing additional scenarios</span></span>](active-directory-licensing-group-advanced.md)
