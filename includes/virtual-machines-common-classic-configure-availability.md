


Rendelkezésre állási készlet nyújt segítséget, mint tartani a virtuális gépek állásidő, során rendelkezésre álló karbantartás során. Hello szükséges redundancia toomaintain rendelkezésre állási hello az alkalmazások és szolgáltatások a virtuális gépet futtató két vagy több hasonló módon konfigurált virtuális gépek elhelyezése egy rendelkezésre állási csoportot hoz létre. Ez működésével kapcsolatos részletekért lásd: [hello virtuális gépek rendelkezésre állásának kezelése][Manage hello availability of virtual machines].

A bevált gyakorlat toouse rendelkezésre állási készletek és a terheléselosztás végpontok toohelp gondoskodjon arról, hogy az alkalmazás mindig elérhető, és fut. hatékony. További információk az elosztott terhelésű végpont: [Azure infrastruktúra-szolgáltatásokat a terheléselosztás][Load balancing for Azure infrastructure services].

Klasszikus virtuális gépeket adhat hozzá a rendelkezésre állási készlet két lehetőség egyikét használva:

* [1. lehetőség: Hozzon létre egy virtuális gépet, és a rendelkezésre állási készlet: hello azonos idő][Option 1: Create a virtual machine and an availability set at hello same time]. Adja hozzá az új virtuális gépek toohello azon virtuális gépek létrehozásakor állítsa be.
* [2. lehetőség: Hozzáadása egy meglévő virtuális gép tooan rendelkezésre állási csoportot][Option 2: Add an existing virtual machine tooan availability set].

> [!NOTE]
> Hello a klasszikus modellben tooput kívánt virtuális gépek hello azonos rendelkezésre állási csoportba kell tartoznia toohello ugyanaz a felhőalapú szolgáltatás.
> 
> 

## <a id="createset"></a>1. lehetőség: hozzon létre egy virtuális gépet, és a rendelkezésre állási készlet: hello azonos idő
Vagy hello Azure-portálon is használhatja, vagy az Azure PowerShell-parancsok toodo ez.

toouse hello Azure-portálon:

1. Ha még nem tette meg, jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello központ menüben kattintson a **+ új**, és kattintson a **virtuális gép**.
   
    ![Kép helyettesítő szövege](./media/virtual-machines-common-classic-configure-availability/ChooseVMImage.png)
3. Válassza ki a hello piactér virtuálisgép-lemezkép toouse kívánja. Választhat toocreate egy Linux vagy a Windows virtuális gép.
4. A hello kiválasztott virtuális gépet, győződjön meg arról, hogy hello telepítési modell értéke túl**klasszikus** majd **létrehozása**
   
    ![Kép helyettesítő szövege](./media/virtual-machines-common-classic-configure-availability/ChooseClassicModel.png)
5. Adja meg a virtuális gép nevét, felhasználónevet és jelszót (a Windows gépek) vagy (a Linux-gépek) nyilvános SSH-kulcsot. 
6. Válassza ki a hello Virtuálisgép-méretet, és kattintson a **válasszon** toocontinue.
7. Válasszon **opcionális konfigurációs > rendelkezésre állási csoport**, és válassza ki a hello rendelkezésre állási csoport tooadd hello a virtuális gépet kívánja.
   
    ![Kép helyettesítő szövege](./media/virtual-machines-common-classic-configure-availability/ChooseAvailabilitySet.png) 
8. Tekintse át a konfigurációs beállításokat. Amikor elkészült, kattintson a **létrehozása**.
9. Amíg az Azure létrehozza a virtuális gép, előrehaladásának hello alatt **virtuális gépek** hello központ menüben.

toouse Azure PowerShell-parancsok toocreate Azure virtuális gép és tooa új vagy meglévő rendelkezésre állási csoportot adja hozzá, lásd: [használata Azure PowerShell toocreate és előre konfigurálása a Windows-alapú virtuális gépek](../articles/virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a id="addmachine"></a>2. lehetőség: egy meglévő virtuális gép tooan rendelkezésre állási csoport hozzáadása
Hello Azure-portálon, a meglévő klasszikus virtuális gépek tooan meglévő rendelkezésre állási csoport hozzáadása, vagy hozzon létre egy újat a számukra. (Ne feledje, hogy a virtuális gépek hello azonos rendelkezésre állási csoport hello kell tartozniuk toohello ugyanaz a felhőalapú szolgáltatás.) hello lépéseket majdnem azonos hello szövegrészt. Az Azure PowerShell hello virtuális gép tooan meglévő rendelkezésre állási csoport is hozzáadhat.

1. Ha még nem tette meg, jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello központ menüben kattintson a **virtuális gépek (klasszikus)**.
   
    ![Kép helyettesítő szövege](./media/virtual-machines-common-classic-configure-availability/ChooseClassicVM.png)
3. A virtuális gépek hello listáról válassza ki a hello virtuális gép, amelyet az tooadd toohello set hello nevét.
4. Válasszon **rendelkezésre állási csoport** hello virtuális gépről **beállítások**.
   
    ![Kép helyettesítő szövege](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetSettings.png)
5. Válassza ki a hello rendelkezésre állási csoport tooadd hello a virtuális gépet kívánja. hello virtuális gép kell tartozniuk toohello ugyanaz a felhőalapú szolgáltatás, hello rendelkezésre állási csoportot.
   
    ![Kép helyettesítő szövege](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetPicker.png)
6. Kattintson a **Save** (Mentés) gombra.

toouse Azure PowerShell-parancsok, nyisson meg egy rendszergazdai Azure PowerShell-munkamenetet, és futtassa a következő parancs hello. A helyőrzők hello (például &lt;VmCloudServiceName&gt;), cserélje le a mindent, ami hello idézőjelek között, beleértve a hello < és > karakter, hello megfelelő neveket.

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

> [!NOTE]
> hello virtuális gép újraindítása toobe toofinish toohello rendelkezésre állási csoport hozzáadásával rendelkezhet.
> 
> 

<!-- LINKS -->
[Option 1: Create a virtual machine and an availability set at hello same time]: #createset
[Option 2: Add an existing virtual machine tooan availability set]: #addmachine

[Load balancing for Azure infrastructure services]: ../articles/virtual-machines/virtual-machines-linux-load-balance.md
[Manage hello availability of virtual machines]:../articles/virtual-machines/linux/manage-availability.md

[Create a virtual machine running Windows]: ../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md
[Virtual Network overview]: ../articles/virtual-network/virtual-networks-overview.md

