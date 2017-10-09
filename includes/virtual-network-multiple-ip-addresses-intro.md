> [!div class="op_single_selector"]
> * [Portál](../articles/virtual-network/virtual-network-multiple-ip-addresses-portal.md)
> * [PowerShell](../articles/virtual-network/virtual-network-multiple-ip-addresses-powershell.md)
> * [CLI 2.0](../articles/virtual-network/virtual-network-multiple-ip-addresses-cli.md)
> * [CLI 1.0](../articles/virtual-network/virtual-network-multiple-ip-addresses-cli-nodejs.md)
> * [Sablon](../articles/virtual-network/virtual-network-multiple-ip-addresses-template.md)
>

Egy Azure virtuális gép (VM) egy vagy több hálózati adapterek (NIC) tooit csatlakoztatva. A hálózati adapterek egy vagy több statikus vagy dinamikus nyilvános és magánhálózati IP-címek hozzárendelt tooit. Hozzárendelés, több IP-címek tooa VM lehetővé teszi, hogy a következő képességeket hello:

* Több webhely vagy szolgáltatás üzemeltetése különböző IP-címekkel és SSL-tanúsítványokkal egyetlen kiszolgálón.
* Használat hálózati virtuális készülékként, például tűzfalként vagy terheléselosztóként.
* hello képességét tooadd bármely hello privát IP-címek bármely hello hálózati adapterek tooan Azure Load Balancer háttér-készlet. A múltbeli csak hello hello elsődleges IP-címet a hálózati adapter elsődleges oka az lehet, hello tooa háttér-készlethez hozzáadott. További részletek toolearn hogyan tooload kiegyenlítheti a több IP-konfigurációk, olvassa el a hello [terheléselosztás több IP-konfigurációk](../articles/load-balancer/load-balancer-multiple-ip.md?toc=%2fazure%2fvirtual-network%2ftoc.json) cikk.

Minden hálózati adapter csatlakoztatva tooa virtuális gép egy vagy több IP-konfigurációk társított tooit. Az egyes konfigurációkhoz egy statikus vagy dinamikus magánhálózati IP-cím van hozzárendelve. Minden egyes konfigurációs lehetnek IP cím egy nyilvános kapcsolódó erőforrás tooit is. A nyilvános IP-cím erőforrás vagy egy dinamikus vagy statikus nyilvános IP cím tooit rendelkezik. toolearn további információ az IP-címek az Azure-ban, olvassa el a hello [IP-címek az Azure-ban](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md) cikk. 

Sok magánhálózati IP-címe a korlát toohow nincs címek rendelhetők tooa hálózati adaptert. Is van a korlát toohow sok nyilvános IP-címek az Azure-előfizetés használható. Lásd: hello [Azure korlátozza](../articles/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) cikkben alább.
