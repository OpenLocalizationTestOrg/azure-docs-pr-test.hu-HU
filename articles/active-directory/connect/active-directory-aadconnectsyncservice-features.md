---
title: "Az Azure AD Connect szinkronizálási szolgáltatások és konfigurációs |} Microsoft Docs"
description: "Az Azure AD Connect szinkronizálási szolgáltatás szolgáltatás ügyféloldali szolgáltatásait ismerteti."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 213aab20-0a61-434a-9545-c4637628da81
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: c2873510c280a2683c235cfdce3d2617c3b665cd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-service-features"></a><span data-ttu-id="88877-103">Az Azure AD Connect szinkronizálási szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="88877-103">Azure AD Connect sync service features</span></span>
<span data-ttu-id="88877-104">Az Azure AD Connect szinkronizálási szolgáltatás két részből áll:</span><span class="sxs-lookup"><span data-stu-id="88877-104">The synchronization feature of Azure AD Connect has two components:</span></span>

* <span data-ttu-id="88877-105">A helyszíni összetevője **az Azure AD Connect szinkronizálási szolgáltatás**is néven **szinkronizálási motor**.</span><span class="sxs-lookup"><span data-stu-id="88877-105">The on-premises component named **Azure AD Connect sync**, also called **sync engine**.</span></span>
* <span data-ttu-id="88877-106">A szolgáltatás, más néven Azure AD-ben található **az Azure AD Connect szinkronizálási szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="88877-106">The service residing in Azure AD also known as **Azure AD Connect sync service**</span></span>

<span data-ttu-id="88877-107">Ez a témakör azt ismerteti, hogyan a következő funkcióit a **az Azure AD Connect szinkronizálási szolgáltatás** munka, és hogyan konfigurálhatja azokat a Windows PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="88877-107">This topic explains how the following features of the **Azure AD Connect sync service** work and how you can configure them using Windows PowerShell.</span></span>

<span data-ttu-id="88877-108">Ezek a beállítások szerint úgy vannak konfigurálva a [Active Directory modul Windows Powershellhez készült Azure](http://aka.ms/aadposh).</span><span class="sxs-lookup"><span data-stu-id="88877-108">These settings are configured by the [Azure Active Directory Module for Windows PowerShell](http://aka.ms/aadposh).</span></span> <span data-ttu-id="88877-109">Külön letölteni és telepíteni azt az Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="88877-109">Download and install it separately from Azure AD Connect.</span></span> <span data-ttu-id="88877-110">Rendszerben az ebben a témakörben a parancsmagokat a [2016. március kiadásban (build 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1).</span><span class="sxs-lookup"><span data-stu-id="88877-110">The cmdlets documented in this topic were introduced in the [2016 March release (build 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1).</span></span> <span data-ttu-id="88877-111">Ha ebben a témakörben a parancsmagokat nem rendelkezik, vagy nem tud létrehozni ugyanazt az eredményt, majd győződjön meg arról, a legújabb verzióját futtatják.</span><span class="sxs-lookup"><span data-stu-id="88877-111">If you do not have the cmdlets documented in this topic or they do not produce the same result, then make sure you run the latest version.</span></span>

<span data-ttu-id="88877-112">Az Azure AD-címtár konfiguráció megtekintéséhez futtassa `Get-MsolDirSyncFeatures`.</span><span class="sxs-lookup"><span data-stu-id="88877-112">To see the configuration in your Azure AD directory, run `Get-MsolDirSyncFeatures`.</span></span>  
<span data-ttu-id="88877-113">![Get-MsolDirSyncFeatures eredménye](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)</span><span class="sxs-lookup"><span data-stu-id="88877-113">![Get-MsolDirSyncFeatures result](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)</span></span>

<span data-ttu-id="88877-114">Számos beállítás csak módosíthatja az Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="88877-114">Many of these settings can only be changed by Azure AD Connect.</span></span>

<span data-ttu-id="88877-115">Az alábbi beállításokat konfigurálhatja `Set-MsolDirSyncFeature`:</span><span class="sxs-lookup"><span data-stu-id="88877-115">The following settings can be configured by `Set-MsolDirSyncFeature`:</span></span>

| <span data-ttu-id="88877-116">DirSyncFeature</span><span class="sxs-lookup"><span data-stu-id="88877-116">DirSyncFeature</span></span> | <span data-ttu-id="88877-117">Megjegyzés</span><span class="sxs-lookup"><span data-stu-id="88877-117">Comment</span></span> |
| --- | --- |
| [<span data-ttu-id="88877-118">EnableSoftMatchOnUpn</span><span class="sxs-lookup"><span data-stu-id="88877-118">EnableSoftMatchOnUpn</span></span>](#userprincipalname-soft-match) |<span data-ttu-id="88877-119">Lehetővé teszi, hogy csatlakozni a userPrincipalName elsődleges SMTP-cím mellett objektumokat.</span><span class="sxs-lookup"><span data-stu-id="88877-119">Allows objects to join on userPrincipalName in addition to primary SMTP address.</span></span> |
| [<span data-ttu-id="88877-120">SynchronizeUpnForManagedUsers</span><span class="sxs-lookup"><span data-stu-id="88877-120">SynchronizeUpnForManagedUsers</span></span>](#synchronize-userprincipalname-updates) |<span data-ttu-id="88877-121">Lehetővé teszi, hogy a szinkronizálási motor a userPrincipalName attribútum a kezelt vagy licenccel rendelkező felhasználók (nem összevont) frissítése.</span><span class="sxs-lookup"><span data-stu-id="88877-121">Allows the sync engine to update the userPrincipalName attribute for managed/licensed (non-federated) users.</span></span> |

<span data-ttu-id="88877-122">Miután engedélyezte a szolgáltatás, nem lehet letiltani újra.</span><span class="sxs-lookup"><span data-stu-id="88877-122">After you have enabled a feature, it cannot be disabled again.</span></span>

> [!NOTE]
> <span data-ttu-id="88877-123">2016 augusztusától 24 a szolgáltatás *ismétlődő attribútum rugalmassági* alapértelmezés szerint engedélyezve van az új Azure AD könyvtárakban.</span><span class="sxs-lookup"><span data-stu-id="88877-123">From August 24, 2016 the feature *Duplicate attribute resiliency* is enabled by default for new Azure AD directories.</span></span> <span data-ttu-id="88877-124">Ez a szolgáltatás is lehet megkezdődött és engedélyezve van ez a dátum előtt létrehozott könyvtárak.</span><span class="sxs-lookup"><span data-stu-id="88877-124">This feature will also be rolled out and enabled on directories created before this date.</span></span> <span data-ttu-id="88877-125">E-mailben értesítést fog kapni, amikor a címtár arra készül, hogy ezt a szolgáltatást, engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="88877-125">You will receive an email notification when your directory is about to get this feature enabled.</span></span>
> 
> 

<span data-ttu-id="88877-126">A következő beállításokat az Azure AD Connect vannak konfigurálva, és nem módosíthatják a `Set-MsolDirSyncFeature`:</span><span class="sxs-lookup"><span data-stu-id="88877-126">The following settings are configured by Azure AD Connect and cannot be modified by `Set-MsolDirSyncFeature`:</span></span>

| <span data-ttu-id="88877-127">DirSyncFeature</span><span class="sxs-lookup"><span data-stu-id="88877-127">DirSyncFeature</span></span> | <span data-ttu-id="88877-128">Megjegyzés</span><span class="sxs-lookup"><span data-stu-id="88877-128">Comment</span></span> |
| --- | --- |
| <span data-ttu-id="88877-129">DeviceWriteback</span><span class="sxs-lookup"><span data-stu-id="88877-129">DeviceWriteback</span></span> |[<span data-ttu-id="88877-130">Az Azure AD Connect: Eszközvisszaírás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="88877-130">Azure AD Connect: Enabling device writeback</span></span>](active-directory-aadconnect-feature-device-writeback.md) |
| <span data-ttu-id="88877-131">DirectoryExtensions</span><span class="sxs-lookup"><span data-stu-id="88877-131">DirectoryExtensions</span></span> |[<span data-ttu-id="88877-132">Azure AD Connect szinkronizálása: címtárbővítmények</span><span class="sxs-lookup"><span data-stu-id="88877-132">Azure AD Connect sync: Directory extensions</span></span>](active-directory-aadconnectsync-feature-directory-extensions.md) |
| [<span data-ttu-id="88877-133">DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency</span><span class="sxs-lookup"><span data-stu-id="88877-133">DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency</span></span>](#duplicate-attribute-resiliency) |<span data-ttu-id="88877-134">Lehetővé teszi, hogy a karanténba kerül, ha egy másik objektum helyett az exportálás során a teljes objektumot sikertelen duplikált attribútum.</span><span class="sxs-lookup"><span data-stu-id="88877-134">Allows an attribute to be quarantined when it is a duplicate of another object rather than failing the entire object during export.</span></span> |
| <span data-ttu-id="88877-135">PasswordSync</span><span class="sxs-lookup"><span data-stu-id="88877-135">PasswordSync</span></span> |[<span data-ttu-id="88877-136">A jelszó-szinkronizálás és az Azure AD Connect-szinkronizálás megvalósítása</span><span class="sxs-lookup"><span data-stu-id="88877-136">Implementing password synchronization with Azure AD Connect sync</span></span>](active-directory-aadconnectsync-implement-password-synchronization.md) |
| <span data-ttu-id="88877-137">UnifiedGroupWriteback</span><span class="sxs-lookup"><span data-stu-id="88877-137">UnifiedGroupWriteback</span></span> |[<span data-ttu-id="88877-138">Előzetes verzió: A csoportvisszaírás</span><span class="sxs-lookup"><span data-stu-id="88877-138">Preview: Group writeback</span></span>](active-directory-aadconnect-feature-preview.md#group-writeback) |
| <span data-ttu-id="88877-139">UserWriteback</span><span class="sxs-lookup"><span data-stu-id="88877-139">UserWriteback</span></span> |<span data-ttu-id="88877-140">Jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="88877-140">Not currently supported.</span></span> |

## <a name="duplicate-attribute-resiliency"></a><span data-ttu-id="88877-141">Ismétlődő attribútum rugalmasság</span><span class="sxs-lookup"><span data-stu-id="88877-141">Duplicate attribute resiliency</span></span>
<span data-ttu-id="88877-142">Ismétlődő egyszerű felhasználónevek objektumot hibás kiépítését / proxyAddresses, az ismétlődő attribútum "karanténba", és egy ideiglenes érték hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="88877-142">Instead of failing to provision objects with duplicate UPNs / proxyAddresses, the duplicated attribute is “quarantined” and a temporary value is assigned.</span></span> <span data-ttu-id="88877-143">Amikor feloldja az ütközést, az ideiglenes UPN automatikusan megváltozik a megfelelő értéket.</span><span class="sxs-lookup"><span data-stu-id="88877-143">When the conflict is resolved, the temporary UPN is changed to the proper value automatically.</span></span> <span data-ttu-id="88877-144">További részletekért lásd: [identitás-szinkronizálással és duplikált attribútum rugalmassági](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).</span><span class="sxs-lookup"><span data-stu-id="88877-144">For more details, see [Identity synchronization and duplicate attribute resiliency](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).</span></span>

## <a name="userprincipalname-soft-match"></a><span data-ttu-id="88877-145">UserPrincipalName enyhe egyezés</span><span class="sxs-lookup"><span data-stu-id="88877-145">UserPrincipalName soft match</span></span>
<span data-ttu-id="88877-146">Ha ez a funkció engedélyezve van, soft-match kívül UPN engedélyezve van a [elsődleges SMTP-cím](https://support.microsoft.com/kb/2641663), amely mindig engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="88877-146">When this feature is enabled, soft-match is enabled for UPN in addition to the [primary SMTP address](https://support.microsoft.com/kb/2641663), which is always enabled.</span></span> <span data-ttu-id="88877-147">Soft-match felelnek meg a meglévő felhőalapú felhasználók az Azure AD helyszíni felhasználók szolgál.</span><span class="sxs-lookup"><span data-stu-id="88877-147">Soft-match is used to match existing cloud users in Azure AD with on-premises users.</span></span>

<span data-ttu-id="88877-148">Ha szeretné egyezés a helyszíni AD-fiókok a felhőben létrehozott meglévő fiókokhoz nem használ az Exchange Online, majd a szolgáltatás akkor hasznos.</span><span class="sxs-lookup"><span data-stu-id="88877-148">If you need to match on-premises AD accounts with existing accounts created in the cloud and you are not using Exchange Online, then this feature is useful.</span></span> <span data-ttu-id="88877-149">Ebben a forgatókönyvben általában nem rendelkezik SMTP attribútum beállítása a felhőben okát.</span><span class="sxs-lookup"><span data-stu-id="88877-149">In this scenario, you generally don’t have a reason to set the SMTP attribute in the cloud.</span></span>

<span data-ttu-id="88877-150">Ez a funkció a alapértelmezés szerint most hozták létre Azure AD-címtártól.</span><span class="sxs-lookup"><span data-stu-id="88877-150">This feature is on by default for newly created Azure AD directories.</span></span> <span data-ttu-id="88877-151">Ha ezt a szolgáltatást, futtassa a látható:</span><span class="sxs-lookup"><span data-stu-id="88877-151">You can see if this feature is enabled for you by running:</span></span>  

```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

<span data-ttu-id="88877-152">Ha ez a funkció nincs engedélyezve az Azure AD-címtárát, majd engedélyezheti azt futtatásával:</span><span class="sxs-lookup"><span data-stu-id="88877-152">If this feature is not enabled for your Azure AD directory, then you can enable it by running:</span></span>  

```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a><span data-ttu-id="88877-153">UserPrincipalName a frissítések szinkronizálása</span><span class="sxs-lookup"><span data-stu-id="88877-153">Synchronize userPrincipalName updates</span></span>
<span data-ttu-id="88877-154">Hagyományosan a UserPrincipalName attribútum a szinkronizálási szolgáltatás használatát a helyszíni frissítéseit le van tiltva, kivéve, ha az alábbi két feltétel teljesül:</span><span class="sxs-lookup"><span data-stu-id="88877-154">Historically, updates to the UserPrincipalName attribute using the sync service from on-premises has been blocked, unless both of these conditions are true:</span></span>

* <span data-ttu-id="88877-155">A felhasználó (nem összevont) kezeli.</span><span class="sxs-lookup"><span data-stu-id="88877-155">The user is managed (non-federated).</span></span>
* <span data-ttu-id="88877-156">A felhasználó nincs hozzárendelve egy licencet.</span><span class="sxs-lookup"><span data-stu-id="88877-156">The user has not been assigned a license.</span></span>

<span data-ttu-id="88877-157">További részletekért lásd: [Office 365, az Azure vagy az Intune-ban szereplő felhasználónevek nem egyeznek meg, a helyszíni egyszerű Felhasználónévvel vagy másodlagos bejelentkezési Azonosítóval](https://support.microsoft.com/kb/2523192).</span><span class="sxs-lookup"><span data-stu-id="88877-157">For more details, see [User names in Office 365, Azure, or Intune don't match the on-premises UPN or alternate login ID](https://support.microsoft.com/kb/2523192).</span></span>

<span data-ttu-id="88877-158">Ez a funkció lehetővé teszi, hogy a szinkronizálási motor a userPrincipalName frissíteni, ha megváltozott a helyszíni és jelszó-szinkronizálást használ.</span><span class="sxs-lookup"><span data-stu-id="88877-158">Enabling this feature allows the sync engine to update the userPrincipalName when it is changed on-premises and you use password sync.</span></span> <span data-ttu-id="88877-159">Összevonási használja, ha ez a funkció nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="88877-159">If you use federation, this feature is not supported.</span></span>

<span data-ttu-id="88877-160">Ez a funkció a alapértelmezés szerint most hozták létre Azure AD-címtártól.</span><span class="sxs-lookup"><span data-stu-id="88877-160">This feature is on by default for newly created Azure AD directories.</span></span> <span data-ttu-id="88877-161">Ha ezt a szolgáltatást, futtassa a látható:</span><span class="sxs-lookup"><span data-stu-id="88877-161">You can see if this feature is enabled for you by running:</span></span>  

```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

<span data-ttu-id="88877-162">Ha ez a funkció nincs engedélyezve az Azure AD-címtárát, majd engedélyezheti azt futtatásával:</span><span class="sxs-lookup"><span data-stu-id="88877-162">If this feature is not enabled for your Azure AD directory, then you can enable it by running:</span></span>  

```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

<span data-ttu-id="88877-163">A funkció engedélyezése után meglévő userPrincipalName értékek marad-van.</span><span class="sxs-lookup"><span data-stu-id="88877-163">After enabling this feature, existing userPrincipalName values will remain as-is.</span></span> <span data-ttu-id="88877-164">A következő módosításakor a userPrincipalName attribútum helyszínen a normál eltérés szinkronizálása a felhasználók frissíteni fogja az egyszerű Felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="88877-164">On next change of the userPrincipalName attribute on-premises, the normal delta sync on users will update the UPN.</span></span>  

## <a name="see-also"></a><span data-ttu-id="88877-165">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="88877-165">See also</span></span>
* [<span data-ttu-id="88877-166">Az Azure AD Connect szinkronizálása</span><span class="sxs-lookup"><span data-stu-id="88877-166">Azure AD Connect sync</span></span>](active-directory-aadconnectsync-whatis.md)
* <span data-ttu-id="88877-167">[A helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="88877-167">[Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

