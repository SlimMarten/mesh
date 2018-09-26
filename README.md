<p align="center">
	<img src="https://raw.githubusercontent.com/SlimMarten/postcss-mesh/development/assets/img/logo.png" align="center">
<h3 align="center">
Mesh
</h3>
<p align="center">
Lightweight Grid Compiler for <a href="https://github.com/postcss/postcss">PostCSS</a>
</p>
<p align="center">
<a href="https://www.npmjs.com/package/postcss-mesh" rel="nofollow"><img src="https://img.shields.io/npm/v/postcss-mesh.svg" alt="Slack" data-canonical-src="https://img.shields.io/npm/v/postcss-mesh.svg" style="max-width:100%;"></a>
<a href="https://www.npmjs.com/package/postcss-mesh" rel="nofollow"><img src="https://img.shields.io/npm/dt/postcss-mesh.svg" alt="Slack" data-canonical-src="https://img.shields.io/npm/dt/postcss-mesh.svg" style="max-width:100%;"></a>
</p>
</p>

<h2>Table of contents</h2>

- [About Mesh](#about-mesh)
- [Unique Selling Points](#unique-selling-points)
    - [Responsive Gutter](#responsive-gutter)
    - [Gutter On Outside](#gutter-on-outside)
    - [Mobile First || Desktop First](#mobile-first--desktop-first)
    - [Unlimited Breakpoints](#unlimited-breakpoints)
    - [Property Overwrite](#property-overwrite)
- [Components](#components)
  - [Basic Components](#basic-components)
    - [Container](#container)
    - [Void](#void)
    - [Column](#column)
  - [Transform Components](#transform-components)
    - [Offset](#offset)
    - [Pull](#pull)
    - [Push](#push)
- [Properties](#properties)
- [Common Grid Features](#common-grid-features)
- [Getting Started](#getting-started)
  - [Installation](#installation)
  - [Grid Setup](#grid-setup)

## About Mesh

I know there are a lot of different grid systems already out there and most of them are pretty good. But ☝️ none of them is offering the whole bandwidth of possible options. E.g. I wanted to have the ability to switch between different [display-types](#Properties) _(float || inline-block || flex)_ as well as I wanted to be able to overwrite certain parameters like [gutter](#properties) or [column-count](#properties) breakpointwise. That's how I came up with the idea to create my very own grid compiler and Mesh was born 🎉🎉🎉.

## Unique Selling Points

#### Responsive Gutter

Set the [responsive-gutter](#properties) property to `true` and `margins` & `paddings` of all [components](#components) (except [container](#container)) will be calculated on a percentage basis. This means not only the column's width will scale but the gap between two columns to guarantee a true fluid layout.

```css
// true || false
// default: true

@mesh-grid {
	responsive-gutter: true;
}
```

_Notice: This feature only allows to inherit the root columns gap to the first nested level._

#### Gutter On Outside

Allows you to toggle the [container's](#container) padding which is based on the [gutter](#properties)-Size.

```css
// true || false
// default: true

@mesh-grid {
	gutter-on-outside: true;
}
```

#### Mobile First || Desktop First

You can decide if your default viewport is a desktop one or a mobile one using the [query-condition](#properties) property. This property takes `min-width` or `max-width` as an argument. If set to `min-width` your default viewport will be a mobile one. As soon as your screen's width hits the next bigger width defined in all of your breakpoints, it snaps to the related breakpoint.

```css
// min-width || max-width
// default: min-width

@mesh-grid {
	query-condition: min-width;
}
```

#### Unlimited Breakpoints

Bootstrap comes with five predefined breakpoints (Extra small _<576px_, Small _≥576px_, Medium _≥768px_, Large _≥992px_, Extra Large _≥1200px_). These breakpoints have proved its worth over time. But nevertheless, sometimes your design requires more individual breakpoints. In this case Mesh is your best friend. With Mesh you can define as many or as less custom breakpoints as you want using the `@mesh-viewport-VIEWPORTNAME`-@Rule where `VIEWPORTNAME` is the viewport's ID. The ID is used in the viewport specific classes. E.g. `@mesh-viewport-lg` results in `.mesh-column-lg`-classes.

```css
// this is how you would define a standard large bootstrap breakpoint
// properties `container-width` & `viewport` are required

@mesh-viewport-lg {
	container-width: 960px;
	viewport: 992px;
}
```

#### Property Overwrite

Property Overwrite allows you to overwrite some properties breakpointwise, e.g. `gutter`. Learn more about properties [here](#properties).

```css
// px
// default: 30px

@mesh-viewport-lg {
	gutter: 30px;
}
```

## Components

Mesh's compiled Grid is made of three **basic components** and three _(as I like to call)_ **transform components**.

### Basic Components

The basic components describe the `.mesh-container`, `.mesh-void` & `.mesh-column` classes. These components are necessary to set up a very basic grid.

#### Container

The container is the most outer component of a grid instance. It sets up the maximum width of the grid and should not be nested.

```html
<div class="mesh-container"></div>
```

#### Void

The void component is the equivalent to Bootstrap's `row` component and voids its parent gutter. The only immediate child of a void component should be a [column](#column).

```html
<div class="mesh-void"></div>
```

#### Column

```html
<div class="mesh-column-x"></div>
```

### Transform Components

The transform components describe the `.mesh-offset`, `.mesh-pull` & `.mesh-push` classes. These components are necessary to transform a [column](#column) within your grid. Using transform components you can reorder your [columns](#column).

#### Offset

The offset component will add a margin to the respective [column](#column) to create an even bigger gap between two columns.

```html
// using the component like below will offset the
// respective column about the width of a single column.

<div class="mesh-offset-1"></div>
```

#### Pull

The pull component will reposition the respective [column](#column) from the right.

```html
// using the component like below will pull the
// respective column about the width of a single column
// to the left.

<div class="mesh-pull-1"></div>
```

#### Push

The push component will reposition the respective [column](#column) from the left.

```html
// using the component like below will push the
// respective column about the width of a single column
// to the right.

<div class="mesh-push-1"></div>
```

## Properties

Mesh is based on a bunch of properties you can adjust to your needs. Some of them are overwriteable in each defined breakpoint. See the table below to get an overview of what Mesh is offering.

<table>
  <thead>
    <tr>
      <th align="left">Property</th><th align="left">Description</th><th align="left">Default</th><th align="left">Required Options</th><th align="left">Overwriteable</th>
    </tr>
  </thead>
  <tbody>
    <!-- <tr><th colspan="5" align="center">Basic Properties</th></tr> -->
    <!-- <tr><th colspan="5" align="center">Viewport-Relevant Properties</th></tr> -->
    <tr><td><code>column-align</code></td><td>Aligns all columns at the <code>top</code>, <code>middle</code> or <code>bottom</code> of a row</td><td><code>top</code></td><td><code>top</code> || <code>middle</code> || <code>bottom</code></td><td>yes</td></tr>
    <tr><td><code>column-count</code></td><td>Defines how many columns fit in one row</td><td><code>12</code></td><td><code>int</code></td><td>yes</td></tr>
    <tr><td><code>compile-default-classes</code></td><td>If set to <code>false</code> Mesh won't compile the current's grid classes</td><td><code>true</code></td><td><code>true</code> || <code>false</code><td>-</td></tr>
    <tr><td><code>container-width</code></td><td>Defines the containers <code>max-width</code> property for current viewport</td><td>none</td><td><code>px</code> or <code>%</code></td><td>yes</td></tr>
    <tr><td><code>display-type</code></td><td>Defines if the grid is based on inline-block columns, floated columns or flexed columns</td><td><code>inline-block</code></td><td><code>inline-block</code> || <code>float</code> || <code>flex</code><td>-</td></tr>
    <tr><td><code>gutter</code></td><td>Sets the gap between columns</td><td><code>30px</code></td><td><code>px</code></td><td>yes</td></tr>
    <tr><td><code>gutter-on-outside</code></td><td>If set to <code>false</code> the container won't have a padding</td><td><code>true</code></td><td><code>true</code> || <code>false</code></td><td>yes</td></tr>
    <tr><td><code>name</code></td><td>Sets the grid's name and adjusts the component's class prefix</td><td><code>mesh</code></td><td><code>string</code></td><td>-</td></tr>
    <tr><td><code>query-condition</code></td><td>Using this property you can decide if you want the compiled styles to be mobile first or desktop first</td><td><code>min-width</code></td><td><code>min-width</code> || <code>max-width</code><td>-</td></tr>
    <tr><td><code>responsive-gutter</code></td><td>If set to `true` paddings and margins of column and void components will be calculated on a percentage basis</td><td><code>true</code></td><td><code>true</code> || <code>false</code></td><td>yes</td></tr>
    <tr><td><code>viewport</code></td><td>Defines the screen's width at which a new media-query should be initiated</td><td>none</td><td><code>px</code></td><td>yes</td></tr>
  </tbody>
</table>

## Common Grid Features

## Getting Started

### Installation

```console
$ npm i postcss-mesh
```

### Grid Setup

This is what a basic Grid Setup using Mesh looks like:

```css
@mesh-grid {
	name: mesh;
	display-type: inline-block;
	gutter: 30px;
	column-count: 12;
	container-width: fluid;
	compile-default-classes: true;
	mobile-first: true;

	@mesh-viewport-viewportname1 {
		viewport: 1200px;
		container-width: 1170px;
	}

	@mesh-viewport-viewportname2 {
		...;
	}

	@mesh-viewport-viewportname2 {
		...;
	}
}
```
