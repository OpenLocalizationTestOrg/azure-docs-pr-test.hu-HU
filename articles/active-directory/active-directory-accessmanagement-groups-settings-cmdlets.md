---
title: "Azure Active Directory-parancsmagok használatával aaaConfigure beállítások |} Microsoft Docs"
description: "Hogyan kezelheti az Azure Active Directory-parancsmagok használatával csoportok hello beállításait"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 9f2090e6-3af4-4f07-bbb2-1d18dae89b73
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro;
ms.openlocfilehash: 46db49d9dec3eaa41c97ca74c57854189eddc16d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a><span data-ttu-id="316fb-103">Azure Active Directory-parancsmagok csoportbeállítások konfigurálásához</span><span class="sxs-lookup"><span data-stu-id="316fb-103">Azure Active Directory cmdlets for configuring group settings</span></span>

> [!IMPORTANT]
> <span data-ttu-id="316fb-104">Ez a tartalom csak tooOffice 365 csoportok érvényes.</span><span class="sxs-lookup"><span data-stu-id="316fb-104">This content applies only tooOffice 365 groups.</span></span> <span data-ttu-id="316fb-105">További információt a tooallow felhasználók toocreate biztonsági csoportok beállítása `Set-MSOLCompanySettings -UsersPermissionToCreateGroupsEnabled $True` leírtak [Set-MSOLCompanySettings](https://docs.microsoft.com/en-us/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).</span><span class="sxs-lookup"><span data-stu-id="316fb-105">For more information on how tooallow users toocreate Security groups, set `Set-MSOLCompanySettings -UsersPermissionToCreateGroupsEnabled $True` as described in [Set-MSOLCompanySettings](https://docs.microsoft.com/en-us/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).</span></span> 

<span data-ttu-id="316fb-106">Az Office 365 csoportok beállításai a beállítási objektumot és egy SettingsTemplate objektum használatával.</span><span class="sxs-lookup"><span data-stu-id="316fb-106">Office 365 Groups settings are configured using a Settings object and a SettingsTemplate object.</span></span> <span data-ttu-id="316fb-107">Kezdetben nem lát beállítások objektumokat a könyvtárban, mert a könyvtár hello alapértelmezett beállításokkal van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="316fb-107">Initially, you don't see any Settings objects in your directory, because your directory is configured with hello default settings.</span></span> <span data-ttu-id="316fb-108">toochange hello alapértelmezett beállításokat, beállítások sablon használatával új beállítási objektumot kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="316fb-108">toochange hello default settings, you must create a new settings object using a settings template.</span></span> <span data-ttu-id="316fb-109">Beállítások sablonok Microsoft határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="316fb-109">Settings templates are defined by Microsoft.</span></span> <span data-ttu-id="316fb-110">Nincsenek számos különböző beállításokat sablont.</span><span class="sxs-lookup"><span data-stu-id="316fb-110">There are several different settings templates.</span></span> <span data-ttu-id="316fb-111">Office 365 tooconfigure csoport beállításait a címtárban, "Group.Unified" nevű hello sablont használ.</span><span class="sxs-lookup"><span data-stu-id="316fb-111">tooconfigure Office 365 group settings for your directory, you use hello template named "Group.Unified".</span></span> <span data-ttu-id="316fb-112">egy különálló csoportot, Office 365 tooconfigure csoportjára vonatkozó beállításait használja a "Group.Unified.Guest" nevű hello sablont.</span><span class="sxs-lookup"><span data-stu-id="316fb-112">tooconfigure Office 365 group settings on a single group, use hello template named "Group.Unified.Guest".</span></span> <span data-ttu-id="316fb-113">Ez a sablon használt toomanage Vendég hozzáférési tooan Office 365 csoport.</span><span class="sxs-lookup"><span data-stu-id="316fb-113">This template is used toomanage guest access tooan Office 365 group.</span></span> 

<span data-ttu-id="316fb-114">hello parancsmagok hello Azure Active Directory PowerShell V2 modulja részét képezik.</span><span class="sxs-lookup"><span data-stu-id="316fb-114">hello cmdlets are part of hello Azure Active Directory PowerShell V2 module.</span></span> <span data-ttu-id="316fb-115">Hogyan toodownload és hello-modul telepítése a számítógépre, lásd: hello cikk [Azure Active Directory PowerShell 2-es verzió](https://docs.microsoft.com/powershell/azuread/).</span><span class="sxs-lookup"><span data-stu-id="316fb-115">For instructions how toodownload and install hello module on your computer, see hello article [Azure Active Directory PowerShell Version 2](https://docs.microsoft.com/powershell/azuread/).</span></span> <span data-ttu-id="316fb-116">Hello modulnak a 2-es verziójú kiadásában hello telepítése [hello PowerShell-galériában](https://www.powershellgallery.com/packages/AzureAD/).</span><span class="sxs-lookup"><span data-stu-id="316fb-116">You can install hello version 2 release of hello module from [hello PowerShell gallery](https://www.powershellgallery.com/packages/AzureAD/).</span></span>

## <a name="retrieve-a-specific-settings-value"></a><span data-ttu-id="316fb-117">Egy adott beállítás értékének beolvasása</span><span class="sxs-lookup"><span data-stu-id="316fb-117">Retrieve a specific settings value</span></span>
<span data-ttu-id="316fb-118">Ha tudja hello nevét beállítás hello tooretrieve, használhatja a hello parancsmag tooretrieve hello aktuális beállítások érték alá.</span><span class="sxs-lookup"><span data-stu-id="316fb-118">If you know hello name of hello setting you want tooretrieve, you can use hello below cmdlet tooretrieve hello current settings value.</span></span> <span data-ttu-id="316fb-119">Ebben a példában a "UsageGuidelinesUrl." nevű beállítás értékét hello azt olvas</span><span class="sxs-lookup"><span data-stu-id="316fb-119">In this example, we're retrieving hello value for a setting named "UsageGuidelinesUrl."</span></span> <span data-ttu-id="316fb-120">Címtár-beállítások és a nevek kapcsolatos további le a cikkben olvasható.</span><span class="sxs-lookup"><span data-stu-id="316fb-120">You can read more about directory settings and their names further down in this article.</span></span>

```powershell
(Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value UsageGuidelinesUrl -EQ
```

## <a name="create-settings-at-hello-directory-level"></a><span data-ttu-id="316fb-121">Hello könyvtár szintjén beállítások létrehozása</span><span class="sxs-lookup"><span data-stu-id="316fb-121">Create settings at hello directory level</span></span>
<span data-ttu-id="316fb-122">Ezeket a lépéseket beállítások létrehozása könyvtár szintjén tooall Office 365 (egyesített) csoportok a hello directory alkalmazandó.</span><span class="sxs-lookup"><span data-stu-id="316fb-122">These steps create settings at directory level, which apply tooall Office 365 groups (Unified groups) in hello directory.</span></span>

1. <span data-ttu-id="316fb-123">Hello DirectorySettings parancsmagok meg kell adnia azt szeretné, hogy toouse SettingsTemplate hello hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="316fb-123">In hello DirectorySettings cmdlets, you must specify hello ID of hello SettingsTemplate you want toouse.</span></span> <span data-ttu-id="316fb-124">Ha nem ismeri ezt az Azonosítót, a parancsmag minden beállítások sablonok hello listáját adja vissza:</span><span class="sxs-lookup"><span data-stu-id="316fb-124">If you do not know this ID, this cmdlet returns hello list of all settings templates:</span></span>
  
  ```
  PS C:> Get-AzureADDirectorySettingTemplate
  ```
  <span data-ttu-id="316fb-125">Ez a parancsmag hívás elérhető összes sablonok adja vissza:</span><span class="sxs-lookup"><span data-stu-id="316fb-125">This cmdlet call returns all templates that are available:</span></span>
  
  ```
  Id                                   DisplayName         Description
  --                                   -----------         -----------
  62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified       ...
  08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest Settings for a specific Unified Group
  16933506-8a8d-4f0d-ad58-e1db05a5b929 Company.BuiltIn     Setting templates define hello different settings that can be used for hello associ...
  4bc7f740-180e-4586-adb6-38b2e9024e6b Application...
  898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy       Settings ...
  5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule       Settings ...
  ```
2. <span data-ttu-id="316fb-126">tooadd használati iránymutatás URL-címet, először meg kell tooget hello SettingsTemplate objektum, amely meghatározza a hello használati iránymutatás URL-érték; Ez azt jelenti, hogy hello Group.Unified sablont:</span><span class="sxs-lookup"><span data-stu-id="316fb-126">tooadd a usage guideline URL, first you need tooget hello SettingsTemplate object that defines hello usage guideline URL value; that is, hello Group.Unified template:</span></span>
  
  ```
  $Template = Get-AzureADDirectorySettingTemplate -Id 62375ab9-6b52-47ed-826b-58e47e0e304b
  ```
3. <span data-ttu-id="316fb-127">Ezután hozzon létre egy új beállítási objektumot, hogy a sablon alapján:</span><span class="sxs-lookup"><span data-stu-id="316fb-127">Next, create a new settings object based on that template:</span></span>
  
  ```
  $Setting = $template.CreateDirectorySetting()
  ```  
4. <span data-ttu-id="316fb-128">Ezután frissítse hello használati iránymutatás értéket:</span><span class="sxs-lookup"><span data-stu-id="316fb-128">Then update hello usage guideline value:</span></span>
  
  ```
  $setting["UsageGuidelinesUrl"] = "<https://guideline.com>"
  ```  
5. <span data-ttu-id="316fb-129">Végezetül a hello-beállítások alkalmazásához:</span><span class="sxs-lookup"><span data-stu-id="316fb-129">Finally, apply hello settings:</span></span>
  
  ```
  New-AzureADDirectorySetting -DirectorySetting $setting
  ```

<span data-ttu-id="316fb-130">Sikeres befejezését követően hello parancsmag hello hello új beállítások objektum Azonosítóját adja vissza:</span><span class="sxs-lookup"><span data-stu-id="316fb-130">Upon successful completion, hello cmdlet returns hello ID of hello new settings object:</span></span>
  ```
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323             62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```
<span data-ttu-id="316fb-131">Az alábbiakban meghatározott hello Group.Unified SettingsTemplate hello-beállítások.</span><span class="sxs-lookup"><span data-stu-id="316fb-131">Here are hello settings defined in hello Group.Unified SettingsTemplate.</span></span>

| <span data-ttu-id="316fb-132">**Beállítás**</span><span class="sxs-lookup"><span data-stu-id="316fb-132">**Setting**</span></span> | <span data-ttu-id="316fb-133">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="316fb-133">**Description**</span></span> |
| --- | --- |
|  <ul><li><span data-ttu-id="316fb-134">EnableGroupCreation</span><span class="sxs-lookup"><span data-stu-id="316fb-134">EnableGroupCreation</span></span><li><span data-ttu-id="316fb-135">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="316fb-135">Type: Boolean</span></span><li><span data-ttu-id="316fb-136">Alapértelmezett: igaz</span><span class="sxs-lookup"><span data-stu-id="316fb-136">Default: True</span></span> |<span data-ttu-id="316fb-137">hello jelzőt, amely megadja, hogy engedélyezett-e egyesített csoport létrehozása hello könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="316fb-137">hello flag indicating whether Unified Group creation is allowed in hello directory.</span></span> |
|  <ul><li><span data-ttu-id="316fb-138">GroupCreationAllowedGroupId</span><span class="sxs-lookup"><span data-stu-id="316fb-138">GroupCreationAllowedGroupId</span></span><li><span data-ttu-id="316fb-139">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="316fb-139">Type: String</span></span><li><span data-ttu-id="316fb-140">Alapértelmezett érték: ""</span><span class="sxs-lookup"><span data-stu-id="316fb-140">Default: “”</span></span> |<span data-ttu-id="316fb-141">Hello biztonsági csoport mely hello a tagok számára engedélyezett toocreate egyesített csoportok GUID akkor is, ha EnableGroupCreation == false.</span><span class="sxs-lookup"><span data-stu-id="316fb-141">GUID of hello security group for which hello members are allowed toocreate Unified Groups even when EnableGroupCreation == false.</span></span> |
|  <ul><li><span data-ttu-id="316fb-142">UsageGuidelinesUrl</span><span class="sxs-lookup"><span data-stu-id="316fb-142">UsageGuidelinesUrl</span></span><li><span data-ttu-id="316fb-143">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="316fb-143">Type: String</span></span><li><span data-ttu-id="316fb-144">Alapértelmezett érték: ""</span><span class="sxs-lookup"><span data-stu-id="316fb-144">Default: “”</span></span> |<span data-ttu-id="316fb-145">A hivatkozás toohello csoport használatára vonatkozó irányelvek.</span><span class="sxs-lookup"><span data-stu-id="316fb-145">A link toohello Group Usage Guidelines.</span></span> |
|  <ul><li><span data-ttu-id="316fb-146">ClassificationDescriptions</span><span class="sxs-lookup"><span data-stu-id="316fb-146">ClassificationDescriptions</span></span><li><span data-ttu-id="316fb-147">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="316fb-147">Type: String</span></span><li><span data-ttu-id="316fb-148">Alapértelmezett érték: ""</span><span class="sxs-lookup"><span data-stu-id="316fb-148">Default: “”</span></span> | <span data-ttu-id="316fb-149">Besorolási leírások vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="316fb-149">A comma-delimited list of classification descriptions.</span></span> |
|  <ul><li><span data-ttu-id="316fb-150">DefaultClassification</span><span class="sxs-lookup"><span data-stu-id="316fb-150">DefaultClassification</span></span><li><span data-ttu-id="316fb-151">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="316fb-151">Type: String</span></span><li><span data-ttu-id="316fb-152">Alapértelmezett érték: ""</span><span class="sxs-lookup"><span data-stu-id="316fb-152">Default: “”</span></span> | <span data-ttu-id="316fb-153">hello besorolást, ha nincs megadva a csoport hello alapértelmezett osztályozásra használt toobe van.</span><span class="sxs-lookup"><span data-stu-id="316fb-153">hello classification that is toobe used as hello default classification for a group if none was specified.</span></span>|
|  <ul><li><span data-ttu-id="316fb-154">PrefixSuffixNamingRequirement</span><span class="sxs-lookup"><span data-stu-id="316fb-154">PrefixSuffixNamingRequirement</span></span><li><span data-ttu-id="316fb-155">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="316fb-155">Type: String</span></span><li><span data-ttu-id="316fb-156">Alapértelmezett érték: ""</span><span class="sxs-lookup"><span data-stu-id="316fb-156">Default: “”</span></span> |<span data-ttu-id="316fb-157">Még nincs implementálva</span><span class="sxs-lookup"><span data-stu-id="316fb-157">Not implemented yet</span></span>
|  <ul><li><span data-ttu-id="316fb-158">AllowGuestsToBeGroupOwner</span><span class="sxs-lookup"><span data-stu-id="316fb-158">AllowGuestsToBeGroupOwner</span></span><li><span data-ttu-id="316fb-159">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="316fb-159">Type: Boolean</span></span><li><span data-ttu-id="316fb-160">Alapértelmezett: hamis</span><span class="sxs-lookup"><span data-stu-id="316fb-160">Default: False</span></span> | <span data-ttu-id="316fb-161">Logikai érték-e a Vendég felhasználói csoportok tulajdonosa lehet jelző.</span><span class="sxs-lookup"><span data-stu-id="316fb-161">Boolean indicating whether or not a guest user can be an owner of groups.</span></span> |
|  <ul><li><span data-ttu-id="316fb-162">AllowGuestsToAccessGroups</span><span class="sxs-lookup"><span data-stu-id="316fb-162">AllowGuestsToAccessGroups</span></span><li><span data-ttu-id="316fb-163">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="316fb-163">Type: Boolean</span></span><li><span data-ttu-id="316fb-164">Alapértelmezett: igaz</span><span class="sxs-lookup"><span data-stu-id="316fb-164">Default: True</span></span> | <span data-ttu-id="316fb-165">Logikai érték-e a Vendég felhasználó rendelkezhet hozzáférés tooUnified csoportok tartalom jelző.</span><span class="sxs-lookup"><span data-stu-id="316fb-165">Boolean indicating whether or not a guest user can have access tooUnified groups' content.</span></span> |
|  <ul><li><span data-ttu-id="316fb-166">GuestUsageGuidelinesUrl</span><span class="sxs-lookup"><span data-stu-id="316fb-166">GuestUsageGuidelinesUrl</span></span><li><span data-ttu-id="316fb-167">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="316fb-167">Type: String</span></span><li><span data-ttu-id="316fb-168">Alapértelmezett érték: ""</span><span class="sxs-lookup"><span data-stu-id="316fb-168">Default: “”</span></span> | <span data-ttu-id="316fb-169">a hivatkozás toohello Vendég használatára vonatkozó irányelvek hello URL-címe</span><span class="sxs-lookup"><span data-stu-id="316fb-169">hello url of a link toohello guest usage guidelines.</span></span> |
|  <ul><li><span data-ttu-id="316fb-170">AllowToAddGuests</span><span class="sxs-lookup"><span data-stu-id="316fb-170">AllowToAddGuests</span></span><li><span data-ttu-id="316fb-171">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="316fb-171">Type: Boolean</span></span><li><span data-ttu-id="316fb-172">Alapértelmezett: igaz</span><span class="sxs-lookup"><span data-stu-id="316fb-172">Default: True</span></span> | <span data-ttu-id="316fb-173">Hogy van-e engedélyezett tooadd vendégek toothis directory jelző logikai érték.</span><span class="sxs-lookup"><span data-stu-id="316fb-173">A boolean indicating whether or not is allowed tooadd guests toothis directory.</span></span>|
|  <ul><li><span data-ttu-id="316fb-174">ClassificationList</span><span class="sxs-lookup"><span data-stu-id="316fb-174">ClassificationList</span></span><li><span data-ttu-id="316fb-175">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="316fb-175">Type: String</span></span><li><span data-ttu-id="316fb-176">Alapértelmezett érték: ""</span><span class="sxs-lookup"><span data-stu-id="316fb-176">Default: “”</span></span> |<span data-ttu-id="316fb-177">Érvényes osztályozási értékeket, amelyek lehetnek alkalmazott tooUnified csoportok vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="316fb-177">A comma-delimited list of valid classification values that can be applied tooUnified Groups.</span></span> |
|  <ul><li><span data-ttu-id="316fb-178">EnableGroupCreation</span><span class="sxs-lookup"><span data-stu-id="316fb-178">EnableGroupCreation</span></span><li><span data-ttu-id="316fb-179">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="316fb-179">Type: Boolean</span></span><li><span data-ttu-id="316fb-180">Alapértelmezett: igaz</span><span class="sxs-lookup"><span data-stu-id="316fb-180">Default: True</span></span> | <span data-ttu-id="316fb-181">Függetlenül attól, a nem rendszergazda felhasználók hozhat létre új egyesített csoportok jelző logikai érték.</span><span class="sxs-lookup"><span data-stu-id="316fb-181">A boolean indicating whether or not non-admin users can create new Unified groups.</span></span> |


## <a name="read-settings-at-hello-directory-level"></a><span data-ttu-id="316fb-182">Hello könyvtár szintjén beállítások beolvasása</span><span class="sxs-lookup"><span data-stu-id="316fb-182">Read settings at hello directory level</span></span>
<span data-ttu-id="316fb-183">Ezeket a lépéseket beállítások könyvtár szintjén tooall Office csoportok hello directory olvasni.</span><span class="sxs-lookup"><span data-stu-id="316fb-183">These steps read settings at directory level, which apply tooall Office groups in hello directory.</span></span>

1. <span data-ttu-id="316fb-184">Olvassa el az összes meglévő címtár beállításai:</span><span class="sxs-lookup"><span data-stu-id="316fb-184">Read all existing directory settings:</span></span>
  ```
  Get-AzureADDirectorySetting -All $True
  ```
  <span data-ttu-id="316fb-185">Ez a parancsmag az összes könyvtárbeállításai listáját adja vissza:</span><span class="sxs-lookup"><span data-stu-id="316fb-185">This cmdlet returns a list of all directory settings:</span></span>
  ```
  Id                                   DisplayName   TemplateId                           Values
  --                                   -----------   ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```

2. <span data-ttu-id="316fb-186">Egy adott csoport összes beállítások beolvasása:</span><span class="sxs-lookup"><span data-stu-id="316fb-186">Read all settings for a specific group:</span></span>
  ```
  Get-AzureADObjectSetting -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -TargetType Groups
  ```

3. <span data-ttu-id="316fb-187">Olvassa el az összes könyvtár beállítási értékei egy meghatározott könyvtár beállítások objektum, a beállítások azonosító GUID Azonosítónak a használatával:</span><span class="sxs-lookup"><span data-stu-id="316fb-187">Read all directory settings values of a specific directory settings object, using Settings Id GUID:</span></span>
  ```
  (Get-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323).values
  ```
  <span data-ttu-id="316fb-188">Ez a parancsmag hello neveit és értékeit az adott csoport a beállítási objektumot adja vissza:</span><span class="sxs-lookup"><span data-stu-id="316fb-188">This cmdlet returns hello names and values in this settings object for this specific group:</span></span>
  ```
  Name                          Value
  ----                          -----
  ClassificationDescriptions
  DefaultClassification
  PrefixSuffixNamingRequirement
  AllowGuestsToBeGroupOwner     False 
  AllowGuestsToAccessGroups     True
  GuestUsageGuidelinesUrl
  GroupCreationAllowedGroupId
  AllowToAddGuests              True
  UsageGuidelinesUrl            <https://guideline.com>
  ClassificationList
  EnableGroupCreation           True
  ```

## <a name="update-settings-for-a-specific-group"></a><span data-ttu-id="316fb-189">Egy adott csoport beállításainak frissítése</span><span class="sxs-lookup"><span data-stu-id="316fb-189">Update settings for a specific group</span></span>

1. <span data-ttu-id="316fb-190">Keresse meg a "Groups.Unified.Guest" nevű hello-beállítások sablonja</span><span class="sxs-lookup"><span data-stu-id="316fb-190">Search for hello settings template named "Groups.Unified.Guest"</span></span>
  ```
  Get-AzureADDirectorySettingTemplate
  
  Id                                   DisplayName            Description
  --                                   -----------            -----------
  62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified          ...
  08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest    Settings for a specific Unified Group
  4bc7f740-180e-4586-adb6-38b2e9024e6b Application            ...
  898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy Settings ...
  5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule Settings ...
  ```
2. <span data-ttu-id="316fb-191">Hello sablonobjektum hello Groups.Unified.Guest sablon beolvasása:</span><span class="sxs-lookup"><span data-stu-id="316fb-191">Retrieve hello template object for hello Groups.Unified.Guest template:</span></span>
  ```
  $Template = Get-AzureADDirectorySettingTemplate -Id 08d542b9-071f-4e16-94b0-74abb372e3d9
  ```
3. <span data-ttu-id="316fb-192">Hozzon létre egy új beállítási objektumot hello sablonból:</span><span class="sxs-lookup"><span data-stu-id="316fb-192">Create a new settings object from hello template:</span></span>
  ```
  $Setting = $Template.CreateDirectorySetting()
  ```

4. <span data-ttu-id="316fb-193">Állítsa be a szükséges toohello beállításérték hello:</span><span class="sxs-lookup"><span data-stu-id="316fb-193">Set hello setting toohello required value:</span></span>
  ```
  $Setting["AllowToAddGuests"]=$False
  ```
5. <span data-ttu-id="316fb-194">Hozzon létre új beállítása hello hello szükséges csoportot hello könyvtár:</span><span class="sxs-lookup"><span data-stu-id="316fb-194">Create hello new setting for hello required group in hello directory:</span></span>
  ```
  New-AzureADObjectSetting -TargetType Groups -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -DirectorySetting $Setting
  
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  25651479-a26e-4181-afce-ce24111b2cb5             08d542b9-071f-4e16-94b0-74abb372e3d9 {class SettingValue {...
  ```

## <a name="update-settings-at-hello-directory-level"></a><span data-ttu-id="316fb-195">Hello könyvtár szintjén beállításainak frissítése</span><span class="sxs-lookup"><span data-stu-id="316fb-195">Update settings at hello directory level</span></span>

<span data-ttu-id="316fb-196">Ezeket a lépéseket beállítások könyvtár szintjén tooall egyesített csoportok hello directory frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="316fb-196">These steps update settings at directory level, which apply tooall Unified groups in hello directory.</span></span> <span data-ttu-id="316fb-197">Ezek a példák feltételezik, hogy már létezik a beállítási objektumot a címtárban.</span><span class="sxs-lookup"><span data-stu-id="316fb-197">These examples assume there is already a Settings object in your directory.</span></span>

1. <span data-ttu-id="316fb-198">Hello meglévő beállítási objektumot keresése:</span><span class="sxs-lookup"><span data-stu-id="316fb-198">Find hello existing Settings object:</span></span>
  ```
  Get-AzureADDirectorySetting | Where-object -Property Displayname -Value "Group.Unified" -EQ
  
  Id                                   DisplayName   TemplateId                           Values
  --                                   -----------   ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  
  $setting = Get-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323
  ```
2. <span data-ttu-id="316fb-199">Hello érték frissítése:</span><span class="sxs-lookup"><span data-stu-id="316fb-199">Update hello value:</span></span>
  
  ```
  $Setting["AllowToAddGuests"] = "false"
  ```
3. <span data-ttu-id="316fb-200">Hello beállítás frissítése:</span><span class="sxs-lookup"><span data-stu-id="316fb-200">Update hello setting:</span></span>
  
  ```
  Set-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323 -DirectorySetting $Setting
  ```

## <a name="remove-settings-at-hello-directory-level"></a><span data-ttu-id="316fb-201">Távolítsa el a hello könyvtár szintjén beállításait</span><span class="sxs-lookup"><span data-stu-id="316fb-201">Remove settings at hello directory level</span></span>
<span data-ttu-id="316fb-202">Ez a lépés beállítások könyvtár szintjén tooall Office csoportok hello directory eltávolítja.</span><span class="sxs-lookup"><span data-stu-id="316fb-202">This step removes settings at directory level, which apply tooall Office groups in hello directory.</span></span>
  ```
  Remove-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323c
  ```

## <a name="cmdlet-syntax-reference"></a><span data-ttu-id="316fb-203">A parancsmag szintaxisa referencia</span><span class="sxs-lookup"><span data-stu-id="316fb-203">Cmdlet syntax reference</span></span>
<span data-ttu-id="316fb-204">További Azure Active Directory PowerShell dokumentációját a található [Azure Active Directory-modul parancsmagokkal](/powershell/azure/install-adv2?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="316fb-204">You can find more Azure Active Directory PowerShell documentation at [Azure Active Directory Cmdlets](/powershell/azure/install-adv2?view=azureadps-2.0).</span></span>

## <a name="additional-reading"></a><span data-ttu-id="316fb-205">További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="316fb-205">Additional reading</span></span>

* [<span data-ttu-id="316fb-206">Hozzáférés tooresources kezelése az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="316fb-206">Managing access tooresources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="316fb-207">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="316fb-207">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
