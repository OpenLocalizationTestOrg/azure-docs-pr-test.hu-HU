---
title: "VMware-kiszolgáló Azure Backup Server aaaBack |} Microsoft Docs"
description: "Használja az Azure Backup Server tooback egy VMware vCenter/ESXi-kiszolgálók tooAzure vagy a lemez. Ez a cikk ismerteti a lépés = biztonsági mentése (vagy védelmének)-lépésre utasítást a VMware-munkaterhelések."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
ms.assetid: 6b131caf-de85-4eba-b8e6-d8a04545cd9d
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/24/2017
ms.author: markgal;
ms.openlocfilehash: 3edb6880a526ed0b18605fee0fac27196a608e7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-a-vmware-server-tooazure"></a><span data-ttu-id="f2b91-104">Készítsen biztonsági másolatot a VMware server tooAzure</span><span class="sxs-lookup"><span data-stu-id="f2b91-104">Back up a VMware server tooAzure</span></span>

<span data-ttu-id="f2b91-105">Ez a cikk azt ismerteti, hogyan tooconfigure Azure Backup Server toohelp VMware server munkaterhelések védelmét.</span><span class="sxs-lookup"><span data-stu-id="f2b91-105">This article explains how tooconfigure Azure Backup Server toohelp protect VMware server workloads.</span></span> <span data-ttu-id="f2b91-106">Ez a cikk feltételezi, hogy már rendelkezik Azure Backup Server telepítve.</span><span class="sxs-lookup"><span data-stu-id="f2b91-106">This article assumes you already have Azure Backup Server installed.</span></span> <span data-ttu-id="f2b91-107">Ha még nem rendelkezik telepített Azure Backup Server, lásd: [készítse elő a munkaterhelés Azure Backup Server mentése tooback](backup-azure-microsoft-azure-backup.md).</span><span class="sxs-lookup"><span data-stu-id="f2b91-107">If you don't have Azure Backup Server installed, see [Prepare tooback up workloads using Azure Backup Server](backup-azure-microsoft-azure-backup.md).</span></span>

<span data-ttu-id="f2b91-108">Az Azure Backup Server készítsen biztonsági másolatot, vagy védelme, a VMware vCenter Server verziója 6.5, 6.0 és 5.5.</span><span class="sxs-lookup"><span data-stu-id="f2b91-108">Azure Backup Server can back up, or help protect, VMware vCenter Server version 6.5, 6.0 and 5.5.</span></span>


## <a name="create-a-secure-connection-toohello-vcenter-server"></a><span data-ttu-id="f2b91-109">A biztonságos kapcsolat toohello vCenter-kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="f2b91-109">Create a secure connection toohello vCenter Server</span></span>

<span data-ttu-id="f2b91-110">Alapértelmezés szerint Azure Backup Server minden vCenter-kiszolgáló egy HTTPS-csatornán keresztül kommunikál.</span><span class="sxs-lookup"><span data-stu-id="f2b91-110">By default, Azure Backup Server communicates with each vCenter Server via an HTTPS channel.</span></span> <span data-ttu-id="f2b91-111">hello biztonságos kommunikáció tooturn, javasoljuk, hogy az Azure Backup Server telepítse hello VMware tanúsítvány hitelesítésszolgáltatói (CA) tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="f2b91-111">tooturn on hello secure communication, we recommend that you install hello VMware Certificate Authority (CA) certificate on Azure Backup Server.</span></span> <span data-ttu-id="f2b91-112">Ha nem feltétlenül szükséges a biztonságos kommunikációt, és inkább toodisable hello HTTPS követelmény, lásd: [letiltása biztonságos kommunikációs protokollt](backup-azure-backup-server-vmware.md#disable-secure-communication-protocol).</span><span class="sxs-lookup"><span data-stu-id="f2b91-112">If you don't require secure communication, and would prefer toodisable hello HTTPS requirement, see [Disable secure communication protocol](backup-azure-backup-server-vmware.md#disable-secure-communication-protocol).</span></span> <span data-ttu-id="f2b91-113">toocreate egy biztonságos kapcsolatot az Azure Backup Server és hello vCenter-kiszolgáló, hello megbízható tanúsítvány az Azure Backup Server importálása.</span><span class="sxs-lookup"><span data-stu-id="f2b91-113">toocreate a secure connection between Azure Backup Server and hello vCenter Server, import hello trusted certificate on Azure Backup Server.</span></span>

<span data-ttu-id="f2b91-114">Általában akkor használják a böngésző hello Azure Backup Server gép tooconnect toohello vcenter Server hello vSphere webes ügyfél keresztül.</span><span class="sxs-lookup"><span data-stu-id="f2b91-114">Typically, you use a browser on hello Azure Backup Server machine tooconnect toohello vCenter Server via hello vSphere Web Client.</span></span> <span data-ttu-id="f2b91-115">hello hello Azure Backup Server böngésző tooconnect toohello vCenter-kiszolgáló, első használatakor hello nem biztonságos.</span><span class="sxs-lookup"><span data-stu-id="f2b91-115">hello first time you use hello Azure Backup Server browser tooconnect toohello vCenter Server, hello connection isn't secure.</span></span> <span data-ttu-id="f2b91-116">a következő kép hello hello nem biztonságos kapcsolat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="f2b91-116">hello following image shows hello unsecured connection.</span></span>

![Nem biztonságos kapcsolat tooVMware kiszolgáló – példa](./media/backup-azure-backup-server-vmware/unsecure-url.png)

<span data-ttu-id="f2b91-118">toofix erről a problémáról és biztonságos kapcsolat létrehozásához, hello megbízható legfelső szintű Hitelesítésszolgáltatói tanúsítvány letöltése.</span><span class="sxs-lookup"><span data-stu-id="f2b91-118">toofix this issue, and create a secure connection, download hello trusted root CA certificates.</span></span>

1. <span data-ttu-id="f2b91-119">Az Azure Backup Server hello böngészőben meg hello URL-cím toohello vSphere webes ügyfél.</span><span class="sxs-lookup"><span data-stu-id="f2b91-119">In hello browser on Azure Backup Server, enter hello URL toohello vSphere Web Client.</span></span> <span data-ttu-id="f2b91-120">hello vSphere webes ügyfél bejelentkezési oldal jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f2b91-120">hello vSphere Web Client login page appears.</span></span>

    ![a vSphere webügyfél](./media/backup-azure-backup-server-vmware/vsphere-web-client.png)

    <span data-ttu-id="f2b91-122">A rendszergazdák és fejlesztők hello információi hello alján található hello **letöltése megbízható legfelső szintű Hitelesítésszolgáltatói tanúsítványokat** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="f2b91-122">At hello bottom of hello information for administrators and developers, locate hello **Download trusted root CA certificates** link.</span></span>

    ![Hivatkozás toodownload hello megbízható legfelső szintű Hitelesítésszolgáltatói tanúsítványok](./media/backup-azure-backup-server-vmware/vmware-download-ca-cert-prompt.png)

  <span data-ttu-id="f2b91-124">Ha hello vSphere webes ügyfél bejelentkezési oldal nem jelenik meg, ellenőrizze a böngésző proxy beállításait.</span><span class="sxs-lookup"><span data-stu-id="f2b91-124">If you don't see hello vSphere Web Client login page, check your browser's proxy settings.</span></span>

2. <span data-ttu-id="f2b91-125">Kattintson a **letöltése megbízható legfelső szintű Hitelesítésszolgáltatói tanúsítványokat**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-125">Click **Download trusted root CA certificates**.</span></span>

    <span data-ttu-id="f2b91-126">hello vCenter-kiszolgáló letölti a fájl tooyour helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f2b91-126">hello vCenter Server downloads a file tooyour local computer.</span></span> <span data-ttu-id="f2b91-127">hello fájl neve **letöltése**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-127">hello file's name is named **download**.</span></span> <span data-ttu-id="f2b91-128">Attól függően, hogy a böngésző kap egy üzenetet, amely rákérdez, hogy tooopen vagy hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="f2b91-128">Depending on your browser, you receive a message that asks whether tooopen or save hello file.</span></span>

    ![üzenet letöltése a tanúsítványok lesznek letöltve](./media/backup-azure-backup-server-vmware/download-certs.png)

3. <span data-ttu-id="f2b91-130">Azure Backup Server hello fájl tooa helyre mentheti.</span><span class="sxs-lookup"><span data-stu-id="f2b91-130">Save hello file tooa location on Azure Backup Server.</span></span> <span data-ttu-id="f2b91-131">Hello fájl mentésekor hozzáadása hello .zip fájlnévkiterjesztést.</span><span class="sxs-lookup"><span data-stu-id="f2b91-131">When you save hello file, add hello .zip file name extension.</span></span>

    <span data-ttu-id="f2b91-132">hello fájl hello tanúsítványok hello információt tartalmazó .zip fájl.</span><span class="sxs-lookup"><span data-stu-id="f2b91-132">hello file is a .zip file that contains hello information about hello certificates.</span></span> <span data-ttu-id="f2b91-133">Hello .zip kiterjesztésű hello kibontási eszközök is használhatja.</span><span class="sxs-lookup"><span data-stu-id="f2b91-133">With hello .zip extension, you can use hello extraction tools.</span></span>

4. <span data-ttu-id="f2b91-134">Kattintson a jobb gombbal **download.zip**, majd válassza ki **összes kibontása** tooextract hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="f2b91-134">Right-click **download.zip**, and then select **Extract All** tooextract hello contents.</span></span>

    <span data-ttu-id="f2b91-135">hello .zip fájl kibontása a tartalmak tooa nevű mappát **Tanúsítványos**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-135">hello .zip file extracts its contents tooa folder named **certs**.</span></span> <span data-ttu-id="f2b91-136">Két típusú fájlokat hello Tanúsítványos mappában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="f2b91-136">Two types of files appear in hello certs folder.</span></span> <span data-ttu-id="f2b91-137">hello főtanúsítvány-fájl kiterjesztése például.0 és ikonra.1 számozott sorozatát kezdődik.</span><span class="sxs-lookup"><span data-stu-id="f2b91-137">hello root certificate file has an extension that begins with a numbered sequence like .0 and .1.</span></span>
    
    <span data-ttu-id="f2b91-138">hello CRL fájl kiterjesztése, amely egy sorozatban a hasonlóan .r0 vagy .r1 kezdődik.</span><span class="sxs-lookup"><span data-stu-id="f2b91-138">hello CRL file has an extension that begins with a sequence like .r0 or .r1.</span></span> <span data-ttu-id="f2b91-139">hello CRL-fájlnak a tanúsítvány hozzá rendelve.</span><span class="sxs-lookup"><span data-stu-id="f2b91-139">hello CRL file is associated with a certificate.</span></span>

    ![<span data-ttu-id="f2b91-140">Töltse le a fájlt helyileg kibontása</span><span class="sxs-lookup"><span data-stu-id="f2b91-140">Download file extracted locally</span></span> ](./media/backup-azure-backup-server-vmware/extracted-files-in-certs-folder.png)

5. <span data-ttu-id="f2b91-141">A hello **Tanúsítványos** mappába, kattintson a jobb gombbal a hello főtanúsítvány-fájlt, majd kattintson a **átnevezése**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-141">In hello **certs** folder, right-click hello root certificate file, and then click **Rename**.</span></span>

    ![<span data-ttu-id="f2b91-142">Nevezze át a legfelső szintű tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="f2b91-142">Rename root certificate</span></span> ](./media/backup-azure-backup-server-vmware/rename-cert.png)

    <span data-ttu-id="f2b91-143">Hello legfelső szintű tanúsítvány bővítmény too.crt módosítása.</span><span class="sxs-lookup"><span data-stu-id="f2b91-143">Change hello root certificate's extension too.crt.</span></span> <span data-ttu-id="f2b91-144">Még ha biztos abban, hogy toochange hello bővítmény szeretne, kattintson a kérdésnél **Igen** vagy **OK**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-144">When you're asked if you're sure you want toochange hello extension, click **Yes** or **OK**.</span></span> <span data-ttu-id="f2b91-145">Ellenkező esetben hello fájl tervezett függvény változtatható meg.</span><span class="sxs-lookup"><span data-stu-id="f2b91-145">Otherwise, you change hello file's intended function.</span></span> <span data-ttu-id="f2b91-146">hello fájl módosításait tooan ikonjára, legfelső szintű tanúsítvány hello ikonjára.</span><span class="sxs-lookup"><span data-stu-id="f2b91-146">hello icon for hello file changes tooan icon that represents a root certificate.</span></span>

6. <span data-ttu-id="f2b91-147">Kattintson a jobb gombbal a legfelső szintű tanúsítvány hello és hello előugró menüjét, válassza a **tanúsítvány telepítése**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-147">Right-click hello root certificate and from hello pop-up menu, select **Install Certificate**.</span></span>

    <span data-ttu-id="f2b91-148">Hello **Tanúsítványimportáló varázsló** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f2b91-148">hello **Certificate Import Wizard** dialog box appears.</span></span>

7. <span data-ttu-id="f2b91-149">A hello **Tanúsítványimportáló varázsló** párbeszédpanelen jelölje ki **helyi számítógép** hello tanúsítványt, és kattintson a hello célként **tovább** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="f2b91-149">In hello **Certificate Import Wizard** dialog box, select **Local Machine** as hello destination for hello certificate, and then click **Next** toocontinue.</span></span>

    ![<span data-ttu-id="f2b91-150">Tanúsítvány tárolási cél beállításai</span><span class="sxs-lookup"><span data-stu-id="f2b91-150">Certificate storage destination options</span></span> ](./media/backup-azure-backup-server-vmware/certificate-import-wizard1.png)

    <span data-ttu-id="f2b91-151">Ha megkérdezi, hogy ha azt szeretné, tooallow módosítások toohello számítógép, kattintson a **Igen** vagy **OK**, tooall hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="f2b91-151">If you're asked if you want tooallow changes toohello computer, click **Yes** or **OK**, tooall hello changes.</span></span>

8. <span data-ttu-id="f2b91-152">A hello **tanúsítványtároló** lapon jelölje be **minden tanúsítvány tárolása a következő tároló hello**, és kattintson a **Tallózás** toochoose hello tanúsítványtárolójába.</span><span class="sxs-lookup"><span data-stu-id="f2b91-152">On hello **Certificate Store** page, select **Place all certificates in hello following store**, and then click **Browse** toochoose hello certificate store.</span></span>

    ![Bizonyos tárolási helyet a tanúsítvány tárolása](./media/backup-azure-backup-server-vmware/cert-import-wizard-local-store.png)

    <span data-ttu-id="f2b91-154">Hello **tanúsítványtároló kiválasztása** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f2b91-154">hello **Select Certificate Store** dialog box appears.</span></span>

    ![Tanúsítványhierarchia tároló mappa](./media/backup-azure-backup-server-vmware/cert-store.png)

9. <span data-ttu-id="f2b91-156">Válassza ki **megbízható legfelső szintű hitelesítésszolgáltatók** hello tanúsítványokat, és kattintson hello célmappát, **OK**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-156">Select **Trusted Root Certification Authorities** as hello destination folder for hello certificates, and then click **OK**.</span></span>

    ![Tanúsítvány célmappájának](./media/backup-azure-backup-server-vmware/certificate-store-selected.png)

    <span data-ttu-id="f2b91-158">Hello **megbízható legfelső szintű hitelesítésszolgáltatók** mappa Megerősítjük hello tanúsítvány tárolóként.</span><span class="sxs-lookup"><span data-stu-id="f2b91-158">hello **Trusted Root Certification Authorities** folder is confirmed as hello certificate store.</span></span> <span data-ttu-id="f2b91-159">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="f2b91-159">Click **Next**.</span></span>

    ![Tanúsítvány tároló mappa](./media/backup-azure-backup-server-vmware/certificate-import-wizard2.png)

10. <span data-ttu-id="f2b91-161">A hello **Tanúsítványimportáló varázsló befejezése hello** lapon győződjön meg arról, hogy hello tanúsítvány hello kívánt mappában, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-161">On hello **Completing hello Certificate Import Wizard** page, verify that hello certificate is in hello desired folder, and then click **Finish**.</span></span>

    ![Ellenőrizze a tanúsítvány hello megfelelő mappában van](./media/backup-azure-backup-server-vmware/cert-wizard-final-screen.png)

    <span data-ttu-id="f2b91-163">Megjelenik egy párbeszédpanel, hello sikeres tanúsítvány importálása ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="f2b91-163">A dialog box appears, hello successful certificate import is confirmed.</span></span>

11. <span data-ttu-id="f2b91-164">Jelentkezzen be toohello vCenter Server tooconfirm, amely a kapcsolat biztonságos.</span><span class="sxs-lookup"><span data-stu-id="f2b91-164">Sign in toohello vCenter Server tooconfirm that your connection is secure.</span></span>

  <span data-ttu-id="f2b91-165">Ha hello tanúsítvány importálása nem sikeres, és nem tud biztonságos kapcsolatot, dokumentációt hello VMware vSphere a [kiszolgálótanúsítványok](http://pubs.vmware.com/vsphere-60/index.jsp#com.vmware.wssdk.dsg.doc/sdk_sg_server_certificate_Appendixes.6.4.html).</span><span class="sxs-lookup"><span data-stu-id="f2b91-165">If hello certificate import is not successful, and you cannot establish a secure connection, consult hello VMware vSphere documentation on [obtaining server certificates](http://pubs.vmware.com/vsphere-60/index.jsp#com.vmware.wssdk.dsg.doc/sdk_sg_server_certificate_Appendixes.6.4.html).</span></span>

  <span data-ttu-id="f2b91-166">Ha biztonságos határai vannak a szervezeten belül, és nem szeretné, hogy a HTTPS protokoll hello tooturn, használja a következő eljárás toodisable hello biztonságos kommunikáció hello.</span><span class="sxs-lookup"><span data-stu-id="f2b91-166">If you have secure boundaries within your organization, and don't want tooturn on hello HTTPS protocol, use hello following procedure toodisable hello secure communications.</span></span>

### <a name="disable-secure-communication-protocol"></a><span data-ttu-id="f2b91-167">Tiltsa le a biztonságos kommunikációs protokollja</span><span class="sxs-lookup"><span data-stu-id="f2b91-167">Disable secure communication protocol</span></span>

<span data-ttu-id="f2b91-168">Ha a szervezet hello HTTPS protokoll nem igényel, használja a következő lépéseket toodisable HTTPS hello.</span><span class="sxs-lookup"><span data-stu-id="f2b91-168">If your organization doesn't require hello HTTPS protocol, use hello following steps toodisable HTTPS.</span></span> <span data-ttu-id="f2b91-169">toodisable hello alapértelmezett viselkedését, és hozzon létre egy beállításkulcsot, amely figyelmen kívül hagyja az alapértelmezett viselkedés hello.</span><span class="sxs-lookup"><span data-stu-id="f2b91-169">toodisable hello default behavior, create a registry key that ignores hello default behavior.</span></span>

1. <span data-ttu-id="f2b91-170">Másolja és illessze be a szöveget egy .txt fájlban a következő hello.</span><span class="sxs-lookup"><span data-stu-id="f2b91-170">Copy and paste hello following text into a .txt file.</span></span>

  ```
  Windows Registry Editor Version 5.00
  [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager\VMWare]
  "IgnoreCertificateValidation"=dword:00000001
  ```

2. <span data-ttu-id="f2b91-171">Mentse a hello fájl tooyour Azure Backup Server számítógéphez.</span><span class="sxs-lookup"><span data-stu-id="f2b91-171">Save hello file tooyour Azure Backup Server computer.</span></span> <span data-ttu-id="f2b91-172">Hello fájlnév DisableSecureAuthentication.reg használja.</span><span class="sxs-lookup"><span data-stu-id="f2b91-172">For hello file name, use DisableSecureAuthentication.reg.</span></span>

3. <span data-ttu-id="f2b91-173">Kattintson duplán a hello fájl tooactivate hello bejegyzésre.</span><span class="sxs-lookup"><span data-stu-id="f2b91-173">Double-click hello file tooactivate hello registry entry.</span></span>


## <a name="create-a-role-and-user-account-on-hello-vcenter-server"></a><span data-ttu-id="f2b91-174">Hozzon létre egy szerepkör és a felhasználói fiókot hello vCenter-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="f2b91-174">Create a role and user account on hello vCenter Server</span></span>

<span data-ttu-id="f2b91-175">Hello vcenter Server a szerepkör az előre meghatározott jogosultságokat.</span><span class="sxs-lookup"><span data-stu-id="f2b91-175">On hello vCenter Server, a role is a predefined set of privileges.</span></span> <span data-ttu-id="f2b91-176">A vCenter-kiszolgáló rendszergazdája hello szerepkörök hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f2b91-176">A vCenter Server administrator creates hello roles.</span></span> <span data-ttu-id="f2b91-177">tooassign engedélyek hello rendszergazdai szerepkörrel rendelkező felhasználói fiókok párokat.</span><span class="sxs-lookup"><span data-stu-id="f2b91-177">tooassign permissions, hello administrator pairs user accounts with a role.</span></span> <span data-ttu-id="f2b91-178">tooestablish hello szükséges felhasználói hitelesítő adatok tooback hello vCenter-kiszolgáló számítógép, a szerepkör létrehozása adott jogosultságokkal, és majd hello felhasználói fiók társítása hello szerepkör.</span><span class="sxs-lookup"><span data-stu-id="f2b91-178">tooestablish hello necessary user credentials tooback up hello vCenter Server computer, create a role with specific privileges, and then associate hello user account with hello role.</span></span>

<span data-ttu-id="f2b91-179">Az Azure Backup Server egy felhasználónév és jelszó tooauthenticate hello vCenter-kiszolgáló használ.</span><span class="sxs-lookup"><span data-stu-id="f2b91-179">Azure Backup Server uses a username and password tooauthenticate with hello vCenter Server.</span></span> <span data-ttu-id="f2b91-180">Az Azure Backup Server ezeket a hitelesítő adatokat használ az összes biztonsági mentési műveletek hitelesítésként.</span><span class="sxs-lookup"><span data-stu-id="f2b91-180">Azure Backup Server uses these credentials as authentication for all backup operations.</span></span>

<span data-ttu-id="f2b91-181">tooadd egy vCenter-kiszolgáló szerepkör és a biztonsági mentési rendszergazda jogosultsággal:</span><span class="sxs-lookup"><span data-stu-id="f2b91-181">tooadd a vCenter Server role and its privileges for a backup administrator:</span></span>

1. <span data-ttu-id="f2b91-182">Jelentkezzen be a toohello vCenter-kiszolgáló, majd a hello vCenter-kiszolgáló **Navigator** panelen, kattintson a **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-182">Sign in toohello vCenter Server, and then in hello vCenter Server **Navigator** panel, click **Administration**.</span></span>

    ![A vCenter Server Navigator panelen rendszergazdai beállítás](./media/backup-azure-backup-server-vmware/vmware-navigator-panel.png)

2. <span data-ttu-id="f2b91-184">A **felügyeleti** válasszon **szerepkörök**, majd a hello **szerepkörök** panelen kattintson hello szerepkör ikon (hello + szimbólumra) hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="f2b91-184">In **Administration** select **Roles**, and then in hello **Roles** panel click hello add role icon (hello + symbol).</span></span>

    ![Szerepkör hozzáadása](./media/backup-azure-backup-server-vmware/vmware-define-new-role.png)

    <span data-ttu-id="f2b91-186">Hello **szerepkör létrehozása** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f2b91-186">hello **Create Role** dialog box appears.</span></span>

    ![Szerepkör létrehozása](./media/backup-azure-backup-server-vmware/vmware-define-new-role-priv.png)

3. <span data-ttu-id="f2b91-188">A hello **szerepkör létrehozása** párbeszédpanel hello **szerepkörnév** adja meg a *BackupAdminRole*.</span><span class="sxs-lookup"><span data-stu-id="f2b91-188">In hello **Create Role** dialog box, in hello **Role name** box, enter *BackupAdminRole*.</span></span> <span data-ttu-id="f2b91-189">hello szerepkör neve lehet bármilyen, de felismerhető hello szerepkör célra kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f2b91-189">hello role name can be whatever you like, but it should be recognizable for hello role's purpose.</span></span>

4. <span data-ttu-id="f2b91-190">Válassza ki a vCenter megfelelő verziójának hello hello jogosultságokkal, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-190">Select hello privileges for hello appropriate version of vCenter, and then click **OK**.</span></span> <span data-ttu-id="f2b91-191">a következő táblázat hello azonosítja a vCenter 6.0 és vCenter 5.5 hello szükséges jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="f2b91-191">hello following table identifies hello required privileges for vCenter 6.0 and vCenter 5.5.</span></span>

  <span data-ttu-id="f2b91-192">Hello jogosultságokkal kiválasztásakor kattintson hello ikon következő toohello szülő címke tooexpand hello szülő- és nézet hello gyermek jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="f2b91-192">When you select hello privileges, click hello icon next toohello parent label tooexpand hello parent and view hello child privileges.</span></span> <span data-ttu-id="f2b91-193">tooselect hello VirtualMachine jogosultságokkal kell toogo hello be több szintet szülő-gyermek hierarchia.</span><span class="sxs-lookup"><span data-stu-id="f2b91-193">tooselect hello VirtualMachine privileges, you need toogo several levels into hello parent child hierarchy.</span></span> <span data-ttu-id="f2b91-194">Nincs szükség a tooselect belül szülő jogosultság az összes alárendelt jogosultság.</span><span class="sxs-lookup"><span data-stu-id="f2b91-194">You don't need tooselect all child privileges within a parent privilege.</span></span>

  ![Szülő-gyermek jogosultság hierarchiát](./media/backup-azure-backup-server-vmware/cert-add-privilege-expand.png)

  <span data-ttu-id="f2b91-196">Miután rákattintott **OK**, hello új szerepkör hello szerepkörök panelen hello listájában jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f2b91-196">After you click **OK**, hello new role appears in hello list on hello Roles panel.</span></span>

|<span data-ttu-id="f2b91-197">Vcenter 6.0 jogosultságokkal</span><span class="sxs-lookup"><span data-stu-id="f2b91-197">Privileges for vCenter 6.0</span></span>| <span data-ttu-id="f2b91-198">Vcenter 5.5 jogosultságokkal</span><span class="sxs-lookup"><span data-stu-id="f2b91-198">Privileges for vCenter 5.5</span></span>|
|--------------------------|---------------------------|
|<span data-ttu-id="f2b91-199">Datastore.AllocateSpace</span><span class="sxs-lookup"><span data-stu-id="f2b91-199">Datastore.AllocateSpace</span></span>   | <span data-ttu-id="f2b91-200">Datastore.AllocateSpace</span><span class="sxs-lookup"><span data-stu-id="f2b91-200">Datastore.AllocateSpace</span></span>|
|<span data-ttu-id="f2b91-201">Global.ManageCustomFields</span><span class="sxs-lookup"><span data-stu-id="f2b91-201">Global.ManageCustomFields</span></span> | <span data-ttu-id="f2b91-202">Global.ManageCustomerFields</span><span class="sxs-lookup"><span data-stu-id="f2b91-202">Global.ManageCustomerFields</span></span>|
|<span data-ttu-id="f2b91-203">Global.SetCustomFields</span><span class="sxs-lookup"><span data-stu-id="f2b91-203">Global.SetCustomFields</span></span>    |   |
|<span data-ttu-id="f2b91-204">Host.Local.CreateVM</span><span class="sxs-lookup"><span data-stu-id="f2b91-204">Host.Local.CreateVM</span></span>       | <span data-ttu-id="f2b91-205">Network.Assign</span><span class="sxs-lookup"><span data-stu-id="f2b91-205">Network.Assign</span></span> |
|<span data-ttu-id="f2b91-206">Network.Assign</span><span class="sxs-lookup"><span data-stu-id="f2b91-206">Network.Assign</span></span>            |  |
|<span data-ttu-id="f2b91-207">Resource.AssignVMToPool</span><span class="sxs-lookup"><span data-stu-id="f2b91-207">Resource.AssignVMToPool</span></span>   |  |
|<span data-ttu-id="f2b91-208">VirtualMachine.Config.AddNewDisk</span><span class="sxs-lookup"><span data-stu-id="f2b91-208">VirtualMachine.Config.AddNewDisk</span></span>  | <span data-ttu-id="f2b91-209">VirtualMachine.Config.AddNewDisk</span><span class="sxs-lookup"><span data-stu-id="f2b91-209">VirtualMachine.Config.AddNewDisk</span></span>   |
|<span data-ttu-id="f2b91-210">VirtualMachine.Config.AdvanceConfig</span><span class="sxs-lookup"><span data-stu-id="f2b91-210">VirtualMachine.Config.AdvanceConfig</span></span>| <span data-ttu-id="f2b91-211">VirtualMachine.Config.AdvancedConfig</span><span class="sxs-lookup"><span data-stu-id="f2b91-211">VirtualMachine.Config.AdvancedConfig</span></span>|
|<span data-ttu-id="f2b91-212">VirtualMachine.Config.ChangeTracking</span><span class="sxs-lookup"><span data-stu-id="f2b91-212">VirtualMachine.Config.ChangeTracking</span></span>| <span data-ttu-id="f2b91-213">VirtualMachine.Config.ChangeTracking</span><span class="sxs-lookup"><span data-stu-id="f2b91-213">VirtualMachine.Config.ChangeTracking</span></span> |
|<span data-ttu-id="f2b91-214">VirtualMachine.Config.HostUSBDevice</span><span class="sxs-lookup"><span data-stu-id="f2b91-214">VirtualMachine.Config.HostUSBDevice</span></span>||
|<span data-ttu-id="f2b91-215">VirtualMachine.Config.QueryUnownedFiles</span><span class="sxs-lookup"><span data-stu-id="f2b91-215">VirtualMachine.Config.QueryUnownedFiles</span></span>|    |
|<span data-ttu-id="f2b91-216">VirtualMachine.Config.SwapPlacement</span><span class="sxs-lookup"><span data-stu-id="f2b91-216">VirtualMachine.Config.SwapPlacement</span></span>| <span data-ttu-id="f2b91-217">VirtualMachine.Config.SwapPlacement</span><span class="sxs-lookup"><span data-stu-id="f2b91-217">VirtualMachine.Config.SwapPlacement</span></span> |
|<span data-ttu-id="f2b91-218">VirtualMachine.Interact.PowerOff</span><span class="sxs-lookup"><span data-stu-id="f2b91-218">VirtualMachine.Interact.PowerOff</span></span>| <span data-ttu-id="f2b91-219">VirtualMachine.Interact.PowerOff</span><span class="sxs-lookup"><span data-stu-id="f2b91-219">VirtualMachine.Interact.PowerOff</span></span> |
|<span data-ttu-id="f2b91-220">VirtualMachine.Inventory.Create</span><span class="sxs-lookup"><span data-stu-id="f2b91-220">VirtualMachine.Inventory.Create</span></span>| <span data-ttu-id="f2b91-221">VirtualMachine.Inventory.Create</span><span class="sxs-lookup"><span data-stu-id="f2b91-221">VirtualMachine.Inventory.Create</span></span> |
|<span data-ttu-id="f2b91-222">VirtualMachine.Provisioning.DiskRandomAccess</span><span class="sxs-lookup"><span data-stu-id="f2b91-222">VirtualMachine.Provisioning.DiskRandomAccess</span></span>| |
|<span data-ttu-id="f2b91-223">VirtualMachine.Provisioning.DiskRandomRead</span><span class="sxs-lookup"><span data-stu-id="f2b91-223">VirtualMachine.Provisioning.DiskRandomRead</span></span>|<span data-ttu-id="f2b91-224">VirtualMachine.Provisioning.DiskRandomRead</span><span class="sxs-lookup"><span data-stu-id="f2b91-224">VirtualMachine.Provisioning.DiskRandomRead</span></span> |
|<span data-ttu-id="f2b91-225">VirtualMachine.State.CreateSnapshot</span><span class="sxs-lookup"><span data-stu-id="f2b91-225">VirtualMachine.State.CreateSnapshot</span></span>| <span data-ttu-id="f2b91-226">VirtualMachine.State.CreateSnapshot</span><span class="sxs-lookup"><span data-stu-id="f2b91-226">VirtualMachine.State.CreateSnapshot</span></span>|
|<span data-ttu-id="f2b91-227">VirtualMachine.State.RemoveSnapshot</span><span class="sxs-lookup"><span data-stu-id="f2b91-227">VirtualMachine.State.RemoveSnapshot</span></span>|<span data-ttu-id="f2b91-228">VirtualMachine.State.RemoveSnapshot</span><span class="sxs-lookup"><span data-stu-id="f2b91-228">VirtualMachine.State.RemoveSnapshot</span></span> |
</br>



## <a name="create-a-vcenter-server-user-account-and-permissions"></a><span data-ttu-id="f2b91-229">A vCenter Server felhasználói fiókjával és engedélyeivel létrehozása</span><span class="sxs-lookup"><span data-stu-id="f2b91-229">Create a vCenter Server user account and permissions</span></span>

<span data-ttu-id="f2b91-230">Jogosultságokkal hello szerepkör telepítése után, a felhasználói fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f2b91-230">After hello role with privileges is set up, create a user account.</span></span> <span data-ttu-id="f2b91-231">hello felhasználói fiók rendelkezik, egy nevet és jelszót, így a hitelesítéshez használt hello hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="f2b91-231">hello user account has a name and password, which provides hello credentials that are used for authentication.</span></span>

1. <span data-ttu-id="f2b91-232">egy felhasználói fiókot, a hello vCenter Server toocreate **Navigator** panelen, kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-232">toocreate a user account, in hello vCenter Server **Navigator** panel, click **Users and Groups**.</span></span>

    ![Felhasználók és csoportok beállítás](./media/backup-azure-backup-server-vmware/vmware-userandgroup-panel.png)

    <span data-ttu-id="f2b91-234">Hello **vCenter felhasználók és csoportok** panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f2b91-234">hello **vCenter Users and Groups** panel appears.</span></span>

    ![vCenter-felhasználók és csoportok panelen](./media/backup-azure-backup-server-vmware/usersandgroups.png)

2. <span data-ttu-id="f2b91-236">A hello **vCenter felhasználók és csoportok** panelen, jelölje be hello **felhasználók** fülre, majd hello felhasználók ikon (hello + szimbólumra) hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="f2b91-236">In hello **vCenter Users and Groups** panel, select hello **Users** tab, and then click hello add users icon (hello + symbol).</span></span>

    <span data-ttu-id="f2b91-237">Hello **új felhasználó** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f2b91-237">hello **New User** dialog box appears.</span></span>

3. <span data-ttu-id="f2b91-238">A hello **új felhasználó** párbeszédpanel mezőbe, adja hozzá a hello felhasználói adatokat, majd kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-238">In hello **New User** dialog box, add hello user's information and then click **OK**.</span></span> <span data-ttu-id="f2b91-239">Ezzel az eljárással hello felhasználónév BackupAdmin.</span><span class="sxs-lookup"><span data-stu-id="f2b91-239">In this procedure, hello username is BackupAdmin.</span></span>

    ![Új felhasználó párbeszédpanel](./media/backup-azure-backup-server-vmware/vmware-new-user-account.png)

    <span data-ttu-id="f2b91-241">új felhasználói fiók hello hello listájában jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f2b91-241">hello new user account appears in hello list.</span></span>

4. <span data-ttu-id="f2b91-242">tooassociate hello felhasználói fiók hello szerepkörrel, a hello **Navigator** panelen, kattintson a **globális engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-242">tooassociate hello user account with hello role, in hello **Navigator** panel, click **Global Permissions**.</span></span> <span data-ttu-id="f2b91-243">A hello **globális engedélyek** panelen, jelölje be hello **kezelése** fülre, majd hello hozzáadása (hello + szimbólumra) ikonra.</span><span class="sxs-lookup"><span data-stu-id="f2b91-243">In hello **Global Permissions** panel, select hello **Manage** tab, and then click hello add icon (hello + symbol).</span></span>

    ![Globális engedélyek panel](./media/backup-azure-backup-server-vmware/vmware-add-new-perms.png)

    <span data-ttu-id="f2b91-245">Hello **globális engedélyek gyökér - engedély hozzáadása** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f2b91-245">hello **Global Permissions Root - Add Permission** dialog box appears.</span></span>

5. <span data-ttu-id="f2b91-246">A hello **globális engedély gyökér - engedély hozzáadása** párbeszédpanel, kattintson a **Hozzáadás** toochoose hello felhasználó vagy csoport.</span><span class="sxs-lookup"><span data-stu-id="f2b91-246">In hello **Global Permission Root - Add Permission** dialog box, click **Add** toochoose hello user or group.</span></span>

    ![Felhasználó vagy csoport kiválasztása](./media/backup-azure-backup-server-vmware/vmware-add-new-global-perm.png)

    <span data-ttu-id="f2b91-248">Hello **felhasználók vagy csoportok kiválasztása** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f2b91-248">hello **Select Users/Groups** dialog box appears.</span></span>

6. <span data-ttu-id="f2b91-249">A hello **felhasználók vagy csoportok kiválasztása** párbeszédpanelen válassza ki **BackupAdmin** majd **hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-249">In hello **Select Users/Groups** dialog box, choose **BackupAdmin** and then click **Add**.</span></span>

    <span data-ttu-id="f2b91-250">A **felhasználók**, hello *tartomány\felhasználónév* formátum hello felhasználói fiók kerül használatra.</span><span class="sxs-lookup"><span data-stu-id="f2b91-250">In **Users**, hello *domain\username* format is used for hello user account.</span></span> <span data-ttu-id="f2b91-251">Ha azt szeretné, hogy egy másik tartományba toouse, válassza ki, a hello **tartomány** listája.</span><span class="sxs-lookup"><span data-stu-id="f2b91-251">If you want toouse a different domain, choose it from hello **Domain** list.</span></span>

    ![BackupAdmin felhasználó hozzáadása](./media/backup-azure-backup-server-vmware/vmware-assign-account-to-role.png)

    <span data-ttu-id="f2b91-253">Kattintson a **OK** tooadd hello kijelölt felhasználók toohello **hozzáadása engedély** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="f2b91-253">Click **OK** tooadd hello selected users toohello **Add Permission** dialog box.</span></span>

7. <span data-ttu-id="f2b91-254">Most, hogy hello felhasználói állapította meg, hello felhasználói toohello szerepkör hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="f2b91-254">Now that you've identified hello user, assign hello user toohello role.</span></span> <span data-ttu-id="f2b91-255">A **hozzárendelt szerepkör**, hello legördülő listában jelölje ki **BackupAdminRole**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-255">In **Assigned Role**, from hello drop-down list, select **BackupAdminRole**, and then click **OK**.</span></span>

    ![Rendelje hozzá a felhasználó toorole](./media/backup-azure-backup-server-vmware/vmware-choose-role.png)

  <span data-ttu-id="f2b91-257">A hello **kezelése** hello lapján **globális engedélyek** panelen, a hello új felhasználói fiók és a kapcsolódó hello szerepkör hello listájában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="f2b91-257">On hello **Manage** tab in hello **Global Permissions** panel, hello new user account and hello associated role appear in hello list.</span></span>


## <a name="establish-vcenter-server-credentials-on-azure-backup-server"></a><span data-ttu-id="f2b91-258">VCenter Server-felhasználó hitelesítő adatai Azure Backup Server létrehozása</span><span class="sxs-lookup"><span data-stu-id="f2b91-258">Establish vCenter Server credentials on Azure Backup Server</span></span>

<span data-ttu-id="f2b91-259">Hello VMware server tooAzure biztonsági mentés kiszolgáló felvétele előtt telepítse [1. frissítés az Azure Backup Server](https://support.microsoft.com/help/3175529/update-1-for-microsoft-azure-backup-server).</span><span class="sxs-lookup"><span data-stu-id="f2b91-259">Before you add hello VMware server tooAzure Backup Server, install [Update 1 for Azure Backup Server](https://support.microsoft.com/help/3175529/update-1-for-microsoft-azure-backup-server).</span></span>

1. <span data-ttu-id="f2b91-260">Azure Backup Server tooopen ikonra hello hello Azure Backup Server asztalon.</span><span class="sxs-lookup"><span data-stu-id="f2b91-260">tooopen Azure Backup Server, double-click hello icon on hello Azure Backup Server desktop.</span></span>

    ![Az Azure Backup Server ikonja](./media/backup-azure-backup-server-vmware/mabs-icon.png)

    <span data-ttu-id="f2b91-262">Ha hello ikon hello asztal nem találja, nyissa meg a Azure Backup Server hello listából a telepített alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="f2b91-262">If you can't find hello icon on hello desktop, open Azure Backup Server from hello list of installed apps.</span></span> <span data-ttu-id="f2b91-263">hello Azure Backup Server alkalmazás neve a Microsoft Azure Backup szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="f2b91-263">hello Azure Backup Server app name is called Microsoft Azure Backup.</span></span>

2. <span data-ttu-id="f2b91-264">Hello Azure Backup Server konzolon kattintson **felügyeleti**, kattintson a **az üzemi kiszolgálók**, majd hello eszközsávon kattintson **kezelése VMware**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-264">In hello Azure Backup Server console, click **Management**, click **Production Servers**, and then on hello tool ribbon, click **Manage VMware**.</span></span>

    ![Az Azure Backup Server konzol](./media/backup-azure-backup-server-vmware/add-vmware-credentials.png)

    <span data-ttu-id="f2b91-266">Hello **hitelesítő adatok kezelése** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f2b91-266">hello **Manage Credentials** dialog box appears.</span></span>

    ![Az Azure Backup Server hitelesítő adatok kezelése párbeszédpanel](./media/backup-azure-backup-server-vmware/mabs-manage-credentials-dialog.png)

3. <span data-ttu-id="f2b91-268">A hello **hitelesítő adatok kezelése** párbeszédpanel, kattintson a **Hozzáadás** tooopen hello **adja hozzá a hitelesítő adatok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f2b91-268">In hello **Manage Credentials** dialog box, click **Add** tooopen hello **Add Credential** dialog box.</span></span>

4. <span data-ttu-id="f2b91-269">A hello **adja hozzá a hitelesítő adatok** párbeszédpanelen adja meg egy nevet és leírást a hello új hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="f2b91-269">In hello **Add Credential** dialog box, enter a name and a description for hello new credential.</span></span> <span data-ttu-id="f2b91-270">Majd adja meg a hello felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="f2b91-270">Then specify hello username and password.</span></span> <span data-ttu-id="f2b91-271">hello nevét, *Contoso Vcenter hitelesítőadat* használja a következő eljárással hello tooidentify hello hitelesítő adat.</span><span class="sxs-lookup"><span data-stu-id="f2b91-271">hello name, *Contoso Vcenter credential* is used tooidentify hello credential in hello next procedure.</span></span> <span data-ttu-id="f2b91-272">Használjon hello azonos felhasználónév és jelszó, amellyel hello vCenter-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="f2b91-272">Use hello same username and password that is used for hello vCenter Server.</span></span> <span data-ttu-id="f2b91-273">Ha hello vCenter-kiszolgáló és az Azure Backup Server nem a hello ugyanabban a tartományban, a **felhasználónév**, adja meg a hello tartományát.</span><span class="sxs-lookup"><span data-stu-id="f2b91-273">If hello vCenter Server and Azure Backup Server are not in hello same domain, in **User name**, specify hello domain.</span></span>

    ![Az Azure Backup Server hozzáadása hitelesítő párbeszédpanel](./media/backup-azure-backup-server-vmware/mabs-add-credential-dialog2.png)

    <span data-ttu-id="f2b91-275">Kattintson a **Hozzáadás** tooadd hello új hitelesítő adatok tooAzure Server biztonsági másolat.</span><span class="sxs-lookup"><span data-stu-id="f2b91-275">Click **Add** tooadd hello new credential tooAzure Backup Server.</span></span> <span data-ttu-id="f2b91-276">hello új hitelesítő adatok hello hello listájában jelenik meg **hitelesítő adatok kezelése** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="f2b91-276">hello new credential appears in hello list in hello **Manage Credentials** dialog box.</span></span>
    
    ![Az Azure Backup Server hitelesítő adatok kezelése párbeszédpanel](./media/backup-azure-backup-server-vmware/new-list-of-mabs-creds.png)

5. <span data-ttu-id="f2b91-278">tooclose hello **hitelesítő adatok kezelése** párbeszédpanelen kattintson az hello **X** hello jobb felső sarokban.</span><span class="sxs-lookup"><span data-stu-id="f2b91-278">tooclose hello **Manage Credentials** dialog box, click hello **X** in hello upper-right corner.</span></span>


## <a name="add-hello-vcenter-server-tooazure-backup-server"></a><span data-ttu-id="f2b91-279">Hello vCenter Server tooAzure biztonsági mentés kiszolgáló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f2b91-279">Add hello vCenter Server tooAzure Backup Server</span></span>

<span data-ttu-id="f2b91-280">Az üzemi kiszolgáló hozzáadása varázsló használt tooadd hello vCenter Server tooAzure Backup Server.</span><span class="sxs-lookup"><span data-stu-id="f2b91-280">Production Server Addition Wizard is used tooadd hello vCenter Server tooAzure Backup Server.</span></span>

<span data-ttu-id="f2b91-281">tooopen üzemi kiszolgáló hozzáadása varázsló, a következő eljárás teljes hello:</span><span class="sxs-lookup"><span data-stu-id="f2b91-281">tooopen Production Server Addition Wizard, complete hello following procedure:</span></span>

1. <span data-ttu-id="f2b91-282">Hello Azure Backup Server konzolon kattintson **felügyeleti**, kattintson a **az üzemi kiszolgálók**, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-282">In hello Azure Backup Server console, click **Management**, click **Production Servers**, and then click **Add**.</span></span>

    ![Nyissa meg az üzemi kiszolgáló hozzáadása varázsló](./media/backup-azure-backup-server-vmware/add-vcenter-to-mabs.png)

    <span data-ttu-id="f2b91-284">Hello **üzemi kiszolgáló hozzáadása varázsló** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f2b91-284">hello **Production Server Addition Wizard** dialog box appears.</span></span>

    ![Az üzemi kiszolgáló hozzáadása varázsló](./media/backup-azure-backup-server-vmware/production-server-add-wizard.png)

2. <span data-ttu-id="f2b91-286">A hello **válassza ki az üzemi kiszolgáló típusa** lapon, hogy melyik **VMware Server**, és kattintson a **tovább**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-286">On hello **Select Production Server type** page, select **VMware Servers**, and then click **Next**.</span></span>

3. <span data-ttu-id="f2b91-287">A **kiszolgáló neve vagy IP-címet**, adja meg a hello teljesen minősített tartománynevét (FQDN) vagy hello VMware-kiszolgáló IP-címét.</span><span class="sxs-lookup"><span data-stu-id="f2b91-287">In **Server Name/IP Address**, specify hello fully qualified domain name (FQDN) or IP address of hello VMware server.</span></span> <span data-ttu-id="f2b91-288">Ha minden hello ESXi-kiszolgálókat kezeli-e hello eltérő vCenter hello vCenter neve is használhatja.</span><span class="sxs-lookup"><span data-stu-id="f2b91-288">If all hello ESXi servers are managed by hello same vCenter, you can use hello vCenter name.</span></span>

    ![Adja meg a VMware server FQDN vagy IP-címe](./media/backup-azure-backup-server-vmware/add-vmware-server-provide-server-name.png)

4. <span data-ttu-id="f2b91-290">A **SSL-Port**, adja meg a hello portot, amelyet használt toocommunicate hello VMware-kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="f2b91-290">In **SSL Port**, enter hello port that is used toocommunicate with hello VMware server.</span></span> <span data-ttu-id="f2b91-291">Port 443-as, amely hello alapértelmezett portot, hacsak nem tudja, hogy szükség-e egy másik portot használja.</span><span class="sxs-lookup"><span data-stu-id="f2b91-291">Use port 443, which is hello default port, unless you know that a different port is required.</span></span>

5. <span data-ttu-id="f2b91-292">A **adja meg a hitelesítő adatok**, válassza ki a korábban létrehozott hitelesítő hello.</span><span class="sxs-lookup"><span data-stu-id="f2b91-292">In **Specify Credential**, select hello credential that you created earlier.</span></span>

    ![Hitelesítő adatainak megadása](./media/backup-azure-backup-server-vmware/identify-creds.png)

6. <span data-ttu-id="f2b91-294">Kattintson a **Hozzáadás** tooadd hello VMware server toohello listája **hozzá VMware-kiszolgálók**, és kattintson a **következő** toomove toohello varázsló következő lapjára hello.</span><span class="sxs-lookup"><span data-stu-id="f2b91-294">Click **Add** tooadd hello VMware server toohello list of **Added VMware Servers**, and then click **Next** toomove toohello next page in hello wizard.</span></span>

    ![VMWare-kiszolgáló és a hitelesítő adatok hozzáadása](./media/backup-azure-backup-server-vmware/add-vmware-server-credentials.png)

7. <span data-ttu-id="f2b91-296">A hello **összegzés** kattintson **hozzáadása** tooadd hello megadott VMware server tooAzure Server biztonsági másolat.</span><span class="sxs-lookup"><span data-stu-id="f2b91-296">In hello **Summary** page, click **Add** tooadd hello specified VMware server tooAzure Backup Server.</span></span>

    ![Adja hozzá a VMware server tooAzure kiszolgáló biztonsági mentése](./media/backup-azure-backup-server-vmware/tasks-screen.png)

  <span data-ttu-id="f2b91-298">hello VMware server biztonsági másolat egy ügynök nélküli biztonsági mentést, és új kiszolgáló hello jelenik meg azonnal.</span><span class="sxs-lookup"><span data-stu-id="f2b91-298">hello VMware server backup is an agentless backup, and hello new server is added immediately.</span></span> <span data-ttu-id="f2b91-299">Hello **Befejezés** mutat be, hogy hello eredmények lapon.</span><span class="sxs-lookup"><span data-stu-id="f2b91-299">hello **Finish** page shows you hello results.</span></span>

  ![Befejezés lapra](./media/backup-azure-backup-server-vmware/summary-screen.png)

  <span data-ttu-id="f2b91-301">vCenter Server tooAzure Backup Server, ismétlődő hello előző több példánya ebben a szakaszban ismertetett visszaállítási lépésekkel tooadd.</span><span class="sxs-lookup"><span data-stu-id="f2b91-301">tooadd multiple instances of vCenter Server tooAzure Backup Server, repeat hello previous steps in this section.</span></span>

<span data-ttu-id="f2b91-302">Miután hozzáadta a hello vCenter Server tooAzure Server biztonsági másolat, hello következő lépésre toocreate egy védelmi csoportot.</span><span class="sxs-lookup"><span data-stu-id="f2b91-302">After you add hello vCenter Server tooAzure Backup Server, hello next step is toocreate a protection group.</span></span> <span data-ttu-id="f2b91-303">hello védelmi csoport határozza meg, hello rövid és hosszú távú megőrzési különböző részleteit, és határozza meg, és ahol a hello biztonsági mentési házirend alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="f2b91-303">hello protection group specifies hello various details for short or long-term retention, and it is where you define and apply hello backup policy.</span></span> <span data-ttu-id="f2b91-304">a biztonsági mentési házirend hello hello ütemezését Ha biztonsági mentések fordulnak elő, és milyen biztonsági mentése.</span><span class="sxs-lookup"><span data-stu-id="f2b91-304">hello backup policy is hello schedule for when backups occur, and what is backed up.</span></span>


## <a name="configure-a-protection-group"></a><span data-ttu-id="f2b91-305">A védelmi csoportok beállítása</span><span class="sxs-lookup"><span data-stu-id="f2b91-305">Configure a protection group</span></span>

<span data-ttu-id="f2b91-306">Ha még nem használta a System Center Data Protection Manager vagy az Azure Backup Server előtt, tekintse meg [lemezes biztonsági mentések tervezése](https://technet.microsoft.com/library/hh758026.aspx) tooprepare a Hardverkörnyezet.</span><span class="sxs-lookup"><span data-stu-id="f2b91-306">If you have not used System Center Data Protection Manager or Azure Backup Server before, see [Plan for disk backups](https://technet.microsoft.com/library/hh758026.aspx) tooprepare your hardware environment.</span></span> <span data-ttu-id="f2b91-307">Után ellenőrizheti, hogy rendelkezik-e megfelelő tárolását, használja a hello új védelmi csoport létrehozása varázsló tooadd VMware virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="f2b91-307">After you check that you have proper storage, use hello Create New Protection Group wizard tooadd VMware virtual machines.</span></span>

1. <span data-ttu-id="f2b91-308">Hello Azure Backup Server konzolon kattintson **védelmi**, és kattintson a menüszalagon hello, **új** tooopen hello új védelmi csoport létrehozása varázsló.</span><span class="sxs-lookup"><span data-stu-id="f2b91-308">In hello Azure Backup Server console, click **Protection**, and in hello tool ribbon, click **New** tooopen hello Create New Protection Group wizard.</span></span>

    ![Nyissa meg hello új védelmi csoport létrehozása varázsló](./media/backup-azure-backup-server-vmware/open-protection-wizard.png)

    <span data-ttu-id="f2b91-310">Hello **új védelmi csoport létrehozása** varázsló párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f2b91-310">hello **Create New Protection Group** wizard dialog box appears.</span></span>

    ![Új védelmi csoport létrehozása varázsló párbeszédpanelje](./media/backup-azure-backup-server-vmware/protection-wizard.png)

    <span data-ttu-id="f2b91-312">Kattintson a **következő** tooadvance toohello **védelmi csoport típusának kiválasztása** lap.</span><span class="sxs-lookup"><span data-stu-id="f2b91-312">Click **Next** tooadvance toohello **Select protection group type** page.</span></span>

2. <span data-ttu-id="f2b91-313">A hello **válassza ki a védelmi csoport típusának** lapon, hogy melyik **kiszolgálók** , majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-313">On hello **Select Protection group type** page, select **Servers** and then click **Next**.</span></span> <span data-ttu-id="f2b91-314">Hello **csoporttagok kiválasztása** lap jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f2b91-314">hello **Select group members** page appears.</span></span>

3. <span data-ttu-id="f2b91-315">A hello **csoporttagok kiválasztása** oldal, a választható tagok hello és a kiválasztott hello tagok jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="f2b91-315">On hello **Select group members** page, hello available members and hello selected members appear.</span></span> <span data-ttu-id="f2b91-316">Válassza ki a kívánt tooprotect, és kattintson hello tagokat **következő**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-316">Select hello members that you want tooprotect, and then click **Next**.</span></span>

    ![Csoporttagok kiválasztása](./media/backup-azure-backup-server-vmware/server-add-selected-members.png)

    <span data-ttu-id="f2b91-318">Felhasználót, ha más mappákban vagy a virtuális gépeket tartalmazó mappa kiválasztásakor ezen mappák és a virtuális gépek is ki lesz választva.</span><span class="sxs-lookup"><span data-stu-id="f2b91-318">When you select a member, if you select a folder that contains other folders or VMs, those folders and VMs are also selected.</span></span> <span data-ttu-id="f2b91-319">hello felvétel hello mappák és a virtuális gépek által hello szülőmappa neve mappa szintű védelmet.</span><span class="sxs-lookup"><span data-stu-id="f2b91-319">hello inclusion of hello folders and VMs in hello parent folder is called folder-level protection.</span></span> <span data-ttu-id="f2b91-320">tooremove egy mappa vagy a virtuális gép, törölje a jelet hello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="f2b91-320">tooremove a folder or VM, clear hello check box.</span></span>

    <span data-ttu-id="f2b91-321">Ha egy virtuális Gépet, vagy egy mappát, amely tartalmazza a virtuális gép már védett tooAzure, nem választhat ki ezt a virtuális Gépet újra.</span><span class="sxs-lookup"><span data-stu-id="f2b91-321">If a VM, or a folder containing a VM, is already protected tooAzure, you cannot select that VM again.</span></span> <span data-ttu-id="f2b91-322">Ez azt jelenti, hogy miután a virtuális gép védett tooAzure, ezért nem is védheti újra, amely megakadályozza, hogy ismétlődő helyreállítási pontokat hoz létre egy virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="f2b91-322">That is, after a VM is protected tooAzure, it cannot be protected again, which prevents duplicate recovery points from being created for one VM.</span></span> <span data-ttu-id="f2b91-323">Ha azt szeretné, hogy melyik Azure Backup Server-példány már védi a tag, pont toohello tag toosee hello neve kiszolgálót védő hello toosee.</span><span class="sxs-lookup"><span data-stu-id="f2b91-323">If you want toosee which Azure Backup Server instance already protects a member, point toohello member toosee hello name of hello protecting server.</span></span>

4. <span data-ttu-id="f2b91-324">A hello **adatvédelmi módszer kiválasztása** lapján adjon meg egy nevet hello védelmi csoportra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="f2b91-324">On hello **Select Data Protection Method** page, enter a name for hello protection group.</span></span> <span data-ttu-id="f2b91-325">Rövid távú védelem (toodisk) és az online védelem van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="f2b91-325">Short-term protection (toodisk) and online protection are selected.</span></span> <span data-ttu-id="f2b91-326">Ha azt szeretné, hogy az online védelem toouse (tooAzure), a rövid távú védelem toodisk kell használnia.</span><span class="sxs-lookup"><span data-stu-id="f2b91-326">If you want toouse online protection (tooAzure), you must use short-term protection toodisk.</span></span> <span data-ttu-id="f2b91-327">Kattintson a **következő** tooproceed toohello rövid távú védelmi ideje.</span><span class="sxs-lookup"><span data-stu-id="f2b91-327">Click **Next** tooproceed toohello short-term protection range.</span></span>

    ![Adatvédelmi módszer kiválasztása](./media/backup-azure-backup-server-vmware/name-protection-group.png)

5. <span data-ttu-id="f2b91-329">A hello **rövid távú célok megadása** lap, a **megőrzési időtartam**, adja meg a kívánt tooretain helyreállítási pontok megadott napok száma hello *toodisk tárolt*.</span><span class="sxs-lookup"><span data-stu-id="f2b91-329">On hello **Specify Short-Term Goals** page, for **Retention Range**, specify hello number of days that you want tooretain recovery points that are *stored toodisk*.</span></span> <span data-ttu-id="f2b91-330">Toochange hello idő és a nap, amikor helyreállítási pontokat készít, kattintson a **módosítás**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-330">If you want toochange hello time and days when recovery points are taken, click **Modify**.</span></span> <span data-ttu-id="f2b91-331">hello rövid távú helyreállítási pontok teljes biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="f2b91-331">hello short-term recovery points are full backups.</span></span> <span data-ttu-id="f2b91-332">Nincsenek növekményes biztonsági mentést.</span><span class="sxs-lookup"><span data-stu-id="f2b91-332">They are not incremental backups.</span></span> <span data-ttu-id="f2b91-333">Ha elégedett hello rövid távú célok, kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-333">When you are satisfied with hello short-term goals, click **Next**.</span></span>

    ![Rövid távú célok megadása](./media/backup-azure-backup-server-vmware/short-term-goals.png)

6. <span data-ttu-id="f2b91-335">A hello **tekintse át a lemezfoglalás** lapon ellenőrizze, és szükség esetén módosítsa a virtuális gépek hello hello lemezterület.</span><span class="sxs-lookup"><span data-stu-id="f2b91-335">On hello **Review Disk Allocation** page, review and if necessary, modify hello disk space for hello VMs.</span></span> <span data-ttu-id="f2b91-336">hello ajánlott lemezfoglalásokat hello megőrzési tartomány megadott hello alapuló **rövid távú célok megadása** hello munkaterhelés típusától és hello hello mérete védett adatokat (a 3. lépésben azonosított) lapon.</span><span class="sxs-lookup"><span data-stu-id="f2b91-336">hello recommended disk allocations are based on hello retention range that is specified in hello **Specify Short-Term Goals** page, hello type of workload, and hello size of hello protected data (identified in step 3).</span></span>  

  - <span data-ttu-id="f2b91-337">**Adatok mérete:** hello védelmi csoport adatainak hello méretét.</span><span class="sxs-lookup"><span data-stu-id="f2b91-337">**Data size:** Size of hello data in hello protection group.</span></span>
  - <span data-ttu-id="f2b91-338">**Szabad lemezterület:** hello ajánlott lemezterület hello védelmi csoportra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="f2b91-338">**Disk space:** hello recommended amount of disk space for hello protection group.</span></span> <span data-ttu-id="f2b91-339">Ha azt szeretné, toomodify ezt a beállítást, minden adatforrás növekedésének becslései hello összeg valamivel nagyobb helyet kell lefoglalni.</span><span class="sxs-lookup"><span data-stu-id="f2b91-339">If you want toomodify this setting, you should allocate total space that is slightly larger than hello amount that you estimate each data source grows.</span></span>
  - <span data-ttu-id="f2b91-340">**Tartozó adatok közös elhelyezése:** bekapcsolja a közös elhelyezést, ha több adatforrás hello védelmi leképezheti tooa egyetlen replika- és helyreállításipont-köteten.</span><span class="sxs-lookup"><span data-stu-id="f2b91-340">**Colocate data:** If you turn on colocation, multiple data sources in hello protection can map tooa single replica and recovery point volume.</span></span> <span data-ttu-id="f2b91-341">Közös elhelyezés nem minden munkaterhelésnél támogatott.</span><span class="sxs-lookup"><span data-stu-id="f2b91-341">Colocation isn't supported for all workloads.</span></span>
  - <span data-ttu-id="f2b91-342">**Méretének automatikus növelése:** Ha bekapcsolja ezt a beállítást, ha a védett hello csoport adatainak kezdeti foglalás hello meghaladja, a System Center Data Protection Manager tooincrease hello lemezméretet megpróbál 25 %-kal.</span><span class="sxs-lookup"><span data-stu-id="f2b91-342">**Automatically grow:** If you turn on this setting, if data in hello protected group outgrows hello initial allocation, System Center Data Protection Manager tries tooincrease hello disk size by 25 percent.</span></span>
  - <span data-ttu-id="f2b91-343">**Tárolókészlet részletei:** hello tárolókészlet, beleértve a teljes és szabad lemezterülettel hello állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="f2b91-343">**Storage pool details:** Shows hello status of hello storage pool, including total and remaining disk size.</span></span>

    ![Tekintse át a lemezkiosztást](./media/backup-azure-backup-server-vmware/review-disk-allocation.png)

    <span data-ttu-id="f2b91-345">Ha elégedett hello lemezterület-foglalás, kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-345">When you are satisfied with hello space allocation, click **Next**.</span></span>

7. <span data-ttu-id="f2b91-346">A hello **replika-létrehozási módszer kiválasztása** lapján adja meg, hogyan toogenerate hello kezdeti másolatot, vagy az Azure Backup Server hello védett adatok replikáját,.</span><span class="sxs-lookup"><span data-stu-id="f2b91-346">On hello **Choose Replica Creation Method** page, specify how you want toogenerate hello initial copy, or replica, of hello protected data on Azure Backup Server.</span></span>

    <span data-ttu-id="f2b91-347">hello alapértelmezett érték a **automatikusan hello hálózaton keresztül** és **most**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-347">hello default is **Automatically over hello network** and **Now**.</span></span> <span data-ttu-id="f2b91-348">Hello alapértelmezett használ, azt javasoljuk, hogy megadja a csúcsidőn kívüli időpontot.</span><span class="sxs-lookup"><span data-stu-id="f2b91-348">If you use hello default, we recommend that you specify an off-peak time.</span></span> <span data-ttu-id="f2b91-349">Válasszon **később** , és adja meg a napját és időpontját.</span><span class="sxs-lookup"><span data-stu-id="f2b91-349">Choose **Later** and specify a day and time.</span></span>

    <span data-ttu-id="f2b91-350">Nagy mennyiségű adatok vagy kevésbé optimális hálózati körülmények esetén érdemes megfontolni adatreplikálás hello offline cserélhető adathordozó használatával.</span><span class="sxs-lookup"><span data-stu-id="f2b91-350">For large amounts of data or less-than-optimal network conditions, consider replicating hello data offline by using removable media.</span></span>

    <span data-ttu-id="f2b91-351">Miután elvégezte a választott, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-351">After you have made your choices, click **Next**.</span></span>

    ![A replika-létrehozási módszer kiválasztása](./media/backup-azure-backup-server-vmware/replica-creation.png)

8. <span data-ttu-id="f2b91-353">A hello **konzisztencia-ellenőrzési beállítások** lapon válassza ki hogyan és mikor tooautomate hello konzisztencia-ellenőrzést.</span><span class="sxs-lookup"><span data-stu-id="f2b91-353">On hello **Consistency Check Options** page, select how and when tooautomate hello consistency checks.</span></span> <span data-ttu-id="f2b91-354">Konzisztencia-ellenőrzéseket is futtathatja, ha a replikaadatok inkonzisztenssé válik, vagy adott ütemezés szerinti.</span><span class="sxs-lookup"><span data-stu-id="f2b91-354">You can run consistency checks when replica data becomes inconsistent, or on a set schedule.</span></span>

    <span data-ttu-id="f2b91-355">Ha nem szeretné tooconfigure automatikus konzisztencia-ellenőrzést, egy manuális ellenőrzést is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="f2b91-355">If you don't want tooconfigure automatic consistency checks, you can run a manual check.</span></span> <span data-ttu-id="f2b91-356">Hello védelmi hello Azure Backup Server konzol, kattintson a jobb gombbal a hello védelmi csoportot, és válassza ki **konzisztencia-ellenőrzés**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-356">In hello protection area of hello Azure Backup Server console, right-click hello protection group and then select **Perform Consistency Check**.</span></span>

    <span data-ttu-id="f2b91-357">Kattintson a **következő** toomove toohello következő oldalra.</span><span class="sxs-lookup"><span data-stu-id="f2b91-357">Click **Next** toomove toohello next page.</span></span>

9. <span data-ttu-id="f2b91-358">A hello **Online védelem adatainak megadása** lapon, válassza ki a megjeleníteni kívánt tooprotect egy vagy több adatforrást.</span><span class="sxs-lookup"><span data-stu-id="f2b91-358">On hello **Specify Online Protection Data** page, select one or more data sources that you want tooprotect.</span></span> <span data-ttu-id="f2b91-359">Válassza ki egyenként hello tagokat, vagy kattintson a **kijelölése az összes** toochoose összes tagjához.</span><span class="sxs-lookup"><span data-stu-id="f2b91-359">You can select hello members individually, or click **Select All** toochoose all members.</span></span> <span data-ttu-id="f2b91-360">Hello tagok kiválasztása után kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-360">After you choose hello members, click **Next**.</span></span>

    ![Online védelem adatainak megadása](./media/backup-azure-backup-server-vmware/select-data-to-protect.png)

10. <span data-ttu-id="f2b91-362">A hello **Online biztonsági mentés ütemezésének megadása** adja meg azokat a hello ütemezés toogenerate helyreállítási pontok hello biztonsági másolat.</span><span class="sxs-lookup"><span data-stu-id="f2b91-362">On hello **Specify Online Backup Schedule** page, specify hello schedule toogenerate recovery points from hello disk backup.</span></span> <span data-ttu-id="f2b91-363">Miután hello helyreállítási pont jön létre, az Azure Recovery Services-tároló átvitt toohello áll.</span><span class="sxs-lookup"><span data-stu-id="f2b91-363">After hello recovery point is generated, it is transferred toohello Recovery Services vault in Azure.</span></span> <span data-ttu-id="f2b91-364">Ha elégedett hello online biztonsági mentési ütemezést, kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-364">When you are satisfied with hello online backup schedule, click **Next**.</span></span>

    ![Adja meg az online biztonsági mentés ütemezése](./media/backup-azure-backup-server-vmware/online-backup-schedule.png)

11. <span data-ttu-id="f2b91-366">A hello **Online adatmegőrzési szabály megadása** lapján adja meg, hogy mennyi ideig tooretain hello biztonsági mentési adatokat az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="f2b91-366">On hello **Specify Online Retention Policy** page, indicate how long you want tooretain hello backup data in Azure.</span></span> <span data-ttu-id="f2b91-367">Miután hello házirend lett meghatározva, kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-367">After hello policy is defined, click **Next**.</span></span>

    ![Online megőrzési szabály megadása](./media/backup-azure-backup-server-vmware/retention-policy.png)

    <span data-ttu-id="f2b91-369">Nincs nincs határideje, mennyi ideig tárolhatja az adatokat az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="f2b91-369">There is no time limit for how long you can keep data in Azure.</span></span> <span data-ttu-id="f2b91-370">Ha a helyreállítási pont adatait az Azure-ban tárolja, hello csak határértéke, hogy rendelkezik-e nem védett példányonként legfeljebb 9999 helyreállítási pontokat.</span><span class="sxs-lookup"><span data-stu-id="f2b91-370">When you store recovery point data in Azure, hello only limit is that you cannot have more than 9999 recovery points per protected instance.</span></span> <span data-ttu-id="f2b91-371">Ebben a példában hello védett példány hello VMware server.</span><span class="sxs-lookup"><span data-stu-id="f2b91-371">In this example, hello protected instance is hello VMware server.</span></span>

12. <span data-ttu-id="f2b91-372">A hello **összegzés** lapon tekintse át a hello részletes adatait a védelmi csoport tagjainak és a beállításokat, és kattintson a **csoport létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="f2b91-372">On hello **Summary** page, review hello details for your protection group members and settings, and then click **Create Group**.</span></span>

    ![Védelmi csoport tagja és beállítás összegzése](./media/backup-azure-backup-server-vmware/protection-group-summary.png)

## <a name="next-steps"></a><span data-ttu-id="f2b91-374">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f2b91-374">Next steps</span></span>
<span data-ttu-id="f2b91-375">Azure Backup Server tooprotect VMware munkaterhelések használja, ha szeretné használni Azure Backup Server toohelp védelméhez esetleg egy [Microsoft Exchange server](./backup-azure-exchange-mabs.md), egy [Microsoft SharePoint-farm](./backup-azure-backup-sharepoint-mabs.md), vagy egy [SQL Server-adatbázis](./backup-azure-sql-mabs.md).</span><span class="sxs-lookup"><span data-stu-id="f2b91-375">If you use Azure Backup Server tooprotect VMware workloads, you may be interested in using Azure Backup Server toohelp protect a [Microsoft Exchange server](./backup-azure-exchange-mabs.md), a [Microsoft SharePoint farm](./backup-azure-backup-sharepoint-mabs.md), or a [SQL Server database](./backup-azure-sql-mabs.md).</span></span>

<span data-ttu-id="f2b91-376">Információk a problémák hello ügynök regisztrálása hello védelmi csoport konfigurálásához, vagy biztonsági mentése feladat,: [hibaelhárítása Azure Backup Server](./backup-azure-mabs-troubleshoot.md).</span><span class="sxs-lookup"><span data-stu-id="f2b91-376">For information on problems with registering hello agent, configuring hello protection group, or backing up jobs, see [Troubleshoot Azure Backup Server](./backup-azure-mabs-troubleshoot.md).</span></span>
