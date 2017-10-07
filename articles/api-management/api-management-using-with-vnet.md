---
title: "aaaHow toouse Azure API Management virtuális hálózatokat"
description: "Ismerje meg, hogyan toosetup kapcsolat tooa virtuális hálózatot az Azure API Management és az access web services rajta."
services: api-management
documentationcenter: 
author: antonba
manager: erikre
editor: 
ms.assetid: 64b58f7b-ca22-47dc-89c0-f6bb0af27a48
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 426b3974e4fed7daffdb0c3f02381edbc326dc28
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-api-management-with-virtual-networks"></a>Hogyan toouse Azure API Management virtuális hálózatokat
Az Azure virtuális hálózatokról (Vnetekről) teszik lehetővé, amelyek nem internetes routeable hálózat az Azure-erőforrások bármelyike elérhető tooplace. Ezek a hálózatok majd lehet különböző VPN technológiáin használatával csatlakoztatott tooyour helyszíni hálózatot. több Azure virtuális hálózatokról kezdődnie hello információkat itt toolearn: [Azure virtuális hálózat áttekintése](../virtual-network/virtual-networks-overview.md).

Az Azure API Management telepíthető hello virtuális hálózatban (VNET), így hozzáférhet háttér szolgáltatások hello hálózaton belül. hello fejlesztői portálján és API-átjárón lehet konfigurált toobe elérhető hello Internet vagy csak hello virtuális hálózaton belül.

> [!NOTE]
> Az Azure API Management támogatja a classic és az Azure Resource Manager Vnetek.
>
>

## <a name="enable-vpn"></a>Kapcsolatcsoporttal engedélyezése
> [!NOTE]
> VNET-kapcsolatot érhető el hello **prémium** és **fejlesztői** rétegek. hello rétegek közötti tooswitch nyissa meg az API Management szolgáltatás hello Azure-portálon, és nyissa meg hello **és az árképzés** fülre. A hello **tarifacsomag** szakasz hello támogatási vagy fejlesztői réteg között, kattintson a Mentés gombra.
>

tooenable VNET-kapcsolatot, nyissa meg az API Management szolgáltatás hello Azure-portálon, és nyissa meg a hello **virtuális hálózati** lap.

![Virtuális hálózati menü API Management][api-management-using-vnet-menu]

Válassza ki a kívánt hello hozzáférés típusát:

* **Külső**: hello API Management gateway és fejlesztői portálon érhetők el a nyilvános interneten keresztül egy külső terheléselosztó hello. hello átjáró hello virtuális hálózaton belüli erőforrások eléréséhez.

![Nyilvános társviszony-létesítés][api-management-vnet-public]

* **Belső**: hello API Management-átjáró és a fejlesztői portálra csak a belső terheléselosztók virtuális hálózatát hello belülről érhetők el. hello átjáró hello virtuális hálózaton belüli erőforrások eléréséhez.

![Magánhálózati társviszony-létesítés][api-management-vnet-private]

Ekkor minden régióban, ahol az API Management szolgáltatás ki van építve listáját. Válassza ki a virtuális hálózat és alhálózat minden régióhoz. hello lista fel van töltve, a klasszikus és Resource Manager virtuális hálózatok az Azure-előfizetések, amelyek a telepítő konfigurálja a hello régióban elérhető.

> [!NOTE]
> **Szolgáltatásvégpont** a fenti ábrán hello átjáró /-Proxy, Publisher Portal, fejlesztői portálján, GIT, és tartalmaz hello közvetlen felügyeleti végpontja.
> **Felügyeleti végpont** a fenti ábrán hello hello végpont üzemeltetett toomanage szolgáltatáskonfiguráció hello Azure-portál és a Powershell segítségével.
> Vegye figyelembe azt is, amely akkor is, ha hello látható IP-címek a különböző végpontokhoz, az API Management szolgáltatás **csak** reagál a beállított állomásnevek a.

> [!IMPORTANT]
> Az Azure API Management példány tooa erőforrás-kezelő virtuális hálózat telepítésekor hello szolgáltatást az Azure API Management példányok kivételével nincs más erőforrásokat tartalmazó dedikált alhálózat kell lennie. Ha toodeploy tett kísérlet az Azure API Management példány tooa más erőforrások, az hello telepítését tartalmazó erőforrás-kezelő virtuális hálózat alhálózati sikertelen lesz.
>
>

![Jelölje ki a VPN][api-management-setup-vpn-select]

Kattintson a **mentése** üdvözlő képernyőt hello tetején.

> [!NOTE]
> hello hello API Management-példány a címe változik minden alkalommal, amikor virtuális hálózat engedélyezve vagy letiltva.  
> hello virtuális IP-cím is megváltozik az API Management mozgatásakor **külső** túl**belső** vagy fordítva
>


> [!IMPORTANT]
> Távolítsa el az API Management egy VNETET, vagy módosítsa egy telepítették hello, ha korábban használt VNET maradjanak hello zárolt too4 órában. Ebben az időszakban nem kell a lehetséges toodelete hello VNET és központi telepítése egy új erőforrás tooit.

## <a name="enable-vnet-powershell"></a>Engedélyezése kapcsolatcsoporttal PowerShell-parancsmagok használatával
Engedélyezheti a virtuális hálózat kapcsolat hello PowerShell-parancsmagok használatával

* **A VNETEN belül az API Management szolgáltatás létrehozása**: hello parancsmaggal [New-AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) toocreate Azure API Management szolgáltatásnak a VNETEN belül.

* **A VNETEN belül az meglévő API Management-szolgáltatások üzembe**: hello parancsmaggal [frissítés-AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) toomove egy meglévő Azure API Management szolgáltatást egy virtuális hálózaton belül.

## <a name="connect-vnet"></a>Csatlakozzon a virtuális hálózaton belül futó tooa webszolgáltatás
Miután az API Management szolgáltatás csatlakoztatott toohello VNET, benne háttér-szolgáltatások eléréséhez szükséges eltér nem nyilvános szolgáltatások eléréséhez szükséges. Csak írja be a hello helyi IP-cím vagy hello állomásneve (ha hello virtuális hálózat DNS-kiszolgáló van konfigurálva) a webszolgáltatás rendszerbe hello **webszolgáltatás URL-címe** mezőben egy új API létrehozásakor vagy egy meglévőt.

![A VPN API hozzáadása][api-management-setup-vpn-add-api]

## <a name="network-configuration-issues"></a>Közös hálózati konfigurációs problémák
Az alábbiakban az API-kezelés szolgáltatás telepítése virtuális hálózatba során előforduló gyakori helytelen konfiguráció-problémák listáját.

* **Egyéni DNS-kiszolgáló beállításainak**: hello API-kezelés szolgáltatás több Azure-szolgáltatások függ. Az API Management egyéni DNS-kiszolgáló a VNETEN belül helyezkedik el, amikor jogcímadatokat tooresolve hello állomásnevek Azure szolgáltatások. Kövesse az [ez](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) egyéni DNS-beállításainak útmutatást. Lásd: hello portokat az alábbi táblázat és az egyéb hálózati követelmények hivatkozás.

> [!IMPORTANT]
> Javasoljuk, hogy egy egyéni DNS-kiszolgálói használatakor a hello VNET beállítása, amely **előtt** bele az API Management szolgáltatás telepítéséhez. Ellenkező esetben kell túl frissítenie hello API-kezelés szolgáltatás hello DNS-kiszolgálók (s) minden módosításakor hello futtatásával [alkalmazni a hálózati konfigurációs műveletet](https://docs.microsoft.com/en-us/rest/api/apimanagement/apimanagementservice#ApiManagementService_ApplyNetworkConfigurationUpdates)

* **Az API Management szükséges portok**: hello alhálózati API Management van telepítve a bejövő és kimenő forgalom vezérelhető [hálózati biztonsági csoport][Network Security Group]. Ha ezeket a portokat bármelyike nem érhető el, az API Management nem fog megfelelően működni, és lehetnek nem elérhetők. Rendelkezik egy vagy több ezeket a portokat blokkolva helytelen konfigurálása más gyakori problémák használata esetén az API Management olyan virtuális hálózaton.

Az API Management service-példány a VNETEN belül helyezkedik el, amikor a rendszer a következő táblázat hello hello portokat használja.

| Forrás / cél port(ok) | Irány | Átviteli protokoll | Cél | Forrás / cél | Hozzáférés típusa |
| --- | --- | --- | --- | --- | --- |
| * / 80, 443 |Bejövő |TCP |Ügyfél-kommunikáció tooAPI kezelése |INTERNET / VIRTUAL_NETWORK |Külső |
| * / 3443 |Bejövő |TCP |Az Azure portál és a Powershell felügyeleti végpont |INTERNET / VIRTUAL_NETWORK |Külső és belső |
| * / 80, 443 |Kimenő |TCP |Az Azure Storage és az Azure Service Bus függőség |VIRTUAL_NETWORK / INTERNET |Külső és belső |
| * / 1433 |Kimenő |TCP |Az Azure SQL függőség |VIRTUAL_NETWORK / INTERNET |Külső és belső |
| * / 11000 - 11999 |Kimenő |TCP |V12 Azure SQL függőség |VIRTUAL_NETWORK / INTERNET |Külső és belső |
| * / 14000 - 14999 |Kimenő |TCP |V12 Azure SQL függőség |VIRTUAL_NETWORK / INTERNET |Külső és belső |
| * / 5671 |Kimenő |AMQP |A napló tooEvent központi házirend- és figyelési ügynök függőség |VIRTUAL_NETWORK / INTERNET |Külső és belső |
| 6381 - 6383 / 6381 - 6383 |Bejövő és kimenő |UDP |Redis gyorsítótár függőség |VIRTUAL_NETWORK / VIRTUAL_NETWORK |Külső és belső |-
| * / 445 |Kimenő |TCP |Azure-fájlmegosztáshoz git függőség |VIRTUAL_NETWORK / INTERNET |Külső és belső |
| * / * | Bejövő |TCP |Az Azure infrastruktúra Terheléselosztóját | AZURE_LOAD_BALANCER / VIRTUAL_NETWORK |Külső és belső |

* **SSL-funkció**: tooenable SSL-tanúsítvány láncában épület és az érvényesítés hello API Management szolgáltatásnak kell kimenő hálózati kapcsolat tooocsp.msocsp.com, mscrl.microsoft.com és crl.microsoft.com. A függőség nincs szükség, ha bármelyik felügyeleti tooAPI feltöltött tanúsítvány nem tartalmaz hello teljes lánc toohello hitelesítésszolgáltató legfelső szintű.

* **DNS hozzáférési**: 53-as port kimenő hozzáférést kell DNS-kiszolgálókkal való kommunikációhoz. Ha egy egyéni DNS-kiszolgáló létezik a VPN-átjáró másik végén hello, hello DNS-kiszolgálót futtató API Management hello alhálózatból elérhetőnek kell lennie.

* **Metrikák és az állapotfigyelés**: kimenő hálózati kapcsolat tooAzure figyelés végpontok, amely alapján a következő tartományok hello feloldani: global.metrics.nsatc.net, shoebox2.metrics.nsatc.net, prod3.metrics.nsatc.net.

* **Útvonal gyorstelepítése**: egy közös felhasználói konfigurálása toodefine a saját alapértelmezett útvonalat (0.0.0.0/0), amely arra kényszeríti a kimenő Internet forgalom tooinstead folyamata a helyszíni van. A forgalom áramlását tüntetnek megszakítja a kapcsolatot az Azure API Management, mert hello kimenő adatforgalmat a letiltott helyi, vagy NAT-d tooan felismerhetetlen címek, amelyek már nem használhatók a különböző Azure-végpontok készlete. hello megoldás egy (vagy több) toodefine felhasználó által definiált útvonalak ([udr-EK][UDRs]), amely tartalmazza az Azure API Management hello hello alhálózaton. Egy UDR hello alapértelmezett útvonal helyett szembeni szerződéses kötelezettségeket vonatkozó alhálózati útvonalakat határozza meg.
  Ha lehetséges a következő konfigurációs toouse hello ajánlott:
 * hello ExpressRoute konfigurációs hirdeti 0.0.0.0/0, és alapértelmezés szerint kényszerített bújtatja minden kimenő forgalom helyszíni.
 * hello alkalmazott UDR toohello alhálózati hello Azure API Management tartalmazó 0.0.0.0/0 az Internet egy következő ugrás típusa határozza meg.
 hello kombinált hatását, hogy ezeket a lépéseket az, hogy a hello alhálózat-szintű UDR elsőbbséget élvez hello ExpressRoute kényszerített bújtatás, biztosítva ezzel az Azure API Management hello kimenő Internet-hozzáféréssel.

>[!WARNING]  
>Az Azure API Management használata nem támogatott az ExpressRoute-konfigurációkat, amelyek **helytelenül hello nyilvános társviszony-létesítési elérési toohello magánhálózati társviszony-létesítési elérési útvonalak határokon hirdetési**. ExpressRoute-konfigurációk, amelyek rendelkeznek a nyilvános társviszony konfigurálva, a Microsoft Azure IP-címtartományok számos útvonal-hirdetéseinek kap Microsoft. Ha ezek címtartományai helytelenül határokon meghirdetett hello magánhálózati társviszony-létesítési elérési úton, hello végeredménynek, hogy minden kimenő hálózati csomagok hello Azure API Management példány alhálózatból helytelenül kényszerített bújtatott tooa az ügyfél helyszíni hálózat infrastruktúra. Hálózati folyamatot az Azure API Management megszakítja. hello megoldás toothis probléma a toostop cross-hirdetési útvonalak hello nyilvános társviszony-létesítési elérési toohello magánhálózati társviszony-létesítési elérési útról.


## <a name="troubleshooting"></a>Hibaelhárítása
Módosítások tooyour hálózati meghozásakor tekintse meg a túl[NetworkStatus API](https://docs.microsoft.com/en-us/rest/api/apimanagement/networkstatus), toovalidate Ha hello API-kezelés szolgáltatás nem csatlakozik hozzáférés tooany hello a kritikus erőforrásokat, amelyek függ. hello állapotának 15 percenként frissíteni kell.

## <a name="limitations"></a>Korlátozásai
* Az API Management-példányokat tartalmazó alhálózat bármely más Azure-erőforrás típusa nem tartalmazhat.
* hello alhálózati és hello szolgáltatást kell lennie az API Management hello ugyanahhoz az előfizetéshez.
* Az API Management-példányokat tartalmazó alhálózat előfizetésekhez nem helyezhetők.
* Belső virtuális hálózat, használatával csak a belső IP-cím elérhető lesz a hello tartomány megadott [RFC 1918](https://tools.ietf.org/html/rfc1918), egy nyilvános IP-cím nem adható meg.
* Több területi API-kezelés központi telepítése esetén a konfigurált, belső virtuális hálózatok felhasználók felelősek hello DNS saját, a saját terheléselosztási kezelése.


## <a name="related-content"></a>Kapcsolódó tartalom
* [Csatlakozás a virtuális hálózati toobackend VPN-átjáró használatával](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti)
* [Egy virtuális hálózathoz csatlakozó különböző üzembe helyezési modellel](../vpn-gateway/vpn-gateway-connect-different-deployment-models-powershell.md)
* [Hogyan toouse hello API Inspector tootrace meghívja az Azure API Management](api-management-howto-api-inspector.md)

[api-management-using-vnet-menu]: ./media/api-management-using-with-vnet/api-management-menu-vnet.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-type.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-using-with-vnet/api-management-using-vnet-add-api.png
[api-management-vnet-private]: ./media/api-management-using-with-vnet/api-management-vnet-private.png
[api-management-vnet-public]: ./media/api-management-using-with-vnet/api-management-vnet-public.png

[Enable VPN connections]: #enable-vpn
[Connect tooa web service behind VPN]: #connect-vpn
[Related content]: #related-content

[UDRs]: ../virtual-network/virtual-networks-udr-overview.md
[Network Security Group]: ../virtual-network/virtual-networks-nsg.md
