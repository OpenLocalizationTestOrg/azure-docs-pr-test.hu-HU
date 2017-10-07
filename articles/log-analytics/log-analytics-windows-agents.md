---
title: "aaaConnect Windows számítógépek tooAzure Naplóelemzési |} Microsoft Docs"
description: "Ez a cikk jeleníti meg hello lépéseket tooconnect hello Windows rendszerű számítógépek a helyszíni infrastruktúra toohello Naplóelemzés szolgáltatás a Microsoft Monitoring Agent (MMA) hello testre szabott verzióját használja."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 932f7b8c-485c-40c1-98e3-7d4c560876d2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7e15f9eeb0440bd2f6557d7215df701526e4f9aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-windows-computers-toohello-log-analytics-service-in-azure"></a><span data-ttu-id="a6c5d-103">Csatlakozás a Windows számítógépek toohello Naplóelemzés szolgáltatás az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="a6c5d-103">Connect Windows computers toohello Log Analytics service in Azure</span></span>

<span data-ttu-id="a6c5d-104">Ez a cikk lépéseit mutatja be, hello tooconnect Windows rendszerű számítógépek a helyszíni infrastruktúra tooOMS munkaterületeken a testreszabott hello Microsoft Monitoring Agent (MMA) használatával.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-104">This article shows hello steps tooconnect Windows computers in your on-premises infrastructure tooOMS workspaces by using a customized version of hello Microsoft Monitoring Agent (MMA).</span></span> <span data-ttu-id="a6c5d-105">Tooinstall kell, és csatlakoztassa az összes, amelyet ahhoz tooonboard Naplóelemzési toohello toosend adatszolgáltatás és tooview és az adatok act hello számítógépek ügynökök.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-105">You need tooinstall and connect agents for all of hello computers that you want tooonboard in order for them toosend data toohello Log Analytics service and tooview and act on that data.</span></span> <span data-ttu-id="a6c5d-106">Minden ügynök toomultiple munkaterületek is tud jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-106">Each agent can report toomultiple workspaces.</span></span>

<span data-ttu-id="a6c5d-107">Telepítés, parancssor, ügynökök telepítése vagy a Szükségeskonfiguráció-konfiguráló (DSC) az Azure Automationben.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-107">You can install agents using Setup, command line, or with Desired State Configuration (DSC) in Azure Automation.</span></span>  

>[!NOTE]
<span data-ttu-id="a6c5d-108">Az Azure-ban futó virtuális gépek, egyszerűbbé teheti az telepítési hello segítségével [virtuálisgép-bővítmény](log-analytics-azure-vm-extension.md).</span><span class="sxs-lookup"><span data-stu-id="a6c5d-108">For virtual machines running in Azure, you can simplify installation by using hello [virtual machine extension](log-analytics-azure-vm-extension.md).</span></span>

<span data-ttu-id="a6c5d-109">Internetkapcsolattal rendelkező számítógépeken a hello ügynök hello kapcsolat toohello Internet toosend adatok tooOMS használja.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-109">On computers with Internet connectivity, hello agent uses hello connection toohello Internet toosend data tooOMS.</span></span> <span data-ttu-id="a6c5d-110">Azok a számítógépek, amelyek nem rendelkeznek internetkapcsolattal, használhatja a proxy vagy az hello [OMS átjáró](log-analytics-oms-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="a6c5d-110">For computers that do not have Internet connectivity, you can use a proxy or hello [OMS Gateway](log-analytics-oms-gateway.md).</span></span>

<span data-ttu-id="a6c5d-111">Csatlakozás a Windows-számítógépek tooOMS egyszerű három egyszerű lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="a6c5d-111">Connecting your Windows computers tooOMS is straightforward using three simple steps:</span></span>

1. <span data-ttu-id="a6c5d-112">Hello ügynök telepítő fájl letöltését hello OMS-portálon</span><span class="sxs-lookup"><span data-stu-id="a6c5d-112">Download hello agent setup file from hello OMS portal</span></span>
2. <span data-ttu-id="a6c5d-113">Dönthet hello metódussal hello ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="a6c5d-113">Install hello agent using hello method you choose</span></span>
3. <span data-ttu-id="a6c5d-114">Hello ügynök beállítása, vagy adja hozzá a további munkaterületek, ha szükséges</span><span class="sxs-lookup"><span data-stu-id="a6c5d-114">Configure hello agent or add additional workspaces, if necessary</span></span>

<span data-ttu-id="a6c5d-115">hello alábbi ábrán látható a Windows rendszerű számítógépek és az OMS hello kapcsolatát telepíteni és konfigurálni az ügynököket után.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-115">hello following diagram shows hello relationship between your Windows computers and OMS after you’ve installed and configured agents.</span></span>

![OMS-közvetlen-ügynök – diagram](./media/log-analytics-windows-agents/oms-direct-agent-diagram.png)

<span data-ttu-id="a6c5d-117">Ha az IT-biztonsági házirendeknek nem engedélyezi a hálózati tooconnect toohello Internet a számítógépek, konfigurálhatja a számítógépek tooconnect toohello OMS-átjáró.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-117">If your IT security policies do not allow computers on your network tooconnect toohello Internet, you can configure your computers tooconnect toohello OMS Gateway.</span></span> <span data-ttu-id="a6c5d-118">További információk és bemutatjuk, hogyan tooconfigure egy OMS átjáró toohello OMS szolgáltatással, a kiszolgálók toocommunicate: [számítógépek tooOMS hello OMS átjáró használatával csatlakozzon](log-analytics-oms-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="a6c5d-118">For more information and steps on how tooconfigure your servers toocommunicate through an OMS Gateway toohello OMS service, see [Connect computers tooOMS using hello OMS Gateway](log-analytics-oms-gateway.md).</span></span>

## <a name="system-requirements-and-required-configuration"></a><span data-ttu-id="a6c5d-119">Rendszerkövetelmények és a szükséges konfigurációval</span><span class="sxs-lookup"><span data-stu-id="a6c5d-119">System requirements and required configuration</span></span>
<span data-ttu-id="a6c5d-120">Telepítéséhez, és ügynökök telepítésére, tekintse át a következő részleteket tooensure hello követelményeknek hello.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-120">Before you install or deploy agents, review hello following details tooensure you meet hello requirements.</span></span>

- <span data-ttu-id="a6c5d-121">Csak telepíthető hello OMS MMA futtató Windows Server 2008 SP 1 vagy újabb vagy Windows 7 SP1 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-121">You can only install hello OMS MMA on computers running Windows Server 2008 SP 1 or later or Windows 7 SP1 or later.</span></span>
- <span data-ttu-id="a6c5d-122">Azure-előfizetés szükséges.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-122">You need an Azure subscription.</span></span>  <span data-ttu-id="a6c5d-123">További információkért lásd: [Ismerkedés a Naplóelemzési](log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a6c5d-123">For more information, see [Get started with Log Analytics](log-analytics-get-started.md).</span></span>
- <span data-ttu-id="a6c5d-124">Minden Windows-számítógép kell tudni tooconnect toohello Internet HTTPS vagy toohello OMS átjáró használatával.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-124">Each Windows computer must be able tooconnect toohello Internet using HTTPS or toohello OMS Gateway.</span></span> <span data-ttu-id="a6c5d-125">Lehet, hogy a kapcsolat közvetlen, egy proxyn keresztül, vagy hello OMS-átjárón keresztül.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-125">This connection can be direct, via a proxy, or through hello OMS Gateway.</span></span>
- <span data-ttu-id="a6c5d-126">Hello OMS MMA telepíthető önálló számítógépek, kiszolgálók és virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-126">You can install hello OMS MMA on stand-alone computers, servers, and virtual machines.</span></span> <span data-ttu-id="a6c5d-127">Ha azt szeretné, hogy tooconnect Azure által üzemeltetett virtuális gépek tooOMS, [csatlakozás Azure virtuális gépek tooLog Analytics](log-analytics-azure-vm-extension.md).</span><span class="sxs-lookup"><span data-stu-id="a6c5d-127">If you want tooconnect Azure-hosted virtual machines tooOMS, see [Connect Azure virtual machines tooLog Analytics](log-analytics-azure-vm-extension.md).</span></span>
- <span data-ttu-id="a6c5d-128">a különböző erőforrások toouse 443-as TCP-portot kell hello ügynököt.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-128">hello agent needs toouse TCP port 443 for various resources.</span></span>

### <a name="network"></a><span data-ttu-id="a6c5d-129">Network (Hálózat)</span><span class="sxs-lookup"><span data-stu-id="a6c5d-129">Network</span></span>

<span data-ttu-id="a6c5d-130">A Windows ügynökök tooconnect tooand register hello OMS szolgáltatással toonetwork erőforrások eléréséhez, beleértve hello portszámok és a tartomány URL-címek kell rendelkezniük.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-130">For Windows agents tooconnect tooand register with hello OMS service, they must have access toonetwork resources, including hello port numbers and domain URLs.</span></span>

- <span data-ttu-id="a6c5d-131">Proxykiszolgálók van szüksége, amely megfelelő proxykiszolgáló erőforrások vannak konfigurálva ügynökbeállítások hello tooensure.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-131">For proxy servers, you need tooensure that hello appropriate proxy server resources are configured in agent settings.</span></span>
- <span data-ttu-id="a6c5d-132">A, amelyek korlátozzák a hozzáférést toohello Internet tűzfalakat akkor vagy a hálózati mérnökök kell tooconfigure a tűzfal toopermit hozzáférés tooOMS.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-132">For firewalls that restrict access toohello Internet, you or your networking engineers need tooconfigure your firewall toopermit access tooOMS.</span></span> <span data-ttu-id="a6c5d-133">Az ügynök beállításait nem kell módosítania.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-133">No action is needed in agent settings.</span></span>

<span data-ttu-id="a6c5d-134">a következő táblázat hello látható kommunikációhoz szükséges erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-134">hello following table shows resources needed for communication.</span></span>

>[!NOTE]
><span data-ttu-id="a6c5d-135">A következő erőforrások hello némelyike említse meg az Operational Insights eddig Naplóelemzési előző nevét.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-135">Some of hello following resources mention Operational Insights, which was a previous name for Log Analytics.</span></span>

| <span data-ttu-id="a6c5d-136">Ügynök erőforrása</span><span class="sxs-lookup"><span data-stu-id="a6c5d-136">Agent Resource</span></span> | <span data-ttu-id="a6c5d-137">Portok</span><span class="sxs-lookup"><span data-stu-id="a6c5d-137">Ports</span></span> | <span data-ttu-id="a6c5d-138">HTTPS-ellenőrzés kihagyása</span><span class="sxs-lookup"><span data-stu-id="a6c5d-138">Bypass HTTPS inspection</span></span> |
|---|---|---|
| <span data-ttu-id="a6c5d-139">*.ods.opinsights.azure.com</span><span class="sxs-lookup"><span data-stu-id="a6c5d-139">*.ods.opinsights.azure.com</span></span> | <span data-ttu-id="a6c5d-140">443</span><span class="sxs-lookup"><span data-stu-id="a6c5d-140">443</span></span> | <span data-ttu-id="a6c5d-141">Igen</span><span class="sxs-lookup"><span data-stu-id="a6c5d-141">Yes</span></span> |
| <span data-ttu-id="a6c5d-142">*.oms.opinsights.azure.com</span><span class="sxs-lookup"><span data-stu-id="a6c5d-142">*.oms.opinsights.azure.com</span></span> | <span data-ttu-id="a6c5d-143">443</span><span class="sxs-lookup"><span data-stu-id="a6c5d-143">443</span></span> | <span data-ttu-id="a6c5d-144">Igen</span><span class="sxs-lookup"><span data-stu-id="a6c5d-144">Yes</span></span> |
| <span data-ttu-id="a6c5d-145">*.blob.core.windows.net</span><span class="sxs-lookup"><span data-stu-id="a6c5d-145">*.blob.core.windows.net</span></span> | <span data-ttu-id="a6c5d-146">443</span><span class="sxs-lookup"><span data-stu-id="a6c5d-146">443</span></span> | <span data-ttu-id="a6c5d-147">Igen</span><span class="sxs-lookup"><span data-stu-id="a6c5d-147">Yes</span></span> |
| <span data-ttu-id="a6c5d-148">*.azure-automation.net</span><span class="sxs-lookup"><span data-stu-id="a6c5d-148">*.azure-automation.net</span></span> | <span data-ttu-id="a6c5d-149">443</span><span class="sxs-lookup"><span data-stu-id="a6c5d-149">443</span></span> | <span data-ttu-id="a6c5d-150">Igen</span><span class="sxs-lookup"><span data-stu-id="a6c5d-150">Yes</span></span> |



## <a name="download-hello-agent-setup-file-from-oms"></a><span data-ttu-id="a6c5d-151">Az OMS Szolgáltatáshoz hello ügynök telepítési fájl letöltése</span><span class="sxs-lookup"><span data-stu-id="a6c5d-151">Download hello agent setup file from OMS</span></span>
1. <span data-ttu-id="a6c5d-152">Az hello OMS-portálon a hello **áttekintése** hello kattintson **beállítások** csempére.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-152">In hello OMS portal, on hello **Overview** page, click hello **Settings** tile.</span></span>  <span data-ttu-id="a6c5d-153">Kattintson a hello **csatlakoztatott források** hello felső fülre.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-153">Click hello **Connected Sources** tab at hello top.</span></span>  
    <span data-ttu-id="a6c5d-154">![Csatlakoztatott adatforrások lap](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)</span><span class="sxs-lookup"><span data-stu-id="a6c5d-154">![Connected Sources tab](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)</span></span>
2. <span data-ttu-id="a6c5d-155">Kattintson a **Windows kiszolgálók** majd **Windows-ügynök letöltése** alkalmazható tooyour számítógép processzor típusa toodownload hello telepítőfájl.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-155">Click **Windows Servers** and then click **Download Windows Agent** applicable tooyour computer processor type toodownload hello setup file.</span></span>
3. <span data-ttu-id="a6c5d-156">A jobbra hello **munkaterület azonosítója**, hello másolás ikonra, és illessze be a Jegyzettömbbe hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-156">On hello right of **Workspace ID**, click hello copy icon and paste hello ID into Notepad.</span></span>
4. <span data-ttu-id="a6c5d-157">A jobbra hello **elsődleges kulcs**, hello másolás ikonra, és illessze be a hello kulcs a Jegyzettömbbe.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-157">On hello right of **Primary Key**, click hello copy icon and paste hello key into Notepad.</span></span>     

## <a name="install-hello-agent-using-setup"></a><span data-ttu-id="a6c5d-158">A telepítőprogram használatával hello ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="a6c5d-158">Install hello agent using setup</span></span>
1. <span data-ttu-id="a6c5d-159">Futtassa a telepítő tooinstall hello ügynököt a számítógépen, amelyet az toomanage.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-159">Run Setup tooinstall hello agent on a computer that you want toomanage.</span></span>
2. <span data-ttu-id="a6c5d-160">A hello kezdőlapján kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-160">On hello Welcome page, click **Next**.</span></span>
3. <span data-ttu-id="a6c5d-161">Hello licencfeltételeket oldalon olvassa el a hello licenc, és kattintson **elfogadom**.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-161">On hello License Terms page, read hello license and then click **I Agree**.</span></span>
4. <span data-ttu-id="a6c5d-162">Hello célmappa oldalon módosíthatja vagy tartsa hello alapértelmezett telepítési mappáját, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-162">On hello Destination Folder page, change or keep hello default installation folder and then click **Next**.</span></span>
5. <span data-ttu-id="a6c5d-163">Hello ügynök telepítésének beállításai lapon tooconnect hello ügynök tooAzure Naplóelemzés (OMS), Operations Manager, dönthet úgy, vagy üresen hello döntések Ha később szeretné tooconfigure hello ügynök.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-163">On hello Agent Setup Options page, you can choose tooconnect hello agent tooAzure Log Analytics (OMS), Operations Manager, or you can leave hello choices blank if you want tooconfigure hello agent later.</span></span> <span data-ttu-id="a6c5d-164">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-164">Click **Next**.</span></span>   
    - <span data-ttu-id="a6c5d-165">Ha úgy dönt, hogy tooconnect tooAzure Naplóelemzés (OMS), illessze be a hello **munkaterület azonosítója** és **Munkaterületkulcsot (elsődleges kulcs)** másolja a Jegyzettömbbe hello előző eljárásban, és kattintson  **Következő**.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-165">If you chose tooconnect tooAzure Log Analytics (OMS), paste hello **Workspace ID** and **Workspace Key (Primary Key)** that you copied into Notepad in hello previous procedure and then click **Next**.</span></span>  
        <span data-ttu-id="a6c5d-166">![illessze be a munkaterület azonosítója és az elsődleges kulcs](./media/log-analytics-windows-agents/connect-workspace.png)</span><span class="sxs-lookup"><span data-stu-id="a6c5d-166">![paste Workspace ID and Primary Key](./media/log-analytics-windows-agents/connect-workspace.png)</span></span>
    - <span data-ttu-id="a6c5d-167">Ha úgy dönt, hogy a kezelő tooconnect tooOperations, írja be a hello **felügyeleti csoport neve**, **felügyeleti kiszolgáló** nevét, és **felügyeleti kiszolgáló portszáma**, majd **Következő**.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-167">If you chose tooconnect tooOperations Manager, type hello **Management Group Name**, **Management Server** name, and **Management Server Port**, and then click **Next**.</span></span> <span data-ttu-id="a6c5d-168">Hello Ügynökműveleti fiók lapon válassza ki vagy a helyi rendszerfiók hello, vagy a helyi tartományi fiókot, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-168">On hello Agent Action Account page, choose either hello Local System account or a local domain account and then click **Next**.</span></span>  
        <span data-ttu-id="a6c5d-169">![felügyeleti csoport konfigurációja](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![a műveleti fiók](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)</span><span class="sxs-lookup"><span data-stu-id="a6c5d-169">![management group configuration](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![agent action account](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)</span></span>

6. <span data-ttu-id="a6c5d-170">Hello készen tooInstall oldalon tekintse át a kiválasztott beállításokat, és kattintson **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-170">On hello Ready tooInstall page, review your choices and then click **Install**.</span></span>
7. <span data-ttu-id="a6c5d-171">Hello konfigurálása sikeresen befejeződött a lapon, és kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-171">On hello Configuration completed successfully page, click **Finish**.</span></span>
8. <span data-ttu-id="a6c5d-172">Amikor végzett, hello **Microsoft-Figyelőügynök** megjelenik **Vezérlőpult**.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-172">When complete, hello **Microsoft Monitoring Agent** appears in **Control Panel**.</span></span> <span data-ttu-id="a6c5d-173">Tekintse át a hiba a konfiguráció, és győződjön meg arról, hogy hello ügynök csatlakoztatott tooOperational Insights (OMS).</span><span class="sxs-lookup"><span data-stu-id="a6c5d-173">You can review your configuration there and verify that hello agent is connected tooOperational Insights (OMS).</span></span> <span data-ttu-id="a6c5d-174">Ha csatlakoztatott tooOMS hello ügynök jeleníti meg a következő üzenet: **hello Microsoft Monitoring Agent sikeresen csatlakozott toohello a Microsoft Operations Management Suite szolgáltatással.**</span><span class="sxs-lookup"><span data-stu-id="a6c5d-174">When connected tooOMS, hello agent displays a message stating: **hello Microsoft Monitoring Agent has successfully connected toohello Microsoft Operations Management Suite service.**</span></span>

## <a name="configure-proxy-settings"></a><span data-ttu-id="a6c5d-175">Proxybeállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a6c5d-175">Configure proxy settings</span></span>

<span data-ttu-id="a6c5d-176">A következő eljárás tooconfigure proxybeállítások hello Microsoft Monitoring Agent vezérlőpultján keresztül hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-176">You can use hello following procedure tooconfigure proxy settings for hello Microsoft Monitoring Agent using Control Panel.</span></span> <span data-ttu-id="a6c5d-177">Szükség van toouse ezt az eljárást minden kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-177">You need toouse this procedure for each server.</span></span> <span data-ttu-id="a6c5d-178">Ha telepíteni kell, amely tooconfigure kiszolgálók számát, bizonyára hasznosnak találja azt egy parancsfájl tooautomate könnyebb toouse ezt a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-178">If you have many servers that you need tooconfigure, you might find it easier toouse a script tooautomate this process.</span></span> <span data-ttu-id="a6c5d-179">Ha igen, tekintse meg a következő eljárással hello [tooconfigure proxybeállítások hello Microsoft Monitoring Agent egy olyan parancsfájllal](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).</span><span class="sxs-lookup"><span data-stu-id="a6c5d-179">If so, see hello next procedure [tooconfigure proxy settings for hello Microsoft Monitoring Agent using a script](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).</span></span>

### <a name="tooconfigure-proxy-settings-for-hello-microsoft-monitoring-agent-using-control-panel"></a><span data-ttu-id="a6c5d-180">a Microsoft Monitoring Agent vezérlőpultján keresztül hello tooconfigure proxybeállítások</span><span class="sxs-lookup"><span data-stu-id="a6c5d-180">tooconfigure proxy settings for hello Microsoft Monitoring Agent using Control Panel</span></span>
1. <span data-ttu-id="a6c5d-181">Nyissa meg **Vezérlőpultot**.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-181">Open **Control Panel**.</span></span>
2. <span data-ttu-id="a6c5d-182">Válassza a **Microsoft Monitoring Agent** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-182">Open **Microsoft Monitoring Agent**.</span></span>
3. <span data-ttu-id="a6c5d-183">Kattintson a hello **proxybeállítások** fülre.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-183">Click hello **Proxy Settings** tab.</span></span>  
    <span data-ttu-id="a6c5d-184">![proxybeállítások lap](./media/log-analytics-windows-agents/proxy-direct-agent-proxy.png)</span><span class="sxs-lookup"><span data-stu-id="a6c5d-184">![proxy settings tab](./media/log-analytics-windows-agents/proxy-direct-agent-proxy.png)</span></span>
4. <span data-ttu-id="a6c5d-185">Válassza ki **proxykiszolgálót használni** és hello URL-címét és portszámát, ha szükséges, hasonló toohello példában is látható.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-185">Select **Use a proxy server** and type hello URL and port number, if one is needed, similar toohello example shown.</span></span> <span data-ttu-id="a6c5d-186">Ha a proxykiszolgálóhoz hitelesítés szükséges, írja be a hello felhasználónév és jelszó tooaccess hello proxykiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-186">If your proxy server requires authentication, type hello username and password tooaccess hello proxy server.</span></span>


### <a name="verify-agent-connectivity-toooms"></a><span data-ttu-id="a6c5d-187">Ellenőrizze az ügynök csatlakozási tooOMS</span><span class="sxs-lookup"><span data-stu-id="a6c5d-187">Verify agent connectivity tooOMS</span></span>

<span data-ttu-id="a6c5d-188">Könnyen ellenőrizheti, hogy az ügynökök kommunikáló OMS eljárást követő hello használata:</span><span class="sxs-lookup"><span data-stu-id="a6c5d-188">You can easily verify whether your agents are communicating with OMS using hello following procedure:</span></span>

1.  <span data-ttu-id="a6c5d-189">A Windows hello-ügynökkel hello számítógépen nyissa meg a Vezérlőpultot.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-189">On hello computer with hello Windows agent, open Control Panel.</span></span>
2.  <span data-ttu-id="a6c5d-190">Nyissa meg a Microsoft Monitoring Agent szolgáltatásnál.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-190">Open Microsoft Monitoring Agent.</span></span>
3.  <span data-ttu-id="a6c5d-191">Kattintson a hello Azure Naplóelemzés (OMS) fülre.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-191">Click hello Azure Log Analytics (OMS) tab.</span></span>
4.  <span data-ttu-id="a6c5d-192">Hello Állapot oszlopban megtekintheti az adott hello ügynök sikeresen csatlakoztatva toohello Operations Management Suite szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-192">In hello Status column, you should see that hello agent connected successfully toohello Operations Management Suite service.</span></span>

![ügynök csatlakoztatva](./media/log-analytics-windows-agents/mma-connected.png)


### <a name="tooconfigure-proxy-settings-for-hello-microsoft-monitoring-agent-using-a-script"></a><span data-ttu-id="a6c5d-194">a Microsoft Monitoring Agent egy olyan parancsfájllal hello tooconfigure proxybeállítások</span><span class="sxs-lookup"><span data-stu-id="a6c5d-194">tooconfigure proxy settings for hello Microsoft Monitoring Agent using a script</span></span>
<span data-ttu-id="a6c5d-195">Másolja a következő mintát, információkat adott tooyour környezet frissíti, mentse a PS1 fájlnév-kiterjesztésűeket, és futtassa a hello parancsfájl minden számítógépen, amely közvetlenül a toohello OMS szolgáltatás hello.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-195">Copy hello following sample, update it with information specific tooyour environment, save it with a PS1 file name extension, and then run hello script on each computer that connects directly toohello OMS service.</span></span>

    param($ProxyDomainName="http://proxy.contoso.com:80", $cred=(Get-Credential))

    # First we get hello Health Service configuration object.  We need toodetermine if we
    #have hello right update rollup with hello API we need.  If not, no need toorun hello rest of hello script.
    $healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

    $proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

    if (!$proxyMethod)
    {
         Write-Output 'Health Service proxy API not present, will not update settings.'
         return
    }

    Write-Output "Clearing proxy settings."
    $healthServiceSettings.SetProxyInfo('', '', '')

    $ProxyUserName = $cred.username

    Write-Output "Setting proxy too$ProxyDomainName with proxy username $ProxyUserName."
    $healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)



## <a name="install-hello-agent-using-hello-command-line"></a><span data-ttu-id="a6c5d-196">Hello parancssorból hello ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="a6c5d-196">Install hello agent using hello command line</span></span>
- <span data-ttu-id="a6c5d-197">Módosítsa, majd a következő példa tooinstall hello ügynök hello parancssorból hello.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-197">Modify and then use hello following example tooinstall hello agent using hello command line.</span></span> <span data-ttu-id="a6c5d-198">hello példa teljesen csendes telepítést hajt végre.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-198">hello example performs a fully silent installation.</span></span>

    >[!NOTE]
    <span data-ttu-id="a6c5d-199">Ha azt szeretné, hogy az ügynök tooupgrade, toouse hello Naplóelemzési kell parancsfájlkezelési API.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-199">If you want tooupgrade an agent, you need toouse hello Log Analytics scripting API.</span></span> <span data-ttu-id="a6c5d-200">Lásd: hello következő szakasz tooupgrade ügynök.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-200">See hello next section tooupgrade an agent.</span></span>

    ```dos
    MMASetup-AMD64.exe /Q:A /R:N /C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1"
    ```

<span data-ttu-id="a6c5d-201">hello ügynök használ annak önkibontó IExpress hello segítségével `/c` parancsot.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-201">hello agent uses IExpress as its self-extractor using hello `/c` command.</span></span> <span data-ttu-id="a6c5d-202">Megtekintheti a hello parancssori kapcsolók: [parancssori kapcsolók a IExpress](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages) és majd frissítés hello példa toosuit igényeinek.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-202">You can see hello command-line switches at [Command-line switches for IExpress](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages) and then update hello example toosuit your needs.</span></span>

|<span data-ttu-id="a6c5d-203">MMA vonatkozó beállítások</span><span class="sxs-lookup"><span data-stu-id="a6c5d-203">MMA-specific options</span></span>                   |<span data-ttu-id="a6c5d-204">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="a6c5d-204">Notes</span></span>         |
|---------------------------------------|--------------|
|<span data-ttu-id="a6c5d-205">ADD_OPINSIGHTS_WORKSPACE</span><span class="sxs-lookup"><span data-stu-id="a6c5d-205">ADD_OPINSIGHTS_WORKSPACE</span></span>               | <span data-ttu-id="a6c5d-206">1 = konfigurálása hello ügynök tooreport tooa munkaterület</span><span class="sxs-lookup"><span data-stu-id="a6c5d-206">1 = Configure hello agent tooreport tooa workspace</span></span>                |
|<span data-ttu-id="a6c5d-207">OPINSIGHTS_WORKSPACE_ID</span><span class="sxs-lookup"><span data-stu-id="a6c5d-207">OPINSIGHTS_WORKSPACE_ID</span></span>                | <span data-ttu-id="a6c5d-208">Hello munkaterület tooadd munkaterület azonosítója (guid)</span><span class="sxs-lookup"><span data-stu-id="a6c5d-208">Workspace Id (guid) for hello workspace tooadd</span></span>                    |
|<span data-ttu-id="a6c5d-209">OPINSIGHTS_WORKSPACE_KEY</span><span class="sxs-lookup"><span data-stu-id="a6c5d-209">OPINSIGHTS_WORKSPACE_KEY</span></span>               | <span data-ttu-id="a6c5d-210">Munkaterület kulcs használt tooinitially hitelesítéshez hello munkaterület</span><span class="sxs-lookup"><span data-stu-id="a6c5d-210">Workspace key used tooinitially authenticate with hello workspace</span></span> |
|<span data-ttu-id="a6c5d-211">OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE</span><span class="sxs-lookup"><span data-stu-id="a6c5d-211">OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE</span></span>  | <span data-ttu-id="a6c5d-212">Adja meg, ahol hello munkaterület hello felhőalapú környezetben</span><span class="sxs-lookup"><span data-stu-id="a6c5d-212">Specify hello cloud environment where hello workspace is located</span></span> <br> <span data-ttu-id="a6c5d-213">0 = azure kereskedelmi cloud (alapértelmezett)</span><span class="sxs-lookup"><span data-stu-id="a6c5d-213">0 = Azure commercial cloud (default)</span></span> <br> <span data-ttu-id="a6c5d-214">1 = azure Government</span><span class="sxs-lookup"><span data-stu-id="a6c5d-214">1 = Azure Government</span></span> |
|<span data-ttu-id="a6c5d-215">OPINSIGHTS_PROXY_URL</span><span class="sxs-lookup"><span data-stu-id="a6c5d-215">OPINSIGHTS_PROXY_URL</span></span>               | <span data-ttu-id="a6c5d-216">URI-JÁNAK hello proxy toouse</span><span class="sxs-lookup"><span data-stu-id="a6c5d-216">URI for hello proxy toouse</span></span> |
|<span data-ttu-id="a6c5d-217">OPINSIGHTS_PROXY_USERNAME</span><span class="sxs-lookup"><span data-stu-id="a6c5d-217">OPINSIGHTS_PROXY_USERNAME</span></span>               | <span data-ttu-id="a6c5d-218">Felhasználónév tooaccess egy hitelesített proxykiszolgálót</span><span class="sxs-lookup"><span data-stu-id="a6c5d-218">Username tooaccess an authenticated proxy</span></span> |
|<span data-ttu-id="a6c5d-219">OPINSIGHTS_PROXY_PASSWORD</span><span class="sxs-lookup"><span data-stu-id="a6c5d-219">OPINSIGHTS_PROXY_PASSWORD</span></span>               | <span data-ttu-id="a6c5d-220">Jelszó tooaccess egy hitelesített proxykiszolgálót</span><span class="sxs-lookup"><span data-stu-id="a6c5d-220">Password tooaccess an authenticated proxy</span></span> |

>[!NOTE]
<span data-ttu-id="a6c5d-221">tooavoid elérte-e hello parancssori hosszkorlátot IExpress, nincs konfigurálva munkaterület hello ügynök telepítése, majd egy parancsfájl tooset konfigurációs hello munkaterülethez.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-221">tooavoid hitting hello command-line length limit of IExpress, install hello agent with no workspace configured and then use a script tooset configuration for hello workspace.</span></span>

>[!NOTE]
<span data-ttu-id="a6c5d-222">Ha egy `Command line option syntax error.` hello használatakor `OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE` paraméter, a megoldás a következő hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="a6c5d-222">If you get a `Command line option syntax error.` when using hello `OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE` parameter, you can use hello following workaround:</span></span>
```dos
MMASetup-AMD64.exe /C /T:.\MMAExtract
cd .\MMAExtract
setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1
```

## <a name="add-a-workspace-using-a-script"></a><span data-ttu-id="a6c5d-223">Egy parancsfájl segítségével munkaterület hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a6c5d-223">Add a workspace using a script</span></span>
<span data-ttu-id="a6c5d-224">A munkaterület hello Naplóelemzési ügynök parancsfájlkezelési API használata a következő példa hello hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="a6c5d-224">Add a workspace using hello Log Analytics agent scripting API with hello following example:</span></span>

```PowerShell
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

<span data-ttu-id="a6c5d-225">az Amerikai Egyesült államokbeli kormányzati, a következő parancsfájl használata hello Azure munkaterületeinek tooadd:</span><span class="sxs-lookup"><span data-stu-id="a6c5d-225">tooadd a workspace in Azure for US Government, use hello following script sample:</span></span>
```PowerShell
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey, 1)
$mma.ReloadConfiguration()
```

>[!NOTE]
<span data-ttu-id="a6c5d-226">Ha a használt hello parancssori vagy parancsfájl korábban tooinstall vagy hello ügynök konfigurálása `EnableAzureOperationalInsights` le lett cserélve `AddCloudWorkspace`.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-226">If you've used hello command line or script previously tooinstall or configure hello agent, `EnableAzureOperationalInsights` was replaced by `AddCloudWorkspace`.</span></span>

## <a name="install-hello-agent-using-dsc-in-azure-automation"></a><span data-ttu-id="a6c5d-227">Azure Automation DSC használata hello ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="a6c5d-227">Install hello agent using DSC in Azure Automation</span></span>

<span data-ttu-id="a6c5d-228">A következő parancsfájl például tooinstall hello ügynök az Azure Automation DSC használata hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-228">You can use hello following script example tooinstall hello agent using DSC in Azure Automation.</span></span> <span data-ttu-id="a6c5d-229">telepíti a 64 bites ügynök hello által azonosított hello hello példa `URI` érték.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-229">hello example installs hello 64-bit agent, identified by hello `URI` value.</span></span> <span data-ttu-id="a6c5d-230">Hello 32 bites verziója tartományvezérlőkkel történő lecserélésével hello URI érték is használható.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-230">You can also use hello 32-bit version by replacing hello URI value.</span></span> <span data-ttu-id="a6c5d-231">hello URI-azonosítók mindkét-verziók a következők:</span><span class="sxs-lookup"><span data-stu-id="a6c5d-231">hello URIs for both versions are:</span></span>

- <span data-ttu-id="a6c5d-232">A Windows 64 bites ügynök – https://go.microsoft.com/fwlink/?LinkId=828603</span><span class="sxs-lookup"><span data-stu-id="a6c5d-232">Windows 64-bit agent - https://go.microsoft.com/fwlink/?LinkId=828603</span></span>
- <span data-ttu-id="a6c5d-233">A Windows 32 bites ügynök – https://go.microsoft.com/fwlink/?LinkId=828604</span><span class="sxs-lookup"><span data-stu-id="a6c5d-233">Windows 32-bit agent - https://go.microsoft.com/fwlink/?LinkId=828604</span></span>


>[!NOTE]
<span data-ttu-id="a6c5d-234">A művelet és a parancsfájl például nem frissíti a meglévő ügynököt.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-234">This procedure and script example will not upgrade an existing agent.</span></span>

1. <span data-ttu-id="a6c5d-235">Hello xPSDesiredStateConfiguration DSC modul importálása a [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) az Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-235">Import hello xPSDesiredStateConfiguration DSC Module from [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) into Azure Automation.</span></span>  
2.  <span data-ttu-id="a6c5d-236">Azure Automation változó eszközök létrehozása *OPSINSIGHTS_WS_ID* és *OPSINSIGHTS_WS_KEY*.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-236">Create Azure Automation variable assets for *OPSINSIGHTS_WS_ID* and *OPSINSIGHTS_WS_KEY*.</span></span> <span data-ttu-id="a6c5d-237">Állítsa be *OPSINSIGHTS_WS_ID* tooyour OMS Naplóelemzési munkaterület azonosítója és *OPSINSIGHTS_WS_KEY* toohello elsődleges kulcs a munkaterületet.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-237">Set *OPSINSIGHTS_WS_ID* tooyour OMS Log Analytics workspace ID and set *OPSINSIGHTS_WS_KEY* toohello primary key of your workspace.</span></span>
3.  <span data-ttu-id="a6c5d-238">Használja a következő parancsfájlt, és mentse MMAgent.ps1 hello</span><span class="sxs-lookup"><span data-stu-id="a6c5d-238">Use hello following script and save it as MMAgent.ps1</span></span>
4.  <span data-ttu-id="a6c5d-239">Módosítsa, majd a következő példa tooinstall hello ügynök az Azure Automation DSC használata hello.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-239">Modify and then use hello following example tooinstall hello agent using DSC in Azure Automation.</span></span> <span data-ttu-id="a6c5d-240">Azure Automation hello Azure Automation-illesztőt vagy parancsmag használatával MMAgent.ps1 importálja.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-240">Import MMAgent.ps1 into Azure Automation by using hello Azure Automation interface or cmdlet.</span></span>
5.  <span data-ttu-id="a6c5d-241">Egy csomópont toohello-konfiguráció hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-241">Assign a node toohello configuration.</span></span> <span data-ttu-id="a6c5d-242">15 percen belül hello csomópont ellenőrzi a konfigurációját, és hello MMA fejlesztőre toohello csomópont.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-242">Within 15 minutes, hello node checks its configuration and hello MMA is pushed toohello node.</span></span>

```PowerShell
Configuration MMAgent
{
    $OIPackageLocalPath = "C:\MMASetup-AMD64.exe"
    $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
    $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"


    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    Node OMSnode {
        Service OIService
        {
            Name = "HealthService"
            State = "Running"
            DependsOn = "[Package]OI"
        }

        xRemoteFile OIPackage {
            Uri = "https://go.microsoft.com/fwlink/?LinkId=828603"
            DestinationPath = $OIPackageLocalPath
        }

        Package OI {
            Ensure = "Present"
            Path  = $OIPackageLocalPath
            Name = "Microsoft Monitoring Agent"
            ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
            Arguments = '/C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
            DependsOn = "[xRemoteFile]OIPackage"
        }
    }
}


```

### <a name="get-hello-latest-productid-value"></a><span data-ttu-id="a6c5d-243">Hello legújabb ProductId érték</span><span class="sxs-lookup"><span data-stu-id="a6c5d-243">Get hello latest ProductId value</span></span>

<span data-ttu-id="a6c5d-244">Hello `ProductId value` parancsfájl hello MMAgent.ps1 egyedi tooeach ügynök verziója.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-244">hello `ProductId value` in hello MMAgent.ps1 script is unique tooeach agent version.</span></span> <span data-ttu-id="a6c5d-245">Minden ügynök frissített verziójának közzétételekor hello ProductId érték megváltozik.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-245">When an updated version of each agent is published, hello ProductId value changes.</span></span> <span data-ttu-id="a6c5d-246">Igen hello ProductId a jövőbeli hello megváltozásakor található hello ügynök verziója egyszerű parancsfájl használatával.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-246">So, when hello ProductId changes in hello future, you can find hello agent version using a simple script.</span></span> <span data-ttu-id="a6c5d-247">Miután hello ügynök legfrissebb teszt kiszolgálóra telepíteni, a következő parancsfájl tooget telepítve hello ProductId érték hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-247">After you have hello latest agent version installed on a test server, you can use hello following script tooget hello installed ProductId value.</span></span> <span data-ttu-id="a6c5d-248">Hello legújabb ProductId értéket használva frissítheti hello MMAgent.ps1 parancsfájl hello értéket.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-248">Using hello latest ProductId value, you can update hello value in hello MMAgent.ps1 script.</span></span>

```PowerShell
$InstalledApplications  = Get-ChildItem hklm:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall


foreach ($Application in $InstalledApplications)

{

     $Key = Get-ItemProperty $Application.PSPath

     if ($Key.DisplayName -eq "Microsoft Monitoring Agent")

     {

        $Key.DisplayName

        Write-Output ("Product ID is: " + $Key.PSChildName.Substring(1,$Key.PSChildName.Length -2))

     }

}  
```

## <a name="configure-an-agent-manually-or-add-additional-workspaces"></a><span data-ttu-id="a6c5d-249">Az ügynök manuális konfigurálásához, vagy adja hozzá a további munkaterületek</span><span class="sxs-lookup"><span data-stu-id="a6c5d-249">Configure an agent manually or add additional workspaces</span></span>
<span data-ttu-id="a6c5d-250">Ha telepített ügynökök, de nem állította be, vagy ha azt szeretné, hogy hello ügynök tooreport toomultiple munkaterületek, használja a következő információk tooenable ügynök hello, vagy konfigurálja azt újra.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-250">If you've installed agents but did not configure them or if you want hello agent tooreport toomultiple workspaces, you can use hello following information tooenable an agent or reconfigure it.</span></span> <span data-ttu-id="a6c5d-251">Hello ügynök konfigurálása után regisztrálja az hello ügynök szolgáltatás, és megkapja a szükséges konfigurációs adatokat és a felügyeleti csomagok, amelyekben a megoldással kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-251">After you've configured hello agent, it will register with hello agent service and will get necessary configuration information and management packs that contain solution information.</span></span>

1. <span data-ttu-id="a6c5d-252">Ha már telepítette a Microsoft Monitoring Agent hello, nyissa meg a **Vezérlőpult**.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-252">After you've installed hello Microsoft Monitoring Agent, open **Control Panel**.</span></span>
2. <span data-ttu-id="a6c5d-253">Nyissa meg **Microsoft-Figyelőügynök** majd hello **Azure Naplóelemzés (OMS)** fülre.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-253">Open **Microsoft Monitoring Agent** and then click hello **Azure Log Analytics (OMS)** tab.</span></span>   
3. <span data-ttu-id="a6c5d-254">Kattintson a **Hozzáadás** tooopen hello **hozzáadni a Naplóelemzési munkaterület** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-254">Click **Add** tooopen hello **Add a Log Analytics Workspace** box.</span></span>
4. <span data-ttu-id="a6c5d-255">Beillesztés hello **munkaterület azonosítója** és **Munkaterületkulcsot (elsődleges kulcs)** másolja a Jegyzettömbbe hello munkaterület tooadd szeretne, és kattintson az előző eljárásban **OK**.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-255">Paste hello **Workspace ID** and **Workspace Key (Primary Key)** that you copied into Notepad in a previous procedure for hello workspace that you want tooadd and then click **OK**.</span></span>  
    <span data-ttu-id="a6c5d-256">![Operational insights szolgáltatás konfigurálása](./media/log-analytics-windows-agents/add-workspace.png)</span><span class="sxs-lookup"><span data-stu-id="a6c5d-256">![configure Operational Insights](./media/log-analytics-windows-agents/add-workspace.png)</span></span>

<span data-ttu-id="a6c5d-257">Adatgyűjtés hello ügynök által felügyelt számítógépek, hello száma OMS által felügyelt számítógépek megjelennek a hello hello OMS-portálon **csatlakoztatott források** lapján **beállítások** ,  **Csatlakoztatott**.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-257">After data is collected from computers monitored by hello agent, hello number of computers monitored by OMS will appear in hello OMS portal on hello **Connected Sources** tab in **Settings** as **Servers Connected**.</span></span>


## <a name="toodisable-an-agent"></a><span data-ttu-id="a6c5d-258">az ügynök toodisable</span><span class="sxs-lookup"><span data-stu-id="a6c5d-258">toodisable an agent</span></span>
1. <span data-ttu-id="a6c5d-259">Hello ügynök telepítése után nyissa meg a **Vezérlőpult**.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-259">After installing hello agent, open **Control Panel**.</span></span>
2. <span data-ttu-id="a6c5d-260">Nyissa meg a Microsoft Monitoring Agent, és kattintson a hello **Azure Naplóelemzés (OMS)** fülre.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-260">Open Microsoft Monitoring Agent and then click hello **Azure Log Analytics (OMS)** tab.</span></span>
3. <span data-ttu-id="a6c5d-261">Válasszon egy munkaterületet, és kattintson a **eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-261">Select a workspace and then click **Remove**.</span></span> <span data-ttu-id="a6c5d-262">Ismételje meg ezt a lépést minden egyéb munkaterületekkel.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-262">Repeat this step for all other workspaces.</span></span>


## <a name="optionally-configure-agents-tooreport-tooan-operations-manager-management-group"></a><span data-ttu-id="a6c5d-263">Nem kötelező lépésként beállíthatja az ügynökök tooreport tooan Operations Manager felügyeleti csoport</span><span class="sxs-lookup"><span data-stu-id="a6c5d-263">Optionally, configure agents tooreport tooan Operations Manager management group</span></span>

<span data-ttu-id="a6c5d-264">Ha az Operations Manager az informatikai infrastruktúrát használ, az Operations Manager ügynök is használhatja a hello MMA ügynök.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-264">If you use Operations Manager in your IT infrastructure, you can also use hello MMA agent as an Operations Manager agent.</span></span>

### <a name="tooconfigure-mma-agents-tooreport-tooan-operations-manager-management-group"></a><span data-ttu-id="a6c5d-265">tooconfigure MMA ügynökök tooreport tooan Operations Manager felügyeleti csoport</span><span class="sxs-lookup"><span data-stu-id="a6c5d-265">tooconfigure MMA agents tooreport tooan Operations Manager management group</span></span>
1.  <span data-ttu-id="a6c5d-266">Hello számítógépre, amelyen telepítve van-e az hello ügynök, nyissa meg **Vezérlőpult**.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-266">On hello computer where hello agent is installed, open **Control Panel**.</span></span>  
2.  <span data-ttu-id="a6c5d-267">Nyissa meg **Microsoft-Figyelőügynök** majd hello **Operations Manager** fülre.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-267">Open **Microsoft Monitoring Agent** and then click hello **Operations Manager** tab.</span></span>  
    <span data-ttu-id="a6c5d-268">![A Microsoft figyelési ügynök Operations Manager lap](./media/log-analytics-windows-agents/om-mg01.png)</span><span class="sxs-lookup"><span data-stu-id="a6c5d-268">![Microsoft Monitoring Agent Operations Manager tab](./media/log-analytics-windows-agents/om-mg01.png)</span></span>
3.  <span data-ttu-id="a6c5d-269">Ha az Operations Manager-kiszolgáló Active Directory integrációja a, kattintson a **az AD DS-ből felügyeleti csoport-hozzárendelések automatikus frissítése**.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-269">If your Operations Manager servers have integration with Active Directory, click **Automatically update management group assignments from AD DS**.</span></span>
4.  <span data-ttu-id="a6c5d-270">Kattintson a **Hozzáadás** tooopen hello **felügyeleti csoport hozzáadása** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-270">Click **Add** tooopen hello **Add a Management Group** dialog box.</span></span>  
    <span data-ttu-id="a6c5d-271">![A Microsoft Monitoring Agent egy felügyeleti csoport hozzáadása](./media/log-analytics-windows-agents/oms-mma-om02.png)</span><span class="sxs-lookup"><span data-stu-id="a6c5d-271">![Microsoft Monitoring Agent Add a Management Group](./media/log-analytics-windows-agents/oms-mma-om02.png)</span></span>
5.  <span data-ttu-id="a6c5d-272">A **felügyeleti csoport neve** mezőbe, a felügyeleti csoport hello típusnév.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-272">In **Management group name** box, type hello name of your management group.</span></span>
6.  <span data-ttu-id="a6c5d-273">A hello **elsődleges felügyeleti kiszolgáló** mezőbe, a típus hello hello elsődleges felügyeleti kiszolgáló számítógépneve.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-273">In hello **Primary management server** box, type hello computer name of hello primary management server.</span></span>
7.  <span data-ttu-id="a6c5d-274">A hello **felügyeleti kiszolgáló portszáma** mezőbe típus hello TCP-portszámot.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-274">In hello **Management server port** box, type hello TCP port number.</span></span>
8.  <span data-ttu-id="a6c5d-275">A **Ügynökműveleti fiók**, vagy a helyi rendszerfiók hello, vagy a helyi tartományi fiókot válasszon.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-275">Under **Agent Action Account**, choose either hello Local System account or a local domain account.</span></span>
9.  <span data-ttu-id="a6c5d-276">Kattintson a **OK** tooclose hello **felügyeleti csoport hozzáadása** párbeszédpanel megnyitásához, majd kattintson **OK** tooclose hello **Microsoft Monitoring Agent tulajdonságai**párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-276">Click **OK** tooclose hello **Add a Management Group** dialog box and then click **OK** tooclose hello **Microsoft Monitoring Agent Properties** dialog box.</span></span>


## <a name="next-steps"></a><span data-ttu-id="a6c5d-277">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a6c5d-277">Next steps</span></span>

- <span data-ttu-id="a6c5d-278">[Log Analytics-megoldások hozzáadása hello megoldások gyűjtemény](log-analytics-add-solutions.md) tooadd funkciókat és összefog adatokat.</span><span class="sxs-lookup"><span data-stu-id="a6c5d-278">[Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md) tooadd functionality and gather data.</span></span>
