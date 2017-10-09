### <a name="app-service-plan"></a>App Service-csomag
Hello service-csomag hello a webalkalmazás üzemeltetéséhez létrehoz. Itt megadni hello hello terv keresztül hello **hostingPlanName** paraméter. hello hello terv helye hello ugyanazon a helyen használt hello erőforráscsoport. hello árképzési szint és munkavégző mérete meg van határozva a hello **sku** és **workerSize** paraméterek

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

