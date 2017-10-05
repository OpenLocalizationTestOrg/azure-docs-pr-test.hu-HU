---
title: "Service Bus REST oktatóanyag Azure Relay használatával |} Microsoft Docs"
description: "Hozzon létre egy egyszerű Azure Service Bus relay gazdaalkalmazást REST-alapú felületet."
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 1312b2db-94c4-4a48-b815-c5deb5b77a6a
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2017
ms.author: sethm
ms.openlocfilehash: 0db9dbd2d2743907e3f0b259228201d4f5d0c3c2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-wcf-relay-rest-tutorial"></a><span data-ttu-id="bf156-103">Az Azure WCF Relay REST-oktatóanyaga</span><span class="sxs-lookup"><span data-stu-id="bf156-103">Azure WCF Relay REST tutorial</span></span>

<span data-ttu-id="bf156-104">Ez az oktatóanyag leírja, hogyan hozhat létre egy egyszerű Azure Relay gazdaalkalmazást REST-alapú felületet.</span><span class="sxs-lookup"><span data-stu-id="bf156-104">This tutorial describes how to build a simple Azure Relay host application that exposes a REST-based interface.</span></span> <span data-ttu-id="bf156-105">A REST lehetővé teszi egy webes ügyfél, például egy webes böngésző számára, hogy hozzáférjen a HTTP-kérelmeken keresztül a Service Bus alkalmazásprogramozási felületekhez.</span><span class="sxs-lookup"><span data-stu-id="bf156-105">REST enables a web client, such as a web browser, to access the Service Bus APIs through HTTP requests.</span></span>

<span data-ttu-id="bf156-106">Az oktatóprogram a Windows Communication Foundation (WCF) REST programozási modell segítségével egy REST-szolgáltatást a Service Bus összeállításához.</span><span class="sxs-lookup"><span data-stu-id="bf156-106">The tutorial uses the Windows Communication Foundation (WCF) REST programming model to construct a REST service on Service Bus.</span></span> <span data-ttu-id="bf156-107">További információt a WCF-dokumentáció [WCF REST Programming Model](/dotnet/framework/wcf/feature-details/wcf-web-http-programming-model) (WCF REST programozási modell) és [Designing and Implementing Services](/dotnet/framework/wcf/designing-and-implementing-services) (Szolgáltatások tervezése és megvalósítása) témakörében találhat.</span><span class="sxs-lookup"><span data-stu-id="bf156-107">For more information, see [WCF REST Programming Model](/dotnet/framework/wcf/feature-details/wcf-web-http-programming-model) and [Designing and Implementing Services](/dotnet/framework/wcf/designing-and-implementing-services) in the WCF documentation.</span></span>

## <a name="step-1-create-a-namespace"></a><span data-ttu-id="bf156-108">1. lépés: Névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="bf156-108">Step 1: Create a namespace</span></span>

<span data-ttu-id="bf156-109">A Relay-funkciók Azure-ban való használatához először létre kell hoznia egy szolgáltatásnévteret.</span><span class="sxs-lookup"><span data-stu-id="bf156-109">To begin using the relay features in Azure, you must first create a service namespace.</span></span> <span data-ttu-id="bf156-110">A névtér egy hatókörkezelési tárolót biztosít az Azure erőforrásainak címzéséhez az alkalmazáson belül.</span><span class="sxs-lookup"><span data-stu-id="bf156-110">A namespace provides a scoping container for addressing Azure resources within your application.</span></span> <span data-ttu-id="bf156-111">Relay-névtér létrehozásához kövesse az [itt leírt utasításokat](relay-create-namespace-portal.md).</span><span class="sxs-lookup"><span data-stu-id="bf156-111">Follow the [instructions here](relay-create-namespace-portal.md) to create a Relay namespace.</span></span>

## <a name="step-2-define-a-rest-based-wcf-service-contract-to-use-with-azure-relay"></a><span data-ttu-id="bf156-112">2. lépés: Azure Relay használata egy REST-alapú WCF szolgáltatási szerződés megadása</span><span class="sxs-lookup"><span data-stu-id="bf156-112">Step 2: Define a REST-based WCF service contract to use with Azure Relay</span></span>

<span data-ttu-id="bf156-113">Egy WCF REST-stílusú szolgáltatás létrehozásakor meg kell adnia egy szerződést.</span><span class="sxs-lookup"><span data-stu-id="bf156-113">When you create a WCF REST-style service, you must define the contract.</span></span> <span data-ttu-id="bf156-114">A szerződés megadja a gazdagép által támogatott műveleteket.</span><span class="sxs-lookup"><span data-stu-id="bf156-114">The contract specifies what operations the host supports.</span></span> <span data-ttu-id="bf156-115">A szolgáltatási művelet tekinthető webszolgáltatási módszernek.</span><span class="sxs-lookup"><span data-stu-id="bf156-115">A service operation can be thought of as a web service method.</span></span> <span data-ttu-id="bf156-116">A szerződések a C++, a C# vagy a Visual Basic felület meghatározásával jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="bf156-116">Contracts are created by defining a C++, C#, or Visual Basic interface.</span></span> <span data-ttu-id="bf156-117">A felület minden metódusa egy konkrét szolgáltatási műveletnek felel meg.</span><span class="sxs-lookup"><span data-stu-id="bf156-117">Each method in the interface corresponds to a specific service operation.</span></span> <span data-ttu-id="bf156-118">A [ServiceContractAttriibute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribútumot minden felületre, az [OperationContractAttribaute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribútumot pedig minden műveletre alkalmazni kell.</span><span class="sxs-lookup"><span data-stu-id="bf156-118">The [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribute must be applied to each interface, and the [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribute must be applied to each operation.</span></span> <span data-ttu-id="bf156-119">Ha egy felület egy metódusa rendelkezik a [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribútummal, de nem rendelkezik az [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribútummal, nem lesz közzétéve.</span><span class="sxs-lookup"><span data-stu-id="bf156-119">If a method in an interface that has the [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) does not have the [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), that method is not exposed.</span></span> <span data-ttu-id="bf156-120">A feladatokhoz használt kód megtalálható az eljárást követő példában.</span><span class="sxs-lookup"><span data-stu-id="bf156-120">The code used for these tasks is shown in the example following the procedure.</span></span>

<span data-ttu-id="bf156-121">Az elsődleges különbség a WCF szerződés és egy REST-stílusú szerződés között a következő tulajdonság hozzáadása a [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx).</span><span class="sxs-lookup"><span data-stu-id="bf156-121">The primary difference between a WCF contract and a REST-style contract is the addition of a property to the [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx).</span></span> <span data-ttu-id="bf156-122">Ez a tulajdonság lehetővé teszi a felület egy metódusának leképezését egy, a felület másik oldalán levő metódussá.</span><span class="sxs-lookup"><span data-stu-id="bf156-122">This property enables you to map a method in your interface to a method on the other side of the interface.</span></span> <span data-ttu-id="bf156-123">Ebben az esetben a [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) segítségével csatolunk egy metódust a HTTP GET kérelemhez.</span><span class="sxs-lookup"><span data-stu-id="bf156-123">In this case, we will use [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) to link a method to HTTP GET.</span></span> <span data-ttu-id="bf156-124">Ez lehetővé teszi a Service Bus számára, hogy pontosan lekérje és értelmezze a felületre küldött parancsokat.</span><span class="sxs-lookup"><span data-stu-id="bf156-124">This allows Service Bus to accurately retrieve and interpret commands sent to the interface.</span></span>

### <a name="to-create-a-contract-with-an-interface"></a><span data-ttu-id="bf156-125">A szerződés létrehozása felülettel</span><span class="sxs-lookup"><span data-stu-id="bf156-125">To create a contract with an interface</span></span>

1. <span data-ttu-id="bf156-126">Nyissa meg a Visual Studiót rendszergazdaként: ehhez a **Start** menüben kattintson a jobb gombbal a programra, majd kattintson a **Futtatás rendszergazdaként** parancsra.</span><span class="sxs-lookup"><span data-stu-id="bf156-126">Open Visual Studio as an administrator: right-click the program in the **Start** menu, and then click **Run as administrator**.</span></span>
2. <span data-ttu-id="bf156-127">Hozzon létre új egy új konzolalkalmazás-projektet.</span><span class="sxs-lookup"><span data-stu-id="bf156-127">Create a new console application project.</span></span> <span data-ttu-id="bf156-128">Kattintson a **File** (Fájl) menüre, és válassza a **New** (Új), majd a **Project** (Projekt) elemet.</span><span class="sxs-lookup"><span data-stu-id="bf156-128">Click the **File** menu and select **New**, then select **Project**.</span></span> <span data-ttu-id="bf156-129">A **New Project** (Új projekt) párbeszédpanelen kattintson a **Visual C#** elemre, válassza ki a **Console Application** (Konzolalkalmazás) sablont, és nevezze el **ImageListener** néven.</span><span class="sxs-lookup"><span data-stu-id="bf156-129">In the **New Project** dialog box, click **Visual C#**, select the **Console Application** template, and name it **ImageListener**.</span></span> <span data-ttu-id="bf156-130">Használja az alapértelmezett **Location** (Hely) értéket.</span><span class="sxs-lookup"><span data-stu-id="bf156-130">Use the default **Location**.</span></span> <span data-ttu-id="bf156-131">A projekt létrehozásához kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="bf156-131">Click **OK** to create the project.</span></span>
3. <span data-ttu-id="bf156-132">C# projekt esetében a Visual Studio létrehoz egy `Program.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="bf156-132">For a C# project, Visual Studio creates a `Program.cs` file.</span></span> <span data-ttu-id="bf156-133">Ez az osztály tartalmaz egy üres `Main()` metódust, amely szükséges a konzolalkalmazás projektek helyes létrejöttéhez.</span><span class="sxs-lookup"><span data-stu-id="bf156-133">This class contains an empty `Main()` method, required for a console application project to build correctly.</span></span>
4. <span data-ttu-id="bf156-134">Adjon hozzá Service Bus- és **System.ServiceModel.dll**-hivatkozásokat a projekthez a Service Bus NuGet csomag telepítésével.</span><span class="sxs-lookup"><span data-stu-id="bf156-134">Add references to Service Bus and **System.ServiceModel.dll** to the project by installing the Service Bus NuGet package.</span></span> <span data-ttu-id="bf156-135">Ez a csomag automatikusan hivatkozásokat ad a Service Bus-könyvtárakhoz, valamint a WCF **System.ServiceModel** névtérhez.</span><span class="sxs-lookup"><span data-stu-id="bf156-135">This package automatically adds references to the Service Bus libraries, as well as the WCF **System.ServiceModel**.</span></span> <span data-ttu-id="bf156-136">A Solution Explorerben (Megoldáskezelőben) kattintson a jobb gombbal az **ImageListener** projektre, majd kattintson a **Manage NuGet Packages** (NuGet-csomagok kezelése) parancsra.</span><span class="sxs-lookup"><span data-stu-id="bf156-136">In Solution Explorer, right-click the **ImageListener** project, and then click **Manage NuGet Packages**.</span></span> <span data-ttu-id="bf156-137">Kattintson a **Browse** (Tallózás) lapra, és keressen a következőre: `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="bf156-137">Click the **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="bf156-138">Kattintson az **Install** (Telepítés) gombra, és fogadja el a használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="bf156-138">Click **Install**, and accept the terms of use.</span></span>
5. <span data-ttu-id="bf156-139">Expliciten hozzá kell adnia egy hivatkozást a projekt **System.ServiceModel.Web.dll** fájljához:</span><span class="sxs-lookup"><span data-stu-id="bf156-139">You must explicitly add a reference to **System.ServiceModel.Web.dll** to the project:</span></span>
   
    <span data-ttu-id="bf156-140">a.</span><span class="sxs-lookup"><span data-stu-id="bf156-140">a.</span></span> <span data-ttu-id="bf156-141">A Solution Explorerben (Megoldáskezelőben) kattintson a jobb gombbal a **References** (Hivatkozások) mappára a projektmappa területén, majd kattintson az **Add Reference** (Hivatkozás hozzáadása) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="bf156-141">In Solution Explorer, right-click the **References** folder under the project folder and then click **Add Reference**.</span></span>
   
    <span data-ttu-id="bf156-142">b.</span><span class="sxs-lookup"><span data-stu-id="bf156-142">b.</span></span> <span data-ttu-id="bf156-143">Az **Add Reference** (Hivatkozás hozzáadása) párbeszédablakban kattintson a bal oldalon a **Framework** (Keretrendszer) lapra, majd a **Search** (Keresés) mezőben írja be a **System.ServiceModel.Web** kifejezést.</span><span class="sxs-lookup"><span data-stu-id="bf156-143">In the **Add Reference** dialog box, click the **Framework** tab on the left-hand side and in the **Search** box, type **System.ServiceModel.Web**.</span></span> <span data-ttu-id="bf156-144">Jelölje be a **System.ServiceModel.Web** jelölőnégyzetet, majd kattintson az **OK** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="bf156-144">Select the **System.ServiceModel.Web** check box, then click **OK**.</span></span>
6. <span data-ttu-id="bf156-145">Adja hozzá a következő `using` utasításokat a Program.cs fájl elejéhez:</span><span class="sxs-lookup"><span data-stu-id="bf156-145">Add the following `using` statements at the top of the Program.cs file.</span></span>
   
    ```csharp
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```
   
    <span data-ttu-id="bf156-146">A [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) az a névtér, amely lehetővé teszi a programozott hozzáférést a WCF alapszintű szolgáltatásaihoz.</span><span class="sxs-lookup"><span data-stu-id="bf156-146">[System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) is the namespace that enables programmatic access to basic features of WCF.</span></span> <span data-ttu-id="bf156-147">WCF továbbító használja az objektumok és attribútumok WCF szolgáltatási szerződések meghatározására.</span><span class="sxs-lookup"><span data-stu-id="bf156-147">WCF Relay uses many of the objects and attributes of WCF to define service contracts.</span></span> <span data-ttu-id="bf156-148">A relay alkalmazásban a legtöbb fogja használni ehhez a névtérhez.</span><span class="sxs-lookup"><span data-stu-id="bf156-148">You will use this namespace in most of your relay applications.</span></span> <span data-ttu-id="bf156-149">Hasonlóképpen [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) segítségével meghatározhatja a csatornát, amely az objektum, amelyen keresztül kommunikálhat a Azure-továbbító és az ügyfél webböngészőjével.</span><span class="sxs-lookup"><span data-stu-id="bf156-149">Similarly, [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) helps define the channel, which is the object through which you communicate with Azure Relay and the client web browser.</span></span> <span data-ttu-id="bf156-150">Végül a [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) tartalmazza a webalapú alkalmazások létrehozását engedélyező típusokat.</span><span class="sxs-lookup"><span data-stu-id="bf156-150">Finally, [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) contains the types that enable you to create web-based applications.</span></span>
7. <span data-ttu-id="bf156-151">Nevezze át az `ImageListener` névteret **Microsoft.ServiceBus.Samples** névre.</span><span class="sxs-lookup"><span data-stu-id="bf156-151">Rename the `ImageListener` namespace to **Microsoft.ServiceBus.Samples**.</span></span>
   
    ```csharp
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```
8. <span data-ttu-id="bf156-152">Közvetlenül a névtér-deklaráció nyitó zárójele után adjon meg egy új, **IImageContract** nevű felületet, és alkalmazza a **ServiceContractAttribute** attribútumot a felületen `http://samples.microsoft.com/ServiceModel/Relay/` értékkel.</span><span class="sxs-lookup"><span data-stu-id="bf156-152">Directly after the opening curly brace of the namespace declaration, define a new interface named **IImageContract** and apply the **ServiceContractAttribute** attribute to the interface with a value of `http://samples.microsoft.com/ServiceModel/Relay/`.</span></span> <span data-ttu-id="bf156-153">A névtér értéke különbözik a kód tartományában használt névtértől.</span><span class="sxs-lookup"><span data-stu-id="bf156-153">The namespace value differs from the namespace that you use throughout the scope of your code.</span></span> <span data-ttu-id="bf156-154">A névtér értéke egyedi azonosítóként van használatban ehhez a szerződéshez, és rendelkeznie kell a verzióinformációkkal.</span><span class="sxs-lookup"><span data-stu-id="bf156-154">The namespace value is used as a unique identifier for this contract, and should have version information.</span></span> <span data-ttu-id="bf156-155">További információ: [Service Versioning](http://go.microsoft.com/fwlink/?LinkID=180498) (Szolgáltatás verziószámozása).</span><span class="sxs-lookup"><span data-stu-id="bf156-155">For more information, see [Service Versioning](http://go.microsoft.com/fwlink/?LinkID=180498).</span></span> <span data-ttu-id="bf156-156">A névtér explicit meghatározásával megelőzhető az alapértelmezett névtér hozzáadása a szerződésnévhez.</span><span class="sxs-lookup"><span data-stu-id="bf156-156">Specifying the namespace explicitly prevents the default namespace value from being added to the contract name.</span></span>
   
    ```csharp
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```
9. <span data-ttu-id="bf156-157">Az `IImageContract` felületen belül deklaráljon egy metódust az `IImageContract` szerződés által a felületen közzétett egyetlen művelethez, valamint alkalmazza az `OperationContractAttribute` attribútumot a nyilvános Service Bus szerződés részeként közzétenni kívánt metódusra.</span><span class="sxs-lookup"><span data-stu-id="bf156-157">Within the `IImageContract` interface, declare a method for the single operation the `IImageContract` contract exposes in the interface and apply the `OperationContractAttribute` attribute to the method that you want to expose as part of the public Service Bus contract.</span></span>
   
    ```csharp
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```
10. <span data-ttu-id="bf156-158">Az **OperationContract** attribútumban adja hozzá a **WebGet** értéket.</span><span class="sxs-lookup"><span data-stu-id="bf156-158">In the **OperationContract** attribute, add the **WebGet** value.</span></span>
    
    ```csharp
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```
    
    <span data-ttu-id="bf156-159">Ez igen értékkel engedélyezheti a továbbítási szolgáltatás HTTP GET kérelmek `GetImage`, és a visszaadott értékeinek lefordítását `GetImage` egy HTTP GETRESPONSE válaszba.</span><span class="sxs-lookup"><span data-stu-id="bf156-159">Doing so enables the relay service to route HTTP GET requests to `GetImage`, and to translate the return values of `GetImage` into an HTTP GETRESPONSE reply.</span></span> <span data-ttu-id="bf156-160">Az oktatóanyagban később egy webböngészőt használhat majd a metódus eléréséhez és a kép megjelenítéséhez böngészőben.</span><span class="sxs-lookup"><span data-stu-id="bf156-160">Later in the tutorial, you will use a web browser to access this method, and to display the image in the browser.</span></span>
11. <span data-ttu-id="bf156-161">Közvetlenül az `IImageContract` definíciója után deklaráljon egy csatornát, amely örökli az `IImageContract` és az `IClientChannel` felületek tulajdonságait is.</span><span class="sxs-lookup"><span data-stu-id="bf156-161">Directly after the `IImageContract` definition, declare a channel that inherits from both the `IImageContract` and `IClientChannel` interfaces.</span></span>
    
    ```csharp
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```
    
    <span data-ttu-id="bf156-162">A csatorna egy olyan WCF-objektum, amelyen keresztül a szolgáltatás és az ügyfél információkat adnak át egymásnak.</span><span class="sxs-lookup"><span data-stu-id="bf156-162">A channel is the WCF object through which the service and client pass information to each other.</span></span> <span data-ttu-id="bf156-163">Később létrehozhat egy csatornát a gazdaalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="bf156-163">Later, you will create the channel in your host application.</span></span> <span data-ttu-id="bf156-164">Az Azure továbbítási használja ezt a csatornát továbbítja a HTTP GET kérelmeket a böngészőből a **GetImage** végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="bf156-164">Azure Relay then uses this channel to pass the HTTP GET requests from the browser to your **GetImage** implementation.</span></span> <span data-ttu-id="bf156-165">A relay is használja a csatornát érvénybe a **GetImage** ad vissza értéket, és azt jelenti azt, hogy az ügyfél böngészője a HTTP getresponse értékre.</span><span class="sxs-lookup"><span data-stu-id="bf156-165">The relay also uses the channel to take the **GetImage** return value and translate it into an HTTP GETRESPONSE for the client browser.</span></span>
12. <span data-ttu-id="bf156-166">A **Build** (Létrehozás) menüben kattintson a **Build Solution** (Megoldás létrehozása) elemre az eddigi munkája pontosságának ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="bf156-166">From the **Build** menu, click **Build Solution** to confirm the accuracy of your work so far.</span></span>

### <a name="example"></a><span data-ttu-id="bf156-167">Példa</span><span class="sxs-lookup"><span data-stu-id="bf156-167">Example</span></span>
<span data-ttu-id="bf156-168">A következő kód bemutatja egy WCF továbbító szerződést meghatározó alapszintű felületet.</span><span class="sxs-lookup"><span data-stu-id="bf156-168">The following code shows a basic interface that defines a WCF Relay contract.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="step-3-implement-a-rest-based-wcf-service-contract-to-use-service-bus"></a><span data-ttu-id="bf156-169">3. lépés: A Service Busszal használni kívánt REST-alapú WCF szolgáltatási szerződés megvalósítása</span><span class="sxs-lookup"><span data-stu-id="bf156-169">Step 3: Implement a REST-based WCF service contract to use Service Bus</span></span>
<span data-ttu-id="bf156-170">REST-stílusú WCF továbbító szolgáltatás létrehozása megköveteli, hogy először létre kell hoznia a szerződést, amelyet egy felület használatával.</span><span class="sxs-lookup"><span data-stu-id="bf156-170">Creating a REST-style WCF Relay service requires that you first create the contract, which is defined by using an interface.</span></span> <span data-ttu-id="bf156-171">A következő lépés a felület megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="bf156-171">The next step is to implement the interface.</span></span> <span data-ttu-id="bf156-172">Ebbe beletartozik egy **ImageService** nevű osztály létrehozása, amely megvalósítja a felhasználó által megadott **IImageContract** felületet.</span><span class="sxs-lookup"><span data-stu-id="bf156-172">This involves creating a class named **ImageService** that implements the user-defined **IImageContract** interface.</span></span> <span data-ttu-id="bf156-173">A szerződés megvalósítása után egy App.config fájl segítségével konfigurálhatja a felületet.</span><span class="sxs-lookup"><span data-stu-id="bf156-173">After you implement the contract, you then configure the interface using an App.config file.</span></span> <span data-ttu-id="bf156-174">A konfigurációs fájl tartalmazza a szükséges információkat az alkalmazás, például a szolgáltatás nevét, a neve, a szerződés és a továbbítási szolgáltatás folytatott kommunikációhoz használt protokoll típusát.</span><span class="sxs-lookup"><span data-stu-id="bf156-174">The configuration file contains necessary information for the application, such as the name of the service, the name of the contract, and the type of protocol that is used to communicate with the relay service.</span></span> <span data-ttu-id="bf156-175">A feladatokhoz használt kód megtalálható az eljárást követő példában.</span><span class="sxs-lookup"><span data-stu-id="bf156-175">The code used for these tasks is provided in the example following the procedure.</span></span>

<span data-ttu-id="bf156-176">Csakúgy, mint az előző lépéseknél, nagyon kicsi a különbség a REST-stílusú szerződés és egy WCF továbbító szerződés megvalósítása között van.</span><span class="sxs-lookup"><span data-stu-id="bf156-176">As with the previous steps, there is very little difference between implementing a REST-style contract and a WCF Relay contract.</span></span>

### <a name="to-implement-a-rest-style-service-bus-contract"></a><span data-ttu-id="bf156-177">REST-stílusú Service Bus szerződés megvalósítása</span><span class="sxs-lookup"><span data-stu-id="bf156-177">To implement a REST-style Service Bus contract</span></span>
1. <span data-ttu-id="bf156-178">Hozzon létre egy új, **ImageService** nevű osztályt közvetlenül az **IImageContract** felület meghatározása után.</span><span class="sxs-lookup"><span data-stu-id="bf156-178">Create a new class named **ImageService** directly after the definition of the **IImageContract** interface.</span></span> <span data-ttu-id="bf156-179">Az **ImageService** osztály az **IImageContract** felületet valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="bf156-179">The **ImageService** class implements the **IImageContract** interface.</span></span>
   
    ```csharp
    class ImageService : IImageContract
    {
    }
    ```
    <span data-ttu-id="bf156-180">Hasonlóan az egyéb felületi megvalósításokhoz, a definíciót megvalósíthatja egy másik fájlban.</span><span class="sxs-lookup"><span data-stu-id="bf156-180">Similar to other interface implementations, you can implement the definition in a different file.</span></span> <span data-ttu-id="bf156-181">Ebben az oktatóanyagban azonban a megvalósítás ugyanabban a fájlban jelenik meg, mint a felületdefiníció és a `Main()` metódus.</span><span class="sxs-lookup"><span data-stu-id="bf156-181">However, for this tutorial, the implementation appears in the same file as the interface definition and `Main()` method.</span></span>
2. <span data-ttu-id="bf156-182">Alkalmazza a [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) attribútumot az **IImageService** osztályra, így jelezheti, hogy az osztály egy WCF-szerződés megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="bf156-182">Apply the [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) attribute to the **IImageService** class to indicate that the class is an implementation of a WCF contract.</span></span>
   
    ```csharp
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```
   
    <span data-ttu-id="bf156-183">Amint azt korábban említettük, ez a névtér nem egy hagyományos névtér.</span><span class="sxs-lookup"><span data-stu-id="bf156-183">As mentioned previously, this namespace is not a traditional namespace.</span></span> <span data-ttu-id="bf156-184">Ehelyett ez a szerződést azonosító WCF-architektúra része.</span><span class="sxs-lookup"><span data-stu-id="bf156-184">Instead, it is part of the WCF architecture that identifies the contract.</span></span> <span data-ttu-id="bf156-185">További információt a WCF-dokumentáció [Data Contract Names](https://msdn.microsoft.com/library/ms731045.aspx) (Adategyezmény-nevek) témakörében találhat.</span><span class="sxs-lookup"><span data-stu-id="bf156-185">For more information, see the [Data Contract Names](https://msdn.microsoft.com/library/ms731045.aspx) topic in the WCF documentation.</span></span>
3. <span data-ttu-id="bf156-186">Adjon hozzá egy .jpg képet a projekthez.</span><span class="sxs-lookup"><span data-stu-id="bf156-186">Add a .jpg image to your project.</span></span>  
   
    <span data-ttu-id="bf156-187">Ez egy kép, amelyet a szolgáltatás megjelenít a fogadó böngészőben.</span><span class="sxs-lookup"><span data-stu-id="bf156-187">This is a picture that the service displays in the receiving browser.</span></span> <span data-ttu-id="bf156-188">Kattintson a jobb gombbal a projektre, majd kattintson az **Add** (Hozzáadás) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="bf156-188">Right-click your project, then click **Add**.</span></span> <span data-ttu-id="bf156-189">Ezután kattintson az **Existing Item** (Meglévő elem) elemre.</span><span class="sxs-lookup"><span data-stu-id="bf156-189">Then click **Existing Item**.</span></span> <span data-ttu-id="bf156-190">Az **Add Existing Item** (Meglévő elem hozzáadása) párbeszédablak segítségével keressen egy megfelelő .jpg fájlt, és kattintson az **Add** (Hozzáadás) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="bf156-190">Use the **Add Existing Item** dialog box to browse to an appropriate .jpg, and then click **Add**.</span></span>
   
    <span data-ttu-id="bf156-191">Egy fájl hozzáadásakor győződjön meg arról, hogy a legördülő menüben kiválasztotta az **All Files** (Minden fájl) lehetőséget a **File nane:** (Fájlnév:) mező mellett.</span><span class="sxs-lookup"><span data-stu-id="bf156-191">When adding the file, make sure that **All Files** is selected in the drop-down list next to the **File name:** field.</span></span> <span data-ttu-id="bf156-192">Az oktatóanyag hátralevő része feltételezi, hogy a kép neve image.jpg.</span><span class="sxs-lookup"><span data-stu-id="bf156-192">The rest of this tutorial assumes that the name of the image is "image.jpg".</span></span> <span data-ttu-id="bf156-193">Ha egy másik fájlja van, át kell neveznie a képet, vagy módosítania kell a kódot a helyesbítéshez.</span><span class="sxs-lookup"><span data-stu-id="bf156-193">If you have a different file, you will have to rename the image, or change your code to compensate.</span></span>
4. <span data-ttu-id="bf156-194">Annak biztosításához, hogy a futó szolgáltatás megtalálja a képfájlt, a **Solution Explorerben** (Megoldáskezelőben) kattintson a jobb gombbal képfájlra, majd kattintson a **Properties** (Tulajdonságok) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="bf156-194">To make sure that the running service can find the image file, in **Solution Explorer** right-click the image file, then click **Properties**.</span></span> <span data-ttu-id="bf156-195">A **Properties** (Tulajdonságok) panelen állítsa a **Copy to Output Directory** (Másolás a kimeneti könyvtárba) beállítást **Copy if newer** (Másolás, ha újabb) értékre.</span><span class="sxs-lookup"><span data-stu-id="bf156-195">In the **Properties** pane, set **Copy to Output Directory** to **Copy if newer**.</span></span>
5. <span data-ttu-id="bf156-196">Adjon egy hivatkozást a projekt **System.Drawing.dll** szerelvényéhez, és adja hozzá az alábbi társított `using` utasításokat is.</span><span class="sxs-lookup"><span data-stu-id="bf156-196">Add a reference to the **System.Drawing.dll** assembly to the project, and also add the following associated `using` statements.</span></span>  
   
    ```csharp
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```
6. <span data-ttu-id="bf156-197">Az **ImageService** osztályban adja hozzá az alábbi konstruktort, amely betölti a bitképet, és előkészíti az ügyfél böngészőjébe való küldéshez.</span><span class="sxs-lookup"><span data-stu-id="bf156-197">In the **ImageService** class, add the following constructor that loads the bitmap and prepares to send it to the client browser.</span></span>
   
    ```csharp
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";
   
        Image bitmap;
   
        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }
    }
    ```
7. <span data-ttu-id="bf156-198">Közvetlenül az előző kód után adja hozzá az alábbi **GetImage** metódust az **ImageService** osztályban egy, a képet tartalmazó HTTP-üzenet visszaadásához.</span><span class="sxs-lookup"><span data-stu-id="bf156-198">Directly after the previous code, add the following **GetImage** method in the **ImageService** class to return an HTTP message that contains the image.</span></span>
   
    ```csharp
    public Stream GetImage()
    {
        MemoryStream stream = new MemoryStream();
        this.bitmap.Save(stream, ImageFormat.Jpeg);
   
        stream.Position = 0;
        WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";
   
        return stream;
    }
    ```
   
    <span data-ttu-id="bf156-199">Ez a megvalósítás a **MemoryStream** segítségével kéri le a képet, és előkészíti a böngészőbe történő streameléshez.</span><span class="sxs-lookup"><span data-stu-id="bf156-199">This implementation uses **MemoryStream** to retrieve the image and prepare it for streaming to the browser.</span></span> <span data-ttu-id="bf156-200">Nullánál indítja el a streamelési pozíciót, jpeg-ként deklarálja a stream tartalmát, valamint streameli az információt.</span><span class="sxs-lookup"><span data-stu-id="bf156-200">It starts the stream position at zero, declares the stream content as a jpeg, and streams the information.</span></span>
8. <span data-ttu-id="bf156-201">A **Build** (Létrehozás) menüben kattintson a **Build Solution** (Megoldás létrehozása) elemre.</span><span class="sxs-lookup"><span data-stu-id="bf156-201">From the **Build** menu, click **Build Solution**.</span></span>

### <a name="to-define-the-configuration-for-running-the-web-service-on-service-bus"></a><span data-ttu-id="bf156-202">A konfiguráció meghatározása a webszolgáltatás Service Buson való futtatásához</span><span class="sxs-lookup"><span data-stu-id="bf156-202">To define the configuration for running the web service on Service Bus</span></span>
1. <span data-ttu-id="bf156-203">A **Solution Explorerben** (Megoldáskezelőben) kattintson duplán az **App.config** fájlra a Visual Studio-szerkesztőben való megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="bf156-203">In **Solution Explorer**, double-click **App.config** to open it in the Visual Studio editor.</span></span>
   
    <span data-ttu-id="bf156-204">A **App.config** fájl tartalmazza a szolgáltatás nevét, végpontját (Ez azt jelenti, hogy a helyet Azure továbbítási közzétesz az ügyfelek és a gazdáknak az egymással való kommunikációhoz) és kötés (a kommunikációhoz használt protokoll típusát).</span><span class="sxs-lookup"><span data-stu-id="bf156-204">The **App.config** file includes the service name, endpoint (that is, the location Azure Relay exposes for clients and hosts to communicate with each other), and binding (the type of protocol that is used to communicate).</span></span> <span data-ttu-id="bf156-205">A fő különbség itt az, hogy a konfigurált szolgáltatásvégpont hivatkozik egy [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) kötés.</span><span class="sxs-lookup"><span data-stu-id="bf156-205">The main difference here is that the configured service endpoint refers to a [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) binding.</span></span>
2. <span data-ttu-id="bf156-206">A `<system.serviceModel>` XML-elem egy WCF-elem, amely egy vagy több szolgáltatást határoz meg.</span><span class="sxs-lookup"><span data-stu-id="bf156-206">The `<system.serviceModel>` XML element is a WCF element that defines one or more services.</span></span> <span data-ttu-id="bf156-207">Itt a szolgáltatás nevének és végpontjának meghatározására szolgál.</span><span class="sxs-lookup"><span data-stu-id="bf156-207">Here, it is used to define the service name and endpoint.</span></span> <span data-ttu-id="bf156-208">A `<system.serviceModel>` elem aljánál (de még a `<bindings>` elemen belül), adjon hozzá egy, az alábbi tartalommal rendelkező `<system.serviceModel>` elemet.</span><span class="sxs-lookup"><span data-stu-id="bf156-208">At the bottom of the `<system.serviceModel>` element (but still within `<system.serviceModel>`), add a `<bindings>` element that has the following content.</span></span> <span data-ttu-id="bf156-209">Ez határozza meg az alkalmazásban használt kötéseket.</span><span class="sxs-lookup"><span data-stu-id="bf156-209">This defines the bindings used in the application.</span></span> <span data-ttu-id="bf156-210">Meghatározhat több kötést, de ez az oktatóanyag csak egyet határoz meg.</span><span class="sxs-lookup"><span data-stu-id="bf156-210">You can define multiple bindings, but for this tutorial you are defining only one.</span></span>
   
    ```xml
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```
   
    <span data-ttu-id="bf156-211">Az előző kód meghatároz egy WCF továbbító [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) kötés **relayClientAuthenticationType** beállítása **nincs**.</span><span class="sxs-lookup"><span data-stu-id="bf156-211">The previous code defines a WCF Relay [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) binding with **relayClientAuthenticationType** set to **None**.</span></span> <span data-ttu-id="bf156-212">Ez a beállítás jelöli, ha egy, a kötést használó végpont nem igényel ügyfél-hitelesítőt.</span><span class="sxs-lookup"><span data-stu-id="bf156-212">This setting indicates that an endpoint using this binding does not require a client credential.</span></span>
3. <span data-ttu-id="bf156-213">A `<bindings>` elem után adjon hozzá egy `<services>` elemet.</span><span class="sxs-lookup"><span data-stu-id="bf156-213">After the `<bindings>` element, add a `<services>` element.</span></span> <span data-ttu-id="bf156-214">A kötésekhez hasonlóan megadhat több szolgáltatást is egyetlen konfigurációs fájlon belül.</span><span class="sxs-lookup"><span data-stu-id="bf156-214">Similar to the bindings, you can define multiple services in a single configuration file.</span></span> <span data-ttu-id="bf156-215">Ez az oktatóanyag azonban csak egyet ad meg.</span><span class="sxs-lookup"><span data-stu-id="bf156-215">However, for this tutorial, you define only one.</span></span>
   
    ```xml
    <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
            <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IImageContract"
                    binding="webHttpRelayBinding"
                    bindingConfiguration="default"
                    behaviorConfiguration="sbTokenProvider"
                    address="" />
        </service>
    </services>
    ```
   
    <span data-ttu-id="bf156-216">Ez a lépés konfigurál egy szolgáltatást, amely a korábban meghatározott alapértelmezett **webHttpRelayBinding** elemet használja.</span><span class="sxs-lookup"><span data-stu-id="bf156-216">This step configures a service that uses the previously defined default **webHttpRelayBinding**.</span></span> <span data-ttu-id="bf156-217">Használja az alapértelmezett **sbTokenProvider** elemet is, amely a következő lépésben lesz meghatározva.</span><span class="sxs-lookup"><span data-stu-id="bf156-217">It also uses the default **sbTokenProvider**, which is defined in the next step.</span></span>
4. <span data-ttu-id="bf156-218">Után a `<services>` elemet, hozzon létre egy `<behaviors>` elem a következő tartalommal, cseréje a "SAS_KEY" a *közös hozzáférésű Jogosultságkód* (SAS) kulcsára korábban már beolvasták őket az [Azure-portálon] [Azure portal].</span><span class="sxs-lookup"><span data-stu-id="bf156-218">After the `<services>` element, create a `<behaviors>` element with the following content, replacing "SAS_KEY" with the *Shared Access Signature* (SAS) key you previously obtained from the [Azure portal][Azure portal].</span></span>
   
    ```xml
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="YOUR_SAS_KEY" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
            </endpointBehaviors>
            <serviceBehaviors>
                <behavior name="default">
                    <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
                </behavior>
            </serviceBehaviors>
    </behaviors>
    ```
5. <span data-ttu-id="bf156-219">Még mindig az App.config fájlban, az `<appSettings>` elemben cserélje le a teljes kapcsolati karakterlánc értékét a korábban a portálról beszerzett kapcsolati karakterláncéra.</span><span class="sxs-lookup"><span data-stu-id="bf156-219">Still in App.config, in the `<appSettings>` element, replace the entire connection string value with the connection string you previously obtained from the portal.</span></span> 
   
    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SAS_KEY"/>
    </appSettings>
    ```
6. <span data-ttu-id="bf156-220">A teljes megoldás létrehozásához a **Build** (Létrehozás) menüben kattintson a **Build Solution** (Megoldás létrehozása) elemre.</span><span class="sxs-lookup"><span data-stu-id="bf156-220">From the **Build** menu, click **Build Solution** to build the entire solution.</span></span>

### <a name="example"></a><span data-ttu-id="bf156-221">Példa</span><span class="sxs-lookup"><span data-stu-id="bf156-221">Example</span></span>
<span data-ttu-id="bf156-222">Az alábbi kód bemutatja a szerződés és a szolgáltatás megvalósítását egy REST-alapú szolgáltatáshoz, amely a **WebHttpRelayBinding** kötés használatával fut a Service Buson.</span><span class="sxs-lookup"><span data-stu-id="bf156-222">The following code shows the contract and service implementation for a REST-based service that is running on  Service Bus using the **WebHttpRelayBinding** binding.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{


    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

<span data-ttu-id="bf156-223">A következő példa a szolgáltatáshoz társított App.config fájlt mutatja be.</span><span class="sxs-lookup"><span data-stu-id="bf156-223">The following example shows the App.config file associated with the service.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove the ones they don't need. -->
            <behaviorExtensions>
                <add name="connectionStatusBehavior"
                    type="Microsoft.ServiceBus.Configuration.ConnectionStatusElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="transportClientEndpointBehavior"
                    type="Microsoft.ServiceBus.Configuration.TransportClientEndpointBehaviorElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="serviceRegistrySettings"
                    type="Microsoft.ServiceBus.Configuration.ServiceRegistrySettingsElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </behaviorExtensions>
            <bindingElementExtensions>
                <add name="netMessagingTransport"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingTransportExtensionElement, Microsoft.ServiceBus,  Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="tcpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.TcpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpsRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpsRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="onewayRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.RelayedOnewayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingElementExtensions>
            <bindingExtensions>
                <add name="basicHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.BasicHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="webHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WebHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="ws2007HttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WS2007HttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netOnewayRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetOnewayRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netEventRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetEventRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netMessagingBinding"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingExtensions>
        </extensions>
      <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
          <binding name="default">
            <security relayClientAuthenticationType="None" />
          </binding>
        </webHttpRelayBinding>
      </bindings>
      <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
          <endpoint name="RelayEndpoint"
                  contract="Microsoft.ServiceBus.Samples.IImageContract"
                  binding="webHttpRelayBinding"
                  bindingConfiguration="default"
                  behaviorConfiguration="sbTokenProvider"
                  address="" />
        </service>
      </services>
      <behaviors>
        <endpointBehaviors>
          <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
              <tokenProvider>
                <sharedAccessSignature keyName="RootManageSharedAccessKey" key="YOUR_SAS_KEY" />
              </tokenProvider>
            </transportClientEndpointBehavior>
          </behavior>
        </endpointBehaviors>
        <serviceBehaviors>
          <behavior name="default">
            <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
          </behavior>
        </serviceBehaviors>
      </behaviors>
    </system.serviceModel>
    <appSettings>
        <!-- Service Bus specific app setings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey="YOUR_SAS_KEY"/>
    </appSettings>
</configuration>
```

## <a name="step-4-host-the-rest-based-wcf-service-to-use-azure-relay"></a><span data-ttu-id="bf156-224">4. lépés: A REST-alapú WCF szolgáltatás Azure továbbítási üzemeltetéséhez</span><span class="sxs-lookup"><span data-stu-id="bf156-224">Step 4: Host the REST-based WCF service to use Azure Relay</span></span>
<span data-ttu-id="bf156-225">Ebben a lépésben egy konzolalkalmazás használatával a WCF továbbító egy webszolgáltatás futtatását ismerteti.</span><span class="sxs-lookup"><span data-stu-id="bf156-225">This step describes how to run a web service using a console application with WCF Relay.</span></span> <span data-ttu-id="bf156-226">Az ebben a lépésben írt kód teljes listája megtalálható az eljárást követő példában.</span><span class="sxs-lookup"><span data-stu-id="bf156-226">A complete listing of the code written in this step is provided in the example following the procedure.</span></span>

### <a name="to-create-a-base-address-for-the-service"></a><span data-ttu-id="bf156-227">Alapszintű cím létrehozása a szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="bf156-227">To create a base address for the service</span></span>
1. <span data-ttu-id="bf156-228">Az a `Main()` függvény deklarációjában, hozzon létre egy változót a projekt névterének tárolásához.</span><span class="sxs-lookup"><span data-stu-id="bf156-228">In the `Main()` function declaration, create a variable to store the namespace of your project.</span></span> <span data-ttu-id="bf156-229">Győződjön meg arról, hogy `yourNamespace` a korábban létrehozott továbbítási névtér nevét.</span><span class="sxs-lookup"><span data-stu-id="bf156-229">Make sure to replace `yourNamespace` with the name of the Relay namespace you created previously.</span></span>
   
    ```csharp
    string serviceNamespace = "yourNamespace";
    ```
    <span data-ttu-id="bf156-230">A Service Bus a névtér nevét használva létrehoz egy egyedi URI-t.</span><span class="sxs-lookup"><span data-stu-id="bf156-230">Service Bus uses the name of your namespace to create a unique URI.</span></span>
2. <span data-ttu-id="bf156-231">Hozzon létre egy `Uri`-példányt a névtéren alapuló szolgáltatás alapszintű címéhez.</span><span class="sxs-lookup"><span data-stu-id="bf156-231">Create a `Uri` instance for the base address of the service that is based on the namespace.</span></span>
   
    ```csharp
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="to-create-and-configure-the-web-service-host"></a><span data-ttu-id="bf156-232">A webszolgáltatásgazda létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bf156-232">To create and configure the web service host</span></span>
* <span data-ttu-id="bf156-233">Hozza létre a webszolgáltatás gazdáját a szakaszban korábban létrehozott URI-cím használatával.</span><span class="sxs-lookup"><span data-stu-id="bf156-233">Create the web service host, using the URI address created earlier in this section.</span></span>
  
    ```csharp
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    <span data-ttu-id="bf156-234">A szolgáltatásgazda az a WCF-objektum, amely a gazdaalkalmazást példányosítja.</span><span class="sxs-lookup"><span data-stu-id="bf156-234">The service host is the WCF object that instantiates the host application.</span></span> <span data-ttu-id="bf156-235">Ez a példa továbbítja a létrehozni kívánt gazda típusát (**ImageService**), valamint a címet, ahol közzé kívánja tenni a gazdaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bf156-235">This example passes it the type of host you want to create (an **ImageService**), and also the address at which you want to expose the host application.</span></span>

### <a name="to-run-the-web-service-host"></a><span data-ttu-id="bf156-236">A webszolgáltatásgazda futtatása</span><span class="sxs-lookup"><span data-stu-id="bf156-236">To run the web service host</span></span>
1. <span data-ttu-id="bf156-237">Nyissa meg a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="bf156-237">Open the service.</span></span>
   
    ```csharp
    host.Open();
    ```
    <span data-ttu-id="bf156-238">A szolgáltatás jelenleg fut.</span><span class="sxs-lookup"><span data-stu-id="bf156-238">The service is now running.</span></span>
2. <span data-ttu-id="bf156-239">Megjelenít egy üzenetet, amely jelzi, hogy a szolgáltatás fut, valamint a szolgáltatás leállításának módját.</span><span class="sxs-lookup"><span data-stu-id="bf156-239">Display a message indicating that the service is running, and how to stop the service.</span></span>
   
    ```csharp
    Console.WriteLine("Copy the following address into a browser to see the image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```
3. <span data-ttu-id="bf156-240">Ha kész, zárja be a szolgáltatásgazdát.</span><span class="sxs-lookup"><span data-stu-id="bf156-240">When finished, close the service host.</span></span>
   
    ```csharp
    host.Close();
    ```

## <a name="example"></a><span data-ttu-id="bf156-241">Példa</span><span class="sxs-lookup"><span data-stu-id="bf156-241">Example</span></span>
<span data-ttu-id="bf156-242">Az alábbi példa tartalmazza a szolgáltatási szerződést és a megvalósítását az oktatóanyag előző lépéseiből, és egy konzolalkalmazásban működteti a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="bf156-242">The following example includes the service contract and implementation from previous steps in the tutorial and hosts the service in a console application.</span></span> <span data-ttu-id="bf156-243">Fordítsa le az alábbi kódot egy ImageListener.exe nevű végrehajtható fájlba.</span><span class="sxs-lookup"><span data-stu-id="bf156-243">Compile the following code into an executable named ImageListener.exe.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string serviceNamespace = "InsertServiceNamespaceHere";
            Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");

            WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
            host.Open();

            Console.WriteLine("Copy the following address into a browser to see the image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### <a name="compiling-the-code"></a><span data-ttu-id="bf156-244">A kód fordítása</span><span class="sxs-lookup"><span data-stu-id="bf156-244">Compiling the code</span></span>
<span data-ttu-id="bf156-245">A megoldás létrehozása után az alábbi lépéseket követve futtathatja az alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="bf156-245">After building the solution, do the following to run the application:</span></span>

1. <span data-ttu-id="bf156-246">A szolgáltatás futtatásához nyomja le az **F5** billentyűt, vagy lépjen a végrehajtható fájlhoz (ImageListener\bin\Debug\ImageListener.exe).</span><span class="sxs-lookup"><span data-stu-id="bf156-246">Press **F5**, or browse to the executable file location (ImageListener\bin\Debug\ImageListener.exe), to run the service.</span></span> <span data-ttu-id="bf156-247">Nem állítsa le az alkalmazás futását, mert szükség van rá a következő lépés elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="bf156-247">Keep the app running, as it's required to perform the next step.</span></span>
2. <span data-ttu-id="bf156-248">A kép megtekintéséhez másolja és illessze be a címet a parancssorból egy böngészőbe.</span><span class="sxs-lookup"><span data-stu-id="bf156-248">Copy and paste the address from the command prompt into a browser to see the image.</span></span>
3. <span data-ttu-id="bf156-249">Ha kész van, a parancssori ablakban az **Enter** billentyűt lenyomva zárja be az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bf156-249">When you are finished, press **Enter** in the command prompt window to close the app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bf156-250">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bf156-250">Next steps</span></span>
<span data-ttu-id="bf156-251">Most, hogy létrehozott egy alkalmazás, amely a Service Bus relay szolgáltatás használja, tekintse meg a következő cikkekben olvashat Azure továbbítási:</span><span class="sxs-lookup"><span data-stu-id="bf156-251">Now that you've built an application that uses the Service Bus relay service, see the following articles to learn more about Azure Relay:</span></span>

* [<span data-ttu-id="bf156-252">Az Azure Service Bus-architektúra áttekintése</span><span class="sxs-lookup"><span data-stu-id="bf156-252">Azure Service Bus architectural overview</span></span>](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
* [<span data-ttu-id="bf156-253">Az Azure Relay áttekintése</span><span class="sxs-lookup"><span data-stu-id="bf156-253">Azure Relay overview</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="bf156-254">A WCF továbbító szolgáltatás a .NET használatával</span><span class="sxs-lookup"><span data-stu-id="bf156-254">How to use the WCF relay service with .NET</span></span>](relay-wcf-dotnet-get-started.md)

[Azure portal]: https://portal.azure.com
