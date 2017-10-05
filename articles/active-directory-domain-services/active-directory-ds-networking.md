---
title: "Azure AD tartományi szolgáltatások: Hálózat irányelvek |} Microsoft Docs"
description: "Azure Active Directory tartományi szolgáltatások hálózati szempontjai"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 23a857a5-2720-400a-ab9b-1ba61e7b145a
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: maheshu
ms.openlocfilehash: 8306c1ff72d348f5f327b79617e1422a78e26bdb
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="networking-considerations-for-azure-ad-domain-services"></a>Azure AD tartományi szolgáltatások hálózati szempontjai
## <a name="how-to-select-an-azure-virtual-network"></a>Egy Azure virtuális hálózat kiválasztása
A következő irányelvek segítségével válassza ki a virtuális hálózatot az Azure AD tartományi szolgáltatások használatára.

### <a name="type-of-azure-virtual-network"></a>Az Azure virtuális hálózat típusát
* Engedélyezheti a klasszikus Azure virtuális hálózatot az Azure AD tartományi szolgáltatásokat.
* Azure AD tartományi szolgáltatások **nem lehet engedélyezni a Azure Resource Manager használatával létrehozott virtuális hálózatok**.
* A klasszikus virtuális hálózatot, amelyben engedélyezve van az Azure AD tartományi szolgáltatások a Resource Manager-alapú virtuális hálózat csatlakozhat. Ezt követően a Resource Manager-alapú virtuális hálózat az Azure AD tartományi szolgáltatások is használhatja. További információkért lásd: a [hálózati kapcsolatra](active-directory-ds-networking.md#network-connectivity) szakasz.
* **Regionális virtuális hálózatokba**: Ha tervezi egy meglévő virtuális hálózathoz, győződjön meg arról, hogy az regionális virtuális hálózatot.

  * Azok a virtuális hálózatok, amelyek az örökölt affinitáscsoport-mechanizmust alkalmazzák, nem használhatók az Azure AD tartományi szolgáltatásokkal.
  * Azure AD tartományi szolgáltatások, [örökölt virtuális hálózatokat regionális virtuális hálózatokba kell áttelepíteni](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

### <a name="azure-region-for-the-virtual-network"></a>A virtuális hálózat az Azure-régió
* A felügyelt tartományra telepítve van a azonos Azure-régió, ahol a virtuális hálózatot az Azure AD tartományi szolgáltatások úgy dönt, hogy engedélyezze a szolgáltatást.
* Válasszon egy virtuális hálózatot az Azure AD tartományi szolgáltatások által támogatott Azure-régióba.
* [Az Azure régiói](https://azure.microsoft.com/regions/#services/) lapon találja azoknak az Azure-régióknak a felsorolását, amelyekben elérhetők az Azure AD tartományi szolgáltatások.

### <a name="requirements-for-the-virtual-network"></a>A virtuális hálózati követelményei
* **Az Azure munkaterhelések közelében**: válassza ki a virtuális hálózatot, amely jelenleg futtatja/fogja tárolni az Azure AD tartományi szolgáltatások elérését igénylő virtuális gépektől.
* **Egyéni/bring-a-saját DNS-kiszolgálók**: Győződjön meg arról, hogy nincsenek-e nincsenek egyéni DNS-kiszolgálók konfigurálva a virtuális hálózat.
* **Az azonos nevű tartomány meglévő tartományok**: Győződjön meg arról, hogy nem rendelkezik egy meglévő tartományhoz, a tartomány néven érhető el a virtuális hálózat. Tegyük fel például, hogy a kiválasztott virtuális hálózatban már van egy contoso.com nevű tartomány. Később akkor próbálja meg engedélyezni a ugyanazon tartomány nevét (az "contoso.com") a virtuális hálózat az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz. Azure AD tartományi szolgáltatások engedélyezése közben hibát tapasztal. Ez a hiba a tartománynév, a virtuális hálózatban tapasztalható névütközés okozza. Ebben az esetben az Azure AD tartományi szolgáltatások által kezelt tartomány beállításához másik nevet kell használnia. A másik lehetséges megoldás az, hogy leépíti a meglévő tartományt, majd ezután folytatja a Azure AD tartományi szolgáltatások engedélyezését.

> [!WARNING]
> Tartományi szolgáltatások nem helyezhető át egy másik virtuális hálózathoz, a szolgáltatás engedélyezése után.
>
>

## <a name="network-security-groups-and-subnet-design"></a>Hálózati biztonsági csoportok és alhálózat kialakítása
A [hálózati biztonsági csoport (NSG)](../virtual-network/virtual-networks-nsg.md) hozzáférés-vezérlési lista (ACL) szabályokat, amelyek engedélyezik vagy megtagadják a Virtuálisgép-példány egy virtuális hálózat hálózati forgalmának listáját tartalmazza. Az NSG-ket alhálózatokhoz vagy az alhálózaton belüli virtuálisgép-példányokhoz lehet hozzárendelni. Ha az NSG-t hozzárendelik egy alhálózathoz, az ACL-szabályok érvényesek lesznek az alhálózatban lévő összes virtuálisgép-példányra. Emellett az egyes virtuális gép is lehet korlátozni további korlátozásokat NGS társítása közvetlenül a virtuális gép.

![Ajánlott alhálózat kialakítása](./media/active-directory-domain-services-design-guide/vnet-subnet-design.png)

### <a name="best-practices-for-choosing-a-subnet"></a>Egy alhálózat kiválasztására vonatkozó ajánlott eljárásokat
* Az Azure AD tartományi szolgáltatások telepítése a **dedikált alhálózati külön** Azure virtuális hálózaton belül.
* Az NSG-k nem vonatkoznak a dedikált IP-alhálózatot a felügyelt tartományok. Ha a dedikált alhálózati NSG-ket kell alkalmaznia, győződjön meg arról, **nem blokkolja a szolgáltatás szükséges portokat, és a tartomány kezelése**.
* Nem túlságosan korlátozhatja a felügyelt tartományok dedikált alhálózaton belül elérhető IP-címek számát. Ez a korlátozás miatt a szolgáltatás elérhetővé teszi két tartományvezérlő a felügyelt tartományok.
* **Ne engedélyezze az átjáró alhálózatának az Azure AD tartományi szolgáltatások** a virtuális hálózat.

> [!WARNING]
> Amikor társít egy NSG-t egy alhálózattal, amelyben az Azure AD tartományi szolgáltatások engedélyezve van, a szolgáltatást, és a tartomány kezelése a Microsoft képes zavart okozhat. Emellett az Azure AD-bérlő és a felügyelt tartományok közötti szinkronizálás megszakad. **Az SLA nem vonatkozik a központi telepítések, ahol az NSG telepítve van, amely blokkolja az Azure AD tartományi szolgáltatások frissítése és a tartomány kezelése.**
>
>

### <a name="ports-required-for-azure-ad-domain-services"></a>Az Azure AD tartományi szolgáltatásokhoz szükséges portok
A következő portokat Azure AD tartományi szolgáltatások szolgáltatáshoz szükséges, és a felügyelt tartományok karbantartása. Győződjön meg arról, hogy ezek a portok nincsenek letiltva az alhálózat, ahol a felügyelt tartományok engedélyezett.

| Portszám | Cél |
| --- | --- |
| 443 |Az Azure AD-bérlő-szinkronizálás |
| 3389 |A tartomány kezelése |
| 5986 |A tartomány kezelése |
| 636 |Biztonságos LDAP (LDAPS) hozzáféréssel a felügyelt tartományhoz |

### <a name="sample-nsg-for-virtual-networks-with-azure-ad-domain-services"></a>NSG a minta virtuális hálózatok az Azure AD tartományi szolgáltatások
Az alábbi táblázat mutatja be egy minta NSG-t is konfigurálhatja a virtuális hálózat az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz. Ez a szabály lehetővé teszi, hogy a fenti megadott porttartományt, hogy a felügyelt tartományra marad érkező bejövő forgalom lett, frissítése és a Microsoft által is figyelhetők. Minden bejövő forgalom az internetről az alapértelmezett "DenyAll" szabály vonatkozik.

Emellett az NSG-t is bemutatja, hogyan biztonságos LDAP hozzáférést zárolását az interneten keresztül. A szabály mellőzése, ha nem biztonságos LDAP access számára engedélyezett a felügyelt tartományok az interneten keresztül. Az NSG tartalmaz egy TCP-porton keresztül 636 csak egy megadott készletből IP-címek bejövő LDAPS hozzáférést engedélyező szabályokat. Az NSG-szabály LDAPS hozzáférést a megadott IP-címeket az interneten keresztül rendelkezik, magasabb prioritású, mint a DenyAll NSG-szabály.

![Minta NSG LDAPS hozzáférés biztonságossá az interneten keresztül](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

**További információ** - [hálózati biztonsági csoport létrehozása](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).


## <a name="network-connectivity"></a>Hálózati kapcsolat
Az Azure-ban egy klasszikus virtuális hálózaton belül csak akkor engedélyezhető az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz. Azure Resource Manager használatával létrehozott virtuális hálózatok nem támogatottak.

### <a name="scenarios-for-connecting-azure-networks"></a>Csatlakozás Azure-hálózatok forgatókönyvei
Csatlakozás az Azure virtuális hálózatok a felügyelt tartományra, sem a következő telepítési helyzetekben:

#### <a name="use-the-managed-domain-in-more-than-one-azure-classic-virtual-network"></a>A felügyelt tartományra egynél több Azure klasszikus virtuális hálózat használata
Egyéb Azure klasszikus virtuális hálózatok csatlakozhatnak az Azure klasszikus virtuális hálózatot, amelyben engedélyezte az Azure AD tartományi szolgáltatásokat. A VPN-kapcsolat lehetővé teszi a felügyelt tartományra használ a többi virtuális hálózatok üzembe helyezett munkaterhelések tekintetében.

![Klasszikus virtuális hálózati kapcsolat](./media/active-directory-domain-services-design-guide/classic-vnet-connectivity.png)

#### <a name="use-the-managed-domain-in-a-resource-manager-based-virtual-network"></a>A felügyelt tartományra Resource Manager-alapú virtuális hálózat használata
Az Azure klasszikus virtuális hálózatot, amelyben engedélyezte az Azure AD tartományi szolgáltatásokat a Resource Manager-alapú virtuális hálózat csatlakozhat. Ez a kapcsolat lehetővé teszi a felügyelt tartományra használata a Resource Manager-alapú virtuális hálózatban üzembe helyezett munkaterhelések tekintetében.

![Erőforrás-kezelő klasszikus virtuális hálózati kapcsolat](./media/active-directory-domain-services-design-guide/classic-arm-vnet-connectivity.png)

### <a name="network-connection-options"></a>A hálózati kapcsolati beállítások
* **Pont-pont VPN-kapcsolattal VNet – VNet kapcsolatokhoz**: Csatlakozás a virtuális hálózat egy másik virtuális hálózathoz (VNet – VNet) hasonlít egy virtuális hálózathoz csatlakozik egy helyszíni hely. Mindkét kapcsolattípus egy VPN-átjárót használ a biztonságos alagút IPsec/IKE használatával való kialakításához.

    ![Virtuális hálózati kapcsolat segítségével a VPN-átjáró](./media/active-directory-domain-services-design-guide/vnet-connection-vpn-gateway.jpg)

    [További információ – segítségével a VPN-átjáró virtuális hálózatok csatlakoztatása](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)
* **VNet – VNet kapcsolatokhoz használó virtuális hálózati társviszony-létesítés**: virtuális hálózati társviszony-létesítés egy olyan mechanizmus, amely összeköti a két virtuális hálózat ugyanabban a régióban a Azure gerincét a hálózaton keresztül. A társviszony kialakítását követően a két virtuális hálózat a csatlakozás minden szempontjából egyetlen hálózatnak látszik. A kezelésük továbbra is külön erőforrásként történik, de az ezekbe a virtuális hálózatokba tartozó virtuális gépek közvetlenül, magánhálózati IP-címekkel kommunikálhatnak egymással.

    ![Virtuális hálózati kapcsolat használata a társviszony-létesítés](./media/active-directory-domain-services-design-guide/vnet-peering.png)

    [További információ – a virtuális hálózati társviszony-létesítés](../virtual-network/virtual-network-peering-overview.md)

<br>

## <a name="related-content"></a>Kapcsolódó tartalom
* [Az Azure virtuális hálózati társviszony-létesítés](../virtual-network/virtual-network-peering-overview.md)
* [A klasszikus üzembe helyezési modell VNet – VNet kapcsolat konfigurálása](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)
* [Az Azure hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md)
* [Hálózati biztonsági csoport létrehozása](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
