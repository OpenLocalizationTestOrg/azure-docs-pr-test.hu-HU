

Nincsenek elérhető Azure infrastruktúra-szolgáltatásokat a terheléselosztás két szintje:

* **DNS-szintű**: a különböző adatközpontokban vagy külső végpontok száma a terheléselosztást a forgalmat különböző felhőszolgáltatások más adatok mappában lévő csoportosítva, különböző Azure-webhelyek találhatók. Ez történik, az Azure Traffic Manager és a ciklikus multiplexelés terheléselosztási a betöltési metódust.
* **Hálózati szintű**: terheléselosztása bejövő internetes forgalmat különböző virtuális gépek egy felhőalapú szolgáltatás, vagy a felhőszolgáltatás vagy a virtuális hálózaton lévő virtuális gépek közötti forgalom terheléselosztása. Ez a lépés az Azure load balancer.

## <a name="traffic-manager-load-balancing-for-cloud-services-and-websites"></a>A TRAFFIC Manager terheléselosztást felhőszolgáltatások és webhelyek
Traffic Manager azonban lehetővé teszik a felhasználói forgalomnak a végpontok, amelyek magukban foglalhatják a felhőszolgáltatások, a webhelyek, a külső helyek és a más Traffic Manager-profilok terjesztése. Traffic Manager úgy működik az intelligens házirendmotor alkalmazása az internetes erőforrások tartománynevének tartománynévrendszer (DNS) lekérdezéseket. A felhőszolgáltatás vagy webhelyek futtathat különböző adatközpontokban keresztül történik.

Külső végpontok száma vagy a Traffic Manager-profilok beállítása végpontok REST vagy a Windows PowerShell kell használnia.

A TRAFFIC Manager forgalom terjesztésére terheléselosztás megvalósítására három módszert alkalmaz:

* **Feladatátvételi**: ezt a módszert használja, ha meg szeretné az összes forgalom az elsődleges végpont használatára, de adja meg a biztonsági mentések, abban az esetben, ha az elsődleges nem érhető el.
* **Teljesítmény**: ezt a módszert használja a végpontok különböző földrajzi helyeken található, ha azt szeretné, hogy kérő ügyfelek számára a "legközelebbi" végpont a legkisebb mértékű késleltetést tekintetében.
* **Ciklikus multiplexelés:** ezt a módszert használja, ha meg szeretné osszák szét a felhőalapú szolgáltatások ugyanabban az adatközpontban vagy felhőszolgáltatások vagy különböző adatközpontokban webhelyek között.

További információkért lásd: [kapcsolatos Traffic Manager terhelés terheléselosztás módszerek](../articles/traffic-manager/traffic-manager-routing-methods.md).

Az alábbi ábrán látható egy példa a ciklikus multiplexelés terheléselosztási módszer különböző felhőszolgáltatások közötti forgalom elosztásához.

![terheléselosztás](./media/virtual-machines-common-load-balance/TMSummary.png)

A folyamat alapvetően a következő történik:

1. Egy internetes ügyfél lekérdezi egy webszolgáltatás-bővítmény a tartomány nevével.
2. DNS továbbítja a név lekérdezés kérelmet felvétele a Traffic Manager.
3. A TRAFFIC Manager úgy dönt, a következő felhőszolgáltatás ciklikus multiplexelés listájában, és küld vissza a DNS-nevét. Az internetes ügyfél DNS-kiszolgáló nevét feloldja egy IP-címre, és elküldi az internetes ügyfél.
4. Az internetes ügyfél kapcsolódik a Traffic Manager által a kiválasztott felhőszolgáltatással.

További információkért lásd: [Traffic Manager](../articles/traffic-manager/traffic-manager-overview.md).

## <a name="azure-load-balancing-for-virtual-machines"></a>Az Azure virtuális gépek terheléselosztását
A virtuális gépet felvesz ugyanabba a felhőalapú szolgáltatás, vagy a virtuális hálózati kommunikálhatnak egymással közvetlenül a magánhálózati IP-címek használata. Számítógépek és szolgáltatások a felhőalapú szolgáltatás, vagy a virtuális hálózaton kívül csak kommunikálhatnak egy felhőalapú szolgáltatás, vagy a konfigurált kötéssel a következő virtuális hálózaton lévő virtuális gépek. A végpont a leképezés egy nyilvános IP-cím és port privát IP-címet, és a virtuális gép vagy egy Azure felhőszolgáltatást belül webes szerepkör port.

Az Azure terheléselosztó bejövő forgalmat egy adott típusú véletlenszerűen több virtuális gépek vagy szolgáltatások egy elosztott terhelésű készlet néven konfigurációban osztja el. Például a webes kérelem forgalom terhelését elosztva is több webkiszolgálók vagy webes szerepkörök.

Az alábbi ábrán egy elosztott terhelésű végpont szabványos (titkosítatlan) webes forgalom, amely a nyilvános és titkos TCP port a 80-as három virtuális gép között meg van osztva. A három virtuális gépek vannak egy elosztott terhelésű készlet.

![terheléselosztás](./media/virtual-machines-common-load-balance/LoadBalancing.png)

További információkért lásd: [Azure Load Balancer](../articles/load-balancer/load-balancer-overview.md). Hozzon létre egy elosztott terhelésű készletet lépéseiért lásd: [konfigurálása egy elosztott terhelésű készlet](../articles/load-balancer/load-balancer-get-started-internet-arm-ps.md).

Azure is be tudják tölteni a felhőalapú szolgáltatás vagy a virtuális hálózaton belül egyenleg. Ez az úgynevezett belső terheléselosztás, és a következőképpen használható:

* A többrétegű alkalmazások (például közötti web- és az Adatbázisréteg) külön rétegekhez kiszolgálója között terheléselosztásához.
* Egyenleg-üzletági (LOB) alkalmazások anélkül, hogy további load balancer hardver- vagy Azure-ban üzemeltetett betöltése.
* A helyszíni kiszolgálók tartalmazza a számítógépet, amelynek a forgalmát terhelés készletében átgondolni.

Az Azure load hasonló terheléselosztási, belső terheléselosztási megkönnyíthető egy belső elosztott terhelésű készlet konfigurálása.

Az alábbi ábrán látható egy példa egy belső elosztott terhelésű végpont üzletági (LOB) alkalmazás három virtuális gépek között a létesítmények közötti virtuális hálózaton lévő megosztott sor.

![terheléselosztás](./media/virtual-machines-common-load-balance/LOBServers.png)

## <a name="load-balancer-considerations"></a>Load balancer kapcsolatos szempontok
A terheléselosztó egy 4 percben üresjárat időkorlátja alapértelmezés szerint van konfigurálva. Ha az alkalmazás egy terheléselosztó mögött elhagyja a kapcsolat üresjáratban 4 percnél hosszabb ideig, és nem rendelkezik egy életben tartási konfigurációs, a kapcsolatot a rendszer eldobja. Módosíthatja a terheléselosztó GUID engedélyezi egy [hosszabb időtúllépés beállítása az Azure terheléselosztó](../articles/load-balancer/load-balancer-tcp-idle-timeout.md).

Egyéb szempont, hogy milyen típusú Azure terheléselosztó által támogatott terjesztési mód. Konfigurálhatja a forrás IP-kapcsolat (forrás IP-címe, cél IP-cím) vagy a forrás IP-protokoll (forrás IP-címe, cél IP-címe és protokoll). Tekintse meg [Azure Load Balancer módot (forrás IP-kapcsolat)](../articles/load-balancer/load-balancer-distribution-mode.md) további információt.

## <a name="next-steps"></a>Következő lépések
Hozzon létre egy elosztott terhelésű készletet lépéseiért lásd: [konfigurálása egy belső elosztott terhelésű készlet](../articles/load-balancer/load-balancer-get-started-ilb-arm-ps.md).

Terheléselosztó kapcsolatos további információkért lásd: [belső terheléselosztás](../articles/load-balancer/load-balancer-internal-overview.md).

