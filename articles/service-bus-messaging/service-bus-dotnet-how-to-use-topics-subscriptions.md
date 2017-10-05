---
title: "Ismerkedés az Azure Service Bus-üzenettémákkal és előfizetésekkel | Microsoft Docs"
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
ms.openlocfilehash: 9401ada519f600b0d2817f06a396e16607a24129
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-service-bus-topics"></a><span data-ttu-id="d2b16-103">Bevezetés a Service Bus-üzenettémák használatába</span><span class="sxs-lookup"><span data-stu-id="d2b16-103">Get started with Service Bus topics</span></span>

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

## <a name="what-will-be-accomplished"></a><span data-ttu-id="d2b16-104">Az oktatóanyag célja</span><span class="sxs-lookup"><span data-stu-id="d2b16-104">What will be accomplished</span></span>

<span data-ttu-id="d2b16-105">Ez az oktatóanyag a következő lépéseken vezet végig:</span><span class="sxs-lookup"><span data-stu-id="d2b16-105">This tutorial covers the following steps:</span></span>

1. <span data-ttu-id="d2b16-106">Service Bus-névtér létrehozása az Azure Portal használatával.</span><span class="sxs-lookup"><span data-stu-id="d2b16-106">Create a Service Bus namespace, using the Azure portal.</span></span>
2. <span data-ttu-id="d2b16-107">Service Bus-üzenettéma létrehozása az Azure Portal használatával.</span><span class="sxs-lookup"><span data-stu-id="d2b16-107">Create a Service Bus topic, using the Azure portal.</span></span>
3. <span data-ttu-id="d2b16-108">Service Bus-előfizetés létrehozása az üzenettémához az Azure Portal használatával.</span><span class="sxs-lookup"><span data-stu-id="d2b16-108">Create a Service Bus subscription to that topic, using the Azure portal.</span></span>
4. <span data-ttu-id="d2b16-109">Az üzenettémához üzenetet küldő konzolalkalmazás megírása.</span><span class="sxs-lookup"><span data-stu-id="d2b16-109">Write a console application to send a message to the topic.</span></span>
5. <span data-ttu-id="d2b16-110">Az előfizetéstől üzenetet fogadó konzolalkalmazás megírása.</span><span class="sxs-lookup"><span data-stu-id="d2b16-110">Write a console application to receive that message from the subscription.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d2b16-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d2b16-111">Prerequisites</span></span>

1. <span data-ttu-id="d2b16-112">[Visual Studio 2015 vagy újabb](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="d2b16-112">[Visual Studio 2015 or higher](http://www.visualstudio.com).</span></span> <span data-ttu-id="d2b16-113">A jelen oktatóanyag példái a Visual Studio 2017-et használják.</span><span class="sxs-lookup"><span data-stu-id="d2b16-113">The examples in this tutorial use Visual Studio 2017.</span></span>
2. <span data-ttu-id="d2b16-114">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="d2b16-114">An Azure subscription.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a><span data-ttu-id="d2b16-115">1. Névtér létrehozása az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="d2b16-115">1. Create a namespace using the Azure portal</span></span>

<span data-ttu-id="d2b16-116">Ha már létrehozta a Service Bus Messaging-névteret, lépjen a [Üzenettéma létrehozása az Azure Portal használatával](#2-create-a-topic-using-the-azure-portal) szakaszra.</span><span class="sxs-lookup"><span data-stu-id="d2b16-116">If you have already created a Service Bus Messaging namespace, jump to the [Create a topic using the Azure portal](#2-create-a-topic-using-the-azure-portal) section.</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-topic-using-the-azure-portal"></a><span data-ttu-id="d2b16-117">2. Üzenettéma létrehozása az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="d2b16-117">2. Create a topic using the Azure portal</span></span>

1. <span data-ttu-id="d2b16-118">Jelentkezzen be az [Azure Portalra][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="d2b16-118">Log on to the [Azure portal][azure-portal].</span></span>
2. <span data-ttu-id="d2b16-119">A portál bal oldali navigációs panelén kattintson a **Service Bus** elemre (ha nem lát **Service Bus** elemet, kattintson a **További szolgáltatások** lehetőségre).</span><span class="sxs-lookup"><span data-stu-id="d2b16-119">In the left navigation pane of the portal, click **Service Bus** (if you don't see **Service Bus**, click **More services**).</span></span>
3. <span data-ttu-id="d2b16-120">Kattintson a névtérre, amelyben az üzenettémát létre kívánja hozni.</span><span class="sxs-lookup"><span data-stu-id="d2b16-120">Click the namespace in which you would like to create the topic.</span></span> <span data-ttu-id="d2b16-121">Megjelenik a névtér áttekintő panelje:</span><span class="sxs-lookup"><span data-stu-id="d2b16-121">The namespace overview blade appears:</span></span>
   
    ![Üzenettémakör létrehozása][createtopic1]
4. <span data-ttu-id="d2b16-123">A **Service Bus-névtér** panelen kattintson az **Üzenettémák**, majd az **Üzenettéma hozzáadása** elemre.</span><span class="sxs-lookup"><span data-stu-id="d2b16-123">In the **Service Bus namespace** blade, click **Topics**, then click **Add topic**.</span></span>
   
    ![Üzenettéma kiválasztása][createtopic2]
5. <span data-ttu-id="d2b16-125">Adjon meg egy nevet a témához, és törölje a jelet a **Particionálás engedélyezése** lehetőség mellől.</span><span class="sxs-lookup"><span data-stu-id="d2b16-125">Enter a name for the topic, and uncheck the **Enable partitioning** option.</span></span> <span data-ttu-id="d2b16-126">A többi beállítást hagyja az alapértelmezett értékükön.</span><span class="sxs-lookup"><span data-stu-id="d2b16-126">Leave the other options with their default values.</span></span>
   
    ![Új kiválasztása][createtopic3]
6. <span data-ttu-id="d2b16-128">Kattintson a panel alján található **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d2b16-128">At the bottom of the blade, click **Create**.</span></span>

## <a name="3-create-a-subscription-to-the-topic"></a><span data-ttu-id="d2b16-129">3. Előfizetés létrehozása az üzenettémához</span><span class="sxs-lookup"><span data-stu-id="d2b16-129">3. Create a subscription to the topic</span></span>

1. <span data-ttu-id="d2b16-130">A portál-erőforrások panelen kattintson az 1. lépésben létrehozott névtérre majd a 2. lépésben létrehozott üzenettéma nevére.</span><span class="sxs-lookup"><span data-stu-id="d2b16-130">In the portal resources pane, click the namespace you created in step 1, then click name of the topic you created in step 2.</span></span>
2. <span data-ttu-id="d2b16-131">Új előfizetést az áttekintő panel tetején az **Előfizetés** elem melletti plusz-jelre kattintva adhat az üzenettémához.</span><span class="sxs-lookup"><span data-stu-id="d2b16-131">A the top of the overview pane, click the plus sign next to **Subscription** to add a subscription to this topic.</span></span>

    ![Előfizetés létrehozása][createtopic4]

3. <span data-ttu-id="d2b16-133">Adjon egy nevet az előfizetésnek.</span><span class="sxs-lookup"><span data-stu-id="d2b16-133">Enter a name for the subscription.</span></span> <span data-ttu-id="d2b16-134">A többi beállítást hagyja az alapértelmezett értékükön.</span><span class="sxs-lookup"><span data-stu-id="d2b16-134">Leave the other options with their default values.</span></span>

## <a name="4-send-messages-to-the-topic"></a><span data-ttu-id="d2b16-135">4. Üzenet küldése az üzenettémához</span><span class="sxs-lookup"><span data-stu-id="d2b16-135">4. Send messages to the topic</span></span>

<span data-ttu-id="d2b16-136">A Visual Studio használatával C# konzolalkalmazást írunk az üzeneteknek az üzenettémához való küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="d2b16-136">To send messages to the topic, we write a C# console application using Visual Studio.</span></span>

### <a name="create-a-console-application"></a><span data-ttu-id="d2b16-137">Konzolalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="d2b16-137">Create a console application</span></span>

<span data-ttu-id="d2b16-138">Indítsa el a Visual Studiót, majd hozzon létre egy új **Konzolalkalmazás (.NET-keretrendszer)** projektet.</span><span class="sxs-lookup"><span data-stu-id="d2b16-138">Launch Visual Studio and create a new **Console app (.NET Framework)** project.</span></span>

### <a name="add-the-service-bus-nuget-package"></a><span data-ttu-id="d2b16-139">A Service Bus NuGet-csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d2b16-139">Add the Service Bus NuGet package</span></span>

1. <span data-ttu-id="d2b16-140">Kattintson a jobb gombbal az újonnan létrehozott projektre, és válassza a **Manage Nuget Packages** (NuGet-csomagok kezelése) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d2b16-140">Right-click the newly created project and select **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="d2b16-141">Kattintson a **Browse** (Tallózás) fülre, és keressen a **Microsoft Azure Service Bus** kifejezésre, majd válassza ki a **WindowsAzure.ServiceBus** elemet.</span><span class="sxs-lookup"><span data-stu-id="d2b16-141">Click the **Browse** tab, search for **Microsoft Azure Service Bus**, and then select the **WindowsAzure.ServiceBus** item.</span></span> <span data-ttu-id="d2b16-142">Kattintson a **Telepítés** gombra a telepítés befejezéséhez, majd zárja be a párbeszédpanelt.</span><span class="sxs-lookup"><span data-stu-id="d2b16-142">Click **Install** to complete the installation, then close this dialog box.</span></span>
   
    ![NuGet-csomag kiválasztása][nuget-pkg]

### <a name="write-some-code-to-send-a-message-to-the-topic"></a><span data-ttu-id="d2b16-144">Írjon egy kódrészletet egy üzenet küldéséhez az üzenettémához</span><span class="sxs-lookup"><span data-stu-id="d2b16-144">Write some code to send a message to the topic</span></span>

1. <span data-ttu-id="d2b16-145">Adja hozzá az alábbi `using` utasítást a Program.cs fájl elejéhez.</span><span class="sxs-lookup"><span data-stu-id="d2b16-145">Add the following `using` statement to the top of the Program.cs file.</span></span>
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. <span data-ttu-id="d2b16-146">Adja hozzá a következő kódot a(z) `Main` metódushoz.</span><span class="sxs-lookup"><span data-stu-id="d2b16-146">Add the following code to the `Main` method.</span></span> <span data-ttu-id="d2b16-147">A(z) `connectionString` változó értékének állítsa be a névtér létrehozásakor kapott kapcsolati karakterláncot, `topicName` értékének pedig az üzenettéma létrehozásakor használt nevet.</span><span class="sxs-lookup"><span data-stu-id="d2b16-147">Set the `connectionString` variable to the connection string that you obtained when creating the namespace, and set `topicName` to the name that you used when creating the topic.</span></span>
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = TopicClient.CreateFromConnectionString(connectionString, topicName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
    Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

    client.Send(message);

    Console.WriteLine("Message successfully sent! Press ENTER to exit program");
    Console.ReadLine();
    ```
   
    <span data-ttu-id="d2b16-148">A Program.cs fájlnak így kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="d2b16-148">Here is what your Program.cs file should look like.</span></span>
   
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

                Console.WriteLine("Message successfully sent! Press ENTER to exit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. <span data-ttu-id="d2b16-149">Futtassa a programot, és ellenőrizze az Azure Portalon: kattintson az üzenettéma nevére a névtér **Áttekintés** paneljén.</span><span class="sxs-lookup"><span data-stu-id="d2b16-149">Run the program, and check the Azure portal: click the name of your topic in the namespace **Overview** blade.</span></span> <span data-ttu-id="d2b16-150">Megjelenik az üzenettéma **Essentials** (Alapok) panelje.</span><span class="sxs-lookup"><span data-stu-id="d2b16-150">The topic **Essentials** blade is displayed.</span></span> <span data-ttu-id="d2b16-151">Arra figyeljen, hogy a panel alsó részén megjelenő előfizetés-lista minden tagjának **Message Count** (Üzenetek száma) értéke elvileg már 1.</span><span class="sxs-lookup"><span data-stu-id="d2b16-151">In the subscription(s) listed near the bottom of the blade, notice that the **Message Count** value for each subscription should now be 1.</span></span> <span data-ttu-id="d2b16-152">Valahányszor a küldő alkalmazást az üzenetek (a következő szakaszban leírt módon történő) fogadása nélkül futtatja, ez az érték egyel növekszik.</span><span class="sxs-lookup"><span data-stu-id="d2b16-152">Each time you run the sender application without retrieving the messages (as described in the next section), this value increases by 1.</span></span> <span data-ttu-id="d2b16-153">Azt is megfigyelheti, hogy a téma jelenlegi méretét tükröző **Current** (Jelenlegi) érték az **Essentials** panelen mindig növekszik, amikor az alkalmazás újabb üzenetet ad hozzá a témához/előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="d2b16-153">Also note that the current size of the topic increments the **Current** value on the **Essentials** blade each time the app adds a message to the topic/subscription.</span></span>
   
      ![Üzenet mérete][topic-message]

## <a name="5-receive-messages-from-the-subscription"></a><span data-ttu-id="d2b16-155">5. Üzenet fogadása az előfizetéstől</span><span class="sxs-lookup"><span data-stu-id="d2b16-155">5. Receive messages from the subscription</span></span>

1. <span data-ttu-id="d2b16-156">Az imént elküldött üzenet vagy üzenetek fogadásához hozzon létre egy új konzolalkalmazást, majd vegyen fel egy hivatkozást a Service Bus NuGet-csomagjára, hasonlóan az előző küldő alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="d2b16-156">To receive the message or messages you just sent, create a new console application and add a reference to the Service Bus NuGet package, similar to the previous sender application.</span></span>
2. <span data-ttu-id="d2b16-157">Adja hozzá az alábbi `using` utasítást a Program.cs fájl elejéhez.</span><span class="sxs-lookup"><span data-stu-id="d2b16-157">Add the following `using` statement to the top of the Program.cs file.</span></span>
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. <span data-ttu-id="d2b16-158">Adja hozzá a következő kódot a(z) `Main` metódushoz.</span><span class="sxs-lookup"><span data-stu-id="d2b16-158">Add the following code to the `Main` method.</span></span> <span data-ttu-id="d2b16-159">A(z) `connectionString` változó értékének állítsa be a névtér létrehozásakor kapott kapcsolati karakterláncot, `topicName` értékének pedig az üzenettéma létrehozásakor használt nevet.</span><span class="sxs-lookup"><span data-stu-id="d2b16-159">Set the `connectionString` variable to the connection string you obtained when creating the namespace, and set `topicName` to the name that you used when creating the topic.</span></span>
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = SubscriptionClient.CreateFromConnectionString(connectionString, topicName, "<your subscription name>");
   
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
   
    Console.WriteLine("Press ENTER to exit program");
    Console.ReadLine();
    ```
   
    <span data-ttu-id="d2b16-160">A Program.cs fájlnak így kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="d2b16-160">Here is what your Program.cs file should look like:</span></span>
   
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

          Console.WriteLine("Press ENTER to exit program");   
          Console.ReadLine();
        }
      }
    }
    ```
4. <span data-ttu-id="d2b16-161">Futtassa a programot, majd ellenőrizze ismét a portálon.</span><span class="sxs-lookup"><span data-stu-id="d2b16-161">Run the program, and check the portal again.</span></span> <span data-ttu-id="d2b16-162">Figyelje meg, hogy az **Message Count** (Üzenetek száma) és a **Current** (Jelenlegi) értéke most 0.</span><span class="sxs-lookup"><span data-stu-id="d2b16-162">Notice that the **Message Count** and **Current** values are now 0.</span></span>
   
    ![A témakör hossza][topic-message-receive]

<span data-ttu-id="d2b16-164">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="d2b16-164">Congratulations!</span></span> <span data-ttu-id="d2b16-165">Létrehozott egy üzenettémát és egy előfizetést, elküldött egy üzenetet, és fogadta is azt.</span><span class="sxs-lookup"><span data-stu-id="d2b16-165">You have now created a topic and subscription, sent a message, and received that message.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d2b16-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d2b16-166">Next steps</span></span>

<span data-ttu-id="d2b16-167">Tekintse meg a [GitHub-tárunkat, ahol további példákat talál](https://github.com/Azure/azure-service-bus/tree/master/samples), amelyek a Service Bus üzenetkezelési szolgáltatásának néhány speciális funkcióját mutatják be.</span><span class="sxs-lookup"><span data-stu-id="d2b16-167">Check out our [GitHub repository with samples](https://github.com/Azure/azure-service-bus/tree/master/samples) that demonstrate some of the more advanced features of Service Bus messaging.</span></span>

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
