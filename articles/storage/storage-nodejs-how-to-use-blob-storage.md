---
title: "Blob storage-ának Node.js aaaHow toouse |} Microsoft Docs"
description: "Strukturálatlan adatok tárolása az Azure Blob storage (object storage) hello felhő."
services: storage
documentationcenter: nodejs
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 8b0df222-1ca8-4967-8248-6d6d720947b8
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: e405eecdc60cd1eaa77510e7b29b41269372b65e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-nodejs"></a>Hogyan toouse Blob-tároló Node.js-ből
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a>Áttekintés
Ez a cikk bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz Blob Storage tárolóban. hello minták hello Node.js API használatával készültek. hello jelzett esetek hogyan tooupload, listázása, letöltése és blobok törlése.

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Node.js alkalmazás létrehozása
Útmutatást toocreate egy Node.js-alkalmazást, lásd: [Node.js-webalkalmazás létrehozása az Azure App Service], [és üzembe egy Node.js-alkalmazás tooan Azure Cloud Service] – Windows PowerShell használatával , vagy [létrehozása és központi telepítése a Node.js web app tooAzure Web Matrix].

## <a name="configure-your-application-tooaccess-storage"></a>Az alkalmazás tooaccess tároló konfigurálása
az Azure storage toouse, kell hello Azure Storage szolgáltatás SDK a Node.js, amely kényelmi szalagtárak hello tárolószolgáltatások REST kommunikációt készletét tartalmazza.

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a>Csomópont Package Manager (NPM) tooobtain hello csomag használata
1. Használjon például egy parancssori felületet **PowerShell** (Windows), **Terminálszolgáltatások** (Mac), vagy **Bash** (Unix) toonavigate toohello mappa, amelyben létrehozta a minta az alkalmazás.
2. Típus **npm telepítése azure-tároló** hello parancssori ablakban. Hello parancs az alábbi kódpéldát hasonló toohello.

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
3. Manuális futtatásával hello **ls** parancs tooverify, amely egy **csomópont\_modulok** mappa hozták létre. A mappában található hello **azure-tároló** csomagot, amely tartalmazza, hogy kell-e tooaccess tárolási hello szalagtárak.

### <a name="import-hello-package"></a>Hello csomag importálása
A Jegyzettömbben vagy más szövegszerkesztőben, adja hozzá a következő toohello felső részén hello hello **server.js** toouse tárolási célhelyeként hello alkalmazás fájlt:

```nodejs
var azure = require('azure-storage');
```

## <a name="set-up-an-azure-storage-connection"></a>Egy Azure Storage-kapcsolat beállítása
hello Azure modul olvassák hello környezeti változók `AZURE_STORAGE_ACCOUNT` és `AZURE_STORAGE_ACCESS_KEY`, vagy `AZURE_STORAGE_CONNECTION_STRING`, információ tooconnect tooyour Azure storage-fiók szükséges. Ha ezek a környezeti változók nem, meg kell adnia hello fiók meghívásakor **createBlobService**.

Példa a hello hello környezeti változók beállítása [Azure-portálon](https://portal.azure.com) Azure-webalkalmazás, lásd: [Node.js web app használatával hello Azure Table szolgáltatás].

## <a name="create-a-container"></a>Tároló létrehozása
Hello **BlobService** objektum lehetővé teszi, hogy a tárolók és blobok. hello alábbi kód létrehoz egy **BlobService** objektum. Adja hozzá hello következő tetején hello **server.js**:

```nodejs
var blobSvc = azure.createBlobService();
```

> [!NOTE]
> Akkor érhető el blob névtelenül **createBlobServiceAnonymous** hello állomás cím megadásával. Tegyük fel például, `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.
>
>

[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

egy új tároló toocreate használja **createContainerIfNotExists**. hello alábbi példakód létrehoz egy új tároló "mycontainer" nevű:

```nodejs
blobSvc.createContainerIfNotExists('mycontainer', function(error, result, response){
    if(!error){
      // Container exists and is private
    }
});
```

Hello tároló újonnan jön létre, ha `result.created` értéke true. Ha hello tároló már létezik, `result.created` értéke "false". `response`hello művelet hello ETag párbeszédeket hello a tároló adatait tartalmazza.

### <a name="container-security"></a>Tároló biztonsági
Alapértelmezés szerint az új tárolók személyesek, és nem érhető el névtelen hozzáféréssel. nyilvános, hogy hozzá tud férni névtelenül toomake hello tároló, állíthatja be hello tároló hozzáférési szint túl**blob** vagy **tároló**.

* **a BLOB** -lehetővé teszi a névtelen olvasási hozzáférés tooblob tartalom és metaadatok található, de nem toocontainer metaadatainak, például az összes BLOB a tárolóban lévő listázása
* **tároló** -lehetővé teszi a névtelen olvasási hozzáférés tooblob tartalom és a metaadatok, valamint a csomagtároló metaadatai

hello alábbi példakód mutatja be érvényes hello hozzáférési szint túl**blob**:

```nodejs
blobSvc.createContainerIfNotExists('mycontainer', {publicAccessLevel : 'blob'}, function(error, result, response){
    if(!error){
      // Container exists and allows
      // anonymous read access tooblob
      // content and metadata within this container
    }
});
```

Azt is megteheti, módosíthatja a tároló hello hozzáférési szint használatával **setContainerAcl** toospecify hello hozzáférési szintet. hello következő példa módosítások hello hozzáférési szint toocontainer kódot:

```nodejs
blobSvc.setContainerAcl('mycontainer', null /* signedIdentifiers */, {publicAccessLevel : 'container'} /* publicAccessLevel*/, function(error, result, response){
  if(!error){
    // Container access level set too'container'
  }
});
```

hello eredmény információt tartalmaz hello műveletek, például a hello aktuális **ETag** hello tároló.

### <a name="filters"></a>Szűrők
Nem kötelező szűrési műveletek végre toooperations használatával is alkalmazhat **BlobService**. Műveletek szűrésének lehetnek naplózási, automatikus újrapróbálkozása, stb. Szűrők hello aláírással metódus megvalósító objektumok a következők:

```nodejs
function handle (requestOptions, next)
```

Ezután a hello lehetőségek előfeldolgozása, hello metódust kell toocall "Tovább" gombra, egy visszahívási átadja a aláírását hello:

```nodejs
function (returnObject, finalCallback, next)
```

A visszahívási és hello returnObject (hello hello kérelem toohello kiszolgálótól kapott válasz) feldolgozása után hello visszahívási kell tooeither következő meghívni, ha más szűrők feldolgozása toocontinue létezik, vagy egyszerűen a finalCallback tooend hello szolgáltatás meghívása hívása.

Két szűrőket, amelyek megvalósítják az újrapróbálkozási logika érhetők el a hello Azure SDK for Node.js, **ExponentialRetryPolicyFilter** és **LinearRetryPolicyFilter**. hello következő létrehoz egy **BlobService** hello használó objektum **ExponentialRetryPolicyFilter**:

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var blobSvc = azure.createBlobService().withFilter(retryOperations);
```

## <a name="upload-a-blob-into-a-container"></a>Blobok feltöltése a tárolóba
Nincsenek a blobok három különböző típusa: blokkblobokat, lapblobokat és hozzáfűző blobokat. Blokkblobok lehetővé teszik toomore hatékonyan nagy fájlok feltöltése. Hozzáfűző blobok vannak optimalizálva, műveletek hozzáfűzésére. Optimalizált blobok, amelyek az olvasási/írási műveletek. További információkért lásd: [ismertetése Blokkblobokat, hozzáfűző blobokat és Lapblobokat](http://msdn.microsoft.com/library/azure/ee691964.aspx).

### <a name="block-blobs"></a>Blokkblobok
tooupload adatok tooa blokkblob, a következő használatát hello:

* **createBlockBlobFromLocalFile** - hoz létre egy új blokkblob, és feltölti a fájl tartalmának hello
* **createBlockBlobFromStream** - hoz létre egy új blokkblob, és feltölti a hello tartalmát egy stream
* **createBlockBlobFromText** - hoz létre egy új blokkblob és feltölt egy karakterlánc hello tartalmát
* **createWriteStreamToBlockBlob** -biztosít egy írási adatfolyam tooa blokkblob

hello alábbi példakód feltölt hello hello tartalmát **teszt.txt** fájlt **myblob**.

```nodejs
blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

Hello `result` ezen módszerek által visszaadott hello művelet, például a hello információkat tartalmaz az **ETag** hello BLOB.

### <a name="append-blobs"></a>Hozzáfűző blobokat
tooupload adatok tooa új hozzáfűző blob, használja a következő hello:

* **createAppendBlobFromLocalFile** - hoz létre egy új hozzáfűző blob, és feltölti a fájl tartalmának hello
* **createAppendBlobFromStream** - hoz létre egy új hozzáfűző blob, és feltölti a hello tartalmát egy stream
* **createAppendBlobFromText** - hoz létre egy új hozzáfűző blob és feltölt egy karakterlánc hello tartalmát
* **createWriteStreamToNewAppendBlob** - hoz létre egy új hozzáfűző blob, majd egy adatfolyam toowrite tooit

hello alábbi példakód feltölt hello hello tartalmát **teszt.txt** fájlt **myappendblob**.

```nodejs
blobSvc.createAppendBlobFromLocalFile('mycontainer', 'myappendblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

tooappend egy blokk tooan létező hozzáfűző blob, a következő használatát hello:

* **appendFromLocalFile** -hozzáfűzése hello tartalmát, a fájl tooan a hozzáfűző blob meglévő
* **appendFromStream** -hozzáfűzése hello tartalmát egy stream tooan a meglévő hozzáfűző blob
* **appendFromText** -hello tartalma hozzáfűzése egy karakterlánc tooan a meglévő hozzáfűző blob
* **appendBlockFromStream** -hozzáfűzése hello tartalmát egy stream tooan a meglévő hozzáfűző blob
* **appendBlockFromText** -hello tartalma hozzáfűzése egy karakterlánc tooan a meglévő hozzáfűző blob

> [!NOTE]
> API-k appendFromXXX néhány ügyféloldali ellenőrzés toofail gyors tooavoid szükségtelen server hívás fog tenni. appendBlockFromXXX nem.
>
>

hello alábbi példakód feltölt hello hello tartalmát **teszt.txt** fájlt **myappendblob**.

```nodejs
blobSvc.appendFromText('mycontainer', 'myappendblob', 'text toobe appended', function(error, result, response){
  if(!error){
    // text appended
  }
});
```

### <a name="page-blobs"></a>Lapblobok
tooupload adatok tooa oldalakra vonatkozó blob, a következő használatát hello:

* **createPageBlob** -hoz létre egy új oldalakra vonatkozó blob egy meghatározott hosszúságú
* **createPageBlobFromLocalFile** - hoz létre egy új oldalakra vonatkozó blob, és feltölti a fájl tartalmának hello
* **createPageBlobFromStream** - hoz létre egy új oldalakra vonatkozó blob, és feltölti a hello tartalmát egy stream
* **createWriteStreamToExistingPageBlob** -írási adatfolyam tooan meglévő oldalakra vonatkozó blob biztosít
* **createWriteStreamToNewPageBlob** - hoz létre egy új oldalakra vonatkozó blob, majd egy adatfolyam toowrite tooit

hello alábbi példakód feltölt hello hello tartalmát **teszt.txt** fájlt **mypageblob**.

```nodejs
blobSvc.createPageBlobFromLocalFile('mycontainer', 'mypageblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

> [!NOTE]
> Lapblobokat 512 bájtos "lap" alkotják. Egy hibaüzenetet fog kapni, amely nincs 512 többszöröse méretű adatok feltöltésekor.
>
>

## <a name="list-hello-blobs-in-a-container"></a>Lista hello a tárolóban lévő blobok
toolist hello BLOB a tárolóban lévő hello használata **listBlobsSegmented** metódust. Ha szeretné, hogy egy adott előtaggal rendelkező tooreturn blobok, **listBlobsSegmentedWithPrefix**.

```nodejs
blobSvc.listBlobsSegmented('mycontainer', null, function(error, result, response){
  if(!error){
      // result.entries contains hello entries
      // If not all blobs were returned, result.continuationToken has hello continuation token.
  }
});
```

Hello `result` tartalmaz egy `entries` gyűjteményt, amely minden egyes blob leíró objektumok tömbje. Összes BLOB nem adható vissza, ha hello `result` is biztosít a `continuationToken`, amely akár hello második paraméter tooretrieve további bejegyzéseknek is használhatja.

## <a name="download-blobs"></a>Blobok letöltése
toodownload adatait egy blob hello következő használja:

* **getBlobToLocalFile** -hello blob tartalma toofile írási műveletek
* **getBlobToStream** -ír hello blob tartalma tooa adatfolyam
* **getBlobToText** -ír hello blob tartalmát tooa karakterlánc
* **createReadStream** -biztosít egy adatfolyam tooread hello blobból

hello alábbi példakód bemutatja használatával **getBlobToStream** toodownload hello tartalmát hello **myblob** blob-, és tárolja toohello **kimenet.txt** fájl használatával egy az adatfolyam:

```nodejs
var fs = require('fs');
blobSvc.getBlobToStream('mycontainer', 'myblob', fs.createWriteStream('output.txt'), function(error, result, response){
  if(!error){
    // blob retrieved
  }
});
```

Hello `result` hello blob adatait tartalmazza többek között **ETag** információkat.

## <a name="delete-a-blob"></a>Blob törlése
Végezetül toodelete blob, hívja **deleteBlob**. a következő példakód törlések hello nevű blob hello **myblob**.

```nodejs
blobSvc.deleteBlob(containerName, 'myblob', function(error, response){
  if(!error){
    // Blob has been deleted
  }
});
```

## <a name="concurrent-access"></a>Egyidejű hozzáférés
toosupport egyidejű hozzáférés tooa blob több ügyfél vagy a folyamat több példányt is használhat **ETag-EK** vagy **bérleteket**.

* **ETag** -tartalmaz egy módja toodetect, amely blob vagy tároló hello módosították egy másik folyamat
* **Címbérlet** - tartalmaz egy módja tooobtain kizárólagos, megújítható, írási vagy egy adott időn belül hozzáférés tooa blob törlése

### <a name="etag"></a>ETag
ETag-EK használja, ha több ügyfelek vagy a példányok toowrite toohello blokk Blob vagy a lap Blob egyidejűleg tooallow van szüksége. hello ETag lehetővé teszi toodetermine hello tárolót vagy blobot óta módosították kezdetben olvasására vagy létrehozta, amely lehetővé teszi egy másik ügyfél vagy a folyamat által előjegyzett módosítások felülírását tooavoid.

Választható hello segítségével beállíthatja az ETag feltételek `options.accessConditions` paraméter. hello következő Kódpélda csak feltölt hello **teszt.txt** által tartalmazott fájlok Ha hello blob már létezik, és hello ETag érték `etagToMatch`.

```nodejs
blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', { accessConditions: { EtagMatch: etagToMatch} }, function(error, result, response){
    if(!error){
    // file uploaded
  }
});
```

ETag-EK használata esetén hello általános mintát esetén:

1. Szerezze be a hello ETag hello így létrehozása, a lista vagy a get műveletet.
2. Egy műveletet, hogy hello ETag érték ellenőrzése nem történt módosítás.

Ha hello érték módosítva lett, az azt jelenti, hogy egy másik ügyfél vagy a példány hello blob vagy tároló óta módosított hello ETag értéket kapott.

### <a name="lease"></a>Címbérlet
Hello segítségével lekérheti egy új bérleti **acquireLease** metódus hello blob vagy tároló megadott kívánt tooobtain a címbérlet a megadásával. Például a következő kód hello címbérletet a **myblob**.

```nodejs
blobSvc.acquireLease('mycontainer', 'myblob', function(error, result, response){
  if(!error) {
    console.log('leaseId: ' + result.id);
  }
});
```

A későbbi műveletek **myblob** biztosítania kell a hello `options.leaseId` paraméter. hello bérleti Azonosítóját adja vissza a rendszer `result.id` a **acquireLease**.

> [!NOTE]
> Alapértelmezés szerint hello címbérlet érték a végtelen. Megadhat egy nem végtelen időtartama (között 15 és 60 másodperc), adja meg a hello `options.leaseDuration` paraméter.
>
>

használja a címbérlet tooremove **releaseLease**. toobreak a címbérlet, de megakadályozza mások beszerezni új bérletet hello eredeti időtartam lejártáig, használjon **breakLease**.

## <a name="work-with-shared-access-signatures"></a>Megosztott hozzáférési aláírásokkal működik
Közös hozzáférésű jogosultságkód (SAS) egy biztonságos módon tooprovide részletes hozzáférés tooblobs és a tárolók a tárfiók neve vagy a kulcsok megadása nélkül. Közös hozzáférésű jogosultságkód gyakran használt tooprovide korlátozott hozzáférés tooyour adatok, például a mobilalkalmazások tooaccess blobok így.

> [!NOTE]
> Közben is engedélyezheti a névtelen hozzáférés tooblobs, közös hozzáférésű jogosultságkód lehetővé teszik több ellenőrzött tooprovide hozzáférés, például hello SAS létre kell hoznia.
>
>

A megbízható alkalmazások, például egy felhőalapú szolgáltatás közös hozzáférésű jogosultságkód hello segítségével hoz létre **generateSharedAccessSignature** a hello **BlobService**, és a nem megbízható tooan vagy félig megbízható alkalmazásadatokat, például egy mobilalkalmazást. Közös hozzáférésű aláírások akkor jönnek létre a szabályzatot, amely ismerteti a hello start használatával és záró dátumát, mely hello során közös hozzáférésű jogosultságkód érvényesek, valamint hello hozzáférési toohello megosztott hozzáférési szint megadott aláírások tulajdonosával.

hello alábbi példakód létrehoz egy új megosztott elérési házirendet, amely lehetővé teszi, hogy hello megosztott hozzáférési aláírásokkal jogosult tooperform olvasási műveletek a hello **myblob** blob-, és 100 perc létrehozták hello idő múlva lejár.

```nodejs
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
    Start: startDate,
    Expiry: expiryDate
  },
};

var blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', 'myblob', sharedAccessPolicy);
var host = blobSvc.host;
```

Vegye figyelembe, hogy hello állomásadatai csak megadott is szükség rá, amikor hello megosztott hozzáférési aláírásokkal jogosult megpróbálja tooaccess hello tároló.

hello majd ügyfélalkalmazás megosztott hozzáférési aláírásokkal rendelkező **BlobServiceWithSAS** hello blob tooperform műveleteket. hello következő lekérdezi, **myblob**.

```nodejs
var sharedBlobSvc = azure.createBlobServiceWithSas(host, blobSAS);
sharedBlobSvc.getBlobProperties('mycontainer', 'myblob', function (error, result, response) {
  if(!error) {
    // retrieved info
  }
});
```

Hello megosztott hozzáférési aláírásokkal csak olvasási hozzáféréssel rendelkező jött létre, ha toomodify hello blob tett kísérlet, mert egy hibaüzenetet küld.

### <a name="access-control-lists"></a>Hozzáférés-vezérlési listák
Hozzáférés-vezérlési lista (ACL) tooset hello hozzáférési szabályzatok az SAS is használhatja. Ez akkor hasznos, ha akarja tooallow több ügyfelek tooaccess tárolója, de adjon meg másik hozzáférési házirendeket az egyes ügyfelekhez.

Hozzáférés-vezérlési Listában hozzáférési házirendeket, tömbje segítségével minden házirendhez társított azonosítójú van megvalósítva. a következő példakód hello meghatározza, hogy két házirend, egy "felhasználó1", és egy "felhasználó2":

```nodejs
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.WRITE,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

a következő kód példa lekérdezi hello hello aktuális hozzáférés-Vezérlési **mycontainer**, és hozzáadja az új házirendeket hello segítségével **setBlobAcl**. Ez a megközelítés lehetővé teszi, hogy:

```nodejs
var extend = require('extend');
blobSvc.getBlobAcl('mycontainer', function(error, result, response) {
  if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    blobSvc.setBlobAcl('mycontainer', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

Egyszer hello hozzáférés-vezérlési lista van beállítva, majd létrehozhat megosztott hozzáférési aláírásokkal hello azonosító egy házirend alapján. a következő példakód hello "felhasználó2" az új közös hozzáférésű jogosultságkód hoz létre:

```nodejs
blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', { Id: 'user2' });
```

## <a name="next-steps"></a>Következő lépések
További információkért tekintse meg a következő erőforrások hello.

* [Az Azure Storage SDK csomópont API-referencia][Azure Storage SDK for Node API Reference]
* [Az Azure Storage csapat blogja][Azure Storage Team Blog]
* [Az Azure Storage szolgáltatás SDK csomópont] [ Azure Storage SDK for Node] GitHub tárházából
* [Node.js fejlesztői központ](https://azure.microsoft.com/develop/nodejs/)
* [Adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md)

[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node

[Node.js-webalkalmazás létrehozása az Azure App Service]: ../app-service-web/app-service-web-get-started-nodejs.md
[Node.js web app használatával hello Azure Table szolgáltatás]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md
[létrehozása és központi telepítése a Node.js web app tooAzure Web Matrix]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
[Using hello REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure portal]: https://portal.azure.com
[és üzembe egy Node.js-alkalmazás tooan Azure Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure Storage SDK for Node API Reference]: http://dl.windowsazure.com/nodestoragedocs/index.html
