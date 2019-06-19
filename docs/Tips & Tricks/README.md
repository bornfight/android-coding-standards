## Tips & tricks

## 1. In DBFlow, for nested OneToMany or ManyToMany relations, the save(), delete() etc. methods will not be called on the list of related objects. Disable efficientMethods for those methods to be called.


If there's an Ant object with it's own OneToMany or ManyToMany relation (list of Leaf objects), two outcomes are possible.

```java
   public class Ant extends BaseModel {

      @OneToMany(methods = {OneToMany.Method.ALL}, variableName = "leaves")
      public List<Leaf> getLeaves() { ... }
```

#### a) Nested relation objects of an Ant will NOT be saved when called Queen.save():

```java
   public class Queen extends BaseModel {

      @OneToMany(methods = {OneToMany.Method.ALL}, variableName = "ants", efficientMethods = true)
      public List<Ant> getMyAnts() { ... }
```

#### b) Nested relation objects of an Ant object will be saved when called Queen.save():

```java
   public class Queen extends BaseModel {

      @OneToMany(methods = {OneToMany.Method.ALL}, variableName = "ants", efficientMethods = false)
      public List<Ant> getMyAnts() { ... }
```

##
## 2. Since Kotlin only does shallow copy, if you're ever in a need of a deep copy, here's how to do it!


For class such as this one:
```kotlin
   data class User(val name: String = "", val age: Int = 0)
```
the copy implementation would be:
```kotlin
   fun copy(name: String = this.name, age: Int = this.age) = User(name, age)
```
So, as obvious, it' a **shallow** copy. **Deep** copy implementation would look like this:
```kotlin
   fun copy(a: Int = this.a, bar: Bar = this.bar, list: MutableList<Int> = this.list) = Foo(a, bar, list)
   fun copy(x: Int = this.x) = Bar(x)
```
More details about this topic can be found at: https://stackoverflow.com/questions/47359496/kotlin-data-class-copy-method-not-deep-copying-all-members/47361217#47361217


##
## 3. Customizing Android X Button?
With the release of the new Android X Support Library, a couple of changes related to component customization have also come to life. For instance, when defining a Button, setting a selector as the background resource will not longer do the job. In fact, it may even result in some unwanted behaviour.

To fix the issue, background, stroke, text color etc., are now defined with their own selectors, which are joined to button separatelly via proper attributes:

#### Text color (button_text.xml):
```xml
   <selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:color="@color/colorAccent" android:state_enabled="false" />
    <item android:color="@color/white" />
   </selector>
```
#### Background color (button_background.xml):
```xml
   <selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:color="#0000" android:state_enabled="false" />
    <item android:color="@color/colorAccent" />
   </selector>
```
#### Button definition:
```xml
   <Button
       android:layout_width="match_parent"
       android:layout_height="wrap_content"
       android:textColor="@color/button_text"
       app:backgroundTint="@color/button_background"
       app:rippleColor="@color/white"
       app:strokeColor="@color/colorAccent" />
```

More details about this can be found at: https://material.io/develop/android/components/material-button/

##
## 4. Updating View Pager Content with ```notifyDataSetChanged()```.

If you have your View Pager displayed, content set, and it's time for update which you plan to do by calling ```notifyDataSetChanged()```, you won't be able to do it (app will still work; content just won't get updated) unless you override the following method in your View Pager adapter:
```kotlin
   override fun getItemPosition(obj: Any): Int {
        // notifyDataSetChanged() won't work unless this method is overridden.
        return PagerAdapter.POSITION_NONE
    }
```

##
## 5. Extend the Android Studio logcat capacity
If you’re losing debug info because logcat can’t remember enough text (e.g. due to some huge api responses taking up the buffer), you can increase the value of ```
idea.cycle.buffer.size``` in property file ```android-studio\bin\idea.properties```, for example to:
```
idea.cycle.buffer.size=1024
```
