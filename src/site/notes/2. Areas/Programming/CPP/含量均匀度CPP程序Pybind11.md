---
{"date":"2022-09-26","time":"16:17","tags":[],"cssclass":null,"dg-publish":true,"dg-class":"program","permalink":"/2-areas/programming/cpp/cpp-pybind11/","dgHomeLink":true,"dgPassFrontmatter":true,"dgShowLocalGraph":true,"dgShowBacklinks":true,"dgShowInlineTitle":true}
---

## 1. 封装 CUTest
将中美药典含量均匀度检测方法分别封装为类，在类内添加成员函数，返回含量均匀度检测的结果。由于在美国药典中，需要用到类似 `np.all` 的方法，因此在成员函数中，选择`Eigen`库的`Array`类型，而不是`std::array`或者`std::vector`之类的标准库作为函数返回的接口。`Eigen::Array`可以执行逐元素的运算，非常适合该案例。

```cpp
// CUTest.hpp

#pragma once
#include <Eigen/Dense>
#include <EigenRand/EigenRand>
#include <chrono>
#include <cmath>
#include <random>
#include <iostream>

/// @brief standard deviation with 1 degree of freedom (unbiased)
/// @param array an Eigen array
/// @param ddof the degree of freedom, default is 1
/// @return the standard deviation of the array
double std_ddof(Eigen::ArrayXd array, int ddof);

class ChPTest {
public:
    // members
    Eigen::Array<double, 10, 1> s1;
    Eigen::Array<double, 20, 1> s2;
    Eigen::Array<double, 30, 1> stage_2;
    double k1{ 2.2 };
    double k2{ 0.5 };
    double k3{ 1.7 };
    double L{ 15 };
    double std_s1;
    double std_stage2;
    double mean_s1;
    double mean_stage2;

    Eigen::Array4d get_result();

    // constructor function
    ChPTest(const Eigen::ArrayXd& s1_in, const Eigen::ArrayXd& s2_in);
    ChPTest(const Eigen::ArrayXd& s1_in, const Eigen::ArrayXd& s2_in, const double k1_in, const double k2_in, const double k3_in, const double L_in);
    ~ChPTest();
};

class USPTest {
public:
    Eigen::Array<double, 10, 1> s1;
    Eigen::Array<double, 20, 1> s2;
    Eigen::Array<double, 30, 1> stage_2;
    double k1{ 2.4 };
    double k2{ 2.0 };
    double L1{ 15.0 };
    double L2{ 25.0 };
    double std_s1;
    double std_stage2;
    double mean_s1;
    double mean_stage2;

    // constructor function
    USPTest(const Eigen::ArrayXd& s1_in, const Eigen::ArrayXd& s2_in);
    USPTest(const Eigen::ArrayXd& s1_in, const Eigen::ArrayXd& s2_in, double k1_in, double k2_in, double L1_in, double L2_in);
    ~USPTest();
    // member function
    Eigen::Array4d get_result();
};
```
创建了两个构造函数，一个包括了默认参数，一个不包括，其实两个函数都是一样的。

```cpp
// CUTest.cpp

#include "CUTest.hpp"

// standard deviation with 1 degree of freedom (unbiased)
double std_ddof(Eigen::ArrayXd array, int ddof=1) {
    double standard_deviation = std::sqrt((array - array.mean()).square().sum() / (array.size() - ddof));
    return standard_deviation;
}

ChPTest::ChPTest(const Eigen::ArrayXd& s1_in, const Eigen::ArrayXd& s2_in) {
    // check length error
    if (s1_in.size() != 10) {
        throw("The length of s1 must be 10!");
    }
    if (s2_in.size() != 20) {
        throw("The legnth of s2 must be 20!");
    }
    s1 = s1_in;
    s2 = s2_in;
    stage_2 << s1, s2;
    mean_s1 = s1.mean();
    std_s1 = std_ddof(s1, 1);
    mean_stage2 = stage_2.mean();
    std_stage2 = std_ddof(stage_2, 1);
}

ChPTest::ChPTest(const Eigen::ArrayXd& s1_in, const Eigen::ArrayXd& s2_in, const double k1_in, const double k2_in, const double k3_in, const double L_in) {
    // check length error
    if (s1_in.size() != 10) {
        throw("The length of s1 must be 10!");
    }
    if (s2_in.size() != 20) {
        throw("The legnth of s2 must be 20!");
    }

    s1 = s1_in;
    s2 = s2_in;
    k1 = k1_in;
    k2 = k2_in;
    k3 = k3_in;
    L = L_in;
    stage_2 << s1, s2;
    mean_s1 = s1.mean();
    std_s1 = std_ddof(s1, 1);
    mean_stage2 = stage_2.mean();
    std_stage2 = std_ddof(stage_2, 1);
}

ChPTest::~ChPTest() { }

//defintion of member function get_result
Eigen::Array4d ChPTest::get_result() {
    int pass_s1 = 0;
    int pass_s2 = 0;
    int fail_s1 = 0;
    int fail_s2 = 0;

    double Ave = std::abs(100 - mean_s1);
    if (Ave + k1 * std_s1 <= L) {
        pass_s1 = 1;
    }
    else if (Ave + std_s1 > L) {
        fail_s1 = 1;
    }
    else {
        Ave = std::abs(100 - mean_stage2);
        if (Ave <= k2 * L) {
            if (pow(Ave, 2) + pow(std_stage2, 2) <= k2 * pow(L, 2)) {
                pass_s2 = 1;
            }
            else {
                fail_s2 = 1;
            }
        }
        else
        {
            if (Ave + k3 * std_stage2 <= L) {
                pass_s2 = 1;
            }
            else {
                fail_s2 = 1;
            }
        }
    }
    Eigen::Array4d result;
    result << pass_s1, pass_s2, fail_s1, fail_s2;
    return result;
}


USPTest::USPTest(const Eigen::ArrayXd& s1_in, const Eigen::ArrayXd& s2_in) {
    // check length error
    if (s1_in.size() != 10) {
        throw("The length of s1 must be 10!");
    }
    if (s2_in.size() != 20) {
        throw("The legnth of s2 must be 20!");
    }

    s1 = s1_in;
    s2 = s2_in;
    stage_2 << s1, s2;
    mean_s1 = s1.mean();
    std_s1 = std_ddof(s1, 1);
    mean_stage2 = stage_2.mean();
    std_stage2 = std_ddof(stage_2, 1);
}

USPTest::USPTest(const Eigen::ArrayXd& s1_in, const Eigen::ArrayXd& s2_in, double k1_in, double k2_in, double L1_in, double L2_in)
{
    // check length error
    if (s1_in.size() != 10) {
        throw("The length of s1 must be 10!");
    }
    if (s2_in.size() != 20) {
        throw("The legnth of s2 must be 20!");
    }

    s1 = s1_in;
    s2 = s2_in;
    k1 = k1_in;
    k2 = k2_in;
    L1 = L1_in;
    L2 = L2_in;
    stage_2 << s1, s2;
    mean_s1 = s1.mean();
    std_s1 = std_ddof(s1, 1);
    mean_stage2 = stage_2.mean();
    std_stage2 = std_ddof(stage_2, 1);
}

USPTest::~USPTest() {}

// return Eigen::Array4d [pass_s1, pass_s2, fail_s1, fail_s2]
Eigen::Array4d USPTest::get_result() {
    int pass_s1 = 0;
    int pass_s2 = 0;
    int fail_s1 = 0;  //no fail in stage 1, indeed
    int fail_s2 = 0;

    double Ave = 0;
    double Ave2 = 0;
    double limit = 0;
    if (mean_s1 < 98.5) {
        Ave = 98.5 - mean_s1;
    }
    else if (mean_s1 > 101.5) {
        Ave = mean_s1 - 101.5;
    }
    else {
        Ave = 0;
    }

    // stage 1 test
    if (Ave + k1 * std_s1 <= L1) {
        pass_s1 = 1;
    }
    else {
        if (mean_stage2 < 98.5) {
            Ave2 = 98.5 - mean_stage2;
            limit = 98.5;
        }
        else if (mean_stage2 > 101.5) {
            Ave2 = mean_stage2 - 101.5;
            limit = 101.5;
        }
        else {
            Ave2 = 0;
            limit = mean_stage2;
        }

        if ((Ave2 + k2 * std_stage2 <= L1) && (stage_2 >= (1 - 0.01 * L2) * limit).all() && (stage_2 <= (1 + 0.01 * L2) * limit).all()) {
            pass_s2 = 1;
        }
        else {
            fail_s2 = 1;
        }
    }
    Eigen::Array4d result;
    result << pass_s1, pass_s2, fail_s1, fail_s2;
    return result;
}
```

>[!notes]
>1. 需要注意的是，在 `stage_2 << s1, s2;` 这一句中，若 `stage_2` 的长度大于 30，则会取 `[s1, s2]` 前 30 个数，剩下的数以初始值填充；若小于 30，则取前若干个数将 `stage2` 填充。因此，若 `stage2` 的长度不是 30，编译器==并不会报错==。
>2. 此外，在传参的时候，`s1` 和 `s2` 的形参是引用类型，但其实在函数内做了拷贝，似乎不用引用对性能也影响不大。
>3. 之前想将 `std_ddof` 作为内联函数，但发现性能并没有提升，反而略有下降。此外，若是作为函数模板，需要将声明和定义一起放在头文件中。
