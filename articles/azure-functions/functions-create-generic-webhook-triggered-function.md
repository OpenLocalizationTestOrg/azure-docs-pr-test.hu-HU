---
title: "egy Azure-változáskor induló általános webhook függvényt aaaCreate |} Microsoft Docs"
description: "Használja az Azure Functions toocreate egy kiszolgáló nélküli függvény, amelyet a webhook az Azure-ban."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: fafc10c0-84da-4404-b4fa-eea03c7bf2b1
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/12/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 0a4868da91d216c8d20930ce7ec82eaa059c75ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-triggered-by-a-generic-webhook"></a>Általános webhook által indított függvény létrehozása

Az Azure Functions lehetővé teszi, hogy a kód egy kiszolgáló nélküli környezetben anélkül, hogy hozzon létre egy virtuális Gépet, vagy tegye közzé a webalkalmazást toofirst hajtható végre. Konfigurálhatja például, egy Azure-figyelő által kiváltott riasztást által indított függvény toobe. Ez a témakör bemutatja, hogyan tooexecute C#-kódban erőforráscsoport esetén hozzáadott tooyour előfizetés.   

![Általános webhook indított függvényt hello Azure-portálon](./media/functions-create-generic-webhook-triggered-function/function-completed.png)

## <a name="prerequisites"></a>Előfeltételek 

toocomplete Ez az oktatóanyag:

+ Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Azure-függvényalkalmazás létrehozása

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

A függvény a következő alkalmazásban hello új függvény létrehozása.

## <a name="create-function"></a>Általános webhook indított függvény létrehozása

1. Bontsa ki a függvény alkalmazást, majd kattintson a hello ** + ** gomb melletti túl**funkciók**. Ha ez a függvény van hello elsőt a függvény alkalmazásban, válassza ki a **egyéni függvény**. Ez a függvény sablonok teljes készletének hello jeleníti meg.

    ![Gyors üzembe helyezés lap funkciók hello Azure-portálon](./media/functions-create-generic-webhook-triggered-function/add-first-function.png)

2. Jelölje be hello **általános WebHook - C#** sablont. Adja meg a C# függvény nevét, majd válasszon **létrehozása**.

     ![Általános webhook indított függvény létrehozása a hello Azure-portálon](./media/functions-create-generic-webhook-triggered-function/functions-create-generic-webhook-trigger.png) 

2. Kattintson az új függvény **<> / Get függvény URL-cím**, majd másolja ki és mentse a hello érték. Az érték tooconfigure hello webhook használhatja. 

    ![Tekintse át a hello funkciókódot](./media/functions-create-generic-webhook-triggered-function/functions-copy-function-url.png)
         
A következő hoz létre egy webhook végpont Azure figyelése tevékenység napló riasztást. 

## <a name="create-an-activity-log-alert"></a>Tevékenység napló riasztás létrehozása

1. A hello Azure-portálon, keresse meg a toohello **figyelő** szolgáltatás, válassza **riasztások**, és kattintson a **Hozzáadás figyelmeztetés a napló**.   

    ![Figyelés](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert.png)

2. Hello beállításokkal hello táblázatban megadottak szerint:

    ![Tevékenység napló riasztás létrehozása](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert-settings.png)

    | Beállítás      |  Ajánlott érték   | Leírás                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Tevékenység napló riasztás neve** | erőforrás-csoport-létrehozása – figyelmeztetés | Hello figyelmeztetés a napló nevét. |
    | **Előfizetés** | Az Ön előfizetése | hello előfizetés ehhez az oktatóanyaghoz használ. | 
    |  **Erőforráscsoport** | myResourceGroup | hello riasztási erőforrások telepített hello erőforráscsoportot. Használatával hello ugyanabban az erőforráscsoportban, mivel a függvény app teszi egyszerűbbé tooclean hello az oktatóanyag befejezése után. |
    | **Eseménykategória** | Felügyeleti | Ez a kategória tooAzure erőforrások végrehajtott módosításokat tartalmazza.  |
    | **Erőforrástípus** | Erőforráscsoportok | Riasztások tooresource csoport tevékenységek szűrők. |
    | **Erőforráscsoport**<br/>és **erőforrás** | Összes | Figyelje az összes erőforrást. |
    | **A művelet neve** | Create resource group (Erőforráscsoport létrehozása) | Riasztások toocreate műveletek szűrése. |
    | **Szint** | Tájékoztató | Tájékoztatási szintű riasztások tartalmazzák. | 
    | **Állapot** | Sikeres | Sikeresen befejeződött-e riasztások tooactions szűrők. |
    | **A művelet csoport** | új | Hozzon létre egy új művelet, amely meghatározza a hello művelet vesz igénybe, egy riasztás esetén. |
    | **A művelet csoport neve** | függvény-webhook | A név tooidentify hello művelet csoport.  | 
    | **Rövid név** | funcwebhook | Hello művelet csoport rövid nevét. |  

3. A **műveletek**, új hello beállításokkal hello táblázatban megadott művelet: 

    ![Egy művelet csoport hozzáadása](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert-settings-2.png)

    | Beállítás      |  Ajánlott érték   | Leírás                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Name (Név)** | CallFunctionWebhook | Hello művelet nevét. |
    | **Művelet típusa** | Webhook | hello válasz toohello riasztás, hogy a Webhook URL-cím neve. |
    | **Részletek** | Függvény URL-címe | Illessze be a korábban kimásolt hello függvény hello webhook URL-CÍMÉT. |v

4. Kattintson a **OK** toocreate hello riasztás és művelet csoport.  

hello webhook neve erőforráscsoport létrehozásakor az előfizetéshez. A következő hello kód frissítenie a függvény toohandle hello JSON naplóadatokat hello kérelem hello törzsében.   

## <a name="update-hello-function-code"></a>Hello funkciókódot frissítése

1. Lépjen vissza tooyour függvény app hello portálon, és bontsa ki a függvény. 

2. Hello C# parancsfájlkód hello portálon hello függvényében cserélje le a következő kód hello:

    ```csharp
    #r "Newtonsoft.Json"
    
    using System;
    using System.Net;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
    {
        log.Info($"Webhook was triggered!");
    
        // Get hello activityLog object from hello JSON in hello message body.
        string jsonContent = await req.Content.ReadAsStringAsync();
        JToken activityLog = JObject.Parse(jsonContent.ToString())
            .SelectToken("data.context.activityLog");
    
        // Return an error if hello resource in hello activity log isn't a resource group. 
        if (activityLog == null || !string.Equals((string)activityLog["resourceType"], 
            "Microsoft.Resources/subscriptions/resourcegroups"))
        {
            log.Error("An error occured");
            return req.CreateResponse(HttpStatusCode.BadRequest, new
            {
                error = "Unexpected message payload or wrong alert received."
            });
        }
    
        // Write information about hello created resource group toohello streaming log.
        log.Info(string.Format("Resource group '{0}' was {1} on {2}.",
            (string)activityLog["resourceGroupName"],
            ((string)activityLog["subStatus"]).ToLower(), 
            (DateTime)activityLog["submissionTimestamp"]));
    
        return req.CreateResponse(HttpStatusCode.OK);    
    }
    ```

Hello függvény most tesztelheti az előfizetés egy új erőforráscsoport létrehozásával.

## <a name="test-hello-function"></a>Hello függvény tesztelése

1. Hello erőforrás csoport ikonra hello bal oldalán hello Azure portálon, válassza a **+ Hozzáadás**, adjon meg egy **erőforráscsoport-név**, és válassza ki **létrehozása** toocreate egy üres erőforráscsoporthoz.
    
    ![Hozzon létre egy erőforráscsoportot.](./media/functions-create-generic-webhook-triggered-function/functions-create-resource-group.png)

2. Lépjen vissza tooyour függvény, és bontsa ki a hello **naplók** ablak. Hello erőforráscsoport létrehozása után hello tevékenység napló riasztási eseményindítók hello webhook és hello függvény végrehajtása. Hello új erőforráscsoport toohello naplók írt hello nevét láthatja.  

    ![Egy teszt Alkalmazásbeállítás hozzáadása.](./media/functions-create-generic-webhook-triggered-function/function-view-logs.png)

3. (Választható) Lépjen vissza, és létrehozott hello-csoport törléséhez. Vegye figyelembe, hogy ez a tevékenység hello függvény nem következik. Ennek az az oka törlési műveletek hello riasztás ki van szűrve. 

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Következő lépések

Egy függvényt, amely akkor fut, amikor egy kérelem érkezett egy általános webhook hozott létre. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

További információt a webhook-eseményindítókról az [Azure Functions – HTTP- és webhookkötések](functions-bindings-http-webhook.md) című témakörben talál. További információ a C# funkciók fejlesztése toolearn lásd: [Azure Functions C# parancsfájl fejlesztői leírás](functions-reference-csharp.md).

