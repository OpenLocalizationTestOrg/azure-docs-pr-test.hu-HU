<span data-ttu-id="bd830-101">Ellenőrizheti, hogy sikerült-e a kapcsolat parancsmaggal "Get-AzureRmVirtualNetworkGatewayConnection" hello, vagy anélkül "-Debug".</span><span class="sxs-lookup"><span data-stu-id="bd830-101">You can verify that your connection succeeded by using hello 'Get-AzureRmVirtualNetworkGatewayConnection' cmdlet, with or without '-Debug'.</span></span> 

1. <span data-ttu-id="bd830-102">A következő példa parancsmagban, hello értékek toomatch konfigurálása használata hello saját.</span><span class="sxs-lookup"><span data-stu-id="bd830-102">Use hello following cmdlet example, configuring hello values toomatch your own.</span></span> <span data-ttu-id="bd830-103">Ha a rendszer kéri, jelölje ki "A" order toorun "All".</span><span class="sxs-lookup"><span data-stu-id="bd830-103">If prompted, select 'A' in order toorun 'All'.</span></span> <span data-ttu-id="bd830-104">Hello példában "-Name", amelyet az tootest hello kapcsolat toohello nevére hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="bd830-104">In hello example, '-Name' refers toohello name of hello connection that you want tootest.</span></span>

  ```powershell
  Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG
  ```
2. <span data-ttu-id="bd830-105">Hello parancsmag befejeződését követően hello értékeinek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="bd830-105">After hello cmdlet has finished, view hello values.</span></span> <span data-ttu-id="bd830-106">Hello az alábbi példában hello létesített kapcsolat állapotát jeleníti meg, ahogyan "Csatlakoztatott", és láthatja a bemenő és kimenő bájtokat.</span><span class="sxs-lookup"><span data-stu-id="bd830-106">In hello example below, hello connection status shows as 'Connected' and you can see ingress and egress bytes.</span></span>
   
  ```
  "connectionStatus": "Connected",
  "ingressBytesTransferred": 33509044,
  "egressBytesTransferred": 4142431
  ```