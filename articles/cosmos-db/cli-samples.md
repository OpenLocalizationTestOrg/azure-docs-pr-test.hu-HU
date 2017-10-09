---
title: "aaaAzure Azure Cosmos DB CLI-példák |} Microsoft Docs"
description: "Az Azure CLI minták – Azure Cosmos DB fiókok, adatbázisok, tárolók, régiók és tűzfalak létrehozása és kezelése."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: database
ms.date: 06/07/2017
ms.author: mimig
ms.openlocfilehash: d6eefc3274e0b66eec4e69166bb7d4ddd58a522e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>Az Azure Cosmos DB Azure CLI-minták

hello alábbi táblázat tartalmaz hivatkozásokat toosample Azure CLI parancsfájlok Azure Cosmos DB. Az összes Azure Cosmos DB parancssori felület parancsait hivatkozás lapok érhetők el hello [Azure CLI 2.0 hivatkozás](https://docs.microsoft.com/cli/azure/cosmosdb).

| |  |
|---|---|
|**Azure Cosmos DB fiók, az adatbázis és a tárolók létrehozása**||
|[A DocumentDB API-t, a Graph vagy a tábla API-fiók létrehozása](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Egyetlen Azure Cosmos DB API-fiók, adatbázis és tárolót használja a DocumentDB, a diagram vagy a tábla API-k hello hoz létre. |
| [MongoDB API-fiók létrehozása](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Létrehoz egy egyetlen Azure Cosmos DB MongoDB API fiók, adatbázis és gyűjtemény. |
|**Az Azure Cosmos DB méretezése**||
| [Skála tároló átviteli sebesség](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Módosítások hello tárolóba througput kiépítve.|
|[Azure Cosmos DB adatbázisfiók több régióba replikálja, és feladatátvételi prioritások beállítása](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Egy adott feladatátvevő prioritású több területekre globálisan replikálja a fiók adatait.|
|**Az Azure Cosmos DB biztonságos**||
| [Fiók kulcsok beszerzése](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Lekérdezi a hello fő írási elsődleges és másodlagos kulcsok és hello fiók az elsődleges és másodlagos csak olvasható kulcsok.|
| [MongoDB kapcsolati karakterlánc beolvasása](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Hello kapcsolati karakterlánc tooconnect lekérdezi a MongoDB-alkalmazás tooyour Azure Cosmos DB fiók.|
|[Fiók kulcsok újragenerálása](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Hello fő vagy a csak olvasható hello fiók kulcsának újragenerálása.|
|[Hozzon létre egy tűzfal](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Létrehoz egy bejövő IP hozzáférés vezérlő házirend toolimit hozzáférés toohello fiókot gépek és/vagy felhőszolgáltatások egy jóváhagyott készletből.|
|**Magas rendelkezésre állású, katasztrófa utáni helyreállítás, biztonsági mentése és visszaállítása**||
|[Feladatátvételi házirend konfigurálása](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Készlet minden egyes régió, ahol a rendszer replikálja a hello fiók feladatátvételi prioritási hello.|
|**Csatlakozás Azure Cosmos DB erőforrások**||
|[Csatlakozás egy webes alkalmazás tooAzure Cosmos DB](https://docs.microsoft.com/azure/app-service-web/scripts/app-service-cli-app-service-documentdb?toc=%2fcli%2fazure%2ftoc.json)|Hozzon létre, és csatlakoztassa egy Azure Cosmos DB adatbázis és az Azure-webalkalmazás.|
|||
