---
title: "Azure App Service Web Apps Linux támogatása aaaSSH |} Microsoft Docs"
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
ms.openlocfilehash: e00be6d4631e8936a2a8bc106da1fc06237a4b39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="ssh-support-for-azure-web-app-on-linux"></a>Azure Web Apps Linux SSH támogatása

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="overview"></a>Áttekintés

[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) kriptográfiai hálózati protokoll a biztonságos hálózati szolgáltatások használatával. Rendszerben a leggyakrabban használt toolog távolról biztonságosan parancsot a parancssorból, és távolról felügyeleti parancsokat hajthat végre.

Webes alkalmazás Linux SSH támogatja az egyes hello használt hello futásidejű vermet az új webes alkalmazások beépített Docker képek hello app tárolóba. 

![Futásidejű verem](./media/app-service-linux-ssh-support/app-service-linux-runtime-stack.png)

Is használata SSH az egyéni lemezképek Docker hello SSH-kiszolgálót például hello lemezkép részeként és konfigurálása ebben a témakörben leírtak szerint.



## <a name="making-a-client-connection"></a>Egy ügyfél-kapcsolatot

az SSH ügyfél-kapcsolat, hello fő helye toomake el kell indítani. 

Hello forrás vezérlő Management (SCM) végpont a webalkalmazás illessze be a böngészőt, a következő képernyő hello használata:

        https://<your sitename>.scm.azurewebsites.net/webssh/host

Ha Ön nem már hitelesítve, és az Azure-előfizetés tooconnect áll szükséges tooauthenticate.

![SSH-kapcsolat](./media/app-service-linux-ssh-support/app-service-linux-ssh-connection.png)


## <a name="ssh-support-with-custom-docker-images"></a>Egyéni Docker-lemezképekkel SSH-támogatás

Ahhoz, hogy egy egyéni Docker kép toosupport SSH hello tároló és ügyfél közötti kommunikációhoz hello a hello Azure-portálon hajtsa végre a következő lépéseket a Docker kép hello. 

Ezeket a lépéseket, amelyek megjelennek-e hello Azure App Service-tárház példaként [Itt](https://github.com/Azure-App-Service/node/blob/master/6.9.3/).

1. Hello tartalmaznak `openssh-server` telepítés [ `RUN` utasítás](https://docs.docker.com/engine/reference/builder/#run) a Dockerfile hello a lemezképet, és állítsa be a hello hello root fiókjának jelszava túl`"Docker!"`. 

    > [!NOTE] 
    > Ez a konfiguráció nem teszi lehetővé a külső kapcsolatok toohello tároló. SSH csak hello Kudu keresztül érhető el / SCM helyre, amely segítségével hitelesített hello közzétételi hitelesítő adatokat.

    ```docker
    # ------------------------
    # SSH Server support
    # ------------------------
    RUN apt-get update \ 
      && apt-get install -y --no-install-recommends openssh-server \
      && echo "root:Docker!" | chpasswd
    ``` 

2. Adja hozzá a [ `COPY` utasítás](https://docs.docker.com/engine/reference/builder/#copy) toohello Dockerfile toocopy egy [sshd_config](http://man.openbsd.org/sshd_config) toohello fájl */stb/ssh/* könyvtár. A konfigurációs fájl alapján a sshd_config fájl hello Azure alkalmazásszolgáltatási GitHub-tárházban [Itt](https://github.com/Azure-App-Service/node/blob/master/6.11/sshd_config).

    > [!NOTE] 
    > Hello *sshd_config* tartalmaznia kell következő hello vagy hello való kapcsolódás sikertelen: 
    > * `Ciphers`tartalmaznia kell legalább egy hello alábbi: `aes128-cbc,3des-cbc,aes256-cbc`.
    > * `MACs`tartalmaznia kell legalább egy hello alábbi: `hmac-sha1,hmac-sha1-96`.

    ```docker
    COPY sshd_config /etc/ssh/
    ```


3. Port 2222 foglalandó hello [ `EXPOSE` utasítás](https://docs.docker.com/engine/reference/builder/#expose) hello Dockerfile számára. Bár a hello gyökér szintű jelszó is ismert, port 2222 nem érhető el a hello internet. Egy belső egyetlen port elérhető csak által a virtuális magánhálózatok hello híd hálózati tárolókra is.

    ```docker
    EXPOSE 2222 80
    ```

4. Ellenőrizze, hogy toostart hello ssh szolgáltatást. hello példa [Itt](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh) használja a héjparancsfájlt */bin* könyvtár.

    ```bash
    #!/bin/bash
    service ssh start
    ```

    hello Dockerfile használ hello [ `CMD` utasítás](https://docs.docker.com/engine/reference/builder/#cmd) toorun hello parancsfájl.

    ```docker
    COPY init_container.sh /bin/
      ...
    RUN chmod 755 /bin/init_container.sh 
      ...       
    CMD ["/bin/init_container.sh"]
    ```



## <a name="next-steps"></a>Következő lépések
Tekintse meg a következő hivatkozások webalkalmazás kapcsolatos további tudnivalókért Linux hello. Kérdések és problémákat is közzétesz a [fórumban](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Hogyan toouse egyéni Docker kép Linux Azure webalkalmazás számára](app-service-linux-using-custom-docker-image.md)
* [PM2 Configuration for Node.js Linux Azure Web App használatával](app-service-linux-using-nodejs-pm2.md)
* [.NET Core Linux Azure Web App használatával](app-service-linux-using-dotnetcore.md)
* [Ruby Linux Azure Web App használatával](app-service-linux-ruby-get-started.md)
* [Az Azure App Service webalkalmazásba Linux – gyakori kérdések](app-service-linux-faq.md)

