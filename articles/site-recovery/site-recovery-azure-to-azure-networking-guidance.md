---
title: "a Site Recovery hálózati útmutató a virtuális gépek replikálása az Azure tooAzure aaaAzure |} Microsoft Docs"
description: "Útmutató az Azure virtuális gépek replikálása a hálózat"
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
ms.date: 05/13/2017
ms.author: sujayt
ms.openlocfilehash: 3a3391b8c3512932d243458fd17d2a2b39248448
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="networking-guidance-for-replicating-azure-virtual-machines"></a>Útmutató az Azure virtuális gépek replikálása a hálózat

>[!NOTE]
> Az Azure virtuális gépek helyreállítási helyreplikálásának jelenleg előzetes verzió.

Ez a cikk részletek hello hálózati útmutatót az Azure Site Recovery, amikor Ön replikálódik, és az Azure virtuális gépek helyreállítása egy régió tartozik tooanother régióban. Az Azure Site Recovery követelményeiről kapcsolatban bővebben lásd: hello [Előfeltételek](site-recovery-prereq.md) cikk.

## <a name="site-recovery-architecture"></a>Site Recovery architektúrájáról

A Site Recovery lehetővé teszi egy egyszerű és egyszerűen elvégezhető tooreplicate futó Azure virtuális gépek tooanother az Azure-régió, hogy a helyre, ha a szüneteltetése hello elsődleges régióban. További információ [Ez a forgatókönyv és a Site Recovery architektúrájáról](site-recovery-azure-to-azure-architecture.md).

## <a name="your-network-infrastructure"></a>A hálózati infrastruktúra

hello következő ábra szemlélteti hello Azure tipikus környezet az Azure virtuális gépeken futó alkalmazások:

![ügyfél-környezet](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Ha az Azure expressroute-on vagy egy helyszíni hálózati tooAzure egy VPN-kapcsolatot használ, akkor a hello környezet néz ki:

![ügyfél-környezet](./media/site-recovery-azure-to-azure-architecture/source-environment-expressroute.png)

Általában az ügyfelek védelme a tűzfalak és/vagy a hálózati biztonsági csoportokkal (NSG-k) használó hálózatokon. hello tűzfalak használható URL-cím vagy IP-alapú engedélyezett hálózati kapcsolat ellenőrzése. Az NSG-k engedélyezési szabály IP-címtartományok toocontrol hálózati kapcsolatot használ.

>[!IMPORTANT]
> Ha egy hitelesített proxykiszolgálót toocontrol hálózati kapcsolat használata esetén nem támogatott, és a Site Recovery nem engedélyezhető. 

hello következő szakaszok tárgyalják hello hálózati kimenő kapcsolat változtatásokat, hogy a virtuális gépek az Azure Site Recovery replikációs toowork szükséges.

## <a name="outbound-connectivity-for-azure-site-recovery-urls"></a>Kimenő kapcsolódás az Azure Site Recovery URL-címek

Ha URL-alapú tűzfal proxy toocontrol kimenő kapcsolatok használata esetén lehet, hogy toowhitelist Azure Site Recovery szolgáltatás URL-címeket az alábbi szükséges:


**URL-CÍME** | **Cél**  
--- | ---
*.blob.core.windows.net | Szükséges, hogy az adatok beírhatók toohello gyorsítótár tárfiók hello forrás régióban hello virtuális gép.
login.microsoftonline.com | A hitelesítési és engedélyezési toohello Site Recovery szolgáltatás URL-címek esetén szükséges.
*.hypervrecoverymanager.windowsazure.com | Szükséges, hogy hello Site Recovery szolgáltatás kommunikációja akkor fordulhat elő, a virtuális gép hello.
*. servicebus.windows.net | Szükséges, hogy hello Site Recovery figyelése és diagnosztikai adatok hello VM beírhatók.

## <a name="outbound-connectivity-for-azure-site-recovery-ip-ranges"></a>Kimenő kapcsolódás az Azure Site Recovery IP-címtartományok

>[!NOTE]
> tooautomatically hello hálózati biztonsági csoport szükséges hello NSG-szabályok létrehozása, érdemes [töltse le és használja ezt a parancsfájlt](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).

>[!IMPORTANT]
> * Azt javasoljuk, hogy a tesztelési hálózati biztonsági csoport szükséges hello NSG-szabályok létrehozása, és ellenőrizze, hogy nincs probléma hello szabályok a termelési hálózati biztonsági csoport létrehozása előtt.
> * NSG-szabályok száma toocreate szükséges hello győződjön meg arról, hogy az előfizetés szerepel az engedélyezési listán. Forduljon a támogatási szolgálathoz tooincrease hello NSG szabály vonatkozó korlát az előfizetésben.

Ha bármely IP-alapú tűzfal proxy vagy az NSG-szabályok toocontrol kimenő kapcsolatot használ, hello következő IP-címtartományok kell toobe szerepel az engedélyezési listán, attól függően, hogy hello forrása és célja helyek hello virtuális gépek:

- Az összes IP-címtartományok toohello forráshely tartozik. (Letöltheti a hello [IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653).) Engedélyezett szükség, így az adatok is toohello gyorsítótár tárfiók a virtuális gép hello.

- Az összes IP-címtartományokat, amelyek megfelelnek a tooOffice 365 [hitelesítés és identitás IP V4 végpontok](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

    >[!NOTE]
    > Ha új IP-címek a jövőbeli hello hozzáadják tooOffice 365 IP-címtartományok, akkor toocreate új NSG-szabályok.
    
- Hely helyreállítási szolgáltatás végpontjának IP-címek ([rendelkezésre álló XML-fájlban](https://aka.ms/site-recovery-public-ips)), a célhely függ: 

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

## <a name="sample-nsg-configuration"></a>Mintakonfiguráció NSG
Ez a szakasz ismerteti a hello lépéseket tooconfigure NSG-szabályok, így a Site Recovery replikációs virtuális gépen használható. NSG-szabályok toocontrol kimenő kapcsolat használatakor a "HTTPS kimenő forgalom engedélyezése" szabályok használata az összes szükséges hello IP-címtartományok.

>[!Note]
> tooautomatically hello hálózati biztonsági csoport szükséges hello NSG-szabályok létrehozása, érdemes [töltse le és használja ezt a parancsfájlt](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).

Például ha a virtuális gép forrás elérési útja "USA keleti régiója" és a replikációs célhelye "Középső Régiójában", lépésekkel hello hello mellett két szakaszban.

>[!IMPORTANT]
> * Azt javasoljuk, hogy a tesztelési hálózati biztonsági csoport szükséges hello NSG-szabályok létrehozása, és ellenőrizze, hogy nincs probléma hello szabályok a termelési hálózati biztonsági csoport létrehozása előtt.
> * NSG-szabályok száma toocreate szükséges hello győződjön meg arról, hogy az előfizetés szerepel az engedélyezési listán. Forduljon a támogatási szolgálathoz tooincrease hello NSG szabály vonatkozó korlát az előfizetésben. 

### <a name="nsg-rules-on-hello-east-us-network-security-group"></a>NSG-szabályok hello USA keleti régiója hálózati biztonsági csoport

* Létrehozhat szabályokat, amelyek megfelelnek túl[keleti USA IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653). Erre azért szükség, így az adatok is toohello gyorsítótár tárfiók a virtuális gép hello.

* Létrehozhat szabályokat, amelyek megfelelnek a tooOffice 365 összes IP-címtartományokhoz [hitelesítés és identitás IP V4 végpontok](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

* Hozzon létre szabályokat, amelyek megfelelnek a toohello célhelye:

   **Hely** | **Site Recovery szolgáltatás IP-címek** |  **A Site Recovery IP figyelése**
    --- | --- | ---
   USA középső régiója | 40.69.144.231</br>40.69.167.116 | 52.165.34.144

### <a name="nsg-rules-on-hello-central-us-network-security-group"></a>NSG-szabályok hello USA középső RÉGIÓJA hálózati biztonsági csoport

Ezek a szabályok szükség, hogy a replikációs hello cél régió toohello forrás régió feladatátvételt követően engedélyezhető:

* Szabályok, amelyek megfelelnek túl[központi USA IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653). Ezek szükségesek, így az adatok is toohello gyorsítótár tárfiók a virtuális gép hello.

* Összes IP-címtartományokat, amelyek megfelelnek a tooOffice 365 szabályainak [hitelesítés és identitás IP V4 végpontok](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

* Szabályok, amelyek megfelelnek a toohello adatforrásról:

   **Hely** | **Site Recovery szolgáltatás IP-címek** |  **A Site Recovery IP figyelése**
    --- | --- | ---
   USA keleti régiója | 13.82.88.226</br>40.71.38.173 | 104.45.147.24


## <a name="guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration"></a>Meglévő Azure--helyszíni ExpressRoute és a VPN-konfiguráció vonatkozó irányelvek

Ha egy ExpressRoute- vagy VPN-kapcsolatot a helyszíni és hello rendelkezik az Azure-adatforrásról, kövesse az ebben a szakaszban hello irányelveket.

### <a name="forced-tunneling-configuration"></a>A kényszerített bújtatás konfigurálása

A közös felhasználói konfigurálása toodefine egy alapértelmezett útvonalat (0.0.0.0/0), amely arra kényszeríti a kimenő Internet forgalom tooflow hello helyszíni helyeken keresztül. Ez nem ajánlott meg. hello replikációs forgalom és a Site Recovery szolgáltatás kommunikációja nem hagyja hello Azure határ. hello megoldás tooadd felhasználó által definiált útvonalak (udr-EK) [ezek IP-címtartományok](#outbound-connectivity-for-azure-site-recovery-ip-ranges) , hogy hello replikációs forgalom nem halad a helyszíni.

### <a name="connectivity-between-hello-target-and-on-premises-location"></a>Hello cél és a helyszíni hely közötti kapcsolat

Kövesse a kapcsolatok hello célhelyet és hello helyszíni hely között:
- Ha az alkalmazásnak tooconnect toohello a helyszíni gépeket, vagy ha nincsenek a csatlakozó ügyfelek toohello alkalmazás a helyszíni VPN/ExpressRoute keresztül, győződjön meg arról, hogy legalább egy [pont-pont kapcsolat](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) között a cél Azure régióban és hello helyszíni adatközpontban.

- Ha nagy mennyiségű forgalom tooflow hello helyszíni adatközpontját a cél Azure-régiót között, akkor hozzon létre egy másik [ExpressRoute-kapcsolat](../expressroute/expressroute-introduction.md) hello cél Azure régióban és hello a helyszíni adatközpont között.

- Ha szeretne IP-címek tooretain hello virtuális gépek azok feladatátvételt követően, ne hello cél régió hely-hely vagy ExpressRoute-kapcsolat megszakad. Ez a toomake arról, hogy a nem tartomány csomópontjánál hello forrás régió IP-címtartományok és IP-címtartományok cél régió között.

### <a name="best-practices-for-expressroute-configuration"></a>Ajánlott eljárások az ExpressRoute-konfiguráció
Kövesse az alábbi gyakorlati tanácsok az ExpressRoute-konfiguráció:

- Mindkét hello forrása és célja régiókban ExpressRoute-kapcsolatcsoportot toocreate van szüksége. Akkor toocreate közötti kapcsolat:
  - hello forrás virtuális hálózat és hello ExpressRoute-kapcsolatcsoportot.
  - hello virtuális célhálózat és hello ExpressRoute-kapcsolatcsoportot.

- ExpressRoute standard részeként hozhatja létre kapcsolatok hello geopolitikai ugyanabban a régióban. toocreate ExpressRoute-Kapcsolatcsoportok geopolitikai különböző régiókban, a prémium szintű Azure ExpressRoute szükség, amely magában foglalja egy növekményes költséget. (Ha már használ ExpressRoute prémium, hogy ingyenesen.) További részletekért lásd: hello [helyek ExpressRoute a dokumentum](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) és [ExpressRoute árképzési](https://azure.microsoft.com/pricing/details/expressroute/).

- Azt javasoljuk, hogy a forrás és cél régióban különböző IP-címtartományok használja. hello ExpressRoute-kapcsolatcsoport nem fog tudni tooconnect hello két Azure virtuális hálózatokkal azonos IP-címtartományok: hello ugyanannyi időt vesz igénybe.

- Virtuális hálózatok ugyanazon IP-tartományok mindkét régió, és majd hozza létre az ExpressRoute-Kapcsolatcsoportok mindkét régiókban hello létrehozhat. A feladatátadási esemény hello esetben hello kapcsolat bontása hello forrás virtuális hálózat, valamint elérheti a hello áramkör hello cél virtuális hálózatban.

 >[!IMPORTANT]
 > Ha elsődleges régió hello teljesen le, hello művelet megszakítására sikertelen lehet. Amely megakadályozza, hogy hello cél virtuális hálózat első ExpressRoute-kapcsolatot.

## <a name="next-steps"></a>Következő lépések
A munkaterhelések számára a védelmének megkezdéséhez [Azure virtuális gépek replikálásához](site-recovery-azure-to-azure.md).
