# Component Interaction

### *Share information between different directives and components*

This cookbook contains recipes for common component communication scenarios in which two or more components share information.

## Content

The following patterns are:

* (Input-Output) [1st choice] Pass data from parent to child with input binding and vice versa with output
* (ngOnChanges) [2nd choice] Intercept input property changes with ngOnChanges
* (ViewChild) [3rd choice] Parent interacts with child via ViewChild or ViewChildren
* (Service) [last choice] Parent and children communicate via a service provider

When choosing an interaction method, start from `input` then work your way down... the service pattern should be your last choice. The service pattern is only used for global variables and binding flows. 

When building child components, be sure to:

    * Make components reusable as much as possible
    * Keep components well documented
    * Avoid too many requirements, no more than 5 inputs/outputs
    * Keep component's small: **rule of thumb** 100 lines of code and no more than 20 lines of html.

