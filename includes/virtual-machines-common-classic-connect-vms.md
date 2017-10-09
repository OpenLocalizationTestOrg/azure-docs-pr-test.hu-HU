

![Önálló virtuális gépek a felhőalapú szolgáltatás](./media/virtual-machines-common-classic-connect-vms/CloudServiceExample.png)

Ha a virtuális gépeket helyez egy virtuális hálózatot, dönthet úgy is azt szeretné, hogy a toouse hány felhőszolgáltatások és a rendelkezésre állási készletek betölteni. Emellett rendezhetők hello virtuális gépek hello alhálózataiban azonos módon a helyszíni hálózatot, és hello virtuális hálózati tooyour csatlakoznak a helyszíni hálózat. Íme egy példa:

![A virtuális hálózatban lévő virtuális gépek](./media/virtual-machines-common-classic-connect-vms/VirtualNetworkExample.png)

Virtuális hálózatok hello ajánlott módja tooconnect virtuális gépek Azure-ban. ajánlott eljárás hello tooconfigure az alkalmazást egy külön felhőalapú szolgáltatás egyes rétegeinek van. Azonban szükség lehet toocombine néhány virtuális gépnél a különböző alkalmazásrétegek hello be azonos cloud service tooremain hello legfeljebb 200 felhőszolgáltatások előfizetésenként belül. tooreview ezzel és más korlátok, lásd: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../articles/azure-subscription-service-limits.md).

## <a name="connect-vms-in-a-virtual-network"></a>Csatlakozás a virtuális gépek virtuális hálózatban
a virtuális hálózatban lévő virtuális gépek tooconnect:

1. Hello virtuális hálózat létrehozása a hello [Azure-portálon](../articles/virtual-network/virtual-networks-create-vnet-classic-pportal.md) , és adja meg a "klasszikus üzembe helyezési".
2. Hozza létre a felhőalapú szolgáltatásokat a központi telepítés tooreflect hello csoportját a tervező a rendelkezésre állási csoportok és a terheléselosztás. Hello Azure-portálon, kattintson **új > számítási > Felhőszolgáltatás** minden felhőalapú szolgáltatás.

  Adja meg az hello felhőalapú szolgáltatás részleteit, válassza ki a hello azonos _erőforráscsoport_ használt hello virtuális hálózattal.

3. toocreate minden egyes új virtuális gépre, kattintson a **új > számítási**, majd válassza ki a megfelelő Virtuálisgép-lemezkép a hello hello **a kiemelt alkalmazások**.

  A virtuális gép hello **alapjai** panelen válassza a hello azonos _erőforráscsoport_ használt hello virtuális hálózattal.

  ![Virtuális gép alapvető beállítások panel egy virtuális hálózat használatakor](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_VN.png)

4. Adja meg az hello VM **beállítások**, válassza ki a megfelelő hello _felhőalapú szolgáltatás_ vagy _virtuális hálózati_ hello virtuális gép számára.

  Azure hello más a kijelölt elem alapján választja ki.

  ![Virtuálisgép-beállítások panel egy virtuális hálózat használatakor](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_VN.png)


## <a name="connect-vms-in-a-standalone-cloud-service"></a>Csatlakozás a virtuális gépek önálló felhőszolgáltatásban
az önálló felhőalapú szolgáltatásként tooconnect a virtuális gépek:

1. Hello felhőalapú szolgáltatás létrehozása hello [Azure-portálon](http://portal.azure.com). Kattintson a **új > számítási > Felhőszolgáltatás**. Másik lehetőségként is létrehozhat az üzembe helyezéshez hello felhőalapú szolgáltatás, az első virtuális gép létrehozásakor.
2. Hello virtuális gépek létrehozásakor válassza ki a hello hello felhőalapú szolgáltatással használt ugyanabban az erőforráscsoportban.

  ![A virtuális gép tooan meglévő felhőalapú szolgáltatás hozzáadása](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_SA.png)

3.  Adja meg az hello VM részleteit, mivel válassza ki a hello nevét hello első lépésben létrehozott felhőalapú szolgáltatás.

  ![A virtuális gép egy felhőalapú szolgáltatás kiválasztása](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_SA.png)
