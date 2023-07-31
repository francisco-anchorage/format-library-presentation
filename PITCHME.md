---
marp: true
title: Format library in C++20
author: Francisco Carrola
theme: gaia
class:
  - lead
size: 16:9
paginate: true
---
<style>
/* got this from Anchorage-Brand-Guidelines-may2023.pdf */
  :root {
    --color-background: #E0E0E0;
    --color-foreground: #000000;
    --color-highlight: #5681F6;
    --color-dimmed: #5681F6;
  }
</style>

# `std::format` in C++20

C++ Study Session

July 31st, 2023

![width:400px](./assets/logo.png)

---

<!-- header: '' -->
<!-- footer: '' -->

## A bit of History...

Through its almost 40 years of history, C++ has had multiple “tries” to bring **text formatting** to the language.

---

### `printf()`, `sprintf()` and similar functions

```c++
printf("Invalid %s address length: got (%zu), expected (%u)",
      asset_type_str(ETH),
      address.size(),
      ETHLIKE_LAST_KECCAK_USED_HEX_BYTES_WITH_PREFIX);
```

* Advantages:
  - Simple to use
  - Efficient
* Disadvantages:
  - Not type-safe - format specifier **must** match the variable type
  - Not extensible - can only be called for the supported types

---

### `std::cout` and `std::stringstream` streams

```c++
std::cout << "Invalid " << asset_type_str(ETH)
          << " address length: got " << address.size()
          << ", expected " << ETHLIKE_LAST_KECCAK_USED_HEX_BYTES_WITH_PREFIX;
```

* Advantages:
  - Type-safe
  - Extensible - by operator overloading
* Disadvantages:
  - Bad performance
  - Very verbose

---

### Introducing `std::format` library

* It is a subset of the `fmtlib/fmt` library (GH repo [here](https://github.com/fmtlib/fmt))
* Created by Victor Zverovich (first GH release in Feb 7, 2015)
  ![width:800px](./assets/victor-linkedin.png)
* Proposed to the C++ commitee in 2016
* Integration in the standard library approved in 2019, after **10 revisions**!

---

### Library's main features

* Python-like string formatting
* Type-safe formatting
* **No more format specifiers**!
  ```c++
  std::format("Invalid {} address length: got {}, expected {}",
              asset_type_str(ETH),
              address.size(),
              ETHLIKE_LAST_KECCAK_USED_HEX_BYTES_WITH_PREFIX);
  ```

---

* :heavy_exclamation_mark: but we don't want to call `asset_type_str` every time we want to print a certain `asset_type`
* **Solution:** add support for user-defined types

  ```c++
  template <>
  struct std::formatter<asset_type_t> : std::formatter<const char*> {

    auto format(asset_type_t asset_type, auto& ctx) const -> decltype(ctx.out()) {
      using base = formatter<const char*>;
      return base::format(asset_type_str(asset_type), ctx);
    }
  };
  ```

---

# Let's check how can this be useful in our codebase

---

## References

* [Good summary of history and main features](https://madridccppug.github.io/posts/stdformat/)
* [Blog post by Victor himself on the proposal timeline](https://www.zverovich.net/2019/07/23/std-format-cpp20.html)
* [Text formatting in C++ using libc++](https://blog.llvm.org/posts/2022-08-14-libc++-format/)

---

# Any questions or comments?
