---
title: "A kezelt alkalmazások Azure áttekintése |} Microsoft Docs"
description: "A fogalmakat ismerteti az Azure által felügyelt alkalmazások"
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/09/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: 7ace8e1ea8038e0748bfed00c0cc0a4fa340588b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-managed-applications-overview"></a>Az Azure kezelt alkalmazások – áttekintés

Azure használó szállítók is megoldásokat nyújtsanak a világ különböző ügyfeleknek. Az Azure piactér egy gyűjteménye, amely a belső és külső gyártóktól származó összetett, multiresource sablonok több száz áll. Az ügyfelek percnél, telepítheti és indíthatja platform (PaaS) vagy szoftver, mint egy szolgáltatott szoftverként (SaaS) alkalmazások. 

Bár a piactér az ügyfelek számára, hogy gyorsan üzembe helyezhet egy ajánlat nagyszerű lehetőséget nyújt, az ügyfél felelős karbantartásáért, valamint a megoldás frissítése. A virtuális gép lemezképének számlázási túl szállítók nem díjat számítanak ügyfelek számára az alkalmazás használatát. Ezenkívül szállítók nem hogy felhasználók módosítsák a kritikus alkalmazás-erőforrásokat. Szállítókkal is nem blokkolják a hozzáférést egy alkalmazást alkotó szellemi tulajdonhoz. Azure kezelt alkalmazások megoldásokat biztosítanak a problémák. 

Kezelt alkalmazás hasonlít a piactéren, és egy fő különbség a megoldássablon. A kezelt alkalmazás az erőforrások törlődnek a szállító által kezelt az erőforráscsoporthoz. Az erőforráscsoport megtalálható-e az ügyfél előfizetését, de a gyártó által készített bérlői identitás hozzáfér az erőforráscsoportot.

## <a name="advantages-of-managed-applications"></a>Felügyelt alkalmazások előnyei

Felügyelt szolgáltató (MSPs) ISV-k, és a vállalati központi informatikai csapatoknak kezelt alkalmazások használatával megoldásokat a piactér vagy a szolgáltatáskatalógus keresztül. Bár az ügyfelek ezeket az előfizetéseket a kezelt alkalmazások telepítéséhez, karbantartásához, frissítéséhez vagy szolgáltatás őket nincs. Szállítókkal kezelését, és az alkalmazások támogatását, mert az ügyfelek ezeket az alkalmazásokat kezeléséhez az alkalmazás-specifikus tartomány Tudásbázis fejlesztéséhez nem rendelkezik. Az ügyfelek automatikusan szerezheti be az alkalmazás frissítései nem kell foglalkoznia a hibaelhárítás és az alkalmazásokkal kapcsolatos problémák diagnosztizálásához.

A szállítók és szolgáltatók kezelt alkalmazások infrastruktúra és a szoftver a piactéren keresztül eladásra csatornát létrehozni. Felügyelt alkalmazások is lehetőséget nyújtanak a szolgáltatások és működési támogatás csatlakoztatása az Azure-ügyfél. A szállítók is kiszámlázni ügyfelek az Azure számlázási rendszer használatával. Ezek sablonok segítségével központilag telepített alkalmazások életciklusának kezelését. Ezek a megoldások olyan önálló és lezárt az ügyfélnek, a szállítók is magas színvonalú szolgáltatásokat nyújthatnak. Ez a megközelítés PaaS és a Szolgáltatottszoftver-szállítók előnyökkel jár. Emellett segít vállalati központi platform csoportok és rendszerintegrátorok (SIs) csomag, és a megoldások értékesítik szeretnék.

## <a name="managed-application-types"></a>Kezelt alkalmazás típusa
Kezelt alkalmazások az Azure két típus térjen: szolgáltatáskatalógus és a piactéren.
 
### <a name="service-catalog"></a>Service Catalog  

A szolgáltatáskatalógus az ügyfelek egy katalógust a szervezet munkatársai által használt Azure jóváhagyott megoldások hozhat létre. Ilyen karbantartása megoldások katalógus akkor hasznos, központi informatikai csapata hozza létre a a vállalatok számára. Ezek segítségével a katalógus közben megoldásokat biztosítanak a szervezetek számára, győződjön meg arról, hogy bizonyos szervezeti szabványoknak való megfelelés. Ezek szabályozhatja, frissítése és ezek az alkalmazások karbantartása. Az alkalmazottak a katalógus segítségével könnyen felfedezheti az alkalmazásokat, amelyek az informatikai részleg által jóváhagyott és ajánlott széles skáláját. Az ügyfelek hozza létre őket a szolgáltatáskatalógus felügyelt alkalmazásokat láthatják. Is láthatják a kezelt alkalmazások, amelyek a szervezet más munkatársainak velük.
 
További információ a szolgáltatáskatalógus kezelt alkalmazás közzététele: [létrehozása és a szolgáltatáskatalógus kezelt alkalmazás közzététele](managed-application-publishing.md).
 
További információ a szolgáltatási katalógus által felügyelt alkalmazások felhasználása: [felhasználását a szolgáltatási katalógus által felügyelt alkalmazások](managed-application-consumption.md).
 
### <a name="marketplace"></a>Piactér

Kezelt alkalmazások az Azure-portálon a piactéren keresztül érhetők el. Miután a szállító közzéteszi ezeket az alkalmazásokat, azok elérhetők a cégen belül vagy kívül egy szervezet felhasználásához. Ezzel a megközelítéssel MSPs ISV-k és SIs kínálhat megoldásuk összes Azure-ügyfél. Az ügyfelek élvezheti a megismeréséhez, és a megoldások karbantartása fektetnek ezeket nem kell összetett megoldások segítségével. 

Jelenleg közzétevők elérhetővé teheti az ajánlatok egy kezelt alkalmazást, vagy a megoldássablon, amely nem rendelkezik felügyelt. A kezelt alkalmazás közzététele a fő összetevők közé tartoznak, a sablon és a felhasználói felület definition fájlt. A sablonfájl kiépített erőforrásokat ismerteti. A felhasználói felület csomagdefiníciós fájl ismerteti, hogyan jelennek meg a portál az ezekhez az erőforrásokhoz történő üzembe helyezéséhez szükséges adatokat. A szükséges fájlok csomagolva egy .zip fájlt, és a feltöltése a közzétételi portálon keresztül.
 
További információ a piactéren kezelt alkalmazás közzététele: [Azure felügyelt alkalmazások a piactéren](managed-application-author-marketplace.md).

További információ a piactérről kezelt alkalmazás felhasználása: [felhasználásához Azure felügyelt alkalmazások a piactéren](managed-application-consume-marketplace.md).

## <a name="key-concepts"></a>Fő fogalmak

### <a name="managed-resource-group"></a>Felügyelt erőforráscsoport
A felügyelt erőforráscsoport, ahol az Azure erőforrások a sablonban kiépített jönnek létre. Például ha a készülék használatával hozzon létre egy tárfiókot, az erőforráscsoport a tárolási fiók erőforrás. A készülék erőforrás nem tartalmaz.

### <a name="appliance-package"></a>Készülék csomag
A közzétevő a sablonfájlokat importálni és a createUIDefinition fájlt tartalmazó csomagot hoz létre. Pontosabban a következő fájlokat tartalmazza:

- **applianceMainTemplate.json**: Ez a sablon fájl határozza meg a készülék által kiépített összes erőforrást. Ezt a fájlt egy olyan rendszeres sablon fájl erőforrások létrehozására szolgál.

- **MainTemplate.json**: Ez a sablon fájl határozza meg a készülék erőforrás (Microsoft.Solutions/appliances). Egy kulcstulajdonsággal ehhez az erőforráshoz a ManagedResourceGroupId. Ez a tulajdonság azt jelzi, hogy melyik erőforráscsoport a tényleges erőforrások applianceMainTemplate.json definiált futtatására szolgál.

- **applianceCreateUIDefinition.json**: Ez a fájl ismerteti, hogyan megjelenítése a felhasználói felület a sablonban definiált paraméterek szükséges.

### <a name="authorization"></a>Engedélyezés
A közzétevő a szállító által az ügyfél nevében az erőforrások kezeléséhez szükséges engedélyeket kell adnia. Ezt az engedélyt a felügyelt erőforrások csoportra vonatkozik. Állítsa be a következő értékeket:

- **PrincipalID**: a felhasználó, csoport vagy alkalmazás, amelynek hozzáférési jogot a felügyelt erőforráscsoportot az Azure Active Directory (Azure AD) azonosítója. Ez az azonosító a közzétevő bérlőhöz tartozik.

- **Roledefinitionid-értékkel**: az előző egyszerű azonosító rendelt szerepkör az Azure AD azonosítója A beépített szerepköralapú hozzáférés-vezérlés szerepköreinek a közzétevő bérlői bármelyike lehet. További információkért lásd: [átruházásához hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md).

## <a name="next-steps"></a>Következő lépések

* Felügyelt alkalmazások közzétételéhez a piactéren kapcsolatos információkért lásd: [Azure felügyelt alkalmazások a piactéren](managed-application-author-marketplace.md).
* További információ a piactérről kezelt alkalmazás felhasználása: [felhasználásához Azure felügyelt alkalmazások a piactéren](managed-application-consume-marketplace.md).
* További információ a szolgáltatáskatalógus kezelt alkalmazás közzététele: [létrehozása és a szolgáltatáskatalógus kezelt alkalmazás közzététele](managed-application-publishing.md).
* További információ a szolgáltatási katalógus által felügyelt alkalmazások felhasználása: [felhasználását a szolgáltatási katalógus által felügyelt alkalmazások](managed-application-consumption.md).
* A felhasználói felület csomagdefiníciós fájl létrehozásához lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).
