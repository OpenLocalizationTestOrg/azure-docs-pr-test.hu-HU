---
title: "Egy Azure-tárolót beállításjegyzék földrajzi replikálása"
description: "Ismerkedés a létrehozása és kezelése az Azure-tárolót georeplikált nyilvántartó."
services: container-registry
author: stevelas
manager: timlt
ms.service: container-registry
ms.topic: overview-article
ms.date: 10/24/2017
ms.author: stevelas
ms.openlocfilehash: 92df5a37d62dc9731842a4312339aa571072a487
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/18/2017
---
# <a name="geo-replication-in-azure-container-registry"></a>Azure-tárolót beállításjegyzék georeplikáció

Egy helyi vagy a működés közbeni biztonsági másolatot, kívánó vállalatok válassza ki a több Azure-régiók szolgáltatások futtatásához. Ajánlott eljárásként helyezi el a tároló beállításjegyzék minden egyes régió, ahol képek futnak engedélyezi a hálózati bezárású műveleteket, gyors és megbízható kép réteg átvitel engedélyezése.

A georeplikáció lehetővé teszi, hogy az Azure-tárolót beállításjegyzék egy egyetlen beállításjegyzék működhet több régióba szolgál a több főkiszolgálós regionális internetregiszterek.

> [!IMPORTANT]
> A georeplikáció Azure tároló beállításjegyzék használata a jelenleg a **előzetes**. Az előzetes verziójú funkciók elérhetővé válnak, a feltétellel, hogy elfogadja a [kiegészítő használati feltételek](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Ez a funkció az egyes funkcióit általános elérhetőségével (GA) előtt módosíthatja.
>

Georeplikált beállításjegyzék a következő előnyöket nyújtja:

* Egyetlen beállításjegyzék/képcímke nevek használható különféle régiókban
* Beállításjegyzék hálózati zárja be a hozzáférést a regionális központi telepítések
* Nincsenek további kilépő díjak képként kikerülnek egy helyi, a replikált beállításjegyzék ugyanabban a régióban, mint a tároló-gazdagépen
* Egyetlen felügyeleti különféle régiókban nyilvántartó

## <a name="example-use-case"></a>Példa használati eset
Contoso futtat egy nyilvános jelenléte webhelyen keresztül az Amerikai Egyesült Államok, Kanada és Európa el. A piacon kiszolgálására helyi és a hálózat-záró tartalommal, Contoso fut [Azure Tárolószolgáltatás](/azure/container-service/kubernetes/) az USA nyugati régiója, USA keleti régiója, Kanada központi és Nyugat-Európában clusters (ACS) Kubernetes. A webhely alkalmazást, Docker képként telepített összes régiók közötti kód és a lemezkép használja. Tartalom helyi a régióba veszi át egy adatbázis, amely egyedileg minden régióban lett kiépítve. Minden egyes regionális központi telepítés rendelkezik az erőforrásokhoz, mint a helyi adatbázis egyedi konfigurációjában.

A fejlesztői csapat Seattle WA, az USA nyugati régiója adatközpont okhoz található.

![Több nyilvántartó küldését](media/container-registry-geo-replication/before-geo-replicate.png)<br />*Több nyilvántartó küldését*

A georeplikáció funkciókat használ, mielőtt Contoso kellett egy Egyesült államokbeli beállításjegyzéket az USA nyugati régiója, egy további beállításjegyzék Nyugat-Európa. Ezek különböző régiókban kiszolgálására, a fejlesztői csapat kellett képek leküldése két különböző nyilvántartó.

```bash
docker push contoso.azurecr.io/pubic/products/web:1.2
docker push contosowesteu.azurecr.io/pubic/products/web:1.2
```
![Jogosultak-e a több nyilvántartó](media/container-registry-geo-replication/before-geo-replicate-pull.png)<br />*Jogosultak-e a több nyilvántartó*

Az több nyilvántartó tipikus kihívások közé tartozik:

* USA keleti régiója, USA nyugati régiója és Kanada központi fürtök összes lekéréses nélül kilépő díjak, az egyes alábbi távoli tároló állomások képek lekéréses az adatközpontok USA nyugati régiója, USA nyugati régiója beállításjegyzékből való.
* A fejlesztői csapat kell lemezképeket leküldése USA nyugati régiója és Nyugat-Európában nyilvántartó.
* A fejlesztői csapat kell konfigurálása és karbantartása egyes regionális központi lemezkép nevű a helyi beállításjegyzékben hivatkozik.
* Beállításjegyzék elérésének mindegyik régióhoz kell konfigurálni.

## <a name="benefits-of-geo-replication"></a>A georeplikáció előnyei

![Húzza a georeplikált beállításjegyzékből.](media/container-registry-geo-replication/after-geo-replicate-pull.png)

Az Azure-tároló beállításjegyzék georeplikáció szolgáltatásával, a következő előnyökkel is vannak tudjuk:

* Minden egyes egyetlen beállításjegyzék kezelését:`contoso.azurecr.io`
* Minden egyes használt a kép URL-CÍMÉRE felügyelete az lemezképek központi telepítésére egyetlen konfigurálása:`contoso.azurecr.io/public/products/web:1.2`
* ACR kezeli a georeplikáció, beleértve a helyi értesítések regionális webhookok egy egyetlen beállításjegyzék nyomni

## <a name="configure-geo-replication"></a>Aktív georeplikáció konfigurálása
A georeplikáció konfigurálása lehető legkönnyebben kattintva régiók a térképen.

A georeplikáció csak a [prémium nyilvántartó](container-registry-skus.md) csak. Ha a beállításjegyzék nem prémium, módosíthatja a Basic és Standard a prémium szintű még a [Azure-portálon](https://portal.azure.com):

![Az Azure portálon átváltását termékváltozatok](media/container-registry-skus/update-registry-sku.png)

A georeplikáció a prémium szintű beállításjegyzék konfigurálása, jelentkezzen be az Azure portálon, a http://portal.azure.com.

Az Azure-tároló beállításjegyzék váltson, és jelölje ki **replikációk**:

![Az Azure portál tároló beállításjegyzék felhasználói felület replikációk](media/container-registry-geo-replication/registry-services.png)

Olyan térképet jelenik meg, melyen az összes jelenlegi Azure-régiókat:

 ![Az Azure portálon régió térkép](media/container-registry-geo-replication/registry-geo-map.png)

* Kék hatszögek képviselő aktuális replikák
* Zöld hatszögek képviselő lehetséges replika régiók
* Szürke hatszögek képviselő még nem érhető el a replikáció Azure-régiók

Konfigurálhatja egy replikát, jelöljön ki egy zöld hatszög, majd **létrehozása**:

 ![Replikációs felhasználói felület létrehozása az Azure portálon](media/container-registry-geo-replication/create-replication.png)

További replikák konfigurálásához a zöld hatszögek más területen, majd kattintson **létrehozása**.

ACR kezdődik, képek között a konfigurált replikák szinkronizálása. Művelet befejeződése után a portál által adott jelentéseket tükrözik *készen*. A másodpéldány állapotát a portál nem frissül automatikusan. A frissített állapotának megtekintéséhez használja a frissítés gombra.

## <a name="geo-replication-pricing"></a>A georeplikáció díjszabása

A georeplikáció csak a a [Premium Termékváltozat](container-registry-skus.md) Azure tároló beállításjegyzék. A replikált beállításjegyzékbeli a kívánt területek, Ön tudomásával prémium beállításjegyzék díjak mindegyik régióhoz.

Az előző példában a Contoso konszolidált le egy, két nyilvántartó replikák hozzáadása USA keleti régiója, Kanada központi és Nyugat-Európa. Contoso négy alkalommal prémium további konfigurációs és felügyeleti nélkül havonta fizetendő. Minden egyes régió most kéri le. a képek helyileg, teljesítmény, a megbízhatóság nélkül hálózati kilépő díjak Kanada USA nyugati régiója és USA keleti régiója.

## <a name="summary"></a>Összegzés

A georeplikáció kezelheti a regionális adatközpontok egy globális felhők. Különböző sok Azure használnak, előnyeit úgy használhatja ki egy egyetlen felügyeleti vezérlősík gyors hálózati bezárású fenntartva, és megbízható kép: helyi kéri le.

## <a name="next-steps"></a>További lépések

Tekintse meg a három részből oktatóanyag adatsorozat [Azure tároló beállításjegyzék georeplikáció](container-registry-tutorial-prepare-registry.md). Georeplikált beállításjegyzékbeli létrehozását, a tároló létrehozása, és majd telepítését egyetlen bízná `docker push` több regionális Web Apps tárolók példányok parancsot.

> [!div class="nextstepaction"]
> [Azure-tárolót beállításjegyzék georeplikáció](container-registry-tutorial-prepare-registry.md)