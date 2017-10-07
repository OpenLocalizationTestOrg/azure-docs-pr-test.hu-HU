---
title: "Kapcsolatszolgáltatók és helyek: Azure ExpressRoute | Microsoft Docs"
description: "Ez a cikk részletes áttekintést nyújt a helyek és azokat a szolgáltatásokat kínálják hogyan tooconnect tooAzure régiók. Kapcsolatszolgáltatók szerint rendezve."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
ms.assetid: c878513a-d594-42ad-8b0e-403efd0c4b25
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/17/2017
ms.author: kaanan
ms.openlocfilehash: df906ae6ff4e149c9cab4aa46ab78c8dd6aa4366
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-partners-and-peering-locations"></a>ExpressRoute-partnerek és társviszony-létesítési helyszínek.

> [!div class="op_single_selector"]
> * [Helyek szolgáltató alapján](expressroute-locations.md)
> * [Szolgáltatók hely alapján](expressroute-locations-providers.md)


Ebben a cikkben hello táblák ExpressRoute kapcsolat szolgáltatók, melyek ExpressRoute, ExpressRoute és ExpressRoute rendszerintegrátorok (SIs) keresztül támogatja a Microsoft más felhőszolgáltatásaival tájékoztatást nyújt.

## <a name="partners"></a>ExpressRoute-kapcsolatszolgáltatók
Az ExpressRoute az összes Azure-régióban és -helyen támogatott. a következő térkép hello Azure-régiók és a helyek ExpressRoute listáját tartalmazza. Helyek ExpressRoute toothose, ahol a Microsoft több szolgáltatókkal állomásokhoz hivatkozik.

![Helyek térképe][0]

Között geopolitikai régión belül minden egyes tooAzure szolgáltatást lesz, ha létrejött a kapcsolat tooat legalább egy ExpressRoute hely hello geopolitikai régión belül.

### <a name="azure-regions-tooexpressroute-locations-within-a-geopolitical-region"></a>Azure-régiók tooExpressRoute helyek geopolitikai régión belül.
a következő táblázat hello biztosít Azure-régiók térképe tooExpressRoute helyek geopolitikai régión belül.

| **Geopolitikai régió** | **Azure-régiók** | **ExpressRoute-helyek** |
| --- | --- | --- |
| **Észak-Amerika** |USA keleti régiója, USA nyugati régiója, USA 2. keleti régiója, USA 2. nyugati régiója, USA középső régiója, USA déli középső régiója, USA északi középső régiója, USA középnyugati régiója, Közép-Kanada, Kelet-Kanada |Atlanta, Chicago, Dallas, Denver, Las Vegas, Los Angeles, Miami, New York, San Antonio, Seattle, Szilícium-völgy, Washington, D.C., Montreal, Québec város, Toronto |
| **Dél-Amerika** |Dél-Brazília |Sao Paulo |
| **Európa** |Észak-Európa, Nyugat-Európa, az Egyesült Királyság nyugati része, az Egyesült Királyság déli része |Amszterdam, Dublin, London, Newport (Wales), Párizs |
| **Ázsia** |Kelet-Ázsia, Délkelet-Ázsia |Hongkong, Szingapúr |
| **Japán** |Nyugat-Japán, Kelet-Japán |Oszaka, Tokió |
| **Ausztrália** |Délkelet-Ausztrália, Kelet-Ausztrália |Melbourne, Sydney |
| **India** |Nyugat-India, Közép-India, Dél-India |Csennai, Mumbai |
| **Dél-Korea** |Korea középső régiója, Korea déli régiója |Busan, Szöul |

### <a name="regions-and-geopolitical-boundaries-for-national-clouds"></a>A régiók és az országos felhők geopolitikai határai
hello az alábbi táblázat bemutatja, régiók és geopolitikai határok nemzeti felhők.

| **Geopolitikai régió** | **Azure-régiók** | **ExpressRoute-helyek** |
| --- | --- | --- |
| **Az Egyesült Államok kormányának felhője** |USA-beli államigazgatás – Iowa, USA-beli államigazgatás – Virginia, USA Védelmi Minisztériuma – Középső régió, USA Védelmi Minisztériuma – Keleti régió  |Chicago, Dallas, New York, Seattle, Szilícium-völgy, Washington, D.C. |
| **Kína** |Észak-Kína, Kelet-Kína |Peking, Sanghaj |
| **Németország** |Közép-Németország, Kelet-Németország |Berlin, Frankfurt |

Geopolitikai régiók közötti kapcsolat hello standard ExpressRoute SKU nem támogatott. Szüksége lesz tooenable hello ExpressRoute prémium bővítmény toosupport globális kapcsolatot. Kapcsolat toonational felhőalapú környezetben nem támogatott. Igény esetén tájékozódjon kapcsolatszolgáltatójánál a lehetőségekről.

## <a name="locations"></a>Kapcsolatszolgáltatói helyek

a következő táblázat hello szolgáltató helyek jeleníti meg. Ha azt szeretné, hogy tooview elérhető szolgáltatók hely szerint, lásd: [szolgáltatók helyének](expressroute-locations-providers.md#locations).


### <a name="production-azure"></a>Termelési Azure
| **Szolgáltató** | **Microsoft Azure** | **Office 365 és Dynamics 365** | **Helyek** |
| --- | --- | --- | --- |
| **[AARNet](https://www.aarnet.edu.au/network-and-services/cloud-services-applications/azure-expressroute/)** |Támogatott |Támogatott |Melbourne, Sydney |
| **[Airtel](http://www.airtel.in/business/connexion)** | Támogatott | Támogatott | Csennai, Mumbai |
| **[Aryaka Networks](http://www.aryaka.com/)** |Támogatott |Támogatott |Amszterdam, Dallas, Hongkong, Szilícium-völgy, Szingapúr, Tokió, Washington, D.C. |
| **[Ascenty Data Centers](https://ascenty.com/solucoes/conectividade-e-interconexoes/Microsoft-express-route/)** |Hamarosan elérhető |Hamarosan elérhető |Sao Paulo |
| **[AT&amp;T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Támogatott |Támogatott |Amszterdam, Chicago, Dallas, London, Szilícium-völgy, Szingapúr, Sydney, Tokió, Toronoto, Washington, D.C. |
| **[Bell Canada](https://business.bell.ca/shop/enterprise/cloud-connect-access-to-cloud-partner-services)** |Támogatott |Támogatott |Montréal, Toronto |
| **[British Telecom](http://www.globalservices.bt.com/uk/en/news/bt_to_provide_connectivity_to_microsoft_azure)** |Támogatott |Támogatott |Amszterdam, Hongkong, London, Szilícium-völgy, Szingapúr, Sydney, Tokió, Washington, D.C. |
| **[CenturyLink](http://www.centurylink.com/business/enterprise/services/data-network/mpls-vpn.html)** |Hamarosan elérhető |Hamarosan elérhető |Szilícium-völgy |
| **China Telecom Global** |Támogatott |Nem támogatott |Hongkong |
| **[Cologix](http://www.cologix.com/solutions/cloud-connect/public-clouds/microsoft-cloud/)** |Támogatott |Támogatott |Dallas, Montréal, Toronto |
| **[Colt](http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Támogatott |Támogatott |Amszterdam, Dublin, London, Párizs, Tokió |
| **Comcast** |Támogatott |Támogatott |Chicago, Szilícium-völgy, Washington, D.C. |
| **[Console](https://www.consoleconnect.com/partners/cloudsaas/)**| Támogatott | Támogatott |Szilícium-völgy, Toronto |
| **[CoreSite](http://www.coresite.com/solutions/cloud-services/public-cloud-providers/microsoft-azure-expressroute)** |Támogatott |Támogatott |Denver, Los Angeles, New York |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Támogatott |Támogatott |Amszterdam, Atlanta, Chicago, Dallas, Hongkong, London, Los Angeles, Melbourne, New York, Oszaka, Párizs, Sao Paulo, Seattle, Szilícium-völgy, Szingapúr, Sydney, Tokió, Toronto, Washington, D.C. |
| **euNetworks** |Támogatott |Támogatott |Amszterdam |
| **GÉANT** |Támogatott |Támogatott |Amszterdam |
| **[Global Cloud Xchange (GCX)] (http://globalcloudxchange.com/cloud-platform/cloud-x-fusion/cloud-x-fusion-for-azure/)** | Támogatott| Támogatott | Csennai |
| **[InterCloud](https://www.intercloud.com/)** |Támogatott |Támogatott |Amszterdam, London, Szingapúr, Washington, D.C. |
| **[Internet Initiative Japan Inc. - IIJ](http://www.iij.ad.jp/en/news/pressrelease/2015/1216-2.html)** |Támogatott |Támogatott |Oszaka, Tokió |
| **Internet Solutions - Cloud Connect** |Támogatott |Támogatott |Amszterdam, London |
| **[Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/Microsoft-Azure/)** |Támogatott |Támogatott |Amszterdam, Dublin, London, Párizs |
| **Jisc** |Támogatott |Támogatott |London |
| **KINX** |Támogatott |Támogatott |Szöul |
| **[KPN](http://www.kpn.com/cloudconnect)** | Támogatott | Támogatott | Amszterdam | 
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Támogatott |Támogatott |Amszterdam, Chicago, Dallas, Las Vegas, London, São Paulo, Seattle, Szilícium-völgy, Szingapúr, Washington, D.C. |
| **LG CNS** |Támogatott |Támogatott |Busan, Szöul |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Támogatott |Támogatott |Amszterdam, Chicago, Dallas, Hongkong, Las Vegas, London, Los Angeles, Melbourne, Miami, New York, Quebec város, San Antonio, Seattle, Szilícium-völgy, Szingapúr, Sydney, Toronto, Washington, D.C. |
| **MTN** |Támogatott |Támogatott |London |
| **[Neutrona Networks](http://www.neutrona.com/index.php/services#cloud-connect)** |Támogatott |Támogatott |Miami, Sao Paulo |
| **[Következő generációs adatok](http://www.nextgenerationdata.co.uk/ngd-cloud-gateway/)** |Támogatott |Támogatott |Newport (Wales) |
| **NEXTDC** |Támogatott |Támogatott |Melbourne, Sydney |
| **[NTT Communications](http://www.ntt.com/en/services/network/virtual-private-network.html)** |Támogatott |Támogatott |London, Los Angeles, Oszaka, Szingapúr, Tokió, Washington, D.C. |
| **[NTT SmartConnect](http://cloud.nttsmc.com/cxc/azure.html)** |Támogatott |Támogatott |Oszaka |
| **[Orange](http://www.orange-business.com/en/products/business-vpn-galerie)** |Támogatott |Támogatott |Amszterdam, Hong Kong, London, Párizs, Szilícium-völgy, Szingapúr, Sydney, Washington, D.C. |
| **PCCW Global Limited** |Támogatott |Támogatott |Hongkong |
| **Sejong Telecom** |Támogatott |Támogatott |Szöul |
| **[SIFY](http://telecom.sify.com/azure-expressroute.html)** |Támogatott |Támogatott |Csennai |
| **[SingTel](http://info.singtel.com/about-us/news-releases/singtel-provide-secure-private-access-microsoft-azure-public-cloud)** |Támogatott |Támogatott |Szingapúr |
| **[Softbank](http://www.softbank.jp/biz/cloud/cloud_access/direct_access_for_az/)** |Támogatott |Támogatott |Oszaka, Tokió |
| **[Tata Communications](http://www.tatacommunications.com/lp/izo/azure/azure_index.html)** |Támogatott |Támogatott |Amszterdam, Csennai, Hongkong, London, Mumbai, Szilícium-völgy, Szingapúr, Washington, D.C. |
| **[TeleCity Group](http://www.telecitygroup.com/investor-centre/news_details.htm?locid=03100500400b00d&xml)** |Támogatott |Támogatott |Amszterdam, Dublin, London |
| **[Telefonica](https://www.business-solutions.telefonica.com/es/enterprise/solutions/efficient-infrastructure/managed-voice-data-connectivity/)** |Támogatott |Támogatott |Amszterdam, Sao Paulo |
| **[Telehouse – KDDI](http://www.telehouse.net/solutions/cloud-services/cloud-link)** |Támogatott |Támogatott |London |
| **Telenor** |Támogatott |Támogatott |Amszterdam, London |
| **[Telstra Corporation](http://www.telstra.com.au/business-enterprise/network-services/networks/cloud-direct-connect/)** |Támogatott |Támogatott |Melbourne, Sydney |
| **[Telus](http://www.telus.com)** |Támogatott |Támogatott |Toronto |
| **[UOLDIVEO](http://www.uoldiveo.com.br/solucoes/cloud.html#rmcl)** |Támogatott |Támogatott |Sao Paulo |
| **[Verizon](http://www.verizonenterprise.com/products/networking/secure-cloud-interconnect/)** |Támogatott |Támogatott |Amszterdam, Chicago, Dallas, Hongkong, London, Szilícium-völgy, Szingapúr, Sydney, Tokió, Washington, D.C. |
| **[Vodafone](http://www.vodafone.com/business/global-enterprise/global-connectivity/vodafone-ip-vpn-cloud-connect)** |Támogatott |Nem támogatott |London |
| **[Zayo Group](http://www.zayo.com/solutions/industries/cloud-connectivity/microsoft-expressroute)** |Támogatott |Támogatott |Amszterdam, Chicago, Dallas+, London+, Los Angeles, New York, Szilícium-völgy, Toronto, Washington, D.C. |

 **+** = hamarosan elérhető

### <a name="national-cloud-environment"></a>Országos felhőkörnyezet

### <a name="us-government-cloud"></a>Az Egyesült Államok kormányának felhője
| **Szolgáltató** | **Microsoft Azure** | **Office 365** | **Helyek** |
| --- | --- | --- | --- |
| **[AT&amp;T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Támogatott |Támogatott |Chicago, Washington, D.C. |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Támogatott |Támogatott |Chicago, Dallas, New York, Seattle, Szilícium-völgy, Washington, D.C. |
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Támogatott |Támogatott |Chicago, New York+, Szilícium-völgy, Washington, D.C. |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Támogatott | Támogatott | Chicago, Dallas |
| **[Verizon](http://news.verizonenterprise.com/2014/04/secure-cloud-interconnect-solutions-enterprise/)** |Támogatott |Támogatott |Chicago, Dallas, New York, Washington, D.C. |

### <a name="china"></a>Kína
| **Szolgáltató** | **Microsoft Azure** | **Office 365** | **Helyek** |
| --- | --- | --- | --- |
| **China Telecom** |Támogatott |Nem támogatott |Peking, Sanghaj |

több, lásd: toolearn [Kínában ExpressRoute](http://www.windowsazure.cn/home/features/expressroute/).

### <a name="germany"></a>Németország
| **Szolgáltató** | **Microsoft Azure** | **Office 365** | **Helyek** |
| --- | --- | --- | --- |
| **[Colt](http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Támogatott |Nem támogatott |Berlin+, Frankfurt |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Támogatott |Nem támogatott |Frankfurt |
| **[e-shelter](https://www.e-shelter.de/en/microsoft-expressroutetm)** |Támogatott |Nem támogatott |Berlin |
| **Interxion** |Támogatott |Nem támogatott |Frankfurt |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Támogatott  | Nem támogatott | Berlin |
| **T-Systems** |Támogatott |Nem támogatott |Berlin |

## <a name="connectivity-through-exchange-providers"></a>Kapcsolódás adatcsere-szolgáltatókon keresztül

Ha a kapcsolatszolgáltató nincs felsorolva az előző szakaszokban, akkor is létesíthet kapcsolatot.

* Kérje meg a kapcsolat szolgáltató toosee, ha azok a fenti hello tábla hello cseréjét csatlakoztatott tooany. Ellenőrizheti a hello következő hivatkozásait toogather exchange szolgáltatók által kínált szolgáltatások további információt. Több kapcsolat szolgáltatók a következők: már csatlakoztatott tooEthernet cseréjét.
  * [Cologix](http://www.cologix.com/)
  * [Console](https://www.consoleconnect.com/partners/cloudsaas/)
  * [CoreSite](http://www.coresite.com/)
  * [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [Interxion](http://www.interxion.com/products/interconnection/cloud-connect/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [NextDC](http://www.nextdc.com/)
  * [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
* A kapcsolat szolgáltatójánál, a hálózati toohello társviszony-létesítési helye választott kiterjesztése rendelkezik.
  * Győződjön meg róla, hogy a kapcsolatszolgáltató magas rendelkezésre állással terjesztette ki a kapcsolatot, tehát nincsenek kritikus hibapontok a rendszeren belül.
* Rendelés az hello exchange ExpressRoute-kapcsolatcsoportot, a kapcsolat szolgáltatójánál tooconnect tooMicrosoft.
  * Kövesse lépéseket [ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-classic.md) tooset fel a kapcsolatot.

## <a name="connectivity-through-additional-service-providers"></a>Kapcsolódás további szolgáltatókon keresztül

| **Kapcsolatszolgáltató** | **Exchange** | **Helyek** |
| --- | --- | --- |
| **[1CLOUDSTAR](http://www.1cloudstar.com/service/cloudconnect-azure-expressroute/)** |Equinix |Szingapúr |
| **[Airgate Technologies, Inc.](http://airgate.ca/cloud-express/)** | Equinix, Cologix | Toronto, Montreal |
| **[Alaska Communications](http://www.alaskacommunications.com/For-Your-Business/Direct-Cloud-Service)** |Equinix |Seattle |
| **[Altice Business](https://golightpath.com/transport)** |Equinix |New York, Washington, D.C. |
| **[Arteria Networks Corporation](https://arteria-net.com/business/service/cloud_access/sca/)** |Equinix |Tokió |
| **[Bezeq International Ltd.](https://www.bezeqint.net/english)** | euNetworks | London |
| **[BroadBand Tower, Inc.](http://www.bbtower.co.jp/product-service/data-center/network/dcconnect-for-azure/)** | Equinix | Tokió |
| **[C3ntro Telecom](http://www.c3ntro.com/data/cloud-conectivity/)** | Equinix, Megaport | Dallas |
| **[Cogeco Peer 1](https://www.cogecopeer1.com/en/)**| Equinix | Montréal, Toronto |
| **[Cox Business](https://www.cox.com/business/networking/cloud-connectivity.html)** | Equinix | Dallas, Szilícium-völgy, Washington, D.C. | 
| **[Data Foundry](https://www.datafoundry.com/services/cloud-connect)** | Megaport | Dallas |
| **[Epsilon Telecommunications Limited](http://www.epsilontel.com/data-connectivity/cloud-access/)** | Equinix | London, Szingapúr, Washington, D.C. |
| **[Eurofiber](https://eurofiber.nl/microsoft-azure/)** | Equinix | Amszterdam |
| **[Exponential E](http://www.exponential-e.com/services/connectivity-services/cloud-connect-exchange)** | Equinix | London |
| **[Fastweb S.p.A](http://www.fastweb.it/grandi-aziende/connessione-voce-e-wifi/scheda-prodotto/rete-privata-virtuale/)** | Equinix | Amszterdam |
| **[Gtt Communications Inc](https://www.gtt.net/wp-content/uploads/2017/04/EtherCloud-Data-Sheet.pdf)** |Equinix | Washington, D.C. |
| **[HSO](http://www.hso.co.uk/products/cloud-direct)** |Equinix | London, Slough |
| **LGA Telecom** |Equinix |Szingapúr|
| **[Lightower](http://www.lightower.com/network-solutions/cloud-connect/#microsoft-azure)** |Equinix | Chicago, New York, Washington, D.C. |
| **[Macroview Telecom](http://www.macroview.com/en/scripts/catitem.php?catid=solution&sectionid=expressroute)** |Equinix |Hongkong |
| **[Macquarie Telecom Group](https://macquariegovernment.com/secure-cloud/secure-cloud-exchange/)** | Megaport | Sydney |
| **[Masergy](https://www.masergy.com/solutions/hybrid-networking/cloud-marketplace/microsoft-azure)** | Equinix | Washington, D.C. |
| **[NexGen Networks](http://www.nexgen-net.com/nexgen-networks-direct-connect-microsoft-azure-expressroute.html)** | Interxion | London |
| **[Nianet](https://nianet.dk/produkter/internet/microsoft-expressroute)** |Telecity | Amsterdam, Frankfurt |  
| **[QSC AG](https://www.qsc.de/de/produkte-loesungen/cloud-services-und-it-outsourcing/pure-enterprise-cloud/multi-cloud-management/azure-expressroute/)** |Interxion | Frankfurt |  
| **Rogers** | Cologix, Equinix | Montréal, Toronto |
| **[Tamares Telecom](http://www.tamarestelecom.com/our-services/#Connectivity)** | Telecity | London | 
| **[ThinkTel](http://www.thinktel.ca/services/agile-ix-data/expressroute/)** | Equinix | Toronto | 
| **[Transtelco](http://www.transtelco.net/tcloud/microsoft)** |Equinix | Dallas, Los Angeles |  
| **[United Information Highway (UIH)](https://www.uih.co.th/en/internet-solution/cloud-direct/uih-cloud-direct-for-microsoft-azure-expressroute)**| Equinix | Szingapúr |
| **[Webair](https://www.webair.com/microsoft-express-route-partnership/)**| Megaport | New York |
| **[Windstream](http://www.windstreambusiness.com/solutions/cloud-services/cloud-and-managed-hosting-services)**| Equinix | Chicago, Szilícium-völgy, Washington, D.C. |
| **Zain** |Equinix |London|
| **[Zertia](http://www.zertia.es/index.php/novedades)**| 3-as szint | Madrid |
| **[Zirro](https://zirro.com/services/)**| Equinix | Montréal, Toronto |

## <a name="connectivity-through-datacenter-providers"></a>Kapcsolódás adatközpont-szolgáltatókon keresztül
| **Szolgáltató** | **Exchange** |
| --- | --- |
| **[Cyrus One](https://cyrusone.com/enterprise-data-center-services/connectivity-and-interconnection/cloud-connectivity-reaching-amazon-microsoft-google-and-more/microsoft-azure-expressroute/?doing_wp_cron=1498512235.6733090877532958984375)** | Megaport |
| **[Digital Realty](https://www.digitalrealty.com/services/interconnection/service-exchange/)** | Megaport |
| **[EdgeConnex](http://www.edgeconnex.com/services/edge-data-centers-proximity-matters/)** | Megaport |
| **[RagingWire Data Centers](http://www.ragingwire.com/wholesale/wholesale-data-centers-worldwide-nexcenters)** | Konzol |
| **[T5 Data Centers](http://t5datacenters.com/network-cloud-connect/)** | Konzol |

## <a name="connectivity-through-national-research-and-education-networks-nren"></a>Kapcsolódás nemzeti kutatási és oktatási hálózatokon (NREN) keresztül

| **Szolgáltató**|
| --- |
| **AARNET**| 
| **DeIC, a GÉANT-on keresztül**|
| **GARR, a GÉANT-on keresztül**|
| **GÉANT**|
| **HEAnet, a GÉANT-on keresztül**|
| **JISC**|
| **RedIRIS, a GÉANT-on keresztül**|
| **SINET**|
| **Surfnet, a GÉANT-on keresztül**|

* Ha a kapcsolat szolgáltatójánál itt nem látható, ellenőrizze toosee, ha a fent felsorolt ExpressRoute Exchange partnerek hello csatlakoztatott tooany.

## <a name="expressroute-system-integrators"></a>ExpressRoute-rendszerintegrátorok
A hálózat méretezését hello alapján igényeinek kihívást jelenthet magánhálózati kapcsolat toofit engedélyezése esetén. Dolgozhat sem szerepel a következő tábla tooassist hello hello rendszerintegrátorok meg bevezetési tooExpressRoute.

| **Rendszerintegrátor** | **Kontinens** |
| --- | --- |
| **[Altogee](http://www.altogee.be/expressroute)** | Európa |
| **[Avanade Inc.](http://www.avanade.com/)** | Ázsia, Európa, Észak-Amerika, Dél-Amerika |
| **[Bright Skies GmbH](http://bskies.io/expressroute)** | Európa
| **[Ensyst](http://www.ensyst.com.au)** | Ázsia
| **[Equinix Professional Services](http://www.equinix.com/services/consulting/)** | Észak-Amerika |
| **[FlexManage](http://www.flexmanage.com/cloud)** | Észak-Amerika |
| **[Inframon](http://www.inframon.com/partner/microsoft/)** | Európa |
| **[hello tanácsadási IT-részleg](http://itconsult.com.au/microsoft-expressroute)** | Ausztrália |
| **[MOQdigital](http://www.moqdigital.com.au/insights/technical/network-connectivity-options-for-azure)** | Ausztrália |
| **[MSG Services](https://www.msg-services.de/it-services/managed-services/cloud-outsourcing/)** | Európa (Németország) |
| **[Nelite](http://nelite.com/)** | Európa |
| **[New Signature](https://www.newsignature.co.uk/)** | Európa |
| **[OneAs1a](http://www.oneas1a.com/express-connect-any-cloud-ecac)** | Ázsia |
| **[Orange Networks](https://orange-networks.com/blog/88-azureexpressroute)** | Európa |
| **[Perficient](http://www.perficient.com/Partners/Microsoft/Cloud/Azure-ExpressRoute)** | Észak-Amerika |
| **[Presidio](http://info.presidio.com/microsoft-azure-expressroute)** | Észak-Amerika |
| **[sol-tec](http://www.sol-tec.com/Technologies)** | Európa |
| **[Vigilant.IT](https://vigilant.it/expressroute)** | Ausztrália |


## <a name="next-steps"></a>Következő lépések
* ExpressRoute kapcsolatos további információkért lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).
* Ellenőrizze, hogy minden előfeltétel teljesül-e. Lásd: [ExpressRoute-előfeltételek](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Helyek térképe"
