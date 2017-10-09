### <a name="create-a-nodejs-application"></a>Node.js alkalmazás létrehozása

Hozzon létre egy `listener.js` nevű JavaScript-fájlt.

### <a name="add-hello-relay-npm-package"></a>Hello továbbítási NPM csomag hozzáadása

Futtassa az `npm install hyco-ws` parancsot a projektmappában lévő Csomópont parancssorból.

### <a name="write-some-code-tooreceive-messages"></a>Írnia egy kódrészletet tooreceive üzenetek

1. Adja hozzá a következő konstans toohello felső részén hello hello `listener.js` fájlt.
   
    ```js
    const WebSocket = require('hyco-ws');
    ```
2. Adja hozzá a következő állandókat toohello hello `listener.js` hello hibrid kapcsolat részletek fájlban találhatók. Szögletes zárójelbe hello helyőrzőket cserélje le hello értékek hello hibrid kapcsolat létrehozása után kapott.
   
   1. `const ns`-Továbbítási névtér hello. Lehet, hogy toouse hello teljesen minősített névtér neve; például `{namespace}.servicebus.windows.net`.
   2. `const path`-hello hibrid kapcsolat hello nevét.
   3. `const keyrule`-hello hello SAS-kulcs neve.
   4. `const key`-hello SAS-kulcs értékét.

3. Adja hozzá a következő kód toohello hello `listener.js` fájlt:
   
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
    A listener.js fájlnak így kell kinéznie:
   
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

