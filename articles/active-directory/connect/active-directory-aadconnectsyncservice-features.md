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
# <a name="azure-ad-connect-sync-service-features"></a>Az Azure AD Connect szinkronizálási szolgáltatások
hello szinkronizálási szolgáltatás az Azure AD Connect két részből áll:

* nevű hello helyszíni összetevő **az Azure AD Connect szinkronizálási szolgáltatás**is néven **szinkronizálási motor**.
* az Azure ad-ben más néven található hello szolgáltatás **az Azure AD Connect szinkronizálási szolgáltatás**

Ez a témakör ismerteti, hogyan hello a következő szolgáltatásokat a hello **az Azure AD Connect szinkronizálási szolgáltatás** munka, és hogyan konfigurálhatja azokat a Windows PowerShell használatával.

Ezek a beállítások hello által konfigurált [Active Directory modul Windows Powershellhez készült Azure](http://aka.ms/aadposh). Külön letölteni és telepíteni azt az Azure AD Connect. Ebben a témakörben ismertetett hello parancsmagok bevezetett hello [2016. március kiadásban (build 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1). Ha nem rendelkezik hello parancsmagokat ebben a témakörben, vagy nem tud létrehozni hello azonos vezethet, majd győződjön meg arról, hogy hello legújabb verzióját futtatja.

toosee hello konfigurációját az Azure AD-címtár, futtassa a `Get-MsolDirSyncFeatures`.  
![Get-MsolDirSyncFeatures eredménye](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)

Számos beállítás csak módosíthatja az Azure AD Connect.

hello következő beállításait konfigurálhatja `Set-MsolDirSyncFeature`:

| DirSyncFeature | Megjegyzés |
| --- | --- |
| [EnableSoftMatchOnUpn](#userprincipalname-soft-match) |Lehetővé teszi az objektumok toojoin userPrincipalName tooprimary SMTP-cím hozzáadását. |
| [SynchronizeUpnForManagedUsers](#synchronize-userprincipalname-updates) |Lehetővé teszi a hello szinkronizálási motor tooupdate hello userPrincipalName attribútum a kezelt vagy licenccel rendelkező felhasználók (nem összevont). |

Miután engedélyezte a szolgáltatás, nem lehet letiltani újra.

> [!NOTE]
> 2016 augusztusától 24 hello szolgáltatás *ismétlődő attribútum rugalmassági* alapértelmezés szerint engedélyezve van az új Azure AD könyvtárakban. Ez a szolgáltatás is lehet megkezdődött és engedélyezve van ez a dátum előtt létrehozott könyvtárak. E-mailben értesítést fog kapni, ha a címtár kapcsolatos tooget Ez a funkció engedélyezve van.
> 
> 

hello következő beállítások konfigurálva az Azure AD Connect, és nem módosíthatják a `Set-MsolDirSyncFeature`:

| DirSyncFeature | Megjegyzés |
| --- | --- |
| DeviceWriteback |[Az Azure AD Connect: Eszközvisszaírás engedélyezése](active-directory-aadconnect-feature-device-writeback.md) |
| DirectoryExtensions |[Azure AD Connect szinkronizálása: címtárbővítmények](active-directory-aadconnectsync-feature-directory-extensions.md) |
| [DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency](#duplicate-attribute-resiliency) |Lehetővé teszi, hogy az attribútum toobe karanténba helyezve, amikor az üzenet egy másolat egy másik objektum helyett az exportálás során hello teljes objektum sikertelen. |
| PasswordSync |[A jelszó-szinkronizálás és az Azure AD Connect-szinkronizálás megvalósítása](active-directory-aadconnectsync-implement-password-synchronization.md) |
| UnifiedGroupWriteback |[Előzetes verzió: A csoportvisszaírás](active-directory-aadconnect-feature-preview.md#group-writeback) |
| UserWriteback |Jelenleg nem támogatott. |

## <a name="duplicate-attribute-resiliency"></a>Ismétlődő attribútum rugalmasság
Az ismétlődő egyszerű felhasználónevek objektumok helyett tooprovision sikertelen / proxyAddresses, hello duplikált attribútum "karanténba", és egy ideiglenes érték hozzá van rendelve. Amikor hello ütközés fel lett oldva, hello ideiglenes egyszerű felhasználónév értéke automatikusan módosította toohello megfelelő értéket. További részletekért lásd: [identitás-szinkronizálással és duplikált attribútum rugalmassági](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).

## <a name="userprincipalname-soft-match"></a>UserPrincipalName enyhe egyezés
Ha ez a funkció engedélyezve van, soft-match engedélyezve van az egyszerű felhasználónév hozzáadása toohello [elsődleges SMTP-cím](https://support.microsoft.com/kb/2641663), amely mindig engedélyezve van. Soft-match használt toomatch meglévő felhő felhasználók a helyszíni felhasználók az Azure AD-ben.

Ha szüksége toomatch a helyszíni AD-fiókok hello felhőben létrehozott meglévő fiókokhoz nem használ az Exchange Online, majd a szolgáltatás akkor hasznos. Ebben a forgatókönyvben általában nem rendelkezik egy oka tooset hello SMTP attribútum hello felhőben.

Ez a funkció a alapértelmezés szerint most hozták létre Azure AD-címtártól. Ha ezt a szolgáltatást, futtassa a látható:  

```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

Ha ez a funkció nincs engedélyezve az Azure AD-címtárát, majd engedélyezheti azt futtatásával:  

```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a>UserPrincipalName a frissítések szinkronizálása
Hagyományosan frissítések toohello UserPrincipalName attribútum hello szinkronizálási szolgáltatás használatát a helyszíni le van tiltva, kivéve, ha az alábbi két feltétel teljesül:

* hello felhasználói (nem összevont) kezeli.
* hello felhasználó nincs hozzárendelve egy licencet.

További részletekért lásd: [felhasználói az Office 365, az Azure vagy az Intune-ban neve nem egyezik meg a helyszíni egyszerű Felhasználónévvel vagy másodlagos bejelentkezési Azonosítóval hello](https://support.microsoft.com/kb/2523192).

Ez a funkció lehetővé teszi, hogy hello szinkronizálási motor tooupdate hello userPrincipalName ha megváltozott a helyszíni és jelszó-szinkronizálást használ. Összevonási használja, ha ez a funkció nem támogatott.

Ez a funkció a alapértelmezés szerint most hozták létre Azure AD-címtártól. Ha ezt a szolgáltatást, futtassa a látható:  

```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

Ha ez a funkció nincs engedélyezve az Azure AD-címtárát, majd engedélyezheti azt futtatásával:  

```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

A funkció engedélyezése után meglévő userPrincipalName értékek marad-van. A következő módosításakor a hello userPrincipalName attribútum a helyszíni hello normál eltérés szinkronizálása a felhasználók hello UPN frissíti.  

## <a name="see-also"></a>Lásd még:
* [Az Azure AD Connect szinkronizálása](active-directory-aadconnectsync-whatis.md)
* [A helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).

