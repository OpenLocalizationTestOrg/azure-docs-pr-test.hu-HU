---
title: "az Azure Tárolószolgáltatás és Azure tároló beállításjegyzék vázlat aaaUse |} Microsoft Docs"
description: "Az ACS Kubernetes fürt és az Azure-tároló beállításjegyzék toocreate az első alkalmazás létrehozása az Azure-ban Vázlat."
services: container-service
documentationcenter: 
author: squillace
manager: gamonroy
editor: 
tags: draft, helm, acs, azure-container-service
keywords: "Docker, tárolók, mikroszolgáltatások, Kubernetes, Draft, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: rasquill
ms.custom: mvc
ms.openlocfilehash: f5e21cda01e5e8452bf86a5c8fa458904d89f451
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-draft-with-azure-container-service-and-azure-container-registry-toobuild-and-deploy-an-application-tookubernetes"></a>Azure Tárolószolgáltatás és Azure tároló beállításjegyzék toobuild vázlat használjon, és egy alkalmazás tooKubernetes telepítése

[Vázlat](https://aka.ms/draft) egy új nyílt forráskódú eszköz, így könnyen toodevelop tároló-alapú alkalmazások és azok tooKubernetes fürtök nélkül nagy Docker és Kubernetes--ismerete, vagy még akkor is telepíteni kell őket. Vázlat hasonló eszközökkel legyen, és a csoportok fókusz Kubernetes hello alkalmazás felépítése, nem a legtöbb figyelmet tooinfrastructure fizet.

A Draft bármely Docker-rendszerképjegyzékkel és Kubernetes-fürttel használható, beleértve a helyi használatot. Ez az oktatóanyag bemutatja, hogyan toouse ACS Kubernetes ACR és Azure DNS toocreate az élő CI/CD fejlesztők csővezeték-vázlat használata.


## <a name="create-an-azure-container-registry"></a>Azure Container Registry létrehozása
Egyszerűen [hozzon létre egy új Azure-tároló beállításjegyzék](../../container-registry/container-registry-get-started-azure-cli.md), de hello lépései a következők:

1. Hozzon létre egy Azure-erőforrás csoport toomanage a ACR beállításjegyzék és hello Kubernetes fürtöt az ACS szolgáltatásban.
      ```azurecli
      az group create --name draft --location eastus
      ```

2. Hozzon létre egy ACR-rendszerképjegyzéket az [az acr create](/cli/azure/acr#create) használatával
      ```azurecli
      az acr create -g draft -n draftacs --sku Basic --admin-enabled true -l eastus
      ```


## <a name="create-an-azure-container-service-with-kubernetes"></a>Azure Container Service létrehozása a Kubernetes-szel

Most már készen áll a toouse Ön [az acs létre](/cli/azure/acs#create) Kubernetes használata hello toocreate az ACS-fürt `--orchestrator-type` érték.
```azurecli
az acs create --resource-group draft --name draft-kube-acs --dns-prefix draft-cluster --orchestrator-type kubernetes
```

> [!NOTE]
> Kubernetes nincs hello alapértelmezett orchestrator típus, mert kell használnia, hello `--orchestrator-type kubernetes` váltani.

hello kimeneti, ha sikeres a következőhöz hasonló toohello következő.

```json
waiting for AAD role toopropagate.done
{
  "id": "/subscriptions/<guid>/resourceGroups/draft/providers/Microsoft.Resources/deployments/azurecli14904.93snip09",
  "name": "azurecli1496227204.9323909",
  "properties": {
    "correlationId": "<guid>",
    "debugSetting": null,
    "dependencies": [],
    "mode": "Incremental",
    "outputs": null,
    "parameters": {
      "clientSecret": {
        "type": "SecureString"
      }
    },
    "parametersLink": null,
    "providers": [
      {
        "id": null,
        "namespace": "Microsoft.ContainerService",
        "registrationState": null,
        "resourceTypes": [
          {
            "aliases": null,
            "apiVersions": null,
            "locations": [
              "westus"
            ],
            "properties": null,
            "resourceType": "containerServices"
          }
        ]
      }
    ],
    "provisioningState": "Succeeded",
    "template": null,
    "templateLink": null,
    "timestamp": "2017-05-31T10:46:29.434095+00:00"
  },
  "resourceGroup": "draft"
}
```

Most, hogy a fürt, hello hitelesítő adatokat importálhat hello segítségével [az acs kubernetes get-hitelesítő adatok](/cli/azure/acs/kubernetes#get-credentials) parancsot. Most már rendelkezik a helyi konfigurációs fájlt a fürt, amely milyen Helm és vázlathoz kell tooget a munkájukat.

## <a name="install-and-configure-draft"></a>A draft telepítése és konfigurálása
hello telepítési utasításait vázlat szerepelnek hello [vázlat tárház](https://github.com/Azure/draft/blob/master/docs/install.md). Viszonylag könnyen, de egyéb konfigurációra van szükség, attól függ, mint [Helm](https://aka.ms/helm) toocreate és központi telepítése egy Helm diagram hello Kubernetes fürtbe.

1. [A Helm letöltése és telepítése](https://aka.ms/helm#install).
2. A Helm toosearch használ, és telepíti `stable/traefik`, és a bejövő kérelmek a buildek érkező vezérlő tooenable.
    ```bash
    $ helm search traefik
    NAME            VERSION DESCRIPTION
    stable/traefik  1.3.0   A Traefik based Kubernetes ingress controller w...

    $ helm install stable/traefik --name ingress
    ```
    A figyelési most be hello `ingress` vezérlő toocapture hello külső IP érték, ha telepítve van. Az IP-cím lesz egy hello [tooyour telepítési tartományhoz csatlakoztatott](#wire-up-deployment-domain) hello a következő szakaszban.

    ```bash
    kubectl get svc -w
    NAME                          CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
    ingress-traefik               10.0.248.104   13.64.108.240   80:31046/TCP,443:32556/TCP   1h
    kubernetes                    10.0.0.1       <none>          443/TCP                      7h
    ```

    Ebben az esetben a hello telepítési tartomány: hello külső IP `13.64.108.240`. Most már a toothat IP-Címének is leképezheti.

## <a name="wire-up-deployment-domain"></a>Az üzembe helyezési tartomány beállítása

A Draft kiadást hoz létre minden egyes létrehozott Helm-diagramhoz – minden egyes alkalmazáshoz, amin dolgozik. Mindegyik lekérdezi egy létrehozott nevet, mint a Vázlat által használt egy _altartomány_ hello legfelső szintű funkciók mellett kártevőészlelést _telepítési tartomány_ , amelyek. (A jelen példában használjuk `squillace.io` hello telepítési tartomány.) tooenable altartomány a jelenség kell létrehoznia A rekordjainak `'*'` a központi telepítés tartomány, a DNS-bejegyzéseket, hogy minden egyes létrehozott altartomány irányított toohello Kubernetes fürt érkező vezérlő.

Saját tartomány szolgáltatónak van-e a saját módon tooassign DNS-kiszolgálók; túl[delegálása a tartományi nameservers tooAzure DNS](../../dns/dns-delegate-domain-azure-dns.md), hello lépések végrehajtása:

1. Hozzon létre egy erőforráscsoportot a zóna számára.
    ```azurecli
    az group create --name squillace.io --location eastus
    {
      "id": "/subscriptions/<guid>/resourceGroups/squillace.io",
      "location": "eastus",
      "managedBy": null,
      "name": "zones",
      "properties": {
        "provisioningState": "Succeeded"
      },
      "tags": null
    }
    ```

2. Hozzon létre DNS-zónát a tartományához.
Használjon hello [az hálózati DNS-zóna létrehozása](/cli/azure/network/dns/zone#create) tooobtain hello nameservers toodelegate DNS szabályozhatja a tartomány DNS tooAzure parancsot.
    ```azurecli
    az network dns zone create --resource-group squillace.io --name squillace.io
    {
      "etag": "<guid>",
      "id": "/subscriptions/<guid>/resourceGroups/zones/providers/Microsoft.Network/dnszones/squillace.io",
      "location": "global",
      "maxNumberOfRecordSets": 5000,
      "name": "squillace.io",
      "nameServers": [
        "ns1-09.azure-dns.com.",
        "ns2-09.azure-dns.net.",
        "ns3-09.azure-dns.org.",
        "ns4-09.azure-dns.info."
      ],
      "numberOfRecordSets": 2,
      "resourceGroup": "squillace.io",
      "tags": {},
      "type": "Microsoft.Network/dnszones"
    }
    ```
3. Adja hozzá a hello DNS-kiszolgálók toohello tartomány szolgáltató lehetősége van a központi telepítés tartományba, amely lehetővé teszi, hogy Ön toouse Azure DNS toorepoint a tartomány ahányat csak szeretne.
4. A központi telepítés tartomány leképezése toohello A rekordhalmaz bejegyzés létrehozása `ingress` IP hello előző szakaszban 2. lépés.
    ```azurecli
    az network dns record-set a add-record --ipv4-address 13.64.108.240 --record-set-name '*' -g squillace.io -z squillace.io
    ```
hello kimeneti alábbihoz hasonló:
    ```json
    {
      "arecords": [
        {
          "ipv4Address": "13.64.108.240"
        }
      ],
      "etag": "<guid>",
      "id": "/subscriptions/<guid>/resourceGroups/squillace.io/providers/Microsoft.Network/dnszones/squillace.io/A/*",
      "metadata": null,
      "name": "*",
      "resourceGroup": "squillace.io",
      "ttl": 3600,
      "type": "Microsoft.Network/dnszones/A"
    }
    ```

5. Konfigurálja a beállításjegyzéket az Vázlat toouse és altartományok minden Helm diagram hoz létre. tooconfigure vázlat, lesz szüksége:
  - Az Azure Container Registry neve (a példában `draft`)
  - A beállításkulcs vagy jelszó a következőből: `az acr credential show -n <registry name> --output tsv --query "passwords[0].value"`.
  - hello telepítési gyökértartomány, hogy konfigurálta-e az toomap toohello Kubernetes érkező külső IP-cím (Itt `squillace.io`)

  Hívás `draft init` és hello konfigurációs folyamat felajánlja a fenti hello értékek. hello folyamat tűnik valami hello következő hello első alkalommal futtatja azt.
 ```bash
    $ draft init
    Creating pack ruby...
    Creating pack node...
    Creating pack gradle...
    Creating pack maven...
    Creating pack php...
    Creating pack python...
    Creating pack dotnetcore...
    Creating pack golang...
    $DRAFT_HOME has been configured at /Users/ralphsquillace/.draft.

    In order tooinstall Draft, we need a bit more information...

    1. Enter your Docker registry URL (e.g. docker.io, quay.io, myregistry.azurecr.io): draft.azurecr.io
    2. Enter your username: draft
    3. Enter your password:
    4. Enter your org where Draft will push images [draft]: draft
    5. Enter your top-level domain for ingress (e.g. draft.example.com): squillace.io
    Draft has been installed into your Kubernetes Cluster.
    Happy Sailing!
    ```

Most már készen áll a toodeploy egy alkalmazás most.


## <a name="build-and-deploy-an-application"></a>Alkalmazás fejlesztése és üzembe helyezése

Hello vázlat tárházban vannak [hat egyszerű példa alkalmazások](https://github.com/Azure/draft/tree/master/examples). Hello tárház klónozása, és most használja az hello [Python példa](https://github.com/Azure/draft/tree/master/examples/python). Váltson át hello példák/Python könyvtár, és írja be `draft create` toobuild hello alkalmazás. A következő példa hello kell hasonlítania.
```bash
$ draft create
--> Python app detected
--> Ready toosail
```

hello kimenete egy Dockerfile és Helm diagram. toobuild és telepíteni, csak gépelje `draft up`. hello kimeneti túl hosszú, de a következő példa hello például kezdődik.
```bash
$ draft up
--> Building Dockerfile
Step 1 : FROM python:onbuild
onbuild: Pulling from library/python
10a267c67f42: Pulling fs layer
fb5937da9414: Pulling fs layer
9021b2326a1e: Pulling fs layer
dbed9b09434e: Pulling fs layer
ea8a37f15161: Pulling fs layer
<snip>
```

mikor és a következő példa valami hasonló toohello sikeres multiplicitású.
```bash
ab68189731eb: Pushed
53c0ab0341bee12d01be3d3c192fbd63562af7f1: digest: sha256:bb0450ec37acf67ed461c1512ef21f58a500ff9326ce3ec623ce1e4427df9765 size: 2841
--> Deploying tooKubernetes
--> Status: DEPLOYED
--> Notes:

  http://gangly-bronco.squillace.io tooaccess your application

Watching local files for changes...
```

Függetlenül a diagram neve van, akkor most `curl http://gangly-bronco.squillace.io` tooreceive hello válasz, `Hello World!`.

## <a name="next-steps"></a>Következő lépések

Most, hogy az ACS Kubernetes fürt, segítségével kivizsgálhatja [Azure tároló beállításjegyzék](../../container-registry/container-registry-intro.md) toocreate további és különböző példányainak ebben a forgatókönyvben. Például létrehozhat egy draft._basedomain.toplevel_ tartományi DNS-rekordkészletet, amely mélyebb altartományból szabályozza a dolgokat adott ACS-környezetekhez.






