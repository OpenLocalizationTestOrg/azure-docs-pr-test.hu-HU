---
title: "Távelérés engedélyezése az Azure-környezetekhez az eclipse-ben"
description: "Útmutató: a távelérés engedélyezése az Azure üzembe helyezése az Azure-eszközkészlet az eclipse-ben."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: b6150006-9a7f-4d83-be18-d35ec780c7c5
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 654d511bd5a62341f87569317e97360c94a6f26c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="enabling-remote-access-for-azure-deployments-in-eclipse"></a><span data-ttu-id="61157-103">Távelérés engedélyezése az Azure-környezetekhez az eclipse-ben</span><span class="sxs-lookup"><span data-stu-id="61157-103">Enabling Remote Access for Azure Deployments in Eclipse</span></span>
<span data-ttu-id="61157-104">Megoldhatja a központi telepítések, előfordulhat, hogy engedélyezése, és csatlakozzon a virtuális géphez, a központi telepítés üzemeltető távelérési segítségével.</span><span class="sxs-lookup"><span data-stu-id="61157-104">To help troubleshoot your deployments, you may enable and use Remote Access to connect to the virtual machine hosting your deployment.</span></span> <span data-ttu-id="61157-105">A távelérés funkciót a távoli asztal protokoll (RDP) a támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="61157-105">The Remote Access functionality relies on the Remote Desktop Protocol (RDP).</span></span> <span data-ttu-id="61157-106">Konfigurálja a távoli hozzáférést a központi telepítés után az Azure-bA rendelkezik közzétett, vagy Eclipse használatakor a Windows operációs rendszer konfigurálja a távoli hozzáférést az Azure-bA közzététele előtt.</span><span class="sxs-lookup"><span data-stu-id="61157-106">You can configure Remote Access for your deployment after you have published it to Azure, or if you are using Eclipse with a Windows operating system, you can configure Remote Access before you publish to Azure.</span></span> <span data-ttu-id="61157-107">Vegye figyelembe, hogy szüksége lesz a távoli asztali ügyfél, amely kompatibilis az operációs rendszer a központi telepítés az Azure virtuális géphez való csatlakozás érdekében.</span><span class="sxs-lookup"><span data-stu-id="61157-107">Note that you will need a remote desktop client that is compatible with your operating system in order to connect to your deployment's virtual machine in Azure.</span></span>

## <a name="how-to-enable-remote-access-before-you-deploy-to-azure"></a><span data-ttu-id="61157-108">Távoli hozzáférés engedélyezése előtt telepítse az Azure</span><span class="sxs-lookup"><span data-stu-id="61157-108">How to enable Remote Access before you deploy to Azure</span></span>
> [!NOTE]
> <span data-ttu-id="61157-109">Távelérés engedélyezése az Azure-bA az alkalmazás központi telepítése előtt, szüksége lehet az eclipse-ben futó Windows.</span><span class="sxs-lookup"><span data-stu-id="61157-109">To enable Remote Access before you deploy your application to Azure, you need to be running Eclipse on Windows.</span></span>
> 
> 

<span data-ttu-id="61157-110">Az alábbi képen látható a **távelérési** távoli hozzáférésének engedélyezésére szolgáló tulajdonságai párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="61157-110">The following image shows the **Remote Access** properties dialog used to enable remote access.</span></span>

![][ic719494]

<span data-ttu-id="61157-111">Két módon való megjelenítése a **távelérési** tulajdonságainak párbeszédpanelét jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="61157-111">There are two ways to display the **Remote Access** properties dialog:</span></span>

* <span data-ttu-id="61157-112">Kattintson a **speciális** hivatkozásra a **távelérési** szakasza a **Azure közzététel** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="61157-112">Click the **Advanced** link in the **Remote Access** section of the **Publish to Azure** dialog.</span></span>

* <span data-ttu-id="61157-113">Nyissa meg a **tulajdonságok** a Azure projekt párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="61157-113">Open the **Properties** dialog of your Azure project.</span></span>

<span data-ttu-id="61157-114">Amikor létrehoz egy új Azure-telepítés projektet, a projekt nem lesz alapértelmezés szerint engedélyezett távoli hozzáférési.</span><span class="sxs-lookup"><span data-stu-id="61157-114">When you create a new Azure deployment project, the project will not have Remote Access enabled by default.</span></span> <span data-ttu-id="61157-115">Azonban könnyen engedélyezheti távoli hozzáférést a felhasználónév és jelszó megadásával a **Azure közzététel** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="61157-115">However, you can easily enable remote access by specifying the user name and password in the **Publish to Azure** dialog.</span></span> <span data-ttu-id="61157-116">A távelérés jelszó titkosított X.509-tanúsítványokat használ.</span><span class="sxs-lookup"><span data-stu-id="61157-116">The Remote Access password is encrypted using X.509 certificates.</span></span> <span data-ttu-id="61157-117">Ha nem adja meg a tanúsítvány, a titkosítást egy önaláírt tanúsítványt, az Azure Eclipse beépülő modul rendszerrel szállított támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="61157-117">If you do not use provide your own certificate, the encryption relies on a self-signed certificate shipped with the Azure Plugin for Eclipse.</span></span> <span data-ttu-id="61157-118">Az önaláírt tanúsítvány van a **cert** mappában található az Azure-projekt, egy nyilvános tanúsítványfájlt (SampleRemoteAccessPublic.cer) mind a személyes információcseréhez kapcsolódó (PFX) Tanúsítványfájl (SampleRemoteAccessPrivate.pfx) tárolja.</span><span class="sxs-lookup"><span data-stu-id="61157-118">This self-signed certificate is in the **cert** folder of your Azure project, stored both as a public certificate file (SampleRemoteAccessPublic.cer) and as a Personal Information Exchange (PFX) certificate file (SampleRemoteAccessPrivate.pfx).</span></span> <span data-ttu-id="61157-119">Ez utóbbi tartalmazza a tanúsítvány titkos kulcsát, és nem rendelkezik, egy alapértelmezett jelszó **jelszó1**.</span><span class="sxs-lookup"><span data-stu-id="61157-119">The latter contains the private key for the certificate, and it has a default password, **Password1**.</span></span> <span data-ttu-id="61157-120">Azonban mivel a jelszó nyilvános Tudásbázis, az alapértelmezett tanúsítvány használható csak tanulási célokra, nem az éles környezet.</span><span class="sxs-lookup"><span data-stu-id="61157-120">However, since this password is public knowledge, the default certificate should be used only for learning purposes, not for a production deployment.</span></span> <span data-ttu-id="61157-121">Így eltérő megismerésére céljából, ha szeretné engedélyezni a központi telepítés távoli munkamenetek kattintson a **speciális** hivatkozásra a **Azure közzététel** párbeszédpanelen adja meg a saját tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="61157-121">So other than for learning purposes, when you want to enabled remote sessions for your deployments, you should click the **Advanced** link in the **Publish to Azure** dialog to specify your own certificate.</span></span> <span data-ttu-id="61157-122">Vegye figyelembe, hogy szeretné-e a tanúsítvány PFX verziója feltölteni az Azure felügyeleti portálon, az üzemeltetett szolgáltatás úgy, hogy Azure vissza tudja fejteni a felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="61157-122">Note that you'll need to upload the PFX version of the certificate to your hosted service within the Azure Management Portal, so that Azure can decrypt the user password.</span></span>

<span data-ttu-id="61157-123">A hátralévő részét az oktatóanyag bemutatja, hogyan engedélyezze a távoli hozzáférést egy Azure-telepítés projekt, amely eredetileg hoztak létre a távoli hozzáférés le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="61157-123">The remainder of the tutorial shows you how to enable remote access for an Azure deployment project that was initially created with remote access disabled.</span></span> <span data-ttu-id="61157-124">Ebben az oktatóanyagban létre fogunk hozni egy új önaláírt tanúsítványt, és a .pfx-fájlt egy tetszőleges jelszót kell.</span><span class="sxs-lookup"><span data-stu-id="61157-124">For purposes of this tutorial, we'll create a new self-signed certificate, and its .pfx file will have a password of your choice.</span></span> <span data-ttu-id="61157-125">Akkor is használhatja a hitelesítésszolgáltató által kiállított tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="61157-125">You also have the option of using a certificate issued by a certificate authority.</span></span>

## <a name="how-to-enable-remote-access-after-you-have-deployed-to-azure"></a><span data-ttu-id="61157-126">Távoli hozzáférés engedélyezése, miután telepítette az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="61157-126">How to enable Remote Access after you have deployed to Azure</span></span>
<span data-ttu-id="61157-127">Engedélyezze a távelérést, miután telepítette az Azure-ba, használja az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="61157-127">To enable remote access after you have deployed to Azure, use the following steps:</span></span>

1. <span data-ttu-id="61157-128">Jelentkezzen be az Azure felügyeleti portálra, az Azure-fiókjával</span><span class="sxs-lookup"><span data-stu-id="61157-128">Log into the Azure management portal using your Azure account</span></span>

2. <span data-ttu-id="61157-129">A listájában **Felhőszolgáltatások**, válassza ki a központilag telepített a felhőalapú szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="61157-129">In your list of **Cloud Services**, select your deployed cloud service</span></span>

3. <span data-ttu-id="61157-130">A felhőalapú szolgáltatás weblapon kattintson a **konfigurálása** hivatkozás</span><span class="sxs-lookup"><span data-stu-id="61157-130">In the cloud service web page, click the **Configure** link</span></span>

4. <span data-ttu-id="61157-131">A lap alján, kattintson a **távoli** hivatkozás</span><span class="sxs-lookup"><span data-stu-id="61157-131">On the bottom of the configuration page, click the **Remote** link</span></span>

5. <span data-ttu-id="61157-132">A felugró párbeszédpanel megjelenésekor:</span><span class="sxs-lookup"><span data-stu-id="61157-132">When the pop-up dialog box appears:</span></span>
   
   * <span data-ttu-id="61157-133">Adja meg a legyen az engedélyezi a távoli hozzáférést</span><span class="sxs-lookup"><span data-stu-id="61157-133">Specify the Role you for which you want to enable remote access</span></span>

   * <span data-ttu-id="61157-134">Kattintással jelölje ki a **távoli asztal engedélyezése** jelölőnégyzet</span><span class="sxs-lookup"><span data-stu-id="61157-134">Click to select the **Enable Remote Desktop** checkbox</span></span>
   
   * <span data-ttu-id="61157-135">Adjon meg egy felhasználónevet és a távoli hozzáféréshez használni kívánt jelszót</span><span class="sxs-lookup"><span data-stu-id="61157-135">Specify a user name and password you want to use for remote access</span></span>
   
   * <span data-ttu-id="61157-136">Válassza ki a használni kívánt tanúsítványt</span><span class="sxs-lookup"><span data-stu-id="61157-136">Select the certificate to use</span></span>

6. <span data-ttu-id="61157-137">Kattintson az **OK** gombra</span><span class="sxs-lookup"><span data-stu-id="61157-137">Click **OK**</span></span> 

<span data-ttu-id="61157-138">Látni fogja üzenet jelenik meg, hogy a konfiguráció módosítását van folyamatban, néhány percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="61157-138">You will see a message stating that your configuration change is in progress, which may take a few minutes to complete.</span></span> <span data-ttu-id="61157-139">A konfigurációs módosítás befejezése után kövesse a lépéseket a **távoli bejelentkezési** szakasz a cikk későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="61157-139">After the configuration change has completed, follow the steps in the **To log in remotely** section later in this article.</span></span>

## <a name="how-to-enable-remote-access-in-your-package"></a><span data-ttu-id="61157-140">A csomagban lévő távoli hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="61157-140">How to enable Remote Access in your package</span></span>
1. <span data-ttu-id="61157-141">Belül meg Eclipse Project Explorer ablaktáblában kattintson a jobb gombbal az Azure-projekt, és kattintson a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="61157-141">Within Eclipse's Project Explorer pane, right-click your Azure project and click **Properties**.</span></span>

2. <span data-ttu-id="61157-142">Az a **tulajdonságok** párbeszédpanelen bontsa ki a **Azure** a bal oldali ablaktáblán, majd kattintson a **távelérési**.</span><span class="sxs-lookup"><span data-stu-id="61157-142">In the **Properties** dialog, expand **Azure** in the left-hand pane and click **Remote Access**.</span></span>

3. <span data-ttu-id="61157-143">Az a **távelérési** párbeszédpanelen győződjön meg arról **engedélyezése a távoli asztali kapcsolatok fogadására a bejelentkezési hitelesítő adatokat az összes szerepkör** be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="61157-143">In the **Remote Access** dialog, ensure **Enable all roles to accept Remote Desktop Connections with these login credentials** is checked.</span></span>

4. <span data-ttu-id="61157-144">Adjon meg egy felhasználónevet, a távoli asztali kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="61157-144">Specify a user name for the Remote Desktop connection.</span></span>

5. <span data-ttu-id="61157-145">Adja meg, és erősítse meg a felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="61157-145">Specify and confirm the password for the user.</span></span> <span data-ttu-id="61157-146">Ezen a párbeszédpanelen beállított felhasználó nevét és jelszavát értékeket fogja használni, amikor a távoli asztali kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="61157-146">The user name and password values set in this dialog will be used when you make a Remote Desktop connection.</span></span> <span data-ttu-id="61157-147">(Vegye figyelembe, hogy ez a PFX-jelszót külön jelszó.)</span><span class="sxs-lookup"><span data-stu-id="61157-147">(Note that this is a separate password from your PFX password.)</span></span>

6. <span data-ttu-id="61157-148">Adja meg a felhasználói fiók lejárati dátumát.</span><span class="sxs-lookup"><span data-stu-id="61157-148">Specify the expiration date for the user account.</span></span>

7. <span data-ttu-id="61157-149">Kattintson a **új** egy új önaláírt tanúsítvány létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="61157-149">Click **New** to create a new self-signed certificate.</span></span> <span data-ttu-id="61157-150">(Másik lehetőségként a munkaterület vagy a fájl rendszerről keresztül kiválaszthatja egy tanúsítványt a **munkaterület** vagy **fájlrendszer** gomb, illetve, de célokra, ebben az oktatóanyagban létre fogunk hozni egy új tanúsítványt.)</span><span class="sxs-lookup"><span data-stu-id="61157-150">(Alternatively, you could select a certificate from your workspace or file system through the **Workspace** or **FileSystem** buttons, respectively, but for purposes of this tutorial we'll create a new certificate.)</span></span>

   * <span data-ttu-id="61157-151">Az a **új tanúsítvány** párbeszédpanelen adja meg, és erősítse meg a jelszót a PFX-fájljának fogja használni.</span><span class="sxs-lookup"><span data-stu-id="61157-151">In the **New Certificate** dialog, specify and confirm the password you'll use for your PFX file.</span></span>

   * <span data-ttu-id="61157-152">Fogadja el a megadott érték **név (CN)**, vagy használjon egy egyéni nevet.</span><span class="sxs-lookup"><span data-stu-id="61157-152">Accept the value provided for **Name (CN)**, or use a custom name.</span></span>

   * <span data-ttu-id="61157-153">Adja meg az új tanúsítvány .cer formában menteni elérési útját és nevét.</span><span class="sxs-lookup"><span data-stu-id="61157-153">Specify the path and file name where the new certificate, in .cer form, will be saved.</span></span> <span data-ttu-id="61157-154">Ezt a lépést, és a következő lépés, használhatja a **cert** mappában található az Azure-projekt, de szabadon válasszon másik helyet.</span><span class="sxs-lookup"><span data-stu-id="61157-154">For this step and the next step, you could use the **cert** folder of your Azure project, but you're free to choose another location.</span></span> <span data-ttu-id="61157-155">Ez az oktatóanyag céljából, ezen **c:\mycert\mycert.cer**.</span><span class="sxs-lookup"><span data-stu-id="61157-155">For purposes of this tutorial, we'll use **c:\mycert\mycert.cer**.</span></span> <span data-ttu-id="61157-156">(Létrehozása a **c:\mycert** mappa eljárás, vagy használjon egy létező mappát, ha szükséges.)</span><span class="sxs-lookup"><span data-stu-id="61157-156">(Create the **c:\mycert** folder prior to proceeding, or use an existing folder if desired.)</span></span>

   * <span data-ttu-id="61157-157">Adja meg a elérési útját és nevét, ahová az új tanúsítványt és annak titkos kulcsát, .pfx formátumban menti.</span><span class="sxs-lookup"><span data-stu-id="61157-157">Specify the path and file name where the new certificate and its private key, in .pfx form, will be saved.</span></span> <span data-ttu-id="61157-158">Ez az oktatóanyag céljából, ezen **c:\mycert\mycert.pfx**.</span><span class="sxs-lookup"><span data-stu-id="61157-158">For purposes of this tutorial, we'll use **c:\mycert\mycert.pfx**.</span></span> <span data-ttu-id="61157-159">A **új tanúsítvány** párbeszédpanelen a következőhöz hasonlóan kell kinéznie (frissítse a mappák elérési útjában, ha nem használja **c:\mycert**):</span><span class="sxs-lookup"><span data-stu-id="61157-159">Your **New Certificate** dialog should look similar to the following (update the folder paths if you did not use **c:\mycert**):</span></span>
     
      ![][ic712275]

   * <span data-ttu-id="61157-160">Kattintson a **OK** bezárásához a **új tanúsítvány** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="61157-160">Click **OK** to close the **New Certificate** dialog.</span></span>

8. <span data-ttu-id="61157-161">A **távelérési** párbeszédpanelen a következőhöz hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="61157-161">Your **Remote Access** dialog should look similar to the following:</span></span></p>
   
   ![][ic719495]

9. <span data-ttu-id="61157-162">Kattintson a **OK** bezárásához a **távelérési** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="61157-162">Click **OK** to close the **Remote Access** dialog.</span></span>

<span data-ttu-id="61157-163">Az alkalmazás építenie a központi telepítés felhőbe összeállítása.</span><span class="sxs-lookup"><span data-stu-id="61157-163">Rebuild your application, with the build set for deployment to cloud.</span></span>

## <a name="to-log-in-remotely"></a><span data-ttu-id="61157-164">A távoli bejelentkezéshez</span><span class="sxs-lookup"><span data-stu-id="61157-164">To log in remotely</span></span>
<span data-ttu-id="61157-165">Ha készen áll a szerepkör példánya, távolról bejelentkezhet a virtuális géphez, amelyen az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="61157-165">Once your role instance is ready, you can remotely log in to the virtual machine that is hosting your application.</span></span>

* <span data-ttu-id="61157-166">Ha az eclipse-ben a Windows és a kiválasztott használ a **Start távoli asztal a telepítése** lehetőséget az Azure-ba való üzembe helyezés során választhat egy távoli asztali kapcsolat bejelentkezési képernyőt, ha a telepítés megkezdése.</span><span class="sxs-lookup"><span data-stu-id="61157-166">If are using Eclipse on Windows and you selected the **Start remote desktop on deploy** option during your deployment to Azure, you will be presented with a Remote Desktop Connection logon screen when your deployment starts.</span></span> <span data-ttu-id="61157-167">Amikor a felhasználónevet és jelszót kéri, adja meg a távoli felhasználó számára megadott értékeket, és fog tudni jelentkezni.</span><span class="sxs-lookup"><span data-stu-id="61157-167">When you are prompted for the user name and password, enter the values that you specified for the remote user and will be able to log in.</span></span>

* <span data-ttu-id="61157-168">A távoli bejelentkezéshez egy másik lehetőség az <a href="http://go.microsoft.com/fwlink/?LinkID=512959">Azure felügyeleti portálon</a>:</span><span class="sxs-lookup"><span data-stu-id="61157-168">Another way to log in remotely is through the <a href="http://go.microsoft.com/fwlink/?LinkID=512959">Azure Management Portal</a>:</span></span>
  
  * <span data-ttu-id="61157-169">Belül a **Felhőszolgáltatások** az Azure felügyeleti portál, kattintson a felhőalapú szolgáltatás megtekintéséhez kattintson **példányok**, kattintson egy adott példányt, majd a **Connect** gombra.</span><span class="sxs-lookup"><span data-stu-id="61157-169">Within the **Cloud Services** view of the Azure Management portal, click your cloud service, click **Instances**, click a specific instance, and then click the **Connect** button.</span></span> <span data-ttu-id="61157-170">A **Connect** gombja megjelenik ugyan a parancssávon az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="61157-170">The **Connect** button appears as the following in the command bar:</span></span>
    
      ![][ic659273]

  * <span data-ttu-id="61157-171">Miután rákattintott a **Connect** gomb kérni fogja az RDP-fájl megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="61157-171">After clicking the **Connect** button, you will be prompted to open an RDP file.</span></span> <span data-ttu-id="61157-172">Nyissa meg a fájlt, és kövesse az utasításokat.</span><span class="sxs-lookup"><span data-stu-id="61157-172">Open the file and follow the prompts.</span></span> <span data-ttu-id="61157-173">(Ön sikerült is mentse a fájlt a helyi számítógépen, és futtassa a fájl kattintson duplán a virtuális géphez távoli bejelentkezési anélkül, hogy nyissa meg a felügyeleti portálon.)</span><span class="sxs-lookup"><span data-stu-id="61157-173">(You could also save this file to your local computer, and then run the file by double-clicking it to remote log in to your virtual machine without needing to first go the management portal.)</span></span>

  * <span data-ttu-id="61157-174">Amikor a felhasználónevet és jelszót kéri, adja meg a távoli felhasználó számára megadott értékeket, és fog tudni jelentkezni.</span><span class="sxs-lookup"><span data-stu-id="61157-174">When you are prompted for the user name and password, enter the values that you specified for the remote user and will be able to log in.</span></span>

> [!NOTE]
> <span data-ttu-id="61157-175">Ha nem Windows operációs rendszeren, szüksége egy távoli asztali ügyfél, amely kompatibilis az operációs rendszer, és kövesse a lépéseket, hogy az ügyfél konfigurálása az RDP-fájlban letöltött beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="61157-175">If you are on a non-Windows operating system, you need to use a Remote Desktop client that is compatible with your operating system and follow the steps to configure that client with the settings in the RDP file that you downloaded.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="61157-176">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="61157-176">See Also</span></span>
<span data-ttu-id="61157-177">[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="61157-177">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="61157-178">[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="61157-178">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="61157-179">[Az eclipse-ben az Azure eszközkészlet telepítése][Installing the Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="61157-179">[Installing the Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="61157-180">Azure Java használatával kapcsolatos további információkért lásd: a [Azure Java fejlesztői központból][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="61157-180">For more information about using Azure with Java, see the [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic712275]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic712275.png
[ic719495]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719495.png
[ic719494]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719494.png
[ic659273]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic659273.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690951.aspx -->
