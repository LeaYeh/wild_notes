# Meta Programming

## What is Meta Programming?

* **Metaprogramming** is a programming technique in which computer programs have the ability to treat other programs as their data.
* With metaprogramming, you can write code that generates code at runtime. It means during the compilation phase, you can generate code based on custom conditions.
* Metaprogramming is useful in scenarios where you need to perform computations or make decisions at compile time rather than runtime.

## Scenarios where Metaprogramming can be useful:

### Type Traits and Type Checking

Example: is_integral Type Trait

```cpp
template <typename T>
struct ascending {
    static bool compare(const T& a, const T& b) {
        return a < b;
    }
};

template <typename T>
struct descending {
    static bool compare(const T& a, const T& b) {
        return a > b;
    }
};

template <typename T, typename Policy>
void sort(T* array, int size) {
    for (int i = 0; i < size - 1; ++i) {
        for (int j = 0; j < size - i - 1; ++j) {
            if (Policy::compare(array[j], array[j + 1])) {
                std::swap(array[j], array[j + 1]);
            }
        }
    }
}

// Usage
#include <iostream>

int main() {
    int arr[] = {5, 2, 9, 1, 5, 6};
    sort<int, ascending<int>>(arr, 6);
    for (int i = 0; i < 6; ++i) {
        std::cout << arr[i] << " "; // Outputs: 1 2 5 5 6 9
    }
    std::cout << std::endl;

    sort<int, descending<int>>(arr, 6);
    for (int i = 0; i < 6; ++i) {
        std::cout << arr[i] << " "; // Outputs: 9 6 5 5 2 1
    }
    std::cout << std::endl;

    return 0;
}

```
