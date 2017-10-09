---
title: "Azure DC/OS-fürtöt aaaManage Marathon felhasználói felület |} Microsoft Docs"
description: "Tárolók tooan Azure tárolószolgáltatásba hello Marathon webes felhasználói felület segítségével telepítheti."
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, mikroszolgáltatások, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/04/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: a90180e1b4763e6d2ddfa699ed4b7269f209f728
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-container-service-dcos-cluster-through-hello-marathon-web-ui"></a>Egy Azure tároló szolgáltatás DC/OS-fürt hello Marathon webes felhasználói felületen keresztül kezeléséhez
A DC/OS telepítését és skálázását lehetővé fürtözött munkaterhelések, absztrakt módon megjelenítve a mögöttes hardver hello közben környezetet biztosít. A DC/OS fölötti keretrendszer gondoskodik a számítási feladatok ütemezéséről és végrehajtásáról.

Számos népszerű számítási elérhetők keretrendszerek, ez a dokumentum ismerteti, hogyan tooget el őket a marathon segítségével tárolók telepítése. 


## <a name="prerequisites"></a>Előfeltételek
A példákban szereplő feladatok elvégzéséhez szüksége lesz egy az Azure tárolószolgáltatásban konfigurált DC/OS-fürtre, Meg kell toohave távoli kapcsolatot toothis fürtöt is. További információ ezekről az elemekről tekintse meg a következő cikkek hello:

* [Az Azure Container Service-fürt üzembe helyezése](container-service-deployment.md)
* [Csatlakozás Azure Tárolószolgáltatás-fürt tooan](../container-service-connect.md)

> [!NOTE]
> Ez a cikk feltételezi, hogy az alagutat toohello DC/OS-fürtről a helyi 80-as porton keresztül.
>

## <a name="explore-hello-dcos-ui"></a>DC/OS felhasználói felületének hello felfedezés
Az egy Secure Shell (SSH) alagút [létrehozott](../container-service-connect.md), keresse meg a toohttp://localhost/. Ez betölti a hello DC/OS webes felhasználói felület, és hello fürt, például a felhasznált erőforrásokat, aktív ügynököket és futó szolgáltatások információkat jeleníti meg.

![A DC/OS UI felhasználói felülete](./media/container-service-mesos-marathon-ui/dcos2.png)

## <a name="explore-hello-marathon-ui"></a>Marathon felhasználói felület hello felfedezés
Marathon felhasználói felület toosee hello toohttp://localhost/marathon Tallózás. Ezen a képernyőn elindíthatja egy új tároló vagy egy másik alkalmazás hello Azure tároló szolgáltatás DC/OS-fürtön. A futó tárolókkal és alkalmazásokkal kapcsolatos információkat is láthat.  

![Marathon felhasználói felület](./media/container-service-mesos-marathon-ui/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a>Docker-formázású tároló üzembe helyezése
a Marathon, egy új tároló toodeploy kattintson **alkalmazás létrehozása**, és írja be a következő információ be hello űrlap lapok hello:

| Mező | Érték |
| --- | --- |
| ID (Azonosító) |nginx |
| Memory (Memória) | 32 |
| Image (Kép) |nginx |
| Network (Hálózat) |Bridged |
| Host Port (Gazdaport) |80 |
| Protocol (Protokoll) |TCP |

![New Application (Új alkalmazás) felhasználói felület – General (Általános)](./media/container-service-mesos-marathon-ui/dcos4.png)

![New Application (Új alkalmazás) felhasználói felület – Docker Container (Docker-tároló)](./media/container-service-mesos-marathon-ui/dcos5.png)

![New Application (Új alkalmazás) felhasználói felület – Ports and Service Discovery (Portok és szolgáltatások felderítése)](./media/container-service-mesos-marathon-ui/dcos6.png)

Ha azt szeretné, hogy toostatically térkép hello tárolóportot tooa port hello ügynökön, toouse JSON üzemmódot kell. toodo tehát az túl hello új alkalmazás varázsló kapcsoló**JSON üzemmódot** hello váltása használatával. Írja be a következő hello beállítás hello `portMappings` hello definíciót szakasza. Ebben a példában a hello tároló tooport 80 hello DC/OS-ügynök 80-as porton van kötve. Ennek a módosításnak az elvégzése után visszaválthat a varázsló JSON üzemmódjából.

```none
"hostPort": 80,
```

![New Application (Új alkalmazás) felhasználói felület – példa a 80-as portra](./media/container-service-mesos-marathon-ui/dcos13.png)

Ha azt szeretné, hogy tooenable állapotellenőrzést, egy elérési utat meg hello **ellenőrzést** lapon.

![New Application (Új alkalmazás) felhasználói felület – állapot-ellenőrzések](./media/container-service-mesos-marathon-ui/dcos_healthcheck.png)

hello DC/OS-fürtről van telepítve, privát és nyilvános ügynökök vannak beállítva. A hello fürt toobe képes tooaccess alkalmazások hello Internet toodeploy hello alkalmazások tooa nyilvános ügynökre kell. toodo Igen, válassza ki a hello **nem kötelező** hello új alkalmazás varázsló lapja, és írja be **slave_public** a hello **elfogadott erőforrás szerepkörök**.

Ezután kattintson a **Create Application** (Alkalmazás létrehozása) elemre.

![New Application (Új alkalmazás) felhasználói felület – nyilvános ügynök beállítása](./media/container-service-mesos-marathon-ui/dcos14.png)

Vissza a hello Marathon főoldalára visszatérve hello tároló hello telepítési állapota látható. Kezdetben a **Deploying** (Üzembe helyezés) állapotot fogja látni. A sikeres telepítést követően hello állapotmódosítások túl**futtató**.

![A Marathon főoldala – a tároló üzembe helyezési állapota](./media/container-service-mesos-marathon-ui/dcos7.png)

Amikor hátsó toohello DC/OS webes felhasználói Felületére (http://localhost/), láthatja, hogy hello DC/OS fürtben fut egy feladat (ebben az esetben egy Docker-formázású tároló).

![A DC/OS webes felhasználói felülete – hello fürtben futó feladat](./media/container-service-mesos-marathon-ui/dcos8.png)

toosee hello fürtcsomóponton, amely hello feladat fut, kattintson a hello **csomópontok** fülre.

![A DC/OS webes felhasználói felülete – a feladat fürtcsomópontja](./media/container-service-mesos-marathon-ui/dcos9.png)

## <a name="reach-hello-container"></a>Hello tároló elérése

Ebben a példában hello alkalmazás fut egy nyilvános ügynök csomóponton. Hello hello alkalmazás eléri toohello ügynök hello fürt tallózással internet: `http://[DNSPREFIX]agents.[REGION].cloudapp.azure.com`, ahol:

* **DNSPREFIX** hello hello fürt telepítésekor megadott DNS-előtag van.
* **A régióban** hello régió, ahol az erőforráscsoport megtalálható.

    ![Internetről érkező Nginx](./media/container-service-mesos-marathon-ui/nginx.png)


## <a name="next-steps"></a>Következő lépések
* [A DC/OS használata és hello Marathon API](container-service-mesos-marathon-rest.md)

* Részletes bemutatója a hello Azure Tárolószolgáltatás a Mesos

    > [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON203/player]
    > 
