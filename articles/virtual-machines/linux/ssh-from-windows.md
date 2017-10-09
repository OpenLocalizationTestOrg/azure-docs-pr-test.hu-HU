---
title: "a Windows, Linux virtuális gépek kulcsok aaaUse SSH |} Microsoft Docs"
description: "Ismerje meg, hogyan toogenerate és -felhasználási SSH kulcsok Windows számítógép tooconnect tooa Linux virtuális gépre az Azure-on."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 2cacda3b-7949-4036-bd5d-837e8b09a9c8
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: danlep
ms.openlocfilehash: 6c44217332538857cc2ca2e85de4b476aa71251c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-ssh-keys-with-windows-on-azure"></a>Hogyan tooUse SSH-kulcsok a Windows Azure
> [!div class="op_single_selector"]
> * [Windows](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> * [Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
>
>

TooLinux virtuális gépek (VM) az Azure-ban kapcsolódáskor használjon [nyilvános kulcsú](https://wikipedia.org/wiki/Public-key_cryptography) tooprovide egy biztonságosabb módszer toolog tooyour Linux virtuális gép található. A folyamat során a nyilvános és titkos kulcs az exchange rendszer hello secure shell (SSH) parancs tooauthenticate saját kezűleg ahelyett, hogy a felhasználónév és jelszó. A jelszóban sebezhető toobrute-módszerén alapuló támadások, különösen az internetre irányuló virtuális gépeken futó például. Ez a cikk áttekintést nyújt az SSH-kulcsokat, és hogyan toogenerate hello Windows rendszerű számítógépeken a megfelelő kulcsokkal.

## <a name="overview-of-ssh-and-keys"></a>SSH és a kulcsok – áttekintés
Biztonságosan bejelentkezhet tooyour Linux virtuális gép nyilvános és titkos kulcsok használatával:

* Hello **nyilvános kulcs** a Linux virtuális Gépet, vagy bármely más, hogy kívánja-e a nyilvános kulcsú titkosítás toouse szolgáltatás el van helyezve.
* Hello **titkos kulcs** esetén milyen meg jelenlegi tooyour Linux virtuális gép jelentkezik be, tooverify a személyazonosságát. Védje a titkos kulcsot. Ne ossza meg senkivel.

A nyilvános és titkos kulcsokat több virtuális gépek és szolgáltatások is használható. Nem kell minden virtuális gép vagy szolgáltatás tooaccess kívánja egy kulcspárra van szüksége. Részletesebb áttekintéséért lásd: [nyilvános kulcsú](https://wikipedia.org/wiki/Public-key_cryptography).

Az SSH egy olyan titkosított kapcsolati protokoll, amely lehetővé teszi, hogy a biztonságos bejelentkezés titkosítatlan kapcsolaton keresztül. Hello alapértelmezett csatlakozási protokoll Azure szolgáltatásban üzemeltetett Linux virtuális gépek esetén is. Habár maga SSH titkosított kapcsolatot biztosít, jelszavak használata SSH-kapcsolatok továbbra is hagyja, hello VM sebezhető toobrute-módszerén alapuló támadások vagy a jelszavak találgatás. A biztonságosabb és előnyben részesített csatlakozó tooa SSH használó virtuális gépek módszer a nyilvános és titkos kulcsok, más néven az SSH-kulcsok használatával.

Ha nem kíván toouse SSH-kulcsok, továbbra is bejelentkezhet tooyour Linux virtuális gépek jelszó használatával. Ha a virtuális gép nem kitett toohello Internet, elegendő jelszóval lehet. Azonban továbbra is a jelszavak toomanage kell az egyes Linux virtuális gépek és karbantartása a megfelelő jelszót házirendeket és gyakorlatokat, például a jelszó minimális hossza, és rendszeresen frissíteni azokat. SSH-kulcsok hello használata csökkenti a hello összetettsége egyéni hitelesítő adatok kezelése több virtuális gépek között.

## <a name="windows-packages-and-ssh-clients"></a>Windows-csomagok és az SSH-ügyfél
Csatlakozás tooand Linux virtuális gépek kezelése az Azure használatával egy **SSH-ügyfél**. Windows rendszerű számítógépek általában nincs egy SSH-ügyfél telepítve. Windows 10 évforduló frissítés hello hozzáadott Bash a Windows, és hello Windows 10 Creators legfrissebb nyújt további frissítések. A Linux Windows alrendszere lehetővé teszi például egy SSH-ügyfél alapértelmezés szerint a rendszerhéjakba belül toorun és hozzáférési segédprogramok. Kövesse a hello Linux dokumentumok, például a [hogyan toogenerate SSH-kulcsot a Linux párokat](mac-create-ssh-keys.md). Bash Windows még fejlesztés alatt áll, ezért a bétaverzió tekinthető. A Windows Bash kapcsolatos további információkért lásd: [Windows Ubuntu a Bash](https://msdn.microsoft.com/commandline/wsl/about).

Ha a toouse valami Bash a Windowstól eltérő, közös Windows SSH ügyfelek telepítése hello csomagok a következő tartalmazza:

* [A Windows Git](https://git-for-windows.github.io/)
* [a puTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
* [MobaXterm](http://mobaxterm.mobatek.net/)
* [Cygwin](https://cygwin.com/)


## <a name="which-key-files-do-you-need-toocreate"></a>Amely kulcs fájlok toocreate van szüksége?
Az Azure-nak, legalább 2048 bites **ssh-rsa** formátumú nyilvános és titkos kulcsok. Azure-erőforrások hello klasszikus telepítési modell használatával felügyeli, is szükség van egy PEM toogenerate (`.pem` fájl).

Az alábbiakban hello telepítési helyzetekben, és minden használni kívánt hello típusú:

1. **ssh-rsa** hello segítségével minden telepítésében szükség [Azure-portálon](https://portal.azure.com), és a Resource Manager üzembe helyezése hello [Azure CLI](../../cli-install-nodejs.md).
   * Ezek a kulcsok rendszerint minden legtöbbek számára szükséges.
2. A `.pem` fájl szükséges toocreate virtuális gépek hello klasszikus központi telepítéssel. Ezek a kulcsok hello használata esetén támogatottak klasszikus telepítések [Azure-portálon](https://portal.azure.com) vagy [Azure CLI](../../cli-install-nodejs.md).
   * Csak akkor kell toocreate ezeket a további kulcsoknak és tanúsítványoknak hello klasszikus telepítési modellel készült erőforrások kezelése.

## <a name="install-git-for-windows"></a>Git for Windows telepítése
hello előző szakaszban felsorolt több csomagot, amely tartalmazza az hello `openssl` eszköz Windows rendszerhez. Ez az eszköz a szükséges toocreate nyilvános és titkos kulcsok. a következő példák részletes hogyan hello tooinstall, és **Git for Windows**, abban az esetben, ha bármelyik csomag inkább választhat. **Git for Windows** hozzáférést tud biztosítani, hogy toosome további nyílt forráskódú szoftvereket ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) eszközök és segédeszközök Linux virtuális gépek használata hasznos lehet.

1. Töltse le és telepítse **Git for Windows** a hello a következő helyen: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).
2. Fogadja el a hello alapértelmezett beállítások hello során a telepítés folyamata hacsak nincs kifejezetten szüksége a toochange őket.
3. Futtatás **Git bash eszközt** a hello **Start menü** > **Git** > **Git bash eszközt**. hello konzol a következő példa hasonló toohello néz ki:

    ![Git a Windows Bash rendszerhéjat](./media/ssh-from-windows/git-bash-window.png)

## <a name="create-a-private-key"></a>A titkos kulcs létrehozása
1. Az a **Git bash eszközt** ablakban használata `openssl.exe` toocreate egy titkos kulccsal. hello alábbi példa létrehoz egy nevű kulcsot `myPrivateKey` és nevű tanúsítvány `myCert.pem`:

    ```bash
    openssl.exe req -x509 -nodes -days 365 -newkey rsa:2048 \
        -keyout myPrivateKey.key -out myCert.pem
    ```

    hello kimenete a következő példa hasonló toohello néz ki:

    ```bash
    Generating a 2048 bit RSA private key
    .......................................+++
    .......................+++
    writing new private key too'myPrivateKey.key'
    -----
    You are about toobe asked tooenter information that will be incorporated
    into your certificate request.
    What you are about tooenter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', hello field will be left blank.
    -----
    Country Name (2 letter code) [AU]:
    ```

   Ha bash hiba azt jelenti, próbálkozzon egy új **Git bash eszközt** ablakot emelt szintű jogosultságokkal. Ezután futtassa újra a hello `openssl` parancsot.

2. Válasz hello megadását kéri az ország-név, a helyet, a szervezet neve, a stb.
3. Az új titkos kulcsot és tanúsítványt az aktuális munkakönyvtárban jönnek létre. Biztonsági okból kell hello engedélyeinek beállítása a titkos kulcsot, hogy csak Ön férhessen hozzá:

    ```bash
    chmod 0600 myPrivateKey.key
    ```

4. Hello [következő szakasz](#create-a-private-key-for-putty) PuTTYgen tooboth használatával részletek megtekintése és hello nyilvános kulcsot használ, és a titkos kulcs PuTTY tooSSH tooLinux virtuális gépek az adott létrehozása. hello alábbi parancs létrehozza nevű nyilvános kulcsfájlt `myPublicKey.key` azonnal használható:

    ```bash
    openssl.exe rsa -pubout -in myPrivateKey.key -out myPublicKey.key
    ```

5. Ha toomanage klasszikus erőforrásokat is szükség van, alakítsa át hello `myCert.pem` túl`myCert.cer` (DER kódolású X509 tanúsítvány). Hajtsa végre ezt a lépést nem kötelező, csak akkor, ha toospecifically kell régebbi klasszikus erőforrások kezelése.

    Alakítsa át a következő parancs hello hello-tanúsítvány:

    ```bash
    openssl.exe  x509 -outform der -in myCert.pem -out myCert.cer
    ```

## <a name="create-a-private-key-for-putty"></a>Hozzon létre egy titkos kulcsot a putty rendszerhez
A puTTY gyakori SSH-ügyfél Windows rendszerre. Meg vannak szabad toouse kívánja SSH ügyfelek. a PuTTY toouse, meg kell toocreate egy további típusú kulcsot – a PuTTY titkos kulcs (PPK). Ha nem szeretné a toouse PuTTY, hagyja ki az ebben a szakaszban.

hello alábbi példakód létrehozza a kifejezetten a PuTTY toouse további titkos kulcs:

1. Használjon **Git bash eszközt** tooconvert a titkos kulcs titkos RSA-kulcs az, hogy a PuTTYgen tudja értelmezni. hello alábbi példa létrehoz egy nevű kulcsot `myPrivateKey_rsa` nevű hello meglévő kulcsból `myPrivateKey`:

    ```bash
    openssl rsa -in ./myPrivateKey.key -out myPrivateKey_rsa
    ```

    Biztonsági okból kell hello engedélyeinek beállítása a titkos kulcsot, hogy csak Ön férhessen hozzá:

    ```bash
    chmod 0600 myPrivateKey_rsa
    ```
2. Töltse le és futtassa a következő helyen hello PuTTYgen: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
3. Hello menüjét: **fájl** > **terhelés titkos kulcs**
4. Keresse meg a titkos kulcsot (`myPrivateKey_rsa` hello előző példában). hello alapértelmezett címtár indításakor **Git bash eszközt** van `C:\Users\%username%`. Módosítsa a hello fájl szűrő tooshow **minden fájl (\*.\*)** :

    ![Hello meglévő titkos kulcs betöltése a PuTTYgen](./media/ssh-from-windows/load-private-key.png)
5. Kattintson a **nyitott**. A kérdés azt jelzi, hogy hello kulcs sikeresen importálva lett:

    ![Az importálás sikeres kulcs tooPuTTYgen](./media/ssh-from-windows/successfully-imported-key.png)
6. Kattintson a **OK** tooclose hello kérdés.
7. nyilvános kulcs hello jelenik meg a hello hello tetején **PuTTYgen** ablak. Másolja és illessze be a nyilvános kulcs hello Azure-portálon vagy az Azure Resource Manager sablon Linux virtuális gép létrehozásakor. Is **mentés nyilvános kulcs** toosave másolási tooyour számítógép:

    ![PuTTY nyilvánoskulcs-fájl mentése](./media/ssh-from-windows/save-public-key.png)

    hello alábbi példa bemutatja, hogyan szeretné másolja és illessze be a nyilvános kulcs hello Azure-portálon való Linux virtuális gép létrehozásakor. nyilvános kulcs hello általában aztán a `~/.ssh/authorized_keys` a új virtuális gépén.

    ![Nyilvános kulcs használata a virtuális gépek létrehozásakor a hello Azure-portálon](./media/ssh-from-windows/use-public-key-azure-portal.png)
8. Vissza a **PuTTYgen**, kattintson a **titkos kulcs mentése**:

    ![A PuTTY titkos kulcs fájl mentése](./media/ssh-from-windows/save-ppk-file.png)

   > [!WARNING]
   > A kérdés megkérdezi, hogy ha toocontinue kívánja-e a kulcsot a jelszó megadása nélkül. Jelszó szükséges jelszó csatolt tooyour titkos kulcsa hasonlít. Akkor is, ha valaki tooobtain volt a titkos kulcsot, azok továbbra is nem lenne képes tooauthenticate csak hello kulcs használatával. Hello jelszót is kell. Nélkül egy hozzáférési kódot Ha valaki beszerzi a titkos kulcsot, akkor is tooany Virtuálisgép jelentkezni vagy adott kulcsot használja. Azt javasoljuk, hogy hozzon létre egy hozzáférési kódot. Azonban ha elfelejti hello jelszó, nincs nincs módja toorecover azt.
   >
   >

    Ha tooenter egy hozzáférési kódot, kattintson **nem**, hello fő PuTTYgen ablakban adja meg, és kattintson a **mentés titkos kulcs** újra. Ellenkező esetben kattintson a **Igen** toocontinue hello választható jelszó megadása nélkül.
9. Adjon meg egy név és hely toosave a PPK fájlt.

## <a name="use-putty-toossh-tooa-linux-machine"></a>Használja a Putty tooSSH tooa Linux rendszerű számítógép
A PuTTY újra, a gyakori SSH-ügyfél Windows. Meg vannak szabad toouse kívánja SSH ügyfelek. a következő lépések részletes hogyan hello toouse a titkos kulcs tooauthenticate együtt az Azure virtuális gép SSH használatával. hello lépések hasonlóak a más SSH-kulcs ügyfél kellene tooload a titkos kulcs tooauthenticate hello SSH-kapcsolat tekintetében.

1. Letöltése és futtatása a putty a hello a következő helyen: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2. Töltse ki hello állomásnevet vagy IP-címet a virtuális gép a hello Azure-portálon:

    ![Új PuTTY kapcsolat megnyitása](./media/ssh-from-windows/putty-new-connection.png)
3. Mielőtt kiválasztja **nyissa meg**, kattintson a **kapcsolat** > **SSH** > **Auth** lapon. Tallózás tooand a titkos kulcs kiválasztása:

    ![Válassza ki a PuTTY titkos kulcsot a hitelesítéshez](./media/ssh-from-windows/putty-auth-dialog.png)
4. Kattintson a **nyitott** tooconnect tooyour virtuális gép

## <a name="next-steps"></a>Következő lépések
Hello nyilvános és titkos kulcsokat is létrehozhat [OS X-és Linux](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

További információ a Bash a Windows és a hello előnyei OSS eszközöket a Windows-számítógépen: [Windows Ubuntu a Bash](https://msdn.microsoft.com/commandline/wsl/about).

Ha gondja támad SSH tooconnect tooyour Linux virtuális gépek használatával, lásd: [hibaelhárítása SSH kapcsolatok tooan Azure Linux virtuális gép](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
