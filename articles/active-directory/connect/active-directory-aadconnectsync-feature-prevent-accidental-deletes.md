---
title: "Azure AD Connect szinkronizálása: véletlen törlések megakadályozása |} Microsoft Docs"
description: "Ez a témakör az Azure AD Connectben megakadályozása véletlen törlések (véletlen törlések megakadályozása) szolgáltatást ismerteti."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 6b852cb4-2850-40a1-8280-8724081601f7
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: a33fb729cff5007e40820af696cfec823a3ecfde
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-prevent-accidental-deletes"></a><span data-ttu-id="3cad9-103">Az Azure AD Connect szinkronizálása: véletlen törlések megakadályozása</span><span class="sxs-lookup"><span data-stu-id="3cad9-103">Azure AD Connect sync: Prevent accidental deletes</span></span>
<span data-ttu-id="3cad9-104">Ez a témakör az Azure AD Connectben megakadályozása véletlen törlések (véletlen törlések megakadályozása) szolgáltatást ismerteti.</span><span class="sxs-lookup"><span data-stu-id="3cad9-104">This topic describes the prevent accidental deletes (preventing accidental deletions) feature in Azure AD Connect.</span></span>

<span data-ttu-id="3cad9-105">Ha az Azure AD Connect telepítése megakadályozása véletlen törlések alapértelmezés szerint engedélyezve, nem engedélyezi az export-legfeljebb 500 törlést.</span><span class="sxs-lookup"><span data-stu-id="3cad9-105">When installing Azure AD Connect, prevent accidental deletes is enabled by default and configured to not allow an export with more than 500 deletes.</span></span> <span data-ttu-id="3cad9-106">A szolgáltatás célja elleni véletlen konfigurációs és a változásokat a helyszíni címtár, amely hatással lenne a sok felhasználóval és egyéb objektumok.</span><span class="sxs-lookup"><span data-stu-id="3cad9-106">This feature is designed to protect you from accidental configuration changes and changes to your on-premises directory that would affect many users and other objects.</span></span>

## <a name="what-is-prevent-accidental-deletes"></a><span data-ttu-id="3cad9-107">Mi az véletlen törlések megakadályozása</span><span class="sxs-lookup"><span data-stu-id="3cad9-107">What is prevent accidental deletes</span></span>
<span data-ttu-id="3cad9-108">Gyakori helyzetek, amikor megjelenik a sok törlések tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="3cad9-108">Common scenarios when you see many deletes include:</span></span>

* <span data-ttu-id="3cad9-109">Vált [szűrés](active-directory-aadconnectsync-configure-filtering.md) Amennyiben egy teljes [OU](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) vagy [tartomány](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) nincs bejelölve.</span><span class="sxs-lookup"><span data-stu-id="3cad9-109">Changes to [filtering](active-directory-aadconnectsync-configure-filtering.md) where an entire [OU](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) or [domain](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) is unselected.</span></span>
* <span data-ttu-id="3cad9-110">Egy szervezeti egység összes objektum törlődnek.</span><span class="sxs-lookup"><span data-stu-id="3cad9-110">All objects in an OU are deleted.</span></span>
* <span data-ttu-id="3cad9-111">A szervezeti egység összes objektumára, szinkronizálás hatókörén kívül kell tekinteni, új neve.</span><span class="sxs-lookup"><span data-stu-id="3cad9-111">An OU is renamed so all objects in it are considered to be out of scope for synchronization.</span></span>

<span data-ttu-id="3cad9-112">A PowerShell használatával módosíthatja az alapértelmezett érték 500 objektumok használatával `Enable-ADSyncExportDeletionThreshold`.</span><span class="sxs-lookup"><span data-stu-id="3cad9-112">The default value of 500 objects can be changed with PowerShell using `Enable-ADSyncExportDeletionThreshold`.</span></span> <span data-ttu-id="3cad9-113">Ez az érték a szervezet méretének megfelelően konfigurálni kell.</span><span class="sxs-lookup"><span data-stu-id="3cad9-113">You should configure this value to fit the size of your organization.</span></span> <span data-ttu-id="3cad9-114">Mivel a szinkronizálásütemező 30 percenként fut, a értéke 30 percen belül látható törlések száma.</span><span class="sxs-lookup"><span data-stu-id="3cad9-114">Since the sync scheduler runs every 30 minutes, the value is the number of deletes seen within 30 minutes.</span></span>

<span data-ttu-id="3cad9-115">Ha túl sok törli exportálható az Azure AD előkészített, majd az exportálás nem és egy e-mailt ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="3cad9-115">If there are too many deletes staged to be exported to Azure AD, then the export stops and you receive an email like this:</span></span>

![E-mailek véletlen törlések megakadályozása](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/email.png)

> <span data-ttu-id="3cad9-117">*Hello (technikai kapcsolattartó). (Idő), az identitás-szinkronizálási szolgáltatás észlelte, hogy a törlések száma meghaladta a konfigurált törlési küszöbét (szervezet neve). A (szám) teljes objektumok küldtek az identitások szinkronizálására, futtassa a törlésre. Ez elérte vagy túllépte a beállított törlés küszöbértéket (szám) objektum. Igazolnia kell, hogy erősítse meg, hogy a törlés fel kell dolgozni, mielőtt folytatódik a azt. Tekintse meg az e-mailben felsorolt hibákról bővebben megakadályozásához véletlen törlések.*</span><span class="sxs-lookup"><span data-stu-id="3cad9-117">*Hello (technical contact). At (time) the Identity synchronization service detected that the number of deletions exceeded the configured deletion threshold for (organization name). A total of (number) objects were sent for deletion in this Identity synchronization run. This met or exceeded the configured deletion threshold value of (number) objects. We need you to provide confirmation that these deletions should be processed before we will proceed. Please see the preventing accidental deletions for more information about the error listed in this email message.*</span></span>
>
> 

<span data-ttu-id="3cad9-118">Megtekintheti a állapota is `stopped-deletion-threshold-exceeded` mikor célszerű figyelni a **Synchronization Service Managert** az exportált profil felhasználói felülete.</span><span class="sxs-lookup"><span data-stu-id="3cad9-118">You can also see the status `stopped-deletion-threshold-exceeded` when you look in the **Synchronization Service Manager** UI for the Export profile.</span></span>
<span data-ttu-id="3cad9-119">![Szinkronizálás a Service Manager felhasználói felületén véletlen törlések megakadályozása](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/syncservicemanager.png)</span><span class="sxs-lookup"><span data-stu-id="3cad9-119">![Prevent Accidental deletes Sync Service Manager UI](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/syncservicemanager.png)</span></span>

<span data-ttu-id="3cad9-120">Ha ez nem várt, vizsgálja meg, és tegye intézkedéseket.</span><span class="sxs-lookup"><span data-stu-id="3cad9-120">If this was unexpected, then investigate and take corrective actions.</span></span> <span data-ttu-id="3cad9-121">Mely objektumoknak van törölve lesz, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="3cad9-121">To see which objects are about to be deleted, do the following:</span></span>

1. <span data-ttu-id="3cad9-122">Start **szinkronizálási szolgáltatás** a Start menüből.</span><span class="sxs-lookup"><span data-stu-id="3cad9-122">Start **Synchronization Service** from the Start Menu.</span></span>
2. <span data-ttu-id="3cad9-123">Ugrás a **összekötők**.</span><span class="sxs-lookup"><span data-stu-id="3cad9-123">Go to **Connectors**.</span></span>
3. <span data-ttu-id="3cad9-124">Válassza ki az összekötő típusú **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="3cad9-124">Select the Connector with type **Azure Active Directory**.</span></span>
4. <span data-ttu-id="3cad9-125">A **műveletek** jobb, válassza ki a **Összekötőtér keresési**.</span><span class="sxs-lookup"><span data-stu-id="3cad9-125">Under **Actions** to the right, select **Search Connector Space**.</span></span>
5. <span data-ttu-id="3cad9-126">Az előugró alatt **hatókör**, jelölje be **leválasztása óta** , válassza ki a dátuma a múltban használni.</span><span class="sxs-lookup"><span data-stu-id="3cad9-126">In the pop-up under **Scope**, select **Disconnected Since** and pick a time in the past.</span></span> <span data-ttu-id="3cad9-127">Kattintson a **keresési**.</span><span class="sxs-lookup"><span data-stu-id="3cad9-127">Click **Search**.</span></span> <span data-ttu-id="3cad9-128">Ezen a lapon jeleníti meg az összes objektum törölve lesz.</span><span class="sxs-lookup"><span data-stu-id="3cad9-128">This page provides a view of all objects about to be deleted.</span></span> <span data-ttu-id="3cad9-129">Minden elem kattintva kaphat további információt az objektum.</span><span class="sxs-lookup"><span data-stu-id="3cad9-129">By clicking each item, you can get additional information about the object.</span></span> <span data-ttu-id="3cad9-130">Is **oszlop beállítás** lehet a rácsban látható további attribútumok hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="3cad9-130">You can also click **Column Setting** to add additional attributes to be visible in the grid.</span></span>

![Összekötőtér keresése](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/searchcs.png)

<span data-ttu-id="3cad9-132">A törlés van szükség, majd tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="3cad9-132">If all the deletes are desired, then do the following:</span></span>

1. <span data-ttu-id="3cad9-133">Az aktuális törlési küszöbét lekéréséhez futtassa a PowerShell-parancsmagot `Get-ADSyncExportDeletionThreshold`.</span><span class="sxs-lookup"><span data-stu-id="3cad9-133">To retrieve the current deletion threshold, run the PowerShell cmdlet `Get-ADSyncExportDeletionThreshold`.</span></span> <span data-ttu-id="3cad9-134">Adja meg az Azure AD globális rendszergazdai fiókot és jelszót.</span><span class="sxs-lookup"><span data-stu-id="3cad9-134">Provide an Azure AD Global Administrator account and password.</span></span> <span data-ttu-id="3cad9-135">Az alapértelmezett érték 500.</span><span class="sxs-lookup"><span data-stu-id="3cad9-135">The default value is 500.</span></span>
2. <span data-ttu-id="3cad9-136">Ideiglenes tiltsa le a védelmet, és segítségével azokat törli keresztül halad, futtassa a PowerShell-parancsmagot: `Disable-ADSyncExportDeletionThreshold`.</span><span class="sxs-lookup"><span data-stu-id="3cad9-136">To temporarily disable this protection and let those deletes go through, run the PowerShell cmdlet: `Disable-ADSyncExportDeletionThreshold`.</span></span> <span data-ttu-id="3cad9-137">Adja meg az Azure AD globális rendszergazdai fiókot és jelszót.</span><span class="sxs-lookup"><span data-stu-id="3cad9-137">Provide an Azure AD Global Administrator account and password.</span></span>
   <span data-ttu-id="3cad9-138">![Hitelesítő adatok](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/credentials.png)</span><span class="sxs-lookup"><span data-stu-id="3cad9-138">![Credentials](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/credentials.png)</span></span>
3. <span data-ttu-id="3cad9-139">Az Azure Active Directory-összekötő kijelölését, és válassza ki a **futtatása** válassza **exportálása**.</span><span class="sxs-lookup"><span data-stu-id="3cad9-139">With the Azure Active Directory Connector still selected, select the action **Run** and select **Export**.</span></span>
4. <span data-ttu-id="3cad9-140">Kívánja újból engedélyezni a védelmet, futtassa a PowerShell-parancsmagot: `Enable-ADSyncExportDeletionThreshold -DeletionThreshold 500`.</span><span class="sxs-lookup"><span data-stu-id="3cad9-140">To re-enable the protection, run the PowerShell cmdlet: `Enable-ADSyncExportDeletionThreshold -DeletionThreshold 500`.</span></span> <span data-ttu-id="3cad9-141">Cserélje le az első fellépése beolvasása közben az aktuális törlési küszöbét érték 500.</span><span class="sxs-lookup"><span data-stu-id="3cad9-141">Replace 500 with the value you noticed when retrieving the current deletion threshold.</span></span> <span data-ttu-id="3cad9-142">Adja meg az Azure AD globális rendszergazdai fiókot és jelszót.</span><span class="sxs-lookup"><span data-stu-id="3cad9-142">Provide an Azure AD Global Administrator account and password.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3cad9-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3cad9-143">Next steps</span></span>
<span data-ttu-id="3cad9-144">**Áttekintő témakör**</span><span class="sxs-lookup"><span data-stu-id="3cad9-144">**Overview topics**</span></span>

* [<span data-ttu-id="3cad9-145">Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása</span><span class="sxs-lookup"><span data-stu-id="3cad9-145">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="3cad9-146">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="3cad9-146">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
