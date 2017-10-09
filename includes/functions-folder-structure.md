
az összes hello funkciók egy adott funkció alkalmazásban hello kód él, legfelső szintű mappa, amely tartalmaz egy állomás konfigurációs fájlja és hello kódot egy külön függvény, mint például a következő hello tartalmazhatnak, amelyek egy vagy több almappák:

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
```

Hello *host.json* tartalmaz néhány futásidejű konfigurációs fájl- és hello függvény alkalmazás gyökérmappájában hello helyezkedik el. A rendelkezésre álló beállítások információkért lásd: [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) hello WebJobs.Script tárház wiki.

Minden egyes függvény rendelkezik egy vagy több kódfájlok, hello function.json konfigurációs és egyéb függőségek tartalmazó mappát.

