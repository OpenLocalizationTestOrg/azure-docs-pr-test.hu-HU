---
title: "aaaTroubleshoot Azure File storage problémák a Windows rendszerben |} Microsoft Docs"
description: "A Windows Azure File storage problémáinak"
services: storage
documentationcenter: 
author: genlin
manager: willchen
editor: na
tags: storage
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: genli
ms.openlocfilehash: 19529d8af5d98790e2e381cd21ad4d0284acb124
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-file-storage-problems-in-windows"></a>A Windows Azure File storage kapcsolatos problémák elhárítása

A cikk ismerteti a gyakori problémák, amelyek kapcsolódó tooMicrosoft Azure File storage Windows-ügyfelek csatlakozásakor. Is biztosít lehetséges okokért és megoldásokért ezeket a problémákat. Továbbá ebben a cikkben toohello hibaelhárítási lépések, is [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) győződjön meg arról, hogy a Windows hello ügyfél környezet számára megfelelő előfeltételek. AzFileDiagnostics automatizálja a cikkben említett hello tünetei és állítsa be a környezet tooget optimális teljesítményt nyújt segítséget a legtöbb észlelése. Ez az információ is található hello [Azure fájlmegosztási hibaelhárító](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares) lépéseket tooassist biztosít problémák való csatlakozás/leképezési/csatlakoztatása Azure fájlmegosztási.


<a id="error53-67-87"></a>
## <a name="error-53-error-67-or-error-87-when-you-mount-or-unmount-an-azure-file-share"></a>53-as hiba, a 67-es hiba vagy a hiba 87, amikor csatlakoztatni vagy leválasztani az Azure fájlmegosztások

Amikor egy fájlmegosztás a helyszíni vagy egy másik datacenter toomount, akkor fordulhat elő, a következő hibák hello:

- Rendszer 53-as hiba. hello hálózati elérési út nem található.
- 67-es rendszerhiba történt. hello hálózatnév nem található.
- 87 rendszerhiba történt. hello paraméter nem megfelelő.

### <a name="cause-1-unencrypted-communication-channel"></a>1. ok: Titkosítatlan kommunikációs csatorna

Biztonsági okokból tooAzure fájlmegosztások le vannak tiltva, ha hello kommunikációs csatorna nincs titkosítva, és ha hello kapcsolódási kísérlet nem kapcsolatok hello hello Azure fájlmegosztásokat tároló ugyanabban az adatközpontban. Csak akkor, ha hello felhasználói ügyfél operációs rendszer támogatja az SMB-titkosítás a kommunikációs csatorna titkosítást biztosítja.

Windows 8, Windows Server 2012 és egyes rendszerek újabb verzióit egyezteti az SMB 3.0-s, amely támogatja a titkosítást kérelmeket.

### <a name="solution-for-cause-1"></a>Megoldás ok 1

Csatlakozás, amelyet hello következő ügyfélről:

- Hello megfelel a Windows 8 és Windows Server 2012 vagy újabb verzió
- Csatlakoztatja a virtuális gép a hello azonos módon hello Azure fájlmegosztás használt Azure storage-fiók hello datacenter

### <a name="cause-2-port-445-is-blocked"></a>2. ok: 445-ös Port blokkolva van

Rendszerhiba 53-as vagy rendszerhiba 67 akkor fordulhat elő, ha port 445-ös kimenő kommunikáció tooan Azure File storage datacenter le van tiltva. toosee hello összegzését, 445-ös portot a elérésének engedélyezése vagy letiltása az internetszolgáltatók lépjen túl[TechNet](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx).

toounderstand Ez oka hello "System error 53" üdvözlőüzenetére mögött, hogy használhatja-e Portqry tooquery hello TCP:445 végpont. Hello TCP:445 végpont szűrt jelenik meg, ha a TCP-port hello le van tiltva. Íme egy példa lekérdezést:

  `g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

Ha hello hálózati elérési út mentén szabály blokkolja a 445-ös TCP-portot, a következő kimeneti hello jelenik meg:

  `TCP port 445 (microsoft-ds service): FILTERED`

További információ toouse Portqry, lásd: [hello Portqry.exe parancssori segédprogram](https://support.microsoft.com/help/310099).

### <a name="solution-for-cause-2"></a>Megoldás ok 2

Az informatikai részleg tooopen 445-ös porton kimenő túl együttműködve[Azure IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="cause-3-ntlmv1-is-enabled"></a>3. ok: NTLMv1 engedélyezve

Rendszerhiba 53-as vagy rendszerhiba 87 akkor fordulhat elő, ha NTLMv1 kommunikációs hello ügyfélen engedélyezve van. Az Azure File storage csak NTLMv2-alapú hitelesítést támogatja. Egy kevésbé biztonságos ügyfél engedélyezett NTLMv1 rendelkező hoz létre. Ezért kommunikációja blokkolva van az Azure File storage. 

toodetermine Ez az hello OK hello hibát, hogy ellenőrizze, hogy a következő beállításkulcsot hello értéke 3 tooa értékének:

**HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel**

További információkért lásd: hello [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) a témakör a TechNet webhelyen.

### <a name="solution-for-cause-3"></a>Megoldás ok 3

Állítsa vissza a hello **LmCompatibilityLevel** toohello alapértelmezett értéke a következő beállításkulcsot hello 3 értéket:

  **HKLM\SYSTEM\CurrentControlSet\Control\Lsa**

<a id="error1816"></a>
## <a name="error-1816-not-enough-quota-is-available-tooprocess-this-command-when-you-copy-tooan-azure-file-share"></a>Hiba 1816 "nincs elegendő kvótája a parancs van a rendelkezésre álló tooprocess" tooan Azure fájlmegosztás másolásakor

### <a name="cause"></a>Ok

1816 hiba akkor fordul elő, amikor elérkezik egy fájlt hello számítógépen, amelyen hello fájlmegosztás csatlakoztatva számára engedélyezett egyidejű megnyitott leíróinak hello felső korlátja.

### <a name="solution"></a>Megoldás

Csökkentse a hello száma párhuzamos megnyitott leíróinak néhány leírók bezárásával, majd próbálkozzon újra. További információkért lásd: [Microsoft Azure Storage teljesítményére és méretezhetőségére ellenőrzőlista](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-tooand-from-azure-file-storage-in-windows"></a>A Windows Azure File storage tooand másolását lassú fájl

Lassú teljesítmény tootransfer fájlok toohello Azure File service meg jelenhet meg.

- Még nem rendelkezik egy adott minimális i/o vonatkozó méretbeli követelményt, azt javasoljuk, hogy használható 1 MB hello i/o-méret az optimális teljesítmény.
-   Ha tudja, amelyet a kiterjesztése hello a végleges mérethez ír, és a szoftver nincs kompatibilitási problémákat, ha hello unwritten utóhívás hello fájl tartalmaz nulla, majd hello fájl mérete előre minden egyes egy kibővítése írási elvégzése helyett.
-   Hello jobb copy metódust használja:
    -   Használjon [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#file-copy) bármely két fájlmegosztások közötti átvitel céljából.
    -   Használjon [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) közötti fájlmegosztások a helyi számítógépen.

### <a name="considerations-for-windows-81-or-windows-server-2012-r2"></a>Windows 8.1 vagy Windows Server 2012 R2 szempontjai

Windows 8.1 vagy Windows Server 2012 R2 rendszert futtató ügyfeleken győződjön meg arról, hogy hello [KB3114025](https://support.microsoft.com/help/3114025) gyorsjavítás telepítve van. Ez a gyorsjavítás javítja a Create hello teljesítményét, és zárja be a kezeli.

E hello gyorsjavítás telepítve van a következő parancsfájl toocheck hello futtathatja:

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

Ha a gyorsjavítás telepítve van, hello következő eredmény jelenik meg:

`HKEY_Local_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies {96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1`

> [!Note]
> Windows Server 2012 R2 Azure piactéren képeknek gyorsjavítás KB3114025 alapértelmezés szerint telepítve 2015. decemberi kezdve.

<a id="shareismissing"></a>
## <a name="no-folder-with-a-drive-letter-in-my-computer"></a>Ne legyen a meghajtóbetűjel mappa **Sajátgép**

Ha a nettó használata rendszergazdaként leképez egy Azure fájlmegosztás, hello megosztás megjelenik-e toobe hiányzik.

### <a name="cause"></a>Ok

Alapértelmezés szerint a Windows Fájlkezelőben nem futtassa rendszergazdaként. Ha nettó használata futtatja a parancsot egy rendszergazdai parancssort, akkor hello hálózati meghajtó meghajtó csatlakoztatása rendszergazdaként. Mivel a csatlakoztatott meghajtók felhasználó-központú, hello felhasználói fiókot, amely be van jelentkezve nem jeleníti meg hello meghajtók ha csatlakoztatva vannak egy másik felhasználói fiókkal.

### <a name="solution"></a>Megoldás
Hello fájlmegosztás csatlakoztatása egy nem rendszergazdai parancssorból. Azt is megteheti, amelyeket követve [a TechNet témakör](https://technet.microsoft.com/library/ee844140.aspx) tooconfigure hello **EnableLinkedConnections** beállításazonosítót.

<a id="netuse"></a>
## <a name="net-use-command-fails-if-hello-storage-account-contains-a-forward-slash"></a>Net use parancs sikertelen lesz, ha a tárfiók hello perjellel tartalmazza

### <a name="cause"></a>Ok

hello net use parancs perjellel (/) értelmezi a parancssori kapcsolót. Ha a felhasználói fiók nevét perjellel kezdődik, hello meghajtók hozzárendelése sikertelen lesz.

### <a name="solution"></a>Megoldás

A következő lépéseket toowork hello probléma hello bármelyikét használhatja:

- Futtassa a következő PowerShell-paranccsal hello:

  `New-SmbMapping -LocalPath y: -RemotePath \\server\share -UserName accountName -Password "password can contain / and \ etc" `

  Egy parancsfájlból hello parancs futtatható ily módon:

  `Echo new-smbMapping ... | powershell -command –`

- Helyezze a hello kulcs toowork a probléma megoldásához--dupla idézőjelek közé, ha hello perjelet az hello első karakter. Ha igen, használjon hello interaktív módban és külön-külön írja be a jelszót vagy újragenerálja a kulcsokat tooget egy kulcsot, nem kezdődhet perjellel.

<a id="cannotaccess"></a>
## <a name="application-or-service-cannot-access-a-mounted-azure-file-storage-drive"></a>Alkalmazás vagy szolgáltatás nem tud hozzáférni az Azure File storage meghajtóként

### <a name="cause"></a>Ok

Felhasználónként csatlakoztatott meghajtók. Ha az alkalmazás vagy szolgáltatás hello egy csatlakoztatott hello meghajtó, mint egy másik felhasználói fiók alatt fut, hello alkalmazás nem jelenik meg a hello meghajtó.

### <a name="solution"></a>Megoldás

A következő megoldások hello egyikét használja:

-   Hello meghajtó csatlakoztathatja hello ugyanazzal a fiókkal, amelyik hello alkalmazást tartalmaz. Használhatja például a PsExec eszköz.
- Hello felhasználói nevet és jelszót paraméterei hello net use parancs hello tárolási fióknevet és kulcsot adjon át.

Miután kövesse ezeket az utasításokat, akkor fordulhat elő, hello hello rendszer és a hálózati szolgáltatás fiók nettó használható futtatásakor a következő hibaüzenet: "1312 rendszerhiba történt. A megadott bejelentkezési munkamenet nem létezik. Azt is, hogy már befejeződött." Ha ez történik, ellenőrizze, hogy toonet használata átadott hello felhasználónévhez tartalmaz adatokat (például: "[tárfiók neve]. file.core.windows .net").

<a id="doesnotsupportencryption"></a>
## <a name="error-you-are-copying-a-file-tooa-destination-that-does-not-support-encryption"></a>"Másolja át a fájlt, amely nem támogatja a titkosítást tooa cél" hiba

Hello hálózaton keresztül másolja a fájlt, hello fájl esetén visszafejtett hello forrásszámítógép, nem titkosított szövegként továbbításra, és újból titkosít hello célhelyen. Azonban láthatja a következő hiba toocopy titkosított fájl regisztráláskor hello: "Hello fájl tooa cél, amely nem támogatja a titkosítást másol."

### <a name="cause"></a>Ok
Ez a probléma akkor fordulhat elő, ha titkosított fájlrendszer (EFS) használja. A BitLocker által titkosított fájlok lehet másolt tooAzure fájlok tárolására. Azure File storage azonban nem támogatja az NTFS EFS.

### <a name="workaround"></a>Megkerülő megoldás
toocopy hello hálózaton keresztül egy fájlt, akkor először vissza kell fejtenie azt. Hello a következő módszerek valamelyikével:

- Használjon hello **/d másolása** parancsot. Lehetővé teszi a hello titkosított fájlok toobe hello célhelyen készítését.
- Állítsa be a következő beállításkulcs hello:
  - Elérési út = HKLM\Software\Policies\Microsoft\Windows\System
  - Értéktípus = DWORD
  - Name = CopyFileAllowDecryptedRemoteDestination
  - Érték = 1

Ügyeljen arra, hogy beállítás hello beállításkulcshoz hatással van az összes másolási műveletek végzett toonetwork megosztások.

## <a name="need-help-contact-support"></a>Segítség Forduljon a támogatási szolgálathoz.
Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget gyorsan oldja meg a problémát.
