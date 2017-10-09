---
title: "aaaLearn hogyan toouse hello SFTP-összekötőt a logic Apps alkalmazásait |} Microsoft Docs"
description: "Az Azure App service logic Apps alkalmazások létrehozása Csatlakozás tooSFTP API toosend fájlok és fogadása. Például hozzon létre, update, get vagy fájltörléssel különböző műveleteket hajthat végre."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 697eb8b0-4a66-40c7-be7b-6aa6b131c7ad
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/20/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 3f50570191c9b9339fe6584b9056b2549512b789
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-sftp-connector"></a>Hello SFTP összekötő az első lépései
Hello SFTP összekötő tooaccess egy SFTP fiók toosend használja, és fájlokat fogadni. Például hozzon létre, update, get vagy fájltörléssel különböző műveleteket hajthat végre.  

toouse [a csatlakozókat](apis-list.md), először toocreate logikai alkalmazás. Elkezdheti által [logikai alkalmazás létrehozása most](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-toosftp"></a>Csatlakozás tooSFTP
A Logic Apps alkalmazást férhetnek hozzá a szolgáltatáshoz, először jóvá kell toocreate egy *kapcsolat* toohello szolgáltatás. A [kapcsolat](connectors-overview.md) biztosít a logikai alkalmazás és egy másik szolgáltatás közötti kapcsolat.  

### <a name="create-a-connection-toosftp"></a>Egy kapcsolat tooSFTP létrehozása
> [!INCLUDE [Steps toocreate a connection tooSFTP](../../includes/connectors-create-api-sftp.md)]
> 
> 

## <a name="use-an-sftp-trigger"></a>Használja az SFTP eseményindító
Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény. [További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

Ebben a példában hello **SFTP - amikor egy fájl hozzáadása vagy módosítása** eseményindító használt tooinitiate logic app munkafolyamat akkor, ha egy fájl hozzáadja vagy módosítás dátuma, az SFTP-kiszolgáló. Is hozzáadhat olyan feltétel, amely ellenőrzi a hello hello új vagy módosított fájl tartalmát, és lehetővé teszi egy döntési tooextract hello fájl tartalmának jelzik, hogy azt ki kell nyerni hello tartalmak használata előtt. Végül adja hozzá egy fájl művelet tooextract hello tartalmát, és helyezze el hello kivont tartalom hello SFTP kiszolgálón egy mappában. 

A vállalati tegyük fel új fájlok, amelyek megfelelnek a megrendelések használhat az eseményindító toomonitor az SFTP mappa.  SFTP-összekötő intézkedésre aztán használhatja például a **fájl tartalmának lekérdezése**, hello ahhoz, hogy további feldolgozás és a tárolás rendelések adatbázist tooget hello tartalmát.

> [!INCLUDE [Steps toocreate an SFTP trigger](../../includes/connectors-create-api-sftp-trigger.md)]
> 
> 

## <a name="add-a-condition"></a>Feltétel hozzáadása
> [!INCLUDE [Steps tooadd a condition](../../includes/connectors-create-api-sftp-condition.md)]
> 
> 

## <a name="use-an-sftp-action"></a>SFTP-művelet használata
Egy művelet által definiált logikai alkalmazás hello munkafolyamat során. [További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

> [!INCLUDE [Steps toocreate an SFTP action](../../includes/connectors-create-api-sftp-action.md)]
> 
> 

## <a name="connector-specific-details"></a>Összekötő-specifikus részletei

Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/sftpconnector/).

## <a name="more-connectors"></a>További összekötők
Lépjen vissza toohello [API-k lista](apis-list.md).
