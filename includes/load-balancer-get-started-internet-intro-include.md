Az Azure Load Balancer 4. szintű (TCP, UDP) terheléselosztónak minősül. hello terheléselosztó bejövő forgalmat a felhőszolgáltatások kifogástalan szolgáltatáspéldány vagy a load balancer csoportban lévő virtuális gépek között elosztásával magas rendelkezésre állást biztosít. Az Azure Load Balancer a szolgáltatásokat több portra vagy több IP-címre, illetve portokra és IP-címekre egyaránt továbbíthatja.

A terheléselosztót az alábbi feladatokra lehet konfigurálni:

* Gépek terhelést elosztani bejövő internetes forgalom toovirtual (VM). Ezt a forgatókönyvet tooa terheléselosztó irányítjuk egy [internetre terheléselosztó](../articles/load-balancer/load-balancer-internet-overview.md).
* Forgalom terheléselosztása virtuális hálózat (VNet) virtuális gépei között, felhőszolgáltatások virtuális gépei között, vagy helyszíni számítógépek és létesítmények közötti virtuális hálózat virtuális gépei között. Ezt a forgatókönyvet tooa terheléselosztó irányítjuk egy [belső terheléselosztón (ILB)](../articles/load-balancer/load-balancer-internal-overview.md).
* Külső forgalom tooa adott Virtuálisgép-példány.
