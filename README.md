Try to run BenchmarkFuture()

# Content

- func Future( anyfunc(resolvfunc, rejectfunc) ) (*Promise)
- func FutureDeferred( anyfunc(resolvfunc, rejectfunc) ) (exec func(bAsync bool), *Promise)
- func Race(...*Promise) (*Promise)
- Promise
	- Then(resolvedfunc, rejectedfunc) (*Promise)
	- OnSuccess(resolvedfunc) (*Promise)
	- OnFail(rejectedfunc) (*Promise)
	- Wait() 
	- SetCatch( recoverfn func(...interface{}) )
	- SetFinally( func(state, ...interface{}) )  // state: {resolved, rejected, recovered}

:: resolvedfunc and rejectedfunc can have any function signature

# Example 
```go
	q := Future( func(resolvFn, rejectFn){
		// ... do something
		resolvFn("success")
	})
	
	q.OnSuccess(fn1, fn2, fn3)
	q.OnFail(fn1, fn2)
	
	q.SetCatch( onRecoverFn )
	q.SetFinally( onDone )
	
	q.Wait()
```

# Example
```go
	exec, q :=
		FutureDeferred(func(
			resolve func(string, SomeAction) string,
			rejected func(interface{})) {

			resolve("message 1", SomeAction{})

		})


	q.Then(func(msg string, action SomeAction{}) (string, SomeType) {
		
		// handle msg and action

		return "message 2", SomeType{}
		
	}, func(fail interface{}) {

	}).Then(func(msg string, sometype SomeType) string {
		
		// handle msg and sometype
		return ""
		
	}, func(fail interface{}) {

	})

	bAsync := false
	exec(bAsync)

```
