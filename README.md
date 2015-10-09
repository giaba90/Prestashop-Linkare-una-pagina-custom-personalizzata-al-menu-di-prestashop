Linkare una pagina custom(personalizzata) al menu di prestashop
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

Il file **NewPageController.php** deve essere salvato nella cartella **controller**, mentre i file js e css riferiti alla nostra pagina custom devono essere salvati all’interno delle cartelle js e css del tema stesso.

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

---

Per aggiungere questa pagina appena creata al nostro menu' dopo seguire questi passagi.
Mettiamo il caso di voler creare la voce di menu' ** Azienda ** che ha come sottovoce *Mission*

dobbiamo innanzittutto creare dal Back Office di Prestashop una categoria a cui diamo il nome di Azienda, poi creiamo una pagina sempre dal back office di prestashop e gli diamo il nome Mission. Una volta cliccato su *Salva* la pagina appena creata sara' visualizzata nell'elenco. Da qui prendiamo l'id come mostrato in figura

![alt text](https://github.com/giaba90/Prestashop-Linkare-una-pagina-custom-personalizzata-al-menu-di-prestashop/blob/master/presta.jpg "Prestashop create page")


ed andiamo a modificare il file blocktopmenu.php si trova in modules -> blocktopmenu 
sostituendo questo codice qui
```php
foreach ($pages as $page)
			{
				$cms = new CMS($page['id_cms'], (int)$id_lang);
				$links = $cms->getLinks((int)$id_lang, array((int)$cms->id));

				$selected = ($this->page_name == 'cms' && ((int)Tools::getValue('id_cms') == $page['id_cms'])) ? ' class="sfHoverForce"' : '';
				$this->_menu .= '<li '.$selected.'>';
				$this->_menu .= '<a href="'.$links[0]['link'].'">'.$cms->meta_title.'</a>';
				$this->_menu .= '</li>';
			}
```
con questo
```php
foreach ($pages as $page)
			{
				$cms = new CMS($page['id_cms'], (int)$id_lang);
				$links = $cms->getLinks((int)$id_lang, array((int)$cms->id));

				$selected = ($this->page_name == 'cms' && ((int)Tools::getValue('id_cms') == $page['id_cms'])) ? ' class="sfHoverForce"' : '';
                if ($cms->id==9){
				$this->_menu .= '<li '.$selected.'>';
				$this->_menu .= '<a href="URL_TO_GALLERY">'.$cms->meta_title.'</a>';
				$this->_menu .= '</li>';
                } else {
                $this->_menu .= '<li '.$selected.'>';
				$this->_menu .= '<a href="'.$links[0]['link'].'">'.$cms->meta_title.'</a>';
				$this->_menu .= '</li>';    
                }
			}
```
nel mio ho scritto ```php $cms->id==9 ``` perchè era l'id della mia pagina era 9



Create submenu links with custom page
=====================================

ENG VERSION WILL COMING SOON
