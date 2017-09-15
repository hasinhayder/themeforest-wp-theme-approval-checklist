# Comprehensive WordPress Theme Approval Checklist for ThemeForest


* **Prefix everything:** Make sure that your theme functions are prefixed before submitting your theme for review. Prefix should be identical all across your theme, and you can not use multiple prefixes. While prefixing, double check the following rules 
	* Function names should be prefixed
	* Image sizes added via `add_image_size()` function. All image size names should also be prefixed with that same prefix. 
	* All global variable names. Add a prefix to all variable names that you declare outside a function scope. 

	While prefixing, please remember that refixes should consist of either your `themename_`, `authorname_`, or `frameworkname_`

	**Example**
	
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
	
* **Rename your script and css handlers:** If you're enqueueing external css and JavaScript files, you need to rename your style and js handlers inside the `wp_enqueue_script()` function and use meaningful and readable handler names.  

	**Example** 
	
	```php
function mythemename_enqueue_js_and_css(){
		$admin_js = admin_url("admin-ajax.php"); //see it's not a global scoped variable, so you can skip prefixing
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
	
	add_action( 'wp_enqueue_scripts','mythemename_enqueue_js_and_css' );
	```
	
	For details you can check out this grappler documentation - [https://github.com/grappler/wp-standard-handles](https://github.com/grappler/wp-standard-handles)