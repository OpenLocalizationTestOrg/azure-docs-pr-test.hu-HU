---
title: "aaaScenario - eseményindító logic Apps alkalmazások az Azure Functions és az Azure Service Bus |} Microsoft Docs"
description: "Egy függvény tootrigger logikai alkalmazás létrehozása az Azure Functions és az Azure Service Bus használatával"
services: logic-apps,functions
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 19cbd921-7071-4221-ab86-b44d0fc0ecef
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/23/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: a7b78ebcfe492eee2e08ceeae6b9c5f8ed4717bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scenario-trigger-a-logic-app-with-azure-functions-and-azure-service-bus"></a>Forgatókönyv: Egy logikai alkalmazást az Azure Functions és az Azure Service Bus indítás

Ha egy figyelő vagy feladat toodeploy van szüksége, az Azure Functions toocreate eseményindító használható logikai alkalmazás. Például hozzon létre egy függvényt, amely figyeli várólistát, és azonnal érvényesítést leküldéses kiindulópontként logikai alkalmazás.

## <a name="build-hello-logic-app"></a>Hello logikai alkalmazás létrehozása
Ebben a példában, hogy a funkciót, az egyes logikai alkalmazás, amelyet a indított toobe futtatását. Először hozzon létre egy logikai alkalmazás, amely egy HTTP-kérelem eseményindító tartozik. hello függvény meghívja az adott végpontra, ha egy üzenetsor-üzenetet kap.  

1. Logikai alkalmazás létrehozása.
2. Jelölje be hello **manuális - HTTP-kérelem fogadásakor** eseményindító.
   Porttartományt, megadhat egy JSON-séma toouse hello várólista üzenettel hasonló eszköz használatával [jsonschema.net](http://jsonschema.net). Illessze be hello séma hello eseményindító. Sémák hello designer hello adatok és a folyamat tulajdonságok könnyebben hello munkafolyamaton keresztül hello alakját megismerése érdekében.
2. Ad hozzá a megjeleníteni kívánt toooccur várólista üzenet fogadása után további lépéseket. Például az Office 365 keresztül e-mail küldése.  
3. Mentse a hello logic app toogenerate hello visszahívási URL-címe hello eseményindító toothis logikai alkalmazást. hello URL-címe valószínűleg hello eseményindító kártya.

![hello visszahívási URL-címe valószínűleg hello eseményindító kártya][1]

## <a name="build-hello-function"></a>Hello függvény létrehozása
A következő függvényhez, hello eseményindító funkcionál, és figyeli a toohello várólista kell létrehoznia.

1. A hello [Azure Functions portálon](https://functions.azure.com/signin), jelölje be **új függvény**, majd válassza ki a hello **ServiceBusQueueTrigger - C#** sablon.
   
    ![Az Azure Functions portálra][2]
2. Hello kapcsolat toohello Service Bus-üzenetsorba, használó Azure Service Bus SDK hello konfigurálása `OnMessageReceive()` figyelő.
3. Egy alapszintű függvény toocall hello logic app végpont (korábban létrehozott) írása egy eseményindító üdvözlőüzenetére várólista segítségével. Íme egy teljes példa egy olyan függvényt. hello példa egy `application/json` üzenet tartalomtípusa, de a típusát, szükség esetén módosíthatja.
   
   ```
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;
   
   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/.........";
   
   public static void Run(string myQueueItem, TraceWriter log)
   {
   
       log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
       using (var client = new HttpClient())
       {
           var response = client.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
       }
   }
   ```

tootest, adja hozzá egy üzenetsor hasonló eszköz használatával [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer). Lásd: hello logikai alkalmazás hello függvény kap üdvözlőüzenetére után azonnal érvényesítést.

<!-- Image References -->
[1]: ./media/logic-apps-scenario-function-sb-trigger/manualtrigger.png
[2]: ./media/logic-apps-scenario-function-sb-trigger/newqueuetriggerfunction.png
