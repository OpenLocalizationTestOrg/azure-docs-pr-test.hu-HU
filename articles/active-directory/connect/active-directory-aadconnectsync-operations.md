---
title: "Azure AD Connect szinkronizálása: működtetési feladatok és szempontok |} Microsoft Docs"
description: "Ez a témakör ismerteti az operatív feladatok az Azure AD Connect szinkronizálási szolgáltatás és előkészítése operációs ezt az összetevőt."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: b29c1790-37a3-470f-ab69-3cee824d220d
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: b7583a1556bb1113f349a78890768451e39c6878
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-operational-tasks-and-consideration"></a><span data-ttu-id="9917f-103">Azure AD Connect szinkronizálása: működtetési feladatok és szempont</span><span class="sxs-lookup"><span data-stu-id="9917f-103">Azure AD Connect sync: Operational tasks and consideration</span></span>
<span data-ttu-id="9917f-104">Ez a témakör célja az Azure AD Connect szinkronizálási szolgáltatás működési feladatokat írják le.</span><span class="sxs-lookup"><span data-stu-id="9917f-104">The objective of this topic is to describe operational tasks for Azure AD Connect sync.</span></span>

## <a name="staging-mode"></a><span data-ttu-id="9917f-105">Átmeneti mód</span><span class="sxs-lookup"><span data-stu-id="9917f-105">Staging mode</span></span>
<span data-ttu-id="9917f-106">Az átmeneti környezetű üzemmód használható több forgatókönyvek, köztük:</span><span class="sxs-lookup"><span data-stu-id="9917f-106">Staging mode can be used for several scenarios, including:</span></span>

* <span data-ttu-id="9917f-107">Magas rendelkezésre állású.</span><span class="sxs-lookup"><span data-stu-id="9917f-107">High availability.</span></span>
* <span data-ttu-id="9917f-108">Tesztelése és telepítése az új konfigurációs módosításokat.</span><span class="sxs-lookup"><span data-stu-id="9917f-108">Test and deploy new configuration changes.</span></span>
* <span data-ttu-id="9917f-109">Új kiszolgáló esetében, és a régi leszereléséhez.</span><span class="sxs-lookup"><span data-stu-id="9917f-109">Introduce a new server and decommission the old.</span></span>

<span data-ttu-id="9917f-110">Átmeneti módban a kiszolgálóval a konfigurációs módosításokat, és tekintse meg a módosítások elvégzése előtt a kiszolgáló aktív.</span><span class="sxs-lookup"><span data-stu-id="9917f-110">With a server in staging mode, you can make changes to the configuration and preview the changes before you make the server active.</span></span> <span data-ttu-id="9917f-111">Lehetővé teszi teljes importálást és teljes szinkronizálás győződjön meg arról, hogy minden módosítás előtt ezeket a módosításokat az éles környezetében várható futtatásához.</span><span class="sxs-lookup"><span data-stu-id="9917f-111">It also allows you to run full import and full synchronization to verify that all changes are expected before you make these changes into your production environment.</span></span>

<span data-ttu-id="9917f-112">A telepítés során válassza ki a kiszolgálót a **átmeneti módban**.</span><span class="sxs-lookup"><span data-stu-id="9917f-112">During installation, you can select the server to be in **staging mode**.</span></span> <span data-ttu-id="9917f-113">Ez a művelet lehetővé teszi a kiszolgáló importálása és szinkronizálás aktív, de nem futtatható bármely exportálja.</span><span class="sxs-lookup"><span data-stu-id="9917f-113">This action makes the server active for import and synchronization, but it does not run any exports.</span></span> <span data-ttu-id="9917f-114">Átmeneti módban nem fut a jelszó-szinkronizálás és jelszóvisszaíró, még akkor is, ha ezek a szolgáltatások telepítésekor kiválasztott.</span><span class="sxs-lookup"><span data-stu-id="9917f-114">A server in staging mode is not running password sync or password writeback, even if you selected these features during installation.</span></span> <span data-ttu-id="9917f-115">Az átmeneti környezetű üzemmód letiltása esetén a kiszolgáló kezdődik, exportálása, lehetővé teszi, hogy a jelszó-szinkronizálást, és lehetővé teszi, hogy a jelszóvisszaíró.</span><span class="sxs-lookup"><span data-stu-id="9917f-115">When you disable staging mode, the server starts exporting, enables password sync, and enables password writeback.</span></span>

<span data-ttu-id="9917f-116">Az export továbbra is kényszerítheti a synchronization service manager használatával.</span><span class="sxs-lookup"><span data-stu-id="9917f-116">You can still force an export by using the synchronization service manager.</span></span>

<span data-ttu-id="9917f-117">Átmeneti módban továbbra is kaphat a módosítást az Active Directoryból és az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9917f-117">A server in staging mode continues to receive changes from Active Directory and Azure AD.</span></span> <span data-ttu-id="9917f-118">Azt mindig van egy példányát a legutóbbi módosítások és a részleg nagyon gyorsan elvégezhető a kötelezettségeit, egy másik kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="9917f-118">It always has a copy of the latest changes and can very fast take over the responsibilities of another server.</span></span> <span data-ttu-id="9917f-119">Ha konfigurációs módosítások az elsődleges kiszolgálóval, feladata a ugyanazt a módosításokat a kiszolgáló átmeneti módban.</span><span class="sxs-lookup"><span data-stu-id="9917f-119">If you make configuration changes to your primary server, it is your responsibility to make the same changes to the server in staging mode.</span></span>

<span data-ttu-id="9917f-120">Azok a régebbi szinkronizálási technológiák ismerete is az átmeneti környezetű üzemmód nem azonos mivel a kiszolgáló csak a saját SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="9917f-120">For those of you with knowledge of older sync technologies, the staging mode is different since the server has its own SQL database.</span></span> <span data-ttu-id="9917f-121">Ez az architektúra lehetővé teszi, hogy az átmeneti mód kiszolgáló ugyanabban az adatközpontban található.</span><span class="sxs-lookup"><span data-stu-id="9917f-121">This architecture allows the staging mode server to be located in a different datacenter.</span></span>

### <a name="verify-the-configuration-of-a-server"></a><span data-ttu-id="9917f-122">A kiszolgáló konfigurációjának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="9917f-122">Verify the configuration of a server</span></span>
<span data-ttu-id="9917f-123">Ha szeretné alkalmazni ezt a módszert, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9917f-123">To apply this method, follow these steps:</span></span>

1. [<span data-ttu-id="9917f-124">Előkészítése</span><span class="sxs-lookup"><span data-stu-id="9917f-124">Prepare</span></span>](#prepare)
2. [<span data-ttu-id="9917f-125">Konfigurálás</span><span class="sxs-lookup"><span data-stu-id="9917f-125">Configuration</span></span>](#configuration)
3. [<span data-ttu-id="9917f-126">Importálja és szinkronizálja</span><span class="sxs-lookup"><span data-stu-id="9917f-126">Import and Synchronize</span></span>](#import-and-synchronize)
4. [<span data-ttu-id="9917f-127">Ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="9917f-127">Verify</span></span>](#verify)
5. [<span data-ttu-id="9917f-128">Az active server kapcsoló</span><span class="sxs-lookup"><span data-stu-id="9917f-128">Switch active server</span></span>](#switch-active-server)

#### <a name="prepare"></a><span data-ttu-id="9917f-129">Előkészítés</span><span class="sxs-lookup"><span data-stu-id="9917f-129">Prepare</span></span>
1. <span data-ttu-id="9917f-130">Azure AD Connectet telepíti, válassza ki **átmeneti módban**, és kikapcsolni **a szinkronizálás megkezdéséhez** a varázsló utolsó lapján.</span><span class="sxs-lookup"><span data-stu-id="9917f-130">Install Azure AD Connect, select **staging mode**, and unselect **start synchronization** on the last page in the installation wizard.</span></span> <span data-ttu-id="9917f-131">Ebben a módban a szinkronizálási motor manuális futtatása teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="9917f-131">This mode allows you to run the sync engine manually.</span></span>
   <span data-ttu-id="9917f-132">![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)</span><span class="sxs-lookup"><span data-stu-id="9917f-132">![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)</span></span>
2. <span data-ttu-id="9917f-133">Jelentkezzen ki/sign és a start menüből válassza a **szinkronizálási szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="9917f-133">Sign off/sign in and from the start menu select **Synchronization Service**.</span></span>

#### <a name="configuration"></a><span data-ttu-id="9917f-134">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="9917f-134">Configuration</span></span>
<span data-ttu-id="9917f-135">Ha egyéni módosításokat végzett az elsődleges kiszolgáló, és a átmeneti kiszolgálóval konfigurációt szeretne, majd használja [az Azure AD Connect konfigurációs dokumentáló](https://github.com/Microsoft/AADConnectConfigDocumenter).</span><span class="sxs-lookup"><span data-stu-id="9917f-135">If you have made custom changes to the primary server and want to compare the configuration with the staging server, then use [Azure AD Connect configuration documenter](https://github.com/Microsoft/AADConnectConfigDocumenter).</span></span>

#### <a name="import-and-synchronize"></a><span data-ttu-id="9917f-136">Importálja és szinkronizálja</span><span class="sxs-lookup"><span data-stu-id="9917f-136">Import and Synchronize</span></span>
1. <span data-ttu-id="9917f-137">Válassza ki **összekötők**, és válassza ki az első összekötőt típusú **Active Directory tartományi szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="9917f-137">Select **Connectors**, and select the first Connector with the type **Active Directory Domain Services**.</span></span> <span data-ttu-id="9917f-138">Kattintson a **futtatása**, jelölje be **teljes importálás**, és **OK**.</span><span class="sxs-lookup"><span data-stu-id="9917f-138">Click **Run**, select **Full import**, and **OK**.</span></span> <span data-ttu-id="9917f-139">Hajtsa végre ezeket a lépéseket az összes ilyen típusú-összekötőhöz.</span><span class="sxs-lookup"><span data-stu-id="9917f-139">Do these steps for all Connectors of this type.</span></span>
2. <span data-ttu-id="9917f-140">Válassza ki az összekötő típusú **Azure Active Directoryban (Microsoft)**.</span><span class="sxs-lookup"><span data-stu-id="9917f-140">Select the Connector with type **Azure Active Directory (Microsoft)**.</span></span> <span data-ttu-id="9917f-141">Kattintson a **futtatása**, jelölje be **teljes importálás**, és **OK**.</span><span class="sxs-lookup"><span data-stu-id="9917f-141">Click **Run**, select **Full import**, and **OK**.</span></span>
3. <span data-ttu-id="9917f-142">Győződjön meg arról, hogy a lap összekötők aktív marad.</span><span class="sxs-lookup"><span data-stu-id="9917f-142">Make sure the tab Connectors is still selected.</span></span> <span data-ttu-id="9917f-143">Minden típusú összekötő **Active Directory tartományi szolgáltatások**, kattintson a **futtatása**, jelölje be **különbözeti szinkronizálás**, és **OK**.</span><span class="sxs-lookup"><span data-stu-id="9917f-143">For each Connector with type **Active Directory Domain Services**, click **Run**, select **Delta Synchronization**, and **OK**.</span></span>
4. <span data-ttu-id="9917f-144">Válassza ki az összekötő típusú **Azure Active Directoryban (Microsoft)**.</span><span class="sxs-lookup"><span data-stu-id="9917f-144">Select the Connector with type **Azure Active Directory (Microsoft)**.</span></span> <span data-ttu-id="9917f-145">Kattintson a **futtatása**, jelölje be **különbözeti szinkronizálás**, és **OK**.</span><span class="sxs-lookup"><span data-stu-id="9917f-145">Click **Run**, select **Delta Synchronization**, and **OK**.</span></span>

<span data-ttu-id="9917f-146">Most előkészítette rendelkezik exportálási módosításai az Azure AD és a helyszíni AD (ha az Exchange hibrid telepítést használ).</span><span class="sxs-lookup"><span data-stu-id="9917f-146">You have now staged export changes to Azure AD and on-premises AD (if you are using Exchange hybrid deployment).</span></span> <span data-ttu-id="9917f-147">A következő lépéseket lehetővé teszi a vizsgálandó ténylegesen a könyvtárakat az exportálás megkezdése előtt megváltoztatta újdonságai.</span><span class="sxs-lookup"><span data-stu-id="9917f-147">The next steps allow you to inspect what is about to change before you actually start the export to the directories.</span></span>

#### <a name="verify"></a><span data-ttu-id="9917f-148">Ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="9917f-148">Verify</span></span>
1. <span data-ttu-id="9917f-149">Indítsa el egy parancssort, és navigáljon a`%ProgramFiles%\Microsoft Azure AD Sync\bin`</span><span class="sxs-lookup"><span data-stu-id="9917f-149">Start a cmd prompt and go to `%ProgramFiles%\Microsoft Azure AD Sync\bin`</span></span>
2. <span data-ttu-id="9917f-150">Futtatás: `csexport "Name of Connector" %temp%\export.xml /f:x` a nevét, az összekötő szinkronizálási szolgáltatás található.</span><span class="sxs-lookup"><span data-stu-id="9917f-150">Run: `csexport "Name of Connector" %temp%\export.xml /f:x` The name of the Connector can be found in Synchronization Service.</span></span> <span data-ttu-id="9917f-151">A "contoso.com – AAD" hasonló névvel rendelkezik az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9917f-151">It has a name similar to "contoso.com – AAD" for Azure AD.</span></span>
3. <span data-ttu-id="9917f-152">A szakasz a PowerShell parancsfájl másolása [CSAnalyzer](#appendix-csanalyzer) nevű fájlba `csanalyzer.ps1`.</span><span class="sxs-lookup"><span data-stu-id="9917f-152">Copy the PowerShell script from the section [CSAnalyzer](#appendix-csanalyzer) to a file named `csanalyzer.ps1`.</span></span>
4. <span data-ttu-id="9917f-153">Nyisson meg egy PowerShell-ablakot, és keresse meg a mappát, amelyben létrehozta a PowerShell-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="9917f-153">Open a PowerShell window and browse to the folder where you created the PowerShell script.</span></span>
5. <span data-ttu-id="9917f-154">Futtatás: `.\csanalyzer.ps1 -xmltoimport %temp%\export.xml`.</span><span class="sxs-lookup"><span data-stu-id="9917f-154">Run: `.\csanalyzer.ps1 -xmltoimport %temp%\export.xml`.</span></span>
6. <span data-ttu-id="9917f-155">Most már rendelkezik egy fájlt **processedusers1.csv** , amely a Microsoft Excel kell vizsgálni.</span><span class="sxs-lookup"><span data-stu-id="9917f-155">You now have a file named **processedusers1.csv** that can be examined in Microsoft Excel.</span></span> <span data-ttu-id="9917f-156">Az Azure ad Szolgáltatásba exportálni előkészített összes módosítást a fájlban találhatók.</span><span class="sxs-lookup"><span data-stu-id="9917f-156">All changes staged to be exported to Azure AD are found in this file.</span></span>
7. <span data-ttu-id="9917f-157">Hajtsa végre a módosításokat az adatok vagy konfiguráció, és futtassa ezeket a lépéseket újra (importálás és szinkronizálás és ellenőrzése) addig, amíg az exportálni kívánt módosításokkal várható.</span><span class="sxs-lookup"><span data-stu-id="9917f-157">Make necessary changes to the data or configuration and run these steps again (Import and Synchronize and Verify) until the changes that are about to be exported are expected.</span></span>

#### <a name="switch-active-server"></a><span data-ttu-id="9917f-158">Az active server kapcsoló</span><span class="sxs-lookup"><span data-stu-id="9917f-158">Switch active server</span></span>
1. <span data-ttu-id="9917f-159">A jelenleg aktív kiszolgálón vagy kapcsolja ki a kiszolgálót (a DirSync vagy FIM vagy az Azure AD Sync), akkor nem exportálása az Azure ad Szolgáltatásba, vagy állítsa az átmeneti módban (az Azure AD Connect).</span><span class="sxs-lookup"><span data-stu-id="9917f-159">On the currently active server, either turn off the server (DirSync/FIM/Azure AD Sync) so it is not exporting to Azure AD or set it in staging mode (Azure AD Connect).</span></span>
2. <span data-ttu-id="9917f-160">A telepítővarázsló futtatása a kiszolgálón a **átmeneti módban** és tiltsa le a **átmeneti módban**.</span><span class="sxs-lookup"><span data-stu-id="9917f-160">Run the installation wizard on the server in **staging mode** and disable **staging mode**.</span></span>
   <span data-ttu-id="9917f-161">![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)</span><span class="sxs-lookup"><span data-stu-id="9917f-161">![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)</span></span>

## <a name="disaster-recovery"></a><span data-ttu-id="9917f-162">Vészhelyreállítás</span><span class="sxs-lookup"><span data-stu-id="9917f-162">Disaster recovery</span></span>
<span data-ttu-id="9917f-163">A megvalósítási terv részét képezi, mi a teendő, ha van egy olyan vészhelyzet esetén Ha elveszti a sync-kiszolgáló tervezését.</span><span class="sxs-lookup"><span data-stu-id="9917f-163">Part of the implementation design is to plan for what to do in case there is a disaster where you lose the sync server.</span></span> <span data-ttu-id="9917f-164">Számos különböző képességekkel és melyiket használja többek között számos tényezőtől függ:</span><span class="sxs-lookup"><span data-stu-id="9917f-164">There are different models to use and which one to use depends on several factors including:</span></span>

* <span data-ttu-id="9917f-165">Mi az a tűrés, nem tudja ellenőrizze a objektumok változásait az Azure AD a leállások során?</span><span class="sxs-lookup"><span data-stu-id="9917f-165">What is your tolerance for not being able make changes to objects in Azure AD during the downtime?</span></span>
* <span data-ttu-id="9917f-166">Ha használja a jelszó-szinkronizálás, tegye a felhasználók fogadja el, hogy kell-e a régi jelszót használja az Azure AD abban az esetben, ha azok megváltoztatnia a helyszíni?</span><span class="sxs-lookup"><span data-stu-id="9917f-166">If you use password synchronization, do the users accept that they have to use the old password in Azure AD in case they change it on-premises?</span></span>
* <span data-ttu-id="9917f-167">Rendelkezik a valós idejű műveletek, például a jelszóvisszaírás függőség?</span><span class="sxs-lookup"><span data-stu-id="9917f-167">Do you have a dependency on real-time operations, such as password writeback?</span></span>

<span data-ttu-id="9917f-168">Attól függően, hogy ezek a kérdések és a szervezet házirendje a választ a következő stratégiák egyikét is kell végrehajtani:</span><span class="sxs-lookup"><span data-stu-id="9917f-168">Depending on the answers to these questions and your organization’s policy, one of the following strategies can be implemented:</span></span>

* <span data-ttu-id="9917f-169">Építse újra, amikor szükséges.</span><span class="sxs-lookup"><span data-stu-id="9917f-169">Rebuild when needed.</span></span>
* <span data-ttu-id="9917f-170">Tartalék készenléti kiszolgáló, úgynevezett **átmeneti módban**.</span><span class="sxs-lookup"><span data-stu-id="9917f-170">Have a spare standby server, known as **staging mode**.</span></span>
* <span data-ttu-id="9917f-171">Használja a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="9917f-171">Use virtual machines.</span></span>

<span data-ttu-id="9917f-172">Ha nem használja a beépített SQL Express adatbázist, majd tekintse meg a [SQL magas rendelkezésre állású](#sql-high-availability) szakasz.</span><span class="sxs-lookup"><span data-stu-id="9917f-172">If you do not use the built-in SQL Express database, then you should also review the [SQL High Availability](#sql-high-availability) section.</span></span>

### <a name="rebuild-when-needed"></a><span data-ttu-id="9917f-173">Szükség esetén újraépítése</span><span class="sxs-lookup"><span data-stu-id="9917f-173">Rebuild when needed</span></span>
<span data-ttu-id="9917f-174">Életképes stratégiát, hogy szükség esetén egy kiszolgáló újjáépítése tervezése.</span><span class="sxs-lookup"><span data-stu-id="9917f-174">A viable strategy is to plan for a server rebuild when needed.</span></span> <span data-ttu-id="9917f-175">A szinkronizálási motor és a kezdeti importálás és szinkronizálás elvégezhető néhány órán belül nem általában telepítése.</span><span class="sxs-lookup"><span data-stu-id="9917f-175">Usually, installing the sync engine and do the initial import and sync can be completed within a few hours.</span></span> <span data-ttu-id="9917f-176">Ha tartalék kiszolgáló nem érhető el, akkor lehet a ideiglenesen tartományvezérlő is tárolni a szinkronizálási motor.</span><span class="sxs-lookup"><span data-stu-id="9917f-176">If there isn’t a spare server available, it is possible to temporarily use a domain controller to host the sync engine.</span></span>

<span data-ttu-id="9917f-177">A Szinkronizáló vezérlő kiszolgálója nem tárolja az objektumok olyan állapotokat, az adatbázis is építhető újra, az Active Directory és az Azure AD az adatokból.</span><span class="sxs-lookup"><span data-stu-id="9917f-177">The sync engine server does not store any state about the objects so the database can be rebuilt from the data in Active Directory and Azure AD.</span></span> <span data-ttu-id="9917f-178">A **sourceAnchor** attribútum segítségével csatlakoztassa az objektumokat a helyszíni és felhőben.</span><span class="sxs-lookup"><span data-stu-id="9917f-178">The **sourceAnchor** attribute is used to join the objects from on-premises and the cloud.</span></span> <span data-ttu-id="9917f-179">A meglévő objektumokat a helyszíni és a felhő kiszolgáló újraépítése, majd a szinkronizálási motor megegyezik azokat az objektumokat együtt újra az újratelepítést.</span><span class="sxs-lookup"><span data-stu-id="9917f-179">If you rebuild the server with existing objects on-premises and the cloud, then the sync engine matches those objects together again on reinstallation.</span></span> <span data-ttu-id="9917f-180">A dokumentum, és menteni kell minden olyan a kiszolgálót, például a szűrés és szinkronizálási szabályok konfigurációs módosításait.</span><span class="sxs-lookup"><span data-stu-id="9917f-180">The things you need to document and save are the configuration changes made to the server, such as filtering and synchronization rules.</span></span> <span data-ttu-id="9917f-181">Ezen egyéni konfigurációk kell kell újra alkalmazza a szinkronizálás megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="9917f-181">These custom configurations must be reapplied before you start synchronizing.</span></span>

### <a name="have-a-spare-standby-server---staging-mode"></a><span data-ttu-id="9917f-182">Tartalék készenléti kiszolgáló - átmeneti módban</span><span class="sxs-lookup"><span data-stu-id="9917f-182">Have a spare standby server - staging mode</span></span>
<span data-ttu-id="9917f-183">Ha összetettebb környezetben, akkor ajánlott, ha egy vagy több készenléti kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="9917f-183">If you have a more complex environment, then having one or more standby servers is recommended.</span></span> <span data-ttu-id="9917f-184">A telepítés során engedélyezheti a server a **átmeneti módban**.</span><span class="sxs-lookup"><span data-stu-id="9917f-184">During installation, you can enable a server to be in **staging mode**.</span></span>

<span data-ttu-id="9917f-185">További információkért lásd: [átmeneti módban](#staging-mode).</span><span class="sxs-lookup"><span data-stu-id="9917f-185">For more information, see [staging mode](#staging-mode).</span></span>

### <a name="use-virtual-machines"></a><span data-ttu-id="9917f-186">Virtuális gépek használata</span><span class="sxs-lookup"><span data-stu-id="9917f-186">Use virtual machines</span></span>
<span data-ttu-id="9917f-187">A közös és támogatott módja, hogy a szinkronizálási motor egy virtuális gép futtatásához.</span><span class="sxs-lookup"><span data-stu-id="9917f-187">A common and supported method is to run the sync engine in a virtual machine.</span></span> <span data-ttu-id="9917f-188">Abban az esetben, ha a gazdagép egy hibát tartalmaz, a kép és a Szinkronizáló vezérlő kiszolgálója áttelepíthetők egy másik kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="9917f-188">In case the host has an issue, the image with the sync engine server can be migrated to another server.</span></span>

### <a name="sql-high-availability"></a><span data-ttu-id="9917f-189">Az SQL magas rendelkezésre állás</span><span class="sxs-lookup"><span data-stu-id="9917f-189">SQL High Availability</span></span>
<span data-ttu-id="9917f-190">Ha nem használ az SQL Server Express, az Azure AD Connect előre, majd az SQL Server magas rendelkezésre állás is meg kell fontolni.</span><span class="sxs-lookup"><span data-stu-id="9917f-190">If you are not using the SQL Server Express that comes with Azure AD Connect, then high availability for SQL Server should also be considered.</span></span> <span data-ttu-id="9917f-191">A támogatott magas rendelkezésre állás megoldásai a következők: SQL-fürtözés és AOA (az Always On rendelkezésre állási csoportok).</span><span class="sxs-lookup"><span data-stu-id="9917f-191">The high availability solutions supported include SQL clustering and AOA (Always On Availability Groups).</span></span> <span data-ttu-id="9917f-192">Nem támogatott megoldásai a következők tükrözés.</span><span class="sxs-lookup"><span data-stu-id="9917f-192">Unsupported solutions include mirroring.</span></span>

<span data-ttu-id="9917f-193">SQL AOA támogatása az Azure AD Connect 1.1.524.0 verziójában lett hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="9917f-193">Support for SQL AOA was added to Azure AD Connect in version 1.1.524.0.</span></span> <span data-ttu-id="9917f-194">Az Azure AD Connect telepítése előtt engedélyeznie kell az SQL AOA.</span><span class="sxs-lookup"><span data-stu-id="9917f-194">You must enable SQL AOA before installing Azure AD Connect.</span></span> <span data-ttu-id="9917f-195">A telepítés során az Azure AD Connect észleli, hogy a megadott SQL-példány engedélyezve van az SQL AOA vagy nem.</span><span class="sxs-lookup"><span data-stu-id="9917f-195">During installation, Azure AD Connect detects whether the SQL instance provided is enabled for SQL AOA or not.</span></span> <span data-ttu-id="9917f-196">Ha SQL AOA engedélyezve van, az Azure AD Connect további adatok Ha SQL AOA replikáció szinkron vagy aszinkron replikáció használatára van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="9917f-196">If SQL AOA is enabled, Azure AD Connect further figures out if SQL AOA is configured to use synchronous replication or asynchronous replication.</span></span> <span data-ttu-id="9917f-197">A rendelkezésre állási csoport figyelőjének beállításakor javasoljuk, hogy a RegisterAllProvidersIP tulajdonságot 0 értékre állítja.</span><span class="sxs-lookup"><span data-stu-id="9917f-197">When setting up the Availability Group Listener, it is recommended that you set the RegisterAllProvidersIP property to 0.</span></span> <span data-ttu-id="9917f-198">Ennek az az oka az Azure AD Connect jelenleg használ SQL Native Client kapcsolódni az SQL és az SQL natív ügyfél nem támogatja a MultiSubNetFailover tulajdonság használatával.</span><span class="sxs-lookup"><span data-stu-id="9917f-198">This is because Azure AD Connect currently uses SQL Native Client to connect to SQL and SQL Native Client does not support the use of MultiSubNetFailover property.</span></span>

## <a name="appendix-csanalyzer"></a><span data-ttu-id="9917f-199">A függelék CSAnalyzer</span><span class="sxs-lookup"><span data-stu-id="9917f-199">Appendix CSAnalyzer</span></span>
<span data-ttu-id="9917f-200">Című témakör [ellenőrizze](#verify) a parancsfájl használatával.</span><span class="sxs-lookup"><span data-stu-id="9917f-200">See the section [verify](#verify) on how to use this script.</span></span>

```
Param(
    [Parameter(Mandatory=$true, HelpMessage="Must be a file generated using csexport 'Name of Connector' export.xml /f:x)")]
    [string]$xmltoimport="%temp%\exportedStage1a.xml",
    [Parameter(Mandatory=$false, HelpMessage="Maximum number of users per output file")][int]$batchsize=1000,
    [Parameter(Mandatory=$false, HelpMessage="Show console output")][bool]$showOutput=$false
)

#LINQ isn't loaded automatically, so force it
[Reflection.Assembly]::Load("System.Xml.Linq, Version=3.5.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089") | Out-Null

[int]$count=1
[int]$outputfilecount=1
[array]$objOutputUsers=@()

#XML must be generated using "csexport "Name of Connector" export.xml /f:x"
write-host "Importing XML" -ForegroundColor Yellow

#XmlReader.Create won't properly resolve the file location,
#so expand and then resolve it
$resolvedXMLtoimport=Resolve-Path -Path ([Environment]::ExpandEnvironmentVariables($xmltoimport))

#use an XmlReader to deal with even large files
$result=$reader = [System.Xml.XmlReader]::Create($resolvedXMLtoimport) 
$result=$reader.ReadToDescendant('cs-object')
do 
{
    #create the object placeholder
    #adding them up here means we can enforce consistency
    $objOutputUser=New-Object psobject
    Add-Member -InputObject $objOutputUser -MemberType NoteProperty -Name ID -Value ""
    Add-Member -InputObject $objOutputUser -MemberType NoteProperty -Name Type -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name DN -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name operation -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name UPN -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name displayName -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name sourceAnchor -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name alias -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name primarySMTP -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name onPremisesSamAccountName -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name mail -Value ""

    $user = [System.Xml.Linq.XElement]::ReadFrom($reader)
    if ($showOutput) {Write-Host Found an exported object... -ForegroundColor Green}

    #object id
    $outID=$user.Attribute('id').Value
    if ($showOutput) {Write-Host ID: $outID}
    $objOutputUser.ID=$outID

    #object type
    $outType=$user.Attribute('object-type').Value
    if ($showOutput) {Write-Host Type: $outType}
    $objOutputUser.Type=$outType

    #dn
    $outDN= $user.Element('unapplied-export').Element('delta').Attribute('dn').Value
    if ($showOutput) {Write-Host DN: $outDN}
    $objOutputUser.DN=$outDN

    #operation
    $outOperation= $user.Element('unapplied-export').Element('delta').Attribute('operation').Value
    if ($showOutput) {Write-Host Operation: $outOperation}
    $objOutputUser.operation=$outOperation

    #now that we have the basics, go get the details

    foreach ($attr in $user.Element('unapplied-export-hologram').Element('entry').Elements("attr"))
    {
        $attrvalue=$attr.Attribute('name').Value
        $internalvalue= $attr.Element('value').Value

        switch ($attrvalue)
        {
            "userPrincipalName"
            {
                if ($showOutput) {Write-Host UPN: $internalvalue}
                $objOutputUser.UPN=$internalvalue
            }
            "displayName"
            {
                if ($showOutput) {Write-Host displayName: $internalvalue}
                $objOutputUser.displayName=$internalvalue
            }
            "sourceAnchor"
            {
                if ($showOutput) {Write-Host sourceAnchor: $internalvalue}
                $objOutputUser.sourceAnchor=$internalvalue
            }
            "alias"
            {
                if ($showOutput) {Write-Host alias: $internalvalue}
                $objOutputUser.alias=$internalvalue
            }
            "proxyAddresses"
            {
                if ($showOutput) {Write-Host primarySMTP: ($internalvalue -replace "SMTP:","")}
                $objOutputUser.primarySMTP=$internalvalue -replace "SMTP:",""
            }
        }
    }

    $objOutputUsers += $objOutputUser

    Write-Progress -activity "Processing ${xmltoimport} in batches of ${batchsize}" -status "Batch ${outputfilecount}: " -percentComplete (($objOutputUsers.Count / $batchsize) * 100)

    #every so often, dump the processed users in case we blow up somewhere
    if ($count % $batchsize -eq 0)
    {
        Write-Host Hit the maximum users processed without completion... -ForegroundColor Yellow

        #export the collection of users as as CSV
        Write-Host Writing processedusers${outputfilecount}.csv -ForegroundColor Yellow
        $objOutputUsers | Export-Csv -path processedusers${outputfilecount}.csv -NoTypeInformation

        #increment the output file counter
        $outputfilecount+=1

        #reset the collection and the user counter
        $objOutputUsers = $null
        $count=0
    }

    $count+=1

    #need to bail out of the loop if no more users to process
    if ($reader.NodeType -eq [System.Xml.XmlNodeType]::EndElement)
    {
        break
    }

} while ($reader.Read)

#need to write out any users that didn't get picked up in a batch of 1000
#export the collection of users as as CSV
Write-Host Writing processedusers${outputfilecount}.csv -ForegroundColor Yellow
$objOutputUsers | Export-Csv -path processedusers${outputfilecount}.csv -NoTypeInformation
```

## <a name="next-steps"></a><span data-ttu-id="9917f-201">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9917f-201">Next steps</span></span>
<span data-ttu-id="9917f-202">**Áttekintő témakör**</span><span class="sxs-lookup"><span data-stu-id="9917f-202">**Overview topics**</span></span>  

* [<span data-ttu-id="9917f-203">Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása</span><span class="sxs-lookup"><span data-stu-id="9917f-203">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)  
* [<span data-ttu-id="9917f-204">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="9917f-204">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)  
