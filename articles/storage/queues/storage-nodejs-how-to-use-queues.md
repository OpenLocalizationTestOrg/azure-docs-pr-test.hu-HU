---
title: "Node.js-ből a Queue storage aaaHow toouse |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure Queue szolgáltatás toocreate és a delete várólisták és a Beszúrás, get, és törli az üzenetet. A minta Node.js nyelven írt."
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
ms.openlocfilehash: 7e9778da4efa69f2e9d8fd480b9b6f5ace85951e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-nodejs"></a><span data-ttu-id="e566b-104">Hogyan toouse a Queue storage Node.js-ből</span><span class="sxs-lookup"><span data-stu-id="e566b-104">How toouse Queue storage from Node.js</span></span>
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a><span data-ttu-id="e566b-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="e566b-105">Overview</span></span>
<span data-ttu-id="e566b-106">Ez az útmutató bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz hello Microsoft Azure Queue szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="e566b-106">This guide shows you how tooperform common scenarios using hello Microsoft Azure Queue service.</span></span> <span data-ttu-id="e566b-107">hello minták hello Node.js API segítségével készül.</span><span class="sxs-lookup"><span data-stu-id="e566b-107">hello samples are written using hello Node.js API.</span></span> <span data-ttu-id="e566b-108">hello tárgyalt forgatókönyvekben szerepel a **beszúrása**, **megtekintésekor**, **első**, és **törlése** üzenetek, várólista, valamint  **létrehozása és törlése várólisták**.</span><span class="sxs-lookup"><span data-stu-id="e566b-108">hello scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating and deleting queues**.</span></span>

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a><span data-ttu-id="e566b-109">Node.js-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e566b-109">Create a Node.js Application</span></span>
<span data-ttu-id="e566b-110">Üres Node.js-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e566b-110">Create a blank Node.js application.</span></span> <span data-ttu-id="e566b-111">A Node.js-alkalmazás létrehozása utasításokért lásd: [Node.js-webalkalmazás létrehozása az Azure App Service](../../app-service-web/app-service-web-get-started-nodejs.md), [és üzembe egy Node.js-alkalmazás tooan Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) a Windows PowerShell vagy [ Hozza létre és telepítheti a Node.js web app tooAzure Web Matrix](https://www.microsoft.com/web/webmatrix/).</span><span class="sxs-lookup"><span data-stu-id="e566b-111">For instructions creating a Node.js application, see [Create a Node.js web app in Azure App Service](../../app-service-web/app-service-web-get-started-nodejs.md), [Build and deploy a Node.js application tooan Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) using Windows PowerShell, or [Build and deploy a Node.js web app tooAzure using Web Matrix](https://www.microsoft.com/web/webmatrix/).</span></span>

## <a name="configure-your-application-tooaccess-storage"></a><span data-ttu-id="e566b-112">Alkalmazás tooAccess tároló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e566b-112">Configure Your Application tooAccess Storage</span></span>
<span data-ttu-id="e566b-113">az Azure storage toouse, kell hello Azure Storage szolgáltatás SDK a Node.js, amely kényelmi szalagtárak hello tárolószolgáltatások REST kommunikációt készletét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="e566b-113">toouse Azure storage, you need hello Azure Storage SDK for Node.js, which includes a set of convenience libraries that communicate with hello storage REST services.</span></span>

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a><span data-ttu-id="e566b-114">Csomópont Package Manager (NPM) tooobtain hello csomag használata</span><span class="sxs-lookup"><span data-stu-id="e566b-114">Use Node Package Manager (NPM) tooobtain hello package</span></span>
1. <span data-ttu-id="e566b-115">Használjon például egy parancssori felületet **PowerShell** (Windows), **Terminálszolgáltatások** (Mac) vagy **Bash** (Unix), keresse meg a toohello mappa, amelyben létrehozta a mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e566b-115">Use a command-line interface such as **PowerShell** (Windows,) **Terminal** (Mac,) or **Bash** (Unix), navigate toohello folder where you created your sample application.</span></span>
2. <span data-ttu-id="e566b-116">Típus **npm telepítése azure-tároló** hello parancssori ablakban.</span><span class="sxs-lookup"><span data-stu-id="e566b-116">Type **npm install azure-storage** in hello command window.</span></span> <span data-ttu-id="e566b-117">Hello parancs kimenete a következő példa hasonló toohello.</span><span class="sxs-lookup"><span data-stu-id="e566b-117">Output from hello command is similar toohello following example.</span></span>
 
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

3. <span data-ttu-id="e566b-118">Manuális futtatásával hello **ls** parancs tooverify, amely egy **csomópont\_modulok** mappa hozták létre.</span><span class="sxs-lookup"><span data-stu-id="e566b-118">You can manually run hello **ls** command tooverify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="e566b-119">Mappán belül található hello **azure-tároló** csomagot, amely tartalmazza a hello szalagtárak kell érniük a tárhelyet.</span><span class="sxs-lookup"><span data-stu-id="e566b-119">Inside that folder you will find hello **azure-storage** package, which contains hello libraries you need to access storage.</span></span>

### <a name="import-hello-package"></a><span data-ttu-id="e566b-120">Hello csomag importálása</span><span class="sxs-lookup"><span data-stu-id="e566b-120">Import hello package</span></span>
<span data-ttu-id="e566b-121">A Jegyzettömbben vagy más szövegszerkesztőben, adja hozzá a következő toohello felső hello a **server.js** toouse tárolási célhelyeként hello alkalmazás fájlt:</span><span class="sxs-lookup"><span data-stu-id="e566b-121">Using Notepad or another text editor, add hello following toohello top the **server.js** file of hello application where you intend toouse storage:</span></span>

```
var azure = require('azure-storage');
```

## <a name="setup-an-azure-storage-connection"></a><span data-ttu-id="e566b-122">Az Azure Storage-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="e566b-122">Setup an Azure Storage Connection</span></span>
<span data-ttu-id="e566b-123">hello azure modul olvassák hello környezeti változók AZURE\_tárolási\_FIÓKOT és az AZURE\_tárolási\_hozzáférés\_kulcs, vagy AZURE\_tárolási\_kapcsolat \_Szükséges adatokat tooconnect tooyour Azure storage-fiók KARAKTERLÁNCOT.</span><span class="sxs-lookup"><span data-stu-id="e566b-123">hello azure module will read hello environment variables AZURE\_STORAGE\_ACCOUNT and AZURE\_STORAGE\_ACCESS\_KEY, or AZURE\_STORAGE\_CONNECTION\_STRING for information required tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="e566b-124">Ha ezek a környezeti változók nem, meg kell adnia hello fiók meghívásakor **createQueueService**.</span><span class="sxs-lookup"><span data-stu-id="e566b-124">If these environment variables are not set, you must specify hello account information when calling **createQueueService**.</span></span>

<span data-ttu-id="e566b-125">Példa a hello hello környezeti változók beállítása [Azure Portal](https://portal.azure.com) egy Azure-webhelyre, lásd: [Node.js web app használatával hello Azure Table szolgáltatás].</span><span class="sxs-lookup"><span data-stu-id="e566b-125">For an example of setting hello environment variables in hello [Azure Portal](https://portal.azure.com) for an Azure Website, see [Node.js web app using hello Azure Table Service].</span></span>

## <a name="how-to-create-a-queue"></a><span data-ttu-id="e566b-126">Útmutató: A várólista létrehozása</span><span class="sxs-lookup"><span data-stu-id="e566b-126">How To: Create a Queue</span></span>
<span data-ttu-id="e566b-127">hello alábbi kód létrehoz egy **QueueService** objektum, amely lehetővé teszi az üzenetsorok toowork.</span><span class="sxs-lookup"><span data-stu-id="e566b-127">hello following code creates a **QueueService** object, which enables you toowork with queues.</span></span>

```
var queueSvc = azure.createQueueService();
```

<span data-ttu-id="e566b-128">Használjon hello **createQueueIfNotExists** metódus, amely a megadott várólista hello adja vissza, ha már létezik, vagy létrehoz egy új üzenetsort hello megadott névvel, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="e566b-128">Use hello **createQueueIfNotExists** method, which returns hello specified queue if it already exists or creates a new queue with hello specified name if it does not already exist.</span></span>

```
queueSvc.createQueueIfNotExists('myqueue', function(error, result, response){
  if(!error){
    // Queue created or exists
  }
});
```

<span data-ttu-id="e566b-129">Ha hello várólista létrehozása `result.created` értéke true.</span><span class="sxs-lookup"><span data-stu-id="e566b-129">If hello queue is created, `result.created` is true.</span></span> <span data-ttu-id="e566b-130">Ha hello várólista létezik, `result.created` értéke "false".</span><span class="sxs-lookup"><span data-stu-id="e566b-130">If hello queue exists, `result.created` is false.</span></span>

### <a name="filters"></a><span data-ttu-id="e566b-131">Szűrők</span><span class="sxs-lookup"><span data-stu-id="e566b-131">Filters</span></span>
<span data-ttu-id="e566b-132">Választható szűrési műveletek lehet segítségével alkalmazott toooperations **QueueService**.</span><span class="sxs-lookup"><span data-stu-id="e566b-132">Optional filtering operations can be applied toooperations performed using **QueueService**.</span></span> <span data-ttu-id="e566b-133">Műveletek szűrésének lehetnek naplózási, automatikus újrapróbálkozása, stb. Szűrők hello aláírással metódus megvalósító objektumok a következők:</span><span class="sxs-lookup"><span data-stu-id="e566b-133">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with hello signature:</span></span>

```
function handle (requestOptions, next)
```

<span data-ttu-id="e566b-134">Ezután a előfeldolgozása hello kérelem lehetőségekről, hello metódus van szüksége a "Tovább" gombra. átadja a aláírását hello egy visszahívási toocall:</span><span class="sxs-lookup"><span data-stu-id="e566b-134">After doing its preprocessing on hello request options, hello method needs toocall "next" passing a callback with hello following signature:</span></span>

```
function (returnObject, finalCallback, next)
```

<span data-ttu-id="e566b-135">A visszahívási és hello returnObject (hello hello kérelem toohello kiszolgálótól kapott válasz) feldolgozása után hello visszahívási kell tooeither következő meghívni, ha más szűrők feldolgozása toocontinue létezik, vagy egyszerűen a finalCallback meghívása egyéb tooend hello mentése szolgáltatás hívása.</span><span class="sxs-lookup"><span data-stu-id="e566b-135">In this callback, and after processing hello returnObject (hello response from hello request toohello server), hello callback needs tooeither invoke next if it exists toocontinue processing other filters or simply invoke finalCallback otherwise tooend up hello service invocation.</span></span>

<span data-ttu-id="e566b-136">Két szűrőket, amelyek megvalósítják az újrapróbálkozási logika érhetők el a hello Azure SDK for Node.js, **ExponentialRetryPolicyFilter** és **LinearRetryPolicyFilter**.</span><span class="sxs-lookup"><span data-stu-id="e566b-136">Two filters that implement retry logic are included with hello Azure SDK for Node.js, **ExponentialRetryPolicyFilter** and **LinearRetryPolicyFilter**.</span></span> <span data-ttu-id="e566b-137">hello következő létrehoz egy **QueueService** hello használó objektum **ExponentialRetryPolicyFilter**:</span><span class="sxs-lookup"><span data-stu-id="e566b-137">hello following creates a **QueueService** object that uses hello **ExponentialRetryPolicyFilter**:</span></span>

```
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var queueSvc = azure.createQueueService().withFilter(retryOperations);
```

## <a name="how-to-insert-a-message-into-a-queue"></a><span data-ttu-id="e566b-138">Útmutató: A várólista üzenet beszúrása</span><span class="sxs-lookup"><span data-stu-id="e566b-138">How To: Insert a Message into a Queue</span></span>
<span data-ttu-id="e566b-139">a várólistán, használjon hello üzenet tooinsert **createMessage** hozzon létre egy új üzenetet, és adja hozzá toohello várólista módszert.</span><span class="sxs-lookup"><span data-stu-id="e566b-139">tooinsert a message into a queue, use hello **createMessage** method to create a new message and add it toohello queue.</span></span>

```
queueSvc.createMessage('myqueue', "Hello world!", function(error, result, response){
  if(!error){
    // Message inserted
  }
});
```

## <a name="how-to-peek-at-hello-next-message"></a><span data-ttu-id="e566b-140">Útmutató: A következő üzenetet hello betekintés</span><span class="sxs-lookup"><span data-stu-id="e566b-140">How To: Peek at hello Next Message</span></span>
<span data-ttu-id="e566b-141">Eltávolítása hello által hívó hello anélkül is bepillanthat hello betekintés a várólista elejére hello **peekMessages** metódust.</span><span class="sxs-lookup"><span data-stu-id="e566b-141">You can peek at hello message in hello front of a queue without removing it from hello queue by calling hello **peekMessages** method.</span></span> <span data-ttu-id="e566b-142">Alapértelmezés szerint **peekMessages** betekintés egyetlen üzenetben.</span><span class="sxs-lookup"><span data-stu-id="e566b-142">By default, **peekMessages** peeks at a single message.</span></span>

```
queueSvc.peekMessages('myqueue', function(error, result, response){
  if(!error){
    // Message text is in messages[0].messageText
  }
});
```

<span data-ttu-id="e566b-143">Hello `result` üdvözlőüzenetére tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="e566b-143">hello `result` contains hello message.</span></span>

> [!NOTE]
> <span data-ttu-id="e566b-144">Használatával **peekMessages** Ha nincsenek hello várólistában lévő üzenetek nem hibát adnak vissza, azonban nem adja vissza.</span><span class="sxs-lookup"><span data-stu-id="e566b-144">Using **peekMessages** when there are no messages in hello queue will not return an error, however no messages will be returned.</span></span>
> 
> 

## <a name="how-to-dequeue-hello-next-message"></a><span data-ttu-id="e566b-145">Útmutató: A következő üzenetet hello created</span><span class="sxs-lookup"><span data-stu-id="e566b-145">How To: Dequeue hello Next Message</span></span>
<span data-ttu-id="e566b-146">Egy üzenet feldolgozása egy két lépésből álló folyamat:</span><span class="sxs-lookup"><span data-stu-id="e566b-146">Processing a message is a two-stage process:</span></span>

1. <span data-ttu-id="e566b-147">Created üdvözlőüzenetére.</span><span class="sxs-lookup"><span data-stu-id="e566b-147">Dequeue hello message.</span></span>
2. <span data-ttu-id="e566b-148">Hello üzenet törlése.</span><span class="sxs-lookup"><span data-stu-id="e566b-148">Delete hello message.</span></span>

<span data-ttu-id="e566b-149">egy üzenet toodequeue használja **getMessages**.</span><span class="sxs-lookup"><span data-stu-id="e566b-149">toodequeue a message, use **getMessages**.</span></span> <span data-ttu-id="e566b-150">Így köszönőüzenetei láthatatlan hello várólista, így más ügyfelek nem képes feldolgozni azokat.</span><span class="sxs-lookup"><span data-stu-id="e566b-150">This makes hello messages invisible in hello queue, so no other clients can process them.</span></span> <span data-ttu-id="e566b-151">Az alkalmazás rendelkezik egy üzenet feldolgozása után hívja **deleteMessage** toodelete hello várólista le.</span><span class="sxs-lookup"><span data-stu-id="e566b-151">Once your application has processed a message, call **deleteMessage** toodelete it from hello queue.</span></span> <span data-ttu-id="e566b-152">a következő példa hello lekérdezi egy üzenetet, majd törli őket:</span><span class="sxs-lookup"><span data-stu-id="e566b-152">hello following example gets a message, then deletes it:</span></span>

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
> <span data-ttu-id="e566b-153">Alapértelmezés szerint az üzenet csak rejtett 30 másodpercig, amely után látható tooother ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="e566b-153">By default, a message is only hidden for 30 seconds, after which it is visible tooother clients.</span></span> <span data-ttu-id="e566b-154">A jelenlegitől eltérő értéket is megadhat `options.visibilityTimeout` rendelkező **getMessages**.</span><span class="sxs-lookup"><span data-stu-id="e566b-154">You can specify a different value by using `options.visibilityTimeout` with **getMessages**.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="e566b-155">Használatával **getMessages** Ha nincsenek hello várólistában lévő üzenetek nem hibát adnak vissza, azonban nem adja vissza.</span><span class="sxs-lookup"><span data-stu-id="e566b-155">Using **getMessages** when there are no messages in hello queue will not return an error, however no messages will be returned.</span></span>
> 
> 

## <a name="how-to-change-hello-contents-of-a-queued-message"></a><span data-ttu-id="e566b-156">Útmutató: Módosítsa az sorba állított üzenetek hello tartalma</span><span class="sxs-lookup"><span data-stu-id="e566b-156">How To: Change hello Contents of a Queued Message</span></span>
<span data-ttu-id="e566b-157">Módosíthatja egy üzenet helyben hello várólista használatával hello tartalmát **updateMessage**.</span><span class="sxs-lookup"><span data-stu-id="e566b-157">You can change hello contents of a message in-place in hello queue using **updateMessage**.</span></span> <span data-ttu-id="e566b-158">a következő példa frissítések hello szöveges üzenet hello:</span><span class="sxs-lookup"><span data-stu-id="e566b-158">hello following example updates hello text of a message:</span></span>

```
queueSvc.getMessages('myqueue', function(error, result, response){
  if(!error){
    // Got hello message
    var message = result[0];
    queueSvc.updateMessage('myqueue', message.messageId, message.popReceipt, 10, {messageText: 'new text'}, function(error, result, response){
      if(!error){
        // Message updated successfully
      }
    });
  }
});
```

## <a name="how-to-additional-options-for-dequeuing-messages"></a><span data-ttu-id="e566b-159">Útmutató: További beállítások üzenetmozgatót üzenetek</span><span class="sxs-lookup"><span data-stu-id="e566b-159">How To: Additional Options for Dequeuing Messages</span></span>
<span data-ttu-id="e566b-160">Két módon szabhatja testre az üzenetek lekérését egy üzenetsorból:</span><span class="sxs-lookup"><span data-stu-id="e566b-160">There are two ways you can customize message retrieval from a queue:</span></span>

* <span data-ttu-id="e566b-161">`options.numOfMessages`-Beolvasása az üzenetkötegek (mentése too32.)</span><span class="sxs-lookup"><span data-stu-id="e566b-161">`options.numOfMessages` - Retrieve a batch of messages (up too32.)</span></span>
* <span data-ttu-id="e566b-162">`options.visibilityTimeout`– Állítsa be a hosszabb vagy rövidebb láthatatlansági időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="e566b-162">`options.visibilityTimeout` - Set a longer or shorter invisibility timeout.</span></span>

<span data-ttu-id="e566b-163">hello alábbi példában hello **getMessages** metódus tooget 15 üzenetek egy hívásban.</span><span class="sxs-lookup"><span data-stu-id="e566b-163">hello following example uses hello **getMessages** method tooget 15 messages in one call.</span></span> <span data-ttu-id="e566b-164">Ezután minden üzenetet használatával feldolgozza a hurok.</span><span class="sxs-lookup"><span data-stu-id="e566b-164">Then it processes each message using a for loop.</span></span> <span data-ttu-id="e566b-165">Beállítja a hello láthatatlansági időtúllépés toofive percig, amíg ez a metódus által visszaadott összes üzenetet is.</span><span class="sxs-lookup"><span data-stu-id="e566b-165">It also sets hello invisibility timeout toofive minutes for all messages returned by this method.</span></span>

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

## <a name="how-to-get-hello-queue-length"></a><span data-ttu-id="e566b-166">How To: Get hello várólistájának hossza</span><span class="sxs-lookup"><span data-stu-id="e566b-166">How To: Get hello Queue Length</span></span>
<span data-ttu-id="e566b-167">Hello **getQueueMetadata** hello várólista, beleértve a hello hozzávetőleges hello várólistáján lévő üzenetek száma kapcsolatos metaadatokat adja vissza.</span><span class="sxs-lookup"><span data-stu-id="e566b-167">hello **getQueueMetadata** returns metadata about hello queue, including hello approximate number of messages waiting in hello queue.</span></span>

```
queueSvc.getQueueMetadata('myqueue', function(error, result, response){
  if(!error){
    // Queue length is available in result.approximateMessageCount
  }
});
```

## <a name="how-to-list-queues"></a><span data-ttu-id="e566b-168">Útmutató: A lista várólisták</span><span class="sxs-lookup"><span data-stu-id="e566b-168">How To: List Queues</span></span>
<span data-ttu-id="e566b-169">a várólisták, használja listáját tooretrieve **listQueuesSegmented**.</span><span class="sxs-lookup"><span data-stu-id="e566b-169">tooretrieve a list of queues, use **listQueuesSegmented**.</span></span> <span data-ttu-id="e566b-170">egy adott előtag alapján szűrt listája tooretrieve, használja a **listQueuesSegmentedWithPrefix**.</span><span class="sxs-lookup"><span data-stu-id="e566b-170">tooretrieve a list filtered by a specific prefix, use **listQueuesSegmentedWithPrefix**.</span></span>

```
queueSvc.listQueuesSegmented(null, function(error, result, response){
  if(!error){
    // result.entries contains hello list of queues
  }
});
```

<span data-ttu-id="e566b-171">Az összes várólistán nem adható vissza, ha `result.continuationToken` is használhat az első paraméter hello **listQueuesSegmented** vagy második paramétere hello **listQueuesSegmentedWithPrefix** tooretrieve további eredmények.</span><span class="sxs-lookup"><span data-stu-id="e566b-171">If all queues cannot be returned, `result.continuationToken` can be used as hello first parameter of **listQueuesSegmented** or hello second parameter of **listQueuesSegmentedWithPrefix** tooretrieve more results.</span></span>

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="e566b-172">Útmutató: A várólista törlése</span><span class="sxs-lookup"><span data-stu-id="e566b-172">How To: Delete a Queue</span></span>
<span data-ttu-id="e566b-173">toodelete várólista és köszönőüzenetei minden benne tárolt, hívja az **deleteQueue** hello várólista-objektum metódust.</span><span class="sxs-lookup"><span data-stu-id="e566b-173">toodelete a queue and all hello messages contained in it, call the **deleteQueue** method on hello queue object.</span></span>

```
queueSvc.deleteQueue(queueName, function(error, response){
  if(!error){
    // Queue has been deleted
  }
});
```

<span data-ttu-id="e566b-174">tooclear összes üzenetet az üzenetsorból, törlése nélkül használja **clearMessages**.</span><span class="sxs-lookup"><span data-stu-id="e566b-174">tooclear all messages from a queue without deleting it, use **clearMessages**.</span></span>

## <a name="how-to-work-with-shared-access-signatures"></a><span data-ttu-id="e566b-175">Hogyan: közös hozzáférésű Jogosultságkód használata</span><span class="sxs-lookup"><span data-stu-id="e566b-175">How to: Work with Shared Access Signatures</span></span>
<span data-ttu-id="e566b-176">Megosztott hozzáférési aláírásokkal (SAS) anélkül, hogy a tárfiók neve vagy a kulcsok egy biztonságos módon tooprovide részletes hozzáférés tooqueues.</span><span class="sxs-lookup"><span data-stu-id="e566b-176">Shared Access Signatures (SAS) are a secure way tooprovide granular access tooqueues without providing your storage account name or keys.</span></span> <span data-ttu-id="e566b-177">SAS nincsenek gyakran használt tooprovide korlátozott hozzáférés tooyour, például, amely lehetővé teszi a mobilalkalmazások toosubmit üzenetek.</span><span class="sxs-lookup"><span data-stu-id="e566b-177">SAS are often used tooprovide limited access tooyour queues, such as allowing a mobile app toosubmit messages.</span></span>

<span data-ttu-id="e566b-178">A megbízható alkalmazások, például egy felhőalapú szolgáltatás hoz létre egy SAS használatával hello **generateSharedAccessSignature** a hello **QueueService**, tooan biztosítja azt, és nem megbízható vagy félig megbízható alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e566b-178">A trusted application such as a cloud-based service generates a SAS using hello **generateSharedAccessSignature** of hello **QueueService**, and provides it tooan untrusted or semi-trusted application.</span></span> <span data-ttu-id="e566b-179">Ha például egy mobilalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e566b-179">For example, a mobile app.</span></span> <span data-ttu-id="e566b-180">hello SAS egy házirendet, amely ismerteti a hello kezdő és záró dátumát, mely hello során SAS érvénytelen jön létre, valamint hello hozzáférési szint megadott toohello SAS tulajdonosával.</span><span class="sxs-lookup"><span data-stu-id="e566b-180">hello SAS is generated using a policy, which describes hello start and end dates during which hello SAS is valid, as well as hello access level granted toohello SAS holder.</span></span>

<span data-ttu-id="e566b-181">a következő példa hello hoz létre egy új megosztott elérési házirendet, amely lehetővé teszi a hello SAS jogosult tooadd üzenetek toohello várólista, és 100 perc létrehozták hello idő múlva lejár.</span><span class="sxs-lookup"><span data-stu-id="e566b-181">hello following example generates a new shared access policy that will allow hello SAS holder tooadd messages toohello queue, and expires 100 minutes after hello time it is created.</span></span>

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

<span data-ttu-id="e566b-182">Vegye figyelembe, hogy hello állomásadatai csak megadott is szükség rá, amikor hello SAS jogosult megpróbálja tooaccess hello várólista.</span><span class="sxs-lookup"><span data-stu-id="e566b-182">Note that hello host information must be provided also, as it is required when hello SAS holder attempts tooaccess hello queue.</span></span>

<span data-ttu-id="e566b-183">hello ügyfélalkalmazást, akkor használja a biztonsági Társítások hello **QueueServiceWithSAS** hello várólista tooperform műveleteket.</span><span class="sxs-lookup"><span data-stu-id="e566b-183">hello client application then uses hello SAS with **QueueServiceWithSAS** tooperform operations against hello queue.</span></span> <span data-ttu-id="e566b-184">a következő példa hello toohello várólista csatlakozik, és létrehoz egy üzenetet.</span><span class="sxs-lookup"><span data-stu-id="e566b-184">hello following example connects toohello queue and creates a message.</span></span>

```
var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
  if(!error){
    //message added
  }
});
```

<span data-ttu-id="e566b-185">Hello SAS létre lett hozva a hozzáférés hozzáadása, ha kísérlet történt tooread, update vagy delete üzenetek, mivel hiba volna adott vissza.</span><span class="sxs-lookup"><span data-stu-id="e566b-185">Since hello SAS was generated with add access, if an attempt were made tooread, update or delete messages, an error would be returned.</span></span>

### <a name="access-control-lists"></a><span data-ttu-id="e566b-186">Hozzáférés-vezérlési listák</span><span class="sxs-lookup"><span data-stu-id="e566b-186">Access control lists</span></span>
<span data-ttu-id="e566b-187">A hozzáférés-vezérlési lista (ACL) tooset hello hozzáférési házirendek tartozó SAS korlátozására is használhatja.</span><span class="sxs-lookup"><span data-stu-id="e566b-187">You can also use an Access Control List (ACL) tooset hello access policy for a SAS.</span></span> <span data-ttu-id="e566b-188">Ez akkor hasznos, ha akarja tooallow több ügyfelek tooaccess hello várólista, de adjon meg másik hozzáférési házirendeket az egyes ügyfelekhez.</span><span class="sxs-lookup"><span data-stu-id="e566b-188">This is useful if you wish tooallow multiple clients tooaccess hello queue, but provide different access policies for each client.</span></span>

<span data-ttu-id="e566b-189">Hozzáférés-vezérlési Listában hozzáférési házirendeket, tömbje segítségével minden házirendhez társított azonosítójú van megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="e566b-189">An ACL is implemented using an array of access policies, with an ID associated with each policy.</span></span> <span data-ttu-id="e566b-190">a következő példa hello két szabályzatokat; határoz meg. egy "felhasználó1" és "felhasználó2":</span><span class="sxs-lookup"><span data-stu-id="e566b-190">hello  following example defines two policies; one for 'user1' and one for 'user2':</span></span>

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

<span data-ttu-id="e566b-191">a következő példa lekérdezi hello hello aktuális hozzáférés-Vezérlési **Várólista_neve**, majd hozzáadja az új házirendeket hello segítségével **setQueueAcl**.</span><span class="sxs-lookup"><span data-stu-id="e566b-191">hello following example gets hello current ACL for **myqueue**, then adds hello new policies using **setQueueAcl**.</span></span> <span data-ttu-id="e566b-192">Ez a megközelítés lehetővé teszi, hogy:</span><span class="sxs-lookup"><span data-stu-id="e566b-192">This approach allows:</span></span>

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

<span data-ttu-id="e566b-193">Egyszer hello hozzáférés-vezérlési lista van beállítva, majd létrehozhat egy SAS-házirendje hello azonosítója alapján.</span><span class="sxs-lookup"><span data-stu-id="e566b-193">Once hello ACL has been set, you can then create a SAS based on hello ID for a policy.</span></span> <span data-ttu-id="e566b-194">a következő példa hello hoz létre egy új SAS-kód "felhasználó2":</span><span class="sxs-lookup"><span data-stu-id="e566b-194">hello following example creates a new SAS for 'user2':</span></span>

```
queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });
```

## <a name="next-steps"></a><span data-ttu-id="e566b-195">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e566b-195">Next Steps</span></span>
<span data-ttu-id="e566b-196">Most, hogy megismerte a queue storage alapjait hello, kövesse az összetettebb tárolási feladatok elvégzéséről kapcsolatos alábbi hivatkozások toolearn.</span><span class="sxs-lookup"><span data-stu-id="e566b-196">Now that you've learned hello basics of queue storage, follow these links toolearn about more complex storage tasks.</span></span>

* <span data-ttu-id="e566b-197">A Microsoft hello [Azure Storage csapat blogja] [Azure Storage csapat blogja].</span><span class="sxs-lookup"><span data-stu-id="e566b-197">Visit hello [Azure Storage Team Blog][Azure Storage Team Blog].</span></span>
* <span data-ttu-id="e566b-198">A Microsoft hello [Azure Storage szolgáltatás SDK csomópont] [ Azure Storage SDK for Node] GitHub tárházából.</span><span class="sxs-lookup"><span data-stu-id="e566b-198">Visit hello [Azure Storage SDK for Node][Azure Storage SDK for Node] repository on GitHub.</span></span>

[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node
[using hello REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure Portal]: https://portal.azure.com<span data-ttu-id="e566b-199">
[Node.js-webalkalmazás létrehozása az Azure App Service-ben](../../app-service-web/app-service-web-get-started-nodejs.md) </span><span class="sxs-lookup"><span data-stu-id="e566b-199">
[Create a Node.js web app in Azure App Service](../../app-service-web/app-service-web-get-started-nodejs.md) </span></span>  
<span data-ttu-id="e566b-200">[Hello Azure Table szolgáltatás használata node.js-webalkalmazás](../../app-service-web/storage-nodejs-use-table-storage-web-site.md)
</span><span class="sxs-lookup"><span data-stu-id="e566b-200">[Node.js web app using hello Azure Table Service](../../app-service-web/storage-nodejs-use-table-storage-web-site.md)
</span></span>  


<span data-ttu-id="e566b-201">[Hozza létre, és a Node.js-alkalmazás tooan Azure Cloud Service telepítése](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) </span><span class="sxs-lookup"><span data-stu-id="e566b-201">[Build and deploy a Node.js application tooan Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) </span></span>  
<span data-ttu-id="e566b-202">[Az azure Storage csapat blogja]: http://blogs.msdn.com/b/windowsazurestorage/ [létrehozása és telepítése a Node.js web app tooAzure Web Matrix]: https://www.microsoft.com/web/webmatrix/</span><span class="sxs-lookup"><span data-stu-id="e566b-202">[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/ [Build and deploy a Node.js web app tooAzure using Web Matrix]: https://www.microsoft.com/web/webmatrix/</span></span>   
