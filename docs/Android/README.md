# Android standards

## 1. For minSdkVersion lower than 21 (Lollipop), use vector drawables only via support library. Otherwise, the app will fail at runtime because the vector drawables will be missing on Android versions < 21.

### Do:

```xml
  <ImageView
  app:srcCompat="@drawable/vector_drawable" />
```

### Don't:

```xml
  <ImageView
  android:src="@drawable/vector_drawable" />
```


### Do:

```java
  textView.setCompoundDrawablesWithIntrinsicBounds(
    VectorDrawableCompat.create(resources, R.drawable.vector_drawable), 
    null, 
    null, 
    null);
```

### Don't:

```xml
  <TextView
  android:drawableLeft="@drawable/vector_drawable" />
```
