<span data-ttu-id="10989-101">Győződjön meg róla, hogy már létrehozott egy Service Bus-névteret, az [itt][namespace-how-to] ismertetettek szerint.</span><span class="sxs-lookup"><span data-stu-id="10989-101">Please ensure that you have already created a Service Bus namespace, as shown [here][namespace-how-to].</span></span>

1. <span data-ttu-id="10989-102">Jelentkezzen be az [Azure Portalra][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="10989-102">Log on to the [Azure portal][azure-portal].</span></span>
2. <span data-ttu-id="10989-103">A portál bal oldali navigációs panelén kattintson a **Service Bus** elemre (ha nem lát **Service Bus** elemet, kattintson a **További szolgáltatások** lehetőségre).</span><span class="sxs-lookup"><span data-stu-id="10989-103">In the left navigation pane of the portal, click **Service Bus** (if you don't see **Service Bus**, click **More services**).</span></span>
3. <span data-ttu-id="10989-104">Kattintson a névtérre, amelyben az üzenetsort létre kívánja hozni.</span><span class="sxs-lookup"><span data-stu-id="10989-104">Click the namespace in which you would like to create the queue.</span></span> <span data-ttu-id="10989-105">Ebben az esetben ez az **nstest1**.</span><span class="sxs-lookup"><span data-stu-id="10989-105">In this case, it is **nstest1**.</span></span>
   
    ![Üzenetsor létrehozása][createqueue1]
4. <span data-ttu-id="10989-107">A **Service Bus-névtér** panelen kattintson az **Üzenetsorok**, majd az **Üzenetsor hozzáadása** elemre.</span><span class="sxs-lookup"><span data-stu-id="10989-107">In the **Service Bus namespace** blade, select **Queues**, then click **Add queue**.</span></span>
   
    ![Üzenetsorok kiválasztása][createqueue2]
5. <span data-ttu-id="10989-109">Adjon meg egy nevet az **Üzenetsor neve** mezőben, a többi érték alapértelmezését pedig ne módosítsa.</span><span class="sxs-lookup"><span data-stu-id="10989-109">Enter the **Queue Name** and leave the other values with their defaults.</span></span>
   
    ![Új kiválasztása][createqueue3]
6. <span data-ttu-id="10989-111">Kattintson a panel alján található **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="10989-111">At the bottom of the blade, click **Create**.</span></span>

[createqueue1]: ./media/service-bus-create-queue-portal/create-queue1.png
[createqueue2]: ./media/service-bus-create-queue-portal/create-queue2.png
[createqueue3]: ./media/service-bus-create-queue-portal/create-queue3.png

[namespace-how-to]: ../articles/service-bus-messaging/service-bus-create-namespace-portal.md
[azure-portal]: https://portal.azure.com
