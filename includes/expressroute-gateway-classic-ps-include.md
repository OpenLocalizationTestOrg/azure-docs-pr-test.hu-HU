Létre kell hoznia egy VNet és egy átjáró-alhálózatot, először a következő feladatok hello használata előtt. Hello cikke [hello klasszikus portál használatával virtuális hálózat konfigurálása](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) további információt.   

## <a name="add-a-gateway"></a>Átjáró hozzáadása
Hello paranccsal toocreate átjáró alatt. Lehet, hogy toosubstitute bármely értékeket a saját.

    New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard

## <a name="verify-hello-gateway-was-created"></a>Hello átjáró létrejött-e
Hello parancsot használja, amelyek átjáró hello tooverify létrejött. A parancs segítségével lekérdezhető hello átjáró azonosítója, amely a többi művelet kell is.

    Get-AzureVirtualNetworkGateway

## <a name="resize-a-gateway"></a>Átjáró méretezése
A több [Gateway SKU-n](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Hello parancs toochange hello átjáró-Termékváltozat követően bármikor használhatja.

> [!IMPORTANT]
> Ez a parancs UltraPerformance átjáró nem működik. toochange a tooan UltraPerformance átjárók, először távolítsa el a meglévő ExpressRoute-átjáró hello, és ezután hozzon létre újat UltraPerformance. toodowngrade az átjáró UltraPerformance-átjáróról, először el kell távolítani hello UltraPerformance átjáró, és hozzon létre egy új átjárót. 
> 
> 

    Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance

## <a name="remove-a-gateway"></a>Átjáró eltávolítása
Alább tooremove átjáró hello paranccsal

    Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>