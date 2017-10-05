---
title: "Fejlesztés és helyileg történő futtatása az Azure functions |} Microsoft Docs"
description: "Megtudhatja, hogyan kód és a helyi számítógépen az Azure functions tesztelése az Azure Functions futtatása előtt."
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
ms.assetid: 242736be-ec66-4114-924b-31795fd18884
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 07/12/2017
ms.author: glenga
ms.openlocfilehash: bbe03973dbd7c70463caa6d1a45efae2ec722c05
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="code-and-test-azure-functions-locally"></a>Kód és helyileg az Azure functions tesztelése

Amíg a [Azure-portálon] teljes készlete eszközök fejlesztési és tesztelési Azure Functions számos fejlesztők inkább egy helyi fejlesztési felület biztosít. Az Azure Functions megkönnyíti, hogy a kedvenc kód szerkesztése és a helyi fejlesztői eszközök segítségével történő fejlesztéséhez és teszteléséhez a funkciók a helyi számítógépen. A funkciók is elindíthatja az eseményeket az Azure-ban, és a C# és JavaScript-funkcióként is debug a helyi számítógépen. 

A Visual Studio C# fejlesztő, az Azure Functions is [integrálható a Visual Studio 2017](functions-develop-vs.md).

## <a name="install-the-azure-functions-core-tools"></a>Az Azure Functions Core eszközök telepítése

Az Azure Functions Core eszközök, amelyek a helyi Windows-számítógépen futtathatja az Azure Functions futtatókörnyezettel helyi verziója telepítve. Nincs emulátor vagy szimulátor. Az azonos futásidejű powers működik az Azure-ban.

A [Azure Functions Core eszközök] is letöltheti az npm-csomagot. Először [NodeJS telepítése](https://docs.npmjs.com/getting-started/installing-node), mely tartalmazza az npm.  

>[!NOTE]
>Ilyenkor a Windows rendszerű számítógépeken csak az Azure Functions Core eszközcsomag is telepítheti. Ez a korlátozás az az oka egy átmeneti korlátozás a funkciók fogadó.

[Azure Functions Core eszközök] ad hozzá a következő parancsot:
* **FUNC**
* **azfun**
* **azurefunctions**

Ahelyett, hogy az összes alábbi alias is használható `func` a példákban ebben a témakörben.

## <a name="create-a-local-functions-project"></a>Helyi funkciók-projekt létrehozása

Helyben fut, a funkciók projekt esetén a fájlok host.json és local.settings.json megegyező nevű könyvtárat. Ez a könyvtár megegyezik a függvény alkalmazások az Azure-ban. Az Azure Functions mappaszerkezet kapcsolatos további tudnivalókért tekintse meg a [Azure Functions fejlesztői útmutatója](functions-reference.md#folder-structure).

A parancssorban futtassa a következő parancsot:

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

Kikapcsolja a Git-tárház létrehozása, használja a kapcsolót `--no-source-control [-n]`.

<a name="local-settings"></a>

## <a name="local-settings-file"></a>Helyi fájl

A fájl local.settings.json Alkalmazásbeállítások, a kapcsolati karakterláncok és az Azure Functions Core eszközök beállításai tárolja. Az alábbi szerkezettel rendelkezik:

```json
{
  "IsEncrypted": false,   
  "Values": {
    "AzureWebJobsStorage": "<connection string>", 
    "AzureWebJobsDashboard": "<connection string>", 
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
| **Értékek** | A helyi futtatás során használt Alkalmazásbeállítások gyűjteménye. Ez az objektum hozzáadása az alkalmazás beállításait.  |
| **AzureWebJobsStorage** | A kapcsolati karakterlánc beállítása az Azure Storage-fiók, amely az Azure Functions futtatókörnyezettel általi belső használatra szolgál. A tárfiók a függvény eseményindítók támogatja. A tárolási fiók kapcsolat beállítását indított HTTP funkciók kivételével minden funkciók szükség.  |
| **AzureWebJobsDashboard** | A kapcsolati karakterlánc beállítása az Azure Storage-fiók, amely a függvény naplók tárolására szolgál. Ezt az értéket nem kötelező elérhetővé válnak a naplók a portálon.|
| **Állomás** | Ebben a szakaszban beállítások testreszabása a funkciók gazdafolyamat, a helyi futtatás során. | 
| **LocalHttpPort** | Beállítja azt a portot használja a helyi funkciók állomás fut (`func host start` és `func run`). A `--port` parancssori kapcsoló elsőbbséget élvez ezt az értéket. |
| **CORS** | Meghatározza az engedélyezett eredeteket [eltérő eredetű erőforrások megosztása (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing). Források, szóközök nélkül vesszővel tagolt lista formájában vannak megadva. A helyettesítő karakteres érték (**\***) támogatott, amely lehetővé teszi a kérelmek bármely a forrásból. |
| **ConnectionStrings** | Az adatbázis-kapcsolati karakterláncok a függvényeket tartalmaz. Ez az objektum kapcsolati karakterláncokat hozzáadódnak a szolgáltató típusát a környezet **System.Data.SqlClient**.  | 

A legtöbb eseményindítók és kötések rendelkezik egy **kapcsolat** tulajdonság, amely leképezhető egy környezeti változó vagy alkalmazás beállítás nevét. Minden kapcsolat tulajdonság local.settings.json fájlban meghatározott Alkalmazásbeállítás kell lennie. 

Ezek a beállítások is elolvashatja a kódban környezeti változóként. A C#, használjon [System.Environment.GetEnvironmentVariable](https://msdn.microsoft.com/library/system.environment.getenvironmentvariable(v=vs.110).aspx) vagy [ConfigurationManager.AppSettings](https://msdn.microsoft.com/library/system.configuration.configurationmanager.appsettings%28v=vs.110%29.aspx). A JavaScript, használjon `process.env`. A rendszer környezeti változó megadott érvényesülnek a local.settings.json fájl értékeit. 

A local.settings.json fájl csak által használt funkciók eszközök a helyi futtatás során. Alapértelmezés szerint ezek a beállítások nem települnek át automatikusan a projektet az Azure-ba való közzétételekor. Használja a `--publish-local-settings` kapcsoló [közzétételekor](#publish) való győződjön meg arról, hogy ezek a beállítások hozzáadódnak a függvény alkalmazást az Azure-ban.

Ha nincs érvényes tárolási kapcsolati karakterlánc beállítása a **AzureWebJobsStorage**, a következő hibaüzenet jelenik meg:  

>Hiányzó érték a AzureWebJobsStorage local.settings.json. Ez azért szükséges, az összes eseményindítók HTTP eltérő. Futtatása "func azure functionary fetch--Alkalmazásbeállítások <functionAppName>", vagy adjon meg egy kapcsolati karakterláncot a local.settings.json.
  
[!INCLUDE [Note to not use local storage](../../includes/functions-local-settings-note.md)]

### <a name="configure-app-settings"></a>Alkalmazásbeállítások konfigurálása

Kapcsolati karakterláncok érték beállításához tegye a következők egyikét:
* Adja meg a kapcsolati karakterláncnak a következőről [Azure Tártallózó](http://storageexplorer.com/).
* Használja a következő parancsok egyikét:

    ```
    func azure functionapp fetch-app-settings <FunctionAppName>
    ```
    ```
    func azure functionapp storage fetch-connection-string <StorageAccountName>
    ```
    Mindkét parancsok használatba történő első bejelentkezés az Azure-bA.

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
| **`--timeout -t`** | Az időkorlát a funkciók állomás elindítása, másodpercben. Alapértelmezett: 20 másodperc.|
| **`--useHttps`** | Https://localhost köthető: {port} helyett a http://localhost: {port}. Ez a beállítás alapértelmezés szerint létrehoz megbízható tanúsítvány a számítógépen.|
| **`--pause-on-error`** | A folyamat leállítása előtt szüneteltetése további adatokat. Akkor hasznos, ha az Azure Functions Core eszközök fókusza az integrált fejlesztési környezeti (IDE).|

A funkciók gazdagép indításakor azt az URL-cím a HTTP-eseményindítókkal aktivált függvényeket kimenete:

```
Found the following functions:
Host.Functions.MyHttpTrigger

ob host started
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

Egy függvény segítségével közvetlenül is hívhat `func run <FunctionName>` , és adjon meg a függvény a bemeneti adatok. Ez a parancs hasonlít fut, a függvény használatával a **teszt** fülre az Azure portálon. Ez a parancs a teljes funkciók gazdagépen elindítja.

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

A `publish` parancs feltölti a funkciók projekt könyvtár tartalmát. Ha törli a fájlokat helyileg, a `publish` parancs nem törli azokat az Azure-ból. Használatával törölheti a fájlokat az Azure-ban a [Kudu eszköz](functions-how-to-use-azure-function-app-settings.md#kudu) a a [Azure-portálon].

## <a name="next-steps"></a>Következő lépések

Az Azure Functions Core Tools [nyissa meg a forrás és a Githubon található](https://github.com/azure/azure-functions-cli).  
A következő fájl egy hiba vagy a szolgáltatás kérelem [nyissa meg a GitHub probléma](https://github.com/azure/azure-functions-cli/issues). 

<!-- LINKS -->

[Azure Functions Core eszközök]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure-portálon]: https://portal.azure.com 
