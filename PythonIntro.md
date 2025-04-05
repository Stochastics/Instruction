
# Common Python Data Structures

| Data Structure | Ordered | Mutable | Allows Duplicates | Unique Keys/Values |
|----------------|---------|---------|-------------------|--------------------|
| List           | ✅      | ✅      | ✅                | ❌                |
| Tuple          | ✅      | ❌      | ✅                | ❌                |
| Set            | ❌      | ✅      | ❌                | ✅ (only unique elements) |
| Dictionary     | ✅ (3.7+) | ✅    | Keys unique       | ✅                |
| String         | ✅      | ❌      | ✅                | ❌                |

---

## 1. List
**Create:**
```python
my_list = [1, 2, 3, 4]
```

**Manipulate:**
```python
my_list.append(5)   # Add element
my_list.remove(2)   # Remove element
my_list[0] = 10     # Update element
```

---

## 2. Tuple
**Create:**
```python
my_tuple = (1, 2, 3)
```

**Manipulate:**
```python
# Tuples are immutable. To change, convert to a list
temp_list = list(my_tuple)
temp_list.append(4)
my_tuple = tuple(temp_list)
```

---

## 3. Set
**Create:**
```python
my_set = {1, 2, 3}
```

**Manipulate:**
```python
my_set.add(4)       # Add element
my_set.remove(2)    # Remove element
```

---

## 4. Dictionary (dict)
**Create:**
```python
my_dict = {"name": "Alice", "age": 25}
```

**Manipulate:**
```python
my_dict["age"] = 26         # Update value
my_dict["city"] = "Paris"   # Add new key-value pair
del my_dict["name"]         # Delete a key

value = my_dict.get("city")    # Get a value safely
keys = list(my_dict.keys())    # Get all keys
values = list(my_dict.values())  # Get all values
items = list(my_dict.items())    # Get all key-value pairs

"city" in my_dict    # Check if key exists (returns True)
```

---

## 5. String
**Create:**
```python
my_string = "Hello, World!"
```

**Manipulate:**
```python
upper_string = my_string.upper()    # Convert to uppercase
split_string = my_string.split(",") # Split into a list
```
