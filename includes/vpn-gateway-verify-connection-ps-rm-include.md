Ellenőrizheti, hogy sikerült-e a kapcsolat parancsmaggal "Get-AzureRmVirtualNetworkGatewayConnection" hello, vagy anélkül "-Debug". 

1. A következő példa parancsmagban, hello értékek toomatch konfigurálása használata hello saját. Ha a rendszer kéri, jelölje ki "A" order toorun "All". Hello példában "-Name", amelyet az tootest hello kapcsolat toohello nevére hivatkozik.

  ```powershell
  Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG
  ```
2. Hello parancsmag befejeződését követően hello értékeinek megtekintése. Hello az alábbi példában hello létesített kapcsolat állapotát jeleníti meg, ahogyan "Csatlakoztatott", és láthatja a bemenő és kimenő bájtokat.
   
  ```
  "connectionStatus": "Connected",
  "ingressBytesTransferred": 33509044,
  "egressBytesTransferred": 4142431
  ```