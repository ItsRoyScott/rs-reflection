# rs-reflection

Cross-compiler, header-only reflection library with a modern C++11 design.

[type_meta](README.md#type_meta)<br>
[type_name](README.md#type_name)<br>

## type_meta
Stores type_traits information for a type.

+ 

  ```cpp
  type_meta const& decay() const
  ```

+ 

  ```cpp
  type_meta const& get<T>()
  ```

+ 

  ```cpp
  std::string get_conversion_result_string<T>(conversion_result result) const
  ```
  
+ 

  ```cpp
  std::string get_conversion_result_string(conversion_result result, type_meta const& a) const
  ```

+ 
  
  ```cpp
  type_name const& get_name() const
  ```

+ 
  
  ```cpp
  bool has_trait(trait t) const
  ```
  
+ 
  
  ```cpp
  bool is_implicitly_convertible<T>() const
  ```
  
+ 
  
  ```cpp
  bool is_implicitly_convertible<T>(conversion_result& result) const
  ```

+ 
  
  ```cpp
  bool is_implicitly_convertible(type_meta const& a) const
  ```
  
+ 
  
  ```cpp
  bool is_implicitly_convertible<T>(type_meta const& a, conversion_result& result) const
  ```

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

+ 

  ```cpp
  enum class trait : unsigned int
  {
    has_virtual_destructor = 1 << 0,
    is_arithmetic = 1 << 1,
    is_array = 1 << 2,
    is_const = 1 << 3,
    is_const_reference = (1<<3)|(1<<9)|(1<<12),
    is_enum = 1 << 4,
    is_floating_point = 1 << 5,
    is_function = 1 << 6,
    is_integral = 1 << 7,
    is_literal_type = 1 << 8,
    is_lvalue_reference = 1 << 9,
    is_pointer = 1 << 10,
    is_polymorphic = 1 << 11,
    is_reference = (1<<9)|(1<<12),
    is_rvalue_reference = 1 << 12,
    is_signed = 1 << 13,
    is_standard_layout = 1 << 14,
    is_trivial = 1 << 15,
    is_union = 1 << 16,
    is_unsigned = 1 << 17,
    is_void = 1 << 18,
    is_volatile = 1 << 19,
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
