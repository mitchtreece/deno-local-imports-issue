# Deno Local Imports Issue

- Original issue: [#26704](https://github.com/denoland/deno/issues/26704)
- New issue [TODO](https://github.com/denoland/deno/issues/26704)

## Overview

Given this project structure where `app` depends on `@pkg/printer`, & `@pkg/printer`
depends on `@pkg/formatter` via local imports & the `patch` field in `deno.json`.
When running `app` we get the error:

`JSR package not found: @pkg/formatter`

and warning:

`patch field can only be specified in the workspace root deno.json file`

Oddly enough, running a test script from `@pkg/printer`, everything works as expected.
It's able to import and use `@pkg/formatter` without any errors _or_ warnings.
It seems that as soon as there's more than a single level of local dependant imports,
Deno gets confused. Here's a reference of the `app` & `@pkg/printer` import configs:

`app`:
```json
{
    ...
    "imports": {
        "@pkg/printer": "jsr:@pkg/printer@1.0.0"
    },
    "patch": [
        "../pkg/printer"
    ]
}
```

`@pkg/printer`:
```json
{
    ...
    "imports": {
        "@pkg/formatter": "jsr:@pkg/formatter@1.0.0"
    },
    "patch": [
        "../formatter"
    ]
}

```
