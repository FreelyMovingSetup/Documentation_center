## Waiter

*Example :*

???+ note "Note : static"
    Static is an argument to create a variable that will remain the same between function calls.
    Static var declaration is run only once so we don't mind putting it inside the "root" of the loop function.

```c++
// loop run code repeatedly
void loop() {
  
  static waiter memo = waiter(1000, waiter::millisec, true);
  
  if (memo()){ // check if a memo is 
    memo.wait(1000);
    Serial.println("DueTime");
  }
}
```

### Constructor :



```c++
waiter waiter_object;
```

Called 