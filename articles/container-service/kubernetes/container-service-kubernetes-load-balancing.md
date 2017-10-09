---
title: "aaaLoad Azure Kubernetes a tárolók elosztása |} Microsoft Docs"
description: "Csatlakozás kívülről, és az Azure Tárolószolgáltatásban Kubernetes fürtben több tároló terheléselosztása."
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Tárolók, Micro-szolgáltatások, Kubernetes, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 8073c8d3a015a53a532c326749571cb2582e1bac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-containers-in-a-kubernetes-cluster-in-azure-container-service"></a>Load balance tárolók Kubernetes fürtben az Azure Tárolószolgáltatásban 
Ez a cikk az Azure Tárolószolgáltatásban Kubernetes fürtben terheléselosztási be. Terheléselosztás hello szolgáltatást biztosít egy kívülről elérhető IP-címet, majd továbbítja a hello három munkaállomás-csoporttal futó ügynök virtuális gépek közötti hálózati forgalom.

Beállíthat egy Kubernetes szolgáltatás toouse [Azure Load Balancer](../../load-balancer/load-balancer-overview.md) toomanage külső (TCP) hálózati forgalom. A további konfigurációs terhelés, és a HTTP vagy HTTPS-forgalom vagy speciális forgatókönyvekhez útválasztási is előfordulhatnak.

## <a name="prerequisites"></a>Előfeltételek
* [Kubernetes fürt központi telepítése](container-service-kubernetes-walkthrough.md) az Azure Tárolószolgáltatásban
* [Csatlakozás az ügyfél](../container-service-connect.md) tooyour fürt

## <a name="azure-load-balancer"></a>Az Azure terheléselosztó

Alapértelmezés szerint az Azure Tárolószolgáltatásban telepített Kubernetes fürt tartozik egy internetre irányuló Azure terheléselosztó hello ügynök virtuális gépeket. (Külön terheléselosztó erőforrást hello fő virtuális gépek van konfigurálva.) Az Azure terheléselosztó a réteg 4 terheléselosztó. Jelenleg hello terheléselosztó csak támogatja a TCP-forgalom a Kubernetes.

Egy Kubernetes szolgáltatás létrehozásakor hello Azure load balancer tooallow toohello szolgáltatás automatikusan konfigurálhatja. tooconfigure hello terheléselosztó hello szolgáltatás beállítása `type` túl`LoadBalancer`. hello terheléselosztót hoz létre egy szabály toomap egy nyilvános IP-cím és port bejövő szolgáltatás forgalom toohello privát IP-címek száma és hello három munkaállomás-csoporttal portszámait ügynök virtuális gépek (és fordítva a forgalom válasz). 

 Az alábbiakban látható, hogyan tooset hello Kubernetes szolgáltatás két példa `type` túl`LoadBalancer`. (Után közben hello példák törölni hello központi telepítések, ha már nem szükséges.)

### <a name="example-use-hello-kubectl-expose-command"></a>Példa: Használata hello `kubectl expose` parancs 
Hello [Kubernetes forgatókönyv](container-service-kubernetes-walkthrough.md) magában foglalja a példa bemutatja, hogyan tooexpose rendelkező hello szolgáltatást `kubectl expose` parancs és annak `--type=LoadBalancer` jelzőt. Az alábbiakban hello lépéseket:

1. Indítsa el az új tároló központi telepítést. Például a parancs elindul egy új központi telepítési hívása a következő hello `mynginx`. hello központi telepítés három tároló hello Nginx web Server hello Docker-lemezképen alapuló áll.

    ```console
    kubectl run mynginx --replicas=3 --image nginx
    ```
2. Győződjön meg arról, hogy fut-e a hello tárolók. Például, ha hello tárolók kérdezze le `kubectl get pods`, akkor tekintse meg a kimeneti hasonló toohello következőt:

    ![Nginx tároló beolvasása](./media/container-service-kubernetes-load-balancing/nginx-get-pods.png)

3. tooconfigure hello terhelés terheléselosztó tooaccept külső forgalom toohello központi telepítését, futtassa `kubectl expose` rendelkező `--type=LoadBalancer`. hello következő parancsot tesz elérhetővé hello Nginx-kiszolgáló 80-as porton:

    ```console
    kubectl expose deployments mynginx --port=80 --type=LoadBalancer
    ```

4. Típus `kubectl get svc` hello szolgáltatásainak hello fürt toosee hello állapotát. Amíg hello terheléselosztó hello szabály konfigurálja, hello `EXTERNAL-IP` a hello szolgáltatás jelenik meg `<pending>`. Néhány perc elteltével hello külső IP-cím van konfigurálva: 

    ![Az Azure terheléselosztó konfigurálása](./media/container-service-kubernetes-load-balancing/nginx-external-ip.png)

5. Győződjön meg arról, hogy van-e hozzáférési hello szolgáltatás hello külső IP-címen. Például nyisson meg egy webes böngésző toohello IP-cím látható. hello böngésző hello Nginx szolgáltatást futtató kiszolgáló valamelyik hello tárolók jeleníti meg. Vagy futtatási hello `curl` vagy `wget` parancsot. Példa:

    ```
    curl 13.82.93.130
    ```

    A következőhöz hasonló kimenetnek kell megjelennie:

    ![A curl hozzáférés Nginx](./media/container-service-kubernetes-load-balancing/curl-output.png)

6. hello Azure terheléselosztó, nyissa meg toohello toosee hello konfigurációja [Azure-portálon](https://portal.azure.com).

7. Tallózással keresse meg a tárolószolgáltatási fürt hello erőforráscsoportot, és válassza ki a terheléselosztó hello hello ügynök virtuális gépek. A név ugyanaz, mint a hello tárolószolgáltatás kell hello. (Válassza ki a terheléselosztó hello hello fő csomópontok, hello, amelynek a neve tartalmaz egy nem **master-lb**.) 

    ![Terheléselosztó erőforráscsoportban található](./media/container-service-kubernetes-load-balancing/container-resource-group-portal.png)

8. toosee hello részleteit hello terheléselosztó-konfiguráció, kattintson a **terheléselosztási szabályok** és hello szabály konfigurált hello neve.

    ![Terheléselosztói szabály](./media/container-service-kubernetes-load-balancing/load-balancing-rules.png) 

### <a name="example-specify-type-loadbalancer-in-hello-service-configuration-file"></a>Például: Adja meg, `type: LoadBalancer` hello szolgáltatás konfigurációs fájljában

Ha JSON vagy YAM Kubernetes tároló alkalmazást telepít [szolgáltatás konfigurációs fájlja](https://kubernetes.io/docs/user-guide/services/operations/#service-configuration-file), adjon meg egy külső terheléselosztó a következő sor toohello szolgáltatás meghatározása hello hozzáadásával:

```YAML
 "type": "LoadBalancer"
``` 



hello következő lépések használják hello Kubernetes [vendégkönyv példa](https://github.com/kubernetes/kubernetes/tree/master/examples/guestbook). Ez a példa egy többrétegű webalkalmazás Redis és a PHP Docker képek alapján. Hello szolgáltatás konfigurációs fájljában megadhatja, hello előtér PHP kiszolgálón hello Azure terheléselosztó használja.

1. Töltse le a hello fájl `guestbook-all-in-one.yaml` a [GitHub](https://github.com/kubernetes/kubernetes/tree/master/examples/guestbook/all-in-one). 
2. Tallózással keresse meg a hello `spec` a hello `frontend` szolgáltatás.
3. Állítsa vissza a hello sor `type: LoadBalancer`.

    ![Terheléselosztó szolgáltatás konfigurációja](./media/container-service-kubernetes-load-balancing/guestbook-frontend-loadbalance.png)

4. Hello fájl mentéséhez és hello alkalmazás telepítése hello a következő parancs futtatásával:

    ```
    kubectl create -f guestbook-all-in-one.yaml
    ```

5. Típus `kubectl get svc` hello szolgáltatásainak hello fürt toosee hello állapotát. Amíg hello terheléselosztó hello szabály konfigurálja, hello `EXTERNAL-IP` a hello `frontend` szolgáltatás jelenik meg `<pending>`. Néhány perc elteltével hello külső IP-cím van konfigurálva: 

    ![Az Azure terheléselosztó konfigurálása](./media/container-service-kubernetes-load-balancing/guestbook-external-ip.png)

6. Győződjön meg arról, hogy van-e hozzáférési hello szolgáltatás hello külső IP-címen. Például egy webes böngésző toohello külső IP-cím hello szolgáltatást is megnyithatja.

    ![Külső hozzáférés vendégkönyv](./media/container-service-kubernetes-load-balancing/guestbook-web.png)

    Vendégkönyv bejegyzést adhat hozzá.

7. hello Azure terheléselosztó, hello terheléselosztó erőforrást tallózzon a hello hello fürt toosee hello konfigurációja [Azure-portálon](https://portal.azure.com). Lásd: hello hello előző példában szereplő lépéseket.

### <a name="considerations"></a>Megfontolandó szempontok

* Hello terheléselosztási szabály létrehozása aszinkron módon történik, és a kiépített hello terheléselosztó információt közzé van téve, hello szolgáltatásban `status.loadBalancer` mező.
* Minden szolgáltatás automatikusan saját hello terheléselosztó virtuális IP-címhez.
* Ha a DNS-név tooreach hello terheléselosztóhoz, dolgozni a tartományi szolgáltatás szolgáltató toocreate hello szabály IP-címet a DNS-nevét.

## <a name="http-or-https-traffic"></a>HTTP vagy HTTPS-forgalom

tooload egyenleg HTTP vagy HTTPS-forgalom toocontainer webalkalmazások és kezelheti a transport layer security (TLS) tanúsítványait, használhatja a hello Kubernetes [érkező](https://kubernetes.io/docs/user-guide/ingress/) erőforrás. Egy érkező olyan szabályok, amelyek lehetővé teszik a bejövő kapcsolatok tooreach hello fürtszolgáltatások gyűjteménye. Egy érkező erőforrás toowork hello Kubernetes fürt rendelkeznie kell egy [érkező vezérlő](https://kubernetes.io/docs/user-guide/ingress/#ingress-controllers) futtatása.

Az Azure Tárolószolgáltatás nem valósítja meg a Kubernetes érkező vezérlő automatikusan. Több vezérlő megvalósítások érhetők el. Jelenleg hello [Nginx érkező vezérlő](https://github.com/kubernetes/ingress/tree/master/examples/deployment/nginx) javasolt tooconfigure érkező szabályok és a terheléselosztás HTTP és HTTPS-forgalmat. 

További információkért lásd: hello [Nginx érkező dokumentációjában](https://github.com/kubernetes/ingress/tree/master/controllers/nginx/README.md).

> [!IMPORTANT]
> Hello Nginx érkező vezérlő használata az Azure Tárolószolgáltatásban, fel kell fednie hello tartományvezérlő központi telepítése a szolgáltatásként `type: LoadBalancer`. Ez konfigurálja az hello Azure load balancer tooroute forgalom toohello vezérlő. További információkért lásd: hello előző szakaszban.


## <a name="next-steps"></a>Következő lépések

* Lásd: hello [Kubernetes terheléselosztó dokumentáció](https://kubernetes.io/docs/user-guide/load-balancer/)
* További információ [Kubernetes érkező és a bejövő adatok tartományvezérlők](https://kubernetes.io/docs/user-guide/ingress/)
* Lásd: [Kubernetes példák](https://github.com/kubernetes/kubernetes/tree/master/examples)

