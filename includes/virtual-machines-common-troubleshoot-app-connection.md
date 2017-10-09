Nincsenek különböző okokból nem kezdődhet vagy csatlakoztassa egy Azure virtuális gép (VM) tooan alkalmazást. Okok közé tartoznak a hello alkalmazás nem fut, vagy a várt hello porton figyel hello blokkolja, vagy nem megfelelően áthaladó forgalom toohello alkalmazás szabályok hálózati figyelő portja. Ez a cikk ismerteti a módszeres megközelítés toofind és a megfelelő hello probléma.

Ha a Kapcsolódás a virtuális gép tooyour problémák RDP vagy SSH, lásd: hello következő először cikkek:

* [Távoli asztali kapcsolatok tooa Windows-alapú Azure virtuális gépek hibaelhárítása](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md)
* [Secure Shell (SSH) kapcsolatok tooa Linux-alapú Azure virtuális gép hibakeresése](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md).

> [!NOTE]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../articles/resource-manager-deployment-model.md). Ez a cikk mindkét modell használatát bemutatja, de a Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure-szakértők hello [MSDN Azure hello és fórumok Stack Overflow hello](https://azure.microsoft.com/support/forums/). Másik lehetőségként is fájl is az Azure támogatási incidens. Nyissa meg toohello [az Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) válassza ki **támogatja az beszerzése**.

## <a name="quick-start-troubleshooting-steps"></a>Gyors üzembe helyezési hibaelhárítási lépések
Ha problémába ütközik tooan alkalmazás, próbálja hello általános hibaelhárítási lépéseket. Minden lépés után próbálja meg újból a csatlakozás tooyour alkalmazást:

* Hello virtuális gép újraindítása
* Hozza létre újra a hello végpont / tűzfal-szabályokat / hálózati biztonsági csoport (NSG) szabályok
  * [Erőforrás-kezelő modell - hálózati biztonsági csoportok kezelése](../articles/virtual-network/virtual-networks-create-nsg-arm-pportal.md)
  * [Klasszikus modell - Felhőszolgáltatások kezelése végpontok](../articles/cloud-services/cloud-services-enable-communication-role-instances.md)
* Csatlakozás másik helyről, például különböző Azure virtuális hálózathoz
* Hello virtuális gép újbóli üzembe helyezése
  * [Telepítse újra a Windows virtuális gép](../articles/virtual-machines/windows/redeploy-to-new-node.md)
  * [Telepítse újra a Linux virtuális gép](../articles/virtual-machines/linux/redeploy-to-new-node.md)
* Hozza létre újra a virtuális gép hello

További információkért lásd: [hibaelhárítási végpont kapcsolat (RDP/SSH/HTTP, hiba stb.)](https://social.msdn.microsoft.com/Forums/azure/en-US/538a8f18-7c1f-4d6e-b81c-70c00e25c93d/troubleshooting-endpoint-connectivity-rdpsshhttp-etc-failures?forum=WAVirtualMachinesforWindows).

## <a name="detailed-troubleshooting-overview"></a>Részletes hibaelhárítási áttekintése
Nincsenek négy fő területet tootroubleshoot hello hozzáférés az Azure virtuális gépként futó alkalmazások.

![hibaelhárítás alkalmazás nem indítható el.](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access1.png)

1. hello a hello Azure virtuális gépen futó alkalmazás.
   * Maga hello alkalmazás megfelelően fut?
2. hello Azure virtuális géphez.
   * Hello virtuális gépért megfelelően működik és válaszol toorequests?
3. Azure-hálózat végpontok.
   * A felhő virtuális gépek hello klasszikus üzembe helyezési modellel végpontok.
   * Hálózati biztonsági csoportok és a bejövő NAT-szabályok virtuális gépek erőforrás-kezelő üzembe helyezési modellben.
   * A felhasználók toohello VM/alkalmazás a várt hello portokon folyamata is forgalom?
4. Az Internet peremhálózati eszköz.
   * Tűzfalszabályok helyen akadályozzák a forgalom továbbítására megfelelően?

Az alkalmazás hello pont-pont származó VPN- vagy ExpressRoute-kapcsolaton keresztül elérő ügyfélszámítógépeken hello fő területet problémákat okozhat, hello alkalmazás és hello Azure virtuális gép.

toodetermine hello forrás hello probléma és a hibajavítási, kövesse az alábbi lépéseket.

## <a name="step-1-access-application-from-target-vm"></a>1. lépés: A cél virtuális gép alkalmazás elérése
Próbálja meg a futtató hello virtuális hello megfelelő ügyfélprogram tooaccess hello alkalmazást. Hello helyi állomásnév, hello helyi IP-címmel, vagy hello visszacsatolási (127.0.0.1) használja.

![Indítsa el az alkalmazás közvetlenül a virtuális gép hello](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access2.png)

Például webkiszolgáló hello alkalmazás esetén nyisson meg egy böngészőt, a virtuális gép hello, és próbálja tooaccess hello VM üzemeltetett weblapon.

Hello alkalmazást érheti el, ha túl Ugrás[2. lépés](#step2).

Ha hello alkalmazás nem fér hozzá, ellenőrizze a hello a következő beállításokat:

* hello alkalmazás hello cél virtuális gépen fut.
* hello alkalmazás várt hello TCP és UDP-porton figyel.

A Windows és Linux-alapú virtuális gépek használata a hello **netstat - a** parancs tooshow hello aktív figyelő portot. Vizsgálja meg a várt hello portokat, amelyen az alkalmazás figyelésére kell hello kimenetét. Hello alkalmazás újraindítása, vagy konfigurálja úgy toouse várt hello portok igény szerint, és próbálkozzon újra a tooaccess hello alkalmazás helyi.

## <a id="step2"></a>2. lépés: A hello egy másik virtuális alkalmazás elérése ugyanabban a virtuális hálózatban
Próbálja tooaccess hello alkalmazás egy másik virtuális gépről, de a hello azonos virtuális hálózatra, hello virtuális gép állomásnevét vagy az Azure által hozzárendelt public, private vagy szolgáltatói IP-címét. A virtuális gépek hello klasszikus telepítési modellel készült ne használjon hello felhőalapú szolgáltatás hello nyilvános IP-címét.

![Indítsa el az alkalmazás egy másik virtuális gépről](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access3.png)

Például ha hello alkalmazás webkiszolgálóra, próbálja meg tooaccess egy weblapot egy böngészőből eltérő virtuális gépet a hello azonos virtuális hálózatban.

Hello alkalmazást érheti el, ha túl Ugrás[3. lépés](#step3).

Ha hello alkalmazás nem fér hozzá, ellenőrizze a hello a következő beállításokat:

* hello cél virtuális gép hello állomás tűzfal átengedi a hello bejövő kérelem és válasz kimenő forgalmat.
* Behatolásérzékelési vagy hálózatfigyelési hello cél virtuális gép futó szoftver keresztülhaladó forgalmat hello.
* Cloud Services végpontjainak vagy a hálózati biztonsági csoportok hello forgalmat engedélyezi:
  * [Klasszikus modell - Felhőszolgáltatások kezelése végpontok](../articles/cloud-services/cloud-services-enable-communication-role-instances.md)
  * [Erőforrás-kezelő modell - hálózati biztonsági csoportok kezelése](../articles/virtual-network/virtual-networks-create-nsg-arm-pportal.md)
* A virtuális gép és a virtuális Gépet, például terheléselosztó vagy tűzfal, hello teszt közötti hello elérési úton a virtuális gépen futó külön összetevő keresztülhaladó forgalmat hello.

A Windows-alapú virtuális gépen a Windows tűzfal a speciális biztonsági toodetermine egyaránt használhatja hello tűzfalszabályokat az alkalmazás bejövő és kimenő forgalom kizárása.

## <a id="step3"></a>3. lépés: A külső hello virtuális hálózatról alkalmazás elérése
Próbálja meg tooaccess hello alkalmazás hello virtuális hálózaton kívüli gépről a hello alkalmazást futtató hello virtuális gépként. Egy másik hálózati használják az eredeti ügyfélszámítógépen.

![Indítsa el az alkalmazás hello virtuális hálózaton kívüli gépről](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access4.png)

Például ha már a webkiszolgáló hello alkalmazás, próbálja tooaccess hello weblapot egy böngészőből, amely nincs hello virtuális hálózatban lévő számítógépeken futó.

Ha hello alkalmazás nem fér hozzá, ellenőrizze a hello a következő beállításokat:

* Virtuális gépek hello klasszikus telepítési modellel készült:
  
  * Győződjön meg arról, hogy virtuális gép átengedi a hello bejövő forgalmat, különösen a hello protocol (TCP és UDP) és a hello nyilvános és titkos portszámok hello hello végpont-konfiguráció.
  * Ellenőrizze, hogy hello végponton hozzáférés-vezérlési listák (ACL) nem akadályozzák hello Internet érkező bejövő forgalmat.
  * További információkért lásd: [hogyan tooSet be végpontok tooa virtuális gép](../articles/virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
* Virtuális gépek hello erőforrás-kezelő telepítési modellel készült:
  
  * Győződjön meg arról, amely hello VM átengedi a hello bejövő forgalmat, különösen a hello protocol (TCP és UDP) és a hello nyilvános és titkos portszámok hello bejövő NAT szabály konfigurálását.
  * Győződjön meg arról, hogy hálózati biztonsági csoportok bejövő hello kérelem és válasz kimenő forgalmat engedélyezi.
  * További információ: [What is a Network Security Group (NSG)?](../articles/virtual-network/virtual-networks-nsg.md) (Mi az a hálózati biztonsági csoport?).

Ha hello virtuális gép vagy a végpont egy elosztott terhelésű készlet tagja:

* Győződjön meg arról, hogy hello mintavételi protocol (TCP és UDP) és a portszám helyességéről.
* Ha a mintavételi hello protokoll és port eltér hello elosztott terhelésű készlet protokoll és port:
  * Győződjön meg arról, hogy hello alkalmazás figyel a következőn: hello mintavételi protocol (TCP és UDP) és a portszám (használata **netstat – a** hello a cél virtuális gép).
  * Győződjön meg arról, hogy a hello állomás tűzfal átengedi a virtuális gép hello bejövő mintavételi kérelem és a kimenő mintavételi válasz forgalom hello célponton.

Ha hello alkalmazást érheti el, győződjön meg arról, hogy az Internet peremhálózati eszközön keresztülhaladó:

* hello kimenő alkalmazás kérelem forgalmát az ügyfél számítógép toohello Azure virtuális géphez.
* hello hello Azure virtuális gép bejövő kérelem-válasz forgalom.

## <a name="step-4-if-you-cannot-access-hello-application-use-ip-verify-toocheck-hello-settings"></a>4. lépés: Ha hello alkalmazás, IP-ellenőrzés toocheck hello-beállítások használata nem érhető el. 

További információkért lásd: [áttekintése Azure hálózatfigyelési](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-monitoring-overview). 

## <a name="additional-resources"></a>További források
[Távoli asztali kapcsolatok tooa Windows-alapú Azure virtuális gépek hibaelhárítása](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md)

[Secure Shell (SSH) kapcsolatok tooa Linux-alapú Azure virtuális gép hibakeresése](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md)

