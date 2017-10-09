---
title: "aaaAbout VPN-eszközök a létesítmények közötti Azure-kapcsolatok |} Microsoft Docs"
description: "Ez a cikk a létesítmények közötti S2S VPN Gateway-kapcsolatokhoz használt VPN-eszközöket és IPsec paramétereket ismerteti. Hivatkozásokkal tooconfiguration utasítások és a minták."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager, azure-service-management
ms.assetid: ba449333-2716-4b7f-9889-ecc521e4d616
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/14/2017
ms.author: yushwang;cherylmc
ms.openlocfilehash: 8b84afbf93d807342ecd56ab369d5909a13343e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="about-vpn-devices-and-ipsecike-parameters-for-site-to-site-vpn-gateway-connections"></a>Információk a helyek közötti VPN Gateway-kapcsolatok VPN-eszközeinek IPsec/IKE-paramétereiről

A VPN-eszköz szükség tooconfigure segítségével a VPN-átjáró webhelyek (közötti S2S) létesítmények közötti VPN-kapcsolat. Pont-pont kapcsolatok használt toocreate hibrid megoldás, vagy tetszés szerinti biztonságos kapcsolat a helyszíni hálózatokhoz és a virtuális hálózatok között. Ez a cikk ellenőrzött VPN-eszközök listáját, valamint a VPN-átjárók IPsec-/IKE-paramétereinek listáját tartalmazza.

> [!IMPORTANT]
> Ha problémák a helyszíni VPN-eszközök és a VPN-átjárók között tapasztal, tekintse meg a túl[ismert eszköz kompatibilitási problémák](#known).
>
>

### <a name="items-toonote-when-viewing-hello-tables"></a>Elemek toonote hello táblák megtekintésekor:

* Az Azure VPN Gateway esetében terminológiai változás történt. Csak a hello neve megváltozott. A funkció nem változik.
  * Statikus útválasztás = Házirendalapú
  * Dinamikus útválasztás = Útvonalalapú
* HighPerformance VPN gateway és az átjáró RouteBased VPN specifikációk vannak hello azonos, hacsak máshogy nincs jelezve. Például érvényesítve hello VPN-eszközök, amelyek kompatibilisek a RouteBased VPN-átjárók kompatibilisek is hello HighPerformance VPN-átjáró.

## <a name="devicetable"></a>Ellenőrzött VPN-eszközök és eszközkonfigurációs útmutatók

> [!NOTE]
> Helyek közötti kapcsolat konfigurálásakor a VPN-eszköz számára egy nyilvános IPv4 IP-címre van szükség.
>

Eszközszállítói partnereinkkel különböző standard VPN-eszközöket ellenőriztünk. Hello eszközeiről a hello eszközcsaládok hello a következő listában a VPN-átjárók együtt kell működnie. Lásd: [kapcsolatos VPN-átjáró beállítások](vpn-gateway-about-vpn-gateway-settings.md#vpntype) toounderstand hello VPN írja be a hello tooconfigure kívánt VPN-átjáró megoldás (PolicyBased vagy RouteBased) használható.

toohelp a VPN-eszköz beállításához tekintse meg a megfelelő tooappropriate eszköz termékcsalád toohello hivatkozásokat. hello hivatkozások tooconfiguration útmutatást legjobb alapon. A VPN-eszközök támogatásával kapcsolatban lépjen kapcsolatba az eszköze gyártójával.

|**Szállító**          |**Eszközcsalád**     |**Operációs rendszer minimális verziója** |**Házirendalapú konfigurációs utasítások** |**Útválasztó-alapú konfigurációs utasítások** |
| ---                | ---                  | ---                   | ---            | ---           |
| A10 Networks, Inc. |Thunder CFW           |ACOS 4.1.1             |Nem kompatibilis  |[Konfigurációs útmutató](https://www.a10networks.com/resources/deployment-guides/a10-thunder-cfw-ipsec-vpn-interoperability-azure-vpn-gateways)|
| Allied Telesis     |AR sorozatú VPN-útválasztók |2.9.2                  |Hamarosan elérhető     |Nem kompatibilis  |
| Barracuda Networks, Inc. |Barracuda NextGen tűzfal, F sorozat |Házirendalapú: 5.4.3<br>Útvonalalapú: 6.2.0 |[Konfigurációs útmutató](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) |[Konfigurációs útmutató](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW) |
| Barracuda Networks, Inc. |Barracuda NextGen tűzfal, X sorozat |Barracuda tűzfal, 6.5-ös verzió |[Konfigurációs útmutató](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) |Nem kompatibilis |
| Brocade            |Vyatta 5400 vRouter   |Virtual Router 6.6R3 GA|[Konfigurációs útmutató](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html) |Nem kompatibilis |
| Ellenőrzőpont |Biztonsági átjáró |R77.30 |[Konfigurációs útmutató](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |[Konfigurációs útmutató](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco              |ASA       |8.3 |[Konfigurációs minták](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASA) |Nem kompatibilis |
| Cisco |ASR |Házirendalapú: IOS 15.1<br>Útvonalalapú: IOS 15.2 |[Konfigurációs minták](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR) |[Konfigurációs minták](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR) |
| Cisco |ISR |Házirendalapú: IOS 15.0<br>Útvonalalapú*: IOS 15.1 |[Konfigurációs minták](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR) |[Konfigurációs minták*](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR) |
| Citrix |NetScaler MPX, SDX, VPX |10.1-es vagy újabb verzió |[Konfigurációs útmutató](https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html) |Nem kompatibilis |
| F5 |BIG-IP sorozat |12.0 |[Konfigurációs útmutató](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip) |[Konfigurációs útmutató](https://devcentral.f5.com/articles/big-ip-to-azure-dynamic-ipsec-tunneling) |
| Fortinet |FortiGate |FortiOS 5.4.2 |  |[Konfigurációs útmutató](http://cookbook.fortinet.com/ipsec-vpn-microsoft-azure-54) |
| Internet Initiative Japan (IIJ) |SEIL sorozat |SEIL/X 4.60<br>SEIL/B1 4.60<br>SEIL/x86 3.20 |[Konfigurációs útmutató](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf) |Nem kompatibilis |
| Juniper |SRX |Házirendalapú: JunOS 10.2<br>Útvonalalapú: JunOS 11.4 |[Konfigurációs minták](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX) |[Konfigurációs minták](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX) |
| Juniper |J sorozat |Házirendalapú: JunOS 10.4r9<br>Útvonalalapú: JunOS 11.4 |[Konfigurációs minták](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries) |[Konfigurációs minták](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries) |
| Juniper |ISG |ScreenOS 6.3 |[Konfigurációs minták](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG) |[Konfigurációs minták](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG) |
| Juniper |SSG |ScreenOS 6.2 |[Konfigurációs minták](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG) |[Konfigurációs minták](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG) |
| Microsoft |Útválasztás és távelérés szolgáltatás |Windows Server 2012 |Nem kompatibilis |[Konfigurációs minták](http://go.microsoft.com/fwlink/p/?LinkId=717761) |
| Open Systems AG |Mission Control biztonsági átjáró |N/A |[Konfigurációs útmutató](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |Nem kompatibilis |
| Openswan |Openswan |2.6.32 |(Hamarosan elérhető) |Nem kompatibilis |
| Palo Alto Networks |Az összes PAN-OS rendszert futtató eszköz |PAN-OS<br>Házirendalapú: 6.1.5 vagy újabb<br>Útvonalalapú: 7.1.4 |[Konfigurációs útmutató](https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065) |[Konfigurációs útmutató](https://live.paloaltonetworks.com/t5/Integration-Articles/Configuring-IKEv2-VPN-for-Microsoft-Azure-Environment/ta-p/60340) |
| SonicWall |TZ sorozat, NSA sorozat<br>SuperMassive sorozat<br>E-Class NSA sorozat |SonicOS 5.8.x<br>SonicOS 5.9.x<br>SonicOS 6.x |[Beállítási útmutató a SonicOS 6.2 rendszerhez](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646)<br>[Beállítási útmutató a SonicOS 5.9 rendszerhez](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850) |[Beállítási útmutató a SonicOS 6.2 rendszerhez](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646)<br>[Beállítási útmutató a SonicOS 5.9 rendszerhez](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850) |
| WatchGuard |Összes |Fireware XTM<br> Házirendalapú: v11.11.x<br>Útvonalalapú: v11.12.x |[Konfigurációs útmutató](http://watchguardsupport.force.com/publicKB?type=KBArticle&SFDCID=kA2F00000000LI7KAM&lang=en_US) |[Konfigurációs útmutató](http://watchguardsupport.force.com/publicKB?type=KBArticle&SFDCID=kA22A000000XZogSAG&lang=en_US)|

(*) Az ISR 7200 sorozatba tartozó útválasztók csak a házirendalapú VPN-eket támogatják.

## <a name="additionaldevices"></a>Nem ellenőrzött VPN-eszközök

Ha nem látja az eszközt, hello érvényesítve VPN eszközök táblázatban, az eszköz még használhatja a pont-pont kapcsolat. További támogatásért és konfigurációs útmutatásért lépjen kapcsolatba az eszköze gyártójával.

## <a name="editing"></a>Az eszköz konfigurációs mintáinak szerkesztése

A megadott hello VPN eszköz konfigurációs minta letöltése után tooreplace lesz szüksége néhány hello értékek tooreflect hello a környezetre vonatkozó beállításokat.

### <a name="tooedit-a-sample"></a>egy minta tooedit:

1. Nyissa meg a Jegyzettömbben hello minta.
2. Keresés és csere összes <*szöveg*> karakterláncok, amelyek a projektjükhöz tooyour környezet hello értékekkel. Lehet, hogy tooinclude < és >. Ha olyan nevet ad meg, választja hello nevének egyedinek kell lennie. Ha egy parancs nem működik, tekintse meg az eszköz gyártói dokumentációját.

| **Szövegminta** | **Módosítsa a következőre:** |
| --- | --- |
| &lt;RP_OnPremisesNetwork&gt; |Az objektum választott neve. Például: myOnPremisesNetwork |
| &lt;RP_AzureNetwork&gt; |Az objektum választott neve. Például: myAzureNetwork |
| &lt;RP_AccessList&gt; |Az objektum választott neve. Például: myAzureAccessList |
| &lt;RP_IPSecTransformSet&gt; |Az objektum választott neve. Például: myIPSecTransformSet |
| &lt;RP_IPSecCryptoMap&gt; |Az objektum választott neve. Például: myIPSecCryptoMap |
| &lt;SP_AzureNetworkIpRange&gt; |Adja meg a tartományt. Példa: 192.168.0.0 |
| &lt;SP_AzureNetworkSubnetMask&gt; |Adja meg az alhálózati maszkot. Példa: 255.255.0.0 |
| &lt;SP_OnPremisesNetworkIpRange&gt; |Adja meg a helyszíni tartományt. Példa: 10.2.1.0 |
| &lt;SP_OnPremisesNetworkSubnetMask&gt; |Adja meg a helyszíni alhálózati maszkot. Példa: 255.255.255.0 |
| &lt;SP_AzureGatewayIpAddress&gt; |Ezen információk adott tooyour a virtuális hálózaton, és a kezelési portál hello található, **átjáró IP-cím**. |
| &lt;SP_PresharedKey&gt; |Ezt az információt adott tooyour virtuális hálózat, és a felügyeleti portálon kezelheti kulcsként hello található. |

## <a name="ipsec"></a>IPsec/IKE-paraméterek

> [!NOTE]
> Bár hello a következő táblázatban felsorolt hello értékek által támogatott hello VPN-átjáró, jelenleg nincs az Ön mechanizmus toospecify, vagy válasszon egyedi kombinációja algoritmusok vagy paraméterek hello VPN-átjáró. Meg kell adnia a hello a helyszíni VPN-eszköz korlátozásokat. Ezenfelül az **MSS** korlátozását **1350-re** kell állítani.
> 
>

A következő táblák hello:

* SA = Biztonsági társítás
* Az IKE 1. fázis másik elnevezése: „Main Mode” (Elsődleges mód)
* Az IKE 2. fázis másik elnevezése: „Quick Mode” (Gyors mód)

### <a name="ike-phase-1-main-mode-parameters"></a>Az IKE 1. fázis (Elsődleges mód) paraméterei

| **Tulajdonság**          |**Házirendalapú**    | **Útvonalalapú**    |
| ---                   | ---               | ---               |
| IKE verziószám           |IKEv1              |IKEv2              |
| Diffie-Hellman Group  |2. csoport (1024 bites) |2. csoport (1024 bites) |
| Hitelesítési módszer |Előre megosztott kulcs     |Előre megosztott kulcs     |
| Titkosító és kivonatoló algoritmus |1. AES256, SHA256<br>2. AES256, SHA1<br>3. AES128, SHA1<br>4. 3DES, SHA1 |1. AES256, SHA1<br>2. AES256, SHA256<br>3. AES128, SHA1<br>4. AES128, SHA256<br>5. 3DES, SHA1<br>6. 3DES, SHA256 |
| SA élettartama           |28 800 másodperc     |28 800 másodperc     |

### <a name="ike-phase-2-quick-mode-parameters"></a>Az IKE 2. fázis (Gyors mód) paraméterei

| **Tulajdonság**                  |**Házirendalapú**| **Útvonalalapú**                              |
| ---                           | ---           | ---                                         |
| IKE verziószám                   |IKEv1          |IKEv2                                        |
| Titkosító és kivonatoló algoritmus |1. AES256, SHA256<br>2. AES256, SHA1<br>3. AES128, SHA1<br>4. 3DES, SHA1 |[Útvonalalapú QM SA ajánlatok](#RouteBasedOffers) |
| SA élettartama (Idő)            |3 600 másodperc  |27 000 másodperc                                |
| SA élettartama (bájt)           |102 400 000 kB | -                                           |
| Sérülés utáni titkosságvédelem (PFS) |Nem             |[Útvonalalapú QM SA ajánlatok](#RouteBasedOffers) |
| Kapcsolat megszakadásának észlelése (DPD)     |Nem támogatott  |Támogatott                                    |


### <a name ="RouteBasedOffers"></a>Útvonalalapú VPN IPsec biztonsági társítás (IKE – gyors mód SA) ajánlatai

hello következő táblázat felsorolja az IPsec biztonsági Társítás (IKE gyorsmódú) ajánlatokat. Ajánlatok akkor felsorolt hello sorrend figyelembe vételével adott hello ajánlat jelenik meg, vagy elfogadva.

#### <a name="azure-gateway-as-initiator"></a>Azure-átjáró, mint kezdeményező

|-  |**Titkosítás**|**Hitelesítés**|**PFS-csoport**|
|---| ---          |---               |---          |
| 1 |GCM AES256    |GCM (AES256)      |None         |
| 2 |AES256        |SHA1              |None         |
| 3 |3DES          |SHA1              |None         |
| 4 |AES256        |SHA256            |None         |
| 5 |AES128        |SHA1              |None         |
| 6 |3DES          |SHA256            |None         |

#### <a name="azure-gateway-as-responder"></a>Azure-átjáró, mint válaszadó

|-  |**Titkosítás**|**Hitelesítés**|**PFS-csoport**|
|---| ---          | ---              |---          |
| 1 |GCM AES256    |GCM (AES256)      |None         |
| 2 |AES256        |SHA1              |None         |
| 3 |3DES          |SHA1              |None         |
| 4 |AES256        |SHA256            |None         |
| 5 |AES128        |SHA1              |None         |
| 6 |3DES          |SHA256            |None         |
| 7 |DES           |SHA1              |None         |
| 8 |AES256        |SHA1              |1            |
| 9 |AES256        |SHA1              |2            |
| 10|AES256        |SHA1              |14           |
| 11|AES128        |SHA1              |1            |
| 12|AES128        |SHA1              |2            |
| 13|AES128        |SHA1              |14           |
| 14|3DES          |SHA1              |1            |
| 15|3DES          |SHA1              |2            |
| 16|3DES          |SHA256            |2            |
| 17|AES256        |SHA256            |1            |
| 18|AES256        |SHA256            |2            |
| 19|AES256        |SHA256            |14           |
| 20|AES256        |SHA1              |24           |
| 21|AES256        |SHA256            |24           |
| 22|AES128        |SHA256            |None         |
| 23|AES128        |SHA256            |1            |
| 24|AES128        |SHA256            |2            |
| 25|AES128        |SHA256            |14           |
| 26|3DES          |SHA1              |14           |

* Az IPsec ESP NULL titkosítás útvonalalapú és nagy teljesítményű VPN-átjárók segítségével adható meg. NULL alapú titkosítás nem nyújt védelmet toodata az átvitel során, és csak kell használni, amikor maximális átviteli sebesség és a minimális késés szükség. Ügyfelek előfordulhat, hogy ezzel toouse VNet – VNet kommunikációs helyzetek, vagy amikor titkosítási máshol hello megoldásban végzi.
* A létesítmények közötti kapcsolathoz hello interneten keresztül hello az alapértelmezett beállításokkal Azure VPN gateway titkosítási és kivonatoló algoritmusok hello táblázatokban tooensure biztonsági a kritikus fontosságú kommunikációhoz a fent felsorolt.

## <a name="known"></a>Ismert eszközkompatibilitási problémák

> [!IMPORTANT]
> A rendszer hello ismert kompatibilitási problémákkal külső VPN-eszközök és az Azure VPN-átjárók között. hello Azure csapata tooaddress hello az itt felsorolt problémák hello szállítókkal aktívan. Miután hello problémák megoldott, ezen a lapon hello legfrissebb információkkal fog frissülni. Kérjük, időnként látogasson vissza ide.
>
>

### <a name="feb-16-2017"></a>2017. február 16.

**Palo Alto hálózatok eszközök előzetes verzió too7.1.4** Azure útvonalalapú VPN: Ha PÁSZTÁZÁS-OS verziója előzetes too7.1.4 VPN-eszközök a Palo Alto hálózatok használ, és a kapcsolat tapasztalt problémák tooAzure útvonalalapú VPN-átjárók, hajtsa végre az alábbi lépésekkel hello:

1. Ellenőrizze a Palo Alto hálózatok eszköz hello belső vezérlőprogram verzióját. Ha a PÁSZTÁZÁS-OS verziója régebbi, mint 7.1.4, frissítse az too7.1.4.
2. Hello Palo Alto hálózatok eszközön módosítása hello fázis 2 SA (vagy gyors biztonsági Társítás) élettartamát too28, 800 másodperc (8 óra) Ha toohello Azure VPN gatewayhez csatlakozó.
3. Ha továbbra is problémák, nyissa meg egy támogatási kérést hello Azure-portálon.
