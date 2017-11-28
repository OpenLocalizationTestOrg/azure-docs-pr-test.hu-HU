---
title: "Hozzon létre egy StorSimple támogatási csomagot |} Microsoft Docs"
description: "Megtudhatja, hogyan létrehozása és szerkesztése a StorSimple eszköz támogatási csomag visszafejtésére."
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
ms.openlocfilehash: 32d20e7a8adcfc646c592213fe7395b87a93c985
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-a-storsimple-support-package"></a><span data-ttu-id="2800d-103">Létrehozását és kezelését egy StorSimple támogatási csomag</span><span class="sxs-lookup"><span data-stu-id="2800d-103">Create and manage a StorSimple support package</span></span>
## <a name="overview"></a><span data-ttu-id="2800d-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="2800d-104">Overview</span></span>
<span data-ttu-id="2800d-105">StorSimple támogatási csomag olyan könnyen használható mechanizmus, amely gyűjti az összes vonatkozó naplókat, a StorSimple eszköz problémák elhárítása a Microsoft Support segítésére.</span><span class="sxs-lookup"><span data-stu-id="2800d-105">A StorSimple support package is an easy-to-use mechanism that collects all relevant logs to assist Microsoft Support with troubleshooting any StorSimple device issues.</span></span> <span data-ttu-id="2800d-106">A gyűjtött naplók titkosított és tömörített.</span><span class="sxs-lookup"><span data-stu-id="2800d-106">The collected logs are encrypted and compressed.</span></span>

<span data-ttu-id="2800d-107">Ez az oktatóanyag létrehozásához és kezeléséhez a támogatási csomag részletes útmutatást tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2800d-107">This tutorial includes step-by-step instructions to create and manage the support package.</span></span>

## <a name="create-and-upload-a-support-package-in-the-azure-classic-portal"></a><span data-ttu-id="2800d-108">Hozzon létre, és töltse fel a klasszikus Azure portálon támogatási csomag</span><span class="sxs-lookup"><span data-stu-id="2800d-108">Create and upload a support package in the Azure classic portal</span></span>
<span data-ttu-id="2800d-109">Hozzon létre, és töltse fel egy támogatási csomag a Microsoft Support webhelyén keresztül a **karbantartási** a szolgáltatást a klasszikus Azure portálra a lap.</span><span class="sxs-lookup"><span data-stu-id="2800d-109">You can create and upload a support package to the Microsoft Support site through the **Maintenance** page of the service in the Azure classic portal.</span></span>

> [!NOTE]
> <span data-ttu-id="2800d-110">A feltöltés támogatási hitelesítő kulcs szükséges.</span><span class="sxs-lookup"><span data-stu-id="2800d-110">The upload requires a support passkey.</span></span> <span data-ttu-id="2800d-111">A támogatási szakértőhöz biztosítania kell a Önnek e-mailben.</span><span class="sxs-lookup"><span data-stu-id="2800d-111">Your support engineer should provide this to you in an email.</span></span>
> 
> 

<span data-ttu-id="2800d-112">Egy titkosított és tömörített támogatási csomagot (.cab fájl) létrehozása és a támogatási webhely feltöltött.</span><span class="sxs-lookup"><span data-stu-id="2800d-112">An encrypted and compressed support package (.cab file) is created and uploaded to the Support site.</span></span> <span data-ttu-id="2800d-113">A támogatási szakember majd beolvasni a csomag a támogatási webhelyről a hiba elhárításához.</span><span class="sxs-lookup"><span data-stu-id="2800d-113">The support engineer can then retrieve this package from the Support site for troubleshooting the issue.</span></span>

<span data-ttu-id="2800d-114">Hajtsa végre a következő lépéseket a klasszikus portálon hozzon létre egy támogatási csomagot.</span><span class="sxs-lookup"><span data-stu-id="2800d-114">Perform the following steps in the classic portal to create a support package.</span></span>

#### <a name="to-create-a-support-package-in-the-azure-classic-portal"></a><span data-ttu-id="2800d-115">Támogatási csomag létrehozása a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="2800d-115">To create a support package in the Azure classic portal</span></span>
1. <span data-ttu-id="2800d-116">Válassza ki **eszközök** > **karbantartási**.</span><span class="sxs-lookup"><span data-stu-id="2800d-116">Select **Devices** > **Maintenance**.</span></span>
2. <span data-ttu-id="2800d-117">Az a **támogatási csomag** szakaszban jelölje be **létrehozása és feltöltése támogatási csomag**.</span><span class="sxs-lookup"><span data-stu-id="2800d-117">In the **Support package** section, select **Create and upload support package**.</span></span>
3. <span data-ttu-id="2800d-118">Az a **létrehozása és feltöltése támogatási csomag** párbeszédpanelen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="2800d-118">In the **Create and upload support package** dialog box, do the following:</span></span>
   
    ![Támogatási csomag létrehozása](./media/storsimple-create-manage-support-package/IC740923.png)
   
   * <span data-ttu-id="2800d-120">Az a **támogatási hozzáférési kulcs** szöveg mezőbe írja be a hozzáférési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="2800d-120">In the **Support Passkey** text box, enter the passkey.</span></span> <span data-ttu-id="2800d-121">A Microsoft támogatási szakértőhöz küldjön-e a hitelesítő kulcs Önnek e-mailben.</span><span class="sxs-lookup"><span data-stu-id="2800d-121">Your Microsoft support engineer should send this passkey to you in email.</span></span>
   * <span data-ttu-id="2800d-122">Jelölje be a jelölőnégyzetet a hozzájárulásukat adják meg a támogatási csomag automatikusan feltölteni a Microsoft Support webhelyén.</span><span class="sxs-lookup"><span data-stu-id="2800d-122">Select the check box to provide consent to automatically upload the support package to the Microsoft Support site.</span></span>
   * <span data-ttu-id="2800d-123">Kattintson a pipa ikonra</span><span class="sxs-lookup"><span data-stu-id="2800d-123">Click the check icon</span></span> ![Pipa ikon](./media/storsimple-create-manage-support-package/IC740895.png)<span data-ttu-id="2800d-125">.</span><span class="sxs-lookup"><span data-stu-id="2800d-125">.</span></span>

## <a name="manually-create-a-support-package"></a><span data-ttu-id="2800d-126">Manuálisan hozzon létre egy támogatási csomag</span><span class="sxs-lookup"><span data-stu-id="2800d-126">Manually create a support package</span></span>
<span data-ttu-id="2800d-127">Bizonyos esetekben lesz szüksége a StorSimple a támogatási csomag a Windows PowerShell segítségével manuálisan létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="2800d-127">In some cases, you'll need to manually create the support package through Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="2800d-128">Példa:</span><span class="sxs-lookup"><span data-stu-id="2800d-128">For example:</span></span>

* <span data-ttu-id="2800d-129">Ha el kell távolítania a bizalmas adatokat a naplófájlokból Microsoft Support megosztása előtt.</span><span class="sxs-lookup"><span data-stu-id="2800d-129">If you need to remove sensitive information from your log files prior to sharing with Microsoft Support.</span></span>
* <span data-ttu-id="2800d-130">Ha a csatlakozási problémák miatt a csomag feltöltése.</span><span class="sxs-lookup"><span data-stu-id="2800d-130">If you are having difficulty uploading the package due to connectivity issues.</span></span>

<span data-ttu-id="2800d-131">A manuálisan létrehozott támogatási csomag Microsoft Support keresztül e-mailek megoszthatja.</span><span class="sxs-lookup"><span data-stu-id="2800d-131">You can share your manually generated support package with Microsoft Support over email.</span></span> <span data-ttu-id="2800d-132">A következő lépésekkel hozzon létre egy támogatási csomag a Windows PowerShell StorSimple.</span><span class="sxs-lookup"><span data-stu-id="2800d-132">Perform the following steps to create a support package in Windows PowerShell for StorSimple.</span></span>

#### <a name="to-create-a-support-package-in-windows-powershell-for-storsimple"></a><span data-ttu-id="2800d-133">Egy támogatási csomag létrehozása a Windows PowerShell StorSimple</span><span class="sxs-lookup"><span data-stu-id="2800d-133">To create a support package in Windows PowerShell for StorSimple</span></span>
1. <span data-ttu-id="2800d-134">Indítsa el a Windows PowerShell-munkamenetet a távoli számítógépen a StorSimple eszköz való kapcsolódáshoz használt rendszergazdaként, írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="2800d-134">To start a Windows PowerShell session as an administrator on the remote computer that's used to connect to your StorSimple device, enter the following command:</span></span>
   
    `Start PowerShell`
2. <span data-ttu-id="2800d-135">A Windows PowerShell-munkamenetben kapcsolódjon a SSAdmin konzol, az eszköz:</span><span class="sxs-lookup"><span data-stu-id="2800d-135">In the Windows PowerShell session, connect to the SSAdmin Console of your device:</span></span>
   
   * <span data-ttu-id="2800d-136">A parancssorba írja be:</span><span class="sxs-lookup"><span data-stu-id="2800d-136">At the command prompt, enter:</span></span>
     
       `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`
   * <span data-ttu-id="2800d-137">A megnyíló párbeszédpanelen írja be az eszköz rendszergazdai jelszava.</span><span class="sxs-lookup"><span data-stu-id="2800d-137">In the dialog box that opens, enter your device administrator password.</span></span> <span data-ttu-id="2800d-138">Az alapértelmezett jelszava:</span><span class="sxs-lookup"><span data-stu-id="2800d-138">The default password is:</span></span>
     
      `Password1`
     
      ![PowerShell-hitelesítő adat párbeszédpanel](./media/storsimple-create-manage-support-package/IC740962.png)
   * <span data-ttu-id="2800d-140">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="2800d-140">Select **OK**.</span></span>
   * <span data-ttu-id="2800d-141">A parancssorba írja be:</span><span class="sxs-lookup"><span data-stu-id="2800d-141">At the command prompt, enter:</span></span>
     
      `Enter-PSSession $MS`
3. <span data-ttu-id="2800d-142">A megnyitott munkamenetben adja meg a megfelelő parancsot.</span><span class="sxs-lookup"><span data-stu-id="2800d-142">In the session that opens, enter the appropriate command.</span></span>
   
   * <span data-ttu-id="2800d-143">Jelszóval védett hálózati megosztások adja meg:</span><span class="sxs-lookup"><span data-stu-id="2800d-143">For network shares that are password protected, enter:</span></span>
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`
     
       <span data-ttu-id="2800d-144">Kéri a jelszót, a hálózati megosztott mappa, és a titkosítás jelszavát elérési útját (mert a támogatási csomag titkosított).</span><span class="sxs-lookup"><span data-stu-id="2800d-144">You'll be prompted for a password, a path to the network shared folder, and an encryption passphrase (because the support package is encrypted).</span></span> <span data-ttu-id="2800d-145">Egy támogatási csomag a megadott mappában létrejön.</span><span class="sxs-lookup"><span data-stu-id="2800d-145">A support package is then created in the specified folder.</span></span>
   * <span data-ttu-id="2800d-146">Az megosztások, amelyek nem jelszóval védett, nem kell a `-Credential` paraméter.</span><span class="sxs-lookup"><span data-stu-id="2800d-146">For shares that are not password protected, you do not need the `-Credential` parameter.</span></span> <span data-ttu-id="2800d-147">Írja be a következőket:</span><span class="sxs-lookup"><span data-stu-id="2800d-147">Enter the following:</span></span>
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`
     
       <span data-ttu-id="2800d-148">A támogatási csomag mindkét vezérlők, a megadott hálózati megosztott mappában jön létre.</span><span class="sxs-lookup"><span data-stu-id="2800d-148">The support package is created for both controllers in the specified network shared folder.</span></span> <span data-ttu-id="2800d-149">Egy titkosított, tömörített fájlt, amely a Microsoft Support hibaelhárítási elküldhető.</span><span class="sxs-lookup"><span data-stu-id="2800d-149">It's an encrypted, compressed file that can be sent to Microsoft Support for troubleshooting.</span></span> <span data-ttu-id="2800d-150">További információkért lásd: [forduljon a Microsoft ügyfélszolgálatához](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="2800d-150">For more information, see [Contact Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>

### <a name="the-export-hcssupportpackage-cmdlet-parameters"></a><span data-ttu-id="2800d-151">Az Export-HcsSupportPackage parancsmag-paraméterek</span><span class="sxs-lookup"><span data-stu-id="2800d-151">The Export-HcsSupportPackage cmdlet parameters</span></span>
<span data-ttu-id="2800d-152">A következő paraméterek az Export-HcsSupportPackage parancsmag használható.</span><span class="sxs-lookup"><span data-stu-id="2800d-152">You can use the following parameters with the Export-HcsSupportPackage cmdlet.</span></span>

| <span data-ttu-id="2800d-153">Paraméter</span><span class="sxs-lookup"><span data-stu-id="2800d-153">Parameter</span></span> | <span data-ttu-id="2800d-154">Kötelező/választható</span><span class="sxs-lookup"><span data-stu-id="2800d-154">Required/Optional</span></span> | <span data-ttu-id="2800d-155">Leírás</span><span class="sxs-lookup"><span data-stu-id="2800d-155">Description</span></span> |
| --- | --- | --- |
| `-Path` |<span data-ttu-id="2800d-156">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2800d-156">Required</span></span> |<span data-ttu-id="2800d-157">Adja meg a helyét, a hálózati megosztott mappa, amelyben a támogatási csomag kerül segítségével.</span><span class="sxs-lookup"><span data-stu-id="2800d-157">Use to provide the location of the network shared folder in which the support package is placed.</span></span> |
| `-EncryptionPassphrase` |<span data-ttu-id="2800d-158">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2800d-158">Required</span></span> |<span data-ttu-id="2800d-159">Használja a jelszó segítségével titkosítja a támogatási csomag szükséges.</span><span class="sxs-lookup"><span data-stu-id="2800d-159">Use to provide a passphrase to help encrypt the support package.</span></span> |
| `-Credential` |<span data-ttu-id="2800d-160">Optional</span><span class="sxs-lookup"><span data-stu-id="2800d-160">Optional</span></span> |<span data-ttu-id="2800d-161">Használja a hálózati megosztott mappa, a hozzáférési hitelesítő adatok megadását.</span><span class="sxs-lookup"><span data-stu-id="2800d-161">Use to supply access credentials for the network shared folder.</span></span> |
| `-Force` |<span data-ttu-id="2800d-162">Optional</span><span class="sxs-lookup"><span data-stu-id="2800d-162">Optional</span></span> |<span data-ttu-id="2800d-163">Használja a titkosítási jelszót megerősítési lépés kihagyásához.</span><span class="sxs-lookup"><span data-stu-id="2800d-163">Use to skip the encryption passphrase confirmation step.</span></span> |
| `-PackageTag` |<span data-ttu-id="2800d-164">Optional</span><span class="sxs-lookup"><span data-stu-id="2800d-164">Optional</span></span> |<span data-ttu-id="2800d-165">Ezzel adhatja meg a könyvtárat *elérési* , amely a támogatási csomag el van helyezve a.</span><span class="sxs-lookup"><span data-stu-id="2800d-165">Use to specify a directory under *Path* in which the support package is placed.</span></span> <span data-ttu-id="2800d-166">Az alapértelmezett érték a [eszköznév]-[aktuális dátumot és time:yyyy-MM-dd-HH-mm-ss].</span><span class="sxs-lookup"><span data-stu-id="2800d-166">The default is [device name]-[current date and time:yyyy-MM-dd-HH-mm-ss].</span></span> |
| `-Scope` |<span data-ttu-id="2800d-167">Optional</span><span class="sxs-lookup"><span data-stu-id="2800d-167">Optional</span></span> |<span data-ttu-id="2800d-168">Adja meg a **fürt** (alapértelmezett) hozzon létre egy támogatási csomag mindkét vezérlők.</span><span class="sxs-lookup"><span data-stu-id="2800d-168">Specify as **Cluster** (default) to create a support package for both controllers.</span></span> <span data-ttu-id="2800d-169">Ha azt szeretné, csak az aktuális tartományvezérlő a csomag létrehozásához, adja meg a **vezérlő**.</span><span class="sxs-lookup"><span data-stu-id="2800d-169">If you want to create a package only for the current controller, specify **Controller**.</span></span> |

## <a name="edit-a-support-package"></a><span data-ttu-id="2800d-170">Egy támogatási csomag szerkesztése</span><span class="sxs-lookup"><span data-stu-id="2800d-170">Edit a support package</span></span>
<span data-ttu-id="2800d-171">Miután létrehozta a támogatási csomag, szükség lehet a bizalmas adatokat a csomag szerkesztésével.</span><span class="sxs-lookup"><span data-stu-id="2800d-171">After you have generated a support package, you might need to edit the package to remove sensitive information.</span></span> <span data-ttu-id="2800d-172">Ez magában foglalhatja kötetnevek eszköz IP-címek és a naplófájlok biztonsági mentési nevek.</span><span class="sxs-lookup"><span data-stu-id="2800d-172">This can include volume names, device IP addresses, and backup names from the log files.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2800d-173">Csak akkor szerkeszthető egy támogatási csomag, amelyik a Windows PowerShell segítségével a StorSimple jött létre.</span><span class="sxs-lookup"><span data-stu-id="2800d-173">You can only edit a support package that was generated through Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="2800d-174">A StorSimple Manager szolgáltatásban a klasszikus Azure portálon létrehozott csomagot nem szerkeszthetők.</span><span class="sxs-lookup"><span data-stu-id="2800d-174">You can't edit a package created in the Azure classic portal with StorSimple Manager service.</span></span>
> 
> 

<span data-ttu-id="2800d-175">Egy támogatási csomag a Microsoft Support helyen feltöltés előtt szerkesztéséhez először visszafejtése a támogatási csomag, szerkesztheti a fájlokat és újbóli titkosítására a azt.</span><span class="sxs-lookup"><span data-stu-id="2800d-175">To edit a support package before uploading it on the Microsoft Support site, first decrypt the support package, edit the files, and then re-encrypt it.</span></span> <span data-ttu-id="2800d-176">A következő lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="2800d-176">Perform the following steps.</span></span>

#### <a name="to-edit-a-support-package-in-windows-powershell-for-storsimple"></a><span data-ttu-id="2800d-177">A StorSimple egy támogatási csomag a Windows PowerShell szerkesztése</span><span class="sxs-lookup"><span data-stu-id="2800d-177">To edit a support package in Windows PowerShell for StorSimple</span></span>
1. <span data-ttu-id="2800d-178">Egy támogatási csomagot leírtak korábban [egy támogatási csomag létrehozása a Windows PowerShell StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).</span><span class="sxs-lookup"><span data-stu-id="2800d-178">Generate a support package as described earlier, in [To create a support package in Windows PowerShell for StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).</span></span>
2. <span data-ttu-id="2800d-179">[Töltse le a parancsfájl](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) helyileg az ügyfélen.</span><span class="sxs-lookup"><span data-stu-id="2800d-179">[Download the script](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) locally on your client.</span></span>
3. <span data-ttu-id="2800d-180">A Windows PowerShell-modul importálásához.</span><span class="sxs-lookup"><span data-stu-id="2800d-180">Import the Windows PowerShell module.</span></span> <span data-ttu-id="2800d-181">Adja meg a helyi mappát, amelybe letöltötte a parancsfájl elérési útja.</span><span class="sxs-lookup"><span data-stu-id="2800d-181">Specify the path to the local folder in which you downloaded the script.</span></span> <span data-ttu-id="2800d-182">Importálja a modult, írja be a következőt:</span><span class="sxs-lookup"><span data-stu-id="2800d-182">To import the module, enter:</span></span>
   
    `Import-module <Path to the folder that contains the Windows PowerShell script>`
4. <span data-ttu-id="2800d-183">A fájlok *.aes* titkosított és tömörített fájlok.</span><span class="sxs-lookup"><span data-stu-id="2800d-183">All the files are *.aes* files that are compressed and encrypted.</span></span> <span data-ttu-id="2800d-184">Kibontani és fejti vissza a fájlokat, írja be a következőt:</span><span class="sxs-lookup"><span data-stu-id="2800d-184">To decompress and decrypt files, enter:</span></span>
   
    `Open-HcsSupportPackage <Path to the folder that contains support package files>`
   
    <span data-ttu-id="2800d-185">Vegye figyelembe, hogy a tényleges kiterjesztések jelennek meg a fájlok.</span><span class="sxs-lookup"><span data-stu-id="2800d-185">Note that the actual file extensions are now displayed for all the files.</span></span>
   
    ![Támogatási csomag szerkesztése](./media/storsimple-create-manage-support-package/IC750706.png)
5. <span data-ttu-id="2800d-187">Amikor a rendszer kéri, a titkosítás jelszavát, adja meg a jelszót, amelyet a támogatási csomag létrehozásakor használja.</span><span class="sxs-lookup"><span data-stu-id="2800d-187">When you're prompted for the encryption passphrase, enter the passphrase that you used when the support package was created.</span></span>
   
        cmdlet Open-HcsSupportPackage at command pipeline position 1
   
        Supply values for the following parameters:EncryptionPassphrase: ****
6. <span data-ttu-id="2800d-188">Tallózással keresse meg a naplófájlokat tartalmazó mappát.</span><span class="sxs-lookup"><span data-stu-id="2800d-188">Browse to the folder that contains the log files.</span></span> <span data-ttu-id="2800d-189">Mivel a naplófájlok most kibontása és visszafejtése, ezek lesz eredeti fájlkiterjesztés.</span><span class="sxs-lookup"><span data-stu-id="2800d-189">Because the log files are now decompressed and decrypted, these will have original file extensions.</span></span> <span data-ttu-id="2800d-190">Módosíthatja bármely adatok, például a kötet neve és az eszköz IP-címek, távolítsa el ezeket a fájlokat, és menti a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="2800d-190">Modify these files to remove any customer-specific information, such as volume names and device IP addresses, and save the files.</span></span>
7. <span data-ttu-id="2800d-191">Zárja be a fájlok tömörítése a gzip és az AES-256 titkosítani.</span><span class="sxs-lookup"><span data-stu-id="2800d-191">Close the files to compress them with gzip and encrypt them with AES-256.</span></span> <span data-ttu-id="2800d-192">Ez a sebesség és a biztonság a támogatási csomag átvitele a hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="2800d-192">This is for speed and security in transferring the support package over a network.</span></span> <span data-ttu-id="2800d-193">Tömöríti, és titkosíthatják a fájlokat, írja be a következőket:</span><span class="sxs-lookup"><span data-stu-id="2800d-193">To compress and encrypt files, enter the following:</span></span>
   
    `Close-HcsSupportPackage <Path to the folder that contains support package files>`
   
    ![Támogatási csomag szerkesztése](./media/storsimple-create-manage-support-package/IC750707.png)
8. <span data-ttu-id="2800d-195">Amikor a rendszer kéri, adja meg a módosított támogatási csomag titkosítás jelszavát.</span><span class="sxs-lookup"><span data-stu-id="2800d-195">When prompted, provide an encryption passphrase for the modified support package.</span></span>
   
        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for the following parameters:EncryptionPassphrase: ****
9. <span data-ttu-id="2800d-196">Írja le az új jelszót, úgy, hogy a Microsoft Support kérésekor megoszthatja azt.</span><span class="sxs-lookup"><span data-stu-id="2800d-196">Write down the new passphrase, so that you can share it with Microsoft Support when requested.</span></span>

### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a><span data-ttu-id="2800d-197">Példa: Egy támogatási csomag jelszóval védett megosztott fájlok szerkesztése</span><span class="sxs-lookup"><span data-stu-id="2800d-197">Example: Editing files in a support package on a password-protected share</span></span>
<span data-ttu-id="2800d-198">A következő példa bemutatja, hogyan visszafejtése, szerkesztése és egy támogatási csomag újbóli titkosítására.</span><span class="sxs-lookup"><span data-stu-id="2800d-198">The following example shows how to decrypt, edit, and re-encrypt a support package.</span></span>

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1

        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Close-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32>

## <a name="next-steps"></a><span data-ttu-id="2800d-199">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2800d-199">Next steps</span></span>
* <span data-ttu-id="2800d-200">Megtudhatja, hogyan [támogatási csomag és eszköznaplók segítségével eszköz hibaelhárítás](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="2800d-200">Learn how to [use support packages and device logs to troubleshoot your device deployment](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).</span></span>
* <span data-ttu-id="2800d-201">Megtudhatja, hogyan [felügyelete a StorSimple eszközt a StorSimple Manager szolgáltatás segítségével](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="2800d-201">Learn how to [use the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>

