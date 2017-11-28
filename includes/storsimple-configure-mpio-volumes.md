#### <a name="to-configure-mpio-for-storsimple-volumes"></a><span data-ttu-id="e7084-101">Az MPIO konfigurálása a StorSimple-köteteket</span><span class="sxs-lookup"><span data-stu-id="e7084-101">To configure MPIO for StorSimple volumes</span></span>
1. <span data-ttu-id="e7084-102">Nyissa meg a **az MPIO konfigurációja**.</span><span class="sxs-lookup"><span data-stu-id="e7084-102">Open the **MPIO configuration**.</span></span> <span data-ttu-id="e7084-103">Kattintson a **Kiszolgálókezelő > irányítópult > eszközök > MPIO**.</span><span class="sxs-lookup"><span data-stu-id="e7084-103">Click **Server Manager > Dashboard > Tools > MPIO**.</span></span>
2. <span data-ttu-id="e7084-104">Az a **többutas I/O tulajdonságai** párbeszédpanelen jelölje ki a **felderítése többutas** fülre.</span><span class="sxs-lookup"><span data-stu-id="e7084-104">In the **MPIO Properties** dialog box, select the **Discover Multi-Paths** tab.</span></span>
3. <span data-ttu-id="e7084-105">Válassza ki **az iSCSI-eszközök támogatását**, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="e7084-105">Select **Add support for iSCSI devices**, and then click **Add**.</span></span>  
   
    ![Többutas I/O tulajdonságai többféle elérési utak felderítése](./media/storsimple-configure-mpio-volumes/IC741003.png)
4. <span data-ttu-id="e7084-107">Indítsa újra a kiszolgálót, amikor a rendszer kéri.</span><span class="sxs-lookup"><span data-stu-id="e7084-107">Reboot the server when prompted.</span></span>

