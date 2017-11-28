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
# <a name="how-toouse-blob-storage-from-ios"></a><span data-ttu-id="bb157-103">Hogyan toouse Blob storage-ának iOS</span><span class="sxs-lookup"><span data-stu-id="bb157-103">How toouse Blob storage from iOS</span></span>
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="bb157-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="bb157-104">Overview</span></span>
<span data-ttu-id="bb157-105">Ez a cikk bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz a Microsoft Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="bb157-105">This article will show you how tooperform common scenarios using Microsoft Azure Blob storage.</span></span> <span data-ttu-id="bb157-106">hello minták Objective-C nyelven íródtak, és használja a hello [Azure Storage ügyféloldali kódtára a iOS](https://github.com/Azure/azure-storage-ios).</span><span class="sxs-lookup"><span data-stu-id="bb157-106">hello samples are written in Objective-C and use hello [Azure Storage Client Library for iOS](https://github.com/Azure/azure-storage-ios).</span></span> <span data-ttu-id="bb157-107">hello tárgyalt forgatókönyvekben szerepel a **feltöltése**, **felsoroló**, **letöltése**, és **törlése** blobokat.</span><span class="sxs-lookup"><span data-stu-id="bb157-107">hello scenarios covered include **uploading**, **listing**, **downloading**, and **deleting** blobs.</span></span> <span data-ttu-id="bb157-108">A blobok további információkért lásd: hello [lépések](#next-steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="bb157-108">For more information on blobs, see hello [Next Steps](#next-steps) section.</span></span> <span data-ttu-id="bb157-109">Emellett letöltheti a hello [mintaalkalmazás](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) tooquickly lásd: hello iOS-alkalmazás az Azure Storage használatát.</span><span class="sxs-lookup"><span data-stu-id="bb157-109">You can also download hello [sample app](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) tooquickly see hello use of Azure Storage in an iOS application.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="import-hello-azure-storage-ios-library-into-your-application"></a><span data-ttu-id="bb157-110">Hello Azure Storage iOS kódtár importálnia kell az alkalmazás</span><span class="sxs-lookup"><span data-stu-id="bb157-110">Import hello Azure Storage iOS library into your application</span></span>
<span data-ttu-id="bb157-111">Importálhatja hello Azure Storage iOS könyvtár az alkalmazásba vagy hello segítségével [Azure Storage CocoaPod](https://cocoapods.org/pods/AZSClient) vagy hello importálásával **keretrendszer** fájlt.</span><span class="sxs-lookup"><span data-stu-id="bb157-111">You can import hello Azure Storage iOS library into your application either by using hello [Azure Storage CocoaPod](https://cocoapods.org/pods/AZSClient) or by importing hello **Framework** file.</span></span> <span data-ttu-id="bb157-112">CocoaPod hello integráló hello könyvtár azonban hello keretrendszer fájlból való importálása a meglévő projekthez kevesebb zavaró egyszerűbbé teszi ajánlott módja.</span><span class="sxs-lookup"><span data-stu-id="bb157-112">CocoaPod is hello recommended way as it makes integrating hello library easier, however importing from hello framework file is less intrusive for your existing project.</span></span>

<span data-ttu-id="bb157-113">toouse ebben a könyvtárban van szüksége a következő hello:</span><span class="sxs-lookup"><span data-stu-id="bb157-113">toouse this library, you need hello following:</span></span>
- <span data-ttu-id="bb157-114">iOS 8 +</span><span class="sxs-lookup"><span data-stu-id="bb157-114">iOS 8+</span></span>
- <span data-ttu-id="bb157-115">Xcode 7 +</span><span class="sxs-lookup"><span data-stu-id="bb157-115">Xcode 7+</span></span>

## <a name="cocoapod"></a><span data-ttu-id="bb157-116">CocoaPod</span><span class="sxs-lookup"><span data-stu-id="bb157-116">CocoaPod</span></span>
1. <span data-ttu-id="bb157-117">Ha még nem tette, [telepítése CocoaPods](https://guides.cocoapods.org/using/getting-started.html#toc_3) a számítógépen nyisson meg egy terminálablakot, és hello a következő parancs futtatása</span><span class="sxs-lookup"><span data-stu-id="bb157-117">If you haven't done so already, [Install CocoaPods](https://guides.cocoapods.org/using/getting-started.html#toc_3) on your computer by opening a terminal window and running hello following command</span></span>
    
    ```shell   
    sudo gem install cocoapods
    ```

2. <span data-ttu-id="bb157-118">A következő hello projektkönyvtár (hello a .xcodeproj fájlt tartalmazó könyvtár), hozzon létre egy új fájlt nevű _Podfile_(nincs fájl kiterjesztése).</span><span class="sxs-lookup"><span data-stu-id="bb157-118">Next, in hello project directory (hello directory containing your .xcodeproj file), create a new file called _Podfile_(no file extension).</span></span> <span data-ttu-id="bb157-119">Adja hozzá a következő too_Podfile_ hello és mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="bb157-119">Add hello following too_Podfile_ and save.</span></span>

    ```ruby
    platform :ios, '8.0'

    target 'TargetName' do
      pod 'AZSClient'
    end
    ```

3. <span data-ttu-id="bb157-120">Hello terminálablakot lépjen a toohello projektkönyvtár és a következő parancs futtatása hello</span><span class="sxs-lookup"><span data-stu-id="bb157-120">In hello terminal window, navigate toohello project directory and run hello following command</span></span>

    ```shell    
    pod install
    ```

4. <span data-ttu-id="bb157-121">Ha a .xcodeproj nyissa meg az xcode-ban, zárja be.</span><span class="sxs-lookup"><span data-stu-id="bb157-121">If your .xcodeproj is open in Xcode, close it.</span></span> <span data-ttu-id="bb157-122">A projektben nyissa meg az újonnan létrehozott hello projektfájlban hello .xcworkspace bővítményt, amelyekről fog készülni.</span><span class="sxs-lookup"><span data-stu-id="bb157-122">In your project directory open hello newly created project file which will have hello .xcworkspace extension.</span></span> <span data-ttu-id="bb157-123">Ez az fogja dolgozik a most már a hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="bb157-123">This is hello file you'll work from for now on.</span></span>

## <a name="framework"></a><span data-ttu-id="bb157-124">Keretrendszer</span><span class="sxs-lookup"><span data-stu-id="bb157-124">Framework</span></span>
<span data-ttu-id="bb157-125">hello más módon toouse hello könyvtár toobuild hello keretrendszer manuálisan:</span><span class="sxs-lookup"><span data-stu-id="bb157-125">hello other way toouse hello library is toobuild hello framework manually:</span></span>

1. <span data-ttu-id="bb157-126">First, a letöltési vagy a Klónozás hello [azure-storage-ios tárház](https://github.com/azure/azure-storage-ios).</span><span class="sxs-lookup"><span data-stu-id="bb157-126">First, download or clone hello [azure-storage-ios repo](https://github.com/azure/azure-storage-ios).</span></span>
2. <span data-ttu-id="bb157-127">Nyissa meg a *azure-storage-ios* -> *Lib* -> *Azure Storage ügyféloldali kódtár*, és nyissa meg a `AZSClient.xcodeproj` az xcode-ban.</span><span class="sxs-lookup"><span data-stu-id="bb157-127">Go into *azure-storage-ios* -> *Lib* -> *Azure Storage Client Library*, and open `AZSClient.xcodeproj` in Xcode.</span></span>
3. <span data-ttu-id="bb157-128">A bal felső az Xcode-ban módosítás hello aktív hello sémáját az "Azure Storage ügyféloldali kódtár" túl "Keretrendszer".</span><span class="sxs-lookup"><span data-stu-id="bb157-128">At hello top-left of Xcode, change hello active scheme from "Azure Storage Client Library" too"Framework".</span></span>
4. <span data-ttu-id="bb157-129">(⌘ + B) hello projekt felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="bb157-129">Build hello project (⌘+B).</span></span> <span data-ttu-id="bb157-130">Ezzel létrehoz egy `AZSClient.framework` fájl az asztalon.</span><span class="sxs-lookup"><span data-stu-id="bb157-130">This will create an `AZSClient.framework` file on your Desktop.</span></span>

<span data-ttu-id="bb157-131">Majd hello keretrendszer fájlt importálhatja az alkalmazásba hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="bb157-131">You can then import hello framework file into your application by doing hello following:</span></span>

1. <span data-ttu-id="bb157-132">Hozzon létre egy új projektet, vagy nyissa meg a meglévő projektet az xcode-ban.</span><span class="sxs-lookup"><span data-stu-id="bb157-132">Create a new project or open up your existing project in Xcode.</span></span>
2. <span data-ttu-id="bb157-133">A fogd és vidd hello `AZSClient.framework` az Xcode project navigator be.</span><span class="sxs-lookup"><span data-stu-id="bb157-133">Drag and drop hello `AZSClient.framework` into your Xcode project navigator.</span></span>
3. <span data-ttu-id="bb157-134">Válassza ki *elemek másolása, szükség esetén*, majd kattintson a *Befejezés*.</span><span class="sxs-lookup"><span data-stu-id="bb157-134">Select *Copy items if needed*, and click on *Finish*.</span></span>
4. <span data-ttu-id="bb157-135">Kattintson a bal oldali navigációs hello projektre, majd a hello *általános* lapon hello projekt szerkesztő hello tetején.</span><span class="sxs-lookup"><span data-stu-id="bb157-135">Click on your project in hello left-hand navigation and click hello *General* tab at hello top of hello project editor.</span></span>
5. <span data-ttu-id="bb157-136">A hello *csatolt keretrendszerek és könyvtárak* területen kattintson a hello Hozzáadás gomb (+).</span><span class="sxs-lookup"><span data-stu-id="bb157-136">Under hello *Linked Frameworks and Libraries* section, click hello Add button (+).</span></span>
6. <span data-ttu-id="bb157-137">A már megadott könyvtárak hello listájában keresse meg a `libxml2.2.tbd` és tooyour projekt hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="bb157-137">In hello list of libraries already provided, search for `libxml2.2.tbd` and add it tooyour project.</span></span>

## <a name="import-hello-library"></a><span data-ttu-id="bb157-138">Importálás hello könyvtár</span><span class="sxs-lookup"><span data-stu-id="bb157-138">Import hello Library</span></span> 
```objc
// Include hello following import statement toouse blob APIs.
#import <AZSClient/AZSClient.h>
```

<span data-ttu-id="bb157-139">Swift használ, ha egy áthidalási fejlécet toocreate kell, és ott importálni < AZSClient/AZSClient.h >:</span><span class="sxs-lookup"><span data-stu-id="bb157-139">If you are using Swift, you will need toocreate a bridging header and import <AZSClient/AZSClient.h> there:</span></span>

1. <span data-ttu-id="bb157-140">Hozzon létre egy fejlécfájlt `Bridging-Header.h`, és adja hozzá a hello fent importálási utasítást.</span><span class="sxs-lookup"><span data-stu-id="bb157-140">Create a header file `Bridging-Header.h`, and add hello above import statement.</span></span>
2. <span data-ttu-id="bb157-141">Nyissa meg toohello *Build Settings* lapot, és keresse meg *Objective-C Bridging fejléc*.</span><span class="sxs-lookup"><span data-stu-id="bb157-141">Go toohello *Build Settings* tab, and search for *Objective-C Bridging Header*.</span></span>
3. <span data-ttu-id="bb157-142">Kattintson duplán arra a hello mezője *Objective-C Bridging fejléc* , és adja hozzá az elérési út tooyour fejlécfájlt hello:`ProjectName/Bridging-Header.h`</span><span class="sxs-lookup"><span data-stu-id="bb157-142">Double-click on hello field of *Objective-C Bridging Header* and add hello path tooyour header file: `ProjectName/Bridging-Header.h`</span></span>
4. <span data-ttu-id="bb157-143">Build hello project (⌘ + B) tooverify áthidalási fejléc hello Xcode lett észlelnie.</span><span class="sxs-lookup"><span data-stu-id="bb157-143">Build hello project (⌘+B) tooverify that hello bridging header was picked up by Xcode.</span></span>
5. <span data-ttu-id="bb157-144">Indítsa el a hello könyvtár közvetlenül a Swift fájl használatával, és nincs szükség az importálási utasítást.</span><span class="sxs-lookup"><span data-stu-id="bb157-144">Start using hello library directly in any Swift file, there is no need for import statements.</span></span>

[!INCLUDE [storage-mobile-authentication-guidance](../../../includes/storage-mobile-authentication-guidance.md)]

## <a name="asynchronous-operations"></a><span data-ttu-id="bb157-145">Az aszinkron műveletek</span><span class="sxs-lookup"><span data-stu-id="bb157-145">Asynchronous Operations</span></span>
> [!NOTE]
> <span data-ttu-id="bb157-146">Minden hello szolgáltatást egy kérelmet végző módszereket aszinkron műveletek.</span><span class="sxs-lookup"><span data-stu-id="bb157-146">All methods that perform a request against hello service are asynchronous operations.</span></span> <span data-ttu-id="bb157-147">Hello mintakódok talál, ezek a módszerek rendelkezik-e a befejezési kezelő.</span><span class="sxs-lookup"><span data-stu-id="bb157-147">In hello code samples, you'll find that these methods have a completion handler.</span></span> <span data-ttu-id="bb157-148">Hello befejezési kezelő kódot fog futni **után** hello kérelem teljesítése.</span><span class="sxs-lookup"><span data-stu-id="bb157-148">Code inside hello completion handler will run **after** hello request is completed.</span></span> <span data-ttu-id="bb157-149">Miután hello befejezési kezelő akkor futtatható kód **közben** hello kérelem történik.</span><span class="sxs-lookup"><span data-stu-id="bb157-149">Code after hello completion handler will run **while** hello request is being made.</span></span>
> 
> 

## <a name="create-a-container"></a><span data-ttu-id="bb157-150">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="bb157-150">Create a container</span></span>
<span data-ttu-id="bb157-151">Az Azure Storage összes blobjának egy tárolóban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="bb157-151">Every blob in Azure Storage must reside in a container.</span></span> <span data-ttu-id="bb157-152">hello következő példa bemutatja, hogyan toocreate egy tároló nevű *newcontainer*, ha még nem létezik a tárfiókban lévő.</span><span class="sxs-lookup"><span data-stu-id="bb157-152">hello following example shows how toocreate a container, called *newcontainer*, in your Storage account if it doesn't already exist.</span></span> <span data-ttu-id="bb157-153">A tároló nevét kiválasztásakor kell elnevezési szabályait, fent említett hello szem előtt tartva.</span><span class="sxs-lookup"><span data-stu-id="bb157-153">When choosing a name for your container, be mindful of hello naming rules mentioned above.</span></span>

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

<span data-ttu-id="bb157-154">Úgy ellenőrizheti, ha megnézi a hello működőképességéről [Microsoft Azure Tártallózó](http://storageexplorer.com) és ellenőrizze, hogy *newcontainer* hello a tárfiók tárolók listája szerepel.</span><span class="sxs-lookup"><span data-stu-id="bb157-154">You can confirm that this works by looking at hello [Microsoft Azure Storage Explorer](http://storageexplorer.com) and verifying that *newcontainer* is in hello list of containers for your Storage account.</span></span>

## <a name="set-container-permissions"></a><span data-ttu-id="bb157-155">Tároló engedélyeinek beállítása</span><span class="sxs-lookup"><span data-stu-id="bb157-155">Set Container Permissions</span></span>
<span data-ttu-id="bb157-156">Egy tároló-engedélyek vannak beállítva a **titkos** alapértelmezés szerint a hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="bb157-156">A container's permissions are configured for **Private** access by default.</span></span> <span data-ttu-id="bb157-157">Azonban tárolók biztosítják a tároló hozzáférés különböző lehetőségek közül néhány:</span><span class="sxs-lookup"><span data-stu-id="bb157-157">However, containers provide a few different options for container access:</span></span>

* <span data-ttu-id="bb157-158">**Személyes**: tároló és a blob adatainak által hello fiók tulajdonosának csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="bb157-158">**Private**: Container and blob data can be read by hello account owner only.</span></span>
* <span data-ttu-id="bb157-159">**A BLOB**: Blobadatok található olvasható névtelen kérelem keresztül, de adatai nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="bb157-159">**Blob**: Blob data within this container can be read via anonymous request, but container data is not available.</span></span> <span data-ttu-id="bb157-160">Ügyfelek névtelen kérelem keresztül hello tárolóban található blobok nem tudja felsorolni.</span><span class="sxs-lookup"><span data-stu-id="bb157-160">Clients cannot enumerate blobs within hello container via anonymous request.</span></span>
* <span data-ttu-id="bb157-161">**Tároló**: tároló és a blob adatainak keresztül névtelen kérelem olvasható.</span><span class="sxs-lookup"><span data-stu-id="bb157-161">**Container**: Container and blob data can be read via anonymous request.</span></span> <span data-ttu-id="bb157-162">Ügyfelek névtelen kérelem keresztül hello tárolóban található blobok enumerálása, de nem tudja felsorolni a tárolók hello tárfiókon belül.</span><span class="sxs-lookup"><span data-stu-id="bb157-162">Clients can enumerate blobs within hello container via anonymous request, but cannot enumerate containers within hello storage account.</span></span>

<span data-ttu-id="bb157-163">hello alábbi példa bemutatja, hogyan toocreate a tárolóhoz **tároló** hozzáférési engedélyek, amelyek hello Internet összes felhasználója számára engedélyezi a nyilvános, csak olvasható hozzáférést:</span><span class="sxs-lookup"><span data-stu-id="bb157-163">hello following example shows you how toocreate a container with **Container** access permissions, which will allow public, read-only access for all users on hello Internet:</span></span>

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

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="bb157-164">Blobok feltöltése a tárolóba</span><span class="sxs-lookup"><span data-stu-id="bb157-164">Upload a blob into a container</span></span>
<span data-ttu-id="bb157-165">A hello [Blob szolgáltatással kapcsolatos fogalmak](#blob-service-concepts) szakaszban, a Blob Storage blobok három különböző típusú biztosít: blokkblobokat, hozzáfűző blobokat és lapblobokat.</span><span class="sxs-lookup"><span data-stu-id="bb157-165">As mentioned in hello [Blob service concepts](#blob-service-concepts) section, Blob Storage offers three different types of blobs: block blobs, append blobs, and page blobs.</span></span> <span data-ttu-id="bb157-166">hello Azure Storage iOS kódtára támogatja az összes háromféle blobot.</span><span class="sxs-lookup"><span data-stu-id="bb157-166">hello Azure Storage iOS library supports all three types of blobs.</span></span> <span data-ttu-id="bb157-167">A legtöbb esetben blokkblob típusú toouse ajánlott hello.</span><span class="sxs-lookup"><span data-stu-id="bb157-167">In most cases, block blob is hello recommended type toouse.</span></span>

<span data-ttu-id="bb157-168">hello a következő példa bemutatja, hogyan tooupload egy blokk egy NSString a blob.</span><span class="sxs-lookup"><span data-stu-id="bb157-168">hello following example shows how tooupload a block blob from an NSString.</span></span> <span data-ttu-id="bb157-169">Ha egy blobot a hello ugyanazzal a névvel már létezik ebben a tárolóban, a blob tartalmát hello felülíródik.</span><span class="sxs-lookup"><span data-stu-id="bb157-169">If a blob with hello same name already exists in this container, hello contents of this blob will be overwritten.</span></span>

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

<span data-ttu-id="bb157-170">Úgy ellenőrizheti, ha megnézi a hello működőképességéről [Microsoft Azure Tártallózó](http://storageexplorer.com) és ellenőrzése, hogy hello tároló *containerpublic*, hello blob tartalmaz *sampleblob*.</span><span class="sxs-lookup"><span data-stu-id="bb157-170">You can confirm that this works by looking at hello [Microsoft Azure Storage Explorer](http://storageexplorer.com) and verifying that hello container, *containerpublic*, contains hello blob, *sampleblob*.</span></span> <span data-ttu-id="bb157-171">Ez a példa használtuk nyilvános tárolókban úgy is ellenőrizheti, hogy az alkalmazás által is toohello BLOB URI dolgozott:</span><span class="sxs-lookup"><span data-stu-id="bb157-171">In this sample, we used a public container so you can also verify that this application worked by going toohello blobs URI:</span></span>

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

<span data-ttu-id="bb157-172">Továbbá a blokkblob egy NSString a toouploading, hasonló mód létezik NSData, NSInputStream vagy egy helyi fájl.</span><span class="sxs-lookup"><span data-stu-id="bb157-172">In addition toouploading a block blob from an NSString, similar methods exist for NSData, NSInputStream, or a local file.</span></span>

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="bb157-173">Lista hello a tárolóban lévő blobok</span><span class="sxs-lookup"><span data-stu-id="bb157-173">List hello blobs in a container</span></span>
<span data-ttu-id="bb157-174">a következő példa azt mutatja meg, hogyan összes toolist a tárolóban lévő blobok hello.</span><span class="sxs-lookup"><span data-stu-id="bb157-174">hello following example shows how toolist all blobs in a container.</span></span> <span data-ttu-id="bb157-175">Ez a művelet végrehajtásakor kell szem előtt tartva a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="bb157-175">When performing this operation, be mindful of hello following parameters:</span></span>     

* <span data-ttu-id="bb157-176">**continuationToken** -hello folytatási token jelöli, ahol művelet listázása hello kell kezdődnie.</span><span class="sxs-lookup"><span data-stu-id="bb157-176">**continuationToken** - hello continuation token represents where hello listing operation should start.</span></span> <span data-ttu-id="bb157-177">Ha nincs jogkivonat van megadva, blobok hello elejétől felsorolja.</span><span class="sxs-lookup"><span data-stu-id="bb157-177">If no token is provided, it will list blobs from hello beginning.</span></span> <span data-ttu-id="bb157-178">Korlátlan számú blobot is listázva lehet, nulla mentése tooa maximális száma.</span><span class="sxs-lookup"><span data-stu-id="bb157-178">Any number of blobs can be listed, from zero up tooa set maximum.</span></span> <span data-ttu-id="bb157-179">Akkor is, ha ez a módszer nulla eredményeket ad vissza, ha `results.continuationToken` értéke nem nulla, előfordulhat, hogy további blobok hello szolgáltatás, amely nem szerepel.</span><span class="sxs-lookup"><span data-stu-id="bb157-179">Even if this method returns zero results, if `results.continuationToken` is not nil, there may be more blobs on hello service that have not been listed.</span></span>
* <span data-ttu-id="bb157-180">**előtag** -hello előtag toouse blob felsorolást is megadhat.</span><span class="sxs-lookup"><span data-stu-id="bb157-180">**prefix** - You can specify hello prefix toouse for blob listing.</span></span> <span data-ttu-id="bb157-181">Csak a előtaggal kezdődő blobok jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="bb157-181">Only blobs that begin with this prefix will be listed.</span></span>
* <span data-ttu-id="bb157-182">**Listblobs** – hello a [elnevezési és a tárolók és blobok hivatkozó](#naming-and-referencing-containers-and-blobs) szakaszban hello Blob szolgáltatás azonban egy egyszerű tároló séma hozhat létre egy virtuális hierarchia történő elérési úttal rendelkező blobok elnevezésével információ.</span><span class="sxs-lookup"><span data-stu-id="bb157-182">**useFlatBlobListing** - As mentioned in hello [Naming and referencing containers and blobs](#naming-and-referencing-containers-and-blobs) section, although hello Blob service is a flat storage scheme, you can create a virtual hierarchy by naming blobs with path information.</span></span> <span data-ttu-id="bb157-183">Azonban nem simán listaelem jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="bb157-183">However, non-flat listing is currently not supported.</span></span> <span data-ttu-id="bb157-184">Ez a funkció hamarosan elérhető.</span><span class="sxs-lookup"><span data-stu-id="bb157-184">This feature is coming soon.</span></span> <span data-ttu-id="bb157-185">Most ezt az értéket kell **Igen**.</span><span class="sxs-lookup"><span data-stu-id="bb157-185">For now, this value should be **YES**.</span></span>
* <span data-ttu-id="bb157-186">**blobListingDetails** -blobok listázása során megadhatja mely elemek tooinclude</span><span class="sxs-lookup"><span data-stu-id="bb157-186">**blobListingDetails** - You can specify which items tooinclude when listing blobs</span></span>
  * <span data-ttu-id="bb157-187">_AZSBlobListingDetailsNone_: csak a véglegesített blobok listázása, és nem ad vissza a blob metaadatait.</span><span class="sxs-lookup"><span data-stu-id="bb157-187">_AZSBlobListingDetailsNone_: List only committed blobs, and do not return blob metadata.</span></span>
  * <span data-ttu-id="bb157-188">_AZSBlobListingDetailsSnapshots_: véglegesített blobok listázása és a blob-pillanatképeket.</span><span class="sxs-lookup"><span data-stu-id="bb157-188">_AZSBlobListingDetailsSnapshots_: List committed blobs and blob snapshots.</span></span>
  * <span data-ttu-id="bb157-189">_AZSBlobListingDetailsMetadata_: minden egyes blob lekérése blob metaadatait hello lista tartalmazza az adott vissza.</span><span class="sxs-lookup"><span data-stu-id="bb157-189">_AZSBlobListingDetailsMetadata_: Retrieve blob metadata for each blob returned in hello listing.</span></span>
  * <span data-ttu-id="bb157-190">_AZSBlobListingDetailsUncommittedBlobs_: véglegesített és a nem véglegesített blobok listázása.</span><span class="sxs-lookup"><span data-stu-id="bb157-190">_AZSBlobListingDetailsUncommittedBlobs_: List committed and uncommitted blobs.</span></span>
  * <span data-ttu-id="bb157-191">_AZSBlobListingDetailsCopy_: közé tartozik a másolási hello listaelem-tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="bb157-191">_AZSBlobListingDetailsCopy_: Include copy properties in hello listing.</span></span>
  * <span data-ttu-id="bb157-192">_AZSBlobListingDetailsAll_: kilistázhatja az összes elérhető véglegesített BLOB, a nem véglegesített blobok és pillanatképeket, és térjen vissza az ezeket a blobok minden metaadatok és a példány állapota.</span><span class="sxs-lookup"><span data-stu-id="bb157-192">_AZSBlobListingDetailsAll_: List all available committed blobs, uncommitted blobs, and snapshots, and return all metadata and copy status for those blobs.</span></span>
* <span data-ttu-id="bb157-193">**maxResults** -hello eredmények tooreturn ehhez a művelethez maximális számát.</span><span class="sxs-lookup"><span data-stu-id="bb157-193">**maxResults** - hello maximum number of results tooreturn for this operation.</span></span> <span data-ttu-id="bb157-194">-1 érték toonot korlátozni.</span><span class="sxs-lookup"><span data-stu-id="bb157-194">Use -1 toonot set a limit.</span></span>
* <span data-ttu-id="bb157-195">**completionHandler** -művelet listázása hello hello eredményeit a hello kód tooexecute blokkjának.</span><span class="sxs-lookup"><span data-stu-id="bb157-195">**completionHandler** - hello block of code tooexecute with hello results of hello listing operation.</span></span>

<span data-ttu-id="bb157-196">Ebben a példában egy segédmetódust használt toorecursively telefonhívás hello lista blobok módban, minden alkalommal, amikor a folytatási kód adja vissza.</span><span class="sxs-lookup"><span data-stu-id="bb157-196">In this example, a helper method is used toorecursively call hello list blobs method every time a continuation token is returned.</span></span>

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

## <a name="download-a-blob"></a><span data-ttu-id="bb157-197">Blob letöltése</span><span class="sxs-lookup"><span data-stu-id="bb157-197">Download a blob</span></span>
<span data-ttu-id="bb157-198">a következő példa azt mutatja meg hogyan hello toodownload egy blob tooa NSString objektum.</span><span class="sxs-lookup"><span data-stu-id="bb157-198">hello following example shows how toodownload a blob tooa NSString object.</span></span>

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

## <a name="delete-a-blob"></a><span data-ttu-id="bb157-199">Blob törlése</span><span class="sxs-lookup"><span data-stu-id="bb157-199">Delete a blob</span></span>
<span data-ttu-id="bb157-200">a következő példa azt mutatja meg hogyan hello toodelete blob.</span><span class="sxs-lookup"><span data-stu-id="bb157-200">hello following example shows how toodelete a blob.</span></span>

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

## <a name="delete-a-blob-container"></a><span data-ttu-id="bb157-201">A blob-tároló törlése</span><span class="sxs-lookup"><span data-stu-id="bb157-201">Delete a blob container</span></span>
<span data-ttu-id="bb157-202">a következő példa azt mutatja meg hogyan hello toodelete egy tárolót.</span><span class="sxs-lookup"><span data-stu-id="bb157-202">hello following example shows how toodelete a container.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="bb157-203">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bb157-203">Next steps</span></span>
<span data-ttu-id="bb157-204">Most, hogy megismerte hogyan iOS, a Blob Storage toouse kövesse ezeket hello iOS-könyvtár ismertetése További hivatkozások toolearn, és hello tároló szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="bb157-204">Now that you've learned how toouse Blob Storage from iOS, follow these links toolearn more about hello iOS library and hello Storage service.</span></span>

* [<span data-ttu-id="bb157-205">Az IOS rendszerhez készült Azure Storage ügyféloldali kódtár</span><span class="sxs-lookup"><span data-stu-id="bb157-205">Azure Storage Client Library for iOS</span></span>](https://github.com/azure/azure-storage-ios)
* [<span data-ttu-id="bb157-206">Az Azure Storage iOS Referenciadokumentációt</span><span class="sxs-lookup"><span data-stu-id="bb157-206">Azure Storage iOS Reference Documentation</span></span>](http://azure.github.io/azure-storage-ios/)
* [<span data-ttu-id="bb157-207">Az Azure Storage-szolgáltatások REST API-ja</span><span class="sxs-lookup"><span data-stu-id="bb157-207">Azure Storage Services REST API</span></span>](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [<span data-ttu-id="bb157-208">Az Azure Storage csapat blogja</span><span class="sxs-lookup"><span data-stu-id="bb157-208">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage)

<span data-ttu-id="bb157-209">Ha ezt a szalagtárat kapcsolatos kérdése van, érzi, hogy szabad toopost tooour [MSDN Azure fórumon](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) vagy [Stack Overflow](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).</span><span class="sxs-lookup"><span data-stu-id="bb157-209">If you have questions regarding this library, feel free toopost tooour [MSDN Azure forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) or [Stack Overflow](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).</span></span>
<span data-ttu-id="bb157-210">Ha az Azure Storage szolgáltatási javaslataikat, tegye túl[Azure Storage visszajelzés](https://feedback.azure.com/forums/217298-storage/).</span><span class="sxs-lookup"><span data-stu-id="bb157-210">If you have feature suggestions for Azure Storage, please post too[Azure Storage Feedback](https://feedback.azure.com/forums/217298-storage/).</span></span>

