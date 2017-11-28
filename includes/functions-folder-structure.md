
<span data-ttu-id="aa209-101">A kódot az összes, egy adott funkció alkalmazást a funkciókat él, az állomás konfigurációs fájlt tartalmazó legfelső szintű mappa és egy vagy több almappáiban, amelyek tartalmazzák a kód egy külön függvény, az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="aa209-101">The code for all of the functions in a given function app lives in a root folder that contains a host configuration file and one or more subfolders, each of which contain the code for a separate function, as in the following example:</span></span>

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

<span data-ttu-id="aa209-102">A *host.json* tartalmaz néhány futásidejű konfigurációs fájl- és a gyökérmappában található, a függvény alkalmazás helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="aa209-102">The *host.json* file contains some runtime-specific configuration and sits in the root folder of the function app.</span></span> <span data-ttu-id="aa209-103">A rendelkezésre álló beállítások információkért lásd: [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) a WebJobs.Script tárház wiki.</span><span class="sxs-lookup"><span data-stu-id="aa209-103">For information on settings that are available, see [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) in the WebJobs.Script repository wiki.</span></span>

<span data-ttu-id="aa209-104">Minden egyes függvény rendelkezik egy vagy több kódfájlok, a function.json konfigurációs és egyéb függőségek tartalmazó mappát.</span><span class="sxs-lookup"><span data-stu-id="aa209-104">Each function has a folder that contains one or more code files, the function.json configuration and other dependencies.</span></span>

