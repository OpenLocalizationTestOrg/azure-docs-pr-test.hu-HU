---
title: "aaaCustomize HDInsight-fürtök Parancsfájlműveletek - Azure használatával |} Microsoft Docs"
description: "Adja hozzá a tooLinux-alapú HDInsight-fürtök Parancsfájlműveletek segítségével egyéni összetevőket. A Parancsfájlműveletek olyan Bash parancsfájlok használt toocustomize hello fürtkonfiguráció vagy adja hozzá a további szolgáltatások és segédprogramok Hue, Solr vagy R."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 48e85f53-87c1-474f-b767-ca772238cc13
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: larryfr
ms.openlocfilehash: ff22680a8a50b21985f6941f1edaf1dcf863d13f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customize-linux-based-hdinsight-clusters-using-script-action"></a><span data-ttu-id="75495-104">Parancsfájlművelet Linux-alapú HDInsight-fürtök testreszabása</span><span class="sxs-lookup"><span data-stu-id="75495-104">Customize Linux-based HDInsight clusters using Script Action</span></span>

<span data-ttu-id="75495-105">A HDInsight lehetővé nevű konfigurációs beállítás **parancsfájlművelet** , amely elindítja az egyéni parancsfájlok, amelyek hello fürt testreszabása.</span><span class="sxs-lookup"><span data-stu-id="75495-105">HDInsight provides a configuration option called **Script Action** that invokes custom scripts that customize hello cluster.</span></span> <span data-ttu-id="75495-106">Ezek a parancsfájlok használt tooinstall további összetevőket, és konfigurációs beállításokat módosítaná.</span><span class="sxs-lookup"><span data-stu-id="75495-106">These scripts are used tooinstall additional components and change configuration settings.</span></span> <span data-ttu-id="75495-107">A Parancsfájlműveletek használható során vagy a fürt létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="75495-107">Script actions can be used during or after cluster creation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="75495-108">Linux-alapú HDInsight-fürtök hello képességét toouse Parancsfájlműveletek egy már futó fürt csak érhető el.</span><span class="sxs-lookup"><span data-stu-id="75495-108">hello ability toouse script actions on an already running cluster is only available for Linux-based HDInsight clusters.</span></span>
>
> <span data-ttu-id="75495-109">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="75495-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="75495-110">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="75495-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="75495-111">Parancsfájlműveletek HDInsight alkalmazásként is közzétett toohello Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="75495-111">Script actions can also be published toohello Azure Marketplace as an HDInsight application.</span></span> <span data-ttu-id="75495-112">Ebben a dokumentumban hello példák némelyike bemutatják, hogyan telepítse a PowerShell és a .NET SDK hello parancsfájl művelet parancsokkal HDInsight-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="75495-112">Some of hello examples in this document show how you can install an HDInsight application using script action commands from PowerShell and hello .NET SDK.</span></span> <span data-ttu-id="75495-113">További információk a HDInsight-alkalmazások: [hello Azure piactér közzé HDInsight-alkalmazások](hdinsight-apps-publish-applications.md).</span><span class="sxs-lookup"><span data-stu-id="75495-113">For more information on HDInsight applications, see [Publish HDInsight applications into hello Azure Marketplace](hdinsight-apps-publish-applications.md).</span></span>

## <a name="permissions"></a><span data-ttu-id="75495-114">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="75495-114">Permissions</span></span>

<span data-ttu-id="75495-115">Ha egy tartományhoz csatlakozó HDInsight-fürtöt használ, nincsenek két Ambari szükséges engedélyek hello fürt integrált parancsfájl-műveletek használata esetén:</span><span class="sxs-lookup"><span data-stu-id="75495-115">If you are using a domain-joined HDInsight cluster, there are two Ambari permissions that are required when using script actions with hello cluster:</span></span>

* <span data-ttu-id="75495-116">**AMBARI. Futtatás\_egyéni\_parancs**: hello Ambari rendszergazdai szerepkör rendelkezik ezzel az engedéllyel, alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="75495-116">**AMBARI.RUN\_CUSTOM\_COMMAND**: hello Ambari Administrator role has this permission by default.</span></span>
* <span data-ttu-id="75495-117">**A FÜRT. Futtatás\_egyéni\_parancs**: mindkét hello HDInsight fürt rendszergazdája és Ambari rendszergazda alapértelmezés szerint rendelkezik ilyen engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="75495-117">**CLUSTER.RUN\_CUSTOM\_COMMAND**: Both hello HDInsight Cluster Administrator and Ambari Administrator have this permission by default.</span></span>

<span data-ttu-id="75495-118">Tartományhoz csatlakozó HDInsight engedélyeket munkáról bővebben lásd: [tartományhoz HDInsight-fürtök kezelése](hdinsight-domain-joined-manage.md).</span><span class="sxs-lookup"><span data-stu-id="75495-118">For more information on working with permissions with domain-joined HDInsight, see [Manage domain-joined HDInsight clusters](hdinsight-domain-joined-manage.md).</span></span>

## <a name="access-control"></a><span data-ttu-id="75495-119">Hozzáférés-vezérlés</span><span class="sxs-lookup"><span data-stu-id="75495-119">Access control</span></span>

<span data-ttu-id="75495-120">Ha nem hello az Azure-előfizetéshez, rendszergazdai vagy tulajdonosa, a fióknak rendelkeznie kell legalább **közreműködő** hozzáférés toohello tartalmazó erőforráscsoportot hello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="75495-120">If you are not hello administrator/owner of your Azure subscription, your account must have at least **Contributor** access toohello resource group that contains hello HDInsight cluster.</span></span>

<span data-ttu-id="75495-121">Emellett ha hoz létre HDInsight-fürtöt, valaki legalább **közreműködő** hozzáférés toohello Azure-előfizetéssel kell már korábban regisztrált HDInsight hello szolgáltatója.</span><span class="sxs-lookup"><span data-stu-id="75495-121">Additionally, if you are creating an HDInsight cluster, someone with at least **Contributor** access toohello Azure subscription must have previously registered hello provider for HDInsight.</span></span> <span data-ttu-id="75495-122">A szolgáltató regisztrációja történik, ha egy felhasználó közreműködői hozzáférés toohello előfizetéssel hoz létre hello erőforrás először hello az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="75495-122">Provider registration happens when a user with Contributor access toohello subscription creates a resource for hello first time on hello subscription.</span></span> <span data-ttu-id="75495-123">Az erőforrás létrehozása nélkül is elvégezhető, ha [ a REST segítségével regisztrál egy szolgáltatót](https://msdn.microsoft.com/library/azure/dn790548.aspx).</span><span class="sxs-lookup"><span data-stu-id="75495-123">It can also be accomplished without creating a resource by [registering a provider using REST](https://msdn.microsoft.com/library/azure/dn790548.aspx).</span></span>

<span data-ttu-id="75495-124">A access management munkáról bővebben lásd a következő dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="75495-124">For more information on working with access management, see hello following documents:</span></span>

* [<span data-ttu-id="75495-125">Hozzáférés-kezelés hello Azure-portálon az első lépések</span><span class="sxs-lookup"><span data-stu-id="75495-125">Get started with access management in hello Azure portal</span></span>](../active-directory/role-based-access-control-what-is.md)
* [<span data-ttu-id="75495-126">Szerepkör hozzárendelések toomanage hozzáférés tooyour Azure-előfizetés erőforrásainak használatához</span><span class="sxs-lookup"><span data-stu-id="75495-126">Use role assignments toomanage access tooyour Azure subscription resources</span></span>](../active-directory/role-based-access-control-configure.md)

## <a name="understanding-script-actions"></a><span data-ttu-id="75495-127">A Parancsfájlműveletek ismertetése</span><span class="sxs-lookup"><span data-stu-id="75495-127">Understanding Script Actions</span></span>

<span data-ttu-id="75495-128">A parancsfájlművelet egyszerűen egy Bash parancsfájlok mutató URI-t biztosító, de paramétereit.</span><span class="sxs-lookup"><span data-stu-id="75495-128">A Script Action is simply a Bash script that you provide a URI to, and parameters for.</span></span> <span data-ttu-id="75495-129">hello parancsfájl hello HDInsight-fürt csomópontján fut.</span><span class="sxs-lookup"><span data-stu-id="75495-129">hello script runs on nodes in hello HDInsight cluster.</span></span> <span data-ttu-id="75495-130">hello a következők jellemzők és Parancsfájlműveletek funkcióit.</span><span class="sxs-lookup"><span data-stu-id="75495-130">hello following are characteristics and features of script actions.</span></span>

* <span data-ttu-id="75495-131">URI, amely a HDInsight-fürt hello elérhető kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="75495-131">Must be stored on a URI that is accessible from hello HDInsight cluster.</span></span> <span data-ttu-id="75495-132">Az alábbiakban hello lehetséges tárolókba:</span><span class="sxs-lookup"><span data-stu-id="75495-132">hello following are possible storage locations:</span></span>

    * <span data-ttu-id="75495-133">Egy **Azure Data Lake Store** hello HDInsight-fürt által elérhető fiók.</span><span class="sxs-lookup"><span data-stu-id="75495-133">An **Azure Data Lake Store** account that is accessible by hello HDInsight cluster.</span></span> <span data-ttu-id="75495-134">Azure Data Lake Store és a HDInsight együttes használatáról további információért lásd: [HDInsight-fürtök létrehozása a Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="75495-134">For information on using Azure Data Lake Store with HDInsight, see [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

        <span data-ttu-id="75495-135">A Data Lake Store-ban tárolt parancsfájl használata esetén hello URI-formátum van `adl://DATALAKESTOREACCOUNTNAME.azuredatalakestore.net/path_to_file`.</span><span class="sxs-lookup"><span data-stu-id="75495-135">When using a script stored in Data Lake Store, hello URI format is `adl://DATALAKESTOREACCOUNTNAME.azuredatalakestore.net/path_to_file`.</span></span>

        > [!NOTE]
        > <span data-ttu-id="75495-136">hello szolgáltatás egyszerű HDInsight használ tooaccess Data Lake Store toohello parancsfájl olvasási hozzáféréssel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="75495-136">hello service principal HDInsight uses tooaccess Data Lake Store must have read access toohello script.</span></span>

    * <span data-ttu-id="75495-137">A blob egy **Azure Storage-fiók** , hogy-e vagy hello elsődleges vagy további tárfiók hello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="75495-137">A blob in an **Azure Storage account** that is either hello primary or additional storage account for hello HDInsight cluster.</span></span> <span data-ttu-id="75495-138">HDInsight kapnak hozzáférést tooboth az ilyen típusú tárfiókok a fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="75495-138">HDInsight is granted access tooboth of these types of storage accounts during cluster creation.</span></span>

    * <span data-ttu-id="75495-139">Egy nyilvános fájlmegosztási szolgáltatással, például az Azure Blob, a GitHub, a onedrive vállalati verzió, a Dropbox, a stb.</span><span class="sxs-lookup"><span data-stu-id="75495-139">A public file sharing service such as Azure Blob, GitHub, OneDrive, Dropbox, etc.</span></span>

        <span data-ttu-id="75495-140">Például URI, lásd: hello [parancsfájl művelet parancsfájlpéldákat](#example-script-action-scripts) szakasz.</span><span class="sxs-lookup"><span data-stu-id="75495-140">For example URIs, see hello [Example script action scripts](#example-script-action-scripts) section.</span></span>

        > [!WARNING]
        > <span data-ttu-id="75495-141">Csak a HDInsight támogatja __általános célú__ Azure Storage-fiókokat.</span><span class="sxs-lookup"><span data-stu-id="75495-141">HDInsight only supports __General-purpose__ Azure Storage accounts.</span></span> <span data-ttu-id="75495-142">Jelenleg nem támogatja hello __Blob-tároló__ fiók típusa.</span><span class="sxs-lookup"><span data-stu-id="75495-142">It does not currently support hello __Blob storage__ account type.</span></span>

* <span data-ttu-id="75495-143">Túl korlátozható**futhat az egyes csomóponttípusok**, például átjárócsomópontokkal vagy munkavégző csomópontokhoz.</span><span class="sxs-lookup"><span data-stu-id="75495-143">Can be restricted too**run on only certain node types**, for example head nodes or worker nodes.</span></span>

  > [!NOTE]
  > <span data-ttu-id="75495-144">HDInsight prémium használata esetén adja meg, hogy hello parancsfájl hello élcsomópont kell használni.</span><span class="sxs-lookup"><span data-stu-id="75495-144">When used with HDInsight Premium, you can specify that hello script should be used on hello edge node.</span></span>

* <span data-ttu-id="75495-145">Lehet **megőrzött** vagy **alkalmi**.</span><span class="sxs-lookup"><span data-stu-id="75495-145">Can be **persisted** or **ad hoc**.</span></span>

    <span data-ttu-id="75495-146">**Megőrzött** parancsfájlok fürtöket alkalmazott tooworker csomópontok hozzáadott toohello hello parancsfájl futtatása után.</span><span class="sxs-lookup"><span data-stu-id="75495-146">**Persisted** scripts are applied tooworker nodes added toohello cluster after hello script runs.</span></span> <span data-ttu-id="75495-147">Például, amikor hello fürt vertikális felskálázásával.</span><span class="sxs-lookup"><span data-stu-id="75495-147">For example, when scaling up hello cluster.</span></span>

    <span data-ttu-id="75495-148">Egy megőrzött parancsfájl is előfordulhat, hogy alkalmazza a módosításokat tooanother csomóponttípus, például egy átjárócsomóponttal.</span><span class="sxs-lookup"><span data-stu-id="75495-148">A persisted script might also apply changes tooanother node type, such as a head node.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="75495-149">A megőrzött Parancsfájlműveletek egy egyedi névvel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="75495-149">Persisted script actions must have a unique name.</span></span>

    <span data-ttu-id="75495-150">**Az ad hoc** parancsfájlok nem maradnak meg.</span><span class="sxs-lookup"><span data-stu-id="75495-150">**Ad hoc** scripts are not persisted.</span></span> <span data-ttu-id="75495-151">Nincsenek alkalmazott tooworker csomópontok hozzáadott toohello fürt után hello parancsfájl lefutott rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="75495-151">They are not applied tooworker nodes added toohello cluster after hello script has ran.</span></span> <span data-ttu-id="75495-152">Ezt követően előléptetheti az alkalmi parancsfájl tooa megőrzött parancsfájl vagy egy megőrzött parancsfájl tooan alkalmi parancsfájl fokozni.</span><span class="sxs-lookup"><span data-stu-id="75495-152">You can subsequently promote an ad hoc script tooa persisted script, or demote a persisted script tooan ad hoc script.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="75495-153">Fürt létrehozása során használt Parancsfájlműveletek automatikusan megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="75495-153">Script actions used during cluster creation are automatically persisted.</span></span>
  >
  > <span data-ttu-id="75495-154">Parancsfájlok, amelyek nem sikertelen tárolt, még akkor is, ha kifejezetten Megadja, hogy azok kell lennie.</span><span class="sxs-lookup"><span data-stu-id="75495-154">Scripts that fail are not persisted, even if you specifically indicate that they should be.</span></span>

* <span data-ttu-id="75495-155">Fogadhat **paraméterek** használt hello parancsfájl végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="75495-155">Can accept **parameters** that are used by hello script during execution.</span></span>
* <span data-ttu-id="75495-156">Futtassa a **gyökér szintű jogosultságokkal** hello fürtcsomópontokon.</span><span class="sxs-lookup"><span data-stu-id="75495-156">Run with **root level privileges** on hello cluster nodes.</span></span>
* <span data-ttu-id="75495-157">Hello keresztül használható **Azure-portálon**, **Azure PowerShell**, **Azure CLI**, vagy **HDInsight .NET SDK**</span><span class="sxs-lookup"><span data-stu-id="75495-157">Can be used through hello **Azure portal**, **Azure PowerShell**, **Azure CLI**, or **HDInsight .NET SDK**</span></span>

<span data-ttu-id="75495-158">hello fürt megtart lett futtatott összes parancsfájl előzményeit.</span><span class="sxs-lookup"><span data-stu-id="75495-158">hello cluster keeps a history of all scripts that have been ran.</span></span> <span data-ttu-id="75495-159">hello előzmények akkor hasznos, ha egy parancsfájl toofind hello azonosítója a(z) előléptetés vagy a lefokozás műveletek van szüksége.</span><span class="sxs-lookup"><span data-stu-id="75495-159">hello history is useful when you need toofind hello ID of a script for promotion or demotion operations.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="75495-160">Nincs nincs automatikus módon tooundo hello egy parancsfájl műveletet hajtott végre.</span><span class="sxs-lookup"><span data-stu-id="75495-160">There is no automatic way tooundo hello changes made by a script action.</span></span> <span data-ttu-id="75495-161">Manuálisan fordított hello módosításokat, vagy adjon meg egy parancsfájlt, amely visszavonja őket.</span><span class="sxs-lookup"><span data-stu-id="75495-161">Either manually reverse hello changes or provide a script that reverses them.</span></span>


### <a name="script-action-in-hello-cluster-creation-process"></a><span data-ttu-id="75495-162">Hello fürtlétrehozás parancsfájlművelet</span><span class="sxs-lookup"><span data-stu-id="75495-162">Script Action in hello cluster creation process</span></span>

<span data-ttu-id="75495-163">Fürt létrehozása során használt Parancsfájlműveletek némileg eltérnek a műveletek egy meglévő fürthöz futtatott parancsfájl:</span><span class="sxs-lookup"><span data-stu-id="75495-163">Script Actions used during cluster creation are slightly different from script actions ran on an existing cluster:</span></span>

* <span data-ttu-id="75495-164">hello parancsfájl **automatikusan megőrzött**.</span><span class="sxs-lookup"><span data-stu-id="75495-164">hello script is **automatically persisted**.</span></span>
* <span data-ttu-id="75495-165">A **hiba** hello parancsfájl hello fürt létrehozási folyamat toofail azt okozhatja.</span><span class="sxs-lookup"><span data-stu-id="75495-165">A **failure** in hello script can cause hello cluster creation process toofail.</span></span>

<span data-ttu-id="75495-166">hello alábbi ábrán látható parancsfájl hello létrehozási folyamata során kerül végrehajtásra:</span><span class="sxs-lookup"><span data-stu-id="75495-166">hello following diagram illustrates when Script Action is executed during hello creation process:</span></span>

<span data-ttu-id="75495-167">![A HDInsight fürt testreszabási és fürt létrehozása során szakaszai][img-hdi-cluster-states]</span><span class="sxs-lookup"><span data-stu-id="75495-167">![HDInsight cluster customization and stages during cluster creation][img-hdi-cluster-states]</span></span>

<span data-ttu-id="75495-168">hello parancsfájl fut, amíg a HDInsight konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="75495-168">hello script runs while HDInsight is being configured.</span></span> <span data-ttu-id="75495-169">Ebben a szakaszban hello parancsfájl futtatása párhuzamosan összes hello hello fürt és a legfelső szintű jogosultságokkal fut hello csomóponton megadott csomópontok.</span><span class="sxs-lookup"><span data-stu-id="75495-169">At this stage, hello script runs in parallel on all hello specified nodes in hello cluster, and runs with root privileges on hello nodes.</span></span>

> [!NOTE]
> <span data-ttu-id="75495-170">Mivel hello parancsfájl fut gyökér szintű jogosultsággal rendelkező hello fürtcsomópontokon, például a szolgáltatások, beleértve a Hadoop-kapcsolatos szolgáltatások indítása és leállítása műveleteket hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="75495-170">Because hello script runs with root level privilege on hello cluster nodes, you can perform operations like stopping and starting services, including Hadoop-related services.</span></span> <span data-ttu-id="75495-171">Ha leállítja a szolgáltatások, bizonyosodjon meg, hogy hello Ambari szolgáltatás és más Hadoop kapcsolatos szolgáltatások működik, és mielőtt hello parancsfájl befejezése után történik.</span><span class="sxs-lookup"><span data-stu-id="75495-171">If you stop services, you must ensure that hello Ambari service and other Hadoop-related services are up and running before hello script finishes running.</span></span> <span data-ttu-id="75495-172">Ezek a szolgáltatások szükségesek toosuccessfully hello állapotát és hello fürt állapotának meghatározásához azt létrehozása közben.</span><span class="sxs-lookup"><span data-stu-id="75495-172">These services are required toosuccessfully determine hello health and state of hello cluster while it is being created.</span></span>


<span data-ttu-id="75495-173">Fürt létrehozása során is használhat több Parancsfájlműveletek egyszerre.</span><span class="sxs-lookup"><span data-stu-id="75495-173">During cluster creation, you can use multiple script actions at once.</span></span> <span data-ttu-id="75495-174">Ezek a parancsfájlok hívják meg volt hello sorrendben.</span><span class="sxs-lookup"><span data-stu-id="75495-174">These scripts are invoked in hello order in which they were specified.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="75495-175">Parancsfájlműveletek 60 perc, vagy időtúllépés belül kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="75495-175">Script actions must complete within 60 minutes, or timeout.</span></span> <span data-ttu-id="75495-176">Fürt telepítése során, hello parancsfájl egyidejűleg más beállítás és konfiguráció folyamatok fut.</span><span class="sxs-lookup"><span data-stu-id="75495-176">During cluster provisioning, hello script runs concurrently with other setup and configuration processes.</span></span> <span data-ttu-id="75495-177">Erőforrások, például a CPU-idő vagy a hálózati sávszélesség erőforrásokért való versengés hello parancsfájl tootake hosszabb toofinish azonban nem a fejlesztési környezetet, mint okozhat.</span><span class="sxs-lookup"><span data-stu-id="75495-177">Competition for resources such as CPU time or network bandwidth may cause hello script tootake longer toofinish than it does in your development environment.</span></span>
>
> <span data-ttu-id="75495-178">toominimize hello alkalommal vesz toorun hello parancsfájl, feladatokat, mint a letöltés és forrásból származó alkalmazások fordítása elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="75495-178">toominimize hello time it takes toorun hello script, avoid tasks such as downloading and compiling applications from source.</span></span> <span data-ttu-id="75495-179">Előre lefordítani az alkalmazásokat, és az Azure Storage hello bináris tárolja.</span><span class="sxs-lookup"><span data-stu-id="75495-179">Pre-compile applications and store hello binary in Azure Storage.</span></span>


### <a name="script-action-on-a-running-cluster"></a><span data-ttu-id="75495-180">Egy futó fürtön parancsfájlművelet</span><span class="sxs-lookup"><span data-stu-id="75495-180">Script action on a running cluster</span></span>

<span data-ttu-id="75495-181">Egy hiba történt egy parancsfájlt a fürt létrehozása során használt műveletek egy már futó fürtön futtatott parancsfájl eltérően nem automatikusan okoz hello fürtállapot toochange tooa nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="75495-181">Unlike script actions used during cluster creation, a failure in a script ran on an already running cluster does not automatically cause hello cluster toochange tooa failed state.</span></span> <span data-ttu-id="75495-182">A parancsfájl befejeztét követően hello fürt tooa "fut" állapotba kell visszaadnia.</span><span class="sxs-lookup"><span data-stu-id="75495-182">Once a script completes, hello cluster should return tooa "running" state.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="75495-183">Akkor is, ha hello fürt "fut" állapotú, hello sikertelen parancsfájlt is megszakítják dolgot.</span><span class="sxs-lookup"><span data-stu-id="75495-183">Even if hello cluster has a 'running' state, hello failed script may have broken things.</span></span> <span data-ttu-id="75495-184">A parancsfájl például hello fürt számára szükséges fájlok sikerült törölni.</span><span class="sxs-lookup"><span data-stu-id="75495-184">For example, a script could delete files needed by hello cluster.</span></span>
>
> <span data-ttu-id="75495-185">Parancsfájlok műveletek futtassa legfelső szintű jogosultságokkal, ezért meg kell győződnie arról, hogy tudomásul veszi a parancsfájl funkciója tooyour fürt alkalmazása előtt.</span><span class="sxs-lookup"><span data-stu-id="75495-185">Scripts actions run with root privileges, so you should make sure that you understand what a script does before applying it tooyour cluster.</span></span>

<span data-ttu-id="75495-186">Egy parancsfájl tooa fürt alkalmazásakor hello fürt állapota megváltozik-e toofrom **futtató** túl**elfogadott**, majd **HDInsight konfigurációs**, és végül biztonsági túl**Futtató** sikeres parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="75495-186">When applying a script tooa cluster, hello cluster state changes toofrom **Running** too**Accepted**, then **HDInsight configuration**, and finally back too**Running** for successful scripts.</span></span> <span data-ttu-id="75495-187">hello parancsfájl állapot hello parancsfájlművelet-előzmény van bejelentkezve, és ezen információk toodetermine használhatja, hogy hello parancsfájl sikeres vagy sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="75495-187">hello script status is logged in hello script action history, and you can use this information toodetermine whether hello script succeeded or failed.</span></span> <span data-ttu-id="75495-188">Például hello `Get-AzureRmHDInsightScriptActionHistory` PowerShell-parancsmag használt tooview hello állapotát egy parancsfájlt is lehet.</span><span class="sxs-lookup"><span data-stu-id="75495-188">For example, hello `Get-AzureRmHDInsightScriptActionHistory` PowerShell cmdlet can be used tooview hello status of a script.</span></span> <span data-ttu-id="75495-189">Információk a következő szöveg hasonló toohello adja vissza:</span><span class="sxs-lookup"><span data-stu-id="75495-189">It returns information similar toohello following text:</span></span>

    ScriptExecutionId : 635918532516474303
    StartTime         : 8/14/2017 7:40:55 PM
    EndTime           : 8/14/2017 7:41:05 PM
    Status            : Succeeded

> [!NOTE]
> <span data-ttu-id="75495-190">Ha módosította hello fürt (rendszergazda) felhasználói jelszó hello fürt létrehozása után, műveletek lefutott-e a fürt parancsfájl sikertelen lehet.</span><span class="sxs-lookup"><span data-stu-id="75495-190">If you have changed hello cluster user (admin) password after hello cluster was created, script actions ran against this cluster may fail.</span></span> <span data-ttu-id="75495-191">Ha a megőrzött Parancsfájlműveletek adott cél munkavégző csomópontokhoz, ezek a parancsfájlok nem tud hello fürt méretezni.</span><span class="sxs-lookup"><span data-stu-id="75495-191">If you have any persisted script actions that target worker nodes, these scripts may fail when you scale hello cluster.</span></span>

## <a name="example-script-action-scripts"></a><span data-ttu-id="75495-192">Példa parancsfájlművelet parancsfájlok</span><span class="sxs-lookup"><span data-stu-id="75495-192">Example Script Action scripts</span></span>

<span data-ttu-id="75495-193">Parancsfájl művelet parancsfájlok keresztül hello segédeszközök a következő használható:</span><span class="sxs-lookup"><span data-stu-id="75495-193">Script Action scripts can be used through hello following utilities:</span></span>

* <span data-ttu-id="75495-194">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="75495-194">Azure portal</span></span>
* <span data-ttu-id="75495-195">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="75495-195">Azure PowerShell</span></span>
* <span data-ttu-id="75495-196">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="75495-196">Azure CLI</span></span>
* <span data-ttu-id="75495-197">HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="75495-197">HDInsight .NET SDK</span></span>

<span data-ttu-id="75495-198">HDInsight összetevők követően a HDInsight-fürtökön parancsfájlok tooinstall hello biztosítja:</span><span class="sxs-lookup"><span data-stu-id="75495-198">HDInsight provides scripts tooinstall hello following components on HDInsight clusters:</span></span>

| <span data-ttu-id="75495-199">Név</span><span class="sxs-lookup"><span data-stu-id="75495-199">Name</span></span> | <span data-ttu-id="75495-200">Szkript</span><span class="sxs-lookup"><span data-stu-id="75495-200">Script</span></span> |
| --- | --- |
| <span data-ttu-id="75495-201">**Egy Azure Storage-fiók hozzáadása**</span><span class="sxs-lookup"><span data-stu-id="75495-201">**Add an Azure Storage account**</span></span> |<span data-ttu-id="75495-202">https://hdiconfigactions.BLOB.Core.Windows.NET/linuxaddstorageaccountv01/Add-Storage-Account-v01.SH. Lásd: [Hozzáadás további tárhely tooan HDInsight-fürt](hdinsight-hadoop-add-storage.md).</span><span class="sxs-lookup"><span data-stu-id="75495-202">https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh. See [Add additional storage tooan HDInsight cluster](hdinsight-hadoop-add-storage.md).</span></span> |
| <span data-ttu-id="75495-203">**Hue telepítése**</span><span class="sxs-lookup"><span data-stu-id="75495-203">**Install Hue**</span></span> |<span data-ttu-id="75495-204">https://hdiconfigactions.BLOB.Core.Windows.NET/linuxhueconfigactionv02/Install-hue-uber-v02.SH. Lásd: [telepítése és használata a HDInsight Hue-fürtök](hdinsight-hadoop-hue-linux.md).</span><span class="sxs-lookup"><span data-stu-id="75495-204">https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh. See [Install and use Hue on HDInsight clusters](hdinsight-hadoop-hue-linux.md).</span></span> |
| <span data-ttu-id="75495-205">**Presto telepítése**</span><span class="sxs-lookup"><span data-stu-id="75495-205">**Install Presto**</span></span> |<span data-ttu-id="75495-206">https://RAW.githubusercontent.com/hdinsight/presto-hdinsight/Master/installpresto.SH. Lásd: [telepítése és használata Presto a HDInsight-fürtök](hdinsight-hadoop-install-presto.md).</span><span class="sxs-lookup"><span data-stu-id="75495-206">https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh. See [Install and use Presto on HDInsight clusters](hdinsight-hadoop-install-presto.md).</span></span> |
| <span data-ttu-id="75495-207">**Solr telepítése**</span><span class="sxs-lookup"><span data-stu-id="75495-207">**Install Solr**</span></span> |<span data-ttu-id="75495-208">https://hdiconfigactions.BLOB.Core.Windows.NET/linuxsolrconfigactionv01/solr-Installer-v01.SH. Lásd: [telepítése és használata Solr a HDInsight-fürtök](hdinsight-hadoop-solr-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="75495-208">https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh. See [Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install-linux.md).</span></span> |
| <span data-ttu-id="75495-209">**Giraph telepítése**</span><span class="sxs-lookup"><span data-stu-id="75495-209">**Install Giraph**</span></span> |<span data-ttu-id="75495-210">https://hdiconfigactions.BLOB.Core.Windows.NET/linuxgiraphconfigactionv01/giraph-Installer-v01.SH. Lásd: [telepítése és használata Giraph a HDInsight-fürtök](hdinsight-hadoop-giraph-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="75495-210">https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh. See [Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install-linux.md).</span></span> |
| <span data-ttu-id="75495-211">**Hive-könyvtárakhoz előzetes betöltése**</span><span class="sxs-lookup"><span data-stu-id="75495-211">**Pre-load Hive libraries**</span></span> |<span data-ttu-id="75495-212">https://hdiconfigactions.BLOB.Core.Windows.NET/linuxsetupcustomhivelibsv01/Setup-customhivelibs-v01.SH. Lásd: [kódtárak hozzáadása Hive HDInsight-fürtök](hdinsight-hadoop-add-hive-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="75495-212">https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh. See [Add Hive libraries on HDInsight clusters](hdinsight-hadoop-add-hive-libraries.md).</span></span> |
| <span data-ttu-id="75495-213">**Mono telepítése vagy frissítése**</span><span class="sxs-lookup"><span data-stu-id="75495-213">**Install or update Mono**</span></span> | <span data-ttu-id="75495-214">https://hdiconfigactions.BLOB.Core.Windows.NET/Install-mono/Install-mono.bash.</span><span class="sxs-lookup"><span data-stu-id="75495-214">https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash.</span></span> <span data-ttu-id="75495-215">Lásd: [telepítése vagy frissítése a HDInsight monó](hdinsight-hadoop-install-mono.md).</span><span class="sxs-lookup"><span data-stu-id="75495-215">See [Install or update Mono on HDInsight](hdinsight-hadoop-install-mono.md).</span></span> |

## <a name="use-a-script-action-during-cluster-creation"></a><span data-ttu-id="75495-216">Egy parancsfájlművelettel fürt létrehozása során</span><span class="sxs-lookup"><span data-stu-id="75495-216">Use a Script Action during cluster creation</span></span>

<span data-ttu-id="75495-217">Ez a szakasz hello különböző módokon használhatja a Parancsfájlműveletek létrehozása a HDInsight-fürtök esetén a példákat.</span><span class="sxs-lookup"><span data-stu-id="75495-217">This section provides examples on hello different ways you can use script actions when creating an HDInsight cluster.</span></span>

### <a name="use-a-script-action-during-cluster-creation-from-hello-azure-portal"></a><span data-ttu-id="75495-218">Egy parancsfájlművelettel hello Azure-portálon a fürt létrehozása során</span><span class="sxs-lookup"><span data-stu-id="75495-218">Use a Script Action during cluster creation from hello Azure portal</span></span>

1. <span data-ttu-id="75495-219">Indítsa el a fürt létrehozása a részben ismertetett módon [Hadoop létrehozása a HDInsight-fürtök](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="75495-219">Start creating a cluster as described at [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span> <span data-ttu-id="75495-220">Állítsa le a hello elérésekor __fürt összefoglaló__ szakasz.</span><span class="sxs-lookup"><span data-stu-id="75495-220">Stop when you reach hello __Cluster summary__ section.</span></span>

2. <span data-ttu-id="75495-221">A hello __fürt összefoglaló__ szakaszban, jelölje be hello __szerkesztése__ hivatkozás a __speciális beállítások__.</span><span class="sxs-lookup"><span data-stu-id="75495-221">From hello __Cluster summary__ section, select hello __edit__ link for __Advanced settings__.</span></span>

    ![Speciális beállítások hivatkozása](./media/hdinsight-hadoop-customize-cluster-linux/advanced-settings-link.png)

3. <span data-ttu-id="75495-223">A hello __speciális beállítások__ szakaszban jelölje be __parancsfájl-műveletek__.</span><span class="sxs-lookup"><span data-stu-id="75495-223">From hello __Advanced settings__ section, select __Script actions__.</span></span> <span data-ttu-id="75495-224">A hello __parancsfájl-műveletek__ szakaszban jelölje be __+ Submit újdonságai__</span><span class="sxs-lookup"><span data-stu-id="75495-224">From hello __Script actions__ section, select __+ Submit new__</span></span>

    ![Egy új parancsfájlművelet](./media/hdinsight-hadoop-customize-cluster-linux/add-script-action.png)

4. <span data-ttu-id="75495-226">Használjon hello __válassza ki a parancsprogramot__ bejegyzés tooselect egy előre elkészített parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="75495-226">Use hello __Select a script__ entry tooselect a pre-made script.</span></span> <span data-ttu-id="75495-227">toouse egyéni parancsfájl, válassza ki __egyéni__ és adja meg a hello __neve__ és __Bash parancsfájlok URI__ a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="75495-227">toouse a custom script, select __Custom__ and then provide hello __Name__ and __Bash script URI__ for your script.</span></span>

    ![A parancsprogramok hozzáadásához hello válassza parancsfájl formátumban](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    <span data-ttu-id="75495-229">a következő táblázat hello hello űrlap hello elemeket ismerteti:</span><span class="sxs-lookup"><span data-stu-id="75495-229">hello following table describes hello elements on hello form:</span></span>

    | <span data-ttu-id="75495-230">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="75495-230">Property</span></span> | <span data-ttu-id="75495-231">Érték</span><span class="sxs-lookup"><span data-stu-id="75495-231">Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="75495-232">Válassza ki a parancsprogramot</span><span class="sxs-lookup"><span data-stu-id="75495-232">Select a script</span></span> | <span data-ttu-id="75495-233">toouse a saját parancsfájl, jelölje be __egyéni__.</span><span class="sxs-lookup"><span data-stu-id="75495-233">toouse your own script, select __Custom__.</span></span> <span data-ttu-id="75495-234">Ellenkező esetben válasszon egyet a megadott hello parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="75495-234">Otherwise, select one of hello provided scripts.</span></span> |
    | <span data-ttu-id="75495-235">Név</span><span class="sxs-lookup"><span data-stu-id="75495-235">Name</span></span> |<span data-ttu-id="75495-236">Adja meg a hello parancsfájlművelet nevét.</span><span class="sxs-lookup"><span data-stu-id="75495-236">Specify a name for hello script action.</span></span> |
    | <span data-ttu-id="75495-237">Bash parancsfájlok URI</span><span class="sxs-lookup"><span data-stu-id="75495-237">Bash script URI</span></span> |<span data-ttu-id="75495-238">Hello URI toohello parancsfájl megadásához, amely meghívott toocustomize hello fürt.</span><span class="sxs-lookup"><span data-stu-id="75495-238">Specify hello URI toohello script that is invoked toocustomize hello cluster.</span></span> |
    | <span data-ttu-id="75495-239">HEAD/munkavégző/Zookeeper</span><span class="sxs-lookup"><span data-stu-id="75495-239">Head/Worker/Zookeeper</span></span> |<span data-ttu-id="75495-240">Adja meg a hello csomópontok (**Head**, **munkavégző**, vagy **ZooKeeper**) mely hello testreszabási a parancsfájl futtatása.</span><span class="sxs-lookup"><span data-stu-id="75495-240">Specify hello nodes (**Head**, **Worker**, or **ZooKeeper**) on which hello customization script is run.</span></span> |
    | <span data-ttu-id="75495-241">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="75495-241">Parameters</span></span> |<span data-ttu-id="75495-242">Adja meg a hello paraméterek hello parancsfájl által szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="75495-242">Specify hello parameters, if required by hello script.</span></span> |

    <span data-ttu-id="75495-243">Használjon hello __parancsfájlműveletet__ bejegyzés tooensure, hogy a parancsfájl hello skálázás műveletek során alkalmazzák.</span><span class="sxs-lookup"><span data-stu-id="75495-243">Use hello __Persist this script action__ entry tooensure that hello script is applied during scaling operations.</span></span>

5. <span data-ttu-id="75495-244">Válassza ki __létrehozása__ toosave hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="75495-244">Select __Create__ toosave hello script.</span></span> <span data-ttu-id="75495-245">Ezután __+ új küldési__ tooadd parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="75495-245">You can then use __+ Submit new__ tooadd another script.</span></span>

    ![Több parancsprogram-műveletek](./media/hdinsight-hadoop-customize-cluster-linux/multiple-scripts.png)

    <span data-ttu-id="75495-247">Amikor elkészült a parancsfájlok hozzáadását, használja a hello __kiválasztása__ gombra, és ezután hello __tovább__ gomb tooreturn toohello __fürt összefoglaló__ szakasz.</span><span class="sxs-lookup"><span data-stu-id="75495-247">When you are done adding scripts, use hello __Select__ button, and then hello __Next__ button tooreturn toohello __Cluster summary__ section.</span></span>

3. <span data-ttu-id="75495-248">toocreate hello fürt, jelölje be __létrehozása__ a hello __fürt összefoglaló__ kijelölés.</span><span class="sxs-lookup"><span data-stu-id="75495-248">toocreate hello cluster, select __Create__ from hello __Cluster summary__ selection.</span></span>

### <a name="use-a-script-action-from-azure-resource-manager-templates"></a><span data-ttu-id="75495-249">Egy parancsfájlművelettel Azure Resource Manager-sablonok alapján</span><span class="sxs-lookup"><span data-stu-id="75495-249">Use a Script Action from Azure Resource Manager templates</span></span>

<span data-ttu-id="75495-250">Ebben a szakaszban hello példák bemutatják, hogyan toouse parancsfájl-műveletek Azure Resource Manager-sablonok.</span><span class="sxs-lookup"><span data-stu-id="75495-250">hello examples in this section demonstrate how toouse script actions with Azure Resource Manager templates.</span></span>

#### <a name="before-you-begin"></a><span data-ttu-id="75495-251">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="75495-251">Before you begin</span></span>

* <span data-ttu-id="75495-252">A munkaállomás toorun HDInsight Powershell-parancsmagok konfigurálásával kapcsolatos további információkért lásd: [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="75495-252">For information about configuring a workstation toorun HDInsight Powershell cmdlets, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="75495-253">Útmutatást toocreate sablonjainak használatáról [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="75495-253">For instructions on how toocreate templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="75495-254">Ha nem korábban használt Azure PowerShell a Resource Manager, lásd: [az Azure PowerShell használata Azure Resource Managerrel](../azure-resource-manager/powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="75495-254">If you have not previously used Azure PowerShell with Resource Manager, see [Using Azure PowerShell with Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).</span></span>

#### <a name="create-clusters-using-script-action"></a><span data-ttu-id="75495-255">Parancsfájlművelet-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="75495-255">Create clusters using Script Action</span></span>

1. <span data-ttu-id="75495-256">A sablon tooa helye a számítógépen a következő hello másolja.</span><span class="sxs-lookup"><span data-stu-id="75495-256">Copy hello following template tooa location on your computer.</span></span> <span data-ttu-id="75495-257">Ez a sablon telepítése Giraph hello headnodes és munkavégző hello fürt csomópontja.</span><span class="sxs-lookup"><span data-stu-id="75495-257">This template installs Giraph on hello headnodes and worker nodes in hello cluster.</span></span> <span data-ttu-id="75495-258">Ha a hello JSON-sablon esetében érvényes is ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="75495-258">You can also verify if hello JSON template is valid.</span></span> <span data-ttu-id="75495-259">Illessze be a sablon a tartalom [JSONLint](http://jsonlint.com/), online JSON-érvényesítési segédprogramot.</span><span class="sxs-lookup"><span data-stu-id="75495-259">Paste your template content into [JSONLint](http://jsonlint.com/), an online JSON validation tool.</span></span>

            {
            "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {
                "clusterLocation": {
                    "type": "string",
                    "defaultValue": "West US",
                    "allowedValues": [ "West US" ]
                },
                "clusterName": {
                    "type": "string"
                },
                "clusterUserName": {
                    "type": "string",
                    "defaultValue": "admin"
                },
                "clusterUserPassword": {
                    "type": "securestring"
                },
                "sshUserName": {
                    "type": "string",
                    "defaultValue": "username"
                },
                "sshPassword": {
                    "type": "securestring"
                },
                "clusterStorageAccountName": {
                    "type": "string"
                },
                "clusterStorageAccountResourceGroup": {
                    "type": "string"
                },
                "clusterStorageType": {
                    "type": "string",
                    "defaultValue": "Standard_LRS",
                    "allowedValues": [
                        "Standard_LRS",
                        "Standard_GRS",
                        "Standard_ZRS"
                    ]
                },
                "clusterStorageAccountContainer": {
                    "type": "string"
                },
                "clusterHeadNodeCount": {
                    "type": "int",
                    "defaultValue": 1
                },
                "clusterWorkerNodeCount": {
                    "type": "int",
                    "defaultValue": 2
                }
            },
            "variables": {
            },
            "resources": [
                {
                    "name": "[parameters('clusterStorageAccountName')]",
                    "type": "Microsoft.Storage/storageAccounts",
                    "location": "[parameters('clusterLocation')]",
                    "apiVersion": "2015-05-01-preview",
                    "dependsOn": [ ],
                    "tags": { },
                    "properties": {
                        "accountType": "[parameters('clusterStorageType')]"
                    }
                },
                {
                    "name": "[parameters('clusterName')]",
                    "type": "Microsoft.HDInsight/clusters",
                    "location": "[parameters('clusterLocation')]",
                    "apiVersion": "2015-03-01-preview",
                    "dependsOn": [
                        "[concat('Microsoft.Storage/storageAccounts/', parameters('clusterStorageAccountName'))]"
                    ],
                    "tags": { },
                    "properties": {
                        "clusterVersion": "3.2",
                        "osType": "Linux",
                        "clusterDefinition": {
                            "kind": "hadoop",
                            "configurations": {
                                "gateway": {
                                    "restAuthCredential.isEnabled": true,
                                    "restAuthCredential.username": "[parameters('clusterUserName')]",
                                    "restAuthCredential.password": "[parameters('clusterUserPassword')]"
                                }
                            }
                        },
                        "storageProfile": {
                            "storageaccounts": [
                                {
                                    "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                                    "isDefault": true,
                                    "container": "[parameters('clusterStorageAccountContainer')]",
                                    "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), '2015-05-01-preview').key1]"
                                }
                            ]
                        },
                        "computeProfile": {
                            "roles": [
                                {
                                    "name": "headnode",
                                    "targetInstanceCount": "[parameters('clusterHeadNodeCount')]",
                                    "hardwareProfile": {
                                        "vmSize": "Large"
                                    },
                                    "osProfile": {
                                        "linuxOperatingSystemProfile": {
                                            "username": "[parameters('sshUserName')]",
                                            "password": "[parameters('sshPassword')]"
                                        }
                                    },
                                    "scriptActions": [
                                        {
                                            "name": "installGiraph",
                                            "uri": "https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh",
                                            "parameters": ""
                                        }
                                    ]
                                },
                                {
                                    "name": "workernode",
                                    "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                                    "hardwareProfile": {
                                        "vmSize": "Large"
                                    },
                                    "osProfile": {
                                        "linuxOperatingSystemProfile": {
                                            "username": "[parameters('sshUserName')]",
                                            "password": "[parameters('sshPassword')]"
                                        }
                                    },
                                    "scriptActions": [
                                        {
                                            "name": "installR",
                                            "uri": "https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh",
                                            "parameters": ""
                                        }
                                    ]
                                }
                            ]
                        }
                    }
                }
            ],
            "outputs": {
                "cluster":{
                    "type" : "object",
                    "value" : "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                }
            }
        }
2. <span data-ttu-id="75495-260">Indítsa el az Azure PowerShell, és jelentkezzen be tooyour Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="75495-260">Start Azure PowerShell and Log in tooyour Azure account.</span></span> <span data-ttu-id="75495-261">Miután megadta a hitelesítő adatait, a hello parancs visszaadja a fiókja adatait.</span><span class="sxs-lookup"><span data-stu-id="75495-261">After providing your credentials, hello command returns information about your account.</span></span>

        Add-AzureRmAccount

        Id                             Type       ...
        --                             ----
        someone@example.com            User       ...
3. <span data-ttu-id="75495-262">Ha több előfizetéssel, adja meg az előfizetés-azonosító hello kívánja toouse központi telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="75495-262">If you have multiple subscriptions, provide hello subscription ID you wish toouse for deployment.</span></span>

        Select-AzureRmSubscription -SubscriptionID <YourSubscriptionId>

    > [!NOTE]
    > <span data-ttu-id="75495-263">Használhat `Get-AzureRmSubscription` tooget, beleértve az előfizetés-azonosító hello minden egyes a fiókjához társított előfizetéseket listáját.</span><span class="sxs-lookup"><span data-stu-id="75495-263">You can use `Get-AzureRmSubscription` tooget a list of all subscriptions associated with your account, which includes hello subscription ID for each one.</span></span>

4. <span data-ttu-id="75495-264">Ha nem rendelkezik egy meglévő erőforráscsoportot, hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="75495-264">If you do not have an existing resource group, create a resource group.</span></span> <span data-ttu-id="75495-265">Adja meg a hello erőforráscsoportot és helyet, a megoldáshoz szükséges hello nevét.</span><span class="sxs-lookup"><span data-stu-id="75495-265">Provide hello name of hello resource group and location that you need for your solution.</span></span> <span data-ttu-id="75495-266">Új erőforráscsoport hello összegzését adja vissza.</span><span class="sxs-lookup"><span data-stu-id="75495-266">A summary of hello new resource group is returned.</span></span>

        New-AzureRmResourceGroup -Name myresourcegroup -Location "West US"

        ResourceGroupName : myresourcegroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

5. <span data-ttu-id="75495-267">az erőforráscsoport, futtassa a hello telepítésének toocreate **New-AzureRmResourceGroupDeployment** parancsot, és adja meg a szükséges paraméterek hello.</span><span class="sxs-lookup"><span data-stu-id="75495-267">toocreate a deployment for your resource group, run hello **New-AzureRmResourceGroupDeployment** command and provide hello necessary parameters.</span></span> <span data-ttu-id="75495-268">hello paraméternek számít a hello a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="75495-268">hello parameters include hello following data:</span></span>

    * <span data-ttu-id="75495-269">A központi telepítés nevét</span><span class="sxs-lookup"><span data-stu-id="75495-269">A name for your deployment</span></span>
    * <span data-ttu-id="75495-270">az erőforráscsoport neve hello</span><span class="sxs-lookup"><span data-stu-id="75495-270">hello name of your resource group</span></span>
    * <span data-ttu-id="75495-271">hello elérési útja vagy URL-cím létrehozott toohello sablont.</span><span class="sxs-lookup"><span data-stu-id="75495-271">hello path or URL toohello template you created.</span></span>

  <span data-ttu-id="75495-272">Ha a sablon bármely paramétereket igényel, át kell adnia ezeket a paramétereket.</span><span class="sxs-lookup"><span data-stu-id="75495-272">If your template requires any parameters, you must pass those parameters as well.</span></span> <span data-ttu-id="75495-273">Hello parancsfájl művelet tooinstall R hello fürtön ebben az esetben nincs szükség a paramétereket.</span><span class="sxs-lookup"><span data-stu-id="75495-273">In this case, hello script action tooinstall R on hello cluster does not require any parameters.</span></span>

        New-AzureRmResourceGroupDeployment -Name mydeployment -ResourceGroupName myresourcegroup -TemplateFile <PathOrLinkToTemplate>

    <span data-ttu-id="75495-274">Hello sablon hello paraméterek felszólító tooprovide értékei lesznek.</span><span class="sxs-lookup"><span data-stu-id="75495-274">You are prompted tooprovide values for hello parameters defined in hello template.</span></span>

1. <span data-ttu-id="75495-275">Hello erőforráscsoportban van telepítve, hello telepítés összegzését jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="75495-275">When hello resource group has been deployed, a summary of hello deployment is displayed.</span></span>

          DeploymentName    : mydeployment
          ResourceGroupName : myresourcegroup
          ProvisioningState : Succeeded
          Timestamp         : 8/14/2017 7:00:27 PM
          Mode              : Incremental
          ...

2. <span data-ttu-id="75495-276">Ha nem sikerül a telepítés, a következő parancsmagok tooget információ hello hibák hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="75495-276">If your deployment fails, you can use hello following cmdlets tooget information about hello failures.</span></span>

        Get-AzureRmResourceGroupDeployment -ResourceGroupName myresourcegroup -ProvisioningState Failed

### <a name="use-a-script-action-during-cluster-creation-from-azure-powershell"></a><span data-ttu-id="75495-277">Egy parancsfájlművelettel Azure PowerShell a fürt létrehozása során</span><span class="sxs-lookup"><span data-stu-id="75495-277">Use a Script Action during cluster creation from Azure PowerShell</span></span>

<span data-ttu-id="75495-278">Ebben a szakaszban hello használjuk [Add-AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx) parancsmag tooinvoke parancsfájlokat parancsfájlművelet toocustomize fürt használatával.</span><span class="sxs-lookup"><span data-stu-id="75495-278">In this section, we use hello [Add-AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx) cmdlet tooinvoke scripts by using Script Action toocustomize a cluster.</span></span> <span data-ttu-id="75495-279">A folytatás előtt győződjön meg arról, hogy telepítette és konfigurálta az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75495-279">Before proceeding, make sure you have installed and configured Azure PowerShell.</span></span> <span data-ttu-id="75495-280">A munkaállomás toorun HDInsight PowerShell-parancsmagok konfigurálásával kapcsolatos további információkért lásd: [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="75495-280">For information about configuring a workstation toorun HDInsight PowerShell cmdlets, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="75495-281">hello alábbi parancsfájl bemutatja, hogyan tooapply egy parancsfájlművelet, amikor a PowerShell használatával fürtöt hoz létre:</span><span class="sxs-lookup"><span data-stu-id="75495-281">hello following script demonstrates how tooapply a script action when creating a cluster using PowerShell:</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=5-90)]

<span data-ttu-id="75495-282">Hello fürt létrehozása előtt több percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="75495-282">It can take several minutes before hello cluster is created.</span></span>

### <a name="use-a-script-action-during-cluster-creation-from-hello-hdinsight-net-sdk"></a><span data-ttu-id="75495-283">Egy parancsfájlművelettel hello HDInsight .NET SDK a fürt létrehozása során</span><span class="sxs-lookup"><span data-stu-id="75495-283">Use a Script Action during cluster creation from hello HDInsight .NET SDK</span></span>

<span data-ttu-id="75495-284">hello HDInsight .NET SDK biztosít, amely megkönnyíti a .NET-alkalmazás és a HDInsight együttes könnyebb toowork klienskódtárak segítségével.</span><span class="sxs-lookup"><span data-stu-id="75495-284">hello HDInsight .NET SDK provides client libraries that makes it easier toowork with HDInsight from a .NET application.</span></span> <span data-ttu-id="75495-285">Kódminta, lásd: [létrehozása Linux-alapú fürtökön a Hdinsightban az hello .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action).</span><span class="sxs-lookup"><span data-stu-id="75495-285">For a code sample, see [Create Linux-based clusters in HDInsight using hello .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action).</span></span>

## <a name="apply-a-script-action-tooa-running-cluster"></a><span data-ttu-id="75495-286">A fürt futtató parancsfájlművelet tooa alkalmazása</span><span class="sxs-lookup"><span data-stu-id="75495-286">Apply a Script Action tooa running cluster</span></span>

<span data-ttu-id="75495-287">Ebben a szakaszban megtudhatja, hogyan tooapply parancsfájl-műveletek tooa rendszerű fürt.</span><span class="sxs-lookup"><span data-stu-id="75495-287">In this section, learn how tooapply script actions tooa running cluster.</span></span>

### <a name="apply-a-script-action-tooa-running-cluster-from-hello-azure-portal"></a><span data-ttu-id="75495-288">A fürt futtatja hello Azure-portálon parancsfájlművelet tooa alkalmazása</span><span class="sxs-lookup"><span data-stu-id="75495-288">Apply a Script Action tooa running cluster from hello Azure portal</span></span>

1. <span data-ttu-id="75495-289">A hello [Azure-portálon](https://portal.azure.com), válassza ki a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="75495-289">From hello [Azure portal](https://portal.azure.com), select your HDInsight cluster.</span></span>

2. <span data-ttu-id="75495-290">Hello HDInsight fürt áttekintése, válassza ki hello **Parancsfájlműveletek** csempére.</span><span class="sxs-lookup"><span data-stu-id="75495-290">From hello HDInsight cluster overview, select hello **Script Actions** tile.</span></span>

    ![Parancsfájl műveletek csempe](./media/hdinsight-hadoop-customize-cluster-linux/scriptactionstile.png)

   > [!NOTE]
   > <span data-ttu-id="75495-292">Igény szerint kiválaszthatja **összes beállítás** , és válassza **Parancsfájlműveletek** a hello beállítások szakaszban.</span><span class="sxs-lookup"><span data-stu-id="75495-292">You can also select **All settings** and then select **Script Actions** from hello Settings section.</span></span>

3. <span data-ttu-id="75495-293">A Parancsfájlműveletek szakasz hello hello felső részén jelölje ki **új küldési**.</span><span class="sxs-lookup"><span data-stu-id="75495-293">From hello top of hello Script Actions section, select **Submit new**.</span></span>

    ![A parancsfájl tooa futtató fürt hozzáadása](./media/hdinsight-hadoop-customize-cluster-linux/add-script-running-cluster.png)

4. <span data-ttu-id="75495-295">Használjon hello __válassza ki a parancsprogramot__ bejegyzés tooselect egy előre elkészített parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="75495-295">Use hello __Select a script__ entry tooselect a pre-made script.</span></span> <span data-ttu-id="75495-296">toouse egyéni parancsfájl, válassza ki __egyéni__ és adja meg a hello __neve__ és __Bash parancsfájlok URI__ a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="75495-296">toouse a custom script, select __Custom__ and then provide hello __Name__ and __Bash script URI__ for your script.</span></span>

    ![A parancsprogramok hozzáadásához hello válassza parancsfájl formátumban](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    <span data-ttu-id="75495-298">a következő táblázat hello hello űrlap hello elemeket ismerteti:</span><span class="sxs-lookup"><span data-stu-id="75495-298">hello following table describes hello elements on hello form:</span></span>

    | <span data-ttu-id="75495-299">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="75495-299">Property</span></span> | <span data-ttu-id="75495-300">Érték</span><span class="sxs-lookup"><span data-stu-id="75495-300">Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="75495-301">Válassza ki a parancsprogramot</span><span class="sxs-lookup"><span data-stu-id="75495-301">Select a script</span></span> | <span data-ttu-id="75495-302">toouse a saját parancsfájl, jelölje be __egyéni__.</span><span class="sxs-lookup"><span data-stu-id="75495-302">toouse your own script, select __custom__.</span></span> <span data-ttu-id="75495-303">Ellenkező esetben válassza ki a megadott parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="75495-303">Otherwise, select a provided script.</span></span> |
    | <span data-ttu-id="75495-304">Név</span><span class="sxs-lookup"><span data-stu-id="75495-304">Name</span></span> |<span data-ttu-id="75495-305">Adja meg a hello parancsfájlművelet nevét.</span><span class="sxs-lookup"><span data-stu-id="75495-305">Specify a name for hello script action.</span></span> |
    | <span data-ttu-id="75495-306">Bash parancsfájlok URI</span><span class="sxs-lookup"><span data-stu-id="75495-306">Bash script URI</span></span> |<span data-ttu-id="75495-307">Hello URI toohello parancsfájl megadásához, amely meghívott toocustomize hello fürt.</span><span class="sxs-lookup"><span data-stu-id="75495-307">Specify hello URI toohello script that is invoked toocustomize hello cluster.</span></span> |
    | <span data-ttu-id="75495-308">HEAD/munkavégző/Zookeeper</span><span class="sxs-lookup"><span data-stu-id="75495-308">Head/Worker/Zookeeper</span></span> |<span data-ttu-id="75495-309">Adja meg a hello csomópontok (**Head**, **munkavégző**, vagy **ZooKeeper**) mely hello testreszabási a parancsfájl futtatása.</span><span class="sxs-lookup"><span data-stu-id="75495-309">Specify hello nodes (**Head**, **Worker**, or **ZooKeeper**) on which hello customization script is run.</span></span> |
    | <span data-ttu-id="75495-310">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="75495-310">Parameters</span></span> |<span data-ttu-id="75495-311">Adja meg a hello paraméterek hello parancsfájl által szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="75495-311">Specify hello parameters, if required by hello script.</span></span> |

    <span data-ttu-id="75495-312">Használjon hello __parancsfájlműveletet__ bejegyzés toomake meg arról, hogy hello parancsfájl skálázás műveletek során alkalmazzák.</span><span class="sxs-lookup"><span data-stu-id="75495-312">Use hello __Persist this script action__ entry toomake sure hello script is applied during scaling operations.</span></span>

5. <span data-ttu-id="75495-313">Végül, használja a hello **létrehozása** gomb tooapply hello parancsfájl toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="75495-313">Finally, use hello **Create** button tooapply hello script toohello cluster.</span></span>

### <a name="apply-a-script-action-tooa-running-cluster-from-azure-powershell"></a><span data-ttu-id="75495-314">A fürt futtatja az Azure PowerShell parancsfájlművelet tooa alkalmazása</span><span class="sxs-lookup"><span data-stu-id="75495-314">Apply a Script Action tooa running cluster from Azure PowerShell</span></span>

<span data-ttu-id="75495-315">A folytatás előtt győződjön meg arról, hogy telepítette és konfigurálta az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75495-315">Before proceeding, make sure you have installed and configured Azure PowerShell.</span></span> <span data-ttu-id="75495-316">A munkaállomás toorun HDInsight PowerShell-parancsmagok konfigurálásával kapcsolatos további információkért lásd: [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="75495-316">For information about configuring a workstation toorun HDInsight PowerShell cmdlets, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="75495-317">hello következő példa bemutatja, hogyan tooapply parancsfájl művelet tooa működő fürthöz:</span><span class="sxs-lookup"><span data-stu-id="75495-317">hello following example demonstrates how tooapply a script action tooa running cluster:</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=105-117)]

<span data-ttu-id="75495-318">Hello művelet után információk hasonló toohello a következő szöveg jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="75495-318">Once hello operation completes, you receive information similar toohello following text:</span></span>

    OperationState  : Succeeded
    ErrorMessage    :
    Name            : Giraph
    Uri             : https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
    Parameters      :
    NodeTypes       : {HeadNode, WorkerNode}

### <a name="apply-a-script-action-tooa-running-cluster-from-hello-azure-cli"></a><span data-ttu-id="75495-319">A fürt hello Azure CLI-ről fut parancsfájlművelet tooa alkalmazása</span><span class="sxs-lookup"><span data-stu-id="75495-319">Apply a Script Action tooa running cluster from hello Azure CLI</span></span>

<span data-ttu-id="75495-320">A folytatás előtt győződjön meg arról, hogy telepítette és konfigurálta a hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="75495-320">Before proceeding, make sure you have installed and configured hello Azure CLI.</span></span> <span data-ttu-id="75495-321">További információkért lásd: [telepítés hello Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="75495-321">For more information, see [Install hello Azure CLI](../cli-install-nodejs.md).</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. <span data-ttu-id="75495-322">tooswitch tooAzure Resource Manager módra a következő parancsot a következő hello parancssori hello használata:</span><span class="sxs-lookup"><span data-stu-id="75495-322">tooswitch tooAzure Resource Manager mode, use hello following command at hello command line:</span></span>

        azure config mode arm

2. <span data-ttu-id="75495-323">A következő tooauthenticate tooyour Azure-előfizetés hello használata.</span><span class="sxs-lookup"><span data-stu-id="75495-323">Use hello following tooauthenticate tooyour Azure subscription.</span></span>

        azure login

3. <span data-ttu-id="75495-324">A következő parancs tooapply egy fürtben futó parancsfájl művelet tooa hello használata</span><span class="sxs-lookup"><span data-stu-id="75495-324">Use hello following command tooapply a script action tooa running cluster</span></span>

        azure hdinsight script-action create <clustername> -g <resourcegroupname> -n <scriptname> -u <scriptURI> -t <nodetypes>

    <span data-ttu-id="75495-325">Ha nincs megadva ez a parancs paramétereit, kéri azokat.</span><span class="sxs-lookup"><span data-stu-id="75495-325">If you omit parameters for this command, you are prompted for them.</span></span> <span data-ttu-id="75495-326">Ha a megadott parancsfájl hello `-u` fogad el paramétert, megadhatja őket hello segítségével `-p` paraméter.</span><span class="sxs-lookup"><span data-stu-id="75495-326">If hello script you specify with `-u` accepts parameters, you can specify them using hello `-p` parameter.</span></span>

    <span data-ttu-id="75495-327">Érvénytelen csomópont típusok `headnode`, `workernode`, és `zookeeper`.</span><span class="sxs-lookup"><span data-stu-id="75495-327">Valid node types are `headnode`, `workernode`, and `zookeeper`.</span></span> <span data-ttu-id="75495-328">Ha hello parancsfájl alkalmazott toomultiple csomóponttípusok, adja meg a elválasztott hello típusokat egy ";".</span><span class="sxs-lookup"><span data-stu-id="75495-328">If hello script should be applied toomultiple node types, specify hello types separated by a ';'.</span></span> <span data-ttu-id="75495-329">Például: `-n headnode;workernode`.</span><span class="sxs-lookup"><span data-stu-id="75495-329">For example, `-n headnode;workernode`.</span></span>

    <span data-ttu-id="75495-330">toopersist hello parancsfájl, vegye fel a hello `--persistOnSuccess`.</span><span class="sxs-lookup"><span data-stu-id="75495-330">toopersist hello script, add hello `--persistOnSuccess`.</span></span> <span data-ttu-id="75495-331">Akkor is is megmaradnak hello parancsfájl később használatával `azure hdinsight script-action persisted set`.</span><span class="sxs-lookup"><span data-stu-id="75495-331">You can also persist hello script later by using `azure hdinsight script-action persisted set`.</span></span>

    <span data-ttu-id="75495-332">Amint hello feladat befejeződik, a következő szöveg kimeneti hasonló toohello kapni:</span><span class="sxs-lookup"><span data-stu-id="75495-332">Once hello job completes, you receive output similar toohello following text:</span></span>

        info:    Executing command hdinsight script-action create
        + Executing Script Action on HDInsight cluster
        data:    Operation Info
        data:    ---------------
        data:    Operation status:
        data:    Operation ID:  b707b10e-e633-45c0-baa9-8aed3d348c13
        info:    hdinsight script-action create command OK

### <a name="apply-a-script-action-tooa-running-cluster-using-rest-api"></a><span data-ttu-id="75495-333">A futó REST API használatával fürt parancsfájlművelet tooa alkalmazása</span><span class="sxs-lookup"><span data-stu-id="75495-333">Apply a Script Action tooa running cluster using REST API</span></span>

<span data-ttu-id="75495-334">Lásd: [Parancsfájlműveletek futtatása a futó fürt](https://msdn.microsoft.com/library/azure/mt668441.aspx).</span><span class="sxs-lookup"><span data-stu-id="75495-334">See [Run Script Actions on a running cluster](https://msdn.microsoft.com/library/azure/mt668441.aspx).</span></span>

### <a name="apply-a-script-action-tooa-running-cluster-from-hello-hdinsight-net-sdk"></a><span data-ttu-id="75495-335">A fürt hello HDInsight .NET SDK-ről fut parancsfájlművelet tooa alkalmazása</span><span class="sxs-lookup"><span data-stu-id="75495-335">Apply a Script Action tooa running cluster from hello HDInsight .NET SDK</span></span>

<span data-ttu-id="75495-336">Például egy hello .NET SDK tooapply parancsfájlok tooa fürt használatával, lásd: [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).</span><span class="sxs-lookup"><span data-stu-id="75495-336">For an example of using hello .NET SDK tooapply scripts tooa cluster, see [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).</span></span>

## <a name="view-history-promote-and-demote-script-actions"></a><span data-ttu-id="75495-337">Előzményeinek megtekintése, előléptetése és lefokozása Parancsfájlműveletek</span><span class="sxs-lookup"><span data-stu-id="75495-337">View history, promote, and demote Script Actions</span></span>

### <a name="using-hello-azure-portal"></a><span data-ttu-id="75495-338">Hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="75495-338">Using hello Azure portal</span></span>

1. <span data-ttu-id="75495-339">A hello [Azure-portálon](https://portal.azure.com), válassza ki a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="75495-339">From hello [Azure portal](https://portal.azure.com), select your HDInsight cluster.</span></span>

2. <span data-ttu-id="75495-340">Hello HDInsight fürt áttekintése, válassza ki hello **Parancsfájlműveletek** csempére.</span><span class="sxs-lookup"><span data-stu-id="75495-340">From hello HDInsight cluster overview, select hello **Script Actions** tile.</span></span>

    ![Parancsfájl műveletek csempe](./media/hdinsight-hadoop-customize-cluster-linux/scriptactionstile.png)

   > [!NOTE]
   > <span data-ttu-id="75495-342">Igény szerint kiválaszthatja **összes beállítás** , és válassza **Parancsfájlműveletek** a hello beállítások szakaszban.</span><span class="sxs-lookup"><span data-stu-id="75495-342">You can also select **All settings** and then select **Script Actions** from hello Settings section.</span></span>

4. <span data-ttu-id="75495-343">Ehhez a fürthöz parancsfájlok előzményeit hello Parancsfájlműveletek szakaszban látható.</span><span class="sxs-lookup"><span data-stu-id="75495-343">A history of scripts for this cluster is displayed on hello Script Actions section.</span></span> <span data-ttu-id="75495-344">Ezen információk közé tartozik a megőrzött parancsfájlok listáját.</span><span class="sxs-lookup"><span data-stu-id="75495-344">This information includes a list of persisted scripts.</span></span> <span data-ttu-id="75495-345">A hello alábbi képernyőképen látható megtekintheti, hogy ezen a fürtön futtatott parancsfájl lett Solr hello.</span><span class="sxs-lookup"><span data-stu-id="75495-345">In hello screenshot below, you can see that hello Solr script has been ran on this cluster.</span></span> <span data-ttu-id="75495-346">képernyőfelvétel a hello nem szerepelnek a megőrzött parancsfájlokat.</span><span class="sxs-lookup"><span data-stu-id="75495-346">hello screenshot does not show any persisted scripts.</span></span>

    ![Parancsfájl-műveletek szakasz](./media/hdinsight-hadoop-customize-cluster-linux/script-action-history.png)

5. <span data-ttu-id="75495-348">Hello tulajdonságok szakaszának ehhez a parancsprogramhoz parancsfájl kijelölésével hello előzményeit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="75495-348">Selecting a script from hello history displays hello Properties section for this script.</span></span> <span data-ttu-id="75495-349">A hello felső az üdvözlő képernyőt futtassa újra a hello parancsfájl, vagy előléptetheti.</span><span class="sxs-lookup"><span data-stu-id="75495-349">From hello top of hello screen, you can rerun hello script or promote it.</span></span>

    ![Parancsfájl műveletek tulajdonságai](./media/hdinsight-hadoop-customize-cluster-linux/promote-script-actions.png)

6. <span data-ttu-id="75495-351">Is használhatja a hello **...**  toohello sarkában hello Parancsfájlműveletek szakasz tooperform műveletek bejegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="75495-351">You can also use hello **...** toohello right of entries on hello Script Actions section tooperform actions.</span></span>

    ![Parancsfájl-műveletek... kihasználtsága](./media/hdinsight-hadoop-customize-cluster-linux/deletepromoted.png)

### <a name="using-azure-powershell"></a><span data-ttu-id="75495-353">Az Azure PowerShell használata</span><span class="sxs-lookup"><span data-stu-id="75495-353">Using Azure PowerShell</span></span>

| <span data-ttu-id="75495-354">Használja a hello következő...</span><span class="sxs-lookup"><span data-stu-id="75495-354">Use hello following...</span></span> | <span data-ttu-id="75495-355">túl...</span><span class="sxs-lookup"><span data-stu-id="75495-355">too...</span></span> |
| --- | --- |
| <span data-ttu-id="75495-356">Get-AzureRmHDInsightPersistedScriptAction</span><span class="sxs-lookup"><span data-stu-id="75495-356">Get-AzureRmHDInsightPersistedScriptAction</span></span> |<span data-ttu-id="75495-357">A megőrzött Parancsfájlműveletek információk beolvasása</span><span class="sxs-lookup"><span data-stu-id="75495-357">Retrieve information on persisted script actions</span></span> |
| <span data-ttu-id="75495-358">Get-AzureRmHDInsightScriptActionHistory</span><span class="sxs-lookup"><span data-stu-id="75495-358">Get-AzureRmHDInsightScriptActionHistory</span></span> |<span data-ttu-id="75495-359">Parancsfájl műveletek alkalmazása toohello fürt, vagy egy adott parancsfájl részletes előzményeit beolvasása</span><span class="sxs-lookup"><span data-stu-id="75495-359">Retrieve a history of script actions applied toohello cluster, or details for a specific script</span></span> |
| <span data-ttu-id="75495-360">Set-AzureRmHDInsightPersistedScriptAction</span><span class="sxs-lookup"><span data-stu-id="75495-360">Set-AzureRmHDInsightPersistedScriptAction</span></span> |<span data-ttu-id="75495-361">Az ad hoc elősegíthető parancsfájl művelet tooa megőrzött parancsfájl művelet</span><span class="sxs-lookup"><span data-stu-id="75495-361">Promotes an ad hoc script action tooa persisted script action</span></span> |
| <span data-ttu-id="75495-362">Remove-AzureRmHDInsightPersistedScriptAction</span><span class="sxs-lookup"><span data-stu-id="75495-362">Remove-AzureRmHDInsightPersistedScriptAction</span></span> |<span data-ttu-id="75495-363">Úgy fokozza le egy megőrzött parancsfájl művelet tooan alkalmi művelet</span><span class="sxs-lookup"><span data-stu-id="75495-363">Demotes a persisted script action tooan ad hoc action</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="75495-364">Használatával `Remove-AzureRmHDInsightPersistedScriptAction` nem vonja vissza a parancsfájl által végrehajtott hello műveleteket.</span><span class="sxs-lookup"><span data-stu-id="75495-364">Using `Remove-AzureRmHDInsightPersistedScriptAction` does not undo hello actions performed by a script.</span></span> <span data-ttu-id="75495-365">Ez a parancsmag csak eltávolítja hello megőrzött jelzőt.</span><span class="sxs-lookup"><span data-stu-id="75495-365">This cmdlet only removes hello persisted flag.</span></span>

<span data-ttu-id="75495-366">a példaként megadott parancsfájlt a következő hello hello parancsmagok toopromote segítségével bemutatja, majd egy parancsfájl fokozni.</span><span class="sxs-lookup"><span data-stu-id="75495-366">hello following example script demonstrates using hello cmdlets toopromote, then demote a script.</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=123-140)]

### <a name="using-hello-azure-cli"></a><span data-ttu-id="75495-367">Hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="75495-367">Using hello Azure CLI</span></span>

| <span data-ttu-id="75495-368">Használja a hello következő...</span><span class="sxs-lookup"><span data-stu-id="75495-368">Use hello following...</span></span> | <span data-ttu-id="75495-369">túl...</span><span class="sxs-lookup"><span data-stu-id="75495-369">too...</span></span> |
| --- | --- |
| `azure hdinsight script-action persisted list <clustername>` |<span data-ttu-id="75495-370">A megőrzött Parancsfájlműveletek listájának beolvasása</span><span class="sxs-lookup"><span data-stu-id="75495-370">Retrieve a list of persisted script actions</span></span> |
| `azure hdinsight script-action persisted show <clustername> <scriptname>` |<span data-ttu-id="75495-371">Egy adott megőrzött parancsfájlművelet információk beolvasása</span><span class="sxs-lookup"><span data-stu-id="75495-371">Retrieve information on a specific persisted script action</span></span> |
| `azure hdinsight script-action history list <clustername>` |<span data-ttu-id="75495-372">Parancsfájl műveletek alkalmazása toohello fürt előzményeit beolvasása</span><span class="sxs-lookup"><span data-stu-id="75495-372">Retrieve a history of script actions applied toohello cluster</span></span> |
| `azure hdinsight script-action history show <clustername> <scriptname>` |<span data-ttu-id="75495-373">Egy adott parancsfájlművelet információk beolvasása</span><span class="sxs-lookup"><span data-stu-id="75495-373">Retrieve information on a specific script action</span></span> |
| `azure hdinsight script action persisted set <clustername> <scriptexecutionid>` |<span data-ttu-id="75495-374">Az ad hoc elősegíthető parancsfájl művelet tooa megőrzött parancsfájl művelet</span><span class="sxs-lookup"><span data-stu-id="75495-374">Promotes an ad hoc script action tooa persisted script action</span></span> |
| `azure hdinsight script-action persisted delete <clustername> <scriptname>` |<span data-ttu-id="75495-375">Úgy fokozza le egy megőrzött parancsfájl művelet tooan alkalmi művelet</span><span class="sxs-lookup"><span data-stu-id="75495-375">Demotes a persisted script action tooan ad hoc action</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="75495-376">Használatával `azure hdinsight script-action persisted delete` nem vonja vissza a parancsfájl által végrehajtott hello műveleteket.</span><span class="sxs-lookup"><span data-stu-id="75495-376">Using `azure hdinsight script-action persisted delete` does not undo hello actions performed by a script.</span></span> <span data-ttu-id="75495-377">Ez a parancsmag csak eltávolítja hello megőrzött jelzőt.</span><span class="sxs-lookup"><span data-stu-id="75495-377">This cmdlet only removes hello persisted flag.</span></span>

### <a name="using-hello-hdinsight-net-sdk"></a><span data-ttu-id="75495-378">Hello HDInsight .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="75495-378">Using hello HDInsight .NET SDK</span></span>

<span data-ttu-id="75495-379">Például egy hello .NET SDK tooretrieve parancsfájl előzmények fürtök használata, lépteti elő vagy parancsfájlok fokozni, lásd: [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).</span><span class="sxs-lookup"><span data-stu-id="75495-379">For an example of using hello .NET SDK tooretrieve script history from a cluster, promote or demote scripts, see [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).</span></span>

> [!NOTE]
> <span data-ttu-id="75495-380">Ez a példa is mutatja be, hogyan egy HDInsight alkalmazást, amely tooinstall hello .NET SDK-val.</span><span class="sxs-lookup"><span data-stu-id="75495-380">This example also demonstrates how tooinstall an HDInsight application using hello .NET SDK.</span></span>

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a><span data-ttu-id="75495-381">A HDInsight-fürtökön használt nyílt forráskódú szoftvereket támogatása</span><span class="sxs-lookup"><span data-stu-id="75495-381">Support for open-source software used on HDInsight clusters</span></span>

<span data-ttu-id="75495-382">a Microsoft Azure HDInsight szolgáltatás hello egy nyílt forráskódú technológiák formátumú-e körül Hadoop ökoszisztémájának használja.</span><span class="sxs-lookup"><span data-stu-id="75495-382">hello Microsoft Azure HDInsight service uses an ecosystem of open-source technologies formed around Hadoop.</span></span> <span data-ttu-id="75495-383">A Microsoft Azure támogatási általános szintű nyílt forráskódú technológiák nyújt.</span><span class="sxs-lookup"><span data-stu-id="75495-383">Microsoft Azure provides a general level of support for open-source technologies.</span></span> <span data-ttu-id="75495-384">További információkért lásd: hello **támogatási hatókör** hello szakasza [Azure támogatás – gyakori kérdések webhely](https://azure.microsoft.com/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="75495-384">For more information, see hello **Support Scope** section of hello [Azure Support FAQ website](https://azure.microsoft.com/support/faq/).</span></span> <span data-ttu-id="75495-385">hello HDInsight-szolgáltatás egy további szintű támogatást biztosít a beépített összetevők.</span><span class="sxs-lookup"><span data-stu-id="75495-385">hello HDInsight service provides an additional level of support for built-in components.</span></span>

<span data-ttu-id="75495-386">Nyílt forráskódú összetevők hello HDInsight szolgáltatás elérhető két típusa van:</span><span class="sxs-lookup"><span data-stu-id="75495-386">There are two types of open-source components that are available in hello HDInsight service:</span></span>

* <span data-ttu-id="75495-387">**A beépített összetevők** -ezeket az összetevőket a HDInsight-fürtök előre telepített, és alapfunkciókat hello fürt adja meg.</span><span class="sxs-lookup"><span data-stu-id="75495-387">**Built-in components** - These components are pre-installed on HDInsight clusters and provide core functionality of hello cluster.</span></span> <span data-ttu-id="75495-388">YARN ResourceManager, hello Hive query language (HiveQL) és hello Mahout könyvtár például toothis kategóriába tartozik.</span><span class="sxs-lookup"><span data-stu-id="75495-388">For example, YARN ResourceManager, hello Hive query language (HiveQL), and hello Mahout library belong toothis category.</span></span> <span data-ttu-id="75495-389">A kiszolgálófürt-összetevők teljes listáját a érhető [What's new in HDInsight által biztosított hello Hadoop-fürt verziók](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="75495-389">A full list of cluster components is available in [What's new in hello Hadoop cluster versions provided by HDInsight](hdinsight-component-versioning.md).</span></span>
* <span data-ttu-id="75495-390">**Egyéni összetevők** -hello fürt felhasználóként, telepítése vagy használata előtt a munkaterhelésnek valamelyik összetevő a hello közösségi vagy Ön által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="75495-390">**Custom components** - You, as a user of hello cluster, can install or use in your workload any component available in hello community or created by you.</span></span>

> [!WARNING]
> <span data-ttu-id="75495-391">HDInsight-fürt hello összetevői teljes mértékben támogatottak.</span><span class="sxs-lookup"><span data-stu-id="75495-391">Components provided with hello HDInsight cluster are fully supported.</span></span> <span data-ttu-id="75495-392">Microsoft Support tooisolate segít, és a kapcsolódó toothese összetevők problémák megoldásához.</span><span class="sxs-lookup"><span data-stu-id="75495-392">Microsoft Support helps tooisolate and resolve issues related toothese components.</span></span>
>
> <span data-ttu-id="75495-393">Egyéni összetevők kap minden üzleti szempontból ésszerű támogatási toohelp meg toofurther hello problémával kapcsolatos hibaelhárítás elősegítéséhez.</span><span class="sxs-lookup"><span data-stu-id="75495-393">Custom components receive commercially reasonable support toohelp you toofurther troubleshoot hello issue.</span></span> <span data-ttu-id="75495-394">A Microsoft támogatási képes tooresolve hello probléma vagy azok kérheti tooengage elérhető csatorna a hello nyílt forráskódú technológiák részletes segítséget, hogy a technológiát találhatók.</span><span class="sxs-lookup"><span data-stu-id="75495-394">Microsoft support may be able tooresolve hello issue OR they may ask you tooengage available channels for hello open source technologies where deep expertise for that technology is found.</span></span> <span data-ttu-id="75495-395">Például nincsenek sok közösségi webhelyek használható, például: [MSDN fórum hdinsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Is Apache projektek rendelkezik projekt helyek [http://apache.org](http://apache.org), például: [Hadoop](http://hadoop.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="75495-395">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/).</span></span>

<span data-ttu-id="75495-396">HDInsight-szolgáltatás hello toouse egyéni összetevők számos lehetőséget biztosít.</span><span class="sxs-lookup"><span data-stu-id="75495-396">hello HDInsight service provides several ways toouse custom components.</span></span> <span data-ttu-id="75495-397">hello azonos szintű támogatást vonatkoznak, függetlenül attól, hogyan összetevőt használja, és hello fürtön telepítve.</span><span class="sxs-lookup"><span data-stu-id="75495-397">hello same level of support applies, regardless of how a component is used or installed on hello cluster.</span></span> <span data-ttu-id="75495-398">hello alábbi lista ismerteti azokat a hello leggyakoribb módszereket, hogy használható-e az egyéni összetevőkben HDInsight-fürtök:</span><span class="sxs-lookup"><span data-stu-id="75495-398">hello following list describes hello most common ways that custom components can be used on HDInsight clusters:</span></span>

1. <span data-ttu-id="75495-399">Feladat elküldése - Hadoop és egyéb feladatok végrehajtása, vagy használjon egyéni összetevők elküldött toohello tárolófürt is lehet.</span><span class="sxs-lookup"><span data-stu-id="75495-399">Job submission - Hadoop or other types of jobs that execute or use custom components can be submitted toohello cluster.</span></span>

2. <span data-ttu-id="75495-400">Fürt testreszabási - fürt létrehozása során megadhatja további beállításokat és egyéni hello fürtcsomópontokon telepített összetevőket.</span><span class="sxs-lookup"><span data-stu-id="75495-400">Cluster customization - During cluster creation, you can specify additional settings and custom components that are installed on hello cluster nodes.</span></span>

3. <span data-ttu-id="75495-401">Minták – a népszerű egyéni összetevők, a Microsoft és a mások által biztosított mintákat ezeket az összetevőket hogyan használható a hello HDInsight-fürtökön.</span><span class="sxs-lookup"><span data-stu-id="75495-401">Samples - For popular custom components, Microsoft and others may provide samples of how these components can be used on hello HDInsight clusters.</span></span> <span data-ttu-id="75495-402">Ezeket a mintákat támogatás nélkül van megadva.</span><span class="sxs-lookup"><span data-stu-id="75495-402">These samples are provided without support.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="75495-403">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="75495-403">Troubleshooting</span></span>

<span data-ttu-id="75495-404">Ambari webes felhasználói felület tooview által naplózott információ Parancsfájlműveletek is használhatja.</span><span class="sxs-lookup"><span data-stu-id="75495-404">You can use Ambari web UI tooview information logged by script actions.</span></span> <span data-ttu-id="75495-405">Hello parancsfájlt a fürt létrehozása során nem sikerül, ha hello naplókat is rendelkezésre állnak a hello hello-fürthöz tartozó alapértelmezett tárfiók.</span><span class="sxs-lookup"><span data-stu-id="75495-405">If hello script fails during cluster creation, hello logs are also available in hello default storage account associated with hello cluster.</span></span> <span data-ttu-id="75495-406">Ebben a szakaszban megtudhatja hogyan tooretrieve hello naplózza, mindkét ezen beállítások használatával.</span><span class="sxs-lookup"><span data-stu-id="75495-406">This section provides information on how tooretrieve hello logs using both these options.</span></span>

### <a name="using-hello-ambari-web-ui"></a><span data-ttu-id="75495-407">Hello Ambari webes felhasználói felület használatával</span><span class="sxs-lookup"><span data-stu-id="75495-407">Using hello Ambari Web UI</span></span>

1. <span data-ttu-id="75495-408">A böngészőben nyissa meg a toohttps://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="75495-408">In your browser, navigate toohttps://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="75495-409">CLUSTERNAME cserélje le a HDInsight fürt hello neve.</span><span class="sxs-lookup"><span data-stu-id="75495-409">Replace CLUSTERNAME with hello name of your HDInsight cluster.</span></span>

    <span data-ttu-id="75495-410">Amikor a rendszer kéri, adja meg (rendszergazda) hello admin fióknevet és jelszót hello fürt.</span><span class="sxs-lookup"><span data-stu-id="75495-410">When prompted, enter hello admin account name (admin) and password for hello cluster.</span></span> <span data-ttu-id="75495-411">Előfordulhat, hogy tooreenter hello rendszergazdai hitelesítő adatokat egy webes űrlapon.</span><span class="sxs-lookup"><span data-stu-id="75495-411">You may have tooreenter hello admin credentials in a web form.</span></span>

2. <span data-ttu-id="75495-412">Hello sorából hello oldal hello tetején, válassza ki a hello **ops** bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="75495-412">From hello bar at hello top of hello page, select hello **ops** entry.</span></span> <span data-ttu-id="75495-413">Hello fürtön Ambari keresztül végrehajtott jelenlegi és korábbi műveletek listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="75495-413">A list of current and previous operations performed on hello cluster through Ambari is displayed.</span></span>

    ![Ambari webes felhasználói felület sávon kicserélhet kijelölt ops](./media/hdinsight-hadoop-customize-cluster-linux/ambari-nav.png)

3. <span data-ttu-id="75495-415">Keresés hello bejegyzéseket **futtatása\_customscriptaction** a hello **műveletek** oszlop.</span><span class="sxs-lookup"><span data-stu-id="75495-415">Find hello entries that have **run\_customscriptaction** in hello **Operations** column.</span></span> <span data-ttu-id="75495-416">Ezek a bejegyzések hello Parancsfájlműveletek futásakor jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="75495-416">These entries are created when hello Script Actions run.</span></span>

    ![Képernyőkép a műveletek](./media/hdinsight-hadoop-customize-cluster-linux/ambariscriptaction.png)

    <span data-ttu-id="75495-418">tooview hello STDOUT és az STDERR kimeneti, jelöljön ki hello run\customscriptaction bejegyzést, és hello található hivatkozásokon keresztül részletekbe menően tárhatják.</span><span class="sxs-lookup"><span data-stu-id="75495-418">tooview hello STDOUT and STDERR output, select hello run\customscriptaction entry and drill down through hello links.</span></span> <span data-ttu-id="75495-419">A kimenet jön létre, amikor hello parancsfájl fut, és hasznos információt tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="75495-419">This output is generated when hello script runs, and may contain useful information.</span></span>

### <a name="access-logs-from-hello-default-storage-account"></a><span data-ttu-id="75495-420">Az alapértelmezett tárfiók hello belépési naplók</span><span class="sxs-lookup"><span data-stu-id="75495-420">Access logs from hello default storage account</span></span>

<span data-ttu-id="75495-421">Hello fürt létrehozása tooa parancsfájl műveleti hiba miatt nem sikerül, ha hello naplók elérhetők hello fürt tárfiók.</span><span class="sxs-lookup"><span data-stu-id="75495-421">If hello cluster creation fails due tooa script action error, hello logs can be accessed from hello cluster storage account.</span></span>

* <span data-ttu-id="75495-422">hello tárolási érhetők el naplók a `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE`.</span><span class="sxs-lookup"><span data-stu-id="75495-422">hello storage logs are available at `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE`.</span></span>

    ![Képernyőkép a műveletek](./media/hdinsight-hadoop-customize-cluster-linux/script_action_logs_in_storage.png)

    <span data-ttu-id="75495-424">Ebben a könyvtárban, a hello naplók vannak rendszerezve külön-külön headnode, workernode és zookeeper csomópontok.</span><span class="sxs-lookup"><span data-stu-id="75495-424">Under this directory, hello logs are organized separately for headnode, workernode, and zookeeper nodes.</span></span> <span data-ttu-id="75495-425">Néhány példa:</span><span class="sxs-lookup"><span data-stu-id="75495-425">Some examples are:</span></span>

    * <span data-ttu-id="75495-426">**Headnode** - `<uniqueidentifier>AmbariDb-hn0-<generated_value>.cloudapp.net`</span><span class="sxs-lookup"><span data-stu-id="75495-426">**Headnode** - `<uniqueidentifier>AmbariDb-hn0-<generated_value>.cloudapp.net`</span></span>

    * <span data-ttu-id="75495-427">**Munkavégző csomópont** - `<uniqueidentifier>AmbariDb-wn0-<generated_value>.cloudapp.net`</span><span class="sxs-lookup"><span data-stu-id="75495-427">**Worker node** - `<uniqueidentifier>AmbariDb-wn0-<generated_value>.cloudapp.net`</span></span>

    * <span data-ttu-id="75495-428">**Zookeeper csomópont** - `<uniqueidentifier>AmbariDb-zk0-<generated_value>.cloudapp.net`</span><span class="sxs-lookup"><span data-stu-id="75495-428">**Zookeeper node** - `<uniqueidentifier>AmbariDb-zk0-<generated_value>.cloudapp.net`</span></span>

* <span data-ttu-id="75495-429">Az stdout és az stderr hello megfelelő gazdagép toohello tárfiók feltöltve.</span><span class="sxs-lookup"><span data-stu-id="75495-429">All stdout and stderr of hello corresponding host is uploaded toohello storage account.</span></span> <span data-ttu-id="75495-430">Van egy **kimeneti -\*.txt** és **hibák -\*.txt** parancsfájl műveleteket.</span><span class="sxs-lookup"><span data-stu-id="75495-430">There is one **output-\*.txt** and **errors-\*.txt** for each script action.</span></span> <span data-ttu-id="75495-431">hello kimeneti-*.txt fájlt kapott hello gazdagépen futó hello parancsfájl hello URI információt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="75495-431">hello output-*.txt file contains information about hello URI of hello script that got run on hello host.</span></span> <span data-ttu-id="75495-432">Példa:</span><span class="sxs-lookup"><span data-stu-id="75495-432">For example</span></span>

        'Start downloading script locally: ', u'https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh'

* <span data-ttu-id="75495-433">Lehetséges, hogy meg ismételten hozzon létre egy parancsfájl művelet fürtöt hello ugyanazzal a névvel.</span><span class="sxs-lookup"><span data-stu-id="75495-433">It's possible that you repeatedly create a script action cluster with hello same name.</span></span> <span data-ttu-id="75495-434">Ilyen esetben különböztetheti meg egymástól a naplók megfelelő hello hello dátum mappa neve alapján.</span><span class="sxs-lookup"><span data-stu-id="75495-434">In such case, you can distinguish hello relevant logs based on hello DATE folder name.</span></span> <span data-ttu-id="75495-435">Például hello mappaszerkezet különböző időpontokban létrehozott fürt (sajátfürt) jelenik meg, a következő naplóbejegyzések hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="75495-435">For example, hello folder structure for a cluster (mycluster) created on different dates appears similar toohello following log entries:</span></span>

    <span data-ttu-id="75495-436">`\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04` `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`</span><span class="sxs-lookup"><span data-stu-id="75495-436">`\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04` `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`</span></span>

* <span data-ttu-id="75495-437">Ha egy parancsfájl művelet fürtöt hoz létre hello azonos név a hello azonos nap, hello egyedi előtag tooidentify hello kapcsolatos fontos naplófájlokat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="75495-437">If you create a script action cluster with hello same name on hello same day, you can use hello unique prefix tooidentify hello relevant log files.</span></span>

* <span data-ttu-id="75495-438">12:00 -kor (éjfél) mellett a fürt létrehozása, akkor lehetséges, hogy hello naplófájlok span két napon keresztül.</span><span class="sxs-lookup"><span data-stu-id="75495-438">If you create a cluster near 12:00AM (midnight), it's possible that hello log files span across two days.</span></span> <span data-ttu-id="75495-439">Ilyen esetben két külön dátummal mappát hello láthat ugyanabban a fürtben.</span><span class="sxs-lookup"><span data-stu-id="75495-439">In such cases, you see two different date folders for hello same cluster.</span></span>

* <span data-ttu-id="75495-440">Feltöltése naplózási fájlok toohello alapértelmezett tároló too5 perc, különösen olyan nagyméretű fürtök is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="75495-440">Uploading log files toohello default container can take up too5 mins, especially for large clusters.</span></span> <span data-ttu-id="75495-441">Ezért ha azt szeretné, hogy tooaccess hello naplókat, törölni kell, nem azonnal hello fürt Ha egy parancsfájl művelet sikertelen.</span><span class="sxs-lookup"><span data-stu-id="75495-441">So, if you want tooaccess hello logs, you should not immediately delete hello cluster if a script action fails.</span></span>

### <a name="ambari-watchdog"></a><span data-ttu-id="75495-442">Ambari figyelő</span><span class="sxs-lookup"><span data-stu-id="75495-442">Ambari watchdog</span></span>

> [!WARNING]
> <span data-ttu-id="75495-443">Ne változtassa meg a Linux-alapú HDInsight-fürtök az Ambari figyelő (hdinsightwatchdog) hello hello jelszavát.</span><span class="sxs-lookup"><span data-stu-id="75495-443">Do not change hello password for hello Ambari Watchdog (hdinsightwatchdog) on your Linux-based HDInsight cluster.</span></span> <span data-ttu-id="75495-444">Változó hello jelszót a fiókhoz tartozó hello képességét toorun új Parancsfájlműveletek hello HDInsight-fürt megszakítja.</span><span class="sxs-lookup"><span data-stu-id="75495-444">Changing hello password for this account breaks hello ability toorun new script actions on hello HDInsight cluster.</span></span>

### <a name="cant-import-name-blobservice"></a><span data-ttu-id="75495-445">BlobService neve nem lehet importálni.</span><span class="sxs-lookup"><span data-stu-id="75495-445">Can't import name BlobService</span></span>

<span data-ttu-id="75495-446">__A jelenség__: hello parancsfájl művelet sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="75495-446">__Symptoms__: hello script action fails.</span></span> <span data-ttu-id="75495-447">Hiba a következő szöveg hasonló toohello Ambari hello művelet megtekintésekor jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="75495-447">Text similar toohello following error is displayed when you view hello operation in Ambari:</span></span>

```
Traceback (most recent call list):
  File "/var/lib/ambari-agent/cache/custom_actions/scripts/run_customscriptaction.py", line 21, in <module>
    from azure.storage.blob import BlobService
ImportError: cannot import name BlobService
```

<span data-ttu-id="75495-448">__OK__: Ez a hiba akkor fordul elő, ha hello HDInsight-fürt részét képező hello Python Azure Storage ügyfelet frissít.</span><span class="sxs-lookup"><span data-stu-id="75495-448">__Cause__: This error occurs if you upgrade hello Python Azure Storage client that is included with hello HDInsight cluster.</span></span> <span data-ttu-id="75495-449">A HDInsight az Azure Storage ügyfél 0.20.0 vár.</span><span class="sxs-lookup"><span data-stu-id="75495-449">HDInsight expects Azure Storage client 0.20.0.</span></span>

<span data-ttu-id="75495-450">__Megoldási__: tooresolve hibára, manuálisan a tooeach fürt csomópont használatával csatlakozzon `ssh` és a következő parancs tooreinstall hello megfelelő tárolási ügyfélverzió hello használata:</span><span class="sxs-lookup"><span data-stu-id="75495-450">__Resolution__: tooresolve this error, manually connect tooeach cluster node using `ssh` and use hello following command tooreinstall hello correct storage client version:</span></span>

```
sudo pip install azure-storage==0.20.0
```

<span data-ttu-id="75495-451">Az SSH csatlakozó toohello fürtön információkért lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="75495-451">For information on connecting toohello cluster with SSH, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

### <a name="history-doesnt-show-scripts-used-during-cluster-creation"></a><span data-ttu-id="75495-452">Előzmények nem jelenik meg a fürt létrehozása során használt parancsfájlok</span><span class="sxs-lookup"><span data-stu-id="75495-452">History doesn't show scripts used during cluster creation</span></span>

<span data-ttu-id="75495-453">Ha a fürt létrehozása előtt 2016. március 15-én nem jelenhet meg a parancsfájlművelet előzményeinek bejegyzése.</span><span class="sxs-lookup"><span data-stu-id="75495-453">If your cluster was created before March 15, 2016, you may not see an entry in Script Action history.</span></span> <span data-ttu-id="75495-454">2016. március 15-én után átméretezésekor hello fürt hello parancsfájlok használata a fürt létrehozása során jelennek meg előzmények, a rendszer alkalmazza azokat hello fürt hello részeként toonew csomópontja átméretezése műveletet.</span><span class="sxs-lookup"><span data-stu-id="75495-454">If you resize hello cluster after March 15, 2016, hello scripts using during cluster creation appear in history as they are applied toonew nodes in hello cluster as part of hello resize operation.</span></span>

<span data-ttu-id="75495-455">Ez alól két kivétel van:</span><span class="sxs-lookup"><span data-stu-id="75495-455">There are two exceptions:</span></span>

* <span data-ttu-id="75495-456">Ha a fürt 2015. szeptember 1. előtt létrehozott.</span><span class="sxs-lookup"><span data-stu-id="75495-456">If your cluster was created before September 1, 2015.</span></span> <span data-ttu-id="75495-457">Ez a dátum esetén a Parancsfájlműveletek lett bevezetve.</span><span class="sxs-lookup"><span data-stu-id="75495-457">This date is when Script Actions were introduced.</span></span> <span data-ttu-id="75495-458">Ez a dátum előtt létrehozott fürthöz sikerült nem használta a Parancsfájlműveletek a fürt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="75495-458">Any cluster created before this date could not have used Script Actions for cluster creation.</span></span>

* <span data-ttu-id="75495-459">Ha több Parancsfájlműveletek fürt létrehozása során használt, és azonos több parancsfájl nevet, vagy hello hello használt azonos nevet, ugyanilyen URI, de több parancsfájl különböző paramétereit.</span><span class="sxs-lookup"><span data-stu-id="75495-459">If you used multiple Script Actions during cluster creation, and used hello same name for multiple scripts, or hello same name, same URI, but different parameters for multiple scripts.</span></span> <span data-ttu-id="75495-460">Ezekben az esetekben hello a következő hiba jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="75495-460">In these cases, you receive hello following error:</span></span>

    <span data-ttu-id="75495-461">Nem lehet műveleteket új parancsfájl lefutott, tooconflicting parancsfájl nevét a meglévő parancsfájlok miatt ezen a fürtön.</span><span class="sxs-lookup"><span data-stu-id="75495-461">No new script actions can be ran on this cluster due tooconflicting script names in existing scripts.</span></span> <span data-ttu-id="75495-462">Fürt elérhető parancsfájl nevét kell létrehozni minden egyedi lehet.</span><span class="sxs-lookup"><span data-stu-id="75495-462">Script names provided at cluster create must be all unique.</span></span> <span data-ttu-id="75495-463">Meglévő parancsfájlok megbízhatóságához ablakhoz.</span><span class="sxs-lookup"><span data-stu-id="75495-463">Existing scripts are ran on resize.</span></span>

## <a name="next-steps"></a><span data-ttu-id="75495-464">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="75495-464">Next steps</span></span>

* [<span data-ttu-id="75495-465">A HDInsight parancsfájlművelet-parancsfájlok fejlesztése</span><span class="sxs-lookup"><span data-stu-id="75495-465">Develop Script Action scripts for HDInsight</span></span>](hdinsight-hadoop-script-actions-linux.md)
* [<span data-ttu-id="75495-466">Telepítheti és használhatja a HDInsight-fürtök Solr</span><span class="sxs-lookup"><span data-stu-id="75495-466">Install and use Solr on HDInsight clusters</span></span>](hdinsight-hadoop-solr-install-linux.md)
* [<span data-ttu-id="75495-467">Telepítheti és használhatja a HDInsight-fürtök Giraph</span><span class="sxs-lookup"><span data-stu-id="75495-467">Install and use Giraph on HDInsight clusters</span></span>](hdinsight-hadoop-giraph-install-linux.md)
* [<span data-ttu-id="75495-468">További tárhely tooan HDInsight-fürt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="75495-468">Add additional storage tooan HDInsight cluster</span></span>](hdinsight-hadoop-add-storage.md)

[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster-linux/HDI-Cluster-state.png "Fürt létrehozása során szakaszból"
