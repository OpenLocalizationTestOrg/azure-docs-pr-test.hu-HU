---
title: "az Azure Naplóelemzés Surface hub aaaMonitor |} Microsoft Docs"
description: "Hello Surface Hub megoldás tootrack hello állapotát a Surface hub használja, és megérteni, hogyan kívánják felhasználni."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 8b4e56bc-2d4f-4648-a236-16e9e732ebef
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 623d30e749cafdd4a34ba0c5b3408164f1b4a95b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-surface-hubs-with-log-analytics-tootrack-their-health"></a><span data-ttu-id="103d3-103">A Naplóelemzési tootrack Surface hub figyelő a rendszerállapot</span><span class="sxs-lookup"><span data-stu-id="103d3-103">Monitor Surface Hubs with Log Analytics tootrack their health</span></span>

![Surface Hub szimbólum](./media/log-analytics-surface-hubs/surface-hub-symbol.png)

<span data-ttu-id="103d3-105">Ez a cikk ismerteti, hogyan használhatja a Naplóelemzési toomonitor Microsoft Surface Hub eszközöket a Microsoft Operations Management Suite (OMS) hello hello Surface Hub megoldás.</span><span class="sxs-lookup"><span data-stu-id="103d3-105">This article describes how you can use hello Surface Hub solution in Log Analytics toomonitor Microsoft Surface Hub devices with hello Microsoft Operations Management Suite (OMS).</span></span> <span data-ttu-id="103d3-106">Jelentkezzen a Surface hub hello állapotát, valamint megérteni, hogyan kívánják felhasználni nyomon Analytics segítségével.</span><span class="sxs-lookup"><span data-stu-id="103d3-106">Log Analytics helps you track hello health of your Surface Hubs as well as understand how they are being used.</span></span>

<span data-ttu-id="103d3-107">Minden egyes Surface Hub rendelkezik hello Microsoft Monitoring Agent telepítését.</span><span class="sxs-lookup"><span data-stu-id="103d3-107">Each Surface Hub has hello Microsoft Monitoring Agent installed.</span></span> <span data-ttu-id="103d3-108">Az, hogy adatokat küldhet a Surface Hub tooOMS a hello ügynök keresztül.</span><span class="sxs-lookup"><span data-stu-id="103d3-108">Its through hello agent that you can send data from your Surface Hub tooOMS.</span></span> <span data-ttu-id="103d3-109">Naplófájlok a Surface hub és a olvasni, majd küldött toohello OMS szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="103d3-109">Log files are read from your Surface Hubs and are then are sent toohello OMS service.</span></span> <span data-ttu-id="103d3-110">Problémák, mint a kiszolgáló nem kapcsolódik, a naptár nem szinkronizálása hello, vagy ha hello fiók Skype be nem toolog láthatók OMS hello Surface Hub irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="103d3-110">Issues like servers being offline, hello calendar not syncing, or if hello device account is unable toolog into Skype are shown in OMS in hello Surface Hub dashboard.</span></span> <span data-ttu-id="103d3-111">Hello irányítópult hello adatok használatával azonosíthatja az eszközök nem fut, vagy, hogy más problémákat, amelynek, potenciálisan alkalmazhat hello észlelt probléma javítását.</span><span class="sxs-lookup"><span data-stu-id="103d3-111">By using hello data in hello dashboard, you can identify devices that are not running, or that are having other problems, and potentially apply fixes for hello detected issues.</span></span>

## <a name="installing-and-configuring-hello-solution"></a><span data-ttu-id="103d3-112">Telepítése és konfigurálása hello megoldás</span><span class="sxs-lookup"><span data-stu-id="103d3-112">Installing and configuring hello solution</span></span>
<span data-ttu-id="103d3-113">A következő információk tooinstall hello használja, és hello megoldás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="103d3-113">Use hello following information tooinstall and configure hello solution.</span></span> <span data-ttu-id="103d3-114">A sorrend toomanage a hello Microsoft Operations Management Suite (OMS), a Surface hub szüksége lesz a következő hello:</span><span class="sxs-lookup"><span data-stu-id="103d3-114">In order toomanage your Surface Hubs from hello Microsoft Operations Management Suite (OMS), you'll need hello following:</span></span>

* <span data-ttu-id="103d3-115">Egy érvényes előfizetéssel túl[OMS](http://www.microsoft.com/oms).</span><span class="sxs-lookup"><span data-stu-id="103d3-115">A valid subscription too[OMS](http://www.microsoft.com/oms).</span></span>
* <span data-ttu-id="103d3-116">Egy [OMS előfizetés](https://azure.microsoft.com/pricing/details/log-analytics/) szint hello szám támogató eszközök toomonitor szeretné.</span><span class="sxs-lookup"><span data-stu-id="103d3-116">An [OMS subscription](https://azure.microsoft.com/pricing/details/log-analytics/) level that will support hello number of devices you want toomonitor.</span></span> <span data-ttu-id="103d3-117">OMS árképzési változó attól függően, hogy hány eszköz regisztrálva van, és mennyi adatot azt folyamatokat.</span><span class="sxs-lookup"><span data-stu-id="103d3-117">OMS pricing varies depending on how many devices are enrolled, and how much data it processes.</span></span> <span data-ttu-id="103d3-118">Érdemes tootake ezt számításba venni a Surface Hub-bevezetés tervezése során.</span><span class="sxs-lookup"><span data-stu-id="103d3-118">You'll want tootake this into consideration when planning your Surface Hub rollout.</span></span>

<span data-ttu-id="103d3-119">A következő fog egy OMS előfizetés tooyour meglévő Microsoft Azure-előfizetés felvétele vagy hozzon létre egy új munkaterületet hello OMS-portálon keresztül közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="103d3-119">Next, you will either add an OMS subscription tooyour existing Microsoft Azure subscription or create a new workspace directly through hello OMS portal.</span></span> <span data-ttu-id="103d3-120">Részletes utasításokat a következő módszerek egyikével érték [Ismerkedés a Naplóelemzési](log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="103d3-120">Detailed instructions for using either method is at [Get started with Log Analytics](log-analytics-get-started.md).</span></span> <span data-ttu-id="103d3-121">Hello OMS-előfizetés beállítása után van két módon tooenroll a Surface Hub eszközöket:</span><span class="sxs-lookup"><span data-stu-id="103d3-121">Once hello OMS subscription is set up, there are two ways tooenroll your Surface Hub devices:</span></span>

* <span data-ttu-id="103d3-122">Automatikusan Intune-nal</span><span class="sxs-lookup"><span data-stu-id="103d3-122">Automatically through Intune</span></span>
* <span data-ttu-id="103d3-123">Manuálisan osztályig **beállítások** Surface Hub eszközén.</span><span class="sxs-lookup"><span data-stu-id="103d3-123">Manually through **Settings** on your Surface Hub device.</span></span>

## <a name="set-up-monitoring"></a><span data-ttu-id="103d3-124">Állítsa be a figyelést,</span><span class="sxs-lookup"><span data-stu-id="103d3-124">Set up monitoring</span></span>
<span data-ttu-id="103d3-125">Hello állapotát és tevékenységét a Surface Hub OMS Naplóelemzési használatával figyelheti.</span><span class="sxs-lookup"><span data-stu-id="103d3-125">You can monitor hello health and activity of your Surface Hub using Log Analytics in OMS.</span></span> <span data-ttu-id="103d3-126">Hello Surface Hub az OMS regisztrálása az Intune-ban, vagy helyileg segítségével **beállítások** a Surface Hub hello.</span><span class="sxs-lookup"><span data-stu-id="103d3-126">You can enroll hello Surface Hub in OMS by using Intune, or locally by using **Settings** on hello Surface Hub.</span></span>

## <a name="connect-surface-hubs-toooms-through-intune"></a><span data-ttu-id="103d3-127">Csatlakozás a Surface hub tooOMS Intune-nal</span><span class="sxs-lookup"><span data-stu-id="103d3-127">Connect Surface Hubs tooOMS through Intune</span></span>
<span data-ttu-id="103d3-128">Akkor lesz kell hello munkaterület Azonosítóját és kulcsát az hello OMS-munkaterület, a Surface hub kezelésére.</span><span class="sxs-lookup"><span data-stu-id="103d3-128">You'll need hello workspace ID and workspace key for hello OMS workspace that will manage your Surface Hubs.</span></span> <span data-ttu-id="103d3-129">Hello OMS-portálon szerintiek kérheti le.</span><span class="sxs-lookup"><span data-stu-id="103d3-129">You can get those from hello OMS portal.</span></span>

<span data-ttu-id="103d3-130">Intune-ban egy Microsoft-termék, amely lehetővé teszi toocentrally hello OMS beállítások, amelyek alkalmazott tooone vagy több, az eszközök kezelése.</span><span class="sxs-lookup"><span data-stu-id="103d3-130">Intune is a Microsoft product that allows you toocentrally manage hello OMS configuration settings that are applied tooone or more of your devices.</span></span> <span data-ttu-id="103d3-131">Kövesse ezeket a lépéseket tooconfigure az eszközök Intune-nal:</span><span class="sxs-lookup"><span data-stu-id="103d3-131">Follow these steps tooconfigure your devices through Intune:</span></span>

1. <span data-ttu-id="103d3-132">Jelentkezzen be tooIntune.</span><span class="sxs-lookup"><span data-stu-id="103d3-132">Sign in tooIntune.</span></span>
2. <span data-ttu-id="103d3-133">Keresse meg a túl**beállítások** > **csatlakoztatott források**.</span><span class="sxs-lookup"><span data-stu-id="103d3-133">Navigate too**Settings** > **Connected Sources**.</span></span>
3. <span data-ttu-id="103d3-134">Hozzon létre vagy hello Surface Hub sablonon alapuló házirend szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="103d3-134">Create or edit a policy based on hello Surface Hub template.</span></span>
4. <span data-ttu-id="103d3-135">Keresse meg a toohello OMS (Azure Operational Insights) szakasz hello házirend, és adja hozzá a hello *munkaterület azonosítója* és *Munkaterületkulcsot* toohello házirend.</span><span class="sxs-lookup"><span data-stu-id="103d3-135">Navigate toohello OMS (Azure Operational Insights) section of hello policy, and add hello *Workspace ID* and *Workspace Key* toohello policy.</span></span>
5. <span data-ttu-id="103d3-136">Hello szabályzat mentése.</span><span class="sxs-lookup"><span data-stu-id="103d3-136">Save hello policy.</span></span>
6. <span data-ttu-id="103d3-137">Hello házirend társítása hello megfelelő eszközcsoportra.</span><span class="sxs-lookup"><span data-stu-id="103d3-137">Associate hello policy with hello appropriate group of devices.</span></span>

   ![Az Intune-házirend](./media/log-analytics-surface-hubs/intune.png)

<span data-ttu-id="103d3-139">Az Intune majd szinkronizál hello OMS beállítások hello célcsoportot, ha regisztrálja azokat az OMS-munkaterület hello eszközöket.</span><span class="sxs-lookup"><span data-stu-id="103d3-139">Intune then syncs hello OMS settings with hello devices in hello target group, enrolling them in your OMS workspace.</span></span>

## <a name="connect-surface-hubs-toooms-using-hello-settings-app"></a><span data-ttu-id="103d3-140">Csatlakozás a Surface hub tooOMS hello-beállítások alkalmazással</span><span class="sxs-lookup"><span data-stu-id="103d3-140">Connect Surface Hubs tooOMS using hello Settings app</span></span>
<span data-ttu-id="103d3-141">Akkor lesz kell hello munkaterület Azonosítóját és kulcsát az hello OMS-munkaterület, a Surface hub kezelésére.</span><span class="sxs-lookup"><span data-stu-id="103d3-141">You'll need hello workspace ID and workspace key for hello OMS workspace that will manage your Surface Hubs.</span></span> <span data-ttu-id="103d3-142">Hello OMS-portálon szerintiek kérheti le.</span><span class="sxs-lookup"><span data-stu-id="103d3-142">You can get those from hello OMS portal.</span></span>

<span data-ttu-id="103d3-143">Ha nem adja meg Intune toomanage a környezetében, manuálisan keresztül az eszközök regisztrálása **beállítások** minden Surface hub:</span><span class="sxs-lookup"><span data-stu-id="103d3-143">If you don't use Intune toomanage your environment, you can enroll devices manually through **Settings** on each Surface Hub:</span></span>

1. <span data-ttu-id="103d3-144">Nyissa meg a Surface Hub **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="103d3-144">From your Surface Hub, open **Settings**.</span></span>
2. <span data-ttu-id="103d3-145">Adja meg a hello eszközt rendszergazdai hitelesítő adatokat, amikor a rendszer kéri.</span><span class="sxs-lookup"><span data-stu-id="103d3-145">Enter hello device admin credentials when prompted.</span></span>
3. <span data-ttu-id="103d3-146">Kattintson a **az eszköz**, és a hello **figyelés**, kattintson a **OMS-beállítások konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="103d3-146">Click **This device**, and hello under **Monitoring**, click **Configure OMS Settings**.</span></span>
4. <span data-ttu-id="103d3-147">Válassza ki **engedélyezze a megfigyelést**.</span><span class="sxs-lookup"><span data-stu-id="103d3-147">Select **Enable monitoring**.</span></span>
5. <span data-ttu-id="103d3-148">Hello OMS-beállítások párbeszédpanelen írja be a hello **munkaterület azonosítója** és típus hello **Munkaterületkulcsot**.</span><span class="sxs-lookup"><span data-stu-id="103d3-148">In hello OMS settings dialog, type hello **Workspace ID** and type hello **Workspace Key**.</span></span>  
   <span data-ttu-id="103d3-149">![beállítások](./media/log-analytics-surface-hubs/settings.png)</span><span class="sxs-lookup"><span data-stu-id="103d3-149">![settings](./media/log-analytics-surface-hubs/settings.png)</span></span>
6. <span data-ttu-id="103d3-150">Kattintson a **OK** toocomplete hello konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="103d3-150">Click **OK** toocomplete hello configuration.</span></span>

<span data-ttu-id="103d3-151">Megerősítést felszólító-e az OMS-konfigurációja sikeresen hello alkalmazott toohello eszköz jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="103d3-151">A confirmation appears telling you whether or not hello OMS configuration was successfully applied toohello device.</span></span> <span data-ttu-id="103d3-152">Amennyiben igen, megjelenik egy üzenet, amely meghatározza, hogy az hello ügynök sikeresen csatlakoztatva lett toohello OMS szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="103d3-152">If it was, a message appears stating that hello agent successfully connected toohello OMS service.</span></span> <span data-ttu-id="103d3-153">hello eszköz ezután elindítja az adatok tooOMS, ahol megtekintheti és a működésre küldésekor.</span><span class="sxs-lookup"><span data-stu-id="103d3-153">hello device then starts sending data tooOMS where you can view and act on it.</span></span>

## <a name="monitor-surface-hubs"></a><span data-ttu-id="103d3-154">A figyelő hubok</span><span class="sxs-lookup"><span data-stu-id="103d3-154">Monitor Surface Hubs</span></span>
<span data-ttu-id="103d3-155">Figyelés, a Surface hub OMS használata sokkal hasonlóan a további regisztrált eszközök figyelését.</span><span class="sxs-lookup"><span data-stu-id="103d3-155">Monitoring your Surface Hubs using OMS is much like monitoring any other enrolled devices.</span></span>

1. <span data-ttu-id="103d3-156">Bejelentkezés toohello OMS-portálon.</span><span class="sxs-lookup"><span data-stu-id="103d3-156">Sign in toohello OMS portal.</span></span>
2. <span data-ttu-id="103d3-157">Keresse meg a toohello Surface Hub megoldás pack irányítópult.</span><span class="sxs-lookup"><span data-stu-id="103d3-157">Navigate toohello Surface Hub solution pack dashboard.</span></span>
3. <span data-ttu-id="103d3-158">Az eszköz állapota akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="103d3-158">Your device's health is displayed.</span></span>

   ![Surface Hub irányítópult](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

<span data-ttu-id="103d3-160">Létrehozhat [riasztások](log-analytics-alerts.md) meglévő vagy egyéni napló keresés alapján.</span><span class="sxs-lookup"><span data-stu-id="103d3-160">You can create [alerts](log-analytics-alerts.md) based on existing or custom log searches.</span></span> <span data-ttu-id="103d3-161">Hello adatok hello OMS gyűjti össze a Surface hub használva kereshet problémákat és az eszközök meghatározó hello feltételeket a riasztás.</span><span class="sxs-lookup"><span data-stu-id="103d3-161">Using hello data hello OMS collects from your Surface Hubs, you can search for issues and alert on hello conditions that you define for your devices.</span></span>

## <a name="next-steps"></a><span data-ttu-id="103d3-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="103d3-162">Next steps</span></span>
* <span data-ttu-id="103d3-163">Használjon [Log Analytics-e jelentkezni a keresések](log-analytics-log-searches.md) tooview részletes Surface Hub-adatok.</span><span class="sxs-lookup"><span data-stu-id="103d3-163">Use [Log searches in Log Analytics](log-analytics-log-searches.md) tooview detailed Surface Hub data.</span></span>
* <span data-ttu-id="103d3-164">Hozzon létre [riasztások](log-analytics-alerts.md) toonotify, ha a probléma lép fel a Surface hub.</span><span class="sxs-lookup"><span data-stu-id="103d3-164">Create [alerts](log-analytics-alerts.md) toonotify you when issues occur with your Surface Hubs.</span></span>
