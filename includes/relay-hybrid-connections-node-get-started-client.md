### <a name="create-a-nodejs-application"></a>Node.js alkalmazás létrehozása

Hozzon létre egy `sender.js` nevű JavaScript-fájlt.

### <a name="add-hello-relay-npm-package"></a>Hello továbbítási NPM csomag hozzáadása

Futtassa az `npm install hyco-ws` parancsot a projektmappában lévő Csomópont parancssorból.

### <a name="write-some-code-toosend-messages"></a>Írnia egy kódrészletet toosend üzenetek

1. Adja hozzá a következő hello `constants` toohello felső részén hello `sender.js` fájlt.
   
    ```js
    const WebSocket = require('hyco-ws');
    const readline = require('readline')
        .createInterface({
            input: process.stdin,
            output: process.stdout
        });;
    ```
2. Adja hozzá a következő állandókat toohello hello `sender.js` hello hibrid kapcsolat részletek fájlban találhatók. Szögletes zárójelbe hello helyőrzőket cserélje le hello értékek hello hibrid kapcsolat létrehozása után kapott.
   
   1. `const ns`-Továbbítási névtér hello. Lehet, hogy toouse hello teljesen minősített névtér neve; például `{namespace}.servicebus.windows.net`.
   2. `const path`-hello hibrid kapcsolat hello nevét.
   3. `const keyrule`-hello hello SAS-kulcs neve.
   4. `const key`-hello SAS-kulcs értékét.

3. Adja hozzá a következő kód toohello hello `sender.js` fájlt:
   
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
    A sender.js fájlnak így kell kinéznie:
   
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

