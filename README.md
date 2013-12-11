# rs-reflection

Cross-compiler, header-only reflection library with modern C++11 design.

[blah](README.md#type_meta)

## type_meta
Stores type_traits information for a type.

+ 

  ```cpp
  enum class conversion_result : unsigned int
  {
    const_to_nonconst,            // conversion loses const qualifier
    lvalue_to_rvalue,             // cannot bind an lvalue to an rvalue reference
    valid,                        // conversion valid
    value_to_nonconst_reference,  // a non-const reference may only be bound to an lvalue
    volatile_to_nonvolatile,      // conversion loses volatile qualifier
  };
  ```


## type_name
Replacement for type_info which provides O(1) getters for a type's name, namespace name, and hash code.

+ 

  ```cpp
  static type_name const& get<T>()
  ```

+ 
```cpp
std::size_t hash_code() const
```

+ 
```cpp
std::size_t index() const
```

+ 
```cpp
char const* name() const
```

+ 
```cpp
std::size_t name_length() const
```

+ 
```cpp
char const* name_space() const
```

+ 
```cpp
std::size_t name_space_length() const
```

+ 
```cpp
friend std::ostream& operator<<(std::ostream& os, type_name const& tn)
```

+ Relational operators which compare by hash value
