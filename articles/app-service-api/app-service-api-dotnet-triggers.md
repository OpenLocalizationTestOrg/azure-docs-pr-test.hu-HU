---
title: "aaaApp Service API app eseményindítók |} Microsoft Docs"
description: "Hogyan tooimplement elindítja az API-alkalmazás az Azure App Service-ben"
services: logic-apps
documentationcenter: .net
author: guangyang
manager: erikre
editor: jimbe
ms.assetid: 493c3703-786d-4434-9dca-8f77744b2f5d
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 08/25/2016
ms.author: rachelap
ms.openlocfilehash: 2d6b6a942a23c0a93987e9c48b69ecc739bfd814
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-api-app-triggers"></a>Azure App Service API app triggers (Azure App Service API-alkalmazások eseményindítói)
> [!NOTE]
> Hello cikk e verziója tooAPI apps 2014-12-01-előnézeti sémaverziójára vonatkozik.
>
>

## <a name="overview"></a>Áttekintés
Ez a cikk azt ismerteti, hogyan tooimplement API app váltja ki, és felhasználni azokat a logikai alkalmazás.

Minden ebben a témakörben hello kódtöredékek hello átmásolva [FileWatcher API-alkalmazás kódminta](http://go.microsoft.com/fwlink/?LinkId=534802).

Vegye figyelembe, hogy szüksége lesz a következő hello kód Ez a cikk toobuild a nuget-csomagot, és futtassa toodownload hello: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).

## <a name="what-are-api-app-triggers"></a>Mik azok a API app eseményindítók?
Egy általános forgatókönyv egy API-alkalmazás toofire egy eseményt az, hogy az ügyfelek hello API-alkalmazás is hello megfelelő intézkedéseket válasz toohello eseményben. hello REST API-alapú mechanizmus, amely támogatja az ebben a forgatókönyvben egy API-alkalmazás eseményindító neve.

Például tételezzük fel az Ügyfélkód használ hello [Twitter-összekötő API-alkalmazás](../connectors/connectors-create-api-twitter.md) , ezért a kód tooperform művelet alapuló új Twitter-üzeneteket, amelyek szavakat tartalmaz. Ebben az esetben előfordulhat, hogy beállította egy lekérdezési vagy leküldéses eseményindító toofacilitate igénynek.

## <a name="poll-trigger-versus-push-trigger"></a>Leküldéses eseményindítók és a lekérdezési eseményindító
Jelenleg kétféle típusú eseményindítók támogatottak:

* A lekérdezési eseményindító - ügyfél kérdezze le az értesítés egy eseményt, hogy lejárt a hello API-alkalmazás
* Leküldéses eseményindító - ügyfél értesítést hello API-alkalmazás által az esemény akkor következik be

### <a name="poll-trigger"></a>A lekérdezési eseményindító
A lekérdezési eseményindító valósul meg rendszeres REST API-t, és az ügyfelek (például a logikai alkalmazás) toopoll vár az order tooget értesítés. Hello ügyfél fenntarthatja állapotba, amíg hello lekérdezési eseményindító magát az állapot nélküli.

hello hello kérés- és csomagok vonatkozó információkat a következő néhány kulcsfontosságú elemeit annak hello lekérdezési eseményindító szerződés mutatja be:

* Kérés
  * HTTP-metódus: beolvasása
  * Paraméterek
    * triggerState - a nem kötelező paraméter lehetővé teszi az ügyfelek úgy, hogy a lekérdezési eseményindító hello állapotukra is megfelelően toospecify mellett dönt, hogy tooreturn értesítési vagy nem alapján hello megadott állapot.
    * API-specifikus paramétereket
* Válasz
  * Állapotkód **200** - kérelem érvényes, és értesítést hello eseményindítóval. hello tartalom hello értesítés hello adott válasz törzsének lesz. Hello válaszul "Újrapróbálkozási-után" fejléc azt jelzi, hogy további értesítési adatokat kell beolvasni későbbi kérés hívással.
  * Állapotkód **202** - kérelem érvényes, de nincs új értesítés hello eseményindítóval van.
  * Állapotkód **4xx** -kérelem érvénytelen. hello ügyfél hello kérelem nem újra kell próbálkoznia.
  * Állapotkód **5xx** -kérelmet egy belső kiszolgálóhiba és/vagy az átmeneti hibát eredményezett. hello ügyfél hello kérelem újra kell próbálkoznia.

a következő kódrészletet hello példája hogyan indítható el, a lekérdezési tooimplement.

    // Implement a poll trigger.
    [HttpGet]
    [Route("api/files/poll/TouchedFiles")]
    public HttpResponseMessage TouchedFilesPollTrigger(
        // triggerState is a UTC timestamp
        string triggerState,
        // Additional parameters
        string searchPattern = "*")
    {
        // Check toosee whether there is any file touched after hello timestamp.
        var lastTriggerTimeUtc = DateTime.Parse(triggerState).ToUniversalTime();
        var touchedFiles = Directory.EnumerateFiles(rootPath, searchPattern, SearchOption.AllDirectories)
            .Select(f => FileInfoWrapper.FromFileInfo(new FileInfo(f)))
            .Where(fi => fi.LastAccessTimeUtc > lastTriggerTimeUtc);

        // If there are files touched after hello timestamp, return their information.
        if (touchedFiles != null && touchedFiles.Count() != 0)
        {
            // Extension method provided by hello AppService service SDK.
            return this.Request.EventTriggered(new { files = touchedFiles });
        }
        // If there are no files touched after hello timestamp, tell hello caller toopoll again after 1 mintue.
        else
        {
            // Extension method provided by hello AppService service SDK.
            return this.Request.EventWaitPoll(new TimeSpan(0, 1, 0));
        }
    }

indítás, a lekérdezési tootest, kövesse az alábbi lépéseket:

1. Hello API App hitelesítési-beállítással a telepítése **névtelen nyilvános**.
2. Hello hívás **touch** művelet tootouch egy fájlt. hello kép a következő egy minta kérelem keresztül Postman jeleníti meg.
   ![Postman keresztül Touch művelet hívása](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
3. Hello lekérdezési eseményindító hello hívás **triggerState** paraméterkészlet tooa idő stamp előzetes tooStep #2. hello következő kép bemutatja hello kérelemmintát Postman keresztül.
   ![Hívás lekérdezési eseményindító Postman keresztül](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)

### <a name="push-trigger"></a>Leküldéses eseményindítók
A leküldéses eseményindítót, egy rendszeres REST API-t, amely értesíti, ha a meghatározott események érvényesítést toobe regisztrált értesítések tooclients leküldéses értesítések lett megvalósítva.

hello kérés- és csomagok vonatkozó információkat a következő hello hello leküldéses eseményindító szerződés néhány kulcsfontosságú elemeit mutatják be.

* Kérés
  * HTTP-metódus: PUT
  * Paraméterek
    * eseményindító azonosítója: szükséges – átlátszatlan karakterlánc (például egy GUID), hogy jelöli hello regisztráció leküldéses eseményindító.
    * callbackUrl: szükséges – hello visszahívási tooinvoke hello az esemény akkor következik be, amikor URL-CÍMÉT. hello meghívása egy egyszerű POST HTTP-hívás, amely.
    * API-specifikus paramétereket
* Válasz
  * Állapotkód **200** -kérelem tooregister ügyfél sikeres.
  * Állapotkód **4xx** -kérelem érvénytelen. hello ügyfél hello kérelem nem újra kell próbálkoznia.
  * Állapotkód **5xx** -kérelmet egy belső kiszolgálóhiba és/vagy az átmeneti hibát eredményezett. hello ügyfél hello kérelem újra kell próbálkoznia.
* A visszahívási
  * HTTP-metódus: POST
  * A kérelem törzse: értesítési tartalom.

a következő kódrészletet hello példája hogyan indítható el, egy leküldéses tooimplement:

    // Implement a push trigger.
    [HttpPut]
    [Route("api/files/push/TouchedFiles/{triggerId}")]
    public HttpResponseMessage TouchedFilesPushTrigger(
        // triggerId is an opaque string.
        string triggerId,
        // A helper class provided by hello AppService service SDK.
        // Here it defines hello input of hello push trigger is a string and hello output toohello callback is a FileInfoWrapper object.
        [FromBody]TriggerInput<string, FileInfoWrapper> triggerInput)
    {
        // Register hello trigger toosome trigger store.
        triggerStore.RegisterTrigger(triggerId, rootPath, triggerInput);

        // Extension method provided by hello AppService service SDK indicating hello registration is completed.
        return this.Request.PushTriggerRegistered(triggerInput.GetCallback());
    }

    // A simple in-memory trigger store.
    public class InMemoryTriggerStore
    {
        private static InMemoryTriggerStore instance;

        private IDictionary<string, FileSystemWatcher> _store;

        private InMemoryTriggerStore()
        {
            _store = new Dictionary<string, FileSystemWatcher>();
        }

        public static InMemoryTriggerStore Instance
        {
            get
            {
                if (instance == null)
                {
                    instance = new InMemoryTriggerStore();
                }
                return instance;
            }
        }

        // Register a push trigger.
        public void RegisterTrigger(string triggerId, string rootPath,
            TriggerInput<string, FileInfoWrapper> triggerInput)
        {
            // Use FileSystemWatcher toolisten toofile change event.
            var filter = string.IsNullOrEmpty(triggerInput.inputs) ? "*" : triggerInput.inputs;
            var watcher = new FileSystemWatcher(rootPath, filter);
            watcher.IncludeSubdirectories = true;
            watcher.EnableRaisingEvents = true;
            watcher.NotifyFilter = NotifyFilters.LastAccess;

            // When some file is changed, fire hello push trigger.
            watcher.Changed +=
                (sender, e) => watcher_Changed(sender, e,
                    Runtime.FromAppSettings(),
                    triggerInput.GetCallback());

            // Assoicate hello FileSystemWatcher object with hello triggerId.
            _store[triggerId] = watcher;

        }

        // Fire hello assoicated push trigger when some file is changed.
        void watcher_Changed(object sender, FileSystemEventArgs e,
            // AppService runtime object needed tooinvoke hello callback.
            Runtime runtime,
            // hello callback tooinvoke.
            ClientTriggerCallback<FileInfoWrapper> callback)
        {
            // Helper method provided by AppService service SDK tooinvoke a push trigger callback.
            callback.InvokeAsync(runtime, FileInfoWrapper.FromFileInfo(new FileInfo(e.FullPath)));
        }
    }

indítás, a lekérdezési tootest, kövesse az alábbi lépéseket:

1. Hello API App hitelesítési-beállítással a telepítése **névtelen nyilvános**.
2. Keresse meg a túl[http://requestb.in/](http://requestb.in/) toocreate egy RequestBin, amelyek erre a célra a visszahívási URL-CÍMÉT.
3. Hello leküldéses eseményindító, GUID-nak hívja **eseményindító azonosítója** és RequestBin az URL-címet hello **callbackUrl**.
   ![Hívást leküldéses eseményindító Postman keresztül](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)
4. Hello hívás **touch** művelet tootouch egy fájlt. hello kép a következő egy minta kérelem keresztül Postman jeleníti meg.
   ![Postman keresztül Touch művelet hívása](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
5. Jelölőnégyzet hello RequestBin tooconfirm leküldéses eseményindító visszahívási hello tulajdonság kimeneti hívják meg.
   ![Hívás lekérdezési eseményindító Postman keresztül](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)

### <a name="describe-triggers-in-api-definition"></a>Az API-definíció eseményindítók leírása
Utáni végrehajtási hello eseményindítók és központi telepítése az API-alkalmazás tooAzure, keresse meg a toohello **API-definíció** hello Azure betekintő portálon, és panel jelenik meg, hogy az eseményindítók a rendszer automatikusan felismeri a felhasználói felület, amelyek célja a hello hello Swagger 2.0 API definition hello API-alkalmazás.

![API Definition panel](./media/app-service-api-dotnet-triggers/apidefinitionblade.PNG)

Ha hello **letöltése Swagger** gombra, nyissa meg hello JSON-fájl, látni fogja a eredmények hasonló toohello a következő:

    "/api/files/poll/TouchedFiles": {
      "get": {
        "operationId": "Files_TouchedFilesPollTrigger",
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    },
    "/api/files/push/TouchedFiles/{triggerId}": {
      "put": {
        "operationId": "Files_TouchedFilesPushTrigger",
        ...
        "x-ms-scheduler-trigger": "push"
      }
    }

bővített tulajdonság hello **x-ms-schedular-eseményindító** hogyan eseményindítók részelemcímkék ismertetését API-definíció, és ha hello kérik a tooone hello API definition hello átjárón keresztül igénylésekor automatikusan hello API app átjáró által hozzáadott a következő feltételek hello. (Azt is megteheti ezt a tulajdonságot manuálisan.)

* A lekérdezési eseményindító
  * Ha a HTTP-metódus hello **beolvasása**.
  * Ha hello **OperationID azonosítójú** tulajdonság hello karakterláncot tartalmaz **eseményindító**.
  * Ha hello **paraméterek** tulajdonságánál paraméterrel egy **neve** tulajdonsága túl**triggerState**.
* Leküldéses eseményindítók
  * Ha a HTTP-metódus hello **PUT**.
  * Ha hello **OperationID azonosítójú** tulajdonság hello karakterláncot tartalmaz **eseményindító**.
  * Ha hello **paraméterek** tulajdonságánál paraméterrel egy **neve** tulajdonsága túl**eseményindító azonosítója**.

## <a name="use-api-app-triggers-in-logic-apps"></a>A Logic apps API app eseményindítók használata
### <a name="list-and-configure-api-app-triggers-in-hello-logic-apps-designer"></a>Listázhatja és konfigurálhatja az API app eseményindítók hello Logic apps-tervezőben
Ha létrehoz egy logikai alkalmazás hello erőforráscsoportjában hello API-alkalmazás, képes tooadd lehet azt toohello tervezői vászonra ehhez egyszerűen kattintson rá. a következő lemezképek hello ezt mutatják be:

![A Logic App Designer eseményindítók](./media/app-service-api-dotnet-triggers/triggersinlogicappdesigner.PNG)

![A lekérdezési eseményindító Logic App Designer konfigurálása](./media/app-service-api-dotnet-triggers/configurepolltriggerinlogicappdesigner.PNG)

![A Logic App Designer leküldéses eseményindító konfigurálása](./media/app-service-api-dotnet-triggers/configurepushtriggerinlogicappdesigner.PNG)

## <a name="optimize-api-app-triggers-for-logic-apps"></a>A Logic apps az API app eseményindítók optimalizálása
Eseményindítók tooan API app hozzáadta, van néhány módszert megismerhet tooimprove hello élmény hello API-alkalmazás a logikai alkalmazás használatakor.

Például hello **triggerState** hello logikai alkalmazás kifejezés a következő toohello lekérdezéses eseményindítók paramétert kell beállítani. Ebben a kifejezésben kell kiértékelni hello utolsó meghívása hello eseményindító hello Logic App, és ezt az értéket adja vissza.  

    @coalesce(triggers()?.outputs?.body?['triggerState'], '')

Megjegyzés: A fenti hello kifejezésben használt hello funkció magyarázat, tekintse meg a toohello dokumentációját a [Logic App Munkafolyamatdefiníciós nyelve](https://msdn.microsoft.com/library/azure/dn948512.aspx).

Logic app felhasználók kellene tooprovide hello kifejezés fent a hello **triggerState** paraméter hello eseményindító használata során. Már lehetséges toohave ezt az értéket az adott néven beállítás hello Logic app Designer hello bővítmény tulajdonságon keresztül **x-ms-ütemező – ajánlás**.  Hello **x-ms-láthatósági** bővített tulajdonság értéke tooa állítható be *belső* , hogy maga hello paraméter nem jelenik meg a hello Tervező.  a következő kódrészletet hello mutatja be, amely.

    "/api/Messages/poll": {
      "get": {
        "operationId": "Messages_NewMessageTrigger",
        "parameters": [
          {
            "name": "triggerState",
            "in": "query",
            "required": true,
            "x-ms-visibility": "internal",
            "x-ms-scheduler-recommendation": "@coalesce(triggers()?.outputs?.body?['triggerState'], '')",
            "type": "string"
          }
        ]
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    }

Leküldéses eseményindítók hello **eseményindító azonosítója** paraméter egyedi módon kell azonosítania hello logikai alkalmazást. Ajánlott eljárás a tulajdonságnév toohello hello munkafolyamat használatával hello kifejezés a következő tooset:

    @workflow().name

Hello segítségével **x-ms-ütemező – ajánlás** és **x-ms-láthatósági** bővítmény tulajdonságai a saját API definiton hello API-alkalmazás is átadja toohello Logic app Tervező tooautomatically állítsa hello felhasználói értékeit meghatározó kifejezés.

        "parameters":[  
          {  
            "name":"triggerId",
            "in":"path",
            "required":true,
            "x-ms-visibility":"internal",
            "x-ms-scheduler-recommendation":"@workflow().name",
            "type":"string"
          },


### <a name="add-extension-properties-in-api-defintion"></a>Az API attribútumdefiníciós bővítmény tulajdonságai hozzáadása
További metaadat-információkat – például hello bővítmény tulajdonságai **x-ms-ütemező – ajánlás** és **x-ms-láthatósági** -hello API attribútumdefiníciós az alábbi két módszer egyikével lehet hozzáadni: statikus vagy dinamikus.

A statikus metaadatok közvetlenül szerkesztheti hello */metadata/apiDefinition.swagger.json* a projekt fájlt, és adja hozzá manuálisan hello tulajdonságokat.

Dinamikus metaadatok segítségével API-alkalmazások esetén szerkesztheti hello SwaggerConfig.cs fájl tooadd egy művelet szűrőt, amely ezen bővítmények adhat hozzá.

    GlobalConfiguration.Configuration
        .EnableSwagger(c =>
            {
                ...
                c.OperationFilter<TriggerStateFilter>();
                ...
            }


hello Ez az osztály hogyan lehet megvalósított toofacilitate hello dinamikus metaadatok forgatókönyv egy példa látható.

    // Add extension properties on hello triggerState parameter
    public class TriggerStateFilter : IOperationFilter
    {

        public void Apply(Operation operation, SchemaRegistry schemaRegistry, System.Web.Http.Description.ApiDescription apiDescription)
        {
            if (operation.operationId.IndexOf("Trigger", StringComparison.InvariantCultureIgnoreCase) >= 0)
            {
                // this is a possible trigger
                var triggerStateParam = operation.parameters.FirstOrDefault(x => x.name.Equals("triggerState"));
                if (triggerStateParam != null)
                {
                    if (triggerStateParam.vendorExtensions == null)
                    {
                        triggerStateParam.vendorExtensions = new Dictionary<string, object>();
                    }

                    // add 2 vendor extensions
                    // x-ms-visibility: set too'internal' toosignify this is an internal field
                    // x-ms-scheduler-recommendation: set tooa value that logic app can use
                    triggerStateParam.vendorExtensions.Add("x-ms-visibility", "internal");
                    triggerStateParam.vendorExtensions.Add("x-ms-scheduler-recommendation",
                                                           "@coalesce(triggers()?.outputs?.body?['triggerState'], '')");
                }
            }
        }
    }
