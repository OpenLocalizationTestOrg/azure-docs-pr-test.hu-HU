---
title: "A queue storage (C++) használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan használható a queue storage szolgáltatás az Azure-ban. A c++ minták írja."
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
ms.openlocfilehash: 5e81d5e0af9871099b7f921f355cf94249e4d30c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-queue-storage-from-c"></a><span data-ttu-id="22792-104">A C++ Queue Storage használata</span><span class="sxs-lookup"><span data-stu-id="22792-104">How to use Queue Storage from C++</span></span>
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="22792-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="22792-105">Overview</span></span>
<span data-ttu-id="22792-106">Ez az útmutató bemutatja, hogyan hajthat végre a szolgáltatást az Azure Queue storage szolgáltatást használó általános forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="22792-106">This guide will show you how to perform common scenarios using the Azure Queue storage service.</span></span> <span data-ttu-id="22792-107">A minták írt C++ és használni a [Azure Storage ügyféloldali kódtára a C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="22792-107">The samples are written in C++ and use the [Azure Storage Client Library for C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span></span> <span data-ttu-id="22792-108">Az ismertetett forgatókönyvek **beszúrása**, **megtekintésekor**, **első**, és **törlése** üzenetek várólistára, valamint **létrehozása és törlése várólisták**.</span><span class="sxs-lookup"><span data-stu-id="22792-108">The scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating and deleting queues**.</span></span>

> [!NOTE]
> <span data-ttu-id="22792-109">Ez az útmutató az Azure Storage ügyféloldali kódtár célozza meg, a C++ 1.0.0 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="22792-109">This guide targets the Azure Storage Client Library for C++ version 1.0.0 and above.</span></span> <span data-ttu-id="22792-110">Az ajánlott verziója a Storage ügyféloldali kódtára 2.2.0, amelyik keresztül elérhető [NuGet](http://www.nuget.org/packages/wastorage) vagy [GitHub](http://github.com/Azure/azure-storage-cpp/).</span><span class="sxs-lookup"><span data-stu-id="22792-110">The recommended version is Storage Client Library 2.2.0, which is available via [NuGet](http://www.nuget.org/packages/wastorage) or [GitHub](http://github.com/Azure/azure-storage-cpp/).</span></span>
> 
> 

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a><span data-ttu-id="22792-111">A C++-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="22792-111">Create a C++ application</span></span>
<span data-ttu-id="22792-112">Ez az útmutató egy C++ alkalmazáson belül futtatható tárolási szolgáltatásokkal fog használni.</span><span class="sxs-lookup"><span data-stu-id="22792-112">In this guide, you will use storage features which can be run within a C++ application.</span></span>

<span data-ttu-id="22792-113">Ehhez az szükséges, akkor telepítse az Azure Storage ügyféloldali kódtára a C++ és az Azure storage-fiók létrehozása az Azure-előfizetése.</span><span class="sxs-lookup"><span data-stu-id="22792-113">To do so, you will need to install the Azure Storage Client Library for C++ and create an Azure storage account in your Azure subscription.</span></span>

<span data-ttu-id="22792-114">Telepítse az Azure Storage ügyféloldali kódtára a C++, a következő módszereket használhatja:</span><span class="sxs-lookup"><span data-stu-id="22792-114">To install the Azure Storage Client Library for C++, you can use the following methods:</span></span>

* <span data-ttu-id="22792-115">**Linux:** megadott kövesse a [Azure Storage ügyféloldali kódtára a C++ információs](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) lap.</span><span class="sxs-lookup"><span data-stu-id="22792-115">**Linux:** Follow the instructions given in the [Azure Storage Client Library for C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) page.</span></span>
* <span data-ttu-id="22792-116">**Windows:** a Visual Studióban kattintson **eszközök > NuGet-Csomagkezelő > Csomagkezelő konzol**.</span><span class="sxs-lookup"><span data-stu-id="22792-116">**Windows:** In Visual Studio, click **Tools > NuGet Package Manager > Package Manager Console**.</span></span> <span data-ttu-id="22792-117">Írja be a következő parancsot a [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) nyomja le az ENTER **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="22792-117">Type the following command into the [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) and press **ENTER**.</span></span>

```  
Install-Package wastorage
```

## <a name="configure-your-application-to-access-queue-storage"></a><span data-ttu-id="22792-118">Állítsa be az alkalmazását, Queue Storage eléréséhez</span><span class="sxs-lookup"><span data-stu-id="22792-118">Configure your application to access Queue Storage</span></span>
<span data-ttu-id="22792-119">Adja hozzá a következő tartalmaznak utasításokat a felső részén a C++-fájlt, amelyre az Azure storage API-k várólisták eléréséhez használható:</span><span class="sxs-lookup"><span data-stu-id="22792-119">Add the following include statements to the top of the C++ file where you want to use the Azure storage APIs to access queues:</span></span>  

```cpp
#include <was/storage_account.h>
#include <was/queue.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="22792-120">Egy Azure storage kapcsolati karakterlánc beállítása</span><span class="sxs-lookup"><span data-stu-id="22792-120">Set up an Azure storage connection string</span></span>
<span data-ttu-id="22792-121">Egy Azure storage-ügyfél egy tárolási kapcsolati karakterlánc végpontok és adatok szolgáltatások eléréséhez szükséges hitelesítő adatok tárolására használ.</span><span class="sxs-lookup"><span data-stu-id="22792-121">An Azure storage client uses a storage connection string to store endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="22792-122">Ha egy ügyfél-alkalmazás fut, meg kell adnia a tárolási kapcsolati karakterlánc a következő formátumban a tárfiók és a tárelérési kulcs nevét használja a tárfiók szerepel a [Azure Portal](https://portal.azure.com) a a *AccountName* és *AccountKey* értékeket.</span><span class="sxs-lookup"><span data-stu-id="22792-122">When running in a client application, you must provide the storage connection string in the following format, using the name of your storage account and the storage access key for the storage account listed in the [Azure Portal](https://portal.azure.com) for the *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="22792-123">A storage-fiókok és a hívóbetűk információkért lásd: [kapcsolatos Azure Storage-fiókok](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="22792-123">For information on storage accounts and access keys, see [About Azure Storage Accounts](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).</span></span> <span data-ttu-id="22792-124">Ez a példa bemutatja, hogyan deklarálhatnak ahhoz, hogy a kapcsolati karakterlánc statikus mezőben:</span><span class="sxs-lookup"><span data-stu-id="22792-124">This example shows how you can declare a static field to hold the connection string:</span></span>  

```cpp
// Define the connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

<span data-ttu-id="22792-125">Az alkalmazás tesztelése a helyi Windows-számítógép, használhatja a Microsoft Azure [storage emulator](../common/storage-use-emulator.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) együtt települ, amely a [Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="22792-125">To test your application in your local Windows computer, you can use the Microsoft Azure [storage emulator](../common/storage-use-emulator.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) that is installed with the [Azure SDK](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="22792-126">A storage emulator egy segédprogram, amely a helyi fejlesztési számítógépén elérhető az Azure Blob, Queue és Table szolgáltatások szimulálja.</span><span class="sxs-lookup"><span data-stu-id="22792-126">The storage emulator is a utility that simulates the Blob, Queue, and Table services available in Azure on your local development machine.</span></span> <span data-ttu-id="22792-127">A következő példa bemutatja, hogyan deklarálhatja, hogy tárolni tudja a kapcsolati karakterláncot a helyi storage emulator statikus mezőben:</span><span class="sxs-lookup"><span data-stu-id="22792-127">The following example shows how you can declare a static field to hold the connection string to your local storage emulator:</span></span>  

```cpp
// Define the connection-string with Azure Storage Emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

<span data-ttu-id="22792-128">Az Azure storage emulator elindításához válassza ki a **Start** gombra vagy nyomja le az **Windows** kulcs.</span><span class="sxs-lookup"><span data-stu-id="22792-128">To start the Azure storage emulator, select the **Start** button or press the **Windows** key.</span></span> <span data-ttu-id="22792-129">Írja be a szöveget **Azure Storage Emulator**, és válassza ki **Microsoft Azure Storage Emulator** az alkalmazások listájából.</span><span class="sxs-lookup"><span data-stu-id="22792-129">Begin typing **Azure Storage Emulator**, and select **Microsoft Azure Storage Emulator** from the list of applications.</span></span>

<span data-ttu-id="22792-130">A következő minták azt feltételezik, hogy használt két módszer közül egyik beolvasni a tárolási kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="22792-130">The following samples assume that you have used one of these two methods to get the storage connection string.</span></span>

## <a name="retrieve-your-connection-string"></a><span data-ttu-id="22792-131">A kapcsolat-karakterlánc beolvasása</span><span class="sxs-lookup"><span data-stu-id="22792-131">Retrieve your connection string</span></span>
<span data-ttu-id="22792-132">Használhatja a **cloud_storage_account** osztályt határoz meg a Tárfiók adatait.</span><span class="sxs-lookup"><span data-stu-id="22792-132">You can use the **cloud_storage_account** class to represent your Storage Account information.</span></span> <span data-ttu-id="22792-133">A tárfiók adatait a tárolási kapcsolati karakterlánc lekéréséhez használja a **elemezni** metódust.</span><span class="sxs-lookup"><span data-stu-id="22792-133">To retrieve your storage account information from the storage connection string, you can use the **parse** method.</span></span>

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

## <a name="how-to-create-a-queue"></a><span data-ttu-id="22792-134">Útmutató: a várólista létrehozása</span><span class="sxs-lookup"><span data-stu-id="22792-134">How to: Create a queue</span></span>
<span data-ttu-id="22792-135">A **cloud_queue_client** objektum lehetővé teszi a várólisták hivatkozási objektumok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="22792-135">A **cloud_queue_client** object lets you get reference objects for queues.</span></span> <span data-ttu-id="22792-136">Az alábbi kód létrehoz egy **cloud_queue_client** objektum.</span><span class="sxs-lookup"><span data-stu-id="22792-136">The following code creates a **cloud_queue_client** object.</span></span>

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create a queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();
```

<span data-ttu-id="22792-137">Használja a **cloud_queue_client** hivatkozás a használni kívánt várólista-objektum.</span><span class="sxs-lookup"><span data-stu-id="22792-137">Use the **cloud_queue_client** object to get a reference to the queue you want to use.</span></span> <span data-ttu-id="22792-138">A várólista hozhat létre, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="22792-138">You can create the queue if it doesn't exist.</span></span>

```cpp
// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Create the queue if it doesn't already exist.
 queue.create_if_not_exists();  
```

## <a name="how-to-insert-a-message-into-a-queue"></a><span data-ttu-id="22792-139">Útmutató: üzenet beszúrása egy várólistát</span><span class="sxs-lookup"><span data-stu-id="22792-139">How to: Insert a message into a queue</span></span>
<span data-ttu-id="22792-140">Üzenet beszúrása egy létező üzenetsorba, először létre kell hoznia egy új **cloud_queue_message**.</span><span class="sxs-lookup"><span data-stu-id="22792-140">To insert a message into an existing queue, first create a new **cloud_queue_message**.</span></span> <span data-ttu-id="22792-141">Ezután hívja a **add_message** metódust.</span><span class="sxs-lookup"><span data-stu-id="22792-141">Next, call the **add_message** method.</span></span> <span data-ttu-id="22792-142">A **cloud_queue_message** hozható létre vagy egy karakterláncot vagy egy **bájt** tömb.</span><span class="sxs-lookup"><span data-stu-id="22792-142">A **cloud_queue_message** can be created from either a string or a **byte** array.</span></span> <span data-ttu-id="22792-143">Az alábbi kód létrehoz egy üzenetsort (ha még nem létezik), és beszúrja a „Hello, World” üzenetet:</span><span class="sxs-lookup"><span data-stu-id="22792-143">Here is code which creates a queue (if it doesn't exist) and inserts the message 'Hello, World':</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Create the queue if it doesn't already exist.
queue.create_if_not_exists();

// Create a message and add it to the queue.
azure::storage::cloud_queue_message message1(U("Hello, World"));
queue.add_message(message1);  
```

## <a name="how-to-peek-at-the-next-message"></a><span data-ttu-id="22792-144">Útmutató: a következő üzenet megtekintése</span><span class="sxs-lookup"><span data-stu-id="22792-144">How to: Peek at the next message</span></span>
<span data-ttu-id="22792-145">Is bepillanthat, hogy egy sor elején található üzenetbe anélkül, hogy eltávolítaná az üzenetsorból meghívásával a **peek_message** metódust.</span><span class="sxs-lookup"><span data-stu-id="22792-145">You can peek at the message in the front of a queue without removing it from the queue by calling the **peek_message** method.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Peek at the next message.
azure::storage::cloud_queue_message peeked_message = queue.peek_message();

// Output the message content.
std::wcout << U("Peeked message content: ") << peeked_message.content_as_string() << std::endl;
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a><span data-ttu-id="22792-146">Útmutató: az üzenetsorban található üzenet tartalmának módosítása</span><span class="sxs-lookup"><span data-stu-id="22792-146">How to: Change the contents of a queued message</span></span>
<span data-ttu-id="22792-147">Egy üzenetet tartalmát helyben, az üzenetsorban módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="22792-147">You can change the contents of a message in-place in the queue.</span></span> <span data-ttu-id="22792-148">Ha az üzenet munkafeladatot jelöl, ezzel a funkcióval frissítheti a munkafeladat állapotát.</span><span class="sxs-lookup"><span data-stu-id="22792-148">If the message represents a work task, you could use this feature to update the status of the work task.</span></span> <span data-ttu-id="22792-149">Az alábbi kód frissíti az üzenetsorban található üzenetet az új tartalommal, és a láthatósági időkorlátot további 60 másodperccel bővíti.</span><span class="sxs-lookup"><span data-stu-id="22792-149">The following code updates the queue message with new contents, and sets the visibility timeout to extend another 60 seconds.</span></span> <span data-ttu-id="22792-150">Elmenti az üzenethez társított feladat állapotát, és az ügyfél számára további egy percet biztosít az üzenet használatának folytatására.</span><span class="sxs-lookup"><span data-stu-id="22792-150">This saves the state of work associated with the message, and gives the client another minute to continue working on the message.</span></span> <span data-ttu-id="22792-151">Ezzel a technikával többlépéses munkafolyamatokat is nyomon követhet az üzenetsor üzenetein anélkül, hogy újra kéne kezdenie, ha a folyamat valamelyik lépése hardver- vagy szoftverhiba miatt meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="22792-151">You could use this technique to track multi-step workflows on queue messages, without having to start over from the beginning if a processing step fails due to hardware or software failure.</span></span> <span data-ttu-id="22792-152">Általában tartja az újrapróbálkozások számát is, és az üzenet több mint n alkalommal megismétlése, akkor törlődik.</span><span class="sxs-lookup"><span data-stu-id="22792-152">Typically, you would keep a retry count as well, and if the message is retried more than n times, you would delete it.</span></span> <span data-ttu-id="22792-153">Ez védelmet biztosít az ellen, hogy egy üzenetet minden feldolgozásakor kiváltson egy alkalmazáshibát.</span><span class="sxs-lookup"><span data-stu-id="22792-153">This protects against a message that triggers an application error each time it is processed.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_conection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Get the message from the queue and update the message contents.
// The visibility timeout "0" means make it visible immediately.
// The visibility timeout "60" means the client can get another minute to continue
// working on the message.
azure::storage::cloud_queue_message changed_message = queue.get_message();

changed_message.set_content(U("Changed message"));
queue.update_message(changed_message, std::chrono::seconds(60), true);

// Output the message content.
std::wcout << U("Changed message content: ") << changed_message.content_as_string() << std::endl;  
```

## <a name="how-to-de-queue-the-next-message"></a><span data-ttu-id="22792-154">Útmutató: a következő üzenet kivétele az üzenetsorból</span><span class="sxs-lookup"><span data-stu-id="22792-154">How to: De-queue the next message</span></span>
<span data-ttu-id="22792-155">A kód két lépésben távolít el egy üzenetet az üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="22792-155">Your code de-queues a message from a queue in two steps.</span></span> <span data-ttu-id="22792-156">A hívás esetén **get_message**, a következő üzenetet kap a sorhoz.</span><span class="sxs-lookup"><span data-stu-id="22792-156">When you call **get_message**, you get the next message in a queue.</span></span> <span data-ttu-id="22792-157">Az üzenet **get_message** ebből a várólistából üzeneteket olvasó többi kód láthatatlanná válik.</span><span class="sxs-lookup"><span data-stu-id="22792-157">A message returned from **get_message** becomes invisible to any other code reading messages from this queue.</span></span> <span data-ttu-id="22792-158">Szeretné távolítani az üzenetet az üzenetsorból, meg kell is hívni **delete_message**.</span><span class="sxs-lookup"><span data-stu-id="22792-158">To finish removing the message from the queue, you must also call **delete_message**.</span></span> <span data-ttu-id="22792-159">Az üzenetek kétlépéses eltávolítása lehetővé teszi, hogy ha a kód hardver- vagy szoftverhiba miatt nem tud feldolgozni egy üzenetet, a kód egy másik példánya megkaphassa ugyanazt az üzenetet, és újra megpróbálkozhasson a feldolgozásával.</span><span class="sxs-lookup"><span data-stu-id="22792-159">This two-step process of removing a message assures that if your code fails to process a message due to hardware or software failure, another instance of your code can get the same message and try again.</span></span> <span data-ttu-id="22792-160">A kód hívások **delete_message** jobb gombbal az üzenet feldolgozása után.</span><span class="sxs-lookup"><span data-stu-id="22792-160">Your code calls **delete_message** right after the message has been processed.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Get the next message.
azure::storage::cloud_queue_message dequeued_message = queue.get_message();
std::wcout << U("Dequeued message: ") << dequeued_message.content_as_string() << std::endl;

// Delete the message.
queue.delete_message(dequeued_message);
```

## <a name="how-to-leverage-additional-options-for-de-queuing-messages"></a><span data-ttu-id="22792-161">Útmutató: az üzenetsorból üzenetek egyéb lehetőségek</span><span class="sxs-lookup"><span data-stu-id="22792-161">How to: Leverage additional options for de-queuing messages</span></span>
<span data-ttu-id="22792-162">Két módon szabhatja testre az üzenetek lekérését egy üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="22792-162">There are two ways you can customize message retrieval from a queue.</span></span> <span data-ttu-id="22792-163">Az első lehetőség az üzenetkötegek (legfeljebb 32) lekérése.</span><span class="sxs-lookup"><span data-stu-id="22792-163">First, you can get a batch of messages (up to 32).</span></span> <span data-ttu-id="22792-164">A második lehetőség az, hogy beállít egy hosszabb vagy rövidebb láthatatlansági időkorlátot, így a kódnak lehetősége van hosszabb vagy rövidebb idő alatt teljesen feldolgozni az egyes üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="22792-164">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time to fully process each message.</span></span> <span data-ttu-id="22792-165">Az alábbi példakód a **get_messages** metódus használatával kérje le a 20 üzenetet egy hívásban.</span><span class="sxs-lookup"><span data-stu-id="22792-165">The following code example uses the **get_messages** method to get 20 messages in one call.</span></span> <span data-ttu-id="22792-166">Ezután minden üzenetet használatával feldolgozza a **a** hurok.</span><span class="sxs-lookup"><span data-stu-id="22792-166">Then it processes each message using a **for** loop.</span></span> <span data-ttu-id="22792-167">Mindemellett a láthatatlansági időkorlátot minden üzenethez öt percre állítja be.</span><span class="sxs-lookup"><span data-stu-id="22792-167">It also sets the invisibility timeout to five minutes for each message.</span></span> <span data-ttu-id="22792-168">Vegye figyelembe, hogy a 5 perc minden üzenetet elindítja a egyszerre, így után 5 perccel átadott hívása **get_messages**, nem törölt üzenetek újra láthatóvá válnak.</span><span class="sxs-lookup"><span data-stu-id="22792-168">Note that the 5 minutes starts for all messages at the same time, so after 5 minutes have passed since the call to **get_messages**, any messages which have not been deleted will become visible again.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Dequeue some queue messages (maximum 32 at a time) and set their visibility timeout to
// 5 minutes (300 seconds).
azure::storage::queue_request_options options;
azure::storage::operation_context context;

// Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
std::vector<azure::storage::cloud_queue_message> messages = queue.get_messages(20, std::chrono::seconds(300), options, context);

for (auto it = messages.cbegin(); it != messages.cend(); ++it)
{
    // Display the contents of the message.
    std::wcout << U("Get: ") << it->content_as_string() << std::endl;
}
```

## <a name="how-to-get-the-queue-length"></a><span data-ttu-id="22792-169">Útmutató: az üzenetsor hosszának lekérése</span><span class="sxs-lookup"><span data-stu-id="22792-169">How to: Get the queue length</span></span>
<span data-ttu-id="22792-170">Megbecsülheti egy üzenetsorban található üzenetek számát.</span><span class="sxs-lookup"><span data-stu-id="22792-170">You can get an estimate of the number of messages in a queue.</span></span> <span data-ttu-id="22792-171">A **download_attributes** módszert kéri a Queue szolgáltatás olvashatók be a várólista attribútumai, az üzenetek száma.</span><span class="sxs-lookup"><span data-stu-id="22792-171">The **download_attributes** method asks the Queue service to retrieve the queue attributes, including the message count.</span></span> <span data-ttu-id="22792-172">A **approximate_message_count** metódus lekéri az üzenetek hozzávetőleges száma a várakozási sorban.</span><span class="sxs-lookup"><span data-stu-id="22792-172">The **approximate_message_count** method gets the approximate number of messages in the queue.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Fetch the queue attributes.
queue.download_attributes();

// Retrieve the cached approximate message count.
int cachedMessageCount = queue.approximate_message_count();

// Display number of messages.
std::wcout << U("Number of messages in queue: ") << cachedMessageCount << std::endl;  
```

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="22792-173">Útmutató: a várólista törlése</span><span class="sxs-lookup"><span data-stu-id="22792-173">How to: Delete a queue</span></span>
<span data-ttu-id="22792-174">Egy üzenetsor és a benne tárolt összes üzenet törléséhez hívja meg a **delete_queue_if_exists** a várólista-objektum metódust.</span><span class="sxs-lookup"><span data-stu-id="22792-174">To delete a queue and all the messages contained in it, call the **delete_queue_if_exists** method on the queue object.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// If the queue exists and delete it.
queue.delete_queue_if_exists();  
```

## <a name="next-steps"></a><span data-ttu-id="22792-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="22792-175">Next steps</span></span>
<span data-ttu-id="22792-176">Most, hogy megismerte a Queue storage alapjait, az alábbi hivatkozásokból tudhat meg többet az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="22792-176">Now that you've learned the basics of Queue storage, follow these links to learn more about Azure Storage.</span></span>

* [<span data-ttu-id="22792-177">A C++ Blob Storage használata</span><span class="sxs-lookup"><span data-stu-id="22792-177">How to use Blob Storage from C++</span></span>](../blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [<span data-ttu-id="22792-178">A C++ Table Storage használata</span><span class="sxs-lookup"><span data-stu-id="22792-178">How to use Table Storage from C++</span></span>](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [<span data-ttu-id="22792-179">A c++ Azure Storage-erőforrások listája</span><span class="sxs-lookup"><span data-stu-id="22792-179">List Azure Storage Resources in C++</span></span>](../common/storage-c-plus-plus-enumeration.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [<span data-ttu-id="22792-180">A Storage ügyféloldali kódtára a c++ nyelvhez – dokumentáció</span><span class="sxs-lookup"><span data-stu-id="22792-180">Storage Client Library for C++ Reference</span></span>](http://azure.github.io/azure-storage-cpp)
* [<span data-ttu-id="22792-181">Az Azure Storage dokumentációja</span><span class="sxs-lookup"><span data-stu-id="22792-181">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)