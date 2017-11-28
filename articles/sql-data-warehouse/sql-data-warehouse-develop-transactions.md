---
title: "Az SQL Data Warehouse tranzakciók |} Microsoft Docs"
description: "Ötletek a tranzakciók az Azure SQL Data Warehouse adattárházzal történő, megoldások."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: ae621788-e575-41f5-8bfe-fa04dc4b0b53
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 29d53e18539f2c24dd64090b2ac6f9dd4c783961
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="transactions-in-sql-data-warehouse"></a><span data-ttu-id="0899a-103">Az SQL Data Warehouse-tranzakciók</span><span class="sxs-lookup"><span data-stu-id="0899a-103">Transactions in SQL Data Warehouse</span></span>
<span data-ttu-id="0899a-104">Módon teheti meg, az SQL Data Warehouse támogatja a tranzakciókat, az adatraktár-számítási feladat részeként.</span><span class="sxs-lookup"><span data-stu-id="0899a-104">As you would expect, SQL Data Warehouse supports transactions as part of the data warehouse workload.</span></span> <span data-ttu-id="0899a-105">Azonban kell biztosítják az SQL Data Warehouse teljesítményét léptékű néhány funkció korlátozva az SQL Server képest.</span><span class="sxs-lookup"><span data-stu-id="0899a-105">However, to ensure the performance of SQL Data Warehouse is maintained at scale some features are limited when compared to SQL Server.</span></span> <span data-ttu-id="0899a-106">Ez a cikk kiemeli a különbségeket, és felsorolja a többi.</span><span class="sxs-lookup"><span data-stu-id="0899a-106">This article highlights the differences and lists the others.</span></span> 

## <a name="transaction-isolation-levels"></a><span data-ttu-id="0899a-107">Tranzakció elkülönítési szinten</span><span class="sxs-lookup"><span data-stu-id="0899a-107">Transaction isolation levels</span></span>
<span data-ttu-id="0899a-108">Az SQL Data Warehouse megvalósítja az ACID-tranzakciókat.</span><span class="sxs-lookup"><span data-stu-id="0899a-108">SQL Data Warehouse implements ACID transactions.</span></span> <span data-ttu-id="0899a-109">Azonban a dokumentumos tranzakciótámogatást elkülönítését korlátozódik `READ UNCOMMITTED` és ez nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="0899a-109">However, the Isolation of the transactional support is limited to `READ UNCOMMITTED` and this cannot be changed.</span></span> <span data-ttu-id="0899a-110">Megvalósíthat számos kódolási módszerek a nem véglegesített adatokat az adatok elkerülésére, ha ez problémát jelent meg.</span><span class="sxs-lookup"><span data-stu-id="0899a-110">You can implement a number of coding methods to prevent dirty reads of data if this is a concern for you.</span></span> <span data-ttu-id="0899a-111">A legnépszerűbb módszerek megakadályozhatja, hogy a felhasználók továbbra is előkészítés alatt álló adatok lekérdezése a CTAS és a tábla partíciós váltás (gyakran más néven mozgó ablak mintát) használja.</span><span class="sxs-lookup"><span data-stu-id="0899a-111">The most popular methods leverage both CTAS and table partition switching (often known as the sliding window pattern) to prevent users from querying data that is still being prepared.</span></span> <span data-ttu-id="0899a-112">A népszerű megközelítés nézeteket, amelyek előre szűrje az adatokat is.</span><span class="sxs-lookup"><span data-stu-id="0899a-112">Views that pre-filter the data is also a popular approach.</span></span>  

## <a name="transaction-size"></a><span data-ttu-id="0899a-113">Tranzakció mérete</span><span class="sxs-lookup"><span data-stu-id="0899a-113">Transaction size</span></span>
<span data-ttu-id="0899a-114">Egyetlen módosítása tranzakció mérete korlátozott.</span><span class="sxs-lookup"><span data-stu-id="0899a-114">A single data modification transaction is limited in size.</span></span> <span data-ttu-id="0899a-115">A korlát még ma alkalmazza ") eloszlása feladatonként (".</span><span class="sxs-lookup"><span data-stu-id="0899a-115">The limit today is applied "per distribution".</span></span> <span data-ttu-id="0899a-116">Ezért az összes felosztás kerülhet sor, a korlát megszorozzuk a terjesztési száma.</span><span class="sxs-lookup"><span data-stu-id="0899a-116">Therefore, the total allocation can be calculated by multiplying the limit by the distribution count.</span></span> <span data-ttu-id="0899a-117">Hozzávetőleges tranzakcióban sorok maximális számát osztja a terjesztési kap minden egyes sorára teljes mérete.</span><span class="sxs-lookup"><span data-stu-id="0899a-117">To approximate the maximum number of rows in the transaction divide the distribution cap by the total size of each row.</span></span> <span data-ttu-id="0899a-118">A változó hosszúságú oszloppal vegye figyelembe véve az átlagos oszlop hossza, nem pedig a maximális méret használatával.</span><span class="sxs-lookup"><span data-stu-id="0899a-118">For variable length columns consider taking an average column length rather than using the maximum size.</span></span>

<span data-ttu-id="0899a-119">A következő előfeltételek az alábbi táblázatban szereplő történtek:</span><span class="sxs-lookup"><span data-stu-id="0899a-119">In the table below the following assumptions have been made:</span></span>

* <span data-ttu-id="0899a-120">Az adatok még akkor is, terjesztési történt.</span><span class="sxs-lookup"><span data-stu-id="0899a-120">An even distribution of data has occurred</span></span> 
* <span data-ttu-id="0899a-121">Az átlagos sor hossza 250 bájt</span><span class="sxs-lookup"><span data-stu-id="0899a-121">The average row length is 250 bytes</span></span>

| <span data-ttu-id="0899a-122">[DWU][DWU]</span><span class="sxs-lookup"><span data-stu-id="0899a-122">[DWU][DWU]</span></span> | <span data-ttu-id="0899a-123">Cap eloszlása (GB)</span><span class="sxs-lookup"><span data-stu-id="0899a-123">Cap per distribution (GiB)</span></span> | <span data-ttu-id="0899a-124">Azokat a Terjesztéseket száma</span><span class="sxs-lookup"><span data-stu-id="0899a-124">Number of Distributions</span></span> | <span data-ttu-id="0899a-125">Tranzakció maximális (GB)</span><span class="sxs-lookup"><span data-stu-id="0899a-125">MAX transaction size (GiB)</span></span> | <span data-ttu-id="0899a-126"># Sorok eloszlása</span><span class="sxs-lookup"><span data-stu-id="0899a-126"># Rows per distribution</span></span> | <span data-ttu-id="0899a-127">Tranzakciónként sorok maximális száma</span><span class="sxs-lookup"><span data-stu-id="0899a-127">Max Rows per transaction</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="0899a-128">DW100</span><span class="sxs-lookup"><span data-stu-id="0899a-128">DW100</span></span> |<span data-ttu-id="0899a-129">1</span><span class="sxs-lookup"><span data-stu-id="0899a-129">1</span></span> |<span data-ttu-id="0899a-130">60</span><span class="sxs-lookup"><span data-stu-id="0899a-130">60</span></span> |<span data-ttu-id="0899a-131">60</span><span class="sxs-lookup"><span data-stu-id="0899a-131">60</span></span> |<span data-ttu-id="0899a-132">4,000,000</span><span class="sxs-lookup"><span data-stu-id="0899a-132">4,000,000</span></span> |<span data-ttu-id="0899a-133">240,000,000</span><span class="sxs-lookup"><span data-stu-id="0899a-133">240,000,000</span></span> |
| <span data-ttu-id="0899a-134">DW200</span><span class="sxs-lookup"><span data-stu-id="0899a-134">DW200</span></span> |<span data-ttu-id="0899a-135">1.5</span><span class="sxs-lookup"><span data-stu-id="0899a-135">1.5</span></span> |<span data-ttu-id="0899a-136">60</span><span class="sxs-lookup"><span data-stu-id="0899a-136">60</span></span> |<span data-ttu-id="0899a-137">90</span><span class="sxs-lookup"><span data-stu-id="0899a-137">90</span></span> |<span data-ttu-id="0899a-138">6,000,000</span><span class="sxs-lookup"><span data-stu-id="0899a-138">6,000,000</span></span> |<span data-ttu-id="0899a-139">360,000,000</span><span class="sxs-lookup"><span data-stu-id="0899a-139">360,000,000</span></span> |
| <span data-ttu-id="0899a-140">DW300</span><span class="sxs-lookup"><span data-stu-id="0899a-140">DW300</span></span> |<span data-ttu-id="0899a-141">2.25</span><span class="sxs-lookup"><span data-stu-id="0899a-141">2.25</span></span> |<span data-ttu-id="0899a-142">60</span><span class="sxs-lookup"><span data-stu-id="0899a-142">60</span></span> |<span data-ttu-id="0899a-143">135</span><span class="sxs-lookup"><span data-stu-id="0899a-143">135</span></span> |<span data-ttu-id="0899a-144">9,000,000</span><span class="sxs-lookup"><span data-stu-id="0899a-144">9,000,000</span></span> |<span data-ttu-id="0899a-145">540,000,000</span><span class="sxs-lookup"><span data-stu-id="0899a-145">540,000,000</span></span> |
| <span data-ttu-id="0899a-146">DW400</span><span class="sxs-lookup"><span data-stu-id="0899a-146">DW400</span></span> |<span data-ttu-id="0899a-147">3</span><span class="sxs-lookup"><span data-stu-id="0899a-147">3</span></span> |<span data-ttu-id="0899a-148">60</span><span class="sxs-lookup"><span data-stu-id="0899a-148">60</span></span> |<span data-ttu-id="0899a-149">180</span><span class="sxs-lookup"><span data-stu-id="0899a-149">180</span></span> |<span data-ttu-id="0899a-150">12,000,000</span><span class="sxs-lookup"><span data-stu-id="0899a-150">12,000,000</span></span> |<span data-ttu-id="0899a-151">720,000,000</span><span class="sxs-lookup"><span data-stu-id="0899a-151">720,000,000</span></span> |
| <span data-ttu-id="0899a-152">DW500</span><span class="sxs-lookup"><span data-stu-id="0899a-152">DW500</span></span> |<span data-ttu-id="0899a-153">3.75</span><span class="sxs-lookup"><span data-stu-id="0899a-153">3.75</span></span> |<span data-ttu-id="0899a-154">60</span><span class="sxs-lookup"><span data-stu-id="0899a-154">60</span></span> |<span data-ttu-id="0899a-155">225</span><span class="sxs-lookup"><span data-stu-id="0899a-155">225</span></span> |<span data-ttu-id="0899a-156">15,000,000</span><span class="sxs-lookup"><span data-stu-id="0899a-156">15,000,000</span></span> |<span data-ttu-id="0899a-157">900,000,000</span><span class="sxs-lookup"><span data-stu-id="0899a-157">900,000,000</span></span> |
| <span data-ttu-id="0899a-158">DW600</span><span class="sxs-lookup"><span data-stu-id="0899a-158">DW600</span></span> |<span data-ttu-id="0899a-159">4.5</span><span class="sxs-lookup"><span data-stu-id="0899a-159">4.5</span></span> |<span data-ttu-id="0899a-160">60</span><span class="sxs-lookup"><span data-stu-id="0899a-160">60</span></span> |<span data-ttu-id="0899a-161">270</span><span class="sxs-lookup"><span data-stu-id="0899a-161">270</span></span> |<span data-ttu-id="0899a-162">18,000,000</span><span class="sxs-lookup"><span data-stu-id="0899a-162">18,000,000</span></span> |<span data-ttu-id="0899a-163">1,080,000,000</span><span class="sxs-lookup"><span data-stu-id="0899a-163">1,080,000,000</span></span> |
| <span data-ttu-id="0899a-164">DW1000</span><span class="sxs-lookup"><span data-stu-id="0899a-164">DW1000</span></span> |<span data-ttu-id="0899a-165">7.5</span><span class="sxs-lookup"><span data-stu-id="0899a-165">7.5</span></span> |<span data-ttu-id="0899a-166">60</span><span class="sxs-lookup"><span data-stu-id="0899a-166">60</span></span> |<span data-ttu-id="0899a-167">450</span><span class="sxs-lookup"><span data-stu-id="0899a-167">450</span></span> |<span data-ttu-id="0899a-168">30,000,000</span><span class="sxs-lookup"><span data-stu-id="0899a-168">30,000,000</span></span> |<span data-ttu-id="0899a-169">1,800,000,000</span><span class="sxs-lookup"><span data-stu-id="0899a-169">1,800,000,000</span></span> |
| <span data-ttu-id="0899a-170">DW1200</span><span class="sxs-lookup"><span data-stu-id="0899a-170">DW1200</span></span> |<span data-ttu-id="0899a-171">9</span><span class="sxs-lookup"><span data-stu-id="0899a-171">9</span></span> |<span data-ttu-id="0899a-172">60</span><span class="sxs-lookup"><span data-stu-id="0899a-172">60</span></span> |<span data-ttu-id="0899a-173">540</span><span class="sxs-lookup"><span data-stu-id="0899a-173">540</span></span> |<span data-ttu-id="0899a-174">36,000,000</span><span class="sxs-lookup"><span data-stu-id="0899a-174">36,000,000</span></span> |<span data-ttu-id="0899a-175">2,160,000,000</span><span class="sxs-lookup"><span data-stu-id="0899a-175">2,160,000,000</span></span> |
| <span data-ttu-id="0899a-176">DW1500</span><span class="sxs-lookup"><span data-stu-id="0899a-176">DW1500</span></span> |<span data-ttu-id="0899a-177">11.25</span><span class="sxs-lookup"><span data-stu-id="0899a-177">11.25</span></span> |<span data-ttu-id="0899a-178">60</span><span class="sxs-lookup"><span data-stu-id="0899a-178">60</span></span> |<span data-ttu-id="0899a-179">675</span><span class="sxs-lookup"><span data-stu-id="0899a-179">675</span></span> |<span data-ttu-id="0899a-180">45,000,000</span><span class="sxs-lookup"><span data-stu-id="0899a-180">45,000,000</span></span> |<span data-ttu-id="0899a-181">2,700,000,000</span><span class="sxs-lookup"><span data-stu-id="0899a-181">2,700,000,000</span></span> |
| <span data-ttu-id="0899a-182">DW2000</span><span class="sxs-lookup"><span data-stu-id="0899a-182">DW2000</span></span> |<span data-ttu-id="0899a-183">15</span><span class="sxs-lookup"><span data-stu-id="0899a-183">15</span></span> |<span data-ttu-id="0899a-184">60</span><span class="sxs-lookup"><span data-stu-id="0899a-184">60</span></span> |<span data-ttu-id="0899a-185">900</span><span class="sxs-lookup"><span data-stu-id="0899a-185">900</span></span> |<span data-ttu-id="0899a-186">60,000,000</span><span class="sxs-lookup"><span data-stu-id="0899a-186">60,000,000</span></span> |<span data-ttu-id="0899a-187">3,600,000,000</span><span class="sxs-lookup"><span data-stu-id="0899a-187">3,600,000,000</span></span> |
| <span data-ttu-id="0899a-188">DW3000</span><span class="sxs-lookup"><span data-stu-id="0899a-188">DW3000</span></span> |<span data-ttu-id="0899a-189">22.5</span><span class="sxs-lookup"><span data-stu-id="0899a-189">22.5</span></span> |<span data-ttu-id="0899a-190">60</span><span class="sxs-lookup"><span data-stu-id="0899a-190">60</span></span> |<span data-ttu-id="0899a-191">1,350</span><span class="sxs-lookup"><span data-stu-id="0899a-191">1,350</span></span> |<span data-ttu-id="0899a-192">90,000,000</span><span class="sxs-lookup"><span data-stu-id="0899a-192">90,000,000</span></span> |<span data-ttu-id="0899a-193">5,400,000,000</span><span class="sxs-lookup"><span data-stu-id="0899a-193">5,400,000,000</span></span> |
| <span data-ttu-id="0899a-194">DW6000</span><span class="sxs-lookup"><span data-stu-id="0899a-194">DW6000</span></span> |<span data-ttu-id="0899a-195">45</span><span class="sxs-lookup"><span data-stu-id="0899a-195">45</span></span> |<span data-ttu-id="0899a-196">60</span><span class="sxs-lookup"><span data-stu-id="0899a-196">60</span></span> |<span data-ttu-id="0899a-197">2,700</span><span class="sxs-lookup"><span data-stu-id="0899a-197">2,700</span></span> |<span data-ttu-id="0899a-198">180,000,000</span><span class="sxs-lookup"><span data-stu-id="0899a-198">180,000,000</span></span> |<span data-ttu-id="0899a-199">10,800,000,000</span><span class="sxs-lookup"><span data-stu-id="0899a-199">10,800,000,000</span></span> |

<span data-ttu-id="0899a-200">A tranzakció méretkorlátot tranzakció vagy műveletet alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="0899a-200">The transaction size limit is applied per transaction or operation.</span></span> <span data-ttu-id="0899a-201">Minden egyidejű tranzakciókat keresztül nem lesz alkalmazva.</span><span class="sxs-lookup"><span data-stu-id="0899a-201">It is not applied across all concurrent transactions.</span></span> <span data-ttu-id="0899a-202">Ezért a mennyiségű adatot írni a napló minden tranzakció engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="0899a-202">Therefore each transaction is permitted to write this amount of data to the log.</span></span> 

<span data-ttu-id="0899a-203">Optimalizálása és a naplóba írt adatok csökkentése érdekében tekintse meg a [tranzakciók gyakorlati tanácsok] [ Transactions best practices] cikk.</span><span class="sxs-lookup"><span data-stu-id="0899a-203">To optimize and minimize the amount of data written to the log please refer to the [Transactions best practices][Transactions best practices] article.</span></span>

> [!WARNING]
> <span data-ttu-id="0899a-204">A tranzakció maximális mérete csak érhető el, a kivonat, vagy akkor is igaz, ahol az adatok terjedésének az elosztott ROUND_ROBIN táblákat.</span><span class="sxs-lookup"><span data-stu-id="0899a-204">The maximum transaction size can only be achieved for HASH or ROUND_ROBIN distributed tables where the spread of the data is even.</span></span> <span data-ttu-id="0899a-205">Ha a tranzakció van írás a kihasználtságot módon a terjesztési majd korlát valószínű előtt a tranzakció maximális méret érhető el.</span><span class="sxs-lookup"><span data-stu-id="0899a-205">If the transaction is writing data in a skewed fashion to the distributions then the limit is likely to be reached prior to the maximum transaction size.</span></span>
> <!--REPLICATED_TABLE-->
> 
> 

## <a name="transaction-state"></a><span data-ttu-id="0899a-206">Tranzakció állapota</span><span class="sxs-lookup"><span data-stu-id="0899a-206">Transaction state</span></span>
<span data-ttu-id="0899a-207">Az SQL Data Warehouse a XACT_STATE() függvény segítségével -2 értéke hibás tranzakció jelentést.</span><span class="sxs-lookup"><span data-stu-id="0899a-207">SQL Data Warehouse uses the XACT_STATE() function to report a failed transaction using the value -2.</span></span> <span data-ttu-id="0899a-208">Ez azt jelenti, hogy a tranzakció sikertelen volt, és csak visszaállítás meg van jelölve</span><span class="sxs-lookup"><span data-stu-id="0899a-208">This means that the transaction has failed and is marked for rollback only</span></span>

> [!NOTE]
> <span data-ttu-id="0899a-209">-2, sikertelen tranzakció jelöléséhez a XACT_STATE függvény használatát az SQL Server különböző viselkedés képviseli.</span><span class="sxs-lookup"><span data-stu-id="0899a-209">The use of -2 by the XACT_STATE function to denote a failed transaction represents different behavior to SQL Server.</span></span> <span data-ttu-id="0899a-210">SQL Server vissza nem vonható véglegesítésű tranzakcióból képviselő -1 értéket használja.</span><span class="sxs-lookup"><span data-stu-id="0899a-210">SQL Server uses the value -1 to represent an un-committable transaction.</span></span> <span data-ttu-id="0899a-211">SQL Server egy tranzakción belül hibák tűri szerint vissza nem vonható véglegesítésű kell megjelölni, hogy nélkül.</span><span class="sxs-lookup"><span data-stu-id="0899a-211">SQL Server can tolerate some errors inside a transaction without it having to be marked as un-committable.</span></span> <span data-ttu-id="0899a-212">Például `SELECT 1/0` volna hibát jelez, de nem kényszerítik ki a tranzakció vissza nem vonható véglegesítésű állapotba kerülnek.</span><span class="sxs-lookup"><span data-stu-id="0899a-212">For example `SELECT 1/0` would cause an error but not force a transaction into an un-committable state.</span></span> <span data-ttu-id="0899a-213">SQL Server is lehetővé teszi az olvasási műveletek vissza nem vonható véglegesítésű a tranzakcióban.</span><span class="sxs-lookup"><span data-stu-id="0899a-213">SQL Server also permits reads in the un-committable transaction.</span></span> <span data-ttu-id="0899a-214">Azonban az SQL Data Warehouse nem teszik ezt.</span><span class="sxs-lookup"><span data-stu-id="0899a-214">However, SQL Data Warehouse does not let you do this.</span></span> <span data-ttu-id="0899a-215">Ha a hiba akkor fordul elő, az SQL Data Warehouse tranzakción belül automatikusan módba lép,-2 állapotát, és nem tudnak elvégezni további választhatja ki utasításokat, amíg az utasítás vissza lett állítva.</span><span class="sxs-lookup"><span data-stu-id="0899a-215">If an error occurs inside a SQL Data Warehouse transaction it will automatically enter the -2 state and you will not be able to make any further select statements until the statement has been rolled back.</span></span> <span data-ttu-id="0899a-216">Ezért fontos ellenőrizni, hogy az alkalmazás kódjában, hogy XACT_STATE() használ, ha Ön esetleg kód módosításokat.</span><span class="sxs-lookup"><span data-stu-id="0899a-216">It is therefore important to check that your application code to see if it uses  XACT_STATE() as you may need to make code modifications.</span></span>
> 
> 

<span data-ttu-id="0899a-217">Például az SQL Serverben láthatja a tranzakciót, amelynek a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="0899a-217">For example, in SQL Server you might see a transaction that looks like this:</span></span>

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;

        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

<span data-ttu-id="0899a-218">Ha nem adja meg a kódot, mert az újabb majd jelenik meg a következő hibaüzenet:</span><span class="sxs-lookup"><span data-stu-id="0899a-218">If you leave your code as it is above then you will get the following error message:</span></span>

<span data-ttu-id="0899a-219">Üzenet 111233, szint 16, 1, 1 111233; sor állapota Az aktuális tranzakció megszakadt, és minden függő módosításának rendelkezik visszaállításra került.</span><span class="sxs-lookup"><span data-stu-id="0899a-219">Msg 111233, Level 16, State 1, Line 1 111233;The current transaction has aborted, and any pending changes have been rolled back.</span></span> <span data-ttu-id="0899a-220">OK: A tranzakció csak visszaállítási állapotban nem kifejezetten vissza lett állítva egy DDL, DML vagy SELECT utasítás előtt.</span><span class="sxs-lookup"><span data-stu-id="0899a-220">Cause: A transaction in a rollback-only state was not explicitly rolled back before a DDL, DML or SELECT statement.</span></span>

<span data-ttu-id="0899a-221">Még nem fog ERROR_ * függvények kimenete.</span><span class="sxs-lookup"><span data-stu-id="0899a-221">You will also not get the output of the ERROR_* functions.</span></span>

<span data-ttu-id="0899a-222">Az SQL Data Warehouse a kód némileg módosítani kell:</span><span class="sxs-lookup"><span data-stu-id="0899a-222">In SQL Data Warehouse the code needs to be slightly altered:</span></span>

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

<span data-ttu-id="0899a-223">Most már a várt működést követi.</span><span class="sxs-lookup"><span data-stu-id="0899a-223">The expected behavior is now observed.</span></span> <span data-ttu-id="0899a-224">A hiba a tranzakció kezeli, és a ERROR_ * funkciók elvárt adjon meg értékeket.</span><span class="sxs-lookup"><span data-stu-id="0899a-224">The error in the transaction is managed and the ERROR_* functions provide values as expected.</span></span>

<span data-ttu-id="0899a-225">Összes megváltozott, hogy a `ROLLBACK` a tranzakció kellett az olvasás a hibaadatok előtt megtörténjen a `CATCH` blokkot.</span><span class="sxs-lookup"><span data-stu-id="0899a-225">All that has changed is that the `ROLLBACK` of the transaction had to happen before the read of the error information in the `CATCH` block.</span></span>

## <a name="errorline-function"></a><span data-ttu-id="0899a-226">Error_Line() függvény</span><span class="sxs-lookup"><span data-stu-id="0899a-226">Error_Line() function</span></span>
<span data-ttu-id="0899a-227">Akkor is érdemes megjegyezni, hogy az SQL Data Warehouse nem valósítja meg, és nem támogatja a ERROR_LINE() függvény.</span><span class="sxs-lookup"><span data-stu-id="0899a-227">It is also worth noting that SQL Data Warehouse does not implement or support the ERROR_LINE() function.</span></span> <span data-ttu-id="0899a-228">Ha ez a kódban, távolítsa el, hogy meg kell felelnie az SQL Data Warehouse kell.</span><span class="sxs-lookup"><span data-stu-id="0899a-228">If you have this in your code you will need to remove it to be compliant with SQL Data Warehouse.</span></span> <span data-ttu-id="0899a-229">A kódban lekérdezés címkék inkább megfelelő funkciók végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="0899a-229">Use query labels in your code instead to implement equivalent functionality.</span></span> <span data-ttu-id="0899a-230">Tekintse meg a [címke] [ LABEL] cikk további részleteket a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="0899a-230">Please refer to the [LABEL][LABEL] article for more details on this feature.</span></span>

## <a name="using-throw-and-raiserror"></a><span data-ttu-id="0899a-231">Segítségével THROW és RAISERROR</span><span class="sxs-lookup"><span data-stu-id="0899a-231">Using THROW and RAISERROR</span></span>
<span data-ttu-id="0899a-232">THROW az SQL Data Warehouse kivételt váltson ki több modern végrehajtására, de a RAISERROR is támogatott.</span><span class="sxs-lookup"><span data-stu-id="0899a-232">THROW is the more modern implementation for raising exceptions in SQL Data Warehouse but RAISERROR is also supported.</span></span> <span data-ttu-id="0899a-233">Néhány dologban is érdemes figyelmet kell fordítani azonban fizet.</span><span class="sxs-lookup"><span data-stu-id="0899a-233">There are a few differences that are worth paying attention to however.</span></span>

* <span data-ttu-id="0899a-234">Felhasználó által definiált hibaüzenetek számok nem lehet THROW 100 000-150 000 tartományán</span><span class="sxs-lookup"><span data-stu-id="0899a-234">User defined error messages numbers cannot be in the 100,000 - 150,000 range for THROW</span></span>
* <span data-ttu-id="0899a-235">RAISERROR hibaüzenetek 50 000 van rögzítve.</span><span class="sxs-lookup"><span data-stu-id="0899a-235">RAISERROR error messages are fixed at 50,000</span></span>
* <span data-ttu-id="0899a-236">Ilyen hibaszámú használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="0899a-236">Use of sys.messages is not supported</span></span>

## <a name="limitiations"></a><span data-ttu-id="0899a-237">Limitiations</span><span class="sxs-lookup"><span data-stu-id="0899a-237">Limitiations</span></span>
<span data-ttu-id="0899a-238">Az SQL Data Warehouse rendelkezik néhány egyéb korlátozások is tranzakciók.</span><span class="sxs-lookup"><span data-stu-id="0899a-238">SQL Data Warehouse does have a few other restrictions that relate to transactions.</span></span>

<span data-ttu-id="0899a-239">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="0899a-239">They are as follows:</span></span>

* <span data-ttu-id="0899a-240">Elosztott tranzakciók</span><span class="sxs-lookup"><span data-stu-id="0899a-240">No distributed transactions</span></span>
* <span data-ttu-id="0899a-241">Nem engedélyezett beágyazott tranzakciók</span><span class="sxs-lookup"><span data-stu-id="0899a-241">No nested transactions permitted</span></span>
* <span data-ttu-id="0899a-242">Nem engedélyezett pontokon menthet</span><span class="sxs-lookup"><span data-stu-id="0899a-242">No save points allowed</span></span>
* <span data-ttu-id="0899a-243">Elnevezett tranzakciók</span><span class="sxs-lookup"><span data-stu-id="0899a-243">No named transactions</span></span>
* <span data-ttu-id="0899a-244">Nem jelölt tranzakciók</span><span class="sxs-lookup"><span data-stu-id="0899a-244">No marked transactions</span></span>
* <span data-ttu-id="0899a-245">Nem támogatott például DDL `CREATE TABLE` belül a felhasználó definiált tranzakció</span><span class="sxs-lookup"><span data-stu-id="0899a-245">No support for DDL such as `CREATE TABLE` inside a user defined transaction</span></span>

## <a name="next-steps"></a><span data-ttu-id="0899a-246">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0899a-246">Next steps</span></span>
<span data-ttu-id="0899a-247">Tranzakciók optimalizálása kapcsolatos további információkért lásd: [tranzakciók gyakorlati tanácsok][Transactions best practices].</span><span class="sxs-lookup"><span data-stu-id="0899a-247">To learn more about optimizing transactions, see [Transactions best practices][Transactions best practices].</span></span>  <span data-ttu-id="0899a-248">Az SQL Data Warehouse gyakorlati tanácsokat kapcsolatban lásd: [gyakorlati tanácsok az SQL Data Warehouse][SQL Data Warehouse best practices].</span><span class="sxs-lookup"><span data-stu-id="0899a-248">To learn about other SQL Data Warehouse best practices, see [SQL Data Warehouse best practices][SQL Data Warehouse best practices].</span></span>

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[development overview]: ./sql-data-warehouse-overview-develop.md
[Transactions best practices]: ./sql-data-warehouse-develop-best-practices-transactions.md
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[LABEL]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->

<!--Other Web references-->
