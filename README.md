# lajavamemomana

## 2. Values and References
### The final keyword.
When we say that the final Keyword means that a variable can only be assigned once, what we mean is that when the variable in this stack is assigned to an object in the heap, then we can't change which object in the heap this variable points to. The final Keyword stops us doing that. But we can change the object in the heap if we wish

## 5. Generational Garbage Collection

 We think of garbage collection as removing from the heap those objects which are no longer reachable. While this is the end result, it's the purpose of garbage collection modern garbage collectors use a clever mechanism for achieving it. Rather than searching for all the objects to remove, instead, the garbage collector looks for all the objects that need to be retained and it rescues them. The general algorithm that garbage collectors use is called mark and sweep. It's a two-stage process. The first stage is the marking, and the second stage is sweeping. In the marking stage, the program's execution is first paused. This is sometimes called a stop the world event. Marking cannot work properly if there are any threads still executing. So all the threads in the application are paused. The garbage collector then checks every single live reference. This is easy to do. It simply looks at every variable on the stack and follows its reference. The object that it finds at the end of the reference is marked as being alive. And it then follows any other references that that object has, and marks those as being alive, also. Once all the objects that are referenced are marked for keeping, a full scan of the heap takes place, and the stages of all of the objects is checked. The memory occupied by those objects not marked of in use, can then be freed up. And finally, the objects that are being kept, are moved into a single contiguous block of memory. This stops the heap from becoming fragmented over time and makes it easier and quicker for the virtual machine to find memory to allocate to future objects. So it's interesting to note that the garbage collector doesn't really collect any garbage. It actually collects the objects which are not eligible for garbage collection. This means that the garbage collection process is faster than bulk garbage --- If everything on the heap was garbage, well the the garbage collection process we perceive instantaneous
## 6. Tuning the Virtual Machine
### Garbage collection and generation sizes
```
-verbose:gc
```
 We know that you can check this using the Visual VM application, but if you want a quicker way just to monitor it, this might be useful. The argument is -verbose:gc. The verbose argument does have some other options you can output, but these aren't related to memory or garbage collection so I'm not going to cover those here. You can read the Java docs if you're interested. Just to see this one in action, I've come up with some simple code. You could pause the video and type this in if you'd like. It's not too exciting. What I've got is an array of customers and then a never ending loop that adds a new customer to the array. If the number of items in the array is going to the one hundred, then it removes 10 of the customers from the array. So we'll be, every so often, removing 10 items at a time. Those 10 items will then be eligible for garbage collection. I expect if we run this with a reasonably small heap, that we should see some garbage collections happening relatively frequently. If we right-click on the application, and do Run As and Run configurations, I think that option's just off-screen, and in the arguments I've typed in here a 10 megabyte heap, I want it to be very small. Then here's by -verbose:gc argument. So if we run this now, it's not terribly exciting. In fact you can see we've already had two garbage collections straight away. My application is still running. I imagine we will get another one in a moment. Then we've had a couple more now. And in fact they're becoming more frequent. So I guess that my application is probably starting to run out of space. If you're trying to fine tune your application, you might want to alter the size of the young generation. That is, how much of the heap is occupied by the young generation versus the old generation. By default the young generation is set to be a third of the total heap size. If your application, like the one we've just seen, has very few objects that are going to survive into the old generation, you might want to increase the size of the young generation. Oracle recommends that you size your young generation between a half and a quarter of the overall heap size. The option to set the size of the young generation is the argument -Xmn. This works just like the other - Xmx and -Xms arguments we've just seen. For example, if you wanted to set the young generation to 256 megabytes, you'd issue the command -Xmn256m. Note that this option sets the total size and the initial size of the young generation at the same time. So in this example you'd need your overall heap size and your initial heap size to both be sufficiently greater than this figure. If you set your initial heap size to be 512m, then when your application starts, half the heap will be for the young generation.
```
-Xmn256m 
```
### Generating Heap Dumps
```
-XX:HeapDumpOnOutOfMemory
```
