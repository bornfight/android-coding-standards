# Android standards


## 1. For sdk < 21 (Lollipop), use vector drawables only via support library
Otherwise, the app will fail at runtime because the vector drawables will be missing on Android versions < 21.

#### Do:

```xml
  <ImageView
  app:srcCompat="@drawable/vector_drawable" />
```

#### Don't:

```xml
  <ImageView
  android:src="@drawable/vector_drawable" />
```


#### Do:

```java
  textView.setCompoundDrawablesWithIntrinsicBounds(
    VectorDrawableCompat.create(resources, R.drawable.vector_drawable), 
    null, 
    null, 
    null);
```

#### Don't:

```xml
  <TextView
  android:drawableLeft="@drawable/vector_drawable" />
```

## 2. Take care after wrap_content WebViews
If you have a Webview that has wrap_content set as its height, Android Studio will throw a warning. This warning is here because there's a possibility that the content inside the webview will take longer to render, which may result in broken layout.

To avoid that, we can, firstly, secure that the layout will re-draw after the content is parsed. 

```java
mHtmlTextView.setWebViewClient(new WebViewClient() {
    @Override
    public void onPageFinished(WebView view, String url) {
        super.onPageFinished(view, url);
        mHtmlTextViewHolder.requestLayout();
    }
});
```
Next, you can disable the warning AS is giving you as follows:


```xml
<LinearLayout
    tools:ignore="WebViewLayout">
            
    <WebView
        android:layout_height="wrap_content" />
```


