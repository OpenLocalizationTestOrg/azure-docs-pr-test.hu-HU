Ez a feladat használható egy Vnetet hello lépéseket a következő konfigurációs hivatkozáslista hello hello értékei alapján. További beállításokat és a nevek azt is ezen a listán. Nem használjuk a lista közvetlenül a hello műveletekkel, bár jelenleg felvenni a következő felsorolásban szereplő hello értékek alapján változók. Másolhatja hello lista toouse referenciaként hello értékeket cserélje le a saját.

**Konfigurációs hivatkozáslista**

* Virtuális hálózati név = "TestVNet"
* Virtuális hálózati címterület = 192.168.0.0/16
* Erőforráscsoport = "TestRG"
* Alhalozat_1 Name = "Előtér" 
* Címterület Alhalozat_1 = "192.168.1.0/24"
* Átjáró alhálózati név: "GatewaySubnet" mindig neve egy átjáró-alhálózatot kell *GatewaySubnet*.
* Átjáró alhálózati címtartományt = "192.168.200.0/26"
* A régióban = "USA keleti régiója"
* Átjáró Name = "GW"
* Átjáró IP-név = "GWIP"
* Átjáró IP-konfiguráció neve = "gwipconf"
* Típus = "ExpressRoute" Ez a típus egy ExpressRoute-konfiguráció szükséges.
* Átjáró nyilvános IP-név = "gwpip"

## <a name="add-a-gateway"></a>Átjáró hozzáadása
1. Csatlakozás Azure-előfizetés tooyour.

  ```powershell 
  Login-AzureRmAccount
  Get-AzureRmSubscription 
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
2. Deklarálja a változókat, ehhez a gyakorlathoz. Lehet, hogy tooedit hello minta tooreflect hello beállításokat, hogy szeretné-e toouse.

  ```powershell 
  $RG = "TestRG"
  $Location = "East US"
  $GWName = "GW"
  $GWIPName = "GWIP"
  $GWIPconfName = "gwipconf"
  $VNetName = "TestVNet"
  ```
3. Hello virtuális hálózat objektumot tárolja változóként.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
  ```
4. Adjon hozzá egy átjáró alhálózati tooyour virtuális hálózat. hello átjáróalhálózatot "GatewaySubnet" nevet kell kapniuk. Hozzon létre egy átjáró-alhálózatot, amely /27 vagy nagyobb (26, / / 25, stb.).

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26
  ```
5. Hello konfigurációjának beállítása

  ```powershell
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
6. Hello átjáróalhálózatot tárolására változóként.

  ```powershell
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
  ```
7. Kérjen egy nyilvános IP-címet. hello IP-címre van szükség hello átjáró létrehozása előtt. Nem adható meg, hogy szeretné-e toouse; hello IP-cím dinamikusan történik. Hello következő konfigurációs szakasz az IP-címet fogja használni. hello AllocationMethod dinamikus kell lennie.

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name $GWIPName  -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
  ```
8. Hozza létre az átjáró hello konfigurációt. hello átjáró konfigurációs hello alhálózatot és a hello nyilvános IP-cím toouse határoz meg. Ebben a lépésben meg hello konfigurációs hello átjáró létrehozásakor használható. Ez a lépés nem hoz létre hello átjáró objektum. Alább toocreate hello minta az átjáró konfigurációt használja.

  ```powershell
  $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
  ```
9. Hello átjáró létrehozása. Ebben a lépésben hello **- GatewayType** különösen fontos. Hello értéket kell használnia **ExpressRoute**. Miután ezek a parancsmagok, hello átjáró 45 perc vagy több toocreate vehet igénybe.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard
  ```

## <a name="verify-hello-gateway-was-created"></a>Hello átjáró létrejött-e
A következő parancsokat, amelyek átjáró hello tooverify létrejött hello használata:

```powershell
Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG
```

## <a name="resize-a-gateway"></a>Átjáró méretezése
A több [Gateway SKU-n](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Hello parancs toochange hello átjáró-Termékváltozat követően bármikor használhatja.

> [!IMPORTANT]
> Ez a parancs UltraPerformance átjáró nem működik. toochange a tooan UltraPerformance átjárók, először távolítsa el a meglévő ExpressRoute-átjáró hello, és ezután hozzon létre újat UltraPerformance. toodowngrade az átjáró UltraPerformance-átjáróról, először el kell távolítani hello UltraPerformance átjáró, és hozzon létre egy új átjárót.
> 
> 

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="remove-a-gateway"></a>Átjáró eltávolítása
A következő parancs tooremove átjáró hello használata:

```powershell
Remove-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
```