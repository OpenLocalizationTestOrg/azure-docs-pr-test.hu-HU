---
title: "az Azure üzenetsor-üzeneteket által indított függvény aaaCreate |} Microsoft Docs"
description: "Használja az Azure Functions toocreate egy kiszolgáló nélküli függvény, amelyet egy üzenetek által benyújtott tooan Azure Storage üzenetsorába."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 361da2a4-15d1-4903-bdc4-cc4b27fc3ff4
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: e9501ed336b502eaeee3fa62ec4ae085c76de0ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-triggered-by-azure-queue-storage"></a>Azure Storage-üzenetsor által aktivált függvény létrehozása

Ismerje meg, hogyan toocreate üzenetek által elindított függvény benyújtott tooan Azure Storage üzenetsorába.

![Hello naplókban üzenet megtekintése.](./media/functions-create-storage-queue-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Előfeltételek

- Töltse le és telepítse a hello [Microsoft Azure Tártallózó](http://storageexplorer.com/).

- Azure-előfizetés. Ha még nincs előfizetése, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), mielőtt hozzákezd.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Azure-függvényalkalmazás létrehozása

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![A függvényalkalmazás elkészült.](./media/functions-create-first-azure-function/function-app-create-success.png)

A függvény a következő alkalmazásban hello új függvény létrehozása.

<a name="create-function"></a>

## <a name="create-a-queue-triggered-function"></a>Üzenetsor által aktivált függvény létrehozása

1. Bontsa ki a függvény alkalmazást, majd kattintson a hello  **+**  gomb melletti túl**funkciók**. Ha ez az első függvényét hello az függvény alkalmazásban, válassza ki a **egyéni függvény**. Ez a függvény sablonok teljes készletének hello jeleníti meg.

    ![Gyors üzembe helyezés lap funkciók hello Azure-portálon](./media/functions-create-storage-queue-triggered-function/add-first-function.png)

2. Jelölje be hello **QueueTrigger** sablont a kívánt nyelvet, és a hello tábla hello-beállítások használata.

    ![Hello tárolási indított várólista-függvény létrehozása.](./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal.png)
    
    | Beállítás | Ajánlott érték | Leírás |
    |---|---|---|
    | **Üzenetsor neve**   | myqueue-items    | Hello neve várólista tooconnect tooin a tárfiók. |
    | **Tárfiók kapcsolata** | AzureWebJobStorage | A függvény alkalmazás által már használt hello tárolási fiók kapcsolat használatát, vagy hozzon létre egy újat.  |
    | **A függvény neve** | Egyedi a függvényalkalmazásban | Az üzenetsor által aktivált függvény neve. |

3. Kattintson a **létrehozása** toocreate a függvény.

A következő tooyour Azure Storage-fiók csatlakozzon, és hozzon létre hello **Várólista_neve-elemek** tároló várólista.

## <a name="create-hello-queue"></a>Hello várólista létrehozása

1. A függvényben kattintson az **Integráció** elemre, bontsa ki a **Dokumentáció** elemet, és másolja a **Fiók neve** és a **Fiók kulcsa** értéket. Ezen hitelesítő adatok tooconnect toohello tárfiókot használni. Ha a tárfiók már csatlakozott, hagyja ki a toostep 4.

    ![Hello tárfiók kapcsolat hitelesítő adatainak lekéréséhez.](./media/functions-create-storage-queue-triggered-function/functions-storage-account-connection.png)v

1. Hello futtatása [Microsoft Azure Tártallózó](http://storageexplorer.com/) eszköz, kattintson a hello hello bal oldali ikon csatlakozni, válassza a **használja a tárfiók nevét és a kulcs**, és kattintson a **következő**.

    ![Hello fiók Tártallózó eszköz futtatásához.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-1.png)

1. Adja meg a hello **fióknév** és **fiókkulcs** 1. lépésben kattintson **következő** , majd **Connect**.

    ![Adja meg hello tároló hitelesítő adatait, és csatlakozzon.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-2.png)

1. Bontsa ki a csatolt hello storage-fiókot, kattintson a jobb gombbal **várólisták**, kattintson a **létrehozás várólista**, típus `myqueue-items`, és nyomja meg az enter.

    ![Hozzon létre egy tárolási üzenetsort.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-create-queue.png)

Most, hogy a tároló várólista, tesztelheti hello függvény toohello üzenet-várólista hozzáadásával.

## <a name="test-hello-function"></a>Hello függvény tesztelése

1. Vissza a hello Azure-portálon, a Tallózás tooyour függvény bontsa ki a hello **naplók** hello alján lévő hello lap, és győződjön meg arról, hogy a napló streaming nem szünetel.

1. A Storage Explorerben bontsa ki a tárfiókot, az **Üzenetsorok**, majd az **üzenetsorelemek** elemet, és kattintson az **Üzenet hozzáadása** elemre.

    ![Adjon hozzá egy üzenetsor toohello.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-add-message.png)

1. Írja be a „Hello World!” üzenetet az **Üzenet szövege** mezőbe, és kattintson az **OK** gombra.

1. Várjon néhány másodpercet, majd lépjen vissza tooyour függvény naplókat, és győződjön meg arról, hogy új üdvözlőüzenetére kiolvasott hello várólista.

    ![Hello naplókban üzenet megtekintése.](./media/functions-create-storage-queue-triggered-function/functions-queue-storage-trigger-view-logs.png)

1. Tártallózó, kattintson **frissítése** , és győződjön meg arról, hogy üdvözlőüzenetére feldolgozása megtörtént, és már nem hello várólista.

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Következő lépések

Létrehozott egy függvényt, amely egy üzenetet tooa storage üzenetsorába felvételekor futnak.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

További információ a tárolási üzenetsor eseményindítóiról: [Azure Functions – a tárolási üzenetsor kötései](functions-bindings-storage-queue.md).