---
title: "egy Docker-tároló segítségével aaaPublish hello Azure eszközkészlet IntelliJ |} Microsoft Docs"
description: "Megtudhatja, hogyan toopublish egy webes alkalmazás tooMicrosoft Azure-bA egy Docker-tároló segítségével hello Azure eszközkészlet intellij-t."
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
ms.openlocfilehash: bee94cb269ea707ae7ad55232e23e915aec48c34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-hello-azure-toolkit-for-intellij"></a><span data-ttu-id="9ca64-103">A webes alkalmazás is egy Docker-tároló közzé a IntelliJ hello Azure eszközkészlet használatával</span><span class="sxs-lookup"><span data-stu-id="9ca64-103">Publish a web app as a Docker container by using hello Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="9ca64-104">Docker-tároló széles körben használt módszerrel az webes alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="9ca64-104">Docker containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="9ca64-105">Docker-tárolók használatával a fejlesztők képes egyesíteni a project fájlok és a függőségek telepítési tooa kiszolgáló egyetlen csomagban.</span><span class="sxs-lookup"><span data-stu-id="9ca64-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment tooa server.</span></span> <span data-ttu-id="9ca64-106">hello Azure eszköztára IntelliJ egyszerűbben a Java-fejlesztőknek hozzáadásával *Docker-tároló közzététel* telepítési tooMicrosoft Azure szolgáltatásai.</span><span class="sxs-lookup"><span data-stu-id="9ca64-106">hello Azure Toolkit for IntelliJ simplifies this process for Java developers by adding *Publish as Docker Container* features for deployment tooMicrosoft Azure.</span></span> <span data-ttu-id="9ca64-107">Ez a cikk bemutatja, hogyan hello lépéseket szükséges toopublish az alkalmazások tooAzure Docker tárolóként.</span><span class="sxs-lookup"><span data-stu-id="9ca64-107">This article walks you through hello steps required toopublish your applications tooAzure as Docker containers.</span></span>

> [!NOTE]
>
> <span data-ttu-id="9ca64-108">További információ a Docker érhető el hello [Docker webhely].</span><span class="sxs-lookup"><span data-stu-id="9ca64-108">More information about Docker is available on hello [Docker website].</span></span>
>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="publish-your-web-app-tooazure-by-using-a-docker-container"></a><span data-ttu-id="9ca64-109">A webes alkalmazás tooAzure közzététele egy Docker-tároló használatával</span><span class="sxs-lookup"><span data-stu-id="9ca64-109">Publish your web app tooAzure by using a Docker container</span></span>

> [!NOTE]
> * <span data-ttu-id="9ca64-110">toopublish webalkalmazását, létre kell hoznia egy központi telepítés kész összetevő.</span><span class="sxs-lookup"><span data-stu-id="9ca64-110">toopublish your web app, you must create a deployment-ready artifact.</span></span> <span data-ttu-id="9ca64-111">toolearn több, tekintse meg a hello [összetevők létrehozásával kapcsolatos további információért](#artifacts) szakasz.</span><span class="sxs-lookup"><span data-stu-id="9ca64-111">toolearn more, see hello [Additional information about creating artifacts](#artifacts) section.</span></span>
>
> * <span data-ttu-id="9ca64-112">Befejezését követően hello telepítési varázsló legalább egyszer, a beállítások a legtöbb alapértelmezett szolgálnak, hogy hello varázsló futtatásakor ismét.</span><span class="sxs-lookup"><span data-stu-id="9ca64-112">After you have completed hello deployment wizard at least once, most of your settings are used as defaults when you run hello wizard again.</span></span>
>

1. <span data-ttu-id="9ca64-113">Nyissa meg a webalkalmazás-projektet az intellij-t.</span><span class="sxs-lookup"><span data-stu-id="9ca64-113">Open your web app project in IntelliJ.</span></span>

2. <span data-ttu-id="9ca64-114">toostart hello **Docker-tároló közzététel** varázsló hello alábbi módszerekkel:</span><span class="sxs-lookup"><span data-stu-id="9ca64-114">toostart hello **Publish as Docker Container** wizard, do either of hello following:</span></span>

   * <span data-ttu-id="9ca64-115">A hello **projekt** eszköz ablak, kattintson jobb gombbal a projektre, kattintson a **Azure**, és kattintson a **Docker-tároló közzététel**:</span><span class="sxs-lookup"><span data-stu-id="9ca64-115">In hello **Project** tool window, right-click your project, click **Azure**, and then click **Publish as Docker Container**:</span></span>

      ![hello Docker-tároló parancsként közzététele][PUB01]

   * <span data-ttu-id="9ca64-117">Hello IntelliJ eszköztáron kattintson a hello **közzététel csoport** gombra, majd **Docker-tároló közzététel**:</span><span class="sxs-lookup"><span data-stu-id="9ca64-117">On hello IntelliJ toolbar, click hello **Publish Group** button, and then click **Publish as Docker Container**:</span></span>

      <span data-ttu-id="9ca64-118">![hello Docker-tároló parancsként közzététele][PUB02]</span><span class="sxs-lookup"><span data-stu-id="9ca64-118">![hello Publish as Docker Container command][PUB02]</span></span>  
    <span data-ttu-id="9ca64-119">Hello **Docker-tároló központi telepítése az Azure-on** varázsló megnyílik.</span><span class="sxs-lookup"><span data-stu-id="9ca64-119">hello **Deploy Docker Container on Azure** wizard opens.</span></span>

   ![hello Azure varázsló a Docker-tároló központi telepítése][PUB03]

3. <span data-ttu-id="9ca64-121">A hello **írja be a lemezkép nevét, válassza ki a hello összetevő elérési útja, ellenőrizze a használt Docker állomás toobe** ablakban, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="9ca64-121">In hello **Type an image name, select hello artifact's path and check a Docker host toobe used** window, do hello following:</span></span> 

   <span data-ttu-id="9ca64-122">a.</span><span class="sxs-lookup"><span data-stu-id="9ca64-122">a.</span></span> <span data-ttu-id="9ca64-123">A hello **Docker lemezképnév** mezőben adjon meg egy egyedi nevet a Docker-állomás.</span><span class="sxs-lookup"><span data-stu-id="9ca64-123">In hello **Docker image name** box, enter a unique name for your Docker host.</span></span> <span data-ttu-id="9ca64-124">(hello varázsló automatikusan létrehozza a nevét, de módosíthatja.)</span><span class="sxs-lookup"><span data-stu-id="9ca64-124">(hello wizard automatically creates a name, but you can modify it.)</span></span> 

   <span data-ttu-id="9ca64-125">b.</span><span class="sxs-lookup"><span data-stu-id="9ca64-125">b.</span></span> <span data-ttu-id="9ca64-126">Hello **állomások** terület tartalmazza a Docker állomásokon, már létrehozott.</span><span class="sxs-lookup"><span data-stu-id="9ca64-126">hello **Hosts** area displays any Docker hosts that you have already created.</span></span> <span data-ttu-id="9ca64-127">Hello alábbi módszerekkel:</span><span class="sxs-lookup"><span data-stu-id="9ca64-127">Do either of hello following:</span></span> 
      * <span data-ttu-id="9ca64-128">Ha egy meglévő Docker-állomás, a webes alkalmazás tooit telepítheti.</span><span class="sxs-lookup"><span data-stu-id="9ca64-128">If you have an existing Docker host, you can deploy your web app tooit.</span></span>
      * <span data-ttu-id="9ca64-129">toocreate egy Docker-állomás, kattintson a hello zöld plusz jel (**+**).</span><span class="sxs-lookup"><span data-stu-id="9ca64-129">toocreate a Docker host, click hello green plus sign (**+**).</span></span>  
       <span data-ttu-id="9ca64-130">Hello **Docker állomás létrehozása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9ca64-130">hello **Create Docker Host** dialog box opens.</span></span> 

      ![Az Azure varázsló Docker-tároló telepítése][PUB04a]

4. <span data-ttu-id="9ca64-132">A hello **hello új virtuális gép konfigurálása** ablakban adja meg a következő információ a Docker állomás hello.</span><span class="sxs-lookup"><span data-stu-id="9ca64-132">In hello **Configure hello new virtual machine** window, provide hello following information about your Docker host.</span></span> <span data-ttu-id="9ca64-133">(hello varázsló automatikusan létrehozza a legtöbb hello az adatokat, de egyikük módosíthatja.)</span><span class="sxs-lookup"><span data-stu-id="9ca64-133">(hello wizard automatically generates most of hello information for you, but you can modify any of them.)</span></span> 

   <span data-ttu-id="9ca64-134">a.</span><span class="sxs-lookup"><span data-stu-id="9ca64-134">a.</span></span> <span data-ttu-id="9ca64-135">A hello **neve** hello Docker állomás egyedi nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="9ca64-135">In hello **Name** box, enter a unique name for hello Docker host.</span></span> <span data-ttu-id="9ca64-136">(Fontos nem ugyanaz, mint a korábban megadott Docker lemezképnév hello hello.)</span><span class="sxs-lookup"><span data-stu-id="9ca64-136">(It is not hello same as hello Docker image name that you specified earlier.)</span></span> 
    
   <span data-ttu-id="9ca64-137">b.</span><span class="sxs-lookup"><span data-stu-id="9ca64-137">b.</span></span> <span data-ttu-id="9ca64-138">A hello **előfizetés** mezőbe írja be a hello Azure-előfizetéssel, amelyekkel az állomáshoz.</span><span class="sxs-lookup"><span data-stu-id="9ca64-138">In hello **Subscription** box, enter hello Azure subscription that you use for your host.</span></span> 
      
   <span data-ttu-id="9ca64-139">c.</span><span class="sxs-lookup"><span data-stu-id="9ca64-139">c.</span></span> <span data-ttu-id="9ca64-140">A hello **régió** mezőbe írja be a hello földrajzi terület, ahol a gazdagépen található.</span><span class="sxs-lookup"><span data-stu-id="9ca64-140">In hello **Region** box, enter hello geographical region where your host is located.</span></span>
      
   <span data-ttu-id="9ca64-141">d.</span><span class="sxs-lookup"><span data-stu-id="9ca64-141">d.</span></span> <span data-ttu-id="9ca64-142">A hello **az operációs rendszer és a méret** lapon, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="9ca64-142">On hello **OS and Size** tab, do hello following:</span></span>      
      * <span data-ttu-id="9ca64-143">**Az operációs rendszer üzemeltetéséhez**: hello operációs rendszer hello virtuális gépet, amely tartalmazza a gazdagépet adjon meg.</span><span class="sxs-lookup"><span data-stu-id="9ca64-143">**Host OS**: Enter hello operating system for hello virtual machine that contains your host.</span></span> 
      * <span data-ttu-id="9ca64-144">**Méret**: hello virtuális gép méretét adja meg a gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="9ca64-144">**Size**: Enter hello virtual-machine size for your host.</span></span>   
       
   <span data-ttu-id="9ca64-145">e.</span><span class="sxs-lookup"><span data-stu-id="9ca64-145">e.</span></span> <span data-ttu-id="9ca64-146">A hello **erőforráscsoport** lapra, válassza ki a hello a következők egyikét:</span><span class="sxs-lookup"><span data-stu-id="9ca64-146">On hello **Resource Group** tab, select either of hello following:</span></span>      
      * <span data-ttu-id="9ca64-147">**Új erőforráscsoport**: hozzon létre egy erőforráscsoportot az állomáshoz.</span><span class="sxs-lookup"><span data-stu-id="9ca64-147">**New resource group**: Create a resource group for your host.</span></span>
      * <span data-ttu-id="9ca64-148">**Meglévő erőforráscsoport**: Adjon meg egy meglévő erőforráscsoportot az Azure-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="9ca64-148">**Existing resource group**: Specify an existing resource group from your Azure account.</span></span> 
       
   <span data-ttu-id="9ca64-149">f.</span><span class="sxs-lookup"><span data-stu-id="9ca64-149">f.</span></span> <span data-ttu-id="9ca64-150">A hello **hálózati** lapra, válassza ki a hello a következők egyikét:</span><span class="sxs-lookup"><span data-stu-id="9ca64-150">On hello **Network** tab, select either of hello following:</span></span>      
      * <span data-ttu-id="9ca64-151">**Új virtuális hálózat**: virtuális hálózat létrehozása az állomáshoz.</span><span class="sxs-lookup"><span data-stu-id="9ca64-151">**New virtual network**: Create a virtual network for your host.</span></span>
      * <span data-ttu-id="9ca64-152">**Meglévő virtuális hálózat**: Adjon meg egy meglévő virtuális hálózatot az Azure-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="9ca64-152">**Existing virtual network**: Specify an existing virtual network from your Azure account.</span></span> 
       
   <span data-ttu-id="9ca64-153">g.</span><span class="sxs-lookup"><span data-stu-id="9ca64-153">g.</span></span> <span data-ttu-id="9ca64-154">A hello **tárolási** lapra, válassza ki a hello a következők egyikét:</span><span class="sxs-lookup"><span data-stu-id="9ca64-154">On hello **Storage** tab, select either of hello following:</span></span>      
      * <span data-ttu-id="9ca64-155">**Új tárfiók**: hozzon létre egy tárfiókot az állomáshoz.</span><span class="sxs-lookup"><span data-stu-id="9ca64-155">**New storage account**: Create a storage account for your host.</span></span>
      * <span data-ttu-id="9ca64-156">**Meglévő tárfiók**: Adja meg az Azure-fiókjába meglévő tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="9ca64-156">**Existing storage account**: Specify an existing storage account from your Azure account.</span></span>
       
5. <span data-ttu-id="9ca64-157">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="9ca64-157">Click **Next**.</span></span>  
     <span data-ttu-id="9ca64-158">Hello **napló konfigurálja a hitelesítő adatokat, és a portbeállítások** ablak nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="9ca64-158">hello **Configure log in credentials and port settings** window opens.</span></span>

      ![hello konfigurálása bejelentkezési hitelesítő adatok és a port beállításai ablak][PUB05]

6. <span data-ttu-id="9ca64-160">Válasszon egyet az alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="9ca64-160">Select one of hello following options:</span></span>

      * <span data-ttu-id="9ca64-161">**Hitelesítő adatok importálása az Azure Key Vault**: Adjon meg egy korábban mentett hitelesítő adatokat, amelyek az Azure-előfizetéshez vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="9ca64-161">**Import credentials from Azure Key Vault**: Specify a previously saved set of credentials that are stored in your Azure subscription.</span></span>

          > [!NOTE]
          > <span data-ttu-id="9ca64-162">Az az Azure key vault, amely egy meghatározott fiók vagy a szolgáltatás egyszerű nincs egy másik fiókot vagy egyszerű szolgáltatásnév osztja meg a hello előfizetés automatikusan elérhető.</span><span class="sxs-lookup"><span data-stu-id="9ca64-162">An Azure key vault that's created with a specific account or service principal is not automatically accessible by another account or service principal that shares hello subscription.</span></span> <span data-ttu-id="9ca64-163">tooallow egy másik fiókot, vagy a szolgáltatás egyszerű toouse hello kulcsot tároló, hello Azure portál tooadd hello fiók vagy a szolgáltatásnevet kell használnia.</span><span class="sxs-lookup"><span data-stu-id="9ca64-163">tooallow another account or service principal toouse hello key vault, you must use hello Azure portal tooadd hello account or service principal.</span></span>

      * <span data-ttu-id="9ca64-164">**Új bejelentkezési hitelesítő adatok**: létrehozhat egy új bejelentkezési hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="9ca64-164">**New log in credentials**: Create a new set of login credentials.</span></span> <span data-ttu-id="9ca64-165">Ha ezt a lehetőséget választja, a hello, a következő:</span><span class="sxs-lookup"><span data-stu-id="9ca64-165">If you select this option, do hello following:</span></span>

        <span data-ttu-id="9ca64-166">a.</span><span class="sxs-lookup"><span data-stu-id="9ca64-166">a.</span></span> <span data-ttu-id="9ca64-167">A hello **VM hitelesítő adatok** lapra, adja meg a következő információ hello virtuális gép bejelentkezési hitelesítő adatokat a Docker-állomás hello: * **felhasználónév**: Adja meg a virtuális gép bejelentkezési hello felhasználónév hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="9ca64-167">On hello **VM Credentials** tab, provide hello following information for hello virtual-machine login credentials of your Docker host: * **Username**: Enter hello username for your virtual-machine login credentials.</span></span>
             <span data-ttu-id="9ca64-168">* **Jelszó** és **megerősítése**: hello jelszó megadása a virtuális gép bejelentkezési hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="9ca64-168">* **Password** and **Confirm**: Enter hello password for your virtual-machine login credentials.</span></span>
             <span data-ttu-id="9ca64-169">* **SSH**: Adja meg a Docker állomás hello Secure Shell (SSH) beállításait.</span><span class="sxs-lookup"><span data-stu-id="9ca64-169">* **SSH**: Enter hello Secure Shell (SSH) settings for your Docker host.</span></span> <span data-ttu-id="9ca64-170">Hello alábbi beállítások közül választhat: * **nincs**: meghatározza, hogy a virtuális gép nem teszi lehetővé SSH-kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="9ca64-170">You can select one of hello following options: * **None**: Specifies that your virtual machine does not allow SSH connections.</span></span>
                <span data-ttu-id="9ca64-171">* **Automatikus generálása**: hozza létre automatikusan hello szükséges beállítások SSH-kapcsolaton keresztül csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="9ca64-171">* **Auto-generate**: Automatically creates hello requisite settings for connecting via SSH.</span></span>
                <span data-ttu-id="9ca64-172">* **Importálás a directory**: lehetővé teszi egy korábban mentett SSH-beállítások készletét tartalmazó könyvtár toospecify.</span><span class="sxs-lookup"><span data-stu-id="9ca64-172">* **Import from directory**: Allows you toospecify a directory that contains a set of previously saved SSH settings.</span></span> <span data-ttu-id="9ca64-173">hello directory tartalmaznia kell a következő két fájlok hello:</span><span class="sxs-lookup"><span data-stu-id="9ca64-173">hello directory must contain hello following two files:</span></span>
                
                  * *id_rsa*: Contains hello RSA identification for a user.
                  * *id_rsa.pub*: Contains hello RSA public key that is used for authentication.
            
        <span data-ttu-id="9ca64-174">b.</span><span class="sxs-lookup"><span data-stu-id="9ca64-174">b.</span></span> <span data-ttu-id="9ca64-175">A hello **Docker démon hozzáférés** lapra, adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="9ca64-175">On hello **Docker Daemon Access** tab, provide hello following information:</span></span>

          ![Docker állomás létrehozása][PUB06]
    
             * **Docker Daemon port**: Enter hello unique TCP port for your Docker host.
             * **TLS Security**: Enter hello Transport Layer Security settings for your Docker host. You can choose from hello following options:
                * **None**: Specifies that your virtual machine does not allow TLS connections.
                * **Auto-generate**: Automatically creates hello requisite settings for connecting via TLS.
                * **Import from directory**: Specifies a directory that contains a set of previously saved TLS settings. hello directory must contain hello following six files: 
                   * *ca.pem* and *ca-key.pem*: Contain hello certificate and public key for hello TLS Certificate Authority.
                   * *cert.pem* and *key.pem*: Contain client certificate and public key which will be used for TLS authentication.
                   * *server.pem* and *server-key.pem*: Contain hello client certificate and public key that is used for TLS authentication.

7. <span data-ttu-id="9ca64-177">Miután megadta a hello szükséges információkat, kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="9ca64-177">After you have entered hello required information, click **Finish**.</span></span>  
    <span data-ttu-id="9ca64-178">Hello **Docker-tároló központi telepítése az Azure-on** varázsló ismét megjelenik.</span><span class="sxs-lookup"><span data-stu-id="9ca64-178">hello **Deploy Docker Container on Azure** wizard reappears.</span></span>

   ![Az Azure varázsló Docker-tároló telepítése][PUB07]

8. <span data-ttu-id="9ca64-180">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="9ca64-180">Click **Next**.</span></span>  
    <span data-ttu-id="9ca64-181">Hello **hello Docker-tároló toobe létrehozott konfigurálása** ablak nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="9ca64-181">hello **Configure hello Docker container toobe created** window opens.</span></span>

   ![hello konfigurálása hello Docker tároló létrehozott toobe ablak][PUB08]

9. <span data-ttu-id="9ca64-183">A hello **hello Docker-tároló toobe létrehozott konfigurálása** ablakban adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="9ca64-183">In hello **Configure hello Docker container toobe created** window, provide hello following information:</span></span> 

   <span data-ttu-id="9ca64-184">a.</span><span class="sxs-lookup"><span data-stu-id="9ca64-184">a.</span></span> <span data-ttu-id="9ca64-185">A hello **Docker-tároló neve** mezőben adjon meg egy egyedi nevet a Docker-tároló.</span><span class="sxs-lookup"><span data-stu-id="9ca64-185">In hello **Docker container name** box, enter a unique name for your Docker container.</span></span>

   <span data-ttu-id="9ca64-186">b.</span><span class="sxs-lookup"><span data-stu-id="9ca64-186">b.</span></span> <span data-ttu-id="9ca64-187">A következő Docker képek hello közül választhat:</span><span class="sxs-lookup"><span data-stu-id="9ca64-187">Choose one of hello following Docker images:</span></span> 

      * <span data-ttu-id="9ca64-188">**Előre definiált Docker kép**: Adjon meg egy már meglévő Docker-lemezképet az Azure-ból.</span><span class="sxs-lookup"><span data-stu-id="9ca64-188">**Predefined Docker image**: Specify a pre-existing Docker image from Azure.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="9ca64-189">hello Docker képek ebben a mezőben a lista az számos olyan rendszerkép található, hogy hello Azure eszközkészlet lett konfigurálva toopatch, hogy a rendszer automatikusan telepíti az összetevő.</span><span class="sxs-lookup"><span data-stu-id="9ca64-189">hello list of Docker images in this box consists of several images that hello Azure Toolkit has been configured toopatch so that your artifact is deployed automatically.</span></span> 

      * <span data-ttu-id="9ca64-190">**Egyéni Dockerfile**: Adjon meg egy korábban mentett Dockerfile a helyi számítógépről.</span><span class="sxs-lookup"><span data-stu-id="9ca64-190">**Custom Dockerfile**: Specify a previously saved Dockerfile from your local computer.</span></span>

        > [!NOTE]
        > <span data-ttu-id="9ca64-191">Ez a tulajdonság speciális fejlesztők számára toodeploy saját Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="9ca64-191">This is a more advanced feature for developers who want toodeploy their own Dockerfile.</span></span> <span data-ttu-id="9ca64-192">Azt azonban ez a beállítás tooensure azok Dockerfile megfelelően részét képező használó toodevelopers legfeljebb lesz.</span><span class="sxs-lookup"><span data-stu-id="9ca64-192">However, it is up toodevelopers who use this option tooensure that their Dockerfile is built correctly.</span></span> <span data-ttu-id="9ca64-193">Hello Azure eszközkészlet nem felel meg egy egyéni Dockerfile hello tartalma, mert hello telepítése meghiúsulhat, ha hello Dockerfile problémákkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="9ca64-193">Because hello Azure Toolkit does not validate hello content in a custom Dockerfile, hello deployment might fail if hello Dockerfile has issues.</span></span> <span data-ttu-id="9ca64-194">Ezenkívül hello Azure eszközkészlet hello egyéni Dockerfile toocontain egy webes alkalmazás összetevő vár, mert az megpróbál tooopen HTTP-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="9ca64-194">In addition, because hello Azure Toolkit expects hello custom Dockerfile toocontain a web app artifact, it attempts tooopen an HTTP connection.</span></span> <span data-ttu-id="9ca64-195">Fejlesztők összetevő más típusú közzé, ha azok ártalmatlan hibák fordulhat elő, a telepítés utáni.</span><span class="sxs-lookup"><span data-stu-id="9ca64-195">If developers publish a different type of artifact, they might receive innocuous errors after deployment.</span></span>

   <span data-ttu-id="9ca64-196">c.</span><span class="sxs-lookup"><span data-stu-id="9ca64-196">c.</span></span> <span data-ttu-id="9ca64-197">A hello **portbeállításokat** mezőbe írja be a hello egyedi TCP port kötést a Docker-tároló.</span><span class="sxs-lookup"><span data-stu-id="9ca64-197">In hello **Port settings** box, enter hello unique TCP port binding for your Docker container.</span></span> 

10. <span data-ttu-id="9ca64-198">Kattintson az előző lépésekben hello befejezését követően **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="9ca64-198">After you have completed hello preceding steps, click **Finish**.</span></span> 

<span data-ttu-id="9ca64-199">hello Azure eszközkészlet megkezdi a központi telepítését a webes alkalmazás tooAzure egy Docker-tároló.</span><span class="sxs-lookup"><span data-stu-id="9ca64-199">hello Azure Toolkit begins deploying your web app tooAzure in a Docker container.</span></span> <span data-ttu-id="9ca64-200">Ha beállította az IntelliJ toobe hello háttérben telepített egy **tooAzure telepítése** folyamatjelző sáv jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="9ca64-200">Unless you have configured IntelliJ toobe deployed in hello background, a **Deploying tooAzure** progress bar appears.</span></span> 

![hello telepítés folyamatjelző][PUB09]

<a name="artifacts"></a>
## <a name="additional-information-about-creating-artifacts"></a><span data-ttu-id="9ca64-202">Az összetevők létrehozásával kapcsolatos további információért</span><span class="sxs-lookup"><span data-stu-id="9ca64-202">Additional information about creating artifacts</span></span>

<span data-ttu-id="9ca64-203">a telepítés kész összetevő toocreate hello a következő:</span><span class="sxs-lookup"><span data-stu-id="9ca64-203">toocreate a deployment-ready artifact, do hello following:</span></span>

1. <span data-ttu-id="9ca64-204">Nyissa meg a webalkalmazás-projektet az intellij-t.</span><span class="sxs-lookup"><span data-stu-id="9ca64-204">Open your web app project in IntelliJ.</span></span>

2. <span data-ttu-id="9ca64-205">Kattintson a **fájl**, és kattintson a **szerkezetének**.</span><span class="sxs-lookup"><span data-stu-id="9ca64-205">Click **File**, and then click **Project Structure**.</span></span>

   ![hello szerkezetének parancs][ART01]

3. <span data-ttu-id="9ca64-207">az összetevő, tooadd kattintson hello zöld plusz jel (**+**), és kattintson a **webalkalmazás: archív**.</span><span class="sxs-lookup"><span data-stu-id="9ca64-207">tooadd an artifact, click hello green plus sign (**+**), and then click **Web Application: Archive**.</span></span>

   ![hello ": webalkalmazások archívumából" parancs][ART02]

4. <span data-ttu-id="9ca64-209">A hello **neve** mezőbe írja be a adatösszetevőt nevét (nem tartalmaznak hello *.war* bővítmény), és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="9ca64-209">In hello **Name** box, enter a name for your artifact (do not include hello *.war* extension), and then click **OK**.</span></span>

   ![hello összetevő neve mező][ART03]

<span data-ttu-id="9ca64-211">Az IntelliJ összetevők létrehozásával kapcsolatos további információkért lásd: [összetevők konfigurálása] hello JetBrains webhelyen.</span><span class="sxs-lookup"><span data-stu-id="9ca64-211">For more information about creating artifacts in IntelliJ, see [Configuring artifacts] on hello JetBrains website.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9ca64-212">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9ca64-212">Next steps</span></span>
<span data-ttu-id="9ca64-213">A Java IDEs hello Azure eszközök gazdag kapcsolatos további információkért tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="9ca64-213">For more information about hello Azure Toolkits for Java IDEs, see hello following resources:</span></span>

* <span data-ttu-id="9ca64-214">[Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="9ca64-214">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="9ca64-215">[What's new in hello Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="9ca64-215">[What's new in hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="9ca64-216">[Hello Azure eszköztára Eclipse telepítése]</span><span class="sxs-lookup"><span data-stu-id="9ca64-216">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="9ca64-217">[Hello Azure eszköztára Eclipse bejelentkezési utasítások]</span><span class="sxs-lookup"><span data-stu-id="9ca64-217">[Sign-in instructions for hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="9ca64-218">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]</span><span class="sxs-lookup"><span data-stu-id="9ca64-218">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="9ca64-219">[Az IntelliJ-hez készült Azure-eszközkészlet]</span><span class="sxs-lookup"><span data-stu-id="9ca64-219">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="9ca64-220">[What's new in hello IntelliJ Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="9ca64-220">[What's new in hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="9ca64-221">[Az IntelliJ hello Azure eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="9ca64-221">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="9ca64-222">[Bejelentkezési utasításokat hello Azure eszközkészlet az intellij-t]</span><span class="sxs-lookup"><span data-stu-id="9ca64-222">[Sign-in instructions for hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="9ca64-223">[Hello World webalkalmazás létrehozása az intellij-t az Azure]</span><span class="sxs-lookup"><span data-stu-id="9ca64-223">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="9ca64-224">Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból] és hello [Java Tools for Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="9ca64-224">For more information about using Azure with Java, see hello [Azure Java Developer Center] and hello [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="9ca64-225">További erőforrásokat a Docker, lásd: hello hivatalos [Docker webhely].</span><span class="sxs-lookup"><span data-stu-id="9ca64-225">For additional resources for Docker, see hello official [Docker website].</span></span>

<!-- URL List -->

[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md
[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md
[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Hello Azure eszköztára Eclipse telepítése]: ./azure-toolkit-for-eclipse-installation.md
[Az IntelliJ hello Azure eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md
[Hello Azure eszköztára Eclipse bejelentkezési utasítások]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Bejelentkezési utasításokat hello Azure eszközkészlet az intellij-t]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[What's new in hello Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse-whats-new.md
[What's new in hello IntelliJ Azure eszköztára]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java fejlesztői központ]: https://azure.microsoft.com/develop/java/
[A Visual Studio Team Services Java-eszközök]: https://java.visualstudio.com/

[Docker-webhely]: https://www.docker.com/
[Az összetevők konfigurálása]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

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
