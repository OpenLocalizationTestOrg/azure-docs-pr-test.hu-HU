---
title: "aaaPreparing a környezet tooback Azure virtuális gépek |} Microsoft Docs"
description: "Győződjön meg arról, hogy a környezet készen áll a testreszabásra az Azure virtuális gépek biztonsági mentéséről"
services: backup
documentationcenter: 
author: markgalioto
manager: carmonn
editor: 
keywords: "biztonsági mentések; biztonsági mentése;"
ms.assetid: 238ab93b-8acc-4262-81b7-ce930f76a662
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/25/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: 3b914c507dd6ad703ea799115ae84ac229e27787
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-your-environment-tooback-up-azure-virtual-machines"></a>A környezet tooback Azure virtuális gépek előkészítése
> [!div class="op_single_selector"]
> * [Erőforrás-kezelő modell](backup-azure-arm-vms-prepare.md)
> * [Klasszikus modell](backup-azure-vms-prepare.md)
>
>

Készíthet biztonsági mentést egy Azure virtuális gép (VM), mielőtt nincsenek három feltétel, amely már léteznie kell.

* A mentési tároló toocreate kell, vagy egy meglévő biztonsági mentési tárolót azonosító *hello a és a virtuális gép ugyanabban a régióban*.
* Az Azure nyilvános Internet-címek és az Azure storage végpontok hello hello közötti hálózati kapcsolatot létrehozni.
* Hello VM hello Virtuálisgép-ügynök telepítését.

Ha ismeri ezeket a feltételeket még vannak jelen a környezetében, majd a folytatáshoz toohello [készítsen biztonsági másolatot a virtuális gépek cikk](backup-azure-vms.md). Ellenkező esetben olvasni, ez a cikk végigvezeti hello lépéseket tooprepare a környezet tooback egy Azure virtuális gépet.

##<a name="supported-operating-system-for-backup"></a>Támogatott operációs rendszer biztonsági mentése
 * **Linux**: Az Azure Backup az [Azure által támogatott disztribúciókat](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) támogatja, a Core OS Linux kivételével. _Más kerüljön-a-saját-Linux terjesztéseket is előfordulhat, hogy működni, amíg hello Virtuálisgép-ügynök hello virtuális gépen érhető el, a Python létezik támogatása. Azonban azt hitelesíti ezeket terjesztéseket, a biztonsági mentéshez._
 * **Windows Server**: A Windows Server 2008 R2-nél régebbi verziók nem támogatottak.


## <a name="limitations-when-backing-up-and-restoring-a-vm"></a>Ha a biztonsági mentése és visszaállítása egy virtuális gép korlátozásai
> [!NOTE]
> Azure az erőforrások létrehozására és kezelésére két üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md). hello alábbiakban hello korlátozások hello klasszikus modellben való telepítésekor.
>
>

* Több mint 16 adatlemezekkel rendelkező virtuális gépek biztonsági mentését nem támogatott.
* A fenntartott IP-cím és a nem definiált végpontot a virtuális gépek biztonsági mentését nem támogatott.
* Biztonsági másolat adataiban nem tartalmazza a csatlakoztatott hálózati meghajtó csatlakoztatva tooVM.
* Egy meglévő virtuális gép cseréje a visszaállítás során nem támogatott. Először törölje a hello meglévő virtuális gépet és a kapcsolódó lemezzel, és ezután hello adatok visszaállítása biztonsági másolatból.
* Kereszt-régió biztonsági mentése és visszaállítása nem támogatott.
* Azure minden nyilvános régiókban támogatott virtuális gépek biztonsági mentését hello Azure Backup szolgáltatás segítségével (lásd: hello [ellenőrzőlista](https://azure.microsoft.com/regions/#services) a támogatott régiók). Ha a keresett hello régióban jelenleg nem támogatott, már nem jelenik hello legördülő lista tároló létrehozása során.
* Virtuális gépek biztonsági mentését hello Azure Backup szolgáltatás használatával csak select operációs rendszereken támogatott:
* A tartományvezérlők visszaállítását (DC) virtuális Gépet, amely része egy multi-tartományvezérlő-konfiguráció támogatott csak a PowerShell segítségével. Tudjon meg többet az [multi-DC tartományvezérlő visszaállítása](backup-azure-restore-vms.md#restoring-domain-controller-vms).
* Virtuális gépeken, amelyek a következő speciális hálózati konfigurációk hello visszaállítása csak a PowerShell használatával támogatott. Hello hello visszaállítási munkafolyamat használatával létrehozott virtuális gépek felhasználói felület nem lesz a hálózati konfiguráció hello visszaállítási művelet befejeződése után. több, lásd: toolearn [visszaállítását virtuális gépek speciális hálózati konfigurációkkal](backup-azure-restore-vms.md#restoring-vms-with-special-network-configurations).
  * Virtuális gépek a terheléselosztó-konfigurációja (belső és külső)
  * Virtuális gépek több foglalt IP-címmel
  * Virtuális gépek több hálózati adapterrel

## <a name="create-a-backup-vault-for-a-vm"></a>A virtuális gépek biztonsági mentési tároló létrehozása
A mentési tárolóban olyan entitás, amely hello biztonsági mentések és adott idő alatt létrehozott helyreállítási pontokat tárolja. hello mentési tároló hello biztonsági mentési házirendek, amelyek lesznek alkalmazott toohello virtuális gépek biztonsági mentését is tartalmaz.

> [!IMPORTANT]
> 2017. március-től kezdődően már nem használható hello klasszikus portál toocreate mentési tárolókban. Meglévő mentési tárolókban továbbra is támogatottak, és lehetséges túl[használja az Azure PowerShell toocreate mentési tárolókban](./backup-client-automation-classic.md#create-a-backup-vault). Azonban a Microsoft azt javasolja, akkor hozza létre az telepítésére vonatkozó Recovery Services-tárolók, mivel a jövőbeli fejlesztések tooRecovery szolgáltatások tárolók, csak alkalmazni.


A képen látható hello hello kapcsolatai különböző Azure biztonsági mentési entitásokat: ![Azure biztonsági mentési entitásokat és kapcsolatok](./media/backup-azure-vms-prepare/vault-policy-vm.png)



## <a name="network-connectivity"></a>Hálózati kapcsolat
A sorrend toomanage hello VM pillanatképek hello tartalék mellék kell kapcsolatot toohello Azure nyilvános IP-címeket. Hello jobb Internet kapcsolat nélkül hello virtuális gép HTTP kérelem túllépi az időkorlátot és hello biztonsági mentés sikertelen lesz. Ha a központi telepítés rendelkezik korlátozza a hozzáférést (a hálózati biztonsági csoport (NSG), például), majd válassza ki biztosítani azt egy tiszta elérési utat a biztonsági mentési forgalom ezen beállítások valamelyikét:

* [Engedélyezési lista hello Azure datacenter IP-címtartományok](http://www.microsoft.com/en-us/download/details.aspx?id=41653) -hello cikk útmutatásért tekintse meg a hogyan toowhitelist hello IP-címek.
* A forgalom útválasztási HTTP-proxy kiszolgáló telepítése.

Amikor arról dönt, hogy melyik lehetőség toouse, hello kompromisszumot kezelhetőség, a tanúsítványhasználat pontos szabályzása és költsége közötti vannak.

| Beállítás | Előnyei | Hátrányok |
| --- | --- | --- |
| Engedélyezett IP-címtartományok |Nincsenek további költségek.<br><br>A hozzáférés egy NSG, használja a hello <i>Set-AzureNetworkSecurityRule</i> parancsmag. |Összetett toomanage, érintett hello IP-címtartományok módosítása adott idő alatt.<br><br>Azure, és nem csak a tárolási toohello teljes hozzáférést biztosít. |
| HTTP-proxy |Részletes szabályozását a hello proxy hello tároló URL-címek engedélyezett. a tanúsítványhasználat pontos szabályzása toosetup hello proxy a https://\*.blob.core.windows.net/\* URL-minta kell toobe szerepel az engedélyezési listán. csak a hello hello virtuális gépek által használt tárfiók toowhitelist https://\<storageAccount\>.blob.core.windows.net/\* URL-minta kell toobe szerepel az engedélyezési listán. <br>Egyetlen pont, Internet-hozzáférés tooVMs.<br>Nem tulajdonos tooAzure IP-címet érintő módosításait. |További költségekkel egy virtuális Gépet futtató hello proxy szoftverrel. |

### <a name="whitelist-hello-azure-datacenter-ip-ranges"></a>Engedélyezési lista hello Azure datacenter IP-címtartományok
toowhitelist hello Azure datacenter IP-címtartományai, tekintse meg a hello [Azure-webhelyen](http://www.microsoft.com/en-us/download/details.aspx?id=41653) hello IP-címtartományok és utasításokat.

### <a name="using-an-http-proxy-for-vm-backups"></a>A virtuális gép biztonsági mentésekhez egy HTTP-proxy használatával
Virtuális gépek biztonsági mentéséről, hello tartalék mellék a virtuális gép hello küld hello pillanatkép felügyeleti parancsok tooAzure HTTPS API segítségével. Hello tartalék mellék forgalom hello HTTP-proxyn keresztül történő továbbításához, mert hello csak összetevő hozzáférés toohello konfigurált nyilvános internethez.

> [!NOTE]
> Nincs Nincs javaslat hello proxy szoftverek kell használni. Győződjön meg arról, hogy a proxy, amely rendelkezik a kimenő tölcsérútvonalak választja, és amely kompatibilis a hello alábbi konfigurációs lépéseket. Győződjön meg arról, hogy a harmadik féltől származó szoftverekkel nem módosítják a hello proxybeállítások
>
>

az alábbi példa képen hello szükséges toouse HTTP proxy hello három konfigurációs lépéseit mutatja be:

* Alkalmazás VM útvonalak kapcsolódik az összes HTTP-forgalom a nyilvános interneten keresztül Proxy VM hello.
* Proxy VM lehetővé teszi bejövő forgalmat a virtuális gépekről érkező hello virtuális hálózat.
* Hálózati biztonsági csoport (NSG) nevű Elégtelen-zárolási hello kell egy biztonsági szabály engedélyezése kimenő internetforgalom Proxy virtuális gépről.

![NSG a HTTP-proxy telepítési diagram](./media/backup-azure-vms-prepare/nsg-with-http-proxy.png)

toouse HTTP proxy toocommunicating toohello nyilvános Internet, kövesse az alábbi lépéseket:

#### <a name="step-1-configure-outgoing-network-connections"></a>1. lépés A kimenő hálózati kapcsolatok konfigurálása
###### <a name="for-windows-machines"></a>A Windows-alapú gépek
Ez a helyi rendszerfiókhoz proxykiszolgálót állítják be.

1. Töltse le [PsExec](https://technet.microsoft.com/sysinternals/bb897553)
2. Futtassa a következő parancs rendszergazda jogú parancssorból

     ```
     psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"
     ```
     Megnyílik az internet explorer ablakot.
3. Nyissa meg tooTools Internet-beállítások -> -> kapcsolatok LAN-beállítások ->.
4. Ellenőrizze a proxybeállítások helyességét a rendszerfiók. Állítsa be a Proxy IP-cím és port.
5. Zárja be az Internet Explorert.

Beállításához nyújt útmutatást a gépre kiterjedő proxy konfigurációját, és egyetlen kimenő HTTP/HTTPS-forgalmat fog történni.

Ha a telepítő egy proxykiszolgáló a jelenlegi felhasználói fiók (nem a helyi rendszerfiók), használja hello parancsfájl tooapply következő őket tooSYSTEMACCOUNT:

```
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver
```

> [!NOTE]
> Ha "(407) Proxyhitelesítés szükséges" proxy server naplóban, ellenőrizze a hitelesítési helyesen beállítva.
>
>

###### <a name="for-linux-machines"></a>Linux-gépekhez
Adja hozzá a következő sor toohello hello ```/etc/environment``` fájlt:

```
http_proxy=http://<proxy IP>:<proxy port>
```

Adja hozzá a következő sorokat toohello hello ```/etc/waagent.conf``` fájlt:

```
HttpProxy.Host=<proxy IP>
HttpProxy.Port=<proxy port>
```

#### <a name="step-2-allow-incoming-connections-on-hello-proxy-server"></a>2. lépés Bejövő kapcsolatok engedélyezése hello proxykiszolgáló:
1. Nyissa meg a Windows tűzfal hello proxy kiszolgálón. hello legegyszerűbb módja tooaccess hello tűzfal toosearch a fokozott biztonságú Windows tűzfal.

    ![Nyissa meg a hello tűzfal](./media/backup-azure-vms-prepare/firewall-01.png)
2. Hello Windows tűzfal párbeszédpanelen kattintson a jobb gombbal **bejövő szabályok** kattintson **új szabály létrehozása...** .

    ![Új szabály létrehozása](./media/backup-azure-vms-prepare/firewall-02.png)
3. A hello **új bejövő szabály varázsló**, válassza ki a hello **egyéni** hello beállítása **szabály típusa** kattintson **következő**.
4. A hello lap tooselect hello **Program**, válassza a **minden program** kattintson **következő**.
5. A hello **protokoll és portok** lapon adja meg a következő információ hello, majd kattintson **következő**:

    ![Új szabály létrehozása](./media/backup-azure-vms-prepare/firewall-03.png)

   * a *protokolltípus* válasszon *TCP*
   * a *helyi port* válasszon *adott*, az alábbi hello mezőben adja meg a hello ```<Proxy Port>``` be van állítva.
   * a *távoli port* válasszon *minden port*

     Hello többi hello varázsló kattintson az összes hello módon toohello végfelhasználói, és nevezze el ez a szabály.

#### <a name="step-3-add-an-exception-rule-toohello-nsg"></a>3. lépés Egy kivétel szabály toohello NSG hozzáadása:
Az Azure PowerShell-parancssort írja be a hello a következő parancsot:

a következő parancs hello ad hozzá egy kivétel toohello NSG. Ez a kivétel lehetővé teszi, hogy a TCP-forgalom 10.0.0.5 a bármely portról tooany internetcím a 80-as (HTTP) vagy a 443-as (HTTPS) porton. Ha szüksége van egy adott portot a hello nyilvános Internet kell arról, hogy a port toohello tooadd ```-DestinationPortRange``` is.

```
Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
```

*Győződjön meg arról, hello nevek hello példában cserélje hello részletek megfelelő tooyour központi telepítés.*

## <a name="vm-agent"></a>Virtuálisgép-ügynök
Mielőtt készíthet biztonsági mentést hello Azure virtuális gép, biztosítania kell, hogy hello Azure Virtuálisgép-ügynök megfelelően telepítve van a hello virtuális gépen. Mivel a hello Virtuálisgép-ügynök, amely a virtuális gép hello hello időpontban opcionális összetevője jön létre, győződjön meg arról, hogy hello jelölőnégyzetet hello Virtuálisgép-ügynök ki van választva, hello virtuális gép kiépítése előtt.

### <a name="manual-installation-and-update"></a>Manuális telepítés és frissítés
Virtuálisgép-ügynök hello már létezik az Azure katalógusában hello a létrehozott virtuális gépeken. A helyszíni adatközpontját a áttelepített virtuális gépek azonban nem áll hello Virtuálisgép-ügynök telepítve. Ilyen virtuális gépek hello Virtuálisgép-ügynök telepítve explicit módon toobe kell. Tudjon meg többet az [hello VM ügynök telepítése egy meglévő virtuális gép](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

| **Művelet** | **Windows** | **Linux** |
| --- | --- | --- |
| Hello Virtuálisgép-ügynök telepítése |<li>Töltse le és telepítse a hello [ügynök MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Rendszergazdai jogosultságok toocomplete hello telepítési kell. <li>[Hello VM tulajdonság](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) tooindicate, amely hello ügynök telepítve van. |<li> Telepítse legújabb hello [Linux-ügynök](https://github.com/Azure/WALinuxAgent) a Githubról. Rendszergazdai jogosultságok toocomplete hello telepítési kell. <li> [Hello VM tulajdonság](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) tooindicate, amely hello ügynök telepítve van. |
| Hello Virtuálisgép-ügynök frissítése |Frissítési hello Virtuálisgép-ügynök újratelepítését hello egyszerűen [virtuális gép ügynök bináris fájljai](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). <br><br>Győződjön meg arról, hogy fut-e biztonsági mentési művelet, hello Virtuálisgép-ügynök frissítése közben. |Hello utasításokat kövesse a megjelenő [hello Linux Virtuálisgép-ügynök frissítése ](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). <br><br>Győződjön meg arról, hogy fut-e biztonsági mentési művelet, hello Virtuálisgép-ügynök frissítése közben. |
| Hello Virtuálisgép-ügynök telepítésének ellenőrzése |<li>Keresse meg a toohello *C:\WindowsAzure\Packages* hello Azure virtuális gép mappájában. <li>Keresse meg hello WaAppAgent.exe fájl található.<li> Kattintson a jobb gombbal a hello fájlt, nyissa meg túl**tulajdonságok**, majd válassza ki a hello **részletek** külön-külön hello termékverzió mező lehet 2.6.1198.718 vagy újabb. |N/A |

További tudnivalók: hello [Virtuálisgép-ügynök](https://go.microsoft.com/fwLink/?LinkID=390493&clcid=0x409) és [hogyan tooinstall azt](https://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/).

### <a name="backup-extension"></a>Backup bővítmény
virtuális gép hello tooback, hello Azure Backup szolgáltatás bővítmény toohello VM ügynök telepítése. hello Azure Backup szolgáltatás zökkenőmentesen frissíti, és javításokkal hello tartalék mellék további felhasználói beavatkozás nélkül.

tartalék mellék hello hello virtuális gép fut. Ha telepítve van. A virtuális gép hello legnagyobb esély arra az alkalmazáskonzisztens helyreállítási pontot is biztosít. Azonban hello Azure Backup szolgáltatás folytatja a VM – hello mentése tooback, még akkor is, ha ki van kapcsolva, és hello bővítményt nem sikerült telepíteni (más néven Offline virtuális gép). Ebben az esetben hello helyreállítási pont lesz *összeomlás-konzisztens* kapcsolatban a fentiekben ismertetett módon.

## <a name="questions"></a>Kérdései vannak?
Ha kérdése van, vagy ha bármely új szolgáltatása része, toosee szeretné [visszajelzést küldhet](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Következő lépések
Most, hogy előkészítette a környezetet az biztonsági mentése a virtuális Gépet, a következő logikai lépésre toocreate biztonsági másolat. a cikk tervezési hello biztosít a virtuális gépek biztonsági mentéséről további részletes információk.

* [Készítsen biztonsági másolatot a virtuális gépek](backup-azure-vms.md)
* [A virtuális gép biztonsági mentési infrastruktúra megtervezése](backup-azure-vms-introduction.md)
* [Virtuális gépek biztonsági mentéseinek kezelése](backup-azure-manage-vms.md)
