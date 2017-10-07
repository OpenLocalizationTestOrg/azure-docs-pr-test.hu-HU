---
title: "a Site Recovery-VMware tooAzure replikációhoz hálózati aaaPlan |} Microsoft Docs"
description: "Ez a cikk ismerteti, amelyek az alaphálózati topológia tervezése szükséges, az Azure Site Recovery Azure virtuális gépek replikálása esetén"
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/01/2017
ms.author: sujayt
ms.openlocfilehash: e4036351ca707bd4966cf2a855d4a162f88153e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-3-plan-networking-for-azure-vm-replication"></a>3. lépés: Az Azure virtuális gép replikációs hálózat tervezése

Miután ellenőrizte, hogy hello [telepítésének előfeltételei](azure-to-azure-walkthrough-prerequisites.md), olvassa el a hálózati szempontjai replikálódik, és az Azure virtuális gépek (VM) helyreállítani egy Azure-régiót tooanother cikk toounderstand hello hello használata Az Azure Site Recovery szolgáltatásban. 

- Hello cikk befejezése után kell egyértelműen megértése hogyan mentése kimenő tooset elérhető Azure virtuális gépekhez, szeretné, hogy tooreplicate, és hogyan tooconnect a helyszíni hely toothose virtuális gépeket.
- Ez a cikk hello alján a megjegyzéseket, vagy kérdései vannak a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

>[!NOTE]
> Az Azure Site Recovery szolgáltatással Virtuálisgép-replikációt jelenleg előzetes verzió.



## <a name="network-overview"></a>Hálózat áttekintése

Általában egy Azure virtuális hálózatot/alhálózatot az Azure virtuális gépek találhatók, és van-e a helyszíni hely tooAzure Azure expressroute-on vagy a VPN-kapcsolat használata a kapcsolat.

Hálózatok általában védett tűzfalakat használ, és/vagy a hálózati biztonsági csoportokkal (NSG-k). Tűzfalak használható URL-alapú IP-alapú listák, toocontrol hálózati kapcsolatot. Az NSG-k használata IP-címtartomány alapú szabályok. 


A Site Recovery toowork várt kell toomake a kimenő hálózati kapcsolat, a virtuális gépek, amelyet az tooreplicate néhány módosítást. A Site Recovery használatát egy hitelesítési proxy toocontrol hálózati kapcsolat nem támogatja. Ha egy hitelesítési proxy, nem engedélyezhető. 


hello következő ábra szemlélteti a tipikus környezet az Azure virtuális gépeken futó alkalmazás.

![ügyfél-környezet](./media/azure-to-azure-walkthrough-network/source-environment.png)

**Azure virtuális környezetben**

Is szükség lehet egy kapcsolat tooAzure állítsa be a helyszíni hely Azure expressroute-on vagy VPN-kapcsolat használatával. 

![ügyfél-környezet](./media/azure-to-azure-walkthrough-network/source-environment-expressroute.png)

**A helyi kapcsolat tooAzure**



## <a name="outbound-connectivity-for-urls"></a>Kimenő kapcsolat URL-címek esetén

Ha egy URL-alapú tűzfal proxy toocontrol kimenő kapcsolatot használ, ellenőrizze, hogy a Site Recovery által használt URL-

**URL-CÍME** | **Részletek**  
--- | ---
*.blob.core.windows.net | Lehetővé teszi, hogy a hello VM, toohello gyorsítótár tárfiók hello forrás régióban írt adatok toobe.
login.microsoftonline.com | Hitelesítési és engedélyezési tooSite biztosít a helyreállítási szolgáltatás URL-címeit.
*.hypervrecoverymanager.windowsazure.com | Lehetővé teszi, hogy a kommunikáció a hello hello VM a Site Recovery szolgáltatásban.
*. servicebus.windows.net | Szükséges, hogy hello Site Recovery figyelése és diagnosztikai adatok hello VM beírhatók.

## <a name="outbound-connectivity-for-ip-address-ranges"></a>Kimenő kapcsolódás az IP-címtartományok

- Automatikusan létrehozhat hello szükséges szabályok hello NSG letöltése és futtatása [ezt a parancsfájlt](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).
- Azt javasoljuk, hogy egy NSG-teszt szükséges hello NSG-szabályok létrehozása, és ellenőrizze, hogy nem jelez problémákat, egy termelési NSG hello szabályok létrehozása előtt.
- NSG-szabályok száma toocreate szükséges hello győződjön meg arról, hogy az előfizetés szerepel az engedélyezési listán. Forduljon a támogatási szolgálathoz tooincrease hello NSG szabály vonatkozó korlát az előfizetésben.
Ha bármely IP-alapú tűzfal proxy vagy az NSG-szabályok toocontrol kimenő kapcsolatot használ, hello következő IP-címtartományok kell toobe szerepel az engedélyezési listán, attól függően, hogy hello forrása és célja helyek hello virtuális gépek:

    - Az összes IP-címtartományokat, amelyek megfelelnek a toohello forráshelyeként. Letöltési hello [tartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653).) Engedélyezett, így az adatok is toohello gyorsítótár tárfiók a virtuális gép hello.
    - Az összes IP-címtartományokat, amelyek megfelelnek a tooOffice 365 [hitelesítés és identitás IP V4 végpontok](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity). Ha új IP-címek hozzáadják tooOffice 365 IP-címtartományok, akkor toocreate új NSG-szabályok.
-     Hely helyreállítási szolgáltatási végpont IP-címek ([rendelkezésre álló XML-fájlban](https://aka.ms/site-recovery-public-ips)), a célhely függ: 

   **Célhely** | **Site Recovery szolgáltatás IP-címek** |  **A Site Recovery IP figyelése**
   --- | --- | ---
   Kelet-Ázsia | 52.175.17.132</br>40.83.121.61 | 13.94.47.61
   Délkelet-Ázsia | 52.187.58.193</br>52.187.169.104 | 13.76.179.223
   Közép-India | 52.172.187.37</br>52.172.157.193 | 104.211.98.185
   Dél-India | 52.172.46.220</br>52.172.13.124 | 104.211.224.190
   USA északi középső régiója | 23.96.195.247</br>23.96.217.22 | 168.62.249.226
   Észak-Európa | 40.69.212.238</br>13.74.36.46 | 52.169.18.8
   Nyugat-Európa | 52.166.13.64</br>52.166.6.245 | 40.68.93.145
   USA keleti régiója | 13.82.88.226</br>40.71.38.173 | 104.45.147.24
   USA nyugati régiója | 40.83.179.48</br>13.91.45.163 | 104.40.26.199
   USA déli középső régiója | 13.84.148.14</br>13.84.172.239 | 104.210.146.250
   USA középső régiója | 40.69.144.231</br>40.69.167.116 | 52.165.34.144
   USA 2. keleti régiója | 52.184.158.163</br>52.225.216.31 | 40.79.44.59
   Kelet-Japán | 52.185.150.140</br>13.78.87.185 | 138.91.1.105
   Nyugat-Japán | 52.175.146.69</br>52.175.145.200 | 138.91.17.38
   Dél-Brazília | 191.234.185.172</br>104.41.62.15 | 23.97.97.36
   Kelet-Ausztrália | 104.210.113.114</br>40.126.226.199 | 191.239.64.144
   Délkelet-Ausztrália | 13.70.159.158</br>13.73.114.68 | 191.239.160.45
   Közép-Kanada | 52.228.36.192</br>52.228.39.52 | 40.85.226.62
   Kelet-Kanada | 52.229.125.98</br>52.229.126.170 | 40.86.225.142
   USA nyugati középső régiója | 52.161.20.168</br>13.78.230.131 | 13.78.149.209
   USA nyugati régiója, 2. | 52.183.45.166</br>52.175.207.234 | 13.66.228.204
   Az Egyesült Királyság nyugati régiója | 51.141.3.203</br>51.140.226.176 | 51.141.14.113
   Az Egyesült Királyság déli régiója | 51.140.43.158</br>51.140.29.146 | 51.140.189.52

## <a name="example-nsg-configuration"></a>Példa NSG-konfiguráció

Ez a szakasz bemutatja, hogyan tooconfigure NSG-szabályok, úgy, hogy a replikáció működik, ha egy virtuális Gépet. NSG-szabályok toocontrol kimenő kapcsolat használata, "A HTTPS engedélyezése kimenő" szabályok használata az összes szükséges hello IP-címtartományok.

Ebben a példában hello VM forráshely az "USA keleti régiója". hello replikációs célhelye USA középső RÉGIÓJA.


### <a name="example"></a>Példa

#### <a name="east-us"></a>USA keleti régiója

1. Létrehozhat szabályokat, amelyek megfelelnek túl[keleti USA IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653). Erre azért szükség, így az adatok is toohello gyorsítótár tárfiók a virtuális gép hello.
2. Létrehozhat szabályokat, amelyek megfelelnek a tooOffice 365 összes IP-címtartományokhoz [hitelesítés és identitás IP V4 végpontok](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).
3. Hozzon létre szabályokat, amelyek megfelelnek a toohello célhelye:

   **Hely** | **Site Recovery szolgáltatás IP-címek** |  **A Site Recovery IP figyelése**
    --- | --- | ---
   USA középső régiója | 40.69.144.231</br>40.69.167.116 | 52.165.34.144

#### <a name="central-us"></a>USA középső régiója

Ezek a szabályok szükség, így használható a replikáció funkció hello cél régió toohello forrás régióban, a feladatátvételt követően.

1. Létrehozhat szabályokat, amelyek megfelelnek túl[központi USA IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653).
2. Létrehozhat szabályokat, amelyek megfelelnek a tooOffice 365 összes IP-címtartományokhoz [hitelesítés és identitás IP V4 végpontok](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).
3. Hozzon létre szabályokat, amelyek megfelelnek a toohello Forráshely:

   **Hely** | **Site Recovery szolgáltatás IP-címek** |  **A Site Recovery IP figyelése**
    --- | --- | ---
   USA keleti régiója | 13.82.88.226</br>40.71.38.173 | 104.45.147.24


## <a name="existing-on-premises-connection"></a>Meglévő helyszíni kapcsolat

Ha egy a helyszíni hely és a forráshelyét hello Azure ExpressRoute- vagy VPN-kapcsolatot, kövesse az alábbiakat:

**Konfigurálás** | **Részletek**
--- | ---
**A kényszerített bújtatás** | Általában az alapértelmezett útvonalat (0.0.0.0/0) kényszeríti a kimenő internet forgalom tooflow hello helyszíni helyeken keresztül. Nem ajánlott ennek. Replikációs forgalom és a Site Recovery kommunikációs Azure belül kell maradnak.<br/><br/> Megoldás, felhasználó által definiált útvonalak (udr-EK) hozzáadása [ezek IP-címtartományok](#outbound-connectivity-for-azure-site-recovery-ip-ranges), így hello forgalom nem halad a helyszínen.
**Kapcsolatok** | Ha alkalmazásokat kell tooconnect tooon helyszíni gépeket, vagy helyszíni ügyfeleken toohello app protokollon keresztül kapcsolódhatnak a helyszíni VPN/ExpressRoute keresztül, ellenőrizze, hogy legalább egy [pont-pont kapcsolat](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)közötti hello cél Azure-régió, és hello helyszíni hely.<br/><br/> Ha a forgalom kötetek hello cél Azure-régió, és hello helyszíni hely közötti magas, létrehozhat egy másik [ExpressRoute-kapcsolat](../expressroute/expressroute-introduction.md), hello cél régió és a helyszíni között.<br/><br/> Ha azt szeretné, IP-címek tooretain virtuális gépek a feladatátvételt követően, ne hello cél régió hely-hely vagy ExpressRoute-kapcsolat megszakad. Ez biztosítja, hogy nincs tartomány ütközés hello forrás és cél IP-címtartományok között van.
**ExpressRoute** | ExpressRoute-kapcsolatcsoportot létrehozni hello a forrás- és régiókban.<br/><br/> Hozzon létre egy kapcsolatot hello Forráshálózat és hello ExpressRoute-kapcsolatcsoport között, valamint hello célhálózat és hello kapcsolatcsoport között.<br/><br/> Azt javasoljuk, hogy a forrás és cél régióban különböző IP-címtartományok használja. hello kapcsolatcsoport nem lesz képes tooconnect toomore ugyanaz a hello hálózatok egy Azure, mint az IP-címtartományok: hello ugyanannyi időt vesz igénybe.<br/><br/> Virtuális hálózatok ugyanazon IP-tartományok mindkét régió, és majd hozza létre az ExpressRoute-Kapcsolatcsoportok mindkét régiókban hello létrehozhat. Feladatátvétel hello kapcsolat bontása hello forrás virtuális hálózat, valamint elérheti a hello áramkör hello cél virtuális hálózatban.<br/><br/> Ha elsődleges régió hello teljesen le, hello művelet megszakítására sikertelen lehet. Ebben az esetben hello cél virtuális hálózat nem jelenik meg az ExpressRoute-kapcsolatot.
**Az ExpressRoute-támogatás** | Hello hozhatja létre kapcsolatok geopolitikai ugyanabban a régióban.<br/><br/> más geopolitikai régióban áramkör toocreate, prémium szintű Azure ExpressRoute kell.<br/><br/> Prémium szintű hello költsége növekményes. Ha már használja, akkor további költség nélkül.<br/><br/> További információ [helyek](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) és [árképzési](https://azure.microsoft.com/pricing/details/expressroute/).



## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[4. lépés: a tároló létrehozása](azure-to-azure-walkthrough-vault.md)

