---
title: "aaaManage Azure Data Lake Analytics használatával hello Azure portálon |} Microsoft Docs"
description: "Megtudhatja, hogyan toomanage Data Lake Analytics könyvvitelének, adatok adatforrásokat, felhasználók, és a feladatokat."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: a0e045f1-73d6-427f-868d-7b55c10f811b
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: f63ccdfae79772c92e92462194e8cdc636a73dc6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-data-lake-analytics-by-using-hello-azure-portal"></a><span data-ttu-id="568d4-103">Azure Data Lake Analytics kezelése hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="568d4-103">Manage Azure Data Lake Analytics by using hello Azure portal</span></span>
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

<span data-ttu-id="568d4-104">Ismerje meg, hogyan toomanage Azure Data Lake Analytics-fiókok, a fiók adatforrások, a felhasználók és a feladatok használatával hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="568d4-104">Learn how toomanage Azure Data Lake Analytics accounts, account data sources, users, and jobs by using hello Azure portal.</span></span> <span data-ttu-id="568d4-105">toosee kapcsolatos témakörök más eszközök használatával kapcsolatos kattintson egy fülre az hello oldal hello tetején.</span><span class="sxs-lookup"><span data-stu-id="568d4-105">toosee management topics about using other tools, click a tab at hello top of hello page.</span></span>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-lake-analytics-accounts"></a><span data-ttu-id="568d4-106">Data Lake Analytics-fiókok kezelése</span><span class="sxs-lookup"><span data-stu-id="568d4-106">Manage Data Lake Analytics accounts</span></span>

### <a name="create-an-account"></a><span data-ttu-id="568d4-107">Fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="568d4-107">Create an account</span></span>

1. <span data-ttu-id="568d4-108">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="568d4-108">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="568d4-109">Kattintson az **Új** > **Intelligencia és elemzés** > **Data Lake Analytics** elemre.</span><span class="sxs-lookup"><span data-stu-id="568d4-109">Click **New** > **Intelligence + analytics** > **Data Lake Analytics**.</span></span>
3. <span data-ttu-id="568d4-110">Válassza ki a következő elemek hello értékeit:</span><span class="sxs-lookup"><span data-stu-id="568d4-110">Select values for hello following items:</span></span> 
   1. <span data-ttu-id="568d4-111">**Név**: hello hello Data Lake Analytics-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="568d4-111">**Name**: hello name of hello Data Lake Analytics account.</span></span>
   2. <span data-ttu-id="568d4-112">**Előfizetés**: hello hello fiókhoz használt Azure-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="568d4-112">**Subscription**: hello Azure subscription used for hello account.</span></span>
   3. <span data-ttu-id="568d4-113">**Erőforráscsoport**: hello Azure erőforráscsoport mely toocreate hello fiók.</span><span class="sxs-lookup"><span data-stu-id="568d4-113">**Resource Group**: hello Azure resource group in which toocreate hello account.</span></span> 
   4. <span data-ttu-id="568d4-114">**Hely**: hello Azure datacenter hello Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="568d4-114">**Location**: hello Azure datacenter for hello Data Lake Analytics account.</span></span> 
   5. <span data-ttu-id="568d4-115">**Data Lake Store**: hello alapértelmezett tároló toobe hello Data Lake Analytics-fiókhoz használt.</span><span class="sxs-lookup"><span data-stu-id="568d4-115">**Data Lake Store**: hello default store toobe used for hello Data Lake Analytics account.</span></span> <span data-ttu-id="568d4-116">hello Azure Data Lake Store-fiók és a fióknak szerepelnie kell a Data Lake Analytics hello hello ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="568d4-116">hello Azure Data Lake Store account and hello Data Lake Analytics account must be in hello same location.</span></span>
4. <span data-ttu-id="568d4-117">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="568d4-117">Click **Create**.</span></span> 

### <a name="delete-a-data-lake-analytics-account"></a><span data-ttu-id="568d4-118">A Data Lake Analytics-fiók törlése</span><span class="sxs-lookup"><span data-stu-id="568d4-118">Delete a Data Lake Analytics account</span></span>

<span data-ttu-id="568d4-119">Data Lake Analytics-fiók törlése előtt törölje az alapértelmezett Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="568d4-119">Before you delete a Data Lake Analytics account, delete its default Data Lake Store account.</span></span>

1. <span data-ttu-id="568d4-120">A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="568d4-120">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="568d4-121">Kattintson a **Törlés** gombra.</span><span class="sxs-lookup"><span data-stu-id="568d4-121">Click **Delete**.</span></span>
3. <span data-ttu-id="568d4-122">Hello fiók neve.</span><span class="sxs-lookup"><span data-stu-id="568d4-122">Type hello account name.</span></span>
4. <span data-ttu-id="568d4-123">Kattintson a **Törlés** gombra.</span><span class="sxs-lookup"><span data-stu-id="568d4-123">Click **Delete**.</span></span>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-sources"></a><span data-ttu-id="568d4-124">Adatforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="568d4-124">Manage data sources</span></span>

<span data-ttu-id="568d4-125">A Data Lake Analytics a következő adatforrások hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="568d4-125">Data Lake Analytics supports hello following data sources:</span></span>

* <span data-ttu-id="568d4-126">Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="568d4-126">Data Lake Store</span></span>
* <span data-ttu-id="568d4-127">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="568d4-127">Azure Storage</span></span>

<span data-ttu-id="568d4-128">Data Explorer toobrowse adatforrások használja, és hajtsa végre az alapvető felügyeleti műveletet.</span><span class="sxs-lookup"><span data-stu-id="568d4-128">You can use Data Explorer toobrowse data sources and perform basic file management operations.</span></span> 

### <a name="add-a-data-source"></a><span data-ttu-id="568d4-129">Egy adatforrás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="568d4-129">Add a data source</span></span>

1. <span data-ttu-id="568d4-130">A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="568d4-130">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="568d4-131">Kattintson a **adatforrások**.</span><span class="sxs-lookup"><span data-stu-id="568d4-131">Click **Data Sources**.</span></span>
3. <span data-ttu-id="568d4-132">Kattintson a **adja hozzá az adatforrást**.</span><span class="sxs-lookup"><span data-stu-id="568d4-132">Click **Add Data Source**.</span></span>
    
   * <span data-ttu-id="568d4-133">hello fiók nevét és a hozzáférési toohello fiók toobe képes tooquery tooadd Data Lake Store-fiók, kell azt.</span><span class="sxs-lookup"><span data-stu-id="568d4-133">tooadd a Data Lake Store account, you need hello account name and access toohello account toobe able tooquery it.</span></span>
   * <span data-ttu-id="568d4-134">tooadd Azure Blob Storage tárolóban, és van szükség hello tárfiók hello kulcsára.</span><span class="sxs-lookup"><span data-stu-id="568d4-134">tooadd Azure Blob storage, you need hello storage account and hello account key.</span></span> <span data-ttu-id="568d4-135">toofind őket, lépjen toohello tárfiók hello portálon.</span><span class="sxs-lookup"><span data-stu-id="568d4-135">toofind them, go toohello storage account in hello portal.</span></span>

## <a name="set-up-firewall-rules"></a><span data-ttu-id="568d4-136">Tűzfalszabályok beállítása</span><span class="sxs-lookup"><span data-stu-id="568d4-136">Set up firewall rules</span></span>

<span data-ttu-id="568d4-137">Hello hálózati szintű hozzáférés tooyour Data Lake Analytics-fiók zárolása Data Lake Analytics toofurther is használhatja.</span><span class="sxs-lookup"><span data-stu-id="568d4-137">You can use Data Lake Analytics toofurther lock down access tooyour Data Lake Analytics account at hello network level.</span></span> <span data-ttu-id="568d4-138">Tűzfal engedélyezése, IP-cím vagy IP-címtartomány megadása a megbízható ügyfeleket.</span><span class="sxs-lookup"><span data-stu-id="568d4-138">You can enable a firewall, specify an IP address, or define an IP address range for your trusted clients.</span></span> <span data-ttu-id="568d4-139">Ezeket a mértékeket engedélyezése után csak hello definiált tartományon belüli hello IP-címmel rendelkező ügyfelek kapcsolódhatnak toohello tároló.</span><span class="sxs-lookup"><span data-stu-id="568d4-139">After you enable these measures, only clients that have hello IP addresses within hello defined range can connect toohello store.</span></span>

<span data-ttu-id="568d4-140">Ha más Azure-szolgáltatásokkal, például az Azure Data Factory vagy a virtuális gépek, toohello Data Lake Analytics-fiók, győződjön meg arról, hogy **Azure-szolgáltatások engedélyezése** van kapcsolva **a**.</span><span class="sxs-lookup"><span data-stu-id="568d4-140">If other Azure services, like Azure Data Factory or VMs, connect toohello Data Lake Analytics account, make sure that **Allow Azure Services** is turned **On**.</span></span> 

### <a name="set-up-a-firewall-rule"></a><span data-ttu-id="568d4-141">A tűzfalszabályok beállítása</span><span class="sxs-lookup"><span data-stu-id="568d4-141">Set up a firewall rule</span></span>

1. <span data-ttu-id="568d4-142">A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="568d4-142">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="568d4-143">A hello hello bal oldali menüben kattintson **tűzfal**.</span><span class="sxs-lookup"><span data-stu-id="568d4-143">On hello menu on hello left, click **Firewall**.</span></span>

## <a name="add-a-new-user"></a><span data-ttu-id="568d4-144">Új felhasználó hozzáadása</span><span class="sxs-lookup"><span data-stu-id="568d4-144">Add a new user</span></span>

<span data-ttu-id="568d4-145">Használhatja a hello **varázslót** tooeasily konfigurálta a új Data Lake-felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="568d4-145">You can use hello **Add User Wizard** tooeasily provision new Data Lake users.</span></span>

1. <span data-ttu-id="568d4-146">A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="568d4-146">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="568d4-147">A bal oldali a hello **bevezetés**, kattintson a **varázslót**.</span><span class="sxs-lookup"><span data-stu-id="568d4-147">On hello left, under **Getting Started**, click **Add User Wizard**.</span></span>
3. <span data-ttu-id="568d4-148">Válasszon ki egy felhasználót, és kattintson **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="568d4-148">Select a user, and then click **Select**.</span></span>
4. <span data-ttu-id="568d4-149">Válassza ki a szerepkört, és kattintson **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="568d4-149">Select a role, and then click **Select**.</span></span> <span data-ttu-id="568d4-150">fel egy új Azure Data Lake, jelölje be hello fejlesztői toouse tooset **Data Lake Analytics fejlesztői** szerepkör.</span><span class="sxs-lookup"><span data-stu-id="568d4-150">tooset up a new developer toouse Azure Data Lake, select hello **Data Lake Analytics Developer** role.</span></span>
5. <span data-ttu-id="568d4-151">Válassza ki a hello hozzáférés-vezérlési listák (ACL) hello U-SQL-adatbázisok számára.</span><span class="sxs-lookup"><span data-stu-id="568d4-151">Select hello access control lists (ACLs) for hello U-SQL databases.</span></span> <span data-ttu-id="568d4-152">Ha elégedett a kiválasztott beállításokat, kattintson **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="568d4-152">When you're satisfied with your choices, click **Select**.</span></span>
6. <span data-ttu-id="568d4-153">Válassza ki a fájlok hello ACL-listát.</span><span class="sxs-lookup"><span data-stu-id="568d4-153">Select hello ACLs for files.</span></span> <span data-ttu-id="568d4-154">Nem hello alapértelmezett tároló, hello gyökérmappához hello ACL-ek módosítása "/" és hello vagy mappához.</span><span class="sxs-lookup"><span data-stu-id="568d4-154">For hello default store, don't change hello ACLs for hello root folder "/" and for hello /system folder.</span></span> <span data-ttu-id="568d4-155">Kattintson a **Kiválasztás** gombra.</span><span class="sxs-lookup"><span data-stu-id="568d4-155">Click **Select**.</span></span>
7. <span data-ttu-id="568d4-156">Tekintse át a kiválasztott módosításokat, és kattintson **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="568d4-156">Review all your selected changes, and then click **Run**.</span></span>
8. <span data-ttu-id="568d4-157">Ha hello varázsló végzett, kattintson a **végzett**.</span><span class="sxs-lookup"><span data-stu-id="568d4-157">When hello wizard is finished, click **Done**.</span></span>

## <a name="manage-role-based-access-control"></a><span data-ttu-id="568d4-158">Szerepköralapú hozzáférés-vezérlés kezelése</span><span class="sxs-lookup"><span data-stu-id="568d4-158">Manage Role-Based Access Control</span></span>

<span data-ttu-id="568d4-159">Más Azure-szolgáltatásokkal, például a felhasználók hogyan használják a szerepköralapú hozzáférés-vezérlést (RBAC) toocontrol hello szolgáltatást is használhatja.</span><span class="sxs-lookup"><span data-stu-id="568d4-159">Like other Azure services, you can use Role-Based Access Control (RBAC) toocontrol how users interact with hello service.</span></span>

<span data-ttu-id="568d4-160">hello szabványos RBAC-szerepkörök hello a következő képességekkel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="568d4-160">hello standard RBAC roles have hello following capabilities:</span></span>
* <span data-ttu-id="568d4-161">**Tulajdonos**: feladatok küldéséhez, feladatok figyelése, szakítsa bármely felhasználó, és hello fiók konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="568d4-161">**Owner**: Can submit jobs, monitor jobs, cancel jobs from any user, and configure hello account.</span></span>
* <span data-ttu-id="568d4-162">**A közreműködői**: feladatok küldéséhez, feladatok figyelése, szakítsa bármely felhasználó, és hello fiók konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="568d4-162">**Contributor**: Can submit jobs, monitor jobs, cancel jobs from any user, and configure hello account.</span></span>
* <span data-ttu-id="568d4-163">**Olvasó**: figyelheti a feladat.</span><span class="sxs-lookup"><span data-stu-id="568d4-163">**Reader**: Can monitor jobs.</span></span>

<span data-ttu-id="568d4-164">Hello Data Lake Analytics fejlesztői szerepkör tooenable U-SQL fejlesztők toouse hello Data Lake Analytics szolgáltatás használata.</span><span class="sxs-lookup"><span data-stu-id="568d4-164">Use hello Data Lake Analytics Developer role tooenable U-SQL developers toouse hello Data Lake Analytics service.</span></span> <span data-ttu-id="568d4-165">Hello Data Lake Analytics fejlesztői szerepkört is használhatja:</span><span class="sxs-lookup"><span data-stu-id="568d4-165">You can use hello Data Lake Analytics Developer role to:</span></span>
* <span data-ttu-id="568d4-166">Feladatok elküldéséhez.</span><span class="sxs-lookup"><span data-stu-id="568d4-166">Submit jobs.</span></span>
* <span data-ttu-id="568d4-167">Bármely felhasználó által küldött feladatok feladat állapotát és hello állapotát figyeli.</span><span class="sxs-lookup"><span data-stu-id="568d4-167">Monitor job status and hello progress of jobs submitted by any user.</span></span>
* <span data-ttu-id="568d4-168">Lásd: hello U-SQL-parancsfájlok feladatokból bármely felhasználó által küldött.</span><span class="sxs-lookup"><span data-stu-id="568d4-168">See hello U-SQL scripts from jobs submitted by any user.</span></span>
* <span data-ttu-id="568d4-169">Csak a saját feladatok megszakítása</span><span class="sxs-lookup"><span data-stu-id="568d4-169">Cancel only your own jobs.</span></span>

### <a name="add-users-or-security-groups-tooa-data-lake-analytics-account"></a><span data-ttu-id="568d4-170">Felhasználók vagy biztonsági csoportok tooa Data Lake Analytics-fiók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="568d4-170">Add users or security groups tooa Data Lake Analytics account</span></span>

1. <span data-ttu-id="568d4-171">A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="568d4-171">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="568d4-172">Kattintson a **hozzáférés-vezérlés (IAM)** > **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="568d4-172">Click **Access control (IAM)** > **Add**.</span></span>
3. <span data-ttu-id="568d4-173">Szerepkör kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="568d4-173">Select a role.</span></span>
4. <span data-ttu-id="568d4-174">Felhasználó hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="568d4-174">Add a user.</span></span>
5. <span data-ttu-id="568d4-175">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="568d4-175">Click **OK**.</span></span>

>[!NOTE]
><span data-ttu-id="568d4-176">Ha egy felhasználónak vagy biztonsági csoportot kell toosubmit feladatok, engedéllyel rendelkezik a hello store-fiók is szükséges.</span><span class="sxs-lookup"><span data-stu-id="568d4-176">If a user or a security group needs toosubmit jobs, they also need permission on hello store account.</span></span> <span data-ttu-id="568d4-177">További információkért lásd: [adatok a védelméről a Data Lake Store](../data-lake-store/data-lake-store-secure-data.md).</span><span class="sxs-lookup"><span data-stu-id="568d4-177">For more information, see [Secure data stored in Data Lake Store](../data-lake-store/data-lake-store-secure-data.md).</span></span>
>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-jobs"></a><span data-ttu-id="568d4-178">Feladatok kezelése</span><span class="sxs-lookup"><span data-stu-id="568d4-178">Manage jobs</span></span>

### <a name="submit-a-job"></a><span data-ttu-id="568d4-179">Feladat elküldése</span><span class="sxs-lookup"><span data-stu-id="568d4-179">Submit a job</span></span>

1. <span data-ttu-id="568d4-180">A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="568d4-180">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>

2. <span data-ttu-id="568d4-181">Kattintson a **új feladat**.</span><span class="sxs-lookup"><span data-stu-id="568d4-181">Click **New Job**.</span></span> <span data-ttu-id="568d4-182">Az egyes feladatokhoz konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="568d4-182">For each job,  configure:</span></span>

    1. <span data-ttu-id="568d4-183">**Feladat neve**: hello feladat hello nevét.</span><span class="sxs-lookup"><span data-stu-id="568d4-183">**Job Name**: hello name of hello job.</span></span>
    2. <span data-ttu-id="568d4-184">**Prioritás**: alacsonyabb számok magasabb prioritással rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="568d4-184">**Priority**: Lower numbers have higher priority.</span></span> <span data-ttu-id="568d4-185">Ha a két feladat várólistára kerülnek, hello alacsonyabb prioritással rendelkező első futtatja.</span><span class="sxs-lookup"><span data-stu-id="568d4-185">If two jobs are queued, hello one with lower priority value runs first.</span></span>
    3. <span data-ttu-id="568d4-186">**Párhuzamossági**: hello számítási maximális száma a feladat tooreserve dolgozza fel.</span><span class="sxs-lookup"><span data-stu-id="568d4-186">**Parallelism**: hello maximum number of compute processes tooreserve for this job.</span></span>

3. <span data-ttu-id="568d4-187">Kattintson a **Feladat elküldése** elemre.</span><span class="sxs-lookup"><span data-stu-id="568d4-187">Click **Submit Job**.</span></span>

### <a name="monitor-jobs"></a><span data-ttu-id="568d4-188">Feladatok figyelése</span><span class="sxs-lookup"><span data-stu-id="568d4-188">Monitor jobs</span></span>

1. <span data-ttu-id="568d4-189">A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="568d4-189">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="568d4-190">Kattintson a **összes feladat megtekintéséhez**.</span><span class="sxs-lookup"><span data-stu-id="568d4-190">Click **View All Jobs**.</span></span> <span data-ttu-id="568d4-191">Hello aktív és a közelmúltban befejezett levő összes feladatnak hello fiók listája látható.</span><span class="sxs-lookup"><span data-stu-id="568d4-191">A list of all hello active and recently finished jobs in hello account is shown.</span></span>
3. <span data-ttu-id="568d4-192">Ha úgy gondolja, a **szűrő** hello feladatok által talált toohelp **időtartomány**, **feladat neve**, és **Szerző** értékek.</span><span class="sxs-lookup"><span data-stu-id="568d4-192">Optionally, click **Filter** toohelp you find hello jobs by **Time Range**, **Job Name**, and **Author** values.</span></span> 

### <a name="monitoring-pipeline-jobs"></a><span data-ttu-id="568d4-193">Feldolgozási sor feladatok figyelése</span><span class="sxs-lookup"><span data-stu-id="568d4-193">Monitoring pipeline jobs</span></span>
<span data-ttu-id="568d4-194">Feladatok, amelyek egy folyamat része a munka együtt, általában sorrendben történik, tooaccomplish egy adott forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="568d4-194">Jobs that are part of a pipeline work together, usually sequentially, tooaccomplish a specific scenario.</span></span> <span data-ttu-id="568d4-195">Például hogy egy folyamatot, amely megtisztítja, kiolvassa, átalakítja, összesíti az ügyfél insights használata.</span><span class="sxs-lookup"><span data-stu-id="568d4-195">For example, you can have a pipeline that cleans, extracts, transforms, aggregates usage for customer insights.</span></span> <span data-ttu-id="568d4-196">Feldolgozási sor feladatok azonosított hello "Futószalag" tulajdonság használatát, ha hello feladat el lett küldve.</span><span class="sxs-lookup"><span data-stu-id="568d4-196">Pipeline jobs are identified using hello "Pipeline" property when hello job was submitted.</span></span> <span data-ttu-id="568d4-197">ADF V2 használatával ütemezett feladatok automatikusan lesz feltöltve ezt a tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="568d4-197">Jobs scheduled using ADF V2 will automatically have this property populated.</span></span> 

<span data-ttu-id="568d4-198">tooview folyamatok részét képező U-SQL-feladatok listája:</span><span class="sxs-lookup"><span data-stu-id="568d4-198">tooview a list of U-SQL jobs that are part of pipelines:</span></span> 

1. <span data-ttu-id="568d4-199">A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiókok.</span><span class="sxs-lookup"><span data-stu-id="568d4-199">In hello Azure portal, go tooyour Data Lake Analytics accounts.</span></span>
2. <span data-ttu-id="568d4-200">Kattintson a **Insights feladat**.</span><span class="sxs-lookup"><span data-stu-id="568d4-200">Click **Job Insights**.</span></span> <span data-ttu-id="568d4-201">"Az összes feladat" lapon alapértelmezett lesz, futó, listáját tartalmazó hello sorba, és a feladat befejeződött.</span><span class="sxs-lookup"><span data-stu-id="568d4-201">hello "All Jobs" tab will be defaulted, showing a list of running, queued, and ended jobs.</span></span>
3. <span data-ttu-id="568d4-202">Kattintson a hello **csővezeték feladatok** fülre. Feldolgozási sor feladatokra minden adatcsatorna összesített statisztikák együtt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="568d4-202">Click hello **Pipeline Jobs** tab. A list of pipeline jobs will be shown along with aggregated statistics for each pipeline.</span></span>

### <a name="monitoring-recurring-jobs"></a><span data-ttu-id="568d4-203">Ismétlődő feladatok figyelése</span><span class="sxs-lookup"><span data-stu-id="568d4-203">Monitoring recurring jobs</span></span>
<span data-ttu-id="568d4-204">Ismétlődő feladat egyike, amely rendelkezik hello azonos üzleti logika de használ a különböző bemeneti adatokat, minden egyes futásakor.</span><span class="sxs-lookup"><span data-stu-id="568d4-204">A recurring job is one that has hello same business logic but uses different input data every time it runs.</span></span> <span data-ttu-id="568d4-205">Ideális esetben ismétlődő feladatok mindig sikeres legyen, és viszonylag állandó végrehajtási idővel; Ezek közül a viselkedésmódok a figyelés segítségével ellenőrizze, hogy hello feladat kifogástalan.</span><span class="sxs-lookup"><span data-stu-id="568d4-205">Ideally, recurring jobs should always succeed, and have relatively stable execution time; monitoring these behaviors will help ensure hello job is healthy.</span></span> <span data-ttu-id="568d4-206">Ismétlődő feladatok azonosított hello "Ismétlődési" tulajdonság használatával.</span><span class="sxs-lookup"><span data-stu-id="568d4-206">Recurring jobs are identified using hello "Recurrence" property.</span></span> <span data-ttu-id="568d4-207">ADF V2 használatával ütemezett feladatok automatikusan lesz feltöltve ezt a tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="568d4-207">Jobs scheduled using ADF V2 will automatically have this property populated.</span></span>

<span data-ttu-id="568d4-208">tooview ismétlődő U-SQL feladatok listáját:</span><span class="sxs-lookup"><span data-stu-id="568d4-208">tooview a list of U-SQL jobs that are recurring:</span></span> 

1. <span data-ttu-id="568d4-209">A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiókok.</span><span class="sxs-lookup"><span data-stu-id="568d4-209">In hello Azure portal, go tooyour Data Lake Analytics accounts.</span></span>
2. <span data-ttu-id="568d4-210">Kattintson a **Insights feladat**.</span><span class="sxs-lookup"><span data-stu-id="568d4-210">Click **Job Insights**.</span></span> <span data-ttu-id="568d4-211">"Az összes feladat" lapon alapértelmezett lesz, futó, listáját tartalmazó hello sorba, és a feladat befejeződött.</span><span class="sxs-lookup"><span data-stu-id="568d4-211">hello "All Jobs" tab will be defaulted, showing a list of running, queued, and ended jobs.</span></span>
3. <span data-ttu-id="568d4-212">Kattintson a hello **ismétlődő feladatok** fülre. Ismétlődő feladatok listája jelenik meg minden ismétlődő feladat összesített statisztikák együtt.</span><span class="sxs-lookup"><span data-stu-id="568d4-212">Click hello **Recurring Jobs** tab. A list of recurring jobs will be shown along with aggregated statistics for each recurring job.</span></span>

## <a name="manage-policies"></a><span data-ttu-id="568d4-213">Házirendek kezelése</span><span class="sxs-lookup"><span data-stu-id="568d4-213">Manage policies</span></span>

### <a name="account-level-policies"></a><span data-ttu-id="568d4-214">Fiók szintű szabályzatok</span><span class="sxs-lookup"><span data-stu-id="568d4-214">Account-level policies</span></span>

<span data-ttu-id="568d4-215">Ezek a házirendek alkalmazása tooall feladatok Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="568d4-215">These policies apply tooall jobs in a Data Lake Analytics account.</span></span>

#### <a name="maximum-number-of-aus-in-a-data-lake-analytics-account"></a><span data-ttu-id="568d4-216">A Data Lake Analytics-fiók ausztráliai maximális száma</span><span class="sxs-lookup"><span data-stu-id="568d4-216">Maximum number of AUs in a Data Lake Analytics account</span></span>
<span data-ttu-id="568d4-217">A szabályzatok vezérlik hello Analytics egységek száma (ausztráliai) a Data Lake Analytics-fiók használható.</span><span class="sxs-lookup"><span data-stu-id="568d4-217">A policy controls hello total number of Analytics Units (AUs) your Data Lake Analytics account can use.</span></span> <span data-ttu-id="568d4-218">Alapértelmezés szerint hello értéke too250.</span><span class="sxs-lookup"><span data-stu-id="568d4-218">By default, hello value is set too250.</span></span> <span data-ttu-id="568d4-219">Például, ha az alapérték too250 ausztráliai, akkor 250 hozzárendelt ausztráliai tooit fut egy feladat, és 25 futtató 10 feladatok minden ausztráliai.</span><span class="sxs-lookup"><span data-stu-id="568d4-219">For example, if this value is set too250 AUs, you can have one job running with 250 AUs assigned tooit, or 10 jobs running with 25 AUs each.</span></span> <span data-ttu-id="568d4-220">További feladatok tartalmazó sorba hello futó feladat befejezéséig.</span><span class="sxs-lookup"><span data-stu-id="568d4-220">Additional jobs that are submitted are queued until hello running jobs are finished.</span></span> <span data-ttu-id="568d4-221">Futó feladat befejeződése után ausztráliai feliratkozott hello felszabadítását feladatok toorun sorba.</span><span class="sxs-lookup"><span data-stu-id="568d4-221">When running jobs are finished, AUs are freed up for hello queued jobs toorun.</span></span>

<span data-ttu-id="568d4-222">toochange hello száma ausztráliai a Data Lake Analytics-fiókhoz:</span><span class="sxs-lookup"><span data-stu-id="568d4-222">toochange hello number of AUs for your Data Lake Analytics account:</span></span>

1. <span data-ttu-id="568d4-223">A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="568d4-223">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="568d4-224">Kattintson a **Tulajdonságok** elemre.</span><span class="sxs-lookup"><span data-stu-id="568d4-224">Click **Properties**.</span></span>
3. <span data-ttu-id="568d4-225">A **maximális ausztráliai**, helyezze át a hello csúszkát tooselect értéket, vagy hello érték hello mezőben adja meg.</span><span class="sxs-lookup"><span data-stu-id="568d4-225">Under **Maximum AUs**, move hello slider tooselect a value, or enter hello value in hello text box.</span></span> 
4. <span data-ttu-id="568d4-226">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="568d4-226">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="568d4-227">Ha meg kell ennél nagyobb (250) alapértelmezett hello ausztráliai, hello portálon kattintson **súgó + támogatás** toosubmit egy támogatási kérést.</span><span class="sxs-lookup"><span data-stu-id="568d4-227">If you need more than hello default (250) AUs, in hello portal, click **Help+Support** toosubmit a support request.</span></span> <span data-ttu-id="568d4-228">Ausztráliai érhető el a Data Lake Analytics-fiók hello száma növelhető.</span><span class="sxs-lookup"><span data-stu-id="568d4-228">hello number of AUs available in your Data Lake Analytics account can be increased.</span></span>
>

#### <a name="maximum-number-of-jobs-that-can-run-simultaneously"></a><span data-ttu-id="568d4-229">Az egyidejűleg futtatható feladatok maximális számát</span><span class="sxs-lookup"><span data-stu-id="568d4-229">Maximum number of jobs that can run simultaneously</span></span>
<span data-ttu-id="568d4-230">Egy házirend szabályozza, hogy hány feladatok futtatható hello ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="568d4-230">A policy controls how many jobs can run at hello same time.</span></span> <span data-ttu-id="568d4-231">Alapértelmezés szerint ez az érték too20 van beállítva.</span><span class="sxs-lookup"><span data-stu-id="568d4-231">By default, this value is set too20.</span></span> <span data-ttu-id="568d4-232">Ha a Data Lake Analytics ausztráliai elérhető, új feladatok csak ütemezett toorun azonnal hello teljes futó feladatok száma eléri ezt a házirendet hello értéket.</span><span class="sxs-lookup"><span data-stu-id="568d4-232">If your Data Lake Analytics has AUs available, new jobs are scheduled toorun immediately until hello total number of running jobs reaches hello value of this policy.</span></span> <span data-ttu-id="568d4-233">Amikor elérte az egyidejűleg futtatható feladatok maximális száma hello, további feladat várólistára prioritási sorrendben mindaddig, amíg legalább egy futó feladat befejeződik (attól függően, hogy AU rendelkezésre állás).</span><span class="sxs-lookup"><span data-stu-id="568d4-233">When you reach hello maximum number of jobs that can run simultaneously, subsequent jobs are queued in priority order until one or more running jobs complete (depending on AU availability).</span></span>

<span data-ttu-id="568d4-234">egyidejűleg futtatható feladatok toochange hello száma:</span><span class="sxs-lookup"><span data-stu-id="568d4-234">toochange hello number of jobs that can run simultaneously:</span></span>

1. <span data-ttu-id="568d4-235">A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="568d4-235">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="568d4-236">Kattintson a **Tulajdonságok** elemre.</span><span class="sxs-lookup"><span data-stu-id="568d4-236">Click **Properties**.</span></span>
3. <span data-ttu-id="568d4-237">A **maximális számát a futó feladatok**, helyezze át a hello csúszkát tooselect értéket, vagy hello érték hello mezőben adja meg.</span><span class="sxs-lookup"><span data-stu-id="568d4-237">Under **Maximum Number of Running Jobs**, move hello slider tooselect a value, or enter hello value in hello text box.</span></span> 
4. <span data-ttu-id="568d4-238">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="568d4-238">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="568d4-239">Ha több, mint az alapértelmezett (20) hello portálon, a feladatok száma hello kattintson toorun kell **súgó + támogatás** toosubmit egy támogatási kérést.</span><span class="sxs-lookup"><span data-stu-id="568d4-239">If you need toorun more than hello default (20) number of jobs, in hello portal, click **Help+Support** toosubmit a support request.</span></span> <span data-ttu-id="568d4-240">a Data Lake Analytics-fiók egyidejűleg futtatható feladatok hello száma növelhető.</span><span class="sxs-lookup"><span data-stu-id="568d4-240">hello number of jobs that can run simultaneously in your Data Lake Analytics account can be increased.</span></span>
>

#### <a name="how-long-tookeep-job-metadata-and-resources"></a><span data-ttu-id="568d4-241">Mennyi ideig tookeep feladat metaadatok és erőforrások</span><span class="sxs-lookup"><span data-stu-id="568d4-241">How long tookeep job metadata and resources</span></span> 
<span data-ttu-id="568d4-242">A felhasználók a U-SQL feladatok futtatása, megtartja a hello Data Lake Analytics szolgáltatás és az összes kapcsolódó fájlt.</span><span class="sxs-lookup"><span data-stu-id="568d4-242">When your users run U-SQL jobs, hello Data Lake Analytics service retains all related files.</span></span> <span data-ttu-id="568d4-243">Kapcsolódó fájlok hello U-SQL parancsfájl, a hello U-SQL parancsfájl, a lefordított erőforrások és a statisztika hivatkozott hello DLL-fájlok közé tartoznak.</span><span class="sxs-lookup"><span data-stu-id="568d4-243">Related files include hello U-SQL script, hello DLL files referenced in hello U-SQL script, compiled resources, and statistics.</span></span> <span data-ttu-id="568d4-244">hello fájlok mappájában hello /system/ hello alapértelmezett Azure Data Lake-tárfiókra.</span><span class="sxs-lookup"><span data-stu-id="568d4-244">hello files are in hello /system/ folder of hello default Azure Data Lake Storage account.</span></span> <span data-ttu-id="568d4-245">Ez az irányelv szabályozza, hogy mennyi ideig tárolja ezeket az erőforrásokat automatikusan törlés előtt (hello alapértelmezett érték 30 nap).</span><span class="sxs-lookup"><span data-stu-id="568d4-245">This policy controls how long these resources are stored before they are automatically deleted (hello default is 30 days).</span></span> <span data-ttu-id="568d4-246">Ezeket a fájlokat is használhatja, a hibakeresés, valamint a teljesítményének hangolása lesz futtassa újra a jövőbeli hello feladatok.</span><span class="sxs-lookup"><span data-stu-id="568d4-246">You can use these files for debugging, and for performance-tuning of jobs that you'll rerun in hello future.</span></span>

<span data-ttu-id="568d4-247">toochange mennyi ideig tookeep feladat metaadatok és erőforrások:</span><span class="sxs-lookup"><span data-stu-id="568d4-247">toochange how long tookeep job metadata and resources:</span></span>

1. <span data-ttu-id="568d4-248">A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="568d4-248">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="568d4-249">Kattintson a **Tulajdonságok** elemre.</span><span class="sxs-lookup"><span data-stu-id="568d4-249">Click **Properties**.</span></span>
3. <span data-ttu-id="568d4-250">A **nap tooRetain Feladatlekérdezéseit**, helyezze át a hello csúszkát tooselect értéket, vagy hello érték hello mezőben adja meg.</span><span class="sxs-lookup"><span data-stu-id="568d4-250">Under **Days tooRetain Job Queries**, move hello slider tooselect a value, or enter hello value in hello text box.</span></span>  
4. <span data-ttu-id="568d4-251">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="568d4-251">Click **Save**.</span></span>

### <a name="job-level-policies"></a><span data-ttu-id="568d4-252">Feladat szintű szabályzatok</span><span class="sxs-lookup"><span data-stu-id="568d4-252">Job-level policies</span></span>
<span data-ttu-id="568d4-253">A feladat szintű házirendek, megadhatja a maximális ausztráliai hello és hello a legnagyobb prioritású virtuális gép, amely az egyes felhasználók (vagy a meghatározott biztonsági csoportok tagjai) feladatok, ugyanúgy lehet beállítani.</span><span class="sxs-lookup"><span data-stu-id="568d4-253">With job-level policies, you can control hello maximum AUs and hello maximum priority that individual users (or members of specific security groups) can set on jobs that they submit.</span></span> <span data-ttu-id="568d4-254">Ez lehetővé teszi, megadhatja a felhasználók hello költségeit.</span><span class="sxs-lookup"><span data-stu-id="568d4-254">This lets you control hello costs incurred by users.</span></span> <span data-ttu-id="568d4-255">Ezenkívül lehetővé teszi, hogy az ütemezett feladatok vezérlő hello hatással lehet a magas prioritású termelési a futó feladatok hello ugyanazt a Data Lake Analytics-fiókot.</span><span class="sxs-lookup"><span data-stu-id="568d4-255">It also lets you control hello effect that scheduled jobs might have on high-priority production jobs that are running in hello same Data Lake Analytics account.</span></span>

<span data-ttu-id="568d4-256">A Data Lake Analytics rendelkezik, amely lehet hello feladat szinten két házirendek:</span><span class="sxs-lookup"><span data-stu-id="568d4-256">Data Lake Analytics has two policies that you can set at hello job level:</span></span>

* <span data-ttu-id="568d4-257">**AU felső határ az egyes feladatok**: a felhasználók csak elküldhetik ausztráliai toothis számú feladatok.</span><span class="sxs-lookup"><span data-stu-id="568d4-257">**AU limit per job**: Users can only submit jobs that have up toothis number of AUs.</span></span> <span data-ttu-id="568d4-258">Alapértelmezés szerint ezt a határt hello ugyanaz, mint a hello AU maximális száma hello fiók.</span><span class="sxs-lookup"><span data-stu-id="568d4-258">By default, this limit is hello same as hello maximum AU limit for hello account.</span></span>
* <span data-ttu-id="568d4-259">**Prioritás**: csak a küldje el a kisebb vagy egyenlő toothis prioritásérték feladatok.</span><span class="sxs-lookup"><span data-stu-id="568d4-259">**Priority**: Users can only submit jobs that have a priority lower than or equal toothis value.</span></span> <span data-ttu-id="568d4-260">Vegye figyelembe, hogy a nagyobb szám azt jelenti, hogy a kisebb prioritással.</span><span class="sxs-lookup"><span data-stu-id="568d4-260">Note that a higher number means a lower priority.</span></span> <span data-ttu-id="568d4-261">Alapértelmezés szerint értéke too1, amely hello lehető legmagasabb prioritású.</span><span class="sxs-lookup"><span data-stu-id="568d4-261">By default, this is set too1, which is hello highest possible priority.</span></span>

<span data-ttu-id="568d4-262">Nincs minden fiókhoz beállított alapértelmezett házirend.</span><span class="sxs-lookup"><span data-stu-id="568d4-262">There is a default policy set on every account.</span></span> <span data-ttu-id="568d4-263">hello alapértelmezett házirend tooall felhasználók hello fiók vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="568d4-263">hello default policy applies tooall users of hello account.</span></span> <span data-ttu-id="568d4-264">További házirendjeinek beállítása meghatározott felhasználókhoz és csoportokhoz.</span><span class="sxs-lookup"><span data-stu-id="568d4-264">You can set additional policies for specific users and groups.</span></span> 

> [!NOTE]
> <span data-ttu-id="568d4-265">Fiók szintű és a feladat szintű házirendeket alkalmazza egyszerre.</span><span class="sxs-lookup"><span data-stu-id="568d4-265">Account-level policies and job-level policies apply simultaneously.</span></span>
>

#### <a name="add-a-policy-for-a-specific-user-or-group"></a><span data-ttu-id="568d4-266">Egy szabályzat egy adott felhasználó vagy csoport hozzáadása</span><span class="sxs-lookup"><span data-stu-id="568d4-266">Add a policy for a specific user or group</span></span>

1. <span data-ttu-id="568d4-267">A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="568d4-267">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="568d4-268">Kattintson a **Tulajdonságok** elemre.</span><span class="sxs-lookup"><span data-stu-id="568d4-268">Click **Properties**.</span></span>
3. <span data-ttu-id="568d4-269">A **feladat elküldése korlátok**, hello kattintson **házirend hozzáadása** gombra.</span><span class="sxs-lookup"><span data-stu-id="568d4-269">Under **Job Submission Limits**, click hello **Add Policy** button.</span></span> <span data-ttu-id="568d4-270">Ezután válassza ki, vagy adja meg a következő beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="568d4-270">Then, select or enter hello following settings:</span></span>
    1. <span data-ttu-id="568d4-271">**Házirend neve számítási**: Adja meg a házirend nevét, tooremind a hello házirend hello céljának.</span><span class="sxs-lookup"><span data-stu-id="568d4-271">**Compute Policy Name**: Enter a policy name, tooremind you of hello purpose of hello policy.</span></span>
    2. <span data-ttu-id="568d4-272">**Válassza ki a felhasználó vagy csoport**: válassza hello felhasználót vagy csoportot a házirend vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="568d4-272">**Select User or Group**: Select hello user or group this policy applies to.</span></span>
    3. <span data-ttu-id="568d4-273">**Hello feladat AU korlát beállítása**: toohello alkalmazó hello AU korlát beállítása a kijelölt felhasználót vagy csoportot.</span><span class="sxs-lookup"><span data-stu-id="568d4-273">**Set hello Job AU Limit**: Set hello AU limit that applies toohello selected user or group.</span></span>
    4. <span data-ttu-id="568d4-274">**Hello prioritás korlát beállítása**: hello prioritás korlát beállítása toohello alkalmazó kiválasztott felhasználó vagy csoport.</span><span class="sxs-lookup"><span data-stu-id="568d4-274">**Set hello Priority Limit**: Set hello priority limit that applies toohello selected user or group.</span></span>

4. <span data-ttu-id="568d4-275">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="568d4-275">Click **Ok**.</span></span>

5. <span data-ttu-id="568d4-276">hello új házirend megjelenik hello **alapértelmezett** házirend a tábla **feladat elküldése korlátok**.</span><span class="sxs-lookup"><span data-stu-id="568d4-276">hello new policy is listed in hello **Default** policy table, under **Job Submission Limits**.</span></span> 

#### <a name="delete-or-edit-an-existing-policy"></a><span data-ttu-id="568d4-277">Törölje vagy egy meglévő házirend szerkesztése</span><span class="sxs-lookup"><span data-stu-id="568d4-277">Delete or edit an existing policy</span></span>

1. <span data-ttu-id="568d4-278">A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="568d4-278">In hello Azure portal, go tooyour Data Lake Analytics account.</span></span>
2. <span data-ttu-id="568d4-279">Kattintson a **Tulajdonságok** elemre.</span><span class="sxs-lookup"><span data-stu-id="568d4-279">Click **Properties**.</span></span>
3. <span data-ttu-id="568d4-280">A **feladat elküldése korlátok**, keresés hello kívánt házirendet, tooedit.</span><span class="sxs-lookup"><span data-stu-id="568d4-280">Under **Job Submission Limits**, find hello policy you want tooedit.</span></span>
4.  <span data-ttu-id="568d4-281">toosee hello **törlése** és **szerkesztése** beállítások hello tábla hello jobb szélső oszlopában kattintson **...** .</span><span class="sxs-lookup"><span data-stu-id="568d4-281">toosee hello **Delete** and **Edit** options, in hello rightmost column of hello table, click **...**.</span></span>

### <a name="additional-resources-for-job-policies"></a><span data-ttu-id="568d4-282">További erőforrásokat a feladat házirendek</span><span class="sxs-lookup"><span data-stu-id="568d4-282">Additional resources for job policies</span></span>
* [<span data-ttu-id="568d4-283">Házirend által írt blogbejegyzés áttekintése</span><span class="sxs-lookup"><span data-stu-id="568d4-283">Policy overview blog post</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-overview/)
* [<span data-ttu-id="568d4-284">Fiók szintű szabályzatok által írt blogbejegyzés</span><span class="sxs-lookup"><span data-stu-id="568d4-284">Account-level policies blog post</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-account-level-policy/)
* [<span data-ttu-id="568d4-285">Feladat szintű szabályzatok által írt blogbejegyzés</span><span class="sxs-lookup"><span data-stu-id="568d4-285">Job-level policies blog post</span></span>](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-job-level-policy/)

## <a name="next-steps"></a><span data-ttu-id="568d4-286">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="568d4-286">Next steps</span></span>

* [<span data-ttu-id="568d4-287">Az Azure Data Lake Analytics áttekintése</span><span class="sxs-lookup"><span data-stu-id="568d4-287">Overview of Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="568d4-288">Ismerkedés a Data Lake Analytics hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="568d4-288">Get started with Data Lake Analytics by using hello Azure portal</span></span>](data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="568d4-289">Azure Data Lake Analytics kezelése az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="568d4-289">Manage Azure Data Lake Analytics by using Azure PowerShell</span></span>](data-lake-analytics-manage-use-powershell.md)

