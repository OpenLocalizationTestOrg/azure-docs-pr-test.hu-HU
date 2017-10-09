### <a name="toomodify-hello-local-network-gateway-gatewayipaddress"></a><span data-ttu-id="4f9e2-101">toomodify hello helyi hálózati átjáró "gatewayIpAddress"</span><span class="sxs-lookup"><span data-stu-id="4f9e2-101">toomodify hello local network gateway 'gatewayIpAddress'</span></span>

<span data-ttu-id="4f9e2-102">Nyilvános IP-címének hello VPN-eszköz, amelyet az tooconnect toohas megváltozásakor toomodify hello helyi hálózati átjáró tooreflect módosító szüksége.</span><span class="sxs-lookup"><span data-stu-id="4f9e2-102">If hello VPN device that you want tooconnect toohas changed its public IP address, you need toomodify hello local network gateway tooreflect that change.</span></span> <span data-ttu-id="4f9e2-103">hello átjáró IP-cím módosítása nem egy meglévő VPN-átjáró kapcsolat eltávolítása (ha van).</span><span class="sxs-lookup"><span data-stu-id="4f9e2-103">hello gateway IP address can be changed without removing an existing VPN gateway connection (if you have one).</span></span> <span data-ttu-id="4f9e2-104">toomodify hello átjáró IP-cím, hello értékek "Hely2" és "TestRG1" a név felülírandó a saját használatával hello [az hálózati helyi-átjáró frissítési](https://docs.microsoft.com/cli/azure/network/local-gateway#update) parancsot.</span><span class="sxs-lookup"><span data-stu-id="4f9e2-104">toomodify hello gateway IP address, replace hello values 'Site2' and 'TestRG1' with your own using hello [az network local-gateway update](https://docs.microsoft.com/cli/azure/network/local-gateway#update) command.</span></span>

```azurecli
az network local-gateway update --gateway-ip-address 23.99.222.170 --name Site2 --resource-group TestRG1
```

<span data-ttu-id="4f9e2-105">Ellenőrizze, hogy helyes-e hello kimeneti hello IP-cím:</span><span class="sxs-lookup"><span data-stu-id="4f9e2-105">Verify that hello IP address is correct in hello output:</span></span>

```
"gatewayIpAddress": "23.99.222.170",
```