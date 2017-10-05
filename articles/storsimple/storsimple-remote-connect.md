---
title: "Távoli csatlakozás a StorSimple eszköz |} Microsoft Docs"
description: "Távoli kezelésére szolgál az eszköz konfigurálása és a StorSimple HTTP vagy HTTPS PROTOKOLLON keresztül és a Windows PowerShell összekapcsolása mutatja be."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 923377aa-f451-4656-87de-5e95a34a6a2a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b916173e127394d3ea06eded36285bdbbf884b12
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connect-remotely-to-your-storsimple-8000-series-device"></a><span data-ttu-id="682f2-103">Távoli csatlakozás a StorSimple 8000 series eszköz</span><span class="sxs-lookup"><span data-stu-id="682f2-103">Connect remotely to your StorSimple 8000 series device</span></span>

## <a name="overview"></a><span data-ttu-id="682f2-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="682f2-104">Overview</span></span>
<span data-ttu-id="682f2-105">A Windows PowerShell távoli eljáráshívás segítségével a StorSimple eszköz csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="682f2-105">You can use Windows PowerShell remoting to connect to your StorSimple device.</span></span> <span data-ttu-id="682f2-106">Ezzel a módszerrel csatlakoztatásakor menü nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="682f2-106">When you connect this way, you will not see a menu.</span></span> <span data-ttu-id="682f2-107">(Megjelenik a menü csak akkor, ha a soros konzol az eszközön való csatlakozáshoz használ.) A Windows PowerShell-távelérést csatlakozhat egy adott futási teret.</span><span class="sxs-lookup"><span data-stu-id="682f2-107">(You see a menu only if you use the serial console on the device to connect.) With Windows PowerShell remoting, you connect to a specific runspace.</span></span> <span data-ttu-id="682f2-108">A megjelenítési nyelv is megadható.</span><span class="sxs-lookup"><span data-stu-id="682f2-108">You can also specify the display language.</span></span> 

<span data-ttu-id="682f2-109">Az eszköz kezelését Windows PowerShell távoli eljáráshívás használatával kapcsolatos további információkért látogasson el [használata a Windows PowerShell-lel felügyelete a StorSimple eszköz](storsimple-windows-powershell-administration.md).</span><span class="sxs-lookup"><span data-stu-id="682f2-109">For more information about using Windows PowerShell remoting to manage your device, go to [Use Windows PowerShell for StorSimple to administer your StorSimple device](storsimple-windows-powershell-administration.md).</span></span>

<span data-ttu-id="682f2-110">Ez az oktatóanyag azt ismerteti, az eszköz a távoli felügyelet beállítása, majd a StorSimple és a Windows PowerShell összekapcsolása.</span><span class="sxs-lookup"><span data-stu-id="682f2-110">This tutorial explains how to configure your device for remote management and then how to connect to Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="682f2-111">HTTP vagy HTTPS használatával Windows PowerShell távoli eljáráshívás keresztül csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="682f2-111">You can use HTTP or HTTPS to connect via Windows PowerShell remoting.</span></span> <span data-ttu-id="682f2-112">Azonban a StorSimple és a Windows PowerShell összekapcsolása dönt, vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="682f2-112">However, when you are deciding how to connect to Windows PowerShell for StorSimple, consider the following:</span></span> 

* <span data-ttu-id="682f2-113">Az eszköz soros konzoljához való közvetlen csatlakozás biztonságos, de nincs csatlakozás soros konzolon keresztül hálózati kapcsolók.</span><span class="sxs-lookup"><span data-stu-id="682f2-113">Connecting directly to the device serial console is secure, but connecting to the serial console over network switches is not.</span></span> <span data-ttu-id="682f2-114">Legyen óvatos, a biztonsági kockázat, hálózati kapcsolók keresztül az eszköz soros konzoljához való kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="682f2-114">Be cautious of the security risk when connecting to the device serial console over network switches.</span></span> 
* <span data-ttu-id="682f2-115">Egy HTTP-kapcsolaton keresztül csatlakozó felajánlhatja a nagyobb biztonság, mint a hálózaton keresztül a soros konzolon keresztül kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="682f2-115">Connecting through an HTTP session might offer more security than connecting through the serial console over the network.</span></span> <span data-ttu-id="682f2-116">Bár ez nem a legbiztonságosabb módszer, akkor elfogadható megbízható hálózatokon.</span><span class="sxs-lookup"><span data-stu-id="682f2-116">Although this is not the most secure method, it is acceptable on trusted networks.</span></span> 
* <span data-ttu-id="682f2-117">Keresztül egy önaláírt tanúsítványt a HTTPS-KAPCSOLATON keresztül csatlakozó értéke a legbiztonságosabb és ajánlott.</span><span class="sxs-lookup"><span data-stu-id="682f2-117">Connecting through an HTTPS session with a self-signed certificate is the most secure and the recommended option.</span></span>

<span data-ttu-id="682f2-118">Kapcsolódás távolról a Windows PowerShell-felületet.</span><span class="sxs-lookup"><span data-stu-id="682f2-118">You can connect remotely to the Windows PowerShell interface.</span></span> <span data-ttu-id="682f2-119">Azonban a Windows PowerShell felületén keresztül a StorSimple eszközhöz való távoli hozzáféréssel nem alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="682f2-119">However, remote access to your StorSimple device via the Windows PowerShell interface is not enabled by default.</span></span> <span data-ttu-id="682f2-120">Az eszköz távoli kezeléséhez először engedélyeznie kell, és ezután az ügyfélen, amelynek használatával az eszközhöz való hozzáféréshez.</span><span class="sxs-lookup"><span data-stu-id="682f2-120">You need to enable remote management on the device first, and then on the client that is used to access your device.</span></span>

<span data-ttu-id="682f2-121">Ebben a cikkben leírt lépéseket a Windows Server 2012 R2 rendszerű gazdarendszer végrehajtott.</span><span class="sxs-lookup"><span data-stu-id="682f2-121">The steps described in this article were performed on a host system running Windows Server 2012 R2.</span></span>

## <a name="connect-through-http"></a><span data-ttu-id="682f2-122">Kapcsolódás HTTP Protokollon keresztül</span><span class="sxs-lookup"><span data-stu-id="682f2-122">Connect through HTTP</span></span>
<span data-ttu-id="682f2-123">Csatlakozás a Windows PowerShell StorSimple egy HTTP-kapcsolaton keresztül a StorSimple eszköz soros konzolon keresztül csatlakozó-nál nagyobb védelmet kínál.</span><span class="sxs-lookup"><span data-stu-id="682f2-123">Connecting to Windows PowerShell for StorSimple through an HTTP session offers more security than connecting through the serial console of your StorSimple device.</span></span> <span data-ttu-id="682f2-124">Bár ez nem a legbiztonságosabb módszer, akkor elfogadható megbízható hálózatokon.</span><span class="sxs-lookup"><span data-stu-id="682f2-124">Although this is not the most secure method, it is acceptable on trusted networks.</span></span>

<span data-ttu-id="682f2-125">A klasszikus Azure portálon vagy a soros konzol segítségével távoli felügyeletének konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="682f2-125">You can use either the Azure classic portal or the serial console to configure remote management.</span></span> <span data-ttu-id="682f2-126">Az alábbi eljárások közül választhat:</span><span class="sxs-lookup"><span data-stu-id="682f2-126">Select from the following procedures:</span></span>

* [<span data-ttu-id="682f2-127">Távoli felügyelet engedélyezéséhez a HTTP Protokollon keresztül a klasszikus Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="682f2-127">Use the Azure classic portal to enable remote management over HTTP</span></span>](#use-the-azure-classic-portal-to-enable-remote-management-over-http)
* [<span data-ttu-id="682f2-128">A soros konzol használatával engedélyezze a távoli felügyeleti HTTP Protokollon keresztül</span><span class="sxs-lookup"><span data-stu-id="682f2-128">Use the serial console to enable remote management over HTTP</span></span>](#use-the-serial-console-to-enable-remote-management-over-http)

<span data-ttu-id="682f2-129">Miután engedélyezte távfelügyeletet, a következő eljárással az ügyfélszoftver előkészítése a távoli kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="682f2-129">After you enable remote management, use the following procedure to prepare the client for a remote connection.</span></span>

* [<span data-ttu-id="682f2-130">Az ügyfélszoftver előkészítése a távoli kapcsolat</span><span class="sxs-lookup"><span data-stu-id="682f2-130">Prepare the client for remote connection</span></span>](#prepare-the-client-for-remote-connection)

### <a name="use-the-azure-classic-portal-to-enable-remote-management-over-http"></a><span data-ttu-id="682f2-131">Távoli felügyelet engedélyezéséhez a HTTP Protokollon keresztül a klasszikus Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="682f2-131">Use the Azure classic portal to enable remote management over HTTP</span></span>
<span data-ttu-id="682f2-132">Hajtsa végre az alábbi lépéseket a klasszikus Azure portálon távoli felügyelet engedélyezéséhez a HTTP Protokollon keresztül.</span><span class="sxs-lookup"><span data-stu-id="682f2-132">Perform the following steps in the Azure classic portal to enable remote management over HTTP.</span></span>

#### <a name="to-enable-remote-management-through-the-azure-classic-portal"></a><span data-ttu-id="682f2-133">A klasszikus Azure portálon keresztül távoli felügyeletének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="682f2-133">To enable remote management through the Azure classic portal</span></span>
1. <span data-ttu-id="682f2-134">Hozzáférés **eszközök** > **konfigurálása** az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="682f2-134">Access **Devices** > **Configure** for your device.</span></span>
2. <span data-ttu-id="682f2-135">Görgessen le a **Távoli felügyelet** részig.</span><span class="sxs-lookup"><span data-stu-id="682f2-135">Scroll down to the **Remote Management** section.</span></span>
3. <span data-ttu-id="682f2-136">A **Távoli felügyelet engedélyezése** lehetőségnél válassza az **Igen** beállítást.</span><span class="sxs-lookup"><span data-stu-id="682f2-136">Set **Enable Remote Management** to **Yes**.</span></span>
4. <span data-ttu-id="682f2-137">Mostantól HTTP-n keresztül is csatlakozhat.</span><span class="sxs-lookup"><span data-stu-id="682f2-137">You can now choose to connect using HTTP.</span></span> <span data-ttu-id="682f2-138">(Az alapértelmezett érték a HTTPS-KAPCSOLATON keresztül csatlakozni.) Győződjön meg arról, hogy a HTTP van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="682f2-138">(The default is to connect over HTTPS.) Make sure that HTTP is selected.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="682f2-139">A HTTP-n keresztüli csatlakozást kizárólag megbízható hálózatokon célszerű engedélyezni.</span><span class="sxs-lookup"><span data-stu-id="682f2-139">Connecting over HTTP is acceptable only on trusted networks.</span></span>
   > 
   > 
5. <span data-ttu-id="682f2-140">Kattintson a lap alján található **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="682f2-140">Click **Save** at the bottom of the page.</span></span>

### <a name="use-the-serial-console-to-enable-remote-management-over-http"></a><span data-ttu-id="682f2-141">A soros konzol használatával engedélyezze a távoli felügyeleti HTTP Protokollon keresztül</span><span class="sxs-lookup"><span data-stu-id="682f2-141">Use the serial console to enable remote management over HTTP</span></span>
<span data-ttu-id="682f2-142">Hajtsa végre a következő lépéseket távoli felügyeletének engedélyezése az eszköz soros konzoljához.</span><span class="sxs-lookup"><span data-stu-id="682f2-142">Perform the following steps on the device serial console to enable remote management.</span></span>

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a><span data-ttu-id="682f2-143">Az eszköz soros konzolon keresztül távoli felügyeletének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="682f2-143">To enable remote management through the device serial console</span></span>
1. <span data-ttu-id="682f2-144">A soros konzol menüben válassza az 1. lehetőség.</span><span class="sxs-lookup"><span data-stu-id="682f2-144">On the serial console menu, select option 1.</span></span> <span data-ttu-id="682f2-145">Az eszközön a soros konzol használatával kapcsolatos további információkért látogasson el [csatlakozás Windows PowerShell-lel eszköz soros konzolon keresztül](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="682f2-145">For more information about using the serial console on the device, go to [Connect to Windows PowerShell for StorSimple via device serial console](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span></span>
2. <span data-ttu-id="682f2-146">A parancssorba írja be:`Enable-HcsRemoteManagement –AllowHttp`</span><span class="sxs-lookup"><span data-stu-id="682f2-146">At the prompt, type: `Enable-HcsRemoteManagement –AllowHttp`</span></span>
3. <span data-ttu-id="682f2-147">Értesítést fog kapni a HTTP protokollal csatlakozni az eszköz biztonsági rések kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="682f2-147">You will be notified about the security vulnerabilities of using HTTP to connect to the device.</span></span> <span data-ttu-id="682f2-148">Amikor a rendszer kéri, erősítse meg, írja be a **Y**.</span><span class="sxs-lookup"><span data-stu-id="682f2-148">When prompted, confirm by typing **Y**.</span></span>
4. <span data-ttu-id="682f2-149">Győződjön meg arról, hogy a HTTP engedélyezett beírásával:`Get-HcsSystem`</span><span class="sxs-lookup"><span data-stu-id="682f2-149">Verify that HTTP is enabled by typing: `Get-HcsSystem`</span></span>
5. <span data-ttu-id="682f2-150">Ellenőrizze, hogy a **RemoteManagementMode** mezőben látható **HttpsAndHttpEnabled**. A következő ábrán ezek a beállítások a PuTTY.</span><span class="sxs-lookup"><span data-stu-id="682f2-150">Verify that the **RemoteManagementMode** field shows **HttpsAndHttpEnabled**.The following illustration shows these settings in PuTTY.</span></span>
   
     ![Soros HTTPS- és HTTP engedélyezve](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-the-client-for-remote-connection"></a><span data-ttu-id="682f2-152">Az ügyfélszoftver előkészítése a távoli kapcsolat</span><span class="sxs-lookup"><span data-stu-id="682f2-152">Prepare the client for remote connection</span></span>
<span data-ttu-id="682f2-153">A következő lépésekkel távoli felügyeletének engedélyezése az ügyfélen.</span><span class="sxs-lookup"><span data-stu-id="682f2-153">Perform the following steps on the client to enable remote management.</span></span>

#### <a name="to-prepare-the-client-for-remote-connection"></a><span data-ttu-id="682f2-154">Az ügyfélszoftver előkészítése a távoli kapcsolat</span><span class="sxs-lookup"><span data-stu-id="682f2-154">To prepare the client for remote connection</span></span>
1. <span data-ttu-id="682f2-155">Indítsa el a Windows PowerShell-munkamenetet rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="682f2-155">Start a Windows PowerShell session as an administrator.</span></span>
2. <span data-ttu-id="682f2-156">Írja be a következő parancs futtatásával adja hozzá a StorSimple eszköz IP-címét az ügyfél megbízható állomások listájához:</span><span class="sxs-lookup"><span data-stu-id="682f2-156">Type the following command to add the IP address of the StorSimple device to the client’s trusted hosts list:</span></span> 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
     <span data-ttu-id="682f2-157">Cserélje le <*device_ip*> az IP-cím, az eszköz; például:</span><span class="sxs-lookup"><span data-stu-id="682f2-157">Replace <*device_ip*> with the IP address of your device; for example:</span></span> 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. <span data-ttu-id="682f2-158">Írja be a következő parancsot a hitelesítő adatai mentése változóként:</span><span class="sxs-lookup"><span data-stu-id="682f2-158">Type the following command to save the device credentials in a variable:</span></span> 
   
    ```
    $cred = Get-Credential
    ```
    
4. <span data-ttu-id="682f2-159">A megjelenő párbeszédpanelen:</span><span class="sxs-lookup"><span data-stu-id="682f2-159">In the dialog box that appears:</span></span>
   
   1. <span data-ttu-id="682f2-160">Írja be a felhasználónevet a következő formátumban: *device_ip\SSAdmin*.</span><span class="sxs-lookup"><span data-stu-id="682f2-160">Type the user name in this format: *device_ip\SSAdmin*.</span></span>
   2. <span data-ttu-id="682f2-161">Írja be az eszköz rendszergazdai jelszava, amely lett beállítva, amikor az eszköz a telepítővarázsló lett konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="682f2-161">Type the device administrator password that was set when the device was configured with the setup wizard.</span></span> <span data-ttu-id="682f2-162">Az alapértelmezett jelszó *jelszó1*.</span><span class="sxs-lookup"><span data-stu-id="682f2-162">The default password is *Password1*.</span></span>
5. <span data-ttu-id="682f2-163">Ez a parancs beírásával indítsa el a Windows PowerShell-munkamenetet az eszközön:</span><span class="sxs-lookup"><span data-stu-id="682f2-163">Start a Windows PowerShell session on the device by typing this command:</span></span>
   
     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`
   
   > [!NOTE]
   > <span data-ttu-id="682f2-164">A StorSimple virtuális eszköz létrehozásához használja a Windows PowerShell-munkamenetet, hozzáfűzése a `–Port` paraméter, és adja meg a nyilvános port a StorSimple virtuális készülék távoli eljáráshívási konfigurált.</span><span class="sxs-lookup"><span data-stu-id="682f2-164">To create a Windows PowerShell session for use with the StorSimple virtual device, append the `–Port` parameter and specify the public port that you configured in Remoting for StorSimple Virtual Appliance.</span></span>
   > 
   > 
   
     <span data-ttu-id="682f2-165">Ezen a ponton rendelkeznie kell egy aktív távoli Windows PowerShell-munkamenetben az eszközre.</span><span class="sxs-lookup"><span data-stu-id="682f2-165">At this point, you should have an active remote Windows PowerShell session to the device.</span></span>
   
    ![PowerShell távvezérlése HTTP-n keresztül](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a><span data-ttu-id="682f2-167">HTTPS keresztül csatlakozni</span><span class="sxs-lookup"><span data-stu-id="682f2-167">Connect through HTTPS</span></span>
<span data-ttu-id="682f2-168">Csatlakozás a Windows PowerShell StorSimple egy HTTPS-kapcsolaton keresztül a legbiztonságosabb és ajánlott mód távolról kapcsolódni a Microsoft Azure StorSimple eszközt.</span><span class="sxs-lookup"><span data-stu-id="682f2-168">Connecting to Windows PowerShell for StorSimple through an HTTPS session is the most secure and recommended method of remotely connecting to your Microsoft Azure StorSimple device.</span></span> <span data-ttu-id="682f2-169">Az alábbi eljárások azt ismertetik, hogy a Windows PowerShell csatlakoztatni a StorSimple is a HTTPS PROTOKOLLT használja a soros konzol és az ügyfél számítógépek beállítása.</span><span class="sxs-lookup"><span data-stu-id="682f2-169">The following procedures explain how to set up the serial console and client computers so that you can use HTTPS to connect to Windows PowerShell for StorSimple.</span></span>

<span data-ttu-id="682f2-170">A klasszikus Azure portálon vagy a soros konzol segítségével távoli felügyeletének konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="682f2-170">You can use either the Azure classic portal or the serial console to configure remote management.</span></span> <span data-ttu-id="682f2-171">Az alábbi eljárások közül választhat:</span><span class="sxs-lookup"><span data-stu-id="682f2-171">Select from the following procedures:</span></span>

* [<span data-ttu-id="682f2-172">A klasszikus Azure portál segítségével távoli felügyeletének engedélyezése a HTTPS-KAPCSOLATON keresztül</span><span class="sxs-lookup"><span data-stu-id="682f2-172">Use the Azure classic portal to enable remote management over HTTPS</span></span>](#use-the-azure-classic-portal-to-enable-remote-management-over-https)
* [<span data-ttu-id="682f2-173">A soros konzol használatával engedélyezze a távoli felügyeleti HTTPS-KAPCSOLATON keresztül</span><span class="sxs-lookup"><span data-stu-id="682f2-173">Use the serial console to enable remote management over HTTPS</span></span>](#use-the-serial-console-to-enable-remote-management-over-https)

<span data-ttu-id="682f2-174">Miután engedélyezte távfelügyeletet, az alábbi eljárásokkal egy távoli kezeléshez való előkészítése a gazdagép és az eszköz csatlakozzon a távoli állomástól.</span><span class="sxs-lookup"><span data-stu-id="682f2-174">After you enable remote management, use the following procedures to prepare the host for a remote management and connect to the device from the remote host.</span></span>

* [<span data-ttu-id="682f2-175">A gazdagép Felkészülés a távfelügyeletre</span><span class="sxs-lookup"><span data-stu-id="682f2-175">Prepare the host for remote management</span></span>](#prepare-the-host-for-remote-management)
* [<span data-ttu-id="682f2-176">Az eszköz csatlakozzon a távoli gazdagépen</span><span class="sxs-lookup"><span data-stu-id="682f2-176">Connect to the device from the remote host</span></span>](#connect-to-the-device-from-the-remote-host)

### <a name="use-the-azure-classic-portal-to-enable-remote-management-over-https"></a><span data-ttu-id="682f2-177">A klasszikus Azure portál segítségével távoli felügyeletének engedélyezése a HTTPS-KAPCSOLATON keresztül</span><span class="sxs-lookup"><span data-stu-id="682f2-177">Use the Azure classic portal to enable remote management over HTTPS</span></span>
<span data-ttu-id="682f2-178">Hajtsa végre az alábbi lépéseket a klasszikus Azure portálon távoli felügyeletének engedélyezése a HTTPS-KAPCSOLATON keresztül.</span><span class="sxs-lookup"><span data-stu-id="682f2-178">Perform the following steps in the Azure classic portal to enable remote management over HTTPS.</span></span>

#### <a name="to-enable-remote-management-over-https-from-the-azure-classic-portal"></a><span data-ttu-id="682f2-179">Távoli felügyelet engedélyezéséhez a HTTPS-KAPCSOLATON keresztül a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="682f2-179">To enable remote management over HTTPS from the Azure classic portal</span></span>
1. <span data-ttu-id="682f2-180">Hozzáférés **eszközök** > **konfigurálása** az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="682f2-180">Access **Devices** > **Configure** for your device.</span></span>
2. <span data-ttu-id="682f2-181">Görgessen le a **Távoli felügyelet** részig.</span><span class="sxs-lookup"><span data-stu-id="682f2-181">Scroll down to the **Remote Management** section.</span></span>
3. <span data-ttu-id="682f2-182">A **Távoli felügyelet engedélyezése** lehetőségnél válassza az **Igen** beállítást.</span><span class="sxs-lookup"><span data-stu-id="682f2-182">Set **Enable Remote Management** to **Yes**.</span></span>
4. <span data-ttu-id="682f2-183">Ha most szeretné HTTPS protokoll használatával kapcsolódó levelezőprogramokkal.</span><span class="sxs-lookup"><span data-stu-id="682f2-183">You can now choose to connect using HTTPS.</span></span> <span data-ttu-id="682f2-184">(Az alapértelmezett érték a HTTPS-KAPCSOLATON keresztül csatlakozni.) Győződjön meg arról, hogy HTTPS van-e kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="682f2-184">(The default is to connect over HTTPS.) Make sure that HTTPS is selected.</span></span> 
5. <span data-ttu-id="682f2-185">Kattintson a **távfelügyeleti tanúsítvány letöltése**.</span><span class="sxs-lookup"><span data-stu-id="682f2-185">Click **Download Remote Management Certificate**.</span></span> <span data-ttu-id="682f2-186">Adjon meg egy helyet a fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="682f2-186">Specify a location to save this file.</span></span> <span data-ttu-id="682f2-187">Szüksége lesz a tanúsítvány telepítése a számítógépen az ügyfélszámítógépre vagy állomásra, amely az eszköz való kapcsolódáshoz használandó.</span><span class="sxs-lookup"><span data-stu-id="682f2-187">You will need to install this certificate on the client or host computer that you will use to connect to the device.</span></span>
6. <span data-ttu-id="682f2-188">Kattintson a lap alján található **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="682f2-188">Click **Save** at the bottom of the page.</span></span>

### <a name="use-the-serial-console-to-enable-remote-management-over-https"></a><span data-ttu-id="682f2-189">A soros konzol használatával engedélyezze a távoli felügyeleti HTTPS-KAPCSOLATON keresztül</span><span class="sxs-lookup"><span data-stu-id="682f2-189">Use the serial console to enable remote management over HTTPS</span></span>
<span data-ttu-id="682f2-190">Hajtsa végre a következő lépéseket távoli felügyeletének engedélyezése az eszköz soros konzoljához.</span><span class="sxs-lookup"><span data-stu-id="682f2-190">Perform the following steps on the device serial console to enable remote management.</span></span>

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a><span data-ttu-id="682f2-191">Az eszköz soros konzolon keresztül távoli felügyeletének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="682f2-191">To enable remote management through the device serial console</span></span>
1. <span data-ttu-id="682f2-192">A soros konzol menüben válassza az 1. lehetőség.</span><span class="sxs-lookup"><span data-stu-id="682f2-192">On the serial console menu, select option 1.</span></span> <span data-ttu-id="682f2-193">Az eszközön a soros konzol használatával kapcsolatos további információkért látogasson el [csatlakozás Windows PowerShell-lel eszköz soros konzolon keresztül](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="682f2-193">For more information about using the serial console on the device, go to [Connect to Windows PowerShell for StorSimple via device serial console](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span></span>
2. <span data-ttu-id="682f2-194">A parancssorba írja be:</span><span class="sxs-lookup"><span data-stu-id="682f2-194">At the prompt, type:</span></span> 
   
     `Enable-HcsRemoteManagement`
   
    <span data-ttu-id="682f2-195">Ez engedélyezze a HTTPS-eszközön.</span><span class="sxs-lookup"><span data-stu-id="682f2-195">This should enable HTTPS on your device.</span></span>
3. <span data-ttu-id="682f2-196">Ellenőrizze, hogy engedélyezték-e a HTTPS beírásával:</span><span class="sxs-lookup"><span data-stu-id="682f2-196">Verify that HTTPS has been enabled by typing:</span></span> 
   
     `Get-HcsSystem`
   
    <span data-ttu-id="682f2-197">Győződjön meg arról, hogy a **RemoteManagementMode** mezőben látható **HttpsEnabled**. A következő ábrán ezek a beállítások a PuTTY.</span><span class="sxs-lookup"><span data-stu-id="682f2-197">Make sure that the **RemoteManagementMode** field shows **HttpsEnabled**.The following illustration shows these settings in PuTTY.</span></span>
   
     ![Soros HTTPS-kompatibilis](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)
4. <span data-ttu-id="682f2-199">Kimenetében `Get-HcsSystem`, másolja a sorozatszámot az eszköz, és mentse azokat a későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="682f2-199">From the output of `Get-HcsSystem`, copy the serial number of the device and save it for later use.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="682f2-200">A sorozatszám van leképezve a tanúsítvány CN-név.</span><span class="sxs-lookup"><span data-stu-id="682f2-200">The serial number maps to the CN name in the certificate.</span></span>
   > 
   > 
5. <span data-ttu-id="682f2-201">Írja be a távoli felügyeleti tanúsítvány beszerzése:</span><span class="sxs-lookup"><span data-stu-id="682f2-201">Obtain a remote management certificate by typing:</span></span> 
   
     `Get-HcsRemoteManagementCert`
   
    <span data-ttu-id="682f2-202">Egy tanúsítványt a következőhöz hasonló jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="682f2-202">A certificate similar to the following will appear.</span></span>
   
    ![Távfelügyeleti tanúsítvány beszerzése](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)
6. <span data-ttu-id="682f2-204">Másolja az adatokat a tanúsítványban lévő **---BEGIN CERTIFICATE---** való **---vége tanúsítvány---** egy szövegszerkesztőbe például a Jegyzettömbben, és mentse azt egy .cer fájlba.</span><span class="sxs-lookup"><span data-stu-id="682f2-204">Copy the information in the certificate from **-----BEGIN CERTIFICATE-----** to **-----END CERTIFICATE-----** into a text editor such as Notepad, and save it as a .cer file.</span></span> <span data-ttu-id="682f2-205">(Ön másolja ezt a fájlt a távoli állomás a gazdagép előkészítésekor.)</span><span class="sxs-lookup"><span data-stu-id="682f2-205">(You will copy this file to your remote host when you prepare the host.)</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="682f2-206">Új tanúsítvány létrehozásához használja a `Set-HcsRemoteManagementCert` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="682f2-206">To generate a new certificate, use the `Set-HcsRemoteManagementCert` cmdlet.</span></span>
   > 
   > 

### <a name="prepare-the-host-for-remote-management"></a><span data-ttu-id="682f2-207">A gazdagép Felkészülés a távfelügyeletre</span><span class="sxs-lookup"><span data-stu-id="682f2-207">Prepare the host for remote management</span></span>
<span data-ttu-id="682f2-208">Egy távoli kapcsolathoz, amely HTTPS-KAPCSOLATON keresztül használja a számítógép előkészítéséhez hajtsa végre az alábbi eljárásokat:</span><span class="sxs-lookup"><span data-stu-id="682f2-208">To prepare the host computer for a remote connection that uses an HTTPS session, perform the following procedures:</span></span>

* <span data-ttu-id="682f2-209">[A .cer fájlt importálja a gyökérszintű tárolóban. az ügyfél vagy a távoli állomás](#to-import-the-certificate-on-the-remote-host).</span><span class="sxs-lookup"><span data-stu-id="682f2-209">[Import the .cer file into the root store of the client or remote host](#to-import-the-certificate-on-the-remote-host).</span></span>
* <span data-ttu-id="682f2-210">[Az eszközök sorozatszámait hozzáadása az állomásleíró fájlhoz, a távoli állomáson levő](#to-add-device-serial-numbers-to-the-remote-host).</span><span class="sxs-lookup"><span data-stu-id="682f2-210">[Add the device serial numbers to the hosts file on your remote host](#to-add-device-serial-numbers-to-the-remote-host).</span></span>

<span data-ttu-id="682f2-211">Ezek az eljárások leírását alább.</span><span class="sxs-lookup"><span data-stu-id="682f2-211">Each of these procedures is described below.</span></span>

#### <a name="to-import-the-certificate-on-the-remote-host"></a><span data-ttu-id="682f2-212">A távoli állomáson levő tanúsítvány importálása</span><span class="sxs-lookup"><span data-stu-id="682f2-212">To import the certificate on the remote host</span></span>
1. <span data-ttu-id="682f2-213">Kattintson a jobb gombbal a .cer fájlt, és válassza ki **telepítés tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="682f2-213">Right-click the .cer file and select **Install certificate**.</span></span> <span data-ttu-id="682f2-214">A Tanúsítványimportáló varázsló elindítja.</span><span class="sxs-lookup"><span data-stu-id="682f2-214">This will start the Certificate Import Wizard.</span></span>
   
    ![Tanúsítványimportáló varázsló 1](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)
2. <span data-ttu-id="682f2-216">A **tárolóhelyére**, jelölje be **helyi számítógép**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="682f2-216">For **Store location**, select **Local Machine**, and then click **Next**.</span></span>
3. <span data-ttu-id="682f2-217">Válassza ki **minden tanúsítvány tárolása ebben a tárolóban**, és kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="682f2-217">Select **Place all certificates in the following store**, and then click **Browse**.</span></span> <span data-ttu-id="682f2-218">Keresse meg a gyökérszintű tárolóban. a távoli állomás, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="682f2-218">Navigate to the root store of your remote host, and then click **Next**.</span></span>
   
    ![Tanúsítványimportáló varázsló 2. régiója](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)
4. <span data-ttu-id="682f2-220">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="682f2-220">Click **Finish**.</span></span> <span data-ttu-id="682f2-221">Megjelenik egy üzenet, amely közli, hogy az importálás sikeres volt-e.</span><span class="sxs-lookup"><span data-stu-id="682f2-221">A message that tells you that the import was successful appears.</span></span>
   
    ![Tanúsítványimportáló varázsló 3](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="to-add-device-serial-numbers-to-the-remote-host"></a><span data-ttu-id="682f2-223">Az eszközök sorozatszámait hozzáadása a távoli állomás</span><span class="sxs-lookup"><span data-stu-id="682f2-223">To add device serial numbers to the remote host</span></span>
1. <span data-ttu-id="682f2-224">Rendszergazdaként a Jegyzettömb, és nyissa meg a \Windows\System32\Drivers\etc helyen található gazdafájlt.</span><span class="sxs-lookup"><span data-stu-id="682f2-224">Start Notepad as an administrator, and then open the hosts file located at \Windows\System32\Drivers\etc.</span></span>
2. <span data-ttu-id="682f2-225">Adja hozzá a következő három bejegyzéseket a hosts fájl: **DATA 0 IP-cím**, **0 vezérlő rögzített IP-cím**, és **vezérlő 1 fix IP-cím**.</span><span class="sxs-lookup"><span data-stu-id="682f2-225">Add the following three entries to your hosts file: **DATA 0 IP address**, **Controller 0 Fixed IP address**, and **Controller 1 Fixed IP address**.</span></span>
3. <span data-ttu-id="682f2-226">Adja meg a korábban mentett eszköz sorozatszámát.</span><span class="sxs-lookup"><span data-stu-id="682f2-226">Enter the device serial number that you saved earlier.</span></span> <span data-ttu-id="682f2-227">Képezze le az ezt az IP-cím a következő ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="682f2-227">Map this to the IP address as shown in the following image.</span></span> <span data-ttu-id="682f2-228">A vezérlő 0 és a vezérlő 1 hozzáfűzése **Controller0** és **Controller1** a sorozatszám (CN-név) végén.</span><span class="sxs-lookup"><span data-stu-id="682f2-228">For Controller 0 and Controller 1, append **Controller0** and **Controller1** at the end of the serial number (CN name).</span></span>
   
    ![CN-név felvétele a hosts fájl](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)
4. <span data-ttu-id="682f2-230">Mentse a hosts fájlt.</span><span class="sxs-lookup"><span data-stu-id="682f2-230">Save the hosts file.</span></span>

### <a name="connect-to-the-device-from-the-remote-host"></a><span data-ttu-id="682f2-231">Az eszköz csatlakozzon a távoli gazdagépen</span><span class="sxs-lookup"><span data-stu-id="682f2-231">Connect to the device from the remote host</span></span>
<span data-ttu-id="682f2-232">A Windows PowerShell és az SSL segítségével adjon meg egy SSAdmin munkamenet az eszköz az ügyfél vagy egy távoli állomástól.</span><span class="sxs-lookup"><span data-stu-id="682f2-232">Use Windows PowerShell and SSL to enter an SSAdmin session on your device from a remote host or client.</span></span> <span data-ttu-id="682f2-233">Az 1. lehetőség van leképezve a SSAdmin munkamenet a [soros konzol](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) menü az eszköz.</span><span class="sxs-lookup"><span data-stu-id="682f2-233">The SSAdmin session maps to option 1 in the [serial console](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) menu of your device.</span></span>

<span data-ttu-id="682f2-234">Az alábbi eljárás végrehajtásához a számítógépen, amelyből el kívánja a távoli Windows PowerShell-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="682f2-234">Perform the following procedure on the computer from which you want to make the remote Windows PowerShell connection.</span></span>

#### <a name="to-enter-an-ssadmin-session-on-the-device-by-using-windows-powershell-and-ssl"></a><span data-ttu-id="682f2-235">Adjon meg egy SSAdmin munkamenet az eszközön a Windows PowerShell és az SSL használatával történő</span><span class="sxs-lookup"><span data-stu-id="682f2-235">To enter an SSAdmin session on the device by using Windows PowerShell and SSL</span></span>
1. <span data-ttu-id="682f2-236">Indítsa el a Windows PowerShell-munkamenetet rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="682f2-236">Start a Windows PowerShell session as an administrator.</span></span>
2. <span data-ttu-id="682f2-237">Az ügyfél megbízható állomások írja be az eszköz IP-cím hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="682f2-237">Add the device IP address to the client’s trusted hosts by typing:</span></span>
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
    <span data-ttu-id="682f2-238">Ahol <*device_ip*> az eszköz IP-címét, például:</span><span class="sxs-lookup"><span data-stu-id="682f2-238">Where <*device_ip*> is the IP address of your device; for example:</span></span> 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. <span data-ttu-id="682f2-239">Hozzon létre új hitelesítő adatokat írja be:</span><span class="sxs-lookup"><span data-stu-id="682f2-239">Create a new credential by typing:</span></span> 
   
     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`
   
    <span data-ttu-id="682f2-240">Ahol <*IP-címe céleszközön*> 0 adatok az eszköz; IP-címe például **10.126.173.90** a hosts fájl az előző ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="682f2-240">Where <*IP of target device*> is the IP address of DATA 0 for your device; for example, **10.126.173.90** as shown in the preceding image of the hosts file.</span></span> <span data-ttu-id="682f2-241">Továbbá adja meg az eszköz rendszergazdai jelszava.</span><span class="sxs-lookup"><span data-stu-id="682f2-241">Also, supply the administrator password for your device.</span></span>
4. <span data-ttu-id="682f2-242">Írja be a munkamenet létrehozása:</span><span class="sxs-lookup"><span data-stu-id="682f2-242">Create a session by typing:</span></span>
   
     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`
   
    <span data-ttu-id="682f2-243">A parancsmag - ComputerName paraméter adja meg a <*figyelt eszköz sorozatszámát*>.</span><span class="sxs-lookup"><span data-stu-id="682f2-243">For the -ComputerName parameter in the cmdlet, provide the <*serial number of target device*>.</span></span> <span data-ttu-id="682f2-244">Ezzel a sorozatszámmal leképezve a következőre: DATA 0 a hosts fájl, a távoli állomáson; IP-címe például **SHX0991003G44MT** a következő ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="682f2-244">This serial number was mapped to the IP address of DATA 0 in the hosts file on your remote host; for example, **SHX0991003G44MT** as shown in the following image.</span></span>
5. <span data-ttu-id="682f2-245">Típus:</span><span class="sxs-lookup"><span data-stu-id="682f2-245">Type:</span></span> 
   
     `Enter-PSSession $session`
6. <span data-ttu-id="682f2-246">Várjon néhány percet kell, és majd meg fog csatlakozni az eszköz keresztül HTTPS SSL-en keresztül.</span><span class="sxs-lookup"><span data-stu-id="682f2-246">You will need to wait a few minutes, and then you will be connected to your device via HTTPS over SSL.</span></span> <span data-ttu-id="682f2-247">Egy üzenet, amely jelzi, hogy az eszköz csatlakozik jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="682f2-247">You will see a message that indicates you are connected to your device.</span></span>
   
    ![PowerShell távvezérlése HTTPS és az SSL használata](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a><span data-ttu-id="682f2-249">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="682f2-249">Next steps</span></span>
* <span data-ttu-id="682f2-250">További információ [a StorSimple eszköz felügyelete a Windows PowerShell használatával](storsimple-windows-powershell-administration.md).</span><span class="sxs-lookup"><span data-stu-id="682f2-250">Learn more about [using Windows PowerShell to administer your StorSimple device](storsimple-windows-powershell-administration.md).</span></span>
* <span data-ttu-id="682f2-251">További információ [a StorSimple Manager szolgáltatás használata a StorSimple eszköz felügyeletéhez](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="682f2-251">Learn more about [using the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>

