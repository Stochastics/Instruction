# Using `while` vs `for` Loops in Python

Understanding when to use a `while` loop vs a `for` loop can help you write clearer, more efficient code. Here are examples and explanations for both.

## `for` Loop

Use a `for` loop when you know ahead of time how many times you want to iterate, or when iterating over a sequence (like a list, tuple, or range).

### Example 1: Iterating over a range
```python
for i in range(5):
    print(i)
```
*Use case: You want to run a block of code 5 times.*

### Example 2: Iterating over a list
```python
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)
```
*Use case: You want to perform an action for each item in a list.*

## `while` Loop

Use a `while` loop when the number of iterations is not known beforehand and the loop should continue based on a condition.

### Example 1: Real-world condition-based loop (monitoring server status)
```python
import time

def check_server():
    # Simulate checking server status
    from random import choice
    return choice(["online", "offline"])

status = check_server()
while status != "online":
    print("Server is offline. Retrying in 5 seconds...")
    time.sleep(5)
    status = check_server()

print("Server is online!")
```
*Use case: You want to keep checking until an external condition changes (like a server coming online). You don't know how many attempts it will take.*

### Example 2: Waiting for user input
```python
user_input = ""
while user_input != "exit":
    user_input = input("Type 'exit' to quit: ")
```
*Use case: You want to keep prompting the user until they type a specific word.*

## Summary

| Use Case                           | Use `for` Loop | Use `while` Loop |
| ---------------------------------- | -------------- | ---------------- |
| Known number of iterations         | ✅              |                  |
| Iterating over a sequence          | ✅              |                  |
| Indeterminate number of iterations |                | ✅                |
| Loop controlled by a condition     |                | ✅                |

Use the loop that makes your code more readable and matches the intent of your logic.

## Extra Tip
If you're writing a `while` loop that could easily be rewritten as a `for` loop (like looping through a list or counting to 10), it's usually better to use a `for` loop for clarity. Use `while` when you're dealing with unknowns or external signals like user input, server responses, sensor data, etc.
