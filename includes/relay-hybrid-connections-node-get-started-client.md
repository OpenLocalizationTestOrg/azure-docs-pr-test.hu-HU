### <a name="create-a-nodejs-application"></a><span data-ttu-id="67a50-101">Node.js alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="67a50-101">Create a Node.js application</span></span>

<span data-ttu-id="67a50-102">Hozzon létre egy `sender.js` nevű JavaScript-fájlt.</span><span class="sxs-lookup"><span data-stu-id="67a50-102">Create a new JavaScript file called `sender.js`.</span></span>

### <a name="add-hello-relay-npm-package"></a><span data-ttu-id="67a50-103">Hello továbbítási NPM csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="67a50-103">Add hello Relay NPM package</span></span>

<span data-ttu-id="67a50-104">Futtassa az `npm install hyco-ws` parancsot a projektmappában lévő Csomópont parancssorból.</span><span class="sxs-lookup"><span data-stu-id="67a50-104">Run `npm install hyco-ws` from a Node command prompt in your project folder.</span></span>

### <a name="write-some-code-toosend-messages"></a><span data-ttu-id="67a50-105">Írnia egy kódrészletet toosend üzenetek</span><span class="sxs-lookup"><span data-stu-id="67a50-105">Write some code toosend messages</span></span>

1. <span data-ttu-id="67a50-106">Adja hozzá a következő hello `constants` toohello felső részén hello `sender.js` fájlt.</span><span class="sxs-lookup"><span data-stu-id="67a50-106">Add hello following `constants` toohello top of hello `sender.js` file.</span></span>
   
    ```js
    const WebSocket = require('hyco-ws');
    const readline = require('readline')
        .createInterface({
            input: process.stdin,
            output: process.stdout
        });;
    ```
2. <span data-ttu-id="67a50-107">Adja hozzá a következő állandókat toohello hello `sender.js` hello hibrid kapcsolat részletek fájlban találhatók.</span><span class="sxs-lookup"><span data-stu-id="67a50-107">Add hello following constants toohello `sender.js` file for hello hybrid connection details.</span></span> <span data-ttu-id="67a50-108">Szögletes zárójelbe hello helyőrzőket cserélje le hello értékek hello hibrid kapcsolat létrehozása után kapott.</span><span class="sxs-lookup"><span data-stu-id="67a50-108">Replace hello placeholders in brackets with hello values you obtained when you created hello hybrid connection.</span></span>
   
   1. <span data-ttu-id="67a50-109">`const ns`-Továbbítási névtér hello.</span><span class="sxs-lookup"><span data-stu-id="67a50-109">`const ns` - hello Relay namespace.</span></span> <span data-ttu-id="67a50-110">Lehet, hogy toouse hello teljesen minősített névtér neve; például `{namespace}.servicebus.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="67a50-110">Be sure toouse hello fully qualified namespace name; for example, `{namespace}.servicebus.windows.net`.</span></span>
   2. <span data-ttu-id="67a50-111">`const path`-hello hibrid kapcsolat hello nevét.</span><span class="sxs-lookup"><span data-stu-id="67a50-111">`const path` - hello name of hello hybrid connection.</span></span>
   3. <span data-ttu-id="67a50-112">`const keyrule`-hello hello SAS-kulcs neve.</span><span class="sxs-lookup"><span data-stu-id="67a50-112">`const keyrule` - hello name of hello SAS key.</span></span>
   4. <span data-ttu-id="67a50-113">`const key`-hello SAS-kulcs értékét.</span><span class="sxs-lookup"><span data-stu-id="67a50-113">`const key` - hello SAS key value.</span></span>

3. <span data-ttu-id="67a50-114">Adja hozzá a következő kód toohello hello `sender.js` fájlt:</span><span class="sxs-lookup"><span data-stu-id="67a50-114">Add hello following code toohello `sender.js` file:</span></span>
   
    ```js
    WebSocket.relayedConnect(
        WebSocket.createRelaySendUri(ns, path),
        WebSocket.createRelayToken('http://'+ns, keyrule, key),
        function (wss) {
            readline.on('line', (input) => {
                wss.send(input, null);
            });
   
            console.log('Started client interval.');
            wss.on('close', function () {
                console.log('stopping client interval');
                process.exit();
            });
        }
    );
    ```
    <span data-ttu-id="67a50-115">A sender.js fájlnak így kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="67a50-115">Here is what your sender.js file should look like:</span></span>
   
    ```js
    const WebSocket = require('hyco-ws');
    const readline = require('readline')
        .createInterface({
            input: process.stdin,
            output: process.stdout
        });;
   
    const ns = "{RelayNamespace}";
    const path = "{HybridConnectionName}";
    const keyrule = "{SASKeyName}";
    const key = "{SASKeyValue}";
   
    WebSocket.relayedConnect(
        WebSocket.createRelaySendUri(ns, path),
        WebSocket.createRelayToken('http://'+ns, keyrule, key),
        function (wss) {
            readline.on('line', (input) => {
                wss.send(input, null);
            });
   
            console.log('Started client interval.');
            wss.on('close', function () {
                console.log('stopping client interval');
                process.exit();
            });
        }
    );
    ```

