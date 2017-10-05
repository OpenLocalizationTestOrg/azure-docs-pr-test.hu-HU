---
title: "Az Azure Naplóelemzés hubok figyelése |} Microsoft Docs"
description: "A Surface Hub megoldást használni a Surface hub állapotának nyomon követése és megérteni, hogyan kívánják felhasználni."
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
ms.openlocfilehash: b6ecd0d09589fec85c1633f528afc1165c346b7f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-surface-hubs-with-log-analytics-to-track-their-health"></a><span data-ttu-id="6c7db-103">A Naplóelemzési nyomon követéséhez az állapotukat Surface hub figyelése</span><span class="sxs-lookup"><span data-stu-id="6c7db-103">Monitor Surface Hubs with Log Analytics to track their health</span></span>

![Surface Hub szimbólum](./media/log-analytics-surface-hubs/surface-hub-symbol.png)

<span data-ttu-id="6c7db-105">Ez a cikk ismerteti, hogyan használhatja a Surface Hub-megoldás a Log Analyticshez figyelése a Microsoft Surface Hub eszközöket a Microsoft Operations Management Suite (OMS).</span><span class="sxs-lookup"><span data-stu-id="6c7db-105">This article describes how you can use the Surface Hub solution in Log Analytics to monitor Microsoft Surface Hub devices with the Microsoft Operations Management Suite (OMS).</span></span> <span data-ttu-id="6c7db-106">Naplóelemzési segítségével nyomon követheti a Surface hub állapotát, valamint megérteni, hogyan kívánják felhasználni.</span><span class="sxs-lookup"><span data-stu-id="6c7db-106">Log Analytics helps you track the health of your Surface Hubs as well as understand how they are being used.</span></span>

<span data-ttu-id="6c7db-107">Minden egyes Surface Hub a Microsoft Monitoring Agent telepítve van.</span><span class="sxs-lookup"><span data-stu-id="6c7db-107">Each Surface Hub has the Microsoft Monitoring Agent installed.</span></span> <span data-ttu-id="6c7db-108">Annak az ügynököt, hogy küldhet adatokat a Surface Hub az OMS keresztül.</span><span class="sxs-lookup"><span data-stu-id="6c7db-108">Its through the agent that you can send data from your Surface Hub to OMS.</span></span> <span data-ttu-id="6c7db-109">Naplófájlok a Surface hub és a olvasni, majd az OMS szolgáltatáshoz kerülnek.</span><span class="sxs-lookup"><span data-stu-id="6c7db-109">Log files are read from your Surface Hubs and are then are sent to the OMS service.</span></span> <span data-ttu-id="6c7db-110">Problémák, például kiszolgálók nem kapcsolódik, a naptár, a szinkronizálás nem, vagy ha a fiók nem tud bejelentkezni az Skype OMS láthatók, a Surface Hub irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="6c7db-110">Issues like servers being offline, the calendar not syncing, or if the device account is unable to log into Skype are shown in OMS in the Surface Hub dashboard.</span></span> <span data-ttu-id="6c7db-111">Az adatok az irányítópult használatával azonosíthatja az eszközök nem fut, vagy amelyek egyéb problémákat, amelynek, és potenciálisan alkalmazása az észlelt probléma javítását.</span><span class="sxs-lookup"><span data-stu-id="6c7db-111">By using the data in the dashboard, you can identify devices that are not running, or that are having other problems, and potentially apply fixes for the detected issues.</span></span>

## <a name="installing-and-configuring-the-solution"></a><span data-ttu-id="6c7db-112">Telepítése és a megoldás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6c7db-112">Installing and configuring the solution</span></span>
<span data-ttu-id="6c7db-113">Az alábbi információk segítségével telepítse és konfigurálja a megoldást.</span><span class="sxs-lookup"><span data-stu-id="6c7db-113">Use the following information to install and configure the solution.</span></span> <span data-ttu-id="6c7db-114">Ahhoz, hogy a Surface hub kezelése a Microsoft Operations Management Suite (OMS) származó, a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="6c7db-114">In order to manage your Surface Hubs from the Microsoft Operations Management Suite (OMS), you'll need the following:</span></span>

* <span data-ttu-id="6c7db-115">Egy érvényes előfizetést [OMS](http://www.microsoft.com/oms).</span><span class="sxs-lookup"><span data-stu-id="6c7db-115">A valid subscription to [OMS](http://www.microsoft.com/oms).</span></span>
* <span data-ttu-id="6c7db-116">Egy [OMS előfizetés](https://azure.microsoft.com/pricing/details/log-analytics/) szintje, amely támogatja a figyelni kívánt eszközök számára.</span><span class="sxs-lookup"><span data-stu-id="6c7db-116">An [OMS subscription](https://azure.microsoft.com/pricing/details/log-analytics/) level that will support the number of devices you want to monitor.</span></span> <span data-ttu-id="6c7db-117">OMS árképzési változó attól függően, hogy hány eszköz regisztrálva van, és mennyi adatot azt folyamatokat.</span><span class="sxs-lookup"><span data-stu-id="6c7db-117">OMS pricing varies depending on how many devices are enrolled, and how much data it processes.</span></span> <span data-ttu-id="6c7db-118">Érdemes figyelembe venni a, a Surface Hub-bevezetés tervezése során.</span><span class="sxs-lookup"><span data-stu-id="6c7db-118">You'll want to take this into consideration when planning your Surface Hub rollout.</span></span>

<span data-ttu-id="6c7db-119">A következő fog egy OMS-előfizetés hozzáadása a meglévő Microsoft Azure-előfizetés vagy hozzon létre egy új munkaterületet közvetlenül az OMS-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="6c7db-119">Next, you will either add an OMS subscription to your existing Microsoft Azure subscription or create a new workspace directly through the OMS portal.</span></span> <span data-ttu-id="6c7db-120">Részletes utasításokat a következő módszerek egyikével érték [Ismerkedés a Naplóelemzési](log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="6c7db-120">Detailed instructions for using either method is at [Get started with Log Analytics](log-analytics-get-started.md).</span></span> <span data-ttu-id="6c7db-121">Az OMS-előfizetés beállítása után két módon lehet regisztrálni kell az Surface Hub eszközöket:</span><span class="sxs-lookup"><span data-stu-id="6c7db-121">Once the OMS subscription is set up, there are two ways to enroll your Surface Hub devices:</span></span>

* <span data-ttu-id="6c7db-122">Automatikusan Intune-nal</span><span class="sxs-lookup"><span data-stu-id="6c7db-122">Automatically through Intune</span></span>
* <span data-ttu-id="6c7db-123">Manuálisan osztályig **beállítások** Surface Hub eszközén.</span><span class="sxs-lookup"><span data-stu-id="6c7db-123">Manually through **Settings** on your Surface Hub device.</span></span>

## <a name="set-up-monitoring"></a><span data-ttu-id="6c7db-124">Állítsa be a figyelést,</span><span class="sxs-lookup"><span data-stu-id="6c7db-124">Set up monitoring</span></span>
<span data-ttu-id="6c7db-125">Állapotát és tevékenységét a Surface Hub OMS Naplóelemzési használatával figyelheti.</span><span class="sxs-lookup"><span data-stu-id="6c7db-125">You can monitor the health and activity of your Surface Hub using Log Analytics in OMS.</span></span> <span data-ttu-id="6c7db-126">A Surface Hub az OMS regisztrálása az Intune-ban, vagy helyileg segítségével **beállítások** a Surface hub.</span><span class="sxs-lookup"><span data-stu-id="6c7db-126">You can enroll the Surface Hub in OMS by using Intune, or locally by using **Settings** on the Surface Hub.</span></span>

## <a name="connect-surface-hubs-to-oms-through-intune"></a><span data-ttu-id="6c7db-127">Hubok csatlakoztatni az OMS Szolgáltatáshoz Intune-nal</span><span class="sxs-lookup"><span data-stu-id="6c7db-127">Connect Surface Hubs to OMS through Intune</span></span>
<span data-ttu-id="6c7db-128">A munkaterület Azonosítóját és kulcsát az OMS-munkaterület, a Surface hub kezelésére lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="6c7db-128">You'll need the workspace ID and workspace key for the OMS workspace that will manage your Surface Hubs.</span></span> <span data-ttu-id="6c7db-129">Az OMS-portálon szerintiek kérheti le.</span><span class="sxs-lookup"><span data-stu-id="6c7db-129">You can get those from the OMS portal.</span></span>

<span data-ttu-id="6c7db-130">Intune-ban egy Microsoft-termék, amely lehetővé teszi az OMS konfigurációs beállításait egy vagy több eszköz központi kezelését.</span><span class="sxs-lookup"><span data-stu-id="6c7db-130">Intune is a Microsoft product that allows you to centrally manage the OMS configuration settings that are applied to one or more of your devices.</span></span> <span data-ttu-id="6c7db-131">Kövesse az alábbi lépéseket az eszközök Intune-nal:</span><span class="sxs-lookup"><span data-stu-id="6c7db-131">Follow these steps to configure your devices through Intune:</span></span>

1. <span data-ttu-id="6c7db-132">Jelentkezzen be az Intune-hoz.</span><span class="sxs-lookup"><span data-stu-id="6c7db-132">Sign in to Intune.</span></span>
2. <span data-ttu-id="6c7db-133">Navigáljon a **beállítások** > **csatlakoztatott adatforrások**.</span><span class="sxs-lookup"><span data-stu-id="6c7db-133">Navigate to **Settings** > **Connected Sources**.</span></span>
3. <span data-ttu-id="6c7db-134">Hozzon létre, vagy szerkesztheti a házirendet, a Surface Hub-sablon alapján.</span><span class="sxs-lookup"><span data-stu-id="6c7db-134">Create or edit a policy based on the Surface Hub template.</span></span>
4. <span data-ttu-id="6c7db-135">Lépjen a OMS (Azure Operational Insights) szakaszra a házirend, és adja hozzá a *munkaterület azonosítója* és *Munkaterületkulcsot* a házirendhez.</span><span class="sxs-lookup"><span data-stu-id="6c7db-135">Navigate to the OMS (Azure Operational Insights) section of the policy, and add the *Workspace ID* and *Workspace Key* to the policy.</span></span>
5. <span data-ttu-id="6c7db-136">A házirend mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="6c7db-136">Save the policy.</span></span>
6. <span data-ttu-id="6c7db-137">A megfelelő csoporthoz az eszközök társítja a házirendet.</span><span class="sxs-lookup"><span data-stu-id="6c7db-137">Associate the policy with the appropriate group of devices.</span></span>

   ![Az Intune-házirend](./media/log-analytics-surface-hubs/intune.png)

<span data-ttu-id="6c7db-139">Az Intune majd szinkronizál az OMS-beállítások az eszközök a célcsoportban, ha regisztrálja azokat az OMS-munkaterület.</span><span class="sxs-lookup"><span data-stu-id="6c7db-139">Intune then syncs the OMS settings with the devices in the target group, enrolling them in your OMS workspace.</span></span>

## <a name="connect-surface-hubs-to-oms-using-the-settings-app"></a><span data-ttu-id="6c7db-140">Surface hub csatlakoztatni az OMS Szolgáltatáshoz, a beállítások alkalmazással</span><span class="sxs-lookup"><span data-stu-id="6c7db-140">Connect Surface Hubs to OMS using the Settings app</span></span>
<span data-ttu-id="6c7db-141">A munkaterület Azonosítóját és kulcsát az OMS-munkaterület, a Surface hub kezelésére lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="6c7db-141">You'll need the workspace ID and workspace key for the OMS workspace that will manage your Surface Hubs.</span></span> <span data-ttu-id="6c7db-142">Az OMS-portálon szerintiek kérheti le.</span><span class="sxs-lookup"><span data-stu-id="6c7db-142">You can get those from the OMS portal.</span></span>

<span data-ttu-id="6c7db-143">Ha az Intune használatával nem a környezete kezeléséhez, manuálisan keresztül az eszközök regisztrálása **beállítások** minden Surface hub:</span><span class="sxs-lookup"><span data-stu-id="6c7db-143">If you don't use Intune to manage your environment, you can enroll devices manually through **Settings** on each Surface Hub:</span></span>

1. <span data-ttu-id="6c7db-144">Nyissa meg a Surface Hub **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="6c7db-144">From your Surface Hub, open **Settings**.</span></span>
2. <span data-ttu-id="6c7db-145">Adja meg az eszköz rendszergazdai hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="6c7db-145">Enter the device admin credentials when prompted.</span></span>
3. <span data-ttu-id="6c7db-146">Kattintson a **az eszköz**, és az alatt **figyelés**, kattintson a **OMS-beállítások konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="6c7db-146">Click **This device**, and the under **Monitoring**, click **Configure OMS Settings**.</span></span>
4. <span data-ttu-id="6c7db-147">Válassza ki **engedélyezze a megfigyelést**.</span><span class="sxs-lookup"><span data-stu-id="6c7db-147">Select **Enable monitoring**.</span></span>
5. <span data-ttu-id="6c7db-148">Az OMS-beállításai párbeszédpanelen írja be a **munkaterület azonosítója** , és írja be a **Munkaterületkulcsot**.</span><span class="sxs-lookup"><span data-stu-id="6c7db-148">In the OMS settings dialog, type the **Workspace ID** and type the **Workspace Key**.</span></span>  
   <span data-ttu-id="6c7db-149">![beállítások](./media/log-analytics-surface-hubs/settings.png)</span><span class="sxs-lookup"><span data-stu-id="6c7db-149">![settings](./media/log-analytics-surface-hubs/settings.png)</span></span>
6. <span data-ttu-id="6c7db-150">Kattintson a **OK** a konfigurálás befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="6c7db-150">Click **OK** to complete the configuration.</span></span>

<span data-ttu-id="6c7db-151">Megerősítést felszólító-e az OMS-konfigurációja sikeresen alkalmazta az eszköz jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6c7db-151">A confirmation appears telling you whether or not the OMS configuration was successfully applied to the device.</span></span> <span data-ttu-id="6c7db-152">Ha igen, megjelenik egy üzenet arról, hogy az ügynök sikeresen csatlakozott az OMS szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="6c7db-152">If it was, a message appears stating that the agent successfully connected to the OMS service.</span></span> <span data-ttu-id="6c7db-153">Az eszköz ezután elindítja az adatok küldése az OMS-be, ahol megtekintheti és a működésre.</span><span class="sxs-lookup"><span data-stu-id="6c7db-153">The device then starts sending data to OMS where you can view and act on it.</span></span>

## <a name="monitor-surface-hubs"></a><span data-ttu-id="6c7db-154">A figyelő hubok</span><span class="sxs-lookup"><span data-stu-id="6c7db-154">Monitor Surface Hubs</span></span>
<span data-ttu-id="6c7db-155">Figyelés, a Surface hub OMS használata sokkal hasonlóan a további regisztrált eszközök figyelését.</span><span class="sxs-lookup"><span data-stu-id="6c7db-155">Monitoring your Surface Hubs using OMS is much like monitoring any other enrolled devices.</span></span>

1. <span data-ttu-id="6c7db-156">Jelentkezzen be az OMS-portálon.</span><span class="sxs-lookup"><span data-stu-id="6c7db-156">Sign in to the OMS portal.</span></span>
2. <span data-ttu-id="6c7db-157">Nyissa meg a Surface Hub megoldás pack irányítópult.</span><span class="sxs-lookup"><span data-stu-id="6c7db-157">Navigate to the Surface Hub solution pack dashboard.</span></span>
3. <span data-ttu-id="6c7db-158">Az eszköz állapota akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6c7db-158">Your device's health is displayed.</span></span>

   ![Surface Hub irányítópult](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

<span data-ttu-id="6c7db-160">Létrehozhat [riasztások](log-analytics-alerts.md) meglévő vagy egyéni napló keresés alapján.</span><span class="sxs-lookup"><span data-stu-id="6c7db-160">You can create [alerts](log-analytics-alerts.md) based on existing or custom log searches.</span></span> <span data-ttu-id="6c7db-161">Az OMS gyűjti össze a Surface hub adatok használatával is kereshet problémákat és az eszközök meghatározó feltételeket a riasztást.</span><span class="sxs-lookup"><span data-stu-id="6c7db-161">Using the data the OMS collects from your Surface Hubs, you can search for issues and alert on the conditions that you define for your devices.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6c7db-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6c7db-162">Next steps</span></span>
* <span data-ttu-id="6c7db-163">Használjon [Log Analytics-e jelentkezni a keresések](log-analytics-log-searches.md) Surface Hub részletes adatainak megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="6c7db-163">Use [Log searches in Log Analytics](log-analytics-log-searches.md) to view detailed Surface Hub data.</span></span>
* <span data-ttu-id="6c7db-164">Hozzon létre [riasztások](log-analytics-alerts.md) értesítse arról, ha a probléma lép fel a Surface hub.</span><span class="sxs-lookup"><span data-stu-id="6c7db-164">Create [alerts](log-analytics-alerts.md) to notify you when issues occur with your Surface Hubs.</span></span>
