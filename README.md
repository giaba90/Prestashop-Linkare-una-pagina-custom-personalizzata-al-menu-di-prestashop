[Prestashop] Linkare una pagina custom(personalizzata) al menu di prestashop
======================================================

Per prima cosa bisogna creare una pagina custom :

1. Create un nuovo controller per la nostra pagina. Es. **NewPageController.php**
```php
<?php

class NewPageControllerCore extends FrontController
{
  public $php_self = 'new-page.php';

  public function setMedia()
  {
		  parent::setMedia();
		  Tools::addCSS(_THEME_CSS_DIR_.'new-page.css');
		  Tools::addJS(_THEME_JS_DIR_.'new-page.js');
  }

  public function displayContent()
  {
		   parent::displayContent();
		   self::$smarty->display(_PS_THEME_DIR_.'new-page.tpl');
  }
}
```

Il file **NewPageController.php** deve essere salvato nella cartella **controller**, mentre i file js e css riferiti alla nostra pagina custom devono essere salvate all’interno delle cartelle js e css del tema stesso.

2. Creiamo la pagina **new-page.php** ed incolliamo al suo interno questo codice
```php
<?php

require(dirname(__FILE__).'/config/config.inc.php');
ControllerFactory::getController('NewPageController')->run();
```

il file new-page.php deve essere posizionato nella root del sito, dove sono presenti gli altri file php di prestashop  

3. Creiamo il file tpl per la pagina php creata prima
**new-page.tpl** avra’ ad esempio una forma del genere
```php
{capture name=path}{l s='NewPage'}{/capture}
{include file="$tpl_dir./breadcrumb.tpl"}

<h1>{l s='Custom new page'}</h1>
```

Ultima cosa da fare e’ andare nel **Back Office -> Preferenze -> SEO & URLs** poi cliccate su **Aggiungi Nuovo** e selezionate la pagina custom appena creata e compilate il resto dei campi. Cliccate su **Salva**

Così facendo la vostra pagina sarà visibile all’indirizzo miosito.it/custom-new-page 

—————


Create submenu links with custom page
=====================================
