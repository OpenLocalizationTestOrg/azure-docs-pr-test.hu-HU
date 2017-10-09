---
title: Data Lake Store a Vnetek aaaConnect tooAzure |} Microsoft Docs
description: "Csatlakozás tooAzure Data Lake Store az Azure Vnetekhez"
services: data-lake-store,data-catalog
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 683fcfdc-cf93-46c3-b2d2-5cb79f5e9ea5
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: c695dcf49fe4e1a87a90729cf085a938f3b51fe3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="access-azure-data-lake-store-from-vms-within-an-azure-vnet"></a>Az Azure Data Lake Store egy Azure virtuális hálózaton belüli virtuális gépek
Azure Data Lake Store PaaS szolgáltatás, amely a nyilvános Internet IP-címek. Általában csatlakoztatható toohello nyilvános Internet azonban bármely kiszolgáló csatlakozzon tooAzure Data Lake Store végpontok is. Alapértelmezés szerint az Azure Vnetekhez lévő összes virtuális gép hello Internet hozzáférhet, és ezért férhetnek hozzá az Azure Data Lake Store. Azonban lehetséges tooconfigure egy VNET toonot virtuális gépe access toohello Internet rendelkezik. Ilyen virtuális gépek hozzáférési tooAzure Data Lake Store korlátozódik is. Nyilvános Internet-hozzáférés letiltása a virtuális gépek az Azure Vnetekhez végezhető bármely módszer a következő hello használatával.

* Úgy konfigurálja a hálózati biztonsági csoportok (NSG)
* Úgy konfigurálja a felhasználó definiált útvonalakat (UDR)
* Útvonalak BGP (industry standard dinamikus útválasztási protokoll) útján kicserélésével ExpressRoute használatakor, amelyek blokkolják az toohello Internet eléréséhez

Ebből a cikkből megtudhatja, hogyan tooenable elérni az Azure virtuális gépeken, amelyek hello három módszer egyikével erőforrások fent felsorolt korlátozott tooaccess toohello Azure Data Lake Store.

## <a name="enabling-connectivity-tooazure-data-lake-store-from-vms-with-restricted-connectivity"></a>Korlátozott kapcsolattal rendelkező kapcsolat tooAzure Data Lake Store virtuális gépek engedélyezése
ilyen virtuális gépek tárolására az Azure Data Lake tooaccess, konfigurálnia kell azokat tooaccess hello IP-cím ahol hello Azure Data Lake Store-fiók érhető el. Hello IP-címek a Data Lake Store-fiókok alapján azonosíthatja a fiókok hello DNS-nevek feloldása (`<account>.azuredatalakestore.net`). Ehhez használhatja például a **nslookup**. Nyisson meg egy parancssort a számítógépen, és futtassa a következő parancs hello.

    nslookup mydatastore.azuredatalakestore.net

hello kimeneti hello következőhöz hasonló. hello képest érték **cím** tulajdonság a Data Lake Store-fiók társított hello IP-címet.

    Non-authoritative answer:
    Name:    1434ceb1-3a4b-4bc0-9c69-a0823fd69bba-mydatastore.projectcabostore.net
    Address:  104.44.88.112
    Aliases:  mydatastore.azuredatalakestore.net


### <a name="enabling-connectivity-from-vms-restricted-by-using-nsg"></a>Virtuális gépek NSG szolgáltatással kapcsolat engedélyezése
Egy NSG-szabály használata esetén tooblock toohello Internet, érhető el, majd létrehozhat egy másik NSG-t, amely lehetővé teszi a hozzáférést toohello Data Lake Store IP-cím. További információt az NSG-szabályok [Mi az a hálózati biztonsági csoport?](../virtual-network/virtual-networks-nsg.md). Hogyan toocreate NSG-ket: kapcsolatos utasításokat [hogyan toomanage NSG-k használatával hello Azure-portálon](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).

### <a name="enabling-connectivity-from-vms-restricted-by-using-udr-or-expressroute"></a>Virtuális gépek szolgáltatással UDR vagy ExpressRoute kapcsolat engedélyezése
Ha útvonalakat, udr-EK vagy a BGP-kicserélt útvonalak használt tooblock hozzáférés toohello Internet, egy különös útvonalat kell beállítani, hogy az ilyen alhálózatban lévő virtuális gépek hozzáférhetnek a Data Lake Store végpontok toobe. További információkért lásd: [Mik azok a felhasználó által megadott útvonalak?](../virtual-network/virtual-networks-udr-overview.md). Udr-EK létrehozásával kapcsolatos utasításokért lásd: [létrehozása udr a Resource Manager-EK](../virtual-network/virtual-network-create-udr-arm-ps.md).

### <a name="enabling-connectivity-from-vms-restricted-by-using-expressroute"></a>Virtuális gépek szolgáltatással ExpressRoute kapcsolat engedélyezése
Ha ExpressRoute-kapcsolatcsoportot van konfigurálva, hello a helyszíni kiszolgálók elérhető Data Lake Store nyilvános társviszony keresztül. További részleteket a nyilvános társviszony található ExpressRoute konfigurálása [ExpressRoute – gyakori kérdések](../expressroute/expressroute-faqs.md).

## <a name="see-also"></a>Lásd még:
* [Az Azure Data Lake Store áttekintése](data-lake-store-overview.md)
* [Azure Data Lake Store-ban tárolt adatok védelme](data-lake-store-security-overview.md)

