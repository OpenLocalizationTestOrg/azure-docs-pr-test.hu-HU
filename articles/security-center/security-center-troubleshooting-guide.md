---
title: "Biztonsági központ hibaelhárítási útmutató aaaAzure |} Microsoft Docs"
description: "A dokumentum segítséget nyújt az Azure Security Centerben tootroubleshoot problémákat."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 44462de6-2cc5-4672-b1d3-dbb4749a28cd
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: yurid
ms.openlocfilehash: 78b3c49eb66fe3a4f80efbba3a47a87b039c07ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-troubleshooting-guide"></a><span data-ttu-id="6e408-103">Azure Security Center – Hibaelhárítási útmutató</span><span class="sxs-lookup"><span data-stu-id="6e408-103">Azure Security Center Troubleshooting Guide</span></span>
<span data-ttu-id="6e408-104">Ez az útmutató informatikai (IT) szakemberek, adatbiztonsági elemzők és felhő rendszergazdák, amelynek szervezetek az Azure Security Center használ, és szükség, tootroubleshoot Security Center kapcsolatos hiba lépett fel.</span><span class="sxs-lookup"><span data-stu-id="6e408-104">This guide is for information technology (IT) professionals, information security analysts, and cloud administrators whose organizations are using Azure Security Center and need tootroubleshoot Security Center related issues.</span></span>

>[!NOTE] 
><span data-ttu-id="6e408-105">Korai. június 2017 verziótól kezdve a Security Center hello Microsoft Monitoring Agent toocollect és a tároló adatait használja.</span><span class="sxs-lookup"><span data-stu-id="6e408-105">Beginning in early June 2017, Security Center uses hello Microsoft Monitoring Agent toocollect and store data.</span></span> <span data-ttu-id="6e408-106">Lásd: [Azure Security Center Platform áttelepítési](security-center-platform-migration.md) további toolearn.</span><span class="sxs-lookup"><span data-stu-id="6e408-106">See [Azure Security Center Platform Migration](security-center-platform-migration.md) toolearn more.</span></span> <span data-ttu-id="6e408-107">a cikkben szereplő információkat hello Security Center funkció átmenet toohello Microsoft Monitoring Agent után jelöli.</span><span class="sxs-lookup"><span data-stu-id="6e408-107">hello information in this article represents Security Center functionality after transition toohello Microsoft Monitoring Agent.</span></span>
>

## <a name="troubleshooting-guide"></a><span data-ttu-id="6e408-108">Hibaelhárítási útmutató</span><span class="sxs-lookup"><span data-stu-id="6e408-108">Troubleshooting guide</span></span>
<span data-ttu-id="6e408-109">Ez az útmutató ismerteti, hogyan tootroubleshoot Security Center kapcsolatos hiba lépett fel.</span><span class="sxs-lookup"><span data-stu-id="6e408-109">This guide explains how tootroubleshoot Security Center related issues.</span></span> <span data-ttu-id="6e408-110">Hello hibaelhárítási végezheti el a Security Center legtöbb történik hello első megtekintésével [napló](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) hello rekordok összetevő nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="6e408-110">Most of hello troubleshooting done in Security Center takes place by first looking at hello [Audit Log](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) records for hello failed component.</span></span> <span data-ttu-id="6e408-111">A naplókból a következők állapíthatók meg:</span><span class="sxs-lookup"><span data-stu-id="6e408-111">Through audit logs, you can determine:</span></span>

* <span data-ttu-id="6e408-112">A végrehajtott műveletek</span><span class="sxs-lookup"><span data-stu-id="6e408-112">Which operations were taken place</span></span>
* <span data-ttu-id="6e408-113">Hello művelet korábban kik kezdeményeztek</span><span class="sxs-lookup"><span data-stu-id="6e408-113">Who initiated hello operation</span></span>
* <span data-ttu-id="6e408-114">Ha a hello művelet történt</span><span class="sxs-lookup"><span data-stu-id="6e408-114">When hello operation occurred</span></span>
* <span data-ttu-id="6e408-115">hello művelet hello állapotát</span><span class="sxs-lookup"><span data-stu-id="6e408-115">hello status of hello operation</span></span>
* <span data-ttu-id="6e408-116">hello értékek, amelyek segíthetnek tulajdonságokat kutatás hello művelet</span><span class="sxs-lookup"><span data-stu-id="6e408-116">hello values of other properties that might help you research hello operation</span></span>

<span data-ttu-id="6e408-117">hello napló tartalmazza az erőforrásokon végrehajtott minden írási műveletek (PUT, POST, Törlés), azonban nem tartalmazza az olvasási műveletek (GET).</span><span class="sxs-lookup"><span data-stu-id="6e408-117">hello audit log contains all write operations (PUT, POST, DELETE) performed on your resources, however it does not include read operations (GET).</span></span>

## <a name="microsoft-monitoring-agent"></a><span data-ttu-id="6e408-118">Microsoft Monitoring Agent</span><span class="sxs-lookup"><span data-stu-id="6e408-118">Microsoft Monitoring Agent</span></span>
<span data-ttu-id="6e408-119">A Security Center a Microsoft Monitoring Agent hello használ, – hello ugyanannak az ügynöknek használt hello Operations Management Suite és Naplóelemzési service – az Azure virtuális gépek toocollect biztonsági adatait.</span><span class="sxs-lookup"><span data-stu-id="6e408-119">Security Center uses hello Microsoft Monitoring Agent – this is hello same agent used by hello Operations Management Suite and Log Analytics service – toocollect security data from your Azure virtual machines.</span></span> <span data-ttu-id="6e408-120">Miután adatgyűjtés engedélyezve van, és hello ügynök megfelelően van telepítve a célszámítógépen hello, hello folyamat az alábbi végrehajtása kell lennie:</span><span class="sxs-lookup"><span data-stu-id="6e408-120">After data collection is enabled and hello agent is correctly installed in hello target machine, hello process below should be in execution:</span></span>

* <span data-ttu-id="6e408-121">HealthService.exe</span><span class="sxs-lookup"><span data-stu-id="6e408-121">HealthService.exe</span></span>

<span data-ttu-id="6e408-122">Ha hello services management console (services.msc) megnyitásához is láthat hello Microsoft Monitoring Agent szolgáltatás fut alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="6e408-122">If you open hello services management console (services.msc), you will also see hello Microsoft Monitoring Agent service running as shown below:</span></span>

![Szolgáltatások](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig5.png)

<span data-ttu-id="6e408-124">toosee hello ügynök melyik verzióját telepítette, nyissa meg a **Feladatkezelő**, a hello **folyamatok** lapon keresse meg a hello **a Microsoft figyelési ügynök szolgáltatás**, kattintson a jobb gombbal a és Kattintson a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="6e408-124">toosee which version of hello agent you have, open **Task Manager**, in hello **Processes** tab locate hello **Microsoft Monitoring Agent Service**, right-click on it and click **Properties**.</span></span> <span data-ttu-id="6e408-125">A hello **részletek** fülre, tekintse meg a hello fájlverzió alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="6e408-125">In hello **Details** tab, look hello file version as shown below:</span></span>

![Fájl](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig6.png)
   

## <a name="microsoft-monitoring-agent-installation-scenarios"></a><span data-ttu-id="6e408-127">A Microsoft Monitoring Agent telepítési forgatókönyvei</span><span class="sxs-lookup"><span data-stu-id="6e408-127">Microsoft Monitoring Agent installation scenarios</span></span>
<span data-ttu-id="6e408-128">Nincsenek két telepítési forgatókönyvek hello Microsoft Monitoring Agent telepítése a számítógép eltérő eredményeket eredményezhetnek.</span><span class="sxs-lookup"><span data-stu-id="6e408-128">There are two installation scenarios that can produce different results when installing hello Microsoft Monitoring Agent on your computer.</span></span> <span data-ttu-id="6e408-129">hello támogatott forgatókönyvek a következők:</span><span class="sxs-lookup"><span data-stu-id="6e408-129">hello supported scenarios are:</span></span>

* <span data-ttu-id="6e408-130">**A Security Center által automatikusan telepített ügynök**: Ebben a forgatókönyvben képes tooview hello riasztásokat a helyek, a Security Center és a keresési napló lesz.</span><span class="sxs-lookup"><span data-stu-id="6e408-130">**Agent installed automatically by Security Center**: in this scenario you will be able tooview hello alerts in both locations, Security Center and Log search.</span></span> <span data-ttu-id="6e408-131">E-mail értesítések toohello e-mail címet a hello előfizetés hello erőforrás tartozik hello biztonsági házirendben beállított fog kapni.</span><span class="sxs-lookup"><span data-stu-id="6e408-131">You will receive e-mail notifications toohello email address that was configured in hello security policy for hello subscription hello resource belongs to.</span></span>
<span data-ttu-id="6e408-132">.</span><span class="sxs-lookup"><span data-stu-id="6e408-132">.</span></span>
* <span data-ttu-id="6e408-133">**Az Azure-ban található egy virtuális Gépet manuálisan telepített ügynök**: Ebben a forgatókönyvben, ha használ ügynökök letöltése és telepítése manuálisan előzetes tooFebruary 2017, csak akkor, ha szűrheti a hello képes tooview hello riasztásokat a Security Center portál hello lesz előfizetés hello munkaterület tartozik.</span><span class="sxs-lookup"><span data-stu-id="6e408-133">**Agent manually installed on a VM located in Azure**: in this scenario, if you are using agents downloaded and installed manually prior tooFebruary 2017, you will be able tooview hello alerts in hello Security Center portal only if you filter on hello subscription hello workspace belongs to.</span></span> <span data-ttu-id="6e408-134">Abban az esetben szűrő hello előfizetés hello erőforráshoz tartozik, akkor nem fogja tudni toosee e riasztások.</span><span class="sxs-lookup"><span data-stu-id="6e408-134">In case you filter on hello subscription hello resource belongs to, you won’t be able toosee any alerts.</span></span> <span data-ttu-id="6e408-135">E-mail értesítések toohello e-mail címet a hello előfizetés hello munkaterület tartozik hello biztonsági házirendben beállított fog kapni.</span><span class="sxs-lookup"><span data-stu-id="6e408-135">You will receive e-mail notifications toohello email address that was configured in hello security policy for hello subscription hello workspace belongs to.</span></span>

>[!NOTE]
> <span data-ttu-id="6e408-136">tooavoid hello viselkedését, tekintse meg a hello második, ellenőrizze, hogy hello hello ügynök legújabb verziójának letöltése.</span><span class="sxs-lookup"><span data-stu-id="6e408-136">tooavoid hello behavior explained in hello second, make sure you download hello latest version of hello agent.</span></span>
> 

## <a name="troubleshooting-monitoring-agent-network-requirements"></a><span data-ttu-id="6e408-137">A figyelőügynök hibaelhárítása – hálózati követelmények</span><span class="sxs-lookup"><span data-stu-id="6e408-137">Troubleshooting monitoring agent network requirements</span></span>
<span data-ttu-id="6e408-138">Az ügynökök tooconnect tooand regisztrálása a Security Center toonetwork erőforrások eléréséhez, beleértve hello portszámok és a tartomány URL-címek kell rendelkezniük.</span><span class="sxs-lookup"><span data-stu-id="6e408-138">For agents tooconnect tooand register with Security Center, they must have access toonetwork resources, including hello port numbers and domain URLs.</span></span>

- <span data-ttu-id="6e408-139">Proxykiszolgálók van szüksége, amely megfelelő proxykiszolgáló erőforrások vannak konfigurálva ügynökbeállítások hello tooensure.</span><span class="sxs-lookup"><span data-stu-id="6e408-139">For proxy servers, you need tooensure that hello appropriate proxy server resources are configured in agent settings.</span></span> <span data-ttu-id="6e408-140">Olvassa el ebben a cikkben találhat további információt a [hogyan toochange hello proxybeállítások](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-windows-agents#configure-proxy-settings).</span><span class="sxs-lookup"><span data-stu-id="6e408-140">Read this article for more information on [how toochange hello proxy settings](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-windows-agents#configure-proxy-settings).</span></span>
- <span data-ttu-id="6e408-141">A, amelyek korlátozzák a hozzáférést toohello Internet tűzfalak kell tooconfigure a tűzfal toopermit hozzáférés tooOMS.</span><span class="sxs-lookup"><span data-stu-id="6e408-141">For firewalls that restrict access toohello Internet, you need tooconfigure your firewall toopermit access tooOMS.</span></span> <span data-ttu-id="6e408-142">Az ügynök beállításait nem kell módosítania.</span><span class="sxs-lookup"><span data-stu-id="6e408-142">No action is needed in agent settings.</span></span>

<span data-ttu-id="6e408-143">a következő táblázat hello látható kommunikációhoz szükséges erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="6e408-143">hello following table shows resources needed for communication.</span></span>

| <span data-ttu-id="6e408-144">Ügynök erőforrása</span><span class="sxs-lookup"><span data-stu-id="6e408-144">Agent Resource</span></span> | <span data-ttu-id="6e408-145">Portok</span><span class="sxs-lookup"><span data-stu-id="6e408-145">Ports</span></span> | <span data-ttu-id="6e408-146">HTTPS-ellenőrzés kihagyása</span><span class="sxs-lookup"><span data-stu-id="6e408-146">Bypass HTTPS inspection</span></span> |
|---|---|---|
| <span data-ttu-id="6e408-147">*.ods.opinsights.azure.com</span><span class="sxs-lookup"><span data-stu-id="6e408-147">*.ods.opinsights.azure.com</span></span> | <span data-ttu-id="6e408-148">443</span><span class="sxs-lookup"><span data-stu-id="6e408-148">443</span></span> | <span data-ttu-id="6e408-149">Igen</span><span class="sxs-lookup"><span data-stu-id="6e408-149">Yes</span></span> |
| <span data-ttu-id="6e408-150">*.oms.opinsights.azure.com</span><span class="sxs-lookup"><span data-stu-id="6e408-150">*.oms.opinsights.azure.com</span></span> | <span data-ttu-id="6e408-151">443</span><span class="sxs-lookup"><span data-stu-id="6e408-151">443</span></span> | <span data-ttu-id="6e408-152">Igen</span><span class="sxs-lookup"><span data-stu-id="6e408-152">Yes</span></span> |
| <span data-ttu-id="6e408-153">*.blob.core.windows.net</span><span class="sxs-lookup"><span data-stu-id="6e408-153">*.blob.core.windows.net</span></span> | <span data-ttu-id="6e408-154">443</span><span class="sxs-lookup"><span data-stu-id="6e408-154">443</span></span> | <span data-ttu-id="6e408-155">Igen</span><span class="sxs-lookup"><span data-stu-id="6e408-155">Yes</span></span> |
| <span data-ttu-id="6e408-156">*.azure-automation.net</span><span class="sxs-lookup"><span data-stu-id="6e408-156">*.azure-automation.net</span></span> | <span data-ttu-id="6e408-157">443</span><span class="sxs-lookup"><span data-stu-id="6e408-157">443</span></span> | <span data-ttu-id="6e408-158">Igen</span><span class="sxs-lookup"><span data-stu-id="6e408-158">Yes</span></span> |

<span data-ttu-id="6e408-159">Ha hibát tapasztal bevezetési hello ügynökkel, győződjön meg arról, hogy tooread hello cikk [hogyan tootroubleshoot Operations Management Suite előkészítési problémák](https://support.microsoft.com/en-us/help/3126513/how-to-troubleshoot-operations-management-suite-onboarding-issues).</span><span class="sxs-lookup"><span data-stu-id="6e408-159">If you encounter onboarding issues with hello agent, make sure tooread hello article [How tootroubleshoot Operations Management Suite onboarding issues](https://support.microsoft.com/en-us/help/3126513/how-to-troubleshoot-operations-management-suite-onboarding-issues).</span></span>


## <a name="troubleshooting-endpoint-protection-not-working-properly"></a><span data-ttu-id="6e408-160">Az Endpoint Protection hibaelhárítása nem működik megfelelően</span><span class="sxs-lookup"><span data-stu-id="6e408-160">Troubleshooting endpoint protection not working properly</span></span>

<span data-ttu-id="6e408-161">a vendégügynök hello hello szülő folyamat minden, a hello [Microsoft Antimalware](../security/azure-security-antimalware.md) bővítmény does.</span><span class="sxs-lookup"><span data-stu-id="6e408-161">hello guest agent is hello parent process of everything hello [Microsoft Antimalware](../security/azure-security-antimalware.md) extension does.</span></span> <span data-ttu-id="6e408-162">Hello Vendég ügynök folyamat sikertelen lesz, amikor a futtatását, egyik gyermekfolyamata hello Vendég ügynöke a Microsoft Antimalware hello is sikertelen lehet.</span><span class="sxs-lookup"><span data-stu-id="6e408-162">When hello guest agent process fails, hello Microsoft Antimalware that runs as a child process of hello guest agent may also fail.</span></span>  <span data-ttu-id="6e408-163">A helyzetekben, például, hogy van ajánlott tooverify hello a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="6e408-163">In scenarios like that is recommended tooverify hello following options:</span></span>

- <span data-ttu-id="6e408-164">Ha hello cél virtuális gép egy egyéni lemezképet, és hello VM hello létrehozója nem telepítve vendégügynök.</span><span class="sxs-lookup"><span data-stu-id="6e408-164">If hello target VM is a custom image and hello creator of hello VM never installed guest agent.</span></span>
- <span data-ttu-id="6e408-165">Ha hello célként megadott helyett egy Windows virtuális Gépet, majd a Windows-verzió hello hello kártevőirtó-bővítmény telepítése Linux virtuális gép Linux virtuális gép sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="6e408-165">If hello target is a Linux VM instead of a Windows VM then installing hello Windows version of hello antimalware extension on a Linux VM will fail.</span></span> <span data-ttu-id="6e408-166">Linux-vendégügynök hello virtualizálásra konkrét követelmények vonatkoznak, az operációs rendszer verziója és a szükséges csomagokat, és ezek nem teljesülnek hello Virtuálisgép-ügynök nem fog működni hiba vagy.</span><span class="sxs-lookup"><span data-stu-id="6e408-166">hello Linux guest agent has specific requirements in terms of OS version and required packages, and if those requirements are not met hello VM agent will not work there either.</span></span> 
- <span data-ttu-id="6e408-167">Ha hello virtuális gép vendégügynökének egy régi verziója lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="6e408-167">If hello VM was created with an old version of guest agent.</span></span> <span data-ttu-id="6e408-168">Ha igen, vegye figyelembe, hogy az egyes régi ügynökök sikerült nem automatikus frissítés maga toohello újabb verzióra, és a toothis probléma vezethet kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6e408-168">If it was, you should be aware that some old agents could not auto-update itself toohello newer version and this could lead toothis problem.</span></span> <span data-ttu-id="6e408-169">Mindig használja a vendégügynök hello legújabb verzióját, ha a saját lemezképek létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="6e408-169">Always use hello latest version of guest agent if creating your own images.</span></span>
- <span data-ttu-id="6e408-170">Egyes külső felügyeleti szoftverek a hello vendégügynök letiltása, vagy hozzáférési toocertain fájlhelyek letiltása.</span><span class="sxs-lookup"><span data-stu-id="6e408-170">Some third-party administration software may disable hello guest agent, or block access toocertain file locations.</span></span> <span data-ttu-id="6e408-171">Ha külső a virtuális gép telepítve van, gondoskodjon arról, hogy az hello ügynök hello kizárási listához.</span><span class="sxs-lookup"><span data-stu-id="6e408-171">If you have third-party installed on your VM, make sure that hello agent is on hello exclusion list.</span></span>
- <span data-ttu-id="6e408-172">Bizonyos tűzfal vagy a hálózati biztonsági csoport (NSG) blokkolhatják a hálózati forgalom tooand Vendég ügynöktől.</span><span class="sxs-lookup"><span data-stu-id="6e408-172">Certain firewall settings or Network Security Group (NSG) may block network traffic tooand from guest agent.</span></span>
- <span data-ttu-id="6e408-173">Bizonyos hozzáférés-vezérlési listák (ACL) megakadályozhatják a lemezhez való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="6e408-173">Certain Access Control List (ACL) may prevent disk access.</span></span>
- <span data-ttu-id="6e408-174">Kevés a szabad lemezterület akkor képes blokkolni a hello vendégügynök helyes működését.</span><span class="sxs-lookup"><span data-stu-id="6e408-174">Lack of disk space can block hello guest agent from functioning properly.</span></span> 

<span data-ttu-id="6e408-175">Alapértelmezett hello Microsoft Antimalware felhasználói felület le van tiltva, az olvasási [engedélyezése a Microsoft Antimalware felhasználói felület Azure Resource Manager virtuális gépek feladás egy vagy több központi telepítési](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/09/enabling-microsoft-antimalware-user-interface-post-deployment/) további információt a tooenable, ha van szüksége.</span><span class="sxs-lookup"><span data-stu-id="6e408-175">By default hello Microsoft Antimalware User Interface is disabled, read [Enabling Microsoft Antimalware User Interface on Azure Resource Manager VMs Post Deployment](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/09/enabling-microsoft-antimalware-user-interface-post-deployment/) for more information on how tooenable it if you need.</span></span>

## <a name="troubleshooting-problems-loading-hello-dashboard"></a><span data-ttu-id="6e408-176">Hello irányítópult betöltése hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="6e408-176">Troubleshooting problems loading hello dashboard</span></span>

<span data-ttu-id="6e408-177">Ha problémák hello Security Center irányítópultjának betöltése, győződjön meg arról, hogy regisztrálja hello előfizetés tooSecurity Center (azaz hello első felhasználó, aki a Security Center megnyitott hello előfizetés) hello felhasználó és a tooturn szeretné hello felhasználó adatgyűjtés kell *tulajdonos* vagy *közreműködő* hello az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="6e408-177">If you experience issues loading hello Security Center dashboard, ensure that hello user that registers hello subscription tooSecurity Center (i.e. hello first user one who opened Security Center with hello subscription) and hello user who would like tooturn on data collection should be *Owner* or *Contributor* on hello subscription.</span></span> <span data-ttu-id="6e408-178">Adott pillanattól is rendelkező felhasználók *olvasó* hello az előfizetés hello-irányítópult és riasztások/ajánlás/házirend látható.</span><span class="sxs-lookup"><span data-stu-id="6e408-178">From that moment on also users with *Reader* on hello subscription can see hello dashboard/alerts/recommendation/policy.</span></span>

## <a name="contacting-microsoft-support"></a><span data-ttu-id="6e408-179">Kapcsolatfelvétel a Microsoft támogatási szolgálatával</span><span class="sxs-lookup"><span data-stu-id="6e408-179">Contacting Microsoft Support</span></span>
<span data-ttu-id="6e408-180">Bizonyos problémák hello útmutatásait, ebben a cikkben használatával azonosíthatók, mások is megkeresheti című cikk dokumentálja hello Security Center nyilvános [fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter).</span><span class="sxs-lookup"><span data-stu-id="6e408-180">Some issues can be identified using hello guidelines provided in this article, others you can also find documented at hello Security Center public [Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter).</span></span> <span data-ttu-id="6e408-181">Ha további hibaelhárításra van szüksége, az alábbi képen látható módon nyithat meg új támogatási kérelmet az **Azure Portalon**:</span><span class="sxs-lookup"><span data-stu-id="6e408-181">However if you need further troubleshooting, you can open a new support request using **Azure portal** as shown below:</span></span> 

![Microsoft támogatási szolgálat](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)


## <a name="see-also"></a><span data-ttu-id="6e408-183">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="6e408-183">See also</span></span>
<span data-ttu-id="6e408-184">Ebben a dokumentumban, megtudta, hogyan tooconfigure biztonsági házirendek az Azure Security Centerben.</span><span class="sxs-lookup"><span data-stu-id="6e408-184">In this document, you learned how tooconfigure security policies in Azure Security Center.</span></span> <span data-ttu-id="6e408-185">További információ az Azure Security Center toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="6e408-185">toolearn more about Azure Security Center, see hello following:</span></span>

* <span data-ttu-id="6e408-186">[Azure Security Center tervezéséhez és az üzemeltetési útmutatóban](security-center-planning-and-operations-guide.md) – további hogyan tooplan és hello kialakítási szempontok tooadopt az Azure Security Center ismertetése.</span><span class="sxs-lookup"><span data-stu-id="6e408-186">[Azure Security Center Planning and Operations Guide](security-center-planning-and-operations-guide.md) — Learn how tooplan and understand hello design considerations tooadopt Azure Security Center.</span></span>
* <span data-ttu-id="6e408-187">[Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello állapotát az Azure-erőforrások</span><span class="sxs-lookup"><span data-stu-id="6e408-187">[Security health monitoring in Azure Security Center](security-center-monitoring.md) — Learn how toomonitor hello health of your Azure resources</span></span>
* <span data-ttu-id="6e408-188">[Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – további hogyan toomanage és válaszoljon toosecurity riasztások</span><span class="sxs-lookup"><span data-stu-id="6e408-188">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) — Learn how toomanage and respond toosecurity alerts</span></span>
* <span data-ttu-id="6e408-189">[Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="6e408-189">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) — Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="6e408-190">[Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban</span><span class="sxs-lookup"><span data-stu-id="6e408-190">[Azure Security Center FAQ](security-center-faq.md) — Find frequently asked questions about using hello service</span></span>
* <span data-ttu-id="6e408-191">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) – Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.</span><span class="sxs-lookup"><span data-stu-id="6e408-191">[Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) — Find blog posts about Azure security and compliance</span></span>
