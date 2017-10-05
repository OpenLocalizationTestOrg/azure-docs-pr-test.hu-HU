---
title: "Ismerkedés az Azure várólista-tároló és a Visual Studio kapcsolódó szolgáltatások (ASP.NET) |} Microsoft Docs"
description: "Ismerkedés az Azure üzenetsorának tárhelyet használ egy ASP.NET-projekt, a Visual Studio egy tárfiókot, a Visual Studio kapcsolódó szolgáltatások használatával történő kapcsolódás után"
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
ms.openlocfilehash: 76b0d5e270e16a317ce8a7b424c06c867b537a8e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-azure-queue-storage-and-visual-studio-connected-services-aspnet"></a><span data-ttu-id="853c9-103">Ismerkedés az Azure várólista-tároló és a Visual Studio kapcsolódó szolgáltatások (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="853c9-103">Get started with Azure queue storage and Visual Studio Connected Services (ASP.NET)</span></span>
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="853c9-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="853c9-104">Overview</span></span>

<span data-ttu-id="853c9-105">Az Azure várólista-tároló alkalmazás-összetevők közötti üzenetküldési felhő biztosít.</span><span class="sxs-lookup"><span data-stu-id="853c9-105">Azure queue storage provides cloud messaging between application components.</span></span> <span data-ttu-id="853c9-106">A méretezhető alkalmazások tervezésekor az alkalmazás összetevői gyakran le vannak választva, hogy egymástól függetlenül lehessen őket méretezni.</span><span class="sxs-lookup"><span data-stu-id="853c9-106">In designing applications for scale, application components are often decoupled, so that they can scale independently.</span></span> <span data-ttu-id="853c9-107">A Queue Storage aszinkron üzenetkezelést biztosít az alkalmazások összetevői közötti kommunikációhoz, függetlenül attól, hogy az összetevők a felhőben, asztali gépen, egy helyszíni kiszolgálón vagy egy mobileszközön futnak.</span><span class="sxs-lookup"><span data-stu-id="853c9-107">Queue storage delivers asynchronous messaging for communication between application components, whether they are running in the cloud, on the desktop, on an on-premises server, or on a mobile device.</span></span> <span data-ttu-id="853c9-108">A Queue Storage támogatja az aszinkron feladatok kezelését és a feldolgozási munkafolyamatok kialakítását is.</span><span class="sxs-lookup"><span data-stu-id="853c9-108">Queue storage also supports managing asynchronous tasks and building process work flows.</span></span>

<span data-ttu-id="853c9-109">Ez az oktatóanyag bemutatja, hogyan írhat kódot ASP.NET olyan gyakori forgatókönyveket tartalmaz Azure üzenetsor-kezelési tárolási entitások használata.</span><span class="sxs-lookup"><span data-stu-id="853c9-109">This tutorial shows how to write ASP.NET code for some common scenarios using Azure queue storage entities.</span></span> <span data-ttu-id="853c9-110">Ilyen például a gyakori feladatokat, mint egy Azure-üzenetsorba, létrehozása és hozzáadása, módosítása, olvasása, és eltávolítása a üzenetsor-üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="853c9-110">These scenarios include common tasks such as creating an Azure queue, and adding, modifying, reading, and removing queue messages.</span></span>

##<a name="prerequisites"></a><span data-ttu-id="853c9-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="853c9-111">Prerequisites</span></span>

* [<span data-ttu-id="853c9-112">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="853c9-112">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="853c9-113">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="853c9-113">Azure storage account</span></span>](storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a><span data-ttu-id="853c9-114">Hozzon létre az MVC-vezérlő</span><span class="sxs-lookup"><span data-stu-id="853c9-114">Create an MVC controller</span></span> 

1. <span data-ttu-id="853c9-115">A a **Megoldáskezelőben**, kattintson a jobb gombbal **tartományvezérlők**, és a helyi menüből válassza ki a **Hozzáadás -> tartományvezérlő**.</span><span class="sxs-lookup"><span data-stu-id="853c9-115">In the **Solution Explorer**, right-click **Controllers**, and, from the context menu, select **Add->Controller**.</span></span>

    ![Vezérlő hozzáadása az ASP.NET MVC alkalmazások számára](./media/vs-storage-aspnet-getting-started-queues/add-controller-menu.png)

1. <span data-ttu-id="853c9-117">A a **hozzáadása Scaffold** párbeszédablakban válassza **MVC 5 vezérlő - üres**, és válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="853c9-117">On the **Add Scaffold** dialog, select **MVC 5 Controller - Empty**, and select **Add**.</span></span>

    ![Adja meg az MVC-vezérlő típusa](./media/vs-storage-aspnet-getting-started-queues/add-controller.png)

1. <span data-ttu-id="853c9-119">Az a **vezérlő hozzáadása** párbeszédpanelen, a tartományvezérlő nevét *QueuesController*, és válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="853c9-119">On the **Add Controller** dialog, name the controller *QueuesController*, and select **Add**.</span></span>

    ![Neve az MVC-vezérlő](./media/vs-storage-aspnet-getting-started-queues/add-controller-name.png)

1. <span data-ttu-id="853c9-121">Adja hozzá a következő *használatával* irányelvek a `QueuesController.cs` fájlt:</span><span class="sxs-lookup"><span data-stu-id="853c9-121">Add the following *using* directives to the `QueuesController.cs` file:</span></span>

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;
    ```
## <a name="create-a-queue"></a><span data-ttu-id="853c9-122">Üzenetsor létrehozása</span><span class="sxs-lookup"><span data-stu-id="853c9-122">Create a queue</span></span>

<span data-ttu-id="853c9-123">A következő lépések bemutatják, hogyan várólista létrehozása:</span><span class="sxs-lookup"><span data-stu-id="853c9-123">The following steps illustrate how to create a queue:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="853c9-124">Ez a szakasz azt feltételezi, hogy végrehajtotta a [beállította a fejlesztőkörnyezetet](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="853c9-124">This section assumes you have completed the steps [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="853c9-125">Nyissa meg az `QueuesController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="853c9-125">Open the `QueuesController.cs` file.</span></span> 

1. <span data-ttu-id="853c9-126">Adja hozzá a hívott metódus **CreateQueue** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="853c9-126">Add a method called **CreateQueue** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult CreateQueue()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="853c9-127">Belül a **CreateQueue** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="853c9-127">Within the **CreateQueue** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="853c9-128">A tárolási kapcsolati karakterlánc és tárfiókadatok beolvasása az Azure szolgáltatás konfigurációs az alábbi kód használatával: (módosítása  *&lt;tárfióknév >* elérni az Azure storage-fiók nevére.)</span><span class="sxs-lookup"><span data-stu-id="853c9-128">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="853c9-129">Első egy **CloudQueueClient** objektum által jelképezett várólista szolgáltatás ügyfél.</span><span class="sxs-lookup"><span data-stu-id="853c9-129">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```
1. <span data-ttu-id="853c9-130">Első egy **CloudQueue** hivatkozni kell a kívánt sor nevét képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="853c9-130">Get a **CloudQueue** object that represents a reference to the desired queue name.</span></span> <span data-ttu-id="853c9-131">A **CloudQueueClient.GetQueueReference** metódus nem tesz egy kérelmet a queue storage.</span><span class="sxs-lookup"><span data-stu-id="853c9-131">The **CloudQueueClient.GetQueueReference** method does not make a request against queue storage.</span></span> <span data-ttu-id="853c9-132">A referencia-e a várólista létezik adja vissza.</span><span class="sxs-lookup"><span data-stu-id="853c9-132">The reference is returned whether or not the queue exists.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="853c9-133">Hívja a **CloudQueue.CreateIfNotExists** módszer a várólista létrehozására, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="853c9-133">Call the **CloudQueue.CreateIfNotExists** method to create the queue if it does not yet exist.</span></span> <span data-ttu-id="853c9-134">A **CloudQueue.CreateIfNotExists** metódus beolvasása **igaz** , ha a várólista nem létezik, és sikeresen létrejött.</span><span class="sxs-lookup"><span data-stu-id="853c9-134">The **CloudQueue.CreateIfNotExists** method returns **true** if the queue does not exist, and is successfully created.</span></span> <span data-ttu-id="853c9-135">Ellenkező esetben **hamis** adja vissza.</span><span class="sxs-lookup"><span data-stu-id="853c9-135">Otherwise, **false** is returned.</span></span>    

    ```csharp
    ViewBag.Success = queue.CreateIfNotExists();
    ```

1. <span data-ttu-id="853c9-136">Frissítés a **ViewBag** a várólista nevét.</span><span class="sxs-lookup"><span data-stu-id="853c9-136">Update the **ViewBag** with the name of the queue.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```

1. <span data-ttu-id="853c9-137">A a **Solution Explorer**, bontsa ki a **nézetek** mappát, kattintson a jobb gombbal **várólisták**, és válassza a helyi menüben a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="853c9-137">In the **Solution Explorer**, expand the **Views** folder, right-click **Queues**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="853c9-138">Az a **nézet hozzáadása** párbeszédpanelen adja meg **CreateQueue** a nézet nevét, majd válassza a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="853c9-138">On the **Add View** dialog, enter **CreateQueue** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="853c9-139">Nyissa meg `CreateQueue.cshtml`, és módosítsa úgy, hogy például a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="853c9-139">Open `CreateQueue.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Create Queue";
    }
    
    <h2>Create Queue results</h2>

    Creation of @ViewBag.QueueName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. <span data-ttu-id="853c9-140">Az a **Megoldáskezelőben**, bontsa ki a **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="853c9-140">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="853c9-141">Után utolsó **Html.ActionLink**, adja hozzá a következő **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="853c9-141">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Create queue", "CreateQueue", "Queues")</li>
    ```

1. <span data-ttu-id="853c9-142">Futtassa az alkalmazást, és válassza ki **létrehozás várólista** az alábbi képernyőfelvételhez hasonló eredmények megtekintése érdekében:</span><span class="sxs-lookup"><span data-stu-id="853c9-142">Run the application, and select **Create queue** to see results similar to the following screen shot:</span></span>
  
    ![Várólista létrehozása](./media/vs-storage-aspnet-getting-started-queues/create-queue-results.png)

    <span data-ttu-id="853c9-144">Ahogy korábban említettük a **CloudQueue.CreateIfNotExists** metódus beolvasása **igaz** csak a várólista nem létezik és jön létre.</span><span class="sxs-lookup"><span data-stu-id="853c9-144">As mentioned previously, the **CloudQueue.CreateIfNotExists** method returns **true** only when the queue doesn't exist and is created.</span></span> <span data-ttu-id="853c9-145">Ezért amikor a várólista létezik-e, futtassa az alkalmazást, ha a metódus visszaadja **hamis**.</span><span class="sxs-lookup"><span data-stu-id="853c9-145">Therefore, if you run the app when the queue exists, the method returns **false**.</span></span> <span data-ttu-id="853c9-146">Az alkalmazás többször is lefuthat, az alkalmazás ismételt futtatása előtt kell törölnie a várólistát.</span><span class="sxs-lookup"><span data-stu-id="853c9-146">To run the app multiple times, you must delete the queue before running the app again.</span></span> <span data-ttu-id="853c9-147">A sor törlése megteheti a **CloudQueue.Delete** metódust.</span><span class="sxs-lookup"><span data-stu-id="853c9-147">Deleting the queue can be done via the **CloudQueue.Delete** method.</span></span> <span data-ttu-id="853c9-148">A várólista használatával is törölheti a [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040) vagy a [Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="853c9-148">You can also delete the queue using the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) or the [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>  

## <a name="add-a-message-to-a-queue"></a><span data-ttu-id="853c9-149">A várólista üzenet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="853c9-149">Add a message to a queue</span></span>

<span data-ttu-id="853c9-150">Miután megismerte [egy sor](#create-a-queue), erre a várólistára üzenetet is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="853c9-150">Once you've [created a queue](#create-a-queue), you can add messages to that queue.</span></span> <span data-ttu-id="853c9-151">Ez a szakasz bemutatja, hogyan üzenet ad hozzá egy várólista *teszt-várólista*.</span><span class="sxs-lookup"><span data-stu-id="853c9-151">This section walks you through adding a message to a queue *test-queue*.</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="853c9-152">Ez a szakasz azt feltételezi, hogy végrehajtotta a [beállította a fejlesztőkörnyezetet](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="853c9-152">This section assumes you have completed the steps [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="853c9-153">Nyissa meg az `QueuesController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="853c9-153">Open the `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="853c9-154">Adja hozzá a hívott metódus **AddMessage** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="853c9-154">Add a method called **AddMessage** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult AddMessage()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="853c9-155">Belül a **AddMessage** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="853c9-155">Within the **AddMessage** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="853c9-156">A tárolási kapcsolati karakterlánc és tárfiókadatok beolvasása az Azure szolgáltatás konfigurációs az alábbi kód használatával: (módosítása  *&lt;tárfióknév >* elérni az Azure storage-fiók nevére.)</span><span class="sxs-lookup"><span data-stu-id="853c9-156">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="853c9-157">Első egy **CloudQueueClient** objektum által jelképezett várólista szolgáltatás ügyfél.</span><span class="sxs-lookup"><span data-stu-id="853c9-157">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="853c9-158">Első egy **CloudQueueContainer** hivatkozni kell a várólista képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="853c9-158">Get a **CloudQueueContainer** object that represents a reference to the queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="853c9-159">Hozzon létre a **CloudQueueMessage** az üzenetet a várólistában hozzá szeretné képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="853c9-159">Create the **CloudQueueMessage** object representing the message you want to add to the queue.</span></span> <span data-ttu-id="853c9-160">A **CloudQueueMessage** objektum vagy egy karakterláncból (UTF-8 formátumban), vagy egy bájttömböt hozható létre.</span><span class="sxs-lookup"><span data-stu-id="853c9-160">A **CloudQueueMessage** object can be created from either a string (in UTF-8 format) or a byte array.</span></span>

    ```csharp
    CloudQueueMessage message = new CloudQueueMessage("Hello, Azure Queue Storage");
    ```

1. <span data-ttu-id="853c9-161">Hívja a **CloudQueue.AddMessage** a várakozási sorba a messaged adható hozzá.</span><span class="sxs-lookup"><span data-stu-id="853c9-161">Call the **CloudQueue.AddMessage** method to add the messaged to the queue.</span></span>

    ```csharp
    queue.AddMessage(message);
    ```

1. <span data-ttu-id="853c9-162">Néhány létrehozásáról és **ViewBag** a Nézet tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="853c9-162">Create and set a couple of **ViewBag** properties for display in the view.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```

1. <span data-ttu-id="853c9-163">A a **Solution Explorer**, bontsa ki a **nézetek** mappát, kattintson a jobb gombbal **várólisták**, és válassza a helyi menüben a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="853c9-163">In the **Solution Explorer**, expand the **Views** folder, right-click **Queues**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="853c9-164">Az a **nézet hozzáadása** párbeszédpanelen adja meg **AddMessage** a nézet nevét, majd válassza a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="853c9-164">On the **Add View** dialog, enter **AddMessage** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="853c9-165">Nyissa meg `AddMessage.cshtml`, és módosítsa úgy, hogy például a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="853c9-165">Open `AddMessage.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Add Message";
    }
    
    <h2>Add Message results</h2>
    
    The message '@ViewBag.Message' was added to the queue '@ViewBag.QueueName'.
    ```

1. <span data-ttu-id="853c9-166">Az a **Megoldáskezelőben**, bontsa ki a **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="853c9-166">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="853c9-167">Után utolsó **Html.ActionLink**, adja hozzá a következő **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="853c9-167">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Add message", "AddMessage", "Queues")</li>
    ```

1. <span data-ttu-id="853c9-168">Futtassa az alkalmazást, és válassza ki **Hozzáadás üzenet** az alábbi képernyőfelvételhez hasonló eredmények megtekintése érdekében:</span><span class="sxs-lookup"><span data-stu-id="853c9-168">Run the application, and select **Add message** to see results similar to the following screen shot:</span></span>
  
    ![Üzenet hozzáadása](./media/vs-storage-aspnet-getting-started-queues/add-message-results.png)

<span data-ttu-id="853c9-170">A két szakasz - [üzenet olvasása az üzenetsorból eltávolítása nélkül](#read-a-message-from-a-queue-without-removing-it) és [olvasási és eltávolítása egy üzenetet az üzenetsorból](#read-and-remove-a-message-from-a-queue) -bemutatják, hogyan lehet üzenetek olvasása az üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="853c9-170">The two sections - [Read a message from a queue without removing it](#read-a-message-from-a-queue-without-removing-it) and [Read and remove a message from a queue](#read-and-remove-a-message-from-a-queue) - illustrate how to read messages from a queue.</span></span>    

## <a name="read-a-message-from-a-queue-without-removing-it"></a><span data-ttu-id="853c9-171">Egy üzenet olvasása az üzenetsorból eltávolítása nélkül</span><span class="sxs-lookup"><span data-stu-id="853c9-171">Read a message from a queue without removing it</span></span>

<span data-ttu-id="853c9-172">Ez a szakasz bemutatja, hogyan való aszinkron üzenet (az első üzenet olvasása eltávolítása nélkül).</span><span class="sxs-lookup"><span data-stu-id="853c9-172">This section illustrates how to peek at a queued message (read the first message without removing it).</span></span>  

> [!NOTE]
> 
> <span data-ttu-id="853c9-173">Ez a szakasz azt feltételezi, hogy végrehajtotta a [beállította a fejlesztőkörnyezetet](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="853c9-173">This section assumes you have completed the steps [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="853c9-174">Nyissa meg az `QueuesController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="853c9-174">Open the `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="853c9-175">Adja hozzá a hívott metódus **PeekMessage** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="853c9-175">Add a method called **PeekMessage** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult PeekMessage()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="853c9-176">Belül a **PeekMessage** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="853c9-176">Within the **PeekMessage** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="853c9-177">A tárolási kapcsolati karakterlánc és tárfiókadatok beolvasása az Azure szolgáltatás konfigurációs az alábbi kód használatával: (módosítása  *&lt;tárfióknév >* elérni az Azure storage-fiók nevére.)</span><span class="sxs-lookup"><span data-stu-id="853c9-177">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="853c9-178">Első egy **CloudQueueClient** objektum által jelképezett várólista szolgáltatás ügyfél.</span><span class="sxs-lookup"><span data-stu-id="853c9-178">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="853c9-179">Első egy **CloudQueueContainer** hivatkozni kell a várólista képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="853c9-179">Get a **CloudQueueContainer** object that represents a reference to the queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="853c9-180">Hívja a **CloudQueue.PeekMessage** módszer a várólista első üzenetébe olvasni a eltávolítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="853c9-180">Call the **CloudQueue.PeekMessage** method to read the first message in the queue without removing it from the queue.</span></span> 

    ```csharp
    CloudQueueMessage message = queue.PeekMessage();
    ```

1. <span data-ttu-id="853c9-181">Frissítés a **ViewBag** két értékekkel: a várólista nevét és az üzenetet, amely lett beolvasva.</span><span class="sxs-lookup"><span data-stu-id="853c9-181">Update the **ViewBag** with two values: the queue name and the message that was read.</span></span> <span data-ttu-id="853c9-182">A **CloudQueueMessage** vezérlőnek két tulajdonságait az objektum értékének lekérését: **CloudQueueMessage.AsBytes** és **CloudQueueMessage.AsString**.</span><span class="sxs-lookup"><span data-stu-id="853c9-182">The **CloudQueueMessage** object exposes two properties for getting the object's value: **CloudQueueMessage.AsBytes** and **CloudQueueMessage.AsString**.</span></span> <span data-ttu-id="853c9-183">**AsString** (ebben a példában használt) egy karakterláncot ad vissza, miközben **AsBytes** adja vissza egy bájttömböt.</span><span class="sxs-lookup"><span data-stu-id="853c9-183">**AsString** (used in this example) returns a string, while **AsBytes** returns a byte array.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name; 
    ViewBag.Message = (message != null ? message.AsString : "");
    ```

1. <span data-ttu-id="853c9-184">A a **Solution Explorer**, bontsa ki a **nézetek** mappát, kattintson a jobb gombbal **várólisták**, és válassza a helyi menüben a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="853c9-184">In the **Solution Explorer**, expand the **Views** folder, right-click **Queues**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="853c9-185">Az a **nézet hozzáadása** párbeszédpanelen adja meg **PeekMessage** a nézet nevét, majd válassza a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="853c9-185">On the **Add View** dialog, enter **PeekMessage** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="853c9-186">Nyissa meg `PeekMessage.cshtml`, és módosítsa úgy, hogy például a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="853c9-186">Open `PeekMessage.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

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

1. <span data-ttu-id="853c9-187">Az a **Megoldáskezelőben**, bontsa ki a **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="853c9-187">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="853c9-188">Után utolsó **Html.ActionLink**, adja hozzá a következő **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="853c9-188">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Peek message", "PeekMessage", "Queues")</li>
    ```

1. <span data-ttu-id="853c9-189">Futtassa az alkalmazást, és válassza ki **betekintés üzenet** az alábbi képernyőfelvételhez hasonló eredmények megtekintése érdekében:</span><span class="sxs-lookup"><span data-stu-id="853c9-189">Run the application, and select **Peek message** to see results similar to the following screen shot:</span></span>
  
    ![Üzenet megtekintése](./media/vs-storage-aspnet-getting-started-queues/peek-message-results.png)

## <a name="read-and-remove-a-message-from-a-queue"></a><span data-ttu-id="853c9-191">Olvassa el, és távolítsa el az üzenetet az üzenetsorból</span><span class="sxs-lookup"><span data-stu-id="853c9-191">Read and remove a message from a queue</span></span>

<span data-ttu-id="853c9-192">Ebben a szakaszban megismerheti, hogyan kiolvasni, és távolítsa el az üzenetet az üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="853c9-192">In this section, you learn how to read and remove a message from a queue.</span></span>   

> [!NOTE]
> 
> <span data-ttu-id="853c9-193">Ez a szakasz azt feltételezi, hogy végrehajtotta a [beállította a fejlesztőkörnyezetet](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="853c9-193">This section assumes you have completed the steps [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="853c9-194">Nyissa meg az `QueuesController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="853c9-194">Open the `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="853c9-195">Adja hozzá a hívott metódus **ReadMessage** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="853c9-195">Add a method called **ReadMessage** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult ReadMessage()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="853c9-196">Belül a **ReadMessage** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="853c9-196">Within the **ReadMessage** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="853c9-197">A tárolási kapcsolati karakterlánc és tárfiókadatok beolvasása az Azure szolgáltatás konfigurációs az alábbi kód használatával: (módosítása  *&lt;tárfióknév >* elérni az Azure storage-fiók nevére.)</span><span class="sxs-lookup"><span data-stu-id="853c9-197">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="853c9-198">Első egy **CloudQueueClient** objektum által jelképezett várólista szolgáltatás ügyfél.</span><span class="sxs-lookup"><span data-stu-id="853c9-198">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="853c9-199">Első egy **CloudQueueContainer** hivatkozni kell a várólista képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="853c9-199">Get a **CloudQueueContainer** object that represents a reference to the queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="853c9-200">Hívja a **CloudQueue.GetMessage** metódus olvasni a várólista első üzenetébe.</span><span class="sxs-lookup"><span data-stu-id="853c9-200">Call the **CloudQueue.GetMessage** method to read the first message in the queue.</span></span> <span data-ttu-id="853c9-201">A **CloudQueue.GetMessage** módszer lehetővé teszi az üzenet nem látható üzeneteket olvasó, így nincs más kód módosíthatja vagy törölheti az üzenet feldolgozásakor a program a azt többi kód számára (alapértelmezés) 30 másodpercig.</span><span class="sxs-lookup"><span data-stu-id="853c9-201">The **CloudQueue.GetMessage** method makes the message invisible for 30 seconds (by default) to any other code reading messages so that no other code can modify or delete the message while your processing it.</span></span> <span data-ttu-id="853c9-202">Az üzenet nem látható idő megváltoztatásához módosítsa a **visibilityTimeout** átadott paraméter a **CloudQueue.GetMessage** metódust.</span><span class="sxs-lookup"><span data-stu-id="853c9-202">To change the amount of time the message is invisible, modify the **visibilityTimeout** parameter being passed to the **CloudQueue.GetMessage** method.</span></span>

    ```csharp
    // This message will be invisible to other code for 30 seconds.
    CloudQueueMessage message = queue.GetMessage();     
    ```

1. <span data-ttu-id="853c9-203">Hívja a **CloudQueueMessage.Delete** metódus az üzenet törlése az üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="853c9-203">Call the **CloudQueueMessage.Delete** method to delete the message from the queue.</span></span>

    ```csharp
    queue.DeleteMessage(message);
    ```

1. <span data-ttu-id="853c9-204">Frissítés a **ViewBag** törli az üzenetet, és a várólista nevét.</span><span class="sxs-lookup"><span data-stu-id="853c9-204">Update the **ViewBag** with the message deleted, and the name of the queue.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```
 
1. <span data-ttu-id="853c9-205">A a **Solution Explorer**, bontsa ki a **nézetek** mappát, kattintson a jobb gombbal **várólisták**, és válassza a helyi menüben a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="853c9-205">In the **Solution Explorer**, expand the **Views** folder, right-click **Queues**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="853c9-206">Az a **nézet hozzáadása** párbeszédpanelen adja meg **ReadMessage** a nézet nevét, majd válassza a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="853c9-206">On the **Add View** dialog, enter **ReadMessage** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="853c9-207">Nyissa meg `ReadMessage.cshtml`, és módosítsa úgy, hogy például a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="853c9-207">Open `ReadMessage.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

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

1. <span data-ttu-id="853c9-208">Az a **Megoldáskezelőben**, bontsa ki a **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="853c9-208">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="853c9-209">Után utolsó **Html.ActionLink**, adja hozzá a következő **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="853c9-209">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Read/Delete message", "ReadMessage", "Queues")</li>
    ```

1. <span data-ttu-id="853c9-210">Futtassa az alkalmazást, és válassza ki **olvasás/törlés üzenet** az alábbi képernyőfelvételhez hasonló eredmények megtekintése érdekében:</span><span class="sxs-lookup"><span data-stu-id="853c9-210">Run the application, and select **Read/Delete message** to see results similar to the following screen shot:</span></span>
  
    ![Olvasási és törlési üzenet](./media/vs-storage-aspnet-getting-started-queues/read-message-results.png)

## <a name="get-the-queue-length"></a><span data-ttu-id="853c9-212">Az üzenetsor hosszának lekérése</span><span class="sxs-lookup"><span data-stu-id="853c9-212">Get the queue length</span></span>

<span data-ttu-id="853c9-213">Ez a szakasz bemutatja az beszerzése a várólista hossza (üzenetek száma).</span><span class="sxs-lookup"><span data-stu-id="853c9-213">This section illustrates how to get the queue length (number of messages).</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="853c9-214">Ez a szakasz azt feltételezi, hogy végrehajtotta a [beállította a fejlesztőkörnyezetet](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="853c9-214">This section assumes you have completed the steps [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="853c9-215">Nyissa meg az `QueuesController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="853c9-215">Open the `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="853c9-216">Adja hozzá a hívott metódus **GetQueueLength** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="853c9-216">Add a method called **GetQueueLength** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult GetQueueLength()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="853c9-217">Belül a **ReadMessage** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="853c9-217">Within the **ReadMessage** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="853c9-218">A tárolási kapcsolati karakterlánc és tárfiókadatok beolvasása az Azure szolgáltatás konfigurációs az alábbi kód használatával: (módosítása  *&lt;tárfióknév >* elérni az Azure storage-fiók nevére.)</span><span class="sxs-lookup"><span data-stu-id="853c9-218">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="853c9-219">Első egy **CloudQueueClient** objektum által jelképezett várólista szolgáltatás ügyfél.</span><span class="sxs-lookup"><span data-stu-id="853c9-219">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="853c9-220">Első egy **CloudQueueContainer** hivatkozni kell a várólista képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="853c9-220">Get a **CloudQueueContainer** object that represents a reference to the queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="853c9-221">Hívja a **CloudQueue.FetchAttributes** metódusának segítéségével lekérheti a várólista attribútumok (beleértve a hossza).</span><span class="sxs-lookup"><span data-stu-id="853c9-221">Call the **CloudQueue.FetchAttributes** method to retrieve the queue's attributes (including its length).</span></span> 

    ```csharp
    queue.FetchAttributes();
    ```

6. <span data-ttu-id="853c9-222">Hozzáférés a **CloudQueue.ApproximateMessageCount** tulajdonság használatával beolvassa a várólista hossza.</span><span class="sxs-lookup"><span data-stu-id="853c9-222">Access the **CloudQueue.ApproximateMessageCount** property to get the queue's length.</span></span>
 
    ```csharp
    int? nMessages = queue.ApproximateMessageCount;
    ```

1. <span data-ttu-id="853c9-223">Frissítés a **ViewBag** a nevét, valamint a várólista hossza.</span><span class="sxs-lookup"><span data-stu-id="853c9-223">Update the **ViewBag** with the name of the queue, and its length.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Length = nMessages;
    ```
 
1. <span data-ttu-id="853c9-224">A a **Solution Explorer**, bontsa ki a **nézetek** mappát, kattintson a jobb gombbal **várólisták**, és válassza a helyi menüben a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="853c9-224">In the **Solution Explorer**, expand the **Views** folder, right-click **Queues**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="853c9-225">Az a **nézet hozzáadása** párbeszédpanelen adja meg **GetQueueLength** a nézet nevét, majd válassza a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="853c9-225">On the **Add View** dialog, enter **GetQueueLength** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="853c9-226">Nyissa meg `GetQueueLengthMessage.cshtml`, és módosítsa úgy, hogy például a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="853c9-226">Open `GetQueueLengthMessage.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "GetQueueLength";
    }
    
    <h2>Get Queue Length results</h2>
    
    The queue '@ViewBag.QueueName' has a length of (number of messages): @ViewBag.Length
    ```

1. <span data-ttu-id="853c9-227">Az a **Megoldáskezelőben**, bontsa ki a **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="853c9-227">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="853c9-228">Után utolsó **Html.ActionLink**, adja hozzá a következő **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="853c9-228">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Get queue length", "GetQueueLength", "Queues")</li>
    ```

1. <span data-ttu-id="853c9-229">Futtassa az alkalmazást, és válassza ki **várólista hosszának lekérése** az alábbi képernyőfelvételhez hasonló eredmények megtekintése érdekében:</span><span class="sxs-lookup"><span data-stu-id="853c9-229">Run the application, and select **Get queue length** to see results similar to the following screen shot:</span></span>
  
    ![Get-várólista hossza](./media/vs-storage-aspnet-getting-started-queues/get-queue-length-results.png)


## <a name="delete-a-queue"></a><span data-ttu-id="853c9-231">Üzenetsor törlése</span><span class="sxs-lookup"><span data-stu-id="853c9-231">Delete a queue</span></span>
<span data-ttu-id="853c9-232">Ez a szakasz bemutatja, hogyan várólista törlése.</span><span class="sxs-lookup"><span data-stu-id="853c9-232">This section illustrates how to delete a queue.</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="853c9-233">Ez a szakasz azt feltételezi, hogy végrehajtotta a [beállította a fejlesztőkörnyezetet](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="853c9-233">This section assumes you have completed the steps [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="853c9-234">Nyissa meg az `QueuesController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="853c9-234">Open the `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="853c9-235">Adja hozzá a hívott metódus **DeleteQueue** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="853c9-235">Add a method called **DeleteQueue** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult DeleteQueue()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="853c9-236">Belül a **DeleteQueue** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="853c9-236">Within the **DeleteQueue** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="853c9-237">A tárolási kapcsolati karakterlánc és tárfiókadatok beolvasása az Azure szolgáltatás konfigurációs az alábbi kód használatával: (módosítása  *&lt;tárfióknév >* elérni az Azure storage-fiók nevére.)</span><span class="sxs-lookup"><span data-stu-id="853c9-237">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="853c9-238">Első egy **CloudQueueClient** objektum által jelképezett várólista szolgáltatás ügyfél.</span><span class="sxs-lookup"><span data-stu-id="853c9-238">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="853c9-239">Első egy **CloudQueueContainer** hivatkozni kell a várólista képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="853c9-239">Get a **CloudQueueContainer** object that represents a reference to the queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="853c9-240">Hívja a **CloudQueue.Delete** metódus által képviselt a várólista törlése a **CloudQueue** objektum.</span><span class="sxs-lookup"><span data-stu-id="853c9-240">Call the **CloudQueue.Delete** method to delete the queue represented by the **CloudQueue** object.</span></span>

    ```csharp
    queue.Delete();
    ```

1. <span data-ttu-id="853c9-241">Frissítés a **ViewBag** a nevét, valamint a várólista hossza.</span><span class="sxs-lookup"><span data-stu-id="853c9-241">Update the **ViewBag** with the name of the queue, and its length.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```
 
1. <span data-ttu-id="853c9-242">A a **Solution Explorer**, bontsa ki a **nézetek** mappát, kattintson a jobb gombbal **várólisták**, és válassza a helyi menüben a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="853c9-242">In the **Solution Explorer**, expand the **Views** folder, right-click **Queues**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="853c9-243">Az a **nézet hozzáadása** párbeszédpanelen adja meg **DeleteQueue** a nézet nevét, majd válassza a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="853c9-243">On the **Add View** dialog, enter **DeleteQueue** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="853c9-244">Nyissa meg `DeleteQueue.cshtml`, és módosítsa úgy, hogy például a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="853c9-244">Open `DeleteQueue.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "DeleteQueue";
    }
    
    <h2>Delete Queue results</h2>
    
    @ViewBag.QueueName deleted.
    ```

1. <span data-ttu-id="853c9-245">Az a **Megoldáskezelőben**, bontsa ki a **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="853c9-245">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="853c9-246">Után utolsó **Html.ActionLink**, adja hozzá a következő **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="853c9-246">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Delete queue", "DeleteQueue", "Queues")</li>
    ```

1. <span data-ttu-id="853c9-247">Futtassa az alkalmazást, és válassza ki **várólista hosszának lekérése** az alábbi képernyőfelvételhez hasonló eredmények megtekintése érdekében:</span><span class="sxs-lookup"><span data-stu-id="853c9-247">Run the application, and select **Get queue length** to see results similar to the following screen shot:</span></span>
  
    ![Várólista törlése](./media/vs-storage-aspnet-getting-started-queues/delete-queue-results.png)

## <a name="next-steps"></a><span data-ttu-id="853c9-249">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="853c9-249">Next steps</span></span>
<span data-ttu-id="853c9-250">Az Azure-ban való adattárolás további lehetőségeiről tekintse meg a többi szolgáltatás-útmutatót.</span><span class="sxs-lookup"><span data-stu-id="853c9-250">View more feature guides to learn about additional options for storing data in Azure.</span></span>

  * [<span data-ttu-id="853c9-251">Ismerkedés az Azure blob storage és a Visual Studio kapcsolódó szolgáltatások (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="853c9-251">Get started with Azure blob storage and Visual Studio Connected Services (ASP.NET)</span></span>](./vs-storage-aspnet-getting-started-blobs.md)
  * [<span data-ttu-id="853c9-252">Ismerkedés az Azure table storage és a Visual Studio kapcsolódó szolgáltatások (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="853c9-252">Get started with Azure table storage and Visual Studio Connected Services (ASP.NET)</span></span>](./vs-storage-aspnet-getting-started-tables.md)
