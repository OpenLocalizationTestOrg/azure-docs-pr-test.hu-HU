---
title: "Adatbiztonság az Operations Management Suite biztonsági és auditálási megoldásban | Microsoft Docs"
description: "Ez a dokumentum azt ismerteti, hogyan zajlik az adatok kezelése az Operations Management Suite biztonsági és auditálási megoldásban."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 9cdf7deb-2a30-4672-b89f-71179ee8326a
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: 3b6327b1f5150f32afd71639f32c55d823f1d1f0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="operations-management-suite-security-and-audit-solution-data-security"></a><span data-ttu-id="d46ac-103">Adatbiztonság az Operations Management Suite biztonsági és auditálási megoldásban</span><span class="sxs-lookup"><span data-stu-id="d46ac-103">Operations Management Suite Security and Audit solution data security</span></span>
<span data-ttu-id="d46ac-104">Annak érdekében, hogy segítse az ügyfeleket a fenyegetések kiküszöbölésében, felderítésében, illetve a rájuk való válaszadásban, az [Operations Management Suite (OMS) biztonsági és auditálási megoldás](operations-management-suite-overview.md) adatokat gyűjt, illetve dolgoz fel az Ön erőforrásairól, többek között a következőkről:</span><span class="sxs-lookup"><span data-stu-id="d46ac-104">To help customers prevent, detect, and respond to threats, [Operations Management Suite  (OMS) Security and Audit Solution](operations-management-suite-overview.md) collects and processes data about your resources, which includes:</span></span>

* <span data-ttu-id="d46ac-105">Biztonsági eseménynapló</span><span class="sxs-lookup"><span data-stu-id="d46ac-105">Security event log</span></span>
* <span data-ttu-id="d46ac-106">A Windows esemény-nyomkövetés (ETW) eseményei</span><span class="sxs-lookup"><span data-stu-id="d46ac-106">Event Tracing for Windows (ETW) events</span></span>
* <span data-ttu-id="d46ac-107">AppLocker naplózási események</span><span class="sxs-lookup"><span data-stu-id="d46ac-107">AppLocker auditing events</span></span>
* <span data-ttu-id="d46ac-108">A Windows tűzfal naplója</span><span class="sxs-lookup"><span data-stu-id="d46ac-108">Windows Firewall log</span></span>
* <span data-ttu-id="d46ac-109">Advanced Threat Analytics-események</span><span class="sxs-lookup"><span data-stu-id="d46ac-109">Advanced Threat Analytics events</span></span>
* <span data-ttu-id="d46ac-110">A biztonsági alapkonfiguráció értékelése</span><span class="sxs-lookup"><span data-stu-id="d46ac-110">Results of baseline assessment</span></span>
* <span data-ttu-id="d46ac-111">Kártevőirtó-felmérés eredménye</span><span class="sxs-lookup"><span data-stu-id="d46ac-111">Results of antimalware assessment</span></span>
* <span data-ttu-id="d46ac-112">Frissítések/javítások felmérésének eredménye</span><span class="sxs-lookup"><span data-stu-id="d46ac-112">Results of update/patch assessment</span></span>
* <span data-ttu-id="d46ac-113">Az ügynökön kifejezetten engedélyezett Syslogs-streamek</span><span class="sxs-lookup"><span data-stu-id="d46ac-113">Syslogs streams that are explicitly enabled on the agent</span></span>

<span data-ttu-id="d46ac-114">Fontos kötelességünknek tekintjük ezen adatok védelmét és biztonságát.</span><span class="sxs-lookup"><span data-stu-id="d46ac-114">We make strong commitments to protect the privacy and security of this data.</span></span> <span data-ttu-id="d46ac-115">A Microsoft szigorú megfelelőségi és biztonsági szabályokat követ, a kódolástól kezdve egészen a szolgáltatások üzemeltetéséig.</span><span class="sxs-lookup"><span data-stu-id="d46ac-115">Microsoft adheres to strict compliance and security guidelines—from coding to operating a service.</span></span>
<span data-ttu-id="d46ac-116">Ez a cikk bemutatja, hogyan kezeli az OMS biztonsági és auditálási megoldás az adatokat, illetve hogyan gondoskodik a védelmükről.</span><span class="sxs-lookup"><span data-stu-id="d46ac-116">This article explains how data is managed and safeguarded in OMS Security and Audit Solution.</span></span>

## <a name="data-sources"></a><span data-ttu-id="d46ac-117">Adatforrások</span><span class="sxs-lookup"><span data-stu-id="d46ac-117">Data sources</span></span>
<span data-ttu-id="d46ac-118">Az OMS biztonsági és auditálási megoldás elemzi azokról a virtuális gépekről és fizikai számítógépekről származó adatokat, amelyekre telepítve van az OMS-ügynök.</span><span class="sxs-lookup"><span data-stu-id="d46ac-118">OMS Security and Audit Solution analyze data from your Virtual Machines and physical computers where the OMS Agent is installed.</span></span> <span data-ttu-id="d46ac-119">Az OMS biztonsági és auditálási megoldás konfigurációs információkat gyűjthet a biztonsági eseményekről, például a Windows-eseményekről, az auditnaplókról, az IIS-naplókról és a syslog-üzenetekről.</span><span class="sxs-lookup"><span data-stu-id="d46ac-119">OMS Security and Audit Solution can collect configuration information about security events, such as Windows event, audit logs, IIS logs and syslog messages.</span></span> <span data-ttu-id="d46ac-120">A gyűjtött adatok például a következők: az operációs rendszer típusa és verziója, a futó folyamatok, a gép neve, az IP-címek, a bejelentkezett felhasználó és a bérlő azonosítója.</span><span class="sxs-lookup"><span data-stu-id="d46ac-120">Examples of such data are: operating system type and version, running processes, machine name, IP addresses, logged in user, and tenant ID.</span></span>  

## <a name="data-protection"></a><span data-ttu-id="d46ac-121">Adatvédelem</span><span class="sxs-lookup"><span data-stu-id="d46ac-121">Data protection</span></span>
<span data-ttu-id="d46ac-122">**Az adatok elkülönítése**: az adatok logikailag elkülönítve vannak tárolva a szolgáltatás egyes összetevőiben.</span><span class="sxs-lookup"><span data-stu-id="d46ac-122">**Data segregation**: Data is kept logically separate on each component throughout the service.</span></span> <span data-ttu-id="d46ac-123">Az összes adat szervezet szerint van megcímkézve.</span><span class="sxs-lookup"><span data-stu-id="d46ac-123">All data is tagged per organization.</span></span> <span data-ttu-id="d46ac-124">Ez a címkézés megmarad az adatok teljes életciklusa alatt, és a szolgáltatás minden rétegében érvényes.</span><span class="sxs-lookup"><span data-stu-id="d46ac-124">This tagging persists throughout the data lifecycle, and it is enforced at each layer of the service.</span></span> 

<span data-ttu-id="d46ac-125">**Hozzáférés az adatokhoz**: a biztonsági javaslatok és a lehetséges biztonsági fenyegetések kivizsgálása érdekében a Microsoft munkatársai hozzáférhetnek az Azure-szolgáltatások által összegyűjtött vagy elemzett információkhoz.</span><span class="sxs-lookup"><span data-stu-id="d46ac-125">**Data access**: To provide security recommendations and investigate potential security threats, Microsoft personnel may access information collected or analyzed by services.</span></span> <span data-ttu-id="d46ac-126">Betartjuk a [Microsoft Online Services](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) és az [adatvédelmi nyilatkozat](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx) feltételeit, amelyek szerint a Microsoft nem használja fel az ügyféladatokat, és nem veszi igénybe azokat hirdetési vagy hasonló kereskedelmi célokra.</span><span class="sxs-lookup"><span data-stu-id="d46ac-126">We adhere to the [Microsoft Online Services Terms](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) and [Privacy Statement](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), which state that Microsoft will not use Customer Data or derive information from it for any advertising or similar commercial purposes.</span></span> <span data-ttu-id="d46ac-127">A biztonsági javaslatok és a lehetséges biztonsági fenyegetések kivizsgálása érdekében a Microsoft munkatársai hozzáférhetnek az Azure-szolgáltatások által összegyűjtött vagy elemzett információkhoz.</span><span class="sxs-lookup"><span data-stu-id="d46ac-127">To provide security recommendations and investigate potential security threats, Microsoft personnel may access information collected or analyzed by services.</span></span> <span data-ttu-id="d46ac-128">Az ügyféladatokat szükség esetén csak arra használjuk, hogy biztosítsuk Önnek az Azure-szolgáltatásokat, beleértve a szolgáltatások nyújtásának megfelelő célokat is.</span><span class="sxs-lookup"><span data-stu-id="d46ac-128">We only use Customer Data as needed to provide you with Azure services, including purposes compatible with providing those services.</span></span> <span data-ttu-id="d46ac-129">A saját adataihoz fűződő összes jog az Ön tulajdonában marad.</span><span class="sxs-lookup"><span data-stu-id="d46ac-129">You retain all rights to your own data.</span></span>

<span data-ttu-id="d46ac-130">**Az adatok felhasználása**: a Microsoft a különböző bérlőknél észlelt mintákat és fenyegetésre vonatkozó intelligenciát használ a megelőzési és észlelési funkcióihoz, és ezt az [adatvédelmi nyilatkozatában](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx) ismertetett adatvédelmi kötelezettségeinek megfelelően teszi.</span><span class="sxs-lookup"><span data-stu-id="d46ac-130">**Data use**: Microsoft uses patterns and threat intelligence seen across multiple tenants to enhance our prevention and detection capabilities; we do so in accordance with the privacy commitments described in our [Privacy Statement](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="d46ac-131">Az adatok helye az OMS-munkaterület szintjén állítható be a munkaterület létrehozása során, az OMS biztonsági és auditálási megoldás kezdeti konfigurációs folyamatának részeként.</span><span class="sxs-lookup"><span data-stu-id="d46ac-131">Data location is configured at the OMS workspace level, during the workspace creation, which is part of the initial OMS Security and Audit configuration process.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="d46ac-132">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="d46ac-132">See also</span></span>
<span data-ttu-id="d46ac-133">Ebből a dokumentumból megtudta, hogyan kezeli az OMS az adatokat, és hogyan gondoskodik a védelmükről.</span><span class="sxs-lookup"><span data-stu-id="d46ac-133">In this document, you learned how data is managed and safeguarded in OMS.</span></span> <span data-ttu-id="d46ac-134">Ha többet szeretne megtudni az OMS biztonsági és a naplózási megoldásról, tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="d46ac-134">To learn more about OMS Security and Audit solution, see:</span></span>

* [<span data-ttu-id="d46ac-135">Az Operations Management Suite (OMS) áttekintése</span><span class="sxs-lookup"><span data-stu-id="d46ac-135">Operations Management Suite (OMS) overview</span></span>](operations-management-suite-overview.md)
* [<span data-ttu-id="d46ac-136">A biztonsági riasztások figyelése és megválaszolása az Operations Management Suite biztonsági és auditálási megoldásban</span><span class="sxs-lookup"><span data-stu-id="d46ac-136">Monitoring and Responding to Security Alerts in Operations Management Suite Security and Audit Solution</span></span>](oms-security-responding-alerts.md)
* [<span data-ttu-id="d46ac-137">Az erőforrások figyelése az Operations Management Suite biztonsági és auditálási megoldásban</span><span class="sxs-lookup"><span data-stu-id="d46ac-137">Monitoring Resources in Operations Management Suite Security and Audit Solution</span></span>](oms-security-monitoring-resources.md)

