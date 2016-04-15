# Animation Notes

## Transitions vs Animations
 
### Sample code 
 
```css
.demo {
  width: 100px;
  height: 100px;
  background-color: red;
  transition: width 2s;
}

.demo:hover {
  width: 200px;
}

/* vs */
 
@keyframes anim {
  from { width: 100px; }
  to { width: 200px; }
}

.demo {
  width: 100px;
  height: 100px;
  background-color: red;
}

.demo:hover {
  animation: anim 2s;
}
``` 

### Notes

1. Transition is an animation between two *different* states.
2. Transitions are one-time.
   Animations can be (and most often are) looped
3. Transition is about single CSS value change (multiple vals = multiple transitions).
4. Animations have keyframes (more control)
5. Transitions are JS-friendly.
   Animations are not. It's a mess to deal with keyframes through JS

Cases when JS animation beats CSS animations (https://css-tricks.com/myth-busting-css-animations-vs-javascript/):

* Relative values: Like "animate the rotation 30 degrees more" or "move the element down 100px from where it is when the animation starts".
* Nesting: Imagine being able to create animations that can get nested into another animation which itself can be nested, etc. Imagine controlling that master animation while everything remains perfectly synchronized. This structure would promote modularized code that is much easier to produce and maintain.
* Progress reporting: Is a particular animation finished? If not, exactly where is it at in terms of its progress?
* Targeted kills: Sometimes it's incredibly useful to kill all animations that are affecting the "scale" of an element (or whatever properties you want), while allowing the rest to continue.
* Concise code: CSS keyframe animations are verbose even if you don't factor in all the redundant vendor-prefixed versions necessary. Anyone who has tried building something even moderately complex will attest to the fact that CSS animations quickly get cumbersome and unwieldy. In fact, the sheer volume of CSS necessary to accomplish animation tasks can exceed the weight of a JavaScript library (which is easier to cache and reuse across many animations).

Also you can't really do any of the following with CSS animations:
* Animate along a curve (like a Bezier path).
* Use interesting eases like elastic or bounce or a rough ease. There's a cubic-bezier() option, but it only allows 2 control points, so it's pretty limited.
* Use different eases for different properties in a CSS keyframe animation; eases apply to the whole keyframe.
* Physics-based motion. For example, the smooth momentum-based flicking and snap-back implemented in this Draggable demo.
* Animate the scroll position.
* Directional rotation (like "animate to exactly 270 degrees in the shortest direction, clockwise or counter-clockwise").

### Conclusion 

Use CSS transitions and JS animations. CSS animations don't worth the effort.

## Behavior

### IE
<table>
<tr><th>Version</th><th>setInterval</th><th>requestAnimationFrame</th></tr>
<tr><td>9-</td><td>not affected</td><td>not supported</td></tr>
<tr><td>10+</td><td>not affected</td><td>paused</td></tr>
</table>

### Chrome
<table>
<tr><th>Version</th><th>setInterval</th><th>requestAnimationFrame</th></tr>
<tr><td>9-</td><td>not affected</td><td>not supported</td></tr>
<tr><td>10</td><td>not affected</td><td>paused</td></tr>
<tr><td>11+</td><td>>=1000ms</td><td>paused</td></tr>
</table>

### Firefox
<table>
<tr><th>Version</th><th>setInterval</th><th>requestAnimationFrame</th></tr>
<tr><td>3-</td><td>not affected</td><td>not supported</td></tr>
<tr><td>4</td><td>not affected</td><td>1s</td></tr>
<tr><td>5+</td><td>>=1000ms</td><td>(2^n)s </td></tr>    
</table>
Where n is number of frames since inactivity

### Safari
<table>
<tr><th>Version</th><th>setInterval</th><th>requestAnimationFrame</th></tr>
<tr><td>5-</td><td>not affected</td><td>not supported</td></tr>
<tr><td>6</td><td>not affected</td><td>paused</td></tr>
<tr><td>7+</td><td>>=1000ms</td><td>paused</td></tr>    
</table>  
  
### Opera
<table>  
<tr><th>Version</th><th>setInterval</th><th>requestAnimationFrame</th></tr>
<tr><td>12-</td><td>not affected</td><td>not supported</td></tr>
<tr><td>15+</td><td>>=1000ms</td><td>paused</td></tr>
</table>

## Tips

It's almost always best to key animations on the amount of real time elapsed since beginning of the animation, as read from `Date`. So that when intervals don't fire quickly (for this or other reasons) the animation just gets jerkier, not slower.

Web Worker is allowed to use intervals/timeouts without limitation.
