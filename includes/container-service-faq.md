# <a name="container-service-frequently-asked-questions"></a>A Container Service-re vonatkozó gyakori kérdések

## <a name="orchestrators"></a>Vezénylők

### <a name="which-container-orchestrators-do-you-support-on-azure-container-service"></a>Mely tárolóvezénylőket támogatja az Azure Container Service? 

A következők élveznek támogatást: nyílt forráskódú DC/OS, Docker Swarm és Kubernetes. További információkért lásd: hello [áttekintése](../articles/container-service/kubernetes/container-service-intro-kubernetes.md).
 
### <a name="do-you-support-docker-swarm-mode"></a>Támogatja a Docker Swarm módot? 

Swarm mód jelenleg nem támogatott, de a hello szolgáltatás terv. 

### <a name="does-azure-container-service-support-windows-containers"></a>Az Azure Container Service támogatja a Windows-tárolókat?  

Jelenleg a Linux-tárolók az összes vezénylővel támogatást élveznek. A Windows-tárolók Kubernetesszel való használatának támogatása előzetes verzióban érhető el.

### <a name="do-you-recommend-a-specific-orchestrator-in-azure-container-service"></a>Ajánlanak egy adott vezénylőt az Azure Container Service-ben? 
Nem ajánlunk általánosan egy adott vezénylőt. Ha egy támogatott hello orchestrators élmény, az Azure Tárolószolgáltatásban tapasztalatok is alkalmazhat. Az adattrendek azonban azt jelzik, hogy a DC/OS kiválóan használható éles környezetben a Big Data és IoT számítási feladatokhoz, a Kubernetes a felhők natív számítási feladataihoz megfelelő, a Docker Swarm pedig közismerten integrálható Docker-eszközökkel, illetve egyszerűen elsajátítható a használata.

A forgatókönyvtől függően más Azure-szolgáltatásokkal is létrehozhat és kezelhet egyéni tárolómegoldásokat. Ilyen szolgáltatás például a [Virtual Machines](../articles/virtual-machines/linux/overview.md), a [Service Fabric](../articles/service-fabric/service-fabric-overview.md), a [Web Apps](../articles/app-service-web/app-service-web-overview.md) és a [Batch](../articles/batch/batch-technical-overview.md).  

### <a name="what-is-hello-difference-between-azure-container-service-and-acs-engine"></a>Mi az Azure Tárolószolgáltatás és az ACS-motor hello különbségének? 
Az Azure Tárolószolgáltatás egy olyan SLA biztonsági Azure szolgáltatás hello Azure-portálon, az Azure parancssori eszközök és az Azure API-k funkcióit. hello szolgáltatás lehetővé teszi, hogy a tooquickly megvalósítása, és kezelheti a fürtöket szabványos tároló vezénylési eszközök futtatása egy viszonylag kis mennyiségű konfigurációs beállításokkal. 

[Az ACS-motor](http://github.com/Azure/acs-engine) egy nyílt forráskódú projekt, amely lehetővé teszi a kiemelt felhasználók toocustomize hello fürtkonfiguráció minden szinten van. Infrastruktúra és a szoftver ezen képességét tooalter hello beállítása azt jelenti, hogy nincs SLA kínálunk az ACS-motor. Támogatási hello nyílt forráskódú projekt a Githubon keresztül, nem pedig hivatalos Microsoft csatornákon keresztül kell kezelni. 

## <a name="cluster-management"></a>Fürtkezelés

### <a name="how-do-i-create-ssh-keys-for-my-cluster"></a>Hogyan hozhatok létre SSH-kulcsokat a fürtöm számára?

A fürt hello Linux virtuális gépek elleni hitelesítéshez használandó az operációs rendszer toocreate egy SSH-RSA nyilvános és titkos kulcsból álló kulcspárt szabványos eszközöket is. Útmutató: hello [OS X- és Linux](../articles/virtual-machines/linux/mac-create-ssh-keys.md) vagy [Windows](../articles/virtual-machines/linux/ssh-from-windows.md) útmutatást. 

Ha [Azure CLI 2.0 parancsok](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md) toodeploy a tároló szolgáltatás fürt, SSH kulcsok automatikusan létre lehet hozni a fürt számára.

### <a name="how-do-i-create-a-service-principal-for-my-kubernetes-cluster"></a>Hogyan hozhatok létre egyszerű szolgáltatást a Kubernetes-fürtöm számára?

Egy Azure Active Directory szolgáltatás egyszerű azonosító és jelszó egyaránt szükséges toocreate az Azure Tárolószolgáltatásban Kubernetes fürt. További információkért lásd: [kapcsolatos hello szolgáltatás egyszerű Kubernetes fürt](../articles/container-service/kubernetes/container-service-kubernetes-service-principal.md).

Ha [Azure CLI 2.0 parancsok](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md) toodeploy egy Kubernetes fürtszolgáltatás, egyszerű hitelesítő adatok automatikusan létre lehet hozni a fürt számára.

### <a name="how-large-a-cluster-can-i-create"></a>Legfeljebb mekkora fürtöket hozhatok létre?
1, 3 vagy 5 fő csomóponttal rendelkező fürtöket hozhat létre. Választhat, too100 ügynök csomópontja fel.

> [!IMPORTANT]
> A nagyobb fürtök és hello Virtuálisgép-méretet a csomópontok beállításoktól függően szükség lehet tooincrease hello magok kvóta az előfizetésben. a kvóta növelését, nyissa meg toorequest egy [online felhasználói támogatási kérelem](../articles/azure-supportability/how-to-create-azure-support-request.md) díjmentesen. Amennyiben [ingyenes Azure-fiókot](https://azure.microsoft.com/free/) használ, csak korlátozott számú számítási magot használhat az Azure-ban.
> 

### <a name="how-do-i-increase-hello-number-of-masters-after-a-cluster-is-created"></a>Hogyan növelje meg hello főkiszolgálók száma, a fürt létrehozása után? 
Hello fürt létrehozása után a főkiszolgálók száma hello rögzített, és nem módosítható. Hello fürt hello létrehozásakor ki kell választania a magas rendelkezésre állású több főkiszolgálók ideális.

### <a name="how-do-i-increase-hello-number-of-agents-after-a-cluster-is-created"></a>Hogyan növelje meg hello ügynökök számát, a fürt létrehozása után? 
Ügynökök száma hello hello fürt hello Azure-portálon vagy a parancssori eszközök segítségével méretezhető. Lásd: [Azure Container Service-fürt méretezése](../articles/container-service/kubernetes/container-service-scale.md).

### <a name="what-are-hello-urls-of-my-masters-and-agents"></a>Mik azok a hello URL-címei a főkiszolgálók és az ügynökök? 
erőforrások az Azure Tárolószolgáltatásban hello alapuló fürt URL-címei hello DNS-beli név előtagot adja meg, majd hello hello Azure-régió, központi telepítés számára is választott nevét. Például hello teljesen minősített tartománynevét (FQDN) hello fő csomópont van a következő formátumban:

``` 
DNSnamePrefix.AzureRegion.cloudapp.azure.net
```

Gyakran használt URL-címek található a fürt hello Azure-portálon, hello Azure erőforrás-kezelő vagy más Azure-eszközök számára.

### <a name="how-do-i-tell-which-orchestrator-version-is-running-in-my-cluster"></a>Hogyan állapíthatom meg, hogy melyik vezénylőverzió fut a fürtben?

* A DC/OS: Lásd: hello [Mesosphere dokumentációjában](https://support.mesosphere.com/hc/en-us/articles/207719793-How-to-get-the-DCOS-version-from-the-command-line-)
* Docker Swarm: Futtassa a `docker version` parancsot
* Kubernetes: Futtassa a `kubectl version` parancsot

### <a name="how-do-i-upgrade-hello-orchestrator-after-deployment"></a>Hogyan lehet frissíteni a hello orchestrator telepítés után?

Azure Tárolószolgáltatás jelenleg nem biztosít eszközök tooupgrade hello hello orchestrator a fürtön lévő telepített verzióját. Ha a Container Service egy későbbi verziót támogat, üzembe helyezhet egy új fürtöt. Lehetősége a toouse orchestrator-specifikus eszköz, ha elérhető tooupgrade egy fürt helyben. Lásd például a [DC/OS verziófrissítését](https://dcos.io/docs/1.8/administration/upgrading/) ismertető cikket.
 
### <a name="where-do-i-find-hello-ssh-connection-string-toomy-cluster"></a>Hol található hello SSH kapcsolati karakterlánc toomy fürt?

Hello kapcsolati karakterláncban található hello Azure-portálon, vagy az Azure parancssori eszközök segítségével. 

1. Hello portálon lépjen hello fürttelepítés toohello erőforráscsoportot.  

2. Kattintson a **áttekintése** és hello hivatkozásra kattintva **központi telepítések** alatt **Essentials**. 

3. A hello **üzembe helyezési előzményeket** panelen hello központi telepítés, amelynek a neve, kattintson **microsoft-acs** egy központi telepítés dátumától követ. Például: microsoft-acs-201701310000.  

4. A hello **összegzés** lap **kimenetek**, több fürt hivatkozásokkal érhető el. **SSHMaster0** egy SSH kapcsolati karakterlánc toohello első főkiszolgálójának a tárolószolgáltatási fürt biztosít. 

Mint korábban feljegyzett Azure eszközök toofind hello hello fő teljes Tartománynevét is használhatja. Győződjön meg az SSH-kapcsolat toohello fő használatával hello hello fő és hello felhasználónév hello fürt létrehozásakor megadott teljes Tartományneve. Példa:

```bash
ssh userName@masterFQDN –A –p 22 
```

További információkért lásd: [Connect tooan Azure Tárolószolgáltatás-fürt](../articles/container-service/kubernetes/container-service-connect.md).

## <a name="next-steps"></a>Következő lépések

* [További információ](../articles/container-service/kubernetes/container-service-intro-kubernetes.md) az Azure Container Service-ről.
* A tárolószolgáltatási fürt hello segítségével telepítheti [portal](../articles/container-service/dcos-swarm/container-service-deployment.md) vagy [Azure CLI 2.0](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md).
