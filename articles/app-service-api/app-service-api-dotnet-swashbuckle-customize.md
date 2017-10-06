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
# <a name="customize-swashbuckle-generated-api-definitions"></a>Swashbuckle által generált API-definíciók testreszabása
## <a name="overview"></a>Áttekintés
Ez a cikk azt ismerteti, hogyan toocustomize Swashbuckle toohandle gyakori helyzetek ahol érdemes lehet tooalter hello alapértelmezett viselkedést:

* Swashbuckle duplikált műveleti azonosítók vezérlő módszerek túlterhelések állít elő.
* Swashbuckle feltételezi, hogy hello csak egy metódus érvényes válasz HTTP 200 (OK) 

## <a name="customize-operation-identifier-generation"></a>Művelet azonosítója generációs testreszabása
Swashbuckle a Swagger művelet azonosítók vezérlőnév és metódus szerint összefűzésével állít elő. Ebben a mintában a problémát jelent, ha a metódus több túlterheléssel rendelkezik: Swashbuckle hoz létre duplikált műveleti azonosítókat, vagyis a Swagger JSON érvénytelen.

Például a következő vezérlő kód hello okoz Swashbuckle toogenerate három Contact_Get művelet azonosítók.

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsincode.png)

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsinjson.png)

Mivel hello módszerek egyedi nevek, például hello következő ehhez a példához manuálisan megoldható hello problémát:

* Lekérés
* GetById
* GetPage

hello alternatíva a következő: tooextend Swashbuckle toomake automatikus generálása a egyedi műveleti azonosítókat.

hello következő lépések bemutatják, hogyan toocustomize Swashbuckle használatával hello *SwaggerConfig.cs* hello projekt hello Visual Studio API Apps Preview projektsablon által mellékelt fájlt.  Testre szabhatja Swashbuckle állít be központi telepítésben API-alkalmazást is a Web API-projektet.

1. Hozzon létre egyéni `IOperationFilter` végrehajtása 
   
    Hello `IOperationFilter` felületet biztosít egy bővíthetőségi ponton Swashbuckle felhasználók számára, akik toocustomize hello Swagger-metaadatok folyamat különböző jellemzőinek megtekintését. hello következő kód bemutatja, egyik lehetséges módja hello művelet generációs azonosító viselkedésének módosítása. hello kód hozzáfűzi a paraméter nevének toohello műveletet azonosító neve.  
   
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
2. A *App_Start\SwaggerConfig.cs* fájl, a hívás hello `OperationFilter` metódus toocause Swashbuckle toouse hello új `IOperationFilter` végrehajtására.
   
        c.OperationFilter<MultipleOperationsWithSameVerbFilter>();
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/usefilter.png)
   
    Hello *SwaggerConfig.cs* fájlt, amely a Swashbuckle NuGet csomagot hello megszakad megjegyzésként kibővített számos példát bővítési pontokat tartalmazza. hello további megjegyzéseket itt nem látható. 
   
    Ez a módosítás elvégzése után a `IOperationFilter` végrehajtására szolgál, és hatására az egyedi műveleti azonosítókat toobe jön létre.
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/uniqueids.png)

<a id="multiple-response-codes" name="multiple-response-codes"></a>

## <a name="allow-response-codes-other-than-200"></a>Eltérő 200-as válaszkódot engedélyezése
Alapértelmezés szerint Swashbuckle feltételezi, hogy a 200-as (OK) HTTP-választ hello *csak* jogszerű egy webes API-módszer válaszát. Bizonyos esetekben érdemes lehet tooreturn többi válaszkódnál anélkül, hogy ez hello ügyfél tooraise kivételt.  Például hello következő Web API kód bemutatja, egy olyan forgatókönyvet, amelyben hello ügyfél tooaccept célszerű a 200-as vagy a 404-es, érvényes válaszokat.

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

Ebben a forgatókönyvben hello Swagger, amely Swashbuckle hoz létre, alapértelmezés szerint csak egy érvényes HTTP-állapotkód, 200-as HTTP határozza meg.

![](./media/app-service-api-dotnet-swashbuckle-customize/http-200-output-only.png)

Mivel a Visual Studio hello ügyfél hello Swagger API definition toogenerate kódot használja, ügyfélkódot, amely kiváltja a választ egy HTTP 200 eltérő kivételt hoz létre. hello kódot az ügyfélről egy C# jön létre a minta webes API-metódus.

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

Swashbuckle két lehetőséget biztosít a várt HTTP válaszkódot, létrehozott, hello testreszabása XML-megjegyzések vagy hello `SwaggerResponse` attribútum. hello attribútum egyszerűbb, de csak a Swashbuckle 5.1.5 vagy későbbi. hello API-alkalmazások – új projekt sablon megtekintése a Visual Studio 2013 tartalmazza a Swashbuckle verzió 5.0.0, így ha hello sablont használja, és nem szeretné a tooupdate Swashbuckle, egyetlen választása marad: toouse XML-megjegyzések. 

### <a name="customize-expected-response-codes-using-xml-comments"></a>XML-megjegyzések használatával várt válaszkódot testreszabása
A metódus toospecify válaszkódot akkor használja, ha a Swashbuckle verziója korábbi, mint 5.1.5.

1. Először adja hozzá a dokumentációs célú XML hello módszerek a HTTP válaszkódot kívánja toospecify keresztül. Fenti és alkalmazása hello XML dokumentáció tooit eredményeznének kód például a következő példa hello hello minta Web API művelet van folyamatban. 
   
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
2. Adja hozzá az utasításokat a hello *SwaggerConfig.cs* toodirect Swashbuckle toomake használatát hello XML-dokumentációfájl fájlt.
   
   * Nyissa meg *SwaggerConfig.cs* , és létrehoz egy metódust a hello *SwaggerConfig* osztály toospecify hello elérési toohello dokumentációs XML-fájl. 
     
           private static string GetXmlCommentsPath()
           {
               return string.Format(@"{0}\XmlComments.xml", 
                   System.AppDomain.CurrentDomain.BaseDirectory);
           }
   * Görgessen le a hello *SwaggerConfig.cs* fájlt, amíg megjelenik a hello megjegyzésként kibővített kódsort színű hello képernyőfelvétel alatt. 
     
       ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-commented-out.png)
   * Állítsa vissza a hello sor tooenable hello XML-megjegyzések feldolgozása Swagger létrehozása során. 
     
       ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-uncommented.png)
3. Rendelés toogenerate hello XML-dokumentációfájl létrehozása, a állapotba hello projekt tulajdonságait, és engedélyezze a hello XML-dokumentációfájl létrehozása az alábbi hello képernyőfelvételen látható módon. 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/enable-xml-documentation-file.png) 

A fenti lépések végrehajtása után hello Swashbuckle által generált Swagger JSON hello HTTP válaszkódot hello XML-megjegyzések megadott fogja tartalmazni. az alábbi hello képernyőfelvétel az új JSON-adattartalmat mutatja be. 

![](./media/app-service-api-dotnet-swashbuckle-customize/swagger-multiple-responses.png)

Visual Studio tooregenerate hello ügyfélkód a REST API-t használja, ha elfogadja-e hello C#-kódban mindkét hello HTTP OK gombra, és nem található az állapotkódnak nélkül kivételt, így a fogyasztó kód toomake döntést arról, hogyan toohandle hello vissza egy NULL értékű Lépjen kapcsolatba a rekordot. 

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

Ebben a bemutatóban hello kódja megtalálható [a GitHub-tárházban](https://github.com/Azure-Samples/app-service-api-dotnet-swashbuckle-swaggerresponse). Együtt hello webes API projektet a dokumentációs célú XML jelölt egy konzolalkalmazás-projektet, amely tartalmazza a létrehozott az API-ügyfél. 

### <a name="customize-expected-response-codes-using-hello-swaggerresponse-attribute"></a>Hello SwaggerResponse attribútum használatával várt válaszkódot testreszabása
Hello [SwaggerResponse](https://github.com/domaindrivendev/Swashbuckle/blob/master/Swashbuckle.Core/Swagger/Annotations/SwaggerResponseAttribute.cs) attribútum Swashbuckle 5.1.5 és újabb verziói. Abban az esetben, ha egy korábbi verziója van a projekt, ebben a szakaszban először elmagyarázza, hogyan tooupdate hello Swashbuckle NuGet csomagot, hogy ez az attribútum használható.

1. A **Megoldáskezelőben**, kattintson a jobb gombbal a Web API-projektet, és kattintson a **NuGet-csomagok kezelése**. 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/manage-nuget-packages.png)
2. Kattintson a hello *frissítés* gomb következő toohello *Swashbuckle* NuGet-csomagot. 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/update-nuget-dialog.png)
3. Adja hozzá a hello *SwaggerResponse* toohello Web API műveletmetódusokhoz legyen toospecify érvényes HTTP-válaszkódot attribútumok. 
   
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
4. Adja hozzá a `using` hello attribútum névtere utasítást:
   
        using Swashbuckle.Swagger.Annotations;
5. Keresse meg a toohello */swagger/docs/v1* URL-CÍMÉT a projekt és a különböző HTTP-válaszkódot fog megjelenni a hello hello Swagger JSON-NÁ. 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/multiple-responses-post-attributes.png)

Ebben a bemutatóban hello kódja megtalálható [a GitHub-tárházban](https://github.com/Azure-Samples/API-Apps-DotNet-Swashbuckle-Customization-MultipleResponseCodes-With-Attributes). Hello együtt Web API-projektet a hello kitüntetett *SwaggerResponse* attribútum egy konzolalkalmazás-projektet, amely tartalmazza a létrehozott az API-ügyfél. 

## <a name="next-steps"></a>Következő lépések
Ez a cikk azt mutatja, hogyan toocustomize hello módon Swashbuckle hoz létre, művelet azonosítók és érvényes válaszkódot. További információkért lásd: [Swashbuckle a Githubon](https://github.com/domaindrivendev/Swashbuckle).

