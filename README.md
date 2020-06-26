# How to use transition.js and math formulas manipulation

## transition.js

transition.js is a javascript script allowing the user to make web presnetations and transitions. You can find the original version [at this address](https://math.unice.fr/~pantz/Geek/transition.html).
In order to use this script, simply add this line to your html page : 

```<script type="text/javascript" src="transition.js"></script>```

## How to add math formulas

In order to add math formulas to your web site, you can use the javascript library named Mathjax. To use this library, just add this other line in your code :

```<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.6.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML"> </script>```

After that, you can insert math formulas by simply adding $$...$$ with your math formulas in it. (Note that mathjax supports LaTeX, AsciiMath or MathML syntax)

## Highlight your formulas

Now that you got your scripts settled and your formulas on your page, it is time to manipulate them !

First, just add a css Id to the math element which you want to highlight. If you're using Mathjax, you can do it this way :

```$$ \cssId{yourId}{a}x + b$$```

Then, add this little line in your web page.

```<script>document.addEventListener("DOMContentLoaded",function(){addDiv("yourId", "highlightId")})</script>```

Upon loading your document, the addDiv function will automatically create a new div element with an id (which is "highlightId"). This div will be your math element's child and will be at the exact same position as its parent.

```
function addDiv(iDiv, aChild) {
        var div = document.createElement('div');
        div.id = iDiv;
        div.classList.add("hidden");
        div.classList.add("normal");
        document.getElementById(aChild).appendChild(div);
    } 
```

Now, you can simply add some css style to your id, such as adding a background-color to highlight your element.

## Manipulate your formulas

In order to move your elements or formulas here're the steps : 
* Write your formula at its initial place

``` $$ \cssId{myElement}{a}x +b$$ ```

* Write the exact same element/formula you want to move at the position where you want to move it, and make it hidden (with css)

``` Let's move the element here : $$\cssId{destination}{a}$$ ```

* Now just add this in the fragment section, as if you wanted to add a new class 

``` fragments = "[myElement]{move destination}" ```

This will call a function which will automatically move "myElement" to "destination" and scale it the same way the destination element was scaled.

### Technical details

Let's review this part of the code : 
```
  let moveTarget=frag.target[J];
	let mObserver = new MutationObserver(
  function(mutations) {
	  mutations.forEach(function(mutation) {
		  if (mutation.attributeName==="class") {
        if(moveTarget.classList.contains("move")){
           move(moveTarget, document.getElementById(moveTarget.classList[moveTarget.classList.length -1].toString()));
        }
        else {
					  moveTarget.style.setProperty("transform","");
				}	
			}
		});
		});
			mObserver.observe(moveTarget,{attributes:true}); 
```

Upon adding a new class, the script will check if your class' name is "move". If so, the script will take the last class name you have added to your element (which's name will be the same as your destination id's name) and call the move function :

```
  function move(divToMove, divDest){
		  divToMove.style.setProperty("transform","translate("+(PositionLeft(divDest)-PositionLeft(divToMove))+"px,"+(PositionTop(divDest)-PositionTop(divToMove))+"px) scale("+
			(divDest.offsetWidth/divToMove.offsetWidth))
	} 
```  
    
This function here will set your element's porperty in order to move it and scale it at the exact position and scale your destination element is. Note that you can add ```transition:all 3s;``` to your initial element's style to make the transition smoother and easier to watch.

