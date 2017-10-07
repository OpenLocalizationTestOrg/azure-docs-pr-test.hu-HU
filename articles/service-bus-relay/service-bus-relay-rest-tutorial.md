---
title: "Azure Relay használatával aaaService Bus REST oktatóanyag |} Microsoft Docs"
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
ms.openlocfilehash: b68650993a0390e7cef891ccb4236095cd86d4c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-wcf-relay-rest-tutorial"></a><span data-ttu-id="e2078-103">Az Azure WCF Relay REST-oktatóanyaga</span><span class="sxs-lookup"><span data-stu-id="e2078-103">Azure WCF Relay REST tutorial</span></span>

<span data-ttu-id="e2078-104">Ez az oktatóanyag leírja, hogyan toobuild egy egyszerű Azure-továbbítási fogadó alkalmazás REST-alapú felületet.</span><span class="sxs-lookup"><span data-stu-id="e2078-104">This tutorial describes how toobuild a simple Azure Relay host application that exposes a REST-based interface.</span></span> <span data-ttu-id="e2078-105">A REST lehetővé teszi egy webes ügyfél, például egy webes böngésző, a Service Bus alkalmazásprogramozási keresztül HTTP-kérelmek tooaccess hello.</span><span class="sxs-lookup"><span data-stu-id="e2078-105">REST enables a web client, such as a web browser, tooaccess hello Service Bus APIs through HTTP requests.</span></span>

<span data-ttu-id="e2078-106">hello oktatóprogram hello Windows Communication Foundation (WCF) REST programozási modell tooconstruct REST-szolgáltatást a Service Buson.</span><span class="sxs-lookup"><span data-stu-id="e2078-106">hello tutorial uses hello Windows Communication Foundation (WCF) REST programming model tooconstruct a REST service on Service Bus.</span></span> <span data-ttu-id="e2078-107">További információkért lásd: [WCF REST programozási modell](/dotnet/framework/wcf/feature-details/wcf-web-http-programming-model) és [tervezése és megvalósítása szolgáltatások](/dotnet/framework/wcf/designing-and-implementing-services) a hello WCF-dokumentáció.</span><span class="sxs-lookup"><span data-stu-id="e2078-107">For more information, see [WCF REST Programming Model](/dotnet/framework/wcf/feature-details/wcf-web-http-programming-model) and [Designing and Implementing Services](/dotnet/framework/wcf/designing-and-implementing-services) in hello WCF documentation.</span></span>

## <a name="step-1-create-a-namespace"></a><span data-ttu-id="e2078-108">1. lépés: Névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="e2078-108">Step 1: Create a namespace</span></span>

<span data-ttu-id="e2078-109">az Azure-továbbítási funkciók toobegin használatával hello, először létre kell hoznia egy szolgáltatásnévteret.</span><span class="sxs-lookup"><span data-stu-id="e2078-109">toobegin using hello relay features in Azure, you must first create a service namespace.</span></span> <span data-ttu-id="e2078-110">A névtér egy hatókörkezelési tárolót biztosít az Azure erőforrásainak címzéséhez az alkalmazáson belül.</span><span class="sxs-lookup"><span data-stu-id="e2078-110">A namespace provides a scoping container for addressing Azure resources within your application.</span></span> <span data-ttu-id="e2078-111">Hajtsa végre a hello [utasításokat itt](relay-create-namespace-portal.md) toocreate továbbítási névtér.</span><span class="sxs-lookup"><span data-stu-id="e2078-111">Follow hello [instructions here](relay-create-namespace-portal.md) toocreate a Relay namespace.</span></span>

## <a name="step-2-define-a-rest-based-wcf-service-contract-toouse-with-azure-relay"></a><span data-ttu-id="e2078-112">2. lépés: Egy REST-alapú WCF szolgáltatási szerződés toouse az Azure-továbbítási meghatározása</span><span class="sxs-lookup"><span data-stu-id="e2078-112">Step 2: Define a REST-based WCF service contract toouse with Azure Relay</span></span>

<span data-ttu-id="e2078-113">A WCF REST-stílusú szolgáltatás létrehozásakor meg kell adnia a hello szerződés.</span><span class="sxs-lookup"><span data-stu-id="e2078-113">When you create a WCF REST-style service, you must define hello contract.</span></span> <span data-ttu-id="e2078-114">hello szerződés Megadja, hogy milyen műveleteket hello gazdagép támogatja.</span><span class="sxs-lookup"><span data-stu-id="e2078-114">hello contract specifies what operations hello host supports.</span></span> <span data-ttu-id="e2078-115">A szolgáltatási művelet tekinthető webszolgáltatási módszernek.</span><span class="sxs-lookup"><span data-stu-id="e2078-115">A service operation can be thought of as a web service method.</span></span> <span data-ttu-id="e2078-116">A szerződések a C++, a C# vagy a Visual Basic felület meghatározásával jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="e2078-116">Contracts are created by defining a C++, C#, or Visual Basic interface.</span></span> <span data-ttu-id="e2078-117">Hello felület minden metódusa tooa konkrét szolgáltatási műveletnek felel meg.</span><span class="sxs-lookup"><span data-stu-id="e2078-117">Each method in hello interface corresponds tooa specific service operation.</span></span> <span data-ttu-id="e2078-118">Hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribútum alkalmazott tooeach illesztőfelület kell, és hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribútum alkalmazott tooeach művelet lehet.</span><span class="sxs-lookup"><span data-stu-id="e2078-118">hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribute must be applied tooeach interface, and hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribute must be applied tooeach operation.</span></span> <span data-ttu-id="e2078-119">Ha a metódus egy illesztőfelületben, amely rendelkezik hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) nincs hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), metódus nem lesz közzétéve.</span><span class="sxs-lookup"><span data-stu-id="e2078-119">If a method in an interface that has hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) does not have hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), that method is not exposed.</span></span> <span data-ttu-id="e2078-120">hello a feladatokhoz használt kód hello a hello eljárást követő példában.</span><span class="sxs-lookup"><span data-stu-id="e2078-120">hello code used for these tasks is shown in hello example following hello procedure.</span></span>

<span data-ttu-id="e2078-121">hello elsődleges különbség a WCF szerződés és egy REST-stílusú szerződés között az hello hozzáadása egy tulajdonság toohello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx).</span><span class="sxs-lookup"><span data-stu-id="e2078-121">hello primary difference between a WCF contract and a REST-style contract is hello addition of a property toohello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx).</span></span> <span data-ttu-id="e2078-122">Ez a tulajdonság lehetővé teszi, hogy a hello felület tooa metódusban metódus toomap hello felület másik oldalán.</span><span class="sxs-lookup"><span data-stu-id="e2078-122">This property enables you toomap a method in your interface tooa method on hello other side of hello interface.</span></span> <span data-ttu-id="e2078-123">Ebben az esetben használjuk [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) toolink egy GET metódus tooHTTP.</span><span class="sxs-lookup"><span data-stu-id="e2078-123">In this case, we will use [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) toolink a method tooHTTP GET.</span></span> <span data-ttu-id="e2078-124">Ez lehetővé teszi, hogy a Service Bus tooaccurately kérje le, majd értelmezhetők toohello felület küldött parancsokat.</span><span class="sxs-lookup"><span data-stu-id="e2078-124">This allows Service Bus tooaccurately retrieve and interpret commands sent toohello interface.</span></span>

### <a name="toocreate-a-contract-with-an-interface"></a><span data-ttu-id="e2078-125">a szerződés felülettel toocreate</span><span class="sxs-lookup"><span data-stu-id="e2078-125">toocreate a contract with an interface</span></span>

1. <span data-ttu-id="e2078-126">Nyissa meg a Visual Studiót rendszergazdaként: kattintson a jobb gombbal hello programra hello **Start** menüben, majd kattintson **Futtatás rendszergazdaként**.</span><span class="sxs-lookup"><span data-stu-id="e2078-126">Open Visual Studio as an administrator: right-click hello program in hello **Start** menu, and then click **Run as administrator**.</span></span>
2. <span data-ttu-id="e2078-127">Hozzon létre új egy új konzolalkalmazás-projektet.</span><span class="sxs-lookup"><span data-stu-id="e2078-127">Create a new console application project.</span></span> <span data-ttu-id="e2078-128">Kattintson a hello **fájl** menüre, majd válassza **új**, majd jelölje be **projekt**.</span><span class="sxs-lookup"><span data-stu-id="e2078-128">Click hello **File** menu and select **New**, then select **Project**.</span></span> <span data-ttu-id="e2078-129">A hello **új projekt** párbeszédpanel, kattintson a **Visual C#**, jelölje be hello **Console Application** sablont, és adjon neki nevet **ImageListener**.</span><span class="sxs-lookup"><span data-stu-id="e2078-129">In hello **New Project** dialog box, click **Visual C#**, select hello **Console Application** template, and name it **ImageListener**.</span></span> <span data-ttu-id="e2078-130">Hello alapértelmezett **hely**.</span><span class="sxs-lookup"><span data-stu-id="e2078-130">Use hello default **Location**.</span></span> <span data-ttu-id="e2078-131">Kattintson a **OK** toocreate hello projekt.</span><span class="sxs-lookup"><span data-stu-id="e2078-131">Click **OK** toocreate hello project.</span></span>
3. <span data-ttu-id="e2078-132">C# projekt esetében a Visual Studio létrehoz egy `Program.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="e2078-132">For a C# project, Visual Studio creates a `Program.cs` file.</span></span> <span data-ttu-id="e2078-133">Ez az osztály tartalmaz egy üres `Main()` metódus, a konzol alkalmazás projekt toobuild megfelelően szükséges.</span><span class="sxs-lookup"><span data-stu-id="e2078-133">This class contains an empty `Main()` method, required for a console application project toobuild correctly.</span></span>
4. <span data-ttu-id="e2078-134">Adja hozzá a hivatkozások tooService busz és **System.ServiceModel.dll** toohello projekt hello Service Bus NuGet csomag telepítésével.</span><span class="sxs-lookup"><span data-stu-id="e2078-134">Add references tooService Bus and **System.ServiceModel.dll** toohello project by installing hello Service Bus NuGet package.</span></span> <span data-ttu-id="e2078-135">Ez a csomag automatikusan hozzáadja a hivatkozások toohello Service Bus-könyvtárakhoz, valamint a hello WCF **System.ServiceModel**.</span><span class="sxs-lookup"><span data-stu-id="e2078-135">This package automatically adds references toohello Service Bus libraries, as well as hello WCF **System.ServiceModel**.</span></span> <span data-ttu-id="e2078-136">A Megoldáskezelőben kattintson a jobb gombbal hello **ImageListener** projektre, és kattintson a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="e2078-136">In Solution Explorer, right-click hello **ImageListener** project, and then click **Manage NuGet Packages**.</span></span> <span data-ttu-id="e2078-137">Kattintson a hello **Tallózás** lapra, és keresse meg `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="e2078-137">Click hello **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="e2078-138">Kattintson a **telepítése**, és el kell fogadnia a használati feltételek hello.</span><span class="sxs-lookup"><span data-stu-id="e2078-138">Click **Install**, and accept hello terms of use.</span></span>
5. <span data-ttu-id="e2078-139">Explicit módon adja a hivatkozást túl**System.ServiceModel.Web.dll** toohello projekt:</span><span class="sxs-lookup"><span data-stu-id="e2078-139">You must explicitly add a reference too**System.ServiceModel.Web.dll** toohello project:</span></span>
   
    <span data-ttu-id="e2078-140">a.</span><span class="sxs-lookup"><span data-stu-id="e2078-140">a.</span></span> <span data-ttu-id="e2078-141">A Megoldáskezelőben kattintson a jobb gombbal hello **hivatkozások** mappa hello projekt mappába, majd **hivatkozás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="e2078-141">In Solution Explorer, right-click hello **References** folder under hello project folder and then click **Add Reference**.</span></span>
   
    <span data-ttu-id="e2078-142">b.</span><span class="sxs-lookup"><span data-stu-id="e2078-142">b.</span></span> <span data-ttu-id="e2078-143">A hello **hivatkozás hozzáadása** párbeszédpanelen kattintson hello **keretrendszer** lap bal oldalán hello és hello **keresési** mezőbe írja be **System.ServiceModel.Web** .</span><span class="sxs-lookup"><span data-stu-id="e2078-143">In hello **Add Reference** dialog box, click hello **Framework** tab on hello left-hand side and in hello **Search** box, type **System.ServiceModel.Web**.</span></span> <span data-ttu-id="e2078-144">Jelölje be hello **System.ServiceModel.Web** jelölőnégyzetet, majd kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="e2078-144">Select hello **System.ServiceModel.Web** check box, then click **OK**.</span></span>
6. <span data-ttu-id="e2078-145">Adja hozzá a következő hello `using` utasításokat a Program.cs fájl hello hello tetején.</span><span class="sxs-lookup"><span data-stu-id="e2078-145">Add hello following `using` statements at hello top of hello Program.cs file.</span></span>
   
    ```csharp
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```
   
    <span data-ttu-id="e2078-146">[System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) hello névtér, amely lehetővé teszi a programozott hozzáférést toobasic szolgáltatások WCF.</span><span class="sxs-lookup"><span data-stu-id="e2078-146">[System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) is hello namespace that enables programmatic access toobasic features of WCF.</span></span> <span data-ttu-id="e2078-147">WCF továbbító hello objektumok és attribútumok WCF toodefine szolgáltatási szerződések használja.</span><span class="sxs-lookup"><span data-stu-id="e2078-147">WCF Relay uses many of hello objects and attributes of WCF toodefine service contracts.</span></span> <span data-ttu-id="e2078-148">A relay alkalmazásban a legtöbb fogja használni ehhez a névtérhez.</span><span class="sxs-lookup"><span data-stu-id="e2078-148">You will use this namespace in most of your relay applications.</span></span> <span data-ttu-id="e2078-149">Hasonlóképpen [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) segítségével meghatározhatja a hello csatornát, amely hello objektum, amelyen keresztül kommunikálhat a Azure és a hello ügyfél webböngészőjével.</span><span class="sxs-lookup"><span data-stu-id="e2078-149">Similarly, [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) helps define hello channel, which is hello object through which you communicate with Azure Relay and hello client web browser.</span></span> <span data-ttu-id="e2078-150">Végezetül [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) hello típusokat, amelyek lehetővé teszik a webes alkalmazások toocreate tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="e2078-150">Finally, [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) contains hello types that enable you toocreate web-based applications.</span></span>
7. <span data-ttu-id="e2078-151">Nevezze át a hello `ImageListener` névtér túl**Microsoft.ServiceBus.Samples**.</span><span class="sxs-lookup"><span data-stu-id="e2078-151">Rename hello `ImageListener` namespace too**Microsoft.ServiceBus.Samples**.</span></span>
   
    ```csharp
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```
8. <span data-ttu-id="e2078-152">Közvetlenül után hello hello névtér-deklaráció kapcsos zárójelet megnyitása, adja meg a nevű új kapcsolat **IImageContract** és hello alkalmazása **ServiceContractAttribute** attribútum toohello illesztőfelület egy az érték `http://samples.microsoft.com/ServiceModel/Relay/`.</span><span class="sxs-lookup"><span data-stu-id="e2078-152">Directly after hello opening curly brace of hello namespace declaration, define a new interface named **IImageContract** and apply hello **ServiceContractAttribute** attribute toohello interface with a value of `http://samples.microsoft.com/ServiceModel/Relay/`.</span></span> <span data-ttu-id="e2078-153">hello névtér értéke különbözik hello hatókör a kód egészében használt hello névtér.</span><span class="sxs-lookup"><span data-stu-id="e2078-153">hello namespace value differs from hello namespace that you use throughout hello scope of your code.</span></span> <span data-ttu-id="e2078-154">hello névtér értéke egyedi azonosítóként van használatban a szerződéshez, és rendelkeznie kell a fájlverzió-információkat.</span><span class="sxs-lookup"><span data-stu-id="e2078-154">hello namespace value is used as a unique identifier for this contract, and should have version information.</span></span> <span data-ttu-id="e2078-155">További információ: [Service Versioning](http://go.microsoft.com/fwlink/?LinkID=180498) (Szolgáltatás verziószámozása).</span><span class="sxs-lookup"><span data-stu-id="e2078-155">For more information, see [Service Versioning](http://go.microsoft.com/fwlink/?LinkID=180498).</span></span> <span data-ttu-id="e2078-156">Hello névtér explicit megadásával megakadályozza, hogy az hello alapértelmezett névtér érték kerüljön toohello szerződés neve.</span><span class="sxs-lookup"><span data-stu-id="e2078-156">Specifying hello namespace explicitly prevents hello default namespace value from being added toohello contract name.</span></span>
   
    ```csharp
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```
9. <span data-ttu-id="e2078-157">Hello belül `IImageContract` csatoló, deklaráljon egy metódust hello egyetlen műveletben hello `IImageContract` szerződés tesz elérhetővé a hello felületet, és alkalmazza a hello `OperationContractAttribute` attribútumot, amelyet az tooexpose hello nyilvános Service Bus részeként toohello módszer szerződés.</span><span class="sxs-lookup"><span data-stu-id="e2078-157">Within hello `IImageContract` interface, declare a method for hello single operation hello `IImageContract` contract exposes in hello interface and apply hello `OperationContractAttribute` attribute toohello method that you want tooexpose as part of hello public Service Bus contract.</span></span>
   
    ```csharp
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```
10. <span data-ttu-id="e2078-158">A hello **OperationContract** attribútumot, vegye fel a hello **WebGet** érték.</span><span class="sxs-lookup"><span data-stu-id="e2078-158">In hello **OperationContract** attribute, add hello **WebGet** value.</span></span>
    
    ```csharp
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```
    
    <span data-ttu-id="e2078-159">Ennek során, így lehetővé teszi, hogy a továbbítási szolgáltatás tooroute túl HTTP GET kérelmek hello`GetImage`, és visszatérési értékek a tootranslate hello `GetImage` egy HTTP GETRESPONSE válaszba.</span><span class="sxs-lookup"><span data-stu-id="e2078-159">Doing so enables hello relay service tooroute HTTP GET requests too`GetImage`, and tootranslate hello return values of `GetImage` into an HTTP GETRESPONSE reply.</span></span> <span data-ttu-id="e2078-160">Hello oktatóanyag későbbi részében szüksége lesz egy webes böngésző tooaccess ezt a módszert, és a toodisplay hello kép hello böngészőben.</span><span class="sxs-lookup"><span data-stu-id="e2078-160">Later in hello tutorial, you will use a web browser tooaccess this method, and toodisplay hello image in hello browser.</span></span>
11. <span data-ttu-id="e2078-161">Hello után közvetlenül `IImageContract` definícióját, deklaráljon egy csatornát, amely mindkét hello örökli `IImageContract` és `IClientChannel` felületek.</span><span class="sxs-lookup"><span data-stu-id="e2078-161">Directly after hello `IImageContract` definition, declare a channel that inherits from both hello `IImageContract` and `IClientChannel` interfaces.</span></span>
    
    ```csharp
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```
    
    <span data-ttu-id="e2078-162">A csatorna egy olyan hello WCF-objektum, amelyen keresztül a hello szolgáltatás és az ügyfél át adatokat tooeach más.</span><span class="sxs-lookup"><span data-stu-id="e2078-162">A channel is hello WCF object through which hello service and client pass information tooeach other.</span></span> <span data-ttu-id="e2078-163">Később létrehozhat hello csatornát a gazdaalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e2078-163">Later, you will create hello channel in your host application.</span></span> <span data-ttu-id="e2078-164">Az Azure továbbítási akkor használja a csatorna toopass hello HTTP GET kérelmeket a hello böngésző tooyour **GetImage** végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="e2078-164">Azure Relay then uses this channel toopass hello HTTP GET requests from hello browser tooyour **GetImage** implementation.</span></span> <span data-ttu-id="e2078-165">hello továbbítási is használ az hello csatorna tootake hello **GetImage** ad vissza értéket, és azt jelenti azt, hogy az ügyfél böngészője hello HTTP getresponse értékre.</span><span class="sxs-lookup"><span data-stu-id="e2078-165">hello relay also uses hello channel tootake hello **GetImage** return value and translate it into an HTTP GETRESPONSE for hello client browser.</span></span>
12. <span data-ttu-id="e2078-166">A hello **Build** menüben kattintson a **megoldás fordítása** eddigi munkája pontosságát tooconfirm hello.</span><span class="sxs-lookup"><span data-stu-id="e2078-166">From hello **Build** menu, click **Build Solution** tooconfirm hello accuracy of your work so far.</span></span>

### <a name="example"></a><span data-ttu-id="e2078-167">Példa</span><span class="sxs-lookup"><span data-stu-id="e2078-167">Example</span></span>
<span data-ttu-id="e2078-168">a következő kód hello egy WCF továbbító szerződést meghatározó alapszintű felületet jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="e2078-168">hello following code shows a basic interface that defines a WCF Relay contract.</span></span>

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

## <a name="step-3-implement-a-rest-based-wcf-service-contract-toouse-service-bus"></a><span data-ttu-id="e2078-169">3. lépés: Egy REST-alapú WCF szolgáltatási szerződés toouse Service Bus megvalósítása</span><span class="sxs-lookup"><span data-stu-id="e2078-169">Step 3: Implement a REST-based WCF service contract toouse Service Bus</span></span>
<span data-ttu-id="e2078-170">REST-stílusú WCF továbbító szolgáltatás létrehozása megköveteli, hogy először létre kell hoznia hello szerződést, amelyet egy felület használatával.</span><span class="sxs-lookup"><span data-stu-id="e2078-170">Creating a REST-style WCF Relay service requires that you first create hello contract, which is defined by using an interface.</span></span> <span data-ttu-id="e2078-171">hello a következő lépésre tooimplement hello felületet.</span><span class="sxs-lookup"><span data-stu-id="e2078-171">hello next step is tooimplement hello interface.</span></span> <span data-ttu-id="e2078-172">Ez magában foglalja a nevű osztály létrehozása **ImageService** , amely megvalósítja a felhasználó által definiált hello **IImageContract** felületet.</span><span class="sxs-lookup"><span data-stu-id="e2078-172">This involves creating a class named **ImageService** that implements hello user-defined **IImageContract** interface.</span></span> <span data-ttu-id="e2078-173">Hello szerződés megvalósítása után egy App.config fájl segítségével hello felület majd konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e2078-173">After you implement hello contract, you then configure hello interface using an App.config file.</span></span> <span data-ttu-id="e2078-174">hello konfigurációs fájl hello alkalmazások, például hello neve hello szolgáltatást, hello hello szerződés és a protokoll, amely hello relay szolgáltatással használt toocommunicate hello típusú szükséges információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e2078-174">hello configuration file contains necessary information for hello application, such as hello name of hello service, hello name of hello contract, and hello type of protocol that is used toocommunicate with hello relay service.</span></span> <span data-ttu-id="e2078-175">a feladatokhoz használt hello kód hello eljárást követő hello példában valósul meg.</span><span class="sxs-lookup"><span data-stu-id="e2078-175">hello code used for these tasks is provided in hello example following hello procedure.</span></span>

<span data-ttu-id="e2078-176">Mivel az előző lépéseket hello, nincs nagyon kicsi a különbség a REST-stílusú szerződés és egy WCF továbbító szerződés megvalósítása között.</span><span class="sxs-lookup"><span data-stu-id="e2078-176">As with hello previous steps, there is very little difference between implementing a REST-style contract and a WCF Relay contract.</span></span>

### <a name="tooimplement-a-rest-style-service-bus-contract"></a><span data-ttu-id="e2078-177">tooimplement egy REST-stílusú Service Bus szerződés</span><span class="sxs-lookup"><span data-stu-id="e2078-177">tooimplement a REST-style Service Bus contract</span></span>
1. <span data-ttu-id="e2078-178">Hozzon létre egy új osztályt **ImageService** hello hello meghatározása után közvetlenül **IImageContract** felületet.</span><span class="sxs-lookup"><span data-stu-id="e2078-178">Create a new class named **ImageService** directly after hello definition of hello **IImageContract** interface.</span></span> <span data-ttu-id="e2078-179">Hello **ImageService** osztály megvalósít hello **IImageContract** felületet.</span><span class="sxs-lookup"><span data-stu-id="e2078-179">hello **ImageService** class implements hello **IImageContract** interface.</span></span>
   
    ```csharp
    class ImageService : IImageContract
    {
    }
    ```
    <span data-ttu-id="e2078-180">Hasonló tooother interfészes megvalósítások hello definition Megvalósíthat egy másik fájlban.</span><span class="sxs-lookup"><span data-stu-id="e2078-180">Similar tooother interface implementations, you can implement hello definition in a different file.</span></span> <span data-ttu-id="e2078-181">Azonban ebben az oktatóanyagban hello megvalósítása szerepel ugyanazon fájl, mint a felületdefiníció hello hello és `Main()` metódust.</span><span class="sxs-lookup"><span data-stu-id="e2078-181">However, for this tutorial, hello implementation appears in hello same file as hello interface definition and `Main()` method.</span></span>
2. <span data-ttu-id="e2078-182">Hello alkalmazása [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) toohello attribútum **IImageService** osztály tooindicate, amely hello osztály egy WCF szerződés megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="e2078-182">Apply hello [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) attribute toohello **IImageService** class tooindicate that hello class is an implementation of a WCF contract.</span></span>
   
    ```csharp
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```
   
    <span data-ttu-id="e2078-183">Amint azt korábban említettük, ez a névtér nem egy hagyományos névtér.</span><span class="sxs-lookup"><span data-stu-id="e2078-183">As mentioned previously, this namespace is not a traditional namespace.</span></span> <span data-ttu-id="e2078-184">Ehelyett az már hello hello szerződést azonosító WCF-architektúra része.</span><span class="sxs-lookup"><span data-stu-id="e2078-184">Instead, it is part of hello WCF architecture that identifies hello contract.</span></span> <span data-ttu-id="e2078-185">További információkért lásd: hello [az egyezmény nevének](https://msdn.microsoft.com/library/ms731045.aspx) hello WCF-dokumentáció témakörében.</span><span class="sxs-lookup"><span data-stu-id="e2078-185">For more information, see hello [Data Contract Names](https://msdn.microsoft.com/library/ms731045.aspx) topic in hello WCF documentation.</span></span>
3. <span data-ttu-id="e2078-186">Adjon hozzá egy .jpg képet tooyour projektet.</span><span class="sxs-lookup"><span data-stu-id="e2078-186">Add a .jpg image tooyour project.</span></span>  
   
    <span data-ttu-id="e2078-187">Ez a hello szolgáltatás megjelenít a fogadó böngészőben hello kép.</span><span class="sxs-lookup"><span data-stu-id="e2078-187">This is a picture that hello service displays in hello receiving browser.</span></span> <span data-ttu-id="e2078-188">Kattintson a jobb gombbal a projektre, majd kattintson az **Add** (Hozzáadás) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="e2078-188">Right-click your project, then click **Add**.</span></span> <span data-ttu-id="e2078-189">Ezután kattintson az **Existing Item** (Meglévő elem) elemre.</span><span class="sxs-lookup"><span data-stu-id="e2078-189">Then click **Existing Item**.</span></span> <span data-ttu-id="e2078-190">Használjon hello **meglévő elem hozzáadása** párbeszédpanel bezárásához toobrowse tooan megfelelő .jpg, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="e2078-190">Use hello **Add Existing Item** dialog box toobrowse tooan appropriate .jpg, and then click **Add**.</span></span>
   
    <span data-ttu-id="e2078-191">Hello fájl hozzáadásakor győződjön meg arról, hogy **minden fájl** hello legördülő lista következő toohello kiválasztott **fájlnév:** mező.</span><span class="sxs-lookup"><span data-stu-id="e2078-191">When adding hello file, make sure that **All Files** is selected in hello drop-down list next toohello **File name:** field.</span></span> <span data-ttu-id="e2078-192">hello rest oktatóanyag feltételezi, hogy hello hello lemezkép neve: "image.jpg".</span><span class="sxs-lookup"><span data-stu-id="e2078-192">hello rest of this tutorial assumes that hello name of hello image is "image.jpg".</span></span> <span data-ttu-id="e2078-193">Ha egy másik fájlba, fog hogy toorename hello lemezképet, vagy módosítsa a kód toocompensate.</span><span class="sxs-lookup"><span data-stu-id="e2078-193">If you have a different file, you will have toorename hello image, or change your code toocompensate.</span></span>
4. <span data-ttu-id="e2078-194">arról, hogy hello szolgáltatást futtató megtalálhatja hello képfájl, a toomake **Solution Explorer** kattintson a jobb gombbal a hello képfájl, majd kattintson az **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="e2078-194">toomake sure that hello running service can find hello image file, in **Solution Explorer** right-click hello image file, then click **Properties**.</span></span> <span data-ttu-id="e2078-195">A hello **tulajdonságok** panelen állítsa **tooOutput Directory másolása** túl**másolhatja, ha újabb**.</span><span class="sxs-lookup"><span data-stu-id="e2078-195">In hello **Properties** pane, set **Copy tooOutput Directory** too**Copy if newer**.</span></span>
5. <span data-ttu-id="e2078-196">Adja hozzá egy hivatkozást toohello **System.Drawing.dll** szerelvény toohello projektre, és is hozzáadhatók az hello alábbi társított `using` utasításokat.</span><span class="sxs-lookup"><span data-stu-id="e2078-196">Add a reference toohello **System.Drawing.dll** assembly toohello project, and also add hello following associated `using` statements.</span></span>  
   
    ```csharp
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```
6. <span data-ttu-id="e2078-197">A hello **ImageService** osztály, adja hozzá hello követően, hogy a terhelés hello bitkép, és előkészíti a toosend konstruktor azt toohello ügyfélböngészőnek.</span><span class="sxs-lookup"><span data-stu-id="e2078-197">In hello **ImageService** class, add hello following constructor that loads hello bitmap and prepares toosend it toohello client browser.</span></span>
   
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
7. <span data-ttu-id="e2078-198">Közvetlenül a hello előző kód után adja hozzá a következő hello **GetImage** metódus a hello **ImageService** hello képet tartalmazó osztály tooreturn HTTP-üzenet.</span><span class="sxs-lookup"><span data-stu-id="e2078-198">Directly after hello previous code, add hello following **GetImage** method in hello **ImageService** class tooreturn an HTTP message that contains hello image.</span></span>
   
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
   
    <span data-ttu-id="e2078-199">Ez a megvalósítás **MemoryStream** tooretrieve hello lemezképet, és előkészíti az toohello böngészőben adatfolyamként történő.</span><span class="sxs-lookup"><span data-stu-id="e2078-199">This implementation uses **MemoryStream** tooretrieve hello image and prepare it for streaming toohello browser.</span></span> <span data-ttu-id="e2078-200">Az hello adatfolyam-pozíció nulla kezdődik, hello tartalomátvitelre jpeg-ként deklarálja, és az adatfolyamokat, hello információkat.</span><span class="sxs-lookup"><span data-stu-id="e2078-200">It starts hello stream position at zero, declares hello stream content as a jpeg, and streams hello information.</span></span>
8. <span data-ttu-id="e2078-201">A hello **Build** menüben kattintson a **megoldás fordítása**.</span><span class="sxs-lookup"><span data-stu-id="e2078-201">From hello **Build** menu, click **Build Solution**.</span></span>

### <a name="toodefine-hello-configuration-for-running-hello-web-service-on-service-bus"></a><span data-ttu-id="e2078-202">a Service Buson hello webszolgáltatást futtató toodefine hello konfigurációja</span><span class="sxs-lookup"><span data-stu-id="e2078-202">toodefine hello configuration for running hello web service on Service Bus</span></span>
1. <span data-ttu-id="e2078-203">A **Megoldáskezelőben**, kattintson duplán a **App.config** tooopen azt hello Visual Studio szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="e2078-203">In **Solution Explorer**, double-click **App.config** tooopen it in hello Visual Studio editor.</span></span>
   
    <span data-ttu-id="e2078-204">Hello **App.config** fájl tartalmazza hello szolgáltatás nevét, végpontját (Ez azt jelenti, hogy hello helyet Azure továbbítási közzétesz az ügyfeleknek és a gazdáknak toocommunicate egymás mellett), és kötéstípus (hello protokoll, amely használt toocommunicate).</span><span class="sxs-lookup"><span data-stu-id="e2078-204">hello **App.config** file includes hello service name, endpoint (that is, hello location Azure Relay exposes for clients and hosts toocommunicate with each other), and binding (hello type of protocol that is used toocommunicate).</span></span> <span data-ttu-id="e2078-205">hello fő különbség itt az, hogy hello konfigurált szolgáltatásvégpont hivatkozik tooa [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) kötés.</span><span class="sxs-lookup"><span data-stu-id="e2078-205">hello main difference here is that hello configured service endpoint refers tooa [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) binding.</span></span>
2. <span data-ttu-id="e2078-206">Hello `<system.serviceModel>` XML-elem egy WCF-elem, amely meghatározza egy vagy több szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="e2078-206">hello `<system.serviceModel>` XML element is a WCF element that defines one or more services.</span></span> <span data-ttu-id="e2078-207">Itt is használt toodefine hello szolgáltatás nevének és végpontjának.</span><span class="sxs-lookup"><span data-stu-id="e2078-207">Here, it is used toodefine hello service name and endpoint.</span></span> <span data-ttu-id="e2078-208">Hello hello alján `<system.serviceModel>` elem (azonban továbbra is belül `<system.serviceModel>`), adja hozzá a `<bindings>` elem, amely rendelkezik a tartalom a következő hello.</span><span class="sxs-lookup"><span data-stu-id="e2078-208">At hello bottom of hello `<system.serviceModel>` element (but still within `<system.serviceModel>`), add a `<bindings>` element that has hello following content.</span></span> <span data-ttu-id="e2078-209">Ez határozza meg a hello alkalmazásban használt hello kötéseket.</span><span class="sxs-lookup"><span data-stu-id="e2078-209">This defines hello bindings used in hello application.</span></span> <span data-ttu-id="e2078-210">Meghatározhat több kötést, de ez az oktatóanyag csak egyet határoz meg.</span><span class="sxs-lookup"><span data-stu-id="e2078-210">You can define multiple bindings, but for this tutorial you are defining only one.</span></span>
   
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
   
    <span data-ttu-id="e2078-211">hello előző kód meghatároz egy WCF továbbító [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) kötés **relayClientAuthenticationType** túl beállítása**nincs**.</span><span class="sxs-lookup"><span data-stu-id="e2078-211">hello previous code defines a WCF Relay [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) binding with **relayClientAuthenticationType** set too**None**.</span></span> <span data-ttu-id="e2078-212">Ez a beállítás jelöli, ha egy, a kötést használó végpont nem igényel ügyfél-hitelesítőt.</span><span class="sxs-lookup"><span data-stu-id="e2078-212">This setting indicates that an endpoint using this binding does not require a client credential.</span></span>
3. <span data-ttu-id="e2078-213">Hello után `<bindings>` elemet, adja hozzá a `<services>` elemet.</span><span class="sxs-lookup"><span data-stu-id="e2078-213">After hello `<bindings>` element, add a `<services>` element.</span></span> <span data-ttu-id="e2078-214">Hasonló toohello kötések, több szolgáltatás megadhatja a egyetlen konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="e2078-214">Similar toohello bindings, you can define multiple services in a single configuration file.</span></span> <span data-ttu-id="e2078-215">Ez az oktatóanyag azonban csak egyet ad meg.</span><span class="sxs-lookup"><span data-stu-id="e2078-215">However, for this tutorial, you define only one.</span></span>
   
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
   
    <span data-ttu-id="e2078-216">Ez a lépés konfigurál egy szolgáltatás, amely korábban már definiálva hello alapértelmezett **webHttpRelayBinding**.</span><span class="sxs-lookup"><span data-stu-id="e2078-216">This step configures a service that uses hello previously defined default **webHttpRelayBinding**.</span></span> <span data-ttu-id="e2078-217">Ugyancsak hello alapértelmezett **sbTokenProvider**, amely a következő lépésben hello van megadva.</span><span class="sxs-lookup"><span data-stu-id="e2078-217">It also uses hello default **sbTokenProvider**, which is defined in hello next step.</span></span>
4. <span data-ttu-id="e2078-218">Hello után `<services>` elemet, hozzon létre egy `<behaviors>` hello tartalom a következő, "SAS_KEY" lecserélését hello elem *közös hozzáférésű Jogosultságkód* korábban szerzett hello (SAS-) kulcs [Azure-portálon ][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="e2078-218">After hello `<services>` element, create a `<behaviors>` element with hello following content, replacing "SAS_KEY" with hello *Shared Access Signature* (SAS) key you previously obtained from hello [Azure portal][Azure portal].</span></span>
   
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
5. <span data-ttu-id="e2078-219">Még mindig az App.config fájlban, a hello `<appSettings>` elem, a név felülírandó hello teljes kapcsolati karakterlánc hello portálról korábban beszerzett hello kapcsolati karakterlánccal.</span><span class="sxs-lookup"><span data-stu-id="e2078-219">Still in App.config, in hello `<appSettings>` element, replace hello entire connection string value with hello connection string you previously obtained from hello portal.</span></span> 
   
    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SAS_KEY"/>
    </appSettings>
    ```
6. <span data-ttu-id="e2078-220">A hello **Build** menüben kattintson a **megoldás fordítása** toobuild hello teljes megoldás.</span><span class="sxs-lookup"><span data-stu-id="e2078-220">From hello **Build** menu, click **Build Solution** toobuild hello entire solution.</span></span>

### <a name="example"></a><span data-ttu-id="e2078-221">Példa</span><span class="sxs-lookup"><span data-stu-id="e2078-221">Example</span></span>
<span data-ttu-id="e2078-222">hello következő kód bemutatja hello szerződés és a szolgáltatás megvalósítását egy REST-alapú szolgáltatás a futó Service Bus használatával hello **WebHttpRelayBinding** kötés.</span><span class="sxs-lookup"><span data-stu-id="e2078-222">hello following code shows hello contract and service implementation for a REST-based service that is running on  Service Bus using hello **WebHttpRelayBinding** binding.</span></span>

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

<span data-ttu-id="e2078-223">hello következő példa bemutatja hello szolgáltatás társított hello App.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="e2078-223">hello following example shows hello App.config file associated with hello service.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove hello ones they don't need. -->
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

## <a name="step-4-host-hello-rest-based-wcf-service-toouse-azure-relay"></a><span data-ttu-id="e2078-224">4. lépés: Állomás hello REST-alapú WCF szolgáltatás toouse Azure továbbító</span><span class="sxs-lookup"><span data-stu-id="e2078-224">Step 4: Host hello REST-based WCF service toouse Azure Relay</span></span>
<span data-ttu-id="e2078-225">Ez a lépés ismerteti, hogyan toorun egy webes szolgáltatás, a WCF továbbító egy konzolalkalmazás használatával.</span><span class="sxs-lookup"><span data-stu-id="e2078-225">This step describes how toorun a web service using a console application with WCF Relay.</span></span> <span data-ttu-id="e2078-226">Ebben a lépésben írt hello kód teljes listája megtalálható hello eljárást követő hello példában.</span><span class="sxs-lookup"><span data-stu-id="e2078-226">A complete listing of hello code written in this step is provided in hello example following hello procedure.</span></span>

### <a name="toocreate-a-base-address-for-hello-service"></a><span data-ttu-id="e2078-227">toocreate hello szolgáltatás alapcíme</span><span class="sxs-lookup"><span data-stu-id="e2078-227">toocreate a base address for hello service</span></span>
1. <span data-ttu-id="e2078-228">A hello `Main()` függvény deklarációjában, a projekt változó toostore hello névtér létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e2078-228">In hello `Main()` function declaration, create a variable toostore hello namespace of your project.</span></span> <span data-ttu-id="e2078-229">Győződjön meg arról, hogy tooreplace `yourNamespace` hello nevű hello továbbítási névtér korábban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="e2078-229">Make sure tooreplace `yourNamespace` with hello name of hello Relay namespace you created previously.</span></span>
   
    ```csharp
    string serviceNamespace = "yourNamespace";
    ```
    <span data-ttu-id="e2078-230">A Service Bus a névtér toocreate egy egyedi URI hello nevét használja.</span><span class="sxs-lookup"><span data-stu-id="e2078-230">Service Bus uses hello name of your namespace toocreate a unique URI.</span></span>
2. <span data-ttu-id="e2078-231">Hozzon létre egy `Uri` hello névtér alapuló hello szolgáltatás példányának a hello alapcím.</span><span class="sxs-lookup"><span data-stu-id="e2078-231">Create a `Uri` instance for hello base address of hello service that is based on hello namespace.</span></span>
   
    ```csharp
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="toocreate-and-configure-hello-web-service-host"></a><span data-ttu-id="e2078-232">toocreate és hello webszolgáltatásgazda konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e2078-232">toocreate and configure hello web service host</span></span>
* <span data-ttu-id="e2078-233">Hozzon létre hello webszolgáltatás gazdáját hello ebben a szakaszban korábban létrehozott URI-cím használatával.</span><span class="sxs-lookup"><span data-stu-id="e2078-233">Create hello web service host, using hello URI address created earlier in this section.</span></span>
  
    ```csharp
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    <span data-ttu-id="e2078-234">hello szolgáltatás állomás hello WCF-objektum, amely hello gazdaalkalmazást példányosítja.</span><span class="sxs-lookup"><span data-stu-id="e2078-234">hello service host is hello WCF object that instantiates hello host application.</span></span> <span data-ttu-id="e2078-235">Ez a példa továbbítja hello típus fogadó toocreate kívánt (egy **ImageService**), és is hello címet, amelyen tooexpose hello gazdaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e2078-235">This example passes it hello type of host you want toocreate (an **ImageService**), and also hello address at which you want tooexpose hello host application.</span></span>

### <a name="toorun-hello-web-service-host"></a><span data-ttu-id="e2078-236">toorun hello webszolgáltatásgazda</span><span class="sxs-lookup"><span data-stu-id="e2078-236">toorun hello web service host</span></span>
1. <span data-ttu-id="e2078-237">Nyissa meg a hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e2078-237">Open hello service.</span></span>
   
    ```csharp
    host.Open();
    ```
    <span data-ttu-id="e2078-238">hello szolgáltatás már fut.</span><span class="sxs-lookup"><span data-stu-id="e2078-238">hello service is now running.</span></span>
2. <span data-ttu-id="e2078-239">Megjelenít egy üzenetet, amely azt jelzi, hogy hello szolgáltatás fut, és hogyan toostop hello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="e2078-239">Display a message indicating that hello service is running, and how toostop hello service.</span></span>
   
    ```csharp
    Console.WriteLine("Copy hello following address into a browser toosee hello image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] tooexit");
    Console.ReadLine();
    ```
3. <span data-ttu-id="e2078-240">Ha elkészült, zárja be a hello szolgáltatásgazda.</span><span class="sxs-lookup"><span data-stu-id="e2078-240">When finished, close hello service host.</span></span>
   
    ```csharp
    host.Close();
    ```

## <a name="example"></a><span data-ttu-id="e2078-241">Példa</span><span class="sxs-lookup"><span data-stu-id="e2078-241">Example</span></span>
<span data-ttu-id="e2078-242">a következő példa hello hello oktatóanyag és állomások hello szolgáltatást egy konzolalkalmazásban hello szolgáltatási szerződést és a korábbi lépések megvalósítását tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="e2078-242">hello following example includes hello service contract and implementation from previous steps in hello tutorial and hosts hello service in a console application.</span></span> <span data-ttu-id="e2078-243">Fordítsa le a kódot egy ImageListener.exe nevű végrehajtható fájlba a következő hello.</span><span class="sxs-lookup"><span data-stu-id="e2078-243">Compile hello following code into an executable named ImageListener.exe.</span></span>

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

            Console.WriteLine("Copy hello following address into a browser toosee hello image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] tooexit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### <a name="compiling-hello-code"></a><span data-ttu-id="e2078-244">Hello kód fordítása</span><span class="sxs-lookup"><span data-stu-id="e2078-244">Compiling hello code</span></span>
<span data-ttu-id="e2078-245">Hello megoldás létrehozása, után hello toorun hello alkalmazás követően:</span><span class="sxs-lookup"><span data-stu-id="e2078-245">After building hello solution, do hello following toorun hello application:</span></span>

1. <span data-ttu-id="e2078-246">Nyomja le az **F5**, vagy tallózással keresse meg a toohello végrehajtható fájl helye (ImageListener\bin\Debug\ImageListener.exe) toorun hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e2078-246">Press **F5**, or browse toohello executable file location (ImageListener\bin\Debug\ImageListener.exe), toorun hello service.</span></span> <span data-ttu-id="e2078-247">Tartsa hello alkalmazás fut, tooperform hello következő lépésre van szükség.</span><span class="sxs-lookup"><span data-stu-id="e2078-247">Keep hello app running, as it's required tooperform hello next step.</span></span>
2. <span data-ttu-id="e2078-248">Másolással illessze be a hello cím hello parancssorból egy böngésző toosee hello lemezképpel.</span><span class="sxs-lookup"><span data-stu-id="e2078-248">Copy and paste hello address from hello command prompt into a browser toosee hello image.</span></span>
3. <span data-ttu-id="e2078-249">Amikor elkészült, nyomja meg az **Enter** hello parancssori ablakban tooclose hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e2078-249">When you are finished, press **Enter** in hello command prompt window tooclose hello app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e2078-250">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e2078-250">Next steps</span></span>
<span data-ttu-id="e2078-251">Most, hogy létrehozott egy hello Service Bus relay szolgáltatást használó alkalmazást, tekintse meg a következő Azure Relayjel kapcsolatos további cikkek toolearn hello:</span><span class="sxs-lookup"><span data-stu-id="e2078-251">Now that you've built an application that uses hello Service Bus relay service, see hello following articles toolearn more about Azure Relay:</span></span>

* [<span data-ttu-id="e2078-252">Az Azure Service Bus-architektúra áttekintése</span><span class="sxs-lookup"><span data-stu-id="e2078-252">Azure Service Bus architectural overview</span></span>](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
* [<span data-ttu-id="e2078-253">Az Azure Relay áttekintése</span><span class="sxs-lookup"><span data-stu-id="e2078-253">Azure Relay overview</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="e2078-254">Hogyan toouse hello WCF továbbítják a .NET-bA</span><span class="sxs-lookup"><span data-stu-id="e2078-254">How toouse hello WCF relay service with .NET</span></span>](relay-wcf-dotnet-get-started.md)

[Azure portal]: https://portal.azure.com
