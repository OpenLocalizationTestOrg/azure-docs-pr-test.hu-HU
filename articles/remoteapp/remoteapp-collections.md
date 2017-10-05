---
title: "What kind of collection do you need for Azure RemoteApp? (Milyen típusú gyűjteményre van szüksége az Azure RemoteApphoz?) | Microsoft Docs"
description: "További tudnivalók az Azure RemoteApp rendelkezésre álló gyűjtemények típusú."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: c13ec78d-07e9-4646-8194-cf3efafc1760
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 10f6c0533027767b6635ebff1e6a9872bde06a68
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="what-kind-of-collection-do-you-need-for-azure-remoteapp"></a>What kind of collection do you need for Azure RemoteApp? (Milyen típusú gyűjteményre van szüksége az Azure RemoteApphoz?)
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Az Azure RemoteApp lehetővé teszi az alkalmazások és erőforrások megosztása bármely eszköz felhasználója. A Microsoft ehhez gyűjtemények tárolására az alkalmazások és erőforrások létrehozásával, és majd ezekhez a gyűjteményekhez megosztása felhasználókkal. Nincsenek 2 különböző adatgyűjtési beállítások, a különböző hálózati és a hitelesítési beállítások – amely az Ön számára legmegfelelőbb?

Bemutatjuk, a különböző szempontok és a választási lehetőségek meg kell győződnie, hogy a legtöbbet hozhatja ki az Azure RemoteApp-gyűjteménnyel. 

## <a name="quick-differences-between-the-collection-types"></a>Gyors gyűjtemény típusa közötti különbségek
|  | Felhő | Hibrid |
| --- | --- | --- |
| Egy meglévő virtuális hálózatot |Igen |Igen |
| AD-kapcsolódó fiókok (DirSync) igényel |Nem |Igen |
| Tartományhoz való csatlakozást igényel |Nem |Igen |
| A tartományvezérlő elérhető-e a virtuális hálózatba igényel |Nem |Igen |

## <a name="cloud-collections"></a>Felhőalapú gyűjtemények
* Gyors létrehozása – a gyűjtemény gyorsan kiépítését, ami azt jelenti, az alkalmazások eléréséhez felhasználók gyorsabb.
* Kapcsolja a saját alkalmazásai, vagy a megosztási is. Egy egyéni lemezképet (az Azure virtuális gép létrehozott) vagy a rendszerképeket az előfizetéskor egyikét használhatja.
* Nem kell konfigurálni a gyűjtemény és a helyszíni tartomány közötti kapcsolat.
* De is használhat a saját Azure VNET hozzáférést biztosítanak az adatok megosztása a helyszíni környezetbe, vagy az erőforrásokat, például az SQL Server (adatbázis-hitelesítés használatával) a Windows hitelesítés használatára.

Az OK gombra hogyan hozható létre egy?

* Csak felhőalapú? Hozzon létre a **Gyorslétrehozás** lehetőséget a portálon.
* Felhő + virtuális Hálózatot? Hozzon létre az a **hozzon létre virtuális hálózaton** lehetőséget, de nem választotta a tartományhoz való csatlakozáshoz.

## <a name="hybrid-collections"></a>A hibrid gyűjtemények
* A helyszíni hálózat + Azure VNET teljes hozzáférést biztosítanak.
* Tartományi csatlakozási hozzáférés alkalmazásokat és adatokat tartalmazza. Távoli alkalmazások is a helyszíni Active Directory általi hitelesítési - majd a tartományban lévő erőforrások eléréséhez.
* Engedélyezi speciális figyelésére és (a Windows Server 2012 R2 beépített egyéni lemezkép) meglévő System Center-megoldásokat és a Windows-csoportházirendek kezelése
* Támogatási [ExpressRoute](https://azure.microsoft.com/services/expressroute/) az Azure virtuális hálózat csatlakozni a helyi virtuális hálózat.

Hozzon létre az a **hozzon létre virtuális hálózaton** lehetőséget, majd válassza a tartományhoz való csatlakozáshoz.

## <a name="authentication-options"></a>Hitelesítési lehetőségek
Az Azure RemoteApp támogatja a Microsoft-fiókokat és Azure Active Directory-fiókokat, de nem minden gyűjtemény összes módszer támogatja. 

| Fiók típusa |  | Felhő | Felhő + virtuális hálózat | Hibrid |
| --- | --- | --- | --- | --- |
| Microsoft-fiók | |Igen |Igen |Nem |
| Azure Active Directory (Azure AD) | | | | |
| Csak az Azure AD |Igen |Igen |Nem | |
| AD Connect jelszó-szinkronizálással |Igen |Igen |Igen | |
| AD Connect jelszó-szinkronizálás nélkül |Igen |Igen |Nem | |
| AD Connect az AD FS-sel |Igen |Igen |Igen | |
| 3. fél Azure által támogatott Identitásszolgáltatók (például a Ping) |Igen |Igen |Igen | |
| Multi-Factor Authentication | |Igen |Igen |Igen |

### <a name="cloud-and-cloud--vnet"></a>Felhő és a felhő + a virtuális hálózat
A felhőalapú gyűjtemények használhatja a Microsoft-fiókkal, az Azure AD-fiókok vagy a két vegyesen. A fiókokat, a felhasználók számára leginkább fogja használni.

Nincsenek megadott Microsoft-fiókok használatához. 

Ha szeretné használni az Azure AD-fiókok, győződjön meg arról, hogy az Azure AD-bérlő megfelel az Ön előfizetéséhez rendelve van szükség. Az Azure RemoteApp előfizetés létrehozásakor automatikusan hozzárendelve az előfizetés lett használta az Azure AD-bérlő. Minden Azure AD-felhasználó adjon engedélyt kell lennie, hogy ugyanannak a bérlőnek. Ha szükséges, akkor [az Azure AD-bérlő módosítása](remoteapp-changetenant.md) az Ön előfizetéséhez rendelve.

### <a name="hybrid-or-cloud--azure-ad--ad"></a>Hibrid (vagy a felhő, + az Azure AD + AD)
Az Azure AD + a helyszíni Active Directory hibrid gyűjtemény előfeltétele. AD Connect használatával a két címtár integrálni kell. Azonban néhány választott hogyan konfigurálja az AD Connect ismét. 

Nincsenek 2 AD Connect forgatókönyvek - jelszó-szinkronizálás használatával vagy az AD összevonási. Tekintse meg a [AD Connect információk](../active-directory/active-directory-aadconnect.md) mérje fel, amelyek ezek works legjobb az Ön számára.

Is használhatja az Azure AD + AD felhőalapú gyűjtemény. Ellenőrizze, hogy ugyanaz a beállítása lépéseket hajtsa végre.

Tekintse meg [az Azure AD + Active Directory követelményeit, az Azure RemoteApp](remoteapp-ad.md) az Azure AD konfigurálásához szükséges lépéseket és az Active Directory.

## <a name="go-create-your-collection"></a>Nyissa meg a gyűjtemény létrehozása!
Az OK gombra úgy vélem, azt is szerepelnek,, így csak egy dolog maradt el – az első Azure RemoteApp-gyűjtemény létrehozása.

[Felhőalapú gyűjtemény létrehozása](remoteapp-create-cloud-deployment.md) vagy [hibrid gyűjtemény létrehozása](remoteapp-create-hybrid-deployment.md) -get létrehozni.

