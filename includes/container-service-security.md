# <a name="securing-docker-containers-in-azure-container-service"></a>Docker-tárolók védelme az Azure Container Service-ben

Ez a cikk az Azure Container Service-ben üzembe helyezett Docker-tárolók védelmével kapcsolatos szempontokat és javaslatokat tartalmazza. Ezeket a szempontokat számos alkalmazása általában tooDocker tárolók üzembe helyezett Azure-ban vagy más környezetekben. 

## <a name="image-security"></a>Rendszerképek biztonsága

A tárolók egy vagy több adattárban található rendszerképekből állnak össze. A tárolóhelyekkel toopublic vagy személyes tárolót nyilvántartó is tartozhatnak. Ilyen nyilvános beállításjegyzék például a [Docker Hub](https://hub.docker.com/). Példa egy titkos beállításjegyzék: hello [Docker megbízható beállításjegyzék](https://docs.docker.com/datacenter/dtr/2.0/), amely lehet telepítve a helyi vagy a magánfelhő-alapú. Léteznek továbbá felhőalapú privát tárolójegyzék-szolgáltatások is, mint az [Azure Container Registry](../articles/container-registry/container-registry-intro.md).

### <a name="public-and-private-images"></a>Nyilvános és privát rendszerképek
Általánosságban, ahogyan más nyilvánosan közzétett szoftvercsomagok, a nyilvánosan elérhető tárolórendszerképek sem garantálják a biztonságot. A tárolórendszerképek több szoftverrétegből épülnek fel, és mindegyik rétegben lehetnek biztonsági rések. Kulcs toounderstand hello származási hello tároló kép, hello tulajdonos hello kép (Ha egy megbízható forrásból vagy nem toodetermine), beleértve a hello áll szoftverréteget, és hello szoftververziók. 

Például, ha hivatalos toohello [nginx-tárház](https://hub.docker.com/_/nginx/) Docker központban, és keresse meg a toohello **címkék** lapon megjelenik itt a színkódolás biztonsági rések minden lemezképben. Minden egyes szín hello biztonsági rés hello lemezkép szoftver réteg ábrázol. További információt a biztonsági rések a Docker Hubon való vizsgálatával kapcsolatban a [Docker Hub hivatalos adattárait ismertető](https://blog.docker.com/2015/06/understanding-official-repos-docker-hub/) cikkben talál.

![Nginx-rendszerképek a Docker Hubon](./media/container-service-security/docker-hub-nginx.png)

A vállalatok fontos mélyen kapcsolatos biztonsági és tooprotect magukat biztonsági támadások elleni kell tárolásához és lekéréséhez képek titkos beállításjegyzékből, például Azure tároló beállításjegyzék- vagy Docker megbízható beállításjegyzék. Továbbá tooproviding egy felügyelt titkos beállításjegyzék, az Azure-tároló beállításjegyzék támogatja [szolgáltatás-alapú hitelesítési](../articles/container-registry/container-registry-authentication.md) Azure Active Directory, az egyszerű hitelesítés adatfolyamok, például a szerepköralapú hozzáférés olvasási, írási és tulajdonos.

### <a name="image-security-scanning"></a>Rendszerképek biztonsági ellenőrzése

Akkor is, ha egy titkos beállításjegyzékkel, egy további biztonsági ellenőrzés céljából megoldások keresése jó ötlet toouse lemezképet is. Minden szoftver réteg tároló lemezkép független hello tároló kép más rétegeiből a potenciálisan nagyon eséllyel fordulnak elő toovulnerabilities. A vállalatok egyre kezdési, üzembe helyezése a termelési számítási feladatokhoz tároló technológiák alapján, a biztonsági fenyegetések ellen szabhatnak fontos tooensure megelőzési kép vizsgálata válik. 

Biztonsági figyelését és vizsgálatát, megoldások, például a [Twistlock](https://www.twistlock.com/2016/11/07/twistlock-supports-azure-container-registry) és [Tengerkék biztonsági](http://blog.aquasec.com/image-vulnerability-scanning-in-azure-container-registry), többek között is használt tooscan titkos beállításjegyzék tároló képek és azonosítani a potenciális biztonsági réseket. Fontos, hogy hello különböző megoldások keresése toounderstand hello mélysége adja meg. Egyes megoldások például csak az ismert biztonsági résekkel szemben ellenőrzik a rendszerképek rétegeit. Ezek a megoldások nem feltétlenül tudja tooverify lemezkép-réteg szoftver beépített bizonyos package manager szoftver segítségével. Más megoldások azonban részletesebb ellenőrzésre képesek, és bármely csomagolt szoftverben képesek megtalálni a biztonsági réseket.

### <a name="production-deployment-rules-and-audit"></a>Éles rendszerek üzembe helyezésének szabályai és ellenőrzése
Ha egy alkalmazás központi telepítése éles környezetben, is alapvető tooset néhány szabályok tooensure képek éles környezetben használt biztonságos, és nem a biztonsági rések tartalmaznak.

* Szabály biztonsági rések, még akkor is kisebb képekkel nem szabadna toorun éles környezetben. Ezenkívül éles környezetben telepített összes lemezkép ideális mentésére titkos beállításjegyzék elérhető tooa SELECT kevés. Azt is fontos tookeep hello száma éles képek kis tooensure, hogy ezek kezelhetők hatékonyan érték.

* Mivel a merevlemez toopinpoint hello származási szoftver nyilvánosan elérhető tároló lemezképről, egy forrás tooensure Tudásbázis hello réteg hello származási célszerű toobuild képek is. A biztonsági rés felületek önálló beépített tároló lemezkép, az ügyfelek található gyorsabb elérési tooa felbontást eredményez. Nyilvános képének, az ügyfelek lenne szükség egy nyilvános kép toofix toofind hello gyökérkönyvtárában, vagy szerezzen be egy másik biztonságos kép hello közzétételi.

* Éles környezetben telepített alaposan beolvasott kép nem garantált toobe toodate fel a hello hello élettartama. Biztonsági rések rétegek hello lemezkép nem volt korábban ismert vagy bevezetett hello éles környezet után előfordulhat, hogy jelenteni. Éles környezetben telepített lemezképek rendszeres naplózását elavultak, vagy nem lett frissítve egy kis idő szükséges tooidentify lemezképeket. Egy használhatja a kék-zöld telepítési módszerek és a működés közbeni frissítési mechanizmusok tooupdate tároló képek állásidő nélkül. Kép vizsgálata az hello előző szakaszban leírt eszközök segítségével végezhető. 

* Folyamatos integrációt (CI) folyamat toobuild lemezképeket és az integrált security Scanning eszköz segítségével biztonságos TITKOS nyilvántartó, biztonságos tároló-lemezképek kezelése. hello biztonsági rések keresése épített hello CI megoldás biztosítja, hogy összes hello tesztjét lemezképeket vannak leküldött toohello titkos beállításjegyzék mely gyártási munkaterhelések vannak telepítve. CI feldolgozási sori hiba biztosítja, hogy a sebezhető lemezképek nem leküldött termelési alkalmazások és szolgáltatások központi telepítése céljából hello titkos beállításjegyzékbe. Emellett a folyamattal automatizálható a rendszerképek biztonsági ellenőrzése, ha sok rendszerkép van használatban. Máskülönben a rendszerképek manuális vizsgálata rendkívül hosszadalmas és sok hibalehetőséget tartalmazó folyamat lenne.

## <a name="host-level-container-isolation"></a>Tárolók gazdagépszintű elkülönítése
Ha a felhasználók Azure-erőforrásokon helyeznek üzembe tárolóalkalmazásokat, azok az előfizetés szintjén lesznek üzembe helyezve erőforráscsoportokban, és nem lesznek több-bérlősek. Ez azt jelenti, hogy az ügyfél egy előfizetés másokkal közösen használja, ha nincsenek határok, amely építhetők között két központi telepítések hello azonos előfizetéssel. Emiatt a tárolószintű biztonság nem biztosítható. 

Akkor is, hogy a tárolók közös hello kernel és hello erőforrások hello állomás (Ez az Azure Tárolószolgáltatásban egy Azure virtuális gép fürttagként) kritikus toounderstand. Az éles környezetekben futó tárolókat ezért nem kiemelt jogosultságú felhasználói módban kell futtatni. Egy tároló legfelső szintű jogosultságokkal fut kedvezőtlenül befolyásolhatja a hello teljes környezetet. Gyökérszintű hozzáféréssel a tárolóban lévő egy támadó hozzáférést toohello teljes root jogosultságot hello gazdagépen. Ezenkívül fontos toorun tárolók csak olvasható fájlrendszerek. Ez megakadályozza, hogy hozzáférési sérült toohello tároló toowrite rosszindulatú parancsfájlok toohello fájlt a rendszer, és hozzáférést tooother fájlok rendelkező személy. Hasonlóan fontos toolimit hello erőforrások (például memória, a CPU és a hálózati sávszélesség) tooa tároló lefoglalt. Ez megakadályozza, hogy támadók lefoglalják az erőforrásokat, és szabálytalan tevékenységeket, például a hitelkártya csalás vagy bitcoin adatbányászati, amely megakadályozhatja, hogy más tárolók hello gazdagépen vagy fürtön futó követni.

## <a name="orchestrator-considerations"></a>A vezénylőkkel kapcsolatos szempontok

Az Azure Container Service-ben elérhető egyes vezénylőkre más-más biztonsági szempontok vonatkoznak. Például korlátozza Tárolószolgáltatás közvetlen SSH hozzáférés tooorchestrator csomópontján. Ehelyett kell használnia minden egyes orchestrator felhasználói felület és parancssori eszközök (például `kubectl` a Kubernetes) toomanage hello tároló környezet hello állomások elérése nélkül. További információkért lásd: [ellenőrizze a távoli kapcsolat tooa Kubernetes, DC/OS- vagy Docker Swarm-fürt](../articles/container-service/kubernetes/container-service-connect.md).

Az orchestrator-specifikus további biztonsági információ: a következő erőforrások hello:

* **Kubernetes**: [Ajánlott biztonsági eljárások a Kubernetes-telepítéshez](http://blog.kubernetes.io/2016/08/security-best-practices-kubernetes-deployment.html)

* **DC/OS**: [A fürt védelmének biztosítása](https://dcos.io/docs/1.8/administration/securing-your-cluster/)

* **Docker Swarm**: [Docker-biztonság](https://www.docker.com/docker-security)

## <a name="next-steps"></a>Következő lépések

* Docker-architektúra és a tároló biztonsági kapcsolatban bővebben lásd: [bemutatása tooContainer biztonsági](https://www.docker.com/sites/default/files/WP_IntrotoContainerSecurity_08.19.2016.pdf).

* Az Azure platform biztonsággal kapcsolatos információkért lásd: hello [az Azure Security Center](https://www.microsoft.com/en-us/trustcenter/cloudservices/azure).
