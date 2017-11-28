#### <a name="to-delete-a-cloud-appliance"></a><span data-ttu-id="2fe84-101">Felhőalapú készülék törlése</span><span class="sxs-lookup"><span data-stu-id="2fe84-101">To delete a cloud appliance</span></span>

1. <span data-ttu-id="2fe84-102">Jelentkezzen be az Azure portálra.</span><span class="sxs-lookup"><span data-stu-id="2fe84-102">Sign in to the Azure portal.</span></span>
2. <span data-ttu-id="2fe84-103">Csak olyan inaktív eszközt törölhet, amely nem tartalmaz adatot.</span><span class="sxs-lookup"><span data-stu-id="2fe84-103">You can only delete a deactivated device that does not contain data.</span></span> <span data-ttu-id="2fe84-104">Először távolítsa el az eszközön található adatokat, vagy [feladatátvétellel](../articles/storsimple/storsimple-8000-device-failover-cloud-appliance.md) vigye át a kötet tárolójában található adatokat egy másik eszközre.</span><span class="sxs-lookup"><span data-stu-id="2fe84-104">Delete the data on the device first or you can [fail over the data](../articles/storsimple/storsimple-8000-device-failover-cloud-appliance.md) in volume containers to another device.</span></span> <span data-ttu-id="2fe84-105">Az adatok törlése után inaktiválhatja az eszközt.</span><span class="sxs-lookup"><span data-stu-id="2fe84-105">Once the data is deleted, you are ready to deactivate the device.</span></span>
3. <span data-ttu-id="2fe84-106">A StorSimple-eszközkezelő szolgáltatás oldalán kattintson az **Eszközök** elemre, majd válassza ki az eszközt.</span><span class="sxs-lookup"><span data-stu-id="2fe84-106">In your StorSimple Devide Manager service page, click **Devices** and then select the device.</span></span> <span data-ttu-id="2fe84-107">Kattintson a jobb gombbal, és válassza az **Inaktiválás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2fe84-107">Right-click and select **Deactivate**.</span></span>
4. <span data-ttu-id="2fe84-108">Az eszköz az inaktiválása után kattintson rá a jobb gombbal, és válassza a **Törlés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2fe84-108">Once the device is deactivated, right-click the device and select **Delete**.</span></span>

    ![Válassza ki az inaktív eszközt, és kattintson a törlés lehetőségre](./media/storsimple-8000-delete-cloud-appliance/delete-cloud-appliance1.png)

5. <span data-ttu-id="2fe84-110">Gépelje be az eszköz nevét, és erősítse meg a törlést.</span><span class="sxs-lookup"><span data-stu-id="2fe84-110">Type the device name to confirm the deletion.</span></span> <span data-ttu-id="2fe84-111">Az eszköz törlését követően az eszközlista frissül.</span><span class="sxs-lookup"><span data-stu-id="2fe84-111">After the device is deleted, the device list updates.</span></span>

    ![Törlés megerősítése](./media/storsimple-8000-delete-cloud-appliance/delete-cloud-appliance2.png)

6. <span data-ttu-id="2fe84-113">Az eszköz törléséről értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="2fe84-113">You are notified after the device is deleted.</span></span>

    ![Értesítés az eszköz sikeres törléséről](./media/storsimple-8000-delete-cloud-appliance/delete-cloud-appliance4.png)

7. <span data-ttu-id="2fe84-115">Az eszközök listája frissül, jelezve a törölt eszközt.</span><span class="sxs-lookup"><span data-stu-id="2fe84-115">The list of devices updates to indicate the deleted device.</span></span>

    ![Frissített eszközlista](./media/storsimple-8000-delete-cloud-appliance/delete-cloud-appliance5.png)
