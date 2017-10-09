Az Azure felhőmegoldásai virtuális gépekre (fizikai számítógép hardverelemeinek emulációira) épülnek, ami lehetőséget teremt a szoftvertelepítések gyors csomagolására, illetve a fizikai hardvereknél jobb erőforrás-konszolidációra. [Docker](https://www.docker.com) tárolók és hello docker-ökoszisztéma rendelkezik jelentősen bővített hello módon fejleszthet, küldje el, és elosztott szoftverek kezeléséhez. A tárolóban lévő alkalmazáskód el különítve a hello állomást a virtuális Gépet, és más tárolók a hello azonos virtuális gép. Ez az elkülönítés gyorsabb fejlesztést és üzembe helyezést tesz lehetővé.

Az Azure Docker értékeket a következő hello kínál:

* [Sok](../articles/virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) [különböző](../articles/virtual-machines/linux/dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) módon toocreate Docker üzemelteti a tárolók toosuit a helyzet
* Hello [Azure Tárolószolgáltatás](https://azure.microsoft.com/documentation/services/container-service/) hoz létre a tároló-állomást orchestrators, mint a fürtök **marathon** és **swarm**.
* [Az Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md) és [erőforrás csoport sablonok](../articles/resource-group-authoring-templates.md) toosimplify üzembe helyezése és összetett elosztott alkalmazások frissítése
* Integráció jogvédett és nyílt forráskódú konfigurációkezelési eszközök széles választékával.

És mivel programozott módon hozhat létre virtuális gépek és a Linux tárolók a Azure-ban is használható virtuális gép és tároló *vezénylési* toocreate csoportok virtuális gépek (VM) és toodeploy alkalmazások mindkét Linux belüli eszközök tárolók most [Windows tárolók](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview).

Nem csak a cikkben ezekről a fogalmakról magas szinten, hivatkozások toomore információk, oktatóanyagok, tonna is tartalmaz, és a termékek kapcsolódó toocontainer és a fürt használata az Azure-on. Ha tudja, hogy minden ez, és most szeretné, hogy hello hivatkozások, fontosságúak azonnal itt [tárolók használata az eszközök](#tools-for-working-with-azure-vms-and-containers).

## <a name="hello-difference-between-virtual-machines-and-containers"></a>virtuális gépek és a tárolók hello különbségének
A virtuális gépek egy [hipervizor](http://en.wikipedia.org/wiki/Hypervisor) által biztosított, elkülönített hardvervirtualizálási környezetben futnak. Az Azure hello [virtuális gépek](https://azure.microsoft.com/services/virtual-machines/) szolgáltatás leíróinak, hogy az Ön: hello operációs rendszer kiválasztása és konfigurálása a virtuális gépek létrehozása &mdash;vagy egy egyéni Virtuálisgép-lemezkép feltöltése. Virtuális gépek time-tested, "túlélésért folyó megerősítve" technológia, és sok eszközök elérhető toomanage hello az operációs rendszer és alkalmazások tartalmazzák.  A virtuális alkalmazások rejtve maradnak az hello gazda operációs rendszer számára. Hello szempontból egy alkalmazás vagy a felhasználó a virtuális gép hello VM toobe egy önálló fizikai számítógép jelenik meg.

[Linux-tárolók](http://en.wikipedia.org/wiki/LXC) és a létrehozott és tárolt docker eszközökkel, ne használja a hipervizor tooprovide elkülönítési. A tárolók a hello tároló-gazdagépen folyamat és a fájlrendszer elkülönítési szolgáltatásokat hello Linux kernel tooexpose toohello tároló, az alkalmazások, bizonyos kernel szolgáltatások és a saját elkülönített fájlrendszer használja. Hello szempontból egy alkalmazás egy tároló belül futó hello tároló toobe egyedi OS példány jelenik meg. A tárolóban található alkalmazások nem láthatják a tárolón kívüli folyamatokat vagy erőforrásokat.

A Docker-tárolók sokkal kevesebb erőforrást használnak, mint a virtuális gépek. Docker-tárolók egy alkalmazás elkülönítési és végrehajtási modell, amely nem osztható meg hello kernel hello Docker fogadó alkalmaz. hello tároló rendelkezik sokkal alacsonyabb lemez erőforrásigényét, nem tartoznak bele az operációs rendszer teljes hello. Az indítási idő és az elfoglalt lemezterület sokkal kevesebb, mint a virtuális gépek esetében.
Windows-tárolók Linux tárolóként ugyanezeket az előnyöket biztosítanak hello Windows rendszeren futó alkalmazások. Windows-tárolók hello Docker képformátum és Docker API támogatja, de ezek is kezelhetők PowerShell használatával. Windows-tárolók, Windows Server-tárolók és Hyper-V-tárolók esetében kétféle tároló-futtatókörnyezet érhető el. A Hyper-V-tárolók egy további elkülönítési réteget biztosítanak azáltal, hogy az egyes tárolókat nagymértékben optimalizált virtuális gépeken üzemeltetik. Windows-tárolók kapcsolatos információkért tekintse meg toolearn [Windows tárolók](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview). a Windows Azure, a tárolók használatába tooget megtudhatja, hogyan túl[Azure Tárolószolgáltatás-fürt üzembe helyezése](../articles/container-service/dcos-swarm/container-service-deployment.md).

## <a name="what-are-containers-good-for"></a>Mire jók a tárolók?

A tárolók képesek javítani:

* hello sebesség alkalmazáskód fejlesztett, és megosztott széles körben
* hello sebesség és a abban, hogy egy alkalmazás tesztelhető
* hello sebesség és a abban, hogy az alkalmazások is telepíthető.

A tárolók operációs rendszert&mdash;üzemeltető tárolón futnak, ami pedig az Azure-ban egy Azure-alapú virtuális gépet jelent. Akkor is, ha már kedvelt hello meghatározni, hogy tárolók, továbbra is fog tooneed egy virtuális gép infrastruktúra hello tárolók üzemeltető, de hello előnyt, hogy nem fontos a tárolók virtuálisgép futnak (bár szeretne-e hello tároló a Linux- vagy Windows végrehajtási környezet is fontos, például).


## <a name="what-are-containers-good-for"></a>Mire jók a tárolók?
Számos dolgot kiváló fontosságúak, de azok ösztönözzék&mdash;, mint [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/) és [Azure Service Fabric](../articles/service-fabric/service-fabric-overview.md)&mdash;hello single-szolgáltatás, mikroszolgáltatási célú létrehozása elosztott alkalmazások, melyik alkalmazás a Tervező további kicsi, összeállítható részek és nem nagyobb, erősen összekapcsolt összetevők a alapul.

Ez különösen az Azure-hoz hasonló, nyilvános felhőkörnyezetekben igaz, ahol szükség szerinti időpontban és helyen kölcsönözhet virtuális gépeket. Így nem csupán elkülönítési lehetőséghez, illetve gyors üzembe helyezési és vezénylési eszközökhöz juthat, de hatékonyabb döntéseket is hozhat az alkalmazás-infrastruktúrával kapcsolatban.

Az aktuális környezet egy magas rendelkezésre állású elosztott alkalmazás esetében például állhat 9 nagyméretű Azure virtuális gépből. Ha az alkalmazás összetevői hello tárolók üzembe helyezhető, előfordulhat, hogy csak 4 virtuális gépek képes toouse lenniük, és a redundancia és a terheléselosztási 20 tárolókba alkalmazás-összetevők telepítéséhez.

Ez csak egy példa, természetesen, de ehhez a forgatókönyvben, ha több Azure virtuális gépek helyett több tároló toousage teljesítményt módosíthatja, és sokkal hatékonyabban fennmaradó általános CPU-terhelés hello használata.

Emellett nincs több forgatókönyv áll rendelkezésre, amelyek nem alkalmasak tooa mikroszolgáltatások megközelítés; tudni fogja legjobb e mikroszolgáltatások létrehozására és a tárolók segítséget.

### <a name="container-benefits-for-developers"></a>A tárolók használatából származó előnyök fejlesztők számára
Ez általában könnyen toosee, hogy tároló technológia egy lépést, de van pontosabb előnyei is. A következőkben a Docker-tároló hello példa. Ez a témakör fog nem alaposabban mélyen Docker most (olvasható [Docker újdonságai?](https://www.docker.com/whatisdocker/) adott szövegegység vagy [wikipedia](http://wikipedia.org/wiki/Docker_%28software%29)), de Docker és az ökoszisztémákhoz rengeteg előnyöket kínálják tooboth fejlesztők és informatikai szakemberek számára.

A fejlesztők gyorsan, érvénybe tooDocker tárolók, ez biztosítja ugyanis mindenekelőtt a Linux és Windows-tárolókon alkalmazását:

* Egyszerű, növekményes parancsok toocreate a rögzített lemezkép, amely könnyen toodeploy használhatja, és a automatizálhat azokat egy dockerfile használatával lemezképek összeállításakor
* Könnyen használata egyszerű, képek is megoszthatják [git](https://git-scm.com/)-style leküldéses és lekéréses túl parancsok[nyilvános](https://registry.hub.docker.com/) vagy [titkos docker nyilvántartó](../articles/virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Számítógépek helyett elkülönített alkalmazás-összetevőkben gondolkodhatnak.
* Számos olyan eszközt használhatnak, amelyek támogatják a Docker-tárolókat és az alapul szolgáló különféle rendszerképeket.

### <a name="container-benefits-for-operations-and-it-professionals"></a>A tárolók használatából származó előnyök üzemeltetők és informatikai szakemberek számára
IT-műveletek szakemberek is igénybe tárolók és a virtuális gépek hello kombinációja.

* A tartalmazott szolgáltatások elkülönülnek a virtuálisgép-gazda végrehajtási környezetétől.
* A tartalmazott kód azonossága igazolható.
* A tartalmazott szolgáltatások elindíthatók, leállíthatók, valamint gyorsan áthelyezhetők az egyes fejlesztési, tesztelési és éles környezetek között.

Szolgáltatások, például ezek&mdash;, így nincsenek a további&mdash;meghatározott vállalatok számára, ahol szakembereknek szóló információkat technológia szervezet rendelkezik-e a méretezés erőforrások hello feladat hogy&mdash;többek között a tiszta feldolgozási teljesítmény&mdash; toohello feladatok szükséges toonot csak belül üzleti, de ügyfelek elégedettségének növelése, és a felhasználók elérését. Kisméretű vállalkozások számára, ISV-k és indítások is pontosan hello azonos követelmény, de nem ír le, eltérően.

## <a name="what-are-virtual-machines-good-for"></a>Mire jók a virtuális gépek?
Virtuális gépek, adja meg a felhőalapú informatika meghatározására hello gerincét, és, amely nem változik. Ha virtuális gépek lassabban, egy nagyobb méretű lemez erőforrásigényét rendelkezik, és nem képezi le közvetlenül tooa mikroszolgáltatások architektúra, rendelkeznek nagyon fontos előnyei:

1. Alapesetben sokkal nagyobb teljesítményű alapértelmezett védelmet biztosítanak a gazdagépek számára.
2. Az összes fő operációsrendszer- és alkalmazás-konfigurációt támogatják.
3. Bevált eszköz-ökoszisztémákkal rendelkeznek a parancsokhoz és a vezérléshez.
4. Ezek biztosítanak hello végrehajtási környezet toohost tárolók

hello utolsó elemének fontos, mert a benne lévő alkalmazások továbbra is egy adott operációs rendszerrel és a CPU-típus van szükség, attól függően, hello hívások hello alkalmazás biztosítják. Fontos, hogy telepítsen tárolók a virtuális gépek bennük hello alkalmazásokat toodeploy; mert tooremember tárolók nincsenek virtuális gépek vagy operációs rendszerek cserékhez.

## <a name="high-level-feature-comparison-of-vms-and-containers"></a>A virtuális gépek és a tárolók magas szintű szolgáltatásainak összehasonlítása
hello alábbi táblázat részletesen ismerteti a nagyon nagy szolgáltatás szintű hello típusú különbségek attól függnek, amelyek&mdash;nélkül extra alkalmazásfejlesztőre&mdash;tárolók közötti virtuális gépek és a Linux létezik. Ne feledje, hogy egyes szolgáltatások az alkalmazását, lehet, hogy több vagy kevesebb kívánatos függően kell, által nyújtott szolgáltatások támogatásáról, különösen a biztonsági hello területén, hogy az összes szoftver, a további munkahelyi biztosít nőtt.

| Szolgáltatás | Virtuális gépek | Tárolók |
|:--- | --- | --- |
| „Alapértelmezett” biztonsági támogatás |tooa nagyobb mértékben |tooa valamivel kisebb mértékű |
| Szükséges lemezmemória mennyisége |Teljes OS plusz az alkalmazások |Csak alkalmazáskövetelmények |
| Idő toostart |Jelenősen hosszabb: az operációs rendszer indítása és az alkalmazások betöltése |Jelentősen rövidebb: csak alkalmazások toostart kell, mert már fut a kernel |
| Hordozhatóság |A hordozhatóság megfelelő előkészítés mellett biztosított |A hordozhatóság az adott képformátumon belül biztosított; jellemzően kisebb mértékű |
| Rendszerkép-automatizálás |Az operációs rendszertől és az alkalmazásoktól függően nagyban változó |[Docker-beállításjegyzék](https://registry.hub.docker.com/); egyebek |

## <a name="creating-and-managing-groups-of-vms-and-containers"></a>Virtuálisgép- és tárolócsoportok létrehozása és kezelése
Ezen a ponton a tervezők, a fejlesztők vagy az informatikai üzemeltetési szakemberek azt gondolhatják: „Ezt az EGÉSZET automatizálhatom; ez TÉNYLEG egy szolgáltatásként biztosított adatközpont!”.

Még jobb, lehet, és nincsenek a rendszerek, nagy része, amely már használatban, vagy Azure virtuális gépek kezelése és parancsfájlok, gyakran használata hello egyéni kódot szúrjon tetszőleges számú [CustomScriptingExtension for Windows](https://msdn.microsoft.com/library/azure/dn781373.aspx) vagy Hello [Linux CustomScriptingExtension](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/). A&mdash;PowerShell-lel vagy az Azure CLI-szkriptekkel&mdash;elvégezheti, és talán már el is végezte az Azure-környezetek automatizálását.

Ezeket a funkciókat gyakran akkor áttelepített tootools, például [Puppet](https://puppetlabs.com/) és [Chef](https://www.chef.io/) tooautomate hello létrehozását és a skála virtuális gépek konfigurációját. (Az alábbiakban néhány hivatkozások túl[ezek az eszközök használatával az Azure-ral](#tools-for-working-with-containers).)

### <a name="azure-resource-group-templates"></a>Azure erőforráscsoport-sablonok
Frissebb, amely az Azure a hello [az Azure erőforrás-kezelés](../articles/resource-manager-deployment-model.md) REST API-t, és a frissített PowerShell és az Azure parancssori felület eszközök toouse azt könnyen. Telepíthet, módosítása, vagy telepítse újra a teljes alkalmazás topológiák használatával [Azure Resource Manager-sablonok](../articles/resource-group-authoring-templates.md) hello Azure-erőforrás felügyeleti API használatával:

* Hello [sablonok használata az Azure portál](https://github.com/Azure/azure-quickstart-templates)&mdash;mutatót, hello "DeployToAzure" gomb
* Hello [Azure parancssori felület](../articles/virtual-machines/linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="deployment-and-management-of-entire-groups-of-azure-vms-and-containers"></a>Azure virtuális gépek és tárolók teljes csoportjainak üzembe helyezése és kezelése
Számos népszerű rendszeren helyezhet üzembe teljes virtuálisgép-csoportokat és telepítheti a Dockert (vagy egyéb, Linux-tárolókat üzemeltető rendszert) automatizálható csoportként. Közvetlen hivatkozások: hello [tárolók és eszközök](#containers-and-vm-technologies) szakaszában, az alábbiakban. Több rendszert, amelyek a tooa kisebb vagy nagyobb mértékben, és a lista nem teljes. A lista egyes elemeinek hasznossága a felhasználó készségeitől és az adott alkalmazási helyzettől függ.

A Docker saját virtuálisgép-létrehozási ([docker-machine](../articles/virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)), valamint a Docker és a tároló közötti terheléselosztást vezérlő fürtkezelő eszközökkel ([Swarm](../articles/virtual-machines/virtual-machines-linux-docker-swarm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) is rendelkezik. Ezenkívül hello [Azure Docker Virtuálisgép-bővítmény](https://github.com/Azure/azure-docker-extension/blob/master/README.md) alapértelmezett támogatja a [ `docker-compose` ](https://docs.docker.com/compose/), több tároló között alkalmazás tárolók telepítheti, amelyen konfigurálni.

Mindezek mellett kipróbálhatja a [Mesosphere Data Center Operating System (DCOS)](http://docs.mesosphere.com) rendszerét is. Hello nyílt forráskódú alapuló Vezénylőtípusú [mesos](http://mesos.apache.org/) "elosztott rendszerek kernel", amely lehetővé teszi az Ön tootreat egy megcímezhető szolgáltatásként az adatközpontban. A DCOS több fontos rendszerhez (többek között a [Sparkhoz](http://spark.apache.org/) és a [Kafkához](http://kafka.apache.org/)) való beépített csomagokkal, valamint beépített szolgáltatásokkal (többek között a [Marathon](https://mesosphere.github.io/marathon/) tárolóvezérlő rendszerrel és a [Chronos](https://mesos.github.io/chronos/) elosztott ütemezővel) rendelkezik. A Mesost a Twitter, az AirBnb és egyéb webes vállalkozások tapasztalatai alapján fejlesztették ki. Is **swarm** hello vezénylő alrendszer szerint.

A [Kubernetes](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/) szintén egy nyílt forráskódú virtuálisgép- és tárolócsoport-kezelési rendszer, amely a Google tapasztalatai alapján lett összeállítva. Akkor is használhatja [rendelkező kubernetes Szövetes tooprovide hálózatkezelési támogatásának](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave).

[Deis](http://deis.io/overview/) a megnyitott forrás "Platform,--szolgáltatás" (PaaS), így könnyen toodeploy, és kezelheti az alkalmazásokat a saját kiszolgálójára. Deis buildek esetén Docker és CoreOS tooprovide egy egyszerűsített PaaS Heroku ösztönző munkafolyamattal.

A [CoreOS](https://coreos.com/os/docs/latest/booting-on-azure.html), egy optimalizált tárhelyszükségletű, Docker-támogatással és saját, [rkt](https://github.com/coreos/rkt) nevű tárolórendszerrel ellátott Linux-disztribúció is rendelkezik egy [Fleet](https://coreos.com/fleet/docs/latest/) nevű tárolócsoport-kezelési eszközzel.

Az Ubuntu (egy másik nagyon népszerű Linux-disztribúció) széles körű támogatást biztosít a Dockernek, ugyanakkor a [Linux- (LXC stílusú) fürtöket](https://help.ubuntu.com/lts/serverguide/lxc.html) is támogatja.

## <a name="tools-for-working-with-azure-vms-and-containers"></a>Az Azure virtuális gépek és tárolók használatához szükséges eszközök
A tárolók és az Azure virtuális gépek használatához különböző eszközökre van szükség. Ez a szakasz csak néhány hello hasznos vagy a fontos fogalmakat és a tárolók, csoportok, és hello nagyobb konfigurációs és a velük használt vezénylési eszközök listáját tartalmazza.

> [!NOTE]
> Ez a terület amazingly gyorsan változik, és közben a legjobb tookeep műveleteket végezzük el, ez a témakör és a hivatkozások toodate fel, és előfordulhat, hogy egy lehetetlen feladat. Ellenőrizze, hogy a fontos témákról tookeep toodate másolatot kereshet!
>
>

### <a name="containers-and-vm-technologies"></a>Tárolókkal és virtuális gépekkel kapcsolatos technológiák
Példák Linux-tárolókhoz kapcsolódó technológiákra:

* [Docker](https://www.docker.com)
* [LXC](https://linuxcontainers.org/)
* [CoreOS és rkt](https://github.com/coreos/rkt)
* [Open Container projekt](http://opencontainers.org/)
* [RancherOS](http://rancher.com/rancher-os/)

Hivatkozások Windows-tárolókhoz:

* [Windows-tárolók](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview)

Hivatkozások a Visual Studio Dockerhez:

* [Dockerhez készült Visual Studio-eszközök](https://docs.microsoft.com/en-us/dotnet/core/docker/visual-studio-tools-for-docker)

Docker-eszközök:

* [Docker-démon](https://docs.docker.com/installation/#installation)
* Docker-ügyfelek
  * [Windows Docker-ügyfél a Chocolatey-n](https://chocolatey.org/packages/docker)
  * [Docker – Telepítési utasítások](https://docs.docker.com/installation/#installation)

A Docker a Microsoft Azure-on:

* [Docker VM-bővítmény Linuxhoz az Azure-on](../articles/virtual-machines/linux/dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure Docker VM-bővítmény – Használati útmutató](https://github.com/Azure/azure-docker-extension/blob/master/README.md)
* [Hello Docker Virtuálisgép-bővítmény a hello Azure parancssori felület (CLI) használatával](../articles/virtual-machines/linux/classic/cli-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Hello Docker Virtuálisgép-bővítmény a hello Azure-portál használatával](../articles/virtual-machines/linux/classic/portal-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Hogyan toouse docker-gép az Azure-on](../articles/virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Hogyan toouse docker swarm Azure rendelkező](../articles/virtual-machines/virtual-machines-linux-docker-swarm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Bevezetés a Docker és a Docker Compose használatába az Azure-on](../articles/virtual-machines/linux/docker-compose-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Egy Azure-erőforrás csoport sablon toocreate egy Docker-állomás gyorsan Azure használatával](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu)
* [beépített támogatása hello `compose` ](https://github.com/Azure/azure-docker-extension#11-public-configuration-keys) az abban található alkalmazásokhoz
* [Magán Docker-beállításjegyzék létrehozása az Azure-on](../articles/virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Linux-disztribúciók és példák az Azure-on:

* [CoreOS](https://coreos.com/os/docs/latest/booting-on-azure.html)

Konfigurálás, fürtfelügyelet és tárolóvezénylés:

* [CoreOS-flotta](https://coreos.com/fleet/docs/latest/)
* Deis

  * [Az útmutató tooautomated Kubernetes fürttelepítés CoreOS és nagy befejezése](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
  * [Kubernetes megjelenítő](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)
* [Mesos](http://mesos.apache.org/)

  * [Mesosphere Data Center Operating System (DCOS)](https://docs.mesosphere.com/1.7/overview/design/azure-container-service/)
* [Jenkins](https://jenkins.io/) és [Hudson](http://hudson-ci.org/)

  * [Jenkins virtuálisgép-ügynök beépülő modulja Azure-hoz](https://wiki.jenkins.io/display/JENKINS/Azure+VM+Agents+plugin)
  * [GitHub-tár: Jenkins tároló beépülő modul Azure-hoz](https://github.com/jenkinsci/windows-azure-storage-plugin)
  * [Külső gyártó: Hudson alárendelt beépülő modul Azure-hoz](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
  * [Külső gyártó: Hudson tároló beépülő modul Azure-hoz](https://github.com/hudson3-plugins/windows-azure-storage-plugin)
* [Azure Automation](https://azure.microsoft.com/services/automation/)

  * [Videó: Hogyan tooUse Azure automatizálása a Linux virtuális gépek](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)
* PowerShell DSC Linux-hoz

  * [Blog: Hogyan toodo Powershell DSC Linux](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
  * [GitHub: DSC Docker-ügyfelekhez](https://github.com/anweiss/DockerClientDSC)

## <a name="next-steps"></a>Következő lépések
Tekintse meg a [Docker](https://www.docker.com) webhelyét és a [Windows-tárolókkal](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview) foglalkozó témakört.

<!--Anchors-->
[microservices]: http://martinfowler.com/articles/microservices.html
[microservice]: http://martinfowler.com/articles/microservices.html
<!--Image references-->
