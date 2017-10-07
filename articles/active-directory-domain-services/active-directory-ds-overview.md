---
title: "az Azure Active Directory tartományi szolgáltatások aaaOverview |} Microsoft Docs"
description: "Az Azure Active Directory tartományi szolgáltatások áttekintése"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 0d47178f-773e-45f9-9ff4-9e8cffa4ffa2
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 2b4884b3b8b639fcca438ddbab0bd3361aac53c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-domain-services"></a>Azure AD Domain Services
## <a name="overview"></a>Áttekintés
Azure infrastruktúra-szolgáltatások lehetővé teszik a toodeploy számos különböző megoldásokat számítástechnikai gyors módon. Azure virtuális gépekkel, szinte azonnal telepíthető, és csak által hello perc után kell fizetnie. A támogatás a Windows, Linux, SQL Server, Oracle, IBM, SAP, és a BizTalk, alapján telepíthet bármilyen munkaterhelést, ismeretcikkekkel, szinte bármilyen operációs rendszeren. Ilyen előnyt lehetővé teszik a toomigrate örökölt alkalmazások telepítése a helyszíni tooAzure, toosave a működési költségeket.

Kulcsfontosságú az áttelepítése a helyszíni alkalmazások tooAzure kezeli a hello identitás igényeinek ezeket az alkalmazásokat. Címtár-kompatibilis alkalmazások előfordulhat, hogy az olvasási vagy írási hozzáférést toohello vállalati directory LDAP támaszkodnak vagy integrált Windows-hitelesítés (Kerberos vagy NTLM-hitelesítés) tooauthenticate végfelhasználók támaszkodnak. Működő Windows Server-üzletági (LOB) alkalmazások általában a tartományhoz csatlakoztatott gépeken vannak telepítve, ezért ezek kezelhetők biztonságosan a csoportházirend használatával. too'lift és shift "a helyszíni alkalmazások toohello felhő, ezeket a függőségeket a hello vállalati identitás-infrastruktúrát kell toobe feloldva.

A rendszergazdák gyakran kapcsolja be a következő megoldások toosatisfy hello identitás igényeinek az Azure szolgáltatásba telepített alkalmazások hello tooone:

* Telepítse az Azure infrastruktúra-szolgáltatásokat és hello vállalati címtár a helyszínen futó feladatok közötti pont-pont VPN-kapcsolat.
* Hello vállalati AD-tartomány vagy erdő infrastruktúra kiterjesztése az Azure virtuális gépek használatával replika tartományvezérlők beállításával.
* Egy önálló tartományhoz az Azure-ban az Azure virtuális gépként telepített tartományvezérlők telepítése.

Ezek a módszerek érinti a magas költség- és adminisztratív terhelés mellett. A rendszergazdák az Azure virtuális gépek használata szükséges toodeploy tartományvezérlők. Emellett azok toomanage, biztonságos, a javítás, a figyelő, a biztonsági mentés kell, és ezek a virtuális gépek hibaelhárítása. VPN-kapcsolatok toohello hello igényel a helyszíni címtár hatására az Azure toobe sebezhető tootransient hálózati hibáktól vagy üzemszünet korlátozza az üzembe helyezett munkaterhelések tekintetében. A hálózati kimaradások pedig alacsonyabb hasznos üzemidő és az alkalmazás csökkentett megbízhatóságának eredményez.

Azure AD tartományi szolgáltatások tooprovide könnyebb alternatív tervezett azt.

## <a name="introducing-azure-ad-domain-services"></a>Azure AD tartományi szolgáltatások bemutatása
Azure AD tartományi szolgáltatások által kezelt tartomány szolgáltatásokat, például a tartományhoz való csatlakozást, csoport házirend, az LDAP, Kerberos vagy NTLM-hitelesítés, amelyek a teljes mértékben kompatibilis a Windows Server Active Directory nyújt. A tartományi szolgáltatások nélkül hello meg toodeploy felhasználását, kezelése és tartományvezérlők hello felhőben javítás. Azure AD tartományi szolgáltatások integrálható a meglévő Azure AD-bérlőn, így lehetővé teszi a felhasználók toolog a vállalati hitelesítő adataival. Emellett használhatja a meglévő csoportok és felhasználói fiókok toosecure hozzáférés tooresources, biztosítva ezzel a gördülékenyebb "növekedési-és-shift" a helyszíni erőforrások tooAzure infrastruktúra-szolgáltatásokat.

Függetlenül attól, hogy az Azure AD-bérlő csak felhőalapú vagy a helyszíni Active Directoryval szinkronizált Azure AD tartományi szolgáltatások funkcióit problémamentesen működik.

### <a name="azure-ad-domain-services-for-cloud-only-organizations"></a>A szervezetek csak felhőalapú Azure AD tartományi szolgáltatások
Egy kizárólag felhőalapú Azure AD bérlői (gyakran hivatkozott tooas "felügyelt bérlők") nem rendelkezik a helyszíni identitás erőforrásigényét. Ez azt jelenti felhasználói fiókok, a jelszavakat és a csoporttagságokat felhőalkalmazástól minden natív toohello - Ez azt jelenti, hogy létre és kezelhető az Azure ad-ben. Érdemes lehet egy kis ideig, hogy a Contoso egy kizárólag felhőalapú Azure AD-bérlő. Ahogy az ábra a következő hello, Contoso-rendszergazda konfigurált virtuális hálózatot az Azure infrastruktúra-szolgáltatásokat. Alkalmazások és a kiszolgáló-munkaterhelések vannak telepítve a virtuális hálózat az Azure virtuális gépeken. Mivel a Contoso egy kizárólag felhőalapú bérlői, minden felhasználói identitások, a hitelesítő adatokat és a csoporttagságokat létrehozása és kezelése az Azure ad-ben.

![Az Azure AD tartományi szolgáltatások áttekintése](./media/active-directory-domain-services-overview/aadds-overview.png)

Contoso-rendszergazda engedélyezi az Azure AD tartományi szolgáltatásokat az Azure AD-bérlő, és válassza ki a toomake tartományi szolgáltatások a virtuális hálózaton rendelkezésre álló. Ezt követően az Azure AD tartományi szolgáltatások egy felügyelt tartomány kiépítését, és lehetővé teszi hello virtuális hálózat. Összes felhasználói fiókok, a csoporttagságot és a felhasználó hitelesítési adat lesz elérhető, a Contoso Azure AD-bérlő is rendelkezésre áll az újonnan létrehozott tartományban. Ez a funkció lehetővé teszi a felhasználók a hello szervezet toosign toohello tartomány a vállalati hitelesítő adataik használatával – például távoli asztalon keresztül távolról toodomain csatlakoztatott számítógépeken kapcsolódáskor. A rendszergazdák hozzáférés tooresources hello tartományban meglévő csoporttagságok használatával hozhat létre. A hello virtuális hálózat virtuális gépei a telepített alkalmazások funkciók, például tartományhoz való csatlakozást, LDAP olvasható, LDAP bind, NTLM és Kerberos hitelesítéshez, és a csoportházirend is használhatók.

Azure AD tartományi szolgáltatások által ellátott hello által kezelt tartomány néhány jellegzetességei a következők:

* Contoso-rendszergazda nem kell toomanage, javítás, vagy a figyelő az ebben a tartományban vagy bármely olyan tartományvezérlőn, a felügyelt tartomány.
* Nincs szükség toomanage AD replikálása ebben a tartományban van. Felhasználói fiókok, csoporttagságot és hitelesítő adatait a Contoso Azure AD-bérlő érhetők el automatikusan a kezelt tartományon belül.
* Mert hello tartomány kezeli az Azure AD tartományi szolgáltatások, a Contoso a rendszergazda nem rendelkezik tartománygazdai vagy vállalati rendszergazdai jogosultságokkal ezen a tartományon.

### <a name="azure-ad-domain-services-for-hybrid-organizations"></a>A hibrid szervezetek számára az Azure AD tartományi szolgáltatások
Egy hibrid informatikai infrastruktúrával rendelkező szervezeteknek felhőalapú erőforrásokat és a helyszíni erőforrások felhasználását. Olyan szervezetek azonosító adatokat a saját helyszíni címtár tootheir Azure AD-bérlő szinkronizálni. Hibrid szervezetek keresse meg a helyszíni alkalmazások toohello felhő, különösen a régebbi címtár-kompatibilis alkalmazások, további toomigrate Azure AD tartományi szolgáltatások hasznos toothem válhat.

Telepítve van a Litware Corporation [az Azure AD Connect](../active-directory/active-directory-aadconnect.md), toosynchronize azonosító adatokat a saját helyszíni directory tootheir az Azure AD-bérlő. hello azonosító adatok szinkronizált felhasználói fiókok, a hitelesítő kivonatokat a hitelesítés (jelszó-szinkronizálás) és a csoporttagságokat tartalmazza.

> [!NOTE]
> **A jelszó-szinkronizálás használata kötelező a hibrid szervezetek toouse Azure AD tartományi szolgáltatások**. Ez a követelmény azért, mert a felhasználói hitelesítő adatok szükségesek a felügyelt hello Azure AD tartományi szolgáltatásokat, tooauthenticate által biztosított tartományt ezek a felhasználók keresztül NTLM vagy Kerberos hitelesítési módszert.
>
>

![Az Azure AD tartományi szolgáltatások a Litware Corporation](./media/active-directory-domain-services-overview/aadds-overview-synced-tenant.png)

hello előző ábrán egy hibrid informatikai infrastruktúrát, például a Litware Corporation rendelkező szervezetek használatát Azure AD tartományi szolgáltatásokat. Egy virtuális hálózatot az Azure infrastruktúra-szolgáltatásokat litware's alkalmazások és kiszolgálói szolgáltatások, amelyek tartományi szolgáltatások vannak telepítve. Litware's rendszergazda engedélyezi az Azure AD tartományi szolgáltatásokat az Azure AD-bérlő, és válassza ki a toomake érhető el a virtuális hálózaton található felügyelt tartományhoz. Mivel Litware egy hibrid informatikai infrastruktúra rendelkező szervezeteknél, felhasználói fiókok, csoportok és hitelesítő adatokat a helyszíni címtár szinkronizált tootheir az Azure AD bérlő. Ez a funkció lehetővé teszi, hogy a felhasználók toosign toohello tartományban a vállalati hitelesítő adatok használatával – például távoli csatlakozáskor toomachines toohello tartományhoz csatlakozott távoli asztalon keresztül. A rendszergazdák hozzáférés tooresources hello tartományban meglévő csoporttagságok használatával hozhat létre. A hello virtuális hálózat virtuális gépei a telepített alkalmazások funkciók, például tartományhoz való csatlakozást, LDAP olvasható, LDAP bind, NTLM és Kerberos hitelesítéshez, és a csoportházirend is használhatók.

Azure AD tartományi szolgáltatások által ellátott hello által kezelt tartomány néhány jellegzetességei a következők:

* hello által kezelt tartomány egy különálló tartományban. Egy bővítmény Litware meg a helyi tartomány nincs.
* Litware's rendszergazda nem kell toomanage, a javítás vagy a figyelő tartományvezérlők a felügyelt tartomány.
* Nincs szükség toomanage AD replikációs toothis tartomány van. Felhasználói fiókok, csoporttagságot és hitelesítő adatait Litware a helyszíni címtár szinkronizált tooAzure AD az Azure AD Connect használatával. A felhasználói fiókok, a csoporttagságot és a hitelesítő adatok automatikusan elérhetők hello felügyelt tartományon belül.
* Mert hello tartomány kezeli az Azure AD tartományi szolgáltatások, Litware a rendszergazda nem rendelkezik tartománygazdai vagy vállalati rendszergazdai jogosultságokkal ezen a tartományon.

## <a name="benefits"></a>Előnyök
Az Azure AD tartományi szolgáltatásokkal hello a következő előnyöket élvezheti:

* **Egyszerű** – is megfelel a virtuális gépek hello identitás igényeinek telepített néhány egyszerű kattintással tooAzure infrastruktúra-szolgáltatásokat. Nem kell toodeploy és identitás-infrastruktúra az Azure vagy a telepítő kapcsolatot hátsó tooyour a helyszíni identitás-infrastruktúra kezelése.
* **Integrált** – Azure AD tartományi szolgáltatások mélyen integrált a Azure AD-bérlő. Most már használhatja az Azure AD caters toohello kell a modern alkalmazások és a hagyományos címtár-kompatibilis alkalmazások integrált, felhőalapú vállalati könyvtárként.
* **Kompatibilis** – Azure AD tartományi szolgáltatások hello bevált enterprise osztályú infrastruktúra Windows Server Active Directory épül. Ezért az alkalmazások kompatibilisek a Windows Server Active Directory-szolgáltatások nagyobb fokú támaszkodhat. Nem minden szolgáltatások a Windows Server AD jelenleg érhetők el az Azure AD tartományi szolgáltatásokban. Azonban elérhető szolgáltatások kompatibilisek-e hello megfelelő Windows Server AD-funkciót használ, a helyszíni-infrastruktúrájában. hello LDAP Kerberos, az NTLM, a csoportházirend és a tartományhoz csatlakoztatás képességek képeznek egy összetett ajánlat, amely tesztelése és Windows Server különböző kiadásai keresztül is.
* **Költséghatékony** – az Azure AD tartományi szolgáltatásokkal, elkerülheti a hello infrastruktúra és a felügyeleti terheket társított identitás infrastruktúra toosupport hagyományos címtár-kompatibilis alkalmazásokat kezeléséhez. Helyezze át a alkalmazások tooAzure infrastruktúra-szolgáltatásokat, és jelentősebb megtakarítást tesznek lehetővé a működési költségeket kihasználhassa.
