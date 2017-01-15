# Page layout with a fixed header and footer

## Introduction to Flexbox
The Flexible Box, or as it is commonly called flexbox, was added to CSS3 as an alternative to the classical block model. Flexbox was created to provide predictive behavior of page elements for different screen sizes and different display devices of the displayed page. Since it doesn’t use floats and its margins don’t collapse with the contents’ margins, flexbox often provides a much simpler model over the standard block model.
Many designers will find the flexbox model easier to use. Child elements in a flexbox can be laid out in any direction and can have flexible dimensions to adapt to the display space. Positioning child elements is thus much easier, and complex layouts can be achieved more simply and with cleaner code, as the display order of the elements is independent of their order in the source code. This independence intentionally affects only the visual rendering, leaving speech order and navigation based on the source order.

## What we'll build
Today we’ll see a cool technique for creating a web layout with fixed header and footer. The height of the header and footer adjusts to their inner content, and the middle section takes the rest of the screen, showing a scrollbar if its content needs more height.

For example, our layout can be used to keep the page’s name, menu or breadcrumbs always visible on the header, and Save/Cancel buttons on the footer.

[Demo](https://omriallouche.github.io/asana-style-loader/)

I’ve seen this layout implemented with a fixed position and set heights for the header and footer. The problems with this approach start when the height of either the header or the footer change, for example because of dynamic data, or different screen resolutions. Since the approach hard-codes the height, it doesn’t adapt dynamically to the content it holds.

## Adding some Flexbox magic
Luckily for us, there’s a much simpler approach that solves the problem, using Flexbox, and [works in all major browsers](http://caniuse.com/#feat=flexbox).
The basic concept is to make the header, middle and footer sections use a flexbox layout. The height of the header and footer sections will adjust automatically to their content, as is the standard behavior for any div.
We first create a simple HTML layout - a parent div holding 3 children - for the top, middle and bottom sections.
```html
  <div class="sections-container">
    <div class="section-top"></div>
    <div class="section-middle"></div>
    <div class="section-bottom"></div>
  </div>
```


We now set the wrapper for the 3 sections to display using flexbox, taking the full height of the viewport:

```css
.sections-container {
    display: flex;
    flex-direction: column;
    height: 100vh;
}
```
When the content of the middle section doesn’t fill the remaining space, we’d like to make sure that the middle section expands while the header and footer stay at the size of their content. We therefore set their flex-grow to 0:

```css
.section-top, .panel-bottom {
    flex-shrink: 0;
}
```
When the content of the middle section overflow the remaining space, we’d like to make sure that the header and footer keep their size, and the middle section shows a vertical scrollbar. To do this, we set the flex-shrink of the header and footer to 0:

## Adding a shadow to indicate additional content
Let’s now add a bit of shadow, to visually indicate when there’s more content available above/below what’s currently displayed on screen.
We create a combination of a linear gradient and radial gradient for the shadow effect. We make sure it doesn’t repeat so it is shown only on the top and/or bottom sides of the middle section.
We also set the shadow to start 40 pixels from each side, so there’s no shadow when the user reaches to top/bottom of the middle section.

```css
.section-middle {
    overflow: overlay;
    overflow-x: hidden;
    background: linear-gradient(#fff 30%, rgba(255,255,255,0)),linear-gradient(rgba(255,255,255,0), #fff 70%) 0 100%,radial-gradient(farthest-side at 50% 0, rgba(43,43,43,0.25),rgba(43,43,43,0)),radial-gradient(farthest-side at 50% 100%, rgba(43,43,43,0.2),rgba(43,43,43,0)) 0 100%;
    background-repeat: no-repeat;
    background-color: white;
    background-size: 100% 40px, 100% 40px, 100% 14px, 100% 14px;
    background-attachment: local, local, scroll, scroll;
}
```

## Styling the Scrollbar
In Chrome, we can also style the vertical scrollbar. Let’s change the styling a bit:

```css
.section-middle::-webkit-scrollbar
{
	 width: 4px;
	 background: rgb(243,243,243);
}

.section-middle::-webkit-scrollbar-thumb
{
    background: rgb(150,150,150);
}
```

## That’s all folks!
As we’ve just seen, flexbox can sometimes magically give elegant solutions to complex problems.

Here's our [final solution](https://omriallouche.github.io/asana-style-loader/) again, and a [Plunker](https://plnkr.co/edit/Imh0DC?p=preview) for your convenience.