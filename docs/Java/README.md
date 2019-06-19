# Java standards

## 1. Iterating through map entries in order
Don't think of some custom sorting logic for this.
There's a neat data structure you can use to iterate map entries in the order which they were added in.
[LinkedHashMap](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedHashMap.html)

##
## 2. Getting ConcurrentModificationException? 
This is a legitimate exception, so think whether this implementation is appropriate before putting it in your codebase. The exception is most likely thrown because you're trying to delete something **while** iterating a collection, which can mess up th iterator logic.
To remedy this, you can use a thread-safe implementation of ArrayList, [CopyOnWriteArrayList](https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/CopyOnWriteArrayList.html).


