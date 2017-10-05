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
# <a name="ssh-support-for-azure-web-app-on-linux"></a><span data-ttu-id="cdd66-104">Azure Web Apps Linux SSH támogatása</span><span class="sxs-lookup"><span data-stu-id="cdd66-104">SSH support for Azure Web App on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="overview"></a><span data-ttu-id="cdd66-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="cdd66-105">Overview</span></span>

<span data-ttu-id="cdd66-106">[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) kriptográfiai hálózati protokoll a biztonságos hálózati szolgáltatások használatával.</span><span class="sxs-lookup"><span data-stu-id="cdd66-106">[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) is a cryptographic network protocol for using network services securely.</span></span> <span data-ttu-id="cdd66-107">Leggyakrabban jelentkezzen be a rendszer távolról biztonságosan parancsot a parancssorból, és távolról felügyeleti parancsok végrehajtására szolgál.</span><span class="sxs-lookup"><span data-stu-id="cdd66-107">It is most commonly used to log into a system remotely securely from a command-line and execute administrative commands remotely.</span></span>

<span data-ttu-id="cdd66-108">Webes alkalmazás Linux SSH támogatja az egyes a beépített Docker lemezképeket az új webes alkalmazások futásidejű verem használható alkalmazás tárolóba.</span><span class="sxs-lookup"><span data-stu-id="cdd66-108">Web App on Linux provides SSH support into the app container with each of the built-in Docker images used for the Runtime Stack of new web apps.</span></span> 

![Futásidejű verem](./media/app-service-linux-ssh-support/app-service-linux-runtime-stack.png)

<span data-ttu-id="cdd66-110">Többek között az SSH-kiszolgálót a lemezkép részeként, és ebben a témakörben leírtak szerint konfigurálja úgy az egyéni Docker-lemezképekkel is használható SSH.</span><span class="sxs-lookup"><span data-stu-id="cdd66-110">You can also use SSH with your custom Docker images by including the SSH server as part of the image and configuring it as described in this topic.</span></span>



## <a name="making-a-client-connection"></a><span data-ttu-id="cdd66-111">Egy ügyfél-kapcsolatot</span><span class="sxs-lookup"><span data-stu-id="cdd66-111">Making a client connection</span></span>

<span data-ttu-id="cdd66-112">Ahhoz, hogy az SSH-ügyfél kapcsolat, a fő helye el kell indítani.</span><span class="sxs-lookup"><span data-stu-id="cdd66-112">To make an SSH client connection, the main site must be started.</span></span> 

<span data-ttu-id="cdd66-113">A webalkalmazás a forrás vezérlő Management (SCM) végpont illessze be a böngészőt az alábbi paranccsal:</span><span class="sxs-lookup"><span data-stu-id="cdd66-113">Paste the Source Control Management (SCM) endpoint for your web app into your browser using the following form:</span></span>

        https://<your sitename>.scm.azurewebsites.net/webssh/host

<span data-ttu-id="cdd66-114">Ha Ön nem már hitelesítve, hitelesítik magukat az Azure-előfizetéshez való csatlakozáshoz szükségesek.</span><span class="sxs-lookup"><span data-stu-id="cdd66-114">If you are not already authenticated, you are required to authenticate with your Azure subscription to connect.</span></span>

![SSH-kapcsolat](./media/app-service-linux-ssh-support/app-service-linux-ssh-connection.png)


## <a name="ssh-support-with-custom-docker-images"></a><span data-ttu-id="cdd66-116">Egyéni Docker-lemezképekkel SSH-támogatás</span><span class="sxs-lookup"><span data-stu-id="cdd66-116">SSH support with custom Docker images</span></span>

<span data-ttu-id="cdd66-117">Ahhoz, hogy egy egyéni Docker lemezképeinek SSH-kommunikációhoz a tároló és az ügyfél között az Azure portálon hajtsa végre a következő lépéseket a Docker kép.</span><span class="sxs-lookup"><span data-stu-id="cdd66-117">In order for a custom Docker image to support SSH communication between the container and the client in the Azure portal, perform the following steps for your Docker image.</span></span> 

<span data-ttu-id="cdd66-118">Ezeket a lépéseket, amelyek megjelennek-e az Azure App Service-tárház példaként [Itt](https://github.com/Azure-App-Service/node/blob/master/6.9.3/).</span><span class="sxs-lookup"><span data-stu-id="cdd66-118">These steps are are shown in the Azure App Service repository as an example [here](https://github.com/Azure-App-Service/node/blob/master/6.9.3/).</span></span>

1. <span data-ttu-id="cdd66-119">Tartalmazza a `openssh-server` telepítés [ `RUN` utasítás](https://docs.docker.com/engine/reference/builder/#run) a lemezképet, és a beállított fiók jelszavát a legfelső szintű Dockerfile a `"Docker!"`.</span><span class="sxs-lookup"><span data-stu-id="cdd66-119">Include the `openssh-server` installation in [`RUN` instruction](https://docs.docker.com/engine/reference/builder/#run) in the Dockerfile for your image and set the password for the root account to `"Docker!"`.</span></span> 

    > [!NOTE] 
    > <span data-ttu-id="cdd66-120">Ez a konfiguráció nem teszi lehetővé a tároló külső kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="cdd66-120">This configuration does not allow external connections to the container.</span></span> <span data-ttu-id="cdd66-121">SSH csak a Kudu keresztül érhető el vagy SCM helyhez, ami hitelesített közzétételi hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="cdd66-121">SSH can only be accessed via the Kudu / SCM Site, which is authenticated using the publishing credentials.</span></span>

    ```docker
    # ------------------------
    # SSH Server support
    # ------------------------
    RUN apt-get update \ 
      && apt-get install -y --no-install-recommends openssh-server \
      && echo "root:Docker!" | chpasswd
    ``` 

2. <span data-ttu-id="cdd66-122">Adja hozzá egy [ `COPY` utasítás](https://docs.docker.com/engine/reference/builder/#copy) történő másolásához Dockerfile egy [sshd_config](http://man.openbsd.org/sshd_config) fájlt a */stb/ssh/* könyvtár.</span><span class="sxs-lookup"><span data-stu-id="cdd66-122">Add a [`COPY` instruction](https://docs.docker.com/engine/reference/builder/#copy) to the Dockerfile to copy a [sshd_config](http://man.openbsd.org/sshd_config) file to the */etc/ssh/* directory.</span></span> <span data-ttu-id="cdd66-123">A konfigurációs fájl alapján a sshd_config fájlt az Azure-alkalmazásszolgáltatási GitHub-tárházban [Itt](https://github.com/Azure-App-Service/node/blob/master/6.11/sshd_config).</span><span class="sxs-lookup"><span data-stu-id="cdd66-123">Your configuration file should be based on our sshd_config file in the Azure-App-Service GitHub repository [here](https://github.com/Azure-App-Service/node/blob/master/6.11/sshd_config).</span></span>

    > [!NOTE] 
    > <span data-ttu-id="cdd66-124">A *sshd_config* tartalmaznia kell a következő, vagy a kapcsolat hibája esetén:</span><span class="sxs-lookup"><span data-stu-id="cdd66-124">The *sshd_config* file must include the following or the connection fails:</span></span> 
    > * <span data-ttu-id="cdd66-125">`Ciphers`tartalmaznia kell legalább a következők egyikét: `aes128-cbc,3des-cbc,aes256-cbc`.</span><span class="sxs-lookup"><span data-stu-id="cdd66-125">`Ciphers` must include at least one of the following: `aes128-cbc,3des-cbc,aes256-cbc`.</span></span>
    > * <span data-ttu-id="cdd66-126">`MACs`tartalmaznia kell legalább a következők egyikét: `hmac-sha1,hmac-sha1-96`.</span><span class="sxs-lookup"><span data-stu-id="cdd66-126">`MACs` must include at least one of the following: `hmac-sha1,hmac-sha1-96`.</span></span>

    ```docker
    COPY sshd_config /etc/ssh/
    ```


3. <span data-ttu-id="cdd66-127">Port 2222 közé tartozik a [ `EXPOSE` utasítás](https://docs.docker.com/engine/reference/builder/#expose) a Dockerfile számára.</span><span class="sxs-lookup"><span data-stu-id="cdd66-127">Include port 2222 in the [`EXPOSE` instruction](https://docs.docker.com/engine/reference/builder/#expose) for the Dockerfile.</span></span> <span data-ttu-id="cdd66-128">Bár a gyökér szintű jelszó is ismert, port 2222 nem érhető el az internetről.</span><span class="sxs-lookup"><span data-stu-id="cdd66-128">Although the root password is known, port 2222 cannot be accessed from the internet.</span></span> <span data-ttu-id="cdd66-129">Egy belső egyetlen port elérhető csak által a virtuális magánhálózat híd hálózati tárolókra is.</span><span class="sxs-lookup"><span data-stu-id="cdd66-129">It is an internal only port accessible only by containers within the bridge network of a private virtual network.</span></span>

    ```docker
    EXPOSE 2222 80
    ```

4. <span data-ttu-id="cdd66-130">Ügyeljen arra, hogy indítsa el az ssh szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="cdd66-130">Make sure to start the ssh service.</span></span> <span data-ttu-id="cdd66-131">A példa [Itt](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh) használja a héjparancsfájlt */bin* könyvtár.</span><span class="sxs-lookup"><span data-stu-id="cdd66-131">The example [here](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh) uses a shell script in */bin* directory.</span></span>

    ```bash
    #!/bin/bash
    service ssh start
    ```

    <span data-ttu-id="cdd66-132">A Dockerfile használja a [ `CMD` utasítás](https://docs.docker.com/engine/reference/builder/#cmd) a parancsfájl futtatásához.</span><span class="sxs-lookup"><span data-stu-id="cdd66-132">The Dockerfile uses the [`CMD` instruction](https://docs.docker.com/engine/reference/builder/#cmd) to run the script.</span></span>

    ```docker
    COPY init_container.sh /bin/
      ...
    RUN chmod 755 /bin/init_container.sh 
      ...       
    CMD ["/bin/init_container.sh"]
    ```



## <a name="next-steps"></a><span data-ttu-id="cdd66-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cdd66-133">Next steps</span></span>
<span data-ttu-id="cdd66-134">Az alábbi hivatkozások webalkalmazás vonatkozó további információért tekintse meg Linux.</span><span class="sxs-lookup"><span data-stu-id="cdd66-134">See the following links for more information regarding Web App on Linux.</span></span> <span data-ttu-id="cdd66-135">Kérdések és problémákat is közzétesz a [fórumban](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span><span class="sxs-lookup"><span data-stu-id="cdd66-135">You can post questions and concerns on [our forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span></span>

* [<span data-ttu-id="cdd66-136">Egyéni Docker-lemezkép használata Linux Azure webalkalmazás számára</span><span class="sxs-lookup"><span data-stu-id="cdd66-136">How to use a custom Docker image for Azure Web App on Linux</span></span>](app-service-linux-using-custom-docker-image.md)
* [<span data-ttu-id="cdd66-137">PM2 Configuration for Node.js Linux Azure Web App használatával</span><span class="sxs-lookup"><span data-stu-id="cdd66-137">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="cdd66-138">.NET Core Linux Azure Web App használatával</span><span class="sxs-lookup"><span data-stu-id="cdd66-138">Using .NET Core in Azure Web App on Linux</span></span>](app-service-linux-using-dotnetcore.md)
* [<span data-ttu-id="cdd66-139">Ruby Linux Azure Web App használatával</span><span class="sxs-lookup"><span data-stu-id="cdd66-139">Using Ruby in Azure Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)
* [<span data-ttu-id="cdd66-140">Az Azure App Service webalkalmazásba Linux – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="cdd66-140">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)

