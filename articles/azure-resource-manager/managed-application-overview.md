---
title: "a kezelt alkalmazások az Azure aaaOverview |} Microsoft Docs"
description: "Hello fogalmakat ismerteti az Azure által felügyelt alkalmazások"
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/09/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: 2fd1844a442515f4492c890c9798073475a66f88
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-managed-applications-overview"></a>Az Azure kezelt alkalmazások – áttekintés

Azure használó szállítók kínálhat megoldások toocustomers hello world körül. Az Azure piactér egy gyűjteménye, amely a belső és külső gyártóktól származó összetett, multiresource sablonok több száz áll. Az ügyfelek percnél, telepítheti és indíthatja platform (PaaS) vagy szoftver, mint egy szolgáltatott szoftverként (SaaS) alkalmazások. 

Bár hello piactér nagyszerű lehetőséget biztosít az ügyfelek tooquickly egy ajánlat, a rendszer hello ügyfél felelős karbantartásáért, valamint a hello megoldás frissítése. Hello virtuális gép lemezképének számlázási túl szállítók nem díjat számítanak ügyfelek hello használata az alkalmazások számára. Ezenkívül szállítók nem hogy felhasználók módosítsák a kritikus alkalmazás-erőforrásokat. A szállítók nem blokkolható hozzáférés toointellectual tulajdonságot, amely az alkalmazás. Azure kezelt alkalmazások megoldásokat biztosítanak a problémák. 

Egy felügyelt alkalmazást egy fő különbség a piactér, hello hasonló tooa megoldássablonban. A kezelt alkalmazás a kiépített tooa erőforráscsoport hello szállító által kezelt áll hello erőforrás. hello erőforráscsoport hello ügyfél előfizetésben, de hello gyártója által biztosított bérlői identitás hozzáférés toohello erőforráscsoportot.

## <a name="advantages-of-managed-applications"></a>Felügyelt alkalmazások előnyei

Felügyelt szolgáltató (MSPs) ISV-k, és a vállalati központi informatikai csapatoknak használhatja a kezelt alkalmazások toodeliver megoldások hello piactér vagy szolgáltatáskatalógus hello. Bár az ügyfelek központi telepítése az előfizetések felügyelt alkalmazást, nem rendelkeznek toomaintain, frissíteni vagy szolgáltatás őket. Szállítók kezelését, és hello alkalmazások támogatását, mert az ügyfelek nem rendelkezik toodevelop alkalmazásspecifikus tartomány Tudásbázis toomanage ezeket az alkalmazásokat. Az ügyfelek is automatikusan megszerezze alkalmazás hello kell tooworry hibaelhárítási és hello alkalmazásokkal kapcsolatos problémák diagnosztizálásával kapcsolatos nélkül.

A szállítók és szolgáltatók kezelt alkalmazások hozzon létre egy csatorna toosell infrastruktúra és a szoftver hello piactéren keresztül. Felügyelt alkalmazások is adjon meg egy módon tooattach szolgáltatások és működési támogatással rendelkező ügyfelek válaszidejével tooAzure. A szállítók is kiszámlázni ügyfelek hello Azure számlázási rendszer használatával. Sablonok toomanage hello életciklusát központilag telepített alkalmazások is használják. Ezek a megoldások önálló és lezárt toohello-ügyfelekre, a szállítók is magas színvonalú szolgáltatásokat nyújthatnak. Ez a megközelítés PaaS és a Szolgáltatottszoftver-szállítók előnyökkel jár. Emellett segít vállalati központi platform csoportok és rendszerintegrátorok (SIs) ki szeretné, hogy toopackage, és a megoldások értékesítik.

## <a name="managed-application-types"></a>Kezelt alkalmazás típusa
Kezelt alkalmazások az Azure két típus térjen: szolgáltatáskatalógus és a piactéren.
 
### <a name="service-catalog"></a>Service Catalog  

A szolgáltatáskatalógus hello az ügyfelek egy katalógust a szervezet munkatársai által használt Azure toobe jóváhagyott megoldások hozhatnak létre. Ilyen karbantartása megoldások katalógus akkor hasznos, központi informatikai csapata hozza létre a a vállalatok számára. Hello katalógus tooensure szabványoknak való megfelelés bizonyos szervezeti használhatnak, amíg megoldásokat biztosítanak a szervezetek számára. Ezek szabályozhatja, frissítése és ezek az alkalmazások karbantartása. Az alkalmazottak használhatnak hello katalógus tooeasily hello gazdag készletét, amely ajánlott, és az informatikai részleg által jóváhagyott alkalmazások felderítésére. Az ügyfelek hozza létre őket hello szolgáltatáskatalógus felügyelt alkalmazásokat láthatják. Is láthatják hello felügyelt alkalmazások a szervezet megosztása őket, hogy mások.
 
További információ a szolgáltatáskatalógus kezelt alkalmazás közzététele: [létrehozása és a szolgáltatáskatalógus kezelt alkalmazás közzététele](managed-application-publishing.md).
 
További információ a szolgáltatási katalógus által felügyelt alkalmazások felhasználása: [felhasználását a szolgáltatási katalógus által felügyelt alkalmazások](managed-application-consumption.md).
 
### <a name="marketplace"></a>Piactér

Felügyelt alkalmazások hello piactér a hello Azure-portálon keresztül érhetők el. Miután hello szállító közzéteszi ezeket az alkalmazásokat, azok elérhetők a cégen belül vagy kívül egy szervezet tooconsume. Ezzel a megközelítéssel MSPs ISV-k és SIs kínálhat a megoldások tooall Azure-ügyfél. Az ügyfelek beolvasása hello előnye, hogy ezek nélkül hello kell tooinvest megismeréséhez és hello megoldások fenntartása összetett megoldás használata. 

Jelenleg közzétevők elérhetővé teheti az ajánlatok egy kezelt alkalmazást, vagy a megoldássablon, amely nem rendelkezik felügyelt. hello fő összetevőit kezelt alkalmazás közzététele hello sablonfájl és hello felhasználói felület csomagdefiníciós fájl tartalmazza. hello sablonfájl hello erőforrások kiépített ismerteti. hello felhasználói felület csomagdefiníciós fájl ismerteti, hogyan hello szükséges bemeneti ezekhez az erőforrásokhoz történő üzembe helyezéséhez hello portálon jelennek meg. hello szükséges fájlok csomagolva egy .zip fájlt, és a feltöltése hello közzétételi portálon keresztül.
 
További információ a felügyelt alkalmazási toohello piactér közzététele: [Azure felügyelt alkalmazások a piactér hello](managed-application-author-marketplace.md).

További információ a piactér hello a kezelt alkalmazás felhasználása: [felhasználásához Azure felügyelt alkalmazások a piactér hello](managed-application-consume-marketplace.md).

## <a name="key-concepts"></a>Fő fogalmak

### <a name="managed-resource-group"></a>Felügyelt erőforráscsoport
hello felügyelt erőforráscsoport egy adott összes hello Azure hello sablonban kiépített erőforrások jönnek létre. Például ha hello készülék használt toocreate egy tárfiókot, ez az erőforráscsoport hello tárolási fiók erőforrás tartalmazza. Hello készülék erőforrás nem tartalmaz.

### <a name="appliance-package"></a>Készülék csomag
hello publisher hello sablonfájlokat és hello createUIDefinition fájlt tartalmazó csomagot hoz létre. Pontosabban a következő fájlok hello tartalmazza:

- **applianceMainTemplate.json**: A sablonfájl hello készülék által kiépített hello erőforrások meghatározása. Ezt a fájlt egy olyan rendszeres sablon fájl toocreate erőforrások használatban van.

- **MainTemplate.json**: Ez a sablonfájl hello készülék erőforrás (Microsoft.Solutions/appliances) határozza meg. Egy kulcstulajdonsággal ehhez az erőforráshoz a ManagedResourceGroupId. Ez a tulajdonság azt jelzi, melyik erőforráscsoport használt toohost hello tényleges erőforrások applianceMainTemplate.json definiált.

- **applianceCreateUIDefinition.json**: Ez a fájl ismerteti, hogyan hello hello sablon hello paraméterek szükséges felhasználói felület megjelenítése.

### <a name="authorization"></a>Engedélyezés
hello publisher hello ügyfél nevében hello szállító toomanage hello erőforrások szükséges hello engedélyek kell megadnia. Ez az engedély toohello felügyelt erőforráscsoport vonatkozik. Állítsa be a következő értékek hello:

- **PrincipalID**: hello hello felhasználó, csoport vagy toogrant hozzáférés toohello felügyelt erőforráscsoport által használt alkalmazás Azure Active Directory (Azure AD) azonosítója. Ez az azonosító toohello közzétevő-bérlő tartozik.

- **Roledefinitionid-értékkel**: hello hello szerepkörrel toohello megelőző egyszerű azonosító az Azure AD-azonosítója Beépített szerepköralapú hozzáférés-vezérlés szerepköreinek hello hello publisher bérlői bármelyike lehet. További információkért lásd: [átruházásához hozzáférés-vezérlés beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md).

## <a name="next-steps"></a>Következő lépések

* Közzétételi kezelt alkalmazások toohello piactér kapcsolatos információkért lásd: [Azure felügyelt alkalmazások a piactér hello](managed-application-author-marketplace.md).
* További információ a piactér hello a kezelt alkalmazás felhasználása: [felhasználásához Azure felügyelt alkalmazások a piactér hello](managed-application-consume-marketplace.md).
* További információ a szolgáltatáskatalógus kezelt alkalmazás közzététele: [létrehozása és a szolgáltatáskatalógus kezelt alkalmazás közzététele](managed-application-publishing.md).
* További információ a szolgáltatási katalógus által felügyelt alkalmazások felhasználása: [felhasználását a szolgáltatási katalógus által felügyelt alkalmazások](managed-application-consumption.md).
* a felhasználói felület csomagdefiníciós fájl toocreate lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).
