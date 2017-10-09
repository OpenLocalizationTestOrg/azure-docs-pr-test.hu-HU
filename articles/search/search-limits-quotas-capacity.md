---
title: "az Azure Search aaaService korlátok |} Microsoft Docs"
description: "A kapacitástervezés használt szolgáltatásra vonatkozó korlátozások és kérelmeit és válaszait az Azure Search maximális korlátozásait."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 857a8606-c1bf-48f1-8758-8032bbe220ad
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 06/07/2017
ms.author: heidist
ms.openlocfilehash: cb13a0f1c87a654fb5845c9c741f74a91da5b372
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-limits-in-azure-search"></a>Az Azure Search szolgáltatásra vonatkozó korlátozások
Maximális korlátozza a tárolási, számítási feladatok és indexek, dokumentumok, mennyiségét, és más objektumok függenek, hogy Ön [kiépíteni az Azure Search](search-create-service-portal.md) , egy **szabad**, **alapvető**, vagy **Szabványos** tarifacsomagra vált.

* **Szabad** egy több-bérlős megosztott szolgáltatás, amely az Azure-előfizetéssel rendelkezik. Meglévő előfizető egy további-ingyenes beállítása, amely lehetővé teszi a dedikált erőforrások regisztrálása előtt tooexperiment hello szolgáltatásban is.
* **Alapszintű** dedikált számítási erőforrások biztosít egy kisebb léptékű termelési számítási feladatokhoz.
* **Standard** minden szinten további tárolási és feldolgozási kapacitással rendelkező dedikált gépeken futtatja. Standard elérhető lesz a négy szinten lévő: S1, S2, S3 és S3 nagy sűrűségű (S3 HD).

> [!NOTE]
> A szolgáltatás van üzembe helyezve egy konkrét csomagot kiválasztani. Ha toojump rétegek tooget nagyobb kapacitású, el kell juttatnia (nincs nincs frissítés) új szolgáltatása. További információkért lásd: [válasszon egy SKU vagy a réteg](search-sku-tier.md). További információk a szolgáltatásokon belüli kapacitás értékének toolearn korábban már kiépített, lásd: [erőforrás szintek lekérdezési és indexelési munkaterhelések méretezése](search-capacity-planning.md).
>

## <a name="per-subscription-limits"></a>Előfizetés korlátokat
[!INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## <a name="per-service-limits"></a>Egy szolgáltatásra vonatkozó korlátozások
[!INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

## <a name="per-index-limits"></a>Index korlátokat
Van egy-az-egyhez levelezés között indexek vonatkozó korlátozások és indexelők korlátozásait. 200 indexek legfeljebb megadott, hello maximális száma indexelők egyben hello a 200 ugyanazt a szolgáltatást.

| Erőforrás | Ingyenes | Alapszintű | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Index: maximális mezők / index |1000 |100 <sup>1</sup> |1000 |1000 |1000 |1000 |
| Index: maximális pontozási / index profilok |100 |100 |100 |100 |100 |100 |
| Index: maximális funkciók profilonként |8 |8 |8 |8 |8 |8 |
| Indexelők: hívásonkénti indexelési terhelés maximális |10 000 dokumentumok |Maximális dokumentumokat csak korlátozva |Maximális dokumentumokat csak korlátozva |Maximális dokumentumokat csak korlátozva |Maximális dokumentumokat csak korlátozva |N/A <sup>2</sup> |
| Indexelők: maximális futási időt | 1 – 3 percet <sup>3</sup> |24 óra |24 óra |24 óra |24 óra |N/A <sup>2</sup> |
| A BLOB indexelő: blob maximális mérete, MB |16 |16 |128 |256 |256 |N/A <sup>2</sup> |
| A BLOB indexelő: blob kinyert tartalom maximális karakterszám |32,000 |64,000 |4 millió |4 millió |4 millió |N/A <sup>2</sup> |

<sup>1</sup> alapszintű rétegben van hello csak az index / 100 mezők alsó határának Termékváltozat.

<sup>2</sup> S3 HD jelenleg nem támogatja az indexelők. Ha ezt a képességet sürgős van, forduljon az Azure támogatási szolgálatához.

<sup>3</sup> indexelő maximális végrehajtási hello ingyenes szint ideje 3 perc, a blob-forrásoknak és más adatforrások 1 perc.

## <a name="document-size-limits"></a>A dokumentum méretkorlátai
| Erőforrás | Ingyenes | Alapszintű | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Egyes dokumentum méretet a Index API |< 16 MB |< 16 MB |< 16 MB |< 16 MB |< 16 MB |< 16 MB |

Toohello maximális dokumentum mérete hivatkozik, amikor egy Index API felület meghívásakor. A dokumentum mérete ténylegesen hello Index API-kérés törzsében hello méretének korlátozására. Mivel egyszerre több dokumentumok toohello Index API egy tranzakcióköteghez átadhatók, hello méretkorlátot ténylegesen hány dokumentumok olyan hello kötegben függ. Az egyetlen dokumentum kötegben hello maximális dokumentum mérete 16 MB JSON.

tookeep dokumentum mérete, tooexclude nem lekérdezhető adatok megjegyzése hello kérelemből. Képek és egyéb bináris adatokat nem közvetlenül lekérdezhető, és nem szabad hello index. keresési eredmények toointegrate nem lekérdezhető adatokat tároló URL-hivatkozás toohello erőforráshoz nem kereshető mező határozza meg.

## <a name="workload-limits-queries-per-second"></a>Munkaterhelés-korlátok (lekérdezések / másodperc)
| Erőforrás | Ingyenes | Alapszintű | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| QPS |N/A |~3 replikánként |~15 replikánként |~60 replikánként |>60 replikánként |>60 replikánként |

Lekérdezések / másodperc (QPS) közelítő heurisztikus szimulált és tényleges ügyfelek munkaterhelések tooderive becsült értékek alapján. Pontos QPS átviteli hello lekérdezés az adatok és hello jellegű függ.

Bár durva becslést fent vannak, egy tényleges sebessége akkor nehéz toodetermine, különösen a szabad megosztott szolgáltatás, amelyben átviteli sebesség alapján rendelkezésre álló sávszélesség és a rendszer-erőforrások verseny hello. Hello ingyenes szint, a számítási és tárolási erőforrásokat megosztott több előfizető, így a megoldás QPS mindig változhatnak attól függően, hogy hány más feladatokat futtató: hello ugyanannyi időt vesz igénybe.

Hello standard szintű megbecsülheti további QPS szorosan mert befolyásolni több hello paramétert. Hello ajánlott eljárások a című [kezelése a keresési megoldás](search-manage.md) kapcsolatos útmutatás a munkaterhelések QPS toocalculate.

## <a name="api-request-limits"></a>API-kérelmekre vonatkozó korlátok
* 16 MB kérelmenként maximálisan <sup>1</sup>
* 8 KB-os URL-cím maximális hossza
* Legfeljebb 1000 dokumentumok kötegenként index feltöltését, egyesíti vagy törlése
* A $orderby záradékban maximális 32 mezők
* Maximális keresési kifejezés mérete 32766 bájt (32 KB-os mínusz 2 bájt) UTF-8 kódolású szöveg

<sup>1</sup> az Azure Search hello kérelem törzse tulajdonos tooan felső határ az egyes mezők vagy gyűjtemények, ellenkező esetben nem korlátozott elméleti korlátok hello tartalmát gyakorlati korlátozást előíró 16 MB (lásd: [támogatott adattípusok](https://msdn.microsoft.com/library/azure/dn798938.aspx) mező összetételűnek és korlátozások olvashat).

## <a name="api-response-limits"></a>API-válaszból korlátok
* A keresési eredmények laponként visszaadott legfeljebb 1000 dokumentumok
* Javaslat API kérelmenként visszaadott legfeljebb 100 javaslatok

## <a name="api-key-limits"></a>Az API-kulcs korlátok
API-kulcsokat használnak a szolgáltatás-hitelesítéshez. Két típusa van. Adminisztrációs kulcsok hello kérelem fejlécében megadott, és teljes olvasási és írási hozzáférése toohello szolgáltatást biztosítania. Lekérdezés kulcsai csak olvasható, hello URL-címet, és általában elosztott tooclient alkalmazások megadott.

* Legfeljebb 2 adminisztrációs kulcsok-szolgáltatás
* Legfeljebb 50 lekérdezési kulcsok-szolgáltatás
