---
title: "aaaCustomize Swashbuckle által generált API-definíciók"
description: "Megtudhatja, hogyan toocustomize Swagger API-definíciók, az Azure App Service API-alkalmazás a Swashbuckle által generált."
services: app-service\api
documentationcenter: .net
author: bradygaster
manager: erikre
editor: jimbe
ms.assetid: 6b8cbc38-d282-4a0f-b0c5-762631bae6f3
ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 08/29/2016
ms.author: rachelap
ms.openlocfilehash: e31c665f8993533c5ec9a935e42cce34f86a5ade
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customize-swashbuckle-generated-api-definitions"></a><span data-ttu-id="8df93-103">Swashbuckle által generált API-definíciók testreszabása</span><span class="sxs-lookup"><span data-stu-id="8df93-103">Customize Swashbuckle-generated API definitions</span></span>
## <a name="overview"></a><span data-ttu-id="8df93-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="8df93-104">Overview</span></span>
<span data-ttu-id="8df93-105">Ez a cikk azt ismerteti, hogyan toocustomize Swashbuckle toohandle gyakori helyzetek ahol érdemes lehet tooalter hello alapértelmezett viselkedést:</span><span class="sxs-lookup"><span data-stu-id="8df93-105">This article explains how toocustomize Swashbuckle toohandle common scenarios where you may want tooalter hello default behavior:</span></span>

* <span data-ttu-id="8df93-106">Swashbuckle duplikált műveleti azonosítók vezérlő módszerek túlterhelések állít elő.</span><span class="sxs-lookup"><span data-stu-id="8df93-106">Swashbuckle generates duplicate operation identifiers for overloads of controller methods</span></span>
* <span data-ttu-id="8df93-107">Swashbuckle feltételezi, hogy hello csak egy metódus érvényes válasz HTTP 200 (OK)</span><span class="sxs-lookup"><span data-stu-id="8df93-107">Swashbuckle assumes that hello only valid response from a method is HTTP 200 (OK)</span></span> 

## <a name="customize-operation-identifier-generation"></a><span data-ttu-id="8df93-108">Művelet azonosítója generációs testreszabása</span><span class="sxs-lookup"><span data-stu-id="8df93-108">Customize operation identifier generation</span></span>
<span data-ttu-id="8df93-109">Swashbuckle a Swagger művelet azonosítók vezérlőnév és metódus szerint összefűzésével állít elő.</span><span class="sxs-lookup"><span data-stu-id="8df93-109">Swashbuckle generates Swagger operation identifiers by concatenating controller name and method name.</span></span> <span data-ttu-id="8df93-110">Ebben a mintában a problémát jelent, ha a metódus több túlterheléssel rendelkezik: Swashbuckle hoz létre duplikált műveleti azonosítókat, vagyis a Swagger JSON érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="8df93-110">This pattern creates a problem when you have multiple overloads of a method: Swashbuckle generates duplicate operation ids, which is invalid Swagger JSON.</span></span>

<span data-ttu-id="8df93-111">Például a következő vezérlő kód hello okoz Swashbuckle toogenerate három Contact_Get művelet azonosítók.</span><span class="sxs-lookup"><span data-stu-id="8df93-111">For example, hello following controller code causes Swashbuckle toogenerate three Contact_Get operation ids.</span></span>

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsincode.png)

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsinjson.png)

<span data-ttu-id="8df93-112">Mivel hello módszerek egyedi nevek, például hello következő ehhez a példához manuálisan megoldható hello problémát:</span><span class="sxs-lookup"><span data-stu-id="8df93-112">You can solve hello problem manually by giving hello methods unique names, such as hello following for this example:</span></span>

* <span data-ttu-id="8df93-113">Lekérés</span><span class="sxs-lookup"><span data-stu-id="8df93-113">Get</span></span>
* <span data-ttu-id="8df93-114">GetById</span><span class="sxs-lookup"><span data-stu-id="8df93-114">GetById</span></span>
* <span data-ttu-id="8df93-115">GetPage</span><span class="sxs-lookup"><span data-stu-id="8df93-115">GetPage</span></span>

<span data-ttu-id="8df93-116">hello alternatíva a következő: tooextend Swashbuckle toomake automatikus generálása a egyedi műveleti azonosítókat.</span><span class="sxs-lookup"><span data-stu-id="8df93-116">hello alternative is tooextend Swashbuckle toomake it automatically generate unique operation ids.</span></span>

<span data-ttu-id="8df93-117">hello következő lépések bemutatják, hogyan toocustomize Swashbuckle használatával hello *SwaggerConfig.cs* hello projekt hello Visual Studio API Apps Preview projektsablon által mellékelt fájlt.</span><span class="sxs-lookup"><span data-stu-id="8df93-117">hello following steps show how toocustomize Swashbuckle by using hello *SwaggerConfig.cs* file that is included in hello project by hello Visual Studio API Apps Preview project template.</span></span>  <span data-ttu-id="8df93-118">Testre szabhatja Swashbuckle állít be központi telepítésben API-alkalmazást is a Web API-projektet.</span><span class="sxs-lookup"><span data-stu-id="8df93-118">You can also customize Swashbuckle in a Web API project that you configure for deployment as an API app.</span></span>

1. <span data-ttu-id="8df93-119">Hozzon létre egyéni `IOperationFilter` végrehajtása</span><span class="sxs-lookup"><span data-stu-id="8df93-119">Create a custom `IOperationFilter` implementation</span></span> 
   
    <span data-ttu-id="8df93-120">Hello `IOperationFilter` felületet biztosít egy bővíthetőségi ponton Swashbuckle felhasználók számára, akik toocustomize hello Swagger-metaadatok folyamat különböző jellemzőinek megtekintését.</span><span class="sxs-lookup"><span data-stu-id="8df93-120">hello `IOperationFilter` interface provides an extensibility point for Swashbuckle users who want toocustomize various aspects of hello Swagger metadata process.</span></span> <span data-ttu-id="8df93-121">hello következő kód bemutatja, egyik lehetséges módja hello művelet generációs azonosító viselkedésének módosítása.</span><span class="sxs-lookup"><span data-stu-id="8df93-121">hello following code demonstrates one method of changing hello operation-id-generation behavior.</span></span> <span data-ttu-id="8df93-122">hello kód hozzáfűzi a paraméter nevének toohello műveletet azonosító neve.</span><span class="sxs-lookup"><span data-stu-id="8df93-122">hello code appends parameter names toohello operation id name.</span></span>  
   
        using Swashbuckle.Swagger;
        using System.Web.Http.Description;
   
        namespace ContactsList
        {
            public class MultipleOperationsWithSameVerbFilter : IOperationFilter
            {
                public void Apply(
                    Operation operation,
                    SchemaRegistry schemaRegistry,
                    ApiDescription apiDescription)
                {
                    if (operation.parameters != null)
                    {
                        operation.operationId += "By";
                        foreach (var parm in operation.parameters)
                        {
                            operation.operationId += string.Format("{0}",parm.name);
                        }
                    }
                }
            }
        }
2. <span data-ttu-id="8df93-123">A *App_Start\SwaggerConfig.cs* fájl, a hívás hello `OperationFilter` metódus toocause Swashbuckle toouse hello új `IOperationFilter` végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="8df93-123">In *App_Start\SwaggerConfig.cs* file, call hello `OperationFilter` method toocause Swashbuckle toouse hello new `IOperationFilter` implementation.</span></span>
   
        c.OperationFilter<MultipleOperationsWithSameVerbFilter>();
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/usefilter.png)
   
    <span data-ttu-id="8df93-124">Hello *SwaggerConfig.cs* fájlt, amely a Swashbuckle NuGet csomagot hello megszakad megjegyzésként kibővített számos példát bővítési pontokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8df93-124">hello *SwaggerConfig.cs* file that is dropped in by hello Swashbuckle NuGet package contains many commented-out examples of extensibility points.</span></span> <span data-ttu-id="8df93-125">hello további megjegyzéseket itt nem látható.</span><span class="sxs-lookup"><span data-stu-id="8df93-125">hello additional comments are not shown here.</span></span> 
   
    <span data-ttu-id="8df93-126">Ez a módosítás elvégzése után a `IOperationFilter` végrehajtására szolgál, és hatására az egyedi műveleti azonosítókat toobe jön létre.</span><span class="sxs-lookup"><span data-stu-id="8df93-126">After you make this change, your `IOperationFilter` implementation is used and causes unique operation ids toobe generated.</span></span>
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/uniqueids.png)

<a id="multiple-response-codes" name="multiple-response-codes"></a>

## <a name="allow-response-codes-other-than-200"></a><span data-ttu-id="8df93-127">Eltérő 200-as válaszkódot engedélyezése</span><span class="sxs-lookup"><span data-stu-id="8df93-127">Allow response codes other than 200</span></span>
<span data-ttu-id="8df93-128">Alapértelmezés szerint Swashbuckle feltételezi, hogy a 200-as (OK) HTTP-választ hello *csak* jogszerű egy webes API-módszer válaszát.</span><span class="sxs-lookup"><span data-stu-id="8df93-128">By default, Swashbuckle assumes that an HTTP 200 (OK) response is hello *only* legitimate response from a Web API method.</span></span> <span data-ttu-id="8df93-129">Bizonyos esetekben érdemes lehet tooreturn többi válaszkódnál anélkül, hogy ez hello ügyfél tooraise kivételt.</span><span class="sxs-lookup"><span data-stu-id="8df93-129">In some cases, you may want tooreturn other response codes without causing hello client tooraise an exception.</span></span>  <span data-ttu-id="8df93-130">Például hello következő Web API kód bemutatja, egy olyan forgatókönyvet, amelyben hello ügyfél tooaccept célszerű a 200-as vagy a 404-es, érvényes válaszokat.</span><span class="sxs-lookup"><span data-stu-id="8df93-130">For example, hello following Web API code demonstrates a scenario in which you would want hello client tooaccept either a 200 or a 404 as valid responses.</span></span>

    [ResponseType(typeof(Contact))]
    public HttpResponseMessage Get(int id)
    {
        var contacts = GetContacts();

        var requestedContact = contacts.FirstOrDefault(x => x.Id == id);

        if (requestedContact == null)
        {
            return Request.CreateResponse(HttpStatusCode.NotFound);
        }
        else
        {
            return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
        }
    }

<span data-ttu-id="8df93-131">Ebben a forgatókönyvben hello Swagger, amely Swashbuckle hoz létre, alapértelmezés szerint csak egy érvényes HTTP-állapotkód, 200-as HTTP határozza meg.</span><span class="sxs-lookup"><span data-stu-id="8df93-131">In this scenario, hello Swagger that Swashbuckle generates by default specifies only one legitimate HTTP status code, HTTP 200.</span></span>

![](./media/app-service-api-dotnet-swashbuckle-customize/http-200-output-only.png)

<span data-ttu-id="8df93-132">Mivel a Visual Studio hello ügyfél hello Swagger API definition toogenerate kódot használja, ügyfélkódot, amely kiváltja a választ egy HTTP 200 eltérő kivételt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="8df93-132">Since Visual Studio uses hello Swagger API definition toogenerate code for hello client, it creates client code that raises an exception for any response other than an HTTP 200.</span></span> <span data-ttu-id="8df93-133">hello kódot az ügyfélről egy C# jön létre a minta webes API-metódus.</span><span class="sxs-lookup"><span data-stu-id="8df93-133">hello code below is from a C# client generated for this sample Web API method.</span></span>

    if (statusCode != HttpStatusCode.OK)
    {
        HttpOperationException<object> ex = new HttpOperationException<object>();
        ex.Request = httpRequest;
        ex.Response = httpResponse;
        ex.Body = null;
        if (shouldTrace)
        {
            ServiceClientTracing.Error(invocationId, ex);
        }
        throw ex;
    } 

<span data-ttu-id="8df93-134">Swashbuckle két lehetőséget biztosít a várt HTTP válaszkódot, létrehozott, hello testreszabása XML-megjegyzések vagy hello `SwaggerResponse` attribútum.</span><span class="sxs-lookup"><span data-stu-id="8df93-134">Swashbuckle provides two ways of customizing hello list of expected HTTP response codes that it generates, using XML comments or hello `SwaggerResponse` attribute.</span></span> <span data-ttu-id="8df93-135">hello attribútum egyszerűbb, de csak a Swashbuckle 5.1.5 vagy későbbi.</span><span class="sxs-lookup"><span data-stu-id="8df93-135">hello attribute is easier, but it is only available in Swashbuckle 5.1.5 or later.</span></span> <span data-ttu-id="8df93-136">hello API-alkalmazások – új projekt sablon megtekintése a Visual Studio 2013 tartalmazza a Swashbuckle verzió 5.0.0, így ha hello sablont használja, és nem szeretné a tooupdate Swashbuckle, egyetlen választása marad: toouse XML-megjegyzések.</span><span class="sxs-lookup"><span data-stu-id="8df93-136">hello API Apps preview new-project template in Visual Studio 2013 includes Swashbuckle version 5.0.0, so if you used hello template and don't want tooupdate Swashbuckle, your only option is toouse XML comments.</span></span> 

### <a name="customize-expected-response-codes-using-xml-comments"></a><span data-ttu-id="8df93-137">XML-megjegyzések használatával várt válaszkódot testreszabása</span><span class="sxs-lookup"><span data-stu-id="8df93-137">Customize expected response codes using XML comments</span></span>
<span data-ttu-id="8df93-138">A metódus toospecify válaszkódot akkor használja, ha a Swashbuckle verziója korábbi, mint 5.1.5.</span><span class="sxs-lookup"><span data-stu-id="8df93-138">Use this method toospecify response codes if your Swashbuckle version is earlier than 5.1.5.</span></span>

1. <span data-ttu-id="8df93-139">Először adja hozzá a dokumentációs célú XML hello módszerek a HTTP válaszkódot kívánja toospecify keresztül.</span><span class="sxs-lookup"><span data-stu-id="8df93-139">First, add XML documentation comments over hello methods you wish toospecify HTTP response codes for.</span></span> <span data-ttu-id="8df93-140">Fenti és alkalmazása hello XML dokumentáció tooit eredményeznének kód például a következő példa hello hello minta Web API művelet van folyamatban.</span><span class="sxs-lookup"><span data-stu-id="8df93-140">Taking hello sample Web API action shown above and applying hello XML documentation tooit would result in code like hello following example.</span></span> 
   
        /// <summary>
        /// Returns hello specified contact.
        /// </summary>
        /// <param name="id">hello ID of hello contact.</param>
        /// <returns>A contact record with an HTTP 200, or null with an HTTP 404.</returns>
        /// <response code="200">OK</response>
        /// <response code="404">Not Found</response>
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();
   
            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
   
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }
2. <span data-ttu-id="8df93-141">Adja hozzá az utasításokat a hello *SwaggerConfig.cs* toodirect Swashbuckle toomake használatát hello XML-dokumentációfájl fájlt.</span><span class="sxs-lookup"><span data-stu-id="8df93-141">Add instructions in hello *SwaggerConfig.cs* file toodirect Swashbuckle toomake use of hello XML documentation file.</span></span>
   
   * <span data-ttu-id="8df93-142">Nyissa meg *SwaggerConfig.cs* , és létrehoz egy metódust a hello *SwaggerConfig* osztály toospecify hello elérési toohello dokumentációs XML-fájl.</span><span class="sxs-lookup"><span data-stu-id="8df93-142">Open *SwaggerConfig.cs* and create a method on hello *SwaggerConfig* class toospecify hello path toohello documentation XML file.</span></span> 
     
           private static string GetXmlCommentsPath()
           {
               return string.Format(@"{0}\XmlComments.xml", 
                   System.AppDomain.CurrentDomain.BaseDirectory);
           }
   * <span data-ttu-id="8df93-143">Görgessen le a hello *SwaggerConfig.cs* fájlt, amíg megjelenik a hello megjegyzésként kibővített kódsort színű hello képernyőfelvétel alatt.</span><span class="sxs-lookup"><span data-stu-id="8df93-143">Scroll down in hello *SwaggerConfig.cs* file until you see hello commented-out line of code resembling hello screen shot below.</span></span> 
     
       ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-commented-out.png)
   * <span data-ttu-id="8df93-144">Állítsa vissza a hello sor tooenable hello XML-megjegyzések feldolgozása Swagger létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="8df93-144">Uncomment hello line tooenable hello XML comments processing during Swagger generation.</span></span> 
     
       ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-uncommented.png)
3. <span data-ttu-id="8df93-145">Rendelés toogenerate hello XML-dokumentációfájl létrehozása, a állapotba hello projekt tulajdonságait, és engedélyezze a hello XML-dokumentációfájl létrehozása az alábbi hello képernyőfelvételen látható módon.</span><span class="sxs-lookup"><span data-stu-id="8df93-145">In order toogenerate hello XML documentation file, go into hello project's properties and enable hello XML documentation file as shown in hello screenshot below.</span></span> 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/enable-xml-documentation-file.png) 

<span data-ttu-id="8df93-146">A fenti lépések végrehajtása után hello Swashbuckle által generált Swagger JSON hello HTTP válaszkódot hello XML-megjegyzések megadott fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="8df93-146">Once you perform these steps, hello Swagger JSON generated by Swashbuckle will reflect hello HTTP response codes that you specified in hello XML comments.</span></span> <span data-ttu-id="8df93-147">az alábbi hello képernyőfelvétel az új JSON-adattartalmat mutatja be.</span><span class="sxs-lookup"><span data-stu-id="8df93-147">hello screenshot below demonstrates this new JSON payload.</span></span> 

![](./media/app-service-api-dotnet-swashbuckle-customize/swagger-multiple-responses.png)

<span data-ttu-id="8df93-148">Visual Studio tooregenerate hello ügyfélkód a REST API-t használja, ha elfogadja-e hello C#-kódban mindkét hello HTTP OK gombra, és nem található az állapotkódnak nélkül kivételt, így a fogyasztó kód toomake döntést arról, hogyan toohandle hello vissza egy NULL értékű Lépjen kapcsolatba a rekordot.</span><span class="sxs-lookup"><span data-stu-id="8df93-148">When you use Visual Studio tooregenerate hello client code for your REST API, hello C# code accepts both hello HTTP OK and Not Found status codes without raising an exception, allowing your consuming code toomake decisions on how toohandle hello return of a null Contact record.</span></span> 

        if (statusCode != HttpStatusCode.OK && statusCode != HttpStatusCode.NotFound)
        {
            HttpOperationException<object> ex = new HttpOperationException<object>();
            ex.Request = httpRequest;
            ex.Response = httpResponse;
            ex.Body = null;
            if (shouldTrace)
            {
                ServiceClientTracing.Error(invocationId, ex);
            }
                throw ex;
        }

<span data-ttu-id="8df93-149">Ebben a bemutatóban hello kódja megtalálható [a GitHub-tárházban](https://github.com/Azure-Samples/app-service-api-dotnet-swashbuckle-swaggerresponse).</span><span class="sxs-lookup"><span data-stu-id="8df93-149">hello code for this demonstration can be found in [this GitHub repository](https://github.com/Azure-Samples/app-service-api-dotnet-swashbuckle-swaggerresponse).</span></span> <span data-ttu-id="8df93-150">Együtt hello webes API projektet a dokumentációs célú XML jelölt egy konzolalkalmazás-projektet, amely tartalmazza a létrehozott az API-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="8df93-150">Along with hello Web API project marked up with XML documentation comments is a Console Application project that contains a generated client for this API.</span></span> 

### <a name="customize-expected-response-codes-using-hello-swaggerresponse-attribute"></a><span data-ttu-id="8df93-151">Hello SwaggerResponse attribútum használatával várt válaszkódot testreszabása</span><span class="sxs-lookup"><span data-stu-id="8df93-151">Customize expected response codes using hello SwaggerResponse attribute</span></span>
<span data-ttu-id="8df93-152">Hello [SwaggerResponse](https://github.com/domaindrivendev/Swashbuckle/blob/master/Swashbuckle.Core/Swagger/Annotations/SwaggerResponseAttribute.cs) attribútum Swashbuckle 5.1.5 és újabb verziói.</span><span class="sxs-lookup"><span data-stu-id="8df93-152">hello [SwaggerResponse](https://github.com/domaindrivendev/Swashbuckle/blob/master/Swashbuckle.Core/Swagger/Annotations/SwaggerResponseAttribute.cs) attribute is available in Swashbuckle 5.1.5 and later.</span></span> <span data-ttu-id="8df93-153">Abban az esetben, ha egy korábbi verziója van a projekt, ebben a szakaszban először elmagyarázza, hogyan tooupdate hello Swashbuckle NuGet csomagot, hogy ez az attribútum használható.</span><span class="sxs-lookup"><span data-stu-id="8df93-153">In case you have an earlier version in your project, this section starts by explaining how tooupdate hello Swashbuckle NuGet package so that you can use this attribute.</span></span>

1. <span data-ttu-id="8df93-154">A **Megoldáskezelőben**, kattintson a jobb gombbal a Web API-projektet, és kattintson a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="8df93-154">In **Solution Explorer**, right-click your Web API project and click **Manage NuGet Packages**.</span></span> 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/manage-nuget-packages.png)
2. <span data-ttu-id="8df93-155">Kattintson a hello *frissítés* gomb következő toohello *Swashbuckle* NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="8df93-155">Click hello *Update* button next toohello *Swashbuckle* NuGet package.</span></span> 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/update-nuget-dialog.png)
3. <span data-ttu-id="8df93-156">Adja hozzá a hello *SwaggerResponse* toohello Web API műveletmetódusokhoz legyen toospecify érvényes HTTP-válaszkódot attribútumok.</span><span class="sxs-lookup"><span data-stu-id="8df93-156">Add hello *SwaggerResponse* attributes toohello Web API action methods for which you want toospecify valid HTTP response codes.</span></span> 
   
        [SwaggerResponse(HttpStatusCode.OK)]
        [SwaggerResponse(HttpStatusCode.NotFound)]
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();
   
            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }
4. <span data-ttu-id="8df93-157">Adja hozzá a `using` hello attribútum névtere utasítást:</span><span class="sxs-lookup"><span data-stu-id="8df93-157">Add a `using` statement for hello attribute's namespace:</span></span>
   
        using Swashbuckle.Swagger.Annotations;
5. <span data-ttu-id="8df93-158">Keresse meg a toohello */swagger/docs/v1* URL-CÍMÉT a projekt és a különböző HTTP-válaszkódot fog megjelenni a hello hello Swagger JSON-NÁ.</span><span class="sxs-lookup"><span data-stu-id="8df93-158">Browse toohello */swagger/docs/v1* URL of your project and hello various HTTP response codes will be visible in hello Swagger JSON.</span></span> 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/multiple-responses-post-attributes.png)

<span data-ttu-id="8df93-159">Ebben a bemutatóban hello kódja megtalálható [a GitHub-tárházban](https://github.com/Azure-Samples/API-Apps-DotNet-Swashbuckle-Customization-MultipleResponseCodes-With-Attributes).</span><span class="sxs-lookup"><span data-stu-id="8df93-159">hello code for this demonstration can be found in [this GitHub repository](https://github.com/Azure-Samples/API-Apps-DotNet-Swashbuckle-Customization-MultipleResponseCodes-With-Attributes).</span></span> <span data-ttu-id="8df93-160">Hello együtt Web API-projektet a hello kitüntetett *SwaggerResponse* attribútum egy konzolalkalmazás-projektet, amely tartalmazza a létrehozott az API-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="8df93-160">Along with hello Web API project decorated with hello *SwaggerResponse* attribute is a Console Application project that contains a generated client for this API.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="8df93-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8df93-161">Next steps</span></span>
<span data-ttu-id="8df93-162">Ez a cikk azt mutatja, hogyan toocustomize hello módon Swashbuckle hoz létre, művelet azonosítók és érvényes válaszkódot.</span><span class="sxs-lookup"><span data-stu-id="8df93-162">This article has shown how toocustomize hello way Swashbuckle generates operation ids and valid response codes.</span></span> <span data-ttu-id="8df93-163">További információkért lásd: [Swashbuckle a Githubon](https://github.com/domaindrivendev/Swashbuckle).</span><span class="sxs-lookup"><span data-stu-id="8df93-163">For more information, see [Swashbuckle on GitHub](https://github.com/domaindrivendev/Swashbuckle).</span></span>

