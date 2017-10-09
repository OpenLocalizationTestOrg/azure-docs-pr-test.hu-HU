<span data-ttu-id="72c37-101">hello storage emulator megosztott kulcsos hitelesítést támogatja egy rögzített fiókhoz és egy jól ismert hitelesítési kulcs.</span><span class="sxs-lookup"><span data-stu-id="72c37-101">hello storage emulator supports a single fixed account and a well-known authentication key for Shared Key authentication.</span></span> <span data-ttu-id="72c37-102">A fiók és a kulcs nem hello csak megosztott kulcsos hitelesítő adatok hello storage emulator való használatra engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="72c37-102">This account and key are hello only Shared Key credentials permitted for use with hello storage emulator.</span></span> <span data-ttu-id="72c37-103">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="72c37-103">They are:</span></span>

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> <span data-ttu-id="72c37-104">hello storage emulator által támogatott hello hitelesítési kulcs csak tesztelési hello funkcióit az ügyfél-hitelesítési kód készült.</span><span class="sxs-lookup"><span data-stu-id="72c37-104">hello authentication key supported by hello storage emulator is intended only for testing hello functionality of your client authentication code.</span></span> <span data-ttu-id="72c37-105">Bármilyen biztonsági célt nem szolgál.</span><span class="sxs-lookup"><span data-stu-id="72c37-105">It does not serve any security purpose.</span></span> <span data-ttu-id="72c37-106">A termelési tárfiók és a kulcs hello storage emulator nem használható.</span><span class="sxs-lookup"><span data-stu-id="72c37-106">You cannot use your production storage account and key with hello storage emulator.</span></span> <span data-ttu-id="72c37-107">Ne használjon hello fejlesztői fiók termelési adatokkal.</span><span class="sxs-lookup"><span data-stu-id="72c37-107">You should not use hello development account with production data.</span></span>
> 
> <span data-ttu-id="72c37-108">hello storage emulator csak a HTTP Protokollon keresztül kapcsolatot támogat.</span><span class="sxs-lookup"><span data-stu-id="72c37-108">hello storage emulator supports connection via HTTP only.</span></span> <span data-ttu-id="72c37-109">Azonban a HTTPS protokoll egy éles Azure-tárfiók-erőforrások eléréséhez ajánlott hello.</span><span class="sxs-lookup"><span data-stu-id="72c37-109">However, HTTPS is hello recommended protocol for accessing resources in a production Azure storage account.</span></span>
> 

#### <a name="connect-toohello-emulator-account-using-a-shortcut"></a><span data-ttu-id="72c37-110">Csatlakozás parancsikonnal toohello emulátor fiók</span><span class="sxs-lookup"><span data-stu-id="72c37-110">Connect toohello emulator account using a shortcut</span></span>
<span data-ttu-id="72c37-111">hello legegyszerűbb módja tooconnect toohello storage emulator az alkalmazásról egy kapcsolati karakterláncot az alkalmazás konfigurációs fájljában hello helyi hivatkozó tooconfigure `UseDevelopmentStorage=true`.</span><span class="sxs-lookup"><span data-stu-id="72c37-111">hello easiest way tooconnect toohello storage emulator from your application is tooconfigure a connection string in your application's configuration file that references hello shortcut `UseDevelopmentStorage=true`.</span></span> <span data-ttu-id="72c37-112">A kapcsolati karakterlánc toohello a storage emulatort, például egy *app.config* fájlt:</span><span class="sxs-lookup"><span data-stu-id="72c37-112">Here's an example of a connection string toohello storage emulator in an *app.config* file:</span></span> 

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

#### <a name="connect-toohello-emulator-account-using-hello-well-known-account-name-and-key"></a><span data-ttu-id="72c37-113">Csatlakozás toohello emulátor fiókját hello jól ismert fióknevet és kulcsot</span><span class="sxs-lookup"><span data-stu-id="72c37-113">Connect toohello emulator account using hello well-known account name and key</span></span>
<span data-ttu-id="72c37-114">toocreate egy kapcsolati karakterláncot, hogy hivatkozásokat hello emulátor fióknevet és kulcs, meg kell adnia hello végpontok egyes hello szolgáltatási meg akarja toouse hello emulátorától hello kapcsolat-karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="72c37-114">toocreate a connection string that references hello emulator account name and key, you must specify hello endpoints for each of hello services you wish toouse from hello emulator in hello connection string.</span></span> <span data-ttu-id="72c37-115">Erre akkor szükség, így hello kapcsolati karakterlánc használatával hello emulátor végpontok, amelyek eltérnek a termelési tárfiókon hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="72c37-115">This is necessary so that hello connection string will reference hello emulator endpoints, which are different than those for a production storage account.</span></span> <span data-ttu-id="72c37-116">Például a kapcsolati karakterlánc értékét hello fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="72c37-116">For example, hello value of your connection string will look like this:</span></span>

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```

<span data-ttu-id="72c37-117">Az értéket nem látható a fenti azonos toohello helyi `UseDevelopmentStorage=true`.</span><span class="sxs-lookup"><span data-stu-id="72c37-117">This value is identical toohello shortcut shown above, `UseDevelopmentStorage=true`.</span></span>

#### <a name="specify-an-http-proxy"></a><span data-ttu-id="72c37-118">Adjon meg egy HTTP-proxy</span><span class="sxs-lookup"><span data-stu-id="72c37-118">Specify an HTTP proxy</span></span>
<span data-ttu-id="72c37-119">Ha a szolgáltatás hello storage emulatorban tesztelést egy HTTP-proxy toouse is megadható.</span><span class="sxs-lookup"><span data-stu-id="72c37-119">You can also specify an HTTP proxy toouse when you're testing your service against hello storage emulator.</span></span> <span data-ttu-id="72c37-120">Ez lehet hasznos, ha HTTP-kérések és válaszok betartásával, akkor hibakeresése hello tárolószolgáltatások műveleteket közben.</span><span class="sxs-lookup"><span data-stu-id="72c37-120">This can be useful for observing HTTP requests and responses while you're debugging operations against hello storage services.</span></span> <span data-ttu-id="72c37-121">a proxy toospecify hozzáadása hello `DevelopmentStorageProxyUri` toohello kapcsolati karakterlánc lehetőséget, és állítsa be az érték toohello proxy URI.</span><span class="sxs-lookup"><span data-stu-id="72c37-121">toospecify a proxy, add hello `DevelopmentStorageProxyUri` option toohello connection string, and set its value toohello proxy URI.</span></span> <span data-ttu-id="72c37-122">Például: Itt toohello storage emulator mutat, és konfigurálja a HTTP-proxy kapcsolati karakterlánc:</span><span class="sxs-lookup"><span data-stu-id="72c37-122">For example, here is a connection string that points toohello storage emulator and configures an HTTP proxy:</span></span>

```
UseDevelopmentStorage=true;DevelopmentStorageProxyUri=http://myProxyUri
```

