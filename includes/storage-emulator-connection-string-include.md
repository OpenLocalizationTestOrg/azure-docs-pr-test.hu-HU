<span data-ttu-id="2c6e4-101">A storage emulator megosztott kulcsos hitelesítést támogatja egy rögzített fiókhoz és egy jól ismert hitelesítési kulcs.</span><span class="sxs-lookup"><span data-stu-id="2c6e4-101">The storage emulator supports a single fixed account and a well-known authentication key for Shared Key authentication.</span></span> <span data-ttu-id="2c6e4-102">A fiók és a kulcs a storage emulator való használatra engedélyezett csak megosztott kulcsos hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="2c6e4-102">This account and key are the only Shared Key credentials permitted for use with the storage emulator.</span></span> <span data-ttu-id="2c6e4-103">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="2c6e4-103">They are:</span></span>

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> <span data-ttu-id="2c6e4-104">A hitelesítési kulcs a storage emulator által támogatott készült csak egy ügyfél-hitelesítési kód funkció tesztelése.</span><span class="sxs-lookup"><span data-stu-id="2c6e4-104">The authentication key supported by the storage emulator is intended only for testing the functionality of your client authentication code.</span></span> <span data-ttu-id="2c6e4-105">Bármilyen biztonsági célt nem szolgál.</span><span class="sxs-lookup"><span data-stu-id="2c6e4-105">It does not serve any security purpose.</span></span> <span data-ttu-id="2c6e4-106">A termelési tárfiók és a kulcs a storage emulator nem használható.</span><span class="sxs-lookup"><span data-stu-id="2c6e4-106">You cannot use your production storage account and key with the storage emulator.</span></span> <span data-ttu-id="2c6e4-107">Ne használjon a fejlesztői fiók termelési adatokkal.</span><span class="sxs-lookup"><span data-stu-id="2c6e4-107">You should not use the development account with production data.</span></span>
> 
> <span data-ttu-id="2c6e4-108">A storage emulator csak a HTTP Protokollon keresztül kapcsolatot támogat.</span><span class="sxs-lookup"><span data-stu-id="2c6e4-108">The storage emulator supports connection via HTTP only.</span></span> <span data-ttu-id="2c6e4-109">HTTPS azonban az ajánlott protokoll egy éles Azure-tárfiók-erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="2c6e4-109">However, HTTPS is the recommended protocol for accessing resources in a production Azure storage account.</span></span>
> 

#### <a name="connect-to-the-emulator-account-using-a-shortcut"></a><span data-ttu-id="2c6e4-110">Csatlakozás a parancsikonnal emulátor-fiókhoz</span><span class="sxs-lookup"><span data-stu-id="2c6e4-110">Connect to the emulator account using a shortcut</span></span>
<span data-ttu-id="2c6e4-111">Csatlakozás a storage emulator az alkalmazás a legegyszerűbb módja a konfigurálhat egy kapcsolati karakterláncot az alkalmazás konfigurációs fájljában, amely hivatkozik a helyi `UseDevelopmentStorage=true`.</span><span class="sxs-lookup"><span data-stu-id="2c6e4-111">The easiest way to connect to the storage emulator from your application is to configure a connection string in your application's configuration file that references the shortcut `UseDevelopmentStorage=true`.</span></span> <span data-ttu-id="2c6e4-112">A storage emulator a kapcsolati karakterlánc példa egy *app.config* fájlt:</span><span class="sxs-lookup"><span data-stu-id="2c6e4-112">Here's an example of a connection string to the storage emulator in an *app.config* file:</span></span> 

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

#### <a name="connect-to-the-emulator-account-using-the-well-known-account-name-and-key"></a><span data-ttu-id="2c6e4-113">Csatlakozás a jól ismert fióknevet és a kulcs segítségével emulátor-fiókhoz</span><span class="sxs-lookup"><span data-stu-id="2c6e4-113">Connect to the emulator account using the well-known account name and key</span></span>
<span data-ttu-id="2c6e4-114">Hozzon létre egy kapcsolati karakterláncot, amely hivatkozik a emulátor fióknevet és kulcsot, meg kell adnia a végpontok minden, a szolgáltatások szeretné használni a kapcsolódási karakterláncban emulátorától.</span><span class="sxs-lookup"><span data-stu-id="2c6e4-114">To create a connection string that references the emulator account name and key, you must specify the endpoints for each of the services you wish to use from the emulator in the connection string.</span></span> <span data-ttu-id="2c6e4-115">Erre akkor szükség, úgy, hogy a kapcsolati karakterlánc használatával hivatkozik a emulátor végpontok, amelyek eltérnek a storage-fiók esetében.</span><span class="sxs-lookup"><span data-stu-id="2c6e4-115">This is necessary so that the connection string will reference the emulator endpoints, which are different than those for a production storage account.</span></span> <span data-ttu-id="2c6e4-116">Például a kapcsolati karakterlánc értékét fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="2c6e4-116">For example, the value of your connection string will look like this:</span></span>

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```

<span data-ttu-id="2c6e4-117">Ez az érték megegyezik a fenti, helyi `UseDevelopmentStorage=true`.</span><span class="sxs-lookup"><span data-stu-id="2c6e4-117">This value is identical to the shortcut shown above, `UseDevelopmentStorage=true`.</span></span>

#### <a name="specify-an-http-proxy"></a><span data-ttu-id="2c6e4-118">Adjon meg egy HTTP-proxy</span><span class="sxs-lookup"><span data-stu-id="2c6e4-118">Specify an HTTP proxy</span></span>
<span data-ttu-id="2c6e4-119">A szolgáltatás a storage emulatorban tesztelést használandó HTTP-proxy is megadható.</span><span class="sxs-lookup"><span data-stu-id="2c6e4-119">You can also specify an HTTP proxy to use when you're testing your service against the storage emulator.</span></span> <span data-ttu-id="2c6e4-120">Ez lehet hasznos, ha HTTP-kérések és válaszok betartásával, akkor a tárolási szolgáltatások műveleteket hibakeresése közben.</span><span class="sxs-lookup"><span data-stu-id="2c6e4-120">This can be useful for observing HTTP requests and responses while you're debugging operations against the storage services.</span></span> <span data-ttu-id="2c6e4-121">A proxy megadásához adja hozzá a `DevelopmentStorageProxyUri` a kapcsolati karakterlánc módosításait lehetőséget, és állítsa be az értékét a proxy URI.</span><span class="sxs-lookup"><span data-stu-id="2c6e4-121">To specify a proxy, add the `DevelopmentStorageProxyUri` option to the connection string, and set its value to the proxy URI.</span></span> <span data-ttu-id="2c6e4-122">Például itt található egy kapcsolati karakterláncot, amely a storage emulator mutat, és egy HTTP-proxy konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="2c6e4-122">For example, here is a connection string that points to the storage emulator and configures an HTTP proxy:</span></span>

```
UseDevelopmentStorage=true;DevelopmentStorageProxyUri=http://myProxyUri
```

