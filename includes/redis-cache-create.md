a gyorsítótár toocreate toohello először bejelentkezik [Azure-portálon](https://portal.azure.com), és kattintson **új** > **adatbázisok** > **Redis Cache**.

> [!NOTE]
> Ha nincs Azure-fiókja, [ingyenesen nyithat egyet](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero) mindössze pár perc alatt.
> 
> 

![Új gyorsítótár](media/redis-cache-create/redis-cache-new-cache-menu.png)

> [!NOTE]
> Ezenkívül toocreating gyorsítótárazza a hello Azure-portálon, is létrehozhat az erőforrás-kezelő használatával sablonok, a PowerShell vagy az Azure parancssori felület.
> 
> * a Resource Manager-sablonok használatával gyorsítótár toocreate lásd [egy sablon használatával Redis gyorsítótár létrehozása](../articles/redis-cache/cache-redis-cache-arm-provision.md).
> * a gyorsítótár Azure PowerShell használatával toocreate lásd [kezelése Azure Redis Cache az Azure PowerShell](../articles/redis-cache/cache-howto-manage-redis-cache-powershell.md).
> * a gyorsítótár Azure parancssori felület használatával toocreate lásd [hogyan toocreate és kezelése az Azure Redis Cache hello Azure parancssori felület (CLI) használatával](../articles/redis-cache/cache-manage-cli.md).
> 
> 

A hello **új Redis Cache** panelen adja meg a hello hello gyorsítótár kívánt beállításait.

![Gyorsítótár létrehozása](media/redis-cache-create/redis-cache-cache-create.png) 

* A **DNS-név**, adjon meg egy egyedi gyorsítótár neve toouse hello gyorsítótár végpontjához. hello gyorsítótár nevének 1 és 63 karakter közötti karakterláncnak kell és csak számokat, betűket és hello `-` karakter. hello gyorsítótár neve nem kezdődhet vagy végződhet hello `-` karakterrel, és egymást követő `-` karakterek nem érvényesek.
* A **előfizetés**, válassza ki a megjeleníteni kívánt toouse hello gyorsítótár Azure-előfizetés hello. Ha a fiókja csak egyetlen előfizetéssel rendelkezik, akkor lesz automatikusan kiválasztva, és hello **előfizetés** nem jelenik meg a legördülő listán.
* Az **Erőforráscsoport** listából válasszon ki vagy hozzon létre egy erőforráscsoportot a gyorsítótárhoz. További információkért lásd: [használatával erőforráscsoportokat toomanage az Azure-erőforrások](../articles/azure-resource-manager/resource-group-overview.md). 
* Használjon **hely** toospecify hello földrajzi hely, ahol a gyorsítótárat üzemeltetni kívánja. Hello legjobb teljesítmény érdekében a Microsoft nyomatékosan javasolja, hogy a hello hello gyorsítótár létrehozása és hello ügyfélalkalmazás ugyanabban a régióban.
* Használjon **tarifacsomag** tooselect hello szükségeskonfiguráció-gyorsítótár mérete és a szolgáltatásokat.
* **Redis-fürt** lehetővé teszi a toocreate gyorsítótárak nagyobb, mint 53 GB és tooshard adatok több Redis-csomópont között. További információkért lásd: [hogyan fürtözése a Premium Azure Redis Cache tooconfigure](../articles/redis-cache/cache-how-to-premium-clustering.md).
* **Redis-adatmegőrzés** hello képességét toopersist kínál a gyorsítótár tooan Azure Storage-fiók. Útmutatás az adatmegőrzés konfigurálásához: [hogyan tooconfigure megőrzését egy prémium szintű Azure Redis Cache](../articles/redis-cache/cache-how-to-premium-persistence.md).
* **Virtuális hálózati** biztosít magasabb védelmet és elszigeteltséget hozzáférés tooyour gyorsítótár tooonly korlátozásával megadott hello található ügyfelek Azure-beli virtuális hálózathoz. Használhatja a vnet összes hello szolgáltatás, például alhálózatok, hozzáférés-vezérlési házirendeket és egyéb szolgáltatások toofurther korlátozása a hozzáférés tooRedis. További információkért lásd: [hogyan támogatják a virtuális hálózati tooconfigure a Premium Azure Redis Cache](../articles/redis-cache/cache-how-to-premium-vnet.md).
* A nem SSL hozzáférés alapértelmezés szerint le van tiltva az új gyorsítótárakhoz. tooenable hello nem SSL port, ellenőrzés **6379 (nem SSL-titkosítású) port feloldásához**.

Hello új gyorsítótár beállításainak konfigurálása után kattintson **létrehozása**. Hello gyorsítótár toobe létrehozott néhány percet is igénybe vehet. toocheck hello állapotát, figyelheti a hello kezdőpulton hello előrehaladását. Hello gyorsítótár létrehozása után az új gyorsítótár tartalmaz egy **futtató** állapotát és való használatra kész [alapértelmezett beállítások](../articles/redis-cache/cache-configure.md#default-redis-server-configuration).

![A gyorsítótár létrejött](media/redis-cache-create/redis-cache-cache-created.png)

