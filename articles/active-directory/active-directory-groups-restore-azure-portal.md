---
title: "az Azure Active Directoryban egy törölt Office 365 aaaRestore csoport |} Microsoft Docs"
description: "Hogyan toorestore a törölt csoportban, visszaállítható csoportok megtekintése és permamnently csoport törlése az Azure Active Directoryban"
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
ms.openlocfilehash: 4e6650d64aa19f0c909f1c511f48681de8032f63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restore-a-deleted-office-365-group-in-azure-active-directory"></a><span data-ttu-id="075ac-103">Állítsa vissza egy törölt Office 365-csoport az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="075ac-103">Restore a deleted Office 365 group in Azure Active Directory</span></span>

<span data-ttu-id="075ac-104">Ha töröl egy Office 365 csoportot hello Azure Active Directory (Azure AD), törölt hello csoport származik megtartott, de nem látható 30 napig hello törlés dátuma.</span><span class="sxs-lookup"><span data-stu-id="075ac-104">When you delete an Office 365 group in hello Azure Active Directory (Azure AD), hello deleted group is retained but not visible for 30 days from hello deletion date.</span></span> <span data-ttu-id="075ac-105">Ez az, hogy hello csoportot és annak tartalmát is állítható vissza, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="075ac-105">This is so that hello group and its contents can be restored if needed.</span></span> <span data-ttu-id="075ac-106">Ez a funkció nem használható kizárólag tooOffice 365 csoportok az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="075ac-106">This functionality is restricted exclusively tooOffice 365 groups in Azure AD.</span></span> <span data-ttu-id="075ac-107">Nincs elérhető biztonsági és terjesztési csoportok.</span><span class="sxs-lookup"><span data-stu-id="075ac-107">It is not available for security groups and distribution groups.</span></span>

> [!NOTE] 
> <span data-ttu-id="075ac-108">Ne használjon `Remove-MsolGroup` , mert azt az hello csoport véglegesen kiürítése.</span><span class="sxs-lookup"><span data-stu-id="075ac-108">Don't use `Remove-MsolGroup` because it purges hello group permanently.</span></span> <span data-ttu-id="075ac-109">Mindig `Remove-AzureADMSGroup` toodelete az Office 365-csoport.</span><span class="sxs-lookup"><span data-stu-id="075ac-109">Always use `Remove-AzureADMSGroup` toodelete an O365 group.</span></span> 

<span data-ttu-id="075ac-110">a szükséges hello jogosultságok toorestore csoport hello következő lehet:</span><span class="sxs-lookup"><span data-stu-id="075ac-110">hello permissions required toorestore a group can be any of hello following:</span></span>

<span data-ttu-id="075ac-111">Szerepkör</span><span class="sxs-lookup"><span data-stu-id="075ac-111">Role</span></span>  | <span data-ttu-id="075ac-112">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="075ac-112">Permissions</span></span> 
--------- | ---------
<span data-ttu-id="075ac-113">Vállalati rendszergazda, a Partner Tier2 támogatása és az InTune szolgáltatás-rendszergazdák</span><span class="sxs-lookup"><span data-stu-id="075ac-113">Company Administrator, Partner Tier2 support, and InTune Service Admins</span></span> | <span data-ttu-id="075ac-114">Visszaállíthatja a törölt Office 365-csoport</span><span class="sxs-lookup"><span data-stu-id="075ac-114">Can restore any deleted Office 365 group</span></span> 
<span data-ttu-id="075ac-115">Felhasználói fiók rendszergazdájához, és a Partner Tier1 támogatása</span><span class="sxs-lookup"><span data-stu-id="075ac-115">User Account Administrator and Partner Tier1 support</span></span> | <span data-ttu-id="075ac-116">E hozzárendelt toohello vállalati rendszergazda szerepkört kivéve bármely törölt Office 365-csoport állíthatja vissza.</span><span class="sxs-lookup"><span data-stu-id="075ac-116">Can restore any deleted Office 365 group except those assigned toohello Company Administrator role</span></span> 
<span data-ttu-id="075ac-117">Felhasználó</span><span class="sxs-lookup"><span data-stu-id="075ac-117">User</span></span> | <span data-ttu-id="075ac-118">Bármely törölt Office 365-csoport, amelynek azok tulajdonosa állíthatja vissza.</span><span class="sxs-lookup"><span data-stu-id="075ac-118">Can restore any deleted Office 365 group that they owned</span></span> 


## <a name="view-hello-deleted-office-365-groups-that-are-available-toorestore"></a><span data-ttu-id="075ac-119">Nézet hello Office 365-csoportokat, amelyek a rendelkezésre álló toorestore törlése</span><span class="sxs-lookup"><span data-stu-id="075ac-119">View hello deleted Office 365 groups that are available toorestore</span></span>
<span data-ttu-id="075ac-120">a következő parancsmagok hello lehet használt tooview törölt hello csoportok tooverify egy hello, vagy azokat, továbbra is érdekli nem még véglegesen eltávolításuk.</span><span class="sxs-lookup"><span data-stu-id="075ac-120">hello following cmdlets can be used tooview hello deleted groups tooverify that hello one or ones you're interested in have not yet been permanently purged.</span></span> <span data-ttu-id="075ac-121">Ezek a parancsmagok hello részét képező [Azure AD PowerShell modult](https://www.powershellgallery.com/packages/AzureAD/).</span><span class="sxs-lookup"><span data-stu-id="075ac-121">These cmdlets are part of hello [Azure AD PowerShell module](https://www.powershellgallery.com/packages/AzureAD/).</span></span> <span data-ttu-id="075ac-122">Ez a modul további információ található a hello [Azure Active Directory PowerShell 2-es verzió](/powershell/azure/install-adv2?view=azureadps-2.0) cikk.</span><span class="sxs-lookup"><span data-stu-id="075ac-122">More information about this module can be found in hello [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0) article.</span></span>

1.  <span data-ttu-id="075ac-123">Futtassa a következő parancsmag törli az összes toodisplay Office 365-csoportok az Ön bérlőjében, amelyek továbbra is elérhető toorestore hello.</span><span class="sxs-lookup"><span data-stu-id="075ac-123">Run hello following cmdlet toodisplay all deleted Office 365 groups in your tenant that are still available toorestore.</span></span>
  ```
  Get-AzureADMSDeletedGroup
  ```

2.  <span data-ttu-id="075ac-124">A másik lehetőség Ha tudja, egy adott csoport hello objectID (és lekérheti az 1. lépésben hello parancsmag), futtassa a következő parancsmag tooverify, amelyek adott törölt csoportban hello hello nem még véglegesen törölve lettek.</span><span class="sxs-lookup"><span data-stu-id="075ac-124">Alternately, if you know hello objectID of a specific group (and you can get it from hello cmdlet in step 1), run hello following cmdlet tooverify that hello specific deleted group has not yet been permanently purged.</span></span>
  ```
  Get-AzureADMSDeletedGroup –Id <objectId>
  ```



## <a name="how-toorestore-your-deleted-office-365-group"></a><span data-ttu-id="075ac-125">Hogyan toorestore a törölt Office 365-csoport</span><span class="sxs-lookup"><span data-stu-id="075ac-125">How toorestore your deleted Office 365 group</span></span>
<span data-ttu-id="075ac-126">Miután ellenőrizte, hogy hello csoport továbbra is elérhető toorestore, állítsa vissza a törölt hello csoport hello lépések egyike.</span><span class="sxs-lookup"><span data-stu-id="075ac-126">Once you have verified that hello group is still available toorestore, restore hello deleted group with one of hello following steps.</span></span> <span data-ttu-id="075ac-127">Ha hello csoport tartalmazza a dokumentumok, SP helyek vagy más állandó objektumok, azt is eltarthat too24 óra toofully visszaállítási, egy csoportot és annak tartalmát.</span><span class="sxs-lookup"><span data-stu-id="075ac-127">If hello group contains documents, SP sites, or other persistent objects, it might take up too24 hours toofully restore a group and its contents.</span></span>

1.  <span data-ttu-id="075ac-128">Futtatási hello következő parancsmag toorestore hello csoportot és annak tartalmát.</span><span class="sxs-lookup"><span data-stu-id="075ac-128">Run hello following cmdlet toorestore hello group and its contents.</span></span>
  
  ```
  Restore-AzureADMSDeletedDirectoryObject –Id <objectId>
  ``` 

<span data-ttu-id="075ac-129">Azt is megteheti hello következő parancsmagot futtathatja törölt hello csoport toopermanently eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="075ac-129">Alternatively, hello following cmdlet can be run toopermanently remove hello deleted group.</span></span>
  ```
  Remove-AzureADMSDeletedDirectoryObject –Id <objectId>
  ```

## <a name="how-do-you-know-this-worked"></a><span data-ttu-id="075ac-130">Honnan tudhatja ez dolgozott?</span><span class="sxs-lookup"><span data-stu-id="075ac-130">How do you know this worked?</span></span>
<span data-ttu-id="075ac-131">hogy az Office 365 csoport hello futtatása sikeresen visszaállította már tooverify `Get-AzureADGroup –ObjectId <objectId>` hello csoport parancsmag toodisplay információit.</span><span class="sxs-lookup"><span data-stu-id="075ac-131">tooverify that you’ve successfully restored an Office 365 group, run hello `Get-AzureADGroup –ObjectId <objectId>` cmdlet toodisplay information about hello group.</span></span> <span data-ttu-id="075ac-132">Hello visszaállítás után kérelem teljesítése:</span><span class="sxs-lookup"><span data-stu-id="075ac-132">After hello restore request is completed:</span></span>
- <span data-ttu-id="075ac-133">hello csoport megjelenik hello bal oldali navigációs sávban a Exchange</span><span class="sxs-lookup"><span data-stu-id="075ac-133">hello group will appear in hello Left nav bar on Exchange</span></span>
- <span data-ttu-id="075ac-134">Planner jelennek meg hello csoport hello tervezése</span><span class="sxs-lookup"><span data-stu-id="075ac-134">hello plan for hello group will appear in Planner</span></span>
- <span data-ttu-id="075ac-135">Minden olyan Sharepoint webhelyet, és minden, a tartalom elérhető lesz</span><span class="sxs-lookup"><span data-stu-id="075ac-135">Any Sharepoint sites and all of their contents will be available</span></span>
- <span data-ttu-id="075ac-136">hello csoport is elérhetők, bármelyik hello Exchange végpontok és más Office 365 számítási feladattal, amely támogatja az Office 365-csoportokat</span><span class="sxs-lookup"><span data-stu-id="075ac-136">hello group can be accessed from any of hello Exchange endpoints and other Office 365 workloads that support Office 365 groups</span></span>


## <a name="next-steps"></a><span data-ttu-id="075ac-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="075ac-137">Next steps</span></span>
<span data-ttu-id="075ac-138">Ezek a cikkek további információkkal az Azure Active Directory csoportokat.</span><span class="sxs-lookup"><span data-stu-id="075ac-138">These articles provide additional information on Azure Active Directory groups.</span></span>

* [<span data-ttu-id="075ac-139">Tekintse meg a meglévő csoportok</span><span class="sxs-lookup"><span data-stu-id="075ac-139">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="075ac-140">Egy csoport beállításainak kezelése</span><span class="sxs-lookup"><span data-stu-id="075ac-140">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="075ac-141">A csoport tagjai kezelése</span><span class="sxs-lookup"><span data-stu-id="075ac-141">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="075ac-142">A csoport tagságát kezelése</span><span class="sxs-lookup"><span data-stu-id="075ac-142">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="075ac-143">A csoport dinamikus szabályok kezelése</span><span class="sxs-lookup"><span data-stu-id="075ac-143">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
