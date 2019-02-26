# Tips & tricks

## 1. In DBFlow, for nested OneToMany or ManyToMany relations, the save(), delete() etc. methods will not be called on the list of related objects. Disable efficientMethods for those methods to be called.


### If there's an Ant object with it's own OneToMany or ManyToMany relation (list of Leaf objects), two outcomes are possible.

```java
   public class Ant extends BaseModel {

      @OneToMany(methods = {OneToMany.Method.ALL}, variableName = "leaves")
      public List<Leaf> getLeaves() { ... }
```

### a) Nested relation objects of an Ant will NOT be saved when called Queen.save():

```java
   public class Queen extends BaseModel {

      @OneToMany(methods = {OneToMany.Method.ALL}, variableName = "ants", efficientMethods = true)
      public List<Ant> getMyAnts() { ... }
```

### b) Nested relation objects of an Ant object will be saved when called Queen.save():

```java
   public class Queen extends BaseModel {

      @OneToMany(methods = {OneToMany.Method.ALL}, variableName = "ants", efficientMethods = false)
      public List<Ant> getMyAnts() { ... }
```


## 2. Since Kotlin only does shallow copy, if you're ever in a need of a deep copy, here's how to do it!


### For class such as this one:
```kotlin
   data class User(val name: String = "", val age: Int = 0)
```
### the copy implementation would be:
```kotlin
   fun copy(name: String = this.name, age: Int = this.age) = User(name, age)
```
### So, as obvious, it' a shallow copy. Deep copy implementation would look like this:
```kotlin
   fun copy(a: Int = this.a, bar: Bar = this.bar, list: MutableList<Int> = this.list) = Foo(a, bar, list)
   fun copy(x: Int = this.x) = Bar(x)
```
### More details about this topic can be found at: https://stackoverflow.com/questions/47359496/kotlin-data-class-copy-method-not-deep-copying-all-members/47361217#47361217

