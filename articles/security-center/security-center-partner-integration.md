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
# <a name="partner-integration-in-azure-security-center"></a>Partnerintegráció az Azure Security Centerben

Ez a cikk azt ismerteti, hogyan integrálható az Azure Security Center partnerek toohelp növelheti, hogy a teljes biztonságában. A Security Center az Azure-ban integrált megoldást nyújt, és kihasználja a hello Azure piactér partner hitelesítő és számlázási.

> [!NOTE] 
> Frissítésétől. június 2017 Security Center hello Microsoft Monitoring Agent toocollect és a tároló adatait használja. További információk: [Az Azure Security Center platform migrálása](security-center-platform-migration.md). a cikkben szereplő információkat hello Security Center funkció átmenet toohello Microsoft Monitoring Agent után jelöli.
>

## <a name="why-deploy-partner-solutions-from-security-center"></a>Miért érdemes a Security Centerből üzembe helyezni a partnermegoldásokat?

A Security Center négy fő oka annak tooleverage partner integrálást a következők:

- **Egyszerű üzembe helyezés**. A partner megoldás telepítése a következő hello Security Center ajánlás szerint sokkal könnyebben. hello telepítési folyamat egy alapértelmezett telepítés és a hálózati topológia segítségével teljes mértékben automatizálható. Az ügyfelek választhatják a félig automatikus lehetőséget is, amely rugalmasabb és nagyobb mértékben testre szabható.
- **Integrált észlelések**. A partnermegoldásoktól érkező biztonsági eseményeket a rendszer automatikusan összegyűjti, összesíti és megjeleníti a Security Center riasztásainak és incidenseinek részeként. Ezeket az eseményeket is vannak illesztett észlelések a más forrásokból tooprovide advanced threat-észlelési képességek.
- **Egyesített állapotmonitorozás és -kezelés**. Az ügyfelek használhatják integrált állapotfigyelő események toomonitor összes partneri megoldások egy pillanat alatt. Alapvető felügyeleti esetén érhető el, egyszerű hozzáférés tooadvanced beállítása hello partneri megoldás segítségével.
- **Exportálja a tooSIEM**. Az ügyfelek összes Security Center exportálhatók és partner riasztások közös esemény formátum (CEF) tooon helyszíni biztonsági adatai és az esemény felügyeleti SIEM-rendszerekben az Azure naplóelemzés integrálása révén (előzetes verzió).


## <a name="partners-that-integrate-with-security-center"></a>Security Center-integrációt kínáló partnerek

A Security Center jelenleg ezekkel a megoldásokkal van integrálva:

- Végpontvédelem ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), Symantec és [Microsoft Antimalware az Azure Cloud Services és Virtual Machines szolgáltatásokhoz](https://docs.microsoft.com/azure/security/azure-security-antimalware)) 
- Webalkalmazás-tűzfal ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/resources.html?limit=10&search=&document-type=data-sheets) és [Azure Application Gateway](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/)) 
- Új generációs tűzfalmegoldások ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2) és [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html)) 
- Biztonságirés-felmérés ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/))  

Adott idő alatt, a Security Center fog bontsa ki ezen kategóriák belül partnerek hello számát, és új kategória hozzáadása. 

## <a name="deploy-a-partner-solution"></a>Partnermegoldás üzembe helyezése

Hello beállítása az Azure-alapú környezetben és a meghatározott hello biztonsági házirend alapján, a Security Center használatát javasolja, hogy telepít egy partneri megoldást. a Security Center javaslat hello végigvezeti hello kiválasztása és telepítése egy partneri megoldást. hello általános üzembe helyezését változhat, hello típus megoldás és a partner használja. További információkért tekintse meg a következő cikkek hello:

- [Végpontvédelem telepítése](security-center-install-endpoint-protection.md)
- [Webalkalmazási tűzfal hozzáadása](security-center-add-web-application-firewall.md)
- [Új generációs tűzfal hozzáadása](security-center-add-next-generation-firewall.md)
- [A sebezhetőségi felmérés nincs telepítve](security-center-vulnerability-assessment-recommendations.md)

## <a name="manage-partner-solutions"></a>A partnermegoldások kezelése

A központi telepítést követően tooview információk készül hello hello megoldás állapotát, és végre alapvető felügyeleti feladatokat, hello **Security Center** panelen, jelölje be hello **partneri megoldások** lehetőséget. A partnermegoldások Security Centerben való kezelésével kapcsolatos további információkért tekintse meg a [Partnermegoldások monitorozása az Azure Security Centerrel](security-center-partner-solutions.md) című cikket.

![Partnerintegráció](./media/security-center-partner-integration/security-center-partner-integration-fig1-new2.png)

> [!NOTE]
> Symantec endpoint protection támogatási korlátozott toodiscovery. Az állapotriasztások nem érhetők el.
>

## <a name="see-also"></a>Lásd még:

Ebben a cikkben megtanulta, hogyan toointegrate partnermegoldások az Azure Security Centerben. További információ a Security Center toolearn tekintse meg a következő cikkek hello:

* [Útmutató a Security Center tervezéséhez és működtetéséhez](security-center-planning-and-operations-guide.md)
* [Kezelésének és megoldásának toosecurity riasztásokat a Security Center](security-center-managing-and-responding-alerts.md)
* [Biztonsági riasztások típus szerint a Security Centerben](security-center-alerts-type.md)
* [Biztonsági állapot monitorozása a Security Centerben](security-center-monitoring.md). Ismerje meg, hogyan toomonitor hello az Azure-erőforrások állapotát.
* [Partnermegoldások monitorozása a Security Centerrel](security-center-partner-solutions.md). Ismerje meg, hogyan toomonitor hello partneri megoldások biztonsági állapotát.
* [Azure Security Center – gyakori kérdések](security-center-faq.md) Válaszok toofrequently hello szolgáltatás használatára vonatkozó kérdések beolvasása.
* [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/). Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.
