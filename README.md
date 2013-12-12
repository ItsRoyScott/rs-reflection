# rs-reflection

Cross-compiler, header-only reflection library with a modern C++11 design.

[type_meta](README.md#type_meta)<br>
[type_name](README.md#type_name)<br>

## field_desc
Describes a static or member field of an object.

```cpp
class field_desc
{
public:
  field_meta const& meta() const;
  any wrap() const;
  any wrap(T& obj) const;
};
```

## function_desc
Describes a static or member function of an object.

```cpp
class function_desc
{
public: // functions
  R call<R>(args...);
  function_meta const& meta() const;
  
public: // operators
  any operator()(args...); // NOTE: will convert C++ type or 'any' type to properly call function
};
```

## type_desc
Describes a type with getters for function and field descriptions.

```cpp
class type_desc 
{
public: // types
  template <typename T>
  class member_container {
  public:
    const_iterator begin() const;
    const_iterator end() const;
    const_iterator find(std::string const& name) const;
    std::size_t size() const;
  };
  
public: // functions
  member_container<field_desc> const& fields() const;
  member_container<function_desc> const& functions() const;
  type_meta const& meta() const;
  type_name const& name() const;
};
```

## type_meta
Stores type_traits information for a type.

```cpp
class type_meta 
{
public: // types
  enum class conversion_result : unsigned int {
    const_to_nonconst,            // conversion loses const qualifier
    lvalue_to_rvalue,             // cannot bind an lvalue to an rvalue reference
    valid,                        // conversion valid
    value_to_nonconst_reference,  // a non-const reference may only be bound to an lvalue
    volatile_to_nonvolatile,      // conversion loses volatile qualifier
  };
  
  enum class trait : unsigned int {
    has_virtual_destructor,
    is_arithmetic,
    is_array,
    is_const,
    is_const_reference,
    is_enum,
    is_floating_point,
    is_function,
    is_integral,
    is_literal_type,
    is_lvalue_reference,
    is_pointer,
    is_polymorphic,
    is_reference,
    is_rvalue_reference,
    is_signed,
    is_standard_layout,
    is_trivial,
    is_union,
    is_unsigned,
    is_void,
    is_volatile,
  };

public: // functions
  type_meta const& decay() const;
  std::string format_conversion_result<T>(conversion_result result) const;
  std::string format_conversion_result(conversion result, type_meta const& tm) const;
  static type_meta const& get<T>();
  bool has_trait(trait t) const;
  bool is_convertible_from<T>() const;
  bool is_convertible_from<T>(conversion_result& result) const;
  bool is_convertible_from(type_meta const& tm) const;
  bool is_convertible_from(type_meta const& tm, conversion_result& result) const;
  type_name const& name() const;
  
public: // operators
  bool operator==(type_meta const& b) const;
  bool operator!=(type_meta const& b) const;
  bool operator< (type_meta const& b) const;
  bool operator<=(type_meta const& b) const;
  bool operator> (type_meta const& b) const;
  bool operator>=(type_meta const& b) const;
  friend std::ostream& operator<<(std::ostream& os, type_meta const& tm);
};
```

## type_name
Replacement for type_info which provides O(1) getters for a type's name, namespace name, and hash code.

```cpp
class type_name
{
public: // functions
  static type_name const& get<T>();
  std::size_t hash_code() const;
  std::size_t index() const;
  std::pair<char const*, std::size_t> name() const; // TODO: replace with conststring
  std::pair<char const*, std::size_t> name_space() const;
  
public: // operators
  bool operator==(type_name const& b) const;
  bool operator!=(type_name const& b) const;
  bool operator< (type_name const& b) const;
  bool operator<=(type_name const& b) const;
  bool operator> (type_name const& b) const;
  bool operator>=(type_name const& b) const;
  friend std::ostream& operator<<(std::ostream& os, type_name const& tn);
};
```

