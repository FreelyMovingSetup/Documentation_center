!!! info "Purpose"
    This class is meant to be used as a way to make easier the process of waiting for events in a non blocking way, when the necessity of instantaneous capture of transient events is not required (in which case, the use of hardware interrupts is preferable)

## Example Usage :

??? note "Note : static"
    Static is an argument to create a variable that will remain the same between function calls.
    Static var declaration is run only once so we don't mind putting it inside the "root" of the loop function.

```c++
// loop run code repeatedly
void loop() {
  
  static waiter waiter_object = waiter(1000, waiter::millisec, true);
  
  if (waiter_object()){ // check if a memo is 
    waiter_object.wait(1000);
    Serial.println("DueTime");
  }
}
```

## Constructor :



```c++
waiter waiter_object;
```

Called 