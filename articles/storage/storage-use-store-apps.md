---
title: "a Windows Áruházbeli alkalmazások az Azure storage aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy Windows Áruházbeli alkalmazást, amely az Azure Blob, a várólista, a tábla vagy a fájl tárolót használ."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 63c4b29d-b2f2-4d7c-b164-a0d38f4d14f6
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: ac21b0695c0d77c1d81c1b921718fb929945d45e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-storage-in-windows-store-apps"></a><span data-ttu-id="e6fa2-103">Hogyan toouse Azure Storage a Windows Áruházbeli alkalmazások</span><span class="sxs-lookup"><span data-stu-id="e6fa2-103">How toouse Azure Storage in Windows Store apps</span></span>
## <a name="overview"></a><span data-ttu-id="e6fa2-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="e6fa2-104">Overview</span></span>
<span data-ttu-id="e6fa2-105">Ez az útmutató bemutatja, hogyan tooget használatába, amely lehetővé teszi az Azure Storage használata Windows Áruházbeli alkalmazást fejleszt.</span><span class="sxs-lookup"><span data-stu-id="e6fa2-105">This guide shows how tooget started with developing a Windows Store app that makes use of Azure Storage.</span></span>

## <a name="download-required-tools"></a><span data-ttu-id="e6fa2-106">Töltse le a szükséges eszközök</span><span class="sxs-lookup"><span data-stu-id="e6fa2-106">Download required tools</span></span>
* <span data-ttu-id="e6fa2-107">[A Visual Studio](https://www.visualstudio.com/downloads/) teszi, hogy könnyen toobuild debug localize, a csomagot, majd a Windows Áruházbeli alkalmazások telepítése.</span><span class="sxs-lookup"><span data-stu-id="e6fa2-107">[Visual Studio](https://www.visualstudio.com/downloads/) makes it easy toobuild, debug, localize, package, and deploy Windows Store apps.</span></span> <span data-ttu-id="e6fa2-108">A Visual Studio 2012 vagy újabb rendszer szükséges.</span><span class="sxs-lookup"><span data-stu-id="e6fa2-108">Visual Studio 2012 or later is required.</span></span>
* <span data-ttu-id="e6fa2-109">Hello [Azure Storage ügyféloldali kódtár](https://www.nuget.org/packages/WindowsAzure.Storage) egy Windows-Futtatókörnyezetű osztálytár biztosít az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="e6fa2-109">hello [Azure Storage Client Library](https://www.nuget.org/packages/WindowsAzure.Storage) provides a Windows Runtime class library for working with Azure Storage.</span></span>
* <span data-ttu-id="e6fa2-110">[A WCF Data Services eszközök Windows Áruházbeli alkalmazásokkal való](http://www.microsoft.com/download/details.aspx?id=30714) hello szolgáltatás hivatkozás hozzáadása élmény a Windows Áruházbeli alkalmazások a Visual Studio ügyféloldali OData támogató bővíti.</span><span class="sxs-lookup"><span data-stu-id="e6fa2-110">[WCF Data Services Tools for Windows Store Apps](http://www.microsoft.com/download/details.aspx?id=30714) extends hello Add Service Reference experience with client-side OData support for Windows Store apps in Visual Studio.</span></span>

## <a name="develop-apps"></a><span data-ttu-id="e6fa2-111">Alkalmazásfejlesztés</span><span class="sxs-lookup"><span data-stu-id="e6fa2-111">Develop apps</span></span>
### <a name="getting-ready"></a><span data-ttu-id="e6fa2-112">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="e6fa2-112">Getting ready</span></span>
<span data-ttu-id="e6fa2-113">Hozzon létre egy új Windows Áruházbeli alkalmazás projektjére a Visual Studio 2012 vagy újabb verzió:</span><span class="sxs-lookup"><span data-stu-id="e6fa2-113">Create a new Windows Store app project in Visual Studio 2012 or later:</span></span>

![tároló-alkalmazások-storage-Visual Studio-projekt][store-apps-storage-vs-project]

<span data-ttu-id="e6fa2-115">Ezután adja hozzá egy hivatkozást toohello Azure Storage ügyféloldali kódtár kattintson a jobb gombbal **hivatkozások**gombra, majd **hivatkozás hozzáadása**, és keresse meg a tároló ügyfél Library a Windows-Futtatókörnyezetű toohello során le:</span><span class="sxs-lookup"><span data-stu-id="e6fa2-115">Next, add a reference toohello Azure Storage Client Library by right-clicking **References**, clicking **Add Reference**, and then browsing toohello Storage Client Library for Windows Runtime that you downloaded:</span></span>

![tároló-alkalmazások-storage-válasszon-kódtár][store-apps-storage-choose-library]

### <a name="using-hello-library-with-hello-blob-and-queue-services"></a><span data-ttu-id="e6fa2-117">Hello függvénytár hello Blob és várólista-szolgáltatás használatával</span><span class="sxs-lookup"><span data-stu-id="e6fa2-117">Using hello library with hello Blob and Queue services</span></span>
<span data-ttu-id="e6fa2-118">Ezen a ponton a az alkalmazás készen áll a toocall hello Azure Blob és a Queue szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="e6fa2-118">At this point, your app is ready toocall hello Azure Blob and Queue services.</span></span> <span data-ttu-id="e6fa2-119">Adja hozzá a következő hello **használatával** utasításokat, hogy az Azure Storage típusok közvetlenül lehet hivatkozni:</span><span class="sxs-lookup"><span data-stu-id="e6fa2-119">Add hello following **using** statements so that Azure Storage types can be referenced directly:</span></span>

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
```

<span data-ttu-id="e6fa2-120">Ezután adja hozzá a gomb tooyour lapja.</span><span class="sxs-lookup"><span data-stu-id="e6fa2-120">Next, add a button tooyour page.</span></span> <span data-ttu-id="e6fa2-121">Adja hozzá a következő kód tooits hello **kattintson** esemény, és módosítsa a eseménykezelő metódus hello segítségével [aszinkron kulcsszó](http://msdn.microsoft.com/library/vstudio/hh156513.aspx):</span><span class="sxs-lookup"><span data-stu-id="e6fa2-121">Add hello following code tooits **Click** event and modify your event handler method by using hello [async keyword](http://msdn.microsoft.com/library/vstudio/hh156513.aspx):</span></span>

```csharp
var credentials = new StorageCredentials(accountName, accountKey);
var account = new CloudStorageAccount(credentials, true);
var blobClient = account.CreateCloudBlobClient();
var container = blobClient.GetContainerReference("container1");
await container.CreateIfNotExistsAsync();
```

<span data-ttu-id="e6fa2-122">Ez a kód feltételezi, hogy rendelkezik-e két karakterlánc típusú változót, *accountName* és *accountKey*.</span><span class="sxs-lookup"><span data-stu-id="e6fa2-122">This code assumes that you have two string variables, *accountName* and *accountKey*.</span></span> <span data-ttu-id="e6fa2-123">A tárolási fiók és hello fiókkulcs azzal a fiókkal társított hello neve jelölnek.</span><span class="sxs-lookup"><span data-stu-id="e6fa2-123">They represent hello name of your storage account and hello account key that is associated with that account.</span></span>

<span data-ttu-id="e6fa2-124">Hozza létre és hello alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="e6fa2-124">Build and run hello application.</span></span> <span data-ttu-id="e6fa2-125">Gombra kattintva hello gomb ellenőrzi, hogy a tároló nevű *container1* a fiók létezik, és majd hozza létre, ha nem.</span><span class="sxs-lookup"><span data-stu-id="e6fa2-125">Clicking hello button will check whether a container named *container1* exists in your account and then create it if not.</span></span>

### <a name="using-hello-library-with-hello-table-service"></a><span data-ttu-id="e6fa2-126">A Table szolgáltatás hello hello szalagtár használata</span><span class="sxs-lookup"><span data-stu-id="e6fa2-126">Using hello library with hello Table service</span></span>
<span data-ttu-id="e6fa2-127">Hello Azure Table szolgáltatással használt toocommunicate típusok a WCF Data Services hello Windows Áruházbeli alkalmazás szalagtár függ.</span><span class="sxs-lookup"><span data-stu-id="e6fa2-127">Types that are used toocommunicate with hello Azure Table service depend on WCF Data Services for hello Windows Store app library.</span></span> <span data-ttu-id="e6fa2-128">Ezután adja hozzá egy hivatkozást toohello szükséges WCF tárak segítségével hello Csomagkezelő konzol:</span><span class="sxs-lookup"><span data-stu-id="e6fa2-128">Next, add a reference toohello required WCF libraries by using hello Package Manager Console:</span></span>

![tároló-alkalmazások-storage--Csomagkezelő][store-apps-storage-package-manager]

<span data-ttu-id="e6fa2-130">Parancs toopoint Package Manager toohello helyet a számítógépre következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="e6fa2-130">Use hello following command toopoint Package Manager toohello location on your machine:</span></span>

    Install-Package Microsoft.Data.OData.WindowsStore -Source "C:\Program Files (x86)\Microsoft WCF Data Services\5.0\bin\NuGet"

<span data-ttu-id="e6fa2-131">Ez a parancs automatikusan adja hozzá az összes szükséges hivatkozásokat tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="e6fa2-131">This command will automatically add all required references tooyour project.</span></span> <span data-ttu-id="e6fa2-132">Ha nem szeretné, hogy toouse hello Csomagkezelő konzol, hello WCF Data Services NuGet mappa hozzáadása a helyi számítógép toohello csomag olyan adatforrások listáját, és hozzáadhatja hello hivatkozás hello felhasználói felületén keresztül, a [kezelése NuGet csomagjainak használata hello párbeszédpanel](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog).</span><span class="sxs-lookup"><span data-stu-id="e6fa2-132">If you do not want toouse hello Package Manager Console, you can add hello WCF Data Services NuGet folder on your local machine toohello list of package sources and then add hello reference through hello UI, as described in [Managing NuGet Packages Using hello Dialog](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog).</span></span>

<span data-ttu-id="e6fa2-133">Ha a WCF Data Services NuGet-csomag hello rendelkezik hivatkozott, módosíthatja a gomb hello kódot **kattintson** esemény:</span><span class="sxs-lookup"><span data-stu-id="e6fa2-133">When you have referenced hello WCF Data Services NuGet package, change hello code in your button's **Click** event:</span></span>

```csharp
var credentials = new StorageCredentials(accountName, accountKey);
var account = new CloudStorageAccount(credentials, true);
var tableClient = account.CreateCloudTableClient();
var table = tableClient.GetTableReference("table1");
await table.CreateIfNotExistsAsync();
```

<span data-ttu-id="e6fa2-134">Ez a kód ellenőrzi, hogy nevű tábla *table1* a fiók létezik, és ezután a Ha nem hoz létre.</span><span class="sxs-lookup"><span data-stu-id="e6fa2-134">This code checks whether a table named *table1* exists in your account, and then creates it if not.</span></span>

<span data-ttu-id="e6fa2-135">Azt is megteheti egy hivatkozás tooMicrosoft.WindowsAzure.Storage.Table.dll a letöltött csomag azonos hello elérhető.</span><span class="sxs-lookup"><span data-stu-id="e6fa2-135">You can also add a reference tooMicrosoft.WindowsAzure.Storage.Table.dll, which is available in hello same package that you downloaded.</span></span> <span data-ttu-id="e6fa2-136">A könyvtárban további funkciókat, például a reflexió alapú szerializálási és általános lekérdezésekben.</span><span class="sxs-lookup"><span data-stu-id="e6fa2-136">This library contains additional functionality, such as reflection-based serialization and generic queries.</span></span> <span data-ttu-id="e6fa2-137">Vegye figyelembe, hogy ezt a szalagtárat támogatja a JavaScript használatát.</span><span class="sxs-lookup"><span data-stu-id="e6fa2-137">Note that this library does not support JavaScript.</span></span>

[store-apps-storage-vs-project]: ./media/storage-use-store-apps/store-apps-storage-vs-project.png
[store-apps-storage-choose-library]: ./media/storage-use-store-apps/store-apps-storage-choose-library.png
[store-apps-storage-package-manager]: ./media/storage-use-store-apps/store-apps-storage-package-manager.png
