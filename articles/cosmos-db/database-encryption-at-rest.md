---
title: "Az adatbázis-titkosítás aktívan: Azure Cosmos DB |} Microsoft Docs"
description: "Ismerje meg, hogyan nyújt az Azure Cosmos DB a alapértelmezett titkosítás az összes adatot."
services: cosmos-db
author: voellm
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 99725c52-d7ca-4bfa-888b-19b1569754d3
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: voellm
ms.openlocfilehash: d52d55fe45fdd18394166ec7ccd6e46e8f8f434b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-database-encryption-at-rest"></a>Az Azure Cosmos DB adatbázis titkosítását

Aktívan nem adattitkosítás egy kódot, amely toohello titkosítás az adatok nem felejtő tárolóeszközök, általában arra utal, például tartós állapotú meghajtók (SSD) és a merevlemezes (HDD) meghajtók. Cosmos DB az SSD-k az elsődleges adatbázis tárolja. A media mellékletek és a biztonsági mentések Azure Blob Storage tárolóban, amely általában a biztonsági HDD vannak tárolva. A Cosmos DB titkosítását hello számára készült az adatbázisok, media mellékletek és biztonsági mentések vannak titkosítva. A most adattitkosítás átvitel közben (hello hálózaton), és inaktív (nem felejtő tárolási), felkínálva végpontok közötti titkosítását.

Mivel a PaaS szolgáltatás Cosmos DB nagyon könnyen toouse. Mert Cosmos DB tárolt összes felhasználói adatokat aktívan és átviteli titkosított, nincs tootake bármilyen műveletet. Egy másik módja tooput azt, hogy a titkosítás, a többi pedig "a" alapértelmezés szerint. Nincsenek nem vezérlők tooturn azt ki- és. Ez a szolgáltatás közben továbbra is toomeet nyújtunk a [rendelkezésre állásának és teljesítményének SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db).

## <a name="implement-encryption-at-rest"></a>A titkosítás aktívan

Titkosítását számos biztonsági technológia, beleértve a biztonságos kulcs tárolása rendszerek, a titkosított hálózatokat és a kriptográfiai API-k segítségével történik. Rendszerek, adatok feldolgozása és visszafejtésére toocommunicate kulcsok kezelése rendszerrel rendelkezik. hello ábra azt mutatja, hogyan különül el a titkosított adatok és hello felügyeleti kulcsok tárolására. 

![Tervezési diagramja](./media/database-encryption-at-rest/design-diagram.png)

hello alapszintű folyamat egy felhasználói kérelem a következőképpen történik:
- hello felhasználói adatbázis fiók létesítése készen áll, és a tárolási kulcsokat a rendszer beolvassa a kérelem toohello felügyeleti szolgáltatás erőforrás-szolgáltató használatával.
- Egy felhasználó létrehoz egy kapcsolat tooCosmos DB HTTPS/secure átviteli keresztül. (hello SDK-k absztrakt hello adatokat.)
- hello felhasználó egy korábban létrehozott biztonságos kapcsolat hello keresztül tárolt JSON-dokumentum toobe elküldi.
- hello JSON-dokumentum egy indexelt, kivéve, ha hello felhasználó kikapcsolta indexelő.
- Hello JSON-dokumentumában, mind az indexelési adatok toosecure tárolására készültek.
- Rendszeres időközönként adatokat hello biztonságos tárolási olvasni és biztonsági mentése Azure titkosított Blob tároló toohello.

## <a name="frequently-asked-questions"></a>Gyakori kérdések

### <a name="q-how-much-more-does-azure-storage-cost-if-storage-service-encryption-is-enabled"></a>K: hogyan sokkal költségeket a Azure Storage a Storage szolgáltatás titkosítási engedélyezésekor?
V: nincs további költség nélkül.

### <a name="q-who-manages-hello-encryption-keys"></a>K: kezelő hello titkosítási kulcsokat?
A: a Microsoft által felügyelt hello kulcsok.

### <a name="q-how-often-are-encryption-keys-rotated"></a>K: milyen gyakran elforgatott a titkosítási kulcsokat?
V: Microsoft tartozik egy belső iránymutatásokat titkosítási kulcs elforgatás, amely Cosmos DB követi. hello adott irányelveket nem kerülnek közzétételre. A Microsoft hello közzététele [biztonságos fejlesztési Életciklussal (SDL)](https://www.microsoft.com/sdl/default.aspx), amely belső útmutatást részhalmazának tekintendő, és rendelkezik a hasznos gyakorlati tanácsok a fejlesztők számára.

### <a name="q-can-i-use-my-own-encryption-keys"></a>K: használhatok saját titkosítási kulcsokat?
V: cosmos DB egy PaaS szolgáltatás, és ezért rögzített tookeep hello szolgáltatás egyszerű toouse előre. Ez a kérdés gyakran ismételt egy megfelelőségi követelményt, például a PCI DSS kiválassza proxy kérdésként már észleltünk. Ez a szolgáltatás létrehozásának részeként azt együttműködik megfelelőségi ellenőrök tooensure, hogy az ügyfelek, akik Cosmos-Adatbázist kíván használni követelményeknek a hello kell toomanage kulcsok maguk nélkül is.
Ennek eredményeképpen jelenleg nem kínálunk felhasználók hello beállítás tooburden magukat a kulcskezelést.

### <a name="q-what-regions-have-encryption-turned-on"></a>K: Mi memóriaterületnél engedélyezve van a titkosítás?
V: minden Azure Cosmos DB memóriaterületnél titkosítás az összes felhasználói adat-e kapcsolva.

### <a name="q-does-encryption-affect-hello-performance-latency-and-throughput-slas"></a>K: titkosítási befolyásolja hello teljesítmény késést és átviteli SLA-kat?
V: nincs nem gyakorolt hatás vagy módosításokat toohello teljesítmény SLA-k most, hogy aktívan engedélyezve van az összes meglévő és új fiók. További a hello [Cosmos DB SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db) toosee hello legújabb garanciák lapon.

### <a name="q-does-hello-local-emulator-support-encryption-at-rest"></a>K: hello helyi emulátor támogatja titkosítását?
V: hello emulátor az önálló fejlesztési és tesztelési célú eszközzel, és nem használja, amely felügyelt Cosmos DB szolgáltatás által használt hello hello kulcskezelő szolgáltatások. Azt javasoljuk, ahol bizalmas emulátor Tesztadatok tároló meghajtókon a BitLocker tooenable. Hello [emulátor támogatja változó hello alapértelmezett adatkönyvtár](local-emulator.md) továbbá használatával egy ismert helyre.

## <a name="next-steps"></a>Következő lépések

A Cosmos DB biztonsági és hello legújabb fejlesztései rendelkezésre megtudhatja, [Azure Cosmos DB adatbázis biztonsági](database-security.md).
Microsoft minősítései közül kapcsolatos további információkért lásd: hello [Azure biztonsági és adatkezelési központ](https://azure.microsoft.com/en-us/support/trust-center/).
