---
title: "aaaAzure biztonsági kezelés és figyelés áttekintése |} Microsoft Docs"
description: " Azure biztonsági mechanizmusok tooaid hello kezelése és figyelése Azure felhőszolgáltatások és virtuális gépek a biztosít.  Ez a cikk áttekintése ezeket az alapvető biztonsági funkciókat és szolgáltatásokat. "
services: security
documentationcenter: na
author: TerryLanfear
manager: StevenPo
editor: TomSh
ms.assetid: 5cf2827b-6cd3-434d-9100-d7411f7ed424
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: terrylan
ms.openlocfilehash: 0026fa97bab7e15c9f8de6856b5075abe2288f61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-management-and-monitoring-overview"></a>Az Azure biztonsági kezelési és figyelési áttekintés
Azure biztonsági mechanizmusok tooaid hello kezelése és figyelése Azure felhőszolgáltatások és virtuális gépek a biztosít. Ez a cikk áttekintése ezeket az alapvető biztonsági funkciókat és szolgáltatásokat. Hivatkozásokkal érhető el, amelyek részletes biztosítanak, így további tooarticles.

a Microsoft más felhőszolgáltatásaival hello biztonsági szintje a egy partneri kapcsolat áll fenn, és a megosztott felelősség és a Microsoft között. Megosztott felelőssége azt jelenti, hogy a Microsoft hello Microsoft Azure és az Adatközpont fizikai biztonságát felelős (például zárolt jelvény bejegyzés kerítések, vagy megóvja a biztonsági védelmet használatával). Ezenkívül az Azure biztosít erős szintű felhőalapú biztonságot hello szoftver rétegben, amely megfelel a hello biztonsági, adatvédelmi és megfelelőségi igények nagy ügyfelei.

Ön a tulajdonosa az adatok és identitások, hello felelősséget védi őket, a helyszíni erőforrások biztonsági hello és hello biztonsági felhő összetevők, amelyben az informatikai részleg ellenőrzése. A Microsoft biztosít Önnek a biztonsági vezérlők és képességek toohelp védelme adatait és alkalmazásait. A biztonsági felelősséget fokú hello típusú felhőalapú szolgáltatás alapul.

a következő diagram hello hello egyenleg a Microsoft és a hello ügyfél felelősségi foglalja össze.

![Közös felelősség][1]

A biztonsági felügyeletbe mélyebb bemutatója, lásd: [biztonságkezelés az Azure-ban](azure-security-management.md).

Az alábbiakban a cikkben szereplő hello alapvető szolgáltatások toobe:

* Szerepköralapú hozzáférés-vezérlés
* Kártevőirtó
* Multi-Factor Authentication
* ExpressRoute
* Virtuális hálózati átjárók
* Privileged identity management
* Identity Protection
* Security Center

## <a name="role-based-access-control"></a>Szerepköralapú hozzáférés-vezérlés
Szerepköralapú hozzáférés-vezérlés (RBAC) biztosít a részletes hozzáféréskezelést az Azure-erőforrások. Az RBAC használata, biztosíthat személyek csak hello mértékű hozzáférést, hogy be kell tooperform a munkájukat.  Az RBAC segítségével győződjön meg arról, hogy ha személyek hagyja hello szervezet megszakad a hozzáférési tooresources hello felhőben.

További információ:

* [Szerepalapú az Active Directory csapat blogja](http://i1.blogs.technet.com/b/ad/archive/2015/10/12/azure-rbac-is-ga.aspx)
* [Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md)

## <a name="antimalware"></a>Kártevőirtó
Az Azure-használhatja például a Microsoft, a Symantec, Trend Micro, McAfee fő biztonsági szállítóktól származó kártevőirtó szoftver, és Kaspersky toohelp a virtuális gépek védelmét a rosszindulatú fájlok, hirdetéseket és más fenyegetésekkel szemben.

A Microsoft Antimalware kínál, akkor hello képességét tooinstall PaaS szerepkörök és a virtuális gépek kártevőirtó ügynök. A System Center Endpoint Protection alapján, ez a funkció számos lehetőséget kínál helyszíni bevált biztonsági technológia toohello felhő.

Szoros integrációja trend meg is fel [mély biztonsági](http://www.trendmicro.com/us/enterprise/cloud-solutions/deep-security/)™ és [SecureCloud](http://www.trendmicro.com/us/enterprise/cloud-solutions/secure-cloud/)™ termékek hello Azure platformon. DeepSecurity víruskereső megoldás és SecureCloud egy titkosítási megoldás. DeepSecurity egy bővítmény modellt használó virtuális gépeken belül van telepítve. Hello portál felhasználói felület és a PowerShell használatával, dönthet úgy toouse DeepSecurity új virtuális gépek láthatók, amelyek alatt hoz létre, vagy a már telepített meglévő virtuális gépeken belül.

Symantec végpont védelme (Szeptember) az Azure-on is támogatott. Portál integrálásán keresztül ügyfelek adhatja meg, hogy szeretné-toouse Szeptember egy virtuális Gépen belül. Szeptember is telepíthető egy teljesen új virtuális gép hello Azure-portálon keresztül, vagy egy meglévő virtuális gép PowerShell használatával is telepíthető.

További információ:

* [Kártevőirtó megoldások telepítése Azure-beli virtuális gépeken](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
* [A Microsoft Antimalware Azure Felhőszolgáltatások és virtuális gépek](azure-security-antimalware.md)
* [Hogyan tooinstall és a Trend Micro mély biztonságának konfigurálása a Windows virtuális gép szolgáltatásként](../virtual-machines/windows/classic/install-trend.md)
* [Hogyan tooinstall és a Symantec Endpoint Protection konfigurálása a Windows virtuális gép](../virtual-machines/windows/classic/install-symantec.md)
* [Új kártevőirtó lehetőségei az Azure virtuális gépek – McAfee az Endpoint Protection védelme](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication
Az Azure többtényezős hitelesítés (MFA), amely hello egynél több ellenőrzési módszer használatát igényli, és a kritikus fontosságú második réteget biztonsági toouser bejelentkezéseket és tranzakciókat ad hitelesítési mód. MFA segít a biztonságos működés érdekében hozzáférés toodata és alkalmazások mellett egyszerű bejelentkezési folyamatot a felhasználó igény szerint. Erős hitelesítés, ellenőrzési lehetőségek széles keresztül biztosítja – a telefonhívás, szöveges üzenet vagy mobilalkalmazás értesítés vagy ellenőrző kód és a harmadik féltől származó OATH-tokeneket.

További információ:

* [Többtényezős hitelesítés](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [Mi az az Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)
* [Azure multi-factor Authentication működése](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="expressroute"></a>ExpressRoute
A Microsoft Azure ExpressRoute lehetővé teszi a helyszíni hálózatokhoz kiterjeszti a Microsoft cloud hello könnyíteni a kapcsolat szolgáltatóját egy dedikált titkos kapcsolaton keresztül. Az ExpressRoute létrehozhat kapcsolatok tooMicrosoft felhőszolgáltatások, például a Microsoft Azure, a Office 365 és a CRM Online-hoz. A kapcsolatok lehetnek: bármely elemek közötti (IP VPN) hálózat, pontok közötti Ethernet-hálózat vagy egy virtuális keresztkapcsolat egy kapcsolatszolgáltatón keresztül egy közös elhelyezési létesítményben. Az ExpressRoute-kapcsolatok ne lépje át hello nyilvános internethez. Ez lehetővé teszi ExpressRoute kapcsolatok toooffer további megbízhatóságát, gyorsabb sebességű, kisebb késések fordulnak elő, és nagyobb biztonságot nyújtana tipikus kapcsolatok hello interneten keresztül.

További információ:

* [ExpressRoute műszaki áttekintés](../expressroute/expressroute-introduction.md)

## <a name="virtual-network-gateways"></a>Virtuális hálózati átjárók
VPN-átjárók, más néven az Azure virtuális hálózati átjárók használt virtuális hálózatok és a helyszíni helyek közötti toosend hálózati forgalmat. Azok is használt toosend forgalom az Azure (VNet – VNet) több virtuális hálózat között.  VPN-átjárók adja meg az Azure és az infrastruktúra közötti biztonságos létesítmények közötti kapcsolathoz.

További információ:

* [Információk a VPN-átjárókról](../vpn-gateway/vpn-gateway-about-vpngateways.md)
* [Az Azure biztonsági áttekintése](security-network-overview.md)

## <a name="privileged-identity-management"></a>Privileged Identity Management
Néha a felhasználóknak kell toocarry ki az Azure-erőforrások vagy más SaaS-alkalmazásokhoz privilegizált műveleteket. Ez gyakran azt jelenti, a szervezetek toogive őket állandó jogosultsági szintű hozzáférés az Azure Active Directory (Azure AD). Ez a felhőben üzemeltetett erőforrásokhoz az egyre növekvő biztonsági kockázatot jelent, mert a szervezeteknek elég nem tud figyelni, ezek a felhasználók tevékenységeit a rendszerjogosultságú hozzáférést.
Továbbá ha jogosultsági szintű hozzáféréssel rendelkező felhasználói fiók biztonsága sérül, egy megsértésének jelentős hatással lehet az általános felhő biztonsági. Az Azure AD Privileged Identity Management a kockázat segít tooresolve jogosultságokat hello kitettség idő csökkentése és a használati láthatósága növelésével.  

A privileged Identity Management bemutatja hello egy ideiglenes rendszergazdai egy szerepkör vagy a "csak az időben" rendszergazdai hozzáféréssel, amely a felhasználókat, akiknek az aktiválási folyamat az adott szerepkörrel toocomplete kell. hello aktiválási folyamat módosítások hello inaktív tooactive, az Azure AD-hez egy adott időszakra vonatkozó, például a nyolc óra hello felhasználói tooa szerepkör-hozzárendelést.

További információ:

* [Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md)
* [Ismerkedés az Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-getting-started.md)

## <a name="identity-protection"></a>Identity Protection
Az Azure Active Directory (AD) identitás Protection gyanús bejelentkezési tevékenységek konszolidált nézetét jeleníti meg, és a potenciális biztonsági réseket toohelp az üzleti védelmet. Azonosító adatok védelmét a felhasználók gyanús tevékenységek észlelése és a kiemelt (rendszergazda) identitások, a találgatásos támadásokkal, például jelek alapján elszivárgott a hitelesítő adatokat, és bejelentkezések ismeretlen helyekről, és fertőzött eszközök.

Értesítések és az ajánlott javítási megadásával Identity Protection segít valós időben toomitigate kockázatokat. Felhasználói kockázat súlyosság alapján számítja ki, és konfigurálhatja a kockázat-alapú házirendek tooautomatically súgó hozzáférés biztonságossá tételét alkalmazás jövőbeli fenyegetések ellen.

További információ:

* [Az Azure Active Directory azonosító adatok védelmét](../active-directory/active-directory-identityprotection.md)
* [9. csatornán: Az Azure AD és az Identity: Identity Protection előzetes kiadásának](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="security-center"></a>Security Center
Az Azure Security Center segít a megakadályozása, észlelésében és kezelésében toothreats, és biztosítja, hogy lássák, és szabályozhatják, az Azure-erőforrások biztonságának hello nőtt. Az ügyfél összes előfizetésére kiterjedő, integrált biztonsági monitorozást és szabályzatkezelést biztosít, megkönnyíti a nehezen észlelhető fenyegetések azonosítását, és számos biztonsági megoldással együttműködik.

A Security Center segítségével optimalizálhatja és hello biztonsági által az Azure-erőforrások figyelése:

* Így lehetővé teszi az Azure-előfizetés erőforrások szerint tooyour toodefine házirendek a vállalat biztonsági kell, és az alkalmazások típusának vagy az egyes előfizetések hello adatok érzékenységének hello.
* Az Azure virtuális gépeken, a hálózat és az alkalmazások hello állapotának figyelése.
* Listájával rangsorolt biztonsági riasztások, beleértve az integrált partneri megoldások tooquickly kell hello információk mellett vizsgálata és javaslatok kapcsolatos riasztásait a támadás tooremediate.

További információ:

* [A Security Center bemutatása tooAzure](../security-center/security-center-intro.md)

<!--Image references-->
[1]: ./media/security-management-and-monitoring-overview/shared-responsibility.png
