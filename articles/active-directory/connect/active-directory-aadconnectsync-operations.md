---
title: "Azure AD Connect szinkronizálása: működtetési feladatok és szempontok |} Microsoft Docs"
description: "Ez a témakör ismerteti az Azure AD Connect szinkronizálási szolgáltatás működési feladatokat, és hogyan tooprepare működtetéséhez ezt az összetevőt."
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
ms.openlocfilehash: e6b21262e0924785808888d66f08a04a56581edc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-operational-tasks-and-consideration"></a><span data-ttu-id="bd4fb-103">Azure AD Connect szinkronizálása: működtetési feladatok és szempont</span><span class="sxs-lookup"><span data-stu-id="bd4fb-103">Azure AD Connect sync: Operational tasks and consideration</span></span>
<span data-ttu-id="bd4fb-104">hello Ez a témakör célja toodescribe operatív feladatok az Azure AD Connect szinkronizálási szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-104">hello objective of this topic is toodescribe operational tasks for Azure AD Connect sync.</span></span>

## <a name="staging-mode"></a><span data-ttu-id="bd4fb-105">Átmeneti mód</span><span class="sxs-lookup"><span data-stu-id="bd4fb-105">Staging mode</span></span>
<span data-ttu-id="bd4fb-106">Az átmeneti környezetű üzemmód használható több forgatókönyvek, köztük:</span><span class="sxs-lookup"><span data-stu-id="bd4fb-106">Staging mode can be used for several scenarios, including:</span></span>

* <span data-ttu-id="bd4fb-107">Magas rendelkezésre állású.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-107">High availability.</span></span>
* <span data-ttu-id="bd4fb-108">Tesztelése és telepítése az új konfigurációs módosításokat.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-108">Test and deploy new configuration changes.</span></span>
* <span data-ttu-id="bd4fb-109">Új kiszolgáló esetében, és régi hello leszereléséhez.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-109">Introduce a new server and decommission hello old.</span></span>

<span data-ttu-id="bd4fb-110">Átmeneti módban a kiszolgálóval, módosíthatja módosítások toohello konfigurációs és preview hello előtt hello kiszolgáló aktív.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-110">With a server in staging mode, you can make changes toohello configuration and preview hello changes before you make hello server active.</span></span> <span data-ttu-id="bd4fb-111">Azt is lehetővé teszi toorun teljes importálást és teljes szinkronizálást tooverify változik, hogy minden változást várható előtt ezeket az éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-111">It also allows you toorun full import and full synchronization tooverify that all changes are expected before you make these changes into your production environment.</span></span>

<span data-ttu-id="bd4fb-112">A telepítés során kiválaszthatja a hello server toobe **átmeneti módban**.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-112">During installation, you can select hello server toobe in **staging mode**.</span></span> <span data-ttu-id="bd4fb-113">Ez a művelet kiszolgálóvá hello importálása és szinkronizálás aktív, de nem futtatható bármely exportálja.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-113">This action makes hello server active for import and synchronization, but it does not run any exports.</span></span> <span data-ttu-id="bd4fb-114">Átmeneti módban nem fut a jelszó-szinkronizálás és jelszóvisszaíró, még akkor is, ha ezek a szolgáltatások telepítésekor kiválasztott.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-114">A server in staging mode is not running password sync or password writeback, even if you selected these features during installation.</span></span> <span data-ttu-id="bd4fb-115">Az átmeneti környezetű üzemmód letiltása esetén hello server exportálása elindul, lehetővé teszi, hogy a jelszó-szinkronizálást, és lehetővé teszi, hogy a jelszóvisszaíró.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-115">When you disable staging mode, hello server starts exporting, enables password sync, and enables password writeback.</span></span>

<span data-ttu-id="bd4fb-116">Az export hello synchronization service Managert használatával továbbra is kényszeríthető.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-116">You can still force an export by using hello synchronization service manager.</span></span>

<span data-ttu-id="bd4fb-117">Átmeneti módban továbbra is fennáll, az Active Directoryból és az Azure AD tooreceive módosításokat.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-117">A server in staging mode continues tooreceive changes from Active Directory and Azure AD.</span></span> <span data-ttu-id="bd4fb-118">Keresztül hello kötelezettségeit, egy másik kiszolgáló egy másolattal hello legutóbbi módosítások és a részleg nagyon gyorsan elvégezhető mindig rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-118">It always has a copy of hello latest changes and can very fast take over hello responsibilities of another server.</span></span> <span data-ttu-id="bd4fb-119">Ha konfigurációs módosítások tooyour elsődleges kiszolgáló, a felelősség toomake hello azonos módosítások toohello kiszolgáló átmeneti módban.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-119">If you make configuration changes tooyour primary server, it is your responsibility toomake hello same changes toohello server in staging mode.</span></span>

<span data-ttu-id="bd4fb-120">A mellékelt régebbi szinkronizálási technológiák ismerete átmeneti módban hello különbözik mivel hello server csak a saját SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-120">For those of you with knowledge of older sync technologies, hello staging mode is different since hello server has its own SQL database.</span></span> <span data-ttu-id="bd4fb-121">Ez az architektúra lehetővé teszi, hogy a hello átmeneti mód server toobe ugyanabban az adatközpontban található.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-121">This architecture allows hello staging mode server toobe located in a different datacenter.</span></span>

### <a name="verify-hello-configuration-of-a-server"></a><span data-ttu-id="bd4fb-122">A kiszolgáló hello konfigurációjának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="bd4fb-122">Verify hello configuration of a server</span></span>
<span data-ttu-id="bd4fb-123">tooapply ebben az esetben kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="bd4fb-123">tooapply this method, follow these steps:</span></span>

1. [<span data-ttu-id="bd4fb-124">Előkészítése</span><span class="sxs-lookup"><span data-stu-id="bd4fb-124">Prepare</span></span>](#prepare)
2. [<span data-ttu-id="bd4fb-125">Konfigurálás</span><span class="sxs-lookup"><span data-stu-id="bd4fb-125">Configuration</span></span>](#configuration)
3. [<span data-ttu-id="bd4fb-126">Importálja és szinkronizálja</span><span class="sxs-lookup"><span data-stu-id="bd4fb-126">Import and Synchronize</span></span>](#import-and-synchronize)
4. [<span data-ttu-id="bd4fb-127">Ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="bd4fb-127">Verify</span></span>](#verify)
5. [<span data-ttu-id="bd4fb-128">Az active server kapcsoló</span><span class="sxs-lookup"><span data-stu-id="bd4fb-128">Switch active server</span></span>](#switch-active-server)

#### <a name="prepare"></a><span data-ttu-id="bd4fb-129">Előkészítés</span><span class="sxs-lookup"><span data-stu-id="bd4fb-129">Prepare</span></span>
1. <span data-ttu-id="bd4fb-130">Azure AD Connectet telepíti, válassza ki **átmeneti módban**, és kikapcsolni **a szinkronizálás megkezdéséhez** hello hello telepítési varázsló utolsó lapján.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-130">Install Azure AD Connect, select **staging mode**, and unselect **start synchronization** on hello last page in hello installation wizard.</span></span> <span data-ttu-id="bd4fb-131">Ez a mód lehetővé teszi toorun hello szinkronizálási motor manuálisan.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-131">This mode allows you toorun hello sync engine manually.</span></span>
   <span data-ttu-id="bd4fb-132">![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)</span><span class="sxs-lookup"><span data-stu-id="bd4fb-132">![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)</span></span>
2. <span data-ttu-id="bd4fb-133">Jelentkezzen ki/bejelentkezés és hello a start menüből válassza **szinkronizálási szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-133">Sign off/sign in and from hello start menu select **Synchronization Service**.</span></span>

#### <a name="configuration"></a><span data-ttu-id="bd4fb-134">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="bd4fb-134">Configuration</span></span>
<span data-ttu-id="bd4fb-135">Ha a kiszolgáló átmeneti hello végrehajtott az egyéni módosítások toohello elsődleges kiszolgáló és a kívánt toocompare hello konfigurációs használja [az Azure AD Connect konfigurációs dokumentáló](https://github.com/Microsoft/AADConnectConfigDocumenter).</span><span class="sxs-lookup"><span data-stu-id="bd4fb-135">If you have made custom changes toohello primary server and want toocompare hello configuration with hello staging server, then use [Azure AD Connect configuration documenter](https://github.com/Microsoft/AADConnectConfigDocumenter).</span></span>

#### <a name="import-and-synchronize"></a><span data-ttu-id="bd4fb-136">Importálja és szinkronizálja</span><span class="sxs-lookup"><span data-stu-id="bd4fb-136">Import and Synchronize</span></span>
1. <span data-ttu-id="bd4fb-137">Válassza ki **összekötők**, és válassza ki a hello típusú első összekötő hello **Active Directory tartományi szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-137">Select **Connectors**, and select hello first Connector with hello type **Active Directory Domain Services**.</span></span> <span data-ttu-id="bd4fb-138">Kattintson a **futtatása**, jelölje be **teljes importálás**, és **OK**.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-138">Click **Run**, select **Full import**, and **OK**.</span></span> <span data-ttu-id="bd4fb-139">Hajtsa végre ezeket a lépéseket az összes ilyen típusú-összekötőhöz.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-139">Do these steps for all Connectors of this type.</span></span>
2. <span data-ttu-id="bd4fb-140">Válassza ki a Connector típusú hello **Azure Active Directoryban (Microsoft)**.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-140">Select hello Connector with type **Azure Active Directory (Microsoft)**.</span></span> <span data-ttu-id="bd4fb-141">Kattintson a **futtatása**, jelölje be **teljes importálás**, és **OK**.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-141">Click **Run**, select **Full import**, and **OK**.</span></span>
3. <span data-ttu-id="bd4fb-142">Győződjön meg arról, hogy hello lapon összekötők aktív marad.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-142">Make sure hello tab Connectors is still selected.</span></span> <span data-ttu-id="bd4fb-143">Minden típusú összekötő **Active Directory tartományi szolgáltatások**, kattintson a **futtatása**, jelölje be **különbözeti szinkronizálás**, és **OK**.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-143">For each Connector with type **Active Directory Domain Services**, click **Run**, select **Delta Synchronization**, and **OK**.</span></span>
4. <span data-ttu-id="bd4fb-144">Válassza ki a Connector típusú hello **Azure Active Directoryban (Microsoft)**.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-144">Select hello Connector with type **Azure Active Directory (Microsoft)**.</span></span> <span data-ttu-id="bd4fb-145">Kattintson a **futtatása**, jelölje be **különbözeti szinkronizálás**, és **OK**.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-145">Click **Run**, select **Delta Synchronization**, and **OK**.</span></span>

<span data-ttu-id="bd4fb-146">Lehetősége van most előkészített exportálás tooAzure AD módosításai és a helyszíni AD (ha az Exchange hibrid telepítést használ).</span><span class="sxs-lookup"><span data-stu-id="bd4fb-146">You have now staged export changes tooAzure AD and on-premises AD (if you are using Exchange hybrid deployment).</span></span> <span data-ttu-id="bd4fb-147">hello lépések lehetővé teszik a tooinspect Újdonságok kapcsolatos toochange toohello könyvtárak hello exportálása ténylegesen megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-147">hello next steps allow you tooinspect what is about toochange before you actually start hello export toohello directories.</span></span>

#### <a name="verify"></a><span data-ttu-id="bd4fb-148">Ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="bd4fb-148">Verify</span></span>
1. <span data-ttu-id="bd4fb-149">Indítsa el egy parancssort, és nyissa meg túl`%ProgramFiles%\Microsoft Azure AD Sync\bin`</span><span class="sxs-lookup"><span data-stu-id="bd4fb-149">Start a cmd prompt and go too`%ProgramFiles%\Microsoft Azure AD Sync\bin`</span></span>
2. <span data-ttu-id="bd4fb-150">Futtatás: `csexport "Name of Connector" %temp%\export.xml /f:x` hello összekötő hello neve található szinkronizálási szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-150">Run: `csexport "Name of Connector" %temp%\export.xml /f:x` hello name of hello Connector can be found in Synchronization Service.</span></span> <span data-ttu-id="bd4fb-151">Rendelkezik egy neve hasonló too"contoso.com – AAD" az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-151">It has a name similar too"contoso.com – AAD" for Azure AD.</span></span>
3. <span data-ttu-id="bd4fb-152">Hello PowerShell parancsfájl másolása hello szakasz [CSAnalyzer](#appendix-csanalyzer) tooa fájlt `csanalyzer.ps1`.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-152">Copy hello PowerShell script from hello section [CSAnalyzer](#appendix-csanalyzer) tooa file named `csanalyzer.ps1`.</span></span>
4. <span data-ttu-id="bd4fb-153">Nyisson meg egy PowerShell-ablakot, és keresse meg a toohello mappa, amelyben létrehozta hello PowerShell-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-153">Open a PowerShell window and browse toohello folder where you created hello PowerShell script.</span></span>
5. <span data-ttu-id="bd4fb-154">Futtatás: `.\csanalyzer.ps1 -xmltoimport %temp%\export.xml`.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-154">Run: `.\csanalyzer.ps1 -xmltoimport %temp%\export.xml`.</span></span>
6. <span data-ttu-id="bd4fb-155">Most már rendelkezik egy fájlt **processedusers1.csv** , amely a Microsoft Excel kell vizsgálni.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-155">You now have a file named **processedusers1.csv** that can be examined in Microsoft Excel.</span></span> <span data-ttu-id="bd4fb-156">Minden előkészített módosítások exportált toobe tooAzure AD ebben a fájlban találhatók.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-156">All changes staged toobe exported tooAzure AD are found in this file.</span></span>
7. <span data-ttu-id="bd4fb-157">A szükséges módosításokat toohello adatok vagy konfiguráció, és ezeket a lépéseket újra (importálás és szinkronizálás és ellenőrzése) futását a változtatásokat, hogy exportált toobe kapcsolatos várható hello.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-157">Make necessary changes toohello data or configuration and run these steps again (Import and Synchronize and Verify) until hello changes that are about toobe exported are expected.</span></span>

#### <a name="switch-active-server"></a><span data-ttu-id="bd4fb-158">Az active server kapcsoló</span><span class="sxs-lookup"><span data-stu-id="bd4fb-158">Switch active server</span></span>
1. <span data-ttu-id="bd4fb-159">A jelenleg aktív kiszolgálói hello vagy kapcsolja ki a hello server (DirSync vagy FIM vagy az Azure AD Sync), így azt nem exportálnia kell tooAzure AD, vagy állítsa az átmeneti módban (az Azure AD Connect).</span><span class="sxs-lookup"><span data-stu-id="bd4fb-159">On hello currently active server, either turn off hello server (DirSync/FIM/Azure AD Sync) so it is not exporting tooAzure AD or set it in staging mode (Azure AD Connect).</span></span>
2. <span data-ttu-id="bd4fb-160">Hello telepítővarázslójának futtatása a hello kiszolgálón **átmeneti módban** és tiltsa le a **átmeneti módban**.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-160">Run hello installation wizard on hello server in **staging mode** and disable **staging mode**.</span></span>
   <span data-ttu-id="bd4fb-161">![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)</span><span class="sxs-lookup"><span data-stu-id="bd4fb-161">![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)</span></span>

## <a name="disaster-recovery"></a><span data-ttu-id="bd4fb-162">Vészhelyreállítás</span><span class="sxs-lookup"><span data-stu-id="bd4fb-162">Disaster recovery</span></span>
<span data-ttu-id="bd4fb-163">Hello megvalósítási terv része a milyen toodo tooplan abban az esetben, ha van egy olyan vészhelyzet esetén hol elvesznek a hello szinkronizálási kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-163">Part of hello implementation design is tooplan for what toodo in case there is a disaster where you lose hello sync server.</span></span> <span data-ttu-id="bd4fb-164">Nincsenek különböző modell toouse és mely egy toouse többek között számos tényezőtől függ:</span><span class="sxs-lookup"><span data-stu-id="bd4fb-164">There are different models toouse and which one toouse depends on several factors including:</span></span>

* <span data-ttu-id="bd4fb-165">Mi az a tűrés, azzal nem tud ellenőrizze tooobjects hello leállások során az Azure AD-ben?</span><span class="sxs-lookup"><span data-stu-id="bd4fb-165">What is your tolerance for not being able make changes tooobjects in Azure AD during hello downtime?</span></span>
* <span data-ttu-id="bd4fb-166">A jelszó-szinkronizálás használatakor tegye hello felhasználók fogadja el, hogy rendelkeznek toouse hello régi jelszó az Azure AD abban az esetben, ha azok megváltoztatnia a helyszínen?</span><span class="sxs-lookup"><span data-stu-id="bd4fb-166">If you use password synchronization, do hello users accept that they have toouse hello old password in Azure AD in case they change it on-premises?</span></span>
* <span data-ttu-id="bd4fb-167">Rendelkezik a valós idejű műveletek, például a jelszóvisszaírás függőség?</span><span class="sxs-lookup"><span data-stu-id="bd4fb-167">Do you have a dependency on real-time operations, such as password writeback?</span></span>

<span data-ttu-id="bd4fb-168">Attól függően, hogy hello válaszok toothese kérdések és a szervezet házirendje hello következő stratégiák egyikét is kell végrehajtani:</span><span class="sxs-lookup"><span data-stu-id="bd4fb-168">Depending on hello answers toothese questions and your organization’s policy, one of hello following strategies can be implemented:</span></span>

* <span data-ttu-id="bd4fb-169">Építse újra, amikor szükséges.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-169">Rebuild when needed.</span></span>
* <span data-ttu-id="bd4fb-170">Tartalék készenléti kiszolgáló, úgynevezett **átmeneti módban**.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-170">Have a spare standby server, known as **staging mode**.</span></span>
* <span data-ttu-id="bd4fb-171">Használja a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-171">Use virtual machines.</span></span>

<span data-ttu-id="bd4fb-172">Ha nem használja a hello beépített SQL Express adatbázist, majd tekintse meg hello [SQL magas rendelkezésre állású](#sql-high-availability) szakasz.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-172">If you do not use hello built-in SQL Express database, then you should also review hello [SQL High Availability](#sql-high-availability) section.</span></span>

### <a name="rebuild-when-needed"></a><span data-ttu-id="bd4fb-173">Szükség esetén újraépítése</span><span class="sxs-lookup"><span data-stu-id="bd4fb-173">Rebuild when needed</span></span>
<span data-ttu-id="bd4fb-174">Életképes stratégia tooplan egy kiszolgáló újjáépítése szükség esetén a rendszer.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-174">A viable strategy is tooplan for a server rebuild when needed.</span></span> <span data-ttu-id="bd4fb-175">Általában telepítése szinkronizálási motor hello és hello kezdeti importálása és szinkronizálás néhány órán belül végrehajtható.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-175">Usually, installing hello sync engine and do hello initial import and sync can be completed within a few hours.</span></span> <span data-ttu-id="bd4fb-176">Tartalék kiszolgáló nem érhető el, ha egy tartomány tartományvezérlői toohost hello szinkronizálási motor lehetséges tootemporarily használatát is.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-176">If there isn’t a spare server available, it is possible tootemporarily use a domain controller toohost hello sync engine.</span></span>

<span data-ttu-id="bd4fb-177">hello a Szinkronizáló vezérlő kiszolgálója nem tárol hello objektumokról olyan állapotokat, hello adatbázis is építhető újra, az Active Directory és az Azure AD hello adataiból.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-177">hello sync engine server does not store any state about hello objects so hello database can be rebuilt from hello data in Active Directory and Azure AD.</span></span> <span data-ttu-id="bd4fb-178">Hello **sourceAnchor** attribútum használt toojoin hello objektumokat a helyszíni és felhőalapú hello.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-178">hello **sourceAnchor** attribute is used toojoin hello objects from on-premises and hello cloud.</span></span> <span data-ttu-id="bd4fb-179">Újraépítése hello kiszolgáló meglévő objektumokat a helyszíni és hello felhő, majd hello tényleges szinkronizálási motor megfelel azokat az objektumokat újra újratelepítés együtt.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-179">If you rebuild hello server with existing objects on-premises and hello cloud, then hello sync engine matches those objects together again on reinstallation.</span></span> <span data-ttu-id="bd4fb-180">hello toodocument kell, és mentse a dolog hello konfigurációs módosítások toohello kiszolgálót, például szűrési és szinkronizálási szabályok.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-180">hello things you need toodocument and save are hello configuration changes made toohello server, such as filtering and synchronization rules.</span></span> <span data-ttu-id="bd4fb-181">Ezen egyéni konfigurációk kell kell újra alkalmazza a szinkronizálás megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-181">These custom configurations must be reapplied before you start synchronizing.</span></span>

### <a name="have-a-spare-standby-server---staging-mode"></a><span data-ttu-id="bd4fb-182">Tartalék készenléti kiszolgáló - átmeneti módban</span><span class="sxs-lookup"><span data-stu-id="bd4fb-182">Have a spare standby server - staging mode</span></span>
<span data-ttu-id="bd4fb-183">Ha összetettebb környezetben, akkor ajánlott, ha egy vagy több készenléti kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-183">If you have a more complex environment, then having one or more standby servers is recommended.</span></span> <span data-ttu-id="bd4fb-184">A telepítés során engedélyezheti a kiszolgáló toobe a **átmeneti módban**.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-184">During installation, you can enable a server toobe in **staging mode**.</span></span>

<span data-ttu-id="bd4fb-185">További információkért lásd: [átmeneti módban](#staging-mode).</span><span class="sxs-lookup"><span data-stu-id="bd4fb-185">For more information, see [staging mode](#staging-mode).</span></span>

### <a name="use-virtual-machines"></a><span data-ttu-id="bd4fb-186">Virtuális gépek használata</span><span class="sxs-lookup"><span data-stu-id="bd4fb-186">Use virtual machines</span></span>
<span data-ttu-id="bd4fb-187">A közös és támogatott metódus toorun hello szinkronizálási motor egy virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-187">A common and supported method is toorun hello sync engine in a virtual machine.</span></span> <span data-ttu-id="bd4fb-188">Abban az esetben hello állomás egy hibát tartalmaz, hello a Szinkronizáló vezérlő kiszolgálója hello rendszerképének áttelepített tooanother kiszolgáló lehet.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-188">In case hello host has an issue, hello image with hello sync engine server can be migrated tooanother server.</span></span>

### <a name="sql-high-availability"></a><span data-ttu-id="bd4fb-189">Az SQL magas rendelkezésre állás</span><span class="sxs-lookup"><span data-stu-id="bd4fb-189">SQL High Availability</span></span>
<span data-ttu-id="bd4fb-190">Ha nem használ SQL Server Express, az Azure AD Connect előre hello, majd az SQL Server magas rendelkezésre állás is meg kell fontolni.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-190">If you are not using hello SQL Server Express that comes with Azure AD Connect, then high availability for SQL Server should also be considered.</span></span> <span data-ttu-id="bd4fb-191">támogatott hello magas rendelkezésre állás megoldásai a következők: SQL-fürtözés és AOA (az Always On rendelkezésre állási csoportok).</span><span class="sxs-lookup"><span data-stu-id="bd4fb-191">hello high availability solutions supported include SQL clustering and AOA (Always On Availability Groups).</span></span> <span data-ttu-id="bd4fb-192">Nem támogatott megoldásai a következők tükrözés.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-192">Unsupported solutions include mirroring.</span></span>

<span data-ttu-id="bd4fb-193">Az SQL AOA támogatást kaptak tooAzure AD Connect 1.1.524.0 verzióban.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-193">Support for SQL AOA was added tooAzure AD Connect in version 1.1.524.0.</span></span> <span data-ttu-id="bd4fb-194">Az Azure AD Connect telepítése előtt engedélyeznie kell az SQL AOA.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-194">You must enable SQL AOA before installing Azure AD Connect.</span></span> <span data-ttu-id="bd4fb-195">A telepítés során az Azure AD Connect észleli a megadott hello SQL-példány engedélyezve van az SQL AOA-e nem.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-195">During installation, Azure AD Connect detects whether hello SQL instance provided is enabled for SQL AOA or not.</span></span> <span data-ttu-id="bd4fb-196">Ha SQL AOA engedélyezve van, az Azure AD Connect további adatok Ha SQL AOA-e a konfigurált toouse szinkron replikáció vagy aszinkron replikáció.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-196">If SQL AOA is enabled, Azure AD Connect further figures out if SQL AOA is configured toouse synchronous replication or asynchronous replication.</span></span> <span data-ttu-id="bd4fb-197">Hello rendelkezésre állási csoport figyelőjének beállításakor javasoljuk, hogy állítsa a hello RegisterAllProvidersIP tulajdonság too0.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-197">When setting up hello Availability Group Listener, it is recommended that you set hello RegisterAllProvidersIP property too0.</span></span> <span data-ttu-id="bd4fb-198">Ennek az az oka az Azure AD Connect jelenleg használ SQL Native Client tooconnect tooSQL és az SQL natív ügyfél nem támogatja a MultiSubNetFailover tulajdonság hello használatával.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-198">This is because Azure AD Connect currently uses SQL Native Client tooconnect tooSQL and SQL Native Client does not support hello use of MultiSubNetFailover property.</span></span>

## <a name="appendix-csanalyzer"></a><span data-ttu-id="bd4fb-199">A függelék CSAnalyzer</span><span class="sxs-lookup"><span data-stu-id="bd4fb-199">Appendix CSAnalyzer</span></span>
<span data-ttu-id="bd4fb-200">Című rész hello [ellenőrizze](#verify) hogyan toouse ezt a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="bd4fb-200">See hello section [verify](#verify) on how toouse this script.</span></span>

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

#XmlReader.Create won't properly resolve hello file location,
#so expand and then resolve it
$resolvedXMLtoimport=Resolve-Path -Path ([Environment]::ExpandEnvironmentVariables($xmltoimport))

#use an XmlReader toodeal with even large files
$result=$reader = [System.Xml.XmlReader]::Create($resolvedXMLtoimport) 
$result=$reader.ReadToDescendant('cs-object')
do 
{
    #create hello object placeholder
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

    #now that we have hello basics, go get hello details

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

    #every so often, dump hello processed users in case we blow up somewhere
    if ($count % $batchsize -eq 0)
    {
        Write-Host Hit hello maximum users processed without completion... -ForegroundColor Yellow

        #export hello collection of users as as CSV
        Write-Host Writing processedusers${outputfilecount}.csv -ForegroundColor Yellow
        $objOutputUsers | Export-Csv -path processedusers${outputfilecount}.csv -NoTypeInformation

        #increment hello output file counter
        $outputfilecount+=1

        #reset hello collection and hello user counter
        $objOutputUsers = $null
        $count=0
    }

    $count+=1

    #need toobail out of hello loop if no more users tooprocess
    if ($reader.NodeType -eq [System.Xml.XmlNodeType]::EndElement)
    {
        break
    }

} while ($reader.Read)

#need toowrite out any users that didn't get picked up in a batch of 1000
#export hello collection of users as as CSV
Write-Host Writing processedusers${outputfilecount}.csv -ForegroundColor Yellow
$objOutputUsers | Export-Csv -path processedusers${outputfilecount}.csv -NoTypeInformation
```

## <a name="next-steps"></a><span data-ttu-id="bd4fb-201">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bd4fb-201">Next steps</span></span>
<span data-ttu-id="bd4fb-202">**Áttekintő témakör**</span><span class="sxs-lookup"><span data-stu-id="bd4fb-202">**Overview topics**</span></span>  

* [<span data-ttu-id="bd4fb-203">Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása</span><span class="sxs-lookup"><span data-stu-id="bd4fb-203">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)  
* [<span data-ttu-id="bd4fb-204">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="bd4fb-204">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)  
