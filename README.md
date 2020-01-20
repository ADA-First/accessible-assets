#Accessibile Assets
## Updated
###(sorta)
We have an accessibility auditing application called the [Website Accessibility Checker](https://compliance.adafirst.org) that is based on an open-source project called the [Functional Accessibility Evaluator](https://github.com/opena11y/fae2), which uses Bootstrap 3.x.

While working on that, I came across a [collection of accessible JS libraries](https://assets.cms.gov/resources/framework/3.4.1/Pages/) that was actually compiled by the gov't while I was browsing a [list accessible JS libraries from the University of Illinois](https://www.disability.illinois.edu/academic-support/accessible-it-group/accessiblejslibraries).

It was a bit outdated and was full of broken links (the links to assets were all in directory called `vendor` yet upon download there was no such folder.

There were also folders with no assets at all in them and this doesn't seem to have been updated in about 5 years—Bootstrap 5 is due out soon and the current version of jQuery is 3.4.1 (as of January 2020).

I also found it amusing that in their documentation they included a viewport meta tag with `maximum-scale=1.0, user-scalable=no` attributes and that in itself causes accessibility problems by preventing users from zooming.

As such, I decided to give it a mild update and upload it for anyone that finds it useful.

##How To Use These Assets

First of all, use them as a **place to start your research**.

By that I mean check if there is a more current version of any asset you find here. As noted above, many of these assets are out-if-date and accessibility has become increasingly important in recent years (legally speaking. In 2015 when this was released there were about 60 lawsuits over web accessibility filed in U.S. Federal Courts in 2019 there were over 2,500).

The files downloaded from the government's website didn't include an `index.html` file, nor a README so I added them.

The `index.html` file I udpated slightly to illustrate what I consider best practices.

1. Added a meta tag, which might more ideally be sent as an HTTP header to tell users of I.E. to render it using the latest rendering engine available.
2. Modified the viewort meta tag to allow zooming.
3. Put the print sytles into a separate `print.min.css` file along with an attribute on the link indicating it is for print. All browsers will still load it but unless the user agent is one designed for priniting it will be given a lower priority thus allowing it to more or less be downloaded in the background asynchronously (it's unlikely anyone will want to print your page before it loads and they read it, so we prioritize on-screen rendering)
4. Load the `HTML5 shiv` and `respond.js` from a CDN, but only for user agents that need it. by putting them in conditional comments directed at Internet Explorer less than 9. **Note**: the HTML5 shiv is included in mdernizr which is loaded shortly thereafter so use one or the other. I don't usually use modernizr anymore because I'm lazy (and working on my own projects), so while it does tests and add claseses, unless you act upon that by loading polyfills or by writing CSS that is specific to those additional classes then it's just bloat that goes ununsed.
5. Demonstrating the use of `rel="preconnect` to give resource hints to speed up the loading of jQuery (or othr libraries) from a CDN.
6. I included skip links as the first on-page content to help screenreader users. I applied basic bootstrap style to visually hide the content while still having it be accessibile and focusable. Search is left commented out because there is no search function and the links to the sitemap and etc. don't point to anything that actually exists, you need to create them.
7. The standard "legal" pages websites have needed for quite some time have been a terms of service and a privacy policy. Given the rise in accessibility lawsuits (because people care more about avoiding getting sued than they do about user experience and search engine optimization), it's now quite important to have a sitemap (an HTML sitemap, for humans not an XML sitemap that you'd submit to search engines) with links to important parts of your site and an accessibility statement where you list alternate ways that users can contact you to report accessibility problems.
8. I'm using the jQuery from a CDN with a local fallback script from the [HTML5 boilerplate](https://html5boilerplate.com) if you have read this far, you should go check out that project, too
9. I'm using [Bootstrap](https://getbootstrap.com) 3.x for some simple styling (including the Bootstrap theme to give gradients and dropshadows to button and an additioal Bootstrap accessibility library that fixes som accessibilty problems, not sure if it works with the version of jQuery being loaded, and it's really overkill, a simple page like this probably only needs 20 lines of CSS and no JS at all.
10. If you're going to use Bootstrap, use the latest version if it fits your needs or customize it to fit your needs. Truth be told, I think I prefer [Foundation](https://foundation.zurb.com) but both are awesome libraries.
11. I go back and forth about whether to use `lang="en"` or `lang="en-US"` as this project does. Brittish English and American English are so similar, I doubt it matters but I did make it a point to add `dir="ltr"` for good measure as English is always read "left to right".
12. Added an Accessibility Widget that allows users to customize the appearance to suit their needs.

##Warning About The Gov't Documentation
The code below is taken from the gov't website about how to use these assets, **take it with a grain of salt** using `@ imports` in CSS is really slow. Not only does your browser have to parse and excute but it add HTTP requests and those will slow your site down considerably. If you don't mind sluggish loading in development for testing they are fine but once you have the component finalized for production, they should be concatenated into as few files as possible (for both CSS, Javascript and any other assets).

Many if not most of the assets include SASS stylesheets which get compiled into CSS. Ideally, you should work with the SASS stylesheets to save yourself time and let it automatically compile the components you need into a single stylesheet..anything you are not using doesn't need to be in the files for production.

```
/*Core stylesheets*/
  @import url("../bootstrap/3.3.1/css/bootstrap.css");
  @import url("../globalassets/styles/font-awesome/font-awesome.css");
  @import url("../globalassets/styles/adobeBlank/css/adobe-blank.css");
  @import url("../jquery-ui/themes/jquery-ui.css");
/*External library stylesheets*/
  @import url("../chart/1.0.1/css/charts.css");
  @import url("../datatables/1.9.1/css/datatables.css");
  @import url("../datepicker/v6/css/datepicker.css");
  @import url("../jquery.customInput/css/enhanced.css");
  @import url("../jquery.collapsible/1.0/css/collapsible.css");
  @import url("../rwdTable/1.0/css/rwdtable-core.css");
  @import url("../share/4.0/styles/sharewidget-4.0.css");
/*Global Assets stylesheets*/
  @import url("../globalassets/styles/widgets/alerts.css");
  @import url("../globalassets/styles/widgets/buttons.css");
  @import url("../globalassets/styles/widgets/tooltips.css");
  @import url("../globalassets/styles/widgets/carousel.css");
  @import url("../globalassets/styles/widgets/modalstyles.css");
```

###Downside to SASS
Bootstrap now uses SASS (the SCSS syntax, I believe, which I prefer) but if you go an look at the compiled, minified file in `bootstrap-3.3.1/3.3.1/css/bootstrap.min.css` and you do a search for `}}` you'll notice it occurs 71 times. This means that, a few exceptions aside, there are 71 media queries in that file. That is going to be hard on a browser to parse because the rules aren't neatly ordered a placed together in a single media query.

Instead, there are probably 20 media queries for a screen greater than 768px wide. This makes it hard to work on, hard to red a slow to parse and can have unintended consequences because if 2 selctors have the same wight, the later on wins.

There are some CSS settings that are set repeatedly (first they are set in the reset, then they are set by bootstrap, then they might be overridden by a media query and then they might be restlyed by a user tryng to make their specifc use look correct).

That is a bad practice so in the `main.css` file, I consolidated them into a few media queries as were actually unique (and put the print media queires in their own file as discussed above).

There are tools that can autmate this because really, each property should only be set once but I'm not actually putting this into production just demonstrating and explaining here so I didn't mess with it too much.

Here's a list of all the files in this repo:
```
accessibility/accessibility.min.js
backbone/backbone.js
bootstrap-sass-official-3.3.1/_bootstrap.scss
bootstrap-sass-official-3.3.1/affix.js
bootstrap-sass-official-3.3.1/alert.js
bootstrap-sass-official-3.3.1/assets/fonts/bootstrap/
bootstrap-sass-official-3.3.1/assets/fonts/bootstrap/glyphicons-halflings-regular.eot
bootstrap-sass-official-3.3.1/assets/fonts/bootstrap/glyphicons-halflings-regular.svg
bootstrap-sass-official-3.3.1/assets/fonts/bootstrap/glyphicons-halflings-regular.ttf
bootstrap-sass-official-3.3.1/assets/fonts/bootstrap/glyphicons-halflings-regular.woff
bootstrap-sass-official-3.3.1/assets/javascripts/
bootstrap-sass-official-3.3.1/assets/javascripts/bootstrap-sprockets.js
bootstrap-sass-official-3.3.1/assets/javascripts/bootstrap.js
bootstrap-sass-official-3.3.1/assets/javascripts/bootstrap/2015 affix.js
bootstrap-sass-official-3.3.1/assets/javascripts/bootstrap/2015 alert.js
bootstrap-sass-official-3.3.1/assets/javascripts/bootstrap/2015 button.js
bootstrap-sass-official-3.3.1/assets/javascripts/bootstrap/2015 carousel.js
bootstrap-sass-official-3.3.1/assets/javascripts/bootstrap/2015 collapse.js
bootstrap-sass-official-3.3.1/assets/javascripts/bootstrap/2015 dropdown.js
bootstrap-sass-official-3.3.1/assets/javascripts/bootstrap/2015 modal.js
bootstrap-sass-official-3.3.1/assets/javascripts/bootstrap/2015 popover.js
bootstrap-sass-official-3.3.1/assets/javascripts/bootstrap/2015 scrollspy.js
bootstrap-sass-official-3.3.1/assets/javascripts/bootstrap/2015 tab.js
bootstrap-sass-official-3.3.1/assets/javascripts/bootstrap/2015 tooltip.js
bootstrap-sass-official-3.3.1/assets/javascripts/bootstrap/2015 transition.js
bootstrap-sass-official-3.3.1/assets/stylesheets/_bootstrap-compass.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/_bootstrap-mincer.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/_bootstrap-sprockets.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/_bootstrap.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_alerts.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_badges.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_breadcrumbs.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_button-groups.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_buttons.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_carousel.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_close.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_code.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_component-animations.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_dropdowns.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_forms.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_glyphicons.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_grid.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_input-groups.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_jumbotron.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_labels.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_list-group.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_media.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_mixins.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_modals.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_navbar.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_navs.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_normalize.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_pager.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_pagination.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_panels.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_popovers.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_print.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_progress-bars.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_responsive-embed.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_responsive-utilities.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_scaffolding.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_tables.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_theme.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_thumbnails.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_tooltip.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_type.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_utilities.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_variables.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/_wells.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_alerts.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_background-variant.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_border-radius.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_buttons.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_center-block.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_clearfix.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_forms.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_gradients.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_grid-framework.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_grid.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_hide-text.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_image.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_labels.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_list-group.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_nav-divider.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_nav-vertical-align.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_opacity.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_pagination.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_panels.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_progress-bar.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_reset-filter.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_resize.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_responsive-visibility.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_size.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_tab-focus.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_table-row.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_text-emphasis.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_text-overflow.scss
bootstrap-sass-official-3.3.1/assets/stylesheets/bootstrap/mixins/_vendor-prefixes.scss
bootstrap-sass-official-3.3.1/bootstrap.scss
bootstrap-sass-official-3.3.1/bootstrap/_alerts.scss
bootstrap-sass-official-3.3.1/bootstrap/_badges.scss
bootstrap-sass-official-3.3.1/bootstrap/_breadcrumbs.scss
bootstrap-sass-official-3.3.1/bootstrap/_button-groups.scss
bootstrap-sass-official-3.3.1/bootstrap/_buttons.scss
bootstrap-sass-official-3.3.1/bootstrap/_carousel.scss
bootstrap-sass-official-3.3.1/bootstrap/_close.scss
bootstrap-sass-official-3.3.1/bootstrap/_code.scss
bootstrap-sass-official-3.3.1/bootstrap/_component-animations.scss
bootstrap-sass-official-3.3.1/bootstrap/_dropdowns.scss
bootstrap-sass-official-3.3.1/bootstrap/_forms.scss
bootstrap-sass-official-3.3.1/bootstrap/_glyphicons.scss
bootstrap-sass-official-3.3.1/bootstrap/_grid.scss
bootstrap-sass-official-3.3.1/bootstrap/_input-groups.scss
bootstrap-sass-official-3.3.1/bootstrap/_jumbotron.scss
bootstrap-sass-official-3.3.1/bootstrap/_labels.scss
bootstrap-sass-official-3.3.1/bootstrap/_list-group.scss
bootstrap-sass-official-3.3.1/bootstrap/_media.scss
bootstrap-sass-official-3.3.1/bootstrap/_mixins.scss
bootstrap-sass-official-3.3.1/bootstrap/_modals.scss
bootstrap-sass-official-3.3.1/bootstrap/_navbar.scss
bootstrap-sass-official-3.3.1/bootstrap/_navs.scss
bootstrap-sass-official-3.3.1/bootstrap/_normalize.scss
bootstrap-sass-official-3.3.1/bootstrap/_pager.scss
bootstrap-sass-official-3.3.1/bootstrap/_pagination.scss
bootstrap-sass-official-3.3.1/bootstrap/_panels.scss
bootstrap-sass-official-3.3.1/bootstrap/_popovers.scss
bootstrap-sass-official-3.3.1/bootstrap/_print.scss
bootstrap-sass-official-3.3.1/bootstrap/_progress-bars.scss
bootstrap-sass-official-3.3.1/bootstrap/_responsive-embed.scss
bootstrap-sass-official-3.3.1/bootstrap/_responsive-utilities.scss
bootstrap-sass-official-3.3.1/bootstrap/_scaffolding.scss
bootstrap-sass-official-3.3.1/bootstrap/_tables.scss
bootstrap-sass-official-3.3.1/bootstrap/_theme.scss
bootstrap-sass-official-3.3.1/bootstrap/_thumbnails.scss
bootstrap-sass-official-3.3.1/bootstrap/_tooltip.scss
bootstrap-sass-official-3.3.1/bootstrap/_type.scss
bootstrap-sass-official-3.3.1/bootstrap/_utilities.scss
bootstrap-sass-official-3.3.1/bootstrap/_variables.scss
bootstrap-sass-official-3.3.1/bootstrap/_wells.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_alerts.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_background-variant.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_border-radius.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_buttons.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_center-block.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_clearfix.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_forms.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_gradients.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_grid-framework.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_grid.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_hide-text.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_image.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_labels.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_list-group.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_nav-divider.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_nav-vertical-align.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_opacity.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_pagination.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_panels.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_progress-bar.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_reset-filter.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_resize.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_responsive-visibility.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_size.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_tab-focus.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_table-row.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_text-emphasis.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_text-overflow.scss
bootstrap-sass-official-3.3.1/bootstrap/mixins/_vendor-prefixes.scss
bootstrap-sass-official-3.3.1/button.js
bootstrap-sass-official-3.3.1/carousel.js
bootstrap-sass-official-3.3.1/collapse.js
bootstrap-sass-official-3.3.1/dropdown.js
bootstrap-sass-official-3.3.1/glyphicons-halflings-regular.eot
bootstrap-sass-official-3.3.1/glyphicons-halflings-regular.svg
bootstrap-sass-official-3.3.1/glyphicons-halflings-regular.ttf
bootstrap-sass-official-3.3.1/glyphicons-halflings-regular.woff
bootstrap-sass-official-3.3.1/modal.js
bootstrap-sass-official-3.3.1/popover.js
bootstrap-sass-official-3.3.1/scrollspy.js
bootstrap-sass-official-3.3.1/tab.js
bootstrap-sass-official-3.3.1/tooltip.js
bootstrap-sass-official-3.3.1/transition.js
bootstrap-3.3.1/3.3.1/css/bootstrap-theme.css.map
bootstrap-3.3.1/3.3.1/css/bootstrap-theme.min.css
bootstrap-3.3.1/3.3.1/css/bootstrap.css.map
bootstrap-3.3.1/3.3.1/css/bootstrap.min.css
bootstrap-3.3.1/3.3.1/fonts/glyphicons-halflings-regular.eot
bootstrap-3.3.1/3.3.1/fonts/glyphicons-halflings-regular.svg
bootstrap-3.3.1/3.3.1/fonts/glyphicons-halflings-regular.ttf
bootstrap-3.3.1/3.3.1/fonts/glyphicons-halflings-regular.woff
bootstrap-3.3.1/3.3.1/js/bootstrap.min.js
bootstrap-3.3.1/3.3.1/js/npm.js
chart/1.0.1/css/_charts.scss
chart/1.0.1/css/charts.css
chart/1.0.1/js/2015 chart-page.js
chart/1.0.1/js/2015 Chart.js
chart/1.0.1/js/2015 excanvas.js
chart/1.0.1/js/2015 legend.js
datatables-colvis/dataTables.colVis.css
datatables-colvis/dataTables.colVis.js
datatables-tableTools/dataTables.tableTools.css
datatables-tableTools/dataTables.tableTools.js
datatables/1.10.7/css/_jquery.dataTables.scss
datatables/1.10.7/css/jquery.dataTables.min.css
datatables/1.10.7/css/jquery.dataTables_themeroller.css
datatables/1.10.7/js/jquery.dataTables.js
datatables/1.9.1/css/_datatables.scss
datatables/1.9.1/css/datatables.css
datatables/1.9.1/images/back_disabled.jpg
datatables/1.9.1/images/back_disabled.png
datatables/1.9.1/images/back_enabled.png
datatables/1.9.1/images/back_enabled_hover.png
datatables/1.9.1/images/detail_close_gray.png
datatables/1.9.1/images/detail_close_white.png
datatables/1.9.1/images/detail_open_gray.png
datatables/1.9.1/images/detail_open_white.png
datatables/1.9.1/images/details_close.png
datatables/1.9.1/images/details_open.png
datatables/1.9.1/images/forward_disabled.jpg
datatables/1.9.1/images/sort_asc.png
datatables/1.9.1/images/sort_asc_disabled.png
datatables/1.9.1/images/sort_both.png
datatables/1.9.1/images/sort_desc.png
datatables/1.9.1/images/sort_desc_disabled.png
datatables/1.9.1/js/colvis.js
datatables/1.9.1/js/datatable.pagination.js
datatables/1.9.1/js/datatableinitiate.js
datatables/1.9.1/js/datatables.js
datatables/1.9.1/js/tabletools.js
datepicker/v6/css/_datepicker.scss
datepicker/v6/css/datepicker.css
datepicker/v6/js/datepicker-es.js
datepicker/v6/js/datepicker.js
datepicker/v6/js/datepicker2.js
equalize/css/style.css
equalize/js/equalize.min.js
font-awesome/_bordered-pulled.scss
font-awesome/_core.scss
font-awesome/_extras.scss
font-awesome/_fixed-width.scss
font-awesome/_icons.scss
font-awesome/_larger.scss
font-awesome/_list.scss
font-awesome/_mixins.scss
font-awesome/_path.scss
font-awesome/_rotated-flipped.scss
font-awesome/_spinning.scss
font-awesome/_stacked.scss
font-awesome/_variables.scss
font-awesome/font-awesome.css
font-awesome/font-awesome.scss
font-awesome/fontawesome-webfont.eot
font-awesome/fontawesome-webfont.svg
font-awesome/fontawesome-webfont.ttf
font-awesome/fontawesome-webfont.woff
font-awesome/FontAwesome.otf
formatCurrency/1.4.0/js/jquery.formatCurrency.js
globalassets/images/calendar.gif
globalassets/images/checkbox.png
globalassets/images/clear.gif
globalassets/images/datepicker-sprite.png
globalassets/images/icn_select_arrow_1.png
globalassets/images/icon-triangle.png
globalassets/images/jquery_32x32.png
globalassets/images/jqueryui_32x32.png
globalassets/images/radiobutton.png
globalassets/images/share_sprite_compact.png
globalassets/images/sizzlejs_32x32.png
globalassets/images/transparent_1x1.png
globalassets/images/ui-bg_diagonals-thick_18_b81900_40x40.png
globalassets/images/ui-bg_diagonals-thick_20_666666_40x40.png
globalassets/images/ui-bg_flat_0_aaaaaa_40x100.png
globalassets/images/ui-bg_flat_10_000000_40x100.png
globalassets/images/ui-bg_flat_75_f3f8fc_40x100.png
globalassets/images/ui-bg_flat_75_ffffff_40x100.png
globalassets/images/ui-bg_glass_100_f6f6f6_1x400.png
globalassets/images/ui-bg_glass_100_fdf5ce_1x400.png
globalassets/images/ui-bg_glass_55_fbf9ee_1x400.png
globalassets/images/ui-bg_glass_65_ffffff_1x400.png
globalassets/images/ui-bg_glass_75_c67205_1x400.png
globalassets/images/ui-bg_glass_75_dadada_1x400.png
globalassets/images/ui-bg_glass_75_e6e6e6_1x400.png
globalassets/images/ui-bg_glass_95_fef1ec_1x400.png
globalassets/images/ui-bg_gloss-wave_35_f6a828_500x100.png
globalassets/images/ui-bg_highlight-soft_100_eeeeee_1x100.png
globalassets/images/ui-bg_highlight-soft_75_629ab9_1x100.png
globalassets/images/ui-bg_highlight-soft_75_cccccc_1x100.png
globalassets/images/ui-bg_highlight-soft_75_ffe45c_1x100.png
globalassets/images/ui-bg_inset-soft_95_fef1ec_1x100.png
globalassets/images/ui-icons_222222_256x240.png
globalassets/images/ui-icons_228ef1_256x240.png
globalassets/images/ui-icons_2e83ff_256x240.png
globalassets/images/ui-icons_454545_256x240.png
globalassets/images/ui-icons_888888_256x240.png
globalassets/images/ui-icons_cd0a0a_256x240.png
globalassets/images/ui-icons_darkGray.png
globalassets/images/ui-icons_default.png
globalassets/images/ui-icons_defaultInactive.png
globalassets/images/ui-icons_ef8c08_256x240.png
globalassets/images/ui-icons_ffd27a_256x240.png
globalassets/images/ui-icons_ffffff_256x240.png
globalassets/images/ui-icons_focus.png
globalassets/images/ui-icons_highlight.png
globalassets/images/ui-icons_white.png
globalassets/scripts/assets.core.js
globalassets/scripts/assets.responsive.js
globalassets/scripts/assets.tracking.js
globalassets/scripts/breadcrumb.js
globalassets/scripts/hc-detect.js
globalassets/styles/_custom-input.scss
globalassets/styles/_custom-mixins.scss
globalassets/styles/_helpers.scss
globalassets/styles/adobeBlank/adobe-blank.css
globalassets/styles/adobeBlank/adobe-blank.scss
globalassets/styles/adobeBlank/font/AdobeBlank.b64.txt
globalassets/styles/adobeBlank/font/AdobeBlank.eot
globalassets/styles/adobeBlank/font/AdobeBlank.otf
globalassets/styles/adobeBlank/font/AdobeBlank.otf.woff
globalassets/styles/adobeBlank/font/AdobeBlank.ttf
globalassets/styles/adobeBlank/font/AdobeBlank.ttf.woff
globalassets/styles/breadcrumb.scss
globalassets/styles/fonts/AdobeBlank.eot
globalassets/styles/fonts/AdobeBlank.otf.woff
globalassets/styles/fonts/AdobeBlank.ttf
globalassets/styles/fonts/AdobeBlank.ttf.woff
globalassets/styles/formvalidator-3.0.css
globalassets/styles/global-template.scss
globalassets/styles/jquery-ui.css
globalassets/styles/jquery-ui.scss
globalassets/styles/reset-3.0.css
globalassets/styles/widgets/_accordion.scss
globalassets/styles/widgets/_alerts.scss
globalassets/styles/widgets/_buttons.scss
globalassets/styles/widgets/_carousel.scss
globalassets/styles/widgets/_collapsible.scss
globalassets/styles/widgets/_datepicker.scss
globalassets/styles/widgets/_dialog.scss
globalassets/styles/widgets/_dropdowns.scss
globalassets/styles/widgets/_equalize.scss
globalassets/styles/widgets/_errors.scss
globalassets/styles/widgets/_expand-collapse.scss
globalassets/styles/widgets/_form.scss
globalassets/styles/widgets/_inputs.scss
globalassets/styles/widgets/_progressbar.scss
globalassets/styles/widgets/_slider.scss
globalassets/styles/widgets/_tabs.scss
globalassets/styles/widgets/_tooltips.scss
globalassets/styles/widgets/alerts.css
globalassets/styles/widgets/buttons.css
globalassets/styles/widgets/carousel.css
globalassets/styles/widgets/collapsible.css
globalassets/styles/widgets/dropdowns.css
globalassets/styles/widgets/modal-styles.css
globalassets/styles/widgets/modalstyles.css
globalassets/styles/widgets/tooltips.css
index.html
jpanelmenu/jquery.jpanelmenu.min.js
jquery-cookie/jquery.cookie.js
jquery-ui/AUTHORS.txt
jquery-ui/component.json
jquery-ui/composer.json
jquery-ui/jquery-ui.js
jquery-ui/MIT-LICENSE.txt
jquery-ui/package.json
jquery-ui/themes/base/_jquery-ui.scss
jquery-ui/themes/base/images/animated-overlay.gif
jquery-ui/themes/base/images/ui-bg_flat_0_aaaaaa_40x100.png
jquery-ui/themes/base/images/ui-bg_flat_75_ffffff_40x100.png
jquery-ui/themes/base/images/ui-bg_glass_55_fbf9ee_1x400.png
jquery-ui/themes/base/images/ui-bg_glass_65_ffffff_1x400.png
jquery-ui/themes/base/images/ui-bg_glass_75_dadada_1x400.png
jquery-ui/themes/base/images/ui-bg_glass_75_e6e6e6_1x400.png
jquery-ui/themes/base/images/ui-bg_glass_95_fef1ec_1x400.png
jquery-ui/themes/base/images/ui-bg_highlight-soft_75_cccccc_1x100.png
jquery-ui/themes/base/images/ui-icons_222222_256x240.png
jquery-ui/themes/base/images/ui-icons_2e83ff_256x240.png
jquery-ui/themes/base/images/ui-icons_454545_256x240.png
jquery-ui/themes/base/images/ui-icons_888888_256x240.png
jquery-ui/themes/base/images/ui-icons_cd0a0a_256x240.png
jquery-ui/themes/base/jquery-ui.css
jquery-ui/themes/base/jquery.ui.accordion.css
jquery-ui/themes/base/jquery.ui.all.css
jquery-ui/themes/base/jquery.ui.autocomplete.css
jquery-ui/themes/base/jquery.ui.base.css
jquery-ui/themes/base/jquery.ui.button.css
jquery-ui/themes/base/jquery.ui.core.css
jquery-ui/themes/base/jquery.ui.datepicker.css
jquery-ui/themes/base/jquery.ui.dialog.css
jquery-ui/themes/base/jquery.ui.menu.css
jquery-ui/themes/base/jquery.ui.progressbar.css
jquery-ui/themes/base/jquery.ui.resizable.css
jquery-ui/themes/base/jquery.ui.selectable.css
jquery-ui/themes/base/jquery.ui.slider.css
jquery-ui/themes/base/jquery.ui.spinner.css
jquery-ui/themes/base/jquery.ui.tabs.css
jquery-ui/themes/base/jquery.ui.theme.css
jquery-ui/themes/base/jquery.ui.tooltip.css
jquery-ui/themes/base/minified/images/animated-overlay.gif
jquery-ui/themes/base/minified/images/ui-bg_flat_0_aaaaaa_40x100.png
jquery-ui/themes/base/minified/images/ui-bg_flat_75_ffffff_40x100.png
jquery-ui/themes/base/minified/images/ui-bg_glass_55_fbf9ee_1x400.png
jquery-ui/themes/base/minified/images/ui-bg_glass_65_ffffff_1x400.png
jquery-ui/themes/base/minified/images/ui-bg_glass_75_dadada_1x400.png
jquery-ui/themes/base/minified/images/ui-bg_glass_75_e6e6e6_1x400.png
jquery-ui/themes/base/minified/images/ui-bg_glass_95_fef1ec_1x400.png
jquery-ui/themes/base/minified/images/ui-bg_highlight-soft_75_cccccc_1x100.png
jquery-ui/themes/base/minified/images/ui-icons_222222_256x240.png
jquery-ui/themes/base/minified/images/ui-icons_2e83ff_256x240.png
jquery-ui/themes/base/minified/images/ui-icons_454545_256x240.png
jquery-ui/themes/base/minified/images/ui-icons_888888_256x240.png
jquery-ui/themes/base/minified/images/ui-icons_cd0a0a_256x240.png
jquery-ui/themes/base/minified/jquery-ui.min.css
jquery-ui/themes/base/minified/jquery.ui.accordion.min.css
jquery-ui/themes/base/minified/jquery.ui.autocomplete.min.css
jquery-ui/themes/base/minified/jquery.ui.button.min.css
jquery-ui/themes/base/minified/jquery.ui.core.min.css
jquery-ui/themes/base/minified/jquery.ui.datepicker.min.css
jquery-ui/themes/base/minified/jquery.ui.dialog.min.css
jquery-ui/themes/base/minified/jquery.ui.menu.min.css
jquery-ui/themes/base/minified/jquery.ui.progressbar.min.css
jquery-ui/themes/base/minified/jquery.ui.resizable.min.css
jquery-ui/themes/base/minified/jquery.ui.selectable.min.css
jquery-ui/themes/base/minified/jquery.ui.slider.min.css
jquery-ui/themes/base/minified/jquery.ui.spinner.min.css
jquery-ui/themes/base/minified/jquery.ui.tabs.min.css
jquery-ui/themes/base/minified/jquery.ui.theme.min.css
jquery-ui/themes/base/minified/jquery.ui.tooltip.min.css
jquery-ui/ui/.jshintrc
jquery-ui/ui/i18n/jquery-ui-i18n.js
jquery-ui/ui/i18n/jquery.ui.datepicker-af.js
jquery-ui/ui/i18n/jquery.ui.datepicker-ar-DZ.js
jquery-ui/ui/i18n/jquery.ui.datepicker-ar.js
jquery-ui/ui/i18n/jquery.ui.datepicker-az.js
jquery-ui/ui/i18n/jquery.ui.datepicker-be.js
jquery-ui/ui/i18n/jquery.ui.datepicker-bg.js
jquery-ui/ui/i18n/jquery.ui.datepicker-bs.js
jquery-ui/ui/i18n/jquery.ui.datepicker-ca.js
jquery-ui/ui/i18n/jquery.ui.datepicker-cs.js
jquery-ui/ui/i18n/jquery.ui.datepicker-cy-GB.js
jquery-ui/ui/i18n/jquery.ui.datepicker-da.js
jquery-ui/ui/i18n/jquery.ui.datepicker-de.js
jquery-ui/ui/i18n/jquery.ui.datepicker-el.js
jquery-ui/ui/i18n/jquery.ui.datepicker-en-AU.js
jquery-ui/ui/i18n/jquery.ui.datepicker-en-GB.js
jquery-ui/ui/i18n/jquery.ui.datepicker-en-NZ.js
jquery-ui/ui/i18n/jquery.ui.datepicker-eo.js
jquery-ui/ui/i18n/jquery.ui.datepicker-es.js
jquery-ui/ui/i18n/jquery.ui.datepicker-et.js
jquery-ui/ui/i18n/jquery.ui.datepicker-eu.js
jquery-ui/ui/i18n/jquery.ui.datepicker-fa.js
jquery-ui/ui/i18n/jquery.ui.datepicker-fi.js
jquery-ui/ui/i18n/jquery.ui.datepicker-fo.js
jquery-ui/ui/i18n/jquery.ui.datepicker-fr-CA.js
jquery-ui/ui/i18n/jquery.ui.datepicker-fr-CH.js
jquery-ui/ui/i18n/jquery.ui.datepicker-fr.js
jquery-ui/ui/i18n/jquery.ui.datepicker-gl.js
jquery-ui/ui/i18n/jquery.ui.datepicker-he.js
jquery-ui/ui/i18n/jquery.ui.datepicker-hi.js
jquery-ui/ui/i18n/jquery.ui.datepicker-hr.js
jquery-ui/ui/i18n/jquery.ui.datepicker-hu.js
jquery-ui/ui/i18n/jquery.ui.datepicker-hy.js
jquery-ui/ui/i18n/jquery.ui.datepicker-id.js
jquery-ui/ui/i18n/jquery.ui.datepicker-is.js
jquery-ui/ui/i18n/jquery.ui.datepicker-it.js
jquery-ui/ui/i18n/jquery.ui.datepicker-ja.js
jquery-ui/ui/i18n/jquery.ui.datepicker-ka.js
jquery-ui/ui/i18n/jquery.ui.datepicker-kk.js
jquery-ui/ui/i18n/jquery.ui.datepicker-km.js
jquery-ui/ui/i18n/jquery.ui.datepicker-ko.js
jquery-ui/ui/i18n/jquery.ui.datepicker-ky.js
jquery-ui/ui/i18n/jquery.ui.datepicker-lb.js
jquery-ui/ui/i18n/jquery.ui.datepicker-lt.js
jquery-ui/ui/i18n/jquery.ui.datepicker-lv.js
jquery-ui/ui/i18n/jquery.ui.datepicker-mk.js
jquery-ui/ui/i18n/jquery.ui.datepicker-ml.js
jquery-ui/ui/i18n/jquery.ui.datepicker-ms.js
jquery-ui/ui/i18n/jquery.ui.datepicker-nb.js
jquery-ui/ui/i18n/jquery.ui.datepicker-nl-BE.js
jquery-ui/ui/i18n/jquery.ui.datepicker-nl.js
jquery-ui/ui/i18n/jquery.ui.datepicker-nn.js
jquery-ui/ui/i18n/jquery.ui.datepicker-no.js
jquery-ui/ui/i18n/jquery.ui.datepicker-pl.js
jquery-ui/ui/i18n/jquery.ui.datepicker-pt-BR.js
jquery-ui/ui/i18n/jquery.ui.datepicker-pt.js
jquery-ui/ui/i18n/jquery.ui.datepicker-rm.js
jquery-ui/ui/i18n/jquery.ui.datepicker-ro.js
jquery-ui/ui/i18n/jquery.ui.datepicker-ru.js
jquery-ui/ui/i18n/jquery.ui.datepicker-sk.js
jquery-ui/ui/i18n/jquery.ui.datepicker-sl.js
jquery-ui/ui/i18n/jquery.ui.datepicker-sq.js
jquery-ui/ui/i18n/jquery.ui.datepicker-sr-SR.js
jquery-ui/ui/i18n/jquery.ui.datepicker-sr.js
jquery-ui/ui/i18n/jquery.ui.datepicker-sv.js
jquery-ui/ui/i18n/jquery.ui.datepicker-ta.js
jquery-ui/ui/i18n/jquery.ui.datepicker-th.js
jquery-ui/ui/i18n/jquery.ui.datepicker-tj.js
jquery-ui/ui/i18n/jquery.ui.datepicker-tr.js
jquery-ui/ui/i18n/jquery.ui.datepicker-uk.js
jquery-ui/ui/i18n/jquery.ui.datepicker-vi.js
jquery-ui/ui/i18n/jquery.ui.datepicker-zh-CN.js
jquery-ui/ui/i18n/jquery.ui.datepicker-zh-HK.js
jquery-ui/ui/i18n/jquery.ui.datepicker-zh-TW.js
jquery-ui/ui/jquery-ui.custom.js
jquery-ui/ui/jquery-ui.js
jquery-ui/ui/jquery.ui.accordion.js
jquery-ui/ui/jquery.ui.autocomplete.js
jquery-ui/ui/jquery.ui.button.js
jquery-ui/ui/jquery.ui.core.js
jquery-ui/ui/jquery.ui.datepicker.js
jquery-ui/ui/jquery.ui.dialog.js
jquery-ui/ui/jquery.ui.draggable.js
jquery-ui/ui/jquery.ui.droppable.js
jquery-ui/ui/jquery.ui.effect-blind.js
jquery-ui/ui/jquery.ui.effect-bounce.js
jquery-ui/ui/jquery.ui.effect-clip.js
jquery-ui/ui/jquery.ui.effect-drop.js
jquery-ui/ui/jquery.ui.effect-explode.js
jquery-ui/ui/jquery.ui.effect-fade.js
jquery-ui/ui/jquery.ui.effect-fold.js
jquery-ui/ui/jquery.ui.effect-highlight.js
jquery-ui/ui/jquery.ui.effect-pulsate.js
jquery-ui/ui/jquery.ui.effect-scale.js
jquery-ui/ui/jquery.ui.effect-shake.js
jquery-ui/ui/jquery.ui.effect-slide.js
jquery-ui/ui/jquery.ui.effect-transfer.js
jquery-ui/ui/jquery.ui.effect.js
jquery-ui/ui/jquery.ui.menu.js
jquery-ui/ui/jquery.ui.mouse.js
jquery-ui/ui/jquery.ui.position.js
jquery-ui/ui/jquery.ui.progressbar.js
jquery-ui/ui/jquery.ui.resizable.js
jquery-ui/ui/jquery.ui.selectable.js
jquery-ui/ui/jquery.ui.slider.js
jquery-ui/ui/jquery.ui.sortable.js
jquery-ui/ui/jquery.ui.spinner.js
jquery-ui/ui/jquery.ui.tabs.js
jquery-ui/ui/jquery.ui.tooltip.js
jquery-ui/ui/jquery.ui.widget.js
jquery-ui/ui/minified/i18n/jquery-ui-i18n.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-af.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-ar-DZ.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-ar.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-az.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-be.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-bg.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-bs.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-ca.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-cs.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-cy-GB.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-da.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-de.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-el.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-en-AU.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-en-GB.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-en-NZ.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-eo.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-es.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-et.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-eu.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-fa.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-fi.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-fo.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-fr-CA.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-fr-CH.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-fr.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-gl.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-he.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-hi.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-hr.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-hu.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-hy.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-id.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-is.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-it.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-ja.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-ka.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-kk.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-km.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-ko.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-ky.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-lb.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-lt.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-lv.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-mk.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-ml.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-ms.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-nb.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-nl-BE.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-nl.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-nn.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-no.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-pl.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-pt-BR.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-pt.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-rm.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-ro.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-ru.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-sk.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-sl.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-sq.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-sr-SR.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-sr.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-sv.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-ta.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-th.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-tj.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-tr.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-uk.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-vi.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-zh-CN.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-zh-HK.min.js
jquery-ui/ui/minified/i18n/jquery.ui.datepicker-zh-TW.min.js
jquery-ui/ui/minified/jquery-ui.custom.min.js
jquery-ui/ui/minified/jquery-ui.min.js
jquery-ui/ui/minified/jquery.ui.accordion.min.js
jquery-ui/ui/minified/jquery.ui.autocomplete.min.js
jquery-ui/ui/minified/jquery.ui.button.min.js
jquery-ui/ui/minified/jquery.ui.core.min.js
jquery-ui/ui/minified/jquery.ui.datepicker.min.js
jquery-ui/ui/minified/jquery.ui.dialog.min.js
jquery-ui/ui/minified/jquery.ui.draggable.min.js
jquery-ui/ui/minified/jquery.ui.droppable.min.js
jquery-ui/ui/minified/jquery.ui.effect-blind.min.js
jquery-ui/ui/minified/jquery.ui.effect-bounce.min.js
jquery-ui/ui/minified/jquery.ui.effect-clip.min.js
jquery-ui/ui/minified/jquery.ui.effect-drop.min.js
jquery-ui/ui/minified/jquery.ui.effect-explode.min.js
jquery-ui/ui/minified/jquery.ui.effect-fade.min.js
jquery-ui/ui/minified/jquery.ui.effect-fold.min.js
jquery-ui/ui/minified/jquery.ui.effect-highlight.min.js
jquery-ui/ui/minified/jquery.ui.effect-pulsate.min.js
jquery-ui/ui/minified/jquery.ui.effect-scale.min.js
jquery-ui/ui/minified/jquery.ui.effect-shake.min.js
jquery-ui/ui/minified/jquery.ui.effect-slide.min.js
jquery-ui/ui/minified/jquery.ui.effect-transfer.min.js
jquery-ui/ui/minified/jquery.ui.effect.min.js
jquery-ui/ui/minified/jquery.ui.menu.min.js
jquery-ui/ui/minified/jquery.ui.mouse.min.js
jquery-ui/ui/minified/jquery.ui.position.min.js
jquery-ui/ui/minified/jquery.ui.progressbar.min.js
jquery-ui/ui/minified/jquery.ui.resizable.min.js
jquery-ui/ui/minified/jquery.ui.selectable.min.js
jquery-ui/ui/minified/jquery.ui.slider.min.js
jquery-ui/ui/minified/jquery.ui.sortable.min.js
jquery-ui/ui/minified/jquery.ui.spinner.min.js
jquery-ui/ui/minified/jquery.ui.tabs.min.js
jquery-ui/ui/minified/jquery.ui.tooltip.min.js
jquery-ui/ui/minified/jquery.ui.widget.min.js
jquery.autotab/js/1.8.1/jquery-autotab.js
jquery.blockui/js/jQuery.blockUI.js
jquery.collapsible/1.0/css/_collapsible.scss
jquery.collapsible/1.0/css/collapsible.css
jquery.collapsible/1.0/js/jQuery.collapsible.js
jquery.customInput/css/_enhanced.scss
jquery.customInput/css/basic.css
jquery.customInput/css/enhanced.css
jquery.customInput/images/button-green.gif
jquery.customInput/images/checkbox.gif
jquery.customInput/images/radiobutton.gif
jquery.customInput/js/jQuery.customInput.js
jquery.customInput/license.txt
jquery.validation-1.13.1additional-methods.min.js
jquery.validation-1.13.1/jquery.validate.min.js
jquery-1.11.3/jquery.js
jquery-1.11.3/jquery.min.map
jquery-3.4.1/jquery-3.4.1.min.js
lodash-2.4.1/lodash.compat.js
lodash-2.4.1/lodash.js
maskedInput/3.1.3/css/jasny-bootstrap.css
maskedInput/3.1.3/css/jasny-bootstrap.css.map
maskedInput/3.1.3/css/jasny-bootstrap.min.css
maskedInput/3.1.3/js/jasny-bootstrap.min.js
modernizr-2.7.2/modernizr.js
README.md
requirejs-plugins/async.js
requirejs-plugins/async/async.js
requirejs-plugins/depend.js
requirejs-plugins/font.js
requirejs-plugins/goog.js
requirejs-plugins/image.js
requirejs-plugins/json.js
requirejs-plugins/Markdown.Converter.js
requirejs-plugins/mdown.js
requirejs-plugins/noext.js
requirejs-plugins/propertyParser.js
requirejs-plugins/text.js
requirejs-text/LICENSE
requirejs-text/package.json
requirejs-text/text.js
requirejs/require.js
respond-1.4.0/respond.src.js
rwdTable/1.0/css/_rwdtable-core.scss
rwdTable/1.0/css/rwdtable-core.css
rwdTable/1.0/js/rwdtable.js
share/4.0/images/icons/share_sprite_compact.png
share/4.0/js/sharewidget-4.0.js
share/4.0/styles/sharewidget-4.0.css
share/4.0/styles/sharewidget-4.0.scss
sinonjs-1.8.3/sinon.js
styles/main.css
underscore-1.7.0/underscore.js
```
