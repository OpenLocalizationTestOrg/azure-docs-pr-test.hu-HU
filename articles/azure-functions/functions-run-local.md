---
title: "aaaDevelop és futtatási Azure functions helyileg |} Microsoft Docs"
description: "Ismerje meg, hogyan toocode és tesztelési Azure működik a helyi számítógépen az Azure Functions futtatása előtt."
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
ms.openlocfilehash: 342ed4d6df41a2d2df9067948e19e347bb52c844
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="code-and-test-azure-functions-locally"></a>Kód és helyileg az Azure functions tesztelése

Hello közben [Azure-portálon] teljes készlete eszközök fejlesztési és tesztelési Azure Functions számos fejlesztők inkább egy helyi fejlesztési felület biztosít. Az Azure Functions segítségével könnyen toouse meg kedvenc kód szerkesztő és a helyi fejlesztési eszközök toodevelop, és tesztelje a funkciók a helyi számítógépen. A funkciók is elindíthatja az eseményeket az Azure-ban, és a C# és JavaScript-funkcióként is debug a helyi számítógépen. 

A Visual Studio C# fejlesztő, az Azure Functions is [integrálható a Visual Studio 2017](functions-develop-vs.md).

## <a name="install-hello-azure-functions-core-tools"></a>Hello Azure Functions Core eszközök telepítése

Az Azure Functions Core eszközök futtathatja a helyi számítógépen a Windows hello Azure Functions futtatókörnyezettel helyi verziója telepítve. Nincs emulátor vagy szimulátor. Azonos futásidejű rendelkezik azt, hogy az Azure Functions powers hello

Hello [Azure Functions Core eszközök] is letöltheti az npm-csomagot. Először [NodeJS telepítése](https://docs.npmjs.com/getting-started/installing-node), mely tartalmazza az npm.  

>[!NOTE]
>Ilyenkor a Windows rendszerű számítógépeken csak hello Azure Functions Core eszközcsomag is telepítheti. Ez a korlátozás tooa átmeneti korlátozás hello funkciók fogadó miatt van.

[Azure Functions Core eszközök] ad hozzá a következő parancs aliasok hello:
* **FUNC**
* **azfun**
* **azurefunctions**

Ahelyett, hogy az összes alábbi alias is használható `func` hello példákban ebben a témakörben.

## <a name="create-a-local-functions-project"></a>Helyi funkciók-projekt létrehozása

A helyi futtatás során egy funkciók projekt hello fájlok host.json és local.settings.json megegyező nevű könyvtárat. Ebben a könyvtárban van hello egyenértékű, a függvény alkalmazások az Azure-ban. toolearn hello Azure Functions gyökérmappa-szerkezetében kapcsolatos további információkért lásd: hello [Azure Functions fejlesztői útmutatója](functions-reference.md#folder-structure).

Parancsot egy parancssorba futtassa a következő parancs hello:

```
func init MyFunctionProj
```

hello kimenete a következő példa hello néz ki:

```
Writing .gitignore
Writing host.json
Writing local.settings.json
Created launch.json
Initialized empty Git repository in D:/Code/Playground/MyFunctionProj/.git/
```

tooopt kívül a Git-tárház, az hello kapcsolóval létrehozása `--no-source-control [-n]`.

<a name="local-settings"></a>

## <a name="local-settings-file"></a>Helyi fájl

hello fájl local.settings.json Alkalmazásbeállítások, a kapcsolati karakterláncok és az Azure Functions Core eszközök beállításai tárolja. A következő struktúra hello rendelkezik:

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
| **IsEncrypted** | Ha értéke túl**igaz**, minden értéket a helyi számítógép kulccsal titkosított. A használt `func settings` parancsok. Alapértelmezett érték **hamis**. |
| **Értékek** | A helyi futtatás során használt Alkalmazásbeállítások gyűjteménye. Adja hozzá az alkalmazás beállításainak toothis objektumot.  |
| **AzureWebJobsStorage** | Készletek hello kapcsolati karakterlánc toohello belsőleg hello Azure Functions futtatókörnyezettel Azure Storage-fiók. hello tárfiók a függvény eseményindítók támogatja. A tárolási fiók kapcsolat beállítását indított HTTP funkciók kivételével minden funkciók szükség.  |
| **AzureWebJobsDashboard** | Beállítja a hello kapcsolati karakterlánc toohello Azure Storage-fiók, amely használt toostore hello függvény naplókat. Ezt az értéket nem kötelező hello portálon elérhetővé hello naplókat.|
| **Állomás** | Ebben a szakaszban beállítások testreszabása hello funkciók gazdafolyamat, a helyi futtatás során. | 
| **LocalHttpPort** | Készletek hello hello funkciók localhost futtatásakor használt alapértelmezett port (`func host start` és `func run`). Hello `--port` parancssori kapcsoló elsőbbséget élvez ezt az értéket. |
| **CORS** | Meghatározza az engedélyezett hello források [eltérő eredetű erőforrások megosztása (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing). Források, szóközök nélkül vesszővel tagolt lista formájában vannak megadva. helyettesítő karakteres érték hello (**\***) támogatott, amely lehetővé teszi a kérelmek bármely a forrásból. |
| **ConnectionStrings** | A függvények hello adatbázis-kapcsolati karakterláncok tartalmazza. Ez az objektum kapcsolati karakterláncokat kerülnek toohello környezet hello szolgáltató típusú **System.Data.SqlClient**.  | 

A legtöbb eseményindítók és kötések rendelkezik egy **kapcsolat** tulajdonság, amely leképezhető a környezeti változó vagy alkalmazás beállításokat toohello nevét. Minden kapcsolat tulajdonság local.settings.json fájlban meghatározott Alkalmazásbeállítás kell lennie. 

Ezek a beállítások is elolvashatja a kódban környezeti változóként. A C#, használjon [System.Environment.GetEnvironmentVariable](https://msdn.microsoft.com/library/system.environment.getenvironmentvariable(v=vs.110).aspx) vagy [ConfigurationManager.AppSettings](https://msdn.microsoft.com/library/system.configuration.configurationmanager.appsettings%28v=vs.110%29.aspx). A JavaScript, használjon `process.env`. A rendszer környezeti változó megadott érvényesülnek hello local.settings.json fájl értékeit. 

Hello local.settings.json fájl beállításai csak által használt funkciók eszközök a helyi futtatás során. Alapértelmezés szerint ezek a beállítások nem települnek át automatikusan közzétett tooAzure hello projekt esetén. Használjon hello `--publish-local-settings` kapcsoló [közzétételekor](#publish) toomake meg arról, hogy ezek a beállítások vannak toohello függvény alkalmazást az Azure-ban.

Ha nincs érvényes tárolási kapcsolati karakterlánc beállítása a **AzureWebJobsStorage**, hello a következő hibaüzenet jelenik meg:  

>Hiányzó érték a AzureWebJobsStorage local.settings.json. Ez azért szükséges, az összes eseményindítók HTTP eltérő. Futtatása "func azure functionary fetch--Alkalmazásbeállítások <functionAppName>", vagy adjon meg egy kapcsolati karakterláncot a local.settings.json.
  
[!INCLUDE [Note toonot use local storage](../../includes/functions-local-settings-note.md)]

### <a name="configure-app-settings"></a>Alkalmazásbeállítások konfigurálása

kapcsolati karakterláncok értékét tooset, tegye hello következők egyikét:
* Adjon meg hello kapcsolati karakterláncot a [Azure Tártallózó](http://storageexplorer.com/).
* Hello a következő parancsok egyikét használja:

    ```
    func azure functionapp fetch-app-settings <FunctionAppName>
    ```
    ```
    func azure functionapp storage fetch-connection-string <StorageAccountName>
    ```
    Mindkét parancsok, toofirst bejelentkezési tooAzure igényelnek.

## <a name="create-a-function"></a>Függvény létrehozása

egy függvény toocreate hello a következő parancsot futtassa:

```
func new
``` 
`func new`nem kötelező argumentum a következő hello támogatja:

| Argumentum     | Leírás                            |
| ------------ | -------------------------------------- |
| **`--language -l`** | programozási nyelv, például a C#, F # vagy JavaScript hello sablont. |
| **`--template -t`** | hello a sablonnevet. |
| **`--name -n`** | hello függvény neve. |

Például toocreate egy JavaScript HTTP-eseményindítóval futtatása:

```
func new --language JavaScript --template HttpTrigger --name MyHttpTrigger
```

a várólista-eseményindítóval aktivált függvény toocreate futtatása:

```
func new --language JavaScript --template QueueTrigger --name QueueTriggerJS
```

## <a name="run-functions-locally"></a>Futtassa helyben a Funkciók

a funkciók projekt toorun hello funkciók állomás futtassa. hello állomás lehetővé teszi, hogy az eseményindítók hello projekt összes funkciójának:

```
func host start
```

`func host start`támogatja az alábbi beállítások hello:

| Beállítás     | Leírás                            |
| ------------ | -------------------------------------- |
|**`--port -p`** | hello helyi port toolisten meg. Alapértelmezett érték: 7071. |
| **`--debug <type>`** | hello beállítások `VSCode` és `VS`. |
| **`--cors`** | A CORS források, szóközök nélkül vesszővel tagolt listája. |
| **`--nodeDebugPort -n`** | hello csomópont hibakereső toouse hello port. Alapértelmezett: Launch.json vagy 5858 egy értéket. |
| **`--debugLevel -d`** | hello konzol nyomkövetési szint (kikapcsolt, részletes, információ, figyelmeztetés vagy hiba). Alapértelmezett: adatait.|
| **`--timeout -t`** | hello időtúllépés hello funkciók állomás elindítása, másodpercben. Alapértelmezett: 20 másodperc.|
| **`--useHttps`** | Kötési toohttps://localhost: {port} helyett toohttp://localhost: {port}. Ez a beállítás alapértelmezés szerint létrehoz megbízható tanúsítvány a számítógépen.|
| **`--pause-on-error`** | Felfüggesztés előtt hello folyamat kilép további adatokat. Akkor hasznos, ha az Azure Functions Core eszközök fókusza az integrált fejlesztési környezeti (IDE).|

Hello funkciók gazdagép indításakor azt hello URL-cím a HTTP-eseményindítókkal aktivált függvényeket kimenete:

```
Found hello following functions:
Host.Functions.MyHttpTrigger

ob host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
```

### <a name="debug-in-vs-code-or-visual-studio"></a>A Visual STUDIO Code vagy a Visual Studio hibakeresési

tooattach hibakereső, átadni hello `--debug` argumentum. JavaScript-funkcióként toodebug, a Visual Studio Code használja. C# funkciók a Visual Studio használata.

toodebug C# funkciók használata `--debug vs`. Is [Azure Functions Visual Studio 2017 eszközök](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/). 

toolaunch hello állomás, és állítsa be a JavaScript-hibakeresés, futtassa:

```
func host start --debug vscode
```

Ezt követően a Visual Studio Code, a hello **Debug** nézetben jelölje ki **tooAzure funkciók csatolása**. Töréspontokat csatolása, vizsgálja meg a változók és kód lépéseit.

![JavaScript-hibakeresés a Visual Studio Code](./media/functions-run-local/vscode-javascript-debugging.png)

### <a name="passing-test-data-tooa-function"></a>Sikeres vizsgálati adatok tooa függvény

Egy függvény segítségével közvetlenül is hívhat `func run <FunctionName>` , és adja meg a bemeneti adatok hello függvény. Ez a parancs hasonló toorunning feladata hello segítségével **teszt** hello Azure-portálon lapján. Ez a parancs hello teljes funkciók gazdagépen elindítja.

`func run`támogatja az alábbi beállítások hello:

| Beállítás     | Leírás                            |
| ------------ | -------------------------------------- |
| **`--content -c`** | Beágyazott tartalmat. |
| **`--debug -d`** | A hibakereső toohello gazdafolyamatokon csatolása hello függvény futtatása előtt.|
| **`--timeout -t`** | Készen áll az idő toowait (másodpercben), amíg hello helyi funkciók állomás.|
| **`--file -f`** | hello fájl neve toouse tartalmat.|
| **`--no-interactive`** | Nem kéri a bemenetben. Automatizálási esetekben hasznos.|

Például egy HTTP-eseményindítóval aktivált függvény toocall és pass tartalomtörzs, futtassa a következő parancs hello:

```
func run MyHttpTrigger -c '{\"name\": \"Azure\"}'
```

## <a name="publish"></a>TooAzure közzététele

a funkciók projekt tooa függvény alkalmazások az Azure használatát hello toopublish `publish` parancs:

```
func azure functionapp publish <FunctionAppName>
```

Használhatja az alábbi beállítások hello:

| Beállítás     | Leírás                            |
| ------------ | -------------------------------------- |
| **`--publish-local-settings -i`** |  Közzétételi beállítások a local.settings.json tooAzure toooverwrite értesítése, ha hello beállítása már létezik.|
| **`--overwrite-settings -y`** | Együtt kell használni `-i`. Felülírja a helyi érték AppSettings az Azure-ban, ha különböző. Alapértelmezett érték kérése.|

Hello `publish` parancs feltölt hello funkciók projektkönyvtár hello tartalmát. Ha törli a fájlokat helyileg, hello `publish` parancs nem törli azokat az Azure-ból. Hello segítségével törölheti a fájlokat az Azure-ban [Kudu eszköz](functions-how-to-use-azure-function-app-settings.md#kudu) a hello [Azure-portálon].

## <a name="next-steps"></a>Következő lépések

Az Azure Functions Core Tools [nyissa meg a forrás és a Githubon található](https://github.com/azure/azure-functions-cli).  
egy hiba vagy a szolgáltatás kérelem toofile [nyissa meg a GitHub probléma](https://github.com/azure/azure-functions-cli/issues). 

<!-- LINKS -->

[Azure Functions Core eszközök]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure-portálon]: https://portal.azure.com 
