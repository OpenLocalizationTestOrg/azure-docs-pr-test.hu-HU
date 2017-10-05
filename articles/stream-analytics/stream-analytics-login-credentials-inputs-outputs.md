---
title: "A Stream Analytics: Elforgatása napló hitelesítő adataival bemenetekhez és kimenetekhez |} Microsoft Docs"
description: "Útmutató a Stream Analytics bemenetekhez és kimenetekhez hitelesítő adatait."
keywords: "a bejelentkezési hitelesítő adatok"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 42ae83e1-cd33-49bb-a455-a39a7c151ea4
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 2cb995a3969a8cb025f371ed0ab160cd04b0454d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="rotate-login-credentials-for-inputs-and-outputs-in-stream-analytics-jobs"></a><span data-ttu-id="d1d38-104">Forgassa el a be- és kimenetekkel a Stream Analytics-feladatok bejelentkezési hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="d1d38-104">Rotate login credentials for inputs and outputs in Stream Analytics Jobs</span></span>
## <a name="abstract"></a><span data-ttu-id="d1d38-105">Absztrakt</span><span class="sxs-lookup"><span data-stu-id="d1d38-105">Abstract</span></span>
<span data-ttu-id="d1d38-106">Az Azure Stream Analytics jelenleg nem engedélyezi a felhasználó hitelesítő adatai egy bemeneti/kimeneti cseréje a feladat futása közben.</span><span class="sxs-lookup"><span data-stu-id="d1d38-106">Azure Stream Analytics today doesn’t allow replacing the credentials on an input/output while the job is running.</span></span>

<span data-ttu-id="d1d38-107">Azure Stream Analytics nem támogatja az utolsó kimenetből egy feladat folytatása, amíg meg akartunk ahhoz, hogy minimalizálja a leállítási közötti időeltérés és a feladat elindítása és elforgatása a bejelentkezési hitelesítő adatok a teljes folyamatot.</span><span class="sxs-lookup"><span data-stu-id="d1d38-107">While Azure Stream Analytics does support resuming a job from last output, we wanted to share the entire process for minimizing the lag between the stopping and starting of the job and rotating the login credentials.</span></span>

## <a name="part-1---prepare-the-new-set-of-credentials"></a><span data-ttu-id="d1d38-108">1. rész - készítse elő az újonnan létrehozott hitelesítő adatokat:</span><span class="sxs-lookup"><span data-stu-id="d1d38-108">Part 1 - Prepare the new set of credentials:</span></span>
<span data-ttu-id="d1d38-109">Ez a kijelző tulajdonság a következő bemenetek/kimenetek vonatkozik:</span><span class="sxs-lookup"><span data-stu-id="d1d38-109">This part is applicable to the following inputs/outputs:</span></span>

* <span data-ttu-id="d1d38-110">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="d1d38-110">Blob Storage</span></span>
* <span data-ttu-id="d1d38-111">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="d1d38-111">Event Hubs</span></span>
* <span data-ttu-id="d1d38-112">SQL Database</span><span class="sxs-lookup"><span data-stu-id="d1d38-112">SQL Database</span></span>
* <span data-ttu-id="d1d38-113">Table Storage</span><span class="sxs-lookup"><span data-stu-id="d1d38-113">Table Storage</span></span>

<span data-ttu-id="d1d38-114">Az egyéb bemenetek/kimenetek folytassa a 2. rész.</span><span class="sxs-lookup"><span data-stu-id="d1d38-114">For other inputs/outputs, proceed with Part 2.</span></span>

### <a name="blob-storagetable-storage"></a><span data-ttu-id="d1d38-115">A BLOB storage-tároló vagy tábla</span><span class="sxs-lookup"><span data-stu-id="d1d38-115">Blob storage/Table storage</span></span>
1. <span data-ttu-id="d1d38-116">A tárolási kiterjesztés nyissa meg az Azure felügyeleti portálra:</span><span class="sxs-lookup"><span data-stu-id="d1d38-116">Go to the Storage extention on the Azure Management portal:</span></span>  
   ![graphic1][graphic1]
2. <span data-ttu-id="d1d38-118">Keresse meg a tároló, a feladat által használt, és lépjen be:</span><span class="sxs-lookup"><span data-stu-id="d1d38-118">Locate the storage used by your job and go into it:</span></span>  
   ![graphic2][graphic2]
3. <span data-ttu-id="d1d38-120">Kattintson a elérési kulcsok kezelése parancsot:</span><span class="sxs-lookup"><span data-stu-id="d1d38-120">Click the Manage Access Keys command:</span></span>  
   ![graphic3][graphic3]
4. <span data-ttu-id="d1d38-122">Az elsődleges elérési kulcsot és a másodlagos hozzáférési kulcsot közötti **válassza ki a feladat nem használja azt**.</span><span class="sxs-lookup"><span data-stu-id="d1d38-122">Between the Primary Access Key and the Secondary Access Key, **pick the one not used by your job**.</span></span>
5. <span data-ttu-id="d1d38-123">Találati újragenerálása:</span><span class="sxs-lookup"><span data-stu-id="d1d38-123">Hit regenerate:</span></span>  
   ![graphic4][graphic4]
6. <span data-ttu-id="d1d38-125">Másolja ki az újonnan létrehozott kulcsot:</span><span class="sxs-lookup"><span data-stu-id="d1d38-125">Copy the newly generated key:</span></span>  
   ![graphic5][graphic5]
7. <span data-ttu-id="d1d38-127">Folytassa a 2.</span><span class="sxs-lookup"><span data-stu-id="d1d38-127">Continue to Part 2.</span></span>

### <a name="event-hubs"></a><span data-ttu-id="d1d38-128">Az Event hubs</span><span class="sxs-lookup"><span data-stu-id="d1d38-128">Event hubs</span></span>
1. <span data-ttu-id="d1d38-129">Nyissa meg az Azure felügyeleti portálra a Service Bus-bővítmény:</span><span class="sxs-lookup"><span data-stu-id="d1d38-129">Go to the Service Bus extension on the Azure Management portal:</span></span>  
   ![graphic6][graphic6]
2. <span data-ttu-id="d1d38-131">Keresse meg a Service Bus Namespace a feladat által használt, és lépjen be:</span><span class="sxs-lookup"><span data-stu-id="d1d38-131">Locate the Service Bus Namespace used by your job and go into it:</span></span>  
   ![graphic7][graphic7]
3. <span data-ttu-id="d1d38-133">Ha a feladat egy megosztott elérési házirendet használja a Service Bus Namespace, Ugrás a 6. lépés</span><span class="sxs-lookup"><span data-stu-id="d1d38-133">If your job uses a shared access policy on the Service Bus Namespace, jump to step 6</span></span>  
4. <span data-ttu-id="d1d38-134">Ugrás az Event Hubs lapon:</span><span class="sxs-lookup"><span data-stu-id="d1d38-134">Go to the Event Hubs tab:</span></span>  
   ![graphic8][graphic8]
5. <span data-ttu-id="d1d38-136">Keresse meg a feladat által használt Eseményközpont, és lépjen be:</span><span class="sxs-lookup"><span data-stu-id="d1d38-136">Locate the Event Hub used by your job and go into it:</span></span>  
   ![graphic9][graphic9]
6. <span data-ttu-id="d1d38-138">Ugrás a konfigurálása lapon:</span><span class="sxs-lookup"><span data-stu-id="d1d38-138">Go to the Configure Tab:</span></span>  
   ![graphic10][graphic10]
7. <span data-ttu-id="d1d38-140">A házirendnév legördülő menüben keresse meg a megosztott elérési házirendet, a feladat használja:</span><span class="sxs-lookup"><span data-stu-id="d1d38-140">On the Policy Name drop-down, locate the shared access policy used by your job:</span></span>  
   ![graphic11][graphic11]
8. <span data-ttu-id="d1d38-142">Az elsődleges és a másodlagos kulcs közötti **válassza ki a feladat nem használja azt**.</span><span class="sxs-lookup"><span data-stu-id="d1d38-142">Between the Primary Key and the Secondary Key, **pick the one not used by your job**.</span></span>  
9. <span data-ttu-id="d1d38-143">Találati újragenerálása:</span><span class="sxs-lookup"><span data-stu-id="d1d38-143">Hit regenerate:</span></span>  
   ![graphic12][graphic12]
10. <span data-ttu-id="d1d38-145">Másolja ki az újonnan létrehozott kulcsot:</span><span class="sxs-lookup"><span data-stu-id="d1d38-145">Copy the newly generated key:</span></span>  
   ![graphic13][graphic13]
11. <span data-ttu-id="d1d38-147">Folytassa a 2.</span><span class="sxs-lookup"><span data-stu-id="d1d38-147">Continue to Part 2.</span></span>  

### <a name="sql-database"></a><span data-ttu-id="d1d38-148">SQL Database</span><span class="sxs-lookup"><span data-stu-id="d1d38-148">SQL Database</span></span>
> [!NOTE]
> <span data-ttu-id="d1d38-149">Megjegyzés: szüksége lesz a SQL Database szolgáltatáshoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="d1d38-149">Note: you will need to connect to the SQL Database Service.</span></span> <span data-ttu-id="d1d38-150">Fogjuk mutatják be ehhez a felügyeleti élmény használja az Azure felügyeleti portálon, de néhány ügyféloldali eszköz például az SQL Server Management Studio is választhatja, hogy.</span><span class="sxs-lookup"><span data-stu-id="d1d38-150">We are going to show how to do this using the management experience on the Azure Management portal but you may choose to use some client-side tool such as SQL Server Management Studio as well.</span></span>
>
> 

1. <span data-ttu-id="d1d38-151">Az SQL-adatbázis bővítmény nyissa meg az Azure felügyeleti portálra:</span><span class="sxs-lookup"><span data-stu-id="d1d38-151">Go to the SQL Databases extension on the Azure Management portal:</span></span>  
   ![graphic14][graphic14]
2. <span data-ttu-id="d1d38-153">Keresse meg a feladat által használt SQL-adatbázis és **a kiszolgálón kattintson** ugyanazon hivatkozásra:</span><span class="sxs-lookup"><span data-stu-id="d1d38-153">Locate the SQL Database used by your job and **click on the server** link on the same line:</span></span>  
   <span data-ttu-id="d1d38-154">![graphic15][graphic15]</span><span class="sxs-lookup"><span data-stu-id="d1d38-154">![graphic15][graphic15]</span></span>
3. <span data-ttu-id="d1d38-155">Kattintson a kezelés parancsot:</span><span class="sxs-lookup"><span data-stu-id="d1d38-155">Click the Manage command:</span></span>  
   ![graphic16][graphic16]
4. <span data-ttu-id="d1d38-157">Adatbázis fő típusa:</span><span class="sxs-lookup"><span data-stu-id="d1d38-157">Type Database Master:</span></span>  
   ![graphic17][graphic17]
5. <span data-ttu-id="d1d38-159">Írja be a felhasználónevét és jelszavát, és kattintson a napló a:</span><span class="sxs-lookup"><span data-stu-id="d1d38-159">Type in your User Name, Password, and click Log on:</span></span>  
   ![graphic18][graphic18]
6. <span data-ttu-id="d1d38-161">Kattintson az új lekérdezés:</span><span class="sxs-lookup"><span data-stu-id="d1d38-161">Click New Query:</span></span>  
   ![graphic19][graphic19]
7. <span data-ttu-id="d1d38-163">Írja be a következő lekérdezést a < login_name > lecserélését a felhasználó nevét, és lecserél <enterStrongPasswordHere> az új jelszóval:</span><span class="sxs-lookup"><span data-stu-id="d1d38-163">Type in the following query replacing <login_name> with your User Name and replacing <enterStrongPasswordHere> with your new password:</span></span>  
   `CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8. <span data-ttu-id="d1d38-164">Kattintson a Futtatás gombra:</span><span class="sxs-lookup"><span data-stu-id="d1d38-164">Click Run:</span></span>  
   ![graphic20][graphic20]
9. <span data-ttu-id="d1d38-166">Vissza a lépés 2 és az idő kattintson az adatbázisban:</span><span class="sxs-lookup"><span data-stu-id="d1d38-166">Go back to step 2 and this time click the database:</span></span>  
   ![graphic21][graphic21]
10. <span data-ttu-id="d1d38-168">Kattintson a kezelés parancsot:</span><span class="sxs-lookup"><span data-stu-id="d1d38-168">Click the Manage command:</span></span>  
   ![graphic22][graphic22]
11. <span data-ttu-id="d1d38-170">Írja be a felhasználónevét és jelszavát, és kattintson a bejelentkezés:</span><span class="sxs-lookup"><span data-stu-id="d1d38-170">type in your User Name, Password, and click Logon:</span></span>  
   ![graphic23][graphic23]
12. <span data-ttu-id="d1d38-172">Kattintson az új lekérdezés:</span><span class="sxs-lookup"><span data-stu-id="d1d38-172">Click New Query:</span></span>  
   ![graphic24][graphic24]
13. <span data-ttu-id="d1d38-174">Adja meg a következő lekérdezés < felhasználónév > lecserélését egy nevet, amely szerint ehhez a bejelentkezéshez (biztosíthat < login_name >, például adott ugyanarra az értékre) adatbázis környezetében azonosítani szeretné és lecserél < login_name > az új felhasználónév:</span><span class="sxs-lookup"><span data-stu-id="d1d38-174">Type in the following query replacing <user_name> with a name by which you want to identify this login in the context of this database (you can provide the same value you gave for <login_name>, for example) and replacing <login_name> with your new user name:</span></span>  
   `CREATE USER <user_name> FROM LOGIN <login_name>`
14. <span data-ttu-id="d1d38-175">Kattintson a Futtatás gombra:</span><span class="sxs-lookup"><span data-stu-id="d1d38-175">Click Run:</span></span>  
   ![graphic25][graphic25]
15. <span data-ttu-id="d1d38-177">Az új felhasználó most már azonos a szerepkörök és az eredeti felhasználó rendelkezik-e jogosultsággal kell biztosítania.</span><span class="sxs-lookup"><span data-stu-id="d1d38-177">You should now provide your new user with the same roles and privileges your original user had.</span></span>
16. <span data-ttu-id="d1d38-178">Folytassa a 2.</span><span class="sxs-lookup"><span data-stu-id="d1d38-178">Continue to Part 2.</span></span>

## <a name="part-2-stopping-the-stream-analytics-job"></a><span data-ttu-id="d1d38-179">2. lépés: A Stream Analytics-feladat leállítása</span><span class="sxs-lookup"><span data-stu-id="d1d38-179">Part 2: Stopping the Stream Analytics Job</span></span>
1. <span data-ttu-id="d1d38-180">Nyissa meg az Azure felügyeleti portálra a Stream Analytics-bővítmény:</span><span class="sxs-lookup"><span data-stu-id="d1d38-180">Go to the Stream Analytics extension on the Azure Management portal:</span></span>  
   ![graphic26][graphic26]
2. <span data-ttu-id="d1d38-182">Keresse meg a feladat, és lépjen be:</span><span class="sxs-lookup"><span data-stu-id="d1d38-182">Locate your job and go into it:</span></span>  
   ![graphic27][graphic27]
3. <span data-ttu-id="d1d38-184">Nyissa meg a bemeneti adatok vagy az alapján, hogy a hitelesítő adatok bemenettel vagy kimenettel vannak elforgatása kimenetek lapra.</span><span class="sxs-lookup"><span data-stu-id="d1d38-184">Go to the Inputs tab or the Outputs tab based on whether you are rotating the credentials on an Input or on an Output.</span></span>  
   ![graphic28][graphic28]
4. <span data-ttu-id="d1d38-186">Kattintson a leállítási parancsot, és erősítse meg a feladat leállt:</span><span class="sxs-lookup"><span data-stu-id="d1d38-186">Click the Stop command and confirm the job has stopped:</span></span>  
   <span data-ttu-id="d1d38-187">![graphic29][graphic29] várja meg a feladat leállítása.</span><span class="sxs-lookup"><span data-stu-id="d1d38-187">![graphic29][graphic29] Wait for the job to stop.</span></span>
5. <span data-ttu-id="d1d38-188">Keresse meg a bemeneti/kimeneti forgassa el a hitelesítő adatokat a, és lépjen be:</span><span class="sxs-lookup"><span data-stu-id="d1d38-188">Locate the input/output you want to rotate credentials on and go into it:</span></span>  
   ![graphic30][graphic30]
6. <span data-ttu-id="d1d38-190">Folytassa a 3.</span><span class="sxs-lookup"><span data-stu-id="d1d38-190">Proceed to Part 3.</span></span>

## <a name="part-3-editing-the-credentials-on-the-stream-analytics-job"></a><span data-ttu-id="d1d38-191">3. lépés: A Stream Analytics-feladat a hitelesítő adatok szerkesztése</span><span class="sxs-lookup"><span data-stu-id="d1d38-191">Part 3: Editing the credentials on the Stream Analytics Job</span></span>
### <a name="blob-storagetable-storage"></a><span data-ttu-id="d1d38-192">A BLOB storage-tároló vagy tábla</span><span class="sxs-lookup"><span data-stu-id="d1d38-192">Blob storage/Table storage</span></span>
1. <span data-ttu-id="d1d38-193">A Tárfiók kulcsa mező található, és az újonnan létrehozott kulcs beillesztése:</span><span class="sxs-lookup"><span data-stu-id="d1d38-193">Find the Storage Account Key field and paste your newly generated key into it:</span></span>  
   ![graphic31][graphic31]
2. <span data-ttu-id="d1d38-195">Kattintson a Mentés parancsot, és ellenőrizze, hogy a módosítások mentése:</span><span class="sxs-lookup"><span data-stu-id="d1d38-195">Click the Save command and confirm saving your changes:</span></span>  
   ![graphic32][graphic32]
3. <span data-ttu-id="d1d38-197">A kapcsolat tesztelése automatikusan elindul, amikor menti a módosításokat, győződjön meg arról, amely sikeresen megfelelt.</span><span class="sxs-lookup"><span data-stu-id="d1d38-197">A connection test will automatically start when you save your changes, make sure that is has successfully passed.</span></span>
4. <span data-ttu-id="d1d38-198">Folytassa a 4.</span><span class="sxs-lookup"><span data-stu-id="d1d38-198">Proceed to Part 4.</span></span>

### <a name="event-hubs"></a><span data-ttu-id="d1d38-199">Az Event hubs</span><span class="sxs-lookup"><span data-stu-id="d1d38-199">Event hubs</span></span>
1. <span data-ttu-id="d1d38-200">Az Event Hub házirend kulcs mező található, és az újonnan létrehozott kulcs beillesztése:</span><span class="sxs-lookup"><span data-stu-id="d1d38-200">Find the Event Hub Policy Key field and paste your newly generated key into it:</span></span>  
   ![graphic33][graphic33]
2. <span data-ttu-id="d1d38-202">Kattintson a Mentés parancsot, és ellenőrizze, hogy a módosítások mentése:</span><span class="sxs-lookup"><span data-stu-id="d1d38-202">Click the Save command and confirm saving your changes:</span></span>  
   ![graphic34][graphic34]
3. <span data-ttu-id="d1d38-204">A kapcsolat tesztelése automatikusan elindul, amikor menti a módosításokat, győződjön meg arról, hogy azt sikeresen megfelelt.</span><span class="sxs-lookup"><span data-stu-id="d1d38-204">A connection test will automatically start when you save your changes, make sure that it has successfully passed.</span></span>
4. <span data-ttu-id="d1d38-205">Folytassa a 4.</span><span class="sxs-lookup"><span data-stu-id="d1d38-205">Proceed to Part 4.</span></span>

### <a name="power-bi"></a><span data-ttu-id="d1d38-206">Power BI</span><span class="sxs-lookup"><span data-stu-id="d1d38-206">Power BI</span></span>
1. <span data-ttu-id="d1d38-207">Kattintson a megújítás engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="d1d38-207">Click the Renew authorization:</span></span>  

   ![graphic35][graphic35]
2. <span data-ttu-id="d1d38-209">A következő megerősítő jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="d1d38-209">You will get the following confirmation:</span></span>  

   ![graphic36][graphic36]
3. <span data-ttu-id="d1d38-211">Kattintson a Mentés parancsot, és ellenőrizze, hogy a módosítások mentése:</span><span class="sxs-lookup"><span data-stu-id="d1d38-211">Click the Save command and confirm saving your changes:</span></span>  
   ![graphic37][graphic37]
4. <span data-ttu-id="d1d38-213">A kapcsolat tesztelése automatikusan elindul, amikor a módosítások mentése, győződjön meg arról, hogy sikeresen megfelelt.</span><span class="sxs-lookup"><span data-stu-id="d1d38-213">A connection test will automatically start when you save your changes, make sure it has successfully passed.</span></span>
5. <span data-ttu-id="d1d38-214">Folytassa a 4.</span><span class="sxs-lookup"><span data-stu-id="d1d38-214">Proceed to Part 4.</span></span>

### <a name="sql-database"></a><span data-ttu-id="d1d38-215">SQL Database</span><span class="sxs-lookup"><span data-stu-id="d1d38-215">SQL Database</span></span>
1. <span data-ttu-id="d1d38-216">A felhasználónév és a jelszó mező található, és illessze be az újonnan létrehozott hitelesítőadat-készletet a őket:</span><span class="sxs-lookup"><span data-stu-id="d1d38-216">Find the User Name and Password fields and paste your newly created set of credentials into them:</span></span>  
   ![graphic38][graphic38]
2. <span data-ttu-id="d1d38-218">Kattintson a Mentés parancsot, és ellenőrizze, hogy a módosítások mentése:</span><span class="sxs-lookup"><span data-stu-id="d1d38-218">Click the Save command and confirm saving your changes:</span></span>  
   ![graphic39][graphic39]
3. <span data-ttu-id="d1d38-220">A kapcsolat tesztelése automatikusan elindul, amikor menti a módosításokat, győződjön meg arról, hogy azt sikeresen megfelelt.</span><span class="sxs-lookup"><span data-stu-id="d1d38-220">A connection test will automatically start when you save your changes, make sure that it has successfully passed.</span></span>  
4. <span data-ttu-id="d1d38-221">Folytassa a 4.</span><span class="sxs-lookup"><span data-stu-id="d1d38-221">Proceed to Part 4.</span></span>

## <a name="part-4-starting-your-job-from-last-stopped-time"></a><span data-ttu-id="d1d38-222">4. lépés: A feladat kiindulva leállított legutóbbi</span><span class="sxs-lookup"><span data-stu-id="d1d38-222">Part 4: Starting your job from last stopped time</span></span>
1. <span data-ttu-id="d1d38-223">A bemeneti/kimeneti elhagyni:</span><span class="sxs-lookup"><span data-stu-id="d1d38-223">Navigate away from the Input/Output:</span></span>  
   ![graphic40][graphic40]
2. <span data-ttu-id="d1d38-225">Kattintson az indítási parancsot:</span><span class="sxs-lookup"><span data-stu-id="d1d38-225">Click the Start command:</span></span>  
   ![graphic41][graphic41]
3. <span data-ttu-id="d1d38-227">Válassza ki a feladat utolsó befejezési időpontja, és kattintson az OK gombra:</span><span class="sxs-lookup"><span data-stu-id="d1d38-227">Pick the Last Stopped Time and click OK:</span></span>  
   ![graphic42][graphic42]
4. <span data-ttu-id="d1d38-229">Folytassa az 5.</span><span class="sxs-lookup"><span data-stu-id="d1d38-229">Proceed to Part 5.</span></span>  

## <a name="part-5-removing-the-old-set-of-credentials"></a><span data-ttu-id="d1d38-230">5. lépés: Régi megadott hitelesítői eltávolítása</span><span class="sxs-lookup"><span data-stu-id="d1d38-230">Part 5: Removing the old set of credentials</span></span>
<span data-ttu-id="d1d38-231">Ez a kijelző tulajdonság a következő bemenetek/kimenetek vonatkozik:</span><span class="sxs-lookup"><span data-stu-id="d1d38-231">This part is applicable to the following inputs/outputs:</span></span>

* <span data-ttu-id="d1d38-232">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="d1d38-232">Blob Storage</span></span>
* <span data-ttu-id="d1d38-233">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="d1d38-233">Event Hubs</span></span>
* <span data-ttu-id="d1d38-234">SQL Database</span><span class="sxs-lookup"><span data-stu-id="d1d38-234">SQL Database</span></span>
* <span data-ttu-id="d1d38-235">Table Storage</span><span class="sxs-lookup"><span data-stu-id="d1d38-235">Table Storage</span></span>

### <a name="blob-storagetable-storage"></a><span data-ttu-id="d1d38-236">A BLOB storage-tároló vagy tábla</span><span class="sxs-lookup"><span data-stu-id="d1d38-236">Blob storage/Table storage</span></span>
<span data-ttu-id="d1d38-237">Ismételje meg az 1. rész az elérési kulcsot, amely korábban a használt a feladat már nem használt a hozzáférési kulcs megújításához.</span><span class="sxs-lookup"><span data-stu-id="d1d38-237">Repeat Part 1 for the Access Key that was previously used by your job to renew the now unused Access Key.</span></span>

### <a name="event-hubs"></a><span data-ttu-id="d1d38-238">Az Event hubs</span><span class="sxs-lookup"><span data-stu-id="d1d38-238">Event hubs</span></span>
<span data-ttu-id="d1d38-239">Ismételje meg a feladat már nem használt kulcs megújításához korábban használt kulcs 1. rész.</span><span class="sxs-lookup"><span data-stu-id="d1d38-239">Repeat Part 1 for the Key that was previously used by your job to renew the now unused Key.</span></span>

### <a name="sql-database"></a><span data-ttu-id="d1d38-240">SQL Database</span><span class="sxs-lookup"><span data-stu-id="d1d38-240">SQL Database</span></span>
1. <span data-ttu-id="d1d38-241">A rész 1 7. lépés és írja be a következő lekérdezés < previous_login_name > lecserélését a felhasználó nevét, a feladat által korábban használt lépjen vissza a lekérdezési ablakban:</span><span class="sxs-lookup"><span data-stu-id="d1d38-241">Go back to the query window from Part 1 Step 7 and type in the following query, replacing <previous_login_name> with the User Name that was previously used by your job:</span></span>  
   `DROP LOGIN <previous_login_name>`  
2. <span data-ttu-id="d1d38-242">Kattintson a Futtatás gombra:</span><span class="sxs-lookup"><span data-stu-id="d1d38-242">Click Run:</span></span>  
   ![graphic43][graphic43]  

<span data-ttu-id="d1d38-244">A következő megerősítő szerezheti be:</span><span class="sxs-lookup"><span data-stu-id="d1d38-244">You should get the following confirmation:</span></span> 

    Command(s) completed successfully.

## <a name="get-help"></a><span data-ttu-id="d1d38-245">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="d1d38-245">Get help</span></span>
<span data-ttu-id="d1d38-246">További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="d1d38-246">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="d1d38-247">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d1d38-247">Next steps</span></span>
* [<span data-ttu-id="d1d38-248">Az Azure Stream Analytics bemutatása</span><span class="sxs-lookup"><span data-stu-id="d1d38-248">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="d1d38-249">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="d1d38-249">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="d1d38-250">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="d1d38-250">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="d1d38-251">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="d1d38-251">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="d1d38-252">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="d1d38-252">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[graphic1]: ./media/stream-analytics-login-credentials-inputs-outputs/1-stream-analytics-login-credentials-inputs-outputs.png
[graphic2]: ./media/stream-analytics-login-credentials-inputs-outputs/2-stream-analytics-login-credentials-inputs-outputs.png
[graphic3]: ./media/stream-analytics-login-credentials-inputs-outputs/3-stream-analytics-login-credentials-inputs-outputs.png
[graphic4]: ./media/stream-analytics-login-credentials-inputs-outputs/4-stream-analytics-login-credentials-inputs-outputs.png
[graphic5]: ./media/stream-analytics-login-credentials-inputs-outputs/5-stream-analytics-login-credentials-inputs-outputs.png
[graphic6]: ./media/stream-analytics-login-credentials-inputs-outputs/6-stream-analytics-login-credentials-inputs-outputs.png
[graphic7]: ./media/stream-analytics-login-credentials-inputs-outputs/7-stream-analytics-login-credentials-inputs-outputs.png
[graphic8]: ./media/stream-analytics-login-credentials-inputs-outputs/8-stream-analytics-login-credentials-inputs-outputs.png
[graphic9]: ./media/stream-analytics-login-credentials-inputs-outputs/9-stream-analytics-login-credentials-inputs-outputs.png
[graphic10]: ./media/stream-analytics-login-credentials-inputs-outputs/10-stream-analytics-login-credentials-inputs-outputs.png
[graphic11]: ./media/stream-analytics-login-credentials-inputs-outputs/11-stream-analytics-login-credentials-inputs-outputs.png
[graphic12]: ./media/stream-analytics-login-credentials-inputs-outputs/12-stream-analytics-login-credentials-inputs-outputs.png
[graphic13]: ./media/stream-analytics-login-credentials-inputs-outputs/13-stream-analytics-login-credentials-inputs-outputs.png
[graphic14]: ./media/stream-analytics-login-credentials-inputs-outputs/14-stream-analytics-login-credentials-inputs-outputs.png
[graphic15]: ./media/stream-analytics-login-credentials-inputs-outputs/15-stream-analytics-login-credentials-inputs-outputs.png
[graphic16]: ./media/stream-analytics-login-credentials-inputs-outputs/16-stream-analytics-login-credentials-inputs-outputs.png
[graphic17]: ./media/stream-analytics-login-credentials-inputs-outputs/17-stream-analytics-login-credentials-inputs-outputs.png
[graphic18]: ./media/stream-analytics-login-credentials-inputs-outputs/18-stream-analytics-login-credentials-inputs-outputs.png
[graphic19]: ./media/stream-analytics-login-credentials-inputs-outputs/19-stream-analytics-login-credentials-inputs-outputs.png
[graphic20]: ./media/stream-analytics-login-credentials-inputs-outputs/20-stream-analytics-login-credentials-inputs-outputs.png
[graphic21]: ./media/stream-analytics-login-credentials-inputs-outputs/21-stream-analytics-login-credentials-inputs-outputs.png
[graphic22]: ./media/stream-analytics-login-credentials-inputs-outputs/22-stream-analytics-login-credentials-inputs-outputs.png
[graphic23]: ./media/stream-analytics-login-credentials-inputs-outputs/23-stream-analytics-login-credentials-inputs-outputs.png
[graphic24]: ./media/stream-analytics-login-credentials-inputs-outputs/24-stream-analytics-login-credentials-inputs-outputs.png
[graphic25]: ./media/stream-analytics-login-credentials-inputs-outputs/25-stream-analytics-login-credentials-inputs-outputs.png
[graphic26]: ./media/stream-analytics-login-credentials-inputs-outputs/26-stream-analytics-login-credentials-inputs-outputs.png
[graphic27]: ./media/stream-analytics-login-credentials-inputs-outputs/27-stream-analytics-login-credentials-inputs-outputs.png
[graphic28]: ./media/stream-analytics-login-credentials-inputs-outputs/28-stream-analytics-login-credentials-inputs-outputs.png
[graphic29]: ./media/stream-analytics-login-credentials-inputs-outputs/29-stream-analytics-login-credentials-inputs-outputs.png
[graphic30]: ./media/stream-analytics-login-credentials-inputs-outputs/30-stream-analytics-login-credentials-inputs-outputs.png
[graphic31]: ./media/stream-analytics-login-credentials-inputs-outputs/31-stream-analytics-login-credentials-inputs-outputs.png
[graphic32]: ./media/stream-analytics-login-credentials-inputs-outputs/32-stream-analytics-login-credentials-inputs-outputs.png
[graphic33]: ./media/stream-analytics-login-credentials-inputs-outputs/33-stream-analytics-login-credentials-inputs-outputs.png
[graphic34]: ./media/stream-analytics-login-credentials-inputs-outputs/34-stream-analytics-login-credentials-inputs-outputs.png
[graphic35]: ./media/stream-analytics-login-credentials-inputs-outputs/35-stream-analytics-login-credentials-inputs-outputs.png
[graphic36]: ./media/stream-analytics-login-credentials-inputs-outputs/36-stream-analytics-login-credentials-inputs-outputs.png
[graphic37]: ./media/stream-analytics-login-credentials-inputs-outputs/37-stream-analytics-login-credentials-inputs-outputs.png
[graphic38]: ./media/stream-analytics-login-credentials-inputs-outputs/38-stream-analytics-login-credentials-inputs-outputs.png
[graphic39]: ./media/stream-analytics-login-credentials-inputs-outputs/39-stream-analytics-login-credentials-inputs-outputs.png
[graphic40]: ./media/stream-analytics-login-credentials-inputs-outputs/40-stream-analytics-login-credentials-inputs-outputs.png
[graphic41]: ./media/stream-analytics-login-credentials-inputs-outputs/41-stream-analytics-login-credentials-inputs-outputs.png
[graphic42]: ./media/stream-analytics-login-credentials-inputs-outputs/42-stream-analytics-login-credentials-inputs-outputs.png
[graphic43]: ./media/stream-analytics-login-credentials-inputs-outputs/43-stream-analytics-login-credentials-inputs-outputs.png

