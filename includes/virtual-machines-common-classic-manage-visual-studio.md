A Visual Studio Server Explorer használatával hozhat létre virtuális gépeket az Azure-ban.

## <a name="create-an-azure-virtual-machine-in-server-explorer"></a>A Server Explorer egy Azure virtuális gép létrehozása
Amíg létrehozhat egy virtuális gép hello [Azure felügyeleti portálon](http://go.microsoft.com/fwlink/?LinkID=253103), is létrehozhat egy virtuális gépet az Azure-ban a Server Explorer parancsok használatával. Virtuális gépek is használható, például egy első tooprovide end mögött egy közös terhelésű nyilvános végpontot.

### <a name="toocreate-a-new-virtual-machine"></a>új virtuális gép toocreate
1. A Server Explorer eszközben nyissa meg a hello **Azure** csomópontot, és kattintson **virtuális gépek**.
2. Hello helyi menüben, kattintson a **virtuális gép létrehozása**.
   
    Hello **hozzon létre egy új virtuális gépet** varázsló jelenik meg.
   
    ![Virtuális gép létrehozása parancs hello](./media/virtual-machines-common-classic-create-manage-visual-studio/IC718342.png)
3. A hello **válasszon egy előfizetést** lapon, válassza ki egy előfizetés toouse hello virtuális gép létrehozásakor, majd kattintson a **következő**.
   
    Ha jelenleg nincs bejelentkezve tooAzure, kattintson a **bejelentkezés** a toosign. Ezután válassza az Azure-előfizetéshez hello legördülő listában, ha még nincs kiválasztva.
4. A hello **válassza ki a virtuálisgép-lemezkép** lapon, válassza ki a képhez a hello **képtípussal** legördülő listán, és válassza ki a virtuálisgép-rendszerképek hello **lemezképnév** a listában. Amikor elkészült, kattintson a **következő**.
   
    ![Válassza ki a virtuális gép lap](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744137.png)
   
    A következő képtípussal hello választhat.
   
   * **Nyilvános képek** sorolja fel az operációs rendszerek és a kiszolgáló szoftvert, például a Windows Server és SQL Server virtuálisgép-lemezképeket.
   * **MSDN-lemezképeihez** virtuálisgép-rendszerképek szoftver elérhető tooMSDN előfizetők, például a Visual Studio és a Microsoft Dynamics sorolja fel.
   * **Saját lemezképek** listák kifejezetten és általánosítva létrehozott virtuális gépek lemezképeivel.
     
     toolearn készül kifejezetten és általánosított virtuális gépek [Virtuálisgép-lemezkép](https://azure.microsoft.com/blog/2014/04/14/vm-image-blog-post/). Lásd: [hogyan tooCapture egy Windows rendszerű virtuális gép tooUse sablonként](https://azure.microsoft.com/documentation/articles/virtual-machines-capture-image-windows-server/) hogyan létrehozása új virtuális gép használható tooquickly sablonba tooturn előre konfigurált virtuális gépek kapcsolatos információkat.
     
     Kattintson a virtuális gép lemezképének neve toosee lemezképére vonatkozó információ hello hello jobb oldalán hello lap.
     
     > [!NOTE]
     > Nem adható hozzá a virtuális gép képek toohello **nyilvános képek** vagy **MSDN-lemezképeihez** sorolja fel, mert csak olvasható. Minden olyan virtuális gépeket az Ön által létrehozott adnak toohello **saját lemezképek** listája.
     > 
     > 
     
     Ha a Visual Studio-szintű előfizetést az MSDN-előfizető, létrehozhat egy előre létrehozott Azure virtuális gép, amely a Visual Studio, valamint több lemezképet tartalmaz. További információkért lásd: [virtuális gép létrehozása a Visual Studio használatával képek Visual Studio 2013 gyűjtemény kép az MSDN-előfizetők](http://visualstudio2013msdngalleryimage.azurewebsites.net) és [MSDN előfizetések](https://www.visualstudio.com/products/msdn-subscriptions-vs). |}
5. A hello **virtuális gép alapbeállítások** lapon adjon a gép nevét, majd adja hozzá a hello specifikációk hello virtuális géphez, beleértve a hello méretét, és egy felhasználónevet és jelszót. Amikor elkészült, kattintson a **következő**.
   
    Hello új nevet fogja használni, és elfelejtett jelszó toolog hello géphez távoli asztali kapcsolattal, így egy jó ötlet toowrite le őket az eset, akkor azokat. Azure virtuális gép létrehozása a Visual Studio, után módosíthatja annak méretét és egyéb beállítások hello [Azure felügyeleti portálon](http://go.microsoft.com/fwlink/?LinkID=253103).
   
   > [!NOTE]
   > Ha úgy dönt, hogy a nagyobb méretű hello virtuális géphez, külön költségek merülhetnek fel. Lásd: [virtuális gépek díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/) további információt.
   > 
   > 
6. A Visual Studio létrehozott virtuális gépeknél szükség van a felhőszolgáltatásban. A hello **felhőalapú szolgáltatás beállításainak** lapon jelölje be a hello virtuális gép egy felhőalapú szolgáltatás, vagy kattintson **< hozzon létre új... >** hello legördülő lista, ha még nem telepítette a felhőalapú szolgáltatás, vagy szeretné, hogy új toouse az egyik. A storage-fiók is szükség, ezért válasszon tárfiókot (vagy hozzon létre egy új tárfiókot) a hello **tárfiók** legördülő listában. Lásd: [Azure Storage bemutatása tooMicrosoft](../articles/storage/common/storage-introduction.md) további információt.
7. Ha azt szeretné, hogy toospecify a virtuális hálózat (nem kötelező), válassza ki azt a hello virtuális hálózati és alhálózati legördülő listák.
   
    Virtuális gépek rendelkezésre állási csoportok tagjai telepített toodifferent tartalék tartományok. Lásd: [Azure Virtual Network](https://azure.microsoft.com/services/virtual-network/) további információt.
8. Ha a virtuális gép toobelong tooan rendelkezésre állási csoport (is nem kötelező), jelölje be a hello **adja meg a rendelkezésre állási csoportok** jelölje be a jelölőnégyzetet, és válassza a rendelkezésre állási készlet hello legördülő listában. Amikor elkészült, válassza ki a hello **következő** gombra.
   
    A virtuális gép tooan hozzáadása a rendelkezésre állási csoport segít az alkalmazás hálózati hibák, helyi lemezhiba hardver és a tervezett leállások közben elérhető marad. Toouse hello kell [Azure felügyeleti portálon](http://go.microsoft.com/fwlink/?LinkID=253103) toocreate virtuális hálózatok, az alhálózatok és a rendelkezésre állási állítja be. Lásd: [kezelése hello virtuális gépek rendelkezésre állási](https://azure.microsoft.com/documentation/articles/manage-availability-virtual-machines/) további információt.
9. A hello **végpontok** csoportjában adja meg, hogy szeretné-e a virtuális gép elérhető toousers hello nyilvános végpontok. Például választása tooenable HTTP (80-as Port) továbbá toohello távoli asztal és a PowerShell végpont, amely alapértelmezés szerint engedélyezve vannak. egy végpont, tooadd válasszon hello **Port neve** legördülő listán, és válassza a hello **Hozzáadás** gombra. tooremove egy végpontot, válassza ki a piros hello **X** hello végpontok listáját a következő toohello neve.
   
    ![hello végpontok oldalon hello virtuális gépek varázslóban.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC718351.png)
   
    rendelkezésre álló hello végpontok hello felhőszolgáltatást választotta, a virtuális gép függ. Lásd: [Azure Szolgáltatásvégpontok](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/) további információt.
   
   > [!NOTE]
   > Nyilvános végpontok az engedélyezése lehetővé teszi a szolgáltatások a virtuális gép elérhető toohello az interneten. Lehet, hogy tooinstall, és megfelelően konfigurálni hello végpontokat és a szolgáltatások a virtuális gépet, például a beállítás hozzáférés-vezérlési lista (ACL) hello végpontok. Lásd: [hogyan tooSet be végpontok tooa virtuális gép](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/) további információt.
   > 
   > 
10. Miután elkészült hello virtuális gép beállításainak konfigurálását, válassza ki azt a hello **létrehozása** gomb toocreate hello virtuális gépet.
    
     Azure hello virtuális gépet hoz létre, mert hello **Azure tevékenységnapló** mutat be hello hello virtuális gép létrehozási művelet előrehaladását.
    
     ![Virtuális gép tevékenységnapló - folyamatban.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744138.png)
    
     tooview csak virtuális gép adatait, válassza ki a hello **virtuális gépek** hello lapján **Azure tevékenységnapló**.
    
     ![Virtuális gép tevékenységnapló - befejeződött.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744139.png)
    
     Ha hello művelet sikeresen befejeződött, hello új virtuális gép megjelenik-e a hello **virtuális gépek** a Server Explorer csomópont. Jelentkezhet be hello kattintva **csatlakozzon a távoli asztali kapcsolattal** helyi.
    
     ![A virtuális gép a Server Explorer megjelenne.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744140.png)

## <a name="manage-your-virtual-machines"></a>A virtuális gépek kezelése
Hello virtuális gép konfigurációs lapján továbbá tooshutting le, csatlakozás, frissítése és toohello. az ellenőrzőpontok hozzáadását. a kiválasztott virtuális gépet, is megtekintése vagy módosítása hello virtuális gép beállításait. A következőket teheti:

* Hello virtuális gép méretének módosítása.
* Jelölje be hello rendelkezésre állási csoport toouse hello virtuális géppel.
* Hozzáadása, eltávolítása vagy nyilvános végpontok beállításainak módosítása.
* Hozzáadása, eltávolítása vagy virtuálisgép-bővítmények konfigurálása.
* Hello virtuális géphez társított hello lemezek adatainak megtekintése.

### <a name="view-or-change-virtual-machine-settings"></a>Virtuális gép beállításainak megtekintése vagy módosítása
1. A Server Explorer eszközben válassza ki a virtuális gép hello **Azure virtuális gépek** csomópont.
2. Hello helyi menüjében válassza **konfigurálása** tooview hello virtuális gép konfigurációs lapján.
   
    ![hello Azure virtuális gép konfigurációs lapján](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744141.png)
3. Hello virtuális gép adatainak megtekintése, vagy módosítsa azt.

### <a name="save-or-restore-hello-status-of-your-virtual-machine"></a>Mentse, vagy a virtuális gép hello állapotának visszaállítása
Konfigurálja a virtuális gépet, és a szoftverek telepítését, hozzon létre virtuális gépek ellenőrzőpontjaival kapcsolatban egy jó ötlet tooregularly mentse a munkáját. Ellenőrzőpont pillanatképét, vagy a virtuális gép jelenlegi állapotában hello képe. Ha probléma merül fel a virtuális gép hello, vagy azt szeretné, hogy tooreconfigure hello virtuális gép, időt takaríthat meg nulláról keresztül helyett tooa ellenőrzőpont állapotba állítja vissza.

### <a name="toocreate-a-virtual-machine-checkpoint"></a>a virtuálisgép-ellenőrzőpont toocreate
1. A Server Explorer eszközben válassza ki a virtuális gép hello **Azure virtuális gépek** csomópont.
2. Hello helyi menüjében válassza **konfigurálása** tooview hello virtuális gép konfigurációs lapján.
3. A hello konfigurálása lapon válassza a hello **lemezkép rögzítése** gombra.
   
    ![Azure-alapú konfigurációs oldal rögzítése gomb](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744142.png)
   
    Hello **virtuális gép rögzítése** párbeszédpanel jelenik meg.
   
    ![Az Azure rögzítési virtuális gép párbeszédpanel](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744143.png)
4. Adjon meg egy kép címke és a leírást. Felülírhatja azokat a saját Ha szeretné, de egy alapértelmezett címkét és egy leírást vannak megadva.
5. Ha már futtatta a Sysprep a virtuális gépen, jelölje be a hello **futtattam Sysprep hello virtuális gépen** mezőbe.
   
    A Sysprep az eszközzel, többek között rendszerek vonatkozó adatokat eltávolítja a Windows, így mások is használt sablon hello virtuális gép verziója. Lásd: [hogyan tooCapture egy Windows rendszerű virtuális gép tooUse sablonként](https://azure.microsoft.com/documentation/articles/virtual-machines-capture-image-windows-server/) további információt. Készítsen biztonsági másolatot hello VM a Sysprep futtatása előtt.
6. Miután elkészült hello rögzítési beállításainak konfigurálását, válassza ki azt a hello **rögzítése** gomb toocreate hello ellenőrzőpont.
   
    Azure hello ellenőrzőpontot hoz létre, mert hello **Azure tevékenységnapló** mutat be hello hello művelet előrehaladását.
   
    ![A virtuálisgép-ellenőrzőpont rögzítése](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744144.png)
   
    Hello ellenőrzőpont-művelet befejezése után megjelenik a hello **Azure tevékenységnapló**.
   
    ![Ellenőrzőpont művelete befejeződött](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744145.png)

## <a name="toomanage-virtual-machine-checkpoints"></a>toomanage virtuális gépek ellenőrzőpontjaival kapcsolatban
### <a name="toorestore-a-virtual-machine-tooa-previously-saved-state"></a>a virtuális gép tooa toorestore korábban mentett állapota
* Hello című rész lépéseit követve [lépésről lépésre: hajtsa végre felhő állítja vissza a Microsoft Azure virtuális gépek használata a PowerShell - 2. rész](http://blogs.technet.com/b/keithmayer/archive/2014/02/04/step-by-step-perform-cloud-restores-of-windows-azure-virtual-machines-using-powershell-part-2.aspx).

### <a name="toodelete-a-checkpoint"></a>ellenőrzőpont toodelete
1. Nyissa meg toohello [Azure felügyeleti portálon](http://go.microsoft.com/fwlink/?LinkID=253103).
2. Hello virtuális gép konfigurációs lapján válassza ki a hello **képek** lapon hello oldal hello tetején.
3. Válassza ki a kívánt toodelete, és válassza a hello hello ellenőrzőpont **törlése** alján hello hello gombra.

## <a name="shut-down-your-virtual-machine"></a>A virtuális gép leállítása
1. A Server Explorer eszközben válassza ki a kívánt tooshut le a hello hello virtuális gép **Azure virtuális gépek** csomópont.
2. A helyi menü hello, vagy válassza hello **leállítási** parancsot, vagy válasszon **konfigurálása** tooview hello a virtuális gép konfigurációs lapján, és válassza a hello **leállítási**gombra.

## <a name="next-steps"></a>Következő lépések
toolearn több virtuális gépek létrehozásával kapcsolatban lásd: [hozzon létre egy virtuális gép futó Linux](../articles/virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) és [hozzon létre egy olyan virtuális géphez a Windows hello Azure betekintő portálon](../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

