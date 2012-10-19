#iOS InAppPurchaseManager EXAMPLE OCTOBER 2012

I have had so many issues getting the OG version to work, I had to share this so others can stop struggling and get to work! Here is a modifed working version of this plugin, which also explains how to use the ApplicationPreferences Phonegap plugin to unlock products in your app, and the ever dreaded, "how do I make a restore purchases call" and then unlock contents. I have also included a few project screen shots that will hopefully help first timers out.

Props first go to the main deveoper who has made this possible, Matt Kane. And the countless other people on the web posting examples and helping out with questions.

IF ANYONE wants to help clean this up and bring it more up to speed than this, I am all for it!

## Adding the Plugin to your project

Copy the NEWLY REVISED .h and .m files to the Plugins directory in your project. Copy the NEWLY REVISED .js file to your www directory and reference it from your html file(s). Finally, add StoreKit.framework to your Xcode project if you haven't already. 

I am using cordova 2.0. If you are using 2.1 all instances of JSONString in InAppPurchaseManager.m must be replaced with cdvjk_JSONString

If you want to UNLOCK PRODUCTS as I have done below you will need to use the ApplicationPreferences Plugin. Everything works out of the box here too. https://github.com/phonegap/phonegap-plugins/tree/master/iPhone/ApplicationPreferences
You do not really need to worry about the examples displayed there for the example code below, just add the files to your plugin folder as instructed on that page.

## MOST IMPORTANT
Dont forget to add the plugins names to your .plist in your project name folder.
For example the in-app plugin needs to be..take note the first on has a lower case i, this is not a typo!
inAppPurchaseManager (string name is) InAppPurchaseManager

Then if you are using going to implement the ApplicationPreferences Plugin don't forget to add that to .plist too.
This plugins names both start with a lower case a.
applicationPreferences (string name is) applicationPreferences

## Using the plugin
You can read the details outlined on the specifics of the in-app js of the code below in this link, but my example code below will get you up and running. https://github.com/phonegap/phonegap-plugins/tree/master/iPhone/InAppPurchaseManager
I AM NOT including all the css and js for the example code below because this is a personal project, but you should be able to take the guts of what you see here and make it work for you. Maybe in the near future I will post a full working example.

Note the js is closest to top so it will load your products first, and unlock products if already purchased.

## About Itunes Connect
Make sure your in-app products are in itunes connect and make sure the message says, waiting for screenshot. That worked for me best and right away, no delay.

Also make sure you have signed all the tax documents and filled out your bank info etc for the iOS Paid Applications. This is key to selling :) You can find this under your main window when you login to Itunes Connect. Then click on the link that says, Contracts, Tax, and Banking.

![My image](http://slickremix.com/gitub-InAppPurchaseManager-EXAMPLE-images/contract-link.png)
 

### DO NOT USE SIMULATOR FOR IN-APP TESTING, USE YOUR DEVICE TO TEST. 

So in this project I used
-------------------------

1. InAppPurchaseManager
2. applicationPreferences (btw. you don't have to use this to make the inAppPurchaseManager function)
3. Cordova 2.0.0
4. Xcode 4.5.1

### NOTE
Also See Project Images below this code depicting Cordova.plist settings and bundle identifier settings, just in case you have overlooked anything. It's always better to see what a real project looks like than hearing about it, especially if it's your first one. 


	<!DOCTYPE html>
	<html>
	<head>

	<meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
	<meta name = "format-detection" content = "telephone=no"/>
	<meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no"/>
	<meta name="apple-mobile-web-app-capable" content="yes"/>
	<meta name="apple-mobile-web-app-status-bar-style" content="black"/>
	<title>FORTHEKREW MAGAZINE</title>
	<script src="js/cordova-2.0.0.js" type="text/javascript" ></script>
	<script src="js/InAppPurchaseManager.js" type="text/javascript" ></script>
	<script src="js/applicationPreferences.js" type="text/javascript" ></script>
	<script src="js/jquery-1.7.1.min.js" type="text/javascript"></script>
	<script type="text/javascript" charset="utf-8">
    	document.addEventListener('deviceready', function() {
		
		// Product ftk50                      
		window.plugins.inAppPurchaseManager.requestProductData("FTKMAG50", function(productId, title, description, price) {
															   console.log("productId: " + productId + " title: " + title + " description: " + description + " price: " + price);
			 // verify product has already been purchases and unlock 												   
			 var productIds1 = ["FTKMAG50"];
			 
			 for (var i = 0; i < productIds1.length; i++) {
			 var productId = productIds1[i];
			 window.plugins.applicationPreferences.get(productId, function(result) {
													   if (result) {
													   unlockProduct(productId);
													   console.log("restart unlocked: " + productId);
														 }
													   });
					 }
				}, 
			 
			 function(id) {
			 console.log("Invalid product id: " + id);
		});
			
		
		// Product ftk49
		window.plugins.inAppPurchaseManager.requestProductData("FTKMAG49", function(productId, title, description, price) {
															   console.log("productId: " + productId + " title: " + title + " description: " + description + " price: " + price);
			 // verify product has already been purchases and unlock 												   
			 var productIds1 = ["FTKMAG49"];
			 
			 for (var i = 0; i < productIds1.length; i++) {
			 var productId = productIds1[i];
			 window.plugins.applicationPreferences.get(productId, function(result) {
													   if (result) {
													   unlockProduct(productId);
													   console.log("restart unlocked: " + productId);
														 }
													   });
					}
				}, 
				
			 function(id) {
			 console.log("Invalid product id: " + id);
			 
		});
		
		
		// unlock product 
		function unlockProduct(productId) {
				var   a = "FTKMAG50",
					  b = "FTKMAG49";
				
				if(productId == a) {
					  $('.ftk50 .button-background div:nth-child(1n)').hide();
					  $('.ftk50 .button-background').addClass('highlight-color');
					  $('.ftk50 .button-background div:nth-child(2n)').fadeIn();
				}
				
				if(productId == b) {
					  $('.ftk49 .button-background div:nth-child(1n)').hide();
					  $('.ftk49 .button-background').addClass('highlight-color');
					  $('.ftk49 .button-background div:nth-child(2n)').fadeIn();
				}
				
			console.log('unlocked: ' + productId);
		}
							  
		// unlock product                       
		window.plugins.inAppPurchaseManager.onPurchased = function(transactionIdentifier, productId, transactionReceipt) {
		console.log('purchased: ' + productId);
		
			window.plugins.applicationPreferences.set(productId, true, function() {
													  unlockProduct(productId);
													  });
		}
		
		
		// restore products	
		window.plugins.inAppPurchaseManager.onRestored = function(transactionIdentifier, productId, originalTransactionReceipt) {
			window.plugins.applicationPreferences.set(productId, true, function() {
													  unlockProduct(productId);
                                                        //  $('#popupPadded h2').removeClass('loader');
														  $('.restore-purchases-btn').hide();
														  $('.restore-purchases-btn-complete').fadeIn();
                                                      
                                                            $('.purchase-message').hide();
                                                            $('.purchase-message-complete').show();
                                                      
													  });
			console.log('restored: ' + productId);
		}
		
		// onFailed	
		window.plugins.inAppPurchaseManager.onFailed = function(errno, errtext) {
			console.log('failed: ' + errtext);
                          
		}
							  
							  
	}); // on device ready close
	
	
		// process the confirmation dialog result
		function onConfirm(button) {
			if(button == 2) {
				window.plugins.inAppPurchaseManager.restoreCompletedTransactions();
               // $('#popupPadded h2').addClass('loader');
			}
		}
		
		// Show a custom confirmation dialog
		function showConfirm() {
			navigator.notification.confirm(
											   'Restore All Purchases?',  // message
												onConfirm,               // callback to invoke with index of button pressed
											   'FTK Magazine',          // title
											   'Cancel, OK'            // buttonLabels
										   );
            
		}
	</script>
	<link href="css/jquery.mobile-1.2.0.css" media="screen" rel="stylesheet" type="text/css" />
	<link href="css/bartender.css" type="text/css" rel="stylesheet" />
	<link href="css/jquery-mobile.css" type="text/css" rel="stylesheet" />
	<link href="css/jquery.mobile.iscrollview.css" media="screen" rel="stylesheet" type="text/css" />

	<script src="js/jquery.mobile-1.2.0.js" type="text/javascript"></script>
	<script src="js/iscroll.js" type="text/javascript"></script>
	<script src="js/jquery.mobile.iscrollview.js" type="text/javascript"></script>
    
	</head>
	<body>

	<div data-role="page" data-theme="a" id="page1">
  	<div data-role="header" data-position="fixed" data-tap-toggle="false" class="animated-icons"> <a href="#" data-icon="home" id="home" data-iconpos="notext">Home</a> <a data-icon="gear" data-iconpos="notext"  href="#popupPadded" data-rel="popup" data-transition="fade" data-position-to="window" id="settings-home">Settings</a>
    	<h1>FORTHEKREW LIBRARY</h1>
 	 </div>
 	 <!-- /header -->
    
 	 <div id="results"></div>
    
 	 <div data-role="content" data-iscroll="">
 	   <ul data-inset="true" data-role="listview" data-theme="a" id="ftk-list">
  	    <li> <a href="#"><img src="img/50-icon2.jpg" class="my-test" />
    	    <h3>Issue#50</h3>
        	<p>September-October 2012<br/>
        	Cover: Gary Bolos</p>
        	<div class="description-long">Trick tip with Dan Plunkett. Interviews with Gary Bolos and John DiLorenzo. Scene check out Naples, Florida. Coming up with Caleb Des Cognets. Shoplifter with Relief Skate Supply.</div>
      	  <h4>$1.99</h4>
     	   <div class="ftk-btn-wrap ftk50">
     	   <div class="glow-hover-effect"></div>
    	    <div class="button-background">
        	    <input type="button" value="Buy Now" data-ajax="false" id="ftk50" />
     	        <input type="button" onClick="window.open('ftk50.html','_self');" value="View Magazine" data-ajax="false" id="ftk50-view" />
    	      </div>
   		   </div>
        </a> <a href="#" data-theme="a"></a> </li>
      <li><a href="#"><img src="img/49-icon.png" />
        <h3>Issue#49</h3>
        <p>July-August 2012<br/>
        Cover: Josh Weathers</p>
        <div class="description-long">Trick tip with Andrew Edge.  Interviews with Nate Greenwood and Josh Weathers. Scene check out Jacksonville, Florida. Hemming Plaza. Go Skateboarding Day. Coming ups, Chris Lesh & more.</div>
        <h4>$1.99</h4>
        <div class="ftk-btn-wrap ftk49">
        <div class="glow-hover-effect"></div>
        <div class="button-background">
            <input type="button" value="Buy Now" data-ajax="false" id="ftk49" />
            <input type="button" onClick="window.open('ftk49.html','_self');" value="View Magazine" data-ajax="false" id="ftk49-view" />
          </div>
      </div>
        </a> <a href="#" data-theme="a"></a> </li>
     </ul>
    </div>
    <!--/content-->
  
    <div data-role="footer" data-id="mainFooter" data-position="fixed" data-tap-toggle="false">
      <div data-role="navbar" data-grid="b" id="slick-footer">
        <ul class="apple-navbar-ui comboSprite" id="footer-animate">
          <li class"settings-page"><a href="#popupPadded" data-rel="popup" data-transition="fade" data-position-to="window" data-icon="features" id="more-page">Settings</a></li>
          <li class"library-page-active"><a data-icon="brands" class="ui-btn-active ui-state-persist ui-btn-active-stay">Library</a></li>
          <li><a href="https://www.facebook.com/ftkmagazine" target="_blank" data-icon="contact">Facebook</a></li>
        </ul>
      </div>
    </div>

    

    <div id="popupPadded" data-role="popup" data-theme="a" data-overlay-theme="a">
        <a href="#" data-rel="back" data-role="button" data-theme="a" data-icon="delete" data-iconpos="notext" class="ui-btn-right close-btn">Close</a>
     
        <h2>Settings</h2>
        <p class="purchase-message">If you have previously purchased magazines that are not in your library, and would like to restore them please click below.</p>
        <p class="purchase-message-complete">Your download(s) are now complete. Thanks for keeping up with our mag. More good things to come, stay tuned!</p>
        <div id="restore-purchases"> <a href="#" data-role="button" onclick="showConfirm(); return false;" id="restore-purchases-btn" class="restore-purchases-btn">Restore All Purchases</a> <a href="#page1" data-role="button" class="restore-purchases-btn-complete ui-btn-active ui-state-persist" style="display:none" data-transition="fade">Restore Complete</a>
        
        </div>
    </div><!--#popupPadded-->
    
    
    
	</div>
	<!--/page-->


	<script type="text/javascript" charset="utf-8">
	$(document).ready(function(){
             
                  $('.close-btn').click(function () {
                            $('.library-page-active a').addClass('ui-btn-active');
                                                    })
                  
		$('#ftk-list img').fadeIn(1000);

		 setTimeout (function(){ $('.apple-navbar-ui li').fadeIn(500); },1000);
	
	$('#ftk-list li a input').buttonMarkup({ corners: true, theme: "a" });
									
	$('#ftk50').bind('touchstart', function() {
					 $('.ftk50 .glow-hover-effect').fadeIn();          
					 window.plugins.inAppPurchaseManager.makePurchase('FTKMAG50', 1); //here we make our purchase when button is tapped.
					 });
	$('#ftk50').bind('touchend', function() {
					 $('.ftk50 .glow-hover-effect').fadeOut();
					 });
	$('#ftk50-view').bind('touchstart', function() {
				     $('.ftk50 .glow-hover-effect').fadeIn();  
				     });
	$('#ftk50-view').bind('touchend', function() {
					 $('.ftk50 .glow-hover-effect').fadeOut();
				     });
	$('#ftk49').bind('touchstart', function() {
					 $('.ftk49 .glow-hover-effect').fadeIn();         
					 window.plugins.inAppPurchaseManager.makePurchase('FTKMAG49', 1); //here we make our purchase when button is tapped.
					 });
	$('#ftk49').bind('touchend', function() {
					 $('.ftk49 .glow-hover-effect').fadeOut();
					 });

	$('#ftk49-view').bind('touchstart', function() {
					 $('.ftk49 .glow-hover-effect').fadeIn();
					 });
	$('#ftk49-view').bind('touchend', function() {
					 $('.ftk49 .glow-hover-effect').fadeOut();
					 });
	                       
	});
	</script>
    
	</body>
	</html>
	
## Additional Project Images ##

Hopefully these will help some people out too. I am using Xcode 4.5.1 


### This is a look at the cordova.plist
![My image](http://slickremix.com/gitub-InAppPurchaseManager-EXAMPLE-images/overview%20of%20cordova%20plist.png)


### This is a look at the bundle identifier area that needs to match your App ID you should have created in the Itunes Developer area.
![My image](http://slickremix.com/gitub-InAppPurchaseManager-EXAMPLE-images/bundle%20identifier.png)