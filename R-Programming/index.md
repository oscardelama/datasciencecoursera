R Programming, Lecture notes
=============================

Overview and History of R
--------------------------
Basically R is a dialect of S.

Recommended Books:
* Statistical Models in S. John M. Chambers.
* Software for Data Analysis, Programming with R. John M. Chambers.

Getting Help
------------
In R:

```R
?subject
```

Vector
-------

The best basic object in R is what's called a vector, and a vector contains multiple copies of, for example, **of a single type of object**. 

We can build a vector with c() and listing its members.

```R
# Basic vector construction
vec <- c(2, 5, 8)

# Object class
# A vector is atomic: it contains a collection of elements of one type
> class(vec)
  [1] "numeric"
  
# Object structure summary
> str(vec)
 atomic [1:3] 2 5 8
 - attr(*, "comment")= chr "A dummy sample"

 # Object length
 > length(vec)
[1] 3

# We can freely attach a comment to the object
> comment(vec) 
  NULL
> comment(vec) <- "A dummy sample" 
> comment(vec)
  [1] "A dummy sample"

# Names of the vector elements
> names(vec)
  NULL
> names(vec) <- c("two", "five", "eight")
vec
  two  five eight 
    2     5     8 
# We can use those names to select vector elements
> vec["five"]
five 
   5 
#However, it is still a vector
> class(vec)
[1] "numeric"

# Object dimensions. 
# The vector is "dimensionless",  a matrix, array or data frame are not.
> dim(vec) 
  NULL
# There is name for each dimension
> dimnames(vec) 
  NULL
```

### Handling Vector Content

```R
# Append elements
> vec <- c(2, 5, 8)
> vec
[1] 2 5 8
> vec_ext <- append(vec, c(98, 99))
> vec_ext
[1]  2  5  8 98 99

# Insert new element at any position
> vec_mod <- append(vec, c(98,99), after=2)
> vec_mod
[1]  2  5 98 99  8

# Build a vector from other vectors
# e.g. prefix and suffix a vector
> vec2 <- c(c(80,81), vec, c(98,99))
[1] 80 81  2  5  8 98 99
```

### Type Coercion (Type Casting)

```R
# We can cast a vector with one type of content to another one
vec <- c(2, 5, 8)

# As character elements
vec_tect <- as.characer(vec)
vec_tect <- as.character(vec)
vec_tect
  [1] "2" "5" "8"

# As logical elements
# Zero is FALSE, otherwise is TRUE
vec_log <- as.logical(vec)
vec_log
[1] TRUE TRUE TRUE
```

Matrix
-------

```R
# Basic matrix construction
> mtx <- matrix(1:6, nrow=2, ncol=3)

# Notice how the values are assigned column-wise.
# As a vector, all the matrix elements are of the same type.
# We can think of the matrix as a vector segmented in columns
> mtx
     [,1] [,2] [,3]
[1,]    1    3    5
[2,]    2    4    6

# Object class
> class(mtx)
[1] "matrix"

# Object length
> length(mtx)
[1] 6

# Number of columns and rows
> nrow(mtx)
[1] 2
> ncol(mtx)
[1] 3

# Object structure summary
> str(mtx)
 int [1:2, 1:3] 1 2 3 4 5 6
 
# Object dimensions. 
> dim(mtx) 
[1] 2 3

# Names of each object dimension
> dimnames(mtx)
NULL
```

### Handling Matrix Content

```R
# cbind and rbind are more powerful way to build a matrix
# Each parameter in the call becomes a column of
# the resulting matrix. 
# The smaller parameters (with smaller count of elements) 
# are recycled, so they have to be a sub-multiple of 
# the largest one. Otherwise you will get a warning message.
> mtx <- cbind(seq=1:8, quarter=1:4, bin=0:1)
> mtx
     seq quarter bin
[1,]   1       1   0
[2,]   2       2   1
[3,]   3       3   0
[4,]   4       4   1
[5,]   5       1   0
[6,]   6       2   1
[7,]   7       3   0
[8,]   8       4   1

# Get the column names
> colnames(mtx)
[1] "seq"     "quarter" "bin" 

# We can build the same matrix using:
> seq=1:8
> quarter=1:4
> bin=0:1
> mtx <- cbind(seq, quarter, bin)

# The name of the columns once again will be 
> colnames(mtx)
[1] "seq"     "quarter" "bin" 

# row names
> row.names(mtx)
NULL

# The matrix structure
> str(mtx)
 int [1:8, 1:3] 1 2 3 4 5 6 7 8 1 2 ...
 - attr(*, "dimnames")=List of 2
  ..$ : NULL
  ..$ : chr [1:3] "seq" "quarter" "bin"

# You can flat ("vectorize") a matrix with c()
> vec <- c(mtx)
> vec
 [1] 1 2 3 4 5 6 7 8 1 2 3 4 1 2 3 4 0 1 0 1 0 1 0 1
 
# In the same way you can use mtx[[i]] to traverse mtx as a vector
> mtx[[5]]
[1] 5

# All the matrix elements should have the same type.
# An attempt top use mixed types will cause the casting to
# the broader type
> mtx <- cbind(seq=1:8, quarter=1:4, gender=c("male", "female"))
> mtx
     seq quarter gender  
[1,] "1" "1"     "male"  
[2,] "2" "2"     "female"
[3,] "3" "3"     "male"  
[4,] "4" "4"     "female"
[5,] "5" "1"     "male"  
[6,] "6" "2"     "female"
[7,] "7" "3"     "male"  
[8,] "8" "4"     "female"

# row wise binding
> mtx <- rbind(seq=1:8, quarter=1:4, bin=0:1)
> mtx
        [,1] [,2] [,3] [,4] [,5] [,6] [,7] [,8]
seq        1    2    3    4    5    6    7    8
quarter    1    2    3    4    1    2    3    4
bin        0    1    0    1    0    1    0    1

# row names
> row.names(mtx)
[1] "seq"     "quarter" "bin"    

# The matrix structure
> str(mtx)
 int [1:3, 1:8] 1 1 0 2 2 1 3 3 0 4 ...
 - attr(*, "dimnames")=List of 2
  ..$ : chr [1:3] "seq" "quarter" "bin"
  ..$ : NULL
```

List
-----

```R
> l <- list(1, "Hello", c(18,65))
> l
[[1]]
[1] 1

[[2]]
[1] "Hello"

[[3]]
[1] 18 65
```

Factor
-------
Is *a special type* of vector which is used to create to represent categorical data. 

```R
# Lets build a vector with factor values
> has_stock = c("yes", "yes", "no", "yes", "no")
> has_stock
[1] "yes" "yes" "no"  "yes" "no"

# Convert the vector to an *unordered* factor
> fctr <- factor(has_stock)
# Notice the following values are not quoted: they has not 
# the characters, they are factors!
# Also notice the sequence of the levels
> fctr
[1] yes yes no  yes no 
Levels: no yes

# Object class
> class(fctr)
[1] "factor"

# Object structure summary
> str(fctr)
 Factor w/ 2 levels "no","yes": 2 2 1 2 1

# Object length
> length(fctr)
[1] 5

# Factors has the attribute levels, which returns the encoded 
# labels. The factors are alphabetically as an ordered sequence.
# Each factor is encoded with its index in that sequence.
# i.e. {1:no, 2:yes}
> levels(fctr)
[1] "no"  "yes"

# You can specify the encoding with the levels parameter
> fctr <- factor(c("yes", "yes", "no", "yes", "no"), 
+    levels = c("yes", "no"))
# Now the the factors are encoded as specified
> levels(fctr)
[1] "yes" "no" 
> str(fctr)
 Factor w/ 2 levels "yes","no": 1 1 2 1 2
```

### Handling Factors Content

```R
# *table()* returns an object of 'table' type with the frequency
# of each disticnt factor
> fctr <- factor(c("yes", "yes", "no", "yes", "no"))
> table(fctr)
fctr
 no yes 
  2   3 

# Unclass() allows the access to the coded values
> ucl <- unclass(fctr)
> str(ucl)
 atomic [1:5] 2 2 1 2 1
 - attr(*, "levels")= chr [1:2] "no" "yes"
# Get the factor as label
> fctr[2]
[1] yes
Levels: no yes
# Get the factor as code
 > ucl[2]
[1] 2
```

Missing Values
---------------
*NA*: Not Available. Missing value.
*NaN*: Has a "numeric nature" but is not a numeric value (e.g the value of 0/0. They are used to represent missing numeric values.

```R
# is.na() and is.nan() are used to identify this missing values.
# Not Available
> notAvail <- NA
> is.na(notAvail)
[1] TRUE

# Not a Number
> notNumber <- NAN
> is.nan(notNumber)
[1] TRUE

# A NaN is a missing value
> is.na(notNumber)
[1] TRUE

# But a missing value is not a NaN
> is.nan(notAvail)
[1] FALSE
```

Swirl
------
library(swirl)
info() displays these options:
* *skip()* allows you to skip the current question.
* *play()* lets you experiment with R on your own.
* *nxt()* will come back to swirl after a play().
* *bye()* Exits swirl. Your progress will be saved.
* *main()* returns you to swirl's main menu.


