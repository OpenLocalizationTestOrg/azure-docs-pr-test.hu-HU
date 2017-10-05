---
title: "Az Azure AD Connect: További lépések és kezelése az Azure AD Connect |} Microsoft Docs"
description: "Ismerje meg, hogyan terjeszthető ki az alapértelmezett konfiguráció és a működtetési feladatok az Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: c18bee36-aebf-4281-b8fc-3fe14116f1a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: beace24fa00c85a5038a3c39ae8f76af5fd12111
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a><span data-ttu-id="5f4e6-103">Következő lépések és az Azure AD Connect kezelése</span><span class="sxs-lookup"><span data-stu-id="5f4e6-103">Next steps and how to manage Azure AD Connect</span></span>
<span data-ttu-id="5f4e6-104">Ebben a cikkben a műveleti eljárások segítségével testre szabhatja az Azure Active Directory (Azure AD) Csatlakozás a szervezeti igények és követelmények.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-104">Use the operational procedures in this article to customize Azure Active Directory (Azure AD) Connect to meet your organization's needs and requirements.</span></span>  

## <a name="add-additional-sync-admins"></a><span data-ttu-id="5f4e6-105">További szinkronizálási rendszergazdák hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5f4e6-105">Add additional sync admins</span></span>
<span data-ttu-id="5f4e6-106">Alapértelmezés szerint csak a felhasználó adta meg a telepítési és a helyi rendszergazdák képesek a telepített szinkronizálási motor kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-106">By default, only the user who did the installation and local admins are able to manage the installed sync engine.</span></span> <span data-ttu-id="5f4e6-107">A további személyek férhessen hozzá és felügyelhesse a szinkronizálási motor keresse meg a csoport ADSyncAdmins nevű a helyi kiszolgálón, és adja hozzá ezt a csoportot.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-107">For additional people to be able to access and manage the sync engine, locate the group named ADSyncAdmins on the local server and add them to this group.</span></span>

## <a name="assign-licenses-to-azure-ad-premium-and-enterprise-mobility-suite-users"></a><span data-ttu-id="5f4e6-108">Licencek hozzárendelése az Azure AD Premium és nagyvállalati mobilitási csomag számára</span><span class="sxs-lookup"><span data-stu-id="5f4e6-108">Assign licenses to Azure AD Premium and Enterprise Mobility Suite users</span></span>
<span data-ttu-id="5f4e6-109">Most, hogy a felhő szinkronizálta a felhasználókat, rendeljen hozzájuk licencet, a felhőalapú alkalmazások, például az Office 365 használatbavétele szeretné.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-109">Now that your users have been synchronized to the cloud, you need to assign them a license so they can get going with cloud apps such as Office 365.</span></span>

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a><span data-ttu-id="5f4e6-110">Az Azure AD Premium vagy Enterprise Mobility Suite licenc hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="5f4e6-110">To assign an Azure AD Premium or Enterprise Mobility Suite License</span></span>

1. <span data-ttu-id="5f4e6-111">Rendszergazdaként jelentkezzen be az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="5f4e6-111">Sign in to the Azure portal as an admin.</span></span>
2. <span data-ttu-id="5f4e6-112">A bal oldalon válassza az **Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-112">On the left, select **Active Directory**.</span></span>
3. <span data-ttu-id="5f4e6-113">Az a **Active Directory** lapon, kattintson duplán a könyvtár, amely rendelkezik a beállítani kívánt felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-113">On the **Active Directory** page, double-click the directory that has the users you want to set up.</span></span>
4. <span data-ttu-id="5f4e6-114">A könyvtárlap tetején válassza a **Licencek** elemet.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-114">At the top of the directory page, select **Licenses**.</span></span>
5. <span data-ttu-id="5f4e6-115">Az a **licencek** lapon jelölje be **Active Directory Premium** vagy **nagyvállalati mobilitási csomag**, és kattintson a **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-115">On the **Licenses** page, select **Active Directory Premium** or **Enterprise Mobility Suite**, and then click **Assign**.</span></span>
6. <span data-ttu-id="5f4e6-116">A párbeszédpanelen válassza ki azokat a felhasználókat, akikhez licenceket szeretne rendelni, majd kattintson a pipa ikonra a módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-116">In the dialog box, select the users you want to assign licenses to, and then click the check mark icon to save the changes.</span></span>

## <a name="verify-the-scheduled-synchronization-task"></a><span data-ttu-id="5f4e6-117">Az ütemezett szinkronizálás task ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-117">Verify the scheduled synchronization task</span></span>
<span data-ttu-id="5f4e6-118">Az Azure portál segítségével tekintse meg a szinkronizálást.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-118">Use the Azure portal to check the status of a synchronization.</span></span>

### <a name="to-verify-the-scheduled-synchronization-task"></a><span data-ttu-id="5f4e6-119">Az ütemezett szinkronizálás task ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-119">To verify the scheduled synchronization task</span></span>
1. <span data-ttu-id="5f4e6-120">Rendszergazdaként jelentkezzen be az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="5f4e6-120">Sign in to the Azure portal as an admin.</span></span>
2. <span data-ttu-id="5f4e6-121">A bal oldalon válassza az **Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-121">On the left, select **Active Directory**.</span></span>
3. <span data-ttu-id="5f4e6-122">Az a **Active Directory** lapon, kattintson duplán a könyvtár, amely rendelkezik a beállítani kívánt felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-122">On the **Active Directory** page, double-click the directory that has the users you want to set up.</span></span>
4. <span data-ttu-id="5f4e6-123">A címtár lap tetején jelölje ki a **címtár-integráció**.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-123">At the top of the directory page, select **Directory Integration**.</span></span>
5. <span data-ttu-id="5f4e6-124">A **helyi active directory integrációja a**, vegye figyelembe a legutóbbi szinkronizálás ideje.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-124">Under **integration with local active directory**, note the last sync time.</span></span>

<span data-ttu-id="5f4e6-125"><center>![Címtár-szinkronizálás ideje](./media/active-directory-aadconnect-whats-next/verify.png)</center></span><span class="sxs-lookup"><span data-stu-id="5f4e6-125"><center>![Directory sync time](./media/active-directory-aadconnect-whats-next/verify.png)</center></span></span>

## <a name="start-a-scheduled-synchronization-task"></a><span data-ttu-id="5f4e6-126">Egy beütemezett szinkronizálási feladat indítása</span><span class="sxs-lookup"><span data-stu-id="5f4e6-126">Start a scheduled synchronization task</span></span>
<span data-ttu-id="5f4e6-127">Ha szeretne futtatni egy szinkronizálási feladat, ehhez az Azure AD Connect varázsló ismételt futtatásával.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-127">If you need to run a synchronization task, you can do this by running through the Azure AD Connect wizard again.</span></span>  <span data-ttu-id="5f4e6-128">Meg kell adnia az Azure AD hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-128">You need to provide your Azure AD credentials.</span></span>  <span data-ttu-id="5f4e6-129">A varázslóban válassza a **testre szabhatja a szinkronizálási beállítások** feladat, és kattintson a **tovább** helyezhető át, a varázsló használatával.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-129">In the wizard, select the **Customize synchronization options** task, and click **Next** to move through the wizard.</span></span> <span data-ttu-id="5f4e6-130">A végén győződjön meg arról, hogy a **, amint a kezdeti konfigurálás befejeződik, indítsa el a szinkronizálási folyamat** be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-130">At the end, ensure that the **Start the synchronization process as soon as the initial configuration completes** box is selected.</span></span>

<span data-ttu-id="5f4e6-131"><center>![A szinkronizálás megkezdéséhez](./media/active-directory-aadconnect-whats-next/startsynch.png)</center></span><span class="sxs-lookup"><span data-stu-id="5f4e6-131"><center>![Start synchronization](./media/active-directory-aadconnect-whats-next/startsynch.png)</center></span></span>

<span data-ttu-id="5f4e6-132">Az Azure AD Connect szinkronizálási Feladatütemező további információkért lásd: [az Azure AD Connect Feladatütemező](active-directory-aadconnectsync-feature-scheduler.md).</span><span class="sxs-lookup"><span data-stu-id="5f4e6-132">For more information on the Azure AD Connect sync Scheduler, see [Azure AD Connect Scheduler](active-directory-aadconnectsync-feature-scheduler.md).</span></span>

## <a name="additional-tasks-available-in-azure-ad-connect"></a><span data-ttu-id="5f4e6-133">Az Azure AD Connectben elérhető további feladatok</span><span class="sxs-lookup"><span data-stu-id="5f4e6-133">Additional tasks available in Azure AD Connect</span></span>
<span data-ttu-id="5f4e6-134">Az Azure AD Connect a kezdeti telepítés után is mindig a varázsló ismételt futtatása a Azure AD Connect start lap vagy az asztal parancsikonnal.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-134">After your initial installation of Azure AD Connect, you can always start the wizard again from the Azure AD Connect start page or desktop shortcut.</span></span>  <span data-ttu-id="5f4e6-135">Megfigyelheti, hogy a varázsló újra áthaladás lehetőségeket is kínál néhány új további feladatok formájában.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-135">You will notice that going through the wizard again provides some new options in the form of additional tasks.</span></span>  

<span data-ttu-id="5f4e6-136">A következő táblázat a feladatok összefoglalása, és minden feladat rövid leírása.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-136">The following table provides a summary of these tasks and a brief description of each task.</span></span>

![További feladatok listája](./media/active-directory-aadconnect-whats-next/addtasks.png)

| <span data-ttu-id="5f4e6-138">További feladatok</span><span class="sxs-lookup"><span data-stu-id="5f4e6-138">Additional task</span></span> | <span data-ttu-id="5f4e6-139">Leírás</span><span class="sxs-lookup"><span data-stu-id="5f4e6-139">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5f4e6-140">**A választott forgatókönyv megtekintése**</span><span class="sxs-lookup"><span data-stu-id="5f4e6-140">**View the selected scenario**</span></span> |<span data-ttu-id="5f4e6-141">A jelenleg az Azure AD Connect megoldás megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-141">View your current Azure AD Connect solution.</span></span>  <span data-ttu-id="5f4e6-142">Ez magában foglalja az általános beállítások szinkronizálása könyvtárak, és a szinkronizálási beállítások.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-142">This includes general settings, synchronized directories, and sync settings.</span></span> |
| <span data-ttu-id="5f4e6-143">**Szinkronizálási beállítások testreszabása**</span><span class="sxs-lookup"><span data-stu-id="5f4e6-143">**Customize synchronization options**</span></span> |<span data-ttu-id="5f4e6-144">Például további Active Directory-erdők hozzáadása a konfigurációt, vagy a szinkronizálási beállítások, például felhasználó, csoport, eszköz vagy a jelszóvisszaírás engedélyezése a jelenlegi konfiguráció módosítása.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-144">Change the current configuration like adding additional Active Directory forests to the configuration, or enabling sync options such as user, group, device, or password write-back.</span></span> |
| <span data-ttu-id="5f4e6-145">**Az átmeneti környezetű üzemmód engedélyezése**</span><span class="sxs-lookup"><span data-stu-id="5f4e6-145">**Enable Staging Mode**</span></span> |<span data-ttu-id="5f4e6-146">Szakasz információkat, amelyek nem azonnal szinkronizálja, és nem az Azure ad Szolgáltatásba exportálni vagy a helyszíni Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-146">Stage information that is not immediately synchronized and is not exported to Azure AD or on-premises Active Directory.</span></span>  <span data-ttu-id="5f4e6-147">Ezzel a szolgáltatással mielőtt bekövetkeznének megtekintheti a szinkronizálást.</span><span class="sxs-lookup"><span data-stu-id="5f4e6-147">With this feature, you can preview the synchronizations before they occur.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="5f4e6-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5f4e6-148">Next steps</span></span>
<span data-ttu-id="5f4e6-149">További információ [a helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="5f4e6-149">Learn more about [integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
