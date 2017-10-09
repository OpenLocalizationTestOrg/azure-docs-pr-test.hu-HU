---
title: aaaHow toouse a queue storage (C++) |} Microsoft Docs
description: "Ismerje meg, hogyan toouse hello queue storage szolgáltatás az Azure-ban. A c++ minták írja."
services: storage
documentationcenter: .net
author: cbrooksmsft
manager: jahogg
editor: tysonn
ms.assetid: c8a36365-29f6-404d-8fd1-858a7f33b50a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: cpp
ms.topic: article
ms.date: 05/11/2017
ms.author: cbrooksmsft
ms.openlocfilehash: 755380824890ad83774e14d258975915e10cfede
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-c"></a><span data-ttu-id="342d0-104">Hogyan toouse C++ a Queue Storage</span><span class="sxs-lookup"><span data-stu-id="342d0-104">How toouse Queue Storage from C++</span></span>
[!INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="342d0-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="342d0-105">Overview</span></span>
<span data-ttu-id="342d0-106">Ez az útmutató bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz hello Azure Queue storage szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="342d0-106">This guide will show you how tooperform common scenarios using hello Azure Queue storage service.</span></span> <span data-ttu-id="342d0-107">hello minták C++ nyelven íródtak, és használja a hello [Azure Storage ügyféloldali kódtára a C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="342d0-107">hello samples are written in C++ and use hello [Azure Storage Client Library for C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span></span> <span data-ttu-id="342d0-108">hello tárgyalt forgatókönyvekben szerepel a **beszúrása**, **megtekintésekor**, **első**, és **törlése** üzenetek, várólista, valamint  **létrehozása és törlése várólisták**.</span><span class="sxs-lookup"><span data-stu-id="342d0-108">hello scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating and deleting queues**.</span></span>

> [!NOTE]
> <span data-ttu-id="342d0-109">Ez az útmutató célok hello a Azure Storage ügyféloldali kódtára a C++ 1.0.0 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="342d0-109">This guide targets hello Azure Storage Client Library for C++ version 1.0.0 and above.</span></span> <span data-ttu-id="342d0-110">hello ajánlott verzió a Storage ügyféloldali kódtára 2.2.0, amelyik keresztül elérhető [NuGet](http://www.nuget.org/packages/wastorage) vagy [GitHub](http://github.com/Azure/azure-storage-cpp/).</span><span class="sxs-lookup"><span data-stu-id="342d0-110">hello recommended version is Storage Client Library 2.2.0, which is available via [NuGet](http://www.nuget.org/packages/wastorage) or [GitHub](http://github.com/Azure/azure-storage-cpp/).</span></span>
> 
> 

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a><span data-ttu-id="342d0-111">A C++-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="342d0-111">Create a C++ application</span></span>
<span data-ttu-id="342d0-112">Ez az útmutató egy C++ alkalmazáson belül futtatható tárolási szolgáltatásokkal fog használni.</span><span class="sxs-lookup"><span data-stu-id="342d0-112">In this guide, you will use storage features which can be run within a C++ application.</span></span>

<span data-ttu-id="342d0-113">toodo tooinstall kell tehát hello Azure Storage ügyféloldali kódtára a C++ és az Azure storage-fiók létrehozása az Azure-előfizetése.</span><span class="sxs-lookup"><span data-stu-id="342d0-113">toodo so, you will need tooinstall hello Azure Storage Client Library for C++ and create an Azure storage account in your Azure subscription.</span></span>

<span data-ttu-id="342d0-114">tooinstall hello Azure Storage ügyféloldali kódtára a C++, a következő módszerek hello használhatják:</span><span class="sxs-lookup"><span data-stu-id="342d0-114">tooinstall hello Azure Storage Client Library for C++, you can use hello following methods:</span></span>

* <span data-ttu-id="342d0-115">**Linux:** hello megadott hello utasítások [Azure Storage ügyféloldali kódtára a C++ információs](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) lap.</span><span class="sxs-lookup"><span data-stu-id="342d0-115">**Linux:** Follow hello instructions given in hello [Azure Storage Client Library for C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) page.</span></span>
* <span data-ttu-id="342d0-116">**Windows:** a Visual Studióban kattintson **eszközök > NuGet-Csomagkezelő > Csomagkezelő konzol**.</span><span class="sxs-lookup"><span data-stu-id="342d0-116">**Windows:** In Visual Studio, click **Tools > NuGet Package Manager > Package Manager Console**.</span></span> <span data-ttu-id="342d0-117">Típus hello következő parancsot a hello történő [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) nyomja le az ENTER **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="342d0-117">Type hello following command into hello [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) and press **ENTER**.</span></span>

```  
Install-Package wastorage
```

## <a name="configure-your-application-tooaccess-queue-storage"></a><span data-ttu-id="342d0-118">Az alkalmazás tooaccess Queue Storage konfigurálása</span><span class="sxs-lookup"><span data-stu-id="342d0-118">Configure your application tooaccess Queue Storage</span></span>
<span data-ttu-id="342d0-119">Adja hozzá, hello következő tartalmazó utasítások toohello felső részén hello C++ fájl toouse hello az Azure storage API-k tooaccess várólisták:</span><span class="sxs-lookup"><span data-stu-id="342d0-119">Add hello following include statements toohello top of hello C++ file where you want toouse hello Azure storage APIs tooaccess queues:</span></span>  

```cpp
#include <was/storage_account.h>
#include <was/queue.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="342d0-120">Egy Azure storage kapcsolati karakterlánc beállítása</span><span class="sxs-lookup"><span data-stu-id="342d0-120">Set up an Azure storage connection string</span></span>
<span data-ttu-id="342d0-121">Egy Azure storage-ügyfél egy tárolási kapcsolati karakterlánc toostore végpontok használ, és adatok felügyeleti szolgáltatások eléréséhez szükséges hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="342d0-121">An Azure storage client uses a storage connection string toostore endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="342d0-122">Ha egy ügyfél-alkalmazás fut, meg kell adnia hello tárolási kapcsolati karakterlánc formátuma a következő, a tárolási fiók és hello tárelérési kulcs hello nevének használatával hello felsorolt hello tárfiók hello [Azure Portal](https://portal.azure.com)a hello *AccountName* és *AccountKey* értékeket.</span><span class="sxs-lookup"><span data-stu-id="342d0-122">When running in a client application, you must provide hello storage connection string in hello following format, using hello name of your storage account and hello storage access key for hello storage account listed in hello [Azure Portal](https://portal.azure.com) for hello *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="342d0-123">A storage-fiókok és a hívóbetűk információkért lásd: [kapcsolatos Azure Storage-fiókok](storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="342d0-123">For information on storage accounts and access keys, see [About Azure Storage Accounts](storage-create-storage-account.md).</span></span> <span data-ttu-id="342d0-124">Ez a példa bemutatja, hogyan deklarálhatnak statikus mező toohold hello kapcsolati karakterláncot:</span><span class="sxs-lookup"><span data-stu-id="342d0-124">This example shows how you can declare a static field toohold hello connection string:</span></span>  

```cpp
// Define hello connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

<span data-ttu-id="342d0-125">tootest az alkalmazás a helyi számítógép Windows hello Microsoft Azure használhat [storage emulator](storage-use-emulator.md) hello a telepített [Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="342d0-125">tootest your application in your local Windows computer, you can use hello Microsoft Azure [storage emulator](storage-use-emulator.md) that is installed with hello [Azure SDK](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="342d0-126">hello storage emulator egy segédprogram, amely a hello Blob, Queue és Table szolgáltatások az Azure-ban található a helyi fejlesztési számítógépén szimulálja.</span><span class="sxs-lookup"><span data-stu-id="342d0-126">hello storage emulator is a utility that simulates hello Blob, Queue, and Table services available in Azure on your local development machine.</span></span> <span data-ttu-id="342d0-127">hello következő példa bemutatja, hogyan deklarálhatnak egy statikus mező toohold hello kapcsolati karakterlánc tooyour helyi storage emulatort:</span><span class="sxs-lookup"><span data-stu-id="342d0-127">hello following example shows how you can declare a static field toohold hello connection string tooyour local storage emulator:</span></span>  

```cpp
// Define hello connection-string with Azure Storage Emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

<span data-ttu-id="342d0-128">toostart hello Azure storage emulatort, jelölje be hello **Start** gombra vagy nyomja le az hello **Windows** kulcs.</span><span class="sxs-lookup"><span data-stu-id="342d0-128">toostart hello Azure storage emulator, select hello **Start** button or press hello **Windows** key.</span></span> <span data-ttu-id="342d0-129">Írja be a szöveget **Azure Storage Emulator**, és válassza ki **Microsoft Azure Storage Emulator** hello alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="342d0-129">Begin typing **Azure Storage Emulator**, and select **Microsoft Azure Storage Emulator** from hello list of applications.</span></span>

<span data-ttu-id="342d0-130">hello következő mintákat feltételezik használt egyik alábbi két módszer tooget hello tárolási kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="342d0-130">hello following samples assume that you have used one of these two methods tooget hello storage connection string.</span></span>

## <a name="retrieve-your-connection-string"></a><span data-ttu-id="342d0-131">A kapcsolat-karakterlánc beolvasása</span><span class="sxs-lookup"><span data-stu-id="342d0-131">Retrieve your connection string</span></span>
<span data-ttu-id="342d0-132">Használhatja a hello **cloud_storage_account** osztály toorepresent Tárfiók adatait.</span><span class="sxs-lookup"><span data-stu-id="342d0-132">You can use hello **cloud_storage_account** class toorepresent your Storage Account information.</span></span> <span data-ttu-id="342d0-133">tooretrieve a tárolási fiók hello tárolási kapcsolati karakterlánc adatait, használhatja a hello **elemezni** metódust.</span><span class="sxs-lookup"><span data-stu-id="342d0-133">tooretrieve your storage account information from hello storage connection string, you can use hello **parse** method.</span></span>

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

## <a name="how-to-create-a-queue"></a><span data-ttu-id="342d0-134">Útmutató: a várólista létrehozása</span><span class="sxs-lookup"><span data-stu-id="342d0-134">How to: Create a queue</span></span>
<span data-ttu-id="342d0-135">A **cloud_queue_client** objektum lehetővé teszi a várólisták hivatkozási objektumok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="342d0-135">A **cloud_queue_client** object lets you get reference objects for queues.</span></span> <span data-ttu-id="342d0-136">hello alábbi kód létrehoz egy **cloud_queue_client** objektum.</span><span class="sxs-lookup"><span data-stu-id="342d0-136">hello following code creates a **cloud_queue_client** object.</span></span>

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create a queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();
```

<span data-ttu-id="342d0-137">Használjon hello **cloud_queue_client** tooget toouse kívánt referencia toohello várólista-objektum.</span><span class="sxs-lookup"><span data-stu-id="342d0-137">Use hello **cloud_queue_client** object tooget a reference toohello queue you want toouse.</span></span> <span data-ttu-id="342d0-138">Hello várólista hozhat létre, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="342d0-138">You can create hello queue if it doesn't exist.</span></span>

```cpp
// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Create hello queue if it doesn't already exist.
 queue.create_if_not_exists();  
```

## <a name="how-to-insert-a-message-into-a-queue"></a><span data-ttu-id="342d0-139">Útmutató: üzenet beszúrása egy várólistát</span><span class="sxs-lookup"><span data-stu-id="342d0-139">How to: Insert a message into a queue</span></span>
<span data-ttu-id="342d0-140">tooinsert egy létező üzenetsorba, az üzenet először létre kell hoznia egy új **cloud_queue_message**.</span><span class="sxs-lookup"><span data-stu-id="342d0-140">tooinsert a message into an existing queue, first create a new **cloud_queue_message**.</span></span> <span data-ttu-id="342d0-141">Ezután hívja hello **add_message** metódust.</span><span class="sxs-lookup"><span data-stu-id="342d0-141">Next, call hello **add_message** method.</span></span> <span data-ttu-id="342d0-142">A **cloud_queue_message** hozható létre vagy egy karakterláncot vagy egy **bájt** tömb.</span><span class="sxs-lookup"><span data-stu-id="342d0-142">A **cloud_queue_message** can be created from either a string or a **byte** array.</span></span> <span data-ttu-id="342d0-143">Íme, amely létrehoz egy üzenetsort (Ha még nem létezik) és a Beszúrás üdvözlőüzenetére "Hello, World" code:</span><span class="sxs-lookup"><span data-stu-id="342d0-143">Here is code which creates a queue (if it doesn't exist) and inserts hello message 'Hello, World':</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Create hello queue if it doesn't already exist.
queue.create_if_not_exists();

// Create a message and add it toohello queue.
azure::storage::cloud_queue_message message1(U("Hello, World"));
queue.add_message(message1);  
```

## <a name="how-to-peek-at-hello-next-message"></a><span data-ttu-id="342d0-144">Hogyan: betekintés a következő köszönőüzenetei</span><span class="sxs-lookup"><span data-stu-id="342d0-144">How to: Peek at hello next message</span></span>
<span data-ttu-id="342d0-145">Eltávolítása hello által hívó hello anélkül is bepillanthat hello betekintés a várólista elejére hello **peek_message** metódust.</span><span class="sxs-lookup"><span data-stu-id="342d0-145">You can peek at hello message in hello front of a queue without removing it from hello queue by calling hello **peek_message** method.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Peek at hello next message.
azure::storage::cloud_queue_message peeked_message = queue.peek_message();

// Output hello message content.
std::wcout << U("Peeked message content: ") << peeked_message.content_as_string() << std::endl;
```

## <a name="how-to-change-hello-contents-of-a-queued-message"></a><span data-ttu-id="342d0-146">Hogyan: hello aszinkron üzenet tartalmának módosítása</span><span class="sxs-lookup"><span data-stu-id="342d0-146">How to: Change hello contents of a queued message</span></span>
<span data-ttu-id="342d0-147">Módosíthatja egy üzenet helyben hello várólista hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="342d0-147">You can change hello contents of a message in-place in hello queue.</span></span> <span data-ttu-id="342d0-148">Üdvözlőüzenetére jelöl, használhatja a munkafeladat hello szolgáltatás tooupdate hello állapotának.</span><span class="sxs-lookup"><span data-stu-id="342d0-148">If hello message represents a work task, you could use this feature tooupdate hello status of hello work task.</span></span> <span data-ttu-id="342d0-149">a következő kód hello hello üzenetsor frissíti az új tartalommal, és a készletek hello látható időtúllépés tooextend további 60 másodperccel.</span><span class="sxs-lookup"><span data-stu-id="342d0-149">hello following code updates hello queue message with new contents, and sets hello visibility timeout tooextend another 60 seconds.</span></span> <span data-ttu-id="342d0-150">Hello üdvözlőüzenetére társított feladat állapotát menti, és lehetőséget ad a hello ügyfél üdvözlőüzenetére dolgozik egy másik perces toocontinue.</span><span class="sxs-lookup"><span data-stu-id="342d0-150">This saves hello state of work associated with hello message, and gives hello client another minute toocontinue working on hello message.</span></span> <span data-ttu-id="342d0-151">Ez a módszer tootrack többlépéses munkafolyamatokat üzenetsor üzenetein anélkül, hogy toostart keresztül hello kezdődően, ha a folyamat valamelyik lépése toohardware-vagy szoftverhiba miatt nem használható.</span><span class="sxs-lookup"><span data-stu-id="342d0-151">You could use this technique tootrack multi-step workflows on queue messages, without having toostart over from hello beginning if a processing step fails due toohardware or software failure.</span></span> <span data-ttu-id="342d0-152">Általában tartja az újrapróbálkozások számát is, és hello üzenet több mint n alkalommal megismétlése, akkor törlődik.</span><span class="sxs-lookup"><span data-stu-id="342d0-152">Typically, you would keep a retry count as well, and if hello message is retried more than n times, you would delete it.</span></span> <span data-ttu-id="342d0-153">Ez védelmet biztosít az ellen, hogy egy üzenetet minden feldolgozásakor kiváltson egy alkalmazáshibát.</span><span class="sxs-lookup"><span data-stu-id="342d0-153">This protects against a message that triggers an application error each time it is processed.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_conection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Get hello message from hello queue and update hello message contents.
// hello visibility timeout "0" means make it visible immediately.
// hello visibility timeout "60" means hello client can get another minute toocontinue
// working on hello message.
azure::storage::cloud_queue_message changed_message = queue.get_message();

changed_message.set_content(U("Changed message"));
queue.update_message(changed_message, std::chrono::seconds(60), true);

// Output hello message content.
std::wcout << U("Changed message content: ") << changed_message.content_as_string() << std::endl;  
```

## <a name="how-to-de-queue-hello-next-message"></a><span data-ttu-id="342d0-154">Hogyan: hello következő üzenet kivétele az üzenetsorból</span><span class="sxs-lookup"><span data-stu-id="342d0-154">How to: De-queue hello next message</span></span>
<span data-ttu-id="342d0-155">A kód két lépésben távolít el egy üzenetet az üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="342d0-155">Your code de-queues a message from a queue in two steps.</span></span> <span data-ttu-id="342d0-156">A hívás esetén **get_message**, a sorhoz hello a következő üzenetet kapott.</span><span class="sxs-lookup"><span data-stu-id="342d0-156">When you call **get_message**, you get hello next message in a queue.</span></span> <span data-ttu-id="342d0-157">Az üzenet **get_message** láthatatlan tooany ebből a várólistából üzeneteket olvasó többi kód válik.</span><span class="sxs-lookup"><span data-stu-id="342d0-157">A message returned from **get_message** becomes invisible tooany other code reading messages from this queue.</span></span> <span data-ttu-id="342d0-158">toofinish eltávolításakor üdvözlőüzenetére hello üzenetsorból, meg kell is hívni **delete_message**.</span><span class="sxs-lookup"><span data-stu-id="342d0-158">toofinish removing hello message from hello queue, you must also call **delete_message**.</span></span> <span data-ttu-id="342d0-159">A kétlépéses folyamat eltávolításával előállított üzenet biztosítja, hogy a kód meghibásodásakor tooprocess miatt toohardware vagy szoftver-hiba, a kód egy másik példánya üzenet kérheti le hello ugyanazt az üzenetet, és próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="342d0-159">This two-step process of removing a message assures that if your code fails tooprocess a message due toohardware or software failure, another instance of your code can get hello same message and try again.</span></span> <span data-ttu-id="342d0-160">A kód hívások **delete_message** jobb gombbal az üdvözlő üzenet feldolgozása után.</span><span class="sxs-lookup"><span data-stu-id="342d0-160">Your code calls **delete_message** right after hello message has been processed.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Get hello next message.
azure::storage::cloud_queue_message dequeued_message = queue.get_message();
std::wcout << U("Dequeued message: ") << dequeued_message.content_as_string() << std::endl;

// Delete hello message.
queue.delete_message(dequeued_message);
```

## <a name="how-to-leverage-additional-options-for-de-queuing-messages"></a><span data-ttu-id="342d0-161">Útmutató: az üzenetsorból üzenetek egyéb lehetőségek</span><span class="sxs-lookup"><span data-stu-id="342d0-161">How to: Leverage additional options for de-queuing messages</span></span>
<span data-ttu-id="342d0-162">Két módon szabhatja testre az üzenetek lekérését egy üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="342d0-162">There are two ways you can customize message retrieval from a queue.</span></span> <span data-ttu-id="342d0-163">Először is kaphat az üzenetkötegek (felfelé too32).</span><span class="sxs-lookup"><span data-stu-id="342d0-163">First, you can get a batch of messages (up too32).</span></span> <span data-ttu-id="342d0-164">Második beállíthat egy hosszabb vagy rövidebb láthatatlansági időkorlátot, így a kódnak több vagy kevesebb idő toofully feldolgozni az egyes üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="342d0-164">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time toofully process each message.</span></span> <span data-ttu-id="342d0-165">hello alábbi példakód hello **get_messages** metódus tooget 20 üzenetek egy hívásban.</span><span class="sxs-lookup"><span data-stu-id="342d0-165">hello following code example uses hello **get_messages** method tooget 20 messages in one call.</span></span> <span data-ttu-id="342d0-166">Ezután minden üzenetet használatával feldolgozza a **a** hurok.</span><span class="sxs-lookup"><span data-stu-id="342d0-166">Then it processes each message using a **for** loop.</span></span> <span data-ttu-id="342d0-167">Beállítja a hello láthatatlansági időtúllépés toofive percig, amíg minden üzenetet is.</span><span class="sxs-lookup"><span data-stu-id="342d0-167">It also sets hello invisibility timeout toofive minutes for each message.</span></span> <span data-ttu-id="342d0-168">Vegye figyelembe, hogy hello 5 perc indítása: hello összes üzenet azonos idő, így miután 5 perc telt túl hello hívás**get_messages**, nem törölt üzenetek újra láthatóvá válnak.</span><span class="sxs-lookup"><span data-stu-id="342d0-168">Note that hello 5 minutes starts for all messages at hello same time, so after 5 minutes have passed since hello call too**get_messages**, any messages which have not been deleted will become visible again.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Dequeue some queue messages (maximum 32 at a time) and set their visibility timeout to
// 5 minutes (300 seconds).
azure::storage::queue_request_options options;
azure::storage::operation_context context;

// Retrieve 20 messages from hello queue with a visibility timeout of 300 seconds.
std::vector<azure::storage::cloud_queue_message> messages = queue.get_messages(20, std::chrono::seconds(300), options, context);

for (auto it = messages.cbegin(); it != messages.cend(); ++it)
{
    // Display hello contents of hello message.
    std::wcout << U("Get: ") << it->content_as_string() << std::endl;
}
```

## <a name="how-to-get-hello-queue-length"></a><span data-ttu-id="342d0-169">Hogyan: hello várólista hosszának lekérése</span><span class="sxs-lookup"><span data-stu-id="342d0-169">How to: Get hello queue length</span></span>
<span data-ttu-id="342d0-170">A várólistában lévő üzenetek hello számának becslése kérheti le.</span><span class="sxs-lookup"><span data-stu-id="342d0-170">You can get an estimate of hello number of messages in a queue.</span></span> <span data-ttu-id="342d0-171">Hello **download_attributes** módszert kéri hello várólista szolgáltatás tooretrieve hello várólista attribútumok, beleértve a hello üzenetek száma.</span><span class="sxs-lookup"><span data-stu-id="342d0-171">hello **download_attributes** method asks hello Queue service tooretrieve hello queue attributes, including hello message count.</span></span> <span data-ttu-id="342d0-172">Hello **approximate_message_count** metódus lekéri az üzenetek hello hozzávetőleges száma hello várólistában.</span><span class="sxs-lookup"><span data-stu-id="342d0-172">hello **approximate_message_count** method gets hello approximate number of messages in hello queue.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Fetch hello queue attributes.
queue.download_attributes();

// Retrieve hello cached approximate message count.
int cachedMessageCount = queue.approximate_message_count();

// Display number of messages.
std::wcout << U("Number of messages in queue: ") << cachedMessageCount << std::endl;  
```

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="342d0-173">Útmutató: a várólista törlése</span><span class="sxs-lookup"><span data-stu-id="342d0-173">How to: Delete a queue</span></span>
<span data-ttu-id="342d0-174">egy üzenetsor és az összes köszönőüzenetei benne tárolt, hívás hello toodelete **delete_queue_if_exists** hello várólista-objektum metódust.</span><span class="sxs-lookup"><span data-stu-id="342d0-174">toodelete a queue and all hello messages contained in it, call hello **delete_queue_if_exists** method on hello queue object.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// If hello queue exists and delete it.
queue.delete_queue_if_exists();  
```

## <a name="next-steps"></a><span data-ttu-id="342d0-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="342d0-175">Next steps</span></span>
<span data-ttu-id="342d0-176">Most, hogy megismerte a Queue storage alapjait hello, kövesse az alábbi hivatkozások toolearn Azure Storage-ról további.</span><span class="sxs-lookup"><span data-stu-id="342d0-176">Now that you've learned hello basics of Queue storage, follow these links toolearn more about Azure Storage.</span></span>

* [<span data-ttu-id="342d0-177">Hogyan toouse Blob Storage-ának C++</span><span class="sxs-lookup"><span data-stu-id="342d0-177">How toouse Blob Storage from C++</span></span>](storage-c-plus-plus-how-to-use-blobs.md)
* [<span data-ttu-id="342d0-178">Hogyan toouse Table Storage-ának C++</span><span class="sxs-lookup"><span data-stu-id="342d0-178">How toouse Table Storage from C++</span></span>](storage-c-plus-plus-how-to-use-tables.md)
* [<span data-ttu-id="342d0-179">A c++ Azure Storage-erőforrások listája</span><span class="sxs-lookup"><span data-stu-id="342d0-179">List Azure Storage Resources in C++</span></span>](storage-c-plus-plus-enumeration.md)
* [<span data-ttu-id="342d0-180">A Storage ügyféloldali kódtára a c++ nyelvhez – dokumentáció</span><span class="sxs-lookup"><span data-stu-id="342d0-180">Storage Client Library for C++ Reference</span></span>](http://azure.github.io/azure-storage-cpp)
* [<span data-ttu-id="342d0-181">Az Azure Storage dokumentációja</span><span class="sxs-lookup"><span data-stu-id="342d0-181">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)