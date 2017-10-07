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
# <a name="back-up-a-vmware-server-tooazure"></a>Készítsen biztonsági másolatot a VMware server tooAzure

Ez a cikk azt ismerteti, hogyan tooconfigure Azure Backup Server toohelp VMware server munkaterhelések védelmét. Ez a cikk feltételezi, hogy már rendelkezik Azure Backup Server telepítve. Ha még nem rendelkezik telepített Azure Backup Server, lásd: [készítse elő a munkaterhelés Azure Backup Server mentése tooback](backup-azure-microsoft-azure-backup.md).

Az Azure Backup Server készítsen biztonsági másolatot, vagy védelme, a VMware vCenter Server verziója 6.5, 6.0 és 5.5.


## <a name="create-a-secure-connection-toohello-vcenter-server"></a>A biztonságos kapcsolat toohello vCenter-kiszolgáló létrehozása

Alapértelmezés szerint Azure Backup Server minden vCenter-kiszolgáló egy HTTPS-csatornán keresztül kommunikál. hello biztonságos kommunikáció tooturn, javasoljuk, hogy az Azure Backup Server telepítse hello VMware tanúsítvány hitelesítésszolgáltatói (CA) tanúsítvány. Ha nem feltétlenül szükséges a biztonságos kommunikációt, és inkább toodisable hello HTTPS követelmény, lásd: [letiltása biztonságos kommunikációs protokollt](backup-azure-backup-server-vmware.md#disable-secure-communication-protocol). toocreate egy biztonságos kapcsolatot az Azure Backup Server és hello vCenter-kiszolgáló, hello megbízható tanúsítvány az Azure Backup Server importálása.

Általában akkor használják a böngésző hello Azure Backup Server gép tooconnect toohello vcenter Server hello vSphere webes ügyfél keresztül. hello hello Azure Backup Server böngésző tooconnect toohello vCenter-kiszolgáló, első használatakor hello nem biztonságos. a következő kép hello hello nem biztonságos kapcsolat jeleníti meg.

![Nem biztonságos kapcsolat tooVMware kiszolgáló – példa](./media/backup-azure-backup-server-vmware/unsecure-url.png)

toofix erről a problémáról és biztonságos kapcsolat létrehozásához, hello megbízható legfelső szintű Hitelesítésszolgáltatói tanúsítvány letöltése.

1. Az Azure Backup Server hello böngészőben meg hello URL-cím toohello vSphere webes ügyfél. hello vSphere webes ügyfél bejelentkezési oldal jelenik meg.

    ![a vSphere webügyfél](./media/backup-azure-backup-server-vmware/vsphere-web-client.png)

    A rendszergazdák és fejlesztők hello információi hello alján található hello **letöltése megbízható legfelső szintű Hitelesítésszolgáltatói tanúsítványokat** hivatkozásra.

    ![Hivatkozás toodownload hello megbízható legfelső szintű Hitelesítésszolgáltatói tanúsítványok](./media/backup-azure-backup-server-vmware/vmware-download-ca-cert-prompt.png)

  Ha hello vSphere webes ügyfél bejelentkezési oldal nem jelenik meg, ellenőrizze a böngésző proxy beállításait.

2. Kattintson a **letöltése megbízható legfelső szintű Hitelesítésszolgáltatói tanúsítványokat**.

    hello vCenter-kiszolgáló letölti a fájl tooyour helyi számítógépen. hello fájl neve **letöltése**. Attól függően, hogy a böngésző kap egy üzenetet, amely rákérdez, hogy tooopen vagy hello fájl mentéséhez.

    ![üzenet letöltése a tanúsítványok lesznek letöltve](./media/backup-azure-backup-server-vmware/download-certs.png)

3. Azure Backup Server hello fájl tooa helyre mentheti. Hello fájl mentésekor hozzáadása hello .zip fájlnévkiterjesztést.

    hello fájl hello tanúsítványok hello információt tartalmazó .zip fájl. Hello .zip kiterjesztésű hello kibontási eszközök is használhatja.

4. Kattintson a jobb gombbal **download.zip**, majd válassza ki **összes kibontása** tooextract hello tartalmát.

    hello .zip fájl kibontása a tartalmak tooa nevű mappát **Tanúsítványos**. Két típusú fájlokat hello Tanúsítványos mappában jelennek meg. hello főtanúsítvány-fájl kiterjesztése például.0 és ikonra.1 számozott sorozatát kezdődik.
    
    hello CRL fájl kiterjesztése, amely egy sorozatban a hasonlóan .r0 vagy .r1 kezdődik. hello CRL-fájlnak a tanúsítvány hozzá rendelve.

    ![Töltse le a fájlt helyileg kibontása ](./media/backup-azure-backup-server-vmware/extracted-files-in-certs-folder.png)

5. A hello **Tanúsítványos** mappába, kattintson a jobb gombbal a hello főtanúsítvány-fájlt, majd kattintson a **átnevezése**.

    ![Nevezze át a legfelső szintű tanúsítvány ](./media/backup-azure-backup-server-vmware/rename-cert.png)

    Hello legfelső szintű tanúsítvány bővítmény too.crt módosítása. Még ha biztos abban, hogy toochange hello bővítmény szeretne, kattintson a kérdésnél **Igen** vagy **OK**. Ellenkező esetben hello fájl tervezett függvény változtatható meg. hello fájl módosításait tooan ikonjára, legfelső szintű tanúsítvány hello ikonjára.

6. Kattintson a jobb gombbal a legfelső szintű tanúsítvány hello és hello előugró menüjét, válassza a **tanúsítvány telepítése**.

    Hello **Tanúsítványimportáló varázsló** párbeszédpanel jelenik meg.

7. A hello **Tanúsítványimportáló varázsló** párbeszédpanelen jelölje ki **helyi számítógép** hello tanúsítványt, és kattintson a hello célként **tovább** toocontinue.

    ![Tanúsítvány tárolási cél beállításai ](./media/backup-azure-backup-server-vmware/certificate-import-wizard1.png)

    Ha megkérdezi, hogy ha azt szeretné, tooallow módosítások toohello számítógép, kattintson a **Igen** vagy **OK**, tooall hello módosításokat.

8. A hello **tanúsítványtároló** lapon jelölje be **minden tanúsítvány tárolása a következő tároló hello**, és kattintson a **Tallózás** toochoose hello tanúsítványtárolójába.

    ![Bizonyos tárolási helyet a tanúsítvány tárolása](./media/backup-azure-backup-server-vmware/cert-import-wizard-local-store.png)

    Hello **tanúsítványtároló kiválasztása** párbeszédpanel jelenik meg.

    ![Tanúsítványhierarchia tároló mappa](./media/backup-azure-backup-server-vmware/cert-store.png)

9. Válassza ki **megbízható legfelső szintű hitelesítésszolgáltatók** hello tanúsítványokat, és kattintson hello célmappát, **OK**.

    ![Tanúsítvány célmappájának](./media/backup-azure-backup-server-vmware/certificate-store-selected.png)

    Hello **megbízható legfelső szintű hitelesítésszolgáltatók** mappa Megerősítjük hello tanúsítvány tárolóként. Kattintson a **Tovább** gombra.

    ![Tanúsítvány tároló mappa](./media/backup-azure-backup-server-vmware/certificate-import-wizard2.png)

10. A hello **Tanúsítványimportáló varázsló befejezése hello** lapon győződjön meg arról, hogy hello tanúsítvány hello kívánt mappában, és kattintson a **Befejezés**.

    ![Ellenőrizze a tanúsítvány hello megfelelő mappában van](./media/backup-azure-backup-server-vmware/cert-wizard-final-screen.png)

    Megjelenik egy párbeszédpanel, hello sikeres tanúsítvány importálása ellenőrzése.

11. Jelentkezzen be toohello vCenter Server tooconfirm, amely a kapcsolat biztonságos.

  Ha hello tanúsítvány importálása nem sikeres, és nem tud biztonságos kapcsolatot, dokumentációt hello VMware vSphere a [kiszolgálótanúsítványok](http://pubs.vmware.com/vsphere-60/index.jsp#com.vmware.wssdk.dsg.doc/sdk_sg_server_certificate_Appendixes.6.4.html).

  Ha biztonságos határai vannak a szervezeten belül, és nem szeretné, hogy a HTTPS protokoll hello tooturn, használja a következő eljárás toodisable hello biztonságos kommunikáció hello.

### <a name="disable-secure-communication-protocol"></a>Tiltsa le a biztonságos kommunikációs protokollja

Ha a szervezet hello HTTPS protokoll nem igényel, használja a következő lépéseket toodisable HTTPS hello. toodisable hello alapértelmezett viselkedését, és hozzon létre egy beállításkulcsot, amely figyelmen kívül hagyja az alapértelmezett viselkedés hello.

1. Másolja és illessze be a szöveget egy .txt fájlban a következő hello.

  ```
  Windows Registry Editor Version 5.00
  [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager\VMWare]
  "IgnoreCertificateValidation"=dword:00000001
  ```

2. Mentse a hello fájl tooyour Azure Backup Server számítógéphez. Hello fájlnév DisableSecureAuthentication.reg használja.

3. Kattintson duplán a hello fájl tooactivate hello bejegyzésre.


## <a name="create-a-role-and-user-account-on-hello-vcenter-server"></a>Hozzon létre egy szerepkör és a felhasználói fiókot hello vCenter-kiszolgáló

Hello vcenter Server a szerepkör az előre meghatározott jogosultságokat. A vCenter-kiszolgáló rendszergazdája hello szerepkörök hoz létre. tooassign engedélyek hello rendszergazdai szerepkörrel rendelkező felhasználói fiókok párokat. tooestablish hello szükséges felhasználói hitelesítő adatok tooback hello vCenter-kiszolgáló számítógép, a szerepkör létrehozása adott jogosultságokkal, és majd hello felhasználói fiók társítása hello szerepkör.

Az Azure Backup Server egy felhasználónév és jelszó tooauthenticate hello vCenter-kiszolgáló használ. Az Azure Backup Server ezeket a hitelesítő adatokat használ az összes biztonsági mentési műveletek hitelesítésként.

tooadd egy vCenter-kiszolgáló szerepkör és a biztonsági mentési rendszergazda jogosultsággal:

1. Jelentkezzen be a toohello vCenter-kiszolgáló, majd a hello vCenter-kiszolgáló **Navigator** panelen, kattintson a **felügyeleti**.

    ![A vCenter Server Navigator panelen rendszergazdai beállítás](./media/backup-azure-backup-server-vmware/vmware-navigator-panel.png)

2. A **felügyeleti** válasszon **szerepkörök**, majd a hello **szerepkörök** panelen kattintson hello szerepkör ikon (hello + szimbólumra) hozzáadása.

    ![Szerepkör hozzáadása](./media/backup-azure-backup-server-vmware/vmware-define-new-role.png)

    Hello **szerepkör létrehozása** párbeszédpanel jelenik meg.

    ![Szerepkör létrehozása](./media/backup-azure-backup-server-vmware/vmware-define-new-role-priv.png)

3. A hello **szerepkör létrehozása** párbeszédpanel hello **szerepkörnév** adja meg a *BackupAdminRole*. hello szerepkör neve lehet bármilyen, de felismerhető hello szerepkör célra kell lennie.

4. Válassza ki a vCenter megfelelő verziójának hello hello jogosultságokkal, és kattintson a **OK**. a következő táblázat hello azonosítja a vCenter 6.0 és vCenter 5.5 hello szükséges jogosultságokkal.

  Hello jogosultságokkal kiválasztásakor kattintson hello ikon következő toohello szülő címke tooexpand hello szülő- és nézet hello gyermek jogosultságokkal. tooselect hello VirtualMachine jogosultságokkal kell toogo hello be több szintet szülő-gyermek hierarchia. Nincs szükség a tooselect belül szülő jogosultság az összes alárendelt jogosultság.

  ![Szülő-gyermek jogosultság hierarchiát](./media/backup-azure-backup-server-vmware/cert-add-privilege-expand.png)

  Miután rákattintott **OK**, hello új szerepkör hello szerepkörök panelen hello listájában jelenik meg.

|Vcenter 6.0 jogosultságokkal| Vcenter 5.5 jogosultságokkal|
|--------------------------|---------------------------|
|Datastore.AllocateSpace   | Datastore.AllocateSpace|
|Global.ManageCustomFields | Global.ManageCustomerFields|
|Global.SetCustomFields    |   |
|Host.Local.CreateVM       | Network.Assign |
|Network.Assign            |  |
|Resource.AssignVMToPool   |  |
|VirtualMachine.Config.AddNewDisk  | VirtualMachine.Config.AddNewDisk   |
|VirtualMachine.Config.AdvanceConfig| VirtualMachine.Config.AdvancedConfig|
|VirtualMachine.Config.ChangeTracking| VirtualMachine.Config.ChangeTracking |
|VirtualMachine.Config.HostUSBDevice||
|VirtualMachine.Config.QueryUnownedFiles|    |
|VirtualMachine.Config.SwapPlacement| VirtualMachine.Config.SwapPlacement |
|VirtualMachine.Interact.PowerOff| VirtualMachine.Interact.PowerOff |
|VirtualMachine.Inventory.Create| VirtualMachine.Inventory.Create |
|VirtualMachine.Provisioning.DiskRandomAccess| |
|VirtualMachine.Provisioning.DiskRandomRead|VirtualMachine.Provisioning.DiskRandomRead |
|VirtualMachine.State.CreateSnapshot| VirtualMachine.State.CreateSnapshot|
|VirtualMachine.State.RemoveSnapshot|VirtualMachine.State.RemoveSnapshot |
</br>



## <a name="create-a-vcenter-server-user-account-and-permissions"></a>A vCenter Server felhasználói fiókjával és engedélyeivel létrehozása

Jogosultságokkal hello szerepkör telepítése után, a felhasználói fiók létrehozása. hello felhasználói fiók rendelkezik, egy nevet és jelszót, így a hitelesítéshez használt hello hitelesítő adatait.

1. egy felhasználói fiókot, a hello vCenter Server toocreate **Navigator** panelen, kattintson a **felhasználók és csoportok**.

    ![Felhasználók és csoportok beállítás](./media/backup-azure-backup-server-vmware/vmware-userandgroup-panel.png)

    Hello **vCenter felhasználók és csoportok** panel jelenik meg.

    ![vCenter-felhasználók és csoportok panelen](./media/backup-azure-backup-server-vmware/usersandgroups.png)

2. A hello **vCenter felhasználók és csoportok** panelen, jelölje be hello **felhasználók** fülre, majd hello felhasználók ikon (hello + szimbólumra) hozzáadása.

    Hello **új felhasználó** párbeszédpanel jelenik meg.

3. A hello **új felhasználó** párbeszédpanel mezőbe, adja hozzá a hello felhasználói adatokat, majd kattintson **OK**. Ezzel az eljárással hello felhasználónév BackupAdmin.

    ![Új felhasználó párbeszédpanel](./media/backup-azure-backup-server-vmware/vmware-new-user-account.png)

    új felhasználói fiók hello hello listájában jelenik meg.

4. tooassociate hello felhasználói fiók hello szerepkörrel, a hello **Navigator** panelen, kattintson a **globális engedélyek**. A hello **globális engedélyek** panelen, jelölje be hello **kezelése** fülre, majd hello hozzáadása (hello + szimbólumra) ikonra.

    ![Globális engedélyek panel](./media/backup-azure-backup-server-vmware/vmware-add-new-perms.png)

    Hello **globális engedélyek gyökér - engedély hozzáadása** párbeszédpanel jelenik meg.

5. A hello **globális engedély gyökér - engedély hozzáadása** párbeszédpanel, kattintson a **Hozzáadás** toochoose hello felhasználó vagy csoport.

    ![Felhasználó vagy csoport kiválasztása](./media/backup-azure-backup-server-vmware/vmware-add-new-global-perm.png)

    Hello **felhasználók vagy csoportok kiválasztása** párbeszédpanel jelenik meg.

6. A hello **felhasználók vagy csoportok kiválasztása** párbeszédpanelen válassza ki **BackupAdmin** majd **hozzáadása**.

    A **felhasználók**, hello *tartomány\felhasználónév* formátum hello felhasználói fiók kerül használatra. Ha azt szeretné, hogy egy másik tartományba toouse, válassza ki, a hello **tartomány** listája.

    ![BackupAdmin felhasználó hozzáadása](./media/backup-azure-backup-server-vmware/vmware-assign-account-to-role.png)

    Kattintson a **OK** tooadd hello kijelölt felhasználók toohello **hozzáadása engedély** párbeszédpanel megnyitásához.

7. Most, hogy hello felhasználói állapította meg, hello felhasználói toohello szerepkör hozzárendelése. A **hozzárendelt szerepkör**, hello legördülő listában jelölje ki **BackupAdminRole**, és kattintson a **OK**.

    ![Rendelje hozzá a felhasználó toorole](./media/backup-azure-backup-server-vmware/vmware-choose-role.png)

  A hello **kezelése** hello lapján **globális engedélyek** panelen, a hello új felhasználói fiók és a kapcsolódó hello szerepkör hello listájában jelennek meg.


## <a name="establish-vcenter-server-credentials-on-azure-backup-server"></a>VCenter Server-felhasználó hitelesítő adatai Azure Backup Server létrehozása

Hello VMware server tooAzure biztonsági mentés kiszolgáló felvétele előtt telepítse [1. frissítés az Azure Backup Server](https://support.microsoft.com/help/3175529/update-1-for-microsoft-azure-backup-server).

1. Azure Backup Server tooopen ikonra hello hello Azure Backup Server asztalon.

    ![Az Azure Backup Server ikonja](./media/backup-azure-backup-server-vmware/mabs-icon.png)

    Ha hello ikon hello asztal nem találja, nyissa meg a Azure Backup Server hello listából a telepített alkalmazások. hello Azure Backup Server alkalmazás neve a Microsoft Azure Backup szolgáltatás neve.

2. Hello Azure Backup Server konzolon kattintson **felügyeleti**, kattintson a **az üzemi kiszolgálók**, majd hello eszközsávon kattintson **kezelése VMware**.

    ![Az Azure Backup Server konzol](./media/backup-azure-backup-server-vmware/add-vmware-credentials.png)

    Hello **hitelesítő adatok kezelése** párbeszédpanel jelenik meg.

    ![Az Azure Backup Server hitelesítő adatok kezelése párbeszédpanel](./media/backup-azure-backup-server-vmware/mabs-manage-credentials-dialog.png)

3. A hello **hitelesítő adatok kezelése** párbeszédpanel, kattintson a **Hozzáadás** tooopen hello **adja hozzá a hitelesítő adatok** párbeszédpanel.

4. A hello **adja hozzá a hitelesítő adatok** párbeszédpanelen adja meg egy nevet és leírást a hello új hitelesítő adatok. Majd adja meg a hello felhasználónevet és jelszót. hello nevét, *Contoso Vcenter hitelesítőadat* használja a következő eljárással hello tooidentify hello hitelesítő adat. Használjon hello azonos felhasználónév és jelszó, amellyel hello vCenter-kiszolgáló. Ha hello vCenter-kiszolgáló és az Azure Backup Server nem a hello ugyanabban a tartományban, a **felhasználónév**, adja meg a hello tartományát.

    ![Az Azure Backup Server hozzáadása hitelesítő párbeszédpanel](./media/backup-azure-backup-server-vmware/mabs-add-credential-dialog2.png)

    Kattintson a **Hozzáadás** tooadd hello új hitelesítő adatok tooAzure Server biztonsági másolat. hello új hitelesítő adatok hello hello listájában jelenik meg **hitelesítő adatok kezelése** párbeszédpanel megnyitásához.
    
    ![Az Azure Backup Server hitelesítő adatok kezelése párbeszédpanel](./media/backup-azure-backup-server-vmware/new-list-of-mabs-creds.png)

5. tooclose hello **hitelesítő adatok kezelése** párbeszédpanelen kattintson az hello **X** hello jobb felső sarokban.


## <a name="add-hello-vcenter-server-tooazure-backup-server"></a>Hello vCenter Server tooAzure biztonsági mentés kiszolgáló hozzáadása

Az üzemi kiszolgáló hozzáadása varázsló használt tooadd hello vCenter Server tooAzure Backup Server.

tooopen üzemi kiszolgáló hozzáadása varázsló, a következő eljárás teljes hello:

1. Hello Azure Backup Server konzolon kattintson **felügyeleti**, kattintson a **az üzemi kiszolgálók**, és kattintson a **Hozzáadás**.

    ![Nyissa meg az üzemi kiszolgáló hozzáadása varázsló](./media/backup-azure-backup-server-vmware/add-vcenter-to-mabs.png)

    Hello **üzemi kiszolgáló hozzáadása varázsló** párbeszédpanel jelenik meg.

    ![Az üzemi kiszolgáló hozzáadása varázsló](./media/backup-azure-backup-server-vmware/production-server-add-wizard.png)

2. A hello **válassza ki az üzemi kiszolgáló típusa** lapon, hogy melyik **VMware Server**, és kattintson a **tovább**.

3. A **kiszolgáló neve vagy IP-címet**, adja meg a hello teljesen minősített tartománynevét (FQDN) vagy hello VMware-kiszolgáló IP-címét. Ha minden hello ESXi-kiszolgálókat kezeli-e hello eltérő vCenter hello vCenter neve is használhatja.

    ![Adja meg a VMware server FQDN vagy IP-címe](./media/backup-azure-backup-server-vmware/add-vmware-server-provide-server-name.png)

4. A **SSL-Port**, adja meg a hello portot, amelyet használt toocommunicate hello VMware-kiszolgálóval. Port 443-as, amely hello alapértelmezett portot, hacsak nem tudja, hogy szükség-e egy másik portot használja.

5. A **adja meg a hitelesítő adatok**, válassza ki a korábban létrehozott hitelesítő hello.

    ![Hitelesítő adatainak megadása](./media/backup-azure-backup-server-vmware/identify-creds.png)

6. Kattintson a **Hozzáadás** tooadd hello VMware server toohello listája **hozzá VMware-kiszolgálók**, és kattintson a **következő** toomove toohello varázsló következő lapjára hello.

    ![VMWare-kiszolgáló és a hitelesítő adatok hozzáadása](./media/backup-azure-backup-server-vmware/add-vmware-server-credentials.png)

7. A hello **összegzés** kattintson **hozzáadása** tooadd hello megadott VMware server tooAzure Server biztonsági másolat.

    ![Adja hozzá a VMware server tooAzure kiszolgáló biztonsági mentése](./media/backup-azure-backup-server-vmware/tasks-screen.png)

  hello VMware server biztonsági másolat egy ügynök nélküli biztonsági mentést, és új kiszolgáló hello jelenik meg azonnal. Hello **Befejezés** mutat be, hogy hello eredmények lapon.

  ![Befejezés lapra](./media/backup-azure-backup-server-vmware/summary-screen.png)

  vCenter Server tooAzure Backup Server, ismétlődő hello előző több példánya ebben a szakaszban ismertetett visszaállítási lépésekkel tooadd.

Miután hozzáadta a hello vCenter Server tooAzure Server biztonsági másolat, hello következő lépésre toocreate egy védelmi csoportot. hello védelmi csoport határozza meg, hello rövid és hosszú távú megőrzési különböző részleteit, és határozza meg, és ahol a hello biztonsági mentési házirend alkalmazása. a biztonsági mentési házirend hello hello ütemezését Ha biztonsági mentések fordulnak elő, és milyen biztonsági mentése.


## <a name="configure-a-protection-group"></a>A védelmi csoportok beállítása

Ha még nem használta a System Center Data Protection Manager vagy az Azure Backup Server előtt, tekintse meg [lemezes biztonsági mentések tervezése](https://technet.microsoft.com/library/hh758026.aspx) tooprepare a Hardverkörnyezet. Után ellenőrizheti, hogy rendelkezik-e megfelelő tárolását, használja a hello új védelmi csoport létrehozása varázsló tooadd VMware virtuális gépeket.

1. Hello Azure Backup Server konzolon kattintson **védelmi**, és kattintson a menüszalagon hello, **új** tooopen hello új védelmi csoport létrehozása varázsló.

    ![Nyissa meg hello új védelmi csoport létrehozása varázsló](./media/backup-azure-backup-server-vmware/open-protection-wizard.png)

    Hello **új védelmi csoport létrehozása** varázsló párbeszédpanel jelenik meg.

    ![Új védelmi csoport létrehozása varázsló párbeszédpanelje](./media/backup-azure-backup-server-vmware/protection-wizard.png)

    Kattintson a **következő** tooadvance toohello **védelmi csoport típusának kiválasztása** lap.

2. A hello **válassza ki a védelmi csoport típusának** lapon, hogy melyik **kiszolgálók** , majd **következő**. Hello **csoporttagok kiválasztása** lap jelenik meg.

3. A hello **csoporttagok kiválasztása** oldal, a választható tagok hello és a kiválasztott hello tagok jelennek meg. Válassza ki a kívánt tooprotect, és kattintson hello tagokat **következő**.

    ![Csoporttagok kiválasztása](./media/backup-azure-backup-server-vmware/server-add-selected-members.png)

    Felhasználót, ha más mappákban vagy a virtuális gépeket tartalmazó mappa kiválasztásakor ezen mappák és a virtuális gépek is ki lesz választva. hello felvétel hello mappák és a virtuális gépek által hello szülőmappa neve mappa szintű védelmet. tooremove egy mappa vagy a virtuális gép, törölje a jelet hello jelölőnégyzetet.

    Ha egy virtuális Gépet, vagy egy mappát, amely tartalmazza a virtuális gép már védett tooAzure, nem választhat ki ezt a virtuális Gépet újra. Ez azt jelenti, hogy miután a virtuális gép védett tooAzure, ezért nem is védheti újra, amely megakadályozza, hogy ismétlődő helyreállítási pontokat hoz létre egy virtuális gép. Ha azt szeretné, hogy melyik Azure Backup Server-példány már védi a tag, pont toohello tag toosee hello neve kiszolgálót védő hello toosee.

4. A hello **adatvédelmi módszer kiválasztása** lapján adjon meg egy nevet hello védelmi csoportra vonatkozóan. Rövid távú védelem (toodisk) és az online védelem van kiválasztva. Ha azt szeretné, hogy az online védelem toouse (tooAzure), a rövid távú védelem toodisk kell használnia. Kattintson a **következő** tooproceed toohello rövid távú védelmi ideje.

    ![Adatvédelmi módszer kiválasztása](./media/backup-azure-backup-server-vmware/name-protection-group.png)

5. A hello **rövid távú célok megadása** lap, a **megőrzési időtartam**, adja meg a kívánt tooretain helyreállítási pontok megadott napok száma hello *toodisk tárolt*. Toochange hello idő és a nap, amikor helyreállítási pontokat készít, kattintson a **módosítás**. hello rövid távú helyreállítási pontok teljes biztonsági mentés. Nincsenek növekményes biztonsági mentést. Ha elégedett hello rövid távú célok, kattintson **következő**.

    ![Rövid távú célok megadása](./media/backup-azure-backup-server-vmware/short-term-goals.png)

6. A hello **tekintse át a lemezfoglalás** lapon ellenőrizze, és szükség esetén módosítsa a virtuális gépek hello hello lemezterület. hello ajánlott lemezfoglalásokat hello megőrzési tartomány megadott hello alapuló **rövid távú célok megadása** hello munkaterhelés típusától és hello hello mérete védett adatokat (a 3. lépésben azonosított) lapon.  

  - **Adatok mérete:** hello védelmi csoport adatainak hello méretét.
  - **Szabad lemezterület:** hello ajánlott lemezterület hello védelmi csoportra vonatkozóan. Ha azt szeretné, toomodify ezt a beállítást, minden adatforrás növekedésének becslései hello összeg valamivel nagyobb helyet kell lefoglalni.
  - **Tartozó adatok közös elhelyezése:** bekapcsolja a közös elhelyezést, ha több adatforrás hello védelmi leképezheti tooa egyetlen replika- és helyreállításipont-köteten. Közös elhelyezés nem minden munkaterhelésnél támogatott.
  - **Méretének automatikus növelése:** Ha bekapcsolja ezt a beállítást, ha a védett hello csoport adatainak kezdeti foglalás hello meghaladja, a System Center Data Protection Manager tooincrease hello lemezméretet megpróbál 25 %-kal.
  - **Tárolókészlet részletei:** hello tárolókészlet, beleértve a teljes és szabad lemezterülettel hello állapotát jeleníti meg.

    ![Tekintse át a lemezkiosztást](./media/backup-azure-backup-server-vmware/review-disk-allocation.png)

    Ha elégedett hello lemezterület-foglalás, kattintson **következő**.

7. A hello **replika-létrehozási módszer kiválasztása** lapján adja meg, hogyan toogenerate hello kezdeti másolatot, vagy az Azure Backup Server hello védett adatok replikáját,.

    hello alapértelmezett érték a **automatikusan hello hálózaton keresztül** és **most**. Hello alapértelmezett használ, azt javasoljuk, hogy megadja a csúcsidőn kívüli időpontot. Válasszon **később** , és adja meg a napját és időpontját.

    Nagy mennyiségű adatok vagy kevésbé optimális hálózati körülmények esetén érdemes megfontolni adatreplikálás hello offline cserélhető adathordozó használatával.

    Miután elvégezte a választott, kattintson a **következő**.

    ![A replika-létrehozási módszer kiválasztása](./media/backup-azure-backup-server-vmware/replica-creation.png)

8. A hello **konzisztencia-ellenőrzési beállítások** lapon válassza ki hogyan és mikor tooautomate hello konzisztencia-ellenőrzést. Konzisztencia-ellenőrzéseket is futtathatja, ha a replikaadatok inkonzisztenssé válik, vagy adott ütemezés szerinti.

    Ha nem szeretné tooconfigure automatikus konzisztencia-ellenőrzést, egy manuális ellenőrzést is futtathatja. Hello védelmi hello Azure Backup Server konzol, kattintson a jobb gombbal a hello védelmi csoportot, és válassza ki **konzisztencia-ellenőrzés**.

    Kattintson a **következő** toomove toohello következő oldalra.

9. A hello **Online védelem adatainak megadása** lapon, válassza ki a megjeleníteni kívánt tooprotect egy vagy több adatforrást. Válassza ki egyenként hello tagokat, vagy kattintson a **kijelölése az összes** toochoose összes tagjához. Hello tagok kiválasztása után kattintson **következő**.

    ![Online védelem adatainak megadása](./media/backup-azure-backup-server-vmware/select-data-to-protect.png)

10. A hello **Online biztonsági mentés ütemezésének megadása** adja meg azokat a hello ütemezés toogenerate helyreállítási pontok hello biztonsági másolat. Miután hello helyreállítási pont jön létre, az Azure Recovery Services-tároló átvitt toohello áll. Ha elégedett hello online biztonsági mentési ütemezést, kattintson **következő**.

    ![Adja meg az online biztonsági mentés ütemezése](./media/backup-azure-backup-server-vmware/online-backup-schedule.png)

11. A hello **Online adatmegőrzési szabály megadása** lapján adja meg, hogy mennyi ideig tooretain hello biztonsági mentési adatokat az Azure-ban. Miután hello házirend lett meghatározva, kattintson **következő**.

    ![Online megőrzési szabály megadása](./media/backup-azure-backup-server-vmware/retention-policy.png)

    Nincs nincs határideje, mennyi ideig tárolhatja az adatokat az Azure-ban. Ha a helyreállítási pont adatait az Azure-ban tárolja, hello csak határértéke, hogy rendelkezik-e nem védett példányonként legfeljebb 9999 helyreállítási pontokat. Ebben a példában hello védett példány hello VMware server.

12. A hello **összegzés** lapon tekintse át a hello részletes adatait a védelmi csoport tagjainak és a beállításokat, és kattintson a **csoport létrehozása**.

    ![Védelmi csoport tagja és beállítás összegzése](./media/backup-azure-backup-server-vmware/protection-group-summary.png)

## <a name="next-steps"></a>Következő lépések
Azure Backup Server tooprotect VMware munkaterhelések használja, ha szeretné használni Azure Backup Server toohelp védelméhez esetleg egy [Microsoft Exchange server](./backup-azure-exchange-mabs.md), egy [Microsoft SharePoint-farm](./backup-azure-backup-sharepoint-mabs.md), vagy egy [SQL Server-adatbázis](./backup-azure-sql-mabs.md).

Információk a problémák hello ügynök regisztrálása hello védelmi csoport konfigurálásához, vagy biztonsági mentése feladat,: [hibaelhárítása Azure Backup Server](./backup-azure-mabs-troubleshoot.md).
