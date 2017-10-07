---
title: "aaaHow toouse Azure API Management belső virtuális hálózattal |} Microsoft Docs"
description: "Megtudhatja, hogyan toosetup és konfigurálása az Azure API Management belső virtuális hálózat."
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
ms.openlocfilehash: 8d25de14e0c0bebe7ba7b47ca432ea4e45dde312
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-api-management-service-with-internal-virtual-network"></a>A belső virtuális hálózattal Azure API Management szolgáltatással
Azure virtuális hálózatokról (Vnetekről) az API Management API-k hello Internet nem érhető el tud kezelni. VPN technológiáin számos elérhető toomake hello kapcsolat, és az API Management hello virtuális hálózaton belül két fő módban telepíthető:
* Külső
* Belső

## <a name="overview"></a>Áttekintés
Az API Management egy belső virtuális hálózat módban telepítésekor minden hello Szolgáltatásvégpontok (átjáró, fejlesztői portálján, publisher portált, közvetlen felügyelet és Git) láthatók csak való hozzáférés szabályozása virtuális hálózaton belül. Hello Szolgáltatásvégpontok egyike a hello nyilvános DNS-kiszolgáló van regisztrálva.

Az API Management belső módban, érhet el a következő forgatókönyvek hello
* 3. fél kívül, pont-pont vagy ExpressRoute VPN-kapcsolatok használatával érhető el a privát adatközpontban-környezetben üzemeltetett API biztonságosan ellenőrizze.
* Engedélyezze a hibrid felhős rendszerekben jelentkezik, mintha a API-k felhőalapú és helyszíni API-k közös átjárón keresztül.
* Az átjáró egyetlen végpont használatával több földrajzi helyeken található üzemeltetett API kezelése. 

## <a name="enable-vpn"></a>Az API Management belső virtuális hálózat létrehozása
hello API-kezelés szolgáltatás a belső virtuális hálózatban egy belső terheléselosztási Balancer(ILB) mögött helyezkedik el. hello hello ILB IP-címe van hello [RFC1918](http://www.faqs.org/rfcs/rfc1918.html) tartományon.  

### <a name="enable-vnet-connection-using-azure-portal"></a>Azure-portál használatával VNET-kapcsolat engedélyezése
Először hozzon létre hello API-kezelés szolgáltatás hello lépéseket követve [létrehozása API-kezelés szolgáltatás][Create API Management service]. Majd beállításról API Management toobe rendszerbe a virtuális hálózaton belül.

![Belső virtuális hálózat APIM beállításának menü][api-management-using-internal-vnet-menu]

Miután hello központi telepítés sikeres, meg kell jelennie hello belső virtuális IP-címet, a szolgáltatás hello irányítópulton.

![Belső virtuális hálózaton konfigurált API-kezelési irányítópult][api-management-internal-vnet-dashboard]

### <a name="enable-vnet-connection-using-powershell-cmdlets"></a>Powershell-parancsmagok használatával VNET-kapcsolat engedélyezése
VNET-kapcsolatot hello PowerShell-parancsmagok használatával engedélyezheti.

* **A VNETEN belül az API Management szolgáltatás létrehozása**: hello parancsmaggal [New-AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) toocreate az Azure API Management szolgáltatás a VNETEN belül, és állítsa be toouse hello belső virtuális hálózat típusát.

* **A VNETEN belül az meglévő API Management-szolgáltatások üzembe**: hello parancsmaggal [frissítés-AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) toomove egy meglévő Azure API Management szolgáltatást egy virtuális hálózaton belül, és konfigurálja toouse Belső virtuális hálózat típusát.

## <a name="apim-dns-configuration"></a>DNS-konfiguráció
Ha az API Management külső virtuális hálózati módban, Azure DNS kezeli. Belső virtuális hálózat mód hogy toomanage a saját DNS.

> [!NOTE]
> API-kezelés szolgáltatás nem figyel toorequests hamarosan IP-címeket. Csak reagáljon toorequests toohello állomásnév konfigurált szolgáltatás végpontját (amely tartalmazza az átjáró, fejlesztői portálján, publisher portált, közvetlen felügyelet végpont és git).

### <a name="access-on-default-host-names"></a>Alapértelmezett állomásnevek hozzáférést:
Amikor létrehoz egy API-kezelés szolgáltatás nyilvános Azure felhőben, például a "contoso" nevű, hello következő végpontok által konfigurált alapértelmezett.

>   Átjáró / Proxy - contoso.azure-api.net

> Közzétevő és fejlesztői portálhoz - contoso.portal.azure-api.net

> Közvetlen felügyelet végpont - contoso.management.azure-api.net

>   GIT - contoso.scm.azure-api.net

tooaccess ezeket API-kezelés szolgáltatás a végpontokat, létrehozhat egy virtuális gép van telepítve az API Management kapcsolódó alhálózati toohello virtuális hálózatban. A szolgáltatás belső virtuális IP-cím hello feltéve 10.0.0.5, megteheti az alábbiak szerint hello állomások fájlhozzárendelést (% SystemDrive%\drivers\etc\hosts):

> 10.0.0.5 contoso.azure-api.net

> 10.0.0.5 contoso.portal.azure-api.net

> 10.0.0.5 contoso.management.azure-api.net

> 10.0.0.5 contoso.scm.azure-api.net

A létrehozott virtuális gép hello majd hello szolgáltatás végpontjai érheti el. Ha egy egyéni DNS-kiszolgáló használatával egy virtuális hálózatban, is A DNS-rekordok létrehozása, és ezeket a végpontokat hozzáférni bárhonnan virtuális hálózatban. 

### <a name="access-on-custom-domain-names"></a>Egyéni tartománynevek hozzáférést:
Ha nem szeretné tooaccess hello hello alapértelmezett állomásnevek API Management szolgáltatást, beállíthatja az összes a végpontok például alatt használt egyéni tartománynevek

![Egyéni tartománynév beállítása az API Management][api-management-custom-domain-name]

Ezután létrehozhat egy rekordot a DNS-kiszolgáló tooaccess ezeket a végpontokat, amelyek csak a virtuális hálózaton belülről érhetők el.

## <a name="related-content"></a>Kapcsolódó tartalom
* [Általános hálózati konfigurációs problémák virtuális APIM beállítása közben][Common Network Configuration Issues]
* [Virtuális hálózat – gyakori kérdések](../virtual-network/virtual-networks-faq.md)
* [Létrehozása A rekord a DNS-ben](https://msdn.microsoft.com/en-us/library/bb727018.aspx)

[api-management-using-internal-vnet-menu]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-menu.png
[api-management-internal-vnet-dashboard]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-dashboard.png
[api-management-custom-domain-name]: ./media/api-management-using-with-internal-vnet/api-management-custom-domain-name.png

[Create API Management service]: api-management-get-started.md#create-service-instance
[Common Network Configuration Issues]: api-management-using-with-vnet.md#network-configuration-issues
