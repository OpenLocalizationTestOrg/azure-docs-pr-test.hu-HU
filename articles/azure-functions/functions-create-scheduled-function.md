---
title: "egy függvényt, amely az Azure-ban ütemezés szerint futtatott aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate futó Azure-ban függvény alapján meghatározott ütemezés szerint."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: ba50ee47-58e0-4972-b67b-828f2dc48701
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 793b06a65a154466dfd4c121bcc88082227cd597
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-in-azure-that-is-triggered-by-a-timer"></a>Időzítő által aktivált függvény létrehozása az Azure-ban

Ismerje meg, hogyan toouse Azure Functions toocreate lefutó függvény alapján meghatározott ütemezés szerint.

![Függvény-alkalmazás létrehozása az Azure-portálon hello](./media/functions-create-scheduled-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Előfeltételek

toocomplete Ez az oktatóanyag:

+ Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Azure-függvényalkalmazás létrehozása

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![A függvényalkalmazás elkészült.](./media/functions-create-first-azure-function/function-app-create-success.png)

A függvény a következő alkalmazásban hello új függvény létrehozása.

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a>Időzítő által aktivált függvény létrehozása

1. Bontsa ki a függvény alkalmazást, majd kattintson a hello  **+**  gomb melletti túl**funkciók**. Ha ez az első függvényét hello az függvény alkalmazásban, válassza ki a **egyéni függvény**. Ez a függvény sablonok teljes készletének hello jeleníti meg.

    ![Gyors üzembe helyezés lap funkciók hello Azure-portálon](./media/functions-create-scheduled-function/add-first-function.png)

2. Jelölje be hello **TimerTrigger** sablont a kívánt nyelvet. Majd beállításokkal hello hello táblázatban megadottak szerint:

    ![Hozzon létre egy indított időzítő hello Azure-portálon.](./media/functions-create-scheduled-function/functions-create-timer-trigger.png)

    | Beállítás | Ajánlott érték | Leírás |
    |---|---|---|
    | **A függvény neve** | TimerTriggerCSharp1 | Az időzítő indított függvény hello nevét adja meg. |
    | **[Ütemezés](http://en.wikipedia.org/wiki/Cron#CRON_expression)** | 0 \*/1 \* \* \* \* | A hat mező [CRON-kifejezés](http://en.wikipedia.org/wiki/Cron#CRON_expression) , amely ütemezi a függvény toorun percenként. |

2. Kattintson a **Create** (Létrehozás) gombra. Létrejön egy függvény a választott nyelven, amely minden percben futni fog.

3. Ellenőrizze a végrehajtási toohello naplók írt nyomkövetési adatok megtekintéséhez használatos időkategóriát.

    ![Funkciók viewer bejelentkezés hello Azure-portálon.](./media/functions-create-scheduled-function/functions-timer-trigger-view-logs2.png)

Most módosíthatja hello függvény ütemezését, hogy kevesebb gyakran, például óránként egyszer fusson. 

## <a name="update-hello-timer-schedule"></a>Frissítés hello időzítő ütemezése

1. Bontsa ki a függvényt, és kattintson az **Integráció** elemre. Ez az ahol határozza meg a bemeneti és kimeneti kötések a függvény és hello ütemezést is beállíthat. 

2. Adja meg az új `0 0 */1 * * *` értéket az **Ütemezés** beállításnál, és kattintson a **Mentés** gombra.  

![Funkciók hello Azure-portálon időzítő ütemezés frissítése.](./media/functions-create-scheduled-function/functions-timer-trigger-change-schedule.png)

A függvény ezután óránként egyszer fog futni. 

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Következő lépések

Létrehozott egy ütemezés alapján futó függvényt.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

További információ az időzítési eseményindítóktól: [A kódvégrehajtás ütemezése az Azure Functions szolgáltatásban](functions-bindings-timer.md).