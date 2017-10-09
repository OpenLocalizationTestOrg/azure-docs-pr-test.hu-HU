Fontos, hogy van két módon tooconfigure egy rendelkezésre állási csoport figyelőjének Azure toorealize. hello módja nem egyezik a hello típusú hello figyelő létrehozásakor használja az Azure terheléselosztóhoz. a következő táblázat hello hello különbségeket mutatja be:

| Terheléselosztó típusa | Megvalósítás | A következő esetekben használja: |
| --- | --- | --- |
| **Külső** |Felhasználási hello *nyilvános virtuális IP-cím* hello felhőalapú szolgáltatás, amely hello virtuális gépek (VM) üzemelteti. |A külső hello virtuális hálózatról, többek között a hello Internet tooaccess hello figyelő van szüksége. |
| **Belső** |Használja az *belső terheléselosztó* hello figyelő titkos címével. |Van-e hozzáférési hello figyelő csak hello belül azonos virtuális hálózatban. A hozzáférés a hibrid környezetek telephelyek közötti VPN tartalmazza. |

> [!IMPORTANT]
> A figyelő az, hogy hello felhőalapú szolgáltatás nyilvános VIP (külső terheléselosztó), lehető leghosszabbak hello ügyfél, a figyelő és az adatbázisok szerepelnek használ hello azonos Azure-régió, után nem számítunk kilépő díjakat. Ellenkező esetben bármely az adatok visszaadásához hello keresztül figyelő kilépő minősül, és normál adatátvitel ütemben fel van töltve. 
> 
> 

Egy ILB csak virtuális hálózatokat regionális hatókörbe és konfigurálhatja. Meglévő virtuális hálózatot, amely egy affinitáscsoporthoz konfigurált egy ILB nem használható. További információkért lásd: [belső load balancer áttekintése](../articles/load-balancer/load-balancer-internal-overview.md).

