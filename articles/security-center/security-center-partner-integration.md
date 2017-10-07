---
title: "aaaPartner integráció az Azure Security Centerben |} Microsoft Docs"
description: "Hogyan integrálható az Azure Security Center partnerek tooenhance megismerése az Azure-erőforrások teljes biztonságában."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: yurid
ms.openlocfilehash: 3621335730a076721cb3c23788a47be50aa8fc73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="partner-integration-in-azure-security-center"></a><span data-ttu-id="49df7-103">Partnerintegráció az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="49df7-103">Partner integration in Azure Security Center</span></span>

<span data-ttu-id="49df7-104">Ez a cikk azt ismerteti, hogyan integrálható az Azure Security Center partnerek toohelp növelheti, hogy a teljes biztonságában.</span><span class="sxs-lookup"><span data-stu-id="49df7-104">In this article, we describe how Azure Security Center integrates with partners toohelp you enhance overall security.</span></span> <span data-ttu-id="49df7-105">A Security Center az Azure-ban integrált megoldást nyújt, és kihasználja a hello Azure piactér partner hitelesítő és számlázási.</span><span class="sxs-lookup"><span data-stu-id="49df7-105">Security Center offers an integrated experience in Azure, and takes advantage of hello Azure Marketplace for partner certification and billing.</span></span>

> [!NOTE] 
> <span data-ttu-id="49df7-106">Frissítésétől. június 2017 Security Center hello Microsoft Monitoring Agent toocollect és a tároló adatait használja.</span><span class="sxs-lookup"><span data-stu-id="49df7-106">As of June 2017, Security Center uses hello Microsoft Monitoring Agent toocollect and store data.</span></span> <span data-ttu-id="49df7-107">További információk: [Az Azure Security Center platform migrálása](security-center-platform-migration.md).</span><span class="sxs-lookup"><span data-stu-id="49df7-107">For more information, see [Azure Security Center platform migration](security-center-platform-migration.md).</span></span> <span data-ttu-id="49df7-108">a cikkben szereplő információkat hello Security Center funkció átmenet toohello Microsoft Monitoring Agent után jelöli.</span><span class="sxs-lookup"><span data-stu-id="49df7-108">hello information in this article represents Security Center functionality after transition toohello Microsoft Monitoring Agent.</span></span>
>

## <a name="why-deploy-partner-solutions-from-security-center"></a><span data-ttu-id="49df7-109">Miért érdemes a Security Centerből üzembe helyezni a partnermegoldásokat?</span><span class="sxs-lookup"><span data-stu-id="49df7-109">Why deploy partner solutions from Security Center</span></span>

<span data-ttu-id="49df7-110">A Security Center négy fő oka annak tooleverage partner integrálást a következők:</span><span class="sxs-lookup"><span data-stu-id="49df7-110">Four main reasons tooleverage partner integration in Security Center are:</span></span>

- <span data-ttu-id="49df7-111">**Egyszerű üzembe helyezés**.</span><span class="sxs-lookup"><span data-stu-id="49df7-111">**Ease of deployment**.</span></span> <span data-ttu-id="49df7-112">A partner megoldás telepítése a következő hello Security Center ajánlás szerint sokkal könnyebben.</span><span class="sxs-lookup"><span data-stu-id="49df7-112">Deploying a partner solution by following hello Security Center recommendation is much easier.</span></span> <span data-ttu-id="49df7-113">hello telepítési folyamat egy alapértelmezett telepítés és a hálózati topológia segítségével teljes mértékben automatizálható.</span><span class="sxs-lookup"><span data-stu-id="49df7-113">hello deployment process can be fully automated by using a default setup and network topology.</span></span> <span data-ttu-id="49df7-114">Az ügyfelek választhatják a félig automatikus lehetőséget is, amely rugalmasabb és nagyobb mértékben testre szabható.</span><span class="sxs-lookup"><span data-stu-id="49df7-114">Alternatively, customers can choose a semi-automated option for more flexibility and customization.</span></span>
- <span data-ttu-id="49df7-115">**Integrált észlelések**.</span><span class="sxs-lookup"><span data-stu-id="49df7-115">**Integrated detections**.</span></span> <span data-ttu-id="49df7-116">A partnermegoldásoktól érkező biztonsági eseményeket a rendszer automatikusan összegyűjti, összesíti és megjeleníti a Security Center riasztásainak és incidenseinek részeként.</span><span class="sxs-lookup"><span data-stu-id="49df7-116">Security events from partner solutions are automatically collected, aggregated, and displayed as part of Security Center alerts and incidents.</span></span> <span data-ttu-id="49df7-117">Ezeket az eseményeket is vannak illesztett észlelések a más forrásokból tooprovide advanced threat-észlelési képességek.</span><span class="sxs-lookup"><span data-stu-id="49df7-117">These events also are fused with detections from other sources tooprovide advanced threat-detection capabilities.</span></span>
- <span data-ttu-id="49df7-118">**Egyesített állapotmonitorozás és -kezelés**.</span><span class="sxs-lookup"><span data-stu-id="49df7-118">**Unified health monitoring and management**.</span></span> <span data-ttu-id="49df7-119">Az ügyfelek használhatják integrált állapotfigyelő események toomonitor összes partneri megoldások egy pillanat alatt.</span><span class="sxs-lookup"><span data-stu-id="49df7-119">Customers can use integrated health events toomonitor all partner solutions at a glance.</span></span> <span data-ttu-id="49df7-120">Alapvető felügyeleti esetén érhető el, egyszerű hozzáférés tooadvanced beállítása hello partneri megoldás segítségével.</span><span class="sxs-lookup"><span data-stu-id="49df7-120">Basic management is available, with easy access tooadvanced setup by using hello partner solution.</span></span>
- <span data-ttu-id="49df7-121">**Exportálja a tooSIEM**.</span><span class="sxs-lookup"><span data-stu-id="49df7-121">**Export tooSIEM**.</span></span> <span data-ttu-id="49df7-122">Az ügyfelek összes Security Center exportálhatók és partner riasztások közös esemény formátum (CEF) tooon helyszíni biztonsági adatai és az esemény felügyeleti SIEM-rendszerekben az Azure naplóelemzés integrálása révén (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="49df7-122">Customers can export all Security Center and partner alerts in Common Event Format (CEF) tooon-premises Security Information and Event Management (SIEM) systems by using Azure log integration (preview).</span></span>


## <a name="partners-that-integrate-with-security-center"></a><span data-ttu-id="49df7-123">Security Center-integrációt kínáló partnerek</span><span class="sxs-lookup"><span data-stu-id="49df7-123">Partners that integrate with Security Center</span></span>

<span data-ttu-id="49df7-124">A Security Center jelenleg ezekkel a megoldásokkal van integrálva:</span><span class="sxs-lookup"><span data-stu-id="49df7-124">Currently, Security Center integrates with these solutions:</span></span>

- <span data-ttu-id="49df7-125">Végpontvédelem ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), Symantec és [Microsoft Antimalware az Azure Cloud Services és Virtual Machines szolgáltatásokhoz](https://docs.microsoft.com/azure/security/azure-security-antimalware))</span><span class="sxs-lookup"><span data-stu-id="49df7-125">Endpoint protection ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), Symantec, and [Microsoft Antimalware for Azure Cloud Services and Virtual Machines](https://docs.microsoft.com/azure/security/azure-security-antimalware))</span></span> 
- <span data-ttu-id="49df7-126">Webalkalmazás-tűzfal ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/resources.html?limit=10&search=&document-type=data-sheets) és [Azure Application Gateway](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/))</span><span class="sxs-lookup"><span data-stu-id="49df7-126">Web application firewall ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/resources.html?limit=10&search=&document-type=data-sheets), and [Azure Application Gateway](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/))</span></span> 
- <span data-ttu-id="49df7-127">Új generációs tűzfalmegoldások ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2) és [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html))</span><span class="sxs-lookup"><span data-stu-id="49df7-127">Next-generation firewall ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2), and [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html))</span></span> 
- <span data-ttu-id="49df7-128">Biztonságirés-felmérés ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/))</span><span class="sxs-lookup"><span data-stu-id="49df7-128">Vulnerability assessment ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/))</span></span>  

<span data-ttu-id="49df7-129">Adott idő alatt, a Security Center fog bontsa ki ezen kategóriák belül partnerek hello számát, és új kategória hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="49df7-129">Over time, Security Center will expand hello number of partners within these categories, and add new categories.</span></span> 

## <a name="deploy-a-partner-solution"></a><span data-ttu-id="49df7-130">Partnermegoldás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="49df7-130">Deploy a partner solution</span></span>

<span data-ttu-id="49df7-131">Hello beállítása az Azure-alapú környezetben és a meghatározott hello biztonsági házirend alapján, a Security Center használatát javasolja, hogy telepít egy partneri megoldást.</span><span class="sxs-lookup"><span data-stu-id="49df7-131">Based on hello setup of your Azure environment and hello security policy you defined, Security Center might recommend that you deploy a partner solution.</span></span> <span data-ttu-id="49df7-132">a Security Center javaslat hello végigvezeti hello kiválasztása és telepítése egy partneri megoldást.</span><span class="sxs-lookup"><span data-stu-id="49df7-132">hello Security Center recommendation guides you through hello process of selecting and installing a partner solution.</span></span> <span data-ttu-id="49df7-133">hello általános üzembe helyezését változhat, hello típus megoldás és a partner használja.</span><span class="sxs-lookup"><span data-stu-id="49df7-133">hello overall deployment experience might vary, depending on hello type of solution and partner you use.</span></span> <span data-ttu-id="49df7-134">További információkért tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="49df7-134">For more information, see hello following articles:</span></span>

- [<span data-ttu-id="49df7-135">Végpontvédelem telepítése</span><span class="sxs-lookup"><span data-stu-id="49df7-135">Install endpoint protection</span></span>](security-center-install-endpoint-protection.md)
- [<span data-ttu-id="49df7-136">Webalkalmazási tűzfal hozzáadása</span><span class="sxs-lookup"><span data-stu-id="49df7-136">Add a web application firewall</span></span>](security-center-add-web-application-firewall.md)
- [<span data-ttu-id="49df7-137">Új generációs tűzfal hozzáadása</span><span class="sxs-lookup"><span data-stu-id="49df7-137">Add a next-generation firewall</span></span>](security-center-add-next-generation-firewall.md)
- [<span data-ttu-id="49df7-138">A sebezhetőségi felmérés nincs telepítve</span><span class="sxs-lookup"><span data-stu-id="49df7-138">Vulnerability assessment not installed</span></span>](security-center-vulnerability-assessment-recommendations.md)

## <a name="manage-partner-solutions"></a><span data-ttu-id="49df7-139">A partnermegoldások kezelése</span><span class="sxs-lookup"><span data-stu-id="49df7-139">Manage partner solutions</span></span>

<span data-ttu-id="49df7-140">A központi telepítést követően tooview információk készül hello hello megoldás állapotát, és végre alapvető felügyeleti feladatokat, hello **Security Center** panelen, jelölje be hello **partneri megoldások** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="49df7-140">After deployment, tooview information about hello health of hello solution and perform basic management tasks, on hello **Security Center** blade, select hello **Partner solutions** option.</span></span> <span data-ttu-id="49df7-141">A partnermegoldások Security Centerben való kezelésével kapcsolatos további információkért tekintse meg a [Partnermegoldások monitorozása az Azure Security Centerrel](security-center-partner-solutions.md) című cikket.</span><span class="sxs-lookup"><span data-stu-id="49df7-141">For more information about managing partner solutions in Security Center, see [Monitor partner solutions with Azure Security Center](security-center-partner-solutions.md).</span></span>

![Partnerintegráció](./media/security-center-partner-integration/security-center-partner-integration-fig1-new2.png)

> [!NOTE]
> <span data-ttu-id="49df7-143">Symantec endpoint protection támogatási korlátozott toodiscovery.</span><span class="sxs-lookup"><span data-stu-id="49df7-143">Symantec endpoint protection support is limited toodiscovery.</span></span> <span data-ttu-id="49df7-144">Az állapotriasztások nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="49df7-144">No health alerts are available.</span></span>
>

## <a name="see-also"></a><span data-ttu-id="49df7-145">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="49df7-145">See also</span></span>

<span data-ttu-id="49df7-146">Ebben a cikkben megtanulta, hogyan toointegrate partnermegoldások az Azure Security Centerben.</span><span class="sxs-lookup"><span data-stu-id="49df7-146">In this article, you learned how toointegrate partner solutions in Azure Security Center.</span></span> <span data-ttu-id="49df7-147">További információ a Security Center toolearn tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="49df7-147">toolearn more about Security Center, see hello following articles:</span></span>

* [<span data-ttu-id="49df7-148">Útmutató a Security Center tervezéséhez és működtetéséhez</span><span class="sxs-lookup"><span data-stu-id="49df7-148">Security Center planning and operations guide</span></span>](security-center-planning-and-operations-guide.md)
* [<span data-ttu-id="49df7-149">Kezelésének és megoldásának toosecurity riasztásokat a Security Center</span><span class="sxs-lookup"><span data-stu-id="49df7-149">Manage and respond toosecurity alerts in Security Center</span></span>](security-center-managing-and-responding-alerts.md)
* [<span data-ttu-id="49df7-150">Biztonsági riasztások típus szerint a Security Centerben</span><span class="sxs-lookup"><span data-stu-id="49df7-150">Security alerts by type in Security Center</span></span>](security-center-alerts-type.md)
* <span data-ttu-id="49df7-151">[Biztonsági állapot monitorozása a Security Centerben](security-center-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="49df7-151">[Security health monitoring in Security Center](security-center-monitoring.md).</span></span> <span data-ttu-id="49df7-152">Ismerje meg, hogyan toomonitor hello az Azure-erőforrások állapotát.</span><span class="sxs-lookup"><span data-stu-id="49df7-152">Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="49df7-153">[Partnermegoldások monitorozása a Security Centerrel](security-center-partner-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="49df7-153">[Monitoring partner solutions with Security Center](security-center-partner-solutions.md).</span></span> <span data-ttu-id="49df7-154">Ismerje meg, hogyan toomonitor hello partneri megoldások biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="49df7-154">Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="49df7-155">[Azure Security Center – gyakori kérdések](security-center-faq.md)</span><span class="sxs-lookup"><span data-stu-id="49df7-155">[Azure Security Center FAQs](security-center-faq.md).</span></span> <span data-ttu-id="49df7-156">Válaszok toofrequently hello szolgáltatás használatára vonatkozó kérdések beolvasása.</span><span class="sxs-lookup"><span data-stu-id="49df7-156">Get answers toofrequently asked questions about using hello service.</span></span>
* <span data-ttu-id="49df7-157">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/).</span><span class="sxs-lookup"><span data-stu-id="49df7-157">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/).</span></span> <span data-ttu-id="49df7-158">Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.</span><span class="sxs-lookup"><span data-stu-id="49df7-158">Find blog posts about Azure security and compliance.</span></span>