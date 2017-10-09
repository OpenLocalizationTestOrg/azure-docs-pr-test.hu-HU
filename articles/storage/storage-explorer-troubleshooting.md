---
title: "aaaAzure Tártallózó hibaelhárítási útmutatója |} Microsoft Docs"
description: "Az Azure két hello hibakeresési szolgáltatása – áttekintés"
services: virtual-machines
documentationcenter: 
author: Deland-Han
manager: cshepard
editor: 
ms.assetid: 
ms.service: virtual-machines
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: delhan
ms.openlocfilehash: 21705629500359222bc566c599f0864ad50036ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a>Az Azure Tártallózó hibaelhárítási útmutató

## <a name="introduction"></a>Bevezetés

A Microsoft Azure Tártallózó (előzetes verzió) egy különálló alkalmazás, amely lehetővé teszi tooeasily dolgozhat Azure Storage-adatokkal Windows, a macOS és a Linux. hello app Azure állami felhők és Azure verem üzemeltetett toStorage fiókok is elérheti.

Ez az útmutató a megoldások a Tártallózó tapasztalt gyakori problémákat foglalja össze.

## <a name="sign-in-issues"></a>Jelentkezzen be a problémák

Csak az Azure Active Directory (AAD) fiókok támogatottak. Az AD FS fiókot használjon, ha rendelkezik várt tooStorage Explorer csatlakoztatás nem működik az aláíráshoz. A folytatás előtt indítsa újra az alkalmazást, és hogy kell-e javítani hello problémákat.

### <a name="error-self-signed-certificate-in-certificate-chain"></a>Hiba: A tanúsítványláncában szereplő önaláírt tanúsítvány

Több oka miért merülhetnek fel ezt a hibát, és hello leggyakrabban használt két okok a következők:

1. hello app "transzparens proxyra", vagyis a kiszolgálóhoz (például a vállalati kiszolgálónak) elfogja a HTTPS-forgalmat, visszafejtése, és azt egy önaláírt tanúsítványt használ majd titkosítása keresztül csatlakozik.

2. Egy alkalmazás, például víruskereső szoftver, amely egy önaláírt SSL-tanúsítvány van hogy fogadott hello HTTPS üzenetek futtatja.

Amikor Tártallózó észlel hello problémák egyike, azt már nem tudja, hogy kapott HTTPS üdvözlőüzenetére van-e illetéktelenül. Ha hello önaláírt tanúsítvány egy példányát, hogy a Tártallózó megbízható. Ha biztos ki van beszúrva hello tanúsítvány, kövesse ezeket a lépéseket toofind azt:

1. Nyissa meg az SSL telepítése

    - [Windows](https://slproweb.com/products/Win32OpenSSL.html) (hello könnyű verzióinak elegendőnek kell lennie)

    - Mac- és Linux: kell figyelembe venni az operációs rendszer

2. Nyissa meg az SSL futtatása

    - Windows: hello telepítési könyvtár megnyitásához kattintson **/bin/**, majd kattintson duplán **openssl.exe**.
    - Mac- és Linux: futtatása **openssl** terminálról.

3. Végrehajtás s_client - showcerts-csatlakozás microsoft.com:443

4. Keresse meg az önaláírt tanúsítványokat. Ha biztos benne, amelyeket önaláírt, keresse meg bárhol hello tulajdonos ("%s") és kibocsátó ("i:") van hello azonos.

5. Miután megtalálta az összes önaláírt tanúsítványokat, mindegyikhez, másolja be minden-kra **---BEGIN CERTIFICATE---** túl**---vége tanúsítvány---** tooa új .cer kiterjesztésű fájlba.

6. Nyissa meg a Tártallózót, kattintson a **szerkesztése** > **SSL-tanúsítványok** > **importálási tanúsítványok**, és majd használata hello fájl objektumválasztó toofind, jelölje be, majd nyissa meg a létrehozott hello .cer kiterjesztésű fájlokat.

Ha bármely hello a fenti lépések használatával önaláírt tanúsítványok nem találja, kapcsolatfelvétel hello visszajelzés eszköz további segítséget itt találhat.

### <a name="unable-tooretrieve-subscriptions"></a>Nem lehet tooretrieve előfizetések

Ha nem tooretrieve az előfizetések, miután sikeresen jelentkezzen be, kövesse ezeket a lépéseket tootroubleshoot probléma:

- Győződjön meg arról, hogy a fiók rendelkezik-e hozzáférési toohello előfizetések Ha bejelentkezik hello Azure-portálon.

- Győződjön meg arról, hogy bejelentkezett hello megfelelő környezet (Azure, Azure Kína, Azure Németország, Azure Amerikai Egyesült államokbeli kormányzati vagy egyéni környezet vagy az Azure-verem) használatával.

- Ha a rendszer proxy mögött, győződjön meg arról, hogy hello Tártallózó proxy megfelelően van konfigurálva.

- Próbálja meg eltávolítani, és olvasása a következő hello fiók.

- Törölje a következő fájlok a gyökérkönyvtárából (Ez azt jelenti, hogy C:\Users\ContosoUser), és majd ismételt felvételével hello fiók hello:

    - .adalcache

    - .devaccounts

    - .extaccounts

- A Watch hello fejlesztői eszközök konzol (F12 billentyű megnyomásával) amikor bejelentkezik a hibaüzenet:

![Fejlesztői eszközök](./media/storage-explorer-troubleshooting/4022501_en_2.png)

### <a name="unable-toosee-hello-authentication-page"></a>Nem lehet toosee hello hitelesítés lap

Ha nem toosee hello hitelesítési lapot, kövesse ezeket a lépéseket tootroubleshoot probléma:

- Attól függően, hogy a kapcsolat sebességétől hello azt is igénybe vehet igénybe az hello bejelentkezési oldal tooload, hello párbeszédpanel bezárása előtt legalább egy percig várjon.

- Ha a rendszer proxy mögött, győződjön meg arról, hogy hello Tártallózó proxy megfelelően van konfigurálva.

- Nézet hello fejlesztői konzolján hello F12 billentyű lenyomása mellett. Hello válaszok hello fejlesztői konzolról nézze meg, és ellenőrizze, hogy található bármely clue ennek okát a hitelesítés nem működik.

### <a name="cannot-remove-account"></a>Fiók nem távolítható el.

Ha Ön nem tooremove egy fiókot, vagy hello újrahitelesítés elemre hivatkozás nem befolyásolja, kövesse ezeket a lépéseket tootroubleshoot probléma:

- Törölje a következő fájlok a gyökérkönyvtárából, majd olvasása a következő hello fiók hello:

    - .adalcache

    - .devaccounts

    - .extaccounts

- Ha szeretné, hogy tooremove SAS csatlakoztatott tároló-erőforrások, törölje a következő fájlok hello:

    - A Windows %AppData%/StorageExplorer mappa

    - /Felhasználók/ < sajat_nev >/Library/Applicaiton támogatási/StorageExplorer Mac rendszerre

    - Linux ~/.config/StorageExplorer

> [!NOTE]
>  Hogy tooreenter a hitelesítő adatok Ha törli ezeket a fájlokat.

## <a name="proxy-issues"></a>Proxy problémák

Először is győződjön meg arról, hogy hello a következő megadott adatok helyességét:

- hello proxy URL-cím és port száma

- Felhasználónév és jelszó igényei szerint hello proxy

### <a name="common-solutions"></a>Közös megoldások

Ha továbbra is problémákat tapasztal, kövesse ezeket a lépéseket tootroubleshoot őket:

- Toohello Internet a proxy használata nélkül is elérheti, győződjön meg arról, hogy működik-e a Tártallózó nélkül proxy-beállítások engedélyezve vannak. Ha ez helyzet hello, valószínűleg a proxybeállítások kapcsolatos problémát. A proxy felügyeleti tooidentify hello problémák működik.

- Győződjön meg arról, hogy más alkalmazások hello proxykiszolgáló használata a várt módon működik-e.

- Gondoskodjon arról, hogy csatlakozhasson a webböngésző segítségével toohello Microsoft Azure-portálon

- Győződjön meg arról, hogy fogadhat válaszok a végpontok. Adja meg a végpont URL-címek egyikét a böngészőbe. Ha csatlakoztat, egy InvalidQueryParameterValue vagy hasonló XML-válasz kell kapnia.

- Ha a proxykiszolgáló valaki más is használja a Tártallózó, győződjön meg arról, hogy csatlakozhassanak. Ha a csatlakozás, előfordulhat, hogy toocontact a proxy kiszolgáló rendszergazdájával.

### <a name="tools-for-diagnosing-issues"></a>A problémák diagnosztizálásával eszközök

Ha hálózati eszközök, például a Fiddler a Windows, hogy az alábbiak szerint képes toodiagnose hello problémák merülhetnek fel:

- Ha a proxyn keresztül történő toowork, előfordulhat, hogy tooconfigure a hálózati eszköz tooconnect hello proxyn keresztül.

- Ellenőrizze a hálózati eszköz által használt hello portszámot.

- Adja meg a hello localhost URL-cím és hello hálózati eszköz portszámot a Tártallózó proxykiszolgáló-beállításként. Ebben az esetben megfelelően, ha a hálózati eszköz elindítja a hálózati kérelmek Tártallózó toomanagement és a szolgáltatási végpont által végzett naplózás. Például adja meg a blob-végpontot egy böngészőben https://cawablobgrs.blob.core.windows.net/, és választ kap alábbihoz hello, ami alapján a hello erőforrás létezik-e, bár nem férhet hozzá.

![kódminta](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a>Lépjen kapcsolatba a proxy-kiszolgálói rendszergazda

Ha a proxybeállításai megfelelőek, előfordulhat, hogy toocontact a proxy server rendszer rendszergazdájához, és

- Győződjön meg arról, hogy a proxy blokkolja forgalom tooAzure felügyeleti vagy erőforrás-végpontot.

- Ellenőrizze a proxy server által használt hello hitelesítési protokoll. A Tártallózó jelenleg nem támogatja az NTLM-proxyk.

## <a name="unable-tooretrieve-children-error-message"></a>"Nem tooRetrieve gyermekek" hibaüzenet jelenik meg

Ha olyan proxyn keresztül csatlakoztatott tooAzure, győződjön meg arról, hogy a proxybeállítások helyességéről. Hozzáférés tooa erőforrás kaptak hello tulajdonos hello előfizetés vagy a fiókot, ha győződjön meg arról, hogy elolvasta, vagy engedélyeket az adott erőforrás listában.

### <a name="issues-with-sas-url"></a>SAS URL-cím problémái
Ha egy SAS URL-cím segítségével, és ezt a hibát tapasztaló tooa szolgáltatás csatlakozik:

- Győződjön meg arról, hogy hello URL-cím biztosítanak hello szükséges engedélyek tooread vagy a lista erőforrásokat.

- Győződjön meg arról, hogy hello URL-cím nem járt le.

- Hello SAS URL-cím alapú hozzáférési házirend, győződjön meg arról, hogy hello házirend nincs visszavonva.

## <a name="next-steps"></a>Következő lépések

Ha hello megoldások egyike sem működik, az Ön, küldje el a problémát az e-mail hello visszajelzés eszközzel, és annyi részleteit tartalmazza, ha Ön hello probléma is, így elküldhetjük Önnek a hello a probléma kijavítása.

toodo, kattintson **súgó** menüben, majd kattintson **visszajelzés küldése**.

![Visszajelzés](./media/storage-explorer-troubleshooting/4022503_en_1.png)
