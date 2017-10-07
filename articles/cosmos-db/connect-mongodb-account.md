---
title: "egy Azure Cosmos DB fiókhoz aaaMongoDB kapcsolati karakterláncot |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconnect a MongoDB-alkalmazás tooan Azure Cosmos DB fiók MongoDB kapcsolati karakterlánc használatával."
keywords: "mongodb-kapcsolati karakterlánc"
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: e36f7375-9329-403b-afd1-4ab49894f75e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: anhoh
ms.openlocfilehash: c0b81cb49a10e09e3f02411b91731c7f980ec47d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-mongodb-application-tooazure-cosmos-db"></a>Csatlakozás a MongoDB-alkalmazás tooAzure Cosmos DB
Ismerje meg, hogyan tooconnect a MongoDB-alkalmazás tooan Azure Cosmos DB fiók MongoDB kapcsolati karakterlánc használatával. Ezután használhatja az Azure Cosmos DB adatbázis hello adatként a MongoDB alkalmazás tárolására. 

Ez az oktatóanyag két lehetőséget biztosít tooretrieve kapcsolati karakterlánc adatokat:

- [hello gyors üzembe helyezési módszer](#QuickstartConnection), .NET, Node.js, MongoDB rendszerhéj, Java és Python illesztőprogramok való használatra
- [Egyéni kapcsolati karakterlánc metódus hello](#GetCustomConnection), más illesztőprogramok való használatra

## <a name="prerequisites"></a>Előfeltételek

- Egy Azure-fiók. Ha az Azure-fiók nem rendelkezik, hozzon létre egy [ingyenes Azure-fiókot](https://azure.microsoft.com/free/) most. 
- Azure Cosmos DB-fiók. Útmutatásért lásd: [a .NET MongoDB API webalkalmazás létrehozása és hello Azure-portálon](create-mongodb-dotnet.md).

## <a id="QuickstartConnection"></a>Gyors üzembe helyezési hello segítségével könnyebben nyerhet hello MongoDB kapcsolati karakterlánc
1. Egy webböngészőben, jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. A hello **Azure Cosmos DB** panelen, jelölje be hello API-t a MongoDB-fiók. 
3. Hello bal oldali ablaktábláján hello fiók panelén kattintson **gyors üzembe helyezési**. 
4. Válassza ki a platformot (**.NET**, **Node.js**, **MongoDB rendszerhéj**, **Java**, **Python**). Ha nem látja az illesztőprogram vagy eszköz szerepel, ne aggódjon--azt folyamatosan a dokumentum több kapcsolat kódrészleteket. Adjon fűzni alatt mi szeretné toosee. toolearn hogyan toocraft a saját hálózati kapcsolatot, olvassa el [hello fiókhoz tartozó kapcsolati karakterlánc adatainak megszerzése](#GetCustomConnection).
5. Másolással illessze be a hello kódrészletet a MongoDB-alkalmazásba.

    ![Gyors indítás panel](./media/connect-mongodb-account/QuickStartBlade.png)

## <a id="GetCustomConnection"></a>Hello MongoDB kapcsolati karakterlánc toocustomize beolvasása
1. Egy webböngészőben, jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. A hello **Azure Cosmos DB** panelen, jelölje be hello API-t a MongoDB-fiók. 
3. Hello bal oldali ablaktábláján hello fiók panelén kattintson **kapcsolati karakterlánc**. 
4. Hello **kapcsolati karakterlánc** panel nyílik meg. Minden hello információ szükséges tooconnect toohello fiók rendelkezik illesztőprogrammal mongodb, beleértve a preconstructed kapcsolati karakterláncot.

    ![Kapcsolati karakterlánc panel](./media/connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a>Kapcsolati karakterlánc követelmények
> [!Important]
> Azure Cosmos DB szigorú biztonsági követelmények és szabványok rendelkezik. Azure Cosmos DB fiókok szükséges hitelesítés és a biztonságos kommunikáció keresztül *SSL*. 
>
>

Azure Cosmos DB hello szabványos MongoDB kapcsolati karakterlánc URI-formátum, az adott igények szerint néhány támogatja: Azure Cosmos DB fiókok hitelesítési és a biztonságos kommunikációhoz SSL-en keresztüli van szükség. Igen van hello kapcsolati karakterlánc-formátum:

    mongodb://username:password@host:port/[database]?ssl=true

Ez a karakterlánc hello értékei a következők hello érhető el **kapcsolati karakterlánc** korábban bemutatott panel:

* Felhasználónév (kötelező): Azure Cosmos DB fióknevet.
* Jelszó (szükséges): Azure Cosmos DB fiók jelszavát.
* Állomás (kötelező): hello Azure Cosmos DB fiók teljes Tartományneve.
* Port (kötelező): 10255.
* Adatbázis (nem kötelező): hello adatbázis, amely hello kapcsolatot használ. Ha nincs adatbázis, hello alapértelmezett adatbázis-e a "teszt".
* SSL = true (kötelező)

Vegyük példaként hello látható hello fiók **kapcsolati karakterlánc** panelen. Egy érvényes kapcsolati karakterlánca:

    mongodb://contoso123:0Fc3IolnL12312asdfawejunASDF@asdfYXX2t8a97kghVcUzcDv98hawelufhawefafnoQRGwNj2nMPL1Y9qsIr9Srdw==@anhohmongo.documents.azure.com:10255/mydatabase?ssl=true

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[MongoChef használja](mongodb-mongochef.md) egy Azure Cosmos DB API-t a MongoDB-fiók.
* Hello Azure Cosmos DB API megismerkedhet a mongodb-protokolltámogatással megtekintésével [minták](mongodb-samples.md).
