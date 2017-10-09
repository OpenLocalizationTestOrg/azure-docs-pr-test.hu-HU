---
title: "aaaAzure WCF továbbító hibrid helyszíni/felhőbeli alkalmazás (.NET) |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate egy .NET helyszíni/felhőbeli hibridalkalmazást Azure WCF Relay használatával."
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 9ed02f7c-ebfb-4f39-9c97-b7dc15bcb4c1
ms.service: service-bus-relay
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/14/2017
ms.author: sethm
ms.openlocfilehash: aab8b1dbdc85c4edf7b0ccef0921b69524b2d306
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="net-on-premisescloud-hybrid-application-using-azure-wcf-relay"></a><span data-ttu-id="97337-103">Helyszíni/felhőbeli .NET-hibridalkalmazás az Azure WCF Relay használatával</span><span class="sxs-lookup"><span data-stu-id="97337-103">.NET on-premises/cloud hybrid application using Azure WCF Relay</span></span>
## <a name="introduction"></a><span data-ttu-id="97337-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="97337-104">Introduction</span></span>

<span data-ttu-id="97337-105">Ez a cikk bemutatja, hogyan toobuild hibrid felhő Microsoft Azure és a Visual Studio alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="97337-105">This article shows how toobuild a hybrid cloud application with Microsoft Azure and Visual Studio.</span></span> <span data-ttu-id="97337-106">hello oktatóanyag feltételezi, hogy rendelkezik-e nincs előzetes tapasztalata az Azure használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="97337-106">hello tutorial assumes you have no prior experience using Azure.</span></span> <span data-ttu-id="97337-107">30 percen belül hogy be több Azure-erőforrásokat használó alkalmazások és hello felhőben futó.</span><span class="sxs-lookup"><span data-stu-id="97337-107">In less than 30 minutes, you will have an application that uses multiple Azure resources up and running in hello cloud.</span></span>

<span data-ttu-id="97337-108">Az oktatóanyagban érintett témák köre:</span><span class="sxs-lookup"><span data-stu-id="97337-108">You will learn:</span></span>

* <span data-ttu-id="97337-109">Hogyan toocreate vagy alakítása a megjeleníthető meglévő webszolgáltatás egy webes megoldással.</span><span class="sxs-lookup"><span data-stu-id="97337-109">How toocreate or adapt an existing web service for consumption by a web solution.</span></span>
* <span data-ttu-id="97337-110">Egy Azure alkalmazás és egy webszolgáltatás-bővítmény toouse tooshare hello Azure WCF továbbító szolgáltatásadatok hogyan máshol tárolt.</span><span class="sxs-lookup"><span data-stu-id="97337-110">How toouse hello Azure WCF Relay service tooshare data between an Azure application and a web service hosted elsewhere.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="how-azure-relay-helps-with-hybrid-solutions"></a><span data-ttu-id="97337-111">Hogyan segít az Azure Relay a hibrid megoldások terén?</span><span class="sxs-lookup"><span data-stu-id="97337-111">How Azure Relay helps with hybrid solutions</span></span>

<span data-ttu-id="97337-112">Üzleti megoldások általában egyéni kódok tootackle új és egyedi üzleti követelmények és a megoldások és rendszerek már által szolgáltatott létező funkciók álló.</span><span class="sxs-lookup"><span data-stu-id="97337-112">Business solutions are typically composed of a combination of custom code written tootackle new and unique business requirements and existing functionality provided by solutions and systems that are already in place.</span></span>

<span data-ttu-id="97337-113">Megoldás fejlesztők indító toouse hello felhő könnyebb kezelésére vonatkozó méretkövetelményekhez és alacsonyabb üzemi költségek.</span><span class="sxs-lookup"><span data-stu-id="97337-113">Solution architects are starting toouse hello cloud for easier handling of scale requirements and lower operational costs.</span></span> <span data-ttu-id="97337-114">Ennek során találnak, hogy szeretnének tooleverage, mivel azok megoldások építőelemeit hello vállalati tűzfalon belül és kívül könnyen meglévő szolgáltatási eszközök elérni hello felhőalapú megoldás.</span><span class="sxs-lookup"><span data-stu-id="97337-114">In doing so, they find that existing service assets they'd like tooleverage as building blocks for their solutions are inside hello corporate firewall and out of easy reach for access by hello cloud solution.</span></span> <span data-ttu-id="97337-115">Számos belső szolgáltatás nem épített, illetve, hogy azok könnyen elérhető legyen vállalati hálózat peremhálózaton hello tárolva.</span><span class="sxs-lookup"><span data-stu-id="97337-115">Many internal services are not built or hosted in a way that they can be easily exposed at hello corporate network edge.</span></span>

<span data-ttu-id="97337-116">[Az Azure továbbítási](https://azure.microsoft.com/services/service-bus/) készült használati eset a meglévő Windows Communication Foundation (WCF) a webszolgáltatások hello és azokat, így biztonságosan elérhetik toosolutions anélkül, hogy hello vállalati peremhálózati kívül található szolgáltatások zavaró módosításokat toohello vállalati hálózati infrastruktúrában.</span><span class="sxs-lookup"><span data-stu-id="97337-116">[Azure Relay](https://azure.microsoft.com/services/service-bus/) is designed for hello use-case of taking existing Windows Communication Foundation (WCF) web services and making those services securely accessible toosolutions that reside outside hello corporate perimeter without requiring intrusive changes toohello corporate network infrastructure.</span></span> <span data-ttu-id="97337-117">Ilyen relay-szolgáltatások továbbra is megtalálhatóak a meglévő környezeten belül, de azok delegálása figyeli a bejövő munkamenetek és kérések toohello felhőben üzemeltetett továbbítási szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="97337-117">Such relay services are still hosted inside their existing environment, but they delegate listening for incoming sessions and requests toohello cloud-hosted relay service.</span></span> <span data-ttu-id="97337-118">Az Azure Relay ezeket a szolgáltatásokat [közös hozzáférésű jogosultságkód- (SAS-)](../service-bus-messaging/service-bus-sas.md) hitelesítéssel a jogosulatlan hozzáféréssel szemben is védi.</span><span class="sxs-lookup"><span data-stu-id="97337-118">Azure Relay also protects those services from unauthorized access by using [Shared Access Signature (SAS)](../service-bus-messaging/service-bus-sas.md) authentication.</span></span>

## <a name="solution-scenario"></a><span data-ttu-id="97337-119">A megoldás forgatókönyve</span><span class="sxs-lookup"><span data-stu-id="97337-119">Solution scenario</span></span>
<span data-ttu-id="97337-120">Ebben az oktatóanyagban létrehoz egy ASP.NET-webhely, amely lehetővé teszi a toosee hello Termékleltár oldalán a termékek listáját.</span><span class="sxs-lookup"><span data-stu-id="97337-120">In this tutorial, you will create an ASP.NET website that enables you toosee a list of products on hello product inventory page.</span></span>

![][0]

<span data-ttu-id="97337-121">hello oktatóanyag feltételezi, hogy termékinformációk rendelkezik egy meglévő helyi rendszeren, és használja az Azure továbbítási tooreach el ezt a rendszert.</span><span class="sxs-lookup"><span data-stu-id="97337-121">hello tutorial assumes that you have product information in an existing on-premises system, and uses Azure Relay tooreach into that system.</span></span> <span data-ttu-id="97337-122">Ezt egy olyan webszolgáltatás szimulálja, amely egyszerű konzolalkalmazásként fut, és a termékek memóriában szereplő készletére épül.</span><span class="sxs-lookup"><span data-stu-id="97337-122">This is simulated by a web service that runs in a simple console application and is backed by an in-memory set of products.</span></span> <span data-ttu-id="97337-123">Meg kell tudni toorun ezt a konzolalkalmazást a saját számítógépén, és hello webes szerepkör üzembe helyezés Azure.</span><span class="sxs-lookup"><span data-stu-id="97337-123">You will be able toorun this console application on your own computer and deploy hello web role into Azure.</span></span> <span data-ttu-id="97337-124">Ezzel a módszerrel látni fogja hogyan hello Azure adatközpontjában futó hello webes szerepkör intéz hívást a számítógépet annak ellenére, hogy a számítógép szinte biztosan legalább egy tűzfal és a hálózati cím címfordítási (NAT-) réteg mögött lesznek tárolva.</span><span class="sxs-lookup"><span data-stu-id="97337-124">By doing so, you will see how hello web role running in hello Azure datacenter will indeed call into your computer, even though your computer will almost certainly reside behind at least one firewall and a network address translation (NAT) layer.</span></span>

## <a name="set-up-hello-development-environment"></a><span data-ttu-id="97337-125">Hello fejlesztési környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="97337-125">Set up hello development environment</span></span>

<span data-ttu-id="97337-126">Mielőtt elkezdené az Azure-alkalmazások fejlesztésével, hello eszközök, és állítsa be a fejlesztési környezetet:</span><span class="sxs-lookup"><span data-stu-id="97337-126">Before you can begin developing Azure applications, download hello tools and set up your development environment:</span></span>

1. <span data-ttu-id="97337-127">Hello Azure SDK telepítse a .NET hello SDK [letöltési oldalon](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="97337-127">Install hello Azure SDK for .NET from hello SDK [downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="97337-128">A hello **.NET** oszlopban kattintson hello verziója [Visual Studio](http://www.visualstudio.com) használ.</span><span class="sxs-lookup"><span data-stu-id="97337-128">In hello **.NET** column, click hello version of [Visual Studio](http://www.visualstudio.com) you are using.</span></span> <span data-ttu-id="97337-129">hello lépéseit az oktatóanyag Visual Studio 2015-öt használja, de ezek a Visual Studio 2017 is működnek.</span><span class="sxs-lookup"><span data-stu-id="97337-129">hello steps in this tutorial use Visual Studio 2015, but they also work with Visual Studio 2017.</span></span>
3. <span data-ttu-id="97337-130">Ha toorun kéri, vagy hello telepítő mentéséhez, kattintson **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="97337-130">When prompted toorun or save hello installer, click **Run**.</span></span>
4. <span data-ttu-id="97337-131">A hello **Webplatform-telepítő**, kattintson a **telepítése** és hello a telepítés folytatásához.</span><span class="sxs-lookup"><span data-stu-id="97337-131">In hello **Web Platform Installer**, click **Install** and proceed with hello installation.</span></span>
5. <span data-ttu-id="97337-132">Hello telepítés befejezése után, hogy minden szükséges toostart toodevelop hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="97337-132">Once hello installation is complete, you will have everything necessary toostart toodevelop hello app.</span></span> <span data-ttu-id="97337-133">hello SDK olyan eszközöket tartalmaz, amelyekkel könnyedén fejleszthet Azure-alkalmazásokat a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="97337-133">hello SDK includes tools that let you easily develop Azure applications in Visual Studio.</span></span>

## <a name="create-a-namespace"></a><span data-ttu-id="97337-134">Névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="97337-134">Create a namespace</span></span>

<span data-ttu-id="97337-135">az Azure-továbbítási funkciók toobegin használatával hello, először létre kell hoznia egy szolgáltatásnévteret.</span><span class="sxs-lookup"><span data-stu-id="97337-135">toobegin using hello relay features in Azure, you must first create a service namespace.</span></span> <span data-ttu-id="97337-136">A névtér egy hatókörkezelési tárolót biztosít az Azure erőforrásainak címzéséhez az alkalmazáson belül.</span><span class="sxs-lookup"><span data-stu-id="97337-136">A namespace provides a scoping container for addressing Azure resources within your application.</span></span> <span data-ttu-id="97337-137">Hajtsa végre a hello [utasításokat itt](relay-create-namespace-portal.md) toocreate továbbítási névtér.</span><span class="sxs-lookup"><span data-stu-id="97337-137">Follow hello [instructions here](relay-create-namespace-portal.md) toocreate a Relay namespace.</span></span>

## <a name="create-an-on-premises-server"></a><span data-ttu-id="97337-138">Helyszíni kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="97337-138">Create an on-premises server</span></span>

<span data-ttu-id="97337-139">Először létrehoz egy (utánzatként funkcionáló) helyszíni termékkatalógus-rendszert.</span><span class="sxs-lookup"><span data-stu-id="97337-139">First, you will build a (mock) on-premises product catalog system.</span></span> <span data-ttu-id="97337-140">Lesz viszonylag egyszerű; Ez egy tényleges helyszíni termékkatalógus-rendszert, hogy toointegrate próbált teljes szolgáltatási felülettel képviselik tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="97337-140">It will be fairly simple; you can see this as representing an actual on-premises product catalog system with a complete service surface that we're trying toointegrate.</span></span>

<span data-ttu-id="97337-141">Ez a projekt egy Visual Studio-Konzolalkalmazás, és használja a hello [Azure Service Bus NuGet-csomag](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) tooinclude hello Service Bus-kódtárak és konfigurációs beállítások.</span><span class="sxs-lookup"><span data-stu-id="97337-141">This project is a Visual Studio console application, and uses hello [Azure Service Bus NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) tooinclude hello Service Bus libraries and configuration settings.</span></span>

### <a name="create-hello-project"></a><span data-ttu-id="97337-142">Hello projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="97337-142">Create hello project</span></span>

1. <span data-ttu-id="97337-143">Rendszergazdai jogosultságokkal indítsa el a Microsoft Visual Studiót.</span><span class="sxs-lookup"><span data-stu-id="97337-143">Using administrator privileges, start Microsoft Visual Studio.</span></span> <span data-ttu-id="97337-144">toodo Igen, kattintson a jobb gombbal a hello Visual Studio program ikonjára, és kattintson **Futtatás rendszergazdaként**.</span><span class="sxs-lookup"><span data-stu-id="97337-144">toodo so, right-click hello Visual Studio program icon, and then click **Run as administrator**.</span></span>
2. <span data-ttu-id="97337-145">A Visual Studio, a hello **fájl** menüben kattintson a **új**, és kattintson a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="97337-145">In Visual Studio, on hello **File** menu, click **New**, and then click **Project**.</span></span>
3. <span data-ttu-id="97337-146">Az **Installed Templates** (Telepített sablonok) lap **Visual C#** területén kattintson a **Console App (.NET Framework)** (Konzolalkalmazás (.NET keretrendszer)) elemre.</span><span class="sxs-lookup"><span data-stu-id="97337-146">From **Installed Templates**, under **Visual C#**, click **Console App (.NET Framework)**.</span></span> <span data-ttu-id="97337-147">A hello **neve** mezőbe, írja be a hello nevet **ProductsServer**:</span><span class="sxs-lookup"><span data-stu-id="97337-147">In hello **Name** box, type hello name **ProductsServer**:</span></span>

   ![][11]
4. <span data-ttu-id="97337-148">Kattintson a **OK** toocreate hello **ProductsServer** projekt.</span><span class="sxs-lookup"><span data-stu-id="97337-148">Click **OK** toocreate hello **ProductsServer** project.</span></span>
5. <span data-ttu-id="97337-149">Ha már telepítette a hello NuGet-Csomagkezelőt a Visual Studio, akkor hagyja toohello tovább.</span><span class="sxs-lookup"><span data-stu-id="97337-149">If you have already installed hello NuGet package manager for Visual Studio, skip toohello next step.</span></span> <span data-ttu-id="97337-150">Ellenkező esetben látogasson el a [NuGet][NuGet] oldalára, és kattintson az [Install NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) (NuGet telepítése) parancsra.</span><span class="sxs-lookup"><span data-stu-id="97337-150">Otherwise, visit [NuGet][NuGet] and click [Install NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c).</span></span> <span data-ttu-id="97337-151">Hajtsa végre a hello kér tooinstall hello NuGet-Csomagkezelő, majd indítsa újra a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="97337-151">Follow hello prompts tooinstall hello NuGet package manager, then re-start Visual Studio.</span></span>
6. <span data-ttu-id="97337-152">A Megoldáskezelőben kattintson a jobb gombbal hello **ProductsServer** projektre, majd kattintson a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="97337-152">In Solution Explorer, right-click hello **ProductsServer** project, then click **Manage NuGet Packages**.</span></span>
7. <span data-ttu-id="97337-153">Kattintson a hello **Tallózás** lapra, és keresse meg `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="97337-153">Click hello **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="97337-154">Jelölje be hello **WindowsAzure.ServiceBus** csomag.</span><span class="sxs-lookup"><span data-stu-id="97337-154">Select hello **WindowsAzure.ServiceBus** package.</span></span>
8. <span data-ttu-id="97337-155">Kattintson a **telepítése**, és el kell fogadnia a használati feltételek hello.</span><span class="sxs-lookup"><span data-stu-id="97337-155">Click **Install**, and accept hello terms of use.</span></span>

   ![][13]

   <span data-ttu-id="97337-156">Vegye figyelembe, hogy hello szükséges ügyfélszerelvények most már hivatkozottak.</span><span class="sxs-lookup"><span data-stu-id="97337-156">Note that hello required client assemblies are now referenced.</span></span>
8. <span data-ttu-id="97337-157">Adjon egy új osztályt a termékszerződéshez.</span><span class="sxs-lookup"><span data-stu-id="97337-157">Add a new class for your product contract.</span></span> <span data-ttu-id="97337-158">A Megoldáskezelőben kattintson a jobb gombbal hello **ProductsServer** projektre, kattintson **Hozzáadás**, és kattintson a **osztály**.</span><span class="sxs-lookup"><span data-stu-id="97337-158">In Solution Explorer, right-click hello **ProductsServer** project and click **Add**, and then click **Class**.</span></span>
9. <span data-ttu-id="97337-159">A hello **neve** mezőbe, írja be a hello nevet **ProductsContract.cs**.</span><span class="sxs-lookup"><span data-stu-id="97337-159">In hello **Name** box, type hello name **ProductsContract.cs**.</span></span> <span data-ttu-id="97337-160">Ezután kattintson az **Add** (Hozzáadás) gombra.</span><span class="sxs-lookup"><span data-stu-id="97337-160">Then click **Add**.</span></span>
10. <span data-ttu-id="97337-161">A **ProductsContract.cs**, hello névtér definícióját cserélje le a következő kódra, amely meghatározza a hello szerződés hello szolgáltatás hello.</span><span class="sxs-lookup"><span data-stu-id="97337-161">In **ProductsContract.cs**, replace hello namespace definition with hello following code, which defines hello contract for hello service.</span></span>

    ```csharp
    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;

        // Define hello data contract for hello service
        [DataContract]
        // Declare hello serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }

        // Define hello service contract.
        [ServiceContract]
        interface IProducts
        {
            [OperationContract]
            IList<ProductData> GetProducts();

        }

        interface IProductsChannel : IProducts, IClientChannel
        {
        }
    }
    ```
11. <span data-ttu-id="97337-162">A productscontract.cs fájlban cserélje le a következő kódra, amely hozzáadja a hello profilszolgáltatást és annak állomását hello hello hello névtér-definíciót.</span><span class="sxs-lookup"><span data-stu-id="97337-162">In Program.cs, replace hello namespace definition with hello following code, which adds hello profile service and hello host for it.</span></span>

    ```csharp
    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;

        // Implement hello IProducts interface.
        class ProductsService : IProducts
        {

            // Populate array of products for display on website
            ProductData[] products =
                new []
                    {
                        new ProductData{ Id = "1", Name = "Rock",
                                         Quantity = "1"},
                        new ProductData{ Id = "2", Name = "Paper",
                                         Quantity = "3"},
                        new ProductData{ Id = "3", Name = "Scissors",
                                         Quantity = "5"},
                        new ProductData{ Id = "4", Name = "Well",
                                         Quantity = "2500"},
                    };

            // Display a message in hello service console application
            // when hello list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }

        }

        class Program
        {
            // Define hello Main() function in hello service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();

                Console.WriteLine("Press ENTER tooclose");
                Console.ReadLine();

                sh.Close();
            }
        }
    }
    ```
12. <span data-ttu-id="97337-163">A Megoldáskezelőben kattintson duplán a hello **App.config** fájl tooopen azt hello Visual Studio szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="97337-163">In Solution Explorer, double-click hello **App.config** file tooopen it in hello Visual Studio editor.</span></span> <span data-ttu-id="97337-164">Hello hello alján `<system.ServiceModel>` elem (de továbbra is belül `<system.ServiceModel>`), adja hozzá a következő XML-kódot hello.</span><span class="sxs-lookup"><span data-stu-id="97337-164">At hello bottom of hello `<system.ServiceModel>` element (but still within `<system.ServiceModel>`), add hello following XML code.</span></span> <span data-ttu-id="97337-165">Lehet, hogy tooreplace *yourServiceNamespace* hello nevű a névtér és *yourKey* a hello hello portálról korábban lekért SAS-kulcs:</span><span class="sxs-lookup"><span data-stu-id="97337-165">Be sure tooreplace *yourServiceNamespace* with hello name of your namespace, and *yourKey* with hello SAS key you retrieved earlier from hello portal:</span></span>

    ```xml
    <system.serviceModel>
    ...
      <services>
         <service name="ProductsServer.ProductsService">
           <endpoint address="sb://yourServiceNamespace.servicebus.windows.net/products" binding="netTcpRelayBinding" contract="ProductsServer.IProducts" behaviorConfiguration="products"/>
         </service>
      </services>
      <behaviors>
         <endpointBehaviors>
           <behavior name="products">
             <transportClientEndpointBehavior>
                <tokenProvider>
                   <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
             </transportClientEndpointBehavior>
           </behavior>
         </endpointBehaviors>
      </behaviors>
    </system.serviceModel>
    ```
13. <span data-ttu-id="97337-166">Még mindig az App.config fájlban, a hello `<appSettings>` elem, a név felülírandó hello kapcsolódási karakterlánc értéke hello portálról korábban beszerzett hello kapcsolati karakterlánccal.</span><span class="sxs-lookup"><span data-stu-id="97337-166">Still in App.config, in hello `<appSettings>` element, replace hello connection string value with hello connection string you previously obtained from hello portal.</span></span>

    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```
14. <span data-ttu-id="97337-167">Nyomja le az **Ctrl + Shift + B** vagy hello **Build** menüben kattintson a **megoldás fordítása** toobuild hello alkalmazást, és ellenőrizze eddigi munkája pontosságának hello.</span><span class="sxs-lookup"><span data-stu-id="97337-167">Press **Ctrl+Shift+B** or from hello **Build** menu, click **Build Solution** toobuild hello application and verify hello accuracy of your work so far.</span></span>

## <a name="create-an-aspnet-application"></a><span data-ttu-id="97337-168">ASP.NET-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="97337-168">Create an ASP.NET application</span></span>

<span data-ttu-id="97337-169">Ebben a szakaszban egy egyszerű ASP.NET-alkalmazást fog létrehozni, amely megjeleníti a termékszolgáltatásból lekért adatokat.</span><span class="sxs-lookup"><span data-stu-id="97337-169">In this section you will build a simple ASP.NET application that displays data retrieved from your product service.</span></span>

### <a name="create-hello-project"></a><span data-ttu-id="97337-170">Hello projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="97337-170">Create hello project</span></span>

1. <span data-ttu-id="97337-171">Ellenőrizze, hogy a Visual Studio rendszergazdai jogosultságokkal fut-e.</span><span class="sxs-lookup"><span data-stu-id="97337-171">Ensure that Visual Studio is running with administrator privileges.</span></span>
2. <span data-ttu-id="97337-172">A Visual Studio, a hello **fájl** menüben kattintson a **új**, és kattintson a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="97337-172">In Visual Studio, on hello **File** menu, click **New**, and then click **Project**.</span></span>
3. <span data-ttu-id="97337-173">Az **Installed Templates** (Telepített sablonok) lap **Visual C#** területén kattintson az **ASP.NET Web Application (.NET Framework)** (ASP.NET-webalkalmazás (.NET keretrendszer)) elemre.</span><span class="sxs-lookup"><span data-stu-id="97337-173">From **Installed Templates**, under **Visual C#**, click **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="97337-174">Név hello projekt **ProductsPortal**.</span><span class="sxs-lookup"><span data-stu-id="97337-174">Name hello project **ProductsPortal**.</span></span> <span data-ttu-id="97337-175">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="97337-175">Then click **OK**.</span></span>

   ![][15]

4. <span data-ttu-id="97337-176">A hello **ASP.NET sablonok** hello listájában **új ASP.NET-webalkalmazás** párbeszédpanel, kattintson a **MVC**.</span><span class="sxs-lookup"><span data-stu-id="97337-176">From hello **ASP.NET Templates** list in hello **New ASP.NET Web Application** dialog, click **MVC**.</span></span>

   ![][16]

6. <span data-ttu-id="97337-177">Kattintson a hello **hitelesítés módosítása** gombra.</span><span class="sxs-lookup"><span data-stu-id="97337-177">Click hello **Change Authentication** button.</span></span> <span data-ttu-id="97337-178">A hello **hitelesítés módosítása** párbeszédpanelen ellenőrizze, hogy **nem hitelesítési** van kiválasztva, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="97337-178">In hello **Change Authentication** dialog box, ensure that **No Authentication** is selected, and then click **OK**.</span></span> <span data-ttu-id="97337-179">Ebben az oktatóanyaghoz egy olyan alkalmazást helyezhet üzembe, amelyhez nincs szükség felhasználói bejelentkezésre.</span><span class="sxs-lookup"><span data-stu-id="97337-179">For this tutorial, you're deploying an app that does not need a user login.</span></span>

    ![][18]

7. <span data-ttu-id="97337-180">Vissza a hello **új ASP.NET-webalkalmazás** párbeszédpanel, kattintson a **OK** toocreate hello MVC-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="97337-180">Back in hello **New ASP.NET Web Application** dialog, click **OK** toocreate hello MVC app.</span></span>
8. <span data-ttu-id="97337-181">Most az Azure-erőforrásokat kell konfigurálnia az új webalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="97337-181">Now you must configure Azure resources for a new web app.</span></span> <span data-ttu-id="97337-182">Hello kövesse hello [tooAzure szakaszban ismertetett közzététele](../app-service-web/app-service-web-get-started-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="97337-182">Follow hello steps in hello [Publish tooAzure section of this article](../app-service-web/app-service-web-get-started-dotnet.md).</span></span> <span data-ttu-id="97337-183">Ezt követően térjen vissza a toothis oktatóanyag, és folytassa a következő lépésre toohello.</span><span class="sxs-lookup"><span data-stu-id="97337-183">Then, return toothis tutorial and proceed toohello next step.</span></span>
10. <span data-ttu-id="97337-184">A Megoldáskezelőben kattintson a jobb gombbal a **Models** (Modellek) elemre, kattintson az **Add** (Hozzáadás) parancsra, majd kattintson a **Class** (Osztály) elemre.</span><span class="sxs-lookup"><span data-stu-id="97337-184">In Solution Explorer, right-click **Models** and then click **Add**, then click **Class**.</span></span> <span data-ttu-id="97337-185">A hello **neve** mezőbe, írja be a hello nevet **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="97337-185">In hello **Name** box, type hello name **Product.cs**.</span></span> <span data-ttu-id="97337-186">Ezután kattintson az **Add** (Hozzáadás) gombra.</span><span class="sxs-lookup"><span data-stu-id="97337-186">Then click **Add**.</span></span>

    ![][17]

### <a name="modify-hello-web-application"></a><span data-ttu-id="97337-187">Hello webalkalmazás módosítása</span><span class="sxs-lookup"><span data-stu-id="97337-187">Modify hello web application</span></span>

1. <span data-ttu-id="97337-188">Visual Studio hello Product.cs fájlban cserélje le a következő kód hello hello meglévő névtér-definíciót.</span><span class="sxs-lookup"><span data-stu-id="97337-188">In hello Product.cs file in Visual Studio, replace hello existing namespace definition with hello following code.</span></span>

   ```csharp
    // Declare properties for hello products inventory.
    namespace ProductsWeb.Models
    {
       public class Product
       {
           public string Id { get; set; }
           public string Name { get; set; }
           public string Quantity { get; set; }
       }
    }
    ```
2. <span data-ttu-id="97337-189">A Megoldáskezelőben bontsa ki a hello **tartományvezérlők** mappát, majd kattintson duplán a hello **HomeController.cs** tooopen fájl, a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="97337-189">In Solution Explorer, expand hello **Controllers** folder, then double-click hello **HomeController.cs** file tooopen it in Visual Studio.</span></span>
3. <span data-ttu-id="97337-190">A **HomeController.cs**, cserélje le a következő kód hello hello meglévő névtér-definíciót.</span><span class="sxs-lookup"><span data-stu-id="97337-190">In **HomeController.cs**, replace hello existing namespace definition with hello following code.</span></span>

    ```csharp
    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;

        public class HomeController : Controller
        {
            // Return a view of hello products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
         }
    }
    ```
4. <span data-ttu-id="97337-191">A Megoldáskezelőben bontsa ki a hello Views\Shared mappát, majd kattintson duplán a **_Layout.cshtml** tooopen azt hello Visual Studio szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="97337-191">In Solution Explorer, expand hello Views\Shared folder, then double-click **_Layout.cshtml** tooopen it in hello Visual Studio editor.</span></span>
5. <span data-ttu-id="97337-192">Összes előfordulását módosítsa **My ASP.NET Application** túl**LITWARE's termékek**.</span><span class="sxs-lookup"><span data-stu-id="97337-192">Change all occurrences of **My ASP.NET Application** too**LITWARE's Products**.</span></span>
6. <span data-ttu-id="97337-193">Távolítsa el a hello **Home**, **kapcsolatos**, és **forduljon** hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="97337-193">Remove hello **Home**, **About**, and **Contact** links.</span></span> <span data-ttu-id="97337-194">A következő példa hello törölje a kiemelt hello kódot.</span><span class="sxs-lookup"><span data-stu-id="97337-194">In hello following example, delete hello highlighted code.</span></span>

    ![][41]

7. <span data-ttu-id="97337-195">A Megoldáskezelőben bontsa ki a hello Views\Home mappát, majd kattintson duplán a **Index.cshtml** tooopen azt hello Visual Studio szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="97337-195">In Solution Explorer, expand hello Views\Home folder, then double-click **Index.cshtml** tooopen it in hello Visual Studio editor.</span></span> <span data-ttu-id="97337-196">Cserélje le a következő kód hello hello hello fájl teljes tartalmát.</span><span class="sxs-lookup"><span data-stu-id="97337-196">Replace hello entire contents of hello file with hello following code.</span></span>

   ```html
   @model IEnumerable<ProductsWeb.Models.Product>

   @{
            ViewBag.Title = "Index";
   }

   <h2>Prod Inventory</h2>

   <table>
             <tr>
                 <th>
                     @Html.DisplayNameFor(model => model.Name)
                 </th>
                 <th></th>
                 <th>
                     @Html.DisplayNameFor(model => model.Quantity)
                 </th>
             </tr>

   @foreach (var item in Model) {
             <tr>
                 <td>
                     @Html.DisplayFor(modelItem => item.Name)
                 </td>
                 <td>
                     @Html.DisplayFor(modelItem => item.Quantity)
                 </td>
             </tr>
   }

   </table>
   ```
8. <span data-ttu-id="97337-197">munkája pontosságát tooverify hello eddig lenyomhatja **Ctrl + Shift + B** toobuild hello projekt.</span><span class="sxs-lookup"><span data-stu-id="97337-197">tooverify hello accuracy of your work so far, you can press **Ctrl+Shift+B** toobuild hello project.</span></span>

### <a name="run-hello-app-locally"></a><span data-ttu-id="97337-198">Hello alkalmazás helyi futtatása</span><span class="sxs-lookup"><span data-stu-id="97337-198">Run hello app locally</span></span>

<span data-ttu-id="97337-199">Futtassa a hello alkalmazás tooverify a működését.</span><span class="sxs-lookup"><span data-stu-id="97337-199">Run hello application tooverify that it works.</span></span>

1. <span data-ttu-id="97337-200">Győződjön meg arról, hogy **ProductsPortal** hello aktív projekt.</span><span class="sxs-lookup"><span data-stu-id="97337-200">Ensure that **ProductsPortal** is hello active project.</span></span> <span data-ttu-id="97337-201">Kattintson a jobb gombbal a hello projekt nevére a Megoldáskezelőben, majd válassza **beállítása szerint Startup Project**.</span><span class="sxs-lookup"><span data-stu-id="97337-201">Right-click hello project name in Solution Explorer and select **Set As Startup Project**.</span></span>
2. <span data-ttu-id="97337-202">Nyomja le az **F5** billentyűt a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="97337-202">In Visual Studio, press **F5**.</span></span>
3. <span data-ttu-id="97337-203">Az alkalmazásának meg kell jelennie egy böngészőben.</span><span class="sxs-lookup"><span data-stu-id="97337-203">Your application should appear, running in a browser.</span></span>

   ![][21]

## <a name="put-hello-pieces-together"></a><span data-ttu-id="97337-204">Hello darab hozzáfoghat</span><span class="sxs-lookup"><span data-stu-id="97337-204">Put hello pieces together</span></span>

<span data-ttu-id="97337-205">hello tovább hello a helyszíni termékek kiszolgáló az ASP.NET-alkalmazás hello toohook.</span><span class="sxs-lookup"><span data-stu-id="97337-205">hello next step is toohook up hello on-premises products server with hello ASP.NET application.</span></span>

1. <span data-ttu-id="97337-206">Ha az még nincs megnyitva, a Visual Studióban nyissa meg újra hello **ProductsPortal** hello létrehozott projekt [ASP.NET-alkalmazás létrehozása](#create-an-aspnet-application) szakasz.</span><span class="sxs-lookup"><span data-stu-id="97337-206">If it is not already open, in Visual Studio re-open hello **ProductsPortal** project you created in hello [Create an ASP.NET application](#create-an-aspnet-application) section.</span></span>
2. <span data-ttu-id="97337-207">Hasonló toohello lépés hello "Egy helyszíni kiszolgáló létrehozása" szakaszban hozzáadása hello NuGet csomag toohello projekt hivatkozásait.</span><span class="sxs-lookup"><span data-stu-id="97337-207">Similar toohello step in hello "Create an On-Premises Server" section, add hello NuGet package toohello project references.</span></span> <span data-ttu-id="97337-208">A Megoldáskezelőben kattintson a jobb gombbal hello **ProductsPortal** projektre, majd kattintson a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="97337-208">In Solution Explorer, right-click hello **ProductsPortal** project, then click **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="97337-209">Keressen a "Service Bus" és a select hello **WindowsAzure.ServiceBus** elemet.</span><span class="sxs-lookup"><span data-stu-id="97337-209">Search for "Service Bus" and select hello **WindowsAzure.ServiceBus** item.</span></span> <span data-ttu-id="97337-210">Ezután hello telepítéséhez, és zárja be a párbeszédpanelt.</span><span class="sxs-lookup"><span data-stu-id="97337-210">Then complete hello installation and close this dialog box.</span></span>
4. <span data-ttu-id="97337-211">A Megoldáskezelőben kattintson a jobb gombbal hello **ProductsPortal** projektre, majd kattintson a **Hozzáadás**, majd **meglévő cikk**.</span><span class="sxs-lookup"><span data-stu-id="97337-211">In Solution Explorer, right-click hello **ProductsPortal** project, then click **Add**, then **Existing Item**.</span></span>
5. <span data-ttu-id="97337-212">Keresse meg a toohello **ProductsContract.cs** hello fájlt **ProductsServer** konzolos projektet.</span><span class="sxs-lookup"><span data-stu-id="97337-212">Navigate toohello **ProductsContract.cs** file from hello **ProductsServer** console project.</span></span> <span data-ttu-id="97337-213">Kattintson a toohighlight ProductsContract.cs.</span><span class="sxs-lookup"><span data-stu-id="97337-213">Click toohighlight ProductsContract.cs.</span></span> <span data-ttu-id="97337-214">Kattintson a lefelé mutató nyíl hello tovább túl**Hozzáadás**, majd kattintson a **Hozzáadás hivatkozásként**.</span><span class="sxs-lookup"><span data-stu-id="97337-214">Click hello down arrow next too**Add**, then click **Add as Link**.</span></span>

   ![][24]

6. <span data-ttu-id="97337-215">Most nyissa meg a hello **HomeController.cs** hello Visual Studio-szerkesztőben fájlt, és cserélje le a következő kód hello hello névtér-definíciót.</span><span class="sxs-lookup"><span data-stu-id="97337-215">Now open hello **HomeController.cs** file in hello Visual Studio editor and replace hello namespace definition with hello following code.</span></span> <span data-ttu-id="97337-216">Lehet, hogy tooreplace *yourServiceNamespace* hello nevű a szolgáltatásnévtér és *yourKey* a SAS-kulccsal.</span><span class="sxs-lookup"><span data-stu-id="97337-216">Be sure tooreplace *yourServiceNamespace* with hello name of your service namespace, and *yourKey* with your SAS key.</span></span> <span data-ttu-id="97337-217">Ezzel a lépéssel engedélyezi a hello toocall hello helyszíni ügyfélszolgáltatást, hello hívás hello eredményt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="97337-217">This will enable hello client toocall hello on-premises service, returning hello result of hello call.</span></span>

   ```csharp
   namespace ProductsWeb.Controllers
   {
       using System.Linq;
       using System.ServiceModel;
       using System.Web.Mvc;
       using Microsoft.ServiceBus;
       using Models;
       using ProductsServer;

       public class HomeController : Controller
       {
           // Declare hello channel factory.
           static ChannelFactory<IProductsChannel> channelFactory;

           static HomeController()
           {
               // Create shared access signature token credentials for authentication.
               channelFactory = new ChannelFactory<IProductsChannel>(new NetTcpRelayBinding(),
                   "sb://yourServiceNamespace.servicebus.windows.net/products");
               channelFactory.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior {
                   TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                       "RootManageSharedAccessKey", "yourKey") });
           }

           public ActionResult Index()
           {
               using (IProductsChannel channel = channelFactory.CreateChannel())
               {
                   // Return a view of hello products inventory.
                   return this.View(from prod in channel.GetProducts()
                                    select
                                        new Product { Id = prod.Id, Name = prod.Name,
                                            Quantity = prod.Quantity });
               }
           }
       }
   }
   ```
7. <span data-ttu-id="97337-218">A Megoldáskezelőben kattintson a jobb gombbal hello **ProductsPortal** (Győződjön meg arról, hogy tooright kattintással hello megoldás, nem hello projekt) megoldás.</span><span class="sxs-lookup"><span data-stu-id="97337-218">In Solution Explorer, right-click hello **ProductsPortal** solution (make sure tooright-click hello solution, not hello project).</span></span> <span data-ttu-id="97337-219">Kattintson az **Add** (Hozzáadás), majd az **Existing Project** (Meglévő projekt) elemre.</span><span class="sxs-lookup"><span data-stu-id="97337-219">Click **Add**, then click **Existing Project**.</span></span>
8. <span data-ttu-id="97337-220">Keresse meg a toohello **ProductsServer** projektre, majd kattintson duplán a hello **ProductsServer.csproj** megoldás fájl tooadd azt.</span><span class="sxs-lookup"><span data-stu-id="97337-220">Navigate toohello **ProductsServer** project, then double-click hello **ProductsServer.csproj** solution file tooadd it.</span></span>
9. <span data-ttu-id="97337-221">**ProductsServer** kell futnia ahhoz toodisplay hello adatok **ProductsPortal**.</span><span class="sxs-lookup"><span data-stu-id="97337-221">**ProductsServer** must be running in order toodisplay hello data on **ProductsPortal**.</span></span> <span data-ttu-id="97337-222">A Megoldáskezelőben kattintson a jobb gombbal hello **ProductsPortal** megoldás, és kattintson **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="97337-222">In Solution Explorer, right-click hello **ProductsPortal** solution and click **Properties**.</span></span> <span data-ttu-id="97337-223">Hello **tulajdonságlapjain** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="97337-223">hello **Property Pages** dialog box is displayed.</span></span>
10. <span data-ttu-id="97337-224">Kattintson a bal oldali hello, **Startup Project**.</span><span class="sxs-lookup"><span data-stu-id="97337-224">On hello left side, click **Startup Project**.</span></span> <span data-ttu-id="97337-225">Kattintson a jobb oldalon hello, **több kezdőprojekt**.</span><span class="sxs-lookup"><span data-stu-id="97337-225">On hello right side, click **Multiple startup projects**.</span></span> <span data-ttu-id="97337-226">Győződjön meg arról, hogy **ProductsServer** és **ProductsPortal** jelöli, ebben a sorrendben, **Start** mindkét hello művelet állítja be.</span><span class="sxs-lookup"><span data-stu-id="97337-226">Ensure that **ProductsServer** and **ProductsPortal** appear, in that order, with **Start** set as hello action for both.</span></span>

      ![][25]

11. <span data-ttu-id="97337-227">Még tart a hello **tulajdonságok** párbeszédpanel, kattintson a **projektfüggőségek** a bal oldali hello.</span><span class="sxs-lookup"><span data-stu-id="97337-227">Still in hello **Properties** dialog box, click **Project Dependencies** on hello left side.</span></span>
12. <span data-ttu-id="97337-228">A hello **projektek** listában, kattintson **ProductsServer**.</span><span class="sxs-lookup"><span data-stu-id="97337-228">In hello **Projects** list, click **ProductsServer**.</span></span> <span data-ttu-id="97337-229">Győződjön meg arról, hogy a **ProductsPortal** nincs kijelölve.</span><span class="sxs-lookup"><span data-stu-id="97337-229">Ensure that **ProductsPortal** is not selected.</span></span>
13. <span data-ttu-id="97337-230">A hello **projektek** listában, kattintson **ProductsPortal**.</span><span class="sxs-lookup"><span data-stu-id="97337-230">In hello **Projects** list, click **ProductsPortal**.</span></span> <span data-ttu-id="97337-231">Győződjön meg arról, hogy a **ProductsServer** ki van jelölve.</span><span class="sxs-lookup"><span data-stu-id="97337-231">Ensure that **ProductsServer** is selected.</span></span>

    ![][26]

14. <span data-ttu-id="97337-232">Kattintson a **OK** a hello **tulajdonságlapjain** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="97337-232">Click **OK** in hello **Property Pages** dialog box.</span></span>

## <a name="run-hello-project-locally"></a><span data-ttu-id="97337-233">Hello projekt helyi futtatása</span><span class="sxs-lookup"><span data-stu-id="97337-233">Run hello project locally</span></span>

<span data-ttu-id="97337-234">tootest hello alkalmazás helyi, a Visual Studio nyomja le az ENTER **F5**.</span><span class="sxs-lookup"><span data-stu-id="97337-234">tootest hello application locally, in Visual Studio press **F5**.</span></span> <span data-ttu-id="97337-235">hello helyszíni kiszolgáló (**ProductsServer**) kell először, majd hello **ProductsPortal** alkalmazás elindul egy böngészőablakban.</span><span class="sxs-lookup"><span data-stu-id="97337-235">hello on-premises server (**ProductsServer**) should start first, then hello **ProductsPortal** application should start in a browser window.</span></span> <span data-ttu-id="97337-236">Most, látni fogja, hogy hello Termékleltár hello termék szolgáltatás a helyi rendszer származó adatok sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="97337-236">This time, you will see that hello product inventory lists data retrieved from hello product service on-premises system.</span></span>

![][10]

<span data-ttu-id="97337-237">Nyomja le az **frissítése** a hello **ProductsPortal** lap.</span><span class="sxs-lookup"><span data-stu-id="97337-237">Press **Refresh** on hello **ProductsPortal** page.</span></span> <span data-ttu-id="97337-238">Minden alkalommal, amikor hello lap frissítésekor, hello kiszolgáló alkalmazása megjelenít egy üzenetet láthatja Ha `GetProducts()` a **ProductsServer** nevezik.</span><span class="sxs-lookup"><span data-stu-id="97337-238">Each time you refresh hello page, you'll see hello server app display a message when `GetProducts()` from **ProductsServer** is called.</span></span>

<span data-ttu-id="97337-239">Zárja be mindkét alkalmazás toohello következő lépés a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="97337-239">Close both applications before proceeding toohello next step.</span></span>

## <a name="deploy-hello-productsportal-project-tooan-azure-web-app"></a><span data-ttu-id="97337-240">Hello ProductsPortal projekt tooan Azure webalkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="97337-240">Deploy hello ProductsPortal project tooan Azure web app</span></span>

<span data-ttu-id="97337-241">következő lépés hello toorepublish hello Azure Web app **ProductsPortal** előtér.</span><span class="sxs-lookup"><span data-stu-id="97337-241">hello next step is toorepublish hello Azure Web app **ProductsPortal** frontend.</span></span> <span data-ttu-id="97337-242">A következő hello:</span><span class="sxs-lookup"><span data-stu-id="97337-242">Do hello following:</span></span>

1. <span data-ttu-id="97337-243">A Megoldáskezelőben kattintson a jobb gombbal hello **ProductsPortal** projektre, majd kattintson az **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="97337-243">In Solution Explorer, right-click hello **ProductsPortal** project, and click **Publish**.</span></span> <span data-ttu-id="97337-244">Kattintson a **közzététel** a hello **közzététel** lap.</span><span class="sxs-lookup"><span data-stu-id="97337-244">Then, click **Publish** on hello **Publish** page.</span></span>

  > [!NOTE]
  > <span data-ttu-id="97337-245">Megjelenik egy hibaüzenet jelenik meg a hello böngészőablakot hello **ProductsPortal** webes projekt hello telepítés után automatikusan elindul.</span><span class="sxs-lookup"><span data-stu-id="97337-245">You may see an error message in hello browser window when hello **ProductsPortal** web project is automatically launched after hello deployment.</span></span> <span data-ttu-id="97337-246">Ez egy várható üzenet, és akkor fordul elő, mert hello **ProductsServer** alkalmazás még nem fut..</span><span class="sxs-lookup"><span data-stu-id="97337-246">This is expected, and occurs because hello **ProductsServer** application isn't running yet.</span></span>
>
>

2. <span data-ttu-id="97337-247">Másolás hello URL-címe hello üzembe web app alkalmazásban, az szüksége lesz a következő lépésben hello hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="97337-247">Copy hello URL of hello deployed web app, as you will need hello URL in hello next step.</span></span> <span data-ttu-id="97337-248">Az URL-cím is szerezhet be a Visual Studio hello Azure App Service-tevékenység ablakában:</span><span class="sxs-lookup"><span data-stu-id="97337-248">You can also obtain this URL from hello Azure App Service Activity window in Visual Studio:</span></span>

  ![][9]

3. <span data-ttu-id="97337-249">Zárja be a hello böngésző ablak toostop hello alkalmazást futtat.</span><span class="sxs-lookup"><span data-stu-id="97337-249">Close hello browser window toostop hello running application.</span></span>

### <a name="set-productsportal-as-web-app"></a><span data-ttu-id="97337-250">A ProductsPortal beállítása webalkalmazásként</span><span class="sxs-lookup"><span data-stu-id="97337-250">Set ProductsPortal as web app</span></span>

<span data-ttu-id="97337-251">Hello felhőben futó hello alkalmazás előtt győződjön meg arról, hogy **ProductsPortal** Visual Studio webalkalmazásként indult el.</span><span class="sxs-lookup"><span data-stu-id="97337-251">Before running hello application in hello cloud, you must ensure that **ProductsPortal** is launched from within Visual Studio as a web app.</span></span>

1. <span data-ttu-id="97337-252">A Visual Studióban, kattintson a jobb gombbal hello **ProductsPortal** projektre, majd kattintson **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="97337-252">In Visual Studio, right-click hello **ProductsPortal** project and then click **Properties**.</span></span>
2. <span data-ttu-id="97337-253">Kattintson a bal oldali oszlopban hello **webes**.</span><span class="sxs-lookup"><span data-stu-id="97337-253">In hello left-hand column, click **Web**.</span></span>
3. <span data-ttu-id="97337-254">A hello **művelet indítása** területen kattintson a hello **Start URL-cím** gombra, és hello mezőbe írjon be hello URL-címet a korábban telepített webalkalmazás; például `http://productsportal1234567890.azurewebsites.net/`.</span><span class="sxs-lookup"><span data-stu-id="97337-254">In hello **Start Action** section, click hello **Start URL** button, and in hello text box enter hello URL for your previously deployed web app; for example, `http://productsportal1234567890.azurewebsites.net/`.</span></span>

    ![][27]

4. <span data-ttu-id="97337-255">A hello **fájl** a Visual Studio menüjében kattintson **összes mentése**.</span><span class="sxs-lookup"><span data-stu-id="97337-255">From hello **File** menu in Visual Studio, click **Save All**.</span></span>
5. <span data-ttu-id="97337-256">A Visual Studio hello Build menüben kattintson **Rebuild Solution**.</span><span class="sxs-lookup"><span data-stu-id="97337-256">From hello Build menu in Visual Studio, click **Rebuild Solution**.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="97337-257">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="97337-257">Run hello application</span></span>

1. <span data-ttu-id="97337-258">Nyomja le az F5 toobuild és hello alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="97337-258">Press F5 toobuild and run hello application.</span></span> <span data-ttu-id="97337-259">hello helyszíni kiszolgáló (hello **ProductsServer** Konzolalkalmazás) kell először, majd hello **ProductsPortal** alkalmazás elindul egy böngészőablakban, ahogy az a következő képernyő hello hibaüzenetet.</span><span class="sxs-lookup"><span data-stu-id="97337-259">hello on-premises server (hello **ProductsServer** console application) should start first, then hello **ProductsPortal** application should start in a browser window, as shown in hello following screen shot.</span></span> <span data-ttu-id="97337-260">Figyelje meg újra a adott hello Termékleltár hello termék szolgáltatás a helyi rendszer származó adatokat, és jeleníti meg, hogy hello web app alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="97337-260">Notice again that hello product inventory lists data retrieved from hello product service on-premises system, and displays that data in hello web app.</span></span> <span data-ttu-id="97337-261">Hello URL-cím toomake meg arról, hogy ellenőrizze, hogy **ProductsPortal** hello felhőben Azure-webalkalmazás futtatja.</span><span class="sxs-lookup"><span data-stu-id="97337-261">Check hello URL toomake sure that **ProductsPortal** is running in hello cloud, as an Azure web app.</span></span>

   ![][1]

   > [!IMPORTANT]
   > <span data-ttu-id="97337-262">Hello **ProductsServer** Konzolalkalmazás fut, és képesnek kell lennie. tooserve hello adatok toohello **ProductsPortal** alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="97337-262">hello **ProductsServer** console application must be running and able tooserve hello data toohello **ProductsPortal** application.</span></span> <span data-ttu-id="97337-263">Ha hello böngészőben egy hibaüzenet jelenik meg, várjon néhány másodpercet a **ProductsServer** tooload és a következő megjelenítési hello üzenet.</span><span class="sxs-lookup"><span data-stu-id="97337-263">If hello browser displays an error, wait a few more seconds for **ProductsServer** tooload and display hello following message.</span></span> <span data-ttu-id="97337-264">Nyomja le az **frissítése** hello böngészőben.</span><span class="sxs-lookup"><span data-stu-id="97337-264">Then press **Refresh** in hello browser.</span></span>
   >
   >

   ![][37]
2. <span data-ttu-id="97337-265">Vissza a hello böngészőben, nyomja le az **frissítése** a hello **ProductsPortal** lap.</span><span class="sxs-lookup"><span data-stu-id="97337-265">Back in hello browser, press **Refresh** on hello **ProductsPortal** page.</span></span> <span data-ttu-id="97337-266">Minden alkalommal, amikor hello lap frissítésekor, hello kiszolgáló alkalmazása megjelenít egy üzenetet láthatja Ha `GetProducts()` a **ProductsServer** nevezik.</span><span class="sxs-lookup"><span data-stu-id="97337-266">Each time you refresh hello page, you'll see hello server app display a message when `GetProducts()` from **ProductsServer** is called.</span></span>

    ![][38]

## <a name="next-steps"></a><span data-ttu-id="97337-267">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="97337-267">Next steps</span></span>

<span data-ttu-id="97337-268">toolearn Azure továbbítási kapcsolatos további információkért tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="97337-268">toolearn more about Azure Relay, see hello following resources:</span></span>  

* [<span data-ttu-id="97337-269">Mi az az Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="97337-269">What is Azure Relay?</span></span>](relay-what-is-it.md)  
* [<span data-ttu-id="97337-270">Hogyan toouse továbbítása</span><span class="sxs-lookup"><span data-stu-id="97337-270">How toouse Relay</span></span>](service-bus-dotnet-how-to-use-relay.md)  

[0]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hybrid.png
[1]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
[NuGet]: http://nuget.org

[11]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-con-1.png
[13]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-13.png
[15]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-2.png
[16]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-4.png
[17]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-7.png
[18]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-5.png
[9]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-9.png
[10]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App3.png

[21]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App1.png
[24]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-12.png
[25]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-13.png
[26]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-14.png
[27]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-8.png

[36]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
[37]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service1.png
[38]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service2.png
[41]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-40.png
[43]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-43.png
