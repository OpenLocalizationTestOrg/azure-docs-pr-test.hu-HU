---
title: "Azure Active Directory-parancsmagok használatával beállításainak konfigurálásához |} Microsoft Docs"
description: "Hogyan az Azure Active Directory-parancsmagok használatával csoportok beállításainak kezelése"
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
ms.openlocfilehash: 0d89f12955b90c7e1a8301b7c3a1a92e7f62d085
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a><span data-ttu-id="f642f-103">Azure Active Directory-parancsmagok csoportbeállítások konfigurálásához</span><span class="sxs-lookup"><span data-stu-id="f642f-103">Azure Active Directory cmdlets for configuring group settings</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f642f-104">Ez a tartalom csak az Office 365-csoportok vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="f642f-104">This content applies only to Office 365 groups.</span></span> <span data-ttu-id="f642f-105">További információ a biztonsági csoportok létrehozása a felhasználók hogyan állítsa be `Set-MSOLCompanySettings -UsersPermissionToCreateGroupsEnabled $True` leírtak [Set-MSOLCompanySettings](https://docs.microsoft.com/en-us/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).</span><span class="sxs-lookup"><span data-stu-id="f642f-105">For more information on how to allow users to create Security groups, set `Set-MSOLCompanySettings -UsersPermissionToCreateGroupsEnabled $True` as described in [Set-MSOLCompanySettings](https://docs.microsoft.com/en-us/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).</span></span> 

<span data-ttu-id="f642f-106">Az Office 365 csoportok beállításai a beállítási objektumot és egy SettingsTemplate objektum használatával.</span><span class="sxs-lookup"><span data-stu-id="f642f-106">Office 365 Groups settings are configured using a Settings object and a SettingsTemplate object.</span></span> <span data-ttu-id="f642f-107">Kezdetben nem lát minden objektumokat a könyvtárban, a könyvtár van beállítva, az alapértelmezett beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="f642f-107">Initially, you don't see any Settings objects in your directory, because your directory is configured with the default settings.</span></span> <span data-ttu-id="f642f-108">Ha módosítani szeretné az alapértelmezett beállításokat, a beállítások sablon használatával új beállítási objektumot kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="f642f-108">To change the default settings, you must create a new settings object using a settings template.</span></span> <span data-ttu-id="f642f-109">Beállítások sablonok Microsoft határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="f642f-109">Settings templates are defined by Microsoft.</span></span> <span data-ttu-id="f642f-110">Nincsenek számos különböző beállításokat sablont.</span><span class="sxs-lookup"><span data-stu-id="f642f-110">There are several different settings templates.</span></span> <span data-ttu-id="f642f-111">Hogy a címtárban a Office 365-beállításainak konfigurálásához, a "Group.Unified" nevű sablont használ.</span><span class="sxs-lookup"><span data-stu-id="f642f-111">To configure Office 365 group settings for your directory, you use the template named "Group.Unified".</span></span> <span data-ttu-id="f642f-112">Egy különálló csoportot az Office 365 csoport beállításainak konfigurálásához használja a "Group.Unified.Guest" nevű sablont.</span><span class="sxs-lookup"><span data-stu-id="f642f-112">To configure Office 365 group settings on a single group, use the template named "Group.Unified.Guest".</span></span> <span data-ttu-id="f642f-113">Ez a sablon Vendég hozzáférést az Office 365-csoportok kezelésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="f642f-113">This template is used to manage guest access to an Office 365 group.</span></span> 

<span data-ttu-id="f642f-114">A parancsmagok az Azure Active Directory PowerShell V2 modulja részét képezik.</span><span class="sxs-lookup"><span data-stu-id="f642f-114">The cmdlets are part of the Azure Active Directory PowerShell V2 module.</span></span> <span data-ttu-id="f642f-115">További tájékoztatást szeretne töltse le és a modul telepítése a számítógépre, tekintse meg a cikket [Azure Active Directory PowerShell 2-es verzió](https://docs.microsoft.com/powershell/azuread/).</span><span class="sxs-lookup"><span data-stu-id="f642f-115">For instructions how to download and install the module on your computer, see the article [Azure Active Directory PowerShell Version 2](https://docs.microsoft.com/powershell/azuread/).</span></span> <span data-ttu-id="f642f-116">A modulnak a 2-es verziójú kiadásában telepítése [a PowerShell-galériában](https://www.powershellgallery.com/packages/AzureAD/).</span><span class="sxs-lookup"><span data-stu-id="f642f-116">You can install the version 2 release of the module from [the PowerShell gallery](https://www.powershellgallery.com/packages/AzureAD/).</span></span>

## <a name="retrieve-a-specific-settings-value"></a><span data-ttu-id="f642f-117">Egy adott beállítás értékének beolvasása</span><span class="sxs-lookup"><span data-stu-id="f642f-117">Retrieve a specific settings value</span></span>
<span data-ttu-id="f642f-118">Ha ismeri a keresett beállítás nevét, használhatja az alábbi parancsmagot, hogy a jelenlegi beállítás értékének beolvasása.</span><span class="sxs-lookup"><span data-stu-id="f642f-118">If you know the name of the setting you want to retrieve, you can use the below cmdlet to retrieve the current settings value.</span></span> <span data-ttu-id="f642f-119">Ebben a példában azt olvas be a "UsageGuidelinesUrl." nevű beállítás értéke</span><span class="sxs-lookup"><span data-stu-id="f642f-119">In this example, we're retrieving the value for a setting named "UsageGuidelinesUrl."</span></span> <span data-ttu-id="f642f-120">Címtár-beállítások és a nevek kapcsolatos további le a cikkben olvasható.</span><span class="sxs-lookup"><span data-stu-id="f642f-120">You can read more about directory settings and their names further down in this article.</span></span>

```powershell
(Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value UsageGuidelinesUrl -EQ
```

## <a name="create-settings-at-the-directory-level"></a><span data-ttu-id="f642f-121">A könyvtár szintjén beállítások létrehozása</span><span class="sxs-lookup"><span data-stu-id="f642f-121">Create settings at the directory level</span></span>
<span data-ttu-id="f642f-122">Ezeket a lépéseket beállítások létrehozása könyvtár szinten, amelyek vonatkoznak az összes Office 365 (egyesített) csoportok a címtárban.</span><span class="sxs-lookup"><span data-stu-id="f642f-122">These steps create settings at directory level, which apply to all Office 365 groups (Unified groups) in the directory.</span></span>

1. <span data-ttu-id="f642f-123">A DirectorySettings parancsmagok meg kell adnia a használni kívánt SettingsTemplate Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="f642f-123">In the DirectorySettings cmdlets, you must specify the ID of the SettingsTemplate you want to use.</span></span> <span data-ttu-id="f642f-124">Ha nem ismeri ezt az Azonosítót, ez a parancsmag az összes beállítások sablonok listáján adja vissza:</span><span class="sxs-lookup"><span data-stu-id="f642f-124">If you do not know this ID, this cmdlet returns the list of all settings templates:</span></span>
  
  ```
  PS C:> Get-AzureADDirectorySettingTemplate
  ```
  <span data-ttu-id="f642f-125">Ez a parancsmag hívás elérhető összes sablonok adja vissza:</span><span class="sxs-lookup"><span data-stu-id="f642f-125">This cmdlet call returns all templates that are available:</span></span>
  
  ```
  Id                                   DisplayName         Description
  --                                   -----------         -----------
  62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified       ...
  08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest Settings for a specific Unified Group
  16933506-8a8d-4f0d-ad58-e1db05a5b929 Company.BuiltIn     Setting templates define the different settings that can be used for the associ...
  4bc7f740-180e-4586-adb6-38b2e9024e6b Application...
  898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy       Settings ...
  5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule       Settings ...
  ```
2. <span data-ttu-id="f642f-126">A használati iránymutatás URL-cím hozzáadásához először kell beszereznie a SettingsTemplate objektum, amely meghatározza a használati iránymutatás URL-érték; Ez azt jelenti, hogy a Group.Unified sablont:</span><span class="sxs-lookup"><span data-stu-id="f642f-126">To add a usage guideline URL, first you need to get the SettingsTemplate object that defines the usage guideline URL value; that is, the Group.Unified template:</span></span>
  
  ```
  $Template = Get-AzureADDirectorySettingTemplate -Id 62375ab9-6b52-47ed-826b-58e47e0e304b
  ```
3. <span data-ttu-id="f642f-127">Ezután hozzon létre egy új beállítási objektumot, hogy a sablon alapján:</span><span class="sxs-lookup"><span data-stu-id="f642f-127">Next, create a new settings object based on that template:</span></span>
  
  ```
  $Setting = $template.CreateDirectorySetting()
  ```  
4. <span data-ttu-id="f642f-128">Ezután frissítse a használati iránymutatás értéket:</span><span class="sxs-lookup"><span data-stu-id="f642f-128">Then update the usage guideline value:</span></span>
  
  ```
  $setting["UsageGuidelinesUrl"] = "<https://guideline.com>"
  ```  
5. <span data-ttu-id="f642f-129">Végezetül a beállítások alkalmazásához:</span><span class="sxs-lookup"><span data-stu-id="f642f-129">Finally, apply the settings:</span></span>
  
  ```
  New-AzureADDirectorySetting -DirectorySetting $setting
  ```

<span data-ttu-id="f642f-130">Sikeres létrehozása után a parancsmag az új beállítások objektum Azonosítóját adja vissza:</span><span class="sxs-lookup"><span data-stu-id="f642f-130">Upon successful completion, the cmdlet returns the ID of the new settings object:</span></span>
  ```
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323             62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```
<span data-ttu-id="f642f-131">Az alábbiakban a Group.Unified SettingsTemplate megadott beállításoknak.</span><span class="sxs-lookup"><span data-stu-id="f642f-131">Here are the settings defined in the Group.Unified SettingsTemplate.</span></span>

| <span data-ttu-id="f642f-132">**Beállítás**</span><span class="sxs-lookup"><span data-stu-id="f642f-132">**Setting**</span></span> | <span data-ttu-id="f642f-133">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="f642f-133">**Description**</span></span> |
| --- | --- |
|  <ul><li><span data-ttu-id="f642f-134">EnableGroupCreation</span><span class="sxs-lookup"><span data-stu-id="f642f-134">EnableGroupCreation</span></span><li><span data-ttu-id="f642f-135">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="f642f-135">Type: Boolean</span></span><li><span data-ttu-id="f642f-136">Alapértelmezett: igaz</span><span class="sxs-lookup"><span data-stu-id="f642f-136">Default: True</span></span> |<span data-ttu-id="f642f-137">A jelzőt, amely azt jelzi, hogy a címtárban engedélyezett-e a egyesített csoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f642f-137">The flag indicating whether Unified Group creation is allowed in the directory.</span></span> |
|  <ul><li><span data-ttu-id="f642f-138">GroupCreationAllowedGroupId</span><span class="sxs-lookup"><span data-stu-id="f642f-138">GroupCreationAllowedGroupId</span></span><li><span data-ttu-id="f642f-139">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f642f-139">Type: String</span></span><li><span data-ttu-id="f642f-140">Alapértelmezett érték: ""</span><span class="sxs-lookup"><span data-stu-id="f642f-140">Default: “”</span></span> |<span data-ttu-id="f642f-141">A biztonsági csoport, amelynek a tagjai hozhatnak létre egységes csoportok GUID akkor is, ha EnableGroupCreation == false.</span><span class="sxs-lookup"><span data-stu-id="f642f-141">GUID of the security group for which the members are allowed to create Unified Groups even when EnableGroupCreation == false.</span></span> |
|  <ul><li><span data-ttu-id="f642f-142">UsageGuidelinesUrl</span><span class="sxs-lookup"><span data-stu-id="f642f-142">UsageGuidelinesUrl</span></span><li><span data-ttu-id="f642f-143">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f642f-143">Type: String</span></span><li><span data-ttu-id="f642f-144">Alapértelmezett érték: ""</span><span class="sxs-lookup"><span data-stu-id="f642f-144">Default: “”</span></span> |<span data-ttu-id="f642f-145">A csoport használatára vonatkozó irányelvek mutató hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="f642f-145">A link to the Group Usage Guidelines.</span></span> |
|  <ul><li><span data-ttu-id="f642f-146">ClassificationDescriptions</span><span class="sxs-lookup"><span data-stu-id="f642f-146">ClassificationDescriptions</span></span><li><span data-ttu-id="f642f-147">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f642f-147">Type: String</span></span><li><span data-ttu-id="f642f-148">Alapértelmezett érték: ""</span><span class="sxs-lookup"><span data-stu-id="f642f-148">Default: “”</span></span> | <span data-ttu-id="f642f-149">Besorolási leírások vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="f642f-149">A comma-delimited list of classification descriptions.</span></span> |
|  <ul><li><span data-ttu-id="f642f-150">DefaultClassification</span><span class="sxs-lookup"><span data-stu-id="f642f-150">DefaultClassification</span></span><li><span data-ttu-id="f642f-151">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f642f-151">Type: String</span></span><li><span data-ttu-id="f642f-152">Alapértelmezett érték: ""</span><span class="sxs-lookup"><span data-stu-id="f642f-152">Default: “”</span></span> | <span data-ttu-id="f642f-153">A besorolás, amely használható a alapértelmezett besorolást egy csoportra, ha nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="f642f-153">The classification that is to be used as the default classification for a group if none was specified.</span></span>|
|  <ul><li><span data-ttu-id="f642f-154">PrefixSuffixNamingRequirement</span><span class="sxs-lookup"><span data-stu-id="f642f-154">PrefixSuffixNamingRequirement</span></span><li><span data-ttu-id="f642f-155">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f642f-155">Type: String</span></span><li><span data-ttu-id="f642f-156">Alapértelmezett érték: ""</span><span class="sxs-lookup"><span data-stu-id="f642f-156">Default: “”</span></span> |<span data-ttu-id="f642f-157">Még nincs implementálva</span><span class="sxs-lookup"><span data-stu-id="f642f-157">Not implemented yet</span></span>
|  <ul><li><span data-ttu-id="f642f-158">AllowGuestsToBeGroupOwner</span><span class="sxs-lookup"><span data-stu-id="f642f-158">AllowGuestsToBeGroupOwner</span></span><li><span data-ttu-id="f642f-159">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="f642f-159">Type: Boolean</span></span><li><span data-ttu-id="f642f-160">Alapértelmezett: hamis</span><span class="sxs-lookup"><span data-stu-id="f642f-160">Default: False</span></span> | <span data-ttu-id="f642f-161">Logikai érték-e a Vendég felhasználói csoportok tulajdonosa lehet jelző.</span><span class="sxs-lookup"><span data-stu-id="f642f-161">Boolean indicating whether or not a guest user can be an owner of groups.</span></span> |
|  <ul><li><span data-ttu-id="f642f-162">AllowGuestsToAccessGroups</span><span class="sxs-lookup"><span data-stu-id="f642f-162">AllowGuestsToAccessGroups</span></span><li><span data-ttu-id="f642f-163">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="f642f-163">Type: Boolean</span></span><li><span data-ttu-id="f642f-164">Alapértelmezett: igaz</span><span class="sxs-lookup"><span data-stu-id="f642f-164">Default: True</span></span> | <span data-ttu-id="f642f-165">Jelző logikai érték beolvasása-e a Vendég felhasználó rendelkezhet egyesített csoportok tartalomhoz való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="f642f-165">Boolean indicating whether or not a guest user can have access to Unified groups' content.</span></span> |
|  <ul><li><span data-ttu-id="f642f-166">GuestUsageGuidelinesUrl</span><span class="sxs-lookup"><span data-stu-id="f642f-166">GuestUsageGuidelinesUrl</span></span><li><span data-ttu-id="f642f-167">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f642f-167">Type: String</span></span><li><span data-ttu-id="f642f-168">Alapértelmezett érték: ""</span><span class="sxs-lookup"><span data-stu-id="f642f-168">Default: “”</span></span> | <span data-ttu-id="f642f-169">A Vendég használatára vonatkozó irányelvek mutató hivatkozás URL-címét.</span><span class="sxs-lookup"><span data-stu-id="f642f-169">The url of a link to the guest usage guidelines.</span></span> |
|  <ul><li><span data-ttu-id="f642f-170">AllowToAddGuests</span><span class="sxs-lookup"><span data-stu-id="f642f-170">AllowToAddGuests</span></span><li><span data-ttu-id="f642f-171">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="f642f-171">Type: Boolean</span></span><li><span data-ttu-id="f642f-172">Alapértelmezett: igaz</span><span class="sxs-lookup"><span data-stu-id="f642f-172">Default: True</span></span> | <span data-ttu-id="f642f-173">Egy logikai jelző Vendégek hozzáadása a következő könyvtár számára engedélyezett-e.</span><span class="sxs-lookup"><span data-stu-id="f642f-173">A boolean indicating whether or not is allowed to add guests to this directory.</span></span>|
|  <ul><li><span data-ttu-id="f642f-174">ClassificationList</span><span class="sxs-lookup"><span data-stu-id="f642f-174">ClassificationList</span></span><li><span data-ttu-id="f642f-175">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f642f-175">Type: String</span></span><li><span data-ttu-id="f642f-176">Alapértelmezett érték: ""</span><span class="sxs-lookup"><span data-stu-id="f642f-176">Default: “”</span></span> |<span data-ttu-id="f642f-177">Egyesített csoportok alkalmazható érvényes osztályozási értékeket vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="f642f-177">A comma-delimited list of valid classification values that can be applied to Unified Groups.</span></span> |
|  <ul><li><span data-ttu-id="f642f-178">EnableGroupCreation</span><span class="sxs-lookup"><span data-stu-id="f642f-178">EnableGroupCreation</span></span><li><span data-ttu-id="f642f-179">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="f642f-179">Type: Boolean</span></span><li><span data-ttu-id="f642f-180">Alapértelmezett: igaz</span><span class="sxs-lookup"><span data-stu-id="f642f-180">Default: True</span></span> | <span data-ttu-id="f642f-181">Függetlenül attól, a nem rendszergazda felhasználók hozhat létre új egyesített csoportok jelző logikai érték.</span><span class="sxs-lookup"><span data-stu-id="f642f-181">A boolean indicating whether or not non-admin users can create new Unified groups.</span></span> |


## <a name="read-settings-at-the-directory-level"></a><span data-ttu-id="f642f-182">A könyvtár szintjén beállítások beolvasása</span><span class="sxs-lookup"><span data-stu-id="f642f-182">Read settings at the directory level</span></span>
<span data-ttu-id="f642f-183">Ezeket a lépéseket a címtár összes Office-csoport könyvtár szintjén beállítások olvasása.</span><span class="sxs-lookup"><span data-stu-id="f642f-183">These steps read settings at directory level, which apply to all Office groups in the directory.</span></span>

1. <span data-ttu-id="f642f-184">Olvassa el az összes meglévő címtár beállításai:</span><span class="sxs-lookup"><span data-stu-id="f642f-184">Read all existing directory settings:</span></span>
  ```
  Get-AzureADDirectorySetting -All $True
  ```
  <span data-ttu-id="f642f-185">Ez a parancsmag az összes könyvtárbeállításai listáját adja vissza:</span><span class="sxs-lookup"><span data-stu-id="f642f-185">This cmdlet returns a list of all directory settings:</span></span>
  ```
  Id                                   DisplayName   TemplateId                           Values
  --                                   -----------   ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```

2. <span data-ttu-id="f642f-186">Egy adott csoport összes beállítások beolvasása:</span><span class="sxs-lookup"><span data-stu-id="f642f-186">Read all settings for a specific group:</span></span>
  ```
  Get-AzureADObjectSetting -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -TargetType Groups
  ```

3. <span data-ttu-id="f642f-187">Olvassa el az összes könyvtár beállítási értékei egy meghatározott könyvtár beállítások objektum, a beállítások azonosító GUID Azonosítónak a használatával:</span><span class="sxs-lookup"><span data-stu-id="f642f-187">Read all directory settings values of a specific directory settings object, using Settings Id GUID:</span></span>
  ```
  (Get-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323).values
  ```
  <span data-ttu-id="f642f-188">Ez a parancsmag az adott csoport a beállítási objektumot a neveket és értékeket adja vissza:</span><span class="sxs-lookup"><span data-stu-id="f642f-188">This cmdlet returns the names and values in this settings object for this specific group:</span></span>
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

## <a name="update-settings-for-a-specific-group"></a><span data-ttu-id="f642f-189">Egy adott csoport beállításainak frissítése</span><span class="sxs-lookup"><span data-stu-id="f642f-189">Update settings for a specific group</span></span>

1. <span data-ttu-id="f642f-190">Keresse meg a "Groups.Unified.Guest" nevű beállítások sablont</span><span class="sxs-lookup"><span data-stu-id="f642f-190">Search for the settings template named "Groups.Unified.Guest"</span></span>
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
2. <span data-ttu-id="f642f-191">A sablon objektumának Groups.Unified.Guest sablon beolvasása:</span><span class="sxs-lookup"><span data-stu-id="f642f-191">Retrieve the template object for the Groups.Unified.Guest template:</span></span>
  ```
  $Template = Get-AzureADDirectorySettingTemplate -Id 08d542b9-071f-4e16-94b0-74abb372e3d9
  ```
3. <span data-ttu-id="f642f-192">Hozzon létre egy új beállítási objektumot a sablon alapján:</span><span class="sxs-lookup"><span data-stu-id="f642f-192">Create a new settings object from the template:</span></span>
  ```
  $Setting = $Template.CreateDirectorySetting()
  ```

4. <span data-ttu-id="f642f-193">Állítsa be a szükséges érték:</span><span class="sxs-lookup"><span data-stu-id="f642f-193">Set the setting to the required value:</span></span>
  ```
  $Setting["AllowToAddGuests"]=$False
  ```
5. <span data-ttu-id="f642f-194">A szükséges csoportot az új beállítás létrehozása a könyvtárban:</span><span class="sxs-lookup"><span data-stu-id="f642f-194">Create the new setting for the required group in the directory:</span></span>
  ```
  New-AzureADObjectSetting -TargetType Groups -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -DirectorySetting $Setting
  
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  25651479-a26e-4181-afce-ce24111b2cb5             08d542b9-071f-4e16-94b0-74abb372e3d9 {class SettingValue {...
  ```

## <a name="update-settings-at-the-directory-level"></a><span data-ttu-id="f642f-195">A könyvtár szintjén beállításainak frissítése</span><span class="sxs-lookup"><span data-stu-id="f642f-195">Update settings at the directory level</span></span>

<span data-ttu-id="f642f-196">Ezeket a lépéseket a címtár összes egyesített csoportot könyvtár szintjén beállítások frissítése.</span><span class="sxs-lookup"><span data-stu-id="f642f-196">These steps update settings at directory level, which apply to all Unified groups in the directory.</span></span> <span data-ttu-id="f642f-197">Ezek a példák feltételezik, hogy már létezik a beállítási objektumot a címtárban.</span><span class="sxs-lookup"><span data-stu-id="f642f-197">These examples assume there is already a Settings object in your directory.</span></span>

1. <span data-ttu-id="f642f-198">A meglévő beállítások objektum keresése:</span><span class="sxs-lookup"><span data-stu-id="f642f-198">Find the existing Settings object:</span></span>
  ```
  Get-AzureADDirectorySetting | Where-object -Property Displayname -Value "Group.Unified" -EQ
  
  Id                                   DisplayName   TemplateId                           Values
  --                                   -----------   ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  
  $setting = Get-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323
  ```
2. <span data-ttu-id="f642f-199">Frissítse az értéket:</span><span class="sxs-lookup"><span data-stu-id="f642f-199">Update the value:</span></span>
  
  ```
  $Setting["AllowToAddGuests"] = "false"
  ```
3. <span data-ttu-id="f642f-200">A beállítás módosítása:</span><span class="sxs-lookup"><span data-stu-id="f642f-200">Update the setting:</span></span>
  
  ```
  Set-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323 -DirectorySetting $Setting
  ```

## <a name="remove-settings-at-the-directory-level"></a><span data-ttu-id="f642f-201">A könyvtár szintjén beállítások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="f642f-201">Remove settings at the directory level</span></span>
<span data-ttu-id="f642f-202">Ez a lépés eltávolítja a címtár összes Office-csoport könyvtár szintjén beállítások.</span><span class="sxs-lookup"><span data-stu-id="f642f-202">This step removes settings at directory level, which apply to all Office groups in the directory.</span></span>
  ```
  Remove-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323c
  ```

## <a name="cmdlet-syntax-reference"></a><span data-ttu-id="f642f-203">A parancsmag szintaxisa referencia</span><span class="sxs-lookup"><span data-stu-id="f642f-203">Cmdlet syntax reference</span></span>
<span data-ttu-id="f642f-204">További Azure Active Directory PowerShell dokumentációját a található [Azure Active Directory-modul parancsmagokkal](/powershell/azure/install-adv2?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="f642f-204">You can find more Azure Active Directory PowerShell documentation at [Azure Active Directory Cmdlets](/powershell/azure/install-adv2?view=azureadps-2.0).</span></span>

## <a name="additional-reading"></a><span data-ttu-id="f642f-205">További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="f642f-205">Additional reading</span></span>

* [<span data-ttu-id="f642f-206">Erőforráshozzáférés-kezelés Azure Active Directory-csoportokkal</span><span class="sxs-lookup"><span data-stu-id="f642f-206">Managing access to resources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="f642f-207">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="f642f-207">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
