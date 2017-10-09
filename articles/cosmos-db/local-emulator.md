---
title: "helyileg a hello Azure Cosmos DB emulátor aaaDevelop |} Microsoft Docs"
description: "Hello Azure Cosmos DB Emulator használatával, fejlesztése és tesztelése az alkalmazás helyileg szabad, Azure-előfizetés létrehozása nélkül."
services: cosmos-db
documentationcenter: 
keywords: "Az Azure Cosmos DB emulátor"
author: arramac
manager: jhubbard
editor: 
ms.assetid: 90b379a6-426b-4915-9635-822f1a138656
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: arramac
ms.openlocfilehash: fb5449489e5f71664e72d8e11e583315be371bf3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-cosmos-db-emulator-for-local-development-and-testing"></a><span data-ttu-id="756df-104">Hello Azure Cosmos DB emulátor használja a helyi fejlesztéshez és teszteléshez</span><span class="sxs-lookup"><span data-stu-id="756df-104">Use hello Azure Cosmos DB Emulator for local development and testing</span></span>

<table>
<tr>
  <td><span data-ttu-id="756df-105"><strong>Bináris fájlok</strong></span><span class="sxs-lookup"><span data-stu-id="756df-105"><strong>Binaries</strong></span></span></td>
  <td>[<span data-ttu-id="756df-106">MSI-fájl letöltése</span><span class="sxs-lookup"><span data-stu-id="756df-106">Download MSI</span></span>](https://aka.ms/cosmosdb-emulator)</td>
</tr>
<tr>
  <td><span data-ttu-id="756df-107"><strong>Docker</strong></span><span class="sxs-lookup"><span data-stu-id="756df-107"><strong>Docker</strong></span></span></td>
  <td>[<span data-ttu-id="756df-108">Docker központ</span><span class="sxs-lookup"><span data-stu-id="756df-108">Docker Hub</span></span>](https://hub.docker.com/r/microsoft/azure-documentdb-emulator/)</td>
</tr>
<tr>
  <td><span data-ttu-id="756df-109"><strong>Docker-forrás</strong></span><span class="sxs-lookup"><span data-stu-id="756df-109"><strong>Docker source</strong></span></span></td>
  <td>[<span data-ttu-id="756df-110">Github</span><span class="sxs-lookup"><span data-stu-id="756df-110">Github</span></span>](https://github.com/azure/azure-documentdb-emulator-docker)</td>
</tr>
</table>
  
<span data-ttu-id="756df-111">hello Azure Cosmos DB Emulator emulálja hello Azure Cosmos DB szolgáltatás fejlesztési célokra szolgál egy helyi környezet biztosít.</span><span class="sxs-lookup"><span data-stu-id="756df-111">hello Azure Cosmos DB Emulator provides a local environment that emulates hello Azure Cosmos DB service for development purposes.</span></span> <span data-ttu-id="756df-112">Hello Azure Cosmos DB Emulator használatával, létrehozhatja és helyileg, az alkalmazás tesztelése egy Azure-előfizetés létrehozása, és ezzel járó költségeket nélkül.</span><span class="sxs-lookup"><span data-stu-id="756df-112">Using hello Azure Cosmos DB Emulator, you can develop and test your application locally, without creating an Azure subscription or incurring any costs.</span></span> <span data-ttu-id="756df-113">Ha elégedett az alkalmazás a hello Azure Cosmos DB emulátor alakulását, toousing hello felhőben Azure Cosmos DB fiók válthat.</span><span class="sxs-lookup"><span data-stu-id="756df-113">When you're satisfied with how your application is working in hello Azure Cosmos DB Emulator, you can switch toousing an Azure Cosmos DB account in hello cloud.</span></span>

<span data-ttu-id="756df-114">Ez a cikk ismerteti a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="756df-114">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="756df-115">Hello emulátor telepítése</span><span class="sxs-lookup"><span data-stu-id="756df-115">Installing hello Emulator</span></span>
> * <span data-ttu-id="756df-116">A Docker a Windows hello emulátor fut</span><span class="sxs-lookup"><span data-stu-id="756df-116">Running hello Emulator on Docker for Windows</span></span>
> * <span data-ttu-id="756df-117">Kérések hitelesítése</span><span class="sxs-lookup"><span data-stu-id="756df-117">Authenticating requests</span></span>
> * <span data-ttu-id="756df-118">Az emulátor hello adatkezelő hello használata</span><span class="sxs-lookup"><span data-stu-id="756df-118">Using hello Data Explorer in hello Emulator</span></span>
> * <span data-ttu-id="756df-119">SSL-tanúsítványok exportálása</span><span class="sxs-lookup"><span data-stu-id="756df-119">Exporting SSL certificates</span></span>
> * <span data-ttu-id="756df-120">Hívása hello emulátor hello parancssorból</span><span class="sxs-lookup"><span data-stu-id="756df-120">Calling hello Emulator from hello command line</span></span>
> * <span data-ttu-id="756df-121">Nyomkövetési fájlok összegyűjtése</span><span class="sxs-lookup"><span data-stu-id="756df-121">Collecting trace files</span></span>

<span data-ttu-id="756df-122">Azt javasoljuk, hogy Kezdésként tekintse a következő videót, amely Kirill Gavrylyuk mutatja, hogyan tooget hello Azure Cosmos DB emulátor lépések hello.</span><span class="sxs-lookup"><span data-stu-id="756df-122">We recommend getting started by watching hello following video, where Kirill Gavrylyuk shows how tooget started with hello Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="756df-123">Vegye figyelembe, hogy hello videó hello DocumentDB emulátor toohello emulátor néven hivatkozik, de maga hello eszköz át lett nevezve hello Azure Cosmos DB emulátor hello videó taping óta.</span><span class="sxs-lookup"><span data-stu-id="756df-123">Note that hello video refers toohello emulator as hello DocumentDB Emulator, but hello tool itself has been renamed hello Azure Cosmos DB Emulator since taping hello video.</span></span> <span data-ttu-id="756df-124">Videó hello összes adata hello Azure Cosmos DB emulátor pontosak még.</span><span class="sxs-lookup"><span data-stu-id="756df-124">All information in hello video is still accurate for hello Azure Cosmos DB Emulator.</span></span> 

> [!VIDEO https://channel9.msdn.com/Events/Connect/2016/192/player]
> 
> 

## <a name="how-hello-emulator-works"></a><span data-ttu-id="756df-125">Hello emulátor működése</span><span class="sxs-lookup"><span data-stu-id="756df-125">How hello Emulator works</span></span>
<span data-ttu-id="756df-126">hello Azure Cosmos DB emulátor egy valósághű emulációját hello Azure Cosmos DB szolgáltatást biztosít.</span><span class="sxs-lookup"><span data-stu-id="756df-126">hello Azure Cosmos DB Emulator provides a high-fidelity emulation of hello Azure Cosmos DB service.</span></span> <span data-ttu-id="756df-127">Azure Cosmos DB, például létrehozása és a JSON-dokumentumok lekérdezését kiépítés azonos funkciókat támogatja, és gyűjtemények skálázás, és végrehajtása tárolt eljárásokként és eseményindítókként.</span><span class="sxs-lookup"><span data-stu-id="756df-127">It supports identical functionality as Azure Cosmos DB, including support for creating and querying JSON documents, provisioning and scaling collections, and executing stored procedures and triggers.</span></span> <span data-ttu-id="756df-128">Fejlesztés és hello Azure Cosmos DB emulátor használó alkalmazások tesztelése, és telepítheti őket az Azure Cosmos DB toohello csatlakozási végpont megváltoztatásához egyetlen konfigurációja csak tételével globális léptékű tooAzure.</span><span class="sxs-lookup"><span data-stu-id="756df-128">You can develop and test applications using hello Azure Cosmos DB Emulator, and deploy them tooAzure at global scale by just making a single configuration change toohello connection endpoint for Azure Cosmos DB.</span></span>

<span data-ttu-id="756df-129">Létrehoztunk egy valósághű helyi emuláció hello tényleges Azure Cosmos DB szolgáltatás, miközben hello végrehajtásának hello Azure Cosmos DB emulátor hello szolgáltatást, amely eltér.</span><span class="sxs-lookup"><span data-stu-id="756df-129">While we created a high-fidelity local emulation of hello actual Azure Cosmos DB service, hello implementation of hello Azure Cosmos DB Emulator is different than that of hello service.</span></span> <span data-ttu-id="756df-130">Például hello Azure Cosmos DB emulátor használja például a helyi fájlrendszer hello szabványos operációs rendszer összetevőit adatmegőrzési, és a HTTPS protokollhoz tartozó, a hálózati kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="756df-130">For example, hello Azure Cosmos DB Emulator uses standard OS components such as hello local file system for persistence, and HTTPS protocol stack for connectivity.</span></span> <span data-ttu-id="756df-131">Ez azt jelenti, hogy bizonyos funkciók, például globális replikációs, egyjegyű ezredmásodperces késési olvasása/írása, és beállítható konzisztenciaszinteket hello Azure Cosmos DB emulátor keresztül nem érhetők el az Azure infrastruktúra támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="756df-131">This means that some functionality that relies on Azure infrastructure like global replication, single-digit millisecond latency for reads/writes, and tunable consistency levels are not available via hello Azure Cosmos DB Emulator.</span></span>

> [!NOTE]
> <span data-ttu-id="756df-132">Ez idő hello adatkezelő hello: emulátor csak DocumentDB API gyűjtemények és a MongoDB-gyűjtemény hello létrehozását támogatja.</span><span class="sxs-lookup"><span data-stu-id="756df-132">At this time hello Data Explorer in hello emulator only supports hello creation of DocumentDB API collections and MongoDB collections.</span></span> <span data-ttu-id="756df-133">hello adatkezelő hello emulátorban jelenleg nem támogatja a hello létrehozását táblázatokat és diagramokat.</span><span class="sxs-lookup"><span data-stu-id="756df-133">hello Data Explorer in hello emulator does not currently support hello creation of tables and graphs.</span></span> 

## <a name="system-requirements"></a><span data-ttu-id="756df-134">Rendszerkövetelmények</span><span class="sxs-lookup"><span data-stu-id="756df-134">System requirements</span></span>
<span data-ttu-id="756df-135">hello Azure Cosmos DB emulátor hardver- és szoftverkövetelmények a következő hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="756df-135">hello Azure Cosmos DB Emulator has hello following hardware and software requirements:</span></span>

* <span data-ttu-id="756df-136">Szoftverkövetelmények</span><span class="sxs-lookup"><span data-stu-id="756df-136">Software requirements</span></span>
  * <span data-ttu-id="756df-137">Windows Server 2012 R2, Windows Server 2016 vagy Windows 10</span><span class="sxs-lookup"><span data-stu-id="756df-137">Windows Server 2012 R2, Windows Server 2016, or Windows 10</span></span>
*   <span data-ttu-id="756df-138">Minimális hardverkövetelmények</span><span class="sxs-lookup"><span data-stu-id="756df-138">Minimum Hardware requirements</span></span>
  * <span data-ttu-id="756df-139">2 GB RAM</span><span class="sxs-lookup"><span data-stu-id="756df-139">2 GB RAM</span></span>
  * <span data-ttu-id="756df-140">10 GB szabad lemezterület</span><span class="sxs-lookup"><span data-stu-id="756df-140">10 GB available hard disk space</span></span>

## <a name="installation"></a><span data-ttu-id="756df-141">Telepítés</span><span class="sxs-lookup"><span data-stu-id="756df-141">Installation</span></span>
<span data-ttu-id="756df-142">Töltse le, és telepítse a hello Azure Cosmos DB emulátor hello a [Microsoft Download Center](https://aka.ms/cosmosdb-emulator).</span><span class="sxs-lookup"><span data-stu-id="756df-142">You can download and install hello Azure Cosmos DB Emulator from hello [Microsoft Download Center](https://aka.ms/cosmosdb-emulator).</span></span> 

> [!NOTE]
> <span data-ttu-id="756df-143">tooinstall, konfigurálásához és futtatásához hello Azure Cosmos DB emulátor, hello számítógépen rendszergazdai jogosultsággal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="756df-143">tooinstall, configure, and run hello Azure Cosmos DB Emulator, you must have administrative privileges on hello computer.</span></span>

## <a name="running-on-docker-for-windows"></a><span data-ttu-id="756df-144">A Windows Docker fut</span><span class="sxs-lookup"><span data-stu-id="756df-144">Running on Docker for Windows</span></span>

<span data-ttu-id="756df-145">Docker a Windows hello Azure Cosmos DB emulátor futtatható.</span><span class="sxs-lookup"><span data-stu-id="756df-145">hello Azure Cosmos DB Emulator can be run on Docker for Windows.</span></span> <span data-ttu-id="756df-146">hello emulátor nem működnek a Docker az Oracle Linux.</span><span class="sxs-lookup"><span data-stu-id="756df-146">hello Emulator does not work on Docker for Oracle Linux.</span></span>

<span data-ttu-id="756df-147">Ha már [Docker for Windows](https://www.docker.com/docker-windows) telepítve, akkor is lekérés hello emulátor rendszerképe a Docker Hub futtassa a következő parancsot a kedvenc rendszerhéjból hello (cmd.exe, PowerShell, stb.).</span><span class="sxs-lookup"><span data-stu-id="756df-147">Once you have [Docker for Windows](https://www.docker.com/docker-windows) installed, you can pull hello Emulator image from Docker Hub by running hello following command from your favorite shell (cmd.exe, PowerShell, etc.).</span></span>

```      
docker pull microsoft/azure-cosmosdb-emulator 
```
<span data-ttu-id="756df-148">toostart hello kép, futtassa a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="756df-148">toostart hello image, run hello following commands.</span></span>

``` 
md %LOCALAPPDATA%\CosmosDBEmulatorCert 2>nul
docker run -v %LOCALAPPDATA%\CosmosDBEmulatorCert:c:\CosmosDBEmulator\CosmosDBEmulatorCert -P -t -i microsoft/azure-cosmosdb-emulator 
```

<span data-ttu-id="756df-149">hello válasz a következőhöz hasonló toohello következő:</span><span class="sxs-lookup"><span data-stu-id="756df-149">hello response looks similar toohello following:</span></span>

```
Starting Emulator
Emulator Endpoint: https://172.20.229.193:8081/
Master Key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
Exporting SSL Certificate
You can import hello SSL certificate from an administrator command prompt on hello host by running:
cd /d %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
--------------------------------------------------------------------------------------------------
Starting interactive shell
``` 

<span data-ttu-id="756df-150">Hello interaktív rendszerhéj bezárása, ha emulátor elindult hello leállítási hello emulátor tároló.</span><span class="sxs-lookup"><span data-stu-id="756df-150">Closing hello interactive shell once hello Emulator has been started will shutdown hello Emulator’s container.</span></span>

<span data-ttu-id="756df-151">Hello végpont és a hello válaszban található főkulcs használata az ügyfél és hello SSL-tanúsítvány importálása a gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="756df-151">Use hello endpoint and master key in from hello response in your client and import hello SSL certificate into your host.</span></span> <span data-ttu-id="756df-152">SSL-tanúsítvány, tooimport hello hello egy rendszergazdai parancssorból a következő:</span><span class="sxs-lookup"><span data-stu-id="756df-152">tooimport hello SSL certificate, do hello following from an admin command prompt:</span></span>

```
cd %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
```


## <a name="start-hello-emulator"></a><span data-ttu-id="756df-153">Indítsa el a hello emulátor</span><span class="sxs-lookup"><span data-stu-id="756df-153">Start hello Emulator</span></span>

<span data-ttu-id="756df-154">toostart hello Azure Cosmos DB emulátor hello Start gombra, vagy hello Windows billentyűt.</span><span class="sxs-lookup"><span data-stu-id="756df-154">toostart hello Azure Cosmos DB Emulator, select hello Start button or press hello Windows key.</span></span> <span data-ttu-id="756df-155">Írja be a szöveget **Azure Cosmos DB emulátor**, és jelölje be hello emulátor hello alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="756df-155">Begin typing **Azure Cosmos DB Emulator**, and select hello emulator from hello list of applications.</span></span> 

![Hello Start gombra, vagy nyomja le az hello Windows kulcs kiválasztásához, írja be a szöveget ** Azure Cosmos DB emulátor **, és jelölje be hello emulátor hello alkalmazások listája](./media/local-emulator/database-local-emulator-start.png)

<span data-ttu-id="756df-157">Hello emulátor futtatásakor a Windows tálca értesítési területén hello egy ikon látható.</span><span class="sxs-lookup"><span data-stu-id="756df-157">When hello emulator is running, you'll see an icon in hello Windows taskbar notification area.</span></span> ![Az Azure Cosmos DB helyi emulátor értesítési](./media/local-emulator/database-local-emulator-taskbar.png)

<span data-ttu-id="756df-159">hello Azure Cosmos DB emulátor alapértelmezés szerint a hello helyi számítógépen ("localhost") porton 8081 fut.</span><span class="sxs-lookup"><span data-stu-id="756df-159">hello Azure Cosmos DB Emulator by default runs on hello local machine ("localhost") listening on port 8081.</span></span>

<span data-ttu-id="756df-160">hello Azure Cosmos DB emulátor szerint telepíti a rendszer alapértelmezett toohello `C:\Program Files\Azure Cosmos DB Emulator` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="756df-160">hello Azure Cosmos DB Emulator is installed by default toohello `C:\Program Files\Azure Cosmos DB Emulator` directory.</span></span> <span data-ttu-id="756df-161">Is elindíthatja, és leállítja a parancssori hello hello emulátor.</span><span class="sxs-lookup"><span data-stu-id="756df-161">You can also start and stop hello emulator from hello command-line.</span></span> <span data-ttu-id="756df-162">Lásd: [parancssori eszköz](#command-line) további információt.</span><span class="sxs-lookup"><span data-stu-id="756df-162">See [command-line tool reference](#command-line) for more information.</span></span>

## <a name="start-data-explorer"></a><span data-ttu-id="756df-163">Adatok Explorer elindítása</span><span class="sxs-lookup"><span data-stu-id="756df-163">Start Data Explorer</span></span>

<span data-ttu-id="756df-164">Amikor hello Azure Cosmos DB emulátor elindítja azt automatikusan megnyílik hello Azure Cosmos DB adatkezelő a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="756df-164">When hello Azure Cosmos DB emulator launches it will automatically open hello Azure Cosmos DB Data Explorer in your browser.</span></span> <span data-ttu-id="756df-165">hello cím jelenik meg [https://localhost:8081/_explorer/index.html](https://localhost:8081/_explorer/index.html).</span><span class="sxs-lookup"><span data-stu-id="756df-165">hello address will appear as [https://localhost:8081/_explorer/index.html](https://localhost:8081/_explorer/index.html).</span></span> <span data-ttu-id="756df-166">Ha akkor zárja be hello explorer, és szeretné toore-nyissa meg azt később hello URL-cím megnyitása a böngészőben, vagy indítsa el a hello Azure Cosmos DB emulátor tálcaikon Windows hello a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="756df-166">If you close hello explorer and would like toore-open it later, you can either open hello URL in your browser or launch it from hello Azure Cosmos DB Emulator in hello Windows Tray Icon as shown below.</span></span>

![Az Azure Cosmos DB helyi emulátor data explorer indítója](./media/local-emulator/database-local-emulator-data-explorer-launcher.png)

## <a name="checking-for-updates"></a><span data-ttu-id="756df-168">Frissítések keresése</span><span class="sxs-lookup"><span data-stu-id="756df-168">Checking for updates</span></span>
<span data-ttu-id="756df-169">Adatkezelő azt jelzi, ha van egy új frissítés elérhető letöltésre.</span><span class="sxs-lookup"><span data-stu-id="756df-169">Data Explorer indicates if there is a new update available for download.</span></span> 

> [!NOTE]
> <span data-ttu-id="756df-170">Hello Azure Cosmos DB emulátor egy verziójában létrehozott adatokat nem garantált toobe érhető el egy másik verzió használata esetén.</span><span class="sxs-lookup"><span data-stu-id="756df-170">Data created in one version of hello Azure Cosmos DB Emulator is not guaranteed toobe accessible when using a different version.</span></span> <span data-ttu-id="756df-171">Ha toopersist az adatok hosszú távú hello, ajánlott, hogy egy Cosmos-DB Azure-fiók, nem pedig a hello Azure Cosmos DB emulátor tárolja ezeket az adatokat.</span><span class="sxs-lookup"><span data-stu-id="756df-171">If you need toopersist your data for hello long term, it is recommended that you store that data in an Azure Cosmos DB account, rather than in hello Azure Cosmos DB Emulator.</span></span> 

## <a name="authenticating-requests"></a><span data-ttu-id="756df-172">Kérések hitelesítése</span><span class="sxs-lookup"><span data-stu-id="756df-172">Authenticating requests</span></span>
<span data-ttu-id="756df-173">Ahogy az Azure Cosmos DB hello felhőben hello Azure Cosmos DB emulátor képest végrehajtott kérelmek hitelesíteni kell.</span><span class="sxs-lookup"><span data-stu-id="756df-173">Just as with Azure Cosmos DB in hello cloud, every request that you make against hello Azure Cosmos DB Emulator must be authenticated.</span></span> <span data-ttu-id="756df-174">hello Azure Cosmos DB emulátor főkulcs hitelesítéshez ugyanazt a rögzített fiókot és egy jól ismert hitelesítési kulcs támogatja.</span><span class="sxs-lookup"><span data-stu-id="756df-174">hello Azure Cosmos DB Emulator supports a single fixed account and a well-known authentication key for master key authentication.</span></span> <span data-ttu-id="756df-175">A fiók és a kulcs nem hello csak hitelesítő adatokkal való használathoz az Azure Cosmos DB emulátor hello engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="756df-175">This account and key are hello only credentials permitted for use with hello Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="756df-176">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="756df-176">They are:</span></span>

    Account name: localhost:<port>
    Account key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==

> [!NOTE]
> <span data-ttu-id="756df-177">hello Azure Cosmos DB emulátor által támogatott hello főkulcs csak hello emulátor való használatra készült.</span><span class="sxs-lookup"><span data-stu-id="756df-177">hello master key supported by hello Azure Cosmos DB Emulator is intended for use only with hello emulator.</span></span> <span data-ttu-id="756df-178">Az éles Azure Cosmos DB fiókot és kulcsot hello Azure Cosmos DB emulátor nem használható.</span><span class="sxs-lookup"><span data-stu-id="756df-178">You cannot use your production Azure Cosmos DB account and key with hello Azure Cosmos DB Emulator.</span></span> 

> [!NOTE] 
> <span data-ttu-id="756df-179">Hello emulátor indította hello /Key lehetőséget, ha létrehozott hello kulcsa használja "C2y6yDjf5/R + ob0N8A7Cgv30VRDJIWEHLM + 4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw =="</span><span class="sxs-lookup"><span data-stu-id="756df-179">If you have started hello emulator with hello /Key option, then use hello generated key instead of "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw=="</span></span>

<span data-ttu-id="756df-180">Továbbá, ugyanúgy, mint a hello Azure Cosmos DB service, Azure Cosmos DB emulátor hello támogatja az SSL-en keresztüli csak biztonságos kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="756df-180">Additionally, just as hello Azure Cosmos DB service, hello Azure Cosmos DB Emulator supports only secure communication via SSL.</span></span>

## <a name="running-hello-emulator-on-a-local-network"></a><span data-ttu-id="756df-181">A helyi hálózaton futó hello emulátor</span><span class="sxs-lookup"><span data-stu-id="756df-181">Running hello emulator on a local network</span></span>

<span data-ttu-id="756df-182">Hello emulator egy helyi hálózaton is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="756df-182">You can run hello emulator on a local network.</span></span> <span data-ttu-id="756df-183">tooenable hálózati hozzáférés, adja meg a hello /AllowNetworkAccess kapcsolót hello [parancssori](#command-line-syntax), ami megköveteli azt is meg kell adnia /Key = key_string vagy /KeyFile = fájlnév.</span><span class="sxs-lookup"><span data-stu-id="756df-183">tooenable network access, specify hello /AllowNetworkAccess option at hello [command line](#command-line-syntax), which also requires that you specify /Key=key_string or /KeyFile=file_name.</span></span> <span data-ttu-id="756df-184">Használhatja a /GenKeyFile = fájlnév toogenerate fájl véletlenszerű kulccsal előzetes megfizetése esetén.</span><span class="sxs-lookup"><span data-stu-id="756df-184">You can use /GenKeyFile=file_name toogenerate a file with a random key upfront.</span></span>  <span data-ttu-id="756df-185">Ezt követően, hogy túl/KeyFile átadhatók = fájlnév vagy /Key = contents_of_file.</span><span class="sxs-lookup"><span data-stu-id="756df-185">Then you can pass that too/KeyFile=file_name or /Key=contents_of_file.</span></span>

<span data-ttu-id="756df-186">tooenable hálózati hozzáférés hello első alkalommal hello felhasználó leállítási hello emulátor kell, és törölje a hello emulátor adatkönyvtára (C:\Users\user_name\AppData\Local\CosmosDBEmulator).</span><span class="sxs-lookup"><span data-stu-id="756df-186">tooenable network access for hello first time hello user should shutdown hello emulator and delete hello emulator’s data directory (C:\Users\user_name\AppData\Local\CosmosDBEmulator).</span></span>

## <a name="developing-with-hello-emulator"></a><span data-ttu-id="756df-187">Fejlesztés az emulátor hello</span><span class="sxs-lookup"><span data-stu-id="756df-187">Developing with hello Emulator</span></span>
<span data-ttu-id="756df-188">Ha Ön rendelkezik hello Azure Cosmos DB-emulátort az asztalon, használata támogatott [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) vagy hello [Azure Cosmos DB REST API](/rest/api/documentdb/) az emulátor hello toointeract.</span><span class="sxs-lookup"><span data-stu-id="756df-188">Once you have hello Azure Cosmos DB Emulator running on your desktop, you can use any supported [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) or hello [Azure Cosmos DB REST API](/rest/api/documentdb/) toointeract with hello Emulator.</span></span> <span data-ttu-id="756df-189">hello Azure Cosmos DB emulátor egy beépített adatkezelő, amely lehetővé teszi, hogy a gyűjtemények hello DocumentDB és MongoDB API-kat, és tekintse meg a dokumentumok létrehozásához és szerkesztéséhez programozás nélkül is.</span><span class="sxs-lookup"><span data-stu-id="756df-189">hello Azure Cosmos DB Emulator also includes a built-in Data Explorer that lets you create collections for hello DocumentDB and MongoDB APIs, and view and edit documents without writing any code.</span></span>   

    // Connect toohello Azure Cosmos DB Emulator running locally
    DocumentClient client = new DocumentClient(
        new Uri("https://localhost:8081"), 
        "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==");

<span data-ttu-id="756df-190">Ha használ [Azure Cosmos DB protokoll támogatása mongodb](mongodb-introduction.md), használja a következő kapcsolati karakterlánc hello:</span><span class="sxs-lookup"><span data-stu-id="756df-190">If you're using [Azure Cosmos DB protocol support for MongoDB](mongodb-introduction.md), please use hello following connection string:</span></span>

    mongodb://localhost:C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==@localhost:10255/admin?ssl=true&3t.sslSelfSignedCerts=true

<span data-ttu-id="756df-191">Használhatja például a meglévő eszközöket [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio) tooconnect toohello Azure Cosmos DB emulátor.</span><span class="sxs-lookup"><span data-stu-id="756df-191">You can use existing tools like [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio) tooconnect toohello Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="756df-192">Is áttelepítheti az adatokat közötti hello Azure Cosmos DB emulátor és hello Azure Cosmos DB szolgáltatást hello [Azure Cosmos DB adatáttelepítési eszköz](https://github.com/azure/azure-documentdb-datamigrationtool).</span><span class="sxs-lookup"><span data-stu-id="756df-192">You can also migrate data between hello Azure Cosmos DB Emulator and hello Azure Cosmos DB service using hello [Azure Cosmos DB Data Migration Tool](https://github.com/azure/azure-documentdb-datamigrationtool).</span></span>

> [!NOTE] 
> <span data-ttu-id="756df-193">Hello emulátor indította hello /Key lehetőséget, ha létrehozott hello kulcsa használja "C2y6yDjf5/R + ob0N8A7Cgv30VRDJIWEHLM + 4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw =="</span><span class="sxs-lookup"><span data-stu-id="756df-193">If you have started hello emulator with hello /Key option, then use hello generated key instead of "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw=="</span></span>

<span data-ttu-id="756df-194">Hello Azure Cosmos DB emulátor, alapértelmezés szerint használva létrehozhat too25 az egypartíciós gyűjtemények vagy 1 particionált gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="756df-194">Using hello Azure Cosmos DB emulator, by default, you can create up too25 single partition collections or 1 partitioned collection.</span></span> <span data-ttu-id="756df-195">Ez az érték módosításával kapcsolatos további információkért lásd: [hello PartitionCount érték](#set-partitioncount).</span><span class="sxs-lookup"><span data-stu-id="756df-195">For more information about changing this value, see [Setting hello PartitionCount value](#set-partitioncount).</span></span>

## <a name="export-hello-ssl-certificate"></a><span data-ttu-id="756df-196">Hello SSL-tanúsítvány exportálása</span><span class="sxs-lookup"><span data-stu-id="756df-196">Export hello SSL certificate</span></span>

<span data-ttu-id="756df-197">.NET-nyelveket és futásidejű használata hello Windows tanúsítványtároló toosecurely csatlakozás toohello Azure Cosmos DB helyi emulátor.</span><span class="sxs-lookup"><span data-stu-id="756df-197">.NET languages and runtime use hello Windows Certificate Store toosecurely connect toohello Azure Cosmos DB local emulator.</span></span> <span data-ttu-id="756df-198">Nyelvek kezelése és tanúsítványok segítségével a saját metódus rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="756df-198">Other languages have their own method of managing and using certificates.</span></span> <span data-ttu-id="756df-199">Java használ a saját [tanúsítványtároló](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) mivel Python használ [burkolók szoftvercsatorna](https://docs.python.org/2/library/ssl.html).</span><span class="sxs-lookup"><span data-stu-id="756df-199">Java uses its own [certificate store](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) whereas Python uses [socket wrappers](https://docs.python.org/2/library/ssl.html).</span></span>

<span data-ttu-id="756df-200">A sorrend tooobtain nyelvet és a futtatókörnyezetek, amely integrálható a tanúsítványtároló Windows hello, egy tanúsítvány toouse kell tooexport Tanúsítványkezelő Windows hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="756df-200">In order tooobtain a certificate toouse with languages and runtimes that do not integrate with hello Windows Certificate Store you will need tooexport it using hello Windows Certificate Manager.</span></span> <span data-ttu-id="756df-201">Indítsa el a certlm.msc futtatásával, vagy hello részletes utasításokat követve [hello Azure Cosmos DB emulátor tanúsítványok exportálása](./local-emulator-export-ssl-certificates.md).</span><span class="sxs-lookup"><span data-stu-id="756df-201">You can start it by running certlm.msc or follow hello step by step instructions in [Export hello Azure Cosmos DB Emulator Certificates](./local-emulator-export-ssl-certificates.md).</span></span> <span data-ttu-id="756df-202">Hello Tanúsítványkezelő futásakor, hello nyissa meg a személyes tanúsítványok alább látható módon és exportálási hello hello rövid nevű "DocumentDBEmulatorCertificate" tanúsítvány, egy BASE-64 kódolású X.509 (.cer) fájlba.</span><span class="sxs-lookup"><span data-stu-id="756df-202">Once hello certificate manager is running, open hello Personal Certificates as shown below and export hello certificate with hello friendly name "DocumentDBEmulatorCertificate" as a BASE-64 encoded X.509 (.cer) file.</span></span>

![Az Azure Cosmos DB helyi emulátor SSL-tanúsítvány](./media/local-emulator/database-local-emulator-ssl_certificate.png)

<span data-ttu-id="756df-204">hello Java tanúsítványtároló hello X.509 tanúsítvány importálható hello utasításait követve [hozzáadása a Java-hitelesítésszolgáltató tanúsítványtárolójában tanúsítvány toohello](https://docs.microsoft.com/azure/java-add-certificate-ca-store).</span><span class="sxs-lookup"><span data-stu-id="756df-204">hello X.509 certificate can be imported into hello Java certificate store by following hello instructions in [Adding a Certificate toohello Java CA Certificates Store](https://docs.microsoft.com/azure/java-add-certificate-ca-store).</span></span> <span data-ttu-id="756df-205">Hello tanúsítvány hello tanúsítványtárolóba importálása, Java és a MongoDB alkalmazások képesek tooconnect toohello Azure Cosmos DB emulátor állnak.</span><span class="sxs-lookup"><span data-stu-id="756df-205">Once hello certificate is imported into hello certificate store, Java and MongoDB applications will be able tooconnect toohello Azure Cosmos DB Emulator.</span></span>

<span data-ttu-id="756df-206">A Python és a Node.js SDK-k toohello emulátor kapcsolódáskor SSL ellenőrzés le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="756df-206">When connecting toohello emulator from Python and Node.js SDKs, SSL verification is disabled.</span></span>

## <span data-ttu-id="756df-207"><a id="command-line"></a>Parancssori eszköz</span><span class="sxs-lookup"><span data-stu-id="756df-207"><a id="command-line"></a>Command-line tool reference</span></span>
<span data-ttu-id="756df-208">Hello telepítési helyéről hello parancssori toostart használjon és hello emulátor leállítása, beállításokat adhat meg, és egyéb műveleteket végezhet.</span><span class="sxs-lookup"><span data-stu-id="756df-208">From hello installation location, you can use hello command-line toostart and stop hello emulator, configure options, and perform other operations.</span></span>

### <a name="command-line-syntax"></a><span data-ttu-id="756df-209">Parancssori szintaxis</span><span class="sxs-lookup"><span data-stu-id="756df-209">Command-line Syntax</span></span>

    CosmosDB.Emulator.exe [/Shutdown] [/DataPath] [/Port] [/MongoPort] [/DirectPorts] [/Key] [/EnableRateLimiting] [/DisableRateLimiting] [/NoUI] [/NoExplorer] [/?]

<span data-ttu-id="756df-210">beállítások, típus tooview hello listájának `CosmosDB.Emulator.exe /?` hello parancssorban.</span><span class="sxs-lookup"><span data-stu-id="756df-210">tooview hello list of options, type `CosmosDB.Emulator.exe /?` at hello command prompt.</span></span>

<table>
<tr>
  <td><span data-ttu-id="756df-211"><strong>A beállítás</strong></span><span class="sxs-lookup"><span data-stu-id="756df-211"><strong>Option</strong></span></span></td>
  <td><span data-ttu-id="756df-212"><strong>Leírás</strong></span><span class="sxs-lookup"><span data-stu-id="756df-212"><strong>Description</strong></span></span></td>
  <td><span data-ttu-id="756df-213"><strong>A parancs</strong></span><span class="sxs-lookup"><span data-stu-id="756df-213"><strong>Command</strong></span></span></td>
  <td><span data-ttu-id="756df-214"><strong>Argumentumok</strong></span><span class="sxs-lookup"><span data-stu-id="756df-214"><strong>Arguments</strong></span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="756df-215">[Nincs argumentumok]</span><span class="sxs-lookup"><span data-stu-id="756df-215">[No arguments]</span></span></td>
  <td><span data-ttu-id="756df-216">Hello Azure Cosmos DB emulátor alapértelmezett beállításokkal indul.</span><span class="sxs-lookup"><span data-stu-id="756df-216">Starts up hello Azure Cosmos DB Emulator with default settings.</span></span></td>
  <td><span data-ttu-id="756df-217">CosmosDB.Emulator.exe</span><span class="sxs-lookup"><span data-stu-id="756df-217">CosmosDB.Emulator.exe</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="756df-218">[Súgó]</span><span class="sxs-lookup"><span data-stu-id="756df-218">[Help]</span></span></td>
  <td><span data-ttu-id="756df-219">Hello listájának megjelenítése támogatott parancssori argumentumokat.</span><span class="sxs-lookup"><span data-stu-id="756df-219">Displays hello list of supported command-line arguments.</span></span></td>
  <td><span data-ttu-id="756df-220">CosmosDB.Emulator.exe /?</span><span class="sxs-lookup"><span data-stu-id="756df-220">CosmosDB.Emulator.exe /?</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="756df-221">Leállítás</span><span class="sxs-lookup"><span data-stu-id="756df-221">Shutdown</span></span></td>
  <td><span data-ttu-id="756df-222">Hello Azure Cosmos DB emulátor leáll.</span><span class="sxs-lookup"><span data-stu-id="756df-222">Shuts down hello Azure Cosmos DB Emulator.</span></span></td>
  <td><span data-ttu-id="756df-223">CosmosDB.Emulator.exe shutdown</span><span class="sxs-lookup"><span data-stu-id="756df-223">CosmosDB.Emulator.exe /Shutdown</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="756df-224">DataPath</span><span class="sxs-lookup"><span data-stu-id="756df-224">DataPath</span></span></td>
  <td><span data-ttu-id="756df-225">A toostore adatfájlok hello elérési út megadása</span><span class="sxs-lookup"><span data-stu-id="756df-225">Specifies hello path in which toostore data files.</span></span> <span data-ttu-id="756df-226">Alapértelmezett érték a % LocalAppdata%\CosmosDBEmulator.</span><span class="sxs-lookup"><span data-stu-id="756df-226">Default is %LocalAppdata%\CosmosDBEmulator.</span></span></td>
  <td><span data-ttu-id="756df-227">CosmosDB.Emulator.exe /DataPath =&lt;datapath&gt;</span><span class="sxs-lookup"><span data-stu-id="756df-227">CosmosDB.Emulator.exe /DataPath=&lt;datapath&gt;</span></span></td>
  <td><span data-ttu-id="756df-228">&lt;DataPath&gt;: egy elérhető elérési útja</span><span class="sxs-lookup"><span data-stu-id="756df-228">&lt;datapath&gt;: An accessible path</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="756df-229">Port</span><span class="sxs-lookup"><span data-stu-id="756df-229">Port</span></span></td>
  <td><span data-ttu-id="756df-230">Hello port száma toouse hello emulátor határozza meg.</span><span class="sxs-lookup"><span data-stu-id="756df-230">Specifies hello port number toouse for hello emulator.</span></span>  <span data-ttu-id="756df-231">Alapértelmezés szerint 8081.</span><span class="sxs-lookup"><span data-stu-id="756df-231">Default is 8081.</span></span></td>
  <td><span data-ttu-id="756df-232">CosmosDB.Emulator.exe/port =&lt;port&gt;</span><span class="sxs-lookup"><span data-stu-id="756df-232">CosmosDB.Emulator.exe /Port=&lt;port&gt;</span></span></td>
  <td><span data-ttu-id="756df-233">&lt;port&gt;: egy portszám</span><span class="sxs-lookup"><span data-stu-id="756df-233">&lt;port&gt;: Single port number</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="756df-234">MongoPort</span><span class="sxs-lookup"><span data-stu-id="756df-234">MongoPort</span></span></td>
  <td><span data-ttu-id="756df-235">Megadja a hello port száma toouse MongoDB kompatibilitás API.</span><span class="sxs-lookup"><span data-stu-id="756df-235">Specifies hello port number toouse for MongoDB compatibility API.</span></span> <span data-ttu-id="756df-236">Alapértelmezés szerint 10255.</span><span class="sxs-lookup"><span data-stu-id="756df-236">Default is 10255.</span></span></td>
  <td><span data-ttu-id="756df-237">CosmosDB.Emulator.exe /MongoPort =&lt;mongoport&gt;</span><span class="sxs-lookup"><span data-stu-id="756df-237">CosmosDB.Emulator.exe /MongoPort=&lt;mongoport&gt;</span></span></td>
  <td><span data-ttu-id="756df-238">&lt;mongoport&gt;: egy portszám</span><span class="sxs-lookup"><span data-stu-id="756df-238">&lt;mongoport&gt;: Single port number</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="756df-239">DirectPorts</span><span class="sxs-lookup"><span data-stu-id="756df-239">DirectPorts</span></span></td>
  <td><span data-ttu-id="756df-240">Megadja a hello portok toouse közvetlen kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="756df-240">Specifies hello ports toouse for direct connectivity.</span></span> <span data-ttu-id="756df-241">Alapértelmezett 10251,10252,10253,10254.</span><span class="sxs-lookup"><span data-stu-id="756df-241">Defaults are 10251,10252,10253,10254.</span></span></td>
  <td><span data-ttu-id="756df-242">CosmosDB.Emulator.exe /DirectPorts:&lt;directports&gt;</span><span class="sxs-lookup"><span data-stu-id="756df-242">CosmosDB.Emulator.exe /DirectPorts:&lt;directports&gt;</span></span></td>
  <td><span data-ttu-id="756df-243">&lt;directports&gt;: 4 portok vesszővel tagolt listája</span><span class="sxs-lookup"><span data-stu-id="756df-243">&lt;directports&gt;: Comma-delimited list of 4 ports</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="756df-244">Kulcs</span><span class="sxs-lookup"><span data-stu-id="756df-244">Key</span></span></td>
  <td><span data-ttu-id="756df-245">Hello emulátor hitelesítési kulcs.</span><span class="sxs-lookup"><span data-stu-id="756df-245">Authorization key for hello emulator.</span></span> <span data-ttu-id="756df-246">Kulcs hello base 64 kódolás egy 64 bájtos vektor kell lennie.</span><span class="sxs-lookup"><span data-stu-id="756df-246">Key must be hello base-64 encoding of a 64-byte vector.</span></span></td>
  <td><span data-ttu-id="756df-247">CosmosDB.Emulator.exe /Key:&lt;kulcs&gt;</span><span class="sxs-lookup"><span data-stu-id="756df-247">CosmosDB.Emulator.exe /Key:&lt;key&gt;</span></span></td>
  <td><span data-ttu-id="756df-248">&lt;kulcs&gt;: hello base 64 kódolás egy 64 bájtos vektor kulcsnak kell lennie</span><span class="sxs-lookup"><span data-stu-id="756df-248">&lt;key&gt;: Key must be hello base-64 encoding of a 64-byte vector</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="756df-249">EnableRateLimiting</span><span class="sxs-lookup"><span data-stu-id="756df-249">EnableRateLimiting</span></span></td>
  <td><span data-ttu-id="756df-250">Megadja, hogy korlátozza az viselkedés kérelmek aránya engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="756df-250">Specifies that request rate limiting behavior is enabled.</span></span></td>
  <td><span data-ttu-id="756df-251">CosmosDB.Emulator.exe /EnableRateLimiting</span><span class="sxs-lookup"><span data-stu-id="756df-251">CosmosDB.Emulator.exe /EnableRateLimiting</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="756df-252">DisableRateLimiting</span><span class="sxs-lookup"><span data-stu-id="756df-252">DisableRateLimiting</span></span></td>
  <td><span data-ttu-id="756df-253">Meghatározza, hogy korlátozza az viselkedés kérelmek aránya le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="756df-253">Specifies that request rate limiting behavior is disabled.</span></span></td>
  <td><span data-ttu-id="756df-254">CosmosDB.Emulator.exe /DisableRateLimiting</span><span class="sxs-lookup"><span data-stu-id="756df-254">CosmosDB.Emulator.exe /DisableRateLimiting</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="756df-255">NoUI</span><span class="sxs-lookup"><span data-stu-id="756df-255">NoUI</span></span></td>
  <td><span data-ttu-id="756df-256">Ne jelenjen meg hello emulator felhasználói felületét.</span><span class="sxs-lookup"><span data-stu-id="756df-256">Do not show hello emulator user interface.</span></span></td>
  <td><span data-ttu-id="756df-257">CosmosDB.Emulator.exe/noui</span><span class="sxs-lookup"><span data-stu-id="756df-257">CosmosDB.Emulator.exe /NoUI</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="756df-258">NoExplorer</span><span class="sxs-lookup"><span data-stu-id="756df-258">NoExplorer</span></span></td>
  <td><span data-ttu-id="756df-259">Ne jelenjen meg indításkor dokumentumkezelő.</span><span class="sxs-lookup"><span data-stu-id="756df-259">Don't show document explorer on startup.</span></span></td>
  <td><span data-ttu-id="756df-260">CosmosDB.Emulator.exe /NoExplorer</span><span class="sxs-lookup"><span data-stu-id="756df-260">CosmosDB.Emulator.exe /NoExplorer</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="756df-261">PartitionCount</span><span class="sxs-lookup"><span data-stu-id="756df-261">PartitionCount</span></span></td>
  <td><span data-ttu-id="756df-262">A particionált gyűjtemények a hello maximális számát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="756df-262">Specifies hello maximum number of partitioned collections.</span></span> <span data-ttu-id="756df-263">Lásd: [gyűjtemények hello számának módosításához](#set-partitioncount) további információt.</span><span class="sxs-lookup"><span data-stu-id="756df-263">See [Change hello number of collections](#set-partitioncount) for more information.</span></span></td>
  <td><span data-ttu-id="756df-264">CosmosDB.Emulator.exe /PartitionCount =&lt;partitioncount&gt;</span><span class="sxs-lookup"><span data-stu-id="756df-264">CosmosDB.Emulator.exe /PartitionCount=&lt;partitioncount&gt;</span></span></td>
  <td><span data-ttu-id="756df-265">&lt;partitioncount&gt;: maximális száma az egypartíciós gyűjtemények engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="756df-265">&lt;partitioncount&gt;: Maximum number of allowed single partition collections.</span></span> <span data-ttu-id="756df-266">Alapértelmezett érték 25.</span><span class="sxs-lookup"><span data-stu-id="756df-266">Default is 25.</span></span> <span data-ttu-id="756df-267">Megengedett legnagyobb érték 250.</span><span class="sxs-lookup"><span data-stu-id="756df-267">Maximum allowed is 250.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="756df-268">DefaultPartitionCount</span><span class="sxs-lookup"><span data-stu-id="756df-268">DefaultPartitionCount</span></span></td>
  <td><span data-ttu-id="756df-269">A particionált gyűjtemény partíciók hello alapértelmezett számát adja meg.</span><span class="sxs-lookup"><span data-stu-id="756df-269">Specifies hello default number of partitions for a partitioned collection.</span></span></td>
  <td><span data-ttu-id="756df-270">CosmosDB.Emulator.exe /DefaultPartitionCount =&lt;defaultpartitioncount&gt;</span><span class="sxs-lookup"><span data-stu-id="756df-270">CosmosDB.Emulator.exe /DefaultPartitionCount=&lt;defaultpartitioncount&gt;</span></span></td>
  <td><span data-ttu-id="756df-271">&lt;defaultpartitioncount&gt; alapértelmezett érték 25.</span><span class="sxs-lookup"><span data-stu-id="756df-271">&lt;defaultpartitioncount&gt; Default is 25.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="756df-272">AllowNetworkAccess</span><span class="sxs-lookup"><span data-stu-id="756df-272">AllowNetworkAccess</span></span></td>
  <td><span data-ttu-id="756df-273">Lehetővé teszi, hogy toohello emulátor hozzáfér a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="756df-273">Enables access toohello emulator over a network.</span></span> <span data-ttu-id="756df-274">Is át kell /Key =&lt;key_string&gt; vagy /KeyFile =&lt;Fájlnév&gt; tooenable hálózati hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="756df-274">You must also pass /Key=&lt;key_string&gt; or /KeyFile=&lt;file_name&gt; tooenable network access.</span></span></td>
  <td><span data-ttu-id="756df-275">CosmosDB.Emulator.exe AllowNetworkAccess /Key =&lt;key_string&gt;</span><span class="sxs-lookup"><span data-stu-id="756df-275">CosmosDB.Emulator.exe /AllowNetworkAccess /Key=&lt;key_string&gt;</span></span><br><br><span data-ttu-id="756df-276">vagy</span><span class="sxs-lookup"><span data-stu-id="756df-276">or</span></span><br><br><span data-ttu-id="756df-277">CosmosDB.Emulator.exe /AllowNetworkAccess /KeyFile =&lt;fájlnév&gt;</span><span class="sxs-lookup"><span data-stu-id="756df-277">CosmosDB.Emulator.exe /AllowNetworkAccess /KeyFile=&lt;file_name&gt;</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="756df-278">NoFirewall</span><span class="sxs-lookup"><span data-stu-id="756df-278">NoFirewall</span></span></td>
  <td><span data-ttu-id="756df-279">Ne állítsa be úgy a tűzfalszabályok /AllowNetworkAccess használata esetén.</span><span class="sxs-lookup"><span data-stu-id="756df-279">Don't adjust firewall rules when /AllowNetworkAccess is used.</span></span></td>
  <td><span data-ttu-id="756df-280">CosmosDB.Emulator.exe /NoFirewall</span><span class="sxs-lookup"><span data-stu-id="756df-280">CosmosDB.Emulator.exe /NoFirewall</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="756df-281">GenKeyFile</span><span class="sxs-lookup"><span data-stu-id="756df-281">GenKeyFile</span></span></td>
  <td><span data-ttu-id="756df-282">Hozzon létre egy új hitelesítési kulcsot, és mentse toohello megadott fájlt.</span><span class="sxs-lookup"><span data-stu-id="756df-282">Generate a new authorization key and save toohello specified file.</span></span> <span data-ttu-id="756df-283">hello létrehozott kulcs hello /Key vagy /KeyFile lehetőséggel használható.</span><span class="sxs-lookup"><span data-stu-id="756df-283">hello generated key can be used with hello /Key or /KeyFile options.</span></span></td>
  <td><span data-ttu-id="756df-284">CosmosDB.Emulator.exe /GenKeyFile =&lt;tookey fájl elérési útja&gt;</span><span class="sxs-lookup"><span data-stu-id="756df-284">CosmosDB.Emulator.exe  /GenKeyFile=&lt;path tookey file&gt;</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="756df-285">Konzisztencia</span><span class="sxs-lookup"><span data-stu-id="756df-285">Consistency</span></span></td>
  <td><span data-ttu-id="756df-286">A fiók hello hello alapértelmezett konzisztencia szintjének beállítása.</span><span class="sxs-lookup"><span data-stu-id="756df-286">Set hello default consistency level for hello account.</span></span></td>
  <td><span data-ttu-id="756df-287">CosmosDB.Emulator.exe /Consistency =&lt;konzisztencia&gt;</span><span class="sxs-lookup"><span data-stu-id="756df-287">CosmosDB.Emulator.exe /Consistency=&lt;consistency&gt;</span></span></td>
  <td><span data-ttu-id="756df-288">&lt;konzisztencia&gt;: értéknek kell lennie a következő hello [konzisztenciaszintek](consistency-levels.md): munkamenet, erős, Eventual vagy BoundedStaleness.</span><span class="sxs-lookup"><span data-stu-id="756df-288">&lt;consistency&gt;: Value must be one of hello following [consistency levels](consistency-levels.md): Session, Strong, Eventual, or BoundedStaleness.</span></span>  <span data-ttu-id="756df-289">hello alapértelmezett értéke munkamenet.</span><span class="sxs-lookup"><span data-stu-id="756df-289">hello default value is Session.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="756df-290">?</span><span class="sxs-lookup"><span data-stu-id="756df-290">?</span></span></td>
  <td><span data-ttu-id="756df-291">Üdvözlőüzenetére Súgó megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="756df-291">Show hello help message.</span></span></td>
  <td></td>
  <td></td>
</tr>
</table>

## <a name="differences-between-hello-azure-cosmos-db-emulator-and-azure-cosmos-db"></a><span data-ttu-id="756df-292">Hello Azure Cosmos DB emulátor és Azure Cosmos DB közötti különbségek</span><span class="sxs-lookup"><span data-stu-id="756df-292">Differences between hello Azure Cosmos DB Emulator and Azure Cosmos DB</span></span> 
<span data-ttu-id="756df-293">Hello Azure Cosmos DB Emulator egy helyi fejlesztői munkaállomáson fut emulált környezetet biztosít, mert vannak eltérések a funkciók között hello emulátor és Azure Cosmos DB fiók hello felhőben:</span><span class="sxs-lookup"><span data-stu-id="756df-293">Because hello Azure Cosmos DB Emulator provides an emulated environment running on a local developer workstation, there are some differences in functionality between hello emulator and an Azure Cosmos DB account in hello cloud:</span></span>

* <span data-ttu-id="756df-294">hello Azure Cosmos DB emulátor csak egyetlen rögzített fiók és a jól ismert főkulcs támogatja.</span><span class="sxs-lookup"><span data-stu-id="756df-294">hello Azure Cosmos DB Emulator supports only a single fixed account and a well-known master key.</span></span>  <span data-ttu-id="756df-295">Kulcs újragenerálása erre nincs lehetőség hello Azure Cosmos DB emulátor.</span><span class="sxs-lookup"><span data-stu-id="756df-295">Key regeneration is not possible in hello Azure Cosmos DB Emulator.</span></span>
* <span data-ttu-id="756df-296">hello Azure Cosmos DB emulátor nem egy méretezhető szolgáltatás, és nem fogja támogatni a nagy mennyiségű gyűjteményhez.</span><span class="sxs-lookup"><span data-stu-id="756df-296">hello Azure Cosmos DB Emulator is not a scalable service and will not support a large number of collections.</span></span>
* <span data-ttu-id="756df-297">hello Azure Cosmos DB emulátor nem szimulálása különböző [Azure Cosmos DB konzisztenciaszintek](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="756df-297">hello Azure Cosmos DB Emulator does not simulate different [Azure Cosmos DB consistency levels](consistency-levels.md).</span></span>
* <span data-ttu-id="756df-298">hello Azure Cosmos DB emulátor nem szimulálása [több területi replikációs](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="756df-298">hello Azure Cosmos DB Emulator does not simulate [multi-region replication](distribute-data-globally.md).</span></span>
* <span data-ttu-id="756df-299">hello Azure Cosmos DB Emulator nem támogatja a hello szolgáltatás kvóta felülbírálások által biztosított hello Azure Cosmos DB szolgáltatás (pl. dokumentum méretkorlátait, nagyobb particionált gyűjtemény tároló).</span><span class="sxs-lookup"><span data-stu-id="756df-299">hello Azure Cosmos DB Emulator does not support hello service quota overrides that are available in hello Azure Cosmos DB service (e.g. document size limits, increased partitioned collection storage).</span></span>
* <span data-ttu-id="756df-300">Kérjük, mert hello Azure Cosmos DB emulátor a példány nem lehet másolatot toodate hello Azure Cosmos DB szolgáltatással hello legújabb módosításokkal, [Azure Cosmos DB kapacitás planner](https://www.documentdb.com/capacityplanner) tooaccurately becsült éles átviteli sebesség (RUs) az alkalmazás igényeinek.</span><span class="sxs-lookup"><span data-stu-id="756df-300">As your copy of hello Azure Cosmos DB Emulator might not be up toodate with hello most recent changes with hello Azure Cosmos DB service, please [Azure Cosmos DB capacity planner](https://www.documentdb.com/capacityplanner) tooaccurately estimate production throughput (RUs) needs of your application.</span></span>

## <span data-ttu-id="756df-301"><a id="set-partitioncount"></a>Gyűjtemények hello számának módosítása</span><span class="sxs-lookup"><span data-stu-id="756df-301"><a id="set-partitioncount"></a>Change hello number of collections</span></span>

<span data-ttu-id="756df-302">Alapértelmezés szerint too25 az egypartíciós gyűjtemények vagy 1 particionált gyűjtemény hello Azure Cosmos DB Emulator használatával hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="756df-302">By default, you can create up too25 single partition collections, or 1 partitioned collection using hello Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="756df-303">Hello módosításával **PartitionCount** érték, létrehozhat too250 az egypartíciós gyűjtemények vagy 10 particionált gyűjtemények, vagy tetszőleges kombinációja hello legfeljebb 250 egyetlen két partíciók (ahol 1 particionálva gyűjtemény = 25 egypartíciós gyűjtemény).</span><span class="sxs-lookup"><span data-stu-id="756df-303">By modifying hello **PartitionCount** value, you can create up too250 single partition collections or 10 partitioned collections, or any combination of hello two that does not exceed 250 single partitions (where 1 partitioned collection = 25 single partition collection).</span></span>

<span data-ttu-id="756df-304">Ha után hello aktuális partíciók száma túl lett lépve. Próbálja toocreate egy gyűjtemény, hello emulátor ServiceUnavailable kivételt, a következő üzenet hello okoz.</span><span class="sxs-lookup"><span data-stu-id="756df-304">If you attempt toocreate a collection after hello current partition count has been exceeded, hello emulator throws a ServiceUnavailable exception, with hello following message.</span></span>

    Sorry, we are currently experiencing high demand in this region, 
    and cannot fulfill your request at this time. We work continuously 
    toobring more and more capacity online, and encourage you tootry again. 
    Please do not hesitate tooemail docdbswat@microsoft.com at any time or 
    for any reason. ActivityId: 29da65cc-fba1-45f9-b82c-bf01d78a1f91

<span data-ttu-id="756df-305">gyűjtemények elérhető toohello Azure Cosmos DB emulátor toochange hello száma hello a következő:</span><span class="sxs-lookup"><span data-stu-id="756df-305">toochange hello number of collections available toohello Azure Cosmos DB Emulator, do hello following:</span></span>

1. <span data-ttu-id="756df-306">Kattintson a jobb gombbal a hello Azure Cosmos DB emulátor összes helyi adat törlése **Azure Cosmos DB emulátor** hello tálcán, majd kattintson az ikonra **adatok alaphelyzetbe állítása...** .</span><span class="sxs-lookup"><span data-stu-id="756df-306">Delete all local Azure Cosmos DB Emulator data by right-clicking hello **Azure Cosmos DB Emulator** icon on hello system tray, and then clicking **Reset Data…**.</span></span>
2. <span data-ttu-id="756df-307">Ez a mappa C:\Users\user_name\AppData\Local\CosmosDBEmulator összes emulátor adatok törlése.</span><span class="sxs-lookup"><span data-stu-id="756df-307">Delete all emulator data in this folder C:\Users\user_name\AppData\Local\CosmosDBEmulator.</span></span>
3. <span data-ttu-id="756df-308">Minden nyitott példányokat kilépéshez kattintson a jobb gombbal a hello **Azure Cosmos DB emulátor** hello tálcán, majd kattintson az ikonra **kilépési**.</span><span class="sxs-lookup"><span data-stu-id="756df-308">Exit all open instances by right-clicking hello **Azure Cosmos DB Emulator** icon on hello system tray, and then clicking **Exit**.</span></span> <span data-ttu-id="756df-309">Minden példány tooexit percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="756df-309">It may take a minute for all instances tooexit.</span></span>
4. <span data-ttu-id="756df-310">Hello hello legújabb verziójának telepítéséhez [Azure Cosmos DB emulátor](https://aka.ms/cosmosdb-emulator).</span><span class="sxs-lookup"><span data-stu-id="756df-310">Install hello latest version of hello [Azure Cosmos DB Emulator](https://aka.ms/cosmosdb-emulator).</span></span>
5. <span data-ttu-id="756df-311">Indítsa el a hello PartitionCount jelző hello emulátor egy < = 250.</span><span class="sxs-lookup"><span data-stu-id="756df-311">Launch hello emulator with hello PartitionCount flag by setting a value <= 250.</span></span> <span data-ttu-id="756df-312">Például: `C:\Program Files\Azure CosmosDB Emulator>CosmosDB.Emulator.exe /PartitionCount=100`.</span><span class="sxs-lookup"><span data-stu-id="756df-312">For example: `C:\Program Files\Azure CosmosDB Emulator>CosmosDB.Emulator.exe /PartitionCount=100`.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="756df-313">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="756df-313">Troubleshooting</span></span>

<span data-ttu-id="756df-314">Használja a következő toohelp elhárítása hello Azure Cosmos DB emulátorral tapasztal hello:</span><span class="sxs-lookup"><span data-stu-id="756df-314">Use hello following tips toohelp troubleshoot issues you encounter with hello Azure Cosmos DB emulator:</span></span>

- <span data-ttu-id="756df-315">Ha telepített hello emulátor új verziója, és hibákat tapasztal, győződjön meg arról, alaphelyzetbe állítja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="756df-315">If you installed a new version of hello Emulator and are experiencing errors, ensure you reset your data.</span></span> <span data-ttu-id="756df-316">Visszaállíthatja az adatokat, kattintson a jobb gombbal a hello Azure Cosmos DB emulátor ikonjára hello rendszerterületén, és kattintson az adatok alaphelyzetbe állítása...</span><span class="sxs-lookup"><span data-stu-id="756df-316">You can reset your data by right-clicking hello Azure Cosmos DB Emulator icon on hello system tray, and then clicking Reset Data….</span></span> <span data-ttu-id="756df-317">Ha ez nem segít hello hibák, távolítsa el, és telepítse újra az hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="756df-317">If that does not fix hello errors, you can uninstall and reinstall hello app.</span></span> <span data-ttu-id="756df-318">Lásd: [hello helyi emulátor eltávolítása](#uninstall) utasításokat.</span><span class="sxs-lookup"><span data-stu-id="756df-318">See [Uninstall hello local emulator](#uninstall) for instructions.</span></span>

- <span data-ttu-id="756df-319">Ha hello Azure Cosmos DB emulátor összeomlik, memóriaképek összegyűjtése c:\Users\user_name\AppData\Local\CrashDumps mappából, tömörítéssel és csatolja őket túl tooan e-mail[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="756df-319">If hello Azure Cosmos DB emulator crashes, collect dump files from c:\Users\user_name\AppData\Local\CrashDumps folder, compress them, and attach them tooan email too[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span></span>

- <span data-ttu-id="756df-320">Ha összeomlik a CosmosDB.StartupEntryPoint.exe, futtassa a következő parancsot egy rendszergazdai parancssorból hello:`lodctr /R`</span><span class="sxs-lookup"><span data-stu-id="756df-320">If you experience crashes in CosmosDB.StartupEntryPoint.exe, run hello following command from an admin command prompt: `lodctr /R`</span></span> 

- <span data-ttu-id="756df-321">Ha egy hálózati probléma, [nyomkövetési fájlok összegyűjtése](#trace-files), tömörítéssel, és csatolja őket túl tooan e-mail[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="756df-321">If you encounter a connectivity issue, [collect trace files](#trace-files), compress them, and attach them tooan email too[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span></span>

- <span data-ttu-id="756df-322">Ha megjelenik egy **szolgáltatás nem érhető el** üzenet, hello emulátor valószínűleg hibás tooinitialize hello hálózati vermet.</span><span class="sxs-lookup"><span data-stu-id="756df-322">If you receive a **Service Unavailable** message, hello emulator might be failing tooinitialize hello network stack.</span></span> <span data-ttu-id="756df-323">Ellenőrizze a toosee, ha nem rendelkezik hello Pulse secure ügyfél vagy Juniper hálózatok telepített, mint a hálózati fájlrendszerszűrő-illesztőprogramok hello problémát okozhatja.</span><span class="sxs-lookup"><span data-stu-id="756df-323">Check toosee if you have hello Pulse secure client or Juniper networks client installed, as their network filter drivers may cause hello problem.</span></span> <span data-ttu-id="756df-324">Harmadik féltől származó hálózati fájlrendszerszűrő-illesztőprogramok eltávolítása általában javít hello hibát.</span><span class="sxs-lookup"><span data-stu-id="756df-324">Uninstalling third party network filter drivers typically fixes hello issue.</span></span>

### <span data-ttu-id="756df-325"><a id="trace-files"></a>Nyomkövetési fájlok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="756df-325"><a id="trace-files"></a>Collect trace files</span></span>

<span data-ttu-id="756df-326">hibakeresési nyomkövetés, futtassa a következő parancsokat egy rendszergazdai parancssorból hello toocollect:</span><span class="sxs-lookup"><span data-stu-id="756df-326">toocollect debugging traces, run hello following commands from an administrative command prompt:</span></span>

1. `cd /d "%ProgramFiles%\Azure Cosmos DB Emulator"`
2. <span data-ttu-id="756df-327">`CosmosDB.Emulator.exe /shutdown`.</span><span class="sxs-lookup"><span data-stu-id="756df-327">`CosmosDB.Emulator.exe /shutdown`.</span></span> <span data-ttu-id="756df-328">A Watch hello rendszer tálca toomake meg arról, hogy hello program leállt, egy percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="756df-328">Watch hello system tray toomake sure hello program has shut down, it may take a minute.</span></span> <span data-ttu-id="756df-329">Csak is kattinthat **kilépési** hello Azure Cosmos DB emulator felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="756df-329">You can also just click **Exit** in hello Azure Cosmos DB emulator user interface.</span></span>
3. `CosmosDB.Emulator.exe /starttraces`
4. `CosmosDB.Emulator.exe`
5. <span data-ttu-id="756df-330">Hello probléma reprodukálásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="756df-330">Reproduce hello problem.</span></span> <span data-ttu-id="756df-331">Data Explorer nem működik, ha elegendő toowait hello böngésző tooopen néhány másodperc toocatch hello hibához.</span><span class="sxs-lookup"><span data-stu-id="756df-331">If Data Explorer is not working, you only need toowait for hello browser tooopen for a few seconds toocatch hello error.</span></span>
5. `CosmosDB.Emulator.exe /stoptraces`
6. <span data-ttu-id="756df-332">Keresse meg a túl`%ProgramFiles%\Azure Cosmos DB Emulator` hello docdbemulator_000001.etl fájlt.</span><span class="sxs-lookup"><span data-stu-id="756df-332">Navigate too`%ProgramFiles%\Azure Cosmos DB Emulator` and find hello docdbemulator_000001.etl file.</span></span>
7. <span data-ttu-id="756df-333">Hello .etl fájl együtt Reprodukálja lépéseket túl küldése[ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com) a hibakereséshez.</span><span class="sxs-lookup"><span data-stu-id="756df-333">Send hello .etl file along with repro steps too[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com) for debugging.</span></span>

### <span data-ttu-id="756df-334"><a id="uninstall"></a>Távolítsa el hello helyi emulátor</span><span class="sxs-lookup"><span data-stu-id="756df-334"><a id="uninstall"></a>Uninstall hello local Emulator</span></span>

1. <span data-ttu-id="756df-335">Hello minden nyitott példányai való kilépéshez kattintson a jobb gombbal a hello tálcán hello Azure Cosmos DB emulátor ikonjára, majd a kilépési helyi emulátor.</span><span class="sxs-lookup"><span data-stu-id="756df-335">Exit all open instances of hello local Emulator by right-clicking hello Azure Cosmos DB Emulator icon on hello system tray, and then clicking Exit.</span></span> <span data-ttu-id="756df-336">Minden példány tooexit percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="756df-336">It may take a minute for all instances tooexit.</span></span>
2. <span data-ttu-id="756df-337">Írja be a Windows hello keresőmezőbe, **alkalmazások és szolgáltatások** , majd kattintson a hello **alkalmazások és szolgáltatások (rendszerbeállítások)** eredménye.</span><span class="sxs-lookup"><span data-stu-id="756df-337">In hello Windows search box, type **Apps & features** and click on hello **Apps & features (System settings)** result.</span></span>
3. <span data-ttu-id="756df-338">Hello alkalmazások listájának megtekintéséhez görgessen túl**Azure Cosmos DB emulátor**, válassza ki azt, kattintson a **Eltávolítás**, majd erősítse meg, és kattintson a **Eltávolítás** újra.</span><span class="sxs-lookup"><span data-stu-id="756df-338">In hello list of apps, scroll too**Azure Cosmos DB Emulator**, select it, click **Uninstall**, then confirm and click **Uninstall** again.</span></span>
4. <span data-ttu-id="756df-339">Hello alkalmazás eltávolításakor, keresse meg a tooC:\Users\<felhasználó > \AppData\Local\CosmosDBEmulator és delete hello mappában.</span><span class="sxs-lookup"><span data-stu-id="756df-339">When hello app is uninstalled, navigate tooC:\Users\<user>\AppData\Local\CosmosDBEmulator and delete hello folder.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="756df-340">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="756df-340">Next steps</span></span>

<span data-ttu-id="756df-341">Ebben az oktatóanyagban hello következő régebben már kötöttek:</span><span class="sxs-lookup"><span data-stu-id="756df-341">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="756df-342">Telepített helyi emulátor hello</span><span class="sxs-lookup"><span data-stu-id="756df-342">Installed hello local Emulator</span></span>
> * <span data-ttu-id="756df-343">VÉL hello emulátor Docker a Windows rendszeren</span><span class="sxs-lookup"><span data-stu-id="756df-343">Rand hello Emulator on Docker for Windows</span></span>
> * <span data-ttu-id="756df-344">Hitelesített kéréseknél</span><span class="sxs-lookup"><span data-stu-id="756df-344">Authenticated requests</span></span>
> * <span data-ttu-id="756df-345">Az emulátor hello adatkezelő hello használt</span><span class="sxs-lookup"><span data-stu-id="756df-345">Used hello Data Explorer in hello Emulator</span></span>
> * <span data-ttu-id="756df-346">Exportált SSL-tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="756df-346">Exported SSL certificates</span></span>
> * <span data-ttu-id="756df-347">Hello emulátor nevű hello parancssorból</span><span class="sxs-lookup"><span data-stu-id="756df-347">Called hello Emulator from hello command line</span></span>
> * <span data-ttu-id="756df-348">Összegyűjtött nyomkövetési fájlokat</span><span class="sxs-lookup"><span data-stu-id="756df-348">Collected trace files</span></span>

<span data-ttu-id="756df-349">Ebben az oktatóanyagban korábban megtanulta, hogyan toouse hello szabad helyi fejlesztési helyi emulátor.</span><span class="sxs-lookup"><span data-stu-id="756df-349">In this tutorial, you've learned how toouse hello local Emulator for free local development.</span></span> <span data-ttu-id="756df-350">Ezután folytassa a következő oktatóanyag toohello és megtudhatja, hogyan tooexport emulátor SSL-tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="756df-350">You can now proceed toohello next tutorial and learn how tooexport Emulator SSL certificates.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="756df-351">Hello Azure Cosmos DB emulátor tanúsítványok exportálása</span><span class="sxs-lookup"><span data-stu-id="756df-351">Export hello Azure Cosmos DB Emulator certificates</span></span>](local-emulator-export-ssl-certificates.md)
