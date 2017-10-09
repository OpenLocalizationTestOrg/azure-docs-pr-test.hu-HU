<span data-ttu-id="e1219-101">tooconnect tooan Azure Redis Cache példány gyorsítótár-ügyfelek számára szükségesek hello állomás nevét, a portok és a kulcsok hello gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="e1219-101">tooconnect tooan Azure Redis Cache instance, cache clients need hello host name, ports, and keys of hello cache.</span></span> <span data-ttu-id="e1219-102">Egyes ügyfelek különböző neveken hivatkozhatnak toothese elemeket.</span><span class="sxs-lookup"><span data-stu-id="e1219-102">Some clients may refer toothese items by slightly different names.</span></span> <span data-ttu-id="e1219-103">Ezt az információt a hello Azure-portálon vagy a parancssori eszközök, például az Azure parancssori felület használatával kérheti le.</span><span class="sxs-lookup"><span data-stu-id="e1219-103">You can retrieve this information in hello Azure portal or by using command-line tools such as Azure CLI.</span></span>

### <a name="retrieve-host-name-ports-and-access-keys-using-hello-azure-portal"></a><span data-ttu-id="e1219-104">Gazdagép neve, a portok és a hívóbetűk hello Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="e1219-104">Retrieve host name, ports, and access keys using hello Azure Portal</span></span>
<span data-ttu-id="e1219-105">tooretrieve gazdagép nevét, a portok és a hívóbetűk hello Azure portál használatával [Tallózás](../articles/redis-cache/cache-configure.md#configure-redis-cache-settings) hello tooyour gyorsítótár [Azure-portálon](https://portal.azure.com) kattintson **hívóbetűk** és  **Tulajdonságok** a hello **erőforrás menü**.</span><span class="sxs-lookup"><span data-stu-id="e1219-105">tooretrieve host name, ports, and access keys using hello Azure Portal, [browse](../articles/redis-cache/cache-configure.md#configure-redis-cache-settings) tooyour cache in hello [Azure portal](https://portal.azure.com) and click **Access keys** and **Properties** in hello **Resource menu**.</span></span> 

![A Redis Cache-gyorsítótár beállításai](media/redis-cache-access-keys/redis-cache-hostname-ports-keys.png)

### <a name="retrieve-host-name-ports-and-access-keys-using-azure-cli"></a><span data-ttu-id="e1219-107">Gazdagépnév, portok és hozzáférési kulcsok lekérése az Azure CLI-vel</span><span class="sxs-lookup"><span data-stu-id="e1219-107">Retrieve host name, ports, and access keys using Azure CLI</span></span>
<span data-ttu-id="e1219-108">hívása tooretrieve hello állomásnév és portok használata az Azure CLI 2.0 [az redis megjelenítése](https://docs.microsoft.com/cli/azure/redis#show), és tooretrieve hello kulcsok hívása [az redis listázása](https://docs.microsoft.com/cli/azure/redis#list-keys).</span><span class="sxs-lookup"><span data-stu-id="e1219-108">tooretrieve hello host name and ports using Azure CLI 2.0 you can call [az redis show](https://docs.microsoft.com/cli/azure/redis#show), and tooretrieve hello keys you can call [az redis list-keys](https://docs.microsoft.com/cli/azure/redis#list-keys).</span></span> <span data-ttu-id="e1219-109">hello következő parancsfájl meghívja a két parancsok és Echok hello állomásnév, a portok, valamint a kulcsok toohello konzol.</span><span class="sxs-lookup"><span data-stu-id="e1219-109">hello following script calls these two commands and echos hello hostname, ports, and keys toohello console.</span></span>

```azurecli
#/bin/bash

# Retrieve hello hostname, ports, and keys for contosoCache located in contosoGroup

# Retrieve hello hostname and ports for an Azure Redis Cache instance
redis=($(az redis show --name contosoCache --resource-group contosoGroup --query [hostName,enableNonSslPort,port,sslPort] --output tsv))

# Retrieve hello keys for an Azure Redis Cache instance
keys=($(az redis list-keys --name contosoCache --resource-group contosoGroup --query [primaryKey,secondaryKey] --output tsv))

# Display hello retrieved hostname, keys, and ports
echo "Hostname:" ${redis[0]}
echo "Non SSL Port:" ${redis[2]}
echo "Non SSL Port Enabled:" ${redis[1]}
echo "SSL Port:" ${redis[3]}
echo "Primary Key:" ${keys[0]}
echo "Secondary Key:" ${keys[1]}
```

<span data-ttu-id="e1219-110">Ez a parancsfájl kapcsolatos további információkért lásd: [hello állomásnév, portokat és a kulcsok beolvasása az Azure Redis Cache](../articles/redis-cache/scripts/cache-keys-ports.md).</span><span class="sxs-lookup"><span data-stu-id="e1219-110">For more information about this script, see [Get hello hostname, ports, and keys for Azure Redis Cache](../articles/redis-cache/scripts/cache-keys-ports.md).</span></span> <span data-ttu-id="e1219-111">Az Azure CLI 2.0-val kapcsolatos további információkat [az Azure CLI 2.0 telepítésével](https://docs.microsoft.com/cli/azure/install-azure-cli) foglalkozó cikkben és [az Azure CLI 2.0-s verziójának használatát ismertető](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) bevezetőben talál.</span><span class="sxs-lookup"><span data-stu-id="e1219-111">For more information on Azure CLI 2.0, see [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) and [Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).</span></span>
