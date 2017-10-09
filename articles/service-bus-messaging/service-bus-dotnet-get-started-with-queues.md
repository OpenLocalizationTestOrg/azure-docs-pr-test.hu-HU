---
title: "aaaGet Azure Service Bus-üzenetsorok használatába |} Microsoft Docs"
description: "Írjon egy Service Bus üzenetkezelési sorokat használó C# konzolalkalmazást."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 68a34c00-5600-43f6-bbcc-fea599d500da
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/26/2017
ms.author: sethm
ms.openlocfilehash: eaa362ab0eabd2427977398c1deab5dc00105ae9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-service-bus-queues"></a><span data-ttu-id="60bb0-103">Bevezetés a Service Bus által kezelt üzenetsorok használatába</span><span class="sxs-lookup"><span data-stu-id="60bb0-103">Get started with Service Bus queues</span></span>
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

## <a name="what-will-be-accomplished"></a><span data-ttu-id="60bb0-104">Az oktatóanyag célja</span><span class="sxs-lookup"><span data-stu-id="60bb0-104">What will be accomplished</span></span>
<span data-ttu-id="60bb0-105">Ez az oktatóanyag ismerteti a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="60bb0-105">This tutorial covers hello following steps:</span></span>

1. <span data-ttu-id="60bb0-106">Hozzon létre egy Service Bus-névtér, hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="60bb0-106">Create a Service Bus namespace, using hello Azure portal.</span></span>
2. <span data-ttu-id="60bb0-107">Hozzon létre egy Service Bus-üzenetsorba, hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="60bb0-107">Create a Service Bus queue, using hello Azure portal.</span></span>
3. <span data-ttu-id="60bb0-108">A konzol alkalmazás toosend egy üzenetet ír.</span><span class="sxs-lookup"><span data-stu-id="60bb0-108">Write a console application toosend a message.</span></span>
4. <span data-ttu-id="60bb0-109">A konzol alkalmazás tooreceive hello hello előző lépésben küldött üzenetek írni.</span><span class="sxs-lookup"><span data-stu-id="60bb0-109">Write a console application tooreceive hello messages sent in hello previous step.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="60bb0-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="60bb0-110">Prerequisites</span></span>
1. <span data-ttu-id="60bb0-111">[Visual Studio 2015 vagy újabb](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="60bb0-111">[Visual Studio 2015 or higher](http://www.visualstudio.com).</span></span> <span data-ttu-id="60bb0-112">az oktatóanyagban szereplő példák hello használja a Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="60bb0-112">hello examples in this tutorial use Visual Studio 2017.</span></span>
2. <span data-ttu-id="60bb0-113">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="60bb0-113">An Azure subscription.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-hello-azure-portal"></a><span data-ttu-id="60bb0-114">1. Hello Azure-portál használatával névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="60bb0-114">1. Create a namespace using hello Azure portal</span></span>
<span data-ttu-id="60bb0-115">Ha már létrehozott egy Service Bus üzenetkezelés névtér, jump toohello [hello Azure-portál használatával várólista létrehozása](#2-create-a-queue-using-the-azure-portal) szakasz.</span><span class="sxs-lookup"><span data-stu-id="60bb0-115">If you've already created a Service Bus Messaging namespace, jump toohello [Create a queue using hello Azure portal](#2-create-a-queue-using-the-azure-portal) section.</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-queue-using-hello-azure-portal"></a><span data-ttu-id="60bb0-116">2. Létrehoz egy sort hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="60bb0-116">2. Create a queue using hello Azure portal</span></span>
<span data-ttu-id="60bb0-117">Ha már létrehozott egy Service Bus-üzenetsorba, jump toohello [küldési üzenetek toohello sor](#3-send-messages-to-the-queue) szakasz.</span><span class="sxs-lookup"><span data-stu-id="60bb0-117">If you have already created a Service Bus queue, jump toohello [Send messages toohello queue](#3-send-messages-to-the-queue) section.</span></span>

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="3-send-messages-toohello-queue"></a><span data-ttu-id="60bb0-118">3. Üzenetek toohello várólista küldése</span><span class="sxs-lookup"><span data-stu-id="60bb0-118">3. Send messages toohello queue</span></span>
<span data-ttu-id="60bb0-119">toosend üzenetek toohello várólista, azt írni egy C#-konzolalkalmazást a Visual Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="60bb0-119">toosend messages toohello queue, we write a C# console application using Visual Studio.</span></span>

### <a name="create-a-console-application"></a><span data-ttu-id="60bb0-120">Konzolalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="60bb0-120">Create a console application</span></span>

<span data-ttu-id="60bb0-121">Indítsa el a Visual Studiót, majd hozzon létre egy új **Konzolalkalmazás (.NET-keretrendszer)** projektet.</span><span class="sxs-lookup"><span data-stu-id="60bb0-121">Launch Visual Studio and create a new **Console app (.NET Framework)** project.</span></span>

### <a name="add-hello-service-bus-nuget-package"></a><span data-ttu-id="60bb0-122">Hello Service Bus NuGet-csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="60bb0-122">Add hello Service Bus NuGet package</span></span>
1. <span data-ttu-id="60bb0-123">Kattintson a jobb gombbal az újonnan létrehozott hello projektet, és válassza ki **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="60bb0-123">Right-click hello newly created project and select **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="60bb0-124">Hello kattintson **Tallózás** lapra, keressen rá **Microsoft Azure Service Bus**, majd válassza ki a hello **WindowsAzure.ServiceBus** elemet.</span><span class="sxs-lookup"><span data-stu-id="60bb0-124">Click hello **Browse** tab, search for **Microsoft Azure Service Bus**, and then select hello **WindowsAzure.ServiceBus** item.</span></span> <span data-ttu-id="60bb0-125">Kattintson a **telepítése** toocomplete hello telepítését, majd zárja be a párbeszédpanelt.</span><span class="sxs-lookup"><span data-stu-id="60bb0-125">Click **Install** toocomplete hello installation, then close this dialog box.</span></span>
   
    ![NuGet-csomag kiválasztása][nuget-pkg]

### <a name="write-some-code-toosend-a-message-toohello-queue"></a><span data-ttu-id="60bb0-127">Néhány kódot toosend toohello üzenet-várólista írása</span><span class="sxs-lookup"><span data-stu-id="60bb0-127">Write some code toosend a message toohello queue</span></span>
1. <span data-ttu-id="60bb0-128">Adja hozzá a következő hello `using` hello Program.cs fájl elejéhez utasítás toohello.</span><span class="sxs-lookup"><span data-stu-id="60bb0-128">Add hello following `using` statement toohello top of hello Program.cs file.</span></span>
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. <span data-ttu-id="60bb0-129">Adja hozzá a következő kód toohello hello `Main` metódust.</span><span class="sxs-lookup"><span data-stu-id="60bb0-129">Add hello following code toohello `Main` method.</span></span> <span data-ttu-id="60bb0-130">Set hello `connectionString` változó toohello kapcsolati karakterlánc, amikor kapja meg, amikor hello névtér létrehozása, és állítsa be `queueName` toohello várólista hello várólista létrehozásakor használt név.</span><span class="sxs-lookup"><span data-stu-id="60bb0-130">Set hello `connectionString` variable toohello connection string that you obtained when creating hello namespace, and set `queueName` toohello queue name that you used when creating hello queue.</span></span>
   
    ```csharp
    var connectionString = "<your connection string>";
    var queueName = "<your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

    client.Send(message);

    Console.WriteLine("Message successfully sent! Press ENTER tooexit program");
    Console.ReadLine();
    ```
   
    <span data-ttu-id="60bb0-131">A Program.cs fájlnak így kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="60bb0-131">Here is what your Program.cs file should look like.</span></span>
   
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;

    namespace qsend
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";
                var queueName = "<your queue name>";

                var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
                var message = new BrokeredMessage("This is a test message!");

                Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

                client.Send(message);

                Console.WriteLine("Message successfully sent! Press ENTER tooexit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. <span data-ttu-id="60bb0-132">Hello program futtatása, és ellenőrizze a hello Azure-portálon: kattintson a sor hello névtér nevére hello **áttekintése** panelen.</span><span class="sxs-lookup"><span data-stu-id="60bb0-132">Run hello program, and check hello Azure portal: click hello name of your queue in hello namespace **Overview** blade.</span></span> <span data-ttu-id="60bb0-133">hello várólista **Essentials** panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="60bb0-133">hello queue **Essentials** blade is displayed.</span></span> <span data-ttu-id="60bb0-134">Figyelje meg, hogy hello **aktív üzenetek száma** értéket kell 1.</span><span class="sxs-lookup"><span data-stu-id="60bb0-134">Notice that hello **Active Message Count** value should now be 1.</span></span> <span data-ttu-id="60bb0-135">Minden egyes futtatásakor hello küldő alkalmazás köszönőüzenetei, lekérdezése nélkül ez az érték 1 nő.</span><span class="sxs-lookup"><span data-stu-id="60bb0-135">Each time you run hello sender application without retrieving hello messages, this value increases by 1.</span></span> <span data-ttu-id="60bb0-136">Vegye figyelembe, hogy hello hello várólista aktuális méretét növeli-e minden alkalommal hello alkalmazás bővíti ezenkívül toohello üzenet-várólista.</span><span class="sxs-lookup"><span data-stu-id="60bb0-136">Also note that hello current size of hello queue increments each time hello app adds a message toohello queue.</span></span>
   
      ![Üzenet mérete][queue-message]

## <a name="4-receive-messages-from-hello-queue"></a><span data-ttu-id="60bb0-138">4. Üzenetek fogadása hello üzenetsorból</span><span class="sxs-lookup"><span data-stu-id="60bb0-138">4. Receive messages from hello queue</span></span>

1. <span data-ttu-id="60bb0-139">tooreceive köszönőüzenetei csak küldte, hozzon létre egy új konzolalkalmazást, és adja hozzá egy hivatkozást toohello Service Bus NuGet-csomagot, hasonló toohello előző küldő alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="60bb0-139">tooreceive hello messages you just sent, create a new console application and add a reference toohello Service Bus NuGet package, similar toohello previous sender application.</span></span>
2. <span data-ttu-id="60bb0-140">Adja hozzá a következő hello `using` hello Program.cs fájl elejéhez utasítás toohello.</span><span class="sxs-lookup"><span data-stu-id="60bb0-140">Add hello following `using` statement toohello top of hello Program.cs file.</span></span>
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. <span data-ttu-id="60bb0-141">Adja hozzá a következő kód toohello hello `Main` metódust.</span><span class="sxs-lookup"><span data-stu-id="60bb0-141">Add hello following code toohello `Main` method.</span></span> <span data-ttu-id="60bb0-142">Set hello `connectionString` változó toohello kapcsolati karakterlánc lett kapja meg, amikor hello névtér létrehozása, és állítsa be `queueName` toohello várólista hello várólista létrehozásakor használt név.</span><span class="sxs-lookup"><span data-stu-id="60bb0-142">Set hello `connectionString` variable toohello connection string that was obtained when creating hello namespace, and set `queueName` toohello queue name that you used when creating hello queue.</span></span>
   
    ```csharp
    var connectionString = "<your connection string>";
    var queueName = "<your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
   
    Console.WriteLine("Press ENTER tooexit program");
    Console.ReadLine();
    ```
   
    <span data-ttu-id="60bb0-143">A Program.cs fájlnak így kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="60bb0-143">Here is what your Program.cs file should look like:</span></span>
   
    ```csharp
    using System;
    using Microsoft.ServiceBus.Messaging;
   
    namespace GettingStartedWithQueues
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";;
          var queueName = "<your queue name>";
   
          var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
          client.OnMessage(message =>
          {
            Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
            Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
          });

          Console.WriteLine("Press ENTER tooexit program");   
          Console.ReadLine();
        }
      }
    }
    ```
4. <span data-ttu-id="60bb0-144">Hello program futtatása, és hello portal ismételt ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="60bb0-144">Run hello program, and check hello portal again.</span></span> <span data-ttu-id="60bb0-145">Figyelje meg, hogy hello **aktív üzenetek száma** és **aktuális** értékek most: 0.</span><span class="sxs-lookup"><span data-stu-id="60bb0-145">Notice that hello **Active Message Count** and **Current** values are now 0.</span></span>
   
    ![Várólista hossza][queue-message-receive]

<span data-ttu-id="60bb0-147">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="60bb0-147">Congratulations!</span></span> <span data-ttu-id="60bb0-148">Ezzel létrehozott egy üzenetsort, illetve küldött és fogadott is üzenetet.</span><span class="sxs-lookup"><span data-stu-id="60bb0-148">You have now created a queue, sent a message, and received a message.</span></span>

## <a name="next-steps"></a><span data-ttu-id="60bb0-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="60bb0-149">Next steps</span></span>

<span data-ttu-id="60bb0-150">Tekintse meg a [minták a GitHub-tárházban](https://github.com/Azure/azure-service-bus/tree/master/samples) , amelyek bemutatják, hogy néhány hello fejlettebb funkciókat a Service Bus üzenetkezelés.</span><span class="sxs-lookup"><span data-stu-id="60bb0-150">Check out our [GitHub repository with samples](https://github.com/Azure/azure-service-bus/tree/master/samples) that demonstrate some of hello more advanced features of Service Bus messaging.</span></span>

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/queue-message.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples
