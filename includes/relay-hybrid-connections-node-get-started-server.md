### <a name="create-a-nodejs-application"></a><span data-ttu-id="d7e4e-101">Node.js alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="d7e4e-101">Create a Node.js application</span></span>

<span data-ttu-id="d7e4e-102">Hozzon létre egy `listener.js` nevű JavaScript-fájlt.</span><span class="sxs-lookup"><span data-stu-id="d7e4e-102">Create a new JavaScript file called `listener.js`.</span></span>

### <a name="add-hello-relay-npm-package"></a><span data-ttu-id="d7e4e-103">Hello továbbítási NPM csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d7e4e-103">Add hello Relay NPM package</span></span>

<span data-ttu-id="d7e4e-104">Futtassa az `npm install hyco-ws` parancsot a projektmappában lévő Csomópont parancssorból.</span><span class="sxs-lookup"><span data-stu-id="d7e4e-104">Run `npm install hyco-ws` from a Node command prompt in your project folder.</span></span>

### <a name="write-some-code-tooreceive-messages"></a><span data-ttu-id="d7e4e-105">Írnia egy kódrészletet tooreceive üzenetek</span><span class="sxs-lookup"><span data-stu-id="d7e4e-105">Write some code tooreceive messages</span></span>

1. <span data-ttu-id="d7e4e-106">Adja hozzá a következő konstans toohello felső részén hello hello `listener.js` fájlt.</span><span class="sxs-lookup"><span data-stu-id="d7e4e-106">Add hello following constant toohello top of hello `listener.js` file.</span></span>
   
    ```js
    const WebSocket = require('hyco-ws');
    ```
2. <span data-ttu-id="d7e4e-107">Adja hozzá a következő állandókat toohello hello `listener.js` hello hibrid kapcsolat részletek fájlban találhatók.</span><span class="sxs-lookup"><span data-stu-id="d7e4e-107">Add hello following constants toohello `listener.js` file for hello hybrid connection details.</span></span> <span data-ttu-id="d7e4e-108">Szögletes zárójelbe hello helyőrzőket cserélje le hello értékek hello hibrid kapcsolat létrehozása után kapott.</span><span class="sxs-lookup"><span data-stu-id="d7e4e-108">Replace hello placeholders in brackets with hello values you obtained when you created hello hybrid connection.</span></span>
   
   1. <span data-ttu-id="d7e4e-109">`const ns`-Továbbítási névtér hello.</span><span class="sxs-lookup"><span data-stu-id="d7e4e-109">`const ns` - hello Relay namespace.</span></span> <span data-ttu-id="d7e4e-110">Lehet, hogy toouse hello teljesen minősített névtér neve; például `{namespace}.servicebus.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="d7e4e-110">Be sure toouse hello fully qualified namespace name; for example, `{namespace}.servicebus.windows.net`.</span></span>
   2. <span data-ttu-id="d7e4e-111">`const path`-hello hibrid kapcsolat hello nevét.</span><span class="sxs-lookup"><span data-stu-id="d7e4e-111">`const path` - hello name of hello hybrid connection.</span></span>
   3. <span data-ttu-id="d7e4e-112">`const keyrule`-hello hello SAS-kulcs neve.</span><span class="sxs-lookup"><span data-stu-id="d7e4e-112">`const keyrule` - hello name of hello SAS key.</span></span>
   4. <span data-ttu-id="d7e4e-113">`const key`-hello SAS-kulcs értékét.</span><span class="sxs-lookup"><span data-stu-id="d7e4e-113">`const key` - hello SAS key value.</span></span>

3. <span data-ttu-id="d7e4e-114">Adja hozzá a következő kód toohello hello `listener.js` fájlt:</span><span class="sxs-lookup"><span data-stu-id="d7e4e-114">Add hello following code toohello `listener.js` file:</span></span>
   
    ```js
    var wss = WebSocket.createRelayedServer(
    {
        server : WebSocket.createRelayListenUri(ns, path),
        token: WebSocket.createRelayToken('http://' + ns, keyrule, key)
    }, 
    function (ws) {
        console.log('connection accepted');
        ws.onmessage = function (event) {
            console.log(event.data);
        };
        ws.on('close', function () {
            console.log('connection closed');
        });       
    });
   
    console.log('listening');
   
    wss.on('error', function(err) {
        console.log('error' + err);
    });
    ```
    <span data-ttu-id="d7e4e-115">A listener.js fájlnak így kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="d7e4e-115">Here is what your listener.js file should look like:</span></span>
   
    ```js
    const WebSocket = require('hyco-ws');
   
    const ns = "{RelayNamespace}";
    const path = "{HybridConnectionName}";
    const keyrule = "{SASKeyName}";
    const key = "{SASKeyValue}";
   
    var wss = WebSocket.createRelayedServer(
        {
            server : WebSocket.createRelayListenUri(ns, path),
            token: WebSocket.createRelayToken('http://' + ns, keyrule, key)
        }, 
        function (ws) {
            console.log('connection accepted');
            ws.onmessage = function (event) {
                console.log(event.data);
            };
            ws.on('close', function () {
                console.log('connection closed');
            });       
    });
   
    console.log('listening');
   
    wss.on('error', function(err) {
        console.log('error' + err);
    });
    ```

