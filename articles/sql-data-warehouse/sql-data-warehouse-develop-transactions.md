---
title: az SQL Data Warehouse aaaTransactions |} Microsoft Docs
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
ms.openlocfilehash: 7c541648553238443b407666612561918096eb61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="transactions-in-sql-data-warehouse"></a><span data-ttu-id="4fa2b-103">Az SQL Data Warehouse-tranzakciók</span><span class="sxs-lookup"><span data-stu-id="4fa2b-103">Transactions in SQL Data Warehouse</span></span>
<span data-ttu-id="4fa2b-104">Módon teheti meg, az SQL Data Warehouse hello adatraktár-számítási feladat részeként támogatja a tranzakciókat.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-104">As you would expect, SQL Data Warehouse supports transactions as part of hello data warehouse workload.</span></span> <span data-ttu-id="4fa2b-105">Azonban tooensure hello teljesítmény az SQL Data Warehouse megmarad, egyes funkciók korlátozva, ha léptékű összehasonlított tooSQL kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-105">However, tooensure hello performance of SQL Data Warehouse is maintained at scale some features are limited when compared tooSQL Server.</span></span> <span data-ttu-id="4fa2b-106">Ez a cikk kiemeli a hello különbségeket, és listák hello mások.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-106">This article highlights hello differences and lists hello others.</span></span> 

## <a name="transaction-isolation-levels"></a><span data-ttu-id="4fa2b-107">Tranzakció elkülönítési szinten</span><span class="sxs-lookup"><span data-stu-id="4fa2b-107">Transaction isolation levels</span></span>
<span data-ttu-id="4fa2b-108">Az SQL Data Warehouse megvalósítja az ACID-tranzakciókat.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-108">SQL Data Warehouse implements ACID transactions.</span></span> <span data-ttu-id="4fa2b-109">Azonban hello hello tranzakciós támogatás elkülönítési korlátozódik túl`READ UNCOMMITTED` és ez nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-109">However, hello Isolation of hello transactional support is limited too`READ UNCOMMITTED` and this cannot be changed.</span></span> <span data-ttu-id="4fa2b-110">Megvalósíthat számos programozási módszer inkonzisztencia tooprevent adatok olvassa be, ha ez problémát jelent meg.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-110">You can implement a number of coding methods tooprevent dirty reads of data if this is a concern for you.</span></span> <span data-ttu-id="4fa2b-111">hello legnépszerűbb módszerek kihasználja CTAS és a tábla partíciós váltás (gyakran más néven mozgó ablak mintát hello) tooprevent felhasználók továbbra is előkészítés alatt álló adatok lekérdezésére.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-111">hello most popular methods leverage both CTAS and table partition switching (often known as hello sliding window pattern) tooprevent users from querying data that is still being prepared.</span></span> <span data-ttu-id="4fa2b-112">Győződjön meg arról, hogy előtti szűrő hello adatokat is a népszerű megközelítés nézetek.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-112">Views that pre-filter hello data is also a popular approach.</span></span>  

## <a name="transaction-size"></a><span data-ttu-id="4fa2b-113">Tranzakció mérete</span><span class="sxs-lookup"><span data-stu-id="4fa2b-113">Transaction size</span></span>
<span data-ttu-id="4fa2b-114">Egyetlen módosítása tranzakció mérete korlátozott.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-114">A single data modification transaction is limited in size.</span></span> <span data-ttu-id="4fa2b-115">hello korlát ma alkalmazza ") eloszlása feladatonként (".</span><span class="sxs-lookup"><span data-stu-id="4fa2b-115">hello limit today is applied "per distribution".</span></span> <span data-ttu-id="4fa2b-116">Ezért hello teljes foglalási kiszámítható hello korlát megszorozzuk hello terjesztési száma.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-116">Therefore, hello total allocation can be calculated by multiplying hello limit by hello distribution count.</span></span> <span data-ttu-id="4fa2b-117">tooapproximate hello sorok maximális számát hello tranzakcióban hello terjesztési cap nullával való minden egyes sorára hello teljes mérete.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-117">tooapproximate hello maximum number of rows in hello transaction divide hello distribution cap by hello total size of each row.</span></span> <span data-ttu-id="4fa2b-118">A változó hosszúságú oszloppal vegye figyelembe véve az átlagos oszlop hossza, nem pedig hello maximális méret használatával.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-118">For variable length columns consider taking an average column length rather than using hello maximum size.</span></span>

<span data-ttu-id="4fa2b-119">Hello tábla hello alább következő feltételezéseket végzett:</span><span class="sxs-lookup"><span data-stu-id="4fa2b-119">In hello table below hello following assumptions have been made:</span></span>

* <span data-ttu-id="4fa2b-120">Az adatok még akkor is, terjesztési történt.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-120">An even distribution of data has occurred</span></span> 
* <span data-ttu-id="4fa2b-121">hello átlagos sor hossza 250 bájt</span><span class="sxs-lookup"><span data-stu-id="4fa2b-121">hello average row length is 250 bytes</span></span>

| <span data-ttu-id="4fa2b-122">[DWU][DWU]</span><span class="sxs-lookup"><span data-stu-id="4fa2b-122">[DWU][DWU]</span></span> | <span data-ttu-id="4fa2b-123">Cap eloszlása (GB)</span><span class="sxs-lookup"><span data-stu-id="4fa2b-123">Cap per distribution (GiB)</span></span> | <span data-ttu-id="4fa2b-124">Azokat a Terjesztéseket száma</span><span class="sxs-lookup"><span data-stu-id="4fa2b-124">Number of Distributions</span></span> | <span data-ttu-id="4fa2b-125">Tranzakció maximális (GB)</span><span class="sxs-lookup"><span data-stu-id="4fa2b-125">MAX transaction size (GiB)</span></span> | <span data-ttu-id="4fa2b-126"># Sorok eloszlása</span><span class="sxs-lookup"><span data-stu-id="4fa2b-126"># Rows per distribution</span></span> | <span data-ttu-id="4fa2b-127">Tranzakciónként sorok maximális száma</span><span class="sxs-lookup"><span data-stu-id="4fa2b-127">Max Rows per transaction</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="4fa2b-128">DW100</span><span class="sxs-lookup"><span data-stu-id="4fa2b-128">DW100</span></span> |<span data-ttu-id="4fa2b-129">1</span><span class="sxs-lookup"><span data-stu-id="4fa2b-129">1</span></span> |<span data-ttu-id="4fa2b-130">60</span><span class="sxs-lookup"><span data-stu-id="4fa2b-130">60</span></span> |<span data-ttu-id="4fa2b-131">60</span><span class="sxs-lookup"><span data-stu-id="4fa2b-131">60</span></span> |<span data-ttu-id="4fa2b-132">4,000,000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-132">4,000,000</span></span> |<span data-ttu-id="4fa2b-133">240,000,000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-133">240,000,000</span></span> |
| <span data-ttu-id="4fa2b-134">DW200</span><span class="sxs-lookup"><span data-stu-id="4fa2b-134">DW200</span></span> |<span data-ttu-id="4fa2b-135">1.5</span><span class="sxs-lookup"><span data-stu-id="4fa2b-135">1.5</span></span> |<span data-ttu-id="4fa2b-136">60</span><span class="sxs-lookup"><span data-stu-id="4fa2b-136">60</span></span> |<span data-ttu-id="4fa2b-137">90</span><span class="sxs-lookup"><span data-stu-id="4fa2b-137">90</span></span> |<span data-ttu-id="4fa2b-138">6,000,000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-138">6,000,000</span></span> |<span data-ttu-id="4fa2b-139">360,000,000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-139">360,000,000</span></span> |
| <span data-ttu-id="4fa2b-140">DW300</span><span class="sxs-lookup"><span data-stu-id="4fa2b-140">DW300</span></span> |<span data-ttu-id="4fa2b-141">2.25</span><span class="sxs-lookup"><span data-stu-id="4fa2b-141">2.25</span></span> |<span data-ttu-id="4fa2b-142">60</span><span class="sxs-lookup"><span data-stu-id="4fa2b-142">60</span></span> |<span data-ttu-id="4fa2b-143">135</span><span class="sxs-lookup"><span data-stu-id="4fa2b-143">135</span></span> |<span data-ttu-id="4fa2b-144">9,000,000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-144">9,000,000</span></span> |<span data-ttu-id="4fa2b-145">540,000,000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-145">540,000,000</span></span> |
| <span data-ttu-id="4fa2b-146">DW400</span><span class="sxs-lookup"><span data-stu-id="4fa2b-146">DW400</span></span> |<span data-ttu-id="4fa2b-147">3</span><span class="sxs-lookup"><span data-stu-id="4fa2b-147">3</span></span> |<span data-ttu-id="4fa2b-148">60</span><span class="sxs-lookup"><span data-stu-id="4fa2b-148">60</span></span> |<span data-ttu-id="4fa2b-149">180</span><span class="sxs-lookup"><span data-stu-id="4fa2b-149">180</span></span> |<span data-ttu-id="4fa2b-150">12,000,000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-150">12,000,000</span></span> |<span data-ttu-id="4fa2b-151">720,000,000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-151">720,000,000</span></span> |
| <span data-ttu-id="4fa2b-152">DW500</span><span class="sxs-lookup"><span data-stu-id="4fa2b-152">DW500</span></span> |<span data-ttu-id="4fa2b-153">3.75</span><span class="sxs-lookup"><span data-stu-id="4fa2b-153">3.75</span></span> |<span data-ttu-id="4fa2b-154">60</span><span class="sxs-lookup"><span data-stu-id="4fa2b-154">60</span></span> |<span data-ttu-id="4fa2b-155">225</span><span class="sxs-lookup"><span data-stu-id="4fa2b-155">225</span></span> |<span data-ttu-id="4fa2b-156">15,000,000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-156">15,000,000</span></span> |<span data-ttu-id="4fa2b-157">900,000,000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-157">900,000,000</span></span> |
| <span data-ttu-id="4fa2b-158">DW600</span><span class="sxs-lookup"><span data-stu-id="4fa2b-158">DW600</span></span> |<span data-ttu-id="4fa2b-159">4.5</span><span class="sxs-lookup"><span data-stu-id="4fa2b-159">4.5</span></span> |<span data-ttu-id="4fa2b-160">60</span><span class="sxs-lookup"><span data-stu-id="4fa2b-160">60</span></span> |<span data-ttu-id="4fa2b-161">270</span><span class="sxs-lookup"><span data-stu-id="4fa2b-161">270</span></span> |<span data-ttu-id="4fa2b-162">18,000,000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-162">18,000,000</span></span> |<span data-ttu-id="4fa2b-163">1,080,000,000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-163">1,080,000,000</span></span> |
| <span data-ttu-id="4fa2b-164">DW1000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-164">DW1000</span></span> |<span data-ttu-id="4fa2b-165">7.5</span><span class="sxs-lookup"><span data-stu-id="4fa2b-165">7.5</span></span> |<span data-ttu-id="4fa2b-166">60</span><span class="sxs-lookup"><span data-stu-id="4fa2b-166">60</span></span> |<span data-ttu-id="4fa2b-167">450</span><span class="sxs-lookup"><span data-stu-id="4fa2b-167">450</span></span> |<span data-ttu-id="4fa2b-168">30,000,000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-168">30,000,000</span></span> |<span data-ttu-id="4fa2b-169">1,800,000,000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-169">1,800,000,000</span></span> |
| <span data-ttu-id="4fa2b-170">DW1200</span><span class="sxs-lookup"><span data-stu-id="4fa2b-170">DW1200</span></span> |<span data-ttu-id="4fa2b-171">9</span><span class="sxs-lookup"><span data-stu-id="4fa2b-171">9</span></span> |<span data-ttu-id="4fa2b-172">60</span><span class="sxs-lookup"><span data-stu-id="4fa2b-172">60</span></span> |<span data-ttu-id="4fa2b-173">540</span><span class="sxs-lookup"><span data-stu-id="4fa2b-173">540</span></span> |<span data-ttu-id="4fa2b-174">36,000,000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-174">36,000,000</span></span> |<span data-ttu-id="4fa2b-175">2,160,000,000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-175">2,160,000,000</span></span> |
| <span data-ttu-id="4fa2b-176">DW1500</span><span class="sxs-lookup"><span data-stu-id="4fa2b-176">DW1500</span></span> |<span data-ttu-id="4fa2b-177">11.25</span><span class="sxs-lookup"><span data-stu-id="4fa2b-177">11.25</span></span> |<span data-ttu-id="4fa2b-178">60</span><span class="sxs-lookup"><span data-stu-id="4fa2b-178">60</span></span> |<span data-ttu-id="4fa2b-179">675</span><span class="sxs-lookup"><span data-stu-id="4fa2b-179">675</span></span> |<span data-ttu-id="4fa2b-180">45,000,000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-180">45,000,000</span></span> |<span data-ttu-id="4fa2b-181">2,700,000,000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-181">2,700,000,000</span></span> |
| <span data-ttu-id="4fa2b-182">DW2000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-182">DW2000</span></span> |<span data-ttu-id="4fa2b-183">15</span><span class="sxs-lookup"><span data-stu-id="4fa2b-183">15</span></span> |<span data-ttu-id="4fa2b-184">60</span><span class="sxs-lookup"><span data-stu-id="4fa2b-184">60</span></span> |<span data-ttu-id="4fa2b-185">900</span><span class="sxs-lookup"><span data-stu-id="4fa2b-185">900</span></span> |<span data-ttu-id="4fa2b-186">60,000,000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-186">60,000,000</span></span> |<span data-ttu-id="4fa2b-187">3,600,000,000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-187">3,600,000,000</span></span> |
| <span data-ttu-id="4fa2b-188">DW3000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-188">DW3000</span></span> |<span data-ttu-id="4fa2b-189">22.5</span><span class="sxs-lookup"><span data-stu-id="4fa2b-189">22.5</span></span> |<span data-ttu-id="4fa2b-190">60</span><span class="sxs-lookup"><span data-stu-id="4fa2b-190">60</span></span> |<span data-ttu-id="4fa2b-191">1,350</span><span class="sxs-lookup"><span data-stu-id="4fa2b-191">1,350</span></span> |<span data-ttu-id="4fa2b-192">90,000,000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-192">90,000,000</span></span> |<span data-ttu-id="4fa2b-193">5,400,000,000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-193">5,400,000,000</span></span> |
| <span data-ttu-id="4fa2b-194">DW6000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-194">DW6000</span></span> |<span data-ttu-id="4fa2b-195">45</span><span class="sxs-lookup"><span data-stu-id="4fa2b-195">45</span></span> |<span data-ttu-id="4fa2b-196">60</span><span class="sxs-lookup"><span data-stu-id="4fa2b-196">60</span></span> |<span data-ttu-id="4fa2b-197">2,700</span><span class="sxs-lookup"><span data-stu-id="4fa2b-197">2,700</span></span> |<span data-ttu-id="4fa2b-198">180,000,000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-198">180,000,000</span></span> |<span data-ttu-id="4fa2b-199">10,800,000,000</span><span class="sxs-lookup"><span data-stu-id="4fa2b-199">10,800,000,000</span></span> |

<span data-ttu-id="4fa2b-200">hello tranzakció maximális mérete / tranzakció vagy műveletet alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-200">hello transaction size limit is applied per transaction or operation.</span></span> <span data-ttu-id="4fa2b-201">Minden egyidejű tranzakciókat keresztül nem lesz alkalmazva.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-201">It is not applied across all concurrent transactions.</span></span> <span data-ttu-id="4fa2b-202">Ezért az egyes tranzakciókra, engedélyezett toowrite ezt a adatok toohello napló mennyiséget.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-202">Therefore each transaction is permitted toowrite this amount of data toohello log.</span></span> 

<span data-ttu-id="4fa2b-203">toooptimize minimalizálása érdekében hello toohello napló írt adatok mennyiségét, és tekintse meg a toohello [tranzakciók gyakorlati tanácsok] [ Transactions best practices] cikk.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-203">toooptimize and minimize hello amount of data written toohello log please refer toohello [Transactions best practices][Transactions best practices] article.</span></span>

> [!WARNING]
> <span data-ttu-id="4fa2b-204">tranzakció mérete csak érhető el, a KIVONATOLÓ vagy az elosztott ROUND_ROBIN táblák, ahol hello terjednek a hello adatok maximális hello is van.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-204">hello maximum transaction size can only be achieved for HASH or ROUND_ROBIN distributed tables where hello spread of hello data is even.</span></span> <span data-ttu-id="4fa2b-205">Ha hello tranzakció az adatok írása kihasználtságot módon toohello terjesztéseket majd hello határértéke valószínűleg toobe elérte előzetes toohello tranzakció maximális méretet.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-205">If hello transaction is writing data in a skewed fashion toohello distributions then hello limit is likely toobe reached prior toohello maximum transaction size.</span></span>
> <!--REPLICATED_TABLE-->
> 
> 

## <a name="transaction-state"></a><span data-ttu-id="4fa2b-206">Tranzakció állapota</span><span class="sxs-lookup"><span data-stu-id="4fa2b-206">Transaction state</span></span>
<span data-ttu-id="4fa2b-207">Az SQL Data Warehouse használ hello XACT_STATE() függvény tooreport hello értékével -2 sikertelen tranzakció.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-207">SQL Data Warehouse uses hello XACT_STATE() function tooreport a failed transaction using hello value -2.</span></span> <span data-ttu-id="4fa2b-208">Ez azt jelenti, hogy hello tranzakció sikertelen volt, és csak visszaállítás meg van jelölve</span><span class="sxs-lookup"><span data-stu-id="4fa2b-208">This means that hello transaction has failed and is marked for rollback only</span></span>

> [!NOTE]
> <span data-ttu-id="4fa2b-209">hello-2 hello XACT_STATE függvény toodenote egy sikertelen tranzakció jelöli másképp viselkednek tooSQL kiszolgáló használatához.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-209">hello use of -2 by hello XACT_STATE function toodenote a failed transaction represents different behavior tooSQL Server.</span></span> <span data-ttu-id="4fa2b-210">SQL Server hello-1 érték toorepresent vissza nem vonható véglegesítésű tranzakcióból használja.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-210">SQL Server uses hello value -1 toorepresent an un-committable transaction.</span></span> <span data-ttu-id="4fa2b-211">SQL Server egy tranzakción belül hibák tűri megjelölve, vissza nem vonható véglegesítésű toobe rendelkező nélkül.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-211">SQL Server can tolerate some errors inside a transaction without it having toobe marked as un-committable.</span></span> <span data-ttu-id="4fa2b-212">Például `SELECT 1/0` volna hibát jelez, de nem kényszerítik ki a tranzakció vissza nem vonható véglegesítésű állapotba kerülnek.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-212">For example `SELECT 1/0` would cause an error but not force a transaction into an un-committable state.</span></span> <span data-ttu-id="4fa2b-213">SQL Server is lehetővé teszi az olvasási műveletek hello vissza nem vonható véglegesítésű tranzakcióban.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-213">SQL Server also permits reads in hello un-committable transaction.</span></span> <span data-ttu-id="4fa2b-214">Azonban az SQL Data Warehouse nem teszik ezt.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-214">However, SQL Data Warehouse does not let you do this.</span></span> <span data-ttu-id="4fa2b-215">Hiba esetén egy SQL Data Warehouse tranzakción belül automatikusan módba lép, -2 hello állapotát, és nem fogja tudni toomake esetleges további kiválasztása utasítások amíg hello utasítás vissza lett állítva.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-215">If an error occurs inside a SQL Data Warehouse transaction it will automatically enter hello -2 state and you will not be able toomake any further select statements until hello statement has been rolled back.</span></span> <span data-ttu-id="4fa2b-216">Éppen ezért fontos, hogy az alkalmazás kódja toosee XACT_STATE() használ, akkor esetleg toomake kódmódosításra toocheck.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-216">It is therefore important toocheck that your application code toosee if it uses  XACT_STATE() as you may need toomake code modifications.</span></span>
> 
> 

<span data-ttu-id="4fa2b-217">Például az SQL Serverben láthatja a tranzakciót, amelynek a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="4fa2b-217">For example, in SQL Server you might see a transaction that looks like this:</span></span>

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

<span data-ttu-id="4fa2b-218">Ha nem adja meg a kódot, mert az újabb majd jelenik meg hibaüzenet a következő hello:</span><span class="sxs-lookup"><span data-stu-id="4fa2b-218">If you leave your code as it is above then you will get hello following error message:</span></span>

<span data-ttu-id="4fa2b-219">Üzenet 111233, szint 16 állapot 1, 1 sor 111233; hello aktuális tranzakció megszakadt, és minden függő módosítás rendelkezik vissza lett állítva.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-219">Msg 111233, Level 16, State 1, Line 1 111233;hello current transaction has aborted, and any pending changes have been rolled back.</span></span> <span data-ttu-id="4fa2b-220">OK: A tranzakció csak visszaállítási állapotban nem kifejezetten vissza lett állítva egy DDL, DML vagy SELECT utasítás előtt.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-220">Cause: A transaction in a rollback-only state was not explicitly rolled back before a DDL, DML or SELECT statement.</span></span>

<span data-ttu-id="4fa2b-221">Még nem fog hello ERROR_ * funkciók hello kimenetét.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-221">You will also not get hello output of hello ERROR_* functions.</span></span>

<span data-ttu-id="4fa2b-222">Az SQL Data Warehouse hello kód toobe kis mértékben meg van szüksége:</span><span class="sxs-lookup"><span data-stu-id="4fa2b-222">In SQL Data Warehouse hello code needs toobe slightly altered:</span></span>

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

<span data-ttu-id="4fa2b-223">hello várt működést Mostantól követi.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-223">hello expected behavior is now observed.</span></span> <span data-ttu-id="4fa2b-224">hello hiba hello tranzakcióban kezeli, és hello ERROR_ * funkciók elvárt adjon meg értékeket.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-224">hello error in hello transaction is managed and hello ERROR_* functions provide values as expected.</span></span>

<span data-ttu-id="4fa2b-225">Megváltozott csak adott hello `ROLLBACK` a hello a tranzakció toohappen volna, mielőtt hello hello hello hiba adatainak olvasása `CATCH` blokkot.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-225">All that has changed is that hello `ROLLBACK` of hello transaction had toohappen before hello read of hello error information in hello `CATCH` block.</span></span>

## <a name="errorline-function"></a><span data-ttu-id="4fa2b-226">Error_Line() függvény</span><span class="sxs-lookup"><span data-stu-id="4fa2b-226">Error_Line() function</span></span>
<span data-ttu-id="4fa2b-227">Akkor is érdemes megjegyezni, hogy az SQL Data Warehouse nem valósítja meg, vagy hello ERROR_LINE() funkció támogatja.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-227">It is also worth noting that SQL Data Warehouse does not implement or support hello ERROR_LINE() function.</span></span> <span data-ttu-id="4fa2b-228">Ha ez a kód szüksége lesz a tooremove azt az SQL Data Warehouse szolgáltatással kompatibilis toobe.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-228">If you have this in your code you will need tooremove it toobe compliant with SQL Data Warehouse.</span></span> <span data-ttu-id="4fa2b-229">Helyette a kódban lekérdezés címkék tooimplement funkciókat.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-229">Use query labels in your code instead tooimplement equivalent functionality.</span></span> <span data-ttu-id="4fa2b-230">Tekintse meg a toohello [címke] [ LABEL] cikk további részleteket a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-230">Please refer toohello [LABEL][LABEL] article for more details on this feature.</span></span>

## <a name="using-throw-and-raiserror"></a><span data-ttu-id="4fa2b-231">Segítségével THROW és RAISERROR</span><span class="sxs-lookup"><span data-stu-id="4fa2b-231">Using THROW and RAISERROR</span></span>
<span data-ttu-id="4fa2b-232">THROW hello korszerűbb implementációja az SQL Data Warehouse kivételt váltson ki, de a RAISERROR is támogatott.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-232">THROW is hello more modern implementation for raising exceptions in SQL Data Warehouse but RAISERROR is also supported.</span></span> <span data-ttu-id="4fa2b-233">Néhány dologban is érdemes figyelmet toohowever fizet.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-233">There are a few differences that are worth paying attention toohowever.</span></span>

* <span data-ttu-id="4fa2b-234">Felhasználó által definiált hibaüzenetek számok nem lehet hello THROW 100 000-150 000 tartományon</span><span class="sxs-lookup"><span data-stu-id="4fa2b-234">User defined error messages numbers cannot be in hello 100,000 - 150,000 range for THROW</span></span>
* <span data-ttu-id="4fa2b-235">RAISERROR hibaüzenetek 50 000 van rögzítve.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-235">RAISERROR error messages are fixed at 50,000</span></span>
* <span data-ttu-id="4fa2b-236">Ilyen hibaszámú használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-236">Use of sys.messages is not supported</span></span>

## <a name="limitiations"></a><span data-ttu-id="4fa2b-237">Limitiations</span><span class="sxs-lookup"><span data-stu-id="4fa2b-237">Limitiations</span></span>
<span data-ttu-id="4fa2b-238">Az SQL Data Warehouse néhány egyéb korlátozások is tootransactions rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4fa2b-238">SQL Data Warehouse does have a few other restrictions that relate tootransactions.</span></span>

<span data-ttu-id="4fa2b-239">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="4fa2b-239">They are as follows:</span></span>

* <span data-ttu-id="4fa2b-240">Elosztott tranzakciók</span><span class="sxs-lookup"><span data-stu-id="4fa2b-240">No distributed transactions</span></span>
* <span data-ttu-id="4fa2b-241">Nem engedélyezett beágyazott tranzakciók</span><span class="sxs-lookup"><span data-stu-id="4fa2b-241">No nested transactions permitted</span></span>
* <span data-ttu-id="4fa2b-242">Nem engedélyezett pontokon menthet</span><span class="sxs-lookup"><span data-stu-id="4fa2b-242">No save points allowed</span></span>
* <span data-ttu-id="4fa2b-243">Elnevezett tranzakciók</span><span class="sxs-lookup"><span data-stu-id="4fa2b-243">No named transactions</span></span>
* <span data-ttu-id="4fa2b-244">Nem jelölt tranzakciók</span><span class="sxs-lookup"><span data-stu-id="4fa2b-244">No marked transactions</span></span>
* <span data-ttu-id="4fa2b-245">Nem támogatott például DDL `CREATE TABLE` belül a felhasználó definiált tranzakció</span><span class="sxs-lookup"><span data-stu-id="4fa2b-245">No support for DDL such as `CREATE TABLE` inside a user defined transaction</span></span>

## <a name="next-steps"></a><span data-ttu-id="4fa2b-246">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4fa2b-246">Next steps</span></span>
<span data-ttu-id="4fa2b-247">toolearn optimalizálása tranzakciók, bővebben lásd: [tranzakciók gyakorlati tanácsok][Transactions best practices].</span><span class="sxs-lookup"><span data-stu-id="4fa2b-247">toolearn more about optimizing transactions, see [Transactions best practices][Transactions best practices].</span></span>  <span data-ttu-id="4fa2b-248">Lásd az SQL Data Warehouse gyakorlati tanácsokat, toolearn [gyakorlati tanácsok az SQL Data Warehouse][SQL Data Warehouse best practices].</span><span class="sxs-lookup"><span data-stu-id="4fa2b-248">toolearn about other SQL Data Warehouse best practices, see [SQL Data Warehouse best practices][SQL Data Warehouse best practices].</span></span>

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[development overview]: ./sql-data-warehouse-overview-develop.md
[Transactions best practices]: ./sql-data-warehouse-develop-best-practices-transactions.md
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[LABEL]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->

<!--Other Web references-->
