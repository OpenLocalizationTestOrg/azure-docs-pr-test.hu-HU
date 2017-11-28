---
title: "Office 365 aaaPreview csoportosítja az Azure Active Directoryban lejárati |} Microsoft Docs"
description: "Hogyan tooset be az Office 365 lejárati csoportosítja az Azure Active Directoryban (előzetes verzió)"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: a8c99961cff3aad3f4d8b0cc1b2eb8e8a4c9ba95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-office-365-groups-expiration-preview"></a><span data-ttu-id="ff1fb-103">Office 365 csoportok lejárati (előzetes verzió) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ff1fb-103">Configure Office 365 groups expiration (preview)</span></span>

<span data-ttu-id="ff1fb-104">Office 365-csoportok hello életciklusát úgy, hogy kiválasztott Office 365 csoportok lejárati most kezelhetik.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-104">You can now manage hello lifecycle of Office 365 groups by setting expiration for any Office 365 groups that you select.</span></span> <span data-ttu-id="ff1fb-105">A lejárati be van állítva, ha ezeket a csoportokat tulajdonosainak vannak-e hogy az alkalmazás megkéri toorenew csoportjaikra hello csoportok továbbra is szükséges.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-105">Once this expiration is set, owners of those groups are asked toorenew their groups if they still need hello groups.</span></span> <span data-ttu-id="ff1fb-106">Minden Office 365-csoportot, amely nem hosszabbítja meg törlődni fog.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-106">Any Office 365 group that is not renewed will be deleted.</span></span> <span data-ttu-id="ff1fb-107">Minden Office 365 csoport törölt hello csoport tulajdonosainak vagy hello rendszergazda számított 30 napon belül állítható vissza.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-107">Any Office 365 group that was deleted can be restored within 30 days by hello group owners or hello administrator.</span></span>  


> [!NOTE]
> <span data-ttu-id="ff1fb-108">Beállíthatja, hogy csak az Office 365-csoportok lejárati idejét.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-108">You can set expiration for only Office 365 groups.</span></span>
>
> <span data-ttu-id="ff1fb-109">Az Office 365-csoportok lejárati beállításához, hogy be van-e rendelve egy Azure AD Premium-licenc</span><span class="sxs-lookup"><span data-stu-id="ff1fb-109">Setting expiration for O365 groups requires that an Azure AD Premium license is assigned to</span></span>
>   - <span data-ttu-id="ff1fb-110">hello rendszergazda, aki hello bérlő hello lejárati beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ff1fb-110">hello administrator who configures hello expiration settings for hello tenant</span></span>
>   - <span data-ttu-id="ff1fb-111">Ez a beállítás a kijelölt hello csoportok összes tagja</span><span class="sxs-lookup"><span data-stu-id="ff1fb-111">All members of hello groups selected for this setting</span></span>

## <a name="set-office-365-groups-expiration"></a><span data-ttu-id="ff1fb-112">Állítsa be az Office 365 csoportok lejárata</span><span class="sxs-lookup"><span data-stu-id="ff1fb-112">Set Office 365 groups expiration</span></span>

1. <span data-ttu-id="ff1fb-113">Nyissa meg hello [az Azure AD felügyeleti központban](https://aad.portal.azure.com) egy olyan fiókkal, amely az Azure AD-bérlő globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-113">Open hello [Azure AD admin center](https://aad.portal.azure.com) with an account that is a global administrator in your Azure AD tenant.</span></span>

2. <span data-ttu-id="ff1fb-114">Nyissa meg az Azure AD, válassza ki **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-114">Open Azure AD, select **Users and groups**.</span></span>

3. <span data-ttu-id="ff1fb-115">Válassza ki **csoportbeállítások** majd **lejárati** tooopen hello lejárati beállítások.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-115">Select **Group settings** and then select **Expiration** tooopen hello expiration settings.</span></span>
  
  ![Lejárati panel](./media/active-directory-groups-lifecycle-azure-portal/expiration-settings.png)

4. <span data-ttu-id="ff1fb-117">A hello **lejárati** panelen is:</span><span class="sxs-lookup"><span data-stu-id="ff1fb-117">On hello **Expiration** blade, you can:</span></span>

  * <span data-ttu-id="ff1fb-118">Hello csoport élettartamának beállítása napban.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-118">Set hello group lifetime in days.</span></span> <span data-ttu-id="ff1fb-119">Kiválaszthatja az egyik hello beállított értékeket, vagy egy egyéni érték (kell 31 nap vagy több).</span><span class="sxs-lookup"><span data-stu-id="ff1fb-119">You could select one of hello preset values, or a custom value (should be 31 days or more).</span></span> 
  * <span data-ttu-id="ff1fb-120">Adjon meg egy e-mail címet, ahol hello megújítási és lejárati kell értesítéseket küldeni, ha egy csoport már nincs tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-120">Specify an email address where hello renewal and expiration notifications should be sent when a group has no owner.</span></span> 
  * <span data-ttu-id="ff1fb-121">Válassza ki, melyik Office 365-csoportok lejár.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-121">Select which Office 365 groups expire.</span></span> <span data-ttu-id="ff1fb-122">Engedélyezheti a lejárati **összes** Office 365-csoportokat, választhatók ki hello Office 365-csoportok, vagy választja **nincs** letiltása az összes csoport lejárati.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-122">You can enable expiration for **All** Office 365 groups, you can select from among hello Office 365 groups, or you select **None** to disable expiration for all groups.</span></span>
  * <span data-ttu-id="ff1fb-123">Mentse a beállításokat, amikor elkészült, kiválasztásával **mentése**.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-123">Save your settings when you're done by selecting **Save**.</span></span>

<span data-ttu-id="ff1fb-124">Hogyan toodownload és a telepítés hello Microsoft PowerShell modul tooconfigure lejárati PowerShell Office 365-csoportokat, lásd: [Azure Active Directory V2 PowerShell-modul - nyilvános előzetes 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).</span><span class="sxs-lookup"><span data-stu-id="ff1fb-124">For instructions on how toodownload and install hello Microsoft PowerShell module tooconfigure expiration for Office 365 groups via PowerShell, see [Azure Active Directory V2 PowerShell Module - Public Preview Release 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).</span></span>

<span data-ttu-id="ff1fb-125">Például az e-mail értesítések küldését toohello Office 365 csoport tulajdonosainak 30 nap, 15 nap, és 1 nap előzetes tooexpiration hello csoport.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-125">Email notifications such as this one are sent toohello Office 365 group owners 30 days, 15 days, and 1 day prior tooexpiration of hello group.</span></span>

![Lejárati értesítést](./media/active-directory-groups-lifecycle-azure-portal/expiration-notification.png)

<span data-ttu-id="ff1fb-127">hello csoporttulajdonos ezután kijelölhet **megújítási csoport** toorenew Office 365 csoport hello, vagy választhatja **toogroup Ugrás** toosee hello tagok és egyéb részleteit hello csoport.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-127">hello group owner can then select **Renew group** toorenew hello Office 365 group, or can select **Go toogroup** toosee hello members and other details about hello group.</span></span>

<span data-ttu-id="ff1fb-128">Amikor lejár egy csoportot, hello csoport egy nap hello lejárati dátum után törlődik.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-128">When a group expires, hello group is deleted one day after hello expiration date.</span></span> <span data-ttu-id="ff1fb-129">Erre például e-mailben értesítést küldött toohello Office 365 csoport tulajdonosainak értesítheti őket hello lejárati és az Office 365 csoport későbbi törlését.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-129">An email notification such as this one is sent toohello Office 365 group owners informing them about hello expiration and subsequent deletion of their Office 365 group.</span></span>

![Csoport törlése e-mail-értesítések](./media/active-directory-groups-lifecycle-azure-portal/deletion-notification.png)

<span data-ttu-id="ff1fb-131">hello csoport kiválasztásával visszaállítható **visszaállítása** vagy a PowerShell-parancsmagok használatával [visszaállítása egy törölt Office 365 csoport az Azure Active Directoryban] leírtak (https://docs.microsoft.com/azure/active-directory/ Active-Directory-groups-restore-Azure-Portal).</span><span class="sxs-lookup"><span data-stu-id="ff1fb-131">hello group can be restored by selecting **Restore group** or by using PowerShell cmdlets, as described in [Restore a deleted Office 365 group in Azure Active Directory] (https://docs.microsoft.com/azure/active-directory/active-directory-groups-restore-azure-portal).</span></span>
    
<span data-ttu-id="ff1fb-132">Ha most visszaállítása hello csoport tartalmazza a dokumentumok, SharePoint-webhelyeken vagy más állandó objektumok, eltarthat too24 óra toofully visszaállítási hello csoportot és annak tartalmát.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-132">If hello group you're restoring contains documents, SharePoint sites, or other persistent objects, it might take up too24 hours toofully restore hello group and its contents.</span></span>

> [!NOTE]
> * <span data-ttu-id="ff1fb-133">Hello lejárati beállítások telepítésekor előfordulhat néhány olyan csoportot, a régebbi, mint hello lejárati időszak.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-133">When deploying hello expiration settings, there might be some groups that are older than hello expiration window.</span></span> <span data-ttu-id="ff1fb-134">Ezek a csoportok nem lehet azonnal törlődnek, de beállítani lejárati too30 napok száma.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-134">These groups are not be immediately deleted, but are set too30 days until expiration.</span></span> <span data-ttu-id="ff1fb-135">hello első megújítási értesítő e-mailt által kiküldött egy napon belül.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-135">hello first renewal notification email is sent out within a day.</span></span> <span data-ttu-id="ff1fb-136">Például csoport 400 nappal ezelőtt lett létrehozva, és hello lejárati időköz értéke too180 nap.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-136">For example, Group A was created 400 days ago, and hello expiration interval is set too180 days.</span></span> <span data-ttu-id="ff1fb-137">Lejárati beállítások alkalmazásakor a csoport van 30 nap elteltével törlődik, ha hello tulajdonos megújítja azt.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-137">When you apply expiration settings, Group A has 30 days before it is deleted, unless hello owner renews it.</span></span>
> * <span data-ttu-id="ff1fb-138">Ha egy dinamikus csoport törlése és visszaállítása, az új csoport és újra ki van töltve függően toohello szabály látható.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-138">When a dynamic group is deleted and restored, it is seen as a new group and re-populated according toohello rule.</span></span> <span data-ttu-id="ff1fb-139">Ez a folyamat eltarthat too24 órában.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-139">This process might take up too24 hours.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ff1fb-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ff1fb-140">Next steps</span></span>
<span data-ttu-id="ff1fb-141">Ezek a cikkek kiegészítő információt nyújt az Azure Active Directory csoportokat.</span><span class="sxs-lookup"><span data-stu-id="ff1fb-141">These articles provide additional information on Azure AD groups.</span></span>

* [<span data-ttu-id="ff1fb-142">Tekintse meg a meglévő csoportok</span><span class="sxs-lookup"><span data-stu-id="ff1fb-142">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="ff1fb-143">Egy csoport beállításainak kezelése</span><span class="sxs-lookup"><span data-stu-id="ff1fb-143">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="ff1fb-144">A csoport tagjai kezelése</span><span class="sxs-lookup"><span data-stu-id="ff1fb-144">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="ff1fb-145">A csoport tagságát kezelése</span><span class="sxs-lookup"><span data-stu-id="ff1fb-145">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="ff1fb-146">A csoport dinamikus szabályok kezelése</span><span class="sxs-lookup"><span data-stu-id="ff1fb-146">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
