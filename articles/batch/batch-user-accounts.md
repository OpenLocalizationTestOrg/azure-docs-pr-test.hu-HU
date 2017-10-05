---
title: "A felhasználói fiókok feladatok futtatása az Azure Batch |} Microsoft Docs"
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
ms.openlocfilehash: d408c0565c0ed81fc97cc2b3976a4fc233e31302
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="run-tasks-under-user-accounts-in-batch"></a><span data-ttu-id="2578c-103">Kötegelt a felhasználói fiókok feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="2578c-103">Run tasks under user accounts in Batch</span></span>

<span data-ttu-id="2578c-104">Mindig fusson a feladat, az Azure Batch egy felhasználói fiókkal.</span><span class="sxs-lookup"><span data-stu-id="2578c-104">A task in Azure Batch always runs under a user account.</span></span> <span data-ttu-id="2578c-105">Alapértelmezés szerint a feladatok futtathatók általános jogú felhasználói fiókokat, a rendszergazdai engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="2578c-105">By default, tasks run under standard user accounts, without administrator permissions.</span></span> <span data-ttu-id="2578c-106">Ezek az alapértelmezett felhasználói fiók beállítások általában elegendők.</span><span class="sxs-lookup"><span data-stu-id="2578c-106">These default user account settings are typically sufficient.</span></span> <span data-ttu-id="2578c-107">Bizonyos esetekben azonban célszerű tudják a felhasználói fiók, amely alatt a feladat futtatása szeretne konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="2578c-107">For certain scenarios, however, it's useful to be able to configure the user account under which you want a task to run.</span></span> <span data-ttu-id="2578c-108">A cikkből megtudhatja, milyen felhasználói fiókok, és hogyan konfigurálhatók a forgatókönyvnek.</span><span class="sxs-lookup"><span data-stu-id="2578c-108">This article discusses the types of user accounts and how you can configure them for your scenario.</span></span>

## <a name="types-of-user-accounts"></a><span data-ttu-id="2578c-109">Felhasználói fiókok típusai</span><span class="sxs-lookup"><span data-stu-id="2578c-109">Types of user accounts</span></span>

<span data-ttu-id="2578c-110">Az Azure Batch kétféle típusú felhasználói fiókokat biztosít az éppen futó feladatok:</span><span class="sxs-lookup"><span data-stu-id="2578c-110">Azure Batch provides two types of user accounts for running tasks:</span></span>

- <span data-ttu-id="2578c-111">**Automatikus-felhasználói fiókokat.**</span><span class="sxs-lookup"><span data-stu-id="2578c-111">**Auto-user accounts.**</span></span> <span data-ttu-id="2578c-112">Automatikus-felhasználói fiókok olyan beépített felhasználói fiókok, a Batch szolgáltatás automatikusan létrehozza.</span><span class="sxs-lookup"><span data-stu-id="2578c-112">Auto-user accounts are built-in user accounts that are created automatically by the Batch service.</span></span> <span data-ttu-id="2578c-113">Alapértelmezés szerint feladatok automatikus felhasználói fiókkal futtassa.</span><span class="sxs-lookup"><span data-stu-id="2578c-113">By default, tasks run under an auto-user account.</span></span> <span data-ttu-id="2578c-114">Konfigurálhatja az automatikus felhasználói specifikáció annak jelzésére, hogy mely automatikus-felhasználói fiók alatt egy feladat fusson a feladat.</span><span class="sxs-lookup"><span data-stu-id="2578c-114">You can configure the auto-user specification for a task to indicate under which auto-user account a task should run.</span></span> <span data-ttu-id="2578c-115">A felhasználói automatikus megadását lehetővé teszi, hogy a jogosultságszint-emelés szint és a auto-felhasználói fiók, amely a feladat hatókör megadását.</span><span class="sxs-lookup"><span data-stu-id="2578c-115">The auto-user specification allows you to specify the elevation level and scope of the auto-user account that will run the task.</span></span> 

- <span data-ttu-id="2578c-116">**Egy névvel ellátott felhasználói fiókot.**</span><span class="sxs-lookup"><span data-stu-id="2578c-116">**A named user account.**</span></span> <span data-ttu-id="2578c-117">Megadhatja a készlet egy vagy több elnevezett felhasználói fiókot, a készlet létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="2578c-117">You can specify one or more named user accounts for a pool when you create the pool.</span></span> <span data-ttu-id="2578c-118">Minden hozzáadott felhasználói fiókot a készlet minden egyes csomóponton jön létre.</span><span class="sxs-lookup"><span data-stu-id="2578c-118">Each user account is created on each node of the pool.</span></span> <span data-ttu-id="2578c-119">A fiók neve mellett, adja meg a felhasználói fiók jelszavát, jogosultságszint-emelési szinten, valamint a Linux-készletek, a titkos SSH-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="2578c-119">In addition to the account name, you specify the user account password, elevation level, and, for Linux pools, the SSH private key.</span></span> <span data-ttu-id="2578c-120">Ha hozzáad egy feladatot, megadhatja a névvel ellátott felhasználói fiókot, amely alatt ez a feladat futni.</span><span class="sxs-lookup"><span data-stu-id="2578c-120">When you add a task, you can specify the named user account under which that task should run.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="2578c-121">A Batch szolgáltatás 2017-01-01.4.0 tartalmazza az használhatatlanná tévő változást, amely megköveteli, hogy a kód hívására, hogy verziót frissíti.</span><span class="sxs-lookup"><span data-stu-id="2578c-121">The Batch service version 2017-01-01.4.0 introduces a breaking change that requires that you update your code to call that version.</span></span> <span data-ttu-id="2578c-122">Ha áttelepítése kód köteg egy korábbi verziójából származó, vegye figyelembe, hogy a **runElevated** tulajdonság már nem támogatott a REST API vagy kötegelt klienskódtárak segítségével.</span><span class="sxs-lookup"><span data-stu-id="2578c-122">If you are migrating code from an older version of Batch, note that the **runElevated** property is no longer supported in the REST API or Batch client libraries.</span></span> <span data-ttu-id="2578c-123">Az új **userIdentity** tulajdonság a feladatok jogosultságszint-emelés szintjének beállításához.</span><span class="sxs-lookup"><span data-stu-id="2578c-123">Use the new **userIdentity** property of a task to specify elevation level.</span></span> <span data-ttu-id="2578c-124">Kifejezéseit leíró szakasza [frissítse a kódot a legújabb kötegelt ügyféloldali kódtár](#update-your-code-to-the-latest-batch-client-library) kapcsolatos frissítése a kötegelt kód használata a klienskódtárak segítségével egyik gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="2578c-124">See the section titled [Update your code to the latest Batch client library](#update-your-code-to-the-latest-batch-client-library) for quick guidelines for updating your Batch code if you are using one of the client libraries.</span></span>
>
>

> [!NOTE] 
> <span data-ttu-id="2578c-125">A cikkben szereplő felhasználói fiókok nem támogatják az Remote Desktop Protocol (RDP) vagy a Secure Shell (SSH), a biztonsági okok miatt.</span><span class="sxs-lookup"><span data-stu-id="2578c-125">The user accounts discussed in this article do not support Remote Desktop Protocol (RDP) or Secure Shell (SSH), for security reasons.</span></span> 
>
> <span data-ttu-id="2578c-126">A csomópont futó Linux virtuálisgép-konfiguráció SSH-kapcsolaton keresztül csatlakoznak, lásd: [távoli asztal használata a Linux virtuális gépre az Azure-ban](../virtual-machines/virtual-machines-linux-use-remote-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="2578c-126">To connect to a node running the Linux virtual machine configuration via SSH, see [Use Remote Desktop to a Linux VM in Azure](../virtual-machines/virtual-machines-linux-use-remote-desktop.md).</span></span> <span data-ttu-id="2578c-127">Csomópontok Windows rendszerű RDP-kapcsolaton keresztül csatlakoznak, lásd: [csatlakozás egy Windows Server virtuális gép](../virtual-machines/windows/connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="2578c-127">To connect to nodes running Windows via RDP, see [Connect to a Windows Server VM](../virtual-machines/windows/connect-logon.md).</span></span><br /><br />
> <span data-ttu-id="2578c-128">Egy csomópontot, a felhőalapú szolgáltatás konfigurációja fut RDP-kapcsolaton keresztül csatlakoznak, lásd: [engedélyezése a távoli asztali kapcsolat az Azure Cloud Services szerepkör](../cloud-services/cloud-services-role-enable-remote-desktop-new-portal.md).</span><span class="sxs-lookup"><span data-stu-id="2578c-128">To connect to a node running the cloud service configuration via RDP, see [Enable Remote Desktop Connection for a Role in Azure Cloud Services](../cloud-services/cloud-services-role-enable-remote-desktop-new-portal.md).</span></span>
>
>

## <a name="user-account-access-to-files-and-directories"></a><span data-ttu-id="2578c-129">Fájlok és könyvtárak felhasználóifiók-hozzáférés</span><span class="sxs-lookup"><span data-stu-id="2578c-129">User account access to files and directories</span></span>

<span data-ttu-id="2578c-130">Automatikus-felhasználói fiókkal és a nevű felhasználói fiók rendelkezik olvasási/írási hozzáférést a feladatütemezési munkakönyvtár, megosztott és többpéldányos feladatok könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="2578c-130">Both an auto-user account and a named user account have read/write access to the task’s working directory, shared directory, and multi-instance tasks directory.</span></span> <span data-ttu-id="2578c-131">Mindkét típusú fiókok a indítási és a feladat előkészítése könyvtárakhoz olvasási hozzáféréssel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2578c-131">Both types of accounts have read access to the startup and job preparation directories.</span></span>

<span data-ttu-id="2578c-132">Ha a feladat kezdési feladat futtatásához használt ugyanazon fiók alatt fut, a feladat rendelkezik olvasási és írási hozzáférése a kezdő tevékenység könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="2578c-132">If a task runs under the same account that was used for running a start task, the task has read-write access to the start task directory.</span></span> <span data-ttu-id="2578c-133">Hasonlóképpen ha egy feladat a feladat előkészítése tevékenység futtatásához használt ugyanazon fiók alatt fut, a feladat olvasási és írási hozzáférése a feladat előkészítése tevékenység könyvtár rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2578c-133">Similarly, if a task runs under the same account that was used for running a job preparation task, the task has read-write access to the job preparation task directory.</span></span> <span data-ttu-id="2578c-134">Ha egy feladat fut, mint a kezdő tevékenység vagy a feladat előkészítése tevékenységet egy másik fiókból, a feladat a megfelelő könyvtár csak olvasási hozzáféréssel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2578c-134">If a task runs under a different account than the start task or job preparation task, then the task has only read access to the respective directory.</span></span>

<span data-ttu-id="2578c-135">A fájlok és könyvtárak feladat eléréséhez további információkért lásd: [Develop nagyméretű párhuzamos számítási solutions a kötegelt](batch-api-basics.md#files-and-directories).</span><span class="sxs-lookup"><span data-stu-id="2578c-135">For more information on accessing files and directories from a task, see [Develop large-scale parallel compute solutions with Batch](batch-api-basics.md#files-and-directories).</span></span>

## <a name="elevated-access-for-tasks"></a><span data-ttu-id="2578c-136">Emelt szintű hozzáférés feladatokhoz</span><span class="sxs-lookup"><span data-stu-id="2578c-136">Elevated access for tasks</span></span> 

<span data-ttu-id="2578c-137">A felhasználói fiók jogosultságszint-emelés szintjét jelzi, hogy fut-e a feladat emelt szintű hozzáféréssel rendelkező.</span><span class="sxs-lookup"><span data-stu-id="2578c-137">The user account's elevation level indicates whether a task runs with elevated access.</span></span> <span data-ttu-id="2578c-138">Automatikus-felhasználói fiókkal és a egy névvel ellátott felhasználói fiókot is futtassa emelt szintű hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="2578c-138">Both an auto-user account and a named user account can run with elevated access.</span></span> <span data-ttu-id="2578c-139">Jogosultságszint-emelés szint két lehetőségek állnak rendelkezésére:</span><span class="sxs-lookup"><span data-stu-id="2578c-139">The two options for elevation level are:</span></span>

- <span data-ttu-id="2578c-140">**NonAdmin:** a feladat fut, az általános jogú felhasználó emelt szintű hozzáférés nélkül.</span><span class="sxs-lookup"><span data-stu-id="2578c-140">**NonAdmin:** The task runs as a standard user without elevated access.</span></span> <span data-ttu-id="2578c-141">Az alapértelmezett jogosultságszint-emelés kötegelt felhasználói fiók szintje mindig **NonAdmin**.</span><span class="sxs-lookup"><span data-stu-id="2578c-141">The default elevation level for a Batch user account is always **NonAdmin**.</span></span>
- <span data-ttu-id="2578c-142">**Felügyeleti:** a feladat emelt szintű hozzáféréssel rendelkező felhasználóként fut, és teljes körű felügyeleti engedélyekkel működik.</span><span class="sxs-lookup"><span data-stu-id="2578c-142">**Admin:** The task runs as a user with elevated access and operates with full Administrator permissions.</span></span> 

## <a name="auto-user-accounts"></a><span data-ttu-id="2578c-143">Automatikus-felhasználói fiókok</span><span class="sxs-lookup"><span data-stu-id="2578c-143">Auto-user accounts</span></span>

<span data-ttu-id="2578c-144">Alapértelmezés szerint feladatokat futtató kötegben automatikus felhasználói fiókkal, az általános jogú felhasználó emelt szintű hozzáférés nélkül, és a feladat hatókörében.</span><span class="sxs-lookup"><span data-stu-id="2578c-144">By default, tasks run in Batch under an auto-user account, as a standard user without elevated access, and with task scope.</span></span> <span data-ttu-id="2578c-145">Amikor az auto-felhasználó specifikáció tevékenység hatókörében van konfigurálva, a Batch szolgáltatás ezt a feladatot csak egy automatikus-felhasználói fiókot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2578c-145">When the auto-user specification is configured for task scope, the Batch service creates an auto-user account for that task only.</span></span>

<span data-ttu-id="2578c-146">A feladat hatókörébe esetben készlet hatókör.</span><span class="sxs-lookup"><span data-stu-id="2578c-146">The alternative to task scope is pool scope.</span></span> <span data-ttu-id="2578c-147">Ha egy feladat automatikus felhasználói előírásának készlet hatókör van konfigurálva, a feladat érhető el a készletben található összes feladatütemezési automatikus felhasználói fiókkal fut.</span><span class="sxs-lookup"><span data-stu-id="2578c-147">When the auto-user specification for a task is configured for pool scope, the task runs under an auto-user account that is available to any task in the pool.</span></span> <span data-ttu-id="2578c-148">Készlet hatókör kapcsolatos további információkért lásd: című részre [feladat felhasználóként futtatja az automatikus-készlet hatókörű](#run-a-task-as-the-autouser-with-pool-scope).</span><span class="sxs-lookup"><span data-stu-id="2578c-148">For more information about pool scope, see the section titled [Run a task as the auto-user with pool scope](#run-a-task-as-the-autouser-with-pool-scope).</span></span>   

<span data-ttu-id="2578c-149">Az alapértelmezett hatókör nem azonos a Windows és Linux-csomópontok:</span><span class="sxs-lookup"><span data-stu-id="2578c-149">The default scope is different on Windows and Linux nodes:</span></span>

- <span data-ttu-id="2578c-150">Windows csomópontján feladatok futtathatók feladat hatókör alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="2578c-150">On Windows nodes, tasks run under task scope by default.</span></span>
- <span data-ttu-id="2578c-151">Linux-csomópontok a készlet hatókör mindig futnia.</span><span class="sxs-lookup"><span data-stu-id="2578c-151">Linux nodes always run under pool scope.</span></span>

<span data-ttu-id="2578c-152">Az automatikus felhasználói megadását, amelyek mindegyike megfelel egy egyedi automatikus-felhasználói fiókhoz négy lehetséges konfiguráció van:</span><span class="sxs-lookup"><span data-stu-id="2578c-152">There are four possible configurations for the auto-user specification, each of which corresponds to a unique auto-user account:</span></span>

- <span data-ttu-id="2578c-153">A nem rendszergazda hozzáférést feladat hatókörrel (az alapértelmezett automatikus felhasználói specifikáció)</span><span class="sxs-lookup"><span data-stu-id="2578c-153">Non-admin access with task scope (the default auto-user specification)</span></span>
- <span data-ttu-id="2578c-154">A feladat hatókörű (emelt szintű) rendszergazdai hozzáférés</span><span class="sxs-lookup"><span data-stu-id="2578c-154">Admin (elevated) access with task scope</span></span>
- <span data-ttu-id="2578c-155">A nem rendszergazda hozzáférést készlet hatókörű</span><span class="sxs-lookup"><span data-stu-id="2578c-155">Non-admin access with pool scope</span></span>
- <span data-ttu-id="2578c-156">Rendszergazdai hozzáférés készlet hatókörű</span><span class="sxs-lookup"><span data-stu-id="2578c-156">Admin access with pool scope</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="2578c-157">Feladat hatókör alatt futó feladatok nincs hozzáférése tényleges más feladatok csomóponton.</span><span class="sxs-lookup"><span data-stu-id="2578c-157">Tasks running under task scope do not have de facto access to other tasks on a node.</span></span> <span data-ttu-id="2578c-158">Azonban egy rosszindulatú felhasználó hozzáfér a fiókjához sikerült megoldható, ez a korlátozás, amely rendszergazdai jogosultságokkal futtatja, és más tevékenység könyvtárak fér hozzá a feladat elküldése.</span><span class="sxs-lookup"><span data-stu-id="2578c-158">However, a malicious user with access to the account could work around this restriction by submitting a task that runs with administrator privileges and accesses other task directories.</span></span> <span data-ttu-id="2578c-159">Egy rosszindulatú felhasználó is használhatja az RDP és az SSH egy csomópont való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="2578c-159">A malicious user could also use RDP or SSH to connect to a node.</span></span> <span data-ttu-id="2578c-160">Fontos, hogy megakadályozza az ilyen esetben a kötegelt kulcsait való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="2578c-160">It's important to protect access to your Batch account keys to prevent such a scenario.</span></span> <span data-ttu-id="2578c-161">Ha azt gyanítja, hogy a fiók sérült, ügyeljen arra, hogy a kulcsok újragenerálása.</span><span class="sxs-lookup"><span data-stu-id="2578c-161">If you suspect your account may have been compromised, be sure to regenerate your keys.</span></span>
>
>

### <a name="run-a-task-as-an-auto-user-with-elevated-access"></a><span data-ttu-id="2578c-162">Emelt szintű hozzáféréssel rendelkező automatikus-felhasználóként feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="2578c-162">Run a task as an auto-user with elevated access</span></span>

<span data-ttu-id="2578c-163">Rendszergazdai jogosultságokkal automatikus felhasználói előírásának konfigurálhatja, ha a feladat futtatásához emelt szintű hozzáféréssel rendelkező szükséges.</span><span class="sxs-lookup"><span data-stu-id="2578c-163">You can configure the auto-user specification for administrator privileges when you need to run a task with elevated access.</span></span> <span data-ttu-id="2578c-164">A kezdő tevékenység Előfordulhat például, emelt szintű hozzáférés szoftver telepítéséhez a csomóponton.</span><span class="sxs-lookup"><span data-stu-id="2578c-164">For example, a start task may need elevated access to install software on the node.</span></span>

> [!NOTE] 
> <span data-ttu-id="2578c-165">Általában célszerű emelt szintű hozzáférés csak szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="2578c-165">In general, it's best to use elevated access only when necessary.</span></span> <span data-ttu-id="2578c-166">A bevált gyakorlat része a kívánt eredmény eléréséhez szükséges minimális jogosultság megadása.</span><span class="sxs-lookup"><span data-stu-id="2578c-166">Best practices recommend granting the minimum privilege necessary to achieve the desired outcome.</span></span> <span data-ttu-id="2578c-167">Például ha egy kezdő tevékenység szoftvereket telepít az aktuális felhasználó ahelyett, hogy a felhasználók esetleg elkerülheti a feladatok emelt szintű hozzáférés biztosítása.</span><span class="sxs-lookup"><span data-stu-id="2578c-167">For example, if a start task installs software for the current user, instead of for all users, you may be able to avoid granting elevated access to tasks.</span></span> <span data-ttu-id="2578c-168">Beállíthatja, hogy a készlet hatókör és a nem rendszergazdai hozzáférést, amely ugyanazt a fiókot, beleértve a kezdő tevékenység futnia kell az összes tevékenység automatikus felhasználói megadását.</span><span class="sxs-lookup"><span data-stu-id="2578c-168">You can configure the auto-user specification for pool scope and non-admin access for all tasks that need to run under the same account, including the start task.</span></span> 
>
>

<span data-ttu-id="2578c-169">Az alábbi kódtöredékek bemutatják, hogyan konfigurálása a felhasználói automatikus megadását.</span><span class="sxs-lookup"><span data-stu-id="2578c-169">The following code snippets show how to configure the auto-user specification.</span></span> <span data-ttu-id="2578c-170">A példák állítsa a jogosultságszint-emelés szintet `Admin` és a hatókör `Task`.</span><span class="sxs-lookup"><span data-stu-id="2578c-170">The examples set the elevation level to `Admin` and the scope to `Task`.</span></span> <span data-ttu-id="2578c-171">Feladat hatókörében az alapértelmezett beállítás, de megtalálható itt az példa.</span><span class="sxs-lookup"><span data-stu-id="2578c-171">Task scope is the default setting, but is included here for the sake of example.</span></span>

#### <a name="batch-net"></a><span data-ttu-id="2578c-172">Batch .NET</span><span class="sxs-lookup"><span data-stu-id="2578c-172">Batch .NET</span></span>

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin, scope: AutoUserScope.Task));
```
#### <a name="batch-java"></a><span data-ttu-id="2578c-173">Kötegelt Java</span><span class="sxs-lookup"><span data-stu-id="2578c-173">Batch Java</span></span>

```java
taskToAdd.withId(taskId)
        .withUserIdentity(new UserIdentity()
            .withAutoUser(new AutoUserSpecification()
                .withElevationLevel(ElevationLevel.ADMIN))
                .withScope(AutoUserScope.TASK));
        .withCommandLine("cmd /c echo hello");                        
```

#### <a name="batch-python"></a><span data-ttu-id="2578c-174">Batch Python</span><span class="sxs-lookup"><span data-stu-id="2578c-174">Batch Python</span></span>

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

### <a name="run-a-task-as-an-auto-user-with-pool-scope"></a><span data-ttu-id="2578c-175">Készlet hatókörű automatikus-felhasználóként feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="2578c-175">Run a task as an auto-user with pool scope</span></span>

<span data-ttu-id="2578c-176">Ha egy csomópont ki van építve, két készlet kiterjedő automatikus-felhasználói fiókok létrejöttek mindegyik csomópontján a készletben, egy emelt szintű hozzáféréssel rendelkező és egy emelt szintű hozzáférés nélkül.</span><span class="sxs-lookup"><span data-stu-id="2578c-176">When a node is provisioned, two pool-wide auto-user accounts are created on each node in the pool, one with elevated access, and one without elevated access.</span></span> <span data-ttu-id="2578c-177">Az automatikus felhasználó hatókör beállítást egy adott tevékenység készlet hatókör fut a feladat során két készlet kiterjedő automatikus-felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="2578c-177">Setting the auto-user's scope to pool scope for a given task runs the task under one of these two pool-wide auto-user accounts.</span></span> 

<span data-ttu-id="2578c-178">Ha a készlet hatókör a auto-felhasználó, minden olyan feladat, amely ugyanazt a készlet kiterjedő automatikus-felhasználói fiókot futtatásához rendszergazdai jogosultságokkal futtassa.</span><span class="sxs-lookup"><span data-stu-id="2578c-178">When you specify pool scope for the auto-user, all tasks that run with administrator access run under the same pool-wide auto-user account.</span></span> <span data-ttu-id="2578c-179">Ehhez hasonlóan rendszergazdai jogosultságok nélküli futtatott feladatok is futtathatók egy alkalmazáskészlet kiterjedő automatikus felhasználói fiókkal.</span><span class="sxs-lookup"><span data-stu-id="2578c-179">Similarly, tasks that run without administrator permissions also run under a single pool-wide auto-user account.</span></span> 

> [!NOTE] 
> <span data-ttu-id="2578c-180">A két készlet kiterjedő automatikus-felhasználói fiókok külön fiók is.</span><span class="sxs-lookup"><span data-stu-id="2578c-180">The two pool-wide auto-user accounts are separate accounts.</span></span> <span data-ttu-id="2578c-181">A készlet szintű rendszergazdai fiók alatt futó feladatok nem adatok megosztása a szabványos fiókkal, és ez fordítva is igaz futó feladatok.</span><span class="sxs-lookup"><span data-stu-id="2578c-181">Tasks running under the pool-wide administrative account cannot share data with tasks running under the standard account, and vice versa.</span></span> 
>
>

<span data-ttu-id="2578c-182">Azonos automatikus felhasználói fiók alatt fut előnye, hogy a feladatok képesek adatok megosztása más ugyanazon a csomóponton futó feladatok.</span><span class="sxs-lookup"><span data-stu-id="2578c-182">The advantage to running under the same auto-user account is that tasks are able to share data with other tasks running on the same node.</span></span>

<span data-ttu-id="2578c-183">Egy forgatókönyvet, ahol az éppen futó feladatok a két készlet kiterjedő automatikus-felhasználói fiókok valamelyike akkor hasznos, titkos kulcsok tevékenységek közötti megosztása szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="2578c-183">Sharing secrets between tasks is one scenario where running tasks under one of the two pool-wide auto-user accounts is useful.</span></span> <span data-ttu-id="2578c-184">Tegyük fel, hogy a kezdő tevékenységre kell egy titkos kulcsot, a csomópont, amellyel más feladatok kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="2578c-184">For example, suppose a start task needs to provision a secret onto the node that other tasks can use.</span></span> <span data-ttu-id="2578c-185">Használhatja a Windows Data Protection API (DPAPI), de a rendszergazdai jogosultságokat igényel.</span><span class="sxs-lookup"><span data-stu-id="2578c-185">You could use the Windows Data Protection API (DPAPI), but it requires administrator privileges.</span></span> <span data-ttu-id="2578c-186">Ehelyett a titkos kulcsot a felhasználói szintű védelmet biztosíthat.</span><span class="sxs-lookup"><span data-stu-id="2578c-186">Instead, you can protect the secret at the user level.</span></span> <span data-ttu-id="2578c-187">Az azonos felhasználói fiók alatt futó feladatok férhetnek hozzá a titkos kulcsot, emelt szintű hozzáférés nélkül.</span><span class="sxs-lookup"><span data-stu-id="2578c-187">Tasks running under the same user account can access the secret without elevated access.</span></span>

<span data-ttu-id="2578c-188">Ahol lehetséges, hogy futtatni kívánt feladatokat egy automatikus-felhasználói fiókhoz tartozó készlet hatókörű egy másik helyzet lehet a Message Passing Interface (MPI) fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="2578c-188">Another scenario where you may want to run tasks under an auto-user account with pool scope is a Message Passing Interface (MPI) file share.</span></span> <span data-ttu-id="2578c-189">Egy MPI fájlmegosztás akkor hasznos, ha a fájl ugyanazokat az adatokat a csomópontok a MPI feladat kell használnia.</span><span class="sxs-lookup"><span data-stu-id="2578c-189">An MPI file share is useful when the nodes in the MPI task need to work on the same file data.</span></span> <span data-ttu-id="2578c-190">Az átjárócsomópont létrehoz egy fájlmegosztást, amely a gyermekcsomópontok hozzáférhet, ha azonos automatikus felhasználói fiók alatt futnak.</span><span class="sxs-lookup"><span data-stu-id="2578c-190">The head node creates a file share that the child nodes can access if they are running under the same auto-user account.</span></span> 

<span data-ttu-id="2578c-191">A következő kódrészletet az automatikus felhasználó hatókör készlet hatókör meg olyan feladatra, a Batch .NET állítja be.</span><span class="sxs-lookup"><span data-stu-id="2578c-191">The following code snippet sets the auto-user's scope to pool scope for a task in Batch .NET.</span></span> <span data-ttu-id="2578c-192">A jogosultságszint-emelés szint nincs megadva, ezért a szabványos készlet kiterjedő automatikus-felhasználói fiók alatt fut a feladat.</span><span class="sxs-lookup"><span data-stu-id="2578c-192">The elevation level is omitted, so the task runs under the standard pool-wide auto-user account.</span></span>

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(scope: AutoUserScope.Pool));
```

## <a name="named-user-accounts"></a><span data-ttu-id="2578c-193">Elnevezett felhasználói fiókok</span><span class="sxs-lookup"><span data-stu-id="2578c-193">Named user accounts</span></span>

<span data-ttu-id="2578c-194">Itt megadhatja, hogy a program elnevezett felhasználói fiókokat, amikor a készletet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2578c-194">You can define named user accounts when you create a pool.</span></span> <span data-ttu-id="2578c-195">Egy névvel ellátott felhasználói fiók rendelkezik, egy nevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="2578c-195">A named user account has a name and password that you provide.</span></span> <span data-ttu-id="2578c-196">Megadhatja, hogy a jogosultságszint-emelés szintje egy névvel ellátott felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="2578c-196">You can specify the elevation level for a named user account.</span></span> <span data-ttu-id="2578c-197">Linux-csomópontok titkos SSH-kulcsot is megadhatja.</span><span class="sxs-lookup"><span data-stu-id="2578c-197">For Linux nodes, you can also provide an SSH private key.</span></span>

<span data-ttu-id="2578c-198">Elnevezett felhasználói fiók létezik a készletben található összes csomóponton, és készen áll a azokat a csomópontokat futó összes feladatot.</span><span class="sxs-lookup"><span data-stu-id="2578c-198">A named user account exists on all nodes in the pool and is available to all tasks running on those nodes.</span></span> <span data-ttu-id="2578c-199">Nevesített felhasználók készlet tetszőleges számú adhatók meg.</span><span class="sxs-lookup"><span data-stu-id="2578c-199">You may define any number of named users for a pool.</span></span> <span data-ttu-id="2578c-200">Egy feladat vagy tevékenység gyűjtemény hozzáadásakor megadhatja, hogy a feladat fut. a készlet a megadott elnevezett felhasználói fiókok közül.</span><span class="sxs-lookup"><span data-stu-id="2578c-200">When you add a task or task collection, you can specify that the task runs under one of the named user accounts defined on the pool.</span></span>

<span data-ttu-id="2578c-201">Egy névvel ellátott felhasználói fiókot akkor hasznos, ha szeretne a feladatok az azonos felhasználói fiókhoz tartozó összes feladatok futtatása, de ezeket elszigeteli a más feladatok egy időben futó feladatok.</span><span class="sxs-lookup"><span data-stu-id="2578c-201">A named user account is useful when you want to run all tasks in a job under the same user account, but isolate them from tasks running in other jobs at the same time.</span></span> <span data-ttu-id="2578c-202">Például minden feladat nevű felhasználót kell létrehozni, és az adott nevű felhasználói fiók minden feladat feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="2578c-202">For example, you can create a named user for each job, and run each job's tasks under that named user account.</span></span> <span data-ttu-id="2578c-203">Minden feladat dolgozhat ugyanazon a titkos kulcs a saját feladatokhoz, de nem váltanak futó feladatok.</span><span class="sxs-lookup"><span data-stu-id="2578c-203">Each job can then share a secret with its own tasks, but not with tasks running in other jobs.</span></span>

<span data-ttu-id="2578c-204">Elnevezett felhasználói fiók segítségével, amely olyan engedélyeket ad meg a külső erőforrások, például fájlmegosztásokat feladat futtatása.</span><span class="sxs-lookup"><span data-stu-id="2578c-204">You can also use a named user account to run a task that sets permissions on external resources such as file shares.</span></span> <span data-ttu-id="2578c-205">Elnevezett felhasználói fiókkal szabályozhatja a felhasználó identitását, és használhatja a felhasználói identitás engedélyek beállítása.</span><span class="sxs-lookup"><span data-stu-id="2578c-205">With a named user account, you control the user identity and can use that user identity to set permissions.</span></span>  

<span data-ttu-id="2578c-206">Nevesített felhasználó fiókok lehetővé teszik a jelszó nélküli SSH Linux-csomópontok között.</span><span class="sxs-lookup"><span data-stu-id="2578c-206">Named user accounts enable password-less SSH between Linux nodes.</span></span> <span data-ttu-id="2578c-207">Többpéldányos feladatok futtatásához szükséges Linux csomópontok használható egy névvel ellátott felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="2578c-207">You can use a named user account with Linux nodes that need to run multi-instance tasks.</span></span> <span data-ttu-id="2578c-208">A készlet minden egyes csomópont futtathatnak feladatokat a teljes készletében egy felhasználói fiókkal.</span><span class="sxs-lookup"><span data-stu-id="2578c-208">Each node in the pool can run tasks under a user account defined on the whole pool.</span></span> <span data-ttu-id="2578c-209">Többpéldányos feladatokkal kapcsolatos további információkért lásd: [használata többszörös\-MPI-alkalmazások futtatására feladatok példány](batch-mpi.md).</span><span class="sxs-lookup"><span data-stu-id="2578c-209">For more information about multi-instance tasks, see [Use multi\-instance tasks to run MPI applications](batch-mpi.md).</span></span>

### <a name="create-named-user-accounts"></a><span data-ttu-id="2578c-210">Elnevezett felhasználói fiókok létrehozása</span><span class="sxs-lookup"><span data-stu-id="2578c-210">Create named user accounts</span></span>

<span data-ttu-id="2578c-211">Elnevezett felhasználói fiókok létrehozása a kötegelt, vegye fel a felhasználói fiókok gyűjteménye a készlethez.</span><span class="sxs-lookup"><span data-stu-id="2578c-211">To create named user accounts in Batch, add a collection of user accounts to the pool.</span></span> <span data-ttu-id="2578c-212">Az alábbi kódtöredékek bemutatják, hogyan elnevezett felhasználói fiókok létrehozása a .NET, Java és Python.</span><span class="sxs-lookup"><span data-stu-id="2578c-212">The following code snippets show how to create named user accounts in .NET, Java, and Python.</span></span> <span data-ttu-id="2578c-213">Ezek kódrészletek létrehozását mutatják be mind a rendszergazda, és a nem rendszergazdai fiókok, a készlet neve.</span><span class="sxs-lookup"><span data-stu-id="2578c-213">These code snippets show how to create both admin and non-admin named accounts on a pool.</span></span> <span data-ttu-id="2578c-214">A példák segítségével a felhőalapú szolgáltatás konfigurációja címkészletek létrehozása, de a virtuálisgép-konfiguráció használata Windows vagy Linux készlet létrehozásakor használt sémának.</span><span class="sxs-lookup"><span data-stu-id="2578c-214">The examples create pools using the cloud service configuration, but you use the same approach when creating a Windows or Linux pool using the virtual machine configuration.</span></span>

#### <a name="batch-net-example-windows"></a><span data-ttu-id="2578c-215">Batch .NET típusú példát (Windows)</span><span class="sxs-lookup"><span data-stu-id="2578c-215">Batch .NET example (Windows)</span></span>

```csharp
CloudPool pool = null;
Console.WriteLine("Creating pool [{0}]...", poolId);

// Create a pool using the cloud service configuration.
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

// Commit the pool.
await pool.CommitAsync();
```

#### <a name="batch-net-example-linux"></a><span data-ttu-id="2578c-216">Batch .NET típusú példát (Linux)</span><span class="sxs-lookup"><span data-stu-id="2578c-216">Batch .NET example (Linux)</span></span>

```csharp
CloudPool pool = null;

// Obtain a collection of all available node agent SKUs.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of the VM image to use.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.Sku.Contains("14.04");

// Obtain the first node agent SKU in the collection that matches
// Ubuntu Server 14.04. 
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create the virtual machine configuration to use to create the pool.
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(imageReference, ubuntuAgentSku.Id);

Console.WriteLine("Creating pool [{0}]...", poolId);

// Create the unbound pool.
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

// Commit the pool.
await pool.CommitAsync();
```


#### <a name="batch-java-example"></a><span data-ttu-id="2578c-217">Kötegelt Java – példa</span><span class="sxs-lookup"><span data-stu-id="2578c-217">Batch Java example</span></span>

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

#### <a name="batch-python-example"></a><span data-ttu-id="2578c-218">Kötegelt Python – példa</span><span class="sxs-lookup"><span data-stu-id="2578c-218">Batch Python example</span></span>

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

### <a name="run-a-task-under-a-named-user-account-with-elevated-access"></a><span data-ttu-id="2578c-219">Emelt szintű hozzáférés egy névvel ellátott felhasználói fiókhoz tartozó feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="2578c-219">Run a task under a named user account with elevated access</span></span>

<span data-ttu-id="2578c-220">Feladat futtatása egy rendszergazda jogú felhasználóként, állítsa be a feladat **UserIdentity** tulajdonság hoztak létre elnevezett felhasználói fiókhoz az **ElevationLevel** tulajdonsága `Admin`.</span><span class="sxs-lookup"><span data-stu-id="2578c-220">To run a task as an elevated user, set the task's **UserIdentity** property to a named user account that was created with its **ElevationLevel** property set to `Admin`.</span></span>

<span data-ttu-id="2578c-221">A kódrészletet határozza meg, hogy a feladat egy névvel ellátott felhasználói fiókkal fusson.</span><span class="sxs-lookup"><span data-stu-id="2578c-221">This code snippet specifies that the task should run under a named user account.</span></span> <span data-ttu-id="2578c-222">A névvel ellátott felhasználói fiókot az készletében van definiálva, a készlet létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="2578c-222">This named user account was defined on the pool when the pool was created.</span></span> <span data-ttu-id="2578c-223">Ebben az esetben az elnevezett felhasználói fiók rendszergazdai jogosultságokkal rendelkező jött létre:</span><span class="sxs-lookup"><span data-stu-id="2578c-223">In this case, the named user account was created with admin permissions:</span></span>

```csharp
CloudTask task = new CloudTask("1", "cmd.exe /c echo 1");
task.UserIdentity = new UserIdentity(AdminUserAccountName);
```

## <a name="update-your-code-to-the-latest-batch-client-library"></a><span data-ttu-id="2578c-224">Frissítse a kódot a legújabb kötegelt ügyféloldali kódtár</span><span class="sxs-lookup"><span data-stu-id="2578c-224">Update your code to the latest Batch client library</span></span>

<span data-ttu-id="2578c-225">A Batch szolgáltatás 2017-01-01.4.0 tartalmazza az használhatatlanná tévő változást, cseréje a **runElevated** tulajdonság érhető el a korábbi verzióiban a **userIdentity** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="2578c-225">The Batch service version 2017-01-01.4.0 introduces a breaking change, replacing the **runElevated** property available in earlier versions with the **userIdentity** property.</span></span> <span data-ttu-id="2578c-226">A következő táblázatok tartalmazzák, amelyek segítségével frissítse a kódot a klienskódtárak segítségével korábbi verzióiból egyszerű leképezéseket.</span><span class="sxs-lookup"><span data-stu-id="2578c-226">The following tables provide a simple mapping that you can use to update your code from earlier versions of the client libraries.</span></span>

### <a name="batch-net"></a><span data-ttu-id="2578c-227">Batch .NET</span><span class="sxs-lookup"><span data-stu-id="2578c-227">Batch .NET</span></span>

| <span data-ttu-id="2578c-228">Ha a kódot használja...</span><span class="sxs-lookup"><span data-stu-id="2578c-228">If your code uses...</span></span>                  | <span data-ttu-id="2578c-229">Frissítse úgy, hogy...</span><span class="sxs-lookup"><span data-stu-id="2578c-229">Update it to....</span></span>                                                                                                 |
|---------------------------------------|------------------------------------------------------------------------------------------------------------------|
| `CloudTask.RunElevated = true;`       | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin));`    |
| `CloudTask.RunElevated = false;`      | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.NonAdmin));` |
| <span data-ttu-id="2578c-230">`CloudTask.RunElevated`Nincs megadva</span><span class="sxs-lookup"><span data-stu-id="2578c-230">`CloudTask.RunElevated` not specified</span></span> | <span data-ttu-id="2578c-231">Nincs frissítés szükséges</span><span class="sxs-lookup"><span data-stu-id="2578c-231">No update required</span></span>                                                                                               |

### <a name="batch-java"></a><span data-ttu-id="2578c-232">Kötegelt Java</span><span class="sxs-lookup"><span data-stu-id="2578c-232">Batch Java</span></span>

| <span data-ttu-id="2578c-233">Ha a kódot használja...</span><span class="sxs-lookup"><span data-stu-id="2578c-233">If your code uses...</span></span>                      | <span data-ttu-id="2578c-234">Frissítse úgy, hogy...</span><span class="sxs-lookup"><span data-stu-id="2578c-234">Update it to....</span></span>                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `CloudTask.withRunElevated(true);`        | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.ADMIN));`    |
| `CloudTask.withRunElevated(false);`       | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.NONADMIN));` |
| <span data-ttu-id="2578c-235">`CloudTask.withRunElevated`Nincs megadva</span><span class="sxs-lookup"><span data-stu-id="2578c-235">`CloudTask.withRunElevated` not specified</span></span> | <span data-ttu-id="2578c-236">Nincs frissítés szükséges</span><span class="sxs-lookup"><span data-stu-id="2578c-236">No update required</span></span>                                                                                                                     |

### <a name="batch-python"></a><span data-ttu-id="2578c-237">Batch Python</span><span class="sxs-lookup"><span data-stu-id="2578c-237">Batch Python</span></span>

| <span data-ttu-id="2578c-238">Ha a kódot használja...</span><span class="sxs-lookup"><span data-stu-id="2578c-238">If your code uses...</span></span>                      | <span data-ttu-id="2578c-239">Frissítse úgy, hogy...</span><span class="sxs-lookup"><span data-stu-id="2578c-239">Update it to....</span></span>                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `run_elevated=True`                       | <span data-ttu-id="2578c-240">`user_identity=user`, ahol</span><span class="sxs-lookup"><span data-stu-id="2578c-240">`user_identity=user`, where</span></span> <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.admin)) `                |
| `run_elevated=False`                      | <span data-ttu-id="2578c-241">`user_identity=user`, ahol</span><span class="sxs-lookup"><span data-stu-id="2578c-241">`user_identity=user`, where</span></span> <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.nonadmin)) `             |
| <span data-ttu-id="2578c-242">`run_elevated`Nincs megadva</span><span class="sxs-lookup"><span data-stu-id="2578c-242">`run_elevated` not specified</span></span> | <span data-ttu-id="2578c-243">Nincs frissítés szükséges</span><span class="sxs-lookup"><span data-stu-id="2578c-243">No update required</span></span>                                                                                                                                  |


## <a name="next-steps"></a><span data-ttu-id="2578c-244">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2578c-244">Next steps</span></span>

### <a name="batch-forum"></a><span data-ttu-id="2578c-245">Batch fórum</span><span class="sxs-lookup"><span data-stu-id="2578c-245">Batch Forum</span></span>

<span data-ttu-id="2578c-246">A [Azure Batch fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch) az MSDN webhelyen van remek kötegelt tárgyalja, és kérdése van a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="2578c-246">The [Azure Batch Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch) on MSDN is a great place to discuss Batch and ask questions about the service.</span></span> <span data-ttu-id="2578c-247">Központi a keresztül a hasznos rögzített bejegyzések, és a kötegelt megoldások létrehozása során felmerülő kérdéseit.</span><span class="sxs-lookup"><span data-stu-id="2578c-247">Head on over for helpful pinned posts, and post your questions as they arise while you build your Batch solutions.</span></span>