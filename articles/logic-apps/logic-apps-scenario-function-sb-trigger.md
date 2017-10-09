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
# <a name="scenario-trigger-a-logic-app-with-azure-functions-and-azure-service-bus"></a><span data-ttu-id="37fae-103">Forgatókönyv: Egy logikai alkalmazást az Azure Functions és az Azure Service Bus indítás</span><span class="sxs-lookup"><span data-stu-id="37fae-103">Scenario: Trigger a logic app with Azure Functions and Azure Service Bus</span></span>

<span data-ttu-id="37fae-104">Ha egy figyelő vagy feladat toodeploy van szüksége, az Azure Functions toocreate eseményindító használható logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="37fae-104">You can use Azure Functions toocreate a trigger for a logic app when you need toodeploy a long-running listener or task.</span></span> <span data-ttu-id="37fae-105">Például hozzon létre egy függvényt, amely figyeli várólistát, és azonnal érvényesítést leküldéses kiindulópontként logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="37fae-105">For example, you can create a function that listens in on a queue and then immediately fire a logic app as a push trigger.</span></span>

## <a name="build-hello-logic-app"></a><span data-ttu-id="37fae-106">Hello logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="37fae-106">Build hello logic app</span></span>
<span data-ttu-id="37fae-107">Ebben a példában, hogy a funkciót, az egyes logikai alkalmazás, amelyet a indított toobe futtatását.</span><span class="sxs-lookup"><span data-stu-id="37fae-107">In this example, you have a function running for each logic app that needs toobe triggered.</span></span> <span data-ttu-id="37fae-108">Először hozzon létre egy logikai alkalmazás, amely egy HTTP-kérelem eseményindító tartozik.</span><span class="sxs-lookup"><span data-stu-id="37fae-108">First, create a logic app that has an HTTP request trigger.</span></span> <span data-ttu-id="37fae-109">hello függvény meghívja az adott végpontra, ha egy üzenetsor-üzenetet kap.</span><span class="sxs-lookup"><span data-stu-id="37fae-109">hello function calls that endpoint whenever a queue message is received.</span></span>  

1. <span data-ttu-id="37fae-110">Logikai alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="37fae-110">Create a logic app.</span></span>
2. <span data-ttu-id="37fae-111">Jelölje be hello **manuális - HTTP-kérelem fogadásakor** eseményindító.</span><span class="sxs-lookup"><span data-stu-id="37fae-111">Select hello **Manual - When an HTTP Request is Received** trigger.</span></span>
   <span data-ttu-id="37fae-112">Porttartományt, megadhat egy JSON-séma toouse hello várólista üzenettel hasonló eszköz használatával [jsonschema.net](http://jsonschema.net).</span><span class="sxs-lookup"><span data-stu-id="37fae-112">Optionally, you can specify a JSON schema toouse with hello queue message by using a tool like [jsonschema.net](http://jsonschema.net).</span></span> <span data-ttu-id="37fae-113">Illessze be hello séma hello eseményindító.</span><span class="sxs-lookup"><span data-stu-id="37fae-113">Paste hello schema in hello trigger.</span></span> <span data-ttu-id="37fae-114">Sémák hello designer hello adatok és a folyamat tulajdonságok könnyebben hello munkafolyamaton keresztül hello alakját megismerése érdekében.</span><span class="sxs-lookup"><span data-stu-id="37fae-114">Schemas help hello designer understand hello shape of hello data and flow properties more easily through hello workflow.</span></span>
2. <span data-ttu-id="37fae-115">Ad hozzá a megjeleníteni kívánt toooccur várólista üzenet fogadása után további lépéseket.</span><span class="sxs-lookup"><span data-stu-id="37fae-115">Add any additional steps that you want toooccur after a queue message is received.</span></span> <span data-ttu-id="37fae-116">Például az Office 365 keresztül e-mail küldése.</span><span class="sxs-lookup"><span data-stu-id="37fae-116">For example, send an email via Office 365.</span></span>  
3. <span data-ttu-id="37fae-117">Mentse a hello logic app toogenerate hello visszahívási URL-címe hello eseményindító toothis logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="37fae-117">Save hello logic app toogenerate hello callback URL for hello trigger toothis logic app.</span></span> <span data-ttu-id="37fae-118">hello URL-címe valószínűleg hello eseményindító kártya.</span><span class="sxs-lookup"><span data-stu-id="37fae-118">hello URL appears on hello trigger card.</span></span>

![hello visszahívási URL-címe valószínűleg hello eseményindító kártya][1]

## <a name="build-hello-function"></a><span data-ttu-id="37fae-120">Hello függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="37fae-120">Build hello function</span></span>
<span data-ttu-id="37fae-121">A következő függvényhez, hello eseményindító funkcionál, és figyeli a toohello várólista kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="37fae-121">Next, you must create a function that acts as hello trigger and listens toohello queue.</span></span>

1. <span data-ttu-id="37fae-122">A hello [Azure Functions portálon](https://functions.azure.com/signin), jelölje be **új függvény**, majd válassza ki a hello **ServiceBusQueueTrigger - C#** sablon.</span><span class="sxs-lookup"><span data-stu-id="37fae-122">In hello [Azure Functions portal](https://functions.azure.com/signin), select **New Function**, and then select hello **ServiceBusQueueTrigger - C#** template.</span></span>
   
    ![Az Azure Functions portálra][2]
2. <span data-ttu-id="37fae-124">Hello kapcsolat toohello Service Bus-üzenetsorba, használó Azure Service Bus SDK hello konfigurálása `OnMessageReceive()` figyelő.</span><span class="sxs-lookup"><span data-stu-id="37fae-124">Configure hello connection toohello Service Bus queue, which uses hello Azure Service Bus SDK `OnMessageReceive()` listener.</span></span>
3. <span data-ttu-id="37fae-125">Egy alapszintű függvény toocall hello logic app végpont (korábban létrehozott) írása egy eseményindító üdvözlőüzenetére várólista segítségével.</span><span class="sxs-lookup"><span data-stu-id="37fae-125">Write a basic function toocall hello logic app endpoint (created earlier) by using hello queue message as a trigger.</span></span> <span data-ttu-id="37fae-126">Íme egy teljes példa egy olyan függvényt.</span><span class="sxs-lookup"><span data-stu-id="37fae-126">Here's a full example of a function.</span></span> <span data-ttu-id="37fae-127">hello példa egy `application/json` üzenet tartalomtípusa, de a típusát, szükség esetén módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="37fae-127">hello example uses an `application/json` message content type, but you can change this type as necessary.</span></span>
   
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

<span data-ttu-id="37fae-128">tootest, adja hozzá egy üzenetsor hasonló eszköz használatával [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer).</span><span class="sxs-lookup"><span data-stu-id="37fae-128">tootest, add a queue message via a tool like [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer).</span></span> <span data-ttu-id="37fae-129">Lásd: hello logikai alkalmazás hello függvény kap üdvözlőüzenetére után azonnal érvényesítést.</span><span class="sxs-lookup"><span data-stu-id="37fae-129">See hello logic app fire immediately after hello function receives hello message.</span></span>

<!-- Image References -->
[1]: ./media/logic-apps-scenario-function-sb-trigger/manualtrigger.png
[2]: ./media/logic-apps-scenario-function-sb-trigger/newqueuetriggerfunction.png
