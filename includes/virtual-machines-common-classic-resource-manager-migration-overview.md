# <a name="platform-supported-migration-of-iaas-resources-from-classic-tooazure-resource-manager"></a>IaaS-erőforrásokra, erőforrás-kezelő klasszikus tooAzure a platform által támogatott áttelepítése
Ez a cikk azt ismerteti hogyan azt még engedélyezése egy szolgáltatási (IaaS) erőforrásként a hello klasszikus tooResource Manager üzembe helyezési modellel infrastruktúra áttelepítését. További tudnivalók [Azure erőforrás-kezelő szolgáltatásait és előnyeit](../articles/azure-resource-manager/resource-group-overview.md). Hogyan tooconnect erőforrásokat hello két üzembe helyezési modellel, amelyek futhatnak az előfizetésben a virtuális hálózati helyek átjárók részletességi azt.

## <a name="goal-for-migration"></a>Áttelepítés célja
Erőforrás-kezelő lehetővé teszi, hogy a sablonok összetett alkalmazások telepítését, konfigurálja a virtuális gépek Virtuálisgép-bővítmények használatával, és magában foglalja a hozzáférés-kezelés és a címkézést. Az Azure Resource Manager méretezhető, párhuzamos szolgáltatássablonjaikat a virtuális gépek rendelkezésre állási készletek tartalmazza. hello új üzembe helyezési modellben is biztosít a számítási, hálózati és tárolási életciklus-felügyeletének egymástól függetlenül. Végül pedig a fókusz a biztonsági alapértelmezés szerint a virtuális hálózatban lévő virtuális gépek hello végrehajtásának engedélyezése.

Szinte minden hello szolgáltatás hello klasszikus telepítési modellből a számítási, hálózati és tárolási az Azure Resource Manager használata támogatott. a hello új képességek az Azure Resource Manager toobenefit, áttelepítheti a meglévő telepítések hello klasszikus telepítési modellből.

## <a name="supported-resources-for-migration"></a>Az áttelepítéshez támogatott erőforrások
A klasszikus IaaS-erőforrásokra támogatott az áttelepítés során

* Virtuális gépek
* Rendelkezésre állási csoportok
* Cloud Services
* Tárfiókok
* Virtuális hálózatok
* VPN Gateway átjárók
* Express Route átjárók _(a hello csak virtuális hálózatnak azonos előfizetésben)_
* Network Security Groups (Hálózati biztonsági csoportok) 
* Útvonaltáblák 
* Fenntartott IP-címek 

## <a name="supported-scopes-of-migration"></a>Az áttelepítés támogatott hatókörök
Többféleképpen 4 toocomplete áttelepítése a számítási, hálózati és tárolási erőforrásokat. Ezek a 

* (Nem része virtuális hálózatnak) virtuális gépek áttelepítése
* Virtuális gépek (a virtuális hálózat) áttelepítése
* Fiókok tárolóáttelepítés
* Leválasztott erőforrások (hálózati biztonsági csoportok, Útvonaltábláit & fenntartott IP-címek)

### <a name="migration-of-virtual-machines-not-in-a-virtual-network"></a>(Nem része virtuális hálózatnak) virtuális gépek áttelepítése
Hello Resource Manager üzembe helyezési modellel biztonsági kikényszeríti az alkalmazások alapértelmezett. Minden virtuális gép toobe hello Resource Manager modellt a virtuális hálózatban kell. az Azure platform újraindítást hello (`Stop`, `Deallocate`, és `Start`) virtuális gépek hello hello áttelepítés részeként. Két lehetőség közül választhat a hello virtuális hálózatok, virtuális gépek hello telepíti át:

* Hello platform toocreate egy új virtuális hálózati kérelmek, és hello virtuális gép áttelepítéséhez hello új virtuális hálózat.
* Létrehozni meglévő virtuális hálózatban az erőforrás-kezelőben hello virtuális gépet telepíthet át.

> [!NOTE]
> Az áttelepítési hatókörében mindkét felügyeleti-vezérlősík műveletek hello és hello adatok-vezérlősík műveletek nem engedélyezhető egy adott időn belül hello az áttelepítés során.
>
>

### <a name="migration-of-virtual-machines-in-a-virtual-network"></a>Virtuális gépek (a virtuális hálózat) áttelepítése
A legtöbb Virtuálisgép-konfigurációk esetén csak a hello metaadatok áttelepíti a hello klasszikus és Resource Manager üzembe helyezési modellek között. hello alapul szolgáló virtuális gépek futnak, hello ugyanazt a hardvert, az azonos hálózati, és a hello azonos hello tároló. hello felügyeleti-vezérlősík műveletek nem engedélyezhető egy bizonyos időn hello az áttelepítés során. Hello adatok vezérlősík azonban toowork továbbra is. Ez azt jelenti, hogy a virtuális gépek (klasszikus) felett futó alkalmazások nem számítunk állásidő hello az áttelepítés során.

a következő konfigurációk hello jelenleg nem támogatottak. Támogatás a jövőbeli hello ad hozzá, ha egyes virtuális gépek ebben a konfigurációban előfordulhat, hogy fel Önnek állásidő (Nyissa meg felszabadítani a stop, keresztül, majd indítsa újra a virtuális gép műveletek).

* Egynél több rendelkezésre állási egyetlen felhőszolgáltatás csoport rendelkezik.
* Rendelkezik egy vagy több rendelkezésre állási készletek és a virtuális gépek, amelyek nem tagjai rendelkezésre állási készlet egyetlen felhőszolgáltatásban.

> [!NOTE]
> Az áttelepítés hatókörében hello felügyeleti vezérlősík nem lehet engedélyezni, egy adott időn belül hello az áttelepítés során. Bizonyos konfigurációk ismertetett módon, adat-vezérlősík leállás.
>
>

### <a name="storage-accounts-migration"></a>Fiókok tárolóáttelepítés
tooallow zökkenőmentes áttelepítés, erőforrás-kezelő virtuális gépeket telepíthet klasszikus tárfiókokban. Ezzel a lehetőséggel a számítási és hálózati erőforrásokat is, és a storage-fiókok függetlenül át kell telepíteni. Miután áttelepítette a virtuális gépek és a virtuális hálózat felett, a tárolási fiók toocomplete hello áttelepítési folyamat kell toomigrate.

> [!NOTE]
> hello Resource Manager üzembe helyezési modellben nem rendelkezik a klasszikus lemezképet és lemezt hello fogalmát. Ha hello tárfiók áttelepített, klasszikus képek és lemezek nem láthatók a hello Resource Manager-készletben, de biztonsági hello hello tárfiók VHD-k maradnak.
>
>

### <a name="unattached-resources-network-security-groups-route-tables--reserved-ips"></a>Leválasztott erőforrások (hálózati biztonsági csoportok, Útvonaltábláit & fenntartott IP-címek)
Hálózati biztonsági csoportok, Útvonaltábláit & fenntartott IP-címek, amelyek nincsenek csatlakoztatva a tooany virtuális gépek és virtuális hálózatokat is egymástól függetlenül telepíthető át.

<br>

## <a name="unsupported-features-and-configurations"></a>Nem támogatott funkciókat és konfigurációk
A Microsoft jelenleg nem támogatja bizonyos szolgáltatásokat és konfigurációkat. hello a következő szakaszok ismertetik a Javaslataink a felhasználókat ezekbe a csoportokba.

### <a name="unsupported-features"></a>Nem támogatott funkciók
a következő funkciók hello jelenleg nem támogatottak. Opcionálisan eltávolítsa ezeket a beállításokat, hello virtuális gépek és majd engedélyezze újra hello beállítások hello Resource Manager üzembe helyezési modellben.

| Erőforrás-szolgáltató | Szolgáltatás | Ajánlás |
| --- | --- | --- |
| Számítás |Társítatlan virtuálisgép-lemezeket. | Ezek a lemezek mögött hello VHD-blobok fog települnek át hello Tárfiók áttelepítésekor |
| Számítás |Virtuálisgép-lemezképeket. | Ezek a lemezek mögött hello VHD-blobok fog települnek át hello Tárfiók áttelepítésekor |
| Network (Hálózat) |Végponti ACL-eket. | Távolítsa el a végponti ACL-eket, és ismételje meg az áttelepítés. |
| Network (Hálózat) |Virtuális hálózati ExpressRoute-átjáró és a VPN-átjáró  | Távolítsa el a VPN-átjáró hello áttelepítésének megkezdése előtt, és majd hozza újra létre VPN Gateway hello áttelepítés befejezése után. További információ [ExpressRoute áttelepítési](../articles/expressroute/expressroute-migration-classic-resource-manager.md).|
| Network (Hálózat) |ExpressRoute engedélyezési hivatkozásokkal  | Hello ExpressRoute körön toovirtaul hálózati kapcsolat áttelepítésének megkezdése előtt távolítsa el, és ezután hozza létre újra hello kapcsolatot áttelepítés befejezése után. További információ [ExpressRoute áttelepítési](../articles/expressroute/expressroute-migration-classic-resource-manager.md). |
| Network (Hálózat) |Application Gateway | Távolítsa el a hello Application Gateway áttelepítésének megkezdése előtt, és ezután hozza létre újra hello Alkalmazásátjáró áttelepítés befejezése után. |
| Network (Hálózat) |Virtuális hálózatok használatával a Vnetben társviszony-létesítés. | Telepítse át a virtuális hálózati tooResource Manager, majd a partnert. További információ [Vnetben társviszony-létesítés](../articles/virtual-network/virtual-network-peering-overview.md). | 

### <a name="unsupported-configurations"></a>Nem támogatott konfigurációk
a következő konfigurációk hello jelenleg nem támogatottak.

| Szolgáltatás | Konfiguráció | Ajánlás |
| --- | --- | --- |
| Resource Manager |Szerepköralapú hozzáférés vezérlés (RBAC) hagyományos erőforrások |Mivel hello hello erőforrás URI Azonosítóját az áttelepítés után módosult, ajánlott hello RBAC házirend-frissítési áttelepítés után toohappen igénylő megtervezni. |
| Számítás |A virtuális gépek társított több alhálózattal |Hello alhálózat konfigurációs tooreference csak alhálózatok frissítése. |
| Számítás |Virtuális gépek virtuális hálózati tooa tartozó, de nincs hozzárendelve egy explicit alhálózati |Virtuális gép hello opcionálisan törölheti. |
| Számítás |Riasztások, automatikus skálázás házirendek rendelkező virtuális gépek |hello áttelepítési megy keresztül, és ezeket a beállításokat a rendszer eldobja. Erősen ajánlott a környezet kiértékelése előtt áttelepítési hello. Azt is megteheti újrakonfigurálhatja hello riasztási beállítások az áttelepítés befejezése után. |
| Számítás |XML-Virtuálisgép-bővítmények (BGInfo 1.*, a Visual Studio hibakereső funkcióját, a Web Deploy és távoli hibakeresés) |Ez nem támogatott. Javasoljuk, hogy ezek a bővítmények eltávolítása hello virtuális gép toocontinue áttelepítése, vagy azokat a rendszer eldobja automatikusan hello áttelepítési folyamat során. |
| Számítás |Prémium szintű Storage a rendszerindítási diagnosztika |Áttelepítés folytatása előtt tiltsa le a virtuális gépek hello rendszerindítási diagnosztikai funkciót. Hello Resource Manager-készletben a rendszerindítási diagnosztikát hello áttelepítés befejezése után újra engedélyezheti. Emellett képernyőkép és soros naplók használt blobot törölni kell, már nem az adott blobok van szó. |
| Számítás |Webes vagy feldolgozói szerepköröket tartalmazó felhőszolgáltatások |Ez jelenleg nem támogatott. |
| Network (Hálózat) |Virtuális hálózatok, virtuális gépek és a webes vagy feldolgozói szerepköröket tartalmazó |Ez jelenleg nem támogatott. |
| Azure App Service |Virtuális hálózatok, amelyek tartalmazzák az App Service-környezetek |Ez jelenleg nem támogatott. |
| Az Azure HDInsight |Virtuális hálózatok, amelyek tartalmazzák a HDInsight-szolgáltatások |Ez jelenleg nem támogatott. |
| A Microsoft Dynamics életciklus szolgáltatások |Dynamics életciklus szolgáltatások által kezelt virtuális gépeket tartalmazó virtuális hálózatok |Ez jelenleg nem támogatott. |
| Azure AD Domain Services |Virtuális hálózatok, amelyek tartalmazzák az Azure AD tartományi szolgáltatások |Ez jelenleg nem támogatott. |
| Azure RemoteApp |Virtuális hálózatok, amelyek tartalmazzák az Azure RemoteApp központi telepítések |Ez jelenleg nem támogatott. |
| Azure API Management |Virtuális hálózatok, amelyek tartalmazzák az Azure API Management központi telepítések |Ez jelenleg nem támogatott. toomigrate hello IaaS virtuális hálózat, módosítsa az API Management-környezetben, amely nincs állásidő művelet hello VNET hello. |
| Számítás |Az Azure Security Center bővítményei a virtuális hálózaton, amelynek átvitel kapcsolatot a VPN-átjáró vagy ExpressRoute-átjárót a helyszínen DNS-kiszolgálóval |Az Azure Security Center automatikusan a virtuális gépek toomonitor bővítmények telepíti a biztonsági, és riasztást. Ezek a bővítmények általában telepíteni automatikusan Ha hello Azure Security Center-házirendben engedélyezve van a hello előfizetés. ExpressRoute-átjáró áttelepítése jelenleg nem támogatott, és a VPN-átjárók összefüggő átvitel elveszítette a hozzáférését a helyszíni. ExpressRoute törlése átjáró vagy áttelepítése VPN-átjáró összefüggő átvitel hatására internet access tooVM tárolási fiók toobe történő Folytatás a hello áttelepítés végrehajtása. hello áttelepítési nem folytatódik, ha ez történik, mint hello Vendég ügynök állapotblobja nem tölthetők fel. Az Azure Security Center házirend hello előfizetésben toodisable 3 óra áttelepítés folytatása előtt ajánlott. |

