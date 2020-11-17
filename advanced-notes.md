# Advanced Topics Step Thru

Download and open the the repository on my computer

## Edit home-infographic.html

- Maybe locations aren't that important to this collection, but you think material type is. Let's change the locations card on the front page to feature item_information

- Let's move the description into the narrow row. 

- Now let's delete the subjects card. 

Here's how that should look:

```
 ---
layout: page
---
{%- assign items = site.data[site.metadata] -%}
<div class="row">
  <div class="col-md-8">

    {% include index/carousel.html %}
  
  </div>
  <div class="col-md-4">  
    {% include index/description.html %}

    {% include index/time.html %}

    {% include index/featured-terms.html field="item_information" title="Material Type" btn-color="outline-secondary"  max=6 %}

    {% include index/objects.html %}

  </div>
  <div class="col-md-12">

    {% include index/data-download.html %}

  </div>
</div>
```

The home-infographic layout, and all pages in CollectionBuilder are styled using bootstrap, particularly using its grid feature. More on bootstrap grid --> https://getbootstrap.com/docs/4.5/layout/grid/

## Add TimlineJS Feature

To add the timeline feature to the front page, you'll need to edit "home-infographic.html" in the _layouts directory by adding the below: 

```
  <div class="col-md-12">
    
    {% include feature/timelinejs.html %}

  </div>
```

This creates a full width div that will include the code to display a timelinejs feature.

More detail on this is on our documentation page: [https://collectionbuilder.github.io/docs/advanced.html#timelinejs](https://collectionbuilder.github.io/docs/advanced.html#timelinejs)

Look at the JSON object and see how various pieces are being constructed. 

## Curate TimelineJS Items

You probably don't want the whole collection included, so you'll want to edit that. To do that, you need to edit the **timelinejs.json** file in the **/assets/data/** folder. 

At the top, you can adjust what's included by defining which items are included in the array that is generating the json object below. To do that, you'll use "where" expressions, which are part of the liquid filtering mechanism in jekyll. 

There are two types of where expresions. [The Basic "where:"](https://shopify.github.io/liquid/filters/where/) and [the more powerful where_exp](https://jekyllrb.com/docs/liquid/filters/#where-expression)

(These are liquid expressions. You can see a cheatsheet of those here: https://learn.cloudcannon.com/jekyll-cheat-sheet/)

### Filter timeline objects using where and where_exp

So let's do a few ... 

`{%- assign items = site.data[site.metadata] | where: "format","image/jpg" -%}`

- The above will only include items that are of the format "image/jpg" 

`{%- assign items = site.data[site.metadata] | where_exp: "item","item.date-is-approximate? == nil" -%}`

(is the same as)

`{%- assign items = site.data[site.metadata] | where_exp: "item","item.date-is-approximate? != "yes" -%}`

- Both the above do the same thing --> They only include items whose `date-is-approximate?` field is empty (or doesn't say "yes")

`{%- assign items = site.data[site.metadata] | where_exp: "item","item.subject contains 'God'" -%}`

- This will only include items that have "God" in the subject field (as part of any subject)

See the advanced topic documentation for other ways to include timelines, including in place of timeline page and as an additional page. 

operators: <https://shopify.github.io/liquid/basics/operators/>

| == | equals
| != does not equal
| > | greater than
| < | less than
| >= | greater than or equal to
| <= | less than or equal to
| or | logical or
| and | logical and


## Move to Watkins Collection

1. Shut down server. 
2. Download [Watkins metadata](https://docs.google.com/spreadsheets/u/1/d/1mThECwBYaUdvUrSbc9d2wbjedpYyvVD89jJ15R-7Qmo/edit?usp=sharing). 
3. Change _config.yml metadata variable to watkins. 

All items in this collection are from 1890, and we don't have months, so .... 

The timeline isn't very helpful. 

Let's get rid of it, and fix up our timeline page so that it's different. 

- Remove Timeline feature from home-infographic.html

Go to timeline layout. 
Change top "date" to "depth" 

`{%- assign inYear = items | where_exp: 'item', 'item.depth contains year' -%}`

Change **_data/config-nav.csv** and **/pages/timeline.md** so that they are titled differently and they go to a different url. You can still keep the file named timeline.md, or you can change it. 

You'll want the page to look like this: 

```
---
title: Depth
layout: timeline
permalink: /depth.html
# a timeline visualization will be added below the content in this file
---

## Collection Depth
```

### Really annoying issue with sorting 3- and 4- digit numbers

Obviously we have some issues here. 1000ft is not getting sorted correctly. This is a issue with a lot of programming languages, and also an issue with dates, if they're long enough. 

So, a couple ways to adjust this. 

1. Change the metadata so that all are four digit numbers. So "100" would become "0100" -- this is obviously not idea. 
2. Adjust the numbers in the processing at the top to make sure they are all four digitis. 

*NOTE: This is getting really deep, so if you don't want to edit this, that's fine*

`{%- capture clean-years -%}{% for date in raw-dates %}{% if date contains "000" %}{{ date | strip | split: "-" | first }}{% else %}{{ date | strip | prepend: "0"}}{% endif %}{% unless forloop.last %};{% endunless %}{%- endfor -%}{%- endcapture -%}`

Once you do this, you'll see that nothing shows up. However, if you switch the where_exp to where the year contains the item.depth, it should work. 

`{%- assign inYear = items | where_exp: 'item', 'year contains item.depth' -%}`

Then, you'll likely want to adjust the display below so that the four digit numbers that start with a zero are only displayed as the three digit number they originated as. 

{% if year contains '000'%}{{year}}ft{% else %}{{ year | slice: 1,3 }}ft{% endif %}

You'll also need to adjust the dropdown so that it does the same thing for display. 

Finally, say you wanted to do something really cool and make it look like as you were going down the page, you were also going down in depth ... 

This is where you can [use the cycle command](https://shopify.github.io/liquid/tags/iteration/#cycle) like such

`<tr id="{{ year }}" style='background-color: {% cycle "#FFFFFF","#E7E6DD","#B9B8B1","#94938E","#767672","#5E5E5B","#4B4B49","#3C3C3A","#30302E","#262625","#1E1E1E","Black"%}''>`

## Change Timeline Visualization to Include Words Rather than Numbers

What if you don't want to do depth, or you have something else ... 

Change `depth` to `mine`

Nothing's showing up, and you can see why -- the former processing code got rid of the space between the words. Let's find that part and remove it. 

This

`{%- assign uniqueYears = clean-years | remove: " " | split: ";" | uniq | sort -%}`

becomes

`{%- assign uniqueYears = clean-years  | split: ";" | uniq | sort -%}`

And it should work!






