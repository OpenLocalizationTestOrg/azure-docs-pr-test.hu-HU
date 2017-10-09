---
title: "Azure-telepítésekre az eclipse-ben a távelérés aaaEnabling"
description: "Ismerje meg, hogyan tooenable távelérés az Azure-környezetekhez hello Azure eszközkészlet használata az eclipse-ben."
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
ms.openlocfilehash: 00c2bf22c1f3ec792098f154f771c87506e87881
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enabling-remote-access-for-azure-deployments-in-eclipse"></a><span data-ttu-id="96b7c-103">Távelérés engedélyezése az Azure-környezetekhez az eclipse-ben</span><span class="sxs-lookup"><span data-stu-id="96b7c-103">Enabling Remote Access for Azure Deployments in Eclipse</span></span>
<span data-ttu-id="96b7c-104">toohelp hibaelhárítása a központi telepítések, előfordulhat, hogy engedélyezi, és használja a távelérés tooconnect toohello virtuálisgép üzemeltetési a környezethez.</span><span class="sxs-lookup"><span data-stu-id="96b7c-104">toohelp troubleshoot your deployments, you may enable and use Remote Access tooconnect toohello virtual machine hosting your deployment.</span></span> <span data-ttu-id="96b7c-105">Távelérés funkciót hello hello Remote Desktop Protocol (RDP) alapul.</span><span class="sxs-lookup"><span data-stu-id="96b7c-105">hello Remote Access functionality relies on hello Remote Desktop Protocol (RDP).</span></span> <span data-ttu-id="96b7c-106">Konfigurálja a távoli hozzáférést az üzembe helyezéshez tooAzure van közzétéve, vagy ha az eclipse-ben a Windows operációs rendszert használ, konfigurálja a távoli hozzáférést tooAzure közzététele előtt után.</span><span class="sxs-lookup"><span data-stu-id="96b7c-106">You can configure Remote Access for your deployment after you have published it tooAzure, or if you are using Eclipse with a Windows operating system, you can configure Remote Access before you publish tooAzure.</span></span> <span data-ttu-id="96b7c-107">Vegye figyelembe, hogy szüksége lesz a távoli asztali ügyfelek kompatibilis az operációs rendszer rendelés tooconnect tooyour telepítése virtuális gépre az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="96b7c-107">Note that you will need a remote desktop client that is compatible with your operating system in order tooconnect tooyour deployment's virtual machine in Azure.</span></span>

## <a name="how-tooenable-remote-access-before-you-deploy-tooazure"></a><span data-ttu-id="96b7c-108">Hogyan telepítsék központilag az tooenable távelérési előtt tooAzure</span><span class="sxs-lookup"><span data-stu-id="96b7c-108">How tooenable Remote Access before you deploy tooAzure</span></span>
> [!NOTE]
> <span data-ttu-id="96b7c-109">tooenable távoli elérés az alkalmazás tooAzure központi telepítése előtt, meg kell eclipse-ben futó Windows toobe.</span><span class="sxs-lookup"><span data-stu-id="96b7c-109">tooenable Remote Access before you deploy your application tooAzure, you need toobe running Eclipse on Windows.</span></span>
> 
> 

<span data-ttu-id="96b7c-110">hello következő kép bemutatja hello **távelérési** tulajdonságai párbeszédpanelen tooenable távelérési használt.</span><span class="sxs-lookup"><span data-stu-id="96b7c-110">hello following image shows hello **Remote Access** properties dialog used tooenable remote access.</span></span>

![][ic719494]

<span data-ttu-id="96b7c-111">Nincsenek a két módon toodisplay hello **távelérési** tulajdonságainak párbeszédpanelét jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="96b7c-111">There are two ways toodisplay hello **Remote Access** properties dialog:</span></span>

* <span data-ttu-id="96b7c-112">Kattintson a hello **speciális** hello hivatkozásra **távelérési** hello szakasza **tooAzure közzététele** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="96b7c-112">Click hello **Advanced** link in hello **Remote Access** section of hello **Publish tooAzure** dialog.</span></span>

* <span data-ttu-id="96b7c-113">Nyissa meg hello **tulajdonságok** a Azure projekt párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="96b7c-113">Open hello **Properties** dialog of your Azure project.</span></span>

<span data-ttu-id="96b7c-114">Amikor létrehoz egy új Azure-telepítés projektet, hello projekt nem lesz alapértelmezés szerint engedélyezett távoli hozzáférési.</span><span class="sxs-lookup"><span data-stu-id="96b7c-114">When you create a new Azure deployment project, hello project will not have Remote Access enabled by default.</span></span> <span data-ttu-id="96b7c-115">Azonban, könnyen engedélyezheti távelérési hello hello felhasználónév és jelszó megadásával **tooAzure közzététele** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="96b7c-115">However, you can easily enable remote access by specifying hello user name and password in hello **Publish tooAzure** dialog.</span></span> <span data-ttu-id="96b7c-116">hello távelérési jelszó titkosított X.509-tanúsítványokat használ.</span><span class="sxs-lookup"><span data-stu-id="96b7c-116">hello Remote Access password is encrypted using X.509 certificates.</span></span> <span data-ttu-id="96b7c-117">Ha nem adja meg a tanúsítvány, hello titkosítási támaszkodik hello Azure Eclipse beépülő modul rendszerrel szállított önaláírt tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="96b7c-117">If you do not use provide your own certificate, hello encryption relies on a self-signed certificate shipped with hello Azure Plugin for Eclipse.</span></span> <span data-ttu-id="96b7c-118">Az önaláírt tanúsítvány van hello **cert** az Azure-projekt mappájában tárolt mindkét nyilvános tanúsítvány fájlként (SampleRemoteAccessPublic.cer) és a, a személyes információcseréhez kapcsolódó (PFX) tanúsítványa () SampleRemoteAccessPrivate.pfx).</span><span class="sxs-lookup"><span data-stu-id="96b7c-118">This self-signed certificate is in hello **cert** folder of your Azure project, stored both as a public certificate file (SampleRemoteAccessPublic.cer) and as a Personal Information Exchange (PFX) certificate file (SampleRemoteAccessPrivate.pfx).</span></span> <span data-ttu-id="96b7c-119">Ez utóbbi hello hello hello tanúsítvány titkos kulcsa tartalmazza, és nem rendelkezik, egy alapértelmezett jelszó **jelszó1**.</span><span class="sxs-lookup"><span data-stu-id="96b7c-119">hello latter contains hello private key for hello certificate, and it has a default password, **Password1**.</span></span> <span data-ttu-id="96b7c-120">Azonban mivel ezt a jelszót nyilvános Tudásbázis, hello alapértelmezett tanúsítványt kell használni csak tanulási célokra, nem az éles környezet.</span><span class="sxs-lookup"><span data-stu-id="96b7c-120">However, since this password is public knowledge, hello default certificate should be used only for learning purposes, not for a production deployment.</span></span> <span data-ttu-id="96b7c-121">Így eltérő megismerésére céljából, ha azt szeretné, hogy tooenabled távoli munkamenetek a központi telepítése esetén kattintson hello **speciális** hello hivatkozásra **tooAzure közzététele** párbeszédpanel toospecify saját tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="96b7c-121">So other than for learning purposes, when you want tooenabled remote sessions for your deployments, you should click hello **Advanced** link in hello **Publish tooAzure** dialog toospecify your own certificate.</span></span> <span data-ttu-id="96b7c-122">Vegye figyelembe, hogy szüksége lesz a tooupload hello PFX verziója hello tanúsítvány tooyour üzemeltetett szolgáltatás hello Azure felügyeleti portálon belül, úgy, hogy Azure vissza tudja fejteni hello felhasználói jelszavát.</span><span class="sxs-lookup"><span data-stu-id="96b7c-122">Note that you'll need tooupload hello PFX version of hello certificate tooyour hosted service within hello Azure Management Portal, so that Azure can decrypt hello user password.</span></span>

<span data-ttu-id="96b7c-123">hello maradéka hello az oktatóanyag bemutatja, hogyan tooenable távelérés távoli hozzáférés letiltva eredetileg létrehozott Azure-telepítés projekthez.</span><span class="sxs-lookup"><span data-stu-id="96b7c-123">hello remainder of hello tutorial shows you how tooenable remote access for an Azure deployment project that was initially created with remote access disabled.</span></span> <span data-ttu-id="96b7c-124">Ebben az oktatóanyagban létre fogunk hozni egy új önaláírt tanúsítványt, és a .pfx-fájlt egy tetszőleges jelszót kell.</span><span class="sxs-lookup"><span data-stu-id="96b7c-124">For purposes of this tutorial, we'll create a new self-signed certificate, and its .pfx file will have a password of your choice.</span></span> <span data-ttu-id="96b7c-125">Akkor is hello beállítással, a hitelesítésszolgáltató által kiállított tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="96b7c-125">You also have hello option of using a certificate issued by a certificate authority.</span></span>

## <a name="how-tooenable-remote-access-after-you-have-deployed-tooazure"></a><span data-ttu-id="96b7c-126">Hogyan tooenable távelérési követően tooAzure telepített</span><span class="sxs-lookup"><span data-stu-id="96b7c-126">How tooenable Remote Access after you have deployed tooAzure</span></span>
<span data-ttu-id="96b7c-127">tooenable távelérési tooAzure, használjon hello lépések telepítése után:</span><span class="sxs-lookup"><span data-stu-id="96b7c-127">tooenable remote access after you have deployed tooAzure, use hello following steps:</span></span>

1. <span data-ttu-id="96b7c-128">Jelentkezzen be Azure-fiókjával hello Azure felügyeleti portálra</span><span class="sxs-lookup"><span data-stu-id="96b7c-128">Log into hello Azure management portal using your Azure account</span></span>

2. <span data-ttu-id="96b7c-129">A listájában **Felhőszolgáltatások**, válassza ki a központilag telepített a felhőalapú szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="96b7c-129">In your list of **Cloud Services**, select your deployed cloud service</span></span>

3. <span data-ttu-id="96b7c-130">Hello cloud service weblapon, kattintson a hello **konfigurálása** hivatkozás</span><span class="sxs-lookup"><span data-stu-id="96b7c-130">In hello cloud service web page, click hello **Configure** link</span></span>

4. <span data-ttu-id="96b7c-131">A hello a hello konfiguráció lap alján, kattintson a hello **távoli** hivatkozás</span><span class="sxs-lookup"><span data-stu-id="96b7c-131">On hello bottom of hello configuration page, click hello **Remote** link</span></span>

5. <span data-ttu-id="96b7c-132">Hello felugró párbeszédpanel megjelenésekor:</span><span class="sxs-lookup"><span data-stu-id="96b7c-132">When hello pop-up dialog box appears:</span></span>
   
   * <span data-ttu-id="96b7c-133">Adja meg a szerepkör hello meg a legyen tooenable távelérés</span><span class="sxs-lookup"><span data-stu-id="96b7c-133">Specify hello Role you for which you want tooenable remote access</span></span>

   * <span data-ttu-id="96b7c-134">Kattintson a tooselect hello **távoli asztal engedélyezése** jelölőnégyzet</span><span class="sxs-lookup"><span data-stu-id="96b7c-134">Click tooselect hello **Enable Remote Desktop** checkbox</span></span>
   
   * <span data-ttu-id="96b7c-135">Adjon meg egy felhasználói nevet és jelszót toouse szeretne a távoli hozzáféréshez</span><span class="sxs-lookup"><span data-stu-id="96b7c-135">Specify a user name and password you want toouse for remote access</span></span>
   
   * <span data-ttu-id="96b7c-136">Válassza ki a hello tanúsítvány toouse</span><span class="sxs-lookup"><span data-stu-id="96b7c-136">Select hello certificate toouse</span></span>

6. <span data-ttu-id="96b7c-137">Kattintson az **OK** gombra</span><span class="sxs-lookup"><span data-stu-id="96b7c-137">Click **OK**</span></span> 

<span data-ttu-id="96b7c-138">Egy üzenet arról, hogy a konfiguráció módosítása folyamatban van, ami eltarthat néhány percig toocomplete jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="96b7c-138">You will see a message stating that your configuration change is in progress, which may take a few minutes toocomplete.</span></span> <span data-ttu-id="96b7c-139">Hello konfigurációs módosítás befejezése után kövesse hello hello **a toolog távolról** szakasz a cikk későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="96b7c-139">After hello configuration change has completed, follow hello steps in hello **toolog in remotely** section later in this article.</span></span>

## <a name="how-tooenable-remote-access-in-your-package"></a><span data-ttu-id="96b7c-140">Hogyan távelérés tooenable, ha a csomag</span><span class="sxs-lookup"><span data-stu-id="96b7c-140">How tooenable Remote Access in your package</span></span>
1. <span data-ttu-id="96b7c-141">Belül meg Eclipse Project Explorer ablaktáblában kattintson a jobb gombbal az Azure-projekt, és kattintson a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="96b7c-141">Within Eclipse's Project Explorer pane, right-click your Azure project and click **Properties**.</span></span>

2. <span data-ttu-id="96b7c-142">A hello **tulajdonságok** párbeszédpanelen bontsa ki a **Azure** a hello bal oldali ablaktáblában kattintson **távelérési**.</span><span class="sxs-lookup"><span data-stu-id="96b7c-142">In hello **Properties** dialog, expand **Azure** in hello left-hand pane and click **Remote Access**.</span></span>

3. <span data-ttu-id="96b7c-143">A hello **távelérési** párbeszédpanelen győződjön meg arról **engedélyezése minden szerepkörök tooaccept a távoli asztali kapcsolatokat a bejelentkezési hitelesítő adatokkal rendelkező** be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="96b7c-143">In hello **Remote Access** dialog, ensure **Enable all roles tooaccept Remote Desktop Connections with these login credentials** is checked.</span></span>

4. <span data-ttu-id="96b7c-144">Adjon meg egy felhasználónevet a távoli asztali kapcsolat hello.</span><span class="sxs-lookup"><span data-stu-id="96b7c-144">Specify a user name for hello Remote Desktop connection.</span></span>

5. <span data-ttu-id="96b7c-145">Adja meg, és erősítse meg a hello hello felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="96b7c-145">Specify and confirm hello password for hello user.</span></span> <span data-ttu-id="96b7c-146">hello felhasználói nevet és jelszót értékek ezen a párbeszédpanelen állítsa be a távoli asztali kapcsolat létrehozásakor használható.</span><span class="sxs-lookup"><span data-stu-id="96b7c-146">hello user name and password values set in this dialog will be used when you make a Remote Desktop connection.</span></span> <span data-ttu-id="96b7c-147">(Vegye figyelembe, hogy ez a PFX-jelszót külön jelszó.)</span><span class="sxs-lookup"><span data-stu-id="96b7c-147">(Note that this is a separate password from your PFX password.)</span></span>

6. <span data-ttu-id="96b7c-148">Adja meg a lejárati dátum hello hello felhasználói fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="96b7c-148">Specify hello expiration date for hello user account.</span></span>

7. <span data-ttu-id="96b7c-149">Kattintson a **új** toocreate egy új önaláírt tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="96b7c-149">Click **New** toocreate a new self-signed certificate.</span></span> <span data-ttu-id="96b7c-150">(Másik lehetőségként kiválaszthatja egy tanúsítványt a munkaterület vagy a fájl rendszerről hello keresztül **munkaterület** vagy **fájlrendszer** gomb, illetve, de ebben az oktatóanyagban létre fogunk hozni egy új alkalmazásában tanúsítvány.)</span><span class="sxs-lookup"><span data-stu-id="96b7c-150">(Alternatively, you could select a certificate from your workspace or file system through hello **Workspace** or **FileSystem** buttons, respectively, but for purposes of this tutorial we'll create a new certificate.)</span></span>

   * <span data-ttu-id="96b7c-151">A hello **új tanúsítvány** párbeszédpanelen adja meg, majd erősítse meg fogja használni a PFX-fájljának hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="96b7c-151">In hello **New Certificate** dialog, specify and confirm hello password you'll use for your PFX file.</span></span>

   * <span data-ttu-id="96b7c-152">Fogadja el a megadott érték hello **név (CN)**, vagy használjon egy egyéni nevet.</span><span class="sxs-lookup"><span data-stu-id="96b7c-152">Accept hello value provided for **Name (CN)**, or use a custom name.</span></span>

   * <span data-ttu-id="96b7c-153">Adja meg a hello új tanúsítvány .cer formában menteni hello elérési útját és nevét.</span><span class="sxs-lookup"><span data-stu-id="96b7c-153">Specify hello path and file name where hello new certificate, in .cer form, will be saved.</span></span> <span data-ttu-id="96b7c-154">Ez és hello következő lépést, a hello használata **cert** mappában található az Azure-projekt, de most szabad toochoose egy másik helyre.</span><span class="sxs-lookup"><span data-stu-id="96b7c-154">For this step and hello next step, you could use hello **cert** folder of your Azure project, but you're free toochoose another location.</span></span> <span data-ttu-id="96b7c-155">Ez az oktatóanyag céljából, ezen **c:\mycert\mycert.cer**.</span><span class="sxs-lookup"><span data-stu-id="96b7c-155">For purposes of this tutorial, we'll use **c:\mycert\mycert.cer**.</span></span> <span data-ttu-id="96b7c-156">(Hello létrehozása **c:\mycert** mappa előzetes tooproceeding, vagy használjon egy létező mappát, ha szükséges.)</span><span class="sxs-lookup"><span data-stu-id="96b7c-156">(Create hello **c:\mycert** folder prior tooproceeding, or use an existing folder if desired.)</span></span>

   * <span data-ttu-id="96b7c-157">Adja meg a hello új tanúsítványt és a titkos kulcsot .pfx formátumban szeretné menteni hello elérési útját és nevét.</span><span class="sxs-lookup"><span data-stu-id="96b7c-157">Specify hello path and file name where hello new certificate and its private key, in .pfx form, will be saved.</span></span> <span data-ttu-id="96b7c-158">Ez az oktatóanyag céljából, ezen **c:\mycert\mycert.pfx**.</span><span class="sxs-lookup"><span data-stu-id="96b7c-158">For purposes of this tutorial, we'll use **c:\mycert\mycert.pfx**.</span></span> <span data-ttu-id="96b7c-159">A **új tanúsítvány** párbeszédpanel alábbihoz hasonló toohello következő (hello mappák elérési útjaiban frissítéséhez, ha nem használja **c:\mycert**):</span><span class="sxs-lookup"><span data-stu-id="96b7c-159">Your **New Certificate** dialog should look similar toohello following (update hello folder paths if you did not use **c:\mycert**):</span></span>
     
      ![][ic712275]

   * <span data-ttu-id="96b7c-160">Kattintson a **OK** tooclose hello **új tanúsítvány** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="96b7c-160">Click **OK** tooclose hello **New Certificate** dialog.</span></span>

8. <span data-ttu-id="96b7c-161">A **távelérési** párbeszédpanel hasonló toohello következő kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="96b7c-161">Your **Remote Access** dialog should look similar toohello following:</span></span></p>
   
   ![][ic719495]

9. <span data-ttu-id="96b7c-162">Kattintson a **OK** tooclose hello **távelérési** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="96b7c-162">Click **OK** tooclose hello **Remote Access** dialog.</span></span>

<span data-ttu-id="96b7c-163">Az alkalmazás, a hello összeállítása a központi telepítés toocloud meg van adva.</span><span class="sxs-lookup"><span data-stu-id="96b7c-163">Rebuild your application, with hello build set for deployment toocloud.</span></span>

## <a name="toolog-in-remotely"></a><span data-ttu-id="96b7c-164">a toolog távolról</span><span class="sxs-lookup"><span data-stu-id="96b7c-164">toolog in remotely</span></span>
<span data-ttu-id="96b7c-165">Ha készen áll a szerepkör példánya, távolról bejelentkezhet toohello virtuális gép, amelyen az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="96b7c-165">Once your role instance is ready, you can remotely log in toohello virtual machine that is hosting your application.</span></span>

* <span data-ttu-id="96b7c-166">Ha az eclipse-ben a Windows és a kijelölt hello használ **Start távoli asztal a telepítése** beállítást a központi telepítés tooAzure során választhat egy távoli asztali kapcsolat bejelentkezési képernyőt, amikor a telepítés megkezdése.</span><span class="sxs-lookup"><span data-stu-id="96b7c-166">If are using Eclipse on Windows and you selected hello **Start remote desktop on deploy** option during your deployment tooAzure, you will be presented with a Remote Desktop Connection logon screen when your deployment starts.</span></span> <span data-ttu-id="96b7c-167">Amikor hello felhasználónevet és jelszót kéri, adja meg a távoli felhasználó hello megadott hello értékeket, és képes toolog a.</span><span class="sxs-lookup"><span data-stu-id="96b7c-167">When you are prompted for hello user name and password, enter hello values that you specified for hello remote user and will be able toolog in.</span></span>

* <span data-ttu-id="96b7c-168">Hello keresztül távolról van egy másik módja toolog a <a href="http://go.microsoft.com/fwlink/?LinkID=512959">Azure felügyeleti portálon</a>:</span><span class="sxs-lookup"><span data-stu-id="96b7c-168">Another way toolog in remotely is through hello <a href="http://go.microsoft.com/fwlink/?LinkID=512959">Azure Management Portal</a>:</span></span>
  
  * <span data-ttu-id="96b7c-169">Hello belül **Felhőszolgáltatások** ábrázolása hello Azure felügyeleti portálon, kattintson a felhőalapú szolgáltatás, majd **példányok**, kattintson egy adott példányt, és kattintson a hello **Connect**gombra.</span><span class="sxs-lookup"><span data-stu-id="96b7c-169">Within hello **Cloud Services** view of hello Azure Management portal, click your cloud service, click **Instances**, click a specific instance, and then click hello **Connect** button.</span></span> <span data-ttu-id="96b7c-170">Hello **Connect** gomb hello parancssáv hello következő jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="96b7c-170">hello **Connect** button appears as hello following in hello command bar:</span></span>
    
      ![][ic659273]

  * <span data-ttu-id="96b7c-171">Hello kattintás után **Connect** gomb, felszólító tooopen RDP-fájlba fogja.</span><span class="sxs-lookup"><span data-stu-id="96b7c-171">After clicking hello **Connect** button, you will be prompted tooopen an RDP file.</span></span> <span data-ttu-id="96b7c-172">Nyissa meg a hello fájlt, és hello utasításokat követve.</span><span class="sxs-lookup"><span data-stu-id="96b7c-172">Open hello file and follow hello prompts.</span></span> <span data-ttu-id="96b7c-173">(Képes is mentheti, a fájl tooyour helyi számítógép, és futtassa hello fájl dupla kattintással tooremote napló tooyour a virtuális gép toofirst anélkül hello felügyeleti portálon lépjen.)</span><span class="sxs-lookup"><span data-stu-id="96b7c-173">(You could also save this file tooyour local computer, and then run hello file by double-clicking it tooremote log in tooyour virtual machine without needing toofirst go hello management portal.)</span></span>

  * <span data-ttu-id="96b7c-174">Amikor hello felhasználónevet és jelszót kéri, adja meg a távoli felhasználó hello megadott hello értékeket, és képes toolog a.</span><span class="sxs-lookup"><span data-stu-id="96b7c-174">When you are prompted for hello user name and password, enter hello values that you specified for hello remote user and will be able toolog in.</span></span>

> [!NOTE]
> <span data-ttu-id="96b7c-175">Ha nem Windows operációs rendszeren, meg kell toouse egy távoli asztali ügyfél, amely az operációs rendszer kompatibilis, és kövesse a hello lépéseket tooconfigure, hogy az ügyfélszámítógépek hello beállításokkal letöltött hello RDP-fájlban.</span><span class="sxs-lookup"><span data-stu-id="96b7c-175">If you are on a non-Windows operating system, you need toouse a Remote Desktop client that is compatible with your operating system and follow hello steps tooconfigure that client with hello settings in hello RDP file that you downloaded.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="96b7c-176">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="96b7c-176">See Also</span></span>
<span data-ttu-id="96b7c-177">[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="96b7c-177">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="96b7c-178">[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="96b7c-178">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="96b7c-179">[Hello Azure eszköztára Eclipse telepítése][Installing hello Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="96b7c-179">[Installing hello Azure Toolkit for Eclipse][Installing hello Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="96b7c-180">Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="96b7c-180">For more information about using Azure with Java, see hello [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic712275]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic712275.png
[ic719495]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719495.png
[ic719494]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719494.png
[ic659273]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic659273.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690951.aspx -->
