---
title: "Az intellij-t az Azure-eszközkészlet használatával egy Docker-tároló közzététele |} Microsoft Docs"
description: "Útmutató a webes alkalmazás a Microsoft Azure is közzé egy Docker-tároló az intellij-t az Azure-eszközkészlet használatával."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 96680319a6c4c0f0a4673cd6303a5b172f428797
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-the-azure-toolkit-for-intellij"></a><span data-ttu-id="72f72-103">A webes alkalmazás is egy Docker-tároló közzé a intellij-t az Azure-eszközkészlet használatával</span><span class="sxs-lookup"><span data-stu-id="72f72-103">Publish a web app as a Docker container by using the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="72f72-104">Docker-tároló széles körben használt módszerrel az webes alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="72f72-104">Docker containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="72f72-105">Docker-tárolók használatával a fejlesztők képes egyesíteni a project fájlok és a függőségek egy kiszolgáló üzembe helyezése egyetlen csomagban.</span><span class="sxs-lookup"><span data-stu-id="72f72-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="72f72-106">Az IntelliJ Azure eszköztára egyszerűbben a Java-fejlesztőknek hozzáadásával *Docker-tároló közzététel* központi telepítést, hogy a Microsoft Azure szolgáltatásainak.</span><span class="sxs-lookup"><span data-stu-id="72f72-106">The Azure Toolkit for IntelliJ simplifies this process for Java developers by adding *Publish as Docker Container* features for deployment to Microsoft Azure.</span></span> <span data-ttu-id="72f72-107">Ez a cikk végigvezeti az alkalmazások az Azure-bA Docker tárolóként közzétételéhez szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="72f72-107">This article walks you through the steps required to publish your applications to Azure as Docker containers.</span></span>

> [!NOTE]
>
> <span data-ttu-id="72f72-108">További információ a Docker érhető el a [Docker webhely].</span><span class="sxs-lookup"><span data-stu-id="72f72-108">More information about Docker is available on the [Docker website].</span></span>
>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="72f72-109">A webalkalmazás közzététele az Azure-bA egy Docker-tároló használatával</span><span class="sxs-lookup"><span data-stu-id="72f72-109">Publish your web app to Azure by using a Docker container</span></span>

> [!NOTE]
> * <span data-ttu-id="72f72-110">A webalkalmazás közzétételét, létre kell hoznia egy központi telepítés kész összetevő.</span><span class="sxs-lookup"><span data-stu-id="72f72-110">To publish your web app, you must create a deployment-ready artifact.</span></span> <span data-ttu-id="72f72-111">További tudnivalókért tekintse meg a [összetevők létrehozásával kapcsolatos további információért](#artifacts) szakasz.</span><span class="sxs-lookup"><span data-stu-id="72f72-111">To learn more, see the [Additional information about creating artifacts](#artifacts) section.</span></span>
>
> * <span data-ttu-id="72f72-112">Befejezését követően a központi telepítési varázsló legalább egyszer, a beállítások a legtöbb alapértelmezett használata, amikor a varázsló ismételt futtatása.</span><span class="sxs-lookup"><span data-stu-id="72f72-112">After you have completed the deployment wizard at least once, most of your settings are used as defaults when you run the wizard again.</span></span>
>

1. <span data-ttu-id="72f72-113">Nyissa meg a webalkalmazás-projektet az intellij-t.</span><span class="sxs-lookup"><span data-stu-id="72f72-113">Open your web app project in IntelliJ.</span></span>

2. <span data-ttu-id="72f72-114">Elindítani a **Docker-tároló közzététel** varázsló, tegye a következők egyikét:</span><span class="sxs-lookup"><span data-stu-id="72f72-114">To start the **Publish as Docker Container** wizard, do either of the following:</span></span>

   * <span data-ttu-id="72f72-115">Az a **projekt** eszköz ablak, kattintson jobb gombbal a projektre, kattintson a **Azure**, és kattintson a **Docker-tároló közzététel**:</span><span class="sxs-lookup"><span data-stu-id="72f72-115">In the **Project** tool window, right-click your project, click **Azure**, and then click **Publish as Docker Container**:</span></span>

      ![A közzététel Docker-tároló parancsot][PUB01]

   * <span data-ttu-id="72f72-117">Az IntelliJ eszköztáron kattintson a **közzététel csoport** gombra, majd **Docker-tároló közzététel**:</span><span class="sxs-lookup"><span data-stu-id="72f72-117">On the IntelliJ toolbar, click the **Publish Group** button, and then click **Publish as Docker Container**:</span></span>

      <span data-ttu-id="72f72-118">![A közzététel Docker-tároló parancsot][PUB02]</span><span class="sxs-lookup"><span data-stu-id="72f72-118">![The Publish as Docker Container command][PUB02]</span></span>  
    <span data-ttu-id="72f72-119">A **Docker-tároló központi telepítése az Azure-on** varázsló megnyílik.</span><span class="sxs-lookup"><span data-stu-id="72f72-119">The **Deploy Docker Container on Azure** wizard opens.</span></span>

   ![A központi telepítése Docker-tároló Azure varázsló][PUB03]

3. <span data-ttu-id="72f72-121">Az a **adjon meg egy kép nevet, válassza ki a összetevő elérési útja, ellenőrizze a használt Docker állomás** ablakban tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="72f72-121">In the **Type an image name, select the artifact's path and check a Docker host to be used** window, do the following:</span></span> 

   <span data-ttu-id="72f72-122">a.</span><span class="sxs-lookup"><span data-stu-id="72f72-122">a.</span></span> <span data-ttu-id="72f72-123">Az a **Docker lemezképnév** mezőben adjon meg egy egyedi nevet a Docker-állomás.</span><span class="sxs-lookup"><span data-stu-id="72f72-123">In the **Docker image name** box, enter a unique name for your Docker host.</span></span> <span data-ttu-id="72f72-124">(A varázsló automatikusan létrehozza a nevét, de módosíthatja.)</span><span class="sxs-lookup"><span data-stu-id="72f72-124">(The wizard automatically creates a name, but you can modify it.)</span></span> 

   <span data-ttu-id="72f72-125">b.</span><span class="sxs-lookup"><span data-stu-id="72f72-125">b.</span></span> <span data-ttu-id="72f72-126">A **állomások** terület tartalmazza a Docker állomásokon, már létrehozott.</span><span class="sxs-lookup"><span data-stu-id="72f72-126">The **Hosts** area displays any Docker hosts that you have already created.</span></span> <span data-ttu-id="72f72-127">A következő lehetőségek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="72f72-127">Do either of the following:</span></span> 
      * <span data-ttu-id="72f72-128">Ha egy meglévő Docker-állomás, a webes alkalmazás telepíthet rá.</span><span class="sxs-lookup"><span data-stu-id="72f72-128">If you have an existing Docker host, you can deploy your web app to it.</span></span>
      * <span data-ttu-id="72f72-129">Hozzon létre egy Docker-állomás, kattintson a zöld plusz jel (**+**).</span><span class="sxs-lookup"><span data-stu-id="72f72-129">To create a Docker host, click the green plus sign (**+**).</span></span>  
       <span data-ttu-id="72f72-130">A **Docker állomás létrehozása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="72f72-130">The **Create Docker Host** dialog box opens.</span></span> 

      ![Az Azure varázsló Docker-tároló telepítése][PUB04a]

4. <span data-ttu-id="72f72-132">Az a **állítsa be az új virtuális gép** ablakban adja meg a következő információkat a Docker-állomás.</span><span class="sxs-lookup"><span data-stu-id="72f72-132">In the **Configure the new virtual machine** window, provide the following information about your Docker host.</span></span> <span data-ttu-id="72f72-133">(A varázsló automatikusan létrehozza az adatokat a legtöbb, de egyikük módosíthatja.)</span><span class="sxs-lookup"><span data-stu-id="72f72-133">(The wizard automatically generates most of the information for you, but you can modify any of them.)</span></span> 

   <span data-ttu-id="72f72-134">a.</span><span class="sxs-lookup"><span data-stu-id="72f72-134">a.</span></span> <span data-ttu-id="72f72-135">Az a **neve** mezőben adjon meg egy egyedi nevet a Docker-állomás.</span><span class="sxs-lookup"><span data-stu-id="72f72-135">In the **Name** box, enter a unique name for the Docker host.</span></span> <span data-ttu-id="72f72-136">(Nincs ugyanaz, mint a korábban megadott Docker lemezképnév.)</span><span class="sxs-lookup"><span data-stu-id="72f72-136">(It is not the same as the Docker image name that you specified earlier.)</span></span> 
    
   <span data-ttu-id="72f72-137">b.</span><span class="sxs-lookup"><span data-stu-id="72f72-137">b.</span></span> <span data-ttu-id="72f72-138">Az a **előfizetés** mezőbe írja be az Azure-előfizetés, amelyekkel az állomáshoz.</span><span class="sxs-lookup"><span data-stu-id="72f72-138">In the **Subscription** box, enter the Azure subscription that you use for your host.</span></span> 
      
   <span data-ttu-id="72f72-139">c.</span><span class="sxs-lookup"><span data-stu-id="72f72-139">c.</span></span> <span data-ttu-id="72f72-140">Az a **régió** mezőben adja meg a földrajzi régió, ahol az állomás.</span><span class="sxs-lookup"><span data-stu-id="72f72-140">In the **Region** box, enter the geographical region where your host is located.</span></span>
      
   <span data-ttu-id="72f72-141">d.</span><span class="sxs-lookup"><span data-stu-id="72f72-141">d.</span></span> <span data-ttu-id="72f72-142">Az a **az operációs rendszer és a méret** lapján tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="72f72-142">On the **OS and Size** tab, do the following:</span></span>      
      * <span data-ttu-id="72f72-143">**Az operációs rendszer üzemeltetéséhez**: Adja meg az operációs rendszer a virtuális gép, amely tartalmazza a gazdagépet.</span><span class="sxs-lookup"><span data-stu-id="72f72-143">**Host OS**: Enter the operating system for the virtual machine that contains your host.</span></span> 
      * <span data-ttu-id="72f72-144">**Méret**: Adja meg a virtuális gép méretét az állomáshoz.</span><span class="sxs-lookup"><span data-stu-id="72f72-144">**Size**: Enter the virtual-machine size for your host.</span></span>   
       
   <span data-ttu-id="72f72-145">e.</span><span class="sxs-lookup"><span data-stu-id="72f72-145">e.</span></span> <span data-ttu-id="72f72-146">Az a **erőforráscsoport** lapra, válassza ki a következők egyikét:</span><span class="sxs-lookup"><span data-stu-id="72f72-146">On the **Resource Group** tab, select either of the following:</span></span>      
      * <span data-ttu-id="72f72-147">**Új erőforráscsoport**: hozzon létre egy erőforráscsoportot az állomáshoz.</span><span class="sxs-lookup"><span data-stu-id="72f72-147">**New resource group**: Create a resource group for your host.</span></span>
      * <span data-ttu-id="72f72-148">**Meglévő erőforráscsoport**: Adjon meg egy meglévő erőforráscsoportot az Azure-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="72f72-148">**Existing resource group**: Specify an existing resource group from your Azure account.</span></span> 
       
   <span data-ttu-id="72f72-149">f.</span><span class="sxs-lookup"><span data-stu-id="72f72-149">f.</span></span> <span data-ttu-id="72f72-150">Az a **hálózati** lapra, válassza ki a következők egyikét:</span><span class="sxs-lookup"><span data-stu-id="72f72-150">On the **Network** tab, select either of the following:</span></span>      
      * <span data-ttu-id="72f72-151">**Új virtuális hálózat**: virtuális hálózat létrehozása az állomáshoz.</span><span class="sxs-lookup"><span data-stu-id="72f72-151">**New virtual network**: Create a virtual network for your host.</span></span>
      * <span data-ttu-id="72f72-152">**Meglévő virtuális hálózat**: Adjon meg egy meglévő virtuális hálózatot az Azure-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="72f72-152">**Existing virtual network**: Specify an existing virtual network from your Azure account.</span></span> 
       
   <span data-ttu-id="72f72-153">g.</span><span class="sxs-lookup"><span data-stu-id="72f72-153">g.</span></span> <span data-ttu-id="72f72-154">Az a **tárolási** lapra, válassza ki a következők egyikét:</span><span class="sxs-lookup"><span data-stu-id="72f72-154">On the **Storage** tab, select either of the following:</span></span>      
      * <span data-ttu-id="72f72-155">**Új tárfiók**: hozzon létre egy tárfiókot az állomáshoz.</span><span class="sxs-lookup"><span data-stu-id="72f72-155">**New storage account**: Create a storage account for your host.</span></span>
      * <span data-ttu-id="72f72-156">**Meglévő tárfiók**: Adja meg az Azure-fiókjába meglévő tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="72f72-156">**Existing storage account**: Specify an existing storage account from your Azure account.</span></span>
       
5. <span data-ttu-id="72f72-157">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="72f72-157">Click **Next**.</span></span>  
     <span data-ttu-id="72f72-158">A **napló konfigurálja a hitelesítő adatokat, és a portbeállítások** ablak nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="72f72-158">The **Configure log in credentials and port settings** window opens.</span></span>

      ![A konfigurálás bejelentkezési hitelesítő adatok és a port beállításai ablak][PUB05]

6. <span data-ttu-id="72f72-160">A következő lehetőségek közül:</span><span class="sxs-lookup"><span data-stu-id="72f72-160">Select one of the following options:</span></span>

      * <span data-ttu-id="72f72-161">**Hitelesítő adatok importálása az Azure Key Vault**: Adjon meg egy korábban mentett hitelesítő adatokat, amelyek az Azure-előfizetéshez vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="72f72-161">**Import credentials from Azure Key Vault**: Specify a previously saved set of credentials that are stored in your Azure subscription.</span></span>

          > [!NOTE]
          > <span data-ttu-id="72f72-162">Az az Azure key vault, amely egy meghatározott fiók vagy a szolgáltatás egyszerű nincs egy másik fiókot vagy egyszerű szolgáltatásnév osztja meg az előfizetés által automatikusan elérhető.</span><span class="sxs-lookup"><span data-stu-id="72f72-162">An Azure key vault that's created with a specific account or service principal is not automatically accessible by another account or service principal that shares the subscription.</span></span> <span data-ttu-id="72f72-163">Egy másik fiókot, vagy a szolgáltatás lehetővé teszi a key vault használata egyszerű, az Azure-portálon kell használnia a fiókot, vagy az egyszerű szolgáltatásnév hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="72f72-163">To allow another account or service principal to use the key vault, you must use the Azure portal to add the account or service principal.</span></span>

      * <span data-ttu-id="72f72-164">**Új bejelentkezési hitelesítő adatok**: létrehozhat egy új bejelentkezési hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="72f72-164">**New log in credentials**: Create a new set of login credentials.</span></span> <span data-ttu-id="72f72-165">Ha ezt a lehetőséget választja, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="72f72-165">If you select this option, do the following:</span></span>

        <span data-ttu-id="72f72-166">a.</span><span class="sxs-lookup"><span data-stu-id="72f72-166">a.</span></span> <span data-ttu-id="72f72-167">Az a **VM hitelesítő adatok** lapra, adja meg a következő információkat a virtuális gép bejelentkezési hitelesítő adatok a Docker-állomás: * **felhasználónév**: be a felhasználónevet a virtuális gép bejelentkezési adatait hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="72f72-167">On the **VM Credentials** tab, provide the following information for the virtual-machine login credentials of your Docker host: * **Username**: Enter the username for your virtual-machine login credentials.</span></span>
             <span data-ttu-id="72f72-168">* **Jelszó** és **megerősítése**: Adja meg a jelszót a virtuális gép bejelentkezési hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="72f72-168">* **Password** and **Confirm**: Enter the password for your virtual-machine login credentials.</span></span>
             <span data-ttu-id="72f72-169">* **SSH**: Adja meg a Secure Shell (SSH) beállításait a Docker-állomás.</span><span class="sxs-lookup"><span data-stu-id="72f72-169">* **SSH**: Enter the Secure Shell (SSH) settings for your Docker host.</span></span> <span data-ttu-id="72f72-170">Az alábbi lehetőségek közül választhat: * **nincs**: meghatározza, hogy a virtuális gép nem teszi lehetővé SSH-kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="72f72-170">You can select one of the following options: * **None**: Specifies that your virtual machine does not allow SSH connections.</span></span>
                <span data-ttu-id="72f72-171">* **Automatikus generálása**: automatikusan létrehozza a szükséges beállítások SSH-kapcsolaton keresztül csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="72f72-171">* **Auto-generate**: Automatically creates the requisite settings for connecting via SSH.</span></span>
                <span data-ttu-id="72f72-172">* **Importálás a directory**: lehetővé teszi egy korábban mentett SSH-beállítások készletét tartalmazó könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="72f72-172">* **Import from directory**: Allows you to specify a directory that contains a set of previously saved SSH settings.</span></span> <span data-ttu-id="72f72-173">A könyvtár a következő két fájlt kell tartalmaznia:</span><span class="sxs-lookup"><span data-stu-id="72f72-173">The directory must contain the following two files:</span></span>
                
                  * *id_rsa*: Contains the RSA identification for a user.
                  * *id_rsa.pub*: Contains the RSA public key that is used for authentication.
            
        <span data-ttu-id="72f72-174">b.</span><span class="sxs-lookup"><span data-stu-id="72f72-174">b.</span></span> <span data-ttu-id="72f72-175">Az a **Docker démon hozzáférés** lapra, adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="72f72-175">On the **Docker Daemon Access** tab, provide the following information:</span></span>

          ![Docker állomás létrehozása][PUB06]
    
             * **Docker Daemon port**: Enter the unique TCP port for your Docker host.
             * **TLS Security**: Enter the Transport Layer Security settings for your Docker host. You can choose from the following options:
                * **None**: Specifies that your virtual machine does not allow TLS connections.
                * **Auto-generate**: Automatically creates the requisite settings for connecting via TLS.
                * **Import from directory**: Specifies a directory that contains a set of previously saved TLS settings. The directory must contain the following six files: 
                   * *ca.pem* and *ca-key.pem*: Contain the certificate and public key for the TLS Certificate Authority.
                   * *cert.pem* and *key.pem*: Contain client certificate and public key which will be used for TLS authentication.
                   * *server.pem* and *server-key.pem*: Contain the client certificate and public key that is used for TLS authentication.

7. <span data-ttu-id="72f72-177">Miután megadta a szükséges adatokat, kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="72f72-177">After you have entered the required information, click **Finish**.</span></span>  
    <span data-ttu-id="72f72-178">A **Docker-tároló központi telepítése az Azure-on** varázsló ismét megjelenik.</span><span class="sxs-lookup"><span data-stu-id="72f72-178">The **Deploy Docker Container on Azure** wizard reappears.</span></span>

   ![Az Azure varázsló Docker-tároló telepítése][PUB07]

8. <span data-ttu-id="72f72-180">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="72f72-180">Click **Next**.</span></span>  
    <span data-ttu-id="72f72-181">A **létrehozni a Docker-tároló konfigurálása** ablak nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="72f72-181">The **Configure the Docker container to be created** window opens.</span></span>

   ![A létrehozandó ablakban a Docker-tároló konfigurálása][PUB08]

9. <span data-ttu-id="72f72-183">Az a **létrehozni a Docker-tároló konfigurálása** ablakban adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="72f72-183">In the **Configure the Docker container to be created** window, provide the following information:</span></span> 

   <span data-ttu-id="72f72-184">a.</span><span class="sxs-lookup"><span data-stu-id="72f72-184">a.</span></span> <span data-ttu-id="72f72-185">Az a **Docker-tároló neve** mezőben adjon meg egy egyedi nevet a Docker-tároló.</span><span class="sxs-lookup"><span data-stu-id="72f72-185">In the **Docker container name** box, enter a unique name for your Docker container.</span></span>

   <span data-ttu-id="72f72-186">b.</span><span class="sxs-lookup"><span data-stu-id="72f72-186">b.</span></span> <span data-ttu-id="72f72-187">A következő Docker-rendszerképek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="72f72-187">Choose one of the following Docker images:</span></span> 

      * <span data-ttu-id="72f72-188">**Előre definiált Docker kép**: Adjon meg egy már meglévő Docker-lemezképet az Azure-ból.</span><span class="sxs-lookup"><span data-stu-id="72f72-188">**Predefined Docker image**: Specify a pre-existing Docker image from Azure.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="72f72-189">A Docker képek ebben a mezőben a lista az számos olyan rendszerkép található, amely az Azure-eszközkészlet lett konfigurálva, hogy a rendszer automatikusan telepíti az összetevő javítás.</span><span class="sxs-lookup"><span data-stu-id="72f72-189">The list of Docker images in this box consists of several images that the Azure Toolkit has been configured to patch so that your artifact is deployed automatically.</span></span> 

      * <span data-ttu-id="72f72-190">**Egyéni Dockerfile**: Adjon meg egy korábban mentett Dockerfile a helyi számítógépről.</span><span class="sxs-lookup"><span data-stu-id="72f72-190">**Custom Dockerfile**: Specify a previously saved Dockerfile from your local computer.</span></span>

        > [!NOTE]
        > <span data-ttu-id="72f72-191">Ez a tulajdonság speciális fejlesztők számára a saját Dockerfile telepíteni kívánja.</span><span class="sxs-lookup"><span data-stu-id="72f72-191">This is a more advanced feature for developers who want to deploy their own Dockerfile.</span></span> <span data-ttu-id="72f72-192">Azonban esetén a fejlesztők számára ez a beállítás segítségével győződjön meg arról, hogy azok Dockerfile van megfelelően lett felépítve.</span><span class="sxs-lookup"><span data-stu-id="72f72-192">However, it is up to developers who use this option to ensure that their Dockerfile is built correctly.</span></span> <span data-ttu-id="72f72-193">Az Azure-eszközkészlet nem felel meg egy egyéni Dockerfile tartalmát, mert a telepítés meghiúsulhat, ha a Dockerfile problémákkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="72f72-193">Because the Azure Toolkit does not validate the content in a custom Dockerfile, the deployment might fail if the Dockerfile has issues.</span></span> <span data-ttu-id="72f72-194">Ezenkívül az Azure Toolkit tartalmaz egy webes alkalmazás összetevő egyéni Dockerfile vár, mert megpróbálja nyissa meg a HTTP-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="72f72-194">In addition, because the Azure Toolkit expects the custom Dockerfile to contain a web app artifact, it attempts to open an HTTP connection.</span></span> <span data-ttu-id="72f72-195">Fejlesztők összetevő más típusú közzé, ha azok ártalmatlan hibák fordulhat elő, a telepítés utáni.</span><span class="sxs-lookup"><span data-stu-id="72f72-195">If developers publish a different type of artifact, they might receive innocuous errors after deployment.</span></span>

   <span data-ttu-id="72f72-196">c.</span><span class="sxs-lookup"><span data-stu-id="72f72-196">c.</span></span> <span data-ttu-id="72f72-197">Az a **portbeállításokat** mezőbe írja be a Docker-tároló egyedi TCP-port kötés.</span><span class="sxs-lookup"><span data-stu-id="72f72-197">In the **Port settings** box, enter the unique TCP port binding for your Docker container.</span></span> 

10. <span data-ttu-id="72f72-198">Kattintson a fenti lépések végrehajtása után **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="72f72-198">After you have completed the preceding steps, click **Finish**.</span></span> 

<span data-ttu-id="72f72-199">Az Azure-eszközkészlet megkezdi a központi telepítését a webalkalmazás Azure egy Docker-tároló.</span><span class="sxs-lookup"><span data-stu-id="72f72-199">The Azure Toolkit begins deploying your web app to Azure in a Docker container.</span></span> <span data-ttu-id="72f72-200">Ha beállította az intellij-t a háttérben telepíteni egy **üzembe helyezése az Azure-bA** folyamatjelző sáv jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="72f72-200">Unless you have configured IntelliJ to be deployed in the background, a **Deploying to Azure** progress bar appears.</span></span> 

![A telepítés folyamatjelző][PUB09]

<a name="artifacts"></a>
## <a name="additional-information-about-creating-artifacts"></a><span data-ttu-id="72f72-202">Az összetevők létrehozásával kapcsolatos további információért</span><span class="sxs-lookup"><span data-stu-id="72f72-202">Additional information about creating artifacts</span></span>

<span data-ttu-id="72f72-203">A telepítés kész összetevő létrehozásához tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="72f72-203">To create a deployment-ready artifact, do the following:</span></span>

1. <span data-ttu-id="72f72-204">Nyissa meg a webalkalmazás-projektet az intellij-t.</span><span class="sxs-lookup"><span data-stu-id="72f72-204">Open your web app project in IntelliJ.</span></span>

2. <span data-ttu-id="72f72-205">Kattintson a **fájl**, és kattintson a **szerkezetének**.</span><span class="sxs-lookup"><span data-stu-id="72f72-205">Click **File**, and then click **Project Structure**.</span></span>

   ![A projekt struktúra parancs][ART01]

3. <span data-ttu-id="72f72-207">Összetevő hozzáadásához kattintson a zöld plusz jel (**+**), és kattintson a **webalkalmazás: archív**.</span><span class="sxs-lookup"><span data-stu-id="72f72-207">To add an artifact, click the green plus sign (**+**), and then click **Web Application: Archive**.</span></span>

   ![A ": webalkalmazások archívumából" parancs][ART02]

4. <span data-ttu-id="72f72-209">Az a **neve** mezőbe írja be a adatösszetevőt nevét (nem tartalmaznak a *.war* bővítmény), és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="72f72-209">In the **Name** box, enter a name for your artifact (do not include the *.war* extension), and then click **OK**.</span></span>

   ![Az összetevő neve mező][ART03]

<span data-ttu-id="72f72-211">Az IntelliJ összetevők létrehozásával kapcsolatos további információkért lásd: [összetevők konfigurálása] a JetBrains webhelyen.</span><span class="sxs-lookup"><span data-stu-id="72f72-211">For more information about creating artifacts in IntelliJ, see [Configuring artifacts] on the JetBrains website.</span></span>

## <a name="next-steps"></a><span data-ttu-id="72f72-212">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="72f72-212">Next steps</span></span>
<span data-ttu-id="72f72-213">A Java IDEs az Azure eszközök gazdag kapcsolatos további információkért lásd a következőket:</span><span class="sxs-lookup"><span data-stu-id="72f72-213">For more information about the Azure Toolkits for Java IDEs, see the following resources:</span></span>

* <span data-ttu-id="72f72-214">[Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="72f72-214">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="72f72-215">[What's new in Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="72f72-215">[What's new in the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="72f72-216">[Az Eclipse-hez készült Azure-eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="72f72-216">[Installing the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="72f72-217">[Az Eclipse Azure eszköztára bejelentkezési utasítások]</span><span class="sxs-lookup"><span data-stu-id="72f72-217">[Sign-in instructions for the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="72f72-218">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]</span><span class="sxs-lookup"><span data-stu-id="72f72-218">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="72f72-219">[Az IntelliJ-hez készült Azure-eszközkészlet]</span><span class="sxs-lookup"><span data-stu-id="72f72-219">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="72f72-220">[What's new in IntelliJ Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="72f72-220">[What's new in the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="72f72-221">[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="72f72-221">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="72f72-222">[Bejelentkezési utasításokat az Azure-eszközkészlet az intellij-t]</span><span class="sxs-lookup"><span data-stu-id="72f72-222">[Sign-in instructions for the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="72f72-223">[Hello World webalkalmazás létrehozása az intellij-t az Azure]</span><span class="sxs-lookup"><span data-stu-id="72f72-223">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="72f72-224">Az Azure Javával való használatáról további információ: [Azure Java fejlesztői központ] és [Java-eszközök a Visual Studio Team Serviceshez].</span><span class="sxs-lookup"><span data-stu-id="72f72-224">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="72f72-225">További erőforrásokat a Docker, tekintse meg a hivatalos [Docker webhely].</span><span class="sxs-lookup"><span data-stu-id="72f72-225">For additional resources for Docker, see the official [Docker website].</span></span>

<!-- URL List -->

<span data-ttu-id="72f72-226">[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md</span><span class="sxs-lookup"><span data-stu-id="72f72-226">[Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse.md</span></span>
<span data-ttu-id="72f72-227">[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md</span><span class="sxs-lookup"><span data-stu-id="72f72-227">[Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij.md</span></span>
<span data-ttu-id="72f72-228">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="72f72-228">[Create a Hello World web app for Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span></span>
<span data-ttu-id="72f72-229">[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="72f72-229">[Create a Hello World web app for Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span></span>
<span data-ttu-id="72f72-230">[Az Eclipse-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-eclipse-installation.md</span><span class="sxs-lookup"><span data-stu-id="72f72-230">[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span></span>
<span data-ttu-id="72f72-231">[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md</span><span class="sxs-lookup"><span data-stu-id="72f72-231">[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span></span>
<span data-ttu-id="72f72-232">[Az Eclipse Azure eszköztára bejelentkezési utasítások]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="72f72-232">[Sign-in instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span></span>
<span data-ttu-id="72f72-233">[Bejelentkezési utasításokat az Azure-eszközkészlet az intellij-t]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="72f72-233">[Sign-in instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span></span>
<span data-ttu-id="72f72-234">[What's new in Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="72f72-234">[What's new in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span></span>
<span data-ttu-id="72f72-235">[What's new in IntelliJ Azure eszköztára]: ./azure-toolkit-for-intellij-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="72f72-235">[What's new in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span></span>

<span data-ttu-id="72f72-236">[Azure Java fejlesztői központ]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="72f72-236">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="72f72-237">[Java-eszközök a Visual Studio Team Serviceshez]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="72f72-237">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>

<span data-ttu-id="72f72-238">[Docker webhely]: https://www.docker.com/</span><span class="sxs-lookup"><span data-stu-id="72f72-238">[Docker website]: https://www.docker.com/</span></span>
<span data-ttu-id="72f72-239">[összetevők konfigurálása]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html</span><span class="sxs-lookup"><span data-stu-id="72f72-239">[Configuring artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html</span></span>

<!-- IMG List -->

[PUB01]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB01.png
[PUB02]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB02.png
[PUB03]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB03.png
[PUB04a]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04a.png
[PUB04b]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04b.png
[PUB04c]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04c.png
[PUB04d]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04d.png
[PUB05]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB05.png
[PUB06]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB06.png
[PUB07]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB07.png
[PUB08]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB08.png
[PUB09]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB09.png

[ART01]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/ART01.png
[ART02]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/ART02.png
[ART03]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/ART03.png
