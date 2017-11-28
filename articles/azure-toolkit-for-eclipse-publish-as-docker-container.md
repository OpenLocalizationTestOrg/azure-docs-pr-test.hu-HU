---
title: "Az eclipse-ben az Azure-eszközkészlet használatával egy Docker-tároló közzététele |} Microsoft Docs"
description: "Útmutató a webes alkalmazás a Microsoft Azure is közzé egy Docker-tároló az eclipse-ben az Azure-eszközkészlet használatával."
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
ms.openlocfilehash: 746bb0a073396b84fa8592adda6748a2a5692ee8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="fbda5-103">A webes alkalmazás is egy Docker-tároló közzé a eclipse-ben az Azure-eszközkészlet használatával</span><span class="sxs-lookup"><span data-stu-id="fbda5-103">Publish a web app as a Docker container by using the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="fbda5-104">Docker-tároló széles körben használt módszerrel az webes alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="fbda5-104">Docker containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="fbda5-105">Docker-tárolók használatával a fejlesztők képes egyesíteni a project fájlok és a függőségek egy kiszolgáló üzembe helyezése egyetlen csomagban.</span><span class="sxs-lookup"><span data-stu-id="fbda5-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="fbda5-106">Az Eclipse Azure eszköztára egyszerűbben a Java-fejlesztőknek hozzáadásával *Docker-tároló közzététel* központi telepítést, hogy a Microsoft Azure szolgáltatásainak.</span><span class="sxs-lookup"><span data-stu-id="fbda5-106">The Azure Toolkit for Eclipse simplifies this process for Java developers by adding *Publish as Docker Container* features for deployment to Microsoft Azure.</span></span> <span data-ttu-id="fbda5-107">Ez a cikk végigvezeti az alkalmazások az Azure-bA Docker tárolóként közzétételéhez szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="fbda5-107">This article walks you through the steps required to publish your applications to Azure as Docker containers.</span></span>

> [!NOTE]
> <span data-ttu-id="fbda5-108">További információ a Docker érhető el a [Docker webhely].</span><span class="sxs-lookup"><span data-stu-id="fbda5-108">More information about Docker is available on the [Docker website].</span></span>
>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="fbda5-109">A webalkalmazás közzététele az Azure-bA egy Docker-tároló használatával</span><span class="sxs-lookup"><span data-stu-id="fbda5-109">Publish your web app to Azure by using a Docker container</span></span>

1. <span data-ttu-id="fbda5-110">Nyissa meg a webalkalmazás-projektet az eclipse-ben.</span><span class="sxs-lookup"><span data-stu-id="fbda5-110">Open your web app project in Eclipse.</span></span>

2. <span data-ttu-id="fbda5-111">Elindítani a **Docker-tároló közzététel** varázsló, tegye a következők egyikét:</span><span class="sxs-lookup"><span data-stu-id="fbda5-111">To start the **Publish as Docker Container** wizard, do either of the following:</span></span>

   * <span data-ttu-id="fbda5-112">Az a **Navigator** megtekinteni, kattintson jobb gombbal a projektre, kattintson a **Azure**, és kattintson a **Docker-tároló közzététel**.</span><span class="sxs-lookup"><span data-stu-id="fbda5-112">In the **Navigator** view, right-click your project, click **Azure**, and then click **Publish as Docker Container**.</span></span>

      ![Közzététel Docker-tároló parancsként Navigator megtekintése][PUB01]

   * <span data-ttu-id="fbda5-114">Az Eclipse eszköztáron kattintson a **közzététel** gombra, majd **Docker-tároló közzététel**.</span><span class="sxs-lookup"><span data-stu-id="fbda5-114">On the Eclipse toolbar, click the **Publish** button, and then click **Publish as Docker Container**.</span></span>

      ![Közzététel Docker-tároló parancsként eclipse eszköztár][PUB02]
      
    <span data-ttu-id="fbda5-116">A **Docker-tároló központi telepítése az Azure-on** varázsló megnyílik.</span><span class="sxs-lookup"><span data-stu-id="fbda5-116">The **Deploy Docker Container on Azure** wizard opens.</span></span>

    ![A központi telepítése Docker-tároló Azure varázsló][PUB03]

3. <span data-ttu-id="fbda5-118">Az a **adjon meg egy kép nevet, válassza ki a összetevő elérési útja, ellenőrizze a használt Docker állomás** ablakban tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="fbda5-118">In the **Type an image name, select the artifact's path and check a Docker host to be used** window, do the following:</span></span>

    <span data-ttu-id="fbda5-119">a.</span><span class="sxs-lookup"><span data-stu-id="fbda5-119">a.</span></span> <span data-ttu-id="fbda5-120">Az a **Docker lemezképnév** mezőben adjon meg egy egyedi nevet a Docker-állomás.</span><span class="sxs-lookup"><span data-stu-id="fbda5-120">In the **Docker image name** box, enter a unique name for your Docker host.</span></span> <span data-ttu-id="fbda5-121">(A varázsló automatikusan létrehozza a nevét, de módosíthatja.)</span><span class="sxs-lookup"><span data-stu-id="fbda5-121">(The wizard automatically creates a name, but you can modify it.)</span></span>

    <span data-ttu-id="fbda5-122">b.</span><span class="sxs-lookup"><span data-stu-id="fbda5-122">b.</span></span> <span data-ttu-id="fbda5-123">A **állomások** terület tartalmazza a Docker állomásokon, már létrehozott.</span><span class="sxs-lookup"><span data-stu-id="fbda5-123">The **Hosts** area displays any Docker hosts that you have already created.</span></span> <span data-ttu-id="fbda5-124">A következő lehetőségek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="fbda5-124">Do either of the following:</span></span>

    * <span data-ttu-id="fbda5-125">Ha egy meglévő Docker-állomás, a webes alkalmazás telepíthet rá.</span><span class="sxs-lookup"><span data-stu-id="fbda5-125">If you have an existing Docker host, you can deploy your web app to it.</span></span>
    * <span data-ttu-id="fbda5-126">Hozzon létre egy új Docker-állomás, kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="fbda5-126">To create a new Docker host, click **Add**.</span></span>  
      
    <span data-ttu-id="fbda5-127">A **Docker állomás létrehozása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fbda5-127">The **Create Docker Host** dialog box opens.</span></span>

    ![Az Azure varázsló Docker-tároló telepítése][PUB04a]

4. <span data-ttu-id="fbda5-129">Az a **állítsa be az új virtuális gép** ablakban adja meg a következő beállítások érhetők el a Docker-állomás.</span><span class="sxs-lookup"><span data-stu-id="fbda5-129">In the **Configure the new virtual machine** window, specify the following options for your Docker host.</span></span> <span data-ttu-id="fbda5-130">(A varázsló automatikusan létrehozza a lehetőségek a legtöbb, de egyikük módosíthatja.)</span><span class="sxs-lookup"><span data-stu-id="fbda5-130">(The wizard automatically generates most of the options for you, but you can modify any of them.)</span></span>

   <span data-ttu-id="fbda5-131">a.</span><span class="sxs-lookup"><span data-stu-id="fbda5-131">a.</span></span> <span data-ttu-id="fbda5-132">**Név**: Adjon meg egy egyedi nevet a Docker-állomás.</span><span class="sxs-lookup"><span data-stu-id="fbda5-132">**Name**: Enter a unique name for the Docker host.</span></span> <span data-ttu-id="fbda5-133">(Nincs ugyanaz, mint a korábban megadott Docker lemezképnév.)</span><span class="sxs-lookup"><span data-stu-id="fbda5-133">(It is not the same as the Docker image name that you specified earlier.)</span></span>

   <span data-ttu-id="fbda5-134">b.</span><span class="sxs-lookup"><span data-stu-id="fbda5-134">b.</span></span> <span data-ttu-id="fbda5-135">**Előfizetés**: Adja meg az Azure-előfizetés, amelyekkel az állomáshoz.</span><span class="sxs-lookup"><span data-stu-id="fbda5-135">**Subscription**: Enter the Azure subscription that you use for your host.</span></span>

   <span data-ttu-id="fbda5-136">c.</span><span class="sxs-lookup"><span data-stu-id="fbda5-136">c.</span></span> <span data-ttu-id="fbda5-137">**A régióban**: Adja meg a földrajzi régió, ahol az állomás.</span><span class="sxs-lookup"><span data-stu-id="fbda5-137">**Region**: Enter the geographical region where your host is located.</span></span>

   <span data-ttu-id="fbda5-138">d.</span><span class="sxs-lookup"><span data-stu-id="fbda5-138">d.</span></span> <span data-ttu-id="fbda5-139">Az a **állomás operációs rendszer és a méret** lapon:</span><span class="sxs-lookup"><span data-stu-id="fbda5-139">On the **Host OS and Size** tab:</span></span>
     * <span data-ttu-id="fbda5-140">**Az operációs rendszer üzemeltetéséhez**: Adja meg az operációs rendszer a virtuális gép, amely tartalmazza a gazdagépet.</span><span class="sxs-lookup"><span data-stu-id="fbda5-140">**Host OS**: Enter the operating system for the virtual machine that contains your host.</span></span>
     * <span data-ttu-id="fbda5-141">**Méret**: Adja meg a virtuális gép méretét az állomáshoz.</span><span class="sxs-lookup"><span data-stu-id="fbda5-141">**Size**: Enter the virtual-machine size for your host.</span></span>

   <span data-ttu-id="fbda5-142">e.</span><span class="sxs-lookup"><span data-stu-id="fbda5-142">e.</span></span> <span data-ttu-id="fbda5-143">Az a **erőforráscsoport** lapon:</span><span class="sxs-lookup"><span data-stu-id="fbda5-143">On the **Resource Group** tab:</span></span>
     * <span data-ttu-id="fbda5-144">**Új erőforráscsoport**: hozzon létre egy új erőforráscsoportot az állomáshoz.</span><span class="sxs-lookup"><span data-stu-id="fbda5-144">**New resource group**: Create a new resource group for your host.</span></span>
     * <span data-ttu-id="fbda5-145">**Meglévő erőforráscsoport**: Írjon be egy meglévő erőforráscsoportot az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="fbda5-145">**Existing resource group**: Enter an existing resource group from your Azure account.</span></span>

   <span data-ttu-id="fbda5-146">f.</span><span class="sxs-lookup"><span data-stu-id="fbda5-146">f.</span></span> <span data-ttu-id="fbda5-147">Az a **hálózati** lapon:</span><span class="sxs-lookup"><span data-stu-id="fbda5-147">On the **Network** tab:</span></span>
     * <span data-ttu-id="fbda5-148">**Új virtuális hálózat**: új virtuális hálózat létrehozása az állomáshoz.</span><span class="sxs-lookup"><span data-stu-id="fbda5-148">**New virtual network**: Create a new virtual network for your host.</span></span>
     * <span data-ttu-id="fbda5-149">**Meglévő virtuális hálózat**: Írjon be egy meglévő virtuális hálózatot az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="fbda5-149">**Existing virtual network**: Enter an existing virtual network from your Azure account.</span></span>

   <span data-ttu-id="fbda5-150">g.</span><span class="sxs-lookup"><span data-stu-id="fbda5-150">g.</span></span> <span data-ttu-id="fbda5-151">Az a **tárolási** lapon:</span><span class="sxs-lookup"><span data-stu-id="fbda5-151">On the **Storage** tab:</span></span>
     * <span data-ttu-id="fbda5-152">**Új tárfiók**: hozzon létre egy új tárfiókot az állomáshoz.</span><span class="sxs-lookup"><span data-stu-id="fbda5-152">**New storage account**: Create a new storage account for your host.</span></span>
     * <span data-ttu-id="fbda5-153">**Meglévő tárfiók**: Írjon be egy meglévő tárfiókot az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="fbda5-153">**Existing storage account**: Enter an existing storage account from your Azure account.</span></span>

5. <span data-ttu-id="fbda5-154">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="fbda5-154">Click **Next**.</span></span>

6. <span data-ttu-id="fbda5-155">Az a **napló konfigurálja a hitelesítő adatokat, és a portbeállítások** ablakban, a következő lehetőségek közül:</span><span class="sxs-lookup"><span data-stu-id="fbda5-155">In the **Configure log in credentials and port settings** window, select one of the following options:</span></span>

    * <span data-ttu-id="fbda5-156">**Hitelesítő adatok importálása az Azure Key Vault**: egy korábban mentett kumulatív hitelesítő adatokat, amelyek az Azure-előfizetéshez vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="fbda5-156">**Import credentials from Azure Key Vault**: Specifies a previously saved set of credentials that are stored in your Azure subscription.</span></span>

      >[!NOTE]
      ><span data-ttu-id="fbda5-157">Egy Azure Key Vault, amely egy meghatározott fiók vagy a szolgáltatás egyszerű nincs egy másik fiókot vagy egyszerű szolgáltatásnév osztja meg az előfizetés által automatikusan elérhető.</span><span class="sxs-lookup"><span data-stu-id="fbda5-157">An Azure Key Vault that's created with a specific account or service principal is not automatically accessible by another account or service principal that shares the subscription.</span></span> <span data-ttu-id="fbda5-158">Egy másik fiókot, vagy a szolgáltatás lehetővé teszi a Key Vault használata egyszerű, az Azure-portálon kell használnia a fiókot, vagy az egyszerű szolgáltatásnév hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="fbda5-158">To allow another account or service principal to use the Key Vault, you must use the Azure portal to add the account or service principal.</span></span>

    * <span data-ttu-id="fbda5-159">**Új bejelentkezési hitelesítő adatok**: hoz létre új bejelentkezési hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="fbda5-159">**New log in credentials**: Creates a new set of login credentials.</span></span> <span data-ttu-id="fbda5-160">Ha ezt a lehetőséget választja, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="fbda5-160">If you select this option, do the following:</span></span>
    
      * <span data-ttu-id="fbda5-161">Az a **VM hitelesítő adatok** adja meg a virtuális gép bejelentkezési hitelesítő adatok a Docker-állomás beállítások a következők egyikét:</span><span class="sxs-lookup"><span data-stu-id="fbda5-161">On the **VM Credentials** tab, choose one of the following options for the virtual-machine login credentials of your Docker host:</span></span>

          * <span data-ttu-id="fbda5-162">**Felhasználónév**: Adja meg a felhasználónevet a virtuális gép bejelentkezési hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="fbda5-162">**Username**: Enter the username for your virtual machine login credentials.</span></span>
          * <span data-ttu-id="fbda5-163">**Jelszó** és **megerősítése**: Adja meg a jelszót a virtuális gép bejelentkezési hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="fbda5-163">**Password** and **Confirm**: Enter the password for your virtual machine login credentials.</span></span>
          * <span data-ttu-id="fbda5-164">**SSH**: Adja meg a Secure Shell (SSH) beállításait a Docker-állomás.</span><span class="sxs-lookup"><span data-stu-id="fbda5-164">**SSH**: Enter the Secure Shell (SSH) settings for your Docker host.</span></span> <span data-ttu-id="fbda5-165">Az alábbi lehetőségek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="fbda5-165">You can choose from the following options:</span></span>
            * <span data-ttu-id="fbda5-166">**Nincs**: meghatározza, hogy a virtuális gép nem teszi lehetővé SSH-kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="fbda5-166">**None**: Specifies that your virtual machine will not allow SSH connections.</span></span>
            * <span data-ttu-id="fbda5-167">**Automatikus generálása**: automatikusan létrehozza a szükséges beállítások SSH-kapcsolaton keresztül csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="fbda5-167">**Auto-generate**: Automatically creates the requisite settings for connecting via SSH.</span></span>
            * <span data-ttu-id="fbda5-168">**Importálás a directory**: Adja meg a korábban mentett SSH-beállítások készletét tartalmazó könyvtár.</span><span class="sxs-lookup"><span data-stu-id="fbda5-168">**Import from directory**: Specifies a directory that contains a set of previously saved SSH settings.</span></span> <span data-ttu-id="fbda5-169">A könyvtár a következő két fájlt kell tartalmaznia:</span><span class="sxs-lookup"><span data-stu-id="fbda5-169">The directory must contain the following two files:</span></span>
                * <span data-ttu-id="fbda5-170">*id_rsa*: a felhasználó RSA azonosítóját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fbda5-170">*id_rsa*: Contains the RSA identification for a user.</span></span>
                * <span data-ttu-id="fbda5-171">*id_rsa.pub*: a hitelesítéshez használt RSA nyilvános kulcsot tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fbda5-171">*id_rsa.pub*: Contains the RSA public key that is used for authentication.</span></span>
        
        ![Docker állomás létrehozása][PUB05]

      * <span data-ttu-id="fbda5-173">Az a **Docker démon hitelesítő adatok** lapra, adja meg a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="fbda5-173">On the **Docker Daemon Credentials** tab, specify the following options:</span></span>

          * <span data-ttu-id="fbda5-174">**Docker démon port**: Adja meg az egyedi TCP-port a Docker-állomás.</span><span class="sxs-lookup"><span data-stu-id="fbda5-174">**Docker Daemon port**: Enter the unique TCP port for your Docker host.</span></span>
          * <span data-ttu-id="fbda5-175">**A TLS biztonsági**: Adja meg a Transport Layer Security-beállításokat a Docker-állomás.</span><span class="sxs-lookup"><span data-stu-id="fbda5-175">**TLS Security**: Enter the Transport Layer Security settings for your Docker host.</span></span> <span data-ttu-id="fbda5-176">Az alábbi lehetőségek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="fbda5-176">You can choose from the following options:</span></span>
            * <span data-ttu-id="fbda5-177">**Nincs**: Megadja, hogy a virtuális gép nem engedélyezi TLS-kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="fbda5-177">**None**: Specifies that your virtual machine will not allow TLS connections.</span></span>
            * <span data-ttu-id="fbda5-178">**Automatikus generálása**: automatikusan létrehozza a szükséges beállítások TLS keresztül kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="fbda5-178">**Auto-generate**: Automatically creates the requisite settings for connecting via TLS.</span></span>
            * <span data-ttu-id="fbda5-179">**Importálás a directory**: Adja meg a korábban mentett TLS beállítások készletét tartalmazó könyvtár.</span><span class="sxs-lookup"><span data-stu-id="fbda5-179">**Import from directory**: Specifies a directory that contains a set of previously saved TLS settings.</span></span> <span data-ttu-id="fbda5-180">Pontosabban a könyvtár a következő hat fájlokat kell tartalmaznia:</span><span class="sxs-lookup"><span data-stu-id="fbda5-180">More specifically, the directory must contain the following six files:</span></span>
                * <span data-ttu-id="fbda5-181">*CA.PEM* és *ca-key.pem*: az a TLS-hitelesítésszolgáltató a tanúsítvány és nyilvános kulcs található.</span><span class="sxs-lookup"><span data-stu-id="fbda5-181">*ca.pem* and *ca-key.pem*: Contain the certificate and public key for the TLS Certificate Authority.</span></span>
                * <span data-ttu-id="fbda5-182">*CERT.PEM* és *key.pem*: az ügyfél-tanúsítványt és a TLS hitelesítésre használt nyilvános kulcsot tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fbda5-182">*cert.pem* and *key.pem*: Contain the client certificate and public key that is used for TLS authentication.</span></span>
                * <span data-ttu-id="fbda5-183">*Server.PEM* és *kiszolgáló-key.pem*: a kiszolgálói tanúsítvány és nyilvános kulcs tartalmaznia ahhoz, hogy a gazdagép.</span><span class="sxs-lookup"><span data-stu-id="fbda5-183">*server.pem* and *server-key.pem*: Contain the server certificate and public key for the host.</span></span>

        ![Docker állomás létrehozása][PUB06]

7. <span data-ttu-id="fbda5-185">Miután megadta a fenti adatokat, kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="fbda5-185">After you have entered all of the preceding information, click **Finish**.</span></span>

8. <span data-ttu-id="fbda5-186">Az a **Docker-tároló központi telepítése az Azure-on** varázsló, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="fbda5-186">In the **Deploy Docker Container on Azure** wizard, click **Next**.</span></span>

   ![A központi telepítése Docker-tároló Azure varázsló][PUB07]

9. <span data-ttu-id="fbda5-188">Az a **létrehozni a Docker-tároló konfigurálása** ablakban tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="fbda5-188">In the **Configure the Docker container to be created** window, do the following:</span></span>

   <span data-ttu-id="fbda5-189">a.</span><span class="sxs-lookup"><span data-stu-id="fbda5-189">a.</span></span> <span data-ttu-id="fbda5-190">Az a **Docker-tároló neve** mezőben adjon meg egy egyedi nevet a Docker-tároló.</span><span class="sxs-lookup"><span data-stu-id="fbda5-190">In the **Docker container name** box, enter a unique name for your Docker container.</span></span>

   <span data-ttu-id="fbda5-191">b.</span><span class="sxs-lookup"><span data-stu-id="fbda5-191">b.</span></span> <span data-ttu-id="fbda5-192">A következő Docker-rendszerképek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="fbda5-192">Choose one of the following Docker images:</span></span>
     * <span data-ttu-id="fbda5-193">**Előre definiált Docker kép**: Megadja a már meglévő Docker-lemezkép az Azure-ból.</span><span class="sxs-lookup"><span data-stu-id="fbda5-193">**Predefined Docker image**: Specifies a pre-existing Docker image from Azure.</span></span>

       >[!NOTE]
       ><span data-ttu-id="fbda5-194">A Docker képek ebben a mezőben a lista az számos olyan rendszerkép található, amely az Azure-eszközkészlet lett konfigurálva, hogy a rendszer automatikusan telepíti az összetevő javítás.</span><span class="sxs-lookup"><span data-stu-id="fbda5-194">The list of Docker images in this box consists of several images that the Azure Toolkit has been configured to patch so that your artifact is deployed automatically.</span></span>

     * <span data-ttu-id="fbda5-195">**Egyéni Dockerfile**: a helyi számítógépről egy korábban mentett Dockerfile határozza meg.</span><span class="sxs-lookup"><span data-stu-id="fbda5-195">**Custom Dockerfile**: Specifies a previously saved Dockerfile from your local computer.</span></span>

       >[!NOTE]
       ><span data-ttu-id="fbda5-196">Ez a tulajdonság speciális fejlesztők számára a saját Dockerfile telepíteni kívánja.</span><span class="sxs-lookup"><span data-stu-id="fbda5-196">This is a more advanced feature for developers who want to deploy their own Dockerfile.</span></span> <span data-ttu-id="fbda5-197">Azonban esetén a fejlesztők számára ez a beállítás segítségével győződjön meg arról, hogy azok Dockerfile van megfelelően lett felépítve.</span><span class="sxs-lookup"><span data-stu-id="fbda5-197">However, it is up to developers who use this option to ensure that their Dockerfile is built correctly.</span></span> <span data-ttu-id="fbda5-198">Az Azure-eszközkészlet nem felel meg egy egyéni Dockerfile tartalma, a telepítés esetleg sikertelen lesz, ha a Dockerfile problémákkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="fbda5-198">The Azure Toolkit does not validate the content in a custom Dockerfile, so the deployment might fail if the Dockerfile has issues.</span></span> <span data-ttu-id="fbda5-199">Ezenkívül az Azure-eszközkészlet vár az egyéni Dockerfile magában foglalja a webes alkalmazás-összetevő, és megkísérli nyissa meg a HTTP-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="fbda5-199">In addition, the Azure Toolkit expects the custom Dockerfile to contain a web app artifact, and it will attempt to open an HTTP connection.</span></span> <span data-ttu-id="fbda5-200">Ha a fejlesztők közzététele összetevő más típusú, a telepítés utáni jelenhet ártalmatlan hibák.</span><span class="sxs-lookup"><span data-stu-id="fbda5-200">If developers publish a different type of artifact, they may receive innocuous errors after deployment.</span></span>

   <span data-ttu-id="fbda5-201">c.</span><span class="sxs-lookup"><span data-stu-id="fbda5-201">c.</span></span> <span data-ttu-id="fbda5-202">**Portbeállításokat**: Adja meg a Docker-tároló egyedi TCP-port kötés.</span><span class="sxs-lookup"><span data-stu-id="fbda5-202">**Port settings**: Enter the unique TCP port binding for your Docker container.</span></span>

     ![A létrehozandó ablakban a Docker-tároló konfigurálása][PUB08]

10. <span data-ttu-id="fbda5-204">Miután elvégezte az előző lépéseket, kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="fbda5-204">After you have completed all of the preceding steps, click **Finish**.</span></span>

<span data-ttu-id="fbda5-205">Az Azure-eszközkészlet megkezdi a központi telepítését a webalkalmazás Azure egy Docker-tároló.</span><span class="sxs-lookup"><span data-stu-id="fbda5-205">The Azure Toolkit begins deploying your web app to Azure in a Docker container.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="fbda5-206">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fbda5-206">Next steps</span></span>
<span data-ttu-id="fbda5-207">A Java IDEs az Azure eszközök gazdag kapcsolatos további információkért lásd a következőket:</span><span class="sxs-lookup"><span data-stu-id="fbda5-207">For more information about the Azure Toolkits for Java IDEs, see the following resources:</span></span>

* <span data-ttu-id="fbda5-208">[Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="fbda5-208">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="fbda5-209">[What's new in Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="fbda5-209">[What's new in the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="fbda5-210">[Az Eclipse-hez készült Azure-eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="fbda5-210">[Installing the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="fbda5-211">[Az Eclipse Azure eszköztára bejelentkezési utasítások]</span><span class="sxs-lookup"><span data-stu-id="fbda5-211">[Sign-in instructions for the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="fbda5-212">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]</span><span class="sxs-lookup"><span data-stu-id="fbda5-212">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="fbda5-213">[Az IntelliJ-hez készült Azure-eszközkészlet]</span><span class="sxs-lookup"><span data-stu-id="fbda5-213">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="fbda5-214">[What's new in IntelliJ Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="fbda5-214">[What's new in the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="fbda5-215">[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="fbda5-215">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="fbda5-216">[Bejelentkezési utasításokat az Azure-eszközkészlet az intellij-t]</span><span class="sxs-lookup"><span data-stu-id="fbda5-216">[Sign-in instructions for the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="fbda5-217">[Hello World webalkalmazás létrehozása az intellij-t az Azure]</span><span class="sxs-lookup"><span data-stu-id="fbda5-217">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="fbda5-218">Azure Java használatával kapcsolatos további információkért lásd: [Azure Java fejlesztői központból] és [Java Tools for Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="fbda5-218">For more information about using Azure with Java, see [Azure Java Developer Center] and [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="fbda5-219">További erőforrásokat a Docker, tekintse meg a hivatalos [Docker webhely].</span><span class="sxs-lookup"><span data-stu-id="fbda5-219">For additional resources for Docker, see the official [Docker website].</span></span>

<!-- URL List -->

<span data-ttu-id="fbda5-220">[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md</span><span class="sxs-lookup"><span data-stu-id="fbda5-220">[Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse.md</span></span>
<span data-ttu-id="fbda5-221">[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md</span><span class="sxs-lookup"><span data-stu-id="fbda5-221">[Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij.md</span></span>
<span data-ttu-id="fbda5-222">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="fbda5-222">[Create a Hello World web app for Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span></span>
<span data-ttu-id="fbda5-223">[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="fbda5-223">[Create a Hello World web app for Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span></span>
<span data-ttu-id="fbda5-224">[Az Eclipse-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-eclipse-installation.md</span><span class="sxs-lookup"><span data-stu-id="fbda5-224">[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span></span>
<span data-ttu-id="fbda5-225">[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md</span><span class="sxs-lookup"><span data-stu-id="fbda5-225">[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span></span>
<span data-ttu-id="fbda5-226">[Az Eclipse Azure eszköztára bejelentkezési utasítások]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="fbda5-226">[Sign-in instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span></span>
<span data-ttu-id="fbda5-227">[Bejelentkezési utasításokat az Azure-eszközkészlet az intellij-t]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="fbda5-227">[Sign-in instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span></span>
<span data-ttu-id="fbda5-228">[What's new in Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="fbda5-228">[What's new in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span></span>
<span data-ttu-id="fbda5-229">[What's new in IntelliJ Azure eszköztára]: ./azure-toolkit-for-intellij-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="fbda5-229">[What's new in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span></span>

<span data-ttu-id="fbda5-230">[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="fbda5-230">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="fbda5-231">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="fbda5-231">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>

<span data-ttu-id="fbda5-232">[Docker webhely]: https://www.docker.com/</span><span class="sxs-lookup"><span data-stu-id="fbda5-232">[Docker website]: https://www.docker.com/</span></span>

<!-- IMG List -->

[PUB01]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB01.png
[PUB02]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB02.png
[PUB03]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB03.png
[PUB04a]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04a.png
[PUB04b]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04b.png
[PUB04c]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04c.png
[PUB04d]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04d.png
[PUB05]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB05.png
[PUB06]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB06.png
[PUB07]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB07.png
[PUB08]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB08.png