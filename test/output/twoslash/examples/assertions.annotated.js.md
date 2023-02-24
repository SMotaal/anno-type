# examples/assertions.js

## Source Code

```js
// @errors: 7027 2775 2502
// @strict: true
// @allowUnreachableCode: false
// @noImplicitAny: true

/**
 * @example
 * 
 * For assertion functions to work, they need to have explicit type annotations on every declaration.
 * 
 * Their expected behaviour is that code following a falsy assertion would be unreachable.
 * 
 * @param {unknown} value
 * @returns { asserts value }
 */
export const assert1 = (value) => { if (!value) throw new Error('Expected true'); };

() => { assert1(false); assert1; };
//      ^?
// @errs:

() => { assert1(false); assert1; };
//                      ^?
// @errs:

() => { assert1(true); assert1; };
//                     ^?
// @errs:

/**
 * @example
 * 
 * The following works because of the type annotation on the declaration.
 * 
 * @type { typeof assert1 }
 */
export const assert2 = assert1;

() => { assert2(false); assert2; };
//      ^?
// @errs:

() => { assert2(false); assert2; };
//                      ^?
// @errs:

() => { assert2(true); assert2; };
//                     ^?
// @errs:

/**
 * @example
 * 
 * This does not work since there is no type annotation on the declaration.
 * 
 * @no-type-annotation
 */
export const assert3 = assert1;

() => { assert3(false); assert3; };
//      ^?
// @errs:

() => { assert3(false); assert3; };
//                      ^?
// @errs:

() => { assert3(true); assert3; };
//                     ^?
// @errs:

/**
 * @example
 * 
 * The following works because of the type annotation on the argument.
 * 
 * @param {(value) => asserts value} assert
 */
(
    /**/ assert
    //   ^?
    // @errs:
) => {

    /**/ assert(false);
    //   ^?
    // @errs:

    /**/ assert;
    //   ^?
    // @errs:

};

/**
 * @example
 * 
 * The following does not work because of a circularity problem.
 * 
 * @param {typeof assert1} assert
 */
(

    /**/ assert
    //   ^?
    // @errs:

) => {

    /**/ assert(false);
    //   ^?
    // @errs:

    /**/ assert;
    //   ^?
    // @errs:

};

/**
 * @example
 * 
 * The following does not work as well because of a circularity problem.
 * 
 * @param {typeof assert2} callAssertAnything
 */
(

    /**/ callAssertAnything
    //   ^?
    // @errs:

) => {

    /**/ callAssertAnything(false);
    //   ^?
    // @errs:

    /**/ callAssertAnything;
    //   ^?
    // @errs:

};

// @source: test/sources/examples/assertions.js

```

## Annotated Code

```js
// @errors: 7027 2775 2502
// @strict: true
// @allowUnreachableCode: false
// @noImplicitAny: true

/**
 * @example
 * 
 * For assertion functions to work, they need to have explicit type annotations on every declaration.
 * 
 * Their expected behaviour is that code following a falsy assertion would be unreachable.
 * 
 * @param {unknown} value
 * @returns { asserts value }
 */
export const assert1 = (value) => { if (!value) throw new Error('Expected true'); };

() => { assert1(false); assert1; };
//      ^?
// @errs: 
//

() => { assert1(false); assert1; };
//                      ^?
// @errs: [Error: 7027]: Unreachable code detected.
//

() => { assert1(true); assert1; };
//                     ^?
// @errs: 
//

/**
 * @example
 * 
 * The following works because of the type annotation on the declaration.
 * 
 * @type { typeof assert1 }
 */
export const assert2 = assert1;

() => { assert2(false); assert2; };
//      ^?
// @errs: 
//

() => { assert2(false); assert2; };
//                      ^?
// @errs: [Error: 7027]: Unreachable code detected.
//

() => { assert2(true); assert2; };
//                     ^?
// @errs: 
//

/**
 * @example
 * 
 * This does not work since there is no type annotation on the declaration.
 * 
 * @no-type-annotation
 */
export const assert3 = assert1;

() => { assert3(false); assert3; };
//      ^?
// @errs: [Error: 2775]: Assertions require every name in the call target to be declared with an explicit type annotation.
//

() => { assert3(false); assert3; };
//                      ^?
// @errs: 
//

() => { assert3(true); assert3; };
//                     ^?
// @errs: 
//

/**
 * @example
 * 
 * The following works because of the type annotation on the argument.
 * 
 * @param {(value) => asserts value} assert
 */
(
    /**/ assert
    //   ^?
    // @errs: 
    //
) => {

    /**/ assert(false);
    //   ^?
    // @errs: 
    //

    /**/ assert;
    //   ^?
    // @errs: [Error: 7027]: Unreachable code detected.
    //

};

/**
 * @example
 * 
 * The following does not work because of a circularity problem.
 * 
 * @param {typeof assert1} assert
 */
(

    /**/ assert
    //   ^?
    // @errs: [Error: 2502]: 'assert' is referenced directly or indirectly in its own type annotation.
    //

) => {

    /**/ assert(false);
    //   ^?
    // @errs: 
    //

    /**/ assert;
    //   ^?
    // @errs: 
    //

};

/**
 * @example
 * 
 * The following does not work as well because of a circularity problem.
 * 
 * @param {typeof assert2} callAssertAnything
 */
(

    /**/ callAssertAnything
    //   ^?
    // @errs: [Error: 2502]: 'callAssertAnything' is referenced directly or indirectly in its own type annotation.
    //

) => {

    /**/ callAssertAnything(false);
    //   ^?
    // @errs: 
    //

    /**/ callAssertAnything;
    //   ^?
    // @errs: 
    //

};

// @source: test/sources/examples/assertions.js

```

## Annotation Results

``` json
{
  "twoslash": {
    "code": "\n/**\n * @example\n * \n * For assertion functions to work, they need to have explicit type annotations on every declaration.\n * \n * Their expected behaviour is that code following a falsy assertion would be unreachable.\n * \n * @param {unknown} value\n * @returns { asserts value }\n */\nexport const assert1 = (value) => { if (!value) throw new Error('Expected true'); };\n\n() => { assert1(false); assert1; };\n// @errs:\n\n() => { assert1(false); assert1; };\n// @errs:\n\n() => { assert1(true); assert1; };\n// @errs:\n\n/**\n * @example\n * \n * The following works because of the type annotation on the declaration.\n * \n * @type { typeof assert1 }\n */\nexport const assert2 = assert1;\n\n() => { assert2(false); assert2; };\n// @errs:\n\n() => { assert2(false); assert2; };\n// @errs:\n\n() => { assert2(true); assert2; };\n// @errs:\n\n/**\n * @example\n * \n * This does not work since there is no type annotation on the declaration.\n * \n * @no-type-annotation\n */\nexport const assert3 = assert1;\n\n() => { assert3(false); assert3; };\n// @errs:\n\n() => { assert3(false); assert3; };\n// @errs:\n\n() => { assert3(true); assert3; };\n// @errs:\n\n/**\n * @example\n * \n * The following works because of the type annotation on the argument.\n * \n * @param {(value) => asserts value} assert\n */\n(\n    /**/ assert\n    // @errs:\n) => {\n\n    /**/ assert(false);\n    // @errs:\n\n    /**/ assert;\n    // @errs:\n\n};\n\n/**\n * @example\n * \n * The following does not work because of a circularity problem.\n * \n * @param {typeof assert1} assert\n */\n(\n\n    /**/ assert\n    // @errs:\n\n) => {\n\n    /**/ assert(false);\n    // @errs:\n\n    /**/ assert;\n    // @errs:\n\n};\n\n/**\n * @example\n * \n * The following does not work as well because of a circularity problem.\n * \n * @param {typeof assert2} callAssertAnything\n */\n(\n\n    /**/ callAssertAnything\n    // @errs:\n\n) => {\n\n    /**/ callAssertAnything(false);\n    // @errs:\n\n    /**/ callAssertAnything;\n    // @errs:\n\n};\n\n",
    "extension": "js",
    "highlights": [],
    "queries": [
      { "docs": "", "kind": "query", "start": 376, "length": 48, "text": "const assert1: (value: unknown) => asserts value", "offset": 8, "line": 14 },
      { "docs": "", "kind": "query", "start": 439, "length": 48, "text": "const assert1: (value: unknown) => asserts value", "offset": 24, "line": 17 },
      { "docs": "", "kind": "query", "start": 485, "length": 48, "text": "const assert1: (value: unknown) => asserts value", "offset": 23, "line": 20 },
      { "docs": "", "kind": "query", "start": 679, "length": 48, "text": "const assert2: (value: unknown) => asserts value", "offset": 8, "line": 32 },
      { "docs": "", "kind": "query", "start": 742, "length": 48, "text": "const assert2: (value: unknown) => asserts value", "offset": 24, "line": 35 },
      { "docs": "", "kind": "query", "start": 788, "length": 48, "text": "const assert2: (value: unknown) => asserts value", "offset": 23, "line": 38 },
      { "docs": "", "kind": "query", "start": 979, "length": 48, "text": "const assert3: (value: unknown) => asserts value", "offset": 8, "line": 50 },
      { "docs": "", "kind": "query", "start": 1042, "length": 48, "text": "const assert3: (value: unknown) => asserts value", "offset": 24, "line": 53 },
      { "docs": "", "kind": "query", "start": 1088, "length": 48, "text": "const assert3: (value: unknown) => asserts value", "offset": 23, "line": 56 },
      { "docs": "", "kind": "query", "start": 1265, "length": 49, "text": "(parameter) assert: (value: any) => asserts value", "offset": 9, "line": 67 },
      { "docs": "", "kind": "query", "start": 1303, "length": 49, "text": "(parameter) assert: (value: any) => asserts value", "offset": 9, "line": 71 },
      { "docs": "", "kind": "query", "start": 1342, "length": 49, "text": "(parameter) assert: (value: any) => asserts value", "offset": 9, "line": 74 },
      { "docs": "", "kind": "query", "start": 1508, "length": 23, "text": "(parameter) assert: any", "offset": 9, "line": 88 },
      { "docs": "", "kind": "query", "start": 1547, "length": 23, "text": "(parameter) assert: any", "offset": 9, "line": 93 },
      { "docs": "", "kind": "query", "start": 1586, "length": 23, "text": "(parameter) assert: any", "offset": 9, "line": 96 },
      { "docs": "", "kind": "query", "start": 1772, "length": 35, "text": "(parameter) callAssertAnything: any", "offset": 9, "line": 110 },
      { "docs": "", "kind": "query", "start": 1823, "length": 35, "text": "(parameter) callAssertAnything: any", "offset": 9, "line": 115 },
      { "docs": "", "kind": "query", "start": 1874, "length": 35, "text": "(parameter) callAssertAnything: any", "offset": 9, "line": 118 }
    ],
    "staticQuickInfos": [
      { "text": "const assert1: (value: unknown) => asserts value", "docs": "", "start": 295, "length": 7, "line": 11, "character": 13, "targetString": "assert1" },
      { "text": "(parameter) value: unknown", "docs": "", "start": 306, "length": 5, "line": 11, "character": 24, "targetString": "value" },
      { "text": "(parameter) value: unknown", "docs": "", "start": 323, "length": 5, "line": 11, "character": 41, "targetString": "value" },
      { "text": "var Error: ErrorConstructor\nnew (message?: string | undefined) => Error", "docs": "", "start": 340, "length": 5, "line": 11, "character": 58, "targetString": "Error" },
      { "text": "const assert1: (value: unknown) => asserts value", "docs": "", "start": 376, "length": 7, "line": 13, "character": 8, "targetString": "assert1" },
      { "text": "const assert1: (value: unknown) => asserts value", "docs": "", "start": 392, "length": 7, "line": 13, "character": 24, "targetString": "assert1" },
      { "text": "const assert1: (value: unknown) => asserts value", "docs": "", "start": 423, "length": 7, "line": 16, "character": 8, "targetString": "assert1" },
      { "text": "const assert1: (value: unknown) => asserts value", "docs": "", "start": 439, "length": 7, "line": 16, "character": 24, "targetString": "assert1" },
      { "text": "const assert1: (value: unknown) => asserts value", "docs": "", "start": 470, "length": 7, "line": 19, "character": 8, "targetString": "assert1" },
      { "text": "const assert1: (value: unknown) => asserts value", "docs": "", "start": 485, "length": 7, "line": 19, "character": 23, "targetString": "assert1" },
      { "text": "const assert2: (value: unknown) => asserts value", "docs": "", "start": 651, "length": 7, "line": 29, "character": 13, "targetString": "assert2" },
      { "text": "const assert1: (value: unknown) => asserts value", "docs": "", "start": 661, "length": 7, "line": 29, "character": 23, "targetString": "assert1" },
      { "text": "const assert2: (value: unknown) => asserts value", "docs": "", "start": 679, "length": 7, "line": 31, "character": 8, "targetString": "assert2" },
      { "text": "const assert2: (value: unknown) => asserts value", "docs": "", "start": 695, "length": 7, "line": 31, "character": 24, "targetString": "assert2" },
      { "text": "const assert2: (value: unknown) => asserts value", "docs": "", "start": 726, "length": 7, "line": 34, "character": 8, "targetString": "assert2" },
      { "text": "const assert2: (value: unknown) => asserts value", "docs": "", "start": 742, "length": 7, "line": 34, "character": 24, "targetString": "assert2" },
      { "text": "const assert2: (value: unknown) => asserts value", "docs": "", "start": 773, "length": 7, "line": 37, "character": 8, "targetString": "assert2" },
      { "text": "const assert2: (value: unknown) => asserts value", "docs": "", "start": 788, "length": 7, "line": 37, "character": 23, "targetString": "assert2" },
      { "text": "const assert3: (value: unknown) => asserts value", "docs": "", "start": 951, "length": 7, "line": 47, "character": 13, "targetString": "assert3" },
      { "text": "const assert1: (value: unknown) => asserts value", "docs": "", "start": 961, "length": 7, "line": 47, "character": 23, "targetString": "assert1" },
      { "text": "const assert3: (value: unknown) => asserts value", "docs": "", "start": 979, "length": 7, "line": 49, "character": 8, "targetString": "assert3" },
      { "text": "const assert3: (value: unknown) => asserts value", "docs": "", "start": 995, "length": 7, "line": 49, "character": 24, "targetString": "assert3" },
      { "text": "const assert3: (value: unknown) => asserts value", "docs": "", "start": 1026, "length": 7, "line": 52, "character": 8, "targetString": "assert3" },
      { "text": "const assert3: (value: unknown) => asserts value", "docs": "", "start": 1042, "length": 7, "line": 52, "character": 24, "targetString": "assert3" },
      { "text": "const assert3: (value: unknown) => asserts value", "docs": "", "start": 1073, "length": 7, "line": 55, "character": 8, "targetString": "assert3" },
      { "text": "const assert3: (value: unknown) => asserts value", "docs": "", "start": 1088, "length": 7, "line": 55, "character": 23, "targetString": "assert3" },
      { "text": "(parameter) assert: (value: any) => asserts value", "docs": "", "start": 1265, "length": 6, "line": 66, "character": 9, "targetString": "assert" },
      { "text": "(parameter) assert: (value: any) => asserts value", "docs": "", "start": 1303, "length": 6, "line": 70, "character": 9, "targetString": "assert" },
      { "text": "(parameter) assert: (value: any) => asserts value", "docs": "", "start": 1342, "length": 6, "line": 73, "character": 9, "targetString": "assert" },
      { "text": "(parameter) assert: any", "docs": "", "start": 1508, "length": 6, "line": 87, "character": 9, "targetString": "assert" },
      { "text": "(parameter) assert: any", "docs": "", "start": 1547, "length": 6, "line": 92, "character": 9, "targetString": "assert" },
      { "text": "(parameter) assert: any", "docs": "", "start": 1586, "length": 6, "line": 95, "character": 9, "targetString": "assert" },
      { "text": "(parameter) callAssertAnything: any", "docs": "", "start": 1772, "length": 18, "line": 109, "character": 9, "targetString": "callAssertAnything" },
      { "text": "(parameter) callAssertAnything: any", "docs": "", "start": 1823, "length": 18, "line": 114, "character": 9, "targetString": "callAssertAnything" },
      { "text": "(parameter) callAssertAnything: any", "docs": "", "start": 1874, "length": 18, "line": 117, "character": 9, "targetString": "callAssertAnything" }
    ],
    "errors": [
      { "category": 1, "code": 7027, "length": 8, "start": 392, "line": 13, "character": 24, "renderedMessage": "Unreachable code detected.", "id": "err-7027-392-8" },
      { "category": 1, "code": 7027, "length": 8, "start": 439, "line": 16, "character": 24, "renderedMessage": "Unreachable code detected.", "id": "err-7027-439-8" },
      { "category": 1, "code": 7027, "length": 8, "start": 695, "line": 31, "character": 24, "renderedMessage": "Unreachable code detected.", "id": "err-7027-695-8" },
      { "category": 1, "code": 7027, "length": 8, "start": 742, "line": 34, "character": 24, "renderedMessage": "Unreachable code detected.", "id": "err-7027-742-8" },
      { "category": 1, "code": 2775, "length": 7, "start": 979, "line": 49, "character": 8, "renderedMessage": "Assertions require every name in the call target to be declared with an explicit type annotation.", "id": "err-2775-979-7" },
      { "category": 1, "code": 2775, "length": 7, "start": 1026, "line": 52, "character": 8, "renderedMessage": "Assertions require every name in the call target to be declared with an explicit type annotation.", "id": "err-2775-1026-7" },
      { "category": 1, "code": 2775, "length": 7, "start": 1073, "line": 55, "character": 8, "renderedMessage": "Assertions require every name in the call target to be declared with an explicit type annotation.", "id": "err-2775-1073-7" },
      { "category": 1, "code": 7006, "length": 5, "start": 1218, "line": 63, "character": 12, "renderedMessage": "Parameter 'value' implicitly has an 'any' type.", "id": "err-7006-1218-5" },
      { "category": 1, "code": 7027, "length": 7, "start": 1342, "line": 73, "character": 9, "renderedMessage": "Unreachable code detected.", "id": "err-7027-1342-7" },
      { "category": 1, "code": 2502, "length": 6, "start": 1508, "line": 87, "character": 9, "renderedMessage": "'assert' is referenced directly or indirectly in its own type annotation.", "id": "err-2502-1508-6" },
      { "category": 1, "code": 2502, "length": 18, "start": 1772, "line": 109, "character": 9, "renderedMessage": "'callAssertAnything' is referenced directly or indirectly in its own type annotation.", "id": "err-2502-1772-18" }
    ],
    "playgroundURL": "https://www.typescriptlang.org/play/#code/PTAEAEFMCdoe2gZwFygOwAYBMbQ7QKx4HYBQIEiALtAJYDGVqNArpOWOAIYA2PcAdwCqAO2iQu9ABZcARj0gBhOABNIqAGa9E7CuBFwAkgFsADjwa0qAQREBPZtDalyAKlelQriJAAeXMwVPb2DQADEEUC5EHWgqWjgRUA0WEUYEkURQKjhQAQQAawAabKlIO1ARSEgVbNyZADdIUD9zSypsu1NmrhEDKi54xKzElqboCrV6Hi5oQYyAOlDQgBUy2mgW327GGtBZSEaElk3aLKoZDvpVZo04PkFaEQBzKOTtCujYoaT8lh5agdQKlxJIZPJIEsvKBQuBTLMAqAAN6pAoGAQiAC+oAavGc0PA4ioJ0yyKiMRgVCyuJ4bFAmOCwFIrQQV2GHS+lIAjKAALygAAUNLYAEo+QA+Mm0DSCgCEwsgYou8AElUgqoAorAEAKAOQa7aQXa1ViQXUigDc9ItLgFYt5kqR5O+XIFWh4Oktzu5VsxNoooEDgYAegB+Dg+WAoW32x3euKu92eq2chO+-1gINZ7M5oNhiNQKPIGMSsmpqiu01e8tc9MR3MN7P5vQwJDFtweAl+ALmdjQ1ZlZL3fgCJ6vfLQApZA70LgsHSgOAyi7NKhdHp9OADH6LpIr0BTGZzH5QkIEtfdMkXyBL+MV+mM5nbVmga6ZDkUuJYPl32slh1lp+VBYG62iKimQFYHWAZZs2nCttGpB2qWTrliBSbgXeUHWvWjZ4aAcGRm2-5xmhApVhB3zYX6uH4bmhGFsRHawt2gR9me3hrGcB5wJAWT9HkhSgIgTz0KuZTiKA3EGJ0l69P08yjKM+6HgiJ7LASBgALTXlp8lbopIiPiycSvuyd4AMw-jWNpIbGgHfBZoEeph5YWdBmaweGLZFiRDmUk5GHVkB7k4TBdH0d58G+XZKGWeRTiuSFHkRQ2DEIe2wDuCx-hsRpnGDncDyji8gmTtORpzgut77teUSbtuGS7qUPTQM8LDGJAIhUKeMIEvCczGMiQp4oqpbltSo3YuWj4Cp4QZZa4YAzVmMH5qt0XEfZSIuKt7jLUBznJvNgZreGG1EYhe1LXeNoXSG50LZtiE0cxXa5b2+WgGstzDo8pUqLx-FbmVBT7JV87NLeXCvhs9D-LMVgVKY8AQsYvWwgNiJIte0NAVy01AbNu0Lftd4naAZ0Uz5THbSTp1k+WR3gdTnnrU9l3ttdB3fHdHMPaznMuK9i05T2QT9tCP1DsVY48XxlQgxOYPRHkkB8ODs6Q4uMow-QcMI3Qa6gCjcBoxj-UIkNOPrnjVHYrOfDWEBth2BcY7E9TZOOzwzvfK77svILAsXYxiF017N0+37lIB1IY7M5awcEY9p3PVzpNR7wvsu-YgfPHzad5qnlPp8Ltl6IgcAnGJzB8VQwBVzXfHAKxvaIMA5YZIgCwAFaIKQQA",
    "tags": [{ "name": "source", "line": 144, "annotation": "test/sources/examples/assertions.js" }]
  },
  "annotated": {
    "code": "// @errors: 7027 2775 2502\n// @strict: true\n// @allowUnreachableCode: false\n// @noImplicitAny: true\n\n/**\n * @example\n * \n * For assertion functions to work, they need to have explicit type annotations on every declaration.\n * \n * Their expected behaviour is that code following a falsy assertion would be unreachable.\n * \n * @param {unknown} value\n * @returns { asserts value }\n */\nexport const assert1 = (value) => { if (!value) throw new Error('Expected true'); };\n\n() => { assert1(false); assert1; };\n//      ^?\n// @errs: \n//\n\n() => { assert1(false); assert1; };\n//                      ^?\n// @errs: [Error: 7027]: Unreachable code detected.\n//\n\n() => { assert1(true); assert1; };\n//                     ^?\n// @errs: \n//\n\n/**\n * @example\n * \n * The following works because of the type annotation on the declaration.\n * \n * @type { typeof assert1 }\n */\nexport const assert2 = assert1;\n\n() => { assert2(false); assert2; };\n//      ^?\n// @errs: \n//\n\n() => { assert2(false); assert2; };\n//                      ^?\n// @errs: [Error: 7027]: Unreachable code detected.\n//\n\n() => { assert2(true); assert2; };\n//                     ^?\n// @errs: \n//\n\n/**\n * @example\n * \n * This does not work since there is no type annotation on the declaration.\n * \n * @no-type-annotation\n */\nexport const assert3 = assert1;\n\n() => { assert3(false); assert3; };\n//      ^?\n// @errs: [Error: 2775]: Assertions require every name in the call target to be declared with an explicit type annotation.\n//\n\n() => { assert3(false); assert3; };\n//                      ^?\n// @errs: \n//\n\n() => { assert3(true); assert3; };\n//                     ^?\n// @errs: \n//\n\n/**\n * @example\n * \n * The following works because of the type annotation on the argument.\n * \n * @param {(value) => asserts value} assert\n */\n(\n    /**/ assert\n    //   ^?\n    // @errs: \n    //\n) => {\n\n    /**/ assert(false);\n    //   ^?\n    // @errs: \n    //\n\n    /**/ assert;\n    //   ^?\n    // @errs: [Error: 7027]: Unreachable code detected.\n    //\n\n};\n\n/**\n * @example\n * \n * The following does not work because of a circularity problem.\n * \n * @param {typeof assert1} assert\n */\n(\n\n    /**/ assert\n    //   ^?\n    // @errs: [Error: 2502]: 'assert' is referenced directly or indirectly in its own type annotation.\n    //\n\n) => {\n\n    /**/ assert(false);\n    //   ^?\n    // @errs: \n    //\n\n    /**/ assert;\n    //   ^?\n    // @errs: \n    //\n\n};\n\n/**\n * @example\n * \n * The following does not work as well because of a circularity problem.\n * \n * @param {typeof assert2} callAssertAnything\n */\n(\n\n    /**/ callAssertAnything\n    //   ^?\n    // @errs: [Error: 2502]: 'callAssertAnything' is referenced directly or indirectly in its own type annotation.\n    //\n\n) => {\n\n    /**/ callAssertAnything(false);\n    //   ^?\n    // @errs: \n    //\n\n    /**/ callAssertAnything;\n    //   ^?\n    // @errs: \n    //\n\n};\n\n// @source: test/sources/examples/assertions.js\n"
  }
}
```