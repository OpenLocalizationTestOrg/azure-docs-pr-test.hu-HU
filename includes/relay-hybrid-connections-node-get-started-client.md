### <a name="create-a-nodejs-application"></a><span data-ttu-id="b139d-101">Node.js alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="b139d-101">Create a Node.js application</span></span>

<span data-ttu-id="b139d-102">Hozzon létre egy `sender.js` nevű JavaScript-fájlt.</span><span class="sxs-lookup"><span data-stu-id="b139d-102">Create a new JavaScript file called `sender.js`.</span></span>

### <a name="add-the-relay-npm-package"></a><span data-ttu-id="b139d-103">A Relay NPM-csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b139d-103">Add the Relay NPM package</span></span>

<span data-ttu-id="b139d-104">Futtassa az `npm install hyco-ws` parancsot a projektmappában lévő Csomópont parancssorból.</span><span class="sxs-lookup"><span data-stu-id="b139d-104">Run `npm install hyco-ws` from a Node command prompt in your project folder.</span></span>

### <a name="write-some-code-to-send-messages"></a><span data-ttu-id="b139d-105">Írjon egy kódrészletet üzenetek küldéséhez</span><span class="sxs-lookup"><span data-stu-id="b139d-105">Write some code to send messages</span></span>

1. <span data-ttu-id="b139d-106">Adja hozzá a következő `constants` utasítást a `sender.js` fájl elejéhez.</span><span class="sxs-lookup"><span data-stu-id="b139d-106">Add the following `constants` to the top of the `sender.js` file.</span></span>
   
    ```js
    const WebSocket = require('hyco-ws');
    const readline = require('readline')
        .createInterface({
            input: process.stdin,
            output: process.stdout
        });;
    ```
2. <span data-ttu-id="b139d-107">Adja hozzá a következő állandókat a `sender.js` fájlhoz a hibrid kapcsolat részleteivel.</span><span class="sxs-lookup"><span data-stu-id="b139d-107">Add the following constants to the `sender.js` file for the hybrid connection details.</span></span> <span data-ttu-id="b139d-108">Cserélje le a zárójelben lévő helyőrzőket a hibrid gyűjtemény létrehozásakor beszerzett megfelelő értékekre.</span><span class="sxs-lookup"><span data-stu-id="b139d-108">Replace the placeholders in brackets with the values you obtained when you created the hybrid connection.</span></span>
   
   1. <span data-ttu-id="b139d-109">`const ns` – A Relay-névtér.</span><span class="sxs-lookup"><span data-stu-id="b139d-109">`const ns` - The Relay namespace.</span></span> <span data-ttu-id="b139d-110">Ügyeljen arra, hogy a teljes névtérnevet használja, például: `{namespace}.servicebus.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="b139d-110">Be sure to use the fully qualified namespace name; for example, `{namespace}.servicebus.windows.net`.</span></span>
   2. <span data-ttu-id="b139d-111">`const path` – A hibrid kapcsolat neve.</span><span class="sxs-lookup"><span data-stu-id="b139d-111">`const path` - The name of the hybrid connection.</span></span>
   3. <span data-ttu-id="b139d-112">`const keyrule` – Az SAS-kulcs neve.</span><span class="sxs-lookup"><span data-stu-id="b139d-112">`const keyrule` - The name of the SAS key.</span></span>
   4. <span data-ttu-id="b139d-113">`const key` – Az SAS-kulcs értéke.</span><span class="sxs-lookup"><span data-stu-id="b139d-113">`const key` - The SAS key value.</span></span>

3. <span data-ttu-id="b139d-114">Adja a következő kódot a `sender.js` fájlhoz:</span><span class="sxs-lookup"><span data-stu-id="b139d-114">Add the following code to the `sender.js` file:</span></span>
   
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
    <span data-ttu-id="b139d-115">A sender.js fájlnak így kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="b139d-115">Here is what your sender.js file should look like:</span></span>
   
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

