*funcmock* is a Function Mocking Framework in Go which replaces the lexical placeholder of the function with a mock. It is primarily intended to be used for testing purposes.

 Using funcmock
---------------

**Example:**

    package main
    
    import "funcmock"
    
    var funcToBeMocked = func(foo int, bar string) (int, float64) {
    	// I am computation intensive, hence take long
    	// Don't use me unnecessarily
    	return 2, 2.3
    
    }
    
    func iUseFuncToBeMockedTenTimes() {
    
    	for i := 0; i < 10; i++ {
    		_, _ = funcToBeMocked(1, "qux")
    	}
    }
    
    func main() {
    
    	// The mock engine specific to a function
    	funcMockEngine := funcmock.Mock(&funcToBeMocked)
    
    	// call iUseFuncToBeMockedTenTimes which calls funcToBeMocked
    	iUseFuncToBeMockedTenTimes() // actual funcToBeMocked is not called
    
    }
		

 * `Controller.CallCount()` returns the number of times the function was called

        // call iUseFuncToBeMockedTenTimes which calls funcToBeMocked
        iUseFuncToBeMockedTenTimes() 	// actual funcToBeMocked is not called
        fmt.Println("Mock function called ", funcMockEngine.CallCount(), " times.")	 
			 
	will print *Mock function called:  10*

 * `MockController.SetDefaultReturn` sets default return of the calls to the mock function which are not preset. This function can only be called once. (This behavior, of being able to just call it once, can be expected to change in future)

 * `MockController.Called()` returns a `bool` indicating whether the mock function was ever called or not.

 * `MockController.CallCount()` return a `int` indicating how many times the mock function was called.

 * `MockController.Restore()` returns the function to its original version. Mock function cannot be brought back after calling this. Further call the function will not invoke the mock or update the logs associated with the call to the mocked function, and will invoke the original function.

 * `MockController.NthParams(int)` returns a slice containing all parameters which the mock function has been called with in the nth position.

 * `MockController.NthReturns(int)` returns a slice containing all values the mock function has returned in the nth position.

 * `MockController.SetBehaviour(func)` sets a function to be called instead of the mock function. Must be called with a function of same type as the mock function.

 * `MockController.NthCall(int)` returns a `callHandle` struct instance of the nth call to the mock function since the mock was constructed. Does not have to be already called.

 * `callHandle.SetReturn` presets the return of a particular call of the mock function. (Of course, it should not have already been called, else it could not be of much use.)

 * `callHandle.Called()` returns `bool` indicating whether a particular call to the mock function was made or not.

 * `callHandle.NthParam(int)` returns the parameter that was used in a particular call to the mock function. Panics if the call in question has not been made.

 * `callHandle.NthReturn(int)` returns the value that was returned from that particular call. Panics if the call in question has not been made.