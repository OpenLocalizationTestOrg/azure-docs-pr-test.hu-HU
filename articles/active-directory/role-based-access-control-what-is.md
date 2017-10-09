---
title: "aaaManage hozzáférést és engedélyeket az RBAC - Azure RBAC |} Microsoft Docs"
description: "Ismerkedés a hozzáférés-kezelés az Azure portál hello Azure szerepköralapú hozzáférés-vezérlés. Szerepkör-hozzárendelések tooassign engedélyek használata a címtárban."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: 8f8aadeb-45c9-4d0e-af87-f1f79373e039
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: 1c133b2b57b49d85f0e12a318c7997478e095fb9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-role-based-access-control-in-hello-azure-portal"></a>Szerepköralapú hozzáférés-vezérlés hello Azure-portálon az első lépések
A vállalat biztonsági célú hello pontos engedélyeket szükségük ad az alkalmazottak kell összpontosítania. Túl sok engedélyek egy fiók tooattackers is elérhetővé teheti. Túl kevés engedélyek, az azt jelenti, hogy az alkalmazottak nem munkavégzéséhez hatékony. Azure szerepköralapú hozzáférés-vezérlés (RBAC) segít a részletes hozzáféréskezelést az Azure felajánlásával oldja meg a problémát.

Szerepalapú használ, feladatokat elkülönítse a munkacsoporton belül, és hozzáférési toousers csak hello összegét adja meg, hogy be kell tooperform a munkájukat. Jogosultságot ad a Mindenki helyett korlátozás engedélyek az Ön Azure-előfizetés vagy erőforrásokhoz, engedélyezheti a csak bizonyos műveleteket. Például használja az RBAC toolet egy alkalmazott egy előfizetésben található virtuális gépek kezeléséhez, miközben egy másik kezelhető SQL-adatbázisok hello belül azonos előfizetéssel.

## <a name="basics-of-access-management-in-azure"></a>Hozzáférés-kezelés az Azure-ban alapjai
Minden Azure-előfizetés tartozik egy Azure Active Directory (AD) címtárban. Felhasználók, csoportok és alkalmazások tartalmazza a kezelhetik az erőforrásokat a hello Azure-előfizetés. Rendelje hozzá a következő hozzáférési jogokat hello Azure-portálon, az Azure parancssori eszközök és Azure szolgáltatásfelügyeleti API használatával.

Adjon hozzáférést rendel hello megfelelő RBAC szerepkör toousers, csoportok és alkalmazások egy adott hatókörben. szerepkör-hozzárendelés hello hatóköre lehet előfizetés, egy erőforráscsoport vagy egy erőforrást. Egy szülő hatókörben szerepkörrel is hozzáférést biztosít az abban szereplő toohello gyermekei. Például tooa erőforráscsoport hozzáféréssel rendelkező felhasználó az összes hello erőforrást tartalmaz, például webhelyekhez, virtuális gépek és alhálózatok kezelheti.

![Kapcsolat közötti elemek Azure Active Directory - ábra](./media/role-based-access-control-what-is/rbac_aad.png)

hello RBAC szerepkörhöz hozzárendelt határozzák meg, hogy milyen erőforrások hello felhasználó, csoport vagy alkalmazás adott hatókörén belül kezelheti.

## <a name="built-in-roles"></a>Beépített szerepkörök
Az Azure RBAC három alapvető szerepkörökhöz tooall erőforrástípusok rendelkezik:

* **Tulajdonos** rendelkezik teljes körű hozzáférési tooall erőforrások, például az hello jobb toodelegate hozzáférés tooothers.
* **A közreműködői** is létrehozása és kezelése az Azure-erőforrások minden típusú, de nem tudja engedélyezni a hozzáférést tooothers.
* **Olvasó** megtekintheti a meglévő Azure-erőforrások.

hello rest hello RBAC-szerepkörök az Azure-ban az adott Azure-erőforrások felügyelet lehetővé tételéhez. Virtuális gép közreműködő szerepkört hello például lehetővé teszi, hogy a felhasználó toocreate hello, és virtuális gépeket. Azt nem kapnak hozzáférést toohello virtuális hálózat vagy, amely a virtuális gép hello hello alhálózathoz csatlakozik. 

[Beépített RBAC-szerepkörök](role-based-access-built-in-roles.md) listák hello szerepkörök az Azure-ban érhető el. Hello műveletek és, hogy minden beépített szerepkörök toousers hatókört határoz meg. Ha saját szerepköröket még jobban kézben toodefine van szüksége, lásd: hogyan toobuild [egyéni szerepkörök az Azure RBAC](role-based-access-control-custom-roles.md).

## <a name="resource-hierarchy-and-access-inheritance"></a>Erőforrás-hierarchiát és a hozzáférés öröklési
* Minden egyes **előfizetés** az Azure-ban tartozik tooonly egy címtárban. (De minden directory rendelkezhet egynél több előfizetésnek.)
* Minden egyes **erőforráscsoport** tooonly egy előfizetés tartozik.
* Minden egyes **erőforrás** tooonly egy erőforráscsoporthoz tartozik.

A gyermekhatókör örökölt, hogy a szülő hatókörök számára biztosítson hozzáférést. Példa:

* Hozzárendelt hello olvasó szerepkör tooan az Azure AD-csoport hello előfizetés hatókörben. a csoport tagjai hello megtekintheti minden erőforráscsoport és erőforrások hello előfizetés.
* Hello közreműködői szerepkör tooan alkalmazást hello erőforrás csoport hatóköre lehet kijelölni. Az erőforráscsoport, de nem egyéb erőforráscsoportok hello előfizetésben bármilyen típusú erőforrások kezelésére alkalmas.

## <a name="azure-rbac-vs-classic-subscription-administrators"></a>Az Azure RBAC és a hagyományos előfizetés rendszergazdái
Hagyományos előfizetés rendszergazdák és a társadminisztrátoroknak rendelkezik teljes körű hozzáférési toohello Azure-előfizetés. Hello-erőforrások kezelésére [Azure-portálon](https://portal.azure.com) az Azure Resource Manager API-k vagy hello [a klasszikus Azure portálon](https://manage.windowsazure.com) és az Azure klasszikus üzembe helyezési modellben. Hello RBAC-modellben a hagyományos adminisztrátorait szerepkörrel hello tulajdonos hello előfizetés hatókörben.

Csak hello Azure-portálon, és új Azure Resource Manager API-k támogatása Azure RBAC hello. Felhasználók és az alkalmazások rendelt RBAC-szerepkörök hello klasszikus felügyeleti portált és a hello Azure klasszikus telepítési modell nem használható.

## <a name="authorization-for-management-vs-data-operations"></a>Engedélyezési és adatok az operations Management
Az Azure RBAC csak támogatja a felügyeleti műveleteket, az Azure-erőforrások hello hello Azure-portál és az Azure Resource Manager API-k. Nem lehet engedélyezni, az összes adat szintű műveletet Azure-erőforrások. Például engedélyezheti valaki toomanage Storage-fiókok, de nem toohello blobokkal vagy a táblák egy Tárfiókon belül. Hasonlóképpen SQL-adatbázis kezelheti, de nem hello táblák belül.

## <a name="next-steps"></a>Következő lépések
* Ismerkedés a [szerepköralapú hozzáférés-vezérlés az Azure-portálon hello](role-based-access-control-configure.md).
* Lásd: hello [beépített RBAC-szerepkörök](role-based-access-built-in-roles.md)
* Saját [egyéni szerepkörök az Azure RBAC-ben](role-based-access-control-custom-roles.md)
