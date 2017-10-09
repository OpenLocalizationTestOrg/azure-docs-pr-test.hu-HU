---
title: "aaaAzure AD Connect szinkronizálási szolgáltatás szolgáltatásait és konfigurációs |} Microsoft Docs"
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
ms.openlocfilehash: 7ad05c45bb6b5fd5deaa3466e2936b19d3d9eabb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-service-features"></a><span data-ttu-id="67e87-103">Az Azure AD Connect szinkronizálási szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="67e87-103">Azure AD Connect sync service features</span></span>
<span data-ttu-id="67e87-104">hello szinkronizálási szolgáltatás az Azure AD Connect két részből áll:</span><span class="sxs-lookup"><span data-stu-id="67e87-104">hello synchronization feature of Azure AD Connect has two components:</span></span>

* <span data-ttu-id="67e87-105">nevű hello helyszíni összetevő **az Azure AD Connect szinkronizálási szolgáltatás**is néven **szinkronizálási motor**.</span><span class="sxs-lookup"><span data-stu-id="67e87-105">hello on-premises component named **Azure AD Connect sync**, also called **sync engine**.</span></span>
* <span data-ttu-id="67e87-106">az Azure ad-ben más néven található hello szolgáltatás **az Azure AD Connect szinkronizálási szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="67e87-106">hello service residing in Azure AD also known as **Azure AD Connect sync service**</span></span>

<span data-ttu-id="67e87-107">Ez a témakör ismerteti, hogyan hello a következő szolgáltatásokat a hello **az Azure AD Connect szinkronizálási szolgáltatás** munka, és hogyan konfigurálhatja azokat a Windows PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="67e87-107">This topic explains how hello following features of hello **Azure AD Connect sync service** work and how you can configure them using Windows PowerShell.</span></span>

<span data-ttu-id="67e87-108">Ezek a beállítások hello által konfigurált [Active Directory modul Windows Powershellhez készült Azure](http://aka.ms/aadposh).</span><span class="sxs-lookup"><span data-stu-id="67e87-108">These settings are configured by hello [Azure Active Directory Module for Windows PowerShell](http://aka.ms/aadposh).</span></span> <span data-ttu-id="67e87-109">Külön letölteni és telepíteni azt az Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="67e87-109">Download and install it separately from Azure AD Connect.</span></span> <span data-ttu-id="67e87-110">Ebben a témakörben ismertetett hello parancsmagok bevezetett hello [2016. március kiadásban (build 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1).</span><span class="sxs-lookup"><span data-stu-id="67e87-110">hello cmdlets documented in this topic were introduced in hello [2016 March release (build 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1).</span></span> <span data-ttu-id="67e87-111">Ha nem rendelkezik hello parancsmagokat ebben a témakörben, vagy nem tud létrehozni hello azonos vezethet, majd győződjön meg arról, hogy hello legújabb verzióját futtatja.</span><span class="sxs-lookup"><span data-stu-id="67e87-111">If you do not have hello cmdlets documented in this topic or they do not produce hello same result, then make sure you run hello latest version.</span></span>

<span data-ttu-id="67e87-112">toosee hello konfigurációját az Azure AD-címtár, futtassa a `Get-MsolDirSyncFeatures`.</span><span class="sxs-lookup"><span data-stu-id="67e87-112">toosee hello configuration in your Azure AD directory, run `Get-MsolDirSyncFeatures`.</span></span>  
<span data-ttu-id="67e87-113">![Get-MsolDirSyncFeatures eredménye](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)</span><span class="sxs-lookup"><span data-stu-id="67e87-113">![Get-MsolDirSyncFeatures result](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)</span></span>

<span data-ttu-id="67e87-114">Számos beállítás csak módosíthatja az Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="67e87-114">Many of these settings can only be changed by Azure AD Connect.</span></span>

<span data-ttu-id="67e87-115">hello következő beállításait konfigurálhatja `Set-MsolDirSyncFeature`:</span><span class="sxs-lookup"><span data-stu-id="67e87-115">hello following settings can be configured by `Set-MsolDirSyncFeature`:</span></span>

| <span data-ttu-id="67e87-116">DirSyncFeature</span><span class="sxs-lookup"><span data-stu-id="67e87-116">DirSyncFeature</span></span> | <span data-ttu-id="67e87-117">Megjegyzés</span><span class="sxs-lookup"><span data-stu-id="67e87-117">Comment</span></span> |
| --- | --- |
| [<span data-ttu-id="67e87-118">EnableSoftMatchOnUpn</span><span class="sxs-lookup"><span data-stu-id="67e87-118">EnableSoftMatchOnUpn</span></span>](#userprincipalname-soft-match) |<span data-ttu-id="67e87-119">Lehetővé teszi az objektumok toojoin userPrincipalName tooprimary SMTP-cím hozzáadását.</span><span class="sxs-lookup"><span data-stu-id="67e87-119">Allows objects toojoin on userPrincipalName in addition tooprimary SMTP address.</span></span> |
| [<span data-ttu-id="67e87-120">SynchronizeUpnForManagedUsers</span><span class="sxs-lookup"><span data-stu-id="67e87-120">SynchronizeUpnForManagedUsers</span></span>](#synchronize-userprincipalname-updates) |<span data-ttu-id="67e87-121">Lehetővé teszi a hello szinkronizálási motor tooupdate hello userPrincipalName attribútum a kezelt vagy licenccel rendelkező felhasználók (nem összevont).</span><span class="sxs-lookup"><span data-stu-id="67e87-121">Allows hello sync engine tooupdate hello userPrincipalName attribute for managed/licensed (non-federated) users.</span></span> |

<span data-ttu-id="67e87-122">Miután engedélyezte a szolgáltatás, nem lehet letiltani újra.</span><span class="sxs-lookup"><span data-stu-id="67e87-122">After you have enabled a feature, it cannot be disabled again.</span></span>

> [!NOTE]
> <span data-ttu-id="67e87-123">2016 augusztusától 24 hello szolgáltatás *ismétlődő attribútum rugalmassági* alapértelmezés szerint engedélyezve van az új Azure AD könyvtárakban.</span><span class="sxs-lookup"><span data-stu-id="67e87-123">From August 24, 2016 hello feature *Duplicate attribute resiliency* is enabled by default for new Azure AD directories.</span></span> <span data-ttu-id="67e87-124">Ez a szolgáltatás is lehet megkezdődött és engedélyezve van ez a dátum előtt létrehozott könyvtárak.</span><span class="sxs-lookup"><span data-stu-id="67e87-124">This feature will also be rolled out and enabled on directories created before this date.</span></span> <span data-ttu-id="67e87-125">E-mailben értesítést fog kapni, ha a címtár kapcsolatos tooget Ez a funkció engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="67e87-125">You will receive an email notification when your directory is about tooget this feature enabled.</span></span>
> 
> 

<span data-ttu-id="67e87-126">hello következő beállítások konfigurálva az Azure AD Connect, és nem módosíthatják a `Set-MsolDirSyncFeature`:</span><span class="sxs-lookup"><span data-stu-id="67e87-126">hello following settings are configured by Azure AD Connect and cannot be modified by `Set-MsolDirSyncFeature`:</span></span>

| <span data-ttu-id="67e87-127">DirSyncFeature</span><span class="sxs-lookup"><span data-stu-id="67e87-127">DirSyncFeature</span></span> | <span data-ttu-id="67e87-128">Megjegyzés</span><span class="sxs-lookup"><span data-stu-id="67e87-128">Comment</span></span> |
| --- | --- |
| <span data-ttu-id="67e87-129">DeviceWriteback</span><span class="sxs-lookup"><span data-stu-id="67e87-129">DeviceWriteback</span></span> |[<span data-ttu-id="67e87-130">Az Azure AD Connect: Eszközvisszaírás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="67e87-130">Azure AD Connect: Enabling device writeback</span></span>](active-directory-aadconnect-feature-device-writeback.md) |
| <span data-ttu-id="67e87-131">DirectoryExtensions</span><span class="sxs-lookup"><span data-stu-id="67e87-131">DirectoryExtensions</span></span> |[<span data-ttu-id="67e87-132">Azure AD Connect szinkronizálása: címtárbővítmények</span><span class="sxs-lookup"><span data-stu-id="67e87-132">Azure AD Connect sync: Directory extensions</span></span>](active-directory-aadconnectsync-feature-directory-extensions.md) |
| [<span data-ttu-id="67e87-133">DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency</span><span class="sxs-lookup"><span data-stu-id="67e87-133">DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency</span></span>](#duplicate-attribute-resiliency) |<span data-ttu-id="67e87-134">Lehetővé teszi, hogy az attribútum toobe karanténba helyezve, amikor az üzenet egy másolat egy másik objektum helyett az exportálás során hello teljes objektum sikertelen.</span><span class="sxs-lookup"><span data-stu-id="67e87-134">Allows an attribute toobe quarantined when it is a duplicate of another object rather than failing hello entire object during export.</span></span> |
| <span data-ttu-id="67e87-135">PasswordSync</span><span class="sxs-lookup"><span data-stu-id="67e87-135">PasswordSync</span></span> |[<span data-ttu-id="67e87-136">A jelszó-szinkronizálás és az Azure AD Connect-szinkronizálás megvalósítása</span><span class="sxs-lookup"><span data-stu-id="67e87-136">Implementing password synchronization with Azure AD Connect sync</span></span>](active-directory-aadconnectsync-implement-password-synchronization.md) |
| <span data-ttu-id="67e87-137">UnifiedGroupWriteback</span><span class="sxs-lookup"><span data-stu-id="67e87-137">UnifiedGroupWriteback</span></span> |[<span data-ttu-id="67e87-138">Előzetes verzió: A csoportvisszaírás</span><span class="sxs-lookup"><span data-stu-id="67e87-138">Preview: Group writeback</span></span>](active-directory-aadconnect-feature-preview.md#group-writeback) |
| <span data-ttu-id="67e87-139">UserWriteback</span><span class="sxs-lookup"><span data-stu-id="67e87-139">UserWriteback</span></span> |<span data-ttu-id="67e87-140">Jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="67e87-140">Not currently supported.</span></span> |

## <a name="duplicate-attribute-resiliency"></a><span data-ttu-id="67e87-141">Ismétlődő attribútum rugalmasság</span><span class="sxs-lookup"><span data-stu-id="67e87-141">Duplicate attribute resiliency</span></span>
<span data-ttu-id="67e87-142">Az ismétlődő egyszerű felhasználónevek objektumok helyett tooprovision sikertelen / proxyAddresses, hello duplikált attribútum "karanténba", és egy ideiglenes érték hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="67e87-142">Instead of failing tooprovision objects with duplicate UPNs / proxyAddresses, hello duplicated attribute is “quarantined” and a temporary value is assigned.</span></span> <span data-ttu-id="67e87-143">Amikor hello ütközés fel lett oldva, hello ideiglenes egyszerű felhasználónév értéke automatikusan módosította toohello megfelelő értéket.</span><span class="sxs-lookup"><span data-stu-id="67e87-143">When hello conflict is resolved, hello temporary UPN is changed toohello proper value automatically.</span></span> <span data-ttu-id="67e87-144">További részletekért lásd: [identitás-szinkronizálással és duplikált attribútum rugalmassági](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).</span><span class="sxs-lookup"><span data-stu-id="67e87-144">For more details, see [Identity synchronization and duplicate attribute resiliency](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).</span></span>

## <a name="userprincipalname-soft-match"></a><span data-ttu-id="67e87-145">UserPrincipalName enyhe egyezés</span><span class="sxs-lookup"><span data-stu-id="67e87-145">UserPrincipalName soft match</span></span>
<span data-ttu-id="67e87-146">Ha ez a funkció engedélyezve van, soft-match engedélyezve van az egyszerű felhasználónév hozzáadása toohello [elsődleges SMTP-cím](https://support.microsoft.com/kb/2641663), amely mindig engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="67e87-146">When this feature is enabled, soft-match is enabled for UPN in addition toohello [primary SMTP address](https://support.microsoft.com/kb/2641663), which is always enabled.</span></span> <span data-ttu-id="67e87-147">Soft-match használt toomatch meglévő felhő felhasználók a helyszíni felhasználók az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="67e87-147">Soft-match is used toomatch existing cloud users in Azure AD with on-premises users.</span></span>

<span data-ttu-id="67e87-148">Ha szüksége toomatch a helyszíni AD-fiókok hello felhőben létrehozott meglévő fiókokhoz nem használ az Exchange Online, majd a szolgáltatás akkor hasznos.</span><span class="sxs-lookup"><span data-stu-id="67e87-148">If you need toomatch on-premises AD accounts with existing accounts created in hello cloud and you are not using Exchange Online, then this feature is useful.</span></span> <span data-ttu-id="67e87-149">Ebben a forgatókönyvben általában nem rendelkezik egy oka tooset hello SMTP attribútum hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="67e87-149">In this scenario, you generally don’t have a reason tooset hello SMTP attribute in hello cloud.</span></span>

<span data-ttu-id="67e87-150">Ez a funkció a alapértelmezés szerint most hozták létre Azure AD-címtártól.</span><span class="sxs-lookup"><span data-stu-id="67e87-150">This feature is on by default for newly created Azure AD directories.</span></span> <span data-ttu-id="67e87-151">Ha ezt a szolgáltatást, futtassa a látható:</span><span class="sxs-lookup"><span data-stu-id="67e87-151">You can see if this feature is enabled for you by running:</span></span>  

```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

<span data-ttu-id="67e87-152">Ha ez a funkció nincs engedélyezve az Azure AD-címtárát, majd engedélyezheti azt futtatásával:</span><span class="sxs-lookup"><span data-stu-id="67e87-152">If this feature is not enabled for your Azure AD directory, then you can enable it by running:</span></span>  

```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a><span data-ttu-id="67e87-153">UserPrincipalName a frissítések szinkronizálása</span><span class="sxs-lookup"><span data-stu-id="67e87-153">Synchronize userPrincipalName updates</span></span>
<span data-ttu-id="67e87-154">Hagyományosan frissítések toohello UserPrincipalName attribútum hello szinkronizálási szolgáltatás használatát a helyszíni le van tiltva, kivéve, ha az alábbi két feltétel teljesül:</span><span class="sxs-lookup"><span data-stu-id="67e87-154">Historically, updates toohello UserPrincipalName attribute using hello sync service from on-premises has been blocked, unless both of these conditions are true:</span></span>

* <span data-ttu-id="67e87-155">hello felhasználói (nem összevont) kezeli.</span><span class="sxs-lookup"><span data-stu-id="67e87-155">hello user is managed (non-federated).</span></span>
* <span data-ttu-id="67e87-156">hello felhasználó nincs hozzárendelve egy licencet.</span><span class="sxs-lookup"><span data-stu-id="67e87-156">hello user has not been assigned a license.</span></span>

<span data-ttu-id="67e87-157">További részletekért lásd: [felhasználói az Office 365, az Azure vagy az Intune-ban neve nem egyezik meg a helyszíni egyszerű Felhasználónévvel vagy másodlagos bejelentkezési Azonosítóval hello](https://support.microsoft.com/kb/2523192).</span><span class="sxs-lookup"><span data-stu-id="67e87-157">For more details, see [User names in Office 365, Azure, or Intune don't match hello on-premises UPN or alternate login ID](https://support.microsoft.com/kb/2523192).</span></span>

<span data-ttu-id="67e87-158">Ez a funkció lehetővé teszi, hogy hello szinkronizálási motor tooupdate hello userPrincipalName ha megváltozott a helyszíni és jelszó-szinkronizálást használ. Összevonási használja, ha ez a funkció nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="67e87-158">Enabling this feature allows hello sync engine tooupdate hello userPrincipalName when it is changed on-premises and you use password sync. If you use federation, this feature is not supported.</span></span>

<span data-ttu-id="67e87-159">Ez a funkció a alapértelmezés szerint most hozták létre Azure AD-címtártól.</span><span class="sxs-lookup"><span data-stu-id="67e87-159">This feature is on by default for newly created Azure AD directories.</span></span> <span data-ttu-id="67e87-160">Ha ezt a szolgáltatást, futtassa a látható:</span><span class="sxs-lookup"><span data-stu-id="67e87-160">You can see if this feature is enabled for you by running:</span></span>  

```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

<span data-ttu-id="67e87-161">Ha ez a funkció nincs engedélyezve az Azure AD-címtárát, majd engedélyezheti azt futtatásával:</span><span class="sxs-lookup"><span data-stu-id="67e87-161">If this feature is not enabled for your Azure AD directory, then you can enable it by running:</span></span>  

```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

<span data-ttu-id="67e87-162">A funkció engedélyezése után meglévő userPrincipalName értékek marad-van.</span><span class="sxs-lookup"><span data-stu-id="67e87-162">After enabling this feature, existing userPrincipalName values will remain as-is.</span></span> <span data-ttu-id="67e87-163">A következő módosításakor a hello userPrincipalName attribútum a helyszíni hello normál eltérés szinkronizálása a felhasználók hello UPN frissíti.</span><span class="sxs-lookup"><span data-stu-id="67e87-163">On next change of hello userPrincipalName attribute on-premises, hello normal delta sync on users will update hello UPN.</span></span>  

## <a name="see-also"></a><span data-ttu-id="67e87-164">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="67e87-164">See also</span></span>
* [<span data-ttu-id="67e87-165">Az Azure AD Connect szinkronizálása</span><span class="sxs-lookup"><span data-stu-id="67e87-165">Azure AD Connect sync</span></span>](active-directory-aadconnectsync-whatis.md)
* <span data-ttu-id="67e87-166">[A helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="67e87-166">[Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

