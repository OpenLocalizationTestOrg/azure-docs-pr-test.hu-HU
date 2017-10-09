---
title: "aaaAdd hello Twilio-összekötő az Azure Logic Apps alkalmazásait |} Microsoft Docs"
description: "Hello Twilio-összekötő REST API-paraméterekkel rendelkező áttekintése"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 43116187-4a2f-42e5-9852-a0d62f08c5fc
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/19/2016
ms.author: mandia; ladocs
ms.openlocfilehash: b2b487f34bc76bee24b4237a71ee767d0d22ff7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-twilio-connector"></a>Hello Twilio-összekötő az első lépései
Csatlakozás tooTwilio toosend, és globális SMS, MMS és IP-üzeneteket fogadni. A Twilio a következőket teheti:

* Hozhat létre. az üzleti folyamat kap Twilio hello adatok alapján. 
* Egy üzenet, a lista üzenetek és a további műveleteket használni. Ezeket a műveleteket válaszol, és végezze el hello kimeneti más műveletek érhető el. Például egy új Twilio-üzenetet kap, ha akkor igénybe ezt az üzenetet, és a Service Bus munkafolyamat használatával. 

Hozzon létre egy logic app; első lépései Lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="create-a-connection-tootwilio"></a>Egy kapcsolat tooTwilio létrehozása
Az összekötő tooyour a logic apps hozzáadásakor adja meg a Twilio-értékeket a következő hello:

| Tulajdonság | Szükséges | Leírás |
| --- | --- | --- |
| Futtatófiók-Azonosítóvá |Igen |Írja be a Twilio-fiók Azonosítóját |
| Hozzáférési jogkivonat |Igen |Adja meg a Twilio-hozzáférési jogkivonat |

> [!INCLUDE [Steps toocreate a connection tooTwilio](../../includes/connectors-create-api-twilio.md)]
> 
> 

Ha a Twilio-hozzáférési jogkivonat nem rendelkezik, tekintse meg a [felhasználói identitás- & hozzáférési jogkivonatok](https://www.twilio.com/docs/api/chat/guides/identity).

## <a name="connector-specific-details"></a>Összekötő-specifikus részletei

Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/twilio/).

## <a name="more-connectors"></a>További összekötők
Lépjen vissza toohello [API-k lista](apis-list.md).
