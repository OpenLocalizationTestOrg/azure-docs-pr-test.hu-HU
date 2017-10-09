---
title: "egy Azure Blob storage által indított függvényt aaaCreate |} Microsoft Docs"
description: "Használja az Azure Functions toocreate egy kiszolgáló nélküli függvény, amelyet elemének hozzáadott tooAzure Blob Storage tárolóban."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: d6bff41c-a624-40c1-bbc7-80590df29ded
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: acb7d29abb07a22da11d0e65d2ed54591f8e3f4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-triggered-by-azure-blob-storage"></a>Azure Blob-tároló által aktivált függvény létrehozása

Ismerje meg, hogyan toocreate váltódik ki, amelyek fájljai egy olyan függvényt feltöltött tooor frissítése az Azure Blob Storage tárolóban.

![Hello naplókban üzenet megtekintése.](./media/functions-create-storage-blob-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Előfeltételek

+ Töltse le és telepítse a hello [Microsoft Azure Tártallózó](http://storageexplorer.com/).
+ Azure-előfizetés. Ha még nincs előfizetése, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), mielőtt hozzákezd.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Azure-függvényalkalmazás létrehozása

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![A függvényalkalmazás elkészült.](./media/functions-create-first-azure-function/function-app-create-success.png)

A függvény a következő alkalmazásban hello új függvény létrehozása.

<a name="create-function"></a>

## <a name="create-a-blob-storage-triggered-function"></a>A blobtároló által aktivált függvény létrehozása

1. Bontsa ki a függvény alkalmazást, majd kattintson a hello  **+**  gomb melletti túl**funkciók**. Ha ez az első függvényét hello az függvény alkalmazásban, válassza ki a **egyéni függvény**. Ez a függvény sablonok teljes készletének hello jeleníti meg.

    ![Gyors üzembe helyezés lap funkciók hello Azure-portálon](./media/functions-create-storage-blob-triggered-function/add-first-function.png)

2. Jelölje be hello **BlobTrigger** sablont a kívánt nyelvet, és a hello tábla hello-beállítások használata.

    ![Hello Blob storage indított függvény létrehozása.](./media/functions-create-storage-blob-triggered-function/functions-create-blob-storage-trigger-portal.png)

    | Beállítás | Ajánlott érték | Leírás |
    |---|---|---|
    | **Elérési út**   | mycontainer/{name}    | A figyelt blobtárolóban található hely. hello blob fájlnevét hello átadása hello kötésben szerint hello _neve_ paraméter.  |
    | **Tárfiók kapcsolata** | AzureWebJobStorage | A függvény alkalmazás által már használt hello tárolási fiók kapcsolat használatát, vagy hozzon létre egy újat.  |
    | **A függvény neve** | Egyedi a függvényalkalmazásban | A blob által aktivált függvény neve. |

3. Kattintson a **létrehozása** toocreate a függvény.

A következő tooyour Azure Storage-fiók csatlakozzon, és hozzon létre hello **mycontainer** tároló.

## <a name="create-hello-container"></a>Hello tároló létrehozása

1. A függvényben kattintson az **Integráció** elemre, bontsa ki a **Dokumentáció** elemet, és másolja a **Fiók neve** és a **Fiók kulcsa** értéket. Ezen hitelesítő adatok tooconnect toohello tárfiókot használni. Ha a tárfiók már csatlakozott, hagyja ki a toostep 4.

    ![Hello tárfiók kapcsolat hitelesítő adatainak lekéréséhez.](./media/functions-create-storage-blob-triggered-function/functions-storage-account-connection.png)

1. Hello futtatása [Microsoft Azure Tártallózó](http://storageexplorer.com/) eszköz, kattintson a hello hello bal oldali ikon csatlakozni, válassza a **használja a tárfiók nevét és a kulcs**, és kattintson a **következő**.

    ![Hello fiók Tártallózó eszköz futtatásához.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-connect-1.png)

1. Adja meg a hello **fióknév** és **fiókkulcs** 1. lépésben kattintson **következő** , majd **Connect**. 

    ![Adja meg hello tároló hitelesítő adatait, és csatlakozzon.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-connect-2.png)

1. Bontsa ki a csatolt hello storage-fiókot, kattintson a jobb gombbal **Blob-tárolók**, kattintson a **létrehozás blob tároló**, típus `mycontainer`, és nyomja meg az enter.

    ![Hozzon létre egy tárolási üzenetsort.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-create-blob-container.png)

Most, hogy a blob-tároló, tesztelheti hello függvény egy fájltároló toohello feltöltésével.

## <a name="test-hello-function"></a>Hello függvény tesztelése

1. Vissza a hello Azure-portálon, a Tallózás tooyour függvény bontsa ki a hello **naplók** hello alján lévő hello lap, és győződjön meg arról, hogy a napló streaming nem szünetel.

1. A Storage Explorerben bontsa ki a tárfiókját, a **Blobtárolók** és a **saját tároló** elemet. Kattintson a **Feltöltés**, majd a **Fájlok feltöltése...** elemre.

    ![A fájl toohello blobtárolóban feltöltése.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-upload-file-blob.png)

1. A hello **fájlok feltöltése** párbeszédpanelen kattintson az hello **fájlok** mező. Keresse meg a helyi számítógépen, például a képfájl tooa fájl, válassza ki azt, és kattintson a **nyitott** , majd **feltöltése**.

1. Lépjen vissza tooyour függvény naplókat, és győződjön meg arról, hogy hello a blob beolvasása megtörtént.

   ![Hello naplókban üzenet megtekintése.](./media/functions-create-storage-blob-triggered-function/functions-blob-storage-trigger-view-logs.png)

    >[!NOTE]
    > A függvény alkalmazás futtatásakor hello alapértelmezett felhasználási tervben lehet tooseveral perc közötti blob folyamatban, vagy frissített hello és hello be késleltetést működéséhez indított alatt. Ha kis késleltetésre van szüksége a blob által aktivált függvényekhez, célszerű App Service-csomagban futtatnia a függvényalkalmazást.

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Következő lépések

Létrehozott egy függvényt, amely felvételekor futnak blob frissítése tooor a Blob Storage tárolóban. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

További információ a blobtároló eseményindítóiról: [Azure Functions – a blobtároló kötései](functions-bindings-storage-blob.md).
