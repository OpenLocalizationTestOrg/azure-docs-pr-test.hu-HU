## <a name="install-wordpress"></a><span data-ttu-id="963f1-101">WordPress telepítése</span><span class="sxs-lookup"><span data-stu-id="963f1-101">Install WordPress</span></span>

<span data-ttu-id="963f1-102">Ha ki szeretné próbálni a Jegyzettömböt, a minta-alkalmazások telepítése.</span><span class="sxs-lookup"><span data-stu-id="963f1-102">If you want to try your stack, install a sample app.</span></span> <span data-ttu-id="963f1-103">Tegyük fel, a következő lépéseket a nyílt forráskódú telepítése [WordPress](https://wordpress.org/) platform webhelyek és blogok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="963f1-103">As an example, the following steps install the open source [WordPress](https://wordpress.org/) platform to create websites and blogs.</span></span> <span data-ttu-id="963f1-104">Más munkaterhelésekhez, próbálja meg a következők [Drupal](http://www.drupal.org) és [Moodle](https://moodle.org/).</span><span class="sxs-lookup"><span data-stu-id="963f1-104">Other workloads to try include [Drupal](http://www.drupal.org) and [Moodle](https://moodle.org/).</span></span> 

<span data-ttu-id="963f1-105">A WordPress-telepítő a koncepció igazolása szolgál.</span><span class="sxs-lookup"><span data-stu-id="963f1-105">This WordPress setup is for proof of concept.</span></span> <span data-ttu-id="963f1-106">További információk és éles telepítési beállításait, a [WordPress dokumentáció](https://codex.wordpress.org/Main_Page).</span><span class="sxs-lookup"><span data-stu-id="963f1-106">For more information and settings for production installation, see the [WordPress documentation](https://codex.wordpress.org/Main_Page).</span></span> 



### <a name="install-the-wordpress-package"></a><span data-ttu-id="963f1-107">A WordPress telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="963f1-107">Install the WordPress package</span></span>

<span data-ttu-id="963f1-108">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="963f1-108">Run the following command:</span></span>

```bash
sudo apt install wordpress
```

### <a name="configure-wordpress"></a><span data-ttu-id="963f1-109">A WordPress konfigurálása</span><span class="sxs-lookup"><span data-stu-id="963f1-109">Configure WordPress</span></span>

<span data-ttu-id="963f1-110">WordPress MySQL és a PHP használatára konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="963f1-110">Configure WordPress to use MySQL and PHP.</span></span> <span data-ttu-id="963f1-111">Nyisson meg egy szövegszerkesztőt, az Ön által választott, és a fájl létrehozásához a következő parancsot `/etc/wordpress/config-localhost.php`:</span><span class="sxs-lookup"><span data-stu-id="963f1-111">Run the following command to open a text editor of your choice and create the file `/etc/wordpress/config-localhost.php`:</span></span>

```bash
sudo sensible-editor /etc/wordpress/config-localhost.php
```
<span data-ttu-id="963f1-112">Másolja a következő sorokat a fájlt, és az adatbázis-jelszót *yourPassword* (hagyja változatlanul más értékek).</span><span class="sxs-lookup"><span data-stu-id="963f1-112">Copy the following lines to the file, substituting your database password for *yourPassword* (leave other values unchanged).</span></span> <span data-ttu-id="963f1-113">Ezután mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="963f1-113">Then save the file.</span></span>

```php
<?php
define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpress');
define('DB_PASSWORD', 'yourPassword');
define('DB_HOST', 'localhost');
define('WP_CONTENT_DIR', '/usr/share/wordpress/wp-content');
?>
```

<span data-ttu-id="963f1-114">Egy munkakönyvtárba, hozzon létre egy szövegfájlt `wordpress.sql` a WordPress-adatbázis konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="963f1-114">In a working directory, create a text file `wordpress.sql` to configure the WordPress database:</span></span> 

```bash
sudo sensible-editor wordpress.sql
```

<span data-ttu-id="963f1-115">Adja hozzá a következő parancsokat, és az adatbázis-jelszót *yourPassword* (hagyja változatlanul más értékek).</span><span class="sxs-lookup"><span data-stu-id="963f1-115">Add the following commands, substituting your database password for *yourPassword* (leave other values unchanged).</span></span> <span data-ttu-id="963f1-116">Ezután mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="963f1-116">Then save the file.</span></span>

```sql
CREATE DATABASE wordpress;
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER
ON wordpress.*
TO wordpress@localhost
IDENTIFIED BY 'yourPassword';
FLUSH PRIVILEGES;
```


<span data-ttu-id="963f1-117">A következő parancsot az adatbázis létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="963f1-117">Run the following command to create the database:</span></span>

```bash
cat wordpress.sql | sudo mysql --defaults-extra-file=/etc/mysql/debian.cnf
```

<span data-ttu-id="963f1-118">A parancs befejezése után törölje a fájlt `wordpress.sql`.</span><span class="sxs-lookup"><span data-stu-id="963f1-118">After the command completes, delete the file `wordpress.sql`.</span></span>

<span data-ttu-id="963f1-119">Helyezze át a WordPress telepítése a web server dokumentumgyökér:</span><span class="sxs-lookup"><span data-stu-id="963f1-119">Move the WordPress installation to the web server document root:</span></span>

```bash
sudo ln -s /usr/share/wordpress /var/www/html/wordpress

sudo mv /etc/wordpress/config-localhost.php /etc/wordpress/config-default.php
```

<span data-ttu-id="963f1-120">Most már a WordPress beállításának befejezése és közzététele a platformon.</span><span class="sxs-lookup"><span data-stu-id="963f1-120">Now you can complete the WordPress setup and publish on the platform.</span></span> <span data-ttu-id="963f1-121">Nyisson meg egy böngészőt, és navigáljon `http://yourPublicIPAddress/wordpress`.</span><span class="sxs-lookup"><span data-stu-id="963f1-121">Open a browser and go to `http://yourPublicIPAddress/wordpress`.</span></span> <span data-ttu-id="963f1-122">Helyettesítse be a virtuális gép nyilvános IP-címét.</span><span class="sxs-lookup"><span data-stu-id="963f1-122">Substitute the public IP address of your VM.</span></span> <span data-ttu-id="963f1-123">Ez a rendszerkép hasonlóan kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="963f1-123">It should look similar to this image.</span></span>

![WordPress telepítése oldal](./media/virtual-machines-linux-tutorial-wordpress/wordpressstartpage.png)