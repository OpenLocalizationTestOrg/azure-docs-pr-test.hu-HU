# <a name="scale-agent-nodes-in-a-container-service-cluster"></a>Ügynökcsomópontok méretezése a Container Service-fürtökben
Miután [Azure Tárolószolgáltatás-fürt telepítése](../articles/container-service/dcos-swarm/container-service-deployment.md), előfordulhat, hogy az ügynök csomópontok toochange hello számát. Például ha több ügynökre van szüksége további tárolóalkalmazások vagy -példányok futtatásához. 

DC/OS, Docker Swarm vagy Kubernetes fürt csomópontjai ügynök hello számának módosításához hello Azure-portál használatával, vagy Azure CLI 2.0 hello. 

## <a name="scale-with-hello-azure-portal"></a>Méretezést hello Azure-portálon

1. A hello [Azure-portálon](https://portal.azure.com), tallózással keresse meg **tárolószolgáltatásainak**, majd kattintson a megjeleníteni kívánt toomodify hello tárolószolgáltatás.
2. A hello **tárolószolgáltatás** panelen kattintson a **ügynökök**.
3. A **virtuális gépek száma**, adja meg a szükséges hello ügynökök csomópontok száma.

    ![Egy készlet hello portálon méretezése](./media/container-service-scale/container-service-scale-portal.png)

4. toosave hello konfigurációs, kattintson a **mentése**.

## <a name="scale-with-hello-azure-cli-20"></a>Az Azure CLI 2.0 hello méretezése

Győződjön meg arról, hogy Ön [telepített](/cli/azure/install-az-cli2) hello Azure CLI legújabb 2.0 és tooan bejelentkezve azure-fiók (`az login`).

### <a name="see-hello-current-agent-count"></a>Lásd: hello aktuális ügynökök száma
jelenleg ügynökök száma toosee hello hello fürtben futtatni hello `az acs show` parancsot. Ez azt jelenti, hello fürtkonfiguráció. Például a parancs azt mutatja be hello nevű hello tároló szolgáltatás konfigurációját a következő hello `containerservice-myACSName` hello erőforráscsoportban `myResourceGroup`:

```azurecli
az acs show -g myResourceGroup -n containerservice-myACSName
```

hello parancs hello ügynökök számát adja vissza hello `Count` értékét `AgentPoolProfiles`.

### <a name="use-hello-az-acs-scale-command"></a>Használja hello az acs méretezési parancs
ügynök csomópontok, futtassa a hello toochange hello száma `az acs scale` parancsot, és a szállítási hello **erőforráscsoport**, **Tárolónév-szolgáltatás**, és a szükséges hello **új ügynökök száma**. Alacsonyabb illetve magasabb érték megadásával vertikálisan le- illetve felskálázhatja a fürtöt.

Például toochange hello ügynökök száma hello előző fürt too10, típus hello a következő parancsot:

```azurecli
az acs scale -g myResourceGroup -n containerservice-myACSName --new-agent-count 10
```

hello Azure CLI 2.0 adja vissza JSON karakterláncként hello új konfigurációjának hello tárolószolgáltatás, beleértve a hello új ügynökök száma.

További parancsbeállításokért futtassa az `az acs scale --help` parancsot.

## <a name="scaling-considerations"></a>Méretezési szempontok

* hello ügynök csomópontok száma 1 és 100, a határokat is beleértve között kell lennie. 

* A magok kvótájának hello ügynök fürtben található csomópontok számát korlátozhatja.

* Ügynök csomópont skálázási műveletek esetében alkalmazott tooan Azure virtuális gépek méretezési hello ügynök készletet tartalmazó. A DC/OS fürtben csak ügynök csomópontok hello titkos készletben méretezve, ebben a cikkben szereplő hello műveletei által.

* Attól függően, hogy a fürt telepítése hello orchestrator külön-külön méretezheti a tároló hello fürtben futó példányok hello száma. Például a DC/OS fürtben, használja a hello [Marathon felhasználói felület](../articles/container-service/dcos-swarm/container-service-mesos-marathon-ui.md) toochange hello több példányban egy tároló alkalmazást.

* Az ügynökcsomópontok automatikus méretezése a Container Service-fürtökben jelenleg nem támogatott.

## <a name="next-steps"></a>Következő lépések
* Tekintse meg az Azure CLI 2.0-parancsok az Azure Container Service szolgáltatásban való használatát bemutató [további példákat](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md).
* Ismerkedjen meg a [DC/OS-ügynökkészletekkel](../articles/container-service/dcos-swarm/container-service-dcos-agents.md) az Azure Container Service szolgáltatásban.

