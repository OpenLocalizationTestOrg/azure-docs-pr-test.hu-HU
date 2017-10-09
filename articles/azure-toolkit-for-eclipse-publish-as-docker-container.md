---
title: "egy Docker-tároló segítségével aaaPublish hello Azure eszköztára Eclipse |} Microsoft Docs"
description: "Megtudhatja, hogyan toopublish egy webes alkalmazás tooMicrosoft Azure-bA egy Docker-tároló segítségével hello Azure eszköztára eclipse-ben."
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
ms.openlocfilehash: 53ec3a7f7a171691024e03622fd48d6f1e257b50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-hello-azure-toolkit-for-eclipse"></a><span data-ttu-id="3911d-103">A webes alkalmazás is egy Docker-tároló közzé a Eclipse hello Azure eszközkészlet használatával</span><span class="sxs-lookup"><span data-stu-id="3911d-103">Publish a web app as a Docker container by using hello Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="3911d-104">Docker-tároló széles körben használt módszerrel az webes alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="3911d-104">Docker containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="3911d-105">Docker-tárolók használatával a fejlesztők képes egyesíteni a project fájlok és a függőségek telepítési tooa kiszolgáló egyetlen csomagban.</span><span class="sxs-lookup"><span data-stu-id="3911d-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment tooa server.</span></span> <span data-ttu-id="3911d-106">hello Azure eszköztára Eclipse egyszerűbben a Java-fejlesztőknek hozzáadásával *Docker-tároló közzététel* telepítési tooMicrosoft Azure szolgáltatásai.</span><span class="sxs-lookup"><span data-stu-id="3911d-106">hello Azure Toolkit for Eclipse simplifies this process for Java developers by adding *Publish as Docker Container* features for deployment tooMicrosoft Azure.</span></span> <span data-ttu-id="3911d-107">Ez a cikk bemutatja, hogyan hello lépéseket szükséges toopublish az alkalmazások tooAzure Docker tárolóként.</span><span class="sxs-lookup"><span data-stu-id="3911d-107">This article walks you through hello steps required toopublish your applications tooAzure as Docker containers.</span></span>

> [!NOTE]
> <span data-ttu-id="3911d-108">További információ a Docker érhető el hello [Docker webhely].</span><span class="sxs-lookup"><span data-stu-id="3911d-108">More information about Docker is available on hello [Docker website].</span></span>
>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="publish-your-web-app-tooazure-by-using-a-docker-container"></a><span data-ttu-id="3911d-109">A webes alkalmazás tooAzure közzététele egy Docker-tároló használatával</span><span class="sxs-lookup"><span data-stu-id="3911d-109">Publish your web app tooAzure by using a Docker container</span></span>

1. <span data-ttu-id="3911d-110">Nyissa meg a webalkalmazás-projektet az eclipse-ben.</span><span class="sxs-lookup"><span data-stu-id="3911d-110">Open your web app project in Eclipse.</span></span>

2. <span data-ttu-id="3911d-111">toostart hello **Docker-tároló közzététel** varázsló hello alábbi módszerekkel:</span><span class="sxs-lookup"><span data-stu-id="3911d-111">toostart hello **Publish as Docker Container** wizard, do either of hello following:</span></span>

   * <span data-ttu-id="3911d-112">A hello **Navigator** megtekinteni, kattintson jobb gombbal a projektre, kattintson a **Azure**, és kattintson a **Docker-tároló közzététel**.</span><span class="sxs-lookup"><span data-stu-id="3911d-112">In hello **Navigator** view, right-click your project, click **Azure**, and then click **Publish as Docker Container**.</span></span>

      ![Közzététel Docker-tároló parancsként Navigator megtekintése][PUB01]

   * <span data-ttu-id="3911d-114">Hello Eclipse eszköztáron kattintson a hello **közzététel** gombra, majd **Docker-tároló közzététel**.</span><span class="sxs-lookup"><span data-stu-id="3911d-114">On hello Eclipse toolbar, click hello **Publish** button, and then click **Publish as Docker Container**.</span></span>

      ![Közzététel Docker-tároló parancsként eclipse eszköztár][PUB02]
      
    <span data-ttu-id="3911d-116">Hello **Docker-tároló központi telepítése az Azure-on** varázsló megnyílik.</span><span class="sxs-lookup"><span data-stu-id="3911d-116">hello **Deploy Docker Container on Azure** wizard opens.</span></span>

    ![hello Azure varázsló a Docker-tároló központi telepítése][PUB03]

3. <span data-ttu-id="3911d-118">A hello **írja be a lemezkép nevét, válassza ki a hello összetevő elérési útja, ellenőrizze a használt Docker állomás toobe** ablakban, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="3911d-118">In hello **Type an image name, select hello artifact's path and check a Docker host toobe used** window, do hello following:</span></span>

    <span data-ttu-id="3911d-119">a.</span><span class="sxs-lookup"><span data-stu-id="3911d-119">a.</span></span> <span data-ttu-id="3911d-120">A hello **Docker lemezképnév** mezőben adjon meg egy egyedi nevet a Docker-állomás.</span><span class="sxs-lookup"><span data-stu-id="3911d-120">In hello **Docker image name** box, enter a unique name for your Docker host.</span></span> <span data-ttu-id="3911d-121">(hello varázsló automatikusan létrehozza a nevét, de módosíthatja.)</span><span class="sxs-lookup"><span data-stu-id="3911d-121">(hello wizard automatically creates a name, but you can modify it.)</span></span>

    <span data-ttu-id="3911d-122">b.</span><span class="sxs-lookup"><span data-stu-id="3911d-122">b.</span></span> <span data-ttu-id="3911d-123">Hello **állomások** terület tartalmazza a Docker állomásokon, már létrehozott.</span><span class="sxs-lookup"><span data-stu-id="3911d-123">hello **Hosts** area displays any Docker hosts that you have already created.</span></span> <span data-ttu-id="3911d-124">Hello alábbi módszerekkel:</span><span class="sxs-lookup"><span data-stu-id="3911d-124">Do either of hello following:</span></span>

    * <span data-ttu-id="3911d-125">Ha egy meglévő Docker-állomás, a webes alkalmazás tooit telepítheti.</span><span class="sxs-lookup"><span data-stu-id="3911d-125">If you have an existing Docker host, you can deploy your web app tooit.</span></span>
    * <span data-ttu-id="3911d-126">Kattintson egy új Docker állomás toocreate **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="3911d-126">toocreate a new Docker host, click **Add**.</span></span>  
      
    <span data-ttu-id="3911d-127">Hello **Docker állomás létrehozása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3911d-127">hello **Create Docker Host** dialog box opens.</span></span>

    ![Az Azure varázsló Docker-tároló telepítése][PUB04a]

4. <span data-ttu-id="3911d-129">A hello **hello új virtuális gép konfigurálása** ablakban adja meg az alábbi beállítások a Docker állomás hello.</span><span class="sxs-lookup"><span data-stu-id="3911d-129">In hello **Configure hello new virtual machine** window, specify hello following options for your Docker host.</span></span> <span data-ttu-id="3911d-130">(hello varázsló automatikusan létrehozza a legtöbb hello lehetőségek, de egyikük módosíthatja.)</span><span class="sxs-lookup"><span data-stu-id="3911d-130">(hello wizard automatically generates most of hello options for you, but you can modify any of them.)</span></span>

   <span data-ttu-id="3911d-131">a.</span><span class="sxs-lookup"><span data-stu-id="3911d-131">a.</span></span> <span data-ttu-id="3911d-132">**Név**: Adjon meg egy egyedi nevet hello Docker állomás.</span><span class="sxs-lookup"><span data-stu-id="3911d-132">**Name**: Enter a unique name for hello Docker host.</span></span> <span data-ttu-id="3911d-133">(Fontos nem ugyanaz, mint a korábban megadott Docker lemezképnév hello hello.)</span><span class="sxs-lookup"><span data-stu-id="3911d-133">(It is not hello same as hello Docker image name that you specified earlier.)</span></span>

   <span data-ttu-id="3911d-134">b.</span><span class="sxs-lookup"><span data-stu-id="3911d-134">b.</span></span> <span data-ttu-id="3911d-135">**Előfizetés**: Adja meg a hello Azure-előfizetéssel, amelyekkel az állomáshoz.</span><span class="sxs-lookup"><span data-stu-id="3911d-135">**Subscription**: Enter hello Azure subscription that you use for your host.</span></span>

   <span data-ttu-id="3911d-136">c.</span><span class="sxs-lookup"><span data-stu-id="3911d-136">c.</span></span> <span data-ttu-id="3911d-137">**A régióban**: hello földrajzi régiót, ahol az állomás-e meg.</span><span class="sxs-lookup"><span data-stu-id="3911d-137">**Region**: Enter hello geographical region where your host is located.</span></span>

   <span data-ttu-id="3911d-138">d.</span><span class="sxs-lookup"><span data-stu-id="3911d-138">d.</span></span> <span data-ttu-id="3911d-139">A hello **állomás operációs rendszer és a méret** lapon:</span><span class="sxs-lookup"><span data-stu-id="3911d-139">On hello **Host OS and Size** tab:</span></span>
     * <span data-ttu-id="3911d-140">**Az operációs rendszer üzemeltetéséhez**: hello operációs rendszer hello virtuális gépet, amely tartalmazza a gazdagépet adjon meg.</span><span class="sxs-lookup"><span data-stu-id="3911d-140">**Host OS**: Enter hello operating system for hello virtual machine that contains your host.</span></span>
     * <span data-ttu-id="3911d-141">**Méret**: hello virtuális gép méretét adja meg a gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="3911d-141">**Size**: Enter hello virtual-machine size for your host.</span></span>

   <span data-ttu-id="3911d-142">e.</span><span class="sxs-lookup"><span data-stu-id="3911d-142">e.</span></span> <span data-ttu-id="3911d-143">A hello **erőforráscsoport** lapon:</span><span class="sxs-lookup"><span data-stu-id="3911d-143">On hello **Resource Group** tab:</span></span>
     * <span data-ttu-id="3911d-144">**Új erőforráscsoport**: hozzon létre egy új erőforráscsoportot az állomáshoz.</span><span class="sxs-lookup"><span data-stu-id="3911d-144">**New resource group**: Create a new resource group for your host.</span></span>
     * <span data-ttu-id="3911d-145">**Meglévő erőforráscsoport**: Írjon be egy meglévő erőforráscsoportot az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="3911d-145">**Existing resource group**: Enter an existing resource group from your Azure account.</span></span>

   <span data-ttu-id="3911d-146">f.</span><span class="sxs-lookup"><span data-stu-id="3911d-146">f.</span></span> <span data-ttu-id="3911d-147">A hello **hálózati** lapon:</span><span class="sxs-lookup"><span data-stu-id="3911d-147">On hello **Network** tab:</span></span>
     * <span data-ttu-id="3911d-148">**Új virtuális hálózat**: új virtuális hálózat létrehozása az állomáshoz.</span><span class="sxs-lookup"><span data-stu-id="3911d-148">**New virtual network**: Create a new virtual network for your host.</span></span>
     * <span data-ttu-id="3911d-149">**Meglévő virtuális hálózat**: Írjon be egy meglévő virtuális hálózatot az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="3911d-149">**Existing virtual network**: Enter an existing virtual network from your Azure account.</span></span>

   <span data-ttu-id="3911d-150">g.</span><span class="sxs-lookup"><span data-stu-id="3911d-150">g.</span></span> <span data-ttu-id="3911d-151">A hello **tárolási** lapon:</span><span class="sxs-lookup"><span data-stu-id="3911d-151">On hello **Storage** tab:</span></span>
     * <span data-ttu-id="3911d-152">**Új tárfiók**: hozzon létre egy új tárfiókot az állomáshoz.</span><span class="sxs-lookup"><span data-stu-id="3911d-152">**New storage account**: Create a new storage account for your host.</span></span>
     * <span data-ttu-id="3911d-153">**Meglévő tárfiók**: Írjon be egy meglévő tárfiókot az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="3911d-153">**Existing storage account**: Enter an existing storage account from your Azure account.</span></span>

5. <span data-ttu-id="3911d-154">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="3911d-154">Click **Next**.</span></span>

6. <span data-ttu-id="3911d-155">A hello **napló konfigurálja a hitelesítő adatokat, és a portbeállítások** ablak, válasszon egyet az alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="3911d-155">In hello **Configure log in credentials and port settings** window, select one of hello following options:</span></span>

    * <span data-ttu-id="3911d-156">**Hitelesítő adatok importálása az Azure Key Vault**: egy korábban mentett kumulatív hitelesítő adatokat, amelyek az Azure-előfizetéshez vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="3911d-156">**Import credentials from Azure Key Vault**: Specifies a previously saved set of credentials that are stored in your Azure subscription.</span></span>

      >[!NOTE]
      ><span data-ttu-id="3911d-157">Egy Azure Key Vault, amely egy meghatározott fiók vagy a szolgáltatás egyszerű nincs egy másik fiókot vagy egyszerű szolgáltatásnév osztja meg a hello előfizetés automatikusan elérhető.</span><span class="sxs-lookup"><span data-stu-id="3911d-157">An Azure Key Vault that's created with a specific account or service principal is not automatically accessible by another account or service principal that shares hello subscription.</span></span> <span data-ttu-id="3911d-158">tooallow egy másik fiókot, vagy a szolgáltatás egyszerű toouse hello Key Vault, hello Azure portál tooadd hello fiók vagy a szolgáltatásnevet kell használnia.</span><span class="sxs-lookup"><span data-stu-id="3911d-158">tooallow another account or service principal toouse hello Key Vault, you must use hello Azure portal tooadd hello account or service principal.</span></span>

    * <span data-ttu-id="3911d-159">**Új bejelentkezési hitelesítő adatok**: hoz létre új bejelentkezési hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="3911d-159">**New log in credentials**: Creates a new set of login credentials.</span></span> <span data-ttu-id="3911d-160">Ha ezt a lehetőséget választja, a hello, a következő:</span><span class="sxs-lookup"><span data-stu-id="3911d-160">If you select this option, do hello following:</span></span>
    
      * <span data-ttu-id="3911d-161">A hello **VM hitelesítő adatok** lapra, majd a hello következő hello virtuális gép bejelentkezési hitelesítő adatokat a Docker-állomás beállításai közül:</span><span class="sxs-lookup"><span data-stu-id="3911d-161">On hello **VM Credentials** tab, choose one of hello following options for hello virtual-machine login credentials of your Docker host:</span></span>

          * <span data-ttu-id="3911d-162">**Felhasználónév**: hello felhasználónév megadása a virtuális gép bejelentkezési hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="3911d-162">**Username**: Enter hello username for your virtual machine login credentials.</span></span>
          * <span data-ttu-id="3911d-163">**Jelszó** és **megerősítése**: hello jelszó megadása a virtuális gép bejelentkezési hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="3911d-163">**Password** and **Confirm**: Enter hello password for your virtual machine login credentials.</span></span>
          * <span data-ttu-id="3911d-164">**SSH**: Adja meg a Docker állomás hello Secure Shell (SSH) beállításait.</span><span class="sxs-lookup"><span data-stu-id="3911d-164">**SSH**: Enter hello Secure Shell (SSH) settings for your Docker host.</span></span> <span data-ttu-id="3911d-165">Hello alábbi beállítások közül választhat:</span><span class="sxs-lookup"><span data-stu-id="3911d-165">You can choose from hello following options:</span></span>
            * <span data-ttu-id="3911d-166">**Nincs**: meghatározza, hogy a virtuális gép nem teszi lehetővé SSH-kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="3911d-166">**None**: Specifies that your virtual machine will not allow SSH connections.</span></span>
            * <span data-ttu-id="3911d-167">**Automatikus generálása**: hozza létre automatikusan hello szükséges beállítások SSH-kapcsolaton keresztül csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="3911d-167">**Auto-generate**: Automatically creates hello requisite settings for connecting via SSH.</span></span>
            * <span data-ttu-id="3911d-168">**Importálás a directory**: Adja meg a korábban mentett SSH-beállítások készletét tartalmazó könyvtár.</span><span class="sxs-lookup"><span data-stu-id="3911d-168">**Import from directory**: Specifies a directory that contains a set of previously saved SSH settings.</span></span> <span data-ttu-id="3911d-169">hello directory tartalmaznia kell a következő két fájlok hello:</span><span class="sxs-lookup"><span data-stu-id="3911d-169">hello directory must contain hello following two files:</span></span>
                * <span data-ttu-id="3911d-170">*id_rsa*: hello RSA azonosítás engedélyezése a felhasználó a tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3911d-170">*id_rsa*: Contains hello RSA identification for a user.</span></span>
                * <span data-ttu-id="3911d-171">*id_rsa.pub*: hello RSA nyilvános kulcsot tartalmaz, amelyek használják a hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="3911d-171">*id_rsa.pub*: Contains hello RSA public key that is used for authentication.</span></span>
        
        ![Docker állomás létrehozása][PUB05]

      * <span data-ttu-id="3911d-173">A hello **Docker démon hitelesítő adatok** lapra, adja meg az alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="3911d-173">On hello **Docker Daemon Credentials** tab, specify hello following options:</span></span>

          * <span data-ttu-id="3911d-174">**Docker démon port**: Adjon meg egyedi hello TCP-port a Docker-állomás.</span><span class="sxs-lookup"><span data-stu-id="3911d-174">**Docker Daemon port**: Enter hello unique TCP port for your Docker host.</span></span>
          * <span data-ttu-id="3911d-175">**A TLS biztonsági**: Adja meg a Docker állomás hello Transport Layer Security-beállításokat.</span><span class="sxs-lookup"><span data-stu-id="3911d-175">**TLS Security**: Enter hello Transport Layer Security settings for your Docker host.</span></span> <span data-ttu-id="3911d-176">Hello alábbi beállítások közül választhat:</span><span class="sxs-lookup"><span data-stu-id="3911d-176">You can choose from hello following options:</span></span>
            * <span data-ttu-id="3911d-177">**Nincs**: Megadja, hogy a virtuális gép nem engedélyezi TLS-kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="3911d-177">**None**: Specifies that your virtual machine will not allow TLS connections.</span></span>
            * <span data-ttu-id="3911d-178">**Automatikus generálása**: hozza létre automatikusan hello szükséges beállításokat a TLS keresztül kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="3911d-178">**Auto-generate**: Automatically creates hello requisite settings for connecting via TLS.</span></span>
            * <span data-ttu-id="3911d-179">**Importálás a directory**: Adja meg a korábban mentett TLS beállítások készletét tartalmazó könyvtár.</span><span class="sxs-lookup"><span data-stu-id="3911d-179">**Import from directory**: Specifies a directory that contains a set of previously saved TLS settings.</span></span> <span data-ttu-id="3911d-180">Pontosabban hello directory következő hat fájlok hello kell tartalmaznia:</span><span class="sxs-lookup"><span data-stu-id="3911d-180">More specifically, hello directory must contain hello following six files:</span></span>
                * <span data-ttu-id="3911d-181">*CA.PEM* és *ca-key.pem*: hello tanúsítvány és nyilvános kulcs a hello TLS hitelesítésszolgáltatót tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3911d-181">*ca.pem* and *ca-key.pem*: Contain hello certificate and public key for hello TLS Certificate Authority.</span></span>
                * <span data-ttu-id="3911d-182">*CERT.PEM* és *key.pem*: hello ügyféltanúsítványok és a TLS-hitelesítéshez használt nyilvános kulcsot tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3911d-182">*cert.pem* and *key.pem*: Contain hello client certificate and public key that is used for TLS authentication.</span></span>
                * <span data-ttu-id="3911d-183">*Server.PEM* és *kiszolgáló-key.pem*: hello kiszolgálói tanúsítvány és nyilvános kulcs hello állomás.</span><span class="sxs-lookup"><span data-stu-id="3911d-183">*server.pem* and *server-key.pem*: Contain hello server certificate and public key for hello host.</span></span>

        ![Docker állomás létrehozása][PUB06]

7. <span data-ttu-id="3911d-185">Miután megadta az összes információt megelőző hello, kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="3911d-185">After you have entered all of hello preceding information, click **Finish**.</span></span>

8. <span data-ttu-id="3911d-186">A hello **Docker-tároló központi telepítése az Azure-on** varázsló, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="3911d-186">In hello **Deploy Docker Container on Azure** wizard, click **Next**.</span></span>

   ![hello Azure varázsló a Docker-tároló központi telepítése][PUB07]

9. <span data-ttu-id="3911d-188">A hello **hello Docker-tároló toobe létrehozott konfigurálása** ablakban, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="3911d-188">In hello **Configure hello Docker container toobe created** window, do hello following:</span></span>

   <span data-ttu-id="3911d-189">a.</span><span class="sxs-lookup"><span data-stu-id="3911d-189">a.</span></span> <span data-ttu-id="3911d-190">A hello **Docker-tároló neve** mezőben adjon meg egy egyedi nevet a Docker-tároló.</span><span class="sxs-lookup"><span data-stu-id="3911d-190">In hello **Docker container name** box, enter a unique name for your Docker container.</span></span>

   <span data-ttu-id="3911d-191">b.</span><span class="sxs-lookup"><span data-stu-id="3911d-191">b.</span></span> <span data-ttu-id="3911d-192">A következő Docker képek hello közül választhat:</span><span class="sxs-lookup"><span data-stu-id="3911d-192">Choose one of hello following Docker images:</span></span>
     * <span data-ttu-id="3911d-193">**Előre definiált Docker kép**: Megadja a már meglévő Docker-lemezkép az Azure-ból.</span><span class="sxs-lookup"><span data-stu-id="3911d-193">**Predefined Docker image**: Specifies a pre-existing Docker image from Azure.</span></span>

       >[!NOTE]
       ><span data-ttu-id="3911d-194">hello Docker képek ebben a mezőben a lista az számos olyan rendszerkép található, hogy hello Azure eszközkészlet lett konfigurálva toopatch, hogy a rendszer automatikusan telepíti az összetevő.</span><span class="sxs-lookup"><span data-stu-id="3911d-194">hello list of Docker images in this box consists of several images that hello Azure Toolkit has been configured toopatch so that your artifact is deployed automatically.</span></span>

     * <span data-ttu-id="3911d-195">**Egyéni Dockerfile**: a helyi számítógépről egy korábban mentett Dockerfile határozza meg.</span><span class="sxs-lookup"><span data-stu-id="3911d-195">**Custom Dockerfile**: Specifies a previously saved Dockerfile from your local computer.</span></span>

       >[!NOTE]
       ><span data-ttu-id="3911d-196">Ez a tulajdonság speciális fejlesztők számára toodeploy saját Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="3911d-196">This is a more advanced feature for developers who want toodeploy their own Dockerfile.</span></span> <span data-ttu-id="3911d-197">Azt azonban ez a beállítás tooensure azok Dockerfile megfelelően részét képező használó toodevelopers legfeljebb lesz.</span><span class="sxs-lookup"><span data-stu-id="3911d-197">However, it is up toodevelopers who use this option tooensure that their Dockerfile is built correctly.</span></span> <span data-ttu-id="3911d-198">hello Azure eszközkészlet nem felel meg egy egyéni Dockerfile hello tartalma, hello telepítés esetleg sikertelen lesz, ha hello Dockerfile problémákkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="3911d-198">hello Azure Toolkit does not validate hello content in a custom Dockerfile, so hello deployment might fail if hello Dockerfile has issues.</span></span> <span data-ttu-id="3911d-199">Ezenkívül hello Azure eszközkészlet vár hello egyéni Dockerfile toocontain egy webes alkalmazás-összetevő, és megkísérel tooopen HTTP-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="3911d-199">In addition, hello Azure Toolkit expects hello custom Dockerfile toocontain a web app artifact, and it will attempt tooopen an HTTP connection.</span></span> <span data-ttu-id="3911d-200">Ha a fejlesztők közzététele összetevő más típusú, a telepítés utáni jelenhet ártalmatlan hibák.</span><span class="sxs-lookup"><span data-stu-id="3911d-200">If developers publish a different type of artifact, they may receive innocuous errors after deployment.</span></span>

   <span data-ttu-id="3911d-201">c.</span><span class="sxs-lookup"><span data-stu-id="3911d-201">c.</span></span> <span data-ttu-id="3911d-202">**Portbeállításokat**: hello egyedi TCP port kötést a Docker-tároló adja meg.</span><span class="sxs-lookup"><span data-stu-id="3911d-202">**Port settings**: Enter hello unique TCP port binding for your Docker container.</span></span>

     ![hello konfigurálása hello Docker tároló létrehozott toobe ablak][PUB08]

10. <span data-ttu-id="3911d-204">Kattintson a befejezését követően az összes lépést megelőző hello, **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="3911d-204">After you have completed all of hello preceding steps, click **Finish**.</span></span>

<span data-ttu-id="3911d-205">hello Azure eszközkészlet megkezdi a központi telepítését a webes alkalmazás tooAzure egy Docker-tároló.</span><span class="sxs-lookup"><span data-stu-id="3911d-205">hello Azure Toolkit begins deploying your web app tooAzure in a Docker container.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="3911d-206">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3911d-206">Next steps</span></span>
<span data-ttu-id="3911d-207">A Java IDEs hello Azure eszközök gazdag kapcsolatos további információkért tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="3911d-207">For more information about hello Azure Toolkits for Java IDEs, see hello following resources:</span></span>

* <span data-ttu-id="3911d-208">[Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="3911d-208">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="3911d-209">[What's new in hello Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="3911d-209">[What's new in hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="3911d-210">[Hello Azure eszköztára Eclipse telepítése]</span><span class="sxs-lookup"><span data-stu-id="3911d-210">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="3911d-211">[Hello Azure eszköztára Eclipse bejelentkezési utasítások]</span><span class="sxs-lookup"><span data-stu-id="3911d-211">[Sign-in instructions for hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="3911d-212">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]</span><span class="sxs-lookup"><span data-stu-id="3911d-212">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="3911d-213">[Az IntelliJ-hez készült Azure-eszközkészlet]</span><span class="sxs-lookup"><span data-stu-id="3911d-213">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="3911d-214">[What's new in hello IntelliJ Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="3911d-214">[What's new in hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="3911d-215">[Az IntelliJ hello Azure eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="3911d-215">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="3911d-216">[Bejelentkezési utasításokat hello Azure eszközkészlet az intellij-t]</span><span class="sxs-lookup"><span data-stu-id="3911d-216">[Sign-in instructions for hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="3911d-217">[Hello World webalkalmazás létrehozása az intellij-t az Azure]</span><span class="sxs-lookup"><span data-stu-id="3911d-217">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="3911d-218">Azure Java használatával kapcsolatos további információkért lásd: [Azure Java fejlesztői központból] és [Java Tools for Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="3911d-218">For more information about using Azure with Java, see [Azure Java Developer Center] and [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="3911d-219">További erőforrásokat a Docker, lásd: hello hivatalos [Docker webhely].</span><span class="sxs-lookup"><span data-stu-id="3911d-219">For additional resources for Docker, see hello official [Docker website].</span></span>

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

[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/

[Docker webhely]: https://www.docker.com/

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