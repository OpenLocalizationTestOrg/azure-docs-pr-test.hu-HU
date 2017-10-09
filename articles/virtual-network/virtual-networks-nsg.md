---
title: "aaaNetwork biztonsági csoportok az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan tooisolate és vezérlési forgalom áramlását a virtuális hálózatokon hello az elosztott tűzfalat használ a hálózati biztonsági csoportok használata Azure-ban."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: 20e850fc-6456-4b5f-9a3f-a8379b052bc9
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/11/2016
ms.author: jdial
ms.openlocfilehash: 3528ce833dab17977327c3c9ae0e78316e5e6a05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="filter-network-traffic-with-network-security-groups"></a>Hálózati forgalom szűrése hálózati biztonsági csoportokkal

A hálózati biztonsági csoport (NSG) felsorolja azokat a szabályokat, amelyek engedélyezik vagy megtagadják a hálózati forgalom tooresources csatlakoztatott tooAzure virtuális hálózatot (VNet). NSG-ket lehet társított toosubnets, az egyes virtuális gépeken (klasszikus), vagy az egyes hálózati adapterek (NIC) kapcsolódó tooVMs (Resource Manager). Amikor egy NSG társított tooa alhálózati, hello szabályokat alkalmazni tooall erőforrások csatlakoztatott toohello alhálózat. Forgalom tovább korlátozhatja is társításával egy NSG tooa VM vagy a hálózati adapterhez.

> [!NOTE]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md). Ez a cikk mindkét modell használatát bemutatja, de a Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

## <a name="nsg-resource"></a>NSG-erőforrás
NSG tartalmaz hello következő tulajdonságai:

| Tulajdonság | Leírás | Korlátozások | Megfontolandó szempontok |
| --- | --- | --- | --- |
| Név |Hello NSG neve |Hello régión belül egyedinek kell lennie.<br/>Betűket, számokat, aláhúzásjeleket, pontokat és kötőjeleket tartalmazhat.<br/>Betűvel vagy számmal kell kezdődnie.<br/>Betűvel, számmal vagy aláhúzásjellel kell végződnie.<br/>Nem lehet hosszabb 80 karakternél. |Toocreate esetleg több NSG-t, mert győződjön meg arról, hogy elnevezési szabályokat alkalmaz, amely lehetővé teszi az NSG-k egyszerű tooidentify hello funkcióját. |
| Régió |Azure [régió](https://azure.microsoft.com/regions) ahol hello NSG jön létre. |NSG-ket csak lehet belül hello társított tooresources hello NSG és ugyanabban a régióban. |NSG kapcsolatos toolearn lehet régiónként, olvassa el a hello [Azure korlátozza](../azure-subscription-service-limits.md#virtual-networking-limits-classic) cikk.|
| Erőforráscsoport |Hello [erőforráscsoport](../azure-resource-manager/resource-group-overview.md#resource-groups) hello NSG szerepel. |Bár az NSG szerepel egy erőforráscsoport, azok bármely erőforráscsoport társított tooresources mindaddig, amíg hello erőforrás hello része egy Azure-régióban van hello NSG. |Erőforráscsoportok vannak használt toomanage együtt, egy üzembe helyezési egységként több erőforrást.<br/>Érdemes hello NSG, amelyekhez társítva vannak az erőforrások csoportosítása. |
| Szabályok |Az engedélyezett vagy tiltott forgalmat meghatározó bejövő vagy kimenő szabályok. | |Lásd: hello [NSG-szabályok](#Nsg-rules) című szakaszát. |

> [!NOTE]
> A végpont-alapú ACL-EK és hálózati biztonsági csoportok nem támogatottak a hello az ugyanazon Virtuálisgép-példány. Ha szeretné, hogy toouse egy NSG-t, és már rendelkezik egy végponti ACL, először távolítsa el a hello végponti ACL-t. Hogyan tooremove hozzáférés-vezérlési Listában, olvassa el toolearn hello [kezelése hozzáférés-vezérlési listái (ACL) a PowerShell használatával végpontokhoz](virtual-networks-acl-powershell.md) cikk.
> 

### <a name="nsg-rules"></a>NSG-szabályok
NSG-szabályok a következő tulajdonságai hello tartalmazza:

| Tulajdonság | Leírás | Korlátozások | Megfontolások |
| --- | --- | --- | --- |
| **Name (Név)** |Hello szabály nevét. |Hello régión belül egyedinek kell lennie.<br/>Betűket, számokat, aláhúzásjeleket, pontokat és kötőjeleket tartalmazhat.<br/>Betűvel vagy számmal kell kezdődnie.<br/>Betűvel, számmal vagy aláhúzásjellel kell végződnie.<br/>Nem lehet hosszabb 80 karakternél. |Előfordulhat, hogy az NSG belül több szabály, ezért győződjön meg arról, hogy egy elnevezési konvenciója, amely lehetővé teszi a szabályok funkcióját tooidentify hello. |
| **Protocol (Protokoll)** |Protokoll toomatch hello szabályhoz. |TCP, UDP vagy * |Használatával * protokoll tartalmazza az ICMP (csak kelet-nyugati irányú forgalom), mert, csakúgy, mint az UDP- és TCP, és csökkentheti a szükséges szabályok hello száma.<br/>At hello azonos időben használatával * lehet a túl széles körű, ezért azt ajánljuk, hogy használjon * csak szükség esetén. |
| **Source port range (Forrásporttartomány)** |Source port tartomány toomatch hello szabályhoz. |Egy portszám 1 too65535, porttartomány (Példa: 1 – 65535), vagy * (az összes port). |A forrásportok rövid élettartamúak is lehetnek. Ha az ügyfélprogram nem egy adott portot használ, a legtöbb esetben a * jelet használja.<br/>Próbálja meg toouse porttartományok, mint lehetséges tooavoid hello több szabály van szükség.<br/>Több portot vagy porttartományt nem lehet vesszővel csoportosítani. |
| **Destination port range (Célporttartomány)** |Cél port tartomány toomatch hello szabályhoz. |Egy portszám 1 too65535, porttartomány (Példa: 1 – 65535), vagy \* (az összes port). |Próbálja meg toouse porttartományok, mint lehetséges tooavoid hello több szabály van szükség.<br/>Több portot vagy porttartományt nem lehet vesszővel csoportosítani. |
| **Source address prefix (Forráscímelőtag)** |Forrás cím előtag vagy címke toomatch hello szabályhoz. |Egy IP-cím (pl. 10.10.10.10), IP-alhálózat (pl. 192.168.1.0/24), [alapértelmezett címke](#default-tags) vagy * (minden címhez). |Érdemes lehet tartományokat, alapértelmezett címkéket, és * tooreduce hello szabályok száma. |
| **Destination address prefix (Célcímelőtag)** |Rendeltetési cím előtag vagy címke toomatch hello szabályhoz. | Egy IP-cím (pl. 10.10.10.10), IP-alhálózat (pl. 192.168.1.0/24), [alapértelmezett címke](#default-tags) vagy * (minden címhez). |Érdemes lehet tartományokat, alapértelmezett címkéket, és * tooreduce hello szabályok száma. |
| **Direction (Irány)** |Forgalom toomatch hello szabály irányának. |Bejövő vagy kimenő. |A bejövő vagy kimenő szabályokat a rendszer külön dolgozza fel, az irány alapján. |
| **Priority (Prioritás)** |A prioritásuk szerinti sorrendben hello ellenőrzi. Amint talál egy érvényes szabályt, nem vizsgálja, hogy a többi szabálynak megfelel-e a forgalom. | 100 és 4096 közötti szám. | Vegye figyelembe, létrehozhatja a hello jövőbeli prioritások Ugrás 100 új szabályokat minden egyes szabály tooleave terület-szabályok létrehozása. |
| **Access (Hozzáférés)** |Ha hello szabálynak hozzáférés tooapply típusú. | Engedélyezés vagy megtagadás. | Ne feledje, hogy ha egy olyan engedélyezési szabály nem található a csomagot, hello csomagot a rendszer eldobja. |

Az NSG-k két szabálykészletet tartalmaznak: bejövőt és kimenőt. hello a szabály prioritásának az egyes készleten belül egyedinek kell lennie. 

![Az NSG-szabály feldolgozása](./media/virtual-network-nsg-overview/figure3.png) 

hello előző ábrán is látható NSG-szabályok feldolgozásának módja.

### <a name="default-tags"></a>Alapértelmezett címkék
Alapértelmezett címkék olyan rendszer által biztosított azonosítók tooaddress IP-címek egy kategóriáját. Alapértelmezett címkék hello használható **forráscímelőtag** és **célcím-előtag** szabályok tulajdonságainak. Háromféle alapértelmezett címkét lehet használni:

* **VirtualNetwork** (erőforrás-kezelő) (**VIRTUAL_NETWORK** a klasszikus): ezt a címkét hello virtuális hálózat címtere (az Azure által meghatározott CIDR tartományok), az összes csatlakoztatott helyszíni címterek, és csatlakoztatva Az Azure Vnet (helyi hálózatok).
* **AzureLoadBalancer** (Resource Manager) (**AZURE_LOADBALANCER** klasszikus telepítéshez): Ez a címke az Azure infrastruktúra terheléselosztóját jelöli. hello címke több tooan Azure datacenter IP ahol Azure állapot-mintavételi csomagjai származnak.
* **Internet** (erőforrás-kezelő) (**INTERNET** a klasszikus): Ez a címke jelöli hello IP-címtér hello virtuális hálózaton kívül, és a nyilvános interneten keresztül elérhető. hello tartományba beletartozik hello [Azure tulajdonában lévő nyilvános IP-címtér](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="default-rules"></a>Alapértelmezett szabályok
Minden NSG tartalmaz egy alapértelmezett szabálykészletet. hello alapértelmezett szabályokat nem lehet törölni, de a legalacsonyabb prioritású hello hozzárendeli őket, mert azok felülbírálhatja hello létrehozott szabályok. 

hello alapértelmezett szabályok engedélyezése, és engedélyezi a forgalom az alábbiak szerint:
- **Virtuális hálózat:** A virtuális hálózatból kiinduló és oda érkező forgalom a bejövő és kimenő irányban is engedélyezve van.
- **Internet:** A kimenő forgalom engedélyezett, de a bejövő forgalom le van tiltva.
- **Terheléselosztó:** engedélyezése Azure load terheléselosztó tooprobe hello állapotát a virtuális gépek és a szerepkörpéldányok. Ha nem terheléselosztásos készletet használ, ezt a szabályt felül lehet bírálni.

**Bejövő alapértelmezett szabályok**

| Név | Prioritás | Forrás IP-címe | Forrásport | Cél IP-címe | Célport | Protokoll | Hozzáférés |
| --- | --- | --- | --- | --- | --- | --- | --- |
| AllowVNetInBound |65000 | VirtualNetwork | * | VirtualNetwork | * | * | Engedélyezés |
| AllowAzureLoadBalancerInBound | 65001 | AzureLoadBalancer | * | * | * | * | Engedélyezés |
| DenyAllInBound |65500 | * | * | * | * | * | Megtagadás |

**Kimenő alapértelmezett szabályok**

| Név | Prioritás | Forrás IP-címe | Forrásport | Cél IP-címe | Célport | Protokoll | Hozzáférés |
| --- | --- | --- | --- | --- | --- | --- | --- |
| AllowVnetOutBound | 65000 | VirtualNetwork | * | VirtualNetwork | * | * | Engedélyezés |
| AllowInternetOutBound | 65001 | * | * | Internet | * | * | Engedélyezés |
| DenyAllOutBound | 65500 | * | * | * | * | * | Megtagadás |

## <a name="associating-nsgs"></a>Az NSG-k társítása
Társíthatja egy NSG tooVMs, hálózati adapterek és alhálózatok, attól függően, hogy hello telepítési modellt használ, az alábbiak szerint:

* **Virtuális gép (csak klasszikus):** biztonsági szabályok érvényesek tooall forgalom hello VM onnan. 
* **Hálózati adapter (csak Resource Manager):** biztonsági szabályok érvényesek tooall forgalom onnan hello NIC hello NSG társítva. Több hálózati Adapterrel virtuális gépen, különböző vonatkoznak (vagy hello azonos) NSG tooeach NIC külön-külön. 
* **Az alhálózat (Resource Manager és klasszikus):** biztonsági szabályok érvényesek tooany forgalom onnan semmilyen erőforráshoz kapcsolódó toohello virtuális hálózat.

Különböző NSG-k tooa virtuális gép (vagy hálózati adapter, attól függően, hogy hello üzembe helyezési modellel) társítani, és hello alhálózatot, amely egy hálózati adapter vagy a virtuális gép csatlakozik-e. Biztonsági szabályok alkalmazott toohello forgalom prioritás, minden NSG hello a következő sorrendben:

- **Bejövő forgalom**

  1. **Alkalmazott NSG toosubnet:** NSG alhálózat rendelkezik egy megfelelő szabályt toodeny forgalmat, ha a rendszer eldobja hello csomagot.

  2. **NSG alkalmazott tooNIC** (Resource Manager) vagy a virtuális gép (klasszikus): Ha VM\NIC NSG egy megfelelő szabályt, amely megtiltja a forgalmat, a rendszer eldobott csomagok következő hello VM\NIC, akkor is, ha egy alhálózat NSG-t egy megfelelő szabályt, amely lehetővé teszi a forgalom.

- **Kimenő forgalom**

  1. **NSG alkalmazott tooNIC** (Resource Manager) vagy a virtuális gép (klasszikus): a VM\NIC NSG tartalmaz egy megfelelő szabályt, amely megtiltja a forgalmat, ha a rendszer eldobott csomagok.

  2. **Alkalmazott NSG toosubnet:** NSG alhálózat rendelkezik egy megfelelő szabályt, amely megtiltja a forgalmat, ha vannak miatt eldobott csomagok, akkor is, ha a VM\NIC NSG egy megfelelő szabályt, amely lehetővé teszi a forgalom.

> [!NOTE]
> Bár csak társíthat egy egyetlen NSG tooa alhálózati, VM vagy a hálózati adapter; társíthat hello a legtöbb erőforrást azonos NSG tooas ahányat csak szeretne.
>

## <a name="implementation"></a>Megvalósítás
Az NSG-k hello erőforrás-kezelő vagy a klasszikus üzembe helyezési modellek használata a következő eszközök hello valósíthatja meg:

| Üzembe helyezési eszköz | Klasszikus | Resource Manager |
| --- | --- | --- |
| Azure Portal   | Igen | [Igen](virtual-networks-create-nsg-arm-pportal.md) |
| PowerShell     | [Igen](virtual-networks-create-nsg-classic-ps.md) | [Igen](virtual-networks-create-nsg-arm-ps.md) |
| Azure CLI **V1**   | [Igen](virtual-networks-create-nsg-classic-cli.md) | [Igen](virtual-networks-create-nsg-cli-nodejs.md) |
| Azure CLI **V2**   | Nem | [Igen](virtual-networks-create-nsg-arm-cli.md) |
| Azure Resource Manager-sablon   | Nem  | [Igen](virtual-networks-create-nsg-arm-template.md) |

## <a name="planning"></a>Tervezés
NSG-k megvalósítása előtt kell tooanswer hello a következő kérdéseket:

1. Milyen típusú erőforrásokkal szeretné toofilter forgalom tooor származó? Csatlakoztathat erőforrásokat, például hálózati adaptereket (Resource Manager-alapú modell), virtuális gépeket (klasszikus modell), felhőszolgáltatásokat, alkalmazásszolgáltatási környezeteket és virtuálisgép-méretezési csoportokat. 
2. Azt szeretné, hogy toofilter forgalom és a meglévő vnetek csatlakoztatott toosubnets hello erőforrások vannak?

Hálózati biztonság Azure-ban tervezési további információkért olvassa el a hello [a felhőalapú szolgáltatások és a hálózati biztonság](../best-practices-network-security.md) cikk. 

## <a name="design-considerations"></a>Kialakítási szempontok
Miután eldöntötte hello válaszokat, hello toohello kérdéseire [tervezése](#Planning) szakaszban, tekintse át a következő részekben az NSG-k definiálása előtt hello:

### <a name="limits"></a>Korlátok
Nincsenek korlátozások előfizetés is lehet NSG-k számát toohello és ben szereplő NSG-szabályok száma. További információk hello korlátok, olvassa el a hello toolearn [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) cikk.

### <a name="vnet-and-subnet-design"></a>VNet és alhálózat kialakítása
NSG-ket lehet alkalmazott toosubnets, mivel minimalizálhatja hello NSG-k száma alhálózat szerint csoportosítja az erőforrásokat, és az NSG-k toosubnets alkalmazásával.  Ha úgy dönt, hogy az NSG-k toosubnets tooapply, előfordulhat, hogy meglévő Vnetek és alhálózatok meghatározásánál nem vették az NSG-ket szem előtt. Előfordulhat, hogy toodefine új virtuális hálózatokat és alhálózatokat toosupport kell az NSG-kialakítás és központi telepítése az új erőforrások tooyour új alhálózatokra. A meglévő erőforrásokat toohello új alhálózatok áttelepítési stratégia toomove majd adható meg. 

### <a name="special-rules"></a>Különleges szabályok
Ha hello a következő szabályok által engedélyezett forgalmat blokkolja, az infrastruktúra nem tud kommunikálni alapvető fontosságú Azure szolgáltatásokkal:

* **Virtuális IP-címe hello gazdacsomópont:** alapvető infrastruktúra szolgáltatásokat, például a DHCP, DNS és állapotfigyelés szolgáltatáson keresztül hello virtuális gazdagép IP-cím 168.63.129.16. A nyilvános IP-cím tooMicrosoft tartozik, és hello csak a virtualizált IP-cím minden régióban erre a célra használt. Az IP-cím toohello fizikai IP-címe hello kiszolgálógép (gazdacsomópont) hello Virtuálisgép üzemeltetési képezi le. hello gazdacsomópont eleget kell hello DHCP-továbbítóügynök, rekurzív DNS-feloldó hello és hello vizsgálati forrás hello betölteni a terheléselosztó és hello gép állapotmintáihoz. Kommunikációs toothis IP-cím nincs támadás.
* **Licencelés (Kulcskezelő szolgáltatás):** A virtuális gépeken futó Windows-rendszerképeket licencelni kell. a rendszer toohello kulcskezelő szolgáltatás gazdakiszolgálók ilyen kérelmeket kezelő tooensure licencelés, a kérést küld. hello kérelem kimenő 1688-as porton keresztül.

### <a name="icmp-traffic"></a>ICMP-forgalom
hello jelenlegi NSG-szabályok csak protokollokat engedélyezik *TCP* vagy *UDP*. Az *ICMP*-nek nincs külön címkéje. Azonban az ICMP-forgalmat megengedett történik egy Vneten belül hello AllowVNetInBound alapértelmezett szabály, amely lehetővé teszi a forgalom tooand a bármely portról és protokollról hello virtuális hálózaton belül.

### <a name="subnets"></a>Alhálózatok
* Vegye figyelembe a számítási feladat által igényelt rétegek hello száma. Minden réteget el lehet különíteni egy alhálózatot egy alkalmazott NSG toohello alhálózati való használatával. 
* Ha a VPN-átjáró vagy ExpressRoute-kapcsolatcsoportot tooimplement alhálózat van szüksége, tegye **nem** alkalmazza az NSG-toothat alhálózatot. Ha mégis így tesz, virtuális hálózatok vagy létesítmények közötti kapcsolatok megszakadhatnak. 
* Ha a hálózati virtuális készülék (NVA) tooimplement van szüksége, hello NVA tooits saját alhálózathoz csatlakozzon, és létre felhasználó által definiált útvonalak (UDR) tooand hello NVA. Megvalósíthat egy alhálózatszintű NSG toofilter forgalom mindkét alhálózat. További információk udr-EK, olvassa el a hello toolearn [felhasználó által definiált útvonalak](virtual-networks-udr-overview.md) cikk.

### <a name="load-balancers"></a>Terheléselosztók
* Fontolja meg az egyes használják az egyes terheléselosztó címfordítási (NAT) szabályait hello terheléselosztás és a hálózat címét. NAT-szabályok kötött tooa háttér-készlet, amely tartalmazza a hálózati adaptert (Resource Manager) vagy a virtuális gépek vagy Felhőszolgáltatások szerepkörpéldányokat (klasszikus). Érdemes létrehozni egy NSG-t mindegyik háttér-készlet, így csak által leképezett forgalom hello terheléselosztók megvalósított hello szabályok. Egy NSG-t mindegyik háttér-készlet létrehozása biztosítja, hogy adatforgalma toohello háttér-készlet (hanem közvetlenül hello terheléselosztó keresztül), is szűrve van.
* A klasszikus üzembe helyezés hozzon létre, amelyek leképezik a terhelés terheléselosztó tooports a virtuális gépekre vagy szerepkörpéldányokra a végpontok. Emellett a Resource Managerrel létrehozhatja saját önálló, nyilvános terheléselosztóját. bejövő forgalom célportja hello hello tényleges portja hello virtuális gép vagy szerepkörpéldány, nem jelennek meg, ha egy terhelés-kiegyenlítő hello port. hello forrásport és a virtuális gép egy cím és port a hello kapcsolat toohello címet hello hello Internet távoli számítógépre, nem hello portok és hello terheléselosztó által elérhetővé tett tárolókra.
* NSG-k toofilter forgalmat szűrjék egy belső terheléselosztón (ILB) létrehozásakor hello portok és forrástartományt alkalmazza a számítógépre, nem hello terheléselosztó származó hello vannak. hello portok és céltartomány azok hello célként megadott számítógéphez, nem hello terheléselosztóhoz.

### <a name="other"></a>Egyéb
* A végpont-alapú hozzáférés-vezérlési lista (ACL), és az NSG-k nem támogatottak a hello ugyanazon Virtuálisgép-példány. Ha szeretné, hogy toouse egy NSG-t, és már rendelkezik egy végponti ACL, először távolítsa el a hello végponti ACL-t. További információ tooremove egy végponti ACL, lásd: hello [végponti ACL-eket kezelése](virtual-networks-acl-powershell.md) cikk.
* Az erőforrás-kezelő használata egy társított NSG-t tooa hálózati adapter virtuális gépek több hálózati adapter tooenable kezelés (távelérés) egy hálózati adapter alapon. Választhatók szét a forgalomtípusok egyedi NSG-k tooeach NIC társítása lehetővé teszi a hálózati adapter között.
* Terheléselosztók, más Vnetekről érkező forgalom hasonló toohello használatát, hello forráscímtartományát hello távoli számítógépen, nem a hello Vneteket összekötő átjárót hello kell használnia.
* Sok Azure-szolgáltatásokhoz kapcsolódó tooVNets nem lehet. Ha egy Azure-erőforrás nincs csatlakoztatott tooa virtuális hálózaton, egy NSG toofilter forgalom toohello erőforrás nem használható.  Hello szolgáltatások dokumentációjából hello használata toodetermine e hello szolgáltatás csatlakoztatott tooa VNet is lehet.

## <a name="sample-deployment"></a>Üzembe helyezési minta
tooillustrate hello alkalmazásának hello cikkben, fontolja meg egy általános forgatókönyv a két réteg alkalmazás hello az alábbi képen látható:

![NSG-k](./media/virtual-network-nsg-overview/figure1.png)

Ahogy hello ábrán is látható, hello *Web1* és *Web2* virtuális gépek a csatlakoztatott toohello *előtér* alhálózati és hello *D1* és *DB2* virtuális gépek a csatlakoztatott toohello *háttér* alhálózat.  Mindkét alhálózat részei hello *TestVNet* virtuális hálózat. hello alkalmazás-összetevők minden egyes futtatása egy Azure virtuális Géphez csatlakoztatott tooa Vneten belül. hello forgatókönyv rendelkezik hello követelményeknek:

1. Hello WEBES és adatbázis-kiszolgálók közötti forgalom elkülönítése.
2. A terheléselosztási szabályok forgalom a hello load balancer tooall webkiszolgálók 80-as porton.
3. Terheléselosztó NAT szabályok forgalom a port 50001 tooport 3389-es a WEB1 VM hello hello terheléselosztó beérkező betölteni.
4. Nincs hozzáférés toohello előtér- vagy a háttér-virtuális gépek a hello Internet, kivéve a 2. és 3.
5. Nincs kimenő Internet-hozzáférést a hello WEB vagy adatbázis-kiszolgáló.
6. Hello FrontEnd alhálózatból elérését tooport 3389-es a webkiszolgálón.
7. Hello FrontEnd alhálózatból elérését tooport 3389-es kívánt adatbázis-kiszolgáló.
8. Hello FrontEnd alhálózatból elérését tooport 1433-as port az összes adatbázis-kiszolgáló.
9. A felügyeleti forgalom (3389-es port) elkülönül az adatbázis-forgalomtól (1433) a DB kiszolgálók különböző hálózati adapterein.

1 – 6 követelmény (kivéve a követelmények, 3. és 4) minden zárt toosubnet szóközök. hello következő NSG-k követelményeinek hello előző, ugyanakkor minimalizálja a szükséges NSG-k hello száma:

### <a name="frontend"></a>Előtér
**Bejövő szabályok**

| Szabály | Hozzáférés | Prioritás | Forráscímtartomány | Forrásport | Célcímtartomány | Célport | Protokoll |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Allow-Inbound-HTTP-Internet | Engedélyezés | 100 | Internet | * | * | 80 | TCP |
| Allow-Inbound-RDP-Internet | Engedélyezés | 200 | Internet | * | * | 3389 | TCP |
| Deny-Inbound-All | Megtagadás | 300 | Internet | * | * | * | TCP |

**Kimenő szabályok**

| Szabály | Hozzáférés | Prioritás | Forráscímtartomány | Forrásport | Célcímtartomány | Célport | Protokoll |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Deny-Internet-All |Megtagadás |100 | * | * | Internet | * | * |

### <a name="backend"></a>Háttér
**Bejövő szabályok**

| Szabály | Hozzáférés | Prioritás | Forráscímtartomány | Forrásport | Célcímtartomány | Célport | Protokoll |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Deny-Internet-All | Megtagadás | 100 | Internet | * | * | * | * |

**Kimenő szabályok**

| Szabály | Hozzáférés | Prioritás | Forráscímtartomány | Forrásport | Célcímtartomány | Célport | Protokoll |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Deny-Internet-All | Megtagadás | 100 | * | * | Internet | * | * |

következő NSG-k jönnek létre, és a virtuális gépek a következő hello tooNICs tartozó hello:

### <a name="web1"></a>WEB1
**Bejövő szabályok**

| Szabály | Hozzáférés | Prioritás | Forráscímtartomány | Forrásport | Célcímtartomány | Célport | Protokoll |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Allow-Inbound-RDP-Internet | Engedélyezés | 100 | Internet | * | * | 3389 | TCP |
| Allow-Inbound-HTTP-Internet | Engedélyezés | 200 | Internet | * | * | 80 | TCP |

> [!NOTE]
> hello forrás címtartomány hello előző szabályok **Internet**, nem hello hello terheléselosztó, VIP-címmel. hello forrásport pedig *, nem 500001. Egy terheléselosztó NAT-szabályok nem ugyanaz, mint a NSG biztonsági szabályok vannak hello. NSG biztonsági szabályok mindig kapcsolódó toohello eredeti forrásához és végső célhelyéhez, a forgalom **nem** hello terheléselosztó hello két között. 
> 
> 

### <a name="web2"></a>WEB2
**Bejövő szabályok**

| Szabály | Hozzáférés | Prioritás | Forráscímtartomány | Forrásport | Célcímtartomány | Célport | Protokoll |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Deny-Inbound-RDP-Internet | Megtagadás | 100 | Internet | * | * | 3389 | TCP |
| Allow-Inbound-HTTP-Internet | Engedélyezés | 200 | Internet | * | * | 80 | TCP |

### <a name="db-servers-management-nic"></a>DB kiszolgálók (felügyeleti hálózati adapter)
**Bejövő szabályok**

| Szabály | Hozzáférés | Prioritás | Forráscímtartomány | Forrásport | Célcímtartomány | Célport | Protokoll |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Allow-Inbound-RDP-Front-end | Engedélyezés | 100 | 192.168.1.0/24 | * | * | 3389 | TCP |

### <a name="db-servers-database-traffic-nic"></a>DB kiszolgálók (adatbázis-forgalmi hálózati adapter)
**Bejövő szabályok**

| Szabály | Hozzáférés | Prioritás | Forráscímtartomány | Forrásport | Célcímtartomány | Célport | Protokoll |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Allow-Inbound-SQL-Front-end | Engedélyezés | 100 | 192.168.1.0/24 | * | * | 1433 | TCP |

Hello NSG-k társítva tooindividual hálózati adapter, mivel hello szabályok Resource Manager használatával telepített erőforrások vannak. A szabályokat egyesíti a rendszer az alhálózat és a hálózati adapterek vonatkozásában, attól függően, hogyan vannak társítva. 

## <a name="next-steps"></a>Következő lépések
* [NSG-k üzembe helyezése a Resource Managerrel](virtual-networks-create-nsg-arm-pportal.md).
* [NSG-k telepítése a klasszikus modellel](virtual-networks-create-nsg-classic-ps.md).
* [Manage NSG logs](virtual-network-nsg-manage-log.md) (NSG-naplók kezelése).
* [NSG-k hibaelhárítása] (virtual-network-nsg-troubleshoot-portal.md)
