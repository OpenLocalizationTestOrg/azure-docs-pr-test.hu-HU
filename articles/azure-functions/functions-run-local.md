---
title: "Fejlesztés és helyileg történő futtatása az Azure functions |} Microsoft Docs"
description: "Megtudhatja, hogyan kód és a helyi számítógépen az Azure functions tesztelése az Azure Functions futtatása előtt."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
ms.assetid: 242736be-ec66-4114-924b-31795fd18884
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 10/12/2017
ms.author: glenga
ms.openlocfilehash: 3fd392a3f5b48d6b8d19af530c949d91cd461099
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/24/2018
---
# <a name="code-and-test-azure-functions-locally"></a>Kód és az Azure Functions helyi tesztelése

Amíg a [Azure-portálon] teljes készlete eszközök fejlesztési és tesztelési Azure Functions számos fejlesztők inkább egy helyi fejlesztési felület biztosít. Az Azure Functions megkönnyíti, hogy a kedvenc kód szerkesztése és a helyi fejlesztői eszközök segítségével történő fejlesztéséhez és teszteléséhez a funkciók a helyi számítógépen. A funkciók is elindíthatja az eseményeket az Azure-ban, és a C# és JavaScript-funkcióként is debug a helyi számítógépen. 

A Visual Studio C# fejlesztő, az Azure Functions is [integrálható a Visual Studio 2017](functions-develop-vs.md).

>[!IMPORTANT]  
> A függvény ugyanazt az alkalmazást a portál fejlesztési helyi fejlesztési nem kombinálhatók. Létrehozásakor, és a helyi projektből funkciók közzététele, ha nem próbálja karbantartása, vagy módosítani a projekt kódját a portálon.

## <a name="install-the-azure-functions-core-tools"></a>Az Azure Functions Core Tools telepítése

[Az Azure Functions Core eszközök] az Azure Functions futtatókörnyezettel, amely a helyi fejlesztési számítógépen futtathatja helyi verziója. Nincs emulátor vagy szimulátor. Az azonos futásidejű powers működik az Azure-ban. Azure funkciók Core eszközök, egy verzió a két verziója van a futtatókörnyezet, illetve egy verziójához 1.x 2.x. Mindkét változatához vannak megadva, az [npm csomag](https://docs.npmjs.com/getting-started/what-is-npm).

>[!NOTE]  
> Vagy a verzió telepítése előtt kell [NodeJS telepítése](https://docs.npmjs.com/getting-started/installing-node), mely tartalmazza az npm. A verzió 2.x az eszközök csak a Node.js 8.5 és újabb verziói támogatottak. 

### <a name="version-1x-runtime"></a>Verzió 1.x futásidejű

Az eszközök eredeti verzióját használja a funkciók 1.x futásidejű. Ebben a verzióban a .NET-keretrendszer használ, és csak a Windows rendszerű számítógépeken támogatott. Az alábbi parancs segítségével a verzió 1.x telepíti:

```bash
npm install -g azure-functions-core-tools
```

### <a name="version-2x-runtime"></a>Verzió 2.x futásidejű

Verzió 2.x eszközt használja az Azure Functions futtatókörnyezettel 2.x, amely a .NET Core épül. Ez a támogatott 2.x verziója támogatja a .NET Core minden platformon. A platformfüggetlen fejlesztésekhez jelenlegi verzióját használja, és ha a Functions futtatókörnyezete 2.x szükség. 

>[!IMPORTANT]   
> Azure Functions Core eszközök telepítése előtt [telepítse a .NET Core 2.0](https://www.microsoft.com/net/core).  
>
> Az Azure Functions futtatókörnyezettel 2.0 előzetes, és jelenleg nem minden Azure Functions támogatja. További információkért lásd: [Azure Functions futtatókörnyezet 2.0 ismert problémák](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Azure-Functions-runtime-2.0-known-issues) 

 Az alábbi parancs segítségével a 2.0-s verzióját telepíti:

```bash
npm install -g azure-functions-core-tools@core
```

Ubuntu használatkor telepítésekor `sudo`, az alábbiak szerint:

```bash
sudo npm install -g azure-functions-core-tools@core
```

MacOS és Linux telepítése esetén szükség lehet felvenni a `unsafe-perm` jelzőt, az alábbiak szerint:

```bash
sudo npm install -g azure-functions-core-tools@core --unsafe-perm true
```

## <a name="run-azure-functions-core-tools"></a>Az Azure Functions Core eszközeinek futtatása
 
Az Azure Functions Core eszközök a következő parancs aliasok bővült:
* **func**
* **azfun**
* **azurefunctions**

Ezek aliasok bármelyike használható ahol `func` a példákat is megjelennek.

```
func init MyFunctionProj
```

## <a name="create-a-local-functions-project"></a>Helyi funkciók-projekt létrehozása

A helyi futtatás során egy funkciók projekt-e a fájlok megegyező nevű könyvtárat [host.json](functions-host-json.md) és [local.settings.json](#local-settings-file). Ez a könyvtár megegyezik a függvény alkalmazások az Azure-ban. Az Azure Functions mappaszerkezet kapcsolatos további tudnivalókért tekintse meg a [Azure Functions fejlesztői útmutatója](functions-reference.md#folder-structure).

A következő parancsot a projekt és a helyi Git-tárház létrehozásához futtassa a terminálablakot vagy a parancssorból:

```
func init MyFunctionProj
```

A kimenet a következőképpen néz ki:

```
Writing .gitignore
Writing host.json
Writing local.settings.json
Created launch.json
Initialized empty Git repository in D:/Code/Playground/MyFunctionProj/.git/
```

A projekt nélkül egy helyi Git-tárház létrehozásához használja a `--no-source-control [-n]` lehetőséget.

## <a name="local-settings-file"></a>Helyi fájl

A fájl local.settings.json Alkalmazásbeállítások, a kapcsolati karakterláncok és az Azure Functions Core eszközök beállításai tárolja. Az alábbi szerkezettel rendelkezik:

```json
{
  "IsEncrypted": false,   
  "Values": {
    "AzureWebJobsStorage": "<connection string>", 
    "AzureWebJobsDashboard": "<connection string>" 
  },
  "Host": {
    "LocalHttpPort": 7071, 
    "CORS": "*" 
  },
  "ConnectionStrings": {
    "SQLConnectionString": "Value"
  }
}
```
| Beállítás      | Leírás                            |
| ------------ | -------------------------------------- |
| **IsEncrypted** | Ha beállítása **igaz**, minden értéket a helyi számítógép kulccsal titkosított. A használt `func settings` parancsok. Alapértelmezett érték **hamis**. |
| **Értékek** | A helyi futtatás során használt Alkalmazásbeállítások gyűjteménye. **AzureWebJobsStorage** és **AzureWebJobsDashboard** példák; teljes listáját lásd: [alkalmazás-beállítások referenciája](functions-app-settings.md).  |
| **Gazdagép** | Ebben a szakaszban beállítások testreszabása a funkciók gazdafolyamat, a helyi futtatás során. | 
| **LocalHttpPort** | Beállítja azt a portot használja a helyi funkciók állomás fut (`func host start` és `func run`). A `--port` parancssori kapcsoló elsőbbséget élvez ezt az értéket. |
| **CORS** | Meghatározza az engedélyezett eredeteket [eltérő eredetű erőforrások megosztása (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing). Források, szóközök nélkül vesszővel tagolt lista formájában vannak megadva. A helyettesítő karakteres érték (\*) támogatott, amely lehetővé teszi a kérelmek bármely a forrásból. |
| **ConnectionStrings** | Az adatbázis-kapcsolati karakterláncok a függvényeket tartalmaz. Ez az objektum kapcsolati karakterláncokat hozzáadódnak a szolgáltató típusát a környezet **System.Data.SqlClient**.  | 

A legtöbb eseményindítók és kötések rendelkezik egy **kapcsolat** tulajdonság, amely leképezhető egy környezeti változó vagy alkalmazás beállítás nevét. Minden kapcsolat tulajdonság local.settings.json fájlban meghatározott Alkalmazásbeállítás kell lennie. 

Ezek a beállítások is elolvashatja a kódban környezeti változóként. A C#, használjon [System.Environment.GetEnvironmentVariable](https://msdn.microsoft.com/library/system.environment.getenvironmentvariable(v=vs.110).aspx) vagy [ConfigurationManager.AppSettings](https://msdn.microsoft.com/library/system.configuration.configurationmanager.appsettings%28v=vs.110%29.aspx). A JavaScript, használjon `process.env`. A rendszer környezeti változó megadott érvényesülnek a local.settings.json fájl értékeit. 

A local.settings.json fájl csak által használt funkciók eszközök a helyi futtatás során. Alapértelmezés szerint ezek a beállítások nem települnek át automatikusan a projektet az Azure-ba való közzétételekor. Használja a `--publish-local-settings` kapcsoló [közzétételekor](#publish) való győződjön meg arról, hogy ezek a beállítások hozzáadódnak a függvény alkalmazást az Azure-ban.

Ha nincs érvényes tárolási kapcsolati karakterlánc beállítása a **AzureWebJobsStorage**, a következő hibaüzenet jelenik meg:  

>Hiányzó érték a AzureWebJobsStorage local.settings.json. Ez azért szükséges, az összes eseményindítók HTTP eltérő. Futtatása "func azure functionapp fetch--Alkalmazásbeállítások <functionAppName>", vagy adjon meg egy kapcsolati karakterláncot a local.settings.json.
  
[!INCLUDE [Note to not use local storage](../../includes/functions-local-settings-note.md)]

### <a name="configure-app-settings"></a>Alkalmazásbeállítások konfigurálása

Kapcsolati karakterláncok érték beállításához tegye a következő lehetőségek közül:
* Adja meg a kapcsolati karakterláncnak a következőről [Azure Tártallózó](http://storageexplorer.com/).
* Használja a következő parancsok egyikét:

    ```
    func azure functionapp fetch-app-settings <FunctionAppName>
    ```
    ```
    func azure storage fetch-connection-string <StorageAccountName>
    ```
    Mindkét parancsok használatba történő első bejelentkezés az Azure-bA.

<a name="create-func"></a>
## <a name="create-a-function"></a>Függvény létrehozása

A függvény létrehozásához futtassa a következő parancsot:

```
func new
``` 
`func new`a következő nem kötelező argumentum használatát is támogatja:

| Argumentum     | Leírás                            |
| ------------ | -------------------------------------- |
| **`--language -l`** | A sablon programozási nyelv, például a C#, F # vagy JavaScript. |
| **`--template -t`** | A sablonnevet. |
| **`--name -n`** | A függvény nevét. |

Például a JavaScript HTTP-eseményindítóval létrehozásához futtassa:

```
func new --language JavaScript --template HttpTrigger --name MyHttpTrigger
```

A várólista-eseményindítóval aktivált függvény létrehozásához futtassa:

```
func new --language JavaScript --template QueueTrigger --name QueueTriggerJS
```
<a name="start"></a>
## <a name="run-functions-locally"></a>Futtassa helyben a Funkciók

A funkciók projekt futtatni, futtassa a funkciók állomás. A gazdagép lehetővé teszi, hogy a projekt összes funkciójának eseményindítók:

```
func host start
```

`func host start`támogatja a következő beállításokat:

| Beállítás     | Leírás                            |
| ------------ | -------------------------------------- |
|**`--port -p`** | A helyi portot. Alapértelmezett érték: 7071. |
| **`--debug <type>`** | A beállítások `VSCode` és `VS`. |
| **`--cors`** | A CORS források, szóközök nélkül vesszővel tagolt listája. |
| **`--nodeDebugPort -n`** | A csomópont hibakereső használandó portot. Alapértelmezett: Launch.json vagy 5858 egy értéket. |
| **`--debugLevel -d`** | A konzol nyomkövetési szint (kikapcsolt, részletes, információ, figyelmeztetés vagy hiba). Alapértelmezett: adatait.|
| **`--timeout -t`** | Az időtúllépés másodpercben elindításához a funkciók gazdagép. Alapértelmezett: 20 másodperc.|
| **`--useHttps`** | Https://localhost köthető: {port} helyett a http://localhost: {port}. Ez a beállítás alapértelmezés szerint létrehoz megbízható tanúsítvány a számítógépen.|
| **`--pause-on-error`** | A folyamat leállítása előtt szüneteltetése további adatokat. Akkor hasznos, ha az Azure Functions Core eszközök fókusza az integrált fejlesztési környezeti (IDE).|

A funkciók gazdagép indításakor azt az URL-cím a HTTP-eseményindítókkal aktivált függvényeket kimenete:

```
Found the following functions:
Host.Functions.MyHttpTrigger

Job host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
```

### <a name="debug-in-vs-code-or-visual-studio"></a>A Visual STUDIO Code vagy a Visual Studio hibakeresési

A hibakereső csatolásához át a `--debug` argumentum. JavaScript-funkcióként hibakeresési, használja a Visual Studio Code. C# funkciók a Visual Studio használata.

Hibakeresési C# funkciók, használja a `--debug vs`. Is [Azure Functions Visual Studio 2017 eszközök](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/). 

Indítsa el a gazdagépen, és állítsa be a JavaScript-hibakeresés, futtassa:

```
func host start --debug vscode
```

Ezt követően a Visual Studio Code, az a **Debug** nézetben jelölje ki **csatlakoztatása az Azure Functions**. Töréspontokat csatolása, vizsgálja meg a változók és kód lépéseit.

![JavaScript-hibakeresés a Visual Studio Code](./media/functions-run-local/vscode-javascript-debugging.png)

### <a name="passing-test-data-to-a-function"></a>Egy függvény sikeres Tesztadatok

Helyileg, a funkciók tesztelésére, [a funkciók gazdagép indítása](#start) és végpontok hívja meg a helyi kiszolgálón a HTTP-kérelmekre. A végpont meghívja a függvény függ. 

>[!NOTE]  
> Ebben a témakörben szereplő példák a cURL eszközzel HTTP-kérelmek küldése a Terminálszolgáltatások vagy a parancssorból. A kiválasztott eszköz segítségével HTTP-kérelmeket küldjön a helyi kiszolgáló. A cURL eszköz alapértelmezés szerint a Linux-alapú rendszereken érhető el. A Windows, először le kell töltenie és telepítse a [cURL eszköz](https://curl.haxx.se/).

A tesztelés funkciók további általános információkért lásd: [a kódot az Azure Functions tesztelése kapcsolatos olyan stratégiák](functions-test-a-function.md).

#### <a name="http-and-webhook-triggered-functions"></a>A HTTP és a webhook indított funkciók

A következő meghívja a helyi futtatásához a HTTP és a webhook végpont indított funkciók:

    http://localhost:{port}/api/{function_name}

Ügyeljen arra, hogy az azonos kiszolgáló nevére és a portot, amelyet figyeli a funkciók állomás. Is megjelenik ez a kimenet jön létre, a függvény gazdagép indításakor. Az URL-CÍMÉT az eseményindító által támogatott bármely HTTP-metódus hívása. 

A következő cURL-parancsot az eseményindítók a `MyHttpTrigger` gyors üzembe helyezés függvény érkezett egy GET kérelmet a _neve_ paramétert a lekérdezési karakterláncban. 

```
curl --get http://localhost:7071/api/MyHttpTrigger?name=Azure%20Rocks
```
A következő példa egy fájlból egy POST kérést, hogy ugyanazt a funkciót _neve_ a kérelem törzsében szereplő:

```
curl --request POST http://localhost:7071/api/MyHttpTrigger --data '{"name":"Azure Rocks"}'
```

Vegye figyelembe, hogy GET kérések adatok átadására a lekérdezési karakterláncban a böngészőből. Minden egyéb HTTP-metódus, a cURL, Fiddler, Postman vagy egy hasonló HTTP vizsgálati eszköz kell használnia.  

#### <a name="non-http-triggered-functions"></a>A HTTP nélküli indított funkciók
A különböző funkciók eltérő HTTP eseményindítók és webhookokkal tesztelheti a funkciók helyileg felügyeleti végpont meghívásával. A függvény hívása a következő ehhez a végponthoz, a HTTP POST-kérelmet a helyi kiszolgáló váltja ki. Vizsgálati adatok opcionálisan átadása a végrehajtása a POST kérelem törzsét. Ez a funkció hasonlít a **teszt** fülre az Azure portálon.  

A következő rendszergazda végpont való nem HTTP-függvények hívása:

    http://localhost:{port}/admin/functions/{function_name}

Vizsgálati adatok átadása a rendszergazda végpont egy függvény, meg kell adnia egy POST kérést üzenet törzsében lévő adatokat. Az üzenettörzs szükség van a következő JSON formátummal:

```JSON
{
    "input": "<trigger_input>"
}
```` 
A `<trigger_input>` érték, amelyet a függvény várt formátumú adatokat tartalmaz. A következő cURL-példa egy mutató POST egy `QueueTriggerJS` függvény. Ebben az esetben a bemeneti érték karakterlánc, amely megfelel az várható, hogy a várakozási sorban található üzenetek.      

```
curl --request POST -H "Content-Type:application/json" --data '{"input":"sample queue data"}' http://localhost:7071/admin/functions/QueueTriggerJS
```

#### <a name="using-the-func-run-command-in-version-1x"></a>Használja a `func run` parancs verziójában 1.x

>[!IMPORTANT]  
> A `func run` parancs nem támogatott verzióját az eszközök 2.x. További információkért lásd a témakör [bemutatásához az Azure Functions futásidejű verziók](functions-versions.md).

Egy függvény segítségével közvetlenül is hívhat `func run <FunctionName>` , és adjon meg a függvény a bemeneti adatok. Ez a parancs hasonlít fut, a függvény használatával a **teszt** fülre az Azure portálon. 

`func run`támogatja a következő beállításokat:

| Beállítás     | Leírás                            |
| ------------ | -------------------------------------- |
| **`--content -c`** | Beágyazott tartalmat. |
| **`--debug -d`** | A hibakereső csatolása a gazdafolyamat függvény futtatása előtt.|
| **`--timeout -t`** | Idő (másodpercben) várja meg, míg a helyi funkciók gazdagépen elkészült.|
| **`--file -f`** | A tartalom használandó fájl neve.|
| **`--no-interactive`** | Nem kéri a bemenetben. Automatizálási esetekben hasznos.|

Például egy HTTP-eseményindítóval aktivált függvény, és adja át a tartalomtörzs, futtassa a következő parancsot:

```
func run MyHttpTrigger -c '{\"name\": \"Azure\"}'
```

## <a name="publish"></a>Közzététel az Azure platformon

A funkciók projekt közzététele függvény alkalmazásokhoz az Azure-ban, használja a `publish` parancs:

```
func azure functionapp publish <FunctionAppName>
```

A következő beállításokat is használhatja:

| Beállítás     | Leírás                            |
| ------------ | -------------------------------------- |
| **`--publish-local-settings -i`** |  Közzétételi beállítások a local.settings.json az Azure-ba, arra kéri a írhatja felül, ha a beállítás már létezik.|
| **`--overwrite-settings -y`** | Együtt kell használni `-i`. Felülírja a helyi érték AppSettings az Azure-ban, ha különböző. Alapértelmezett érték kérése.|

Ez a parancs tesz közzé egy meglévő függvény alkalmazáshoz az Azure-ban. Hiba akkor fordul elő, amikor a `<FunctionAppName>` az előfizetéshez nem létezik. A függvény alkalmazás létrehozásának a parancssort vagy terminálablakot az Azure parancssori felület használatával, lásd: [hozzon létre egy kiszolgáló nélküli végrehajtási függvény alkalmazást](./scripts/functions-cli-create-serverless.md).

A `publish` parancs feltölti a funkciók projekt könyvtár tartalmát. Ha törli a fájlokat helyileg, a `publish` parancs nem törli azokat az Azure-ból. Használatával törölheti a fájlokat az Azure-ban a [Kudu eszköz](functions-how-to-use-azure-function-app-settings.md#kudu) a a [Azure-portálon].  

>[!IMPORTANT]  
> Ha az Azure-ban létrehoz egy függvény alkalmazást, akkor verzióját használja 1.x függvény futásidejű alapértelmezés szerint. A függvény az alkalmazás használatát verziója annak 2.x futtatókörnyezet, az Alkalmazásbeállítás hozzáadása `FUNCTIONS_EXTENSION_VERSION=beta`.  
A következő kódot az Azure parancssori felület használatával adja hozzá ezt a beállítást az függvény alkalmazáshoz: 
```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings FUNCTIONS_EXTENSION_VERSION=beta   
```

## <a name="next-steps"></a>További lépések

Az Azure Functions Core Tools [nyissa meg a forrás és a Githubon található](https://github.com/azure/azure-functions-cli).  
A következő fájl egy hiba vagy a szolgáltatás kérelem [nyissa meg a GitHub probléma](https://github.com/azure/azure-functions-cli/issues). 

<!-- LINKS -->

[Az Azure Functions Core eszközök]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure-portálon]: https://portal.azure.com 
