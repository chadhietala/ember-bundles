# Ember Route Bundles

The purpose of this repo is to outline possible directory structures that would work for bundling application assets along top level route lines. As backend applications grow and you go through the process of breaking up the monolithic application, you tend to break up the application into smaller services along top level routes. This seems to be applicable in the case of Ember applications because of the URL centric nature of the framework.

Where it possibly breaks down is the containerization of objects. If a developer needs an object dependency from outside of their route, this may be a smell or it may just mean that the object needs to be pulled into a common place in which all routes can gain access to it. This however is slightly counter intuitive because it means that the entire application will take a payload hit just so we can share code.  One solution to this is [pre-process mapping of object dependencies to file dependencies](https://github.com/chadhietala/ember-object-graph-resolver).

# Lazy Loading 3rd Party Scripts

Currently we don't have a common way of thinking about pulling down 3rd party scripts on demand. While the route approach is going to work for the vast majority of use cases, there still is a desire to lazy load large 3rd party scripts like [Ace Editor](http://ace.c9.io/#nav=about).

Some ideas might be to create a `lazy` directory inside of routes or sub-routes. At build time we could create a manifest of the route paths that they lie in and paths to the files so that Ember can cross reference the manifest at runtime. The problem with this however is this means we would have to `cp` or `ln` bower dependencies to these nested directories.

Another idea is just to use annotations in the file.  At LinkedIn we wrote [Venus](http://www.venusjs.org/) which is a test runner that is comparable to Karma but is way simpler to configure. We do this through annotations in the spec files.  Basically we could parse the files and grab the lazy asset references.

```
/**
@ember-lazy ace-editor
**/

exports default Ember.View.extend({
   ... 
});
``` 

I don't know if adding semantics to Ember is the best solution but rather have the file directory or annotations be the source of truth.