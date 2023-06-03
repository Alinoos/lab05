# lab05
## Laboratory work V

Данная лабораторная работа посвещена изучению фреймворков для тестирования на примере **GTest**

```sh
$ open https://github.com/google/googletest
```

## Tasks

- [ ] 1. Создать публичный репозиторий с названием **lab05** на сервисе **GitHub**
- [ ] 2. Выполнить инструкцию учебного материала
- [ ] 3. Ознакомиться со ссылками учебного материала
- [ ] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```sh
$ export GITHUB_USERNAME=<имя_пользователя>
$ alias gsed=sed # for *-nix system
```

```sh
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
$ source scripts/activate
```

```sh
$ git clone https://github.com/${GITHUB_USERNAME}/lab04 projects/lab05
$ cd projects/lab05
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab05
```

```sh
$ mkdir third-party
$ git submodule add https://github.com/google/googletest third-party/gtest
$ cd third-party/gtest && git checkout release-1.8.1 && cd ../..
$ git add third-party/gtest
$ git commit -m"added gtest framework"
```
```sh
$ gsed -i '/option(BUILD_EXAMPLES "Build examples" OFF)/a\
option(BUILD_TESTS "Build tests" OFF)
' CMakeLists.txt
$ cat >> CMakeLists.txt <<EOF

if(BUILD_TESTS)
  enable_testing()
  add_subdirectory(third-party/gtest)
  file(GLOB \${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
  add_executable(check \${\${PROJECT_NAME}_TEST_SOURCES})
  target_link_libraries(check \${PROJECT_NAME} gtest_main)
  add_test(NAME check COMMAND check)
endif()
EOF
```

```sh
$ mkdir tests
$ cat > tests/test1.cpp <<EOF
#include <print.hpp>

#include <gtest/gtest.h>

TEST(Print, InFileStream)
{
  std::string filepath = "file.txt";
  std::string text = "hello";
  std::ofstream out{filepath};

  print(text, out);
  out.close();

  std::string result;
  std::ifstream in{filepath};
  in >> result;

  EXPECT_EQ(result, text);
}
EOF
```
```sh
$ cmake -H. -B_build -DBUILD_TESTS=ON
$ cmake --build _build
$ cmake --build _build --target test
```

```sh
$ _build/check
$ cmake --build _build --target test -- ARGS=--verbose
```

```sh
$ gsed -i 's/lab04/lab05/g' README.md
$ gsed -i 's/\(DCMAKE_INSTALL_PREFIX=_install\)/\1 -DBUILD_TESTS=ON/' .travis.yml
$ gsed -i '/cmake --build _build --target install/a\
- cmake --build _build --target test -- ARGS=--verbose
' .travis.yml
```

```sh
$ travis lint
```

```sh
$ git add .travis.yml
$ git add tests
$ git add -p
$ git commit -m"added tests"
$ git push origin master
```

```sh
$ travis login --auto
$ travis enable
```

```sh
$ mkdir artifacts
$ sleep 20s && gnome-screenshot --file artifacts/screenshot.png
# for macOS: $ screencapture -T 20 artifacts/screenshot.png
# open https://github.com/${GITHUB_USERNAME}/lab05
```

## Report

```sh
$ popd
$ export LAB_NUMBER=05
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gist REPORT.md
```

## Homework

### Задание
1. Создайте `CMakeList.txt` для библиотеки *banking*.

```sh
$ mkdir lab05
$ cd lab05
$ mkdir banking
$ git submodule add https://github.com/google/googletest third-party/gtest
Клонирование в «/home/alina/lab05/third-party/gtest»...
remote: Enumerating objects: 26402, done.
remote: Counting objects: 100% (9986/9986), done.
remote: Compressing objects: 100% (552/552), done.
remote: Total 26402 (delta 9596), reused 9437 (delta 9434), pack-reused 16416
Получение объектов: 100% (26402/26402), 12.09 МиБ | 10.44 МиБ/с, готово.
Определение изменений: 100% (19625/19625), готово.
$ cd third-party/gtest && git checkout release-1.8.1 && cd ../..
HEAD сейчас на 2fe3bd99 Merge pull request #1433 from dsacre/fix-clang-warnings
$ git add third-party
$ git commit -m "added gtest framework"
[master bc442f8] added gtest framework
 2 files changed, 4 insertions(+)
 create mode 100644 .gitmodules
 create mode 160000 third-party/gtest
$ git push origin master
Username for 'https://github.com': Alinoos
Password for 'https://Alinoos@github.com': 
Перечисление объектов: 12, готово.
Подсчет объектов: 100% (12/12), готово.
При сжатии изменений используется до 8 потоков
Сжатие объектов: 100% (10/10), готово.
Запись объектов: 100% (12/12), 2.05 КиБ | 2.05 МиБ/с, готово.
Всего 12 (изменений 0), повторно использовано 0 (изменений 0), повторно использовано пакетов 0
remote: This repository moved. Please use the new location:
remote:   https://github.com/Alinoos/lab05.git
remote: 
remote: Create a pull request for 'master' on GitHub by visiting:
remote:      https://github.com/Alinoos/lab05/pull/new/master
remote: 
To https://github.com/alinoos/lab05.git
 * [new branch]      master -> master
$ cd banking
$ cat >> CMakeLists.txt << EOF
>EOF
$ nano CMakeLists.txt
project(banking_lib)

if (NOT TARGET libbanking)
    add_library(libbanking STATIC
        /Account.cpp
        /Transaction.cpp
    )

    install(TARGETS libbanking
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib
    )
endif(NOT TARGET libbanking)

include_directories()
$ git add CMakeLists.txt
$ git commit -m "CMake - 1 - 1"
[master 2b71558] CMake - 1 - 1
 1 file changed, 15 insertions(+)
 create mode 100644 banking/CMakeLists.txt
 $ git push origin master
Username for 'https://github.com': Alinoos
Password for 'https://Alinoos@github.com': 
Перечисление объектов: 6, готово.
Подсчет объектов: 100% (6/6), готово.
При сжатии изменений используется до 8 потоков
Сжатие объектов: 100% (4/4), готово.
Запись объектов: 100% (4/4), 503 байта | 503.00 КиБ/с, готово.
Всего 4 (изменений 2), повторно использовано 0 (изменений 0), повторно использовано пакетов 0
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
remote: This repository moved. Please use the new location:
remote:   https://github.com/Alinoos/lab05.git
To https://github.com/alinoos/lab05.git
   bc442f8..2b71558  master -> master
$ cd ..
$ cat >> CMakeLists.txt << EOF
> EOF
$ nano CMakeLists.txt
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_TEST "Build tests" OFF)

project(banking)

add_library(banking STATIC banking/Account.cpp banking/Transaction.cpp)
target_include_directories(banking PUBLIC banking/)

if(BUILD_TESTS)
  enable_testing()
  add_subdirectory(third-party/gtest)
  file(GLOB BANKING_TEST_SOURCES tests/tests.cpp)
  add_executable(check tests/tests.cpp)
  target_link_libraries(check banking gtest_main gmock_main)
  add_test(NAME check COMMAND check)
endif()

$ git add CMakeLists.txt
$ git commit -m "CMake - 1"
[master cdaa130] CMake - 1
 1 file changed, 20 insertions(+)
 create mode 100644 CMakeLists.txt
$ git push origin master
Username for 'https://github.com': Alinoos
Password for 'https://Alinoos@github.com': 
Перечисление объектов: 4, готово.
Подсчет объектов: 100% (4/4), готово.
При сжатии изменений используется до 8 потоков
Сжатие объектов: 100% (3/3), готово.
Запись объектов: 100% (3/3), 602 байта | 602.00 КиБ/с, готово.
Всего 3 (изменений 1), повторно использовано 0 (изменений 0), повторно использовано пакетов 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
remote: This repository moved. Please use the new location:
remote:   https://github.com/Alinoos/lab05.git
To https://github.com/alinoos/lab05.git
   2b71558..cdaa130  master -> master
```

2. Создайте модульные тесты на классы `Transaction` и `Account`.
    * Используйте mock-объекты.
    * Покрытие кода должно составлять 100%.

```sh

$ mkdir tests
$ cd tests
$ cat >> tests.cpp << EOF
> EOF
$ nano tests.cpp

#include "Account.h"
#include "Transaction.h"

#include <gtest/gtest.h>
#include <gmock/gmock.h>

class AccountMock : public Account {
public:
    AccountMock(int id, int balance) : Account(id, balance) {}
    MOCK_CONST_METHOD0(GetBalance, int());
    MOCK_METHOD1(ChangeBalance, void(int diff));
    MOCK_METHOD0(Lock, void());
    MOCK_METHOD0(Unlock, void());
};
class TransactionMock : public Transaction {
public:
    MOCK_METHOD3(Make, bool(Account& from, Account& to, int sum));
};

TEST(Account, Mock) {
    AccountMock acc(1, 100);
    EXPECT_CALL(acc, GetBalance()).Times(1);
    EXPECT_CALL(acc, ChangeBalance(testing::_)).Times(2);
    EXPECT_CALL(acc, Lock()).Times(2);
    EXPECT_CALL(acc, Unlock()).Times(1);
    acc.GetBalance();
    acc.ChangeBalance(100); // throw
    acc.Lock();
    acc.ChangeBalance(100);
    acc.Lock(); // throw
    acc.Unlock();
}

TEST(Account, SimpleTest) {
    Account acc(1, 100);
    EXPECT_EQ(acc.id(), 1);
    EXPECT_EQ(acc.GetBalance(), 100);
    EXPECT_THROW(acc.ChangeBalance(100), std::runtime_error);
    EXPECT_NO_THROW(acc.Lock());
    acc.ChangeBalance(100);
    EXPECT_EQ(acc.GetBalance(), 200);
    EXPECT_THROW(acc.Lock(), std::runtime_error);
}

TEST(Transaction, Mock) {
    TransactionMock tr;
    Account ac1(1, 50);
    Account ac2(2, 500);
    EXPECT_CALL(tr, Make(testing::_, testing::_, testing::_))
    .Times(6);
    tr.set_fee(100);
    tr.Make(ac1, ac2, 199);
    tr.Make(ac2, ac1, 500);
    tr.Make(ac2, ac1, 300);
    tr.Make(ac1, ac1, 0); // throw
    tr.Make(ac1, ac2, -1); // throw
    tr.Make(ac1, ac2, 99); // throw
}

TEST(Transaction, SimpleTest) {
    Transaction tr;
    Account ac1(1, 50);
    Account ac2(2, 500);
    tr.set_fee(100);
    EXPECT_EQ(tr.fee(), 100);
    EXPECT_THROW(tr.Make(ac1, ac1, 0), std::logic_error);
    EXPECT_THROW(tr.Make(ac1, ac2, -1), std::invalid_argument);
    EXPECT_THROW(tr.Make(ac1, ac2, 99), std::logic_error);
    EXPECT_FALSE(tr.Make(ac1, ac2, 199));
    EXPECT_FALSE(tr.Make(ac2, ac1, 500));
    EXPECT_FALSE(tr.Make(ac2, ac1, 300));
}

$ git add tests.cpp
$ git commit -m "Tests - 1"
[master f9f1123] Tests - 1
 1 file changed, 72 insertions(+)
 create mode 100644 tests/tests.cpp
$  git push origin master
Username for 'https://github.com': Alinoos
Password for 'https://Alinoos@github.com': 
Перечисление объектов: 5, готово.
Подсчет объектов: 100% (5/5), готово.
При сжатии изменений используется до 8 потоков
Сжатие объектов: 100% (3/3), готово.
Запись объектов: 100% (4/4), 894 байта | 894.00 КиБ/с, готово.
Всего 4 (изменений 1), повторно использовано 0 (изменений 0), повторно использовано пакетов 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
remote: This repository moved. Please use the new location:
remote:   https://github.com/Alinoos/lab05.git
To https://github.com/alinoos/lab05.git
   cdaa130..f9f1123  master -> master
$ cd ..

```
3. Настройте сборочную процедуру на **TravisCI**.
```sh
$ mkdir .github
$ cd ~/lab05/.github
$ mkdir workflows
$ cd ~/lab05/.github/workflows
$ cat >> Action.yml << EOF
>EOF
$ nano Action.yml
name: CMake

on:
 push:
  branches: [master]
 pull_request:
  branches: [master]

jobs:
 build_Linux:

  runs-on: ubuntu-latest

  steps:
  - uses: actions/checkout@v3

  - name: Adding gtest
    run: git clone https://github.com/google/googletest.git third-party/gtest -b release-1.11.0

  - name: Install lcov
    run: sudo apt-get install -y lcov

  - name: Config banking with tests
    run: cmake -H. -B ${{github.workspace}}/build -DBUILD_TESTS=ON

  - name: Build banking
    run: cmake --build ${{github.workspace}}/build

  - name: Run tests
    run: |
      build/check
      cmake --build ${{github.workspace}}/build --target test -- ARGS=--verbose

$ git add Action.yml
$ git commit -m "Action - 1"
[master 88b774a] Action - 1
 1 file changed, 32 insertions(+)
 create mode 100644 .github/workflows/Action.yml
$ git push origin master
Username for 'https://github.com': Alinoos
Password for 'https://Alinoos@github.com': 
Перечисление объектов: 6, готово.
Подсчет объектов: 100% (6/6), готово.
При сжатии изменений используется до 8 потоков
Сжатие объектов: 100% (3/3), готово.
Запись объектов: 100% (5/5), 699 байтов | 699.00 КиБ/с, готово.
Всего 5 (изменений 1), повторно использовано 0 (изменений 0), повторно использовано пакетов 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
remote: This repository moved. Please use the new location:
remote:   https://github.com/Alinoos/lab05.git
To https://github.com/alinoos/lab05.git
   f9f1123..88b774a  master -> master
$ cd ~
```
4. Настройте [Coveralls.io](https://coveralls.io/).
```sh
$ cat >> lcov.info << EOF
>EOF

$ git add lcov.info
$ git commit -m "lcov - 1"
[master d254cf0] lcov - 1
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 coverage/lcov.info
$ git push origin master
Username for 'https://github.com': Alinoos
Password for 'https://Alinoos@github.com': 
Перечисление объектов: 5, готово.
Подсчет объектов: 100% (5/5), готово.
При сжатии изменений используется до 8 потоков
Сжатие объектов: 100% (2/2), готово.
Запись объектов: 100% (4/4), 312 байтов | 312.00 КиБ/с, готово.
Всего 4 (изменений 1), повторно использовано 0 (изменений 0), повторно использовано пакетов 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
remote: This repository moved. Please use the new location:
remote:   https://github.com/Alinoos/lab05.git
To https://github.com/alinoos/lab05.git
   0032b4e..d254cf0  master -> master
$ cd ..
$ nano CMakeLists.txt 
$ git add CMakeLists.txt
$ git commit -m "CMake - 2"
[master 05761a3] CMake - 2
 1 file changed, 13 insertions(+)

cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_TEST "Build tests" OFF)

if(BUILD_TESTS)
  add_compile_options(--coverage)
endif()

option(COVERAGE "Check coverage" ON)

project(banking)

add_library(banking STATIC banking/Account.cpp banking/Transaction.cpp)
target_include_directories(banking PUBLIC banking/)

target_link_libraries(banking gcov)

if(BUILD_TESTS)
  enable_testing()
  add_subdirectory(third-party/gtest)
  file(GLOB BANKING_TEST_SOURCES tests/tests.cpp)
  add_executable(check tests/tests.cpp)
  target_link_libraries(check banking gtest_main gmock_main)
  add_test(NAME check COMMAND check)
endif()

if (COVERAGE)
	target_compile_options(check PRIVATE --coverage)
	target_link_libraries(check --coverage)
endif()
$ git commit -m "CMake - 2"
$ git push origin master
[master 05761a3] CMake - 2
 1 file changed, 13 insertions(+)
$ git push origin master
Username for 'https://github.com': Alinoos
Password for 'https://Alinoos@github.com': 
Перечисление объектов: 5, готово.
Подсчет объектов: 100% (5/5), готово.
При сжатии изменений используется до 8 потоков
Сжатие объектов: 100% (3/3), готово.
Запись объектов: 100% (3/3), 432 байта | 432.00 КиБ/с, готово.
Всего 3 (изменений 2), повторно использовано 0 (изменений 0), повторно использовано пакетов 0
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
remote: This repository moved. Please use the new location:
remote:   https://github.com/Alinoos/lab05.git
To https://github.com/alinoos/lab05.git
   d254cf0..05761a3  master -> master
$ cd ~/lab-05/.github/workflows   
$ cd ..
$ cd ~/lab05/.github/workflows
$ nano Action.yml


name: CMake

on:
 push:
  branches: [master]
 pull_request:
  branches: [master]

jobs:
 build_Linux:

  runs-on: ubuntu-latest

  steps:
  - uses: actions/checkout@v3

  - name: Adding gtest
    run: git clone https://github.com/google/googletest.git third-party/gtest -b release-1.11.0

  - name: Install lcov
    run: sudo apt-get install -y lcov

  - name: Config banking with tests
    run: cmake -H. -B ${{github.workspace}}/build -DBUILD_TESTS=ON

  - name: Build banking
    run: cmake --build ${{github.workspace}}/build

  - name: Run tests
    run: |
      build/check
      cmake --build ${{github.workspace}}/build --target test -- ARGS=--verbose

  - name: Do lcov stuff
    run: lcov -c -d build/CMakeFiles/banking.dir/banking/ --include *.cpp --output-file ./coverage/lcov.info

  - name: Publish to coveralls.io
    uses: coverallsapp/github-action@v1.1.2
    with:
      github-token: ${{ secrets.GITHUB_TOKEN }}

$ git add Action.yml
$ git commit -m "Action - 2"
[master 4b519e5] Action - 2
 1 file changed, 8 insertions(+)
$ git push origin master
Username for 'https://github.com': Alinoos
Password for 'https://Alinoos@github.com': 
Перечисление объектов: 9, готово.
Подсчет объектов: 100% (9/9), готово.
При сжатии изменений используется до 8 потоков
Сжатие объектов: 100% (3/3), готово.
Запись объектов: 100% (5/5), 594 байта | 594.00 КиБ/с, готово.
Всего 5 (изменений 2), повторно использовано 0 (изменений 0), повторно использовано пакетов 0
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
remote: This repository moved. Please use the new location:
remote:   https://github.com/Alinoos/lab05.git
To https://github.com/alinoos/lab05.git
 05761a3..4b519e5  master -> master
$ cd ..
$ nano README.md 
[![Coverage Status](https://coveralls.io/repos/github/Alinoos/laba05/badge.svg?branch=master)](https://coveralls.io/github/Alinoos/lab05?branch=master)
$ git add README.md
$ git commit -m "README - 2"
[master ee0f601] README - 2
 1 file changed, 2 insertions(+)
 create mode 100644 .github/README.md
 $ git push origin master
Username for 'https://github.com': Alinoos
Password for 'https://Alinoos@github.com': 
Перечисление объектов: 6, готово.
Подсчет объектов: 100% (6/6), готово.
При сжатии изменений используется до 8 потоков
Сжатие объектов: 100% (4/4), готово.
Запись объектов: 100% (4/4), 455 байтов | 455.00 КиБ/с, готово.
Всего 4 (изменений 1), повторно использовано 0 (изменений 0), повторно использовано пакетов 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
remote: This repository moved. Please use the new location:
remote:   https://github.com/Alinoos/lab05.git
To https://github.com/alinoos/lab05.git
   4b519e5..ee0f601  master -> master
```



## Links

- [C++ CI: Travis, CMake, GTest, Coveralls & Appveyor](http://david-grs.github.io/cpp-clang-travis-cmake-gtest-coveralls-appveyor/)
- [Boost.Tests](http://www.boost.org/doc/libs/1_63_0/libs/test/doc/html/)
- [Catch](https://github.com/catchorg/Catch2)

```
Copyright (c) 2015-2021 The ISC Authors
```
