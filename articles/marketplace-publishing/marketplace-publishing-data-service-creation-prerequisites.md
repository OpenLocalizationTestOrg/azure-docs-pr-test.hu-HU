---
title: "a piactér hello adatok szolgáltatás létrehozásához szükséges előfeltételek aaaTechnical |} Microsoft Docs"
description: "Egy adatszolgáltatás toodeploy létrehozásához hello követelményeinek megismeréséhez, és a hello Azure piactér értékesítés"
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: aaff609a-1cd1-4146-98f4-d04166b0fce0
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 2bba4282473fed63c3fcab43043a97e179705844
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="technical-pre-requisites-for-creating-a-data-service-offer-for-hello-azure-marketplace"></a>Egy szolgáltatás létrehozásához szükséges előfeltételek műszaki ajánlat hello Azure piactéren
> [!IMPORTANT]
> **Jelenleg dolgozunk már nem bevezetési bármely új adatszolgáltatás közzétevők. Új dataservices nem get jóváhagyott listaelem.** Ha rendelkezik a Szolgáltatottszoftver-üzleti alkalmazások toopublish szeretné a AppSource tekinthet meg további információkat [Itt](https://appsource.microsoft.com/partners). Ha egy infrastruktúra-szolgáltatási alkalmazások vagy fejlesztői szolgáltatást, akkor például az Azure piactér toopublish tekinthet meg további információkat [Itt](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

Hello folyamat megkezdése előtt alaposan és megértettem, hogy hol és miért minden egyes lépést. Amennyire csak lehetséges, meg kell készítse elő a vállalati adatok és egyéb adatokat, töltse le a szükséges eszközök, és/vagy technikai összetevő létrehozása hello ajánlat létrehozási folyamat megkezdése előtt.

A következő elemek készen áll a hello folyamat megkezdése előtt hello kell rendelkezniük:

## <a name="make-a-decision-on-what-technology-will-be-used-toopublish-your-data-service-offer"></a>A döntést a milyen technológiai használt toopublish lesz a szolgáltatás ajánlatát
Eldöntheti, hogy a közzétevő több technológiák adatszolgáltatás Azure piactéren közzétételekor között. hello fő támogatott technológiákra van az alábbiakban. Attól függetlenül történik milyen technológiai használt toopublish hello adatszolgáltatás, hello végfelhasználói keresztül hello hello adatforrástól **OData-adatcsatorna** Azure piactér szolgáltatás által elérhetővé tett tárolókra. OData szolgáltatás található információ teljes [http://www.odata.org/](http://www.odata.org/)

## <a name="sql-azure-database"></a>Az SQL Azure-adatbázis
Adatkészlet készen áll az SQL Azure-ban, akkor a Publisher felelős. Toosubscribe tooAzure kell kiépíteni az adatbázis megfelelő méretét, majd töltse fel az adatok SQL Azure Database-be. A Publisher képes mindig naprakész adatokat felelős tookeep is van. További információ az előfizetés tooAzure szolgáltatások található [https://azure.microsoft.com/services/sql-database/](https://azure.microsoft.com/services/sql-database/)

Hello adatokat az SQL Azure áthelyezésekor hello Azure piactér táblák és nézetek is elérhetővé teheti. hello Publisher adhat meg, melyik táblák/nézetek és oszlopokra mind a kitett toohello végfelhasználói. További hello tartalomszolgáltató is megadhatja, mely oszlopok szerint hello végfelhasználói kérdezhetők le, és melyeket csak hello tartalmaz adott vissza. Ez biztosítja, hogy a magas fokú rugalmasságot biztosít arról, hogy mely hello adatbázisban elérhetővé tehető. Oszlopok esetében lehet lekérdezni egy vagy több adatbázis indexek által támogatott toobe kell.

## <a name="rest-based-web-service"></a>REST-alapú webszolgáltatás
Támogatott protokollok: **csak HTTPS**

Meglévő REST-alapú szolgáltatások elérhetővé tehető hello Azure piactéren keresztül. Mivel hello adatkészlet mindig kitett toohello végfelhasználói, egy OData-adatcsatorna, hello Azure piactér szolgáltatás toobe képes toomap kell hello szolgáltatás tooa alapú OData-szolgáltatás. így hello REST-alapú végpontok toodo összes paramétert kell tooexpose HTTP paraméterekként.

hello hasznos toobe oly módon, hogy úgy rendelhető hozzá egy ATOM választ kell. Ezért hello szolgáltatások hello válaszát toobe XML formátumban kell, és csak egy ismétlődő elem, amely hello hasznos értékeket (például a rekordhalmaz) tartalmaz. hello Azure piactér szolgáltatás toohello bejegyzés csomópontok az ATOM és hello hasznos értékeinek ismétlődő tulajdonság csomópontok hello bejegyzés csomóponton belül történő hello felelteti meg.

Engedélyezési adatokat (például API fő, hitelesítési jogkivonat, stb.) kell toobe, feltéve HTTP paraméterként vagy hello HTTP-fejlécben (kulcs-érték pár) – az egyszerű hitelesítés is támogatott. Érvényes kulcs megadott toobe kell, és az összes Azure piactéren keresztül alatt álló kérelmek kulcs használatával. Figyelés és számlázási felhasználói hello Azure piactér rétegben történik.

Hello szolgáltatás által visszaadott hibák kell leképezni a HTTP-állapotkódok toobe. Abban az esetben hello szolgáltatást egy hello hibát tartalmazó XML-kódot eredményezzen ezek fog toobe hello Azure piactér szolgáltatás tooHTTP állapotkódok képezi le.

## <a name="soap-based-web-services"></a>Alapú SOAP-webszolgáltatások
Protokoll: **csak HTTPS**

hello követelményei, mint hello REST-alapú szolgáltatás szakasz hello azonos. hello egyetlen különbség, hogy paraméter is megadható feladási toohello Publisher szolgáltatás az Azure piactéren keresztül minden kérelmet XML törzsében. Ez azt jelenti, hogy HTTP paraméterek hello felhasználói biztosítja előtér-hello XML-elemekbe az XML-dokumentum hello kérelem toohello tartalomszolgáltató webszolgáltatással feladott van folyamatban lefordítva.

## <a name="odata-based-web-services"></a>Az OData-alapú webes szolgáltatások
Protokoll: **csak HTTPS**

Egy OData szolgáltatás tooAzure piactér adatokat is elérhető. hello rendszer folyamatos toopass hello szolgáltatást, és hello legfelső szintű hello szolgáltatás lecseréli hello Azure piactér gyökerén – tooensure minden ezt követő hívások Azure piactéren keresztül halad.

OData-szolgáltatásaival csak nincs szükség a hello háttérbeli adatbázis toogo. OData bármilyen tároló- és üzleti logika toodrive hello szolgáltatást támogatja.

## <a name="next-steps"></a>Következő lépések
Most, hogy áttekintette hello szükséges előfeltételek és hello szükséges teendők elvégzése, áthelyezheti előre a létrehozása, a részletes hello adatszolgáltatás ajánlatát hello [adatok szolgáltatás közzétételi útmutató](marketplace-publishing-data-service-creation.md).

Vagy, ha szeretné a teljes folyamat tooreview hello és hello megfelelő cikkek az egyes fázisok közzétételi hello, látogasson el a hello cikk [első lépések: hogyan toopublish egy ajánlat toohello Azure piactér](marketplace-publishing-getting-started.md).

[link-acct]:marketplace-publishing-accounts-creation-registration.md
