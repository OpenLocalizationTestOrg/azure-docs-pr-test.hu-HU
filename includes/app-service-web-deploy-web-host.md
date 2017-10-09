### <a name="app-service-plan"></a><span data-ttu-id="913cd-101">App Service-csomag</span><span class="sxs-lookup"><span data-stu-id="913cd-101">App Service plan</span></span>
<span data-ttu-id="913cd-102">Hello service-csomag hello a webalkalmazás üzemeltetéséhez létrehoz.</span><span class="sxs-lookup"><span data-stu-id="913cd-102">Creates hello service plan for hosting hello web app.</span></span> <span data-ttu-id="913cd-103">Itt megadni hello hello terv keresztül hello **hostingPlanName** paraméter.</span><span class="sxs-lookup"><span data-stu-id="913cd-103">You provide hello name of hello plan through hello **hostingPlanName** parameter.</span></span> <span data-ttu-id="913cd-104">hello hello terv helye hello ugyanazon a helyen használt hello erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="913cd-104">hello location of hello plan is hello same location used for hello resource group.</span></span> <span data-ttu-id="913cd-105">hello árképzési szint és munkavégző mérete meg van határozva a hello **sku** és **workerSize** paraméterek</span><span class="sxs-lookup"><span data-stu-id="913cd-105">hello pricing tier and worker size are specified in hello **sku** and **workerSize** parameters</span></span>

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "[parameters('sku')]",
        "capacity": "[parameters('workerSize')]"
      },
      "properties": {
        "name": "[parameters('hostingPlanName')]"
      }
    },

