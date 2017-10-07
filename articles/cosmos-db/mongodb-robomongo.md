---
title: az Azure Cosmos DB Robomongo aaaUse |} Microsoft Docs
description: "Megtudhatja, hogyan toouse rendelkező egy Azure Cosmos DB Robomongo: API-t a MongoDB-fiók"
keywords: robomongo
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: anhoh
ms.openlocfilehash: 3018243e904015426dc69a69b26fb53421e1fe4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-robomongo-with-an-azure-cosmos-db-api-for-mongodb-account"></a>Egy Azure Cosmos DB Robomongo használni: API-t a MongoDB-fiók
tooconnect tooan Azure Cosmos DB: API használatával Robomongo MongoDB-fiók, meg kell:

* Töltse le és telepítse [Robomongo](https://robomongo.org/)
* Az Azure Cosmos DB rendelkezik: API-t a MongoDB-fiók [kapcsolati karakterlánc](connect-mongodb-account.md) információk

## <a name="connect-using-robomongo"></a>Csatlakozás Robomongo használatával
tooadd az Azure Cosmos DB: API-t a MongoDB fiók toohello Robomongo MongoDB kapcsolatok, hajtsa végre a lépéseket követve hello.

1. Az Azure Cosmos DB beolvasása: API-t a MongoDB kapcsolat fiókadatok hello utasításokat követve [Itt](connect-mongodb-account.md).

    ![Képernyőfelvétel a hello kapcsolati karakterlánc panel](./media/mongodb-robomongo/connectionstringblade.png)
2. Futtatás *Robomongo.exe*

3. Hello kapcsolat gombra a **fájl** toomanage a kapcsolatokat. Kattintson a **létrehozása** hello a **MongoDB kapcsolatok** ablakban nyílik meg hello **Biztonságoskapcsolat-beállításainak** ablak.

4. A hello **kapcsolatbeállítások** ablakban válasszon egy nevet. Ezután megkeresi a hello **állomás** és **Port** a kapcsolati információit az 1. lépés, és írja be őket **cím** és **Port**, illetve .

    ![Képernyőfelvétel a hello Robomongo kapcsolatok kezelése](./media/mongodb-robomongo/manageconnections.png)
5. A hello **hitelesítési** lapra, majd **végrehajtása hitelesítési**. Ezután írja be az adatbázis (alapértelmezett érték *Admin*), **felhasználónév** és **jelszó**.
Mindkét **felhasználónév** és **jelszó** a kapcsolatadatok, 1. lépésben található.

    ![Képernyőfelvétel a hello Robomongo hitelesítés lap](./media/mongodb-robomongo/authentication.png)
6. A hello **SSL** lapon jelölje **használja az SSL protokoll**, majd módosítsa a hello **hitelesítési módszer** túl**önaláírt tanúsítvány**.

    ![Képernyőfelvétel a hello Robomongo SSL lap](./media/mongodb-robomongo/SSL.png)
7. Végezetül kattintson **teszt** képes tooconnect, majd tooverify **mentése**.

## <a name="next-steps"></a>Következő lépések
* Ismerheti meg az Azure Cosmos DB: Mongodb-protokolltámogatással API [minták](mongodb-samples.md).
