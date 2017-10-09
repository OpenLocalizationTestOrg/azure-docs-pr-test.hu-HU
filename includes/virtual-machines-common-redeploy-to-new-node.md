## <a name="use-hello-azure-portal"></a><span data-ttu-id="28d48-101">Hello Azure portál használata</span><span class="sxs-lookup"><span data-stu-id="28d48-101">Use hello Azure portal</span></span>
1. <span data-ttu-id="28d48-102">Válassza ki a virtuális gép tooredeploy kívánja, majd válassza ki a hello hello *újratelepíteni* hello gombjára *beállítások* panelen.</span><span class="sxs-lookup"><span data-stu-id="28d48-102">Select hello VM you wish tooredeploy, then select hello *Redeploy* button in hello *Settings* blade.</span></span> <span data-ttu-id="28d48-103">Szükség lehet a toosee hello le tooscroll **támogatási és hibaelhárítási** hello "Újra" gombra, mint például a következő hello tartalmazó szakasz:</span><span class="sxs-lookup"><span data-stu-id="28d48-103">You may need tooscroll down toosee hello **Support and Troubleshooting** section that contains hello 'Redeploy' button as in hello following example:</span></span>
   
    ![Az Azure virtuális gép panel](./media/virtual-machines-common-redeploy-to-new-node/vmoverview.png)
2. <span data-ttu-id="28d48-105">tooconfirm hello művelet, jelölje be hello *újratelepíteni* gombra:</span><span class="sxs-lookup"><span data-stu-id="28d48-105">tooconfirm hello operation, select hello *Redeploy* button:</span></span>
   
    ![Telepítse újra a virtuális gép panel](./media/virtual-machines-common-redeploy-to-new-node/redeployvm.png)
3. <span data-ttu-id="28d48-107">Hello **állapot** hello a virtuális gép vált közé tartoznak túl*Frissítéskísérleti* , a virtuális gép hello előkészíti tooredeploy, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="28d48-107">hello **Status** of hello VM changes too*Updating* as hello VM prepares tooredeploy, as shown in hello following example:</span></span>
   
    ![Virtuális gép frissítése](./media/virtual-machines-common-redeploy-to-new-node/vmupdating.png)
4. <span data-ttu-id="28d48-109">Hello **állapot** majd módosítja túl*indítása* , hello virtuális gép elindul az új Azure-gazdagépre, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="28d48-109">hello **Status** then changes too*Starting* as hello VM boots up on a new Azure host, as shown in hello following example:</span></span>
   
    ![Virtuális gép indítása](./media/virtual-machines-common-redeploy-to-new-node/vmstarting.png)
5. <span data-ttu-id="28d48-111">Hello VM hello rendszerindítási folyamat befejezése után hello **állapot** visszahelyeződik túl*futtató*, hello VM jelző rendelkezik lett újratelepítése sikeresen megtörtént:</span><span class="sxs-lookup"><span data-stu-id="28d48-111">After hello VM finishes hello boot process, hello **Status** then returns too*Running*, indicating hello VM has been successfully redeployed:</span></span>
   
    ![Virtuális gép fut](./media/virtual-machines-common-redeploy-to-new-node/vmrunning.png)

