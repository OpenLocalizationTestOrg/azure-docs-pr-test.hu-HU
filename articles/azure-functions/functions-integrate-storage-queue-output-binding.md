---
title: "az Azure üzenetsor-üzeneteket által indított függvény aaaCreate |} Microsoft Docs"
description: "Használja az Azure Functions toocreate egy kiszolgáló nélküli függvény, amelyet egy üzenetek által benyújtott tooan Azure Storage üzenetsorába."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 0b609bc0-c264-4092-8e3e-0784dcc23b5d
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/17/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 44db90fa80bf77e31bf53dddabd7136de5800b11
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-messages-tooan-azure-storage-queue-using-functions"></a>Adja hozzá az üzenetek tooan Azure Storage üzenetsorába függvények használata

Az Azure Functions bemeneti és kimeneti kötések adja meg a deklaratív módon tooconnect tooexternal szolgáltatás adatait a függvény. Ebben a témakörben megtudhatja, hogyan tooupdate egy meglévő függvény adja hozzá egy kimeneti kötése, amely üzeneteket küld tooAzure a Queue storage.  

![Hello naplókban üzenet megtekintése.](./media/functions-integrate-storage-queue-output-binding/functions-integrate-storage-binding-in-portal.png)

## <a name="prerequisites"></a>Előfeltételek 

[!INCLUDE [Previous topics](../../includes/functions-quickstart-previous-topics.md)]

* Telepítse a hello [Microsoft Azure Tártallózó](http://storageexplorer.com/).

## <a name="add-binding"></a>Kimeneti kötés hozzáadása
 
1. Bontsa ki a függvényalkalmazást és a függvényt.

2. Válassza ki **integráció** és **+ új kimeneti**, majd válassza **Azure Queue storage** válassza **kiválasztása**.
    
    ![Adja hozzá a várólista tárolási kimeneti kötése tooa függvény hello Azure-portálon.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding.png)

3. Hello beállításokkal hello táblázatban megadottak szerint: 

    ![Adja hozzá a várólista tárolási kimeneti kötése tooa függvény hello Azure-portálon.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding-2.png)

    | Beállítás      |  Ajánlott érték   | Leírás                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Üzenetsor neve**   | myqueue-items    | hello hello neve várólista tooconnect tooin a tárfiók. |
    | **Tárfiók kapcsolata** | AzureWebJobStorage | A függvény alkalmazás által már használt hello tárolási fiók kapcsolat használatát, vagy hozzon létre egy újat.  |
    | **Üzenet-paraméter neve** | outputQueueItem | hello hello kimeneti kötési paraméter neve. | 

4. Kattintson a **mentése** tooadd hello kötés.
 
Most, hogy egy meghatározott kimeneti kötése, tooupdate hello kód toouse hello kötés tooadd üzenetek tooa várólista szüksége.  

## <a name="update-hello-function-code"></a>Hello funkciókódot frissítése

1. Válassza ki a függvény toodisplay hello függvény kódot hello szerkesztő. 

2. C# függvény, frissítse a függvénydefiníciót tooadd hello az alábbi módon **outputQueueItem** tárolási kötési paraméter. JavaScript-függvény esetében hagyja ki ezt a lépést.

    ```cs   
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, 
        ICollector<string> outputQueueItem, TraceWriter log)
    {
        ....
    }
    ```

3. Hozzáadása előtt hello metódus visszaadja a következő kód toohello függvény hello. A függvény hello nyelvi hello megfelelő részlet használja.

    ```javascript
    context.bindings.outputQueueItem = "Name passed toohello function: " + 
                (req.query.name || req.body.name);
    ```

    ```cs
    outputQueueItem.Add("Name passed toohello function: " + name);     
    ```

4. Válassza ki **mentése** toosave módosításokat.

egy üzenetsor hozzáadott toohello toohello HTTP-eseményindítóval átadott hello érték tartalmazza.
 
## <a name="test-hello-function"></a>Hello függvény tesztelése 

1. Hello kód módosítások mentésekor után válassza ki **futtatása**. 

    ![Adja hozzá a várólista tárolási kimeneti kötése tooa függvény hello Azure-portálon.](./media/functions-integrate-storage-queue-output-binding/functions-test-run-function.png)

2. Ellenőrizze a hello naplók toomake meg arról, hogy sikerült-e hello függvény. Egy új sor nevű **outqueue** hozta létre a tárfiókban lévő hello funkciók runtime hello kimeneti kötése először szolgál.

A következő tooyour tárolási fiók tooverify hello új várólista és tooit hozzáadott üdvözlőüzenetére is elérheti. 

## <a name="connect-toohello-queue"></a>Csatlakozás toohello várólista

Kihagyás hello első három lépést, ha már telepítette a Tártallózó és tooyour tárfiók csatlakozna.    

1. Válassza ki a függvény **integráció** és új hello **Azure Queue storage** kimeneti kötése, majd bontsa ki a **dokumentáció**. Másolja a **Fiók neve** és a **Fiók kulcsa** értéket. Ezen hitelesítő adatok tooconnect toohello tárfiókot használni.
 
    ![Hello tárfiók kapcsolat hitelesítő adatainak lekéréséhez.](./media/functions-integrate-storage-queue-output-binding/function-get-storage-account-credentials.png)

2. Hello futtatása [Microsoft Azure Tártallózó](http://storageexplorer.com/) eszközt, jelölje be hello hello bal oldali ikon csatlakozni, válassza a **használja a tárfiók nevét és a kulcs**, és válassza ki **következő**.

    ![Hello fiók Tártallózó eszköz futtatásához.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-1.png)
    
3. Beillesztés hello **fióknév** és **fiókkulcs** az 1. lépés a megfelelő mezőkbe, majd válasszon **következő**, és **Connect**. 
  
    ![Illessze be a hello tároló hitelesítő adatait, és csatlakozzon.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-2.png)

4. Bontsa ki a csatolt hello tárfiókot, **várólisták** , és ellenőrizze, hogy a várólista neve **Várólista_neve-elemek** létezik-e. Emellett meg kell jelennie egy üzenet már hello várólista.  
 
    ![Hozzon létre egy tárolási üzenetsort.](./media/functions-integrate-storage-queue-output-binding/function-queue-storage-output-view-queue.png)
 

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Következő lépések

Egy kimeneti kötése tooan meglévő függvény hozzáadását. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Kötési tooQueue tárolóira vonatkozó további információkért lásd: [Azure Functions tároló várólista kötések](functions-bindings-storage-queue.md). 



