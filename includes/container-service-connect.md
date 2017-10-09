# <a name="make-a-remote-connection-tooa-kubernetes-dcos-or-docker-swarm-cluster"></a>Ellenőrizze a távoli kapcsolat tooa Kubernetes, DC/OS- vagy Docker Swarm-fürt
Miután létrehozta az Azure Tárolószolgáltatás-fürtöt, tooconnect toohello fürt toodeploy kell, és munkaterhelések kezelése. Ez a cikk ismerteti, hogyan tooconnect toohello fő hello virtuális fürt távoli számítógépről. 

hello Kubernetes, a DC/OS és a Docker Swarm-fürtök adja meg a HTTP-végpontokról helyileg. A Kubernetes, ehhez a végponthoz biztonságosan tesz elérhetővé a hello internet, és hozzá tud férni hello futtatásával `kubectl` parancssori eszközt a bármely internetkapcsolattal rendelkező gép. 

A DC/OS- és Docker Swarm azt javasoljuk, hogy a helyi számítógép toohello fürt felügyeleti rendszerről hozzon létre egy secure shell (SSH) alagút. Hello alagút létrejötte után parancsok, amelyek hello HTTP-végpontokról és nézet hello orchestrator webes felülete (ha elérhető) a helyi rendszerről futtathatja. 

## <a name="prerequisites"></a>Előfeltételek

* Egy, az [Azure Container Service](../articles/container-service/dcos-swarm/container-service-deployment.md)-ben üzembe helyezett Kubernetes-, DC/OS- vagy Docker Swarm-fürt.
* SSH-RSA titkos kulcs fájlja, megfelelő toohello nyilvános kulcs hozzáadott toohello fürt üzembe helyezése során. A parancsok használata, hogy hello titkos SSH-kulcs van `$HOME/.ssh/id_rsa` a számítógépen. További információkat a [macOS és Linux](../articles/virtual-machines/linux/mac-create-ssh-keys.md) rendszerre vagy a [Windows](../articles/virtual-machines/linux/ssh-from-windows.md) rendszerre vonatkozó útmutatókban találhat. Hello SSH-kapcsolat nem működik, ha esetleg túl [az SSH-kulcsok alaphelyzetbe](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md).

## <a name="connect-tooa-kubernetes-cluster"></a>Csatlakoztassa tooa Kubernetes fürtöt

Kövesse az alábbi lépéseket tooinstall és konfigurálása `kubectl` a számítógépen.

> [!NOTE] 
> A Linux vagy macOS, szükség lehet a toorun hello parancsok be ez a szakasz `sudo`.
> 

### <a name="install-kubectl"></a>A kubectl telepítése
Egyirányú tooinstall az eszköz toouse hello `az acs kubernetes install-cli` Azure CLI 2.0 parancsot. Ez a parancs, győződjön meg arról, hogy toorun meg [telepített](/cli/azure/install-az-cli2) hello Azure CLI legújabb 2.0 és tooan Azure-fiókjával bejelentkezve (`az login`).

```azurecli
# Linux or macOS
az acs kubernetes install-cli [--install-location=/some/directory/kubectl]

# Windows
az acs kubernetes install-cli [--install-location=C:\some\directory\kubectl.exe]
```

Alternatív megoldásként letöltheti a hello legújabb `kubectl` ügyfél közvetlenül a hello [Kubernetes feloldja a lap](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md). További információ: [A kubectl telepítése és beállítása](https://kubernetes.io/docs/tasks/kubectl/install/).

### <a name="download-cluster-credentials"></a>A fürt hitelesítő adatainak letöltése
Ha már `kubectl` telepítve, toocopy hello fürt hitelesítő adatok tooyour gép kell. Egyirányú toodo get hello hitelesítő adatokat kell a hello `az acs kubernetes get-credentials` parancsot. Hozzáférési hello hello erőforráscsoport és hello nevét hello tároló szolgáltatás-erőforrást:

```azurecli
az acs kubernetes get-credentials --resource-group=<cluster-resource-group> --name=<cluster-name>
```

Ez a parancs letölti hello fürt hitelesítő adatok túl`$HOME/.kube/config`, ahol `kubectl` található toobe vár.

Másik megoldásként használhatja `scp` toosecurely hello fájl másolása a `$HOME/.kube/config` hello fő VM tooyour helyi számítógépen. Példa:

```bash
mkdir $HOME/.kube
scp azureuser@<master-dns-name>:.kube/config $HOME/.kube/config
```

Ha a Windows, a Windows, hello PuTTy biztonságos fájl másolása ügyfél vagy egy hasonló eszköz Ubuntu Bash is használhatja.

### <a name="use-kubectl"></a>Kubectl használata

Ha már `kubectl` hello kapcsolat konfigurálva, tesztelje a fürtben található csomópontok hello listázása:

```bash
kubectl get nodes
```

Egyéb `kubectl` parancsokat is kipróbálhat. Például hello Kubernetes irányítópulton megtekintheti. Először futtassa a proxy toohello Kubernetes API-kiszolgálóhoz:

```bash
kubectl proxy
```

hello Kubernetes felhasználói felület érhető el most: `http://localhost:8001/ui`.

További információkért lásd: hello [Kubernetes gyors üzembe helyezési](http://kubernetes.io/docs/user-guide/quick-start/).

## <a name="connect-tooa-dcos-or-swarm-cluster"></a>Csatlakoztassa tooa DC/OS és Swarm fürtöt

toouse hello DC/OS- és Docker Swarm-fürtöket Azure tárolószolgáltatáson telepített kövesse ezeket utasításokat toocreate egy SSH-alagút a helyi Linux, macOS vagy a Windows rendszert. 

> [!NOTE]
> A jelen útmutatások elsősorban a TCP-forgalom SSH-n keresztüli bújtatására vonatkoznak. A hello belső fürt felügyeleti rendszerek használatával is elkezdheti interaktív SSH-munkamenetet, de nem ajánlott ennek. A közvetlenül a belső rendszeren végzett munka növeli a konfiguráció véletlen módosításának kockázatát.  
> 

### <a name="create-an-ssh-tunnel-on-linux-or-macos"></a>SSH-alagút létrehozása Linux vagy macOS rendszeren
Amikor az SSH-alagút létrehozása Linux vagy macOS hello elsőként toolocate hello nyilvános DNS-neve hello terhelésű főkiszolgálók. Kövesse az alábbi lépéseket:


1. A hello [Azure-portálon](https://portal.azure.com), keresse meg a tárolószolgáltatási fürt tartalmazó toohello erőforráscsoport. Bontsa ki a hello erőforráscsoportot, így az egyes erőforrások jelenik meg. 

2. Kattintson a hello **tárolószolgáltatás** erőforrás, majd kattintson **áttekintése**. Hello **fő FQDN** hello a fürt jelenik meg az **Essentials**. Mentse ezt a nevet a későbbi felhasználásra. 

    ![Nyilvános DNS-név](./media/container-service-connect/pubdns.png)

    Alternatív megoldásként futtassa hello `az acs show` a tárolószolgáltatás parancsot. Hello keressen **fő profil: fqdn** hello parancs kimenetében tulajdonság.

3. Most nyisson meg egy kezelőfelületet, és futtassa a hello `ssh` hello a következő értékek megadásával parancsot: 

    **LOCAL_PORT** hello TCP port hello alagút tooconnect való hello szolgáltatás oldalán. Swarm esetén ez too2375 beállítása. DC/os állítsa be a too80. 
    **REMOTE_PORT** hello port, amelyet az tooexpose hello végpont. Swarm esetén használja a 2375-ös portot. DC/OS esetén használja a 80-as portot.  
    **FELHASZNÁLÓNÉV** hello fürt telepítésekor megadott felhasználónév hello.  
    **DNSPREFIX** hello hello fürt telepítésekor megadott DNS-előtag van.  
    **A régióban** hello régió, ahol az erőforráscsoport megtalálható.  
    **PATH_TO_PRIVATE_KEY** [OPCIONÁLIS] hello elérési toohello titkos kulcsot, amely megfelel a toohello hello fürt létrehozásakor megadott nyilvános kulcs. Ez a beállítás használata hello `-i` jelzőt.

    ```bash
    ssh -fNL LOCAL_PORT:localhost:REMOTE_PORT -p 2200 [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com
    ```
  
  > [!NOTE]
  > hello SSH-kapcsolati port 2200, és nem a szokásos 22-es portot hello. Egynél több fő virtuális gép a fürtben Ez a hello kapcsolati port toohello első főkiszolgálójának virtuális gép.
  > 

  hello parancs kimenete nélkül adja vissza.

Példák hello a DC/OS és Swarm a következő részekben hello.    

### <a name="dcos-tunnel"></a>DC/OS-alagút
tooopen alagutat DC/OS végpontok hello hasonló parancsot:

```bash
sudo ssh -fNL 80:localhost:80 -p 2200 azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com 
```

> [!NOTE]
> Győződjön meg arról, hogy nincs másik olyan helyi folyamat, amely lekötné a 80-as portot. Ha szükséges, beállíthat más, a 80-as porttól eltérő helyi portokat is (például a 8080-as portot). Ha azonban ezt a portot használja, előfordulhat, hogy egyes webes felhasználói felületek hivatkozásai nem működnek.
>

Most a helyi rendszerről hello következő URL-címek (feltéve, hogy a helyi 80-as porton) keresztül hello DC/OS végpontok érhető el:

* DC/OS:`http://localhost:80/`
* Marathon:`http://localhost:80/marathon`
* Mesos:`http://localhost:80/mesos`

Ehhez hasonlóan hello rest API-k minden alkalmazáshoz ezen az alagúton keresztül érheti el.

### <a name="swarm-tunnel"></a>Swarm-alagút
egy alagút toohello Swarm végponthoz tooopen hello hasonló parancs futtatása:

```bash
ssh -fNL 2375:localhost:2375 -p 2200 azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com
```
> [!NOTE]
> Győződjön meg arról, hogy nincs másik olyan helyi folyamat, amely lekötné a 2375-ös portot. Például ha hello Docker démon helyileg futtat, van beállítva alapértelmezett toouse port 2375. Ha szükséges, beállíthat más, a 2375-ös porttól eltérő helyi portokat is.
>

Most hello Docker Swarm-fürt a helyi rendszeren hello Docker parancssori felületet (Docker CLI) használatával végezheti el. A telepítési utasításokért lásd [a Docker telepítését](https://docs.docker.com/engine/installation/) ismertető cikket.

Állítsa be a DOCKER_HOST környezeti változó toohello hello alagúthoz helyi portot. 

```bash
export DOCKER_HOST=:2375
```

Futtassa a Docker parancsokat alagút toohello Docker Swarm-fürthöz. Példa:

```bash
docker info
```

### <a name="create-an-ssh-tunnel-on-windows"></a>SSH-alagút létrehozása Windows rendszeren
Windows-rendszeren az SSH-alagutak többféleképpen is létrehozhatók. Ha Bash Ubuntu a Windows vagy egy hasonló eszköz futtat, kövesse hello SSH tunneling megjelenő utasításokat az ebben a cikkben macOS és Linux rendszerekhez. Másik megoldásként a Windows, ez a szakasz ismerteti, hogyan toouse PuTTY toocreate hello alagutat.

1. [Töltse le a PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) tooyour Windows rendszert.

2. Hello alkalmazás futtatásához.

3. Adjon meg egy állomásnevet, amely hello fürt rendszergazda felhasználóneve és hello hello fürt első főkiszolgálójának hello nyilvános DNS-nevét. Hello **állomásnév** túl hasonlít`azureuser@PublicDNSName`. Adja meg a 2200 hello **Port**.

    ![A PuTTY-konfigurálásának 1. lépése](./media/container-service-connect/putty1.png)

4. Válassza az **SSH > Auth** (SSH > Hitelesítés) parancsot. Adja hozzá az elérési út tooyour titkos kulcsfájlt (.ppk formátum) a hitelesítéshez. Használhatja például egy eszközt [PuTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) ez fájlként toogenerate hello SSH-kulcs használt toocreate hello fürt.

    ![A PuTTY-konfigurálásának 2. lépése](./media/container-service-connect/putty2.png)

5. Válassza ki **SSH > alagutak** , és konfigurálja a hello következő továbbított portokat:

    * **Source port** (Forrásport): DC/OS esetén használja a 80-as, Swarm estén a 2375-ös portot.
    * **Destination** (Cél): DC/OS esetén használja a localhost:80, Swarm esetén a localhost:2375 portot.

    a következő példa hello DC/OS van konfigurálva, de Docker Swarm esetén hasonlóan fog kinézni.

    > [!NOTE]
    > Amikor ezt az alagutat létrehozza, a 80-as port nem lehet használatban.
    > 

    ![A PuTTY-konfigurálásának 3. lépése](./media/container-service-connect/putty3.png)

6. Amikor végzett, kattintson a **munkamenet > Mentés** toosave hello kapcsolat konfigurációját.

7. tooconnect toohello PuTTY munkamenet, kattintson a **nyitott**. Sikeres csatlakozás esetén hello portkonfigurációjának hello PuTTY eseménynaplójában tekintheti meg.

    ![A PuTTY eseménynaplója](./media/container-service-connect/putty4.png)

Miután konfigurálta a hello alagutat DC/os, van-e hozzáférési hello kapcsolódó a végpontokat:

* DC/OS:`http://localhost/`
* Marathon:`http://localhost/marathon`
* Mesos:`http://localhost/mesos`

Miután konfigurálta a hello alagutat a Docker Swarmra, nyissa meg a Windows-beállítások tooconfigure nevű rendszer környezeti változó `DOCKER_HOST` értékkel rendelkező `:2375`. Ezt követően érheti el hello Swarm-fürt hello Docker parancssori felületén keresztül.

## <a name="next-steps"></a>Következő lépések
Tárolók telepítése és felügyelete a fürtben:

* [Az Azure Container Service és a Kubernetes használata](../articles/container-service/kubernetes/container-service-kubernetes-ui.md)
* [Az Azure Container Service és a DC/OS használata](../articles/container-service//dcos-swarm/container-service-mesos-marathon-rest.md)
* [Hello Azure Tárolószolgáltatás és a Docker Swarm használata](../articles//container-service/dcos-swarm/container-service-docker-swarm.md)

