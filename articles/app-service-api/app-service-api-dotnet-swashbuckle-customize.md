---
title: "Swashbuckle által generált API-definíciók testreszabása"
description: "Ismerje meg, hogyan szabhatja testre az Azure App Service API-alkalmazás a Swashbuckle által generált Swagger API-definíciót."
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
ms.openlocfilehash: c83905a97fb2ee988fe06fc1f9a7379c1741fd02
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="customize-swashbuckle-generated-api-definitions"></a>Swashbuckle által generált API-definíciók testreszabása
## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti, hogyan szabhatja testre a Swashbuckle gyakori helyzetek kezelésére, ahol érdemes megváltoztatni az alapértelmezett viselkedést:

* Swashbuckle duplikált műveleti azonosítók vezérlő módszerek túlterhelések állít elő.
* Swashbuckle azt feltételezi, hogy a metódus csak érvényes válasz HTTP 200 (OK) 

## <a name="customize-operation-identifier-generation"></a>Művelet azonosítója generációs testreszabása
Swashbuckle a Swagger művelet azonosítók vezérlőnév és metódus szerint összefűzésével állít elő. Ebben a mintában a problémát jelent, ha a metódus több túlterheléssel rendelkezik: Swashbuckle hoz létre duplikált műveleti azonosítókat, vagyis a Swagger JSON érvénytelen.

Például a következő vezérlő okozza Swashbuckle három Contact_Get művelet azonosítók létrehozásához.

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsincode.png)

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsinjson.png)

A probléma manuálisan megoldásában jogosultságot ad a módszerek egyedi nevek, például az ebben a példában a következőket:

* Lekérés
* GetById
* GetPage

Ez esetben kiterjesztése a Swashbuckle abba, hogy automatikusan létrehozni az egyedi műveleti azonosítókat.

A következő lépések bemutatják, hogyan szabhatja testre a Swashbuckle használatával a *SwaggerConfig.cs* a projektet a Visual Studio API Apps Preview projektsablon által mellékelt fájlt.  Testre szabhatja Swashbuckle állít be központi telepítésben API-alkalmazást is a Web API-projektet.

1. Hozzon létre egyéni `IOperationFilter` végrehajtása 
   
    A `IOperationFilter` felület egy bővítési pontot biztosít a Swashbuckle felhasználók, akik különböző szempontjairól a Swagger-metaadatok folyamat testreszabásához. A következő kód bemutatja az egyik lehetséges módja a művelet generációs azonosító viselkedésének módosítása. A kód paraméterneveknek hozzáfűzi a művelet azonosító neve.  
   
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
2. A *App_Start\SwaggerConfig.cs* fájlt, hívja az `OperationFilter` módszer használatához az új Swashbuckle okozhat `IOperationFilter` végrehajtására.
   
        c.OperationFilter<MultipleOperationsWithSameVerbFilter>();
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/usefilter.png)
   
    A *SwaggerConfig.cs* fájlt, amely a Swashbuckle NuGet csomagot megszakad megjegyzésként kibővített számos példát bővítési pontokat tartalmazza. A további megjegyzéseket itt nem látható. 
   
    Ez a módosítás elvégzése után a `IOperationFilter` végrehajtására szolgál, és egyedi műveleti azonosítók létrehozását eredményezi.
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/uniqueids.png)

<a id="multiple-response-codes" name="multiple-response-codes"></a>

## <a name="allow-response-codes-other-than-200"></a>Eltérő 200-as válaszkódot engedélyezése
Alapértelmezés szerint Swashbuckle feltételezi, hogy a 200-as (OK) HTTP-választ a *csak* jogszerű egy webes API-módszer válaszát. Bizonyos esetekben előfordulhat, hogy szeretne visszaállítani többi válaszkódnál anélkül, hogy ez az ügyfél kivételt.  Például a következő webes API-kód bemutatja egy olyan forgatókönyvet, amelyben az ügyfél számára a 200-as vagy a egy érvényes válaszokat, 404 alapvetően szükség.

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

Ebben a forgatókönyvben a Swagger, amely Swashbuckle hoz létre, alapértelmezés szerint csak egy érvényes HTTP-állapotkód, 200-as HTTP határozza meg.

![](./media/app-service-api-dotnet-swashbuckle-customize/http-200-output-only.png)

Mivel a Visual Studio a Swagger API-definíció generálhat kódot az ügyfél használ, ügyfélkódot, amely kiváltja a választ egy HTTP 200 eltérő kivételt hoz létre. Az alábbi kódot a minta webes API-metódus generált C# ügyfél származik.

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

Swashbuckle két lehetőséget biztosít a testreszabása a várt HTTP válaszkódot, létrehozott, XML-megjegyzések használatával vagy a `SwaggerResponse` attribútum. Az attribútum, ezért könnyebben, de csak a Swashbuckle 5.1.5 vagy későbbi. Az API-alkalmazások – új projekt sablon megtekintése a Visual Studio 2013 magában foglalja a Swashbuckle verzió 5.0.0, így ha a sablont használja, és nem szeretné frissíteni a Swashbuckle, egyetlen választása marad: az XML-megjegyzések. 

### <a name="customize-expected-response-codes-using-xml-comments"></a>XML-megjegyzések használatával várt válaszkódot testreszabása
Ezt a módszert a válaszkódot adja meg, ha a Swashbuckle verziója korábbi, mint 5.1.5.

1. Először adja hozzá a dokumentációs célú XML keresztül szeretné megadni, a HTTP válaszkódot módszerek. A mintavétel Web API fenti művelet és az XML-dokumentációban alkalmazása eredményeznének kódot az alábbi példához hasonló. 
   
        /// <summary>
        /// Returns the specified contact.
        /// </summary>
        /// <param name="id">The ID of the contact.</param>
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
2. Adja hozzá a következő témakör utasításait: a *SwaggerConfig.cs* közvetlen Swashbuckle használnia az XML-fájlt dokumentációfájl létrehozása.
   
   * Nyissa meg *SwaggerConfig.cs* és a metódus létrehozása a *SwaggerConfig* osztályra, hogy a dokumentációs XML-fájl elérési útja. 
     
           private static string GetXmlCommentsPath()
           {
               return string.Format(@"{0}\XmlComments.xml", 
                   System.AppDomain.CurrentDomain.BaseDirectory);
           }
   * Görgessen le a *SwaggerConfig.cs* fájlt, amíg megjelenik a sorok megjegyzésként kibővített kódsort az alábbi képernyőfelvételhez hasonlítanak. 
     
       ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-commented-out.png)
   * Állítsa vissza a sor ahhoz, hogy az XML-megjegyzések feldolgozása Swagger létrehozása során. 
     
       ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-uncommented.png)
3. Ahhoz, hogy az XML-dokumentációfájl létrehozása, azokat a projekt tulajdonságait, és engedélyezze a XML-dokumentációfájl létrehozása az alábbi képernyőfelvételen látható módon. 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/enable-xml-documentation-file.png) 

A fenti lépések végrehajtása után a Swashbuckle által generált Swagger JSON a HTTP válaszkódot megadott XML-megjegyzésben fogja tartalmazni. Az alábbi képernyőfelvételen az új JSON-adattartalmat mutatja be. 

![](./media/app-service-api-dotnet-swashbuckle-customize/swagger-multiple-responses.png)

Visual Studio használatával az Ügyfélkód újragenerálja a REST API-hoz, a C#-kódban a HTTP-OK és a nem található az állapotkódnak nélkül kivételt, így a fogyasztó kódnak döntések kezeléséről egy null forduljon rekord visszatérési fogad el. 

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

Ebben a bemutatóban kódja megtalálható [a GitHub-tárházban](https://github.com/Azure-Samples/app-service-api-dotnet-swashbuckle-swaggerresponse). Együtt a webes API projektet a dokumentációs célú XML jelölt egy konzolalkalmazás-projektet, amely tartalmazza a létrehozott az API-ügyfél. 

### <a name="customize-expected-response-codes-using-the-swaggerresponse-attribute"></a>Testre szabhatja a SwaggerResponse attribútum használatával várt válaszkódok
A [SwaggerResponse](https://github.com/domaindrivendev/Swashbuckle/blob/master/Swashbuckle.Core/Swagger/Annotations/SwaggerResponseAttribute.cs) attribútum Swashbuckle 5.1.5 és újabb verziói. Abban az esetben, ha egy korábbi verziója van a projekt, ebben a szakaszban először foglalja össze a Swashbuckle NuGet-csomag frissítése, hogy ez az attribútum használható.

1. A **Megoldáskezelőben**, kattintson a jobb gombbal a Web API-projektet, és kattintson a **NuGet-csomagok kezelése**. 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/manage-nuget-packages.png)
2. Kattintson a *frissítés* megjelenítő gombra a *Swashbuckle* NuGet-csomagot. 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/update-nuget-dialog.png)
3. Adja hozzá a *SwaggerResponse* a Web API műveletmetódusokhoz, amelynek meg szeretné határozni érvényes HTTP-válaszkódot attribútumok. 
   
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
4. Adja hozzá a `using` az attribútum névtere utasítást:
   
        using Swashbuckle.Swagger.Annotations;
5. Keresse meg a */swagger/docs/v1* URL-CÍMÉT a projekt és a különböző HTTP-válaszkódot fognak megjelenni a Swagger JSON-ban. 
   
    ![](./media/app-service-api-dotnet-swashbuckle-customize/multiple-responses-post-attributes.png)

Ebben a bemutatóban kódja megtalálható [a GitHub-tárházban](https://github.com/Azure-Samples/API-Apps-DotNet-Swashbuckle-Customization-MultipleResponseCodes-With-Attributes). A Web API-projektet együtt attribútummal rendelkező a *SwaggerResponse* attribútum egy konzolalkalmazás-projektet, amely tartalmazza a létrehozott az API-ügyfél. 

## <a name="next-steps"></a>Következő lépések
Ez a cikk azt mutatja, hogyan szabhatja testre a Swashbuckle állít elő a művelet azonosítók és érvényes válaszkódot módját. További információkért lásd: [Swashbuckle a Githubon](https://github.com/domaindrivendev/Swashbuckle).

