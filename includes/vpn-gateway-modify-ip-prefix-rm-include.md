### <a name="noconnection"></a>toomodify helyi hálózati átjáró IP-cím előtagokat - átjáró kapcsolat

tooadd további címelőtagokat:

```powershell
$local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
-AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
```

tooremove címelőtagok:<br>
Hagyja ki hello előtagokat, amely már nem szükséges. Ebben a példában a Microsoft már nem 20.0.0.0/24 előtagra (a hello előző példában), ezért frissítjük hello helyi hálózati átjáró, kivéve a ezt az előtagot.

```powershell
$local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
-AddressPrefix @('10.0.0.0/24','30.0.0.0/24')
```

### <a name="withconnection"></a>toomodify helyi hálózati átjáró IP-cím előtagokat - meglévő gateway-kapcsolatot

Ha átjáró kapcsolat és szeretné, hogy tooadd, vagy távolítsa el a hello a helyi hálózati átjáróban tárolt IP-címelőtagokat, akkor toodo hello lépések sorrendben. Ez némi állásidőt jelent a VPN-kapcsolata számára. Ha módosítja az IP-cím előtagokat, nincs szükség a toodelete hello VPN-átjáró. Csak akkor kell tooremove hello kapcsolat.


1. Távolítsa el a hello kapcsolatot.

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName
  ```
2. Módosítsa a helyi hálózati átjáró hello címelőtagok.
   
  Állítsa be a hello LocalNetworkGateway hello változót.

  ```powershell
  $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName
  ```
   
  Hello előtagok módosítása.
   
  ```powershell
  Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
  -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
  ```
3. Hello kapcsolat létrehozása. Ebben a példában egy IPsec kapcsolattípust konfigurálunk. Ismételt a kapcsolatot, használja a megadott kapcsolattípus hello a konfigurációhoz. További kapcsolattípusokat, lásd: hello [PowerShell-parancsmag](https://msdn.microsoft.com/library/mt603611.aspx) lap.
   
  Állítsa be a hello pedig hello változót.

  ```powershell
  $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName
  ```
   
  Hello kapcsolat létrehozása. A példa hello változó a 2. lépésben beállított $local.

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName -Location 'West US' `
  -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
  -ConnectionType IPsec `
  -RoutingWeight 10 -SharedKey 'abc123'
  ```