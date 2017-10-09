---
title: "az Azure Service Bus-üzenettémák és előfizetések lépései aaaGet |} Microsoft Docs"
description: "Írjon Service Bus-üzenettémákat és előfizetéseket használó C# konzolalkalmazást."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/30/2017
ms.author: sethm
ms.openlocfilehash: 619d602599d97ecff2ded0681a383b19f1a8b7ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-service-bus-topics"></a><span data-ttu-id="1b03d-103">Bevezetés a Service Bus-üzenettémák használatába</span><span class="sxs-lookup"><span data-stu-id="1b03d-103">Get started with Service Bus topics</span></span>

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

## <a name="what-will-be-accomplished"></a><span data-ttu-id="1b03d-104">Az oktatóanyag célja</span><span class="sxs-lookup"><span data-stu-id="1b03d-104">What will be accomplished</span></span>

<span data-ttu-id="1b03d-105">Ez az oktatóanyag ismerteti a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="1b03d-105">This tutorial covers hello following steps:</span></span>

1. <span data-ttu-id="1b03d-106">Hozzon létre egy Service Bus-névtér, hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="1b03d-106">Create a Service Bus namespace, using hello Azure portal.</span></span>
2. <span data-ttu-id="1b03d-107">Hozzon létre egy Service Bus-témakörbe, hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="1b03d-107">Create a Service Bus topic, using hello Azure portal.</span></span>
3. <span data-ttu-id="1b03d-108">Hozzon létre egy Service Bus előfizetés toothat témakör, hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="1b03d-108">Create a Service Bus subscription toothat topic, using hello Azure portal.</span></span>
4. <span data-ttu-id="1b03d-109">A konzol alkalmazás toosend írni egy üzenet toohello témakör.</span><span class="sxs-lookup"><span data-stu-id="1b03d-109">Write a console application toosend a message toohello topic.</span></span>
5. <span data-ttu-id="1b03d-110">A konzol alkalmazás tooreceive beírni az üzenetet, hogy hello előfizetésből.</span><span class="sxs-lookup"><span data-stu-id="1b03d-110">Write a console application tooreceive that message from hello subscription.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1b03d-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1b03d-111">Prerequisites</span></span>

1. <span data-ttu-id="1b03d-112">[Visual Studio 2015 vagy újabb](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="1b03d-112">[Visual Studio 2015 or higher](http://www.visualstudio.com).</span></span> <span data-ttu-id="1b03d-113">az oktatóanyagban szereplő példák hello használja a Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="1b03d-113">hello examples in this tutorial use Visual Studio 2017.</span></span>
2. <span data-ttu-id="1b03d-114">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="1b03d-114">An Azure subscription.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-hello-azure-portal"></a><span data-ttu-id="1b03d-115">1. Hello Azure-portál használatával névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="1b03d-115">1. Create a namespace using hello Azure portal</span></span>

<span data-ttu-id="1b03d-116">Ha már létrehozott egy Service Bus üzenetkezelés névtér, jump toohello [létrehoz egy témát, hello Azure-portál használatával](#2-create-a-topic-using-the-azure-portal) szakasz.</span><span class="sxs-lookup"><span data-stu-id="1b03d-116">If you have already created a Service Bus Messaging namespace, jump toohello [Create a topic using hello Azure portal](#2-create-a-topic-using-the-azure-portal) section.</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-topic-using-hello-azure-portal"></a><span data-ttu-id="1b03d-117">2. Létrehoz egy témát, hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="1b03d-117">2. Create a topic using hello Azure portal</span></span>

1. <span data-ttu-id="1b03d-118">Jelentkezzen be toohello [Azure-portálon][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="1b03d-118">Log on toohello [Azure portal][azure-portal].</span></span>
2. <span data-ttu-id="1b03d-119">Hello portal hello bal oldali navigációs ablaktábláján kattintson **Service Bus** (Ha nem lát **Service Bus**, kattintson a **további szolgáltatások**).</span><span class="sxs-lookup"><span data-stu-id="1b03d-119">In hello left navigation pane of hello portal, click **Service Bus** (if you don't see **Service Bus**, click **More services**).</span></span>
3. <span data-ttu-id="1b03d-120">Kattintson a hello névtérre, amelyben szeretné toocreate hello témakör.</span><span class="sxs-lookup"><span data-stu-id="1b03d-120">Click hello namespace in which you would like toocreate hello topic.</span></span> <span data-ttu-id="1b03d-121">hello névtér áttekintése panel jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="1b03d-121">hello namespace overview blade appears:</span></span>
   
    ![Üzenettémakör létrehozása][createtopic1]
4. <span data-ttu-id="1b03d-123">A hello **Service Bus-névtér** panelen kattintson a **témakörök**, kattintson a **Hozzáadás témakör**.</span><span class="sxs-lookup"><span data-stu-id="1b03d-123">In hello **Service Bus namespace** blade, click **Topics**, then click **Add topic**.</span></span>
   
    ![Üzenettéma kiválasztása][createtopic2]
5. <span data-ttu-id="1b03d-125">Adjon meg egy nevet a hello témakör, és törölje a jelet hello **particionálás engedélyezése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="1b03d-125">Enter a name for hello topic, and uncheck hello **Enable partitioning** option.</span></span> <span data-ttu-id="1b03d-126">Hello a többi beállítást az alapértelmezett értékükön hagyja.</span><span class="sxs-lookup"><span data-stu-id="1b03d-126">Leave hello other options with their default values.</span></span>
   
    ![Új kiválasztása][createtopic3]
6. <span data-ttu-id="1b03d-128">Hello hello panel alsó részén, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="1b03d-128">At hello bottom of hello blade, click **Create**.</span></span>

## <a name="3-create-a-subscription-toohello-topic"></a><span data-ttu-id="1b03d-129">3. Egy előfizetés toohello téma</span><span class="sxs-lookup"><span data-stu-id="1b03d-129">3. Create a subscription toohello topic</span></span>

1. <span data-ttu-id="1b03d-130">Hello portál erőforrások ablaktáblában kattintson az 1. lépésben létrehozott hello névtér, majd a 2. lépésben létrehozott hello témakör nevét.</span><span class="sxs-lookup"><span data-stu-id="1b03d-130">In hello portal resources pane, click hello namespace you created in step 1, then click name of hello topic you created in step 2.</span></span>
2. <span data-ttu-id="1b03d-131">Hello áttekintés ablaktábláján, a hello tetején kattintson hello plus túl Ezután jelentkezzen**előfizetés** tooadd egy előfizetés toothis témakör.</span><span class="sxs-lookup"><span data-stu-id="1b03d-131">A hello top of hello overview pane, click hello plus sign next too**Subscription** tooadd a subscription toothis topic.</span></span>

    ![Előfizetés létrehozása][createtopic4]

3. <span data-ttu-id="1b03d-133">Adja meg a hello előfizetés nevét.</span><span class="sxs-lookup"><span data-stu-id="1b03d-133">Enter a name for hello subscription.</span></span> <span data-ttu-id="1b03d-134">Hello a többi beállítást az alapértelmezett értékükön hagyja.</span><span class="sxs-lookup"><span data-stu-id="1b03d-134">Leave hello other options with their default values.</span></span>

## <a name="4-send-messages-toohello-topic"></a><span data-ttu-id="1b03d-135">4. Üzenetek toohello témakör küldése</span><span class="sxs-lookup"><span data-stu-id="1b03d-135">4. Send messages toohello topic</span></span>

<span data-ttu-id="1b03d-136">toosend üzenetek toohello témakör azt írni egy C#-konzolalkalmazást a Visual Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="1b03d-136">toosend messages toohello topic, we write a C# console application using Visual Studio.</span></span>

### <a name="create-a-console-application"></a><span data-ttu-id="1b03d-137">Konzolalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1b03d-137">Create a console application</span></span>

<span data-ttu-id="1b03d-138">Indítsa el a Visual Studiót, majd hozzon létre egy új **Konzolalkalmazás (.NET-keretrendszer)** projektet.</span><span class="sxs-lookup"><span data-stu-id="1b03d-138">Launch Visual Studio and create a new **Console app (.NET Framework)** project.</span></span>

### <a name="add-hello-service-bus-nuget-package"></a><span data-ttu-id="1b03d-139">Hello Service Bus NuGet-csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1b03d-139">Add hello Service Bus NuGet package</span></span>

1. <span data-ttu-id="1b03d-140">Kattintson a jobb gombbal az újonnan létrehozott hello projektet, és válassza ki **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="1b03d-140">Right-click hello newly created project and select **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="1b03d-141">Hello kattintson **Tallózás** lapra, keressen rá **Microsoft Azure Service Bus**, majd válassza ki a hello **WindowsAzure.ServiceBus** elemet.</span><span class="sxs-lookup"><span data-stu-id="1b03d-141">Click hello **Browse** tab, search for **Microsoft Azure Service Bus**, and then select hello **WindowsAzure.ServiceBus** item.</span></span> <span data-ttu-id="1b03d-142">Kattintson a **telepítése** toocomplete hello telepítését, majd zárja be a párbeszédpanelt.</span><span class="sxs-lookup"><span data-stu-id="1b03d-142">Click **Install** toocomplete hello installation, then close this dialog box.</span></span>
   
    ![NuGet-csomag kiválasztása][nuget-pkg]

### <a name="write-some-code-toosend-a-message-toohello-topic"></a><span data-ttu-id="1b03d-144">Néhány kódot toosend írni egy üzenet toohello témakör</span><span class="sxs-lookup"><span data-stu-id="1b03d-144">Write some code toosend a message toohello topic</span></span>

1. <span data-ttu-id="1b03d-145">Adja hozzá a következő hello `using` hello Program.cs fájl elejéhez utasítás toohello.</span><span class="sxs-lookup"><span data-stu-id="1b03d-145">Add hello following `using` statement toohello top of hello Program.cs file.</span></span>
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. <span data-ttu-id="1b03d-146">Adja hozzá a következő kód toohello hello `Main` metódust.</span><span class="sxs-lookup"><span data-stu-id="1b03d-146">Add hello following code toohello `Main` method.</span></span> <span data-ttu-id="1b03d-147">Set hello `connectionString` változó toohello kapcsolati karakterlánc, amikor kapja meg, amikor hello névtér létrehozása, és állítsa be `topicName` toohello hello témakör létrehozásakor használt név.</span><span class="sxs-lookup"><span data-stu-id="1b03d-147">Set hello `connectionString` variable toohello connection string that you obtained when creating hello namespace, and set `topicName` toohello name that you used when creating hello topic.</span></span>
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = TopicClient.CreateFromConnectionString(connectionString, topicName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
    Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

    client.Send(message);

    Console.WriteLine("Message successfully sent! Press ENTER tooexit program");
    Console.ReadLine();
    ```
   
    <span data-ttu-id="1b03d-148">A Program.cs fájlnak így kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="1b03d-148">Here is what your Program.cs file should look like.</span></span>
   
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;

    namespace tsend
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";
                var topicName = "<your topic name>";

                var client = TopicClient.CreateFromConnectionString(connectionString, topicName);
                var message = new BrokeredMessage("This is a test message!");

                Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
                Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

                client.Send(message);

                Console.WriteLine("Message successfully sent! Press ENTER tooexit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. <span data-ttu-id="1b03d-149">Hello program futtatása, és ellenőrizze a hello Azure-portálon: kattintson a téma hello névtér nevére hello **áttekintése** panelen.</span><span class="sxs-lookup"><span data-stu-id="1b03d-149">Run hello program, and check hello Azure portal: click hello name of your topic in hello namespace **Overview** blade.</span></span> <span data-ttu-id="1b03d-150">hello témakör **Essentials** panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="1b03d-150">hello topic **Essentials** blade is displayed.</span></span> <span data-ttu-id="1b03d-151">A hello előfizetés(ek) felsorolt közelében hello hello panel alján, figyelje meg, hogy hello **üzenetek száma** minden előfizetésben kell 1 érték.</span><span class="sxs-lookup"><span data-stu-id="1b03d-151">In hello subscription(s) listed near hello bottom of hello blade, notice that hello **Message Count** value for each subscription should now be 1.</span></span> <span data-ttu-id="1b03d-152">Minden alkalommal, amikor hello küldő alkalmazás futtatása nélkül hello beolvasásra (hello tovább című részben leírtak szerint), ez az érték 1-gyel növekszik.</span><span class="sxs-lookup"><span data-stu-id="1b03d-152">Each time you run hello sender application without retrieving hello messages (as described in hello next section), this value increases by 1.</span></span> <span data-ttu-id="1b03d-153">Ne feledje, hogy hello hello témakör lépésekben hello aktuális méretét **aktuális** hello értéke **Essentials** panel minden alkalommal, amikor hello alkalmazást ad hozzá egy üzenet toohello témakört/előfizetést.</span><span class="sxs-lookup"><span data-stu-id="1b03d-153">Also note that hello current size of hello topic increments hello **Current** value on hello **Essentials** blade each time hello app adds a message toohello topic/subscription.</span></span>
   
      ![Üzenet mérete][topic-message]

## <a name="5-receive-messages-from-hello-subscription"></a><span data-ttu-id="1b03d-155">5. Üzenetek fogadása hello előfizetés</span><span class="sxs-lookup"><span data-stu-id="1b03d-155">5. Receive messages from hello subscription</span></span>

1. <span data-ttu-id="1b03d-156">tooreceive üdvözlőüzenetére vagy üzenetek csak küldte, hozzon létre egy új konzolalkalmazást, és adja hozzá egy hivatkozást toohello Service Bus NuGet-csomagot, hasonló toohello előző küldő alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="1b03d-156">tooreceive hello message or messages you just sent, create a new console application and add a reference toohello Service Bus NuGet package, similar toohello previous sender application.</span></span>
2. <span data-ttu-id="1b03d-157">Adja hozzá a következő hello `using` hello Program.cs fájl elejéhez utasítás toohello.</span><span class="sxs-lookup"><span data-stu-id="1b03d-157">Add hello following `using` statement toohello top of hello Program.cs file.</span></span>
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. <span data-ttu-id="1b03d-158">Adja hozzá a következő kód toohello hello `Main` metódust.</span><span class="sxs-lookup"><span data-stu-id="1b03d-158">Add hello following code toohello `Main` method.</span></span> <span data-ttu-id="1b03d-159">Set hello `connectionString` változó toohello kapcsolati karakterlánc kapja meg, amikor hello névtér létrehozása, és beállíthatja `topicName` toohello hello témakör létrehozásakor használt név.</span><span class="sxs-lookup"><span data-stu-id="1b03d-159">Set hello `connectionString` variable toohello connection string you obtained when creating hello namespace, and set `topicName` toohello name that you used when creating hello topic.</span></span>
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = SubscriptionClient.CreateFromConnectionString(connectionString, topicName, "<your subscription name>");
   
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
   
    Console.WriteLine("Press ENTER tooexit program");
    Console.ReadLine();
    ```
   
    <span data-ttu-id="1b03d-160">A Program.cs fájlnak így kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="1b03d-160">Here is what your Program.cs file should look like:</span></span>
   
    ```csharp
    using System;
    using Microsoft.ServiceBus.Messaging;
   
    namespace GettingStartedWithTopics
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";;
          var topicName = "<your topic name>";
   
          var client = SubscriptionClient.CreateFromConnectionString(connectionString, topicName, "<your subscription name>");
   
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
4. <span data-ttu-id="1b03d-161">Hello program futtatása, és hello portal ismételt ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="1b03d-161">Run hello program, and check hello portal again.</span></span> <span data-ttu-id="1b03d-162">Figyelje meg, hogy hello **üzenetek száma** és **aktuális** értékek most: 0.</span><span class="sxs-lookup"><span data-stu-id="1b03d-162">Notice that hello **Message Count** and **Current** values are now 0.</span></span>
   
    ![A témakör hossza][topic-message-receive]

<span data-ttu-id="1b03d-164">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="1b03d-164">Congratulations!</span></span> <span data-ttu-id="1b03d-165">Létrehozott egy üzenettémát és egy előfizetést, elküldött egy üzenetet, és fogadta is azt.</span><span class="sxs-lookup"><span data-stu-id="1b03d-165">You have now created a topic and subscription, sent a message, and received that message.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1b03d-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1b03d-166">Next steps</span></span>

<span data-ttu-id="1b03d-167">Tekintse meg a [minták a GitHub-tárházban](https://github.com/Azure/azure-service-bus/tree/master/samples) , amelyek bemutatják, hogy néhány hello fejlettebb funkciókat a Service Bus üzenetkezelés.</span><span class="sxs-lookup"><span data-stu-id="1b03d-167">Check out our [GitHub repository with samples](https://github.com/Azure/azure-service-bus/tree/master/samples) that demonstrate some of hello more advanced features of Service Bus messaging.</span></span>

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/nuget-package.png
[topic-message]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message.png
[topic-message-receive]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message-receive.png
[createtopic1]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic1.png
[createtopic2]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic2.png
[createtopic3]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic3.png
[createtopic4]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic4.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples
[azure-portal]: https://portal.azure.com
