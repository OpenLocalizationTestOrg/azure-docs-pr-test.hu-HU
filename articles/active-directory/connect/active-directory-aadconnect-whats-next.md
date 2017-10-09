---
title: "Az Azure AD Connect: További lépések és hogyan toomanage az Azure AD Connect |} Microsoft Docs"
description: "Ismerje meg, hogyan tooextend hello alapértelmezett konfiguráció és a működtetési feladatok az Azure AD Connect."
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
ms.openlocfilehash: 4404aaff24d54d76b83baca3b331a6a250ba4c03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="next-steps-and-how-toomanage-azure-ad-connect"></a><span data-ttu-id="00e7a-103">További lépések és hogyan toomanage az Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="00e7a-103">Next steps and how toomanage Azure AD Connect</span></span>
<span data-ttu-id="00e7a-104">Eljárásokkal hello működési Ez a cikk toocustomize Azure Active Directory (Azure AD) Connect toomeet a szervezete igényét.</span><span class="sxs-lookup"><span data-stu-id="00e7a-104">Use hello operational procedures in this article toocustomize Azure Active Directory (Azure AD) Connect toomeet your organization's needs and requirements.</span></span>  

## <a name="add-additional-sync-admins"></a><span data-ttu-id="00e7a-105">További szinkronizálási rendszergazdák hozzáadása</span><span class="sxs-lookup"><span data-stu-id="00e7a-105">Add additional sync admins</span></span>
<span data-ttu-id="00e7a-106">Alapértelmezés szerint csak hello felhasználó adott hello telepítési és a helyi rendszergazdák képesek toomanage telepítve hello szinkronizálási motor található.</span><span class="sxs-lookup"><span data-stu-id="00e7a-106">By default, only hello user who did hello installation and local admins are able toomanage hello installed sync engine.</span></span> <span data-ttu-id="00e7a-107">A további személyek toobe képes tooaccess hello szinkronizálási motor kezelése, majd keresse meg az ADSyncAdmins nevű a helyi kiszolgálón hello hello csoport és a toothis csoporthoz adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="00e7a-107">For additional people toobe able tooaccess and manage hello sync engine, locate hello group named ADSyncAdmins on hello local server and add them toothis group.</span></span>

## <a name="assign-licenses-tooazure-ad-premium-and-enterprise-mobility-suite-users"></a><span data-ttu-id="00e7a-108">Licencek tooAzure AD Premium és nagyvállalati mobilitási csomag felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="00e7a-108">Assign licenses tooAzure AD Premium and Enterprise Mobility Suite users</span></span>
<span data-ttu-id="00e7a-109">Most, hogy a felhasználók lettek toohello felhő szinkronizálva tooassign kell őket, a felhőalapú alkalmazások, például az Office 365 használatbavétele licenccel.</span><span class="sxs-lookup"><span data-stu-id="00e7a-109">Now that your users have been synchronized toohello cloud, you need tooassign them a license so they can get going with cloud apps such as Office 365.</span></span>

### <a name="tooassign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a><span data-ttu-id="00e7a-110">az Azure AD Premium vagy Enterprise Mobility Suite licenc tooassign</span><span class="sxs-lookup"><span data-stu-id="00e7a-110">tooassign an Azure AD Premium or Enterprise Mobility Suite License</span></span>

1. <span data-ttu-id="00e7a-111">Jelentkezzen be toohello Azure-portálon, rendszergazdaként</span><span class="sxs-lookup"><span data-stu-id="00e7a-111">Sign in toohello Azure portal as an admin.</span></span>
2. <span data-ttu-id="00e7a-112">Hello bal oldalon válassza ki a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="00e7a-112">On hello left, select **Active Directory**.</span></span>
3. <span data-ttu-id="00e7a-113">A hello **Active Directory** lapon, kattintson duplán a megegyező tooset akarja hello felhasználók hello könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="00e7a-113">On hello **Active Directory** page, double-click hello directory that has hello users you want tooset up.</span></span>
4. <span data-ttu-id="00e7a-114">Hello hello directory oldal tetején, válassza ki a **licencek**.</span><span class="sxs-lookup"><span data-stu-id="00e7a-114">At hello top of hello directory page, select **Licenses**.</span></span>
5. <span data-ttu-id="00e7a-115">A hello **licencek** lapon jelölje be **Active Directory Premium** vagy **nagyvállalati mobilitási csomag**, és kattintson a **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="00e7a-115">On hello **Licenses** page, select **Active Directory Premium** or **Enterprise Mobility Suite**, and then click **Assign**.</span></span>
6. <span data-ttu-id="00e7a-116">A hello párbeszédpanelen válassza ki a hello felhasználók tooassign licenceket szeretne, és kattintson a hello pipa ikon toosave hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="00e7a-116">In hello dialog box, select hello users you want tooassign licenses to, and then click hello check mark icon toosave hello changes.</span></span>

## <a name="verify-hello-scheduled-synchronization-task"></a><span data-ttu-id="00e7a-117">Hello ütemezett szinkronizálás task ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="00e7a-117">Verify hello scheduled synchronization task</span></span>
<span data-ttu-id="00e7a-118">A szinkronizálás hello Azure portál toocheck hello állapotát használja.</span><span class="sxs-lookup"><span data-stu-id="00e7a-118">Use hello Azure portal toocheck hello status of a synchronization.</span></span>

### <a name="tooverify-hello-scheduled-synchronization-task"></a><span data-ttu-id="00e7a-119">tooverify hello beütemezett szinkronizálási feladat</span><span class="sxs-lookup"><span data-stu-id="00e7a-119">tooverify hello scheduled synchronization task</span></span>
1. <span data-ttu-id="00e7a-120">Jelentkezzen be toohello Azure-portálon, rendszergazdaként</span><span class="sxs-lookup"><span data-stu-id="00e7a-120">Sign in toohello Azure portal as an admin.</span></span>
2. <span data-ttu-id="00e7a-121">Hello bal oldalon válassza ki a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="00e7a-121">On hello left, select **Active Directory**.</span></span>
3. <span data-ttu-id="00e7a-122">A hello **Active Directory** lapon, kattintson duplán a megegyező tooset akarja hello felhasználók hello könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="00e7a-122">On hello **Active Directory** page, double-click hello directory that has hello users you want tooset up.</span></span>
4. <span data-ttu-id="00e7a-123">Hello hello directory oldal tetején, válassza ki a **címtár-integráció**.</span><span class="sxs-lookup"><span data-stu-id="00e7a-123">At hello top of hello directory page, select **Directory Integration**.</span></span>
5. <span data-ttu-id="00e7a-124">A **helyi active directory integrációja a**, Megjegyzés hello utolsó szinkronizálás.</span><span class="sxs-lookup"><span data-stu-id="00e7a-124">Under **integration with local active directory**, note hello last sync time.</span></span>

<span data-ttu-id="00e7a-125"><center>![Címtár-szinkronizálás ideje](./media/active-directory-aadconnect-whats-next/verify.png)</center></span><span class="sxs-lookup"><span data-stu-id="00e7a-125"><center>![Directory sync time](./media/active-directory-aadconnect-whats-next/verify.png)</center></span></span>

## <a name="start-a-scheduled-synchronization-task"></a><span data-ttu-id="00e7a-126">Egy beütemezett szinkronizálási feladat indítása</span><span class="sxs-lookup"><span data-stu-id="00e7a-126">Start a scheduled synchronization task</span></span>
<span data-ttu-id="00e7a-127">Ha egy szinkronizálási feladat toorun van szüksége, ehhez keresztül hello Azure AD Connect varázsló ismételt futtatásával.</span><span class="sxs-lookup"><span data-stu-id="00e7a-127">If you need toorun a synchronization task, you can do this by running through hello Azure AD Connect wizard again.</span></span>  <span data-ttu-id="00e7a-128">Azure AD hitelesítő adatait kell tooprovide.</span><span class="sxs-lookup"><span data-stu-id="00e7a-128">You need tooprovide your Azure AD credentials.</span></span>  <span data-ttu-id="00e7a-129">Hello varázslóban válassza hello **testre szabhatja a szinkronizálási beállítások** feladat, és kattintson a **következő** toomove hello varázsló használatával.</span><span class="sxs-lookup"><span data-stu-id="00e7a-129">In hello wizard, select hello **Customize synchronization options** task, and click **Next** toomove through hello wizard.</span></span> <span data-ttu-id="00e7a-130">Hello végén győződjön meg arról, hogy hello **hello szinkronizálási folyamat indítása, amint hello kezdeti konfigurálás befejeződik** be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="00e7a-130">At hello end, ensure that hello **Start hello synchronization process as soon as hello initial configuration completes** box is selected.</span></span>

<span data-ttu-id="00e7a-131"><center>![A szinkronizálás megkezdéséhez](./media/active-directory-aadconnect-whats-next/startsynch.png)</center></span><span class="sxs-lookup"><span data-stu-id="00e7a-131"><center>![Start synchronization](./media/active-directory-aadconnect-whats-next/startsynch.png)</center></span></span>

<span data-ttu-id="00e7a-132">Hello Azure AD Connect szinkronizálási szolgáltatás Feladatütemező további információkért lásd: [az Azure AD Connect Feladatütemező](active-directory-aadconnectsync-feature-scheduler.md).</span><span class="sxs-lookup"><span data-stu-id="00e7a-132">For more information on hello Azure AD Connect sync Scheduler, see [Azure AD Connect Scheduler](active-directory-aadconnectsync-feature-scheduler.md).</span></span>

## <a name="additional-tasks-available-in-azure-ad-connect"></a><span data-ttu-id="00e7a-133">Az Azure AD Connectben elérhető további feladatok</span><span class="sxs-lookup"><span data-stu-id="00e7a-133">Additional tasks available in Azure AD Connect</span></span>
<span data-ttu-id="00e7a-134">Az Azure AD Connect a kezdeti telepítés után is mindig hello varázsló ismételt futtatása az Azure AD Connect start lap vagy az asztal helyi hello.</span><span class="sxs-lookup"><span data-stu-id="00e7a-134">After your initial installation of Azure AD Connect, you can always start hello wizard again from hello Azure AD Connect start page or desktop shortcut.</span></span>  <span data-ttu-id="00e7a-135">Megfigyelheti, hogy újra áthaladás hello varázsló lehetőségeket is kínál néhány új további feladatok hello formájában.</span><span class="sxs-lookup"><span data-stu-id="00e7a-135">You will notice that going through hello wizard again provides some new options in hello form of additional tasks.</span></span>  

<span data-ttu-id="00e7a-136">hello következő táblázat a feladatok összefoglalása, és minden feladat rövid leírása.</span><span class="sxs-lookup"><span data-stu-id="00e7a-136">hello following table provides a summary of these tasks and a brief description of each task.</span></span>

![További feladatok listája](./media/active-directory-aadconnect-whats-next/addtasks.png)

| <span data-ttu-id="00e7a-138">További feladatok</span><span class="sxs-lookup"><span data-stu-id="00e7a-138">Additional task</span></span> | <span data-ttu-id="00e7a-139">Leírás</span><span class="sxs-lookup"><span data-stu-id="00e7a-139">Description</span></span> |
| --- | --- |
| <span data-ttu-id="00e7a-140">**Nézet hello választott forgatókönyv**</span><span class="sxs-lookup"><span data-stu-id="00e7a-140">**View hello selected scenario**</span></span> |<span data-ttu-id="00e7a-141">A jelenleg az Azure AD Connect megoldás megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="00e7a-141">View your current Azure AD Connect solution.</span></span>  <span data-ttu-id="00e7a-142">Ez magában foglalja az általános beállítások szinkronizálása könyvtárak, és a szinkronizálási beállítások.</span><span class="sxs-lookup"><span data-stu-id="00e7a-142">This includes general settings, synchronized directories, and sync settings.</span></span> |
| <span data-ttu-id="00e7a-143">**Szinkronizálási beállítások testreszabása**</span><span class="sxs-lookup"><span data-stu-id="00e7a-143">**Customize synchronization options**</span></span> |<span data-ttu-id="00e7a-144">Hello aktuális konfigurációjának módosítása például további Active Directory-erdők toohello konfiguráció hozzáadása, illetve a szinkronizálási beállítások, például felhasználó, csoport, eszköz vagy a jelszóvisszaírás engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="00e7a-144">Change hello current configuration like adding additional Active Directory forests toohello configuration, or enabling sync options such as user, group, device, or password write-back.</span></span> |
| <span data-ttu-id="00e7a-145">**Az átmeneti környezetű üzemmód engedélyezése**</span><span class="sxs-lookup"><span data-stu-id="00e7a-145">**Enable Staging Mode**</span></span> |<span data-ttu-id="00e7a-146">Szakasz információkat, amelyek nem azonnal szinkronizálja, és nem tooAzure AD vagy a helyszíni Active Directory exportált.</span><span class="sxs-lookup"><span data-stu-id="00e7a-146">Stage information that is not immediately synchronized and is not exported tooAzure AD or on-premises Active Directory.</span></span>  <span data-ttu-id="00e7a-147">Ezzel a szolgáltatással mielőtt bekövetkeznének megtekintheti hello szinkronizálások.</span><span class="sxs-lookup"><span data-stu-id="00e7a-147">With this feature, you can preview hello synchronizations before they occur.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="00e7a-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="00e7a-148">Next steps</span></span>
<span data-ttu-id="00e7a-149">További információ [a helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="00e7a-149">Learn more about [integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
