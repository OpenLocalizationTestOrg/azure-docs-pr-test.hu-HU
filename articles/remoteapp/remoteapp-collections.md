---
title: "gyűjtemény do aaaWhat típusú van szüksége az Azure RemoteApp? | Microsoft Docs"
description: "További tudnivalók az Azure RemoteApp rendelkezésre álló gyűjtemények hello típusú."
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
ms.openlocfilehash: f00b5fe41af597cf75e26300bf7842c3a8ff94fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-kind-of-collection-do-you-need-for-azure-remoteapp"></a>What kind of collection do you need for Azure RemoteApp? (Milyen típusú gyűjteményre van szüksége az Azure RemoteApphoz?)
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Az Azure RemoteApp lehetővé teszi az alkalmazások és erőforrások megosztása bármely eszköz felhasználója. Azt ehhez hozzon létre gyűjteményt toohold hello alkalmazásokhoz és erőforrásokhoz, és majd ezekhez a gyűjteményekhez megosztása felhasználókkal. Nincsenek 2 különböző adatgyűjtési beállítások, a különböző hálózati és a hitelesítési beállítások – amely az Ön számára legmegfelelőbb?

Bemutatjuk, hello kapcsolatos szempontokat és lehetőségeket kell toomake tooget hello legtöbb kívül az Azure RemoteApp-gyűjteménnyel. 

## <a name="quick-differences-between-hello-collection-types"></a>Hello gyűjteménytípusok gyors különbségei
|  | Felhő | Hibrid |
| --- | --- | --- |
| Egy meglévő virtuális hálózatot |Igen |Igen |
| AD-kapcsolódó fiókok (DirSync) igényel |Nem |Igen |
| Tartományhoz való csatlakozást igényel |Nem |Igen |
| Tartomány a tartományvezérlő elérhető tooVNET igényel |Nem |Igen |

## <a name="cloud-collections"></a>Felhőalapú gyűjtemények
* Gyors toocreate - hello gyűjtemény gyorsan üzembe, ami azt jelenti, az alkalmazások toousers gyorsabb beolvasása.
* Kapcsolja a saját alkalmazásai, vagy a megosztási is. (Az Azure virtuális gép létrehozott) egyéni lemezkép vagy hello rendszerképeket az előfizetéskor egyikét használhatja.
* A gyűjtemény és a helyszíni tartománya között nincs szükség a tooconfigure kapcsolatot.
* De is használhat a saját Azure VNET tooprovide hozzáférés a helyszíni környezetbe adatok megosztása vagy toouse-Windows hitelesítés az erőforrásokat, például az SQL Server (adatbázis-hitelesítés használatával).

Az OK gombra hogyan hozható létre egy?

* Csak felhőalapú? Hozza létre a hello **Gyorslétrehozás** hello portálon lehetőséget.
* Felhő + virtuális Hálózatot? Hozzon létre az hello **virtuális hálózaton hozzon létre** lehetőséget, de nem adja meg toojoin egy tartományhoz.

## <a name="hybrid-collections"></a>A hibrid gyűjtemények
* Adja meg a teljes körű hozzáférési tooon helyszíni hálózati + Azure VNET.
* Tartományi csatlakozási hozzáférés alkalmazásokat és adatokat tartalmazza. Távoli alkalmazások is a helyszíni Active Directory általi hitelesítési - majd a tartományban lévő erőforrások eléréséhez.
* Engedélyezi speciális figyelésére és (a Windows Server 2012 R2 beépített egyéni lemezkép) meglévő System Center-megoldásokat és a Windows-csoportházirendek kezelése
* Támogatási [ExpressRoute](https://azure.microsoft.com/services/expressroute/) tooconnect az Azure virtuális hálózat tooyour helyi hálózatok.

Hozzon létre az hello **hozzon létre virtuális hálózaton** lehetőséget, majd válassza ki toojoin a tartományt.

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
A felhőalapú gyűjtemények használhatja a Microsoft-fiókkal, az Azure AD-fiókok vagy hello két vegyesen. A felhasználók számára leginkább hello fiókokat fogja használni.

Nincsenek megadott Microsoft-fiókok használatához. 

Ha azt szeretné, hogy az Azure AD-fiókok toouse, van szüksége arról, hogy az Azure AD-bérlő megegyezik-e az Ön előfizetéséhez rendelve egy hello toomake. Az Azure RemoteApp előfizetés létrehozásakor használt hello Azure AD-bérlő automatikusan hozzárendelve az előfizetés volt. Minden Azure AD-felhasználó ad engedélyt tooneeds toobe ugyanannak a bérlőnek. Ha szükséges, akkor [hello Azure AD-bérlő módosítása](remoteapp-changetenant.md) az Ön előfizetéséhez rendelve.

### <a name="hybrid-or-cloud--azure-ad--ad"></a>Hibrid (vagy a felhő, + az Azure AD + AD)
Az Azure AD + a helyszíni Active Directory hibrid gyűjtemény előfeltétele. Toouse AD Connect toointegrate hello két címtár van szüksége. De engedélyeznie kell bizonyos választott AD Connect konfigurálása toohow ismét. 

Nincsenek 2 AD Connect forgatókönyvek - jelszó-szinkronizálás használatával vagy az AD összevonási. Tekintse meg a hello [AD Connect információk](../active-directory/active-directory-aadconnect.md) toofigure végre ezek közül melyik felel meg leginkább az.

Is használhatja az Azure AD + AD felhőalapú gyűjtemény. Győződjön meg arról, hogy kövesse hello azonos beállítása lépéseket.

Tekintse meg [az Azure AD + Active Directory követelményeit, az Azure RemoteApp](remoteapp-ad.md) hello lépéseket szükséges tooconfigure az Azure AD és az Active Directory.

## <a name="go-create-your-collection"></a>Nyissa meg a gyűjtemény létrehozása!
Az OK gombra úgy vélem, azt is szerepelnek azt, így csak egy művelet bal oldali toodo – az első Azure RemoteApp-gyűjtemény létrehozása.

[Felhőalapú gyűjtemény létrehozása](remoteapp-create-cloud-deployment.md) vagy [hibrid gyűjtemény létrehozása](remoteapp-create-hybrid-deployment.md) -get létrehozni.

