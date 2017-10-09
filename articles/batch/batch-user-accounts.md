---
title: "a felhasználói fiókok az Azure Batch aaaRun feladatok |} Microsoft Docs"
description: "Feladatok futtatása az Azure Batch felhasználói fiókok konfigurálása"
services: batch
author: tamram
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: tamram
ms.openlocfilehash: 13d7d76451d89a3cca090c4ef24ed0ed781bbf09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-tasks-under-user-accounts-in-batch"></a><span data-ttu-id="c7540-103">Kötegelt a felhasználói fiókok feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="c7540-103">Run tasks under user accounts in Batch</span></span>

<span data-ttu-id="c7540-104">Mindig fusson a feladat, az Azure Batch egy felhasználói fiókkal.</span><span class="sxs-lookup"><span data-stu-id="c7540-104">A task in Azure Batch always runs under a user account.</span></span> <span data-ttu-id="c7540-105">Alapértelmezés szerint a feladatok futtathatók általános jogú felhasználói fiókokat, a rendszergazdai engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="c7540-105">By default, tasks run under standard user accounts, without administrator permissions.</span></span> <span data-ttu-id="c7540-106">Ezek az alapértelmezett felhasználói fiók beállítások általában elegendők.</span><span class="sxs-lookup"><span data-stu-id="c7540-106">These default user account settings are typically sufficient.</span></span> <span data-ttu-id="c7540-107">Bizonyos esetekben azonban esetén hasznos toobe képes tooconfigure hello felhasználói fiók alatt, amely egy feladat toorun.</span><span class="sxs-lookup"><span data-stu-id="c7540-107">For certain scenarios, however, it's useful toobe able tooconfigure hello user account under which you want a task toorun.</span></span> <span data-ttu-id="c7540-108">Ez a cikk ismerteti, amelyek hello típusú felhasználói fiókokat, és hogyan konfigurálhatók a forgatókönyvnek.</span><span class="sxs-lookup"><span data-stu-id="c7540-108">This article discusses hello types of user accounts and how you can configure them for your scenario.</span></span>

## <a name="types-of-user-accounts"></a><span data-ttu-id="c7540-109">Felhasználói fiókok típusai</span><span class="sxs-lookup"><span data-stu-id="c7540-109">Types of user accounts</span></span>

<span data-ttu-id="c7540-110">Az Azure Batch kétféle típusú felhasználói fiókokat biztosít az éppen futó feladatok:</span><span class="sxs-lookup"><span data-stu-id="c7540-110">Azure Batch provides two types of user accounts for running tasks:</span></span>

- <span data-ttu-id="c7540-111">**Automatikus-felhasználói fiókokat.**</span><span class="sxs-lookup"><span data-stu-id="c7540-111">**Auto-user accounts.**</span></span> <span data-ttu-id="c7540-112">Automatikus-felhasználói fiókok olyan beépített felhasználói fiókok, hello Batch szolgáltatás automatikusan létrehozza.</span><span class="sxs-lookup"><span data-stu-id="c7540-112">Auto-user accounts are built-in user accounts that are created automatically by hello Batch service.</span></span> <span data-ttu-id="c7540-113">Alapértelmezés szerint feladatok automatikus felhasználói fiókkal futtassa.</span><span class="sxs-lookup"><span data-stu-id="c7540-113">By default, tasks run under an auto-user account.</span></span> <span data-ttu-id="c7540-114">Egy feladat tooindicate alatt automatikus-felhasználói fiókot a feladat futni hello automatikus felhasználói előírása konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="c7540-114">You can configure hello auto-user specification for a task tooindicate under which auto-user account a task should run.</span></span> <span data-ttu-id="c7540-115">hello automatikus felhasználói megadását lehetővé teszi toospecify hello jogosultságszint-emelés szint és hello automatikus-felhasználói fiókot hello feladat hatókörében.</span><span class="sxs-lookup"><span data-stu-id="c7540-115">hello auto-user specification allows you toospecify hello elevation level and scope of hello auto-user account that will run hello task.</span></span> 

- <span data-ttu-id="c7540-116">**Egy névvel ellátott felhasználói fiókot.**</span><span class="sxs-lookup"><span data-stu-id="c7540-116">**A named user account.**</span></span> <span data-ttu-id="c7540-117">Megadhat egy vagy több elnevezett felhasználói fiókok készlet hello készlet létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="c7540-117">You can specify one or more named user accounts for a pool when you create hello pool.</span></span> <span data-ttu-id="c7540-118">Minden felhasználói fiókhoz hello készlet minden egyes csomóponton jön létre.</span><span class="sxs-lookup"><span data-stu-id="c7540-118">Each user account is created on each node of hello pool.</span></span> <span data-ttu-id="c7540-119">Továbbá toohello fiók neve, megadhatja hello felhasználói fiók jelszavát, jogosultságszint-emelés szinten, valamint a Linux-készletek, hello titkos SSH-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="c7540-119">In addition toohello account name, you specify hello user account password, elevation level, and, for Linux pools, hello SSH private key.</span></span> <span data-ttu-id="c7540-120">Ha hozzáad egy feladatot, nevű felhasználói fiók, amely alatt ez a feladat futni hello is megadhat.</span><span class="sxs-lookup"><span data-stu-id="c7540-120">When you add a task, you can specify hello named user account under which that task should run.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="c7540-121">hello Batch szolgáltatás 2017-01-01.4.0 tartalmazza, amely megköveteli, hogy a kód toocall adott verziók frissítése használhatatlanná tévő változást.</span><span class="sxs-lookup"><span data-stu-id="c7540-121">hello Batch service version 2017-01-01.4.0 introduces a breaking change that requires that you update your code toocall that version.</span></span> <span data-ttu-id="c7540-122">Ha áttelepítése kód köteg egy korábbi verziójából származó, vegye figyelembe, hogy hello **runElevated** tulajdonság már nem támogatott a REST API vagy kötegelt hello klienskódtárak segítségével.</span><span class="sxs-lookup"><span data-stu-id="c7540-122">If you are migrating code from an older version of Batch, note that hello **runElevated** property is no longer supported in hello REST API or Batch client libraries.</span></span> <span data-ttu-id="c7540-123">Új használata hello **userIdentity** egy feladat toospecify jogosultságszint-emelés szint tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="c7540-123">Use hello new **userIdentity** property of a task toospecify elevation level.</span></span> <span data-ttu-id="c7540-124">Hello című témakörben talál [frissítse a kódot toohello legújabb kötegelt ügyféloldali kódtár](#update-your-code-to-the-latest-batch-client-library) kapcsolatos gyors útmutatást a kötegelt kód frissítése használata egyik hello klienskódtárak segítségével.</span><span class="sxs-lookup"><span data-stu-id="c7540-124">See hello section titled [Update your code toohello latest Batch client library](#update-your-code-to-the-latest-batch-client-library) for quick guidelines for updating your Batch code if you are using one of hello client libraries.</span></span>
>
>

> [!NOTE] 
> <span data-ttu-id="c7540-125">Ebben a cikkben ismertetett hello felhasználói fiókok nem támogatják az Remote Desktop Protocol (RDP) vagy a Secure Shell (SSH), a biztonsági okokból.</span><span class="sxs-lookup"><span data-stu-id="c7540-125">hello user accounts discussed in this article do not support Remote Desktop Protocol (RDP) or Secure Shell (SSH), for security reasons.</span></span> 
>
> <span data-ttu-id="c7540-126">tooconnect tooa csomóponton futó hello Linux virtuálisgép-konfiguráció ssh, lásd: [használata a távoli asztal tooa Linux virtuális gép az Azure-ban](../virtual-machines/virtual-machines-linux-use-remote-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="c7540-126">tooconnect tooa node running hello Linux virtual machine configuration via SSH, see [Use Remote Desktop tooa Linux VM in Azure](../virtual-machines/virtual-machines-linux-use-remote-desktop.md).</span></span> <span data-ttu-id="c7540-127">Tekintse meg a Windows rendszert futtató RDP,-kapcsolaton keresztül tooconnect toonodes [csatlakozás a Windows Server virtuális gép tooa](../virtual-machines/windows/connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="c7540-127">tooconnect toonodes running Windows via RDP, see [Connect tooa Windows Server VM](../virtual-machines/windows/connect-logon.md).</span></span><br /><br />
> <span data-ttu-id="c7540-128">Tekintse meg a tooconnect tooa csomóponton futó hello felhőalapú szolgáltatás konfigurációja RDP,-kapcsolaton keresztül [engedélyezése a távoli asztali kapcsolat az Azure Cloud Services szerepkör](../cloud-services/cloud-services-role-enable-remote-desktop-new-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c7540-128">tooconnect tooa node running hello cloud service configuration via RDP, see [Enable Remote Desktop Connection for a Role in Azure Cloud Services](../cloud-services/cloud-services-role-enable-remote-desktop-new-portal.md).</span></span>
>
>

## <a name="user-account-access-toofiles-and-directories"></a><span data-ttu-id="c7540-129">Felhasználói fiók hozzáférési toofiles és könyvtárak</span><span class="sxs-lookup"><span data-stu-id="c7540-129">User account access toofiles and directories</span></span>

<span data-ttu-id="c7540-130">Az automatikus-felhasználói fiókkal, és egy névvel ellátott felhasználói fiókot is rendelkezik olvasási/írási hozzáférést toohello feladatütemezési munkakönyvtár, megosztott és többpéldányos feladatok könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="c7540-130">Both an auto-user account and a named user account have read/write access toohello task’s working directory, shared directory, and multi-instance tasks directory.</span></span> <span data-ttu-id="c7540-131">Mindkét típusú fiókok olyan könyvtárai olvasási hozzáférés toohello indítás és a feladat előkészítése.</span><span class="sxs-lookup"><span data-stu-id="c7540-131">Both types of accounts have read access toohello startup and job preparation directories.</span></span>

<span data-ttu-id="c7540-132">Ha egy feladat fut. hello a kezdő tevékenységre, hello feladat futtatásához használt fióknak van olvasási és írási hozzáférése toohello kezdőkönyvtára feladat.</span><span class="sxs-lookup"><span data-stu-id="c7540-132">If a task runs under hello same account that was used for running a start task, hello task has read-write access toohello start task directory.</span></span> <span data-ttu-id="c7540-133">Hasonlóképpen, ha egy feladat fut. hello ugyanaz a feladat előkészítése tevékenységet, hello feladat futtatásához használt fióknak olvasási és írási hozzáférése toohello feladat előkészítése tevékenység könyvtár.</span><span class="sxs-lookup"><span data-stu-id="c7540-133">Similarly, if a task runs under hello same account that was used for running a job preparation task, hello task has read-write access toohello job preparation task directory.</span></span> <span data-ttu-id="c7540-134">Ha egy feladat fut, mint hello kezdő tevékenység vagy a feladat előkészítése tevékenységet egy másik fiókból, hello tevékenység csak olvasási hozzáféréssel toohello megfelelő directory rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c7540-134">If a task runs under a different account than hello start task or job preparation task, then hello task has only read access toohello respective directory.</span></span>

<span data-ttu-id="c7540-135">A fájlok és könyvtárak feladat eléréséhez további információkért lásd: [Develop nagyméretű párhuzamos számítási solutions a kötegelt](batch-api-basics.md#files-and-directories).</span><span class="sxs-lookup"><span data-stu-id="c7540-135">For more information on accessing files and directories from a task, see [Develop large-scale parallel compute solutions with Batch](batch-api-basics.md#files-and-directories).</span></span>

## <a name="elevated-access-for-tasks"></a><span data-ttu-id="c7540-136">Emelt szintű hozzáférés feladatokhoz</span><span class="sxs-lookup"><span data-stu-id="c7540-136">Elevated access for tasks</span></span> 

<span data-ttu-id="c7540-137">hello felhasználói fiók jogosultságszint-emelés szintjét jelzi, hogy fut-e a feladat emelt szintű hozzáféréssel rendelkező.</span><span class="sxs-lookup"><span data-stu-id="c7540-137">hello user account's elevation level indicates whether a task runs with elevated access.</span></span> <span data-ttu-id="c7540-138">Automatikus-felhasználói fiókkal és a egy névvel ellátott felhasználói fiókot is futtassa emelt szintű hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="c7540-138">Both an auto-user account and a named user account can run with elevated access.</span></span> <span data-ttu-id="c7540-139">Jogosultságszint-emelés szint a hello két lehetőségek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="c7540-139">hello two options for elevation level are:</span></span>

- <span data-ttu-id="c7540-140">**NonAdmin:** hello feladat fut, az általános jogú felhasználó emelt szintű hozzáférés nélkül.</span><span class="sxs-lookup"><span data-stu-id="c7540-140">**NonAdmin:** hello task runs as a standard user without elevated access.</span></span> <span data-ttu-id="c7540-141">hello alapértelmezett jogosultságszint-emelés kötegelt felhasználói fiók szintje mindig **NonAdmin**.</span><span class="sxs-lookup"><span data-stu-id="c7540-141">hello default elevation level for a Batch user account is always **NonAdmin**.</span></span>
- <span data-ttu-id="c7540-142">**Felügyeleti:** hello feladat emelt szintű hozzáféréssel rendelkező felhasználóként fut, és teljes körű felügyeleti engedélyekkel működik.</span><span class="sxs-lookup"><span data-stu-id="c7540-142">**Admin:** hello task runs as a user with elevated access and operates with full Administrator permissions.</span></span> 

## <a name="auto-user-accounts"></a><span data-ttu-id="c7540-143">Automatikus-felhasználói fiókok</span><span class="sxs-lookup"><span data-stu-id="c7540-143">Auto-user accounts</span></span>

<span data-ttu-id="c7540-144">Alapértelmezés szerint feladatokat futtató kötegben automatikus felhasználói fiókkal, az általános jogú felhasználó emelt szintű hozzáférés nélkül, és a feladat hatókörében.</span><span class="sxs-lookup"><span data-stu-id="c7540-144">By default, tasks run in Batch under an auto-user account, as a standard user without elevated access, and with task scope.</span></span> <span data-ttu-id="c7540-145">Hello automatikus felhasználói specification feladat hatókör konfigurálásakor hello Batch szolgáltatás ezt a feladatot csak egy automatikus-felhasználói fiókot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c7540-145">When hello auto-user specification is configured for task scope, hello Batch service creates an auto-user account for that task only.</span></span>

<span data-ttu-id="c7540-146">hello alternatív tootask hatóköre készlet hatókör.</span><span class="sxs-lookup"><span data-stu-id="c7540-146">hello alternative tootask scope is pool scope.</span></span> <span data-ttu-id="c7540-147">Hello automatikus felhasználói megadása egy feladatot az alkalmazáskészlet hatókör konfigurálásakor hello feladat elérhető tooany feladat hello készlet automatikus felhasználói fiók alatt fut.</span><span class="sxs-lookup"><span data-stu-id="c7540-147">When hello auto-user specification for a task is configured for pool scope, hello task runs under an auto-user account that is available tooany task in hello pool.</span></span> <span data-ttu-id="c7540-148">Készlet hatókör kapcsolatos további információkért lásd: hello című [feladat futtatása, automatikus felhasználói készlet hatókörű hello](#run-a-task-as-the-autouser-with-pool-scope).</span><span class="sxs-lookup"><span data-stu-id="c7540-148">For more information about pool scope, see hello section titled [Run a task as hello auto-user with pool scope](#run-a-task-as-the-autouser-with-pool-scope).</span></span>   

<span data-ttu-id="c7540-149">hello alapértelmezett hatókör nem azonos a Windows és Linux-csomópontok:</span><span class="sxs-lookup"><span data-stu-id="c7540-149">hello default scope is different on Windows and Linux nodes:</span></span>

- <span data-ttu-id="c7540-150">Windows csomópontján feladatok futtathatók feladat hatókör alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="c7540-150">On Windows nodes, tasks run under task scope by default.</span></span>
- <span data-ttu-id="c7540-151">Linux-csomópontok a készlet hatókör mindig futnia.</span><span class="sxs-lookup"><span data-stu-id="c7540-151">Linux nodes always run under pool scope.</span></span>

<span data-ttu-id="c7540-152">Nincsenek hello automatikus felhasználói megadását, amelyek mindegyike megfelel tooa egyedi automatikus-felhasználói fiók négy lehetséges konfigurációkat:</span><span class="sxs-lookup"><span data-stu-id="c7540-152">There are four possible configurations for hello auto-user specification, each of which corresponds tooa unique auto-user account:</span></span>

- <span data-ttu-id="c7540-153">A nem rendszergazda hozzáférést feladat hatókörrel (hello alapértelmezett automatikus felhasználói specifikáció)</span><span class="sxs-lookup"><span data-stu-id="c7540-153">Non-admin access with task scope (hello default auto-user specification)</span></span>
- <span data-ttu-id="c7540-154">A feladat hatókörű (emelt szintű) rendszergazdai hozzáférés</span><span class="sxs-lookup"><span data-stu-id="c7540-154">Admin (elevated) access with task scope</span></span>
- <span data-ttu-id="c7540-155">A nem rendszergazda hozzáférést készlet hatókörű</span><span class="sxs-lookup"><span data-stu-id="c7540-155">Non-admin access with pool scope</span></span>
- <span data-ttu-id="c7540-156">Rendszergazdai hozzáférés készlet hatókörű</span><span class="sxs-lookup"><span data-stu-id="c7540-156">Admin access with pool scope</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="c7540-157">Feladat hatókör alatt futó feladatok a csomópont nem rendelkeznek tényleges hozzáférési tooother feladatát.</span><span class="sxs-lookup"><span data-stu-id="c7540-157">Tasks running under task scope do not have de facto access tooother tasks on a node.</span></span> <span data-ttu-id="c7540-158">Azonban egy rosszindulatú felhasználó hozzáférési toohello fiókhoz sikerült megoldható, ez a korlátozás, amely rendszergazdai jogosultságokkal futtatja, és más tevékenység könyvtárak fér hozzá a feladat elküldése.</span><span class="sxs-lookup"><span data-stu-id="c7540-158">However, a malicious user with access toohello account could work around this restriction by submitting a task that runs with administrator privileges and accesses other task directories.</span></span> <span data-ttu-id="c7540-159">Egy rosszindulatú felhasználó RDP- vagy SSH-tooconnect tooa csomópont is használhatja.</span><span class="sxs-lookup"><span data-stu-id="c7540-159">A malicious user could also use RDP or SSH tooconnect tooa node.</span></span> <span data-ttu-id="c7540-160">Fontos tooprotect hozzáférés tooyour kötegelt fiók kulcsok tooprevent ilyen esetben.</span><span class="sxs-lookup"><span data-stu-id="c7540-160">It's important tooprotect access tooyour Batch account keys tooprevent such a scenario.</span></span> <span data-ttu-id="c7540-161">Amennyiben azt gyanítja, hogy a fiók sérült, lehet, hogy tooregenerate a kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="c7540-161">If you suspect your account may have been compromised, be sure tooregenerate your keys.</span></span>
>
>

### <a name="run-a-task-as-an-auto-user-with-elevated-access"></a><span data-ttu-id="c7540-162">Emelt szintű hozzáféréssel rendelkező automatikus-felhasználóként feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="c7540-162">Run a task as an auto-user with elevated access</span></span>

<span data-ttu-id="c7540-163">Rendszergazdai jogosultságokkal hello automatikus felhasználói előírása konfigurálhatja, ha a toorun kötegazonosítójú feladat emelt szintű hozzáférés szükséges.</span><span class="sxs-lookup"><span data-stu-id="c7540-163">You can configure hello auto-user specification for administrator privileges when you need toorun a task with elevated access.</span></span> <span data-ttu-id="c7540-164">A kezdő tevékenység Előfordulhat például, emelt szintű hozzáférés tooinstall szoftver hello csomóponton.</span><span class="sxs-lookup"><span data-stu-id="c7540-164">For example, a start task may need elevated access tooinstall software on hello node.</span></span>

> [!NOTE] 
> <span data-ttu-id="c7540-165">Általában érdemes toouse emelt szintű hozzáférés csak szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="c7540-165">In general, it's best toouse elevated access only when necessary.</span></span> <span data-ttu-id="c7540-166">A bevált gyakorlat része hello minimális jogosultság szükséges tooachieve hello kívánt eredmény biztosítása.</span><span class="sxs-lookup"><span data-stu-id="c7540-166">Best practices recommend granting hello minimum privilege necessary tooachieve hello desired outcome.</span></span> <span data-ttu-id="c7540-167">Például ha egy kezdő tevékenység szoftvereket telepít hello aktuális felhasználó, ahelyett, hogy a felhasználók lehet képes tooavoid emelt szintű hozzáférés tootasks megadása.</span><span class="sxs-lookup"><span data-stu-id="c7540-167">For example, if a start task installs software for hello current user, instead of for all users, you may be able tooavoid granting elevated access tootasks.</span></span> <span data-ttu-id="c7540-168">Hello automatikus-felhasználó megadása az alkalmazáskészlet hatókörrel és a nem rendszergazdai hozzáférést minden olyan feladatokat, amelyeket a fiókot használja, beleértve a hello kezdő tevékenység hello toorun konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="c7540-168">You can configure hello auto-user specification for pool scope and non-admin access for all tasks that need toorun under hello same account, including hello start task.</span></span> 
>
>

<span data-ttu-id="c7540-169">a következő kódrészletek hello megjelenítése hogyan tooconfigure hello automatikus felhasználói megadását.</span><span class="sxs-lookup"><span data-stu-id="c7540-169">hello following code snippets show how tooconfigure hello auto-user specification.</span></span> <span data-ttu-id="c7540-170">hello példák túl hello jogosultságszint-emelés szintjének beállítása`Admin` és hatókör túl hello`Task`.</span><span class="sxs-lookup"><span data-stu-id="c7540-170">hello examples set hello elevation level too`Admin` and hello scope too`Task`.</span></span> <span data-ttu-id="c7540-171">Feladat hatókör hello alapértelmezett beállítás, de a hello szakét példa.</span><span class="sxs-lookup"><span data-stu-id="c7540-171">Task scope is hello default setting, but is included here for hello sake of example.</span></span>

#### <a name="batch-net"></a><span data-ttu-id="c7540-172">Batch .NET</span><span class="sxs-lookup"><span data-stu-id="c7540-172">Batch .NET</span></span>

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin, scope: AutoUserScope.Task));
```
#### <a name="batch-java"></a><span data-ttu-id="c7540-173">Kötegelt Java</span><span class="sxs-lookup"><span data-stu-id="c7540-173">Batch Java</span></span>

```java
taskToAdd.withId(taskId)
        .withUserIdentity(new UserIdentity()
            .withAutoUser(new AutoUserSpecification()
                .withElevationLevel(ElevationLevel.ADMIN))
                .withScope(AutoUserScope.TASK));
        .withCommandLine("cmd /c echo hello");                        
```

#### <a name="batch-python"></a><span data-ttu-id="c7540-174">Batch Python</span><span class="sxs-lookup"><span data-stu-id="c7540-174">Batch Python</span></span>

```python
user = batchmodels.UserIdentity(
    auto_user=batchmodels.AutoUserSpecification(
        elevation_level=batchmodels.ElevationLevel.admin,
        scope=batchmodels.AutoUserScope.task))
task = batchmodels.TaskAddParameter(
    id='task_1',
    command_line='cmd /c "echo hello world"',
    user_identity=user)
batch_client.task.add(job_id=jobid, task=task)
```

### <a name="run-a-task-as-an-auto-user-with-pool-scope"></a><span data-ttu-id="c7540-175">Készlet hatókörű automatikus-felhasználóként feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="c7540-175">Run a task as an auto-user with pool scope</span></span>

<span data-ttu-id="c7540-176">Ha egy csomópont ki van építve, két készlet kiterjedő automatikus-felhasználói fiókok létrejöttek hello készlet minden egyes csomópontja, egy emelt szintű hozzáféréssel rendelkező és egy emelt szintű hozzáférés nélkül.</span><span class="sxs-lookup"><span data-stu-id="c7540-176">When a node is provisioned, two pool-wide auto-user accounts are created on each node in hello pool, one with elevated access, and one without elevated access.</span></span> <span data-ttu-id="c7540-177">Ezeket a két készlet kiterjedő automatikus felhasználói fiókokat valamelyike hello feladatot hello automatikus-felhasználó hatókör toopool hatókör egy adott tevékenység beállítása futtatja.</span><span class="sxs-lookup"><span data-stu-id="c7540-177">Setting hello auto-user's scope toopool scope for a given task runs hello task under one of these two pool-wide auto-user accounts.</span></span> 

<span data-ttu-id="c7540-178">Hello automatikus-felhasználó készlet hatókör megadása esetén minden olyan feladat, amely rendszergazdai jogosultságokkal futtassa futni hello ugyanazt a készlet kiterjedő automatikus-felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="c7540-178">When you specify pool scope for hello auto-user, all tasks that run with administrator access run under hello same pool-wide auto-user account.</span></span> <span data-ttu-id="c7540-179">Ehhez hasonlóan rendszergazdai jogosultságok nélküli futtatott feladatok is futtathatók egy alkalmazáskészlet kiterjedő automatikus felhasználói fiókkal.</span><span class="sxs-lookup"><span data-stu-id="c7540-179">Similarly, tasks that run without administrator permissions also run under a single pool-wide auto-user account.</span></span> 

> [!NOTE] 
> <span data-ttu-id="c7540-180">hello két készlet kiterjedő automatikus-felhasználói fiókok külön fiók is.</span><span class="sxs-lookup"><span data-stu-id="c7540-180">hello two pool-wide auto-user accounts are separate accounts.</span></span> <span data-ttu-id="c7540-181">Hello készlet szintű rendszergazdai fiók alatt futó feladatok nem adatok megosztása a hello szabványos fiókkal, és ez fordítva is igaz futó feladatok.</span><span class="sxs-lookup"><span data-stu-id="c7540-181">Tasks running under hello pool-wide administrative account cannot share data with tasks running under hello standard account, and vice versa.</span></span> 
>
>

<span data-ttu-id="c7540-182">hello előny toorunning alapján ugyanazt a auto-felhasználói fiókot az, hogy feladatokat is képes tooshare adatok más hello futó feladatok hello ugyanahhoz a csomóponthoz.</span><span class="sxs-lookup"><span data-stu-id="c7540-182">hello advantage toorunning under hello same auto-user account is that tasks are able tooshare data with other tasks running on hello same node.</span></span>

<span data-ttu-id="c7540-183">Egy forgatókönyvet, ahol az éppen futó feladatok hello két készlet kiterjedő automatikus-felhasználói fiókok valamelyike akkor hasznos, titkos kulcsok tevékenységek közötti megosztása szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c7540-183">Sharing secrets between tasks is one scenario where running tasks under one of hello two pool-wide auto-user accounts is useful.</span></span> <span data-ttu-id="c7540-184">Tegyük fel, hogy a kezdő tevékenységre kell tooprovision hello csomópont, amellyel más feladatok, titkos kulcs.</span><span class="sxs-lookup"><span data-stu-id="c7540-184">For example, suppose a start task needs tooprovision a secret onto hello node that other tasks can use.</span></span> <span data-ttu-id="c7540-185">Hello Windows Data Protection API (DPAPI) használata, de rendszergazdai jogosultságokat igényel.</span><span class="sxs-lookup"><span data-stu-id="c7540-185">You could use hello Windows Data Protection API (DPAPI), but it requires administrator privileges.</span></span> <span data-ttu-id="c7540-186">Ehelyett hello titkos hello felhasználói szintű védelmet biztosíthat.</span><span class="sxs-lookup"><span data-stu-id="c7540-186">Instead, you can protect hello secret at hello user level.</span></span> <span data-ttu-id="c7540-187">Ugyanazzal a fiókkal hozzáférhet hello alatt futó feladatok hello titkos emelt szintű hozzáférés nélkül.</span><span class="sxs-lookup"><span data-stu-id="c7540-187">Tasks running under hello same user account can access hello secret without elevated access.</span></span>

<span data-ttu-id="c7540-188">Ossza meg egy másik forgatókönyv, ahol érdemes lehet toorun feladatok készlet hatókörű egy automatikus-felhasználói fiók alatt egy olyan Message Passing Interface (MPI) fájl.</span><span class="sxs-lookup"><span data-stu-id="c7540-188">Another scenario where you may want toorun tasks under an auto-user account with pool scope is a Message Passing Interface (MPI) file share.</span></span> <span data-ttu-id="c7540-189">Egy MPI fájlmegosztás akkor hasznos, ha hello MPI feladat kell toowork a hello csomópontjának hello azonos fájladatok.</span><span class="sxs-lookup"><span data-stu-id="c7540-189">An MPI file share is useful when hello nodes in hello MPI task need toowork on hello same file data.</span></span> <span data-ttu-id="c7540-190">hello átjárócsomópont létrehoz egy fájlmegosztást, amely hello gyermekcsomópontok hozzáférhetnek, abban az esetben, ha a hello futnak ugyanazt a auto-felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="c7540-190">hello head node creates a file share that hello child nodes can access if they are running under hello same auto-user account.</span></span> 

<span data-ttu-id="c7540-191">a következő kódrészletet hello hello automatikus-felhasználó hatókör toopool hatókör meg olyan feladatra, a Batch .NET állítja be.</span><span class="sxs-lookup"><span data-stu-id="c7540-191">hello following code snippet sets hello auto-user's scope toopool scope for a task in Batch .NET.</span></span> <span data-ttu-id="c7540-192">hello jogosultságszint-emelés szint nincs megadva, ezért hello feladat hello szabványos készlet kiterjedő automatikus-felhasználói fiók alatt fut.</span><span class="sxs-lookup"><span data-stu-id="c7540-192">hello elevation level is omitted, so hello task runs under hello standard pool-wide auto-user account.</span></span>

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(scope: AutoUserScope.Pool));
```

## <a name="named-user-accounts"></a><span data-ttu-id="c7540-193">Elnevezett felhasználói fiókok</span><span class="sxs-lookup"><span data-stu-id="c7540-193">Named user accounts</span></span>

<span data-ttu-id="c7540-194">Itt megadhatja, hogy a program elnevezett felhasználói fiókokat, amikor a készletet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c7540-194">You can define named user accounts when you create a pool.</span></span> <span data-ttu-id="c7540-195">Egy névvel ellátott felhasználói fiók rendelkezik, egy nevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="c7540-195">A named user account has a name and password that you provide.</span></span> <span data-ttu-id="c7540-196">Megadhatja, hogy hello jogosultságszint-emelés szintje egy névvel ellátott felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="c7540-196">You can specify hello elevation level for a named user account.</span></span> <span data-ttu-id="c7540-197">Linux-csomópontok titkos SSH-kulcsot is megadhatja.</span><span class="sxs-lookup"><span data-stu-id="c7540-197">For Linux nodes, you can also provide an SSH private key.</span></span>

<span data-ttu-id="c7540-198">Hello készlet összes csomópontján nevű felhasználói fiók létezik és elérhető tooall feladatok fut azokat a csomópontokat.</span><span class="sxs-lookup"><span data-stu-id="c7540-198">A named user account exists on all nodes in hello pool and is available tooall tasks running on those nodes.</span></span> <span data-ttu-id="c7540-199">Nevesített felhasználók készlet tetszőleges számú adhatók meg.</span><span class="sxs-lookup"><span data-stu-id="c7540-199">You may define any number of named users for a pool.</span></span> <span data-ttu-id="c7540-200">Feladat vagy tevékenység gyűjtemény hozzáadásakor adja meg, hogy hello feladat fut. hello nevű hello készlet definiált felhasználói fiókok közül.</span><span class="sxs-lookup"><span data-stu-id="c7540-200">When you add a task or task collection, you can specify that hello task runs under one of hello named user accounts defined on hello pool.</span></span>

<span data-ttu-id="c7540-201">Egy névvel ellátott felhasználói fiókot akkor hasznos, ha azt szeretné, hogy toorun egy feladat összes tevékenységében a hello ugyanazzal a fiókkal, azonban elkülöníti azokat az egyéb feladatokat hello a futó feladatok ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="c7540-201">A named user account is useful when you want toorun all tasks in a job under hello same user account, but isolate them from tasks running in other jobs at hello same time.</span></span> <span data-ttu-id="c7540-202">Például minden feladat nevű felhasználót kell létrehozni, és az adott nevű felhasználói fiók minden feladat feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="c7540-202">For example, you can create a named user for each job, and run each job's tasks under that named user account.</span></span> <span data-ttu-id="c7540-203">Minden feladat dolgozhat ugyanazon a titkos kulcs a saját feladatokhoz, de nem váltanak futó feladatok.</span><span class="sxs-lookup"><span data-stu-id="c7540-203">Each job can then share a secret with its own tasks, but not with tasks running in other jobs.</span></span>

<span data-ttu-id="c7540-204">Egy elnevezett felhasználói fiók toorun egy feladatot, amely olyan engedélyeket ad meg a külső erőforrások, például fájlmegosztásokat is használható.</span><span class="sxs-lookup"><span data-stu-id="c7540-204">You can also use a named user account toorun a task that sets permissions on external resources such as file shares.</span></span> <span data-ttu-id="c7540-205">Elnevezett felhasználói fiókkal hello felhasználói identitás szabályozza, és használhatja a felhasználói identitás tooset engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="c7540-205">With a named user account, you control hello user identity and can use that user identity tooset permissions.</span></span>  

<span data-ttu-id="c7540-206">Nevesített felhasználó fiókok lehetővé teszik a jelszó nélküli SSH Linux-csomópontok között.</span><span class="sxs-lookup"><span data-stu-id="c7540-206">Named user accounts enable password-less SSH between Linux nodes.</span></span> <span data-ttu-id="c7540-207">Linux-csomópontok toorun többpéldányos feladatok igénylő használható egy névvel ellátott felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="c7540-207">You can use a named user account with Linux nodes that need toorun multi-instance tasks.</span></span> <span data-ttu-id="c7540-208">Minden csomópont hello készletben futtathatnak feladatokat hello teljes készletében egy felhasználói fiókkal.</span><span class="sxs-lookup"><span data-stu-id="c7540-208">Each node in hello pool can run tasks under a user account defined on hello whole pool.</span></span> <span data-ttu-id="c7540-209">Többpéldányos feladatokkal kapcsolatos további információkért lásd: [használata többszörös\-példány feladatok toorun MPI alkalmazások](batch-mpi.md).</span><span class="sxs-lookup"><span data-stu-id="c7540-209">For more information about multi-instance tasks, see [Use multi\-instance tasks toorun MPI applications](batch-mpi.md).</span></span>

### <a name="create-named-user-accounts"></a><span data-ttu-id="c7540-210">Elnevezett felhasználói fiókok létrehozása</span><span class="sxs-lookup"><span data-stu-id="c7540-210">Create named user accounts</span></span>

<span data-ttu-id="c7540-211">toocreate nevű kötegben, a felhasználói fiókok hozzáadása a felhasználói fiókok toohello készlet gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="c7540-211">toocreate named user accounts in Batch, add a collection of user accounts toohello pool.</span></span> <span data-ttu-id="c7540-212">hello alábbi kódtöredékek bemutatják, hogyan toocreate nevű felhasználói fiókokat a .NET, Java és Python.</span><span class="sxs-lookup"><span data-stu-id="c7540-212">hello following code snippets show how toocreate named user accounts in .NET, Java, and Python.</span></span> <span data-ttu-id="c7540-213">Ezek az részletek megjelenítése hogyan code toocreate rendszergazda és a nem rendszergazdai fiókok, a készlet neve.</span><span class="sxs-lookup"><span data-stu-id="c7540-213">These code snippets show how toocreate both admin and non-admin named accounts on a pool.</span></span> <span data-ttu-id="c7540-214">hello példák létrehozása szolgáltatással hello felhőalapú szolgáltatás konfigurációja, de hello azonos közelítse hello virtuálisgép-konfiguráció használata Windows vagy Linux készlet létrehozásakor használható.</span><span class="sxs-lookup"><span data-stu-id="c7540-214">hello examples create pools using hello cloud service configuration, but you use hello same approach when creating a Windows or Linux pool using hello virtual machine configuration.</span></span>

#### <a name="batch-net-example-windows"></a><span data-ttu-id="c7540-215">Batch .NET típusú példát (Windows)</span><span class="sxs-lookup"><span data-stu-id="c7540-215">Batch .NET example (Windows)</span></span>

```csharp
CloudPool pool = null;
Console.WriteLine("Creating pool [{0}]...", poolId);

// Create a pool using hello cloud service configuration.
pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 3,                                                         
    virtualMachineSize: "small",                                                
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));   

// Add named user accounts.
pool.UserAccounts = new List<UserAccount>
{
    new UserAccount("adminUser", "xyz123", ElevationLevel.Admin),
    new UserAccount("nonAdminUser", "123xyz", ElevationLevel.NonAdmin),
};

// Commit hello pool.
await pool.CommitAsync();
```

#### <a name="batch-net-example-linux"></a><span data-ttu-id="c7540-216">Batch .NET típusú példát (Linux)</span><span class="sxs-lookup"><span data-stu-id="c7540-216">Batch .NET example (Linux)</span></span>

```csharp
CloudPool pool = null;

// Obtain a collection of all available node agent SKUs.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of hello VM image toouse.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.Sku.Contains("14.04");

// Obtain hello first node agent SKU in hello collection that matches
// Ubuntu Server 14.04. 
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create hello virtual machine configuration toouse toocreate hello pool.
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(imageReference, ubuntuAgentSku.Id);

Console.WriteLine("Creating pool [{0}]...", poolId);

// Create hello unbound pool.
pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 3,                                             
    virtualMachineSize: "Standard_A1",                                      
    virtualMachineConfiguration: virtualMachineConfiguration);                  

// Add named user accounts.
pool.UserAccounts = new List<UserAccount>
{
    new UserAccount(
        name: "adminUser",
        password: "xyz123",
        elevationLevel: ElevationLevel.Admin,
        linuxUserConfiguration: new LinuxUserConfiguration(
            uid: 12345,
            gid: 98765,
            sshPrivateKey: new Guid().ToString()
            )),
    new UserAccount(
        name: "nonAdminUser",
        password: "123xyz",
        elevationLevel: ElevationLevel.NonAdmin,
        linuxUserConfiguration: new LinuxUserConfiguration(
            uid: 45678,
            gid: 98765,
            sshPrivateKey: new Guid().ToString()
            )),
};

// Commit hello pool.
await pool.CommitAsync();
```


#### <a name="batch-java-example"></a><span data-ttu-id="c7540-217">Kötegelt Java – példa</span><span class="sxs-lookup"><span data-stu-id="c7540-217">Batch Java example</span></span>

```java
List<UserAccount> userList = new ArrayList<>();
userList.add(new UserAccount().withName(adminUserAccountName).withPassword(adminPassword).withElevationLevel(ElevationLevel.ADMIN));
userList.add(new UserAccount().withName(nonAdminUserAccountName).withPassword(nonAdminPassword).withElevationLevel(ElevationLevel.NONADMIN));
PoolAddParameter addParameter = new PoolAddParameter()
        .withId(poolId)
        .withTargetDedicatedNodes(POOL_VM_COUNT)
        .withVmSize(POOL_VM_SIZE)
        .withCloudServiceConfiguration(configuration)
        .withUserAccounts(userList);
batchClient.poolOperations().createPool(addParameter);
```

#### <a name="batch-python-example"></a><span data-ttu-id="c7540-218">Kötegelt Python – példa</span><span class="sxs-lookup"><span data-stu-id="c7540-218">Batch Python example</span></span>

```python
users = [
    batchmodels.UserAccount(
        name='pool-admin',
        password='******',
        elevation_level=batchmodels.ElevationLevel.admin)
    batchmodels.UserAccount(
        name='pool-nonadmin',
        password='******',
        elevation_level=batchmodels.ElevationLevel.nonadmin)
]
pool = batchmodels.PoolAddParameter(
    id=pool_id,
    user_accounts=users,
    virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
        image_reference=image_ref_to_use,
        node_agent_sku_id=sku_to_use),
    vm_size=vm_size,
    target_dedicated=vm_count)
batch_client.pool.add(pool)
```

### <a name="run-a-task-under-a-named-user-account-with-elevated-access"></a><span data-ttu-id="c7540-219">Emelt szintű hozzáférés egy névvel ellátott felhasználói fiókhoz tartozó feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="c7540-219">Run a task under a named user account with elevated access</span></span>

<span data-ttu-id="c7540-220">egy tevékenység egy emelt szintű felhasználó, set hello feladat toorun **UserIdentity** tulajdonság tooa nevű felhasználói fiók hoztak létre a **ElevationLevel** tulajdonsága túl`Admin`.</span><span class="sxs-lookup"><span data-stu-id="c7540-220">toorun a task as an elevated user, set hello task's **UserIdentity** property tooa named user account that was created with its **ElevationLevel** property set too`Admin`.</span></span>

<span data-ttu-id="c7540-221">A kódrészletet megadja hello tevékenység egy elnevezett felhasználói fiókkal kell futtatni.</span><span class="sxs-lookup"><span data-stu-id="c7540-221">This code snippet specifies that hello task should run under a named user account.</span></span> <span data-ttu-id="c7540-222">Ez a nevesített felhasználói fiók hello készletében van definiálva, hello készlet létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="c7540-222">This named user account was defined on hello pool when hello pool was created.</span></span> <span data-ttu-id="c7540-223">Ebben az esetben hello nevű felhasználói fiók rendszergazdai jogosultságokkal rendelkező jött létre:</span><span class="sxs-lookup"><span data-stu-id="c7540-223">In this case, hello named user account was created with admin permissions:</span></span>

```csharp
CloudTask task = new CloudTask("1", "cmd.exe /c echo 1");
task.UserIdentity = new UserIdentity(AdminUserAccountName);
```

## <a name="update-your-code-toohello-latest-batch-client-library"></a><span data-ttu-id="c7540-224">Frissítse a kódot toohello legújabb kötegelt ügyféloldali kódtár</span><span class="sxs-lookup"><span data-stu-id="c7540-224">Update your code toohello latest Batch client library</span></span>

<span data-ttu-id="c7540-225">2017-01-01.4.0 bevezeti használhatatlanná tévő változást, cseréje hello hello kötegelt verziójú **runElevated** korábbi verzióival rendelkező hello tulajdonság **userIdentity** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="c7540-225">hello Batch service version 2017-01-01.4.0 introduces a breaking change, replacing hello **runElevated** property available in earlier versions with hello **userIdentity** property.</span></span> <span data-ttu-id="c7540-226">a következő táblák hello elérhető egy egyszerű leképezési, hogy így tooupdate hello klienskódtárak korábbi verzióiban a kódot.</span><span class="sxs-lookup"><span data-stu-id="c7540-226">hello following tables provide a simple mapping that you can use tooupdate your code from earlier versions of hello client libraries.</span></span>

### <a name="batch-net"></a><span data-ttu-id="c7540-227">Batch .NET</span><span class="sxs-lookup"><span data-stu-id="c7540-227">Batch .NET</span></span>

| <span data-ttu-id="c7540-228">Ha a kódot használja...</span><span class="sxs-lookup"><span data-stu-id="c7540-228">If your code uses...</span></span>                  | <span data-ttu-id="c7540-229">Frissítse úgy, hogy...</span><span class="sxs-lookup"><span data-stu-id="c7540-229">Update it to....</span></span>                                                                                                 |
|---------------------------------------|------------------------------------------------------------------------------------------------------------------|
| `CloudTask.RunElevated = true;`       | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin));`    |
| `CloudTask.RunElevated = false;`      | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.NonAdmin));` |
| <span data-ttu-id="c7540-230">`CloudTask.RunElevated`Nincs megadva</span><span class="sxs-lookup"><span data-stu-id="c7540-230">`CloudTask.RunElevated` not specified</span></span> | <span data-ttu-id="c7540-231">Nincs frissítés szükséges</span><span class="sxs-lookup"><span data-stu-id="c7540-231">No update required</span></span>                                                                                               |

### <a name="batch-java"></a><span data-ttu-id="c7540-232">Kötegelt Java</span><span class="sxs-lookup"><span data-stu-id="c7540-232">Batch Java</span></span>

| <span data-ttu-id="c7540-233">Ha a kódot használja...</span><span class="sxs-lookup"><span data-stu-id="c7540-233">If your code uses...</span></span>                      | <span data-ttu-id="c7540-234">Frissítse úgy, hogy...</span><span class="sxs-lookup"><span data-stu-id="c7540-234">Update it to....</span></span>                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `CloudTask.withRunElevated(true);`        | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.ADMIN));`    |
| `CloudTask.withRunElevated(false);`       | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.NONADMIN));` |
| <span data-ttu-id="c7540-235">`CloudTask.withRunElevated`Nincs megadva</span><span class="sxs-lookup"><span data-stu-id="c7540-235">`CloudTask.withRunElevated` not specified</span></span> | <span data-ttu-id="c7540-236">Nincs frissítés szükséges</span><span class="sxs-lookup"><span data-stu-id="c7540-236">No update required</span></span>                                                                                                                     |

### <a name="batch-python"></a><span data-ttu-id="c7540-237">Batch Python</span><span class="sxs-lookup"><span data-stu-id="c7540-237">Batch Python</span></span>

| <span data-ttu-id="c7540-238">Ha a kódot használja...</span><span class="sxs-lookup"><span data-stu-id="c7540-238">If your code uses...</span></span>                      | <span data-ttu-id="c7540-239">Frissítse úgy, hogy...</span><span class="sxs-lookup"><span data-stu-id="c7540-239">Update it to....</span></span>                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `run_elevated=True`                       | <span data-ttu-id="c7540-240">`user_identity=user`, ahol</span><span class="sxs-lookup"><span data-stu-id="c7540-240">`user_identity=user`, where</span></span> <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.admin)) `                |
| `run_elevated=False`                      | <span data-ttu-id="c7540-241">`user_identity=user`, ahol</span><span class="sxs-lookup"><span data-stu-id="c7540-241">`user_identity=user`, where</span></span> <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.nonadmin)) `             |
| <span data-ttu-id="c7540-242">`run_elevated`Nincs megadva</span><span class="sxs-lookup"><span data-stu-id="c7540-242">`run_elevated` not specified</span></span> | <span data-ttu-id="c7540-243">Nincs frissítés szükséges</span><span class="sxs-lookup"><span data-stu-id="c7540-243">No update required</span></span>                                                                                                                                  |


## <a name="next-steps"></a><span data-ttu-id="c7540-244">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c7540-244">Next steps</span></span>

### <a name="batch-forum"></a><span data-ttu-id="c7540-245">Batch fórum</span><span class="sxs-lookup"><span data-stu-id="c7540-245">Batch Forum</span></span>

<span data-ttu-id="c7540-246">Hello [Azure Batch fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch) MSDN nagyszerű toodiscuss kötegelt helyezze, és hello szolgáltatás kérdése.</span><span class="sxs-lookup"><span data-stu-id="c7540-246">hello [Azure Batch Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch) on MSDN is a great place toodiscuss Batch and ask questions about hello service.</span></span> <span data-ttu-id="c7540-247">Központi a keresztül a hasznos rögzített bejegyzések, és a kötegelt megoldások létrehozása során felmerülő kérdéseit.</span><span class="sxs-lookup"><span data-stu-id="c7540-247">Head on over for helpful pinned posts, and post your questions as they arise while you build your Batch solutions.</span></span>
