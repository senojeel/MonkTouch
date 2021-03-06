#MonkTouch
_version 2.5_   
MonkTouch is a mobile application developed for websites that use the Ekklesia360 CMS.  <http://www.ekklesia360.com>

## Setup 
The __/app/setup.js__ file determines application tabs and Ekklesia360 modules to populate the content pane along with the content layout html. Below will explain the api tags available, as well as, the tab object structure.  In addition, you will be able to set the initial active tab. 
###MonkTouch.setup: Class

This is a custom Sencha Touch class denoted by the Ext.define. It is loaded at the initial launch of the application and only once which is declared by the variable singleton:true. All other variables and functions are custom for the setup of the application which will be described below. 

```	
	Ext.define('MonkTouch.setup',{
	    singleton: true,
	    activeTab:0, 
	    basePath:'/mobile',
	    tabs:[
	      
	        {
	            title   : 'Media',
	            view   : 'navview',
	            iconCls : 'headphones',
	            storeName   : 'Sermons',
	            filters : {
	    		  howmany:2,
	    		  hide_series:'job-an-audience-with-god'
	            },
	            contentTpl:{
	              list: 'imageList',
	              detail: 'sermonDetail'
	            },
	            xtype:'panel',
	            layout:'fit'
	            
	        },
	        {
	            title   : 'Blogs',
	            iconCls : 'doc_list',
	            view   : 'navview',
	            storeName   : 'Blogs',
	            filters : {
	              blogname:'blog-layout-1'
				  howmany:10
	            },
	            contentTpl:{
	              list: 'defaultList',
	              detail: 'defaultDetail'
	            },
	            xtype:'panel',
	            layout:'fit'
	            
	        },
	        {
	            title   : 'Events',
	            iconCls : 'calendar',
	            view   : 'navview',
	            storeName   : 'Events',
	            filters : {
			      howmany:10,
				  series:'job-an-audience-with-god'
	            },
	            contentTpl:{
	              list: 'eventList',
	              detail: 'eventDetail'
	            },
	            xtype:'panel',
	            layout:'fit'
	        },
	        {
	            title   : 'Directions',
	            iconCls : 'doc2',
	            view   : 'section',
	            storeName   : 'Section',
	            filters : {
				  id:'mobile-directions'
	            },
	            contentTpl:{
	              detail: 'defaultDetail'
	            },
	            xtype:'panel',
	            layout:'fit'
	            
	        },
	        {
	            title   : 'Photos',
	            iconCls : 'photo3',
	            view   : 'galleryview',
	            storeName   : 'Galleries',
	            filters : {
	            },
	            contentTpl:{
	              list: 'galleryList'
	            },
	            xtype:'panel',
	            layout:'fit'
	            
	        },
	        {
	            title   : 'Home',
	            iconCls : 'home',
	            view   : 'link',
	            filters : {
	              href:'http://www.monkdev.com'
	            }
	        } 
	    ],
	    setActiveTab:function(idx){
	        this.activeTab = idx;
	    },
	    getBasePath:function(){
	        return this.basePath;
	    }
	});
```

* ### activeTab _(number)_ 
>'activeTab' determines which tab will display initially upon application launch.


* ### basePath  _(string)_ 
>'basePath' is a url path to the folder where the application will be severed from.  

* ### tabs _(array of objects)_
> 'tabs' is an array of objects that determine each tab and the content that is rendered in the template. 

```
	{
           title   : 'Media',
           view   : 'navview',
           iconCls : 'headphones',
           storeName   : 'Sermons',
           filters : {
   		      howmany:2,
   		      hide_series:'job-an-audience-with-god'
           },
           contentTpl:{
             list: 'imageList',
             detail: 'sermonDetail'
           },
           xtype:'panel',
           layout:'fit'
           
       }
```
### title _(string)_  
Title is the string used in the title bar at the top of the application.

---
### view _(string)_
View instructs the application to use a specific structure for the data.
  
* __navview__ - This is a navigation view to support list/detail. Initial screen will be a list when clicked it will render the detail screen.  This will be used with Sermons, Blogs, Articles, Events. 
* __section__ - This is a view that supports straight html generated from an ekklesia360 section. It is only one screen.
* __galleryview__ - This view is a navigation view but specific to ekklesia360 galleries. First, a list of all galleries from ekklesia360 will be shown. When a gallery is chosen a grid of thumbnails will be shown.  When a thumbnail is selected an image slider will be shown allowing swiping of photos. 
* __link__ - This is really not a view but tells the application to use the url from the href variable in the filters object. The user will be prompted with a message _"Do you want to leave the site?"_.

---


### iconCls _(string)_
'iconCls' is a string referencing the icon name which will be used for tabs. More icons can be added via the **/resources/scss/app.scss**. The other icons are located in the **/sdk/resources/themes/images/default/pictos/** folder. Below are the preloaded icon names to use for the application in the iconCls variable.  
 
> calendar  
> doc_list  
> doc2  
> info_plain2  
> list  
> headphones  
> compass1  
> photo3  
> time  
> home  
> locate  
> video  

_NOTE: The goal is to keep file sizes down for mobile loading so only add icons you will use._

---

### storeName _(string)_
Stores are used to load and keep data from internal or external sources. In our case we are loading data from the cms modules. Below is a list of the stores available. 
> Sermons  
> Blogs  
> Events  
> Articles  
> Galleries  
> Section

_NOTE: If you are using a view of 'link) you won't need the storeName variable._

---

### filters _(object)_
Filters are just that, filter parameters to send to the ekklesia360 modules api. You can view what the filters are used for in the api documentation here <http://developers.monkcms.com>.

 __Filter Name__        | type          | __Modules__         
 ---------------------- | ------------- | -------------------------
 howmany         		| number        | Articles, Blogs, Events, Sermons
 howmanydays	        | number        | Events
 category               | string slug * | Articles, Blogs, Events, Sermons
 series                 | string slug   | Articles, Sermons
 group                  | string slug   | Articles, Blogs, Events, Sermons
 blogname               | string slug   | Blogs
 id                     | string slug   | Section (id is for the find api call for sections)
 order                  | string slug   | Articles, Blogs, Events, Sermons *
 hide_category          | string slug   | Articles, Blogs, Events, Sermons
 hide_series            | string slug   | Articles, Sermons
 preacher               | string slug   | Sermons
 author                 | string slug   | Articles, Blogs
 authors                | array of string slugs delineated by commas | Blogs
 tags                   | array of strings delineated by commas | Articles, Blogs, Sermons
 passage	            | string        | Sermons *
 hide_group             | string slug   | Articles, Blogs, Events, Sermons
 location               | string        | Events
 state                  | string        | Events
 near                   | string        | Events
 radius                 | string        | Events
 href                   | string (url)* | This filter is not for a module but for view link
 
 
 
 
>__slug__ : format is taking the string lowercasing, removing punctuation and adding dashes in place of spaces. _example: "job-an-audience-with-god"_  
>__order__ : view ekklesia360 [Documentation](http://developers.monkcms.com) for each module to find order values available for each module  
>__passage__ : view ekklesia360 [Documentation](http://developers.monkcms.com/articles/sermons) for passage possible strings  
>__href__ : to go to a specific url when tab is clicked set view to link and this filter as the url to redirect to
 
---

### contentTpl _(object)_
ContentTpl is to specify the templates that are used for each view of the tab panel. Below describes what template variables are required for each view. The template names are represented in the MonkTouch.templates class in the setup.js file. These are customizable and have defaults ready to use. 

__View__   | __Required Template Variables__
-----------| ----------------------
navview    | list, detail
gallerview | list
section    | detail
link       | _none_

	contentTpl:{
	    list: 'imageList',
	    detail: 'sermonDetail'
    }

----
### xtype _(string)_
This should never be changed from 'panel' and is not needed if view:'link' is used.   

---
### layout _(string)_
This should never be changed from 'fit' and is not needed if view:'link' is used.  

  
  


## MonkTouch.templates: Class
These templates are the html with Ekklesia360 api reference to use in the display panels. HTML is allowed and is pre-styled.        

### Sermons/Articles

| __Name__              | __Usage__     | __Description__                                                                  |
| --------------------- | ------------- | -------------------------------------------------------------------------------- |
| Title                 | {title}       | Title of sermon/article record                                                   |
| Image                 | {image}       | Image of sermon/article record                                                   |
| Preview               | {preview}     | Preview of sermon/article text (200 characters)                                  |
| Author                | {author}      | Author name of sermon/article                                                    |
| Date                  | {date}        | Date of sermon/article in format (Jan 01 2011)                                   |
| Text                  | {text}        | Full Content Text                                                                |
| Summary               | {summary}     | Content from summary field                                                       |
| Slug                  | {slug}        | Slug format of sermon/article name (sermon-name-slug)                            |
| Audio                 | {audio}       | Audio url of sermon/article                                                      |
| Video                 | {video}       | Video url of sermon/article                                                      |
| Embed                 | {embed}       | Embed code of sermon/article (Vimeo or Youtube) *must use iframe embed code      | 
| Embed Src             | {embedsrc}    | Embed iframe src url of sermon/article (Vimeo or Youtube) *must use iframe embed code      | 
| Embed Type            | {embedtype}   | Embed type of sermon/article either (vimeo or youtube) *must use iframe embed code      | 
| Thumb                 | {thmb}        | Thumbnail image of sermon/article (size 50 x 50)                                 | 

### Events

| __Name__              | __Usage__     | __Description__                                                                  |
| --------------------- | ------------- | -------------------------------------------------------------------------------- |
| Title                 | {title}       | Title of event record                                                            |
| Image                 | {image}       | Image of event record                                                            |
| Preview               | {preview}     | Preview of event text (200 characters)                                           |
| Text                  | {text}        | Full text of event content                                                       | 
| Coordinator Name      | {coordname}   | Name of event coordinator                                                        |
| Email                 | {email}       | Email of event coordinator                                                       |
| Phone                 | {phone}       | Phone number of event coordinator                                                |
| Start Date            | {startDate}   | Start Date of event in format (Jan 01 2011)                                      |
| Date Month            | {month}       | Month of event in format (Jan)                                                   |
| Date Day              | {day}         | Day of event in format (01)                                                      |
| Date End              | {end}         | End Date of event in format (Jan 01 2011)                                        |
| Times                 | {times}       | Full date and time of event in format (Jan 01 2011 10:30am - Jan 14 2011 5:00pm )|
| Location Name         | {locname}     | Location name of the event                                                       |
| Location Address      | {address}     | Location Address of the event                                                    |
| Google Map            | {googlemap}   | Google map of location                                                           | 
| Slug                  | {slug}        | Slug format of sermon name (event-name-slug)                                     | 
| Registration          | {register}    | Registration Link with linktext = Register                                       | 

### Blogs

| __Name__              | __Usage__     | __Description__                                                                  |
| --------------------- | ------------- | -------------------------------------------------------------------------------- |
| Title                 | {title}       | Title of blog record                                                             |
| Image                 | {image}       | Image of blog record                                                             |
| Preview               | {preview}     | Preview of blog text (200 characters)                                            |
| Summary               | {summary}     | Summary of blog (200 characters)                                                 |
| Author                | {author}      | Author name of blog                                                              |
| Date                  | {date}        | Date of blog in format (Jan 01 2011)                                            |
| Text                  | {text}        | Blog full text                                                                   |
| Slug                  | {slug}        | Slug format of blog name (blog-name-slug)                                        | 



##Styling
To update the colors for the document you will need to access /resources/sass/app.scss file. Sencha uses [Sass](http://sass-lang.com) syntax which is an extension of CSS3. In tandem with Sass they use [Compass](http://compass-style.org) which is an open-source css authoring framework with Sass as the basis. When you install compass it will include Sass. Read documentation on how to install and use Compass on their website <http://compass-style.org>.

To compile the app.scss in your terminal go to the /resources/sass/ folder. 

	$ cd /path/to/resources/sass/
	
From there you can execute compass commands if it is installed.

	$ compass watch
	
Compass watch will listen for changes to the file and compile it and output app.css to the /resources/css/ folder for you. This will allow you to see change before you build your app.

>Here is a video on theming Sencha Touch applications. [Theming Sencha Touch](http://docs.sencha.com/touch/2-0/#!/guide/theming)  

###App.scss
In the app.scss file there are several sass variables that can be edited to change the color scheme of mobile application. However, you have control to change it any way by viewing the sass variables in the [Sencha Touch Documentation](http://docs.sencha.com/touch/2-0/#!/guide)

* $base-color:  This updates the main color used for title bar and other elements.
* $base-gradient: This is a string of several different styles to choose from. [ matte, bevel,glossy,recessed,flat]
* $list-item-color: Color for each list item. 
* $back-button-color: This is the color for the back button seen in the title bar after switching views.

#### icons
To add additional icons first you can determine which icon you would like by going to the directory here **/sdk/resources/themes/images/default/pictos/** and finding the name of the icon.  Then in the app.scss file add to the css:

	@include pictos-iconmask('name-of-image'); 
	
To create your own icon I suggest using current ones to get similar size add to the pictos directory and include code in css as stated above.

##Application Icons & Startup Screen
iOS allows you to add web apps to the home screen of your device.  You can assign icon to be used to identify your application.  These are located in the **resources/icons/** directory.  You will need to replace the **icon.png** & **icon@2x.png**. The second is for Retina displays.  You can just use these png files as template to create your own. 

Secondly, the loading image when the application is started can be updated by going to **resources/loading/** directory.  You will need to update the Homescreen.jpg file. Currently the other image are of no use. 


##Build
Building the application is going to require several things and one of those is using the terminal. First, you will need to make sure you have the latest Sench SDK tools. In this documentation there is a list depending on platform. [Sencha SDK Tools](http://www.sencha.com/forum/showthread.php?192169-Important-SDK-Tools-Sencha-Command-Update)

Installing the SDK tools adds sencha commands for your terminal. 

Next, get the application files from this repository and you will need to place them in a local web directory. Using your OS's local web server or [MAMP](http://mamp.info) you will need a url to your application directory for building the app.  There are other tools like [MAMP Pro](http://mamp.info) and [VirtualHostX](http://clickontyler.com/virtualhostx/) that will allow you to create a virtual host name that makes the process easier.

In the root directory of the application there is an **app.json** file.  There is a variable at the top of that file called 'url', which will need to be changed to the virtual host url before building the app can take place.

The SDK when run takes all the assets(javascript, css), minifies and combines them and removes unneeded javascript classes. This allows the application to be as lean as possible for the mobile environment.

When you have your local server setup open your terminal and go to the directory of your application. 

	$ cd /path/to/application
	
If you have installed the sdk tools, and have your url in the app.json file we can build the application by:
 
	$ sencha app build production
	
This will add a folder /build/ to your application, within that will be a folder /production/. In that folder will be your application assets to deploy to your website. 

> Remember that you added a url in the setup.js file to the basePath variable. Make sure that path is where your application is located on your web server. 

##Testing
If you are testing your application locally don't forget to add the monkcms.php and monkcode.txt files from the site you are pulling data from to you root directory of your local web server.


##Additional Resources
* [SDK & Tools update](http://www.sencha.com/forum/showthread.php?192169-Important-SDK-Tools-Sencha-Command-Update)
* [Getting Started With Sencha Touch 2](http://docs.sencha.com/touch/2-0/#!/guide/getting_started)

##Credits

Built with Sencha Touch 2.0.1  
built by [Monk Development](http://www.mokdevelopment.com)  

