# Áttekintés
## [Az Azure-hálózat bemutatása](networking-overview.md)
## Architektúra
### [Virtuális adatközpontok](networking-virtual-datacenter.md)
### [Aszimmetrikus útválasztás több hálózati útvonallal](../expressroute/expressroute-asymmetric-routing.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Biztonságos hálózatkialakítások](../best-practices-network-security.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Küllős topológia](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)
### [Ajánlott hálózati biztonsági eljárások](../security/azure-security-network-security-best-practices.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Magas rendelkezésre állású virtuális hálózati készülékek](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha )
### [Többféle terheléselosztási módszer használata](../traffic-manager/traffic-manager-load-balancing-azure.md?toc=%2fazure%2fnetworking%2ftoc.json)
## Tervezés és kialakítás
### [Virtuális hálózatok](../virtual-network/virtual-network-vnet-plan-design-arm.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Forgalomszűrés](../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Létesítmények közötti kapcsolatok – VPN](../vpn-gateway/vpn-gateway-plan-design.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Létesítmények közötti kapcsolatok – dedikált privát](../expressroute/expressroute-workflows.md?toc=%2fazure%2fnetworking%2ftoc.json)

##  Alapelvek
### [Virtuális hálózatok](../virtual-network/virtual-networks-overview.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Hálózati terheléselosztás](../load-balancer/load-balancer-overview.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Alkalmazásterhelés-elosztás](../application-gateway/application-gateway-introduction.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [DNS](../dns/dns-overview.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [DNS-alapú forgalomelosztás](../traffic-manager/traffic-manager-overview.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Helyszíni kapcsolódás – VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Helyszíni kapcsolódás – dedikált](../expressroute/expressroute-introduction.md?toc=%2fazure%2fnetworking%2ftoc.json)

# Bevezetés
## [Az első virtuális hálózat létrehozása](../virtual-network/virtual-network-get-started-vnet-subnet.md?toc=%2fazure%2fnetworking%2ftoc.json)

# Útmutató
## Internetkapcsolat
### [Nyilvános kiszolgálók hálózati terheléselosztása](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Nyilvános kiszolgálók alkalmazásterhelés-elosztása](../application-gateway/application-gateway-create-gateway-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Webalkalmazások védelme](../application-gateway/application-gateway-web-application-firewall-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Helyek közötti forgalomelosztás](../traffic-manager/traffic-manager-configure-geographic-routing-method.md?toc=%2fazure%2fnetworking%2ftoc.json)
## Belső kapcsolatok
### [Magánkiszolgálók hálózati terheléselosztása](../load-balancer/load-balancer-get-started-ilb-arm-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Magánkiszolgálók alkalmazásterhelés-elosztása](../application-gateway/application-gateway-ilb-arm.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Virtuális hálózatok összekapcsolása (egyazon helyen)](../virtual-network/virtual-networks-create-vnetpeering-arm-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Virtuális hálózatok összekapcsolása (eltérő helyeken)](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
## Létesítmények közötti kapcsolatok
### [S2S VPN-kapcsolat létrehozása (IPsec/IKE)](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [P2S VPN-kapcsolat létrehozása (SSTP tanúsítvánnyal)](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Dedikált privát kapcsolat létrehozása (ExpressRoute)](../expressroute/expressroute-howto-circuit-portal-resource-manager.md?toc=%2fazure%2fnetworking%2ftoc.json)

## Kezelés
### [Hálózati topológia megtekintése](../network-watcher/network-watcher-topology-powershell.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Csomagrögzítés kezelése](../network-watcher/network-watcher-packet-capture-manage-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Útválasztás következő ugrásának meghatározása](../network-watcher/network-watcher-check-next-hop-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Virtuális gép IP-forgalmának ellenőrzése](../network-watcher/network-watcher-check-ip-flow-verify-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)

## Mintaszkriptek
### [Azure CLI](cli-samples.md)
### [Azure PowerShell](powershell-samples.md)

## oktatóanyagokat
### [Virtuális gépek terheléselosztása](../virtual-machines/linux/tutorial-load-balance-nodejs.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Számítógép csatlakoztatása egy virtuális hálózathoz](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)

# Referencia
## [Azure CLI](https://docs.microsoft.com/cli/azure/network)
## [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.network/?view=azurermps-3.8.0)
## [.Net](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.network?view=azuremgmtnetwork-9.1.0-preview)
## [Node.js](https://azure.microsoft.com/develop/nodejs/#azure-sdk)
## [REST](https://msdn.microsoft.com/library/mt163658.aspx)

# Erőforrások
## [Szerzői sablonok](/azure/azure-resource-manager/resource-group-authoring-templates?toc=%2fazure%2fnetworking%2ftoc.json)
## [Azure-ütemterv](https://azure.microsoft.com/roadmap/?category=networking)
## [Közösségi sablonok](https://azure.microsoft.com/resources/templates/)
## [Hálózatkezelési blog](http://azure.microsoft.com/blog/topics/networking)
## [Díjszabás](https://azure.microsoft.com/pricing)
## [Díjkalkulátor](https://azure.microsoft.com/pricing/calculator/)
## [Régiónkénti rendelkezésre állás](https://azure.microsoft.com/regions/services/)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-virtual-network)
## [Videók](https://azure.microsoft.com/resources/videos/index/?services=virtual-network)

