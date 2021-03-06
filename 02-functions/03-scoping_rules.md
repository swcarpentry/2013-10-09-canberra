# Scoping in R

Assuming we run the following code:

```coffee
c <- 100
(c  + 1)
```

Can we still use `c()` to concactenate vectors?

```
(x1 <- c(1:4))
```

How does R know which value of `c` to use when? R has separate namespaces for functions and non-functions. That's why this is possible.


When R tried to "bind" a value to a symbol (in this case `c`), it follows a very specific search path, looking first at the Global environment, then the namespaces of each package.

What is this order?

```
> search()
 [1] ".GlobalEnv"        "package:graphics"  "package:grDevices"
 [4] "package:datasets"  "package:devtools"  "package:knitr"
 [7] "package:plyr"      "package:reshape2"  "package:ggplot2"
[10] "package:stats"     "package:coyote"    "package:utils"
[13] "package:methods"   "Autoloads"         "package:base"
```

Newly loaded packages end up in position 2 and everything else gets bumped down the list. `base` is always at the very end.

`.GlobalEnv` is just your workspace. If there's a symbol matching your request, it will take that value based on your request.

If nothing is found, it will search the namespace of each of the packages you've loaded (your list will look different).

Package loading order matters. 

Example:


```
library(plyr)
library(Hmisc)
is.discrete
```

```
library(Hmisc)
library(plyr)
is.discrete
```

Reference functions inside function's namespace using the `::` operator.

```
Hmisc::is.discrete
plyr::is.discrete
```

R uses scoping rules called Lexical scoping (otherwise known as static scoping).

It determines how a value is associated with a free variable in a function.

```
add <- function(a, b) {
    (a + b)/n
}
```

`n` here is the free variable.

**Rules of scoping**  
* R first searches in the environment where the function was defined.* Environment is a collection of symbols and values. Environments have parents. 

Since we defined `add` in the global env, R looks for `n` in that environment. 

```
> parent.env(globalenv())
<environment: package:graphics>
attr(,"name")
[1] "package:graphics"
attr(,"path")
[1] "/Library/Frameworks/R.framework/Versions/3.0/Resources/library/graphics"
> search()
 [1] ".GlobalEnv"        "package:graphics"  "package:grDevices"
 [4] "package:datasets"  "package:devtools"  "package:knitr"
 [7] "package:plyr"      "package:reshape2"  "package:ggplot2"
[10] "package:stats"     "package:coyote"    "package:utils"
[13] "package:methods"   "Autoloads"         "package:base"
````


These rules matter because you can define nested functions. 

Example:


```
make.power <- function(n) {
    pow <- function(x) {
    x^n
 }
pow
}
```

This is a constructor function. A function that creates another one.

```
cube <- make.power(3)
square <- make.power(2)
```

```
cube(3)
square(3)
```

```
ls(environment(cube))
get("n", environment(cube))
```

```
ls(environment(square))
get("n", environment(square))
```

You can see that `R` is searching for `n` first within each environment before looking elsewhere.

Why scoping matters?

```
y <- 10

f1 <- function(x) {
    y <- 2
    y^2 + f2(x)
}


f2 <- function(x) {
    x * y
}
```

What does `f1(10)` return?

Possible answers: 
* `104`
* `24`


This is a consequence of lexical or static scoping. The alternate will result if R were using dynamic scoping. One downside (as you'll see with larger tasks) is that R has to carry everything in memory.

---




