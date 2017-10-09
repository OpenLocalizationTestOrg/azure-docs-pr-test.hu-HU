

Egy webalkalmazás projekthez az Azure-ba való létrehozásakor megadhat egy virtuális gép az Azure-ban. Ezután hello virtuális gépeket konfigurálhatja egy másik szoftver, vagy hello virtuális gép használni hibakereséshez vagy diagnosztikai célokra.

a virtuális gép egy webalkalmazás létrehozásakor toocreate kövesse az alábbi lépéseket:

1. A Visual Studióban kattintson **fájl** > **új** > **projekt** > **webes**, és válassza a **ASP.NET Web Application** (alatt hello **Visual C#** vagy **Visual Basic** csomópontok).
2. A hello **új ASP.NET projekt** párbeszédpanelen jelölje be hello írja be, és hello Azure hello párbeszédpanelen (a hello jobb alsó sarokban) szakaszában, győződjön meg arról, hogy hello webalkalmazás **hellofelhőbenlévőgazdagéphez**jelölőnégyzet be van jelölve (Ez a jelölőnégyzet be van jelölve **távoli erőforrások létrehozása** néhány telepítések esetén).
   
    ![][0]
3. Ebben a példában a Microsoft Azure-ban hello legördülő listában válasszon **virtuális gép (v1)**, és kattintson a hello **OK** gombra.
4. Jelentkezzen be tooAzure, ha a számítógép. Hello **virtuális gép létrehozása** párbeszédpanel jelenik meg.
   
    ![][2]
5. A hello **DNS-név** hello virtuális gép nevét adja meg. hello DNS-neve az Azure-ban egyedinek kell lennie. Hello megadott név nem érhető el, ha egy piros felkiáltójel jelenik meg.
6. A hello **kép** menüben válassza ki a toobase hello virtuális gép kívánt hello kép. Bármely szabványos Azure virtuálisgép-rendszerképek hello vagy az, hogy tooAzure feltöltött lemezkép választhat.
7. Hagyja hello **engedélyezése az IIS és a Web Deploy** jelölőnégyzet be van jelölve, kivéve, ha azt tervezi, hogy egy másik webkiszolgálónak tooinstall. Ha letiltja a Web Deploy, nem fogja tudni toopublish a Visual Studio. Az IIS és a Web Deploy tooany csomagolt hello Windows Server képek, beleértve a saját egyéni rendszerképét is hozzáadhat.
8. A hello **mérete** menüben válassza ki a hello hello virtuális gép méretét.
9. Hello bejelentkezési hitelesítő adatok megadása a virtuális gép számára. Jegyezze fel a őket, mert lesz szüksége tooaccess hello számítógép távoli asztalon keresztül.
10. A hello **hely** menüben válassza ki a hello régió toohost hello virtuális gépet.
11. Kattintson a hello **OK** gomb toostart hello virtuális gépet hoz létre. Hajtsa végre az hello hello művelet előrehaladását hello **kimeneti** ablak.
    
    ![][3]
12. Amikor hello virtuális gép ki van építve, közzétett parancsfájlok jön létre egy **PublishScripts** csomópont a megoldásban. hello közzétette a parancsfájl futtatása és kiosztja a virtuális gép az Azure-ban. Hello **kimeneti** ablak hello állapotát jeleníti meg. hello parancsfájl hajtja végre a következő műveletek tooset hello virtuális gép hello:
    
    * Hello virtuális gépet hoz létre, ha még nem létezik.
    * Tárfiók létrehozása kezdődő nevű `devtest`, de csak akkor, ha még nem ilyen tárfiók hello megadott régión belül.
    * Hello virtuális gép elhelyezése egy felhőalapú szolgáltatás létrehozása, és létrehoz egy webes szerepkör hello webalkalmazáshoz.
    * Konfigurálja a Web Deploy hello virtuális gépen.
    * Konfigurálja az IIS és ASP.NET hello virtuális gépen.
    
    ![][4]
13. (Választható) Új virtuális gép toohello is elérheti. A **Server Explorer**, bontsa ki a hello **virtuális gépek** csomópont, választhat hello csomópont hello virtuális gép hozott létre, és a helyi menüben, **csatlakozás a távoli asztal**. Alternatív megoldásként a **Cloud Explorer** választhat **nyissa meg a portál** a helyi menü hello és toohello virtuális gép nincs csatlakozás.
    
    ![][5]

## <a name="next-steps"></a>Következő lépések
Ha azt szeretné, toocustomize hello közzétett parancsfájlok hozott létre, olvassa el a következő részletesebb információt [Windows PowerShell-parancsfájlok használatával tooPublish tooDev és a tesztkörnyezetek](http://msdn.microsoft.com/library/dn642480.aspx).

[0]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_NewProject.PNG
[1]: ./media/dotnet-visual-studio-create-virtual-machine/CreateVM_SignIn.PNG
[2]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_CreateVM.PNG
[3]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_Provisioning.png
[4]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_SolutionExplorer.png
[5]: ./media/virtual-machines-common-classic-web-app-visual-studio/VS_Create_VM_Connect.png
