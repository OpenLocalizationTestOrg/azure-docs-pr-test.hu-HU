---
title: "a StorSimple támogatási csomag aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate, visszafejtése, és a StorSimple eszköz támogatási csomag szerkesztése."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: eac76f5f-5db1-4c92-af8c-54053b91e66c
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 209aeee50e823fd2ca96ababd1d0cf3ea9cdad53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-a-storsimple-support-package"></a><span data-ttu-id="f42f6-103">Létrehozását és kezelését egy StorSimple támogatási csomag</span><span class="sxs-lookup"><span data-stu-id="f42f6-103">Create and manage a StorSimple support package</span></span>
## <a name="overview"></a><span data-ttu-id="f42f6-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="f42f6-104">Overview</span></span>
<span data-ttu-id="f42f6-105">A StorSimple támogatási csomag olyan könnyen használható mechanizmus, amely az összes releváns naplók tooassist Microsoft Support gyűjti a hibaelhárításban a StorSimple eszköz probléma merül fel.</span><span class="sxs-lookup"><span data-stu-id="f42f6-105">A StorSimple support package is an easy-to-use mechanism that collects all relevant logs tooassist Microsoft Support with troubleshooting any StorSimple device issues.</span></span> <span data-ttu-id="f42f6-106">hello gyűjtött naplók titkosított és tömörített.</span><span class="sxs-lookup"><span data-stu-id="f42f6-106">hello collected logs are encrypted and compressed.</span></span>

<span data-ttu-id="f42f6-107">Ez az oktatóanyag lépéseit toocreate tartalmazza, és hello támogatási csomag kezelése.</span><span class="sxs-lookup"><span data-stu-id="f42f6-107">This tutorial includes step-by-step instructions toocreate and manage hello support package.</span></span>

## <a name="create-and-upload-a-support-package-in-hello-azure-classic-portal"></a><span data-ttu-id="f42f6-108">Hozzon létre, és töltse fel a klasszikus Azure portálon hello támogatási csomag</span><span class="sxs-lookup"><span data-stu-id="f42f6-108">Create and upload a support package in hello Azure classic portal</span></span>
<span data-ttu-id="f42f6-109">Hozzon létre, és töltse fel egy támogatási csomag toohello Microsoft Support webhelyén keresztül hello **karbantartási** hello szolgáltatást a klasszikus Azure portálon hello oldalán.</span><span class="sxs-lookup"><span data-stu-id="f42f6-109">You can create and upload a support package toohello Microsoft Support site through hello **Maintenance** page of hello service in hello Azure classic portal.</span></span>

> [!NOTE]
> <span data-ttu-id="f42f6-110">hello feltöltés támogatási hitelesítő kulcs szükséges.</span><span class="sxs-lookup"><span data-stu-id="f42f6-110">hello upload requires a support passkey.</span></span> <span data-ttu-id="f42f6-111">A támogatási szakértőhöz biztosítania kell a tooyou e-mailben.</span><span class="sxs-lookup"><span data-stu-id="f42f6-111">Your support engineer should provide this tooyou in an email.</span></span>
> 
> 

<span data-ttu-id="f42f6-112">Egy titkosított és tömörített támogatási csomagot (.cab fájl) létrehozása és feltöltése toohello támogatási hely.</span><span class="sxs-lookup"><span data-stu-id="f42f6-112">An encrypted and compressed support package (.cab file) is created and uploaded toohello Support site.</span></span> <span data-ttu-id="f42f6-113">hello támogatási szakértőhöz majd beolvasni a csomag hello hiba elhárításához hello támogatási helyről.</span><span class="sxs-lookup"><span data-stu-id="f42f6-113">hello support engineer can then retrieve this package from hello Support site for troubleshooting hello issue.</span></span>

<span data-ttu-id="f42f6-114">Hajtsa végre a következő lépéseket a hello klasszikus portál toocreate egy támogatási csomag hello.</span><span class="sxs-lookup"><span data-stu-id="f42f6-114">Perform hello following steps in hello classic portal toocreate a support package.</span></span>

#### <a name="toocreate-a-support-package-in-hello-azure-classic-portal"></a><span data-ttu-id="f42f6-115">a klasszikus Azure portálon hello támogatási csomag toocreate</span><span class="sxs-lookup"><span data-stu-id="f42f6-115">toocreate a support package in hello Azure classic portal</span></span>
1. <span data-ttu-id="f42f6-116">Válassza ki **eszközök** > **karbantartási**.</span><span class="sxs-lookup"><span data-stu-id="f42f6-116">Select **Devices** > **Maintenance**.</span></span>
2. <span data-ttu-id="f42f6-117">A hello **támogatási csomag** szakaszban jelölje be **létrehozása és feltöltése támogatási csomag**.</span><span class="sxs-lookup"><span data-stu-id="f42f6-117">In hello **Support package** section, select **Create and upload support package**.</span></span>
3. <span data-ttu-id="f42f6-118">A hello **létrehozása és feltöltése támogatási csomag** párbeszédpanel mezőbe hello a következő:</span><span class="sxs-lookup"><span data-stu-id="f42f6-118">In hello **Create and upload support package** dialog box, do hello following:</span></span>
   
    ![Támogatási csomag létrehozása](./media/storsimple-create-manage-support-package/IC740923.png)
   
   * <span data-ttu-id="f42f6-120">A hello **támogatási hozzáférési kulcs** szöveg mezőbe írja be a hello hozzáférési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="f42f6-120">In hello **Support Passkey** text box, enter hello passkey.</span></span> <span data-ttu-id="f42f6-121">A Microsoft támogatási szakértőhöz a hozzáférési kulcs tooyou küldjön e-mailben.</span><span class="sxs-lookup"><span data-stu-id="f42f6-121">Your Microsoft support engineer should send this passkey tooyou in email.</span></span>
   * <span data-ttu-id="f42f6-122">Válassza ki a hello jelölőnégyzetet tooprovide hozzájárulási tooautomatically feltöltés hello támogatási csomag toohello Microsoft Support helyet.</span><span class="sxs-lookup"><span data-stu-id="f42f6-122">Select hello check box tooprovide consent tooautomatically upload hello support package toohello Microsoft Support site.</span></span>
   * <span data-ttu-id="f42f6-123">Kattintson a pipa ikonra hello</span><span class="sxs-lookup"><span data-stu-id="f42f6-123">Click hello check icon</span></span> ![Pipa ikon](./media/storsimple-create-manage-support-package/IC740895.png)<span data-ttu-id="f42f6-125">.</span><span class="sxs-lookup"><span data-stu-id="f42f6-125">.</span></span>

## <a name="manually-create-a-support-package"></a><span data-ttu-id="f42f6-126">Manuálisan hozzon létre egy támogatási csomag</span><span class="sxs-lookup"><span data-stu-id="f42f6-126">Manually create a support package</span></span>
<span data-ttu-id="f42f6-127">Néhány esetben szüksége lesz toomanually StorSimple hello támogatási csomag a Windows PowerShell segítségével hozzon létre.</span><span class="sxs-lookup"><span data-stu-id="f42f6-127">In some cases, you'll need toomanually create hello support package through Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="f42f6-128">Példa:</span><span class="sxs-lookup"><span data-stu-id="f42f6-128">For example:</span></span>

* <span data-ttu-id="f42f6-129">Ha a naplóból tooremove bizalmas adatokat kell fájlok Microsoft Support előzetes toosharing.</span><span class="sxs-lookup"><span data-stu-id="f42f6-129">If you need tooremove sensitive information from your log files prior toosharing with Microsoft Support.</span></span>
* <span data-ttu-id="f42f6-130">Ha a probléma miatt tooconnectivity problémák hello csomag feltöltése.</span><span class="sxs-lookup"><span data-stu-id="f42f6-130">If you are having difficulty uploading hello package due tooconnectivity issues.</span></span>

<span data-ttu-id="f42f6-131">A manuálisan létrehozott támogatási csomag Microsoft Support keresztül e-mailek megoszthatja.</span><span class="sxs-lookup"><span data-stu-id="f42f6-131">You can share your manually generated support package with Microsoft Support over email.</span></span> <span data-ttu-id="f42f6-132">Hajtsa végre a következő lépéseket toocreate egy támogatási csomag a Windows PowerShellben a StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="f42f6-132">Perform hello following steps toocreate a support package in Windows PowerShell for StorSimple.</span></span>

#### <a name="toocreate-a-support-package-in-windows-powershell-for-storsimple"></a><span data-ttu-id="f42f6-133">toocreate egy támogatási csomag a Windows PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="f42f6-133">toocreate a support package in Windows PowerShell for StorSimple</span></span>
1. <span data-ttu-id="f42f6-134">Windows PowerShell-munkamenet hello távoli számítógépen tooconnect tooyour StorSimple eszköz által használt rendszergazdaként toostart adja meg a hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="f42f6-134">toostart a Windows PowerShell session as an administrator on hello remote computer that's used tooconnect tooyour StorSimple device, enter hello following command:</span></span>
   
    `Start PowerShell`
2. <span data-ttu-id="f42f6-135">A Windows PowerShell-munkamenetben hello csatlakozás toohello SSAdmin konzol, az eszköz:</span><span class="sxs-lookup"><span data-stu-id="f42f6-135">In hello Windows PowerShell session, connect toohello SSAdmin Console of your device:</span></span>
   
   * <span data-ttu-id="f42f6-136">Hello parancssorba írja be:</span><span class="sxs-lookup"><span data-stu-id="f42f6-136">At hello command prompt, enter:</span></span>
     
       `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`
   * <span data-ttu-id="f42f6-137">Hello párbeszédpanel adja meg az eszköz rendszergazdai jelszava.</span><span class="sxs-lookup"><span data-stu-id="f42f6-137">In hello dialog box that opens, enter your device administrator password.</span></span> <span data-ttu-id="f42f6-138">hello alapértelmezett jelszava:</span><span class="sxs-lookup"><span data-stu-id="f42f6-138">hello default password is:</span></span>
     
      `Password1`
     
      ![PowerShell-hitelesítő adat párbeszédpanel](./media/storsimple-create-manage-support-package/IC740962.png)
   * <span data-ttu-id="f42f6-140">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="f42f6-140">Select **OK**.</span></span>
   * <span data-ttu-id="f42f6-141">Hello parancssorba írja be:</span><span class="sxs-lookup"><span data-stu-id="f42f6-141">At hello command prompt, enter:</span></span>
     
      `Enter-PSSession $MS`
3. <span data-ttu-id="f42f6-142">A megnyitott munkamenetben hello írja be a hello megfelelő parancsot.</span><span class="sxs-lookup"><span data-stu-id="f42f6-142">In hello session that opens, enter hello appropriate command.</span></span>
   
   * <span data-ttu-id="f42f6-143">Jelszóval védett hálózati megosztások adja meg:</span><span class="sxs-lookup"><span data-stu-id="f42f6-143">For network shares that are password protected, enter:</span></span>
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`
     
       <span data-ttu-id="f42f6-144">Kéri a jelszót, a elérési toohello hálózati megosztott mappából és a titkosítás jelszavát (mert hello támogatási csomag titkosított).</span><span class="sxs-lookup"><span data-stu-id="f42f6-144">You'll be prompted for a password, a path toohello network shared folder, and an encryption passphrase (because hello support package is encrypted).</span></span> <span data-ttu-id="f42f6-145">Egy támogatási csomag majd hello megadott mappában jön létre.</span><span class="sxs-lookup"><span data-stu-id="f42f6-145">A support package is then created in hello specified folder.</span></span>
   * <span data-ttu-id="f42f6-146">Az megosztások, amelyek nem jelszóval védett, nem kell hello `-Credential` paraméter.</span><span class="sxs-lookup"><span data-stu-id="f42f6-146">For shares that are not password protected, you do not need hello `-Credential` parameter.</span></span> <span data-ttu-id="f42f6-147">Írja be a következő hello:</span><span class="sxs-lookup"><span data-stu-id="f42f6-147">Enter hello following:</span></span>
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`
     
       <span data-ttu-id="f42f6-148">hello támogatási csomag mindkét tartományvezérlők hello megadott hálózati megosztott mappában jön létre.</span><span class="sxs-lookup"><span data-stu-id="f42f6-148">hello support package is created for both controllers in hello specified network shared folder.</span></span> <span data-ttu-id="f42f6-149">Egy titkosított, tömörített fájlt, amely elküldhető tooMicrosoft támogatási hibaelhárítási.</span><span class="sxs-lookup"><span data-stu-id="f42f6-149">It's an encrypted, compressed file that can be sent tooMicrosoft Support for troubleshooting.</span></span> <span data-ttu-id="f42f6-150">További információkért lásd: [forduljon a Microsoft ügyfélszolgálatához](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="f42f6-150">For more information, see [Contact Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>

### <a name="hello-export-hcssupportpackage-cmdlet-parameters"></a><span data-ttu-id="f42f6-151">Export-HcsSupportPackage hello parancsmag-paraméterek</span><span class="sxs-lookup"><span data-stu-id="f42f6-151">hello Export-HcsSupportPackage cmdlet parameters</span></span>
<span data-ttu-id="f42f6-152">A következő paraméterek hello Export-HcsSupportPackage parancsmaggal hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="f42f6-152">You can use hello following parameters with hello Export-HcsSupportPackage cmdlet.</span></span>

| <span data-ttu-id="f42f6-153">Paraméter</span><span class="sxs-lookup"><span data-stu-id="f42f6-153">Parameter</span></span> | <span data-ttu-id="f42f6-154">Kötelező/választható</span><span class="sxs-lookup"><span data-stu-id="f42f6-154">Required/Optional</span></span> | <span data-ttu-id="f42f6-155">Leírás</span><span class="sxs-lookup"><span data-stu-id="f42f6-155">Description</span></span> |
| --- | --- | --- |
| `-Path` |<span data-ttu-id="f42f6-156">Szükséges</span><span class="sxs-lookup"><span data-stu-id="f42f6-156">Required</span></span> |<span data-ttu-id="f42f6-157">Hello hálózati megosztott mappa mely hello támogatási csomag helyet adó tooprovide hello helyét használja.</span><span class="sxs-lookup"><span data-stu-id="f42f6-157">Use tooprovide hello location of hello network shared folder in which hello support package is placed.</span></span> |
| `-EncryptionPassphrase` |<span data-ttu-id="f42f6-158">Szükséges</span><span class="sxs-lookup"><span data-stu-id="f42f6-158">Required</span></span> |<span data-ttu-id="f42f6-159">Használjon tooprovide egy hozzáférési kódot toohelp hello támogatási csomag titkosításához.</span><span class="sxs-lookup"><span data-stu-id="f42f6-159">Use tooprovide a passphrase toohelp encrypt hello support package.</span></span> |
| `-Credential` |<span data-ttu-id="f42f6-160">Optional</span><span class="sxs-lookup"><span data-stu-id="f42f6-160">Optional</span></span> |<span data-ttu-id="f42f6-161">Hálózati megosztott mappa hello toosupply hozzáférési hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="f42f6-161">Use toosupply access credentials for hello network shared folder.</span></span> |
| `-Force` |<span data-ttu-id="f42f6-162">Optional</span><span class="sxs-lookup"><span data-stu-id="f42f6-162">Optional</span></span> |<span data-ttu-id="f42f6-163">Tooskip hello titkosítási jelszót megerősítési lépés használható.</span><span class="sxs-lookup"><span data-stu-id="f42f6-163">Use tooskip hello encryption passphrase confirmation step.</span></span> |
| `-PackageTag` |<span data-ttu-id="f42f6-164">Optional</span><span class="sxs-lookup"><span data-stu-id="f42f6-164">Optional</span></span> |<span data-ttu-id="f42f6-165">Használjon toospecify olyan könyvtárat *elérési* mely hello támogatott csomag kerül.</span><span class="sxs-lookup"><span data-stu-id="f42f6-165">Use toospecify a directory under *Path* in which hello support package is placed.</span></span> <span data-ttu-id="f42f6-166">hello alapértelmezett érték a [eszköznév]-[aktuális dátumot és time:yyyy-MM-dd-HH-mm-ss].</span><span class="sxs-lookup"><span data-stu-id="f42f6-166">hello default is [device name]-[current date and time:yyyy-MM-dd-HH-mm-ss].</span></span> |
| `-Scope` |<span data-ttu-id="f42f6-167">Optional</span><span class="sxs-lookup"><span data-stu-id="f42f6-167">Optional</span></span> |<span data-ttu-id="f42f6-168">Adja meg a **fürt** (alapértelmezett) toocreate egy támogatási csomag mindkét vezérlők.</span><span class="sxs-lookup"><span data-stu-id="f42f6-168">Specify as **Cluster** (default) toocreate a support package for both controllers.</span></span> <span data-ttu-id="f42f6-169">Ha azt szeretné, csak az aktuális vezérlő hello toocreate egy csomagot, adjon meg **vezérlő**.</span><span class="sxs-lookup"><span data-stu-id="f42f6-169">If you want toocreate a package only for hello current controller, specify **Controller**.</span></span> |

## <a name="edit-a-support-package"></a><span data-ttu-id="f42f6-170">Egy támogatási csomag szerkesztése</span><span class="sxs-lookup"><span data-stu-id="f42f6-170">Edit a support package</span></span>
<span data-ttu-id="f42f6-171">Miután létrehozta a támogatási csomag, szükség lehet a tooedit hello csomag tooremove bizalmas adatokat.</span><span class="sxs-lookup"><span data-stu-id="f42f6-171">After you have generated a support package, you might need tooedit hello package tooremove sensitive information.</span></span> <span data-ttu-id="f42f6-172">Ilyen lehet például a kötet, eszköz IP-címek vagy hello naplófájlokból biztonsági másolat neve.</span><span class="sxs-lookup"><span data-stu-id="f42f6-172">This can include volume names, device IP addresses, and backup names from hello log files.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f42f6-173">Csak akkor szerkeszthető egy támogatási csomag, amelyik a Windows PowerShell segítségével a StorSimple jött létre.</span><span class="sxs-lookup"><span data-stu-id="f42f6-173">You can only edit a support package that was generated through Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="f42f6-174">Hello StorSimple Manager szolgáltatás a klasszikus Azure portálon létrehozott csomagot nem szerkeszthetők.</span><span class="sxs-lookup"><span data-stu-id="f42f6-174">You can't edit a package created in hello Azure classic portal with StorSimple Manager service.</span></span>
> 
> 

<span data-ttu-id="f42f6-175">tooedit egy támogatási csomag hello Microsoft Support webhelyén, a feltöltés előtt először visszafejtéséhez hello támogatási csomag hello fájlok szerkesztése és újbóli titkosítására a azt.</span><span class="sxs-lookup"><span data-stu-id="f42f6-175">tooedit a support package before uploading it on hello Microsoft Support site, first decrypt hello support package, edit hello files, and then re-encrypt it.</span></span> <span data-ttu-id="f42f6-176">Hajtsa végre a következő lépéseket hello.</span><span class="sxs-lookup"><span data-stu-id="f42f6-176">Perform hello following steps.</span></span>

#### <a name="tooedit-a-support-package-in-windows-powershell-for-storsimple"></a><span data-ttu-id="f42f6-177">tooedit egy támogatási csomag a Windows PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="f42f6-177">tooedit a support package in Windows PowerShell for StorSimple</span></span>
1. <span data-ttu-id="f42f6-178">Egy támogatási csomagot leírtak korábban [toocreate egy támogatási csomag a Windows PowerShell-lel](#to-create-a-support-package-in-windows-powershell-for-storsimple).</span><span class="sxs-lookup"><span data-stu-id="f42f6-178">Generate a support package as described earlier, in [toocreate a support package in Windows PowerShell for StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).</span></span>
2. <span data-ttu-id="f42f6-179">[Töltse le a hello parancsfájl](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) helyileg az ügyfélen.</span><span class="sxs-lookup"><span data-stu-id="f42f6-179">[Download hello script](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) locally on your client.</span></span>
3. <span data-ttu-id="f42f6-180">Hello Windows PowerShell-modul importálásához.</span><span class="sxs-lookup"><span data-stu-id="f42f6-180">Import hello Windows PowerShell module.</span></span> <span data-ttu-id="f42f6-181">Adja meg a hello elérési toohello helyi mappát, amelyben hello parancsprogram letöltése.</span><span class="sxs-lookup"><span data-stu-id="f42f6-181">Specify hello path toohello local folder in which you downloaded hello script.</span></span> <span data-ttu-id="f42f6-182">tooimport hello modul, írja be:</span><span class="sxs-lookup"><span data-stu-id="f42f6-182">tooimport hello module, enter:</span></span>
   
    `Import-module <Path toohello folder that contains hello Windows PowerShell script>`
4. <span data-ttu-id="f42f6-183">Összes hello fájl *.aes* titkosított és tömörített fájlok.</span><span class="sxs-lookup"><span data-stu-id="f42f6-183">All hello files are *.aes* files that are compressed and encrypted.</span></span> <span data-ttu-id="f42f6-184">toodecompress és visszafejtése fájlokat, írja be:</span><span class="sxs-lookup"><span data-stu-id="f42f6-184">toodecompress and decrypt files, enter:</span></span>
   
    `Open-HcsSupportPackage <Path toohello folder that contains support package files>`
   
    <span data-ttu-id="f42f6-185">Vegye figyelembe, hogy hello tényleges fájlkiterjesztések jelennek meg az összes hello.</span><span class="sxs-lookup"><span data-stu-id="f42f6-185">Note that hello actual file extensions are now displayed for all hello files.</span></span>
   
    ![Támogatási csomag szerkesztése](./media/storsimple-create-manage-support-package/IC750706.png)
5. <span data-ttu-id="f42f6-187">Amikor hello titkosítási jelszót kéri, adja meg a hello hello támogatási csomag létrehozásakor használt jelszót.</span><span class="sxs-lookup"><span data-stu-id="f42f6-187">When you're prompted for hello encryption passphrase, enter hello passphrase that you used when hello support package was created.</span></span>
   
        cmdlet Open-HcsSupportPackage at command pipeline position 1
   
        Supply values for hello following parameters:EncryptionPassphrase: ****
6. <span data-ttu-id="f42f6-188">Keresse meg a hello naplófájlokat tartalmazó toohello mappát.</span><span class="sxs-lookup"><span data-stu-id="f42f6-188">Browse toohello folder that contains hello log files.</span></span> <span data-ttu-id="f42f6-189">Hello naplófájlok most kibontása és visszafejtése, mert ezek lesz az eredeti fájlkiterjesztés.</span><span class="sxs-lookup"><span data-stu-id="f42f6-189">Because hello log files are now decompressed and decrypted, these will have original file extensions.</span></span> <span data-ttu-id="f42f6-190">Módosítsa a fájlok tooremove bármely adatok, például a kötet neve és az eszköz IP-címek, és hello fájlokat menteni.</span><span class="sxs-lookup"><span data-stu-id="f42f6-190">Modify these files tooremove any customer-specific information, such as volume names and device IP addresses, and save hello files.</span></span>
7. <span data-ttu-id="f42f6-191">Bezárás hello fájlok toocompress őket a gzip és az AES-256 titkosítani.</span><span class="sxs-lookup"><span data-stu-id="f42f6-191">Close hello files toocompress them with gzip and encrypt them with AES-256.</span></span> <span data-ttu-id="f42f6-192">Ez a sebesség és a biztonság hello támogatási csomag átvitele a hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="f42f6-192">This is for speed and security in transferring hello support package over a network.</span></span> <span data-ttu-id="f42f6-193">toocompress és titkosíthatják a fájlokat, írja be a következő hello:</span><span class="sxs-lookup"><span data-stu-id="f42f6-193">toocompress and encrypt files, enter hello following:</span></span>
   
    `Close-HcsSupportPackage <Path toohello folder that contains support package files>`
   
    ![Támogatási csomag szerkesztése](./media/storsimple-create-manage-support-package/IC750707.png)
8. <span data-ttu-id="f42f6-195">Amikor a rendszer kéri, adja meg a titkosítás jelszavát az hello módosított támogatási csomag.</span><span class="sxs-lookup"><span data-stu-id="f42f6-195">When prompted, provide an encryption passphrase for hello modified support package.</span></span>
   
        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for hello following parameters:EncryptionPassphrase: ****
9. <span data-ttu-id="f42f6-196">Írja le hello új jelszót, úgy, hogy a Microsoft Support kérésekor megoszthatja azt.</span><span class="sxs-lookup"><span data-stu-id="f42f6-196">Write down hello new passphrase, so that you can share it with Microsoft Support when requested.</span></span>

### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a><span data-ttu-id="f42f6-197">Példa: Egy támogatási csomag jelszóval védett megosztott fájlok szerkesztése</span><span class="sxs-lookup"><span data-stu-id="f42f6-197">Example: Editing files in a support package on a password-protected share</span></span>
<span data-ttu-id="f42f6-198">hello a következő példa bemutatja, hogyan toodecrypt, szerkesztheti, és egy támogatási csomag újbóli titkosítására.</span><span class="sxs-lookup"><span data-stu-id="f42f6-198">hello following example shows how toodecrypt, edit, and re-encrypt a support package.</span></span>

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1

        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for hello following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Close-HcsSupportPackage at command pipeline position 1

        Supply values for hello following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32>

## <a name="next-steps"></a><span data-ttu-id="f42f6-199">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f42f6-199">Next steps</span></span>
* <span data-ttu-id="f42f6-200">Ismerje meg, hogyan túl[használata támogatási csomag és az eszköz naplói tootroubleshoot az eszköz telepítési](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="f42f6-200">Learn how too[use support packages and device logs tootroubleshoot your device deployment](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).</span></span>
* <span data-ttu-id="f42f6-201">Ismerje meg, hogyan túl[használata hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="f42f6-201">Learn how too[use hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

