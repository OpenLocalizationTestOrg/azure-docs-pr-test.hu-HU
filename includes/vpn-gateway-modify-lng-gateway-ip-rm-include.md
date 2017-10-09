### <a name="gwipnoconnection"></a>toomodify hello helyi hálózati átjáró "GatewayIpAddress" - átjáró kapcsolat

Nyilvános IP-címének hello VPN-eszköz, amelyet az tooconnect toohas megváltozásakor toomodify hello helyi hálózati átjáró tooreflect módosító szüksége. Hello példa toomodify a helyi hálózati átjáró, amely nem rendelkezik egy gateway-kapcsolatot használjon.

Ha módosítja ezt az értéket, módosíthatja is hello címelőtagok: hello ugyanannyi időt vesz igénybe. Lehet, hogy toouse hello meglévő neve a helyi hálózati átjáró toooverwrite hello jelenlegi beállításai. Ha más nevet használjon, akkor hozzon létre egy új helyi hálózati átjáró, helyett felülírja a meglévő hello.

```powershell
New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
-Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName
```

### <a name="gwipwithconnection"></a>toomodify hello helyi hálózati átjáró "GatewayIpAddress" - meglévő gateway-kapcsolatot

Nyilvános IP-címének hello VPN-eszköz, amelyet az tooconnect toohas megváltozásakor toomodify hello helyi hálózati átjáró tooreflect módosító szüksége. Átjáró már létezik egy kapcsolat, ha először tooremove hello kapcsolat. Hello kapcsolat eltávolítása után módosítja a hello átjáró IP-címet, és hozza létre újra egy új kapcsolatot. Módosíthatja is hello címelőtagok: hello ugyanannyi időt vesz igénybe. Ez némi állásidőt jelent a VPN-kapcsolata számára. Ha módosítja a hello átjáró IP-címe, nincs szükség a toodelete hello VPN-átjáró. Csak akkor kell tooremove hello kapcsolat.
 

1. Távolítsa el a hello kapcsolatot. Hello neve a kapcsolat található hello "Get-AzureRmVirtualNetworkGatewayConnection" parancsmag használatával.

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName
  ```
2. Módosítsa a hello "GatewayIpAddress" értékét. Módosíthatja is hello címelőtagok: hello ugyanannyi időt vesz igénybe. Lehet, hogy toouse hello meglévő neve a helyi hálózati átjáró toooverwrite hello aktuális beállításokat. Ha ezt elmulasztja, akkor hozzon létre egy új helyi hálózati átjáró, hello felülírása helyett a meglévőt.

  ```powershell
  New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
  -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
  -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName
  ```
3. Hello kapcsolat létrehozása. Ebben a példában egy IPsec kapcsolattípust konfigurálunk. Ismételt a kapcsolatot, használja a megadott kapcsolattípus hello a konfigurációhoz. További kapcsolattípusokat, lásd: hello [PowerShell-parancsmag](https://msdn.microsoft.com/library/mt603611.aspx) lap.  tooobtain hello pedig neve hello "Get-AzureRmVirtualNetworkGateway" parancsmag is futtathatja.
   
    Hello változók megadása.

  ```powershell
  $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
  $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName
  ```
   
    Hello kapcsolat létrehozása.

  ```powershell 
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
  -Location "West US" `
  -VirtualNetworkGateway1 $vnetgw `
  -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```