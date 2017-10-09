### <a name="tooview-local-network-gateways"></a>tooview helyi hálózati átjáró

hello helyi hálózati átjáró, hello használja listáját tooview [az hálózati helyi-átjáró lista](https://docs.microsoft.com/cli/azure/network/local-gateway#list) parancsot.

```azurecli
az network local-gateway list --resource-group TestRG1
```

[!INCLUDE [modify-prefix](vpn-gateway-modify-ip-prefix-cli-include.md)]

[!INCLUDE [modify-gateway-IP](vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

### <a name="tooverify-hello-shared-key-values"></a>tooverify hello megosztott kulcs értékeket

Győződjön meg arról, hogy megosztott hello kulcsérték hello ugyanazt az értéket, amelyet a VPN-eszköz konfigurációjában használt. Ha nem, vagy futtassa újra hello értéket hello, vagy frissítés hello eszköz használata a hello visszatérési értéket hello hello kapcsolat. hello az értékeknek egyezniük kell. tooview hello megosztott kulcs, használjon hello [az hálózati vpn-kapcsolat-lista](https://docs.microsoft.com/cli/azure/network/vpn-connection#list).

```azurecli
az network vpn-connection shared-key show --connection-name VNet1toSite2 --resource-group TestRG1
```
### <a name="tooview-hello-vpn-gateway-public-ip-address"></a>tooview hello VPN-átjáró nyilvános IP-cím

toofind hello nyilvános IP-címet a virtuális hálózati átjáró használata hello [az nyilvános ip-lista](https://docs.microsoft.com/cli/azure/network/public-ip#list) parancsot. Olvasás, ehhez a példához hello kimeneti rendszer formázott toodisplay hello táblázatos formátumú nyilvános IP-címeket.

```azurecli
az network public-ip list --resource-group TestRG1 --output table
```
