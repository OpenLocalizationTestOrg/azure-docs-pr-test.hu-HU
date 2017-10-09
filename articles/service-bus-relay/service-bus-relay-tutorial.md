---
title: "aaaAzure Service Bus WCF továbbító oktatóanyag |} Microsoft Docs"
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
ms.openlocfilehash: 78cd52ef51e9fcfcda2f13ec54bde3af50d76476
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-wcf-relay-tutorial"></a><span data-ttu-id="1d8fd-103">Azure WCF továbbító útmutató</span><span class="sxs-lookup"><span data-stu-id="1d8fd-103">Azure WCF Relay tutorial</span></span>

<span data-ttu-id="1d8fd-104">Ez az oktatóanyag leírja, hogyan toobuild egy egyszerű WCF továbbítása ügyfélalkalmazást és szolgáltatást Azure Relay használatával.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-104">This tutorial describes how toobuild a simple WCF Relay client application and service using Azure Relay.</span></span> <span data-ttu-id="1d8fd-105">Egy alkalmazó hasonló oktatóanyagért [Service Bus üzenetkezelés](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), lásd: [Ismerkedés a Service Bus-üzenetsorok](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span><span class="sxs-lookup"><span data-stu-id="1d8fd-105">For a similar tutorial that uses [Service Bus Messaging](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), see [Get started with Service Bus queues](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span></span>

<span data-ttu-id="1d8fd-106">Az oktatóanyag feldolgozása révén lehetővé teszi az hello lépéseket, amelyek a szükséges toocreate egy WCF továbbító ügyfél és -szogáltatásalkalmazás ismeretét.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-106">Working through this tutorial gives you an understanding of hello steps that are required toocreate a WCF Relay client and service application.</span></span> <span data-ttu-id="1d8fd-107">Az eredeti WCF megfelelők esetében, mint például a "szolgáltatás" olyan konstrukció, amely egy vagy több végpontot, azt mutatja, amelyek egy vagy több szolgáltatási műveletet tesz elérhetővé.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-107">Like their original WCF counterparts, a service is a construct that exposes one or more endpoints, each of which exposes one or more service operations.</span></span> <span data-ttu-id="1d8fd-108">hello a szolgáltatások végpontja megad egy címet, ahol hello szolgáltatás található, egy kötést, amely egy ügyfélnek kommunikálnia kell hello szolgáltatást, valamint egy szerződést, hello szolgáltatás tooits ügyfelek által biztosított hello funkciókat definiáló hello információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-108">hello endpoint of a service specifies an address where hello service can be found, a binding that contains hello information that a client must communicate with hello service, and a contract that defines hello functionality provided by hello service tooits clients.</span></span> <span data-ttu-id="1d8fd-109">hello a fő különbség a WCF és WCF továbbító között hello végpontra hello felhőben ahelyett, hogy helyileg a számítógépen van közzétéve.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-109">hello main difference between WCF and WCF Relay is that hello endpoint is exposed in hello cloud instead of locally on your computer.</span></span>

<span data-ttu-id="1d8fd-110">Miután hello sorozatban ebben az oktatóanyagban, hogy egy futó szolgáltatással, és egy hello szolgáltatás hello műveleteinek meghívására képes ügyféllel.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-110">After you work through hello sequence of topics in this tutorial, you will have a running service, and a client that can invoke hello operations of hello service.</span></span> <span data-ttu-id="1d8fd-111">hello első témakör ismerteti, hogyan tooset fiókot.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-111">hello first topic describes how tooset up an account.</span></span> <span data-ttu-id="1d8fd-112">hello következő lépések leírják, hogyan toodefine egy szolgáltatás, amely használ egy szerződést, hogyan tooimplement hello szolgáltatást, és hogyan tooconfigure hello szolgáltatást a kódban.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-112">hello next steps describe how toodefine a service that uses a contract, how tooimplement hello service, and how tooconfigure hello service in code.</span></span> <span data-ttu-id="1d8fd-113">Azt is leírják, hogyan toohost és Futtatás hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-113">They also describe how toohost and run hello service.</span></span> <span data-ttu-id="1d8fd-114">hello létrejött szolgáltatás próbaüzemben működik, és hello ügyfél és a szolgáltatás futtatásához a hello ugyanazon a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-114">hello service that is created is self-hosted and hello client and service run on hello same computer.</span></span> <span data-ttu-id="1d8fd-115">Hello szolgáltatást kód vagy a konfigurációs fájl segítségével konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-115">You can configure hello service by using either code or a configuration file.</span></span>

<span data-ttu-id="1d8fd-116">hello utolsó három lépés bemutatják, hogyan toocreate ügyfélalkalmazás, hello ügyfélalkalmazást, konfigurálása és létrehozása és hello funkció hello fogadó hozzáférő ügyfél használata.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-116">hello final three steps describe how toocreate a client application, configure hello client application, and create and use a client that can access hello functionality of hello host.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1d8fd-117">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1d8fd-117">Prerequisites</span></span>

<span data-ttu-id="1d8fd-118">toocomplete ebben az oktatóanyagban szüksége lesz a következő hello:</span><span class="sxs-lookup"><span data-stu-id="1d8fd-118">toocomplete this tutorial, you'll need hello following:</span></span>

* <span data-ttu-id="1d8fd-119">[Microsoft Visual Studio 2015 vagy újabb](http://visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="1d8fd-119">[Microsoft Visual Studio 2015 or higher](http://visualstudio.com).</span></span> <span data-ttu-id="1d8fd-120">Ez az oktatóanyag a Visual Studio 2017 használja.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-120">This tutorial uses Visual Studio 2017.</span></span>
* <span data-ttu-id="1d8fd-121">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-121">An active Azure account.</span></span> <span data-ttu-id="1d8fd-122">Ha még nincs fiókja, néhány perc alatt létrehozhat egy ingyenes fiókot.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-122">If you don't have one, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="1d8fd-123">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="1d8fd-123">For details, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="create-a-service-namespace"></a><span data-ttu-id="1d8fd-124">Szolgáltatásnévtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d8fd-124">Create a service namespace</span></span>

<span data-ttu-id="1d8fd-125">hello első lépése a rendszer toocreate egy névtér, tooobtain egy [közös hozzáférésű Jogosultságkód (SAS)](../service-bus-messaging/service-bus-sas.md) kulcs.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-125">hello first step is toocreate a namespace, and tooobtain a [Shared Access Signature (SAS)](../service-bus-messaging/service-bus-sas.md) key.</span></span> <span data-ttu-id="1d8fd-126">A névtér egy biztosít hello továbbítási szolgáltatás keresztül közzétett minden alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-126">A namespace provides an application boundary for each application exposed through hello relay service.</span></span> <span data-ttu-id="1d8fd-127">Rendszer SAS-kulcs hello rendszer automatikusan előállítja a szolgáltatásnévtér létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-127">A SAS key is automatically generated by hello system when a service namespace is created.</span></span> <span data-ttu-id="1d8fd-128">hello kombinációja szolgáltatásnévtér és SAS-kulcs Azure tooauthenticate hozzáférés tooan alkalmazás hello hitelesítő adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-128">hello combination of service namespace and SAS key provides hello credentials for Azure tooauthenticate access tooan application.</span></span> <span data-ttu-id="1d8fd-129">Hajtsa végre a hello [utasításokat itt](relay-create-namespace-portal.md) toocreate továbbítási névtér.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-129">Follow hello [instructions here](relay-create-namespace-portal.md) toocreate a Relay namespace.</span></span>

## <a name="define-a-wcf-service-contract"></a><span data-ttu-id="1d8fd-130">A WCF szolgáltatási szerződés megadása</span><span class="sxs-lookup"><span data-stu-id="1d8fd-130">Define a WCF service contract</span></span>

<span data-ttu-id="1d8fd-131">hello szolgáltatási szerződés Megadja, milyen műveleteket (hello webszolgáltatás-terminológia a metódusokhoz és függvényeket) hello szolgáltatást támogatja.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-131">hello service contract specifies what operations (hello web service terminology for methods or functions) hello service supports.</span></span> <span data-ttu-id="1d8fd-132">A szerződések a C++, a C# vagy a Visual Basic felület meghatározásával jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-132">Contracts are created by defining a C++, C#, or Visual Basic interface.</span></span> <span data-ttu-id="1d8fd-133">Hello felület minden metódusa tooa konkrét szolgáltatási műveletnek felel meg.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-133">Each method in hello interface corresponds tooa specific service operation.</span></span> <span data-ttu-id="1d8fd-134">Minden egyes csatolónak támogatnia kell a hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribútum tooit alkalmazott, és minden műveletnek rendelkeznie hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribútuma tooit.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-134">Each interface must have hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribute applied tooit, and each operation must have hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribute applied tooit.</span></span> <span data-ttu-id="1d8fd-135">Ha a metódus egy illesztőfelületben, amely rendelkezik hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribútumnak nincs hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribútummal, hogy metódus nem lesz közzétéve.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-135">If a method in an interface that has hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribute does not have hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribute, that method is not exposed.</span></span> <span data-ttu-id="1d8fd-136">a feladatok kódja hello hello eljárást követő hello példában valósul meg.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-136">hello code for these tasks is provided in hello example following hello procedure.</span></span> <span data-ttu-id="1d8fd-137">Szerződések és szolgáltatások nagyobb tárgyalását lásd: [tervezése és megvalósítása szolgáltatások](https://msdn.microsoft.com/library/ms729746.aspx) a hello WCF-dokumentáció.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-137">For a larger discussion of contracts and services, see [Designing and Implementing Services](https://msdn.microsoft.com/library/ms729746.aspx) in hello WCF documentation.</span></span>

### <a name="create-a-relay-contract-with-an-interface"></a><span data-ttu-id="1d8fd-138">Továbbító szerződés létrehozása felülettel</span><span class="sxs-lookup"><span data-stu-id="1d8fd-138">Create a relay contract with an interface</span></span>

1. <span data-ttu-id="1d8fd-139">Nyissa meg a Visual Studio rendszergazdaként kattintson a jobb gombbal a programra hello hello **Start** menüben, majd válassza **Futtatás rendszergazdaként**.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-139">Open Visual Studio as an administrator by right-clicking hello program in hello **Start** menu and selecting **Run as administrator**.</span></span>
2. <span data-ttu-id="1d8fd-140">Hozzon létre új egy új konzolalkalmazás-projektet.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-140">Create a new console application project.</span></span> <span data-ttu-id="1d8fd-141">Kattintson a hello **fájl** menüre, majd válassza **új**, majd kattintson a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-141">Click hello **File** menu and select **New**, then click **Project**.</span></span> <span data-ttu-id="1d8fd-142">A hello **új projekt** párbeszédpanel, kattintson a **Visual C#** (Ha **Visual C#** jelenik meg, keresse meg a **más nyelvek**).</span><span class="sxs-lookup"><span data-stu-id="1d8fd-142">In hello **New Project** dialog, click **Visual C#** (if **Visual C#** does not appear, look under **Other Languages**).</span></span> <span data-ttu-id="1d8fd-143">Kattintson a hello **Konzolalkalmazás (.NET-keretrendszer)** sablont, és adjon neki nevet **EchoService**.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-143">Click hello **Console App (.NET Framework)** template, and name it **EchoService**.</span></span> <span data-ttu-id="1d8fd-144">Kattintson a **OK** toocreate hello projekt.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-144">Click **OK** toocreate hello project.</span></span>

    ![][2]

3. <span data-ttu-id="1d8fd-145">Hello Service Bus NuGet-csomagot telepítse.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-145">Install hello Service Bus NuGet package.</span></span> <span data-ttu-id="1d8fd-146">Ez a csomag automatikusan hozzáadja a hivatkozások toohello Service Bus-könyvtárakhoz, valamint a hello WCF **System.ServiceModel**.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-146">This package automatically adds references toohello Service Bus libraries, as well as hello WCF **System.ServiceModel**.</span></span> <span data-ttu-id="1d8fd-147">[System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) hello névtér, amely lehetővé teszi tooprogrammatically hozzáférés hello a WCF alapszintű szolgáltatásaihoz.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-147">[System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) is hello namespace that enables you tooprogrammatically access hello basic features of WCF.</span></span> <span data-ttu-id="1d8fd-148">A Service Bus hello objektumok és attribútumok WCF toodefine szolgáltatási szerződések használ.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-148">Service Bus uses many of hello objects and attributes of WCF toodefine service contracts.</span></span>

    <span data-ttu-id="1d8fd-149">A Megoldáskezelőben kattintson a jobb gombbal a hello projektet, és kattintson **NuGet-csomagok kezelése...** . Kattintson a hello **Tallózás** lapra, és keresse meg `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-149">In Solution Explorer, right-click hello project, and then click **Manage NuGet Packages...**. Click hello **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="1d8fd-150">Győződjön meg arról, hogy hello projektnevet kiválasztott hello **verzió(k)** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-150">Ensure that hello project name is selected in hello **Version(s)** box.</span></span> <span data-ttu-id="1d8fd-151">Kattintson a **telepítése**, és el kell fogadnia a használati feltételek hello.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-151">Click **Install**, and accept hello terms of use.</span></span>

    ![][3]
4. <span data-ttu-id="1d8fd-152">A Megoldáskezelőben kattintson duplán a hello Program.cs fájl tooopen nyissa meg a legyen hello szerkesztő, ha még nincs.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-152">In Solution Explorer, double-click hello Program.cs file tooopen it in hello editor, if it is not already open.</span></span>
5. <span data-ttu-id="1d8fd-153">Adja hozzá a következő hello hello tetején hello fájl található utasítások segítségével:</span><span class="sxs-lookup"><span data-stu-id="1d8fd-153">Add hello following using statements at hello top of hello file:</span></span>

    ```csharp
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```
6. <span data-ttu-id="1d8fd-154">Hello alapértelmezett nevét a névtér nevének módosítása **EchoService** túl**Microsoft.ServiceBus.Samples**.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-154">Change hello namespace name from its default name of **EchoService** too**Microsoft.ServiceBus.Samples**.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="1d8fd-155">Ez az oktatóanyag hello C# névteret használja **Microsoft.ServiceBus.Samples**, amely adategyezmény-alapú hello hello névtere felügyelt hello hello konfigurációs fájlban használt típusának [hello WCF ügyfél konfigurálása](#configure-the-wcf-client) lépés.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-155">This tutorial uses hello C# namespace **Microsoft.ServiceBus.Samples**, which is hello namespace of hello contract-based managed type that is used in hello configuration file in hello [Configure hello WCF client](#configure-the-wcf-client) step.</span></span> <span data-ttu-id="1d8fd-156">Megadhat bármilyen névteret létrehozásakor ez a minta; hello az oktatóanyag azonban nem működik Ha ezután nem módosítja hello névterek hello szerződés és szolgáltatás ennek megfelelően hello alkalmazás konfigurációs fájljában.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-156">You can specify any namespace you want when you build this sample; however, hello tutorial will not work unless you then modify hello namespaces of hello contract and service accordingly, in hello application configuration file.</span></span> <span data-ttu-id="1d8fd-157">hello névtér van megadva a hello App.config fájl kell lennie, a C# fájlokban megadott névtérrel, hello hello azonos.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-157">hello namespace specified in hello App.config file must be hello same as hello namespace specified in your C# files.</span></span>
   >
   >
7. <span data-ttu-id="1d8fd-158">Hello után közvetlenül `Microsoft.ServiceBus.Samples` névtér-deklaráció hello névtérben, adja meg egy új nevű felületet, de `IEchoContract` és hello alkalmazása `ServiceContractAttribute` attribútum toohello felület értékű névtér `http://samples.microsoft.com/ServiceModel/Relay/`.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-158">Directly after hello `Microsoft.ServiceBus.Samples` namespace declaration, but within hello namespace, define a new interface named `IEchoContract` and apply hello `ServiceContractAttribute` attribute toohello interface with a namespace value of `http://samples.microsoft.com/ServiceModel/Relay/`.</span></span> <span data-ttu-id="1d8fd-159">hello névtér értéke különbözik hello hatókör a kód egészében használt hello névtér.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-159">hello namespace value differs from hello namespace that you use throughout hello scope of your code.</span></span> <span data-ttu-id="1d8fd-160">Ehelyett hello névtér értéke lesz egy egyedi azonosítót ehhez a szerződéshez.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-160">Instead, hello namespace value is used as a unique identifier for this contract.</span></span> <span data-ttu-id="1d8fd-161">Hello névtér explicit megadásával megakadályozza, hogy az hello alapértelmezett névtér érték kerüljön toohello szerződés neve.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-161">Specifying hello namespace explicitly prevents hello default namespace value from being added toohello contract name.</span></span> <span data-ttu-id="1d8fd-162">Illessze be a kódot hello névtér-deklaráció után a következő hello:</span><span class="sxs-lookup"><span data-stu-id="1d8fd-162">Paste hello following code after hello namespace declaration:</span></span>

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

   > [!NOTE]
   > <span data-ttu-id="1d8fd-163">Hello szolgáltatási szerződés névtere általában egy elnevezési sémát, amely tartalmazza a verzióinformációkat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-163">Typically, hello service contract namespace contains a naming scheme that includes version information.</span></span> <span data-ttu-id="1d8fd-164">Szolgáltatások tooisolate lényegesen módosul verzióinformációk szerepelnek hello szolgáltatási szerződés névtere lehetővé teszi egy új szolgáltatási szerződés új névtérrel és egy új végponton megjelenítése révén.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-164">Including version information in hello service contract namespace enables services tooisolate major changes by defining a new service contract with a new namespace and exposing it on a new endpoint.</span></span> <span data-ttu-id="1d8fd-165">Ezen a módon kikapcsolja az ügyfelek továbbra is toouse hello régi szolgáltatási szerződést anélkül, hogy a frissített toobe.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-165">In this manner, clients can continue toouse hello old service contract without having toobe updated.</span></span> <span data-ttu-id="1d8fd-166">A verzióinformációk dátumot vagy buildszámot tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-166">Version information can consist of a date or a build number.</span></span> <span data-ttu-id="1d8fd-167">További információ: [Service Versioning](http://go.microsoft.com/fwlink/?LinkID=180498) (Szolgáltatás verziószámozása).</span><span class="sxs-lookup"><span data-stu-id="1d8fd-167">For more information, see [Service Versioning](http://go.microsoft.com/fwlink/?LinkID=180498).</span></span> <span data-ttu-id="1d8fd-168">Ez az oktatóanyag hello céljából hello elnevezési sémája hello szolgáltatási szerződés névtere nem tartalmaz fájlverzió-információkat.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-168">For hello purposes of this tutorial, hello naming scheme of hello service contract namespace does not contain version information.</span></span>
   >
   >
8. <span data-ttu-id="1d8fd-169">Hello belül `IEchoContract` csatoló, deklaráljon egy metódust hello egyetlen műveletben hello `IEchoContract` szerződés tesz elérhetővé a hello felületet, és alkalmazza a hello `OperationContractAttribute` attribútum toohello módszert, amelyet tooexpose részeként hello nyilvános WCF továbbító szerződés, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="1d8fd-169">Within hello `IEchoContract` interface, declare a method for hello single operation hello `IEchoContract` contract exposes in hello interface and apply hello `OperationContractAttribute` attribute toohello method that you want tooexpose as part of hello public WCF Relay contract, as follows:</span></span>

    ```csharp
    [OperationContract]
    string Echo(string text);
    ```
9. <span data-ttu-id="1d8fd-170">Hello után közvetlenül `IEchoContract` felület, deklaráljon egy csatornát, amely mindkét örökli `IEchoContract` és is toohello `IClientChannel` csatoló, ahogy az itt látható:</span><span class="sxs-lookup"><span data-stu-id="1d8fd-170">Directly after hello `IEchoContract` interface definition, declare a channel that inherits from both `IEchoContract` and also toohello `IClientChannel` interface, as shown here:</span></span>

    ```csharp
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    <span data-ttu-id="1d8fd-171">A csatorna egy olyan hello WCF-objektum, amelyen keresztül a hello állomás és az ügyfélszámítógépek át adatokat tooeach más.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-171">A channel is hello WCF object through which hello host and client pass information tooeach other.</span></span> <span data-ttu-id="1d8fd-172">Később hello csatorna tooecho adatainak hello két alkalmazás közötti kódot ír majd.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-172">Later, you will write code against hello channel tooecho information between hello two applications.</span></span>
10. <span data-ttu-id="1d8fd-173">A hello **Build** menüben kattintson a **megoldás fordítása** vagy nyomja le az ENTER **Ctrl + Shift + B** eddigi munkája pontosságát tooconfirm hello.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-173">From hello **Build** menu, click **Build Solution** or press **Ctrl+Shift+B** tooconfirm hello accuracy of your work so far.</span></span>

### <a name="example"></a><span data-ttu-id="1d8fd-174">Példa</span><span class="sxs-lookup"><span data-stu-id="1d8fd-174">Example</span></span>

<span data-ttu-id="1d8fd-175">a következő kód hello egy WCF továbbító szerződést meghatározó alapszintű felületet jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-175">hello following code shows a basic interface that defines a WCF Relay contract.</span></span>

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

<span data-ttu-id="1d8fd-176">Most, hogy hello kapcsolat jön létre, hello felületet is létrehozható.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-176">Now that hello interface is created, you can implement hello interface.</span></span>

## <a name="implement-hello-wcf-contract"></a><span data-ttu-id="1d8fd-177">Hello-WCF szerződés megvalósítása</span><span class="sxs-lookup"><span data-stu-id="1d8fd-177">Implement hello WCF contract</span></span>

<span data-ttu-id="1d8fd-178">Egy Azure relay létrehozásához, először létre kell hoznia hello szerződést, amelyet egy felület használatával.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-178">Creating an Azure relay requires that you first create hello contract, which is defined by using an interface.</span></span> <span data-ttu-id="1d8fd-179">Hello felület létrehozásával kapcsolatos további információkért lásd: hello előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-179">For more information about creating hello interface, see hello previous step.</span></span> <span data-ttu-id="1d8fd-180">hello a következő lépésre tooimplement hello felületet.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-180">hello next step is tooimplement hello interface.</span></span> <span data-ttu-id="1d8fd-181">Ez magában foglalja a nevű osztály létrehozása `EchoService` , amely megvalósítja a felhasználó által definiált hello `IEchoContract` felületet.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-181">This involves creating a class named `EchoService` that implements hello user-defined `IEchoContract` interface.</span></span> <span data-ttu-id="1d8fd-182">Hello felület megvalósítása után egy App.config konfigurációs fájl segítségével hello felület majd konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-182">After you implement hello interface, you then configure hello interface using an App.config configuration file.</span></span> <span data-ttu-id="1d8fd-183">hello konfigurációs fájl hello alkalmazások, például hello neve hello szolgáltatást, hello hello szerződés és a protokoll, amely hello relay szolgáltatással használt toocommunicate hello típusú szükséges információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-183">hello configuration file contains necessary information for hello application, such as hello name of hello service, hello name of hello contract, and hello type of protocol that is used toocommunicate with hello relay service.</span></span> <span data-ttu-id="1d8fd-184">a feladatokhoz használt hello kód hello eljárást követő hello példában valósul meg.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-184">hello code used for these tasks is provided in hello example following hello procedure.</span></span> <span data-ttu-id="1d8fd-185">Hogyan tooimplement a szolgáltatási szerződés kapcsolatos további általános tárgyalását lásd: [szolgáltatási szerződések megvalósítása](https://msdn.microsoft.com/library/ms733764.aspx) a hello WCF-dokumentáció.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-185">For a more general discussion about how tooimplement a service contract, see [Implementing Service Contracts](https://msdn.microsoft.com/library/ms733764.aspx) in hello WCF documentation.</span></span>

1. <span data-ttu-id="1d8fd-186">Hozzon létre egy új osztályt `EchoService` hello hello meghatározása után közvetlenül `IEchoContract` felületet.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-186">Create a new class named `EchoService` directly after hello definition of hello `IEchoContract` interface.</span></span> <span data-ttu-id="1d8fd-187">Hello `EchoService` osztály megvalósít hello `IEchoContract` felületet.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-187">hello `EchoService` class implements hello `IEchoContract` interface.</span></span>

    ```csharp
    class EchoService : IEchoContract
    {
    }
    ```

    <span data-ttu-id="1d8fd-188">Hasonló tooother interfészes megvalósítások hello definition Megvalósíthat egy másik fájlban.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-188">Similar tooother interface implementations, you can implement hello definition in a different file.</span></span> <span data-ttu-id="1d8fd-189">Azonban ebben az oktatóanyagban hello megvalósítási található ugyanazon fájl hello felületdefiníció és hello hello `Main` metódust.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-189">However, for this tutorial, hello implementation is located in hello same file as hello interface definition and hello `Main` method.</span></span>
2. <span data-ttu-id="1d8fd-190">Hello alkalmazása [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) toohello attribútum `IEchoContract` felületet.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-190">Apply hello [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) attribute toohello `IEchoContract` interface.</span></span> <span data-ttu-id="1d8fd-191">hello attribútum határozza meg, hello szolgáltatás nevét és névterét.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-191">hello attribute specifies hello service name and namespace.</span></span> <span data-ttu-id="1d8fd-192">Miután ezt megtette, hello `EchoService` osztály a következőképp jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="1d8fd-192">After doing so, hello `EchoService` class appears as follows:</span></span>

    ```csharp
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```
3. <span data-ttu-id="1d8fd-193">Alkalmazzon hello `Echo` szereplő hello `IEchoContract` hello felülettel `EchoService` osztály.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-193">Implement hello `Echo` method defined in hello `IEchoContract` interface in hello `EchoService` class.</span></span>

    ```csharp
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```
4. <span data-ttu-id="1d8fd-194">Kattintson a **Build**, majd kattintson a **megoldás fordítása** tooconfirm hello munkája pontosságát.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-194">Click **Build**, then click **Build Solution** tooconfirm hello accuracy of your work.</span></span>

### <a name="define-hello-configuration-for-hello-service-host"></a><span data-ttu-id="1d8fd-195">Hello hello szolgáltatásgazda konfigurációjának meghatározása</span><span class="sxs-lookup"><span data-stu-id="1d8fd-195">Define hello configuration for hello service host</span></span>

1. <span data-ttu-id="1d8fd-196">hello konfigurációs fájl nagyon hasonló tooa WCF konfigurációs fájlra.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-196">hello configuration file is very similar tooa WCF configuration file.</span></span> <span data-ttu-id="1d8fd-197">Hello szolgáltatás nevét, végpontját (azaz hello helyet, amely Azure továbbítási elérhetővé teszi az ügyfelek és -állomások toocommunicate egymással) tartalmaz, és hello kötés (hello használt toocommunicate van protokoll típusát).</span><span class="sxs-lookup"><span data-stu-id="1d8fd-197">It includes hello service name, endpoint (that is, hello location that Azure Relay exposes for clients and hosts toocommunicate with each other), and hello binding (hello type of protocol that is used toocommunicate).</span></span> <span data-ttu-id="1d8fd-198">hello fő különbség az, hogy a konfigurált szolgáltatásvégpont hivatkozik tooa [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) kötést, amely nem része a .NET-keretrendszer hello.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-198">hello main difference is that this configured service endpoint refers tooa [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) binding, which is not part of hello .NET Framework.</span></span> <span data-ttu-id="1d8fd-199">[NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) hello szolgáltatás által meghatározott hello kötés egyik.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-199">[NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) is one of hello bindings defined by hello service.</span></span>
2. <span data-ttu-id="1d8fd-200">A **Megoldáskezelőben**, kattintson duplán a hello App.config fájl tooopen azt hello Visual Studio szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-200">In **Solution Explorer**, double-click hello App.config file tooopen it in hello Visual Studio editor.</span></span>
3. <span data-ttu-id="1d8fd-201">A hello `<appSettings>` elem, hello helyőrzőket cserélje le a szolgáltatásnévtér hello nevét, és a korábbi lépésben másolt SAS-kulcs hello.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-201">In hello `<appSettings>` element, replace hello placeholders with hello name of your service namespace, and hello SAS key that you copied in an earlier step.</span></span>
4. <span data-ttu-id="1d8fd-202">Hello belül `<system.serviceModel>` címkék, vegye fel a `<services>` elemet.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-202">Within hello `<system.serviceModel>` tags, add a `<services>` element.</span></span> <span data-ttu-id="1d8fd-203">Több relay alkalmazásban definiálhat egy egyetlen konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-203">You can define multiple relay applications in a single configuration file.</span></span> <span data-ttu-id="1d8fd-204">Ez az oktatóanyag viszont csak egyet határoz meg.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-204">However, this tutorial defines only one.</span></span>

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```
5. <span data-ttu-id="1d8fd-205">Hello belül `<services>` elemet, adja hozzá a `<service>` elem toodefine hello hello szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-205">Within hello `<services>` element, add a `<service>` element toodefine hello name of hello service.</span></span>

    ```xml
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```
6. <span data-ttu-id="1d8fd-206">Hello belül `<service>` elem, hello hello végpont szerződés helyét, és megadása is hello hello végpont kötésének típusát.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-206">Within hello `<service>` element, define hello location of hello endpoint contract, and also hello type of binding for hello endpoint.</span></span>

    ```xml
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    <span data-ttu-id="1d8fd-207">hello végpont határozza meg, ahol hello ügyfél hello gazdaalkalmazást keresi.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-207">hello endpoint defines where hello client will look for hello host application.</span></span> <span data-ttu-id="1d8fd-208">Később hello oktatóanyag használja ezt a lépést toocreate URI, amely teljesen közzéteszi az Azure-továbbítási hello gazdagéppel.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-208">Later, hello tutorial uses this step toocreate a URI that fully exposes hello host through Azure Relay.</span></span> <span data-ttu-id="1d8fd-209">hello kötés deklarálja, hogy a TCP használjuk, hello protokoll toocommunicate hello relay szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-209">hello binding declares that we are using TCP as hello protocol toocommunicate with hello relay service.</span></span>
7. <span data-ttu-id="1d8fd-210">A hello **Build** menüben kattintson **megoldás fordítása** tooconfirm hello munkája pontosságát.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-210">From hello **Build** menu, click **Build Solution** tooconfirm hello accuracy of your work.</span></span>

### <a name="example"></a><span data-ttu-id="1d8fd-211">Példa</span><span class="sxs-lookup"><span data-stu-id="1d8fd-211">Example</span></span>

<span data-ttu-id="1d8fd-212">hello következő kód bemutatja hello hello szolgáltatási szerződés megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-212">hello following code shows hello implementation of hello service contract.</span></span>

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

<span data-ttu-id="1d8fd-213">hello következő kód bemutatja hello hello szolgáltatásgazda társított hello App.config fájl alapszintű formátumát.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-213">hello following code shows hello basic format of hello App.config file associated with hello service host.</span></span>

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

## <a name="host-and-run-a-basic-web-service-tooregister-with-hello-relay-service"></a><span data-ttu-id="1d8fd-214">Üzemeltetése és futtatása egy alapszintű webes szolgáltatás tooregister hello relay szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="1d8fd-214">Host and run a basic web service tooregister with hello relay service</span></span>

<span data-ttu-id="1d8fd-215">Ez a lépés ismerteti, hogyan toorun egy Azure továbbítása szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-215">This step describes how toorun an Azure Relay service.</span></span>

### <a name="create-hello-relay-credentials"></a><span data-ttu-id="1d8fd-216">Hello továbbítási hitelesítő adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d8fd-216">Create hello relay credentials</span></span>

1. <span data-ttu-id="1d8fd-217">A `Main()`, hozzon létre két változót mely toostore hello névtér és SAS-kulcs hello konzolablakból olvasható hello.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-217">In `Main()`, create two variables in which toostore hello namespace and hello SAS key that are read from hello console window.</span></span>

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    <span data-ttu-id="1d8fd-218">hello SAS-kulcsot fogja használt újabb tooaccess a projekthez.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-218">hello SAS key will be used later tooaccess your project.</span></span> <span data-ttu-id="1d8fd-219">hello névtér túl átadása paraméterként`CreateServiceUri` toocreate szolgáltatás URI.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-219">hello namespace is passed as a parameter too`CreateServiceUri` toocreate a service URI.</span></span>
2. <span data-ttu-id="1d8fd-220">Használja a [TransportClientEndpointBehavior](/dotnet/api/microsoft.servicebus.transportclientendpointbehavior) objektumazonosító, deklarálja, hogy fog használni az SAS-kulcsot, hello hitelesítőadat-típus.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-220">Using a [TransportClientEndpointBehavior](/dotnet/api/microsoft.servicebus.transportclientendpointbehavior) object, declare that you will be using a SAS key as hello credential type.</span></span> <span data-ttu-id="1d8fd-221">Adja hozzá a következő kódot közvetlenül az hello előző lépésben felvett hello kód után hello.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-221">Add hello following code directly after hello code added in hello last step.</span></span>

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="create-a-base-address-for-hello-service"></a><span data-ttu-id="1d8fd-222">Hello szolgáltatás alapszintű cím létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d8fd-222">Create a base address for hello service</span></span>

<span data-ttu-id="1d8fd-223">Hello hello előző lépésben felvett kód, után hozzon létre egy `Uri` hello szolgáltatás alapszintű címéhez hello példányt.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-223">After hello code you added in hello last step, create a `Uri` instance for hello base address of hello service.</span></span> <span data-ttu-id="1d8fd-224">Ez az URI megadja hello Service Bus-sémát, hello névtér és hello szolgáltatásfelület hello elérési útját.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-224">This URI specifies hello Service Bus scheme, hello namespace, and hello path of hello service interface.</span></span>

```csharp
Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
```

<span data-ttu-id="1d8fd-225">"az sb" hello Service Bus-séma rövidítése, és azt jelzi, hogy használjuk TCP hello protokoll.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-225">"sb" is an abbreviation for hello Service Bus scheme, and indicates that we are using TCP as hello protocol.</span></span> <span data-ttu-id="1d8fd-226">Ez már korábban is jelezve hello konfigurációs fájlban, amikor [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) hello kötésként lett megadva.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-226">This was also previously indicated in hello configuration file, when [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) was specified as hello binding.</span></span>

<span data-ttu-id="1d8fd-227">Ebben az oktatóanyagban hello URI van `sb://putServiceNamespaceHere.windows.net/EchoService`.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-227">For this tutorial, hello URI is `sb://putServiceNamespaceHere.windows.net/EchoService`.</span></span>

### <a name="create-and-configure-hello-service-host"></a><span data-ttu-id="1d8fd-228">Hozzon létre és hello szolgáltatásgazda konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1d8fd-228">Create and configure hello service host</span></span>

1. <span data-ttu-id="1d8fd-229">Hello csatlakozási mód beállítása túl`AutoDetect`.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-229">Set hello connectivity mode too`AutoDetect`.</span></span>

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    <span data-ttu-id="1d8fd-230">hello csatlakozási mód írja le hello protokoll hello szolgáltatás által használt toocommunicate hello továbbítási szolgáltatás; HTTP vagy TCP.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-230">hello connectivity mode describes hello protocol hello service uses toocommunicate with hello relay service; either HTTP or TCP.</span></span> <span data-ttu-id="1d8fd-231">Hello alapértelmezett beállítás használata `AutoDetect`, hello szolgáltatás megkísérli tooconnect tooAzure továbbítási TCP, amennyiben az rendelkezésre áll, és a HTTP-alapú, ha TCP nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-231">Using hello default setting `AutoDetect`, hello service attempts tooconnect tooAzure Relay over TCP if it is available, and HTTP if TCP is not available.</span></span> <span data-ttu-id="1d8fd-232">Vegye figyelembe, hogy ez eltér hello protokoll hello szolgáltatást határozza meg az ügyfél-kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-232">Note that this differs from hello protocol hello service specifies for client communication.</span></span> <span data-ttu-id="1d8fd-233">Adott protokollhoz használt hello kötés határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-233">That protocol is determined by hello binding used.</span></span> <span data-ttu-id="1d8fd-234">Például a szolgáltatás használhatja hello [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) kötést, amely megadja, hogy a végpont kommunikál az ügyfelek HTTP Protokollon keresztül.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-234">For example, a service can use hello [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) binding, which specifies that its endpoint communicates with clients over HTTP.</span></span> <span data-ttu-id="1d8fd-235">Megadhatja, hogy ugyanazt a szolgáltatást **ConnectivityMode.AutoDetect** , hogy hello szolgáltatás TCP protokollon keresztül kommunikál Azure továbbító.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-235">That same service could specify **ConnectivityMode.AutoDetect** so that hello service communicates with Azure Relay over TCP.</span></span>
2. <span data-ttu-id="1d8fd-236">Hozzon létre hello szolgáltatásgazda hello ebben a szakaszban korábban létrehozott URI segítségével.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-236">Create hello service host, using hello URI created earlier in this section.</span></span>

    ```csharp
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    <span data-ttu-id="1d8fd-237">hello szolgáltatás állomás hello WCF-objektum, amely példányosítja hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-237">hello service host is hello WCF object that instantiates hello service.</span></span> <span data-ttu-id="1d8fd-238">Itt, át kell neki adni hello típus szolgáltatás kívánt toocreate (egy `EchoService` típus), és az is, ahol szeretné tooexpose hello szolgáltatás toohello cím.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-238">Here, you pass it hello type of service you want toocreate (an `EchoService` type), and also toohello address at which you want tooexpose hello service.</span></span>
3. <span data-ttu-id="1d8fd-239">A Program.cs fájl hello hello tetején adja hozzá hivatkozásokat túl[System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) és [Microsoft.ServiceBus.Description](/dotnet/api/microsoft.servicebus.description).</span><span class="sxs-lookup"><span data-stu-id="1d8fd-239">At hello top of hello Program.cs file, add references too[System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) and [Microsoft.ServiceBus.Description](/dotnet/api/microsoft.servicebus.description).</span></span>

    ```csharp
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```
4. <span data-ttu-id="1d8fd-240">Vissza a `Main()`, hello végpont tooenable nyilvános hozzáférés konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-240">Back in `Main()`, configure hello endpoint tooenable public access.</span></span>

    ```csharp
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    <span data-ttu-id="1d8fd-241">Ez a lépés tájékoztatja, hogy az alkalmazás nyilvánosan projektjéhez tartozó ATOM-hírcsatorna hello megvizsgálásával hello továbbítási szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-241">This step informs hello relay service that your application can be found publicly by examining hello ATOM feed for your project.</span></span> <span data-ttu-id="1d8fd-242">Ha **DiscoveryType** túl**titkos**, egy ügyfél továbbra is lenne képes tooaccess hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-242">If you set **DiscoveryType** too**private**, a client would still be able tooaccess hello service.</span></span> <span data-ttu-id="1d8fd-243">Hello szolgáltatás azonban nem jelent hello továbbítási névtér kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-243">However, hello service would not appear when it searches hello Relay namespace.</span></span> <span data-ttu-id="1d8fd-244">Ehelyett hello ügyfélnek ehhez már tooknow hello végpont elérési útja előre.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-244">Instead, hello client would have tooknow hello endpoint path beforehand.</span></span>
5. <span data-ttu-id="1d8fd-245">Alkalmazása hello szolgáltatás hitelesítő adatai toohello hello App.config fájlban meghatározott szolgáltatásvégpontokra:</span><span class="sxs-lookup"><span data-stu-id="1d8fd-245">Apply hello service credentials toohello service endpoints defined in hello App.config file:</span></span>

    ```csharp
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    <span data-ttu-id="1d8fd-246">Hello előző lépésben leírtak meg volt van deklarálva több szolgáltatásra, és a végpontok hello konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-246">As stated in hello previous step, you could have declared multiple services and endpoints in hello configuration file.</span></span> <span data-ttu-id="1d8fd-247">Ha, ez a kód végighalad hello konfigurációs fájlt, és keresse a minden végpont toowhich alkalmaznia kell a hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-247">If you had, this code would traverse hello configuration file and search for every endpoint toowhich it should apply your credentials.</span></span> <span data-ttu-id="1d8fd-248">Ebben az oktatóanyagban azonban hello konfigurációs fájl csak egy végponttal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-248">However, for this tutorial, hello configuration file has only one endpoint.</span></span>

### <a name="open-hello-service-host"></a><span data-ttu-id="1d8fd-249">Nyissa meg hello szolgáltatásgazda</span><span class="sxs-lookup"><span data-stu-id="1d8fd-249">Open hello service host</span></span>

1. <span data-ttu-id="1d8fd-250">Nyissa meg a hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-250">Open hello service.</span></span>

    ```csharp
    host.Open();
    ```
2. <span data-ttu-id="1d8fd-251">Tájékoztatja, hogy a szolgáltatás hello hello felhasználói fut, és azt ismertetik, hogyan tooshut le hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-251">Inform hello user that hello service is running, and explain how tooshut down hello service.</span></span>

    ```csharp
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] tooexit");
    Console.ReadLine();
    ```
3. <span data-ttu-id="1d8fd-252">Ha elkészült, zárja be a hello szolgáltatásgazda.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-252">When finished, close hello service host.</span></span>

    ```csharp
    host.Close();
    ```
4. <span data-ttu-id="1d8fd-253">Nyomja le az **Ctrl + Shift + B** toobuild hello projekt.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-253">Press **Ctrl+Shift+B** toobuild hello project.</span></span>

### <a name="example"></a><span data-ttu-id="1d8fd-254">Példa</span><span class="sxs-lookup"><span data-stu-id="1d8fd-254">Example</span></span>

<span data-ttu-id="1d8fd-255">A befejezett szolgáltatáskód hibáit a következőképpen kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-255">Your completed service code should appear as follows.</span></span> <span data-ttu-id="1d8fd-256">hello kódot tartalmazza hello szolgáltatási szerződést és a korábbi lépések megvalósítását hello oktatóanyag, és a gazdagépek hello szolgáltatást egy konzolalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-256">hello code includes hello service contract and implementation from previous steps in hello tutorial, and hosts hello service in a console application.</span></span>

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

           // Create hello credentials object for hello endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create hello service URI based on hello service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create hello service host reading hello configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create hello ServiceRegistrySettings behavior for hello endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add hello Relay credentials tooall endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }

            // Open hello service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] tooexit");
            Console.ReadLine();

            // Close hello service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-hello-service-contract"></a><span data-ttu-id="1d8fd-257">Hello szolgáltatási szerződés WCF-ügyfél létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d8fd-257">Create a WCF client for hello service contract</span></span>

<span data-ttu-id="1d8fd-258">következő lépés hello toocreate egy ügyfélalkalmazást, és a későbbi lépésekben megvalósítandó hello szolgáltatási szerződés megadása.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-258">hello next step is toocreate a client application and define hello service contract you will implement in later steps.</span></span> <span data-ttu-id="1d8fd-259">Vegye figyelembe, hogy sok lépés hasonlítanak hello lépéseket használt toocreate szolgáltatás: a Szerződés meghatározása, szerkesztését az App.config fájlt, használja a hitelesítő adatok tooconnect toohello továbbítási szolgáltatás, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-259">Note that many of these steps resemble hello steps used toocreate a service: defining a contract, editing an App.config file, using credentials tooconnect toohello relay service, and so on.</span></span> <span data-ttu-id="1d8fd-260">a feladatokhoz használt hello kód hello eljárást követő hello példában valósul meg.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-260">hello code used for these tasks is provided in hello example following hello procedure.</span></span>

1. <span data-ttu-id="1d8fd-261">Hozzon létre egy új projektet hello jelenlegi Visual Studio megoldás hello ügyfél hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="1d8fd-261">Create a new project in hello current Visual Studio solution for hello client by doing hello following:</span></span>

   1. <span data-ttu-id="1d8fd-262">A Megoldáskezelőben a hello azonos hello szolgáltatást tartalmazó megoldásban, kattintson a jobb gombbal a hello aktuális megoldás (nem hello projekt), és kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-262">In Solution Explorer, in hello same solution that contains hello service, right-click hello current solution (not hello project), and click **Add**.</span></span> <span data-ttu-id="1d8fd-263">Ezután kattintson a **New Project** (Új projekt) gombra.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-263">Then click **New Project**.</span></span>
   2. <span data-ttu-id="1d8fd-264">A hello **új projekt hozzáadása** párbeszédpanel, kattintson a **Visual C#** (Ha **Visual C#** jelenik meg, keresse meg a **más nyelvek**) elemre, jelölje be hello **Konzolalkalmazás (.NET-keretrendszer)** sablont, és adjon neki nevet **EchoClient**.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-264">In hello **Add New Project** dialog box, click **Visual C#** (if **Visual C#** does not appear, look under **Other Languages**), select hello **Console App (.NET Framework)** template, and name it **EchoClient**.</span></span>
   3. <span data-ttu-id="1d8fd-265">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-265">Click **OK**.</span></span>
      <br />
2. <span data-ttu-id="1d8fd-266">A Megoldáskezelőben kattintson duplán a Program.cs fájljára hello hello **EchoClient** nyissa meg a legyen hello szerkesztő, ha még nincs tooopen projektre.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-266">In Solution Explorer, double-click hello Program.cs file in hello **EchoClient** project tooopen it in hello editor, if it is not already open.</span></span>
3. <span data-ttu-id="1d8fd-267">Hello alapértelmezett nevét a névtér nevének módosítása `EchoClient` túl`Microsoft.ServiceBus.Samples`.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-267">Change hello namespace name from its default name of `EchoClient` too`Microsoft.ServiceBus.Samples`.</span></span>
4. <span data-ttu-id="1d8fd-268">Telepítse a hello [Service Bus NuGet-csomag](https://www.nuget.org/packages/WindowsAzure.ServiceBus): a Megoldáskezelőben kattintson a jobb gombbal hello **EchoClient** projektre, és kattintson a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-268">Install hello [Service Bus NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus): in Solution Explorer, right-click hello **EchoClient** project, and then click **Manage NuGet Packages**.</span></span> <span data-ttu-id="1d8fd-269">Kattintson a hello **Tallózás** lapra, és keresse meg `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-269">Click hello **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="1d8fd-270">Kattintson a **telepítése**, és el kell fogadnia a használati feltételek hello.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-270">Click **Install**, and accept hello terms of use.</span></span>

    ![][3]
5. <span data-ttu-id="1d8fd-271">Adja hozzá a `using` hello nyilatkozata [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) névtér hello Program.cs fájlban.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-271">Add a `using` statement for hello [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) namespace in hello Program.cs file.</span></span>

    ```csharp
    using System.ServiceModel;
    ```
6. <span data-ttu-id="1d8fd-272">Adja hozzá a hello szolgáltatási szerződés meghatározása toohello névterében, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-272">Add hello service contract definition toohello namespace, as shown in hello following example.</span></span> <span data-ttu-id="1d8fd-273">Vegye figyelembe, hogy ez a definíció hello használt azonos toohello meghatározással **szolgáltatás** projekt.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-273">Note that this definition is identical toohello definition used in hello **Service** project.</span></span> <span data-ttu-id="1d8fd-274">Ez a kód hello hello tetején adja hozzá `Microsoft.ServiceBus.Samples` névtér.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-274">You should add this code at hello top of hello `Microsoft.ServiceBus.Samples` namespace.</span></span>

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```
7. <span data-ttu-id="1d8fd-275">Nyomja le az **Ctrl + Shift + B** toobuild hello ügyfél.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-275">Press **Ctrl+Shift+B** toobuild hello client.</span></span>

### <a name="example"></a><span data-ttu-id="1d8fd-276">Példa</span><span class="sxs-lookup"><span data-stu-id="1d8fd-276">Example</span></span>

<span data-ttu-id="1d8fd-277">hello következő kód bemutatja, hello hello Program.cs fájl aktuális állapotát a hello **EchoClient** projekt.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-277">hello following code shows hello current status of hello Program.cs file in hello **EchoClient** project.</span></span>

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

## <a name="configure-hello-wcf-client"></a><span data-ttu-id="1d8fd-278">Hello WCF-ügyfél konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1d8fd-278">Configure hello WCF client</span></span>

<span data-ttu-id="1d8fd-279">Ebben a lépésben hozzon létre egy alapszintű ügyfélalkalmazáshoz, aki hozzáfér az ebben az oktatóanyagban korábban létrehozott hello szolgáltatást az App.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-279">In this step, you create an App.config file for a basic client application that accesses hello service created previously in this tutorial.</span></span> <span data-ttu-id="1d8fd-280">Ez az App.config fájl határozza meg, hello szerződés, binding és hello végpont nevét.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-280">This App.config file defines hello contract, binding, and name of hello endpoint.</span></span> <span data-ttu-id="1d8fd-281">a feladatokhoz használt hello kód hello eljárást követő hello példában valósul meg.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-281">hello code used for these tasks is provided in hello example following hello procedure.</span></span>

1. <span data-ttu-id="1d8fd-282">A Megoldáskezelőben a hello **EchoClient** projektre, kattintson duplán a **App.config** tooopen hello fájl hello Visual Studio szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-282">In Solution Explorer, in hello **EchoClient** project, double-click **App.config** tooopen hello file in hello Visual Studio editor.</span></span>
2. <span data-ttu-id="1d8fd-283">A hello `<appSettings>` elem, hello helyőrzőket cserélje le a szolgáltatásnévtér hello nevét, és a korábbi lépésben másolt SAS-kulcs hello.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-283">In hello `<appSettings>` element, replace hello placeholders with hello name of your service namespace, and hello SAS key that you copied in an earlier step.</span></span>
3. <span data-ttu-id="1d8fd-284">Hello system.serviceModel elemen belül adja hozzá a `<client>` elemet.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-284">Within hello system.serviceModel element, add a `<client>` element.</span></span>

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    <span data-ttu-id="1d8fd-285">Ez a lépés deklarálja, hogy WCF stílusú ügyfélalkalmazást határoz meg.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-285">This step declares that you are defining a WCF-style client application.</span></span>
4. <span data-ttu-id="1d8fd-286">Hello belül `client` elem, hello nevét, a szerződés és a kötési típus hello végpont megadása.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-286">Within hello `client` element, define hello name, contract, and binding type for hello endpoint.</span></span>

    ```xml
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    <span data-ttu-id="1d8fd-287">Ez a lépés hello végpont, meghatározott hello szolgáltatást, és hello tényt, hogy hello ügyfélalkalmazás TCP toocommunicate az Azure-továbbítási hello szerződést hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-287">This step defines hello name of hello endpoint, hello contract defined in hello service, and hello fact that hello client application uses TCP toocommunicate with Azure Relay.</span></span> <span data-ttu-id="1d8fd-288">hello végpontnév használt hello a következő lépés toolink hello szolgáltatás URI-azonosítója a végpont-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-288">hello endpoint name is used in hello next step toolink this endpoint configuration with hello service URI.</span></span>
5. <span data-ttu-id="1d8fd-289">Kattintson a **fájl**, majd kattintson a **összes mentése**.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-289">Click **File**, then click **Save All**.</span></span>

## <a name="example"></a><span data-ttu-id="1d8fd-290">Példa</span><span class="sxs-lookup"><span data-stu-id="1d8fd-290">Example</span></span>

<span data-ttu-id="1d8fd-291">hello következő kód bemutatja hello hello Echo ügyfél App.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-291">hello following code shows hello App.config file for hello Echo client.</span></span>

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

## <a name="implement-hello-wcf-client"></a><span data-ttu-id="1d8fd-292">Alkalmazzon hello WCF-ügyfél</span><span class="sxs-lookup"><span data-stu-id="1d8fd-292">Implement hello WCF client</span></span>
<span data-ttu-id="1d8fd-293">Ebben a lépésben egy alapszintű ügyfélalkalmazáshoz, aki hozzáfér az ebben az oktatóanyagban korábban létrehozott hello szolgáltatás valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-293">In this step, you implement a basic client application that accesses hello service you created previously in this tutorial.</span></span> <span data-ttu-id="1d8fd-294">Hasonló toohello szolgáltatás hello ügyfél végez hello számos azonos műveletek tooaccess Azure továbbítási:</span><span class="sxs-lookup"><span data-stu-id="1d8fd-294">Similar toohello service, hello client performs many of hello same operations tooaccess Azure Relay:</span></span>

1. <span data-ttu-id="1d8fd-295">Hello csatlakozási mód beállítása.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-295">Sets hello connectivity mode.</span></span>
2. <span data-ttu-id="1d8fd-296">URI, amely hello gazdaszolgáltatás hello hoz létre.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-296">Creates hello URI that locates hello host service.</span></span>
3. <span data-ttu-id="1d8fd-297">Megadja a hello biztonsági hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-297">Defines hello security credentials.</span></span>
4. <span data-ttu-id="1d8fd-298">Hello hitelesítő adatok toohello kapcsolat vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-298">Applies hello credentials toohello connection.</span></span>
5. <span data-ttu-id="1d8fd-299">Hello kapcsolat megnyitása.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-299">Opens hello connection.</span></span>
6. <span data-ttu-id="1d8fd-300">Hello alkalmazásspecifikus feladatokat hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-300">Performs hello application-specific tasks.</span></span>
7. <span data-ttu-id="1d8fd-301">Hello kapcsolat bezárása.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-301">Closes hello connection.</span></span>

<span data-ttu-id="1d8fd-302">Azonban hello fő különbségek egyike, hogy hello ügyfélalkalmazás egy csatorna tooconnect toohello továbbítási szolgáltatás, mivel hello szolgáltatást használ egy hívás túl**ServiceHost**.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-302">However, one of hello main differences is that hello client application uses a channel tooconnect toohello relay service, whereas hello service uses a call too**ServiceHost**.</span></span> <span data-ttu-id="1d8fd-303">a feladatokhoz használt hello kód hello eljárást követő hello példában valósul meg.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-303">hello code used for these tasks is provided in hello example following hello procedure.</span></span>

### <a name="implement-a-client-application"></a><span data-ttu-id="1d8fd-304">Ügyfélalkalmazás megvalósítása</span><span class="sxs-lookup"><span data-stu-id="1d8fd-304">Implement a client application</span></span>
1. <span data-ttu-id="1d8fd-305">Hello csatlakozási mód beállítása túl**automatikus felismerés**.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-305">Set hello connectivity mode too**AutoDetect**.</span></span> <span data-ttu-id="1d8fd-306">Adja hozzá a következő kódot hello hello `Main()` hello metódusában **EchoClient** alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-306">Add hello following code inside hello `Main()` method of hello **EchoClient** application.</span></span>

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```
2. <span data-ttu-id="1d8fd-307">Határozza meg a változók toohold hello értékeinek hello szolgáltatásnévtér és SAS-kulcs hello konzolon olvasható.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-307">Define variables toohold hello values for hello service namespace, and SAS key that are read from hello console.</span></span>

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```
3. <span data-ttu-id="1d8fd-308">Hozzon létre hello URI, amely meghatározza a továbbítási projekt hello állomás hello helyét.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-308">Create hello URI that defines hello location of hello host in your Relay project.</span></span>

    ```csharp
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```
4. <span data-ttu-id="1d8fd-309">Hozzon létre hello hitelesítő objektumot a szolgáltatásnévtér végpontjához.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-309">Create hello credential object for your service namespace endpoint.</span></span>

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```
5. <span data-ttu-id="1d8fd-310">Hello App.config fájlban leírt hello konfigurációt betöltő csatornagyárat hello létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-310">Create hello channel factory that loads hello configuration described in hello App.config file.</span></span>

    ```csharp
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    <span data-ttu-id="1d8fd-311">A beépített csatorna egy WCF-objektum, amely létrehoz egy csatornát, amelyen keresztül a hello szolgáltatás és az ügyfélalkalmazások számára kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-311">A channel factory is a WCF object that creates a channel through which hello service and client applications communicate.</span></span>
6. <span data-ttu-id="1d8fd-312">Hello hitelesítő adatok érvényesek.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-312">Apply hello credentials.</span></span>

    ```csharp
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```
7. <span data-ttu-id="1d8fd-313">Hozzon létre, és nyissa meg a hello csatorna toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-313">Create and open hello channel toohello service.</span></span>

    ```csharp
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```
8. <span data-ttu-id="1d8fd-314">Hello alapszintű felhasználói felületét és funkcióit a hello echo írni.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-314">Write hello basic user interface and functionality for hello echo.</span></span>

    ```csharp
    Console.WriteLine("Enter text tooecho (or [Enter] tooexit):");
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

    <span data-ttu-id="1d8fd-315">Vegye figyelembe, hogy hello kód hello példányát használja hello csatornaobjektum proxyként hello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-315">Note that hello code uses hello instance of hello channel object as a proxy for hello service.</span></span>
9. <span data-ttu-id="1d8fd-316">Zárja be a hello csatorna és Bezárás hello gyári.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-316">Close hello channel, and close hello factory.</span></span>

    ```csharp
    channel.Close();
    channelFactory.Close();
    ```

## <a name="example"></a><span data-ttu-id="1d8fd-317">Példa</span><span class="sxs-lookup"><span data-stu-id="1d8fd-317">Example</span></span>

<span data-ttu-id="1d8fd-318">A befejezett kód kell a következőképpen jelenik meg, hogyan toocreate egy ügyfélalkalmazást, hogyan toocall hello hello műveleteit, és hogyan tooclose hello hello művelet után ügyfél hívja megjelenítő befejeződött.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-318">Your completed code should appear as follows, showing how toocreate a client application, how toocall hello operations of hello service, and how tooclose hello client after hello operation call is finished.</span></span>

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

            Console.WriteLine("Enter text tooecho (or [Enter] tooexit):");
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

## <a name="run-hello-applications"></a><span data-ttu-id="1d8fd-319">Hello alkalmazások futtatásához</span><span class="sxs-lookup"><span data-stu-id="1d8fd-319">Run hello applications</span></span>

1. <span data-ttu-id="1d8fd-320">Nyomja le az **Ctrl + Shift + B** toobuild hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-320">Press **Ctrl+Shift+B** toobuild hello solution.</span></span> <span data-ttu-id="1d8fd-321">Ez felépíti hello ügyfélprojekt és a hello projekt hello előző lépésekben létrehozott.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-321">This builds both hello client project and hello service project that you created in hello previous steps.</span></span>
2. <span data-ttu-id="1d8fd-322">Futó hello ügyfélalkalmazást, előtt biztosítsa, hogy fut-e hello szolgáltatás alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-322">Before running hello client application, you must make sure that hello service application is running.</span></span> <span data-ttu-id="1d8fd-323">A Solution Explorerben a Visual Studióban kattintson a jobb gombbal hello **EchoService** megoldás, majd kattintson a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-323">In Solution Explorer in Visual Studio, right-click hello **EchoService** solution, then click **Properties**.</span></span>
3. <span data-ttu-id="1d8fd-324">Hello megoldás tulajdonságai párbeszédpanel, kattintson a **Startup Project**, majd kattintson a hello **több kezdőprojekt** gombra.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-324">In hello solution properties dialog box, click **Startup Project**, then click hello **Multiple startup projects** button.</span></span> <span data-ttu-id="1d8fd-325">Győződjön meg arról, hogy **EchoService** jelenik meg először hello listában.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-325">Make sure **EchoService** appears first in hello list.</span></span>
4. <span data-ttu-id="1d8fd-326">Set hello **művelet** be mindkét hello **EchoService** és **EchoClient** túl projektek**Start**.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-326">Set hello **Action** box for both hello **EchoService** and **EchoClient** projects too**Start**.</span></span>

    ![][5]
5. <span data-ttu-id="1d8fd-327">Kattintson a **Projektfüggőségek** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-327">Click **Project Dependencies**.</span></span> <span data-ttu-id="1d8fd-328">A hello **projektek** mezőben válassza **EchoClient**.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-328">In hello **Projects** box, select **EchoClient**.</span></span> <span data-ttu-id="1d8fd-329">A hello **függ** győződjön meg arról, hogy **EchoService** be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-329">In hello **Depends on** box, make sure **EchoService** is checked.</span></span>

    ![][6]
6. <span data-ttu-id="1d8fd-330">Kattintson a **OK** toodismiss hello **tulajdonságok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-330">Click **OK** toodismiss hello **Properties** dialog.</span></span>
7. <span data-ttu-id="1d8fd-331">Nyomja le az **F5** toorun mindkét projekthez.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-331">Press **F5** toorun both projects.</span></span>
8. <span data-ttu-id="1d8fd-332">Mindkét konzolablak megnyitása, és kéri a hello névtér nevét.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-332">Both console windows open and prompt you for hello namespace name.</span></span> <span data-ttu-id="1d8fd-333">hello szolgáltatást kell futtatni először, így a hello **EchoService** konzolablakában, adjon meg hello névteret, és nyomja le az **Enter**.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-333">hello service must run first, so in hello **EchoService** console window, enter hello namespace and then press **Enter**.</span></span>
9. <span data-ttu-id="1d8fd-334">Ezután a rendszer az SAS-kulcs megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-334">Next, you are prompted for your SAS key.</span></span> <span data-ttu-id="1d8fd-335">Adja meg a hello SAS-kulcsot, és nyomja le az ENTER BILLENTYŰT.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-335">Enter hello SAS key and press ENTER.</span></span>

    <span data-ttu-id="1d8fd-336">Íme egy példa a kimenetre hello konzolablakból.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-336">Here is example output from hello console window.</span></span> <span data-ttu-id="1d8fd-337">Vegye figyelembe, hogy hello itt megadott értékek például jelleggel.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-337">Note that hello values provided here are for example purposes only.</span></span>

    <span data-ttu-id="1d8fd-338">`Your Service Namespace: myNamespace` `Your SAS Key: <SAS key value>`</span><span class="sxs-lookup"><span data-stu-id="1d8fd-338">`Your Service Namespace: myNamespace` `Your SAS Key: <SAS key value>`</span></span>

    <span data-ttu-id="1d8fd-339">hello szolgáltatásalkalmazás kiírja a toohello-konzol ablak hello címét, amelyen figyel, hello a következő példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-339">hello service application prints toohello console window hello address on which it's listening, as seen in hello following example.</span></span>

    <span data-ttu-id="1d8fd-340">`Service address: sb://mynamespace.servicebus.windows.net/EchoService/` `Press [Enter] tooexit`</span><span class="sxs-lookup"><span data-stu-id="1d8fd-340">`Service address: sb://mynamespace.servicebus.windows.net/EchoService/` `Press [Enter] tooexit`</span></span>
10. <span data-ttu-id="1d8fd-341">A hello **EchoClient** konzolablakában, adja meg ugyanazokat az információkat, amelyek korábban hello szolgáltatásalkalmazáshoz megadott hello.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-341">In hello **EchoClient** console window, enter hello same information that you entered previously for hello service application.</span></span> <span data-ttu-id="1d8fd-342">Kövesse az előző lépéseket tooenter hello hello azonos szolgáltatásnévtér és SAS kulcsértékek hello ügyfélalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-342">Follow hello previous steps tooenter hello same service namespace and SAS key values for hello client application.</span></span>
11. <span data-ttu-id="1d8fd-343">Az értékek megadása után hello ügyfél megnyit egy csatorna toohello szolgáltatás, és felszólítja, tooenter hello a következő példa konzolkimenetben látható szöveg.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-343">After entering these values, hello client opens a channel toohello service and prompts you tooenter some text as seen in hello following console output example.</span></span>

    `Enter text tooecho (or [Enter] tooexit):`

    <span data-ttu-id="1d8fd-344">Adja meg az egyes szöveg toosend toohello szolgáltatásalkalmazás, és nyomja le az ENTER billentyűt.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-344">Enter some text toosend toohello service application and press Enter.</span></span> <span data-ttu-id="1d8fd-345">Ez a szöveg toohello szolgáltatás hello Echo szolgáltatásműveleten keresztül zajlik, és hello szolgáltatás konzolablakában, mint a következő egy példa a kimenetre hello jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-345">This text is sent toohello service through hello Echo service operation and appears in hello service console window as in hello following example output.</span></span>

    `Echoing: My sample text`

    <span data-ttu-id="1d8fd-346">hello ügyfélalkalmazás fogadja hello visszatérési értéke hello `Echo` művelet, amely hello eredeti szöveg, és kiírja tooits konzolablakában.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-346">hello client application receives hello return value of hello `Echo` operation, which is hello original text, and prints it tooits console window.</span></span> <span data-ttu-id="1d8fd-347">hello következő egy példa kimenet hello ügyfél konzolablakából.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-347">hello following is example output from hello client console window.</span></span>

    `Server echoed: My sample text`
12. <span data-ttu-id="1d8fd-348">Folytathatja szöveges üzenetek küldését toohello ügyfélszolgáltatás hello ily módon.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-348">You can continue sending text messages from hello client toohello service in this manner.</span></span> <span data-ttu-id="1d8fd-349">Ha elkészült, nyomja le az Enter a hello ügyfél és a szolgáltatás konzol windows tooend mindkét alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-349">When you are finished, press Enter in hello client and service console windows tooend both applications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1d8fd-350">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1d8fd-350">Next steps</span></span>

<span data-ttu-id="1d8fd-351">Ez az oktatóanyag bemutatta, hogyan toobuild egy Azure-továbbítási ügyfélalkalmazást és szolgáltatást használó hello Service Bus WCF továbbító képességeit.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-351">This tutorial showed how toobuild an Azure Relay client application and service using hello WCF Relay capabilities of Service Bus.</span></span> <span data-ttu-id="1d8fd-352">Egy alkalmazó hasonló oktatóanyagért [Service Bus üzenetkezelés](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), lásd: [Ismerkedés a Service Bus-üzenetsorok](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span><span class="sxs-lookup"><span data-stu-id="1d8fd-352">For a similar tutorial that uses [Service Bus Messaging](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), see [Get started with Service Bus queues](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span></span>

<span data-ttu-id="1d8fd-353">toolearn Azure továbbítási kapcsolatos további információkért tekintse meg a következő témakörök hello.</span><span class="sxs-lookup"><span data-stu-id="1d8fd-353">toolearn more about Azure Relay, see hello following topics.</span></span>

* [<span data-ttu-id="1d8fd-354">Az Azure Service Bus-architektúra áttekintése</span><span class="sxs-lookup"><span data-stu-id="1d8fd-354">Azure Service Bus architectural overview</span></span>](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#relays)
* [<span data-ttu-id="1d8fd-355">Az Azure Relay áttekintése</span><span class="sxs-lookup"><span data-stu-id="1d8fd-355">Azure Relay overview</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="1d8fd-356">Hogyan toouse hello WCF továbbítják a .NET-bA</span><span class="sxs-lookup"><span data-stu-id="1d8fd-356">How toouse hello WCF relay service with .NET</span></span>](relay-wcf-dotnet-get-started.md)

[Azure classic portal]: http://manage.windowsazure.com

[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png
