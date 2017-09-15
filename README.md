# Comprehensive WordPress Theme Approval Checklist for ThemeForest


* Prefix everything: Make sure that your theme functions are prefixed before submitting your theme for review. Prefix should be identical all across your theme, and you can not use multiple prefixes. While prefixing, double check the following rules 
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
	
