### <a name="toomodify-hello-local-network-gateway-gatewayipaddress"></a>toomodify hello helyi hálózati átjáró "gatewayIpAddress"

Nyilvános IP-címének hello VPN-eszköz, amelyet az tooconnect toohas megváltozásakor toomodify hello helyi hálózati átjáró tooreflect módosító szüksége. hello átjáró IP-cím módosítása nem egy meglévő VPN-átjáró kapcsolat eltávolítása (ha van). toomodify hello átjáró IP-cím, hello értékek "Hely2" és "TestRG1" a név felülírandó a saját használatával hello [az hálózati helyi-átjáró frissítési](https://docs.microsoft.com/cli/azure/network/local-gateway#update) parancsot.

```azurecli
az network local-gateway update --gateway-ip-address 23.99.222.170 --name Site2 --resource-group TestRG1
```

Ellenőrizze, hogy helyes-e hello kimeneti hello IP-cím:

```
"gatewayIpAddress": "23.99.222.170",
```