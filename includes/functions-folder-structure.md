
<span data-ttu-id="ed2c2-101">az összes hello funkciók egy adott funkció alkalmazásban hello kód él, legfelső szintű mappa, amely tartalmaz egy állomás konfigurációs fájlja és hello kódot egy külön függvény, mint például a következő hello tartalmazhatnak, amelyek egy vagy több almappák:</span><span class="sxs-lookup"><span data-stu-id="ed2c2-101">hello code for all of hello functions in a given function app lives in a root folder that contains a host configuration file and one or more subfolders, each of which contain hello code for a separate function, as in hello following example:</span></span>

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

<span data-ttu-id="ed2c2-102">Hello *host.json* tartalmaz néhány futásidejű konfigurációs fájl- és hello függvény alkalmazás gyökérmappájában hello helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="ed2c2-102">hello *host.json* file contains some runtime-specific configuration and sits in hello root folder of hello function app.</span></span> <span data-ttu-id="ed2c2-103">A rendelkezésre álló beállítások információkért lásd: [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) hello WebJobs.Script tárház wiki.</span><span class="sxs-lookup"><span data-stu-id="ed2c2-103">For information on settings that are available, see [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) in hello WebJobs.Script repository wiki.</span></span>

<span data-ttu-id="ed2c2-104">Minden egyes függvény rendelkezik egy vagy több kódfájlok, hello function.json konfigurációs és egyéb függőségek tartalmazó mappát.</span><span class="sxs-lookup"><span data-stu-id="ed2c2-104">Each function has a folder that contains one or more code files, hello function.json configuration and other dependencies.</span></span>

