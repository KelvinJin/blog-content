---
title: Swiftc - Linux 环境下编译 Swift
permalink: swiftc-linux-huan-jing-xia-bian-yi-swift
id: 40
updated: '2016-02-29 21:39:35'
tags:
---

```bash
# swiftc -help
OVERVIEW: Swift compiler

USAGE: swiftc [options] <inputs>

MODES:
  -dump-ast        Parse and type-check input file(s) and dump AST(s)
  -dump-parse      Parse input file(s) and dump AST(s)
  -dump-type-refinement-contexts
                   Type-check input file(s) and dump type refinement contexts(s)
  -emit-assembly   Emit assembly file(s) (-S)
  -emit-bc         Emit LLVM BC file(s)
  -emit-executable Emit a linked executable
  -emit-ir         Emit LLVM IR file(s)
  -emit-library    Emit a linked library
  -emit-object     Emit object file(s) (-c)
  -emit-sibgen     Emit serialized AST + raw SIL file(s)
  -emit-sib        Emit serialized AST + canonical SIL file(s)
  -emit-silgen     Emit raw SIL file(s)
  -emit-sil        Emit canonical SIL file(s)
  -parse           Parse input file(s)
  -print-ast       Parse and type-check input file(s) and pretty print AST(s)

OPTIONS:
  -application-extension  Restrict code to those available for App Extensions
  -assert-config <value>  Specify the assert_configuration replacement. Possible values are Debug, Release, Replacement.
  -D <value>              Specifies one or more build configuration options
  -embed-bitcode-marker   Embed placeholder LLVM IR data as a marker
  -embed-bitcode          Embed LLVM IR bitcode as data
  -emit-dependencies      Emit basic Make-compatible dependencies files
  -emit-module-path <path>
                          Emit an importable module to <path>
  -emit-module            Emit an importable module
  -emit-objc-header-path <path>
                          Emit an Objective-C header file to <path>
  -emit-objc-header       Emit an Objective-C header file
  -fixit-all              Apply all fixits from diagnostics without any filtering
  -fixit-code             Get compiler fixits as code edits
  -framework <value>      Specifies a framework which should be linked against
  -F <value>              Add directory to framework search path
  -gline-tables-only      Emit minimal debug info for backtraces only
  -gnone                  Don't emit debug info
  -g                      Emit debug info
  -help                   Display available options
  -import-underlying-module
                          Implicitly imports the Objective-C half of a module
  -I <value>              Add directory to the import search path
  -j <n>                  Number of commands to execute in parallel
  -L <value>              Add directory to library link search path
  -l<value>               Specifies a library which should be linked against
  -module-cache-path <value>
                          Specifies the Clang module cache path
  -module-link-name <value>
                          Library to link against when using this module
  -module-name <value>    Name of the module to build
  -nostdimport            Don't search the standard library import path for modules
  -num-threads <n>        Enable multi-threading and specify number of threads
  -Onone                  Compile without any optimization
  -Ounchecked             Compile with optimizations and remove runtime safety checks
  -output-file-map <path> A file which specifies the location of outputs
  -O                      Compile with optimizations
  -o <file>               Write output to <file>
  -parse-as-library       Parse the input file(s) as libraries, not scripts
  -parse-sil              Parse the input file as SIL code, not Swift source
  -parseable-output       Emit textual output in a parseable format
  -profile-coverage-mapping
                          Generate coverage data for use with profiled execution counts
  -profile-generate       Generate instrumented code to collect execution counts
  -save-temps             Save intermediate compilation results
  -sdk <sdk>              Compile against <sdk>
  -serialize-diagnostics  Serialize diagnostics in a binary format
  -suppress-warnings      Suppress all warnings
  -target-cpu <value>     Generate code for a particular CPU variant
  -target <value>         Generate code for the given target
  -version                Print version information and exit
  -v                      Show commands to run and use verbose output
  -warnings-as-errors     Treat warnings as errors
  -whole-module-optimization
                          Optimize input files together instead of individually
  -Xcc <arg>              Pass <arg> to the C/C++/Objective-C compiler
  -Xlinker <value>        Specifies an option which should be passed to the linker
```