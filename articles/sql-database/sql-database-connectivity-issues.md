---
title: "aaaFix egy SQL-kapcsolati hiba átmeneti hiba |} Microsoft Docs"
description: "Ismerje meg, hogyan tootroubleshoot, diagnosztizálása és megelőzése egy SQL-kapcsolati hiba vagy az Azure SQL Database átmeneti hiba. "
keywords: "SQL-kapcsolatot, kapcsolati karakterláncot, problémák, átmeneti hiba, csatlakozási hiba"
services: sql-database
documentationcenter: 
author: dalechen
manager: cshepard
editor: 
ms.assetid: efb35451-3fed-4264-bf86-72b350f67d50
ms.service: sql-database
ms.custom: develop apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: daleche
ms.openlocfilehash: d225e610b9e88170ab53ca16d615bd07220603cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a><span data-ttu-id="654a3-104">SQL-csatlakozási hibák és átmeneti hibák elhárítása, diagnosztizálása és elkerülése az SQL Database szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="654a3-104">Troubleshoot, diagnose, and prevent SQL connection errors and transient errors for SQL Database</span></span>
<span data-ttu-id="654a3-105">Ez a cikk ismerteti, hogyan tooprevent, hibáinak elhárítása, diagnosztizálása és csatlakozási hibáinak és, hogy az ügyfélalkalmazás tapasztal, amikor hatással van az Azure SQL Database átmeneti hibák elhárítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="654a3-105">This article describes how tooprevent, troubleshoot, diagnose, and mitigate connection errors and transient errors that your client application encounters when it interacts with Azure SQL Database.</span></span> <span data-ttu-id="654a3-106">Ismerje meg, hogyan tooconfigure újrapróbálkozási logika, hello kapcsolati karakterlánc felépítéséhez és további kapcsolati beállításokat.</span><span class="sxs-lookup"><span data-stu-id="654a3-106">Learn how tooconfigure retry logic, build hello connection string, and adjust other connection settings.</span></span>

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a><span data-ttu-id="654a3-107">Átmeneti hibák átmeneti</span><span class="sxs-lookup"><span data-stu-id="654a3-107">Transient errors (transient faults)</span></span>
<span data-ttu-id="654a3-108">Egy átmeneti hiba - is, átmeneti hiba - rendelkezik egy alapul szolgáló ok, amely hamarosan magától megoldódik.</span><span class="sxs-lookup"><span data-stu-id="654a3-108">A transient error - also, transient fault - has an underlying cause that will soon resolve itself.</span></span> <span data-ttu-id="654a3-109">Az alkalmi átmeneti hibák oka amikor hello Azure rendszer gyorsan átvált hardveres erőforrások toobetter terheléselosztásához különböző munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="654a3-109">An occasional cause of transient errors is when hello Azure system quickly shifts hardware resources toobetter load-balance various workloads.</span></span> <span data-ttu-id="654a3-110">A legtöbb gyakran teljes 60 másodpercnél kevesebb újrakonfigurálását eseményekből.</span><span class="sxs-lookup"><span data-stu-id="654a3-110">Most of these reconfiguration events often complete in less than 60 seconds.</span></span> <span data-ttu-id="654a3-111">Az újrakonfigurálás időtartam során előfordulhat, hogy kapcsolódási problémák tooAzure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="654a3-111">During this reconfiguration time span, you may have connectivity issues tooAzure SQL Database.</span></span> <span data-ttu-id="654a3-112">Csatlakozás SQL Database kell tooAzure alkalmazások beépített tooexpect ezek átmeneti hibák őket implementálásával ismételje meg a kód helyett, az alkalmazáshibák toousers felszínre hozza a logikai leíró.</span><span class="sxs-lookup"><span data-stu-id="654a3-112">Applications connecting tooAzure SQL Database should be built tooexpect these transient errors, handle them by implementing retry logic in their code instead of surfacing them toousers as application errors.</span></span>

<span data-ttu-id="654a3-113">Ha az ügyfélprogram ADO.NET használ, a program van a kevésbé hello átmeneti hiba hello throw a egy **SqlException**.</span><span class="sxs-lookup"><span data-stu-id="654a3-113">If your client program is using ADO.NET, your program is told about hello transient error by hello throw of an **SqlException**.</span></span> <span data-ttu-id="654a3-114">Hello **szám** tulajdonság hasonlítható hello témakör hello tetején átmeneti hibák hello listája alapján: [SQL Database-ügyfélalkalmazások SQL hibakódok](sql-database-develop-error-messages.md).</span><span class="sxs-lookup"><span data-stu-id="654a3-114">hello **Number** property can be compared against hello list of transient errors near hello top of hello topic: [SQL error codes for SQL Database client applications](sql-database-develop-error-messages.md).</span></span>

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a><span data-ttu-id="654a3-115">Kapcsolat és a parancs</span><span class="sxs-lookup"><span data-stu-id="654a3-115">Connection versus command</span></span>
<span data-ttu-id="654a3-116">Lesz hello SQL-kapcsolatot. Próbálkozzon újra, vagy létre újra, attól függően, hogy a következő hello:</span><span class="sxs-lookup"><span data-stu-id="654a3-116">You'll retry hello SQL connection or establish it again, depending on hello following:</span></span>

* <span data-ttu-id="654a3-117">**Egy átmeneti hiba akkor fordul elő, a kapcsolódási kísérlet során**: hello kapcsolat meg kell ismételni után késleltetése néhány másodpercig.</span><span class="sxs-lookup"><span data-stu-id="654a3-117">**A transient error occurs during a connection try**: hello connection should be retried after delaying for several seconds.</span></span>
* <span data-ttu-id="654a3-118">**Egy átmeneti hiba jelentkezik egy SQL-lekérdezés parancs közben**: hello parancsot kell nem azonnal kísérli meg újra.</span><span class="sxs-lookup"><span data-stu-id="654a3-118">**A transient error occurs during a SQL query command**: hello command should not be immediately retried.</span></span> <span data-ttu-id="654a3-119">Ehelyett késleltetés után hello kell frissen létesíthető kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="654a3-119">Instead, after a delay, hello connection should be freshly established.</span></span> <span data-ttu-id="654a3-120">Majd hello parancs követően újra.</span><span class="sxs-lookup"><span data-stu-id="654a3-120">Then hello command can be retried.</span></span>

<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

### <a name="retry-logic-for-transient-errors"></a><span data-ttu-id="654a3-121">Újrapróbálkozási logika átmeneti hibák esetén</span><span class="sxs-lookup"><span data-stu-id="654a3-121">Retry logic for transient errors</span></span>
<span data-ttu-id="654a3-122">Ügyfél programok, amelyek találkozik, egy átmeneti hiba is robusztusabb, ha újrapróbálkozási logika tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="654a3-122">Client programs that occasionally encounter a transient error are more robust when they contain retry logic.</span></span>

<span data-ttu-id="654a3-123">Amikor a program a 3. fél köztes keresztül kommunikál az Azure SQL Database, lekérdezése hello gyártójánál, hogy hello köztes újrapróbálkozási logika átmeneti hibák tartalmaz-e.</span><span class="sxs-lookup"><span data-stu-id="654a3-123">When your program communicates with Azure SQL Database through a 3rd party middleware, inquire with hello vendor whether hello middleware contains retry logic for transient errors.</span></span>

<a id="principles-for-retry" name="principles-for-retry"></a>

#### <a name="principles-for-retry"></a><span data-ttu-id="654a3-124">Az újrapróbálkozási alapelvei</span><span class="sxs-lookup"><span data-stu-id="654a3-124">Principles for retry</span></span>
* <span data-ttu-id="654a3-125">Egy kísérlet tooopen kapcsolatot meg kell ismételni, átmeneti hello hiba esetén.</span><span class="sxs-lookup"><span data-stu-id="654a3-125">An attempt tooopen a connection should be retried if hello error is transient.</span></span>
* <span data-ttu-id="654a3-126">Olyan SQL SELECT utasításban, amely egy átmeneti hiba miatt sikertelen nem közvetlenül végrehajtásával lehet újrapróbálkozni.</span><span class="sxs-lookup"><span data-stu-id="654a3-126">A SQL SELECT statement that fails with a transient error should not be retried directly.</span></span>
  
  * <span data-ttu-id="654a3-127">Ehelyett a friss kapcsolatot, majd próbálkozzon újra az hello VÁLASSZA.</span><span class="sxs-lookup"><span data-stu-id="654a3-127">Instead, establish a fresh connection, and then retry hello SELECT.</span></span>
* <span data-ttu-id="654a3-128">Ha egy frissítés az SQL-utasítás egy átmeneti hiba miatt nem sikerül, egy friss kell kapcsolatot frissítés a rendszer ismét megkísérli hello előtt.</span><span class="sxs-lookup"><span data-stu-id="654a3-128">When a SQL UPDATE statement fails with a transient error, a fresh connection should be established before hello UPDATE is retried.</span></span>
  
  * <span data-ttu-id="654a3-129">hello újrapróbálkozási logika győződjön meg arról, hogy a hello teljes adatbázis tranzakció befejeződött, vagy adott hello teljes tranzakció vissza lesz állítva.</span><span class="sxs-lookup"><span data-stu-id="654a3-129">hello retry logic must ensure that either hello entire database transaction completed, or that hello entire transaction is rolled back.</span></span>

#### <a name="other-considerations-for-retry"></a><span data-ttu-id="654a3-130">Újrapróbálkozási egyéb szempontjai</span><span class="sxs-lookup"><span data-stu-id="654a3-130">Other considerations for retry</span></span>
* <span data-ttu-id="654a3-131">Egy parancsfájlban, amely automatikusan elindul a munkaidőn, és amely reggel, mielőtt befejezi az újrapróbálkozások között hosszú idő időközökkel toovery türelmet is biztosít.</span><span class="sxs-lookup"><span data-stu-id="654a3-131">A batch program that is automatically started after work hours, and which will complete before morning, can afford toovery patient with long time intervals between its retry attempts.</span></span>
* <span data-ttu-id="654a3-132">A felhasználói felület program kell figyelembe hello emberi legtöbben volna toogive mentése túl hosszú várakozás után.</span><span class="sxs-lookup"><span data-stu-id="654a3-132">A user interface program should account for hello human tendency toogive up after too long a wait.</span></span>
  
  * <span data-ttu-id="654a3-133">Azonban hello megoldást nem lehet tooretry minden néhány másodpercben, mert az adott házirendnek is kéréssekkel hello rendszer kérések.</span><span class="sxs-lookup"><span data-stu-id="654a3-133">However, hello solution must not be tooretry every few seconds, because that policy can flood hello system with requests.</span></span>

#### <a name="interval-increase-between-retries"></a><span data-ttu-id="654a3-134">Az újrapróbálkozások közötti időköz növekedése</span><span class="sxs-lookup"><span data-stu-id="654a3-134">Interval increase between retries</span></span>
<span data-ttu-id="654a3-135">Azt javasoljuk, hogy addig elhalasztani, az 5 másodperc az első újrapróbálkozás előtti.</span><span class="sxs-lookup"><span data-stu-id="654a3-135">We recommend that you delay for 5 seconds before your first retry.</span></span> <span data-ttu-id="654a3-136">Újrapróbálkozás rövidebb, mint 5 másodperces várakozás után kockázatok túlságosan hello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="654a3-136">Retrying after a delay shorter than 5 seconds risks overwhelming hello cloud service.</span></span> <span data-ttu-id="654a3-137">Minden ezt követő újrapróbálkozásra hello késleltetés kell nő exponenciálisan növekszik, mentése tooa legfeljebb 60 másodperc.</span><span class="sxs-lookup"><span data-stu-id="654a3-137">For each subsequent retry hello delay should grow exponentially, up tooa maximum of 60 seconds.</span></span>

<span data-ttu-id="654a3-138">Hello tárgyalja *blokkolási időtartam* ADO.NET használó ügyfelek számára érhető el [SQL Server készletezését (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).</span><span class="sxs-lookup"><span data-stu-id="654a3-138">A discussion of hello *blocking period* for clients that use ADO.NET is available in [SQL Server Connection Pooling (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).</span></span>

<span data-ttu-id="654a3-139">Is érdemes tooset az újrapróbálkozások maximális számát, mielőtt hello program önálló leáll.</span><span class="sxs-lookup"><span data-stu-id="654a3-139">You might also want tooset a maximum number of retries before hello program self-terminates.</span></span>

#### <a name="code-samples-with-retry-logic"></a><span data-ttu-id="654a3-140">Az újrapróbálkozási logika mintakódok</span><span class="sxs-lookup"><span data-stu-id="654a3-140">Code samples with retry logic</span></span>
<span data-ttu-id="654a3-141">Az újrapróbálkozási logika, a különböző programnyelveken, Kódminták webhelyen érhetők el:</span><span class="sxs-lookup"><span data-stu-id="654a3-141">Code samples with retry logic, in a variety of programming languages, are available at:</span></span>

* [<span data-ttu-id="654a3-142">Adatkapcsolattárak SQL Database és SQL Server</span><span class="sxs-lookup"><span data-stu-id="654a3-142">Connection libraries for SQL Database and SQL Server</span></span>](sql-database-libraries.md)

<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

#### <a name="test-your-retry-logic"></a><span data-ttu-id="654a3-143">Az újrapróbálkozási logika tesztelése</span><span class="sxs-lookup"><span data-stu-id="654a3-143">Test your retry logic</span></span>
<span data-ttu-id="654a3-144">tootest az újrapróbálkozási logika kell szimulálása vagy hibát okoz, mint javítható, a program futása közben.</span><span class="sxs-lookup"><span data-stu-id="654a3-144">tootest your retry logic, you must simulate or cause an error than can be corrected while your program is still running.</span></span>

##### <a name="test-by-disconnecting-from-hello-network"></a><span data-ttu-id="654a3-145">Tesztelje hello hálózati kapcsolat bontása</span><span class="sxs-lookup"><span data-stu-id="654a3-145">Test by disconnecting from hello network</span></span>
<span data-ttu-id="654a3-146">Az újrapróbálkozási logika tesztelheti egyike toodisconnect az ügyfélszámítógépen, a hello hálózati hello program futása közben.</span><span class="sxs-lookup"><span data-stu-id="654a3-146">One way you can test your retry logic is toodisconnect your client computer from hello network while hello program is running.</span></span> <span data-ttu-id="654a3-147">hello hiba a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="654a3-147">hello error will be:</span></span>

* <span data-ttu-id="654a3-148">**SqlException.Number** = 11001</span><span class="sxs-lookup"><span data-stu-id="654a3-148">**SqlException.Number** = 11001</span></span>
* <span data-ttu-id="654a3-149">Üzenet: "ilyen állomás nem található"</span><span class="sxs-lookup"><span data-stu-id="654a3-149">Message: "No such host is known"</span></span>

<span data-ttu-id="654a3-150">Hello részét először próbálkozzon újra a kísérlet, mert a program is javítsa hello helyesírási, és próbálja meg tooconnect.</span><span class="sxs-lookup"><span data-stu-id="654a3-150">As part of hello first retry attempt, your program can correct hello misspelling, and then attempt tooconnect.</span></span>

<span data-ttu-id="654a3-151">a gyakorlati toomake, akkor eltávolíthatja a számítógép hello hálózatról a program indítása előtt.</span><span class="sxs-lookup"><span data-stu-id="654a3-151">toomake this practical, you unplug your computer from hello network before you start your program.</span></span> <span data-ttu-id="654a3-152">Majd a program felismeri a futási idő paraméter, ami miatt hello program:</span><span class="sxs-lookup"><span data-stu-id="654a3-152">Then your program recognizes a run time parameter that causes hello program to:</span></span>

1. <span data-ttu-id="654a3-153">Ideiglenesen hozzáadni átmeneti jellegű hibák tooconsider 11001 tooits listája.</span><span class="sxs-lookup"><span data-stu-id="654a3-153">Temporarily add 11001 tooits list of errors tooconsider as transient.</span></span>
2. <span data-ttu-id="654a3-154">Az első csatlakozás a megszokott módon történt kísérlet.</span><span class="sxs-lookup"><span data-stu-id="654a3-154">Attempt its first connection as usual.</span></span>
3. <span data-ttu-id="654a3-155">Hello után hiba lépett fel, távolítsa el a 11001 hello listáról.</span><span class="sxs-lookup"><span data-stu-id="654a3-155">After hello error is caught, remove 11001 from hello list.</span></span>
4. <span data-ttu-id="654a3-156">Hello hálózatra szólítja fel a hello felhasználói tooplug hello számítógép egy üzenetet jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="654a3-156">Display a message telling hello user tooplug hello computer into hello network.</span></span>
   * <span data-ttu-id="654a3-157">További végrehajtásának felfüggesztése vagy hello segítségével **Console.ReadLine** metódus vagy egy párbeszédpanelen az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="654a3-157">Pause further execution by using either hello **Console.ReadLine** method or a dialog with an OK button.</span></span> <span data-ttu-id="654a3-158">hello felhasználó megnyomja hello Enter billentyűt, miután hello számítógép hello hálózathoz csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="654a3-158">hello user presses hello Enter key after hello computer plugged into hello network.</span></span>
5. <span data-ttu-id="654a3-159">Próbálja megismételni az tooconnect, a program sikeres vár.</span><span class="sxs-lookup"><span data-stu-id="654a3-159">Attempt again tooconnect, expecting success.</span></span>

##### <a name="test-by-misspelling-hello-database-name-when-connecting"></a><span data-ttu-id="654a3-160">Kapcsolódás esetén helyesírási hello adatbázisnév tesztelése</span><span class="sxs-lookup"><span data-stu-id="654a3-160">Test by misspelling hello database name when connecting</span></span>
<span data-ttu-id="654a3-161">A program szándékosan is hibásan hello felhasználónév hello első kapcsolódási kísérlet előtt.</span><span class="sxs-lookup"><span data-stu-id="654a3-161">Your program can purposely misspell hello user name before hello first connection attempt.</span></span> <span data-ttu-id="654a3-162">hello hiba a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="654a3-162">hello error will be:</span></span>

* <span data-ttu-id="654a3-163">**SqlException.Number** = 18456</span><span class="sxs-lookup"><span data-stu-id="654a3-163">**SqlException.Number** = 18456</span></span>
* <span data-ttu-id="654a3-164">Üzenet: "bejelentkezés sikertelen volt a felhasználói"WRONG_MyUserName"."</span><span class="sxs-lookup"><span data-stu-id="654a3-164">Message: "Login failed for user 'WRONG_MyUserName'."</span></span>

<span data-ttu-id="654a3-165">Hello részét először próbálkozzon újra a kísérlet, mert a program is javítsa hello helyesírási, és próbálja meg tooconnect.</span><span class="sxs-lookup"><span data-stu-id="654a3-165">As part of hello first retry attempt, your program can correct hello misspelling, and then attempt tooconnect.</span></span>

<span data-ttu-id="654a3-166">a gyakorlati toomake, a program sikerült ismeri fel a futási idő paraméter, ami miatt hello program:</span><span class="sxs-lookup"><span data-stu-id="654a3-166">toomake this practical, your program could recognize a run time parameter that causes hello program to:</span></span>

1. <span data-ttu-id="654a3-167">Ideiglenesen hozzáadni átmeneti jellegű hibák tooconsider 18456 tooits listája.</span><span class="sxs-lookup"><span data-stu-id="654a3-167">Temporarily add 18456 tooits list of errors tooconsider as transient.</span></span>
2. <span data-ttu-id="654a3-168">Szándékosan hozzáadása "WRONG_" toohello felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="654a3-168">Purposely add 'WRONG_' toohello user name.</span></span>
3. <span data-ttu-id="654a3-169">Hello után hiba lépett fel, távolítsa el a 18456 hello listáról.</span><span class="sxs-lookup"><span data-stu-id="654a3-169">After hello error is caught, remove 18456 from hello list.</span></span>
4. <span data-ttu-id="654a3-170">Távolítsa el a "WRONG_" hello felhasználónévben.</span><span class="sxs-lookup"><span data-stu-id="654a3-170">Remove 'WRONG_' from hello user name.</span></span>
5. <span data-ttu-id="654a3-171">Próbálja megismételni az tooconnect, a program sikeres vár.</span><span class="sxs-lookup"><span data-stu-id="654a3-171">Attempt again tooconnect, expecting success.</span></span>

<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

### <a name="net-sqlconnection-parameters-for-connection-retry"></a><span data-ttu-id="654a3-172">Újrapróbálkozási .NET SqlConnection paramétereinek</span><span class="sxs-lookup"><span data-stu-id="654a3-172">.NET SqlConnection parameters for connection retry</span></span>
<span data-ttu-id="654a3-173">Ha az ügyfélprogram tootooAzure SQL-adatbázis hello .NET-keretrendszer osztály használatával **System.Data.SqlClient.SqlConnection**, használjon .NET 4.6.1 vagy újabb (vagy a .NET Core), a kapcsolat újrapróbálkozási funkcióját.</span><span class="sxs-lookup"><span data-stu-id="654a3-173">If your client program connects tootooAzure SQL Database by using hello .NET Framework class **System.Data.SqlClient.SqlConnection**, you should use .NET 4.6.1 or later (or .NET Core) so you can leverage its connection retry feature.</span></span> <span data-ttu-id="654a3-174">Hello szolgáltatás részletei [Itt](http://go.microsoft.com/fwlink/?linkid=393996).</span><span class="sxs-lookup"><span data-stu-id="654a3-174">Details of hello feature are [here](http://go.microsoft.com/fwlink/?linkid=393996).</span></span>

<!--
2015-11-30, FwLink 393996 points toodn632678.aspx, which links tooa downloadable .docx related tooSqlClient and SQL Server 2014.
-->


<span data-ttu-id="654a3-175">Hello összeállításakor [kapcsolati karakterlánc](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) a a **SqlConnection** objektum hello értékek között a következő paraméterek hello kell koordinálni:</span><span class="sxs-lookup"><span data-stu-id="654a3-175">When you build hello [connection string](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) for your **SqlConnection** object, you should coordinate hello values among hello following parameters:</span></span>

* <span data-ttu-id="654a3-176">ConnectRetryCount &nbsp; &nbsp; *(alapértelmezett érték 1. Tartománya 0 és 255 között.)*</span><span class="sxs-lookup"><span data-stu-id="654a3-176">ConnectRetryCount &nbsp;&nbsp;*(Default is 1. Range is 0 through 255.)*</span></span>
* <span data-ttu-id="654a3-177">A ConnectRetryInterval &nbsp; &nbsp; *(alapértelmezett értéke 1 másodperc. Tartomány: 1 – 60.)*</span><span class="sxs-lookup"><span data-stu-id="654a3-177">ConnectRetryInterval &nbsp;&nbsp;*(Default is 1 second. Range is 1 through 60.)*</span></span>
* <span data-ttu-id="654a3-178">Kapcsolat időtúllépése &nbsp; &nbsp; *(alapértelmezett érték 15 másodpercre. A tartománya 0 és 2147483647 között)*</span><span class="sxs-lookup"><span data-stu-id="654a3-178">Connection Timeout &nbsp;&nbsp;*(Default is 15 seconds. Range is 0 through 2147483647)*</span></span>

<span data-ttu-id="654a3-179">Pontosabban a kiválasztott értékek győződjön hello egyenlőség igaz a következő:</span><span class="sxs-lookup"><span data-stu-id="654a3-179">Specifically, your chosen values should make hello following equality true:</span></span>

* <span data-ttu-id="654a3-180">Kapcsolat időtúllépése = ConnectRetryCount * ConnectionRetryInterval</span><span class="sxs-lookup"><span data-stu-id="654a3-180">Connection Timeout = ConnectRetryCount * ConnectionRetryInterval</span></span>

<span data-ttu-id="654a3-181">Például ha hello száma = 3, és időköz = 10 másodperc, a időtúllépés csak 29 másodperc nem elég adna hello rendszer elegendő időt a 3. és végső újrapróbálkozási: Kapcsolódás: 29 < 3 * 10.</span><span class="sxs-lookup"><span data-stu-id="654a3-181">For example, if hello count = 3, and interval = 10 seconds, a timeout of only 29 seconds would not quite give hello system enough time for its 3rd and final retry at connecting: 29 < 3 * 10.</span></span>

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a><span data-ttu-id="654a3-182">Kapcsolat és a parancs</span><span class="sxs-lookup"><span data-stu-id="654a3-182">Connection versus command</span></span>
<span data-ttu-id="654a3-183">Hello **ConnectRetryCount** és **ConnectRetryInterval** paraméterek lehetővé teszik a **SqlConnection** objektum újrapróbálkozási hello kapcsolódási műveletet szólítja fel vagy bothering nélkül a program, például a vezérlő tooyour program ad vissza.</span><span class="sxs-lookup"><span data-stu-id="654a3-183">hello **ConnectRetryCount** and **ConnectRetryInterval** parameters let your **SqlConnection** object retry hello connect operation without telling or bothering your program, such as returning control tooyour program.</span></span> <span data-ttu-id="654a3-184">hello újrapróbálkozások hello a következő helyzetekben fordul elő:</span><span class="sxs-lookup"><span data-stu-id="654a3-184">hello retries can occur in hello following situations:</span></span>

* <span data-ttu-id="654a3-185">mySqlConnection.Open metódus hívása</span><span class="sxs-lookup"><span data-stu-id="654a3-185">mySqlConnection.Open method call</span></span>
* <span data-ttu-id="654a3-186">mySqlConnection.Execute metódus hívása</span><span class="sxs-lookup"><span data-stu-id="654a3-186">mySqlConnection.Execute method call</span></span>

<span data-ttu-id="654a3-187">Nincs olyan subtlety.</span><span class="sxs-lookup"><span data-stu-id="654a3-187">There is a subtlety.</span></span> <span data-ttu-id="654a3-188">Egy átmeneti hiba akkor fordul elő, ha közben az *lekérdezés* végrehajtott, a **SqlConnection** objektum nem nem az újrapróbálkozási hello kapcsolódási műveletet, és hogy biztosan nem próbálja meg újra a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="654a3-188">If a transient error occurs while your *query* is being executed, your **SqlConnection** object does not retry hello connect operation, and it certainly does not retry your query.</span></span> <span data-ttu-id="654a3-189">Azonban **SqlConnection** nagyon gyorsan ellenőrzések hello kapcsolat végrehajtásra a lekérdezés elküldése előtt.</span><span class="sxs-lookup"><span data-stu-id="654a3-189">However, **SqlConnection** very quickly checks hello connection before sending your query for execution.</span></span> <span data-ttu-id="654a3-190">Ha a Gyorsellenőrzés hello észleli a kapcsolat hibája **SqlConnection** újrapróbálkozások hello kapcsolódási műveletet.</span><span class="sxs-lookup"><span data-stu-id="654a3-190">If hello quick check detects a connection problem, **SqlConnection** retries hello connect operation.</span></span> <span data-ttu-id="654a3-191">Ha hello újrapróbálkozási sikeres, akkor lekérdezést továbbítja a végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="654a3-191">If hello retry succeeds, you query is sent for execution.</span></span>

#### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a><span data-ttu-id="654a3-192">Kombinálható ConnectRetryCount alkalmazás újrapróbálkozási logika?</span><span class="sxs-lookup"><span data-stu-id="654a3-192">Should ConnectRetryCount be combined with application retry logic?</span></span>
<span data-ttu-id="654a3-193">Tegyük fel, hogy az alkalmazás rendelkezik robusztus egyéni újrapróbálkozási logika.</span><span class="sxs-lookup"><span data-stu-id="654a3-193">Suppose your application has robust custom retry logic.</span></span> <span data-ttu-id="654a3-194">Hello újra kapcsolódási műveletet. 4 alkalommal.</span><span class="sxs-lookup"><span data-stu-id="654a3-194">It might retry hello connect operation 4 times.</span></span> <span data-ttu-id="654a3-195">Ha ad hozzá **ConnectRetryInterval** és **ConnectRetryCount** = 3 tooyour kapcsolati karakterláncot, megnöveli a hello újrapróbálkozási számláló too4 * 3 = 12 újrapróbálkozik.</span><span class="sxs-lookup"><span data-stu-id="654a3-195">If you add **ConnectRetryInterval** and **ConnectRetryCount** =3 tooyour connection string, you will increase hello retry count too4 * 3 = 12 retries.</span></span> <span data-ttu-id="654a3-196">Előfordulhat, hogy nem kíván ilyen újrapróbálkozások magas száma.</span><span class="sxs-lookup"><span data-stu-id="654a3-196">You might not intend such a high number of retries.</span></span>

<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-tooazure-sql-database"></a><span data-ttu-id="654a3-197">Kapcsolatok tooAzure SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="654a3-197">Connections tooAzure SQL Database</span></span>
<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a><span data-ttu-id="654a3-198">Kapcsolat: Kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="654a3-198">Connection: Connection string</span></span>
<span data-ttu-id="654a3-199">hello kapcsolati tooAzure csatlakoztatásához szükséges SQL-adatbázis karakterlánca némileg eltérő csatlakozni az SQL Server tooMicrosoft hello karakterláncból.</span><span class="sxs-lookup"><span data-stu-id="654a3-199">hello connection string necessary for connecting tooAzure SQL Database is slightly different from hello string for connecting tooMicrosoft SQL Server.</span></span> <span data-ttu-id="654a3-200">Másolhatja az adatbázis hello kapcsolati karakterlánc hello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="654a3-200">You can copy hello connection string for your database from hello [Azure portal](https://portal.azure.com/).</span></span>

[!INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]

<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a><span data-ttu-id="654a3-201">Kapcsolat: IP-cím</span><span class="sxs-lookup"><span data-stu-id="654a3-201">Connection: IP address</span></span>
<span data-ttu-id="654a3-202">Konfigurálnia kell a hello adatbázis SQL server tooaccept kommunikációt hello IP-címről érkező hello számítógép, amelyen az ügyfélprogram.</span><span class="sxs-lookup"><span data-stu-id="654a3-202">You must configure hello SQL Database server tooaccept communication from hello IP address of hello computer that hosts your client program.</span></span> <span data-ttu-id="654a3-203">Tűzfalbeállítások hello keresztül hello szerkesztésével ehhez [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="654a3-203">You do this by editing hello firewall settings through hello [Azure portal](https://portal.azure.com/).</span></span>

<span data-ttu-id="654a3-204">Ha elfelejti tooconfigure hello IP-cím, a a program sikertelen lesz, és egy kényelmes hibaüzenet hello szükséges IP-címet.</span><span class="sxs-lookup"><span data-stu-id="654a3-204">If you forget tooconfigure hello IP address, your program will fail with a handy error message that states hello necessary IP address.</span></span>

[!INCLUDE [sql-database-include-ip-address-22-portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]

<span data-ttu-id="654a3-205">További információkért lásd: [hogyan: SQL Database tűzfalbeállításainak konfigurálása](sql-database-configure-firewall-settings.md)</span><span class="sxs-lookup"><span data-stu-id="654a3-205">For more information, see: [How to: Configure firewall settings on SQL Database](sql-database-configure-firewall-settings.md)</span></span>

<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a><span data-ttu-id="654a3-206">Kapcsolat: portok</span><span class="sxs-lookup"><span data-stu-id="654a3-206">Connection: Ports</span></span>
<span data-ttu-id="654a3-207">Általában csak akkor kell, hogy a 1433-as port kimenő kommunikációra hello üzemeltető számítógépen akkor ügyfélprogram nyitva-e tooensure.</span><span class="sxs-lookup"><span data-stu-id="654a3-207">Typically you only need tooensure that port 1433 is open for outbound communication, on hello computer that hosts you client program.</span></span>

<span data-ttu-id="654a3-208">Például az ügyfélprogram Windows-számítógép üzemelteti, a Windows tűzfal hello hello gazdagépen lehetővé teszi tooopen 1433-as port:</span><span class="sxs-lookup"><span data-stu-id="654a3-208">For example, when your client program is hosted on a Windows computer, hello Windows Firewall on hello host enables you tooopen port 1433:</span></span>

1. <span data-ttu-id="654a3-209">Nyissa meg a Vezérlőpult hello</span><span class="sxs-lookup"><span data-stu-id="654a3-209">Open hello Control Panel</span></span>
2. <span data-ttu-id="654a3-210">&gt;Minden Vezérlőpultelem</span><span class="sxs-lookup"><span data-stu-id="654a3-210">&gt; All Control Panel Items</span></span>
3. <span data-ttu-id="654a3-211">&gt;A Windows tűzfal</span><span class="sxs-lookup"><span data-stu-id="654a3-211">&gt; Windows Firewall</span></span>
4. <span data-ttu-id="654a3-212">&gt;Speciális beállítások</span><span class="sxs-lookup"><span data-stu-id="654a3-212">&gt; Advanced Settings</span></span>
5. <span data-ttu-id="654a3-213">&gt;Kimenő szabályok</span><span class="sxs-lookup"><span data-stu-id="654a3-213">&gt; Outbound Rules</span></span>
6. <span data-ttu-id="654a3-214">&gt;Műveletek</span><span class="sxs-lookup"><span data-stu-id="654a3-214">&gt; Actions</span></span>
7. <span data-ttu-id="654a3-215">&gt;Új szabály</span><span class="sxs-lookup"><span data-stu-id="654a3-215">&gt; New Rule</span></span>

<span data-ttu-id="654a3-216">Ha az ügyfélprogram egy Azure virtuális gép (VM) üzemelteti, olvassa el:</span><span class="sxs-lookup"><span data-stu-id="654a3-216">If your client program is hosted on an Azure virtual machine (VM), you should read:</span></span><br/><span data-ttu-id="654a3-217">[Az ADO.NET 4.5-ös és az SQL Database 1433 kívüli portokon](sql-database-develop-direct-route-ports-adonet-v12.md).</span><span class="sxs-lookup"><span data-stu-id="654a3-217">[Ports beyond 1433 for ADO.NET 4.5 and SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).</span></span>

<span data-ttu-id="654a3-218">Háttér-információkat a portok és IP-cím cofiguration, lásd: [Azure SQL Database-tűzfal](sql-database-firewall-configure.md)</span><span class="sxs-lookup"><span data-stu-id="654a3-218">For background information about cofiguration of ports and IP address, see: [Azure SQL Database firewall](sql-database-firewall-configure.md)</span></span>

<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a><span data-ttu-id="654a3-219">Kapcsolat: ADO.NET 4.6.1.</span><span class="sxs-lookup"><span data-stu-id="654a3-219">Connection: ADO.NET 4.6.1</span></span>
<span data-ttu-id="654a3-220">Ha a program használja az ADO.NET osztályok hasonló **System.Data.SqlClient.SqlConnection** tooconnect tooAzure SQL-adatbázis, azt javasoljuk, hogy használja-e a .NET-keretrendszer 4.6.1 verzió vagy újabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="654a3-220">If your program uses ADO.NET classes like **System.Data.SqlClient.SqlConnection** tooconnect tooAzure SQL Database, we recommend that you use .NET Framework version 4.6.1 or higher.</span></span>

<span data-ttu-id="654a3-221">ADO.NET 4.6.1:</span><span class="sxs-lookup"><span data-stu-id="654a3-221">ADO.NET 4.6.1:</span></span>

* <span data-ttu-id="654a3-222">Az Azure SQL Database esetén nincs továbbfejlesztett megbízhatósági kapcsolatot hello használatával való megnyitásakor **SqlConnection.Open** metódust.</span><span class="sxs-lookup"><span data-stu-id="654a3-222">For Azure SQL Database, there is improved reliability when you open a connection by using hello **SqlConnection.Open** method.</span></span> <span data-ttu-id="654a3-223">Hello **nyitott** metódus most magában foglalja a legjobb elérhető újrapróbálkozási mechanizmusok válasz tootransient hibaüzenetekben, bizonyos hibák hello kapcsolat időkorláton belül.</span><span class="sxs-lookup"><span data-stu-id="654a3-223">hello **Open** method now incorporates best effort retry mechanisms in response tootransient faults, for certain errors within hello Connection Timeout period.</span></span>
* <span data-ttu-id="654a3-224">Kapcsolat készletezését támogatja.</span><span class="sxs-lookup"><span data-stu-id="654a3-224">Supports connection pooling.</span></span> <span data-ttu-id="654a3-225">Ez magában foglalja egy hatékony annak ellenőrzése, hogy a kapcsolat hello működik-e a program biztosít objektum.</span><span class="sxs-lookup"><span data-stu-id="654a3-225">This includes an efficient verification that hello connection object it gives your program is functioning.</span></span>

<span data-ttu-id="654a3-226">Egy olyan kapcsolatobjektum kapcsolat készletből használatakor javasoljuk, hogy a program ideiglenesen zárja be a hello kapcsolat nem azonnal használata esetén.</span><span class="sxs-lookup"><span data-stu-id="654a3-226">When you use a connection object from a connection pool, we recommend that your program temporarily close hello connection when not immediately using it.</span></span> <span data-ttu-id="654a3-227">A kapcsolat újbóli megnyitásával nincs költséges hello módja az új kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="654a3-227">Re-opening a connection is not expensive hello way creating a new connection is.</span></span>

<span data-ttu-id="654a3-228">Ha az ADO.NET 4.0-s vagy korábbi, azt javasoljuk, hogy frissítse toohello legújabb ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="654a3-228">If you are using ADO.NET 4.0 or earlier, we recommend that you upgrade toohello latest ADO.NET.</span></span>

* <span data-ttu-id="654a3-229">2015. November frissítésétől is [töltse le az ADO.NET 4.6.1](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).</span><span class="sxs-lookup"><span data-stu-id="654a3-229">As of November 2015, you can [download ADO.NET 4.6.1](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).</span></span>

<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a><span data-ttu-id="654a3-230">Diagnosztika</span><span class="sxs-lookup"><span data-stu-id="654a3-230">Diagnostics</span></span>
<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a><span data-ttu-id="654a3-231">Diagnosztika: Ellenőrizze, hogy csatlakozni tud-segédprogramok</span><span class="sxs-lookup"><span data-stu-id="654a3-231">Diagnostics: Test whether utilities can connect</span></span>
<span data-ttu-id="654a3-232">Ha a program sikertelen tooconnect tooAzure SQL-adatbázist, egy diagnosztikai beállítás akkor tootry tooconnect segédprogram programmal.</span><span class="sxs-lookup"><span data-stu-id="654a3-232">If your program is failing tooconnect tooAzure SQL Database, one diagnostic option is tootry tooconnect with a utility program.</span></span> <span data-ttu-id="654a3-233">Ideális esetben hello segédprogram hello segítségével kapcsolódnának ugyanazt a szalagtárat, amelyek a program.</span><span class="sxs-lookup"><span data-stu-id="654a3-233">Ideally hello utility would connect by using hello same library that your program uses.</span></span>

<span data-ttu-id="654a3-234">Minden Windows rendszeren ezeket a segédprogramokat próbálkozhat:</span><span class="sxs-lookup"><span data-stu-id="654a3-234">On any Windows computer, you can try these utilities:</span></span>

* <span data-ttu-id="654a3-235">SQL Server Management Studio (ssms.exe), amely összeköti az ADO.NET használatával.</span><span class="sxs-lookup"><span data-stu-id="654a3-235">SQL Server Management Studio (ssms.exe), which connects by using ADO.NET.</span></span>
* <span data-ttu-id="654a3-236">Az Sqlcmd.exe, amelyek használatával kapcsolódik [ODBC](http://msdn.microsoft.com/library/jj730308.aspx).</span><span class="sxs-lookup"><span data-stu-id="654a3-236">sqlcmd.exe, which connects by using [ODBC](http://msdn.microsoft.com/library/jj730308.aspx).</span></span>

<span data-ttu-id="654a3-237">A csatlakozás után tesztelje, hogy egy rövid SQL SELECT lekérdezés működése.</span><span class="sxs-lookup"><span data-stu-id="654a3-237">Once connected, test whether a short SQL SELECT query works.</span></span>

<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-hello-open-ports"></a><span data-ttu-id="654a3-238">Diagnosztika: Hello megnyitott portok ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="654a3-238">Diagnostics: Check hello open ports</span></span>
<span data-ttu-id="654a3-239">Tegyük fel, hogy azt feltételezi, hogy a kapcsolódási kísérletek tooport problémák miatt nem működnek.</span><span class="sxs-lookup"><span data-stu-id="654a3-239">Suppose you suspect that connection attempts are failing due tooport issues.</span></span> <span data-ttu-id="654a3-240">A számítógépen futtathatja egy segédprogram, amely hello portbeállításokat kapcsolatos jelentések.</span><span class="sxs-lookup"><span data-stu-id="654a3-240">On your computer you can run a utility that reports on hello port configurations.</span></span>

<span data-ttu-id="654a3-241">A Linux hello alábbi segédprogramokat hasznosak lehetnek:</span><span class="sxs-lookup"><span data-stu-id="654a3-241">On Linux hello following utilities might be helpful:</span></span>

* `netstat -nap`
* `nmap -sS -O 127.0.0.1`
  * <span data-ttu-id="654a3-242">(Hello példa érték toobe az IP-cím módosításához.)</span><span class="sxs-lookup"><span data-stu-id="654a3-242">(Change hello example value toobe your IP address.)</span></span>

<span data-ttu-id="654a3-243">A Windows hello [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) segédprogram akkor lehetnek hasznosak.</span><span class="sxs-lookup"><span data-stu-id="654a3-243">On Windows hello [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) utility might be helpful.</span></span> <span data-ttu-id="654a3-244">Íme egy példa végrehajtását, amely egy Azure SQL adatbázis-kiszolgáló port helyzet hello kérdezhetők le, és amelyek futtatták hordozható számítógépen:</span><span class="sxs-lookup"><span data-stu-id="654a3-244">Here is an example execution that queried hello port situation on an Azure SQL Database server, and which was run on a laptop computer:</span></span>

```
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called:
 johndoesvr9.database.windows.net

Attempting tooresolve name tooIP address...
Name resolved too23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```


<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a><span data-ttu-id="654a3-245">Diagnosztika: A hibák naplózására.</span><span class="sxs-lookup"><span data-stu-id="654a3-245">Diagnostics: Log your errors</span></span>
<span data-ttu-id="654a3-246">Probléma néha legjobb felderítette általános a minta az észlelést keresztül nap vagy hét.</span><span class="sxs-lookup"><span data-stu-id="654a3-246">An intermittent problem is sometimes best diagnosed by detection of a general pattern over days or weeks.</span></span>

<span data-ttu-id="654a3-247">Az ügyfél segít a diagnosztikai naplózás minden hibákat.</span><span class="sxs-lookup"><span data-stu-id="654a3-247">Your client can assist in a diagnosis by logging all errors it encounters.</span></span> <span data-ttu-id="654a3-248">Előfordulhat, hogy képes toocorrelate hello naplóbejegyzések hiba adatokkal az Azure SQL Database belső naplók saját magát.</span><span class="sxs-lookup"><span data-stu-id="654a3-248">You might be able toocorrelate hello log entries with error data that Azure SQL Database logs itself internally.</span></span>

<span data-ttu-id="654a3-249">Vállalati könyvtár 6 (EntLib60) által felügyelt .NET osztályok tooassist naplózással kínálja:</span><span class="sxs-lookup"><span data-stu-id="654a3-249">Enterprise Library 6 (EntLib60) offers .NET managed classes tooassist with logging:</span></span>

* [<span data-ttu-id="654a3-250">5 - as könnyen mint alá tartozó ki egy naplófájl: hello alkalmazás blokk-naplózás használatával</span><span class="sxs-lookup"><span data-stu-id="654a3-250">5 - As Easy As Falling Off a Log: Using hello Logging Application Block</span></span>](http://msdn.microsoft.com/library/dn440731.aspx)

<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a><span data-ttu-id="654a3-251">Diagnosztika: Ellenőrizze a rendszer eseménynaplóit az hibák</span><span class="sxs-lookup"><span data-stu-id="654a3-251">Diagnostics: Examine system logs for errors</span></span>
<span data-ttu-id="654a3-252">Hiba a lekérdezés naplók és egyéb adatokat az alábbiakban néhány Transact-SQL SELECT utasítás.</span><span class="sxs-lookup"><span data-stu-id="654a3-252">Here are some Transact-SQL SELECT statements that query logs of error and other information.</span></span>

| <span data-ttu-id="654a3-253">A lekérdezési napló</span><span class="sxs-lookup"><span data-stu-id="654a3-253">Query of log</span></span> | <span data-ttu-id="654a3-254">Leírás</span><span class="sxs-lookup"><span data-stu-id="654a3-254">Description</span></span> |
|:--- |:--- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` |<span data-ttu-id="654a3-255">Hello [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) nézet nyújt információkat események, köztük egyes, amelyek átmeneti hibák vagy csatlakozási hibák okozhatják.</span><span class="sxs-lookup"><span data-stu-id="654a3-255">hello [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) view offers information about individual events, including some that can cause transient errors or connectivity failures.</span></span><br/><br/><span data-ttu-id="654a3-256">Ideális esetben hozhatók hello **start_time** vagy **end_time** értékek információkkal, ha az ügyfélprogram észlelt problémákat.</span><span class="sxs-lookup"><span data-stu-id="654a3-256">Ideally you can correlate hello **start_time** or **end_time** values with information about when your client program experienced problems.</span></span><br/><br/><span data-ttu-id="654a3-257">**Tipp:** csatlakoznia kell a toohello **fő** adatbázis toorun ez.</span><span class="sxs-lookup"><span data-stu-id="654a3-257">**TIP:** You must connect toohello **master** database toorun this.</span></span> |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` |<span data-ttu-id="654a3-258">Hello [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) nézet összesített számát az eseménytípusok, a további diagnosztikához is kínál.</span><span class="sxs-lookup"><span data-stu-id="654a3-258">hello [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) view offers aggregated counts of event types, for additional diagnostics.</span></span><br/><br/><span data-ttu-id="654a3-259">**Tipp:** csatlakoznia kell a toohello **fő** adatbázis toorun ez.</span><span class="sxs-lookup"><span data-stu-id="654a3-259">**TIP:** You must connect toohello **master** database toorun this.</span></span> |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-hello-sql-database-log"></a><span data-ttu-id="654a3-260">Diagnosztika: Keresse meg a probléma események hello SQL-adatbázis naplója</span><span class="sxs-lookup"><span data-stu-id="654a3-260">Diagnostics: Search for problem events in hello SQL Database log</span></span>
<span data-ttu-id="654a3-261">Kereshet kapcsolatos probléma az Azure SQL Database hello naplója bejegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="654a3-261">You can search for entries about problem events in hello log of Azure SQL Database.</span></span> <span data-ttu-id="654a3-262">Próbálja meg a következő Transact-SQL SELECT utasítás hello hello **fő** adatbázis:</span><span class="sxs-lookup"><span data-stu-id="654a3-262">Try hello following Transact-SQL SELECT statement in hello **master** database:</span></span>

```
SELECT
   object_name
  ,CAST(f.event_data as XML).value
      ('(/event/@timestamp)[1]', 'datetime2')                      AS [timestamp]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="error"]/value)[1]', 'int')             AS [error]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="state"]/value)[1]', 'int')             AS [state]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="is_success"]/value)[1]', 'bit')        AS [is_success]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="database_name"]/value)[1]', 'sysname') AS [database_name]
FROM
  sys.fn_xe_telemetry_blob_target_read_file('el', null, null, null) AS f
WHERE
  object_name != 'login_event'  -- Login events are numerous.
  and
  '2015-06-21' < CAST(f.event_data as XML).value
        ('(/event/@timestamp)[1]', 'datetime2')
ORDER BY
  [timestamp] DESC
;
```


#### <a name="a-few-returned-rows-from-sysfnxetelemetryblobtargetreadfile"></a><span data-ttu-id="654a3-263">A sys.fn_xe_telemetry_blob_target_read_file néhány visszaadott sorok</span><span class="sxs-lookup"><span data-stu-id="654a3-263">A few returned rows from sys.fn_xe_telemetry_blob_target_read_file</span></span>
<span data-ttu-id="654a3-264">Ezután van, hogyan nézhet ki egy visszaadott sort.</span><span class="sxs-lookup"><span data-stu-id="654a3-264">Next is what a returned row might look like.</span></span> <span data-ttu-id="654a3-265">hello null látható értékei gyakran nem null értékű a többi sort.</span><span class="sxs-lookup"><span data-stu-id="654a3-265">hello null values shown are often not null in other rows.</span></span>

```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a><span data-ttu-id="654a3-266">Vállalati könyvtár 6</span><span class="sxs-lookup"><span data-stu-id="654a3-266">Enterprise Library 6</span></span>
<span data-ttu-id="654a3-267">Vállalati könyvtár 6 (EntLib60) rendszer keretrendszere a .NET-osztályok, amely segít a felhőszolgáltatások, hatékony ügyfelek megvalósítása melyek egyike hello Azure SQL Database szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="654a3-267">Enterprise Library 6 (EntLib60) is a framework of .NET classes that helps you implement robust clients of cloud services, one of which is hello Azure SQL Database service.</span></span> <span data-ttu-id="654a3-268">Témakörök dedikált tooeach terület, amelyben a EntLib60 első ellátogatva segít keresheti meg:</span><span class="sxs-lookup"><span data-stu-id="654a3-268">You can locate topics dedicated tooeach area in which EntLib60 can assist by first visiting:</span></span>

* [<span data-ttu-id="654a3-269">Vállalati könyvtár 6 – 2013. április</span><span class="sxs-lookup"><span data-stu-id="654a3-269">Enterprise Library 6 - April 2013</span></span>](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx)

<span data-ttu-id="654a3-270">Újrapróbálkozási logika átmeneti hibák kezelése pedig segít a EntLib60 egy területen:</span><span class="sxs-lookup"><span data-stu-id="654a3-270">Retry logic for handling transient errors is one area in which EntLib60 can assist:</span></span>

* [<span data-ttu-id="654a3-271">4 - perseverance, a titkos kulcsot, annak összes Triumphs: átmeneti hiba kezelési alkalmazás blokk hello használata</span><span class="sxs-lookup"><span data-stu-id="654a3-271">4 - Perseverance, Secret of All Triumphs: Using hello Transient Fault Handling Application Block</span></span>](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx)

> [!NOTE]
> <span data-ttu-id="654a3-272">hello EntLib60 forráskódja érhető el a nyilvános [letöltése](http://go.microsoft.com/fwlink/p/?LinkID=290898).</span><span class="sxs-lookup"><span data-stu-id="654a3-272">hello source code for EntLib60 is available for public [download](http://go.microsoft.com/fwlink/p/?LinkID=290898).</span></span> <span data-ttu-id="654a3-273">A Microsoftnál nem tervek toomake további szolgáltatás frissítések és karbantartás tooEntLib frissíti.</span><span class="sxs-lookup"><span data-stu-id="654a3-273">Microsoft has no plans toomake further feature updates or maintenance updates tooEntLib.</span></span>
> 
> 

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a><span data-ttu-id="654a3-274">Átmeneti hibák, és próbálkozzon ismét EntLib60 osztályok</span><span class="sxs-lookup"><span data-stu-id="654a3-274">EntLib60 classes for transient errors and retry</span></span>
<span data-ttu-id="654a3-275">a következő EntLib60 osztályok hello különösen hasznosak újrapróbálkozási logika.</span><span class="sxs-lookup"><span data-stu-id="654a3-275">hello following EntLib60 classes are particularly useful for retry logic.</span></span> <span data-ttu-id="654a3-276">Ezek a, vagy további alatt, az összes névtér hello **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:</span><span class="sxs-lookup"><span data-stu-id="654a3-276">All these  are in, or are further under, hello namespace **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:</span></span>

<span data-ttu-id="654a3-277">*Hello névtérben **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:*</span><span class="sxs-lookup"><span data-stu-id="654a3-277">*In hello namespace **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:*</span></span>

* <span data-ttu-id="654a3-278">**A RetryPolicy** osztály</span><span class="sxs-lookup"><span data-stu-id="654a3-278">**RetryPolicy** class</span></span>
  
  * <span data-ttu-id="654a3-279">**ExecuteAction** módszer</span><span class="sxs-lookup"><span data-stu-id="654a3-279">**ExecuteAction** method</span></span>
* <span data-ttu-id="654a3-280">**ExponentialBackoff** osztály</span><span class="sxs-lookup"><span data-stu-id="654a3-280">**ExponentialBackoff** class</span></span>
* <span data-ttu-id="654a3-281">**SqlDatabaseTransientErrorDetectionStrategy** osztály</span><span class="sxs-lookup"><span data-stu-id="654a3-281">**SqlDatabaseTransientErrorDetectionStrategy** class</span></span>
* <span data-ttu-id="654a3-282">**ReliableSqlConnection** osztály</span><span class="sxs-lookup"><span data-stu-id="654a3-282">**ReliableSqlConnection** class</span></span>
  
  * <span data-ttu-id="654a3-283">**Az ExecuteCommand** módszer</span><span class="sxs-lookup"><span data-stu-id="654a3-283">**ExecuteCommand** method</span></span>

<span data-ttu-id="654a3-284">Hello névtérben **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:</span><span class="sxs-lookup"><span data-stu-id="654a3-284">In hello namespace **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:</span></span>

* <span data-ttu-id="654a3-285">**AlwaysTransientErrorDetectionStrategy** osztály</span><span class="sxs-lookup"><span data-stu-id="654a3-285">**AlwaysTransientErrorDetectionStrategy** class</span></span>
* <span data-ttu-id="654a3-286">**NeverTransientErrorDetectionStrategy** osztály</span><span class="sxs-lookup"><span data-stu-id="654a3-286">**NeverTransientErrorDetectionStrategy** class</span></span>

<span data-ttu-id="654a3-287">Az alábbiakban a hivatkozások tooinformation EntLib60 kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="654a3-287">Here are links tooinformation about EntLib60:</span></span>

* <span data-ttu-id="654a3-288">Szabad [könyv letöltése: Fejlesztői útmutató tooMicrosoft vállalati könyvtárba, 2. kiadás](http://www.microsoft.com/download/details.aspx?id=41145)</span><span class="sxs-lookup"><span data-stu-id="654a3-288">Free [Book Download: Developer's Guide tooMicrosoft Enterprise Library, 2nd Edition](http://www.microsoft.com/download/details.aspx?id=41145)</span></span>
* <span data-ttu-id="654a3-289">Gyakorlati tanácsok: [ismételje meg az általános útmutatást](../best-practices-retry-general.md) az újrapróbálkozási logika egy kiváló részletes ismertető rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="654a3-289">Best practices: [Retry general guidance](../best-practices-retry-general.md) has an excellent in-depth discussion of retry logic.</span></span>
* <span data-ttu-id="654a3-290">A NuGet letöltésének [vállalati Library - alkalmazás blokk átmeneti hiba kezelő 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)</span><span class="sxs-lookup"><span data-stu-id="654a3-290">NuGet download of [Enterprise Library - Transient Fault Handling application block 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)</span></span>

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-hello-logging-block"></a><span data-ttu-id="654a3-291">EntLib60: hello naplózás letiltása</span><span class="sxs-lookup"><span data-stu-id="654a3-291">EntLib60: hello logging block</span></span>
* <span data-ttu-id="654a3-292">hello naplózás letiltása, amely lehetővé teszi magas rugalmas és konfigurálható megoldás:</span><span class="sxs-lookup"><span data-stu-id="654a3-292">hello Logging block is a highly flexible and configurable solution that allows you to:</span></span>
  
  * <span data-ttu-id="654a3-293">Hozzon létre, és számos különböző helyek naplóüzenetek tárolja.</span><span class="sxs-lookup"><span data-stu-id="654a3-293">Create and store log messages in a wide variety of locations.</span></span>
  * <span data-ttu-id="654a3-294">Kategorizálását, és üzenetek szűréséhez.</span><span class="sxs-lookup"><span data-stu-id="654a3-294">Categorize and filter messages.</span></span>
  * <span data-ttu-id="654a3-295">Információgyűjtés környezetfüggő, amely hasznos a Hibakeresés és nyomkövetés, illetve a naplózási és általános naplózási követelményeknek.</span><span class="sxs-lookup"><span data-stu-id="654a3-295">Collect contextual information that is useful for debugging and tracing, as well as for auditing and general logging requirements.</span></span>
* <span data-ttu-id="654a3-296">hello naplózás letiltása kivonatolja hello naplózási funkciót a hello naplócél, úgy, hogy hello alkalmazáskód konzisztens, függetlenül az hello cél naplózási tároló hello helye és típusa.</span><span class="sxs-lookup"><span data-stu-id="654a3-296">hello Logging block abstracts hello logging functionality from hello log destination so that hello application code is consistent, irrespective of hello location and type of hello target logging store.</span></span>

<span data-ttu-id="654a3-297">További információ:: [5 –, egyszerűen mint alá tartozó ki egy naplófájl: hello alkalmazás blokk-naplózás használatával](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="654a3-297">For details see: [5 - As Easy As Falling Off a Log: Using hello Logging Application Block](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)</span></span>

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a><span data-ttu-id="654a3-298">EntLib60 IsTransient metódus forráskód</span><span class="sxs-lookup"><span data-stu-id="654a3-298">EntLib60 IsTransient method source code</span></span>
<span data-ttu-id="654a3-299">Ezt követően a hello **SqlDatabaseTransientErrorDetectionStrategy** osztály, a C# hello forráskódja hello **IsTransient** metódust.</span><span class="sxs-lookup"><span data-stu-id="654a3-299">Next, from hello **SqlDatabaseTransientErrorDetectionStrategy** class, is hello C# source code for hello **IsTransient** method.</span></span> <span data-ttu-id="654a3-300">hello forráskód tisztázza, hogy mely hibák toobe átmeneti jellegű, és az Ismét gombra, 2013. április frissítésétől worthy vették.</span><span class="sxs-lookup"><span data-stu-id="654a3-300">hello source code clarifies which errors were considered toobe transient and worthy of retry, as of April 2013.</span></span>

<span data-ttu-id="654a3-301">Számos **//comment** sorok el lettek távolítva a másolási tooemphasize olvashatóság érdekében.</span><span class="sxs-lookup"><span data-stu-id="654a3-301">Numerous **//comment** lines have been removed from this copy tooemphasize readability.</span></span>

```
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in hello exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // hello service is currently busy. Retry hello request after 10 seconds.
            // Code: (reason code toobe decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode hello reason code from hello error message to
            // determine hello grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach hello decoded values as additional attributes to
            // hello original SQL exception.
            sqlException.Data[condition.ThrottlingMode.GetType().Name] =
              condition.ThrottlingMode.ToString();
            sqlException.Data[condition.GetType().Name] = condition;

            return true;

          case 10928:
          case 10929:
          case 10053:
          case 10054:
          case 10060:
          case 40197:
          case 40540:
          case 40613:
          case 40143:
          case 233:
          case 64:
            // DBNETLIB Error Code: 20
            // hello instance of SQL Server you attempted tooconnect to
            // does not support encryption.
          case (int)ProcessNetLibErrorCode.EncryptionNotSupported:
            return true;
        }
      }
    }
    else if (ex is TimeoutException)
    {
      return true;
    }
    else
    {
      EntityException entityException;
      if ((entityException = ex as EntityException) != null)
      {
        return this.IsTransient(entityException.InnerException);
      }
    }
  }

  return false;
}
```


## <a name="next-steps"></a><span data-ttu-id="654a3-302">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="654a3-302">Next steps</span></span>
* <span data-ttu-id="654a3-303">Más közös Azure SQL-adatbázis kapcsolati problémák elhárítása, látogasson el [kapcsolatos problémák elhárítása kapcsolódási problémák tooAzure SQL-adatbázis](sql-database-troubleshoot-common-connection-issues.md).</span><span class="sxs-lookup"><span data-stu-id="654a3-303">For troubleshooting other common Azure SQL Database connection issues, visit [Troubleshoot connection issues tooAzure SQL Database](sql-database-troubleshoot-common-connection-issues.md).</span></span>
* [<span data-ttu-id="654a3-304">SQL Server-kapcsolatkészlet (ADO.NET)</span><span class="sxs-lookup"><span data-stu-id="654a3-304">SQL Server Connection Pooling (ADO.NET)</span></span>](http://msdn.microsoft.com/library/8xx3tyca.aspx)
* [<span data-ttu-id="654a3-305">*Újrapróbálkozás* van egy Apache 2.0 licenccel rendelkező általános célú könyvtár írt újrapróbálkozás **Python**, toosimplify hello tevékenység hozzáadásának újrapróbálkozási viselkedés toojust kapcsolatos semmit.</span><span class="sxs-lookup"><span data-stu-id="654a3-305">*Retrying* is an Apache 2.0 licensed general-purpose retrying library, written in **Python**, toosimplify hello task of adding retry behavior toojust about anything.</span></span>](https://pypi.python.org/pypi/retrying)

