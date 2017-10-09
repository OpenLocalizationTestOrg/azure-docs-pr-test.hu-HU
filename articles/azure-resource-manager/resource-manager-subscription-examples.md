---
title: "aaaScenarios és példák előfizetés irányításhoz |} Microsoft Docs"
description: "Példákat mutat be, hogy hogyan tooimplement Azure-előfizetés irányítás szabhatják."
services: azure-resource-manager
documentationcenter: na
author: rdendtler
manager: timlt
editor: tysonn
ms.assetid: e8fbeeb8-d7a1-48af-804f-6fe1a6024bcb
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2017
ms.author: rodend;karlku;tomfitz
ms.openlocfilehash: f750e834519c8e64f57f87e2067801feb38b5c29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="examples-of-implementing-azure-enterprise-scaffold"></a>Az Azure enterprise scaffold implementációi
Ez a témakör példák hogyan vállalati hello javaslatok is létrehozható egy [Azure enterprise scaffold](resource-manager-subscription-governance.md). Contoso tooillustrate gyakorlati tanácsok szabhatják nevű fiktív vállalat használ.

## <a name="background"></a>Háttér
Contoso a ellátási lánc megoldást nyújt a "Szoftver szolgáltatásként" modell tooa csomagolt modell ügyfelek világszerte vállalat a helyszíni telepítve.  Ők fejlesztik ki szoftver hello földgömb rendelkező jelentős fejlesztési adatközpontok között India, hello az Amerikai Egyesült Államok és Kanada között.

hello ISV része hello vállalati több független részlegek termékek kezelése jelentős üzleti van osztva. Minden részleg rendelkezik, saját fejlesztők számára, a termék kezelők és a fejlesztők.

hello vállalati informatikai szolgáltatások (ETS) részleg központi informatikai képességet biztosít, és ahol üzleti egységek tárolására az alkalmazások több adatközpontok kezeli. Hello adatközpontok kezelése, valamint a hello ETS szervezet biztosítja, és a központosított együttműködés (például az e-mailek és webhelyek) és a hálózati vagy a telefonos szolgáltatások kezeli. Akkor is kisebb részlegek műveleti személyzet nem rendelkező ügyfélkapcsolati munkaterhelések kezelése.

Ez a témakör a következő személyeknek hello használt:

* Dave hello ETS Azure rendszergazda.
* Alice Contoso igazgató a fejlesztési hello ellátási lánc üzleti egység.

Contoso kell toobuild egy sor üzleti alkalmazás és egy ügyfélkapcsolati alkalmazást. Határozott toorun hello alkalmazások az Azure-on. Dave beolvassa hello [előíró előfizetés irányítás](resource-manager-subscription-governance.md) témakör, most már készen áll a tooimplement hello javaslatokat, és.

## <a name="scenario-1-line-of-business-application"></a>1. forgatókönyv: üzleti alkalmazás
A Contoso egy forrás kód felügyeleti rendszer (Bitbucketből) toobe hello world keresztül a fejlesztők által használt fejleszt.  hello alkalmazás infrastruktúra használ szolgáltatásként (IaaS) futtató, és a webkiszolgálók és adatbázis-kiszolgáló áll. A fejlesztők fejlesztői környezetükben lévő kiszolgálók eléréséhez, de nem kell hozzáféréskor toohello kiszolgálók az Azure-ban. Contoso ETS tooallow hello alkalmazás tulajdonos és a team toomanage hello alkalmazás kívánja. hello alkalmazás csak akkor közben a Contoso vállalati hálózaton. Dave tooset hello előfizetés mentése az alkalmazás igényeinek. hello előfizetés más fejlesztéssel kapcsolatos szoftver a jövőbeli hello is üzemelteti.  

### <a name="naming-standards--resource-groups"></a>Elnevezési szabályai & erőforráscsoportok
Dave toosupport fejlesztői eszközök, amelyek minden hello üzleti egységek közösen használnak egy előfizetést hoz létre. Ezután igényeihez toocreate kifejező nevet hello előfizetésre és -erőforráscsoportok (hello alkalmazás és hello hálózatok). A következő előfizetés és az erőforrás-csoportok hello hoz:

| Elem | Név | Leírás |
| --- | --- | --- |
| Előfizetés |Contoso ETS DeveloperTools éles |Támogatja a közös fejlesztői eszközök |
| Erőforráscsoport |rgBitBucket |Hello alkalmazás web server és adatbázis-kiszolgálót tartalmaz |
| Erőforráscsoport |rgCoreNetworks |Hello virtuális hálózatok és a pont-pont gateway-kapcsolatot tartalmazza |

### <a name="role-based-access-control"></a>Szerepköralapú hozzáférés-vezérlés
Miután létrehozta az előfizetést, Dave azt szeretné, hogy a megfelelő csapat hello tooensure és alkalmazástulajdonosok férhet hozzá az erőforrásokhoz. Dave felismeri, hogy egyes csapatok különböző követelményekkel rendelkezik. Ezután a Contoso a helyszíni Active Directory (AD) tooAzure Active Directory szinkronizált hello csoportok használja, és hello megfelelő szintű hozzáférés toohello csoportok.

Dave rendeli hozzá a következő szerepkörök hello előfizetés hello:

| Szerepkör | Hozzárendelt túl| Leírás |
| --- | --- | --- |
| [Tulajdonos](../active-directory/role-based-access-built-in-roles.md#owner) |Contoso-azonosító a felügyelt AD |Ez az azonosító csak a Contoso-Identity Management eszközzel idő szerinti (JIT) hozzáférési vezérli, és biztosítja, hogy teljesen naplózza az előfizetés tulajdonosa hozzáférést. |
| [Biztonsági kezelője](../active-directory/role-based-access-built-in-roles.md#security-manager) |Adatvédelmi és kockázatkezelési felügyeleti részleg |Ehhez a szerepkörhöz felhasználók toolook hello Azure Security Center: és hello erőforrások hello állapotának teszi lehetővé. |
| [Hálózati közreműködő](../active-directory/role-based-access-built-in-roles.md#network-contributor) |Hálózati adapterek |Ez a szerepkör lehetővé teszi, hogy a Contoso-hálózati team toomanage hello hely tooSite VPN és virtuális hálózatok hello. |
| *Egyéni szerepkör* |Alkalmazás tulajdonosa |Dave szerepet hoz létre, amely engedélyezi a hello képességét toomodify erőforrások hello erőforráscsoporton belül. További információkért lásd: [egyéni szerepkörök az Azure RBAC](../active-directory/role-based-access-control-custom-roles.md) |

### <a name="policies"></a>Házirendek
Dave hello előfizetési erőforrások kezeléséhez szükséges a következő hello rendelkezik:

* Hello Fejlesztőeszközök a fejlesztők közötti hello world támogatásához, mert azt nem szeretné tooblock felhasználók erőforrásokat hozzon létre bármely régióban. Azonban szükség tooknow, ahol erőforrások jönnek létre.
* Érintett költséggel. Ezért azt szeretné, hogy tooprevent alkalmazástulajdonosok feleslegesen költséges virtuális gépeket hozzon létre.  
* Ez az alkalmazás számos üzleti egységek fejlesztők is kiszolgál, mert azt szeretné, hogy tootag az egyes erőforrások hello üzleti egység és a kérelem tulajdonossal rendelkező. Ezek a címkék használatával ETS hello megfelelő csapat is kiszámlázni.

Hello következő hoz [erőforrás-kezelő házirendek](resource-manager-policy.md):

| Mező | Következmény | Leírás |
| --- | --- | --- |
| location |Naplózási |Hello erőforrások bármely régióban hello létrehozás naplózása |
| type |Megtagadása |G sorozatú virtuális gépek létrehozását megtagadása |
| tags |Megtagadása |Igényelnek az alkalmazás tulajdonosa címke |
| tags |Megtagadása |Költség center címke megkövetelése |
| tags |hozzáfűzése |A címke neve hozzáfűzése **részleghez** értéke pedig **ETS** tooall erőforrások |

### <a name="resource-tags"></a>Az erőforráscímkék
Dave tisztában van azzal, hogy ő kell toohave hello számlázási tooidentify hello költségközpont egyedi információkat hello BitBucket végrehajtásához. Emellett a Dave összes hello ETS birtokló erőforrások tooknow szeretne.

Hello következő hozzáad [címkék](resource-group-using-tags.md) toohello erőforráscsoportok és erőforrásokat.

| A címke neve | Címke |
| --- | --- |
| ApplicationOwner |Ezt az alkalmazást kezelő hello személy hello neve. |
| CostCenter |a költségközpont hello hello csoport, amely az Azure-használatát hello fizeti. |
| Részleghez |**ETS** (hello társított részleg hello előfizetés) |

### <a name="core-network"></a>Központi hálózat
hello adatvédelmi és kockázatkezelési ügyfélfelügyeleti csapata ellenőrzi, hogy Dave tartozó adatokat javasolt Contoso ETS toomove hello alkalmazás tooAzure megtervezése. Szeretnék tooensure, amely hello alkalmazás nincs felfedve toohello internet.  Dave is rendelkezik, amely a jövőbeni hello lesz áthelyezett tooAzure fejlesztői alkalmazások. Ezekhez az alkalmazásokhoz igényelnek közös felületek.  toomeet ezeknek a követelményeknek, szolgáltat belső és külső virtuális hálózatok és a hálózati biztonsági csoport toorestrict hozzáférést.

A következő erőforrások hello hoz:

| Erőforrástípus | Név | Leírás |
| --- | --- | --- |
| Virtual Network |vnInternal |Hello BitBucket alkalmazást használni, és ExpressRoute tooContoso vállalati hálózaton keresztül kapcsolódik.  Egy alhálózat (sbBitBucket) biztosít egy adott IP-címtér hello alkalmazást. |
| Virtual Network |vnExternal |Jövőbeli nyilvánosan elérhető végpontok igénylő alkalmazások esetén érhető el. |
| Hálózati biztonsági csoport |nsgBitBucket |Biztosítja, hogy hello csak a 443-as port hello alhálózati hello alkalmazás él, ahol (sbBitBucket) kapcsolatok így minimalizálható a munkaterhelés támadási felületét. |

### <a name="resource-locks"></a>Erőforrás zárolások feloldása
Dave felismeri, hogy hello kapcsolatot a Contoso vállalati hálózat toohello belső virtuális hálózatról védeni kell a wayward parancsfájlok vagy véletlen törlés.

Hello következő hoz [erőforrás zárolási](resource-group-lock-resources.md):

| Zárolás típusa | Erőforrás | Leírás |
| --- | --- | --- |
| **CanNotDelete** |vnInternal |A felhasználó nem törlése hello virtuális hálózat és alhálózat, de nem akadályozza meg az új alhálózatok hello hozzáadását. |

### <a name="azure-automation"></a>Azure Automation
Dave rendelkezik semmi tooautomate ehhez az alkalmazáshoz. Bár a létrehozott egy Azure Automation-fiók, akkor nem kezdetben használja ezt a.

### <a name="azure-security-center"></a>Azure Security Center
A Contoso informatikai szolgáltatások kezelésében kell tooquickly azonosítására és fenyegetések kezelésére. Akkor is érdemes toounderstand milyen problémákat létezhet.  

toofulfill ezeket a követelményeket, Dave lehetővé teszi, hogy hello [az Azure Security Center](../security-center/security-center-intro.md), és biztosítja a hozzáférés toohello biztonságkezelő szerepkör.

## <a name="scenario-2-customer-facing-app"></a>2. forgatókönyv: ügyfélkapcsolati alkalmazás
hello üzleti vezetőségi hello ellátási lánc üzleti egység hűség kártyával észlelt különböző lehetőségek tooincrease engagement Contoso-ügyfelekkel. Ágnes team létre kell hoznia ezt az alkalmazást, és úgy dönt, hogy Azure növeli a képes toomeet hello az üzleti igényeknek. Alice az ETS tooconfigure két előfizetések fejleszt, és az alkalmazás működtetéséhez Dave működik.

### <a name="azure-subscriptions"></a>Azure-előfizetések
Dave jelentkezik be toohello Azure vállalati portálon, és láthatja, hogy hello ellátási lánc osztály már létezik.  Azonban mivel ez a projekt hello első alkalmazásfejlesztési projekt hello ellátási lánc csoport az Azure-ban, a Dave felismeri hello szükségességét Ágnes fejlesztői csapat új fiókot.  Ezután hello "R & D" fiókot a csapata számára hoz létre, és hozzáférést tooAlice rendeli hozzá. Alice hello Azure-portálon keresztül jelentkezik be, és létrehoz két előfizetések: egy toohold hello fejlesztési és egy toohold hello éles kiszolgálók.  Korábban létrehozott hello elnevezési szabályai ő követ, amikor a következő előfizetések hello létrehozása:

| Előfizetés használata | Név |
| --- | --- |
| Fejlesztés |SupplyChain ResearchDevelopment LoyaltyCard fejlesztési |
| Éles |SupplyChain műveletek LoyaltyCard éles |

### <a name="policies"></a>Házirendek
Dave és Alice hello alkalmazás tárgyalja, és azonosíthatja, hogy az alkalmazás csak szolgál hello Észak-amerikai régió ügyfelek.  Alice és csapata megtervezése toouse Azure alkalmazás Service-környezet és az Azure SQL toocreate hello alkalmazás. A fejlesztés során szükségük toocreate virtuális gépek.  Ágnes azt szeretné, hogy a fejlesztők hello erőforrásait tartalmazó tooexplore kell, és vizsgálja meg a problémákat anélkül, hogy a ETS húzza tooensure.

A hello **fejlesztési előfizetés**, hoznak létre a következő házirend hello:

| Mező | Következmény | Leírás |
| --- | --- | --- |
| location |Naplózási |Naplózási hello létrehozása hello erőforrások bármely régióban. |

Ezek nem korlátozzák a felhasználó hozhat létre a fejlesztési sku hello típusú, és nem igényelnek címkék erőforráscsoport-sablonok és erőforrások.

A hello **éles előfizetés**, hoznak létre a következő házirendek hello:

| Mező | Következmény | Leírás |
| --- | --- | --- |
| location |Megtagadása |Megtagadási hello USA adatközpontok kívül semmilyen erőforráshoz hello létrehozását. |
| tags |Megtagadása |Igényelnek az alkalmazás tulajdonosa címke |
| tags |Megtagadása |Részleg címke igényelnek. |
| tags |hozzáfűzése |Hozzáfűzendő címke tooeach erőforráscsoport, amely jelzi, éles környezetben. |

Ezek egy felhasználó létrehozhatja éles környezetben sku hello típusú nem korlátozzák.

### <a name="resource-tags"></a>Az erőforráscímkék
Dave tisztában van azzal, hogy ő kell toohave konkrét információk tooidentify hello megfelelő üzleti csoportok a számlázási és tulajdonjogát. Az erőforráscsoport-sablonok és erőforrások erőforráscímkék kiadásként definiálja.

| A címke neve | Címke |
| --- | --- |
| ApplicationOwner |Ezt az alkalmazást kezelő hello személy hello neve. |
| Szervezeti egység |a költségközpont hello hello csoport, amely az Azure-használatát hello fizeti. |
| EnvironmentType |**Éles** (annak ellenére, hogy előfizetése hello **éles** hello nevét, ennek a címkének lehetővé teszi a könnyebbé teszi a beazonosítást erőforrások hello portálon vagy hello számlázási megtekintve.) |

### <a name="core-networks"></a>Alapvető hálózat
hello adatvédelmi és kockázatkezelési ügyfélfelügyeleti csapata ellenőrzi, hogy Dave tartozó adatokat javasolt Contoso ETS toomove hello alkalmazás tooAzure megtervezése. Szeretnék, hogy hűség Card alkalmazást hello tooensure megfelelően megtalálása és védett, egy Szegélyhálózaton hálózati.  toofulfill ezt a követelményt, Dave és Alice hozzon létre egy külső virtuális hálózatot és egy hálózati biztonsági csoport tooisolate hello hűség Card alkalmazást hello Contoso vállalati hálózatról.  

A hello **fejlesztési előfizetés**, hoznak létre:

| Erőforrástípus | Név | Leírás |
| --- | --- | --- |
| Virtual Network |vnInternal |Hello Contoso hűség kártya fejlesztőkörnyezet szolgál, és ExpressRoute tooContoso vállalati hálózaton keresztül kapcsolódik. |

A hello **éles előfizetés**, hoznak létre:

| Erőforrástípus | Név | Leírás |
| --- | --- | --- |
| Virtual Network |vnExternal |Hello hűség Card alkalmazást használja, és nem csatlakozik közvetlenül tooContoso tartozó ExpressRoute. A forráskód rendszeren keresztül fejlesztőre kódot közvetlenül az toohello PaaS szolgáltatások. |
| Hálózati biztonsági csoport |nsgBitBucket |Biztosítja, hogy hello csak így tesz lehetővé az adathoz kötött kommunikációs TCP 443-as minimalizálható a munkaterhelés támadási felületét.  Contoso is vizsgálja a további védelem webalkalmazási tűzfal használata. |

### <a name="resource-locks"></a>Erőforrás zárolások feloldása
Dave és Alice révén, és döntse el, a kulcsfontosságú erőforrások hello a hello környezet tooprevent véletlen törlés során egy errant kód leküldéses tooadd erőforrás zárolását.

Akkor hozzon létre hello zárolást a következő:

| Zárolás típusa | Erőforrás | Leírás |
| --- | --- | --- |
| **CanNotDelete** |vnExternal |Törölje a virtuális hálózati hello vagy alhálózatok személyek tooprevent. hello zárolása nem akadályozza meg a hello hozzáadása új alhálózatokat. |

### <a name="azure-automation"></a>Azure Automation
Alice és a fejlesztői csapat kiterjedt runbookok toomanage hello környezettel az alkalmazás rendelkezik. runbookokat hello hello és törléséről a csomópontok hello alkalmazás és más DevOps feladatok teszik lehetővé.

toouse ezeknél a runbookoknál engedélyezése [Automation](../automation/automation-intro.md).

### <a name="azure-security-center"></a>Azure Security Center
A Contoso informatikai szolgáltatások kezelésében kell tooquickly azonosítására és fenyegetések kezelésére. Akkor is érdemes toounderstand milyen problémákat létezhet.  

toofulfill ezeknek a követelményeknek, Dave lehetővé teszi, hogy az Azure Security Center. Ezután biztosítja, hogy a hello Azure Security Center által figyelt hello erőforrások, és hozzáférést biztosít a toohello DevOps és biztonsági csoportok.

## <a name="next-steps"></a>Következő lépések
* toolearn Resource Manager-sablonok létrehozásával kapcsolatban lásd: [ajánlott eljárások Azure Resource Manager-sablonok létrehozásához](resource-manager-template-best-practices.md).

