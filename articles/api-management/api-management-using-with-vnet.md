---
title: "Virtuális hálózatok az Azure API Management használata"
description: "Útmutató a kapcsolatot az Azure API Management és az access webszolgáltatások rajta egy virtuális hálózathoz."
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
ms.openlocfilehash: 268cee739188d81feffc36ac07fcdfa18ff95a4d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-azure-api-management-with-virtual-networks"></a>Virtuális hálózatok az Azure API Management használata
Az Azure virtuális hálózatokról (Vnetekről) helyezze el az Azure-erőforrások bármelyike nem internetes routeable hálózati hozzáférést szabályozó teszik lehetővé. Ezek a hálózatok csatlakozhatnak különböző VPN technológiáin a helyszíni hálózatokhoz. További információk az Azure virtuális hálózatok indítsa el az adatok Itt további: [Azure virtuális hálózat áttekintése](../virtual-network/virtual-networks-overview.md).

Az Azure API Management telepítheti a virtuális hálózaton (VNET), így hozzáférhet a hálózaton belüli háttér-szolgáltatások. A fejlesztői portálján és API-átjárón beállítható úgy, hogy érhető el, vagy az internetről, vagy csak a virtuális hálózaton belül.

> [!NOTE]
> Az Azure API Management támogatja a classic és az Azure Resource Manager Vnetek.
>
>

## <a name="enable-vpn"></a>Kapcsolatcsoporttal engedélyezése
> [!NOTE]
> VNET-kapcsolatot is elérhető a **prémium** és **fejlesztői** rétegek. A rétegek közötti váltáshoz nyissa meg az API Management szolgáltatás az Azure-portálon, és nyissa meg a **és az árképzés** fülre. Az a **tarifacsomag** szakaszt, válassza ki a támogatási vagy fejlesztői réteget, és kattintson a Mentés gombra.
>

Ahhoz, hogy a VNET-kapcsolatot, nyissa meg az API Management szolgáltatás az Azure portál, és nyissa meg a **virtuális hálózati** lap.

![Virtuális hálózati menü API Management][api-management-using-vnet-menu]

Válassza ki a kívánt hozzáférés típusát:

* **Külső**: az API Management-átjáró és a fejlesztői portálon keresztül egy külső terheléselosztó a nyilvános interneten keresztül érhetők el. Az átjáró a virtuális hálózaton lévő erőforrások eléréséhez.

![Nyilvános társviszony-létesítés][api-management-vnet-public]

* **Belső**: az API Management-átjáró és a fejlesztői portálon csak érhetők el a belső terheléselosztók használatával a virtuális hálózaton belül. Az átjáró a virtuális hálózaton lévő erőforrások eléréséhez.

![Magánhálózati társviszony-létesítés][api-management-vnet-private]

Ekkor minden régióban, ahol az API Management szolgáltatás ki van építve listáját. Válassza ki a virtuális hálózat és alhálózat minden régióhoz. A lista elkészült a klasszikus és Resource Manager virtuális hálózatok az Azure-előfizetések, amelyek a telepítő konfigurálja a régióban elérhető.

> [!NOTE]
> **Szolgáltatásvégpont** a fenti ábrán például a átjáró /-Proxy, Publisher Portal, fejlesztői portálján, GIT, és a közvetlen felügyelet végpont.
> **Felügyeleti végpont** a fenti ábrán a végpont-konfiguráció Azure-portál és a Powershell segítségével kezelheti a szolgáltatásban üzemeltetett.
> Vegye figyelembe azt is, amely akkor is, ha az ábrán látható IP-címek a különböző végpontokhoz, az API Management szolgáltatás **csak** reagál a beállított állomásnevek a.

> [!IMPORTANT]
> Az Azure API Management-példány egy erőforrás-kezelő virtuális hálózatba való telepítésekor a szolgáltatás az Azure API Management példányok kivételével nincs más erőforrásokat tartalmazó dedikált alhálózat kell lennie. Ha az Azure API Management-példányt telepítése egy erőforrás-kezelő virtuális hálózat alhálózathoz tett kísérlet, amely más erőforrások, a telepítés meghiúsul.
>
>

![Jelölje ki a VPN][api-management-setup-vpn-select]

Kattintson a **mentése** a képernyő tetején.

> [!NOTE]
> Az API Management-példány a címe változik minden alkalommal, amikor virtuális hálózat engedélyezve vagy letiltva.  
> A virtuális IP-cím is megváltozik az API Management mozgatásakor **külső** való **belső** vagy fordítva
>


> [!IMPORTANT]
> Ha az API Management eltávolítása egy VNETET, vagy módosítsa a telepítik egy, a korábban használt virtuális hálózat maradjanak zárolt legfeljebb 4 órán keresztül. Ebben az időszakban, nem lesz lehetséges a virtuális hálózat törlése vagy a központi telepítése egy új erőforrást.

## <a name="enable-vnet-powershell"></a>Engedélyezése kapcsolatcsoporttal PowerShell-parancsmagok használatával
Engedélyezheti a VNET-kapcsolatot a PowerShell-parancsmagok használatával

* **A VNETEN belül az API Management szolgáltatás létrehozása**: parancsmag [New-AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) egy VNETEN belül Azure API Management szolgáltatás létrehozása.

* **A VNETEN belül az meglévő API Management-szolgáltatások üzembe**: parancsmag [frissítés-AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) helyezhető át egy meglévő Azure API Management szolgáltatást egy virtuális hálózaton belül.

## <a name="connect-vnet"></a>Csatlakozás a virtuális hálózaton belül futó webszolgáltatás
Az API-kezelés szolgáltatás a virtuális hálózatba csatlakoztatása után belül háttér-szolgáltatások eléréséhez szükséges eltér nem nyilvános szolgáltatások eléréséhez szükséges. Csak a helyi IP-cím vagy a webszolgáltatás a állomásneve (Ha a virtuális hálózat DNS-kiszolgáló van konfigurálva) írja be a **webszolgáltatás URL-címe** mezőben egy új API létrehozásakor vagy egy meglévőt.

![A VPN API hozzáadása][api-management-setup-vpn-add-api]

## <a name="network-configuration-issues"></a>Közös hálózati konfigurációs problémák
Az alábbiakban az API-kezelés szolgáltatás telepítése virtuális hálózatba során előforduló gyakori helytelen konfiguráció-problémák listáját.

* **Egyéni DNS-kiszolgáló beállításainak**: az API Management szolgáltatás több Azure-szolgáltatások függ. Amikor az API Management egyéni DNS-kiszolgáló a VNETEN belül üzemel kell feloldani a gazdagép neve az Azure szolgáltatások. Kövesse az [ez](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) egyéni DNS-beállításainak útmutatást. Tekintse meg a portokat az alábbi táblázat és az egyéb hálózati követelmények hivatkozás.

> [!IMPORTANT]
> Javasoljuk, hogy egy egyéni DNS-kiszolgálói használatakor a vnet beállítása, amely **előtt** bele az API Management szolgáltatás telepítéséhez. Ellenkező esetben frissítenie kell az API Management szolgáltatás minden alkalommal, amikor a DNS-kiszolgálók (s) módosításához futtassa a [alkalmazni a hálózati konfigurációs műveletet](https://docs.microsoft.com/en-us/rest/api/apimanagement/apimanagementservice#ApiManagementService_ApplyNetworkConfigurationUpdates)

* **Az API Management szükséges portok**: az API Management telepítése alhálózat a bejövő és kimenő forgalom vezérelhető [hálózati biztonsági csoport][Network Security Group]. Ha ezeket a portokat bármelyike nem érhető el, az API Management nem fog megfelelően működni, és lehetnek nem elérhetők. Rendelkezik egy vagy több ezeket a portokat blokkolva helytelen konfigurálása más gyakori problémák használata esetén az API Management olyan virtuális hálózaton.

Ha egy API-kezelés szolgáltatás példányát a VNETEN belül üzemel a portokat az alábbi táblázatban használ.

| Forrás / cél port(ok) | Irány | Átviteli protokoll | Cél | Forrás / cél | Hozzáférés típusa |
| --- | --- | --- | --- | --- | --- |
| * / 80, 443 |Bejövő |TCP |Ügyfél-kommunikációt kíván API Management |INTERNET / VIRTUAL_NETWORK |Külső |
| * / 3443 |Bejövő |TCP |Az Azure portál és a Powershell felügyeleti végpont |INTERNET / VIRTUAL_NETWORK |Külső és belső |
| * / 80, 443 |Kimenő |TCP |Az Azure Storage és az Azure Service Bus függőség |VIRTUAL_NETWORK / INTERNET |Külső és belső |
| * / 1433 |Kimenő |TCP |Az Azure SQL függőség |VIRTUAL_NETWORK / INTERNET |Külső és belső |
| * / 11000 - 11999 |Kimenő |TCP |V12 Azure SQL függőség |VIRTUAL_NETWORK / INTERNET |Külső és belső |
| * / 14000 - 14999 |Kimenő |TCP |V12 Azure SQL függőség |VIRTUAL_NETWORK / INTERNET |Külső és belső |
| * / 5671 |Kimenő |AMQP |Az Event Hubs házirend- és figyelési ügynök napló függőség |VIRTUAL_NETWORK / INTERNET |Külső és belső |
| 6381 - 6383 / 6381 - 6383 |Bejövő és kimenő |UDP |Redis gyorsítótár függőség |VIRTUAL_NETWORK / VIRTUAL_NETWORK |Külső és belső |-
| * / 445 |Kimenő |TCP |Azure-fájlmegosztáshoz git függőség |VIRTUAL_NETWORK / INTERNET |Külső és belső |
| * / * | Bejövő |TCP |Az Azure infrastruktúra Terheléselosztóját | AZURE_LOAD_BALANCER / VIRTUAL_NETWORK |Külső és belső |

* **SSL-funkció**: SSL-tanúsítvány láncának felépítése és -ellenőrzés az API Management engedélyezéséhez szolgáltatásnak kell a kimenő hálózati kapcsolat ocsp.msocsp.com, mscrl.microsoft.com és crl.microsoft.com. A függőség nincs szükség, ha bármely olyan tanúsítvány, az API Management-be feltölteni a teljes lánc és a legfelső szintű tartalmaz.

* **DNS hozzáférési**: 53-as port kimenő hozzáférést kell DNS-kiszolgálókkal való kommunikációhoz. Ha egy egyéni DNS-kiszolgáló létezik a VPN-átjáró másik végén, a DNS-kiszolgáló elérhető-e az API Management üzemeltető alhálózatból kell lennie.

* **Metrikák és az állapotfigyelés**: Azure figyelési végpontok, amelyek oldja meg a következő tartományokkal a kimenő hálózati kapcsolat: global.metrics.nsatc.net, shoebox2.metrics.nsatc.net, prod3.metrics.nsatc.net.

* **Útvonal gyorstelepítése**: egy közös felhasználói konfigurálás az, hogy a saját alapértelmezett útvonalat (0.0.0.0/0), amely arra kényszeríti a kimenő Internet forgalmat inkább a helyszínen. A forgalom áramlását tüntetnek megsérti kapcsolat az Azure API Management szolgáltatással, mert a kimenő adatforgalmat a letiltott helyi, vagy NAT-címek, amelyek már nem használhatók a különböző Azure-végpontok egy felismerhetetlen készletéhez lenne. A megoldás, hogy egy (vagy több) felhasználó által definiált útvonalak megadása ([udr-EK][UDRs]), amely tartalmazza az Azure API Management az alhálózaton. Egy UDR helyett az alapértelmezett útvonal szembeni szerződéses kötelezettségeket vonatkozó alhálózati útvonalakat határozza meg.
  Ha lehetséges javasoljuk, hogy az alábbi konfigurációt használja:
 * Az ExpressRoute konfigurációs hirdeti 0.0.0.0/0, és alapértelmezés szerint kényszerített bújtatja minden kimenő forgalom helyszíni.
 * Az Azure API Management tartalmazó alkalmazva UDR 0.0.0.0/0 az Internet egy következő ugrás típusa határozza meg.
 A kombinált hatását, hogy ezeket a lépéseket az, hogy az alhálózat-szintű UDR elsőbbséget élvez az ExpressRoute kényszerített bújtatás, biztosítva ezzel az Azure API Management a kimenő Internet-hozzáféréssel.

>[!WARNING]  
>Az Azure API Management használata nem támogatott az ExpressRoute-konfigurációkat, amelyek **helytelenül kereszt-hirdetményt a magánhálózati társviszony-létesítési elérési utat a nyilvános társviszony-létesítési elérési útvonalak**. ExpressRoute-konfigurációk, amelyek rendelkeznek a nyilvános társviszony konfigurálva, a Microsoft Azure IP-címtartományok számos útvonal-hirdetéseinek kap Microsoft. Ha ezen címtartomány helytelenül határokon meghirdetett a magánhálózati társviszony-létesítési elérési úton, a záró eredménye, hogy minden kimenő hálózati rendszer érkező csomagokat, az Azure API Management példány alhálózati helytelenül kényszerített-tunneled az ügyfél a helyi hálózati infrastruktúra. Hálózati folyamatot az Azure API Management megszakítja. Ez a probléma megoldása, hogy állítsa le a kereszt-hirdetési útvonalak a nyilvános társviszony-létesítési elérési útról a magánhálózati társviszony-létesítési elérési utat.


## <a name="troubleshooting"></a>Hibaelhárítása
Ha módosítja a hálózathoz, tekintse meg [NetworkStatus API](https://docs.microsoft.com/en-us/rest/api/apimanagement/networkstatus), ellenőrizze, hogy az API-kezelés szolgáltatás nem rendelkezik már nem fér hozzá a kritikus erőforrásokat, amelyek azt határozzák meg. A kapcsolati állapot 15 percenként frissíteni kell.

## <a name="limitations"></a>Korlátozásai
* Az API Management-példányokat tartalmazó alhálózat bármely más Azure-erőforrás típusa nem tartalmazhat.
* Az alhálózat és az API Management szolgáltatás ugyanahhoz az előfizetéshez kell lennie.
* Az API Management-példányokat tartalmazó alhálózat előfizetésekhez nem helyezhetők.
* Belső virtuális hálózat használatával, csak egy belső IP-cím lesz elérhető a megadott tartomány [RFC 1918](https://tools.ietf.org/html/rfc1918), egy nyilvános IP-cím nem adható meg.
* Több területi API Management telepítések esetén konfigurált, belső virtuális hálózatok a felhasználók felelőssége saját terheléselosztási, a DNS saját kezelése.


## <a name="related-content"></a>Kapcsolódó tartalom
* [A virtuális hálózati kapcsolódás kiszolgáló VPN-átjáró használatával](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti)
* [Egy virtuális hálózathoz csatlakozó különböző üzembe helyezési modellel](../vpn-gateway/vpn-gateway-connect-different-deployment-models-powershell.md)
* [A követni kívánt API-Inspector használatával meghívja az Azure API Management](api-management-howto-api-inspector.md)

[api-management-using-vnet-menu]: ./media/api-management-using-with-vnet/api-management-menu-vnet.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-type.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-using-with-vnet/api-management-using-vnet-add-api.png
[api-management-vnet-private]: ./media/api-management-using-with-vnet/api-management-vnet-private.png
[api-management-vnet-public]: ./media/api-management-using-with-vnet/api-management-vnet-public.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[UDRs]: ../virtual-network/virtual-networks-udr-overview.md
[Network Security Group]: ../virtual-network/virtual-networks-nsg.md
