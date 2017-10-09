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
# <a name="ssh-support-for-azure-web-app-on-linux"></a><span data-ttu-id="dfa14-104">Azure Web Apps Linux SSH támogatása</span><span class="sxs-lookup"><span data-stu-id="dfa14-104">SSH support for Azure Web App on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="overview"></a><span data-ttu-id="dfa14-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="dfa14-105">Overview</span></span>

<span data-ttu-id="dfa14-106">[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) kriptográfiai hálózati protokoll a biztonságos hálózati szolgáltatások használatával.</span><span class="sxs-lookup"><span data-stu-id="dfa14-106">[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) is a cryptographic network protocol for using network services securely.</span></span> <span data-ttu-id="dfa14-107">Rendszerben a leggyakrabban használt toolog távolról biztonságosan parancsot a parancssorból, és távolról felügyeleti parancsokat hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="dfa14-107">It is most commonly used toolog into a system remotely securely from a command-line and execute administrative commands remotely.</span></span>

<span data-ttu-id="dfa14-108">Webes alkalmazás Linux SSH támogatja az egyes hello használt hello futásidejű vermet az új webes alkalmazások beépített Docker képek hello app tárolóba.</span><span class="sxs-lookup"><span data-stu-id="dfa14-108">Web App on Linux provides SSH support into hello app container with each of hello built-in Docker images used for hello Runtime Stack of new web apps.</span></span> 

![Futásidejű verem](./media/app-service-linux-ssh-support/app-service-linux-runtime-stack.png)

<span data-ttu-id="dfa14-110">Is használata SSH az egyéni lemezképek Docker hello SSH-kiszolgálót például hello lemezkép részeként és konfigurálása ebben a témakörben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="dfa14-110">You can also use SSH with your custom Docker images by including hello SSH server as part of hello image and configuring it as described in this topic.</span></span>



## <a name="making-a-client-connection"></a><span data-ttu-id="dfa14-111">Egy ügyfél-kapcsolatot</span><span class="sxs-lookup"><span data-stu-id="dfa14-111">Making a client connection</span></span>

<span data-ttu-id="dfa14-112">az SSH ügyfél-kapcsolat, hello fő helye toomake el kell indítani.</span><span class="sxs-lookup"><span data-stu-id="dfa14-112">toomake an SSH client connection, hello main site must be started.</span></span> 

<span data-ttu-id="dfa14-113">Hello forrás vezérlő Management (SCM) végpont a webalkalmazás illessze be a böngészőt, a következő képernyő hello használata:</span><span class="sxs-lookup"><span data-stu-id="dfa14-113">Paste hello Source Control Management (SCM) endpoint for your web app into your browser using hello following form:</span></span>

        https://<your sitename>.scm.azurewebsites.net/webssh/host

<span data-ttu-id="dfa14-114">Ha Ön nem már hitelesítve, és az Azure-előfizetés tooconnect áll szükséges tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="dfa14-114">If you are not already authenticated, you are required tooauthenticate with your Azure subscription tooconnect.</span></span>

![SSH-kapcsolat](./media/app-service-linux-ssh-support/app-service-linux-ssh-connection.png)


## <a name="ssh-support-with-custom-docker-images"></a><span data-ttu-id="dfa14-116">Egyéni Docker-lemezképekkel SSH-támogatás</span><span class="sxs-lookup"><span data-stu-id="dfa14-116">SSH support with custom Docker images</span></span>

<span data-ttu-id="dfa14-117">Ahhoz, hogy egy egyéni Docker kép toosupport SSH hello tároló és ügyfél közötti kommunikációhoz hello a hello Azure-portálon hajtsa végre a következő lépéseket a Docker kép hello.</span><span class="sxs-lookup"><span data-stu-id="dfa14-117">In order for a custom Docker image toosupport SSH communication between hello container and hello client in hello Azure portal, perform hello following steps for your Docker image.</span></span> 

<span data-ttu-id="dfa14-118">Ezeket a lépéseket, amelyek megjelennek-e hello Azure App Service-tárház példaként [Itt](https://github.com/Azure-App-Service/node/blob/master/6.9.3/).</span><span class="sxs-lookup"><span data-stu-id="dfa14-118">These steps are are shown in hello Azure App Service repository as an example [here](https://github.com/Azure-App-Service/node/blob/master/6.9.3/).</span></span>

1. <span data-ttu-id="dfa14-119">Hello tartalmaznak `openssh-server` telepítés [ `RUN` utasítás](https://docs.docker.com/engine/reference/builder/#run) a Dockerfile hello a lemezképet, és állítsa be a hello hello root fiókjának jelszava túl`"Docker!"`.</span><span class="sxs-lookup"><span data-stu-id="dfa14-119">Include hello `openssh-server` installation in [`RUN` instruction](https://docs.docker.com/engine/reference/builder/#run) in hello Dockerfile for your image and set hello password for hello root account too`"Docker!"`.</span></span> 

    > [!NOTE] 
    > <span data-ttu-id="dfa14-120">Ez a konfiguráció nem teszi lehetővé a külső kapcsolatok toohello tároló.</span><span class="sxs-lookup"><span data-stu-id="dfa14-120">This configuration does not allow external connections toohello container.</span></span> <span data-ttu-id="dfa14-121">SSH csak hello Kudu keresztül érhető el / SCM helyre, amely segítségével hitelesített hello közzétételi hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="dfa14-121">SSH can only be accessed via hello Kudu / SCM Site, which is authenticated using hello publishing credentials.</span></span>

    ```docker
    # ------------------------
    # SSH Server support
    # ------------------------
    RUN apt-get update \ 
      && apt-get install -y --no-install-recommends openssh-server \
      && echo "root:Docker!" | chpasswd
    ``` 

2. <span data-ttu-id="dfa14-122">Adja hozzá a [ `COPY` utasítás](https://docs.docker.com/engine/reference/builder/#copy) toohello Dockerfile toocopy egy [sshd_config](http://man.openbsd.org/sshd_config) toohello fájl */stb/ssh/* könyvtár.</span><span class="sxs-lookup"><span data-stu-id="dfa14-122">Add a [`COPY` instruction](https://docs.docker.com/engine/reference/builder/#copy) toohello Dockerfile toocopy a [sshd_config](http://man.openbsd.org/sshd_config) file toohello */etc/ssh/* directory.</span></span> <span data-ttu-id="dfa14-123">A konfigurációs fájl alapján a sshd_config fájl hello Azure alkalmazásszolgáltatási GitHub-tárházban [Itt](https://github.com/Azure-App-Service/node/blob/master/6.11/sshd_config).</span><span class="sxs-lookup"><span data-stu-id="dfa14-123">Your configuration file should be based on our sshd_config file in hello Azure-App-Service GitHub repository [here](https://github.com/Azure-App-Service/node/blob/master/6.11/sshd_config).</span></span>

    > [!NOTE] 
    > <span data-ttu-id="dfa14-124">Hello *sshd_config* tartalmaznia kell következő hello vagy hello való kapcsolódás sikertelen:</span><span class="sxs-lookup"><span data-stu-id="dfa14-124">hello *sshd_config* file must include hello following or hello connection fails:</span></span> 
    > * <span data-ttu-id="dfa14-125">`Ciphers`tartalmaznia kell legalább egy hello alábbi: `aes128-cbc,3des-cbc,aes256-cbc`.</span><span class="sxs-lookup"><span data-stu-id="dfa14-125">`Ciphers` must include at least one of hello following: `aes128-cbc,3des-cbc,aes256-cbc`.</span></span>
    > * <span data-ttu-id="dfa14-126">`MACs`tartalmaznia kell legalább egy hello alábbi: `hmac-sha1,hmac-sha1-96`.</span><span class="sxs-lookup"><span data-stu-id="dfa14-126">`MACs` must include at least one of hello following: `hmac-sha1,hmac-sha1-96`.</span></span>

    ```docker
    COPY sshd_config /etc/ssh/
    ```


3. <span data-ttu-id="dfa14-127">Port 2222 foglalandó hello [ `EXPOSE` utasítás](https://docs.docker.com/engine/reference/builder/#expose) hello Dockerfile számára.</span><span class="sxs-lookup"><span data-stu-id="dfa14-127">Include port 2222 in hello [`EXPOSE` instruction](https://docs.docker.com/engine/reference/builder/#expose) for hello Dockerfile.</span></span> <span data-ttu-id="dfa14-128">Bár a hello gyökér szintű jelszó is ismert, port 2222 nem érhető el a hello internet.</span><span class="sxs-lookup"><span data-stu-id="dfa14-128">Although hello root password is known, port 2222 cannot be accessed from hello internet.</span></span> <span data-ttu-id="dfa14-129">Egy belső egyetlen port elérhető csak által a virtuális magánhálózatok hello híd hálózati tárolókra is.</span><span class="sxs-lookup"><span data-stu-id="dfa14-129">It is an internal only port accessible only by containers within hello bridge network of a private virtual network.</span></span>

    ```docker
    EXPOSE 2222 80
    ```

4. <span data-ttu-id="dfa14-130">Ellenőrizze, hogy toostart hello ssh szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="dfa14-130">Make sure toostart hello ssh service.</span></span> <span data-ttu-id="dfa14-131">hello példa [Itt](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh) használja a héjparancsfájlt */bin* könyvtár.</span><span class="sxs-lookup"><span data-stu-id="dfa14-131">hello example [here](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh) uses a shell script in */bin* directory.</span></span>

    ```bash
    #!/bin/bash
    service ssh start
    ```

    <span data-ttu-id="dfa14-132">hello Dockerfile használ hello [ `CMD` utasítás](https://docs.docker.com/engine/reference/builder/#cmd) toorun hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="dfa14-132">hello Dockerfile uses hello [`CMD` instruction](https://docs.docker.com/engine/reference/builder/#cmd) toorun hello script.</span></span>

    ```docker
    COPY init_container.sh /bin/
      ...
    RUN chmod 755 /bin/init_container.sh 
      ...       
    CMD ["/bin/init_container.sh"]
    ```



## <a name="next-steps"></a><span data-ttu-id="dfa14-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dfa14-133">Next steps</span></span>
<span data-ttu-id="dfa14-134">Tekintse meg a következő hivatkozások webalkalmazás kapcsolatos további tudnivalókért Linux hello.</span><span class="sxs-lookup"><span data-stu-id="dfa14-134">See hello following links for more information regarding Web App on Linux.</span></span> <span data-ttu-id="dfa14-135">Kérdések és problémákat is közzétesz a [fórumban](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span><span class="sxs-lookup"><span data-stu-id="dfa14-135">You can post questions and concerns on [our forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span></span>

* [<span data-ttu-id="dfa14-136">Hogyan toouse egyéni Docker kép Linux Azure webalkalmazás számára</span><span class="sxs-lookup"><span data-stu-id="dfa14-136">How toouse a custom Docker image for Azure Web App on Linux</span></span>](app-service-linux-using-custom-docker-image.md)
* [<span data-ttu-id="dfa14-137">PM2 Configuration for Node.js Linux Azure Web App használatával</span><span class="sxs-lookup"><span data-stu-id="dfa14-137">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="dfa14-138">.NET Core Linux Azure Web App használatával</span><span class="sxs-lookup"><span data-stu-id="dfa14-138">Using .NET Core in Azure Web App on Linux</span></span>](app-service-linux-using-dotnetcore.md)
* [<span data-ttu-id="dfa14-139">Ruby Linux Azure Web App használatával</span><span class="sxs-lookup"><span data-stu-id="dfa14-139">Using Ruby in Azure Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)
* [<span data-ttu-id="dfa14-140">Az Azure App Service webalkalmazásba Linux – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="dfa14-140">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)

