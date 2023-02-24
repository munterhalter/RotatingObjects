# A basic A-Frame scene with a custom component

The HTML page [index.html](./index.html) contains a basic 3D scene created using the [A-frame](https://aframe.io/) library. The whole scene is a single HTML file; HTML files are files containing webpages. 

Documentation for A-Frame is available from https://aframe.io/docs/master/introduction/

This example illustrates the basics of creating a custom component that contains javascript logic and attaching it to an `<a-entity>`

## Javascript
The programming language for the web is [Javascript](https://en.wikipedia.org/wiki/JavaScript)

If you want, you can learn using [Javscript tutorial](https://www.w3schools.com/js/) or figure things out as you go. 

Javascript is included in HTML files by placing it between `<script>` and `</script>` tags. 

Javascript can also be included by loading it from a separate file or url. We already do this in the header with the line
```<script src="https://aframe.io/releases/1.4.0/aframe.min.js"></script>```
that loads the javascript contents available at https://aframe.io/releases/1.4.0/aframe.min.js (don't worry if that file does not make sense, it is compressed a.k.a. *minified* to reduce loading time.)

You can experiment with javascript by opening the javascript console in any webpage. Javascript has access to the internals of any website you are on. [HowTo](./javascript-in-browser.md)

## A-Frame components

Components are pieces of code and logic that can be attached to an `<a-entity>`.

We have already seen several A-Frame components in action: `geometry`, `material`, `position`, `rotation`, `movement-controls`

A general description of components is available here: https://aframe.io/docs/master/core/component.html


### Defining a component
A component is defined using Javascript in a `<script>` inside an html document. To define a component you must call the function 

### Debugging and throttling

To debug the code of the component it is useful to output information to the console. To do this one can always use 
```
console.log()
```
However, console output is a slow operation. We do not want to be doing this every frame. For this reason we put all debugging code into the function 
```
debug(t,dt)
```
and make sure to throttle it: we try to run it on every frame by calling:
```
this.debug(t,dt);
```
in the ```tick(t,dt)``` function. However the line:
``` 
this.debug = AFRAME.utils.throttle(this.debug, 1000, this);
```
makes sure that the function call gets ignored most of the time and it gets executed at most 1 time every 1000ms. 

You can put debugging code into ```this.debug(t,dt)```.

[AFRAME.registerComponent(name,definition)](https://aframe.io/docs/master/core/component.html#aframe-registercomponent-name-definition)

The first argument to the function, `name`, is a *string* with the name of your component
The second argument to the functions, `definition`, is an *object*. In javascript, an object is a compound type that can contain a lot of variables. It is defined as a comma-separated list of objects, enclosed by `{` `}`.

A full description what the definition should contain is found in the [documentation](https://aframe.io/docs/master/core/component.html#aframe-registercomponent-name-definition)

For now, we are concerned with the following parts of the `definition`. 

#### The `init` function
The `init()` function gets executed each time a component gets first attached to an entity. 

#### The `tick` function
The functions `tick(t,dt)` gets executed on each frame render. It can contain any logic, but be careful. Do not put in commands that take a long time to run! 

#### Other functions
You can define other helper functions that are not required by A-Frame


### Attaching a component

To attach a component to an entity you just need to write it into the opening `<a-entity>` tag:

```
<a-entity
        id="box1"
        geometry="primitive: box"
        material="color: red"
        position="1 0 -5"
        rotation="0 0 0"
        my-component
      ></a-entity>
```
A new instance of the component (i.e. a copy) gets created each time it is attached to an `<a-entity>`. From within the code of the component, you can access the current instance using the `this` variable. In javascript, the "sub-variables" of an object `a` are accessed using the `a.b` notation. For example the `tick` function of the current instance of a component can be accessed as

```
this.tick
```

### Accessing the associated `<a-entity>`

Each instance of a component stores in `el` a reference to the `<a-entity>` to which it is attached. It can be accessed using:

``` 
this.el
```

We do this in the line

```
console.log(this.el);
```
in the initialization code of our component

#### 3D object properties
Each `<a-entity>` stores information about the 3D object it represents in the `object3D` variable. From a component you can access it using 

```
this.el.object3D
```

- `this` refers to the instance of the component
- `this.el` refers to the `<a-entity>` to this instance of the component
- `this.el.object3D` refers to the  3D object related to the `<a-entity>` related to this instance of the component

Different properties of `object3D` are documented here:
- https://aframe.io/docs/master/core/entity.html#object3d
- https://threejs.org/docs/#api/en/core/Object3D
