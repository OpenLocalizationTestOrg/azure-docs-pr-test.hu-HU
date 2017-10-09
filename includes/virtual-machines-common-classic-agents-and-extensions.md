

A virtuálisgép-bővítmények a következőkben lehetnek a segítségére:

* Biztonsági és identitásszolgáltatások (például a fiókértékek visszaállítása vagy a kártevőirtó szoftverek használata) módosítása
* Figyelés és diagnosztika indítása, leállítása és konfigurálása
* Kapcsolati szolgáltatások (például az RDP vagy az SSH) visszaállítása vagy telepítése
* A virtuális gépek diagnosztizálása, megfigyelése és felügyelete

Számos egyéb szolgáltatás is elérhető, és rendszeresen jelennek meg új, virtuálisgép-bővítményekkel kapcsolatos szolgáltatások. Ez a cikk hello Azure VM ügynökök a Windows és Linux, és azok hogyan támogatják a Virtuálisgép-bővítmény funkcióit ismerteti. A virtuálisgép-bővítmények szolgáltatási kategóriák szerint rendezett listájáért tekintse át az [Azure virtuálisgép-bővítményekkel és szolgáltatásaikkal](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) foglalkozó témakört.

## <a name="azure-vm-agents-for-windows-and-linux"></a>Windows és Linux rendszerhez való Azure virtuálisgép-ügynökök
hello Azure virtuális gépek ügynök (Virtuálisgép-ügynök) egy védett, a passzív folyamat, amely telepíti, konfigurálja, és eltávolítja a Virtuálisgép-bővítmények Azure virtuális gépek példányokra. Virtuálisgép-ügynök hello hello biztonságos helyi vezérlési szolgáltatásban az Azure virtuális gép funkcionál. ügynök terhelések hello hello kiterjesztések funkciók tooincrease hello példánnyal termelékenységét adja meg.

Kétféle Azure virtuálisgép-ügynök létezik, egy a Windows, egy pedig a Linux rendszerű virtuális gépek számára.

Ha egy virtuális gép példány toouse egy vagy több Virtuálisgép-bővítmények, hello példány rendelkeznie kell egy telepített Virtuálisgép-ügynök. Hello Azure-portálon és hello a lemezkép használatával létrehozott virtuálisgép-lemezkép **piactér** automatikusan telepíti a Virtuálisgép-ügynök hello létrehozási folyamata. Ha virtuálisgép-példányt egy Virtuálisgép-ügynök nem rendelkezik, a Virtuálisgép-ügynök hello hello virtuálisgép-példány létrehozása után telepítheti. Vagy egy egyéni Virtuálisgép-lemezkép, majd töltse fel hello ügynök telepíthető.

> [!IMPORTANT]
> Ezek a virtuálisgép-ügynökök rendkívül leegyszerűsített szolgáltatások, amelyek lehetőséget adnak a virtuálisgép-példányok biztonságos felügyeletére. Előfordulhatnak olyan esetek, amikor nem szeretné, akkor hello Virtuálisgép-ügynök. Ha igen, lehet, hogy toocreate virtuális gépek, amelyeken nincs hello Virtuálisgép-ügynök telepítve hello Azure CLI vagy a PowerShell használatával. Bár a Virtuálisgép-ügynök hello fizikailag lehet eltávolítani, hello Virtuálisgép-bővítmények hello példányon működése nem definiált. Ennek következtében a telepített virtuálisgép-ügynökök eltávolítása nem támogatott.
>

Virtuálisgép-ügynök hello engedélyezve van a következő helyzetekben hello:

* A virtuális gépek példányának használatával létrehozásakor hello Azure-portál és a lemezkép kijelölésével hello **piactér**,
* Ha hoz létre egy virtuális gép példányának hello [New-AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx) vagy hello [New-AzureQuickVM](https://msdn.microsoft.com/library/azure/dn495183.aspx) parancsmag. Létrehozhat egy virtuális Gépet egy Virtuálisgép-ügynök nélkül hello hozzáadásával **– DisableGuestAgent** paraméter toohello [Add-AzureProvisioningConfig](https://msdn.microsoft.com/library/azure/dn495299.aspx) parancsmag,

* Amikor kézzel töltse le és hello Virtuálisgép-ügynök telepítése egy meglévő Virtuálisgép-példány, és állítsa be a hello **ProvisionGuestAgent** érték túl**igaz**. Ez a módszer Windows- és Linux-ügynökök esetében alkalmazható egy PowerShell-parancs vagy REST-hívás használatával. (Ha nincs megadva hello **ProvisionGuestAgent** érték hello Virtuálisgép-ügynök, a Virtuálisgép-ügynök nem található megfelelő hello hello hozzáadása manuális telepítése után.) hogyan kód példa azt mutatja meg a következő hello toodo a PowerShell használatával, ahol hello `$svc` és `$name` argumentumok rendszer már meghatározta:

      $vm = Get-AzureVM –ServiceName $svc –Name $name
      $vm.VM.ProvisionGuestAgent = $TRUE
      Update-AzureVM –Name $name –VM $vm.VM –ServiceName $svc

* Ha egy olyan virtuálisgép-rendszerképet hoz létre, amely tartalmaz egy telepített virtuálisgép-ügynököt. Virtuálisgép-ügynök hello hello lemezkép létezik-e, ha a kép tooAzure feltöltheti. A Windows virtuális gépek, töltse le a hello [Windows Virtuálisgép-ügynök .msi fájlt](http://go.microsoft.com/fwlink/?LinkID=394789) és hello Virtuálisgép-ügynök telepítése. A Linux virtuális Gépet, telepítse a Virtuálisgép-ügynök a hello GitHub-tárházban található hello <https://github.com/Azure/WALinuxAgent>. Hogyan tooinstall hello Linux Virtuálisgép-ügynök további információkért lásd: hello [Azure Linux virtuális gép ügynök felhasználói útmutató](../articles/virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

> [!NOTE]
> A PaaS, hello Virtuálisgép-ügynök nevezik **WindowsAzureGuestAgent**, és mindig elérhető a webes és feldolgozói szerepkör típusú virtuális gépek. (További információkért lásd: [Azure szerepkör architektúra](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx).) hello szerepkör típusú virtuális gépek Virtuálisgép-ügynök mostantól hozzáadhatja azok hello a bővítmények toohello felhőalapú szolgáltatás virtuális gépek azonos ugyanúgy állandó virtuális gépekhez. hello legnagyobb Virtuálisgép-bővítményt a virtuális gépek szerepkör és állandó virtuális gépek közötti különbség hello Virtuálisgép-bővítmények hozzáadásakor. Virtuális gépek szerepkör bővítmények első toohello felhőalapú szolgáltatás, majd toohello telepítések belül, amely a felhőszolgáltatás lettek hozzáadva.
>
> Használja a [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) parancsmag toolist összes szerepkör Virtuálisgép-bővítmények.
>
>

## <a name="find-add-update-and-remove-vm-extensions"></a>Virtuálisgép-bővítmények keresése, hozzáadása, frissítése és eltávolítása
Az ezekkel a feladatokkal kapcsolatos részletekért lásd: [Azure virtuálisgép-bővítmények hozzáadása, keresése, frissítése és eltávolítása](../articles/virtual-machines/windows/classic/manage-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
