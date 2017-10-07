---
title: "az Azure Service Fabric-fürt aaaUpgrade |} Microsoft Docs"
description: "Frissítés hello Service Fabric-kód és/vagy konfiguráció, amelyen a Service Fabric-fürt, beállítása a fürt frissítési üzemmódban van; tanúsítványok hozzáadása az alkalmazás-porttal rendelkezik, ennek során az operációsrendszer-javítások, frissítése, és így tovább. Mi a számíthat, ha hello frissítések történik?"
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 15190ace-31ed-491f-a54b-b5ff61e718db
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/10/2017
ms.author: chackdan
ms.openlocfilehash: 94ac3833ec0810f79de06ecb50f254028fa90408
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-an-azure-service-fabric-cluster"></a><span data-ttu-id="ac904-104">Az Azure Service Fabric-fürt frissítése</span><span class="sxs-lookup"><span data-stu-id="ac904-104">Upgrade an Azure Service Fabric cluster</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ac904-105">Azure-fürttel</span><span class="sxs-lookup"><span data-stu-id="ac904-105">Azure Cluster</span></span>](service-fabric-cluster-upgrade.md)
> * [<span data-ttu-id="ac904-106">Önálló fürthöz</span><span class="sxs-lookup"><span data-stu-id="ac904-106">Standalone Cluster</span></span>](service-fabric-cluster-upgrade-windows-server.md)
> 
> 

<span data-ttu-id="ac904-107">Bármely, modern rendszer megtervezése a bővíthetőség kulcs tooachieving hosszú távú sikert a termék.</span><span class="sxs-lookup"><span data-stu-id="ac904-107">For any modern system, designing for upgradability is key tooachieving long-term success of your product.</span></span> <span data-ttu-id="ac904-108">Az Azure Service Fabric-fürt, amely a saját, de részben Microsoft által felügyelt erőforrás.</span><span class="sxs-lookup"><span data-stu-id="ac904-108">An Azure Service Fabric cluster is a resource that you own, but is partly managed by Microsoft.</span></span> <span data-ttu-id="ac904-109">A cikkből megtudhatja, mi automatikusan kezeli, és mi beállíthatja saját maga.</span><span class="sxs-lookup"><span data-stu-id="ac904-109">This article describes what is managed automatically and what you can configure yourself.</span></span>

## <a name="controlling-hello-fabric-version-that-runs-on-your-cluster"></a><span data-ttu-id="ac904-110">A fürtön futó hello fabric-verzió vezérlése</span><span class="sxs-lookup"><span data-stu-id="ac904-110">Controlling hello fabric version that runs on your Cluster</span></span>
<span data-ttu-id="ac904-111">Beállíthatja a fürt tooreceive automatikus háló frissítések, azokat a Microsoft által kiadott, vagy válassza ki a fürt toobe egy támogatott fabric-verzió.</span><span class="sxs-lookup"><span data-stu-id="ac904-111">You can set your cluster tooreceive automatic fabric upgrades as they are released by Microsoft or you can select a supported fabric version you want your cluster toobe on.</span></span>

<span data-ttu-id="ac904-112">Ehhez a beállítás hello "upgrademode érték" fürtkonfiguráció hello portálon vagy az erőforrás-kezelő hello létrehozása során, vagy később egy élő fürthöz</span><span class="sxs-lookup"><span data-stu-id="ac904-112">You do this by setting hello "upgradeMode" cluster configuration on hello portal or using Resource Manager at hello time of creation or later on a live cluster</span></span> 

> [!NOTE]
> <span data-ttu-id="ac904-113">Győződjön meg arról, hogy tookeep a mindig támogatott háló verzióját futtató fürtre.</span><span class="sxs-lookup"><span data-stu-id="ac904-113">Make sure tookeep your cluster running a supported fabric version always.</span></span> <span data-ttu-id="ac904-114">És azt egy új verzióját a service fabric hello kiadása értesítés, hello előző verziója legalább a dátumnak a 60 nap után végéhez van megjelölve.</span><span class="sxs-lookup"><span data-stu-id="ac904-114">As and when we announce hello release of a new version of service fabric, hello previous version is marked for end of support after a minimum of 60 days from that date.</span></span> <span data-ttu-id="ac904-115">hello hello új kiadásokat történik bejelentés [hello szolgáltatás háló csapat blogjában](https://blogs.msdn.microsoft.com/azureservicefabric/).</span><span class="sxs-lookup"><span data-stu-id="ac904-115">hello  hello new releases are announced [on hello service fabric team blog](https://blogs.msdn.microsoft.com/azureservicefabric/).</span></span> <span data-ttu-id="ac904-116">hello új kiadásban elérhető toochoose majd.</span><span class="sxs-lookup"><span data-stu-id="ac904-116">hello new release is available toochoose then.</span></span> 
> 
> 

<span data-ttu-id="ac904-117">14 napja előzetes toohello lejárati hello kiadás a fürt fut, a health esemény jön létre, amely a fürt elhelyezi a figyelmeztetési állapot.</span><span class="sxs-lookup"><span data-stu-id="ac904-117">14 days prior toohello expiry of hello release your cluster is running, a health event is generated that puts your cluster into a warning health state.</span></span> <span data-ttu-id="ac904-118">hello fürt figyelmeztetési állapotban marad, amíg nem frissíti az tooa támogatott fabric-verzió.</span><span class="sxs-lookup"><span data-stu-id="ac904-118">hello cluster remains in a warning state until you upgrade tooa supported fabric version.</span></span>

### <a name="setting-hello-upgrade-mode-via-portal"></a><span data-ttu-id="ac904-119">Hello a portálon keresztül frissítési mód beállítása</span><span class="sxs-lookup"><span data-stu-id="ac904-119">Setting hello upgrade mode via portal</span></span>
<span data-ttu-id="ac904-120">Beállíthat hello fürt tooautomatic vagy manuális hello fürt létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="ac904-120">You can set hello cluster tooautomatic or manual when you are creating hello cluster.</span></span>

![Create_Manualmode][Create_Manualmode]

<span data-ttu-id="ac904-122">Hello fürt tooautomatic állíthatja be, vagy manuális élő fürtön lévő, élmény hello segítségével kezelheti.</span><span class="sxs-lookup"><span data-stu-id="ac904-122">You can set hello cluster tooautomatic or manual when on a live cluster, using hello manage experience.</span></span> 

#### <a name="upgrading-tooa-new-version-on-a-cluster-that-is-set-toomanual-mode-via-portal"></a><span data-ttu-id="ac904-123">Egy fürtön, amelyek portálon keresztül tooManual mód beállítása a tooa új verziójának frissítése.</span><span class="sxs-lookup"><span data-stu-id="ac904-123">Upgrading tooa new version on a cluster that is set tooManual mode via portal.</span></span>
<span data-ttu-id="ac904-124">tooupgrade tooa új verziója, toodo szüksége hello legördülő listából válassza ki a hello elérhető verzióra, és mentse.</span><span class="sxs-lookup"><span data-stu-id="ac904-124">tooupgrade tooa new version, all you need toodo is select hello available version from hello dropdown and save.</span></span> <span data-ttu-id="ac904-125">Fabric-frissítésnek hello lekérdezi kezdődött el automatikusan.</span><span class="sxs-lookup"><span data-stu-id="ac904-125">hello Fabric upgrade gets kicked off automatically.</span></span> <span data-ttu-id="ac904-126">hello fürt állapotházirendeket (kombinációjából csomópont hello egészségügyi összes hello hello fürtben futó alkalmazások és) betartása tooduring hello frissítését.</span><span class="sxs-lookup"><span data-stu-id="ac904-126">hello cluster health policies (a combination of node health and hello health all hello applications running in hello cluster) are adhered tooduring hello upgrade.</span></span>

<span data-ttu-id="ac904-127">Ha a fürt állapotházirendeket hello nem teljesülnek, hello frissítés vissza lesz állítva.</span><span class="sxs-lookup"><span data-stu-id="ac904-127">If hello cluster health policies are not met, hello upgrade is rolled back.</span></span> <span data-ttu-id="ac904-128">Görgessen lefelé a dokumentum további hogyan tooread tooset ezen egyéni házirendek.</span><span class="sxs-lookup"><span data-stu-id="ac904-128">Scroll down this document tooread more on how tooset those custom health policies.</span></span> 

<span data-ttu-id="ac904-129">Hello visszaállítási eredményező hello problémák kijavítása, amennyiben szükséges tooinitiate hello frissítés ebben az esetben ugyanaz, mint korábban lépések következő hello.</span><span class="sxs-lookup"><span data-stu-id="ac904-129">Once you have fixed hello issues that resulted in hello rollback, you need tooinitiate hello upgrade again, by following hello same steps as before.</span></span>

![Manage_Automaticmode][Manage_Automaticmode]

### <a name="setting-hello-upgrade-mode-via-a-resource-manager-template"></a><span data-ttu-id="ac904-131">A Resource Manager-sablon használatával hello a frissítési mód beállítása</span><span class="sxs-lookup"><span data-stu-id="ac904-131">Setting hello upgrade mode via a Resource Manager template</span></span>
<span data-ttu-id="ac904-132">Adja hozzá a hello "upgrademode érték" konfigurációs toohello Microsoft.ServiceFabric/clusters erőforrás-definícióban, és set hello "clusterCodeVersion" tooone a hello támogatott fabric-verziók alább látható módon, és hello sablon telepítését.</span><span class="sxs-lookup"><span data-stu-id="ac904-132">Add hello "upgradeMode" configuration toohello Microsoft.ServiceFabric/clusters resource definition and set hello "clusterCodeVersion" tooone of hello supported fabric versions as shown below and then deploy hello template.</span></span> <span data-ttu-id="ac904-133">hello érvényes "upgrademode érték" értékei a "Manual" vagy "Automatikus"</span><span class="sxs-lookup"><span data-stu-id="ac904-133">hello valid values for "upgradeMode" are "Manual" or "Automatic"</span></span>

![ARMUpgradeMode][ARMUpgradeMode]

#### <a name="upgrading-tooa-new-version-on-a-cluster-that-is-set-toomanual-mode-via-a-resource-manager-template"></a><span data-ttu-id="ac904-135">Egy fürthöz, amelyet a Resource Manager-sablon használatával tooManual mód beállítása a tooa új verziójának frissítése.</span><span class="sxs-lookup"><span data-stu-id="ac904-135">Upgrading tooa new version on a cluster that is set tooManual mode via a Resource Manager template.</span></span>
<span data-ttu-id="ac904-136">Manuális üzemmódban tooupgrade tooa új verziója, hello fürt esetén módosítja hello "clusterCodeVersion" tooa támogatott verzióját, és telepítse azt.</span><span class="sxs-lookup"><span data-stu-id="ac904-136">When hello cluster is in Manual mode, tooupgrade tooa new version, change hello "clusterCodeVersion" tooa supported version and deploy it.</span></span> <span data-ttu-id="ac904-137">hello sablon hello fürttelepítésben hello Fabric frissítés megrúgni lekérdezi kezdődött el automatikusan.</span><span class="sxs-lookup"><span data-stu-id="ac904-137">hello deployment of hello template, kicks of hello Fabric upgrade gets kicked off automatically.</span></span> <span data-ttu-id="ac904-138">hello fürt állapotházirendeket (kombinációjából csomópont hello egészségügyi összes hello hello fürtben futó alkalmazások és) betartása tooduring hello frissítését.</span><span class="sxs-lookup"><span data-stu-id="ac904-138">hello cluster health policies (a combination of node health and hello health all hello applications running in hello cluster) are adhered tooduring hello upgrade.</span></span>

<span data-ttu-id="ac904-139">Ha a fürt állapotházirendeket hello nem teljesülnek, hello frissítés vissza lesz állítva.</span><span class="sxs-lookup"><span data-stu-id="ac904-139">If hello cluster health policies are not met, hello upgrade is rolled back.</span></span> <span data-ttu-id="ac904-140">Görgessen lefelé a dokumentum további hogyan tooread tooset ezen egyéni házirendek.</span><span class="sxs-lookup"><span data-stu-id="ac904-140">Scroll down this document tooread more on how tooset those custom health policies.</span></span> 

<span data-ttu-id="ac904-141">Hello visszaállítási eredményező hello problémák kijavítása, amennyiben szükséges tooinitiate hello frissítés ebben az esetben ugyanaz, mint korábban lépések következő hello.</span><span class="sxs-lookup"><span data-stu-id="ac904-141">Once you have fixed hello issues that resulted in hello rollback, you need tooinitiate hello upgrade again, by following hello same steps as before.</span></span>

### <a name="get-list-of-all-available-version-for-all-environments-for-a-given-subscription"></a><span data-ttu-id="ac904-142">Az összes elérhető verzió listáját beszerzése az adott előfizetéshez tartozó minden környezetben</span><span class="sxs-lookup"><span data-stu-id="ac904-142">Get list of all available version for all environments for a given subscription</span></span>
<span data-ttu-id="ac904-143">Futtassa a következő parancs hello, és egy kimeneti hasonló toothis kapja meg.</span><span class="sxs-lookup"><span data-stu-id="ac904-143">Run hello following command, and you should get an output similar toothis.</span></span>

<span data-ttu-id="ac904-144">"supportExpiryUtc" közli a Ha egy adott kiadás lejárati ideje, vagy lejárt.</span><span class="sxs-lookup"><span data-stu-id="ac904-144">"supportExpiryUtc" tells your when a given release is expiring or has expired.</span></span> <span data-ttu-id="ac904-145">hello legújabb kiadás nem rendelkezik érvényes dátum - értéke a "9999-12-31T23:59:59.9999999", amely csak azt jelenti, hogy hello lejárati még nem állították.</span><span class="sxs-lookup"><span data-stu-id="ac904-145">hello latest release does not have a valid date - it has a value of "9999-12-31T23:59:59.9999999", which just means that hello expiry date is not yet set.</span></span>

```REST
GET https://<endpoint>/subscriptions/{{subscriptionId}}/providers/Microsoft.ServiceFabric/locations/{{location}}/clusterVersions?api-version=2016-09-01

Example: https://management.azure.com/subscriptions/1857f442-3bce-4b96-ad95-627f76437a67/providers/Microsoft.ServiceFabric/locations/eastus/clusterVersions?api-version=2016-09-01

Output:
{
                  "value": [
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/5.0.1427.9490",
                      "name": "5.0.1427.9490",
                      "type": "Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.0.1427.9490",
                        "supportExpiryUtc": "2016-11-26T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.0.1427.9490",
                      "name": "5.1.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.1.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.4.1427.9490",
                      "name": "4.4.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "4.4.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Linux"
                      }
                    }
                  ]
                }


```

## <a name="fabric-upgrade-behavior-when-hello-cluster-upgrade-mode-is-automatic"></a><span data-ttu-id="ac904-146">Háló frissítési működés, ha automatikus hello fürt frissítési mód</span><span class="sxs-lookup"><span data-stu-id="ac904-146">Fabric upgrade behavior when hello cluster Upgrade Mode is Automatic</span></span>
<span data-ttu-id="ac904-147">A Microsoft fenntartja a hello háló kódot és az Azure-fürtben lévő futtató konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="ac904-147">Microsoft maintains hello fabric code and configuration that runs in an Azure cluster.</span></span> <span data-ttu-id="ac904-148">Automatikus figyelt frissítéseket toohello szoftver szükség szerint alapon végezzük.</span><span class="sxs-lookup"><span data-stu-id="ac904-148">We perform automatic monitored upgrades toohello software on an as-needed basis.</span></span> <span data-ttu-id="ac904-149">Ezek a frissítések lehet kódot, konfigurációs vagy mindkettőt.</span><span class="sxs-lookup"><span data-stu-id="ac904-149">These upgrades could be code, configuration, or both.</span></span> <span data-ttu-id="ac904-150">a toomake meg arról, hogy az alkalmazás szenved, nincs hatása vagy toothese frissítések miatt gyakorolt minimális hatás mellett, a következő fázisok hello végezzük hello frissítések:</span><span class="sxs-lookup"><span data-stu-id="ac904-150">toomake sure that your application suffers no impact or minimal impact due toothese upgrades, we perform hello upgrades in hello following phases:</span></span>

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a><span data-ttu-id="ac904-151">1. fázis: Frissítés történik az összes fürt házirendek használatával</span><span class="sxs-lookup"><span data-stu-id="ac904-151">Phase 1: An upgrade is performed by using all cluster health policies</span></span>
<span data-ttu-id="ac904-152">Ebben a fázisban hello frissítések folytatható egy frissítési tartományt egyszerre, és hello hello fürtben futó alkalmazások továbbra is toorun állásidő nélkül.</span><span class="sxs-lookup"><span data-stu-id="ac904-152">During this phase, hello upgrades proceed one upgrade domain at a time, and hello applications that were running in hello cluster continue toorun without any downtime.</span></span> <span data-ttu-id="ac904-153">hello fürt állapotházirendeket (kombinációjából csomópont hello egészségügyi összes hello hello fürtben futó alkalmazások és) betartása tooduring hello frissítését.</span><span class="sxs-lookup"><span data-stu-id="ac904-153">hello cluster health policies (a combination of node health and hello health all hello applications running in hello cluster) are adhered tooduring hello upgrade.</span></span>

<span data-ttu-id="ac904-154">Ha a fürt állapotházirendeket hello nem teljesülnek, hello frissítés vissza lesz állítva.</span><span class="sxs-lookup"><span data-stu-id="ac904-154">If hello cluster health policies are not met, hello upgrade is rolled back.</span></span> <span data-ttu-id="ac904-155">Ezután egy e-mailt küld hello előfizetés toohello tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="ac904-155">Then an email is sent toohello owner of hello subscription.</span></span> <span data-ttu-id="ac904-156">hello e-mail hello a következő információkat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="ac904-156">hello email contains hello following information:</span></span>

* <span data-ttu-id="ac904-157">Az értesítési tooroll vissza egy Fürtfrissítés le kellett.</span><span class="sxs-lookup"><span data-stu-id="ac904-157">Notification that we had tooroll back a cluster upgrade.</span></span>
* <span data-ttu-id="ac904-158">Javasolt javító műveleteket, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="ac904-158">Suggested remedial actions, if any.</span></span>
* <span data-ttu-id="ac904-159">hello hány napig (n), amíg azt hajtható végre a 2. szakasza.</span><span class="sxs-lookup"><span data-stu-id="ac904-159">hello number of days (n) until we execute Phase 2.</span></span>

<span data-ttu-id="ac904-160">A Microsoft próbálja tooexecute hello azonos néhány alkalommal frissíteni, abban az esetben, ha a megjelenjenek infrastruktúra okok miatt nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="ac904-160">We try tooexecute hello same upgrade a few more times in case any upgrades failed for infrastructure reasons.</span></span> <span data-ttu-id="ac904-161">Hello után n naponta hello dátum hello e-mailt küldték, a Folytatás tooPhase 2.</span><span class="sxs-lookup"><span data-stu-id="ac904-161">After hello n days from hello date hello email was sent, we proceed tooPhase 2.</span></span>

<span data-ttu-id="ac904-162">Ha hello fürt állapotházirendeket teljesülnek, a hello frissítés van sikeresnek minősül, és befejezettként.</span><span class="sxs-lookup"><span data-stu-id="ac904-162">If hello cluster health policies are met, hello upgrade is considered successful and marked complete.</span></span> <span data-ttu-id="ac904-163">Ez akkor fordulhat elő hello kezdeti frissítés vagy frissítési ismétlések hello bármelyikét során ebben a fázisban.</span><span class="sxs-lookup"><span data-stu-id="ac904-163">This can happen during hello initial upgrade or any of hello upgrade reruns in this phase.</span></span> <span data-ttu-id="ac904-164">Nincs e-mailes megerősítés sikeres futtató van.</span><span class="sxs-lookup"><span data-stu-id="ac904-164">There is no email confirmation of a successful run.</span></span> <span data-ttu-id="ac904-165">Ez a tooavoid egy kivétel toonormal kell tekinteni, túl sok e-mailek--küld egy e-maileket küld.</span><span class="sxs-lookup"><span data-stu-id="ac904-165">This is tooavoid sending you too many emails--receiving an email should be seen as an exception toonormal.</span></span> <span data-ttu-id="ac904-166">A legtöbb hello fürt frissítések toosucceed várhatóan az alkalmazások rendelkezésre állásáról befolyásolása nélkül.</span><span class="sxs-lookup"><span data-stu-id="ac904-166">We expect most of hello cluster upgrades toosucceed without impacting your application availability.</span></span>

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a><span data-ttu-id="ac904-167">2. fázis: Egy frissítés végrehajtása csak alapértelmezett állapotházirendeket használatával</span><span class="sxs-lookup"><span data-stu-id="ac904-167">Phase 2: An upgrade is performed by using default health policies only</span></span>
<span data-ttu-id="ac904-168">Ebben a fázisban hello állapotházirendeket meg úgy, hogy a megfelelő hello frissítés hello elején volt alkalmazások hello számát hello azonos hello hello frissítési eljárás ideje alatt továbbra is.</span><span class="sxs-lookup"><span data-stu-id="ac904-168">hello health policies in this phase are set in such a way that hello number of applications that were healthy at hello beginning of hello upgrade remains hello same for hello duration of hello upgrade process.</span></span> <span data-ttu-id="ac904-169">1. fázis, mint 2. szakasza hello frissítések folytatható egy frissítési tartományt egyszerre, és hello hello fürtben futó alkalmazások folytatni toorun állásidő nélkül.</span><span class="sxs-lookup"><span data-stu-id="ac904-169">As in Phase 1, hello Phase 2 upgrades proceed one upgrade domain at a time, and hello applications that were running in hello cluster continue toorun without any downtime.</span></span> <span data-ttu-id="ac904-170">hello fürt állapotházirendeket (kombinációjából csomópont hello egészségügyi összes hello hello fürtben futó alkalmazások és) hello frissítés betartását toofor hello időtartama.</span><span class="sxs-lookup"><span data-stu-id="ac904-170">hello cluster health policies (a combination of node health and hello health all hello applications running in hello cluster) are adhered toofor hello duration of hello upgrade.</span></span>

<span data-ttu-id="ac904-171">Ha hello fürt házirendek érvényben nem teljesülnek, hello frissítés vissza lesz állítva.</span><span class="sxs-lookup"><span data-stu-id="ac904-171">If hello cluster health policies in effect are not met, hello upgrade is rolled back.</span></span> <span data-ttu-id="ac904-172">Ezután egy e-mailt küld hello előfizetés toohello tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="ac904-172">Then an email is sent toohello owner of hello subscription.</span></span> <span data-ttu-id="ac904-173">hello e-mail hello a következő információkat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="ac904-173">hello email contains hello following information:</span></span>

* <span data-ttu-id="ac904-174">Az értesítési tooroll vissza egy Fürtfrissítés le kellett.</span><span class="sxs-lookup"><span data-stu-id="ac904-174">Notification that we had tooroll back a cluster upgrade.</span></span>
* <span data-ttu-id="ac904-175">Javasolt javító műveleteket, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="ac904-175">Suggested remedial actions, if any.</span></span>
* <span data-ttu-id="ac904-176">hello hány napig (n) csak akkor hajtható végre a 3.</span><span class="sxs-lookup"><span data-stu-id="ac904-176">hello number of days (n) until we execute Phase 3.</span></span>

<span data-ttu-id="ac904-177">A Microsoft próbálja tooexecute hello azonos néhány alkalommal frissíteni, abban az esetben, ha a megjelenjenek infrastruktúra okok miatt nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="ac904-177">We try tooexecute hello same upgrade a few more times in case any upgrades failed for infrastructure reasons.</span></span> <span data-ttu-id="ac904-178">Egy emlékeztető e-mailt küld néhány napos n naponta mentése előtt.</span><span class="sxs-lookup"><span data-stu-id="ac904-178">A reminder email is sent a couple of days before n days are up.</span></span> <span data-ttu-id="ac904-179">Hello után n naponta hello dátum hello e-mailt küldték, folytatás tooPhase 3.</span><span class="sxs-lookup"><span data-stu-id="ac904-179">After hello n days from hello date hello email was sent, we proceed tooPhase 3.</span></span> <span data-ttu-id="ac904-180">a 2. szakasza kapni hello e-mailek súlyosan kell venni, és javító műveleteket kell tenni.</span><span class="sxs-lookup"><span data-stu-id="ac904-180">hello emails we send you in Phase 2 must be taken seriously and remedial actions must be taken.</span></span>

<span data-ttu-id="ac904-181">Ha hello fürt állapotházirendeket teljesülnek, a hello frissítés van sikeresnek minősül, és befejezettként.</span><span class="sxs-lookup"><span data-stu-id="ac904-181">If hello cluster health policies are met, hello upgrade is considered successful and marked complete.</span></span> <span data-ttu-id="ac904-182">Ez akkor fordulhat elő hello kezdeti frissítés vagy frissítési ismétlések hello bármelyikét során ebben a fázisban.</span><span class="sxs-lookup"><span data-stu-id="ac904-182">This can happen during hello initial upgrade or any of hello upgrade reruns in this phase.</span></span> <span data-ttu-id="ac904-183">Nincs e-mailes megerősítés sikeres futtató van.</span><span class="sxs-lookup"><span data-stu-id="ac904-183">There is no email confirmation of a successful run.</span></span>

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a><span data-ttu-id="ac904-184">3. fázis: A frissítés végrehajtása túl szigorú házirendek használatával</span><span class="sxs-lookup"><span data-stu-id="ac904-184">Phase 3: An upgrade is performed by using aggressive health policies</span></span>
<span data-ttu-id="ac904-185">Ebben a fázisban a állapotházirendeket hello futó alkalmazások állapotát hello helyett hello frissítés körétől vannak.</span><span class="sxs-lookup"><span data-stu-id="ac904-185">These health policies in this phase are geared towards completion of hello upgrade rather than hello health of hello applications.</span></span> <span data-ttu-id="ac904-186">Ebben a fázisban végül fürt nagyon kevés frissítés.</span><span class="sxs-lookup"><span data-stu-id="ac904-186">Very few cluster upgrades end up in this phase.</span></span> <span data-ttu-id="ac904-187">A fürt toothis fázis kap, ha van esély arra, hogy az alkalmazás akkor kerül sérült állapotba, és/vagy rendelkezésre állási elvesznek.</span><span class="sxs-lookup"><span data-stu-id="ac904-187">If your cluster gets toothis phase, there is a good chance that your application becomes unhealthy and/or lose availability.</span></span>

<span data-ttu-id="ac904-188">Hasonló toohello más két fázis a 3 frissítések folytassa a műveletet egy frissítési tartományt egyszerre.</span><span class="sxs-lookup"><span data-stu-id="ac904-188">Similar toohello other two phases, Phase 3 upgrades proceed one upgrade domain at a time.</span></span>

<span data-ttu-id="ac904-189">Ha a fürt állapotházirendeket hello nem teljesülnek, hello frissítés vissza lesz állítva.</span><span class="sxs-lookup"><span data-stu-id="ac904-189">If hello cluster health policies are not met, hello upgrade is rolled back.</span></span> <span data-ttu-id="ac904-190">A Microsoft próbálja tooexecute hello azonos néhány alkalommal frissíteni, abban az esetben, ha a megjelenjenek infrastruktúra okok miatt nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="ac904-190">We try tooexecute hello same upgrade a few more times in case any upgrades failed for infrastructure reasons.</span></span> <span data-ttu-id="ac904-191">Ezt követően hello fürt rögzítve van, így már nem kap támogatási és/vagy a frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="ac904-191">After that, hello cluster is pinned, so that it will no longer receive support and/or upgrades.</span></span>

<span data-ttu-id="ac904-192">Az információ egy e-mailt küld toohello előfizetés tulajdonosa, valamint hello javító műveleteket.</span><span class="sxs-lookup"><span data-stu-id="ac904-192">An email with this information is sent toohello subscription owner, along with hello remedial actions.</span></span> <span data-ttu-id="ac904-193">A fürtök tooget várhatóan nem olyan állapotba, ha nem a 3.</span><span class="sxs-lookup"><span data-stu-id="ac904-193">We do not expect any clusters tooget into a state where Phase 3 has failed.</span></span>

<span data-ttu-id="ac904-194">Ha hello fürt állapotházirendeket teljesülnek, a hello frissítés van sikeresnek minősül, és befejezettként.</span><span class="sxs-lookup"><span data-stu-id="ac904-194">If hello cluster health policies are met, hello upgrade is considered successful and marked complete.</span></span> <span data-ttu-id="ac904-195">Ez akkor fordulhat elő hello kezdeti frissítés vagy frissítési ismétlések hello bármelyikét során ebben a fázisban.</span><span class="sxs-lookup"><span data-stu-id="ac904-195">This can happen during hello initial upgrade or any of hello upgrade reruns in this phase.</span></span> <span data-ttu-id="ac904-196">Nincs e-mailes megerősítés sikeres futtató van.</span><span class="sxs-lookup"><span data-stu-id="ac904-196">There is no email confirmation of a successful run.</span></span>

## <a name="cluster-configurations-that-you-control"></a><span data-ttu-id="ac904-197">Vezérelt fürtkonfigurációk</span><span class="sxs-lookup"><span data-stu-id="ac904-197">Cluster configurations that you control</span></span>
<span data-ttu-id="ac904-198">Ezenkívül toohello képességét tooset hello fürt frissítési mód, az alábbiakban egy élő fürthöz módosíthatja hello konfigurációkat.</span><span class="sxs-lookup"><span data-stu-id="ac904-198">In addition toohello ability tooset hello cluster upgrade mode, Here are hello configurations that you can change on a live cluster.</span></span>

### <a name="certificates"></a><span data-ttu-id="ac904-199">Tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="ac904-199">Certificates</span></span>
<span data-ttu-id="ac904-200">Hozzáadhat új, vagy a tanúsítványok hello fürt és az ügyfél hello portálon keresztül egyszerűen törölje.</span><span class="sxs-lookup"><span data-stu-id="ac904-200">You can add new or delete certificates for hello cluster and client via hello portal easily.</span></span> <span data-ttu-id="ac904-201">Tekintse meg a túl[Ez a dokumentum részletes információkra van szüksége](service-fabric-cluster-security-update-certs-azure.md)</span><span class="sxs-lookup"><span data-stu-id="ac904-201">Refer too[this document for detailed instructions](service-fabric-cluster-security-update-certs-azure.md)</span></span>

![Képernyőkép a tanúsítvány-ujjlenyomatok a hello Azure-portálon.][CertificateUpgrade]

### <a name="application-ports"></a><span data-ttu-id="ac904-203">Alkalmazás-portok</span><span class="sxs-lookup"><span data-stu-id="ac904-203">Application ports</span></span>
<span data-ttu-id="ac904-204">Alkalmazás portok hello terheléselosztó erőforrás-tulajdonságok hello csomóponttípus társított megváltoztatásával módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="ac904-204">You can change application ports by changing hello Load Balancer resource properties that are associated with hello node type.</span></span> <span data-ttu-id="ac904-205">Hello portálon is használhatja, vagy az erőforrás-kezelő a PowerShell segítségével közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="ac904-205">You can use hello portal, or you can use Resource Manager PowerShell directly.</span></span>

<span data-ttu-id="ac904-206">Új port típusú csomópont összes virtuális tooopen hello a következő:</span><span class="sxs-lookup"><span data-stu-id="ac904-206">tooopen a new port on all VMs in a node type, do hello following:</span></span>

1. <span data-ttu-id="ac904-207">Adjon hozzá egy új mintavételi toohello megfelelő terheléselosztót.</span><span class="sxs-lookup"><span data-stu-id="ac904-207">Add a new probe toohello appropriate load balancer.</span></span>
   
    <span data-ttu-id="ac904-208">Ha a fürt hello portál használatával telepítette, hello terheléselosztók elnevezése "LB-neve hello erőforrás csoport-NodeTypename", az egyes csomóponttípusok egyet.</span><span class="sxs-lookup"><span data-stu-id="ac904-208">If you deployed your cluster by using hello portal, hello load balancers are named "LB-name of hello Resource group-NodeTypename", one for each node type.</span></span> <span data-ttu-id="ac904-209">Mivel hello load balancer nevek csak egy erőforráscsoporton belül egyedi, célszerű, ha egy adott erőforrás csoportban keresse meg őket.</span><span class="sxs-lookup"><span data-stu-id="ac904-209">Since hello load balancer names are unique only within a resource group, it is best if you search for them under a specific resource group.</span></span>
   
    ![Képernyőkép a hozzáadása egy mintavételi tooa terheléselosztó hello portálon.][AddingProbes]
2. <span data-ttu-id="ac904-211">Új szabály toohello terheléselosztó felvételéhez.</span><span class="sxs-lookup"><span data-stu-id="ac904-211">Add a new rule toohello load balancer.</span></span>
   
    <span data-ttu-id="ac904-212">Adjon hozzá egy új szabály toohello terheléselosztó azonos hello mintavételi hello előző lépésben létrehozott használatával.</span><span class="sxs-lookup"><span data-stu-id="ac904-212">Add a new rule toohello same load balancer by using hello probe that you created in hello previous step.</span></span>
   
    ![Új szabály tooa terheléselosztó hozzáadása hello portálon.][AddingLBRules]

### <a name="placement-properties"></a><span data-ttu-id="ac904-214">Elhelyezési tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="ac904-214">Placement properties</span></span>
<span data-ttu-id="ac904-215">Az egyes csomóponttípusok hello toouse szeretné az alkalmazásokat az egyéni elhelyezési tulajdonságokat adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="ac904-215">For each of hello node types, you can add custom placement properties that you want toouse in your applications.</span></span> <span data-ttu-id="ac904-216">A NodeType egy alapértelmezett tulajdonság, amely explicit módon hozzáadná nélkül is használható.</span><span class="sxs-lookup"><span data-stu-id="ac904-216">NodeType is a default property that you can use without adding it explicitly.</span></span>

> [!NOTE]
> <span data-ttu-id="ac904-217">Részletes hello használatára korlátozza, csomópont tulajdonságait, és hogyan toodefine őket, tekintse meg a Service Fabric fürt erőforrás-kezelő dokumentum hello toohello szakaszának "Elhelyezési megkötések és csomópont tulajdonságok" a [a fürt leíró ](service-fabric-cluster-resource-manager-cluster-description.md).</span><span class="sxs-lookup"><span data-stu-id="ac904-217">For details on hello use of placement constraints, node properties, and how toodefine them, refer toohello section "Placement Constraints and Node Properties" in hello Service Fabric Cluster Resource Manager Document on [Describing Your Cluster](service-fabric-cluster-resource-manager-cluster-description.md).</span></span>
> 
> 

### <a name="capacity-metrics"></a><span data-ttu-id="ac904-218">A kapacitás metrikák</span><span class="sxs-lookup"><span data-stu-id="ac904-218">Capacity metrics</span></span>
<span data-ttu-id="ac904-219">Az egyes csomóponttípusok hello megjeleníteni kívánt toouse az alkalmazások tooreport terhelés egyéni kapacitási mérőszámokat adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="ac904-219">For each of hello node types, you can add custom capacity metrics that you want toouse in your applications tooreport load.</span></span> <span data-ttu-id="ac904-220">A kapacitás metrikák tooreport terhelésének hello használata további tájékoztatást toohello Service Fabric fürt erőforrás-kezelő dokumentumok [a fürt leíró](service-fabric-cluster-resource-manager-cluster-description.md) és [metrikák és a](service-fabric-cluster-resource-manager-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="ac904-220">For details on hello use of capacity metrics tooreport load, refer toohello Service Fabric Cluster Resource Manager Documents on [Describing Your Cluster](service-fabric-cluster-resource-manager-cluster-description.md) and [Metrics and Load](service-fabric-cluster-resource-manager-metrics.md).</span></span>

### <a name="fabric-upgrade-settings---health-polices"></a><span data-ttu-id="ac904-221">Frissítési beállítások fabric - irányelveinek</span><span class="sxs-lookup"><span data-stu-id="ac904-221">Fabric upgrade settings - Health polices</span></span>
<span data-ttu-id="ac904-222">Megadhat egyéni irányelveinek fabric frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ac904-222">You can specify custom health polices for fabric upgrade.</span></span> <span data-ttu-id="ac904-223">Ha a fürt tooAutomatic fabric frissítéskezelésének meg, ezek a házirendek alkalmazott toohello 1. fázis hello automatikus fabric frissítéskezelésének beolvasása.</span><span class="sxs-lookup"><span data-stu-id="ac904-223">If you have set your cluster tooAutomatic fabric upgrades, then these policies get applied toohello Phase-1 of hello automatic fabric upgrades.</span></span>
<span data-ttu-id="ac904-224">Ha meg van a fürt manuális háló frissítéseket, akkor ezek a házirendek érvényben minden alkalommal, amikor választja kiváltó hello rendszer tookick hello fabric-frissítésnek a fürt ki új verzió.</span><span class="sxs-lookup"><span data-stu-id="ac904-224">If you have set your cluster for Manual fabric upgrades, then these policies get applied each time you select a new version triggering hello system tookick off hello fabric upgrade in your cluster.</span></span> <span data-ttu-id="ac904-225">Ha nem bírál felül hello házirendek, hello alapértelmezett beállításokat használja.</span><span class="sxs-lookup"><span data-stu-id="ac904-225">If you do not override hello policies, hello defaults are used.</span></span>

<span data-ttu-id="ac904-226">Adjon meg egyéni állapotházirendeket hello, vagy áttekintéséhez hello jelenlegi beállításai alapján hello "fabric-frissítésnek" panelen válassza ki a hello speciális beállítások frissítése.</span><span class="sxs-lookup"><span data-stu-id="ac904-226">You can specify hello custom health policies or review hello current settings under hello "fabric upgrade" blade, by selecting hello advanced upgrade settings.</span></span> <span data-ttu-id="ac904-227">Tekintse át az alábbi képen való hello.</span><span class="sxs-lookup"><span data-stu-id="ac904-227">Review hello following picture on how to.</span></span> 

![Egyéni házirendek kezelése][HealthPolices]

### <a name="customize-fabric-settings-for-your-cluster"></a><span data-ttu-id="ac904-229">A fürt háló beállításainak testreszabása</span><span class="sxs-lookup"><span data-stu-id="ac904-229">Customize Fabric settings for your cluster</span></span>
<span data-ttu-id="ac904-230">Tekintse meg a túl[service fabric-fürt hálóbeállításokat](service-fabric-cluster-fabric-settings.md) mi, és hogyan szabhatja azokat.</span><span class="sxs-lookup"><span data-stu-id="ac904-230">Refer too[service fabric cluster fabric settings](service-fabric-cluster-fabric-settings.md) on what and how you can customize them.</span></span>

### <a name="os-patches-on-hello-vms-that-make-up-hello-cluster"></a><span data-ttu-id="ac904-231">A virtuális gépek hello fürtöt alkotó hello operációsrendszer-javítások</span><span class="sxs-lookup"><span data-stu-id="ac904-231">OS patches on hello VMs that make up hello cluster</span></span>
<span data-ttu-id="ac904-232">Tekintse meg a túl[javítás Vezénylési alkalmazás](service-fabric-patch-orchestration-application.md) amelyek központilag telepíthetők a Windows Update szolgáltatásból a fürt tooinstall javítások tartva hello elérhető szolgáltatások összes hello orchestrated módon.</span><span class="sxs-lookup"><span data-stu-id="ac904-232">Refer too[Patch Orchestration Application](service-fabric-patch-orchestration-application.md) which can be deployed on your cluster tooinstall patches from Windows Update in an orchestrated manner, keeping hello services available all hello time.</span></span> 

### <a name="os-upgrades-on-hello-vms-that-make-up-hello-cluster"></a><span data-ttu-id="ac904-233">Az operációs rendszer frissítései a hello hello fürtöt alkotó virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="ac904-233">OS upgrades on hello VMs that make up hello cluster</span></span>
<span data-ttu-id="ac904-234">Ha frissítenie kell az operációsrendszer-lemezképek hello hello fürt hello virtuális gépeken, tegye azt egy virtuális gép egyszerre.</span><span class="sxs-lookup"><span data-stu-id="ac904-234">If you must upgrade hello OS image on hello virtual machines of hello cluster, you must do it one VM at a time.</span></span> <span data-ttu-id="ac904-235">Az Ön felelőssége ehhez a frissítéshez--jelenleg nincs automatizálva ehhez.</span><span class="sxs-lookup"><span data-stu-id="ac904-235">You are responsible for this upgrade--there is currently no automation for this.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ac904-236">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ac904-236">Next steps</span></span>
* <span data-ttu-id="ac904-237">Megtudhatja, hogyan toocustomize néhány hello [szolgáltatás fabric fürt háló beállításai](service-fabric-cluster-fabric-settings.md)</span><span class="sxs-lookup"><span data-stu-id="ac904-237">Learn how toocustomize some of hello [service fabric cluster fabric settings](service-fabric-cluster-fabric-settings.md)</span></span>
* <span data-ttu-id="ac904-238">Ismerje meg, hogyan túl[a fürt vagy horizontális](service-fabric-cluster-scale-up-down.md)</span><span class="sxs-lookup"><span data-stu-id="ac904-238">Learn how too[scale your cluster in and out](service-fabric-cluster-scale-up-down.md)</span></span>
* <span data-ttu-id="ac904-239">További tudnivalók [alkalmazásfrissítések](service-fabric-application-upgrade.md)</span><span class="sxs-lookup"><span data-stu-id="ac904-239">Learn about [application upgrades](service-fabric-application-upgrade.md)</span></span>

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG
