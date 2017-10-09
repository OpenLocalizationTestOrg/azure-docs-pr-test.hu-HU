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
# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>Azure Active Directory-parancsmagok csoportbeállítások konfigurálásához

> [!IMPORTANT]
> Ez a tartalom csak tooOffice 365 csoportok érvényes. További információt a tooallow felhasználók toocreate biztonsági csoportok beállítása `Set-MSOLCompanySettings -UsersPermissionToCreateGroupsEnabled $True` leírtak [Set-MSOLCompanySettings](https://docs.microsoft.com/en-us/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0). 

Az Office 365 csoportok beállításai a beállítási objektumot és egy SettingsTemplate objektum használatával. Kezdetben nem lát beállítások objektumokat a könyvtárban, mert a könyvtár hello alapértelmezett beállításokkal van konfigurálva. toochange hello alapértelmezett beállításokat, beállítások sablon használatával új beállítási objektumot kell létrehoznia. Beállítások sablonok Microsoft határozzák meg. Nincsenek számos különböző beállításokat sablont. Office 365 tooconfigure csoport beállításait a címtárban, "Group.Unified" nevű hello sablont használ. egy különálló csoportot, Office 365 tooconfigure csoportjára vonatkozó beállításait használja a "Group.Unified.Guest" nevű hello sablont. Ez a sablon használt toomanage Vendég hozzáférési tooan Office 365 csoport. 

hello parancsmagok hello Azure Active Directory PowerShell V2 modulja részét képezik. Hogyan toodownload és hello-modul telepítése a számítógépre, lásd: hello cikk [Azure Active Directory PowerShell 2-es verzió](https://docs.microsoft.com/powershell/azuread/). Hello modulnak a 2-es verziójú kiadásában hello telepítése [hello PowerShell-galériában](https://www.powershellgallery.com/packages/AzureAD/).

## <a name="retrieve-a-specific-settings-value"></a>Egy adott beállítás értékének beolvasása
Ha tudja hello nevét beállítás hello tooretrieve, használhatja a hello parancsmag tooretrieve hello aktuális beállítások érték alá. Ebben a példában a "UsageGuidelinesUrl." nevű beállítás értékét hello azt olvas Címtár-beállítások és a nevek kapcsolatos további le a cikkben olvasható.

```powershell
(Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value UsageGuidelinesUrl -EQ
```

## <a name="create-settings-at-hello-directory-level"></a>Hello könyvtár szintjén beállítások létrehozása
Ezeket a lépéseket beállítások létrehozása könyvtár szintjén tooall Office 365 (egyesített) csoportok a hello directory alkalmazandó.

1. Hello DirectorySettings parancsmagok meg kell adnia azt szeretné, hogy toouse SettingsTemplate hello hello azonosítója. Ha nem ismeri ezt az Azonosítót, a parancsmag minden beállítások sablonok hello listáját adja vissza:
  
  ```
  PS C:> Get-AzureADDirectorySettingTemplate
  ```
  Ez a parancsmag hívás elérhető összes sablonok adja vissza:
  
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
2. tooadd használati iránymutatás URL-címet, először meg kell tooget hello SettingsTemplate objektum, amely meghatározza a hello használati iránymutatás URL-érték; Ez azt jelenti, hogy hello Group.Unified sablont:
  
  ```
  $Template = Get-AzureADDirectorySettingTemplate -Id 62375ab9-6b52-47ed-826b-58e47e0e304b
  ```
3. Ezután hozzon létre egy új beállítási objektumot, hogy a sablon alapján:
  
  ```
  $Setting = $template.CreateDirectorySetting()
  ```  
4. Ezután frissítse hello használati iránymutatás értéket:
  
  ```
  $setting["UsageGuidelinesUrl"] = "<https://guideline.com>"
  ```  
5. Végezetül a hello-beállítások alkalmazásához:
  
  ```
  New-AzureADDirectorySetting -DirectorySetting $setting
  ```

Sikeres befejezését követően hello parancsmag hello hello új beállítások objektum Azonosítóját adja vissza:
  ```
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323             62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```
Az alábbiakban meghatározott hello Group.Unified SettingsTemplate hello-beállítások.

| **Beállítás** | **Leírás** |
| --- | --- |
|  <ul><li>EnableGroupCreation<li>Típus: logikai<li>Alapértelmezett: igaz |hello jelzőt, amely megadja, hogy engedélyezett-e egyesített csoport létrehozása hello könyvtárban. |
|  <ul><li>GroupCreationAllowedGroupId<li>Típus: Karakterlánc<li>Alapértelmezett érték: "" |Hello biztonsági csoport mely hello a tagok számára engedélyezett toocreate egyesített csoportok GUID akkor is, ha EnableGroupCreation == false. |
|  <ul><li>UsageGuidelinesUrl<li>Típus: Karakterlánc<li>Alapértelmezett érték: "" |A hivatkozás toohello csoport használatára vonatkozó irányelvek. |
|  <ul><li>ClassificationDescriptions<li>Típus: Karakterlánc<li>Alapértelmezett érték: "" | Besorolási leírások vesszővel tagolt listája. |
|  <ul><li>DefaultClassification<li>Típus: Karakterlánc<li>Alapértelmezett érték: "" | hello besorolást, ha nincs megadva a csoport hello alapértelmezett osztályozásra használt toobe van.|
|  <ul><li>PrefixSuffixNamingRequirement<li>Típus: Karakterlánc<li>Alapértelmezett érték: "" |Még nincs implementálva
|  <ul><li>AllowGuestsToBeGroupOwner<li>Típus: logikai<li>Alapértelmezett: hamis | Logikai érték-e a Vendég felhasználói csoportok tulajdonosa lehet jelző. |
|  <ul><li>AllowGuestsToAccessGroups<li>Típus: logikai<li>Alapértelmezett: igaz | Logikai érték-e a Vendég felhasználó rendelkezhet hozzáférés tooUnified csoportok tartalom jelző. |
|  <ul><li>GuestUsageGuidelinesUrl<li>Típus: Karakterlánc<li>Alapértelmezett érték: "" | a hivatkozás toohello Vendég használatára vonatkozó irányelvek hello URL-címe |
|  <ul><li>AllowToAddGuests<li>Típus: logikai<li>Alapértelmezett: igaz | Hogy van-e engedélyezett tooadd vendégek toothis directory jelző logikai érték.|
|  <ul><li>ClassificationList<li>Típus: Karakterlánc<li>Alapértelmezett érték: "" |Érvényes osztályozási értékeket, amelyek lehetnek alkalmazott tooUnified csoportok vesszővel tagolt listája. |
|  <ul><li>EnableGroupCreation<li>Típus: logikai<li>Alapértelmezett: igaz | Függetlenül attól, a nem rendszergazda felhasználók hozhat létre új egyesített csoportok jelző logikai érték. |


## <a name="read-settings-at-hello-directory-level"></a>Hello könyvtár szintjén beállítások beolvasása
Ezeket a lépéseket beállítások könyvtár szintjén tooall Office csoportok hello directory olvasni.

1. Olvassa el az összes meglévő címtár beállításai:
  ```
  Get-AzureADDirectorySetting -All $True
  ```
  Ez a parancsmag az összes könyvtárbeállításai listáját adja vissza:
  ```
  Id                                   DisplayName   TemplateId                           Values
  --                                   -----------   ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```

2. Egy adott csoport összes beállítások beolvasása:
  ```
  Get-AzureADObjectSetting -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -TargetType Groups
  ```

3. Olvassa el az összes könyvtár beállítási értékei egy meghatározott könyvtár beállítások objektum, a beállítások azonosító GUID Azonosítónak a használatával:
  ```
  (Get-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323).values
  ```
  Ez a parancsmag hello neveit és értékeit az adott csoport a beállítási objektumot adja vissza:
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

## <a name="update-settings-for-a-specific-group"></a>Egy adott csoport beállításainak frissítése

1. Keresse meg a "Groups.Unified.Guest" nevű hello-beállítások sablonja
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
2. Hello sablonobjektum hello Groups.Unified.Guest sablon beolvasása:
  ```
  $Template = Get-AzureADDirectorySettingTemplate -Id 08d542b9-071f-4e16-94b0-74abb372e3d9
  ```
3. Hozzon létre egy új beállítási objektumot hello sablonból:
  ```
  $Setting = $Template.CreateDirectorySetting()
  ```

4. Állítsa be a szükséges toohello beállításérték hello:
  ```
  $Setting["AllowToAddGuests"]=$False
  ```
5. Hozzon létre új beállítása hello hello szükséges csoportot hello könyvtár:
  ```
  New-AzureADObjectSetting -TargetType Groups -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -DirectorySetting $Setting
  
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  25651479-a26e-4181-afce-ce24111b2cb5             08d542b9-071f-4e16-94b0-74abb372e3d9 {class SettingValue {...
  ```

## <a name="update-settings-at-hello-directory-level"></a>Hello könyvtár szintjén beállításainak frissítése

Ezeket a lépéseket beállítások könyvtár szintjén tooall egyesített csoportok hello directory frissítéséhez. Ezek a példák feltételezik, hogy már létezik a beállítási objektumot a címtárban.

1. Hello meglévő beállítási objektumot keresése:
  ```
  Get-AzureADDirectorySetting | Where-object -Property Displayname -Value "Group.Unified" -EQ
  
  Id                                   DisplayName   TemplateId                           Values
  --                                   -----------   ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  
  $setting = Get-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323
  ```
2. Hello érték frissítése:
  
  ```
  $Setting["AllowToAddGuests"] = "false"
  ```
3. Hello beállítás frissítése:
  
  ```
  Set-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323 -DirectorySetting $Setting
  ```

## <a name="remove-settings-at-hello-directory-level"></a>Távolítsa el a hello könyvtár szintjén beállításait
Ez a lépés beállítások könyvtár szintjén tooall Office csoportok hello directory eltávolítja.
  ```
  Remove-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323c
  ```

## <a name="cmdlet-syntax-reference"></a>A parancsmag szintaxisa referencia
További Azure Active Directory PowerShell dokumentációját a található [Azure Active Directory-modul parancsmagokkal](/powershell/azure/install-adv2?view=azureadps-2.0).

## <a name="additional-reading"></a>További olvasnivaló

* [Hozzáférés tooresources kezelése az Azure Active Directoryval](active-directory-manage-groups.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
