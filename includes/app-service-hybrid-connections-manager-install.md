
1. <span data-ttu-id="c2ed4-101">A hello **hibrid kapcsolatok** panelen hello hibrid kapcsolat imént létrehozott, majd **figyelő telepítő**.</span><span class="sxs-lookup"><span data-stu-id="c2ed4-101">In hello **Hybrid connections** blade, click hello hybrid connection you just created, then click **Listener Setup**.</span></span>
   
    ![Kattintson a figyelő beállítás](./media/app-service-hybrid-connections-manager-install/D04ClickListenerSetup.png)
2. <span data-ttu-id="c2ed4-103">Hello **hibrid kapcsolat tulajdonságai** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="c2ed4-103">hello **Hybrid connection properties** blade opens.</span></span> <span data-ttu-id="c2ed4-104">A **helyszíni Hybrid Connection Manager**, válassza a **manuális letöltés és beállítás**, mentse a letöltött hello HybridConnectionManager.msi csomagot, és másolja a hello átjáró kapcsolati karakterlánca.</span><span class="sxs-lookup"><span data-stu-id="c2ed4-104">Under **On-premises Hybrid Connection Manager**, choose **download and configure manually**, save hello downloaded HybridConnectionManager.msi package, and copy hello gateway connection string.</span></span>
   
    ![Kattintson ide tooinstall](./media/app-service-hybrid-connections-manager-install/D05ClickToInstallHCM.png)
3. <span data-ttu-id="c2ed4-106">Rendszergazdai jogú parancssort írja be a következő parancs toostart hello telepítő hello:</span><span class="sxs-lookup"><span data-stu-id="c2ed4-106">From an administrator command prompt, type hello following command toostart hello installer:</span></span>
   
        start HybridConnectionManager.msi
4. <span data-ttu-id="c2ed4-107">Hello után telepítő fut, kattintson a **most nem**, majd keresse meg a toohello %ProgramFiles%\Microsoft\HybridConnectionManager mappa, futtassa a HCMConfigWizard.exe, és kattintson **Igen** a hello **felhasználó Fiókok felügyelete** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c2ed4-107">After hello installer runs, click **Not now**, then browse toohello %ProgramFiles%\Microsoft\HybridConnectionManager folder, run HCMConfigWizard.exe and click **Yes** in hello **User Account Control** dialog.</span></span>
5. <span data-ttu-id="c2ed4-108">Illessze be a korábban kimásolt hello hibrid kapcsolati karakterláncot, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="c2ed4-108">Paste hello hybrid connection string that you copied earlier and click **OK**.</span></span> 
   
    ![Telepítése](./media/app-service-hybrid-connections-manager-install/D08aHCMInstallManual.png)
6. <span data-ttu-id="c2ed4-110">Hello telepítés befejeztével kattintson **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="c2ed4-110">When hello install completes, click **Close**.</span></span>
   
    ![Kattintson a Bezárás gombra](./media/app-service-hybrid-connections-manager-install/D09HCMInstallComplete.png)
   
    <span data-ttu-id="c2ed4-112">A hello **hibrid kapcsolatok** panelen, hello **állapot** most az oszlop látható **csatlakoztatva**.</span><span class="sxs-lookup"><span data-stu-id="c2ed4-112">On hello **Hybrid connections** blade, hello **Status** column now shows **Connected**.</span></span> 
   
    ![Csatlakoztatott állapot](./media/app-service-hybrid-connections-manager-install/D10HCStatusConnected.png)

