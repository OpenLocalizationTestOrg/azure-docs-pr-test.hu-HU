---
title: "Azure App Service Web Apps Linux SSH támogatása |} Microsoft Docs"
description: "További tudnivalók az Azure Web Apps Linux SSH használatával."
keywords: "az Azure app service, webalkalmazás, linux, oss"
services: app-service
documentationcenter: 
author: wesmc7777
manager: erikre
editor: 
ms.assetid: 66f9988f-8ffa-414a-9137-3a9b15a5573c
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: wesmc
ms.openlocfilehash: feee7a5c91d213a6b0bfdaf264a4da4d9e79cbe7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="ssh-support-for-azure-web-app-on-linux"></a>Azure Web Apps Linux SSH támogatása

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="overview"></a>Áttekintés

[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) kriptográfiai hálózati protokoll a biztonságos hálózati szolgáltatások használatával. Leggyakrabban jelentkezzen be a rendszer távolról biztonságosan parancsot a parancssorból, és távolról felügyeleti parancsok végrehajtására szolgál.

Webes alkalmazás Linux SSH támogatja az egyes a beépített Docker lemezképeket az új webes alkalmazások futásidejű verem használható alkalmazás tárolóba. 

![Futásidejű verem](./media/app-service-linux-ssh-support/app-service-linux-runtime-stack.png)

Többek között az SSH-kiszolgálót a lemezkép részeként, és ebben a témakörben leírtak szerint konfigurálja úgy az egyéni Docker-lemezképekkel is használható SSH.



## <a name="making-a-client-connection"></a>Egy ügyfél-kapcsolatot

Ahhoz, hogy az SSH-ügyfél kapcsolat, a fő helye el kell indítani. 

A webalkalmazás a forrás vezérlő Management (SCM) végpont illessze be a böngészőt az alábbi paranccsal:

        https://<your sitename>.scm.azurewebsites.net/webssh/host

Ha Ön nem már hitelesítve, hitelesítik magukat az Azure-előfizetéshez való csatlakozáshoz szükségesek.

![SSH-kapcsolat](./media/app-service-linux-ssh-support/app-service-linux-ssh-connection.png)


## <a name="ssh-support-with-custom-docker-images"></a>Egyéni Docker-lemezképekkel SSH-támogatás

Ahhoz, hogy egy egyéni Docker lemezképeinek SSH-kommunikációhoz a tároló és az ügyfél között az Azure portálon hajtsa végre a következő lépéseket a Docker kép. 

Ezeket a lépéseket, amelyek megjelennek-e az Azure App Service-tárház példaként [Itt](https://github.com/Azure-App-Service/node/blob/master/6.9.3/).

1. Tartalmazza a `openssh-server` telepítés [ `RUN` utasítás](https://docs.docker.com/engine/reference/builder/#run) a lemezképet, és a beállított fiók jelszavát a legfelső szintű Dockerfile a `"Docker!"`. 

    > [!NOTE] 
    > Ez a konfiguráció nem teszi lehetővé a tároló külső kapcsolatot. SSH csak a Kudu keresztül érhető el vagy SCM helyhez, ami hitelesített közzétételi hitelesítő adatok használatával.

    ```docker
    # ------------------------
    # SSH Server support
    # ------------------------
    RUN apt-get update \ 
      && apt-get install -y --no-install-recommends openssh-server \
      && echo "root:Docker!" | chpasswd
    ``` 

2. Adja hozzá egy [ `COPY` utasítás](https://docs.docker.com/engine/reference/builder/#copy) történő másolásához Dockerfile egy [sshd_config](http://man.openbsd.org/sshd_config) fájlt a */stb/ssh/* könyvtár. A konfigurációs fájl alapján a sshd_config fájlt az Azure-alkalmazásszolgáltatási GitHub-tárházban [Itt](https://github.com/Azure-App-Service/node/blob/master/6.11/sshd_config).

    > [!NOTE] 
    > A *sshd_config* tartalmaznia kell a következő, vagy a kapcsolat hibája esetén: 
    > * `Ciphers`tartalmaznia kell legalább a következők egyikét: `aes128-cbc,3des-cbc,aes256-cbc`.
    > * `MACs`tartalmaznia kell legalább a következők egyikét: `hmac-sha1,hmac-sha1-96`.

    ```docker
    COPY sshd_config /etc/ssh/
    ```


3. Port 2222 közé tartozik a [ `EXPOSE` utasítás](https://docs.docker.com/engine/reference/builder/#expose) a Dockerfile számára. Bár a gyökér szintű jelszó is ismert, port 2222 nem érhető el az internetről. Egy belső egyetlen port elérhető csak által a virtuális magánhálózat híd hálózati tárolókra is.

    ```docker
    EXPOSE 2222 80
    ```

4. Ügyeljen arra, hogy indítsa el az ssh szolgáltatást. A példa [Itt](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh) használja a héjparancsfájlt */bin* könyvtár.

    ```bash
    #!/bin/bash
    service ssh start
    ```

    A Dockerfile használja a [ `CMD` utasítás](https://docs.docker.com/engine/reference/builder/#cmd) a parancsfájl futtatásához.

    ```docker
    COPY init_container.sh /bin/
      ...
    RUN chmod 755 /bin/init_container.sh 
      ...       
    CMD ["/bin/init_container.sh"]
    ```



## <a name="next-steps"></a>Következő lépések
Az alábbi hivatkozások webalkalmazás vonatkozó további információért tekintse meg Linux. Kérdések és problémákat is közzétesz a [fórumban](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Egyéni Docker-lemezkép használata Linux Azure webalkalmazás számára](app-service-linux-using-custom-docker-image.md)
* [PM2 Configuration for Node.js Linux Azure Web App használatával](app-service-linux-using-nodejs-pm2.md)
* [.NET Core Linux Azure Web App használatával](app-service-linux-using-dotnetcore.md)
* [Ruby Linux Azure Web App használatával](app-service-linux-ruby-get-started.md)
* [Az Azure App Service webalkalmazásba Linux – gyakori kérdések](app-service-linux-faq.md)

