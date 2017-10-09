---
title: "aaaCreate a StorSimple 8000 series támogatási csomag |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate, visszafejtése, és a StorSimple 8000 series eszköz támogatási csomag szerkesztése."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/05/2017
ms.author: alkohli
ms.openlocfilehash: 857555b6ba31b1527f8f00d19818ebbec6005d0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-a-support-package-for-storsimple-8000-series"></a><span data-ttu-id="90db7-103">Hozzon létre és kezelheti a StorSimple 8000 Series támogatási csomag</span><span class="sxs-lookup"><span data-stu-id="90db7-103">Create and manage a support package for StorSimple 8000 series</span></span>

## <a name="overview"></a><span data-ttu-id="90db7-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="90db7-104">Overview</span></span>

<span data-ttu-id="90db7-105">A StorSimple támogatási csomag olyan könnyen használható mechanizmus, amely az összes releváns naplók tooassist Microsoft Support gyűjti a hibaelhárításban a StorSimple eszköz probléma merül fel.</span><span class="sxs-lookup"><span data-stu-id="90db7-105">A StorSimple support package is an easy-to-use mechanism that collects all relevant logs tooassist Microsoft Support with troubleshooting any StorSimple device issues.</span></span> <span data-ttu-id="90db7-106">hello gyűjtött naplók titkosított és tömörített.</span><span class="sxs-lookup"><span data-stu-id="90db7-106">hello collected logs are encrypted and compressed.</span></span>

<span data-ttu-id="90db7-107">Ez az oktatóanyag részletesen toocreate tartalmazza, és a StorSimple 8000 series eszköz hello támogatási csomag kezelése.</span><span class="sxs-lookup"><span data-stu-id="90db7-107">This tutorial includes step-by-step instructions toocreate and manage hello support package for your StorSimple 8000 series device.</span></span> <span data-ttu-id="90db7-108">Ha egy StorSimple virtuális tömbbel rendelkező dolgozik, nyissa meg túl[a napló csomagot](storsimple-ova-web-ui-admin.md#generate-a-log-package).</span><span class="sxs-lookup"><span data-stu-id="90db7-108">If you are working with a StorSimple Virtual Array, go too[generate a log package](storsimple-ova-web-ui-admin.md#generate-a-log-package).</span></span>

## <a name="create-a-support-package"></a><span data-ttu-id="90db7-109">Hozzon létre egy támogatási csomag</span><span class="sxs-lookup"><span data-stu-id="90db7-109">Create a support package</span></span>

<span data-ttu-id="90db7-110">Néhány esetben szüksége lesz toomanually StorSimple hello támogatási csomag a Windows PowerShell segítségével hozzon létre.</span><span class="sxs-lookup"><span data-stu-id="90db7-110">In some cases, you'll need toomanually create hello support package through Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="90db7-111">Példa:</span><span class="sxs-lookup"><span data-stu-id="90db7-111">For example:</span></span>

* <span data-ttu-id="90db7-112">Ha a naplóból tooremove bizalmas adatokat kell fájlok Microsoft Support előzetes toosharing.</span><span class="sxs-lookup"><span data-stu-id="90db7-112">If you need tooremove sensitive information from your log files prior toosharing with Microsoft Support.</span></span>
* <span data-ttu-id="90db7-113">Ha a probléma miatt tooconnectivity problémák hello csomag feltöltése.</span><span class="sxs-lookup"><span data-stu-id="90db7-113">If you are having difficulty uploading hello package due tooconnectivity issues.</span></span>

<span data-ttu-id="90db7-114">A manuálisan létrehozott támogatási csomag Microsoft Support keresztül e-mailek megoszthatja.</span><span class="sxs-lookup"><span data-stu-id="90db7-114">You can share your manually generated support package with Microsoft Support over email.</span></span> <span data-ttu-id="90db7-115">Hajtsa végre a következő lépéseket toocreate egy támogatási csomag a Windows PowerShellben a StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="90db7-115">Perform hello following steps toocreate a support package in Windows PowerShell for StorSimple.</span></span>

#### <a name="toocreate-a-support-package-in-windows-powershell-for-storsimple"></a><span data-ttu-id="90db7-116">toocreate egy támogatási csomag a Windows PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="90db7-116">toocreate a support package in Windows PowerShell for StorSimple</span></span>

1. <span data-ttu-id="90db7-117">Windows PowerShell-munkamenet hello távoli számítógépen tooconnect tooyour StorSimple eszköz által használt rendszergazdaként toostart adja meg a hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="90db7-117">toostart a Windows PowerShell session as an administrator on hello remote computer that's used tooconnect tooyour StorSimple device, enter hello following command:</span></span>
   
    `Start PowerShell`
2. <span data-ttu-id="90db7-118">A Windows PowerShell-munkamenetben hello csatlakozás toohello SSAdmin konzol, az eszköz:</span><span class="sxs-lookup"><span data-stu-id="90db7-118">In hello Windows PowerShell session, connect toohello SSAdmin Console of your device:</span></span>
   
   1. <span data-ttu-id="90db7-119">Hello parancssorba írja be:</span><span class="sxs-lookup"><span data-stu-id="90db7-119">At hello command prompt, enter:</span></span>
     
       `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`
   2. <span data-ttu-id="90db7-120">Hello párbeszédpanel adja meg az eszköz rendszergazdai jelszava.</span><span class="sxs-lookup"><span data-stu-id="90db7-120">In hello dialog box that opens, enter your device administrator password.</span></span> <span data-ttu-id="90db7-121">hello alapértelmezett jelszó _jelszó1_.</span><span class="sxs-lookup"><span data-stu-id="90db7-121">hello default password is _Password1_.</span></span>
     
      ![PowerShell-hitelesítő adat párbeszédpanel](./media/storsimple-8000-create-manage-support-package/IC740962.png)
   3. <span data-ttu-id="90db7-123">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="90db7-123">Select **OK**.</span></span>
   4. <span data-ttu-id="90db7-124">Hello parancssorba írja be:</span><span class="sxs-lookup"><span data-stu-id="90db7-124">At hello command prompt, enter:</span></span>
     
      `Enter-PSSession $MS`
3. <span data-ttu-id="90db7-125">A megnyitott munkamenetben hello írja be a hello megfelelő parancsot.</span><span class="sxs-lookup"><span data-stu-id="90db7-125">In hello session that opens, enter hello appropriate command.</span></span>
   
   * <span data-ttu-id="90db7-126">Jelszóval védett hálózati megosztások adja meg:</span><span class="sxs-lookup"><span data-stu-id="90db7-126">For network shares that are password protected, enter:</span></span>
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`
     
       <span data-ttu-id="90db7-127">Kéri a jelszót, a elérési toohello hálózati megosztott mappából és a titkosítás jelszavát (mert hello támogatási csomag titkosított).</span><span class="sxs-lookup"><span data-stu-id="90db7-127">You'll be prompted for a password, a path toohello network shared folder, and an encryption passphrase (because hello support package is encrypted).</span></span> <span data-ttu-id="90db7-128">Egy támogatási csomag majd hello megadott mappában jön létre.</span><span class="sxs-lookup"><span data-stu-id="90db7-128">A support package is then created in hello specified folder.</span></span>
   * <span data-ttu-id="90db7-129">Az megosztások, amelyek nem jelszóval védett, nem kell hello `-Credential` paraméter.</span><span class="sxs-lookup"><span data-stu-id="90db7-129">For shares that are not password protected, you do not need hello `-Credential` parameter.</span></span> <span data-ttu-id="90db7-130">Írja be a következő hello:</span><span class="sxs-lookup"><span data-stu-id="90db7-130">Enter hello following:</span></span>
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`
     
       <span data-ttu-id="90db7-131">hello támogatási csomag mindkét tartományvezérlők hello megadott hálózati megosztott mappában jön létre.</span><span class="sxs-lookup"><span data-stu-id="90db7-131">hello support package is created for both controllers in hello specified network shared folder.</span></span> <span data-ttu-id="90db7-132">Egy titkosított, tömörített fájlt, amely elküldhető tooMicrosoft támogatási hibaelhárítási.</span><span class="sxs-lookup"><span data-stu-id="90db7-132">It's an encrypted, compressed file that can be sent tooMicrosoft Support for troubleshooting.</span></span> <span data-ttu-id="90db7-133">További információkért lásd: [forduljon a Microsoft ügyfélszolgálatához](storsimple-8000-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="90db7-133">For more information, see [Contact Microsoft Support](storsimple-8000-contact-microsoft-support.md).</span></span>

### <a name="hello-export-hcssupportpackage-cmdlet-parameters"></a><span data-ttu-id="90db7-134">Export-HcsSupportPackage hello parancsmag-paraméterek</span><span class="sxs-lookup"><span data-stu-id="90db7-134">hello Export-HcsSupportPackage cmdlet parameters</span></span>

<span data-ttu-id="90db7-135">A következő paraméterek hello Export-HcsSupportPackage parancsmaggal hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="90db7-135">You can use hello following parameters with hello Export-HcsSupportPackage cmdlet.</span></span>

| <span data-ttu-id="90db7-136">Paraméter</span><span class="sxs-lookup"><span data-stu-id="90db7-136">Parameter</span></span> | <span data-ttu-id="90db7-137">Kötelező/választható</span><span class="sxs-lookup"><span data-stu-id="90db7-137">Required/Optional</span></span> | <span data-ttu-id="90db7-138">Leírás</span><span class="sxs-lookup"><span data-stu-id="90db7-138">Description</span></span> |
| --- | --- | --- |
| `-Path` |<span data-ttu-id="90db7-139">Szükséges</span><span class="sxs-lookup"><span data-stu-id="90db7-139">Required</span></span> |<span data-ttu-id="90db7-140">Hello hálózati megosztott mappa mely hello támogatási csomag helyet adó tooprovide hello helyét használja.</span><span class="sxs-lookup"><span data-stu-id="90db7-140">Use tooprovide hello location of hello network shared folder in which hello support package is placed.</span></span> |
| `-EncryptionPassphrase` |<span data-ttu-id="90db7-141">Szükséges</span><span class="sxs-lookup"><span data-stu-id="90db7-141">Required</span></span> |<span data-ttu-id="90db7-142">Használjon tooprovide egy hozzáférési kódot toohelp hello támogatási csomag titkosításához.</span><span class="sxs-lookup"><span data-stu-id="90db7-142">Use tooprovide a passphrase toohelp encrypt hello support package.</span></span> |
| `-Credential` |<span data-ttu-id="90db7-143">Optional</span><span class="sxs-lookup"><span data-stu-id="90db7-143">Optional</span></span> |<span data-ttu-id="90db7-144">Hálózati megosztott mappa hello toosupply hozzáférési hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="90db7-144">Use toosupply access credentials for hello network shared folder.</span></span> |
| `-Force` |<span data-ttu-id="90db7-145">Optional</span><span class="sxs-lookup"><span data-stu-id="90db7-145">Optional</span></span> |<span data-ttu-id="90db7-146">Tooskip hello titkosítási jelszót megerősítési lépés használható.</span><span class="sxs-lookup"><span data-stu-id="90db7-146">Use tooskip hello encryption passphrase confirmation step.</span></span> |
| `-PackageTag` |<span data-ttu-id="90db7-147">Optional</span><span class="sxs-lookup"><span data-stu-id="90db7-147">Optional</span></span> |<span data-ttu-id="90db7-148">Használjon toospecify olyan könyvtárat *elérési* mely hello támogatott csomag kerül.</span><span class="sxs-lookup"><span data-stu-id="90db7-148">Use toospecify a directory under *Path* in which hello support package is placed.</span></span> <span data-ttu-id="90db7-149">hello alapértelmezett érték a [eszköznév]-[aktuális dátumot és time:yyyy-MM-dd-HH-mm-ss].</span><span class="sxs-lookup"><span data-stu-id="90db7-149">hello default is [device name]-[current date and time:yyyy-MM-dd-HH-mm-ss].</span></span> |
| `-Scope` |<span data-ttu-id="90db7-150">Optional</span><span class="sxs-lookup"><span data-stu-id="90db7-150">Optional</span></span> |<span data-ttu-id="90db7-151">Adja meg a **fürt** (alapértelmezett) toocreate egy támogatási csomag mindkét vezérlők.</span><span class="sxs-lookup"><span data-stu-id="90db7-151">Specify as **Cluster** (default) toocreate a support package for both controllers.</span></span> <span data-ttu-id="90db7-152">Ha azt szeretné, csak az aktuális vezérlő hello toocreate egy csomagot, adjon meg **vezérlő**.</span><span class="sxs-lookup"><span data-stu-id="90db7-152">If you want toocreate a package only for hello current controller, specify **Controller**.</span></span> |

## <a name="edit-a-support-package"></a><span data-ttu-id="90db7-153">Egy támogatási csomag szerkesztése</span><span class="sxs-lookup"><span data-stu-id="90db7-153">Edit a support package</span></span>

<span data-ttu-id="90db7-154">Miután létrehozta a támogatási csomag, szükség lehet a tooedit hello csomag tooremove bizalmas adatokat.</span><span class="sxs-lookup"><span data-stu-id="90db7-154">After you have generated a support package, you might need tooedit hello package tooremove sensitive information.</span></span> <span data-ttu-id="90db7-155">Ilyen lehet például a kötet, eszköz IP-címek vagy hello naplófájlokból biztonsági másolat neve.</span><span class="sxs-lookup"><span data-stu-id="90db7-155">This can include volume names, device IP addresses, and backup names from hello log files.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="90db7-156">Csak akkor szerkeszthető egy támogatási csomag, amelyik a Windows PowerShell segítségével a StorSimple jött létre.</span><span class="sxs-lookup"><span data-stu-id="90db7-156">You can only edit a support package that was generated through Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="90db7-157">Hello StorSimple Device Manager szolgáltatás Azure-portálon létrehozott csomagot nem szerkeszthetők.</span><span class="sxs-lookup"><span data-stu-id="90db7-157">You can't edit a package created in hello Azure portal with StorSimple Device Manager service.</span></span>

<span data-ttu-id="90db7-158">tooedit egy támogatási csomag hello Microsoft Support webhelyén, a feltöltés előtt először visszafejtéséhez hello támogatási csomag hello fájlok szerkesztése és újbóli titkosítására a azt.</span><span class="sxs-lookup"><span data-stu-id="90db7-158">tooedit a support package before uploading it on hello Microsoft Support site, first decrypt hello support package, edit hello files, and then re-encrypt it.</span></span> <span data-ttu-id="90db7-159">Hajtsa végre a következő lépéseket hello.</span><span class="sxs-lookup"><span data-stu-id="90db7-159">Perform hello following steps.</span></span>

#### <a name="tooedit-a-support-package-in-windows-powershell-for-storsimple"></a><span data-ttu-id="90db7-160">tooedit egy támogatási csomag a Windows PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="90db7-160">tooedit a support package in Windows PowerShell for StorSimple</span></span>

1. <span data-ttu-id="90db7-161">Egy támogatási csomagot leírtak korábban [toocreate egy támogatási csomag a Windows PowerShell-lel](#to-create-a-support-package-in-windows-powershell-for-storsimple).</span><span class="sxs-lookup"><span data-stu-id="90db7-161">Generate a support package as described earlier, in [toocreate a support package in Windows PowerShell for StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).</span></span>
2. <span data-ttu-id="90db7-162">[Töltse le a hello parancsfájl](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) helyileg az ügyfélen.</span><span class="sxs-lookup"><span data-stu-id="90db7-162">[Download hello script](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) locally on your client.</span></span>
3. <span data-ttu-id="90db7-163">Hello Windows PowerShell-modul importálásához.</span><span class="sxs-lookup"><span data-stu-id="90db7-163">Import hello Windows PowerShell module.</span></span> <span data-ttu-id="90db7-164">Adja meg a hello elérési toohello helyi mappát, amelyben hello parancsprogram letöltése.</span><span class="sxs-lookup"><span data-stu-id="90db7-164">Specify hello path toohello local folder in which you downloaded hello script.</span></span> <span data-ttu-id="90db7-165">tooimport hello modul, írja be:</span><span class="sxs-lookup"><span data-stu-id="90db7-165">tooimport hello module, enter:</span></span>
   
    `Import-module <Path toohello folder that contains hello Windows PowerShell script>`
4. <span data-ttu-id="90db7-166">Összes hello fájl *.aes* titkosított és tömörített fájlok.</span><span class="sxs-lookup"><span data-stu-id="90db7-166">All hello files are *.aes* files that are compressed and encrypted.</span></span> <span data-ttu-id="90db7-167">toodecompress és visszafejtése fájlokat, írja be:</span><span class="sxs-lookup"><span data-stu-id="90db7-167">toodecompress and decrypt files, enter:</span></span>
   
    `Open-HcsSupportPackage <Path toohello folder that contains support package files>`
   
    <span data-ttu-id="90db7-168">Vegye figyelembe, hogy hello tényleges fájlkiterjesztések jelennek meg az összes hello.</span><span class="sxs-lookup"><span data-stu-id="90db7-168">Note that hello actual file extensions are now displayed for all hello files.</span></span>
   
    ![Támogatási csomag szerkesztése](./media/storsimple-8000-create-manage-support-package/IC750706.png)
5. <span data-ttu-id="90db7-170">Amikor hello titkosítási jelszót kéri, adja meg a hello hello támogatási csomag létrehozásakor használt jelszót.</span><span class="sxs-lookup"><span data-stu-id="90db7-170">When you're prompted for hello encryption passphrase, enter hello passphrase that you used when hello support package was created.</span></span>
   
        cmdlet Open-HcsSupportPackage at command pipeline position 1
   
        Supply values for hello following parameters:EncryptionPassphrase: ****
6. <span data-ttu-id="90db7-171">Keresse meg a hello naplófájlokat tartalmazó toohello mappát.</span><span class="sxs-lookup"><span data-stu-id="90db7-171">Browse toohello folder that contains hello log files.</span></span> <span data-ttu-id="90db7-172">Hello naplófájlok most kibontása és visszafejtése, mert ezek lesz az eredeti fájlkiterjesztés.</span><span class="sxs-lookup"><span data-stu-id="90db7-172">Because hello log files are now decompressed and decrypted, these will have original file extensions.</span></span> <span data-ttu-id="90db7-173">Módosítsa a fájlok tooremove bármely adatok, például a kötet neve és az eszköz IP-címek, és hello fájlokat menteni.</span><span class="sxs-lookup"><span data-stu-id="90db7-173">Modify these files tooremove any customer-specific information, such as volume names and device IP addresses, and save hello files.</span></span>
7. <span data-ttu-id="90db7-174">Bezárás hello fájlok toocompress őket a gzip és az AES-256 titkosítani.</span><span class="sxs-lookup"><span data-stu-id="90db7-174">Close hello files toocompress them with gzip and encrypt them with AES-256.</span></span> <span data-ttu-id="90db7-175">Ez a sebesség és a biztonság hello támogatási csomag átvitele a hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="90db7-175">This is for speed and security in transferring hello support package over a network.</span></span> <span data-ttu-id="90db7-176">toocompress és titkosíthatják a fájlokat, írja be a következő hello:</span><span class="sxs-lookup"><span data-stu-id="90db7-176">toocompress and encrypt files, enter hello following:</span></span>
   
    `Close-HcsSupportPackage <Path toohello folder that contains support package files>`
   
    ![Támogatási csomag szerkesztése](./media/storsimple-8000-create-manage-support-package/IC750707.png)
8. <span data-ttu-id="90db7-178">Amikor a rendszer kéri, adja meg a titkosítás jelszavát az hello módosított támogatási csomag.</span><span class="sxs-lookup"><span data-stu-id="90db7-178">When prompted, provide an encryption passphrase for hello modified support package.</span></span>
   
        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for hello following parameters:EncryptionPassphrase: ****
9. <span data-ttu-id="90db7-179">Írja le hello új jelszót, úgy, hogy a Microsoft Support kérésekor megoszthatja azt.</span><span class="sxs-lookup"><span data-stu-id="90db7-179">Write down hello new passphrase, so that you can share it with Microsoft Support when requested.</span></span>

### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a><span data-ttu-id="90db7-180">Példa: Egy támogatási csomag jelszóval védett megosztott fájlok szerkesztése</span><span class="sxs-lookup"><span data-stu-id="90db7-180">Example: Editing files in a support package on a password-protected share</span></span>

<span data-ttu-id="90db7-181">hello a következő példa bemutatja, hogyan toodecrypt, szerkesztheti, és egy támogatási csomag újbóli titkosítására.</span><span class="sxs-lookup"><span data-stu-id="90db7-181">hello following example shows how toodecrypt, edit, and re-encrypt a support package.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="90db7-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="90db7-182">Next steps</span></span>

* <span data-ttu-id="90db7-183">További tudnivalók: hello [hello támogatási csomag gyűjtött információk](https://support.microsoft.com/help/3193606/storsimple-support-packages-and-device-logs)</span><span class="sxs-lookup"><span data-stu-id="90db7-183">Learn about hello [information collected in hello Support package](https://support.microsoft.com/help/3193606/storsimple-support-packages-and-device-logs)</span></span>
* <span data-ttu-id="90db7-184">Ismerje meg, hogyan túl[használata támogatási csomag és az eszköz naplói tootroubleshoot az eszköz telepítési](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="90db7-184">Learn how too[use support packages and device logs tootroubleshoot your device deployment](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).</span></span>
* <span data-ttu-id="90db7-185">Ismerje meg, hogyan túl[használata hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="90db7-185">Learn how too[use hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

