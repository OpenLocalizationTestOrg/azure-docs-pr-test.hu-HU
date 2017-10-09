---
title: az Azure DNS aaaOverview |} Microsoft Docs
description: "A Microsoft Azure szolgáltatást tartalmazó DNS áttekintése. A Microsoft Azure-tartomány üzemeltetésére."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 68747a0d-b358-4b8e-b5e2-e2570745ec3f
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/19/2017
ms.author: gwallace
ms.openlocfilehash: a10f87c488356469e9c04aabde31129049563891
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-dns-overview"></a>Az Azure DNS áttekintése

Tartománynévrendszer hello, vagy a DNS, felelős fordítása (vagy feloldása) egy webhelyre vagy szolgáltatásba név tooits IP-cím. Az Azure DNS egy olyan üzemeltetési szolgáltatás DNS-tartományok, biztosítani a névfeloldást a Microsoft Azure-infrastruktúra használatával. Által az Azure-ban a tartományt üzemeltet, kezelheti a DNS-rekordok hello ugyanazon hitelesítő adatokkal, API-k, eszközök és számlázási, a más Azure-szolgáltatásokkal.

![DNS áttekintése](./media/dns-overview/scenario.png)

## <a name="features"></a>Szolgáltatások

* **Megbízhatóság és teljesítmény** -DNS-tartományok Azure DNS-ben üzemeltetett globális hálózata Azure DNS névkiszolgálóit. Nem egyedi, hogy minden DNS-lekérdezés melléket hello legközelebbi elérhető DNS-kiszolgáló hálózati is használjuk. Gyors teljesítmény és a magas rendelkezésre állást biztosít a tartományhoz.

* **Zökkenőmentes integráció** -hello Azure DNS szolgáltatást lehet használt toomanage DNS-rekordjait az Azure-szolgáltatások és használt tooprovide DNS, valamint a külső erőforrások lehetnek. Az Azure DNS integrálva van a hello Azure-portálon, és használja ugyanazokat a hitelesítő adatokat, a számlázásra és a támogatási szerződése hello mint a más Azure-szolgáltatásokkal.

* **Biztonsági** -hello Azure DNS szolgáltatást az Azure Resource Manager alapul. Ilyen előnyöket az erőforrás-kezelő szolgáltatásait, például a szerepköralapú hozzáférés-vezérlés, a vizsgálati naplók és a erőforrás zárolását. A tartományok és a rekordok hello Azure-portálon az Azure PowerShell-parancsmagok segítségével kezelhetők, és platformfüggetlen Azure CLI hello. Automatikus DNS-kezelési igénylő alkalmazások integrálhatók a hello keresztül hello szolgáltatás REST API-t és az SDK-k.

Az Azure DNS jelenleg nem támogatja tartománynevek megvásárlását. Ha azt szeretné, hogy toopurchase tartományok, egy külső regisztrációs toouse kell. hello regisztráló általában egy kis éves díj költségek. hello tartományok majd az Azure DNS-lehet üzemeltetni, a DNS-rekordok kezelése. Lásd: [delegálása a tartományi tooAzure DNS](dns-domain-delegation.md) részleteiről.

## <a name="pricing"></a>Díjszabás

DNS számlázási hello száma az Azure-ban és a DNS-lekérdezések száma hello által üzemeltetett DNS-zónák alapul. látogasson el árazással kapcsolatos további toolearn [Azure DNS árképzési](https://azure.microsoft.com/pricing/details/dns/).

## <a name="faq"></a>GYIK

További Azure DNS szolgáltatással kapcsolatos gyakran ismételt kérdések: hello [Azure DNS gyakran ismételt kérdések](dns-faq.md).

## <a name="next-steps"></a>Következő lépések

További információk a DNS-zónák és rekordok ellátogatva: [DNS-zónák és áttekintése rögzíti](dns-zones-records.md).

Ismerje meg, hogyan túl[hozzon létre egy DNS-zóna](./dns-getstarted-create-dnszone-portal.md) Azure DNS-ben.

Megismerhet néhány hello más kulcs [hálózati lehetőségeket](../networking/networking-overview.md) Azure.

