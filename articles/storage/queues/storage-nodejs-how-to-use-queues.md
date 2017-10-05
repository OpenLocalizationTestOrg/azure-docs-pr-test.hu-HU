---
title: "Node.js-ből a Queue storage használata |} Microsoft Docs"
description: "Útmutató az Azure Queue szolgáltatás segítségével hozza létre, és törli az üzenetsorok, és helyezze, get, és törli az üzenetet. A minta Node.js nyelven írt."
services: storage
documentationcenter: nodejs
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: a8a92db0-4333-43dd-a116-28b3147ea401
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: 15c1d3cb6eac8fc14837277c4a4275dea91701cd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-queue-storage-from-nodejs"></a><span data-ttu-id="0bf32-104">How to use Queue storage from Node.js (A Queue Storage használata Node.js-sel)</span><span class="sxs-lookup"><span data-stu-id="0bf32-104">How to use Queue storage from Node.js</span></span>
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a><span data-ttu-id="0bf32-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="0bf32-105">Overview</span></span>
<span data-ttu-id="0bf32-106">Ez az útmutató bemutatja, hogyan hajthat végre a szolgáltatást a Microsoft Azure Queue szolgáltatást használó általános forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="0bf32-106">This guide shows you how to perform common scenarios using the Microsoft Azure Queue service.</span></span> <span data-ttu-id="0bf32-107">A minták íródtak, a Node.js API használatával.</span><span class="sxs-lookup"><span data-stu-id="0bf32-107">The samples are written using the Node.js API.</span></span> <span data-ttu-id="0bf32-108">Az ismertetett forgatókönyvek **beszúrása**, **megtekintésekor**, **első**, és **törlése** üzenetek várólistára, valamint **létrehozása és törlése várólisták**.</span><span class="sxs-lookup"><span data-stu-id="0bf32-108">The scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating and deleting queues**.</span></span>

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a><span data-ttu-id="0bf32-109">Node.js-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0bf32-109">Create a Node.js Application</span></span>
<span data-ttu-id="0bf32-110">Üres Node.js-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0bf32-110">Create a blank Node.js application.</span></span> <span data-ttu-id="0bf32-111">A Node.js-alkalmazás létrehozása utasításokért lásd: [Node.js-webalkalmazás létrehozása az Azure App Service](../../app-service-web/app-service-web-get-started-nodejs.md), [létrehozása és központi telepítése egy Azure Cloud Service a Node.js-alkalmazás](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) Windows PowerShell használatával vagy [hozza létre, és a Node.js webalkalmazás telepítése az Azure-ban a Web Matrix](https://www.microsoft.com/web/webmatrix/).</span><span class="sxs-lookup"><span data-stu-id="0bf32-111">For instructions creating a Node.js application, see [Create a Node.js web app in Azure App Service](../../app-service-web/app-service-web-get-started-nodejs.md), [Build and deploy a Node.js application to an Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) using Windows PowerShell, or [Build and deploy a Node.js web app to Azure using Web Matrix](https://www.microsoft.com/web/webmatrix/).</span></span>

## <a name="configure-your-application-to-access-storage"></a><span data-ttu-id="0bf32-112">Állítsa be az alkalmazását tároló elérése érdekében</span><span class="sxs-lookup"><span data-stu-id="0bf32-112">Configure Your Application to Access Storage</span></span>
<span data-ttu-id="0bf32-113">Az Azure storage használatához szüksége az Azure Storage szolgáltatás SDK a Node.js, amely tartalmaz egy kényelmi szalagtár szerepel, amely a többi tárolási szolgáltatásokkal kommunikálni.</span><span class="sxs-lookup"><span data-stu-id="0bf32-113">To use Azure storage, you need the Azure Storage SDK for Node.js, which includes a set of convenience libraries that communicate with the storage REST services.</span></span>

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a><span data-ttu-id="0bf32-114">Csomópont Package Manager (NPM) használja a csomag beszerzése</span><span class="sxs-lookup"><span data-stu-id="0bf32-114">Use Node Package Manager (NPM) to obtain the package</span></span>
1. <span data-ttu-id="0bf32-115">Használjon például egy parancssori felületet **PowerShell** (Windows), **Terminálszolgáltatások** (Mac) vagy **Bash** (Unix), lépjen abba a mappába, amelyben létrehozta a mintaalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="0bf32-115">Use a command-line interface such as **PowerShell** (Windows,) **Terminal** (Mac,) or **Bash** (Unix), navigate to the folder where you created your sample application.</span></span>
2. <span data-ttu-id="0bf32-116">Típus **npm telepítése azure-tároló** a parancsablakban.</span><span class="sxs-lookup"><span data-stu-id="0bf32-116">Type **npm install azure-storage** in the command window.</span></span> <span data-ttu-id="0bf32-117">A parancs kimenetében a következőhöz hasonló.</span><span class="sxs-lookup"><span data-stu-id="0bf32-117">Output from the command is similar to the following example.</span></span>
 
    ```
    azure-storage@0.5.0 node_modules\azure-storage
    +-- extend@1.2.1
    +-- xmlbuilder@0.4.3
    +-- mime@1.2.11
    +-- node-uuid@1.4.3
    +-- validator@3.22.2
    +-- underscore@1.4.4
    +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
    +-- xml2js@0.2.7 (sax@0.5.2)
    +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
    ```

3. <span data-ttu-id="0bf32-118">Manuálisan futtatható a **ls** parancs futtatásával ellenőrizze, hogy egy **csomópont\_modulok** mappa hozták létre.</span><span class="sxs-lookup"><span data-stu-id="0bf32-118">You can manually run the **ls** command to verify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="0bf32-119">A mappában megtalálja az **azure-tároló** csomagot, amely tartalmazza a könyvtárak kell érniük a tárhelyet.</span><span class="sxs-lookup"><span data-stu-id="0bf32-119">Inside that folder you will find the **azure-storage** package, which contains the libraries you need to access storage.</span></span>

### <a name="import-the-package"></a><span data-ttu-id="0bf32-120">A csomag importálása</span><span class="sxs-lookup"><span data-stu-id="0bf32-120">Import the package</span></span>
<span data-ttu-id="0bf32-121">A Jegyzettömbben vagy más szövegszerkesztőben, adja hozzá a következő végighaladva a **server.js** fájl tartalmát, ha szeretne használni a tárolási:</span><span class="sxs-lookup"><span data-stu-id="0bf32-121">Using Notepad or another text editor, add the following to the top the **server.js** file of the application where you intend to use storage:</span></span>

```
var azure = require('azure-storage');
```

## <a name="setup-an-azure-storage-connection"></a><span data-ttu-id="0bf32-122">Az Azure Storage-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="0bf32-122">Setup an Azure Storage Connection</span></span>
<span data-ttu-id="0bf32-123">Az azure-moduljának olvassák a környezeti változók AZURE\_tárolási\_FIÓKOT és az AZURE\_tárolási\_hozzáférés\_kulcs, vagy AZURE\_tárolási\_kapcsolat\_KARAKTERLÁNCOT az Azure storage-fiókhoz való kapcsolódáshoz szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="0bf32-123">The azure module will read the environment variables AZURE\_STORAGE\_ACCOUNT and AZURE\_STORAGE\_ACCESS\_KEY, or AZURE\_STORAGE\_CONNECTION\_STRING for information required to connect to your Azure storage account.</span></span> <span data-ttu-id="0bf32-124">Ha ezek a környezeti változók nem, meg kell adnia a fiókadatokat, meghívásakor **createQueueService**.</span><span class="sxs-lookup"><span data-stu-id="0bf32-124">If these environment variables are not set, you must specify the account information when calling **createQueueService**.</span></span>

<span data-ttu-id="0bf32-125">A környezeti változók beállítása példát a [Azure Portal](https://portal.azure.com) egy Azure-webhelyre, lásd: [Node.js webalkalmazás az Azure Table szolgáltatással].</span><span class="sxs-lookup"><span data-stu-id="0bf32-125">For an example of setting the environment variables in the [Azure Portal](https://portal.azure.com) for an Azure Website, see [Node.js web app using the Azure Table Service].</span></span>

## <a name="how-to-create-a-queue"></a><span data-ttu-id="0bf32-126">Útmutató: A várólista létrehozása</span><span class="sxs-lookup"><span data-stu-id="0bf32-126">How To: Create a Queue</span></span>
<span data-ttu-id="0bf32-127">Az alábbi kód létrehoz egy **QueueService** objektum, amely lehetővé teszi a várólisták használata.</span><span class="sxs-lookup"><span data-stu-id="0bf32-127">The following code creates a **QueueService** object, which enables you to work with queues.</span></span>

```
var queueSvc = azure.createQueueService();
```

<span data-ttu-id="0bf32-128">Használja a **createQueueIfNotExists** metódus, amely a megadott várólista adja vissza, ha már létezik, vagy új várólista létrehozása a megadott névvel, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="0bf32-128">Use the **createQueueIfNotExists** method, which returns the specified queue if it already exists or creates a new queue with the specified name if it does not already exist.</span></span>

```
queueSvc.createQueueIfNotExists('myqueue', function(error, result, response){
  if(!error){
    // Queue created or exists
  }
});
```

<span data-ttu-id="0bf32-129">Ha a várólista létrejön, `result.created` értéke true.</span><span class="sxs-lookup"><span data-stu-id="0bf32-129">If the queue is created, `result.created` is true.</span></span> <span data-ttu-id="0bf32-130">Ha a várólista létezik, `result.created` értéke "false".</span><span class="sxs-lookup"><span data-stu-id="0bf32-130">If the queue exists, `result.created` is false.</span></span>

### <a name="filters"></a><span data-ttu-id="0bf32-131">Szűrők</span><span class="sxs-lookup"><span data-stu-id="0bf32-131">Filters</span></span>
<span data-ttu-id="0bf32-132">Választható szűrési műveletek használatával végrehajtott műveletek alkalmazhatók **QueueService**.</span><span class="sxs-lookup"><span data-stu-id="0bf32-132">Optional filtering operations can be applied to operations performed using **QueueService**.</span></span> <span data-ttu-id="0bf32-133">Műveletek szűrésének lehetnek naplózási, automatikus újrapróbálkozása, stb. A metódus aláírása megvalósító objektumok szűrők a következők:</span><span class="sxs-lookup"><span data-stu-id="0bf32-133">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with the signature:</span></span>

```
function handle (requestOptions, next)
```

<span data-ttu-id="0bf32-134">Ezután a előfeldolgozása kérelmet a beállítások, a metódus hívása a "Tovább" típusra a következő aláírással rendelkező visszahívás kell:</span><span class="sxs-lookup"><span data-stu-id="0bf32-134">After doing its preprocessing on the request options, the method needs to call "next" passing a callback with the following signature:</span></span>

```
function (returnObject, finalCallback, next)
```

<span data-ttu-id="0bf32-135">A visszahívási, és a returnObject (válasza a kérés a kiszolgáló) feldolgozása után a visszahívás kell mellett meghívni, ha más szűrők feldolgozásának folytatásához létezik, vagy egyszerűen meghívása finalCallback ellenkező esetben a szolgáltatás hívásához fog.</span><span class="sxs-lookup"><span data-stu-id="0bf32-135">In this callback, and after processing the returnObject (the response from the request to the server), the callback needs to either invoke next if it exists to continue processing other filters or simply invoke finalCallback otherwise to end up the service invocation.</span></span>

<span data-ttu-id="0bf32-136">Két szűrőket, amelyek megvalósítják az újrapróbálkozási logika érhetők el az Azure SDK for Node.js, a **ExponentialRetryPolicyFilter** és **LinearRetryPolicyFilter**.</span><span class="sxs-lookup"><span data-stu-id="0bf32-136">Two filters that implement retry logic are included with the Azure SDK for Node.js, **ExponentialRetryPolicyFilter** and **LinearRetryPolicyFilter**.</span></span> <span data-ttu-id="0bf32-137">A következő létrehoz egy **QueueService** objektum, amely használja a **ExponentialRetryPolicyFilter**:</span><span class="sxs-lookup"><span data-stu-id="0bf32-137">The following creates a **QueueService** object that uses the **ExponentialRetryPolicyFilter**:</span></span>

```
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var queueSvc = azure.createQueueService().withFilter(retryOperations);
```

## <a name="how-to-insert-a-message-into-a-queue"></a><span data-ttu-id="0bf32-138">Útmutató: A várólista üzenet beszúrása</span><span class="sxs-lookup"><span data-stu-id="0bf32-138">How To: Insert a Message into a Queue</span></span>
<span data-ttu-id="0bf32-139">Üzenet beszúrása egy üzenetsort, használja a **createMessage** módszert, hozzon létre egy új üzenetet, és vegye fel a várólistára.</span><span class="sxs-lookup"><span data-stu-id="0bf32-139">To insert a message into a queue, use the **createMessage** method to create a new message and add it to the queue.</span></span>

```
queueSvc.createMessage('myqueue', "Hello world!", function(error, result, response){
  if(!error){
    // Message inserted
  }
});
```

## <a name="how-to-peek-at-the-next-message"></a><span data-ttu-id="0bf32-140">Útmutató: A következő üzenet megtekintése</span><span class="sxs-lookup"><span data-stu-id="0bf32-140">How To: Peek at the Next Message</span></span>
<span data-ttu-id="0bf32-141">Is bepillanthat, hogy egy sor elején található üzenetbe anélkül, hogy eltávolítaná az üzenetsorból meghívásával a **peekMessages** metódust.</span><span class="sxs-lookup"><span data-stu-id="0bf32-141">You can peek at the message in the front of a queue without removing it from the queue by calling the **peekMessages** method.</span></span> <span data-ttu-id="0bf32-142">Alapértelmezés szerint **peekMessages** betekintés egyetlen üzenetben.</span><span class="sxs-lookup"><span data-stu-id="0bf32-142">By default, **peekMessages** peeks at a single message.</span></span>

```
queueSvc.peekMessages('myqueue', function(error, result, response){
  if(!error){
    // Message text is in messages[0].messageText
  }
});
```

<span data-ttu-id="0bf32-143">A `result` az üzenet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0bf32-143">The `result` contains the message.</span></span>

> [!NOTE]
> <span data-ttu-id="0bf32-144">Használatával **peekMessages** Ha nincsenek a várólistán lévő üzenetek nem hibát adnak vissza, azonban nem adja vissza.</span><span class="sxs-lookup"><span data-stu-id="0bf32-144">Using **peekMessages** when there are no messages in the queue will not return an error, however no messages will be returned.</span></span>
> 
> 

## <a name="how-to-dequeue-the-next-message"></a><span data-ttu-id="0bf32-145">Útmutató: A következő üzenet created</span><span class="sxs-lookup"><span data-stu-id="0bf32-145">How To: Dequeue the Next Message</span></span>
<span data-ttu-id="0bf32-146">Egy üzenet feldolgozása egy két lépésből álló folyamat:</span><span class="sxs-lookup"><span data-stu-id="0bf32-146">Processing a message is a two-stage process:</span></span>

1. <span data-ttu-id="0bf32-147">Az üzenet feldolgozásához.</span><span class="sxs-lookup"><span data-stu-id="0bf32-147">Dequeue the message.</span></span>
2. <span data-ttu-id="0bf32-148">Az üzenet törlése.</span><span class="sxs-lookup"><span data-stu-id="0bf32-148">Delete the message.</span></span>

<span data-ttu-id="0bf32-149">Egy üzenet feldolgozásához, használja a **getMessages**.</span><span class="sxs-lookup"><span data-stu-id="0bf32-149">To dequeue a message, use **getMessages**.</span></span> <span data-ttu-id="0bf32-150">Így az üzenetek nem látható a várólistán, így más ügyfelek nem képes feldolgozni azokat.</span><span class="sxs-lookup"><span data-stu-id="0bf32-150">This makes the messages invisible in the queue, so no other clients can process them.</span></span> <span data-ttu-id="0bf32-151">Az alkalmazás rendelkezik egy üzenet feldolgozása után hívja **deleteMessage** törli-e az üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="0bf32-151">Once your application has processed a message, call **deleteMessage** to delete it from the queue.</span></span> <span data-ttu-id="0bf32-152">A következő példa egy üzenetet kap, majd törli őket:</span><span class="sxs-lookup"><span data-stu-id="0bf32-152">The following example gets a message, then deletes it:</span></span>

```
queueSvc.getMessages('myqueue', function(error, result, response){
  if(!error){
    // Message text is in messages[0].messageText
    var message = result[0];
    queueSvc.deleteMessage('myqueue', message.messageId, message.popReceipt, function(error, response){
      if(!error){
        //message deleted
      }
    });
  }
});
```

> [!NOTE]
> <span data-ttu-id="0bf32-153">Alapértelmezés szerint az üzenet csak rejtett 30 másodpercig, amely után is látható, más ügyfelek számára.</span><span class="sxs-lookup"><span data-stu-id="0bf32-153">By default, a message is only hidden for 30 seconds, after which it is visible to other clients.</span></span> <span data-ttu-id="0bf32-154">A jelenlegitől eltérő értéket is megadhat `options.visibilityTimeout` rendelkező **getMessages**.</span><span class="sxs-lookup"><span data-stu-id="0bf32-154">You can specify a different value by using `options.visibilityTimeout` with **getMessages**.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="0bf32-155">Használatával **getMessages** Ha nincsenek a várólistán lévő üzenetek nem hibát adnak vissza, azonban nem adja vissza.</span><span class="sxs-lookup"><span data-stu-id="0bf32-155">Using **getMessages** when there are no messages in the queue will not return an error, however no messages will be returned.</span></span>
> 
> 

## <a name="how-to-change-the-contents-of-a-queued-message"></a><span data-ttu-id="0bf32-156">Útmutató: Az aszinkron üzenet tartalmának módosítása</span><span class="sxs-lookup"><span data-stu-id="0bf32-156">How To: Change the Contents of a Queued Message</span></span>
<span data-ttu-id="0bf32-157">Módosíthatja a tartalmát egy üzenet helyben a várólista használatával **updateMessage**.</span><span class="sxs-lookup"><span data-stu-id="0bf32-157">You can change the contents of a message in-place in the queue using **updateMessage**.</span></span> <span data-ttu-id="0bf32-158">Az alábbi példa frissíti a szöveges üzenet:</span><span class="sxs-lookup"><span data-stu-id="0bf32-158">The following example updates the text of a message:</span></span>

```
queueSvc.getMessages('myqueue', function(error, result, response){
  if(!error){
    // Got the message
    var message = result[0];
    queueSvc.updateMessage('myqueue', message.messageId, message.popReceipt, 10, {messageText: 'new text'}, function(error, result, response){
      if(!error){
        // Message updated successfully
      }
    });
  }
});
```

## <a name="how-to-additional-options-for-dequeuing-messages"></a><span data-ttu-id="0bf32-159">Útmutató: További beállítások üzenetmozgatót üzenetek</span><span class="sxs-lookup"><span data-stu-id="0bf32-159">How To: Additional Options for Dequeuing Messages</span></span>
<span data-ttu-id="0bf32-160">Két módon szabhatja testre az üzenetek lekérését egy üzenetsorból:</span><span class="sxs-lookup"><span data-stu-id="0bf32-160">There are two ways you can customize message retrieval from a queue:</span></span>

* <span data-ttu-id="0bf32-161">`options.numOfMessages`-Beolvasása az üzenetkötegek (legfeljebb 32.)</span><span class="sxs-lookup"><span data-stu-id="0bf32-161">`options.numOfMessages` - Retrieve a batch of messages (up to 32.)</span></span>
* <span data-ttu-id="0bf32-162">`options.visibilityTimeout`– Állítsa be a hosszabb vagy rövidebb láthatatlansági időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="0bf32-162">`options.visibilityTimeout` - Set a longer or shorter invisibility timeout.</span></span>

<span data-ttu-id="0bf32-163">Az alábbi példában a **getMessages** módszer segítségével 15 üzenetek egy hívásban.</span><span class="sxs-lookup"><span data-stu-id="0bf32-163">The following example uses the **getMessages** method to get 15 messages in one call.</span></span> <span data-ttu-id="0bf32-164">Ezután minden üzenetet használatával feldolgozza a hurok.</span><span class="sxs-lookup"><span data-stu-id="0bf32-164">Then it processes each message using a for loop.</span></span> <span data-ttu-id="0bf32-165">Ez a metódus által visszaadott összes üzenet öt perc értékre állítja a láthatatlansági időkorlátot is.</span><span class="sxs-lookup"><span data-stu-id="0bf32-165">It also sets the invisibility timeout to five minutes for all messages returned by this method.</span></span>

```
queueSvc.getMessages('myqueue', {numOfMessages: 15, visibilityTimeout: 5 * 60}, function(error, result, response){
  if(!error){
    // Messages retreived
    for(var index in result){
      // text is available in result[index].messageText
      var message = result[index];
      queueSvc.deleteMessage(queueName, message.messageId, message.popReceipt, function(error, response){
        if(!error){
          // Message deleted
        }
      });
    }
  }
});
```

## <a name="how-to-get-the-queue-length"></a><span data-ttu-id="0bf32-166">Útmutató: Az üzenetsor hosszának lekérése</span><span class="sxs-lookup"><span data-stu-id="0bf32-166">How To: Get the Queue Length</span></span>
<span data-ttu-id="0bf32-167">A **getQueueMetadata** metaadatainak beolvasása, beleértve a várólistán üzenetek hozzávetőleges számát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="0bf32-167">The **getQueueMetadata** returns metadata about the queue, including the approximate number of messages waiting in the queue.</span></span>

```
queueSvc.getQueueMetadata('myqueue', function(error, result, response){
  if(!error){
    // Queue length is available in result.approximateMessageCount
  }
});
```

## <a name="how-to-list-queues"></a><span data-ttu-id="0bf32-168">Útmutató: A lista várólisták</span><span class="sxs-lookup"><span data-stu-id="0bf32-168">How To: List Queues</span></span>
<span data-ttu-id="0bf32-169">Várólisták listájának lekéréséhez használja **listQueuesSegmented**.</span><span class="sxs-lookup"><span data-stu-id="0bf32-169">To retrieve a list of queues, use **listQueuesSegmented**.</span></span> <span data-ttu-id="0bf32-170">Egy adott előtag alapján szűrt listáját olvashatja, **listQueuesSegmentedWithPrefix**.</span><span class="sxs-lookup"><span data-stu-id="0bf32-170">To retrieve a list filtered by a specific prefix, use **listQueuesSegmentedWithPrefix**.</span></span>

```
queueSvc.listQueuesSegmented(null, function(error, result, response){
  if(!error){
    // result.entries contains the list of queues
  }
});
```

<span data-ttu-id="0bf32-171">Az összes várólistán nem adható vissza, ha `result.continuationToken` első paramétere használható **listQueuesSegmented** vagy a második paraméter **listQueuesSegmentedWithPrefix** további eredmények beolvasásához.</span><span class="sxs-lookup"><span data-stu-id="0bf32-171">If all queues cannot be returned, `result.continuationToken` can be used as the first parameter of **listQueuesSegmented** or the second parameter of **listQueuesSegmentedWithPrefix** to retrieve more results.</span></span>

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="0bf32-172">Útmutató: A várólista törlése</span><span class="sxs-lookup"><span data-stu-id="0bf32-172">How To: Delete a Queue</span></span>
<span data-ttu-id="0bf32-173">Egy üzenetsor és a benne tárolt összes üzenet törléséhez hívja meg a **deleteQueue** a várólista-objektum metódust.</span><span class="sxs-lookup"><span data-stu-id="0bf32-173">To delete a queue and all the messages contained in it, call the **deleteQueue** method on the queue object.</span></span>

```
queueSvc.deleteQueue(queueName, function(error, response){
  if(!error){
    // Queue has been deleted
  }
});
```

<span data-ttu-id="0bf32-174">Törlés nélkül törli az összes üzenetet az üzenetsorból, használjon **clearMessages**.</span><span class="sxs-lookup"><span data-stu-id="0bf32-174">To clear all messages from a queue without deleting it, use **clearMessages**.</span></span>

## <a name="how-to-work-with-shared-access-signatures"></a><span data-ttu-id="0bf32-175">Hogyan: közös hozzáférésű Jogosultságkód használata</span><span class="sxs-lookup"><span data-stu-id="0bf32-175">How to: Work with Shared Access Signatures</span></span>
<span data-ttu-id="0bf32-176">Megosztott hozzáférési aláírásokkal (SAS), amelyek egy biztonságos várólisták részletes hozzáférést biztosítanak a tárfiók neve vagy a kulcsok megadása nélkül.</span><span class="sxs-lookup"><span data-stu-id="0bf32-176">Shared Access Signatures (SAS) are a secure way to provide granular access to queues without providing your storage account name or keys.</span></span> <span data-ttu-id="0bf32-177">SAS korlátozott hozzáférést biztosít a várólisták, például egy mobil alkalmazás üzeneteket küldhet gyakran használják.</span><span class="sxs-lookup"><span data-stu-id="0bf32-177">SAS are often used to provide limited access to your queues, such as allowing a mobile app to submit messages.</span></span>

<span data-ttu-id="0bf32-178">A megbízható alkalmazások, például egy felhőalapú szolgáltatás létrehoz egy SAS használatával a **generateSharedAccessSignature** , a **QueueService**, és a nem megbízható vagy félig megbízható alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="0bf32-178">A trusted application such as a cloud-based service generates a SAS using the **generateSharedAccessSignature** of the **QueueService**, and provides it to an untrusted or semi-trusted application.</span></span> <span data-ttu-id="0bf32-179">Ha például egy mobilalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0bf32-179">For example, a mobile app.</span></span> <span data-ttu-id="0bf32-180">A biztonsági Társítások létrejön egy házirendet, amely a kezdő és záró dátumát, amely alatt a SAS érvénytelen ismerteti, valamint a hozzáférési szintet a biztonsági Társítások jogosult kap.</span><span class="sxs-lookup"><span data-stu-id="0bf32-180">The SAS is generated using a policy, which describes the start and end dates during which the SAS is valid, as well as the access level granted to the SAS holder.</span></span>

<span data-ttu-id="0bf32-181">Az alábbi példa létrehoz egy új megosztott elérési házirendet, amely lehetővé teszi a biztonsági Társítások jogosult üzenetek hozzáadása a várólista, és 100 perc létrehozása után lejár.</span><span class="sxs-lookup"><span data-stu-id="0bf32-181">The following example generates a new shared access policy that will allow the SAS holder to add messages to the queue, and expires 100 minutes after the time it is created.</span></span>

```
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};

var queueSAS = queueSvc.generateSharedAccessSignature('myqueue', sharedAccessPolicy);
var host = queueSvc.host;
```

<span data-ttu-id="0bf32-182">Vegye figyelembe, hogy az állomás információit meg kell adni is, mivel esetén megadása kötelező a biztonsági Társítások jogosult megpróbál hozzáférni a várólista.</span><span class="sxs-lookup"><span data-stu-id="0bf32-182">Note that the host information must be provided also, as it is required when the SAS holder attempts to access the queue.</span></span>

<span data-ttu-id="0bf32-183">Az ügyfélalkalmazás majd használja a biztonsági Társításait **QueueServiceWithSAS** szemben a várólista műveletek végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="0bf32-183">The client application then uses the SAS with **QueueServiceWithSAS** to perform operations against the queue.</span></span> <span data-ttu-id="0bf32-184">A következő példa a várólista csatlakozik, és létrehoz egy üzenetet.</span><span class="sxs-lookup"><span data-stu-id="0bf32-184">The following example connects to the queue and creates a message.</span></span>

```
var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
  if(!error){
    //message added
  }
});
```

<span data-ttu-id="0bf32-185">A SAS létre lett hozva a hozzáférés hozzáadása, ha volt kísérlet olvasása, frissítése vagy törli az üzenetet, mert akkor kell hibát jelez.</span><span class="sxs-lookup"><span data-stu-id="0bf32-185">Since the SAS was generated with add access, if an attempt were made to read, update or delete messages, an error would be returned.</span></span>

### <a name="access-control-lists"></a><span data-ttu-id="0bf32-186">Hozzáférés-vezérlési listák</span><span class="sxs-lookup"><span data-stu-id="0bf32-186">Access control lists</span></span>
<span data-ttu-id="0bf32-187">Egy hozzáférés-vezérlési lista (ACL) segítségével állítsa be a hozzáférési házirend tartozó SAS korlátozására.</span><span class="sxs-lookup"><span data-stu-id="0bf32-187">You can also use an Access Control List (ACL) to set the access policy for a SAS.</span></span> <span data-ttu-id="0bf32-188">Ez akkor hasznos, ha a várólista hozzáférés, de különböző hozzáférési házirendek biztosít az egyes ügyfelek több ügyféllel engedélyez.</span><span class="sxs-lookup"><span data-stu-id="0bf32-188">This is useful if you wish to allow multiple clients to access the queue, but provide different access policies for each client.</span></span>

<span data-ttu-id="0bf32-189">Hozzáférés-vezérlési Listában hozzáférési házirendeket, tömbje segítségével minden házirendhez társított azonosítójú van megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="0bf32-189">An ACL is implemented using an array of access policies, with an ID associated with each policy.</span></span> <span data-ttu-id="0bf32-190">A következő példában két szabályzatokat; határoz meg. egy "felhasználó1" és "felhasználó2":</span><span class="sxs-lookup"><span data-stu-id="0bf32-190">The  following example defines two policies; one for 'user1' and one for 'user2':</span></span>

```
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.QueueUtilities.SharedAccessPermissions.PROCESS,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

<span data-ttu-id="0bf32-191">Az alábbi példa lekérdezi az aktuális hozzáférés-Vezérlési **Várólista_neve**, majd hozzáadja az új házirendek **setQueueAcl**.</span><span class="sxs-lookup"><span data-stu-id="0bf32-191">The following example gets the current ACL for **myqueue**, then adds the new policies using **setQueueAcl**.</span></span> <span data-ttu-id="0bf32-192">Ez a megközelítés lehetővé teszi, hogy:</span><span class="sxs-lookup"><span data-stu-id="0bf32-192">This approach allows:</span></span>

```
var extend = require('extend');
queueSvc.getQueueAcl('myqueue', function(error, result, response) {
  if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    queueSvc.setQueueAcl('myqueue', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

<span data-ttu-id="0bf32-193">Miután a hozzáférés-vezérlési lista van beállítva, majd a házirend-azonosító alapján SAS hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="0bf32-193">Once the ACL has been set, you can then create a SAS based on the ID for a policy.</span></span> <span data-ttu-id="0bf32-194">Az alábbi példa létrehoz egy új SAS-kód "felhasználó2":</span><span class="sxs-lookup"><span data-stu-id="0bf32-194">The following example creates a new SAS for 'user2':</span></span>

```
queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });
```

## <a name="next-steps"></a><span data-ttu-id="0bf32-195">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0bf32-195">Next Steps</span></span>
<span data-ttu-id="0bf32-196">Most, hogy megismerte a queue storage alapjait, az alábbi hivatkozásokból tájékozódhat az összetettebb tárolási feladatok elvégzéséről.</span><span class="sxs-lookup"><span data-stu-id="0bf32-196">Now that you've learned the basics of queue storage, follow these links to learn about more complex storage tasks.</span></span>

* <span data-ttu-id="0bf32-197">Látogasson el a [Azure Storage csapat blogja] [Azure Storage csapat blogja].</span><span class="sxs-lookup"><span data-stu-id="0bf32-197">Visit the [Azure Storage Team Blog][Azure Storage Team Blog].</span></span>
* <span data-ttu-id="0bf32-198">Látogasson el a [Azure Storage szolgáltatás SDK csomópont] [ Azure Storage SDK for Node] GitHub tárházából.</span><span class="sxs-lookup"><span data-stu-id="0bf32-198">Visit the [Azure Storage SDK for Node][Azure Storage SDK for Node] repository on GitHub.</span></span>

[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node
[using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure Portal]: https://portal.azure.com<span data-ttu-id="0bf32-199">
[Node.js-webalkalmazás létrehozása az Azure App Service-ben](../../app-service-web/app-service-web-get-started-nodejs.md) </span><span class="sxs-lookup"><span data-stu-id="0bf32-199">
[Create a Node.js web app in Azure App Service](../../app-service-web/app-service-web-get-started-nodejs.md) </span></span>  
<span data-ttu-id="0bf32-200">[Az Azure Table szolgáltatás használata node.js-webalkalmazás](../../app-service-web/storage-nodejs-use-table-storage-web-site.md)
</span><span class="sxs-lookup"><span data-stu-id="0bf32-200">[Node.js web app using the Azure Table Service](../../app-service-web/storage-nodejs-use-table-storage-web-site.md)
</span></span>  


<span data-ttu-id="0bf32-201">[Hozza létre és Azure Cloud Service a Node.js-alkalmazás központi telepítése](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) </span><span class="sxs-lookup"><span data-stu-id="0bf32-201">[Build and deploy a Node.js application to an Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) </span></span>  
<span data-ttu-id="0bf32-202">[Az azure Storage csapat blogja]: http://blogs.msdn.com/b/windowsazurestorage/ [Build és a Node.js webalkalmazás telepítése az Azure-ban a Web Matrix]: https://www.microsoft.com/web/webmatrix/</span><span class="sxs-lookup"><span data-stu-id="0bf32-202">[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/ [Build and deploy a Node.js web app to Azure using Web Matrix]: https://www.microsoft.com/web/webmatrix/</span></span>   
