---
title: "Az App Service API app eseményindítók |} Microsoft Docs"
description: "Az Azure App Service API-alkalmazás az eseményindítók implementálása"
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
ms.openlocfilehash: 3ddfb142e7f1a47e2a8564387da785acf36fa61f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-api-app-triggers"></a>Azure App Service API app triggers (Azure App Service API-alkalmazások eseményindítói)
> [!NOTE]
> A cikk e verziója API apps 2014 12-01. dátumú előnézeti sémaverziójára vonatkozik.
>
>

## <a name="overview"></a>Áttekintés
Ez a cikk azt ismerteti, hogyan API app eseményindítók és a logikai alkalmazás felhasználni azokat.

Minden ebben a témakörben a kódrészleteket másolja át a [FileWatcher API-alkalmazás kódminta](http://go.microsoft.com/fwlink/?LinkId=534802).

Vegye figyelembe, hogy töltse le a kód a cikk létrehozása és futtatása a következő nuget-csomagot kell: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).

## <a name="what-are-api-app-triggers"></a>Mik azok a API app eseményindítók?
Egy általános forgatókönyv a API-alkalmazás egy eseményt, hogy az ügyfelek az API-alkalmazás az esemény bekövetkeztekor is elvégezheti a szükséges műveleteket. A REST API-alapú mechanizmus, amely támogatja az ebben a forgatókönyvben egy API-alkalmazás eseményindító neve.

Például tételezzük fel az Ügyfélkód használ a [Twitter-összekötő API-alkalmazás](../connectors/connectors-create-api-twitter.md) és a kódot kell egy műveletet, új Twitter-üzeneteket, amelyek szavakat tartalmaz alapján. Ebben az esetben előfordulhat, hogy beállította egy lekérdezési vagy leküldéses eseményindító igénynek megkönnyítése érdekében.

## <a name="poll-trigger-versus-push-trigger"></a>Leküldéses eseményindítók és a lekérdezési eseményindító
Jelenleg kétféle típusú eseményindítók támogatottak:

* A lekérdezési eseményindító - ügyfél kérdezze le az értesítéshez egy esemény, amelynek lejárt az API-alkalmazás
* Leküldéses eseményindító - ügyfél értesítést kaphat az API-alkalmazás által az esemény akkor következik be

### <a name="poll-trigger"></a>A lekérdezési eseményindító
Lekérdezési eseményindító valósul meg rendszeres REST API-t, és az ügyfelek (például a logikai alkalmazás) vár el, és kérdezze le az értesítési eléréséhez. Amíg az ügyfél esetleg-állapot karbantartásához, a lekérdezési eseményindító maga állapotmentes.

A következő adatokat a kérelem-válasz csomagok kapcsolatban néhány kulcsfontosságú elemeit annak a lekérdezési eseményindító szerződés mutatja be:

* Kérés
  * HTTP-metódus: beolvasása
  * Paraméterek
    * triggerState - a nem kötelező paraméter lehetővé teszi az ügyfelek állapotukra, hogy a lekérdezési eseményindító is megfelelően be kell-e értesítés ad vissza, vagy nem a megadott állapotán alapuló megadását.
    * API-specifikus paramétereket
* Válasz
  * Állapotkód **200** - kérelem érvényes, és a eseményindítóval értesítést. Az értesítési tartalom az adott válasz törzsének lesz. A válaszban szereplő "Újrapróbálkozási-után" fejléc azt jelzi, hogy további értesítési adatokat kell beolvasni későbbi kérés hívással.
  * Állapotkód **202** - kérelem érvényes, de nincs új értesítés a eseményindítóval van.
  * Állapotkód **4xx** -kérelem érvénytelen. Az ügyfél nem kell újra a kéréssel.
  * Állapotkód **5xx** -kérelmet egy belső kiszolgálóhiba és/vagy az átmeneti hibát eredményezett. Az ügyfél kell újra a kéréssel.

A következő kódrészlet példa bemutatja, hogyan lekérdezési eseményindító végrehajtása.

    // Implement a poll trigger.
    [HttpGet]
    [Route("api/files/poll/TouchedFiles")]
    public HttpResponseMessage TouchedFilesPollTrigger(
        // triggerState is a UTC timestamp
        string triggerState,
        // Additional parameters
        string searchPattern = "*")
    {
        // Check to see whether there is any file touched after the timestamp.
        var lastTriggerTimeUtc = DateTime.Parse(triggerState).ToUniversalTime();
        var touchedFiles = Directory.EnumerateFiles(rootPath, searchPattern, SearchOption.AllDirectories)
            .Select(f => FileInfoWrapper.FromFileInfo(new FileInfo(f)))
            .Where(fi => fi.LastAccessTimeUtc > lastTriggerTimeUtc);

        // If there are files touched after the timestamp, return their information.
        if (touchedFiles != null && touchedFiles.Count() != 0)
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventTriggered(new { files = touchedFiles });
        }
        // If there are no files touched after the timestamp, tell the caller to poll again after 1 mintue.
        else
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventWaitPoll(new TimeSpan(0, 1, 0));
        }
    }

A lekérdezési eseményindító teszteléséhez kövesse az alábbi lépéseket:

1. Egy hitelesítés használatára az API-alkalmazás telepítése **névtelen nyilvános**.
2. Hívja a **touch** fájl touch művelet. Az alábbi ábrán egy minta kérelem Postman keresztül.
   ![Postman keresztül Touch művelet hívása](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
3. A lekérdezési eseményindító hívja a **triggerState** paraméter értéke egy időbélyegző #2. lépés előtt. A következő kép bemutatja a kérelemmintát Postman keresztül.
   ![Hívás lekérdezési eseményindító Postman keresztül](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)

### <a name="push-trigger"></a>Leküldéses eseményindítók
A leküldéses eseményindítót, egy rendszeres REST API-t, amely a leküldéses értesítések értesítések az ügyfelek számára, ha adott eseményeket érvényesítést értesítést szeretne kapni regisztrált lett megvalósítva.

A következő adatokat a kérelem-válasz csomagok kapcsolatban a leküldéses eseményindító szerződés néhány kulcsfontosságú elemeit mutatják be.

* Kérés
  * HTTP-metódus: PUT
  * Paraméterek
    * eseményindító azonosítója: szükséges – nem átlátszó karakterlánc (például egy GUID), amely a regisztráció leküldéses eseményindító jelöli.
    * callbackUrl: szükséges – a visszahívás URL-cím lehet meghívni, ha az esemény akkor következik be. A meghívási egy egyszerű POST HTTP-hívás, amely.
    * API-specifikus paramétereket
* Válasz
  * Állapotkód **200** -kérelem regisztrálása sikeres ügyfél.
  * Állapotkód **4xx** -kérelem érvénytelen. Az ügyfél nem kell újra a kéréssel.
  * Állapotkód **5xx** -kérelmet egy belső kiszolgálóhiba és/vagy az átmeneti hibát eredményezett. Az ügyfél kell újra a kéréssel.
* A visszahívási
  * HTTP-metódus: POST
  * A kérelem törzse: értesítési tartalom.

A következő kódrészletet a következő példa bemutatja, hogyan leküldéses eseményindító végrehajtásához:

    // Implement a push trigger.
    [HttpPut]
    [Route("api/files/push/TouchedFiles/{triggerId}")]
    public HttpResponseMessage TouchedFilesPushTrigger(
        // triggerId is an opaque string.
        string triggerId,
        // A helper class provided by the AppService service SDK.
        // Here it defines the input of the push trigger is a string and the output to the callback is a FileInfoWrapper object.
        [FromBody]TriggerInput<string, FileInfoWrapper> triggerInput)
    {
        // Register the trigger to some trigger store.
        triggerStore.RegisterTrigger(triggerId, rootPath, triggerInput);

        // Extension method provided by the AppService service SDK indicating the registration is completed.
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
            // Use FileSystemWatcher to listen to file change event.
            var filter = string.IsNullOrEmpty(triggerInput.inputs) ? "*" : triggerInput.inputs;
            var watcher = new FileSystemWatcher(rootPath, filter);
            watcher.IncludeSubdirectories = true;
            watcher.EnableRaisingEvents = true;
            watcher.NotifyFilter = NotifyFilters.LastAccess;

            // When some file is changed, fire the push trigger.
            watcher.Changed +=
                (sender, e) => watcher_Changed(sender, e,
                    Runtime.FromAppSettings(),
                    triggerInput.GetCallback());

            // Assoicate the FileSystemWatcher object with the triggerId.
            _store[triggerId] = watcher;

        }

        // Fire the assoicated push trigger when some file is changed.
        void watcher_Changed(object sender, FileSystemEventArgs e,
            // AppService runtime object needed to invoke the callback.
            Runtime runtime,
            // The callback to invoke.
            ClientTriggerCallback<FileInfoWrapper> callback)
        {
            // Helper method provided by AppService service SDK to invoke a push trigger callback.
            callback.InvokeAsync(runtime, FileInfoWrapper.FromFileInfo(new FileInfo(e.FullPath)));
        }
    }

A lekérdezési eseményindító teszteléséhez kövesse az alábbi lépéseket:

1. Egy hitelesítés használatára az API-alkalmazás telepítése **névtelen nyilvános**.
2. Keresse meg a [http://requestb.in/](http://requestb.in/) egy RequestBin, amelyek erre a célra a visszahívási URL-cím létrehozásához.
3. A leküldéses eseményindító, GUID-nak hívja **eseményindító azonosítója** és a RequestBin az URL-címet **callbackUrl**.
   ![Hívást leküldéses eseményindító Postman keresztül](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)
4. Hívja a **touch** fájl touch művelet. Az alábbi ábrán egy minta kérelem Postman keresztül.
   ![Postman keresztül Touch művelet hívása](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
5. Annak ellenőrzéséhez, hogy a leküldéses eseményindító visszahívás hívása tulajdonság kimeneti RequestBin ellenőrzése.
   ![Hívás lekérdezési eseményindító Postman keresztül](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)

### <a name="describe-triggers-in-api-definition"></a>Az API-definíció eseményindítók leírása
Utáni végrehajtási az eseményindítókat és az API-alkalmazás telepítése az Azure-ba, navigáljon a **API-definíció** panel az Azure betekintő portálon, és jelenik meg, hogy az eseményindítók a rendszer automatikusan felismeri a felhasználói felületen, amelyek célja a Swagger 2.0 API-definíció az API-alkalmazás.

![API Definition panel](./media/app-service-api-dotnet-triggers/apidefinitionblade.PNG)

Ha a **letöltése Swagger** gombra, és nyissa meg a JSON-fájlt, az alábbihoz hasonló eredményeket talál:

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

A bővített tulajdonság **x-ms-schedular-eseményindító** hogyan eseményindítók részelemcímkék ismertetését API-definíció, és automatikusan kerül az API-alkalmazás átjáró által az API-definíció az átjárón keresztül kérése, ha a kérelem a következő feltételek egyike. (Azt is megteheti ezt a tulajdonságot manuálisan.)

* A lekérdezési eseményindító
  * Ha a HTTP-metódus **beolvasása**.
  * Ha a **OperationID azonosítójú** tulajdonság tartalmazza a karakterláncot **eseményindító**.
  * Ha a **paraméterek** tulajdonságánál paraméterrel egy **neve** tulajdonsága **triggerState**.
* Leküldéses eseményindítók
  * Ha a HTTP-metódus **PUT**.
  * Ha a **OperationID azonosítójú** tulajdonság tartalmazza a karakterláncot **eseményindító**.
  * Ha a **paraméterek** tulajdonságánál paraméterrel egy **neve** tulajdonsága **eseményindító azonosítója**.

## <a name="use-api-app-triggers-in-logic-apps"></a>A Logic apps API app eseményindítók használata
### <a name="list-and-configure-api-app-triggers-in-the-logic-apps-designer"></a>Listázhatja és konfigurálhatja az API app eseményindítók a Logic apps-tervezőben
Logikai alkalmazás ugyanabban az erőforráscsoportban, az API-alkalmazást hoz létre, ha lesz, egyszerűen kattintással veheti fel a tervezői vásznon. A következő lemezképek ezt mutatják be:

![A Logic App Designer eseményindítók](./media/app-service-api-dotnet-triggers/triggersinlogicappdesigner.PNG)

![A lekérdezési eseményindító Logic App Designer konfigurálása](./media/app-service-api-dotnet-triggers/configurepolltriggerinlogicappdesigner.PNG)

![A Logic App Designer leküldéses eseményindító konfigurálása](./media/app-service-api-dotnet-triggers/configurepushtriggerinlogicappdesigner.PNG)

## <a name="optimize-api-app-triggers-for-logic-apps"></a>A Logic apps az API app eseményindítók optimalizálása
Miután hozzáadta a eseményindítók API-alkalmazásba, dolgot néhány látna szívesen, ha az API-alkalmazás a logikai alkalmazás is van.

Például a **triggerState** lekérdezéses eseményindítók paramétert kell beállítani a logikai alkalmazást a következő kifejezésre. Ebben a kifejezésben kell értékelje ki a logikai alkalmazást az eseményindító utolsó meghívását, és ezt az értéket adja vissza.  

    @coalesce(triggers()?.outputs?.body?['triggerState'], '')

Megjegyzés: A kifejezésben használt függvény annak magyarázatát, tekintse meg a dokumentációt a [Logic App Munkafolyamatdefiníciós nyelve](https://msdn.microsoft.com/library/azure/dn948512.aspx).

Logic app felhasználók van viszont a kifejezést fent a **triggerState** paraméter használatakor az eseményindító. Ez a Logic app Designer keresztül a bővített tulajdonság beállított értéke lehet **x-ms-ütemező – ajánlás**.  A **x-ms-láthatósági** bővített tulajdonság értéke állítható be *belső* , hogy a paraméter maga nem jelenik meg a tervező.  A következő kódrészletet, amely mutatja be.

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

Leküldéses eseményindítók a **eseményindító azonosítója** paraméter egyedi módon kell azonosítania a logikai alkalmazást. Ajánlott eljárás is, hogy ez a tulajdonság a munkafolyamat nevét a következő kifejezés használatával:

    @workflow().name

Használja a **x-ms-ütemező – javaslat** és **x-ms-láthatósági** bővítmény tulajdonságai a saját API definiton, az API-alkalmazás is továbbítja a Logic app designer automatikusan beállítja az ebben a kifejezésben, a felhasználó számára.

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
További metaadat-információkat – például a bővítmény tulajdonságai **x-ms-ütemező – ajánlás** és **x-ms-láthatósági** -lehet hozzáadni a két módszer egyikével API attribútumdefiníciós: statikus vagy dinamikus.

A statikus metaadatok közvetlenül szerkesztheti a */metadata/apiDefinition.swagger.json* a projekt fájlt, és adja hozzá manuálisan a tulajdonságokat.

API-alkalmazások dinamikus metaadatok segítségével szerkesztheti a SwaggerConfig.cs fájlt egy művelet szűrő, amely adhat hozzá a következő kiterjesztések hozzáadása.

    GlobalConfiguration.Configuration
        .EnableSwagger(c =>
            {
                ...
                c.OperationFilter<TriggerStateFilter>();
                ...
            }


Hogyan Ez az osztály lehetővé teszi a dinamikus metaadatok forgatókönyv implementálhatók példát a következő:

    // Add extension properties on the triggerState parameter
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
                    // x-ms-visibility: set to 'internal' to signify this is an internal field
                    // x-ms-scheduler-recommendation: set to a value that logic app can use
                    triggerStateParam.vendorExtensions.Add("x-ms-visibility", "internal");
                    triggerStateParam.vendorExtensions.Add("x-ms-scheduler-recommendation",
                                                           "@coalesce(triggers()?.outputs?.body?['triggerState'], '')");
                }
            }
        }
    }
