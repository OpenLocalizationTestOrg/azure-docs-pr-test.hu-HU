---
title: "Felügyeleti csomag biztonsági és naplózási megoldás adatbiztonság aaaOperations |} Microsoft Docs"
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
ms.openlocfilehash: 9c4181b3b491e4f7f0c57d7252eca78a819722d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="operations-management-suite-security-and-audit-solution-data-security"></a><span data-ttu-id="962d0-103">Adatbiztonság az Operations Management Suite biztonsági és auditálási megoldásban</span><span class="sxs-lookup"><span data-stu-id="962d0-103">Operations Management Suite Security and Audit solution data security</span></span>
<span data-ttu-id="962d0-104">toohelp felhasználók megakadályozása, észlelésében és kezelésében toothreats, [Operations Management Suite (OMS) biztonsági és naplózási megoldás](operations-management-suite-overview.md) gyűjt, és feldolgozza az erőforrások adatait, amely tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="962d0-104">toohelp customers prevent, detect, and respond toothreats, [Operations Management Suite  (OMS) Security and Audit Solution](operations-management-suite-overview.md) collects and processes data about your resources, which includes:</span></span>

* <span data-ttu-id="962d0-105">Biztonsági eseménynapló</span><span class="sxs-lookup"><span data-stu-id="962d0-105">Security event log</span></span>
* <span data-ttu-id="962d0-106">A Windows esemény-nyomkövetés (ETW) eseményei</span><span class="sxs-lookup"><span data-stu-id="962d0-106">Event Tracing for Windows (ETW) events</span></span>
* <span data-ttu-id="962d0-107">AppLocker naplózási események</span><span class="sxs-lookup"><span data-stu-id="962d0-107">AppLocker auditing events</span></span>
* <span data-ttu-id="962d0-108">A Windows tűzfal naplója</span><span class="sxs-lookup"><span data-stu-id="962d0-108">Windows Firewall log</span></span>
* <span data-ttu-id="962d0-109">Advanced Threat Analytics-események</span><span class="sxs-lookup"><span data-stu-id="962d0-109">Advanced Threat Analytics events</span></span>
* <span data-ttu-id="962d0-110">A biztonsági alapkonfiguráció értékelése</span><span class="sxs-lookup"><span data-stu-id="962d0-110">Results of baseline assessment</span></span>
* <span data-ttu-id="962d0-111">Kártevőirtó-felmérés eredménye</span><span class="sxs-lookup"><span data-stu-id="962d0-111">Results of antimalware assessment</span></span>
* <span data-ttu-id="962d0-112">Frissítések/javítások felmérésének eredménye</span><span class="sxs-lookup"><span data-stu-id="962d0-112">Results of update/patch assessment</span></span>
* <span data-ttu-id="962d0-113">Rendszerbejegyzések adatfolyamok kifejezetten engedélyezett hello ügynökön</span><span class="sxs-lookup"><span data-stu-id="962d0-113">Syslogs streams that are explicitly enabled on hello agent</span></span>

<span data-ttu-id="962d0-114">Erős kötelezettségvállalások tooprotect hello adatvédelmi és biztonsági ezeket az adatokat biztosítjuk.</span><span class="sxs-lookup"><span data-stu-id="962d0-114">We make strong commitments tooprotect hello privacy and security of this data.</span></span> <span data-ttu-id="962d0-115">A Microsoft megfelelő toostrict megfelelőségi és biztonsági irányelvek – toooperating egy szolgáltatás-ból.</span><span class="sxs-lookup"><span data-stu-id="962d0-115">Microsoft adheres toostrict compliance and security guidelines—from coding toooperating a service.</span></span>
<span data-ttu-id="962d0-116">Ez a cikk bemutatja, hogyan kezeli az OMS biztonsági és auditálási megoldás az adatokat, illetve hogyan gondoskodik a védelmükről.</span><span class="sxs-lookup"><span data-stu-id="962d0-116">This article explains how data is managed and safeguarded in OMS Security and Audit Solution.</span></span>

## <a name="data-sources"></a><span data-ttu-id="962d0-117">Adatforrások</span><span class="sxs-lookup"><span data-stu-id="962d0-117">Data sources</span></span>
<span data-ttu-id="962d0-118">OMS biztonsági és naplózási megoldás elemzése a hello OMS-ügynököt futtató virtuális gépek és fizikai számítógépek adatokból.</span><span class="sxs-lookup"><span data-stu-id="962d0-118">OMS Security and Audit Solution analyze data from your Virtual Machines and physical computers where hello OMS Agent is installed.</span></span> <span data-ttu-id="962d0-119">Az OMS biztonsági és auditálási megoldás konfigurációs információkat gyűjthet a biztonsági eseményekről, például a Windows-eseményekről, az auditnaplókról, az IIS-naplókról és a syslog-üzenetekről.</span><span class="sxs-lookup"><span data-stu-id="962d0-119">OMS Security and Audit Solution can collect configuration information about security events, such as Windows event, audit logs, IIS logs and syslog messages.</span></span> <span data-ttu-id="962d0-120">A gyűjtött adatok például a következők: az operációs rendszer típusa és verziója, a futó folyamatok, a gép neve, az IP-címek, a bejelentkezett felhasználó és a bérlő azonosítója.</span><span class="sxs-lookup"><span data-stu-id="962d0-120">Examples of such data are: operating system type and version, running processes, machine name, IP addresses, logged in user, and tenant ID.</span></span>  

## <a name="data-protection"></a><span data-ttu-id="962d0-121">Adatvédelem</span><span class="sxs-lookup"><span data-stu-id="962d0-121">Data protection</span></span>
<span data-ttu-id="962d0-122">**Az adatok elkülönítése**: adatok az egyes összetevők teljes hello szolgáltatás logikailag külön maradnak.</span><span class="sxs-lookup"><span data-stu-id="962d0-122">**Data segregation**: Data is kept logically separate on each component throughout hello service.</span></span> <span data-ttu-id="962d0-123">Az összes adat szervezet szerint van megcímkézve.</span><span class="sxs-lookup"><span data-stu-id="962d0-123">All data is tagged per organization.</span></span> <span data-ttu-id="962d0-124">A címkézés továbbra is fennáll, hello adatok életciklus során, és van kényszerítve hello szolgáltatást minden egyes rétegben.</span><span class="sxs-lookup"><span data-stu-id="962d0-124">This tagging persists throughout hello data lifecycle, and it is enforced at each layer of hello service.</span></span> 

<span data-ttu-id="962d0-125">**Adat-hozzáférési**: tooprovide biztonsági javaslatok és vizsgálja meg az esetleges biztonsági fenyegetéseket jelezhetnek, a Microsoft személyzete elérhessék az összegyűjtött vagy a szolgáltatások által elemzett információkkal.</span><span class="sxs-lookup"><span data-stu-id="962d0-125">**Data access**: tooprovide security recommendations and investigate potential security threats, Microsoft personnel may access information collected or analyzed by services.</span></span> <span data-ttu-id="962d0-126">Toohello mérvadónak [Microsoft Online Services használati](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) és [adatvédelmi nyilatkozat](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), amely adja meg, hogy Microsoft nem használja a felhasználói adatok, vagy származnia hirdetését vagy hasonló származó információk kereskedelmi célra.</span><span class="sxs-lookup"><span data-stu-id="962d0-126">We adhere toohello [Microsoft Online Services Terms](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) and [Privacy Statement](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), which state that Microsoft will not use Customer Data or derive information from it for any advertising or similar commercial purposes.</span></span> <span data-ttu-id="962d0-127">biztonsági javaslatok tooprovide és vizsgálja meg az esetleges biztonsági fenyegetéseket jelezhetnek, a Microsoft személyzete elérhessék az összegyűjtött vagy a szolgáltatások által elemzett információkkal.</span><span class="sxs-lookup"><span data-stu-id="962d0-127">tooprovide security recommendations and investigate potential security threats, Microsoft personnel may access information collected or analyzed by services.</span></span> <span data-ttu-id="962d0-128">Csak vesszük ügyféladatok szükséges tooprovide, az Azure szolgáltatások, beleértve azokat a szolgáltatásokat nyújtó kompatibilis céljából.</span><span class="sxs-lookup"><span data-stu-id="962d0-128">We only use Customer Data as needed tooprovide you with Azure services, including purposes compatible with providing those services.</span></span> <span data-ttu-id="962d0-129">Amely a saját adatok összes jogok tooyour megőrzése mellett.</span><span class="sxs-lookup"><span data-stu-id="962d0-129">You retain all rights tooyour own data.</span></span>

<span data-ttu-id="962d0-130">**Adatok felhasználásával**: Microsoft minták használ, illetve több körében tapasztalható fenyegetésfelderítési adataival bérlők tooenhance a megelőzése és észlelési képességek, azt megteheti hello adatvédelmi kötelezettségvállalások leírtak szerint a [adatvédelmi Utasítás](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).</span><span class="sxs-lookup"><span data-stu-id="962d0-130">**Data use**: Microsoft uses patterns and threat intelligence seen across multiple tenants tooenhance our prevention and detection capabilities; we do so in accordance with hello privacy commitments described in our [Privacy Statement](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="962d0-131">Adatok helye szintjén állítható be hello OMS munkaterület, hello kezdeti OMS biztonsági és a naplózási konfiguráció folyamat része hello munkaterület létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="962d0-131">Data location is configured at hello OMS workspace level, during hello workspace creation, which is part of hello initial OMS Security and Audit configuration process.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="962d0-132">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="962d0-132">See also</span></span>
<span data-ttu-id="962d0-133">Ebből a dokumentumból megtudta, hogyan kezeli az OMS az adatokat, és hogyan gondoskodik a védelmükről.</span><span class="sxs-lookup"><span data-stu-id="962d0-133">In this document, you learned how data is managed and safeguarded in OMS.</span></span> <span data-ttu-id="962d0-134">További információ az OMS biztonsági és naplózási megoldás toolearn lásd:</span><span class="sxs-lookup"><span data-stu-id="962d0-134">toolearn more about OMS Security and Audit solution, see:</span></span>

* [<span data-ttu-id="962d0-135">Az Operations Management Suite (OMS) áttekintése</span><span class="sxs-lookup"><span data-stu-id="962d0-135">Operations Management Suite (OMS) overview</span></span>](operations-management-suite-overview.md)
* [<span data-ttu-id="962d0-136">Figyelés és a válaszoló tooSecurity riasztásait az Operations Management Suite biztonsági és naplózási megoldás</span><span class="sxs-lookup"><span data-stu-id="962d0-136">Monitoring and Responding tooSecurity Alerts in Operations Management Suite Security and Audit Solution</span></span>](oms-security-responding-alerts.md)
* [<span data-ttu-id="962d0-137">Az erőforrások figyelése az Operations Management Suite biztonsági és auditálási megoldásban</span><span class="sxs-lookup"><span data-stu-id="962d0-137">Monitoring Resources in Operations Management Suite Security and Audit Solution</span></span>](oms-security-monitoring-resources.md)

