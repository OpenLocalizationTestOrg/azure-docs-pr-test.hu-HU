---
title: "Állítsa vissza az Azure Active Directory törölt Office 365 csoport |} Microsoft Docs"
description: "Hogyan lehet visszaállítani a törölt csoportban, visszaállítható csoportok megtekintése és az Azure Active Directory csoport permamnently törlése"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: cc5f232a-1e77-45c2-b28b-1fcb4621725c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: curtand
ms.openlocfilehash: 32d36ae96797a59bb47bf782ab038f21f231f046
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="restore-a-deleted-office-365-group-in-azure-active-directory"></a><span data-ttu-id="0ad8b-103">Állítsa vissza egy törölt Office 365-csoport az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="0ad8b-103">Restore a deleted Office 365 group in Azure Active Directory</span></span>

<span data-ttu-id="0ad8b-104">Ha töröl egy Office 365-csoport az Azure Active Directory (Azure AD), a törölt csoportban megtartott, de nem látható 30 napig a törlés dátumát.</span><span class="sxs-lookup"><span data-stu-id="0ad8b-104">When you delete an Office 365 group in the Azure Active Directory (Azure AD), the deleted group is retained but not visible for 30 days from the deletion date.</span></span> <span data-ttu-id="0ad8b-105">Ez az, hogy a csoport és annak tartalmát is állítható vissza, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="0ad8b-105">This is so that the group and its contents can be restored if needed.</span></span> <span data-ttu-id="0ad8b-106">Ez a funkció korlátozott kizárólag Office 365-csoportok az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="0ad8b-106">This functionality is restricted exclusively to Office 365 groups in Azure AD.</span></span> <span data-ttu-id="0ad8b-107">Nincs elérhető biztonsági és terjesztési csoportok.</span><span class="sxs-lookup"><span data-stu-id="0ad8b-107">It is not available for security groups and distribution groups.</span></span>

> [!NOTE] 
> <span data-ttu-id="0ad8b-108">Ne használjon `Remove-MsolGroup` mert véglegesen üríti azt a csoportot.</span><span class="sxs-lookup"><span data-stu-id="0ad8b-108">Don't use `Remove-MsolGroup` because it purges the group permanently.</span></span> <span data-ttu-id="0ad8b-109">Mindig `Remove-AzureADMSGroup` az Office 365-csoport törléséhez.</span><span class="sxs-lookup"><span data-stu-id="0ad8b-109">Always use `Remove-AzureADMSGroup` to delete an O365 group.</span></span> 

<span data-ttu-id="0ad8b-110">Egy csoport visszaállításához szükséges engedélyek a következő lehet:</span><span class="sxs-lookup"><span data-stu-id="0ad8b-110">The permissions required to restore a group can be any of the following:</span></span>

<span data-ttu-id="0ad8b-111">Szerepkör</span><span class="sxs-lookup"><span data-stu-id="0ad8b-111">Role</span></span>  | <span data-ttu-id="0ad8b-112">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="0ad8b-112">Permissions</span></span> 
--------- | ---------
<span data-ttu-id="0ad8b-113">Vállalati rendszergazda, a Partner Tier2 támogatása és az InTune szolgáltatás-rendszergazdák</span><span class="sxs-lookup"><span data-stu-id="0ad8b-113">Company Administrator, Partner Tier2 support, and InTune Service Admins</span></span> | <span data-ttu-id="0ad8b-114">Visszaállíthatja a törölt Office 365-csoport</span><span class="sxs-lookup"><span data-stu-id="0ad8b-114">Can restore any deleted Office 365 group</span></span> 
<span data-ttu-id="0ad8b-115">Felhasználói fiók rendszergazdájához, és a Partner Tier1 támogatása</span><span class="sxs-lookup"><span data-stu-id="0ad8b-115">User Account Administrator and Partner Tier1 support</span></span> | <span data-ttu-id="0ad8b-116">A vállalati rendszergazda szerepkörhöz hozzárendelt kivételével minden törölt Office 365 csoport állíthatja vissza.</span><span class="sxs-lookup"><span data-stu-id="0ad8b-116">Can restore any deleted Office 365 group except those assigned to the Company Administrator role</span></span> 
<span data-ttu-id="0ad8b-117">Felhasználó</span><span class="sxs-lookup"><span data-stu-id="0ad8b-117">User</span></span> | <span data-ttu-id="0ad8b-118">Bármely törölt Office 365-csoport, amelynek azok tulajdonosa állíthatja vissza.</span><span class="sxs-lookup"><span data-stu-id="0ad8b-118">Can restore any deleted Office 365 group that they owned</span></span> 


## <a name="view-the-deleted-office-365-groups-that-are-available-to-restore"></a><span data-ttu-id="0ad8b-119">A törölt Office 365-csoportokat, amelyek segítségével megtekintése</span><span class="sxs-lookup"><span data-stu-id="0ad8b-119">View the deleted Office 365 groups that are available to restore</span></span>
<span data-ttu-id="0ad8b-120">Az alábbi parancsmagok segítségével ellenőrizheti, hogy az egy vagy érdekli a meglévők közül nem még véglegesen eltávolításuk törölt csoportok megtekintése.</span><span class="sxs-lookup"><span data-stu-id="0ad8b-120">The following cmdlets can be used to view the deleted groups to verify that the one or ones you're interested in have not yet been permanently purged.</span></span> <span data-ttu-id="0ad8b-121">Ezek a parancsmagok részét képezik a [Azure AD PowerShell modult](https://www.powershellgallery.com/packages/AzureAD/).</span><span class="sxs-lookup"><span data-stu-id="0ad8b-121">These cmdlets are part of the [Azure AD PowerShell module](https://www.powershellgallery.com/packages/AzureAD/).</span></span> <span data-ttu-id="0ad8b-122">Ez a modul kapcsolatos további információk találhatók a [Azure Active Directory PowerShell 2-es verzió](/powershell/azure/install-adv2?view=azureadps-2.0) cikk.</span><span class="sxs-lookup"><span data-stu-id="0ad8b-122">More information about this module can be found in the [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0) article.</span></span>

1.  <span data-ttu-id="0ad8b-123">Futtassa az alábbi parancsmagot, az összes törölt Office 365-csoportok megjelennek a bérlő visszaállítása továbbra is elérhető.</span><span class="sxs-lookup"><span data-stu-id="0ad8b-123">Run the following cmdlet to display all deleted Office 365 groups in your tenant that are still available to restore.</span></span>
  ```
  Get-AzureADMSDeletedGroup
  ```

2.  <span data-ttu-id="0ad8b-124">A másik lehetőség Ha tudja, egy adott csoport objektumazonosító (és lekérheti a parancsmag az 1. lépésben), futtassa az alábbi parancsmagot, ellenőrizheti, hogy az adott törölt csoportban nem még véglegesen törölve lettek.</span><span class="sxs-lookup"><span data-stu-id="0ad8b-124">Alternately, if you know the objectID of a specific group (and you can get it from the cmdlet in step 1), run the following cmdlet to verify that the specific deleted group has not yet been permanently purged.</span></span>
  ```
  Get-AzureADMSDeletedGroup –Id <objectId>
  ```



## <a name="how-to-restore-your-deleted-office-365-group"></a><span data-ttu-id="0ad8b-125">Hogyan lehet visszaállítani a törölt Office 365-csoport</span><span class="sxs-lookup"><span data-stu-id="0ad8b-125">How to restore your deleted Office 365 group</span></span>
<span data-ttu-id="0ad8b-126">Miután ellenőrizte, hogy a csoport visszaállítása továbbra is elérhető, állítsa vissza a törölt csoportban található, az alábbi lépések egyikét.</span><span class="sxs-lookup"><span data-stu-id="0ad8b-126">Once you have verified that the group is still available to restore, restore the deleted group with one of the following steps.</span></span> <span data-ttu-id="0ad8b-127">A csoport tartalmazza a dokumentumok, SP helyek vagy más állandó objektumok, ha egy csoportot és annak tartalmát teljesen visszaállítására akár 24 óráig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="0ad8b-127">If the group contains documents, SP sites, or other persistent objects, it might take up to 24 hours to fully restore a group and its contents.</span></span>

1.  <span data-ttu-id="0ad8b-128">Futtassa az alábbi parancsmagot visszaállítása a csoportot és annak tartalmát.</span><span class="sxs-lookup"><span data-stu-id="0ad8b-128">Run the following cmdlet to restore the group and its contents.</span></span>
  
  ```
  Restore-AzureADMSDeletedDirectoryObject –Id <objectId>
  ``` 

<span data-ttu-id="0ad8b-129">Alternatív megoldásként a következő parancsmag futtatásával végleg eltávolítani a törölt csoportban.</span><span class="sxs-lookup"><span data-stu-id="0ad8b-129">Alternatively, the following cmdlet can be run to permanently remove the deleted group.</span></span>
  ```
  Remove-AzureADMSDeletedDirectoryObject –Id <objectId>
  ```

## <a name="how-do-you-know-this-worked"></a><span data-ttu-id="0ad8b-130">Honnan tudhatja ez dolgozott?</span><span class="sxs-lookup"><span data-stu-id="0ad8b-130">How do you know this worked?</span></span>
<span data-ttu-id="0ad8b-131">Győződjön meg arról, hogy sikeresen már visszaállította az Office 365-csoportok, futtassa a `Get-AzureADGroup –ObjectId <objectId>` parancsmag használatával jelenítse meg a-csoport információit.</span><span class="sxs-lookup"><span data-stu-id="0ad8b-131">To verify that you’ve successfully restored an Office 365 group, run the `Get-AzureADGroup –ObjectId <objectId>` cmdlet to display information about the group.</span></span> <span data-ttu-id="0ad8b-132">A visszaállítás után kérelem teljesítése:</span><span class="sxs-lookup"><span data-stu-id="0ad8b-132">After the restore request is completed:</span></span>
- <span data-ttu-id="0ad8b-133">A csoport megjelenik a bal oldali navigációs sávban a Exchange</span><span class="sxs-lookup"><span data-stu-id="0ad8b-133">The group will appear in the Left nav bar on Exchange</span></span>
- <span data-ttu-id="0ad8b-134">Planner jelennek meg a csoport tervezése</span><span class="sxs-lookup"><span data-stu-id="0ad8b-134">The plan for the group will appear in Planner</span></span>
- <span data-ttu-id="0ad8b-135">Minden olyan Sharepoint webhelyet, és minden, a tartalom elérhető lesz</span><span class="sxs-lookup"><span data-stu-id="0ad8b-135">Any Sharepoint sites and all of their contents will be available</span></span>
- <span data-ttu-id="0ad8b-136">A csoport érhetők el az Exchange-végpontok és más Office 365 számítási feladattal, amely támogatja az Office 365-csoportokat</span><span class="sxs-lookup"><span data-stu-id="0ad8b-136">The group can be accessed from any of the Exchange endpoints and other Office 365 workloads that support Office 365 groups</span></span>


## <a name="next-steps"></a><span data-ttu-id="0ad8b-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0ad8b-137">Next steps</span></span>
<span data-ttu-id="0ad8b-138">Ezek a cikkek további információkkal az Azure Active Directory csoportokat.</span><span class="sxs-lookup"><span data-stu-id="0ad8b-138">These articles provide additional information on Azure Active Directory groups.</span></span>

* [<span data-ttu-id="0ad8b-139">Tekintse meg a meglévő csoportok</span><span class="sxs-lookup"><span data-stu-id="0ad8b-139">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="0ad8b-140">Egy csoport beállításainak kezelése</span><span class="sxs-lookup"><span data-stu-id="0ad8b-140">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="0ad8b-141">A csoport tagjai kezelése</span><span class="sxs-lookup"><span data-stu-id="0ad8b-141">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="0ad8b-142">A csoport tagságát kezelése</span><span class="sxs-lookup"><span data-stu-id="0ad8b-142">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="0ad8b-143">A csoport dinamikus szabályok kezelése</span><span class="sxs-lookup"><span data-stu-id="0ad8b-143">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
