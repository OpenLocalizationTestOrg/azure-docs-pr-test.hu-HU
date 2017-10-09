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
# <a name="how-toouse-azure-storage-in-windows-store-apps"></a>Hogyan toouse Azure Storage a Windows Áruházbeli alkalmazások
## <a name="overview"></a>Áttekintés
Ez az útmutató bemutatja, hogyan tooget használatába, amely lehetővé teszi az Azure Storage használata Windows Áruházbeli alkalmazást fejleszt.

## <a name="download-required-tools"></a>Töltse le a szükséges eszközök
* [A Visual Studio](https://www.visualstudio.com/downloads/) teszi, hogy könnyen toobuild debug localize, a csomagot, majd a Windows Áruházbeli alkalmazások telepítése. A Visual Studio 2012 vagy újabb rendszer szükséges.
* Hello [Azure Storage ügyféloldali kódtár](https://www.nuget.org/packages/WindowsAzure.Storage) egy Windows-Futtatókörnyezetű osztálytár biztosít az Azure Storage.
* [A WCF Data Services eszközök Windows Áruházbeli alkalmazásokkal való](http://www.microsoft.com/download/details.aspx?id=30714) hello szolgáltatás hivatkozás hozzáadása élmény a Windows Áruházbeli alkalmazások a Visual Studio ügyféloldali OData támogató bővíti.

## <a name="develop-apps"></a>Alkalmazásfejlesztés
### <a name="getting-ready"></a>Előkészületek
Hozzon létre egy új Windows Áruházbeli alkalmazás projektjére a Visual Studio 2012 vagy újabb verzió:

![tároló-alkalmazások-storage-Visual Studio-projekt][store-apps-storage-vs-project]

Ezután adja hozzá egy hivatkozást toohello Azure Storage ügyféloldali kódtár kattintson a jobb gombbal **hivatkozások**gombra, majd **hivatkozás hozzáadása**, és keresse meg a tároló ügyfél Library a Windows-Futtatókörnyezetű toohello során le:

![tároló-alkalmazások-storage-válasszon-kódtár][store-apps-storage-choose-library]

### <a name="using-hello-library-with-hello-blob-and-queue-services"></a>Hello függvénytár hello Blob és várólista-szolgáltatás használatával
Ezen a ponton a az alkalmazás készen áll a toocall hello Azure Blob és a Queue szolgáltatások. Adja hozzá a következő hello **használatával** utasításokat, hogy az Azure Storage típusok közvetlenül lehet hivatkozni:

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
```

Ezután adja hozzá a gomb tooyour lapja. Adja hozzá a következő kód tooits hello **kattintson** esemény, és módosítsa a eseménykezelő metódus hello segítségével [aszinkron kulcsszó](http://msdn.microsoft.com/library/vstudio/hh156513.aspx):

```csharp
var credentials = new StorageCredentials(accountName, accountKey);
var account = new CloudStorageAccount(credentials, true);
var blobClient = account.CreateCloudBlobClient();
var container = blobClient.GetContainerReference("container1");
await container.CreateIfNotExistsAsync();
```

Ez a kód feltételezi, hogy rendelkezik-e két karakterlánc típusú változót, *accountName* és *accountKey*. A tárolási fiók és hello fiókkulcs azzal a fiókkal társított hello neve jelölnek.

Hozza létre és hello alkalmazás futtatásához. Gombra kattintva hello gomb ellenőrzi, hogy a tároló nevű *container1* a fiók létezik, és majd hozza létre, ha nem.

### <a name="using-hello-library-with-hello-table-service"></a>A Table szolgáltatás hello hello szalagtár használata
Hello Azure Table szolgáltatással használt toocommunicate típusok a WCF Data Services hello Windows Áruházbeli alkalmazás szalagtár függ. Ezután adja hozzá egy hivatkozást toohello szükséges WCF tárak segítségével hello Csomagkezelő konzol:

![tároló-alkalmazások-storage--Csomagkezelő][store-apps-storage-package-manager]

Parancs toopoint Package Manager toohello helyet a számítógépre következő hello használata:

    Install-Package Microsoft.Data.OData.WindowsStore -Source "C:\Program Files (x86)\Microsoft WCF Data Services\5.0\bin\NuGet"

Ez a parancs automatikusan adja hozzá az összes szükséges hivatkozásokat tooyour projekt. Ha nem szeretné, hogy toouse hello Csomagkezelő konzol, hello WCF Data Services NuGet mappa hozzáadása a helyi számítógép toohello csomag olyan adatforrások listáját, és hozzáadhatja hello hivatkozás hello felhasználói felületén keresztül, a [kezelése NuGet csomagjainak használata hello párbeszédpanel](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog).

Ha a WCF Data Services NuGet-csomag hello rendelkezik hivatkozott, módosíthatja a gomb hello kódot **kattintson** esemény:

```csharp
var credentials = new StorageCredentials(accountName, accountKey);
var account = new CloudStorageAccount(credentials, true);
var tableClient = account.CreateCloudTableClient();
var table = tableClient.GetTableReference("table1");
await table.CreateIfNotExistsAsync();
```

Ez a kód ellenőrzi, hogy nevű tábla *table1* a fiók létezik, és ezután a Ha nem hoz létre.

Azt is megteheti egy hivatkozás tooMicrosoft.WindowsAzure.Storage.Table.dll a letöltött csomag azonos hello elérhető. A könyvtárban további funkciókat, például a reflexió alapú szerializálási és általános lekérdezésekben. Vegye figyelembe, hogy ezt a szalagtárat támogatja a JavaScript használatát.

[store-apps-storage-vs-project]: ./media/storage-use-store-apps/store-apps-storage-vs-project.png
[store-apps-storage-choose-library]: ./media/storage-use-store-apps/store-apps-storage-choose-library.png
[store-apps-storage-package-manager]: ./media/storage-use-store-apps/store-apps-storage-package-manager.png
