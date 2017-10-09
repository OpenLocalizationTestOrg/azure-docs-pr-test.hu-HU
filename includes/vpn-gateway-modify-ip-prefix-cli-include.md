### <a name="noconnection"></a>toomodify helyi hálózati átjáró IP-cím előtagokat - átjáró kapcsolat

Ha nincs gateway-kapcsolatot, és szeretné, hogy tooadd, vagy távolítsa el az IP-cím előtagokat, használhatja a hello azonos parancs használata toocreate hello helyi hálózati átjáró [az hálózati helyi-átjáró létrehozása](https://docs.microsoft.com/cli/azure/network/local-gateway#create). Ez a parancs tooupdate hello átjáró IP-cím is használható hello VPN-eszközön. toooverwrite hello aktuális beállításokat, a helyi hálózati átjáró hello meglévő nevét használja. Ha más nevet használjon, akkor hozzon létre egy új helyi hálózati átjáró, helyett felülírja a meglévő hello.

Minden egyes olyan változások, az előtagok hello teljes listáját kell megadni, nem pedig csak hello előtagok, amelyet az toochange. Adja meg, hogy szeretné-e tookeep csak hello előtagok. Ebben az esetben a 10.0.0.0/24 és a 20.0.0.0/24 előtagokat.

```azurecli
az network local-gateway create --gateway-ip-address 23.99.221.164 --name Site2 --connection-name TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
```

### <a name="withconnection"></a>toomodify helyi hálózati átjáró IP-cím előtagokat - meglévő gateway-kapcsolatot

Ha gateway-kapcsolatot, és szeretné, hogy tooadd vagy távolítsa el az IP-cím előtagokat, hello előtagok használatával frissítheti [az hálózati helyi-átjáró frissítési](https://docs.microsoft.com/cli/azure/network/local-gateway#update). Ez némi állásidőt jelent a VPN-kapcsolata számára. Amikor hello IP-cím módosítása előtagok toodelete hello VPN-átjáró nem szükséges.

Minden egyes olyan változások, az előtagok hello teljes listáját kell megadni, nem pedig csak hello előtagok, amelyet az toochange. Ebben a példában a 10.0.0.0/24 és a 20.0.0.0/24 már léteznek, A Microsoft hello előtagok 30.0.0.0/24 és 40.0.0.0/24 hozzáadása, és adja meg hello előtagok az összes 4, való frissítése során.

```azurecli
az network local-gateway update --local-address-prefixes 10.0.0.0/24 20.0.0.0/24 30.0.0.0/24 40.0.0.0/24 --name VNet1toSite2 --connection-name TestRG1
```