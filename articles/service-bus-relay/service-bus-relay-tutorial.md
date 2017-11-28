---
title: "Azure Service Bus WCF továbbító útmutató |} Microsoft Docs"
description: "Hozzon létre egy Service Bus-ügyfélalkalmazást, és WCF-továbbítási szolgáltatást."
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 53dfd236-97f1-4778-b376-be91aa14b842
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: sethm
ms.openlocfilehash: 5347bf85cad32b59677369d51a1f36529aef6662
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-wcf-relay-tutorial"></a><span data-ttu-id="63955-103">Azure WCF továbbító útmutató</span><span class="sxs-lookup"><span data-stu-id="63955-103">Azure WCF Relay tutorial</span></span>

<span data-ttu-id="63955-104">Ez az oktatóanyag ismerteti, hogyan hozhat létre egy egyszerű WCF továbbító ügyfélalkalmazást, és Azure Relay szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="63955-104">This tutorial describes how to build a simple WCF Relay client application and service using Azure Relay.</span></span> <span data-ttu-id="63955-105">Egy alkalmazó hasonló oktatóanyagért [Service Bus üzenetkezelés](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), lásd: [Ismerkedés a Service Bus-üzenetsorok](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span><span class="sxs-lookup"><span data-stu-id="63955-105">For a similar tutorial that uses [Service Bus Messaging](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), see [Get started with Service Bus queues](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span></span>

<span data-ttu-id="63955-106">Az oktatóanyag feldolgozása révén lehetővé teszi egy WCF továbbító ügyfél és -szogáltatásalkalmazás létrehozásához szükséges lépéseket megértéséhez.</span><span class="sxs-lookup"><span data-stu-id="63955-106">Working through this tutorial gives you an understanding of the steps that are required to create a WCF Relay client and service application.</span></span> <span data-ttu-id="63955-107">Az eredeti WCF megfelelők esetében, mint például a "szolgáltatás" olyan konstrukció, amely egy vagy több végpontot, azt mutatja, amelyek egy vagy több szolgáltatási műveletet tesz elérhetővé.</span><span class="sxs-lookup"><span data-stu-id="63955-107">Like their original WCF counterparts, a service is a construct that exposes one or more endpoints, each of which exposes one or more service operations.</span></span> <span data-ttu-id="63955-108">A szolgáltatások végpontja megad egy címet, ahol a szolgáltatás megtalálható, egy kötést, amely tartalmazza az információkat, amelyeket az ügyfélnek kommunikálnia kell a szolgáltatás felé, valamint egy szerződést, amely meghatározza a szolgáltatás által az ügyfeleknek nyújtott funkciókat.</span><span class="sxs-lookup"><span data-stu-id="63955-108">The endpoint of a service specifies an address where the service can be found, a binding that contains the information that a client must communicate with the service, and a contract that defines the functionality provided by the service to its clients.</span></span> <span data-ttu-id="63955-109">A WCF és WCF továbbító közötti fő különbség a, hogy a végpont a számítógépre helyi ahelyett, hogy a felhőben van közzétéve.</span><span class="sxs-lookup"><span data-stu-id="63955-109">The main difference between WCF and WCF Relay is that the endpoint is exposed in the cloud instead of locally on your computer.</span></span>

<span data-ttu-id="63955-110">Miután a végére ért az oktatóanyag témaköreinek, rendelkezni fog egy futó szolgáltatással, valamint egy, a szolgáltatás műveleteinek meghívására képes ügyféllel.</span><span class="sxs-lookup"><span data-stu-id="63955-110">After you work through the sequence of topics in this tutorial, you will have a running service, and a client that can invoke the operations of the service.</span></span> <span data-ttu-id="63955-111">Az első témakör egy fiók beállítását ismerteti.</span><span class="sxs-lookup"><span data-stu-id="63955-111">The first topic describes how to set up an account.</span></span> <span data-ttu-id="63955-112">A következő lépések leírják, hogyan határozhat meg egy szerződést használó szolgáltatást, hogyan valósíthatja meg a szolgáltatást, valamint hogyan konfigurálhatja a szolgáltatást a kódban.</span><span class="sxs-lookup"><span data-stu-id="63955-112">The next steps describe how to define a service that uses a contract, how to implement the service, and how to configure the service in code.</span></span> <span data-ttu-id="63955-113">Azt is leírják, hogyan működtethető és futtatható a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="63955-113">They also describe how to host and run the service.</span></span> <span data-ttu-id="63955-114">A létrejött szolgáltatás próbaüzemben működik, és az ügyfél és a szolgáltatás ugyanazon a számítógépen fut.</span><span class="sxs-lookup"><span data-stu-id="63955-114">The service that is created is self-hosted and the client and service run on the same computer.</span></span> <span data-ttu-id="63955-115">A szolgáltatást kód vagy egy konfigurációs fájl segítségével is konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="63955-115">You can configure the service by using either code or a configuration file.</span></span>

<span data-ttu-id="63955-116">Az utolsó három lépés azt írja le, hogy hogyan hozhat létre egy ügyfélalkalmazást, hogyan konfigurálhatja azt, valamint hogyan hozhat létre és használhat olyan ügyfélalkalmazásokat, amelyek hozzáférnek a gazdagép funkcióihoz.</span><span class="sxs-lookup"><span data-stu-id="63955-116">The final three steps describe how to create a client application, configure the client application, and create and use a client that can access the functionality of the host.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="63955-117">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="63955-117">Prerequisites</span></span>

<span data-ttu-id="63955-118">Az oktatóanyag teljesítéséhez az alábbiakra lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="63955-118">To complete this tutorial, you'll need the following:</span></span>

* <span data-ttu-id="63955-119">[Microsoft Visual Studio 2015 vagy újabb](http://visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="63955-119">[Microsoft Visual Studio 2015 or higher](http://visualstudio.com).</span></span> <span data-ttu-id="63955-120">Ez az oktatóanyag a Visual Studio 2017 használja.</span><span class="sxs-lookup"><span data-stu-id="63955-120">This tutorial uses Visual Studio 2017.</span></span>
* <span data-ttu-id="63955-121">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="63955-121">An active Azure account.</span></span> <span data-ttu-id="63955-122">Ha még nincs fiókja, néhány perc alatt létrehozhat egy ingyenes fiókot.</span><span class="sxs-lookup"><span data-stu-id="63955-122">If you don't have one, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="63955-123">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="63955-123">For details, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="create-a-service-namespace"></a><span data-ttu-id="63955-124">Szolgáltatásnévtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="63955-124">Create a service namespace</span></span>

<span data-ttu-id="63955-125">Az első lépés egy névtér létrehozása, valamint a egy [közös hozzáférésű Jogosultságkód (SAS)](../service-bus-messaging/service-bus-sas.md) kulcs.</span><span class="sxs-lookup"><span data-stu-id="63955-125">The first step is to create a namespace, and to obtain a [Shared Access Signature (SAS)](../service-bus-messaging/service-bus-sas.md) key.</span></span> <span data-ttu-id="63955-126">A névtér egy biztosít a továbbítási szolgáltatás keresztül közzétett minden alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="63955-126">A namespace provides an application boundary for each application exposed through the relay service.</span></span> <span data-ttu-id="63955-127">Az SAS-kulcsot a rendszer automatikusan előállítja a szolgáltatásnévtér létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="63955-127">A SAS key is automatically generated by the system when a service namespace is created.</span></span> <span data-ttu-id="63955-128">A szolgáltatásnévtér és SAS-kulcs együttes használata a hitelesítő adatokat az Azure-alkalmazáshoz való hozzáférés hitelesítéséhez biztosít.</span><span class="sxs-lookup"><span data-stu-id="63955-128">The combination of service namespace and SAS key provides the credentials for Azure to authenticate access to an application.</span></span> <span data-ttu-id="63955-129">Relay-névtér létrehozásához kövesse az [itt leírt utasításokat](relay-create-namespace-portal.md).</span><span class="sxs-lookup"><span data-stu-id="63955-129">Follow the [instructions here](relay-create-namespace-portal.md) to create a Relay namespace.</span></span>

## <a name="define-a-wcf-service-contract"></a><span data-ttu-id="63955-130">A WCF szolgáltatási szerződés megadása</span><span class="sxs-lookup"><span data-stu-id="63955-130">Define a WCF service contract</span></span>

<span data-ttu-id="63955-131">A szolgáltatási szerződés Megadja, milyen műveleteket (webszolgáltatás-terminológia a metódusokhoz és függvényeket) a szolgáltatás támogatja.</span><span class="sxs-lookup"><span data-stu-id="63955-131">The service contract specifies what operations (the web service terminology for methods or functions) the service supports.</span></span> <span data-ttu-id="63955-132">A szerződések a C++, a C# vagy a Visual Basic felület meghatározásával jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="63955-132">Contracts are created by defining a C++, C#, or Visual Basic interface.</span></span> <span data-ttu-id="63955-133">A felület minden metódusa egy konkrét szolgáltatási műveletnek felel meg.</span><span class="sxs-lookup"><span data-stu-id="63955-133">Each method in the interface corresponds to a specific service operation.</span></span> <span data-ttu-id="63955-134">A [ServiceContractAttriibute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribútumot minden felületre, az [OperationContractAttribaute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribútumot pedig minden műveletre alkalmazni kell.</span><span class="sxs-lookup"><span data-stu-id="63955-134">Each interface must have the [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribute applied to it, and each operation must have the [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribute applied to it.</span></span> <span data-ttu-id="63955-135">Ha egy felület egy metódusa rendelkezik a [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribútummal, de nem rendelkezik az [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribútummal, nem lesz közzétéve.</span><span class="sxs-lookup"><span data-stu-id="63955-135">If a method in an interface that has the [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribute does not have the [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribute, that method is not exposed.</span></span> <span data-ttu-id="63955-136">A feladatok kódja megtalálható az eljárást követő példában.</span><span class="sxs-lookup"><span data-stu-id="63955-136">The code for these tasks is provided in the example following the procedure.</span></span> <span data-ttu-id="63955-137">A szerződések és szolgáltatások részletesebb leírása a WCF-dokumentáció [Designing and Implementing Services](https://msdn.microsoft.com/library/ms729746.aspx) (Szolgáltatások tervezése és megvalósítása) témakörében található.</span><span class="sxs-lookup"><span data-stu-id="63955-137">For a larger discussion of contracts and services, see [Designing and Implementing Services](https://msdn.microsoft.com/library/ms729746.aspx) in the WCF documentation.</span></span>

### <a name="create-a-relay-contract-with-an-interface"></a><span data-ttu-id="63955-138">Továbbító szerződés létrehozása felülettel</span><span class="sxs-lookup"><span data-stu-id="63955-138">Create a relay contract with an interface</span></span>

1. <span data-ttu-id="63955-139">Nyissa meg a Visual Studiót rendszergazdaként. Ehhez a **Start** menüben kattintson a jobb gombbal a programra, majd válassza a **Futtatás rendszergazdaként** parancsot.</span><span class="sxs-lookup"><span data-stu-id="63955-139">Open Visual Studio as an administrator by right-clicking the program in the **Start** menu and selecting **Run as administrator**.</span></span>
2. <span data-ttu-id="63955-140">Hozzon létre új egy új konzolalkalmazás-projektet.</span><span class="sxs-lookup"><span data-stu-id="63955-140">Create a new console application project.</span></span> <span data-ttu-id="63955-141">Kattintson a **File** (Fájl) menüre, és válassza a **New** (Új), majd a **Project** (Projekt) elemet.</span><span class="sxs-lookup"><span data-stu-id="63955-141">Click the **File** menu and select **New**, then click **Project**.</span></span> <span data-ttu-id="63955-142">A **New Project** (Új projekt) párbeszédpanelen kattintson a **Visual C#** elemre (ha a **Visual C#** nem jelenik meg, keresse meg az **Other Languages** (Más nyelvek) területen).</span><span class="sxs-lookup"><span data-stu-id="63955-142">In the **New Project** dialog, click **Visual C#** (if **Visual C#** does not appear, look under **Other Languages**).</span></span> <span data-ttu-id="63955-143">Kattintson a **Konzolalkalmazás (.NET-keretrendszer)** sablont, és adjon neki nevet **EchoService**.</span><span class="sxs-lookup"><span data-stu-id="63955-143">Click the **Console App (.NET Framework)** template, and name it **EchoService**.</span></span> <span data-ttu-id="63955-144">A projekt létrehozásához kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="63955-144">Click **OK** to create the project.</span></span>

    ![][2]

3. <span data-ttu-id="63955-145">Telepítse a Service Bus NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="63955-145">Install the Service Bus NuGet package.</span></span> <span data-ttu-id="63955-146">Ez a csomag automatikusan hivatkozásokat ad a Service Bus-könyvtárakhoz, valamint a WCF **System.ServiceModel** névtérhez.</span><span class="sxs-lookup"><span data-stu-id="63955-146">This package automatically adds references to the Service Bus libraries, as well as the WCF **System.ServiceModel**.</span></span> <span data-ttu-id="63955-147">A [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) az a névtér, amely lehetővé teszi a programozott hozzáférést a WCF alapszintű szolgáltatásaihoz.</span><span class="sxs-lookup"><span data-stu-id="63955-147">[System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) is the namespace that enables you to programmatically access the basic features of WCF.</span></span> <span data-ttu-id="63955-148">A Service Bus számos WCF-objektumot és -attribútumot használ a szolgáltatási szerződések meghatározására.</span><span class="sxs-lookup"><span data-stu-id="63955-148">Service Bus uses many of the objects and attributes of WCF to define service contracts.</span></span>

    <span data-ttu-id="63955-149">A Megoldáskezelőben kattintson a jobb gombbal a projektre, és kattintson **NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="63955-149">In Solution Explorer, right-click the project, and then click **Manage NuGet Packages...**.</span></span> <span data-ttu-id="63955-150">Kattintson a **Browse** (Tallózás) lapra, és keressen a következőre: `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="63955-150">Click the **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="63955-151">Ügyeljen arra, hogy a projekt neve ki legyen jelölve a **Version(s)** (Verzió(k)) mezőben.</span><span class="sxs-lookup"><span data-stu-id="63955-151">Ensure that the project name is selected in the **Version(s)** box.</span></span> <span data-ttu-id="63955-152">Kattintson az **Install** (Telepítés) gombra, és fogadja el a használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="63955-152">Click **Install**, and accept the terms of use.</span></span>

    ![][3]
4. <span data-ttu-id="63955-153">A Solution Explorerben (Megoldáskezelőben) kattintson duplán a Program.cs fájlra a szerkesztőben való megnyitásához, ha még nincs megnyitva.</span><span class="sxs-lookup"><span data-stu-id="63955-153">In Solution Explorer, double-click the Program.cs file to open it in the editor, if it is not already open.</span></span>
5. <span data-ttu-id="63955-154">Adja hozzá a következő using utasításokat a fájl elejéhez:</span><span class="sxs-lookup"><span data-stu-id="63955-154">Add the following using statements at the top of the file:</span></span>

    ```csharp
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```
6. <span data-ttu-id="63955-155">Módosítsa a névtér alapértelmezett **EchoService** nevét a következőre: **Microsoft.ServiceBus.Samples**.</span><span class="sxs-lookup"><span data-stu-id="63955-155">Change the namespace name from its default name of **EchoService** to **Microsoft.ServiceBus.Samples**.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="63955-156">Ebben az oktatóanyagban a C# névteret használja **Microsoft.ServiceBus.Samples**, amely a szerződés-alapú névtere felügyelt típus, amely a konfigurációs fájl használatban van a [a WCF-ügyfél konfigurálása](#configure-the-wcf-client) a lépést.</span><span class="sxs-lookup"><span data-stu-id="63955-156">This tutorial uses the C# namespace **Microsoft.ServiceBus.Samples**, which is the namespace of the contract-based managed type that is used in the configuration file in the [Configure the WCF client](#configure-the-wcf-client) step.</span></span> <span data-ttu-id="63955-157">A minta létrehozásakor bármilyen névteret megadhat, az oktatóanyag azonban nem fog működni, ha ezután nem módosítja a szerződés és a szolgáltatás névtereit is ennek megfelelően az alkalmazáskonfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="63955-157">You can specify any namespace you want when you build this sample; however, the tutorial will not work unless you then modify the namespaces of the contract and service accordingly, in the application configuration file.</span></span> <span data-ttu-id="63955-158">Az App.config fájlban megadott névtérnek egyeznie kell a C# fájlokban megadott névtérrel.</span><span class="sxs-lookup"><span data-stu-id="63955-158">The namespace specified in the App.config file must be the same as the namespace specified in your C# files.</span></span>
   >
   >
7. <span data-ttu-id="63955-159">Közvetlenül után a `Microsoft.ServiceBus.Samples` névtér-deklaráció, de a névtéren belül, adja meg egy új nevű felületet `IEchoContract` , és alkalmazza a `ServiceContractAttribute` attribútumot a felületre a névtér értékű `http://samples.microsoft.com/ServiceModel/Relay/`.</span><span class="sxs-lookup"><span data-stu-id="63955-159">Directly after the `Microsoft.ServiceBus.Samples` namespace declaration, but within the namespace, define a new interface named `IEchoContract` and apply the `ServiceContractAttribute` attribute to the interface with a namespace value of `http://samples.microsoft.com/ServiceModel/Relay/`.</span></span> <span data-ttu-id="63955-160">A névtér értéke különbözik a kód tartományában használt névtértől.</span><span class="sxs-lookup"><span data-stu-id="63955-160">The namespace value differs from the namespace that you use throughout the scope of your code.</span></span> <span data-ttu-id="63955-161">A névtér értéke ehelyett egyedi azonosítóként van használatban ehhez a szerződéshez.</span><span class="sxs-lookup"><span data-stu-id="63955-161">Instead, the namespace value is used as a unique identifier for this contract.</span></span> <span data-ttu-id="63955-162">A névtér explicit meghatározásával megelőzhető az alapértelmezett névtér hozzáadása a szerződésnévhez.</span><span class="sxs-lookup"><span data-stu-id="63955-162">Specifying the namespace explicitly prevents the default namespace value from being added to the contract name.</span></span> <span data-ttu-id="63955-163">Illessze be a következő kódot a névtér-deklaráció után:</span><span class="sxs-lookup"><span data-stu-id="63955-163">Paste the following code after the namespace declaration:</span></span>

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

   > [!NOTE]
   > <span data-ttu-id="63955-164">A szolgáltatási szerződés névtere általában tartalmaz egy elnevezési sémát, amely tartalmazza a verzióinformációkat.</span><span class="sxs-lookup"><span data-stu-id="63955-164">Typically, the service contract namespace contains a naming scheme that includes version information.</span></span> <span data-ttu-id="63955-165">Ha a verzióinformációk szerepelnek a szolgáltatási szerződés névterében, a szolgáltatások képesek elkülöníteni a nagyobb módosításokat egy új szolgáltatási szerződés új névtérrel való meghatározása, valamint egy új végponton való megjelenítése révén.</span><span class="sxs-lookup"><span data-stu-id="63955-165">Including version information in the service contract namespace enables services to isolate major changes by defining a new service contract with a new namespace and exposing it on a new endpoint.</span></span> <span data-ttu-id="63955-166">Ezen a módon kikapcsolja az ügyfelek továbbra is használhatja a régi szolgáltatási szerződést anélkül, hogy frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="63955-166">In this manner, clients can continue to use the old service contract without having to be updated.</span></span> <span data-ttu-id="63955-167">A verzióinformációk dátumot vagy buildszámot tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="63955-167">Version information can consist of a date or a build number.</span></span> <span data-ttu-id="63955-168">További információ: [Service Versioning](http://go.microsoft.com/fwlink/?LinkID=180498) (Szolgáltatás verziószámozása).</span><span class="sxs-lookup"><span data-stu-id="63955-168">For more information, see [Service Versioning](http://go.microsoft.com/fwlink/?LinkID=180498).</span></span> <span data-ttu-id="63955-169">A jelen oktatóanyag esetén a szolgáltatási szerződés elnevezési sémája nem tartalmazza a verzióinformációkat.</span><span class="sxs-lookup"><span data-stu-id="63955-169">For the purposes of this tutorial, the naming scheme of the service contract namespace does not contain version information.</span></span>
   >
   >
8. <span data-ttu-id="63955-170">Belül a `IEchoContract` csatoló, deklaráljon egy metódust az egyetlen művelettel a `IEchoContract` szerződés által a felületen, és alkalmazza a `OperationContractAttribute` attribútumot az alábbiak szerint a WCF továbbító szerződés részeként közzétenni kívánt metódusra:</span><span class="sxs-lookup"><span data-stu-id="63955-170">Within the `IEchoContract` interface, declare a method for the single operation the `IEchoContract` contract exposes in the interface and apply the `OperationContractAttribute` attribute to the method that you want to expose as part of the public WCF Relay contract, as follows:</span></span>

    ```csharp
    [OperationContract]
    string Echo(string text);
    ```
9. <span data-ttu-id="63955-171">Közvetlenül az `IEchoContract` felület deklarációja után deklaráljon egy csatornát, amely örökli az `IEchoContract` és az `IClientChannel` felületek tulajdonságait is, ahogy az itt látható:</span><span class="sxs-lookup"><span data-stu-id="63955-171">Directly after the `IEchoContract` interface definition, declare a channel that inherits from both `IEchoContract` and also to the `IClientChannel` interface, as shown here:</span></span>

    ```csharp
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    <span data-ttu-id="63955-172">A csatorna egy olyan WCF-objektum, amelyen keresztül a gazda és az ügyfél információkat adnak át egymásnak.</span><span class="sxs-lookup"><span data-stu-id="63955-172">A channel is the WCF object through which the host and client pass information to each other.</span></span> <span data-ttu-id="63955-173">Később kódot ír majd a csatornára, hogy az echo utasítás használatával kommunikálja az információkat a két alkalmazás között.</span><span class="sxs-lookup"><span data-stu-id="63955-173">Later, you will write code against the channel to echo information between the two applications.</span></span>
10. <span data-ttu-id="63955-174">A **Build** (Létrehozás) menüben kattintson a **Build Solution** (Megoldás létrehozása) elemre, vagy nyomja meg a **Ctrl+Shift+B** billentyűkombinációt az eddigi munkája pontosságának ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="63955-174">From the **Build** menu, click **Build Solution** or press **Ctrl+Shift+B** to confirm the accuracy of your work so far.</span></span>

### <a name="example"></a><span data-ttu-id="63955-175">Példa</span><span class="sxs-lookup"><span data-stu-id="63955-175">Example</span></span>

<span data-ttu-id="63955-176">A következő kód bemutatja egy WCF továbbító szerződést meghatározó alapszintű felületet.</span><span class="sxs-lookup"><span data-stu-id="63955-176">The following code shows a basic interface that defines a WCF Relay contract.</span></span>

```csharp
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

<span data-ttu-id="63955-177">Most, hogy létrejött a felület, megvalósíthatja azt.</span><span class="sxs-lookup"><span data-stu-id="63955-177">Now that the interface is created, you can implement the interface.</span></span>

## <a name="implement-the-wcf-contract"></a><span data-ttu-id="63955-178">A WCF szerződés megvalósítása</span><span class="sxs-lookup"><span data-stu-id="63955-178">Implement the WCF contract</span></span>

<span data-ttu-id="63955-179">Egy Azure relay létrehozásához szükséges, hogy először létre kell hoznia a szerződést, amelyet egy felület használatával.</span><span class="sxs-lookup"><span data-stu-id="63955-179">Creating an Azure relay requires that you first create the contract, which is defined by using an interface.</span></span> <span data-ttu-id="63955-180">A felület létrehozásáról az előző lépésben találhat további információkat.</span><span class="sxs-lookup"><span data-stu-id="63955-180">For more information about creating the interface, see the previous step.</span></span> <span data-ttu-id="63955-181">A következő lépés a felület megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="63955-181">The next step is to implement the interface.</span></span> <span data-ttu-id="63955-182">Ebbe beletartozik egy `EchoService` nevű osztály létrehozása, amely megvalósítja a felhasználó által megadott `IEchoContract` felületet.</span><span class="sxs-lookup"><span data-stu-id="63955-182">This involves creating a class named `EchoService` that implements the user-defined `IEchoContract` interface.</span></span> <span data-ttu-id="63955-183">A felület megvalósítása után egy App.config konfigurációs fájl segítségével konfigurálhatja azt.</span><span class="sxs-lookup"><span data-stu-id="63955-183">After you implement the interface, you then configure the interface using an App.config configuration file.</span></span> <span data-ttu-id="63955-184">A konfigurációs fájl tartalmazza a szükséges információkat az alkalmazás, például a szolgáltatás nevét, a neve, a szerződés és a továbbítási szolgáltatás folytatott kommunikációhoz használt protokoll típusát.</span><span class="sxs-lookup"><span data-stu-id="63955-184">The configuration file contains necessary information for the application, such as the name of the service, the name of the contract, and the type of protocol that is used to communicate with the relay service.</span></span> <span data-ttu-id="63955-185">A feladatokhoz használt kód megtalálható az eljárást követő példában.</span><span class="sxs-lookup"><span data-stu-id="63955-185">The code used for these tasks is provided in the example following the procedure.</span></span> <span data-ttu-id="63955-186">A szolgáltatási szerződések megvalósításáról a WCF-dokumentáció [Implementing Service Contracts](https://msdn.microsoft.com/library/ms733764.aspx) (Szolgáltatási szerződések megvalósítása) szakaszában található átfogó ismertetés.</span><span class="sxs-lookup"><span data-stu-id="63955-186">For a more general discussion about how to implement a service contract, see [Implementing Service Contracts](https://msdn.microsoft.com/library/ms733764.aspx) in the WCF documentation.</span></span>

1. <span data-ttu-id="63955-187">Hozzon létre egy új, `EchoService` nevű osztályt közvetlenül az `IEchoContract` felület meghatározása után.</span><span class="sxs-lookup"><span data-stu-id="63955-187">Create a new class named `EchoService` directly after the definition of the `IEchoContract` interface.</span></span> <span data-ttu-id="63955-188">Az `EchoService` osztály megvalósítja az `IEchoContract` felületet.</span><span class="sxs-lookup"><span data-stu-id="63955-188">The `EchoService` class implements the `IEchoContract` interface.</span></span>

    ```csharp
    class EchoService : IEchoContract
    {
    }
    ```

    <span data-ttu-id="63955-189">Hasonlóan az egyéb felületi megvalósításokhoz, a definíciót megvalósíthatja egy másik fájlban.</span><span class="sxs-lookup"><span data-stu-id="63955-189">Similar to other interface implementations, you can implement the definition in a different file.</span></span> <span data-ttu-id="63955-190">Ebben az oktatóanyagban azonban a megvalósítás ugyanabban a fájlban található, mint a felületdefiníció és a `Main` metódus.</span><span class="sxs-lookup"><span data-stu-id="63955-190">However, for this tutorial, the implementation is located in the same file as the interface definition and the `Main` method.</span></span>
2. <span data-ttu-id="63955-191">Alkalmazza a [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) attribútumot az `IEchoContract` felületre.</span><span class="sxs-lookup"><span data-stu-id="63955-191">Apply the [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) attribute to the `IEchoContract` interface.</span></span> <span data-ttu-id="63955-192">Az attribútum megadja a szolgáltatás nevét és névterét.</span><span class="sxs-lookup"><span data-stu-id="63955-192">The attribute specifies the service name and namespace.</span></span> <span data-ttu-id="63955-193">Ezután az `EchoService` osztály a következőképp jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="63955-193">After doing so, the `EchoService` class appears as follows:</span></span>

    ```csharp
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```
3. <span data-ttu-id="63955-194">Valósítsa meg az `IEchoContract` felületen meghatározott `Echo` metódust az `EchoService` osztályban.</span><span class="sxs-lookup"><span data-stu-id="63955-194">Implement the `Echo` method defined in the `IEchoContract` interface in the `EchoService` class.</span></span>

    ```csharp
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```
4. <span data-ttu-id="63955-195">A **Build** (Létrehozás) menüben kattintson a **Build Solution** (Megoldás létrehozása) elemre az eddigi munkája pontosságának ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="63955-195">Click **Build**, then click **Build Solution** to confirm the accuracy of your work.</span></span>

### <a name="define-the-configuration-for-the-service-host"></a><span data-ttu-id="63955-196">A szolgáltatásgazda konfigurációjának meghatározása</span><span class="sxs-lookup"><span data-stu-id="63955-196">Define the configuration for the service host</span></span>

1. <span data-ttu-id="63955-197">A konfigurációs fájl nagyon hasonlít a WCF konfigurációs fájlra.</span><span class="sxs-lookup"><span data-stu-id="63955-197">The configuration file is very similar to a WCF configuration file.</span></span> <span data-ttu-id="63955-198">Ez magában foglalja a szolgáltatás nevét, végpontját (Ez azt jelenti, hogy azt a helyet, amely Azure továbbítási elérhetővé teszi az ügyfelek és a gazdáknak az egymással való kommunikációhoz) és kötését (a kommunikációhoz használt protokoll típusát).</span><span class="sxs-lookup"><span data-stu-id="63955-198">It includes the service name, endpoint (that is, the location that Azure Relay exposes for clients and hosts to communicate with each other), and the binding (the type of protocol that is used to communicate).</span></span> <span data-ttu-id="63955-199">A fő különbség az, hogy ez a konfigurált szolgáltatásvégpont egy [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) kötésre hivatkozik, amely nem része a .NET-keretrendszernek.</span><span class="sxs-lookup"><span data-stu-id="63955-199">The main difference is that this configured service endpoint refers to a [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) binding, which is not part of the .NET Framework.</span></span> <span data-ttu-id="63955-200">[NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) a szolgáltatás által meghatározott kötés egyik.</span><span class="sxs-lookup"><span data-stu-id="63955-200">[NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) is one of the bindings defined by the service.</span></span>
2. <span data-ttu-id="63955-201">A **Solution Explorerben** (Megoldáskezelőben) kattintson duplán az App.config fájlra a Visual Studio-szerkesztőben való megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="63955-201">In **Solution Explorer**, double-click the App.config file to open it in the Visual Studio editor.</span></span>
3. <span data-ttu-id="63955-202">Az `<appSettings>` elemben cserélje le a helyőrzőket a szolgáltatási névtér nevére, valamint a korábbi lépésben másolt SAS-kulcsra.</span><span class="sxs-lookup"><span data-stu-id="63955-202">In the `<appSettings>` element, replace the placeholders with the name of your service namespace, and the SAS key that you copied in an earlier step.</span></span>
4. <span data-ttu-id="63955-203">A `<system.serviceModel>` címkéken belül adjon hozzá egy `<services>` elemet.</span><span class="sxs-lookup"><span data-stu-id="63955-203">Within the `<system.serviceModel>` tags, add a `<services>` element.</span></span> <span data-ttu-id="63955-204">Több relay alkalmazásban definiálhat egy egyetlen konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="63955-204">You can define multiple relay applications in a single configuration file.</span></span> <span data-ttu-id="63955-205">Ez az oktatóanyag viszont csak egyet határoz meg.</span><span class="sxs-lookup"><span data-stu-id="63955-205">However, this tutorial defines only one.</span></span>

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```
5. <span data-ttu-id="63955-206">A `<services>` elemen belül adjon hozzá egy `<service>` elemet a szolgáltatás nevének meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="63955-206">Within the `<services>` element, add a `<service>` element to define the name of the service.</span></span>

    ```xml
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```
6. <span data-ttu-id="63955-207">A `<service>` elemen belül határozza meg a végpont szerződés helyét, valamint a végpont kötésének típusát.</span><span class="sxs-lookup"><span data-stu-id="63955-207">Within the `<service>` element, define the location of the endpoint contract, and also the type of binding for the endpoint.</span></span>

    ```xml
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    <span data-ttu-id="63955-208">A végpont meghatározza, hogy az ügyfél hol keresi majd a gazdaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="63955-208">The endpoint defines where the client will look for the host application.</span></span> <span data-ttu-id="63955-209">Később az oktatóprogram ebben a lépésben URI, amely teljesen közzéteszi a gazdagépet keresztül Azure Relay létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="63955-209">Later, the tutorial uses this step to create a URI that fully exposes the host through Azure Relay.</span></span> <span data-ttu-id="63955-210">A kötés deklarálja, hogy használjuk TCP protokollt a relay szolgáltatással való kommunikációra.</span><span class="sxs-lookup"><span data-stu-id="63955-210">The binding declares that we are using TCP as the protocol to communicate with the relay service.</span></span>
7. <span data-ttu-id="63955-211">A **Build** (Létrehozás) menüben kattintson a **Build Solution** (Megoldás létrehozása) elemre a munkája pontosságának ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="63955-211">From the **Build** menu, click **Build Solution** to confirm the accuracy of your work.</span></span>

### <a name="example"></a><span data-ttu-id="63955-212">Példa</span><span class="sxs-lookup"><span data-stu-id="63955-212">Example</span></span>

<span data-ttu-id="63955-213">A következő kód a szolgáltatási szerződés megvalósítását mutatja be.</span><span class="sxs-lookup"><span data-stu-id="63955-213">The following code shows the implementation of the service contract.</span></span>

```csharp
[ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

<span data-ttu-id="63955-214">A következő kód a szolgáltatásgazdához társított App.config fájl alapszintű formátumát mutatja be.</span><span class="sxs-lookup"><span data-stu-id="63955-214">The following code shows the basic format of the App.config file associated with the service host.</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="host-and-run-a-basic-web-service-to-register-with-the-relay-service"></a><span data-ttu-id="63955-215">Üzemeltetése és futtatása egy alapszintű webszolgáltatás a továbbítási szolgáltatás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="63955-215">Host and run a basic web service to register with the relay service</span></span>

<span data-ttu-id="63955-216">Ez a lépés ismerteti, hogyan lehet egy Azure-továbbítási szolgáltatás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="63955-216">This step describes how to run an Azure Relay service.</span></span>

### <a name="create-the-relay-credentials"></a><span data-ttu-id="63955-217">A továbbító hitelesítő adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="63955-217">Create the relay credentials</span></span>

1. <span data-ttu-id="63955-218">A `Main()` metódusban hozzon létre két változót, amelyben a konzolablakból beolvasott névtér és SAS-kulcs tárolható.</span><span class="sxs-lookup"><span data-stu-id="63955-218">In `Main()`, create two variables in which to store the namespace and the SAS key that are read from the console window.</span></span>

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    <span data-ttu-id="63955-219">A SAS-kulcs később a projekt eléréséhez használandó.</span><span class="sxs-lookup"><span data-stu-id="63955-219">The SAS key will be used later to access your project.</span></span> <span data-ttu-id="63955-220">A névteret a rendszer paraméterként átadja a `CreateServiceUri` számára szolgáltatás URI létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="63955-220">The namespace is passed as a parameter to `CreateServiceUri` to create a service URI.</span></span>
2. <span data-ttu-id="63955-221">Egy [TransportClientEndpointBehavior](/dotnet/api/microsoft.servicebus.transportclientendpointbehavior) objektum segítségével deklarálhatja, hogy SAS-kulcsot fog használni hitelesítési típusként.</span><span class="sxs-lookup"><span data-stu-id="63955-221">Using a [TransportClientEndpointBehavior](/dotnet/api/microsoft.servicebus.transportclientendpointbehavior) object, declare that you will be using a SAS key as the credential type.</span></span> <span data-ttu-id="63955-222">Vegye fel a következő kódot közvetlenül az előző lépésben felvett kód után.</span><span class="sxs-lookup"><span data-stu-id="63955-222">Add the following code directly after the code added in the last step.</span></span>

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="create-a-base-address-for-the-service"></a><span data-ttu-id="63955-223">A szolgáltatás alapszintű cím létrehozása</span><span class="sxs-lookup"><span data-stu-id="63955-223">Create a base address for the service</span></span>

<span data-ttu-id="63955-224">Az előző lépésben felvett kód után hozzon létre egy `Uri` a szolgáltatás alapszintű címéhez példánya.</span><span class="sxs-lookup"><span data-stu-id="63955-224">After the code you added in the last step, create a `Uri` instance for the base address of the service.</span></span> <span data-ttu-id="63955-225">Ez az URI megadja a Service Bus-sémát, a névteret és a szolgáltatási felület útvonalát.</span><span class="sxs-lookup"><span data-stu-id="63955-225">This URI specifies the Service Bus scheme, the namespace, and the path of the service interface.</span></span>

```csharp
Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
```

<span data-ttu-id="63955-226">Az „sb” a Service Bus-séma rövidítése, és azt jelzi, hogy a TCP protokollt használjuk.</span><span class="sxs-lookup"><span data-stu-id="63955-226">"sb" is an abbreviation for the Service Bus scheme, and indicates that we are using TCP as the protocol.</span></span> <span data-ttu-id="63955-227">Ez már korábban is jelezve volt a konfigurációs fájlban, amikor kötésként a [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) lett megadva.</span><span class="sxs-lookup"><span data-stu-id="63955-227">This was also previously indicated in the configuration file, when [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) was specified as the binding.</span></span>

<span data-ttu-id="63955-228">Az oktatóanyaghoz az URI `sb://putServiceNamespaceHere.windows.net/EchoService`.</span><span class="sxs-lookup"><span data-stu-id="63955-228">For this tutorial, the URI is `sb://putServiceNamespaceHere.windows.net/EchoService`.</span></span>

### <a name="create-and-configure-the-service-host"></a><span data-ttu-id="63955-229">Hozza létre és konfigurálja a szolgáltatásgazda</span><span class="sxs-lookup"><span data-stu-id="63955-229">Create and configure the service host</span></span>

1. <span data-ttu-id="63955-230">A csatlakozási mód beállítása legyen `AutoDetect`.</span><span class="sxs-lookup"><span data-stu-id="63955-230">Set the connectivity mode to `AutoDetect`.</span></span>

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    <span data-ttu-id="63955-231">A csatlakozási mód írja le a protokollt, a szolgáltatás használatával kommunikálnak a továbbítási szolgáltatás; HTTP vagy TCP.</span><span class="sxs-lookup"><span data-stu-id="63955-231">The connectivity mode describes the protocol the service uses to communicate with the relay service; either HTTP or TCP.</span></span> <span data-ttu-id="63955-232">Az alapértelmezett beállítás használata `AutoDetect`, a szolgáltatás megpróbál csatlakozni Azure továbbítási TCP, amennyiben az rendelkezésre áll, és a HTTP-alapú Ha TCP nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="63955-232">Using the default setting `AutoDetect`, the service attempts to connect to Azure Relay over TCP if it is available, and HTTP if TCP is not available.</span></span> <span data-ttu-id="63955-233">Vegye figyelembe, hogy ez nem azonos a szolgáltatás által az ügyfél-kommunikációhoz megadott protokollal.</span><span class="sxs-lookup"><span data-stu-id="63955-233">Note that this differs from the protocol the service specifies for client communication.</span></span> <span data-ttu-id="63955-234">Azt a protokollt az alkalmazott kötés határozza meg.</span><span class="sxs-lookup"><span data-stu-id="63955-234">That protocol is determined by the binding used.</span></span> <span data-ttu-id="63955-235">A szolgáltatás használhatja például a [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) kötést, amely megadja, hogy a végpont kommunikál az ügyfelek HTTP Protokollon keresztül.</span><span class="sxs-lookup"><span data-stu-id="63955-235">For example, a service can use the [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) binding, which specifies that its endpoint communicates with clients over HTTP.</span></span> <span data-ttu-id="63955-236">Megadhatja, hogy ugyanazt a szolgáltatást **ConnectivityMode.AutoDetect** , hogy a szolgáltatás TCP protokollon keresztül kommunikál Azure továbbító.</span><span class="sxs-lookup"><span data-stu-id="63955-236">That same service could specify **ConnectivityMode.AutoDetect** so that the service communicates with Azure Relay over TCP.</span></span>
2. <span data-ttu-id="63955-237">Hozza létre a szolgáltatás gazdáját a szakaszban korábban létrehozott URI segítségével.</span><span class="sxs-lookup"><span data-stu-id="63955-237">Create the service host, using the URI created earlier in this section.</span></span>

    ```csharp
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    <span data-ttu-id="63955-238">A szolgáltatásgazda a WCF-objektum, amely a szolgáltatást példányosítja.</span><span class="sxs-lookup"><span data-stu-id="63955-238">The service host is the WCF object that instantiates the service.</span></span> <span data-ttu-id="63955-239">Ebben a lépésben adja át számára a létrehozni kívánt szolgáltatástípust (`EchoService` típus), valamint a címet is, amelyen közzé szeretné tenni a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="63955-239">Here, you pass it the type of service you want to create (an `EchoService` type), and also to the address at which you want to expose the service.</span></span>
3. <span data-ttu-id="63955-240">A Program.cs fájl elejéhez adjon hozzá a következőkre mutató hivatkozásokat: [System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) és [Microsoft.ServiceBus.Description](/dotnet/api/microsoft.servicebus.description).</span><span class="sxs-lookup"><span data-stu-id="63955-240">At the top of the Program.cs file, add references to [System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) and [Microsoft.ServiceBus.Description](/dotnet/api/microsoft.servicebus.description).</span></span>

    ```csharp
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```
4. <span data-ttu-id="63955-241">Újra a `Main()` metódusban konfigurálja a végpontot a nyilvános hozzáférés engedélyezésére.</span><span class="sxs-lookup"><span data-stu-id="63955-241">Back in `Main()`, configure the endpoint to enable public access.</span></span>

    ```csharp
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    <span data-ttu-id="63955-242">Ez a lépés a továbbítási szolgáltatás, amely az alkalmazás nyilvánosan az ATOM-hírcsatornát a projekthez megvizsgálásával tájékoztatja.</span><span class="sxs-lookup"><span data-stu-id="63955-242">This step informs the relay service that your application can be found publicly by examining the ATOM feed for your project.</span></span> <span data-ttu-id="63955-243">Ha a **DiscoveryType** beállítása **private**, attól még egy ügyfél elérheti a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="63955-243">If you set **DiscoveryType** to **private**, a client would still be able to access the service.</span></span> <span data-ttu-id="63955-244">A szolgáltatás azonban nem jelent a továbbítási névtér kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="63955-244">However, the service would not appear when it searches the Relay namespace.</span></span> <span data-ttu-id="63955-245">Az ügyfélnek ehhez már ismernie kell a végpont elérési útját.</span><span class="sxs-lookup"><span data-stu-id="63955-245">Instead, the client would have to know the endpoint path beforehand.</span></span>
5. <span data-ttu-id="63955-246">Alkalmazza a szolgáltatás hitelesítő adatait az App.config fájlban meghatározott szolgáltatásvégpontokra:</span><span class="sxs-lookup"><span data-stu-id="63955-246">Apply the service credentials to the service endpoints defined in the App.config file:</span></span>

    ```csharp
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    <span data-ttu-id="63955-247">Ahogyan az az előző lépésben elhangzott, van mód deklarálni több szolgáltatást és végpontot a konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="63955-247">As stated in the previous step, you could have declared multiple services and endpoints in the configuration file.</span></span> <span data-ttu-id="63955-248">Ha így tett, ez a kód végighalad a konfigurációs fájlon, és megkeres minden végpontot, amelyre alkalmaznia kell a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="63955-248">If you had, this code would traverse the configuration file and search for every endpoint to which it should apply your credentials.</span></span> <span data-ttu-id="63955-249">Ebben az oktatóanyagban viszont a konfigurációs fájl csak egy végponttal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="63955-249">However, for this tutorial, the configuration file has only one endpoint.</span></span>

### <a name="open-the-service-host"></a><span data-ttu-id="63955-250">A szolgáltatásgazda megnyitása</span><span class="sxs-lookup"><span data-stu-id="63955-250">Open the service host</span></span>

1. <span data-ttu-id="63955-251">Nyissa meg a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="63955-251">Open the service.</span></span>

    ```csharp
    host.Open();
    ```
2. <span data-ttu-id="63955-252">Tájékoztassa a felhasználót arról, hogy a szolgáltatás fut, és magyarázza el, hogyan lehet leállítani.</span><span class="sxs-lookup"><span data-stu-id="63955-252">Inform the user that the service is running, and explain how to shut down the service.</span></span>

    ```csharp
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```
3. <span data-ttu-id="63955-253">Ha kész, zárja be a szolgáltatásgazdát.</span><span class="sxs-lookup"><span data-stu-id="63955-253">When finished, close the service host.</span></span>

    ```csharp
    host.Close();
    ```
4. <span data-ttu-id="63955-254">Nyomja le a **Ctrl+Shift+B** billentyűkombinációt a projekt felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="63955-254">Press **Ctrl+Shift+B** to build the project.</span></span>

### <a name="example"></a><span data-ttu-id="63955-255">Példa</span><span class="sxs-lookup"><span data-stu-id="63955-255">Example</span></span>

<span data-ttu-id="63955-256">A befejezett szolgáltatáskód hibáit a következőképpen kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="63955-256">Your completed service code should appear as follows.</span></span> <span data-ttu-id="63955-257">A kód tartalmazza a szolgáltatási szerződés és a korábbi lépések megvalósítását az oktatóanyag, és működteti a szolgáltatást egy konzolalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="63955-257">The code includes the service contract and implementation from previous steps in the tutorial, and hosts the service in a console application.</span></span>

```csharp
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         

            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create the credentials object for the endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create the service URI based on the service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create the service host reading the configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create the ServiceRegistrySettings behavior for the endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add the Relay credentials to all endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }

            // Open the service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            // Close the service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-the-service-contract"></a><span data-ttu-id="63955-258">WCF-ügyfél létrehozása a szolgáltatási szerződéshez</span><span class="sxs-lookup"><span data-stu-id="63955-258">Create a WCF client for the service contract</span></span>

<span data-ttu-id="63955-259">A következő lépés, hogy hozzon létre egy ügyfélalkalmazást és a későbbi lépésekben megvalósítandó szolgáltatási szerződés meghatározása.</span><span class="sxs-lookup"><span data-stu-id="63955-259">The next step is to create a client application and define the service contract you will implement in later steps.</span></span> <span data-ttu-id="63955-260">Vegye figyelembe, hogy sok lépés hasonlítanak szolgáltatás létrehozása lépéseire: a Szerződés meghatározása, szerkesztését az App.config fájlt, hitelesítő adatokkal való csatlakozáshoz a továbbítási szolgáltatás, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="63955-260">Note that many of these steps resemble the steps used to create a service: defining a contract, editing an App.config file, using credentials to connect to the relay service, and so on.</span></span> <span data-ttu-id="63955-261">A feladatokhoz használt kód megtalálható az eljárást követő példában.</span><span class="sxs-lookup"><span data-stu-id="63955-261">The code used for these tasks is provided in the example following the procedure.</span></span>

1. <span data-ttu-id="63955-262">Hozzon létre egy új projektet az ügyfél jelenlegi Visual Studio megoldásában az alábbiakat követve:</span><span class="sxs-lookup"><span data-stu-id="63955-262">Create a new project in the current Visual Studio solution for the client by doing the following:</span></span>

   1. <span data-ttu-id="63955-263">A Solution Explorerben (Megoldáskezelőben) a szolgáltatást tartalmazó megoldásban kattintson a jobb gombbal az aktuális megoldásra (ne a projektre), és kattintson az **Add** (Hozzáadás) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="63955-263">In Solution Explorer, in the same solution that contains the service, right-click the current solution (not the project), and click **Add**.</span></span> <span data-ttu-id="63955-264">Ezután kattintson a **New Project** (Új projekt) gombra.</span><span class="sxs-lookup"><span data-stu-id="63955-264">Then click **New Project**.</span></span>
   2. <span data-ttu-id="63955-265">Az a **új projekt hozzáadása** párbeszédpanel, kattintson a **Visual C#** (Ha **Visual C#** jelenik meg, keresse meg a **más nyelvek**) elemre, jelölje be a **Konzolalkalmazás (.NET-keretrendszer)** sablont, és adjon neki nevet **EchoClient**.</span><span class="sxs-lookup"><span data-stu-id="63955-265">In the **Add New Project** dialog box, click **Visual C#** (if **Visual C#** does not appear, look under **Other Languages**), select the **Console App (.NET Framework)** template, and name it **EchoClient**.</span></span>
   3. <span data-ttu-id="63955-266">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="63955-266">Click **OK**.</span></span>
      <br />
2. <span data-ttu-id="63955-267">A Solution Explorerben (Megoldáskezelőben) kattintson duplán az **EchoClient** projekt Program.cs fájljára a szerkesztőben való megnyitásához, ha még nincs megnyitva.</span><span class="sxs-lookup"><span data-stu-id="63955-267">In Solution Explorer, double-click the Program.cs file in the **EchoClient** project to open it in the editor, if it is not already open.</span></span>
3. <span data-ttu-id="63955-268">Módosítsa a névtér alapértelmezett `EchoClient` nevét a következőre: `Microsoft.ServiceBus.Samples`.</span><span class="sxs-lookup"><span data-stu-id="63955-268">Change the namespace name from its default name of `EchoClient` to `Microsoft.ServiceBus.Samples`.</span></span>
4. <span data-ttu-id="63955-269">Telepítse a [Service Bus NuGet-csomag](https://www.nuget.org/packages/WindowsAzure.ServiceBus): a Megoldáskezelőben kattintson a jobb gombbal a **EchoClient** projektre, és kattintson a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="63955-269">Install the [Service Bus NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus): in Solution Explorer, right-click the **EchoClient** project, and then click **Manage NuGet Packages**.</span></span> <span data-ttu-id="63955-270">Kattintson a **Browse** (Tallózás) lapra, és keressen a következőre: `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="63955-270">Click the **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="63955-271">Kattintson az **Install** (Telepítés) gombra, és fogadja el a használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="63955-271">Click **Install**, and accept the terms of use.</span></span>

    ![][3]
5. <span data-ttu-id="63955-272">Adjon hozzá egy `using` utasítást a Program.cs fájlban található [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) névtérhez.</span><span class="sxs-lookup"><span data-stu-id="63955-272">Add a `using` statement for the [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) namespace in the Program.cs file.</span></span>

    ```csharp
    using System.ServiceModel;
    ```
6. <span data-ttu-id="63955-273">Adja hozzá a szolgáltatási szerződés meghatározását a névtérhez, ahogyan az az alábbi példában látható.</span><span class="sxs-lookup"><span data-stu-id="63955-273">Add the service contract definition to the namespace, as shown in the following example.</span></span> <span data-ttu-id="63955-274">Ügyeljen arra, hogy a meghatározás azonos legyen a **Szolgáltatás** projektben használt meghatározással.</span><span class="sxs-lookup"><span data-stu-id="63955-274">Note that this definition is identical to the definition used in the **Service** project.</span></span> <span data-ttu-id="63955-275">Ezt a kódot a `Microsoft.ServiceBus.Samples` névtér tetején adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="63955-275">You should add this code at the top of the `Microsoft.ServiceBus.Samples` namespace.</span></span>

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```
7. <span data-ttu-id="63955-276">Nyomja le a **Ctrl+Shift+B** billentyűkombinációt az ügyfél felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="63955-276">Press **Ctrl+Shift+B** to build the client.</span></span>

### <a name="example"></a><span data-ttu-id="63955-277">Példa</span><span class="sxs-lookup"><span data-stu-id="63955-277">Example</span></span>

<span data-ttu-id="63955-278">A következő kódot a Program.cs fájl aktuális állapotát jeleníti meg a **EchoClient** projekt.</span><span class="sxs-lookup"><span data-stu-id="63955-278">The following code shows the current status of the Program.cs file in the **EchoClient** project.</span></span>

```csharp
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="configure-the-wcf-client"></a><span data-ttu-id="63955-279">A WCF-ügyfél konfigurálása</span><span class="sxs-lookup"><span data-stu-id="63955-279">Configure the WCF client</span></span>

<span data-ttu-id="63955-280">Ebben a lépésben létrehozza az App.config fájlt egy alapszintű ügyfélalkalmazáshoz, amely hozzáfér az ebben az oktatóanyagban korábban létrehozott szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="63955-280">In this step, you create an App.config file for a basic client application that accesses the service created previously in this tutorial.</span></span> <span data-ttu-id="63955-281">Ez az App.config fájl határozza meg a szerződést, a kötést és a végpont nevét.</span><span class="sxs-lookup"><span data-stu-id="63955-281">This App.config file defines the contract, binding, and name of the endpoint.</span></span> <span data-ttu-id="63955-282">A feladatokhoz használt kód megtalálható az eljárást követő példában.</span><span class="sxs-lookup"><span data-stu-id="63955-282">The code used for these tasks is provided in the example following the procedure.</span></span>

1. <span data-ttu-id="63955-283">A Solution Explorerben (Megoldáskezelőben) az **EchoClient** projektben kattintson duplán az **App.config** fájlra a fájl a Visual Studio-szerkesztőben való megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="63955-283">In Solution Explorer, in the **EchoClient** project, double-click **App.config** to open the file in the Visual Studio editor.</span></span>
2. <span data-ttu-id="63955-284">Az `<appSettings>` elemben cserélje le a helyőrzőket a szolgáltatási névtér nevére, valamint a korábbi lépésben másolt SAS-kulcsra.</span><span class="sxs-lookup"><span data-stu-id="63955-284">In the `<appSettings>` element, replace the placeholders with the name of your service namespace, and the SAS key that you copied in an earlier step.</span></span>
3. <span data-ttu-id="63955-285">A system.serviceModel elemen belül adjon hozzá egy `<client>` elemet.</span><span class="sxs-lookup"><span data-stu-id="63955-285">Within the system.serviceModel element, add a `<client>` element.</span></span>

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    <span data-ttu-id="63955-286">Ez a lépés deklarálja, hogy WCF stílusú ügyfélalkalmazást határoz meg.</span><span class="sxs-lookup"><span data-stu-id="63955-286">This step declares that you are defining a WCF-style client application.</span></span>
4. <span data-ttu-id="63955-287">A `client` elemen belül határozza meg a nevet, a szerződést és a kötéstípust a végponthoz.</span><span class="sxs-lookup"><span data-stu-id="63955-287">Within the `client` element, define the name, contract, and binding type for the endpoint.</span></span>

    ```xml
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    <span data-ttu-id="63955-288">Ez a lépés a végpont, a szolgáltatás, valamint azt, hogy az ügyfélalkalmazás TCP kommunikálni az Azure-továbbítási meghatározott szerződést a nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="63955-288">This step defines the name of the endpoint, the contract defined in the service, and the fact that the client application uses TCP to communicate with Azure Relay.</span></span> <span data-ttu-id="63955-289">A következő lépés a végpont neve használatával ezt a végpont-konfigurációt összekapcsolja a szolgáltatás URI-jával.</span><span class="sxs-lookup"><span data-stu-id="63955-289">The endpoint name is used in the next step to link this endpoint configuration with the service URI.</span></span>
5. <span data-ttu-id="63955-290">Kattintson a **fájl**, majd kattintson a **összes mentése**.</span><span class="sxs-lookup"><span data-stu-id="63955-290">Click **File**, then click **Save All**.</span></span>

## <a name="example"></a><span data-ttu-id="63955-291">Példa</span><span class="sxs-lookup"><span data-stu-id="63955-291">Example</span></span>

<span data-ttu-id="63955-292">A következő kód az Echo ügyfél App.config fájlját mutatja.</span><span class="sxs-lookup"><span data-stu-id="63955-292">The following code shows the App.config file for the Echo client.</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="implement-the-wcf-client"></a><span data-ttu-id="63955-293">A WCF-ügyfél megvalósítása</span><span class="sxs-lookup"><span data-stu-id="63955-293">Implement the WCF client</span></span>
<span data-ttu-id="63955-294">Ebben a lépésben megvalósít egy alapszintű ügyfélalkalmazást, amely hozzáfér az ebben az oktatóanyagban korábban létrehozott szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="63955-294">In this step, you implement a basic client application that accesses the service you created previously in this tutorial.</span></span> <span data-ttu-id="63955-295">A szolgáltatás hasonló, az ügyfél hajtja végre számos Azure-továbbítási eléréséhez ugyanazokat a műveleteket:</span><span class="sxs-lookup"><span data-stu-id="63955-295">Similar to the service, the client performs many of the same operations to access Azure Relay:</span></span>

1. <span data-ttu-id="63955-296">Beállítja a csatlakozási módot.</span><span class="sxs-lookup"><span data-stu-id="63955-296">Sets the connectivity mode.</span></span>
2. <span data-ttu-id="63955-297">Létrehozza a gazdaszolgáltatás helyét megadó URI-t.</span><span class="sxs-lookup"><span data-stu-id="63955-297">Creates the URI that locates the host service.</span></span>
3. <span data-ttu-id="63955-298">Megadja a biztonsági hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="63955-298">Defines the security credentials.</span></span>
4. <span data-ttu-id="63955-299">Alkalmazza a hitelesítő adatokat a kapcsolatra.</span><span class="sxs-lookup"><span data-stu-id="63955-299">Applies the credentials to the connection.</span></span>
5. <span data-ttu-id="63955-300">Megnyitja a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="63955-300">Opens the connection.</span></span>
6. <span data-ttu-id="63955-301">Végrehajtja az alkalmazásspecifikus feladatokat.</span><span class="sxs-lookup"><span data-stu-id="63955-301">Performs the application-specific tasks.</span></span>
7. <span data-ttu-id="63955-302">Bezárja a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="63955-302">Closes the connection.</span></span>

<span data-ttu-id="63955-303">Azonban a fő különbségek egyike, hogy az ügyfélalkalmazás egy csatorna való kapcsolódáshoz használ a továbbítási szolgáltatás, mivel a szolgáltatás használja a következőt hívja **ServiceHost**.</span><span class="sxs-lookup"><span data-stu-id="63955-303">However, one of the main differences is that the client application uses a channel to connect to the relay service, whereas the service uses a call to **ServiceHost**.</span></span> <span data-ttu-id="63955-304">A feladatokhoz használt kód megtalálható az eljárást követő példában.</span><span class="sxs-lookup"><span data-stu-id="63955-304">The code used for these tasks is provided in the example following the procedure.</span></span>

### <a name="implement-a-client-application"></a><span data-ttu-id="63955-305">Ügyfélalkalmazás megvalósítása</span><span class="sxs-lookup"><span data-stu-id="63955-305">Implement a client application</span></span>
1. <span data-ttu-id="63955-306">A csatlakozási mód beállítása legyen **AutoDetect**.</span><span class="sxs-lookup"><span data-stu-id="63955-306">Set the connectivity mode to **AutoDetect**.</span></span> <span data-ttu-id="63955-307">Adja hozzá a következő kódot az **EchoClient** alkalmazás `Main()` metódusában.</span><span class="sxs-lookup"><span data-stu-id="63955-307">Add the following code inside the `Main()` method of the **EchoClient** application.</span></span>

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```
2. <span data-ttu-id="63955-308">Határozzon meg változókat, amelyek a konzolról beolvasott szolgáltatásnévtér és SAS-kulcs értékeit tárolják.</span><span class="sxs-lookup"><span data-stu-id="63955-308">Define variables to hold the values for the service namespace, and SAS key that are read from the console.</span></span>

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```
3. <span data-ttu-id="63955-309">Az URI, amely meghatározza a helyét a gazdagépen a továbbítási projekt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="63955-309">Create the URI that defines the location of the host in your Relay project.</span></span>

    ```csharp
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```
4. <span data-ttu-id="63955-310">Hozza létre a hitelesítő objektumot a szolgáltatásnévtér végpontjához.</span><span class="sxs-lookup"><span data-stu-id="63955-310">Create the credential object for your service namespace endpoint.</span></span>

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```
5. <span data-ttu-id="63955-311">Hozza létre az App.config fájlban leírt konfigurációt betöltő csatornagyárat.</span><span class="sxs-lookup"><span data-stu-id="63955-311">Create the channel factory that loads the configuration described in the App.config file.</span></span>

    ```csharp
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    <span data-ttu-id="63955-312">A csatornagyár egy WCF-objektum, amely létrehoz egy csatornát, amelyen keresztül a szolgáltatás és az ügyfélalkalmazások kommunikálnak.</span><span class="sxs-lookup"><span data-stu-id="63955-312">A channel factory is a WCF object that creates a channel through which the service and client applications communicate.</span></span>
6. <span data-ttu-id="63955-313">A hitelesítő adatok érvényesek.</span><span class="sxs-lookup"><span data-stu-id="63955-313">Apply the credentials.</span></span>

    ```csharp
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```
7. <span data-ttu-id="63955-314">Hozza létre és nyissa meg a csatornát a szolgáltatás felé.</span><span class="sxs-lookup"><span data-stu-id="63955-314">Create and open the channel to the service.</span></span>

    ```csharp
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```
8. <span data-ttu-id="63955-315">Írja meg az Echo alapszintű felhasználói felületét és funkcióit.</span><span class="sxs-lookup"><span data-stu-id="63955-315">Write the basic user interface and functionality for the echo.</span></span>

    ```csharp
    Console.WriteLine("Enter text to echo (or [Enter] to exit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    <span data-ttu-id="63955-316">Vegye figyelembe, hogy a kód a szolgáltatás proxyjaként használja a csatornaobjektum példányát.</span><span class="sxs-lookup"><span data-stu-id="63955-316">Note that the code uses the instance of the channel object as a proxy for the service.</span></span>
9. <span data-ttu-id="63955-317">Zárja be a csatornát, és zárja be a gyárat.</span><span class="sxs-lookup"><span data-stu-id="63955-317">Close the channel, and close the factory.</span></span>

    ```csharp
    channel.Close();
    channelFactory.Close();
    ```

## <a name="example"></a><span data-ttu-id="63955-318">Példa</span><span class="sxs-lookup"><span data-stu-id="63955-318">Example</span></span>

<span data-ttu-id="63955-319">A befejezett kód kell a következőképpen jelenik meg, hozzon létre egy ügyfélalkalmazást, hogyan hívhatja meg a szolgáltatás működésére, és zárhatja be az ügyfelet a művelet hívása után megjelenítő befejeződött.</span><span class="sxs-lookup"><span data-stu-id="63955-319">Your completed code should appear as follows, showing how to create a client application, how to call the operations of the service, and how to close the client after the operation call is finished.</span></span>

```csharp
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;


            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text to echo (or [Enter] to exit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

## <a name="run-the-applications"></a><span data-ttu-id="63955-320">Az alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="63955-320">Run the applications</span></span>

1. <span data-ttu-id="63955-321">Nyomja le a **Ctrl+Shift+B** billentyűkombinációt a megoldás felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="63955-321">Press **Ctrl+Shift+B** to build the solution.</span></span> <span data-ttu-id="63955-322">Ez felépíti az előző lépésekben létrehozott ügyfélprojektet és szolgáltatásprojektet.</span><span class="sxs-lookup"><span data-stu-id="63955-322">This builds both the client project and the service project that you created in the previous steps.</span></span>
2. <span data-ttu-id="63955-323">Az ügyfélalkalmazás futtatása előtt győződjön meg arról, hogy a szolgáltatásalkalmazás fut.</span><span class="sxs-lookup"><span data-stu-id="63955-323">Before running the client application, you must make sure that the service application is running.</span></span> <span data-ttu-id="63955-324">A Visual Studióban a Solution Explorerben (Megoldáskezelőben) kattintson a jobb gombbal az **EchoService** megoldásra, majd kattintson a **Properties** (Tulajdonságok) elemre.</span><span class="sxs-lookup"><span data-stu-id="63955-324">In Solution Explorer in Visual Studio, right-click the **EchoService** solution, then click **Properties**.</span></span>
3. <span data-ttu-id="63955-325">A megoldás tulajdonságait tartalmazó párbeszédpanelen kattintson a **Startup Project** (Kezdőprojekt), majd a **Multiple startup projects** (Több kezdőprojekt) gombra.</span><span class="sxs-lookup"><span data-stu-id="63955-325">In the solution properties dialog box, click **Startup Project**, then click the **Multiple startup projects** button.</span></span> <span data-ttu-id="63955-326">Győződjön meg arról, hogy a lista első eleme az **EchoService**.</span><span class="sxs-lookup"><span data-stu-id="63955-326">Make sure **EchoService** appears first in the list.</span></span>
4. <span data-ttu-id="63955-327">Az **Action** (Művelet) mezőt állítsa az **EchoService** és az **EchoClient** projekt esetén is **Start** (Indítás) értékűre.</span><span class="sxs-lookup"><span data-stu-id="63955-327">Set the **Action** box for both the **EchoService** and **EchoClient** projects to **Start**.</span></span>

    ![][5]
5. <span data-ttu-id="63955-328">Kattintson a **Project Dependencies** (Projektfüggőségek) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="63955-328">Click **Project Dependencies**.</span></span> <span data-ttu-id="63955-329">A **Projects** (Projektek) mezőben válassza az **EchoClient** elemet.</span><span class="sxs-lookup"><span data-stu-id="63955-329">In the **Projects** box, select **EchoClient**.</span></span> <span data-ttu-id="63955-330">Győződjön meg arról, hogy a **Depends on** (Függ:) mezőben az **EchoService** be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="63955-330">In the **Depends on** box, make sure **EchoService** is checked.</span></span>

    ![][6]
6. <span data-ttu-id="63955-331">Kattintson az **OK** gombra a **Properties** (Tulajdonságok) párbeszédpanel bezárásához.</span><span class="sxs-lookup"><span data-stu-id="63955-331">Click **OK** to dismiss the **Properties** dialog.</span></span>
7. <span data-ttu-id="63955-332">Mindkét projekt futtatásához nyomja le az **F5** billentyűt.</span><span class="sxs-lookup"><span data-stu-id="63955-332">Press **F5** to run both projects.</span></span>
8. <span data-ttu-id="63955-333">Mindkét konzolablak megnyílik, és a rendszer a névtér nevének megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="63955-333">Both console windows open and prompt you for the namespace name.</span></span> <span data-ttu-id="63955-334">A szolgáltatásnak először futnia kell, ezért adja meg a névteret az **EchoService** konzolablakában, majd nyomja le az **Enter** billentyűt.</span><span class="sxs-lookup"><span data-stu-id="63955-334">The service must run first, so in the **EchoService** console window, enter the namespace and then press **Enter**.</span></span>
9. <span data-ttu-id="63955-335">Ezután a rendszer az SAS-kulcs megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="63955-335">Next, you are prompted for your SAS key.</span></span> <span data-ttu-id="63955-336">Adja meg az SAS-kulcsot, és nyomja le az ENTER billentyűt.</span><span class="sxs-lookup"><span data-stu-id="63955-336">Enter the SAS key and press ENTER.</span></span>

    <span data-ttu-id="63955-337">Az alábbiakban egy példa kimenet látható a konzolablakból.</span><span class="sxs-lookup"><span data-stu-id="63955-337">Here is example output from the console window.</span></span> <span data-ttu-id="63955-338">Vegye figyelembe, hogy az itt megadott értékek csupán példák.</span><span class="sxs-lookup"><span data-stu-id="63955-338">Note that the values provided here are for example purposes only.</span></span>

    <span data-ttu-id="63955-339">`Your Service Namespace: myNamespace` `Your SAS Key: <SAS key value>`</span><span class="sxs-lookup"><span data-stu-id="63955-339">`Your Service Namespace: myNamespace` `Your SAS Key: <SAS key value>`</span></span>

    <span data-ttu-id="63955-340">A szolgáltatásalkalmazás a konzolablakba írja a címet, amelyen figyel, ahogyan az az előbbi példában is látható.</span><span class="sxs-lookup"><span data-stu-id="63955-340">The service application prints to the console window the address on which it's listening, as seen in the following example.</span></span>

    <span data-ttu-id="63955-341">`Service address: sb://mynamespace.servicebus.windows.net/EchoService/` `Press [Enter] to exit`</span><span class="sxs-lookup"><span data-stu-id="63955-341">`Service address: sb://mynamespace.servicebus.windows.net/EchoService/` `Press [Enter] to exit`</span></span>
10. <span data-ttu-id="63955-342">Az **EchoClient** konzolablakában adja meg a korábban a szolgáltatásalkalmazáshoz megadott adatokat.</span><span class="sxs-lookup"><span data-stu-id="63955-342">In the **EchoClient** console window, enter the same information that you entered previously for the service application.</span></span> <span data-ttu-id="63955-343">Az előző lépéseket követve adja meg az ügyfélalkalmazáshoz ugyanezeket a szolgáltatásnévtér és SAS-kulcs értékeket.</span><span class="sxs-lookup"><span data-stu-id="63955-343">Follow the previous steps to enter the same service namespace and SAS key values for the client application.</span></span>
11. <span data-ttu-id="63955-344">Az értékek megadása után az ügyfél megnyit egy csatornát a szolgáltatás felé, és szövegbevitelre kéri, ahogyan az az alábbi példa konzolkimenetben látható.</span><span class="sxs-lookup"><span data-stu-id="63955-344">After entering these values, the client opens a channel to the service and prompts you to enter some text as seen in the following console output example.</span></span>

    `Enter text to echo (or [Enter] to exit):`

    <span data-ttu-id="63955-345">Adjon meg szöveget, amelyet a szolgáltatásalkalmazásba szeretne küldeni, majd nyomja le az Enter billentyűt.</span><span class="sxs-lookup"><span data-stu-id="63955-345">Enter some text to send to the service application and press Enter.</span></span> <span data-ttu-id="63955-346">Ezt a szöveget az Echo szolgáltatásműveleten keresztül küldi el a rendszer a szolgáltatásnak, és megjelenik a szolgáltatás konzolablakában, ahogyan az az alábbi példa kimenetben látható.</span><span class="sxs-lookup"><span data-stu-id="63955-346">This text is sent to the service through the Echo service operation and appears in the service console window as in the following example output.</span></span>

    `Echoing: My sample text`

    <span data-ttu-id="63955-347">Az ügyfélalkalmazás fogadja az `Echo` művelet visszaadott értékét, amely az eredeti szöveg, és a konzolablakba írja.</span><span class="sxs-lookup"><span data-stu-id="63955-347">The client application receives the return value of the `Echo` operation, which is the original text, and prints it to its console window.</span></span> <span data-ttu-id="63955-348">Az alábbi egy példa kimenet az ügyfél konzolablakából.</span><span class="sxs-lookup"><span data-stu-id="63955-348">The following is example output from the client console window.</span></span>

    `Server echoed: My sample text`
12. <span data-ttu-id="63955-349">Ezzel a módszerrel folytathatja szöveges üzenetek küldését az ügyfélről a szolgáltatásba.</span><span class="sxs-lookup"><span data-stu-id="63955-349">You can continue sending text messages from the client to the service in this manner.</span></span> <span data-ttu-id="63955-350">Ha kész van, a két alkalmazás befejezéséhez nyomja le az Enter billentyűt az ügyfél és a szolgáltatás konzolablakában is.</span><span class="sxs-lookup"><span data-stu-id="63955-350">When you are finished, press Enter in the client and service console windows to end both applications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="63955-351">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="63955-351">Next steps</span></span>

<span data-ttu-id="63955-352">Ez az oktatóanyag bemutatta, hogyan hozhat létre egy Azure-továbbítási-ügyfélalkalmazást és szolgáltatást a Service Bus WCF továbbítási képességeivel.</span><span class="sxs-lookup"><span data-stu-id="63955-352">This tutorial showed how to build an Azure Relay client application and service using the WCF Relay capabilities of Service Bus.</span></span> <span data-ttu-id="63955-353">Egy alkalmazó hasonló oktatóanyagért [Service Bus üzenetkezelés](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), lásd: [Ismerkedés a Service Bus-üzenetsorok](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span><span class="sxs-lookup"><span data-stu-id="63955-353">For a similar tutorial that uses [Service Bus Messaging](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), see [Get started with Service Bus queues](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span></span>

<span data-ttu-id="63955-354">Azure Relayjel kapcsolatos további tudnivalókért tekintse meg a következő témaköröket.</span><span class="sxs-lookup"><span data-stu-id="63955-354">To learn more about Azure Relay, see the following topics.</span></span>

* [<span data-ttu-id="63955-355">Az Azure Service Bus-architektúra áttekintése</span><span class="sxs-lookup"><span data-stu-id="63955-355">Azure Service Bus architectural overview</span></span>](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#relays)
* [<span data-ttu-id="63955-356">Az Azure Relay áttekintése</span><span class="sxs-lookup"><span data-stu-id="63955-356">Azure Relay overview</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="63955-357">A WCF továbbító szolgáltatás a .NET használatával</span><span class="sxs-lookup"><span data-stu-id="63955-357">How to use the WCF relay service with .NET</span></span>](relay-wcf-dotnet-get-started.md)

[Azure classic portal]: http://manage.windowsazure.com

[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png
