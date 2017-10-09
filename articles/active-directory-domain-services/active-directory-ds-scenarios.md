---
title: "Az Azure Active Directory tartományi szolgáltatások: Központi telepítési forgatókönyvek |} Microsoft Docs"
description: "Az Azure AD tartományi szolgáltatásokhoz központi telepítési forgatókönyvek"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c5216ec9-4c4f-4b7e-830b-9d70cf176b20
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: maheshu
ms.openlocfilehash: 8a64bd8f7c6eba8f6490e10fa62bef195b6b3d5f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deployment-scenarios-and-use-cases"></a>Központi telepítési forgatókönyvek és példák
Ebben a szakaszban úgy tekintünk, néhány forgatókönyvek és használati esetet, amelyekkel igénybe vehesse az Azure Active Directory (AD) tartományi szolgáltatásokban.

## <a name="secure-easy-administration-of-azure-virtual-machines"></a>Az Azure virtuális gépek a biztonságos, könnyen felügyelete
Használhat Azure Active Directory tartományi szolgáltatások toomanage az Azure virtuális gépek zökkenőmentes módon. Az Azure virtuális gépek illesztett toohello által felügyelt tartományokhoz, így azt a vállalati AD-felhasználó hitelesítő adatait a toolog toouse lehet. Ez a megközelítés azzal elkerülhetők az credential felügyeleti kellemetlenségektől például a helyi rendszergazdai fiókkal az Azure virtuális gépek minden egyes karbantartása.

Kiszolgáló virtuális gépek, amelyek összeillesztett toohello által kezelt tartomány is kezelhetők, és a csoportházirend használatával biztonságossá. Alkalmazhatja a szükséges biztonsági alapterveket tooyour Azure virtuális gépek és a vállalati biztonsági irányelveknek megfelelően őket zárolását. Például használhatja csoport házirend felügyeleti képességek toorestrict hello típusú alkalmazásokat, amelyek indíthatja a virtuális gépeken.

![Azure virtuális gépek egyszerűbb felügyeletét](./media/active-directory-domain-services-scenarios/streamlined-vm-administration.png)

Kiszolgálók és egyéb infrastruktúrával eléri az élettartam végi, mert a Contoso mozgatása kiszolgálóvá helyszíni toohello felhőbeli számos alkalmazás. Az aktuális informatikai standard arra, hogy a vállalati alkalmazások üzemeltető kiszolgálók tartományhoz csatlakoztatott és a felügyelt csoportházirend használatával kell lennie. Contoso-rendszergazda inkább toodomain illesztési virtuális gépek az Azure könnyebben toomake felügyeleti rendszerbe. Ennek köszönhetően a rendszergazdák és felhasználók jelentkezhetnek a vállalati hitelesítő adataival. Hello: egy időben gépek konfigurált toocomply szükséges biztonsági alaptervvel csoportházirend használatával is lehet. Contoso inkább nem toohave toodeploy, figyelheti és kezelheti a tartományvezérlőket az Azure toosecure Azure virtuális gépeken. Ezért a Azure AD tartományi szolgáltatások, a használati eset kiváló alkalmasnak.

**Üzembe helyezésével kapcsolatos megjegyzések**

Vegye figyelembe a következő központi telepítési példánkban fontos pontok hello:

* Azure AD tartományi szolgáltatások által biztosított felügyelt tartományok alapértelmezés szerint egyetlen egyszerű OU (szervezeti egységhez) alapot biztosítanak. Minden tartományhoz gép egy adott strukturálatlan szervezeti egység található. Azonban dönthet toocreate egyéni szervezeti egységekhez.
* Azure AD tartományi szolgáltatások beépített csoportházirend-objektum minden egyes hello felhasználókhoz és számítógépekhez hello formájában támogatja az egyszerű csoportházirend tárolók. Egyéni csoportházirend-objektumok létrehozása és a cél őket toocustom szervezeti egységekhez.
* Azure AD tartományi szolgáltatások hello alap AD számítógép objektum séma támogatja. Hello számítógép objektum sémája nem bővíthető.

## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-bind-authentication-tooazure-infrastructure-services"></a>Növekedési-és-shift egy helyszíni LDAP-kötési hitelesítés tooAzure infrastruktúra-szolgáltatásokat használó alkalmazás
![LDAP-kötés](./media/active-directory-domain-services-scenarios/ldap-bind.png)

Contoso szereztek független sok éve helyszíni alkalmazás rendelkezik. hello alkalmazás jelenleg hello ISV által karbantartási módban van, és a módosítások toohello azokat kérő alkalmazásnak contoso elfogadhatatlanul magas. Ez az alkalmazás rendelkezik egy webes előtér, amely egy webes űrlap használata felhasználói hitelesítő adatokat gyűjt, és majd hitelesíti a felhasználókat egy LDAP-kötési toohello elvégzésével vállalati Active Directory címtárhoz. Contoso toomigrate szeretné az alkalmazás tooAzure infrastruktúra-szolgáltatásokat. Kívánatos, hogy a hello alkalmazás megfelelően van a módosítása nélkül működik-e. Emellett a felhasználók kell a meglévő vállalati hitelesítő adatok használatával képes tooauthenticate és nélkül tooretrain felhasználók toodo dolgot másképp rendelkező. Ez azt jelenti, a végfelhasználók kell lennie a hello alkalmazást futtató oblivious, hello áttelepítési kell átlátszó toothem.

**Üzembe helyezésével kapcsolatos megjegyzések**

Vegye figyelembe a következő központi telepítési példánkban fontos pontok hello:

* Győződjön meg arról, hogy hello alkalmazás nem kell toomodify/írás toohello könyvtár. Azure AD tartományi szolgáltatások által biztosított LDAP írási toomanaged tartományok nem támogatott.
* Nem módosítható közvetlenül hello felügyelt tartomány alapján jelszavakat. A végfelhasználók vagy is módosíthatják jelszavukat az Azure AD önkiszolgáló jelszó-módosítási mechanizmussal vagy hello a helyszíni címtár. Ezeket a módosításokat is automatikusan szinkronizált elérhető hello kezelt tartományban.

## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-read-tooaccess-hello-directory-tooazure-infrastructure-services"></a>Növekedési és shift LDAP használó helyszíni alkalmazások olvasási tooaccess hello directory tooAzure infrastruktúra-szolgáltatásokat
Contoso rendelkezik egy helyszíni üzletági (LOB) alkalmazás kifejlesztett majdnem egy évtizedben telt el. Ez az alkalmazás kompatibilis könyvtár, és a Windows Server AD tervezett toowork volt. hello alkalmazás LDAP (Lightweight Directory Access Protocol) tooread információk/attribútumok az Active Directory felhasználók használja. hello alkalmazás attribútumok módosítsa és egyéb írási toohello könyvtár. Contoso toomigrate szeretné az alkalmazás tooAzure infrastruktúra-szolgáltatásokat, és ezt az alkalmazást tartalmazó hello korosodási helyszíni hardver kivonása. hello alkalmazást nem lehet egy átírt toouse modern címtár API-k például hello Azure AD Graph API REST-alapú. Ezért a növekedési és shift lehetőség van szükség, amellyel hello alkalmazás áttelepített toorun hello felhőben nélkül kód módosítása, vagy írja át a hello alkalmazás lehet.

**Üzembe helyezésével kapcsolatos megjegyzések**

Vegye figyelembe a következő központi telepítési példánkban fontos pontok hello:

* Győződjön meg arról, hogy hello alkalmazás nem kell toomodify/írás toohello könyvtár. Azure AD tartományi szolgáltatások által biztosított LDAP írási toomanaged tartományok nem támogatott.
* Győződjön meg arról, hogy hello alkalmazás nem kell egyéni bővített Active Directory-sémát. Sémakiterjesztések nem támogatottak az Azure AD tartományi szolgáltatásokban.

## <a name="migrate-an-on-premises-service-or-daemon-application-tooazure-infrastructure-services"></a>Egy a helyszíni szolgáltatás vagy démon alkalmazás tooAzure infrastruktúrák áttelepítése
Egyes alkalmazások a többrétegű konfigurációk, ahol hello szintek egyikét kell tooperform hitelesített hívásokon tooa háttér réteg például adatbázis-rétegből állnak. Active Directory-fiókok gyakran használják az alábbi használati eseteket. Beállíthatja a növekedési és shift ilyen alkalmazások tooAzure infrastruktúra-szolgáltatásokat és hello identitás kell az alkalmazások az Azure AD tartományi szolgáltatások használatát. Választhat toouse hello ugyanazt a fiókot, amely szinkronizálva van a helyszíni címtár tooAzure AD. Alternatív megoldásként először hozzon létre egy egyéni szervezeti egység, és létrehozhat egy különálló szolgáltatásfiókhoz a szervezeti egységre, toodeploy ilyen alkalmazásokat.

![A szolgáltatásfiók WIA használatával](./media/active-directory-domain-services-scenarios/wia-service-account.png)

A Contoso egy egyedi tároló alkalmazás, amely tartalmaz egy előtér-webkiszolgáló, egy SQL server és a háttérkiszolgáló FTP rendelkezik. Windows-hitelesítést szolgáltatásfiókok használt tooauthenticate hello webes előtér toohello FTP-kiszolgáló. hello előtér-webkiszolgáló szolgáltatás fiókként toorun be van állítva. hello háttér-kiszolgálót hello előtér-webkiszolgáló hello szolgáltatás fiókból tooauthorize hozzáférését. Contoso inkább nem toohave toodeploy hello felhő toomove tartomány a tartományvezérlő virtuális gépet az alkalmazás tooAzure infrastruktúra-szolgáltatásokat. Contoso-rendszergazda telepíthetők hello hello előtér-webkiszolgáló, az SQL server és a hello FTP server tooAzure virtuális gépek üzemeltetéséhez. Ezek a gépek nem, akkor a csatlakoztatott tooan az Azure AD tartományi szolgáltatások által felügyelt tartomány. Majd, hogy használhassák hello ugyanazt a fiókot a helyi címtárban hello alkalmazás hitelesítési célokra. Ez a szolgáltatás fiók toohello szinkronizált Azure AD tartományi szolgáltatások által kezelt tartomány, és használható.

**Üzembe helyezésével kapcsolatos megjegyzések**

Vegye figyelembe a következő központi telepítési példánkban fontos pontok hello:

* Győződjön meg arról, hogy a hello alkalmazás a felhasználónév/jelszó a hitelesítéshez használja. -Alapú tanúsítvány/intelligens kártyás hitelesítés Azure AD tartományi szolgáltatások által nem támogatott.
* Nem módosítható közvetlenül hello felügyelt tartomány alapján jelszavakat. A végfelhasználók vagy is módosíthatják jelszavukat az Azure AD önkiszolgáló jelszó-módosítási mechanizmussal vagy hello a helyszíni címtár. Ezeket a módosításokat is automatikusan szinkronizált elérhető hello kezelt tartományban.

## <a name="windows-server-remote-desktop-services-deployments-in-azure"></a>A Windows Server távoli asztali szolgáltatások az Azure-ban központi telepítések
Azure AD tartományi szolgáltatásokkal tooprovide felügyelt AD tartományi szolgáltatások távoli asztali kiszolgálók tooyour Azure szolgáltatásba telepített.

Ez a telepítési forgatókönyv kapcsolatos további információkért lásd: hogyan túl[integrálni Azure AD tartományi szolgáltatásokat a távoli asztali szolgáltatások telepítés](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-azure-adds).
