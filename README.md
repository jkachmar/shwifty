# Shwifty

## Generate Swift types from Haskell types

Examples: 

#### A simple sum type
```haskell
data SumType = Sum1 | Sum2 | Sum3
getShwifty ''SumType
```

```swift
enum SumType {
    case sum1
    case sum2
    case sum3
}
```

#### A simple product type
```haskell
data ProductType = ProductType { x :: Int, y :: Int }
getShwifty ''ProductType
```

```swift
struct ProductType {
    let x: Int
    let y: Int
}
```

#### A sum type with type variables
```haskell
data SumType a b = SumL a | SumR b
getShwifty ''SumType
```

```swift
enum SumType<A, B> {
    case sumL(A)
    case sumR(B)
}
```

#### A product type with type variables
```haskell
data ProductType a b = ProductType 
  { aField :: a
  , bField :: b 
  }
getShwifty ''ProductType
```

```swift
struct ProductType<A, B> {
    let aField: A
    let bField: B
}
```

#### A newtype
```haskell
newtype Newtype a = Newtype { getNewtype :: a }
getShwifty ''Newtype
```

```swift
struct Newtype<A> {
    let getNewtype: A
}
```

#### A type with a function field
```haskell
newtype Endo a = Endo { appEndo :: a -> a }
getShwifty ''Endo
```

```swift
struct Endo<A> {
    let appEndo: ((A) -> A)
}
```

#### A weird type with nested fields. Also note the Result's types being flipped from that of the Either.
```haskell
data YouveGotProblems a b = YouveGotProblems 
  { field1 :: Maybe (Maybe (Maybe a))
  , field2 :: Either (Maybe a) (Maybe b) 
  }
getShwifty ''YouveGotProblems
```

```swift
struct YouveGotProblems<A, B> {
    let field1: A???
    let field2: Result<B?, A?>
}
```

#### A type with polykinded type variables
```haskell
data PolyKinded (a :: k) = PolyKinded
getShwifty ''PolyKinded
```

```swift
struct PolyKinded<A> { }
```

#### A sum type where constructors might be records
```haskell
data SumType a b (c :: k) 
  = Sum1 Int a (Maybe b) 
  | Sum2 b 
  | Sum3 { x :: Int, y :: Int }
getShwifty ''SumType
```

```swift
enum SumType<A, B, C> {
  case field1(Int, A, B?)
  case field2(B)
  case field3(_ x: Int, _ y: Int)
}
```

#### A type containing another type with instance generated by 'getShwifty'
```haskell
newtype MyFirstType a = MyFirstType { getMyFirstType :: a }
getShwifty ''MyFirstType

data Contains a = Contains 
  { x :: MyFirstType Int
  , y :: MyFirstType a 
  }
getShwifty ''Contains
```

```swift
struct MyFirstType<A> {
  let getMyFirstType: A
}

struct Contains<A> {
  let x: MyFirstType<Int>
  let y: MyFirstType<A>
}
```
