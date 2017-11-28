### <a name="to-modify-the-local-network-gateway-gatewayipaddress"></a><span data-ttu-id="b9eed-101">Helyi hálózati átjáró „gatewayIpAddress” értékének módosítása</span><span class="sxs-lookup"><span data-stu-id="b9eed-101">To modify the local network gateway 'gatewayIpAddress'</span></span>

<span data-ttu-id="b9eed-102">Ha a VPN-eszköz, amelyhez csatlakozni akar, megváltoztatta nyilvános IP-címét, a változtatásnak megfelelően módosítania kell a helyi hálózati átjárót.</span><span class="sxs-lookup"><span data-stu-id="b9eed-102">If the VPN device that you want to connect to has changed its public IP address, you need to modify the local network gateway to reflect that change.</span></span> <span data-ttu-id="b9eed-103">Az átjáró IP-címe megváltoztatható a létező VPN átjárókapcsolat eltávolítása nélkül (ha van ilyen).</span><span class="sxs-lookup"><span data-stu-id="b9eed-103">The gateway IP address can be changed without removing an existing VPN gateway connection (if you have one).</span></span> <span data-ttu-id="b9eed-104">Az átjáró IP-címének módosításához cserélje ki a „Site2” és a „TestRG1” értékeket a sajátjaira az [az network local-gateway update](https://docs.microsoft.com/cli/azure/network/local-gateway#update) parancs használatakor.</span><span class="sxs-lookup"><span data-stu-id="b9eed-104">To modify the gateway IP address, replace the values 'Site2' and 'TestRG1' with your own using the [az network local-gateway update](https://docs.microsoft.com/cli/azure/network/local-gateway#update) command.</span></span>

```azurecli
az network local-gateway update --gateway-ip-address 23.99.222.170 --name Site2 --resource-group TestRG1
```

<span data-ttu-id="b9eed-105">Ellenőrizze, hogy az IP-cím megfelelő-e a kimenetben:</span><span class="sxs-lookup"><span data-stu-id="b9eed-105">Verify that the IP address is correct in the output:</span></span>

```
"gatewayIpAddress": "23.99.222.170",
```