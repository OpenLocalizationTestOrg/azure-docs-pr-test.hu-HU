## <a name="use-the-azure-portal"></a><span data-ttu-id="0233a-101">Az Azure Portal használata</span><span class="sxs-lookup"><span data-stu-id="0233a-101">Use the Azure portal</span></span>
1. <span data-ttu-id="0233a-102">Válassza ki a virtuális Gépet kíván telepíteni, majd válassza ki a *újratelepíteni* gombra a *beállítások* panelen.</span><span class="sxs-lookup"><span data-stu-id="0233a-102">Select the VM you wish to redeploy, then select the *Redeploy* button in the *Settings* blade.</span></span> <span data-ttu-id="0233a-103">Görgessen le lásd szeretne a **támogatási és hibaelhárítási** szakaszt, amely tartalmazza az "Újra" gombra az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="0233a-103">You may need to scroll down to see the **Support and Troubleshooting** section that contains the 'Redeploy' button as in the following example:</span></span>
   
    ![Az Azure virtuális gép panel](./media/virtual-machines-common-redeploy-to-new-node/vmoverview.png)
2. <span data-ttu-id="0233a-105">A művelet megerősítését, válassza ki a *újratelepíteni* gombra:</span><span class="sxs-lookup"><span data-stu-id="0233a-105">To confirm the operation, select the *Redeploy* button:</span></span>
   
    ![Telepítse újra a virtuális gép panel](./media/virtual-machines-common-redeploy-to-new-node/redeployvm.png)
3. <span data-ttu-id="0233a-107">A **állapot** változik a virtuális gép *Frissítéskísérleti* , a virtuális gép felkészül, hogy telepítse újra, a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="0233a-107">The **Status** of the VM changes to *Updating* as the VM prepares to redeploy, as shown in the following example:</span></span>
   
    ![Virtuális gép frissítése](./media/virtual-machines-common-redeploy-to-new-node/vmupdating.png)
4. <span data-ttu-id="0233a-109">A **állapot** átvált *indítása* , a virtuális gép elindul egy új Azure gazdagépen az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="0233a-109">The **Status** then changes to *Starting* as the VM boots up on a new Azure host, as shown in the following example:</span></span>
   
    ![Virtuális gép indítása](./media/virtual-machines-common-redeploy-to-new-node/vmstarting.png)
5. <span data-ttu-id="0233a-111">A virtuális Gépet a rendszerindítási folyamat befejezését követően a **állapot** adja vissza *futtató*, jelezve a virtuális gép rendelkezik lett újratelepítése sikeresen megtörtént:</span><span class="sxs-lookup"><span data-stu-id="0233a-111">After the VM finishes the boot process, the **Status** then returns to *Running*, indicating the VM has been successfully redeployed:</span></span>
   
    ![Virtuális gép fut](./media/virtual-machines-common-redeploy-to-new-node/vmrunning.png)

