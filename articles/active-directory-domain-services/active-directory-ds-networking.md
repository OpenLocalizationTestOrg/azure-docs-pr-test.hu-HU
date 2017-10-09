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
ms.openlocfilehash: 804d4ea7d1b3b07b6d224855c7adb90bdfe24022
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="networking-considerations-for-azure-ad-domain-services"></a>Azure AD tartományi szolgáltatások hálózati szempontjai
## <a name="how-tooselect-an-azure-virtual-network"></a>Hogyan tooselect Azure virtuális hálózat
hello következő irányelvek segítségével válassza ki a virtuális hálózati toouse az Azure AD tartományi szolgáltatásokkal.

### <a name="type-of-azure-virtual-network"></a>Az Azure virtuális hálózat típusát
* Engedélyezheti a klasszikus Azure virtuális hálózatot az Azure AD tartományi szolgáltatásokat.
* Azure AD tartományi szolgáltatások **nem lehet engedélyezni a Azure Resource Manager használatával létrehozott virtuális hálózatok**.
* A Resource Manager-alapú virtuális hálózat tooa klasszikus virtuális hálózatot, amelyben engedélyezve van az Azure AD tartományi szolgáltatások csatlakozhatnak. Ezt követően hello Resource Manager-alapú virtuális hálózat az Azure AD tartományi szolgáltatások is használhatja. További információkért lásd: hello [hálózati kapcsolatra](active-directory-ds-networking.md#network-connectivity) szakasz.
* **Regionális virtuális hálózatokba**: Ha azt tervezi, hogy a meglévő virtuális hálózat toouse, győződjön meg arról, hogy az regionális virtuális hálózatot.

  * Virtuális hálózatok, amelyek hello örökölt affinitáscsoport-mechanizmust alkalmazzák nem használható az Azure AD tartományi szolgáltatásokkal.
  * Azure AD tartományi szolgáltatások toouse [örökölt virtuális hálózatokat tooregional virtuális hálózatok áttelepítése](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

### <a name="azure-region-for-hello-virtual-network"></a>Az Azure-régió hello virtuális hálózat
* Az Azure AD tartományi szolgáltatások által kezelt tartomány telepítve van egy Azure-régióban van hello tooenable hello szolgáltatást a választott virtuális hálózat hello.
* Válasszon egy virtuális hálózatot az Azure AD tartományi szolgáltatások által támogatott Azure-régióba.
* Lásd: hello [Azure-szolgáltatások régiónként](https://azure.microsoft.com/regions/#services/) lap tooknow hello Azure-régiók, amelyben az Azure AD tartományi szolgáltatások érhető el.

### <a name="requirements-for-hello-virtual-network"></a>Hello virtuális hálózatra vonatkozó követelmények
* **Közelségi kapcsolat tooyour Azure munkaterhelések**: válassza ki a virtuális hálózat, amely jelenleg futtatja/elérő tooAzure AD tartományi szolgáltatások kell virtuális gépeket fog üzemeltetni hello.
* **Egyéni/bring-a-saját DNS-kiszolgálók**: Győződjön meg arról, hogy nincsenek-e nincsenek egyéni DNS-kiszolgálók konfigurálva hello virtuális hálózat.
* **A meglévő tartományok hello ugyanazon a néven**: Győződjön meg arról, hogy nem rendelkezik egy meglévő tartomány hello ugyanazon a néven érhető el a virtuális hálózat. Például során feltételezzük, hogy a "contoso.com" már elérhető nevű hello kijelölt virtuális hálózati tartományhoz. Később, próbálja meg az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz való hello tooenable azonos tartománynevet ("contoso.com") a virtuális hálózat. Tooenable az Azure AD tartományi szolgáltatások közben hibát tapasztal. Ez a hiba van a virtuális hálózat hello tartománynév tooname ütközések miatt. Ebben az esetben egy másik nevet tooset mentése az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz kell használnia. Alternatív megoldásként leépíti hello meglévő tartományhoz, és folytathatja a tooenable az Azure AD tartományi szolgáltatások.

> [!WARNING]
> Tartományi szolgáltatások tooa másik virtuális hálózatot nem helyezhető át, miután engedélyezte a hello szolgáltatást.
>
>

## <a name="network-security-groups-and-subnet-design"></a>Hálózati biztonsági csoportok és alhálózat kialakítása
A [hálózati biztonsági csoport (NSG)](../virtual-network/virtual-networks-nsg.md) , amelyek engedélyezik vagy megtagadják a hálózati forgalom tooyour Virtuálisgép-példány egy virtuális hálózati hozzáférés-vezérlési lista (ACL) szabályok listáját tartalmazza. Az NSG-ket alhálózatokhoz vagy az alhálózaton belüli virtuálisgép-példányokhoz lehet hozzárendelni. Amikor egy NSG-t hozzárendelnek egy alhálózathoz, hello ACL szabályok érvényessé válnak tooall hello Virtuálisgép-példány alhálózaton. Ezenkívül forgalom tooan adott virtuális Gépre is lehet korlátozni további korlátozásokat NGS társítása közvetlenül a virtuális gép toothat.

![Ajánlott alhálózat kialakítása](./media/active-directory-domain-services-design-guide/vnet-subnet-design.png)

### <a name="best-practices-for-choosing-a-subnet"></a>Egy alhálózat kiválasztására vonatkozó ajánlott eljárásokat
* Központi telepítése az Azure AD tartományi szolgáltatások tooa **dedikált alhálózati külön** Azure virtuális hálózaton belül.
* Ne alkalmazza az NSG-k toohello dedikált IP-alhálózatot a felügyelt tartományok. Ha telepítenie kell az NSG-ket dedikált toohello alhálózati, győződjön meg arról, **nem hello portok szükséges tooservice letiltása, és a tartomány kezelése**.
* Nem túlságosan korlátozhatja a felügyelt tartományok dedikált hello alhálózaton belül elérhető IP-címek hello száma. Ez a korlátozás megakadályozza, hogy hello szolgáltatás két tartományvezérlő elérhető-e a felügyelt tartományok.
* **Ne engedélyezze az átjáró alhálózatának hello Azure AD tartományi szolgáltatások** a virtuális hálózat.

> [!WARNING]
> Amikor társít egy NSG-t egy alhálózattal, amelyben az Azure AD tartományi szolgáltatások engedélyezve van, előfordulhat, hogy a Microsoft képes tooservice megzavarhatják és hello tartomány kezelése. Emellett az Azure AD-bérlő és a felügyelt tartományok közötti szinkronizálás megszakad. **hello SLA nem érvényes toodeployments, ahol az NSG telepítve van, amely blokkolja az Azure AD tartományi szolgáltatások frissítése és a tartomány kezelése.**
>
>

### <a name="ports-required-for-azure-ad-domain-services"></a>Az Azure AD tartományi szolgáltatásokhoz szükséges portok
hello következő portok szükségesek az Azure AD tartományi szolgáltatások tooservice és a felügyelt tartományok karbantartása. Győződjön meg arról, hogy ezeket a portokat, amelyben a felügyelt tartományok engedélyezett hello alhálózat nem blokkolja-e.

| Portszám | Cél |
| --- | --- |
| 443 |Az Azure AD-bérlő-szinkronizálás |
| 3389 |A tartomány kezelése |
| 5986 |A tartomány kezelése |
| 636 |Biztonságos LDAP (LDAPS) hozzáférési tooyour által felügyelt tartományokhoz |

### <a name="sample-nsg-for-virtual-networks-with-azure-ad-domain-services"></a>NSG a minta virtuális hálózatok az Azure AD tartományi szolgáltatások
a következő táblázat hello is konfigurálhatja a virtuális hálózat az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz NSG minta mutatja be. Ez a szabály lehetővé teszi a fent megadott portok tooensure hello érkező bejövő forgalom a felügyelt tartományok nem lett, frissítése és a Microsoft által is figyelhetők. hello alapértelmezett "DenyAll" szabály tooall egyéb bejövő forgalmát hello az interneten.

Emellett hello NSG-t is bemutatja, hogyan biztonságos LDAP hozzáférés keresztül toolock hello internet. Ez a szabály kihagyása nincs engedélyezve a biztonságos LDAP hozzáférés tooyour által kezelt tartomány keresztül hello az internet. hello NSG tartalmaz egy TCP-porton keresztül 636 csak egy megadott készletből IP-címek bejövő LDAPS hozzáférést engedélyező szabályokat. hello NSG szabály tooallow LDAPS hozzáférés hello keresztül megadott IP-címekről érkező internetes rendelkezik magasabb prioritású, mint hello DenyAll NSG-szabály.

![A minta NSG toosecure LDAPS hozzáférés keresztül hello internet](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

**További információ** - [hálózati biztonsági csoport létrehozása](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).


## <a name="network-connectivity"></a>Hálózati kapcsolat
Az Azure-ban egy klasszikus virtuális hálózaton belül csak akkor engedélyezhető az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz. Azure Resource Manager használatával létrehozott virtuális hálózatok nem támogatottak.

### <a name="scenarios-for-connecting-azure-networks"></a>Csatlakozás Azure-hálózatok forgatókönyvei
Csatlakozás az Azure virtuális hálózatok toouse hello által kezelt tartomány egyik sem szerepel a következő telepítési helyzetekben hello:

#### <a name="use-hello-managed-domain-in-more-than-one-azure-classic-virtual-network"></a>Használjon hello felügyelt tartomány egynél több Azure klasszikus virtuális hálózatban
Csatlakozhat a többi Azure klasszikus virtuális hálózatok toohello Azure klasszikus virtuális hálózatot, amelyben engedélyezte az Azure AD tartományi szolgáltatásokat. A VPN-kapcsolat lehetővé teszi toouse hello által kezelt tartomány és a többi virtuális hálózatok üzembe helyezett munkaterhelések tekintetében.

![Klasszikus virtuális hálózati kapcsolat](./media/active-directory-domain-services-design-guide/classic-vnet-connectivity.png)

#### <a name="use-hello-managed-domain-in-a-resource-manager-based-virtual-network"></a>Hello felügyelt tartomány Resource Manager-alapú virtuális hálózat használata
Csatlakoztathatja egy Azure Resource Manager-alapú virtuális hálózat toohello klasszikus virtuális hálózatot, amelyben engedélyezte az Azure AD tartományi szolgáltatásokat. Ez a kapcsolat lehetővé teszi toouse hello által kezelt tartomány a hello Resource Manager-alapú virtuális hálózat üzembe helyezett munkaterhelések tekintetében a.

![Erőforrás-kezelő tooclassic virtuális hálózati kapcsolat](./media/active-directory-domain-services-design-guide/classic-arm-vnet-connectivity.png)

### <a name="network-connection-options"></a>A hálózati kapcsolati beállítások
* **Pont-pont VPN-kapcsolattal VNet – VNet kapcsolatokhoz**: Csatlakozás a virtuális hálózati tooanother virtuális hálózatot (VNet – VNet) hasonló tooconnecting egy virtuális hálózati tooan helyszíni hely. Mindkét csatlakozási típusok használja a VPN-átjáró tooprovide IPsec/IKE használatával biztonságos alagutat.

    ![Virtuális hálózati kapcsolat segítségével a VPN-átjáró](./media/active-directory-domain-services-design-guide/vnet-connection-vpn-gateway.jpg)

    [További információ – segítségével a VPN-átjáró virtuális hálózatok csatlakoztatása](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)
* **VNet – VNet kapcsolatokhoz használó virtuális hálózati társviszony-létesítés**: virtuális hálózati társviszony-létesítés egy olyan mechanizmus, amely összeköti a két virtuális hálózatok hello ugyanabban a régióban hello Azure gerincét a hálózaton keresztül. Amennyiben nincsenek társviszonyban, hello két virtuális hálózat egyik minden kapcsolat célra jelennek meg. A kezelésük továbbra is külön erőforrásként történik, de az ezekbe a virtuális hálózatokba tartozó virtuális gépek közvetlenül, magánhálózati IP-címekkel kommunikálhatnak egymással.

    ![Virtuális hálózati kapcsolat használata a társviszony-létesítés](./media/active-directory-domain-services-design-guide/vnet-peering.png)

    [További információ – a virtuális hálózati társviszony-létesítés](../virtual-network/virtual-network-peering-overview.md)

<br>

## <a name="related-content"></a>Kapcsolódó tartalom
* [Az Azure virtuális hálózati társviszony-létesítés](../virtual-network/virtual-network-peering-overview.md)
* [Hello klasszikus telepítési modell VNet – VNet kapcsolat konfigurálása](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)
* [Az Azure hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md)
* [Hálózati biztonsági csoport létrehozása](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
