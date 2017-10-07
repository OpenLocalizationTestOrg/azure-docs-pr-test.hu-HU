---
title: "az Azure várólista-tároló és a Visual Studio kapcsolódó szolgáltatások (ASP.NET) lépései aaaGet |} Microsoft Docs"
description: "Hogyan tooget használatának az Azure várólista-tároló egy ASP.NET-projekt, a Visual Studio használó Visual Studio kapcsolódó szolgáltatások tooa tárfiókot kapcsolódás után"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 94ca3413-5497-433f-abbe-836f83a9de72
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/23/2016
ms.author: tarcher
ms.openlocfilehash: a9d6ecb1e8d61d75f59658d0ea3fa63d26fd7354
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-queue-storage-and-visual-studio-connected-services-aspnet"></a><span data-ttu-id="3d556-103">Ismerkedés az Azure várólista-tároló és a Visual Studio kapcsolódó szolgáltatások (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="3d556-103">Get started with Azure queue storage and Visual Studio Connected Services (ASP.NET)</span></span>
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="3d556-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="3d556-104">Overview</span></span>

<span data-ttu-id="3d556-105">Az Azure várólista-tároló alkalmazás-összetevők közötti üzenetküldési felhő biztosít.</span><span class="sxs-lookup"><span data-stu-id="3d556-105">Azure queue storage provides cloud messaging between application components.</span></span> <span data-ttu-id="3d556-106">A méretezhető alkalmazások tervezésekor az alkalmazás összetevői gyakran le vannak választva, hogy egymástól függetlenül lehessen őket méretezni.</span><span class="sxs-lookup"><span data-stu-id="3d556-106">In designing applications for scale, application components are often decoupled, so that they can scale independently.</span></span> <span data-ttu-id="3d556-107">A Queue storage biztosítja, hogy az alkalmazás-összetevők közötti kommunikáció aszinkron üzenetkezelési e hello felhőben, hello asztalon, egy helyszíni kiszolgálón vagy egy mobileszközön futnak.</span><span class="sxs-lookup"><span data-stu-id="3d556-107">Queue storage delivers asynchronous messaging for communication between application components, whether they are running in hello cloud, on hello desktop, on an on-premises server, or on a mobile device.</span></span> <span data-ttu-id="3d556-108">A Queue Storage támogatja az aszinkron feladatok kezelését és a feldolgozási munkafolyamatok kialakítását is.</span><span class="sxs-lookup"><span data-stu-id="3d556-108">Queue storage also supports managing asynchronous tasks and building process work flows.</span></span>

<span data-ttu-id="3d556-109">Ez az oktatóanyag bemutatja, hogyan toowrite ASP.NET olyan gyakori forgatókönyveket tartalmaz Azure üzenetsor-kezelési tárolási entitások használata helykódja.</span><span class="sxs-lookup"><span data-stu-id="3d556-109">This tutorial shows how toowrite ASP.NET code for some common scenarios using Azure queue storage entities.</span></span> <span data-ttu-id="3d556-110">Ilyen például a gyakori feladatokat, mint egy Azure-üzenetsorba, létrehozása és hozzáadása, módosítása, olvasása, és eltávolítása a üzenetsor-üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="3d556-110">These scenarios include common tasks such as creating an Azure queue, and adding, modifying, reading, and removing queue messages.</span></span>

##<a name="prerequisites"></a><span data-ttu-id="3d556-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3d556-111">Prerequisites</span></span>

* [<span data-ttu-id="3d556-112">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3d556-112">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="3d556-113">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="3d556-113">Azure storage account</span></span>](storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a><span data-ttu-id="3d556-114">Hozzon létre az MVC-vezérlő</span><span class="sxs-lookup"><span data-stu-id="3d556-114">Create an MVC controller</span></span> 

1. <span data-ttu-id="3d556-115">A hello **Megoldáskezelőben**, kattintson a jobb gombbal **tartományvezérlők**, és hello helyi menüből válassza ki a **Hozzáadás -> tartományvezérlő**.</span><span class="sxs-lookup"><span data-stu-id="3d556-115">In hello **Solution Explorer**, right-click **Controllers**, and, from hello context menu, select **Add->Controller**.</span></span>

    ![A vezérlő tooan ASP.NET MVC alkalmazás hozzáadása](./media/vs-storage-aspnet-getting-started-queues/add-controller-menu.png)

1. <span data-ttu-id="3d556-117">A hello **hozzáadása Scaffold** párbeszédablakban válassza **MVC 5 vezérlő - üres**, és válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="3d556-117">On hello **Add Scaffold** dialog, select **MVC 5 Controller - Empty**, and select **Add**.</span></span>

    ![Adja meg az MVC-vezérlő típusa](./media/vs-storage-aspnet-getting-started-queues/add-controller.png)

1. <span data-ttu-id="3d556-119">A hello **vezérlő hozzáadása** párbeszédpanelen neve hello vezérlő *QueuesController*, és válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="3d556-119">On hello **Add Controller** dialog, name hello controller *QueuesController*, and select **Add**.</span></span>

    ![Hello MVC-vezérlő neve](./media/vs-storage-aspnet-getting-started-queues/add-controller-name.png)

1. <span data-ttu-id="3d556-121">Adja hozzá a következő hello *használatával* irányelvek toohello `QueuesController.cs` fájlt:</span><span class="sxs-lookup"><span data-stu-id="3d556-121">Add hello following *using* directives toohello `QueuesController.cs` file:</span></span>

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;
    ```
## <a name="create-a-queue"></a><span data-ttu-id="3d556-122">Üzenetsor létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d556-122">Create a queue</span></span>

<span data-ttu-id="3d556-123">hello következő lépések bemutatják, hogyan toocreate várólista:</span><span class="sxs-lookup"><span data-stu-id="3d556-123">hello following steps illustrate how toocreate a queue:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="3d556-124">Ez a szakasz azt feltételezi, hogy végrehajtotta hello [hello fejlesztési környezet beállítása](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="3d556-124">This section assumes you have completed hello steps [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="3d556-125">Nyissa meg hello `QueuesController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="3d556-125">Open hello `QueuesController.cs` file.</span></span> 

1. <span data-ttu-id="3d556-126">Adja hozzá a hívott metódus **CreateQueue** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="3d556-126">Add a method called **CreateQueue** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult CreateQueue()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="3d556-127">Hello belül **CreateQueue** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="3d556-127">Within hello **CreateQueue** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="3d556-128">Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)</span><span class="sxs-lookup"><span data-stu-id="3d556-128">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="3d556-129">Első egy **CloudQueueClient** objektum által jelképezett várólista szolgáltatás ügyfél.</span><span class="sxs-lookup"><span data-stu-id="3d556-129">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```
1. <span data-ttu-id="3d556-130">Első egy **CloudQueue** egy toohello kívánt sor hivatkozásnév képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="3d556-130">Get a **CloudQueue** object that represents a reference toohello desired queue name.</span></span> <span data-ttu-id="3d556-131">Hello **CloudQueueClient.GetQueueReference** metódus nem tesz egy kérelmet a queue storage.</span><span class="sxs-lookup"><span data-stu-id="3d556-131">hello **CloudQueueClient.GetQueueReference** method does not make a request against queue storage.</span></span> <span data-ttu-id="3d556-132">hello hivatkozás hello várólista létezik-e adja vissza.</span><span class="sxs-lookup"><span data-stu-id="3d556-132">hello reference is returned whether or not hello queue exists.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="3d556-133">Hello hívás **CloudQueue.CreateIfNotExists** metódus toocreate hello várólista, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="3d556-133">Call hello **CloudQueue.CreateIfNotExists** method toocreate hello queue if it does not yet exist.</span></span> <span data-ttu-id="3d556-134">Hello **CloudQueue.CreateIfNotExists** metódus beolvasása **igaz** Ha hello várólista nem létezik, és sikeresen létrejött.</span><span class="sxs-lookup"><span data-stu-id="3d556-134">hello **CloudQueue.CreateIfNotExists** method returns **true** if hello queue does not exist, and is successfully created.</span></span> <span data-ttu-id="3d556-135">Ellenkező esetben **hamis** adja vissza.</span><span class="sxs-lookup"><span data-stu-id="3d556-135">Otherwise, **false** is returned.</span></span>    

    ```csharp
    ViewBag.Success = queue.CreateIfNotExists();
    ```

1. <span data-ttu-id="3d556-136">Frissítés hello **ViewBag** hello nevű hello várólista.</span><span class="sxs-lookup"><span data-stu-id="3d556-136">Update hello **ViewBag** with hello name of hello queue.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```

1. <span data-ttu-id="3d556-137">A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **várólisták**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="3d556-137">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Queues**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="3d556-138">A hello **nézet hozzáadása** párbeszédpanelen adja meg **CreateQueue** hello nézet nevét, és válassza ki a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="3d556-138">On hello **Add View** dialog, enter **CreateQueue** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="3d556-139">Nyissa meg `CreateQueue.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:</span><span class="sxs-lookup"><span data-stu-id="3d556-139">Open `CreateQueue.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Create Queue";
    }
    
    <h2>Create Queue results</h2>

    Creation of @ViewBag.QueueName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. <span data-ttu-id="3d556-140">A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="3d556-140">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="3d556-141">Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="3d556-141">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Create queue", "CreateQueue", "Queues")</li>
    ```

1. <span data-ttu-id="3d556-142">Hello alkalmazás futtatásához, és válassza ki **létrehozás várólista** toosee eredménye a következő képernyőfelvétel hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="3d556-142">Run hello application, and select **Create queue** toosee results similar toohello following screen shot:</span></span>
  
    ![Várólista létrehozása](./media/vs-storage-aspnet-getting-started-queues/create-queue-results.png)

    <span data-ttu-id="3d556-144">Ahogy korábban említettük hello **CloudQueue.CreateIfNotExists** metódus beolvasása **igaz** csak hello várólista nem létezik és jön létre.</span><span class="sxs-lookup"><span data-stu-id="3d556-144">As mentioned previously, hello **CloudQueue.CreateIfNotExists** method returns **true** only when hello queue doesn't exist and is created.</span></span> <span data-ttu-id="3d556-145">Ezért, ha futtatja a hello app hello várólista létezik, hello metódus visszaadja **hamis**.</span><span class="sxs-lookup"><span data-stu-id="3d556-145">Therefore, if you run hello app when hello queue exists, hello method returns **false**.</span></span> <span data-ttu-id="3d556-146">toorun hello alkalmazás többször, törölnie kell hello várólista hello app ismételt futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="3d556-146">toorun hello app multiple times, you must delete hello queue before running hello app again.</span></span> <span data-ttu-id="3d556-147">Törlése hello várólista megteheti a hello **CloudQueue.Delete** metódust.</span><span class="sxs-lookup"><span data-stu-id="3d556-147">Deleting hello queue can be done via hello **CloudQueue.Delete** method.</span></span> <span data-ttu-id="3d556-148">Hello sorból hello törölheti is [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040) vagy hello [Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="3d556-148">You can also delete hello queue using hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) or hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>  

## <a name="add-a-message-tooa-queue"></a><span data-ttu-id="3d556-149">Egy üzenetsor tooa hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3d556-149">Add a message tooa queue</span></span>

<span data-ttu-id="3d556-150">Miután megismerte [egy sor](#create-a-queue), üzenetek toothat várólista is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="3d556-150">Once you've [created a queue](#create-a-queue), you can add messages toothat queue.</span></span> <span data-ttu-id="3d556-151">Ez a szakasz bemutatja, hogyan hozzáadása egy üzenetsor tooa *teszt-várólista*.</span><span class="sxs-lookup"><span data-stu-id="3d556-151">This section walks you through adding a message tooa queue *test-queue*.</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="3d556-152">Ez a szakasz azt feltételezi, hogy végrehajtotta hello [hello fejlesztési környezet beállítása](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="3d556-152">This section assumes you have completed hello steps [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="3d556-153">Nyissa meg hello `QueuesController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="3d556-153">Open hello `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="3d556-154">Adja hozzá a hívott metódus **AddMessage** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="3d556-154">Add a method called **AddMessage** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult AddMessage()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="3d556-155">Hello belül **AddMessage** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="3d556-155">Within hello **AddMessage** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="3d556-156">Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)</span><span class="sxs-lookup"><span data-stu-id="3d556-156">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="3d556-157">Első egy **CloudQueueClient** objektum által jelképezett várólista szolgáltatás ügyfél.</span><span class="sxs-lookup"><span data-stu-id="3d556-157">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="3d556-158">Első egy **CloudQueueContainer** objektum, amely egy hivatkozás toohello várólistájára vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="3d556-158">Get a **CloudQueueContainer** object that represents a reference toohello queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="3d556-159">Hozzon létre hello **CloudQueueMessage** tooadd toohello várólista kívánt üdvözlőüzenetére képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="3d556-159">Create hello **CloudQueueMessage** object representing hello message you want tooadd toohello queue.</span></span> <span data-ttu-id="3d556-160">A **CloudQueueMessage** objektum vagy egy karakterláncból (UTF-8 formátumban), vagy egy bájttömböt hozható létre.</span><span class="sxs-lookup"><span data-stu-id="3d556-160">A **CloudQueueMessage** object can be created from either a string (in UTF-8 format) or a byte array.</span></span>

    ```csharp
    CloudQueueMessage message = new CloudQueueMessage("Hello, Azure Queue Storage");
    ```

1. <span data-ttu-id="3d556-161">Hello hívás **CloudQueue.AddMessage** metódus tooadd hello messaged toohello várólista.</span><span class="sxs-lookup"><span data-stu-id="3d556-161">Call hello **CloudQueue.AddMessage** method tooadd hello messaged toohello queue.</span></span>

    ```csharp
    queue.AddMessage(message);
    ```

1. <span data-ttu-id="3d556-162">Néhány létrehozásáról és **ViewBag** hello nézet tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="3d556-162">Create and set a couple of **ViewBag** properties for display in hello view.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```

1. <span data-ttu-id="3d556-163">A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **várólisták**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="3d556-163">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Queues**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="3d556-164">A hello **nézet hozzáadása** párbeszédpanelen adja meg **AddMessage** hello nézet nevét, és válassza ki a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="3d556-164">On hello **Add View** dialog, enter **AddMessage** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="3d556-165">Nyissa meg `AddMessage.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:</span><span class="sxs-lookup"><span data-stu-id="3d556-165">Open `AddMessage.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Add Message";
    }
    
    <h2>Add Message results</h2>
    
    hello message '@ViewBag.Message' was added toohello queue '@ViewBag.QueueName'.
    ```

1. <span data-ttu-id="3d556-166">A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="3d556-166">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="3d556-167">Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="3d556-167">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Add message", "AddMessage", "Queues")</li>
    ```

1. <span data-ttu-id="3d556-168">Hello alkalmazás futtatásához, és válassza ki **Hozzáadás üzenet** toosee eredménye a következő képernyőfelvétel hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="3d556-168">Run hello application, and select **Add message** toosee results similar toohello following screen shot:</span></span>
  
    ![Üzenet hozzáadása](./media/vs-storage-aspnet-getting-started-queues/add-message-results.png)

<span data-ttu-id="3d556-170">két szakasz - hello [üzenet olvasása az üzenetsorból eltávolítása nélkül](#read-a-message-from-a-queue-without-removing-it) és [olvasási és eltávolítása egy üzenetet az üzenetsorból](#read-and-remove-a-message-from-a-queue) -bemutatják, hogyan tooread üzenetek várólistából való várólistából.</span><span class="sxs-lookup"><span data-stu-id="3d556-170">hello two sections - [Read a message from a queue without removing it](#read-a-message-from-a-queue-without-removing-it) and [Read and remove a message from a queue](#read-and-remove-a-message-from-a-queue) - illustrate how tooread messages from a queue.</span></span>  

## <a name="read-a-message-from-a-queue-without-removing-it"></a><span data-ttu-id="3d556-171">Egy üzenet olvasása az üzenetsorból eltávolítása nélkül</span><span class="sxs-lookup"><span data-stu-id="3d556-171">Read a message from a queue without removing it</span></span>

<span data-ttu-id="3d556-172">Ez a szakasz bemutatja, hogyan toopeek, egy sorban álló üzenet (olvasási hello első eltávolítása nélkül).</span><span class="sxs-lookup"><span data-stu-id="3d556-172">This section illustrates how toopeek at a queued message (read hello first message without removing it).</span></span>  

> [!NOTE]
> 
> <span data-ttu-id="3d556-173">Ez a szakasz azt feltételezi, hogy végrehajtotta hello [hello fejlesztési környezet beállítása](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="3d556-173">This section assumes you have completed hello steps [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="3d556-174">Nyissa meg hello `QueuesController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="3d556-174">Open hello `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="3d556-175">Adja hozzá a hívott metódus **PeekMessage** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="3d556-175">Add a method called **PeekMessage** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult PeekMessage()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="3d556-176">Hello belül **PeekMessage** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="3d556-176">Within hello **PeekMessage** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="3d556-177">Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)</span><span class="sxs-lookup"><span data-stu-id="3d556-177">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="3d556-178">Első egy **CloudQueueClient** objektum által jelképezett várólista szolgáltatás ügyfél.</span><span class="sxs-lookup"><span data-stu-id="3d556-178">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="3d556-179">Első egy **CloudQueueContainer** objektum, amely egy hivatkozás toohello várólistájára vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="3d556-179">Get a **CloudQueueContainer** object that represents a reference toohello queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="3d556-180">Hello hívás **CloudQueue.PeekMessage** metódus tooread hello várólista első üzenetébe hello hello várólistából eltávolítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="3d556-180">Call hello **CloudQueue.PeekMessage** method tooread hello first message in hello queue without removing it from hello queue.</span></span> 

    ```csharp
    CloudQueueMessage message = queue.PeekMessage();
    ```

1. <span data-ttu-id="3d556-181">Frissítés hello **ViewBag** két értékekkel: hello várólistacímke és hello üzenet, amely lett beolvasva.</span><span class="sxs-lookup"><span data-stu-id="3d556-181">Update hello **ViewBag** with two values: hello queue name and hello message that was read.</span></span> <span data-ttu-id="3d556-182">Hello **CloudQueueMessage** vezérlőnek két tulajdonságainak hello objektum értékének lekérését: **CloudQueueMessage.AsBytes** és **CloudQueueMessage.AsString**.</span><span class="sxs-lookup"><span data-stu-id="3d556-182">hello **CloudQueueMessage** object exposes two properties for getting hello object's value: **CloudQueueMessage.AsBytes** and **CloudQueueMessage.AsString**.</span></span> <span data-ttu-id="3d556-183">**AsString** (ebben a példában használt) egy karakterláncot ad vissza, miközben **AsBytes** adja vissza egy bájttömböt.</span><span class="sxs-lookup"><span data-stu-id="3d556-183">**AsString** (used in this example) returns a string, while **AsBytes** returns a byte array.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name; 
    ViewBag.Message = (message != null ? message.AsString : "");
    ```

1. <span data-ttu-id="3d556-184">A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **várólisták**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="3d556-184">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Queues**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="3d556-185">A hello **nézet hozzáadása** párbeszédpanelen adja meg **PeekMessage** hello nézet nevét, és válassza ki a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="3d556-185">On hello **Add View** dialog, enter **PeekMessage** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="3d556-186">Nyissa meg `PeekMessage.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:</span><span class="sxs-lookup"><span data-stu-id="3d556-186">Open `PeekMessage.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

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

1. <span data-ttu-id="3d556-187">A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="3d556-187">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="3d556-188">Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="3d556-188">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Peek message", "PeekMessage", "Queues")</li>
    ```

1. <span data-ttu-id="3d556-189">Hello alkalmazás futtatásához, és válassza ki **betekintés üzenet** toosee eredménye a következő képernyőfelvétel hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="3d556-189">Run hello application, and select **Peek message** toosee results similar toohello following screen shot:</span></span>
  
    ![Üzenet megtekintése](./media/vs-storage-aspnet-getting-started-queues/peek-message-results.png)

## <a name="read-and-remove-a-message-from-a-queue"></a><span data-ttu-id="3d556-191">Olvassa el, és távolítsa el az üzenetet az üzenetsorból</span><span class="sxs-lookup"><span data-stu-id="3d556-191">Read and remove a message from a queue</span></span>

<span data-ttu-id="3d556-192">Ebben a szakaszban megismerheti, hogyan tooread, és távolítsa el az üzenetet az üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="3d556-192">In this section, you learn how tooread and remove a message from a queue.</span></span>   

> [!NOTE]
> 
> <span data-ttu-id="3d556-193">Ez a szakasz azt feltételezi, hogy végrehajtotta hello [hello fejlesztési környezet beállítása](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="3d556-193">This section assumes you have completed hello steps [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="3d556-194">Nyissa meg hello `QueuesController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="3d556-194">Open hello `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="3d556-195">Adja hozzá a hívott metódus **ReadMessage** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="3d556-195">Add a method called **ReadMessage** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult ReadMessage()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="3d556-196">Hello belül **ReadMessage** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="3d556-196">Within hello **ReadMessage** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="3d556-197">Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)</span><span class="sxs-lookup"><span data-stu-id="3d556-197">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="3d556-198">Első egy **CloudQueueClient** objektum által jelképezett várólista szolgáltatás ügyfél.</span><span class="sxs-lookup"><span data-stu-id="3d556-198">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="3d556-199">Első egy **CloudQueueContainer** objektum, amely egy hivatkozás toohello várólistájára vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="3d556-199">Get a **CloudQueueContainer** object that represents a reference toohello queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="3d556-200">Hello hívás **CloudQueue.GetMessage** metódus tooread hello várólista első üzenetébe hello.</span><span class="sxs-lookup"><span data-stu-id="3d556-200">Call hello **CloudQueue.GetMessage** method tooread hello first message in hello queue.</span></span> <span data-ttu-id="3d556-201">Hello **CloudQueue.GetMessage** módszer teszi hello üzenet láthatatlan 30 másodperc (alapértelmezés) tooany az, hogy nincs más kód módosíthatja vagy törölheti köszönőüzenetei során a feldolgozás üzeneteket olvasó többi kód.</span><span class="sxs-lookup"><span data-stu-id="3d556-201">hello **CloudQueue.GetMessage** method makes hello message invisible for 30 seconds (by default) tooany other code reading messages so that no other code can modify or delete hello message while your processing it.</span></span> <span data-ttu-id="3d556-202">toochange hello idő üdvözlőüzenetére mérete nem látható, előbb módosítsa a hello **visibilityTimeout** toohello a beadott paraméter **CloudQueue.GetMessage** metódust.</span><span class="sxs-lookup"><span data-stu-id="3d556-202">toochange hello amount of time hello message is invisible, modify hello **visibilityTimeout** parameter being passed toohello **CloudQueue.GetMessage** method.</span></span>

    ```csharp
    // This message will be invisible tooother code for 30 seconds.
    CloudQueueMessage message = queue.GetMessage();     
    ```

1. <span data-ttu-id="3d556-203">Hello hívás **CloudQueueMessage.Delete** metódus toodelete üdvözlőüzenetére hello üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="3d556-203">Call hello **CloudQueueMessage.Delete** method toodelete hello message from hello queue.</span></span>

    ```csharp
    queue.DeleteMessage(message);
    ```

1. <span data-ttu-id="3d556-204">Frissítés hello **ViewBag** hello üzenet törölve, és hello hello várólista neve.</span><span class="sxs-lookup"><span data-stu-id="3d556-204">Update hello **ViewBag** with hello message deleted, and hello name of hello queue.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```
 
1. <span data-ttu-id="3d556-205">A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **várólisták**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="3d556-205">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Queues**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="3d556-206">A hello **nézet hozzáadása** párbeszédpanelen adja meg **ReadMessage** hello nézet nevét, és válassza ki a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="3d556-206">On hello **Add View** dialog, enter **ReadMessage** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="3d556-207">Nyissa meg `ReadMessage.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:</span><span class="sxs-lookup"><span data-stu-id="3d556-207">Open `ReadMessage.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

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

1. <span data-ttu-id="3d556-208">A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="3d556-208">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="3d556-209">Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="3d556-209">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Read/Delete message", "ReadMessage", "Queues")</li>
    ```

1. <span data-ttu-id="3d556-210">Hello alkalmazás futtatásához, és válassza ki **olvasás/törlés üzenet** toosee eredménye a következő képernyőfelvétel hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="3d556-210">Run hello application, and select **Read/Delete message** toosee results similar toohello following screen shot:</span></span>
  
    ![Olvasási és törlési üzenet](./media/vs-storage-aspnet-getting-started-queues/read-message-results.png)

## <a name="get-hello-queue-length"></a><span data-ttu-id="3d556-212">Első hello várólistájának hossza</span><span class="sxs-lookup"><span data-stu-id="3d556-212">Get hello queue length</span></span>

<span data-ttu-id="3d556-213">Ez a szakasz bemutatja, hogyan tooget hello várólista hossza (üzenetek száma).</span><span class="sxs-lookup"><span data-stu-id="3d556-213">This section illustrates how tooget hello queue length (number of messages).</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="3d556-214">Ez a szakasz azt feltételezi, hogy végrehajtotta hello [hello fejlesztési környezet beállítása](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="3d556-214">This section assumes you have completed hello steps [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="3d556-215">Nyissa meg hello `QueuesController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="3d556-215">Open hello `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="3d556-216">Adja hozzá a hívott metódus **GetQueueLength** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="3d556-216">Add a method called **GetQueueLength** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult GetQueueLength()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="3d556-217">Hello belül **ReadMessage** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="3d556-217">Within hello **ReadMessage** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="3d556-218">Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)</span><span class="sxs-lookup"><span data-stu-id="3d556-218">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="3d556-219">Első egy **CloudQueueClient** objektum által jelképezett várólista szolgáltatás ügyfél.</span><span class="sxs-lookup"><span data-stu-id="3d556-219">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="3d556-220">Első egy **CloudQueueContainer** objektum, amely egy hivatkozás toohello várólistájára vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="3d556-220">Get a **CloudQueueContainer** object that represents a reference toohello queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="3d556-221">Hello hívás **CloudQueue.FetchAttributes** metódus tooretrieve hello várólista attribútumok (beleértve a hossza).</span><span class="sxs-lookup"><span data-stu-id="3d556-221">Call hello **CloudQueue.FetchAttributes** method tooretrieve hello queue's attributes (including its length).</span></span> 

    ```csharp
    queue.FetchAttributes();
    ```

6. <span data-ttu-id="3d556-222">Hozzáférés hello **CloudQueue.ApproximateMessageCount** tulajdonság tooget hello várólistájának hossza.</span><span class="sxs-lookup"><span data-stu-id="3d556-222">Access hello **CloudQueue.ApproximateMessageCount** property tooget hello queue's length.</span></span>
 
    ```csharp
    int? nMessages = queue.ApproximateMessageCount;
    ```

1. <span data-ttu-id="3d556-223">Frissítés hello **ViewBag** hello nevű hello várólista, és a hossza.</span><span class="sxs-lookup"><span data-stu-id="3d556-223">Update hello **ViewBag** with hello name of hello queue, and its length.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Length = nMessages;
    ```
 
1. <span data-ttu-id="3d556-224">A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **várólisták**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="3d556-224">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Queues**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="3d556-225">A hello **nézet hozzáadása** párbeszédpanelen adja meg **GetQueueLength** hello nézet nevét, és válassza ki a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="3d556-225">On hello **Add View** dialog, enter **GetQueueLength** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="3d556-226">Nyissa meg `GetQueueLengthMessage.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:</span><span class="sxs-lookup"><span data-stu-id="3d556-226">Open `GetQueueLengthMessage.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "GetQueueLength";
    }
    
    <h2>Get Queue Length results</h2>
    
    hello queue '@ViewBag.QueueName' has a length of (number of messages): @ViewBag.Length
    ```

1. <span data-ttu-id="3d556-227">A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="3d556-227">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="3d556-228">Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="3d556-228">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Get queue length", "GetQueueLength", "Queues")</li>
    ```

1. <span data-ttu-id="3d556-229">Hello alkalmazás futtatásához, és válassza ki **várólista hosszának lekérése** toosee eredménye a következő képernyőfelvétel hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="3d556-229">Run hello application, and select **Get queue length** toosee results similar toohello following screen shot:</span></span>
  
    ![Get-várólista hossza](./media/vs-storage-aspnet-getting-started-queues/get-queue-length-results.png)


## <a name="delete-a-queue"></a><span data-ttu-id="3d556-231">Üzenetsor törlése</span><span class="sxs-lookup"><span data-stu-id="3d556-231">Delete a queue</span></span>
<span data-ttu-id="3d556-232">Ez a szakasz bemutatja, hogyan toodelete várólistát.</span><span class="sxs-lookup"><span data-stu-id="3d556-232">This section illustrates how toodelete a queue.</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="3d556-233">Ez a szakasz azt feltételezi, hogy végrehajtotta hello [hello fejlesztési környezet beállítása](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="3d556-233">This section assumes you have completed hello steps [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="3d556-234">Nyissa meg hello `QueuesController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="3d556-234">Open hello `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="3d556-235">Adja hozzá a hívott metódus **DeleteQueue** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="3d556-235">Add a method called **DeleteQueue** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult DeleteQueue()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="3d556-236">Hello belül **DeleteQueue** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="3d556-236">Within hello **DeleteQueue** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="3d556-237">Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)</span><span class="sxs-lookup"><span data-stu-id="3d556-237">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="3d556-238">Első egy **CloudQueueClient** objektum által jelképezett várólista szolgáltatás ügyfél.</span><span class="sxs-lookup"><span data-stu-id="3d556-238">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="3d556-239">Első egy **CloudQueueContainer** objektum, amely egy hivatkozás toohello várólistájára vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="3d556-239">Get a **CloudQueueContainer** object that represents a reference toohello queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="3d556-240">Hello hívás **CloudQueue.Delete** metódus toodelete hello várólista hello által képviselt **CloudQueue** objektum.</span><span class="sxs-lookup"><span data-stu-id="3d556-240">Call hello **CloudQueue.Delete** method toodelete hello queue represented by hello **CloudQueue** object.</span></span>

    ```csharp
    queue.Delete();
    ```

1. <span data-ttu-id="3d556-241">Frissítés hello **ViewBag** hello nevű hello várólista, és a hossza.</span><span class="sxs-lookup"><span data-stu-id="3d556-241">Update hello **ViewBag** with hello name of hello queue, and its length.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```
 
1. <span data-ttu-id="3d556-242">A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **várólisták**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="3d556-242">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Queues**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="3d556-243">A hello **nézet hozzáadása** párbeszédpanelen adja meg **DeleteQueue** hello nézet nevét, és válassza ki a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="3d556-243">On hello **Add View** dialog, enter **DeleteQueue** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="3d556-244">Nyissa meg `DeleteQueue.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:</span><span class="sxs-lookup"><span data-stu-id="3d556-244">Open `DeleteQueue.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "DeleteQueue";
    }
    
    <h2>Delete Queue results</h2>
    
    @ViewBag.QueueName deleted.
    ```

1. <span data-ttu-id="3d556-245">A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="3d556-245">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="3d556-246">Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="3d556-246">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Delete queue", "DeleteQueue", "Queues")</li>
    ```

1. <span data-ttu-id="3d556-247">Hello alkalmazás futtatásához, és válassza ki **várólista hosszának lekérése** toosee eredménye a következő képernyőfelvétel hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="3d556-247">Run hello application, and select **Get queue length** toosee results similar toohello following screen shot:</span></span>
  
    ![Várólista törlése](./media/vs-storage-aspnet-getting-started-queues/delete-queue-results.png)

## <a name="next-steps"></a><span data-ttu-id="3d556-249">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3d556-249">Next steps</span></span>
<span data-ttu-id="3d556-250">Azure-ban való adattárolás további lehetőségeiről további szolgáltatás útmutatók toolearn megtekintése.</span><span class="sxs-lookup"><span data-stu-id="3d556-250">View more feature guides toolearn about additional options for storing data in Azure.</span></span>

  * [<span data-ttu-id="3d556-251">Ismerkedés az Azure blob storage és a Visual Studio kapcsolódó szolgáltatások (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="3d556-251">Get started with Azure blob storage and Visual Studio Connected Services (ASP.NET)</span></span>](./vs-storage-aspnet-getting-started-blobs.md)
  * [<span data-ttu-id="3d556-252">Ismerkedés az Azure table storage és a Visual Studio kapcsolódó szolgáltatások (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="3d556-252">Get started with Azure table storage and Visual Studio Connected Services (ASP.NET)</span></span>](./vs-storage-aspnet-getting-started-tables.md)
