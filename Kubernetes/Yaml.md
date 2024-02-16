ইয়েমেল 

Used to represent configuration data

**যদি manually yaml ফাইল লিখতে চাই, তবে 2 space use করব**

**Key Value Pair**
```yaml
Fruit: Apple
Vegetable: Carrot
Liquid: Water
Meat: Chicken
```
> You must have a space followed by : (colon) differentiating the key and value

**Array/Lists**

Lists are ordered collection। এইদিকে order matter করে 
```yaml
Fruits:
-   Orange 
-   Apple
-   Banana

Vegetables:
-   Carrot
-   Cauliflower
-   Tomato
```

> In Array, each item with a - (dash) in the front of the item. The dash indicates that it's an element of an array. 

**Dictionary/Map**

Dictionary is an unordered collection. এইদিকে order matter করে না, যদি value or format ঠিক থাকে। যেমন carbs যদি fat এর আগে আসে তাহলে কোন problem নাই 
```yaml
Banana:
    Calories: 105 
    Fat: 0.4 g 
    Carbs: 27 g

Grapes:
    Calories: 62 
    Fat: 0.3 g 
    Carbs: 16 g
```


> A Dictionary is a set of properties grouped together under an item. All items should be aligned together


The number of spaces before each property is key in YAML

#### Case 1

একটা object এর different properties গুলো list out করতে আমরা Dictionary use করব। যেমন একটা **car** এর বিভিন্ন properties থাকবে like color, price, model, manufacture year, brand etc...এই ক্ষেত্রে আমরা Dictionary use করব to **store different informations/properties of a single object**

```yaml
# Dictionary within Dictionary
Color: Blue
Model: 
  Name: Corvette
  Year: 1995
Transmission: Manual
Price: $20000
```

> এখানে Model হল **object** যার under এ Name, Year আছে 


#### Case 2

ধরি এখন আমি 6 টা cars এর names list করতে চাই by using cars colors and model numbem. এই ক্ষেত্রে আমরা array/list use করব for **storing multiple items of the same type of object**

```yaml
# list
- Blue Corvette
- Yellow Corvette
- Black Corvette
- Green Corvette
```

#### Case 3

কিন্তু এখন যদি এই প্রতিটা car er details information store করে রাখতে চাই? 

we need to **make dictionary inside list**

```yaml
# list of dictionaries
- Color: Blue
  Model: 
    Name: Corvette
    Year: 1995
 Transmission: Manual
 Price: $20000

 - Color: Yellow
  Model: 
    Name: Corvette
    Year: 1995
 Transmission: Manual
 Price: $20000

 - Color: Black
  Model: 
    Name: Corvette
    Year: 1995
 Transmission: Manual
 Price: $23000
```

##### Questions

```yaml
Fruits:
  - Orange
  - Apple
  - Banana
Vegetables:
  - Carrot
  - CauliFlower
  - Tomato
```

_How many array keys are there in the following yaml snippet?_

2
