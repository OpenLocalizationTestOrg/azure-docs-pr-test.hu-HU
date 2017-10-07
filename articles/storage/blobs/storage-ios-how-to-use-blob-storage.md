---
title: "aaaHow toouse Azure Blob storage-ának iOS |} Microsoft Docs"
description: "Strukturálatlan adatok tárolása az Azure Blob storage (object storage) hello felhő."
services: storage
documentationcenter: ios
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: df188021-86fc-4d31-a810-1b0e7bcd814b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: objective-c
ms.topic: article
ms.date: 05/11/2017
ms.author: michaelhauss
ms.openlocfilehash: cc08b76b682537a9a51e970c76bd76c7c06a4ccb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-ios"></a>Hogyan toouse Blob storage-ának iOS
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Áttekintés
Ez a cikk bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz a Microsoft Azure Blob Storage tárolóban. hello minták Objective-C nyelven íródtak, és használja a hello [Azure Storage ügyféloldali kódtára a iOS](https://github.com/Azure/azure-storage-ios). hello tárgyalt forgatókönyvekben szerepel a **feltöltése**, **felsoroló**, **letöltése**, és **törlése** blobokat. A blobok további információkért lásd: hello [lépések](#next-steps) szakasz. Emellett letöltheti a hello [mintaalkalmazás](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) tooquickly lásd: hello iOS-alkalmazás az Azure Storage használatát.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="import-hello-azure-storage-ios-library-into-your-application"></a>Hello Azure Storage iOS kódtár importálnia kell az alkalmazás
Importálhatja hello Azure Storage iOS könyvtár az alkalmazásba vagy hello segítségével [Azure Storage CocoaPod](https://cocoapods.org/pods/AZSClient) vagy hello importálásával **keretrendszer** fájlt. CocoaPod hello integráló hello könyvtár azonban hello keretrendszer fájlból való importálása a meglévő projekthez kevesebb zavaró egyszerűbbé teszi ajánlott módja.

toouse ebben a könyvtárban van szüksége a következő hello:
- iOS 8 +
- Xcode 7 +

## <a name="cocoapod"></a>CocoaPod
1. Ha még nem tette, [telepítése CocoaPods](https://guides.cocoapods.org/using/getting-started.html#toc_3) a számítógépen nyisson meg egy terminálablakot, és hello a következő parancs futtatása
    
    ```shell   
    sudo gem install cocoapods
    ```

2. A következő hello projektkönyvtár (hello a .xcodeproj fájlt tartalmazó könyvtár), hozzon létre egy új fájlt nevű _Podfile_(nincs fájl kiterjesztése). Adja hozzá a következő too_Podfile_ hello és mentéséhez.

    ```ruby
    platform :ios, '8.0'

    target 'TargetName' do
      pod 'AZSClient'
    end
    ```

3. Hello terminálablakot lépjen a toohello projektkönyvtár és a következő parancs futtatása hello

    ```shell    
    pod install
    ```

4. Ha a .xcodeproj nyissa meg az xcode-ban, zárja be. A projektben nyissa meg az újonnan létrehozott hello projektfájlban hello .xcworkspace bővítményt, amelyekről fog készülni. Ez az fogja dolgozik a most már a hello fájlt.

## <a name="framework"></a>Keretrendszer
hello más módon toouse hello könyvtár toobuild hello keretrendszer manuálisan:

1. First, a letöltési vagy a Klónozás hello [azure-storage-ios tárház](https://github.com/azure/azure-storage-ios).
2. Nyissa meg a *azure-storage-ios* -> *Lib* -> *Azure Storage ügyféloldali kódtár*, és nyissa meg a `AZSClient.xcodeproj` az xcode-ban.
3. A bal felső az Xcode-ban módosítás hello aktív hello sémáját az "Azure Storage ügyféloldali kódtár" túl "Keretrendszer".
4. (⌘ + B) hello projekt felépítéséhez. Ezzel létrehoz egy `AZSClient.framework` fájl az asztalon.

Majd hello keretrendszer fájlt importálhatja az alkalmazásba hello következő tevékenységek végrehajtásával:

1. Hozzon létre egy új projektet, vagy nyissa meg a meglévő projektet az xcode-ban.
2. A fogd és vidd hello `AZSClient.framework` az Xcode project navigator be.
3. Válassza ki *elemek másolása, szükség esetén*, majd kattintson a *Befejezés*.
4. Kattintson a bal oldali navigációs hello projektre, majd a hello *általános* lapon hello projekt szerkesztő hello tetején.
5. A hello *csatolt keretrendszerek és könyvtárak* területen kattintson a hello Hozzáadás gomb (+).
6. A már megadott könyvtárak hello listájában keresse meg a `libxml2.2.tbd` és tooyour projekt hozzáadása.

## <a name="import-hello-library"></a>Importálás hello könyvtár 
```objc
// Include hello following import statement toouse blob APIs.
#import <AZSClient/AZSClient.h>
```

Swift használ, ha egy áthidalási fejlécet toocreate kell, és ott importálni < AZSClient/AZSClient.h >:

1. Hozzon létre egy fejlécfájlt `Bridging-Header.h`, és adja hozzá a hello fent importálási utasítást.
2. Nyissa meg toohello *Build Settings* lapot, és keresse meg *Objective-C Bridging fejléc*.
3. Kattintson duplán arra a hello mezője *Objective-C Bridging fejléc* , és adja hozzá az elérési út tooyour fejlécfájlt hello:`ProjectName/Bridging-Header.h`
4. Build hello project (⌘ + B) tooverify áthidalási fejléc hello Xcode lett észlelnie.
5. Indítsa el a hello könyvtár közvetlenül a Swift fájl használatával, és nincs szükség az importálási utasítást.

[!INCLUDE [storage-mobile-authentication-guidance](../../../includes/storage-mobile-authentication-guidance.md)]

## <a name="asynchronous-operations"></a>Az aszinkron műveletek
> [!NOTE]
> Minden hello szolgáltatást egy kérelmet végző módszereket aszinkron műveletek. Hello mintakódok talál, ezek a módszerek rendelkezik-e a befejezési kezelő. Hello befejezési kezelő kódot fog futni **után** hello kérelem teljesítése. Miután hello befejezési kezelő akkor futtatható kód **közben** hello kérelem történik.
> 
> 

## <a name="create-a-container"></a>Tároló létrehozása
Az Azure Storage összes blobjának egy tárolóban kell lennie. hello következő példa bemutatja, hogyan toocreate egy tároló nevű *newcontainer*, ha még nem létezik a tárfiókban lévő. A tároló nevét kiválasztásakor kell elnevezési szabályait, fent említett hello szem előtt tartva.

```objc
-(void)createContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"newcontainer"];

    // Create container in your Storage account if hello container doesn't already exist
    [blobContainer createContainerIfNotExistsWithCompletionHandler:^(NSError *error, BOOL exists) {
        if (error){
            NSLog(@"Error in creating container.");
        }
    }];
}
```

Úgy ellenőrizheti, ha megnézi a hello működőképességéről [Microsoft Azure Tártallózó](http://storageexplorer.com) és ellenőrizze, hogy *newcontainer* hello a tárfiók tárolók listája szerepel.

## <a name="set-container-permissions"></a>Tároló engedélyeinek beállítása
Egy tároló-engedélyek vannak beállítva a **titkos** alapértelmezés szerint a hozzáférés. Azonban tárolók biztosítják a tároló hozzáférés különböző lehetőségek közül néhány:

* **Személyes**: tároló és a blob adatainak által hello fiók tulajdonosának csak olvasható.
* **A BLOB**: Blobadatok található olvasható névtelen kérelem keresztül, de adatai nem érhető el. Ügyfelek névtelen kérelem keresztül hello tárolóban található blobok nem tudja felsorolni.
* **Tároló**: tároló és a blob adatainak keresztül névtelen kérelem olvasható. Ügyfelek névtelen kérelem keresztül hello tárolóban található blobok enumerálása, de nem tudja felsorolni a tárolók hello tárfiókon belül.

hello alábbi példa bemutatja, hogyan toocreate a tárolóhoz **tároló** hozzáférési engedélyek, amelyek hello Internet összes felhasználója számára engedélyezi a nyilvános, csak olvasható hozzáférést:

```objc
-(void)createContainerWithPublicAccess{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Create container in your Storage account if hello container doesn't already exist
    [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists){
        if (error){
            NSLog(@"Error in creating container.");
        }
    }];
}
```

## <a name="upload-a-blob-into-a-container"></a>Blobok feltöltése a tárolóba
A hello [Blob szolgáltatással kapcsolatos fogalmak](#blob-service-concepts) szakaszban, a Blob Storage blobok három különböző típusú biztosít: blokkblobokat, hozzáfűző blobokat és lapblobokat. hello Azure Storage iOS kódtára támogatja az összes háromféle blobot. A legtöbb esetben blokkblob típusú toouse ajánlott hello.

hello a következő példa bemutatja, hogyan tooupload egy blokk egy NSString a blob. Ha egy blobot a hello ugyanazzal a névvel már létezik ebben a tárolóban, a blob tartalmát hello felülíródik.

```objc
-(void)uploadBlobToContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists)
        {
            if (error){
                NSLog(@"Error in creating container.");
            }
            else{
                // Create a local blob object
                AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

                // Upload blob tooStorage
                [blockBlob uploadFromText:@"This text will be uploaded tooBlob Storage." completionHandler:^(NSError *error) {
                    if (error){
                        NSLog(@"Error in creating blob.");
                    }
                }];
            }
        }];
}
```

Úgy ellenőrizheti, ha megnézi a hello működőképességéről [Microsoft Azure Tártallózó](http://storageexplorer.com) és ellenőrzése, hogy hello tároló *containerpublic*, hello blob tartalmaz *sampleblob*. Ez a példa használtuk nyilvános tárolókban úgy is ellenőrizheti, hogy az alkalmazás által is toohello BLOB URI dolgozott:

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

Továbbá a blokkblob egy NSString a toouploading, hasonló mód létezik NSData, NSInputStream vagy egy helyi fájl.

## <a name="list-hello-blobs-in-a-container"></a>Lista hello a tárolóban lévő blobok
a következő példa azt mutatja meg, hogyan összes toolist a tárolóban lévő blobok hello. Ez a művelet végrehajtásakor kell szem előtt tartva a következő paraméterek hello:     

* **continuationToken** -hello folytatási token jelöli, ahol művelet listázása hello kell kezdődnie. Ha nincs jogkivonat van megadva, blobok hello elejétől felsorolja. Korlátlan számú blobot is listázva lehet, nulla mentése tooa maximális száma. Akkor is, ha ez a módszer nulla eredményeket ad vissza, ha `results.continuationToken` értéke nem nulla, előfordulhat, hogy további blobok hello szolgáltatás, amely nem szerepel.
* **előtag** -hello előtag toouse blob felsorolást is megadhat. Csak a előtaggal kezdődő blobok jelenik meg.
* **Listblobs** – hello a [elnevezési és a tárolók és blobok hivatkozó](#naming-and-referencing-containers-and-blobs) szakaszban hello Blob szolgáltatás azonban egy egyszerű tároló séma hozhat létre egy virtuális hierarchia történő elérési úttal rendelkező blobok elnevezésével információ. Azonban nem simán listaelem jelenleg nem támogatott. Ez a funkció hamarosan elérhető. Most ezt az értéket kell **Igen**.
* **blobListingDetails** -blobok listázása során megadhatja mely elemek tooinclude
  * _AZSBlobListingDetailsNone_: csak a véglegesített blobok listázása, és nem ad vissza a blob metaadatait.
  * _AZSBlobListingDetailsSnapshots_: véglegesített blobok listázása és a blob-pillanatképeket.
  * _AZSBlobListingDetailsMetadata_: minden egyes blob lekérése blob metaadatait hello lista tartalmazza az adott vissza.
  * _AZSBlobListingDetailsUncommittedBlobs_: véglegesített és a nem véglegesített blobok listázása.
  * _AZSBlobListingDetailsCopy_: közé tartozik a másolási hello listaelem-tulajdonságokat.
  * _AZSBlobListingDetailsAll_: kilistázhatja az összes elérhető véglegesített BLOB, a nem véglegesített blobok és pillanatképeket, és térjen vissza az ezeket a blobok minden metaadatok és a példány állapota.
* **maxResults** -hello eredmények tooreturn ehhez a művelethez maximális számát. -1 érték toonot korlátozni.
* **completionHandler** -művelet listázása hello hello eredményeit a hello kód tooexecute blokkjának.

Ebben a példában egy segédmetódust használt toorecursively telefonhívás hello lista blobok módban, minden alkalommal, amikor a folytatási kód adja vissza.

```objc
-(void)listBlobsInContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    //List all blobs in container
    [self listBlobsInContainerHelper:blobContainer continuationToken:nil prefix:nil blobListingDetails:AZSBlobListingDetailsAll maxResults:-1 completionHandler:^(NSError *error) {
        if (error != nil){
            NSLog(@"Error in creating container.");
        }
    }];
}

//List blobs helper method
-(void)listBlobsInContainerHelper:(AZSCloudBlobContainer *)container continuationToken:(AZSContinuationToken *)continuationToken prefix:(NSString *)prefix blobListingDetails:(AZSBlobListingDetails)blobListingDetails maxResults:(NSUInteger)maxResults completionHandler:(void (^)(NSError *))completionHandler
{
    [container listBlobsSegmentedWithContinuationToken:continuationToken prefix:prefix useFlatBlobListing:YES blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:^(NSError *error, AZSBlobResultSegment *results) {
        if (error)
        {
            completionHandler(error);
        }
        else
        {
            for (int i = 0; i < results.blobs.count; i++) {
                NSLog(@"%@",[(AZSCloudBlockBlob *)results.blobs[i] blobName]);
            }
            if (results.continuationToken)
            {
                [self listBlobsInContainerHelper:container continuationToken:results.continuationToken prefix:prefix blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:completionHandler];
            }
            else
            {
                completionHandler(nil);
            }
        }
    }];
}
```

## <a name="download-a-blob"></a>Blob letöltése
a következő példa azt mutatja meg hogyan hello toodownload egy blob tooa NSString objektum.

```objc
-(void)downloadBlobToString{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Create a local blob object
    AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

    // Download blob
    [blockBlob downloadToTextWithCompletionHandler:^(NSError *error, NSString *text) {
        if (error) {
            NSLog(@"Error in downloading blob");
        }
        else{
            NSLog(@"%@",text);
        }
    }];
}
```

## <a name="delete-a-blob"></a>Blob törlése
a következő példa azt mutatja meg hogyan hello toodelete blob.

```objc
-(void)deleteBlob{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Create a local blob object
    AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob1"];

    // Delete blob
    [blockBlob deleteWithCompletionHandler:^(NSError *error) {
        if (error) {
            NSLog(@"Error in deleting blob.");
        }
    }];
}
```

## <a name="delete-a-blob-container"></a>A blob-tároló törlése
a következő példa azt mutatja meg hogyan hello toodelete egy tárolót.

```objc
-(void)deleteContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Delete container
    [blobContainer deleteContainerIfExistsWithCompletionHandler:^(NSError *error, BOOL success) {
        if(error){
            NSLog(@"Error in deleting container");
        }
    }];
}
```

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte hogyan iOS, a Blob Storage toouse kövesse ezeket hello iOS-könyvtár ismertetése További hivatkozások toolearn, és hello tároló szolgáltatást.

* [Az IOS rendszerhez készült Azure Storage ügyféloldali kódtár](https://github.com/azure/azure-storage-ios)
* [Az Azure Storage iOS Referenciadokumentációt](http://azure.github.io/azure-storage-ios/)
* [Az Azure Storage-szolgáltatások REST API-ja](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Az Azure Storage csapat blogja](http://blogs.msdn.com/b/windowsazurestorage)

Ha ezt a szalagtárat kapcsolatos kérdése van, érzi, hogy szabad toopost tooour [MSDN Azure fórumon](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) vagy [Stack Overflow](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).
Ha az Azure Storage szolgáltatási javaslataikat, tegye túl[Azure Storage visszajelzés](https://feedback.azure.com/forums/217298-storage/).

