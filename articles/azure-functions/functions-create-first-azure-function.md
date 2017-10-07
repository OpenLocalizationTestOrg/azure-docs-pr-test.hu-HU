---
title: "az első függvény eltávolítása hello Azure Portal aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan kiszolgáló nélküli végrehajtási a függvény az első Azure toocreate hello Azure-portálon."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 96cf87b9-8db6-41a8-863a-abb828e3d06d
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/07/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 84283d7d4bc6015061946af4589f9a70ae61f36b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-function-in-hello-azure-portal"></a>A hello Azure-portálon az első függvény létrehozása

Az Azure Functions lehetővé teszi, hogy a kód egy kiszolgáló nélküli környezetben anélkül, hogy hozzon létre egy virtuális Gépet, vagy tegye közzé a webalkalmazást toofirst hajtható végre. Ebben a témakörben elsajátíthatja toouse működését toocreate hello Azure-portálon a "hello world" függvényt.

![Függvény-alkalmazás létrehozása az Azure-portálon hello](./media/functions-create-first-azure-function/function-app-in-portal-editor.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure

Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).

## <a name="create-a-function-app"></a>Függvényalkalmazás létrehozása

Rendelkeznie kell egy függvény app toohost hello a függvények végrehajtásához szükséges. A függvényalkalmazás lehetővé teszi, hogy logikai egységbe csoportosítsa a függvényeket az erőforrások egyszerűbb felügyelete, telepítése és megosztása érdekében. 

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

A függvény a következő alkalmazásban hello új függvény létrehozása.

## <a name="create-function"></a>HTTP által aktivált függvény létrehozása

1. Bontsa ki az új függvény alkalmazást, majd kattintson az hello  **+**  gomb melletti túl**funkciók**.

2.  A hello **gyorsan** lapon jelölje be **WebHook + API**, **válassza ki a nyelvet** a függvény, és kattintson a **Ez a függvény létrehozása** . 
   
    ![Az Azure-portálon hello Functions gyorsindítójával.](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)

A következő függvényt egy indított HTTP függvény hello sablonnal a választott nyelven jön létre. HTTP-kérelem küldésével hello új funkció is futtathatja.

## <a name="test-hello-function"></a>Hello függvény tesztelése

1. Az új függvényben kattintson a **< /> Függvény URL-címének beolvasása** elemre, válassza a **default (Function key)** (alapértelmezett (funkcióbillentyű)) lehetőséget, majd kattintson a **Másolás** gombra. 

    ![Hello függvény URL-Címének másolása az Azure-portálon hello](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

2. Hello függvény URL-cím illessze be a böngésző címsorába. Hello lekérdezési karakterlánc hozzáfűzése `&name=<yourname>` toothis URL-címet, és nyomja le az hello `Enter` kulcs a billentyűzet tooexecute hello kérésre. hello az alábbiakban látható egy példa az Edge böngészőben hello hello függvény által visszaadott hello választ:

    ![Függvény válasz hello böngészőben.](./media/functions-create-first-azure-function/function-app-browser-testing.png)

    hello kérelem URL-cím szükséges, alapértelmezés szerint tooaccess kulcsot tartalmaz, a függvény HTTP Protokollon keresztül.   

3. A függvény futásakor nyomkövetési adatok írása toohello naplókat. toosee hello nyomkövetési kimenetét hello előző végrehajtási, térjen vissza a tooyour függvény hello portálon, és kattintson a felfelé mutató nyíl hello képernyő tooexpand hello alján hello **naplók**. 

   ![Funkciók viewer bejelentkezés hello Azure-portálon.](./media/functions-create-first-azure-function/function-view-logs.png)

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

[!INCLUDE [Clean up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Következő lépések

Létrehozott egy függvényalkalmazást és egy HTTP által aktivált egyszerű függvényt.  

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

További információ: [Azure Functions – HTTP- és webhookkötések](functions-bindings-http-webhook.md).



