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
# <a name="how-toouse-queue-storage-from-nodejs"></a>Hogyan toouse a Queue storage Node.js-ből
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a>Áttekintés
Ez az útmutató bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz hello Microsoft Azure Queue szolgáltatás. hello minták hello Node.js API segítségével készül. hello tárgyalt forgatókönyvekben szerepel a **beszúrása**, **megtekintésekor**, **első**, és **törlése** üzenetek, várólista, valamint  **létrehozása és törlése várólisták**.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Node.js-alkalmazás létrehozása
Üres Node.js-alkalmazás létrehozása. A Node.js-alkalmazás létrehozása utasításokért lásd: [Node.js-webalkalmazás létrehozása az Azure App Service](../../app-service-web/app-service-web-get-started-nodejs.md), [és üzembe egy Node.js-alkalmazás tooan Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) a Windows PowerShell vagy [ Hozza létre és telepítheti a Node.js web app tooAzure Web Matrix](https://www.microsoft.com/web/webmatrix/).

## <a name="configure-your-application-tooaccess-storage"></a>Alkalmazás tooAccess tároló konfigurálása
az Azure storage toouse, kell hello Azure Storage szolgáltatás SDK a Node.js, amely kényelmi szalagtárak hello tárolószolgáltatások REST kommunikációt készletét tartalmazza.

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a>Csomópont Package Manager (NPM) tooobtain hello csomag használata
1. Használjon például egy parancssori felületet **PowerShell** (Windows), **Terminálszolgáltatások** (Mac) vagy **Bash** (Unix), keresse meg a toohello mappa, amelyben létrehozta a mintaalkalmazást.
2. Típus **npm telepítése azure-tároló** hello parancssori ablakban. Hello parancs kimenete a következő példa hasonló toohello.
 
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

3. Manuális futtatásával hello **ls** parancs tooverify, amely egy **csomópont\_modulok** mappa hozták létre. Mappán belül található hello **azure-tároló** csomagot, amely tartalmazza a hello szalagtárak kell érniük a tárhelyet.

### <a name="import-hello-package"></a>Hello csomag importálása
A Jegyzettömbben vagy más szövegszerkesztőben, adja hozzá a következő toohello felső hello a **server.js** toouse tárolási célhelyeként hello alkalmazás fájlt:

```
var azure = require('azure-storage');
```

## <a name="setup-an-azure-storage-connection"></a>Az Azure Storage-kapcsolat beállítása
hello azure modul olvassák hello környezeti változók AZURE\_tárolási\_FIÓKOT és az AZURE\_tárolási\_hozzáférés\_kulcs, vagy AZURE\_tárolási\_kapcsolat \_Szükséges adatokat tooconnect tooyour Azure storage-fiók KARAKTERLÁNCOT. Ha ezek a környezeti változók nem, meg kell adnia hello fiók meghívásakor **createQueueService**.

Példa a hello hello környezeti változók beállítása [Azure Portal](https://portal.azure.com) egy Azure-webhelyre, lásd: [Node.js web app használatával hello Azure Table szolgáltatás].

## <a name="how-to-create-a-queue"></a>Útmutató: A várólista létrehozása
hello alábbi kód létrehoz egy **QueueService** objektum, amely lehetővé teszi az üzenetsorok toowork.

```
var queueSvc = azure.createQueueService();
```

Használjon hello **createQueueIfNotExists** metódus, amely a megadott várólista hello adja vissza, ha már létezik, vagy létrehoz egy új üzenetsort hello megadott névvel, ha még nem létezik.

```
queueSvc.createQueueIfNotExists('myqueue', function(error, result, response){
  if(!error){
    // Queue created or exists
  }
});
```

Ha hello várólista létrehozása `result.created` értéke true. Ha hello várólista létezik, `result.created` értéke "false".

### <a name="filters"></a>Szűrők
Választható szűrési műveletek lehet segítségével alkalmazott toooperations **QueueService**. Műveletek szűrésének lehetnek naplózási, automatikus újrapróbálkozása, stb. Szűrők hello aláírással metódus megvalósító objektumok a következők:

```
function handle (requestOptions, next)
```

Ezután a előfeldolgozása hello kérelem lehetőségekről, hello metódus van szüksége a "Tovább" gombra. átadja a aláírását hello egy visszahívási toocall:

```
function (returnObject, finalCallback, next)
```

A visszahívási és hello returnObject (hello hello kérelem toohello kiszolgálótól kapott válasz) feldolgozása után hello visszahívási kell tooeither következő meghívni, ha más szűrők feldolgozása toocontinue létezik, vagy egyszerűen a finalCallback meghívása egyéb tooend hello mentése szolgáltatás hívása.

Két szűrőket, amelyek megvalósítják az újrapróbálkozási logika érhetők el a hello Azure SDK for Node.js, **ExponentialRetryPolicyFilter** és **LinearRetryPolicyFilter**. hello következő létrehoz egy **QueueService** hello használó objektum **ExponentialRetryPolicyFilter**:

```
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var queueSvc = azure.createQueueService().withFilter(retryOperations);
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Útmutató: A várólista üzenet beszúrása
a várólistán, használjon hello üzenet tooinsert **createMessage** hozzon létre egy új üzenetet, és adja hozzá toohello várólista módszert.

```
queueSvc.createMessage('myqueue', "Hello world!", function(error, result, response){
  if(!error){
    // Message inserted
  }
});
```

## <a name="how-to-peek-at-hello-next-message"></a>Útmutató: A következő üzenetet hello betekintés
Eltávolítása hello által hívó hello anélkül is bepillanthat hello betekintés a várólista elejére hello **peekMessages** metódust. Alapértelmezés szerint **peekMessages** betekintés egyetlen üzenetben.

```
queueSvc.peekMessages('myqueue', function(error, result, response){
  if(!error){
    // Message text is in messages[0].messageText
  }
});
```

Hello `result` üdvözlőüzenetére tartalmazza.

> [!NOTE]
> Használatával **peekMessages** Ha nincsenek hello várólistában lévő üzenetek nem hibát adnak vissza, azonban nem adja vissza.
> 
> 

## <a name="how-to-dequeue-hello-next-message"></a>Útmutató: A következő üzenetet hello created
Egy üzenet feldolgozása egy két lépésből álló folyamat:

1. Created üdvözlőüzenetére.
2. Hello üzenet törlése.

egy üzenet toodequeue használja **getMessages**. Így köszönőüzenetei láthatatlan hello várólista, így más ügyfelek nem képes feldolgozni azokat. Az alkalmazás rendelkezik egy üzenet feldolgozása után hívja **deleteMessage** toodelete hello várólista le. a következő példa hello lekérdezi egy üzenetet, majd törli őket:

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
> Alapértelmezés szerint az üzenet csak rejtett 30 másodpercig, amely után látható tooother ügyfelek. A jelenlegitől eltérő értéket is megadhat `options.visibilityTimeout` rendelkező **getMessages**.
> 
> [!NOTE]
> Használatával **getMessages** Ha nincsenek hello várólistában lévő üzenetek nem hibát adnak vissza, azonban nem adja vissza.
> 
> 

## <a name="how-to-change-hello-contents-of-a-queued-message"></a>Útmutató: Módosítsa az sorba állított üzenetek hello tartalma
Módosíthatja egy üzenet helyben hello várólista használatával hello tartalmát **updateMessage**. a következő példa frissítések hello szöveges üzenet hello:

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

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Útmutató: További beállítások üzenetmozgatót üzenetek
Két módon szabhatja testre az üzenetek lekérését egy üzenetsorból:

* `options.numOfMessages`-Beolvasása az üzenetkötegek (mentése too32.)
* `options.visibilityTimeout`– Állítsa be a hosszabb vagy rövidebb láthatatlansági időkorlátot.

hello alábbi példában hello **getMessages** metódus tooget 15 üzenetek egy hívásban. Ezután minden üzenetet használatával feldolgozza a hurok. Beállítja a hello láthatatlansági időtúllépés toofive percig, amíg ez a metódus által visszaadott összes üzenetet is.

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

## <a name="how-to-get-hello-queue-length"></a>How To: Get hello várólistájának hossza
Hello **getQueueMetadata** hello várólista, beleértve a hello hozzávetőleges hello várólistáján lévő üzenetek száma kapcsolatos metaadatokat adja vissza.

```
queueSvc.getQueueMetadata('myqueue', function(error, result, response){
  if(!error){
    // Queue length is available in result.approximateMessageCount
  }
});
```

## <a name="how-to-list-queues"></a>Útmutató: A lista várólisták
a várólisták, használja listáját tooretrieve **listQueuesSegmented**. egy adott előtag alapján szűrt listája tooretrieve, használja a **listQueuesSegmentedWithPrefix**.

```
queueSvc.listQueuesSegmented(null, function(error, result, response){
  if(!error){
    // result.entries contains hello list of queues
  }
});
```

Az összes várólistán nem adható vissza, ha `result.continuationToken` is használhat az első paraméter hello **listQueuesSegmented** vagy második paramétere hello **listQueuesSegmentedWithPrefix** tooretrieve további eredmények.

## <a name="how-to-delete-a-queue"></a>Útmutató: A várólista törlése
toodelete várólista és köszönőüzenetei minden benne tárolt, hívja az **deleteQueue** hello várólista-objektum metódust.

```
queueSvc.deleteQueue(queueName, function(error, response){
  if(!error){
    // Queue has been deleted
  }
});
```

tooclear összes üzenetet az üzenetsorból, törlése nélkül használja **clearMessages**.

## <a name="how-to-work-with-shared-access-signatures"></a>Hogyan: közös hozzáférésű Jogosultságkód használata
Megosztott hozzáférési aláírásokkal (SAS) anélkül, hogy a tárfiók neve vagy a kulcsok egy biztonságos módon tooprovide részletes hozzáférés tooqueues. SAS nincsenek gyakran használt tooprovide korlátozott hozzáférés tooyour, például, amely lehetővé teszi a mobilalkalmazások toosubmit üzenetek.

A megbízható alkalmazások, például egy felhőalapú szolgáltatás hoz létre egy SAS használatával hello **generateSharedAccessSignature** a hello **QueueService**, tooan biztosítja azt, és nem megbízható vagy félig megbízható alkalmazás. Ha például egy mobilalkalmazást. hello SAS egy házirendet, amely ismerteti a hello kezdő és záró dátumát, mely hello során SAS érvénytelen jön létre, valamint hello hozzáférési szint megadott toohello SAS tulajdonosával.

a következő példa hello hoz létre egy új megosztott elérési házirendet, amely lehetővé teszi a hello SAS jogosult tooadd üzenetek toohello várólista, és 100 perc létrehozták hello idő múlva lejár.

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

Vegye figyelembe, hogy hello állomásadatai csak megadott is szükség rá, amikor hello SAS jogosult megpróbálja tooaccess hello várólista.

hello ügyfélalkalmazást, akkor használja a biztonsági Társítások hello **QueueServiceWithSAS** hello várólista tooperform műveleteket. a következő példa hello toohello várólista csatlakozik, és létrehoz egy üzenetet.

```
var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
  if(!error){
    //message added
  }
});
```

Hello SAS létre lett hozva a hozzáférés hozzáadása, ha kísérlet történt tooread, update vagy delete üzenetek, mivel hiba volna adott vissza.

### <a name="access-control-lists"></a>Hozzáférés-vezérlési listák
A hozzáférés-vezérlési lista (ACL) tooset hello hozzáférési házirendek tartozó SAS korlátozására is használhatja. Ez akkor hasznos, ha akarja tooallow több ügyfelek tooaccess hello várólista, de adjon meg másik hozzáférési házirendeket az egyes ügyfelekhez.

Hozzáférés-vezérlési Listában hozzáférési házirendeket, tömbje segítségével minden házirendhez társított azonosítójú van megvalósítva. a következő példa hello két szabályzatokat; határoz meg. egy "felhasználó1" és "felhasználó2":

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

a következő példa lekérdezi hello hello aktuális hozzáférés-Vezérlési **Várólista_neve**, majd hozzáadja az új házirendeket hello segítségével **setQueueAcl**. Ez a megközelítés lehetővé teszi, hogy:

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

Egyszer hello hozzáférés-vezérlési lista van beállítva, majd létrehozhat egy SAS-házirendje hello azonosítója alapján. a következő példa hello hoz létre egy új SAS-kód "felhasználó2":

```
queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });
```

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a queue storage alapjait hello, kövesse az összetettebb tárolási feladatok elvégzéséről kapcsolatos alábbi hivatkozások toolearn.

* A Microsoft hello [Azure Storage csapat blogja] [Azure Storage csapat blogja].
* A Microsoft hello [Azure Storage szolgáltatás SDK csomópont] [ Azure Storage SDK for Node] GitHub tárházából.

[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node
[using hello REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure Portal]: https://portal.azure.com
[Node.js-webalkalmazás létrehozása az Azure App Service-ben](../../app-service-web/app-service-web-get-started-nodejs.md)   
[Hello Azure Table szolgáltatás használata node.js-webalkalmazás](../../app-service-web/storage-nodejs-use-table-storage-web-site.md)
  


[Hozza létre, és a Node.js-alkalmazás tooan Azure Cloud Service telepítése](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md)   
[Az azure Storage csapat blogja]: http://blogs.msdn.com/b/windowsazurestorage/ [létrehozása és telepítése a Node.js web app tooAzure Web Matrix]: https://www.microsoft.com/web/webmatrix/   
