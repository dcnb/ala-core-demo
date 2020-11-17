## timelinejs.json

### Go to home-infographic.html

change locations to item_information
delete carousel

### Add timlinejs

Go to: https://collectionbuilder.github.io/docs/advanced.html#timelinejs

{%- assign items = site.data[site.metadata] | where: "format","image/jpg" -%}


{%- assign items = site.data[site.metadata] | where_exp: "item","item.date-is-approximate? == nil" -%}
{%- assign items = site.data[site.metadata] | where_exp: "item","item.date-is-approximate? != "yes" -%}


{%- assign items = site.data[site.metadata] | where_exp: "item","item.subject contains 'God'" -%}


operators: https://shopify.github.io/liquid/basics/operators/

==	equals
!= does not equal
>	greater than
<	less than
>=	greater than or equal to
<=	less than or equal to
or	logical or
and	logical and


### Go to Watkins open in VS Code

Get rid of timeline. Change subject to mine. 

Go to timeline layout. 
Change top "date" to "depth" 

{%- assign inYear = items | where_exp: 'item', 'item.depth contains year' -%}




