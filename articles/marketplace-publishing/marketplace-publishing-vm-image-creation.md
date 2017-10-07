---
title: "a virtuálisgép-lemezkép az Azure piactér hello aaaCreating |} Microsoft Docs"
description: "Hogyan toocreate egy virtuális gép rendszerképet az Azure piactér hello mások toopurchase részletes utasításokat."
services: Azure Marketplace
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 5c937b8e-e28d-4007-9fef-624046bca2ae
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 01/05/2017
ms.author: hascipio; v-divte
ms.openlocfilehash: 65e1c0530bb050fb379a52544e36c55faacd84df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="guide-toocreate-a-virtual-machine-image-for-hello-azure-marketplace"></a>Az útmutató toocreate egy virtuálisgép-lemezkép az Azure piactér hello
Ez a cikk **2. lépés**, bemutatja, hogyan hello virtuális merevlemezeket (VHD), hogy telepíteni fogja a toohello Azure piactér előkészítése. A virtuális merevlemezek hello foundation a metódust. hello folyamat eltér attól függően, hogy meg van adva egy Linux- vagy Windows-alapú Termékváltozat. Ez a cikk mindkét forgatókönyvet ismertet. Ez a folyamat végrehajtható párhuzamosan [fióklétrehozás és a regisztrációs][link-acct-creation].

## <a name="1-define-offers-and-skus"></a>1. Ajánlatok és termékváltozatok megadása
Ebben a szakaszban megismerheti a toodefine hello ajánlatok és azok kapcsolódó termékváltozatok.

Az ajánlat nem a "parent" tooall az SKU. Több ajánlattal is rendelkezhet. Hogyan úgy dönt, hogy toostructure a ajánlatok tooyou működik. Amikor egy ajánlatot a rendszer előkészítésre toostaging, és az SKU valamennyi topológiájával. A Termékváltozat azonosítók alaposan gondolja át, mert azok fog megjelenni a hello URL-címe:

* Azure.com webhelyre: http://azure.microsoft.com/marketplace/partners/ {PartnerNamespace} / {OfferIdentifier}-{SKUidentifier}
* Az Azure betekintő portál: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {SKUIDdentifier}  

Egy termékváltozata hello kereskedelmi nevet a Virtuálisgép-lemezképet. Virtuálisgép-lemezképet tartalmazza egy operációs rendszert tároló lemezének és nulla vagy több adatlemezek. Lényegében hello teljes tárolási profilt a virtuális gép. Egy virtuális merevlemez lemezenként van szükség. Akkor is üres adatlemez a létrehozott VHD toobe szükséges.

Függetlenül attól, milyen operációs rendszert használ csak hello minimális adatlemezek száma hello Termékváltozat szükséges hozzá. Az ügyfelek hello időpontban a központi telepítés lemezkép részét képező lemez nem távolítható el, de mindig hozzáadhat lemezeket során vagy a telepítés utáni Ha szükségük van rá.

> [!IMPORTANT]
> **Ne változtassa meg egy új verzióban kép lemez számát.** Ha újra kell konfigurálnia az adatlemezek hello kép, adja meg egy új Termékváltozat. Egy új lemezkép verziója és a másik lemezt darabszámukból közzététele hello lehetséges hello új lemezkép verziószámú változata alapján azokban az esetekben az ARM-sablonokat és az egyéb forgatókönyvek megoldások automatikus skálázást, az automatikus központi telepítése új telepítés megszakítása fog rendelkezni.
>
>

### <a name="11-add-an-offer"></a>1.1 ajánlatot hozzáadása
1. Jelentkezzen be toohello [közzétételi Portáljára] [ link-pubportal] a értékesítő fiók használatával.
2. Jelölje be hello **virtuális gépek** hello közzétételi Portáljára lapján. Hello felszólító bejegyzés mezőben adja meg az ajánlat nevét. hello ajánlat neve az általában hello név hello termék vagy szolgáltatás az Azure piactér hello toosell megtervezni.
3. Kattintson a **Létrehozás** gombra.

### <a name="12-define-a-sku"></a>1.2 a Termékváltozat meghatározása
Miután hozzáadta az ajánlatot, toodefine szükségesek, és azonosíthatja a termékváltozat. Több ajánlatok akkor is, és minden ajánlat lehet több SKU rajta. Amikor egy ajánlatot a rendszer előkészítésre toostaging, és az SKU valamennyi topológiájával.

1. **Adja hozzá a Termékváltozat.** hello Termékváltozat szükséges egy azonosító, amely hello URL-cím szerepel. hello azonosítója a közzétételi profil belül egyedieknek kell lenniük, de nincs más gyártók azonosító ütközés kockázata.

   > [!NOTE]
   > hello ajánlatot és az SKU-azonosítók a piactér hello hello ajánlat URL-cím jelenik meg.
   >
   >
2. **Rövid szöveges ismertetése hozzáadása a termékváltozat.** Összefoglaló leírása látható toocustomers, ezért meg kell győződnie azok könnyen olvasható. Ez az információ nem kell toobe amíg hello "Leküldéses tooStaging" szakasz zárolva van. Addig is szabad tooedit azt.
3. Ha Windows-alapú termékváltozatok használ, hivatkozásokból hello javasolt tooacquire hello jóváhagyott Windows Server verzióit.

## <a name="2-create-an-azure-compatible-vhd-linux-based"></a>2. Hozzon létre egy Azure-kompatibilis VHD-t (Linux-alapú)
Ez a szakasz az ajánlott eljárások az Azure piactér hello Linux-alapú Virtuálisgép-lemezkép létrehozásához összpontosít. Részletes útmutató, tekintse meg a következő dokumentáció toohello: [létrehozása, majd ismét feltölteni a virtuális merevlemez a Contains hello Linux operációs rendszer](../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

## <a name="3-create-an-azure-compatible-vhd-windows-based"></a>3. Hozzon létre egy Azure-kompatibilis VHD-t (Windows-alapú)
Ez a szakasz elsősorban hello lépéseket toocreate egy Windows Server meghatározásával hello Azure piactér Termékváltozat.

### <a name="31-ensure-that-you-are-using-hello-correct-base-vhds"></a>3.1-es győződjön meg arról, hogy azok be hello javítsa ki az alap virtuális merevlemezek
hello operációs rendszer virtuális Merevlemeze a VM-lemezkép az Azure-jóváhagyott alapjául szolgáló lemezképhez, amely tartalmazza a Windows Server vagy SQL Server kell alapulnia.

toobegin, hozzon létre egy virtuális Gépet a következő lemezképek helye: hello hello [Microsoft Azure-portálon][link-azure-portal]:

* Windows Server ([2012 R2 Datacenter][link-datactr-2012-r2], [2012 Datacenter][link-datactr-2012], [2008 R2 SP1] [link-datactr-2008-r2])
* SQL Server 2014 ([vállalati][link-sql-2014-ent], [szabványos][link-sql-2014-std], [webes] [ link-sql-2014-web])
* Az SQL Server 2012 SP2 ([vállalati][link-sql-2012-ent], [szabványos][link-sql-2012-std], [webes] [ link-sql-2012-web])

Ezeket a hivatkozásokat is hello közzétételi Portáljára hello SKU lap alatt található.

> [!TIP]
> Hello Azure-portálon vagy a PowerShell használ, ha jóváhagyott közzétett 2014. szeptember 8 és újabb Windows Server-lemezképekben.
>
>

### <a name="32-create-your-windows-based-vm"></a>3.2-es verzióját a Windows-alapú virtuális gép létrehozása
Hello Microsoft Azure portálon a virtuális Gépet egy jóváhagyott alapjául szolgáló lemezképhez, csupán néhány egyszerű lépésben alapján is létrehozhat. hello az alábbiakban látható hello folyamat áttekintése:

1. Hello alapjául szolgáló lemezképhez lapon válassza ki a **virtuális gép létrehozása** toobe irányított toohello új [Microsoft Azure-portálon][link-azure-portal].

    ![rajz][img-acom-1]
2. Jelentkezzen be a toohello portál hello Microsoft-fiók és jelszó hello toouse kívánt Azure-előfizetés.
3. Hajtsa végre a hello kér toocreate egy virtuális gép hello alapjául szolgáló lemezképhez kijelölt használatával. Szükség van tooprovide egy állomásnevet (hello számítógép nevét), a felhasználói név (rendszergazda) és a jelszó hello virtuális gép.

    ![rajz][img-portal-vm-create]
4. Válassza ki a virtuális gép toodeploy hello hello méretét:

    a.    Ha azt tervezi, toodevelop hello VHD-t a helyszíni, hello mérete nem számít. Érdemes lehet egy hello kisebb virtuális gépeket.

    b.    Ha azt tervezi, toodevelop hello lemezkép az Azure-ban, fontolja meg a Virtuálisgép-méretek hello kiválasztott lemezképhez hello egyikével ajánlott.

    c.    Díjszabási információkért tekintse meg a toohello **ajánlott a tarifacsomagok** választó hello portal oldalon található. Hello hello közzétevőtől három az ajánlott méreteket biztosítja. (Ebben az esetben hello közzétevője Microsoft.)

    ![rajz][img-portal-vm-size]
5. Állítsa be a tulajdonságokat:

    a.    Gyors üzembe helyezés esetén hagyhatja hello alatt hello tulajdonságok alapértelmezett értékeinek **opcionális konfigurációs** és **erőforráscsoport**.

    b.    A **Tárfiók**, mely operációs rendszer hello VHD tárolva lesznek hello tárfiókot is választhat.

    c.    A **erőforráscsoport**, mely tooplace a virtuális gép hello hello logikai csoport is választhat.
6. Jelölje be hello **hely** központi telepítéshez:

    a.    Ha azt tervezi, toodevelop hello VHD-t a helyszíni, hello helye nem számít, mert fel kell töltenie hello kép tooAzure később.

    b.    Ha azt tervezi, toodevelop hello lemezkép az Azure-ban, célszerű az Egyesült államokbeli Microsoft Azure-régiókat hello hello elejétől. Ez felgyorsítja hello virtuális merevlemez másolása folyamatot, amely a Microsoft az Ön nevében hajt végre, amikor a hitelesítésszolgáltató kép.

    ![rajz][img-portal-vm-location]
7. Kattintson a **Create** (Létrehozás) gombra. virtuális gép hello toodeploy kezdődik. Percnél a sikeres telepítés lesz, és a termékváltozat toocreate hello kép megkezdése.

### <a name="33-develop-your-vhd-in-hello-cloud"></a>3.3 fejlesztése a VHD-n hello felhő
Határozottan javasoljuk, hogy most kialakított a VHD-n hello felhő Remote Desktop Protocol (RDP) használatával. Csatlakozás tooRDP hello felhasználó nevét és a kiépítés során megadott jelszót.

> [!IMPORTANT]
> Ha a virtuális merevlemez fejleszt helyszíni (ez nem ajánlott), lásd: [egy virtuálisgép-lemezkép létrehozásához a helyszíni](marketplace-publishing-vm-image-creation-on-premise.md). A virtuális merevlemez letöltése nincs szükség fejlesztői hello felhőben.
>
>

**Csatlakozás RDP hello segítségével [Microsoft Azure-portálon][link-azure-portal]**

1. Válassza ki **Tallózás** > **virtuális gépek**.
2. hello virtuális gépek panelje megnyílik. Győződjön meg arról, hogy hello tooconnect a kívánt virtuális Gépnek fut, majd válassza ki azt a hello telepített virtuális gépek.
3. A kijelölt virtuális gép egy panel megnyitása hello leíró. Hello lap tetején kattintson **Connect**.
4. Biztos, hogy felszólító tooenter hello felhasználónevet és a kiépítés során megadott jelszót.

**Csatlakozás RDP PowerShell használatával**

egy távoli asztali fájl tooa helyi gépen toodownload hello használata [Get-AzureRemoteDesktopFile parancsmag][link-technet-2]. A rendezés toouse ezt a parancsmagot, tooknow hello hello szolgáltatás nevét és hello virtuális gép neve van szükség. Ha létrehozott virtuális gép hello hello [Microsoft Azure-portálon][link-azure-portal], ezeket az információkat a virtuális gép tulajdonságok található:

1. Hello Microsoft Azure portálon, válassza ki a **Tallózás** > **virtuális gépek**.
2. hello virtuális gépek panelje megnyílik. Válassza ki a központilag telepített virtuális gép hello.
3. A kijelölt virtuális gép egy panel megnyitása hello leíró.
4. Kattintson a **Tulajdonságok** elemre.
5. hello első hello tartománynév része hello szolgáltatás neve. hello állomásnév hello nevet a virtuális gép.

    ![rajz][img-portal-vm-rdp]
6. hello parancsmag toodownload hello RDP-fájljának helyi asztali hello létrehozott virtuális gép toohello rendszergazda az alábbiak szerint zajlik.

        Get‐AzureRemoteDesktopFile ‐ServiceName “baseimagevm‐6820cq00” ‐Name “BaseImageVM” –LocalPath “C:\Users\Administrator\Desktop\BaseImageVM.rdp”

További információ a RDP hello a cikkben az MSDN Webhelyén található [csatlakozás tooan Azure virtuális gép RDP- vagy SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx).

**A virtuális gép konfigurálása, és hozzon létre a Termékváltozat**

Után hello operációs rendszer virtuális merevlemez letöltődik használja a Hyper-v, és konfigurálja a virtuális gép toobegin létrehozása a Termékváltozat. Részletes utasítások hello TechNet hivatkozás a következő helyen találhatók: [Hyper-v telepítése és konfigurálása a virtuális gépek](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="34-choose-hello-correct-vhd-size"></a>3.4 hello megfelelő VHD-méret kiválasztása
hello Windows operációs rendszer virtuális Merevlemeze a VM-lemezképben 128 GB-os rögzített formátumú virtuális Merevlemezt, létre kell hozni.  

Ha hello fizikai mérete kisebb, mint 128 GB-os, hello VHD ritka kell lennie. hello alap Windows és az SQL Server képek megadott már megfelel a fenti követelményeknek, ezért módosítsa hello formátuma vagy hello hello VHD kapott.  

Az adatlemezek akkora, mint 1 TB-os lehet. Amikor eldönti hello lemezméretet, ne feledje, hogy az ügyfelek nem méretezhető virtuális merevlemezek kép hello időpontban a központi telepítés. Adatok lemez VHD rögzített formátumú virtuális Merevlemezt, létre kell hozni. Akkor is kell ritka. Az adatlemezek tartalmazhatnak adatokat, de lehetnek üresek is.

### <a name="35-install-hello-latest-windows-patches"></a>3.5 telepítése hello legújabb Windows javítások
hello alap lemezképek tartalmazzák a legújabb javítások hello mentése tootheir közzétételének dátuma. Hello operációs rendszer virtuális Merevlemeze közzététele előtt hozott létre, győződjön meg arról, hogy a Windows Update futtatása és, hogy az összes hello legújabb kritikus fontos biztonsági frissítések telepítve vannak.

### <a name="36-perform-additional-configuration-and-schedule-tasks-as-necessary"></a>3.6 szükség szerint további konfigurációval és ütemezéssel feladatok végrehajtása
Ha nincs szükség további konfigurációra, érdemes lehet egy ütemezett feladatot, amely a indítási toomake bármely végleges változtatások toohello VM azt telepítése után:

* A bevált gyakorlat toohave hello feladatok törlése maga sikeres végrehajtása után.
* Konfiguráció nem igazolható a C vagy D meghajtó kívül, mert ezek hello csak két meghajtókat, amelyek mindig tooexist. C meghajtó hello operációsrendszer-lemez, és a D meghajtón hello ideiglenes helyi lemez.

### <a name="37-generalize-hello-image"></a>3.7 generalize hello kép
Az Azure piactér hello összes lemezkép újrafelhasználható általános módon kell lennie. Más szóval hello operációs rendszer virtuális Merevlemeze általánosítva kell:

* A Windows hello a lemezkép "Sysprep használatával létrehozott" lehet, és nem konfigurációk el kell végezni, amelyek nem támogatják az hello **sysprep** parancsot.
* Hello követően – hello directory % windir%\System32\Sysprep parancs futtatása.

        sysprep.exe /generalize /oobe /shutdown

  Hogyan a toosysprep hello operációs rendszer a következő MSDN-cikk hello. lépésében megadott útmutatást: [létrehozása és feltöltése a Windows Server VHD tooAzure](../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="4-deploy-a-vm-from-your-vhds"></a>4. A virtuális merevlemezek egy virtuális gép telepítése
Miután a virtuális merevlemezek (általánosítva hello operációs rendszer virtuális Merevlemeze és lemez VHD-k nulla vagy több adat) feltöltött tooan Azure storage-fiók, regisztrálhatja őket, a felhasználó a Virtuálisgép-lemezkép. Majd tesztelheti ezt a lemezképet. Vegye figyelembe, hogy az operációs rendszer virtuális Merevlemeze általánosított, mert nem közvetlenül telepíthet központilag hello VM hello virtuális merevlemez URL-cím megadásával.

toolearn további információk a Virtuálisgép-lemezképek, a következő blogbejegyzések felülvizsgálati hello:

* [A Virtuálisgép-lemezkép](https://azure.microsoft.com/blog/vm-image-blog-post/)
* [Virtuális gép lemezképének PowerShell hogyan](https://azure.microsoft.com/blog/vm-image-powershell-how-to-blog-post/)
* [Az Azure VM-lemezképekkel kapcsolatban](https://msdn.microsoft.com/library/azure/dn790290.aspx)

### <a name="set-up-hello-necessary-tools-powershell-and-azure-cli"></a>Hello szükséges eszközök, PowerShell és az Azure CLI beállítása
* [Hogyan toosetup PowerShell](/powershell/azure/overview)
* [Hogyan toosetup Azure parancssori felület](../cli-install-nodejs.md)

### <a name="41-create-a-user-vm-image"></a>4.1 a felhasználó a Virtuálisgép-lemezkép létrehozása
#### <a name="capture-vm"></a>Virtuális gép rögzítése
Olvassa el az útmutatót hello API vagy PowerShell vagy az Azure parancssori felület használatával virtuális gép rögzítése az alábbiakban hello hivatkozásokat.

* [API](https://msdn.microsoft.com/library/mt163560.aspx)
* [PowerShell](../virtual-machines/windows/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Azure CLI](../virtual-machines/linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="generalize-image"></a>Kép generalize
Olvassa el az útmutatót hello API vagy PowerShell vagy az Azure parancssori felület használatával virtuális gép rögzítése az alábbiakban hello hivatkozásokat.

* [API](https://msdn.microsoft.com/library/mt269439.aspx)
* [PowerShell](../virtual-machines/windows/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Azure CLI](../virtual-machines/linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="42-deploy-a-vm-from-a-user-vm-image"></a>4.2 egy virtuális Gépet, a felhasználói VM-lemezkép központi telepítése
felhasználói Virtuálisgép-lemezképet a virtuális gép toodeploy, hello aktuális használható [Azure-portálon](https://manage.windowsazure.com) vagy a PowerShell használatával.

**Telepítse a virtuális Gépet hello jelenlegi Azure-portálon**

1. Nyissa meg túl**új** > **számítási** > **virtuális gép** > **gyűjteményből**.

    ![rajz][img-manage-vm-new]
2. Nyissa meg túl**képek**, majd válassza ki hello mely toodeploy a Virtuálisgép-lemezkép a virtuális gépek és:

   1. Fizetési elolvassa toowhich rendszerkép választ ki, mert hello **képek** operációsrendszer-lemezképek és a Virtuálisgép-rendszerképek listák megtekintése.
   2. Megnézi hello lemezeinek száma miatt is meghatározásához, hogy milyen típusú kép telepít, mert Virtuálisgép-rendszerképek hello többsége több lemez. Azonban továbbra is lehetséges toohave csak egy egyetlen operációsrendszer-lemez, amely ezután a virtuális gép rendszerképének **lemezek számát** too1 beállítása.

      ![rajz][img-manage-vm-select]
3. Hajtsa végre a hello virtuális gép létrehozása varázsló, és adja meg a hello virtuális gép nevét, virtuális gép méretét, hely, felhasználónév és jelszó.

**A PowerShell egy virtuális gép üzembe helyezése**

hello egy nagy virtuális gép általánosítva most hozott létre a Virtuálisgép-lemezkép toodeploy, a következő parancsmagok hello is használhatja.

    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot

> [!IMPORTANT]
> További segítségért tekintse meg az [Hibaelhárítás gyakori problémákat észlelt a virtuális merevlemez létrehozása közben].
>
>

## <a name="5-obtain-certification-for-your-vm-image"></a>5. A VM-lemezkép tanúsítvány beszerzése
a Virtuálisgép-lemezkép előkészítése hello Azure piactér következő lépése hello toohave akkor.

Ez a folyamat tartalmazza egy különös hitelesítő eszközt futtató, hello ellenőrzési eredmények toohello feltöltése Azure tároló, ahol a virtuális merevlemezek vannak, ajánlatot hozzáadása, a Termékváltozat meghatározása és elküldése a virtuális gép kép hitelesítésszolgáltató számára.

### <a name="51-download-and-run-hello-certification-test-tool-for-azure-certified"></a>5.1 letöltése és futtatása hello hitelesítő vizsgálati eszköz az Azure hitelesített
hello hitelesítő eszközt futtatja a futó virtuális gép, a felhasználó a Virtuálisgép-lemezkép kiosztott, amely a Virtuálisgép-lemezkép hello tooensure Microsoft Azure-ral kompatibilis. Azt ellenőrzi, hogy hello útmutatás és a virtuális merevlemez előkészítésével kapcsolatos követelmények teljesülnek. hello hello eszköz eredménye egy kompatibilitási jelentést, amely a kérelmező hitelesítésszolgáltató közben közzétételi Portáljára hello fel kell tölteni.

hello hitelesítő eszköz Windows és Linux virtuális gépek egyaránt használható. TooWindows-alapú virtuális gépek Powershellen keresztül csatlakozik, és tooLinux virtuális gépek SSH.Net keresztül csatlakozik:

1. Első lépésként töltse le a hello hitelesítő eszközt: hello [Microsoft letöltési hely][link-msft-download].
2. Nyissa meg a hello hitelesítő eszközt, és kattintson a hello **új Teszt indítása** gombra.
3. A hello **tesztelése információk** képernyőn, adjon meg egy nevet hello teszt futtatásakor.
4. Adja meg, hogy Linux vagy Windows rendszerű virtuális gépről van-e szó. Attól függően, amelyek választja adja meg hello további beállításokat.

### <a name="connect-hello-certification-tool-tooa-linux-vm-image"></a>**Csatlakozás hello hitelesítő eszköz tooa Linux Virtuálisgép-lemezkép**
1. Jelölje be hello SSH hitelesítési mód: jelszó vagy a kulcs fájlt.
2. Ha a jelszó-alapú hitelesítést használ, adja meg a hello tartománynévrendszer (DNS) neve, a felhasználónév és jelszó.
3. Ha kulcsfájl-hitelesítést használ, adja meg a hello DNS-nevét, a felhasználónév és a titkos kulcs helye.

   ![Linux virtuális gép lemezképének jelszó-hitelesítés][img-cert-vm-pswd-lnx]

   ![Linux virtuális gép lemezképének kulcsfájl hitelesítés][img-cert-vm-key-lnx]

### <a name="connect-hello-certification-tool-tooa-windows-based-vm-image"></a>**Csatlakozás hello hitelesítő eszköz tooa Windows-alapú Virtuálisgép-lemezkép**
1. Adja meg a teljesen minősített hello VM DNS-nevét (például MyVMName.Cloudapp.net).
2. Adja meg a hello felhasználónevet és jelszót.

   ![Jelszó-hitelesítés Windows Virtuálisgép-lemezkép][img-cert-vm-pswd-win]

Miután kiválasztotta a megfelelő hello-beállítások a Linux vagy a Windows-alapú Virtuálisgép-lemezkép számára, jelölje ki a **kapcsolat tesztelése** tooensure, hogy SSH.Net vagy a PowerShell rendelkezik-e érvényes kapcsolat tesztelési célokra. A kapcsolat létrejötte után válassza ki a **következő** toostart hello tesztelése.

Hello teszt befejeződése után kapni fog hello eredmények (Pass/sikertelen/figyelmeztetés) minden egyes teszt elemhez.

![Linux virtuális gép lemezképének teszteseteinek][img-cert-vm-test-lnx]

![Windowsos VM-lemezkép teszteseteinek][img-cert-vm-test-win]

Ha bármelyik hello teszt sikertelen, a lemezkép nem kell hitelesíteni. Ha ez történik, hello követelményeinek áttekintése és a szükséges módosításokat.

Miután hello automatikus teszt, a Virtuálisgép-lemezkép egy kérdőív képernyő keresztül a tooprovide további bemeneti kell adnia.  Fejezze be a hello kérdéseket, majd válassza ki **következő**.

![Hitelesítésszolgáltató eszköz kérdőív][img-cert-vm-questionnaire]

![Hitelesítésszolgáltató eszköz kérdőív][img-cert-vm-questionnaire-2]

Hello kérdőív befejezését követően megadhatja például az SSH információ a Linux Virtuálisgép-lemezkép hello további információkat és bármely sikertelen értékelés magyarázata. Hello vizsgálati eredményeket letöltheti és hozzáadását tooyour válaszok toohello kérdőív végre hello teszteseteinek naplófájlok. A hello hello-eredményeket menteni a virtuális merevlemezek ugyanabban a tárolóban.

![Hitelesítésszolgáltató vizsgálati eredményeket menteni][img-cert-vm-results]

### <a name="52-get-hello-shared-access-signature-uri-for-your-vm-images"></a>5.2 a Virtuálisgép-lemezképek hello közös hozzáférésű jogosultságkódot URI lekérése
Során hello közzétételi folyamat adja meg hello egységes erőforrás-azonosítók (URI), amely a VHD-k a Termékváltozat létrehozott hello tooeach vezethet. Microsoft access toothese virtuális merevlemezeket kell hello hitelesítési folyamat során. Ezért szükség van egy közös hozzáférésű jogosultságkódot URI toocreate minden virtuális merevlemez. Ez a hello URI, amely kell megadni a **képek** hello közzétételi Portáljára lapján.

hello közös hozzáférésű jogosultságkódot létrehozni URI toohello követelményeknek kell megfelelniük:

* Közös hozzáférésű jogosultságkódot URI-azonosítók a VHD-k létrehozásakor listáját és az olvasási engedélyek megfelelőek. Ne adjon írási vagy törlési hozzáférést.
* hozzáférés hello időtartama kell legalább három (3) héttel hello közös hozzáférésű jogosultságkódot URI létrehozásakor.
* toosafeguard UTC szerinti idő esetén jelölje be hello nap hello aktuális dátum előtt. Ha hello aktuális dátum 2014. októberi 6 jelölje ki például 10/5/2014.

SAS URL-cím lehet generált több módon tooshare a VHD-t az Azure piactéren.
Következő 3 javasolt eszközök vannak hello:

1.  Azure Storage Explorer
2.  Microsoft Tártallózó
3.  Azure CLI

**Az Azure Tártallózó (Windows-felhasználóknak javasolt)**

Az alábbiakban hello lépéseket SAS URL-cím létrehozásához Azure Tártallózó használatával

1. Töltse le [az Azure Storage Explorer 6 Preview 3](https://azurestorageexplorer.codeplex.com/) a Codeplex webhelyen. Nyissa meg túl[Azure Storage Explorer 6 Preview](https://azurestorageexplorer.codeplex.com/) kattintson **"Letölti"**.

    ![rajz](media/marketplace-publishing-vm-image-creation/img5.2_01.png)

2. Töltse le [AzureStorageExplorer6Preview3.zip](https://azurestorageexplorer.codeplex.com/downloads/get/891668) és telepítése után kicsomagolás azt.

    ![rajz](media/marketplace-publishing-vm-image-creation/img5.2_02.png)

3. A telepítés után hello alkalmazás megnyitása.
4. Kattintson a **fiók hozzáadása**.

    ![rajz](media/marketplace-publishing-vm-image-creation/img5.2_03.png)

5. Adja meg a hello tárfiók neve, a tárfiók kulcsa és a tárolási végpontok tartományt. Ez az hello storage-fiókot az Azure-előfizetéshez ahol megtartotta a VHD-t az Azure-portálon.

    ![rajz](media/marketplace-publishing-vm-image-creation/img5.2_04.png)

6. Azure Tártallózó csatlakoztatása után tooyour adott tárfiókot, fog elindulni megjelenítő az összes hello hello tárfiókon belül tartalmazza. Hello operációs rendszer lemez VHD-fájlt (is adatlemezek, ha a forgatókönyvéhez alkalmazható) másolásakor hello-tároló választása.

    ![rajz](media/marketplace-publishing-vm-image-creation/img5.2_05.png)

7. Miután kiválasztotta a hello blob tároló, Azure Tártallózó ábrázoló hello fájlok hello tárolóban elindul. Hello lemezképfájl (.vhd), benyújtott toobe igénylő van kiválasztva.

    ![rajz](media/marketplace-publishing-vm-image-creation/img5.2_06.png)

8.  Hello tárolóban hello .vhd fájl kiválasztása után kattintson a hello **biztonsági** fülre.

    ![rajz](media/marketplace-publishing-vm-image-creation/img5.2_07.png)

9.  A hello **Blob tároló biztonsági** párbeszédpanelen hagyja hello alapértelmezései hello **hozzáférési szint** fülre, majd **megosztott hozzáférési aláírásokkal** fülre.

    ![rajz](media/marketplace-publishing-vm-image-creation/img5.2_08.png)

10. Kövesse az alábbi toogenerate egy közös hozzáférésű jogosultságkódot URI hello .vhd lemezkép hello lépéseket:

    ![rajz](media/marketplace-publishing-vm-image-creation/img5.2_09.png)

    a. **Engedélyezett hozzáférési:** toosafeguard UTC szerinti idő esetén jelölje be hello nap hello aktuális dátum előtt. Ha hello aktuális dátum 2014. októberi 6 jelölje ki például 10/5/2014.

    b. **Engedélyezett hozzáférési:** válassza ki a dátumot követően hello legalább három hetet **engedélyezett hozzáférési** dátum.

    c. **Engedélyezett műveletek:** válassza hello **lista** és **olvasási** engedélyek.

    d. Ha a .vhd fájlt megfelelően választotta, akkor megjelenik a fájl **Blob neve tooaccess** .vhd kiterjesztéssel együtt.

    e. Kattintson a **aláírását létrehozó**.

    f. A **létrehozott megosztott hozzáférési aláírást URI-azonosítója ebben a tárolóban**, a következő példának fent hello keresése:

       - Győződjön meg arról, hogy a lemezkép-fájl neve és **".vhd"** hello URI szerepelnek.
       - Hello aláírás hello végén győződjön meg arról, hogy **"= rl"** jelenik meg. Ez azt jelenti, hogy olvasási és lista hozzáférés sikeresen biztosítja-e.
       - Hello aláírás közepén, győződjön meg arról, hogy **"sr = c"** jelenik meg. Ez azt jelenti, hogy rendelkezik-e hozzáférési tároló

11. jön létre, amely hello tooensure megosztott hozzáférési aláírást URI működik, kattintson a **böngészőben teszt**. Akkor kell hello letöltési folyamat elindítása.

12. Másolja a hello közös hozzáférésű jogosultságkódot URI. Ez a hello URI toopaste történő hello közzétételi Portáljára.

13. Ismételje meg a 6 – 10. minden virtuális merevlemez hello Termékváltozat.

**A Microsoft Azure Tártallózó (Windows vagy MAC/Linux)**

Az alábbiakban hello lépései a Microsoft Azure Tártallózó SAS URL-cím létrehozása

1.  Töltse le a Microsoft Azure Tártallózó űrlap [http://storageexplorer.com/](http://storageexplorer.com/) webhelyet. Nyissa meg túl[Microsoft Azure Tártallózó](http://storageexplorer.com/releasenotes.html) kattintson **"Töltse le a Windows"**.

    ![rajz](media/marketplace-publishing-vm-image-creation/img5.2_10.png)

2.  A telepítés után hello alkalmazás megnyitása.

3.  Kattintson a **fiók hozzáadása**.

4.  Microsoft Azure Tártallózó tooyour előfizetés tooyour fiók bejelentkezés konfigurálása

    ![rajz](media/marketplace-publishing-vm-image-creation/img5.2_11.png)

5.  Nyissa meg toostorage fiókot, és válassza ki a tároló hello

6.  Válassza ki **"Megosztás hozzáférési aláírás beolvasása..."** Kattintson a jobb gombbal a hello segítségével **tároló**

    ![rajz](media/marketplace-publishing-vm-image-creation/img5.2_12.png)

7.  Kezdési ideje, a lejárat időpontjának és az engedélyek frissítése a következő száma

    ![rajz](media/marketplace-publishing-vm-image-creation/img5.2_13.png)

    a.  **Kezdő időpontja:** toosafeguard UTC szerinti idő esetén jelölje be hello nap hello aktuális dátum előtt. Ha hello aktuális dátum 2014. októberi 6 jelölje ki például 10/5/2014.

    b.  **Lejárati idő:** válassza ki a dátumot követően hello legalább három hetet **kezdete** dátum.

    c.  **Engedélyek:** válassza hello **lista** és **olvasási** engedélyek

8.  Másolja a tároló közös hozzáférésű jogosultságkódot URI

    ![rajz](media/marketplace-publishing-vm-image-creation/img5.2_14.png)

    Generált SAS URL-cím tároló szint és most létre kell azt tooadd virtuális merevlemez nevét.

    Tároló szintű SAS URL-címének formátuma:`https://testrg009.blob.core.windows.net/vhds?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`

    Helyezze be a virtuális merevlemez neve után hello Tárolónév SAS URL-címet az alábbi`https://testrg009.blob.core.windows.net/vhds/<VHD NAME>?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`

    Példa:

    ![rajz](media/marketplace-publishing-vm-image-creation/img5.2_15.png)

    Virtuális merevlemez SAS URL-t nem TestRGVM201631920152.vhd hello virtuális merevlemez neve`https://testrg009.blob.core.windows.net/vhds/TestRGVM201631920152.vhd?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`

    - Győződjön meg arról, hogy a lemezkép-fájl neve és **".vhd"** hello URI szerepelnek.
    - Hello aláírás közepén, győződjön meg arról, hogy **"sp = rl"** jelenik meg. Ez azt jelenti, hogy olvasási és lista hozzáférés sikeresen biztosítja-e.
    - Hello aláírás közepén, győződjön meg arról, hogy **"sr = c"** jelenik meg. Ez azt jelenti, hogy rendelkezik-e hozzáférési tároló

9.  tooensure, amely hello létrehozott megosztott hozzáférési aláírást URI működik, tesztelheti a böngészőben. Hello letöltési folyamatot addig kell elindítani

10. Másolja a hello közös hozzáférésű jogosultságkódot URI. Ez a hello URI toopaste történő hello közzétételi Portáljára.

11. Ismételje meg ezeket a lépéseket minden virtuális merevlemez hello Termékváltozat.

**Az Azure CLI (nem Windows & folyamatos integráció ajánlott)**

Az alábbiakban hello lépései SAS URL-cím létrehozása az Azure parancssori felület használatával

1.  Töltse le a Microsoft Azure CLI [Itt](https://azure.microsoft.com/en-in/documentation/articles/xplat-cli-install/). A különböző hivatkozásait is megtalálhatja  **[Windows](http://aka.ms/webpi-azure-cli)**  és  **[MAC OS](http://aka.ms/mac-azure-cli)**.

2.  Amennyiben a letöltés, telepítse a

3.  Hozzon létre egy PowerShell fájlt a következő kódra, és mentse a helyi

          $conn="DefaultEndpointsProtocol=https;AccountName=<StorageAccountName>;AccountKey=<Storage Account Key>"
          azure storage container list vhds -c $conn
          azure storage container sas create vhds rl <Permission End Date> -c $conn --start <Permission Start Date>  

    Hello következő frissítése a fenti paraméterek

    a. **`<StorageAccountName>`**: Adjon a tárfiók neve

    b. **`<Storage Account Key>`**: Adjon a tárfiók kulcsára

    c. **`<Permission Start Date>`**: toosafeguard UTC szerinti idő esetén jelölje be hello nap hello aktuális dátum előtt. Például ha hello aktuális dátum 26 2016 októberétől kezdve, akkor az érték legyen 10/25/2016

    d. **`<Permission End Date>`**: Válassza ki a dátumot követően hello legalább három hetet **Kezdődátum**. Az érték legyen, majd **11-02-2016**.

    Az alábbiakban látható hello példakódot megfelelő paraméterek frissítése után

          $conn="DefaultEndpointsProtocol=https;AccountName=st20151;AccountKey=TIQE5QWMKHpT5q2VnF1bb+NUV7NVMY2xmzVx1rdgIVsw7h0pcI5nMM6+DVFO65i4bQevx21dmrflA91r0Vh2Yw=="
          azure storage container list vhds -c $conn
          azure storage container sas create vhds rl 11/02/2016 -c $conn --start 10/25/2016  

4.  Powershell-szerkesztő megnyitása a "Futtatás mint" üzemmódban, majd nyissa meg a fájl a #3. lépésben.

5.  Futtatási hello parancsfájlt, és biztosítja, akkor tároló hozzáférési hello SAS URL-cím

    Következő hello kimeneti hello SAS aláírás, és másolja a kijelölt hello rész egy Jegyzettömb lesz.

    ![rajz](media/marketplace-publishing-vm-image-creation/img5.2_16.png)

6.  Most tároló szint SAS URL-cím fog megjelenni, és meg kell azt tooadd virtuális merevlemez nevét.

    Tároló szintű SAS URL-cím #

    `https://st20151.blob.core.windows.net/vhds?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

7.  Helyezze be a virtuális merevlemez neve után hello Tárolónév SAS URL-címben alább látható módon`https://st20151.blob.core.windows.net/vhds/<VHDName>?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

    Példa:

    Virtuális merevlemez SAS URL-t nem TestRGVM201631920152.vhd hello virtuális merevlemez neve

    `https://st20151.blob.core.windows.net/vhds/ TestRGVM201631920152.vhd?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

    - Győződjön meg arról, hogy a kép fájlnevét és a ".vhd" hello URI.
    -   Hello aláírás közepén, győződjön meg arról, hogy "sp = rl" jelenik meg. Ez azt jelenti, hogy olvasási és lista hozzáférés sikeresen biztosítja-e.
    -   Hello aláírás közepén, győződjön meg arról, hogy "sr = c" jelenik meg. Ez azt jelenti, hogy rendelkezik-e hozzáférési tároló

8.  tooensure, amely hello létrehozott megosztott hozzáférési aláírást URI működik, tesztelheti a böngészőben. Hello letöltési folyamatot addig kell elindítani

9.  Másolja a hello közös hozzáférésű jogosultságkódot URI. Ez a hello URI toopaste történő hello közzétételi Portáljára.

10. Ismételje meg ezeket a lépéseket minden virtuális merevlemez hello Termékváltozat.


### <a name="53-provide-information-about-hello-vm-image-and-request-certification-in-hello-publishing-portal"></a>5.3 hello Virtuálisgép-lemezkép kapcsolatos adatok megadása és közzétételi Portáljára hello tanúsításhoz kérése
Miután létrehozta a ajánlatot és az SKU, írja be a Termékváltozat társított hello részletei:

1. Nyissa meg toohello [közzétételi Portáljára][link-pubportal], és jelentkezzen be a értékesítő fiókjával.
2. Jelölje be hello **Virtuálisgép-rendszerképek** fülre.
3. hello hello oldal hello tetején felsorolt azonosító ténylegesen hello ajánlat azonosítója és hello SKU azonosítója.
4. Töltse ki a hello hello tulajdonságok **termékváltozatok** szakasz.
5. A **operációsrendszer-családot**, kattintson a hello operációs rendszer virtuális Merevlemeze társított hello operációs rendszer típusa.
6. A hello **operációs rendszer** mezőbe hello operációs rendszert határozzák meg. Vegye figyelembe például az operációsrendszer-családot, a típus, a verziót és a frissítések formátumban. Példa: "Windows Server Datacenter 2014 R2."
7. Virtuálisgép-méretek ajánlott toosix kiválasztása Ezek a javaslatok, lekérése megjelenített toohello ügyfél hello árazás réteg panel az Azure portál hello toopurchase döntse el, és a lemezkép központi telepítését. **Ezek a javaslatok csak. hello ügyfél képes tooselect bármely hello lemezek üzenettovábbítást Virtuálisgép-méretet a lemezkép megadott.**
8. Adja meg a hello verziója. hello verziómezőjének magában foglalja a szemantikai verzió tooidentify hello terméket és a frissítések:
   * Verziók hello űrlap X.Y.Z, ha X, Y és Z nem egész számnak kell lennie.
   * Különböző Termékváltozatai a lemezképek különböző fő- és alverzió verziója is van.
   * Egy SKU belül verziók csak kell lennie a növekményes változásokat, ami növeli a hello javítás verzió (a X.Y.Z Z).
9. A hello **az operációs rendszer virtuális merevlemez URL-cím** mezőbe írja be a hello közös hozzáférésű jogosultságkódot hello virtuális merevlemez operációs rendszerhez létrehozott URI.
10. Ha a Termékváltozat tartozó adatlemeznek, válassza ki a hello logikai egység száma (LUN) toowhich azt szeretné, hogy az adatok lemezre toobe csatlakoztatva a telepítés után.
11. A hello **LUN X virtuális merevlemez URL-cím** hello közös hozzáférésű jogosultságkódot hello első adatok VHD URI létrehozott adja meg.

    ![rajz](media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus-3.png)


## <a name="common-sas-url-issues--fixes"></a>Közös SAS URL-címet állít ki, és kijavítja a

|Probléma|Hibaüzenet|Javítás|Dokumentáció-hivatkozás|
|---|---|---|---|
|Hiba történt a másolása lemezképek – "?" nem található az SAS URL-címe|Hiba: A képek másolása. Nem sikerült toodownload blob használatával megadott SAS URI-t.|Frissítés hello SAS URL-cím használata ajánlott eszközök|[https://Azure.microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-signature-Part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Hiba történt a Másolás lemezképek - "st" és "se" paraméterek nem az SAS URL-címe|Hiba: A képek másolása. Nem sikerült toodownload blob használatával megadott SAS URI-t.|A kezdő és befejező dátumok hello SAS URL-cím frissítése|[https://Azure.microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-signature-Part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Hiba történt a lemezképek - "sp = rl" nem SAS URL-címben másolása|Hiba: A képek másolása. Nem sikerült toodownload blob használatával megadott SAS URI-t|Frissítés hello SAS URL-cím "Read" & "lista beállítás engedélyekkel|[https://Azure.microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-signature-Part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Hiba történt a lemezképek - SAS URL-cím másolása szóközöket rendelkezik a virtuális merevlemez neve|Hiba: A képek másolása. Nem sikerült toodownload blob használatával megadott SAS URI-t.|Frissítés hello SAS URL-cím szóközök nélkül|[https://Azure.microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-signature-Part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Hiba történt a lemezképek – SAS URL-engedélyezési hiba másolása|Hiba: A képek másolása. Nem sikerült toodownload blob tooauthorization hiba miatt|Hello SAS URL-cím újbóli létrehozása|[https://Azure.microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-signature-Part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|


## <a name="next-step"></a>Következő lépés
Miután befejezte az hello SKU adatokkal, előre toohello áthelyezheti [Azure piactér marketing content útmutató][link-pushstaging]. A közzétételi folyamat hello lépésben megadta tartalom árképzési és egyéb információkat szükséges előzetes túl marketing hello**3. lépés: a virtuális gép tesztelési kínálnak a tesztelési**, ahol különböző használati eset forgatókönyvekben teszteléséhez üzembe helyezése előtt hello ajánlat toohello Azure piactér nyilvános látható és beszerzési.  

## <a name="see-also"></a>Lásd még:
* [Első lépések: hogyan toopublish egy ajánlat toohello Azure piactéren](marketplace-publishing-getting-started.md)

[img-acom-1]:media/marketplace-publishing-vm-image-creation/vm-image-acom-datacenter.png
[img-portal-vm-size]:media/marketplace-publishing-vm-image-creation/vm-image-portal-size.png
[img-portal-vm-create]:media/marketplace-publishing-vm-image-creation/vm-image-portal-create-vm.png
[img-portal-vm-location]:media/marketplace-publishing-vm-image-creation/vm-image-portal-location.png
[img-portal-vm-rdp]:media/marketplace-publishing-vm-image-creation/vm-image-portal-rdp.png
[img-azstg-add]:media/marketplace-publishing-vm-image-creation/vm-image-storage-add.png
[img-manage-vm-new]:media/marketplace-publishing-vm-image-creation/vm-image-manage-new.png
[img-manage-vm-select]:media/marketplace-publishing-vm-image-creation/vm-image-manage-select.png
[img-cert-vm-key-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-keyfile-linux.png
[img-cert-vm-pswd-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-password-linux.png
[img-cert-vm-pswd-win]:media/marketplace-publishing-vm-image-creation/vm-image-certification-password-win.png
[img-cert-vm-test-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-test-sample-linux.png
[img-cert-vm-test-win]:media/marketplace-publishing-vm-image-creation/vm-image-certification-test-sample-win.png
[img-cert-vm-results]:media/marketplace-publishing-vm-image-creation/vm-image-certification-results.png
[img-cert-vm-questionnaire]:media/marketplace-publishing-vm-image-creation/vm-image-certification-questionnaire.png
[img-cert-vm-questionnaire-2]:media/marketplace-publishing-vm-image-creation/vm-image-certification-questionnaire-2.png
[img-pubportal-vm-skus]:media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus.png
[img-pubportal-vm-skus-2]:media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus-2.png

[link-pushstaging]:marketplace-publishing-push-to-staging.md
[link-github-waagent]:https://github.com/Azure/WALinuxAgent
[link-azure-codeplex]:https://azurestorageexplorer.codeplex.com/
[link-azure-2]:../storage/blobs/storage-dotnet-shared-access-signature-part-2.md
[link-azure-1]:../storage/common/storage-dotnet-shared-access-signature-part-1.md
[link-msft-download]:http://www.microsoft.com/download/details.aspx?id=44299
[link-technet-3]:https://technet.microsoft.com/library/hh846766.aspx
[link-technet-2]:https://msdn.microsoft.com/library/dn495261.aspx
[link-azure-portal]:https://portal.azure.com
[link-pubportal]:https://publish.windowsazure.com
[link-sql-2014-ent]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014enterprisewindowsserver2012r2/
[link-sql-2014-std]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014standardwindowsserver2012r2/
[link-sql-2014-web]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014webwindowsserver2012r2/
[link-sql-2012-ent]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2enterprisewindowsserver2012/
[link-sql-2012-std]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2standardwindowsserver2012/
[link-sql-2012-web]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2webwindowsserver2012/
[link-datactr-2012-r2]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2012r2datacenter/
[link-datactr-2012]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2012datacenter/
[link-datactr-2008-r2]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2008r2sp1/
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-technet-1]:https://technet.microsoft.com/library/hh848454.aspx
[link-azure-vm-2]:./virtual-machines-linux-agent-user-guide/
[link-openssl]:https://www.openssl.org/
[link-intsvc]:http://www.microsoft.com/download/details.aspx?id=41554
[link-python]:https://www.python.org/
