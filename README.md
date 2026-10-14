<!--

@license Apache-2.0

Copyright (c) 2026 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->


<details>
  <summary>
    About stdlib...
  </summary>
  <p>We believe in a future in which the web is a preferred environment for numerical computation. To help realize this future, we've built stdlib. stdlib is a standard library, with an emphasis on numerical and scientific computation, written in JavaScript (and C) for execution in browsers and in Node.js.</p>
  <p>The library is fully decomposable, being architected in such a way that you can swap out and mix and match APIs and functionality to cater to your exact preferences and use cases.</p>
  <p>When you use stdlib, you can be absolutely certain that you are using the most thorough, rigorous, well-written, studied, documented, tested, measured, and high-quality code out there.</p>
  <p>To join us in bringing numerical computing to the web, get started by checking us out on <a href="https://github.com/stdlib-js/stdlib">GitHub</a>, and please consider <a href="https://opencollective.com/stdlib">financially supporting stdlib</a>. We greatly appreciate your continued support!</p>
</details>

# incrnanmda

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] <!-- [![dependencies][dependencies-image]][dependencies-url] -->

> Compute the [mean directional accuracy][mean-directional-accuracy] (MDA) incrementally, ignoring `NaN` values.

<section class="intro">

The [mean directional accuracy][mean-directional-accuracy] is defined as

<!-- <equation class="equation" label="eq:mean_directional_accuracy" align="center" raw="\operatorname{MDA} = \begin{cases} 1 & \textrm{if}\ N = 1 \\\frac{1}{N} \sum_{i=1}^{N} \delta_{\operatorname{sgn}(\Delta f_{i,i-1}),\ \operatorname{sgn}(\Delta a_{i,i-1})} & \textrm{if}\ N > 1 \end{cases}" alt="Equation for the mean directional accuracy."> -->

```math
\mathop{\mathrm{MDA}} = \begin{cases} 1 & \textrm{if}\ N = 1 \\\frac{1}{N} \sum_{i=1}^{N} \delta_{\mathop{\mathrm{sgn}}(\Delta f_{i,i-1}),\ \mathop{\mathrm{sgn}}(\Delta a_{i,i-1})} & \textrm{if}\ N > 1 \end{cases}
```

<!-- <div class="equation" align="center" data-raw-text="\operatorname{MDA} = \begin{cases} 1 & \textrm{if}\ N = 1 \\\frac{1}{N} \sum_{i=1}^{N} \delta_{\operatorname{sgn}(\Delta f_{i,i-1}),\ \operatorname{sgn}(\Delta a_{i,i-1})} & \textrm{if}\ N > 1 \end{cases}" data-equation="eq:mean_directional_accuracy">
    <img src="https://cdn.jsdelivr.net/gh/stdlib-js/stdlib@95b364439921fe28429acff89c5ba464a5a60caf/lib/node_modules/@stdlib/stats/incr/mda/docs/img/equation_mean_directional_accuracy.svg" alt="Equation for the mean directional accuracy.">
    <br>
</div> -->

<!-- </equation> -->

where `f_i` is the forecast value, `a_i` is the actual value, `sgn(x)` is the [signum][@stdlib/math/base/special/signum] function, and `δ` is the [Kronecker delta][@stdlib/math/base/special/kronecker-delta].

</section>

<!-- /.intro -->



<section class="usage">

## Usage

To use in Observable,

```javascript
incrnanmda = require( 'https://cdn.jsdelivr.net/gh/stdlib-js/stats-incr-nanmda@umd/browser.js' )
```

To vendor stdlib functionality and avoid installing dependency trees for Node.js, you can use the UMD server build:

```javascript
var incrnanmda = require( 'path/to/vendor/umd/stats-incr-nanmda/index.js' )
```

To include the bundle in a webpage,

```html
<script type="text/javascript" src="https://cdn.jsdelivr.net/gh/stdlib-js/stats-incr-nanmda@umd/browser.js"></script>
```

If no recognized module system is present, access bundle contents via the global scope:

```html
<script type="text/javascript">
(function () {
    window.incrnanmda;
})();
</script>
```

#### incrnanmda()

Returns an accumulator `function` which incrementally computes the [mean directional accuracy][mean-directional-accuracy], ignoring `NaN` values.

```javascript
var accumulator = incrnanmda();
```

#### accumulator( \[f, a] )

If provided input values `f` and `a`, the accumulator function returns an updated [mean directional accuracy][mean-directional-accuracy]. If not provided input values `f` and `a`, the accumulator function returns the current [mean directional accuracy][mean-directional-accuracy].

```javascript
var accumulator = incrnanmda();

var m = accumulator( 2.0, 3.0 );
// returns 1.0

m = accumulator( -1.0, 4.0 );
// returns 0.5

m = accumulator( NaN, 4.0 );
// returns 0.5

m = accumulator( -3.0, -2.0 );
// returns ~0.67

m = accumulator();
// returns ~0.67
```

</section>

<!-- /.usage -->

<section class="notes">

## Notes

-   Input values are type checked. If provided a NaN or a non-numeric value, the update is skipped and the accumulator returns the previously computed mean directional accuracy. If non-numeric inputs are possible, you are advised to type check and handle accordingly before passing the value to the accumulator function.

</section>

<!-- /.notes -->

<section class="examples">

## Examples

<!-- eslint no-undef: "error" -->

```html
<!DOCTYPE html>
<html lang="en">
<body>
<script type="text/javascript" src="https://cdn.jsdelivr.net/gh/stdlib-js/random-base-randu@umd/browser.js"></script>
<script type="text/javascript" src="https://cdn.jsdelivr.net/gh/stdlib-js/stats-incr-nanmda@umd/browser.js"></script>
<script type="text/javascript">
(function () {

var accumulator;
var v1;
var v2;
var i;

// Initialize an accumulator:
accumulator = incrnanmda();

// For each simulated datum, update the mean directional accuracy...
for ( i = 0; i < 100; i++ ) {
    if ( randu() < 0.2 ) {
        v1 = NaN;
    } else {
        v1 = ( randu()*100.0 ) - 50.0;
    }
    if ( randu() < 0.2 ) {
        v2 = NaN;
    } else {
        v2 = ( randu()*100.0 ) - 50.0;
    }
    accumulator( v1, v2 );
}
console.log( accumulator() );

})();
</script>
</body>
</html>
```

</section>

<!-- /.examples -->

<!-- Section for related `stdlib` packages. Do not manually edit this section, as it is automatically populated. -->

<section class="related">

</section>

<!-- /.related -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library for JavaScript and Node.js, with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2026. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/stats-incr-nanmda.svg
[npm-url]: https://npmjs.org/package/@stdlib/stats-incr-nanmda

[test-image]: https://github.com/stdlib-js/stats-incr-nanmda/actions/workflows/test.yml/badge.svg?branch=main
[test-url]: https://github.com/stdlib-js/stats-incr-nanmda/actions/workflows/test.yml?query=branch:main

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/stats-incr-nanmda/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/stats-incr-nanmda?branch=main

<!--

[dependencies-image]: https://img.shields.io/david/stdlib-js/stats-incr-nanmda.svg
[dependencies-url]: https://david-dm.org/stdlib-js/stats-incr-nanmda/main

-->

[chat-image]: https://img.shields.io/badge/zulip-join_chat-brightgreen.svg
[chat-url]: https://stdlib.zulipchat.com

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[umd]: https://github.com/umdjs/umd
[es-module]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules

[deno-url]: https://github.com/stdlib-js/stats-incr-nanmda/tree/deno
[deno-readme]: https://github.com/stdlib-js/stats-incr-nanmda/blob/deno/README.md
[umd-url]: https://github.com/stdlib-js/stats-incr-nanmda/tree/umd
[umd-readme]: https://github.com/stdlib-js/stats-incr-nanmda/blob/umd/README.md
[esm-url]: https://github.com/stdlib-js/stats-incr-nanmda/tree/esm
[esm-readme]: https://github.com/stdlib-js/stats-incr-nanmda/blob/esm/README.md
[branches-url]: https://github.com/stdlib-js/stats-incr-nanmda/blob/main/branches.md

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/stats-incr-nanmda/main/LICENSE

[mean-directional-accuracy]: https://en.wikipedia.org/wiki/Mean_Directional_Accuracy_%28MDA%29

[@stdlib/math/base/special/signum]: https://github.com/stdlib-js/math-base-special-signum/tree/umd

[@stdlib/math/base/special/kronecker-delta]: https://github.com/stdlib-js/math-base-special-kronecker-delta/tree/umd

</section>

<!-- /.links -->
