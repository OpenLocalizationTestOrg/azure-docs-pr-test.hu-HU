### <a name="tooview-local-network-gateways"></a><span data-ttu-id="4c7a5-101">tooview helyi hálózati átjáró</span><span class="sxs-lookup"><span data-stu-id="4c7a5-101">tooview local network gateways</span></span>

<span data-ttu-id="4c7a5-102">hello helyi hálózati átjáró, hello használja listáját tooview [az hálózati helyi-átjáró lista](https://docs.microsoft.com/cli/azure/network/local-gateway#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="4c7a5-102">tooview a list of hello local network gateways, use hello [az network local-gateway list](https://docs.microsoft.com/cli/azure/network/local-gateway#list) command.</span></span>

```azurecli
az network local-gateway list --resource-group TestRG1
```

[!INCLUDE [modify-prefix](vpn-gateway-modify-ip-prefix-cli-include.md)]

[!INCLUDE [modify-gateway-IP](vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

### <a name="tooverify-hello-shared-key-values"></a><span data-ttu-id="4c7a5-103">tooverify hello megosztott kulcs értékeket</span><span class="sxs-lookup"><span data-stu-id="4c7a5-103">tooverify hello shared key values</span></span>

<span data-ttu-id="4c7a5-104">Győződjön meg arról, hogy megosztott hello kulcsérték hello ugyanazt az értéket, amelyet a VPN-eszköz konfigurációjában használt.</span><span class="sxs-lookup"><span data-stu-id="4c7a5-104">Verify that hello shared key value is hello same value that you used for your VPN device configuration.</span></span> <span data-ttu-id="4c7a5-105">Ha nem, vagy futtassa újra hello értéket hello, vagy frissítés hello eszköz használata a hello visszatérési értéket hello hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="4c7a5-105">If it is not, either run hello connection again using hello value from hello device, or update hello device with hello value from hello return.</span></span> <span data-ttu-id="4c7a5-106">hello az értékeknek egyezniük kell.</span><span class="sxs-lookup"><span data-stu-id="4c7a5-106">hello values must match.</span></span> <span data-ttu-id="4c7a5-107">tooview hello megosztott kulcs, használjon hello [az hálózati vpn-kapcsolat-lista](https://docs.microsoft.com/cli/azure/network/vpn-connection#list).</span><span class="sxs-lookup"><span data-stu-id="4c7a5-107">tooview hello shared key, use hello [az network vpn-connection-list](https://docs.microsoft.com/cli/azure/network/vpn-connection#list).</span></span>

```azurecli
az network vpn-connection shared-key show --connection-name VNet1toSite2 --resource-group TestRG1
```
### <a name="tooview-hello-vpn-gateway-public-ip-address"></a><span data-ttu-id="4c7a5-108">tooview hello VPN-átjáró nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="4c7a5-108">tooview hello VPN gateway Public IP address</span></span>

<span data-ttu-id="4c7a5-109">toofind hello nyilvános IP-címet a virtuális hálózati átjáró használata hello [az nyilvános ip-lista](https://docs.microsoft.com/cli/azure/network/public-ip#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="4c7a5-109">toofind hello public IP address of your virtual network gateway, use hello [az network public-ip list](https://docs.microsoft.com/cli/azure/network/public-ip#list) command.</span></span> <span data-ttu-id="4c7a5-110">Olvasás, ehhez a példához hello kimeneti rendszer formázott toodisplay hello táblázatos formátumú nyilvános IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="4c7a5-110">For easy reading, hello output for this example is formatted toodisplay hello list of public IPs in table format.</span></span>

```azurecli
az network public-ip list --resource-group TestRG1 --output table
```
