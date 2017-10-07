---
title: "az Azure VPN Gatewayek magas rendelkezésre állású konfiguráció aaaOverview |} Microsoft Docs"
description: "Ez a cikk áttekintést nyújt a magas rendelkezésre állású konfigurációs lehetőségekről az Azure-alapú VPN-átjárók használatával."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: 
ms.assetid: a8bfc955-de49-4172-95ac-5257e262d7ea
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2016
ms.author: yushwang
ms.openlocfilehash: 316293b9ac79645bf9bb9e89fbc4aa8f3eacd209
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="highly-available-cross-premises-and-vnet-to-vnet-connectivity"></a>Magas rendelkezésre állású kapcsolatok létesítmények, illetve virtuális hálózatok között
Ez a cikk áttekintést nyújt a létesítmények közötti és a virtuális hálózatok közötti kapcsolatokra vonatkozó magas rendelkezésre állású konfigurációs lehetőségekről az Azure-alapú VPN-átjárók használatával.

## <a name = "activestandby"></a>Tudnivalók az Azure VPN gateway redundancia
Minden egyes Azure-alapú VPN-átjáró két példányból áll, amelyek aktív-készenléti konfigurációban vannak. Tervezett karbantartások és nem tervezett megszűnésének, amely toohello aktív példány történik hello készenléti állapotban lévő példány lesz automatikusan átveszi (feladatátvétel), majd folytassa hello S2S VPN vagy a VNet – VNet kapcsolatokhoz. hello kell váltania rövid megszakítás miatt. A tervezett karbantartáshoz hello kapcsolat too15 10 másodpercen belül állítható vissza. Nem tervezett hibák hello kapcsolat helyreállítása hosszabb, 1 perces too1 kapcsolatos lesz, és fél percnél hello legrosszabb esetben. P2S VPN ügyfél kapcsolatok toohello átjáró hello P2S-kapcsolatok le lesz választva, és hello felhasználóknak tudniuk kell, az ügyfélgépek hello tooreconnect.

![Aktív-készenléti](./media/vpn-gateway-highlyavailable/active-standby.png)

## <a name="highly-available-cross-premises-connectivity"></a>Létesítmények közötti magas rendelkezésre állású kapcsolatok
tooprovide jobb rendelkezésre állásának a közötti kapcsolatok helyszíni, nincsenek elérhető beállítások közül néhány:

* Több helyszíni VPN-eszköz
* Aktív-aktív Azure-alapú VPN-átjáró
* A kettő kombinációja

### <a name = "activeactiveonprem"></a>Több helyszíni VPN-eszközök
Több VPN-eszközök használhatja a helyszíni hálózati tooconnect tooyour Azure VPN-átjárót, ahogy az ábra a következő hello:

![Több helyszíni VPN](./media/vpn-gateway-highlyavailable/multiple-onprem-vpns.png)

Ez a konfiguráció biztosítja a hello több aktív bújtatási ugyanazon Azure VPN gateway tooyour a helyszíni eszközök hello ugyanazon a helyen. Van néhány követelmény és megkötés:

1. Szükséges toocreate több S2S VPN-kapcsolatot a VPN-eszközök tooAzure. Ha több VPN-eszközök hello azonos helyszíni hálózati tooAzure, minden egyes VPN-eszköz toocreate egy helyi hálózati átjáró és egy kapcsolatot az Azure VPN gateway toohello helyi hálózati átjáró szükséges.
2. hello helyi hálózati átjáró tooyour VPN-eszközök megfelelő hello "GatewayIpAddress" tulajdonság egyedi nyilvános IP-címeket kell rendelkeznie.
3. Ehhez a konfigurációhoz BGP szükséges. Minden egyes VPN-eszközhöz képviselő helyi hálózati átjáró rendelkeznie kell egy egyedi BGP társ IP-cím hello "BgpPeerIpAddress" tulajdonságban megadott.
4. hello címelőtagja tulajdonságmező az egyes helyi hálózati átjáró nem lehet átfedésben. Adja meg "BgpPeerIpAddress" hello /32 a hello címelőtagja mezőben, például 10.200.200.254/32 CIDR formátumban.
5. Használjon BGP tooadvertise hello azonos előtagok hello az ugyanabban a helyi hálózati előtagok tooyour Azure VPN gateway, és hello forgalmat továbbítja ezeket alagutakon keresztül egyszerre.
6. Minden kapcsolat használatát alagutakat az Azure VPN gateway, 10, a Basic és Standard termékváltozat, a maximális számú hello számít HighPerformance SKU esetében pedig 30. 

Ebben a konfigurációban hello Azure VPN gateway még mindig aktív-készenléti állapotban lévő módban van, így az azonos feladatátvételt hello és rövid megszakítás továbbra is fog történni, leírtak [fent](#activestandby). Ez a beállítás viszont védelmet nyújt a hibák vagy megszakítások ellen a helyszíni hálózat és a VPN-eszközök esetében.

### <a name="active-active-azure-vpn-gateway"></a>Aktív-aktív Azure-alapú VPN-átjáró
Az Azure VPN-átjáró mostantól aktív-aktív konfigurációban, ha mindkét virtuális gépek főkiszolgálójával S2S VPN bújtatja tooyour a helyszíni VPN-eszközön, mint a következő látható hello hello átjáró példánya ábra hozhat létre:

![Aktív-aktív](./media/vpn-gateway-highlyavailable/active-active.png)

Ebben a konfigurációban minden Azure átjárópéldány lesz egy egyedi nyilvános IP-címet, és minden egyes létrehozza az IPsec/IKE S2S VPN-je alagút tooyour a helyszíni VPN-eszköz szerepel a helyi hálózati átjáró és a kapcsolat. Ügyeljen arra, hogy mindkét VPN-alagutat ténylegesen hello része ugyanazt a kapcsolatot. Hoz továbbra is tooconfigure kell a helyszíni VPN-eszköz tooaccept vagy két S2S VPN-je alagutak toothose két Azure VPN átjáró nyilvános IP-címeket létrehozni.

Hello Azure átjárópéldányokról aktív-aktív konfigurációs találhatók, az Azure-beli virtuális hálózat tooyour hello forgalmát a helyszíni hálózaton keresztül történik mindkét alagutak egyszerre, még akkor is, ha a helyszíni VPN-eszköz előfordulhat, hogy alkalmazást egy alagúton keresztül hello más. Vegye figyelembe, ha az adott TCP vagy UDP-adatfolyam a rendszer mindig haladnak át hello hello azonos alagúton vagy az elérési út, kivéve, ha a karbantartási események hello példányok egyik történik.

Ha egy előre tervezett karbantartások vagy váratlan esemény történik a tooone átjárópéldány, az adott példány tooyour hello IPsec-alagutat a helyszíni VPN-eszköz le lesz választva. hello a VPN-eszközök megfelelő útvonalak kell távolítható el és visszavont automatikusan, hogy hello forgalmat fog kapcsolni az toohello keresztül más aktív IPsec-alagutat. Hello Azure oldalon, a hello kell váltania automatikusan elvégzi az érintett hello példány toohello aktív példány.

### <a name="dual-redundancy-active-active-vpn-gateways-for-both-azure-and-on-premises-networks"></a>Kettős redundancia: aktív-aktív VPN-átjárók az Azure és a helyszíni hálózatok számára egyaránt
hello legmegbízhatóbb elem toocombine hello aktív-aktív átjárókat a hálózat és az Azure, a hello alábbi ábrán látható módon.

![Kettős redundancia](./media/vpn-gateway-highlyavailable/dual-redundancy.png)

Itt létrehozása és beállítása hello Azure VPN gateway aktív-aktív konfigurációban, és két helyi hálózati átjáró létrehozása és a két két kapcsolatok a helyszíni VPN-eszközök fent leírt módon. hello eredménye egy teljes rácsot alkotó kapcsolatát 4 IPsec-alagutat az Azure virtuális hálózat és a helyszíni hálózat között.

Minden átjárót és alagutak hello Azure ügyféloldali aktívak, így hello forgalom lesz terjednek közötti összes 4 alagutak egyidejűleg, bár egyes TCP vagy UDP-flow fog újra kövesse hello azonos alagút és nem útvonal a hello Azure oldalán. Annak ellenére, hogy úgy hello forgalom, láthatja valamivel nagyobb átviteli sebesség hello IPsec-alagutakon keresztül, hello elsődleges Ez a konfiguráció célja a magas rendelkezésre állás érdekében. És miatt toohello statisztikai jellegű hello terjedésének, nehéz tooprovide hello mérési a különböző alkalmazás-forgalom feltételek hello összesített teljesítményt befolyásolja.

Ez a topológia két helyi hálózati átjáró és a helyszíni VPN-eszközök a két kapcsolatok toosupport hello két van szükség, és a BGP egy szükséges tooallow hello két kapcsolatok toohello ugyanabban a helyi hálózaton. Ezek a követelmények hello ugyanaz, mint a hello [fent](#activeactiveonprem). 

## <a name="highly-available-vnet-to-vnet-connectivity-through-azure-vpn-gateways"></a>Magas rendelkezésre állású virtuális hálózatok közötti kapcsolatok az Azure VPN Gateway-átjárókon keresztül
hello aktív-aktív azonos konfigurációval is alkalmazhat tooAzure VNet – VNet kapcsolatokhoz. Aktív-aktív VPN-átjárók mindkét virtuális hálózatok létrehozása, és csatlakoztassa őket együtt tooform hello azonos teljes háló 4 alagutat két Vnetek, ahogy hello az alábbi ábra hello között csatlakoztatásához:

![Virtuális hálózatok közötti kapcsolat](./media/vpn-gateway-highlyavailable/vnet-to-vnet.png)

Ez biztosítja, hogy mindig akadnak hello két virtuális hálózatok bármely tervezett karbantartási események, még élvezetesebbé rendelkezésre állásának biztosítása közötti alagutak két. Annak ellenére, hogy hello közötti kapcsolatot nyújthassanak azonos topológia igényel két kapcsolatok, hello VNet – VNet topológia fent látható minden-átjárón csak egy kapcsolat lesz szüksége. Emellett a BGP egy nem kötelező, ha tranzit útválasztást hello VNet – VNet-kapcsolaton keresztül nem szükséges.

## <a name="next-steps"></a>Következő lépések
Lásd: [aktív-aktív VPN-átjárók konfigurálása a létesítmények közötti és VNet – VNet kapcsolatokhoz](vpn-gateway-activeactive-rm-powershell.md) lépéseit tooconfigure aktív-aktív létesítmények közötti és VNet – VNet kapcsolatokhoz.

