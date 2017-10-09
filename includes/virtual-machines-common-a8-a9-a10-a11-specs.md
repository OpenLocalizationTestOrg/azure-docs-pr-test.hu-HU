

## <a name="deployment-considerations"></a>Telepítési szempontok
* **Azure-előfizetés** – toodeploy több mint néhány számítási igényű példányok, fontolja meg a használatalapú előfizetés vagy egyéb beszerzési lehetőségek. Amennyiben [ingyenes Azure-fiókot](https://azure.microsoft.com/free/) használ, csak korlátozott számú számítási magot használhat az Azure-ban.

* **Tarifa- és rendelkezésre állás** -ezek a Virtuálisgép-méretek csak a Standard tarifacsomag hello kínálják. Ellenőrizze a [régió elérhető termékek] (https://azure.microsoft.com/regions/services/) a Azure-régiók rendelkezésre állás érdekében. 
* **Magok kvóta** – tooincrease hello magok kvóta szükség lehet az Azure-előfizetésben hello alapértelmezett értékből. Az előfizetés is korlátozhatja az egyes virtuális gép mérete családok, többek között a hello H-sorozat telepítése magok hello száma. toorequest a kvóta növeléséhez [nyissa meg az online támogatás ügyfélkérés](../articles/azure-supportability/how-to-create-azure-support-request.md) díjmentesen. (Alapértelmezett korlátokat eltérhetnek attól függően, hogy az előfizetés kategória.)
  
  > [!NOTE]
  > Ha nagyméretű kapacitási igényeihez van, forduljon az Azure támogatási szolgálatához. Azure-kvótákról jóváírás korlátozza, nem a kapacitás garanciát. Függetlenül attól, a kvóta van csak szó, a magok használatát.
  > 
  > 
* **Virtuális hálózati** – az Azure [virtuális hálózati](https://azure.microsoft.com/documentation/services/virtual-network/) nem szükséges toouse hello számítási igényű példány van. Azonban számos olyan telepítése esetén meg kell legalább egy felhőalapú Azure virtuális hálózat, vagy ha szüksége tooaccess webhelyek kapcsolatot a helyszíni erőforrások. Szükség esetén hozzon létre egy új virtuális hálózat toodeploy hello példányok. Az affinitáscsoportokban lévő számítási igényű virtuális gépek tooa virtuális hálózat hozzáadása nem támogatott.
* **Átméretezése** – miatt az speciális hardverre átméretezése csak az számítási igényű példányok hello belül azonos méretezés termékcsalád (H-méretek és számítási igényű A-sorozatú). Például csak átméretezheti egy H-sorozat mérete tooanother egy H sorozatú virtuális gép. Egy nem számítási intenzív tooa számítási igényű mérete az átméretezés ezenkívül nem támogatott.  
