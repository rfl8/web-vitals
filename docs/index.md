# Fail-fast approach to improve web-vitals

## FCP

FCP measures how long it takes the browser to render the first piece of DOM content after a user navigates to your page. Images, non-white `canvas` elements, and SVGs on your page are considered DOM content; anything inside an iframe _isn’t_ included.

### Theory

- Lazy-load third-party resources [Efficiently load third-party JavaScript](https://web.dev/efficiently-load-third-party-javascript/#lazy-load-third-party-resources)
- Establish early connections to required origins using `preconnect` and `dns-prefetch`.
  [Efficiently load third-party JavaScript](https://web.dev/efficiently-load-third-party-javascript/#lazy-load-third-party-resources)
- Use a “windowing” library like [react-window](https://web.dev/virtualize-long-lists-react-window/) to minimize the number of DOM nodes created if you are rendering many repeated elements on the page.
- Minimize unnecessary re-renders using [shouldComponentUpdate](https://reactjs.org/docs/optimizing-performance.html#shouldcomponentupdate-in-action) , [PureComponent](https://reactjs.org/docs/react-api.html#reactpurecomponent) , or [React.memo](https://reactjs.org/docs/react-api.html#reactmemo) .
- [Skip effects](https://reactjs.org/docs/hooks-effect.html#tip-optimizing-performance-by-skipping-effects) only until certain dependencies have changed if you are using the Effect hook to improve runtime performance.
- [Avoid an excessive DOM size](https://web.dev/dom-size/#react)

### Fail-fast approach

1. Use a `windowing` library specially when rendering tons of cards above the fold.

2. Use infinite scrolling alongside a windowing technique.

3. Font-awesome and third-party resources should be self-hosted when possible to avoid DNS lookup and round-trip times if you cannot use preconnect/preload.

4. Use service-workers to cache scripts from third-party servers.
   [Handle Third Party Requests](https://developers.google.com/web/tools/workbox/guides/handle-third-party-requests)

5. Use image on load hook, https://usehooks-typescript.com/react-hook/use-image-on-load. Practical use-case, https://react-gallery-ux.netlify.app/

6. Minimize unnecessary re-renders using [shouldComponentUpdate](https://reactjs.org/docs/optimizing-performance.html#shouldcomponentupdate-in-action) , [PureComponent](https://reactjs.org/docs/react-api.html#reactpurecomponent) , or [React.memo](https://reactjs.org/docs/react-api.html#reactmemo) .

7. Do not use `useEffect` without properly setting the array of dependencies. [Skip effects](https://reactjs.org/docs/hooks-effect.html#tip-optimizing-performance-by-skipping-effects)

## LCP

Largest Contentful Paint marks the time at which the largest text or image is painted.

### Fail-fast approach

1. Use next/image component with a custom loader to call our lambda.

- [next/image](https://nextjs.org/docs/api-reference/next/image#loader)

- [lambda](https://github.com/thebyte9/image-cdn-utils/blob/master/packages/image-cdn-utils/README.md)

2. Create a new admin attribute to set `priority` attribute to images above the fold to be preloaded. [next/image](https://nextjs.org/docs/api-reference/next/image#priority)

3. Use intersection-observer to lazy load images above the fold.

## CLS (ad-related layout shift)

Measures layout shifts that were caused by ads or happened near ads. Reducing cumulative ad-related layout shift will improve user experience. [Learn more](https://developers.google.com/publisher-ads-audits/reference/audits/cumulative-ad-shift?utm_source=lighthouse&utm_medium=devtools) .

### Theory

[Optimize Cumulative Layout Shift](https://web.dev/optimize-cls/)

Note: exactly above is what we should improve in Dockwalk and BoatInternational.

### Fail-fast approach

CLS scores higher if space is reserved for Multi-size ad slots.

[Minimize layout shift](https://developers.google.com/publisher-tag/guides/minimize-layout-shift#multisize)

## CLS (generic)

Measures ad-related content layout shifts

### Fail-fast approach

1. Build skeleton components and render skeleton screens to avoid any layout shifts and comply with business constraints such as multiple add sizes. It could be accomplished by just using CSS, e.g [Animation - Tailwind CSS](https://tailwindcss.com/docs/animation#pulse)

## Speed Index

Speed Index measures how quickly content is visually displayed during page load.

### Theory

- [Minimize main thread work](https://web.dev/mainthread-work-breakdown)
- [Reduce JavaScript execution time](https://web.dev/bootup-time)
- [Ensure text remains visible during webfont load](https://web.dev/font-display)

### Fail-fast approach

1. Defer and eliminate render-blocking resources. [Eliminate render-blocking resources](https://web.dev/render-blocking-resources/)
2. Use web workers to minimise main thread work as execution takes place in different threads.

## Eliminate unused Javascript and CSS on main page

1. [Find Unused JavaScript And CSS With The Coverage Tab - Chrome Developers](https://developer.chrome.com/docs/devtools/coverage/)

## Reference

[Fail-fast principle in software development](https://dzone.com/articles/fail-fast-principle-in-software-development)
