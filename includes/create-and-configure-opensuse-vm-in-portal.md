1. Jelentkezzen be toohello [a klasszikus Azure portálon](http://manage.windowsazure.com).  
2. A parancssávon hello hello ablak hello alján, kattintson a **új**.
3. A **számítási**, kattintson a **virtuális gép**, és kattintson a **a gyűjtemény**.
   
    ![Új virtuális gép létrehozása][Image1]
4. A hello **SUSE** csoportban, OpenSUSE virtuális gép lemezképének kiválasztása, és kattintson a nyílra toocontinue hello.
5. A hello első **virtuálisgép-konfiguráció** lap:
   
   * Adjon meg egy **virtuális gép neve**, például a "testlinuxvm". hello neve kell csak 3 és 15 karakter közötti lehet, is tartalmazhat csak betűket, számokat és kötőjeleket tartalmazhat, és kell betűvel kezdődhet, és betűvel vagy számmal végződhet.
   * Ellenőrizze a hello **réteg** , és válasszon egy **mérete**. hello réteg választhat hello méretét határozza meg. hello hello költségeinek, és a konfigurációs beállítások például hogy hány adatlemezek csatolhat mérete érinti. További információkért lásd: [virtuális gépek méretei](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
   * Adjon meg egy **új felhasználónevet**, vagy fogadja el a hello alapértelmezés szerint **azureuser**. Ez a név jelenik meg toohello Sudoers fájlt.
   * Döntse el, hogy milyen típusú **hitelesítési** toouse. Általános jelszó útmutatást lásd: [erős jelszavak](http://msdn.microsoft.com/library/ms161962.aspx).
6. Hello a következő **virtuálisgép-konfiguráció** lap:
   
   * Hello alapértelmezett **hozzon létre egy új felhőalapú szolgáltatás**.
   * A hello **DNS-név** mezőbe írjon be egy egyedi DNS-név toouse hello címet, például "testlinuxvm" részeként.
   * A hello **régió/affinitás csoport/virtuális hálózati** mezőben válasszon ki egy régiót, ahol a virtuális lemezképet fog üzemelni.
   * A **végpontok**, tartsa hello SSH-végpontot. Mások, vegye fel vagy hozzáadása, módosítása, vagy törölje ezeket a hello virtuális gép létrehozása után.
     
     > [!NOTE]
     > Ha azt szeretné, hogy a virtuális gép toouse egy virtuális hálózatot, akkor **kell** adja meg a virtuális hálózati hello hello virtuális gép létrehozásakor. Nem adhat hozzá egy virtuális gép virtuális hálózati tooa hello virtuális gép létrehozása után. További információkért lásd: [virtuális hálózat áttekintése](../articles/virtual-network/virtual-networks-overview.md).
     > 
     > 
7. A hello utolsó **virtuálisgép-konfiguráció** lapon, hello alapértelmezett beállítások megtartásához, és kattintson a pipa jelre toofinish hello.

hello portál megjeleníti hello új virtuális gépek **virtuális gépek**. Amíg hello állapota **(kiépítése)**, hello virtuális gép beállítása folyamatban van. Ha hello állapota az elvártnak megfelelően **futtató**, továbbléphet a következő lépésben toohello.

## <a name="connect-toohello-virtual-machine"></a>Csatlakozás a virtuális gép toohello
SSH vagy PuTTY tooconnect toohello virtuális gépet, attól függően, hogy történő csatlakozás lesz hello számítógép operációs rendszerének hello fogja használni:

* Linux rendszerű számítógépeken az SSH használata. Hello parancsot, írja be a parancssorba:
  
    `$ ssh newuser@testlinuxvm.cloudapp.net -o ServerAliveInterval=180`
  
    Írja be a hello felhasználó jelszavát.
* Windows rendszerű számítógépeken a PuTTY használata. Ha nincs telepítve, töltse le hello [PuTTY letöltési oldalát][PuTTYDownload].
  
    Mentés **putty.exe** tooa könyvtárat a számítógépen. Nyisson meg egy parancssort, keresse meg a toothat mappa, és futtassa **putty.exe**.
  
    Írja be a hello állomásnevet, például a "testlinuxvm.cloudapp.net", és írja be a "22" hello **Port**.
  
    ![PuTTY képernyő][Image6]  

## <a name="update-hello-virtual-machine-optional"></a>Frissítés hello virtuális gép (nem kötelező)
1. Csatlakoztatott toohello virtuális gép közben, rendszer frissítések és javítások igény szerint telepítheti. toorun hello frissítés, típus:
   
    `$ sudo zypper update`
2. Válassza ki **szoftver**, majd **Online frissítés** toolist rendelkezésre álló frissítéseket. Válassza ki **elfogadás** toostart telepítési hello és javításokat minden új érhető el (kivéve a hello választható megfelelően).
3. Telepítés után válassza ki a **Befejezés**.  A rendszer most toodate fel.

[PuTTYDownload]: http://www.puttyssh.org/download.html

[Image1]: ./media/create-and-configure-opensuse-vm-in-portal/CreateVM.png

[Image6]: ./media/create-and-configure-opensuse-vm-in-portal/putty.png
