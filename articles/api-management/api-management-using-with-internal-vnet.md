---
title: "Belső virtuális hálózat Azure API Management használata |} Microsoft Docs"
description: "Megtudhatja, hogyan telepítése és konfigurálása az Azure API Management belső virtuális hálózat."
services: api-management
documentationcenter: 
author: solankisamir
manager: kjoshi
editor: 
ms.assetid: dac28ccf-2550-45a5-89cf-192d87369bc3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 55248387c7e78d05c1cf1afd615b7b921e9669d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-api-management-service-with-internal-virtual-network"></a>A belső virtuális hálózattal Azure API Management szolgáltatással
Azure virtuális hálózatokról (Vnetekről) az API Management kezelhet API-k nem érhető el, az interneten. VPN technológiáin számos érhetők el a kapcsolatot, és az API Management állítható rendszerbe a virtuális hálózaton belül két fő módban:
* Külső
* Belső

## <a name="overview"> </a>Áttekintés
Az API Management telepítésekor egy belső virtuális hálózat módban minden szolgáltatásvégpontokra (átjáró, fejlesztői portálján, publisher portált, közvetlen felügyelet és Git) csak egy virtuális hálózatban, amelyek látható a hozzáférést. A végpontok egyike a nyilvános DNS-kiszolgálón van regisztrálva.

Az API Management belső módban, érhető el az alábbi esetekben
* 3. fél kívül, pont-pont vagy ExpressRoute VPN-kapcsolatok használatával érhető el a privát adatközpontban-környezetben üzemeltetett API biztonságosan ellenőrizze.
* Engedélyezze a hibrid felhős rendszerekben jelentkezik, mintha a API-k felhőalapú és helyszíni API-k közös átjárón keresztül.
* Az átjáró egyetlen végpont használatával több földrajzi helyeken található üzemeltetett API kezelése. 

## <a name="enable-vpn"></a>Az API Management belső virtuális hálózat létrehozása
Az API Management szolgáltatás belső virtuális hálózatban egy belső terheléselosztási Balancer(ILB) mögött helyezkedik el. A ILB IP-címe van a [RFC1918](http://www.faqs.org/rfcs/rfc1918.html) tartományon.  

### <a name="enable-vnet-connection-using-azure-portal"></a>Azure-portál használatával VNET-kapcsolat engedélyezése
Először létre kell hoznia az API-kezelés szolgáltatás a lépéseket követve [létrehozása API-kezelés szolgáltatás][Create API Management service]. Majd beállításról API-kezelés szolgáltatás telepíthető egy virtuális hálózaton belül.

![Belső virtuális hálózat APIM beállításának menü][api-management-using-internal-vnet-menu]

Miután a telepítés sikeres, a szolgáltatás belső virtuális IP-címe megjelenik az irányítópulton.

![Belső virtuális hálózaton konfigurált API-kezelési irányítópult][api-management-internal-vnet-dashboard]

### <a name="enable-vnet-connection-using-powershell-cmdlets"></a>Powershell-parancsmagok használatával VNET-kapcsolat engedélyezése
VNET-kapcsolatot a PowerShell-parancsmagok használatával engedélyezheti.

* **A VNETEN belül az API Management szolgáltatás létrehozása**: parancsmag [New-AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) hozzon létre egy Azure API Management szolgáltatást egy VNETEN belül, és konfigurálja úgy, hogy a belső hálózatok típusával.

* **A VNETEN belül az meglévő API Management-szolgáltatások üzembe**: parancsmag [frissítés-AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) helyezze át egy meglévő Azure API Management szolgáltatást egy virtuális hálózaton belül, és konfigurálja úgy, hogy használata Belső virtuális hálózat típusát.

## <a name="apim-dns-configuration"></a>DNS-konfiguráció
Ha az API Management külső virtuális hálózati módban, Azure DNS kezeli. Belső virtuális hálózat mód felügyelni a saját DNS kell.

> [!NOTE]
> API-kezelés szolgáltatás nem figyel az IP-címek érkező kérésekre. Csak a konfigurált szolgáltatás végpontját az állomásnév (amely tartalmazza az átjáró, fejlesztői portálján, publisher portált, közvetlen felügyelet végpont és git) kérelmére reagálás.

### <a name="access-on-default-host-names"></a>Alapértelmezett állomásnevek hozzáférést:
Amikor létrehoz egy API-kezelés szolgáltatás nyilvános Azure felhőben, például a "contoso" nevű, a következő végpontok alapértelmezés szerint vannak konfigurálva.

>   Átjáró / Proxy - contoso.azure-api.net

> Közzétevő és fejlesztői portálhoz - contoso.portal.azure-api.net

> Közvetlen felügyelet végpont - contoso.management.azure-api.net

>   GIT - contoso.scm.azure-api.net

API-kezelés szolgáltatás a végpontokkal eléréséhez hozzon létre egy virtuális gépet a virtuális hálózathoz van telepítve az API Management alhálózat. A szolgáltatás 10.0.0.5 érték, ha a belső virtuális IP-címet, elvégezhető a gazdagép (% SystemDrive%\drivers\etc\hosts) leképezési fájl az alábbiak szerint:

> 10.0.0.5 contoso.azure-api.net

> 10.0.0.5 contoso.portal.azure-api.net

> 10.0.0.5 contoso.management.azure-api.net

> 10.0.0.5 contoso.scm.azure-api.net

Ezután hozzáférhetnek a Szolgáltatásvégpontok létrehozott virtuális gépről. Ha egy egyéni DNS-kiszolgáló használatával egy virtuális hálózatban, is A DNS-rekordok létrehozása, és ezeket a végpontokat hozzáférni bárhonnan virtuális hálózatban. 

### <a name="access-on-custom-domain-names"></a>Egyéni tartománynevek hozzáférést:
Ha nem szeretné, az API Management szolgáltatás az alapértelmezett állomásneveket eléréséhez, beállíthatja az összes a végpontok például alatt használt egyéni tartománynevek

![Egyéni tartománynév beállítása az API Management][api-management-custom-domain-name]

Majd hozhat létre A rekordokat a DNS-kiszolgáló eléréséhez ezeket a végpontokat, amelyek csak a virtuális hálózaton belülről érhetők el.

## <a name="related-content"></a>Kapcsolódó tartalom
* [Általános hálózati konfigurációs problémák virtuális APIM beállítása közben][Common Network Configuration Issues]
* [Virtuális hálózat – gyakori kérdések](../virtual-network/virtual-networks-faq.md)
* [Létrehozása A rekord a DNS-ben](https://msdn.microsoft.com/en-us/library/bb727018.aspx)

[api-management-using-internal-vnet-menu]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-menu.png
[api-management-internal-vnet-dashboard]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-dashboard.png
[api-management-custom-domain-name]: ./media/api-management-using-with-internal-vnet/api-management-custom-domain-name.png

[Create API Management service]: api-management-get-started.md#create-service-instance
[Common Network Configuration Issues]: api-management-using-with-vnet.md#network-configuration-issues
