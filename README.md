# meminfo parser

## Introduction

A simple header-only tool for parsing `/proc/meminfo` information

## Requirements

* C++17
* Linux

## Usage

```C++
#include "meminfo_parser.hpp"
#include <iostream>

int main() {
    mp::Meminfo mi;
    if(!mp::meminfo_parse(mi)) {
        mp::dump(mi);

        // or you can
        std::cout << "MemTotal: " << mi.field.MemTotal << std::endl;
        std::cout << "MemFree: " << mi.arr[1] << std::endl;
    }
    return 0;
}
```

```C++
#include "meminfo_parser.hpp"
#include <vector>

int main() {
    mp::Meminfo before, after;
    if(mp::meminfo_parse(before)) {
        return EXIT_FAILURE;
    }

    constexpr size_t MAXN = 1e7;
    std::vector<uint64_t> pf(MAXN);
    // so we can observe the page fault
    for(auto &v : pf) pf[0] += v;

    if(mp::meminfo_parse(after)) {
        return EXIT_FAILURE;
    }
    // ignore values with difference less than 100 kB
    mp::dump_diff(before, after, 100);
    return 0;
}
```

## Notes

If you are failed in `mp::meminfo_parse(meminfo)` (returns a non-zero value), you can try `sh ./generator.sh` to generate a custom version `meminfo_parser_for_you.hpp` for your linux environment.
