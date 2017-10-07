---
title: "egy függvényt a GitHub webhook által indított Azure aaaCreate |} Microsoft Docs"
description: "Az Azure Functions toocreate, amelyet a GitHub webhook kiszolgáló nélküli függvényt használja."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 36ef34b8-3729-4940-86d2-cb8e176fcc06
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 8ffcde82c9310d749159ed53eab113658e38a030
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-triggered-by-a-github-webhook"></a>GitHub-webhookok által meghívott függvények létrehozása

Megtudhatja, hogyan toocreate webhook HTTP-kérelem a GitHub-specifikus hasznos adatok között az által elindított függvényt.

![Github Webhook indított függvényt hello Azure-portálon](./media/functions-create-github-webhook-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Előfeltételek

+ Legalább egy projekttel rendelkező GitHub-fiók.
+ Azure-előfizetés. Ha még nincs előfizetése, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), mielőtt hozzákezd.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Azure-függvényalkalmazás létrehozása

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![A függvényalkalmazás elkészült.](./media/functions-create-first-azure-function/function-app-create-success.png)

A függvény a következő alkalmazásban hello új függvény létrehozása.

<a name="create-function"></a>

## <a name="create-a-github-webhook-triggered-function"></a>GitHub-webhook által aktivált függvény létrehozása

1. Bontsa ki a függvény alkalmazást, majd kattintson a hello  **+**  gomb melletti túl**funkciók**. Ha ez az első függvényét hello az függvény alkalmazásban, válassza ki a **egyéni függvény**. Ez a függvény sablonok teljes készletének hello jeleníti meg.

    ![Gyors üzembe helyezés lap funkciók hello Azure-portálon](./media/functions-create-github-webhook-triggered-function/add-first-function.png)

2. Jelölje be hello **GitHub WebHook** sablont a kívánt nyelvet. **Nevezze el a függvényt**, majd kattintson a **Létrehozás** elemre.

     ![GitHub webhook indított függvény létrehozása a hello Azure-portálon](./media/functions-create-github-webhook-triggered-function/functions-create-github-webhook-trigger.png) 

3. Kattintson az új függvény **<> / Get függvény URL-cím**, majd másolja ki és mentse a hello értékeket. Ugyanaz a hello **<> / Get GitHub titkos**. Ezen értékek tooconfigure hello webhook használhatja a Githubon.

    ![Tekintse át a hello funkciókódot](./media/functions-create-github-webhook-triggered-function/functions-copy-function-url-github-secret.png)

A következő lépésben egy webhookot hoz létre a GitHub-tárban.

## <a name="configure-hello-webhook"></a>Hello webhook konfigurálása

1. A github webhelyen lépjen a saját tooa tárház. Használhat bármely elágaztatott adattárat is. Ha a tárház toofork van szüksége, <https://github.com/Azure-Samples/functions-quickstart>.

1. Kattintson a **Settings** (Beállítások), majd a **Webhooks** (Webhookok) és végül az **Add webhook** (Webhook hozzáadása) elemre.

    ![GitHub-webhook hozzáadása](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-2.png)

1. Hello táblázatban megadott beállításokkal, majd kattintson az **adja hozzá a webhook**.

    ![Hello webhook URL-CÍMÉT és a titkos kulcs beállítása](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-3.png)

| Beállítás | Ajánlott érték | Leírás |
|---|---|---|
| **Hasznos adat URL-címe** | Másolt érték | Hello által visszaadott értéket használja **<> / Get függvény URL-cím**. |
| **Titkos kód**   | Másolt érték | Hello által visszaadott értéket használja **<> / Get GitHub titkos**. |
| **Tartalom típusa** | application/json | hello függvényhez a JSON hasznos adatok között. |
| Eseményindítók | Én szeretném kiválasztani az egyes eseményeket | Csak szeretnénk tootrigger probléma Megjegyzés események.  |
| | Probléma megjegyzése |  |

Most, hello webhook, akkor konfigurált tootrigger a függvény egy új probléma megjegyzés hozzáadásakor.

## <a name="test-hello-function"></a>Hello függvény tesztelése

1. Nyissa meg a GitHub-tárház hello **problémák** fülre egy új böngészőablakban.

1. Hello új ablakban kattintson **új probléma**, írja be a címet, és kattintson a **küldje el az új probléma**.

1. Hello probléma, írja be egy megjegyzést, és kattintson a **Megjegyzés**.

    ![Adja meg a GitHub-problémára vonatkozó megjegyzést.](./media/functions-create-github-webhook-triggered-function/functions-github-webhook-add-comment.png)

1. Lépjen vissza toohello portálon, és a hello naplók megtekintése. Meg kell jelennie egy nyomkövetési bejegyzés hello új megjegyzés szövege.

     ![Hello naplók megtekintése hello Megjegyzés szövege.](./media/functions-create-github-webhook-triggered-function/function-app-view-logs.png)

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Következő lépések

Létrehozott egy függvényt, amely akkor fut, amikor kérelem érkezik egy GitHub-webhookból.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

További információt a webhook-eseményindítókról az [Azure Functions – HTTP- és webhookkötések](functions-bindings-http-webhook.md) című témakörben talál.
