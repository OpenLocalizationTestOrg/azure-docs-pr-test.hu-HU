---
title: "Távoli csatlakozás a StorSimple eszköz |} Microsoft Docs"
description: "Távoli kezelésére szolgál az eszköz konfigurálása és a StorSimple HTTP vagy HTTPS PROTOKOLLON keresztül és a Windows PowerShell összekapcsolása mutatja be."
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
ms.openlocfilehash: ff76884f020a0fb8a1b48bd371c419bd65e85fd3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connect-remotely-to-your-storsimple-8000-series-device"></a><span data-ttu-id="7e683-103">Távoli csatlakozás a StorSimple 8000 series eszköz</span><span class="sxs-lookup"><span data-stu-id="7e683-103">Connect remotely to your StorSimple 8000 series device</span></span>

## <a name="overview"></a><span data-ttu-id="7e683-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="7e683-104">Overview</span></span>

<span data-ttu-id="7e683-105">Az eszköz Windows PowerShell használatával távolról csatlakozhat.</span><span class="sxs-lookup"><span data-stu-id="7e683-105">You can remotely connect to your device via Windows PowerShell.</span></span> <span data-ttu-id="7e683-106">Ezzel a módszerrel csatlakoztatásakor nem látja a menüben.</span><span class="sxs-lookup"><span data-stu-id="7e683-106">When you connect this way, you do not see a menu.</span></span> <span data-ttu-id="7e683-107">(Megjelenik a menü csak akkor, ha a soros konzol az eszközön való csatlakozáshoz használ.) A Windows PowerShell-távelérést csatlakozhat egy adott futási teret.</span><span class="sxs-lookup"><span data-stu-id="7e683-107">(You see a menu only if you use the serial console on the device to connect.) With Windows PowerShell remoting, you connect to a specific runspace.</span></span> <span data-ttu-id="7e683-108">A megjelenítési nyelv is megadható.</span><span class="sxs-lookup"><span data-stu-id="7e683-108">You can also specify the display language.</span></span>

<span data-ttu-id="7e683-109">Az eszköz kezelését Windows PowerShell távoli eljáráshívás használatával kapcsolatos további információkért látogasson el [használata a Windows PowerShell-lel felügyelete a StorSimple eszköz](storsimple-8000-windows-powershell-administration.md).</span><span class="sxs-lookup"><span data-stu-id="7e683-109">For more information about using Windows PowerShell remoting to manage your device, go to [Use Windows PowerShell for StorSimple to administer your StorSimple device](storsimple-8000-windows-powershell-administration.md).</span></span>

<span data-ttu-id="7e683-110">Ez az oktatóanyag azt ismerteti, az eszköz a távoli felügyelet beállítása, majd a StorSimple és a Windows PowerShell összekapcsolása.</span><span class="sxs-lookup"><span data-stu-id="7e683-110">This tutorial explains how to configure your device for remote management and then how to connect to Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="7e683-111">HTTP vagy HTTPS használatával távoli csatlakozás a Windows PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="7e683-111">You can use HTTP or HTTPS to remotely connect via Windows PowerShell.</span></span> <span data-ttu-id="7e683-112">Azonban a StorSimple és a Windows PowerShell összekapcsolása dönt, vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="7e683-112">However, when you are deciding how to connect to Windows PowerShell for StorSimple, consider the following information:</span></span>

* <span data-ttu-id="7e683-113">Az eszköz soros konzoljához való közvetlen csatlakozás biztonságos, de nincs csatlakozás soros konzolon keresztül hálózati kapcsolók.</span><span class="sxs-lookup"><span data-stu-id="7e683-113">Connecting directly to the device serial console is secure, but connecting to the serial console over network switches is not.</span></span> <span data-ttu-id="7e683-114">Legyen óvatos, a biztonsági kockázat, hálózati kapcsolók keresztül az eszköz soros konzoljához való kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="7e683-114">Be cautious of the security risk when connecting to the device serial console over network switches.</span></span>
* <span data-ttu-id="7e683-115">Egy HTTP-kapcsolaton keresztül csatlakozó felajánlhatja a nagyobb biztonság, mint a hálózaton keresztül a soros konzolon keresztül kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="7e683-115">Connecting through an HTTP session might offer more security than connecting through the serial console over the network.</span></span> <span data-ttu-id="7e683-116">Bár ez nem a legbiztonságosabb módszer, akkor elfogadható megbízható hálózatokon.</span><span class="sxs-lookup"><span data-stu-id="7e683-116">Although this is not the most secure method, it is acceptable on trusted networks.</span></span>
* <span data-ttu-id="7e683-117">Keresztül egy önaláírt tanúsítványt a HTTPS-KAPCSOLATON keresztül csatlakozó értéke a legbiztonságosabb és ajánlott.</span><span class="sxs-lookup"><span data-stu-id="7e683-117">Connecting through an HTTPS session with a self-signed certificate is the most secure and the recommended option.</span></span>

<span data-ttu-id="7e683-118">Kapcsolódás távolról a Windows PowerShell-felületet.</span><span class="sxs-lookup"><span data-stu-id="7e683-118">You can connect remotely to the Windows PowerShell interface.</span></span> <span data-ttu-id="7e683-119">Azonban a Windows PowerShell felületén keresztül a StorSimple eszközhöz való távoli hozzáféréssel nem alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="7e683-119">However, remote access to your StorSimple device via the Windows PowerShell interface is not enabled by default.</span></span> <span data-ttu-id="7e683-120">Először engedélyezze a távoli felügyeleti az eszközön, és ezután az ügyfélen, amelynek használatával az eszközhöz való hozzáféréshez.</span><span class="sxs-lookup"><span data-stu-id="7e683-120">You must enable remote management on the device first, and then on the client that is used to access your device.</span></span>

<span data-ttu-id="7e683-121">Ebben a cikkben leírt lépéseket a Windows Server 2012 R2 rendszerű gazdarendszer végrehajtott.</span><span class="sxs-lookup"><span data-stu-id="7e683-121">The steps described in this article were performed on a host system running Windows Server 2012 R2.</span></span>

## <a name="connect-through-http"></a><span data-ttu-id="7e683-122">Kapcsolódás HTTP Protokollon keresztül</span><span class="sxs-lookup"><span data-stu-id="7e683-122">Connect through HTTP</span></span>

<span data-ttu-id="7e683-123">Csatlakozás a Windows PowerShell StorSimple egy HTTP-kapcsolaton keresztül a StorSimple eszköz soros konzolon keresztül csatlakozó-nál nagyobb védelmet kínál.</span><span class="sxs-lookup"><span data-stu-id="7e683-123">Connecting to Windows PowerShell for StorSimple through an HTTP session offers more security than connecting through the serial console of your StorSimple device.</span></span> <span data-ttu-id="7e683-124">Bár ez nem a legbiztonságosabb módszer, akkor elfogadható megbízható hálózatokon.</span><span class="sxs-lookup"><span data-stu-id="7e683-124">Although this is not the most secure method, it is acceptable on trusted networks.</span></span>

<span data-ttu-id="7e683-125">Az Azure-portálon vagy a soros konzol segítségével távoli felügyeletének konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="7e683-125">You can use either the Azure portal or the serial console to configure remote management.</span></span> <span data-ttu-id="7e683-126">Az alábbi eljárások közül választhat:</span><span class="sxs-lookup"><span data-stu-id="7e683-126">Select from the following procedures:</span></span>

* [<span data-ttu-id="7e683-127">Az Azure portál segítségével távoli felügyelet engedélyezéséhez a HTTP-kapcsolaton keresztül</span><span class="sxs-lookup"><span data-stu-id="7e683-127">Use the Azure portal to enable remote management over HTTP</span></span>](#use-the-azure-classic-portal-to-enable-remote-management-over-http)
* [<span data-ttu-id="7e683-128">A soros konzol használatával engedélyezze a távoli felügyeleti HTTP Protokollon keresztül</span><span class="sxs-lookup"><span data-stu-id="7e683-128">Use the serial console to enable remote management over HTTP</span></span>](#use-the-serial-console-to-enable-remote-management-over-http)

<span data-ttu-id="7e683-129">Miután engedélyezte távfelügyeletet, a következő eljárással az ügyfélszoftver előkészítése a távoli kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="7e683-129">After you enable remote management, use the following procedure to prepare the client for a remote connection.</span></span>

* [<span data-ttu-id="7e683-130">Az ügyfélszoftver előkészítése a távoli kapcsolat</span><span class="sxs-lookup"><span data-stu-id="7e683-130">Prepare the client for remote connection</span></span>](#prepare-the-client-for-remote-connection)

### <a name="use-the-azure-portal-to-enable-remote-management-over-http"></a><span data-ttu-id="7e683-131">Az Azure portál segítségével távoli felügyelet engedélyezéséhez a HTTP-kapcsolaton keresztül</span><span class="sxs-lookup"><span data-stu-id="7e683-131">Use the Azure portal to enable remote management over HTTP</span></span>

<span data-ttu-id="7e683-132">Hajtsa végre az alábbi lépéseket az Azure-portálon távoli felügyelet engedélyezéséhez a HTTP Protokollon keresztül.</span><span class="sxs-lookup"><span data-stu-id="7e683-132">Perform the following steps in the Azure portal to enable remote management over HTTP.</span></span>

#### <a name="to-enable-remote-management-through-the-azure-portal"></a><span data-ttu-id="7e683-133">Az Azure portálon keresztül távoli felügyeletének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="7e683-133">To enable remote management through the Azure portal</span></span>

1. <span data-ttu-id="7e683-134">Nyissa meg a StorSimple-eszközkezelő szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="7e683-134">Go to your StorSimple Device Manager service.</span></span> <span data-ttu-id="7e683-135">Válassza ki **eszközök** majd válassza ki, és kattintson a konfigurálni kívánt távoli kezelési eszköz.</span><span class="sxs-lookup"><span data-stu-id="7e683-135">Select **Devices** and then select and click the device you want to configure for remote management.</span></span> <span data-ttu-id="7e683-136">Ugrás a **eszközbeállítások > biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="7e683-136">Go to **Device settings > Security**.</span></span>
2. <span data-ttu-id="7e683-137">Az a **biztonsági beállítások** panelen kattintson a **Távfelügyelet**.</span><span class="sxs-lookup"><span data-stu-id="7e683-137">In the **Security settings** blade, click **Remote Management**.</span></span>
3. <span data-ttu-id="7e683-138">Az a **Távfelügyelet** panelen állítsa **távoli felügyelet engedélyezése** való **Igen**.</span><span class="sxs-lookup"><span data-stu-id="7e683-138">In the **Remote management** blade, set **Enable Remote Management** to **Yes**.</span></span>
4. <span data-ttu-id="7e683-139">Mostantól HTTP-n keresztül is csatlakozhat.</span><span class="sxs-lookup"><span data-stu-id="7e683-139">You can now choose to connect using HTTP.</span></span> <span data-ttu-id="7e683-140">(Az alapértelmezett érték a HTTPS-KAPCSOLATON keresztül csatlakozni.) Győződjön meg arról, hogy a HTTP van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="7e683-140">(The default is to connect over HTTPS.) Make sure that HTTP is selected.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="7e683-141">A HTTP-n keresztüli csatlakozást kizárólag megbízható hálózatokon célszerű engedélyezni.</span><span class="sxs-lookup"><span data-stu-id="7e683-141">Connecting over HTTP is acceptable only on trusted networks.</span></span>
   
5. <span data-ttu-id="7e683-142">Kattintson a **mentése** és megerősítést kér, amikor **Igen**.</span><span class="sxs-lookup"><span data-stu-id="7e683-142">Click **Save** and when prompted for confirmation, select **Yes**.</span></span>

### <a name="use-the-serial-console-to-enable-remote-management-over-http"></a><span data-ttu-id="7e683-143">A soros konzol használatával engedélyezze a távoli felügyeleti HTTP Protokollon keresztül</span><span class="sxs-lookup"><span data-stu-id="7e683-143">Use the serial console to enable remote management over HTTP</span></span>
<span data-ttu-id="7e683-144">Hajtsa végre a következő lépéseket távoli felügyeletének engedélyezése az eszköz soros konzoljához.</span><span class="sxs-lookup"><span data-stu-id="7e683-144">Perform the following steps on the device serial console to enable remote management.</span></span>

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a><span data-ttu-id="7e683-145">Az eszköz soros konzolon keresztül távoli felügyeletének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="7e683-145">To enable remote management through the device serial console</span></span>
1. <span data-ttu-id="7e683-146">A soros konzol menüben válassza az 1. lehetőség.</span><span class="sxs-lookup"><span data-stu-id="7e683-146">On the serial console menu, select option 1.</span></span> <span data-ttu-id="7e683-147">Az eszközön a soros konzol használatával kapcsolatos további információkért látogasson el [csatlakozás Windows PowerShell-lel eszköz soros konzolon keresztül](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="7e683-147">For more information about using the serial console on the device, go to [Connect to Windows PowerShell for StorSimple via device serial console](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span></span>
2. <span data-ttu-id="7e683-148">A parancssorba írja be:`Enable-HcsRemoteManagement –AllowHttp`</span><span class="sxs-lookup"><span data-stu-id="7e683-148">At the prompt, type: `Enable-HcsRemoteManagement –AllowHttp`</span></span>
3. <span data-ttu-id="7e683-149">A HTTP protokollal csatlakozni az eszköz biztonsági rések kapcsolatos értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="7e683-149">You are notified about the security vulnerabilities of using HTTP to connect to the device.</span></span> <span data-ttu-id="7e683-150">Amikor a rendszer kéri, erősítse meg, írja be a **Y**.</span><span class="sxs-lookup"><span data-stu-id="7e683-150">When prompted, confirm by typing **Y**.</span></span>
4. <span data-ttu-id="7e683-151">Győződjön meg arról, hogy a HTTP engedélyezett beírásával:`Get-HcsSystem`</span><span class="sxs-lookup"><span data-stu-id="7e683-151">Verify that HTTP is enabled by typing: `Get-HcsSystem`</span></span>
5. <span data-ttu-id="7e683-152">Ellenőrizze, hogy a **RemoteManagementMode** mezőben látható **HttpsAndHttpEnabled**. A következő ábrán ezek a beállítások a PuTTY.</span><span class="sxs-lookup"><span data-stu-id="7e683-152">Verify that the **RemoteManagementMode** field shows **HttpsAndHttpEnabled**.The following illustration shows these settings in PuTTY.</span></span>
   
     ![Soros HTTPS- és HTTP engedélyezve](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-the-client-for-remote-connection"></a><span data-ttu-id="7e683-154">Az ügyfélszoftver előkészítése a távoli kapcsolat</span><span class="sxs-lookup"><span data-stu-id="7e683-154">Prepare the client for remote connection</span></span>
<span data-ttu-id="7e683-155">A következő lépésekkel távoli felügyeletének engedélyezése az ügyfélen.</span><span class="sxs-lookup"><span data-stu-id="7e683-155">Perform the following steps on the client to enable remote management.</span></span>

#### <a name="to-prepare-the-client-for-remote-connection"></a><span data-ttu-id="7e683-156">Az ügyfélszoftver előkészítése a távoli kapcsolat</span><span class="sxs-lookup"><span data-stu-id="7e683-156">To prepare the client for remote connection</span></span>
1. <span data-ttu-id="7e683-157">Indítsa el a Windows PowerShell-munkamenetet rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="7e683-157">Start a Windows PowerShell session as an administrator.</span></span>
2. <span data-ttu-id="7e683-158">Írja be a következő parancs futtatásával adja hozzá a StorSimple eszköz IP-címét az ügyfél megbízható állomások listájához:</span><span class="sxs-lookup"><span data-stu-id="7e683-158">Type the following command to add the IP address of the StorSimple device to the client’s trusted hosts list:</span></span>
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
     <span data-ttu-id="7e683-159">Cserélje le <*device_ip*> az IP-cím, az eszköz; például:</span><span class="sxs-lookup"><span data-stu-id="7e683-159">Replace <*device_ip*> with the IP address of your device; for example:</span></span> 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. <span data-ttu-id="7e683-160">Írja be a következő parancsot a hitelesítő adatai mentése változóként:</span><span class="sxs-lookup"><span data-stu-id="7e683-160">Type the following command to save the device credentials in a variable:</span></span> 
   
    ```
    $cred = Get-Credential
    ```
    
4. <span data-ttu-id="7e683-161">A megjelenő párbeszédpanelen:</span><span class="sxs-lookup"><span data-stu-id="7e683-161">In the dialog box that appears:</span></span>
   
   1. <span data-ttu-id="7e683-162">Írja be a felhasználónevet a következő formátumban: *device_ip\SSAdmin*.</span><span class="sxs-lookup"><span data-stu-id="7e683-162">Type the user name in this format: *device_ip\SSAdmin*.</span></span>
   2. <span data-ttu-id="7e683-163">Írja be az eszköz rendszergazdai jelszava, amely lett beállítva, amikor az eszköz a telepítővarázsló lett konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="7e683-163">Type the device administrator password that was set when the device was configured with the setup wizard.</span></span> <span data-ttu-id="7e683-164">Az alapértelmezett jelszó *jelszó1*.</span><span class="sxs-lookup"><span data-stu-id="7e683-164">The default password is *Password1*.</span></span>
5. <span data-ttu-id="7e683-165">Ez a parancs beírásával indítsa el a Windows PowerShell-munkamenetet az eszközön:</span><span class="sxs-lookup"><span data-stu-id="7e683-165">Start a Windows PowerShell session on the device by typing this command:</span></span>
   
     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`
   
   > [!NOTE]
   > <span data-ttu-id="7e683-166">A StorSimple virtuális eszköz létrehozásához használja a Windows PowerShell-munkamenetet, hozzáfűzése a `–Port` paraméter, és adja meg a nyilvános port a StorSimple virtuális készülék távoli eljáráshívási konfigurált.</span><span class="sxs-lookup"><span data-stu-id="7e683-166">To create a Windows PowerShell session for use with the StorSimple virtual device, append the `–Port` parameter and specify the public port that you configured in Remoting for StorSimple Virtual Appliance.</span></span>
   
   
<span data-ttu-id="7e683-167">Ezen a ponton rendelkeznie kell egy aktív távoli Windows PowerShell-munkamenetben az eszközre.</span><span class="sxs-lookup"><span data-stu-id="7e683-167">At this point, you should have an active remote Windows PowerShell session to the device.</span></span>
   
![PowerShell távvezérlése HTTP-n keresztül](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a><span data-ttu-id="7e683-169">HTTPS keresztül csatlakozni</span><span class="sxs-lookup"><span data-stu-id="7e683-169">Connect through HTTPS</span></span>

<span data-ttu-id="7e683-170">Csatlakozás a Windows PowerShell StorSimple egy HTTPS-kapcsolaton keresztül a legbiztonságosabb és ajánlott mód távolról kapcsolódni a Microsoft Azure StorSimple eszközt.</span><span class="sxs-lookup"><span data-stu-id="7e683-170">Connecting to Windows PowerShell for StorSimple through an HTTPS session is the most secure and recommended method of remotely connecting to your Microsoft Azure StorSimple device.</span></span> <span data-ttu-id="7e683-171">Az alábbi eljárások azt ismertetik, hogy a Windows PowerShell csatlakoztatni a StorSimple is a HTTPS PROTOKOLLT használja a soros konzol és az ügyfél számítógépek beállítása.</span><span class="sxs-lookup"><span data-stu-id="7e683-171">The following procedures explain how to set up the serial console and client computers so that you can use HTTPS to connect to Windows PowerShell for StorSimple.</span></span>

<span data-ttu-id="7e683-172">Az Azure-portálon vagy a soros konzol segítségével távoli felügyeletének konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="7e683-172">You can use either the Azure portal or the serial console to configure remote management.</span></span> <span data-ttu-id="7e683-173">Az alábbi eljárások közül választhat:</span><span class="sxs-lookup"><span data-stu-id="7e683-173">Select from the following procedures:</span></span>

* [<span data-ttu-id="7e683-174">Az Azure portál segítségével távoli felügyeletének engedélyezése a HTTPS-KAPCSOLATON keresztül</span><span class="sxs-lookup"><span data-stu-id="7e683-174">Use the Azure portal to enable remote management over HTTPS</span></span>](#use-the-azure-classic-portal-to-enable-remote-management-over-https)
* [<span data-ttu-id="7e683-175">A soros konzol használatával engedélyezze a távoli felügyeleti HTTPS-KAPCSOLATON keresztül</span><span class="sxs-lookup"><span data-stu-id="7e683-175">Use the serial console to enable remote management over HTTPS</span></span>](#use-the-serial-console-to-enable-remote-management-over-https)

<span data-ttu-id="7e683-176">Miután engedélyezte távfelügyeletet, az alábbi eljárásokkal egy távoli kezeléshez való előkészítése a gazdagép és az eszköz csatlakozzon a távoli állomástól.</span><span class="sxs-lookup"><span data-stu-id="7e683-176">After you enable remote management, use the following procedures to prepare the host for a remote management and connect to the device from the remote host.</span></span>

* [<span data-ttu-id="7e683-177">A gazdagép Felkészülés a távfelügyeletre</span><span class="sxs-lookup"><span data-stu-id="7e683-177">Prepare the host for remote management</span></span>](#prepare-the-host-for-remote-management)
* [<span data-ttu-id="7e683-178">Az eszköz csatlakozzon a távoli gazdagépen</span><span class="sxs-lookup"><span data-stu-id="7e683-178">Connect to the device from the remote host</span></span>](#connect-to-the-device-from-the-remote-host)

### <a name="use-the-azure-portal-to-enable-remote-management-over-https"></a><span data-ttu-id="7e683-179">Az Azure portál segítségével távoli felügyeletének engedélyezése a HTTPS-KAPCSOLATON keresztül</span><span class="sxs-lookup"><span data-stu-id="7e683-179">Use the Azure portal to enable remote management over HTTPS</span></span>

<span data-ttu-id="7e683-180">Hajtsa végre az alábbi lépéseket az Azure-portálon távoli felügyeletének engedélyezése a HTTPS-KAPCSOLATON keresztül.</span><span class="sxs-lookup"><span data-stu-id="7e683-180">Perform the following steps in the Azure portal to enable remote management over HTTPS.</span></span>

#### <a name="to-enable-remote-management-over-https-from-the-azure-portal"></a><span data-ttu-id="7e683-181">HTTPS-KAPCSOLATON keresztül távoli felügyeletének engedélyezése az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="7e683-181">To enable remote management over HTTPS from the Azure portal</span></span>

1. <span data-ttu-id="7e683-182">Nyissa meg a StorSimple-eszközkezelő szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="7e683-182">Go to your StorSimple Device Manager service.</span></span> <span data-ttu-id="7e683-183">Válassza ki **eszközök** majd válassza ki, és kattintson a konfigurálni kívánt távoli kezelési eszköz.</span><span class="sxs-lookup"><span data-stu-id="7e683-183">Select **Devices** and then select and click the device you want to configure for remote management.</span></span> <span data-ttu-id="7e683-184">Ugrás a **eszközbeállítások > biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="7e683-184">Go to **Device settings > Security**.</span></span>
2. <span data-ttu-id="7e683-185">Az a **biztonsági beállítások** panelen kattintson a **Távfelügyelet**.</span><span class="sxs-lookup"><span data-stu-id="7e683-185">In the **Security settings** blade, click **Remote Management**.</span></span>
3. <span data-ttu-id="7e683-186">A **Távoli felügyelet engedélyezése** lehetőségnél válassza az **Igen** beállítást.</span><span class="sxs-lookup"><span data-stu-id="7e683-186">Set **Enable Remote Management** to **Yes**.</span></span>
4. <span data-ttu-id="7e683-187">Ha most szeretné HTTPS protokoll használatával kapcsolódó levelezőprogramokkal.</span><span class="sxs-lookup"><span data-stu-id="7e683-187">You can now choose to connect using HTTPS.</span></span> <span data-ttu-id="7e683-188">(Az alapértelmezett érték a HTTPS-KAPCSOLATON keresztül csatlakozni.) Győződjön meg arról, hogy HTTPS van-e kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="7e683-188">(The default is to connect over HTTPS.) Make sure that HTTPS is selected.</span></span>
5. <span data-ttu-id="7e683-189">Kattintson... majd **távfelügyeleti tanúsítvány letöltése**.</span><span class="sxs-lookup"><span data-stu-id="7e683-189">Click ... and then click **Download Remote Management Certificate**.</span></span> <span data-ttu-id="7e683-190">Adjon meg egy helyet a fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="7e683-190">Specify a location to save this file.</span></span> <span data-ttu-id="7e683-191">A tanúsítvány telepítése a számítógépen az ügyfélszámítógépre vagy állomásra, amely az eszköz való kapcsolódáshoz használandó kell.</span><span class="sxs-lookup"><span data-stu-id="7e683-191">You need to install this certificate on the client or host computer that you will use to connect to the device.</span></span>
6. <span data-ttu-id="7e683-192">Kattintson a **mentése** majd **Igen** során a rendszer megerősítést kér.</span><span class="sxs-lookup"><span data-stu-id="7e683-192">Click **Save** and then click **Yes** when prompted for confirmation.</span></span>

### <a name="use-the-serial-console-to-enable-remote-management-over-https"></a><span data-ttu-id="7e683-193">A soros konzol használatával engedélyezze a távoli felügyeleti HTTPS-KAPCSOLATON keresztül</span><span class="sxs-lookup"><span data-stu-id="7e683-193">Use the serial console to enable remote management over HTTPS</span></span>

<span data-ttu-id="7e683-194">Hajtsa végre a következő lépéseket távoli felügyeletének engedélyezése az eszköz soros konzoljához.</span><span class="sxs-lookup"><span data-stu-id="7e683-194">Perform the following steps on the device serial console to enable remote management.</span></span>

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a><span data-ttu-id="7e683-195">Az eszköz soros konzolon keresztül távoli felügyeletének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="7e683-195">To enable remote management through the device serial console</span></span>
1. <span data-ttu-id="7e683-196">A soros konzol menüben válassza az 1. lehetőség.</span><span class="sxs-lookup"><span data-stu-id="7e683-196">On the serial console menu, select option 1.</span></span> <span data-ttu-id="7e683-197">Az eszközön a soros konzol használatával kapcsolatos további információkért látogasson el [csatlakozás Windows PowerShell-lel eszköz soros konzolon keresztül](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="7e683-197">For more information about using the serial console on the device, go to [Connect to Windows PowerShell for StorSimple via device serial console](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).</span></span>
2. <span data-ttu-id="7e683-198">A parancssorba írja be:</span><span class="sxs-lookup"><span data-stu-id="7e683-198">At the prompt, type:</span></span>
   
     `Enable-HcsRemoteManagement`
   
    <span data-ttu-id="7e683-199">Ez engedélyezze a HTTPS-eszközön.</span><span class="sxs-lookup"><span data-stu-id="7e683-199">This should enable HTTPS on your device.</span></span>
3. <span data-ttu-id="7e683-200">Ellenőrizze, hogy engedélyezték-e a HTTPS beírásával:</span><span class="sxs-lookup"><span data-stu-id="7e683-200">Verify that HTTPS has been enabled by typing:</span></span> 
   
     `Get-HcsSystem`
   
    <span data-ttu-id="7e683-201">Győződjön meg arról, hogy a **RemoteManagementMode** mezőben látható **HttpsEnabled**. A következő ábrán ezek a beállítások a PuTTY.</span><span class="sxs-lookup"><span data-stu-id="7e683-201">Make sure that the **RemoteManagementMode** field shows **HttpsEnabled**.The following illustration shows these settings in PuTTY.</span></span>
   
     ![Soros HTTPS-kompatibilis](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)
4. <span data-ttu-id="7e683-203">Kimenetében `Get-HcsSystem`, másolja a sorozatszámot az eszköz, és mentse azokat a későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="7e683-203">From the output of `Get-HcsSystem`, copy the serial number of the device and save it for later use.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="7e683-204">A sorozatszám van leképezve a tanúsítvány CN-név.</span><span class="sxs-lookup"><span data-stu-id="7e683-204">The serial number maps to the CN name in the certificate.</span></span>
   
5. <span data-ttu-id="7e683-205">Írja be a távoli felügyeleti tanúsítvány beszerzése:</span><span class="sxs-lookup"><span data-stu-id="7e683-205">Obtain a remote management certificate by typing:</span></span> 
   
     `Get-HcsRemoteManagementCert`
   
    <span data-ttu-id="7e683-206">Egy tanúsítványt a következőhöz hasonló jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7e683-206">A certificate similar to the following will appear.</span></span>
   
    ![Távfelügyeleti tanúsítvány beszerzése](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)
6. <span data-ttu-id="7e683-208">Másolja az adatokat a tanúsítványban lévő **---BEGIN CERTIFICATE---** való **---vége tanúsítvány---** egy szövegszerkesztőbe például a Jegyzettömbben, és mentse azt egy .cer fájlba.</span><span class="sxs-lookup"><span data-stu-id="7e683-208">Copy the information in the certificate from **-----BEGIN CERTIFICATE-----** to **-----END CERTIFICATE-----** into a text editor such as Notepad, and save it as a .cer file.</span></span> <span data-ttu-id="7e683-209">(Ön másolja ezt a fájlt a távoli állomás a gazdagép előkészítésekor.)</span><span class="sxs-lookup"><span data-stu-id="7e683-209">(You will copy this file to your remote host when you prepare the host.)</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="7e683-210">Új tanúsítvány létrehozásához használja a `Set-HcsRemoteManagementCert` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="7e683-210">To generate a new certificate, use the `Set-HcsRemoteManagementCert` cmdlet.</span></span>
   
### <a name="prepare-the-host-for-remote-management"></a><span data-ttu-id="7e683-211">A gazdagép Felkészülés a távfelügyeletre</span><span class="sxs-lookup"><span data-stu-id="7e683-211">Prepare the host for remote management</span></span>

<span data-ttu-id="7e683-212">Egy távoli kapcsolathoz, amely HTTPS-KAPCSOLATON keresztül használja a számítógép előkészítéséhez hajtsa végre az alábbi eljárásokat:</span><span class="sxs-lookup"><span data-stu-id="7e683-212">To prepare the host computer for a remote connection that uses an HTTPS session, perform the following procedures:</span></span>

* <span data-ttu-id="7e683-213">[A .cer fájlt importálja a gyökérszintű tárolóban. az ügyfél vagy a távoli állomás](#to-import-the-certificate-on-the-remote-host).</span><span class="sxs-lookup"><span data-stu-id="7e683-213">[Import the .cer file into the root store of the client or remote host](#to-import-the-certificate-on-the-remote-host).</span></span>
* <span data-ttu-id="7e683-214">[Az eszközök sorozatszámait hozzáadása az állomásleíró fájlhoz, a távoli állomáson levő](#to-add-device-serial-numbers-to-the-remote-host).</span><span class="sxs-lookup"><span data-stu-id="7e683-214">[Add the device serial numbers to the hosts file on your remote host](#to-add-device-serial-numbers-to-the-remote-host).</span></span>

<span data-ttu-id="7e683-215">A fenti eljárások leírását alább.</span><span class="sxs-lookup"><span data-stu-id="7e683-215">Each of the preceding procedures, is described below.</span></span>

#### <a name="to-import-the-certificate-on-the-remote-host"></a><span data-ttu-id="7e683-216">A távoli állomáson levő tanúsítvány importálása</span><span class="sxs-lookup"><span data-stu-id="7e683-216">To import the certificate on the remote host</span></span>
1. <span data-ttu-id="7e683-217">Kattintson a jobb gombbal a .cer fájlt, és válassza ki **telepítés tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="7e683-217">Right-click the .cer file and select **Install certificate**.</span></span> <span data-ttu-id="7e683-218">A Tanúsítványimportáló varázsló elindul.</span><span class="sxs-lookup"><span data-stu-id="7e683-218">This starts the Certificate Import Wizard.</span></span>
   
    ![Tanúsítványimportáló varázsló 1](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)
2. <span data-ttu-id="7e683-220">A **tárolóhelyére**, jelölje be **helyi számítógép**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="7e683-220">For **Store location**, select **Local Machine**, and then click **Next**.</span></span>
3. <span data-ttu-id="7e683-221">Válassza ki **minden tanúsítvány tárolása ebben a tárolóban**, és kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="7e683-221">Select **Place all certificates in the following store**, and then click **Browse**.</span></span> <span data-ttu-id="7e683-222">Keresse meg a gyökérszintű tárolóban. a távoli állomás, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="7e683-222">Navigate to the root store of your remote host, and then click **Next**.</span></span>
   
    ![Tanúsítványimportáló varázsló 2. régiója](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)
4. <span data-ttu-id="7e683-224">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="7e683-224">Click **Finish**.</span></span> <span data-ttu-id="7e683-225">Megjelenik egy üzenet, amely közli, hogy az importálás sikeres volt-e.</span><span class="sxs-lookup"><span data-stu-id="7e683-225">A message that tells you that the import was successful appears.</span></span>
   
    ![Tanúsítványimportáló varázsló 3](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="to-add-device-serial-numbers-to-the-remote-host"></a><span data-ttu-id="7e683-227">Az eszközök sorozatszámait hozzáadása a távoli állomás</span><span class="sxs-lookup"><span data-stu-id="7e683-227">To add device serial numbers to the remote host</span></span>
1. <span data-ttu-id="7e683-228">Rendszergazdaként a Jegyzettömb, és nyissa meg a \Windows\System32\Drivers\etc helyen található gazdafájlt.</span><span class="sxs-lookup"><span data-stu-id="7e683-228">Start Notepad as an administrator, and then open the hosts file located at \Windows\System32\Drivers\etc.</span></span>
2. <span data-ttu-id="7e683-229">Adja hozzá a következő három bejegyzéseket a hosts fájl: **DATA 0 IP-cím**, **0 vezérlő rögzített IP-cím**, és **vezérlő 1 fix IP-cím**.</span><span class="sxs-lookup"><span data-stu-id="7e683-229">Add the following three entries to your hosts file: **DATA 0 IP address**, **Controller 0 Fixed IP address**, and **Controller 1 Fixed IP address**.</span></span>
3. <span data-ttu-id="7e683-230">Adja meg a korábban mentett eszköz sorozatszámát.</span><span class="sxs-lookup"><span data-stu-id="7e683-230">Enter the device serial number that you saved earlier.</span></span> <span data-ttu-id="7e683-231">Képezze le az ezt az IP-cím a következő ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="7e683-231">Map this to the IP address as shown in the following image.</span></span> <span data-ttu-id="7e683-232">A vezérlő 0 és a vezérlő 1 hozzáfűzése **Controller0** és **Controller1** a sorozatszám (CN-név) végén.</span><span class="sxs-lookup"><span data-stu-id="7e683-232">For Controller 0 and Controller 1, append **Controller0** and **Controller1** at the end of the serial number (CN name).</span></span>
   
    ![CN-név felvétele a hosts fájl](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)
4. <span data-ttu-id="7e683-234">Mentse a hosts fájlt.</span><span class="sxs-lookup"><span data-stu-id="7e683-234">Save the hosts file.</span></span>

### <a name="connect-to-the-device-from-the-remote-host"></a><span data-ttu-id="7e683-235">Az eszköz csatlakozzon a távoli gazdagépen</span><span class="sxs-lookup"><span data-stu-id="7e683-235">Connect to the device from the remote host</span></span>

<span data-ttu-id="7e683-236">A Windows PowerShell és az SSL segítségével adjon meg egy SSAdmin munkamenet az eszköz az ügyfél vagy egy távoli állomástól.</span><span class="sxs-lookup"><span data-stu-id="7e683-236">Use Windows PowerShell and SSL to enter an SSAdmin session on your device from a remote host or client.</span></span> <span data-ttu-id="7e683-237">Az 1. lehetőség van leképezve a SSAdmin munkamenet a [soros konzol](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) menü az eszköz.</span><span class="sxs-lookup"><span data-stu-id="7e683-237">The SSAdmin session maps to option 1 in the [serial console](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) menu of your device.</span></span>

<span data-ttu-id="7e683-238">Az alábbi eljárás végrehajtásához a számítógépen, amelyből el kívánja a távoli Windows PowerShell-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="7e683-238">Perform the following procedure on the computer from which you want to make the remote Windows PowerShell connection.</span></span>

#### <a name="to-enter-an-ssadmin-session-on-the-device-by-using-windows-powershell-and-ssl"></a><span data-ttu-id="7e683-239">Adjon meg egy SSAdmin munkamenet az eszközön a Windows PowerShell és az SSL használatával történő</span><span class="sxs-lookup"><span data-stu-id="7e683-239">To enter an SSAdmin session on the device by using Windows PowerShell and SSL</span></span>
1. <span data-ttu-id="7e683-240">Indítsa el a Windows PowerShell-munkamenetet rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="7e683-240">Start a Windows PowerShell session as an administrator.</span></span>
2. <span data-ttu-id="7e683-241">Az ügyfél megbízható állomások írja be az eszköz IP-cím hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="7e683-241">Add the device IP address to the client’s trusted hosts by typing:</span></span>
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
    <span data-ttu-id="7e683-242">Ahol <*device_ip*> az eszköz IP-címét, például:</span><span class="sxs-lookup"><span data-stu-id="7e683-242">Where <*device_ip*> is the IP address of your device; for example:</span></span> 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. <span data-ttu-id="7e683-243">Új hitelesítő adatokat írja be a következőt:</span><span class="sxs-lookup"><span data-stu-id="7e683-243">To create a new credential, type:</span></span>
   
     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`
   
    <span data-ttu-id="7e683-244">Ahol <*IP-címe céleszközön*> 0 adatok az eszköz; IP-címe például **10.126.173.90** a hosts fájl az előző ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="7e683-244">Where <*IP of target device*> is the IP address of DATA 0 for your device; for example, **10.126.173.90** as shown in the preceding image of the hosts file.</span></span> <span data-ttu-id="7e683-245">Továbbá adja meg az eszköz rendszergazdai jelszava.</span><span class="sxs-lookup"><span data-stu-id="7e683-245">Also, supply the administrator password for your device.</span></span>
4. <span data-ttu-id="7e683-246">Írja be a munkamenet létrehozása:</span><span class="sxs-lookup"><span data-stu-id="7e683-246">Create a session by typing:</span></span>
   
     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`
   
    <span data-ttu-id="7e683-247">A parancsmag - ComputerName paraméter adja meg a <*figyelt eszköz sorozatszámát*>.</span><span class="sxs-lookup"><span data-stu-id="7e683-247">For the -ComputerName parameter in the cmdlet, provide the <*serial number of target device*>.</span></span> <span data-ttu-id="7e683-248">Ezzel a sorozatszámmal leképezve a következőre: DATA 0 a hosts fájl, a távoli állomáson; IP-címe például **SHX0991003G44MT** a következő ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="7e683-248">This serial number was mapped to the IP address of DATA 0 in the hosts file on your remote host; for example, **SHX0991003G44MT** as shown in the following image.</span></span>
5. <span data-ttu-id="7e683-249">Típus:</span><span class="sxs-lookup"><span data-stu-id="7e683-249">Type:</span></span>
   
     `Enter-PSSession $session`
6. <span data-ttu-id="7e683-250">Várjon néhány percet kell, és majd meg fog csatlakozni az eszköz keresztül HTTPS SSL-en keresztül.</span><span class="sxs-lookup"><span data-stu-id="7e683-250">You will need to wait a few minutes, and then you will be connected to your device via HTTPS over SSL.</span></span> <span data-ttu-id="7e683-251">Megjelenik egy üzenet, amely jelzi, hogy az eszköz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="7e683-251">You see a message that indicates you are connected to your device.</span></span>
   
    ![PowerShell távvezérlése HTTPS és az SSL használata](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a><span data-ttu-id="7e683-253">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7e683-253">Next steps</span></span>

* <span data-ttu-id="7e683-254">További információ [a StorSimple eszköz felügyelete a Windows PowerShell használatával](storsimple-8000-windows-powershell-administration.md).</span><span class="sxs-lookup"><span data-stu-id="7e683-254">Learn more about [using Windows PowerShell to administer your StorSimple device](storsimple-8000-windows-powershell-administration.md).</span></span>
* <span data-ttu-id="7e683-255">További információ [felügyelete a StorSimple eszközt a StorSimple Device Manager szolgáltatással](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="7e683-255">Learn more about [using the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

