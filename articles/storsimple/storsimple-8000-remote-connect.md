---
title: "aaaConnect távolról tooyour StorSimple eszköz |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooconfigure távoli kezelésére szolgál az eszközt, és hogyan tooconnect tooWindows PowerShell-lel HTTP vagy HTTPS PROTOKOLLON keresztül."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/07/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 38b6a6350891b9f6f8fdfc55880b2f47105d947c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-remotely-tooyour-storsimple-8000-series-device"></a><span data-ttu-id="19938-103">Távoli csatlakozás tooyour a StorSimple 8000 series eszköz</span><span class="sxs-lookup"><span data-stu-id="19938-103">Connect remotely tooyour StorSimple 8000 series device</span></span>

## <a name="overview"></a><span data-ttu-id="19938-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="19938-104">Overview</span></span>

<span data-ttu-id="19938-105">Tooyour eszköz Windows PowerShell használatával távolról kapcsolódhatnak.</span><span class="sxs-lookup"><span data-stu-id="19938-105">You can remotely connect tooyour device via Windows PowerShell.</span></span> <span data-ttu-id="19938-106">Ezzel a módszerrel csatlakoztatásakor nem látja a menüben.</span><span class="sxs-lookup"><span data-stu-id="19938-106">When you connect this way, you do not see a menu.</span></span> <span data-ttu-id="19938-107">(Megjelenik a menü csak akkor, ha a soros konzol hello hello eszköz tooconnect használ.) A Windows PowerShell-távelérést csatlakozás tooa adott futási térből.</span><span class="sxs-lookup"><span data-stu-id="19938-107">(You see a menu only if you use hello serial console on hello device tooconnect.) With Windows PowerShell remoting, you connect tooa specific runspace.</span></span> <span data-ttu-id="19938-108">Hello megjelenítési nyelv is megadható.</span><span class="sxs-lookup"><span data-stu-id="19938-108">You can also specify hello display language.</span></span>

<span data-ttu-id="19938-109">A Windows PowerShell távoli eljáráshívás toomanage az eszköz használatával kapcsolatos további információkért lépjen túl[a Windows Powershellt használja a StorSimple tooadminister a StorSimple eszköz](storsimple-8000-windows-powershell-administration.md).</span><span class="sxs-lookup"><span data-stu-id="19938-109">For more information about using Windows PowerShell remoting toomanage your device, go too[Use Windows PowerShell for StorSimple tooadminister your StorSimple device](storsimple-8000-windows-powershell-administration.md).</span></span>

<span data-ttu-id="19938-110">Ez az oktatóanyag azt ismerteti, hogyan tooconfigure majd és a távoli felügyeleti eszközét tooconnect tooWindows PowerShell-lel.</span><span class="sxs-lookup"><span data-stu-id="19938-110">This tutorial explains how tooconfigure your device for remote management and then how tooconnect tooWindows PowerShell for StorSimple.</span></span> <span data-ttu-id="19938-111">Használhatja a HTTP vagy HTTPS tooremotely csatlakozás Windows PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="19938-111">You can use HTTP or HTTPS tooremotely connect via Windows PowerShell.</span></span> <span data-ttu-id="19938-112">Azonban meghatározásakor hogyan tooconnect tooWindows PowerShell-lel, fontolja meg a következő információk hello:</span><span class="sxs-lookup"><span data-stu-id="19938-112">However, when you are deciding how tooconnect tooWindows PowerShell for StorSimple, consider hello following information:</span></span>

* <span data-ttu-id="19938-113">Csatlakozás közvetlenül toohello eszköz soros konzoljához biztonságos, de csatlakozó toohello soros konzolon keresztül hálózati kapcsolók nem.</span><span class="sxs-lookup"><span data-stu-id="19938-113">Connecting directly toohello device serial console is secure, but connecting toohello serial console over network switches is not.</span></span> <span data-ttu-id="19938-114">Legyen óvatos az hello biztonsági kockázatot jelent a toohello eszköz soros konzolon keresztül hálózati kapcsolók kapcsolódó.</span><span class="sxs-lookup"><span data-stu-id="19938-114">Be cautious of hello security risk when connecting toohello device serial console over network switches.</span></span>
* <span data-ttu-id="19938-115">Egy HTTP-kapcsolaton keresztül csatlakozó felajánlhatja a további biztonsági mint hello hálózati hello soros konzolon keresztül.</span><span class="sxs-lookup"><span data-stu-id="19938-115">Connecting through an HTTP session might offer more security than connecting through hello serial console over hello network.</span></span> <span data-ttu-id="19938-116">Ez azonban nem hello legbiztonságosabb módszert, az elfogadható megbízható hálózatokon.</span><span class="sxs-lookup"><span data-stu-id="19938-116">Although this is not hello most secure method, it is acceptable on trusted networks.</span></span>
* <span data-ttu-id="19938-117">Hello legbiztonságosabb és ajánlott beállítás hello keresztül egy önaláírt tanúsítványt a HTTPS-KAPCSOLATON keresztül csatlakozó.</span><span class="sxs-lookup"><span data-stu-id="19938-117">Connecting through an HTTPS session with a self-signed certificate is hello most secure and hello recommended option.</span></span>

<span data-ttu-id="19938-118">Csatlakoztathatja távolról toohello Windows PowerShell-felületet.</span><span class="sxs-lookup"><span data-stu-id="19938-118">You can connect remotely toohello Windows PowerShell interface.</span></span> <span data-ttu-id="19938-119">Azonban távelérési tooyour StorSimple eszközt hello Windows PowerShell felületén keresztül nem alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="19938-119">However, remote access tooyour StorSimple device via hello Windows PowerShell interface is not enabled by default.</span></span> <span data-ttu-id="19938-120">Először hello eszköz távoli felügyeletének engedélyezése, és ezután a hello használt tooaccess ügyfélnek az eszközt.</span><span class="sxs-lookup"><span data-stu-id="19938-120">You must enable remote management on hello device first, and then on hello client that is used tooaccess your device.</span></span>

<span data-ttu-id="19938-121">Ebben a cikkben ismertetett hello lépéseket a Windows Server 2012 R2 rendszerű gazdarendszer végrehajtott.</span><span class="sxs-lookup"><span data-stu-id="19938-121">hello steps described in this article were performed on a host system running Windows Server 2012 R2.</span></span>

## <a name="connect-through-http"></a><span data-ttu-id="19938-122">Kapcsolódás HTTP Protokollon keresztül</span><span class="sxs-lookup"><span data-stu-id="19938-122">Connect through HTTP</span></span>

<span data-ttu-id="19938-123">Csatlakozás tooWindows PowerShell StorSimple egy HTTP-kapcsolaton keresztül hello a StorSimple eszköz soros konzolon keresztül csatlakozó-nál nagyobb védelmet kínál.</span><span class="sxs-lookup"><span data-stu-id="19938-123">Connecting tooWindows PowerShell for StorSimple through an HTTP session offers more security than connecting through hello serial console of your StorSimple device.</span></span> <span data-ttu-id="19938-124">Ez azonban nem hello legbiztonságosabb módszert, az elfogadható megbízható hálózatokon.</span><span class="sxs-lookup"><span data-stu-id="19938-124">Although this is not hello most secure method, it is acceptable on trusted networks.</span></span>

<span data-ttu-id="19938-125">Vagy az Azure portál vagy hello soros konzol tooconfigure Távfelügyelet hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="19938-125">You can use either hello Azure portal or hello serial console tooconfigure remote management.</span></span> <span data-ttu-id="19938-126">Válassza ki az alábbi eljárásokat hello:</span><span class="sxs-lookup"><span data-stu-id="19938-126">Select from hello following procedures:</span></span>

* [<span data-ttu-id="19938-127">HTTP Protokollon keresztül hello Azure portál tooenable Távfelügyelet használata</span><span class="sxs-lookup"><span data-stu-id="19938-127">Use hello Azure portal tooenable remote management over HTTP</span></span>](#use-the-azure-classic-portal-to-enable-remote-management-over-http)
* [<span data-ttu-id="19938-128">HTTP Protokollon keresztül hello soros konzol tooenable Távfelügyelet használata</span><span class="sxs-lookup"><span data-stu-id="19938-128">Use hello serial console tooenable remote management over HTTP</span></span>](#use-the-serial-console-to-enable-remote-management-over-http)

<span data-ttu-id="19938-129">Miután engedélyezte távfelügyeletet, használja a következő eljárás tooprepare hello ügyfél a távoli kapcsolat hello.</span><span class="sxs-lookup"><span data-stu-id="19938-129">After you enable remote management, use hello following procedure tooprepare hello client for a remote connection.</span></span>

* [<span data-ttu-id="19938-130">Hello-ügyfél előkészítése a távoli kapcsolat</span><span class="sxs-lookup"><span data-stu-id="19938-130">Prepare hello client for remote connection</span></span>](#prepare-the-client-for-remote-connection)

### <a name="use-hello-azure-portal-tooenable-remote-management-over-http"></a><span data-ttu-id="19938-131">HTTP Protokollon keresztül hello Azure portál tooenable Távfelügyelet használata</span><span class="sxs-lookup"><span data-stu-id="19938-131">Use hello Azure portal tooenable remote management over HTTP</span></span>

<span data-ttu-id="19938-132">Hajtsa végre a HTTP Protokollon keresztül hello Azure portál tooenable Távfelügyelet lépések hello.</span><span class="sxs-lookup"><span data-stu-id="19938-132">Perform hello following steps in hello Azure portal tooenable remote management over HTTP.</span></span>

#### <a name="tooenable-remote-management-through-hello-azure-portal"></a><span data-ttu-id="19938-133">távoli felügyeleti tooenable hello Azure-portálon keresztül</span><span class="sxs-lookup"><span data-stu-id="19938-133">tooenable remote management through hello Azure portal</span></span>

1. <span data-ttu-id="19938-134">Nyissa meg tooyour StorSimple Device Manager szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="19938-134">Go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="19938-135">Válassza ki **eszközök** majd válassza ki, és kattintson a hello eszközre tooconfigure távoli kezelésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="19938-135">Select **Devices** and then select and click hello device you want tooconfigure for remote management.</span></span> <span data-ttu-id="19938-136">Nyissa meg túl**eszközbeállítások > biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="19938-136">Go too**Device settings > Security**.</span></span>
2. <span data-ttu-id="19938-137">A hello **biztonsági beállítások** panelen kattintson a **Távfelügyelet**.</span><span class="sxs-lookup"><span data-stu-id="19938-137">In hello **Security settings** blade, click **Remote Management**.</span></span>
3. <span data-ttu-id="19938-138">A hello **Távfelügyelet** panelen állítsa **távoli felügyelet engedélyezése** túl**Igen**.</span><span class="sxs-lookup"><span data-stu-id="19938-138">In hello **Remote management** blade, set **Enable Remote Management** too**Yes**.</span></span>
4. <span data-ttu-id="19938-139">Ezután eldöntheti, tooconnect HTTP-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="19938-139">You can now choose tooconnect using HTTP.</span></span> <span data-ttu-id="19938-140">(hello alapértelmezett érték tooconnect HTTPS-KAPCSOLATON keresztül.) Győződjön meg arról, hogy a HTTP van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="19938-140">(hello default is tooconnect over HTTPS.) Make sure that HTTP is selected.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="19938-141">A HTTP-n keresztüli csatlakozást kizárólag megbízható hálózatokon célszerű engedélyezni.</span><span class="sxs-lookup"><span data-stu-id="19938-141">Connecting over HTTP is acceptable only on trusted networks.</span></span>
   
5. <span data-ttu-id="19938-142">Kattintson a **mentése** és megerősítést kér, amikor **Igen**.</span><span class="sxs-lookup"><span data-stu-id="19938-142">Click **Save** and when prompted for confirmation, select **Yes**.</span></span>

### <a name="use-hello-serial-console-tooenable-remote-management-over-http"></a><span data-ttu-id="19938-143">HTTP Protokollon keresztül hello soros konzol tooenable Távfelügyelet használata</span><span class="sxs-lookup"><span data-stu-id="19938-143">Use hello serial console tooenable remote management over HTTP</span></span>
<span data-ttu-id="19938-144">Hajtsa végre a következő lépéseket a hello eszköz soros konzoljához tooenable távoli felügyeleti hello.</span><span class="sxs-lookup"><span data-stu-id="19938-144">Perform hello following steps on hello device serial console tooenable remote management.</span></span>

#### <a name="tooenable-remote-management-through-hello-device-serial-console"></a><span data-ttu-id="19938-145">távoli felügyeleti tooenable hello eszköz soros konzolon keresztül</span><span class="sxs-lookup"><span data-stu-id="19938-145">tooenable remote management through hello device serial console</span></span>
1. <span data-ttu-id="19938-146">A hello soros konzol menüben válassza a 1. lehetőség.</span><span class="sxs-lookup"><span data-stu-id="19938-146">On hello serial console menu, select option 1.</span></span> <span data-ttu-id="19938-147">Hello eszközön hello soros konzol használatával kapcsolatos további információkért lépjen túl[tooWindows PowerShell csatlakoztatni a StorSimple eszköz soros konzolon keresztül](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="19938-147">For more information about using hello serial console on hello device, go too[Connect tooWindows PowerShell for StorSimple via device serial console](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span></span>
2. <span data-ttu-id="19938-148">Hello, írja be a parancssorba:`Enable-HcsRemoteManagement –AllowHttp`</span><span class="sxs-lookup"><span data-stu-id="19938-148">At hello prompt, type: `Enable-HcsRemoteManagement –AllowHttp`</span></span>
3. <span data-ttu-id="19938-149">Hello biztonsági rések HTTP tooconnect toohello eszköz használatával kapcsolatban értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="19938-149">You are notified about hello security vulnerabilities of using HTTP tooconnect toohello device.</span></span> <span data-ttu-id="19938-150">Amikor a rendszer kéri, erősítse meg, írja be a **Y**.</span><span class="sxs-lookup"><span data-stu-id="19938-150">When prompted, confirm by typing **Y**.</span></span>
4. <span data-ttu-id="19938-151">Győződjön meg arról, hogy a HTTP engedélyezett beírásával:`Get-HcsSystem`</span><span class="sxs-lookup"><span data-stu-id="19938-151">Verify that HTTP is enabled by typing: `Get-HcsSystem`</span></span>
5. <span data-ttu-id="19938-152">Győződjön meg arról, hogy hello **RemoteManagementMode** mezőben látható **HttpsAndHttpEnabled**.hello a következő ábrán az ezek a beállítások a PuTTY jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="19938-152">Verify that hello **RemoteManagementMode** field shows **HttpsAndHttpEnabled**.hello following illustration shows these settings in PuTTY.</span></span>
   
     ![Soros HTTPS- és HTTP engedélyezve](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-hello-client-for-remote-connection"></a><span data-ttu-id="19938-154">Hello-ügyfél előkészítése a távoli kapcsolat</span><span class="sxs-lookup"><span data-stu-id="19938-154">Prepare hello client for remote connection</span></span>
<span data-ttu-id="19938-155">Hajtsa végre a következő lépéseket a hello ügyfél tooenable távoli felügyeleti hello.</span><span class="sxs-lookup"><span data-stu-id="19938-155">Perform hello following steps on hello client tooenable remote management.</span></span>

#### <a name="tooprepare-hello-client-for-remote-connection"></a><span data-ttu-id="19938-156">a távoli kapcsolat tooprepare hello ügyfél</span><span class="sxs-lookup"><span data-stu-id="19938-156">tooprepare hello client for remote connection</span></span>
1. <span data-ttu-id="19938-157">Indítsa el a Windows PowerShell-munkamenetet rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="19938-157">Start a Windows PowerShell session as an administrator.</span></span>
2. <span data-ttu-id="19938-158">Írja be a következő parancs tooadd hello IP-cím hello StorSimple eszköz toohello ügyfél megbízható gazdagépek listájának hello:</span><span class="sxs-lookup"><span data-stu-id="19938-158">Type hello following command tooadd hello IP address of hello StorSimple device toohello client’s trusted hosts list:</span></span>
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
     <span data-ttu-id="19938-159">Cserélje le <*device_ip*> hello IP-cím, az eszköz; például:</span><span class="sxs-lookup"><span data-stu-id="19938-159">Replace <*device_ip*> with hello IP address of your device; for example:</span></span> 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. <span data-ttu-id="19938-160">Írja be a következő parancs toosave hello eszközhitelesítő adatok változóként hello:</span><span class="sxs-lookup"><span data-stu-id="19938-160">Type hello following command toosave hello device credentials in a variable:</span></span> 
   
    ```
    $cred = Get-Credential
    ```
    
4. <span data-ttu-id="19938-161">Hello párbeszédpanel, amely akkor jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="19938-161">In hello dialog box that appears:</span></span>
   
   1. <span data-ttu-id="19938-162">Írja be a hello felhasználónevet a következő formátumban: *device_ip\SSAdmin*.</span><span class="sxs-lookup"><span data-stu-id="19938-162">Type hello user name in this format: *device_ip\SSAdmin*.</span></span>
   2. <span data-ttu-id="19938-163">Írja be a hello eszköz rendszergazdai jelszava lett beállítva, amikor hello eszköz hello beállítása varázsló lett konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="19938-163">Type hello device administrator password that was set when hello device was configured with hello setup wizard.</span></span> <span data-ttu-id="19938-164">hello alapértelmezett jelszó *jelszó1*.</span><span class="sxs-lookup"><span data-stu-id="19938-164">hello default password is *Password1*.</span></span>
5. <span data-ttu-id="19938-165">Ez a parancs beírásával indítsa el a Windows PowerShell-munkamenetet hello eszközön:</span><span class="sxs-lookup"><span data-stu-id="19938-165">Start a Windows PowerShell session on hello device by typing this command:</span></span>
   
     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`
   
   > [!NOTE]
   > <span data-ttu-id="19938-166">Windows PowerShell-munkamenet hello StorSimple virtuális eszköz való használatra toocreate hozzáfűzése hello `–Port` paraméter, és adja meg a StorSimple virtuális készülék távoli eljáráshívási konfigurált hello nyilvános port.</span><span class="sxs-lookup"><span data-stu-id="19938-166">toocreate a Windows PowerShell session for use with hello StorSimple virtual device, append hello `–Port` parameter and specify hello public port that you configured in Remoting for StorSimple Virtual Appliance.</span></span>
   
   
<span data-ttu-id="19938-167">Ezen a ponton rendelkeznie kell egy aktív távoli Windows PowerShell munkamenet toohello eszköz.</span><span class="sxs-lookup"><span data-stu-id="19938-167">At this point, you should have an active remote Windows PowerShell session toohello device.</span></span>
   
![PowerShell távvezérlése HTTP-n keresztül](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a><span data-ttu-id="19938-169">HTTPS keresztül csatlakozni</span><span class="sxs-lookup"><span data-stu-id="19938-169">Connect through HTTPS</span></span>

<span data-ttu-id="19938-170">StorSimple tooWindows PowerShell csatlakozás egy HTTPS-kapcsolaton keresztül hello legbiztonságosabb és ajánlott távolról csatlakozó tooyour Microsoft Azure StorSimple eszköz metódusában.</span><span class="sxs-lookup"><span data-stu-id="19938-170">Connecting tooWindows PowerShell for StorSimple through an HTTPS session is hello most secure and recommended method of remotely connecting tooyour Microsoft Azure StorSimple device.</span></span> <span data-ttu-id="19938-171">hello a következő eljárások azt ismertetik, hogyan mentése tooset hello-e soros konzol és az ügyfél számítógépeken, hogy a StorSimple HTTPS tooconnect tooWindows PowerShell használható.</span><span class="sxs-lookup"><span data-stu-id="19938-171">hello following procedures explain how tooset up hello serial console and client computers so that you can use HTTPS tooconnect tooWindows PowerShell for StorSimple.</span></span>

<span data-ttu-id="19938-172">Vagy az Azure portál vagy hello soros konzol tooconfigure Távfelügyelet hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="19938-172">You can use either hello Azure portal or hello serial console tooconfigure remote management.</span></span> <span data-ttu-id="19938-173">Válassza ki az alábbi eljárásokat hello:</span><span class="sxs-lookup"><span data-stu-id="19938-173">Select from hello following procedures:</span></span>

* [<span data-ttu-id="19938-174">Hello Azure portál tooenable Távfelügyelet használja a HTTPS-KAPCSOLATON keresztül</span><span class="sxs-lookup"><span data-stu-id="19938-174">Use hello Azure portal tooenable remote management over HTTPS</span></span>](#use-the-azure-classic-portal-to-enable-remote-management-over-https)
* [<span data-ttu-id="19938-175">Hello soros konzol tooenable Távfelügyelet használja a HTTPS-KAPCSOLATON keresztül</span><span class="sxs-lookup"><span data-stu-id="19938-175">Use hello serial console tooenable remote management over HTTPS</span></span>](#use-the-serial-console-to-enable-remote-management-over-https)

<span data-ttu-id="19938-176">Miután engedélyezte távfelügyeletet, használjon a következő eljárások tooprepare hello állomás olyan távoli kezelési hello és toohello eszköz hello távoli állomástól.</span><span class="sxs-lookup"><span data-stu-id="19938-176">After you enable remote management, use hello following procedures tooprepare hello host for a remote management and connect toohello device from hello remote host.</span></span>

* [<span data-ttu-id="19938-177">Hello állomás Felkészülés a távfelügyeletre</span><span class="sxs-lookup"><span data-stu-id="19938-177">Prepare hello host for remote management</span></span>](#prepare-the-host-for-remote-management)
* [<span data-ttu-id="19938-178">A távoli állomástól hello toohello eszköz csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="19938-178">Connect toohello device from hello remote host</span></span>](#connect-to-the-device-from-the-remote-host)

### <a name="use-hello-azure-portal-tooenable-remote-management-over-https"></a><span data-ttu-id="19938-179">Hello Azure portál tooenable Távfelügyelet használja a HTTPS-KAPCSOLATON keresztül</span><span class="sxs-lookup"><span data-stu-id="19938-179">Use hello Azure portal tooenable remote management over HTTPS</span></span>

<span data-ttu-id="19938-180">Hajtsa végre a HTTPS-KAPCSOLATON keresztül hello Azure portál tooenable Távfelügyelet lépések hello.</span><span class="sxs-lookup"><span data-stu-id="19938-180">Perform hello following steps in hello Azure portal tooenable remote management over HTTPS.</span></span>

#### <a name="tooenable-remote-management-over-https-from-hello-azure-portal"></a><span data-ttu-id="19938-181">tooenable Távfelügyelet HTTPS-KAPCSOLATON keresztül a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="19938-181">tooenable remote management over HTTPS from hello Azure portal</span></span>

1. <span data-ttu-id="19938-182">Nyissa meg tooyour StorSimple Device Manager szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="19938-182">Go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="19938-183">Válassza ki **eszközök** majd válassza ki, és kattintson a hello eszközre tooconfigure távoli kezelésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="19938-183">Select **Devices** and then select and click hello device you want tooconfigure for remote management.</span></span> <span data-ttu-id="19938-184">Nyissa meg túl**eszközbeállítások > biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="19938-184">Go too**Device settings > Security**.</span></span>
2. <span data-ttu-id="19938-185">A hello **biztonsági beállítások** panelen kattintson a **Távfelügyelet**.</span><span class="sxs-lookup"><span data-stu-id="19938-185">In hello **Security settings** blade, click **Remote Management**.</span></span>
3. <span data-ttu-id="19938-186">Állítsa be **távoli felügyelet engedélyezése** túl**Igen**.</span><span class="sxs-lookup"><span data-stu-id="19938-186">Set **Enable Remote Management** too**Yes**.</span></span>
4. <span data-ttu-id="19938-187">Ezután eldöntheti, tooconnect HTTPS-kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="19938-187">You can now choose tooconnect using HTTPS.</span></span> <span data-ttu-id="19938-188">(hello alapértelmezett érték tooconnect HTTPS-KAPCSOLATON keresztül.) Győződjön meg arról, hogy HTTPS van-e kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="19938-188">(hello default is tooconnect over HTTPS.) Make sure that HTTPS is selected.</span></span>
5. <span data-ttu-id="19938-189">Kattintson... majd **távfelügyeleti tanúsítvány letöltése**.</span><span class="sxs-lookup"><span data-stu-id="19938-189">Click ... and then click **Download Remote Management Certificate**.</span></span> <span data-ttu-id="19938-190">Adjon meg egy helyet toosave ezt a fájlt.</span><span class="sxs-lookup"><span data-stu-id="19938-190">Specify a location toosave this file.</span></span> <span data-ttu-id="19938-191">Tooinstall kell ezt a tanúsítványt hello ügyfélszámítógépre vagy állomásra számítógép tooconnect toohello eszköz használt.</span><span class="sxs-lookup"><span data-stu-id="19938-191">You need tooinstall this certificate on hello client or host computer that you will use tooconnect toohello device.</span></span>
6. <span data-ttu-id="19938-192">Kattintson a **mentése** majd **Igen** során a rendszer megerősítést kér.</span><span class="sxs-lookup"><span data-stu-id="19938-192">Click **Save** and then click **Yes** when prompted for confirmation.</span></span>

### <a name="use-hello-serial-console-tooenable-remote-management-over-https"></a><span data-ttu-id="19938-193">Hello soros konzol tooenable Távfelügyelet használja a HTTPS-KAPCSOLATON keresztül</span><span class="sxs-lookup"><span data-stu-id="19938-193">Use hello serial console tooenable remote management over HTTPS</span></span>

<span data-ttu-id="19938-194">Hajtsa végre a következő lépéseket a hello eszköz soros konzoljához tooenable távoli felügyeleti hello.</span><span class="sxs-lookup"><span data-stu-id="19938-194">Perform hello following steps on hello device serial console tooenable remote management.</span></span>

#### <a name="tooenable-remote-management-through-hello-device-serial-console"></a><span data-ttu-id="19938-195">távoli felügyeleti tooenable hello eszköz soros konzolon keresztül</span><span class="sxs-lookup"><span data-stu-id="19938-195">tooenable remote management through hello device serial console</span></span>
1. <span data-ttu-id="19938-196">A hello soros konzol menüben válassza a 1. lehetőség.</span><span class="sxs-lookup"><span data-stu-id="19938-196">On hello serial console menu, select option 1.</span></span> <span data-ttu-id="19938-197">Hello eszközön hello soros konzol használatával kapcsolatos további információkért lépjen túl[tooWindows PowerShell csatlakoztatni a StorSimple eszköz soros konzolon keresztül](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="19938-197">For more information about using hello serial console on hello device, go too[Connect tooWindows PowerShell for StorSimple via device serial console](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span></span>
2. <span data-ttu-id="19938-198">Hello, írja be a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="19938-198">At hello prompt, type:</span></span>
   
     `Enable-HcsRemoteManagement`
   
    <span data-ttu-id="19938-199">Ez engedélyezze a HTTPS-eszközön.</span><span class="sxs-lookup"><span data-stu-id="19938-199">This should enable HTTPS on your device.</span></span>
3. <span data-ttu-id="19938-200">Ellenőrizze, hogy engedélyezték-e a HTTPS beírásával:</span><span class="sxs-lookup"><span data-stu-id="19938-200">Verify that HTTPS has been enabled by typing:</span></span> 
   
     `Get-HcsSystem`
   
    <span data-ttu-id="19938-201">Győződjön meg arról, hogy hello **RemoteManagementMode** mezőben látható **HttpsEnabled**.hello a következő ábrán az ezek a beállítások a PuTTY jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="19938-201">Make sure that hello **RemoteManagementMode** field shows **HttpsEnabled**.hello following illustration shows these settings in PuTTY.</span></span>
   
     ![Soros HTTPS-kompatibilis](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)
4. <span data-ttu-id="19938-203">Hello kimenetét a `Get-HcsSystem`, hello eszköz sorozatszámát hello másolja ki és mentse azt későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="19938-203">From hello output of `Get-HcsSystem`, copy hello serial number of hello device and save it for later use.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="19938-204">hello sorozatszám maps toohello hello tanúsítvány CN-név.</span><span class="sxs-lookup"><span data-stu-id="19938-204">hello serial number maps toohello CN name in hello certificate.</span></span>
   
5. <span data-ttu-id="19938-205">Írja be a távoli felügyeleti tanúsítvány beszerzése:</span><span class="sxs-lookup"><span data-stu-id="19938-205">Obtain a remote management certificate by typing:</span></span> 
   
     `Get-HcsRemoteManagementCert`
   
    <span data-ttu-id="19938-206">A tanúsítvány hasonló toohello következő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="19938-206">A certificate similar toohello following will appear.</span></span>
   
    ![Távfelügyeleti tanúsítvány beszerzése](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)
6. <span data-ttu-id="19938-208">Hello adatok másolása hello tanúsítványban lévő **---BEGIN CERTIFICATE---** túl**---vége tanúsítvány---** egy szövegszerkesztőbe például a Jegyzettömbben, és mentse azt egy .cer fájlba.</span><span class="sxs-lookup"><span data-stu-id="19938-208">Copy hello information in hello certificate from **-----BEGIN CERTIFICATE-----** too**-----END CERTIFICATE-----** into a text editor such as Notepad, and save it as a .cer file.</span></span> <span data-ttu-id="19938-209">(Fogja másolni a fájl tooyour távoli állomás, hello állomás előkészítésekor.)</span><span class="sxs-lookup"><span data-stu-id="19938-209">(You will copy this file tooyour remote host when you prepare hello host.)</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="19938-210">toogenerate egy új tanúsítvány használatára hello `Set-HcsRemoteManagementCert` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="19938-210">toogenerate a new certificate, use hello `Set-HcsRemoteManagementCert` cmdlet.</span></span>
   
### <a name="prepare-hello-host-for-remote-management"></a><span data-ttu-id="19938-211">Hello állomás Felkészülés a távfelügyeletre</span><span class="sxs-lookup"><span data-stu-id="19938-211">Prepare hello host for remote management</span></span>

<span data-ttu-id="19938-212">tooprepare hello fogadó számítógép a távoli kapcsolat által használt HTTPS-KAPCSOLATON keresztül, hajtsa végre az alábbi eljárásokat hello:</span><span class="sxs-lookup"><span data-stu-id="19938-212">tooprepare hello host computer for a remote connection that uses an HTTPS session, perform hello following procedures:</span></span>

* <span data-ttu-id="19938-213">[Importálási hello .cer fájl hello ügyfél vagy a távoli állomás hello legfelső szintű tárolóba](#to-import-the-certificate-on-the-remote-host).</span><span class="sxs-lookup"><span data-stu-id="19938-213">[Import hello .cer file into hello root store of hello client or remote host](#to-import-the-certificate-on-the-remote-host).</span></span>
* <span data-ttu-id="19938-214">[Adjon hozzá hello eszköz sorozatszámokat toohello hosts fájlt a távoli állomáson levő](#to-add-device-serial-numbers-to-the-remote-host).</span><span class="sxs-lookup"><span data-stu-id="19938-214">[Add hello device serial numbers toohello hosts file on your remote host](#to-add-device-serial-numbers-to-the-remote-host).</span></span>

<span data-ttu-id="19938-215">Hello megelőző eljárások leírását alább.</span><span class="sxs-lookup"><span data-stu-id="19938-215">Each of hello preceding procedures, is described below.</span></span>

#### <a name="tooimport-hello-certificate-on-hello-remote-host"></a><span data-ttu-id="19938-216">hello távoli állomáson levő tooimport hello tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="19938-216">tooimport hello certificate on hello remote host</span></span>
1. <span data-ttu-id="19938-217">Kattintson a jobb gombbal a hello .cer fájlt, és válassza ki **telepítés tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="19938-217">Right-click hello .cer file and select **Install certificate**.</span></span> <span data-ttu-id="19938-218">Hello Tanúsítványimportáló varázsló elindul.</span><span class="sxs-lookup"><span data-stu-id="19938-218">This starts hello Certificate Import Wizard.</span></span>
   
    ![Tanúsítványimportáló varázsló 1](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)
2. <span data-ttu-id="19938-220">A **tárolóhelyére**, jelölje be **helyi számítógép**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="19938-220">For **Store location**, select **Local Machine**, and then click **Next**.</span></span>
3. <span data-ttu-id="19938-221">Válassza ki **minden tanúsítvány tárolása a következő tároló hello**, és kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="19938-221">Select **Place all certificates in hello following store**, and then click **Browse**.</span></span> <span data-ttu-id="19938-222">Keresse meg a távoli állomás toohello főtárolójához, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="19938-222">Navigate toohello root store of your remote host, and then click **Next**.</span></span>
   
    ![Tanúsítványimportáló varázsló 2. régiója](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)
4. <span data-ttu-id="19938-224">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="19938-224">Click **Finish**.</span></span> <span data-ttu-id="19938-225">Megjelenik egy üzenet, amely közli, hogy hello importálása sikeres volt-e.</span><span class="sxs-lookup"><span data-stu-id="19938-225">A message that tells you that hello import was successful appears.</span></span>
   
    ![Tanúsítványimportáló varázsló 3](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="tooadd-device-serial-numbers-toohello-remote-host"></a><span data-ttu-id="19938-227">tooadd eszköz sorozatszámokat toohello távoli állomás</span><span class="sxs-lookup"><span data-stu-id="19938-227">tooadd device serial numbers toohello remote host</span></span>
1. <span data-ttu-id="19938-228">Rendszergazdaként a Jegyzettömb, és nyissa meg a \Windows\System32\Drivers\etc hello állomások fájlba.</span><span class="sxs-lookup"><span data-stu-id="19938-228">Start Notepad as an administrator, and then open hello hosts file located at \Windows\System32\Drivers\etc.</span></span>
2. <span data-ttu-id="19938-229">Adja hozzá a következő három bejegyzések tooyour hosts fájl hello: **DATA 0 IP-cím**, **0 vezérlő rögzített IP-cím**, és **vezérlő 1 fix IP-cím**.</span><span class="sxs-lookup"><span data-stu-id="19938-229">Add hello following three entries tooyour hosts file: **DATA 0 IP address**, **Controller 0 Fixed IP address**, and **Controller 1 Fixed IP address**.</span></span>
3. <span data-ttu-id="19938-230">Adja meg a korábban mentett hello eszköz gyári számát.</span><span class="sxs-lookup"><span data-stu-id="19938-230">Enter hello device serial number that you saved earlier.</span></span> <span data-ttu-id="19938-231">Képezze le toohello IP-cím, ahogy az a következő kép hello.</span><span class="sxs-lookup"><span data-stu-id="19938-231">Map this toohello IP address as shown in hello following image.</span></span> <span data-ttu-id="19938-232">A vezérlő 0 és a vezérlő 1 hozzáfűzése **Controller0** és **Controller1** hello végén lévő hello sorozatszám (CN-név).</span><span class="sxs-lookup"><span data-stu-id="19938-232">For Controller 0 and Controller 1, append **Controller0** and **Controller1** at hello end of hello serial number (CN name).</span></span>
   
    ![CN-név toohosts fájl hozzáadása](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)
4. <span data-ttu-id="19938-234">Mentés hello hosts fájlt.</span><span class="sxs-lookup"><span data-stu-id="19938-234">Save hello hosts file.</span></span>

### <a name="connect-toohello-device-from-hello-remote-host"></a><span data-ttu-id="19938-235">A távoli állomástól hello toohello eszköz csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="19938-235">Connect toohello device from hello remote host</span></span>

<span data-ttu-id="19938-236">Windows PowerShell és az SSL használata az eszközön, ügyfél vagy egy távoli állomástól SSAdmin munkamenet egy tooenter.</span><span class="sxs-lookup"><span data-stu-id="19938-236">Use Windows PowerShell and SSL tooenter an SSAdmin session on your device from a remote host or client.</span></span> <span data-ttu-id="19938-237">hello SSAdmin munkamenet leképezi a hello 1 toooption [soros konzol](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) menü az eszköz.</span><span class="sxs-lookup"><span data-stu-id="19938-237">hello SSAdmin session maps toooption 1 in hello [serial console](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) menu of your device.</span></span>

<span data-ttu-id="19938-238">Hajtsa végre a következő eljárás, amelyből el kívánja toomake hello távoli Windows PowerShell-kapcsolat hello számítógépen hello.</span><span class="sxs-lookup"><span data-stu-id="19938-238">Perform hello following procedure on hello computer from which you want toomake hello remote Windows PowerShell connection.</span></span>

#### <a name="tooenter-an-ssadmin-session-on-hello-device-by-using-windows-powershell-and-ssl"></a><span data-ttu-id="19938-239">egy Windows PowerShell és az SSL használatával hello eszközön SSAdmin munkamenet tooenter</span><span class="sxs-lookup"><span data-stu-id="19938-239">tooenter an SSAdmin session on hello device by using Windows PowerShell and SSL</span></span>
1. <span data-ttu-id="19938-240">Indítsa el a Windows PowerShell-munkamenetet rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="19938-240">Start a Windows PowerShell session as an administrator.</span></span>
2. <span data-ttu-id="19938-241">Írja be a hello eszköz IP címe toohello ügyfél megbízható gazdagépeket vehet fel:</span><span class="sxs-lookup"><span data-stu-id="19938-241">Add hello device IP address toohello client’s trusted hosts by typing:</span></span>
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
    <span data-ttu-id="19938-242">Ahol <*device_ip*> hello IP-cím, az eszköz, például:</span><span class="sxs-lookup"><span data-stu-id="19938-242">Where <*device_ip*> is hello IP address of your device; for example:</span></span> 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. <span data-ttu-id="19938-243">toocreate új hitelesítő adatokat, írja be:</span><span class="sxs-lookup"><span data-stu-id="19938-243">toocreate a new credential, type:</span></span>
   
     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`
   
    <span data-ttu-id="19938-244">Ahol <*IP-címe céleszközön*> DATA 0 az eszközhöz; hello IP-címe például **10.126.173.90** hello gazdafájl képe megelőző hello ismertetett módon.</span><span class="sxs-lookup"><span data-stu-id="19938-244">Where <*IP of target device*> is hello IP address of DATA 0 for your device; for example, **10.126.173.90** as shown in hello preceding image of hello hosts file.</span></span> <span data-ttu-id="19938-245">Emellett adja meg az eszköz rendszergazdai jelszava hello.</span><span class="sxs-lookup"><span data-stu-id="19938-245">Also, supply hello administrator password for your device.</span></span>
4. <span data-ttu-id="19938-246">Írja be a munkamenet létrehozása:</span><span class="sxs-lookup"><span data-stu-id="19938-246">Create a session by typing:</span></span>
   
     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`
   
    <span data-ttu-id="19938-247">-ComputerName paramétere hello hello parancsmagot, adja meg a hello <*figyelt eszköz sorozatszámát*>.</span><span class="sxs-lookup"><span data-stu-id="19938-247">For hello -ComputerName parameter in hello cmdlet, provide hello <*serial number of target device*>.</span></span> <span data-ttu-id="19938-248">Ezzel a sorozatszámmal leképezve a DATA 0 hello állomások fájlba a távoli állomáson; toohello IP-címe például **SHX0991003G44MT** hello kép a következő ábrán.</span><span class="sxs-lookup"><span data-stu-id="19938-248">This serial number was mapped toohello IP address of DATA 0 in hello hosts file on your remote host; for example, **SHX0991003G44MT** as shown in hello following image.</span></span>
5. <span data-ttu-id="19938-249">Típus:</span><span class="sxs-lookup"><span data-stu-id="19938-249">Type:</span></span>
   
     `Enter-PSSession $session`
6. <span data-ttu-id="19938-250">Kell toowait néhány percet, és akkor lesz csatlakoztatott tooyour eszközt keresztül HTTPS SSL-en keresztül.</span><span class="sxs-lookup"><span data-stu-id="19938-250">You will need toowait a few minutes, and then you will be connected tooyour device via HTTPS over SSL.</span></span> <span data-ttu-id="19938-251">Megjelenik egy üzenet, amely jelzi, hogy csatlakoztatott tooyour eszköz.</span><span class="sxs-lookup"><span data-stu-id="19938-251">You see a message that indicates you are connected tooyour device.</span></span>
   
    ![PowerShell távvezérlése HTTPS és az SSL használata](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a><span data-ttu-id="19938-253">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="19938-253">Next steps</span></span>

* <span data-ttu-id="19938-254">További információ [Windows PowerShell tooadminister StorSimple-eszköz használata az](storsimple-8000-windows-powershell-administration.md).</span><span class="sxs-lookup"><span data-stu-id="19938-254">Learn more about [using Windows PowerShell tooadminister your StorSimple device](storsimple-8000-windows-powershell-administration.md).</span></span>
* <span data-ttu-id="19938-255">További információ [használatával hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="19938-255">Learn more about [using hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

