---
title: "az IoT-központ esemény tooyour Azure idő adatsorozat Insights forráskörnyezettel aaaHow tooadd |} Microsoft Docs"
description: "Ez az oktatóanyag ismerteti, hogyan egy eseményt, amely forrás tooadd csatlakoztatva tooan IoT-központ tooyour idő adatsorozat Insights környezet"
keywords: 
services: time-series-insights
documentationcenter: 
author: sandshadow
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/19/2017
ms.author: edett
ms.openlocfilehash: c626f9653d1c012360120fa9fc3d211d7d5beb5b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-an-iot-hub-event-source"></a>Hogyan tooadd egy IoT-központ eseményforrás

Ez az oktatóanyag bemutatja, hogyan toouse hello Azure portál tooadd egy eseményforrás, amely egy IoT-központ tooyour idő adatsorozat Insights környezetből olvassa be.

## <a name="prerequisites"></a>Előfeltételek

Az IoT-központ hozott létre, és írás események tooit. Az IoT-központok további információkért lásd: <https://azure.microsoft.com/services/iot-hub/>

> [A fogyasztói csoportok] Minden alkalommal adatsorozat Insights eseményforrás kell toohave saját dedikált fogyasztói csoportot, amelyek nincsenek megosztva, más fogyasztóval. Ha több olvasók felhasználásához származó események hello azonos fogyasztói csoportot, minden olvasók valószínűleg toosee hibák. További információkért lásd: hello [IoT Hub fejlesztői útmutató](../iot-hub/iot-hub-devguide.md).

## <a name="choose-an-import-option"></a>Válassza ki importálása

hello eseményforrás hello beállításait is meg lehet adni manuálisan, vagy az IoT-központ a hello IoT-központok, amelyek a rendelkezésre álló tooyou lehet kiválasztani.
A hello **importálási lehetőség** választó, válasszon egyet az alábbi beállítások hello:

* Adja meg az IoT-központ beállításait manuálisan
* Az elérhető előfizetések IoT Hubjának használata

### <a name="select-an-available-iot-hub"></a>Válassza ki az elérhető IoT-központ

hello következő táblázat ismerteti a hello új esemény (forrás) lapot annak leírását az egyes lehetőségek lehetőséget választva egy elérhető IoT-központot egy esemény forrásként:

| TULAJDONSÁG NEVE | LEÍRÁS |
| --- | --- |
| Eseményforrás neve | a forrás hello neve. Ez a név a idő adatsorozat Insights környezeten belül egyedinek kell lennie.
| Forrás | Válasszon **IoT-központ** toocreate egy IoT-központ esemény forrását.
| Előfizetés-azonosító | Válassza ki, amelyben ez az IoT-központ készült hello előfizetést.
| Az IoT-központ nevét | Válassza ki az IoT-központ hello hello nevét.
| Az IoT hub házirend neve | Válassza ki a megosztott hello hozzáférési házirendet, amely hello IoT-központ beállítások lapján található. Minden megosztott elérési házirend rendelkezik egy nevet, hogy Ön meghatározott engedélyekkel és hozzáférési kulcsokkal. hello megosztott hozzáférési házirend a eseményforrás *kell* rendelkezik **service csatlakozás** engedélyek.
| Az IoT hub fogyasztói csoportot | hello fogyasztói csoportot tooread származó események hello IoT-központot. Nagyon fontos ajánlott toouse az eseményforrás a dedikált fogyasztói csoportot.

### <a name="provide-iot-hub-settings-manually"></a>Adja meg az IoT-központ beállításait manuálisan

hello következő táblázat ismerteti az egyes tulajdonságai hello új esemény (forrás) lapot a leírás beállításainak megadásakor manuálisan:

| TULAJDONSÁG NEVE | LEÍRÁS |
| --- | --- |
| Eseményforrás neve | a forrás hello neve. Ez a név a idő adatsorozat Insights környezeten belül egyedinek kell lennie.
| Forrás | Válasszon **IoT-központ** toocreate egy IoT-központ esemény forrását.
| Előfizetés-azonosító | hello előfizetés, amelyben ez az IoT hub létrehozása történt.
| Erőforráscsoport | hello előfizetés, amelyben ez az IoT hub létrehozása történt.
| Az IoT-központ nevét | az IoT Hub hello neve. Az IoT hub létrehozása után, akkor nevet is adott neki
| Az IoT hub házirend neve | hello megosztott hozzáférési házirendet, amely hello IoT-központ beállítások lapon lehet létrehozni. Minden megosztott elérési házirend rendelkezik egy nevet, hogy Ön meghatározott engedélyekkel és hozzáférési kulcsokkal. hello megosztott hozzáférési házirend a eseményforrás *kell* rendelkezik **service csatlakozás** engedélyek.
| Az IoT hub házirend kulcs | hello megosztott elérési kulcsot használt tooauthenticate hozzáférés toohello Service Bus-névtér. Ide írja be hello elsődleges vagy másodlagos kulcsot.
| Az IoT hub fogyasztói csoportot | hello fogyasztói csoportot tooread származó események hello IoT-központot. Nagyon fontos ajánlott toouse az eseményforrás a dedikált fogyasztói csoportot.

## <a name="next-steps"></a>Következő lépések

1. Adja hozzá a adatok hozzáférési házirend tooyour környezet [Define adat-hozzáférési házirendek](time-series-insights-data-access.md)
1. Férhet hozzá a környezethez a hello [idő adatsorozat Insights portálon találja meg](https://insights.timeseries.azure.com)
