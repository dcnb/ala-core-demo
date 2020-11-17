## timelinejs.json

Download and open the the repository on my computer

### Go to home-infographic.html

change locations card to item_information
Move description
delete subjects

More on bootstrap grid --> https://getbootstrap.com/docs/4.5/layout/grid/


### Add timlinejs

add timelinejs feature


```
  <div class="col-md-12">
    
    {% include feature/timelinejs.html %}

  </div>
```


Info: https://collectionbuilder.github.io/docs/advanced.html#timelinejs


`{%- assign items = site.data[site.metadata] | where: "format","image/jpg" -%}`

- The above will only include items that are of the format "image/jpg" 

`{%- assign items = site.data[site.metadata] | where_exp: "item","item.date-is-approximate? == nil" -%}`
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


### Switch collection to Waktins

Get rid of timeline. Change subject to mine. 

Go to timeline layout. 
Change top "date" to "depth" 

`{%- assign inYear = items | where_exp: 'item', 'item.depth contains year' -%}`

then change them both to 'mine'




