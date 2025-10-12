# ⏱️ ishap — Tiny Reusable Fixed-Timestep Runner

<p align="center">
  <a href="LICENSE"><img alt="License: MIT" src="https://img.shields.io/badge/License-MIT-blue.svg"></a>
  <a href="https://en.cppreference.com/w/cpp/compiler_support"><img alt="C++17+" src="https://img.shields.io/badge/C%2B%2B-17%2B-orange.svg"></a>
  <img alt="Header-only" src="https://img.shields.io/badge/Header--only-yes-success.svg">
  <a href="https://cmake.org"><img alt="CMake 3.15+" src="https://img.shields.io/badge/CMake-3.15%2B-informational.svg"></a>
  <img alt="Lines of Code" src="https://img.shields.io/badge/LOC-%3C500-lightgrey.svg">
</p>

---

> **ishap** is a minimal, header-only fixed–timestep runner for deterministic update loops.  
> It provides a clear, unit-safe interface using `std::chrono`, robust clamping, scaling, and substep controls — all with zero dependencies.  
> A simple, reusable utility built for engines, simulations, and real-time systems — written in an afternoon at a cozy countryside café.

---

### ✨ Features

| | |
|:-|:-|
| 🕒 **Fixed timestep stepping** — via `tick()` or `push_time()` |
| 🎯 **Deterministic updates** when fed explicit `dt` |
| ⚙️ **Unit-safe API** with `std::chrono` |
| 🧩 **Time scaling**, **max delta clamp**, **substep cap** |
| 🚫 **Exception-safe** — internal try/catch maintains `noexcept` |
| 💡 **Error hook support** for caught exceptions |
| 🧱 **Header-only** — no external dependencies |

---

### 🧭 Example Usage

```cpp
#include <ishap/ishap.hpp>
#include <iostream>
using namespace std::chrono_literals;

int main() {
    ishap::timestep::FixedTimestepRunner runner{
        [](std::chrono::nanoseconds dt) {
            // Fixed update logic
            std::cout << "Fixed step: " << dt.count() << "ns\n";
        },
        {.step = 16ms, .safety_max_substeps = 8}
    };

    // Game loop
    while (true) {
        const double alpha = runner.tick(); // or runner.push_time(frame_dt)
        // render(interpolate(alpha));
    }
}
```

---

### ⚙️ Configuration Overview

| Field | Type | Default | Description |
|:--|:--|:--|:--|
| `step` | `std::chrono::nanoseconds` | ~16.67 ms | Fixed update timestep |
| `time_scale` | `double` | `1.0` | Speed multiplier (`0.0` = paused) |
| `safety_max_delta` | `std::chrono::nanoseconds` | 250 ms | Clamp for large frame gaps |
| `safety_max_substeps` | `size_t` | 8 | Maximum fixed steps per tick |
| `safety_max_accumulator_overflow` | `size_t` | 3 | Accumulator clamp multiplier |

---

### 🧱 CMake Integration

#### Option 1 — FetchContent
```cmake
include(FetchContent)
FetchContent_Declare(
  ishap
  GIT_REPOSITORY https://github.com/mickryley/ishap.git
  GIT_TAG        v0.1.0
)
FetchContent_MakeAvailable(ishap)

target_link_libraries(your_target PRIVATE ishap::ishap)
```

#### Option 2 — Installed Package
```cmake
find_package(ishap CONFIG REQUIRED)
target_link_libraries(your_target PRIVATE ishap::ishap)
```

---

### 🧰 Requirements
- CMake **3.15+**
- C++17 or later (`cxx_std_17` minimum; tested with C++20 / 23)
- Header-only — no compiled sources required

---

### 📦 Directory Layout
```
ishap/
├─ include/
│  └─ ishap/
│     └─ ishap.hpp
├─ CMakeLists.txt
├─ LICENSE
└─ README.md
```

---

### 🧩 Example Error Hook
```cpp
runner.set_error_function([] {
    std::cerr << "Step error caught — simulation continued safely.\n";
});
```

---

### 🧾 License
**MIT License** — see [`LICENSE`](LICENSE).  
SPDX Identifier: `MIT`

---

**ishap** is a compact, deterministic, and expressive timestep runner —  
a dependable foundation for simulations, engines, and real-time systems.