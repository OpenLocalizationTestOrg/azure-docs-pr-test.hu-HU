---
title: "az Azure várólista-tároló és a Visual Studio kapcsolódó szolgáltatások (ASP.NET) lépései aaaGet |} Microsoft Docs"
description: "Hogyan tooget használatának az Azure várólista-tároló egy ASP.NET-projekt, a Visual Studio használó Visual Studio kapcsolódó szolgáltatások tooa tárfiókot kapcsolódás után"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 94ca3413-5497-433f-abbe-836f83a9de72
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/23/2016
ms.author: kraigb
ms.openlocfilehash: 415a437c4ce60b1e2e328f8e937c73b0d5c50e78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-queue-storage-and-visual-studio-connected-services-aspnet"></a>Ismerkedés az Azure várólista-tároló és a Visual Studio kapcsolódó szolgáltatások (ASP.NET)
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Áttekintés

Az Azure várólista-tároló alkalmazás-összetevők közötti üzenetküldési felhő biztosít. A méretezhető alkalmazások tervezésekor az alkalmazás összetevői gyakran le vannak választva, hogy egymástól függetlenül lehessen őket méretezni. A Queue storage biztosítja, hogy az alkalmazás-összetevők közötti kommunikáció aszinkron üzenetkezelési e hello felhőben, hello asztalon, egy helyszíni kiszolgálón vagy egy mobileszközön futnak. A Queue Storage támogatja az aszinkron feladatok kezelését és a feldolgozási munkafolyamatok kialakítását is.

Ez az oktatóanyag bemutatja, hogyan toowrite ASP.NET olyan gyakori forgatókönyveket tartalmaz Azure üzenetsor-kezelési tárolási entitások használata helykódja. Ilyen például a gyakori feladatokat, mint egy Azure-üzenetsorba, létrehozása és hozzáadása, módosítása, olvasása, és eltávolítása a üzenetsor-üzeneteket.

##<a name="prerequisites"></a>Előfeltételek

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure Storage-fiók](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a>Hozzon létre az MVC-vezérlő 

1. A hello **Megoldáskezelőben**, kattintson a jobb gombbal **tartományvezérlők**, és hello helyi menüből válassza ki a **Hozzáadás -> tartományvezérlő**.

    ![A vezérlő tooan ASP.NET MVC alkalmazás hozzáadása](./media/vs-storage-aspnet-getting-started-queues/add-controller-menu.png)

1. A hello **hozzáadása Scaffold** párbeszédablakban válassza **MVC 5 vezérlő - üres**, és válassza ki **Hozzáadás**.

    ![Adja meg az MVC-vezérlő típusa](./media/vs-storage-aspnet-getting-started-queues/add-controller.png)

1. A hello **vezérlő hozzáadása** párbeszédpanelen neve hello vezérlő *QueuesController*, és válassza ki **Hozzáadás**.

    ![Hello MVC-vezérlő neve](./media/vs-storage-aspnet-getting-started-queues/add-controller-name.png)

1. Adja hozzá a következő hello *használatával* irányelvek toohello `QueuesController.cs` fájlt:

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;
    ```
## <a name="create-a-queue"></a>Üzenetsor létrehozása

hello következő lépések bemutatják, hogyan toocreate várólista:

> [!NOTE]
> 
> Ez a szakasz azt feltételezi, hogy végrehajtotta hello [hello fejlesztési környezet beállítása](#set-up-the-development-environment). 

1. Nyissa meg hello `QueuesController.cs` fájlt. 

1. Adja hozzá a hívott metódus **CreateQueue** , amely visszaadja az **ActionResult**.

    ```csharp
    public ActionResult CreateQueue()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. Hello belül **CreateQueue** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli. Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Első egy **CloudQueueClient** objektum által jelképezett várólista szolgáltatás ügyfél.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```
1. Első egy **CloudQueue** egy toohello kívánt sor hivatkozásnév képviselő objektum. Hello **CloudQueueClient.GetQueueReference** metódus nem tesz egy kérelmet a queue storage. hello hivatkozás hello várólista létezik-e adja vissza. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Hello hívás **CloudQueue.CreateIfNotExists** metódus toocreate hello várólista, ha még nem létezik. Hello **CloudQueue.CreateIfNotExists** metódus beolvasása **igaz** Ha hello várólista nem létezik, és sikeresen létrejött. Ellenkező esetben **hamis** adja vissza.    

    ```csharp
    ViewBag.Success = queue.CreateIfNotExists();
    ```

1. Frissítés hello **ViewBag** hello nevű hello várólista.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```

1. A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **várólisták**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.

1. A hello **nézet hozzáadása** párbeszédpanelen adja meg **CreateQueue** hello nézet nevét, és válassza ki a **Hozzáadás**.

1. Nyissa meg `CreateQueue.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:

    ```csharp
    @{
        ViewBag.Title = "Create Queue";
    }
    
    <h2>Create Queue results</h2>

    Creation of @ViewBag.QueueName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.

1. Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Create queue", "CreateQueue", "Queues")</li>
    ```

1. Hello alkalmazás futtatásához, és válassza ki **létrehozás várólista** toosee eredménye a következő képernyőfelvétel hasonló toohello:
  
    ![Várólista létrehozása](./media/vs-storage-aspnet-getting-started-queues/create-queue-results.png)

    Ahogy korábban említettük hello **CloudQueue.CreateIfNotExists** metódus beolvasása **igaz** csak hello várólista nem létezik és jön létre. Ezért, ha futtatja a hello app hello várólista létezik, hello metódus visszaadja **hamis**. toorun hello alkalmazás többször, törölnie kell hello várólista hello app ismételt futtatása előtt. Törlése hello várólista megteheti a hello **CloudQueue.Delete** metódust. Hello sorból hello törölheti is [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040) vagy hello [Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md).  

## <a name="add-a-message-tooa-queue"></a>Egy üzenetsor tooa hozzáadása

Miután megismerte [egy sor](#create-a-queue), üzenetek toothat várólista is hozzáadhat. Ez a szakasz bemutatja, hogyan hozzáadása egy üzenetsor tooa *teszt-várólista*. 

> [!NOTE]
> 
> Ez a szakasz azt feltételezi, hogy végrehajtotta hello [hello fejlesztési környezet beállítása](#set-up-the-development-environment). 

1. Nyissa meg hello `QueuesController.cs` fájlt.

1. Adja hozzá a hívott metódus **AddMessage** , amely visszaadja az **ActionResult**.

    ```csharp
    public ActionResult AddMessage()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. Hello belül **AddMessage** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli. Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Első egy **CloudQueueClient** objektum által jelképezett várólista szolgáltatás ügyfél.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Első egy **CloudQueueContainer** objektum, amely egy hivatkozás toohello várólistájára vonatkozik. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Hozzon létre hello **CloudQueueMessage** tooadd toohello várólista kívánt üdvözlőüzenetére képviselő objektum. A **CloudQueueMessage** objektum vagy egy karakterláncból (UTF-8 formátumban), vagy egy bájttömböt hozható létre.

    ```csharp
    CloudQueueMessage message = new CloudQueueMessage("Hello, Azure Queue Storage");
    ```

1. Hello hívás **CloudQueue.AddMessage** metódus tooadd hello messaged toohello várólista.

    ```csharp
    queue.AddMessage(message);
    ```

1. Néhány létrehozásáról és **ViewBag** hello nézet tulajdonságai.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```

1. A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **várólisták**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.

1. A hello **nézet hozzáadása** párbeszédpanelen adja meg **AddMessage** hello nézet nevét, és válassza ki a **Hozzáadás**.

1. Nyissa meg `AddMessage.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:

    ```csharp
    @{
        ViewBag.Title = "Add Message";
    }
    
    <h2>Add Message results</h2>
    
    hello message '@ViewBag.Message' was added toohello queue '@ViewBag.QueueName'.
    ```

1. A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.

1. Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Add message", "AddMessage", "Queues")</li>
    ```

1. Hello alkalmazás futtatásához, és válassza ki **Hozzáadás üzenet** toosee eredménye a következő képernyőfelvétel hasonló toohello:
  
    ![Üzenet hozzáadása](./media/vs-storage-aspnet-getting-started-queues/add-message-results.png)

két szakasz - hello [üzenet olvasása az üzenetsorból eltávolítása nélkül](#read-a-message-from-a-queue-without-removing-it) és [olvasási és eltávolítása egy üzenetet az üzenetsorból](#read-and-remove-a-message-from-a-queue) -bemutatják, hogyan tooread üzenetek várólistából való várólistából.  

## <a name="read-a-message-from-a-queue-without-removing-it"></a>Egy üzenet olvasása az üzenetsorból eltávolítása nélkül

Ez a szakasz bemutatja, hogyan toopeek, egy sorban álló üzenet (olvasási hello első eltávolítása nélkül).  

> [!NOTE]
> 
> Ez a szakasz azt feltételezi, hogy végrehajtotta hello [hello fejlesztési környezet beállítása](#set-up-the-development-environment). 

1. Nyissa meg hello `QueuesController.cs` fájlt.

1. Adja hozzá a hívott metódus **PeekMessage** , amely visszaadja az **ActionResult**.

    ```csharp
    public ActionResult PeekMessage()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. Hello belül **PeekMessage** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli. Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Első egy **CloudQueueClient** objektum által jelképezett várólista szolgáltatás ügyfél.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Első egy **CloudQueueContainer** objektum, amely egy hivatkozás toohello várólistájára vonatkozik. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Hello hívás **CloudQueue.PeekMessage** metódus tooread hello várólista első üzenetébe hello hello várólistából eltávolítása nélkül. 

    ```csharp
    CloudQueueMessage message = queue.PeekMessage();
    ```

1. Frissítés hello **ViewBag** két értékekkel: hello várólistacímke és hello üzenet, amely lett beolvasva. Hello **CloudQueueMessage** vezérlőnek két tulajdonságainak hello objektum értékének lekérését: **CloudQueueMessage.AsBytes** és **CloudQueueMessage.AsString**. **AsString** (ebben a példában használt) egy karakterláncot ad vissza, miközben **AsBytes** adja vissza egy bájttömböt.

    ```csharp
    ViewBag.QueueName = queue.Name; 
    ViewBag.Message = (message != null ? message.AsString : "");
    ```

1. A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **várólisták**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.

1. A hello **nézet hozzáadása** párbeszédpanelen adja meg **PeekMessage** hello nézet nevét, és válassza ki a **Hozzáadás**.

1. Nyissa meg `PeekMessage.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:

    ```csharp
    @{
        ViewBag.Title = "PeekMessage";
    }
    
    <h2>Peek Message results</h2>
    
    <table border="1">
        <tr><th>Queue</th><th>Peeked Message</th></tr>
        <tr><td>@ViewBag.QueueName</td><td>@ViewBag.Message</td></tr>
    </table>    
    ```

1. A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.

1. Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Peek message", "PeekMessage", "Queues")</li>
    ```

1. Hello alkalmazás futtatásához, és válassza ki **betekintés üzenet** toosee eredménye a következő képernyőfelvétel hasonló toohello:
  
    ![Üzenet megtekintése](./media/vs-storage-aspnet-getting-started-queues/peek-message-results.png)

## <a name="read-and-remove-a-message-from-a-queue"></a>Olvassa el, és távolítsa el az üzenetet az üzenetsorból

Ebben a szakaszban megismerheti, hogyan tooread, és távolítsa el az üzenetet az üzenetsorból.   

> [!NOTE]
> 
> Ez a szakasz azt feltételezi, hogy végrehajtotta hello [hello fejlesztési környezet beállítása](#set-up-the-development-environment). 

1. Nyissa meg hello `QueuesController.cs` fájlt.

1. Adja hozzá a hívott metódus **ReadMessage** , amely visszaadja az **ActionResult**.

    ```csharp
    public ActionResult ReadMessage()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. Hello belül **ReadMessage** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli. Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Első egy **CloudQueueClient** objektum által jelképezett várólista szolgáltatás ügyfél.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Első egy **CloudQueueContainer** objektum, amely egy hivatkozás toohello várólistájára vonatkozik. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Hello hívás **CloudQueue.GetMessage** metódus tooread hello várólista első üzenetébe hello. Hello **CloudQueue.GetMessage** módszer teszi hello üzenet láthatatlan 30 másodperc (alapértelmezés) tooany az, hogy nincs más kód módosíthatja vagy törölheti köszönőüzenetei során a feldolgozás üzeneteket olvasó többi kód. toochange hello idő üdvözlőüzenetére mérete nem látható, előbb módosítsa a hello **visibilityTimeout** toohello a beadott paraméter **CloudQueue.GetMessage** metódust.

    ```csharp
    // This message will be invisible tooother code for 30 seconds.
    CloudQueueMessage message = queue.GetMessage();     
    ```

1. Hello hívás **CloudQueueMessage.Delete** metódus toodelete üdvözlőüzenetére hello üzenetsorból.

    ```csharp
    queue.DeleteMessage(message);
    ```

1. Frissítés hello **ViewBag** hello üzenet törölve, és hello hello várólista neve.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```
 
1. A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **várólisták**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.

1. A hello **nézet hozzáadása** párbeszédpanelen adja meg **ReadMessage** hello nézet nevét, és válassza ki a **Hozzáadás**.

1. Nyissa meg `ReadMessage.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:

    ```csharp
    @{
        ViewBag.Title = "ReadMessage";
    }
    
    <h2>Read Message results</h2>
    
    <table border="1">
        <tr><th>Queue</th><th>Read (and Deleted) Message</th></tr>
        <tr><td>@ViewBag.QueueName</td><td>@ViewBag.Message</td></tr>
    </table>
    ```

1. A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.

1. Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Read/Delete message", "ReadMessage", "Queues")</li>
    ```

1. Hello alkalmazás futtatásához, és válassza ki **olvasás/törlés üzenet** toosee eredménye a következő képernyőfelvétel hasonló toohello:
  
    ![Olvasási és törlési üzenet](./media/vs-storage-aspnet-getting-started-queues/read-message-results.png)

## <a name="get-hello-queue-length"></a>Első hello várólistájának hossza

Ez a szakasz bemutatja, hogyan tooget hello várólista hossza (üzenetek száma). 

> [!NOTE]
> 
> Ez a szakasz azt feltételezi, hogy végrehajtotta hello [hello fejlesztési környezet beállítása](#set-up-the-development-environment). 

1. Nyissa meg hello `QueuesController.cs` fájlt.

1. Adja hozzá a hívott metódus **GetQueueLength** , amely visszaadja az **ActionResult**.

    ```csharp
    public ActionResult GetQueueLength()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. Hello belül **ReadMessage** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli. Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Első egy **CloudQueueClient** objektum által jelképezett várólista szolgáltatás ügyfél.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Első egy **CloudQueueContainer** objektum, amely egy hivatkozás toohello várólistájára vonatkozik. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Hello hívás **CloudQueue.FetchAttributes** metódus tooretrieve hello várólista attribútumok (beleértve a hossza). 

    ```csharp
    queue.FetchAttributes();
    ```

6. Hozzáférés hello **CloudQueue.ApproximateMessageCount** tulajdonság tooget hello várólistájának hossza.
 
    ```csharp
    int? nMessages = queue.ApproximateMessageCount;
    ```

1. Frissítés hello **ViewBag** hello nevű hello várólista, és a hossza.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Length = nMessages;
    ```
 
1. A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **várólisták**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.

1. A hello **nézet hozzáadása** párbeszédpanelen adja meg **GetQueueLength** hello nézet nevét, és válassza ki a **Hozzáadás**.

1. Nyissa meg `GetQueueLengthMessage.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:

    ```csharp
    @{
        ViewBag.Title = "GetQueueLength";
    }
    
    <h2>Get Queue Length results</h2>
    
    hello queue '@ViewBag.QueueName' has a length of (number of messages): @ViewBag.Length
    ```

1. A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.

1. Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Get queue length", "GetQueueLength", "Queues")</li>
    ```

1. Hello alkalmazás futtatásához, és válassza ki **várólista hosszának lekérése** toosee eredménye a következő képernyőfelvétel hasonló toohello:
  
    ![Get-várólista hossza](./media/vs-storage-aspnet-getting-started-queues/get-queue-length-results.png)


## <a name="delete-a-queue"></a>Üzenetsor törlése
Ez a szakasz bemutatja, hogyan toodelete várólistát. 

> [!NOTE]
> 
> Ez a szakasz azt feltételezi, hogy végrehajtotta hello [hello fejlesztési környezet beállítása](#set-up-the-development-environment). 

1. Nyissa meg hello `QueuesController.cs` fájlt.

1. Adja hozzá a hívott metódus **DeleteQueue** , amely visszaadja az **ActionResult**.

    ```csharp
    public ActionResult DeleteQueue()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. Hello belül **DeleteQueue** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli. Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Első egy **CloudQueueClient** objektum által jelképezett várólista szolgáltatás ügyfél.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Első egy **CloudQueueContainer** objektum, amely egy hivatkozás toohello várólistájára vonatkozik. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Hello hívás **CloudQueue.Delete** metódus toodelete hello várólista hello által képviselt **CloudQueue** objektum.

    ```csharp
    queue.Delete();
    ```

1. Frissítés hello **ViewBag** hello nevű hello várólista, és a hossza.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```
 
1. A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **várólisták**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.

1. A hello **nézet hozzáadása** párbeszédpanelen adja meg **DeleteQueue** hello nézet nevét, és válassza ki a **Hozzáadás**.

1. Nyissa meg `DeleteQueue.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:

    ```csharp
    @{
        ViewBag.Title = "DeleteQueue";
    }
    
    <h2>Delete Queue results</h2>
    
    @ViewBag.QueueName deleted.
    ```

1. A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.

1. Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete queue", "DeleteQueue", "Queues")</li>
    ```

1. Hello alkalmazás futtatásához, és válassza ki **várólista hosszának lekérése** toosee eredménye a következő képernyőfelvétel hasonló toohello:
  
    ![Várólista törlése](./media/vs-storage-aspnet-getting-started-queues/delete-queue-results.png)

## <a name="next-steps"></a>Következő lépések
Azure-ban való adattárolás további lehetőségeiről további szolgáltatás útmutatók toolearn megtekintése.

  * [Ismerkedés az Azure blob storage és a Visual Studio kapcsolódó szolgáltatások (ASP.NET)](../storage/vs-storage-aspnet-getting-started-blobs.md)
  * [Ismerkedés az Azure table storage és a Visual Studio kapcsolódó szolgáltatások (ASP.NET)](vs-storage-aspnet-getting-started-tables.md)
