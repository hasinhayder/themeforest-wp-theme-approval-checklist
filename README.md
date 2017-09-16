# Comprehensive WordPress Theme Approval Checklist for ThemeForest


* [ ] **Prefix everything:** Make sure that your theme functions are prefixed before submitting your theme for review. Prefix should be identical all across your theme, and you can not use multiple prefixes. While prefixing, double check the following rules 
	* Function names should be prefixed
	* Image sizes added via `add_image_size()` function. All image size names should also be prefixed with that same prefix. 
	* All global variable names. Add a prefix to all variable names that you declare outside a function scope. 

	While prefixing, please remember that refixes should consist of either your `themename_`, `authorname_`, or `frameworkname_`

	_Example_
	
	```php
	$mythemename_variable = "whatever";
	
	function name mythemename_whatever(){
		//your function body goes here
	}
	
	add_action( 'after_setup_theme', 'mythemename_setup' );
	
	function mythemename_setup(){
		add_image_size("mythemename_single_hero", 1140, 9999);
		//rest
	}
	```
	
* [ ] **Rename your script and css handles:** If you're enqueueing external css and JavaScript files, you need to rename your style and js handlers inside the `wp_enqueue_script()` function and use meaningful and readable handler names.  

	_Example_
	
	```php
	function mythemename_enqueue_js_and_css(){
		$admin_js = admin_url("admin-ajax.php");
		wp_enqueue_script( "bootstrap-js", mythemename_get_js_assets_dir()."/bootstrap.js", array("jquery"), null,true );
		wp_enqueue_script( "cookie.js", mythemename_get_js_assets_dir()."/js.cookie.js", array("jquery"), null,true );
		wp_enqueue_script( "modernizer", mythemename_get_js_assets_dir()."/modernizr.custom.js", array("jquery"), null,true );
		wp_enqueue_script( "jquery-slicknav", mythemename_get_js_assets_dir()."/jquery.slicknav.min.js", array("jquery"), null,true );
		wp_enqueue_script( "jquery-owl", mythemename_get_js_assets_dir()."/owl.carousel.min.js", array("jquery"), null,true );
		wp_enqueue_script( "mythemename", mythemename_get_js_assets_dir()."/scripts.js", array("jquery"), "1.0.1", true);
		wp_localize_script("mythemename","mythemename",array("ajaxurl"=>$admin_js));
	
		wp_enqueue_style( "google-fonts", "//fonts.googleapis.com/css?family=Lora:400,400i,700,700i|Montserrat:300,400,700", null );
		wp_enqueue_style( "fonts-local", mythemename_get_fonts_assets_dir()."/selima.css", null );
		wp_enqueue_style( "bootstrap-css", mythemename_get_css_assets_dir()."/bootstrap.css", null );
		wp_enqueue_style( "font-awesome", mythemename_get_css_assets_dir()."/font-awesome.css", null );
		wp_enqueue_style( "owltheme", mythemename_get_css_assets_dir()."/owl.theme.default.min.css", null );
		wp_enqueue_style( "owl", mythemename_get_css_assets_dir()."/owl.carousel.min.css", null );
		wp_enqueue_style( "slicknav", mythemename_get_css_assets_dir()."/slicknav.css", null );
		wp_enqueue_style( "mythemename", mythemename_get_css_assets_dir()."/styles.css", null, "1.0.1" );
		wp_enqueue_style( "mythemename-style-responsive", mythemename_get_css_assets_dir()."/style-responsive.css", null );
	}
	
	add_action( 'wp_enqueue_scripts', 'mythemename_enqueue_js_and_css' );
	
	```
	
	For details you can check out this grappler documentation - [https://github.com/grappler/wp-standard-handles](https://github.com/grappler/wp-standard-handles)
	
* [ ] **Theme preview image size:** According to the WordPress org, the dimension of the "screenshot.png" *a.k.a "Theme Preview Image"* inside your theme directory should be of 1200px width and 900px height (1200x900). This is required to display the "theme preview image" properly on **retina / high-dpi** displays. 

* [ ] **No Comments:** If there are no comments in your WordPress posts, and you want to inform that to your users in your theme, the string must display as "No Comments". 

	In one of our submissions, accidentially the trailing "s" was deleted, and the string turned into "No Comment" which resulted a soft rejection :)
	
* [ ] **No Notices, No Warnings, No Errors:** Zip zilch nada. Make sure that your theme doesn’t raise any PHP errors, notices or warnings. Enabling `wp_debug` inside your **wp-config.php** file can help you with this. Please also remove that your theme must not generate any runtime JavaScript errors as well. To deal with this please install the [Developer](https://wordpress.org/plugins/developer/) plugin from Automattic. Then turn on "Debug Bar" in the plugin serttings. You can also manually install the [Debug Bar](https://wordpress.org/plugins/debug-bar/) plugin for this purpuse.

	_Example_
	
	```php
	define('WP_DEBUG', false);
	```
* [ ] **Oh Licneses:** This is a tricky part and has been a long going battle between WordPress and some theme vendors (and marketplaces) because items being sold in the marketplaces are usually not fully GPLed. Part of these items (for example PHP code) is licensed under GPL and the rest is proprietery licenced. Sometime the third party components like Bootstrap and many other JavaScript plugins are licensed under MIT/BSD/Apache and other different licenses available out there. This result a confusion which you must clarify while submitting your theme. You must mention to the reviewer that the theme is **Split Licenced**. In your theme style.css should mention it too

	_Example_
	
	```css
	/*
	...
	License: Split License
	...
	*/
	
	```

* [ ] **No direct enqueueing of jQuery:** If you're enqueueing third party JavaScript files or your theme scipt files, which often depends on jQuery, you should define the dependency via **dependency** param in wp_enqueue_script() function. Don't enqueue jQuery directly or individually, or it will result a soft rejection. Here's an example

	_Example (Don't do this)_

	```php
	wp_enqueue_script('jquery'); 
	```
	
	_Correct Example_
	
	```php
	wp_enqueue_script( "bootstrap-js", mythemename_get_js_assets_dir()."/bootstrap.js", array("jquery"), null,true );
	```
	
	Once **jQuery** is definied as a dependency, WordPress will automatically enqueue it from the bundled version of jQuery that ships with WordPress.
	
* [ ] **No inline scripts and styles:** You must use css class names and avoid inline css styles (as style attribue). You must also avoid inline JavaScript code through out your theme. Scripts and styles should not be hardcoded anywhere in your theme or added any other way but with `wp_enqueue_*` hook and to be added from the functions file. This includes custom JS/CSS. For inline styles use: [https://developer.wordpress.org/reference/functions/wp_add_inline_style/](https://developer.wordpress.org/reference/functions/wp_add_inline_style/) and for scripts [https://developer.wordpress.org/reference/functions/wp_add_inline_script/](https://developer.wordpress.org/reference/functions/wp_add_inline_script/)

* [ ] **Escape, Escape & Fucking Escape:** All dynamic data must be correctly escaped for the context where it is rendered. Please perform a global search for **"echo $"** in your theme and take necessary actions. You can read more on properly escaping your code, here - [Escaping: Securing Output](https://vip.wordpress.com/documentation/validating-sanitizing-escaping/#escaping-securing-output)

	While escaping, please also remember to late escape to give a clear picture about your escaped data to the reviewers. For example, check this section - [Escaping: Always Escape Late](https://vip.wordpress.com/documentation/validating-sanitizing-escaping/#always-escape-late)
	
* [ ] **No fallback for third party services:** I learned about this when we added an instagram gallery in one of our themes. On first installation, when there is no instagram username configured to display, we were using a fallback **username** to display that user's instagram feed. This behaviour caused soft rejection and we had to remove this fallback username. At the end, if there was no instagram username, we had to hide that section.

	![https://cdn-pro.dprcdn.net/files/acc_493641/EfuhRm](https://cdn-pro.dprcdn.net/files/acc_493641/EfuhRm) 
	
* [ ] **Watch out for plugin territory:** In one of our themes, we had added a feature called "Featured Posts" where anyone can check the "Feature This Post" checkbox in the post editor window. We had also added a new column in the "All Posts" table which showed a checkmark beside all the post titles that has been featured. Unfortunately this resulted in soft rejection. The reveiwer mentioned that it is a plugin territory code. 

	So our suggestion is that don't incorporate any code in your theme that changes any default behavior, or look-n-feel in any core panel or component. Write a helper plugin and add those functionalitites in that plugin. 
	
	Besides, avoid the following things from your theme and move to your helper plugin.
	
	* Any call to `wp_mail()`
	* Registering custom taxonomy
	* Registering custom post type
