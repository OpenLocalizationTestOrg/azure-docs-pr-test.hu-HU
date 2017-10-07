---
title: "aaaHow toogenerate és átviteli HSM által védett kulcsok Azure Key vault |} Microsoft Docs"
description: "Ez a cikk toohelp tervezése, létrehozása és majd a saját HSM által védett kulcsokat toouse az Azure Key Vault átviteléhez használja. Más néven BYOK vagy a saját kulcs."
services: key-vault
documentationcenter: 
author: cabailey
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 51abafa1-812b-460f-a129-d714fdc391da
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: ambapat
ms.openlocfilehash: 3bb234bd1c4b81770542ccf7110e256385ca3309
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toogenerate-and-transfer-hsm-protected-keys-for-azure-key-vault"></a>Hogyan toogenerate és átviteli HSM által védett kulcsok Azure Key vault
## <a name="introduction"></a>Bevezetés
A nagyobb az Azure Key Vault használatakor importálhatja és a hardveres biztonsági modulokkal (HSM), amely sosem hagyják el a HSM határait hello kulcsok létrehozásához. Ebben a forgatókönyvben legtöbbször hivatkozott tooas *a saját kulcs*, vagy BYOK. hello hardveres biztonsági modulok FIPS 140-2 2. szintű érvényesítve. Az Azure Key Vault használja a Thales nShield hardveres biztonsági modulok tooprotect-család a kulcsokat.

Ez a témakör toohelp tervezése, létrehozása és a saját HSM által védett kulcsokat toouse az Azure Key Vault majd át használja fel hello adatokat.

Ez a funkció nem érhető el Azure Kínában.

> [!NOTE]
> Az Azure Key Vault kapcsolatos további információkért lásd: [Mi az Azure Key Vault?](key-vault-whatis.md)  
>
> Egy alapszintű bemutató, amely tartalmazza a HSM által védett kulcsok kulcstároló létrehozása, lásd: [Ismerkedés az Azure Key Vault](key-vault-get-started.md).
>
>

További információ létrehozása és egy HSM által védett kulcs átvitele hello Internet:

* Egy kapcsolat nélküli munkaállomást, amely csökkenti a támadási felület hello hello kulcs generálása.
* hello kulcs és egy kulcs kulcscserekulcs (KEK), amely átvitelig titkosítva marad, amíg az Azure Key Vault HSM átvitt toohello nem titkosított. Csak hello titkosított verziót az Ön kulcsának hello eredeti munkaállomást hagyja.
* hello eszközkészlet tulajdonságainak megadása a bérlőkulcsot, amelyhez a fő toohello Azure Key Vault biztonságivilág van. Ezért után hello Azure Key Vault HSM kap, és fejti vissza a kulcsot, csak a hardveres biztonsági modulok használható. A kulcs nem exportálható. Ez a Thales HSM-ekről hello kötést.
* hello kulcscserekulcs (KEK), amely használt tooencrypt a kulcs hello Azure Key Vault HSM belül jön létre, és nem exportálható. hello HSM-EK biztosítják, hogy az nem hello KEK kívül hello HSM titkosítatlan verziója lehet. Ezenkívül hello eszközkészlet által kiadott igazolást tartalmaz a Thales e hello KEK nem exportálható, és egy eredeti HSM-en, a Thales által gyártott rendszer belül jött létre.
* hello eszközkészlet igazolást tartalmaz arról, hogy hello biztonsági világ szintén egy eredeti Thales által gyártott HSM jön létre az Azure Key Vault Thales is. Az igazolás igazolja, hogy a Microsoft eredeti hardvereket használ tooyou.
* A Microsoft külön kek használ, és biztonsági világot minden egyes földrajzi régióban. Ez az elkülönítés biztosítja, hogy használható-e a kulcsot csak az adatközpontokban hello régióban, ahol titkosítva lett. Például egy Európai ügyfél kulcs nem használható az Észak-amerikai vagy ázsiai adatközpontokban.

## <a name="more-information-about-thales-hsms-and-microsoft-services"></a>További információ a Thales HSM-ekről és a Microsoft-szolgáltatások
A Thales e-Security vezető globális szolgáltató az adattitkosítás és a számítógépes biztonsági megoldások toohello pénzügyi szolgáltatásokat, a csúcstechnológiai, a gyártási, a kormányzati és a technológiai szektorok részére. Egy 40 éves tapasztalattal bíró védelmének vállalati és kormányzati információk Thales megoldásait négy hello öt legnagyobb energetikai és űrtechnikai vállalatok által használt. A megoldások 22 NATO országok is használják, és biztonságos világszerte a pénzügyi tranzakciók több mint 80 %-át.

Microsoft állapotú Thales tooenhance hello argentin van közvetlenül az HSM-EK számára. Az ilyen fejlesztések lehetővé teszik a tooget hello tipikus üzemeltetett szolgáltatások előnyei lemondana szabályozható a kulcsokat. Pontosabban a fejlesztéseknek köszönhetően a Microsoft hello HSM-EK kezelése, így nem kell. Egy felhőalapú szolgáltatás, az Azure Key Vault rendkívül gyorsan igazodik toomeet, a szervezete használati napra. At hello azonos időben, az Ön kulcsának védelmét a Microsoft HSM-jei: mivel hello kulcs létrehozása, és vigye tooMicrosoft a HSM-EK ellenőrzése alatt tartja a hello kulcs életciklusához kapcsolódó megőrzi.

## <a name="implementing-bring-your-own-key-byok-for-azure-key-vault"></a>Az Azure Key Vault a saját kulcs (használatának BYOK) megvalósítása kapcsolása
Használjon hello követő információkat és eljárásokat, ha a saját HSM által védett kulcs létrehozása és tooAzure Key Vault át fogja – hello kapcsolja a saját kulcs (használatának BYOK) forgatókönyv.

## <a name="prerequisites-for-byok"></a>A BYOK előfeltételei
Tekintse meg a következő táblázat az Előfeltételek listáját hello Azure Key Vault teheti a saját kulcs (használatának BYOK).

| Követelmény | További információ |
| --- | --- |
| Egy előfizetés tooAzure |egy Azure Key Vault toocreate, Azure-előfizetés szükséges: [regisztráljon az ingyenes próbaverzióhoz](https://azure.microsoft.com/pricing/free-trial/) |
| hello Key Vault prémium szintű szolgáltatási réteg toosupport HSM által védett kulcsokat |Az Azure Key Vault hello szolgáltatási szinteket és képességeire vonatkozó további információkért lásd: hello [Azure Key Vault díjszabása](https://azure.microsoft.com/pricing/details/key-vault/) webhelyet. |
| A Thales HSM, intelligens kártyák és a szoftvert |Rendelkeznie kell tooa Thales hardveres biztonsági modul és a Thales HSM-ekről alapvető ismeretekre. Lásd: [Thales hardveres biztonsági modul](https://www.thales-esecurity.com/msrms/buy) kompatibilis modellek, vagy egy hardveres biztonsági MODULT, ha még nem rendelkezik ilyennel toopurchase hello listáját. |
| hello következő hardver- és:<ol><li>Egy kapcsolat nélküli x64 munkaállomás, amelyen a Windows 7 és a Thales nShield szoftver legalább egy minimális Windows operációs rendszer verziója 11.50.<br/><br/>Ha ez a munkaállomás Windows 7 rendszeren fut kell [Microsoft .NET-keretrendszer 4.5-ös verziójának telepítése](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</li><li>A munkaállomás csatlakoztatott toohello Internet és a Windows 7 minimális Windows operációs rendszert és [Azure PowerShell](/powershell/azure/overview) **minimális verziója 1.1.0-ás** telepítve.</li><li>Egy USB-meghajtó vagy más hordozható tárolóeszköz, amelyen legalább 16 MB szabad tárhely.</li></ol> |Biztonsági okokból azt javasoljuk, hogy hello első munkaállomás nincs csatlakoztatott tooa hálózati. Azonban ez a javaslat nem szoftveres követelmény.<br/><br/>Vegye figyelembe, hogy a hello utasításokban, ennek a munkaállomásnak hivatkozott tooas leválasztása hello munkaállomásra.</p></blockquote><br/>Emellett ha a bérlőkulcsot egy üzemi hálózat, azt javasoljuk, hogy egy második, különálló munkaállomást toodownload hello eszközkészlet, valamint a feltöltési hello bérlői kulcsot használ. Tesztelési célokra használható, de hello munkaállomást, hello elsőt.<br/><br/>Vegye figyelembe, hogy a hello utasításokban, erre a második munkaállomásra hivatkozott tooas hello internetre kapcsolódó munkaállomásra.</p></blockquote><br/> |

## <a name="generate-and-transfer-your-key-tooazure-key-vault-hsm"></a>Generate and transfer a kulcs tooAzure Key Vault HSM és
Akkor használja a következő öt lépéseket toogenerate hello, és a kulcs tooan Azure Key Vault HSM átviteli:

* [1. lépés: Az internetre kapcsolódó munkaállomás előkészítése](#step-1-prepare-your-internet-connected-workstation)
* [2. lépés: A kapcsolat nélküli munkaállomás előkészítése](#step-2-prepare-your-disconnected-workstation)
* [3. lépés: A kulcs létrehozása](#step-3-generate-your-key)
* [4. lépés: A kulcs Áthelyezés előkészítése](#step-4-prepare-your-key-for-transfer)
* [5. lépés: A kulcs tooAzure Key Vault átvitele](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1-prepare-your-internet-connected-workstation"></a>1. lépés: Az internetre kapcsolódó munkaállomás előkészítése
Az hello az első lépés az alábbi eljárásokat az internethez csatlakoztatott toohello munkaállomáson.

### <a name="step-11-install-azure-powershell"></a>1.1. lépés: Az Azure PowerShell telepítése
Hello internethez csatlakoztatott munkaállomáson töltse le és telepítse hello Azure PowerShell-modult, amely tartalmazza az hello parancsmagok toomanage Azure Key Vault. Ehhez a 0.8.13 verzióját.

A telepítési utasításokért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

### <a name="step-12-get-your-azure-subscription-id"></a>1.2. lépés: Az Azure előfizetés-azonosító lekérése
Indítson el egy Azure PowerShell-munkamenetet, és jelentkezzen be Azure-fiók tooyour hello a következő parancs használatával:

        Add-AzureAccount
Hello előugró böngészőablakban adja meg a Azure-fiók felhasználói nevét és jelszavát. Ezután használja az hello [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) parancs:

        Get-AzureSubscription
Hello kimenetből keresse meg a hello Azonosítóját használja-e az Azure Key Vault hello az előfizetéshez. Később szüksége lesz az előfizetés-azonosító.

Ne zárja be a hello Azure PowerShell-ablakot.

### <a name="step-13-download-hello-byok-toolset-for-azure-key-vault"></a>1.3. lépés: Az Azure Key Vault hello BYOK eszközkészlet letöltése
Nyissa meg a Microsoft Download Center toohello és [hello Azure Key Vault BYOK eszközkészlet letöltése](http://www.microsoft.com/download/details.aspx?id=45345) a földrajzi régióban vagy Azure példányát. Hello alábbi információkat tooidentify hello csomag neve toodownload és a megfelelő SHA-256 kivonatoló csomag:

- - -
**Egyesült Államok:**

KeyVault-BYOK-eszközök – Egyesült States.zip

760EE9BD6445C87CFF0E8B032577118704B3BEAA045AA55977C10EF68BC67E2B

- - -
**Európa:**

KeyVault-BYOK-eszközök – Europe.zip

7A64B94225F59B847C5C27C2200BAD7D16C901E1687767EDBBB8B09BB285011D

- - -
**Ázsia:**

KeyVault-BYOK-eszközök – AsiaPacific.zip

813DC94B23079CF7A5CEA71D5B444E86B292F463C53EE47AED25D4F7CD58E7D8

- - -
**Latin-Amerika:**

KeyVault-BYOK-eszközök – LatinAmerica.zip

3F29069E3500F95C0E156F4B8914E1DC60C20FB64B464306A299EA5145D755C0

- - -
**Japán:**

KeyVault-BYOK-eszközök – Japan.zip

453FFEA2F8F410720B68B8BAC4CF79135A7F37F4E491FF840BE9E69E88A98C90

- - -
**Korea:**

KeyVault-BYOK-eszközök – Korea.zip

C17B7E93224DA80F5668E09CF7DAE2F92527E8226179995BBE2E43DA4323595A

- - -
**Ausztrália:**

KeyVault-BYOK-eszközök – Australia.zip

4AD893396E86F2D2A71682876A6A8EA59E3C7895BEAD2F7E7C8516682582C34B

- - -
[**Az Azure Government:**](https://azure.microsoft.com/features/gov/)

KeyVault-BYOK-eszközök – USGovCloud.zip

3AAE1A96B9D15B899B8126CFC0380719EB54FDF2EA94489B43FAD21ECC745F64

- - -
**Egyesült Államok kormánya DOD:**

KeyVault-BYOK-eszközök – USGovernmentDoD.zip

A61E78297B0732DF2682FDE63D7B572CE4D23B0BC27CC48AFF620BD060BB9E9D

- - -
**Kanada:**

KeyVault-BYOK-eszközök – Canada.zip

30B87A0BA8208F6B7241C30C794FED1C370D7445ACA179685816E4E156CD2AF7

- - -
**Németország:**

KeyVault-BYOK-eszközök – Germany.zip

5E3E4AA54715E4F93C3C145035B18275B7C6815A06D7ABB212E7FADBF2929261

- - -
**India:**

KeyVault-BYOK-eszközök – India.zip

136733A6C6A71D75571BB80819B3D55A9B83CCAD5C996C686BC5682A3F369BF7

- - -
**Egyesült Királyság:**

KeyVault-BYOK-eszközök – UnitedKingdom.zip

ED331A6F1D34A402317D3F27D5396046AF0E5C2D44B5D10CCCE293472942D268

- - -

a letöltött BYOK eszközkészlet, az Azure PowerShell-munkamenetben, használjon hello toovalidate hello sértetlenségének [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) parancsmag.

    Get-FileHash KeyVault-BYOK-Tools-*.zip

hello eszközkészlet hello következőket tartalmazza:

* Egy kulcscserekulcs (KEK) csomag, amelynek a neve a **BYOK-KEK - pkg-.**
* Egy Biztonságivilág-csomag, amelynek a neve a **BYOK-SecurityWorld - pkg-.**
* Nevű python-parancsfájl **verifykeypackage.py.**
* Egy parancssori végrehajtható fájl **KeyTransferRemote.exe** és kapcsolódó dll-fájlok.
* A Visual C++ terjeszthető csomag **vcredist_x64.exe.**

Másolja a hello csomag tooa USB-meghajtó vagy más hordozható tárolóeszközre.

## <a name="step-2-prepare-your-disconnected-workstation"></a>2. lépés: A kapcsolat nélküli munkaállomás előkészítése
Az hello ebben a második lépésben az alábbi eljárásokat hello munkaállomáson, amely nem csatlakoztatott tooa hálózat (hello Internet vagy a belső hálózaton).

### <a name="step-21-prepare-hello-disconnected-workstation-with-thales-hsm"></a>2.1. lépés: A Thales HSM-mel leválasztása hello munkaállomás előkészítése
Hello nCipher (Thales) támogatószoftvert egy Windows számítógépre telepítse, majd csatolja a Thales HSM-en toothat számítógép.

Győződjön meg arról, hogy a Thales-eszközök hello az elérési úthoz (**%nfast_home%\bin**). Írja be például a következő hello:

        set PATH=%PATH%;"%nfast_home%\bin"

További információkért lásd: hello Thales HSM-en mellékelt hello felhasználói útmutatót.

### <a name="step-22-install-hello-byok-toolset-on-hello-disconnected-workstation"></a>2.2. lépés: Telepítse hello BYOK eszközkészlet leválasztása hello munkaállomáson
Hello BYOK eszközkészletcsomagot másolása hello USB-meghajtó vagy más hordozható tárolóeszköz, és ezután hello a következő:

1. Csomagolja ki hello fájlokat hello letöltött csomag egy tetszőleges mappába.
2. Ebből a könyvtárból futtassa a vcredist_x64.exe.
3. Útmutatás alapján hello toohello hello Visual C++ futtatókörnyezeti összetevők telepítése a Visual Studio 2013.

## <a name="step-3-generate-your-key"></a>3. lépés: A kulcs létrehozása
Az hello harmadik ezt a lépést, az alábbi eljárásokat leválasztva hello munkaállomáson. toocomplete ebben a lépésben a HSM-ből módban kell lennie az inicializálás. 


### <a name="step-31-change-hello-hsm-mode-tooi"></a>3.1. lépés: Hello HSM mód too'I módosítása "
A Thales nShield él, toochange hello mód használata: 1. Hello mód gomb toohighlight hello szükséges módot használja. 2. Néhány másodpercen belül tartsa lenyomva a hello világos gomb néhány másodpercig. Ha hello mód hello új mód villogó LED leáll, és bekapcsolásával marad. hello állapot LED szabálytalan flash előfordulhat, hogy néhány másodpercig, és rendszeresen, amikor a hello eszköz készen áll majd tokenkódot. Ellenkező esetben hello eszköz marad hello aktuális módban, hello megfelelő módot LED bekapcsolásával.

### <a name="step-32-create-a-security-world"></a>3.2. lépés: Biztonsági világ létrehozása
Nyisson meg egy parancsablakot, és futtassa a Thales új világot létrehozó programját hello.

    new-world.exe --initialize --cipher-suite=DLf1024s160mRijndael --module=1 --acs-quorum=2/3

Ez a program létrehoz egy **Biztonságivilág** fájl: % NFAST_KMDATA%\local\world, amely toohello C:\ProgramData\nCipher\Key Management Data\local mappának felel meg. Hello kvóruma eltérő értékeket is használhat, de ebben a példában, tehát felszólító tooenter három üres kártyát és mindegyikhez PIN-kód. Bármelyik két kártyának adjon teljes hozzáférés toohello biztonságivilág. Ezek a kártyák lesznek hello **rendszergazdai Kártyakészlete** hello új biztonsági világ számára.

Majd hello a következő:

* Készítsen biztonsági másolatot a világfájlról hello. Biztonságos és hello world fájl, hello rendszergazdai kártyák és a PIN-kódok védelmében, és győződjön meg arról, hogy egy személy önmagában nem rendelkezik-e a hozzáférés toomore, mint egy kártya.

### <a name="step-33-change-hello-hsm-mode-tooo"></a>3.3. lépés: Hello HSM mód too'O módosítása "
A Thales nShield él, toochange hello mód használata: 1. Hello mód gomb toohighlight hello szükséges módot használja. 2. Néhány másodpercen belül tartsa lenyomva a hello világos gomb néhány másodpercig. Ha hello mód hello új mód villogó LED leáll, és bekapcsolásával marad. hello állapot LED szabálytalan flash előfordulhat, hogy néhány másodpercig, és rendszeresen, amikor a hello eszköz készen áll majd tokenkódot. Ellenkező esetben hello eszköz marad hello aktuális módban, hello megfelelő módot LED bekapcsolásával.


### <a name="step-34-validate-hello-downloaded-package"></a>3.4. lépés: Hello letöltött csomag ellenőrzése
Ez a lépés nem kötelező, de ajánlott hello következő ellenőrizheti:

* hello hello eszközkészletben található kulcscserekulcs egy eredeti Thales HSM-en lett létrehozva.
* hello hello biztonsági világának hello eszközkészletben található kivonata egy eredeti Thales HSM-en lett létrehozva.
* hello kulcscserekulcs nem exportálható.

> [!NOTE]
> toovalidate hello letöltött csomag, hello HSM csatlakoztatva kell lennie, az alkalmazás bekapcsolja, és rendelkeznie kell egy biztonsági világgal (például a most létrehozott egy hello).
>
>

toovalidate hello letöltött csomag:

1. Hello verifykeypackage.py parancsfájlt futtassa hello következők egyike, attól függően, hogy a földrajzi régióban vagy Azure példánya beírásával:

   * Észak-amerikai:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-NA-1 -w BYOK-SecurityWorld-pkg-NA-1
   * Európa esetében:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-EU-1 -w BYOK-SecurityWorld-pkg-EU-1
   * Ázsia esetében:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-AP-1 -w BYOK-SecurityWorld-pkg-AP-1
   * Latin-Amerika: a

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-LATAM-1 -w BYOK-SecurityWorld-pkg-LATAM-1
   * A japán:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-JPN-1 -w BYOK-SecurityWorld-pkg-JPN-1
   * Koreai:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-KOREA-1 -w BYOK-SecurityWorld-pkg-KOREA-1
   * Ausztráliában:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
   * A [Azure Government](https://azure.microsoft.com/features/gov/), használó Azure hello Egyesült Államokbeli kormányzati példánya:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
   * Az Amerikai Egyesült Államok kormánya DOD:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USDOD-1 -w BYOK-SecurityWorld-pkg-USDOD-1
   * Kanada esetében:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
   * Németország:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
   * A India:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
     > [!TIP]
     > hello Thales szoftver tartalmaz python %NFAST_HOME%\python\bin címen
     >
     >
2. Ellenőrizze, hogy látható-e hello következő, az ellenőrzés sikerességét jelző: **Result: SUCCESS**

A parancsfájl ellenőrzi az aláírói láncot hello a Thales-gyökérkulcsig toohello fel. a legfelső szintű kulcs hello kivonatoló beágyazott hello parancsfájl és az értéket kell **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**. Azt is ellenőrizheti ezt az értéket külön-külön hello felkeresésével [Thales webhelyén](http://www.thalesesec.com/).

Most már készen áll a toocreate egy új kulcsot.

### <a name="step-35-create-a-new-key"></a>3.5. lépés: Új kulcs létrehozása
Hozzon létre egy kulcsot a hello Thales **generatekey** program.

Futtassa a következő parancs toogenerate hello kulcs hello:

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

Ez a parancs futtatásakor használja ezeket az utasításokat:

* hello paraméter *védelme* toohello értéket meg kell **modul**látható módon. Ez a modul által védett kulcsot hoz létre. hello BYOK eszközkészlet nem támogatja az OCS által védett kulcsokat.
* Cserélje le a hello értékének *contosokey* a hello **ident** és **plainname** bármilyen karakterlánc-értékkel. az adminisztratív terhek toominimize hello kockázatok csökkentése és a hibák, azt javasoljuk, hogy hello értékeként is azonos értéket használja. Hello **ident** érték csak számokat, kötőjeleket és kisbetűket tartalmazhat.
* hello pubexp üresen maradt (alapértelmezett) ebben a példában, de az adott értékeket adhat meg. További információkért lásd: hello a Thales dokumentációjában talál.

Ez a parancs egy tokenekre bontott kulcsfájlt hoz létre az %NFAST_KMDATA%\local mappában, amelynek a neve a **key_simple_**, utána pedig hello **ident** hello parancsban megadott. Például: **key_simple_contosokey**. Ez a fájl egy titkosított kulcsot tartalmaz.

Készítsen biztonsági másolatot a tokenekre bontott Kulcsfájlról egy biztonságos helyre.

> [!IMPORTANT]
> Amikor később átviszi a kulcs tooAzure kulcstároló, a Microsoft nem exportálható a kulcs hátsó tooyou, ezért rendkívül fontos, hogy biztonsági másolatot készíteni a kulcs és a biztonsági világáról biztonságosan. Lépjen kapcsolatba a Thales útmutatás és ajánlott eljárások a kulcs biztonsági mentéséről.
>
>

Meg vannak tootransfer most már készen áll a kulcs tooAzure kulcstároló.

## <a name="step-4-prepare-your-key-for-transfer"></a>4. lépés: A kulcs Áthelyezés előkészítése
Az hello a negyedik lépése az alábbi eljárásokat leválasztva hello munkaállomáson.

### <a name="step-41-create-a-copy-of-your-key-with-reduced-permissions"></a>4.1. lépés: A korlátozott engedélyekkel rendelkező másolata a létrehozása

Nyisson meg egy új parancssort, és hello aktuális directory toohello helyének módosításához ahol unzipped a hello BYOK zip-fájl. tooreduce hello engedélyeit a kulcsot egy parancssorból futtassa hello következők egyike, attól függően, hogy a földrajzi régióban vagy Azure példánya:

* Észak-amerikai:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1
* Európa esetében:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1
* Ázsia esetében:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1
* Latin-Amerika: a

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1
* A japán:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1
* Koreai:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-KOREA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-KOREA-1
* Ausztráliában:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
* A [Azure Government](https://azure.microsoft.com/features/gov/), használó Azure hello Egyesült Államokbeli kormányzati példánya:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
* Az Amerikai Egyesült Államok kormánya DOD:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1
* Kanada esetében:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
* Németország:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
* A India:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1

Ez a parancs futtatásakor cserélje le *contosokey* hello azonos értékre a **lépés 3.5: hozzon létre egy új kulcsot** a hello [a kulcs létrehozása](#step-3-generate-your-key) lépés.

A biztonsági globális rendszergazdai kártyák tooplug kell adnia.

Hello parancs befejeződésekor megjelenik **eredmény: sikeres** hello másolat készítése a kulcsról korlátozott engedélyekkel rendelkező key_xferacId_ nevű hello fájlban, és<contosokey>.

Hello hozzáférés-vezérlési LISTÁKAT, előfordulhat, hogy megvizsgálja a Thales segédprogramjai használatával a következő parancsokat a hello:

* aclprint.PY:

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
* kmfile-dump.exe:

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
  Ezek a parancsok futtatásakor cserélje le contosokey ugyanaz az érték hello **lépés 3.5: hozzon létre egy új kulcsot** a hello [a kulcs létrehozása](#step-3-generate-your-key) lépés.

### <a name="step-42-encrypt-your-key-by-using-microsofts-key-exchange-key"></a>4.2. lépés: A kulcs titkosítása a Microsoft kulcscserekulcs használatával
Futtassa a következő parancsokat, attól függően, hogy a földrajzi régióban vagy Azure példánya hello egyikét:

* Észak-amerikai:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Európa esetében:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Ázsia esetében:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Latin-Amerika: a

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* A japán:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Koreai:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-KOREA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-KOREA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Ausztráliában:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* A [Azure Government](https://azure.microsoft.com/features/gov/), használó Azure hello Egyesült Államokbeli kormányzati példánya:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Az Amerikai Egyesült Államok kormánya DOD:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Kanada esetében:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Németország:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* A India:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey

Ez a parancs futtatásakor használja ezeket az utasításokat:

* Cserélje le *contosokey* toogenerate hello kulcsában használt hello azonosítójú **lépés 3.5: hozzon létre egy új kulcsot** a hello [a kulcs létrehozása](#step-3-generate-your-key) lépés.
* Cserélje le *SubscriptionID* hello azonosítójú, hello Azure-előfizetéssel, amely a kulcstartót tartalmazza. Ez az érték lekért korábban a **lépés 1.2: az Azure-előfizetés beszerzése** a hello [az internetre kapcsolódó munkaállomás előkészítése](#step-1-prepare-your-internet-connected-workstation) lépés.
* Cserélje le *ContosoFirstHSMKey* a címkére, amelyet a kimeneti fájl neveként használatos.

Amikor ez befejeződik, megjeleníti **eredmény: sikeres** és egy új fájlt hello aktuális mappában, amelynek neve a következő hello: KeyTransferPackage -*ContosoFirstHSMkey*.byok

### <a name="step-43-copy-your-key-transfer-package-toohello-internet-connected-workstation"></a>4.3. lépés: A kulcsátviteli csomag toohello internetre kapcsolódó munkaállomás másolása
Hello előző lépést (KeyTransferPackage-ContosoFirstHSMkey.byok) tooyour internetre kapcsolódó munkaállomás egy USB-meghajtó vagy más hordozható tárolóeszköz toocopy hello kimeneti fájlt használja.

## <a name="step-5-transfer-your-key-tooazure-key-vault"></a>5. lépés: A kulcs tooAzure Key Vault átvitele
A végső lépés hello internetkapcsolattal rendelkező munkaállomáson, használja a hello [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) parancsmag tooupload hello kulcsátviteli csomag hello fájlból másolt csatlakoztatva munkaállomás toohello Azure Key Vault hardveres biztonsági MODULT:

    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\KeyTransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'

Sikeres hello feltöltés esetén, amelyek az előzőekben adott hozzá hello kulcs megjelenített hello tulajdonságai jelennek meg.

## <a name="next-steps"></a>Következő lépések
A HSM által védett kulcs key vaultban lévő most már használhatja. További információkért lásd: hello **Ha azt szeretné, a hardveres biztonsági modul (HSM) toouse** hello szakasz [Ismerkedés az Azure Key Vault](key-vault-get-started.md) oktatóanyag.
