# TrampolineHook
A solution for centralized method redirection

### Usage

The interface of TrampolineHook is very very simple. Only two steps are required.

1. Create the global interceptor with your centralized function as follows:

```
// Suppose you have a function defined as
- (void)myInterceptor:(void)
{
    // bla bla bal
}

// Create the global inteceptor with your function
THInterceptor *interceptor = [THInterceptor sharedInterceptorWithFunction:(IMP)myInterceptor]

```

2. Intercept any function you want no matter what the method signature it is.

``` 
// Suppose you want to intercept the call of - [UIView initWithFrame:]
Method m = class_getInstanceMethod([UIView class], @selector(initWithFrame:));
IMP imp = method_getImplementation(m);

// Intercept the imp
THInterceptorResult *result = [interceptor interceptFunction:imp];

// You can check the result.state to find whether the inteception is successfully carried out or not.
result.state == THInterceptStateSuccess
```

### How to debug

The debug of interception is not very easy. 

** Remember always use the `result.replacedAddress` returned from `interceptFunction` for breakpoint.**
** Remember always use the `result.replacedAddress` returned from `interceptFunction` for breakpoint.**
** Remember always use the `result.replacedAddress` returned from `interceptFunction` for breakpoint.**

### Example

There is one typical example associated with this open source project called **MainThreadChecker**。It is the rewritten version of the implementation in **Apple libMainThreadChecker.dylib**

It is almost the same as the one of Apple based on my own reverse engineering. **No Private APIs used.** 

The usage of MainThreadChecker is quite easy.

```
+ (void)load 
{
    [MTInitializer enableMainThreadChecker]
}
```

### TODO
- [] API Stability.
- [] Varadic Argument Interceptor.
- [] More examples.
- [] Performance Benchmark.
