# Essence of Prompurr

## Esbuild

We can configure Svelte preprocess to use esbuild (written in Go) to compile typescript files instead of tsc. This is a fairly drop-in replacement in svelte that just requires

1.  ```bash
    # Basically just installing esbuild as a dependency
    pnpm add -D esbuild
    ```

2.  ````javascript
    // Then just adding this, which is bascially taking a typescript string and calling the esbuild API to convert it and return a js string
    const createPreprocessors = () =>
      sveltePreprocess({
        typescript({ content }) {
          const { code, map } = transformSync(content, {
            loader: 'ts',
          });
          return { code, map };
        },
      });
        ```
    ````

## PostCSS

The autoprefixer in PostCSS is so good, it should be illegal not to use it. Adding it is easy as

1.  ```bash
    # Basically just installing postcss and autoprefixer as dependencies
    pnpm add -D postcss autoprefixer
    ```

2.  ```javascript
    // Then just add this to preprocess
    postcss: {
      // map: production ? ctx.map : false,
      // Not needed unless we're adding more plugins I think
      // syntax: require('postcss-scss'),
      // parser: require('postcss-scss'),
      plugins: [require('autoprefixer')],
    },
    ```

## Better defaults

I'm tired of writing `lang=ts` and `lang=scss` since I'll be using it for every singlee file anyway. Better to just add these as defaults to the `rollup.config.js` file once!

```javascript
defaults: {
  script: 'ts',
  style: 'scss',
},
```

# Conclusion

Was it worth the 4 hours to set this up instead of just actually writing a website? No. Of course not. I might save like 3ms per week from esbuild, 2-3 hours of pure frustration from CSS cross-browser compatability, and 3s per week from better defaults. On the other hand, I learned of a lot more cool open sourced projects and made a first "actual" PR. 