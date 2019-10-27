RETROBOX v2.1
==============


How it works
============

### Input Checked

NB `checked=“”` in the input actually means the browser considers the input checked:

```c
  <!--[if !mso]><!-->
<!-- do not wrap with a form tag or it will not work in IE -->
<input type="radio" class="cboxcheck" style="display:none !important;" checked=""> 
<input type="radio" name="car-rd1" class="cbox0" style="display:none !important;" checked="">
<!--<![endif]-->
```

More details here:

<https://stackoverflow.com/questions/7851868/whats-the-proper-value-for-a-checked-attribute-of-an-html-checkbox>


### Preset Input 1 (**cboxcheck/**kinetic-check**)**

The input with class .**cboxcheck is set to :checked **cboxcheck

`  <input type="radio" class="cboxcheck" style="display:none !important;" checked=""> `

**
**

Devices that read Media query use CSS to 

1.1 overrides **tv-kinetic / thumb-caroussel** inline css to display container of kinetic section

1.2 **tv-content** (each channel tv gif) is hidden by default

1.3 **tv-fallback /fallback **class is targeted to hide the fallback image which displays via inline CSS

### Preset Input 2 (cbox0)

` <input type="radio" name="car-rd1" class="cbox0" style="display:none !important;" checked="">`

1.1 CSS of `.cbox0:checked + * .content-1 `displays the first tv content/gif overiding the default CCS set by **cboxcheck **in 1.2

1.2` .cbox0:checked + * .thumb-1 img.on  `​ This dicates which button states are displayed. When cbox0 is checked onLoad then the image with class off “on” will display 

1.3`  .cbox0:checked + * .thumb-1 img.off, `​ This dicates which button states are displayed. When cbox0 is checked onLoad then the image with class off “on” will not display display 

NB in **Retrobox v2 **version cbox0 is not used. I added “checked” to the first input \#button-ch-1\. 

TV Switches
-----------

In retrobox v1 two images are used for each button with class of “on" and “off” that are targeted via css

In retrobox v2 instead of using 2 images we use the label element with background images which are switched based on the checkbox stated of the input 

Targeting kinetic content to browsers/clients that support input/forms
----------------------------------------------------------------------

Some emails use `@media screen and (-webkit-min-device-pixel-ratio: 0) {`​ to target webkit devices 

Fresh Inbox Caroussel uses checked inputs as part of the css targeting elements to be displayed for the kinetic sections. Why? Seems some clients will read css media queries but don’t support /read the checked state of inputs so what displays is messy non-functional. The fallback is hidden by css but the kinetic is non-functioning mess. 

So by targeting the kinetic sections to display via a checked input you ensure only clients with functionality are displaying them. 

`.cboxcheck:checked + input + * .tv-kinetic{} `

Webkit approach seems to be simpler way to do it - i.e. just target webkit but not sure it is as reliable - EOA inbox tests are hard to judge as the images did not display in some cases - not sure if that is due to bad test or actual CSS issues.

Retrobox v2 - html structure 
==================================

NB Because you can’t target content that sits ABOVE a set of inputs - in the case of a TV where switches are at the bottom I used the ordering below so labels are below the inputs and content - seems to work ( and is a bit more simple than the fresh inbox template/carousel where the content sits inside a set of interwoven nested \<labels\> so you can target the changes with CSS. )

If the switches sit above the content I think you shouldn’t need to follow the FreshInbox approach and can build like the v2 kinetic example:

```c
<div class="controls">  
    <input type="radio" name="cbox" id="cbox1" checked="checked">
    <input type="radio" name="cbox" id="cbox2" >
    <input type="radio" name="cbox" id="cbox3">

    <div class="content-1 tv-content">
        IMG1
    </div>
    <div class="content-2 tv-content">
         IMG2
    </div>
    <div class="content-3 tv-content">
        IMG3
    </div>

    IMG4

    <label class="label-cbox1" for="cbox1"></label>
    <label class="label-cbox2" for="cbox2"></label>
    <label class="label-cbox3" for="cbox3"></label>
 </div>
 
```

Yahoo Issues
============

Using the v2 method - css used ~ selector 

Yahoo doesn’t seem to recognise this selector - when checking in chrome dev tools it appears to get stripped

So we can use the` @media screen yahoo `​ css to force the the fallback

```c

    @media screen yahoo{
      .tv-kinetic{
        display:none!important;
      }
      .arrows-yahoo{
         display:none!important;
      }
      .arrows-yahoo img{
        display:none!important;
      }
      .fallback{
         display:block !important;
        max-height: none !important;
        height: auto !important;
        overflow: visible !important;
      }
     .cboxcheck:checked + input + * .fallback{
         display:block !important;
        max-height: none !important;
        height: auto !important;
        overflow: visible !important;
      }
    }
```
