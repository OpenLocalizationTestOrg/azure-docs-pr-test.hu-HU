---
title: "aaaUse DPM tooback mentése munkaterhelések tooAzure portál |} Microsoft Docs"
description: "Egy bevezető toobacking hello Azure Backup szolgáltatás használatával a DPM-kiszolgáló"
services: backup
documentationcenter: 
author: adigan
manager: nkolli
editor: 
keywords: "A System Center Data Protection Manager, a data protection manager, a dpm biztonsági mentése"
ms.assetid: c8c322cf-f5eb-422c-a34c-04a4801bfec7
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: adigan;giridham;jimpark;markgal;trinadhk
ms.openlocfilehash: 1dd988ae55012ac7dc485d2416458542c60b6ae3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="preparing-tooback-up-workloads-tooazure-with-dpm"></a>Mentése a dpm-mel munkaterhelések tooAzure tooback előkészítése
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
> * [Az Azure Backup Server (klasszikus)](backup-azure-microsoft-azure-backup-classic.md)
> * [SCDPM (klasszikus)](backup-azure-dpm-introduction-classic.md)
>
>

A cikkben egy bevezető toousing a Microsoft Azure Backup szolgáltatás tooprotect, a System Center Data Protection Manager (DPM) kiszolgálók és munkaterhelések. Az olvasásával lesz tisztában:

* Hogyan működik a Azure DPM-kiszolgáló biztonsági mentése
* hello Előfeltételek tooachieve egy zökkenőmentes biztonsági mentés élmény
* hello tipikus hibák történtek, és hogyan velük toodeal
* Támogatott helyzetek

> [!NOTE]
> Azure az erőforrások létrehozására és kezelésére két üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk ismerteti, hello telepített hello Resource Manager modellt használó virtuális gépek visszaállítására.
>
>

A System Center DPM fájl-és alkalmazásadatok készít. TooDPM biztonsági másolatba mentett adatok tárolása a lemezen, szalagon, vagy biztonsági másolat tooAzure a Microsoft Azure Backup szolgáltatásnál. A DPM a következőképpen együttműködik az Azure Backup szolgáltatással:

* **Fizikai kiszolgáló vagy a helyszíni virtuális gépként telepített DPM** – Ha a DPM fizikai kiszolgálóként vagy helyszíni Hyper-V virtuális gépként készíthet biztonsági mentést adatok tooa telepítése Recovery Services tároló továbbá toodisk, és a szalagos biztonsági mentés.
* **Az Azure virtuális gépként telepített DPM** – a System Center 2012 R2 Update 3, a DPM telepíthető Azure virtuális gépként. Ha a DPM központi telepítése egy Azure virtuális gépként is készítsen biztonsági másolatot az adatok tooAzure lemezek toohello DPM Azure virtuális géphez csatolt, vagy a Recovery Services-tároló tooa mentésével is kiszervezése a hello adattárolás.

## <a name="why-backup-from-dpm-tooazure"></a>Miért érdemes biztonsági másolatot készíteni a DPM tooAzure?
az Azure Backup használatával biztonsági mentéséről a DPM-kiszolgálók hello üzleti előnyöket nyújtja:

* A helyszíni DPM-telepítés Azure akár egy alternatív toolong-kifejezés telepítési tootape is használhatja.
* A DPM az Azure-ban történő telepítés esetén Azure Backup szolgáltatás lehetővé teszi hello Azure lemez toooffload tárhelyhez lehetővé tooscale mentése régebbi adatok tárolása a Recovery Services-tároló és a lemezen lévő új adatokat.

## <a name="prerequisites"></a>Előfeltételek
Az alábbiak szerint készítse elő a DPM-adatok Azure biztonsági mentés tooback:

1. **Recovery Services-tároló létrehozása** – Azure-portálon hozzon létre egy tárolót.
2. **Töltse le a tárolói hitelesítő adatokat** – hello hitelesítő adatokat használó tooregister hello DPM server tooRecovery szolgáltatások tároló letöltése.
3. **Telepítse az Azure Backup szolgáltatás ügynökének hello** – az Azure Backup szolgáltatásban hello ügynök telepítéséhez a DPM-kiszolgálókon.
4. **Hello kiszolgáló regisztrálása** – Register hello DPM server tooRecovery Services-tároló.

### <a name="1-create-a-recovery-services-vault"></a>1. Recovery Services-tároló létrehozása
a helyreállítás toocreate tároló szolgáltatások:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Hello központ menüben kattintson a **Tallózás** hello az erőforrások listájához, írja be a **Recovery Services**. Írja be megkezdése előtt, hello listát szűrheti a megadott feltételeknek. Kattintson a **Recovery Services-tároló** elemre.

    ![Recovery Services-tároló létrehozása – 1. lépés](./media/backup-azure-dpm-introduction/open-recovery-services-vault.png)

    Recovery Services-tárolók hello listája jelenik meg.
3. A hello **Recovery Services-tárolók** menüben kattintson a **Hozzáadás**.

    ![Recovery Services-tároló létrehozása – 2. lépés](./media/backup-azure-dpm-introduction/rs-vault-menu.png)

    hello Recovery Services tároló panel nyílik tooprovide kéri egy **neve**, **előfizetés**, **erőforráscsoport**, és **hely**.

    ![Recovery Services-tároló létrehozása – 5. lépés](./media/backup-azure-dpm-introduction/rs-vault-attributes.png)
4. A **neve**, adjon meg egy rövid nevet tooidentify hello tárolóban. hello nevének kell toobe egyedi hello Azure-előfizetés esetében. Írjon be egy 2–50 karakter hosszúságú nevet. Ennek egy betűvel kell kezdődnie, és csak betűket, számokat és kötőjeleket tartalmazhat.
5. Kattintson a **előfizetés** toosee hello rendelkezésre álló előfizetések listáját. Ha nem biztos abban, hogy melyik előfizetéssel toouse, használhatja az alapértelmezettet hello (vagy javasolt) előfizetés. Csak akkor lesz több választási lehetőség, ha a szervezetéhez tartozó fiók több Azure-előfizetéssel van összekötve.
6. Kattintson a **erőforráscsoport** toosee elérhető erőforráscsoportok listáját hello, vagy kattintson a **új** toocreate egy új erőforráscsoportot. Átfogó információk az erőforráscsoportokkal kapcsolatban: [Az Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).
7. Kattintson a **hely** tooselect hello földrajzi régióban hello tároló.
8. Kattintson a **Create** (Létrehozás) gombra. A Recovery Services-tároló létrehozása toobe hello egy ideig is igénybe vehet. Hello állapot értesítések hello jobb felső területen hello portálon figyelése.
   A tároló létrehozása után hello portálon jelenik meg.

### <a name="set-storage-replication"></a>Tárreplikáció beállítása
hello tárolási replikációs beállítás lehetővé teszi toochoose georedundáns tárolás és a helyileg redundáns tárolás között. Alapértelmezés szerint a tárolója georedundáns tárolással rendelkezik. Hello beállítás set toogeo redundáns tárolási hagyja, ha ez az elsődleges biztonsági mentés. Ha egy olcsóbb, rövidebb élettartamú megoldást szeretne, válassza a helyileg redundáns tárolást. Tudjon meg többet az [georedundáns](../storage/common/storage-redundancy.md#geo-redundant-storage) és [helyileg redundáns](../storage/common/storage-redundancy.md#locally-redundant-storage) tárolásáról: hello [Azure Storage replikáció – áttekintés](../storage/common/storage-redundancy.md).

tooedit hello tárolási replikációs beállítását:

1. Válassza ki a tároló tooopen hello tároló irányítópult hello-beállítások panelen. Ha hello **beállítások** panel nem nyitható meg **összes beállítás** hello tároló irányítópulton.
2. A hello **beállítások** panelen kattintson a **biztonsági infrastruktúra** > **biztonsági mentési konfigurációhoz** tooopen hello **biztonságimentésikonfigurációhoz** panelen. A hello **biztonsági mentési konfigurációhoz** panelen válassza ki a replikációs tárolómegoldást hello a tároló számára.

    ![A Backup-tárolók listája](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

    Miután kiválasztotta a tároló számára hello tárolási lehetőség, készen áll a tooassociate hello VM hello tárolóban. toobegin hello társítás, érdemes felderítése, és regisztrálja az Azure virtuális gépek hello.

### <a name="2-download-vault-credentials"></a>2. A tároló hitelesítő adatainak letöltése
hello tárolói hitelesítő adatok fájlját az hello portal minden egyes biztonsági mentési tároló által létrehozott tanúsítványt. hello portal majd feltölti a hello nyilvános kulcs toohello Access Control Service (ACS). hello hello tanúsítvány titkos kulcsának elérhető toohello felhasználói hello munkafolyamat, amely bemenetként hello gép regisztrációs munkafolyamat részeként történik. Ez akkor hitelesíti a hello azonosított tooan gép toosend biztonsági mentési adatokat tároló hello Azure Backup szolgáltatás.

hello tároló hitelesítő adatait csak hello regisztrációs munkamenet során használt. Hello felhasználó felelőssége tooensure, amely hello tárolói hitelesítő adatokat a fájl ne legyenek veszélyben. Ha bármely rosszindulatú felhasználó hello tagoknál esik, hello tároló hitelesítési adatait tartalmazó fájlt lehet használt tooregister más gépek ellen hello azonos tárolóban. Azonban mivel hello biztonsági mentési adatok titkosítása a egy jelszót, amit toohello ügyfél tartozik, meglévő biztonsági mentési adatok nem sérült. toomitigate ezen probléma tárolói hitelesítő adatok vannak beállítva tooexpire 48hrs. Letöltheti a hello tárolói hitelesítő adatokat a helyreállítási szolgáltatás tetszőleges számú alkalommal – azonban csak hello legújabb tárolói hitelesítő adatok fájlját alkalmazható hello regisztrációs munkamenet során.

hello tárolói hitelesítő adatok fájlját az Azure-portálon hello egy biztonságos csatornán keresztül letöltődik. hello Azure Backup szolgáltatás nem észleli a hello hello tanúsítvány titkos kulcsának és hello titkos kulcs nem őrzi meg hello portal vagy hello szolgáltatást. A következő lépéseket toodownload hello tárolói hitelesítő adatok fájl tooa helyi számítógép hello használata.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Nyissa meg Recovery Services tároló toowhich toowhich kívánt tooregister DPM-számítógépről.
3. Alapértelmezés szerint megnyílik beállítások panelen. Ha be van zárva, kattintson a **beállítások** a tároló irányítópult tooopen hello-beállítások panelen. A beállítások panelen, kattintson a **tulajdonságok**.

    ![Tároló panelének megnyitása](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)
4. Hello tulajdonságok lapján kattintson a **letöltése** alatt **biztonsági hitelesítő adatok**. hello portal hello tárolói hitelesítő adatok fájlját, amely letölthető állít elő.

    ![Letöltés](./media/backup-azure-dpm-introduction/vault-credentials.png)

hello portal kombinációjából hello tároló nevére és hello aktuális dátum tárolói hitelesítő adatot hoz létre. Kattintson a **mentése** toodownload hello tárolói hitelesítő adatok toohello helyi fiók Letöltések mappába, vagy a Mentés másként válassza ki a hello mentse menü toospecify hello tárolói hitelesítő adatok helyét. A foglalnak tooa perces hello fájl toobe jön létre.

### <a name="note"></a>Megjegyzés
* Győződjön meg arról, hogy hello tárolói hitelesítő adatok fájlja van egy helyre mentik el a számítógépről is elérhetők. Ha a megosztás/SMB fájl tárolja, ellenőrizze, hogy hello hozzáférési engedélyeket.
* hello tárolói hitelesítő adatok fájlja csak hello regisztrációs munkamenet során használt.
* hello tárolói hitelesítő adatok fájlját 48hrs után lejár, és hello portálról tölthető le.

### <a name="3-install-backup-agent"></a>3. Biztonsági mentési ügynök telepítése
Miután létrehozta a hello Azure Backup-tárolóban, az ügynök kell telepíteni mindegyik a Windows gép (Windows Server, a Windows ügyfél, a System Center Data Protection Manager-kiszolgáló vagy Azure biztonsági mentési kiszolgálóként működő számítógép), amely lehetővé teszi, hogy készítsen biztonsági másolatot az adat és alkalmazás tooAzure.

1. Nyissa meg Recovery Services tároló toowhich toowhich kívánt tooregister DPM-számítógépről.
2. Alapértelmezés szerint megnyílik beállítások panelen. Ha be van zárva, kattintson a **beállítások** tooopen hello-beállítások panelen. A beállítások panelen, kattintson a **tulajdonságok**.

    ![Tároló panelének megnyitása](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)
3. Hello beállítások lapján kattintson a **letöltése** alatt **Azure Backup szolgáltatás ügynökének**.

    ![Letöltés](./media/backup-azure-dpm-introduction/azure-backup-agent.png)

   Miután hello ügynök tölti le, kattintson duplán a MARSAgentInstaller.exe toolaunch hello telepítési hello Azure Backup szolgáltatás ügynökének. Válassza ki a hello telepítési mappát és az ideiglenes mappa hello ügynök szükséges. hello gyorsítótár helyére szabad területet, amely a biztonsági mentési adatok hello legalább 5 %-át kell rendelkeznie.
4. Ha a proxy server tooconnect toohello hello az internet **proxykonfigurációt** képernyőn, írja be a hello proxy kiszolgáló adatait. Ha egy hitelesített proxykiszolgálót használ, meg hello felhasználói nevet és jelszót adatait ezen a képernyőn.
5. hello Azure Backup szolgáltatás ügynökének telepíti a .NET-keretrendszer 4.5 és Windows PowerShell (Ha még nincs elérhető) toocomplete hello telepítés.
6. Hello ügynök telepítése után **Bezárás** hello ablak.

   ![Bezárás](../../includes/media/backup-install-agent/dpm_FinishInstallation.png)
7. túl**DPM-kiszolgáló regisztrálása hello** toohello tárolóban, a hello **felügyeleti** lapra, kattintson a **Online**. Ezt követően válassza **regisztrálása**. Az hello regisztrálása a telepítő varázsló nyílik meg.
8. Ha a proxy server tooconnect toohello hello az internet **proxykonfigurációt** képernyőn, írja be a hello proxy kiszolgáló adatait. Ha egy hitelesített proxykiszolgálót használ, meg hello felhasználói nevet és jelszót adatait ezen a képernyőn.

    ![Proxykiszolgáló-konfiguráció](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Proxy.png)
9. Keresse meg tooand válassza hello tároló hitelesítési adatait tartalmazó fájlt, amely korábban letöltött hello tárolói hitelesítő adatok képernyőn.

    ![Tárolói hitelesítő adatokat](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Credentials.jpg)

    hello tárolói hitelesítő adatok fájlja csak esetén érvényes 48 óra (miután hello portálról letöltés). Ha bármilyen hiba ezen a képernyőn (például "Hitelesítő adatok megadott fájlja lejárt tároló"), a bejelentkezési toohello az Azure portál és a letöltési hello tároló hitelesítési adatait tartalmazó fájlt ismét.

    Győződjön meg arról, hogy hello tároló hitelesítési adatait tartalmazó fájlt elérhető egy helyen hello telepítő alkalmazás is elérhetők. Ha eléréséhez kapcsolódó hibák, ez gépen hello tárolói hitelesítő adatok fájl tooa ideiglenes helyre másolja, majd próbálja megismételni hello műveletet.

    Ha érvénytelen tárolói hitelesítő adatok hibát észlel (például "érvénytelen tárolóban megadott hitelesítő adatok") hello fájl sérült, vagy nem rendelkezik hello hello helyreállítási szolgáltatáshoz tartozó legújabb hitelesítő adatokat. Zónák hello egy új tárolói hitelesítő adatok fájlját letöltése hello portálról. Ez a hiba általában látható, ha hello kattintva hello **letöltési tárolóhitelesítő adatokat** hello Azure-portálon, gyors egymásutánban beállítást. Ebben az esetben csak hello második tárolói hitelesítő adatok fájlját esetén érvényes.
10. hálózati sávszélesség és a munka, munkaidőn kívüli órákra, a hello toocontrol hello használatát **sávszélesség-szabályozás beállítása** képernyőn beállíthatja hello használati korlátjának és hello munkahelyi meghatározásához és munkaórákon kívüli időre.

    ![Szabályozó beállítás](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Throttling.png)
11. A hello **helyreállítási mappa beállítás** képernyőn, keresse meg a hello mappát, ahol a hello fájlok az Azure-ból letöltött ideiglenesen átmenetileg tárolva lesz.

    ![A helyreállítási mappa beállítás](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_RecoveryFolder.png)
12. A hello **titkosítási beállítás** képernyő, megadhat egy hozzáférési kódot létrehozni, vagy adjon meg egy jelszót (minimum 16 karakter). Ne felejtse el toosave hello jelszót biztonságos helyen.

    ![Titkosítás](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Encryption.png)

    > [!WARNING]
    > Ha hello jelszó elvesztése vagy elfelejtése; Nem a Microsoft hello biztonsági mentési adatok helyreállítása a Súgó. hello végfelhasználói hello titkosítási jelszó tulajdonában van, ezért a Microsoft nem látnak bele hello végfelhasználói által használt hello jelszót. Mentse hello fájlt biztonságos helyen szükség van egy helyreállítási művelet során.
    >
    >
13. Hello gombra kattintva **regisztrálása** gombra, hello számítógép regisztrálása sikeres toohello tárolóban, és most már áll készen toostart tooMicrosoft Azure biztonsági mentéséről.
14. A Data Protection Manager használatakor hello során megadott beállítások hello regisztrációs munkafolyamat hello kattintva módosíthatja **konfigurálása** lehetőség kiválasztásával **Online** alatt hello  **Felügyeleti** fülre.

## <a name="requirements-and-limitations"></a>Követelmények (és korlátozások)
* DPM fizikai kiszolgálóként vagy a System Center 2012 SP1 vagy a System Center 2012 R2 rendszeren telepített Hyper-V virtuális gép futtathat. Is futhat egy Azure virtuális gépként legalább fut a System Center 2012 R2 DPM 2012 R2 3. kumulatív frissítéssel vagy egy Windows virtuális gépként VMWare legalább fut a System Center 2012 R2 kumulatív frissítés 5.
* Ha a System Center 2012 SP1 DPM használ telepítenie kell frissítés visszaállítani a 2 a System Center Data Protection Manager SP1. Erre azért szükség, hello Azure Backup szolgáltatás ügynökének telepítése előtt.
* hello DPM-kiszolgálóval kell rendelkeznie a Windows PowerShell és a .net Framework 4.5 telepítve.
* A DPM biztonsági legtöbb munkaterhelések tooAzure biztonsági mentését is. A teljes listáját lásd: Mi van támogatott hello Azure biztonsági mentés támogatja az alábbi elemek.
* Azure biztonsági mentés tárolt adatok nem állíthatók hello "másolható tootape" beállítással.
* Hello Azure Backup szolgáltatás engedélyezve van az Azure-fiókra lesz szüksége. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információ a [Azure Backup árairól](https://azure.microsoft.com/pricing/details/backup/).
* Az Azure Backup használatához hello Azure Backup szolgáltatás ügynökének toobe tooback akarja hello kiszolgálókon telepítve. Minden kiszolgálónak hello adatok, amelyek biztonsági mentése van folyamatban, rendelkezésre álló szabad helyi tárolóként hello mérete legalább 5 %-át kell rendelkeznie. Például 5 GB szabad lemezterület hello ideiglenes helyen legalább 100 GB adat másolatának igényel.
* Adatokat tároló Azure storage hello tárolódnak. Nincs korlát toohello adatmennyiség készíthet biztonsági mentést tooan Azure Backup-tárolóban van, de az adatforrás (például egy virtuális géphez vagy adatbázis) hello mérete nem haladhatja meg a 54400 GB.

Ilyen típusú biztonsági mentését tooAzure támogatja:

* Titkosított (csak teljes biztonsági mentések)
* Tömörített (növekményes biztonsági mentések támogatva)
* Ritka (növekményes biztonsági mentések támogatva)
* Tömörített és ritka (Ritkaként kezelve)

És ezek nem támogatottak:

* A kis-és nagybetűket fájlrendszerek kiszolgálók nem támogatottak.
* A rögzített hivatkozások (kimaradnak)
* Újraelemzési pontok (kimaradnak)
* Titkosított és tömörített (kimaradnak)
* Titkosított és ritka (kimaradnak)
* Tömörített adatfolyam
* Ritka adatfolyam

> [!NOTE]
> A System Center 2012 DPM SP1 és újabb verziók esetében a készíthet biztonsági másolatot a Microsoft Azure Backup szolgáltatás használatával DPM tooAzure által védett munkaterhelések be.
>
>
