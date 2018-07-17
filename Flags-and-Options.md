# Basic Usage

**--compilation_level (-O) VAL**  
Specifies the compilation level to use\. Options: BUNDLE, WHITESPACE_ONLY, SIMPLE \(default\), ADVANCED

**--env [BROWSER | CUSTOM]**  
Determines the set of builtin externs to load\. Options: BROWSER, CUSTOM\. Defaults to BROWSER\.

**--externs VAL**  
<sub><sup>*Not supported by the JavaScript version*</sup></sub>  
The file containing JavaScript externs\. You may specify multiple

**--js VAL**  
<sub><sup>*Not supported by the JavaScript version*</sup></sub>  
The JavaScript filename\. You may specify multiple\. The flag name is optional, because args are interpreted as files by default\. You may also use minimatch\-style glob patterns\. For example, use \-\-js='\*\*\.js' \-\-js='\!\*\*_test\.js' to recursively include all js files that do not end in _test\.js

**--js_output_file VAL**  
Primary output filename\. If not specified, output is written to stdout

**--language_in VAL**  
Sets the language spec to which input sources should conform\. Options: ECMASCRIPT3, ECMASCRIPT5, ECMASCRIPT5_STRICT, ECMASCRIPT6_TYPED \(experimental\), ECMASCRIPT_2015, ECMASCRIPT_2016, ECMASCRIPT_2017, ECMASCRIPT_NEXT

**--language_out VAL**  
Sets the language spec to which output should conform\. Options: ECMASCRIPT3, ECMASCRIPT5, ECMASCRIPT5_STRICT, ECMASCRIPT_2015

**--warning_level (-W) [QUIET | DEFAULT | VERBOSE]**  
Specifies the warning level to use\. Options: QUIET, DEFAULT, VERBOSE


# Warning and Error Management

**--conformance_configs VAL**  
<sub><sup>*Not supported by the JavaScript version*</sup></sub>  
A list of JS Conformance configurations in text protocol buffer format\.

**--error_format [STANDARD | JSON]**  
<sub><sup>*Not supported by the JavaScript version*</sup></sub>  
Specifies format for error messages\.

**--extra_annotation_name VAL**  
A whitelist of tag names in JSDoc\. You may specify multiple

**--hide_warnings_for VAL**  
If specified, files whose path contains this string will have their warnings hidden\. You may specify multiple\.

**--jscomp_error VAL**  
Make the named class of warnings an error\. Must be one of the error group items\. '\*' adds all supported\.

**--jscomp_off VAL**  
Turn off the named class of warnings\. Must be one of the error group items\. '\*' adds all supported\.

**--jscomp_warning VAL**  
Make the named class of warnings a normal warning\. Must be one of the error group items\. '\*' adds all supported\.

**--strict_mode_input**  
Assume input sources are to run in strict mode\.

**--warnings_whitelist_file VAL**  
<sub><sup>*Not supported by the JavaScript version*</sup></sub>  
A file containing warnings to suppress\. Each line should be of the form
                                                                                      \<file\-name\>:\<line\-number\>?  \<warning\-description\>

## Available Error Groups

  - accessControls
  - ambiguousFunctionDecl
  - checkRegExp
  - checkTypes
  - checkVars
  - conformanceViolations
  - const
  - constantProperty
  - deprecated
  - deprecatedAnnotations
  - duplicateMessage
  - es3
  - es5Strict
  - externsValidation
  - fileoverviewTags
  - functionParams
  - globalThis
  - internetExplorerChecks
  - invalidCasts
  - misplacedTypeAnnotation
  - missingGetCssName
  - missingOverride
  - missingPolyfill
  - missingProperties
  - missingProvide
  - missingRequire
  - missingReturn
  - moduleLoad
  - msgDescriptions
  - newCheckTypes
  - nonStandardJsDocs
  - missingSourcesWarnings
  - reportUnknownTypes
  - suspiciousCode
  - strictCheckTypes
  - strictMissingProperties
  - strictModuleDepCheck
  - strictPrimitiveOperators
  - typeInvalidation
  - undefinedNames
  - undefinedVars
  - unknownDefines
  - unusedLocalVariables
  - unusedPrivateMembers
  - uselessCode
  - useOfGoogBase
  - underscore
  - visibility

# Output

**--assume_function_wrapper**  
Enable additional optimizations based on the assumption that the output will be wrapped with a function wrapper\.  This flag is used to indicate that "global" declarations will not actually be global but instead isolated to the compilation unit\. This enables additional optimizations\.

**--debug**  
Enable debugging options\. Property renaming uses long mangled names which can be mapped back to the original name\.

**--export_local_property_definitions**  
Generates export code for local properties marked with @export

**--formatting [PRETTY_PRINT | PRINT_INPUT_DELIMITER | SINGLE_QUOTES]**  
Specifies which formatting options, if any, should be applied to the output JS\. Options: PRETTY_PRINT, PRINT_INPUT_DELIMITER, SINGLE_QUOTES

**--generate_exports**  
Generates export code for those marked with @export

**--isolation_mode [NONE | IIFE]**  
If set to IIFE the compiler output will follow the form:
                                                                                        \(function\(\)\{%output%\)\)\.call\(this\);
                                                                                      Options: NONE, IIFE

**--output_wrapper VAL**  
Interpolate output into this string at the place denoted by the marker token %output%\. Use marker token %output|jsstring% to do js string escaping on the output\. Consider using the \-\-isolation_mode flag instead\.

**--output_wrapper_file VAL**  
<sub><sup>*Not supported by the JavaScript version*</sup></sub>  
Loads the specified file and passes the file contents to the \-\-output_wrapper flag, replacing the value if it exists\. This is useful if you want special characters like newline in the wrapper\.

**--rename_prefix_namespace VAL**  
Specifies the name of an object that will be used to store all non\-extern globals

**--rename_variable_prefix VAL**  
Specifies a prefix that will be prepended to all variables\.


# Dependency Management

**--dependency_mode [NONE | LOOSE | STRICT]**  
Specifies how the compiler should determine the set and order of files for a compilation\. Options: NONE the compiler will include all src files in the order listed, STRICT files will be included and sorted by starting from namespaces or files listed by the \-\-entry_point flag \- files will only be included if they are referenced by a goog\.require or CommonJS require or ES6 import, LOOSE same as with STRICT but files which do not goog\.provide a namespace and are not modules will be automatically added as \-\-entry_point entries\. Defaults to NONE\.

**--entry_point VAL**  
A file or namespace to use as the starting point for determining which src files to include in the compilation\. ES6 and CommonJS modules are specified as file paths \(without the extension\)\. Closure\-library namespaces are specified with a "goog:" prefix\. Example: \-\-entry_point=goog:goog\.Promise


# JS Modules

**--js_module_root VAL**  
Path prefixes to be removed from ES6 & CommonJS modules\.

**--module_resolution [BROWSER | BROWSER_WITH_TRANSFORMED_PREFIXES | NODE | WEBPACK]**  
Specifies how the compiler locates modules\. BROWSER requires all module imports to begin with a '\.' or '/' and have a file extension\. NODE uses the node module rules\. WEBPACK looks up modules from a special lookup map\.

**--process_common_js_modules**  
Process CommonJS modules to a concatenable form\.

**--package_json_entry_names VAL**  
Ordered list of entries to look for in package\.json files when processing modules with the NODE module resolution strategy \(i\.e\. esnext:main,browser,main\)\. Defaults to a list with the following entries: "browser", "module", "main"\.


# Library and Framework Specific

**--angular_pass**  
Generate $inject properties for AngularJS for functions annotated with @ngInject

**--dart_pass**  
Rewrite Dart Dev Compiler output to be compiler\-friendly\.

**--force_inject_library VAL**  
Force injection of named runtime libraries\. The format is \<name\> where \<name\> is the name of a runtime library\. Possible libraries include: base, es6_runtime, runtime_type_check

**--inject_libraries**  
Allow injecting runtime libraries\.

**--polymer_version N**  
Which version of Polymer is being used \(1 or 2\)\.

**--process_closure_primitives**  
Processes built\-ins from the Closure library, such as goog\.require\(\), goog\.provide\(\), and goog\.exportSymbol\(\)\. True by default\.

**--rewrite_polyfills**  
Rewrite ES6 library calls to use polyfills provided by the compiler's runtime\.


# Code Splitting

**--chunk (--module) VAL**  
A JavaScript chunk specification\. The format is \<name\>:\<num\-js\-files\>\[:\[\<dep\>,\.\.\.\]\[:\]\]\]\. Chunk names must be unique\. Each dep is the name of a chunk that this chunk depends on\. Chunks must be listed in dependency order, and JS source files must be listed in the corresponding order\. Where \-\-chunk flags occur in relation to \-\-js flags is unimportant\. \<num\-js\-files\> may be set to 'auto' for the first chunk if it has no dependencies\. Provide the value 'auto' to trigger chunk creation from CommonJSmodules\.

**--chunk_output_path_prefix (--module_output_path_prefix) VAL**  
Prefix for filenames of compiled JS chunks\. \<chunk\-name\>\.js will be appended to this prefix\. Directories will be created as needed\. Use with \-\-chunk

**--chunk_wrapper (--module_wrapper) VAL**  
An output wrapper for a JavaScript chunk \(optional\)\. The format is \<name\>:\<wrapper\>\. The chunk name must correspond with a chunk specified using \-\-chunk\. The wrapper must contain %s as the code placeholder\. Alternately, %output% can be used in place of %s\. %n% can be used to represent a newline\. The %basename% placeholder can also be used to substitute the base name of the chunk output file\.


# Reports

**--create_source_map VAL**  
If specified, a source map file mapping the generated source files back to the original source file will be output to the specified path\. The %outname% placeholder will expand to the name of the output file that the source map corresponds to\.

**--output_manifest VAL**  
<sub><sup>*Not supported by the JavaScript version*</sup></sub>  
Prints out a list of all the files in the compilation\. If \-\-dependency_mode=STRICT or LOOSE is specified, this will not include files that got dropped because they were not required\. The %outname% placeholder expands to the JS output file\. If you're using modularization, using %outname% will create a manifest for each module\.

**--output_chunk_dependencies (--output_module_dependencies) VAL**  
<sub><sup>*Not supported by the JavaScript version*</sup></sub>  
Prints out a JSON file of dependencies between chunks\.

**--property_renaming_report VAL**  
<sub><sup>*Not supported by the JavaScript version*</sup></sub>  
File where the serialized version of the property renaming map produced should be saved

**--source_map_input VAL**  
<sub><sup>*Not supported by the JavaScript version*</sup></sub>  
Source map locations for input files, separated by a '|', \(i\.e\. input\-file\-path|input\-source\-map\)

**--source_map_include_content**  
Includes sources content into source map\. Greatly increases the size of source maps but offers greater portability

**--source_map_location_mapping VAL**  
<sub><sup>*Not supported by the JavaScript version*</sup></sub>  
Source map location mapping separated by a '|' \(i\.e\. filesystem\-path|webserver\-path\)

**--variable_renaming_report VAL**  
<sub><sup>*Not supported by the JavaScript version*</sup></sub>  
File where the serialized version of the variable renaming map produced should be saved


# Miscellaneous

**--charset VAL**  
<sub><sup>*Not supported by the JavaScript version*</sup></sub>  
Input and output charset for all files\. By default, we accept UTF\-8 as input and output US_ASCII

**--checks_only (--checks-only)**  
Don't generate output\. Run checks, but no optimization passes\.

**--define (--D, -D) VAL**  
Override the value of a variable annotated @define\. The format is \<name\>\[=\<val\>\], where \<name\> is the name of a @define variable and \<val\> is a boolean, number, or a single\-quoted string that contains no single quotes\. If \[=\<val\>\] is omitted, the variable is marked true

**--help**  
<sub><sup>*Not supported by the JavaScript version*</sup></sub>  
Displays this message on stdout and exit

**--third_party**  
<sub><sup>*Not supported by the JavaScript version*</sup></sub>  
Check source validity but do not enforce Closure style rules and conventions

**--use_types_for_optimization**  
Enable or disable the optimizations based on available type information\. Inaccurate type annotations may result in incorrect results\.

**--version**  
<sub><sup>*Not supported by the JavaScript version*</sup></sub>  
Prints the compiler version to stdout and exit\.