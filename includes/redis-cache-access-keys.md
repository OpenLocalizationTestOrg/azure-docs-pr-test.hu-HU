tooconnect tooan Azure Redis Cache példány gyorsítótár-ügyfelek számára szükségesek hello állomás nevét, a portok és a kulcsok hello gyorsítótár. Egyes ügyfelek különböző neveken hivatkozhatnak toothese elemeket. Ezt az információt a hello Azure-portálon vagy a parancssori eszközök, például az Azure parancssori felület használatával kérheti le.

### <a name="retrieve-host-name-ports-and-access-keys-using-hello-azure-portal"></a>Gazdagép neve, a portok és a hívóbetűk hello Azure portál használatával
tooretrieve gazdagép nevét, a portok és a hívóbetűk hello Azure portál használatával [Tallózás](../articles/redis-cache/cache-configure.md#configure-redis-cache-settings) hello tooyour gyorsítótár [Azure-portálon](https://portal.azure.com) kattintson **hívóbetűk** és  **Tulajdonságok** a hello **erőforrás menü**. 

![A Redis Cache-gyorsítótár beállításai](media/redis-cache-access-keys/redis-cache-hostname-ports-keys.png)

### <a name="retrieve-host-name-ports-and-access-keys-using-azure-cli"></a>Gazdagépnév, portok és hozzáférési kulcsok lekérése az Azure CLI-vel
hívása tooretrieve hello állomásnév és portok használata az Azure CLI 2.0 [az redis megjelenítése](https://docs.microsoft.com/cli/azure/redis#show), és tooretrieve hello kulcsok hívása [az redis listázása](https://docs.microsoft.com/cli/azure/redis#list-keys). hello következő parancsfájl meghívja a két parancsok és Echok hello állomásnév, a portok, valamint a kulcsok toohello konzol.

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

Ez a parancsfájl kapcsolatos további információkért lásd: [hello állomásnév, portokat és a kulcsok beolvasása az Azure Redis Cache](../articles/redis-cache/scripts/cache-keys-ports.md). Az Azure CLI 2.0-val kapcsolatos további információkat [az Azure CLI 2.0 telepítésével](https://docs.microsoft.com/cli/azure/install-azure-cli) foglalkozó cikkben és [az Azure CLI 2.0-s verziójának használatát ismertető](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) bevezetőben talál.
