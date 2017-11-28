<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-the-storsimple-adapter-for-sharepoint"></a><span data-ttu-id="0fa23-101">A SharePoint a StorSimple-Adapter telepítése</span><span class="sxs-lookup"><span data-stu-id="0fa23-101">To install the StorSimple Adapter for SharePoint</span></span>
1. <span data-ttu-id="0fa23-102">A telepítő másolja a (WFE) előtér-webkiszolgálóján, a SharePoint központi felügyelet webalkalmazás futtatásához is konfigurálva van.</span><span class="sxs-lookup"><span data-stu-id="0fa23-102">Copy the installer to the web front end (WFE) server that is also configured to run the SharePoint Central Administration web application.</span></span> 
2. <span data-ttu-id="0fa23-103">Rendszergazdai jogosultságokkal rendelkező fiók használatával jelentkezzen be az ELŐTÉR-webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="0fa23-103">Use an account with administrator privileges to log on to the WFE server.</span></span>
3. <span data-ttu-id="0fa23-104">Kattintson duplán a telepítő.</span><span class="sxs-lookup"><span data-stu-id="0fa23-104">Double-click the installer.</span></span> <span data-ttu-id="0fa23-105">A StorSimple-adaptert SharePoint telepítővarázsló elindul.</span><span class="sxs-lookup"><span data-stu-id="0fa23-105">The StorSimple Adapter for SharePoint Setup Wizard starts.</span></span> <span data-ttu-id="0fa23-106">Kattintson a **tovább** a telepítés megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="0fa23-106">Click **Next** to begin the installation.</span></span>
   
    ![StorSimple adapter telepítő start lap](./media/storsimple-install-sharepoint-adapter/HCS_SSASP_Setup1-include.png)
4. <span data-ttu-id="0fa23-108">A SharePoint konfigurációs telepítőből StorSimple adapterén, adja meg egy telepítési helyét, írja be a DATA 0 hálózati adapterén IP-címét a StorSimple eszköz, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="0fa23-108">In the StorSimple Adapter for SharePoint setup configuration page, select an installation location, type the IP address for the DATA 0 network interface on your StorSimple device, and then click **Next**.</span></span> 
   
    ![StorSimple adapter telepítés konfigurálása lapon](./media/storsimple-install-sharepoint-adapter/HCS_SSASP_Setup2-include.png) 
5. <span data-ttu-id="0fa23-110">A telepítő megerősítési oldalán kattintson **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="0fa23-110">In the setup confirmation page, click **Install**.</span></span>
   
    ![StorSimple adapter telepítő visszaigazolási lapja](./media/storsimple-install-sharepoint-adapter/HCS_SSASP_Confirm_Setup-include.png) 
6. <span data-ttu-id="0fa23-112">Kattintson a **Befejezés** a telepítővarázsló bezárásához.</span><span class="sxs-lookup"><span data-stu-id="0fa23-112">Click **Finish** to close the Setup Wizard.</span></span>
   
    ![StorSimple adapter beállítása befejeződött lapon](./media/storsimple-install-sharepoint-adapter/HCS_SSASP_Setup_finish-include.png) 
7. <span data-ttu-id="0fa23-114">Nyissa meg a SharePoint központi felügyelet lapján.</span><span class="sxs-lookup"><span data-stu-id="0fa23-114">Open the SharePoint Central Administration page.</span></span> <span data-ttu-id="0fa23-115">Meg kell jelennie egy StorSimple konfigurációs csoportot, amely a SharePoint-hivatkozások a StorSimple-adaptert tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="0fa23-115">You should see a StorSimple Configuration group that contains the StorSimple Adapter for SharePoint links.</span></span>
8. <span data-ttu-id="0fa23-116">Nyissa meg a következő lépéssel: [konfigurálása RBS](#configure-rbs).</span><span class="sxs-lookup"><span data-stu-id="0fa23-116">Go to the next step: [Configure RBS](#configure-rbs).</span></span>

