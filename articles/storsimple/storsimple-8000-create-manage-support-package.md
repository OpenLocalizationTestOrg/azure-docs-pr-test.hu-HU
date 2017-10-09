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
# <a name="create-and-manage-a-support-package-for-storsimple-8000-series"></a>Hozzon létre és kezelheti a StorSimple 8000 Series támogatási csomag

## <a name="overview"></a>Áttekintés

A StorSimple támogatási csomag olyan könnyen használható mechanizmus, amely az összes releváns naplók tooassist Microsoft Support gyűjti a hibaelhárításban a StorSimple eszköz probléma merül fel. hello gyűjtött naplók titkosított és tömörített.

Ez az oktatóanyag részletesen toocreate tartalmazza, és a StorSimple 8000 series eszköz hello támogatási csomag kezelése. Ha egy StorSimple virtuális tömbbel rendelkező dolgozik, nyissa meg túl[a napló csomagot](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="create-a-support-package"></a>Hozzon létre egy támogatási csomag

Néhány esetben szüksége lesz toomanually StorSimple hello támogatási csomag a Windows PowerShell segítségével hozzon létre. Példa:

* Ha a naplóból tooremove bizalmas adatokat kell fájlok Microsoft Support előzetes toosharing.
* Ha a probléma miatt tooconnectivity problémák hello csomag feltöltése.

A manuálisan létrehozott támogatási csomag Microsoft Support keresztül e-mailek megoszthatja. Hajtsa végre a következő lépéseket toocreate egy támogatási csomag a Windows PowerShellben a StorSimple hello.

#### <a name="toocreate-a-support-package-in-windows-powershell-for-storsimple"></a>toocreate egy támogatási csomag a Windows PowerShell-lel

1. Windows PowerShell-munkamenet hello távoli számítógépen tooconnect tooyour StorSimple eszköz által használt rendszergazdaként toostart adja meg a hello a következő parancsot:
   
    `Start PowerShell`
2. A Windows PowerShell-munkamenetben hello csatlakozás toohello SSAdmin konzol, az eszköz:
   
   1. Hello parancssorba írja be:
     
       `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`
   2. Hello párbeszédpanel adja meg az eszköz rendszergazdai jelszava. hello alapértelmezett jelszó _jelszó1_.
     
      ![PowerShell-hitelesítő adat párbeszédpanel](./media/storsimple-8000-create-manage-support-package/IC740962.png)
   3. Kattintson az **OK** gombra.
   4. Hello parancssorba írja be:
     
      `Enter-PSSession $MS`
3. A megnyitott munkamenetben hello írja be a hello megfelelő parancsot.
   
   * Jelszóval védett hálózati megosztások adja meg:
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`
     
       Kéri a jelszót, a elérési toohello hálózati megosztott mappából és a titkosítás jelszavát (mert hello támogatási csomag titkosított). Egy támogatási csomag majd hello megadott mappában jön létre.
   * Az megosztások, amelyek nem jelszóval védett, nem kell hello `-Credential` paraméter. Írja be a következő hello:
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`
     
       hello támogatási csomag mindkét tartományvezérlők hello megadott hálózati megosztott mappában jön létre. Egy titkosított, tömörített fájlt, amely elküldhető tooMicrosoft támogatási hibaelhárítási. További információkért lásd: [forduljon a Microsoft ügyfélszolgálatához](storsimple-8000-contact-microsoft-support.md).

### <a name="hello-export-hcssupportpackage-cmdlet-parameters"></a>Export-HcsSupportPackage hello parancsmag-paraméterek

A következő paraméterek hello Export-HcsSupportPackage parancsmaggal hello is használhatja.

| Paraméter | Kötelező/választható | Leírás |
| --- | --- | --- |
| `-Path` |Szükséges |Hello hálózati megosztott mappa mely hello támogatási csomag helyet adó tooprovide hello helyét használja. |
| `-EncryptionPassphrase` |Szükséges |Használjon tooprovide egy hozzáférési kódot toohelp hello támogatási csomag titkosításához. |
| `-Credential` |Optional |Hálózati megosztott mappa hello toosupply hozzáférési hitelesítő adatok használatával. |
| `-Force` |Optional |Tooskip hello titkosítási jelszót megerősítési lépés használható. |
| `-PackageTag` |Optional |Használjon toospecify olyan könyvtárat *elérési* mely hello támogatott csomag kerül. hello alapértelmezett érték a [eszköznév]-[aktuális dátumot és time:yyyy-MM-dd-HH-mm-ss]. |
| `-Scope` |Optional |Adja meg a **fürt** (alapértelmezett) toocreate egy támogatási csomag mindkét vezérlők. Ha azt szeretné, csak az aktuális vezérlő hello toocreate egy csomagot, adjon meg **vezérlő**. |

## <a name="edit-a-support-package"></a>Egy támogatási csomag szerkesztése

Miután létrehozta a támogatási csomag, szükség lehet a tooedit hello csomag tooremove bizalmas adatokat. Ilyen lehet például a kötet, eszköz IP-címek vagy hello naplófájlokból biztonsági másolat neve.

> [!IMPORTANT]
> Csak akkor szerkeszthető egy támogatási csomag, amelyik a Windows PowerShell segítségével a StorSimple jött létre. Hello StorSimple Device Manager szolgáltatás Azure-portálon létrehozott csomagot nem szerkeszthetők.

tooedit egy támogatási csomag hello Microsoft Support webhelyén, a feltöltés előtt először visszafejtéséhez hello támogatási csomag hello fájlok szerkesztése és újbóli titkosítására a azt. Hajtsa végre a következő lépéseket hello.

#### <a name="tooedit-a-support-package-in-windows-powershell-for-storsimple"></a>tooedit egy támogatási csomag a Windows PowerShell-lel

1. Egy támogatási csomagot leírtak korábban [toocreate egy támogatási csomag a Windows PowerShell-lel](#to-create-a-support-package-in-windows-powershell-for-storsimple).
2. [Töltse le a hello parancsfájl](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) helyileg az ügyfélen.
3. Hello Windows PowerShell-modul importálásához. Adja meg a hello elérési toohello helyi mappát, amelyben hello parancsprogram letöltése. tooimport hello modul, írja be:
   
    `Import-module <Path toohello folder that contains hello Windows PowerShell script>`
4. Összes hello fájl *.aes* titkosított és tömörített fájlok. toodecompress és visszafejtése fájlokat, írja be:
   
    `Open-HcsSupportPackage <Path toohello folder that contains support package files>`
   
    Vegye figyelembe, hogy hello tényleges fájlkiterjesztések jelennek meg az összes hello.
   
    ![Támogatási csomag szerkesztése](./media/storsimple-8000-create-manage-support-package/IC750706.png)
5. Amikor hello titkosítási jelszót kéri, adja meg a hello hello támogatási csomag létrehozásakor használt jelszót.
   
        cmdlet Open-HcsSupportPackage at command pipeline position 1
   
        Supply values for hello following parameters:EncryptionPassphrase: ****
6. Keresse meg a hello naplófájlokat tartalmazó toohello mappát. Hello naplófájlok most kibontása és visszafejtése, mert ezek lesz az eredeti fájlkiterjesztés. Módosítsa a fájlok tooremove bármely adatok, például a kötet neve és az eszköz IP-címek, és hello fájlokat menteni.
7. Bezárás hello fájlok toocompress őket a gzip és az AES-256 titkosítani. Ez a sebesség és a biztonság hello támogatási csomag átvitele a hálózaton keresztül. toocompress és titkosíthatják a fájlokat, írja be a következő hello:
   
    `Close-HcsSupportPackage <Path toohello folder that contains support package files>`
   
    ![Támogatási csomag szerkesztése](./media/storsimple-8000-create-manage-support-package/IC750707.png)
8. Amikor a rendszer kéri, adja meg a titkosítás jelszavát az hello módosított támogatási csomag.
   
        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for hello following parameters:EncryptionPassphrase: ****
9. Írja le hello új jelszót, úgy, hogy a Microsoft Support kérésekor megoszthatja azt.

### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a>Példa: Egy támogatási csomag jelszóval védett megosztott fájlok szerkesztése

hello a következő példa bemutatja, hogyan toodecrypt, szerkesztheti, és egy támogatási csomag újbóli titkosítására.

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

## <a name="next-steps"></a>Következő lépések

* További tudnivalók: hello [hello támogatási csomag gyűjtött információk](https://support.microsoft.com/help/3193606/storsimple-support-packages-and-device-logs)
* Ismerje meg, hogyan túl[használata támogatási csomag és az eszköz naplói tootroubleshoot az eszköz telepítési](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).
* Ismerje meg, hogyan túl[használata hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).

