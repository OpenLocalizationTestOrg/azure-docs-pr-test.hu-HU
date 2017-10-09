

Nincsenek elérhető Azure infrastruktúra-szolgáltatásokat a terheléselosztás két szintje:

* **DNS-szintű**: más adatok terheléselosztás a forgalom toodifferent felhőszolgáltatások található erőforrások, Azure-webhelyek toodifferent különböző adatközpontokban vagy található tooexternal végpontok. Ez történik, az Azure Traffic Manager, és ciklikus multiplexelés hello betölteni a terheléselosztási mód.
* **Hálózati szintű**: terheléselosztás bejövő internetes forgalom toodifferent virtuális gépek egy felhőalapú szolgáltatás, vagy a felhőszolgáltatás vagy a virtuális hálózaton lévő virtuális gépek közötti forgalom terheléselosztása. Ez a lépés hello Azure terheléselosztó.

## <a name="traffic-manager-load-balancing-for-cloud-services-and-websites"></a>A TRAFFIC Manager terheléselosztást felhőszolgáltatások és webhelyek
A TRAFFIC Manager lehetővé teszi felhasználói forgalom tooendpoints, ami magában foglalhatja a felhőszolgáltatások, a webhelyek, a külső helyek és a más Traffic Manager-profilok toocontrol hello terjesztése. A TRAFFIC Manager működik az intelligens házirend motor tooDomain tartománynévrendszer (DNS) lekérdezések hello tartománynevek az internetes erőforrások alkalmazásával. A felhőszolgáltatás vagy webhelyek futtathat különböző adatközpontokban hello world között.

REST- vagy Windows PowerShell tooconfigure külső végpontok vagy a Traffic Manager-profilok végpontként kell használnia.

A TRAFFIC Manager által használt három terheléselosztás módszerek toodistribute forgalmat:

* **Feladatátvételi**: ezt a módszert használja, elsődleges végpont toouse összes forgalom, de adja meg a biztonsági mentések, abban az esetben, ha elsődleges hello nem érhető el.
* **Teljesítmény**: ezt a módszert használja a végpontok különböző földrajzi helyeken található, ha azt szeretné, hogy a kérő ügyfelek toouse hello "legközelebbi" végpont hello legkisebb mértékű késleltetést tekintetében.
* **Ciklikus multiplexelés:** ezt a módszert használja, ha azt szeretné, hogy ugyanaz a hello szolgáltatásai toodistribute betöltése között felhő készlete datacenter vagy a felhőszolgáltatás vagy webhely különböző adatközpontokban.

További információkért lásd: [kapcsolatos Traffic Manager terhelés terheléselosztás módszerek](../articles/traffic-manager/traffic-manager-routing-methods.md).

a következő diagram hello hello ciklikus multiplexelés terhelés terheléselosztási mód különböző felhőszolgáltatások közötti forgalom elosztásához példáját mutatja be.

![terheléselosztás](./media/virtual-machines-common-load-balance/TMSummary.png)

hello folyamat alapvetően a hello következő történik:

1. Egy internetes ügyfél lekérdezi a tartomány neve megfelelő tooa webes szolgáltatás.
2. DNS hello neve lekérdezés kérelem tooTraffic Manager továbbítja.
3. A TRAFFIC Manager hello tovább a felhőalapú szolgáltatás választ a hello ciklikus multiplexelés lista, és küld vissza hello DNS-nevét. hello internetes ügyfél DNS-kiszolgáló oldja fel a hello neve tooan IP-címet, és elküldi azt toohello internetes ügyfél.
4. Traffic Manager által kiválasztott hello felhőszolgáltatás hello internetes ügyfél kapcsolódik.

További információkért lásd: [Traffic Manager](../articles/traffic-manager/traffic-manager-overview.md).

## <a name="azure-load-balancing-for-virtual-machines"></a>Az Azure virtuális gépek terheléselosztását
A virtuális gépek hello ugyanazt a felhőalapú szolgáltatás, vagy a virtuális hálózati kommunikálhatnak egymással közvetlenül a saját privát IP-címek használatával. Számítógépek és szolgáltatások hello kívül a felhőalapú szolgáltatás, vagy a virtuális hálózat csak egy felhőalapú szolgáltatás, vagy a konfigurált kötéssel a következő virtuális hálózaton lévő virtuális gépek kommunikálhatnak. A végpont egy leképezésnek a nyilvános IP-cím és port toothat magánhálózati IP-cím és port egy virtuális gép vagy a webes szerepkör egy Azure felhőszolgáltatást belül.

hello Azure terheléselosztó bejövő forgalmat egy adott típusú véletlenszerűen terjeszti több virtuális gépek vagy szolgáltatások egy elosztott terhelésű készlet néven konfigurációban. Például a webes kérelem forgalom terhelését hello elosztva is több webkiszolgálók vagy webes szerepkörök.

hello alábbi ábrán látható egy elosztott terhelésű végpont hello nyilvános és titkos TCP-port a 80-as három virtuális gépek által közösen használt szokásos (titkosítatlan) webes forgalom. A három virtuális gépek vannak egy elosztott terhelésű készlet.

![terheléselosztás](./media/virtual-machines-common-load-balance/LoadBalancing.png)

További információkért lásd: [Azure Load Balancer](../articles/load-balancer/load-balancer-overview.md). Hello lépések toocreate egy elosztott terhelésű készlet, lásd: [konfigurálása egy elosztott terhelésű készlet](../articles/load-balancer/load-balancer-get-started-internet-arm-ps.md).

Azure is be tudják tölteni a felhőalapú szolgáltatás vagy a virtuális hálózaton belül egyenleg. Ez a belső terheléselosztás nevezik, és használható a következő módokon hello:

* a többrétegű alkalmazások (például közötti web- és az Adatbázisréteg) külön rétegekhez kiszolgálók közötti egyensúly tooload.
* tooload egyenleg-üzletági (LOB) alkalmazások anélkül, hogy további load balancer hardver- vagy Azure-ban üzemeltetett.
* tooinclude helyszíni kiszolgálók a számítógépet, amelynek a forgalmát terhelésű hello készletét.

Hasonló tooAzure terheléselosztási, belső terheléselosztási megkönnyíthető egy belső elosztott terhelésű készlet konfigurálása.

a következő diagram hello belső elosztott terhelésű végpont egy üzletági (LOB) alkalmazás három virtuális gépek között a létesítmények közötti virtuális hálózaton lévő megosztott példáját mutatja be.

![terheléselosztás](./media/virtual-machines-common-load-balance/LOBServers.png)

## <a name="load-balancer-considerations"></a>Load balancer kapcsolatos szempontok
A terheléselosztó alapértelmezés szerint van konfigurálva az üresjárat 4 percben tootimeout. Ha az alkalmazás egy terheléselosztó mögött elhagyja a kapcsolat üresjáratban 4 percnél hosszabb ideig, és nem rendelkezik egy életben tartási konfigurációs, hello kapcsolatot a rendszer eldobja. Módosíthatja a hello load balancer viselkedés tooallow egy [hosszabb időtúllépés beállítása az Azure terheléselosztó](../articles/load-balancer/load-balancer-tcp-idle-timeout.md).

Egyéb szempont, hogy hello típusú Azure terheléselosztó által támogatott terjesztési mód. Konfigurálhatja a forrás IP-kapcsolat (forrás IP-címe, cél IP-cím) vagy a forrás IP-protokoll (forrás IP-címe, cél IP-címe és protokoll). Tekintse meg [Azure Load Balancer módot (forrás IP-kapcsolat)](../articles/load-balancer/load-balancer-distribution-mode.md) további információt.

## <a name="next-steps"></a>Következő lépések
Hello lépések toocreate egy elosztott terhelésű készlet, lásd: [konfigurálása egy belső elosztott terhelésű készlet](../articles/load-balancer/load-balancer-get-started-ilb-arm-ps.md).

Terheléselosztó kapcsolatos további információkért lásd: [belső terheléselosztás](../articles/load-balancer/load-balancer-internal-overview.md).

