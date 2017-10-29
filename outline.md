# Story

## Let's build a search interface

Start with a search box and results
- Easy, when the query changes, run a search
Add some things:
- range filter
- date filter
- top 10 vendors list (pre-facet)
- spending over time (chart)
- results paging/sorting
- more button
- checkboxes in top vendors...
- second, third, fourth... check box list (agency type, etc)
- include/exclude button
- find box on facets
- tons of charts on page now
...

/meme Simpsons Dr. Nick "The knee bone's connected to the... something.
    The something's connected to the red thing.
    The red thing's connected to my wristwatch. Uh oh."

Still not done yet...
- multiple tabs... not visible contexts need to not run 
- and btw, dynamically added filters
- dynamic charts - now people want a more button on "top n vendors by dollar amount".....
- dynamically changeable filter TYPES.
- and now cascading/subquery ('customers of')
-
...


/meme STOP THE MADNESS


## Enter the DataContext.

### let's start over

There are these magic contexts of data
- They have these contextually updated properties, dependant on everyone else
- They can have bits of data that everyone else is also concerned with
- They can have some configuration that only they are concerned with
- They live in a group, let's call that a ContextGroup

So let's define some simple rules:
- When the data changes on a DataContext, everyone else needs to update
- When the config changes, only that guy needs to update

### Reactors
So it turns out there are some other event types that we're concerned with beside just dataChange and configChange
We react to these events with the things called Reactors. Great name, right?

Basic
- dataChange has an 'others' reactor
- configChange has an 'only' reactor
- other more complex reactors for things like type change, item added, item removed, etc

// Aside - What exactly IS a reactor? A function that:
    - Takes:
        - the array of items in the group
        - the context that instigated the event
    - Returns:
        - a list of contexts that need to update

### So how might this work?
- query
    - data
        - search
- results
    - config
        - sort
        - page
        - etc
    - context
        - purchase orders
- facets
    - data
        - what's checked
        - include? exclude?
    - config
        - count (more/less)
        - find (find box)
    - context
        - options to check
- chart
    - context
        - top terms, etc
    - config
        - count? field? :)

### So what else does this enable?

/meme aww yea it's so easy
- Add/remove filters? No problem :)
- Add/remove CHARTS? NO problem :)
- Global debouncing
- Global pause/resume (intelligently running at most 1 or no search on resume)
- Groups are a hierarchy as groups have items AND groups in them
    - why? logical grouping for pausing
    - but also - 'join' for and/or/not

## Lets talk cascading
- in one, out the other
- series of tools that lets you essentially do "select in"
- see: GroupCompositor

## Elastic what?
- Could be backed by mongo, sql, etc
- Might have Matt do the mongo backer
- like flipping a switch - then advanced search and charts and and wizards and everything for mongo too
- and... cascade across databases (aka, cross database joins!)
/meme advanced search all the things!


## The VISION.
- Data: just a context group
- Report: Iterate over fancy ko components ("widget config") - already on posearch2
- Output target (csv, email, pdf... - smart export)
- Alert: agenda (smart export)

Think about it.. almost nothing is specific to purchase orders.
What's really been done here?
"Productized" the "Data Product" COMPANY
:)

## Are we done yet? NO!
- Customers at risk
- Prospecting
- Agency Dashboard
- Custom Reports (aka Phase 2 from our investor slide deck)
- The "widgets"/ "dashboard"
- its all trivial
- This will power the foundation of every major product we can imagine (at least until we move into the marketplace... sadly this doesn't do ecommerce too)

## Fun facts
- Stop calling it advanced search
/image the core
- Well over 1k lines of unit test. Srsly
- This is major iteration 2.5 (entire event processing architecture was recently rewritten to support pause/resume and trur global debouncing
