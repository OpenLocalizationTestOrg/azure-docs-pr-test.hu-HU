### <a name="to-view-local-network-gateways"></a><span data-ttu-id="cf08f-101">Helyi hálózati átjárók megtekintése</span><span class="sxs-lookup"><span data-stu-id="cf08f-101">To view local network gateways</span></span>

<span data-ttu-id="cf08f-102">A helyi hálózati átjárók listájának megtekintéséhez használja az [az network local-gateway list](https://docs.microsoft.com/cli/azure/network/local-gateway#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="cf08f-102">To view a list of the local network gateways, use the [az network local-gateway list](https://docs.microsoft.com/cli/azure/network/local-gateway#list) command.</span></span>

```azurecli
az network local-gateway list --resource-group TestRG1
```

[!INCLUDE [modify-prefix](vpn-gateway-modify-ip-prefix-cli-include.md)]

[!INCLUDE [modify-gateway-IP](vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

### <a name="to-verify-the-shared-key-values"></a><span data-ttu-id="cf08f-103">A megosztottkulcs-értékek ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="cf08f-103">To verify the shared key values</span></span>

<span data-ttu-id="cf08f-104">Ellenőrizze, hogy a megosztott kulcs megegyezik-e a VPN-eszköze konfigurálásakor használt értékkel.</span><span class="sxs-lookup"><span data-stu-id="cf08f-104">Verify that the shared key value is the same value that you used for your VPN device configuration.</span></span> <span data-ttu-id="cf08f-105">Ha nem, futtassa a kapcsolatot újra az eszközből származó értékkel, vagy frissítse az eszközt a visszaadott értékkel.</span><span class="sxs-lookup"><span data-stu-id="cf08f-105">If it is not, either run the connection again using the value from the device, or update the device with the value from the return.</span></span> <span data-ttu-id="cf08f-106">Az értékeknek meg kell egyezniük.</span><span class="sxs-lookup"><span data-stu-id="cf08f-106">The values must match.</span></span> <span data-ttu-id="cf08f-107">A megosztott kulcs megtekintéséhez használja az [az network vpn-connection-list](https://docs.microsoft.com/cli/azure/network/vpn-connection#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="cf08f-107">To view the shared key, use the [az network vpn-connection-list](https://docs.microsoft.com/cli/azure/network/vpn-connection#list).</span></span>

```azurecli
az network vpn-connection shared-key show --connection-name VNet1toSite2 --resource-group TestRG1
```
### <a name="to-view-the-vpn-gateway-public-ip-address"></a><span data-ttu-id="cf08f-108">A VPN Gateway nyilvános IP-címének megtekintése</span><span class="sxs-lookup"><span data-stu-id="cf08f-108">To view the VPN gateway Public IP address</span></span>

<span data-ttu-id="cf08f-109">A virtuális hálózati átjáró IP-címét az [az network public-ip list](https://docs.microsoft.com/cli/azure/network/public-ip#list) paranccsal keresheti meg.</span><span class="sxs-lookup"><span data-stu-id="cf08f-109">To find the public IP address of your virtual network gateway, use the [az network public-ip list](https://docs.microsoft.com/cli/azure/network/public-ip#list) command.</span></span> <span data-ttu-id="cf08f-110">Az olvashatóság érdekében a jelen példa kimenete táblázatos formában jeleníti meg a nyilvános IP-címek listáját.</span><span class="sxs-lookup"><span data-stu-id="cf08f-110">For easy reading, the output for this example is formatted to display the list of public IPs in table format.</span></span>

```azurecli
az network public-ip list --resource-group TestRG1 --output table
```
