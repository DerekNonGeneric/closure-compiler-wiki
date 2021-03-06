Graal `native-image` builds have dramatically faster startup time.  However, for pinned JVMs (nail gun, etc) or long running binaries (roughly ~5 minutes or greater, depending on available resources), the JVM's dynamic tuning will eventually out-perform the native-image builds.

Currently, June 2018, `native-image` is not available on all platforms (being limited to linux and MacOS) and can not be cross compiled, limiting where it can be used.

As of June 18, 2018: build native-image from head as the release has a bug to causes the build to timeout.  See https://github.com/oracle/graal/issues/428

```
native-image --static --no-server \
  -H:+ReportUnsupportedElementsAtRuntime \
  -H:IncludeResourceBundles=com.google.javascript.rhino.Messages \
  -H:IncludeResourceBundles=com.google.javascript.jscomp.CompilerConfig \
  -H:IncludeResourceBundles=com.google.javascript.jscomp.parsing.ParserConfig \
  -H:ReflectionConfigurationFiles="${PWD}/reflectconfig" \
  -H:IncludeResources='.*(js|txt)' -H:EnableURLProtocols=jar \
  -jar default_deploy.jar
```

reflectconfig:

```
[
  {
    "name" : "com.google.javascript.jscomp.CommandLineRunner$Flags",
    "allDeclaredFields" : true
  },
  {
    "name" : "com.google.javascript.jscomp.CommandLineRunner$Flags$BooleanOptionHandler",
    "allDeclaredConstructors" : true,
    "allDeclaredMethods": true
  },
  {
    "name" : "com.google.javascript.jscomp.CommandLineRunner$Flags$WarningGuardErrorOptionHandler",
    "allDeclaredConstructors" : true,
    "allDeclaredMethods": true
  },
  {
    "name" : "com.google.javascript.jscomp.CommandLineRunner$Flags$WarningGuardWarningOptionHandler",
    "allDeclaredConstructors" : true,
    "allDeclaredMethods": true
  },
  {
    "name" : "com.google.javascript.jscomp.CommandLineRunner$Flags$WarningGuardOffOptionHandler",
    "allDeclaredConstructors" : true,
    "allDeclaredMethods": true
  },
  {
    "name" : "com.google.javascript.jscomp.CommandLineRunner$Flags$JsOptionHandler",
    "allDeclaredConstructors" : true,
    "allDeclaredMethods": true
  },
  {
    "name" : "com.google.javascript.jscomp.CommandLineRunner$Flags$JsZipOptionHandler",
    "allDeclaredConstructors" : true,
    "allDeclaredMethods": true
  },
  {
    "name" : "com.google.javascript.jscomp.AbstractCommandLineRunner$JsonFileSpec",
    "allDeclaredConstructors" : true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name" : "org.kohsuke.args4j.CmdLineParser",
    "allDeclaredConstructors" : true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name" : "org.kohsuke.args4j.Option",
    "allDeclaredConstructors" : true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name" : "org.kohsuke.args4j.OptionDef",
    "allDeclaredConstructors" : true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name" : "org.kohsuke.args4j.spi.FieldSetter",
    "allDeclaredConstructors" : true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name" : "org.kohsuke.args4j.spi.Setter",
    "allDeclaredConstructors" : true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name": "org.kohsuke.args4j.spi.BooleanOptionHandler",
    "allDeclaredConstructors": true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name": "org.kohsuke.args4j.spi.ByteOptionHandler",
    "allDeclaredConstructors": true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name": "org.kohsuke.args4j.spi.CharOptionHandler",
    "allDeclaredConstructors": true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name": "org.kohsuke.args4j.spi.DoubleOptionHandler",
    "allDeclaredConstructors": true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name": "org.kohsuke.args4j.spi.EnumOptionHandler",
    "allDeclaredConstructors": true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name": "org.kohsuke.args4j.spi.FileOptionHandler",
    "allDeclaredConstructors": true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name": "org.kohsuke.args4j.spi.FloatOptionHandler",
    "allDeclaredConstructors": true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name": "org.kohsuke.args4j.spi.InetAddressOptionHandler",
    "allDeclaredConstructors": true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name": "org.kohsuke.args4j.spi.IntOptionHandler",
    "allDeclaredConstructors": true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name": "org.kohsuke.args4j.spi.LongOptionHandler",
    "allDeclaredConstructors": true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name": "org.kohsuke.args4j.spi.MapOptionHandler",
    "allDeclaredConstructors": true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name": "org.kohsuke.args4j.spi.OneArgumentOptionHandler",
    "allDeclaredConstructors": true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name": "org.kohsuke.args4j.spi.OptionHandler",
    "allDeclaredConstructors": true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name": "org.kohsuke.args4j.spi.PathOptionHandler",
    "allDeclaredConstructors": true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name": "org.kohsuke.args4j.spi.PatternOptionHandler",
    "allDeclaredConstructors": true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name": "org.kohsuke.args4j.spi.ShortOptionHandler",
    "allDeclaredConstructors": true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name": "org.kohsuke.args4j.spi.StringOptionHandler",
    "allDeclaredConstructors": true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name": "org.kohsuke.args4j.spi.URIOptionHandler",
    "allDeclaredConstructors": true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name": "org.kohsuke.args4j.spi.URLOptionHandler",
    "allDeclaredConstructors": true,
    "allDeclaredMethods": true,
    "allDeclaredFields": true
  },
  {
    "name" : "com.google.javascript.jscomp.FlagBasedOptions",
    "allDeclaredMethods" : true,
    "allDeclaredFields" : true,
    "allPublicMethods" : true,
    "allPublicFields" : true
  },
  {
    "name" : "com.google.javascript.jscomp.JSCompilerRunner",
    "allDeclaredMethods" : true,
    "allDeclaredFields" : true,
    "allPublicMethods" : true,
    "allPublicFields" : true
  },
  {
    "name" : "com.google.javascript.jscomp.ConformanceConfig",
    "allDeclaredMethods" : true,
    "allDeclaredFields" : true,
    "allPublicMethods" : true,
    "allPublicFields" : true
  },
  {
    "name" : "com.google.javascript.jscomp.ConformanceConfig$Builder",
    "allDeclaredMethods" : true,
    "allDeclaredFields" : true,
    "allPublicMethods" : true,
    "allPublicFields" : true
  },
  {
    "name" : "com.google.javascript.jscomp.Requirement",
    "allDeclaredMethods" : true,
    "allDeclaredFields" : true,
    "allPublicMethods" : true,
    "allPublicFields" : true
  },
  {
    "name" : "com.google.javascript.jscomp.Requirement$Builder",
    "allDeclaredMethods" : true,
    "allDeclaredFields" : true,
    "allPublicMethods" : true,
    "allPublicFields" : true
  },
  {
    "name" : "com.google.javascript.jscomp.Requirement$Type",
    "allDeclaredMethods" : true,
    "allDeclaredFields" : true,
    "allPublicMethods" : true,
    "allPublicFields" : true
  },
  {
    "name" : "com.google.javascript.jscomp.Requirement$TypeMatchingStrategy",
    "allDeclaredMethods" : true,
    "allDeclaredFields" : true,
    "allPublicMethods" : true,
    "allPublicFields" : true
  },
  {
    "name" : "com.google.javascript.jscomp.ConformanceRules",
    "allDeclaredMethods" : true,
    "allDeclaredFields" : true,
    "allPublicMethods" : true,
    "allPublicFields" : true
  },
  {
    "name" : "com.google.javascript.jscomp.ConformanceRules$BanCreateDom",
    "methods" : [ { "name" : "<init>" } ],
    "allDeclaredMethods" : true,
    "allDeclaredFields" : true,
    "allPublicMethods" : true,
    "allPublicFields" : true
  },
  {
    "name" : "com.google.javascript.jscomp.ConformanceRules$BanCreateElement",
    "methods" : [ { "name" : "<init>" } ],
    "allDeclaredMethods" : true,
    "allDeclaredFields" : true,
    "allPublicMethods" : true,
    "allPublicFields" : true
  },
  {
    "name" : "com.google.javascript.jscomp.ConformanceRules$BanExpose",
    "methods" : [ { "name" : "<init>" } ],
    "allDeclaredMethods" : true,
    "allDeclaredFields" : true,
    "allPublicMethods" : true,
    "allPublicFields" : true
  },
  {
    "name" : "com.google.javascript.jscomp.ConformanceRules$BanGlobalVars",
    "methods" : [ { "name" : "<init>" } ],
    "allDeclaredMethods" : true,
    "allDeclaredFields" : true,
    "allPublicMethods" : true,
    "allPublicFields" : true
  },
  {
    "name" : "com.google.javascript.jscomp.ConformanceRules$BanThrowOfNonErrorTypes",
    "methods" : [ { "name" : "<init>" } ],
    "allDeclaredMethods" : true,
    "allDeclaredFields" : true,
    "allPublicMethods" : true,
    "allPublicFields" : true
  },
  {
    "name" : "com.google.javascript.jscomp.ConformanceRules$BanUnknownThis",
    "methods" : [ { "name" : "<init>" } ],
    "allDeclaredMethods" : true,
    "allDeclaredFields" : true,
    "allPublicMethods" : true,
    "allPublicFields" : true
  },
  {
    "name" : "com.google.javascript.jscomp.Requirement$Severity",
    "allDeclaredMethods" : true,
    "allDeclaredFields" : true,
    "allPublicMethods" : true,
    "allPublicFields" : true
  },
  {
    "name" : "com.sun.org.apache.xerces.internal.jaxp.SAXParserFactoryImpl",
    "methods" : [ { "name" : "<init>" } ],
    "allDeclaredMethods" : true,
    "allDeclaredFields" : true,
    "allPublicMethods" : true,
    "allPublicFields" : true
  }
]

```

