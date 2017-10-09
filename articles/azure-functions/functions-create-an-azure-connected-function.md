---
title: "egy függvény, amely a tooAzure szolgáltatások aaaCreate |} Microsoft Docs"
description: "Használja az Azure Functions toocreate egy kiszolgáló nélküli alkalmazást, amely a tooother Azure szolgáltatások."
services: functions
documentationcenter: dev-center-name
author: yochay
manager: manager-alias
editor: 
tags: 
keywords: "azure-függvények, függvények, eseményfeldolgozás, webhookok, dinamikus számítás, kiszolgáló nélküli architektúra"
ms.assetid: ab86065d-6050-46c9-a336-1bfc1fa4b5a1
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/01/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 9d1f7d3b236f8d2c1a404c76aee410f6d458fb7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-functions-toocreate-a-function-that-connects-tooother-azure-services"></a>Használja az Azure Functions toocreate egy függvénynek, amely a tooother Azure szolgáltatások

Ez a témakör bemutatja, hogyan toocreate egy függvényt, amely figyeli a egy Azure Storage várólista és másolatok hello toomessages Azure Functions üzenetek toorows egy Azure Storage táblában. Egy időzítő indított függvény használt tooload üzenetek hello várólistán. Egy második funkció hello várólistából beolvassa és üzenetek toohello tábla. Hello várólista és a hello tábla jön létre az Azure Functions hello kötés definíciók alapján. 

toomake dolgot ennél is érdekesebb megoldást, egy függvény JavaScript nyelven írt van, és más hello parancsfájl C# nyelven van megírva. Ez bemutatja, hogy egy függvényalkalmazás különféle nyelveken írt függvényekkel is rendelkezhet. 

Ezt a forgatókönyvet a [Channel 9 blog egyik videója](https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player) mutatja be.

## <a name="create-a-function-that-writes-toohello-queue"></a>Egy függvény írja az toohello várólista létrehozása

Tooa storage üzenetsorába csatlakozáskor toocreate hello üzenet-várólista betöltő függvényt kell. A JavaScript a funkció egy időzítő indítófeltételt toohello üzenet-várólista 10 másodpercenként írja. Ha még nem rendelkezik Azure-fiókra, tekintse meg a hello [próbálja az Azure Functions](https://functions.azure.com/try) tapasztal, vagy [az ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/free/).

1. Nyissa meg toohello Azure-portálon, és keresse meg a függvény alkalmazást.

2. Kattintson a **New Function** (Új függvény)  > **TimeTrigger - JavaScript** elemre. 

3. Hello függvény neve **FunctionsBindingsDemo1**, adja meg a cron-kifejezés érték `0/10 * * * * *` a **ütemezés**, és kattintson a **létrehozása**.
   
    ![Időzítő által aktivált hozzáadása](./media/functions-create-an-azure-connected-function/new-trigger-timer-function.png)

    Létrehozott egy időzítő által aktivált függvényt, amely 10 másodpercenként fut.

5. A hello **Develop** lapra, majd **naplók** és hello naplóban hello tevékenységének megtekintéséhez. 10 másodpercenként írt naplóbejegyzéseket láthat.
   
    ![Hello napló tooverify hello függvény works megtekintése](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-view-log.png)

## <a name="add-a-message-queue-output-binding"></a>Üzenetsor kimeneti kötésének hozzáadása

1. A hello **integráció** lapra, majd **új kimeneti** > **Azure Queue Storage** > **válasszon**.

    ![Eseményindító időzítő függvény hozzáadása](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab.png)

2. Adja meg `myQueueItem` a **üzenet paraméternév** és `functions-bindings` a **várólista neve**, válasszon ki egy létező **fiók tárolókapcsolat** , vagy kattintson a **új** toocreate tárolási fiók kapcsolat, és kattintson a **mentése**.  

    ![Hello kimeneti kötése toohello tárolási üzenetsor létrehozása](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab2.png)

1. Vissza a hello **Develop** lapon, a következő kód toohello függvény hello hozzáfűzése:
   
    ```javascript
   
    function myQueueItem() 
    {
        return {
            msg: "some message goes here",
            time: "time goes here"
        }
    }
   
    ```
2. Keresse meg a hello *Ha* utasítás körülbelül sor 9 hello függvény, és a Beszúrás hello következő kódot adott utasítás után.
   
    ```javascript
   
    var toBeQed = myQueueItem();
    toBeQed.time = timeStamp;
    context.bindings.myQueueItem = toBeQed;
   
    ```  
   
    Ez a kód létrehoz egy **myQueueItem** , és beállítja a **idő** tulajdonság toohello aktuális időbélyeg. Hozzáadja hello új várólista elem toohello környezet által **myQueueItem** kötés.

3. Kattintson a **Mentés és futtatás** gombra.

## <a name="view-storage-updates-by-using-storage-explorer"></a>A tárterület frissítéseinek áttekintése a Storage Explorerrel
A funkció működésének létrehozott hello várólistában lévő üzenetek megtekintésével ellenőrizheti.  Tooyour storage üzenetsorába Cloud Explorerben a Visual Studio használatával is elérheti. Azonban hello portál segítségével könnyen tooconnect tooyour storage-fiók használatával a Microsoft Azure Tártallózó.

1. A hello **integráció** lapra, majd a várólista kimeneti kötése > **dokumentáció**, majd a tárfiók kapcsolati karakterlánc hello felfedése és hello értéket másol. Az érték tooconnect tooyour tárfiókot használni.

    ![Az Azure Storage Explorer letöltése](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab3.png)


2. Ha még nem tette meg, töltse le és telepítse a [Microsoft Azure Storage Explorert](http://storageexplorer.com). 
 
3. A Tártallózó, kattintson a hello tooAzure Kiszolgálótárhely ikon csatlakozzon, illessze be a hello kapcsolati karakterláncot hello mezőben, és hello varázsló befejezéséhez.

    ![Storage Explorer – kapcsolat hozzáadása](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-storage-explorer.png)

4. A **helyi és csatlakoztatott**, bontsa ki a **Tárfiókok** > a tárfiók > **várólisták** > **funkciók-kötések**, és ellenőrizze, hogy üzenetek írt toohello várólista.

    ![Hello várólistában lévő üzenetek ábrázolása](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer.png)

    Ha hello várólista nem létezik, vagy üres, valószínűleg probléma van a függvénykötés vagy kóddal.

## <a name="create-a-function-that-reads-from-hello-queue"></a>Hello várólistából olvasó függvény létrehozása

Most, hogy rendelkezik a hozzáadni kívánt toohello várólista üzenetek, létrehozhat egy másik függvényen olvasó hello várólistából, és az írási műveletek hello üzenetek véglegesen tooan Azure Storage tábla.

1. Kattintson a **New Function** (Új függvény) > **QueueTrigger-CSharp** elemre. 
 
2. Hello függvény neve `FunctionsBindingsDemo2`, adja meg **funkciók-kötések** a hello **üzenetsornév** mezőben válasszon egy meglévő tárfiókot, vagy hozzon létre egyet, majd kattintson a **létrehozása**.

    ![Kimeneti üzenetsor időzítő függvényének hozzáadása](./media/functions-create-an-azure-connected-function/function-demo2-new-function.png) 

3. (Választható) Azt, hogy működik-e új függvény hello Tártallózó, mielőtt új várólista hello megtekintésével ellenőrizheti. A Visual Studio Cloud Explorer eszközét is használhatja.  

4. (Választható) Frissítse a hello **funkciók-kötések** várólistára, és figyelje meg, hogy a cikkek hello várólistából eltávolították. hello eltávolítását okozza, hogy hello függvény kötött toohello **funkciók-kötések** várólistára, egy bemeneti eseményindító és hello függvény hello várólista olvassa be. 
 
## <a name="add-a-table-output-binding"></a>Tábla kimeneti kötésének hozzáadása

1. A FunctionsBindingsDemo2 részen kattintson az **Integrate** (Integrálás) > **New Output** (Új kimenet) > **Azure Table Storage** > **Select** (Kiválasztás) elemre.

    ![A kötés tooan Azure Storage tábla hozzáadása](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab.png) 

2. Adja meg a `functionbindings` értéket a **Table name** (Tábla neve), illetve a `myTable` értéket a **Table parameter name** (Táblaparaméter neve) beállítás számára, válasszon ki egy meglévő kapcsolatot a **Storage account connection** (Tárfiókkapcsolat) mezőben, vagy hozzon létre egy új kapcsolatot, majd kattintson a **Save** (Mentés) gombra.

    ![Hello tárolási táblakötéssel konfigurálása](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab2.png)
   
3. A hello **Develop** lapon, hogy lecseréli a meglévő funkciókódot hello hello következőre:
   
    ```cs
    
    using System;
    
    public static void Run(QItem myQueueItem, ICollector<TableItem> myTable, TraceWriter log)
    {    
        TableItem myItem = new TableItem
        {
            PartitionKey = "key",
            RowKey = Guid.NewGuid().ToString(),
            Time = DateTime.Now.ToString("hh.mm.ss.ffffff"),
            Msg = myQueueItem.Msg,
            OriginalTime = myQueueItem.Time    
        };
        
        // Add hello item toohello table binding collection.
        myTable.Add(myItem);
    
        log.Verbose($"C# Queue trigger function processed: {myItem.RowKey} | {myItem.Msg} | {myItem.Time}");
    }
    
    public class TableItem
    {
        public string PartitionKey {get; set;}
        public string RowKey {get; set;}
        public string Time {get; set;}
        public string Msg {get; set;}
        public string OriginalTime {get; set;}
    }
    
    public class QItem
    {
        public string Msg { get; set;}
        public string Time { get; set;}
    }
    ```
    Hello **TableItem** osztály hello tárolási tábla sorának jelöli, és hozzáadhat hello elem toohello `myTable` gyűjteménye **TableItem** objektumok. Meg kell adni a hello **PartitionKey** és **RowKey** tulajdonságok toobe képes tooinsert hello táblába.

4. Kattintson a **Save** (Mentés) gombra.  Végezetül hello függvény works ellenőrizheti Tártallózóval vagy a Visual Studio Cloud Explorer hello tábla megtekintésével.

5. (Választható) Bontsa ki a tárfiók a Tártallózó **táblák** > **functionsbindings** , és ellenőrizze, hogy a sorok hozzáadásakor toohello tábla. Mindent hello ugyanaz a Cloud Explorerben a Visual Studióban.

    ![Hello tábla sorainak ábrázolása](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer2.png)

    Ha hello tábla nem létezik, vagy üres, valószínűleg probléma van a függvénykötés vagy kóddal. 
 
[!INCLUDE [More binding information](../../includes/functions-bindings-next-steps.md)]

## <a name="next-steps"></a>Következő lépések
Az Azure Functions kapcsolatos további információkért tekintse meg a következő témakörök hello:

* [Az Azure Functions fejlesztői segédanyagai](functions-reference.md)  
  Programozói segédanyagok függvények kódolásához, valamint eseményindítók és kötések meghatározásához.
* [Az Azure Functions tesztelése](functions-test-a-function.md)  
  Különböző függvénytesztelési eszközöket és technikákat ír le.
* [Hogyan tooscale Azure Functions](functions-scale.md)  
  Ismerteti az Azure Functions, beleértve a hello fogyasztás üzemeltetési terv, és hogyan toochoose hello megfelelő csomag elérhető service-csomagokról. 

[!INCLUDE [Getting help note](../../includes/functions-get-help.md)]

