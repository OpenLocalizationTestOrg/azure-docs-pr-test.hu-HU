---
title: "A Stream Analytics: Elforgatása napló hitelesítő adataival bemenetekhez és kimenetekhez |} Microsoft Docs"
description: "Ismerje meg, hogyan tooupdate hello hitelesítő adatait, a Stream Analytics bemenetekhez és kimenetekhez."
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
ms.openlocfilehash: ac2374c539012b66ab390656c5750024e02f6bdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="rotate-login-credentials-for-inputs-and-outputs-in-stream-analytics-jobs"></a><span data-ttu-id="aa3df-104">Forgassa el a be- és kimenetekkel a Stream Analytics-feladatok bejelentkezési hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="aa3df-104">Rotate login credentials for inputs and outputs in Stream Analytics Jobs</span></span>
## <a name="abstract"></a><span data-ttu-id="aa3df-105">Absztrakt</span><span class="sxs-lookup"><span data-stu-id="aa3df-105">Abstract</span></span>
<span data-ttu-id="aa3df-106">Az Azure Stream Analytics jelenleg nem engedélyezi egy bemeneti/kimeneti hello hitelesítő adatok cseréjének hello feladat futása közben.</span><span class="sxs-lookup"><span data-stu-id="aa3df-106">Azure Stream Analytics today doesn’t allow replacing hello credentials on an input/output while hello job is running.</span></span>

<span data-ttu-id="aa3df-107">Azure Stream Analytics nem támogatja az utolsó kimenetből egy feladat folytatása, amíg meg akartunk tooshare hello teljes folyamatot a minimalizálja a hello lag hello leállítása és hello feladat elindítása és elforgatása hello bejelentkezési hitelesítő adatok között.</span><span class="sxs-lookup"><span data-stu-id="aa3df-107">While Azure Stream Analytics does support resuming a job from last output, we wanted tooshare hello entire process for minimizing hello lag between hello stopping and starting of hello job and rotating hello login credentials.</span></span>

## <a name="part-1---prepare-hello-new-set-of-credentials"></a><span data-ttu-id="aa3df-108">1. rész – hello új hitelesítőadat-készletet előkészítése:</span><span class="sxs-lookup"><span data-stu-id="aa3df-108">Part 1 - Prepare hello new set of credentials:</span></span>
<span data-ttu-id="aa3df-109">Ebben a részben a következő bemenetek/kimenetek alkalmazható toohello:</span><span class="sxs-lookup"><span data-stu-id="aa3df-109">This part is applicable toohello following inputs/outputs:</span></span>

* <span data-ttu-id="aa3df-110">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="aa3df-110">Blob Storage</span></span>
* <span data-ttu-id="aa3df-111">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="aa3df-111">Event Hubs</span></span>
* <span data-ttu-id="aa3df-112">SQL Database</span><span class="sxs-lookup"><span data-stu-id="aa3df-112">SQL Database</span></span>
* <span data-ttu-id="aa3df-113">Table Storage</span><span class="sxs-lookup"><span data-stu-id="aa3df-113">Table Storage</span></span>

<span data-ttu-id="aa3df-114">Az egyéb bemenetek/kimenetek folytassa a 2. rész.</span><span class="sxs-lookup"><span data-stu-id="aa3df-114">For other inputs/outputs, proceed with Part 2.</span></span>

### <a name="blob-storagetable-storage"></a><span data-ttu-id="aa3df-115">A BLOB storage-tároló vagy tábla</span><span class="sxs-lookup"><span data-stu-id="aa3df-115">Blob storage/Table storage</span></span>
1. <span data-ttu-id="aa3df-116">Nyissa meg a toohello tárolási kiterjesztés hello Azure Management Portal:</span><span class="sxs-lookup"><span data-stu-id="aa3df-116">Go toohello Storage extention on hello Azure Management portal:</span></span>  
   ![graphic1][graphic1]
2. <span data-ttu-id="aa3df-118">Keresse meg a feladat által használt hello tárolási, és lépjen be:</span><span class="sxs-lookup"><span data-stu-id="aa3df-118">Locate hello storage used by your job and go into it:</span></span>  
   ![graphic2][graphic2]
3. <span data-ttu-id="aa3df-120">Kattintson a hello elérési kulcsok kezelése parancsot:</span><span class="sxs-lookup"><span data-stu-id="aa3df-120">Click hello Manage Access Keys command:</span></span>  
   ![graphic3][graphic3]
4. <span data-ttu-id="aa3df-122">Hello elsődleges elérési kulcsot és a másodlagos elérési kulcsot, hello közötti **válasszon egyet a feladat nem használja hello**.</span><span class="sxs-lookup"><span data-stu-id="aa3df-122">Between hello Primary Access Key and hello Secondary Access Key, **pick hello one not used by your job**.</span></span>
5. <span data-ttu-id="aa3df-123">Találati újragenerálása:</span><span class="sxs-lookup"><span data-stu-id="aa3df-123">Hit regenerate:</span></span>  
   ![graphic4][graphic4]
6. <span data-ttu-id="aa3df-125">Másolja az újonnan létrehozott hello kulcsot:</span><span class="sxs-lookup"><span data-stu-id="aa3df-125">Copy hello newly generated key:</span></span>  
   ![graphic5][graphic5]
7. <span data-ttu-id="aa3df-127">Továbbra is tooPart 2.</span><span class="sxs-lookup"><span data-stu-id="aa3df-127">Continue tooPart 2.</span></span>

### <a name="event-hubs"></a><span data-ttu-id="aa3df-128">Az Event hubs</span><span class="sxs-lookup"><span data-stu-id="aa3df-128">Event hubs</span></span>
1. <span data-ttu-id="aa3df-129">Nyissa meg a toohello Service Bus bővítmény hello Azure Management Portal:</span><span class="sxs-lookup"><span data-stu-id="aa3df-129">Go toohello Service Bus extension on hello Azure Management portal:</span></span>  
   ![graphic6][graphic6]
2. <span data-ttu-id="aa3df-131">Keresse meg a Service Bus Namespace a feladat által használt hello, és lépjen be:</span><span class="sxs-lookup"><span data-stu-id="aa3df-131">Locate hello Service Bus Namespace used by your job and go into it:</span></span>  
   ![graphic7][graphic7]
3. <span data-ttu-id="aa3df-133">Ha a feladat egy megosztott elérési házirendet használ a Service Bus Namespace hello, jump toostep 6</span><span class="sxs-lookup"><span data-stu-id="aa3df-133">If your job uses a shared access policy on hello Service Bus Namespace, jump toostep 6</span></span>  
4. <span data-ttu-id="aa3df-134">Nyissa meg toohello Event Hubs lapon:</span><span class="sxs-lookup"><span data-stu-id="aa3df-134">Go toohello Event Hubs tab:</span></span>  
   ![graphic8][graphic8]
5. <span data-ttu-id="aa3df-136">Keresse meg a feladat által használt Eseményközpont hello, és lépjen be:</span><span class="sxs-lookup"><span data-stu-id="aa3df-136">Locate hello Event Hub used by your job and go into it:</span></span>  
   ![graphic9][graphic9]
6. <span data-ttu-id="aa3df-138">Nyissa meg toohello konfigurálása lapon:</span><span class="sxs-lookup"><span data-stu-id="aa3df-138">Go toohello Configure Tab:</span></span>  
   ![graphic10][graphic10]
7. <span data-ttu-id="aa3df-140">Házirend neve legördülő hello keresse meg a feladat által használt megosztott hello házirend:</span><span class="sxs-lookup"><span data-stu-id="aa3df-140">On hello Policy Name drop-down, locate hello shared access policy used by your job:</span></span>  
   ![graphic11][graphic11]
8. <span data-ttu-id="aa3df-142">Hello elsődleges kulcs és a másodlagos kulcs hello közötti **válasszon egyet a feladat nem használja hello**.</span><span class="sxs-lookup"><span data-stu-id="aa3df-142">Between hello Primary Key and hello Secondary Key, **pick hello one not used by your job**.</span></span>  
9. <span data-ttu-id="aa3df-143">Találati újragenerálása:</span><span class="sxs-lookup"><span data-stu-id="aa3df-143">Hit regenerate:</span></span>  
   ![graphic12][graphic12]
10. <span data-ttu-id="aa3df-145">Másolja az újonnan létrehozott hello kulcsot:</span><span class="sxs-lookup"><span data-stu-id="aa3df-145">Copy hello newly generated key:</span></span>  
   ![graphic13][graphic13]
11. <span data-ttu-id="aa3df-147">Továbbra is tooPart 2.</span><span class="sxs-lookup"><span data-stu-id="aa3df-147">Continue tooPart 2.</span></span>  

### <a name="sql-database"></a><span data-ttu-id="aa3df-148">SQL Database</span><span class="sxs-lookup"><span data-stu-id="aa3df-148">SQL Database</span></span>
> [!NOTE]
> <span data-ttu-id="aa3df-149">Megjegyzés: szüksége lesz a tooconnect toohello SQL adatbázis-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="aa3df-149">Note: you will need tooconnect toohello SQL Database Service.</span></span> <span data-ttu-id="aa3df-150">Tooshow hogyan toodo a használatával hello egyszerűbben kezelhető a hello Azure felügyeleti portálon, de úgy is dönthet, toouse egy ügyféloldali eszközzel például SQL Server Management Studio alkalmazást, valamint fogjuk.</span><span class="sxs-lookup"><span data-stu-id="aa3df-150">We are going tooshow how toodo this using hello management experience on hello Azure Management portal but you may choose toouse some client-side tool such as SQL Server Management Studio as well.</span></span>
>
> 

1. <span data-ttu-id="aa3df-151">Nyissa meg a toohello SQL-adatbázis bővítmény hello Azure Management Portal:</span><span class="sxs-lookup"><span data-stu-id="aa3df-151">Go toohello SQL Databases extension on hello Azure Management portal:</span></span>  
   ![graphic14][graphic14]
2. <span data-ttu-id="aa3df-153">Keresse meg a feladat által használt SQL-adatbázis hello és **hello kiszolgálón kattintson** menüjének hello azonos sorban:</span><span class="sxs-lookup"><span data-stu-id="aa3df-153">Locate hello SQL Database used by your job and **click on hello server** link on hello same line:</span></span>  
   <span data-ttu-id="aa3df-154">![graphic15][graphic15]</span><span class="sxs-lookup"><span data-stu-id="aa3df-154">![graphic15][graphic15]</span></span>
3. <span data-ttu-id="aa3df-155">Kattintson a hello kezelése parancsot:</span><span class="sxs-lookup"><span data-stu-id="aa3df-155">Click hello Manage command:</span></span>  
   ![graphic16][graphic16]
4. <span data-ttu-id="aa3df-157">Adatbázis fő típusa:</span><span class="sxs-lookup"><span data-stu-id="aa3df-157">Type Database Master:</span></span>  
   ![graphic17][graphic17]
5. <span data-ttu-id="aa3df-159">Írja be a felhasználónevét és jelszavát, és kattintson a napló a:</span><span class="sxs-lookup"><span data-stu-id="aa3df-159">Type in your User Name, Password, and click Log on:</span></span>  
   ![graphic18][graphic18]
6. <span data-ttu-id="aa3df-161">Kattintson az új lekérdezés:</span><span class="sxs-lookup"><span data-stu-id="aa3df-161">Click New Query:</span></span>  
   ![graphic19][graphic19]
7. <span data-ttu-id="aa3df-163">Típus a következő a lekérdezést a < login_name > cseréje a felhasználónévvel és lecserél hello <enterStrongPasswordHere> az új jelszóval:</span><span class="sxs-lookup"><span data-stu-id="aa3df-163">Type in hello following query replacing <login_name> with your User Name and replacing <enterStrongPasswordHere> with your new password:</span></span>  
   `CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8. <span data-ttu-id="aa3df-164">Kattintson a Futtatás gombra:</span><span class="sxs-lookup"><span data-stu-id="aa3df-164">Click Run:</span></span>  
   ![graphic20][graphic20]
9. <span data-ttu-id="aa3df-166">Lépjen vissza toostep 2, és most kattintson hello adatbázis:</span><span class="sxs-lookup"><span data-stu-id="aa3df-166">Go back toostep 2 and this time click hello database:</span></span>  
   ![graphic21][graphic21]
10. <span data-ttu-id="aa3df-168">Kattintson a hello kezelése parancsot:</span><span class="sxs-lookup"><span data-stu-id="aa3df-168">Click hello Manage command:</span></span>  
   ![graphic22][graphic22]
11. <span data-ttu-id="aa3df-170">Írja be a felhasználónevét és jelszavát, és kattintson a bejelentkezés:</span><span class="sxs-lookup"><span data-stu-id="aa3df-170">type in your User Name, Password, and click Logon:</span></span>  
   ![graphic23][graphic23]
12. <span data-ttu-id="aa3df-172">Kattintson az új lekérdezés:</span><span class="sxs-lookup"><span data-stu-id="aa3df-172">Click New Query:</span></span>  
   ![graphic24][graphic24]
13. <span data-ttu-id="aa3df-174">Írja be a következő a lekérdezés < felhasználónév > cseréje egy névvel, amellyel tooidentify kíván hello ehhez a bejelentkezéshez, az adatbázis környezetében hello (biztosíthat hello ugyanazt az értéket < login_name >, például adott) és < login_name > cseréje az új felhasználónevet:</span><span class="sxs-lookup"><span data-stu-id="aa3df-174">Type in hello following query replacing <user_name> with a name by which you want tooidentify this login in hello context of this database (you can provide hello same value you gave for <login_name>, for example) and replacing <login_name> with your new user name:</span></span>  
   `CREATE USER <user_name> FROM LOGIN <login_name>`
14. <span data-ttu-id="aa3df-175">Kattintson a Futtatás gombra:</span><span class="sxs-lookup"><span data-stu-id="aa3df-175">Click Run:</span></span>  
   ![graphic25][graphic25]
15. <span data-ttu-id="aa3df-177">Most kell adni az új felhasználó hello azonos szerepkörrel és az eredeti felhasználó rendelkezik-e jogosultsággal.</span><span class="sxs-lookup"><span data-stu-id="aa3df-177">You should now provide your new user with hello same roles and privileges your original user had.</span></span>
16. <span data-ttu-id="aa3df-178">Továbbra is tooPart 2.</span><span class="sxs-lookup"><span data-stu-id="aa3df-178">Continue tooPart 2.</span></span>

## <a name="part-2-stopping-hello-stream-analytics-job"></a><span data-ttu-id="aa3df-179">2. lépés: Leállításával hello Stream Analytics-feladat</span><span class="sxs-lookup"><span data-stu-id="aa3df-179">Part 2: Stopping hello Stream Analytics Job</span></span>
1. <span data-ttu-id="aa3df-180">Nyissa meg a toohello Stream Analytics bővítmény hello Azure Management Portal:</span><span class="sxs-lookup"><span data-stu-id="aa3df-180">Go toohello Stream Analytics extension on hello Azure Management portal:</span></span>  
   ![graphic26][graphic26]
2. <span data-ttu-id="aa3df-182">Keresse meg a feladat, és lépjen be:</span><span class="sxs-lookup"><span data-stu-id="aa3df-182">Locate your job and go into it:</span></span>  
   ![graphic27][graphic27]
3. <span data-ttu-id="aa3df-184">Nyissa meg a toohello bemeneti adatokat vagy hello kimenetek lapon e hello hitelesítő adatok bemenettel vagy kimenettel vannak elforgatása alapján.</span><span class="sxs-lookup"><span data-stu-id="aa3df-184">Go toohello Inputs tab or hello Outputs tab based on whether you are rotating hello credentials on an Input or on an Output.</span></span>  
   ![graphic28][graphic28]
4. <span data-ttu-id="aa3df-186">Kattintson a hello leállítási parancsot, és ellenőrizze a hello feladat leállt:</span><span class="sxs-lookup"><span data-stu-id="aa3df-186">Click hello Stop command and confirm hello job has stopped:</span></span>  
   <span data-ttu-id="aa3df-187">![graphic29][graphic29] hello feladat toostop várja.</span><span class="sxs-lookup"><span data-stu-id="aa3df-187">![graphic29][graphic29] Wait for hello job toostop.</span></span>
5. <span data-ttu-id="aa3df-188">Keresse meg a kívánt toorotate hitelesítő adatok meg, és nyissa meg azt az hello-bemeneti/kimeneti:</span><span class="sxs-lookup"><span data-stu-id="aa3df-188">Locate hello input/output you want toorotate credentials on and go into it:</span></span>  
   ![graphic30][graphic30]
6. <span data-ttu-id="aa3df-190">A folytatáshoz tooPart 3.</span><span class="sxs-lookup"><span data-stu-id="aa3df-190">Proceed tooPart 3.</span></span>

## <a name="part-3-editing-hello-credentials-on-hello-stream-analytics-job"></a><span data-ttu-id="aa3df-191">3. rész: Szerkesztés hello-felhasználó hitelesítő adatai hello Stream Analytics-feladat</span><span class="sxs-lookup"><span data-stu-id="aa3df-191">Part 3: Editing hello credentials on hello Stream Analytics Job</span></span>
### <a name="blob-storagetable-storage"></a><span data-ttu-id="aa3df-192">A BLOB storage-tároló vagy tábla</span><span class="sxs-lookup"><span data-stu-id="aa3df-192">Blob storage/Table storage</span></span>
1. <span data-ttu-id="aa3df-193">Hello Tárfiók kulcsa mező található, és az újonnan létrehozott kulcs beillesztése:</span><span class="sxs-lookup"><span data-stu-id="aa3df-193">Find hello Storage Account Key field and paste your newly generated key into it:</span></span>  
   ![graphic31][graphic31]
2. <span data-ttu-id="aa3df-195">Hello a Mentés gombra, és ellenőrizze, hogy a módosítások mentése:</span><span class="sxs-lookup"><span data-stu-id="aa3df-195">Click hello Save command and confirm saving your changes:</span></span>  
   ![graphic32][graphic32]
3. <span data-ttu-id="aa3df-197">A kapcsolat tesztelése automatikusan elindul, amikor menti a módosításokat, győződjön meg arról, amely sikeresen megfelelt.</span><span class="sxs-lookup"><span data-stu-id="aa3df-197">A connection test will automatically start when you save your changes, make sure that is has successfully passed.</span></span>
4. <span data-ttu-id="aa3df-198">A folytatáshoz tooPart 4.</span><span class="sxs-lookup"><span data-stu-id="aa3df-198">Proceed tooPart 4.</span></span>

### <a name="event-hubs"></a><span data-ttu-id="aa3df-199">Az Event hubs</span><span class="sxs-lookup"><span data-stu-id="aa3df-199">Event hubs</span></span>
1. <span data-ttu-id="aa3df-200">Hello Event Hub házirend kulcs mezőjében keresse meg és az újonnan létrehozott kulcs beillesztése:</span><span class="sxs-lookup"><span data-stu-id="aa3df-200">Find hello Event Hub Policy Key field and paste your newly generated key into it:</span></span>  
   ![graphic33][graphic33]
2. <span data-ttu-id="aa3df-202">Hello a Mentés gombra, és ellenőrizze, hogy a módosítások mentése:</span><span class="sxs-lookup"><span data-stu-id="aa3df-202">Click hello Save command and confirm saving your changes:</span></span>  
   ![graphic34][graphic34]
3. <span data-ttu-id="aa3df-204">A kapcsolat tesztelése automatikusan elindul, amikor menti a módosításokat, győződjön meg arról, hogy azt sikeresen megfelelt.</span><span class="sxs-lookup"><span data-stu-id="aa3df-204">A connection test will automatically start when you save your changes, make sure that it has successfully passed.</span></span>
4. <span data-ttu-id="aa3df-205">A folytatáshoz tooPart 4.</span><span class="sxs-lookup"><span data-stu-id="aa3df-205">Proceed tooPart 4.</span></span>

### <a name="power-bi"></a><span data-ttu-id="aa3df-206">Power BI</span><span class="sxs-lookup"><span data-stu-id="aa3df-206">Power BI</span></span>
1. <span data-ttu-id="aa3df-207">Kattintson a megújítás engedélyezési hello:</span><span class="sxs-lookup"><span data-stu-id="aa3df-207">Click hello Renew authorization:</span></span>  

   ![graphic35][graphic35]
2. <span data-ttu-id="aa3df-209">A következő megerősítő hello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="aa3df-209">You will get hello following confirmation:</span></span>  

   ![graphic36][graphic36]
3. <span data-ttu-id="aa3df-211">Hello a Mentés gombra, és ellenőrizze, hogy a módosítások mentése:</span><span class="sxs-lookup"><span data-stu-id="aa3df-211">Click hello Save command and confirm saving your changes:</span></span>  
   ![graphic37][graphic37]
4. <span data-ttu-id="aa3df-213">A kapcsolat tesztelése automatikusan elindul, amikor a módosítások mentése, győződjön meg arról, hogy sikeresen megfelelt.</span><span class="sxs-lookup"><span data-stu-id="aa3df-213">A connection test will automatically start when you save your changes, make sure it has successfully passed.</span></span>
5. <span data-ttu-id="aa3df-214">A folytatáshoz tooPart 4.</span><span class="sxs-lookup"><span data-stu-id="aa3df-214">Proceed tooPart 4.</span></span>

### <a name="sql-database"></a><span data-ttu-id="aa3df-215">SQL Database</span><span class="sxs-lookup"><span data-stu-id="aa3df-215">SQL Database</span></span>
1. <span data-ttu-id="aa3df-216">Hello felhasználónév és a jelszó mező található, és illessze be az újonnan létrehozott hitelesítőadat-készletet a őket:</span><span class="sxs-lookup"><span data-stu-id="aa3df-216">Find hello User Name and Password fields and paste your newly created set of credentials into them:</span></span>  
   ![graphic38][graphic38]
2. <span data-ttu-id="aa3df-218">Hello a Mentés gombra, és ellenőrizze, hogy a módosítások mentése:</span><span class="sxs-lookup"><span data-stu-id="aa3df-218">Click hello Save command and confirm saving your changes:</span></span>  
   ![graphic39][graphic39]
3. <span data-ttu-id="aa3df-220">A kapcsolat tesztelése automatikusan elindul, amikor menti a módosításokat, győződjön meg arról, hogy azt sikeresen megfelelt.</span><span class="sxs-lookup"><span data-stu-id="aa3df-220">A connection test will automatically start when you save your changes, make sure that it has successfully passed.</span></span>  
4. <span data-ttu-id="aa3df-221">A folytatáshoz tooPart 4.</span><span class="sxs-lookup"><span data-stu-id="aa3df-221">Proceed tooPart 4.</span></span>

## <a name="part-4-starting-your-job-from-last-stopped-time"></a><span data-ttu-id="aa3df-222">4. lépés: A feladat kiindulva leállított legutóbbi</span><span class="sxs-lookup"><span data-stu-id="aa3df-222">Part 4: Starting your job from last stopped time</span></span>
1. <span data-ttu-id="aa3df-223">Hello bemeneti/kimeneti elhagyni:</span><span class="sxs-lookup"><span data-stu-id="aa3df-223">Navigate away from hello Input/Output:</span></span>  
   ![graphic40][graphic40]
2. <span data-ttu-id="aa3df-225">Kattintson a hello indítási parancsot:</span><span class="sxs-lookup"><span data-stu-id="aa3df-225">Click hello Start command:</span></span>  
   ![graphic41][graphic41]
3. <span data-ttu-id="aa3df-227">Válassza ki a hello feladat utolsó befejezési időpontja, és kattintson az OK gombra:</span><span class="sxs-lookup"><span data-stu-id="aa3df-227">Pick hello Last Stopped Time and click OK:</span></span>  
   ![graphic42][graphic42]
4. <span data-ttu-id="aa3df-229">A folytatáshoz tooPart 5.</span><span class="sxs-lookup"><span data-stu-id="aa3df-229">Proceed tooPart 5.</span></span>  

## <a name="part-5-removing-hello-old-set-of-credentials"></a><span data-ttu-id="aa3df-230">5. lépés: Eltávolítása hello régi hitelesítőadat-készletet</span><span class="sxs-lookup"><span data-stu-id="aa3df-230">Part 5: Removing hello old set of credentials</span></span>
<span data-ttu-id="aa3df-231">Ebben a részben a következő bemenetek/kimenetek alkalmazható toohello:</span><span class="sxs-lookup"><span data-stu-id="aa3df-231">This part is applicable toohello following inputs/outputs:</span></span>

* <span data-ttu-id="aa3df-232">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="aa3df-232">Blob Storage</span></span>
* <span data-ttu-id="aa3df-233">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="aa3df-233">Event Hubs</span></span>
* <span data-ttu-id="aa3df-234">SQL Database</span><span class="sxs-lookup"><span data-stu-id="aa3df-234">SQL Database</span></span>
* <span data-ttu-id="aa3df-235">Table Storage</span><span class="sxs-lookup"><span data-stu-id="aa3df-235">Table Storage</span></span>

### <a name="blob-storagetable-storage"></a><span data-ttu-id="aa3df-236">A BLOB storage-tároló vagy tábla</span><span class="sxs-lookup"><span data-stu-id="aa3df-236">Blob storage/Table storage</span></span>
<span data-ttu-id="aa3df-237">Ismételje meg az 1. rész hello hozzáférési kulcsot a feladat által korábban használt toorenew hello most már nem használt a hozzáférési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="aa3df-237">Repeat Part 1 for hello Access Key that was previously used by your job toorenew hello now unused Access Key.</span></span>

### <a name="event-hubs"></a><span data-ttu-id="aa3df-238">Az Event hubs</span><span class="sxs-lookup"><span data-stu-id="aa3df-238">Event hubs</span></span>
<span data-ttu-id="aa3df-239">Ismételje meg az 1. rész hello a feladat által korábban használt kulcs toorenew hello most már nem használt kulcs.</span><span class="sxs-lookup"><span data-stu-id="aa3df-239">Repeat Part 1 for hello Key that was previously used by your job toorenew hello now unused Key.</span></span>

### <a name="sql-database"></a><span data-ttu-id="aa3df-240">SQL Database</span><span class="sxs-lookup"><span data-stu-id="aa3df-240">SQL Database</span></span>
1. <span data-ttu-id="aa3df-241">Lépjen vissza toohello lekérdezési ablakba rész 1 7. lépés, és írja be a következő lekérdezést, < previous_login_name > lecserélését hello felhasználónevet, a feladat által korábban használt hello:</span><span class="sxs-lookup"><span data-stu-id="aa3df-241">Go back toohello query window from Part 1 Step 7 and type in hello following query, replacing <previous_login_name> with hello User Name that was previously used by your job:</span></span>  
   `DROP LOGIN <previous_login_name>`  
2. <span data-ttu-id="aa3df-242">Kattintson a Futtatás gombra:</span><span class="sxs-lookup"><span data-stu-id="aa3df-242">Click Run:</span></span>  
   ![graphic43][graphic43]  

<span data-ttu-id="aa3df-244">A következő megerősítő hello szerezheti be:</span><span class="sxs-lookup"><span data-stu-id="aa3df-244">You should get hello following confirmation:</span></span> 

    Command(s) completed successfully.

## <a name="get-help"></a><span data-ttu-id="aa3df-245">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="aa3df-245">Get help</span></span>
<span data-ttu-id="aa3df-246">További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="aa3df-246">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="aa3df-247">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aa3df-247">Next steps</span></span>
* [<span data-ttu-id="aa3df-248">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="aa3df-248">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="aa3df-249">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="aa3df-249">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="aa3df-250">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="aa3df-250">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="aa3df-251">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="aa3df-251">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="aa3df-252">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="aa3df-252">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

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

