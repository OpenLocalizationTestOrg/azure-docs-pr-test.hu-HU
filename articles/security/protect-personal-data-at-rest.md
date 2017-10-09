---
title: "személyes adatok védelme a titkosítás aktívan aaaAzure |} Microsoft Docs"
description: "Ez a cikk segít az Azure tooprotect személyes adatok felhasználásával több része"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 9af182b4897f1d04f5f519e6671f53b85073bae1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-encryption-technologies-protect-personal-data-at-rest-with-encryption"></a>Az Azure titkosítási technológiák: a titkosítás aktívan személyes adatok védelme

Ez a cikk segít megismerje és hatékonyabban használhassa az Azure titkosítási technológiák toosecure adatokat aktívan.

Az inaktív adatok titkosítása elengedhetetlen a bevált gyakorlat tooprotect bizalmas vagy személyes adatok és a toomeet megfelelőségi és az adatvédelmi követelményeit.
Aktívan nem adattitkosítás tervezett tooprevent hello támadó titkosítatlanul hello adatok hozzáférését az adatok titkosítása a lemezen hello biztosításával.

## <a name="scenario"></a>Forgatókönyv 

A nagy körutazás vállalat, az Amerikai Egyesült Államokban, hello telephelyének bővíti a műveletek toooffer útvonalak hello mediterrán, és Balti tengerek, valamint hello Brit-szigetekre. toosupport e erőfeszítéseket szerzett több kisebb körutazás sorok Olaszország, német, Dánia és hello brit

hello vállalati hello felhő Microsoft Azure toostore vállalati adatait használja. Ide tartozhat alkalmazott és/vagy ügyféladatok, mint:

- címek
- telefonszámok
- azonosító számokat
- orvosi adatok
- Hitelkártya-adatokhoz

hello vállalati kell védelmében hello az alkalmazottak és a felhasználói adatok szükség lenne rá adatok elérhető toothose osztályai tétele közben. (például a bérszámfejtési és foglalások részlegek)

hello körutazás sor is fenntartja, nagy adatbázis ellenszolgáltatás és hűség program tagok, amely tartalmazza az aktuális és korábbi ügyfelek tootrack kapcsolatokkal személyes adatokat.

### <a name="problem-statement"></a>Probléma leírása

hello vállalati kell védelmében hello alkalmazottak és a felhasználók a személyes adatok (például a bérszámfejtési és foglalások részlegek) igénylő adatok elérhető toothose részlegek tétele közben. A személyes adatok hello vállalati vezérelt adatközpont kívül van tárolva, és nem hello vállalat fizikai ellenőrzés alatt áll.

### <a name="company-goal"></a>Vállalati cél

Többrétegű védelemmel az olyan jellegű biztonsági stratégia részeként akkor egy vállalati cél tooensure, hogy minden adatforrás, amely tartalmazza a személyes adatok titkosítva legyenek, beleértve a felhőben található. Nem jogosult személyek nyereség access toohello személyes adatokat, ha megjelenítéséhez, nem olvasható formátumban kell lennie. Titkosítás alkalmazásakor kell lennie, vagy az átlátszó – felhasználók és rendszergazdák könnyen.

## <a name="solutions"></a>Megoldások

Azure-szolgáltatásokhoz biztosít több eszközök és technológiák toohelp titkosítással inaktív személyes adatok védelme.

### <a name="azure-key-vault"></a>Azure Key Vault

[Az Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) nyújt biztonságos tárolási hello kulcsok tooencrypt adatok használt Azure-szolgáltatások aktívan és ajánlott hello tárolási és kezelési megoldást. Titkosítási kulcs felügyeleti az alapvető toosecuring tárolt adatai.

#### <a name="how-do-i-use-azure-key-vault-tooprotect-keys-that-encrypt-personal-data"></a>Miként használható az Azure Key Vault tooprotect kulcsok, amelyek személyes adatok titkosítása?

az Azure Key Vault toouse, kell egy előfizetési tooan Azure-fiók. Szükség Azure PowerShell telepítése. A lépések az alábbiak PowerShell parancsmagok toodo hello alábbi:

1. Csatlakozás tooyour előfizetések

2. Kulcstartó létrehozása

3. Kulcs vagy titkos toohello kulcstároló hozzáadása

4. Regisztrálja az Azure Active Directoryval hello kulcstartót használó alkalmazások

5. Hello alkalmazások toouse hello kulcs vagy titkos kulcs engedélyezése

Kulcstároló, toocreate hello New-AzureRmKeyVault PowerShell-parancsmagot is használja. A tároló nevére, erőforráscsoport-név és földrajzi helyet rendeli. Hello tároló neve fog használni, más parancsmagok segítségével kulcsok kezeléséhez. Hello tároló hello REST API-n keresztül használó alkalmazások hello tároló URI fogja használni.

Az Azure Key Vault biztosítható egy szoftveres védelemmel ellátott kulcs, vagy importálhatja a meglévő kulcs egy. PFX-fájlt. Hello tárolóban lévő titkos kulcsokat (jelszó) is tárolhatja.

Hozzon létre egy kulcsot a helyi HSM-ben is, és hello kulcs elhagyná a HSM határait hello nélkül a Key Vault szolgáltatás hello tooHSMs vigye.

Az Azure Key Vault használatával részletes utasításokat kövesse hello lépései [Ismerkedés az Azure Key Vault.](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-get-started)

Az Azure Key Vault használt PowerShell-parancsmagok listáját lásd: [AzureRM.KeyVault](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).

### <a name="azure-disk-encryption-for-windows"></a>A Windows Azure lemez titkosítása

[Az Azure lemez titkosítása a Windows és Linux IaaS virtuális gépeket](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) Azure virtuális gépeken futó aktívan személyes adatok védelmét, és jól integrálható az Azure Key Vault. Használja az Azure Disk Encryption [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) a Windows és [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux tooencrypt mind az operációs rendszer és hello adatlemezek hello. Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016-os, és a Windows 8 és Windows 10-ügyfelek az Azure Disk Encryption támogatott.

#### <a name="how-do-i-use-azure-disk-encryption-tooprotect-personal-data"></a>Miként használható az Azure Disk Encryption tooprotect személyes adatokat?

Azure Disk Encryption toouse, egy előfizetés tooan Azure-fiók szükséges. tooenable Azure lemez titkosítása a Windows és Linux virtuális gépek, a hello, a következő:

1. Hello Azure lemez titkosítási Resource Manager-sablon, a PowerShell vagy a parancssori felület (CLI) tooenable lemeztitkosítás hello használata, és adja meg a titkosítási konfigurációt. 

2. Adjon hozzáférést toohello Azure platformon tooread hello titkosítási anyagok a kulcstartót.

3. Adjon meg egy Azure Active Directory (AAD) alkalmazás identitását toowrite hello titkosítási kulcs jelentős tooyour kulcstároló.

Azure hello VM és hello kulcstároló konfiguráció frissítése, és a titkosított virtuális gépet.

A kulcstároló toosupport Azure Disk Encryption beállításakor a nagyobb biztonság és toosupport titkosított virtuális gépek biztonsági mentését egy fő titkosítási kulcs-(KEK) adhat hozzá.

![](media/protect-personal-data-at-rest/create-key.png)

Részletes utasítások az adott telepítési forgatókönyvek és a felhasználói élmény szerepelnek [lemez titkosítás a Windows Azure és a Linux IaaS virtuális gépeket.](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption)

### <a name="azure-storage-service-encryption"></a>Azure Storage Service Encryption

[Az Azure Storage szolgáltatás titkosítási (SSE) inaktív adatok](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption) segít és megvédeni az adatok toomeet a szervezeti biztonsági és megfelelőségi jár kötelezettségekkel. Az Azure Storage automatikusan titkosítja az adatokat, 256 bites AES titkosítást előzetes toopersisting toostorage használatával, és a korábbi tooretrieval visszafejti. A szolgáltatás az Azure-BLOB és a fájlok érhető el.

#### <a name="how-do-i-use-storage-service-encryption-tooprotect-personal-data"></a>Hogyan használja a Storage szolgáltatás titkosítási tooprotect személyes adatokat?

Storage szolgáltatás titkosítási tooenable hello a következő:

1. Jelentkezzen be hello Azure-portálon.

2. Válasszon egy tárfiókot.

3. A beállítások hello Blob szolgáltatás szakaszban, válassza ki a titkosítási.

4. Csoportban hello Fájlszolgáltatás szakaszban, a titkosítás.

Hello titkosítási beállítás gombra kattintva engedélyezheti vagy letilthatja a Storage szolgáltatás titkosítási.

![](media/protect-personal-data-at-rest/storage-service-encryption.png)

Új adatok lesznek titkosítva. A meglévő fájlokat a tárfiókban lévő adatokat nem titkosított marad.

Miután engedélyezte a titkosítás, másolja a toohello tárfiók hello a következő módszerek egyikével:

1. Blobok vagy hello a fájlok másolása [AzCopy parancssori segédprogram](https://docs.microsoft.com/en-us/azure/storage/storage-use-azcopy).

2. [Az SMB-fájlmegosztás csatlakoztatása](https://docs.microsoft.com/en-us/azure/storage/storage-file-how-to-use-files-windows) , hogy használhassa a segédprogram például Robocopy toocopy fájlokat.

3. Másolja a blob vagy a fájl adatainak tooand között vagy a blob storage tárolási fiókok [Storage Ügyfélkódtáraival például .NET](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-how-to-use-blobs).

4.  Használja a [Tártallózó](https://docs.microsoft.com/en-us/azure/storage/storage-explorers) tooupload tooyour tárfiók blobok a titkosítás engedélyezve van.

### <a name="transparent-data-encryption"></a>Transzparens adattitkosítás

Transzparens Data Encryption (TDE) egy olyan szolgáltatás, amely mindkét hello adatbázis és a kiszolgáló szintjén az adatokat titkosíthatja az SQL Azure-ban. TDE engedélyezve van az újonnan létrehozott összes adatbázisra alapértelmezés szerint. TDE valós idejű i/o-titkosítási és visszafejtési hello adatainak és naplókönyvtárainak fájlok hajt végre.

#### <a name="how-do-i-use-tde-tooprotect-personal-data"></a>Hogyan használja a TDE tooprotect személyes adatokat?

Konfigurálhat TDE hello Azure-portálon keresztül, hello REST API használatával, vagy a PowerShell használatával. egy létező adatbázis felett hello Azure portál használatával TDE tooenable hello a következő:

1. A Microsoft hello Azure portál, <https://portal.azure.com> és bejelentkezés az Azure rendszergazdai vagy közreműködői fiókját.

2. Hello bal szalagcím tooBROWSE kattintson, majd SQL-adatbázisok.

3. Hello bal oldali ablaktáblán kijelölt SQL adatbázisok és kattintson a felhasználói adatbázisban.

4. Hello adatbázis paneljén kattintson az összes beállítás.

5. Hello-beállítások panelen kattintson átlátható titkosítási rész tooopen hello átlátható titkosítási panelen.

6. Hello adatok titkosítás panelen, helyezze át a hello adatok titkosítás gomb tooOn, és kattintson a Save (hello hello oldal tetején) tooapply hello beállítást. hello titkosítási állapottal fog hozzávetőleges hello átlátható adattitkosítás hello előrehaladását.

![Adatok titkosítás engedélyezése](media/protect-personal-data-at-rest/turn-data-encryption-on.png)

Hogyan tooenable TDE és visszafejtése TDE védett adatbázisok és egyéb információk megtalálhatók hello cikkben lévő utasítások [átlátható adattitkosítást az Azure SQL Database.](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database)

## <a name="summary"></a>Összefoglalás

hello vállalat is elérheti személyes hello Azure felhőben tárolt adatok titkosítása az a célja. Ehhez a Azure Disk Encryption túl a teljes kötetek védelméhez. Ide tartozhat hello operációs rendszer fájljait és a adatfájlok, amely személyes azonosításra alkalmas adatokat és más bizalmas adatokat. Az Azure Storage szolgáltatás titkosítási használt tooprotect blobok és fájlok tárolt személyes adatok is lehet. Az Azure SQL-adatbázisban tárolt adatokat az átlátható adattitkosítási védelmet nyújt az illetéktelen elérhetővé tegyék személyes információkat.

hello vállalati tooprotect hello kulcsokat is használt tooencrypt adatok Azure-ban, használhatja az Azure Key Vault. Ez leegyszerűsíti hello kulcskezelési folyamatot, és lehetővé teszi, hogy hello vállalati toomaintain irányítását kulcsok feletti személyes adatok titkosítása.

## <a name="next-steps"></a>Következő lépések

- [Azure Disk Encryption hibaelhárítási útmutatója](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption-tsg)

- [Azure virtuális gép titkosítása](https://docs.microsoft.com/en-us/azure/security-center/security-center-disk-encryption?toc=%2fazure%2fsecurity%2ftoc.json)

- [Az Azure Data Lake Store az adatok titkosítása](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-encryption)

- [Az Azure Cosmos DB adatbázis titkosítását](https://docs.microsoft.com/en-us/azure/cosmos-db/database-encryption-at-rest)
