# Nebula
#### Developer Information:

Nebula provides developers with the ability to query Nebula's enabled setting, toggle Nebula's enabled setting and create stylesheets for Nebula to use instead of the default stylesheet in certain sites.

## Interacting with Nebula

Out of the box, Nebula does not offer toggles for all browsers. However, other tweaks can interact with Nebula. This means that developers can write add-ons that allow use in more browsers.

The `NBLBroadcaster` class provides a few basic features that are necessary for writing such add-ons. (Reading and toggling dark mode at the time of this writing.) The class is part of Nebula, so it can be used on any device with Nebula installed, and you do not need to download extra files.

### Use

You can use `NBLBroadcaster` in any process where Nebula is running. When Nebula is *not* running, using the class will cause a crash. Therefore, you should be careful to check for the class before you use it.

You also need to interface the class before using it.

```objc

@interface NBLBroadcaster : NSObject
+(NBLBroadcaster *)sharedBroadcaster;
-(BOOL)darkModeEnabled;
-(void)toggleDarkmode:(BOOL)dark;
-(void)forceToggleDarkmode:(BOOL)dark;
@end

```

Toggling darkmode with `toggleDarkmode:` will only work when a web view is visible. If a web view is not visible, use `forceToggleDarkmode:`, which has *almost* the same effect. The only difference is that the effect of `forceToggleDarkmode:` will only be seen when a new web view is created or is interacted with for the first time.

#### Example

```objc

//when something happens that will use NBLBroadcaster
-(void)workMajek {
	//quit if Nebula is disabled in this app
	if(![%c(NBLBroadcaster) class]) return;
	//turn on dark mode
	[[%c(NBLBroadcaster) sharedBroadcaster] toggleDarkmode:YES];
}

```

Note that NBLBroadcaster is a singleton class, and is made to crash with the exception *"Multiple instances of NBLBroadcaster cannot be created"* when you attempt to create a new instance. Always use `[%c(NBLBroadcaster) sharedBroadcaster] `.

## Custom stylesheets

1. Themes should be installed to the /Library/Application Support/Nebula/Themes/ folder

2. Themes are .css files, any other file extensions are ignored

3. On the first line of the file, there should be a comment with the host of the site that the theme is to be loaded on:
`/* www.host.com */` for a single host, or `/* www.host.com, www.host2.com, ...*/` for multiple. Beware that some hosts use *www.* and others don't. You need to make sure that you get the correct host.

### Nebula language differences

While Nebula doesn't change any normal CSS, there are some additions to the language, mainly to allow custom styles to feel more integrated.

`NEBULA_DARK` is the chosen background colour.

`NEBULA_TEXT` is the chosen text colour.

`NEBULA_DARKER` is a darker version of `NEBULA_DARK`. This is a set amount darker, so you cannot change its value.

[See an example of a custom stylesheet](/iDH.css)
