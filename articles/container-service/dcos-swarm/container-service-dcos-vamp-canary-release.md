---
title: "Azure DC/OS-fürtön a Vamp aaaCanary kiadás |} Microsoft Docs"
description: "Hogyan toouse Vamp toocanary kiadás szolgáltatások, és egy Azure tároló szolgáltatás DC/OS-fürtről végez a intelligens forgalom alkalmazása"
services: container-service
author: gggina
manager: rasquill
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/17/2017
ms.author: rasquill
ms.custom: mvc
ms.openlocfilehash: e7b8658a161a7cddcf718e3e1c12a889a330d3d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="canary-release-microservices-with-vamp-on-an-azure-container-service-dcos-cluster"></a>Egy Azure tároló szolgáltatás DC/OS-fürtön a Vamp Kanári kiadás mikroszolgáltatások

Ebben a bemutatóban beállítjuk Vamp Azure tárolószolgáltatás és a DC/OS-fürtről. Azt Kanári szabadítsa fel a hello Vamp bemutató szolgáltatás "Száva", és oldja meg a Firefox hello szolgáltatást inkompatibilitás intelligens forgalomszűrést végez alkalmazásával. 

> [!TIP] 
> A forgatókönyv Vamp a DC/OS-fürtön fut, de akkor is használható Vamp Kubernetes a hello orchestrator.
>

## <a name="about-canary-releases-and-vamp"></a>Feloldja a Kanári kapcsolatos és Vamp


[Kanári felszabadítása](https://martinfowler.com/bliki/CanaryRelease.html) például Netflix, a Facebook-on és a Spotify új szervezetek által elfogadott intelligens központi telepítési stratégiát is. Értelme, mivel csökkenti a problémákat, biztonsági-háló vezet be, és növeli az innováció egy megközelítést. Ezért miért nem minden vállalat használja azt? A CI/CD adatcsatorna tooinclude Kanári stratégiák kiterjesztése hozzáadja összetettségét, majd kiterjedt devops tudásuk és tapasztalataik igényel. Ez elég tooblock kisebb vállalatok és a vállalatok egyaránt ahhoz, azok még a kezdéshez. 

[Vamp](http://vamp.io/) egy nyílt forráskódú rendszert tooease Ez a változás, és kapcsolja a szolgáltatások előnyben részesített tooyour Kanári lazító tároló Feladatütemező. Vamp tartozó Kanári funkció túllép százalék alapú exportálása. Forgalom szűrve, és számos különböző feltételek, például adott felhasználók tootarget, IP-címtartományok vagy eszközök felosztással állít elő. Vamp nyomon követi, és elemzi a teljesítménymutatók, lehetővé teszi a automation valós adatok alapján. A hibák automatikus visszaállítási beállítása, vagy méretezni terhelés alapján vagy késés szolgáltatás Variant adattípusban.

## <a name="set-up-azure-container-service-with-dcos"></a>Azure Tárolószolgáltatási DC/OS rendelkező beállítása



1. [A DC/OS-fürt üzembe](container-service-deployment.md) egy fő és a két ügynök alapértelmezett mérete. 

2. [SSH-alagút létrehozása](../container-service-connect.md) tooconnect toohello DC/OS-fürtről. Ez a cikk feltételezi, hogy a 80-as portjához helyi toohello fürt alagútkezelésre.


## <a name="set-up-vamp"></a>Vamp beállítása

Most, hogy a futó DC/OS-fürtről, a DC/OS felhasználói felületének (http://localhost:80) hello Vamp is telepíthet. 

![A DC/OS UI felhasználói felülete](./media/container-service-dcos-vamp-canary-release/01_set_up_vamp.png)

Telepítés két lépésben történik:

1. **Elasticsearch telepítése**.

2. Majd **Vamp telepítése** hello Vamp DC/OS universe csomag telepítésével.

### <a name="deploy-elasticsearch"></a>Elasticsearch telepítése

Vamp Elasticsearch metrikák adatgyűjtési és -összesítési igényel. Használhatja a hello [magneticio Docker képek](https://hub.docker.com/r/magneticio/elastic/) toodeploy egy kompatibilis Vamp Elasticsearch verem.

1. A DC/OS felhasználói felületének hello, váltson túl**szolgáltatások** kattintson **szolgáltatás telepítése**.

2. Válassza ki **JSON üzemmódot** a hello **új szolgáltatás telepítése** előugró.

  ![Válassza ki a JSON üzemmód](./media/container-service-dcos-vamp-canary-release/02_deploy_service_json_mode.png)

3. Illessze be a következő JSON hello. Ez a konfiguráció hello tárolóban fut, 1 GB RAM és hello Elasticsearch port alapszintű állapotának ellenőrzése.
  
  ```JSON
  {
    "id": "elasticsearch",
    "instances": 1,
    "cpus": 0.2,
    "mem": 1024.0,
    "container": {
      "docker": {
        "image": "magneticio/elastic:2.2",
        "network": "HOST",
        "forcePullImage": true
      }
    },
    "healthChecks": [
      {
        "protocol": "TCP",
        "gracePeriodSeconds": 30,
        "intervalSeconds": 10,
        "timeoutSeconds": 5,
        "port": 9200,
        "maxConsecutiveFailures": 0
      }
    ]
  }
  ```
  

3. Kattintson a **telepítése**.

  A DC/OS hello Elasticsearch tároló telepíti. Akkor tudja nyomon követni a hello **szolgáltatások** lap.  

  ![e telepítéséhez? Elasticsearch](./media/container-service-dcos-vamp-canary-release/03_deply_elasticsearch.png)

### <a name="deploy-vamp"></a>Vamp telepítése

Miután Elasticsearch jelzi **futtató**, hello Vamp DC/OS Universe csomag is hozzáadhat. 

1. Nyissa meg túl**Universe** keresse meg a **vamp**. 
  ![A DC/OS universe vamp](./media/container-service-dcos-vamp-canary-release/04_universe_deploy_vamp.png)

2. Kattintson a **telepítése** következő toohello csomag vamp, és válassza a **speciális telepítési**.

3. Görgessen lefelé, és írja be a következő elasticsearch-url hello: `http://elasticsearch.marathon.mesos:9200`. 

  ![Adja meg a Elasticsearch URL-címe](./media/container-service-dcos-vamp-canary-release/05_universe_elasticsearch_url.png)

4. Kattintson a **áttekintése és telepítése**, majd kattintson a **telepítése** toostart hello központi telepítés.  

  A DC/OS Vamp minden szükséges összetevőket telepíti. Akkor tudja nyomon követni a hello **szolgáltatások** lap.
  
  ![Vamp universe csomag telepítése](./media/container-service-dcos-vamp-canary-release/06_deploy_vamp.png)
  
5. Központi telepítés befejezése után hello Vamp felhasználói felület érhető el:

  ![A DC/OS vamp szolgáltatás](./media/container-service-dcos-vamp-canary-release/07_deploy_vamp_complete.png)
  
  ![Felhasználói felület vamp](./media/container-service-dcos-vamp-canary-release/08_vamp_ui.png)


## <a name="deploy-your-first-service"></a>Az első szolgáltatás üzembe helyezése

Most, hogy Vamp működik-e és fut, a tervezetének a szolgáltatás telepítése. 

Legegyszerűbb formájukban a [Vamp tervezetének](http://vamp.io/documentation/using-vamp/blueprints/) hello végpontok (átjárók), a fürtök és a szolgáltatások toodeploy ismerteti. Vamp használ fürtök toogroup különböző változatai hello azonos logikai csoportokba szolgáltatás Kanári feloldása vagy A / B tesztelés.  

Ez a forgatókönyv használ nevű egységes mintaalkalmazás [ **Száva**](https://github.com/magneticio/sava), amely jelenleg 1.0-s verziója. hello monolit egy Docker-tároló, amely Docker központban magneticio/sava:1.0.0 alatt van csomagolva. hello alkalmazás megfelelően fut a 8080-as porton, de azt szeretné, hogy ebben az esetben port e 9050 tooexpose. Egy egyszerű tervezetének használatával Vamp keresztül hello alkalmazás telepítése.

1. Nyissa meg túl**központi telepítések**.

2. Kattintson az **Add** (Hozzáadás) parancsra.

3. Illessze be a következő hello tervezetének YAM. Ez tervezetének egy fürt csak egy szolgáltatás variant, amely egy későbbi lépésben módosítjuk tartalmazza:

  ```YAML
  name: sava                        # deployment name
  gateways:
    9050: sava_cluster/webport      # stable endpoint
  clusters:
    sava_cluster:               # cluster toocreate
     services:
        -
          breed:
            name: sava:1.0.0        # service variant name
            deployable: magneticio/sava:1.0.0
            ports:
              webport: 8080/http # cluster endpoint, used for canary releasing
  ```

4. Kattintson a **Save** (Mentés) gombra. Vamp hello telepítési kezdeményezi.

hello telepítési szerepel hello **központi telepítések** lap. Kattintson a hello telepítési toomonitor annak állapotát.

![Vamp UI - Száva telepítése](./media/container-service-dcos-vamp-canary-release/09_sava100.png)

![a felhasználói felületen Vamp Száva szolgáltatás](./media/container-service-dcos-vamp-canary-release/09a_sava100.png)

Két átjáró jönnek létre, amely szerepel hello **átjárók** lap:

* egy stabil végpont tooaccess hello szolgáltatást (port 9050) futtató 
* Vamp által kezelt belső átjáró (erről az átjáróról később további). 

![Felhasználói felület – Száva átjárók vamp](./media/container-service-dcos-vamp-canary-release/10_vamp_sava_gateways.png)

hello Száva szolgáltatás már telepítve van, de nem férhet hozzá az külsőleg mert hello Azure Load Balancer még nem ismert tooforward forgalom tooit. tooaccess hello szolgáltatást, a frissítés hello Azure hálózati konfiguráció.


## <a name="update-hello-azure-network-configuration"></a>Hello Azure hálózati konfiguráció frissítése

Telepített vamp hello Száva szolgáltatás csomópontján hello DC/OS ügynök, egy stabil végpont-es porton 9050 teszi ki. tooaccess hello szolgáltatást külső hello DC/OS-fürtről, ellenőrizze a következő módosításokat toohello Azure hálózati konfigurációt a fürtöt tartalmazó környezetben hello: 

1. **Azure Load Balancer hello konfigurálása** hello ügynökök (nevű erőforrás hello **vezénylőtípusú-ügynök-lb-xxxx**) egy állapotmintáihoz és a szabály tooforward forgalom port 9050 toohello Száva példányokon. 

2. **Hello hálózati biztonsági csoport** hello nyilvános ügynökök (nevű erőforrás hello **XXXX-ügynök – nyilvános-nsg-XXXX**) port 9050 tooallow forgalmát.

A részletes lépéseket toocomplete ezek a feladatok az Azure portál, lásd: hello [nyilvános hozzáférés tooan Azure Tárolószolgáltatás-alkalmazás engedélyezése](container-service-enable-public-access.md). Adja meg az összes portbeállítások 9050 portot.


Ha minden létrejött, nyissa meg toohello **áttekintése** hello DC/OS-ügynök terheléselosztó panel (hello nevű erőforrás **vezénylőtípusú-ügynök-lb-xxxx**). Hello található **nyilvános IP-cím**, és hello cím tooaccess Száva port 9050 használatát.

![Azure portál – get nyilvános IP-cím](./media/container-service-dcos-vamp-canary-release/18_public_ip_address.png)

![Száva](./media/container-service-dcos-vamp-canary-release/19_sava100.png)


## <a name="run-a-canary-release"></a>Kanári kiadási futtatása

Tegyük fel, az alkalmazás új verzióját, amelyet az éles környezetben toocanary kiadás. Annak indexelése magneticio/sava:1.1.0, és készen áll a toogo. Vamp lehetővé teszi a könnyen a telepítést futtató új szolgáltatások toohello hozzáadását. A "egyesített" szolgáltatások hello meglévő szolgáltatások hello fürt együtt telepített, és egy 0 %-os értéket kapó. Sincs forgalom újonnan egyesített irányított tooa szolgáltatás, amíg a hello forgalomeloszlás módosíthatja. hello Vamp felhasználói felületén súly csúszkája hello lehetővé teszi az hello terjesztési, lehetővé teszi a növekményes módosításának (Kanári kiadás) vagy egy azonnali visszavonás teljes ellenőrzését.

### <a name="merge-a-new-service-variant"></a>Egy új szolgáltatás variant egyesítése

toomerge hello új Száva 1.1 szolgáltatás a telepítést futtató hello:

1. Hello Vamp felhasználói felület, kattintson **tervrajzokat**.

2. Kattintson a **Hozzáadás** és illessze be a következő hello tervezetének YAM: A tervezetének ismerteti egy új szolgáltatás variant (Száva: 1.1.0-ás) toodeploy hello meglévő fürt (sava_cluster) belül.

  ```YAML
  name: sava:1.1.0      # blueprint name
  clusters:
    sava_cluster:       # cluster tooupdate
      services:
        -
          breed:
            name: sava:1.1.0    # service variant name
            deployable: magneticio/sava:1.1.0    
            ports:
              webport: 8080/http # cluster endpoint tooupdate
  ```
  
3. Kattintson a **Save** (Mentés) gombra. hello tervezetének tárolja, és meg hello **tervrajzokat** lap.

4. Megnyitás hello művelet menü elemre, majd hello Száva: 1.1 tervezetének **egyesítése**.

  ![Felhasználói felület – tervrajzokat vamp](./media/container-service-dcos-vamp-canary-release/20_sava110_mergeto.png)

5. Jelölje be hello **Száva** üzembe helyezési és kattintson **egyesítése**.

  ![Felhasználói felület vamp - tervezetének toodeployment egyesítése](./media/container-service-dcos-vamp-canary-release/21_sava110_merge.png)

Vamp telepíti hello új Száva: 1.1.0-ás szolgáltatás variant hello tervezetének mellett Száva: 1.0.0 hello a leírt **sava_cluster** a telepítést futtató hello. 

![Felhasználói felület – frissített Száva telepítési vamp](./media/container-service-dcos-vamp-canary-release/22_sava_cluster.png)

Hello **Száva/sava_cluster/webport** átjáró (hello a fürt végpontja) is frissül, egy útvonal toohello hozzáadása újonnan telepített Száva: 1.1.0-ás. Ezen a ponton nem továbbítódik itt (hello **súly** too0 % van beállítva).

![Felhasználói felület - fürt átjáró vamp](./media/container-service-dcos-vamp-canary-release/23_sava_cluster_webport.png)

### <a name="canary-release"></a>Kanári kiadás

A Száva verzióját is üzembe helyezett hello azonos fürt esetén állítsa be őket hello mozgatásával forgalom eloszlása hello **súly** csúszkát.

1. Kattintson a ![Vamp UI - szerkesztése](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) következő túl**súly**.

2. Hello súly terjesztési too50%/50% beállítva, és kattintson a **mentése**.

  ![Felhasználói felület – átjáró súly csúszkát vamp](./media/container-service-dcos-vamp-canary-release/24_sava_cluster_webport_weight.png)

3. Lépjen vissza tooyour böngészőt, és néhány alkalommal hello Száva lap frissítése. Száva alkalmazás hello vált egy Száva: 1.0 lap és Száva: 1.1 oldal között.

  ![váltakozó sava1.0 és sava1.1 szolgáltatások](./media/container-service-dcos-vamp-canary-release/25_sava_100_101.png)


  > [!NOTE]
  > A váltási hello lap működik a legjobban hello "Incognito" vagy "Névtelen" módban, a böngésző hello statikus rendezett gyorsítótárazása miatt.
  >

### <a name="filter-traffic"></a>Szűrő forgalom

Tegyük fel, hogy a központi telepítést követően, hogy a inkompatibilitás Száva: 1.1.0-ás okok Firefox böngészőket problémákat jeleníti meg a felderíteni. Állítsa be a Vamp toofilter bejövő forgalmat, és közvetlen senki Firefox toohello stabil Száva: 1.0.0 ismert biztonsági. Ez a szűrő instantly oldja fel a rendszer hello megszűnésének a Firefox felhasználók számára, miközben mindenki más tovább tooenjoy hello előnyei hello továbbfejlesztett Száva: 1.1.0-ás.

Felhasználási vamp **feltételek** toofilter forgalom útvonal az átjáró között. Forgalom függően toohello alkalmazott feltételek tooeach útvonal vezérelt, és először vannak leszűrve. Az összes többi forgalom toohello átjáró súlyozási beállítás szerint történik.

Létrehozhat egy feltétel toofilter senki Firefox és toohello régi Száva: 1.0.0 közvetlen őket:

1. A Száva/sava_cluster/webport hello **átjárók** kattintson ![Vamp UI - szerkesztése](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) tooadd egy **feltétel** toohello útvonal sava/sava_cluster/sava:1.0.0/webport. 

2. Adja meg a hello feltétel **felhasználói ügynök == Firefox** kattintson ![Vamp UI - mentése](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png).

  Vamp hello feltételt a következő egy alapértelmezett erőssége 0 % hozzáadja. toostart forgalmának szűrésével, tooadjust hello feltétel erőssége kell.

3. Kattintson a ![Vamp UI - szerkesztése](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) toochange hello **erőssége** toohello feltétel alkalmazása.
 
4. Set hello **erőssége** too100 % kattintson ![Vamp UI - mentése](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png) toosave.

  Vamp most az összes forgalom megfelelő hello feltétel (minden felhasználó Firefox) toosava:1.0.0 küldi el.

  ![Felhasználói felület vamp - feltétel toogateway alkalmazása](./media/container-service-dcos-vamp-canary-release/26_apply_condition.png)

5. Végül módosítsa hello átjáró súly toosend összes többi forgalom (minden felhasználó nem Firefox) toohello új Száva: 1.1.0-ás. Kattintson a ![Vamp UI - szerkesztése](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) következő túl**súly** és állítsa be a hello súlyozások elosztása, így a 100 %-os irányított toohello útvonal sava/sava_cluster/sava:1.1.0/webport.

  Az összes forgalom nem szűrt hello feltétel már irányított toohello új Száva: 1.1.0-ás.

6. toosee hello szűrő műveletben, nyissa meg két különböző böngészők (egy Firefox és egy másik böngészőben), és elérni a hello Száva szolgáltatást is. Összes Firefox kérelem küldése toosava:1.0.0, míg minden más böngészők irányított toosava:1.1.0.

  ![Felhasználói felület - forgalom szűrésére vamp](./media/container-service-dcos-vamp-canary-release/27_filter_traffic.png)

## <a name="summing-up"></a>Összegzése

Ez a cikk a DC/OS-fürt gyors bevezetés tooVamp volt. Első Vamp portáltól és fut a az Azure tároló szolgáltatás DC/OS fürt, telepített egy szolgáltatást a egy Vamp tervezetének, címen érhető el: hello kitett végpont (átjáró).

Azt is az egyes hatékony szolgáltatásainak Vamp érint: egyesítése egy új szolgáltatás variant toohello-telepítést futtató és a bevezeti a Növekményesen, majd a forgalom tooresolve egy ismert kompatibilitási szűrés.


## <a name="next-steps"></a>Következő lépések

* Kezelésével kapcsolatos Vamp műveletek keresztül hello [Vamp a REST API-t](http://vamp.io/documentation/api/api-reference/).

* A node.js Vamp automatizálási parancsfájlokat létrehozása és futtatása azokat [munkafolyamatok Vamp](http://vamp.io/documentation/tutorials/create-a-workflow/).

* További részletek [VAMP oktatóanyagok](http://vamp.io/documentation/tutorials/overview/).

