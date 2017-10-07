---
title: a Queue storage a Ruby aaaHow toouse |} Microsoft Docs
description: "Ismerje meg, hogyan toouse hello Azure Queue szolgáltatás toocreate és a delete várólisták és a Beszúrás, get, és törli az üzenetet. Ruby írt minták."
services: storage
documentationcenter: ruby
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 59c2d81b-db9c-46ee-ade2-2f0caae6b1e6
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: c8eacac058442419cb9e8fe62cb69ad7ef1e2fc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-ruby"></a><span data-ttu-id="19d0b-104">Hogyan toouse a Queue storage a Rubyhoz</span><span class="sxs-lookup"><span data-stu-id="19d0b-104">How toouse Queue storage from Ruby</span></span>
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="19d0b-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="19d0b-105">Overview</span></span>
<span data-ttu-id="19d0b-106">Ez az útmutató bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz hello Microsoft Azure Queue Storage szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="19d0b-106">This guide shows you how tooperform common scenarios using hello Microsoft Azure Queue Storage service.</span></span> <span data-ttu-id="19d0b-107">hello minták hello Ruby Azure API segítségével készül.</span><span class="sxs-lookup"><span data-stu-id="19d0b-107">hello samples are written using hello Ruby Azure API.</span></span>
<span data-ttu-id="19d0b-108">hello tárgyalt forgatókönyvekben szerepel a **beszúrása**, **megtekintésekor**, **első**, és **törlése** üzenetek, várólista, valamint  **létrehozása és törlése várólisták**.</span><span class="sxs-lookup"><span data-stu-id="19d0b-108">hello scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating and deleting queues**.</span></span>

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a><span data-ttu-id="19d0b-109">Ruby-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="19d0b-109">Create a Ruby Application</span></span>
<span data-ttu-id="19d0b-110">Ruby-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="19d0b-110">Create a Ruby application.</span></span> <span data-ttu-id="19d0b-111">Útmutatásért lásd: [Ruby sínek webalkalmazás egy Azure virtuális gépen](../../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="19d0b-111">For instructions, see [Ruby on Rails Web application on an Azure VM](../../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span></span>

## <a name="configure-your-application-tooaccess-storage"></a><span data-ttu-id="19d0b-112">Alkalmazás tooAccess tároló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="19d0b-112">Configure Your Application tooAccess Storage</span></span>
<span data-ttu-id="19d0b-113">az Azure storage toouse, és van szükség toodownload használata hello Ruby azure csomagot, amely kényelmi szalagtárak hello tárolószolgáltatások REST kommunikációt készletét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="19d0b-113">toouse Azure storage, you need toodownload and use hello Ruby azure package, which includes a set of convenience libraries that communicate with hello storage REST services.</span></span>

### <a name="use-rubygems-tooobtain-hello-package"></a><span data-ttu-id="19d0b-114">RubyGems tooobtain hello csomag használata</span><span class="sxs-lookup"><span data-stu-id="19d0b-114">Use RubyGems tooobtain hello package</span></span>
1. <span data-ttu-id="19d0b-115">Használjon például egy parancssori felületet **PowerShell** (Windows), **Terminálszolgáltatások** (Mac), vagy **Bash** (Unix).</span><span class="sxs-lookup"><span data-stu-id="19d0b-115">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix).</span></span>
2. <span data-ttu-id="19d0b-116">Írja be a "gem telepítése azure" hello parancs ablak tooinstall hello gem és függőségeit.</span><span class="sxs-lookup"><span data-stu-id="19d0b-116">Type "gem install azure" in hello command window tooinstall hello gem and dependencies.</span></span>

### <a name="import-hello-package"></a><span data-ttu-id="19d0b-117">Hello csomag importálása</span><span class="sxs-lookup"><span data-stu-id="19d0b-117">Import hello package</span></span>
<span data-ttu-id="19d0b-118">Kedvenc szövegszerkesztőjével használatához adja hozzá a következő hello toouse tárolási célhelyeként Ruby fájl elejéhez toohello hello:</span><span class="sxs-lookup"><span data-stu-id="19d0b-118">Use your favorite text editor, add hello following toohello top of hello Ruby file where you intend toouse storage:</span></span>

```ruby
require "azure"
```

## <a name="setup-an-azure-storage-connection"></a><span data-ttu-id="19d0b-119">Az Azure Storage-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="19d0b-119">Setup an Azure Storage Connection</span></span>
<span data-ttu-id="19d0b-120">hello azure modul hello környezeti változókat olvassák **AZURE\_tárolási\_fiók** és **AZURE\_tárolási\_ACCESS_KEY** a információ tooconnect tooyour Azure storage-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="19d0b-120">hello azure module will read hello environment variables **AZURE\_STORAGE\_ACCOUNT** and **AZURE\_STORAGE\_ACCESS_KEY** for information required tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="19d0b-121">Ha ezek a környezeti változók nem, meg kell adnia hello fiók használata előtt **Azure::QueueService** a hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="19d0b-121">If these environment variables are not set, you must specify hello account information before using **Azure::QueueService** with hello following code:</span></span>

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your Azure storage access key>"
```

<span data-ttu-id="19d0b-122">tooobtain ezeket az értékeket a klasszikus vagy erőforrás-kezelő tárolási fiók hello Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="19d0b-122">tooobtain these values from a classic or Resource Manager storage account in hello Azure portal:</span></span>

1. <span data-ttu-id="19d0b-123">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="19d0b-123">Log in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="19d0b-124">Keresse meg a kívánt toouse toohello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="19d0b-124">Navigate toohello storage account you want toouse.</span></span>
3. <span data-ttu-id="19d0b-125">Kattintson a jobb oldali hello hello beállítási paneljén **hívóbetűk**.</span><span class="sxs-lookup"><span data-stu-id="19d0b-125">In hello Settings blade on hello right, click **Access Keys**.</span></span>
4. <span data-ttu-id="19d0b-126">Hello elérési kulcsok paneljén megjelenő láthatja, hello hozzáférési kulcs 1 és 2 elérési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="19d0b-126">In hello Access keys blade that appears, you'll see hello access key 1 and access key 2.</span></span> <span data-ttu-id="19d0b-127">Ezek bármelyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="19d0b-127">You can use either of these.</span></span> 
5. <span data-ttu-id="19d0b-128">Kattintson a hello másolás ikon toocopy hello kulcs toohello vágólapra.</span><span class="sxs-lookup"><span data-stu-id="19d0b-128">Click hello copy icon toocopy hello key toohello clipboard.</span></span> 

## <a name="how-to-create-a-queue"></a><span data-ttu-id="19d0b-129">Útmutató: A várólista létrehozása</span><span class="sxs-lookup"><span data-stu-id="19d0b-129">How To: Create a Queue</span></span>
<span data-ttu-id="19d0b-130">hello alábbi kód létrehoz egy **Azure::QueueService** objektum, amely lehetővé teszi az üzenetsorok toowork.</span><span class="sxs-lookup"><span data-stu-id="19d0b-130">hello following code creates a **Azure::QueueService** object, which enables you toowork with queues.</span></span>

```ruby
azure_queue_service = Azure::QueueService.new
```

<span data-ttu-id="19d0b-131">Használjon hello **create_queue()** metódus toocreate hello rendelkező várólista adta meg.</span><span class="sxs-lookup"><span data-stu-id="19d0b-131">Use hello **create_queue()** method toocreate a queue with hello specified name.</span></span>

```ruby
begin
  azure_queue_service.create_queue("test-queue")
rescue
  puts $!
end
```

## <a name="how-to-insert-a-message-into-a-queue"></a><span data-ttu-id="19d0b-132">Útmutató: A várólista üzenet beszúrása</span><span class="sxs-lookup"><span data-stu-id="19d0b-132">How To: Insert a Message into a Queue</span></span>
<span data-ttu-id="19d0b-133">a várólistán, használjon hello üzenet tooinsert **create_message()** metódus toocreate egy új üzenetet, és adja hozzá toohello várólista.</span><span class="sxs-lookup"><span data-stu-id="19d0b-133">tooinsert a message into a queue, use hello **create_message()** method toocreate a new message and add it toohello queue.</span></span>

```ruby
azure_queue_service.create_message("test-queue", "test message")
```

## <a name="how-to-peek-at-hello-next-message"></a><span data-ttu-id="19d0b-134">Útmutató: A következő üzenetet hello betekintés</span><span class="sxs-lookup"><span data-stu-id="19d0b-134">How To: Peek at hello Next Message</span></span>
<span data-ttu-id="19d0b-135">Eltávolítása hello által hívó hello anélkül is bepillanthat hello betekintés a várólista elejére hello **betekintés\_messages()** metódust.</span><span class="sxs-lookup"><span data-stu-id="19d0b-135">You can peek at hello message in hello front of a queue without removing it from hello queue by calling hello **peek\_messages()** method.</span></span> <span data-ttu-id="19d0b-136">Alapértelmezés szerint **betekintés\_messages()** lekéri egyetlen üzenetben.</span><span class="sxs-lookup"><span data-stu-id="19d0b-136">By default, **peek\_messages()** peeks at a single message.</span></span> <span data-ttu-id="19d0b-137">Azt is megadhatja, hogy hány üzenetek toopeek szeretné.</span><span class="sxs-lookup"><span data-stu-id="19d0b-137">You can also specify how many messages you want toopeek.</span></span>

```ruby
result = azure_queue_service.peek_messages("test-queue",
  {:number_of_messages => 10})
```

## <a name="how-to-dequeue-hello-next-message"></a><span data-ttu-id="19d0b-138">Útmutató: A következő üzenetet hello created</span><span class="sxs-lookup"><span data-stu-id="19d0b-138">How To: Dequeue hello Next Message</span></span>
<span data-ttu-id="19d0b-139">Eltávolíthatja a üzenetet az üzenetsorból két lépésben.</span><span class="sxs-lookup"><span data-stu-id="19d0b-139">You can remove a message from a queue in two steps.</span></span>

1. <span data-ttu-id="19d0b-140">A hívás esetén **lista\_messages()**, a hibaüzenet hello tovább a sorhoz alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="19d0b-140">When you call **list\_messages()**, you get hello next message in a queue by default.</span></span> <span data-ttu-id="19d0b-141">Azt is megadhatja, hogy hány üzenetek tooget szeretné.</span><span class="sxs-lookup"><span data-stu-id="19d0b-141">You can also specify how many messages you want tooget.</span></span> <span data-ttu-id="19d0b-142">hello származó üzenetek **lista\_messages()** láthatatlan tooany ebből a várólistából üzeneteket olvasó többi kód válik.</span><span class="sxs-lookup"><span data-stu-id="19d0b-142">hello messages returned from **list\_messages()** becomes invisible tooany other code reading messages from this queue.</span></span> <span data-ttu-id="19d0b-143">Át a hello láthatósági időkorlátot másodpercben paraméterként.</span><span class="sxs-lookup"><span data-stu-id="19d0b-143">You pass in hello visibility timeout in seconds as a parameter.</span></span>
2. <span data-ttu-id="19d0b-144">toofinish eltávolításakor üdvözlőüzenetére hello üzenetsorból, meg kell is hívni **delete_message()**.</span><span class="sxs-lookup"><span data-stu-id="19d0b-144">toofinish removing hello message from hello queue, you must also call **delete_message()**.</span></span>

<span data-ttu-id="19d0b-145">A kétlépéses folyamat eltávolításával előállított üzenet biztosítja, hogy amikor a kód sikertelen tooprocess miatt toohardware vagy szoftver-hiba, a kód egy másik példánya üzenet kaphat hello ugyanazon üzenet, majd próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="19d0b-145">This two-step process of removing a message assures that when your code fails tooprocess a message due toohardware or software failure, another instance of your code can get hello same message and try again.</span></span> <span data-ttu-id="19d0b-146">A kód hívások **törlése\_message()** jobb gombbal az üdvözlő üzenet feldolgozása után.</span><span class="sxs-lookup"><span data-stu-id="19d0b-146">Your code calls **delete\_message()** right after hello message has been processed.</span></span>

```ruby
messages = azure_queue_service.list_messages("test-queue", 30)
azure_queue_service.delete_message("test-queue", 
  messages[0].id, messages[0].pop_receipt)
```

## <a name="how-to-change-hello-contents-of-a-queued-message"></a><span data-ttu-id="19d0b-147">Útmutató: Módosítsa az sorba állított üzenetek hello tartalma</span><span class="sxs-lookup"><span data-stu-id="19d0b-147">How To: Change hello Contents of a Queued Message</span></span>
<span data-ttu-id="19d0b-148">Módosíthatja egy üzenet helyben hello várólista hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="19d0b-148">You can change hello contents of a message in-place in hello queue.</span></span> <span data-ttu-id="19d0b-149">hello kódot használja hello **update_message()** metódus tooupdate üzenetet.</span><span class="sxs-lookup"><span data-stu-id="19d0b-149">hello code below uses hello **update_message()** method tooupdate a message.</span></span> <span data-ttu-id="19d0b-150">hello metódus visszaadható hello pop hello várólista üzenet fogadása tartalmazó rekordot és UTC dátum időt jelző érték amikor üdvözlőüzenetére hello várólista látható lesz.</span><span class="sxs-lookup"><span data-stu-id="19d0b-150">hello method will return a tuple which contains hello pop receipt of hello queue message and a UTC date time value that represents when hello message will be visible on hello queue.</span></span>

```ruby
message = azure_queue_service.list_messages("test-queue", 30)
pop_receipt, time_next_visible = azure_queue_service.update_message(
  "test-queue", message.id, message.pop_receipt, "updated test message", 
  30)
```

## <a name="how-to-additional-options-for-dequeuing-messages"></a><span data-ttu-id="19d0b-151">Útmutató: További beállítások üzenetmozgatót üzenetek</span><span class="sxs-lookup"><span data-stu-id="19d0b-151">How To: Additional Options for Dequeuing Messages</span></span>
<span data-ttu-id="19d0b-152">Két módon szabhatja testre az üzenetek lekérését egy üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="19d0b-152">There are two ways you can customize message retrieval from a queue.</span></span>

1. <span data-ttu-id="19d0b-153">A kötegelt üzenet kérheti le.</span><span class="sxs-lookup"><span data-stu-id="19d0b-153">You can get a batch of message.</span></span>
2. <span data-ttu-id="19d0b-154">Megadhat egy hosszabb vagy rövidebb láthatatlansági időkorlátot, így a kódnak több vagy kevesebb idő toofully feldolgozni az egyes üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="19d0b-154">You can set a longer or shorter invisibility timeout, allowing your code more or less time toofully process each message.</span></span>

<span data-ttu-id="19d0b-155">hello alábbi példakód hello **lista\_messages()** metódus tooget 15 üzenetek egy hívásban.</span><span class="sxs-lookup"><span data-stu-id="19d0b-155">hello following code example uses hello **list\_messages()** method tooget 15 messages in one call.</span></span> <span data-ttu-id="19d0b-156">Majd kinyomtatja, és törli az egyes üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="19d0b-156">Then it prints and deletes each message.</span></span> <span data-ttu-id="19d0b-157">Beállítja a hello láthatatlansági időtúllépés toofive percig, amíg minden üzenetet is.</span><span class="sxs-lookup"><span data-stu-id="19d0b-157">It also sets hello invisibility timeout toofive minutes for each message.</span></span>

```ruby
azure_queue_service.list_messages("test-queue", 300
  {:number_of_messages => 15}).each do |m|
  puts m.message_text
  azure_queue_service.delete_message("test-queue", m.id, m.pop_receipt)
end
```

## <a name="how-to-get-hello-queue-length"></a><span data-ttu-id="19d0b-158">How To: Get hello várólistájának hossza</span><span class="sxs-lookup"><span data-stu-id="19d0b-158">How To: Get hello Queue Length</span></span>
<span data-ttu-id="19d0b-159">Hello várólistában lévő üzenetek száma hello felmérését kérheti le.</span><span class="sxs-lookup"><span data-stu-id="19d0b-159">You can get an estimation of hello number of messages in hello queue.</span></span> <span data-ttu-id="19d0b-160">Hello **beolvasása\_várólista\_metadata()** metódus tesz hello várólista szolgáltatás tooreturn hello hozzávetőleges üzenet számán és a metaadatok hello várólista kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="19d0b-160">hello **get\_queue\_metadata()** method asks hello queue service tooreturn hello approximate message count and metadata about hello queue.</span></span>

```ruby
message_count, metadata = azure_queue_service.get_queue_metadata(
  "test-queue")
```

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="19d0b-161">Útmutató: A várólista törlése</span><span class="sxs-lookup"><span data-stu-id="19d0b-161">How To: Delete a Queue</span></span>
<span data-ttu-id="19d0b-162">toodelete várólista és köszönőüzenetei minden benne tárolt, hívás hello **törlése\_queue()** hello várólista-objektum metódust.</span><span class="sxs-lookup"><span data-stu-id="19d0b-162">toodelete a queue and all hello messages contained in it, call hello **delete\_queue()** method on hello queue object.</span></span>

```ruby
azure_queue_service.delete_queue("test-queue")
```

## <a name="next-steps"></a><span data-ttu-id="19d0b-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="19d0b-163">Next Steps</span></span>
<span data-ttu-id="19d0b-164">Most, hogy megismerte a queue storage alapjait hello, kövesse az összetettebb tárolási feladatok elvégzéséről kapcsolatos alábbi hivatkozások toolearn.</span><span class="sxs-lookup"><span data-stu-id="19d0b-164">Now that you've learned hello basics of queue storage, follow these links toolearn about more complex storage tasks.</span></span>

* <span data-ttu-id="19d0b-165">A Microsoft hello [Azure Storage csapat blogja](http://blogs.msdn.com/b/windowsazurestorage/)</span><span class="sxs-lookup"><span data-stu-id="19d0b-165">Visit hello [Azure Storage Team Blog](http://blogs.msdn.com/b/windowsazurestorage/)</span></span>
* <span data-ttu-id="19d0b-166">A Microsoft hello [Azure SDK for Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) GitHub tárházából</span><span class="sxs-lookup"><span data-stu-id="19d0b-166">Visit hello [Azure SDK for Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) repository on GitHub</span></span>

<span data-ttu-id="19d0b-167">Összehasonlítása a hello Azure Queue szolgáltatás az a cikk és az Azure Service Bus-üzenetsorok hello tárgyalt tárgyalt [hogyan toouse Service Bus-üzenetsorok](/develop/ruby/how-to-guides/service-bus-queues/) cikk című [Azure várólisták és a Service Bus-üzenetsorok - képest és Ellentétben](../../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)</span><span class="sxs-lookup"><span data-stu-id="19d0b-167">For a comparison between hello Azure Queue Service discussed in this article and Azure Service Bus Queues discussed in hello [How toouse Service Bus Queues](/develop/ruby/how-to-guides/service-bus-queues/) article, see [Azure Queues and Service Bus Queues - Compared and Contrasted](../../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)</span></span>
